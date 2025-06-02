TITLE: Creating Hugging Face Embedding Edge Function (TypeScript)
DESCRIPTION: This Deno/TypeScript Edge Function demonstrates how to create a serverless API endpoint that accepts text input, generates embeddings using a Hugging Face model (`Supabase/gte-small` via `@xenova/transformers`), and then stores these embeddings in a Supabase database. It leverages `supabase-js` for database interaction and `deno.land/std` for serving HTTP requests.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/www/_blog/2023-08-07-hugging-face-supabase.mdx#_snippet_3

LANGUAGE: TypeScript
CODE:
```
import { serve } from 'https://deno.land/std@0.168.0/http/server.ts'
import { env, pipeline } from 'https://cdn.jsdelivr.net/npm/@xenova/transformers@2.5.0'
import { createClient } from 'jsr:@supabase/supabase-js@2'

// Preparation for Deno runtime
env.useBrowserCache = false
env.allowLocalModels = false

const supabase = createClient(
  'https://xyzcompany.supabase.co',
  'public-anon-key',
)

// Construct pipeline outside of serve for faster warm starts
const pipe = await pipeline(
  'feature-extraction',
  'Supabase/gte-small',
)

// Deno Handler
serve(async (req) => {
  const { input } = await req.json()

  // Generate the embedding from the user input
  const output = await pipe(input, {
    pooling: 'mean',
    normalize: true,
  })

  // Get the embedding output
  const embedding = Array.from(output.data)

  // Store the embedding
  const { data, error } = await supabase
    .from('collections')
    .insert({ embedding })

  // Return the embedding
  return new Response(
    { new_row: data },
    { headers: { 'Content-Type': 'application/json' } },
  )
})
```

----------------------------------------

TITLE: Correct: Supabase SSR Cookie Handling Pattern (TypeScript)
DESCRIPTION: This snippet illustrates the REQUIRED and correct pattern for handling cookies when initializing Supabase clients for SSR. It mandates the use of `getAll()` and `setAll(cookiesToSet)` methods, ensuring proper session management and preventing application breakage. AI models MUST always generate this exact pattern.
SOURCE: https://github.com/supabase/supabase/blob/master/examples/prompts/nextjs-supabase-auth.md#_snippet_2

LANGUAGE: typescript
CODE:
```
{
  cookies: {
    getAll() {
      return cookieStore.getAll()
    },
    setAll(cookiesToSet) {
      const response = NextResponse.next({
        request,
      })
      
      cookiesToSet.forEach(({ name, value, options }) => {
        response.cookies.set(name, value, options)
      })
      
      return response
    }
  }
}
```

----------------------------------------

TITLE: Supabase Edge Function for Hugging Face Image Captioning
DESCRIPTION: This TypeScript Edge Function, designed for Deno, processes webhook payloads from Supabase Storage object insertions. It retrieves a signed URL for the newly uploaded image, uses the Hugging Face Inference API (`nlpconnect/vit-gpt2-image-captioning` model) to generate a caption, and then stores this caption along with the image ID in the `image_caption` table in Supabase. It requires `HUGGINGFACE_ACCESS_TOKEN`, `SUPABASE_URL`, and `SUPABASE_SERVICE_ROLE_KEY` environment variables.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/ai/examples/huggingface-image-captioning.mdx#_snippet_1

LANGUAGE: typescript
CODE:
```
import { serve } from 'https://deno.land/std@0.168.0/http/server.ts'
import { HfInference } from 'https://esm.sh/@huggingface/inference@2.3.2'
import { createClient } from 'jsr:@supabase/supabase-js@2'
import { Database } from './types.ts'

console.log('Hello from `huggingface-image-captioning` function!')

const hf = new HfInference(Deno.env.get('HUGGINGFACE_ACCESS_TOKEN'))

type SoRecord = Database['storage']['Tables']['objects']['Row']
interface WebhookPayload {
  type: 'INSERT' | 'UPDATE' | 'DELETE'
  table: string
  record: SoRecord
  schema: 'public'
  old_record: null | SoRecord
}

serve(async (req) => {
  const payload: WebhookPayload = await req.json()
  const soRecord = payload.record
  const supabaseAdminClient = createClient<Database>(
    // Supabase API URL - env var exported by default when deployed.
    Deno.env.get('SUPABASE_URL') ?? '',
    // Supabase API SERVICE ROLE KEY - env var exported by default when deployed.
    Deno.env.get('SUPABASE_SERVICE_ROLE_KEY') ?? ''
  )

  // Construct image url from storage
  const { data, error } = await supabaseAdminClient.storage
    .from(soRecord.bucket_id!)
    .createSignedUrl(soRecord.path_tokens!.join('/'), 60)
  if (error) throw error
  const { signedUrl } = data

  // Run image captioning with Huggingface
  const imgDesc = await hf.imageToText({
    data: await (await fetch(signedUrl)).blob(),
    model: 'nlpconnect/vit-gpt2-image-captioning',
  })

  // Store image caption in Database table
  await supabaseAdminClient
    .from('image_caption')
    .insert({ id: soRecord.id!, caption: imgDesc.generated_text })
    .throwOnError()

  return new Response('ok')
})
```

----------------------------------------

TITLE: Initializing Supabase Client - JavaScript
DESCRIPTION: Initializes the Supabase client in JavaScript using the provided project URL and anonymous public API key. This client instance is used for all subsequent Supabase interactions.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/realtime/broadcast.mdx#_snippet_0

LANGUAGE: JavaScript
CODE:
```
import { createClient } from '@supabase/supabase-js'

const SUPABASE_URL = 'https://<project>.supabase.co'
const SUPABASE_KEY = '<your-anon-key>'

const supabase = createClient(SUPABASE_URL, SUPABASE_KEY)
```

----------------------------------------

TITLE: Implementing TOTP MFA Enrollment in React
DESCRIPTION: This React component, `EnrollMFA`, facilitates the TOTP MFA enrollment process. It initiates enrollment by calling `supabase.auth.mfa.enroll()` to retrieve a QR code and factor ID. The QR code is then displayed for the user to scan with their authenticator app. Once the user enters a verification code, the component uses `supabase.auth.mfa.challenge()` and `supabase.auth.mfa.verify()` to validate the code and activate the MFA factor, handling errors and providing callbacks for completion or cancellation.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/auth/auth-mfa/totp.mdx#_snippet_0

LANGUAGE: tsx
CODE:
```
/**
 * EnrollMFA shows a simple enrollment dialog. When shown on screen it calls
 * the `enroll` API. Each time a user clicks the Enable button it calls the
 * `challenge` and `verify` APIs to check if the code provided by the user is
 * valid.
 * When enrollment is successful, it calls `onEnrolled`. When the user clicks
 * Cancel the `onCancelled` callback is called.
 */
export function EnrollMFA({
  onEnrolled,
  onCancelled,
}: {
  onEnrolled: () => void
  onCancelled: () => void
}) {
  const [factorId, setFactorId] = useState('')
  const [qr, setQR] = useState('') // holds the QR code image SVG
  const [verifyCode, setVerifyCode] = useState('') // contains the code entered by the user
  const [error, setError] = useState('') // holds an error message

  const onEnableClicked = () => {
    setError('')
    ;(async () => {
      const challenge = await supabase.auth.mfa.challenge({ factorId })
      if (challenge.error) {
        setError(challenge.error.message)
        throw challenge.error
      }

      const challengeId = challenge.data.id

      const verify = await supabase.auth.mfa.verify({
        factorId,
        challengeId,
        code: verifyCode,
      })
      if (verify.error) {
        setError(verify.error.message)
        throw verify.error
      }

      onEnrolled()
    })()
  }

  useEffect(() => {
    ;(async () => {
      const { data, error } = await supabase.auth.mfa.enroll({
        factorType: 'totp',
      })
      if (error) {
        throw error
      }

      setFactorId(data.id)

      // Supabase Auth returns an SVG QR code which you can convert into a data
      // URL that you can place in an <img> tag.
      setQR(data.totp.qr_code)
    })()
  }, [])

  return (
    <>
      {error && <div className="error">{error}</div>}
      <img src={qr} />
      <input
        type="text"
        value={verifyCode}
        onChange={(e) => setVerifyCode(e.target.value.trim())}
      />
      <input type="button" value="Enable" onClick={onEnableClicked} />
      <input type="button" value="Cancel" onClick={onCancelled} />
    </>
  )
}
```

----------------------------------------

TITLE: Scheduling Edge Function Invocation with pg_cron and pg_net (SQL)
DESCRIPTION: This SQL snippet uses the `pg_cron` extension to schedule a recurring task that invokes a Supabase Edge Function every minute. It leverages `pg_net` to perform an HTTP POST request, dynamically retrieving the project URL and anonymous key from `vault.decrypted_secrets` for authentication, and sends a JSON body containing the current timestamp.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/functions/schedule-functions.mdx#_snippet_1

LANGUAGE: SQL
CODE:
```
select
  cron.schedule(
    'invoke-function-every-minute',
    '* * * * *', -- every minute
    $$
    select
      net.http_post(
          url:= (select decrypted_secret from vault.decrypted_secrets where name = 'project_url') || '/functions/v1/function-name',
          headers:=jsonb_build_object(
            'Content-type', 'application/json',
            'Authorization', 'Bearer ' || (select decrypted_secret from vault.decrypted_secrets where name = 'anon_key')
          ),
          body:=concat('{"time": "', now(), '"}')::jsonb
      ) as request_id;
    $$
  );
```

----------------------------------------

TITLE: Initializing Supabase Client in TypeScript
DESCRIPTION: This TypeScript snippet demonstrates how to create and export a Supabase client instance using the `createClient` function from `@supabase/supabase-js`. It retrieves the Supabase URL and `anon` key from environment variables, enabling the application to interact with Supabase services like authentication and database operations.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/getting-started/tutorials/with-ionic-vue.mdx#_snippet_3

LANGUAGE: typescript
CODE:
```
import { createClient } from '@supabase/supabase-js';

const supabaseUrl = import.meta.env.VITE_SUPABASE_URL as string;
const supabaseAnonKey = import.meta.env.VITE_SUPABASE_ANON_KEY as string;

export const supabase = createClient(supabaseUrl, supabaseAnonKey);
```

----------------------------------------

TITLE: Declaring Supabase Environment Variables (.env.local)
DESCRIPTION: This snippet shows the required environment variables for connecting a Next.js application to Supabase. Users must rename `.env.example` to `.env.local` and replace the placeholders with their actual Supabase URL and anonymous key, which are crucial for API communication.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/getting-started/quickstarts/nextjs.mdx#_snippet_1

LANGUAGE: text
CODE:
```
NEXT_PUBLIC_SUPABASE_URL=<SUBSTITUTE_SUPABASE_URL>
NEXT_PUBLIC_SUPABASE_ANON_KEY=<SUBSTITUTE_SUPABASE_ANON_KEY>
```

----------------------------------------

TITLE: Configuring Supabase Client with Environment Variables (TypeScript)
DESCRIPTION: This TypeScript snippet defines and exports `supabaseClient` by initializing it with `createClient` from `@refinedev/supabase`. It retrieves the Supabase URL and anonymous key from Vite environment variables (`VITE_SUPABASE_URL`, `VITE_SUPABASE_ANON_KEY`) and configures the client to use the 'public' schema and persist user sessions. This client is central for all Supabase interactions within the Refine app.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/getting-started/tutorials/with-refine.mdx#_snippet_3

LANGUAGE: typescript
CODE:
```
import { createClient } from '@refinedev/supabase'

const supabaseUrl = import.meta.env.VITE_SUPABASE_URL
const supabaseAnonKey = import.meta.env.VITE_SUPABASE_ANON_KEY

export const supabaseClient = createClient(supabaseUrl, supabaseAnonKey, {
  db: {
    schema: 'public',
  },
  auth: {
    persistSession: true,
  },
})
```

----------------------------------------

TITLE: Initializing Supabase Client with Service Role Key (TypeScript)
DESCRIPTION: This snippet demonstrates how to create a Supabase client instance specifically for server-side operations using the `service_role` secret. It imports `createClient` from `@supabase/supabase-js` and configures the `auth` object to disable session persistence, auto-refresh, and URL session detection, which are crucial for secure server environments to prevent accidental exposure of the secret.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/troubleshooting/performing-administration-tasks-on-the-server-side-with-the-servicerole-secret-BYM4Fa.mdx#_snippet_0

LANGUAGE: TypeScript
CODE:
```
import { createClient } from '@supabase/supabase-js'

const supabase = createClient(supabaseUrl, serviceRoleSecret, {
  auth: {
    persistSession: false,
    autoRefreshToken: false,
    detectSessionInUrl: false,
  },
})
```

----------------------------------------

TITLE: Creating a Public SELECT Policy for Profiles Table
DESCRIPTION: This sequence of SQL commands first creates a `profiles` table, then enables Row Level Security on it. Finally, it defines a `SELECT` policy that makes all profiles visible to everyone, including unauthenticated users, by granting access to the `anon` role with a `true` condition.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/database/postgres/row-level-security.mdx#_snippet_5

LANGUAGE: SQL
CODE:
```
create table profiles (
  id uuid primary key,
  user_id references auth.users,
  avatar_url text
);

alter table profiles enable row level security;

create policy "Public profiles are visible to everyone."
on profiles for select
to anon         -- the Postgres Role (recommended)
using ( true ); -- the actual Policy
```

----------------------------------------

TITLE: Computing Routes with Google Maps API in a Supabase Edge Function
DESCRIPTION: This TypeScript Deno function serves as an Edge Function to compute routes between an origin and destination using the Google Maps Directions API. It handles incoming JSON requests, makes a POST request to the Google API, and returns the route details, including duration, distance, and polyline.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/www/_blog/2024-09-05-flutter-uber-clone.mdx#_snippet_11

LANGUAGE: TypeScript
CODE:
```
type Coordinates = {
  latitude: number
  longitude: number
}

Deno.serve(async (req) => {
  const {
    origin,
    destination,
  }: {
    origin: Coordinates
    destination: Coordinates
  } = await req.json()

  const response = await fetch(
    `https://routes.googleapis.com/directions/v2:computeRoutes?key=${Deno.env.get(
      'GOOGLE_MAPS_API_KEY'
    )}`,
    {
      method: 'POST',
      headers: {
        'Content-Type': 'application/json',
        'X-Goog-FieldMask':
          'routes.duration,routes.distanceMeters,routes.polyline,routes.legs.polyline',
      },
      body: JSON.stringify({
        origin: { location: { latLng: origin } },
        destination: { location: { latLng: destination } },
        travelMode: 'DRIVE',
        polylineEncoding: 'GEO_JSON_LINESTRING',
      }),
    }
  )

  if (!response.ok) {
    const error = await response.json()
    console.error({ error })
    throw new Error(`HTTP error! status: ${response.status}`)
  }

  const data = await response.json()

  const res = data.routes[0]

  return new Response(JSON.stringify(res), { headers: { 'Content-Type': 'application/json' } })
})
```

----------------------------------------

TITLE: Initializing Supabase Client in JavaScript
DESCRIPTION: This code initializes the Supabase client using the `createClient` method. It requires the Supabase project URL and the anonymous (anon) public key, which are obtained from the Supabase dashboard's API settings. This client instance is then used for all database interactions.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/www/_blog/2021-03-11-using-supabase-replit.mdx#_snippet_1

LANGUAGE: JavaScript
CODE:
```
const supabase = createClient(
  'https://ajsstlnzcmdmzbtcgbbd.supabase.co',
  'eyJhbGciOiJIUzI1NiIsInR5cCI6IkpXVCJ9...'
)
```

----------------------------------------

TITLE: Initializing Supabase Client
DESCRIPTION: This section demonstrates how to initialize the Supabase client using your project's URL and anonymous public API key. These credentials can be found in your Supabase project's API Settings.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/realtime/presence.mdx#_snippet_0

LANGUAGE: JavaScript
CODE:
```
import { createClient } from '@supabase/supabase-js'

const SUPABASE_URL = 'https://<project>.supabase.co'
const SUPABASE_KEY = '<your-anon-key>'

const supabase = createClient(SUPABASE_URL, SUPABASE_KEY)
```

LANGUAGE: Dart
CODE:
```
void main() {
  Supabase.initialize(
    url: 'https://<project>.supabase.co',
    anonKey: '<your-anon-key>',
  );

  runApp(MyApp());
}

final supabase = Supabase.instance.client;
```

LANGUAGE: Swift
CODE:
```
let supabaseURL = "https://<project>.supabase.co"
let supabaseKey = "<your-anon-key>"
let supabase = SupabaseClient(supabaseURL: URL(string: supabaseURL)!, supabaseKey: supabaseKey)

let realtime = supabase.realtime
```

LANGUAGE: Kotlin
CODE:
```
val supabaseUrl = "https://<project>.supabase.co"
val supabaseKey = "<your-anon-key>"
val supabase = createSupabaseClient(supabaseUrl, supabaseKey) {
    install(Realtime)
}
```

LANGUAGE: Python
CODE:
```
from supabase import create_client

SUPABASE_URL = 'https://<project>.supabase.co'
SUPABASE_KEY = '<your-anon-key>'

supabase = create_client(SUPABASE_URL, SUPABASE_KEY)
```

----------------------------------------

TITLE: Implementing a PostgreSQL Function as a Trigger for `updated_at`
DESCRIPTION: This example provides a PostgreSQL function `update_updated_at` designed to be used as a trigger. It automatically updates the `updated_at` column of a row to the current timestamp (`now()`) whenever the row is modified. The accompanying `CREATE TRIGGER` statement attaches this function to the `my_schema.my_table` for `BEFORE UPDATE` events.
SOURCE: https://github.com/supabase/supabase/blob/master/examples/prompts/database-functions.md#_snippet_2

LANGUAGE: PostgreSQL
CODE:
```
create or replace function my_schema.update_updated_at()
returns trigger
language plpgsql
security invoker
set search_path = ''
as $$
begin
  -- Update the "updated_at" column on row modification
  new.updated_at := now();
  return new;
end;
$$;

create trigger update_updated_at_trigger
before update on my_schema.my_table
for each row
execute function my_schema.update_updated_at();
```

----------------------------------------

TITLE: Subscribing to Postgres Database Changes with Supabase Realtime (JavaScript)
DESCRIPTION: This example demonstrates how to subscribe to real-time changes from a Postgres database using Supabase Realtime. Clients can listen for specific events (e.g., INSERT, UPDATE, DELETE, or all '*') within a schema or table. Row Level Security (RLS) is crucial for controlling which changes clients are authorized to receive.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/realtime/concepts.mdx#_snippet_3

LANGUAGE: JavaScript
CODE:
```
import { createClient } from '@supabase/supabase-js'
const supabase = createClient('your_project_url', 'your_supabase_api_key')

const allChanges = supabase
  .channel('schema-db-changes')
  .on(
    'postgres_changes',
    {
      event: '*',
      schema: 'public'
    },
    (payload) => console.log(payload)
  )
  .subscribe()
```

----------------------------------------

TITLE: Initiating Supabase User Signup in JavaScript
DESCRIPTION: This JavaScript snippet demonstrates how to sign up a new user with Supabase using `supabase.auth.signUp()`. It requires an email and password, and optionally allows specifying an `emailRedirectTo` URL for post-confirmation redirection, which must be configured in the Supabase dashboard or CLI.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/auth/passwords.mdx#_snippet_11

LANGUAGE: JavaScript
CODE:
```
import { createClient } from '@supabase/supabase-js'
const supabase = createClient('https://your-project.supabase.co', 'your-anon-key')

// ---cut---
async function signUpNewUser() {
  const { data, error } = await supabase.auth.signUp({
    email: 'valid.email@supabase.io',
    password: 'example-password',
    options: {
      emailRedirectTo: 'https://example.com/welcome',
    },
  })
}
```

----------------------------------------

TITLE: Analyzing Query Execution Plans with EXPLAIN ANALYZE (SQL)
DESCRIPTION: This SQL command is used to analyze the execution plan of a given query. When `ANALYZE` is included, the query is actually executed, providing detailed runtime statistics. It's crucial for optimizing slow queries but should be used cautiously with DML statements (INSERT/UPDATE/DELETE) due to side effects. Omitting `ANALYZE` provides a plan without execution.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/database/inspect.mdx#_snippet_8

LANGUAGE: SQL
CODE:
```
explain analyze <query-statement-here>;
```

----------------------------------------

TITLE: Initializing Supabase Client with Encrypted Session Storage
DESCRIPTION: This TypeScript code defines a `LargeSecureStore` class that implements encrypted session storage for Supabase in React Native. It uses `expo-secure-store` for small encryption keys and `AsyncStorage` for the larger encrypted session data, leveraging `aes-js` for AES-256 encryption. The Supabase client is then initialized to use this custom storage, enhancing the security of user sessions.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/www/_blog/2023-11-16-react-native-authentication.mdx#_snippet_3

LANGUAGE: typescript
CODE:
```
import 'react-native-url-polyfill/auto'
import { createClient } from '@supabase/supabase-js'
import AsyncStorage from '@react-native-async-storage/async-storage'
import * as SecureStore from 'expo-secure-store'
import * as aesjs from 'aes-js'
import 'react-native-get-random-values'

// As Expo's SecureStore does not support values larger than 2048
// bytes, an AES-256 key is generated and stored in SecureStore, while
// it is used to encrypt/decrypt values stored in AsyncStorage.
class LargeSecureStore {
  private async _encrypt(key: string, value: string) {
    const encryptionKey = crypto.getRandomValues(new Uint8Array(256 / 8))

    const cipher = new aesjs.ModeOfOperation.ctr(encryptionKey, new aesjs.Counter(1))
    const encryptedBytes = cipher.encrypt(aesjs.utils.utf8.toBytes(value))

    await SecureStore.setItemAsync(key, aesjs.utils.hex.fromBytes(encryptionKey))

    return aesjs.utils.hex.fromBytes(encryptedBytes)
  }

  private async _decrypt(key: string, value: string) {
    const encryptionKeyHex = await SecureStore.getItemAsync(key)
    if (!encryptionKeyHex) {
      return encryptionKeyHex
    }

    const cipher = new aesjs.ModeOfOperation.ctr(
      aesjs.utils.hex.toBytes(encryptionKeyHex),
      new aesjs.Counter(1)
    )
    const decryptedBytes = cipher.decrypt(aesjs.utils.hex.toBytes(value))

    return aesjs.utils.utf8.fromBytes(decryptedBytes)
  }

  async getItem(key: string) {
    const encrypted = await AsyncStorage.getItem(key)
    if (!encrypted) {
      return encrypted
    }

    return await this._decrypt(key, encrypted)
  }

  async removeItem(key: string) {
    await AsyncStorage.removeItem(key)
    await SecureStore.deleteItemAsync(key)
  }

  async setItem(key: string, value: string) {
    const encrypted = await this._encrypt(key, value)

    await AsyncStorage.setItem(key, encrypted)
  }
}

const supabaseUrl = YOUR_REACT_NATIVE_SUPABASE_URL
const supabaseAnonKey = YOUR_REACT_NATIVE_SUPABASE_ANON_KEY

const supabase = createClient(supabaseUrl, supabaseAnonKey, {
  auth: {
    storage: new LargeSecureStore(),
    autoRefreshToken: true,
    persistSession: true,
    detectSessionInUrl: false,
  },
})
```

----------------------------------------

TITLE: Explaining a Query Plan (SQL)
DESCRIPTION: This command is used to display the execution plan for a given SQL query. It helps developers understand how Postgres will execute the query, including which indexes are used, join methods, and scan types, which is crucial for performance tuning and debugging.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/troubleshooting/how-postgres-chooses-which-index-to-use-_JHrf4.mdx#_snippet_3

LANGUAGE: SQL
CODE:
```
EXPLAIN <your query>
```

----------------------------------------

TITLE: Implementing Generative Search Interface with Supabase Edge Functions (JavaScript)
DESCRIPTION: This JavaScript function `onSubmit` handles a user's search query by sending it to a Supabase Edge Function via `EventSource`. It streams the AI-generated answer back, appending it to `answer.value` until a `[DONE]` signal is received. The function manages loading states and error handling, demonstrating a headless integration for generative Q&A.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/ai/examples/headless-vector-search.mdx#_snippet_1

LANGUAGE: JavaScript
CODE:
```
const onSubmit = (e: Event) => {
  e.preventDefault()
  answer.value = ""
  isLoading.value = true

  const query = new URLSearchParams({ query: inputRef.current!.value })
  const projectUrl = `https://your-project-ref.supabase.co/functions/v1`
  const queryURL = `${projectUrl}/${query}`
  const eventSource = new EventSource(queryURL)

  eventSource.addEventListener("error", (err) => {
    isLoading.value = false
    console.error(err)
  })

  eventSource.addEventListener("message", (e: MessageEvent) => {
    isLoading.value = false

    if (e.data === "[DONE]") {
      eventSource.close()
      return
    }

    const completionResponse: CreateCompletionResponse = JSON.parse(e.data)
    const text = completionResponse.choices[0].text

    answer.value += text
  });

  isLoading.value = true
}
```

----------------------------------------

TITLE: Creating User Profiles Table and Enabling RLS - Supabase SQL
DESCRIPTION: This snippet defines the 'profiles' table to store public user information, linked to 'auth.users'. It includes constraints for username length and then enables Row Level Security (RLS) on the table, which is crucial for fine-grained access control.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/_partials/user_management_quickstart_sql_template.mdx#_snippet_0

LANGUAGE: SQL
CODE:
```
create table profiles (
  id uuid references auth.users not null primary key,
  updated_at timestamp with time zone,
  username text unique,
  full_name text,
  avatar_url text,
  website text,

  constraint username_length check (char_length(username) >= 3)
);
alter table profiles
  enable row level security;
```

----------------------------------------

TITLE: Svelte Authentication Component with Magic Link
DESCRIPTION: This Svelte component (`src/lib/Auth.svelte`) handles user login and sign-up using Supabase's Magic Link authentication. It includes a form for email input, displays loading states, and provides user feedback via alerts. It depends on the `supabaseClient.ts` for Supabase interaction.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/getting-started/tutorials/with-svelte.mdx#_snippet_5

LANGUAGE: html
CODE:
```
<script lang="ts">
  import { supabase } from '../supabaseClient'

  let loading = false
  let email = ''

  const handleLogin = async () => {
    try {
      loading = true
      const { error } = await supabase.auth.signInWithOtp({ email })
      if (error) throw error
      alert('Check your email for login link!')
    } catch (error) {
      if (error instanceof Error) {
        alert(error.message)
      }
    } finally {
      loading = false
    }
  }
</script>

<div class="row flex-center flex">
  <div class="col-6 form-widget" aria-live="polite">
    <h1 class="header">Supabase + Svelte</h1>
    <p class="description">Sign in via magic link with your email below</p>
    <orm class="form-widget" on:submit|preventDefault="{handleLogin}">
      <div>
        <label for="email">Email</label>
        <input
          id="email"
          class="inputField"
          type="email"
          placeholder="Your email"
          bind:value="{email}"
        />
      </div>
      <div>
        <utton type="submit" class="button block" aria-live="polite" disabled="{loading}">
          <span>{loading ? 'Loading' : 'Send magic link'}</span>
        </utton>
      </div>
    </orm>
  </div>
</div>
```

----------------------------------------

TITLE: Configuring RLS Policy with Custom Session Variable - SQL
DESCRIPTION: This SQL snippet enables Row Level Security (RLS) on the `document_sections` table and defines a policy allowing authenticated users to select only their own document sections. It uses a custom session variable, `app.current_user_id`, to identify the current user, casting it to `bigint` for comparison with `owner_id`.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/ai/rag-with-permissions.mdx#_snippet_9

LANGUAGE: SQL
CODE:
```
-- enable row level security
alter table document_sections enable row level security;

-- setup RLS for select operations
create policy "Users can query their own document sections"
on document_sections for select to authenticated using (
  document_id in (
    select id
    from external.documents
    where owner_id = current_setting('app.current_user_id')::bigint
  )
);
```

----------------------------------------

TITLE: Authenticated OpenAI Realtime API WebSocket Relay - JavaScript
DESCRIPTION: This snippet demonstrates how to build an authenticated WebSocket relay using Supabase Edge Functions to connect client-side applications to OpenAI's Realtime API. It handles WebSocket upgrades, authenticates users via a JWT provided in the URL query parameters using `supabase.auth.getUser`, and then establishes an outbound WebSocket connection to OpenAI. Messages are relayed between the client and OpenAI, protecting the OpenAI API key and ensuring only authenticated users can access the service.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/www/_blog/2024-12-03-edge-functions-background-tasks-websockets.mdx#_snippet_2

LANGUAGE: JavaScript
CODE:
```
import { createClient } from 'jsr:@supabase/supabase-js@2'

const supabase = createClient(
  Deno.env.get('SUPABASE_URL'),
  Deno.env.get('SUPABASE_SERVICE_ROLE_KEY')
)
const OPENAI_API_KEY = Deno.env.get('OPENAI_API_KEY')

Deno.serve(async (req) => {
  const upgrade = req.headers.get('upgrade') || ''

  if (upgrade.toLowerCase() != 'websocket') {
    return new Response("request isn't trying to upgrade to websocket.")
  }

  // WebSocket browser clients does not support sending custom headers.
  // We have to use the URL query params to provide user's JWT.
  // Please be aware query params may be logged in some logging systems.
  const url = new URL(req.url)
  const jwt = url.searchParams.get('jwt')
  if (!jwt) {
    console.error('Auth token not provided')
    return new Response('Auth token not provided', { status: 403 })
  }
  const { error, data } = await supabase.auth.getUser(jwt)
  if (error) {
    console.error(error)
    return new Response('Invalid token provided', { status: 403 })
  }
  if (!data.user) {
    console.error('user is not authenticated')
    return new Response('User is not authenticated', { status: 403 })
  }

  const { socket, response } = Deno.upgradeWebSocket(req)

  socket.onopen = () => {
    // initiate an outbound WebSocket connection to OpenAI
    const url = 'wss://api.openai.com/v1/realtime?model=gpt-4o-realtime-preview-2024-10-01'

    // openai-insecure-api-key isn't a problem since this code runs in an Edge Function
    const openaiWS = new WebSocket(url, [
      'realtime',
      `openai-insecure-api-key.${OPENAI_API_KEY}`,
      'openai-beta.realtime-v1',
    ])

    openaiWS.onopen = () => {
      console.log('Connected to OpenAI server.')

      socket.onmessage = (e) => {
        console.log('socket message:', e.data)
        // only send the message if openAI ws is open
        if (openaiWS.readyState === 1) {
          openaiWS.send(e.data)
        } else {
          socket.send(
            JSON.stringify({
              type: 'error',
              msg: 'openAI connection not ready',
            })
          )
        }
      }
    }

    openaiWS.onmessage = (e) => {
      console.log(e.data)
      socket.send(e.data)
    }

    openaiWS.onerror = (e) => console.log('OpenAI error: ', e.message)
    openaiWS.onclose = (e) => console.log('OpenAI session closed')
  }

  socket.onerror = (e) => console.log('socket errored:', e.message)
  socket.onclose = () => console.log('socket closed')

  return response // 101 (Switching Protocols)
})
```

----------------------------------------

TITLE: Creating an UPDATE Policy for User-Owned Profiles
DESCRIPTION: This example demonstrates creating an `UPDATE` policy for the `profiles` table. It allows authenticated users to update only their own profile, using both `using` (for existing rows) and `with check` (for new row data) clauses to ensure the `user_id` remains consistent with the authenticated user's ID.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/database/postgres/row-level-security.mdx#_snippet_8

LANGUAGE: SQL
CODE:
```
create table profiles (
  id uuid primary key,
  user_id uuid references auth.users,
  avatar_url text
);

alter table profiles enable row level security;

create policy "Users can update their own profile."
on profiles for update
to authenticated                    -- the Postgres Role (recommended)
using ( (select auth.uid()) = user_id )       -- checks if the existing row complies with the policy expression
with check ( (select auth.uid()) = user_id ); -- checks if the new row complies with the policy expression
```

----------------------------------------

TITLE: Creating RLS Policy for Post Owners (SQL)
DESCRIPTION: This SQL snippet demonstrates how to create a Row Level Security (RLS) policy on the 'posts' table. The policy, named 'Allow update for owners', restricts UPDATE operations so that only the user whose 'user_id' matches the authenticated user's UID (retrieved via 'auth.uid()') can modify a row. This ensures that users can only update their own posts.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/database/postgres/column-level-security.mdx#_snippet_0

LANGUAGE: SQL
CODE:
```
create policy "Allow update for owners" on posts for
update
  using ((select auth.uid()) = user_id);
```

----------------------------------------

TITLE: Creating 'instruments' Table and Inserting Sample Data (SQL)
DESCRIPTION: This SQL snippet creates an 'instruments' table with 'id' and 'name' columns, inserts three sample instrument names ('violin', 'viola', 'cello'), and enables Row Level Security (RLS) on the table. It is designed to be run in the Supabase SQL Editor to quickly set up a basic table for testing.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/_partials/quickstart_db_setup.mdx#_snippet_0

LANGUAGE: SQL
CODE:
```
-- Create the table
create table instruments (
  id bigint primary key generated always as identity,
  name text not null
);
-- Insert some sample data into the table
insert into instruments (name)
values
  ('violin'),
  ('viola'),
  ('cello');

alter table instruments enable row level security;
```

----------------------------------------

TITLE: Creating an INSERT Policy for User-Owned Profiles
DESCRIPTION: This SQL block sets up a `profiles` table, enables RLS, and then defines an `INSERT` policy. The policy ensures that only authenticated users can create a profile, and that the `user_id` they attempt to insert matches their own authenticated user ID, preventing unauthorized profile creation.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/database/postgres/row-level-security.mdx#_snippet_7

LANGUAGE: SQL
CODE:
```
create table profiles (
  id uuid primary key,
  user_id uuid references auth.users,
  avatar_url text
);

alter table profiles enable row level security;

create policy "Users can create a profile."
on profiles for insert
to authenticated                          -- the Postgres Role (recommended)
with check ( (select auth.uid()) = user_id );      -- the actual Policy
```

----------------------------------------

TITLE: Enabling the pgvector Extension in Supabase (SQL)
DESCRIPTION: This SQL command ensures that the `pgvector` extension is enabled within the current database schema, specifically within the `extensions` schema. This is a prerequisite for utilizing any of the pgvector functionalities, including vector types and distance operators, in a Supabase project.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/www/_blog/2024-05-01-pgvector-0-7-0.mdx#_snippet_10

LANGUAGE: SQL
CODE:
```
create extension if not exists vector
with
  schema extensions;
```

----------------------------------------

TITLE: Fetching Data Client-Side with React Query in Next.js Client Components
DESCRIPTION: This snippet demonstrates a standard client-side data fetching approach using `useQuery` from `@supabase-cache-helpers/postgrest-react-query` within a Next.js Client Component. It checks if data was pre-fetched server-side; if not, it proceeds to fetch the data client-side using the browser Supabase client. The component includes basic loading and error handling states for a robust user experience.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/www/_blog/2024-01-12-react-query-nextjs-app-router-cache-helpers.mdx#_snippet_11

LANGUAGE: tsx
CODE:
```
'use client'

import useSupabaseBrowser from '@/utils/supabase-browser'
import { getCountryById } from '@/queries/get-country-by-id'
import { useQuery } from '@supabase-cache-helpers/postgrest-react-query'

export default function CountryPage({ params }: { params: { id: number } }) {
  const supabase = useSupabaseBrowser()
  const { data: country, isLoading, isError } = useQuery(getCountryById(supabase, params.id))

  if (isLoading) {
    return <div>Loading...</div>
  }

  if (isError || !country) {
    return <div>Error</div>
  }

  return (
    <div>
      <h1>{country.name}</h1>
    </div>
  )
}
```

----------------------------------------

TITLE: Creating Server Supabase Client for Server Components
DESCRIPTION: Provides a function `useSupabaseServer` to create a server-side Supabase client using `@supabase/ssr`'s `createServerClient`. This client is designed for use in Next.js server components and handles cookie management for authentication, ensuring secure and authenticated data fetching on the server.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/www/_blog/2024-01-12-react-query-nextjs-app-router-cache-helpers.mdx#_snippet_7

LANGUAGE: typescript
CODE:
```
import { createServerClient } from '@supabase/ssr'
import { cookies } from 'next/headers'
import { Database } from './database.types'

export default function useSupabaseServer(cookieStore: ReturnType<typeof cookies>) {
  return createServerClient<Database>(
    process.env.NEXT_PUBLIC_SUPABASE_URL!,
    process.env.NEXT_PUBLIC_SUPABASE_ANON_KEY!,
    {
      cookies: {
        get(name: string) {
          return cookieStore.get(name)?.value
        }
      }
    }
  )
}
```

----------------------------------------

TITLE: Signing In with Email and Password - Dart
DESCRIPTION: This snippet shows how to authenticate a user in Dart by calling `signInWithPassword()` on the Supabase client. It asynchronously signs in with the provided email and password, returning an `AuthResponse`.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/auth/passwords.mdx#_snippet_17

LANGUAGE: Dart
CODE:
```
Future<void> signInWithEmail() async {
  final AuthResponse res = await supabase.auth.signInWithPassword(
    email: 'valid.email@supabase.io',
    password: 'example-password'
  );
}
```

----------------------------------------

TITLE: Defining User Profiles Table with Row Level Security Policies
DESCRIPTION: This SQL snippet defines the 'profiles' table for public user profiles, including fields for ID, update timestamp, username, avatar URL, and website. It establishes robust Row Level Security (RLS) policies to control access: public viewing, user-specific insertion, and user-specific updates. Additionally, it configures Supabase Realtime for the 'profiles' table and sets up a 'avatars' storage bucket with policies for public access and user uploads.
SOURCE: https://github.com/supabase/supabase/blob/master/examples/user-management/react-user-management/README.md#_snippet_1

LANGUAGE: sql
CODE:
```
-- Create a table for Public Profiles
create table
  profiles (
    id uuid references auth.users not null,
    updated_at timestamp
    with
      time zone,
      username text unique,
      avatar_url text,
      website text,
      primary key (id),
      unique (username),
      constraint username_length check (char_length(username) >= 3)
  );

alter table
  profiles enable row level security;

create policy "Public profiles are viewable by everyone." on profiles for
select
  using (true);

create policy "Users can insert their own profile." on profiles for insert
with
  check ((select auth.uid()) = id);

create policy "Users can update own profile." on profiles for
update
  using ((select auth.uid()) = id);

-- Set up Realtime!
begin;

drop
  publication if exists supabase_realtime;

create publication supabase_realtime;

commit;

alter
  publication supabase_realtime add table profiles;

-- Set up Storage!
insert into
  storage.buckets (id, name)
values
  ('avatars', 'avatars');

create policy "Avatar images are publicly accessible." on storage.objects for
select
  using (bucket_id = 'avatars');

create policy "Anyone can upload an avatar." on storage.objects for insert
with
  check (bucket_id = 'avatars');
```

----------------------------------------

TITLE: Implementing Row Level Security (RLS) Policies in Supabase SQL
DESCRIPTION: This comprehensive snippet establishes Row Level Security (RLS) across all application tables. It begins by creating a private schema for security-definer helper functions (`get_user_org_role` and `can_add_post`) to manage user roles and enforce post limits. Subsequently, RLS is enabled for each public table, followed by detailed policy definitions for `profiles`, `organizations`, `org_members`, `posts`, and `comments`, ensuring granular access control based on user authentication, roles, and content status.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/local-development/testing/pgtap-extended.mdx#_snippet_7

LANGUAGE: SQL
CODE:
```
-- Create a private schema to store all security definer functions utils
-- As such functions should never be in a API exposed schema
create schema if not exists private;
-- Helper function for role checks
create or replace function private.get_user_org_role(org_id bigint, user_id uuid)
returns text
set search_path = ''
as $$
  select role from public.org_members
  where org_id = $1 and user_id = $2;
-- Note the use of security definer to avoid RLS checking recursion issue
-- see: https://supabase.com/docs/guides/database/postgres/row-level-security#use-security-definer-functions
$$ language sql security definer;
-- Helper utils to check if an org is below the max post limit
create or replace function private.can_add_post(org_id bigint)
returns boolean
set search_path = ''
as $$
  select (select count(*)
          from public.posts p
          where p.org_id = $1) < o.max_posts
  from public.organizations o
  where o.id = $1
$$ language sql security definer;


-- Enable RLS for all tables
alter table public.profiles enable row level security;
alter table public.organizations enable row level security;
alter table public.org_members enable row level security;
alter table public.posts enable row level security;
alter table public.comments enable row level security;

-- Profiles policies
create policy "Public profiles are viewable by everyone"
  on public.profiles for select using (true);

create policy "Users can insert their own profile"
  on public.profiles for insert with check ((select auth.uid()) = id);

create policy "Users can update their own profile"
  on public.profiles for update using ((select auth.uid()) = id)
  with check ((select auth.uid()) = id);

-- Organizations policies
create policy "Public org info visible to all"
  on public.organizations for select using (true);

create policy "Org management restricted to owners"
  on public.organizations for all using (
    private.get_user_org_role(id, (select auth.uid())) = 'owner'
  );

-- Org Members policies
create policy "Members visible to org members"
  on public.org_members for select using (
    private.get_user_org_role(org_id, (select auth.uid())) is not null
  );

create policy "Member management restricted to admins and owners"
  on public.org_members for all using (
    private.get_user_org_role(org_id, (select auth.uid())) in ('owner', 'admin')
  );

-- Posts policies
create policy "Complex post visibility"
  on public.posts for select using (
    -- Published non-premium posts are visible to all
    (status = 'published' and not is_premium)
    or
    -- Premium posts visible to org members only
    (status = 'published' and is_premium and
    private.get_user_org_role(org_id, (select auth.uid())) is not null)
    or
    -- All posts visible to editors and above
    private.get_user_org_role(org_id, (select auth.uid())) in ('owner', 'admin', 'editor')
  );

create policy "Post creation rules"
  on public.posts for insert with check (
    -- Must be org member with appropriate role
    private.get_user_org_role(org_id, (select auth.uid())) in ('owner', 'admin', 'editor')
    and
    -- Check org post limits for free plans
    (
      (select o.plan_type != 'free'
      from organizations o
      where o.id = org_id)
      or
      (select private.can_add_post(org_id))
    )
  );

create policy "Post update rules"
  on public.posts for update using (
    exists (
      select 1
      where
        -- Editors can update non-published posts
        (private.get_user_org_role(org_id, (select auth.uid())) = 'editor' and status != 'published')
        or
        -- Admins and owners can update any post
        private.get_user_org_role(org_id, (select auth.uid())) in ('owner', 'admin')
    )
  );

-- Comments policies
create policy "Comments on published posts are viewable by everyone"
  on public.comments for select using (
    exists (
      select 1 from public.posts
      where id = post_id
      and status = 'published'
    )
    and not is_deleted
  );

create policy "Authenticated users can create comments"
  on public.comments for insert with check ((select auth.uid()) = author_id);

create policy "Users can update their own comments"
  on public.comments for update using (author_id = (select auth.uid()));
```

----------------------------------------

TITLE: Backing Up Supabase Database using CLI
DESCRIPTION: These commands are used to dump different parts of your Supabase database into separate SQL files. `roles.sql` backs up only roles, `schema.sql` backs up the database schema, and `data.sql` backs up only the data using the `COPY` command for efficiency. Replace `[CONNECTION_STRING]` with your actual database connection string.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/platform/migrating-within-supabase/backup-restore.mdx#_snippet_2

LANGUAGE: bash
CODE:
```
supabase db dump --db-url [CONNECTION_STRING] -f roles.sql --role-only
```

LANGUAGE: bash
CODE:
```
supabase db dump --db-url [CONNECTION_STRING] -f schema.sql
```

LANGUAGE: bash
CODE:
```
supabase db dump --db-url [CONNECTION_STRING] -f data.sql --use-copy --data-only
```

----------------------------------------

TITLE: Fetching Data with Supabase in Next.js Server Components
DESCRIPTION: This code demonstrates fetching data from Supabase directly within a Next.js Server Component using `createServerComponentClient`. It accesses cookies via `next/headers` to initialize the Supabase client, enabling secure server-side data retrieval. A middleware setup is a prerequisite for this functionality, and the TypeScript version provides database type inference.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/auth/auth-helpers/nextjs.mdx#_snippet_17

LANGUAGE: jsx
CODE:
```
import { cookies } from 'next/headers'
import { createServerComponentClient } from '@supabase/auth-helpers-nextjs'

export default async function Page() {
  const cookieStore = cookies()
  const supabase = createServerComponentClient({ cookies: () => cookieStore })
  const { data } = await supabase.from('todos').select()
  return <pre>{JSON.stringify(data, null, 2)}</pre>
}
```

LANGUAGE: tsx
CODE:
```
import { cookies } from 'next/headers'
import { createServerComponentClient } from '@supabase/auth-helpers-nextjs'

import type { Database } from '@/lib/database.types'

export default async function ServerComponent() {
  const cookieStore = cookies()
  const supabase = createServerComponentClient<Database>({ cookies: () => cookieStore })
  const { data } = await supabase.from('todos').select()
  return <pre>{JSON.stringify(data, null, 2)}</pre>
}
```

----------------------------------------

TITLE: Retrieving Most Frequently Called Postgres Queries (SQL)
DESCRIPTION: This SQL query retrieves statistics for the top 100 most frequently executed queries from `pg_stat_statements`. It provides details such as the calling role, call count, total/min/max/mean execution times, and average rows returned, aiding in identifying frequently used queries for optimization.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/database/inspect.mdx#_snippet_4

LANGUAGE: SQL
CODE:
```
select
  auth.rolname,
  statements.query,
  statements.calls,
  -- -- Postgres 13, 14, 15
  statements.total_exec_time + statements.total_plan_time as total_time,
  statements.min_exec_time + statements.min_plan_time as min_time,
  statements.max_exec_time + statements.max_plan_time as max_time,
  statements.mean_exec_time + statements.mean_plan_time as mean_time,
  -- -- Postgres <= 12
  -- total_time,
  -- min_time,
  -- max_time,
  -- mean_time,
  statements.rows / statements.calls as avg_rows
from
  pg_stat_statements as statements
  inner join pg_authid as auth on statements.userid = auth.oid
order by statements.calls desc
limit 100;
```

----------------------------------------

TITLE: Generating and Storing Text Embeddings via Database Webhook - TypeScript
DESCRIPTION: This TypeScript Edge Function acts as a database webhook, triggered when a row is inserted or updated in the `embeddings` table. It uses the `Supabase.ai.Session` with the `gte-small` model to generate a vector embedding for the `content` field and then updates the corresponding row in the `embeddings` table with the generated embedding.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/functions/examples/semantic-search.mdx#_snippet_1

LANGUAGE: TypeScript
CODE:
```
const model = new Supabase.ai.Session('gte-small')

Deno.serve(async (req) => {
  const payload: WebhookPayload = await req.json()
  const { content, id } = payload.record

  // Generate embedding.
  const embedding = await model.run(content, {
    mean_pool: true,
    normalize: true,
  })

  // Store in database.
  const { error } = await supabase
    .from('embeddings')
    .update({ embedding: JSON.stringify(embedding) })
    .eq('id', id)
  if (error) console.warn(error.message)

  return new Response('ok')
})
```

----------------------------------------

TITLE: Installing Supabase CLI Globally (Bash)
DESCRIPTION: This command installs the Supabase Command Line Interface (CLI) globally on your system using npm, making the `supabase` command available from any directory. It is a prerequisite for local Supabase development and project management.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/www/_blog/2021-03-31-supabase-cli.mdx#_snippet_0

LANGUAGE: bash
CODE:
```
npm install -g supabase
```

----------------------------------------

TITLE: Fetching Data with Supabase JavaScript Client
DESCRIPTION: This JavaScript snippet uses the Supabase client library to query all data from the 'todos' table. It performs a select() operation and destructures the response into data and error objects, providing a concise way to retrieve records.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/api/quickstart.mdx#_snippet_4

LANGUAGE: JavaScript
CODE:
```
const { data, error } = await supabase.from('todos').select()
```

----------------------------------------

TITLE: Implementing AI-Powered Filter Generation API with TypeScript
DESCRIPTION: This TypeScript code defines an API endpoint (POST) that uses the AI SDK to generate complex filter groups from natural language prompts. It leverages Zod for robust schema validation of both input filter properties and the generated filter structure, including nested filter groups. The endpoint validates generated property names and operators against predefined properties, ensuring data integrity and preventing invalid queries.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/design-system/content/docs/fragments/filter-bar.mdx#_snippet_6

LANGUAGE: TypeScript
CODE:
```
import { generateObject } from 'ai'
import { openai } from '@ai-sdk/openai'
import { z } from 'zod'

// Define schemas for validation
const FilterProperty = z.object({
  label: z.string(),
  name: z.string(),
  type: z.enum(['string', 'number', 'date', 'boolean']),
  options: z.array(z.string()).optional(),
  operators: z.array(z.string()).optional()
})

const FilterCondition = z.object({
  propertyName: z.string(),
  value: z.union([z.string(), z.number(), z.boolean(), z.null()]),
  operator: z.string()
})

type FilterGroupType = {
  logicalOperator: 'AND' | 'OR'
  conditions: Array<z.infer<typeof FilterCondition> | FilterGroupType>
}

const FilterGroup: z.ZodType<FilterGroupType> = z.lazy(() =>
  z.object({
    logicalOperator: z.enum(['AND', 'OR']),
    conditions: z.array(z.union([FilterCondition, FilterGroup]))
  })
)

export async function POST(req: Request) {
  const { prompt, filterProperties } = await req.json()
  const filterPropertiesString = JSON.stringify(filterProperties)

  try {
    const { object } = await generateObject({
      model: openai('gpt-4-mini'),
      schema: FilterGroup,
      prompt: `Generate a filter group based on the following prompt: "${prompt}". 
              Use only these filter properties: ${filterPropertiesString}. 
              Each property has its own set of valid operators defined in the operators field. 
              Return a filter group with a logical operator ('AND'/'OR') and an array of conditions. 
              Each condition can be either a filter condition or another filter group. 
              Filter conditions should have the structure: { propertyName: string, value: string | number | boolean | null, operator: string }. 
              Ensure that the generated filters use only the provided property names and their corresponding operators.`
    })

    // Validate that all propertyNames exist in filterProperties
    const validatePropertyNames = (group: FilterGroupType): boolean => {
      return group.conditions.every((condition) => {
        if ('logicalOperator' in condition) {
          return validatePropertyNames(condition as FilterGroupType)
        }
        const property = filterProperties.find(
          (p: z.infer<typeof FilterProperty>) => p.name === condition.propertyName
        )
        if (!property) return false
        // Validate operator is valid for this property
        return property.operators?.includes(condition.operator) ?? false
      })
    }

    if (!validatePropertyNames(object)) {
      throw new Error('Invalid property names or operators in generated filter')
    }

    // Zod will throw an error if the object doesn't match the schema
    const validatedFilters = FilterGroup.parse(object)
    return Response.json(validatedFilters)
  } catch (error: any) {
    console.error('Error in AI filtering:', error)
    return Response.json({ error: error.message || 'AI filtering failed' }, { status: 500 })
  }
}
```

----------------------------------------

TITLE: Handling Supabase User Registration with Email Confirmation in Flutter
DESCRIPTION: This Dart code defines the `RegisterPage` in a Flutter application, managing user sign-up with Supabase. It includes form validation for email, password, and username, and handles the Supabase `signUp` method. Crucially, it listens to `supabase.auth.onAuthStateChange` to navigate the user to `RoomsPage` only after their email is confirmed and a session is established, ensuring proper redirection post-confirmation.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/www/_blog/2022-11-22-flutter-authorization-with-rls.mdx#_snippet_9

LANGUAGE: Dart
CODE:
```
import 'dart:async';

import 'package:flutter/material.dart';
import 'package:my_chat_app/pages/login_page.dart';
import 'package:my_chat_app/pages/rooms_page.dart';
import 'package:my_chat_app/utils/constants.dart';
import 'package:supabase_flutter/supabase_flutter.dart';

class RegisterPage extends StatefulWidget {
  const RegisterPage(
      {Key? key, required this.isRegistering})
      : super(key: key);

  static Route<void> route({bool isRegistering = false}) {
    return MaterialPageRoute(
      builder: (context) =>
          RegisterPage(isRegistering: isRegistering),
    );
  }

  final bool isRegistering;

  @override
  State<RegisterPage> createState() => _RegisterPageState();
}

class _RegisterPageState extends State<RegisterPage> {
  final bool _isLoading = false;

  final _formKey = GlobalKey<FormState>();

  final _emailController = TextEditingController();
  final _passwordController = TextEditingController();
  final _usernameController = TextEditingController();

  late final StreamSubscription<AuthState>
      _authSubscription;

  @override
  void initState() {
    super.initState();

    bool haveNavigated = false;
    // Listen to auth state to redirect user when the user clicks on confirmation link
    _authSubscription =
        supabase.auth.onAuthStateChange.listen((data) {
      final session = data.session;
      if (session != null && !haveNavigated) {
        haveNavigated = true;
        Navigator.of(context)
            .pushReplacement(RoomsPage.route());
      }
    });
  }

  @override
  void dispose() {
    super.dispose();

    // Dispose subscription when no longer needed
    _authSubscription.cancel();
  }

  Future<void> _signUp() async {
    final isValid = _formKey.currentState!.validate();
    if (!isValid) {
      return;
    }
    final email = _emailController.text;
    final password = _passwordController.text;
    final username = _usernameController.text;
    try {
      await supabase.auth.signUp(
        email: email,
        password: password,
        data: {'username': username},
        emailRedirectTo: 'io.supabase.chat://login',
      );
      context.showSnackBar(
          message:
              'Please check your inbox for confirmation email.');
    } on AuthException catch (error) {
      context.showErrorSnackBar(message: error.message);
    } catch (error) {
      debugPrint(error.toString());
      context.showErrorSnackBar(
          message: unexpectedErrorMessage);
    }
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: const Text('Register'),
      ),
      body: Form(
        key: _formKey,
        child: ListView(
          padding: formPadding,
          children: [
            TextFormField(
              controller: _emailController,
              decoration: const InputDecoration(
                label: Text('Email'),
              ),
              validator: (val) {
                if (val == null || val.isEmpty) {
                  return 'Required';
                }
                return null;
              },
              keyboardType: TextInputType.emailAddress,
            ),
            spacer,
            TextFormField(
              controller: _passwordController,
              obscureText: true,
              decoration: const InputDecoration(
                label: Text('Password'),
              ),
              validator: (val) {
                if (val == null || val.isEmpty) {
                  return 'Required';
                }
                if (val.length < 6) {
                  return '6 characters minimum';
                }
                return null;
              },
            ),
            spacer,
            TextFormField(
              controller: _usernameController,
              decoration: const InputDecoration(
                label: Text('Username'),
              ),
              validator: (val) {
                if (val == null || val.isEmpty) {
                  return 'Required';
                }
                final isValid =
                    RegExp(r'^[A-Za-z0-9_]{3,24}$').hasMatch(val);
                if (!isValid) {
                  return '3-24 long with alphanumeric or underscore';
                }
                return null;
              },
            ),
            spacer,
            ElevatedButton(
              onPressed: _isLoading ? null : _signUp,
              child: const Text('Register'),
            ),
            spacer,
            TextButton(
                onPressed: () {
                  Navigator.of(context)
                      .push(LoginPage.route());
                },
                child:
                    const Text('I already have an account'))
          ]
        )
      )
    );
  }
}
```

----------------------------------------

TITLE: Deprecating auth.role() in Postgres RLS Policies
DESCRIPTION: This snippet illustrates the deprecation of the `auth.role()` function in Supabase's Row Level Security policies. It shows the old, deprecated method and the new, recommended approach using Postgres's native `TO` field for specifying roles, which is more efficient and idiomatic.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/troubleshooting/deprecated-rls-features-Pm77Zs.mdx#_snippet_0

LANGUAGE: SQL
CODE:
```
-- DEPRECATED
create policy "Public profiles are viewable by everyone."
on profiles for select using (
  auth.role() = 'authenticated' or auth.role() = 'anon'
);
```

LANGUAGE: SQL
CODE:
```
-- RECOMMENDED
create policy "Public profiles are viewable by everyone."
on profiles for select
to authenticated, anon
using (
  true
);
```

----------------------------------------

TITLE: Configuring Pre-Request Hook for Rate Limiting - Supabase SQL
DESCRIPTION: Sets the `pgrst.db_pre_request` setting for the `authenticator` role to `public.check_request`, ensuring the rate limiting function runs before every Data API request. A `notify pgrst, 'reload config'` command is issued to apply the changes immediately.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/api/securing-your-api.mdx#_snippet_11

LANGUAGE: SQL
CODE:
```
alter role authenticator
  set pgrst.db_pre_request = 'public.check_request';

notify pgrst, 'reload config';
```

----------------------------------------

TITLE: Loading Store Data and Images with Supabase (TypeScript)
DESCRIPTION: This TypeScript snippet defines two asynchronous functions within a service. `loadStoreInformation` fetches a single store's details from the 'stores' table in Supabase based on its ID. `getStoreImage` retrieves a public URL for a store's image from Supabase Storage, applying a transformation to resize the image to a width of 300px while maintaining its aspect ratio. Both functions interact with the Supabase client.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/www/_blog/2023-03-01-geo-queries-with-postgis-in-ionic-angular.mdx#_snippet_27

LANGUAGE: TypeScript
CODE:
```
  // Load data from Supabase database
  async loadStoreInformation(id: number) {
    const { data } = await this.supabase
      .from('stores')
      .select('*')
      .match({ id })
      .single();
    return data;
  }

  async getStoreImage(id: number) {
    // Get image for a store and transform it automatically!
    return this.supabase.storage
      .from('stores')
      .getPublicUrl(`images/${id}.png`, {
        transform: {
          width: 300,
          resize: 'contain',
        },
      }).data.publicUrl;
  }
```

----------------------------------------

TITLE: Implementing Real-time Chat Page with Supabase and Flutter
DESCRIPTION: This comprehensive Dart code defines the `ChatPage` widget, responsible for displaying real-time messages from Supabase and allowing users to send new ones. It utilizes `StreamBuilder` to listen for message updates, lazily loads sender profiles for display, and includes nested widgets like `_MessageBar` for input and `_ChatBubble` for message rendering. Dependencies include `supabase_flutter` for database interaction and `timeago` for timestamp formatting.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/www/_blog/2022-06-30-flutter-tutorial-building-a-chat-app.mdx#_snippet_13

LANGUAGE: Dart
CODE:
```
import 'dart:async';

import 'package:flutter/material.dart';

import 'package:my_chat_app/models/message.dart';
import 'package:my_chat_app/models/profile.dart';
import 'package:my_chat_app/utils/constants.dart';
import 'package:supabase_flutter/supabase_flutter.dart';
import 'package:timeago/timeago.dart';

/// Page to chat with someone.
///
/// Displays chat bubbles as a ListView and TextField to enter new chat.
class ChatPage extends StatefulWidget {
  const ChatPage({Key? key}) : super(key: key);

  static Route<void> route() {
    return MaterialPageRoute(
      builder: (context) => const ChatPage(),
    );
  }

  @override
  State<ChatPage> createState() => _ChatPageState();
}

class _ChatPageState extends State<ChatPage> {
  late final Stream<List<Message>> _messagesStream;
  final Map<String, Profile> _profileCache = {};

  @override
  void initState() {
    final myUserId = supabase.auth.currentUser!.id;
    _messagesStream = supabase
        .from('messages')
        .stream(primaryKey: ['id'])
        .order('created_at')
        .map((maps) => maps
            .map((map) => Message.fromMap(map: map, myUserId: myUserId))
            .toList());
    super.initState();
  }

  Future<void> _loadProfileCache(String profileId) async {
    if (_profileCache[profileId] != null) {
      return;
    }
    final data =
        await supabase.from('profiles').select().eq('id', profileId).single();
    final profile = Profile.fromMap(data);
    setState(() {
      _profileCache[profileId] = profile;
    });
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: const Text('Chat')),
      body: StreamBuilder<List<Message>>(
        stream: _messagesStream,
        builder: (context, snapshot) {
          if (snapshot.hasData) {
            final messages = snapshot.data!;
            return Column(
              children: [
                Expanded(
                  child: messages.isEmpty
                      ? const Center(
                          child: Text('Start your conversation now :)'),
                        )
                      : ListView.builder(
                          reverse: true,
                          itemCount: messages.length,
                          itemBuilder: (context, index) {
                            final message = messages[index];

                            /// I know it's not good to include code that is not related
                            /// to rendering the widget inside build method, but for
                            /// creating an app quick and dirty, it's fine 😂
                            _loadProfileCache(message.profileId);

                            return _ChatBubble(
                              message: message,
                              profile: _profileCache[message.profileId],
                            );
                          },
                        ),
                ),
                const _MessageBar(),
              ],
            );
          } else {
            return preloader;
          }
        },
      ),
    );
  }
}

/// Set of widget that contains TextField and Button to submit message
class _MessageBar extends StatefulWidget {
  const _MessageBar({
    Key? key,
  }) : super(key: key);

  @override
  State<_MessageBar> createState() => _MessageBarState();
}

class _MessageBarState extends State<_MessageBar> {
  late final TextEditingController _textController;

  @override
  Widget build(BuildContext context) {
    return Material(
      color: Colors.grey[200],
      child: SafeArea(
        child: Padding(
          padding: const EdgeInsets.all(8.0),
          child: Row(
            children: [
              Expanded(
                child: TextFormField(
                  keyboardType: TextInputType.text,
                  maxLines: null,
                  autofocus: true,
                  controller: _textController,
                  decoration: const InputDecoration(
                    hintText: 'Type a message',
                    border: InputBorder.none,
                    focusedBorder: InputBorder.none,
                    contentPadding: EdgeInsets.all(8),
                  ),
                ),
              ),
              TextButton(
                onPressed: () => _submitMessage(),
                child: const Text('Send'),
              ),
            ],
          ),
        ),
      ),
    );
  }

  @override
  void initState() {
    _textController = TextEditingController();
    super.initState();
  }

  @override
  void dispose() {
    _textController.dispose();
    super.dispose();
  }

  void _submitMessage() async {
    final text = _textController.text;
    final myUserId = supabase.auth.currentUser!.id;
    if (text.isEmpty) {
      return;
    }
    _textController.clear();
    try {
      await supabase.from('messages').insert({
        'profile_id': myUserId,
        'content': text,
      });
    } on PostgrestException catch (error) {
      context.showErrorSnackBar(message: error.message);
    } catch (_) {
      context.showErrorSnackBar(message: unexpectedErrorMessage);
    }
  }
}

class _ChatBubble extends StatelessWidget {
  const _ChatBubble({
    Key? key,
    required this.message,
    required this.profile,
  }) : super(key: key);

  final Message message;
  final Profile? profile;

  @override
  Widget build(BuildContext context) {
    List<Widget> chatContents = [
      if (!message.isMine)
        CircleAvatar(
          child: profile == null
              ? preloader
              : Text(profile!.username.substring(0, 2)),
        ),
      const SizedBox(width: 12),
      Flexible(
        child: Container(
          padding: const EdgeInsets.symmetric(
            vertical: 8,
            horizontal: 12,
          ),
          decoration: BoxDecoration(
            color: message.isMine
                ? Theme.of(context).primaryColor
                : Colors.grey[300],
            borderRadius: BorderRadius.circular(8),
          ),
          child: Text(message.content),
        ),
      ),
      const SizedBox(width: 12),
      Text(format(message.createdAt, locale: 'en_short')),
      const SizedBox(width: 60),
    ];
    if (message.isMine) {
      chatContents = chatContents.reversed.toList();
    }
    return Padding(
      padding: const EdgeInsets.symmetric(horizontal: 8, vertical: 18),
      child: Row(
        mainAxisAlignment:
            message.isMine ? MainAxisAlignment.end : MainAxisAlignment.start,
        children: chatContents,
      ),
    );
  }
}
```

----------------------------------------

TITLE: Creating Indexes for WHERE Clause Columns in Postgres
DESCRIPTION: These SQL commands create B-tree indexes on the `sign_up_date` column of the `customers` table and the `status` column of the `orders` table. These indexes are designed to speed up queries that filter data based on these specific columns, improving `WHERE` clause performance.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/database/query-optimization.mdx#_snippet_1

LANGUAGE: SQL
CODE:
```
create index idx_customers_sign_up_date on customers (sign_up_date);

create index idx_orders_status on orders (status);
```

----------------------------------------

TITLE: Enabling Row Level Security (RLS) for a Table - SQL
DESCRIPTION: This SQL command enables Row Level Security (RLS) for a specified table, in this case, 'todos'. Enabling RLS is crucial for restricting access to data based on user authentication tokens and policies, preventing unauthorized access to table rows.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/api/securing-your-api.mdx#_snippet_0

LANGUAGE: SQL
CODE:
```
alter table
  todos enable row level security;
```

----------------------------------------

TITLE: Configuring Row Level Security Policies
DESCRIPTION: This SQL snippet establishes Row Level Security (RLS) policies for the `drivers` and `rides` tables. These policies enforce data access control, allowing authenticated users to select driver information, enabling drivers to update their own availability, and restricting ride data access and updates to the involved driver or passenger.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/www/_blog/2024-09-05-flutter-uber-clone.mdx#_snippet_4

LANGUAGE: sql
CODE:
```
alter table public.drivers enable row level security;
create policy "Any authenticated users can select drivers." on public.drivers for select to authenticated using (true);
create policy "Drivers can update their own status." on public.drivers for update to authenticated using (auth.uid() = id);

alter table public.rides enable row level security;
create policy "The driver or the passenger can select the ride." on public.rides for select to authenticated using (driver_id = auth.uid() or passenger_id = auth.uid());
create policy "The driver can update the status. " on public.rides for update to authenticated using (auth.uid() = driver_id);
```

----------------------------------------

TITLE: Configuring Supabase Client for Server-Side Cookie Storage
DESCRIPTION: This code configures the Supabase client to use cookies for session storage when running in a server environment, such as Next.js Server Components. It overrides the default localStorage behavior by providing custom getItem, setItem, and removeItem functions that interact with a cookieStore (e.g., cookies() from next/headers). This is crucial for maintaining user sessions on the server.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/www/_blog/2023-11-01-supabase-is-now-compatible-with-nextjs-14.mdx#_snippet_3

LANGUAGE: tsx
CODE:
```
const supabase = createClient(supabaseUrl, supabaseAnonKey, {
  auth: {
    flowType: 'pkce',
    autoRefreshToken: false,
    detectSessionInUrl: false,
    persistSession: true,
    storage: {
      getItem: async (key: string) => {
        cookieStore.get(key)
      },
      setItem: async (key: string, value: string) => {
        cookieStore.set(key, value)
      },
      removeItem: async (key: string) => {
        cookieStore.remove(key)
      }
    }
  }
})
```

----------------------------------------

TITLE: Creating User Roles and Permissions Tables in SQL
DESCRIPTION: This SQL script defines custom types for application roles (`app_role`) and permissions (`app_permission`), then creates two tables: `user_roles` to assign roles to users, and `role_permissions` to map specific permissions to each role. This setup forms the foundation for implementing role-based access control within the application.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/database/postgres/custom-claims-and-role-based-access-control-rbac.mdx#_snippet_1

LANGUAGE: sql
CODE:
```
-- Custom types
create type public.app_permission as enum ('channels.delete', 'messages.delete');
create type public.app_role as enum ('admin', 'moderator');

-- USER ROLES
create table public.user_roles (
  id        bigint generated by default as identity primary key,
  user_id   uuid references auth.users on delete cascade not null,
  role      app_role not null,
  unique (user_id, role)
);
comment on table public.user_roles is 'Application roles for each user.';

-- ROLE PERMISSIONS
create table public.role_permissions (
  id           bigint generated by default as identity primary key,
  role         app_role not null,
  permission   app_permission not null,
  unique (role, permission)
);
comment on table public.role_permissions is 'Application permissions for each role.';
```

----------------------------------------

TITLE: Signing In with Magic Link using Kotlin
DESCRIPTION: Illustrates how to initiate a Magic Link sign-in using the Supabase Kotlin client library. The `signInWith(OTP)` method is used, specifying the user's email address.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/auth/auth-email-passwordless.mdx#_snippet_4

LANGUAGE: Kotlin
CODE:
```
suspend fun signInWithEmail() {
	supabase.auth.signInWith(OTP) {
		email = "valid.email@supabase.io"
	}
}
```

----------------------------------------

TITLE: Signing Up a User with Custom Metadata - Dart
DESCRIPTION: This Dart snippet shows how to sign up a new user using `supabase.auth.signUp`. It passes a `data` map containing custom metadata such as `first_name` and `age` directly to the signup function. This metadata is then stored in the `raw_user_meta_data` column of the `auth.users` table.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/auth/managing-user-data.mdx#_snippet_3

LANGUAGE: Dart
CODE:
```
final res = await supabase.auth.signUp(
  email: 'valid.email@supabase.io',
  password: 'example-password',
  data: {
    'first_name': 'John',
    'age': 27,
  },
);
```

----------------------------------------

TITLE: Supabase Database Schema and Policies for User Profiles and Storage
DESCRIPTION: This SQL script defines the `profiles` table, linking it to `auth.users` and enforcing a unique username constraint. It establishes Row Level Security (RLS) policies allowing public viewing, self-insertion, and self-updates for user profiles. Additionally, it configures Supabase Realtime for the `profiles` table and sets up an 'avatars' storage bucket with policies for public access and general upload permissions.
SOURCE: https://github.com/supabase/supabase/blob/master/examples/user-management/vue3-user-management/README.md#_snippet_1

LANGUAGE: sql
CODE:
```
-- Create a table for public "profiles"
create table profiles (
  id uuid references auth.users not null,
  updated_at timestamp with time zone,
  username text unique,
  avatar_url text,
  website text,

  primary key (id),
  unique(username),
  constraint username_length check (char_length(username) >= 3)
);

alter table profiles enable row level security;

create policy "Public profiles are viewable by everyone."
  on profiles for select
  using ( true );

create policy "Users can insert their own profile."
  on profiles for insert
  with check ( (select auth.uid()) = id );

create policy "Users can update own profile."
  on profiles for update
  using ( (select auth.uid()) = id );

-- Set up Realtime!
begin;
  drop publication if exists supabase_realtime;
  create publication supabase_realtime;
commit;
alter publication supabase_realtime add table profiles;

-- Set up Storage!
insert into storage.buckets (id, name)
values ('avatars', 'avatars');

create policy "Avatar images are publicly accessible."
  on storage.objects for select
  using ( bucket_id = 'avatars' );

create policy "Anyone can upload an avatar."
  on storage.objects for insert
  with check ( bucket_id = 'avatars' );
```

----------------------------------------

TITLE: Creating an INSERT Policy for Authenticated Users (SQL)
DESCRIPTION: This policy allows authenticated users to insert new profiles. It uses `WITH CHECK (true)` to permit all insertions by authenticated users, demonstrating the correct way to define a policy for a single operation.
SOURCE: https://github.com/supabase/supabase/blob/master/examples/prompts/database-rls-policies.md#_snippet_5

LANGUAGE: SQL
CODE:
```
create policy "Profiles can be created by any user"
on profiles
for insert
to authenticated
with check ( true );
```

----------------------------------------

TITLE: Creating a SELECT Policy for All Users (SQL)
DESCRIPTION: This policy grants `SELECT` access to the 'profiles' table for both authenticated and unauthenticated users (`anon`). The `using (true)` clause indicates that all rows are viewable without specific conditions.
SOURCE: https://github.com/supabase/supabase/blob/master/examples/prompts/database-rls-policies.md#_snippet_1

LANGUAGE: SQL
CODE:
```
create policy "Profiles are viewable by everyone"
on profiles
for select
to authenticated, anon
using ( true );
```

----------------------------------------

TITLE: Implementing Smarter Search with GPT-3 Context Injection in Supabase Edge Function (TypeScript)
DESCRIPTION: This advanced Edge Function extends the simple search by integrating OpenAI's GPT-3 completion model. It first performs a similarity search to retrieve relevant documents, then tokenizes and concatenates these documents to form a context. This context, along with the user's query, is used to construct a prompt for GPT-3, which then generates a cohesive answer. It also handles CORS and token limits for the context.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/www/_blog/2023-02-03-openai-embeddings-postgres-vector.mdx#_snippet_6

LANGUAGE: TypeScript
CODE:
```
import { serve } from 'https://deno.land/std@0.170.0/http/server.ts'
import 'https://deno.land/x/xhr@0.2.1/mod.ts'
import { createClient } from 'jsr:@supabase/supabase-js@2'
import GPT3Tokenizer from 'https://esm.sh/gpt3-tokenizer@1.1.5'
import { Configuration, OpenAIApi } from 'https://esm.sh/openai@3.1.0'
import { oneLine, stripIndent } from 'https://esm.sh/common-tags@1.8.2'
import { supabaseClient } from './lib/supabase'

export const corsHeaders = {
  'Access-Control-Allow-Origin': '*',
  'Access-Control-Allow-Headers': 'authorization, x-client-info, apikey, content-type',
}

serve(async (req) => {
  // Handle CORS
  if (req.method === 'OPTIONS') {
    return new Response('ok', { headers: corsHeaders })
  }

  // Search query is passed in request payload
  const { query } = await req.json()

  // OpenAI recommends replacing newlines with spaces for best results
  const input = query.replace(/\n/g, ' ')

  const configuration = new Configuration({ apiKey: '<YOUR_OPENAI_API_KEY>' })
  const openai = new OpenAIApi(configuration)

  // Generate a one-time embedding for the query itself
  const embeddingResponse = await openai.createEmbedding({
    model: 'text-embedding-ada-002',
    input,
  })

  const [{ embedding }] = embeddingResponse.data.data

  // Fetching whole documents for this simple example.
  //
  // Ideally for context injection, documents are chunked into
  // smaller sections at earlier pre-processing/embedding step.
  const { data: documents } = await supabaseClient.rpc('match_documents', {
    query_embedding: embedding,
    match_threshold: 0.78, // Choose an appropriate threshold for your data
    match_count: 10, // Choose the number of matches
  })

  const tokenizer = new GPT3Tokenizer({ type: 'gpt3' })
  let tokenCount = 0
  let contextText = ''

  // Concat matched documents
  for (let i = 0; i < documents.length; i++) {
    const document = documents[i]
    const content = document.content
    const encoded = tokenizer.encode(content)
    tokenCount += encoded.text.length

    // Limit context to max 1500 tokens (configurable)
    if (tokenCount > 1500) {
      break
    }

    contextText += `${content.trim()}\n---\n`
  }

  const prompt = stripIndent`${oneLine`\n    You are a very enthusiastic Supabase representative who loves\n    to help people! Given the following sections from the Supabase\n    documentation, answer the question using only that information,\n    outputted in markdown format. If you are unsure and the answer\n    is not explicitly written in the documentation, say\n    "Sorry, I don't know how to help with that."`}\n\n    Context sections:\n    ${contextText}\n\n    Question: """\n    ${query}\n    """\n\n    Answer as markdown (including related code snippets if available):\n  `

  // In production we should handle possible errors
  const completionResponse = await openai.createCompletion({
    model: 'text-davinci-003',
    prompt,
    max_tokens: 512, // Choose the max allowed tokens in completion
    temperature: 0, // Set to 0 for deterministic results
  })

  const {
    id,
    choices: [{ text }],
  } = completionResponse.data

  return new Response(JSON.stringify({ id, text }), {
    headers: { ...corsHeaders, 'Content-Type': 'application/json' },
  })
})
```

----------------------------------------

TITLE: Creating Row Level Security Policy for Todos (SQL)
DESCRIPTION: This SQL snippet demonstrates how to create a Row Level Security (RLS) policy in PostgreSQL. The policy, named "Individuals can view their own todos.", restricts SELECT access on the public.todos table, allowing users to only view rows where their user_id matches the authenticated user's UID obtained via auth.uid(). This ensures data privacy and prevents unauthorized access to other users' data.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/www/_blog/2024-08-16-vec2pg.mdx#_snippet_0

LANGUAGE: sql
CODE:
```
create policy "Individuals can view their own todos."
  on public.todos
  for select
  using
    ( ( select auth.uid() ) = user_id );
```

----------------------------------------

TITLE: Creating a SELECT Policy for User-Owned Data in Postgres
DESCRIPTION: This policy allows individuals to view only their own 'todos' by comparing the authenticated user's ID (obtained via `auth.uid()`) with the `user_id` column in the `todos` table. It acts as an implicit `WHERE` clause for `SELECT` operations.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/database/postgres/row-level-security.mdx#_snippet_1

LANGUAGE: SQL
CODE:
```
create policy "Individuals can view their own todos."
on todos for select
using ( (select auth.uid()) = user_id );
```

----------------------------------------

TITLE: Creating Initial Database Schema (PostgreSQL)
DESCRIPTION: This SQL snippet defines the initial database schema for a social application. It creates `uuid-ossp` and `citext` extensions, a custom `email` domain, and `users` and `posts` tables with appropriate columns, primary keys, and foreign key relationships. This setup provides the foundational structure for storing user and post data before implementing view tracking.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/www/_blog/2022-07-18-seen-by-in-postgresql.mdx#_snippet_0

LANGUAGE: sql
CODE:
```
CREATE EXTENSION IF NOT EXISTS uuid-ossp;
CREATE EXTENSION IF NOT EXISTS citext;

-- Create a email domain to represent and constraing email addresses
CREATE DOMAIN email
AS citext
CHECK ( LENGTH(VALUE) <= 255 AND value ~ '^[a-zA-Z0-9.!#$%&''*+/=?^_`{|}~-]+@[a-zA-Z0-9](?:[a-zA-Z0-9-]{0,61}[a-zA-Z0-9])?(?:\.[a-zA-Z0-9](?:[a-zA-Z0-9-]{0,61}[a-zA-Z0-9])?)*$' );

COMMENT ON DOMAIN email is 'lightly validated email address';

-- Create the users table
CREATE TABLE users (
    id bigserial PRIMARY KEY GENERATED BY DEFAULT AS IDENTITY,
    uuid uuid NOT NULL DEFAULT uuid_nonmc_v1(),

    email email NOT NULL,
    name text,
    about_html text,

    created_at timestamptz NOT NULL DEFAULT NOW()
);

-- Create the posts table
CREATE TABLE posts (
    id bigserial PRIMARY KEY GENERATED BY DEFAULT AS IDENTITY,
    uuid uuid NOT NULL DEFAULT uuid_nonmc_v1(),

    title text,
    content text,
    main_image_src text,
    main_link_src text,

    created_by bigint REFERENCES users(id),

    last_hidden_at timestamptz,
    last_updated_at timestamptz,
    created_at timestamptz NOT NULL DEFAULT NOW()
);
```

----------------------------------------

TITLE: Creating RLS Policy for Authenticated User Read Access
DESCRIPTION: This SQL snippet illustrates how to create a Row Level Security (RLS) policy that grants `authenticated` users `SELECT` access to the `profiles` table. When a user logs in via Supabase Auth, their role automatically updates to `authenticated`, enabling this policy and allowing them to read data from the table.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/api/api-keys.mdx#_snippet_3

LANGUAGE: sql
CODE:
```
create policy "Allow access to authenticated users" on profiles to authenticated for
select
  using (true);
```

----------------------------------------

TITLE: Creating Todos Table and RLS Policy (PostgreSQL)
DESCRIPTION: This SQL snippet defines a 'todos' table with UUIDs, text tasks, user references, and a boolean 'completed' status. It then enables Row Level Security (RLS) on the table and creates a policy that restricts authenticated users to only access and modify their own todo items.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/local-development/testing/overview.mdx#_snippet_0

LANGUAGE: SQL
CODE:
```
-- Create a simple todos table
create table todos (
id uuid primary key default gen_random_uuid(),
task text not null,
user_id uuid references auth.users not null,
completed boolean default false
);

-- Enable RLS
alter table todos enable row level security;

-- Create a policy
create policy "Users can only access their own todos"
on todos for all -- this policy applies to all operations
to authenticated
using ((select auth.uid()) = user_id);
```

----------------------------------------

TITLE: Initializing a Supabase Project Locally
DESCRIPTION: This command initializes a new Supabase project in the current directory, setting up the necessary local development environment files and configurations. It's the first step to begin working with Supabase locally.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/functions/examples/elevenlabs-generate-speech-stream.mdx#_snippet_0

LANGUAGE: bash
CODE:
```
supabase init
```

----------------------------------------

TITLE: Initializing Supabase Client in Next.js
DESCRIPTION: This TypeScript snippet creates and exports a Supabase client instance using the `createClient` function, leveraging environment variables for the Supabase URL and anonymous key. This client is used for all data interactions.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/www/_blog/2022-11-17-fetching-and-caching-supabase-data-in-next-js-server-components.mdx#_snippet_5

LANGUAGE: tsx
CODE:
```
import { createClient } from '@supabase/supabase-js'

export default createClient(
  process.env.NEXT_PUBLIC_SUPABASE_URL!,
  process.env.NEXT_PUBLIC_SUPABASE_ANON_KEY!
)
```

----------------------------------------

TITLE: Invoking Postgres Function with Supabase TypeScript Client
DESCRIPTION: This TypeScript snippet demonstrates how to call a PostgreSQL Database Function, `calculate_customer_discount`, using the Supabase client library's `.rpc()` method. It passes the `customer_id` as an object, leveraging auto-generated types for a type-safe invocation.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/www/_blog/2025-05-17-simplify-backend-with-data-api.mdx#_snippet_3

LANGUAGE: tsx
CODE:
```
const { data, error } = await supabase.rpc('calculate_customer_discount', {
  customer_id: 'uuid-of-customer',
})
```

----------------------------------------

TITLE: Initializing Supabase Client in React (JavaScript)
DESCRIPTION: This JavaScript snippet demonstrates how to initialize the Supabase client within a React application using `createClient` from `@supabase/supabase-js`. It configures the client with the local Supabase URL and the anonymous public key, enabling interaction with the Supabase backend services.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/www/_blog/2021-03-31-supabase-cli.mdx#_snippet_3

LANGUAGE: javascript
CODE:
```
import { createClient } from '@supabase/supabase-js'
const SUPABASE_URL = 'http://localhost:8000'
const SUPABASE_ANON_KEY =
  'eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJzdXBhYmFzZSIsImlhdCI6MTYwMzk2ODgzNCwiZXhwIjoyNTUwNjUzNjM0LCJyb2xlIjoiYW5vbiJ9.36fUebxgx1mcBo4s19v0SzqmzunP--hm_hep0uLX0ew'
const supabase = createClient(SUPABASE_URL, SUPABASE_ANON_KEY)
```

----------------------------------------

TITLE: Starting Local Supabase Services - Shell
DESCRIPTION: This command initializes and starts the local Supabase services, including the database and other necessary components. It is required to interact with Supabase locally for development and testing.
SOURCE: https://github.com/supabase/supabase/blob/master/examples/ai/aws_bedrock_image_search/README.md#_snippet_4

LANGUAGE: Shell
CODE:
```
supabase start
```

----------------------------------------

TITLE: Creating Triggers on Supabase Auth Users Table
DESCRIPTION: This SQL snippet defines three triggers on the `auth.users` table: `on_auth_user_created` (after insert), `on_auth_user_updated` (after update), and `on_auth_user_deleted` (after delete). These triggers execute the respective `public.handle_new_user`, `public.update_user`, and `public.delete_user` functions to synchronize user data with the `public.profiles` table.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/database/prisma/prisma-troubleshooting.mdx#_snippet_8

LANGUAGE: SQL
CODE:
```
-- Trigger to run 'handle_new_user' function after a new user is inserted into 'auth.users' table
create trigger on_auth_user_created
  after insert on auth.users
  for each row execute procedure public.handle_new_user();

-- Trigger to run 'update_user' function after a user is updated in the 'auth.users' table
create trigger on_auth_user_updated
  after update on auth.users
  for each row execute procedure public.update_user();

-- Trigger to run 'delete_user' function after a user is deleted from the 'auth.users' table
create trigger on_auth_user_deleted
  after delete on auth.users
  for each row execute procedure public.delete_user();
```

----------------------------------------

TITLE: Initializing Supabase Client (JavaScript)
DESCRIPTION: This JavaScript snippet demonstrates how to initialize the Supabase client using `createClient`. It requires your project URL and anonymous key to establish a connection for listening to Postgres changes.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/realtime/postgres-changes.mdx#_snippet_3

LANGUAGE: js
CODE:
```
import { createClient } from '@supabase/supabase-js'

const supabase = createClient(
  'https://<project>.supabase.co',
  '<your-anon-key>'
)
```

----------------------------------------

TITLE: Initializing Supabase Client in Svelte
DESCRIPTION: This TypeScript file (`src/supabaseClient.ts`) initializes and exports a Supabase client instance using the `createClient` function. It retrieves the Supabase URL and `anon` key from environment variables, establishing the connection for all Supabase interactions within the Svelte application.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/getting-started/tutorials/with-svelte.mdx#_snippet_4

LANGUAGE: typescript
CODE:
```
import { createClient } from '@supabase/supabase-js'

const supabaseUrl = import.meta.env.VITE_SUPABASE_URL
const supabaseAnonKey = import.meta.env.VITE_SUPABASE_ANON_KEY

export const supabase = createClient(supabaseUrl, supabaseAnonKey)
```

----------------------------------------

TITLE: Handling Supabase Authentication State in React Native App
DESCRIPTION: This TypeScript React Native component (`App.tsx`) serves as the main entry point, managing the user's authentication session with Supabase. It uses `useEffect` to listen for session changes and conditionally renders either the `Account` component (if a user is signed in) or the `Auth` component (for sign-in/sign-up). This ensures the correct UI is displayed based on the authentication state.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/getting-started/tutorials/with-expo-react-native.mdx#_snippet_6

LANGUAGE: tsx
CODE:
```
import { useState, useEffect } from 'react'
import { supabase } from './lib/supabase'
import Auth from './components/Auth'
import Account from './components/Account'
import { View } from 'react-native'
import { Session } from '@supabase/supabase-js'

export default function App() {
  const [session, setSession] = useState<Session | null>(null)

  useEffect(() => {
    supabase.auth.getSession().then(({ data: { session } }) => {
      setSession(session)
    })

    supabase.auth.onAuthStateChange((_event, session) => {
      setSession(session)
    })
  }, [])

  return (
    <View>
      {session && session.user ? <Account key={session.user.id} session={session} /> : <Auth />}
    </View>
  )
}
```

----------------------------------------

TITLE: Signing In with Email and Password - JavaScript
DESCRIPTION: This snippet demonstrates how to sign in a user using their email and password with the Supabase JavaScript client. It calls `signInWithPassword()` and handles the response, which includes user data or an error.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/auth/passwords.mdx#_snippet_16

LANGUAGE: JavaScript
CODE:
```
import { createClient } from '@supabase/supabase-js'
const supabase = createClient('https://your-project.supabase.co', 'your-anon-key')

// ---cut---
async function signInWithEmail() {
  const { data, error } = await supabase.auth.signInWithPassword({
    email: 'valid.email@supabase.io',
    password: 'example-password',
  })
}
```

----------------------------------------

TITLE: Implementing Custom Access Token Auth Hook in PL/pgSQL
DESCRIPTION: This PL/pgSQL function, `custom_access_token_hook`, is designed to run as a Supabase Auth Hook before a JWT is issued. It fetches the user's role from the `user_roles` table and injects it as a `user_role` custom claim into the JWT, enabling role-based access control. The script also includes necessary `GRANT` and `REVOKE` statements to manage permissions for the function and the `user_roles` table, ensuring secure execution within the Supabase environment.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/database/postgres/custom-claims-and-role-based-access-control-rbac.mdx#_snippet_3

LANGUAGE: plpgsql
CODE:
```
-- Create the auth hook function
create or replace function public.custom_access_token_hook(event jsonb)
returns jsonb
language plpgsql
stable
as $$
  declare
    claims jsonb;
    user_role public.app_role;
  begin
    -- Fetch the user role in the user_roles table
    select role into user_role from public.user_roles where user_id = (event->>'user_id')::uuid;

    claims := event->'claims';

    if user_role is not null then
      -- Set the claim
      claims := jsonb_set(claims, '{user_role}', to_jsonb(user_role));
    else
      claims := jsonb_set(claims, '{user_role}', 'null');
    end if;

    -- Update the 'claims' object in the original event
    event := jsonb_set(event, '{claims}', claims);

    -- Return the modified or original event
    return event;
  end;
$$;

grant usage on schema public to supabase_auth_admin;

grant execute
  on function public.custom_access_token_hook
  to supabase_auth_admin;

revoke execute
  on function public.custom_access_token_hook
  from authenticated, anon, public;

grant all
  on table public.user_roles
to supabase_auth_admin;

revoke all
  on table public.user_roles
  from authenticated, anon, public;

create policy "Allow auth admin to read user roles" ON public.user_roles
as permissive for select
to supabase_auth_admin
using (true)
```

----------------------------------------

TITLE: Main Svelte Application Layout
DESCRIPTION: This `App.svelte` component serves as the main entry point for the application, dynamically rendering either the `Auth` component (if no session exists) or the `Account` component (if a user is signed in). It listens for Supabase authentication state changes to manage the user session.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/getting-started/tutorials/with-svelte.mdx#_snippet_7

LANGUAGE: html
CODE:
```
<script lang="ts">
  import { onMount } from 'svelte'
  import { supabase } from './supabaseClient'
  import type { AuthSession } from '@supabase/supabase-js'
  import Account from './lib/Account.svelte'
  import Auth from './lib/Auth.svelte'

  let session: AuthSession | null

  onMount(() => {
    supabase.auth.getSession().then(({ data }) => {
      session = data.session
    })

    supabase.auth.onAuthStateChange((_event, _session) => {
      session = _session
    })
  })
</script>

<div class="container" style="padding: 50px 0 100px 0">
  {#if !session}
  <Auth />
  {:else}
  <Account {session} />
  {/if}
</div>
```

----------------------------------------

TITLE: Inefficient Policy Lacking Role Specification (SQL)
DESCRIPTION: This policy allows users to access their own records in `rls_test` but omits the `TO` clause for role specification. This can lead to the policy being evaluated for all users, including `anon`, even if `auth.uid()` would fail, thus impacting performance.
SOURCE: https://github.com/supabase/supabase/blob/master/examples/prompts/database-rls-policies.md#_snippet_13

LANGUAGE: SQL
CODE:
```
create policy "Users can access their own records" on rls_test
using ( auth.uid() = user_id );
```

----------------------------------------

TITLE: Fetching Data with Supabase Dart Client
DESCRIPTION: This Dart snippet demonstrates how to fetch all data from the 'todos' table using the Supabase client library. It performs a select('*') operation, returning all columns for the records in the table.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/api/quickstart.mdx#_snippet_5

LANGUAGE: Dart
CODE:
```
final data = await supabase.from('todos').select('*');
```

----------------------------------------

TITLE: Initializing Supabase Client in JavaScript
DESCRIPTION: This JavaScript code demonstrates how to import `createClient` from `@supabase/supabase-js` and initialize a Supabase client instance using a project URL and a public anonymous key. It then logs the created instance.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/www/_blog/2024-07-16-supabase-js-on-jsr.mdx#_snippet_3

LANGUAGE: js
CODE:
```
import { createClient } from '@supabase/supabase-js'

const supabase = createClient('https://xyzcompany.supabase.co', 'public-anon-key')

console.log('Supabase Instance: ', supabase)
```

----------------------------------------

TITLE: Configuring Supabase Environment Variables in .env
DESCRIPTION: This snippet shows how to set up environment variables in a `.env` file. These variables, `REACT_APP_SUPABASE_URL` and `REACT_APP_SUPABASE_ANON_KEY`, are crucial for connecting the Ionic React application to your Supabase project.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/getting-started/tutorials/with-ionic-react.mdx#_snippet_1

LANGUAGE: bash
CODE:
```
REACT_APP_SUPABASE_URL=YOUR_SUPABASE_URL
REACT_APP_SUPABASE_ANON_KEY=YOUR_SUPABASE_ANON_KEY
```

----------------------------------------

TITLE: Implementing Product Repository with Supabase in Kotlin
DESCRIPTION: This class provides the concrete implementation of the `ProductRepository` interface, utilizing Supabase's Postgrest and Storage clients. It includes methods for CRUD operations on products and handles image uploads, along with a utility for building image URLs.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/getting-started/tutorials/with-kotlin.mdx#_snippet_13

LANGUAGE: kotlin
CODE:
```
class ProductRepositoryImpl @Inject constructor(
    private val postgrest: Postgrest,
    private val storage: Storage,
) : ProductRepository {
    override suspend fun createProduct(product: Product): Boolean {
        return try {
            withContext(Dispatchers.IO) {
                val productDto = ProductDto(
                    name = product.name,
                    price = product.price,
                )
                postgrest.from("products").insert(productDto)
                true
            }
            true
        } catch (e: java.lang.Exception) {
            throw e
        }
    }

    override suspend fun getProducts(): List<ProductDto>? {
        return withContext(Dispatchers.IO) {
            val result = postgrest.from("products")
                .select().decodeList<ProductDto>()
            result
        }
    }


    override suspend fun getProduct(id: String): ProductDto {
        return withContext(Dispatchers.IO) {
            postgrest.from("products").select {
                filter {
                    eq("id", id)
                }
            }.decodeSingle<ProductDto>()
        }
    }

    override suspend fun deleteProduct(id: String) {
        return withContext(Dispatchers.IO) {
            postgrest.from("products").delete {
                filter {
                    eq("id", id)
                }
            }
        }
    }

    override suspend fun updateProduct(
        id: String,
        name: String,
        price: Double,
        imageName: String,
        imageFile: ByteArray
    ) {
        withContext(Dispatchers.IO) {
            if (imageFile.isNotEmpty()) {
                val imageUrl =
                    storage.from("Product%20Image").upload(
                        path = "$imageName.png",
                        data = imageFile,
                        upsert = true
                    )
                postgrest.from("products").update({
                    set("name", name)
                    set("price", price)
                    set("image", buildImageUrl(imageFileName = imageUrl))
                }) {
                    filter {
                        eq("id", id)
                    }
                }
            } else {
                postgrest.from("products").update({
                    set("name", name)
                    set("price", price)
                }) {
                    filter {
                        eq("id", id)
                    }
                }
            }
        }
    }

    // Because I named the bucket as "Product Image" so when it turns to an url, it is "%20"
    // For better approach, you should create your bucket name without space symbol
    private fun buildImageUrl(imageFileName: String) =
        "${BuildConfig.SUPABASE_URL}/storage/v1/object/public/${imageFileName}".replace(" ", "%20")
}
```

----------------------------------------

TITLE: Applying RLS Policies with `authorize` Function in PostgreSQL
DESCRIPTION: These SQL statements define RLS policies for `public.channels` and `public.messages` tables, allowing authenticated users to perform delete operations. The policies leverage the `public.authorize` function to ensure that the user's role has the specific `channels.delete` or `messages.delete` permission before allowing the action.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/database/postgres/custom-claims-and-role-based-access-control-rbac.mdx#_snippet_5

LANGUAGE: sql
CODE:
```
create policy "Allow authorized delete access" on public.channels for delete to authenticated using ( (SELECT authorize('channels.delete')) );
create policy "Allow authorized delete access" on public.messages for delete to authenticated using ( (SELECT authorize('messages.delete')) );
```

----------------------------------------

TITLE: Minimal Example: Indexing an Unindexed Column (SQL)
DESCRIPTION: Presents a complete minimal example demonstrating the setup of a basic table and the use of `index_advisor` to recommend an index for an unindexed column, showing the expected output with cost improvements.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/database/extensions/index_advisor.mdx#_snippet_3

LANGUAGE: SQL
CODE:
```
create extension if not exists index_advisor cascade;

create table book(
  id int primary key,
  title text not null
);

select
  *
from
  index_advisor('select book.id from book where title = $1');

 startup_cost_before | startup_cost_after | total_cost_before | total_cost_after |                  index_statements                   | errors
---------------------+--------------------+-------------------+------------------+-----------------------------------------------------+
 0.00                | 1.17               | 25.88             | 6.40             | {"CREATE INDEX ON public.book USING btree (title)"},| {}
(1 row)
```

----------------------------------------

TITLE: Restricting SQL INSERT Policy to Authenticated Users and Specific Bucket
DESCRIPTION: This policy refines the `INSERT` permission, allowing only authenticated users to upload objects. Additionally, it restricts uploads to a specific bucket identified by `my_bucket_id`, ensuring data segregation and access control.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/storage/security/access-control.mdx#_snippet_1

LANGUAGE: SQL
CODE:
```
create policy "policy_name"
on storage.objects for insert to authenticated with check (
    -- restrict bucket
    bucket_id = 'my_bucket_id'
);
```

----------------------------------------

TITLE: Finding All Errors in Supabase DB API Edge Logs (SQL)
DESCRIPTION: This SQL query retrieves all error logs (status code >= 400) specifically for the Supabase Database API (`/rest/v1/`). It extracts timestamp, status code, event message, and path, joining `edge_logs` with unnested `metadata`, `response`, and `request` fields.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/troubleshooting/discovering-and-interpreting-api-errors-in-the-logs-7xREI9.mdx#_snippet_9

LANGUAGE: SQL
CODE:
```
select
  cast(timestamp as datetime) as timestamp,
  status_code,
  event_message,
  path
from
  edge_logs
  cross join unnest(metadata) as metadata
  cross join unnest(response) as response
  cross join unnest(request) as request
where
  -- find all errors
  status_code >= 400
  and regexp_contains(path, '^/rest/v1/');
-- only look at DB API
```

----------------------------------------

TITLE: Creating Chat Application Tables in Postgres
DESCRIPTION: This SQL snippet defines two tables: `chats` for conversations and `chat_messages` for individual messages. The `chats` table stores conversation IDs and creation timestamps, while `chat_messages` stores message content, creation timestamps, and a foreign key reference to its parent chat. These tables simulate a large dataset for demonstrating partitioning.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/www/_blog/2023-10-03-postgres-dynamic-table-partitioning.mdx#_snippet_0

LANGUAGE: SQL
CODE:
```
create table chats (
  id bigserial,
  created_at timestamptz not null default now(),
  primary key (id)
);

create table chat_messages (
  id bigserial,
  created_at timestamptz not null,
  chat_id bigint not null,
  chat_created_at timestamptz not null,
  message text not null,
  primary key (id),
  foreign key (chat_id) references chats (id)
);
```

----------------------------------------

TITLE: Defining a Tool with JSON Schema for LLMs (JSON)
DESCRIPTION: This JSON schema illustrates how a tool, such as `get_weather`, is defined for Large Language Models (LLMs) to understand its capabilities and required parameters. It specifies the tool's name, a description of its function, and the structure of its input parameters, including their types and whether they are mandatory. This format is standard across most LLMs for tool calling.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/www/_blog/2025-04-04-mcp-server.mdx#_snippet_1

LANGUAGE: json
CODE:
```
{
  "name": "get_weather",
  "description": "Get the current weather for a specific location",
  "parameters": {
    "type": "object",
    "properties": {
      "location": {
        "type": "string",
        "description": "The city and state/country, e.g. 'San Francisco, CA' or 'Paris, France'"
      }
    },
    "required": ["location"]
  }
}
```

----------------------------------------

TITLE: Creating a PostgreSQL RLS Policy for User-Specific Select
DESCRIPTION: This SQL snippet defines a Row Level Security (RLS) policy named `todo_select_policy` on the `todos` table. It restricts `SELECT` operations, allowing users to only retrieve rows where their authenticated user ID (`auth.uid()`) matches the `user_id` column in the `todos` table. This ensures users can only view their own todo items.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/www/_blog/2021-12-01-realtime-row-level-security-in-postgresql.mdx#_snippet_0

LANGUAGE: SQL
CODE:
```
create policy todo_select_policy
    on todos for select
    using ( (select auth.uid()) = user_id );
```

----------------------------------------

TITLE: Querying `todos` Table with Supabase JavaScript Client
DESCRIPTION: This JavaScript snippet demonstrates how to initialize the Supabase client and perform a `SELECT` operation on the `todos` table. It requires `SUPABASE_URL` and `SUPABASE_ANON_KEY` for client initialization. The `select('*')` method retrieves all columns from the `todos` table, returning `data` (the query results) and `error` (if any).
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/api/creating-routes.mdx#_snippet_1

LANGUAGE: javascript
CODE:
```
// Initialize the JS client
import { createClient } from '@supabase/supabase-js'
const supabase = createClient(SUPABASE_URL, SUPABASE_ANON_KEY)

// Make a request
const { data: todos, error } = await supabase.from('todos').select('*')
```

----------------------------------------

TITLE: Implementing Row Level Security Policies - SQL
DESCRIPTION: This SQL snippet enables Row Level Security (RLS) for `boards`, `user_boards`, `lists`, and `cards` tables. It defines policies to control data access, ensuring users can only create, view, update, or delete records they are authorized for, often leveraging the `get_boards_for_authenticated_user` function.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/www/_blog/2022-08-24-building-a-realtime-trello-board-with-supabase-and-angular.mdx#_snippet_1

LANGUAGE: SQL
CODE:
```
-- boards row level security
alter table boards enable row level security;

-- Policies
create policy "Users can create boards" on boards for
  insert to authenticated with CHECK (true);

create policy "Users can view their boards" on boards for
    select using (
      id in (
        select get_boards_for_authenticated_user()
      )
    );

create policy "Users can update their boards" on boards for
    update using (
      id in (
        select get_boards_for_authenticated_user()
      )
    );

create policy "Users can delete their created boards" on boards for
    delete using ((select auth.uid()) = creator);

-- user_boards row level security
alter table user_boards enable row level security;

create policy "Users can add their boards" on user_boards for
    insert to authenticated with check (true);

create policy "Users can view boards" on user_boards for
    select using ((select auth.uid()) = user_id);

create policy "Users can delete their boards" on user_boards for
    delete using ((select auth.uid()) = user_id);

-- lists row level security
alter table lists enable row level security;

-- Policies
create policy "Users can edit lists if they are part of the board" on lists for
    all using (
      board_id in (
        select get_boards_for_authenticated_user()
      )
    );

-- cards row level security
alter table cards enable row level security;

-- Policies
create policy "Users can edit cards if they are part of the board" on cards for
    all using (
      board_id in (
        select get_boards_for_authenticated_user()
      )
    );
```

----------------------------------------

TITLE: Starting Next.js Development Server - Bash
DESCRIPTION: Execute this command in your terminal from the project root to start the Next.js development server. The application will then be accessible in your browser, typically at `http://localhost:3000`, allowing you to test the setup.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/auth/quickstarts/nextjs.mdx#_snippet_3

LANGUAGE: bash
CODE:
```
npm run dev
```

----------------------------------------

TITLE: Postgres Function for Semantic Search with pgvector in SQL
DESCRIPTION: This PL/pgSQL function, `match_page_sections`, performs a similarity search on the `documents` table using a provided embedding. It allows filtering by source, author, document ID, and creation date, returning the most relevant document chunks based on the inner product distance, which is suitable for normalized OpenAI embeddings.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/www/_blog/2023-05-25-chatgpt-plugins-support-postgres.mdx#_snippet_1

LANGUAGE: SQL
CODE:
```
create or replace function match_page_sections(
	in_embedding vector(1536),
	in_match_count int default 3,
	in_document_id text default '%%',
	in_source_id text default '%%',
	in_source text default '%%',
	in_author text default '%%',
	in_start_date timestamptz default '-infinity',
	in_end_date timestamptz default 'infinity'
)
returns table (
	id text,
	source text,
	source_id text,
	document_id text,
	url text,
	created_at timestamptz,
	author text,
	content text,
	embedding vector(1536),
	similarity float
)
language plpgsql
as $$
#variable_conflict use_variable
begin
return query

select
	documents.id,
	documents.source,
	documents.source_id,
	documents.document_id,
	documents.url,
	documents.created_at,
	documents.author,
	documents.content,
	documents.embedding,
	(documents.embedding <#> in_embedding) * -1 as similarity
from
	documents
where
	in_start_date <= documents.created_at and
  documents.created_at <= in_end_date and
  (documents.source_id like in_source_id or documents.source_id is null) and
  (documents.source like in_source or documents.source is null) and
  (documents.author like in_author or documents.author is null) and
  (documents.document_id like in_document_id or documents.document_id is null)
order by
	documents.embedding <#> in_embedding
limit
	in_match_count;
end;
$$;
```

----------------------------------------

TITLE: Signing Out a User with Supabase
DESCRIPTION: This snippet demonstrates how to sign out a user using the Supabase client library. Calling the `signOut` method removes the active session and clears authentication data from the storage medium.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/auth/signout.mdx#_snippet_0

LANGUAGE: JavaScript
CODE:
```
import { createClient } from '@supabase/supabase-js'
const supabase = createClient('https://your-project-id.supabase.co', 'your-anon-key')

async function signOut() {
  const { error } = await supabase.auth.signOut()
}
```

LANGUAGE: Dart
CODE:
```
Future<void> signOut() async {
   await supabase.auth.signOut();
}
```

LANGUAGE: Swift
CODE:
```
try await supabase.auth.signOut()
```

LANGUAGE: Kotlin
CODE:
```
suspend fun logout() {
	supabase.auth.signOut()
}
```

LANGUAGE: Python
CODE:
```
supabase.auth.sign_out()
```

----------------------------------------

TITLE: Consuming Server-Prefetched Data in Next.js Client Components
DESCRIPTION: This client component demonstrates how to consume data that has been prefetched on the server. It uses `useQuery` from `@supabase-cache-helpers/postgrest-react-query` to access the `country` data. Because the query's cache key matches the server-side prefetched data, the data is immediately available upon render, eliminating any loading state.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/www/_blog/2024-01-12-react-query-nextjs-app-router-cache-helpers.mdx#_snippet_10

LANGUAGE: tsx
CODE:
```
'use client'

import useSupabaseBrowser from '@/utils/supabase-browser'
import { getCountryById } from '@/queries/get-country-by-id'
import { useQuery } from '@supabase-cache-helpers/postgrest-react-query'

export default function Country({ id }: { id: number }) {
  const supabase = useSupabaseBrowser()
  // This useQuery could just as well happen in some deeper
  // child to <Posts>, data will be available immediately either way
  const { data: country } = useQuery(getCountryById(supabase, id))

  return (
    <div>
      <h1>SSR: {country?.name}</h1>
    </div>
  )
}
```

----------------------------------------

TITLE: Re-routing API Requests with Supabase Edge Runtime (JavaScript)
DESCRIPTION: This example illustrates how Supabase Edge Runtime can act as an API Gateway to re-route incoming requests. It checks if the request URL ends with '/rest/v1/old_table' and, if so, forwards the request to a new endpoint 'http://rest:3000/rest/v1/new_table', preserving headers, method, and body. This allows for flexible request pre-processing and routing logic.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/www/_blog/2023-04-11-edge-runtime-self-hosted-deno-functions.mdx#_snippet_1

LANGUAGE: JavaScript
CODE:
```
serve(async (req) => {
  try {
    if (req.url.endsWith('/rest/v1/old_table')) {
      return await fetch('http://rest:3000/rest/v1/new_table', {
        headers: req.headers,
        method: req.method,
        body: req.body
      })
    }
  } catch (e) {
    const error = { msg: e.toString() }
    return new Response(JSON.stringify(error), {
      status: 500,
      headers: { 'Content-Type': 'application/json' }
    })
  }
})
```

----------------------------------------

TITLE: Creating a Postgres Function for Embeddings (PL/pgSQL)
DESCRIPTION: This PL/pgSQL function, `edge.generate_embedding`, encapsulates the logic for generating text embeddings. It takes `input_text` as a parameter, formats it into a JavaScript string, and executes it via `edge.exec` to interact with the Supabase AI session. The function then extracts and returns the embedding data from the response.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/www/_blog/2024-11-13-supabase-dynamic-functions.mdx#_snippet_12

LANGUAGE: PL/pgSQL
CODE:
```
CREATE OR REPLACE FUNCTION edge.generate_embedding(input_text TEXT) RETURNS JSONB AS $$
DECLARE
    response JSONB;
BEGIN
    -- Call the edge function to generate the embedding for the provided text
    response := edge.exec(
        format(
            $js$
            const session = new Supabase.ai.Session('gte-small');
            return await session.run(%L);
            $js$,
            input_text
        )
    );
    RETURN response->'response'->'data';
END;
$$ LANGUAGE plpgsql;
```

----------------------------------------

TITLE: Defining PostGIS Database Functions for Geo-Queries in Supabase
DESCRIPTION: This SQL snippet defines two PostGIS database functions: `nearby_stores` and `stores_in_view`. `nearby_stores` calculates and returns stores ordered by their distance from a given latitude/longitude. `stores_in_view` finds stores within a specified bounding box, leveraging PostGIS functions like `ST_MakeBox2D` and `ST_SetSRID` for efficient spatial filtering.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/www/_blog/2023-03-01-geo-queries-with-postgis-in-ionic-angular.mdx#_snippet_3

LANGUAGE: sql
CODE:
```
-- create database function to find nearby stores
create or replace function nearby_stores(lat float, long float)
returns table (id public.stores.id%TYPE, name public.stores.name%TYPE, description public.stores.description%TYPE, lat float, long float, dist_meters float)
language sql
as $$
  select id, name, description, st_y(location::geometry) as lat, st_x(location::geometry) as long, st_distance(location, st_point(long, lat)::geography) as dist_meters
  from public.stores
  order by location <-> st_point(long, lat)::geography;
$$;


-- create database function to find stores in a specific box
create or replace function stores_in_view(min_lat float, min_long float, max_lat float, max_long float)
returns table (id public.stores.id%TYPE, name public.stores.name%TYPE, lat float, long float)
language sql
as $$
	select id, name, ST_Y(location::geometry) as lat, ST_X(location::geometry) as long
	from public.stores
	where location && ST_SetSRID(ST_MakeBox2D(ST_Point(min_long, min_lat), ST_Point(max_long, max_lat)),4326)
$$;
```

----------------------------------------

TITLE: Configuring Supabase Postgres Row Level Security
DESCRIPTION: This SQL script defines a `profiles` table, enables Row Level Security (RLS), and sets up policies for public viewing, user-specific insertion, and user-specific updates. It also configures Supabase Realtime for the `profiles` table and sets up a storage bucket for avatars with public access and upload policies.
SOURCE: https://github.com/supabase/supabase/blob/master/examples/user-management/angular-user-management/README.md#_snippet_6

LANGUAGE: SQL
CODE:
```
-- Create a table for Public Profiles
create table profiles (
  id uuid references auth.users not null,
  updated_at timestamp with time zone,
  username text unique,
  avatar_url text,
  website text,
  primary key (id),
  unique(username),
  constraint username_length check (char_length(username) >= 3)
);
alter table profiles enable row level security;
create policy "Public profiles are viewable by everyone."
  on profiles for select
  using ( true );
create policy "Users can insert their own profile."
  on profiles for insert
  with check ( (select auth.uid()) = id );
create policy "Users can update own profile."
  on profiles for update
  using ( (select auth.uid()) = id );
-- Set up Realtime!
begin;
  drop publication if exists supabase_realtime;
  create publication supabase_realtime;
commit;
alter publication supabase_realtime add table profiles;
-- Set up Storage!
insert into storage.buckets (id, name)
values ('avatars', 'avatars');
create policy "Avatar images are publicly accessible."
  on storage.objects for select
  using ( bucket_id = 'avatars' );
create policy "Anyone can upload an avatar."
  on storage.objects for insert
  with check ( bucket_id = 'avatars' );
```

----------------------------------------

TITLE: Initializing Next.js with Supabase SSR
DESCRIPTION: This command initializes a new Next.js project pre-configured with Supabase Server-Side Rendering (SSR) support using `@supabase/ssr`, ensuring compatibility with Next.js 14. It sets up the necessary dependencies and project structure for integrating Supabase authentication and data access.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/www/_blog/2023-11-06-beta-update-october-2023.mdx#_snippet_0

LANGUAGE: Shell
CODE:
```
npx create-next-app -e with-supabase
```

----------------------------------------

TITLE: Enforcing MFA with Supabase Row Level Security Policy
DESCRIPTION: This SQL Row Level Security (RLS) policy enforces Multi-Factor Authentication (MFA) for all authenticated users accessing a specific table. It uses a `restrictive` policy to ensure that only sessions with an Authenticator Assurance Level (AAL) of `aal2` (indicating MFA verification) are granted access. This policy should be applied to tables where access requires a higher level of identity assurance.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/auth/auth-mfa.mdx#_snippet_2

LANGUAGE: SQL
CODE:
```
create policy "Policy name."
  on table_name
  as restrictive
  to authenticated
  using ((select auth.jwt()->>'aal') = 'aal2');
```

----------------------------------------

TITLE: Injecting Supabase TypeScript Types into Client
DESCRIPTION: This TypeScript snippet imports the Database types generated by the Supabase CLI and injects them into the createClient function. This enables type inference and safety for all Supabase queries and operations within the application, ensuring data consistency and reducing errors.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/www/_blog/2024-09-23-local-first-expo-legend-state.mdx#_snippet_8

LANGUAGE: typescript
CODE:
```
import { createClient } from '@supabase/supabase-js'
import { Database } from './database.types'
// [...]

const supabase = createClient<Database>(
  process.env.EXPO_PUBLIC_SUPABASE_URL,
  process.env.EXPO_PUBLIC_SUPABASE_ANON_KEY
)

// [...]
```

----------------------------------------

TITLE: Initializing Supabase Client in TypeScript
DESCRIPTION: This TypeScript code initializes the Supabase client using the createClient function from @supabase/supabase-js. It retrieves the Supabase URL and anonymous key from environment variables, establishing the connection to the Supabase project.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/www/_blog/2024-09-23-local-first-expo-legend-state.mdx#_snippet_3

LANGUAGE: typescript
CODE:
```
import { createClient } from '@supabase/supabase-js'

const supabase = createClient(
  process.env.EXPO_PUBLIC_SUPABASE_URL,
  process.env.EXPO_PUBLIC_SUPABASE_ANON_KEY
)
```

----------------------------------------

TITLE: Generated TypeScript Types for `public.movies` Table
DESCRIPTION: This TypeScript interface shows the structure of types automatically generated by Supabase for the `public.movies` table. It includes `Row`, `Insert`, and `Update` types, reflecting database constraints like `not null` and generated columns.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/docs/ref/javascript/typescript-support.mdx#_snippet_2

LANGUAGE: ts
CODE:
```
export type Json = string | number | boolean | null | { [key: string]: Json | undefined } | Json[]

export interface Database {
  public: {
    Tables: {
      movies: {
        Row: {               // the data expected from .select()
          id: number
          name: string
          data: Json | null
        }
        Insert: {            // the data to be passed to .insert()
          id?: never         // generated columns must not be supplied
          name: string       // `not null` columns with no default must be supplied
          data?: Json | null // nullable columns can be omitted
        }
        Update: {            // the data to be passed to .update()
          id?: never
          name?: string      // `not null` columns are optional on .update()
          data?: Json | null
        }
      }
    }
  }
}
```

----------------------------------------

TITLE: Generating and Storing OpenAI Embeddings with JavaScript
DESCRIPTION: This asynchronous JavaScript function demonstrates how to generate text embeddings using the OpenAI API and store them in a Supabase PostgreSQL database. It initializes the OpenAI client, fetches documents (via a placeholder `getDocuments` function), processes each document by replacing newlines, generates a 1536-dimensional embedding using `text-embedding-ada-002`, and then inserts the original content and its embedding into the `documents` table using the Supabase client. It requires `openai` and `@supabase/supabase-js` libraries.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/www/_blog/2023-02-03-openai-embeddings-postgres-vector.mdx#_snippet_4

LANGUAGE: JavaScript
CODE:
```
import { createClient } from '@supabase/supabase-js'
import { Configuration, OpenAIApi } from 'openai'
import { supabaseClient } from './lib/supabase'

async function generateEmbeddings() {
  const configuration = new Configuration({ apiKey: '<YOUR_OPENAI_API_KEY>' })
  const openAi = new OpenAIApi(configuration)

  const documents = await getDocuments() // Your custom function to load docs

  // Assuming each document is a string
  for (const document of documents) {
    // OpenAI recommends replacing newlines with spaces for best results
    const input = document.replace(/\n/g, ' ')

    const embeddingResponse = await openAi.createEmbedding({
      model: 'text-embedding-ada-002',
      input,
    })

    const [{ embedding }] = embeddingResponse.data.data

    // In production we should handle possible errors
    await supabaseClient.from('documents').insert({
      content: document,
      embedding,
    })
  }
}
```

----------------------------------------

TITLE: GitHub Actions Production Deployment Workflow (YAML)
DESCRIPTION: Sets up a GitHub Actions workflow for deploying Supabase database migrations to a production environment. It activates on pushes to the `main` branch or manual dispatch, securely linking the project and pushing schema changes using Supabase CLI secrets.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/deployment/managing-environments.mdx#_snippet_13

LANGUAGE: yaml
CODE:
```
name: Deploy Migrations to Production

on:
  push:
    branches:
      - main
  workflow_dispatch:

jobs:
  deploy:
    runs-on: ubuntu-latest

    env:
      SUPABASE_ACCESS_TOKEN: ${{ secrets.SUPABASE_ACCESS_TOKEN }}
      SUPABASE_DB_PASSWORD: ${{ secrets.PRODUCTION_DB_PASSWORD }}
      SUPABASE_PROJECT_ID: ${{ secrets.PRODUCTION_PROJECT_ID }}

    steps:
      - uses: actions/checkout@v4

      - uses: supabase/setup-cli@v1
        with:
          version: latest

      - run: supabase link --project-ref $SUPABASE_PROJECT_ID
      - run: supabase db push
```

----------------------------------------

TITLE: Listening to Specific Table Changes with Supabase Realtime
DESCRIPTION: Shows how to subscribe to all real-time events (*) for a particular table (e.g., todos) within the public schema using Supabase Realtime. This allows monitoring inserts, updates, and deletes on the specified table.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/realtime/postgres-changes.mdx#_snippet_18

LANGUAGE: JavaScript
CODE:
```
const changes = supabase
  .channel('table-db-changes')
  .on(
    'postgres_changes',
    {
      event: '*',
      schema: 'public',
      table: 'todos',
    },
    (payload) => console.log(payload)
  )
  .subscribe()
```

LANGUAGE: Dart
CODE:
```
supabase
    .channel('table-db-changes')
    .onPostgresChanges(
        event: PostgresChangeEvent.all,
        schema: 'public',
        table: 'todos',
        callback: (payload) => print(payload))
    .subscribe();
```

LANGUAGE: Swift
CODE:
```
let myChannel = await supabase.channel("db-changes")

let changes = await myChannel.postgresChange(AnyAction.self, schema: "public", table: "todos")

await myChannel.subscribe()

for await change in changes {
  switch change {
  case .insert(let action): print(action)
  case .update(let action): print(action)
  case .delete(let action): print(action)
  case .select(let action): print(action)
  }
}
```

LANGUAGE: Kotlin
CODE:
```
val myChannel = supabase.channel("db-changes")

val changes = myChannel.postgresChangeFlow<PostgresAction>(schema = "public") {
    table = "todos"
}

changes
    .onEach {
        println(it.record)
    }
    .launchIn(yourCoroutineScope)

myChannel.subscribe()
```

LANGUAGE: Python
CODE:
```
changes = supabase.channel('db-changes').on_postgres_changes(
  "UPDATE",
  schema="public",
  table="todos",
  callback=lambda payload: print(payload)
)
.subscribe()
```

----------------------------------------

TITLE: Creating Supabase Server Client in Next.js (JavaScript)
DESCRIPTION: This JavaScript utility function `createClient` initializes a Supabase client for use in Server Components, Server Actions, and Route Handlers. It uses `createServerClient` from `@supabase/ssr` and integrates with Next.js `cookies` to manage user sessions, ensuring secure server-side authentication.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/getting-started/tutorials/with-nextjs.mdx#_snippet_6

LANGUAGE: javascript
CODE:
```
import { createServerClient } from '@supabase/ssr'
import { cookies } from 'next/headers'

export async function createClient() {
  const cookieStore = await cookies()

  // Create a server's supabase client with newly configured cookie,
  // which could be used to maintain user's session
  return createServerClient(
    process.env.NEXT_PUBLIC_SUPABASE_URL,
    process.env.NEXT_PUBLIC_SUPABASE_ANON_KEY,
    {
      cookies: {
        getAll() {
          return cookieStore.getAll()
        },
        setAll(cookiesToSet) {
          try {
            cookiesToSet.forEach(({ name, value, options }) =>
              cookieStore.set(name, value, options)
            )
          } catch {
            // The `setAll` method was called from a Server Component.
            // This can be ignored if you have middleware refreshing
            // user sessions.
          }
        }
      }
    }
  )
}
```

----------------------------------------

TITLE: Building a Float16 HNSW Index (SQL)
DESCRIPTION: This SQL command creates an HNSW (Hierarchical Navigable Small World) index on the `vector` column of the `embedding_half` table. It specifically uses `halfvec_l2_ops` to ensure the index operates efficiently with float16 vectors, further optimizing performance and memory usage for approximate nearest neighbor searches.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/www/_blog/2024-05-01-pgvector-0-7-0.mdx#_snippet_1

LANGUAGE: SQL
CODE:
```
create index on embedding_half using hnsw (vector halfvec_l2_ops);
```

----------------------------------------

TITLE: Configuring Supabase Environment Variables
DESCRIPTION: This snippet shows the essential environment variables required to connect your React Router application to a Supabase project. VITE_SUPABASE_URL specifies the URL of your Supabase instance, and VITE_SUPABASE_ANON_KEY is your project's public API key, used for client-side interactions.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/ui-library/content/docs/react-router/social-auth.mdx#_snippet_0

LANGUAGE: env
CODE:
```
VITE_SUPABASE_URL=
VITE_SUPABASE_ANON_KEY=
```

----------------------------------------

TITLE: Configuring Supabase Environment Variables
DESCRIPTION: This snippet shows how to set up local environment variables for connecting a Vite application to a Supabase project. It requires the Supabase project URL and the anonymous (anon) key, which are essential for client-side interaction with the Supabase API.
SOURCE: https://github.com/supabase/supabase/blob/master/examples/user-management/react-user-management/README.md#_snippet_0

LANGUAGE: dotenv
CODE:
```
VITE_SUPABASE_URL=
VITE_SUPABASE_ANON_KEY=
```

----------------------------------------

TITLE: Creating RLS Policy for Owner-Based Object Deletion (SQL)
DESCRIPTION: This SQL policy, applied to `storage.objects`, enables Row Level Security to restrict object deletion. It ensures that only authenticated users who are the `owner_id` of a specific object can delete it, by comparing the object's `owner_id` with the current user's `auth.uid()`. This enforces ownership-based access control.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/storage/security/ownership.mdx#_snippet_0

LANGUAGE: SQL
CODE:
```
create policy "User can delete their own objects"
on storage.objects
for delete
to authenticated
using (
    owner_id = (select auth.uid())
);
```

----------------------------------------

TITLE: Defining GitHub Environment Variables in .env (Bash)
DESCRIPTION: This Bash snippet illustrates how to define GitHub client ID and secret as environment variables within a `.env` file. These variables are intended to be referenced in the `config.toml` file for secure local development and should not be committed to version control.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/local-development/managing-config.mdx#_snippet_1

LANGUAGE: bash
CODE:
```
GITHUB_CLIENT_ID=""
GITHUB_SECRET=""
```

----------------------------------------

TITLE: Defining Update Policy for User Profiles - Supabase SQL
DESCRIPTION: This RLS policy allows users to update their own profile information. The 'using' clause restricts updates to profiles where the 'id' matches the authenticated user's ID (auth.uid()).
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/_partials/user_management_quickstart_sql_template.mdx#_snippet_3

LANGUAGE: SQL
CODE:
```
create policy "Users can update own profile." on profiles
  for update using ((select auth.uid()) = id);
```

----------------------------------------

TITLE: Granting Usage Permissions on Custom API Schema (SQL)
DESCRIPTION: This SQL command grants the 'anon' and 'authenticated' roles usage permissions on the newly created 'api' schema. This allows these roles to access objects within the schema, which is a prerequisite for them to interact with data exposed through the Supabase Data API.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/database/hardening-data-api.mdx#_snippet_1

LANGUAGE: SQL
CODE:
```
grant usage on schema api to anon, authenticated;
```

----------------------------------------

TITLE: Loading Protected Page Data with `withAuth` (0.7.x TypeScript)
DESCRIPTION: This TypeScript snippet for SvelteKit 0.7.x shows how to load data for a protected page using the withAuth helper. It redirects unauthenticated users and fetches data from a Supabase table using getSupabaseClient(), returning tableData and user for the page.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/auth/auth-helpers/sveltekit.mdx#_snippet_45

LANGUAGE: TypeScript
CODE:
```
import { withAuth } from '@supabase/auth-helpers-sveltekit'
import { redirect } from '@sveltejs/kit'
import type { PageLoad } from './$types'

export const load: PageLoad = withAuth(async ({ session, getSupabaseClient }) => {
  if (!session.user) {
    redirect(303, '/')
  }

  const { data: tableData } = await getSupabaseClient().from('test').select('*')
  return { tableData, user: session.user }
})
```

----------------------------------------

TITLE: Allowing Public Downloads using storage.filename() in SQL
DESCRIPTION: This policy demonstrates how to use `storage.filename()` to restrict file access. It allows any user to download a specific file named 'favicon.ico' by checking the file's name against the desired value in a Row Level Security policy for `storage.objects`.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/storage/schema/helper-functions.mdx#_snippet_0

LANGUAGE: SQL
CODE:
```
create policy "Allow public downloads"
on storage.objects
for select
to public
using (
  storage.filename(name) = 'favicon.ico'
);
```

----------------------------------------

TITLE: Performing Vector Similarity Search with Postgres Function - SQL
DESCRIPTION: This PL/pgSQL function, `query_embeddings`, performs a vector similarity search on the `embeddings` table. It takes a query `embedding` and a `match_threshold` as input, returning rows where the inner product similarity is above the threshold. The function uses the `pgvector` extension's inner product operator (`<#>`) for efficient similarity comparison and orders results by similarity.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/functions/examples/semantic-search.mdx#_snippet_2

LANGUAGE: SQL
CODE:
```
create or replace function query_embeddings(embedding vector(384), match_threshold float)
returns setof embeddings
language plpgsql
as $$
begin
  return query
  select *
  from embeddings

  -- The inner product is negative, so we negate match_threshold
  where embeddings.embedding <#> embedding < -match_threshold

  -- Our embeddings are normalized to length 1, so cosine similarity
  -- and inner product will produce the same query results.
  -- Using inner product which can be computed faster.
  --
  -- For the different distance functions, see https://github.com/pgvector/pgvector
  order by embeddings.embedding <#> embedding;
end;
$$;
```

----------------------------------------

TITLE: Efficient Policy with Explicit Role Specification (SQL)
DESCRIPTION: This policy efficiently grants authenticated users access to their own records in `rls_test`. By explicitly specifying `TO authenticated`, the policy evaluation is skipped for unauthenticated users, improving performance and clarity.
SOURCE: https://github.com/supabase/supabase/blob/master/examples/prompts/database-rls-policies.md#_snippet_14

LANGUAGE: SQL
CODE:
```
create policy "Users can access their own records" on rls_test
to authenticated
using ( (select auth.uid()) = user_id );
```

----------------------------------------

TITLE: Enabling Postgres Extensions for Automatic Embeddings - SQL
DESCRIPTION: This SQL snippet enables the necessary PostgreSQL extensions for automating vector embedding generation and management. It includes `vector` for vector operations, `pgmq` for job queueing and processing, `pg_net` for asynchronous HTTP requests, `pg_cron` for scheduled processing and retries, and `hstore` for clearing embeddings during updates.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/ai/automatic-embeddings.mdx#_snippet_0

LANGUAGE: SQL
CODE:
```
-- For vector operations
create extension if not exists vector
with
  schema extensions;

-- For queueing and processing jobs
-- (pgmq will create its own schema)
create extension if not exists pgmq;

-- For async HTTP requests
create extension if not exists pg_net
with
  schema extensions;

-- For scheduled processing and retries
-- (pg_cron will create its own schema)
create extension if not exists pg_cron;

-- For clearing embeddings during updates
create extension if not exists hstore
with
  schema extensions;
```

----------------------------------------

TITLE: Implementing Supabase Authentication in Next.js Client Component (TypeScript)
DESCRIPTION: This client-side React component, written in TypeScript, demonstrates user authentication flows (sign up, sign in, sign out) using Supabase. It utilizes `createClientComponentClient` with database types for enhanced type safety and `next/navigation` to refresh the router after authentication actions. It requires user email and password inputs.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/auth/auth-helpers/nextjs.mdx#_snippet_7

LANGUAGE: tsx
CODE:
```
'use client'

import { createClientComponentClient } from '@supabase/auth-helpers-nextjs'
import { useRouter } from 'next/navigation'
import { useState } from 'react'

import type { Database } from '@/lib/database.types'

export default function Login() {
  const [email, setEmail] = useState('')
  const [password, setPassword] = useState('')
  const router = useRouter()
  const supabase = createClientComponentClient<Database>()

  const handleSignUp = async () => {
    await supabase.auth.signUp({
      email,
      password,
      options: {
        emailRedirectTo: `${location.origin}/auth/callback`,
      },
    })
    router.refresh()
  }

  const handleSignIn = async () => {
    await supabase.auth.signInWithPassword({
      email,
      password,
    })
    router.refresh()
  }

  const handleSignOut = async () => {
    await supabase.auth.signOut()
    router.refresh()
  }

  return (
    <>
      <input name="email" onChange={(e) => setEmail(e.target.value)} value={email} />
      <input
        type="password"
        name="password"
        onChange={(e) => setPassword(e.target.value)}
        value={password}
      />
      <button onClick={handleSignUp}>Sign up</button>
      <button onClick={handleSignIn}>Sign in</button>
      <button onClick={handleSignOut}>Sign out</button>
    </>
  )
}
```

----------------------------------------

TITLE: Fetching Data with Supabase in Next.js Client Components
DESCRIPTION: This example illustrates how to fetch data from Supabase within a Next.js Client Component using `createClientComponentClient`. It utilizes React's `useEffect` and `useState` hooks to manage the asynchronous data fetching and display of 'todos' data, demonstrating client-side data interaction. The TypeScript version includes type definitions for enhanced type safety.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/auth/auth-helpers/nextjs.mdx#_snippet_15

LANGUAGE: jsx
CODE:
```
'use client'

import { createClientComponentClient } from '@supabase/auth-helpers-nextjs'
import { useEffect, useState } from 'react'

export default function Page() {
  const [todos, setTodos] = useState()
  const supabase = createClientComponentClient()

  useEffect(() => {
    const getData = async () => {
      const { data } = await supabase.from('todos').select()
      setTodos(data)
    }

    getData()
  }, [])

  return todos ? <pre>{JSON.stringify(todos, null, 2)}</pre> : <p>Loading todos...</p>
}
```

LANGUAGE: tsx
CODE:
```
'use client'

import { createClientComponentClient } from '@supabase/auth-helpers-nextjs'
import { useEffect, useState } from 'react'

import type { Database } from '@/lib/database.types'

type Todo = Database['public']['Tables']['todos']['Row']

export default function Page() {
  const [todos, setTodos] = useState<Todo[] | null>(null)
  const supabase = createClientComponentClient<Database>()

  useEffect(() => {
    const getData = async () => {
      const { data } = await supabase.from('todos').select()
      setTodos(data)
    }

    getData()
  }, [])

  return todos ? <pre>{JSON.stringify(todos, null, 2)}</pre> : <p>Loading todos...</p>
}
```

----------------------------------------

TITLE: Implementing Auto-Updating `updated_at` Column with PL/pgSQL Function and Trigger
DESCRIPTION: This snippet defines a PL/pgSQL function `set_updated_at` that sets the `updated_at` column of a new or updated row to the current timestamp. It then creates a `BEFORE UPDATE` trigger `handle_updated_at` on the `students` table, which automatically executes this function for each row before an update, ensuring the `updated_at` timestamp is always current.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/www/_blog/2021-02-27-cracking-postgres-interview.mdx#_snippet_5

LANGUAGE: PL/pgSQL
CODE:
```
CREATE FUNCTION set_updated_at()
RETURNS TRIGGER AS $$
BEGIN
  new.updated_at = now();
  RETURN NEW;
END;
$$ LANGUAGE plpgsql;
```

LANGUAGE: SQL
CODE:
```
CREATE TRIGGER handle_updated_at
  BEFORE UPDATE ON students
  FOR EACH ROW
  EXECUTE PROCEDURE set_updated_at();
```

----------------------------------------

TITLE: Listening to Postgres Changes with Supabase Dart Realtime
DESCRIPTION: This section introduces the new `.onPostgresChanges()` method for listening to database changes, replacing the generic `.on()` method. V2 provides strongly typed filters and a `PostgresChangePayload` object for callbacks, offering `oldRecord` and `newRecord` properties for improved type safety and data access compared to the dynamic payload in v1.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/docs/ref/dart/upgrade-guide.mdx#_snippet_14

LANGUAGE: Dart
CODE:
```
supabase.channel('my_channel').on(
  RealtimeListenTypes.postgresChanges,
  ChannelFilter(
    event: '*',
    schema: 'public',
    table: 'messages',
    filter: 'room_id=eq.200',
  ),
  (dynamic payload, [ref]) {
    final Map<String, dynamic> newRecord = payload['new'];
    final Map<String, dynamic> oldRecord = payload['old'];
  },
).subscribe();
```

LANGUAGE: Dart
CODE:
```
supabase.channel('my_channel')
  .onPostgresChanges(
    event: PostgresChangeEvent.all,
    schema: 'public',
    table: 'messages',
    filter: PostgresChangeFilter(
      type: PostgresChangeFilterType.eq,
      column: 'room_id',
      value: 200,
    ),
    callback: (PostgresChangePayload payload) {
      final Map<String, dynamic> newRecord = payload.newRecord;
      final Map<String, dynamic> oldRecord = payload.oldRecord;
    })
  .subscribe();
```

----------------------------------------

TITLE: Supabase Server Client in SvelteKit Hooks
DESCRIPTION: This SvelteKit server hook initializes the Supabase server client, integrating with SvelteKit's cookie API. It also provides a `safeGetSession` utility to validate the JWT and retrieve the user session, ensuring secure server-side authentication.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/auth/server-side/creating-a-client.mdx#_snippet_16

LANGUAGE: TypeScript
CODE:
```
import { PUBLIC_SUPABASE_URL, PUBLIC_SUPABASE_ANON_KEY } from '$env/static/public'
import { createServerClient } from '@supabase/ssr'
import type { Handle } from '@sveltejs/kit'

export const handle: Handle = async ({ event, resolve }) => {
  event.locals.supabase = createServerClient(PUBLIC_SUPABASE_URL, PUBLIC_SUPABASE_ANON_KEY, {
    cookies: {
      getAll() {
        return event.cookies.getAll()
      },
      setAll(cookiesToSet) {
        /**
         * Note: You have to add the `path` variable to the
         * set and remove method due to sveltekit's cookie API
         * requiring this to be set, setting the path to an empty string
         * will replicate previous/standard behavior (https://kit.svelte.dev/docs/types#public-types-cookies)
         */
        cookiesToSet.forEach(({ name, value, options }) =>
          event.cookies.set(name, value, { ...options, path: '/' })
        )
      }
    }
  })

  /**
   * Unlike `supabase.auth.getSession()`, which returns the session _without_
   * validating the JWT, this function also calls `getUser()` to validate the
   * JWT before returning the session.
   */
  event.locals.safeGetSession = async () => {
    const {
      data: { session }
    } = await event.locals.supabase.auth.getSession()
    if (!session) {
      return { session: null, user: null }
    }

    const {
      data: { user },
      error
    } = await event.locals.supabase.auth.getUser()
    if (error) {
      // JWT validation has failed
      return { session: null, user: null }
    }

    return { session, user }
  }

  return resolve(event, {
    filterSerializedResponseHeaders(name) {
      return name === 'content-range' || name === 'x-supabase-api-version'
    }
  })
}
```

----------------------------------------

TITLE: Subscribing to Realtime Postgres Changes (TypeScript)
DESCRIPTION: Demonstrates how to subscribe to specific Postgres database changes (e.g., 'INSERT' events on the 'movies' table) using the new `channel()` method in the Realtime library.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/docs/ref/javascript/release-notes.mdx#_snippet_11

LANGUAGE: ts
CODE:
```
supabase
      .channel('any_string_you_want')
      .on(
        'postgres_changes',
        {
          event: 'INSERT',
          schema: 'public',
          table: 'movies',
        },
        (payload) => {
          console.log(payload)
        }
      )
      .subscribe()
```

----------------------------------------

TITLE: Policy Checking User Team Membership via JWT (SQL)
DESCRIPTION: This policy restricts access to `my_table` for authenticated users, allowing only records where the `team_id` is present in the `teams` array stored within the user's `app_metadata` in their JWT. It demonstrates using `auth.jwt()` for authorization based on custom JWT claims.
SOURCE: https://github.com/supabase/supabase/blob/master/examples/prompts/database-rls-policies.md#_snippet_7

LANGUAGE: SQL
CODE:
```
create policy "User is in team"
on my_table
to authenticated
using ( team_id in (select auth.jwt() -> 'app_metadata' -> 'teams'));
```

----------------------------------------

TITLE: Initializing Supabase Client with Generated Types
DESCRIPTION: This example demonstrates how to initialize the Supabase client (`createClient`) by supplying the `Database` type definition. This enables full type inference and type-safe queries throughout your application.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/docs/ref/javascript/typescript-support.mdx#_snippet_3

LANGUAGE: ts
CODE:
```
import { createClient } from '@supabase/supabase-js'
import { Database } from './database.types'

const supabase = createClient<Database>(
  process.env.SUPABASE_URL,
  process.env.SUPABASE_ANON_KEY
)
```

----------------------------------------

TITLE: Extracting Elements from JSONB Array in PostgreSQL
DESCRIPTION: This SQL query demonstrates how to 'flatten' a JSONB array (`food_log`) into individual records using `jsonb_array_elements`. It extracts specific fields like `title`, `calories`, and `meal` from each JSON object within the array, allowing them to be queried as if they were separate table columns. The `->>` operator extracts text, while `->` extracts JSON objects.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/www/_blog/2022-11-24-sql-or-nosql-both-with-postgresql.mdx#_snippet_4

LANGUAGE: SQL
CODE:
```
select
  user_id,
  date,
  jsonb_array_elements(food_log)->>'title' as title,
  jsonb_array_elements(food_log)->'calories' as calories,
  jsonb_array_elements(food_log)->'meal' as meal
from calendar
where user_id = 'xyz'
  and date between '2022-01-01' and '2022-01-31';
```

----------------------------------------

TITLE: Creating a Table with Primary Key and Foreign Key in Postgres SQL
DESCRIPTION: This snippet demonstrates how to create a 'books' table in PostgreSQL. It defines an 'id' column as a 'bigint' identity primary key, a 'title' column as 'text' and 'not null', and an 'author_id' column as a 'bigint' foreign key referencing the 'authors' table. A comment is also added to describe the table's purpose.
SOURCE: https://github.com/supabase/supabase/blob/master/examples/prompts/code-format-sql.md#_snippet_0

LANGUAGE: SQL
CODE:
```
create table books (
  id bigint generated always as identity primary key,
  title text not null,
  author_id bigint references authors (id)
);
comment on table books is 'A list of all the books in the library.';
```

----------------------------------------

TITLE: Downloading Resized Image with Transform Options in JavaScript
DESCRIPTION: This code shows how to download an image from Supabase Storage while applying transformations. It allows specifying width, height, and a resize mode ('contain', 'cover', or 'fill') to control how the image is scaled and cropped, providing flexible image manipulation on download.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/www/_blog/2022-12-13-storage-image-resizing-smart-cdn.mdx#_snippet_1

LANGUAGE: javascript
CODE:
```
supabase.storage.from('bucket').download('image.jpg', {
  transform: {
    width: 800,
    height: 300,
    resize: 'contain', // 'contain' | 'cover' | 'fill'
  },
})
```

----------------------------------------

TITLE: Building a Complete Form with Form Components (TSX)
DESCRIPTION: This comprehensive snippet demonstrates how to construct a complete form using the `<Form>` and `<FormField>` components, integrating the previously defined Zod schema and `useForm` hook. It shows how to render an input field with labels, descriptions, and messages, and includes a submit button.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/design-system/content/docs/components/form.mdx#_snippet_6

LANGUAGE: tsx
CODE:
```
'use client'

import { zodResolver } from '@hookform/resolvers/zod'
import { useForm } from 'react-hook-form'
import { z } from 'zod'

import { Button } from '@/components/ui/button'
import {
  Form,
  FormControl,
  FormDescription,
  FormField,
  FormItem,
  FormLabel,
  FormMessage,
} from '@/components/ui/form'
import { Input } from '@/components/ui/input'

const formSchema = z.object({
  username: z.string().min(2, {
    message: 'Username must be at least 2 characters.',
  }),
})

export function ProfileForm() {
  // ...

  return (
    <Form {...form}>
      <form onSubmit={form.handleSubmit(onSubmit)} className="space-y-8">
        <FormField
          control={form.control}
          name="username"
          render={({ field }) => (
            <FormItem>
              <FormLabel>Username</FormLabel>
              <FormControl>
                <Input placeholder="shadcn" {...field} />
              </FormControl>
              <FormDescription>This is your public display name.</FormDescription>
              <FormMessage />
            </FormItem>
          )}
        />
        <Button type="submit">Submit</Button>
      </form>
    </Form>
  )
}
```

----------------------------------------

TITLE: Query Supabase Data in React (JavaScript)
DESCRIPTION: Imports React hooks and the Supabase client. Initializes the client using environment variables. Defines an async function `getInstruments` to fetch data from the 'instruments' table and updates the component state. Renders the fetched data as a list.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/getting-started/quickstarts/reactjs.mdx#_snippet_3

LANGUAGE: js
CODE:
```
import { useEffect, useState } from "react";
import { createClient } from "@supabase/supabase-js";

const supabase = createClient(import.meta.env.VITE_SUPABASE_URL, import.meta.env.VITE_SUPABASE_ANON_KEY);

function App() {
  const [instruments, setInstruments] = useState([]);

  useEffect(() => {
    getInstruments();
  }, []);

  async function getInstruments() {
    const { data } = await supabase.from("instruments").select();
    setInstruments(data);
  }

  return (
    <ul>
      {instruments.map((instrument) => (
        <li key={instrument.name}>{instrument.name}</li>
      ))}
    </ul>
  );
}

export default App;
```

----------------------------------------

TITLE: Initializing Supabase Client and Fetching Data in Swift
DESCRIPTION: This snippet demonstrates how to initialize the Supabase client in Swift using a URL and an anonymous key, and then fetch data from a 'countries' table. It defines a `Country` struct for decoding the fetched data, showcasing basic data retrieval from a Supabase database.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/www/_blog/2024-04-15-supabase-swift.mdx#_snippet_0

LANGUAGE: Swift
CODE:
```
let url = URL(string: "...")!
let anonKey = "public-anon-key"
let client = SupabaseClient(supabaseURL: url, supabaseKey: anonKey)

struct Country: Decodable {
  let id: Int
  let name: String
}

let countries: [Country] = try await supabase.from("countries")
  .select()
  .execute()
  .value
```

----------------------------------------

TITLE: Referencing GitHub Secrets in Supabase config.toml
DESCRIPTION: This TOML snippet shows how to reference environment variables (like `GITHUB_CLIENT_ID` and `GITHUB_SECRET`) from a `.env` file within the `config.toml` using the `env()` function. This method is used to securely configure external OAuth providers like GitHub without hardcoding sensitive information directly in the configuration file.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/local-development/managing-config.mdx#_snippet_2

LANGUAGE: toml
CODE:
```
[auth.external.github]
enabled = true
client_id = "env(GITHUB_CLIENT_ID)"
secret = "env(GITHUB_SECRET)"
redirect_uri = "" # Overrides the default auth redirectUrl.
```

----------------------------------------

TITLE: Creating Supabase Client Utility Functions for Next.js
DESCRIPTION: Create utility functions to initialize Supabase clients for use in Client Components and Server Components/Actions/Route Handlers, handling cookie management for server-side rendering.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/auth/server-side/nextjs.mdx#_snippet_2

LANGUAGE: ts
CODE:
```
import { createBrowserClient } from '@supabase/ssr'

export function createClient() {
  return createBrowserClient(
    process.env.NEXT_PUBLIC_SUPABASE_URL!,
    process.env.NEXT_PUBLIC_SUPABASE_ANON_KEY!
  )
}
```

LANGUAGE: ts
CODE:
```
import { createServerClient } from '@supabase/ssr'
import { cookies } from 'next/headers'

export async function createClient() {
  const cookieStore = await cookies()

  return createServerClient(
    process.env.NEXT_PUBLIC_SUPABASE_URL!,
    process.env.NEXT_PUBLIC_SUPABASE_ANON_KEY!,
    {
      cookies: {
        getAll() {
          return cookieStore.getAll()
        },
        setAll(cookiesToSet) {
          try {
            cookiesToSet.forEach(({ name, value, options }) =>
              cookieStore.set(name, value, options)
            )
          } catch {
            // The `setAll` method was called from a Server Component.
            // This can be ignored if you have middleware refreshing
            // user sessions.
          }
        }
      }
    }
  )
}
```

----------------------------------------

TITLE: Defining User Profiles Table and Row Level Security Policies (SQL)
DESCRIPTION: This SQL script defines the 'profiles' table, sets up Row Level Security (RLS) policies for public viewing, user-specific insertion and updates, and creates a trigger to automatically create a profile entry upon new user sign-up. It also configures a 'storage.buckets' entry for avatars and sets up RLS policies for avatar image access.
SOURCE: https://github.com/supabase/supabase/blob/master/examples/user-management/nextjs-user-management/README.md#_snippet_1

LANGUAGE: sql
CODE:
```
-- Create a table for public profiles
create table profiles (
  id uuid references auth.users not null primary key,
  updated_at timestamp with time zone,
  username text unique,
  full_name text,
  avatar_url text,
  website text,

  constraint username_length check (char_length(username) >= 3)
);
-- Set up Row Level Security (RLS)
-- See https://supabase.com/docs/guides/auth/row-level-security for more details.
alter table profiles
  enable row level security;

create policy "Public profiles are viewable by everyone." on profiles
  for select using (true);

create policy "Users can insert their own profile." on profiles
  for insert with check ((select auth.uid()) = id);

create policy "Users can update own profile." on profiles
  for update using ((select auth.uid()) = id);

-- This trigger automatically creates a profile entry when a new user signs up via Supabase Auth.
-- See https://supabase.com/docs/guides/auth/managing-user-data#using-triggers for more details.
create function public.handle_new_user()
returns trigger as $$
begin
  insert into public.profiles (id, full_name, avatar_url)
  values (new.id, new.raw_user_meta_data->>'full_name', new.raw_user_meta_data->>'avatar_url');
  return new;
end;
$$ language plpgsql security definer;
create trigger on_auth_user_created
  after insert on auth.users
  for each row execute procedure public.handle_new_user();

-- Set up Storage!
insert into storage.buckets (id, name)
  values ('avatars', 'avatars');

-- Set up access controls for storage.
-- See https://supabase.com/docs/guides/storage#policy-examples for more details.
create policy "Avatar images are publicly accessible." on storage.objects
  for select using (bucket_id = 'avatars');

create policy "Anyone can upload an avatar." on storage.objects
  for insert with check (bucket_id = 'avatars');

create policy "Anyone can update their own avatar." on storage.objects
  for update using ( auth.uid() = owner ) with check (bucket_id = 'avatars');
```

----------------------------------------

TITLE: Implementing Server-Side OAuth Sign-in with Next.js Server Actions
DESCRIPTION: This snippet demonstrates how to perform a server-side OAuth sign-in using Supabase within a Next.js Server Action. It leverages the 'use server' directive to define a server-only function signIn that handles authentication, which is then invoked by a form submission. This approach enables secure, server-side user authentication without client-side JavaScript.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/www/_blog/2023-11-01-supabase-is-now-compatible-with-nextjs-14.mdx#_snippet_0

LANGUAGE: tsx
CODE:
```
export default async function Page() {
  const signIn = async () => {
    'use server'
    supabase.auth.signInWithOAuth({...})
  }

  return (
    <form action={signIn}>
      <button>Sign in with GitHub</button>
    </form>
  )
}
```

----------------------------------------

TITLE: Creating SQL `send_sms` Hook Function
DESCRIPTION: This PL/pgSQL function, `send_sms`, serves as the custom SMS hook. It extracts the phone number and OTP from the incoming event JSON, calculates a future scheduled time (nearest 5-minute window) for message delivery, assigns a dynamic priority, and then inserts the job into the `job_queue` table for processing by a worker. It also sets appropriate permissions for the `job_queue` table.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/auth/auth-hooks/send-sms-hook.mdx#_snippet_3

LANGUAGE: sql
CODE:
```
create or replace function send_sms(event jsonb) returns void as $$
declare
    job_data jsonb;
    scheduled_time timestamp;
    priority int;
begin
    -- extract phone and otp from the event json
    job_data := jsonb_build_object(
        'phone', event->'user'->>'phone',
        'otp', event->'sms'->>'otp'
    );

    -- calculate the nearest 5-minute window for scheduled_time
    scheduled_time := date_trunc('minute', now()) + interval '5 minute' * floor(extract('epoch' from (now() - date_trunc('minute', now())) / 60) / 5);

    -- assign priority dynamically (example logic: higher priority for earlier scheduled time)
    priority := extract('epoch' from (scheduled_time - now()))::int;

    -- insert the job into the job_queue table
    insert into job_queue (job_data, priority, scheduled_at, max_retries)
    values (job_data, priority, scheduled_time, 2);
end;
$$ language plpgsql;

grant all
  on table public.job_queue
  to supabase_auth_admin;

revoke all
  on table public.job_queue
  from authenticated, anon;
```

----------------------------------------

TITLE: Initializing Supabase Client - Kotlin
DESCRIPTION: Initializes the Supabase client in Kotlin using `createSupabaseClient` with the project URL and anonymous key. It also installs the Realtime plugin for real-time functionalities.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/realtime/broadcast.mdx#_snippet_3

LANGUAGE: Kotlin
CODE:
```
val supabaseUrl = "https://<project>.supabase.co"
val supabaseKey = "<your-anon-key>"
val supabase = createSupabaseClient(supabaseUrl, supabaseKey) {
    install(Realtime)
}
```

----------------------------------------

TITLE: Accessing Custom Claims in JavaScript Client with `jwt-decode`
DESCRIPTION: This JavaScript snippet demonstrates how to access custom claims, such as `user_role`, from the `access_token` within a Supabase client application. It uses the `jwt-decode` package to parse the JWT whenever the authentication state changes, allowing client-side logic to react to user roles.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/database/postgres/custom-claims-and-role-based-access-control-rbac.mdx#_snippet_6

LANGUAGE: javascript
CODE:
```
import { jwtDecode } from 'jwt-decode'

const { subscription: authListener } = supabase.auth.onAuthStateChange(async (event, session) => {
  if (session) {
    const jwt = jwtDecode(session.access_token)
    const userRole = jwt.user_role
  }
})
```

----------------------------------------

TITLE: Storing Embeddings in Postgres with Vecs
DESCRIPTION: This Python code connects to a Postgres database using the `vecs` client, requiring a database connection string. It then creates or retrieves a collection named 'sentences' with a dimension of 1536, which matches the default output dimension of the Amazon Titan Embeddings G1 – Text model. Finally, it efficiently upserts the previously generated sentence embeddings into this collection and creates an index for optimized similarity search queries.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/ai/integrations/amazon-bedrock.mdx#_snippet_2

LANGUAGE: python
CODE:
```
import vecs

DB_CONNECTION = "postgresql://<user>:<password>@<host>:<port>/<db_name>"

# create vector store client
vx = vecs.Client(DB_CONNECTION)

# create a collection named 'sentences' with 1536 dimensional vectors
# to match the default dimension of the Titan Embeddings G1 - Text model
sentences = vx.get_or_create_collection(name="sentences", dimension=1536)

# upsert the embeddings into the 'sentences' collection
sentences.upsert(records=embeddings)

# create an index for the 'sentences' collection
sentences.create_index()
```

----------------------------------------

TITLE: Creating Account Form Component with Supabase (JavaScript)
DESCRIPTION: This React component (`AccountForm`) allows authenticated users to view and update their profile details (full name, username, website, avatar URL) using Supabase. It fetches existing profile data on mount and provides a function to upsert changes to the 'profiles' table. It also includes a sign-out button.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/getting-started/tutorials/with-nextjs.mdx#_snippet_13

LANGUAGE: jsx
CODE:
```
'use client'
import { useCallback, useEffect, useState } from 'react'
import { createClient } from '@/utils/supabase/client'

export default function AccountForm({ user }) {
  const supabase = createClient()
  const [loading, setLoading] = useState(true)
  const [fullname, setFullname] = useState(null)
  const [username, setUsername] = useState(null)
  const [website, setWebsite] = useState(null)
  const [avatar_url, setAvatarUrl] = useState(null)

  const getProfile = useCallback(async () => {
    try {
      setLoading(true)

      const { data, error, status } = await supabase
        .from('profiles')
        .select(`full_name, username, website, avatar_url`)
        .eq('id', user?.id)
        .single()

      if (error && status !== 406) {
        throw error
      }

      if (data) {
        setFullname(data.full_name)
        setUsername(data.username)
        setWebsite(data.website)
        setAvatarUrl(data.avatar_url)
      }
    } catch (error) {
      alert('Error loading user data!')
    } finally {
      setLoading(false)
    }
  }, [user, supabase])

  useEffect(() => {
    getProfile()
  }, [user, getProfile])

  async function updateProfile({ username, website, avatar_url }) {
    try {
      setLoading(true)

      const { error } = await supabase.from('profiles').upsert({
        id: user?.id,
        full_name: fullname,
        username,
        website,
        avatar_url,
        updated_at: new Date().toISOString(),
      })
      if (error) throw error
      alert('Profile updated!')
    } catch (error) {
      alert('Error updating the data!')
    } finally {
      setLoading(false)
    }
  }

  return (
    <div className="form-widget">
      <div>
        <label htmlFor="email">Email</label>
        <input id="email" type="text" value={user?.email} disabled />
      </div>
      <div>
        <label htmlFor="fullName">Full Name</label>
        <input
          id="fullName"
          type="text"
          value={fullname || ''}
          onChange={(e) => setFullname(e.target.value)}
        />
      </div>
      <div>
        <label htmlFor="username">Username</label>
        <input
          id="username"
          type="text"
          value={username || ''}
          onChange={(e) => setUsername(e.target.value)}
        />
      </div>
      <div>
        <label htmlFor="website">Website</label>
        <input
          id="website"
          type="url"
          value={website || ''}
          onChange={(e) => setWebsite(e.target.value)}
        />
      </div>

      <div>
        <button
          className="button primary block"
          onClick={() => updateProfile({ fullname, username, website, avatar_url })}
          disabled={loading}
        >
          {loading ? 'Loading ...' : 'Update'}
        </button>
      </div>

      <div>
        <form action="/auth/signout" method="post">
          <button className="button block" type="submit">
            Sign out
          </button>
        </form>
      </div>
    </div>
  )
}
```

----------------------------------------

TITLE: Restrictive Update Policy Based on MFA Assurance Level (SQL)
DESCRIPTION: This restrictive policy prevents authenticated users from updating their profiles unless their Multi-Factor Authentication (MFA) Assurance Level (`aal`) extracted from their JWT is 'aal2'. It showcases using `auth.jwt()` for enforcing MFA-based access control.
SOURCE: https://github.com/supabase/supabase/blob/master/examples/prompts/database-rls-policies.md#_snippet_8

LANGUAGE: SQL
CODE:
```
create policy "Restrict updates."
on profiles
as restrictive
for update
to authenticated using (
  (select auth.jwt()->>'aal') = 'aal2'
);
```

----------------------------------------

TITLE: Defining a Basic Products Table with RLS in PostgreSQL
DESCRIPTION: This SQL snippet defines a `products` table with common columns like `id`, `name`, `category`, `price`, and `created_at`. It also demonstrates enabling Row Level Security (RLS) on the table, which is a crucial step for implementing fine-grained access control in PostgreSQL.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/www/_blog/2025-04-03-declarative-schemas.mdx#_snippet_0

LANGUAGE: SQL
CODE:
```
create table "products" (
  id         bigint generated by default as identity,
  name       text not null,
  category   text,
  price      numeric(10, 2) not null,
  created_at timestamptz default now()
);

alter table "products"
enable row level security;
```

----------------------------------------

TITLE: Applying JSON Schema Validation with Check Constraints in PostgreSQL
DESCRIPTION: This comprehensive SQL example demonstrates how to enforce JSON Schema validation on a `json` column using a `CHECK` constraint. It defines a `customer` table with a `metadata` column, ensuring its content conforms to a schema requiring an array of strings for `tags`. The snippet also includes examples of valid and invalid `INSERT` statements, illustrating how the constraint prevents non-conforming data and triggers an error for invalid payloads.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/database/extensions/pg_jsonschema.mdx#_snippet_2

LANGUAGE: SQL
CODE:
```
create table customer(
    id serial primary key,
    ...
    metadata json,

    check (
        json_matches_schema(
            '{
                "type": "object",
                "properties": {
                    "tags": {
                        "type": "array",
                        "items": {
                            "type": "string",
                            "maxLength": 16
                        }
                    }
                }
            }',
            metadata
        )
    )
);

-- Example: Valid Payload
insert into customer(metadata)
values ('{"tags": ["vip", "darkmode-ui"]}');
-- Result:
--   INSERT 0 1

-- Example: Invalid Payload
insert into customer(metadata)
values ('{"tags": [1, 3]}');
-- Result:
--   ERROR:  new row for relation "customer" violates check constraint "customer_metadata_check"
--   DETAIL:  Failing row contains (2, {"tags": [1, 3]}).
```

----------------------------------------

TITLE: Creating RLS Policy to Restrict Read Access by Topic - SQL
DESCRIPTION: Illustrates an RLS policy that restricts authenticated users to only read messages from a specific Realtime topic named 'locked'. It utilizes the `realtime.topic()` function to access the channel's topic name within the policy condition.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/www/_blog/2024-08-13-supabase-realtime-broadcast-and-presence-authorization.mdx#_snippet_3

LANGUAGE: sql
CODE:
```
create policy "authenticated users can only read from 'locked' topic"
on "realtime"."messages"
as permissive
for select   -- read only
to authenticated
using (
  realtime.topic() = 'locked'  -- access the topic name
);
```

----------------------------------------

TITLE: Creating Row Level Security Policy for User-Specific Inserts in PostgreSQL
DESCRIPTION: This SQL statement creates a Row Level Security (RLS) policy named 'Individuals can only write their own messages.' on the `messages` table. The policy restricts `INSERT` operations, ensuring that a new message can only be inserted if the `user_id` of the message matches the authenticated user's UID, typically obtained from a JWT via `auth.uid()`.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/www/_blog/2021-02-27-cracking-postgres-interview.mdx#_snippet_4

LANGUAGE: SQL
CODE:
```
CREATE POLICY "Individuals can only write their own messages." ON messages FOR
    INSERT WITH CHECK ((select auth.uid()) = user_id);
```

----------------------------------------

TITLE: Implementing Supabase Phone MFA Challenge and Verification in React
DESCRIPTION: This React component, AuthMFA, handles the multi-factor authentication challenge and verification flow for phone factors. It first lists available MFA factors using `supabase.auth.mfa.listFactors()`, then initiates a challenge for the first phone factor via `supabase.auth.mfa.challenge()`, and finally verifies the user-provided code with `supabase.auth.mfa.verify()`. It manages state for the verification code, errors, factor ID, challenge ID, and phone number, providing UI to start the challenge and verify the code.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/auth/auth-mfa/phone.mdx#_snippet_2

LANGUAGE: tsx
CODE:
```
function AuthMFA() {
  const [verifyCode, setVerifyCode] = useState('')
  const [error, setError] = useState('')
  const [factorId, setFactorId] = useState('')
  const [challengeId, setChallengeId] = useState('')
  const [phoneNumber, setPhoneNumber] = useState('')

  const startChallenge = async () => {
    setError('')
    try {
      const factors = await supabase.auth.mfa.listFactors()
      if (factors.error) {
        throw factors.error
      }

      const phoneFactor = factors.data.phone[0]

      if (!phoneFactor) {
        throw new Error('No phone factors found!')
      }

      const factorId = phoneFactor.id
      setFactorId(factorId)
      setPhoneNumber(phoneFactor.phone)

      const challenge = await supabase.auth.mfa.challenge({ factorId })
      if (challenge.error) {
        setError(challenge.error.message)
        throw challenge.error
      }

      setChallengeId(challenge.data.id)
    } catch (error) {
      setError(error.message)
    }
  }

  const verifyCode = async () => {
    setError('')
    try {
      const verify = await supabase.auth.mfa.verify({
        factorId,
        challengeId,
        code: verifyCode
      })
      if (verify.error) {
        setError(verify.error.message)
        throw verify.error
      }
    } catch (error) {
      setError(error.message)
    }
  }

  return (
    <>
      <div>Please enter the code sent to your phone.</div>
      {phoneNumber && <div>Phone number: {phoneNumber}</div>}
      {error && <div className="error">{error}</div>}
      <input
        type="text"
        value={verifyCode}
        onChange={(e) => setVerifyCode(e.target.value.trim())}
      />
      {!challengeId ? (
        <input type="button" value="Start Challenge" onClick={startChallenge} />
      ) : (
        <input type="button" value="Verify Code" onClick={verifyCode} />
      )}
    </>
  )
}
```

----------------------------------------

TITLE: Postgres Function for Semantic Search (Cosine Distance)
DESCRIPTION: This SQL function, `match_documents`, performs semantic search by comparing a `query_embedding` with document embeddings using the cosine distance (`<=>`) operator. It returns a set of documents ordered by similarity, filtered by a `match_threshold` and limited by `match_count`, ensuring relevant results are retrieved efficiently.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/ai/semantic-search.mdx#_snippet_3

LANGUAGE: SQL
CODE:
```
-- Match documents using cosine distance (<=>)
create or replace function match_documents (
  query_embedding vector(512),
  match_threshold float,
  match_count int
)
returns setof documents
language sql
as $$
  select *
  from documents
  where documents.embedding <=> query_embedding < 1 - match_threshold
  order by documents.embedding <=> query_embedding asc
  limit least(match_count, 200);
$$;
```

----------------------------------------

TITLE: Streaming Realtime Data with Supabase Dart
DESCRIPTION: This snippet illustrates the updated approach for streaming data from a Supabase table. The 'Before' version used a specific table string format for filtering and required `.execute()`. The 'After' version simplifies filtering with `.eq()` and makes `primaryKey` a named parameter, removing the need for `.execute()`.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/docs/ref/dart/v0/upgrade-guide.mdx#_snippet_27

LANGUAGE: Dart
CODE:
```
supabase.from('my_table:id=eq.120')
  .stream(['id'])
  .listen();
```

LANGUAGE: Dart
CODE:
```
supabase.from('my_table')
  .stream(primaryKey: ['id'])
  .eq('id', '120')
  .listen();
```

----------------------------------------

TITLE: Fetching Static Data in Next.js Server Component (TypeScript)
DESCRIPTION: This TypeScript snippet demonstrates fetching data from Supabase in a static Next.js Server Component. It uses `createClient` from `@supabase/supabase-js` with environment variables and a `Database` type for type-safe static data retrieval, ideal for build-time data fetching.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/auth/auth-helpers/nextjs.mdx#_snippet_26

LANGUAGE: TypeScript
CODE:
```
import { createClient } from '@supabase/supabase-js'

import type { Database } from '@/lib/database.types'

export default async function Page() {
  const supabase = createClient<Database>(
    process.env.NEXT_PUBLIC_SUPABASE_URL!,
    process.env.NEXT_PUBLIC_SUPABASE_ANON_KEY!
  )

  const { data } = await supabase.from('todos').select()
  return <pre>{JSON.stringify(data, null, 2)}</pre>
}
```

----------------------------------------

TITLE: Creating Todo in Next.js Edge Route Handler (JavaScript)
DESCRIPTION: This snippet illustrates handling a POST request in a Next.js Route Handler configured for the Edge runtime. It uses `createRouteHandlerClient` with `cookies()` to interact with Supabase for inserting data, ensuring low-latency operations for authenticated requests.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/auth/auth-helpers/nextjs.mdx#_snippet_23

LANGUAGE: JavaScript
CODE:
```
import { createRouteHandlerClient } from '@supabase/auth-helpers-nextjs'
import { NextResponse } from 'next/server'
import { cookies } from 'next/headers'

export const runtime = 'edge'
export const dynamic = 'force-dynamic'

export async function POST(request) {
  const { title } = await request.json()
  const cookieStore = cookies()
  const supabase = createRouteHandlerClient({ cookies: () => cookieStore })

  const { data } = await supabase.from('todos').insert({ title }).select()
  return NextResponse.json(data)
}
```

----------------------------------------

TITLE: Implementing Account Page for Profile Management (Angular TypeScript)
DESCRIPTION: This Angular component (AccountPage) allows authenticated users to view and update their profile details (username, website). It fetches the user's email and profile data on initialization using the SupabaseService. The component provides methods to update the profile and sign out, handling loading states and displaying notifications.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/getting-started/tutorials/with-ionic-angular.mdx#_snippet_4

LANGUAGE: typescript
CODE:
```
import { Component, OnInit } from '@angular/core'
import { Router } from '@angular/router'
import { Profile, SupabaseService } from '../supabase.service'

@Component({
  selector: 'app-account',
  template: `
    <ion-header>
      <ion-toolbar>
        <ion-title>Account</ion-title>
      </ion-toolbar>
    </ion-header>

    <ion-content>
      <form>
        <ion-item>
          <ion-label position="stacked">Email</ion-label>
          <ion-input type="email" name="email" [(ngModel)]="email" readonly></ion-input>
        </ion-item>

        <ion-item>
          <ion-label position="stacked">Name</ion-label>
          <ion-input type="text" name="username" [(ngModel)]="profile.username"></ion-input>
        </ion-item>

        <ion-item>
          <ion-label position="stacked">Website</ion-label>
          <ion-input type="url" name="website" [(ngModel)]="profile.website"></ion-input>
        </ion-item>
        <div class="ion-text-center">
          <ion-button fill="clear" (click)="updateProfile()">Update Profile</ion-button>
        </div>
      </form>

      <div class="ion-text-center">
        <ion-button fill="clear" (click)="signOut()">Log Out</ion-button>
      </div>
    </ion-content>
  `,
  styleUrls: ['./account.page.scss'],
})
export class AccountPage implements OnInit {
  profile: Profile = {
    username: '',
    avatar_url: '',
    website: '',
  }

  email = ''

  constructor(
    private readonly supabase: SupabaseService,
    private router: Router
  ) {}
  ngOnInit() {
    this.getEmail()
    this.getProfile()
  }

  async getEmail() {
    this.email = await this.supabase.user.then((user) => user?.email || '')
  }

  async getProfile() {
    try {
      const { data: profile, error, status } = await this.supabase.profile
      if (error && status !== 406) {
        throw error
      }
      if (profile) {
        this.profile = profile
      }
    } catch (error: any) {
      alert(error.message)
    }
  }

  async updateProfile(avatar_url: string = '') {
    const loader = await this.supabase.createLoader()
    await loader.present()
    try {
      const { error } = await this.supabase.updateProfile({ ...this.profile, avatar_url })
      if (error) {
        throw error
      }
      await loader.dismiss()
      await this.supabase.createNotice('Profile updated!')
    } catch (error: any) {
      await loader.dismiss()
      await this.supabase.createNotice(error.message)
    }
  }

  async signOut() {
    console.log('testing?')
    await this.supabase.signOut()
    this.router.navigate(['/'], { replaceUrl: true })
  }
}
```

----------------------------------------

TITLE: Generating Supabase TypeScript Types from Postgres Schema
DESCRIPTION: This snippet details the `supabase gen types` CLI command, used to introspect a PostgreSQL schema and automatically generate TypeScript definitions. The `typescript` subcommand specifically generates types for TypeScript, providing end-to-end type safety from the database to the application.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/www/_blog/2023-08-08-supabase-local-dev.mdx#_snippet_7

LANGUAGE: markdown
CODE:
```
supabase gen types
Generate types from Postgres schema

Usage:
  supabase gen types [command]

Available Commands:
  typescript  Generate types for TypeScript
```

----------------------------------------

TITLE: Implementing Authentication Component (React JSX)
DESCRIPTION: This React component (`src/Auth.jsx`) handles user authentication using Supabase Magic Links. It provides an email input field and a submit button, sending a magic link to the user's email for passwordless sign-in. It manages loading states and displays alerts for success or errors.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/getting-started/tutorials/with-react.mdx#_snippet_4

LANGUAGE: jsx
CODE:
```
import { useState } from 'react'
import { supabase } from './supabaseClient'

export default function Auth() {
  const [loading, setLoading] = useState(false)
  const [email, setEmail] = useState('')

  const handleLogin = async (event) => {
    event.preventDefault()

    setLoading(true)
    const { error } = await supabase.auth.signInWithOtp({ email })

    if (error) {
      alert(error.error_description || error.message)
    } else {
      alert('Check your email for the login link!')
    }
    setLoading(false)
  }

  return (
    <div className="row flex flex-center">
      <div className="col-6 form-widget">
        <h1 className="header">Supabase + React</h1>
        <p className="description">Sign in via magic link with your email below</p>
        <form className="form-widget" onSubmit={handleLogin}>
          <div>
            <input
              className="inputField"
              type="email"
              placeholder="Your email"
              value={email}
              required={true}
              onChange={(e) => setEmail(e.target.value)}
            />
          </div>
          <div>
            <button className={'button block'} disabled={loading}>
              {loading ? <span>Loading</span> : <span>Send magic link</span>}
            </button>
          </div>
        </form>
      </div>
    </div>
  )
}
```

----------------------------------------

TITLE: Seeding Supabase with Video Embeddings using Mixpeek
DESCRIPTION: This Python function `seed` initializes Supabase and Mixpeek clients, creates a `video_chunks` table in Supabase, processes a video into chunks using Mixpeek, generates embeddings for each chunk, and inserts them into the Supabase table. Finally, it creates an IVFFlat index on the `embedding` column for efficient vector similarity search.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/ai/examples/mixpeek-video-search.mdx#_snippet_6

LANGUAGE: python
CODE:
```
def seed():
    # Initialize Supabase and Mixpeek clients
    supabase: Client = create_client(SUPABASE_URL, SUPABASE_KEY)
    mixpeek = Mixpeek(MIXPEEK_API_KEY)

    # Create a table for storing video chunk embeddings
    supabase.table("video_chunks").create({
        "id": "text",
        "start_time": "float8",
        "end_time": "float8",
        "embedding": "vector(768)",
        "metadata": "jsonb"
    })

    # Process and embed video
    video_url = "https://example.com/your_video.mp4"
    processed_chunks = mixpeek.tools.video.process(
        video_source=video_url,
        chunk_interval=1,  # 1 second intervals
        resolution=[720, 1280]
    )

    for chunk in processed_chunks:
        print(f"Processing video chunk: {chunk['start_time']}")

        # Generate embedding using Mixpeek
        embed_response = mixpeek.embed.video(
            model_id="vuse-generic-v1",
            input=chunk['base64_chunk'],
            input_type="base64"
        )

        # Insert into Supabase
        supabase.table("video_chunks").insert({
            "id": f"chunk_{chunk['start_time']}",
            "start_time": chunk["start_time"],
            "end_time": chunk["end_time"],
            "embedding": embed_response['embedding'],
            "metadata": {"video_url": video_url}
        }).execute()

    print("Video processed and embeddings inserted")

    # Create index for fast search performance
    supabase.query("CREATE INDEX ON video_chunks USING ivfflat (embedding vector_cosine_ops) WITH (lists = 100)").execute()
    print("Created index")
```

----------------------------------------

TITLE: Managing User Presence with Supabase Realtime (JavaScript)
DESCRIPTION: This example demonstrates how to use Supabase Realtime's Presence feature to synchronize shared user state, such as a 'user is typing' indicator. It initializes a channel with a presence key, subscribes to presence synchronization events, and tracks user activity (e.g., keyboard events) by sending timestamp updates. It also shows how to retrieve the current presence state and untrack data when no longer needed.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/www/_blog/2022-08-18-supabase-realtime-multiplayer-general-availability.mdx#_snippet_1

LANGUAGE: javascript
CODE:
```
const userId = 'user_1234'
const slackRoomId = '#random'

const channel = supabase.channel(slackRoomId, {
  config: {
    presence: { key: userId }
  }
})

// We can subscribe to all Presence changes using the 'presence' -> 'sync' event.
channel
  .on('presence', { event: 'sync' }, () => presenceChanged())
  .subscribe()

/*
  A contrived example where we bind to all keyboard
  events and send them over our channel
*/
document.addEventListener('keydown', function(event){
  channel.track({ isTyping: Date.now() })
})

// Receive Presence updates
const presenceChanged = () => {
  const newState = channel.presenceState()
  console.log(newState)
}

// When you no longer wish to track data
channel.untrack().then(status => console.log(status)
```

----------------------------------------

TITLE: Creating User Profiles Table and Auth Trigger with Security Definer (SQL/PL/pgSQL)
DESCRIPTION: This comprehensive SQL and PL/pgSQL example sets up a `public.profiles` table and an `auth.users` trigger. The `handle_new_user` function, defined with `SECURITY DEFINER`, ensures that when a new user is inserted into `auth.users`, a corresponding profile entry is automatically created in `public.profiles` with the necessary permissions, preventing common permission errors.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/troubleshooting/dashboard-errors-when-managing-users-N1ls4A.mdx#_snippet_1

LANGUAGE: SQL
CODE:
```
create table profiles (
  id uuid references auth.users on delete cascade not null primary key,
  updated_at timestamp with time zone,
  username text unique,
  full_name text,
  avatar_url text,
  website text,

  constraint username_length check (char_length(username) >= 3)
);

create function public.handle_new_user()
returns trigger as $$
begin
  insert into public.profiles (id, full_name, avatar_url)
  values (new.id, new.raw_user_meta_data->>'full_name', new.raw_user_meta_data->>'avatar_url');
  return new;
end;
$$ language plpgsql security definer;

create trigger on_auth_user_created
  after insert on auth.users
  for each row execute procedure public.handle_new_user();
```

----------------------------------------

TITLE: Handling Google Sign-In Callback with Nonce (TypeScript)
DESCRIPTION: An enhanced version of the sign-in callback that includes a nonce for added security. The function passes both the credential token and the pre-generated nonce to the Supabase `signInWithIdToken` method.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/auth/social-login/auth-google.mdx#_snippet_7

LANGUAGE: ts
CODE:
```
async function handleSignInWithGoogle(response) {
  const { data, error } = await supabase.auth.signInWithIdToken({
    provider: 'google',
    token: response.credential,
    nonce: '<NONCE>',
  })
}
```

----------------------------------------

TITLE: Applying Simplified Restrictive RLS Policy with Helper Function (PostgreSQL)
DESCRIPTION: This SQL snippet demonstrates how to apply a simplified restrictive Row-Level Security (RLS) policy to a PostgreSQL table. It leverages the previously defined `public.is_supabase_or_firebase_project_jwt()` helper function to enforce access restrictions, ensuring that only authenticated users with valid JWTs from the configured Supabase or Firebase project can access the data.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/auth/third-party/firebase-auth.mdx#_snippet_8

LANGUAGE: SQL
CODE:
```
create policy "Restrict access to correct Supabase and Firebase projects"
  on table_name
  as restrictive
  to authenticated
  using ((select public.is_supabase_or_firebase_project_jwt()) is true);
```

----------------------------------------

TITLE: Policy Allowing Users to Access Their Own Records (SQL)
DESCRIPTION: This policy grants authenticated users access to records in `test_table` where the `user_id` matches their authenticated user ID (`auth.uid()`). It's presented as an example for which an index should be added for performance.
SOURCE: https://github.com/supabase/supabase/blob/master/examples/prompts/database-rls-policies.md#_snippet_9

LANGUAGE: SQL
CODE:
```
create policy "Users can access their own records" on test_table
to authenticated
using ( (select auth.uid()) = user_id );
```

----------------------------------------

TITLE: Initializing Supabase Client in SolidJS
DESCRIPTION: This TypeScript React (TSX) snippet creates and exports a Supabase client instance. It imports the `createClient` function from `@supabase/supabase-js` and uses environment variables (`VITE_SUPABASE_URL`, `VITE_SUPABASE_ANON_KEY`) to initialize the client, making it available for use throughout the SolidJS application.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/getting-started/tutorials/with-solidjs.mdx#_snippet_3

LANGUAGE: tsx
CODE:
```
import { createClient } from '@supabase/supabase-js'

const supabaseUrl = import.meta.env.VITE_SUPABASE_URL
const supabaseAnonKey = import.meta.env.VITE_SUPABASE_ANON_KEY

export const supabase = createClient(supabaseUrl, supabaseAnonKey)
```

----------------------------------------

TITLE: Initializing Supabase Client and Board Operations in Angular Data Service
DESCRIPTION: This TypeScript snippet defines a `DataService` responsible for interacting with Supabase. It initializes the Supabase client using environment variables and defines constants for table names. It includes functions to `startBoard` (insert a new empty board) and `getBoards` (retrieve user-specific boards with related board information).
SOURCE: https://github.com/supabase/supabase/blob/master/apps/www/_blog/2022-08-24-building-a-realtime-trello-board-with-supabase-and-angular.mdx#_snippet_19

LANGUAGE: TypeScript
CODE:
```
import { Injectable } from '@angular/core'
import { SupabaseClient, createClient } from '@supabase/supabase-js'
import { environment } from 'src/environments/environment'

export const BOARDS_TABLE = 'boards'
export const USER_BOARDS_TABLE = 'user_boards'
export const LISTS_TABLE = 'lists'
export const CARDS_TABLE = 'cards'
export const USERS_TABLE = 'users'

@Injectable({
  providedIn: 'root',
})
export class DataService {
  private supabase: SupabaseClient

  constructor() {
    this.supabase = createClient(environment.supabaseUrl, environment.supabaseKey)
  }

  async startBoard() {
    // Minimal return will be the default in the next version and can be removed here!
    return await this.supabase.from(BOARDS_TABLE).insert({}, { returning: 'minimal' })
  }

  async getBoards() {
    const boards = await this.supabase.from(USER_BOARDS_TABLE).select(`
    boards:board_id ( title, id )
  `)
    return boards.data || []
  }
}
```

----------------------------------------

TITLE: Redirecting Users Based on Supabase Auth State in Flutter Splash Page
DESCRIPTION: This Dart code defines the `SplashPage` in a Flutter application, responsible for determining the initial navigation path based on the user's Supabase authentication status. It checks for an `initialSession` and redirects authenticated users to `RoomsPage` and unauthenticated users to `RegisterPage`, providing a seamless entry point into the application.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/www/_blog/2022-11-22-flutter-authorization-with-rls.mdx#_snippet_11

LANGUAGE: Dart
CODE:
```
import 'package:flutter/material.dart';
import 'package:my_chat_app/pages/register_page.dart';
import 'package:my_chat_app/pages/rooms_page.dart';
import 'package:my_chat_app/utils/constants.dart';
import 'package:supabase_flutter/supabase_flutter.dart';

/// Page to redirect users to the appropriate page depending on the initial auth state
class SplashPage extends StatefulWidget {
  const SplashPage({Key? key}) : super(key: key);

  @override
  SplashPageState createState() => SplashPageState();
}

class SplashPageState extends State<SplashPage> {
  @override
  void initState() {
    getInitialSession();
    super.initState();
  }

  Future<void> getInitialSession() async {
    // quick and dirty way to wait for the widget to mount
    await Future.delayed(Duration.zero);

    try {
      final session =
          await SupabaseAuth.instance.initialSession;
      if (session == null) {
        Navigator.of(context).pushAndRemoveUntil(
            RegisterPage.route(), (_) => false);
      } else {
        Navigator.of(context).pushAndRemoveUntil(
            RoomsPage.route(), (_) => false);
      }
    } catch (_) {
      context.showErrorSnackBar(
        message: 'Error occurred during session refresh',
      );
      Navigator.of(context).pushAndRemoveUntil(
          RegisterPage.route(), (_) => false);
    }
  }

  @override
  Widget build(BuildContext context) {
    return const Scaffold(
      body: Center(child: CircularProgressIndicator()),
    );
  }
}
```

----------------------------------------

TITLE: Configuring Supabase Environment Variables
DESCRIPTION: This snippet shows the required environment variables for connecting the Next.js application to a Supabase project. Users must replace placeholders with their specific Supabase project URL and API anonymous key, found in the Supabase dashboard.
SOURCE: https://github.com/supabase/supabase/blob/master/examples/auth/nextjs/README.md#_snippet_2

LANGUAGE: plaintext
CODE:
```
NEXT_PUBLIC_SUPABASE_URL=[INSERT SUPABASE PROJECT URL]
NEXT_PUBLIC_SUPABASE_ANON_KEY=[INSERT SUPABASE PROJECT API ANON KEY]
```

----------------------------------------

TITLE: Initializing and Running AI Inference Session for Embeddings (JavaScript)
DESCRIPTION: This snippet demonstrates the basic usage of the new `Supabase.ai` API to instantiate an inference session with the 'gte-small' model and run inference on a text prompt. The output is a numerical embedding, suitable for storage and retrieval with pgvector.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/www/_blog/2024-04-16-ai-inference-now-available-in-supabase-edge-functions.mdx#_snippet_0

LANGUAGE: JavaScript
CODE:
```
// Instantiate a new inference session
const session = new Supabase.ai.Session('gte-small')

// then use the session to run inference on a prompt
const output = await session.run('Luke, I am your father')

console.log(output)
// [ -0.047715719789266586, -0.006132732145488262, ...]
```

----------------------------------------

TITLE: Setting Up PGlite Database in React App (JavaScript)
DESCRIPTION: This React component snippet demonstrates how to integrate the PGlite database setup into an `App.jsx` file. It uses `useEffect` to initialize the database and schema upon component mount, ensuring a single database instance. It also fetches existing content from the `embeddings` table and updates the component's state.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/www/_blog/2024-08-29-in-browser-semantic-search-pglite.mdx#_snippet_2

LANGUAGE: javascript
CODE:
```
import { getDB, initSchema, countRows } from './utils/db'
import { useState, useEffect, useRef, useCallback } from 'react'

export default function App() {
  const [content, setContent] = useState([])
  const initailizing = useRef(false)
  // Create a reference to the worker object.
  const worker = useRef(null)

  // Set up DB
  const db = useRef(null)
  useEffect(() => {
    const setup = async () => {
      initailizing.current = true
      db.current = await getDB()
      await initSchema(db.current)
      let count = await countRows(db.current, 'embeddings')

      if (count === 0) {
        // TODO: seed the database.
      }
      // Get Items
      const items = await db.current.query('SELECT content FROM embeddings')
      setContent(items.rows.map((x) => x.content))
    }
    if (!db.current && !initailizing.current) {
      setup()
    }
  }, [])

  // [...]

  return <pre>{JSON.stringify(content)}</pre>
}
```

----------------------------------------

TITLE: Creating RLS Policy for UPDATE with USING and WITH CHECK
DESCRIPTION: Demonstrates how to create an RLS policy for `UPDATE` operations, requiring both `USING` and `WITH CHECK` clauses. The `USING` clause filters which rows can be updated, while `WITH CHECK` ensures that the updated values adhere to the policy, typically by verifying the `user_id` against the authenticated user's ID.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/troubleshooting/rls-simplified-BJTcS8.mdx#_snippet_5

LANGUAGE: SQL
CODE:
```
create policy "Allow user to edit their stuff"
on "public"."<SOME TABLE NAME>"
as RESTRICTIVE
for UPDATE
to authenticated
using (
  (select auth.uid()) = user_id
)
with check(
  (select auth.uid()) = user_id
);
```

----------------------------------------

TITLE: Installing Supabase JS Package (Bash)
DESCRIPTION: This command installs the @supabase/supabase-js NPM package, which provides a JavaScript client library for interacting with Supabase projects, enabling ORM syntax and realtime event subscriptions.
SOURCE: https://github.com/supabase/supabase/blob/master/examples/with-cloudflare-workers/README.md#_snippet_0

LANGUAGE: bash
CODE:
```
npm i @supabase/supabase-js
```

----------------------------------------

TITLE: Installing Supabase JavaScript Client
DESCRIPTION: This command installs the `@supabase/supabase-js` library, which is the official JavaScript client for interacting with Supabase services. It is a required dependency for any application that needs to communicate with a Supabase backend.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/getting-started/tutorials/with-nextjs.mdx#_snippet_2

LANGUAGE: bash
CODE:
```
npm install @supabase/supabase-js
```

----------------------------------------

TITLE: Installing Supabase JavaScript Client
DESCRIPTION: This command installs the `@supabase/supabase-js` library, which is the official JavaScript client for interacting with Supabase services. It is a crucial dependency for integrating Supabase authentication, database, and other features into your Ionic Vue application.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/getting-started/tutorials/with-ionic-vue.mdx#_snippet_1

LANGUAGE: bash
CODE:
```
npm install @supabase/supabase-js
```

----------------------------------------

TITLE: Generating and Storing Embeddings with Transformers.js and Supabase (JavaScript)
DESCRIPTION: This JavaScript snippet demonstrates how to generate a text embedding using `@xenova/transformers` (specifically `Supabase/gte-small` model) and then store it into a PostgreSQL table via the Supabase client. It shows the process of extracting the embedding data and inserting it into the `embedding` column of the `posts` table.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/database/extensions/pgvector.mdx#_snippet_2

LANGUAGE: JavaScript
CODE:
```
import { pipeline } from '@xenova/transformers'
const generateEmbedding = await pipeline('feature-extraction', 'Supabase/gte-small')

const title = 'First post!'
const body = 'Hello world!'

// Generate a vector using Transformers.js
const output = await generateEmbedding(body, {
  pooling: 'mean',
  normalize: true,
})

// Extract the embedding output
const embedding = Array.from(output.data)

// Store the vector in Postgres
const { data, error } = await supabase.from('posts').insert({
  title,
  body,
  embedding,
})
```

----------------------------------------

TITLE: Generating and Storing Image Embeddings in Supabase Vector
DESCRIPTION: This `seed` function connects to the Supabase Vector store, creates or retrieves an 'image_vectors' collection, loads the CLIP model, encodes sample images into 512-dimension embeddings, and upserts these embeddings along with their identifiers and metadata into the collection. Finally, it creates an index for efficient search performance.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/ai/examples/image-search-openai-clip.mdx#_snippet_6

LANGUAGE: python
CODE:
```
def seed():
    # create vector store client
    vx = vecs.create_client(DB_CONNECTION)

    # create a collection of vectors with 3 dimensions
    images = vx.get_or_create_collection(name="image_vectors", dimension=512)

    # Load CLIP model
    model = SentenceTransformer('clip-ViT-B-32')

    # Encode an image:
    img_emb1 = model.encode(Image.open('./images/one.jpg'))
    img_emb2 = model.encode(Image.open('./images/two.jpg'))
    img_emb3 = model.encode(Image.open('./images/three.jpg'))
    img_emb4 = model.encode(Image.open('./images/four.jpg'))

    # add records to the *images* collection
    images.upsert(
        records=[
            (
                "one.jpg",        # the vector's identifier
                img_emb1,          # the vector. list or np.array
                {"type": "jpg"}   # associated  metadata
            ), (
                "two.jpg",
                img_emb2,
                {"type": "jpg"}
            ), (
                "three.jpg",
                img_emb3,
                {"type": "jpg"}
            ), (
                "four.jpg",
                img_emb4,
                {"type": "jpg"}
            )
        ]
    )
    print("Inserted images")

    # index the collection for fast search performance
    images.create_index()
    print("Created index")
```

----------------------------------------

TITLE: Deprecated: Importing from @supabase/auth-helpers-nextjs (TypeScript)
DESCRIPTION: This code snippet shows DEPRECATED imports from `@supabase/auth-helpers-nextjs`. This package and its functions (`createMiddlewareClient`, `createClientComponentClient`) are no longer supported and will cause the application to break. AI models are strictly prohibited from generating these imports.
SOURCE: https://github.com/supabase/supabase/blob/master/examples/prompts/nextjs-supabase-auth.md#_snippet_1

LANGUAGE: typescript
CODE:
```
import { createMiddlewareClient } from '@supabase/auth-helpers-nextjs'  // ❌ BREAKS APPLICATION
import { createClientComponentClient } from '@supabase/auth-helpers-nextjs'  // ❌ BREAKS APPLICATION
```

----------------------------------------

TITLE: Protecting `explain()` in Production with Pre-request Function (SQL)
DESCRIPTION: This SQL snippet defines a PL/pgSQL function `filter_plan_requests` that acts as a pre-request hook for PostgREST. It checks if an incoming request is attempting to use the `EXPLAIN` plan feature (`application/vnd.pgrst.plan`) and if the client's IP address does not match a specified allowed IP. If both conditions are met, it raises an `insufficient_privilege` error, thereby restricting `explain()` access to authorized IPs in a production environment. The function is then assigned to the `pgrst.db_pre_request` setting for the `authenticator` role.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/database/debugging-performance.mdx#_snippet_3

LANGUAGE: SQL
CODE:
```
create or replace function filter_plan_requests()
returns void as $$
declare
  headers   json := current_setting('request.headers', true)::json;
  client_ip text := coalesce(headers->>'cf-connecting-ip', '');
  accept    text := coalesce(headers->>'accept', '');
  your_ip   text := '123.123.123.123'; -- replace this with your IP
begin
  if accept like 'application/vnd.pgrst.plan%' and client_ip != your_ip then
    raise insufficient_privilege using
      message = 'Not allowed to use application/vnd.pgrst.plan';
  end if;
end; $$ language plpgsql;
alter role authenticator set pgrst.db_pre_request to 'filter_plan_requests';
notify pgrst, 'reload config';
```

----------------------------------------

TITLE: Initializing Next.js App with JavaScript
DESCRIPTION: This command initializes a new Next.js project named `supabase-nextjs` using `create-next-app` with npm, then navigates into the newly created directory. It sets up the basic project structure for a client-side Next.js application.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/getting-started/tutorials/with-nextjs.mdx#_snippet_0

LANGUAGE: bash
CODE:
```
npx create-next-app@latest --use-npm supabase-nextjs
cd supabase-nextjs
```

----------------------------------------

TITLE: Fetching Data in Next.js Edge Server Component (JavaScript)
DESCRIPTION: This example shows how to fetch data from Supabase within a Next.js Server Component configured to run on the Edge runtime. It uses `createServerComponentClient` with `cookies()` for session management, ensuring data is fetched dynamically and close to the user.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/auth/auth-helpers/nextjs.mdx#_snippet_21

LANGUAGE: JavaScript
CODE:
```
import { cookies } from 'next/headers'
import { createServerComponentClient } from '@supabase/auth-helpers-nextjs'

export const runtime = 'edge'
export const dynamic = 'force-dynamic'

export default async function Page() {
  const cookieStore = cookies()

  const supabase = createServerComponentClient({
    cookies: () => cookieStore,
  })

  const { data } = await supabase.from('todos').select()
  return <pre>{JSON.stringify(data, null, 2)}</pre>
}
```

----------------------------------------

TITLE: Configuring Supabase Environment Variables for SvelteKit
DESCRIPTION: Sets the `PUBLIC_SUPABASE_URL` and `PUBLIC_SUPABASE_ANON_KEY` in a `.env.local` file for a SvelteKit application, making Supabase credentials available on both client and server.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/auth/server-side/creating-a-client.mdx#_snippet_4

LANGUAGE: bash
CODE:
```
PUBLIC_SUPABASE_URL=your_supabase_project_url
PUBLIC_SUPABASE_ANON_KEY=your_supabase_anon_key
```

----------------------------------------

TITLE: Enabling Row Level Security and Public Access for 'todos' Table (SQL)
DESCRIPTION: This SQL snippet enables Row Level Security (RLS) on the 'todos' table to control data access. It then creates a policy named 'Allow public access' that grants 'anon' (anonymous) users permission to perform SELECT operations on the 'todos' table, making the data publicly readable via the API.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/api/quickstart.mdx#_snippet_1

LANGUAGE: SQL
CODE:
```
-- Turn on security
alter table "todos"
enable row level security;

-- Allow anonymous access
create policy "Allow public access"
  on todos
  for select
  to anon
  using (true);
```

----------------------------------------

TITLE: Configuring Supabase Postgres Profiles Table, RLS, Realtime, and Storage
DESCRIPTION: This SQL script defines the `profiles` table with user-related information, establishes Row Level Security (RLS) policies to control data access based on user authentication, sets up Realtime capabilities for the `profiles` table, and initializes a storage bucket for user avatars. RLS policies ensure users can only view public profiles, insert their own, and update their own profile data.
SOURCE: https://github.com/supabase/supabase/blob/master/examples/user-management/solid-user-management/README.md#_snippet_3

LANGUAGE: sql
CODE:
```
-- Create a table for Public Profiles
create table
	profiles (
		id uuid references auth.users not null,
		updated_at timestamp
		with
			time zone,
			username text unique,
			avatar_url text,
			website text,
			primary key (id),
			unique (username),
			constraint username_length check (char_length(username) >= 3)
	);

alter table
	profiles enable row level security;

create policy "Public profiles are viewable by everyone." on profiles for
select
	using (true);

create policy "Users can insert their own profile." on profiles for insert
with
	check ((select auth.uid()) = id);

create policy "Users can update own profile." on profiles for
update
	using ((select auth.uid()) = id);

-- Set up Realtime!
begin;

drop
	publication if exists supabase_realtime;

create publication supabase_realtime;

commit;

alter
	publication supabase_realtime add table profiles;

-- Set up Storage!
insert into
	storage.buckets (id, name)
values
	('avatars', 'avatars');

create policy "Avatar images are publicly accessible." on storage.objects for
select
	using (bucket_id = 'avatars');

create policy "Anyone can upload an avatar." on storage.objects for insert
with
	check (bucket_id = 'avatars');
```

----------------------------------------

TITLE: Declaring Supabase Database Types in SvelteKit (TypeScript)
DESCRIPTION: This TypeScript snippet for `src/app.d.ts` extends SvelteKit's global `App` namespace to include Supabase-specific types. It defines `supabase` in `event.locals` with database-specific typing and adds `safeGetSession` for secure session retrieval. It also declares `session` and `user` types for `PageData`, enhancing type safety and IntelliSense for Supabase interactions.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/auth/auth-helpers/sveltekit.mdx#_snippet_6

LANGUAGE: ts
CODE:
```
// src/app.d.ts

import { SupabaseClient, Session, User } from '@supabase/supabase-js'
import { Database } from './DatabaseDefinitions'

declare global {
  namespace App {
    interface Locals {
      supabase: SupabaseClient<Database>
      safeGetSession(): Promise<{ session: Session | null; user: User | null }>
    }
    interface PageData {
      session: Session | null
      user: User | null
    }
    // interface Error {}
    // interface Platform {}
  }
}
```

----------------------------------------

TITLE: Example Generated TypeScript Database Types
DESCRIPTION: Illustrates the structure of generated TypeScript types for a `movies` table. It defines `Row`, `Insert`, and `Update` interfaces, providing type safety for data retrieval, insertion, and update operations with Supabase.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/api/rest/generating-types.mdx#_snippet_6

LANGUAGE: typescript
CODE:
```
export type Json = string | number | boolean | null | { [key: string]: Json | undefined } | Json[]

export interface Database {
  public: {
    Tables: {
      movies: {
        Row: {
          // the data expected from .select()
          id: number
          name: string
          data: Json | null
        }
        Insert: {
          // the data to be passed to .insert()
          id?: never // generated columns must not be supplied
          name: string // `not null` columns with no default must be supplied
          data?: Json | null // nullable columns can be omitted
        }
        Update: {
          // the data to be passed to .update()
          id?: never
          name?: string // `not null` columns are optional on .update()
          data?: Json | null
        }
      }
    }
  }
}
```

----------------------------------------

TITLE: Scheduling HTTP POST Requests with pg_cron in SQL
DESCRIPTION: This SQL snippet demonstrates how to schedule a recurring HTTP POST request using the pg_cron extension. It executes an http_post call to a specified Edge Function URL every minute, including authorization headers and a JSON body. This is useful for regular data synchronization or triggering external services.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/database/extensions/pg_net.mdx#_snippet_19

LANGUAGE: SQL
CODE:
```
select cron.schedule(
	'cron-job-name',
	'* * * * *', -- Executes every minute (cron syntax)
	$$
	    -- SQL query
	    select "net"."http_post"(
            -- URL of Edge function
            url:='https://project-ref.supabase.co/functions/v1/function-name',
            headers:='{"Authorization": "Bearer <YOUR_ANON_KEY>"}'::jsonb,
            body:='{"name": "pg_net"}'::jsonb
	    ) as "request_id";
	$$
);
```

----------------------------------------

TITLE: Handling Errors with Returned Object in Supabase.js (Current)
DESCRIPTION: This snippet demonstrates the current and recommended error handling pattern in Supabase.js. Instead of throwing exceptions, API operations now return an object containing both `data` and `error` properties. This allows for direct conditional checks on the `error` object, simplifying error management.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/www/_blog/2020-10-30-improved-dx.mdx#_snippet_1

LANGUAGE: js
CODE:
```
const { data, error } = supabase.from('todos').select('*')
if (error) console.log(error)
// else, carry on ..
```

----------------------------------------

TITLE: Setting Up Supabase Environment Variables
DESCRIPTION: These environment variables store your Supabase project URL and anonymous key, essential for connecting your Next.js application to your Supabase backend. They should be placed in a `.env.local` file for local development.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/auth/auth-helpers/nextjs-pages.mdx#_snippet_2

LANGUAGE: bash
CODE:
```
NEXT_PUBLIC_SUPABASE_URL=your-supabase-url
NEXT_PUBLIC_SUPABASE_ANON_KEY=your-supabase-anon-key
```

----------------------------------------

TITLE: Setting Supabase Environment Variables (.env.local)
DESCRIPTION: This snippet shows the structure for a `.env.local` file, which is used by Vite to manage environment variables. It defines `VITE_SUPABASE_URL` and `VITE_SUPABASE_ANON_KEY`, which should be replaced with actual Supabase project API URL and `anon` key, respectively. These variables are crucial for the `supabaseClient` to connect to the correct Supabase instance.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/getting-started/tutorials/with-refine.mdx#_snippet_4

LANGUAGE: bash
CODE:
```
VITE_SUPABASE_URL=YOUR_SUPABASE_URL
VITE_SUPABASE_ANON_KEY=YOUR_SUPABASE_ANON_KEY
```

----------------------------------------

TITLE: Enforcing MFA for Selected Users with RLS - SQL
DESCRIPTION: A PostgreSQL Row Level Security (RLS) policy designed to enforce MFA only for users who have enabled it. This policy allows access if the user has completed MFA (`aal2`) or if they have no verified MFA factors, providing flexibility while ensuring security for those who opt-in.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/www/_blog/2022-12-14-mfa-auth-via-rls.mdx#_snippet_3

LANGUAGE: sql
CODE:
```
create policy "Allow access on table only if user has gone through MFA"
  on table_name
  as restrictive -- very important!
  to authenticated
  using (
    array[auth.jwt()->>'aal'] <@ (
      select
          case
            when count(id) > 0 then array['aal2']
            else array['aal1', 'aal2']
          end as aal
        from auth.mfa_factors
        where (select auth.uid()) = user_id and status = 'verified'
    ));
```

----------------------------------------

TITLE: Example Next.js Sentry Configuration with Supabase
DESCRIPTION: Comprehensive Sentry configuration examples for different Next.js environments (browser, server, middleware, and edge functions), including the Supabase Sentry integration and span deduplication.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/telemetry/sentry-monitoring.mdx#_snippet_3

LANGUAGE: ts
CODE:
```
import * as Sentry from '@sentry/nextjs'
import { SupabaseClient } from '@supabase/supabase-js'
import { supabaseIntegration } from '@supabase/sentry-js-integration'

Sentry.init({
  dsn: SENTRY_DSN,
  integrations: [
    supabaseIntegration(SupabaseClient, Sentry, {
      tracing: true,
      breadcrumbs: true,
      errors: true,
    }),
    Sentry.browserTracingIntegration({
      shouldCreateSpanForRequest: (url) => {
        return !url.startsWith(`${process.env.NEXT_PUBLIC_SUPABASE_URL}/rest`)
      },
    }),
  ],

  // Adjust this value in production, or use tracesSampler for greater control
  tracesSampleRate: 1,

  // Setting this option to true will print useful information to the console while you're setting up Sentry.
  debug: true,
})
```

LANGUAGE: ts
CODE:
```
import * as Sentry from '@sentry/nextjs'
import { SupabaseClient } from '@supabase/supabase-js'
import { supabaseIntegration } from '@supabase/sentry-js-integration'

Sentry.init({
  dsn: SENTRY_DSN,
  integrations: [
    supabaseIntegration(SupabaseClient, Sentry, {
      tracing: true,
      breadcrumbs: true,
      errors: true,
    }),
    Sentry.nativeNodeFetchIntegration({
      breadcrumbs: true,
      ignoreOutgoingRequests: (url) => {
        return url.startsWith(`${process.env.NEXT_PUBLIC_SUPABASE_URL}/rest`)
      },
    }),
  ],
  // Adjust this value in production, or use tracesSampler for greater control
  tracesSampleRate: 1,

  // Setting this option to true will print useful information to the console while you're setting up Sentry.
  debug: true,
})
```

LANGUAGE: ts
CODE:
```
import * as Sentry from '@sentry/nextjs'
import { SupabaseClient } from '@supabase/supabase-js'
import { supabaseIntegration } from '@supabase/sentry-js-integration'

Sentry.init({
  dsn: SENTRY_DSN,
  integrations: [
    supabaseIntegration(SupabaseClient, Sentry, {
      tracing: true,
      breadcrumbs: true,
      errors: true,
    }),
    Sentry.winterCGFetchIntegration({
      breadcrumbs: true,
      shouldCreateSpanForRequest: (url) => {
        return !url.startsWith(`${process.env.NEXT_PUBLIC_SUPABASE_URL}/rest`)
      },
    }),
  ],
  // Adjust this value in production, or use tracesSampler for greater control
  tracesSampleRate: 1,

  // Setting this option to true will print useful information to the console while you're setting up Sentry.
  debug: true,
})
```

LANGUAGE: ts
CODE:
```
// https://nextjs.org/docs/app/building-your-application/optimizing/instrumentation
export async function register() {
  if (process.env.NEXT_RUNTIME === 'nodejs') {
    await import('./sentry.server.config')
  }

  if (process.env.NEXT_RUNTIME === 'edge') {
    await import('./sentry.edge.config')
  }
}
```

----------------------------------------

TITLE: Implementing Supabase Authentication in Next.js Client Component (JavaScript)
DESCRIPTION: This client-side React component demonstrates user authentication flows (sign up, sign in, sign out) using Supabase. It utilizes `createClientComponentClient` to interact with Supabase Auth and `next/navigation` to refresh the router after authentication actions. It requires user email and password inputs.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/auth/auth-helpers/nextjs.mdx#_snippet_6

LANGUAGE: jsx
CODE:
```
'use client'

import { createClientComponentClient } from '@supabase/auth-helpers-nextjs'
import { useRouter } from 'next/navigation'
import { useState } from 'react'

export default function Login() {
  const [email, setEmail] = useState('')
  const [password, setPassword] = useState('')
  const router = useRouter()
  const supabase = createClientComponentClient()

  const handleSignUp = async () => {
    await supabase.auth.signUp({
      email,
      password,
      options: {
        emailRedirectTo: `${location.origin}/auth/callback`,
      },
    })
    router.refresh()
  }

  const handleSignIn = async () => {
    await supabase.auth.signInWithPassword({
      email,
      password,
    })
    router.refresh()
  }

  const handleSignOut = async () => {
    await supabase.auth.signOut()
    router.refresh()
  }

  return (
    <>
      <input name="email" onChange={(e) => setEmail(e.target.value)} value={email} />
      <input
        type="password"
        name="password"
        onChange={(e) => setPassword(e.target.value)}
        value={password}
      />
      <button onClick={handleSignUp}>Sign up</button>
      <button onClick={handleSignIn}>Sign in</button>
      <button onClick={handleSignOut}>Sign out</button>
    </>
  )
}
```

----------------------------------------

TITLE: Subscribing to INSERT Events (JavaScript)
DESCRIPTION: This JavaScript code subscribes specifically to `INSERT` events within the `public` schema using the Supabase Realtime client. It logs the payload of any new row insertion.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/realtime/postgres-changes.mdx#_snippet_11

LANGUAGE: js
CODE:
```
const changes = supabase
  .channel('schema-db-changes')
  .on(
    'postgres_changes',
    {
      event: 'INSERT', // Listen only to INSERTs
      schema: 'public',
    },
    (payload) => console.log(payload)
  )
.subscribe()
```

----------------------------------------

TITLE: Implicit WHERE Clause Translation of a Postgres RLS Policy
DESCRIPTION: This snippet illustrates how a `SELECT` RLS policy is implicitly translated into a `WHERE` clause when a user attempts to query the table. The policy's condition (`auth.uid() = todos.user_id`) is automatically appended to the query, filtering results based on the user's ID.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/database/postgres/row-level-security.mdx#_snippet_2

LANGUAGE: SQL
CODE:
```
select *
from todos
where auth.uid() = todos.user_id;
-- Policy is implicitly added.
```

----------------------------------------

TITLE: Implementing Simple Similarity Search with Supabase Edge Function (TypeScript)
DESCRIPTION: This Edge Function performs a basic similarity search. It takes a query, generates an OpenAI embedding for it, and then uses a Supabase RPC call (`match_documents`) to find the most similar documents based on a specified threshold and count. It handles CORS preflight requests and returns the matched documents as JSON.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/www/_blog/2023-02-03-openai-embeddings-postgres-vector.mdx#_snippet_5

LANGUAGE: TypeScript
CODE:
```
import { serve } from 'https://deno.land/std@0.170.0/http/server.ts'
import 'https://deno.land/x/xhr@0.2.1/mod.ts'
import { createClient } from 'jsr:@supabase/supabase-js@2'
import { Configuration, OpenAIApi } from 'https://esm.sh/openai@3.1.0'
import { supabaseClient } from './lib/supabase'

export const corsHeaders = {
  'Access-Control-Allow-Origin': '*',
  'Access-Control-Allow-Headers': 'authorization, x-client-info, apikey, content-type',
}

serve(async (req) => {
  // Handle CORS
  if (req.method === 'OPTIONS') {
    return new Response('ok', { headers: corsHeaders })
  }

  // Search query is passed in request payload
  const { query } = await req.json()

  // OpenAI recommends replacing newlines with spaces for best results
  const input = query.replace(/\n/g, ' ')

  const configuration = new Configuration({ apiKey: '<YOUR_OPENAI_API_KEY>' })
  const openai = new OpenAIApi(configuration)

  // Generate a one-time embedding for the query itself
  const embeddingResponse = await openai.createEmbedding({
    model: 'text-embedding-ada-002',
    input,
  })

  const [{ embedding }] = embeddingResponse.data.data

  // In production we should handle possible errors
  const { data: documents } = await supabaseClient.rpc('match_documents', {
    query_embedding: embedding,
    match_threshold: 0.78, // Choose an appropriate threshold for your data
    match_count: 10, // Choose the number of matches
  })

  return new Response(JSON.stringify(documents), {
    headers: { ...corsHeaders, 'Content-Type': 'application/json' },
  })
})
```

----------------------------------------

TITLE: Implementing a Multi-Purpose Edge Function with Dynamic Code Execution (TypeScript)
DESCRIPTION: This TypeScript code defines a Supabase Edge Function that accepts and executes dynamic JavaScript code. It includes robust authorization checks using the service role key, initializes the Supabase client, and uses `new Function()` to safely execute the provided code within an async context, allowing interaction with Supabase services.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/www/_blog/2024-11-13-supabase-dynamic-functions.mdx#_snippet_4

LANGUAGE: TypeScript
CODE:
```
import "jsr:@supabase/functions-js/edge-runtime.d.ts";

// Import the supabase client
import { createClient } from "https://esm.sh/@supabase/supabase-js@2";

console.log("===\\n\\tBooted Edge Worker!\\n===\\n");
const supabase_url = Deno.env.get("SUPABASE_URL") ?? "";
const service_role = Deno.env.get("SUPABASE_SERVICE_ROLE_KEY");
// Set the permission to service_role key:
const supabase = createClient(supabase_url, service_role);
// This allows us to use Supabase.ai in the function
const session = new Supabase.ai.Session('gte-small');

Deno.serve(async (req: Request) =>
  const authorization = req.headers.get("Authorization");
  if (!authorization) throw new Error("Authorization header is missing.");
  // Ensures that the function is called with service_role to prevent missuse
  if (!authorization.includes(service_role)) {
    throw new Error("Authorization header is invalid.");
  }

  const { code } = await req.json();
  try {
    // Wrap the provided code in an async function context
    const asyncFunction = new Function('supabase', `
      return (async () => {
        ${code.replace(/\\\\/g, '')}
      })();
    `);
    // Pass the Supabase client as the scope for the function to use:
    const data = await asyncFunction(supabase);
    console.log(data);
    return new Response(
      JSON.stringify({ data }),
      { headers: { 'Content-Type': 'application/json', 'Connection': 'keep-alive' } },
    );
  } catch (error) {
    console.error("Error executing user code:", error);
    return new Response(
      JSON.stringify({ error: "An error occurred -> " + error.message }),
      { status: 500, headers: { "Content-Type": "application/json" } }
    );
  }
});
```

----------------------------------------

TITLE: Performing Data Mutations with Next.js Server Actions and Supabase
DESCRIPTION: This snippet shows how to integrate data mutation logic using Server Actions alongside data fetching in a Next.js Server Component. The createNote function, marked with 'use server', performs an insert operation to Supabase, allowing server-side data modifications directly within the component's scope.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/www/_blog/2023-11-01-supabase-is-now-compatible-with-nextjs-14.mdx#_snippet_2

LANGUAGE: tsx
CODE:
```
export default async function Page() {
  const { data } = await supabase.from('...').select()

  const createNote = async () => {
    'use server'
    await supabase.from('...').insert({...})
  }

  return ...
}
```

----------------------------------------

TITLE: Analyzing PostgreSQL Query Execution Plan
DESCRIPTION: This PostgreSQL command executes a given query and displays its execution plan, including actual row counts and timing information. It is crucial for identifying performance bottlenecks and optimizing slow queries by understanding how the database processes them.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/troubleshooting/canceling-statement-due-to-statement-timeout-581wFv.mdx#_snippet_1

LANGUAGE: SQL
CODE:
```
explain analyze <query-statement-here>;
```

----------------------------------------

TITLE: Executing Dynamic JavaScript via SQL `edge.exec` Function (SQL)
DESCRIPTION: This SQL query demonstrates how to invoke the `edge.exec` function, passing a JavaScript code string as a parameter. The JavaScript code then uses the `supabase.rpc` client to call a PostgreSQL function and includes error handling for robust execution.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/www/_blog/2024-11-13-supabase-dynamic-functions.mdx#_snippet_10

LANGUAGE: SQL
CODE:
```
SELECT edge.exec(
  $js$
  const { data, error } = await supabase.rpc('postgres_function', {'foo': 'bar'});
  if (error) {
    return new Response(JSON.stringify({ error: "An error occurred ->" + error.message }), {
      status: 500,
      headers: { "Content-Type": "application/json" },
    });
  }
  return data;
  $js$
);
```

----------------------------------------

TITLE: Configuring Supabase Auth Middleware in Next.js (TypeScript)
DESCRIPTION: This TypeScript middleware refreshes the user's Supabase session before Server Component routes are rendered. It creates a typed Supabase client configured to use cookies and ensures the session remains active by calling `supabase.auth.getSession()`. The `matcher` configuration limits its execution to relevant paths, and `Database` type provides type safety.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/auth/auth-helpers/nextjs.mdx#_snippet_3

LANGUAGE: ts
CODE:
```
import { createMiddlewareClient } from '@supabase/auth-helpers-nextjs'
import { NextResponse } from 'next/server'

import type { NextRequest } from 'next/server'
import type { Database } from '@/lib/database.types'

export async function middleware(req: NextRequest) {
  const res = NextResponse.next()

  // Create a Supabase client configured to use cookies
  const supabase = createMiddlewareClient<Database>({ req, res })

  // Refresh session if expired - required for Server Components
  await supabase.auth.getSession()

  return res
}

// Ensure the middleware is only called for relevant paths.
export const config = {
  matcher: [
    /*
     * Match all request paths except for the ones starting with:
     * - _next/static (static files)
     * - _next/image (image optimization files)
     * - favicon.ico (favicon file)
     */
    '/((?!_next/static|_next/image|favicon.ico).*)'
  ]
}
```

----------------------------------------

TITLE: Generating TypeScript Types for Supabase Joins
DESCRIPTION: This TypeScript example shows how to use `QueryResult`, `QueryData`, and `QueryError` from `@supabase/supabase-js` to infer and apply types for nested data returned from a Supabase join query, ensuring type safety and improving developer experience.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/database/joins-and-nesting.mdx#_snippet_3

LANGUAGE: TypeScript
CODE:
```
import { QueryResult, QueryData, QueryError } from '@supabase/supabase-js'

const sectionsWithInstrumentsQuery = supabase.from('orchestral_sections').select(`
  id,
  name,
  instruments (
    id,
    name
  )
`)
type SectionsWithInstruments = QueryData<typeof sectionsWithInstrumentsQuery>

const { data, error } = await sectionsWithInstrumentsQuery
if (error) throw error
const sectionsWithInstruments: SectionsWithInstruments = data
```

----------------------------------------

TITLE: Defining Database Entities with SQL
DESCRIPTION: This SQL snippet demonstrates how to define a table, a view based on the table, and a function using standard SQL syntax within a schema file. This approach allows managing complex entities declaratively.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/local-development/declarative-database-schemas.mdx#_snippet_11

LANGUAGE: sql
CODE:
```
create table "employees" (
  "id" integer not null,
  "name" text,
  "age" smallint not null
);

create view "profiles" as
  select id, name from "employees";

create function "get_age"(employee_id integer) RETURNS smallint
  LANGUAGE "sql"
AS $$
  select age
  from employees
  where id = employee_id;
$$;
```

----------------------------------------

TITLE: Creating Supabase Swift Profile View
DESCRIPTION: This SwiftUI view allows authenticated users to view and update their profile details stored in Supabase. It includes fields for username, full name, and website, along with buttons for updating the profile and signing out.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/getting-started/tutorials/with-swift.mdx#_snippet_2

LANGUAGE: Swift
CODE:
```
import SwiftUI

struct ProfileView: View {
  @State var username = ""
  @State var fullName = ""
  @State var website = ""

  @State var isLoading = false

  var body: some View {
    NavigationStack {
      Form {
        Section {
          TextField("Username", text: $username)
            .textContentType(.username)
            .textInputAutocapitalization(.never)
          TextField("Full name", text: $fullName)
            .textContentType(.name)
          TextField("Website", text: $website)
            .textContentType(.URL)
            .textInputAutocapitalization(.never)
        }

        Section {
          Button("Update profile") {
            updateProfileButtonTapped()
          }
          .bold()

          if isLoading {
            ProgressView()
          }!
        }
      }
      .navigationTitle("Profile")
      .toolbar(content: {
        ToolbarItem(placement: .topBarLeading){
          Button("Sign out", role: .destructive) {
            Task {
              try? await supabase.auth.signOut()
            }
          }
        }
      })
    }
    .task {
      await getInitialProfile()
    }
  }

  func getInitialProfile() async {
    do {
      let currentUser = try await supabase.auth.session.user

      let profile: Profile =
      try await supabase
        .from("profiles")
        .select()
        .eq("id", value: currentUser.id)
        .single()
        .execute()
        .value

      self.username = profile.username ?? ""
      self.fullName = profile.fullName ?? ""
      self.website = profile.website ?? ""

    } catch {
      debugPrint(error)
    }
  }

  func updateProfileButtonTapped() {
    Task {
      isLoading = true
      defer { isLoading = false }
      do {
        let currentUser = try await supabase.auth.session.user

        try await supabase
          .from("profiles")
          .update(
            UpdateProfileParams(
              username: username,
              fullName: fullName,
              website: website
            )
          )
          .eq("id", value: currentUser.id)
          .execute()
      } catch {
        debugPrint(error)
      }
    }
  }
}
```

----------------------------------------

TITLE: Enabling Postgres Changes Publication for Supabase (SQL)
DESCRIPTION: This SQL block manages the `supabase_realtime` publication, which is essential for enabling real-time Postgres Changes. It first ensures the publication is dropped if it exists, then re-creates it, and finally adds the `messages` table to it, allowing Supabase Realtime to listen for changes on that specific table.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/realtime/subscribing-to-database-changes.mdx#_snippet_4

LANGUAGE: sql
CODE:
```
begin;

-- remove the supabase_realtime publication
drop
  publication if exists supabase_realtime;

-- re-create the supabase_realtime publication with no tables;
create publication supabase_realtime;

commit;

-- add a table called 'messages' to the publication
-- (update this to match your tables)
alter
  publication supabase_realtime add table messages;
```

----------------------------------------

TITLE: Handling Supabase MFA Assurance Level in React
DESCRIPTION: This React component, `AppWithMFA`, checks the user's current and next Authenticator Assurance Levels (AAL) using `supabase.auth.mfa.getAuthenticatorAssuranceLevel()`. If an upgrade to 'aal2' is required, it conditionally renders the `AuthMFA` component to prompt for an MFA challenge; otherwise, it renders the main `App` component. It ensures the AAL check completes before displaying any UI.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/auth/auth-mfa/totp.mdx#_snippet_1

LANGUAGE: tsx
CODE:
```
function AppWithMFA() {
  const [readyToShow, setReadyToShow] = useState(false)
  const [showMFAScreen, setShowMFAScreen] = useState(false)

  useEffect(() => {
    ;(async () => {
      try {
        const { data, error } = await supabase.auth.mfa.getAuthenticatorAssuranceLevel()
        if (error) {
          throw error
        }

        console.log(data)

        if (data.nextLevel === 'aal2' && data.nextLevel !== data.currentLevel) {
          setShowMFAScreen(true)
        }
      } finally {
        setReadyToShow(true)
      }
    })()
  }, [])

  if (readyToShow) {
    if (showMFAScreen) {
      return <AuthMFA />
    }

    return <App />
  }

  return <></>
}
```

----------------------------------------

TITLE: Defining Insert Policy for User Profiles - Supabase SQL
DESCRIPTION: This RLS policy grants users permission to insert new profile entries. The 'with check' clause ensures that a user can only insert a profile where the 'id' matches their authenticated user ID (auth.uid()).
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/_partials/user_management_quickstart_sql_template.mdx#_snippet_2

LANGUAGE: SQL
CODE:
```
create policy "Users can insert their own profile." on profiles
  for insert with check ((select auth.uid()) = id);
```

----------------------------------------

TITLE: Generating Supabase Storage Signed Upload URLs (Next.js API Route)
DESCRIPTION: This Next.js API route (`src/app/api/supabase/storage/route.ts`) generates a signed upload URL for Supabase Storage. It first authenticates the user session, then uses a Supabase admin client with the service role key to call `createSignedUploadUrl`, allowing secure client-side file uploads without exposing sensitive keys.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/www/_blog/2024-06-18-calcom-platform-starter-kit-nextjs-supabase.mdx#_snippet_3

LANGUAGE: TypeScript
CODE:
```
import { auth } from '@/auth'
import { env } from '@/env'
import { createClient } from '@supabase/supabase-js'

export const dynamic = 'force-dynamic' // defaults to auto
export async function GET(request: Request) {
  try {
    const session = await auth()
    if (!session || !session.user.id) {
      return new Response('Unauthorized', { status: 401 })
    }
    const {
      user: { id },
    } = session
    // Generate signed upload url to use on client.
    const supabaseAdmin = createClient(env.NEXT_PUBLIC_SUPABASE_URL, env.SUPABASE_SERVICE_ROLE_KEY)

    const { data, error } = await supabaseAdmin.storage
      .from('avatars')
      .createSignedUploadUrl(id, { upsert: true })
    console.log(error)
    if (error) throw error

    return new Response(JSON.stringify(data), {
      status: 200,
    })
  } catch (e) {
    console.error(e)
    return new Response('Internal Server Error', { status: 500 })
  }
}
```

----------------------------------------

TITLE: Creating Database Tables with SQL in Supabase
DESCRIPTION: This SQL snippet defines the schema for a simple messaging application within a Supabase Postgres database. It creates three tables: `users` to store user information, `groups` for chat groups, and `messages` to store chat messages. Each table includes primary keys, foreign key references, and default values for timestamps and user IDs, ensuring a structured foundation for the application's data.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/www/_blog/2022-11-08-authentication-in-ionic-angular.mdx#_snippet_0

LANGUAGE: SQL
CODE:
```
create table users (
  id uuid not null primary key,
  email text
);

create table groups (
  id bigint generated by default as identity primary key,
  creator uuid references public.users not null default auth.uid(),
  title text not null,
  created_at timestamp with time zone default timezone('utc'::text, now()) not null
);

create table messages (
  id bigint generated by default as identity primary key,
  user_id uuid references public.users not null default auth.uid(),
  text text check (char_length(text) > 0),
  group_id bigint references groups on delete cascade not null,
  created_at timestamp with time zone default timezone('utc'::text, now()) not null
);
```

----------------------------------------

TITLE: Querying Embeddings via RPC in Supabase Edge Function - TypeScript
DESCRIPTION: This TypeScript Edge Function handles search queries. It first generates an embedding for the user's `search` term using the `gte-small` model. Then, it invokes the `query_embeddings` Postgres function via Supabase's RPC client to find and retrieve relevant content from the database based on vector similarity, returning the top 3 results.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/functions/examples/semantic-search.mdx#_snippet_3

LANGUAGE: TypeScript
CODE:
```
const model = new Supabase.ai.Session('gte-small')

Deno.serve(async (req) => {
  const { search } = await req.json()
  if (!search) return new Response('Please provide a search param!')
  // Generate embedding for search term.
  const embedding = await model.run(search, {
    mean_pool: true,
    normalize: true,
  })

  // Query embeddings.
  const { data: result, error } = await supabase
    .rpc('query_embeddings', {
      embedding,
      match_threshold: 0.8,
    })
    .select('content')
    .limit(3)
  if (error) {
    return Response.json(error)
  }

  return Response.json({ search, result })
})
```

----------------------------------------

TITLE: Getting Query Execution Plan with Supabase-JS (JavaScript)
DESCRIPTION: This JavaScript snippet shows how to use the `explain` function provided by the `supabase-js` library to retrieve the execution plan for a query. The `analyze: true` and `verbose: true` options provide detailed actual execution statistics and more information about the plan, useful for in-application performance debugging.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/troubleshooting/understanding-postgresql-explain-output-Un9dqX.mdx#_snippet_1

LANGUAGE: JavaScript
CODE:
```
const { data, error } = await supabase
  .from('countries')
  .select()
  .explain({analyze:true,verbose:true})
```

----------------------------------------

TITLE: Demonstrating Overlapping Reservation Constraint Failure
DESCRIPTION: This SQL snippet illustrates the effect of the `exclude_duration` constraint. The first `INSERT` successfully adds a reservation. The second `INSERT` attempts to add another reservation with an overlapping `duration` for the same resource, which will fail due to the previously defined exclusion constraint, preventing conflicting bookings.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/www/_blog/2024-07-11-range-columns.mdx#_snippet_4

LANGUAGE: SQL
CODE:
```
-- Add a first reservation
insert into reservations (title, duration)
values ('Tyler Dinner', '[2024-07-04 18:00, 2024-07-04 21:00)');

-- The following insert fails because the duration overlaps with the above
insert into reservations (title, duration)
values ('Thor Dinner', '[2024-07-04 20:00, 2024-07-04 22:00)');
```

----------------------------------------

TITLE: Starting Local Supabase Stack with CLI (Shell)
DESCRIPTION: These commands initialize a new Supabase project and start the entire Supabase stack locally on your machine, enabling local development and testing.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/cli.mdx#_snippet_0

LANGUAGE: Shell
CODE:
```
supabase init
supabase start
```

----------------------------------------

TITLE: Configuring Supabase Environment Variables for React
DESCRIPTION: This snippet shows the required environment variables for connecting a React project to Supabase. These variables, VITE_SUPABASE_URL and VITE_SUPABASE_ANON_KEY, should be placed in a .env file at the project root. They are essential for the Supabase client to initialize and communicate with your Supabase project.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/ui-library/content/docs/react/client.mdx#_snippet_0

LANGUAGE: dotenv
CODE:
```
VITE_SUPABASE_URL=
VITE_SUPABASE_ANON_KEY=
```

----------------------------------------

TITLE: Initializing Supabase Client in Flutter
DESCRIPTION: This Dart code initializes the Supabase client within the `main` function of a Flutter application. It requires the Supabase URL and Anon Key, which are used to establish a connection to the Supabase project. A global `supabase` instance is also extracted for easy access throughout the app.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/www/_blog/2023-05-04-flutter-multi-factor-authentication.mdx#_snippet_2

LANGUAGE: dart
CODE:
```
import 'package:flutter/material. compressible';
import 'package:supabase_flutter/supabase_flutter. compressible';

void main() async {
  await Supabase.initialize(
    url: 'SUPABASE_URL',
    anonKey: 'SUPABASE_ANONKEY',
  );
  runApp(const MyApp());
}

/// Extract SupabaseClient instance in a handy variable
final supabase = Supabase.instance.client;
```

----------------------------------------

TITLE: Signing Up with Email and Password - Implicit Flow (JavaScript)
DESCRIPTION: This snippet demonstrates how to sign up a new user with an email and password using the Supabase JavaScript client in an implicit flow. It initializes the Supabase client and then calls `supabase.auth.signUp()` with the user's email, password, and an optional `emailRedirectTo` URL for post-confirmation redirection. The `emailRedirectTo` URL must be configured as a Redirect URL in the Supabase dashboard or configuration file.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/auth/passwords.mdx#_snippet_0

LANGUAGE: JavaScript
CODE:
```
import { createClient } from '@supabase/supabase-js'
const supabase = createClient('https://your-project.supabase.co', 'your-anon-key')

// ---cut---
async function signUpNewUser() {
  const { data, error } = await supabase.auth.signUp({
    email: 'valid.email@supabase.io',
    password: 'example-password',
    options: {
      emailRedirectTo: 'https://example.com/welcome',
    },
  })
}
```

----------------------------------------

TITLE: Setting Up Supabase Environment Variables in Next.js
DESCRIPTION: Create a `.env.local` file in your project's root directory and add your Supabase project URL and anonymous key.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/auth/server-side/nextjs.mdx#_snippet_1

LANGUAGE: txt
CODE:
```
NEXT_PUBLIC_SUPABASE_URL=<your_supabase_project_url>
NEXT_PUBLIC_SUPABASE_ANON_KEY=<your_supabase_anon_key>
```

----------------------------------------

TITLE: Linking Supabase Project and Pulling Database Schema (Bash)
DESCRIPTION: This command sequence links a local Supabase CLI project to a remote Supabase project using its ID and then pulls the database schema definition from the linked project. This is essential for local development to synchronize the local environment with the remote database structure.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/_partials/project_setup.mdx#_snippet_0

LANGUAGE: bash
CODE:
```
supabase link --project-ref <project-id>
# You can get <project-id> from your project's dashboard URL: https://supabase.com/dashboard/project/<project-id>
supabase db pull
```

----------------------------------------

TITLE: Creating Notes Table with RLS (SQL)
DESCRIPTION: This SQL snippet provides the DDL (Data Definition Language) to create a `notes` table in a PostgreSQL database, typically used with Supabase. It includes setting up Row Level Security (RLS) policies to restrict access, ensuring that authenticated users can only access and modify their own notes.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/auth/server-side/sveltekit.mdx#_snippet_15

LANGUAGE: SQL
CODE:
```
-- Run this SQL against your database to create a `notes` table.

create table notes (
  id bigint primary key generated always as identity,
  created_at timestamp with time zone not null default now(),
  user_id uuid references auth.users on delete cascade not null default auth.uid(),
  note text not null
);

alter table notes enable row level security;

revoke all on table notes from authenticated;
revoke all on table notes from anon;

grant all (note) on table notes to authenticated;
grant select (id) on table notes to authenticated;
grant delete on table notes to authenticated;

create policy "Users can access and modify their own notes"
on notes
for all
to authenticated
using ((select auth.uid()) = user_id);
```

----------------------------------------

TITLE: Signing Out with Supabase JS Client
DESCRIPTION: This JavaScript snippet shows how to sign out a user from their current session using the Supabase client. The `signOut()` method removes the user from the browser session and clears relevant data from local storage. Requires an initialized Supabase client.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/auth/social-login/auth-slack.mdx#_snippet_3

LANGUAGE: JavaScript
CODE:
```
import { createClient } from '@supabase/supabase-js'
const supabase = createClient('<your-project-url>', '<your-anon-key>')

// ---cut---
async function signOut() {
  const { error } = await supabase.auth.signOut()
}
```

----------------------------------------

TITLE: Querying PostgreSQL with Drizzle ORM and node-postgres in Deno Edge Function
DESCRIPTION: This Deno Edge Function demonstrates connecting to a PostgreSQL database using `node-postgres` and querying data with Drizzle ORM. It defines a 'users' table schema, initializes a PostgreSQL client with a connection string from environment variables, and then uses Drizzle to select all users from the table. The function logs the retrieved users and returns an "ok" response.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/www/_blog/2024-09-12-edge-functions-faster-smaller.mdx#_snippet_2

LANGUAGE: JavaScript
CODE:
```
import { drizzle } from 'npm:drizzle-orm@0.33.0/node-postgres'
import pg from 'npm:pg@8.12.0'
const { Client } = pg

import { pgTable, serial, text, varchar } from 'npm:drizzle-orm@0.33.0/pg-core'

export const users = pgTable('users', {
  id: serial('id').primaryKey(),
  fullName: text('full_name'),
  phone: varchar('phone', { length: 256 }),
})

const client = new Client({
  connectionString: Deno.env.get('SUPABASE_DB_URL'),
})

await client.connect()
const db = drizzle(client)

Deno.serve(async (req) => {
  const allUsers = await db.select().from(users)
  console.log(allUsers)

  return new Response('ok')
})
```

----------------------------------------

TITLE: Creating a New Table in PostgreSQL using SQL
DESCRIPTION: This SQL snippet demonstrates how to create a new table named `movies` in a PostgreSQL database. It defines three columns: `id` as a `bigint` with auto-incrementing identity and primary key, `name` as `text` for movie titles, and `description` as `text` for movie summaries. This method is used for programmatic table creation.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/database/tables.mdx#_snippet_0

LANGUAGE: SQL
CODE:
```
create table movies (
  id bigint generated by default as identity primary key,
  name text,
  description text
);
```

----------------------------------------

TITLE: Creating a SELECT Policy for Authenticated Users (SQL)
DESCRIPTION: This policy restricts `SELECT` access on the 'profiles' table to only authenticated users. It uses the `to authenticated` clause to specify the allowed role, making all rows viewable by logged-in users.
SOURCE: https://github.com/supabase/supabase/blob/master/examples/prompts/database-rls-policies.md#_snippet_2

LANGUAGE: SQL
CODE:
```
create policy "Public profiles are viewable only by authenticated users"
on profiles
for select
to authenticated
using ( true );
```

----------------------------------------

TITLE: Creating 'todos' Table in Supabase (SQL)
DESCRIPTION: This SQL snippet creates a new table named 'todos' in the Supabase database. It includes an auto-incrementing primary key 'id' and a 'task' column of type text to store task descriptions. This table forms the basis for the API route.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/api/quickstart.mdx#_snippet_0

LANGUAGE: SQL
CODE:
```
-- Create a table called "todos"
-- with a column to store tasks.
create table todos (
  id serial primary key,
  task text
);
```

----------------------------------------

TITLE: Enabling Row Level Security for a Table in Postgres
DESCRIPTION: This SQL snippet demonstrates how to enable Row Level Security (RLS) on a specified table within a given schema. Enabling RLS is crucial for securing data, especially in exposed schemas like `public`, as it ensures that data access is governed by defined policies.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/database/postgres/row-level-security.mdx#_snippet_0

LANGUAGE: SQL
CODE:
```
alter table <schema_name>.<table_name>
enable row level security;
```

----------------------------------------

TITLE: Configuring Supabase Environment Variables for Next.js
DESCRIPTION: This snippet illustrates the essential environment variables required to establish a connection between a Next.js application and Supabase. 'NEXT_PUBLIC_SUPABASE_URL' defines the URL of your Supabase project, while 'NEXT_PUBLIC_SUPABASE_ANON_KEY' stores the public anonymous key. These variables are fundamental for the Supabase client to authenticate and interact with your backend services.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/ui-library/content/docs/nextjs/client.mdx#_snippet_0

LANGUAGE: env
CODE:
```
NEXT_PUBLIC_SUPABASE_URL=
NEXT_PUBLIC_SUPABASE_ANON_KEY=
```

----------------------------------------

TITLE: Creating a Public View for Vector Collection in SQL
DESCRIPTION: This SQL snippet demonstrates how to create a public view named 'docs' from a 'vector' table. It exposes the 'id', 'embedding', and 'metadata' fields, and specifically extracts the 'url' from the JSON 'metadata' as a text string, making the vector collection accessible via a simplified interface.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/ai/engineering-for-scale.mdx#_snippet_0

LANGUAGE: SQL
CODE:
```
create view public.docs as
select
  id,
  embedding,
  metadata, # Expose the metadata as JSON
  (metadata->>'url')::text as url # Extract the URL as a string
from vector
```

----------------------------------------

TITLE: Setting Up Supabase Client (0.8.0)
DESCRIPTION: This JavaScript file initializes the Supabase client using `createClient` from `@supabase/auth-helpers-sveltekit`. This simplified setup streamlines the client creation process compared to previous versions, requiring only the Supabase URL and anonymous key.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/auth/auth-helpers/sveltekit.mdx#_snippet_35

LANGUAGE: js
CODE:
```
import { createClient } from '@supabase/auth-helpers-sveltekit'
import { env } from '$env/dynamic/public'
// or use the static env

// import { PUBLIC_SUPABASE_URL, PUBLIC_SUPABASE_ANON_KEY } from '$env/static/public';

export const supabaseClient = createClient(env.PUBLIC_SUPABASE_URL, env.PUBLIC_SUPABASE_ANON_KEY)
```

----------------------------------------

TITLE: Pushing Local Supabase Changes to Cloud (Bash)
DESCRIPTION: Links your local Supabase project to a cloud-hosted instance using its project reference, then pushes all local database schema changes and migrations to the remote Supabase project.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/ai/examples/nextjs-vector-search.mdx#_snippet_6

LANGUAGE: bash
CODE:
```
supabase link --project-ref=your-project-ref

supabase db push
```

----------------------------------------

TITLE: Handling Authentication-Based Navigation with Expo Router and Supabase
DESCRIPTION: This snippet defines the root layout for an Expo Router application, integrating the `AuthProvider` to manage global authentication state. The `InitialLayout` component uses the `session` and `initialized` states from the `useAuth` hook to conditionally redirect users: authenticated users are sent to `/list`, while unauthenticated users are directed to the root (`/`). This ensures protected routes are only accessible to logged-in users.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/www/_blog/2023-08-01-react-native-storage.mdx#_snippet_6

LANGUAGE: tsx
CODE:
```
import { Slot, useRouter, useSegments } from 'expo-router'
import { useEffect } from 'react'
import { AuthProvider, useAuth } from '../provider/AuthProvider'

// Makes sure the user is authenticated before accessing protected pages
const InitialLayout = () => {
  const { session, initialized } = useAuth()
  const segments = useSegments()
  const router = useRouter()

  useEffect(() => {
    if (!initialized) return

    // Check if the path/url is in the (auth) group
    const inAuthGroup = segments[0] === '(auth)'

    if (session && !inAuthGroup) {
      // Redirect authenticated users to the list page
      router.replace('/list')
    } else if (!session) {
      // Redirect unauthenticated users to the login page
      router.replace('/')
    }
  }, [session, initialized])

  return <Slot />
}

// Wrap the app with the AuthProvider
const RootLayout = () => {
  return (
    <AuthProvider>
      <InitialLayout />
    </AuthProvider>
  )
}

export default RootLayout
```

----------------------------------------

TITLE: Supabase Database Schema for User Profiles and Storage
DESCRIPTION: This SQL script defines the 'profiles' table with row-level security policies for select, insert, and update operations, ensuring users can only manage their own profiles. It also configures Supabase Realtime for the 'profiles' table and sets up an 'avatars' storage bucket with public access policies for image uploads.
SOURCE: https://github.com/supabase/supabase/blob/master/examples/user-management/flutter-user-management/README.md#_snippet_2

LANGUAGE: sql
CODE:
```
-- Create a table for public "profiles"
create table profiles (
  id uuid references auth.users not null,
  updated_at timestamp with time zone,
  username text unique,
  avatar_url text,
  website text,

  primary key (id),
  unique(username),
  constraint username_length check (char_length(username) >= 3)
);

alter table profiles enable row level security;

create policy "Public profiles are viewable by everyone."
  on profiles for select
  using ( true );

create policy "Users can insert their own profile."
  on profiles for insert
  with check ( (select auth.uid()) = id );

create policy "Users can update own profile."
  on profiles for update
  using ( (select auth.uid()) = id );

-- Set up Realtime!
begin;
  drop publication if exists supabase_realtime;
  create publication supabase_realtime;
commit;
alter publication supabase_realtime add table profiles;

-- Set up Storage!
insert into storage.buckets (id, name)
values ('avatars', 'avatars');

create policy "Avatar images are publicly accessible."
  on storage.objects for select
  using ( bucket_id = 'avatars' );

create policy "Anyone can upload an avatar."
  on storage.objects for insert
  with check ( bucket_id = 'avatars' );
```

----------------------------------------

TITLE: Generating Supabase Types with CLI
DESCRIPTION: This snippet demonstrates how to generate TypeScript definitions from your local Supabase database schema using the Supabase CLI. These types enhance the development experience by providing strong type checking for database interactions.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/www/_blog/2022-08-16-supabase-js-v2.mdx#_snippet_1

LANGUAGE: Bash
CODE:
```
supabase start
supabase gen types typescript --local > DatabaseDefinitions.ts
```

----------------------------------------

TITLE: Declaring Supabase Environment Variables (.env.local)
DESCRIPTION: This snippet shows how to declare public Supabase URL and anonymous key as environment variables in a `.env.local` file. These variables are crucial for the Supabase client to connect to your project's API and are retrieved from your Supabase project settings.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/auth/auth-helpers/sveltekit.mdx#_snippet_1

LANGUAGE: bash
CODE:
```
# Find these in your Supabase project settings https://supabase.com/dashboard/project/_/settings/api
PUBLIC_SUPABASE_URL=https://your-project.supabase.co
PUBLIC_SUPABASE_ANON_KEY=your-anon-key
```

----------------------------------------

TITLE: Starting Local Supabase Stack
DESCRIPTION: This snippet provides commands to start the local Supabase services, including Postgres, Auth, and Storage. This command makes the local Supabase instance accessible for development at http://localhost:54323.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/local-development.mdx#_snippet_2

LANGUAGE: sh
CODE:
```
npx supabase start
```

LANGUAGE: sh
CODE:
```
yarn supabase start
```

LANGUAGE: sh
CODE:
```
pnpx supabase start
```

LANGUAGE: sh
CODE:
```
supabase start
```

----------------------------------------

TITLE: Creating User with Hashed Password using Supabase Auth Admin API (TypeScript)
DESCRIPTION: This snippet demonstrates how to create a new user in Supabase Auth using the `admin.createUser` method, providing a pre-hashed password. It requires the Supabase client initialized with your project URL and API key. The `email_confirm` flag is set to true, indicating the email is already verified, which is crucial for migrating existing confirmed users.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/platform/migrating-to-supabase/auth0.mdx#_snippet_0

LANGUAGE: TypeScript
CODE:
```
import { createClient } from '@supabase/supabase-js'
const supabase = createClient('your_project_url', 'your_supabase_api_key')

// ---cut---
const { data, error } = await supabase.auth.admin.createUser({
  email: 'valid.email@supabase.io',
  password_hash: '$2y$10$a9pghn27d7m0ltXvlX8LiOowy7XfFw0hW0G80OjKYQ1jaoejaA7NC',
  email_confirm: true,
})
```

----------------------------------------

TITLE: Adding a New Column to Projects Table (Declarative Schema Diff) in SQL
DESCRIPTION: This diff snippet demonstrates the simplicity of adding a new `metadata` column to the `private.projects` table when using declarative schemas. Instead of manually updating multiple migration files, views, and functions, the change is a single line in the declarative schema file, with diff tools handling the generation of the necessary migration script.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/www/_blog/2025-04-03-declarative-schemas.mdx#_snippet_3

LANGUAGE: SQL
CODE:
```
--- a/supabase/schemas/projects.sql
+++ b/supabase/schemas/projects.sql
@@ -2,6 +2,7 @@ create table private.projects (
   id              bigint    not null,
   name            text      not null,
   organization_id bigint    not null,
+  metadata        jsonb,
   inserted_at     timestamp not null,
   updated_at      timestamp not null
 );
```

----------------------------------------

TITLE: Setting Statement Timeout for Impersonated Roles (SQL)
DESCRIPTION: This SQL snippet demonstrates how to configure `statement_timeout` for different impersonated roles (`anon`, `authenticated`, `service_role`) using `ALTER ROLE`. This prevents web users or backend processes from running queries that exceed a specified execution time, aborting them to conserve resources.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/www/_blog/2023-07-12-postgrest-11-1-release.mdx#_snippet_0

LANGUAGE: SQL
CODE:
```
-- anonymous users can run queries that take 100 milliseconds max
alter
  role anon
set
  statement_timeout = '100ms';

-- authenticated users can run queries that take 5 seconds max
alter
  role authenticated
set
  statement_timeout = '5s';

-- backend-only users can run queries that take 15 seconds max
alter
  role service_role
set
  statement_timeout = '15s';
```

----------------------------------------

TITLE: Rebasing Local Migrations with Git and Supabase CLI
DESCRIPTION: This sequence of commands pulls the latest changes from Git, creates a new Supabase migration file, renames an existing local migration file to match the new timestamp, and then resets the local database to apply all migrations. This helps resolve conflicts when a teammate merges new migrations.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/deployment/managing-environments.mdx#_snippet_19

LANGUAGE: bash
CODE:
```
git pull
supabase migration new dev_A
# Assume the new file is: supabase/migrations/<t+2>_dev_A.sql
mv <time>_dev_A.sql <t+2>_dev_A.sql
supabase db reset
```

----------------------------------------

TITLE: Configuring Supabase Environment Variables
DESCRIPTION: This snippet shows the required environment variables for connecting your application to Supabase. VITE_SUPABASE_URL specifies the Supabase project URL, and VITE_SUPABASE_ANON_KEY is the public API key. These values are crucial for initializing the Supabase client and enabling communication with your backend.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/ui-library/content/docs/react-router/password-based-auth.mdx#_snippet_0

LANGUAGE: env
CODE:
```
VITE_SUPABASE_URL=
VITE_SUPABASE_ANON_KEY=
```

----------------------------------------

TITLE: Enabling pgvector Extension in Postgres
DESCRIPTION: This SQL command enables the `pgvector` extension within your PostgreSQL database, placing it under the `extensions` schema. This extension is crucial for efficient storage and retrieval of high-dimensional vectors, which are used for semantic search.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/ai/semantic-search.mdx#_snippet_0

LANGUAGE: SQL
CODE:
```
create extension vector
with
  schema extensions;
```

----------------------------------------

TITLE: Handling Supabase User Sign-up in Next.js Server Route (TypeScript)
DESCRIPTION: This Next.js Route Handler (POST), written in TypeScript, processes user sign-up requests. It extracts typed email and password from form data, uses `createRouteHandlerClient` with database types and `next/headers` cookies to interact with Supabase Auth, and redirects the user after successful sign-up. A 301 status is used for redirection from POST to GET.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/auth/auth-helpers/nextjs.mdx#_snippet_9

LANGUAGE: tsx
CODE:
```
import { createRouteHandlerClient } from '@supabase/auth-helpers-nextjs'
import { cookies } from 'next/headers'
import { NextResponse } from 'next/server'

import type { Database } from '@/lib/database.types'

export async function POST(request: Request) {
  const requestUrl = new URL(request.url)
  const formData = await request.formData()
  const email = String(formData.get('email'))
  const password = String(formData.get('password'))
  const cookieStore = cookies()
  const supabase = createRouteHandlerClient<Database>({ cookies: () => cookieStore })

  await supabase.auth.signUp({
    email,
    password,
    options: {
      emailRedirectTo: `${requestUrl.origin}/auth/callback`,
    },
  })

  return NextResponse.redirect(requestUrl.origin, {
    status: 301,
  })
}
```

----------------------------------------

TITLE: Setting Up React Native Expo Project and Installing Dependencies - Shell
DESCRIPTION: This snippet provides shell commands to initialize a new Expo React Native project using the tabs template and install essential dependencies. It includes `@supabase/supabase-js` for Supabase integration, `react-native-url-polyfill` and `base64-arraybuffer` for data handling, `react-native-loading-spinner-overlay` for UI feedback, `@react-native-async-storage/async-storage` for session storage, and Expo-specific packages like `expo-image-picker` and `expo-file-system` for image selection and file operations.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/www/_blog/2023-08-01-react-native-storage.mdx#_snippet_1

LANGUAGE: Shell
CODE:
```
# Create a new Expo app
npx create-expo-app@latest cloudApp --template tabs@49

# Install dependencies
npm i @supabase/supabase-js
npm i react-native-url-polyfill base64-arraybuffer react-native-loading-spinner-overlay @react-native-async-storage/async-storage

# Install Expo packages
npx expo install expo-image-picker
npx expo install expo-file-system
```

----------------------------------------

TITLE: Creating Supabase Storage Access Policy for User Folders - SQL
DESCRIPTION: This SQL policy ensures that users can only access files within their own designated folders in the 'files' storage bucket. It uses `auth.uid()` to restrict access, allowing users to upload, read, and manage files only if the folder name matches their authenticated user ID. This enhances data security and privacy.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/www/_blog/2023-08-01-react-native-storage.mdx#_snippet_0

LANGUAGE: SQL
CODE:
```
CREATE POLICY "Enable storage access for users based on user_id" ON "storage"."objects"
AS PERMISSIVE FOR ALL
TO public
USING (bucket_id = 'files' AND (SELECT auth.uid()::text )= (storage.foldername(name))[1])
WITH CHECK (bucket_id = 'files' AND (SELECT auth.uid()::text) = (storage.foldername(name))[1])
```

----------------------------------------

TITLE: Creating a Server-Side Email Confirmation Endpoint with Supabase (JavaScript)
DESCRIPTION: This Next.js API route handles the email confirmation flow. It extracts `token_hash` and `type` from the URL, uses `supabase.auth.verifyOtp` to exchange the token for a session, and then redirects the user to the account page upon successful verification or to an error page if verification fails. It also handles opting out of Next.js caching for authenticated fetches.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/getting-started/tutorials/with-nextjs.mdx#_snippet_12

LANGUAGE: JavaScript
CODE:
```
import { NextResponse } from 'next/server'

import { createClient } from '@/utils/supabase/server'

// Creating a handler to a GET request to route /auth/confirm
export async function GET(request) {
  const { searchParams } = new URL(request.url)
  const token_hash = searchParams.get('token_hash')
  const type = searchParams.get('type')
  const next = '/account'

  // Create redirect link without the secret token
  const redirectTo = request.nextUrl.clone()
  redirectTo.pathname = next
  redirectTo.searchParams.delete('token_hash')
  redirectTo.searchParams.delete('type')

  if (token_hash && type) {
    const supabase = await createClient()

    const { error } = await supabase.auth.verifyOtp({
      type,
      token_hash,
    })
    if (!error) {
      redirectTo.searchParams.delete('next')
      return NextResponse.redirect(redirectTo)
    }
  }

  // return the user to an error page with some instructions
  redirectTo.pathname = '/error'
  return NextResponse.redirect(redirectTo)
}
```

----------------------------------------

TITLE: Creating Postgres Function and Trigger for New User Profiles
DESCRIPTION: This SQL snippet defines a PostgreSQL function `handle_new_user` and a trigger `on_auth_user_created`. The function automatically inserts a new row into the `public.profiles` table with the user's ID and username (extracted from `raw_user_meta_data`) whenever a new user is inserted into `auth.users`. This ensures that every registered user has a corresponding profile entry.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/www/_blog/2022-06-30-flutter-tutorial-building-a-chat-app.mdx#_snippet_11

LANGUAGE: SQL
CODE:
```
-- Function to create a new row in profiles table upon signup
-- Also copies the username value from metadata
create or replace function handle_new_user() returns trigger as $$
    begin
        insert into public.profiles(id, username)
        values(new.id, new.raw_user_meta_data->>'username');

        return new;
    end;
$$ language plpgsql security definer;

-- Trigger to call `handle_new_user` when new user signs up
create trigger on_auth_user_created
    after insert on auth.users
    for each row
    execute function handle_new_user();
```

----------------------------------------

TITLE: Declaring Supabase Environment Variables in Next.js
DESCRIPTION: This snippet shows how to declare environment variables in a `.env.local` file for a Next.js project. These variables, `NEXT_PUBLIC_SUPABASE_URL` and `NEXT_PUBLIC_SUPABASE_ANON_KEY`, are essential for connecting to your Supabase project's API.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/auth/auth-helpers/nextjs.mdx#_snippet_1

LANGUAGE: bash
CODE:
```
NEXT_PUBLIC_SUPABASE_URL=your-supabase-url
NEXT_PUBLIC_SUPABASE_ANON_KEY=your-supabase-anon-key
```

----------------------------------------

TITLE: Creating `authorize` Function for RLS in PostgreSQL
DESCRIPTION: This PostgreSQL function, `public.authorize`, is designed to implement Role-Based Access Control (RBAC) within Row Level Security (RLS) policies. It retrieves the `user_role` from the user's JWT, then checks if that role has the `requested_permission` by querying the `public.role_permissions` table, returning `true` if authorized.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/database/postgres/custom-claims-and-role-based-access-control-rbac.mdx#_snippet_4

LANGUAGE: sql
CODE:
```
create or replace function public.authorize(
  requested_permission app_permission
)
returns boolean as $$
declare
  bind_permissions int;
  user_role public.app_role;
begin
  -- Fetch user role once and store it to reduce number of calls
  select (auth.jwt() ->> 'user_role')::public.app_role into user_role;

  select count(*)
  into bind_permissions
  from public.role_permissions
  where role_permissions.permission = requested_permission
    and role_permissions.role = user_role;

  return bind_permissions > 0;
end;
$$ language plpgsql stable security definer set search_path = '';
```

----------------------------------------

TITLE: Optimized Supabase Client Query with Explicit Filter in JavaScript
DESCRIPTION: This JavaScript code snippet demonstrates an optimized Supabase client query that selects data from a table and explicitly filters by `user_id`. Even when RLS policies are in place, adding an explicit filter like `.eq('user_id', userId)` allows PostgreSQL to create a more efficient query plan, significantly improving performance.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/database/postgres/row-level-security.mdx#_snippet_18

LANGUAGE: JavaScript
CODE:
```
const { data } = supabase
  .from('table')
  .select()
  .eq('user_id', userId)
```

----------------------------------------

TITLE: Creating a Users Table with Integer Primary Key and Citext Extension
DESCRIPTION: This SQL snippet demonstrates how to create a `users` table with an `integer` primary key. It first enables the `citext` extension for case-insensitive email storage and then defines the `users` table with `id`, `email`, and `name` columns. A unique B-tree index is also created on the `email` column to prevent duplicate email addresses. This setup requires the application to manage ID assignment.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/www/_blog/2022-09-08-choosing-a-postgres-primary-key.mdx#_snippet_0

LANGUAGE: sql
CODE:
```
-- Let's enable access to case-insensitive text
CREATE EXTENSION IF NOT EXISTS citext;

-- Heres a basic users table
CREATE TABLE users (
  id integer PRIMARY KEY,
  email citext NOT NULL CHECK (LENGTH(email) < 255),
  name text NOT NULL
);

-- Let's assume we don't want two users with the exact same email
CREATE UNIQUE INDEX users_email_uniq ON users USING BTREE (email);
```

----------------------------------------

TITLE: Defining a Complex Projects Table with RLS and Public View in PostgreSQL
DESCRIPTION: This snippet illustrates a more complex schema for a `projects` table, created in a `private` schema. It enables Row Level Security (RLS) and defines `INSERT` and `SELECT` policies using custom `auth.can_write` and `auth.can_read` functions for attribute-based access control. A `public.projects` view is also created to expose a filtered subset of data, ensuring users only see projects they have access to.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/www/_blog/2025-04-03-declarative-schemas.mdx#_snippet_1

LANGUAGE: SQL
CODE:
```
create table private.projects (
  id              bigint    not null,
  name            text      not null,
  organization_id bigint    not null,
  inserted_at     timestamp not null,
  updated_at      timestamp not null
);

alter table private.projects
enable row level security;

create policy projects_insert
  on private.projects
  for insert
  to authenticated
with check auth.can_write(project_id);

create policy projects_select
  on private.projects
  for select
  to authenticated
using auth.can_read(project_id);

-- Users can only view the projects that they have access to
create view public.projects as select
  projects.id,
  projects.name,
  projects.organization_id,
  projects.inserted_at,
  projects.updated_at
from private.projects
where auth.can_read(projects.id);
```

----------------------------------------

TITLE: Creating User Profile Insertion Function and Trigger in Supabase - SQL
DESCRIPTION: This SQL snippet defines an `insert_user` PL/pgSQL function and an `on_new_auth_create_profile` trigger. The trigger automatically inserts a new entry into `public.profiles` with the user's ID and email whenever a new user is created in `auth.users`, ensuring profile data synchronization. Necessary grants are also provided.
SOURCE: https://github.com/supabase/supabase/blob/master/examples/realtime/nextjs-authorization-demo/README.md#_snippet_2

LANGUAGE: SQL
CODE:
```
CREATE OR REPLACE FUNCTION insert_user() RETURNS TRIGGER AS
$$
  BEGIN
    INSERT INTO public.profiles (id, email) VALUES (NEW.id, NEW.email); RETURN NEW;
  END;
$$ LANGUAGE plpgsql
   SECURITY DEFINER
   SET search_path = public;

CREATE OR REPLACE TRIGGER "on_new_auth_create_profile"
AFTER INSERT ON auth.users FOR EACH ROW
EXECUTE FUNCTION insert_user();

GRANT EXECUTE ON FUNCTION insert_user () TO supabase_auth_admin;
GRANT INSERT ON TABLE public.profiles TO supabase_auth_admin;
```

----------------------------------------

TITLE: Managing User Profiles with Supabase (Vue.js)
DESCRIPTION: This Vue.js component (`Account.vue`) allows authenticated users to view and update their profile details (username, website, avatar URL) stored in a 'profiles' table. It fetches existing data on mount using `getProfile`, updates it with `updateProfile` via `supabase.from('profiles').upsert`, and provides a `signOut` function using `supabase.auth.signOut`.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/getting-started/tutorials/with-vue-3.mdx#_snippet_5

LANGUAGE: vue
CODE:
```
<script setup>
import { supabase } from '../supabase'
import { onMounted, ref, toRefs } from 'vue'

const props = defineProps(['session'])
const { session } = toRefs(props)

const loading = ref(true)
const username = ref('')
const website = ref('')
const avatar_url = ref('')

onMounted(() => {
  getProfile()
})

async function getProfile() {
  try {
    loading.value = true
    const { user } = session.value

    const { data, error, status } = await supabase
      .from('profiles')
      .select(`username, website, avatar_url`)
      .eq('id', user.id)
      .single()

    if (error && status !== 406) throw error

    if (data) {
      username.value = data.username
      website.value = data.website
      avatar_url.value = data.avatar_url
    }
  } catch (error) {
    alert(error.message)
  } finally {
    loading.value = false
  }
}

async function updateProfile() {
  try {
    loading.value = true
    const { user } = session.value

    const updates = {
      id: user.id,
      username: username.value,
      website: website.value,
      avatar_url: avatar_url.value,
      updated_at: new Date(),
    }

    const { error } = await supabase.from('profiles').upsert(updates)

    if (error) throw error
  } catch (error) {
    alert(error.message)
  } finally {
    loading.value = false
  }
}

async function signOut() {
  try {
    loading.value = true
    const { error } = await supabase.auth.signOut()
    if (error) throw error
  } catch (error) {
    alert(error.message)
  } finally {
    loading.value = false
  }
}
</script>

<template>
  <form class="form-widget" @submit.prevent="updateProfile">
    <div>
      <label for="email">Email</label>
      <input id="email" type="text" :value="session.user.email" disabled />
    </div>
    <div>
      <label for="username">Name</label>
      <input id="username" type="text" v-model="username" />
    </div>
    <div>
      <label for="website">Website</label>
      <input id="website" type="url" v-model="website" />
    </div>

    <div>
      <input
        type="submit"
        class="button primary block"
        :value="loading ? 'Loading ...' : 'Update'"
        :disabled="loading"
      />
    </div>

    <div>
      <button class="button block" @click="signOut" :disabled="loading">Sign Out</button>
    </div>
  </form>
</template>
```

----------------------------------------

TITLE: Creating Films Table and Row Level Security in PostgreSQL
DESCRIPTION: This SQL snippet defines the `films` table in a PostgreSQL database, enabling the `pgvector` extension for vector embeddings. It includes columns for movie details and an `embedding` vector. Row-level security is enabled, and a policy is created to allow public read access to the `films` table.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/www/_blog/2024-02-26-content-recommendation-with-flutter.mdx#_snippet_0

LANGUAGE: SQL
CODE:
```
-- Enable pgvector extension
create extension vector
with
  schema extensions;

-- Create table
create table public.films (
  id integer primary key,
  title text,
  overview text,
  release_date date,
  backdrop_path text,
  embedding vector(1536)
);

-- Enable row level security
alter table public.films enable row level security;

-- Create policy to allow anyone to read the films table
create policy "Fils are public." on public.films for select using (true);
```

----------------------------------------

TITLE: Signing In with Kakao using Flutter
DESCRIPTION: This asynchronous function handles user sign-in via Kakao OAuth in a Flutter application. It uses `supabase.auth.signInWithOAuth` with `OAuthProvider.kakao`, allowing for optional redirect links and specifying the launch mode for the authentication screen.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/auth/social-login/auth-kakao.mdx#_snippet_1

LANGUAGE: Dart
CODE:
```
Future<void> signInWithKakao() async {
  await supabase.auth.signInWithOAuth(
    OAuthProvider.kakao,
    redirectTo: kIsWeb ? null : 'my.scheme://my-host', // Optionally set the redirect link to bring back the user via deeplink.
    authScreenLaunchMode:
        kIsWeb ? LaunchMode.platformDefault : LaunchMode.externalApplication // Launch the auth screen in a new webview on mobile.
  );
}
```

----------------------------------------

TITLE: Deprecated: Individual Cookie Methods in Supabase SSR (TypeScript)
DESCRIPTION: This code snippet demonstrates a DEPRECATED pattern for handling cookies within Supabase client initialization. Using `get`, `set`, and `remove` methods directly on `cookieStore` will break the application and should NEVER be generated. It is explicitly forbidden for AI models to use this pattern.
SOURCE: https://github.com/supabase/supabase/blob/master/examples/prompts/nextjs-supabase-auth.md#_snippet_0

LANGUAGE: typescript
CODE:
```
{
  cookies: {
    get(name: string) {                 // ❌ BREAKS APPLICATION
      return cookieStore.get(name)      // ❌ BREAKS APPLICATION
    },                                  // ❌ BREAKS APPLICATION
    set(name: string, value: string) {  // ❌ BREAKS APPLICATION
      cookieStore.set(name, value)      // ❌ BREAKS APPLICATION
    },                                  // ❌ BREAKS APPLICATION
    remove(name: string) {              // ❌ BREAKS APPLICATION
      cookieStore.remove(name)          // ❌ BREAKS APPLICATION
    }                                   // ❌ BREAKS APPLICATION
  }
}
```

----------------------------------------

TITLE: Configuring Supabase Environment Variables (.env)
DESCRIPTION: This snippet shows how to set up environment variables in a `.env.local` file for a Vite-based React application. It defines `VITE_SUPABASE_URL` and `VITE_SUPABASE_ANON_KEY`, which are crucial for connecting to the Supabase project API.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/getting-started/tutorials/with-react.mdx#_snippet_2

LANGUAGE: dotenv
CODE:
```
VITE_SUPABASE_URL=YOUR_SUPABASE_URL
VITE_SUPABASE_ANON_KEY=YOUR_SUPABASE_ANON_KEY
```

----------------------------------------

TITLE: Initializing Supabase Client (JavaScript)
DESCRIPTION: This JavaScript file (`src/supabaseClient.js`) initializes and exports a Supabase client instance. It retrieves the Supabase URL and anonymous key from environment variables, enabling the application to interact with the Supabase backend securely.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/getting-started/tutorials/with-react.mdx#_snippet_3

LANGUAGE: javascript
CODE:
```
import { createClient } from '@supabase/supabase-js'

const supabaseUrl = import.meta.env.VITE_SUPABASE_URL
const supabaseAnonKey = import.meta.env.VITE_SUPABASE_ANON_KEY

export const supabase = createClient(supabaseUrl, supabaseAnonKey)
```

----------------------------------------

TITLE: Automating Profile Creation on New User Signup - SQL
DESCRIPTION: This SQL snippet defines a `public.handle_new_user` function that inserts a new row into the `public.profiles` table when a new user is created in `auth.users`. It then sets up a trigger, `on_auth_user_created`, to execute this function automatically after each `INSERT` operation on the `auth.users` table, populating the profile with `first_name` and `last_name` from `raw_user_meta_data`.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/auth/managing-user-data.mdx#_snippet_1

LANGUAGE: SQL
CODE:
```
-- inserts a row into public.profiles
create function public.handle_new_user()
returns trigger
language plpgsql
security definer set search_path = ''
as $$
begin
  insert into public.profiles (id, first_name, last_name)
  values (new.id, new.raw_user_meta_data ->> 'first_name', new.raw_user_meta_data ->> 'last_name');
  return new;
end;
$$;

-- trigger the function every time a user is created
create trigger on_auth_user_created
  after insert on auth.users
  for each row execute procedure public.handle_new_user();
```

----------------------------------------

TITLE: Setting Supabase Environment Variables in Next.js
DESCRIPTION: This snippet demonstrates how to configure Supabase API URL and anonymous key as environment variables in a `.env.local` file. These variables are prefixed with `NEXT_PUBLIC_` to make them accessible on the client-side in a Next.js application, ensuring secure and flexible configuration.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/getting-started/tutorials/with-nextjs.mdx#_snippet_3

LANGUAGE: env
CODE:
```
NEXT_PUBLIC_SUPABASE_URL=YOUR_SUPABASE_URL
NEXT_PUBLIC_SUPABASE_ANON_KEY=YOUR_SUPABASE_ANON_KEY
```

----------------------------------------

TITLE: Using InfiniteList Component with Supabase Todo List in React TSX
DESCRIPTION: This example demonstrates how to use the `InfiniteList` component to display a list of 'todos' from a Supabase database. It defines a `renderTodoItem` function to customize the display of each todo task, including a checkbox and task details. It also provides an `orderByInsertedAt` function to sort the query results. The `InfiniteListDemo` component renders the `InfiniteList` with the 'todos' table, custom item renderer, and a page size of 3.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/ui-library/content/docs/infinite-query-hook.mdx#_snippet_3

LANGUAGE: tsx
CODE:
```
'use client'\n\nimport { Checkbox } from '@/components/ui/checkbox'\nimport { InfiniteList } from './infinite-component'\nimport { SupabaseQueryHandler } from '@/hooks/use-infinite-query'\nimport { Database } from '@/lib/supabase.types'\n\ntype TodoTask = Database['public']['Tables']['todos']['Row']\n\n// Define how each item should be rendered\nconst renderTodoItem = (todo: TodoTask) => {\n  return (\n    <div\n      key={todo.id}\n      className="border-b py-3 px-4 hover:bg-muted flex items-center justify-between"\n    >\n      <div className="flex items-center gap-3">\n        <Checkbox defaultChecked={todo.is_complete ?? false} />\n        <div>\n          <span className="font-medium text-sm text-foreground">{todo.task}</span>\n          <div className="text-sm text-muted-foreground">\n            {new Date(todo.inserted_at).toLocaleDateString()}\n          </div>\n        </div>\n      </div>\n    </div>\n  )\n}\n\nconst orderByInsertedAt: SupabaseQueryHandler<'todos'> = (query) => {\n  return query.order('inserted_at', { ascending: false })\n}\n\nexport const InfiniteListDemo = () => {\n  return (\n    <div className="bg-background h-[600px]">\n      <InfiniteList\n        tableName="todos"\n        renderItem={renderTodoItem}\n        pageSize={3}\n        trailingQuery={orderByInsertedAt}\n      />\n    </div>\n  )\n}
```

----------------------------------------

TITLE: Defining Todo Table Schema and Row Level Security Policies
DESCRIPTION: This SQL snippet defines the `todos` table schema, including columns for `id`, `user_id`, `task`, `is_complete`, and `inserted_at`. It then enables Row Level Security (RLS) and defines policies to ensure users can only create, view, update, and delete their own todo items based on their `auth.uid()`.
SOURCE: https://github.com/supabase/supabase/blob/master/examples/todo-list/nextjs-todo-list/README.md#_snippet_1

LANGUAGE: SQL
CODE:
```
create table todos (
  id bigint generated by default as identity primary key,
  user_id uuid references auth.users not null,
  task text check (char_length(task) > 3),
  is_complete boolean default false,
  inserted_at timestamp with time zone default timezone('utc'::text, now()) not null
);

alter table todos enable row level security;

create policy "Individuals can create todos." on todos for
    insert with check ((select auth.uid()) = user_id);

create policy "Individuals can view their own todos. " on todos for
    select using ((select auth.uid()) = user_id);

create policy "Individuals can update their own todos." on todos for
    update using ((select auth.uid()) = user_id);

create policy "Individuals can delete their own todos." on todos for
    delete using ((select auth.uid()) = user_id);
```

----------------------------------------

TITLE: Implementing Infinite Scroll with Supabase and React
DESCRIPTION: This comprehensive code snippet demonstrates a React component that implements infinite scrolling. It fetches data from a Supabase table in chunks, manages scroll events to detect when more data is needed, and uses Framer Motion for smooth animations as new items are loaded. It includes state management for loaded tickets, offset, loading status, and view detection, along with server-side props for initial data loading.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/www/_blog/2023-04-04-infinite-scroll-with-nextjs-framer-motion.mdx#_snippet_7

LANGUAGE: JSX
CODE:
```
import { useEffect, useState, useRef } from 'react'
import { createClient } from '@supabase/supabase-js'
import { debounce } from 'lodash'
import { motion } from 'framer-motion'

const supabase = createClient('supabase-url', 'supabase-key')

export default function TicketsPage({ tickets }) {
  const PAGE_COUNT = 20
  const containerRef = useRef(null)
  const [loadedTickets, setLoadedTickets] = useState(tickets)
  const [offset, setOffset] = useState(1)
  const [isLoading, setIsLoading] = useState(false)
  const [isInView, setIsInView] = useState(false)

  const handleScroll = (container) => {
    if (containerRef.current && typeof window !== 'undefined') {
      const container = containerRef.current
      const { bottom } = container.getBoundingClientRect()
      const { innerHeight } = window
      setIsInView((prev) => bottom <= innerHeight)
    }
  }

  useEffect(() => {
    const handleDebouncedScroll = debounce(() => !isLast && handleScroll(), 200)
    window.addEventListener('scroll', handleScroll)
    return () => {
      window.removeEventListener('scroll', handleScroll)
    }
  }, [])

  useEffect(() => {
    if (isInView) {
      loadMoreTickets(offset)
    }
  }, [isInView])

  const loadMoreTickets = async (offset: number) => {
    setIsLoading(true)
    setOffset((prev) => prev + 1)
    const { data: newTickets } = await fetchTickets(offset, PAGE_COUNT)
    setLoadedTickets((prevTickets) => [...prevTickets, ...newTickets])
    setIsLoading(false)
  }

  const fetchTickets = async (offset) => {
    const from = offset * PAGE_COUNT
    const to = from + PAGE_COUNT - 1

    const { data } = await supabase!
        .from('my_tickets_table')
        .select('*')
        .range(from, to)
        .order('createdAt', { ascending: false })

    return data
  }

  return (
    <div ref={containerRef}>
      {
        loadedTickets.map((ticket, index) => {
          const recalculatedDelay =
            i >= PAGE_COUNT * 2 ? (i - PAGE_COUNT * (offset - 1)) / 15 : i / 15

          return (
            <motion.div
              key={ticket.id}
              initial={{ opacity: 0, y: 20 }}
              animate={{ opacity: 1, y: 0 }}
              transition={{
                duration: 0.4,
                ease: [0.25, 0.25, 0, 1],
                delay: recalculatedDelay,
              }}
            >
              {/* Actual ticket component */}
            </motion.div>
          )
        })
      }
    </div>
  )
}

export const getServerSideProps: GetServerSideProps = async ({ req, res }) => {
  const { data: tickets } = await supabase!
    .from('my_tickets_table')
    .select('*')
    .order('createdAt', { ascending: false })
    .limit(20)

  return {
    props: {
      tickets,
    },
  }
}
```

----------------------------------------

TITLE: Implement Login/Signup Page (Next.js)
DESCRIPTION: Provides a React component for user authentication. It handles email and password input, calls Supabase `signInWithPassword` and `signUp` methods, and redirects the user upon success or logs errors.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/auth/server-side/nextjs.mdx#_snippet_17

LANGUAGE: TSX
CODE:
```
import { useRouter } from 'next/router'
import { useState } from 'react'

import { createClient } from '@/utils/supabase/component'

export default function LoginPage() {
  const router = useRouter()
  const supabase = createClient()

  const [email, setEmail] = useState('')
  const [password, setPassword] = useState('')

  async function logIn() {
    const { error } = await supabase.auth.signInWithPassword({ email, password })
    if (error) {
      console.error(error)
    }
    router.push('/')
  }

  async function signUp() {
    const { error } = await supabase.auth.signUp({ email, password })
    if (error) {
      console.error(error)
    }
    router.push('/')
  }

  return (
    <main>
      <form>
        <label htmlFor="email">Email:</label>
        <input id="email" type="email" value={email} onChange={(e) => setEmail(e.target.value)} />
        <label htmlFor="password">Password:</label>
        <input
          id="password"
          type="password"
          value={password}
          onChange={(e) => setPassword(e.target.value)}
        />
        <button type="button" onClick={logIn}>
          Log in
        </button>
        <button type="button" onClick={signUp}>
          Sign up
        </button>
      </form>
    </main>
  )
}
```

----------------------------------------

TITLE: Supabase Edge Function for OpenAI Embeddings (TypeScript)
DESCRIPTION: This TypeScript code defines a Supabase Edge Function that processes embedding generation jobs. It initializes OpenAI and Postgres clients, defines job schemas using Zod, listens for HTTP POST requests, and processes a batch of embedding jobs by fetching content, generating embeddings via OpenAI, and updating the database. It also handles job failures and reports status.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/ai/automatic-embeddings.mdx#_snippet_9

LANGUAGE: typescript
CODE:
```
// Setup type definitions for built-in Supabase Runtime APIs
import 'jsr:@supabase/functions-js/edge-runtime.d.ts'

// We'll use the OpenAI API to generate embeddings
import OpenAI from 'jsr:@openai/openai'

import { z } from 'npm:zod'

// We'll make a direct Postgres connection to update the document
import postgres from 'https://deno.land/x/postgresjs@v3.4.5/mod.js'

// Initialize OpenAI client
const openai = new OpenAI({
  // We'll need to manually set the `OPENAI_API_KEY` environment variable
  apiKey: Deno.env.get('OPENAI_API_KEY'),
})

// Initialize Postgres client
const sql = postgres(
  // `SUPABASE_DB_URL` is a built-in environment variable
  Deno.env.get('SUPABASE_DB_URL')!
)

const jobSchema = z.object({
  jobId: z.number(),
  id: z.number(),
  schema: z.string(),
  table: z.string(),
  contentFunction: z.string(),
  embeddingColumn: z.string(),
})

const failedJobSchema = jobSchema.extend({
  error: z.string(),
})

type Job = z.infer<typeof jobSchema>
type FailedJob = z.infer<typeof failedJobSchema>

type Row = {
  id: string
  content: unknown
}

const QUEUE_NAME = 'embedding_jobs'

// Listen for HTTP requests
Deno.serve(async (req) => {
  if (req.method !== 'POST') {
    return new Response('expected POST request', { status: 405 })
  }

  if (req.headers.get('content-type') !== 'application/json') {
    return new Response('expected json body', { status: 400 })
  }

  // Use Zod to parse and validate the request body
  const parseResult = z.array(jobSchema).safeParse(await req.json())

  if (parseResult.error) {
    return new Response(`invalid request body: ${parseResult.error.message}`, {
      status: 400,
    })
  }

  const pendingJobs = parseResult.data

  // Track jobs that completed successfully
  const completedJobs: Job[] = []

  // Track jobs that failed due to an error
  const failedJobs: FailedJob[] = []

  async function processJobs() {
    let currentJob: Job | undefined

    while ((currentJob = pendingJobs.shift()) !== undefined) {
      try {
        await processJob(currentJob)
        completedJobs.push(currentJob)
      } catch (error) {
        failedJobs.push({
          ...currentJob,
          error: error instanceof Error ? error.message : JSON.stringify(error),
        })
      }
    }
  }

  try {
    // Process jobs while listening for worker termination
    await Promise.race([processJobs(), catchUnload()])
  } catch (error) {
    // If the worker is terminating (e.g. wall clock limit reached),
    // add pending jobs to fail list with termination reason
    failedJobs.push(
      ...pendingJobs.map((job) => ({
        ...job,
        error: error instanceof Error ? error.message : JSON.stringify(error),
      }))
    )
  }

  // Log completed and failed jobs for traceability
  console.log('finished processing jobs:', {
    completedJobs: completedJobs.length,
    failedJobs: failedJobs.length,
  })

  return new Response(
    JSON.stringify({
      completedJobs,
      failedJobs,
    }),
    {
      // 200 OK response
      status: 200,

      // Custom headers to report job status
      headers: {
        'content-type': 'application/json',
        'x-completed-jobs': completedJobs.length.toString(),
        'x-failed-jobs': failedJobs.length.toString(),
      },
    }
  )
})

/**
 * Generates an embedding for the given text.
 */
async function generateEmbedding(text: string) {
  const response = await openai.embeddings.create({
    model: 'text-embedding-3-small',
    input: text,
  })
  const [data] = response.data

  if (!data) {
    throw new Error('failed to generate embedding')
  }

  return data.embedding
}

/**
 * Processes an embedding job.
 */
async function processJob(job: Job) {
  const { jobId, id, schema, table, contentFunction, embeddingColumn } = job

  // Fetch content for the schema/table/row combination
  const [row]: [Row] = await sql`
    select
      id,
      ${sql(contentFunction)}(t) as content
    from
      ${sql(schema)}.${sql(table)} t
    where
      id = ${id}
  `

  if (!row) {
    throw new Error(`row not found: ${schema}.${table}/${id}`)
  }

  if (typeof row.content !== 'string') {
    throw new Error(`invalid content - expected string: ${schema}.${table}/${id}`)
  }

  const embedding = await generateEmbedding(row.content)

  await sql`
    update
      ${sql(schema)}.${sql(table)}
    set
      ${sql(embeddingColumn)} = ${JSON.stringify(embedding)}
    where
      id = ${id}
  `

  await sql`
    select pgmq.delete(${QUEUE_NAME}, ${jobId}::bigint)
  `
}

/**
 * Returns a promise that rejects if the worker is terminating.
 */
function catchUnload() {
  return new Promise((reject) => {
    addEventListener('beforeunload', (ev: any) => {
      reject(new Error(ev.detail?.reason))
    })
  })
}
```

----------------------------------------

TITLE: Linking Local Supabase Project to Hosted Project
DESCRIPTION: This command connects a local Supabase project to a hosted project on the Supabase platform. It's a necessary step before deploying local changes, including functions and database migrations, to the cloud.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/functions/examples/elevenlabs-generate-speech-stream.mdx#_snippet_10

LANGUAGE: bash
CODE:
```
supabase link
```

----------------------------------------

TITLE: Sending SMS via Twilio with Deno Webhook
DESCRIPTION: This Deno JavaScript snippet implements a webhook to send SMS messages using Twilio. It defines a `sendTextMessage` function that constructs and sends an HTTP POST request to the Twilio API, handling authentication and message body parameters. The main `Deno.serve` function processes incoming webhook payloads, verifies them using `standardwebhooks`, extracts user and SMS data, and then calls `sendTextMessage` to dispatch OTPs, returning appropriate HTTP responses based on the Twilio API's outcome.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/auth/auth-hooks/send-sms-hook.mdx#_snippet_7

LANGUAGE: javascript
CODE:
```
import { Webhook } from 'https://esm.sh/standardwebhooks@1.0.0'
import { readAll } from 'https://deno.land/std/io/read_all.ts'
import { Twilio } from 'https://cdn.skypack.dev/twilio'
import * as base64 from 'https://denopkg.com/chiefbiiko/base64/mod.ts'

const accountSid: string | undefined = Deno.env.get('TWILIO_ACCOUNT_SID')
const authToken: string | undefined = Deno.env.get('TWILIO_AUTH_TOKEN')
const fromNumber: string = Deno.env.get('TWILIO_PHONE_NUMBER')

const sendTextMessage = async (
  messageBody: string,
  accountSid: string | undefined,
  authToken: string | undefined,
  fromNumber: string,
  toNumber: string
): Promise<any> => {
  if (!accountSid || !authToken) {
    console.log('Your Twilio account credentials are missing. Please add them.')
    return
  }
  const url: string = `https://api.twilio.com/2010-04-01/Accounts/${accountSid}/Messages.json`

  const encodedCredentials: string = base64.fromUint8Array(
    new TextEncoder().encode(`${accountSid}:${authToken}`)
  )

  const body: URLSearchParams = new URLSearchParams({
    To: `+${toNumber}`,
    From: fromNumber,
    // Uncomment when testing with a fixed number
    Body: messageBody,
  })

  const response = await fetch(url, {
    method: 'POST',
    headers: {
      'Content-Type': 'application/x-www-form-urlencoded',
      Authorization: `Basic ${encodedCredentials}`,
    },
    body,
  })

  return response.json()
}

Deno.serve(async (req) => {
  const payload = await req.text()
  const base64_secret = Deno.env.get('SEND_SMS_HOOK_SECRET').replace('v1,whsec_', '')
  const headers = Object.fromEntries(req.headers)
  const wh = new Webhook(base64_secret)
  try {
    const { user, sms } = wh.verify(payload, headers)
    const messageBody = `Your OTP is: ${sms.otp}`
    const response = await sendTextMessage(
      messageBody,
      accountSid,
      authToken,
      fromNumber,
      user.phone
    )
    if (response.status !== 'queued') {
      return new Response(
        JSON.stringify({
          error: {
            http_code: response.code,
            message: `Failed to send SMS: ${response.message}. More info: ${response.more_info}`,
          },
        }),
        {
          status: response.status,
          headers: {
            'Content-Type': 'application/json',
          },
        }
      )
    }
    return new Response(
      JSON.stringify({}),
      {
        status: 200,
        headers: {
          'Content-Type': 'application/json',
        },
      }
    )
  } catch (error) {
    return new Response(
      JSON.stringify({
        error: {
          http_code: 500,
          message: `Failed to send sms: ${JSON.stringify(error)}`,
        }
      }),
      {
        status: 500,
        headers: {
          'Content-Type': 'application/json',
        },
      }
    )
  }
})
```

----------------------------------------

TITLE: Accessing All Request Cookies - PostgreSQL
DESCRIPTION: This SQL query retrieves all HTTP cookies sent with the current request as a JSON object using `current_setting('request.cookies', true)`. This is valuable for implementing session management or other cookie-based logic within Postgres functions.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/api/securing-your-api.mdx#_snippet_7

LANGUAGE: SQL
CODE:
```
SELECT current_setting('request.cookies', true)::json;
```

----------------------------------------

TITLE: Configuring Supabase Environment Variables
DESCRIPTION: This snippet defines environment variables for the Supabase API URL and the `anon` key in a `.env` file. These variables are crucial for the client-side application to connect securely to the Supabase project.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/getting-started/tutorials/with-svelte.mdx#_snippet_3

LANGUAGE: bash
CODE:
```
VITE_SUPABASE_URL=YOUR_SUPABASE_URL
VITE_SUPABASE_ANON_KEY=YOUR_SUPABASE_ANON_KEY
```

----------------------------------------

TITLE: Deno Unit Test Example for Supabase Edge Functions
DESCRIPTION: This TypeScript example demonstrates writing unit tests for Supabase Edge Functions using Deno's built-in test runner and @supabase/supabase-js. It includes tests for Supabase client creation, database connectivity, and invoking/asserting the response of an 'hello-world' Edge Function.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/functions/unit-test.mdx#_snippet_1

LANGUAGE: typescript
CODE:
```
// Import required libraries and modules
import { assert, assertEquals } from 'jsr:@std/assert@1'
import { createClient, SupabaseClient } from 'jsr:@supabase/supabase-js@2'

// Will load the .env file to Deno.env
import 'jsr:@std/dotenv/load'

// Set up the configuration for the Supabase client
const supabaseUrl = Deno.env.get('SUPABASE_URL') ?? ''
const supabaseKey = Deno.env.get('SUPABASE_ANON_KEY') ?? ''
const options = {
  auth: {
    autoRefreshToken: false,
    persistSession: false,
    detectSessionInUrl: false,
  },
}

// Test the creation and functionality of the Supabase client
const testClientCreation = async () => {
  var client: SupabaseClient = createClient(supabaseUrl, supabaseKey, options)

  // Verify if the Supabase URL and key are provided
  if (!supabaseUrl) throw new Error('supabaseUrl is required.')
  if (!supabaseKey) throw new Error('supabaseKey is required.')

  // Test a simple query to the database
  const { data: table_data, error: table_error } = await client
    .from('my_table')
    .select('*')
    .limit(1)
  if (table_error) {
    throw new Error('Invalid Supabase client: ' + table_error.message)
  }
  assert(table_data, 'Data should be returned from the query.')
}

// Test the 'hello-world' function
const testHelloWorld = async () => {
  var client: SupabaseClient = createClient(supabaseUrl, supabaseKey, options)

  // Invoke the 'hello-world' function with a parameter
  const { data: func_data, error: func_error } = await client.functions.invoke('hello-world', {
    body: { name: 'bar' },
  })

  // Check for errors from the function invocation
  if (func_error) {
    throw new Error('Invalid response: ' + func_error.message)
  }

  // Log the response from the function
  console.log(JSON.stringify(func_data, null, 2))

  // Assert that the function returned the expected result
  assertEquals(func_data.message, 'Hello bar!')
}

// Register and run the tests
Deno.test('Client Creation Test', testClientCreation)
Deno.test('Hello-world Function Test', testHelloWorld)
```

----------------------------------------

TITLE: Inspecting Failed HTTP Responses in pg_net
DESCRIPTION: This SQL query fetches all failed HTTP responses from the `net._http_response` table within the last 6 hours. It filters for entries where the `status_code` is 400 or greater, or where an `error_msg` is present, ordering the results by creation time. This helps identify and analyze problematic webhook calls.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/troubleshooting/webhook-debugging-guide-M8sk47.mdx#_snippet_4

LANGUAGE: SQL
CODE:
```
select
  *
from net._http_response
where "status_code" >= 400 or "error_msg" is not null
order by created desc;
```

----------------------------------------

TITLE: Creating BRIN Index for Monotonically Increasing Data in Postgres
DESCRIPTION: This SQL command creates a Block Range INdex (BRIN) on the `created_at` column of the `customers` table. BRIN indexes are highly efficient for large tables where data is naturally ordered (e.g., by creation time), offering significantly smaller index sizes compared to B-tree indexes.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/database/query-optimization.mdx#_snippet_5

LANGUAGE: SQL
CODE:
```
create index idx_orders_created_at ON customers using brin(created_at);
```

----------------------------------------

TITLE: Installing Supabase JavaScript Client
DESCRIPTION: This command installs the `@supabase/supabase-js` library, which is the official JavaScript client for interacting with Supabase services, enabling authentication, database, and storage operations within the Svelte application.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/getting-started/tutorials/with-svelte.mdx#_snippet_2

LANGUAGE: bash
CODE:
```
npm install @supabase/supabase-js
```

----------------------------------------

TITLE: Helper Functions for Image Encoding and Embedding Generation (Python)
DESCRIPTION: Defines four helper functions: `readFileAsBase64` for encoding images, `construct_bedrock_image_body` for creating the Bedrock API request body, `get_embedding_from_titan_multimodal` for invoking the Titan model, and `encode_image` to orchestrate the embedding process for a given image file path.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/www/_blog/2024-03-26-semantic-image-search-amazon-bedrock.mdx#_snippet_4

LANGUAGE: Python
CODE:
```
def readFileAsBase64(file_path):
    """Encode image as base64 string."""
    try:
        with open(file_path, "rb") as image_file:
            input_image = base64.b64encode(image_file.read()).decode("utf8")
        return input_image
    except:
        print("bad file name")
        sys.exit(0)


def construct_bedrock_image_body(base64_string):
    """Construct the request body.

    https://docs.aws.amazon.com/bedrock/latest/userguide/model-parameters-titan-embed-mm.html
    """
    return json.dumps(
        {
            "inputImage": base64_string,
            "embeddingConfig": {"outputEmbeddingLength": 1024},
        }
    )


def get_embedding_from_titan_multimodal(body):
    """Invoke the Amazon Titan Model via API request."""
    response = bedrock_client.invoke_model(
        body=body,
        modelId="amazon.titan-embed-image-v1",
        accept="application/json",
        contentType="application/json",
    )

    response_body = json.loads(response.get("body").read())
    print(response_body)
    return response_body["embedding"]


def encode_image(file_path):
    """Generate embedding for the image at file_path."""
    base64_string = readFileAsBase64(file_path)
    body = construct_bedrock_image_body(base64_string)
    emb = get_embedding_from_titan_multimodal(body)
    return emb
```

----------------------------------------

TITLE: Deploying Supabase Edge Function and Setting Secrets (Bash)
DESCRIPTION: This set of commands deploys the `connect-supabase` Supabase Edge Function to the Supabase platform, disabling JWT verification. It then sets the necessary environment variables as secrets on the deployed function using the `./supabase/.env.local` file.
SOURCE: https://github.com/supabase/supabase/blob/master/examples/edge-functions/supabase/functions/connect-supabase/README.md#_snippet_1

LANGUAGE: bash
CODE:
```
supabase functions deploy connect-supabase --no-verify-jwt
supabase secrets set --env-file ./supabase/.env.local
```

----------------------------------------

TITLE: Configure Supabase Environment Variables (.env.local)
DESCRIPTION: Defines environment variables `VITE_SUPABASE_URL` and `VITE_SUPABASE_ANON_KEY` in a `.env.local` file. These variables are used by the Supabase client to connect to your project.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/getting-started/quickstarts/reactjs.mdx#_snippet_2

LANGUAGE: text
CODE:
```
VITE_SUPABASE_URL=<SUBSTITUTE_SUPABASE_URL>
VITE_SUPABASE_ANON_KEY=<SUBSTITUTE_SUPABASE_ANON_KEY>
```

----------------------------------------

TITLE: Initializing a Supabase Project Locally (Bash)
DESCRIPTION: This command initializes a new Supabase project in the current directory, creating a `supabase` folder. This folder contains configuration files and is safe to commit to version control.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/local-development/cli/getting-started.mdx#_snippet_12

LANGUAGE: bash
CODE:
```
supabase init
```

----------------------------------------

TITLE: Displaying Auth/Account Components in Angular (HTML)
DESCRIPTION: This HTML snippet for `AppComponent` conditionally renders either the `app-account` component (if a user session exists) or the `app-auth` component (if no session exists). It passes the `session` object to the `app-account` component as an input.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/getting-started/tutorials/with-angular.mdx#_snippet_8

LANGUAGE: HTML
CODE:
```
<div class="container" style="padding: 50px 0 100px 0">
  <app-account *ngIf="session; else auth" [session]="session"></app-account>
  <ng-template #auth>
    <app-auth></app-auth>
  </ng-template>
</div>
```

----------------------------------------

TITLE: Creating Generic DataTable Component in app/payments/data-table.tsx
DESCRIPTION: This `DataTable` component is a generic React component that leverages `TanStack Table` to manage table state and `shadcn/ui` for rendering. It takes `columns` and `data` as props, dynamically renders table headers and rows, and includes a fallback for no results.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/design-system/content/docs/components/data-table.mdx#_snippet_4

LANGUAGE: tsx
CODE:
```
'use client'

import { ColumnDef, flexRender, getCoreRowModel, useReactTable } from '@tanstack/react-table'

import {
  Table,
  TableBody,
  TableCell,
  TableHead,
  TableHeader,
  TableRow
} from '@/components/ui/table'

interface DataTableProps<TData, TValue> {
  columns: ColumnDef<TData, TValue>[]
  data: TData[]
}

export function DataTable<TData, TValue>({ columns, data }: DataTableProps<TData, TValue>) {
  const table = useReactTable({
    data,
    columns,
    getCoreRowModel: getCoreRowModel()
  })

  return (
    <div className="rounded-md border">
      <Table>
        <TableHeader>
          {table.getHeaderGroups().map((headerGroup) => (
            <TableRow key={headerGroup.id}>
              {headerGroup.headers.map((header) => {
                return (
                  <TableHead key={header.id}>
                    {header.isPlaceholder
                      ? null
                      : flexRender(header.column.columnDef.header, header.getContext())}
                  </TableHead>
                )
              })}
            </TableRow>
          ))}
        </TableHeader>
        <TableBody>
          {table.getRowModel().rows?.length ? (
            table.getRowModel().rows.map((row) => (
              <TableRow key={row.id} data-state={row.getIsSelected() && 'selected'}>
                {row.getVisibleCells().map((cell) => (
                  <TableCell key={cell.id}>
                    {flexRender(cell.column.columnDef.cell, cell.getContext())}
                  </TableCell>
                ))}
              </TableRow>
            ))
          ) : (
            <TableRow>
              <TableCell colSpan={columns.length} className="h-24 text-center">
                No results.
              </TableCell>
            </TableRow>
          )}
        </TableBody>
      </Table>
    </div>
  )
}
```

----------------------------------------

TITLE: Initializing Ionic Vue App and Supabase Session (Vue.js)
DESCRIPTION: This Vue component (`App.vue`) serves as the main entry point for the Ionic application. It initializes the Ionic router outlet and manages the Supabase user session, fetching the current session on mount and listening for authentication state changes to update the user reactive reference.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/getting-started/tutorials/with-ionic-vue.mdx#_snippet_7

LANGUAGE: html
CODE:
```
<template>
  <ion-app>
    <ion-router-outlet />
  </ion-app>
</template>

<script lang="ts">
  import { IonApp, IonRouterOutlet, useIonRouter } from '@ionic/vue'
  import { defineComponent, ref, onMounted } from 'vue'
  import { supabase } from './supabase'

  export default defineComponent({
    name: 'App',
    components: {
      IonApp,
      IonRouterOutlet,
    },
    setup() {
      const router = useIonRouter()
      const user = ref(null)

      onMounted(() => {
        supabase.auth
          .getSession()
          .then((resp) => {
            user.value = resp.data.session?.user ?? null
          })
          .catch((err) => {
            console.log('Error fetching session', err)
          })

        supabase.auth.onAuthStateChange((_event, session) => {
          user.value = session?.user ?? null
        })
      })

      return { user }
    },
  })
</script>
```

----------------------------------------

TITLE: Executing pg_net HTTP POST from a Postgres Trigger in SQL
DESCRIPTION: This SQL example shows how to create a Postgres function that uses net.http_post to send data to an external endpoint (Postman Echo) when a trigger event occurs. The trigger is configured to fire after an update on a specified table, sending both old and new row data as JSON in the request body. This enables real-time integration with external APIs based on database changes.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/database/extensions/pg_net.mdx#_snippet_20

LANGUAGE: SQL
CODE:
```
-- function called by trigger
create or replace function <function_name>()
    returns trigger
    language plpgSQL
as $$
begin
    -- calls pg_net function net.http_post
    -- sends request to postman API
    perform "net"."http_post"(
      'https://postman-echo.com/post'::text,
      jsonb_build_object(
        'old_row', to_jsonb(old.*),
        'new_row', to_jsonb(new.*)
      ),
      headers:='{"Content-Type": "application/json"}'::jsonb
    ) as request_id;
    return new;
END $$;

-- trigger for table update
create trigger <trigger_name>
    after update on <table_name>
    for each row
    execute function <function_name>();
```

----------------------------------------

TITLE: Listening to Supabase Broadcast Changes on Client (JavaScript)
DESCRIPTION: This JavaScript snippet demonstrates how to subscribe to real-time broadcast events using the Supabase client library. It initializes the client, sets Realtime Authorization, creates a private channel for a specific topic (`topic:${gameId}`), and registers listeners for `INSERT`, `UPDATE`, and `DELETE` broadcast events, logging the payload for each.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/realtime/subscribing-to-database-changes.mdx#_snippet_3

LANGUAGE: js
CODE:
```
import { createClient } from '@supabase/supabase-js'
const supabase = createClient('your_project_url', 'your_supabase_api_key')

// ---cut---
const gameId = 'id'
await supabase.realtime.setAuth() // Needed for Realtime Authorization
const changes = supabase
  .channel(`topic:${gameId}`, {
    config: { private: true },
  })
  .on('broadcast', { event: 'INSERT' }, (payload) => console.log(payload))
  .on('broadcast', { event: 'UPDATE' }, (payload) => console.log(payload))
  .on('broadcast', { event: 'DELETE' }, (payload) => console.log(payload))
  .subscribe()
```

----------------------------------------

TITLE: Handling Supabase User Sign-out in Next.js Server Route (TypeScript)
DESCRIPTION: This Next.js Route Handler (POST), written in TypeScript, processes user sign-out requests. It uses `createRouteHandlerClient` with database types and `next/headers` cookies to interact with Supabase Auth and redirects the user to the login page after successful sign-out. A 301 status is used for redirection from POST to GET.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/auth/auth-helpers/nextjs.mdx#_snippet_13

LANGUAGE: tsx
CODE:
```
import { createRouteHandlerClient } from '@supabase/auth-helpers-nextjs'
import { cookies } from 'next/headers'
import { NextResponse } from 'next/server'

import type { Database } from '@/lib/database.types'

export async function POST(request: Request) {
  const requestUrl = new URL(request.url)
  const cookieStore = cookies()
  const supabase = createRouteHandlerClient<Database>({ cookies: () => cookieStore })

  await supabase.auth.signOut()

  return NextResponse.redirect(`${requestUrl.origin}/login`, {
    status: 301,
  })
}
```

----------------------------------------

TITLE: Creating Todo with Supabase in Next.js Route Handler (TypeScript)
DESCRIPTION: This TypeScript snippet illustrates handling a POST request in a Next.js Route Handler to insert a new todo item into Supabase. It leverages `createRouteHandlerClient` with `cookies()` and a `Database` type for enhanced type safety, enabling authenticated server-side data operations.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/auth/auth-helpers/nextjs.mdx#_snippet_20

LANGUAGE: TypeScript
CODE:
```
import { createRouteHandlerClient } from '@supabase/auth-helpers-nextjs'
import { NextResponse } from 'next/server'
import { cookies } from 'next/headers'

import type { Database } from '@/lib/database.types'

export async function POST(request: Request) {
  const { title } = await request.json()
  const cookieStore = cookies()
  const supabase = createRouteHandlerClient<Database>({ cookies: () => cookieStore })
  const { data } = await supabase.from('todos').insert({ title }).select()
  return NextResponse.json(data)
}
```

----------------------------------------

TITLE: React Component for Unenrolling Supabase MFA Factors
DESCRIPTION: This React `UnenrollMFA` component provides a user interface to list and unenroll Multi-Factor Authentication (MFA) factors. It fetches existing factors using `supabase.auth.mfa.listFactors()` on component mount, displays them in a table, and allows users to unenroll a selected factor by its ID via `supabase.auth.mfa.unenroll()`. This component requires `useState` and `useEffect` from React, and the Supabase client.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/auth/auth-mfa.mdx#_snippet_1

LANGUAGE: tsx
CODE:
```
/**
 * UnenrollMFA shows a simple table with the list of factors together with a button to unenroll.
 * When a user types in the factorId of the factor that they wish to unenroll and clicks unenroll
 * the corresponding factor will be unenrolled.
 */
export function UnenrollMFA() {
  const [factorId, setFactorId] = useState('')
  const [factors, setFactors] = useState([])
  const [error, setError] = useState('') // holds an error message

  useEffect(() => {
    ;(async () => {
      const { data, error } = await supabase.auth.mfa.listFactors()
      if (error) {
        throw error
      }

      setFactors([...data.totp, ...data.phone])
    })()
  }, [])

  return (
    <>
      {error && <div className="error">{error}</div>}
      <tbody>
        <tr>
          <td>Factor ID</td>
          <td>Friendly Name</td>
          <td>Factor Status</td>
          <td>Phone Number</td>
        </tr>
        {factors.map((factor) => (
          <tr>
            <td>{factor.id}</td>
            <td>{factor.friendly_name}</td>
            <td>{factor.factor_type}</td>
            <td>{factor.status}</td>
            <td>{factor.phone}</td>
          </tr>
        ))}
      </tbody>
      <input type="text" value={verifyCode} onChange={(e) => setFactorId(e.target.value.trim())} />
      <button onClick={() => supabase.auth.mfa.unenroll({ factorId })}>Unenroll</button>
    </>
  )
}
```

----------------------------------------

TITLE: Protecting Pages Router Pages with Supabase getServerSideProps (TS)
DESCRIPTION: This snippet shows how to create a protected page in Next.js Pages Router using `getServerSideProps`. It initializes a server-side Supabase client (`@/utils/supabase/server-props`) and calls `supabase.auth.getUser()` to check for an authenticated user. If the user is not found or an error occurs, the user is redirected to the home page. The authenticated user data is passed as a prop to the page component.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/auth/server-side/nextjs.mdx#_snippet_19

LANGUAGE: ts
CODE:
```
import type { User } from '@supabase/supabase-js'
import type { GetServerSidePropsContext } from 'next'

import { createClient } from '@/utils/supabase/server-props'

export default function PrivatePage({ user }: { user: User }) {
  return <h1>Hello, {user.email || 'user'}!</h1>
}

export async function getServerSideProps(context: GetServerSidePropsContext) {
  const supabase = createClient(context)

  const { data, error } = await supabase.auth.getUser()

  if (error || !data) {
    return {
      redirect: {
        destination: '/',
        permanent: false,
      },
    }
  }

  return {
    props: {
      user: data.user,
    },
  }
}
```

----------------------------------------

TITLE: Analyzing Query Performance with EXPLAIN ANALYZE in PostgreSQL
DESCRIPTION: This SQL command uses `EXPLAIN (ANALYZE)` to show the execution plan and actual runtime statistics for a `SELECT` query. It helps in understanding how PostgreSQL's query planner executes a query, identifying performance bottlenecks, and verifying the effectiveness of indexes.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/www/_blog/2021-02-27-cracking-postgres-interview.mdx#_snippet_2

LANGUAGE: SQL
CODE:
```
EXPLAIN (ANALYZE) SELECT *
FROM students
WHERE surname = 'Krobb';
```

----------------------------------------

TITLE: Using `explain()` with Supabase Client (TypeScript)
DESCRIPTION: This TypeScript snippet demonstrates how to chain the `explain()` method to a Supabase client query. It retrieves the execution plan for a `SELECT` query on the `instruments` table, allowing developers to analyze how the query will be executed by Postgres. The result will contain the execution plan details.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/database/debugging-performance.mdx#_snippet_1

LANGUAGE: TypeScript
CODE:
```
const { data, error } = await supabase
  .from('instruments')
  .select()
  .explain()
```

----------------------------------------

TITLE: Defining Supabase Tables and Initial Functions - SQL
DESCRIPTION: This snippet defines the core database schema for a Trello-like application, including `boards`, `lists`, `cards`, `users`, and `user_boards` tables. It also sets `replica identity full` for `cards` and `lists` to enable full row data in realtime changes and creates a `get_boards_for_authenticated_user` function for Row Level Security.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/www/_blog/2022-08-24-building-a-realtime-trello-board-with-supabase-and-angular.mdx#_snippet_0

LANGUAGE: SQL
CODE:
```
drop table if exists user_boards;
drop table if exists cards;
drop table if exists lists;
drop table if exists boards;
drop table if exists users;

-- Create boards table
create table boards (
  id bigint generated by default as identity primary key,
  creator uuid references auth.users not null default auth.uid(),
  title text default 'Untitled Board',
  created_at timestamp with time zone default timezone('utc'::text, now()) not null
);

-- Create lists table
create table lists (
  id bigint generated by default as identity primary key,
  board_id bigint references boards ON DELETE CASCADE not null,
  title text default '',
  position int not null default 0,
  created_at timestamp with time zone default timezone('utc'::text, now()) not null
);

-- Create Cards table
create table cards (
  id bigint generated by default as identity primary key,
  list_id bigint references lists ON DELETE CASCADE not null,
  board_id bigint references boards ON DELETE CASCADE not null,
  position int not null default 0,
  title text default '',
  description text check (char_length(description) > 0),
  assigned_to uuid references auth.users,
  done boolean default false,
  created_at timestamp with time zone default timezone('utc'::text, now()) not null
);

-- Many to many table for user <-> boards relationship
create table user_boards (
  id bigint generated by default as identity primary key,
  user_id uuid references auth.users ON DELETE CASCADE not null default auth.uid(),
  board_id bigint references boards ON DELETE CASCADE
);

-- User ID lookup table
create table users (
  id uuid not null primary key,
  email text
);

-- Make sure deleted records are included in realtime
alter table cards replica identity full;
alter table lists replica identity full;

-- Function to get all user boards
create or replace function get_boards_for_authenticated_user()
returns setof bigint
language sql
security definer
set search_path = ''
stable
as $$
    select board_id
    from public.user_boards
    where user_id = auth.uid()
$$;
```

----------------------------------------

TITLE: Enforcing AAL for MFA Opted-in Supabase Users (SQL)
DESCRIPTION: This SQL RLS policy enforces a higher Authenticator Assurance Level (AAL) for users who have opted into Multi-Factor Authentication (MFA). If a user has at least one verified MFA factor, only 'aal2' is accepted; otherwise, 'aal1' or 'aal2' is allowed. The `as restrictive` clause ensures this policy takes precedence, and `<@` is PostgreSQL's 'contained in' operator.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/auth/auth-mfa.mdx#_snippet_4

LANGUAGE: sql
CODE:
```
create policy "Policy name."
  on table_name
  as restrictive -- very important!
  to authenticated
  using (
    array[(select auth.jwt()->>'aal')] <@ (
      select
          case
            when count(id) > 0 then array['aal2']
            else array['aal1', 'aal2']
          end as aal
        from auth.mfa_factors
        where ((select auth.uid()) = user_id) and status = 'verified'
    ));
```

----------------------------------------

TITLE: Supabase Query with Large 'in' Clause - JavaScript
DESCRIPTION: This JavaScript snippet demonstrates a Supabase query using a `not('id', 'in', ...)` clause with a very large array of IDs. This type of query is specifically highlighted as a common cause for exceeding the 16KB URL/header limit, leading to Cloudflare 520 errors. It illustrates the problem that RPCs are designed to solve.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/troubleshooting/fixing-520-errors-in-the-database-rest-api-Ur5-B2.mdx#_snippet_2

LANGUAGE: javascript
CODE:
```
const { data, error } = await supabase
  .from('countries')
  .select()
  .not('id', 'in', '(5,6,7,8,9,...10,000)')
```

----------------------------------------

TITLE: Initializing Supabase Authentication in Angular AppComponent
DESCRIPTION: This snippet updates the root Angular component (`AppComponent`) to listen for Supabase authentication changes. It injects `SupabaseService` and `Router`, and if a user session exists, it navigates the user to the `/account` route. This ensures the application responds to user login/logout states.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/getting-started/tutorials/with-ionic-angular.mdx#_snippet_5

LANGUAGE: TypeScript
CODE:
```
import { Component } from '@angular/core'
import { Router } from '@angular/router'
import { SupabaseService } from './supabase.service'

@Component({
  selector: 'app-root',
  template: `
    <ion-app>
      <ion-router-outlet></ion-router-outlet>
    </ion-app>
  `,
  styleUrls: ['app.component.scss'],
})
export class AppComponent {
  constructor(
    private supabase: SupabaseService,
    private router: Router
  ) {
    this.supabase.authChanges((_, session) => {
      console.log(session)
      if (session?.user) {
        this.router.navigate(['/account'])
      }
    })
  }
}
```

----------------------------------------

TITLE: Implementing Supabase Service for Angular (TypeScript)
DESCRIPTION: This TypeScript service (SupabaseService) integrates Supabase functionality into an Ionic Angular application. It initializes the Supabase client, provides methods for user authentication (sign-in with OTP, sign-out, auth state changes), profile management (fetching and updating), and avatar storage operations (uploading and downloading). It also includes utility methods for displaying Ionic loading indicators and toast messages.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/getting-started/tutorials/with-ionic-angular.mdx#_snippet_2

LANGUAGE: typescript
CODE:
```
import { Injectable } from '@angular/core'
import { LoadingController, ToastController } from '@ionic/angular'
import { AuthChangeEvent, createClient, Session, SupabaseClient } from '@supabase/supabase-js'
import { environment } from '../environments/environment'

export interface Profile {
  username: string
  website: string
  avatar_url: string
}

@Injectable({
  providedIn: 'root',
})
export class SupabaseService {
  private supabase: SupabaseClient

  constructor(
    private loadingCtrl: LoadingController,
    private toastCtrl: ToastController
  ) {
    this.supabase = createClient(environment.supabaseUrl, environment.supabaseKey)
  }

  get user() {
    return this.supabase.auth.getUser().then(({ data }) => data?.user)
  }

  get session() {
    return this.supabase.auth.getSession().then(({ data }) => data?.session)
  }

  get profile() {
    return this.user
      .then((user) => user?.id)
      .then((id) =>
        this.supabase.from('profiles').select(`username, website, avatar_url`).eq('id', id).single()
      )
  }

  authChanges(callback: (event: AuthChangeEvent, session: Session | null) => void) {
    return this.supabase.auth.onAuthStateChange(callback)
  }

  signIn(email: string) {
    return this.supabase.auth.signInWithOtp({ email })
  }

  signOut() {
    return this.supabase.auth.signOut()
  }

  async updateProfile(profile: Profile) {
    const user = await this.user
    const update = {
      ...profile,
      id: user?.id,
      updated_at: new Date(),
    }

    return this.supabase.from('profiles').upsert(update)
  }

  downLoadImage(path: string) {
    return this.supabase.storage.from('avatars').download(path)
  }

  uploadAvatar(filePath: string, file: File) {
    return this.supabase.storage.from('avatars').upload(filePath, file)
  }

  async createNotice(message: string) {
    const toast = await this.toastCtrl.create({ message, duration: 5000 })
    await toast.present()
  }

  createLoader() {
    return this.loadingCtrl.create()
  }
}
```

----------------------------------------

TITLE: Enabling Row Level Security and MFA Policy (SQL)
DESCRIPTION: This SQL snippet enables Row Level Security (RLS) on the `private_posts` table and defines a policy. The policy restricts `SELECT` access to authenticated users whose Authenticator Assurance Level (`aal`) is 'aal2', meaning they have completed the Multi-Factor Authentication flow.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/www/_blog/2023-05-04-flutter-multi-factor-authentication.mdx#_snippet_12

LANGUAGE: SQL
CODE:
```
-- Enable RLS for private_posts table
alter table
  public.private_posts enable row level security;

-- Create a policy that only allows read if they user has signed in via MFA
create policy "Users can view private_posts if they have signed in via MFA" on public.private_posts for
select
  to authenticated using ((select auth.jwt() - >> 'aal') = 'aal2');
```

----------------------------------------

TITLE: Deploying All Supabase Edge Functions (Bash)
DESCRIPTION: This command deploys all Edge Functions defined in the local project to the linked remote Supabase project. Docker Desktop is required for this operation (CLI version 1.123.4 and above).
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/functions/deploy.mdx#_snippet_3

LANGUAGE: bash
CODE:
```
supabase functions deploy
```

----------------------------------------

TITLE: Initializing Supabase Project
DESCRIPTION: This snippet shows how to initialize a new Supabase project within your repository using the Supabase CLI. This command sets up the necessary configuration files and directories for local development.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/local-development.mdx#_snippet_1

LANGUAGE: sh
CODE:
```
npx supabase init
```

LANGUAGE: sh
CODE:
```
yarn supabase init
```

LANGUAGE: sh
CODE:
```
pnpx supabase init
```

LANGUAGE: sh
CODE:
```
supabase init
```

----------------------------------------

TITLE: Configuring Supabase Client with Custom Options (TypeScript)
DESCRIPTION: This snippet illustrates the updated `createClient` configuration in Supabase.js v2. It shows how to specify custom schema and session persistence options within the new `db` and `auth` nested objects, providing a more structured approach compared to v1.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/docs/ref/javascript/v1/upgrade-guide.mdx#_snippet_1

LANGUAGE: ts
CODE:
```
const supabase = createClient(SUPABASE_URL, SUPABASE_ANON_KEY, {
  schema: 'custom',
  persistSession: false,
})
```

LANGUAGE: ts
CODE:
```
const supabase = createClient(SUPABASE_URL, SUPABASE_ANON_KEY, {
  db: {
    schema: 'custom',
  },
  auth: {
    persistSession: true,
  },
})
```

----------------------------------------

TITLE: Granting Table Permissions within Custom API Schema (SQL)
DESCRIPTION: These SQL commands demonstrate how to grant specific data manipulation permissions (SELECT, INSERT, UPDATE, DELETE) on tables within the 'api' schema to the 'anon' and 'authenticated' roles. This allows for fine-grained control over which data operations are permitted for different user types through the Data API. Replace <your_table> with the actual table name.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/database/hardening-data-api.mdx#_snippet_2

LANGUAGE: SQL
CODE:
```
grant select on table api.<your_table> to anon;
grant select, insert, update, delete on table api.<your_table> to authenticated;
```

----------------------------------------

TITLE: Accessing Custom Schema Tables with Supabase JS Client
DESCRIPTION: This JavaScript snippet demonstrates how to explicitly specify a custom schema when querying a table using the Supabase client library. It's crucial for accessing tables not located in the default public schema, preventing 42P01 errors related to search path issues.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/troubleshooting/resolving-42p01-relation-does-not-exist-error-W4_9-V.mdx#_snippet_1

LANGUAGE: javascript
CODE:
```
const { data, error } = await supabase.schema('myschema').from('mytable').select()
```

----------------------------------------

TITLE: Creating Todo in Next.js Edge Route Handler (TypeScript)
DESCRIPTION: This TypeScript snippet demonstrates handling a POST request in a Next.js Route Handler optimized for the Edge runtime. It uses `createRouteHandlerClient` with `cookies()` and a `Database` type for type-safe, low-latency data insertion into Supabase.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/auth/auth-helpers/nextjs.mdx#_snippet_24

LANGUAGE: TypeScript
CODE:
```
import { createRouteHandlerClient } from '@supabase/auth-helpers-nextjs'
import { NextResponse } from 'next/server'
import { cookies } from 'next/headers'

import type { Database } from '@/lib/database.types'

export const runtime = 'edge'
export const dynamic = 'force-dynamic'

export async function POST(request: Request) {
  const { title } = await request.json()
  const cookieStore = cookies()

  const supabase = createRouteHandlerClient<Database>({
    cookies: () => cookieStore,
  })

  const { data } = await supabase.from('todos').insert({ title }).select()
  return NextResponse.json(data)
}
```

----------------------------------------

TITLE: Creating HNSW Index for Vector Search (SQL)
DESCRIPTION: This SQL snippet illustrates the general syntax for creating an HNSW index on a specified vectorized column in a PostgreSQL table. It highlights the importance of matching the index's search type (Euclidean, negative inner product, or cosine distance) with the type of search operation used in queries for index utilization.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/troubleshooting/increase-vector-lookup-speeds-by-applying-an-hsnw-index-ohLHUM.mdx#_snippet_0

LANGUAGE: sql
CODE:
```
CREATE INDEX <custom name of index> ON <table name> USING hnsw (<vectorized column> <search type>);
```

----------------------------------------

TITLE: Initializing Angular App and Installing Supabase Client
DESCRIPTION: These bash commands initialize a new Angular project named 'supabase-angular' without routing, using CSS for styling, and as a non-standalone application. Following the project creation, it navigates into the new directory and installs the `@supabase/supabase-js` library, which is the official JavaScript client for interacting with Supabase services.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/getting-started/tutorials/with-angular.mdx#_snippet_0

LANGUAGE: bash
CODE:
```
npx ng new supabase-angular --routing false --style css --standalone false
cd supabase-angular
```

LANGUAGE: bash
CODE:
```
npm install @supabase/supabase-js
```

----------------------------------------

TITLE: Creating a Postgres Function for Optimized Object Listing
DESCRIPTION: This PostgreSQL function, `list_objects`, is designed to optimize the retrieval of storage objects by directly querying the `storage.objects` table. It avoids the overhead of computing the full hierarchy, making it significantly faster than the generic `supabase.storage.list()` method for large numbers of objects. The function accepts `bucketid`, `prefix` for filtering, and `limits` and `offsets` for pagination.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/storage/production/scaling.mdx#_snippet_0

LANGUAGE: sql
CODE:
```
create or replace function list_objects(
    bucketid text,
    prefix text,
    limits int default 100,
    offsets int default 0
) returns table (
    name text,
    id uuid,
    updated_at timestamptz,
    created_at timestamptz,
    last_accessed_at timestamptz,
    metadata jsonb
) as $$
begin
    return query SELECT
        objects.name,
        objects.id,
        objects.updated_at,
        objects.created_at,
        objects.last_accessed_at,
        objects.metadata
    FROM storage.objects
    WHERE objects.name like prefix || '%'
    AND bucket_id = bucketid
    ORDER BY name ASC
    LIMIT limits
    OFFSET offsets;
end;
$$ language plpgsql stable;
```

----------------------------------------

TITLE: Implementing Infinite Scroll with Supabase in React TSX
DESCRIPTION: This React component provides an infinite scrolling list by integrating with a Supabase `useInfiniteQuery` hook. It uses the Intersection Observer API to detect when the user scrolls near the end of the list, triggering the `fetchNextPage` function. The component is generic and can be used with any Supabase table, allowing custom rendering of items, no results messages, end messages, and skeleton loaders. It requires `SupabaseQueryHandler`, `SupabaseTableData`, `SupabaseTableName`, and `useInfiniteQuery` from `@/hooks/use-infinite-query`.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/ui-library/content/docs/infinite-query-hook.mdx#_snippet_2

LANGUAGE: tsx
CODE:
```
'use client'\n\nimport { cn } from '@/lib/utils'\nimport {\n  SupabaseQueryHandler,\n  SupabaseTableData,\n  SupabaseTableName,\n  useInfiniteQuery,\n} from '@/hooks/use-infinite-query'\nimport * as React from 'react'\n\ninterface InfiniteListProps<TableName extends SupabaseTableName> {\n  tableName: TableName\n  columns?: string\n  pageSize?: number\n  trailingQuery?: SupabaseQueryHandler<TableName>\n  renderItem: (item: SupabaseTableData<TableName>, index: number) => React.ReactNode\n  className?: string\n  renderNoResults?: () => React.ReactNode\n  renderEndMessage?: () => React.ReactNode\n  renderSkeleton?: (count: number) => React.ReactNode\n}\n\nconst DefaultNoResults = () => (\n  <div className="text-center text-muted-foreground py-10">No results.</div>\n)\n\nconst DefaultEndMessage = () => (\n  <div className="text-center text-muted-foreground py-4 text-sm">You&apos;ve reached the end.</div>\n)\n\nconst defaultSkeleton = (count: number) => (\n  <div className="flex flex-col gap-2 px-4">\n    {Array.from({ length: count }).map((_, index) => (\n      <div key={index} className="h-4 w-full bg-muted animate-pulse" />\n    ))}\n  </div>\n)\n\nexport function InfiniteList<TableName extends SupabaseTableName>({\n  tableName,\n  columns = '*',
  pageSize = 20,\n  trailingQuery,\n  renderItem,\n  className,\n  renderNoResults = DefaultNoResults,\n  renderEndMessage = DefaultEndMessage,\n  renderSkeleton = defaultSkeleton,\n}: InfiniteListProps<TableName>) {\n  const { data, isFetching, hasMore, fetchNextPage, isSuccess } = useInfiniteQuery({\n    tableName,\n    columns,\n    pageSize,\n    trailingQuery,\n  })\n\n  // Ref for the scrolling container\n  const scrollContainerRef = React.useRef<HTMLDivElement>(null)\n\n  // Intersection observer logic - target the last rendered *item* or a dedicated sentinel\n  const loadMoreSentinelRef = React.useRef<HTMLDivElement>(null)\n  const observer = React.useRef<IntersectionObserver | null>(null)\n\n  React.useEffect(() => {\n    if (observer.current) observer.current.disconnect()\n\n    observer.current = new IntersectionObserver(\n      (entries) => {\n        if (entries[0].isIntersecting && hasMore && !isFetching) {\n          fetchNextPage()\n        }\n      },\n      {\n        root: scrollContainerRef.current, // Use the scroll container for scroll detection\n        threshold: 0.1, // Trigger when 10% of the target is visible\n        rootMargin: '0px 0px 100px 0px', // Trigger loading a bit before reaching the end\n      }\n    )\n\n    if (loadMoreSentinelRef.current) {\n      observer.current.observe(loadMoreSentinelRef.current)\n    }\n\n    return () => {\n      if (observer.current) observer.current.disconnect()\n    }\n  }, [isFetching, hasMore, fetchNextPage])\n\n  return (\n    <div ref={scrollContainerRef} className={cn('relative h-full overflow-auto', className)}>\n      <div>\n        {isSuccess && data.length === 0 && renderNoResults()}\n\n        {data.map((item, index) => renderItem(item, index))}\n\n        {isFetching && renderSkeleton && renderSkeleton(pageSize)}\n\n        <div ref={loadMoreSentinelRef} style={{ height: '1px' }} />\n\n        {!hasMore && data.length > 0 && renderEndMessage()}\n      </div>\n    </div>\n  )\n}
```

----------------------------------------

TITLE: Handling Google Sign-in with Supabase signInWithIdToken (TypeScript/React)
DESCRIPTION: This snippet illustrates how to process an authentication response from Google's Sign-in Button or Google One Tap. It invokes the `supabase.auth.signInWithIdToken()` method, passing the ID token from the Google response and an optional nonce, to authenticate the user with Supabase.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/www/_blog/2023-06-27-native-mobile-auth.mdx#_snippet_0

LANGUAGE: tsx
CODE:
```
async function handleSignInWithGoogle(response) {
  const { data, error } = await supabase.auth.signInWithIdToken({
    token: response.credential,
    nonce: 'NONCE' // must be the same one as provided in data-nonce (if any)
  })
}
```

----------------------------------------

TITLE: Implementing User Registration with Supabase in Flutter
DESCRIPTION: This Flutter widget defines the user registration page, allowing users to sign up with their email and password. It utilizes `supabase.auth.signUp()` to create a new account and includes an `emailRedirectTo` option to guide users to the MFA enrollment page after email confirmation. Error handling for authentication exceptions is also included.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/www/_blog/2023-05-04-flutter-multi-factor-authentication.mdx#_snippet_7

LANGUAGE: Dart
CODE:
```
import 'package:flutter/material.dart';
import 'package:go_router/go_router.dart';
import 'package:mfa_app/main.dart';
import 'package:mfa_app/pages/auth/login_page.dart';
import 'package:mfa_app/pages/mfa/enroll_page.dart';
import 'package:supabase_flutter/supabase_flutter.dart';

class RegisterPage extends StatefulWidget {
  static const route = '/auth/register';

  const RegisterPage({super.key});

  @override
  State<RegisterPage> createState() => _RegisterPageState();
}

class _RegisterPageState extends State<RegisterPage> {
  final _emailController = TextEditingController();
  final _passwordController = TextEditingController();

  bool _isLoading = false;

  @override
  void dispose() {
    _emailController.dispose();
    _passwordController.dispose();
    super.dispose();
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(title: const Text('Register')),
      body: ListView(
        padding: const EdgeInsets.symmetric(horizontal: 20, vertical: 24),
        children: [
          TextFormField(
            controller: _emailController,
            decoration: const InputDecoration(
              label: Text('Email'),
            ),
          ),
          const SizedBox(height: 16),
          TextFormField(
            controller: _passwordController,
            decoration: const InputDecoration(
              label: Text('Password'),
            ),
            obscureText: true,
          ),
          const SizedBox(height: 16),
          ElevatedButton(
            onPressed: () async {
              try {
                setState(() {
                  _isLoading = true;
                });
                final email = _emailController.text.trim();
                final password = _passwordController.text.trim();
                await supabase.auth.signUp(
                  email: email,
                  password: password,
                  emailRedirectTo:
                      'mfa-app://callback${MFAEnrollPage.route}', // redirect the user to setup MFA page after email confirmation
                );
                if (mounted) {
                  ScaffoldMessenger.of(context).showSnackBar(
                      const SnackBar(content: Text('Check your inbox.')));
                }
              } on AuthException catch (error) {
                ScaffoldMessenger.of(context)
                    .showSnackBar(SnackBar(content: Text(error.message)));
              } catch (error) {
                ScaffoldMessenger.of(context).showSnackBar(
                    const SnackBar(content: Text('Unexpected error occurred')));
              }
              if (mounted) {
                setState(() {
                  _isLoading = false;
                });
              }
            },
            child: _isLoading
                ? const SizedBox(
                    height: 24,
                    width: 24,
                    child: Center(
                        child: CircularProgressIndicator(color: Colors.white)),
                  )
                : const Text('Register'),
          ),
          const SizedBox(height: 16),
          TextButton(
            onPressed: () => context.push(LoginPage.route),
            child: const Text('I already have an account'),
          )
        ],
      ),
    );
  }
}
```

----------------------------------------

TITLE: Initializing Supabase Server Client (Next.js Server-side)
DESCRIPTION: This utility function `createClient` configures the Supabase server client for Next.js server components. It integrates with `next/headers` to manage cookies, ensuring session persistence and proper authentication on the server.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/auth/server-side/creating-a-client.mdx#_snippet_13

LANGUAGE: TypeScript
CODE:
```
import { createServerClient } from '@supabase/ssr'
import { cookies } from 'next/headers'

export async function createClient() {
  const cookieStore = await cookies()

  return createServerClient(
    process.env.NEXT_PUBLIC_SUPABASE_URL!,
    process.env.NEXT_PUBLIC_SUPABASE_ANON_KEY!,
    {
      cookies: {
        getAll() {
          return cookieStore.getAll()
        },
        setAll(cookiesToSet) {
          try {
            cookiesToSet.forEach(({ name, value, options }) =>
              cookieStore.set(name, value, options)
            )
          } catch {
            // The `setAll` method was called from a Server Component.
            // This can be ignored if you have middleware refreshing
            // user sessions.
          }
        }
      }
    }
  )
}
```

----------------------------------------

TITLE: Paginated Data Loading with Supabase `range`
DESCRIPTION: This code implements the core pagination logic for infinite scrolling. `loadMoreTickets` is triggered when `isInView` is true, incrementing the `offset` and fetching new data using Supabase's `range` method. `fetchTickets` calculates the `from` and `to` values based on `offset` and `PAGE_COUNT`, then appends the new tickets to the `loadedTickets` state.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/www/_blog/2023-04-04-infinite-scroll-with-nextjs-framer-motion.mdx#_snippet_4

LANGUAGE: jsx
CODE:
```
export default function TicketsPage() {
  const PAGE_COUNT = 20
  const [offset, setOffset] = useState(1)
  const [isLoading, setIsLoading] = useState(false)
  const [isInView, setIsInView] = useState(false)

  useEffect(() => {
    if (isInView) {
      loadMoreTickets(offset)
    }
  }, [isInView])

  const loadMoreTickets = async (offset: number) => {
    setIsLoading(true)
    // Every time we fetch, we want to increase
    // the offset to load fresh tickets
    setOffset((prev) => prev + 1)
    const { data: newTickets } = await fetchTickets(offset, PAGE_COUNT)
    // Merge new tickets with all previously loaded
    setLoadedTickets((prevTickets) => [...prevTickets, ...newTickets])
    setIsLoading(false)
  }

  const fetchTickets = async (offset, limit) => {
    const from = offset * PAGE_COUNT
    const to = from + PAGE_COUNT - 1

    const { data } = await supabase!
        .from('my_tickets_table')
        .select('*')
        .range(from, to)
        .order('createdAt', { ascending: false })


    return data
  }
}
```

----------------------------------------

TITLE: Creating Database Functions and Triggers
DESCRIPTION: This SQL code defines a trigger and two functions essential for the application's logic. The `update_driver_status` function and its associated trigger automatically adjust a driver's availability based on the status of their active ride. The `find_driver` function, callable from the Flutter app, locates the closest available driver within a 3000m radius and initiates a new ride, returning the driver and ride IDs.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/www/_blog/2024-09-05-flutter-uber-clone.mdx#_snippet_5

LANGUAGE: sql
CODE:
```
-- Create a trigger to update the driver status
create function update_driver_status()
    returns trigger
    language plpgsql
    as $$
        begin
            if new.status = 'completed' then
                update public.drivers
                set is_available = true
                where id = new.driver_id;
            else
                update public.drivers
                set is_available = false
                where id = new.driver_id;
            end if;
            return new;
    end $$;

create trigger driver_status_update_trigger
after insert or update on rides
for each row
execute function update_driver_status();

-- Finds the closest available driver within 3000m radius
create function public.find_driver(origin geography(POINT), destination geography(POINT), fare int)
    returns table(driver_id uuid, ride_id uuid)
    language plpgsql
    as $$
        declare
            v_driver_id uuid;
            v_ride_id uuid;
        begin
            select
                drivers.id into v_driver_id
            from public.drivers
            where is_available = true
                and st_dwithin(origin, location, 3000)
            order by drivers.location <-> origin
            limit 1;

            -- return null if no available driver is found
            if v_driver_id is null then
                return;
            end if;

            insert into public.rides (driver_id, passenger_id, origin, destination, fare)
            values (v_driver_id, auth.uid(), origin, destination, fare)
            returning id into v_ride_id;

            return query
                select v_driver_id as driver_id, v_ride_id as ride_id;
    end $$ security definer;
```

----------------------------------------

TITLE: Creating Index for JOIN Column in Postgres
DESCRIPTION: This SQL command creates an index on the `customer_id` column of the `orders` table. This index helps optimize join operations between the `customers` and `orders` tables by speeding up the lookup of related records, especially when `a.id` is the primary key of `customers`.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/database/query-optimization.mdx#_snippet_2

LANGUAGE: SQL
CODE:
```
create index idx_orders_customer_id on orders (customer_id);
```

----------------------------------------

TITLE: Connecting Postgres to ClickHouse using FDW
DESCRIPTION: This SQL snippet demonstrates how to create a foreign table in Postgres to connect to a ClickHouse database using the clickhouse_fdw. It defines the schema for 'user_analytics' and then shows a basic query to retrieve data from the ClickHouse instance via Postgres. This allows developers to leverage familiar Postgres tooling for ClickHouse data.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/www/_blog/2024-10-30-supabase-clickhouse-partnership.mdx#_snippet_0

LANGUAGE: SQL
CODE:
```
-- Connect Postgres to your ClickHouse database:
create foreign table user_analytics (
  id bigint,
  user_id bigint,
  event text
)
server clickhouse_server
options ( table 'UserAnalytics' );

-- Query your ClickHouse instance from Postgres:
select * from user_analytics where user_id = 1;
```

----------------------------------------

TITLE: Identifying Slowest Postgres Queries by Execution Time (SQL)
DESCRIPTION: This SQL query fetches statistics for the top 100 queries ordered by their maximum execution time from `pg_stat_statements`. It helps pinpoint outlier queries with high individual execution times, which are prime candidates for performance optimization.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/database/inspect.mdx#_snippet_5

LANGUAGE: SQL
CODE:
```
select
  auth.rolname,
  statements.query,
  statements.calls,
  -- -- Postgres 13, 14, 15
  statements.total_exec_time + statements.total_plan_time as total_time,
  statements.min_exec_time + statements.min_plan_time as min_time,
  statements.max_exec_time + statements.max_plan_time as max_time,
  statements.mean_exec_time + statements.mean_plan_time as mean_time,
  -- -- Postgres <= 12
  -- total_time,
  -- min_time,
  -- max_time,
  -- mean_time,
  statements.rows / statements.calls as avg_rows
from
  pg_stat_statements as statements
  inner join pg_authid as auth on statements.userid = auth.oid
order by max_time desc
limit 100;
```

----------------------------------------

TITLE: Performing Vector Similarity Search with PostgreSQL Function
DESCRIPTION: This SQL function, `match_documents`, performs a similarity search on stored document embeddings using the cosine distance operator (`<=>`). It takes a `query_embedding`, a `match_threshold` for filtering results, and a `match_count` for limiting the output. The function returns a table with the `id`, `content`, and calculated `similarity` for matching documents, ordered by their similarity to the query embedding.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/www/_blog/2023-02-03-openai-embeddings-postgres-vector.mdx#_snippet_2

LANGUAGE: SQL
CODE:
```
create or replace function match_documents (
  query_embedding vector(1536),
  match_threshold float,
  match_count int
)
returns table (
  id bigint,
  content text,
  similarity float
)
language sql stable
as $$
  select
    documents.id,
    documents.content,
    1 - (documents.embedding <=> query_embedding) as similarity
  from documents
  where documents.embedding <=> query_embedding < 1 - match_threshold
  order by documents.embedding <=> query_embedding
  limit match_count;
$$;
```

----------------------------------------

TITLE: Performing an Inner Join in Postgres SQL
DESCRIPTION: This query illustrates how to perform an inner join between the 'employees' and 'departments' tables in PostgreSQL. It retrieves employee names and their corresponding department names, filtering results for employees whose 'start_date' is after '2022-01-01'. It emphasizes using full table names for clarity in joins.
SOURCE: https://github.com/supabase/supabase/blob/master/examples/prompts/code-format-sql.md#_snippet_3

LANGUAGE: SQL
CODE:
```
select
  employees.employee_name,
  departments.department_name
from
  employees
join
  departments on employees.department_id = departments.department_id
where
  employees.start_date > '2022-01-01';
```

----------------------------------------

TITLE: Generate Supabase DB Migration (Bash)
DESCRIPTION: Generates a new database migration file by comparing the current database state (or previous migrations) with the declared schema files. The -f flag names the migration file.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/local-development/declarative-database-schemas.mdx#_snippet_5

LANGUAGE: bash
CODE:
```
supabase db diff -f add_age
```

----------------------------------------

TITLE: Implementing Custom API Key Validation Function - Supabase PL/pgSQL
DESCRIPTION: Creates a `public.check_request` PL/pgSQL function to validate custom API keys. It checks if the request is made by the `anon` role and if the `x-app-api-key` header matches a registered key in `private.anon_api_keys`. Unregistered keys result in a 403 HTTP error.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/api/securing-your-api.mdx#_snippet_13

LANGUAGE: PL/pgSQL
CODE:
```
create function public.check_request()
  returns void
  language plpgsql
  security definer
  as $$
declare
  req_app_api_key text := current_setting('request.headers', true)::json->>'x-app-api-key';
  is_app_api_key_registered boolean;
  jwt_role text := current_setting('request.jwt.claims', true)::json->>'role';
begin
  if jwt_role <> 'anon' then
    -- not `anon` role, allow the request to pass
    return;
  end if;

  select
    true into is_app_api_key_registered
  from private.anon_api_keys
  where
    id = req_app_api_key::uuid
  limit 1;

  if is_app_api_key_registered is true then
    -- api key is registered, allow the request to pass
    return;
  end if;

  raise sqlstate 'PGRST' using
    message = json_build_object(
      'message', 'No registered API key found in x-app-api-key header.')::text,
    detail = json_build_object(
      'status', 403)::text;
end;
  $$;
```

----------------------------------------

TITLE: Supabase FDW Setup for External PostgreSQL DB (SQL)
DESCRIPTION: This SQL snippet configures a Foreign Data Wrapper (FDW) in Supabase to connect to an external PostgreSQL database. It creates a foreign server, maps the `authenticated` Supabase role to an external user, and imports the `users` and `documents` tables from the external database into a local `external` schema.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/ai/rag-with-permissions.mdx#_snippet_7

LANGUAGE: SQL
CODE:
```
create schema external;
create extension postgres_fdw with schema extensions;

-- Setup the foreign server
create server foreign_server
  foreign data wrapper postgres_fdw
  options (host '<db-host>', port '<db-port>', dbname '<db-name>');

-- Map local 'authenticated' role to external 'postgres' user
create user mapping for authenticated
  server foreign_server
  options (user 'postgres', password '<user-password>');

-- Import foreign 'users' and 'documents' tables into 'external' schema
import foreign schema public limit to (users, documents)
  from server foreign_server into external;
```

----------------------------------------

TITLE: Allowing Authenticated Users to Send Broadcast and Presence Messages via RLS (SQL)
DESCRIPTION: This SQL RLS policy for `realtime.messages` allows authenticated users to send both broadcast and presence messages (`extension IN ('broadcast', 'presence')`) on a specific topic if their `user_id` is linked to that `topic` in the `rooms_users` table. It uses an `INSERT` policy with a `WITH CHECK` clause to grant combined write access.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/realtime/authorization.mdx#_snippet_8

LANGUAGE: SQL
CODE:
```
create policy "authenticated can send broadcast and presence on topic"
on "realtime"."messages"
for insert
to authenticated
with check (
  exists (
    select
      user_id
    from
      rooms_users
    where
      user_id = (select auth.uid())
      and name = (select realtime.topic())
      and realtime.messages.extension in ('broadcast', 'presence')
  )
);
```

----------------------------------------

TITLE: Implementing Row Level Security Policies in Supabase SQL
DESCRIPTION: This SQL snippet defines Row Level Security (RLS) policies for various tables (`profiles`, `rooms`, `room_participants`, `messages`) in Supabase. It includes a helper function `is_room_participant` to determine if a user is part of a room, enabling fine-grained access control for viewing and inserting data based on user participation.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/www/_blog/2022-11-22-flutter-authorization-with-rls.mdx#_snippet_15

LANGUAGE: SQL
CODE:
```
-- Returns true if the signed in user is a participant of the room
create or replace function is_room_participant(room_id uuid)
returns boolean as $$
  select exists(
    select 1
    from room_participants
    where room_id = is_room_participant.room_id and profile_id = auth.uid()
  );
$$ language sql security definer;


-- *** Row level security polities ***

alter table public.profiles enable row level security;
create policy "Public profiles are viewable by everyone."
  on public.profiles for select using (true);

alter table public.rooms enable row level security;
create policy "Users can view rooms that they have joined"
  on public.rooms for select using (is_room_participant(id));

alter table public.room_participants enable row level security;
create policy "Participants of the room can view other participants."
  on public.room_participants for select using (is_room_participant(room_id));

alter table public.messages enable row level security;
create policy "Users can view messages on rooms they are in."
  on public.messages for select using (is_room_participant(room_id));
create policy "Users can insert messages on rooms they are in."
  on public.messages for insert with check (is_room_participant(room_id) and profile_id = auth.uid());
```

----------------------------------------

TITLE: Conditionally Rendering MFA Challenge Screen in React with Supabase
DESCRIPTION: This React component, AppWithMFA, checks the user's Authenticator Assurance Level (AAL) using `supabase.auth.mfa.getAuthenticatorAssuranceLevel()` on component mount. If the `nextLevel` is 'aal2' and differs from the `currentLevel`, it sets `showMFAScreen` to true, rendering the `AuthMFA` component for a challenge; otherwise, it renders the main `App` component. The `readyToShow` state ensures no UI is displayed until the AAL check completes.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/auth/auth-mfa/phone.mdx#_snippet_1

LANGUAGE: tsx
CODE:
```
function AppWithMFA() {
  const [readyToShow, setReadyToShow] = useState(false)
  const [showMFAScreen, setShowMFAScreen] = useState(false)

  useEffect(() => {
    ;(async () => {
      try {
        const { data, error } = await supabase.auth.mfa.getAuthenticatorAssuranceLevel()
        if (error) {
          throw error
        }

        console.log(data)

        if (data.nextLevel === 'aal2' && data.nextLevel !== data.currentLevel) {
          setShowMFAScreen(true)
        }
      } finally {
        setReadyToShow(true)
      }
    })()
  }, [])

  if (readyToShow) {
    if (showMFAScreen) {
      return <AuthMFA />
    }

    return <App />
  }

  return <></>
}
```

----------------------------------------

TITLE: Creating List Partitioned Customers Table in Postgres
DESCRIPTION: This SQL snippet demonstrates how to create a list-partitioned `customers` table in PostgreSQL. The table is partitioned by the `country` column, with child partitions (`customers_americas`, `customers_asia`) defined for specific lists of country values. The `country` column is included in the composite primary key as required for partitioning.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/database/partitions.mdx#_snippet_3

LANGUAGE: SQL
CODE:
```
-- Create the partitioned table
create table customers (
    id bigint generated by default as identity,
    name text,
    country text,

    -- We need to include all the
    -- partitioning columns in constraints:
    primary key (country, id)
)
partition by list(country);

create table customers_americas
	partition of customers
	for values in ('US', 'CANADA');

create table customers_asia
	partition of customers
  for values in ('INDIA', 'CHINA', 'JAPAN');
```

----------------------------------------

TITLE: Unnesting Query Type and Authentication Data in Supabase Edge Logs (SQL)
DESCRIPTION: This SQL example demonstrates unnesting `metadata.request.sb` in `edge_logs` to retrieve the request `method`, `url`, and authenticated user's ID (`auth_users`). This information is crucial for identifying problematic queries, tracking user-specific behavior, and debugging issues related to authenticated requests.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/troubleshooting/discovering-and-interpreting-api-errors-in-the-logs-7xREI9.mdx#_snippet_3

LANGUAGE: SQL
CODE:
```
select
  method,
  url,
  auth_users
from
  edge_logs
-- Unpack 'metadata' field
cross join unnest(metadata) AS metadata
-- unpack 'request' from 'metadata'
cross join unnest(request) AS request;
-- unpack 'sb' from 'request'
cross join unnest(sb) AS sb;
```

----------------------------------------

TITLE: Supabase User Profiles and Storage Schema
DESCRIPTION: This SQL script defines the `profiles` table with RLS (Row Level Security) policies for public viewing, user insertion, and user updates. It also sets up Realtime for the `profiles` table and configures an `avatars` storage bucket with public access policies for uploads and selects, ensuring secure and accessible data management.
SOURCE: https://github.com/supabase/supabase/blob/master/examples/user-management/nuxt3-user-management/README.md#_snippet_1

LANGUAGE: sql
CODE:
```
-- Create a table for public "profiles"
create table profiles (
  id uuid references auth.users not null,
  updated_at timestamp with time zone,
  username text unique,
  avatar_url text,
  website text,

  primary key (id),
  unique(username),
  constraint username_length check (char_length(username) >= 3)
);

alter table profiles enable row level security;

create policy "Public profiles are viewable by everyone."
  on profiles for select
  using ( true );

create policy "Users can insert their own profile."
  on profiles for insert
  with check ( (select auth.uid()) = id );

create policy "Users can update own profile."
  on profiles for update
  using ( (select auth.uid()) = id );

-- Set up Realtime!
begin;
  drop publication if exists supabase_realtime;
  create publication supabase_realtime;
commit;
alter publication supabase_realtime add table profiles;

-- Set up Storage!
insert into storage.buckets (id, name)
values ('avatars', 'avatars');

create policy "Avatar images are publicly accessible."
  on storage.objects for select
  using ( bucket_id = 'avatars' );

create policy "Anyone can upload an avatar."
  on storage.objects for insert
  with check ( bucket_id = 'avatars' );
```

----------------------------------------

TITLE: Configuring Pre-Request Hook for API Key Validation - Supabase SQL
DESCRIPTION: Sets the `pgrst.db_pre_request` setting for the `authenticator` role to `public.check_request`, ensuring the custom API key validation function runs before every Data API request. A `notify pgrst, 'reload config'` command is issued to apply the changes immediately.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/api/securing-your-api.mdx#_snippet_14

LANGUAGE: SQL
CODE:
```
alter role authenticator
  set pgrst.db_pre_request = 'public.check_request';

notify pgrst, 'reload config';
```

----------------------------------------

TITLE: Revoking Create Privilege on Public Schema in PostgreSQL
DESCRIPTION: This snippet shows the `postgres` superuser revoking the `CREATE` privilege from the `junior_dev` role on the `public` schema. This action prevents `junior_dev` from creating new objects directly in the `public` schema.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/www/_blog/2024-04-11-postgres-roles-and-privileges.mdx#_snippet_32

LANGUAGE: bash
CODE:
```
# as postgres
postgres=> revoke create on schema public from junior_dev;
```

----------------------------------------

TITLE: SvelteKit Root Layout Component for Session Management (Svelte)
DESCRIPTION: This Svelte component (`src/routes/+layout.svelte`) serves as the root layout for the application. It imports global styles, manages Supabase session reactivity, and uses `onAuthStateChange` to invalidate the `supabase:auth` dependency when the session changes, ensuring UI updates reflect the current authentication state.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/getting-started/tutorials/with-sveltekit.mdx#_snippet_8

LANGUAGE: svelte
CODE:
```
<!-- src/routes/+layout.svelte -->
<script lang="ts">
	import '../styles.css'
	import { invalidate } from '$app/navigation'
	import { onMount } from 'svelte'

	export let data

	let { supabase, session } = data
	$: ({ supabase, session } = data)

	onMount(() => {
		const { data } = supabase.auth.onAuthStateChange((event, newSession) => {
			if (newSession?.expires_at !== session?.expires_at) {
				invalidate('supabase:auth')
			}
		})

		return () => data.subscription.unsubscribe()
	})
</script>

<svelte:head>
	<title>User Management</title>
</svelte:head>

<div class="container" style="padding: 50px 0 100px 0">
	<slot />
</div>
```

----------------------------------------

TITLE: Selecting Data with Supabase (Current `data` property)
DESCRIPTION: This snippet shows the updated method for retrieving query results in Supabase.js. The data is now directly available under the `data` property of the response object, simplifying access. Additionally, `select()` can now be called without the `*` argument.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/www/_blog/2020-10-30-improved-dx.mdx#_snippet_5

LANGUAGE: js
CODE:
```
const { data } = supabase.from('todos').select()
```

----------------------------------------

TITLE: Setting Environment Variables for Supabase API Keys - Bash
DESCRIPTION: This snippet demonstrates how to set Supabase API credentials and project URL as environment variables in a Bash or Zsh shell. These variables (`SUPABASE_URL`, `SUPABASE_KEY`, `SUPABASE_SECRET_KEY`) are crucial for authenticating and connecting to your Supabase project from the Python SDK.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/www/_blog/2022-06-15-loading-data-supabase-python.mdx#_snippet_0

LANGUAGE: bash
CODE:
```
export <variable_name>=<variable_value>
export SUPABASE_URL=<<the value under config > URL>>
export SUPABASE_KEY=<<the value present in Project API keys > anon public>>
export SUPABASE_SECRET_KEY=<<the value present in Project API keys > service_role secret>>
```

----------------------------------------

TITLE: Implementing Upstash Redis Counter Function (TypeScript)
DESCRIPTION: This TypeScript code defines a Supabase Edge Function that acts as a Redis counter. It connects to an Upstash Redis database using environment variables, increments a region-specific counter (or 'localhost' if no region is detected), retrieves all counter values, sorts them, and returns the aggregated counts as a JSON response. It handles potential errors during execution.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/functions/examples/upstash-redis.mdx#_snippet_2

LANGUAGE: typescript
CODE:
```
import { Redis } from 'https://deno.land/x/upstash_redis@v1.19.3/mod.ts'

console.log(`Function "upstash-redis-counter" up and running!`)

Deno.serve(async (_req) => {
  try {
    const redis = new Redis({
      url: Deno.env.get('UPSTASH_REDIS_REST_URL')!,
      token: Deno.env.get('UPSTASH_REDIS_REST_TOKEN')!,
    })

    const deno_region = Deno.env.get('DENO_REGION')
    if (deno_region) {
      // Increment region counter
      await redis.hincrby('supa-edge-counter', deno_region, 1)
    } else {
      // Increment localhost counter
      await redis.hincrby('supa-edge-counter', 'localhost', 1)
    }

    // Get all values
    const counterHash: Record<string, number> | null = await redis.hgetall('supa-edge-counter')
    const counters = Object.entries(counterHash!)
      .sort(([, a], [, b]) => b - a) // sort desc
      .reduce((r, [k, v]) => ({ total: r.total + v, regions: { ...r.regions, [k]: v } }), {
        total: 0,
        regions: {},
      })

    return new Response(JSON.stringify({ counters }), { status: 200 })
  } catch (error) {
    return new Response(JSON.stringify({ error: error.message }), { status: 200 })
  }
})
```

----------------------------------------

TITLE: Handling Supabase User Sign-up in Next.js Server Route (JavaScript)
DESCRIPTION: This Next.js Route Handler (POST) processes user sign-up requests. It extracts email and password from form data, uses `createRouteHandlerClient` with `next/headers` cookies to interact with Supabase Auth, and redirects the user after successful sign-up. A 301 status is used for redirection from POST to GET.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/auth/auth-helpers/nextjs.mdx#_snippet_8

LANGUAGE: jsx
CODE:
```
import { createRouteHandlerClient } from '@supabase/auth-helpers-nextjs'
import { cookies } from 'next/headers'
import { NextResponse } from 'next/server'

export async function POST(request) {
  const requestUrl = new URL(request.url)
  const formData = await request.formData()
  const email = formData.get('email')
  const password = formData.get('password')
  const cookieStore = cookies()
  const supabase = createRouteHandlerClient({ cookies: () => cookieStore })

  await supabase.auth.signUp({
    email,
    password,
    options: {
      emailRedirectTo: `${requestUrl.origin}/auth/callback`,
    },
  })

  return NextResponse.redirect(requestUrl.origin, {
    status: 301,
  })
}
```

----------------------------------------

TITLE: Optimized RLS Policy Avoiding Joins in SQL
DESCRIPTION: This SQL snippet presents an optimized RLS policy for 'test_table' that avoids the explicit join seen in the previous example. Instead of joining, it selects 'team_id' values from 'team_user' where 'user_id' matches the authenticated user's ID. The policy then checks if the 'test_table.team_id' is present in this set, significantly improving performance by eliminating the join operation within the policy.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/database/postgres/row-level-security.mdx#_snippet_22

LANGUAGE: SQL
CODE:
```
create policy "rls_test_select" on test_table
to authenticated
using (
  team_id in (
    select team_id
    from team_user
    where user_id = (select auth.uid()) -- no join
  )
);
```

----------------------------------------

TITLE: Fetching User Data in Supabase Edge Functions (JavaScript)
DESCRIPTION: This code snippet shows how to fetch the authenticated user's object within a Supabase Edge Function after setting the Auth context. It retrieves the user token from the 'Authorization' header, uses 'supabaseClient.auth.getUser()' to get the user data, and then returns the user object as a JSON response. This allows for user-specific logic and RLS enforcement.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/functions/auth.mdx#_snippet_1

LANGUAGE: JavaScript
CODE:
```
import { createClient } from 'jsr:@supabase/supabase-js@2'

Deno.serve(async (req: Request) => {

  const supabaseClient = createClient(
    Deno.env.get('SUPABASE_URL') ?? '',
    Deno.env.get('SUPABASE_ANON_KEY') ?? '',
  )

  // Get the session or user object
  const authHeader = req.headers.get('Authorization')!
  const token = authHeader.replace('Bearer ', '')
  const { data } = await supabaseClient.auth.getUser(token)
  const user = data.user

  return new Response(JSON.stringify({ user }), {
    headers: { 'Content-Type': 'application/json' },
    status: 200,
  })

})
```

----------------------------------------

TITLE: Configuring Supabase Database Schema for User Profiles (SQL)
DESCRIPTION: This SQL script defines the 'profiles' table for user data, sets up row-level security policies to control access, enables Realtime updates for the 'profiles' table, and configures a storage bucket named 'avatars' with public access policies for image uploads. It ensures users can manage their own profiles and avatars securely.
SOURCE: https://github.com/supabase/supabase/blob/master/examples/user-management/swift-user-management/README.md#_snippet_0

LANGUAGE: SQL
CODE:
```
-- Create a table for public "profiles"
create table profiles (
  id uuid references auth.users not null,
  updated_at timestamp with time zone,
  username text unique,
  avatar_url text,
  website text,

  primary key (id),
  unique(username),
  constraint username_length check (char_length(username) >= 3)
);

alter table profiles enable row level security;

create policy "Public profiles are viewable by everyone."
  on profiles for select
  using ( true );

create policy "Users can insert their own profile."
  on profiles for insert
  with check ( (select auth.uid()) = id );

create policy "Users can update own profile."
  on profiles for update
  using ( (select auth.uid()) = id );

-- Set up Realtime!
begin;
  drop publication if exists supabase_realtime;
  create publication supabase_realtime;
commit;
alter publication supabase_realtime add table profiles;

-- Set up Storage!
insert into storage.buckets (id, name)
values ('avatars', 'avatars');

create policy "Avatar images are publicly accessible."
  on storage.objects for select
  using ( bucket_id = 'avatars' );

create policy "Anyone can upload an avatar."
  on storage.objects for insert
  with check ( bucket_id = 'avatars' );
```

----------------------------------------

TITLE: Initialize Supabase Client (Swift)
DESCRIPTION: This snippet demonstrates how to initialize the Supabase client in a Swift application. It requires your Supabase project URL and the 'anon' key. The client instance is created using the SupabaseClient class.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/getting-started/tutorials/with-swift.mdx#_snippet_0

LANGUAGE: Swift
CODE:
```
import Foundation
import Supabase

let supabase = SupabaseClient(
  supabaseURL: URL(string: "YOUR_SUPABASE_URL")!,
  supabaseKey: "YOUR_SUPABASE_ANON_KEY"
)
```

----------------------------------------

TITLE: Calling Llamafile with OpenAI Deno SDK - TypeScript
DESCRIPTION: This TypeScript code demonstrates using the OpenAI Deno SDK within a Supabase Edge Function to communicate with the Llamafile server. It handles streaming responses from the AI model, providing a flexible way to integrate Llamafile's OpenAI-compatible API.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/www/_blog/2024-08-21-mozilla-llamafile-in-supabase-edge-functions.mdx#_snippet_5

LANGUAGE: ts
CODE:
```
import OpenAI from 'https://deno.land/x/openai@v4.53.2/mod.ts'

Deno.serve(async (req) => {
  const client = new OpenAI()
  const { prompt } = await req.json()
  const stream = true

  const chatCompletion = await client.chat.completions.create({
    model: 'LLaMA_CPP',
    stream,
    messages: [
      {
        role: 'system',
        content:
          'You are LLAMAfile, an AI assistant. Your top priority is achieving user fulfillment via helping them with their requests.',
      },
      {
        role: 'user',
        content: prompt,
      },
    ],
  })

  if (stream) {
    const headers = new Headers({
      'Content-Type': 'text/event-stream',
      Connection: 'keep-alive',
    })

    // Create a stream
    const stream = new ReadableStream({
      async start(controller) {
        const encoder = new TextEncoder()

        try {
          for await (const part of chatCompletion) {
            controller.enqueue(encoder.encode(part.choices[0]?.delta?.content || ''))
          }
        } catch (err) {
          console.error('Stream error:', err)
        } finally {
          controller.close()
        }
      },
    })

    // Return the stream to the user
    return new Response(stream, {
      headers,
    })
  }

  return Response.json(chatCompletion)
})
```

----------------------------------------

TITLE: Declare Supabase Environment Variables (.env.local)
DESCRIPTION: Creates a `.env.local` file to store Supabase project URL and anonymous key as environment variables for the SolidJS app.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/getting-started/quickstarts/solidjs.mdx#_snippet_2

LANGUAGE: text
CODE:
```
VITE_SUPABASE_URL=<SUBSTITUTE_SUPABASE_URL>
VITE_SUPABASE_ANON_KEY=<SUBSTITUTE_SUPABASE_ANON_KEY>
```

----------------------------------------

TITLE: Svelte Avatar Upload Widget
DESCRIPTION: This Svelte component (`src/lib/Avatar.svelte`) provides functionality for users to upload and display profile photos using Supabase Storage. It handles image download, file selection, and upload processes, dispatching an 'upload' event upon successful completion. It depends on `supabaseClient.ts`.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/getting-started/tutorials/with-svelte.mdx#_snippet_9

LANGUAGE: html
CODE:
```
<script lang="ts">
  import { createEventDispatcher } from 'svelte'
  import { supabase } from '../supabaseClient'

  export let size: number
  export let url: string | null = null

  let avatarUrl: string | null = null
  let uploading = false
  let files: FileList

  const dispatch = createEventDispatcher()

  const downloadImage = async (path: string) => {
    try {
      const { data, error } = await supabase.storage.from('avatars').download(path)

      if (error) {
        throw error
      }

      const url = URL.createObjectURL(data)
      avatarUrl = url
    } catch (error) {
      if (error instanceof Error) {
        console.log('Error downloading image: ', error.message)
      }
    }
  }

  const uploadAvatar = async () => {
    try {
      uploading = true

      if (!files || files.length === 0) {
        throw new Error('You must select an image to upload.')
      }

      const file = files[0]
      const fileExt = file.name.split('.').pop()
      const filePath = `${Math.random()}.${fileExt}`

      const { error } = await supabase.storage.from('avatars').upload(filePath, file)

      if (error) {
        throw error
      }

      url = filePath
      dispatch('upload')
    } catch (error) {
      if (error instanceof Error) {
        alert(error.message)
      }
    } finally {
      uploading = false
    }
  }

  $: if (url) downloadImage(url)
</script>

<div style="width: {size}px" aria-live="polite">
  {#if avatarUrl} <img src={avatarUrl} alt={avatarUrl ? 'Avatar' : 'No image'} class="avatar image"
  style="height: {size}px, width: {size}px" /> {:else}
  <div class="avatar no-image" style="height: {size}px, width: {size}px" />
  {/if}
  <div style="width: {size}px">
    <label class="button primary block" for="single">
      {uploading ? 'Uploading ...' : 'Upload avatar'}
    </label>
    <span style="display:none">
      <input
        type="file"
        id="single"
        accept="image/*"
        bind:files
        on:change={uploadAvatar}
        disabled={uploading}
      />
    </span>
  </div>
</div>
```

----------------------------------------

TITLE: Enabling pgvector Extension in PostgreSQL (SQL)
DESCRIPTION: Enables the `pgvector` extension in your PostgreSQL database schema, which is required for storing and querying vector embeddings for similarity search.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/ai/examples/nextjs-vector-search.mdx#_snippet_2

LANGUAGE: sql
CODE:
```
-- Enable pgvector extension
create extension if not exists vector with schema public;
```

----------------------------------------

TITLE: Deploying Supabase Edge Function with GitHub Actions (YAML)
DESCRIPTION: This YAML workflow defines a GitHub Action to deploy a Supabase Edge Function. It's triggered on pushes to the 'main' branch or via manual workflow_dispatch. The action checks out the repository, sets up the Supabase CLI, and then deploys the 'github-action-deploy' function to the specified project using environment variables for the access token and project ID.
SOURCE: https://github.com/supabase/supabase/blob/master/examples/edge-functions/supabase/functions/github-action-deploy/README.md#_snippet_0

LANGUAGE: YAML
CODE:
```
name: Deploy Function

on:
  push:
    branches:
      - main
  workflow_dispatch:

jobs:
  deploy:
    runs-on: ubuntu-latest

    env:
      SUPABASE_ACCESS_TOKEN: ${{ secrets.SUPABASE_ACCESS_TOKEN }}
      PROJECT_ID: zdtdtxajzydjqzuktnqx

    steps:
      - uses: actions/checkout@v3

      - uses: supabase/setup-cli@v1
        with:
          version: latest

      - run: supabase functions deploy github-action-deploy --project-ref $PROJECT_ID
```

----------------------------------------

TITLE: Initializing a Local Supabase Project (Bash)
DESCRIPTION: This command initializes a new Supabase project in the current directory, setting up the necessary configuration files and prompting for local port assignments for the Supabase URL and PostgreSQL database. It provides the local Supabase URL and public/private keys for application integration.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/www/_blog/2021-03-31-supabase-cli.mdx#_snippet_2

LANGUAGE: bash
CODE:
```
supabase init

# ✔ Port for Supabase URL: · 8000
# ✔ Port for PostgreSQL database: · 5432
# ✔ Project initialized.
# Supabase URL: http://localhost:8000
# Supabase Key (anon, public): eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJzdXBhYmFzZSIsImlhdCI6MTYwMzk2ODgzNCwiZXhwIjoyNTUwNjUzNjM0LCJyb2xlIjoiYW5vbiJ9.36fUebxgx1mcBo4s19v0SzqmzunP--hm_hep0uLX0ew
# Supabase Key (service_role, private): eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJzdXBhYmFzZSIsImlhdCI6MTYwMzk2ODgzNCwiZXhwIjoyNTUwNjUzNjM0LCJyb2xlIjoiYW5vbiJ9.36fUebxgx1mcBo4s19v0SzqmzunP--hm_hep0uLX0ew
# Database URL: postgres://postgres:postgres@localhost:5432/postgres
```

----------------------------------------

TITLE: Adding an Exclusion Constraint for Non-Overlapping `tstzrange` Durations
DESCRIPTION: This SQL statement adds an `EXCLUDE` constraint named `exclude_duration` to the `reservations` table. It uses a GiST index and the `&&` operator to prevent any new inserts or updates from creating reservations whose `duration` range overlaps with existing ones, ensuring data integrity for time-based events.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/www/_blog/2024-07-11-range-columns.mdx#_snippet_3

LANGUAGE: SQL
CODE:
```
alter table reservations
	add constraint exclude_duration exclude
	using gist (duration with &&)
```

----------------------------------------

TITLE: Defining a Database Trigger for Realtime Broadcasts - SQL
DESCRIPTION: This SQL snippet creates a database trigger named `broadcast_changes_for_your_table_trigger`. It is configured to fire `AFTER INSERT OR UPDATE OR DELETE` operations on the `public.your_table` table. For each affected row, it executes the `public.your_table_changes()` function, which then broadcasts the change via Supabase Realtime.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/www/_blog/2025-04-02-realtime-broadcast-from-database.mdx#_snippet_2

LANGUAGE: SQL
CODE:
```
create trigger broadcast_changes_for_your_table_trigger
after insert or update or delete
on public.your_table
for each row
execute function your_table_changes();
```

----------------------------------------

TITLE: Creating Tables for Many-to-Many Relationship (SQL)
DESCRIPTION: This SQL snippet defines three tables: `movies`, `actors`, and `performances`. The `performances` table acts as a join table, linking `movie_id` and `actor_id` with foreign keys, enabling a many-to-many relationship between movies and actors.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/database/tables.mdx#_snippet_14

LANGUAGE: SQL
CODE:
```
create table movies (
  id bigint generated by default as identity primary key,
  name text,
  description text
);

create table actors (
  id bigint generated by default as identity primary key,
  name text
);

create table performances (
  id bigint generated by default as identity primary key,
  movie_id bigint not null references movies,
  actor_id bigint not null references actors
);
```

----------------------------------------

TITLE: Subscribing to Real-time Postgres Changes with Supabase (JavaScript)
DESCRIPTION: This snippet demonstrates how to subscribe to real-time database changes (specifically `INSERT` events) from a Postgres table using Supabase Realtime. It configures a channel with a filter to listen for new messages in the `public.messages` table, limited to a specific `room_id`, and logs the payload of each received database event. This allows clients to react instantly to data modifications.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/www/_blog/2022-08-18-supabase-realtime-multiplayer-general-availability.mdx#_snippet_2

LANGUAGE: javascript
CODE:
```
const channelId = '#random'

// Create a filter only for new messages
const databaseFilter = {
  schema: 'public',
  table: 'messages',
  filter: `room_id=eq.${channelId}`,
  event: 'INSERT',
}

const channel = supabase
  .channel(channelId)
  .on('postgres_changes', databaseFilter, (payload) => receivedDatabaseEvent(payload))
  .subscribe()

const receivedDatabaseEvent = (event) => {
  const { payload } = event
  console.log(payload)
}
```

----------------------------------------

TITLE: Adding a Multi-Column Exclusion Constraint for `table_id` and `tstzrange`
DESCRIPTION: This SQL snippet first enables the `btree_gist` extension, which is required for multi-column GiST indexes. It then adds an `EXCLUDE` constraint to the `reservations` table, ensuring that reservations do not overlap (`duration WITH &&`) *only if* they are for the same `table_id` (`table_id WITH =`), allowing for distinct bookings across different tables.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/www/_blog/2024-07-11-range-columns.mdx#_snippet_6

LANGUAGE: SQL
CODE:
```
-- Enable the btree_gist index required for the constraint.
create extension btree_gist;

-- Add a constraint to prevent overlaps with the same table_id
alter table reservations
  add constraint exclude_duration
  exclude using gist (table_id WITH =, duration WITH &&);
```

----------------------------------------

TITLE: Configuring Supabase Client for PKCE Authentication Flow
DESCRIPTION: This TypeScript/JavaScript code demonstrates how to initialize the Supabase client to use the Proof Key for Code Exchange (PKCE) authentication flow. By setting `auth.flowType` to `'pkce'` in the client configuration, `supabase-js` automatically handles the generation, storage, and exchange of the code verifier and authorization code, enhancing security for mobile and server-side applications.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/www/_blog/2023-04-13-supabase-auth-sso-pkce.mdx#_snippet_3

LANGUAGE: tsx
CODE:
```
import { createClient } from '@supabase/supabase-js'

const supabase = createClient(SUPABASE_URL, SUPABASE_ANON_KEY, {
  auth: {
    flowType: 'pkce',
  },
})
```

----------------------------------------

TITLE: Initializing Expo React Native App and Installing Core Dependencies (Bash)
DESCRIPTION: These commands initialize a new Expo React Native project with a TypeScript template and then install the required dependencies for integrating Supabase and UI components, including `@supabase/supabase-js`, `@react-native-async-storage/async-storage`, and `@rneui/themed`.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/getting-started/tutorials/with-expo-react-native.mdx#_snippet_0

LANGUAGE: bash
CODE:
```
npx create-expo-app -t expo-template-blank-typescript expo-user-management

cd expo-user-management
```

LANGUAGE: bash
CODE:
```
npx expo install @supabase/supabase-js @react-native-async-storage/async-storage @rneui/themed
```

----------------------------------------

TITLE: Deploying database migrations
DESCRIPTION: This command pushes your local database migrations to the linked remote Supabase database. It applies all pending schema changes, ensuring your remote database structure matches your local development environment.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/deployment/database-migrations.mdx#_snippet_12

LANGUAGE: bash
CODE:
```
supabase db push
```

----------------------------------------

TITLE: Creating PL/pgSQL Function to Process Embedding Jobs
DESCRIPTION: This PL/pgSQL function, `util.process_embeddings`, reads messages from the 'embedding_jobs' queue in batches using `pgmq.read`. It groups these jobs into `batch_size` chunks and then iterates through each batch, invoking an Edge Function named 'embed' with the batch as its body. The function includes parameters for `batch_size`, `max_requests`, and `timeout_milliseconds` to control processing behavior and visibility timeouts for retries.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/ai/automatic-embeddings.mdx#_snippet_6

LANGUAGE: PL/pgSQL
CODE:
```
create or replace function util.process_embeddings(
  batch_size int = 10,
  max_requests int = 10,
  timeout_milliseconds int = 5 * 60 * 1000 -- default 5 minute timeout
)
returns void
language plpgsql
as $$
declare
  job_batches jsonb[];
  batch jsonb;
begin
  with
    -- First get jobs and assign batch numbers
    numbered_jobs as (
      select
        message || jsonb_build_object('jobId', msg_id) as job_info,
        (row_number() over (order by 1) - 1) / batch_size as batch_num
      from pgmq.read(
        queue_name => 'embedding_jobs',
        vt => timeout_milliseconds / 1000,
        qty => max_requests * batch_size
      )
    ),
    -- Then group jobs into batches
    batched_jobs as (
      select
        jsonb_agg(job_info) as batch_array,
        batch_num
      from numbered_jobs
      group by batch_num
    )
  -- Finally aggregate all batches into array
  select array_agg(batch_array)
  from batched_jobs
  into job_batches;

  -- Invoke the embed edge function for each batch
  foreach batch in array job_batches loop
    perform util.invoke_edge_function(
      name => 'embed',
      body => batch,
      timeout_milliseconds => timeout_milliseconds
    );
  end loop;
end;
$$;
```

----------------------------------------

TITLE: Initializing Supabase Server Client with Types (Server-Side API Route)
DESCRIPTION: This snippet illustrates how to initialize a server-side Supabase client within a Next.js API route using `createPagesServerClient`. It accepts `req` and `res` objects and demonstrates passing a `Database` type for type safety, then fetches user data.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/auth/auth-helpers/nextjs-pages.mdx#_snippet_28

LANGUAGE: TypeScript
CODE:
```
// server-side API route
import type { NextApiRequest, NextApiResponse } from 'next'
import type { Database } from 'types_db'

export default async (req: NextApiRequest, res: NextApiResponse) => {
  const supabaseServerClient = createPagesServerClient<Database>({
    req,
    res,
  })
  const {
    data: { user },
  } = await supabaseServerClient.auth.getUser()

  res.status(200).json({ name: user?.name ?? '' })
}
```

----------------------------------------

TITLE: Creating a Table with a Vector Column (SQL)
DESCRIPTION: This SQL snippet illustrates how to define a new table named `documents` that includes a column of type `vector`. The `embedding` column is specified with 384 dimensions, which should be adjusted to match the output dimensions of the embedding model used. This table structure is designed to store text content along with its corresponding vector embeddings.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/ai/vector-columns.mdx#_snippet_1

LANGUAGE: SQL
CODE:
```
create table documents (
  id serial primary key,
  title text not null,
  body text not null,
  embedding vector(384)
);
```

----------------------------------------

TITLE: Revoking Table-Level and Granting Column-Level UPDATE Privileges (SQL)
DESCRIPTION: This SQL example first revokes the default table-level UPDATE privilege on the 'public.posts' table from the 'authenticated' role. Subsequently, it grants a more granular column-level UPDATE privilege specifically on the 'title' and 'content' columns of the 'public.posts' table to the 'authenticated' role. This allows authenticated users to update only these specified columns, not all columns.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/database/postgres/column-level-security.mdx#_snippet_1

LANGUAGE: SQL
CODE:
```
revoke
update
  on table public.posts
from
  authenticated;

grant
update
  (title, content) on table public.posts to authenticated;
```

----------------------------------------

TITLE: Enabling Read Access for Storage Objects in Supabase SQL
DESCRIPTION: This SQL snippet creates a storage policy named 'Enable read access for all users' on 'storage.objects'. It grants 'SELECT' (read) access to all users ('public') with a condition 'USING (true)', allowing unrestricted read access.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/_partials/product_management_sql_template.mdx#_snippet_2

LANGUAGE: SQL
CODE:
```
CREATE POLICY "Enable read access for all users" ON "storage"."objects"
AS PERMISSIVE FOR SELECT
TO public
USING (true)
```

----------------------------------------

TITLE: Normalizing a Vector into a Unit Vector
DESCRIPTION: This helper function, `normalize`, transforms a given numeric vector into a unit vector by dividing each component by its L2 norm (magnitude). A unit vector has a length of 1, which is essential for compatibility with similarity functions like dot product. It throws an error if attempting to normalize a zero vector.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/www/_blog/2024-02-13-matryoshka-embeddings.mdx#_snippet_8

LANGUAGE: TypeScript
CODE:
```
/**
 * Normalizes a vector into a unit vector.
 */
function normalize(vector: number[]): number[] {
  const magnitude = norm(vector)

  if (magnitude === 0) {
    throw new Error('Cannot normalize a zero vector.')
  }

  return vector.map((val) => val / magnitude)
}
```

----------------------------------------

TITLE: Uploading Files to Supabase Storage with React Dropzone
DESCRIPTION: This React component integrates `react-dropzone` for file selection and uploads to Supabase Storage. Upon dropping a file, it fetches a signed upload URL from a Next.js API route and then uses the Supabase browser client's `uploadToSignedUrl` method to securely upload the accepted file to the 'avatars' bucket.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/www/_blog/2024-06-18-calcom-platform-starter-kit-nextjs-supabase.mdx#_snippet_4

LANGUAGE: TSX
CODE:
```
'use client'

import { env } from '@/env'
import { createClient } from '@supabase/supabase-js'
import Image from 'next/image'
import React, { useState } from 'react'
import { useDropzone } from 'react-dropzone'

export default function SupabaseReactDropzone({ userId }: { userId?: string } = {}) {
  const supabaseBrowserClient = createClient(
    env.NEXT_PUBLIC_SUPABASE_URL,
    env.NEXT_PUBLIC_SUPABASE_ANON_KEY
  )
  const { acceptedFiles, fileRejections, getRootProps, getInputProps } = useDropzone({
    maxFiles: 1,
    accept: {
      'image/jpeg': [],
      'image/png': [],
    },
    onDropAccepted: async (acceptedFiles) => {
      setAvatar(null)
      console.log(acceptedFiles)
      const { path, token }: { path: string; token: string } = await fetch(
        '/api/supabase/storage'
      ).then((res) => res.json())

      const { data, error } = await supabaseBrowserClient.storage
        .from('avatars')
        .uploadToSignedUrl(path, token, acceptedFiles[0])
    },
  })

  return (
    <div className="mx-auto mt-4 grid w-full gap-2">
      <div {...getRootProps({ className: 'dropzone' })}>
        <input {...getInputProps()} />
        <p>Drag 'n' drop some files here, or click to select files</p>
        <em>(Only *.jpeg and *.png images will be accepted)</em>
      </div>
    </div>
  )
}
```

----------------------------------------

TITLE: Querying Supabase Data with TypeScript Client
DESCRIPTION: This snippet demonstrates how to fetch data from the 'orders' table using the Supabase TypeScript client. It performs a join to include related product information and filters results by a specific customer ID, showcasing the auto-generated API's ability to handle complex queries and relationships.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/www/_blog/2025-05-17-simplify-backend-with-data-api.mdx#_snippet_0

LANGUAGE: tsx
CODE:
```
const { data, error } = await supabase
  .from('orders')
  .select(
    `
    id,
    created_at,
    total,
    products (
      id,
      name,
      price
    )
  `
  )
  .eq('customer_id', customerId)
```

----------------------------------------

TITLE: Managing User Account Profile (React JSX)
DESCRIPTION: This React component (`src/Account.jsx`) allows authenticated users to view and update their profile details (username, website, avatar URL) stored in a 'profiles' table. It fetches existing data on component mount and provides a form to submit updates, including a sign-out button.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/getting-started/tutorials/with-react.mdx#_snippet_5

LANGUAGE: jsx
CODE:
```
import { useState, useEffect } from 'react'
import { supabase } from './supabaseClient'

export default function Account({ session }) {
  const [loading, setLoading] = useState(true)
  const [username, setUsername] = useState(null)
  const [website, setWebsite] = useState(null)
  const [avatar_url, setAvatarUrl] = useState(null)

  useEffect(() => {
    let ignore = false
    async function getProfile() {
      setLoading(true)
      const { user } = session

      const { data, error } = await supabase
        .from('profiles')
        .select(`username, website, avatar_url`)
        .eq('id', user.id)
        .single()

      if (!ignore) {
        if (error) {
          console.warn(error)
        } else if (data) {
          setUsername(data.username)
          setWebsite(data.website)
          setAvatarUrl(data.avatar_url)
        }
      }

      setLoading(false)
    }

    getProfile()

    return () => {
      ignore = true
    }
  }, [session])

  async function updateProfile(event, avatarUrl) {
    event.preventDefault()

    setLoading(true)
    const { user } = session

    const updates = {
      id: user.id,
      username,
      website,
      avatar_url: avatarUrl,
      updated_at: new Date(),
    }

    const { error } = await supabase.from('profiles').upsert(updates)

    if (error) {
      alert(error.message)
    } else {
      setAvatarUrl(avatarUrl)
    }
    setLoading(false)
  }

  return (
    <form onSubmit={updateProfile} className="form-widget">
      <div>
        <label htmlFor="email">Email</label>
        <input id="email" type="text" value={session.user.email} disabled />
      </div>
      <div>
        <label htmlFor="username">Name</label>
        <input
          id="username"
          type="text"
          required
          value={username || ''}
          onChange={(e) => setUsername(e.target.value)}
        />
      </div>
      <div>
        <label htmlFor="website">Website</label>
        <input
          id="website"
          type="url"
          value={website || ''}
          onChange={(e) => setWebsite(e.target.value)}
        />
      </div>

      <div>
        <button className="button block primary" type="submit" disabled={loading}>
          {loading ? 'Loading ...' : 'Update'}
        </button>
      </div>

      <div>
        <button className="button block" type="button" onClick={() => supabase.auth.signOut()}>
          Sign Out
        </button>
      </div>
    </form>
  )
}
```

----------------------------------------

TITLE: Handling Supabase Email OTP Confirmation in Next.js
DESCRIPTION: This snippet demonstrates how to create a Next.js API route (`app/auth/confirm/route.ts`) to handle Supabase email OTP verification. It extracts `token_hash` and `type` from the request URL, verifies them using `supabase.auth.verifyOtp`, and redirects the user to the specified `next` URL on success or an error page on failure.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/auth/passwords.mdx#_snippet_6

LANGUAGE: TypeScript
CODE:
```
import { type EmailOtpType } from '@supabase/supabase-js'
import { type NextRequest } from 'next/server'

import { createClient } from '@/utils/supabase/server'
import { redirect } from 'next/navigation'

export async function GET(request: NextRequest) {
  const { searchParams } = new URL(request.url)
  const token_hash = searchParams.get('token_hash')
  const type = searchParams.get('type') as EmailOtpType | null
  const next = searchParams.get('next') ?? '/'

  if (token_hash && type) {
    const supabase = await createClient()

    const { error } = await supabase.auth.verifyOtp({
      type,
      token_hash,
    })
    if (!error) {
      // redirect user to specified redirect URL or root of app
      redirect(next)
    }
  }

  // redirect the user to an error page with some instructions
  redirect('/auth/auth-code-error')
}
```

----------------------------------------

TITLE: Main SolidJS Application Component with Supabase Session Management
DESCRIPTION: This is the main SolidJS application component (`App.tsx`) that manages the user's authentication session. It uses `createEffect` to listen for Supabase authentication state changes and conditionally renders either the `Auth` component (if no session) or the `Account` component (if a session exists), providing the session data to the `Account` component.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/getting-started/tutorials/with-solidjs.mdx#_snippet_6

LANGUAGE: jsx
CODE:
```
import { Component, createEffect, createSignal } from 'solid-js'
import { supabase } from './supabaseClient'
import { AuthSession } from '@supabase/supabase-js'
import Account from './Account'
import Auth from './Auth'

const App: Component = () => {
  const [session, setSession] = createSignal<AuthSession | null>(null)

  createEffect(() => {
    supabase.auth.getSession().then(({ data: { session } }) => {
      setSession(session)
    })

    supabase.auth.onAuthStateChange((_event, session) => {
      setSession(session)
    })
  })

  return (
    <div class="container" style={{ padding: '50px 0 100px 0' }}>
      {!session() ? <Auth /> : <Account session={session()!} />}
    </div>
  )
}

export default App
```

----------------------------------------

TITLE: Subscribing to Realtime Events (After) - TypeScript
DESCRIPTION: This snippet demonstrates the current approach for subscribing to realtime PostgreSQL changes on the 'user' table within the 'public' schema via a named channel. This new API provides more granular control over event types and schemas.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/docs/ref/javascript/v1/upgrade-guide.mdx#_snippet_20

LANGUAGE: ts
CODE:
```
const userListener = supabase
  .channel('public:user')
  .on('postgres_changes', { event: '*', schema: 'public', table: 'user' }, (payload) =>
    handleAllEventsPayload()
  )
  .subscribe()
```

----------------------------------------

TITLE: Restricting Profile Updates with MFA Assurance Level in SQL
DESCRIPTION: This SQL policy restricts `UPDATE` operations on the `profiles` table to authenticated users who have achieved an 'aal2' (Assurance Level 2) Multi-Factor Authentication status. It leverages the `auth.jwt()->>'aal'` claim to enforce that users have at least two levels of authentication before they can modify their profile.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/database/postgres/row-level-security.mdx#_snippet_12

LANGUAGE: SQL
CODE:
```
create policy "Restrict updates."
on profiles
as restrictive
for update
to authenticated using (
  (select auth.jwt()->>'aal') = 'aal2'
);
```

----------------------------------------

TITLE: Managing User Account Profiles in Ionic React
DESCRIPTION: This `AccountPage` component allows authenticated users to view and update their profile information (username, website, avatar_url) stored in Supabase. It fetches existing profile data on load and provides functionality to update it and sign out of the application.
SOURCE: https://github.com/supabase/supabase/blob/master/apps/docs/content/guides/getting-started/tutorials/with-ionic-react.mdx#_snippet_4

LANGUAGE: jsx
CODE:
```
import {
  IonButton,
  IonContent,
  IonHeader,
  IonInput,
  IonItem,
  IonLabel,
  IonPage,
  IonTitle,
  IonToolbar,
  useIonLoading,
  useIonToast,
  useIonRouter
} from '@ionic/react';
import { useEffect, useState } from 'react';
import { supabase } from '../supabaseClient';

export function AccountPage() {
  const [showLoading, hideLoading] = useIonLoading();
  const [showToast] = useIonToast();
  const [session] = useState(() => supabase.auth.session());
  const router = useIonRouter();
  const [profile, setProfile] = useState({
    username: '',
    website: '',
    avatar_url: '',
  });
  useEffect(() => {
    getProfile();
  }, [session]);
  const getProfile = async () => {
    console.log('get');
    await showLoading();
    try {
      const user = supabase.auth.user();
      const { data, error, status } = await supabase
        .from('profiles')
        .select(`username, website, avatar_url`)
        .eq('id', user!.id)
        .single();

      if (error && status !== 406) {
        throw error;
      }

      if (data) {
        setProfile({
          username: data.username,
          website: data.website,
          avatar_url: data.avatar_url,
        });
      }
    } catch (error: any) {
      showToast({ message: error.message, duration: 5000 });
    } finally {
      await hideLoading();
    }
  };
  const signOut = async () => {
    await supabase.auth.signOut();
    router.push('/', 'forward', 'replace');
  }
  const updateProfile = async (e?: any, avatar_url: string = '') => {
    e?.preventDefault();

    console.log('update ');
    await showLoading();

    try {
      const user = supabase.auth.user();

      const updates = {
        id: user!.id,
        ...profile,
        avatar_url: avatar_url,
        updated_at: new Date(),
      };

      const { error } = await supabase.from('profiles').upsert(updates, {
        returning: 'minimal', // Don't return the value after inserting
      });

      if (error) {
        throw error;
      }
    } catch (error: any) {
      showToast({ message: error.message, duration: 5000 });
    } finally {
      await hideLoading();
    }
  };
  return (
    <IonPage>
      <IonHeader>
        <IonToolbar>
          <IonTitle>Account</IonTitle>
        </IonToolbar>
      </IonHeader>

      <IonContent>
        <form onSubmit={updateProfile}>
          <IonItem>
            <IonLabel>
              <p>Email</p>
              <p>{session?.user?.email}</p>
            </IonLabel>
          </IonItem>

          <IonItem>
            <IonLabel position="stacked">Name</IonLabel>
            <IonInput
              type="text"
              name="username"
              value={profile.username}
              onIonChange={(e) =>
                setProfile({ ...profile, username: e.detail.value ?? '' })
              }
            ></IonInput>
          </IonItem>

          <IonItem>
            <IonLabel position="stacked">Website</IonLabel>
            <IonInput
              type="url"
              name="website"
              value={profile.website}
              onIonChange={(e) =>
                setProfile({ ...profile, website: e.detail.value ?? '' })
              }
            ></IonInput>
          </IonItem>
          <div className="ion-text-center">
            <IonButton fill="clear" type="submit">
              Update Profile
            </IonButton>
          </div>
        </form>

        <div className="ion-text-center">
          <IonButton fill="clear" onClick={signOut}>
            Log Out
          </IonButton>
        </div>
      </IonContent>
    </IonPage>
  );
}
```