TITLE: Migrating getStaticProps and getServerSideProps to App Router with Fetch API
DESCRIPTION: This snippet demonstrates how to replace `getStaticProps` and `getServerSideProps` from the `pages` directory with the new `fetch()` API in the `app` directory. It shows examples for static data fetching (cached indefinitely), dynamic data fetching (no caching), and revalidated data fetching (cached for a specific duration.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/02-guides/migrating/app-router-migration.mdx#_snippet_22

LANGUAGE: tsx
CODE:
```
export default async function Page() {
  // This request should be cached until manually invalidated.
  // Similar to `getStaticProps`.
  // `force-cache` is the default and can be omitted.
  const staticData = await fetch(`https://...`, { cache: 'force-cache' })

  // This request should be refetched on every request.
  // Similar to `getServerSideProps`.
  const dynamicData = await fetch(`https://...`, { cache: 'no-store' })

  // This request should be cached with a lifetime of 10 seconds.
  // Similar to `getStaticProps` with the `revalidate` option.
  const revalidatedData = await fetch(`https://...`, {
    next: { revalidate: 10 },
  })

  return <div>...</div>
}
```

LANGUAGE: jsx
CODE:
```
export default async function Page() {
  // This request should be cached until manually invalidated.
  // Similar to `getStaticProps`.
  // `force-cache` is the default and can be omitted.
  const staticData = await fetch(`https://...`, { cache: 'force-cache' })

  // This request should be refetched on every request.
  // Similar to `getServerSideProps`.
  const dynamicData = await fetch(`https://...`, { cache: 'no-store' })

  // This request should be cached with a lifetime of 10 seconds.
  // Similar to `getStaticProps` with the `revalidate` option.
  const revalidatedData = await fetch(`https://...`, {
    next: { revalidate: 10 },
  })

  return <div>...</div>
}
```

----------------------------------------

TITLE: Basic Usage of create-next-app CLI (Bash)
DESCRIPTION: This command provides the fundamental syntax for `create-next-app`, allowing users to quickly scaffold a new Next.js project. It accepts an optional `project-name` and various `options` to customize the initial setup, serving as the entry point for creating Next.js applications.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/05-api-reference/06-cli/create-next-app.mdx#_snippet_0

LANGUAGE: bash
CODE:
```
npx create-next-app@latest [project-name] [options]
```

----------------------------------------

TITLE: Passing Data from Server to Client Components via Props
DESCRIPTION: This snippet illustrates how data fetched on the server can be passed down to a Client Component using standard React props. The `Page` Server Component fetches `post.likes` and passes it directly to the `LikeButton` Client Component, which then uses this data for its client-side functionality.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/01-getting-started/07-server-and-client-components.mdx#_snippet_3

LANGUAGE: TypeScript
CODE:
```
import LikeButton from '@/app/ui/like-button'
import { getPost } from '@/lib/data'

export default async function Page({ params }: { params: { id: string } }) {
  const post = await getPost(params.id)

  return <LikeButton likes={post.likes} />
}
```

LANGUAGE: JavaScript
CODE:
```
import LikeButton from '@/app/ui/like-button'
import { getPost } from '@/lib/data'

export default async function Page({ params }) {
  const post = await getPost(params.id)

  return <LikeButton likes={post.likes} />
}
```

LANGUAGE: TypeScript
CODE:
```
'use client'

export default function LikeButton({ likes }: { likes: number }) {
  // ...
}
```

LANGUAGE: JavaScript
CODE:
```
'use client'

export default function LikeButton({ likes }) {
  // ...
}
```

----------------------------------------

TITLE: Using Next.js Image Component in app/page.js (JSX)
DESCRIPTION: This snippet demonstrates the basic usage of the `next/image` component in a Next.js application. It imports the `Image` component and renders an image with specified source, width, height, and alt text, enabling automatic image optimization features provided by Next.js.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/05-api-reference/02-components/image.mdx#_snippet_0

LANGUAGE: JSX
CODE:
```
import Image from 'next/image'

export default function Page() {
  return (
    <Image
      src="/profile.png"
      width={500}
      height={500}
      alt="Picture of the author"
    />
  )
}
```

----------------------------------------

TITLE: Running Next.js Development Server with npm
DESCRIPTION: This command first installs all project dependencies using npm, then starts the Next.js development server. This allows local development and live reloading of the application, typically accessible at `http://localhost:3000`.
SOURCE: https://github.com/vercel/next.js/blob/canary/examples/cms-sanity/README.md#_snippet_11

LANGUAGE: bash
CODE:
```
npm install && npm run dev
```

----------------------------------------

TITLE: Starting Next.js Development Server (Bash)
DESCRIPTION: This snippet provides various command-line options to start the Next.js development server. It allows developers to run the application locally for testing and development purposes, with auto-updates on file changes. The server typically runs on `http://localhost:3000`.
SOURCE: https://github.com/vercel/next.js/blob/canary/packages/create-next-app/templates/default-empty/ts/README-template.md#_snippet_0

LANGUAGE: Bash
CODE:
```
npm run dev
# or
yarn dev
# or
pnpm dev
# or
bun dev
```

----------------------------------------

TITLE: Handling Form Data with Server Functions (JavaScript)
DESCRIPTION: This server-side function, marked with ''use server'', demonstrates how to receive and extract data from a FormData object submitted via an HTML form. It retrieves 'title' and 'content' fields, which can then be used to update a database or perform other server-side operations.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/01-getting-started/10-updating-data.mdx#_snippet_7

LANGUAGE: js
CODE:
```
'use server'

export async function createPost(formData) {
  const title = formData.get('title')
  const content = formData.get('content')

  // Update data
  // Revalidate cache
}
```

----------------------------------------

TITLE: Invalidating Cache with revalidateTag in Next.js Server Action
DESCRIPTION: This server action demonstrates how to use `revalidateTag` to purge cached data associated with the `'my-data'` tag after an operation like adding a post, ensuring the client receives fresh data.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/05-api-reference/04-functions/cacheTag.mdx#_snippet_2

LANGUAGE: TSX
CODE:
```
'use server'

import { revalidateTag } from 'next/cache'

export default async function submit() {
  await addPost()
  revalidateTag('my-data')
}
```

LANGUAGE: JSX
CODE:
```
'use server'

import { revalidateTag } from 'next/cache'

export default async function submit() {
  await addPost()
  revalidateTag('my-data')
}
```

----------------------------------------

TITLE: Redirecting User After Server Action Completion (Next.js)
DESCRIPTION: This code shows how to redirect a user to a new route (`/post/${id}`) after a Server Action completes, using the `redirect` API from `next/navigation`. It's crucial to call `redirect` outside of any `try/catch` blocks. The example also combines this with `revalidateTag` to ensure related cached data is updated before redirection.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/03-building-your-application/02-data-fetching/03-server-actions-and-mutations.mdx#_snippet_14

LANGUAGE: ts
CODE:
```
'use server'

import { redirect } from 'next/navigation'
import { revalidateTag } from 'next/cache'

export async function createPost(id: string) {
  try {
    // ...
  } catch (error) {
    // ...
  }

  revalidateTag('posts') // Update cached posts
  redirect(`/post/${id}`) // Navigate to the new post page
}
```

LANGUAGE: js
CODE:
```
'use server'

import { redirect } from 'next/navigation'
import { revalidateTag } from 'next/cache'

export async function createPost(id) {
  try {
    // ...
  } catch (error) {
    // ...
  }

  revalidateTag('posts') // Update cached posts
  redirect(`/post/${id}`) // Navigate to the new post page
}
```

----------------------------------------

TITLE: Composing Server and Client Components (Initial Example)
DESCRIPTION: This example demonstrates how a Next.js Server Component (`Page`) fetches data and passes it as props to a Client Component (`LikeButton`). The `Page` component runs on the server, handling data fetching, while the `LikeButton` is a client component responsible for interactivity, declared with `'use client'` to indicate its client-side execution.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/01-getting-started/07-server-and-client-components.mdx#_snippet_0

LANGUAGE: TypeScript
CODE:
```
import LikeButton from '@/app/ui/like-button'
import { getPost } from '@/lib/data'

export default async function Page({ params }: { params: { id: string } }) {
  const post = await getPost(params.id)

  return (
    <div>
      <main>
        <h1>{post.title}</h1>
        {/* ... */}
        <LikeButton likes={post.likes} />
      </main>
    </div>
  )
}
```

LANGUAGE: JavaScript
CODE:
```
import LikeButton from '@/app/ui/like-button'
import { getPost } from '@/lib/data'

export default async function Page({ params }) {
  const post = await getPost(params.id)

  return (
    <div>
      <main>
        <h1>{post.title}</h1>
        {/* ... */}
        <LikeButton likes={post.likes} />
      </main>
    </div>
  )
}
```

LANGUAGE: TypeScript
CODE:
```
'use client'

import { useState } from 'react'

export default function LikeButton({ likes }: { likes: number }) {
  // ...
}
```

LANGUAGE: JavaScript
CODE:
```
'use client'

import { useState } from 'react'

export default function LikeButton({ likes }) {
  // ...
}
```

----------------------------------------

TITLE: Migrating Next.js Page Params & SearchParams to Async (JavaScript)
DESCRIPTION: This snippet demonstrates how to update `params` and `searchParams` in Next.js `page.js` to be asynchronous in Next.js 15 for JavaScript. Both props are now Promises, requiring `await` to access their properties.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/02-guides/upgrading/version-15.mdx#_snippet_16

LANGUAGE: javascript
CODE:
```
// Before
export function generateMetadata({ params, searchParams }) {
  const { slug } = params
  const { query } = searchParams
}

export default function Page({ params, searchParams }) {
  const { slug } = params
  const { query } = searchParams
}

// After
export async function generateMetadata(props) {
  const params = await props.params
  const searchParams = await props.searchParams
  const slug = params.slug
  const query = searchParams.query
}

export async function Page(props) {
  const params = await props.params
  const searchParams = await props.searchParams
  const slug = params.slug
  const query = searchParams.query
}
```

----------------------------------------

TITLE: Starting Next.js Development Server (Bash)
DESCRIPTION: These commands initiate the Next.js development server, making the application accessible locally. They provide various package manager options (npm, yarn, pnpm, bun) to run the `dev` script defined in `package.json`.
SOURCE: https://github.com/vercel/next.js/blob/canary/packages/create-next-app/templates/app-tw/ts/README-template.md#_snippet_0

LANGUAGE: bash
CODE:
```
npm run dev
# or
yarn dev
# or
pnpm dev
# or
bun dev
```

----------------------------------------

TITLE: Creating Next.js App with npx
DESCRIPTION: This command initializes a new Next.js project named 'hello-world-app' using `npx` and the 'hello-world' example template. It sets up the basic project structure and dependencies.
SOURCE: https://github.com/vercel/next.js/blob/canary/examples/hello-world/README.md#_snippet_0

LANGUAGE: bash
CODE:
```
npx create-next-app --example hello-world hello-world-app
```

----------------------------------------

TITLE: Installing Dependencies and Running Next.js Locally (Shell)
DESCRIPTION: This snippet provides shell commands to set up and run a Next.js project. It first installs necessary Node.js dependencies using 'npm install' and then starts the development server locally on port 3000 with 'npm run dev'.
SOURCE: https://github.com/vercel/next.js/blob/canary/examples/with-formspree/README.md#_snippet_0

LANGUAGE: Shell
CODE:
```
# Install dependencies
npm install

# Run next locally at localhost:3000
npm run dev
```

----------------------------------------

TITLE: Implementing Partial Prerendering with Suspense in Next.js
DESCRIPTION: This Next.js page component demonstrates how to enable and utilize Partial Prerendering. It sets `experimental_ppr` to true and uses React `Suspense` to wrap `DynamicComponent`, allowing the static `StaticComponent` to render immediately while the dynamic content streams in asynchronously with a `Fallback` UI.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/01-getting-started/12-partial-prerendering.mdx#_snippet_0

LANGUAGE: jsx
CODE:
```
import { Suspense } from 'react'
import StaticComponent from './StaticComponent'
import DynamicComponent from './DynamicComponent'
import Fallback from './Fallback'

export const experimental_ppr = true

export default function Page() {
  return (
    <>
      <StaticComponent />
      <Suspense fallback={<Fallback />}>
        <DynamicComponent />
      </Suspense>
    </>
  )
}
```

----------------------------------------

TITLE: Securing Next.js API Route with Authentication and Authorization (TypeScript)
DESCRIPTION: This TypeScript snippet demonstrates how to secure a Next.js API Route by implementing both authentication and authorization checks. It first verifies if a user session exists, returning a 401 if not, and then checks if the authenticated user possesses the required 'admin' role before allowing access to the route's core logic.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/02-guides/authentication.mdx#_snippet_45

LANGUAGE: ts
CODE:
```
import { NextApiRequest, NextApiResponse } from 'next'

export default async function handler(
  req: NextApiRequest,
  res: NextApiResponse
) {
  const session = await getSession(req)

  // Check if the user is authenticated
  if (!session) {
    res.status(401).json({
      error: 'User is not authenticated',
    })
    return
  }

  // Check if the user has the 'admin' role
  if (session.user.role !== 'admin') {
    res.status(401).json({
      error: 'Unauthorized access: User does not have admin privileges.',
    })
    return
  }

  // Proceed with the route for authorized users
  // ... implementation of the API Route
}
```

----------------------------------------

TITLE: Page Component Using Synchronous Token Retrieval (Before Next.js 15)
DESCRIPTION: This `Page` component imports and calls the synchronous `getToken` function. In Next.js 15, if `getToken` is not refactored to be async, this will lead to an error due to the `cookies()` function returning a Promise.
SOURCE: https://github.com/vercel/next.js/blob/canary/errors/next-prerender-sync-headers.mdx#_snippet_1

LANGUAGE: jsx
CODE:
```
import { getToken } from '.../token-utils'

export default function Page() {
  const token = getToken();
  validateToken(token)
  return ...
}
```

----------------------------------------

TITLE: Fetching Data with Server-Only Environment Variable in Next.js
DESCRIPTION: This function demonstrates fetching data from an external service using an API_KEY from process.env. Without proper protection, this server-only environment variable could accidentally be exposed to the client if the module is imported into a Client Component, leading to environment poisoning.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/01-getting-started/07-server-and-client-components.mdx#_snippet_11

LANGUAGE: TypeScript
CODE:
```
export async function getData() {
  const res = await fetch('https://external-service.com/data', {
    headers: {
      authorization: process.env.API_KEY,
    },
  })

  return res.json()
}
```

LANGUAGE: JavaScript
CODE:
```
export async function getData() {
  const res = await fetch('https://external-service.com/data', {
    headers: {
      authorization: process.env.API_KEY,
    },
  })

  return res.json()
}
```

----------------------------------------

TITLE: Creating Root Layout for Next.js App Router (JavaScript)
DESCRIPTION: This JavaScript React component defines the mandatory root layout for applications using the Next.js App Router. It wraps the entire application, requiring `<html>` and `<body>` tags, and receives `children` as a prop to render nested routes and components within the layout.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/01-getting-started/01-installation.mdx#_snippet_4

LANGUAGE: javascript
CODE:
```
export default function RootLayout({ children }) {
  return (
    <html lang="en">
      <body>{children}</body>
    </html>
  )
}
```

----------------------------------------

TITLE: Implementing Next.js Draft Mode Route Handler
DESCRIPTION: Implements a Next.js Route Handler (/api/draft) to enable draft mode. It validates a secret token and a content slug provided in the query parameters, fetches content details from a headless CMS using the slug, and redirects the user to the content's path after successfully enabling draft mode. Includes security considerations for the redirect.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/02-guides/draft-mode.mdx#_snippet_3

LANGUAGE: typescript
CODE:
```
import { draftMode } from 'next/headers';
import { redirect } from 'next/navigation';

export async function GET(request: Request) {
  // Parse query string parameters
  const { searchParams } = new URL(request.url);
  const secret = searchParams.get('secret');
  const slug = searchParams.get('slug');

  // Check the secret and next parameters
  // This secret should only be known to this Route Handler and the CMS
  if (secret !== 'MY_SECRET_TOKEN' || !slug) {
    return new Response('Invalid token', { status: 401 });
  }

  // Fetch the headless CMS to check if the provided `slug` exists
  // getPostBySlug would implement the required fetching logic to the headless CMS
  const post = await getPostBySlug(slug);

  // If the slug doesn't exist prevent draft mode from being enabled
  if (!post) {
    return new Response('Invalid slug', { status: 401 });
  }

  // Enable Draft Mode by setting the cookie
  const draft = await draftMode();
  draft.enable();

  // Redirect to the path from the fetched post
  // We don't redirect to searchParams.slug as that might lead to open redirect vulnerabilities
  redirect(post.slug);
}
```

LANGUAGE: javascript
CODE:
```
import { draftMode } from 'next/headers';
import { redirect } from 'next/navigation';

export async function GET(request) {
  // Parse query string parameters
  const { searchParams } = new URL(request.url);
  const secret = searchParams.get('secret');
  const slug = searchParams.get('slug');

  // Check the secret and next parameters
  // This secret should only be known to this Route Handler and the CMS
  if (secret !== 'MY_SECRET_TOKEN' || !slug) {
    return new Response('Invalid token', { status: 401 });
  }

  // Fetch the headless CMS to check if the provided `slug` exists
  // getPostBySlug would implement the required fetching logic to the headless CMS
  const post = await getPostBySlug(slug);

  // If the slug doesn't exist prevent draft mode from being enabled
  if (!post) {
    return new Response('Invalid slug', { status: 401 });
  }

  // Enable Draft Mode by setting the cookie
  const draft = await draftMode();
  draft.enable();

  // Redirect to the path from the fetched post
  // We don't redirect to searchParams.slug as that might lead to open redirect vulnerabilities
  redirect(post.slug);
}
```

----------------------------------------

TITLE: Displaying Pending State with useActionState (TSX)
DESCRIPTION: This client component uses React's useActionState hook to manage the state of a server action (createPost) and display a loading indicator. The 'pending' boolean returned by the hook allows the UI to show a LoadingSpinner while the action is in progress.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/01-getting-started/10-updating-data.mdx#_snippet_10

LANGUAGE: tsx
CODE:
```
'use client'

import { useActionState } from 'react'
import { createPost } from '@/app/actions'
import { LoadingSpinner } from '@/app/ui/loading-spinner'

export function Button() {
  const [state, action, pending] = useActionState(createPost, false)

  return (
    <button onClick={async () => action()}>
      {pending ? <LoadingSpinner /> : 'Create Post'}
    </button>
  )
}
```

----------------------------------------

TITLE: Migrating Server-Side Rendering to App Router with Server Components
DESCRIPTION: This snippet demonstrates how to achieve server-side rendering similar to `getServerSideProps` in the `app` directory using `async` Server Components and the `fetch()` API with `cache: 'no-store'`. This approach colocates data fetching within the component, reducing client-side JavaScript.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/02-guides/migrating/app-router-migration.mdx#_snippet_24

LANGUAGE: tsx
CODE:
```
// `app` directory

// This function can be named anything
async function getProjects() {
  const res = await fetch(`https://...`, { cache: 'no-store' })
  const projects = await res.json()

  return projects
}

export default async function Dashboard() {
  const projects = await getProjects()

  return (
    <ul>
      {projects.map((project) => (
        <li key={project.id}>{project.name}</li>
      ))}
    </ul>
  )
}
```

LANGUAGE: jsx
CODE:
```
// `app` directory

// This function can be named anything
async function getProjects() {
  const res = await fetch(`https://...`, { cache: 'no-store' })
  const projects = await res.json()

  return projects
}

export default async function Dashboard() {
  const projects = await getProjects()

  return (
    <ul>
      {projects.map((project) => (
        <li key={project.id}>{project.name}</li>
      ))}
    </ul>
  )
}
```

----------------------------------------

TITLE: Handling Form Submissions with Server Actions and FormData (Next.js)
DESCRIPTION: This snippet demonstrates how to create a Server Action function (`createInvoice`) that automatically receives the `FormData` object from an HTML form submission. It shows how to extract individual field values using `formData.get()` and how to assign the Server Action to the form's `action` attribute. This pattern is used for processing form data directly on the server.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/02-guides/forms.mdx#_snippet_0

LANGUAGE: TypeScript
CODE:
```
export default function Page() {
  async function createInvoice(formData: FormData) {
    'use server'

    const rawFormData = {
      customerId: formData.get('customerId'),
      amount: formData.get('amount'),
      status: formData.get('status')
    }

    // mutate data
    // revalidate the cache
  }

  return <form action={createInvoice}>...</form>
}
```

LANGUAGE: JavaScript
CODE:
```
export default function Page() {
  async function createInvoice(formData) {
    'use server'

    const rawFormData = {
      customerId: formData.get('customerId'),
      amount: formData.get('amount'),
      status: formData.get('status')
    }

    // mutate data
    // revalidate the cache
  }

  return <form action={createInvoice}>...</form>
}
```

----------------------------------------

TITLE: Initializing Next.js Project Interactively (CLI)
DESCRIPTION: This snippet demonstrates how to start a new Next.js project using `create-next-app` in interactive mode. It provides commands for `npx`, `yarn`, `pnpm`, and `bunx`, allowing the user to choose their preferred package manager. The interactive prompts guide the user through project setup, including project name and TypeScript configuration.
SOURCE: https://github.com/vercel/next.js/blob/canary/packages/create-next-app/README.md#_snippet_0

LANGUAGE: bash
CODE:
```
npx create-next-app@latest
# or
yarn create next-app
# or
pnpm create next-app
# or
bunx create-next-app
```

----------------------------------------

TITLE: Configuring Twitter App Card in Next.js
DESCRIPTION: This snippet illustrates how to configure the `twitter` metadata property in Next.js for an "app" card. It includes details for `card`, `title`, `description`, `siteId`, `creator`, `creatorId`, `images` (with `url` and `alt`), and specific `app` properties for different platforms (iPhone, iPad, Google Play) including names, IDs, and URLs. This enables deep linking to mobile apps from Twitter cards.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/05-api-reference/04-functions/generate-metadata.mdx#_snippet_37

LANGUAGE: jsx
CODE:
```
export const metadata = {
  twitter: {
    card: 'app',
    title: 'Next.js',
    description: 'The React Framework for the Web',
    siteId: '1467726470533754880',
    creator: '@nextjs',
    creatorId: '1467726470533754880',
    images: {
      url: 'https://nextjs.org/og.png',
      alt: 'Next.js Logo',
    },
    app: {
      name: 'twitter_app',
      id: {
        iphone: 'twitter_app://iphone',
        ipad: 'twitter_app://ipad',
        googleplay: 'twitter_app://googleplay',
      },
      url: {
        iphone: 'https://iphone_url',
        ipad: 'https://ipad_url',
      },
    },
  },
}
```

----------------------------------------

TITLE: Generating Dynamic Links with Next.js Link Component
DESCRIPTION: This snippet illustrates how to create links to dynamic segments in Next.js using the `<Link>` component and JavaScript template literals. It demonstrates mapping over a list of posts to generate individual links with unique slugs, commonly used for blog posts or product pages.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/05-api-reference/02-components/link.mdx#_snippet_12

LANGUAGE: tsx
CODE:
```
import Link from 'next/link'

interface Post {
  id: number
  title: string
  slug: string
}

export default function PostList({ posts }: { posts: Post[] }) {
  return (
    <ul>
      {posts.map((post) => (
        <li key={post.id}>
          <Link href={`/blog/${post.slug}`}>{post.title}</Link>
        </li>
      ))}
    </ul>
  )
}
```

LANGUAGE: jsx
CODE:
```
import Link from 'next/link'

export default function PostList({ posts }) {
  return (
    <ul>
      {posts.map((post) => (
        <li key={post.id}>
          <Link href={`/blog/${post.slug}`}>{post.title}</Link>
        </li>
      ))}
    </ul>
  )
}
```

----------------------------------------

TITLE: Calling revalidateTag in Next.js Route Handler/Server Action (TS/JS)
DESCRIPTION: This snippet demonstrates how to invoke `revalidateTag` within a Next.js Route Handler or Server Action. After a data mutation, calling `revalidateTag('user')` invalidates all cache entries previously tagged with 'user', ensuring that subsequent requests fetch fresh data.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/01-getting-started/09-caching-and-revalidating.mdx#_snippet_7

LANGUAGE: TypeScript
CODE:
```
import { revalidateTag } from 'next/cache'

export async function updateUser(id: string) {
  // Mutate data
  revalidateTag('user')
}
```

LANGUAGE: JavaScript
CODE:
```
import { revalidateTag } from 'next/cache'

export async function updateUser(id) {
  // Mutate data
  revalidateTag('user')
}
```

----------------------------------------

TITLE: Implementing a Dynamic Blog Post Page with Data Fetching in Next.js (TSX/JSX)
DESCRIPTION: This snippet implements a dynamic page for individual blog posts, fetching post data based on the slug parameter. It showcases how dynamic segments ([slug]) are used to generate multiple pages from data, such as blog posts, by accessing route parameters.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/01-getting-started/03-layouts-and-pages.mdx#_snippet_5

LANGUAGE: TypeScript
CODE:
```
export default async function BlogPostPage({
  params,
}: {
  params: Promise<{ slug: string }>
}) {
  const { slug } = await params
  const post = await getPost(slug)

  return (
    <div>
      <h1>{post.title}</h1>
      <p>{post.content}</p>
    </div>
  )
}
```

LANGUAGE: JavaScript
CODE:
```
export default async function BlogPostPage({ params }) {
  const { slug } = await params
  const post = await getPost(slug)

  return (
    <div>
      <h1>{post.title}</h1>
      <p>{post.content}</p>
    </div>
  )
}
```

----------------------------------------

TITLE: Invalidating Specific Tags in Next.js Server Actions
DESCRIPTION: This server action shows how to use `revalidateTag` to invalidate the cache specifically for entries tagged with `'bookings-data'` after an update operation, ensuring data consistency across the application.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/05-api-reference/04-functions/cacheTag.mdx#_snippet_6

LANGUAGE: TSX
CODE:
```
'use server'

import { revalidateTag } from 'next/cache'

export async function updateBookings() {
  await updateBookingData()
  revalidateTag('bookings-data')
}
```

LANGUAGE: JSX
CODE:
```
'use server'

import { revalidateTag } from 'next/cache'

export async function updateBookings() {
  await updateBookingData()
  revalidateTag('bookings-data')
}
```

----------------------------------------

TITLE: Basic Navigation with Next.js Link Component
DESCRIPTION: This snippet demonstrates the fundamental usage of the `next/link` component for client-side navigation. It imports `Link` and renders it with a simple string `href` prop, directing the user to the `/dashboard` route upon clicking. This approach enables fast, pre-fetched transitions without full page reloads.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/05-api-reference/02-components/link.mdx#_snippet_0

LANGUAGE: tsx
CODE:
```
import Link from 'next/link'

export default function Page() {
  return <Link href="/dashboard">Dashboard</Link>
}
```

LANGUAGE: jsx
CODE:
```
import Link from 'next/link'

export default function Page() {
  return <Link href="/dashboard">Dashboard</Link>
}
```

LANGUAGE: tsx
CODE:
```
import Link from 'next/link'

export default function Home() {
  return <Link href="/dashboard">Dashboard</Link>
}
```

LANGUAGE: jsx
CODE:
```
import Link from 'next/link'

export default function Home() {
  return <Link href="/dashboard">Dashboard</Link>
}
```

----------------------------------------

TITLE: Navigating with Link Component in Next.js (TSX/JSX)
DESCRIPTION: This snippet demonstrates the basic usage of the `Link` component from `next/link` for client-side navigation in Next.js. It renders an anchor tag that, when clicked, navigates to the `/dashboard` route. This is the primary and recommended way for declarative navigation.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/03-building-your-application/01-routing/04-linking-and-navigating.mdx#_snippet_0

LANGUAGE: tsx
CODE:
```
import Link from 'next/link'

export default function Page() {
  return <Link href="/dashboard">Dashboard</Link>
}
```

LANGUAGE: jsx
CODE:
```
import Link from 'next/link'

export default function Page() {
  return <Link href="/dashboard">Dashboard</Link>
}
```

----------------------------------------

TITLE: Creating Home Page for Next.js App Router (TypeScript)
DESCRIPTION: This TypeScript React component defines the home page for the Next.js App Router, rendered when a user visits the root URL (`/`). It serves as the entry point for the application's main content, demonstrating a simple 'Hello, Next.js!' heading.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/01-getting-started/01-installation.mdx#_snippet_5

LANGUAGE: typescript
CODE:
```
export default function Page() {
  return <h1>Hello, Next.js!</h1>
}
```

----------------------------------------

TITLE: Managing Cookies in Next.js Route Handlers
DESCRIPTION: This snippet demonstrates how to interact with cookies within a Next.js route handler using the `cookies` utility from `next/headers`. It shows how to retrieve, set, and delete cookies from the incoming request or outgoing response. This functionality is crucial for session management and personalized user experiences.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/05-api-reference/03-file-conventions/route.mdx#_snippet_4

LANGUAGE: typescript
CODE:
```
import { cookies } from 'next/headers'

export async function GET(request: NextRequest) {
  const cookieStore = await cookies()

  const a = cookieStore.get('a')
  const b = cookieStore.set('b', '1')
  const c = cookieStore.delete('c')
}
```

LANGUAGE: javascript
CODE:
```
import { cookies } from 'next/headers'

export async function GET(request) {
  const cookieStore = await cookies()

  const a = cookieStore.get('a')
  const b = cookieStore.set('b', '1')
  const c = cookieStore.delete('c')
}
```

----------------------------------------

TITLE: Handling Authentication in Middleware (Recommended) - TypeScript
DESCRIPTION: This snippet illustrates the *new*, recommended way to handle authentication in Next.js Middleware. Instead of returning a direct response, it redirects unauthorized users to a `/login` page, preserving the original path in a search parameter. This aligns with the updated Middleware behavior in Next.js v12.2+.
SOURCE: https://github.com/vercel/next.js/blob/canary/errors/returning-response-body-in-middleware.mdx#_snippet_2

LANGUAGE: TypeScript
CODE:
```
import { NextResponse } from 'next/server'
import type { NextRequest } from 'next/server'
import { isAuthValid } from './lib/auth'

export function middleware(request: NextRequest) {
  // Example function to validate auth
  if (isAuthValid(request)) {
    return NextResponse.next()
  }

  request.nextUrl.searchParams.set('from', request.nextUrl.pathname)
  request.nextUrl.pathname = '/login'

  return NextResponse.redirect(request.nextUrl)
}
```

----------------------------------------

TITLE: Defining Module-Level Server Action for Client Components (TypeScript)
DESCRIPTION: This snippet defines a Server Action at the module level by placing the 'use server' directive at the top of the file. This allows all exported functions within the file to be marked as Server Actions, making them reusable in both Client and Server Components.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/03-building-your-application/02-data-fetching/03-server-actions-and-mutations.mdx#_snippet_2

LANGUAGE: TypeScript
CODE:
```
'use server'

export async function create() {}
```

----------------------------------------

TITLE: Fetching Data in Server Component (TSX)
DESCRIPTION: Demonstrates how to use the extended `fetch` function within an `async` Server Component written in TSX to fetch data from an external API and render it.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/05-api-reference/04-functions/fetch.mdx#_snippet_0

LANGUAGE: tsx
CODE:
```
export default async function Page() {
  let data = await fetch('https://api.vercel.app/blog')
  let posts = await data.json()
  return (
    <ul>
      {posts.map((post) => (
        <li key={post.id}>{post.title}</li>
      ))}
    </ul>
  )
}
```

----------------------------------------

TITLE: Starting Next.js Development Server
DESCRIPTION: Starts the Next.js development server, which watches for code changes and provides hot module reloading. This command is essential for actively developing and testing changes.
SOURCE: https://github.com/vercel/next.js/blob/canary/contributing/core/developing.md#_snippet_5

LANGUAGE: shell
CODE:
```
pnpm dev
```

----------------------------------------

TITLE: Creating Next.js App with Default Template (Bash)
DESCRIPTION: Executing this command without arguments initiates an interactive setup process for a new Next.js application. It prompts the user for preferences regarding TypeScript, ESLint, Tailwind CSS, `src/` directory, App Router, Turbopack, and import aliases, simplifying project initialization.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/05-api-reference/06-cli/create-next-app.mdx#_snippet_1

LANGUAGE: bash
CODE:
```
npx create-next-app@latest
```

----------------------------------------

TITLE: Using redirect in a Server Component
DESCRIPTION: Example demonstrating how to use the `redirect` function within an asynchronous Server Component to handle cases where data fetching fails, redirecting the user to a different page. It shows importing `redirect` and conditionally calling it.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/05-api-reference/04-functions/redirect.mdx#_snippet_1

LANGUAGE: tsx
CODE:
```
import { redirect } from 'next/navigation'

async function fetchTeam(id: string) {
  const res = await fetch('https://...')
  if (!res.ok) return undefined
  return res.json()
}

export default async function Profile({
  params,
}: {
  params: Promise<{ id: string }>
}) {
  const { id } = await params
  const team = await fetchTeam(id)

  if (!team) {
    redirect('/login')
  }

  // ...
}
```

LANGUAGE: jsx
CODE:
```
import { redirect } from 'next/navigation'

async function fetchTeam(id) {
  const res = await fetch('https://...')
  if (!res.ok) return undefined
  return res.json()
}

export default async function Profile({ params }) {
  const { id } = await params
  const team = await fetchTeam(id)

  if (!team) {
    redirect('/login')
  }

  // ...
}
```

----------------------------------------

TITLE: Receiving Server Action as Prop in Client Component (JavaScript)
DESCRIPTION: This Client Component receives a Server Action as a prop named 'updateItemAction'. It then uses this action directly in a <form> element's 'action' attribute, demonstrating how forms can directly invoke Server Actions for data mutations.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/03-building-your-application/02-data-fetching/03-server-actions-and-mutations.mdx#_snippet_8

LANGUAGE: JavaScript
CODE:
```
'use client'

export default function ClientComponent({ updateItemAction }) {
  return <form action={updateItemAction}>{/* ... */}</form>
}
```

----------------------------------------

TITLE: Defining Static Metadata with `metadata` Object in Next.js (TypeScript)
DESCRIPTION: This snippet demonstrates how to define static metadata by exporting a `Metadata` object from a `layout.tsx` or `page.tsx` file. This method is suitable for metadata that does not change based on dynamic information, improving SEO and web shareability. It requires importing the `Metadata` type from 'next'.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/05-api-reference/04-functions/generate-metadata.mdx#_snippet_0

LANGUAGE: tsx
CODE:
```
import type { Metadata } from 'next'

export const metadata: Metadata = {
  title: '...',
  description: '...',
}

export default function Page() {}
```

----------------------------------------

TITLE: Defining a Dashboard Layout Component in Next.js
DESCRIPTION: This snippet demonstrates how to define a layout component for a specific route segment, such as a dashboard. It accepts a `children` prop, which will be populated with the nested route segments, and wraps them within a `<section>` element. This pattern allows for shared UI across multiple pages within a specific part of the application.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/05-api-reference/03-file-conventions/layout.mdx#_snippet_0

LANGUAGE: typescript
CODE:
```
export default function DashboardLayout({
  children,
}: {
  children: React.ReactNode
}) {
  return <section>{children}</section>
}
```

LANGUAGE: javascript
CODE:
```
export default function DashboardLayout({ children }) {
  return <section>{children}</section>
}
```

----------------------------------------

TITLE: Running Next.js in Development Mode with npm
DESCRIPTION: These commands first install all project dependencies using `npm install`, then start the Next.js development server with `npm run dev`. This allows you to view and test the application locally, typically accessible at `http://localhost:3000`.
SOURCE: https://github.com/vercel/next.js/blob/canary/examples/cms-graphcms/README.md#_snippet_4

LANGUAGE: bash
CODE:
```
npm install
npm run dev
```

----------------------------------------

TITLE: Defining a Root Layout in Next.js
DESCRIPTION: This snippet illustrates how to define a root layout in a Next.js application. Layouts provide shared UI across multiple pages, preserving state and interactivity on navigation. A layout is defined by default exporting a React component from a `layout` file, accepting a `children` prop to render nested pages or layouts. Root layouts are required and must include `html` and `body` tags.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/01-getting-started/03-layouts-and-pages.mdx#_snippet_1

LANGUAGE: tsx
CODE:
```
export default function DashboardLayout({
  children,
}: {
  children: React.ReactNode
}) {
  return (
    <html lang="en">
      <body>
        {/* Layout UI */}
        {/* Place children where you want to render a page or nested layout */}
        <main>{children}</main>
      </body>
    </html>
  )
}
```

LANGUAGE: jsx
CODE:
```
export default function DashboardLayout({ children }) {
  return (
    <html lang="en">
      <body>
        {/* Layout UI */}
        {/* Place children where you want to render a page or nested layout */}
        <main>{children}</main>
      </body>
    </html>
  )
}
```

----------------------------------------

TITLE: Running Next.js Development Server
DESCRIPTION: These commands initiate the Next.js development server, making the application accessible locally at `http://localhost:3000`. They demonstrate how to execute the `dev` script using various popular JavaScript package managers.
SOURCE: https://github.com/vercel/next.js/blob/canary/packages/create-next-app/templates/default/js/README-template.md#_snippet_0

LANGUAGE: bash
CODE:
```
npm run dev
```

LANGUAGE: bash
CODE:
```
yarn dev
```

LANGUAGE: bash
CODE:
```
pnpm dev
```

LANGUAGE: bash
CODE:
```
bun dev
```

----------------------------------------

TITLE: Defining a Basic Next.js Server Action
DESCRIPTION: This snippet shows the basic structure for defining a Server Action in Next.js. The 'use server' directive marks the function to be executed on the server. This action can then be imported and called directly from client or server components.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/02-guides/single-page-applications.mdx#_snippet_9

LANGUAGE: tsx
CODE:
```
'use server'

export async function create() {}
```

LANGUAGE: js
CODE:
```
'use server'

export async function create() {}
```

----------------------------------------

TITLE: Running Next.js Development Server with Yarn
DESCRIPTION: These commands are used to install project dependencies and start the Next.js development server locally. `yarn install` fetches all required packages, and `yarn dev` launches the application, typically accessible at `http://localhost:3000`.
SOURCE: https://github.com/vercel/next.js/blob/canary/examples/cms-agilitycms/README.md#_snippet_6

LANGUAGE: Shell
CODE:
```
yarn install
```

LANGUAGE: Shell
CODE:
```
yarn dev
```

----------------------------------------

TITLE: Defining the Global Root Layout in Next.js
DESCRIPTION: This example illustrates the definition of the top-most root layout for a Next.js application. This layout is mandatory and is responsible for defining the `<html>` and `<body>` tags, along with any other globally shared UI elements. The `children` prop represents the content of the nested route segments.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/05-api-reference/03-file-conventions/layout.mdx#_snippet_1

LANGUAGE: typescript
CODE:
```
export default function RootLayout({
  children,
}: {
  children: React.ReactNode
}) {
  return (
    <html lang="en">
      <body>{children}</body>
    </html>
  )
}
```

LANGUAGE: javascript
CODE:
```
export default function RootLayout({ children }) {
  return (
    <html lang="en">
      <body>{children}</body>
    </html>
  )
}
```

----------------------------------------

TITLE: Running Next.js Development Server
DESCRIPTION: This snippet provides various commands to start the Next.js development server using different package managers. It allows developers to run the application locally for testing and development purposes, typically accessible at http://localhost:3000.
SOURCE: https://github.com/vercel/next.js/blob/canary/packages/create-next-app/templates/app-api/ts/README-template.md#_snippet_0

LANGUAGE: bash
CODE:
```
npm run dev
```

LANGUAGE: bash
CODE:
```
yarn dev
```

LANGUAGE: bash
CODE:
```
pnpm dev
```

LANGUAGE: bash
CODE:
```
bun dev
```

----------------------------------------

TITLE: Running Next.js in Development Mode (Bash)
DESCRIPTION: These commands are used to set up and run the Next.js application in development mode. `npm install` installs all project dependencies, and `npm run dev` starts the development server, enabling hot-reloading and other development features.
SOURCE: https://github.com/vercel/next.js/blob/canary/examples/cms-agilitycms/README.md#_snippet_5

LANGUAGE: bash
CODE:
```
npm install
npm run dev
```

----------------------------------------

TITLE: Dynamic Redirects using Edge Config in Next.js Middleware
DESCRIPTION: This middleware snippet shows how to implement dynamic redirects by fetching redirect rules from a data store like Vercel's Edge Config. It retrieves redirect data based on the incoming request's pathname and performs a redirect with the appropriate status code (307 or 308) if a match is found.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/02-guides/redirecting.mdx#_snippet_10

LANGUAGE: typescript
CODE:
```
import { NextResponse, NextRequest } from 'next/server'
import { get } from '@vercel/edge-config'

type RedirectEntry = {
  destination: string
  permanent: boolean
}

export async function middleware(request: NextRequest) {
  const pathname = request.nextUrl.pathname
  const redirectData = await get(pathname)

  if (redirectData && typeof redirectData === 'string') {
    const redirectEntry: RedirectEntry = JSON.parse(redirectData)
    const statusCode = redirectEntry.permanent ? 308 : 307
    return NextResponse.redirect(redirectEntry.destination, statusCode)
  }

  // No redirect found, continue without redirecting
  return NextResponse.next()
}
```

LANGUAGE: javascript
CODE:
```
import { NextResponse } from 'next/server'
import { get } from '@vercel/edge-config'

export async function middleware(request) {
  const pathname = request.nextUrl.pathname
  const redirectData = await get(pathname)

  if (redirectData) {
    const redirectEntry = JSON.parse(redirectData)
    const statusCode = redirectEntry.permanent ? 308 : 307
    return NextResponse.redirect(redirectEntry.destination, statusCode)
  }

  // No redirect found, continue without redirecting
  return NextResponse.next()
}
```

----------------------------------------

TITLE: Creating an Index Page in Next.js
DESCRIPTION: This snippet demonstrates how to create an index page (`/`) in a Next.js application. A page is UI rendered on a specific route, defined by default exporting a React component from a `page` file within the `app` directory. It serves as the entry point for the root route.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/01-getting-started/03-layouts-and-pages.mdx#_snippet_0

LANGUAGE: tsx
CODE:
```
export default function Page() {
  return <h1>Hello Next.js!</h1>
}
```

LANGUAGE: jsx
CODE:
```
export default function Page() {
  return <h1>Hello Next.js!</h1>
}
```

----------------------------------------

TITLE: Creating a Server Component Page with Data Fetching (TypeScript)
DESCRIPTION: Defines a Server Component page that imports and renders a Client Component. It demonstrates the new data fetching API by asynchronously fetching posts directly within the Server Component and passing the data as props to the Client Component. This replaces `getServerSideProps` or `getStaticProps`.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/02-guides/migrating/app-router-migration.mdx#_snippet_19

LANGUAGE: tsx
CODE:
```
// Import your Client Component
import HomePage from './home-page'

async function getPosts() {
  const res = await fetch('https://...')
  const posts = await res.json()
  return posts
}

export default async function Page() {
  // Fetch data directly in a Server Component
  const recentPosts = await getPosts()
  // Forward fetched data to your Client Component
  return <HomePage recentPosts={recentPosts} />
}
```

----------------------------------------

TITLE: Starting Next.js Development Server
DESCRIPTION: This snippet provides commands to start the Next.js development server using various JavaScript package managers. Running one of these commands will launch the application locally, typically accessible at `http://localhost:3000`, and enable hot module reloading for development.
SOURCE: https://github.com/vercel/next.js/blob/canary/packages/create-next-app/templates/app-api/js/README-template.md#_snippet_0

LANGUAGE: bash
CODE:
```
npm run dev
# or
yarn dev
# or
pnpm dev
# or
bun dev
```

----------------------------------------

TITLE: Implementing Role-Based Access Control in Next.js Server Components (JavaScript)
DESCRIPTION: This snippet shows how to perform an authentication check within a Next.js Server Component using JavaScript to conditionally render UI based on the user's role. It leverages a verifySession function from a Data Access Layer (DAL) to retrieve the user's role and redirects unauthorized users to a login page.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/02-guides/authentication.mdx#_snippet_37

LANGUAGE: jsx
CODE:
```
import { verifySession } from '@/app/lib/dal'

export default function Dashboard() {
  const session = await verifySession()
  const userRole = session.role // Assuming 'role' is part of the session object

  if (userRole === 'admin') {
    return <AdminDashboard />
  } else if (userRole === 'user') {
    return <UserDashboard />
  } else {
    redirect('/login')
  }
}
```

----------------------------------------

TITLE: Defining `generateMetadata` as an Async Function (TSX)
DESCRIPTION: Illustrates how to define the `generateMetadata` function as an asynchronous function, returning a `Promise<Metadata>`. This allows for fetching data or performing other asynchronous operations before generating metadata.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/05-api-reference/04-functions/generate-metadata.mdx#_snippet_69

LANGUAGE: tsx
CODE:
```
import type { Metadata } from 'next'

export async function generateMetadata(): Promise<Metadata> {
  return {
    title: 'Next.js',
  }
}
```

----------------------------------------

TITLE: Caching ORM/Database Queries with `unstable_cache` in Next.js App Router
DESCRIPTION: Illustrates using `unstable_cache` to cache data retrieved from an ORM or database in the Next.js App Router. It defines a `getCachedPosts` function that wraps a database query, applies a `posts` tag, and sets a revalidation time, allowing for on-demand invalidation via `revalidateTag`.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/02-guides/incremental-static-regeneration.mdx#_snippet_7

LANGUAGE: tsx
CODE:
```
import { unstable_cache } from 'next/cache'
import { db, posts } from '@/lib/db'

const getCachedPosts = unstable_cache(
  async () => {
    return await db.select().from(posts)
  },
  ['posts'],
  { revalidate: 3600, tags: ['posts'] }
)

export default async function Page() {
  const posts = getCachedPosts()
  // ...
}
```

LANGUAGE: jsx
CODE:
```
import { unstable_cache } from 'next/cache'
import { db, posts } from '@/lib/db'

const getCachedPosts = unstable_cache(
  async () => {
    return await db.select().from(posts)
  },
  ['posts'],
  { revalidate: 3600, tags: ['posts'] }
)

export default async function Page() {
  const posts = getCachedPosts()
  // ...
}
```

----------------------------------------

TITLE: Running Development Server for Next.js (Bash)
DESCRIPTION: This snippet provides common commands to start the development server for a Next.js application. Developers can choose their preferred package manager (npm, yarn, pnpm, or bun) to run the application locally and enable hot-reloading for development.
SOURCE: https://github.com/vercel/next.js/blob/canary/packages/create-next-app/templates/app-tw/js/README-template.md#_snippet_0

LANGUAGE: bash
CODE:
```
npm run dev
# or
yarn dev
# or
pnpm dev
# or
bun dev
```

----------------------------------------

TITLE: Implementing Server-Side Form Validation with Zod in Next.js (JavaScript)
DESCRIPTION: This snippet demonstrates server-side form validation using the Zod library within a Next.js Server Action. It defines a schema for an email field and uses `safeParse` to validate incoming `FormData`. If validation fails, it returns an object containing field-specific errors. This is the JavaScript equivalent of the TypeScript example.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/02-guides/forms.mdx#_snippet_4

LANGUAGE: JavaScript
CODE:
```
'use server'

import { z } from 'zod'

const schema = z.object({
  email: z.string({
    invalid_type_error: 'Invalid Email',
  }),
})

export default async function createsUser(formData) {
  const validatedFields = schema.safeParse({
    email: formData.get('email'),
  })

  // Return early if the form data is invalid
  if (!validatedFields.success) {
    return {
      errors: validatedFields.error.flatten().fieldErrors,
    }
  }

  // Mutate data
}
```

----------------------------------------

TITLE: Declaring Client Component Entry Point with use client
DESCRIPTION: Demonstrates the basic usage of the 'use client' directive at the top of a file to declare a component as a Client Component. Shows a simple counter component using React state (useState) and event handling (onClick), requiring client-side JavaScript.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/05-api-reference/01-directives/use-client.mdx#_snippet_0

LANGUAGE: tsx
CODE:
```
'use client'\n\nimport { useState } from 'react'\n\nexport default function Counter() {\n  const [count, setCount] = useState(0)\n\n  return (\n    <div>\n      <p>Count: {count}</p>\n      <button onClick={() => setCount(count + 1)}>Increment</button>\n    </div>\n  )\n}
```

LANGUAGE: jsx
CODE:
```
'use client'\n\nimport { useState } from 'react'\n\nexport default function Counter() {\n  const [count, setCount] = useState(0)\n\n  return (\n    <div>\n      <p>Count: {count}</p>\n      <button onClick={() => setCount(count + 1)}>Increment</button>\n    </div>\n  )\n}
```

----------------------------------------

TITLE: Loading Multiple Modules Dynamically (Recommended) - Next.js JSX
DESCRIPTION: This snippet shows the recommended way to dynamically load multiple modules in Next.js. Instead of a single `dynamic` call with a `modules` property, each component (`Hello1`, `Hello2`) is imported using its own `dynamic` call, aligning with React's `lazy` loading pattern for better modularity and compatibility.
SOURCE: https://github.com/vercel/next.js/blob/canary/errors/next-dynamic-modules.mdx#_snippet_1

LANGUAGE: jsx
CODE:
```
import dynamic from 'next/dynamic'

const Hello1 = dynamic(() => import('../components/hello1'))
const Hello2 = dynamic(() => import('../components/hello2'))

function HelloBundle({ title }) {
  return (
    <div>
      <h1>{title}</h1>
      <Hello1 />
      <Hello2 />
    </div>
  )
}

function DynamicBundle() {
  return <HelloBundle title="Dynamic Bundle" />
}

export default DynamicBundle
```

----------------------------------------

TITLE: Secure Next.js Preview API Route with Token and Slug Validation
DESCRIPTION: This advanced Next.js API route (`pages/api/preview.js`) securely enables Preview Mode. It validates a `secret` token and a `slug` query parameter, fetches content from a headless CMS using `getPostBySlug`, and prevents preview mode if the content doesn't exist. Upon successful validation, it calls `res.setPreviewData({})` to enable Preview Mode and then redirects the user to the fetched content's path, preventing open redirect vulnerabilities.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/02-pages/02-guides/preview-mode.mdx#_snippet_2

LANGUAGE: JavaScript
CODE:
```
export default async (req, res) => {
  // Check the secret and next parameters
  // This secret should only be known to this API route and the CMS
  if (req.query.secret !== 'MY_SECRET_TOKEN' || !req.query.slug) {
    return res.status(401).json({ message: 'Invalid token' })
  }

  // Fetch the headless CMS to check if the provided `slug` exists
  // getPostBySlug would implement the required fetching logic to the headless CMS
  const post = await getPostBySlug(req.query.slug)

  // If the slug doesn't exist prevent preview mode from being enabled
  if (!post) {
    return res.status(401).json({ message: 'Invalid slug' })
  }

  // Enable Preview Mode by setting the cookies
  res.setPreviewData({})

  // Redirect to the path from the fetched post
  // We don't redirect to req.query.slug as that might lead to open redirect vulnerabilities
  res.redirect(post.slug)
}
```

----------------------------------------

TITLE: Defining Server Functions in a Separate File
DESCRIPTION: This snippet demonstrates how to define Server Functions in a dedicated file (e.g., `app/lib/actions.ts` or `.js`) by placing the `'use server'` directive at the top of each asynchronous function. These functions, `createPost` and `deletePost`, are intended for updating data and revalidating the cache on the server.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/01-getting-started/10-updating-data.mdx#_snippet_0

LANGUAGE: TypeScript
CODE:
```
export async function createPost(formData: FormData) {
  'use server'
  const title = formData.get('title')
  const content = formData.get('content')

  // Update data
  // Revalidate cache
}

export async function deletePost(formData: FormData) {
  'use server'
  const id = formData.get('id')

  // Update data
  // Revalidate cache
}
```

LANGUAGE: JavaScript
CODE:
```
export async function createPost(formData) {
  'use server'
  const title = formData.get('title')
  const content = formData.get('content')

  // Update data
  // Revalidate cache
}

export async function deletePost(formData) {
  'use server'
  const id = formData.get('id')

  // Update data
  // Revalidate cache
}
```

----------------------------------------

TITLE: Defining a Basic Next.js Page Component
DESCRIPTION: This snippet demonstrates how to define a basic `page` component in Next.js, which is the entry point for a route's UI. It shows the default export of a React component that receives `params` and `searchParams` as props, allowing access to dynamic route segments and URL query parameters. Pages are Server Components by default.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/05-api-reference/03-file-conventions/page.mdx#_snippet_0

LANGUAGE: tsx
CODE:
```
export default function Page({
  params,
  searchParams,
}: {
  params: Promise<{ slug: string }>
  searchParams: Promise<{ [key: string]: string | string[] | undefined }>
}) {
  return <h1>My Page</h1>
}
```

LANGUAGE: jsx
CODE:
```
export default function Page({ params, searchParams }) {
  return <h1>My Page</h1>
}
```

----------------------------------------

TITLE: Initializing Next.js App with npx
DESCRIPTION: This command initializes a new Next.js application using `npx create-next-app`. It fetches the `with-passport-and-next-connect` example from the Next.js repository and sets up a new project directory with the specified name. This is the recommended way to start a new Next.js project.
SOURCE: https://github.com/vercel/next.js/blob/canary/examples/with-passport-and-next-connect/README.md#_snippet_0

LANGUAGE: bash
CODE:
```
npx create-next-app --example with-passport-and-next-connect with-passport-and-next-connect-app
```

----------------------------------------

TITLE: Applying Google Font to Root Layout in Next.js (Geist)
DESCRIPTION: This snippet demonstrates how to import a Google Font using `next/font/google`, initialize it with specified subsets, and apply its generated class name to the `<html>` element in the Root Layout. This ensures the font is applied globally across the Next.js application, leveraging `next/font` for automatic optimization and self-hosting.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/01-getting-started/05-fonts.mdx#_snippet_0

LANGUAGE: tsx
CODE:
```
import { Geist } from 'next/font/google'

const geist = Geist({
  subsets: ['latin'],
})

export default function Layout({ children }: { children: React.ReactNode }) {
  return (
    <html lang="en" className={geist.className}>
      <body>{children}</body>
    </html>
  )
}
```

LANGUAGE: jsx
CODE:
```
import { Geist } from 'next/font/google'

const geist = Geist({
  subsets: ['latin'],
})

export default function Layout({ children }) {
  return (
    <html className={geist.className}>
      <body>{children}</body>
    </html>
  )
}
```

----------------------------------------

TITLE: Defining Base Metadata in Next.js Layout (JSX)
DESCRIPTION: This snippet defines the foundational metadata for a Next.js application within `app/layout.js`. It establishes default `title` and `openGraph` properties that serve as base values for all child routes, demonstrating the initial metadata configuration.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/05-api-reference/04-functions/generate-metadata.mdx#_snippet_81

LANGUAGE: jsx
CODE:
```
export const metadata = {
  title: 'Acme',
  openGraph: {
    title: 'Acme',
    description: 'Acme is a...', 
  },
}
```

----------------------------------------

TITLE: Configuring Metadata with Next.js Metadata API - Next.js (JavaScript)
DESCRIPTION: This snippet demonstrates configuring page metadata using Next.js's `metadata` object API. It exports a `metadata` constant from `app/layout.js` to define `title` and `description`, allowing Next.js to manage these tags automatically and improving SEO. Dependencies include React and Next.js.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/02-guides/migrating/from-create-react-app.mdx#_snippet_9

LANGUAGE: jsx
CODE:
```
export const metadata = {
  title: 'React App',
  description: 'Web site created with Next.js.',
}

export default function RootLayout({ children }) {
  return (
    <html lang="en">
      <body>
        <div id="root">{children}</div>
      </body>
    </html>
  )
}
```

----------------------------------------

TITLE: Revalidating All Data - Next.js Cache - TypeScript
DESCRIPTION: This snippet demonstrates how to revalidate all cached data by calling `revalidatePath` with the root path ('/') and the 'layout' type. This action purges the Client-side Router Cache and revalidates the Data Cache for the entire application on the next page visit.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/05-api-reference/04-functions/revalidatePath.mdx#_snippet_4

LANGUAGE: ts
CODE:
```
import { revalidatePath } from 'next/cache'

revalidatePath('/', 'layout')
```

----------------------------------------

TITLE: Parallel Data Fetching with Promise.all in Next.js (TypeScript)
DESCRIPTION: This snippet illustrates how to perform parallel data fetching in a Next.js page using `Promise.all`. By initiating `getArtist` and `getAlbums` concurrently and then awaiting their resolution together, it avoids sequential blocking, improving performance. Note that `Promise.all` will fail if any of the wrapped promises reject.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/01-getting-started/08-fetching-data.mdx#_snippet_12

LANGUAGE: TypeScript
CODE:
```
import Albums from './albums'

async function getArtist(username: string) {
  const res = await fetch(`https://api.example.com/artist/${username}`)
  return res.json()
}

async function getAlbums(username: string) {
  const res = await fetch(`https://api.example.com/artist/${username}/albums`)
  return res.json()
}

export default async function Page({
  params,
}: {
  params: Promise<{ username: string }>
}) {
  const { username } = await params
  const artistData = getArtist(username)
  const albumsData = getAlbums(username)

  // Initiate both requests in parallel
  const [artist, albums] = await Promise.all([artistData, albumsData])

  return (
    <>
      <h1>{artist.name}</h1>
      <Albums list={albums} />
    </>
  )
}
```

----------------------------------------

TITLE: Fetching Data with `fetch` API in Next.js Server Components
DESCRIPTION: This snippet demonstrates how to fetch data using the native `fetch` API within a Next.js Server Component. The component is an `async` function that awaits the `fetch` call to an external API, processes the JSON response, and renders a list of posts. This approach is suitable for server-side data fetching.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/01-getting-started/08-fetching-data.mdx#_snippet_0

LANGUAGE: tsx
CODE:
```
export default async function Page() {
  const data = await fetch('https://api.vercel.app/blog')
  const posts = await data.json()
  return (
    <ul>
      {posts.map((post) => (
        <li key={post.id}>{post.title}</li>
      ))}
    </ul>
  )
}
```

LANGUAGE: jsx
CODE:
```
export default async function Page() {
  const data = await fetch('https://api.vercel.app/blog')
  const posts = await data.json()
  return (
    <ul>
      {posts.map((post) => (
        <li key={post.id}>{post.title}</li>
      ))}
    </ul>
  )
}
```

----------------------------------------

TITLE: Signup Form with Client-Side Error Display (JSX)
DESCRIPTION: This React component, `SignupForm`, uses the `useActionState` hook to manage form submission and display server-side validation errors. It binds the form's `action` to the `signup` Server Action and conditionally renders error messages for the name field based on the `state` returned by the action. This snippet shows the basic structure in JavaScript.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/02-guides/authentication.mdx#_snippet_7

LANGUAGE: JSX
CODE:
```
'use client'

import { signup } from '@/app/actions/auth'
import { useActionState } from 'react'

export default function SignupForm() {
  const [state, action, pending] = useActionState(signup, undefined)

  return (
    <form action={action}>
      <div>
        <label htmlFor="name">Name</label>
        <input id="name" name="name" placeholder="Name" />
      </div>
      {state?.errors?.name && <p>{state.errors.name}</p>}


```

----------------------------------------

TITLE: Basic Usage of Next.js Image Component
DESCRIPTION: This snippet demonstrates the basic import and usage of the Next.js `<Image>` component. It shows how to import `Image` from `next/image` and render it within a React component, requiring `src` and `alt` properties.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/01-getting-started/04-images.mdx#_snippet_0

LANGUAGE: tsx
CODE:
```
import Image from 'next/image'

export default function Page() {
  return <Image src="" alt="" />
}
```

LANGUAGE: jsx
CODE:
```
import Image from 'next/image'

export default function Page() {
  return <Image src="" alt="" />
}
```

----------------------------------------

TITLE: Creating a Nested Layout for Blog Routes in Next.js (TSX/JSX)
DESCRIPTION: This snippet defines a nested layout component for the /blog route. It demonstrates how layouts wrap their children using the children prop, providing a common structure for all pages within the /blog segment. This layout will be nested under the root layout.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/01-getting-started/03-layouts-and-pages.mdx#_snippet_4

LANGUAGE: TypeScript
CODE:
```
export default function BlogLayout({
  children,
}: {
  children: React.ReactNode
}) {
  return <section>{children}</section>
}
```

LANGUAGE: JavaScript
CODE:
```
export default function BlogLayout({ children }) {
  return <section>{children}</section>
}
```

----------------------------------------

TITLE: Running Next.js in Development Mode (Bash)
DESCRIPTION: These bash commands are used to install project dependencies and start the Next.js development server. The `npm install` or `yarn install` command fetches required packages, and `npm run dev` or `yarn dev` launches the application, typically accessible at `http://localhost:3000`.
SOURCE: https://github.com/vercel/next.js/blob/canary/examples/cms-enterspeed/README.md#_snippet_9

LANGUAGE: bash
CODE:
```
npm install
npm run dev

# or

yarn install
yarn dev
```

----------------------------------------

TITLE: Creating a Next.js App via CLI (Bash)
DESCRIPTION: Shows how to use the `npx create-next-app` command in the terminal to initialize a new Next.js project, demonstrating the use of the `filename` prop for command-line examples.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/04-community/01-contribution-guide.mdx#_snippet_9

LANGUAGE: bash
CODE:
```
npx create-next-app
```

----------------------------------------

TITLE: Starting Next.js Development Server (Bash)
DESCRIPTION: This snippet provides commands to initiate the Next.js development server using different JavaScript package managers. Executing any of these commands will launch the application locally, typically accessible via `http://localhost:3000`, allowing for real-time page updates during development.
SOURCE: https://github.com/vercel/next.js/blob/canary/packages/create-next-app/templates/app-tw-empty/js/README-template.md#_snippet_0

LANGUAGE: bash
CODE:
```
npm run dev
# or
yarn dev
# or
pnpm dev
# or
bun dev
```

----------------------------------------

TITLE: Running Next.js Development Server with Yarn
DESCRIPTION: This command executes the `dev` script defined in `package.json` using Yarn, starting the Next.js development server. It provides the same functionality as `npm run dev` but uses Yarn.
SOURCE: https://github.com/vercel/next.js/blob/canary/examples/cms-prepr/README.md#_snippet_8

LANGUAGE: bash
CODE:
```
yarn dev
```

----------------------------------------

TITLE: Consuming Promise with React `use` Hook
DESCRIPTION: A Client Component (`Profile`) that uses the custom `useUser` hook to access the Promise from context and then unwraps the Promise using React's `use` hook. The component will suspend while the Promise resolves.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/02-guides/single-page-applications.mdx#_snippet_2

LANGUAGE: TSX
CODE:
```
'use client'

import { use } from 'react'
import { useUser } from './user-provider'

export function Profile() {
  const { userPromise } = useUser()
  const user = use(userPromise)

  return '...'
}
```

LANGUAGE: JSX
CODE:
```
'use client'

import { use } from 'react'
import { useUser } from './user-provider'

export function Profile() {
  const { userPromise } = useUser()
  const user = use(userPromise)

  return '...'
}
```

----------------------------------------

TITLE: Enabling Incremental PPR in Next.js Configuration (JavaScript)
DESCRIPTION: This snippet demonstrates how to enable incremental Partial Prerendering (PPR) in a Next.js application by setting the `ppr` experimental option to `'incremental'` in `next.config.js`. This global configuration allows specific routes to opt into PPR.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/01-getting-started/12-partial-prerendering.mdx#_snippet_2

LANGUAGE: JavaScript
CODE:
```
/** @type {import('next').NextConfig} */
const nextConfig = {
  experimental: {
    ppr: 'incremental',
  },
}
```

----------------------------------------

TITLE: Responsive Image with `fill` and `objectFit` in Next.js
DESCRIPTION: This example demonstrates how to use the `fill` prop in conjunction with `objectFit: 'cover'` to make an image fill its parent container, particularly useful when the image's aspect ratio is unknown. The parent container must have `position: relative` for this to work correctly.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/05-api-reference/02-components/image.mdx#_snippet_48

LANGUAGE: jsx
CODE:
```
import Image from 'next/image'
import mountains from '../public/mountains.jpg'

export default function Fill() {
  return (
    <div
      style={{
        display: 'grid',
        gridGap: '8px',
        gridTemplateColumns: 'repeat(auto-fit, minmax(400px, auto))',
      }}
    >
      <div style={{ position: 'relative', width: '400px' }}>
        <Image
          alt="Mountains"
          src={mountains}
          fill
          sizes="(min-width: 808px) 50vw, 100vw"
          style={{
            objectFit: 'cover', // cover, contain, none
          }}
        />
      </div>
      {/* And more images in the grid... */}
    </div>
  )
}
```

----------------------------------------

TITLE: Importing External Stylesheet in Next.js App Router Root Layout (TypeScript)
DESCRIPTION: This TypeScript React component imports an external CSS library, `bootstrap.css`, into the root layout of a Next.js App Router application. This allows the application to utilize styles from third-party packages globally. The `container` class from Bootstrap is applied to the body.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/01-getting-started/06-css.mdx#_snippet_9

LANGUAGE: typescript
CODE:
```
import 'bootstrap/dist/css/bootstrap.css'

export default function RootLayout({
  children,
}: {
  children: React.ReactNode
}) {
  return (
    <html lang="en">
      <body className="container">{children}</body>
    </html>
  )
}
```

----------------------------------------

TITLE: Creating User with Authentication on Server
DESCRIPTION: This example demonstrates implementing authentication and authorization within a server function. Before creating a new user, the `authenticate` function verifies the provided token, ensuring that only authorized users can perform sensitive server-side operations and protecting data integrity.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/05-api-reference/01-directives/use-server.mdx#_snippet_4

LANGUAGE: TypeScript
CODE:
```
'use server'

import { db } from '@/lib/db' // Your database client
import { authenticate } from '@/lib/auth' // Your authentication library

export async function createUser(
  data: { name: string; email: string },
  token: string
) {
  const user = authenticate(token)
  if (!user) {
    throw new Error('Unauthorized')
  }
  const newUser = await db.user.create({ data })
  return newUser
}
```

LANGUAGE: JavaScript
CODE:
```
'use server'

import { db } from '@/lib/db' // Your database client
import { authenticate } from '@/lib/auth' // Your authentication library

export async function createUser(data, token) {
  const user = authenticate(token)
  if (!user) {
    throw new Error('Unauthorized')
  }
  const newUser = await db.user.create({ data })
  return newUser
}
```

----------------------------------------

TITLE: Navigating with Link Component in Next.js (JavaScript)
DESCRIPTION: This snippet demonstrates how to use the `Link` component from `next/link` to navigate between pages in a Next.js application. It maps over a list of posts and creates a link for each, enabling client-side navigation and prefetching.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/01-getting-started/03-layouts-and-pages.mdx#_snippet_7

LANGUAGE: JavaScript
CODE:
```
import Link from 'next/link'

export default async function Post({ post }) {
  const posts = await getPosts()

  return (
    <ul>
      {posts.map((post) => (
        <li key={post.slug}>
          <Link href={`/blog/${post.slug}`}>{post.title}</Link>
        </li>
      ))}
    </ul>
  )
}
```

----------------------------------------

TITLE: Generating Open Graph Image with ImageResponse (app/opengraph-image.tsx)
DESCRIPTION: Shows how to use `ImageResponse` in a `opengraph-image.tsx` file to generate Open Graph images, either at build time or dynamically. It defines metadata like `alt`, `size`, and `contentType`, and then exports an `Image` function that returns a new `ImageResponse` with a JSX element and size options.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/05-api-reference/04-functions/image-response.mdx#_snippet_2

LANGUAGE: tsx
CODE:
```
import { ImageResponse } from 'next/og'

// Image metadata
export const alt = 'My site'
export const size = {
  width: 1200,
  height: 630,
}

export const contentType = 'image/png'

// Image generation
export default async function Image() {
  return new ImageResponse(
    (
      // ImageResponse JSX element
      <div
        style={{
          fontSize: 128,
          background: 'white',
          width: '100%',
          height: '100%',
          display: 'flex',
          alignItems: 'center',
          justifyContent: 'center',
        }}
      >
        My site
      </div>
    ),
    // ImageResponse options
    {
      // For convenience, we can re-use the exported opengraph-image
      // size config to also set the ImageResponse's width and height.
      ...size,
    }
  )
}
```

----------------------------------------

TITLE: Configuring Next.js Metadata with JSDoc (JavaScript)
DESCRIPTION: This snippet demonstrates how to define page metadata in Next.js using a `metadata` object, enhanced with JSDoc for type safety. It sets the page title, which Next.js automatically renders in the `<head>` of the document. This approach ensures proper type checking for metadata properties and is applicable to `layout.js` or `page.js` files.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/05-api-reference/04-functions/generate-metadata.mdx#_snippet_72

LANGUAGE: js
CODE:
```
/** @type {import("next").Metadata} */
export const metadata = {
  title: 'Next.js',
}
```

----------------------------------------

TITLE: Generating Dynamic Metadata with `generateMetadata` Function in Next.js (TypeScript)
DESCRIPTION: This snippet illustrates how to generate dynamic metadata using an asynchronous `generateMetadata` function in a `page.tsx` file. It accepts `params` and `searchParams` to read route information and `parent` to access and extend metadata from parent segments. This is ideal for metadata that depends on external data or dynamic route segments, requiring `Metadata` and `ResolvingMetadata` types from 'next'.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/05-api-reference/04-functions/generate-metadata.mdx#_snippet_2

LANGUAGE: tsx
CODE:
```
import type { Metadata, ResolvingMetadata } from 'next'

type Props = {
  params: Promise<{ id: string }>
  searchParams: Promise<{ [key: string]: string | string[] | undefined }>
}

export async function generateMetadata(
  { params, searchParams }: Props,
  parent: ResolvingMetadata
): Promise<Metadata> {
  // read route params
  const { id } = await params

  // fetch data
  const product = await fetch(`https://.../${id}`).then((res) => res.json())

  // optionally access and extend (rather than replace) parent metadata
  const previousImages = (await parent).openGraph?.images || []

  return {
    title: product.title,
    openGraph: {
      images: ['/some-specific-page-image.jpg', ...previousImages],
    },
  }
}

export default function Page({ params, searchParams }: Props) {}
```

----------------------------------------

TITLE: Implementing Authentication in Next.js Server Actions (TypeScript)
DESCRIPTION: This TypeScript snippet demonstrates how to enforce authentication within a Server Action. It checks if a user is signed in using an `auth` utility and throws an error if no user is found, ensuring that only authorized users can perform the action.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/03-building-your-application/02-data-fetching/03-server-actions-and-mutations.mdx#_snippet_18

LANGUAGE: tsx
CODE:
```
'use server'

import { auth } from './lib'

export function addItem() {
  const { user } = auth()
  if (!user) {
    throw new Error('You must be signed in to perform this action')
  }

  // ...
}
```

----------------------------------------

TITLE: Generating Dynamic OG Image for Blog Post with ImageResponse (TypeScript)
DESCRIPTION: This TypeScript snippet demonstrates how to create a dynamic Open Graph image for a blog post using Next.js's `ImageResponse`. It fetches post data using `getPost` and renders the post title within a styled `div` element, setting the image dimensions and content type. It requires `next/og` and a data fetching utility like `getPost`.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/01-getting-started/13-metadata-and-og-images.mdx#_snippet_9

LANGUAGE: TypeScript
CODE:
```
import { ImageResponse } from 'next/og'
import { getPost } from '@/app/lib/data'

// Image metadata
export const size = {
  width: 1200,
  height: 630,
}

export const contentType = 'image/png'

// Image generation
export default async function Image({ params }: { params: { slug: string } }) {
  const post = await getPost(params.slug)

  return new ImageResponse(
    (
      // ImageResponse JSX element
      <div
        style={{
          fontSize: 128,
          background: 'white',
          width: '100%',
          height: '100%',
          display: 'flex',
          alignItems: 'center',
          justifyContent: 'center',
        }}
      >
        {post.title}
      </div>
    )
  )
}
```

----------------------------------------

TITLE: Forwarding Authorization Header in Next.js
DESCRIPTION: Shows how to retrieve the 'authorization' header from the incoming request using `headers` and then forward it in a subsequent fetch request from a Server Component.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/05-api-reference/04-functions/headers.mdx#_snippet_1

LANGUAGE: jsx
CODE:
```
import { headers } from 'next/headers'

export default async function Page() {
  const authorization = (await headers()).get('authorization')
  const res = await fetch('...', {
    headers: { authorization }, // Forward the authorization header
  })
  const user = await res.json()

  return <h1>{user.name}</h1>
}
```

----------------------------------------

TITLE: Using Wrapped Third-Party Client Component in a Server Component in Next.js
DESCRIPTION: This Server Component (Page) imports and renders the Carousel component, which is now a Client Component due to the wrapper created in the previous step. This demonstrates how to successfully integrate third-party client-only libraries into a Server Component tree by explicitly marking them as client-side.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/01-getting-started/07-server-and-client-components.mdx#_snippet_10

LANGUAGE: TypeScript
CODE:
```
import Carousel from './carousel'

export default function Page() {
  return (
    <div>
      <p>View pictures</p>
      {/*  Works, since Carousel is a Client Component */}
      <Carousel />
    </div>
  )
}
```

LANGUAGE: JavaScript
CODE:
```
import Carousel from './carousel'

export default function Page() {
  return (
    <div>
      <p>View pictures</p>
      {/*  Works, since Carousel is a Client Component */}
      <Carousel />
    </div>
  )
}
```

----------------------------------------

TITLE: Creating Route Handlers for GET Requests in App Router (TypeScript, Next.js)
DESCRIPTION: This snippet demonstrates the basic structure for a Route Handler in the `app` directory using TypeScript. It exports an asynchronous `GET` function that accepts a Web `Request` object. Route Handlers replace API Routes in the `app` directory, providing a flexible way to handle HTTP requests for specific routes.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/02-guides/migrating/app-router-migration.mdx#_snippet_36

LANGUAGE: TS
CODE:
```
export async function GET(request: Request) {}
```

----------------------------------------

TITLE: Signup Form with Client-Side Error Display (TSX)
DESCRIPTION: This React component, `SignupForm`, uses the `useActionState` hook to manage form submission and display server-side validation errors. It binds the form's `action` to the `signup` Server Action and conditionally renders error messages for name, email, and password fields based on the `state` returned by the action. The submit button is disabled while the form is `pending`.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/02-guides/authentication.mdx#_snippet_6

LANGUAGE: TSX
CODE:
```
'use client'

import { signup } from '@/app/actions/auth'
import { useActionState } from 'react'

export default function SignupForm() {
  const [state, action, pending] = useActionState(signup, undefined)

  return (
    <form action={action}>
      <div>
        <label htmlFor="name">Name</label>
        <input id="name" name="name" placeholder="Name" />
      </div>
      {state?.errors?.name && <p>{state.errors.name}</p>}

      <div>
        <label htmlFor="email">Email</label>
        <input id="email" name="email" placeholder="Email" />
      </div>
      {state?.errors?.email && <p>{state.errors.email}</p>}

      <div>
        <label htmlFor="password">Password</label>
        <input id="password" name="password" type="password" />
      </div>
      {state?.errors?.password && (
        <div>
          <p>Password must:</p>
          <ul>
            {state.errors.password.map((error) => (
              <li key={error}>- {error}</li>
            ))}
          </ul>
        </div>
      )}
      <button disabled={pending} type="submit">
        Sign Up
      </button>
    </form>
  )
}
```

----------------------------------------

TITLE: Using a Public Environment Variable in Next.js Pages
DESCRIPTION: This JavaScript snippet shows how to use a `NEXT_PUBLIC_` prefixed environment variable within a Next.js page. The `process.env.NEXT_PUBLIC_ANALYTICS_ID` reference will be replaced with its actual value during the build process, making it available on the client-side.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/02-guides/environment-variables.mdx#_snippet_11

LANGUAGE: JavaScript
CODE:
```
import setupAnalyticsService from '../lib/my-analytics-service'

// 'NEXT_PUBLIC_ANALYTICS_ID' can be used here as it's prefixed by 'NEXT_PUBLIC_'.
// It will be transformed at build time to `setupAnalyticsService('abcdefghijk')`.
setupAnalyticsService(process.env.NEXT_PUBLIC_ANALYTICS_ID)

function HomePage() {
  return <h1>Hello World</h1>
}

export default HomePage
```

----------------------------------------

TITLE: Invoking Server Functions via Event Handlers (JSX)
DESCRIPTION: This client component demonstrates how to call a server function (incrementLike) from an event handler (e.g., onClick). It uses React's useState hook to manage and update the UI state (likes) based on the result returned by the server function.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/01-getting-started/10-updating-data.mdx#_snippet_9

LANGUAGE: jsx
CODE:
```
'use client'

import { incrementLike } from './actions'
import { useState } from 'react'

export default function LikeButton({ initialLikes }) {
  const [likes, setLikes] = useState(initialLikes)

  return (
    <>
      <p>Total Likes: {likes}</p>
      <button
        onClick={async () => {
          const updatedLikes = await incrementLike()
          setLikes(updatedLikes)
        }}
      >
        Like
      </button>
    </>
  )
}
```

----------------------------------------

TITLE: Running Next.js in Development Mode (Bash)
DESCRIPTION: These commands are used to install project dependencies and start the Next.js application in development mode. `npm install` or `yarn install` fetches required packages, while `npm run dev` or `yarn dev` compiles the application and launches it, typically on `http://localhost:3000`.
SOURCE: https://github.com/vercel/next.js/blob/canary/examples/cms-takeshape/README.md#_snippet_5

LANGUAGE: bash
CODE:
```
npm install
npm run dev

# or

yarn install
yarn dev
```

----------------------------------------

TITLE: Accessing Request Object in Next.js Route Handlers
DESCRIPTION: This snippet demonstrates how to access and utilize the `request` object within a Next.js route handler. The `request` object is an instance of `NextRequest`, providing extended functionality like `nextUrl` for parsing the URL. It allows developers to retrieve detailed information about the incoming HTTP request.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/05-api-reference/03-file-conventions/route.mdx#_snippet_2

LANGUAGE: typescript
CODE:
```
import type { NextRequest } from 'next/server'

export async function GET(request: NextRequest) {
  const url = request.nextUrl
}
```

LANGUAGE: javascript
CODE:
```
export async function GET(request) {
  const url = request.nextUrl
}
```

----------------------------------------

TITLE: Fetching Data in Server Component (JSX)
DESCRIPTION: Demonstrates how to use the extended `fetch` function within an `async` Server Component written in JSX to fetch data from an external API and render it.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/05-api-reference/04-functions/fetch.mdx#_snippet_1

LANGUAGE: jsx
CODE:
```
export default async function Page() {
  let data = await fetch('https://api.vercel.app/blog')
  let posts = await data.json()
  return (
    <ul>
      {posts.map((post) => (
        <li key={post.id}>{post.title}</li>
      ))}
    </ul>
  )
}
```

----------------------------------------

TITLE: Defining `generateMetadata` with Parent Metadata (TSX)
DESCRIPTION: Demonstrates how to access `parent` metadata within the `generateMetadata` function using `ResolvingMetadata`. This allows for inheriting or extending metadata from parent layouts, providing a hierarchical metadata structure.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/05-api-reference/04-functions/generate-metadata.mdx#_snippet_71

LANGUAGE: tsx
CODE:
```
import type { Metadata, ResolvingMetadata } from 'next'

export async function generateMetadata(
  { params, searchParams }: Props,
  parent: ResolvingMetadata
): Promise<Metadata> {
  return {
    title: 'Next.js',
  }
}
```

----------------------------------------

TITLE: Fetching Users on Server for Client Components
DESCRIPTION: This example illustrates defining a server function, `fetchUsers`, in a dedicated file marked with 'use server'. This pattern allows server functions to be imported and safely called from client components, enabling data fetching operations to occur securely on the server.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/05-api-reference/01-directives/use-server.mdx#_snippet_1

LANGUAGE: TypeScript
CODE:
```
'use server'
import { db } from '@/lib/db' // Your database client

export async function fetchUsers() {
  const users = await db.user.findMany()
  return users
}
```

LANGUAGE: JavaScript
CODE:
```
'use server'
import { db } from '@/lib/db' // Your database client

export async function fetchUsers() {
  const users = await db.user.findMany()
  return users
}
```

----------------------------------------

TITLE: Implementing Optimistic Updates with useOptimistic in JavaScript
DESCRIPTION: This example shows how to use the `useOptimistic` hook to immediately update the UI with a new message before the server action (`send`) completes. This provides a more responsive user experience by assuming the action will succeed and updating the UI instantly.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/02-guides/forms.mdx#_snippet_16

LANGUAGE: JavaScript
CODE:
```
'use client'

import { useOptimistic } from 'react'
import { send } from './actions'

export function Thread({ messages }) {
  const [optimisticMessages, addOptimisticMessage] = useOptimistic(
    messages,
    (state, newMessage) => [...state, { message: newMessage }]
  )

  const formAction = async (formData) => {
    const message = formData.get('message')
    addOptimisticMessage(message)
    await send(message)
  }

  return (
    <div>
      {optimisticMessages.map((m) => (
        <div>{m.message}</div>
      ))}
      <form action={formAction}>
        <input type="text" name="message" />
        <button type="submit">Send</button>
      </form>
    </div>
  )
}
```

----------------------------------------

TITLE: Configuring Basic Permanent Redirects in Next.js
DESCRIPTION: This snippet demonstrates how to set up a basic permanent redirect in Next.js. It redirects requests from `/about` to the root path `/`, using a 308 status code which instructs clients and search engines to cache the redirect indefinitely. The `redirects` function in `next.config.js` returns an array of redirect objects, each defining a `source`, `destination`, and `permanent` property.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/05-api-reference/05-config/01-next-config-js/redirects.mdx#_snippet_0

LANGUAGE: JavaScript
CODE:
```
module.exports = {
  async redirects() {
    return [
      {
        source: '/about',
        destination: '/',
        permanent: true,
      },
    ]
  },
}
```

----------------------------------------

TITLE: Implementing Streaming with Vercel AI SDK in Next.js
DESCRIPTION: This code demonstrates how to create a streaming text response from a Large Language Model (LLM) using the Vercel AI SDK in a Next.js Route Handler. It takes messages from the request body, streams a response from an OpenAI model, and returns it as a `StreamingTextResponse`. Dependencies include `@ai-sdk/openai` and `ai`.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/03-building-your-application/01-routing/13-route-handlers.mdx#_snippet_14

LANGUAGE: TypeScript
CODE:
```
import { openai } from '@ai-sdk/openai'
import { StreamingTextResponse, streamText } from 'ai'

export async function POST(req: Request) {
  const { messages } = await req.json()
  const result = await streamText({
    model: openai('gpt-4-turbo'),
    messages,
  })

  return new StreamingTextResponse(result.toAIStream())
}
```

LANGUAGE: JavaScript
CODE:
```
import { openai } from '@ai-sdk/openai'
import { StreamingTextResponse, streamText } from 'ai'

export async function POST(req) {
  const { messages } = await req.json()
  const result = await streamText({
    model: openai('gpt-4-turbo'),
    messages,
  })

  return new StreamingTextResponse(result.toAIStream())
}
```

----------------------------------------

TITLE: Copying Environment Variable Example File (Bash)
DESCRIPTION: This command copies the `.env.local.example` file to `.env.local`, which is used for local environment variables and is ignored by Git. This is a prerequisite for setting up API tokens and secrets.
SOURCE: https://github.com/vercel/next.js/blob/canary/examples/cms-datocms/README.md#_snippet_3

LANGUAGE: bash
CODE:
```
cp .env.local.example .env.local
```

----------------------------------------

TITLE: Implementing Granular Streaming with Suspense in Next.js (JavaScript)
DESCRIPTION: This Next.js page component demonstrates granular content streaming using React's `<Suspense>` boundary. Content outside the `<Suspense>` (e.g., header) is immediately sent to the client, while the `BlogList` component inside the boundary is streamed in, displaying a `BlogListSkeleton` as a fallback. This allows parts of the page to become interactive sooner, improving user experience for data-intensive sections and requires the `dynamicIO` config option to be enabled.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/01-getting-started/08-fetching-data.mdx#_snippet_9

LANGUAGE: jsx
CODE:
```
import { Suspense } from 'react'
import BlogList from '@/components/BlogList'
import BlogListSkeleton from '@/components/BlogListSkeleton'

export default function BlogPage() {
  return (
    <div>
      {/* This content will be sent to the client immediately */}
      <header>
        <h1>Welcome to the Blog</h1>
        <p>Read the latest posts below.</p>
      </header>
      <main>
        {/* Any content wrapped in a <Suspense> boundary will be streamed */}
        <Suspense fallback={<BlogListSkeleton />}>
          <BlogList />
        </Suspense>
      </main>
    </div>
  )
}
```

----------------------------------------

TITLE: Revalidating Cache Tag in Server Action - TypeScript
DESCRIPTION: This TypeScript Server Action demonstrates how to use `revalidateTag` to invalidate the 'posts' cache tag. After an asynchronous operation like `addPost()`, calling `revalidateTag('posts')` ensures that any cached data associated with the 'posts' tag will be revalidated on the next visit to a relevant path.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/05-api-reference/04-functions/revalidateTag.mdx#_snippet_2

LANGUAGE: ts
CODE:
```
'use server'

import { revalidateTag } from 'next/cache'

export default async function submit() {
  await addPost()
  revalidateTag('posts')
}
```

----------------------------------------

TITLE: Accessing Runtime Environment Variables in App Router (TypeScript)
DESCRIPTION: This TypeScript snippet demonstrates how to access runtime environment variables within the Next.js App Router. By using dynamic APIs like `connection()` (or `cookies`, `headers`), the component opts into dynamic rendering, ensuring that `process.env.MY_VALUE` is evaluated at runtime on the server.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/02-guides/environment-variables.mdx#_snippet_13

LANGUAGE: TypeScript
CODE:
```
import { connection } from 'next/server'

export default async function Component() {
  await connection()
  // cookies, headers, and other Dynamic APIs
  // will also opt into dynamic rendering, meaning
  // this env variable is evaluated at runtime
  const value = process.env.MY_VALUE
  // ...
}
```

----------------------------------------

TITLE: Caching a GET Route Handler in TypeScript
DESCRIPTION: This example shows how to enable caching for a GET Route Handler by setting `export const dynamic = 'force-static'`. The handler fetches data from an external API and returns it as JSON, demonstrating how to make a static data fetch within a cached route.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/03-building-your-application/01-routing/13-route-handlers.mdx#_snippet_2

LANGUAGE: typescript
CODE:
```
export const dynamic = 'force-static'

export async function GET() {
  const res = await fetch('https://data.mongodb-api.com/...', {
    headers: {
      'Content-Type': 'application/json',
      'API-Key': process.env.DATA_API_KEY,
    },
  })
  const data = await res.json()

  return Response.json({ data })
}
```

----------------------------------------

TITLE: Enabling Partial Prerendering (PPR) in Next.js (TSX)
DESCRIPTION: This snippet demonstrates how to enable Partial Prerendering (PPR) for a Next.js layout or page by exporting the `experimental_ppr` constant. Setting it to `true` activates PPR, optimizing rendering performance. This option applies to both `layout.tsx` and `page.tsx` files.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/05-api-reference/03-file-conventions/route-segment-config.mdx#_snippet_0

LANGUAGE: tsx
CODE:
```
export const experimental_ppr = true
// true | false
```

----------------------------------------

TITLE: Invoking Server Functions from Client Components
DESCRIPTION: This example demonstrates how a Client Component (e.g., `app/ui/button.tsx` or `.js`) can invoke a Server Function. The `createPost` Server Function, defined in a separate file with `'use server'`, is imported and then passed to the `formAction` prop of a `<button>`, allowing client-side interaction to trigger a server-side operation.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/01-getting-started/10-updating-data.mdx#_snippet_3

LANGUAGE: TypeScript
CODE:
```
'use client'

import { createPost } from '@/app/actions'

export function Button() {
  return <button formAction={createPost}>Create</button>
}
```

LANGUAGE: JavaScript
CODE:
```
'use client'

import { createPost } from '@/app/actions'

export function Button() {
  return <button formAction={createPost}>Create</button>
}
```

----------------------------------------

TITLE: Handling Data Fetching Errors in Next.js Server Components (React)
DESCRIPTION: This Server Component demonstrates how to handle errors during data fetching. It performs an asynchronous `fetch` request and checks the `res.ok` property. If the response is not successful, it conditionally renders a simple error message directly within the component. This approach allows for immediate feedback to the user without requiring client-side JavaScript.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/01-getting-started/11-error-handling.mdx#_snippet_2

LANGUAGE: TSX
CODE:
```
export default async function Page() {
  const res = await fetch(`https://...`)
  const data = await res.json()

  if (!res.ok) {
    return 'There was an error.'
  }

  return '...'
}
```

LANGUAGE: JSX
CODE:
```
export default async function Page() {
  const res = await fetch(`https://...`)
  const data = await res.json()

  if (!res.ok) {
    return 'There was an error.'
  }

  return '...'
}
```

----------------------------------------

TITLE: Implementing User Data Fetching with Authentication Check in DAL
DESCRIPTION: This snippet defines the `getUser` function within the Data Access Layer (DAL), demonstrating how to perform a session verification and return `null` if the session is invalid. This ensures that authentication checks are consistently applied whenever user data is accessed, preventing unauthorized data retrieval.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/02-guides/authentication.mdx#_snippet_39

LANGUAGE: ts
CODE:
```
export const getUser = cache(async () => {
  const session = await verifySession()
  if (!session) return null

  // Get user ID from session and fetch data
})
```

LANGUAGE: js
CODE:
```
export const getUser = cache(async () => {
  const session = await verifySession()
  if (!session) return null

  // Get user ID from session and fetch data
})
```

----------------------------------------

TITLE: Memoizing Data Fetching with React Cache (TypeScript)
DESCRIPTION: This snippet demonstrates how to memoize a data fetching function (`getPost`) using React's `cache` function in TypeScript. It prevents duplicate database queries when the same data is requested multiple times, for example, by both metadata generation and the page component. It depends on `react` and a `db` utility.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/01-getting-started/13-metadata-and-og-images.mdx#_snippet_5

LANGUAGE: TypeScript
CODE:
```
import { cache } from 'react'
import { db } from '@/app/lib/db'

// getPost will be used twice, but execute only once
export const getPost = cache(async (slug: string) => {
  const res = await db.query.posts.findFirst({ where: eq(posts.slug, slug) })
  return res
})
```

----------------------------------------

TITLE: Accessing Dynamic Search Params in a Table Component (TypeScript)
DESCRIPTION: This snippet defines a table component that receives `searchParams` as a prop. The component becomes dynamic only when the `sort` value from `searchParams` is accessed, allowing the rest of the page to be prerendered while this specific component streams its content.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/01-getting-started/12-partial-prerendering.mdx#_snippet_11

LANGUAGE: TSX
CODE:
```
export async function Table({
  searchParams,
}: {
  searchParams: Promise<{ sort: string }>
}) {
  const sort = (await searchParams).sort === 'true'
  return '...'
}
```

----------------------------------------

TITLE: Accessing Session Data in Client Component (TSX)
DESCRIPTION: This client component ('use client') illustrates how to consume session data using useSession from an authentication library. It fetches the userId from the session and then uses it to make a client-side data request, typically for displaying user-specific information.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/02-guides/authentication.mdx#_snippet_43

LANGUAGE: tsx
CODE:
```
'use client';

import { useSession } from "auth-lib";

export default function Profile() {
  const { userId } = useSession();
  const { data } = useSWR(`/api/user/${userId}`, fetcher)

  return (
    // ...
  );
}
```

----------------------------------------

TITLE: Revalidating Path Cache in Server Actions (Next.js)
DESCRIPTION: This code illustrates how to revalidate the Next.js Cache for a specific path (`/posts`) within a Server Action using the `revalidatePath` API. This ensures that subsequent requests to `/posts` will fetch fresh data, invalidating any previously cached content. It's typically used after data mutations like creating a new post.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/03-building-your-application/02-data-fetching/03-server-actions-and-mutations.mdx#_snippet_12

LANGUAGE: ts
CODE:
```
'use server'

import { revalidatePath } from 'next/cache'

export async function createPost() {
  try {
    // ...
  } catch (error) {
    // ...
  }

  revalidatePath('/posts')
}
```

LANGUAGE: js
CODE:
```
'use server'

import { revalidatePath } from 'next/cache'

export async function createPost() {
  try {
    // ...
  } catch (error) {
    // ...
  }

  revalidatePath('/posts')
}
```

----------------------------------------

TITLE: Defining Environment Variables in .env
DESCRIPTION: This snippet shows how to define environment variables in a .env file. These variables are automatically loaded into process.env by Next.js, making them accessible in Node.js environments like data fetching methods and API routes.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/02-guides/environment-variables.mdx#_snippet_0

LANGUAGE: txt
CODE:
```
DB_HOST=localhost
DB_USER=myuser
DB_PASS=mypassword
```

----------------------------------------

TITLE: Creating Root Layout for Next.js App Router (TypeScript)
DESCRIPTION: This TypeScript React component defines the mandatory root layout for applications using the Next.js App Router. It wraps the entire application, requiring `<html>` and `<body>` tags, and receives `children` as a prop to render nested routes and components within the layout.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/01-getting-started/01-installation.mdx#_snippet_3

LANGUAGE: typescript
CODE:
```
export default function RootLayout({
  children,
}: {
  children: React.ReactNode
}) {
  return (
    <html lang="en">
      <body>{children}</body>
    </html>
  )
}
```

----------------------------------------

TITLE: Running the Next.js Development Server
DESCRIPTION: This command starts the Next.js development server, making the application accessible locally, typically at `localhost:3000`. It uses a custom script that also initializes Tigris collections.
SOURCE: https://github.com/vercel/next.js/blob/canary/examples/with-tigris/README.md#_snippet_3

LANGUAGE: shell
CODE:
```
npm run dev
```

----------------------------------------

TITLE: Receiving Server Action as Prop in Client Component (TypeScript)
DESCRIPTION: This Client Component receives a Server Action as a prop named 'updateItemAction'. It then uses this action directly in a <form> element's 'action' attribute, demonstrating how forms can directly invoke Server Actions for data mutations.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/03-building-your-application/02-data-fetching/03-server-actions-and-mutations.mdx#_snippet_7

LANGUAGE: TypeScript
CODE:
```
'use client'

export default function ClientComponent({
  updateItemAction,
}: {
  updateItemAction: (formData: FormData) => void
}) {
  return <form action={updateItemAction}>{/* ... */}</form>
}
```

----------------------------------------

TITLE: Passing Derived Value from Tainted Unique Value
DESCRIPTION: This TSX snippet highlights a caveat of the `experimental_taintUniqueValue` API: values derived from a tainted unique value are *not* automatically tainted. Here, `systemConfig.SERVICE_API_KEY` is tainted, but a new string `version` created by concatenating it will *not* be tainted and can be passed to the client without an error, indicating a potential loophole.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/05-api-reference/05-config/01-next-config-js/taint.mdx#_snippet_7

LANGUAGE: TSX
CODE:
```
export async function Dashboard() {
  const systemConfig = await getSystemConfig()
  // Someone makes a mistake in a PR
  const version = `version::${systemConfig.SERVICE_API_KEY}`

  return <ClientDashboard version={version} />
}
```

----------------------------------------

TITLE: Configuring Dynamic Rendering in Next.js App Router (TypeScript)
DESCRIPTION: This snippet demonstrates how to use the `export const dynamic` option in TypeScript files within the Next.js App Router. It controls the rendering and caching strategy for a layout, page, or route, allowing developers to choose between automatic, forced dynamic, error-on-dynamic-API-use, or forced static rendering. The default value is 'auto'.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/05-api-reference/03-file-conventions/route-segment-config.mdx#_snippet_2

LANGUAGE: tsx
CODE:
```
export const dynamic = 'auto'
// 'auto' | 'force-dynamic' | 'error' | 'force-static'
```

----------------------------------------

TITLE: Server-Side Form Validation Action (TypeScript)
DESCRIPTION: This Server Action, `signup`, validates incoming form data against the `SignupFormSchema` using Zod's `safeParse` method. If validation fails, it returns an object containing flattened field errors, preventing unnecessary calls to the authentication provider or database. The `FormState` type is used for type safety.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/02-guides/authentication.mdx#_snippet_4

LANGUAGE: TypeScript
CODE:
```
import { SignupFormSchema, FormState } from '@/app/lib/definitions'

export async function signup(state: FormState, formData: FormData) {
  // Validate form fields
  const validatedFields = SignupFormSchema.safeParse({
    name: formData.get('name'),
    email: formData.get('email'),
    password: formData.get('password'),
  })

  // If any form fields are invalid, return early
  if (!validatedFields.success) {
    return {
      errors: validatedFields.error.flatten().fieldErrors,
    }
  }

  // Call the provider or db to create a user...
}
```

----------------------------------------

TITLE: Configuring Fetch Cache Option (TypeScript)
DESCRIPTION: Shows how to use the `cache` option with Next.js's extended `fetch` to control interaction with the framework's Data Cache. Options include 'force-cache' and 'no-store'.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/05-api-reference/04-functions/fetch.mdx#_snippet_2

LANGUAGE: ts
CODE:
```
fetch(`https://...`, { cache: 'force-cache' | 'no-store' })
```

----------------------------------------

TITLE: Using Next.js Navigation Hooks in Client Components (App Directory)
DESCRIPTION: This snippet demonstrates how to import and use the new `useRouter`, `usePathname`, and `useSearchParams` hooks from `next/navigation` within a Next.js Client Component. These hooks are specifically designed for the `app` directory and enable programmatic navigation, access to the current path, and retrieval of URL search parameters. It requires the `'use client'` directive at the top of the file to mark it as a Client Component, as these hooks are not supported in Server Components.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/02-guides/migrating/app-router-migration.mdx#_snippet_21

LANGUAGE: tsx
CODE:
```
'use client'\n\nimport { useRouter, usePathname, useSearchParams } from 'next/navigation'\n\nexport default function ExampleClientComponent() {\n  const router = useRouter()\n  const pathname = usePathname()\n  const searchParams = useSearchParams()\n\n  // ...\n}
```

LANGUAGE: jsx
CODE:
```
'use client'\n\nimport { useRouter, usePathname, useSearchParams } from 'next/navigation'\n\nexport default function ExampleClientComponent() {\n  const router = useRouter()\n  const pathname = usePathname()\n  const searchParams = useSearchParams()\n\n  // ...\n}
```

----------------------------------------

TITLE: Fetching Data in a Server Component - TypeScript
DESCRIPTION: This example shows how to perform server-side data fetching directly within an `async` Server Component. It fetches blog posts from an API and renders them, leveraging the ability of Server Components to use `async`/`await`.
SOURCE: https://github.com/vercel/next.js/blob/canary/errors/no-async-client-component.mdx#_snippet_1

LANGUAGE: tsx
CODE:
```
export default async function Page() {
  const data = await fetch('https://api.vercel.app/blog')
  const posts = await data.json()
  return (
    <ul>
      {posts.map((post) => (
        <li key={post.id}>{post.title}</li>
      ))}
    </ul>
  )
}
```

----------------------------------------

TITLE: Implementing Post Creation and Redirection with Next.js Server Actions
DESCRIPTION: This server action (`createPost`) handles the logic for creating a new post. It's marked with `'use server'` to indicate it runs on the server. After the post is created, it uses the `redirect` function from `next/navigation` to navigate the user to the newly created post's page, ensuring a seamless user experience post-mutation.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/05-api-reference/02-components/form.mdx#_snippet_7

LANGUAGE: TypeScript
CODE:
```
'use server'
import { redirect } from 'next/navigation'

export async function createPost(formData: FormData) {
  // Create a new post
  // ...

  // Redirect to the new post
  redirect(`/posts/${data.id}`)
}
```

LANGUAGE: JavaScript
CODE:
```
'use server'
import { redirect } from 'next/navigation'

export async function createPost(formData) {
  // Create a new post
  // ...

  // Redirect to the new post
  redirect(`/posts/${data.id}`)
}
```

----------------------------------------

TITLE: Redirecting to a New URL with NextResponse in TypeScript
DESCRIPTION: This snippet shows how to perform a client-side redirect using `NextResponse.redirect()`. It takes a `URL` object as an argument, which can be constructed relative to the current request URL. This is commonly used to send users to a different page or resource.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/05-api-reference/04-functions/next-response.mdx#_snippet_6

LANGUAGE: TypeScript
CODE:
```
import { NextResponse } from 'next/server'

return NextResponse.redirect(new URL('/new', request.url))
```

----------------------------------------

TITLE: Integrating Client-Side Context Provider in Server Component Layout in Next.js
DESCRIPTION: This Server Component (RootLayout) imports and renders the ThemeProvider Client Component, wrapping its children. This pattern allows all subsequent Client Components within the application to consume the provided context, demonstrating how to bridge server-rendered layouts with client-side context.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/01-getting-started/07-server-and-client-components.mdx#_snippet_7

LANGUAGE: TypeScript
CODE:
```
import ThemeProvider from './theme-provider'

export default function RootLayout({
  children,
}: {
  children: React.ReactNode
}) {
  return (
    <html>
      <body>
        <ThemeProvider>{children}</ThemeProvider>
      </body>
    </html>
  )
}
```

LANGUAGE: JavaScript
CODE:
```
import ThemeProvider from './theme-provider'

export default function RootLayout({ children }) {
  return (
    <html>
      <body>
        <ThemeProvider>{children}</ThemeProvider>
      </body>
    </html>
  )
}
```

----------------------------------------

TITLE: Accessing URL Search Parameters (`searchParams`) in Next.js Page
DESCRIPTION: This snippet demonstrates how to retrieve URL search parameters using the `searchParams` prop in a Next.js `page` component. `searchParams` is a promise that resolves to an object mapping query keys to their values. Accessing it requires `async/await` and will opt the page into dynamic rendering.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/05-api-reference/03-file-conventions/page.mdx#_snippet_2

LANGUAGE: tsx
CODE:
```
export default async function Page({
  searchParams,
}: {
  searchParams: Promise<{ [key: string]: string | string[] | undefined }>
}) {
  const filters = (await searchParams).filters
}
```

LANGUAGE: jsx
CODE:
```
export default async function Page({ searchParams }) {
  const filters = (await searchParams).filters
}
```

----------------------------------------

TITLE: Reading and Setting Cookies in Next.js Route Handlers using next/headers
DESCRIPTION: This example shows how to read and set HTTP cookies within a Next.js Route Handler using the `cookies` function from `next/headers`. The `GET` function retrieves a 'token' cookie and then sets it back in the response headers, demonstrating server-side cookie manipulation.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/03-building-your-application/01-routing/13-route-handlers.mdx#_snippet_7

LANGUAGE: TypeScript
CODE:
```
import { cookies } from 'next/headers'

export async function GET(request: Request) {
  const cookieStore = await cookies()
  const token = cookieStore.get('token')

  return new Response('Hello, Next.js!', {
    status: 200,
    headers: { 'Set-Cookie': `token=${token.value}` },
  })
}
```

LANGUAGE: JavaScript
CODE:
```
import { cookies } from 'next/headers'

export async function GET(request) {
  const cookieStore = await cookies()
  const token = cookieStore.get('token')

  return new Response('Hello, Next.js!', {
    status: 200,
    headers: { 'Set-Cookie': `token=${token}` },
  })
}
```

----------------------------------------

TITLE: Loading Third-Party Scripts with next/script in Next.js (JSX)
DESCRIPTION: This snippet demonstrates how to correctly load a third-party script using the `next/script` component within a Next.js page. It ensures optimal performance and compatibility with React Suspense and server-side rendering, replacing the problematic direct use of `<script>` tags in `next/head`.
SOURCE: https://github.com/vercel/next.js/blob/canary/errors/no-script-tags-in-head-component.mdx#_snippet_0

LANGUAGE: jsx
CODE:
```
import Script from 'next/script'

export default function Dashboard() {
  return (
    <>
      <Script src="https://example.com/script.js" />
    </>
  )
}
```

----------------------------------------

TITLE: Creating Post with Temporary Redirect in Next.js
DESCRIPTION: This snippet demonstrates how to use the `redirect` function in a Next.js Server Action to navigate the user to a new post page after a successful database operation. It also uses `revalidatePath` to update cached data, ensuring the client sees the latest content. The `redirect` function issues a temporary (307/303) redirect.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/02-guides/redirecting.mdx#_snippet_0

LANGUAGE: typescript
CODE:
```
'use server'\n\nimport { redirect } from 'next/navigation'\nimport { revalidatePath } from 'next/cache'\n\nexport async function createPost(id: string) {\n  try {\n    // Call database\n  } catch (error) {\n    // Handle errors\n  }\n\n  revalidatePath('/posts') // Update cached posts\n  redirect(`/post/${id}`) // Navigate to the new post page\n}
```

LANGUAGE: javascript
CODE:
```
'use server'\n\nimport { redirect } from 'next/navigation'\nimport { revalidatePath } from 'next/cache'\n\nexport async function createPost(id) {\n  try {\n    // Call database\n  } catch (error) {\n    // Handle errors\n  }\n\n  revalidatePath('/posts') // Update cached posts\n  redirect(`/post/${id}`) // Navigate to the new post page\n}
```

----------------------------------------

TITLE: Updating Post with Inline `use server`
DESCRIPTION: This snippet shows how to use the 'use server' directive inline within a function, marking only that specific function as a server function. The `updatePost` function handles form data, saves the post on the server, and revalidates the path, ensuring data consistency.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/05-api-reference/01-directives/use-server.mdx#_snippet_3

LANGUAGE: TypeScript
CODE:
```
import { EditPost } from './edit-post'
import { revalidatePath } from 'next/cache'

export default async function PostPage({ params }: { params: { id: string } }) {
  const post = await getPost(params.id)

  async function updatePost(formData: FormData) {
    'use server'
    await savePost(params.id, formData)
    revalidatePath(`/posts/${params.id}`)
  }

  return <EditPost updatePostAction={updatePost} post={post} />
}
```

LANGUAGE: JavaScript
CODE:
```
import { EditPost } from './edit-post'
import { revalidatePath } from 'next/cache'

export default async function PostPage({ params }) {
  const post = await getPost(params.id)

  async function updatePost(formData) {
    'use server'
    await savePost(params.id, formData)
    revalidatePath(`/posts/${params.id}`)
  }

  return <EditPost updatePostAction={updatePost} post={post} />
}
```

----------------------------------------

TITLE: Adding Nonce and CSP Header with Next.js Middleware
DESCRIPTION: Explains how to generate a unique nonce and construct a Content Security Policy (CSP) header within a Next.js middleware function. It sets the nonce and CSP header on both the incoming request headers (for potential downstream use) and the outgoing response headers. Requires `next/server`.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/02-guides/content-security-policy.mdx#_snippet_0

LANGUAGE: typescript
CODE:
```
import { NextRequest, NextResponse } from 'next/server'

export function middleware(request: NextRequest) {
  const nonce = Buffer.from(crypto.randomUUID()).toString('base64')
  const cspHeader = `
    default-src 'self';
    script-src 'self' 'nonce-${nonce}' 'strict-dynamic';
    style-src 'self' 'nonce-${nonce}';
    img-src 'self' blob: data:;
    font-src 'self';
    object-src 'none';
    base-uri 'self';
    form-action 'self';
    frame-ancestors 'none';
    upgrade-insecure-requests;
`
  // Replace newline characters and spaces
  const contentSecurityPolicyHeaderValue = cspHeader
    .replace(/\s{2,}/g, ' ')
    .trim()

  const requestHeaders = new Headers(request.headers)
  requestHeaders.set('x-nonce', nonce)

  requestHeaders.set(
    'Content-Security-Policy',
    contentSecurityPolicyHeaderValue
  )

  const response = NextResponse.next({
    request: {
      headers: requestHeaders,
    },
  })
  response.headers.set(
    'Content-Security-Policy',
    contentSecurityPolicyHeaderValue
  )

  return response
}
```

LANGUAGE: javascript
CODE:
```
import { NextResponse } from 'next/server'

export function middleware(request) {
  const nonce = Buffer.from(crypto.randomUUID()).toString('base64')
  const cspHeader = `
    default-src 'self';
    script-src 'self' 'nonce-${nonce}' 'strict-dynamic';
    style-src 'self' 'nonce-${nonce}';
    img-src 'self' blob: data:;
    font-src 'self';
    object-src 'none';
    base-uri 'self';
    form-action 'self';
    frame-ancestors 'none';
    upgrade-insecure-requests;
`
  // Replace newline characters and spaces
  const contentSecurityPolicyHeaderValue = cspHeader
    .replace(/\s{2,}/g, ' ')
    .trim()

  const requestHeaders = new Headers(request.headers)
  requestHeaders.set('x-nonce', nonce)
  requestHeaders.set(
    'Content-Security-Policy',
    contentSecurityPolicyHeaderValue
  )

  const response = NextResponse.next({
    request: {
      headers: requestHeaders,
    },
  })
  response.headers.set(
    'Content-Security-Policy',
    contentSecurityPolicyHeaderValue
  )

  return response
}
```

----------------------------------------

TITLE: Controlling Dynamic Params Behavior in Next.js App Directory
DESCRIPTION: This code shows how `dynamicParams` in the Next.js `app` directory replaces the `fallback` option from `getStaticPaths`. Setting `export const dynamicParams = true;` (the default) allows dynamic segments not included in `generateStaticParams` to be generated on demand, effectively mimicking `fallback: true` or `blocking` behavior with streaming.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/02-guides/migrating/app-router-migration.mdx#_snippet_33

LANGUAGE: JSX
CODE:
```
// `app` directory

export const dynamicParams = true;

export async function generateStaticParams() {
  return [...]
}

async function getPost(params) {
  ...
}

export default async function Post({ params }) {
  const post = await getPost(params);

  return ...
}
```

----------------------------------------

TITLE: Handling Core Web Vitals in App Router (Next.js)
DESCRIPTION: Shows how to use the `useReportWebVitals` hook within a client component in the App Router to capture and handle core Web Vitals metrics such as FCP and LCP by switching on the `metric.name` property. Includes both TSX and JS examples.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/05-api-reference/04-functions/use-report-web-vitals.mdx#_snippet_1

LANGUAGE: tsx
CODE:
```
'use client'

import { useReportWebVitals } from 'next/web-vitals'

export function WebVitals() {
  useReportWebVitals((metric) => {
    switch (metric.name) {
      case 'FCP': {
        // handle FCP results
      }
      case 'LCP': {
        // handle LCP results
      }
      // ...
    }
  })
}
```

LANGUAGE: jsx
CODE:
```
'use client'

import { useReportWebVitals } from 'next/web-vitals'

export function WebVitals() {
  useReportWebVitals((metric) => {
    switch (metric.name) {
      case 'FCP': {
        // handle FCP results
      }
      case 'LCP': {
        // handle LCP results
      }
      // ...
    }
  })
}
```

----------------------------------------

TITLE: Setting CORS Headers for GET Requests in Next.js Route Handler (JavaScript)
DESCRIPTION: This snippet demonstrates how to configure Cross-Origin Resource Sharing (CORS) headers for a GET request in a Next.js Route Handler. It uses the standard Web API `Response` object to set `Access-Control-Allow-Origin`, `Access-Control-Allow-Methods`, and `Access-Control-Allow-Headers` to enable cross-origin requests. This allows clients from any origin to make GET, POST, PUT, DELETE, and OPTIONS requests with specified headers.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/03-building-your-application/01-routing/13-route-handlers.mdx#_snippet_19

LANGUAGE: js
CODE:
```
export async function GET(request) {
  return new Response('Hello, Next.js!', {
    status: 200,
    headers: {
      'Access-Control-Allow-Origin': '*',
      'Access-Control-Allow-Methods': 'GET, POST, PUT, DELETE, OPTIONS',
      'Access-Control-Allow-Headers': 'Content-Type, Authorization',
    },
  })
}
```

----------------------------------------

TITLE: Importing Global Styles in Root Layout (App Router - TypeScript)
DESCRIPTION: Import the global CSS file (`./globals.css`) into your root layout file (`app/layout.tsx`) to apply styles across all routes in your Next.js application when using the App Router. This ensures Tailwind's styles are available throughout your project.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/02-guides/tailwind-css.mdx#_snippet_3

LANGUAGE: typescript
CODE:
```
import type { Metadata } from 'next'

// These styles apply to every route in the application
import './globals.css'

export const metadata: Metadata = {
  title: 'Create Next App',
  description: 'Generated by create next app',
}

export default function RootLayout({
  children,
}: {
  children: React.ReactNode
}) {
  return (
    <html lang="en">
      <body>{children}</body>
    </html>
  )
}
```

----------------------------------------

TITLE: Defining a GET Route Handler in TypeScript
DESCRIPTION: This snippet demonstrates the basic convention for defining a GET request handler in a `route.ts` file within the Next.js `app` directory. It uses the Web Request API to handle incoming requests.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/03-building-your-application/01-routing/13-route-handlers.mdx#_snippet_0

LANGUAGE: typescript
CODE:
```
export async function GET(request: Request) {}
```

----------------------------------------

TITLE: Implementing Data Transfer Object (DTO) in TypeScript
DESCRIPTION: This snippet demonstrates how to implement a Data Transfer Object (DTO) in TypeScript to control which user data fields are exposed to the client. It includes authorization logic (canSeeUsername, canSeePhoneNumber) to conditionally return sensitive information based on the viewer's permissions, ensuring only necessary and authorized data is transferred.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/02-guides/authentication.mdx#_snippet_34

LANGUAGE: tsx
CODE:
```
import 'server-only'
import { getUser } from '@/app/lib/dal'

function canSeeUsername(viewer: User) {
  return true
}

function canSeePhoneNumber(viewer: User, team: string) {
  return viewer.isAdmin || team === viewer.team
}

export async function getProfileDTO(slug: string) {
  const data = await db.query.users.findMany({
    where: eq(users.slug, slug),
    // Return specific columns here
  })
  const user = data[0]

  const currentUser = await getUser(user.id)

  // Or return only what's specific to the query here
  return {
    username: canSeeUsername(currentUser) ? user.username : null,
    phonenumber: canSeePhoneNumber(currentUser, user.team)
      ? user.phonenumber
      : null,
  }
}
```

----------------------------------------

TITLE: Generating Open Graph Image with External Data (JavaScript)
DESCRIPTION: This snippet demonstrates how to create an Open Graph image dynamically in Next.js by fetching post data from an external API using `fetch` and displaying the post title. It utilizes the `params` object to extract the `slug` for the API call, and sets metadata like `alt`, `size`, and `contentType` for the image.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/05-api-reference/03-file-conventions/01-metadata/opengraph-image.mdx#_snippet_21

LANGUAGE: JavaScript
CODE:
```
import { ImageResponse } from 'next/og'

export const alt = 'About Acme'
export const size = {
  width: 1200,
  height: 630,
}
export const contentType = 'image/png'

export default async function Image({ params }) {
  const post = await fetch(`https://.../posts/${params.slug}`).then((res) =>
    res.json()
  )

  return new ImageResponse(
    (
      <div
        style={{
          fontSize: 48,
          background: 'white',
          width: '100%',
          height: '100%',
          display: 'flex',
          alignItems: 'center',
          justifyContent: 'center',
        }}
      >
        {post.title}
      </div>
    ),
    {
      ...size,
    }
  )
}
```

----------------------------------------

TITLE: Installing Dependencies and Running Next.js Development Server
DESCRIPTION: These commands are executed within the Next.js application directory. `npm install` fetches all necessary project dependencies, and `npm run dev` starts the Next.js development server, making the application accessible locally for development and testing purposes.
SOURCE: https://github.com/vercel/next.js/blob/canary/examples/cms-webiny/README.md#_snippet_3

LANGUAGE: bash
CODE:
```
npm install
npm run dev
```

----------------------------------------

TITLE: Running Next.js in Development Mode
DESCRIPTION: These commands start the Next.js development server. They allow you to run and test the application locally, typically accessible at `http://localhost:3000`. Choose the command corresponding to your preferred package manager (npm, Yarn, or pnpm).
SOURCE: https://github.com/vercel/next.js/blob/canary/examples/with-magic/README.md#_snippet_4

LANGUAGE: bash
CODE:
```
npm run dev
```

LANGUAGE: bash
CODE:
```
yarn dev
```

LANGUAGE: bash
CODE:
```
pnpm dev
```

----------------------------------------

TITLE: Starting Next.js Development Server
DESCRIPTION: This command starts the Next.js development server, enabling hot-reloading and other development features. It is typically run in the root directory of the Next.js application.
SOURCE: https://github.com/vercel/next.js/blob/canary/examples/with-temporal/README.md#_snippet_5

LANGUAGE: Bash
CODE:
```
npm run dev
```

----------------------------------------

TITLE: On-demand Revalidation with `revalidatePath` in Next.js Server Actions (TypeScript)
DESCRIPTION: This Server Action demonstrates on-demand cache invalidation using `revalidatePath` in Next.js. When `createPost` is called (e.g., after adding a new post), `revalidatePath('/posts')` clears the cache for the `/posts` route, prompting the Server Component to fetch fresh data on the next request.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/02-guides/incremental-static-regeneration.mdx#_snippet_4

LANGUAGE: ts
CODE:
```
'use server'

import { revalidatePath } from 'next/cache'

export async function createPost() {
  // Invalidate the /posts route in the cache
  revalidatePath('/posts')
}
```

----------------------------------------

TITLE: Inlining Server Functions in Server Components
DESCRIPTION: This example shows how to define a Server Function directly within a Next.js Server Component (e.g., `app/page.tsx` or `.js`). The `'use server'` directive is placed inside the `createPost` asynchronous function, making it an inlined Server Action that can be invoked from the client.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/01-getting-started/10-updating-data.mdx#_snippet_1

LANGUAGE: TypeScript
CODE:
```
export default function Page() {
  // Server Action
  async function createPost(formData: FormData) {
    'use server'
    // ...
  }

  return <></>
}
```

LANGUAGE: JavaScript
CODE:
```
export default function Page() {
  // Server Action
  async function createPost(formData) {
    'use server'
    // ...
  }

  return <></>
}
```

----------------------------------------

TITLE: Tagging Fetched Data for On-demand Revalidation in Next.js App Router
DESCRIPTION: Demonstrates how to tag data fetched using the native `fetch` API in a Next.js App Router component. The `next: { tags: ['posts'] }` option associates a cache tag with the fetched data, enabling targeted revalidation later using `revalidateTag`.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/02-guides/incremental-static-regeneration.mdx#_snippet_6

LANGUAGE: tsx
CODE:
```
export default async function Page() {
  const data = await fetch('https://api.vercel.app/blog', {
    next: { tags: ['posts'] },
  })
  const posts = await data.json()
  // ...
}
```

LANGUAGE: jsx
CODE:
```
export default async function Page() {
  const data = await fetch('https://api.vercel.app/blog', {
    next: { tags: ['posts'] },
  })
  const posts = await data.json()
  // ...
}
```

----------------------------------------

TITLE: Submitting Forms with Server Actions in React (TSX)
DESCRIPTION: This component demonstrates how to use a Server Function (e.g., createPost) as the 'action' prop of an HTML <form> element in a React component. When the form is submitted, the 'createPost' function will automatically receive the FormData object, allowing server-side processing of the form inputs.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/01-getting-started/10-updating-data.mdx#_snippet_4

LANGUAGE: tsx
CODE:
```
import { createPost } from '@/app/actions'

export function Form() {
  return (
    <form action={createPost}>
      <input type="text" name="title" />
      <input type="text" name="content" />
      <button type="submit">Create</button>
    </form>
  )
}
```

----------------------------------------

TITLE: Caching Data Fetching with React Cache (JavaScript)
DESCRIPTION: This JavaScript utility snippet illustrates how to implement a reusable and cached data fetching function using React's `cache` API and the `server-only` package. The `getItem` function is wrapped with `cache` to memoize its results on the server, optimizing data retrieval. It also includes a `preload` function for eagerly triggering the cached `getItem` call.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/01-getting-started/08-fetching-data.mdx#_snippet_17

LANGUAGE: js
CODE:
```
import { cache } from 'react'
import 'server-only'
import { getItem } from '@/lib/data'

export const preload = (id) => {
  void getItem(id)
}

export const getItem = cache(async (id) => {
  // ...
})
```

----------------------------------------

TITLE: Initializing a Next.js Project Automatically with create-next-app
DESCRIPTION: This command initiates the automatic setup of a new Next.js application using the `create-next-app` CLI tool. It prompts the user for project details like name, TypeScript, ESLint, Tailwind CSS, `src/` directory usage, App Router, Turbopack, and import aliases, then creates the project folder and installs dependencies.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/01-getting-started/01-installation.mdx#_snippet_0

LANGUAGE: bash
CODE:
```
npx create-next-app@latest
```

----------------------------------------

TITLE: Loading Google Analytics for All Routes with `@next/third-parties`
DESCRIPTION: This snippet demonstrates how to integrate Google Analytics 4 (GA4) across all routes of a Next.js application by including the `GoogleAnalytics` component from `@next/third-parties/google` in the root layout. It requires passing your GA4 measurement ID (`gaId`) to the component. By default, the script fetches after hydration, optimizing performance.
SOURCE: https://github.com/vercel/next.js/blob/canary/errors/next-script-for-ga.mdx#_snippet_0

LANGUAGE: TypeScript
CODE:
```
import { GoogleAnalytics } from '@next/third-parties/google'

export default function RootLayout({
  children,
}: {
  children: React.ReactNode
}) {
  return (
    <html lang="en">
      <body>{children}</body>
      <GoogleAnalytics gaId="G-XYZ" />
    </html>
  )
}
```

LANGUAGE: JavaScript
CODE:
```
import { GoogleAnalytics } from '@next/third-parties/google'

export default function RootLayout({ children }) {
  return (
    <html lang="en">
      <body>{children}</body>
      <GoogleAnalytics gaId="G-XYZ" />
    </html>
  )
}
```

----------------------------------------

TITLE: Optimizing Bundle Size with Client Component Boundaries
DESCRIPTION: This example demonstrates how to strategically place the `'use client'` directive to minimize client-side JavaScript bundles. It shows a `Search` component marked as a Client Component for interactivity, while the parent `Layout` component remains a Server Component, importing the `Search` component without needing to be a client component itself. This approach ensures only interactive parts are bundled for the client.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/01-getting-started/07-server-and-client-components.mdx#_snippet_2

LANGUAGE: TypeScript
CODE:
```
'use client'

export default function Search() {
  // ...
}
```

LANGUAGE: JavaScript
CODE:
```
'use client'

export default function Search() {
  // ...
}
```

LANGUAGE: TypeScript
CODE:
```
// Client Component
import Search from './search'
// Server Component
import Logo from './logo'

// Layout is a Server Component by default
export default function Layout({ children }: { children: React.ReactNode }) {
  return (
    <>
      <nav>
        <Logo />
        <Search />
      </nav>
      <main>{children}</main>
    </>
  )
}
```

LANGUAGE: JavaScript
CODE:
```
// Client Component
import Search from './search'
// Server Component
import Logo from './logo'

// Layout is a Server Component by default
export default function Layout({ children }) {
  return (
    <>
      <nav>
        <Logo />
        <Search />
      </nav>
      <main>{children}</main>
    </>
  )
}
```

----------------------------------------

TITLE: Defining `generateMetadata` with Segment Props (TSX)
DESCRIPTION: Shows how to pass `params` and `searchParams` as props to the `generateMetadata` function, enabling dynamic metadata generation based on URL segments or query parameters. This allows for context-aware metadata.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/05-api-reference/04-functions/generate-metadata.mdx#_snippet_70

LANGUAGE: tsx
CODE:
```
import type { Metadata } from 'next'

type Props = {
  params: Promise<{ id: string }>
  searchParams: Promise<{ [key: string]: string | string[] | undefined }>
}

export function generateMetadata({ params, searchParams }: Props): Metadata {
  return {
    title: 'Next.js',
  }
}

export default function Page({ params, searchParams }: Props) {}
```

----------------------------------------

TITLE: Memoizing Data Fetching with React Cache (JavaScript)
DESCRIPTION: This snippet demonstrates how to memoize a data fetching function (`getPost`) using React's `cache` function in JavaScript. It prevents duplicate database queries when the same data is requested multiple times, for example, by both metadata generation and the page component. It depends on `react` and a `db` utility.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/01-getting-started/13-metadata-and-og-images.mdx#_snippet_6

LANGUAGE: JavaScript
CODE:
```
import { cache } from 'react'
import { db } from '@/app/lib/db'

// getPost will be used twice, but execute only once
export const getPost = cache(async (slug) => {
  const res = await db.query.posts.findFirst({ where: eq(posts.slug, slug) })
  return res
})
```

----------------------------------------

TITLE: Server-Side Form Validation Action (JavaScript)
DESCRIPTION: This Server Action, `signup`, validates incoming form data against the `SignupFormSchema` using Zod's `safeParse` method. If validation fails, it returns an object containing flattened field errors, preventing unnecessary calls to the authentication provider or database. This JavaScript version provides the core validation logic.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/02-guides/authentication.mdx#_snippet_5

LANGUAGE: JavaScript
CODE:
```
import { SignupFormSchema } from '@/app/lib/definitions'

export async function signup(state, formData) {
  // Validate form fields
  const validatedFields = SignupFormSchema.safeParse({
    name: formData.get('name'),
    email: formData.get('email'),
    password: formData.get('password'),
  })

  // If any form fields are invalid, return early
  if (!validatedFields.success) {
    return {
      errors: validatedFields.error.flatten().fieldErrors,
    }
  }

  // Call the provider or db to create a user...
}
```

----------------------------------------

TITLE: Handling Pending State with useActionState in JavaScript
DESCRIPTION: This snippet demonstrates how to use the `useActionState` hook to manage the pending state of a form submission. It disables the submit button while the `createUser` action is being executed, providing visual feedback to the user.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/02-guides/forms.mdx#_snippet_10

LANGUAGE: JavaScript
CODE:
```
'use client'

import { useActionState } from 'react'
import { createUser } from '@/app/actions'

export function Signup() {
  const [state, formAction, pending] = useActionState(createUser, initialState)

  return (
    <form action={formAction}>
      {/* Other form elements */}
      <button disabled={pending}>Sign up</button>
    </form>
  )
}
```

----------------------------------------

TITLE: Statically Rendering All Paths at Build Time in Next.js
DESCRIPTION: This snippet demonstrates how to use `generateStaticParams` to fetch data from an external API and generate all possible dynamic paths at build time. By mapping the fetched posts to an array of `slug` objects, Next.js ensures that every blog post page is pre-rendered, optimizing for performance and SEO.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/05-api-reference/04-functions/generate-static-params.mdx#_snippet_4

LANGUAGE: TypeScript
CODE:
```
export async function generateStaticParams() {
  const posts = await fetch('https://.../posts').then((res) => res.json())

  return posts.map((post) => ({
    slug: post.slug,
  }))
}
```

LANGUAGE: JavaScript
CODE:
```
export async function generateStaticParams() {
  const posts = await fetch('https://.../posts').then((res) => res.json())

  return posts.map((post) => ({
    slug: post.slug,
  }))
}
```

----------------------------------------

TITLE: Configuring `fetch` Request Caching in Next.js (JS)
DESCRIPTION: This snippet demonstrates how `fetch` requests are no longer cached by default in Next.js 15. It shows how to explicitly opt a specific `fetch` request into caching using `cache: 'force-cache'` and how to apply default caching for all `fetch` requests within a layout or page using `export const fetchCache = 'default-cache'`.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/02-guides/upgrading/version-15.mdx#_snippet_21

LANGUAGE: js
CODE:
```
export default async function RootLayout() {
  const a = await fetch('https://...') // Not Cached
  const b = await fetch('https://...', { cache: 'force-cache' }) // Cached

  // ...
}
```

LANGUAGE: js
CODE:
```
// Since this is the root layout, all fetch requests in the app
// that don't set their own cache option will be cached.
export const fetchCache = 'default-cache'

export default async function RootLayout() {
  const a = await fetch('https://...') // Cached
  const b = await fetch('https://...', { cache: 'no-store' }) // Not cached

  // ...
}
```

----------------------------------------

TITLE: Running Next.js Development Server
DESCRIPTION: These commands initiate the development server for the Next.js application. They allow developers to run the project locally using different package managers (npm, yarn, pnpm, or bun) to view changes in real-time and interact with the application.
SOURCE: https://github.com/vercel/next.js/blob/canary/examples/inngest/README.md#_snippet_4

LANGUAGE: bash
CODE:
```
npm run dev
# or
yarn dev
# or
pnpm dev
# or
bun dev
```

----------------------------------------

TITLE: Nesting Server Components within Client Component Slots in Next.js
DESCRIPTION: This Server Component (Page) demonstrates how to pass a Server Component (<Cart>) as the child of a Client Component (<Modal>). This allows server-rendered UI to be visually nested within client-rendered UI, with the Server Component being fully rendered on the server before the client component is hydrated.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/01-getting-started/07-server-and-client-components.mdx#_snippet_5

LANGUAGE: TypeScript
CODE:
```
import Modal from './ui/modal'
import Cart from './ui/cart'

export default function Page() {
  return (
    <Modal>
      <Cart />
    </Modal>
  )
}
```

LANGUAGE: JavaScript
CODE:
```
import Modal from './ui/modal'
import Cart from './ui/cart'

export default function Page() {
  return (
    <Modal>
      <Cart />
    </Modal>
  )
}
```

----------------------------------------

TITLE: Configuring CORS Headers in Next.js Middleware
DESCRIPTION: This snippet demonstrates how to implement Cross-Origin Resource Sharing (CORS) in Next.js Middleware. It handles both simple and preflighted (OPTIONS) requests by checking allowed origins and setting appropriate `Access-Control` headers. It requires `NextRequest` and `NextResponse` from `next/server`.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/03-building-your-application/01-routing/14-middleware.mdx#_snippet_12

LANGUAGE: TypeScript
CODE:
```
import { NextRequest, NextResponse } from 'next/server'

const allowedOrigins = ['https://acme.com', 'https://my-app.org']

const corsOptions = {
  'Access-Control-Allow-Methods': 'GET, POST, PUT, DELETE, OPTIONS',
  'Access-Control-Allow-Headers': 'Content-Type, Authorization',
}

export function middleware(request: NextRequest) {
  // Check the origin from the request
  const origin = request.headers.get('origin') ?? ''
  const isAllowedOrigin = allowedOrigins.includes(origin)

  // Handle preflighted requests
  const isPreflight = request.method === 'OPTIONS'

  if (isPreflight) {
    const preflightHeaders = {
      ...(isAllowedOrigin && { 'Access-Control-Allow-Origin': origin }),
      ...corsOptions,
    }
    return NextResponse.json({}, { headers: preflightHeaders })
  }

  // Handle simple requests
  const response = NextResponse.next()

  if (isAllowedOrigin) {
    response.headers.set('Access-Control-Allow-Origin', origin)
  }

  Object.entries(corsOptions).forEach(([key, value]) => {
    response.headers.set(key, value)
  })

  return response
}

export const config = {
  matcher: '/api/:path*',
}
```

LANGUAGE: JavaScript
CODE:
```
import { NextResponse } from 'next/server'

const allowedOrigins = ['https://acme.com', 'https://my-app.org']

const corsOptions = {
  'Access-Control-Allow-Methods': 'GET, POST, PUT, DELETE, OPTIONS',
  'Access-Control-Allow-Headers': 'Content-Type, Authorization',
}

export function middleware(request) {
  // Check the origin from the request
  const origin = request.headers.get('origin') ?? ''
  const isAllowedOrigin = allowedOrigins.includes(origin)

  // Handle preflighted requests
  const isPreflight = request.method === 'OPTIONS'

  if (isPreflight) {
    const preflightHeaders = {
      ...(isAllowedOrigin && { 'Access-Control-Allow-Origin': origin }),
      ...corsOptions,
    }
    return NextResponse.json({}, { headers: preflightHeaders })
  }

  // Handle simple requests
  const response = NextResponse.next()

  if (isAllowedOrigin) {
    response.headers.set('Access-Control-Allow-Origin', origin)
  }

  Object.entries(corsOptions).forEach(([key, value]) => {
    response.headers.set(key, value)
  })

  return response
}

export const config = {
  matcher: '/api/:path*',
}
```

----------------------------------------

TITLE: Configuring Next.js Development Scripts in package.json
DESCRIPTION: This JSON snippet defines standard scripts within the `package.json` file, crucial for managing a Next.js project. It includes `dev` for starting the development server, `build` for creating a production-ready application, `start` for running the production server, and `lint` for executing ESLint checks.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/01-getting-started/01-installation.mdx#_snippet_2

LANGUAGE: json
CODE:
```
{
  "scripts": {
    "dev": "next dev",
    "build": "next build",
    "start": "next start",
    "lint": "next lint"
  }
}
```

----------------------------------------

TITLE: Generating Dynamic Metadata with `generateMetadata` Function in Next.js (JavaScript)
DESCRIPTION: This snippet demonstrates how to generate dynamic metadata using an asynchronous `generateMetadata` function in a `page.js` file. It receives `params` and `searchParams` to extract route-specific information and `parent` to optionally retrieve and extend metadata from parent segments. This function is crucial for SEO when metadata needs to be generated based on dynamic content or external API calls.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/05-api-reference/04-functions/generate-metadata.mdx#_snippet_3

LANGUAGE: jsx
CODE:
```
export async function generateMetadata({ params, searchParams }, parent) {
  // read route params
  const { id } = await params

  // fetch data
  const product = await fetch(`https://.../${id}`).then((res) => res.json())

  // optionally access and extend (rather than replace) parent metadata
  const previousImages = (await parent).openGraph?.images || []

  return {
    title: product.title,
    openGraph: {
      images: ['/some-specific-page-image.jpg', ...previousImages],
    },
  }
}

export default function Page({ params, searchParams }) {}
```

----------------------------------------

TITLE: Invoking Server Functions via Event Handlers (TSX)
DESCRIPTION: This client component demonstrates how to call a server function (incrementLike) from an event handler (e.g., onClick). It uses React's useState hook to manage and update the UI state (likes) based on the result returned by the server function.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/01-getting-started/10-updating-data.mdx#_snippet_8

LANGUAGE: tsx
CODE:
```
'use client'

import { incrementLike } from './actions'
import { useState } from 'react'

export default function LikeButton({ initialLikes }: { initialLikes: number }) {
  const [likes, setLikes] = useState(initialLikes)

  return (
    <>
      <p>Total Likes: {likes}</p>
      <button
        onClick={async () => {
          const updatedLikes = await incrementLike()
          setLikes(updatedLikes)
        }}
      >
        Like
      </button>
    </>
  )
}
```

----------------------------------------

TITLE: Fetching Data with End-to-End Type Safety in Next.js App Router (TypeScript)
DESCRIPTION: This snippet demonstrates how to fetch data asynchronously within a Next.js App Router component (`app/page.tsx`) while maintaining end-to-end type safety. It shows a `getData` function that fetches from an API and returns the JSON response, which is then consumed by the `Page` component. The key benefit is that the return value is not serialized, allowing complex types like `Date`, `Map`, and `Set` to be passed directly.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/05-api-reference/05-config/02-typescript.mdx#_snippet_0

LANGUAGE: tsx
CODE:
```
async function getData() {
  const res = await fetch('https://api.example.com/...')
  // The return value is *not* serialized
  // You can return Date, Map, Set, etc.
  return res.json()
}

export default async function Page() {
  const name = await getData()

  return '...'
}
```

----------------------------------------

TITLE: Required Root Layout Structure in Next.js
DESCRIPTION: This snippet reiterates the essential structure of the root layout in Next.js, emphasizing the requirement to define `<html>` and `<body>` tags. This global layout serves as the foundation for the entire application's UI and is crucial for proper rendering and metadata handling.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/05-api-reference/03-file-conventions/layout.mdx#_snippet_3

LANGUAGE: typescript
CODE:
```
export default function RootLayout({
  children,
}: {
  children: React.ReactNode
}) {
  return (
    <html>
      <body>{children}</body>
    </html>
  )
}
```

LANGUAGE: javascript
CODE:
```
export default function RootLayout({ children }) {
  return (
    <html>
      <body>{children}</body>
    </html>
  )
}
```

----------------------------------------

TITLE: Integrating Blog Nav Links into a Next.js Layout (JavaScript)
DESCRIPTION: This server-side Next.js layout component imports and renders the `BlogNavLink` client component. It fetches featured posts asynchronously using `getFeaturedPosts` and maps them to `BlogNavLink` instances, displaying a list of links alongside the main content. This demonstrates how client components can be used within server components.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/05-api-reference/04-functions/use-selected-layout-segment.mdx#_snippet_5

LANGUAGE: JSX
CODE:
```
// Import the Client Component into a parent Layout (Server Component)\nimport { BlogNavLink } from './blog-nav-link'\nimport getFeaturedPosts from './get-featured-posts'\n\nexport default async function Layout({ children }) {\n  const featuredPosts = await getFeaturedPosts()\n  return (\n    <div\n>      {featuredPosts.map((post) => (\n        <div key={post.id}>\n          <BlogNavLink slug={post.slug}>{post.title}</BlogNavLink>\n        </div>\n      ))}\n      <div\n>{children}</div\n>    </div\n>  )\n}
```

----------------------------------------

TITLE: Defining a GET Route Handler in JavaScript
DESCRIPTION: This snippet demonstrates the basic convention for defining a GET request handler in a `route.js` file within the Next.js `app` directory. It uses the Web Request API to handle incoming requests.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/03-building-your-application/01-routing/13-route-handlers.mdx#_snippet_1

LANGUAGE: javascript
CODE:
```
export async function GET(request) {}
```

----------------------------------------

TITLE: Fetching Data with ORM/Database in Next.js Server Components
DESCRIPTION: This example illustrates how to fetch data directly from a database using an ORM or database client within a Next.js Server Component. The component imports a database instance (`db`) and a table (`posts`), then performs a `select` query. This method leverages the server-side rendering environment for secure and direct database access.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/01-getting-started/08-fetching-data.mdx#_snippet_1

LANGUAGE: tsx
CODE:
```
import { db, posts } from '@/lib/db'

export default async function Page() {
  const allPosts = await db.select().from(posts)
  return (
    <ul>
      {allPosts.map((post) => (
        <li key={post.id}>{post.title}</li>
      ))}
    </ul>
  )
}
```

LANGUAGE: jsx
CODE:
```
import { db, posts } from '@/lib/db'

export default async function Page() {
  const allPosts = await db.select().from(posts)
  return (
    <ul>
      {allPosts.map((post) => (
        <li key={post.id}>{post.title}</li>
      ))}
    </ul>
  )
}
```

----------------------------------------

TITLE: Generating Static Params for Multiple Dynamic Segments in Next.js
DESCRIPTION: This snippet demonstrates how to use `generateStaticParams` to pre-render pages with multiple dynamic segments (e.g., `[category]/[product]`). It returns an array of objects, where each object defines the `category` and `product` parameters for a specific static path. The `Page` component then receives these parameters to render the content.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/05-api-reference/04-functions/generate-static-params.mdx#_snippet_2

LANGUAGE: TypeScript
CODE:
```
export function generateStaticParams() {
  return [
    { category: 'a', product: '1' },
    { category: 'b', product: '2' },
    { category: 'c', product: '3' },
  ]
}

// Three versions of this page will be statically generated
// using the `params` returned by `generateStaticParams`
// - /products/a/1
// - /products/b/2
// - /products/c/3
export default async function Page({
  params,
}: {
  params: Promise<{ category: string; product: string }>
}) {
  const { category, product } = await params
  // ...
}
```

LANGUAGE: JavaScript
CODE:
```
export function generateStaticParams() {
  return [
    { category: 'a', product: '1' },
    { category: 'b', product: '2' },
    { category: 'c', product: '3' },
  ]
}

// Three versions of this page will be statically generated
// using the `params` returned by `generateStaticParams`
// - /products/a/1
// - /products/b/2
// - /products/c/3
export default async function Page({ params }) {
  const { category, product } = await params
  // ...
}
```

----------------------------------------

TITLE: Implementing Global Error Boundaries in Next.js
DESCRIPTION: This snippet shows how to create a global error boundary using `global-error.js` in the root `app` directory. This component catches errors that bubble up past all nested error boundaries and must define its own `<html>` and `<body>` tags.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/01-getting-started/11-error-handling.mdx#_snippet_6

LANGUAGE: TypeScript
CODE:
```
'use client' // Error boundaries must be Client Components

export default function GlobalError({
  error,
  reset,
}: {
  error: Error & { digest?: string }
  reset: () => void
}) {
  return (
    // global-error must include html and body tags
    <html>
      <body>
        <h2>Something went wrong!</h2>
        <button onClick={() => reset()}>Try again</button>
      </body>
    </html>
  )
}
```

LANGUAGE: JavaScript
CODE:
```
'use client' // Error boundaries must be Client Components

export default function GlobalError({ error, reset }) {
  return (
    // global-error must include html and body tags
    <html>
      <body>
        <h2>Something went wrong!</h2>
        <button onClick={() => reset()}>Try again</button>
      </body>
    </html>
  )
}
```

----------------------------------------

TITLE: Configuring Server-Side Redirects in Next.js (JavaScript)
DESCRIPTION: Shows how to define server-side redirects in `next.config.js` using the `redirects` asynchronous function. It includes examples for a basic permanent redirect and a wildcard path matching redirect, useful for URL structure changes or known redirect lists.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/02-guides/redirecting.mdx#_snippet_7

LANGUAGE: js
CODE:
```
module.exports = {
  async redirects() {
    return [
      // Basic redirect
      {
        source: '/about',
        destination: '/',
        permanent: true,
      },
      // Wildcard path matching
      {
        source: '/blog/:slug',
        destination: '/news/:slug',
        permanent: true,
      },
    ]
  }
}
```

----------------------------------------

TITLE: Starting Next.js Development Server
DESCRIPTION: This snippet provides commands to start the Next.js development server using various package managers. Running one of these commands will launch the application, typically accessible at `http://localhost:3000`, and enable hot-reloading for development.
SOURCE: https://github.com/vercel/next.js/blob/canary/packages/create-next-app/templates/app/js/README-template.md#_snippet_0

LANGUAGE: bash
CODE:
```
npm run dev
# or
yarn dev
# or
pnpm dev
# or
bun dev
```

----------------------------------------

TITLE: Defining a Server Action for User Signup in Next.js
DESCRIPTION: This snippet defines an asynchronous Next.js Server Action named `signup`. It is intended to process the `FormData` submitted from a signup form. Server Actions execute securely on the server, making them suitable for handling authentication logic like user registration.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/02-guides/authentication.mdx#_snippet_1

LANGUAGE: TypeScript
CODE:
```
export async function signup(formData: FormData) {}
```

LANGUAGE: JavaScript
CODE:
```
export async function signup(formData) {}
```

----------------------------------------

TITLE: Implementing Conditional Routing in Next.js Middleware (TypeScript)
DESCRIPTION: This TypeScript snippet shows how to implement conditional routing logic within Next.js Middleware using `NextResponse.rewrite`. It checks the incoming request's pathname and, if it starts with `/about` or `/dashboard`, rewrites the URL to a different path, effectively serving different content based on the original request.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/03-building-your-application/01-routing/14-middleware.mdx#_snippet_6

LANGUAGE: TypeScript
CODE:
```
import { NextResponse } from 'next/server'
import type { NextRequest } from 'next/server'

export function middleware(request: NextRequest) {
  if (request.nextUrl.pathname.startsWith('/about')) {
    return NextResponse.rewrite(new URL('/about-2', request.url))
  }

  if (request.nextUrl.pathname.startsWith('/dashboard')) {
    return NextResponse.rewrite(new URL('/dashboard/user', request.url))
  }
}
```

----------------------------------------

TITLE: Creating Home Page for Next.js App Router (JavaScript)
DESCRIPTION: This JavaScript React component defines the home page for the Next.js App Router, rendered when a user visits the root URL (`/`). It serves as the entry point for the application's main content, demonstrating a simple 'Hello, Next.js!' heading.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/01-getting-started/01-installation.mdx#_snippet_6

LANGUAGE: javascript
CODE:
```
export default function Page() {
  return <h1>Hello, Next.js!</h1>
}
```

----------------------------------------

TITLE: Creating Next.js Home Page (App Router, TSX)
DESCRIPTION: This Next.js component defines the home page (`/`) for an application using the App Router. It displays a 'Home' heading and includes a `Link` component to navigate to the `/about` page, demonstrating basic page routing.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/02-guides/testing/playwright.mdx#_snippet_2

LANGUAGE: tsx
CODE:
```
import Link from 'next/link'

export default function Page() {
  return (
    <div>
      <h1>Home</h1>
      <Link href="/about">About</Link>
    </div>
  )
}
```

----------------------------------------

TITLE: Upgrading Next.js with Codemod (Bash)
DESCRIPTION: This command uses the Next.js codemod tool to automatically upgrade your Next.js application to the latest version. It simplifies the migration process by applying necessary code transformations.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/02-guides/upgrading/version-15.mdx#_snippet_0

LANGUAGE: bash
CODE:
```
npx @next/codemod@canary upgrade latest
```

----------------------------------------

TITLE: Caching Fetch Requests with force-cache in Next.js
DESCRIPTION: This snippet demonstrates how to cache individual `fetch` requests in a Next.js application by setting the `cache` option to `'force-cache'`. This ensures that subsequent requests for the same URL will be served from the cache, improving performance. It's suitable for data that doesn't change frequently.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/01-getting-started/09-caching-and-revalidating.mdx#_snippet_0

LANGUAGE: tsx
CODE:
```
export default async function Page() {
  const data = await fetch('https://...', { cache: 'force-cache' })
}
```

LANGUAGE: jsx
CODE:
```
export default async function Page() {
  const data = await fetch('https://...', { cache: 'force-cache' })
}
```

----------------------------------------

TITLE: Creating User Session in Next.js (TypeScript)
DESCRIPTION: This TypeScript function `createSession` sets an HttpOnly, Secure, SameSite=lax cookie for user sessions in a Next.js application. It takes a `userId`, encrypts the session data, and sets an expiration date for the cookie, ensuring server-side only access and secure transmission.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/02-guides/authentication.mdx#_snippet_18

LANGUAGE: TypeScript
CODE:
```
import 'server-only'
import { cookies } from 'next/headers'

export async function createSession(userId: string) {
  const expiresAt = new Date(Date.now() + 7 * 24 * 60 * 60 * 1000)
  const session = await encrypt({ userId, expiresAt })
  const cookieStore = await cookies()

  cookieStore.set('session', session, {
    httpOnly: true,
    secure: true,
    expires: expiresAt,
    sameSite: 'lax',
    path: '/',
  })
}
```

----------------------------------------

TITLE: Configuring Route Segment Options for Next.js Route Handler (JavaScript)
DESCRIPTION: This snippet illustrates how to apply route segment configuration options to a Next.js Route Handler. It exports constants like `dynamic`, `dynamicParams`, `revalidate`, `fetchCache`, `runtime`, and `preferredRegion` to control caching behavior, data fetching, and deployment environment for the specific route.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/03-building-your-application/01-routing/13-route-handlers.mdx#_snippet_25

LANGUAGE: js
CODE:
```
export const dynamic = 'auto'
export const dynamicParams = true
export const revalidate = false
export const fetchCache = 'auto'
export const runtime = 'nodejs'
export const preferredRegion = 'auto'
```

----------------------------------------

TITLE: Starting Next.js Development Server (Bash)
DESCRIPTION: This snippet provides various commands to start the Next.js development server. It lists common package managers like npm, yarn, pnpm, and bun, allowing users to choose their preferred method to launch the application locally for development.
SOURCE: https://github.com/vercel/next.js/blob/canary/packages/create-next-app/templates/default-tw-empty/ts/README-template.md#_snippet_0

LANGUAGE: bash
CODE:
```
npm run dev
# or
yarn dev
# or
pnpm dev
# or
bun dev
```

----------------------------------------

TITLE: Defining Initial Next.js Root Layout (Empty Body)
DESCRIPTION: This snippet defines the basic structure of a Next.js root layout component, accepting `children` as a prop. It serves as the initial placeholder before populating with HTML content, demonstrating the required function signature for a root layout.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/02-guides/migrating/from-vite.mdx#_snippet_3

LANGUAGE: tsx
CODE:
```
export default function RootLayout({
  children,
}: {
  children: React.ReactNode
}) {
  return '...'
}
```

LANGUAGE: jsx
CODE:
```
export default function RootLayout({ children }) {
  return '...'
}
```

----------------------------------------

TITLE: Loading Google Analytics for All Routes in Next.js
DESCRIPTION: This snippet demonstrates how to integrate Google Analytics 4 across all pages of a Next.js application. It provides examples for both the App Router (TypeScript and JavaScript) by placing the `GoogleAnalytics` component in the root layout, and for the Pages Router by placing it in the custom `_app` file. The `gaId` prop is required to specify your Google Analytics Measurement ID.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/02-guides/third-party-libraries.mdx#_snippet_7

LANGUAGE: tsx
CODE:
```
import { GoogleAnalytics } from '@next/third-parties/google'

export default function RootLayout({
  children,
}: {
  children: React.ReactNode
}) {
  return (
    <html lang="en">
      <body>{children}</body>
      <GoogleAnalytics gaId="G-XYZ" />
    </html>
  )
}
```

LANGUAGE: jsx
CODE:
```
import { GoogleAnalytics } from '@next/third-parties/google'

export default function RootLayout({ children }) {
  return (
    <html lang="en">
      <body>{children}</body>
      <GoogleAnalytics gaId="G-XYZ" />
    </html>
  )
}
```

LANGUAGE: jsx
CODE:
```
import { GoogleAnalytics } from '@next/third-parties/google'

export default function MyApp({ Component, pageProps }) {
  return (
    <>
      <Component {...pageProps} />
      <GoogleAnalytics gaId="G-XYZ" />
    </>
  )
}
```

----------------------------------------

TITLE: Saving Form Field Changes with onChange and Server Actions in Next.js
DESCRIPTION: This snippet illustrates how to use a Server Action (`saveDraft`) to persist changes from a form field (`textarea`) as they are typed, triggered by the `onChange` event. It also shows a form submission using `action={publishPost}`. The description notes the importance of debouncing for actions fired in quick succession.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/03-building-your-application/02-data-fetching/03-server-actions-and-mutations.mdx#_snippet_10

LANGUAGE: TypeScript
CODE:
```
'use client'

import { publishPost, saveDraft } from './actions'

export default function EditPost() {
  return (
    <form action={publishPost}>
      <textarea
        name="content"
        onChange={async (e) => {
          await saveDraft(e.target.value)
        }}
      />
      <button type="submit">Publish</button>
    </form>
  )
}
```

----------------------------------------

TITLE: Registering New Users with Next.js Server Actions
DESCRIPTION: This snippet demonstrates how to register a new user account using Next.js Server Actions. It involves validating form data, hashing the user's password with `bcrypt`, and inserting the user's details into a database. It expects `FormState` and `FormData` as inputs and returns a message if user creation fails.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/02-guides/authentication.mdx#_snippet_9

LANGUAGE: tsx
CODE:
```
export async function signup(state: FormState, formData: FormData) {
  // 1. Validate form fields
  // ...

  // 2. Prepare data for insertion into database
  const { name, email, password } = validatedFields.data
  // e.g. Hash the user's password before storing it
  const hashedPassword = await bcrypt.hash(password, 10)

  // 3. Insert the user into the database or call an Auth Library's API
  const data = await db
    .insert(users)
    .values({
      name,
      email,
      password: hashedPassword,
    })
    .returning({ id: users.id })

  const user = data[0]

  if (!user) {
    return {
      message: 'An error occurred while creating your account.',
    }
  }

  // TODO:
  // 4. Create user session
  // 5. Redirect user
}
```

LANGUAGE: jsx
CODE:
```
export async function signup(state, formData) {
  // 1. Validate form fields
  // ...

  // 2. Prepare data for insertion into database
  const { name, email, password } = validatedFields.data
  // e.g. Hash the user's password before storing it
  const hashedPassword = await bcrypt.hash(password, 10)

  // 3. Insert the user into the database or call an Library API
  const data = await db
    .insert(users)
    .values({
      name,
      email,
      password: hashedPassword,
    })
    .returning({ id: users.id })

  const user = data[0]

  if (!user) {
    return {
      message: 'An error occurred while creating your account.',
    }
  }

  // TODO:
  // 4. Create user session
  // 5. Redirect user
}
```

----------------------------------------

TITLE: Implementing Optimistic Updates with useOptimistic in TypeScript
DESCRIPTION: This example shows how to use the `useOptimistic` hook to immediately update the UI with a new message before the server action (`send`) completes. This provides a more responsive user experience by assuming the action will succeed and updating the UI instantly.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/02-guides/forms.mdx#_snippet_15

LANGUAGE: TypeScript
CODE:
```
'use client'

import { useOptimistic } from 'react'
import { send } from './actions'

type Message = {
  message: string
}

export function Thread({ messages }: { messages: Message[] }) {
  const [optimisticMessages, addOptimisticMessage] = useOptimistic<
    Message[],
    string
  >(messages, (state, newMessage) => [...state, { message: newMessage }])

  const formAction = async (formData: FormData) => {
    const message = formData.get('message') as string
    addOptimisticMessage(message)
    await send(message)
  }

  return (
    <div>
      {optimisticMessages.map((m, i) => (
        <div key={i}>{m.message}</div>
      ))}
      <form action={formAction}>
        <input type="text" name="message" />
        <button type="submit">Send</button>
      </form>
    </div>
  )
}
```

----------------------------------------

TITLE: Adding Metadata to Root Layout (JavaScript)
DESCRIPTION: This JavaScript snippet demonstrates how to define static metadata for the root layout. The `metadata` object allows setting SEO-related properties like `title` and `description` for the entire application, which Next.js uses to manage `<head>` HTML elements.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/02-guides/migrating/app-router-migration.mdx#_snippet_7

LANGUAGE: jsx
CODE:
```
export const metadata = {
  title: 'Home',
  description: 'Welcome to Next.js',
}
```

----------------------------------------

TITLE: Creating a Submit Button with useFormStatus in TypeScript
DESCRIPTION: This component uses the `useFormStatus` hook to access the pending state of the parent form. It disables the button when the form's action is in progress, providing a clear indication to the user that the submission is ongoing.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/02-guides/forms.mdx#_snippet_11

LANGUAGE: TypeScript
CODE:
```
'use client'

import { useFormStatus } from 'react-dom'

export function SubmitButton() {
  const { pending } = useFormStatus()

  return (
    <button disabled={pending} type="submit">
      Sign Up
    </button>
  )
}
```

----------------------------------------

TITLE: Using next/image Component in Next.js
DESCRIPTION: Demonstrates the recommended way to display images in Next.js using the `<Image />` component from `next/image`. This component automatically handles image optimization, improving performance. It requires `src`, `alt`, `width`, and `height` props.
SOURCE: https://github.com/vercel/next.js/blob/canary/errors/no-img-element.mdx#_snippet_0

LANGUAGE: jsx
CODE:
```
import Image from 'next/image'

function Home() {
  return (
    <Image
      src="https://example.com/hero.jpg"
      alt="Landscape picture"
      width={800}
      height={500}
    />
  )
}

export default Home
```

----------------------------------------

TITLE: Calling revalidatePath in Next.js Route Handler/Server Action (TS/JS)
DESCRIPTION: This example shows how to use `revalidatePath` in a Next.js Route Handler or Server Action. After a data update, calling `revalidatePath('/profile')` invalidates the cache for the specified route, forcing a re-render and ensuring users see the most current content.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/01-getting-started/09-caching-and-revalidating.mdx#_snippet_8

LANGUAGE: TypeScript
CODE:
```
import { revalidatePath } from 'next/cache'

export async function updateUser(id: string) {
  // Mutate data
  revalidatePath('/profile')
}
```

LANGUAGE: JavaScript
CODE:
```
import { revalidatePath } from 'next/cache'

export async function updateUser(id) {
  // Mutate data
  revalidatePath('/profile')
}
```

----------------------------------------

TITLE: Configuring Page Metadata in Next.js Layouts
DESCRIPTION: This snippet demonstrates how to define static page metadata, such as the title, using the `metadata` object in a Next.js root layout. This approach automatically handles advanced requirements like streaming and de-duplicating `<head>` elements, preventing manual `<head>` tag additions.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/05-api-reference/03-file-conventions/layout.mdx#_snippet_14

LANGUAGE: tsx
CODE:
```
import type { Metadata } from 'next'

export const metadata: Metadata = {
  title: 'Next.js',
}

export default function Layout({ children }: { children: React.ReactNode }) {
  return '...'
}
```

LANGUAGE: jsx
CODE:
```
export const metadata = {
  title: 'Next.js',
}

export default function Layout({ children }) {
  return '...'
}
```

----------------------------------------

TITLE: Starting Next.js Development Server
DESCRIPTION: These commands initiate the development server for a Next.js application using various popular package managers. Running any of these commands will start the server, typically accessible at `http://localhost:3000`, enabling live page updates during development.
SOURCE: https://github.com/vercel/next.js/blob/canary/packages/create-next-app/templates/default-tw/js/README-template.md#_snippet_0

LANGUAGE: bash
CODE:
```
npm run dev
# or
yarn dev
# or
pnpm dev
# or
bun dev
```

----------------------------------------

TITLE: Nesting Client Components within Server Components
DESCRIPTION: Shows how to combine Server and Client Components by importing and rendering a Client Component (Counter) within a Server Component (Page). This composition allows leveraging the benefits of both rendering environments.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/05-api-reference/01-directives/use-client.mdx#_snippet_2

LANGUAGE: tsx
CODE:
```
import Header from './header'\nimport Counter from './counter' // This is a Client Component\n\nexport default function Page() {\n  return (\n    <div>\n      <Header />\n      <Counter />\n    </div>\n  )\n}
```

LANGUAGE: jsx
CODE:
```
import Header from './header'\nimport Counter from './counter' // This is a Client Component\n\nexport default function Page() {\n  return (\n    <div>\n      <Header />\n      <Counter />\n    </div>\n  )\n}
```

----------------------------------------

TITLE: Conditional Redirects with Header, Cookie, Query, and Host Matching in Next.js
DESCRIPTION: This comprehensive snippet demonstrates how to apply conditional redirects in Next.js using `has` and `missing` fields. Redirects can be triggered or prevented based on the presence or value of specific HTTP headers, cookies, query parameters, or the request host. It also shows how to capture values from matched fields using named capture groups in regex for use in the destination path.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/05-api-reference/05-config/01-next-config-js/redirects.mdx#_snippet_5

LANGUAGE: JavaScript
CODE:
```
module.exports = {
  async redirects() {
    return [
      // if the header `x-redirect-me` is present,
      // this redirect will be applied
      {
        source: '/:path((?!another-page$).*)',
        has: [
          {
            type: 'header',
            key: 'x-redirect-me',
          },
        ],
        permanent: false,
        destination: '/another-page',
      },
      // if the header `x-dont-redirect` is present,
      // this redirect will NOT be applied
      {
        source: '/:path((?!another-page$).*)',
        missing: [
          {
            type: 'header',
            key: 'x-do-not-redirect',
          },
        ],
        permanent: false,
        destination: '/another-page',
      },
      // if the source, query, and cookie are matched,
      // this redirect will be applied
      {
        source: '/specific/:path*',
        has: [
          {
            type: 'query',
            key: 'page',
            // the page value will not be available in the
            // destination since value is provided and doesn't
            // use a named capture group e.g. (?<page>home)
            value: 'home',
          },
          {
            type: 'cookie',
            key: 'authorized',
            value: 'true',
          },
        ],
        permanent: false,
        destination: '/another/:path*',
      },
      // if the header `x-authorized` is present and
      // contains a matching value, this redirect will be applied
      {
        source: '/',
        has: [
          {
            type: 'header',
            key: 'x-authorized',
            value: '(?<authorized>yes|true)',
          },
        ],
        permanent: false,
        destination: '/home?authorized=:authorized',
      },
      // if the host is `example.com`,
      // this redirect will be applied
      {
        source: '/:path((?!another-page$).*)',
        has: [
          {
            type: 'host',
            value: 'example.com',
          },
        ],
        permanent: false,
        destination: '/another-page',
      },
    ]
  },
}
```

----------------------------------------

TITLE: Creating Custom OpenTelemetry Span for Data Fetching - TypeScript
DESCRIPTION: This TypeScript function demonstrates how to create a custom OpenTelemetry span named 'fetchGithubStars' to track an asynchronous data fetching operation. It uses `trace.getTracer` to obtain a tracer and `startActiveSpan` to wrap the `getValue()` call, ensuring the span starts before the operation and ends afterwards, even if an error occurs, due to the `finally` block. This allows for detailed performance monitoring of specific application logic.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/02-guides/open-telemetry.mdx#_snippet_9

LANGUAGE: ts
CODE:
```
import { trace } from '@opentelemetry/api'

export async function fetchGithubStars() {
  return await trace
    .getTracer('nextjs-example')
    .startActiveSpan('fetchGithubStars', async (span) => {
      try {
        return await getValue()
      } finally {
        span.end()
      }
    })
}
```

----------------------------------------

TITLE: Creating User on Server with File-Level `use server`
DESCRIPTION: This snippet demonstrates how to define server-side functions by placing the 'use server' directive at the top of a file. All functions within this file, such as `createUser`, will execute exclusively on the server, interacting with a database client to persist data.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/05-api-reference/01-directives/use-server.mdx#_snippet_0

LANGUAGE: TypeScript
CODE:
```
'use server'
import { db } from '@/lib/db' // Your database client

export async function createUser(data: { name: string; email: string }) {
  const user = await db.user.create({ data })
  return user
}
```

LANGUAGE: JavaScript
CODE:
```
'use server'
import { db } from '@/lib/db' // Your database client

export async function createUser(data) {
  const user = await db.user.create({ data })
  return user
}
```

----------------------------------------

TITLE: Defining Inline Server Action in Server Component (JavaScript)
DESCRIPTION: This snippet demonstrates how to define an inline Server Action within a Next.js Server Component by placing the 'use server' directive inside an async function. This pattern is suitable for actions that are specific to a single component and do not need to be reused elsewhere.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/03-building-your-application/02-data-fetching/03-server-actions-and-mutations.mdx#_snippet_1

LANGUAGE: JavaScript
CODE:
```
export default function Page() {
  // Server Action
  async function create() {
    'use server'
    // Mutate data
  }

  return '...'
}
```

----------------------------------------

TITLE: Routing requests using middleware
DESCRIPTION: This code snippet demonstrates how to use middleware to route requests based on a dynamic condition, such as a feature flag. It checks if the pathname is `/your-path` and if the `myFeatureFlag` is enabled. If both conditions are true, it rewrites the request to the domain specified in `rewriteDomain`, preserving the original pathname and search parameters.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/02-guides/multi-zones.mdx#_snippet_3

LANGUAGE: javascript
CODE:
```
export async function middleware(request) {
  const { pathname, search } = req.nextUrl;
  if (pathname === '/your-path' && myFeatureFlag.isEnabled()) {
    return NextResponse.rewrite(`${rewriteDomain}${pathname}${search});
  }
}
```

----------------------------------------

TITLE: Calling a Next.js Server Action from a Client Component
DESCRIPTION: This example illustrates how to invoke a Server Action (create) directly from a client component (Button). The Server Action is imported like a regular function and called within an event handler, removing the need for manual API route creation.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/02-guides/single-page-applications.mdx#_snippet_10

LANGUAGE: tsx
CODE:
```
'use client'

import { create } from './actions'

export function Button() {
  return <button onClick={() => create()}>Create</button>
}
```

LANGUAGE: jsx
CODE:
```
'use client'

import { create } from './actions'

export function Button() {
  return <button onClick={() => create()}>Create</button>
}
```

----------------------------------------

TITLE: Building and Starting Next.js Production App
DESCRIPTION: These commands are used to build the Next.js application for production and then start the production server. The `build` command compiles the project into an optimized output, and `start` serves this output, providing options for `npm`, `yarn`, and `pnpm`.
SOURCE: https://github.com/vercel/next.js/blob/canary/examples/with-sass/README.md#_snippet_1

LANGUAGE: bash
CODE:
```
npm run build
npm run start
```

LANGUAGE: bash
CODE:
```
yarn build
yarn start
```

LANGUAGE: bash
CODE:
```
pnpm build
pnpm start
```

----------------------------------------

TITLE: Creating a Client Component with State (Counter)
DESCRIPTION: This snippet illustrates how to define a Client Component by adding the `'use client'` directive at the top of the file. It shows a simple counter component that uses React's `useState` hook to manage its internal state and an `onClick` event handler for interactivity, demonstrating typical client-side functionality.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/01-getting-started/07-server-and-client-components.mdx#_snippet_1

LANGUAGE: TypeScript
CODE:
```
'use client'

import { useState } from 'react'

export default function Counter() {
  const [count, setCount] = useState(0)

  return (
    <div>
      <p>{count} likes</p>
      <button onClick={() => setCount(count + 1)}>Click me</button>
    </div>
  )
}
```

LANGUAGE: JavaScript
CODE:
```
'use client'

import { useState } from 'react'

export default function Counter() {
  const [count, setCount] = useState(0)

  return (
    <div>
      <p>{count} likes</p>
      <button onClick={() => setCount(count + 1)}>Click me</button>
    </div>
  )
}
```

----------------------------------------

TITLE: Starting Next.js Development Server with npm, yarn, pnpm, or bun
DESCRIPTION: This snippet provides various commands to start the Next.js development server. It allows developers to choose their preferred package manager (npm, yarn, pnpm, or bun) to run the `dev` script, which typically launches the application on `http://localhost:3000`. This is the first step to begin local development and see the application in action.
SOURCE: https://github.com/vercel/next.js/blob/canary/packages/create-next-app/templates/default/ts/README-template.md#_snippet_0

LANGUAGE: bash
CODE:
```
npm run dev
# or
yarn dev
# or
pnpm dev
# or
bun dev
```

----------------------------------------

TITLE: Generating a Sitemap Programmatically with Next.js
DESCRIPTION: This example shows how to programmatically generate a sitemap in Next.js using `sitemap.ts` or `sitemap.js`. It exports a default function that returns an array of `MetadataRoute.Sitemap` objects, each representing a URL with properties like `url`, `lastModified`, `changeFrequency`, and `priority`.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/05-api-reference/03-file-conventions/01-metadata/sitemap.mdx#_snippet_1

LANGUAGE: TypeScript
CODE:
```
import type { MetadataRoute } from 'next'

export default function sitemap(): MetadataRoute.Sitemap {
  return [
    {
      url: 'https://acme.com',
      lastModified: new Date(),
      changeFrequency: 'yearly',
      priority: 1,
    },
    {
      url: 'https://acme.com/about',
      lastModified: new Date(),
      changeFrequency: 'monthly',
      priority: 0.8,
    },
    {
      url: 'https://acme.com/blog',
      lastModified: new Date(),
      changeFrequency: 'weekly',
      priority: 0.5,
    },
  ]
}
```

LANGUAGE: JavaScript
CODE:
```
export default function sitemap() {
  return [
    {
      url: 'https://acme.com',
      lastModified: new Date(),
      changeFrequency: 'yearly',
      priority: 1,
    },
    {
      url: 'https://acme.com/about',
      lastModified: new Date(),
      changeFrequency: 'monthly',
      priority: 0.8,
    },
    {
      url: 'https://acme.com/blog',
      lastModified: new Date(),
      changeFrequency: 'weekly',
      priority: 0.5,
    },
  ]
}
```

----------------------------------------

TITLE: Using revalidatePath in a Route Handler - Next.js - JavaScript
DESCRIPTION: This Route Handler demonstrates how to dynamically revalidate a path based on a query parameter. It retrieves the 'path' from the request URL and calls `revalidatePath` if a path is provided, returning a JSON response indicating success or a missing path.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/05-api-reference/04-functions/revalidatePath.mdx#_snippet_7

LANGUAGE: js
CODE:
```
import { revalidatePath } from 'next/cache'

export async function GET(request) {
  const path = request.nextUrl.searchParams.get('path')

  if (path) {
    revalidatePath(path)
    return Response.json({ revalidated: true, now: Date.now() })
  }

  return Response.json({
    revalidated: false,
    now: Date.now(),
    message: 'Missing path to revalidate',
  })
}
```

----------------------------------------

TITLE: Defining Server Functions for Client Component Invocation
DESCRIPTION: This snippet illustrates how to define a Server Function (`createPost`) in a dedicated file (e.g., `app/actions.ts` or `.js`) by placing the `'use server'` directive at the very top of the file. This approach allows the function to be imported and invoked from Client Components, as direct definition within Client Components is not supported.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/01-getting-started/10-updating-data.mdx#_snippet_2

LANGUAGE: TypeScript
CODE:
```
'use server'

export async function createPost() {}
```

LANGUAGE: JavaScript
CODE:
```
'use server'

export async function createPost() {}
```

----------------------------------------

TITLE: Defining Metadata with Built-in SEO in App Directory (TypeScript)
DESCRIPTION: This snippet shows how to define page metadata, such as the title, using the new built-in SEO support in the Next.js `app` directory. It exports a `metadata` object of type `Metadata` to configure the page's head elements, replacing the need for `next/head`.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/02-guides/migrating/app-router-migration.mdx#_snippet_15

LANGUAGE: TypeScript
CODE:
```
import type { Metadata } from 'next'

export const metadata: Metadata = {
  title: 'My Page Title',
}

export default function Page() {
  return '...'
}
```

----------------------------------------

TITLE: Updating Next.js and React Dependencies in package.json (Diff)
DESCRIPTION: This snippet shows the necessary changes in `package.json` to upgrade Next.js, React, and React-DOM to their respective React 19 compatible versions. It also includes `pnpm` overrides for `@types/react` and `@types/react-dom` to ensure correct type resolution for React 19.
SOURCE: https://github.com/vercel/next.js/blob/canary/packages/next-codemod/bin/__testfixtures__/next-14-installed/README.md#_snippet_0

LANGUAGE: diff
CODE:
```
diff --git a/packages/next-codemod/bin/__testfixtures__/next-14-installed/package.json b/packages/next-codemod/bin/__testfixtures__/next-14-installed/package.json
index 5ec4c37f0b..131f5b9f4a 100644
--- a/packages/next-codemod/bin/__testfixtures__/next-14-installed/package.json
+++ b/packages/next-codemod/bin/__testfixtures__/next-14-installed/package.json
@@ -4,10 +4,16 @@
     "dev": "next dev"
   },
   "dependencies": {
-    "next": "14.3.0-canary.44",
-    "react": "18.2.0",
-    "react-dom": "18.2.0",
-    "@types/react": "^18.2.0",
-    "@types/react-dom": "^18.2.0"
+    "next": "15.0.4-canary.43",
+    "react": "19.0.0",
+    "react-dom": "19.0.0",
+    "@types/react": "19.0.0",
+    "@types/react-dom": "19.0.0"
+  },
+  "pnpm": {
+    "overrides": {
+      "@types/react": "19.0.0",
+      "@types/react-dom": "19.0.0"
+    }
   }
 }
```

----------------------------------------

TITLE: Caching Function Output with use cache - Next.js
DESCRIPTION: Shows how 'use cache' can be added to any asynchronous function, not limited to components or routes, to cache its return value. This is useful for caching results of network requests, database queries, or slow computations.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/05-api-reference/01-directives/use-cache.mdx#_snippet_6

LANGUAGE: ts
CODE:
```
export async function getData() {
  'use cache'

  const data = await fetch('/api/data')
  return data
}
```

LANGUAGE: js
CODE:
```
export async function getData() {
  'use cache'

  const data = await fetch('/api/data')
  return data
}
```

----------------------------------------

TITLE: Mixing Node.js Logic and React Components in a Single Module (Breaking Example)
DESCRIPTION: This snippet illustrates an anti-pattern where a module exports both Node.js-specific code (ioredis client) and a React component. This setup prevents effective tree-shaking, causing Node.js-only dependencies to be included in the browser bundle and leading to 'Module Not Found' errors.
SOURCE: https://github.com/vercel/next.js/blob/canary/errors/module-not-found.mdx#_snippet_7

LANGUAGE: js
CODE:
```
import Redis from 'ioredis'

// Modules that hold Node.js-only code can't also export React components
// Tree shaking of getStaticProps/getStaticPaths/getServerSiderProps is ran only on page files
const redis = new Redis(process.env.REDIS_URL)

export function MyComponent() {
  return <h1>Hello</h1>
}

export default redis
```

----------------------------------------

TITLE: Navigating with Link Component in Next.js (TypeScript)
DESCRIPTION: This snippet demonstrates how to use the `Link` component from `next/link` to navigate between pages in a Next.js application. It maps over a list of posts and creates a link for each, enabling client-side navigation and prefetching.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/01-getting-started/03-layouts-and-pages.mdx#_snippet_6

LANGUAGE: TypeScript
CODE:
```
import Link from 'next/link'

export default async function Post({ post }) {
  const posts = await getPosts()

  return (
    <ul>
      {posts.map((post) => (
        <li key={post.slug}>
          <Link href={`/blog/${post.slug}`}>{post.title}</Link>
        </li>
      ))}
    </ul>
  )
}
```

----------------------------------------

TITLE: Copying Environment Variable Example File
DESCRIPTION: This command copies the `.env.local.example` file to `.env.local`. The `.env.local` file is used to store sensitive environment variables for local development and is typically ignored by Git, ensuring credentials are not committed to version control.
SOURCE: https://github.com/vercel/next.js/blob/canary/examples/auth0/README.md#_snippet_3

LANGUAGE: bash
CODE:
```
cp .env.local.example .env.local
```

----------------------------------------

TITLE: Copying Local Environment Variables File (Bash)
DESCRIPTION: This Bash command duplicates the `.env.local.example` file to `.env.local`. The `.env.local` file is crucial for storing sensitive environment variables like API keys and is automatically ignored by Git, ensuring secure configuration for the Next.js application.
SOURCE: https://github.com/vercel/next.js/blob/canary/examples/cms-kontent-ai/README.md#_snippet_2

LANGUAGE: bash
CODE:
```
cp .env.local.example .env.local
```

----------------------------------------

TITLE: Importing and Using Google Fonts in App Router (TypeScript)
DESCRIPTION: This snippet demonstrates how to import and use the `Inter` Google Font in a Next.js App Router `layout.tsx` file. It initializes the font with `latin` subsets and `swap` display strategy, then applies it globally to the `<html>` element using `inter.className`. This optimizes font loading by self-hosting and preventing layout shifts.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/05-api-reference/02-components/font.mdx#_snippet_0

LANGUAGE: tsx
CODE:
```
import { Inter } from 'next/font/google'

// If loading a variable font, you don't need to specify the font weight
const inter = Inter({
  subsets: ['latin'],
  display: 'swap',
})

export default function RootLayout({
  children,
}: {
  children: React.ReactNode
}) {
  return (
    <html lang="en" className={inter.className}>
      <body>{children}</body>
    </html>
  )
}
```

----------------------------------------

TITLE: Defining a Client Component for UI Slot (Modal) in Next.js
DESCRIPTION: This Client Component, marked with 'use client', defines a generic Modal that accepts children as a prop. This pattern creates a "slot" where server-rendered UI can be visually nested within a client-side component, allowing the client component to manage interactivity while displaying server-fetched content.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/01-getting-started/07-server-and-client-components.mdx#_snippet_4

LANGUAGE: TypeScript
CODE:
```
'use client'

export default function Modal({ children }: { children: React.ReactNode }) {
  return <div>{children}</div>
}
```

LANGUAGE: JavaScript
CODE:
```
'use client'

export default function Modal({ children }) {
  return <div>{children}</div>
}
```

----------------------------------------

TITLE: Calling Server Action from Client Component (JavaScript)
DESCRIPTION: This Client Component imports and calls a Server Action defined in a separate module. The 'use client' directive marks this component as a Client Component, enabling interactive client-side functionality while still leveraging server-side mutations.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/03-building-your-application/02-data-fetching/03-server-actions-and-mutations.mdx#_snippet_5

LANGUAGE: JavaScript
CODE:
```
'use client'

import { create } from './actions'

export function Button() {
  return <button onClick={() => create()}>Create</button>
}
```

----------------------------------------

TITLE: Managing Cookies in Next.js Server Actions (TypeScript)
DESCRIPTION: This snippet demonstrates how to interact with cookies within a Next.js Server Action using the `next/headers` `cookies` API. It illustrates operations for retrieving a cookie's value, setting a new cookie with a specified name and value, and deleting an existing cookie.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/03-building-your-application/02-data-fetching/03-server-actions-and-mutations.mdx#_snippet_15

LANGUAGE: ts
CODE:
```
'use server'

import { cookies } from 'next/headers'

export async function exampleAction() {
  const cookieStore = await cookies()

  // Get cookie
  cookieStore.get('name')?.value

  // Set cookie
  cookieStore.set('name', 'Delba')

  // Delete cookie
  cookieStore.delete('name')
}
```

----------------------------------------

TITLE: Generating Dynamic Metadata with JSX in Next.js
DESCRIPTION: This snippet illustrates how to use the asynchronous `generateMetadata` function in JSX to dynamically fetch and set metadata based on route parameters. It's useful for pages where metadata depends on external data, such as a blog post's title and description fetched from an API, ensuring SEO-friendly content for dynamic routes.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/01-getting-started/13-metadata-and-og-images.mdx#_snippet_4

LANGUAGE: JavaScript
CODE:
```
export async function generateMetadata({ params, searchParams }, parent) {
  const slug = (await params).slug

  // fetch post information
  const post = await fetch(`https://api.vercel.app/blog/${slug}`).then((res) =>
    res.json()
  )

  return {
    title: post.title,
    description: post.description,
  }
}

export default function Page({ params, searchParams }) {}
```

----------------------------------------

TITLE: Managing Cookies in Next.js Middleware (JavaScript)
DESCRIPTION: This snippet demonstrates how to read, check for existence, delete, and set cookies on both incoming `NextRequest` and outgoing `NextResponse` objects within a Next.js middleware function. It utilizes the `RequestCookies` and `ResponseCookies` APIs provided by Next.js.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/03-building-your-application/01-routing/14-middleware.mdx#_snippet_9

LANGUAGE: JavaScript
CODE:
```
import { NextResponse } from 'next/server'

export function middleware(request) {
  // Assume a "Cookie:nextjs=fast" header to be present on the incoming request
  // Getting cookies from the request using the `RequestCookies` API
  let cookie = request.cookies.get('nextjs')
  console.log(cookie) // => { name: 'nextjs', value: 'fast', Path: '/' }
  const allCookies = request.cookies.getAll()
  console.log(allCookies) // => [{ name: 'nextjs', value: 'fast' }]

  request.cookies.has('nextjs') // => true
  request.cookies.delete('nextjs')
  request.cookies.has('nextjs') // => false

  // Setting cookies on the response using the `ResponseCookies` API
  const response = NextResponse.next()
  response.cookies.set('vercel', 'fast')
  response.cookies.set({
    name: 'vercel',
    value: 'fast',
    path: '/',
  })
  cookie = response.cookies.get('vercel')
  console.log(cookie) // => { name: 'vercel', value: 'fast', Path: '/' }
  // The outgoing response will have a `Set-Cookie:vercel=fast;path=/test` header.

  return response
}
```

----------------------------------------

TITLE: Using `useSearchParams` in Next.js App Directory in JSX
DESCRIPTION: This snippet shows the simplified usage of `useSearchParams` from `next/navigation` when a component is exclusively used within the Next.js `app` directory. In this context, the `next/compat/router` is no longer needed, and `searchParams` are immediately available, streamlining the code.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/02-pages/04-api-reference/03-functions/use-router.mdx#_snippet_21

LANGUAGE: jsx
CODE:
```
import { useSearchParams } from 'next/navigation'
const MyComponent = () => {
  const searchParams = useSearchParams()
  // As this component is only used in `app/`, the compat router can be removed.
  const search = searchParams.get('search')
  // ...
}
```

----------------------------------------

TITLE: Overriding Default Fetch Cache Behavior with `fetchCache` in TSX
DESCRIPTION: This snippet shows how to use the `fetchCache` option to override the default caching behavior of all `fetch` requests within a Next.js layout or page. It provides various string values like `'auto'`, `'force-cache'`, or `'force-no-store'` to control whether fetches are cached or not, regardless of their individual `cache` options or proximity to Dynamic APIs.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/05-api-reference/03-file-conventions/route-segment-config.mdx#_snippet_8

LANGUAGE: tsx
CODE:
```
export const fetchCache = 'auto'
// 'auto' | 'default-cache' | 'only-cache'
// 'force-cache' | 'force-no-store' | 'default-no-store' | 'only-no-store'
```

----------------------------------------

TITLE: Using Localization Dictionary in Next.js Page Component (JSX)
DESCRIPTION: This JavaScript snippet demonstrates how to fetch and utilize the localized dictionary within a Next.js App Router page component. It retrieves the current locale from `params`, loads the corresponding dictionary using `getDictionary`, and then accesses translated strings for display.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/02-guides/internationalization.mdx#_snippet_9

LANGUAGE: JSX
CODE:
```
import { getDictionary } from './dictionaries'

export default async function Page({ params }) {
  const { lang } = await params
  const dict = await getDictionary(lang) // en
  return <button>{dict.products.cart}</button> // Add to Cart
}
```

----------------------------------------

TITLE: Revalidating Cache Tag in Route Handler - TypeScript
DESCRIPTION: This TypeScript Route Handler illustrates how to dynamically revalidate a cache tag based on a query parameter. It imports `revalidateTag` and `NextRequest`, extracts the `tag` from the request's search parameters, calls `revalidateTag` with it, and returns a JSON response indicating successful revalidation.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/05-api-reference/04-functions/revalidateTag.mdx#_snippet_4

LANGUAGE: ts
CODE:
```
import type { NextRequest } from 'next/server'
import { revalidateTag } from 'next/cache'

export async function GET(request: NextRequest) {
  const tag = request.nextUrl.searchParams.get('tag')
  revalidateTag(tag)
  return Response.json({ revalidated: true, now: Date.now() })
}
```

----------------------------------------

TITLE: Passing Server Action as Prop to Client Component (JSX)
DESCRIPTION: This snippet demonstrates how a Server Action can be passed as a prop to a Client Component. Next.js handles the serialization of the Server Action across the client-server boundary, allowing the Client Component to invoke it.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/03-building-your-application/02-data-fetching/03-server-actions-and-mutations.mdx#_snippet_6

LANGUAGE: JavaScript
CODE:
```
<ClientComponent updateItemAction={updateItem} />
```

----------------------------------------

TITLE: Awaiting Next.js Dynamic API in Server Component
DESCRIPTION: This snippet demonstrates the correct way to access the `params` prop asynchronously in a Next.js Server Component. The function must be marked `async`, and the dynamic API (`params`) must be `await`ed before accessing its properties.
SOURCE: https://github.com/vercel/next.js/blob/canary/errors/sync-dynamic-apis.mdx#_snippet_2

LANGUAGE: javascript
CODE:
```
async function Page({ params }) {
  // asynchronous access of `params.id`.
  const { id } = await params
  return <p>ID: {id}</p>
}
```

----------------------------------------

TITLE: Defining Multiple Google Fonts in a Utility File (Next.js)
DESCRIPTION: This code demonstrates creating a dedicated utility file (`app/fonts.ts` or `.js`) to import and export multiple Google Font instances (Inter and Roboto Mono). Each font is configured with specific subsets and a `display` property, allowing them to be imported and applied modularly across the application.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/05-api-reference/02-components/font.mdx#_snippet_26

LANGUAGE: TS
CODE:
```
import { Inter, Roboto_Mono } from 'next/font/google'

export const inter = Inter({
  subsets: ['latin'],
  display: 'swap',
})

export const roboto_mono = Roboto_Mono({
  subsets: ['latin'],
  display: 'swap',
})
```

LANGUAGE: JS
CODE:
```
import { Inter, Roboto_Mono } from 'next/font/google'

export const inter = Inter({
  subsets: ['latin'],
  display: 'swap',
})

export const roboto_mono = Roboto_Mono({
  subsets: ['latin'],
  display: 'swap',
})
```

----------------------------------------

TITLE: Reading Headers from NextRequest in Next.js Route Handlers
DESCRIPTION: This snippet shows how to read HTTP headers directly from the `request` object, typed as `NextRequest`, within a Next.js Route Handler. It provides an alternative way to access request headers using standard Web APIs.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/03-building-your-application/01-routing/13-route-handlers.mdx#_snippet_10

LANGUAGE: TypeScript
CODE:
```
import { type NextRequest } from 'next/server'

export async function GET(request: NextRequest) {
  const requestHeaders = new Headers(request.headers)
}
```

LANGUAGE: JavaScript
CODE:
```
export async function GET(request) {
  const requestHeaders = new Headers(request.headers)
}
```

----------------------------------------

TITLE: Revalidating Cache Tag in Server Action - JavaScript
DESCRIPTION: This JavaScript Server Action demonstrates how to use `revalidateTag` to invalidate the 'posts' cache tag. After an asynchronous operation like `addPost()`, calling `revalidateTag('posts')` ensures that any cached data associated with the 'posts' tag will be revalidated on the next visit to a relevant path.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/05-api-reference/04-functions/revalidateTag.mdx#_snippet_3

LANGUAGE: js
CODE:
```
'use server'

import { revalidateTag } from 'next/cache'

export default async function submit() {
  await addPost()
  revalidateTag('posts')
}
```

----------------------------------------

TITLE: Generating Open Graph Image with Local Assets (JavaScript)
DESCRIPTION: This snippet illustrates how to generate an Open Graph image in Next.js by embedding a local image asset. It uses Node.js `fs/promises` to read the image file from the file system and converts it to an `ArrayBuffer` for use in the `src` attribute of an `<img>` tag within the `ImageResponse`. The local asset should be placed relative to the project root.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/05-api-reference/03-file-conventions/01-metadata/opengraph-image.mdx#_snippet_23

LANGUAGE: JavaScript
CODE:
```
import { ImageResponse } from 'next/og'
import { join } from 'node:path'
import { readFile } from 'node:fs/promises'

export default async function Image() {
  const logoData = await readFile(join(process.cwd(), 'logo.png'))
  const logoSrc = Uint8Array.from(logoData).buffer

  return new ImageResponse(
    (
      <div
        style={{
          display: 'flex',
          alignItems: 'center',
          justifyContent: 'center',
        }}
      >
        <img src={logoSrc} height="100" />
      </div>
    )
  )
}
```

----------------------------------------

TITLE: Returning JSON Response with NextResponse in TypeScript
DESCRIPTION: This snippet demonstrates how to create and return a JSON response using `NextResponse.json()`. It allows specifying a JSON body and an optional status code, useful for API routes to send structured data back to the client. Here, it returns an error object with a 500 status.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/05-api-reference/04-functions/next-response.mdx#_snippet_4

LANGUAGE: TypeScript
CODE:
```
import { NextResponse } from 'next/server'

export async function GET(request: Request) {
  return NextResponse.json({ error: 'Internal Server Error' }, { status: 500 })
```

----------------------------------------

TITLE: Integrating Dynamic User Component with Suspense (JavaScript)
DESCRIPTION: This snippet demonstrates how to integrate a dynamic component (`User`) into a page that uses Partial Prerendering. By wrapping the `User` component with `Suspense`, the static parts of the page are prerendered, while the dynamic `User` component is streamed, improving initial load performance.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/01-getting-started/12-partial-prerendering.mdx#_snippet_8

LANGUAGE: JSX
CODE:
```
import { Suspense } from 'react'
import { User, AvatarSkeleton } from './user'

export const experimental_ppr = true

export default function Page() {
  return (
    <section>
      <h1>This will be prerendered</h1>
      <Suspense fallback={<AvatarSkeleton />}>
        <User />
      </Suspense>
    </section>
  )
}
```

----------------------------------------

TITLE: Demonstrating Correct Async Context Usage with Next.js Cookies (JSX)
DESCRIPTION: This snippet shows the correct approach to using `cookies()` from `next/headers` when dealing with asynchronous operations like `setTimeout`. By calling `cookies().getAll()` *before* the `setTimeout` and passing the retrieved `cookieData` into the promise, the dynamic function is invoked within its proper async context, preventing a `DynamicServerError`. This ensures the page can be statically generated or dynamically rendered as intended.
SOURCE: https://github.com/vercel/next.js/blob/canary/errors/dynamic-server-error.mdx#_snippet_1

LANGUAGE: JSX
CODE:
```
import { cookies } from 'next/headers'

async function getCookieData() {
  const cookieData = cookies().getAll()
  return new Promise((resolve) =>
    setTimeout(() => {
      resolve(cookieData)
    }, 1000)
  )
}

export default async function Page() {
  const cookieData = await getCookieData()
  return <div>Hello World</div>
}
```

----------------------------------------

TITLE: Fetching Data for Streaming in Next.js Server Component
DESCRIPTION: This snippet demonstrates how to fetch data in a Next.js Server Component without awaiting the promise, and then pass it as a prop to a Client Component. It uses `Suspense` to show a fallback UI while the data is being resolved, enabling server-to-client data streaming.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/01-getting-started/08-fetching-data.mdx#_snippet_2

LANGUAGE: TypeScript
CODE:
```
import Posts from '@/app/ui/posts
import { Suspense } from 'react'

export default function Page() {
  // Don't await the data fetching function
  const posts = getPosts()

  return (
    <Suspense fallback={<div>Loading...</div>}>
      <Posts posts={posts} />
    </Suspense>
  )
}
```

LANGUAGE: JavaScript
CODE:
```
import Posts from '@/app/ui/posts
import { Suspense } from 'react'

export default function Page() {
  // Don't await the data fetching function
  const posts = getPosts()

  return (
    <Suspense fallback={<div>Loading...</div>}>
      <Posts posts={posts} />
    </Suspense>
  )
}
```

----------------------------------------

TITLE: Configuring Fetch Tags Option (TypeScript)
DESCRIPTION: Demonstrates how to assign cache tags to a fetched resource using the `next.tags` option. Tags allow on-demand revalidation using `revalidateTag`.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/05-api-reference/04-functions/fetch.mdx#_snippet_4

LANGUAGE: ts
CODE:
```
fetch(`https://...`, { next: { tags: ['collection'] } })
```

----------------------------------------

TITLE: Handling Form Data with Server Functions (TypeScript)
DESCRIPTION: This server-side function, marked with ''use server'', demonstrates how to receive and extract data from a FormData object submitted via an HTML form. It retrieves 'title' and 'content' fields, which can then be used to update a database or perform other server-side operations.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/01-getting-started/10-updating-data.mdx#_snippet_6

LANGUAGE: ts
CODE:
```
'use server'

export async function createPost(formData: FormData) {
  const title = formData.get('title')
  const content = formData.get('content')

  // Update data
  // Revalidate cache
}
```

----------------------------------------

TITLE: Passing Server Actions via Props in Next.js (Server Side)
DESCRIPTION: Illustrates how to define a Server Action (`performUpdate`) in a Server Component and pass it as a prop (`performUpdate`) through a cacheable component (`CachedComponent`) to a Client Component (`ClientComponent`) without invoking the action within the cached component.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/05-api-reference/01-directives/use-cache.mdx#_snippet_8

LANGUAGE: tsx
CODE:
```
import ClientComponent from './ClientComponent'

export default async function Page() {
  const performUpdate = async () => {
    'use server'
    // Perform some server-side update
    await db.update(...)
  }

  return <CacheComponent performUpdate={performUpdate} />
}

async function CachedComponent({
  performUpdate,
}: {
  performUpdate: () => Promise<void>
}) {
  'use cache'
  // Do not call performUpdate here
  return <ClientComponent action={performUpdate} />
}
```

LANGUAGE: jsx
CODE:
```
import ClientComponent from './ClientComponent'

export default async function Page() {
  const performUpdate = async () => {
    'use server'
    // Perform some server-side update
    await db.update(...)
  }

  return <CacheComponent performUpdate={performUpdate} />
}

async function CachedComponent({ performUpdate }) {
  'use cache'
  // Do not call performUpdate here
  return <ClientComponent action={performUpdate} />
}
```

----------------------------------------

TITLE: Rendering Blog Layout with Dynamic Navigation (Next.js)
DESCRIPTION: Defines an asynchronous React component for the blog layout. It fetches a list of featured posts using `getPosts` and renders a `NavLink` for each post, allowing users to navigate to individual blog entries. This demonstrates combining server-side data fetching with client-side navigation components within a layout.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/05-api-reference/03-file-conventions/layout.mdx#_snippet_13

LANGUAGE: typescript
CODE:
```
import { NavLink } from './nav-link'
import getPosts from './get-posts'

export default async function Layout({
  children,
}: {
  children: React.ReactNode
}) {
  const featuredPosts = await getPosts()
  return (
    <div>
      {featuredPosts.map((post) => (
        <div key={post.id}>
          <NavLink slug={post.slug}>{post.title}</NavLink>
        </div>
      ))}
      <div>{children}</div>
    </div>
  )
}
```

LANGUAGE: javascript
CODE:
```
import { NavLink } from './nav-link'
import getPosts from './get-posts'

export default async function Layout({ children }) {
  const featuredPosts = await getPosts()
  return (
    <div>
      {featuredPosts.map((post) => (
        <div key={post.id}>
          <NavLink slug={post.slug}>{post.title}</NavLink>
        </div>
      ))}
      <div>{children}</div>
    </div>
  )
}
```

----------------------------------------

TITLE: Implementing User Sign-Up Form with Dynamic Error Display in JSX
DESCRIPTION: This JSX snippet defines the structure for a user sign-up form, featuring email and password input fields. It conditionally renders validation errors for each field, retrieving them from a `state.errors` object. The submit button's `disabled` attribute is controlled by a `pending` flag, preventing multiple submissions during processing.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/02-guides/authentication.mdx#_snippet_8

LANGUAGE: JSX
CODE:
```
      <div>
        <label htmlFor="email">Email</label>
        <input id="email" name="email" placeholder="Email" />
      </div>
      {state?.errors?.email && <p>{state.errors.email}</p>}

      <div>
        <label htmlFor="password">Password</label>
        <input id="password" name="password" type="password" />
      </div>
      {state?.errors?.password && (
        <div>
          <p>Password must:</p>
          <ul>
            {state.errors.password.map((error) => (
              <li key={error}>- {error}</li>
            ))}
          </ul>
        </div>
      )}
      <button disabled={pending} type="submit">
        Sign Up
      </button>
    </form>
  )
}
```

----------------------------------------

TITLE: Configuring Various Metadata Fields in Next.js (JSX)
DESCRIPTION: This snippet illustrates how to define a comprehensive set of metadata fields within the `metadata` object in Next.js. It includes `generator`, `applicationName`, `referrer`, `keywords`, `authors`, `creator`, `publisher`, and `formatDetection` for fine-grained control over page information and SEO.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/05-api-reference/04-functions/generate-metadata.mdx#_snippet_17

LANGUAGE: jsx
CODE:
```
export const metadata = {
  generator: 'Next.js',
  applicationName: 'Next.js',
  referrer: 'origin-when-cross-origin',
  keywords: ['Next.js', 'React', 'JavaScript'],
  authors: [{ name: 'Seb' }, { name: 'Josh', url: 'https://nextjs.org' }],
  creator: 'Jiachi Liu',
  publisher: 'Sebastian Markbåge',
  formatDetection: {
    email: false,
    address: false,
    telephone: false,
  },
}
```

----------------------------------------

TITLE: Generating Static Params for Multiple Dynamic Segments (Next.js TSX)
DESCRIPTION: This snippet illustrates how to generate static parameters for a route with multiple dynamic segments, specifically `[category]` and `[product]`. The `generateStaticParams` function fetches product data and maps it to an array of objects, where each object contains values for both dynamic segments. This ensures that Next.js can pre-render or statically generate pages for all specified product categories and products.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/05-api-reference/04-functions/generate-static-params.mdx#_snippet_9

LANGUAGE: tsx
CODE:
```
// Generate segments for both [category] and [product]
export async function generateStaticParams() {
  const products = await fetch('https://.../products').then((res) => res.json())

  return products.map((product) => ({
    category: product.category.slug,
    product: product.id,
  }))
}

export default function Page({
  params,
}: {
  params: Promise<{ category: string; product: string }>
}) {
  // ...
}
```

----------------------------------------

TITLE: Loading Third-Party Scripts with Next.js Script Component
DESCRIPTION: This snippet demonstrates the recommended way to load third-party JavaScript in a Next.js application using the `next/script` component. It prevents synchronous script loading, which can block the main thread and degrade performance. The `Script` component handles script optimization automatically.
SOURCE: https://github.com/vercel/next.js/blob/canary/errors/no-sync-scripts.mdx#_snippet_0

LANGUAGE: jsx
CODE:
```
import Script from 'next/script'

function Home() {
  return (
    <div class="container">
      <Script src="https://third-party-script.js"></Script>
      <div>Home Page</div>
    </div>
  )
}

export default Home
```

----------------------------------------

TITLE: Using Single Local Font in Next.js
DESCRIPTION: This snippet demonstrates how to use a local font file with `next/font/local`. It involves importing the `localFont` function and specifying the `src` path to your font file, typically located in the `public` folder. The font's class name is then applied to the `<html>` element in the Root Layout for application-wide usage.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/01-getting-started/05-fonts.mdx#_snippet_2

LANGUAGE: tsx
CODE:
```
import localFont from 'next/font/local'

const myFont = localFont({
  src: './my-font.woff2',
})

export default function RootLayout({
  children,
}: {
  children: React.ReactNode
}) {
  return (
    <html lang="en" className={myFont.className}>
      <body>{children}</body>
    </html>
  )
}
```

LANGUAGE: jsx
CODE:
```
import localFont from 'next/font/local'

const myFont = localFont({
  src: './my-font.woff2',
})

export default function RootLayout({ children }) {
  return (
    <html lang="en" className={myFont.className}>
      <body>{children}</body>
    </html>
  )
}
```

----------------------------------------

TITLE: Defining a Dynamic Route Segment in Next.js (TypeScript)
DESCRIPTION: This TypeScript example demonstrates how to define a dynamic route segment using `[slug]` in the file path (e.g., `app/blog/[slug]/page.tsx`). The `params` prop, typed as `Promise<{ slug: string }>`, is destructured to asynchronously access the dynamic `slug` value, enabling the page to render content based on the URL segment.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/05-api-reference/03-file-conventions/dynamic-routes.mdx#_snippet_0

LANGUAGE: TypeScript
CODE:
```
export default async function Page({
  params,
}: {
  params: Promise<{ slug: string }>
}) {
  const { slug } = await params
  return <div>My Post: {slug}</div>
}
```

----------------------------------------

TITLE: Client Component Using Passed Server Action in Next.js
DESCRIPTION: Shows a Client Component (`ClientComponent`) that receives a Server Action function via props (`action`) and invokes it directly from an event handler (e.g., a button click). This demonstrates how actions passed through cached components can be used on the client side.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/05-api-reference/01-directives/use-cache.mdx#_snippet_9

LANGUAGE: tsx
CODE:
```
'use client'

export default function ClientComponent({
  action,
}: {
  action: () => Promise<void>
}) {
  return <button onClick={action}>Update</button>
}
```

LANGUAGE: jsx
CODE:
```
'use client'

export default function ClientComponent({ action }) {
  return <button onClick={action}>Update</button>
}
```

----------------------------------------

TITLE: Conditional Redirect with NextResponse.redirect in Next.js Middleware
DESCRIPTION: This middleware snippet demonstrates how to conditionally redirect users based on an authentication check. If the user is not authenticated, they are redirected to the `/login` page using `NextResponse.redirect`. The `config.matcher` ensures this middleware only runs for paths under `/dashboard`.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/02-guides/redirecting.mdx#_snippet_8

LANGUAGE: typescript
CODE:
```
import { NextResponse, NextRequest } from 'next/server'
import { authenticate } from 'auth-provider'

export function middleware(request: NextRequest) {
  const isAuthenticated = authenticate(request)

  // If the user is authenticated, continue as normal
  if (isAuthenticated) {
    return NextResponse.next()
  }

  // Redirect to login page if not authenticated
  return NextResponse.redirect(new URL('/login', request.url))
}

export const config = {
  matcher: '/dashboard/:path*',
}
```

LANGUAGE: javascript
CODE:
```
import { NextResponse } from 'next/server'
import { authenticate } from 'auth-provider'

export function middleware(request) {
  const isAuthenticated = authenticate(request)

  // If the user is authenticated, continue as normal
  if (isAuthenticated) {
    return NextResponse.next()
  }

  // Redirect to login page if not authenticated
  return NextResponse.redirect(new URL('/login', request.url))
}

export const config = {
  matcher: '/dashboard/:path*',
}
```

----------------------------------------

TITLE: Creating Next.js About Page (App Router, TSX)
DESCRIPTION: This Next.js component defines the about page (`/about`) for an application using the App Router. It displays an 'About' heading and includes a `Link` component to navigate back to the home page (`/`), illustrating inter-page navigation.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/02-guides/testing/playwright.mdx#_snippet_3

LANGUAGE: tsx
CODE:
```
import Link from 'next/link'

export default function Page() {
  return (
    <div>
      <h1>About</h1>
      <Link href="/">Home</Link>
    </div>
  )
}
```

----------------------------------------

TITLE: Setting CORS Headers for GET Requests in Next.js Route Handler (TypeScript)
DESCRIPTION: This snippet demonstrates how to configure Cross-Origin Resource Sharing (CORS) headers for a GET request in a Next.js Route Handler. It uses the standard Web API `Response` object to set `Access-Control-Allow-Origin`, `Access-Control-Allow-Methods`, and `Access-Control-Allow-Headers` to enable cross-origin requests. This allows clients from any origin to make GET, POST, PUT, DELETE, and OPTIONS requests with specified headers.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/03-building-your-application/01-routing/13-route-handlers.mdx#_snippet_18

LANGUAGE: ts
CODE:
```
export async function GET(request: Request) {
  return new Response('Hello, Next.js!', {
    status: 200,
    headers: {
      'Access-Control-Allow-Origin': '*',
      'Access-Control-Allow-Methods': 'GET, POST, PUT, DELETE, OPTIONS',
      'Access-Control-Allow-Headers': 'Content-Type, Authorization',
    },
  })
}
```

----------------------------------------

TITLE: Accessing Request Data with headers/cookies in Next.js App (TSX)
DESCRIPTION: Illustrates using the `headers` and `cookies` functions from `next/headers` within Server Components in the `app` directory to access request data. These functions are read-only and can be used directly or within data fetching functions.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/02-guides/migrating/app-router-migration.mdx#_snippet_26

LANGUAGE: TSX
CODE:
```
// `app` directory
import { cookies, headers } from 'next/headers'

async function getData() {
  const authHeader = (await headers()).get('authorization')

  return '...'
}

export default async function Page() {
  // You can use `cookies` or `headers` inside Server Components
  // directly or in your data fetching function
  const theme = (await cookies()).get('theme')
  const data = await getData()
  return '...'
}
```

----------------------------------------

TITLE: Programmatic Navigation with useRouter (TSX)
DESCRIPTION: Demonstrates how to use the `useRouter` hook from `next/navigation` in a Client Component written in TypeScript/TSX to programmatically navigate to a different route (`/dashboard`) when a button is clicked using the `router.push()` method. Requires the `'use client'` directive.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/05-api-reference/04-functions/use-router.mdx#_snippet_0

LANGUAGE: tsx
CODE:
```
'use client'

import { useRouter } from 'next/navigation'

export default function Page() {
  const router = useRouter()

  return (
    <button type="button" onClick={() => router.push('/dashboard')}>
      Dashboard
    </button>
  )
}
```

----------------------------------------

TITLE: Memoizing Data Requests with React cache in TypeScript
DESCRIPTION: This snippet shows how to use the React `cache` function to memoize the return value of an asynchronous function, specifically for database queries. This prevents redundant executions for the same input `id`, optimizing data retrieval for non-`fetch` APIs.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/04-deep-dive/caching.mdx#_snippet_15

LANGUAGE: ts
CODE:
```
import { cache } from 'react'
import db from '@/lib/db'

export const getItem = cache(async (id: string) => {
  const item = await db.item.findUnique({ id })
  return item
})
```

----------------------------------------

TITLE: Statically Rendering All Paths at Runtime using generateStaticParams (Next.js JSX)
DESCRIPTION: This snippet demonstrates how to configure a dynamic route to be statically rendered at runtime for all visited paths. By returning an empty array from `generateStaticParams`, no paths are pre-rendered at build time, but all paths are rendered on demand and then cached. This is useful for routes where the exact paths are not known at build time or are too numerous to pre-render.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/05-api-reference/04-functions/generate-static-params.mdx#_snippet_7

LANGUAGE: jsx
CODE:
```
export async function generateStaticParams() {
  return []
}
```

----------------------------------------

TITLE: Fetching GraphQL Data with Apollo Client in TypeScript
DESCRIPTION: This snippet demonstrates how to fetch GraphQL data using the `useQuery` hook from `@apollo/client` within a TypeScript React component. It leverages `ViewerDocument`, which is typically generated by GraphQL Code Generator, to ensure strong type safety for the fetched `viewer` object, providing rich IDE autocompletion and compile-time validation. The component then renders a property of the typed data.
SOURCE: https://github.com/vercel/next.js/blob/canary/examples/with-typescript-graphql/README.md#_snippet_0

LANGUAGE: tsx
CODE:
```
import { useQuery } from "@apollo/client";
import { ViewerDocument } from "lib/graphql-operations";

const News = () => {
  // Typed already️⚡️
  const {
    data: { viewer },
  } = useQuery(ViewerDocument);

  return <div>{viewer.name}</div>;
};
```

----------------------------------------

TITLE: Configuring getStaticPaths for Internationalized Dynamic Routes in Next.js
DESCRIPTION: This snippet shows how to configure `getStaticPaths` to prerender specific locale variants for dynamic routes. By including a `locale` field within each path object, Next.js generates a version of the page for each specified locale, such as 'en-US' and 'fr' for 'post-1'.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/02-pages/02-guides/internationalization.mdx#_snippet_10

LANGUAGE: jsx
CODE:
```
export const getStaticPaths = ({ locales }) => {
  return {
    paths: [
      // if no `locale` is provided only the defaultLocale will be generated
      { params: { slug: 'post-1' }, locale: 'en-US' },
      { params: { slug: 'post-1' }, locale: 'fr' },
    ],
    fallback: true,
  }
}
```

----------------------------------------

TITLE: Integrating Google Fonts with Tailwind CSS in App Router (JavaScript)
DESCRIPTION: This snippet demonstrates how to use Google Fonts (`Inter`, `Roboto_Mono`) with Tailwind CSS in a Next.js App Router layout. It imports fonts from `next/font/google`, defines them with `variable` names, and applies these CSS variables to the `<html>` tag, enabling their use in Tailwind classes.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/05-api-reference/02-components/font.mdx#_snippet_36

LANGUAGE: jsx
CODE:
```
import { Inter, Roboto_Mono } from 'next/font/google'

const inter = Inter({
  subsets: ['latin'],
  display: 'swap',
  variable: '--font-inter',
})

const roboto_mono = Roboto_Mono({
  subsets: ['latin'],
  display: 'swap',
  variable: '--font-roboto-mono',
})

export default function RootLayout({ children }) {
  return (
    <html
      lang="en"
      className={`${inter.variable} ${roboto_mono.variable} antialiased`}
    >
      <body>{children}</body>
    </html>
  )
}
```

----------------------------------------

TITLE: Installing Node.js Dependencies (npm)
DESCRIPTION: This command installs all required Node.js project dependencies listed in `package.json` using the npm package manager. It's a prerequisite for running the Next.js application.
SOURCE: https://github.com/vercel/next.js/blob/canary/examples/with-edgedb/README.md#_snippet_7

LANGUAGE: bash
CODE:
```
npm install
```

----------------------------------------

TITLE: Updating/Refreshing User Session in Next.js (JavaScript)
DESCRIPTION: This JavaScript function `updateSession` extends the expiration time of an existing user session cookie in a Next.js application. It retrieves the current session, decrypts its payload, and if valid, re-sets the 'session' cookie with a new expiration date, maintaining HttpOnly, Secure, and SameSite=lax options.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/02-guides/authentication.mdx#_snippet_23

LANGUAGE: JavaScript
CODE:
```
import 'server-only'
import { cookies } from 'next/headers'
import { decrypt } from '@/app/lib/session'

export async function updateSession() {
  const session = (await cookies()).get('session')?.value
  const payload = await decrypt(session)

  if (!session || !payload) {
    return null
  }

  const expires = new Date(Date.now() + 7 * 24 * 60 * 60 * 1000)

  const cookieStore = await cookies()
  cookieStore.set('session', session, {
    httpOnly: true,
    secure: true,
    expires: expires,
    sameSite: 'lax',
    path: '/',
  })
}
```

----------------------------------------

TITLE: Deleting Session Cookie in Next.js (App Router)
DESCRIPTION: This asynchronous function, `deleteSession`, removes the 'session' cookie from the server-side cookie store. It leverages `next/headers` to access and manipulate cookies, ensuring server-only execution. This utility is crucial for logout flows, effectively invalidating the user's session.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/02-guides/authentication.mdx#_snippet_24

LANGUAGE: TypeScript
CODE:
```
import 'server-only'
import { cookies } from 'next/headers'

export async function deleteSession() {
  const cookieStore = await cookies()
  cookieStore.delete('session')
}
```

LANGUAGE: JavaScript
CODE:
```
import 'server-only'
import { cookies } from 'next/headers'

export async function deleteSession() {
  const cookieStore = await cookies()
  cookieStore.delete('session')
}
```

----------------------------------------

TITLE: Memoizing Fetch Requests in Next.js with TypeScript
DESCRIPTION: This TypeScript snippet demonstrates Next.js's automatic memoization of `fetch` requests. The `getItem` function, when invoked multiple times with identical URL and options within a single React render pass, executes the network call only once, serving cached data for subsequent calls to optimize performance.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/04-deep-dive/caching.mdx#_snippet_0

LANGUAGE: tsx
CODE:
```
async function getItem() {
  // The `fetch` function is automatically memoized and the result
  // is cached
  const res = await fetch('https://.../item/1')
  return res.json()
}

// This function is called twice, but only executed the first time
const item = await getItem() // cache MISS

// The second call could be anywhere in your route
const item = await getItem() // cache HIT
```

----------------------------------------

TITLE: Reading Cookies in a Server Component
DESCRIPTION: This snippet demonstrates how to read cookies within a Next.js Server Component using the `cookies` function from `next/headers`. It shows how to import the function, get the cookie store, and retrieve a specific cookie by name.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/05-api-reference/04-functions/cookies.mdx#_snippet_0

LANGUAGE: tsx
CODE:
```
import { cookies } from 'next/headers'

export default async function Page() {
  const cookieStore = await cookies()
  const theme = cookieStore.get('theme')
  return '...'
}
```

LANGUAGE: js
CODE:
```
import { cookies } from 'next/headers'

export default async function Page() {
  const cookieStore = await cookies()
  const theme = cookieStore.get('theme')
  return '...'
}
```

----------------------------------------

TITLE: Migrating Next.js Layout Params to Async (TypeScript)
DESCRIPTION: This snippet shows how to update `params` in Next.js `layout.js` to be asynchronous in Next.js 15. The `params` prop is now a Promise, requiring `await` to access its properties in `generateMetadata` and the default export function.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/02-guides/upgrading/version-15.mdx#_snippet_11

LANGUAGE: typescript
CODE:
```
// Before
type Params = { slug: string }

export function generateMetadata({ params }: { params: Params }) {
  const { slug } = params
}

export default async function Layout({
  children,
  params,
}: {
  children: React.ReactNode
  params: Params
}) {
  const { slug } = params
}

// After
type Params = Promise<{ slug: string }>

export async function generateMetadata({ params }: { params: Params }) {
  const { slug } = await params
}

export default async function Layout({
  children,
  params,
}: {
  children: React.ReactNode
  params: Params
}) {
  const { slug } = await params
}
```

----------------------------------------

TITLE: Catch-All Null Component for Parallel Route in Next.js
DESCRIPTION: This snippet provides a catch-all `null` component for the `@auth` parallel route. It handles navigation to any other page (e.g., `/foo`, `/foo/bar`) by ensuring the parallel slot returns `null`, effectively closing the modal when the main route no longer matches the parallel route's rendering condition.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/05-api-reference/03-file-conventions/parallel-routes.mdx#_snippet_11

LANGUAGE: TypeScript
CODE:
```
export default function CatchAll() {\n  return null\n}
```

LANGUAGE: JavaScript
CODE:
```
export default function CatchAll() {\n  return null\n}
```

----------------------------------------

TITLE: Migrated Page Component (After Migration)
DESCRIPTION: This snippet shows a simple page component in the `app` directory after migrating from the `pages` directory's `getLayout` pattern. The `Page.getLayout` property is removed, and the page now directly exports its content, relying on the `app` directory's layout file for wrapping.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/02-guides/migrating/app-router-migration.mdx#_snippet_10

LANGUAGE: jsx
CODE:
```
export default function Page() {
  return <p>My Page</p>
}
```

----------------------------------------

TITLE: Rewriting Routes with Next.js Middleware (JavaScript)
DESCRIPTION: This JavaScript middleware function checks for an authentication token in the request cookies. If present, it rewrites the '/dashboard' path to '/auth/dashboard'; otherwise, it rewrites it to '/public/dashboard'. This ensures users are directed to the appropriate dashboard view based on their authentication status, preventing unnecessary fetches.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/05-api-reference/02-components/link.mdx#_snippet_29

LANGUAGE: JavaScript
CODE:
```
import { NextResponse } from 'next/server'

export function middleware(request) {
  const nextUrl = request.nextUrl
  if (nextUrl.pathname === '/dashboard') {
    if (request.cookies.authToken) {
      return NextResponse.rewrite(new URL('/auth/dashboard', request.url))
    } else {
      return NextResponse.rewrite(new URL('/public/dashboard', request.url))
    }
  }
}
```

----------------------------------------

TITLE: Importing Client Components with next/dynamic in Next.js
DESCRIPTION: This snippet demonstrates how to use `next/dynamic` to lazy load Client Components in a Next.js application. It shows three patterns: immediate loading into a separate bundle, conditional loading based on a state variable, and client-side only loading by setting `ssr: false`. It requires `useState` from React for conditional rendering.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/02-guides/lazy-loading.mdx#_snippet_0

LANGUAGE: jsx
CODE:
```
'use client'

import { useState } from 'react'
import dynamic from 'next/dynamic'

// Client Components:
const ComponentA = dynamic(() => import('../components/A'))
const ComponentB = dynamic(() => import('../components/B'))
const ComponentC = dynamic(() => import('../components/C'), { ssr: false })

export default function ClientComponentExample() {
  const [showMore, setShowMore] = useState(false)

  return (
    <div>
      {/* Load immediately, but in a separate client bundle */}
      <ComponentA />

      {/* Load on demand, only when/if the condition is met */}
      {showMore && <ComponentB />}
      <button onClick={() => setShowMore(!showMore)}>Toggle</button>

      {/* Load only on the client side */}
      <ComponentC />
    </div>
  )
}
```

----------------------------------------

TITLE: Defining a Dynamic Route Segment in Next.js (JavaScript)
DESCRIPTION: This JavaScript example demonstrates how to define a dynamic route segment using `[slug]` in the file path (e.g., `app/blog/[slug]/page.js`). The `params` prop is destructured to asynchronously access the dynamic `slug` value, enabling the page to render content based on the URL segment.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/05-api-reference/03-file-conventions/dynamic-routes.mdx#_snippet_1

LANGUAGE: JavaScript
CODE:
```
export default async function Page({ params }) {
  const { slug } = await params
  return <div>My Post: {slug}</div>
}
```

----------------------------------------

TITLE: Checking Draft Mode Status in Next.js Server Component
DESCRIPTION: Demonstrates how to import and use the `draftMode` function within a Next.js Server Component to check if Draft Mode is currently enabled using the `isEnabled` property.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/05-api-reference/04-functions/draft-mode.mdx#_snippet_0

LANGUAGE: tsx
CODE:
```
import { draftMode } from 'next/headers'

export default async function Page() {
  const { isEnabled } = await draftMode()
}
```

LANGUAGE: jsx
CODE:
```
import { draftMode } from 'next/headers'

export default async function Page() {
  const { isEnabled } = await draftMode()
}
```

----------------------------------------

TITLE: Accessing searchParams in Next.js Server Components
DESCRIPTION: This snippet demonstrates how to access and destructure `searchParams` within an `async` Server Component in Next.js. It allows handling URL query string parameters for filtering, pagination, or sorting, providing default values if parameters are not present. The `searchParams` prop is a Promise that resolves to an object containing the query parameters.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/05-api-reference/03-file-conventions/page.mdx#_snippet_4

LANGUAGE: tsx
CODE:
```
export default async function Page({
  searchParams,
}: {
  searchParams: Promise<{ [key: string]: string | string[] | undefined }>
}) {
  const { page = '1', sort = 'asc', query = '' } = await searchParams

  return (
    <div>
      <h1>Product Listing</h1>
      <p>Search query: {query}</p>
      <p>Current page: {page}</p>
      <p>Sort order: {sort}</p>
    </div>
  )
}
```

LANGUAGE: jsx
CODE:
```
export default async function Page({ searchParams }) {
  const { page = '1', sort = 'asc', query = '' } = await searchParams

  return (
    <div>
      <h1>Product Listing</h1>
      <p>Search query: {query}</p>
      <p>Current page: {page}</p>
      <p>Sort order: {sort}</p>
    </div>
  )
}
```

----------------------------------------

TITLE: Generating Dynamic Metadata with TypeScript in Next.js
DESCRIPTION: This snippet illustrates how to use the asynchronous `generateMetadata` function in TypeScript to dynamically fetch and set metadata based on route parameters. It's useful for pages where metadata depends on external data, such as a blog post's title and description fetched from an API, ensuring SEO-friendly content for dynamic routes.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/01-getting-started/13-metadata-and-og-images.mdx#_snippet_3

LANGUAGE: TypeScript
CODE:
```
import type { Metadata, ResolvingMetadata } from 'next'

type Props = {
  params: Promise<{ slug: string }>
  searchParams: Promise<{ [key: string]: string | string[] | undefined }>
}

export async function generateMetadata(
  { params, searchParams }: Props,
  parent: ResolvingMetadata
): Promise<Metadata> {
  const slug = (await params).slug

  // fetch post information
  const post = await fetch(`https://api.vercel.app/blog/${slug}`).then((res) =>
    res.json()
  )

  return {
    title: post.title,
    description: post.description,
  }
}

export default function Page({ params, searchParams }: Props) {}
```

----------------------------------------

TITLE: Getting Single Cookie in Next.js Server Component
DESCRIPTION: Demonstrates how to retrieve a single cookie by name using the `cookies().get()` method within an asynchronous Next.js Server Component. Requires importing `cookies` from `next/headers` and using `await`.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/05-api-reference/04-functions/cookies.mdx#_snippet_1

LANGUAGE: tsx
CODE:
```
import { cookies } from 'next/headers'

export default async function Page() {
  const cookieStore = await cookies()
  const theme = cookieStore.get('theme')
  return '...'
}
```

LANGUAGE: jsx
CODE:
```
import { cookies } from 'next/headers'

export default async function Page() {
  const cookieStore = await cookies()
  const theme = cookieStore.get('theme')
  return '...'
}
```

----------------------------------------

TITLE: Using Closures with Next.js Server Actions for Data Snapshots (TypeScript)
DESCRIPTION: This TypeScript React component illustrates how a Server Action defined within a component forms a closure, allowing it to access variables from its outer scope, such as `publishVersion`. This captures a snapshot of data at render time, which is then automatically encrypted by Next.js when sent to the client and back to the server.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/03-building-your-application/02-data-fetching/03-server-actions-and-mutations.mdx#_snippet_19

LANGUAGE: tsx
CODE:
```
export default async function Page() {
  const publishVersion = await getLatestVersion();

  async function publish() {
    "use server";
    if (publishVersion !== await getLatestVersion()) {
      throw new Error('The version has changed since pressing publish');
    }
    ...
  }

  return (
    <form>
      <button formAction={publish}>Publish</button>
    </form>
  );
}
```

----------------------------------------

TITLE: Importing and Executing Server Function in Client Component
DESCRIPTION: This client component demonstrates how to import and invoke a server function, `fetchUsers`, defined in a separate 'use server' file. The `onClick` handler triggers the server function, allowing client-side interactions to initiate server-side data operations.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/05-api-reference/01-directives/use-server.mdx#_snippet_2

LANGUAGE: TypeScript
CODE:
```
'use client'
import { fetchUsers } from '../actions'

export default function MyButton() {
  return <button onClick={() => fetchUsers()}>Fetch Users</button>
}
```

LANGUAGE: JavaScript
CODE:
```
'use client'
import { fetchUsers } from '../actions'

export default function MyButton() {
  return <button onClick={() => fetchUsers()}>Fetch Users</button>
}
```

----------------------------------------

TITLE: Migrating Next.js Cookies API to Async (TypeScript)
DESCRIPTION: This snippet demonstrates the recommended asynchronous usage of the `cookies` API in Next.js 15. The `cookies()` function now returns a Promise, requiring `await` to access the cookie store.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/02-guides/upgrading/version-15.mdx#_snippet_2

LANGUAGE: typescript
CODE:
```
import { cookies } from 'next/headers'

// Before
const cookieStore = cookies()
const token = cookieStore.get('token')

// After
const cookieStore = await cookies()
const token = cookieStore.get('token')
```

----------------------------------------

TITLE: Loading Scripts Before Interaction (App Router) - TypeScript
DESCRIPTION: This snippet demonstrates loading a script using the `beforeInteractive` strategy within the Next.js App Router. Scripts with this strategy are injected into the initial HTML from the server, downloaded before any Next.js module, and executed in the order they are placed. They are typically placed in `app/layout.tsx` for critical site-wide scripts like bot detectors or cookie consent managers, and their execution does not block page hydration.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/05-api-reference/02-components/script.mdx#_snippet_1

LANGUAGE: TypeScript
CODE:
```
import Script from 'next/script'

export default function RootLayout({
  children,
}: {
  children: React.ReactNode
}) {
  return (
    <html lang="en">
      <body>
        {children}
        <Script
          src="https://example.com/script.js"
          strategy="beforeInteractive"
        />
      </body>
    </html>
  )
}
```

----------------------------------------

TITLE: Accessing Dynamic Route Parameters with useParams (JavaScript)
DESCRIPTION: This example illustrates the use of the `useParams` hook in a Next.js Client Component to retrieve dynamic route parameters from the current URL. It demonstrates how the `params` object reflects the values from dynamic segments like `/shop/[tag]/[item]`.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/05-api-reference/04-functions/use-params.mdx#_snippet_1

LANGUAGE: javascript
CODE:
```
'use client'

import { useParams } from 'next/navigation'

export default function ExampleClientComponent() {
  const params = useParams()

  // Route -> /shop/[tag]/[item]
  // URL -> /shop/shoes/nike-air-max-97
  // `params` -> { tag: 'shoes', item: 'nike-air-max-97' }
  console.log(params)

  return '...'
}
```

----------------------------------------

TITLE: Defining `generateMetadata` as a Regular Function (TSX)
DESCRIPTION: Demonstrates how to define the `generateMetadata` function as a standard synchronous function, returning a `Metadata` object. This function is used to dynamically generate metadata for a page or layout.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/05-api-reference/04-functions/generate-metadata.mdx#_snippet_68

LANGUAGE: tsx
CODE:
```
import type { Metadata } from 'next'

export function generateMetadata(): Metadata {
  return {
    title: 'Next.js',
  }
}
```

----------------------------------------

TITLE: Controlling Dynamic Segment Generation in Next.js App Router (JavaScript)
DESCRIPTION: This snippet illustrates the `export const dynamicParams` option in JavaScript, used within Next.js App Router layouts or pages. It dictates the behavior when a dynamic segment is accessed that was not pre-generated by `generateStaticParams`. Setting it to `true` (default) generates the segment on demand, while `false` results in a 404 error.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/05-api-reference/03-file-conventions/route-segment-config.mdx#_snippet_5

LANGUAGE: js
CODE:
```
export const dynamicParams = true // true | false,
```

----------------------------------------

TITLE: Creating an Edge API Route Proxy in Next.js (TypeScript)
DESCRIPTION: This snippet shows how to implement an Edge API Route in Next.js to act as a proxy for an external backend API. It demonstrates setting the `runtime` to `'edge'` and forwarding headers (like authorization cookies) from the incoming request to the external `fetch` call, suitable for secure API interactions.
SOURCE: https://github.com/vercel/next.js/blob/canary/errors/middleware-upgrade-guide.mdx#_snippet_5

LANGUAGE: TypeScript
CODE:
```
import { type NextRequest } from 'next/server'

export const config = {
  runtime: 'edge',
}

export default async function handler(req: NextRequest) {
  const authorization = req.cookies.get('authorization')
  return fetch('https://backend-api.com/api/protected', {
    method: req.method,
    headers: {
      authorization,
    },
    redirect: 'manual',
  })
}
```

----------------------------------------

TITLE: Defining Root Layout with Children Prop (JavaScript)
DESCRIPTION: This snippet defines the default root layout for a Next.js application using JavaScript. It accepts a `children` prop, which will be populated with nested layouts or pages, and renders the basic HTML structure including `<html>` and `<body>` tags. This layout applies to all routes within the `app` directory.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/02-guides/migrating/app-router-migration.mdx#_snippet_5

LANGUAGE: jsx
CODE:
```
export default function RootLayout({
  // Layouts must accept a children prop.
  // This will be populated with nested layouts or pages
  children,
}) {
  return (
    <html lang="en">
      <body>{children}</body>
    </html>
  )
}
```

----------------------------------------

TITLE: Correctly Calling `headers()` in Next.js Route Handler
DESCRIPTION: This snippet illustrates the proper usage of the `headers()` function within a Next.js Route Handler. It emphasizes that `headers()` should be called inside an asynchronous function like `GET`, `POST`, etc., ensuring it operates within the request scope and prevents 'Dynamic API called outside request' errors.
SOURCE: https://github.com/vercel/next.js/blob/canary/errors/next-dynamic-api-wrong-context.mdx#_snippet_1

LANGUAGE: JavaScript
CODE:
```
import { headers } from 'next/headers'

export async function GET() {
  const headersList = await headers()
  return ...
}
```

----------------------------------------

TITLE: Base Metadata for Inheritance Example in Next.js Layout (JSX)
DESCRIPTION: This snippet defines the base metadata in `app/layout.js` for the inheritance example. It sets a default `title` and `openGraph` properties that will be inherited by child pages if they do not explicitly define these fields themselves.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/05-api-reference/04-functions/generate-metadata.mdx#_snippet_86

LANGUAGE: jsx
CODE:
```
export const metadata = {
  title: 'Acme',
  openGraph: {
    title: 'Acme',
    description: 'Acme is a...', 
  },
}
```

----------------------------------------

TITLE: Displaying Server Function Errors with useActionState in Next.js (React)
DESCRIPTION: This React client component demonstrates how to use the `useActionState` hook to manage the state and actions of a form. It passes the `createPost` server action and an initial state to the hook, then uses the returned `state` to conditionally display error messages to the user. It takes no explicit props but relies on the `createPost` action for its functionality.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/01-getting-started/11-error-handling.mdx#_snippet_1

LANGUAGE: TSX
CODE:
```
'use client'

import { useActionState } from 'react'
import { createPost } from '@/app/actions'

const initialState = {
  message: '',
}

export function Form() {
  const [state, formAction, pending] = useActionState(createPost, initialState)

  return (
    <form action={formAction}>
      <label htmlFor="title">Title</label>
      <input type="text" id="title" name="title" required />
      <label htmlFor="content">Content</label>
      <textarea id="content" name="content" required />
      {state?.message && <p aria-live="polite">{state.message}</p>}
      <button disabled={pending}>Create Post</button>
    </form>
  )
}
```

LANGUAGE: JSX
CODE:
```
'use client'

import { useActionState } from 'react'
import { createPost } from '@/app/actions'

const initialState = {
  message: '',
}

export function Form() {
  const [state, formAction, pending] = useActionState(createPost, initialState)

  return (
    <form action={formAction}>
      <label htmlFor="title">Title</label>
      <input type="text" id="title" name="title" required />
      <label htmlFor="content">Content</label>
      <textarea id="content" name="content" required />
      {state?.message && <p aria-live="polite">{state.message}</p>}
      <button disabled={pending}>Create Post</button>
    </form>
  )
}
```

----------------------------------------

TITLE: Migrating Route Handlers to Await `params` (JS)
DESCRIPTION: This example illustrates the necessary change for JavaScript Route Handlers, where `segmentData.params` now returns a Promise and must be `await`ed to access its properties. This aligns with the updated asynchronous data handling in Next.js 15.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/02-guides/upgrading/version-15.mdx#_snippet_20

LANGUAGE: js
CODE:
```
// Before
export async function GET(request, segmentData) {
  const params = segmentData.params
  const slug = params.slug
}
```

LANGUAGE: js
CODE:
```
// After
export async function GET(request, segmentData) {
  const params = await segmentData.params
  const slug = params.slug
}
```

----------------------------------------

TITLE: Sending HTTP Response in Next.js API Routes
DESCRIPTION: This snippet demonstrates how to send an HTTP response in a Next.js API route. It shows handling both successful asynchronous operations with a 200 status and error cases with a 500 status, sending a JSON object as the response body.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/02-pages/03-building-your-application/01-routing/07-api-routes.mdx#_snippet_10

LANGUAGE: TypeScript
CODE:
```
import type { NextApiRequest, NextApiResponse } from 'next'

export default async function handler(
  req: NextApiRequest,
  res: NextApiResponse
) {
  try {
    const result = await someAsyncOperation()
    res.status(200).send({ result })
  } catch (err) {
    res.status(500).send({ error: 'failed to fetch data' })
  }
}
```

LANGUAGE: JavaScript
CODE:
```
export default async function handler(req, res) {
  try {
    const result = await someAsyncOperation()
    res.status(200).send({ result })
  } catch (err) {
    res.status(500).send({ error: 'failed to fetch data' })
  }
}
```

----------------------------------------

TITLE: Creating Database Sessions with Next.js App Router (TypeScript)
DESCRIPTION: This snippet demonstrates how to create a new session in a database and store its encrypted ID in a cookie using the Next.js App Router. It involves inserting session data into the database, encrypting the session ID, and setting an httpOnly cookie for secure storage and optimistic authentication checks. Requires `next/headers`, a database client (`@/app/lib/db`), and an encryption utility (`@/app/lib/session`).
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/02-guides/authentication.mdx#_snippet_27

LANGUAGE: TypeScript
CODE:
```
import cookies from 'next/headers'
import { db } from '@/app/lib/db'
import { encrypt } from '@/app/lib/session'

export async function createSession(id: number) {
  const expiresAt = new Date(Date.now() + 7 * 24 * 60 * 60 * 1000)

  // 1. Create a session in the database
  const data = await db
    .insert(sessions)
    .values({
      userId: id,
      expiresAt,
    })
    // Return the session ID
    .returning({ id: sessions.id })

  const sessionId = data[0].id

  // 2. Encrypt the session ID
  const session = await encrypt({ sessionId, expiresAt })

  // 3. Store the session in cookies for optimistic auth checks
  const cookieStore = await cookies()
  cookieStore.set('session', session, {
    httpOnly: true,
    secure: true,
    expires: expiresAt,
    sameSite: 'lax',
    path: '/',
  })
}
```

----------------------------------------

TITLE: Correct Usage: createContext in Client Component
DESCRIPTION: This code snippet shows the corrected approach by adding the `'use client'` directive at the top of the file. This marks the component as a Client Component, allowing the safe and intended use of the `createContext` hook.
SOURCE: https://github.com/vercel/next.js/blob/canary/errors/context-in-server-component.mdx#_snippet_1

LANGUAGE: jsx
CODE:
```
'use client'
import { createContext } from 'react'

const Context = createContext()
```

----------------------------------------

TITLE: Embedding YouTube Video in Next.js App Router
DESCRIPTION: This snippet demonstrates how to use the `YouTubeEmbed` component within a Next.js App Router `page.js` file. It imports the component from `@next/third-parties/google` and renders a YouTube video with a specified `videoid`, `height`, and custom `params` to disable controls.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/02-guides/third-party-libraries.mdx#_snippet_12

LANGUAGE: jsx
CODE:
```
import { YouTubeEmbed } from '@next/third-parties/google'

export default function Page() {
  return <YouTubeEmbed videoid="ogfYd705cRs" height={400} params="controls=0" />
}
```

----------------------------------------

TITLE: Setting a Cookie with NextResponse in TypeScript
DESCRIPTION: This snippet demonstrates how to set a cookie on the `NextResponse` object. It uses `response.cookies.set('name', 'value')` to add a `Set-Cookie` header to the response, which will be sent back to the client. The example sets a cookie named 'show-banner' with a value of 'false'.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/05-api-reference/04-functions/next-response.mdx#_snippet_0

LANGUAGE: TypeScript
CODE:
```
// Given incoming request /home
let response = NextResponse.next()
// Set a cookie to hide the banner
response.cookies.set('show-banner', 'false')
// Response will have a `Set-Cookie:show-banner=false;path=/home` header
return response
```

----------------------------------------

TITLE: Updating Username with Permanent Redirect in Next.js
DESCRIPTION: This snippet illustrates the use of `permanentRedirect` in a Next.js Server Action to permanently redirect a user to a new profile URL after a username change. It also uses `revalidateTag` to invalidate cached data related to the username, ensuring consistency. This function issues a permanent (308) redirect.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/02-guides/redirecting.mdx#_snippet_1

LANGUAGE: typescript
CODE:
```
'use server'\n\nimport { permanentRedirect } from 'next/navigation'\nimport { revalidateTag } from 'next/cache'\n\nexport async function updateUsername(username: string, formData: FormData) {\n  try {\n    // Call database\n  } catch (error) {\n    // Handle errors\n  }\n\n  revalidateTag('username') // Update all references to the username\n  permanentRedirect(`/profile/${username}`) // Navigate to the new user profile\n}
```

LANGUAGE: javascript
CODE:
```
'use server'\n\nimport { permanentRedirect } from 'next/navigation'\nimport { revalidateTag } from 'next/cache'\n\nexport async function updateUsername(username, formData) {\n  try {\n    // Call database\n  } catch (error) {\n    // Handle errors\n  }\n\n  revalidateTag('username') // Update all references to the username\n  permanentRedirect(`/profile/${username}`) // Navigate to the new user profile\n}
```

----------------------------------------

TITLE: Handling Pending State with useActionState in TypeScript
DESCRIPTION: This snippet demonstrates how to use the `useActionState` hook to manage the pending state of a form submission. It disables the submit button while the `createUser` action is being executed, providing visual feedback to the user.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/02-guides/forms.mdx#_snippet_9

LANGUAGE: TypeScript
CODE:
```
'use client'

import { useActionState } from 'react'
import { createUser } from '@/app/actions'

export function Signup() {
  const [state, formAction, pending] = useActionState(createUser, initialState)

  return (
    <form action={formAction}>
      {/* Other form elements */}
      <button disabled={pending}>Sign up</button>
    </form>
  )
}
```

----------------------------------------

TITLE: Using revalidatePath in a Route Handler - Next.js - TypeScript
DESCRIPTION: This Route Handler demonstrates how to dynamically revalidate a path based on a query parameter. It retrieves the 'path' from the request URL and calls `revalidatePath` if a path is provided, returning a JSON response indicating success or a missing path.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/05-api-reference/04-functions/revalidatePath.mdx#_snippet_6

LANGUAGE: ts
CODE:
```
import { revalidatePath } from 'next/cache'
import type { NextRequest } from 'next/server'

export async function GET(request: NextRequest) {
  const path = request.nextUrl.searchParams.get('path')

  if (path) {
    revalidatePath(path)
    return Response.json({ revalidated: true, now: Date.now() })
  }

  return Response.json({
    revalidated: false,
    now: Date.now(),
    message: 'Missing path to revalidate',
  })
}
```

----------------------------------------

TITLE: Accessing URL Query Parameters in Next.js Route Handlers
DESCRIPTION: This snippet demonstrates how to retrieve URL query parameters within a Next.js Route Handler. It uses the `NextRequest` object's `nextUrl.searchParams` property to access the URLSearchParams interface, allowing extraction of individual query values using `get()`. The expected input is a `NextRequest` object, and the output is the value of the specified query parameter.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/03-building-your-application/01-routing/13-route-handlers.mdx#_snippet_13

LANGUAGE: TypeScript
CODE:
```
import { type NextRequest } from 'next/server'

export function GET(request: NextRequest) {
  const searchParams = request.nextUrl.searchParams
  const query = searchParams.get('query')
  // query is "hello" for /api/search?query=hello
}
```

LANGUAGE: JavaScript
CODE:
```
export function GET(request) {
  const searchParams = request.nextUrl.searchParams
  const query = searchParams.get('query')
  // query is "hello" for /api/search?query=hello
}
```

----------------------------------------

TITLE: Correct `next/link` Usage for Next.js Internal Navigation
DESCRIPTION: This snippet demonstrates the recommended way to navigate to internal Next.js pages using the `Link` component from `next/link`. Importing and wrapping the anchor element with `Link` enables client-side route transitions, providing a smooth single-page application experience.
SOURCE: https://github.com/vercel/next.js/blob/canary/errors/no-html-link-for-pages.mdx#_snippet_1

LANGUAGE: jsx
CODE:
```
import Link from 'next/link'

function Home() {
  return (
    <div>
      <Link href="/about">About Us</Link>
    </div>
  )
}

export default Home
```

----------------------------------------

TITLE: Creating JSON Responses with NextResponse (JavaScript)
DESCRIPTION: The static `json` method is a convenient way to create a `NextResponse` instance that sends a JSON payload. It leverages the standard `Response.json` method and then wraps the resulting response body and initialization options within a new `NextResponse` instance. This is commonly used in API routes or middleware to return structured data.
SOURCE: https://github.com/vercel/next.js/blob/canary/turbopack/crates/turbopack-ecmascript/tests/tree-shaker/analyzer/next-response/output.md#_snippet_28

LANGUAGE: JavaScript
CODE:
```
    static json(body, init) {
        const response = Response.json(body, init);
        return new NextResponse(response.body, response);
    }
```

----------------------------------------

TITLE: Statically Generating Routes with `generateStaticParams` in Next.js (TypeScript)
DESCRIPTION: This snippet demonstrates how to use the `generateStaticParams` function in a Next.js App Router page to define dynamic route segments at build time. It fetches a list of posts and returns an array of objects, where each object's `slug` property corresponds to a dynamic route parameter. Required dependencies include `fetch` for data retrieval.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/05-api-reference/03-file-conventions/dynamic-routes.mdx#_snippet_2

LANGUAGE: TypeScript
CODE:
```
export async function generateStaticParams() {
  const posts = await fetch('https://.../posts').then((res) => res.json())

  return posts.map((post) => ({
    slug: post.slug,
  }))
}
```

----------------------------------------

TITLE: Tagging Fetch Cache Entries in Next.js (JSX)
DESCRIPTION: This code snippet illustrates how to associate specific tags with a `fetch` request's cache entry using the `next.tags` option. These tags enable fine-grained control over data caching, allowing later revalidation of specific cache entries based on their assigned tags.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/04-deep-dive/caching.mdx#_snippet_8

LANGUAGE: JSX
CODE:
```
fetch(`https://...`, { next: { tags: ['a', 'b', 'c'] } })
```

----------------------------------------

TITLE: Creating a Dynamic User Component with Cookies (TypeScript)
DESCRIPTION: This snippet defines an asynchronous React component that accesses the `cookies` API from `next/headers`. Since `cookies` requires looking at the incoming request, this component will trigger dynamic rendering, making it suitable for streaming with Suspense.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/01-getting-started/12-partial-prerendering.mdx#_snippet_6

LANGUAGE: TSX
CODE:
```
import { cookies } from 'next/headers'

export async function User() {
  const session = (await cookies()).get('session')?.value
  return '...'
}
```

----------------------------------------

TITLE: Generating Static Routes for Locales in Next.js Layout (JSX)
DESCRIPTION: This JavaScript snippet shows how to use `generateStaticParams` in a Next.js App Router root layout to pre-render static routes for a defined set of locales (e.g., 'en-US', 'de'). This optimizes performance by generating pages at build time for each specified locale.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/02-guides/internationalization.mdx#_snippet_11

LANGUAGE: JSX
CODE:
```
export async function generateStaticParams() {
  return [{ lang: 'en-US' }, { lang: 'de' }]
}

export default async function RootLayout({ children, params }) {
  return (
    <html lang={(await params).lang}>
      <body>{children}</body>
    </html>
  )
}
```

----------------------------------------

TITLE: Displaying Pending State with useActionState (JSX)
DESCRIPTION: This client component uses React's useActionState hook to manage the state of a server action (createPost) and display a loading indicator. The 'pending' boolean returned by the hook allows the UI to show a LoadingSpinner while the action is in progress.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/01-getting-started/10-updating-data.mdx#_snippet_11

LANGUAGE: jsx
CODE:
```
'use client'

import { useActionState } from 'react'
import { createPost } from '@/app/actions'
import { LoadingSpinner } from '@/app/ui/loading-spinner'

export function Button() {
  const [state, action, pending] = useActionState(createPost, false)

  return (
    <button onClick={async () => action()}>
      {pending ? <LoadingSpinner /> : 'Create Post'}
    </button>
  )
}
```

----------------------------------------

TITLE: Creating `getDictionary` Function for Localization (JavaScript)
DESCRIPTION: This JavaScript function, marked as 'server-only', asynchronously loads the appropriate JSON dictionary based on the provided locale. It uses dynamic imports to fetch the translation files, ensuring only the necessary dictionary is loaded.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/02-guides/internationalization.mdx#_snippet_7

LANGUAGE: JavaScript
CODE:
```
import 'server-only'

const dictionaries = {
  en: () => import('./dictionaries/en.json').then((module) => module.default),
  nl: () => import('./dictionaries/nl.json').then((module) => module.default),
}

export const getDictionary = async (locale) => dictionaries[locale]()
```

----------------------------------------

TITLE: Accessing Search Parameters on Results Page in Next.js
DESCRIPTION: This snippet shows how to access the `searchParams` prop on a Next.js page to retrieve the search query from the URL. The query is then used to fetch search results from an external data source (`getSearchResults`), which are subsequently rendered on the page.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/05-api-reference/02-components/form.mdx#_snippet_2

LANGUAGE: tsx
CODE:
```
import { getSearchResults } from '@/lib/search'

export default async function SearchPage({
  searchParams,
}: {
  searchParams: Promise<{ [key: string]: string | string[] | undefined }>
}) {
  const results = await getSearchResults((await searchParams).query)

  return <div>...</div>
}
```

LANGUAGE: jsx
CODE:
```
import { getSearchResults } from '@/lib/search'

export default async function SearchPage({ searchParams }) {
  const results = await getSearchResults((await searchParams).query)

  return <div>...</div>
}
```

----------------------------------------

TITLE: Setting Default Revalidation Time with `revalidate` in JavaScript
DESCRIPTION: This snippet demonstrates how to set the default revalidation time for a Next.js layout, page, or route using the `revalidate` export. It accepts `false` (default, cache indefinitely), `0` (always dynamic), or a `number` (seconds) to control caching behavior. This option does not override `revalidate` values set by individual `fetch` requests.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/05-api-reference/03-file-conventions/route-segment-config.mdx#_snippet_7

LANGUAGE: js
CODE:
```
export const revalidate = false
// false | 0 | number
```

----------------------------------------

TITLE: Accessing Runtime Environment Variables in App Router (JavaScript)
DESCRIPTION: This JavaScript snippet shows how to access runtime environment variables within the Next.js App Router. Similar to the TypeScript example, using dynamic APIs like `connection()` ensures that the component is dynamically rendered, allowing `process.env.MY_VALUE` to be evaluated at runtime on the server.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/02-guides/environment-variables.mdx#_snippet_14

LANGUAGE: JavaScript
CODE:
```
import { connection } from 'next/server'

export default async function Component() {
  await connection()
  // cookies, headers, and other Dynamic APIs
  // will also opt into dynamic rendering, meaning
  // this env variable is evaluated at runtime
  const value = process.env.MY_VALUE
  // ...
}
```

----------------------------------------

TITLE: Implementing Error Recovery with Reset Function in Next.js (TSX)
DESCRIPTION: This Client Component demonstrates how to use the `reset` function provided to an `error.js` boundary to allow users to attempt recovery from a temporary error. Clicking the 'Try again' button triggers a re-render of the error boundary's contents, potentially resolving the issue.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/05-api-reference/03-file-conventions/error.mdx#_snippet_2

LANGUAGE: tsx
CODE:
```
'use client' // Error boundaries must be Client Components

export default function Error({
  error,
  reset,
}: {
  error: Error & { digest?: string }
  reset: () => void
}) {
  return (
    <div>
      <h2>Something went wrong!</h2>
      <button onClick={() => reset()}>Try again</button>
    </div>
  )
}
```

----------------------------------------

TITLE: Implementing Data Transfer Object (DTO) in JavaScript
DESCRIPTION: This snippet demonstrates how to implement a Data Transfer Object (DTO) in JavaScript to control which user data fields are exposed to the client. It includes authorization logic (canSeeUsername, canSeePhoneNumber) to conditionally return sensitive information based on the viewer's permissions, ensuring only necessary and authorized data is transferred.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/02-guides/authentication.mdx#_snippet_35

LANGUAGE: js
CODE:
```
import 'server-only'
import { getUser } from '@/app/lib/dal'

function canSeeUsername(viewer) {
  return true
}

function canSeePhoneNumber(viewer, team) {
  return viewer.isAdmin || team === viewer.team
}

export async function getProfileDTO(slug) {
  const data = await db.query.users.findMany({
    where: eq(users.slug, slug),
    // Return specific columns here
  })
  const user = data[0]

  const currentUser = await getUser(user.id)

  // Or return only what's specific to the query here
  return {
    username: canSeeUsername(currentUser) ? user.username : null,
    phonenumber: canSeePhoneNumber(currentUser, user.team)
      ? user.phonenumber
      : null,
  }
}
```

----------------------------------------

TITLE: Caching Data Fetching with React Cache (TypeScript)
DESCRIPTION: This TypeScript utility snippet demonstrates how to create a reusable and cached data fetching function using React's `cache` API and the `server-only` package. The `getItem` function is wrapped with `cache` to ensure its results are memoized on the server, preventing redundant fetches. A `preload` function is also provided to eagerly initiate the cached `getItem` call.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/01-getting-started/08-fetching-data.mdx#_snippet_16

LANGUAGE: ts
CODE:
```
import { cache } from 'react'
import 'server-only'
import { getItem } from '@/lib/data'

export const preload = (id: string) => {
  void getItem(id)
}

export const getItem = cache(async (id: string) => {
  // ...
})
```

----------------------------------------

TITLE: Generating Category Static Params for Next.js Layout
DESCRIPTION: This snippet demonstrates `generateStaticParams` in a Next.js layout to pre-render dynamic `[category]` segments. It fetches product data and maps it to an array of objects, each containing a `category` slug for static path generation.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/05-api-reference/04-functions/generate-static-params.mdx#_snippet_11

LANGUAGE: TypeScript
CODE:
```
// Generate segments for [category]
export async function generateStaticParams() {
  const products = await fetch('https://.../products').then((res) => res.json())

  return products.map((product) => ({
    category: product.category.slug,
  }))
}

export default function Layout({
  params,
}: {
  params: Promise<{ category: string }>
}) {
  // ...
}
```

LANGUAGE: JavaScript
CODE:
```
// Generate segments for [category]
export async function generateStaticParams() {
  const products = await fetch('https://.../products').then((res) => res.json())

  return products.map((product) => ({
    category: product.category.slug,
  }))
}

export default function Layout({ params }) {
  // ...
}
```

----------------------------------------

TITLE: Generating Metadata and Page Content with Memoized Data (TypeScript)
DESCRIPTION: This Next.js page component in TypeScript demonstrates how to use the memoized `getPost` function to fetch blog post data for both `generateMetadata` and the default page component. This ensures the data is fetched only once, optimizing performance. It takes `params` containing the `slug` as input.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/01-getting-started/13-metadata-and-og-images.mdx#_snippet_7

LANGUAGE: TypeScript
CODE:
```
import { getPost } from '@/app/lib/data'

export async function generateMetadata({
  params,
}: {
  params: { slug: string }
}) {
  const post = await getPost(params.slug)
  return {
    title: post.title,
    description: post.description,
  }
}

export default async function Page({ params }: { params: { slug: string } }) {
  const post = await getPost(params.slug)
  return <div>{post.title}</div>
}
```

----------------------------------------

TITLE: Basic Web Vitals Reporting in App Router
DESCRIPTION: Illustrates how to create a client component (`web-vitals.js`) using `useReportWebVitals` and import it into the root layout (`layout.js`) to enable basic Web Vitals reporting in the App Router.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/02-guides/analytics.mdx#_snippet_2

LANGUAGE: jsx
CODE:
```
'use client'

import { useReportWebVitals } from 'next/web-vitals'

export function WebVitals() {
  useReportWebVitals((metric) => {
    console.log(metric)
  })
}
```

LANGUAGE: jsx
CODE:
```
import { WebVitals } from './_components/web-vitals'

export default function Layout({ children }) {
  return (
    <html>
      <body>
        <WebVitals />
        {children}
      </body>
    </html>
  )
}
```

----------------------------------------

TITLE: Deduplicating Data Requests with React `cache`
DESCRIPTION: This snippet shows how to use React's `cache` function to deduplicate database queries. By wrapping the `getPost` function, subsequent calls with the same `id` during a render pass will reuse the cached result, preventing redundant database access and optimizing performance.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/01-getting-started/08-fetching-data.mdx#_snippet_5

LANGUAGE: TypeScript
CODE:
```
import { cache } from 'react'
import { db, posts, eq } from '@/lib/db'

export const getPost = cache(async (id: string) => {
  const post = await db.query.posts.findFirst({
    where: eq(posts.id, parseInt(id)),
  })
})
```

LANGUAGE: JavaScript
CODE:
```
import { cache } from 'react'
import { db, posts, eq } from '@/lib/db'
import { notFound } from 'next/navigation'

export const getPost = cache(async (id) => {
  const post = await db.query.posts.findFirst({
    where: eq(posts.id, parseInt(id)),
  })
})
```

----------------------------------------

TITLE: Generating Static Routes for Locales in Next.js Layout (TSX)
DESCRIPTION: This TypeScript snippet shows how to use `generateStaticParams` in a Next.js App Router root layout to pre-render static routes for a defined set of locales (e.g., 'en-US', 'de'). This optimizes performance by generating pages at build time for each specified locale.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/02-guides/internationalization.mdx#_snippet_10

LANGUAGE: TSX
CODE:
```
export async function generateStaticParams() {
  return [{ lang: 'en-US' }, { lang: 'de' }]
}

export default async function RootLayout({
  children,
  params,
}: Readonly<{
  children: React.ReactNode
  params: Promise<{ lang: 'en-US' | 'de' }>
}>) {
  return (
    <html lang={(await params).lang}>
      <body>{children}</body>
    </html>
  )
}
```

----------------------------------------

TITLE: Securing Next.js API Route with Authentication and Authorization (JavaScript)
DESCRIPTION: This JavaScript snippet demonstrates how to secure a Next.js API Route by implementing both authentication and authorization checks. It first verifies if a user session exists, returning a 401 if not, and then checks if the authenticated user possesses the required 'admin' role before allowing access to the route's core logic.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/02-guides/authentication.mdx#_snippet_46

LANGUAGE: js
CODE:
```
export default async function handler(req, res) {
  const session = await getSession(req)

  // Check if the user is authenticated
  if (!session) {
    res.status(401).json({
      error: 'User is not authenticated',
    })
    return
  }

  // Check if the user has the 'admin' role
  if (session.user.role !== 'admin') {
    res.status(401).json({
      error: 'Unauthorized access: User does not have admin privileges.',
    })
    return
  }

  // Proceed with the route for authorized users
  // ... implementation of the API Route
}
```

----------------------------------------

TITLE: Creating a Web App Manifest in Next.js
DESCRIPTION: This snippet demonstrates how to create a web app manifest file (manifest.ts or manifest.js) in Next.js using the App Router. The manifest defines essential PWA properties such as name, short name, description, start URL, display mode, theme colors, and icons, enabling users to install the PWA to their home screen and providing a native app-like experience.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/02-guides/progressive-web-apps.mdx#_snippet_0

LANGUAGE: TypeScript
CODE:
```
import type { MetadataRoute } from 'next'

export default function manifest(): MetadataRoute.Manifest {
  return {
    name: 'Next.js PWA',
    short_name: 'NextPWA',
    description: 'A Progressive Web App built with Next.js',
    start_url: '/',
    display: 'standalone',
    background_color: '#ffffff',
    theme_color: '#000000',
    icons: [
      {
        src: '/icon-192x192.png',
        sizes: '192x192',
        type: 'image/png',
      },
      {
        src: '/icon-512x512.png',
        sizes: '512x512',
        type: 'image/png',
      }
    ]
  }
}
```

LANGUAGE: JavaScript
CODE:
```
export default function manifest() {
  return {
    name: 'Next.js PWA',
    short_name: 'NextPWA',
    description: 'A Progressive Web App built with Next.js',
    start_url: '/',
    display: 'standalone',
    background_color: '#ffffff',
    theme_color: '#000000',
    icons: [
      {
        src: '/icon-192x192.png',
        sizes: '192x192',
        type: 'image/png',
      },
      {
        src: '/icon-512x512.png',
        sizes: '512x512',
        type: 'image/png',
      }
    ]
  }
}
```

----------------------------------------

TITLE: Accessing Dynamic Route Parameters in Next.js Server Components
DESCRIPTION: This snippet illustrates how to access and use dynamic route parameters (`params`) within an `async` Next.js server component (layout). The `params` prop, which is a promise, is awaited to extract route segments like `team`, enabling the component to display content specific to that parameter.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/05-api-reference/03-file-conventions/layout.mdx#_snippet_17

LANGUAGE: tsx
CODE:
```
export default async function DashboardLayout({
  children,
  params,
}: {
  children: React.ReactNode
  params: Promise<{ team: string }>
}) {
  const { team } = await params

  return (
    <section>
      <header>
        <h1>Welcome to {team}'s Dashboard</h1>
      </header>
      <main>{children}</main>
    </section>
  )
}
```

LANGUAGE: jsx
CODE:
```
export default async function DashboardLayout({ children, params }) {
  const { team } = await params

  return (
    <section>
      <header>
        <h1>Welcome to {team}'s Dashboard</h1>
      </header>
      <main>{children}</main>
    </section>
  )
}
```

----------------------------------------

TITLE: Reading Dynamic Route Parameters in Next.js Client Components
DESCRIPTION: This snippet demonstrates how to access dynamic route parameters (`params`) within a Next.js client component using React's `use` hook. Since client components cannot be `async`, the `use` hook is essential for unwrapping the `params` promise and accessing route segments like `slug`.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/05-api-reference/03-file-conventions/layout.mdx#_snippet_18

LANGUAGE: tsx
CODE:
```
'use client'

import { use } from 'react'

export default function Page({
  params,
}: {
  params: Promise<{ slug: string }>
}) {
  const { slug } = use(params)
}
```

LANGUAGE: js
CODE:
```
'use client'

import { use } from 'react'

export default function Page({ params }) {
  const { slug } = use(params)
}
```

----------------------------------------

TITLE: Proxying API Requests with Next.js Rewrites - TypeScript
DESCRIPTION: This TypeScript example demonstrates how to use Next.js `rewrites` in `next.config.ts` to proxy API requests to a backend server. This replaces the `proxy` field typically used in CRA's `package.json` for forwarding requests.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/02-guides/migrating/from-create-react-app.mdx#_snippet_25

LANGUAGE: ts
CODE:
```
import { NextConfig } from 'next'

const nextConfig: NextConfig = {
  async rewrites() {
    return [
      {
        source: '/api/:path*',
        destination: 'https://your-backend.com/:path*'
      }
    ]
  }
}
```

----------------------------------------

TITLE: Defining Next.js Layout with User Data Fetching
DESCRIPTION: This snippet defines a Next.js asynchronous layout component that fetches user data using `getUser()`. It demonstrates how a layout can retrieve user information, deferring the actual authentication check to the data access layer to ensure checks are performed consistently regardless of navigation.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/02-guides/authentication.mdx#_snippet_38

LANGUAGE: tsx
CODE:
```
export default async function Layout({
  children,
}: {
  children: React.ReactNode;
}) {
  const user = await getUser();

  return (
    // ...
  )
}
```

LANGUAGE: jsx
CODE:
```
export default async function Layout({ children }) {
  const user = await getUser();

  return (
    // ...
  )
}
```

----------------------------------------

TITLE: Time-based Revalidation for Blog Posts in Next.js App Router (JavaScript)
DESCRIPTION: This snippet demonstrates how to implement time-based revalidation for a Next.js page in the App Router using JavaScript. By setting `export const revalidate = 3600`, the page's cache will be invalidated and regenerated in the background every hour on the next visit, ensuring updated blog posts are displayed.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/02-guides/incremental-static-regeneration.mdx#_snippet_3

LANGUAGE: jsx
CODE:
```
export const revalidate = 3600 // invalidate every hour

export default async function Page() {
  const data = await fetch('https://api.vercel.app/blog')
  const posts = await data.json()
  return (
    <main>
      <h1>Blog Posts</h1>
      <ul>
        {posts.map((post) => (
          <li key={post.id}>{post.title}</li>
        ))}
      </ul>
    </main>
  )
}
```

----------------------------------------

TITLE: Reading searchParams and params in Next.js Client Components
DESCRIPTION: This example illustrates how to read `searchParams` and `params` within a Client Component in Next.js. Since Client Components cannot be `async`, React's `use` hook is employed to unwrap the Promises returned by `params` and `searchParams`, enabling access to dynamic route segments and URL query parameters.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/05-api-reference/03-file-conventions/page.mdx#_snippet_5

LANGUAGE: tsx
CODE:
```
'use client'\n\nimport { use } from 'react'\n\nexport default function Page({
  params,
  searchParams,
}: {
  params: Promise<{ slug: string }>
  searchParams: Promise<{ [key: string]: string | string[] | undefined }>
}) {
  const { slug } = use(params)
  const { query } = use(searchParams)
}
```

LANGUAGE: js
CODE:
```
'use client'\n\nimport { use } from 'react'\n\nexport default function Page({ params, searchParams }) {
  const { slug } = use(params)
  const { query } = use(searchParams)
}
```

----------------------------------------

TITLE: Implementing Two-Tier Authorization in Next.js Route Handler
DESCRIPTION: This snippet illustrates a Route Handler with a two-tier security check. It first verifies the user's session for authentication and then checks if the authenticated user possesses the 'admin' role for authorization, returning appropriate HTTP status codes (401 or 403) for unauthorized access.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/02-guides/authentication.mdx#_snippet_41

LANGUAGE: ts
CODE:
```
import { verifySession } from '@/app/lib/dal'

export async function GET() {
  // User authentication and role verification
  const session = await verifySession()

  // Check if the user is authenticated
  if (!session) {
    // User is not authenticated
    return new Response(null, { status: 401 })
  }

  // Check if the user has the 'admin' role
  if (session.user.role !== 'admin') {
    // User is authenticated but does not have the right permissions
    return new Response(null, { status: 403 })
  }

  // Continue for authorized users
}
```

LANGUAGE: js
CODE:
```
import { verifySession } from '@/app/lib/dal'

export async function GET() {
  // User authentication and role verification
  const session = await verifySession()

  // Check if the user is authenticated
  if (!session) {
    // User is not authenticated
    return new Response(null, { status: 401 })
  }

  // Check if the user has the 'admin' role
  if (session.user.role !== 'admin') {
    // User is authenticated but does not have the right permissions
    return new Response(null, { status: 403 })
  }

  // Continue for authorized users
}
```

----------------------------------------

TITLE: Wrapping Third-Party Client-Only Components for Server Component Use in Next.js
DESCRIPTION: This Client Component acts as a simple wrapper for a third-party component (Carousel) that lacks the 'use client' directive but relies on client-only features. By adding 'use client' to this wrapper, the third-party component can now be safely imported and used within Server Components.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/01-getting-started/07-server-and-client-components.mdx#_snippet_9

LANGUAGE: TypeScript
CODE:
```
'use client'

import { Carousel } from 'acme-carousel'

export default Carousel
```

LANGUAGE: JavaScript
CODE:
```
'use client'

import { Carousel } from 'acme-carousel'

export default Carousel
```

----------------------------------------

TITLE: Defining Basic Next.js Middleware with Redirect and Matcher (TSX/JS)
DESCRIPTION: This snippet demonstrates how to define a basic Next.js middleware function that redirects incoming requests using `NextResponse.redirect`. It also shows how to use the `config` object with a `matcher` property to specify the paths where the middleware should execute.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/05-api-reference/03-file-conventions/middleware.mdx#_snippet_0

LANGUAGE: tsx
CODE:
```
import { NextResponse, NextRequest } from 'next/server'

// This function can be marked `async` if using `await` inside
export function middleware(request: NextRequest) {
  return NextResponse.redirect(new URL('/home', request.url))
}

export const config = {
  matcher: '/about/:path*',
}
```

LANGUAGE: js
CODE:
```
import { NextResponse } from 'next/server'

// This function can be marked `async` if using `await` inside
export function middleware(request) {
  return NextResponse.redirect(new URL('/home', request.url))
}

export const config = {
  matcher: '/about/:path*',
}
```

----------------------------------------

TITLE: Installing `@next/third-parties` Library
DESCRIPTION: This command installs the `@next/third-parties` package along with the latest Next.js version, enabling optimized third-party integrations in your project. It's recommended to use the `latest` or `canary` flags as the library is experimental.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/02-guides/third-party-libraries.mdx#_snippet_0

LANGUAGE: bash
CODE:
```
npm install @next/third-parties@latest next@latest
```

----------------------------------------

TITLE: Performing Server-Side Redirects in Next.js Route Handlers
DESCRIPTION: This example demonstrates how to perform a server-side redirect within a Next.js Route Handler using the `redirect` function from `next/navigation`. When this handler is accessed, it will redirect the client to the specified URL.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/03-building-your-application/01-routing/13-route-handlers.mdx#_snippet_11

LANGUAGE: TypeScript
CODE:
```
import { redirect } from 'next/navigation'

export async function GET(request: Request) {
  redirect('https://nextjs.org/')
}
```

LANGUAGE: JavaScript
CODE:
```
import { redirect } from 'next/navigation'

export async function GET(request) {
  redirect('https://nextjs.org/')
}
```

----------------------------------------

TITLE: Running Next.js Development Server with npm
DESCRIPTION: This command executes the `dev` script defined in `package.json` using npm, which typically starts the Next.js development server. This allows developers to run the application locally and see changes in real-time.
SOURCE: https://github.com/vercel/next.js/blob/canary/examples/cms-prepr/README.md#_snippet_7

LANGUAGE: bash
CODE:
```
npm run dev
```

----------------------------------------

TITLE: Using Closures with Next.js Server Actions for Data Snapshots (JavaScript)
DESCRIPTION: This JavaScript React component demonstrates the use of closures in Next.js Server Actions. An inline Server Action `publish` accesses `publishVersion` from its parent scope, capturing data at render time. Next.js encrypts these closed-over variables to prevent sensitive data exposure during client-server communication.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/03-building-your-application/02-data-fetching/03-server-actions-and-mutations.mdx#_snippet_20

LANGUAGE: jsx
CODE:
```
export default async function Page() {
  const publishVersion = await getLatestVersion();

  async function publish() {
    "use server";
    if (publishVersion !== await getLatestVersion()) {
      throw new Error('The version has changed since pressing publish');
    }
    ...
  }

  return (
    <form>
      <button formAction={publish}>Publish</button>
    </form>
  );
}
```

----------------------------------------

TITLE: Defining Static Metadata with `metadata` Object in Next.js (JavaScript)
DESCRIPTION: This snippet shows how to define static metadata by exporting a plain JavaScript object named `metadata` from a `layout.js` or `page.js` file. This approach is used for fixed metadata values that do not require dynamic computation, contributing to better search engine optimization.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/05-api-reference/04-functions/generate-metadata.mdx#_snippet_1

LANGUAGE: jsx
CODE:
```
export const metadata = {
  title: '...',
  description: '...',
}

export default function Page() {}
```

----------------------------------------

TITLE: Using permanentRedirect in a Next.js Server Component
DESCRIPTION: This example illustrates the usage of the `permanentRedirect` function within a Next.js Server Component (`page.js`). It demonstrates a common pattern where a permanent redirect is triggered if a required resource (e.g., team data) is not found, directing the user to an alternative path like `/login`. The function's execution terminates the current rendering process by throwing a `NEXT_REDIRECT` error.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/05-api-reference/04-functions/permanentRedirect.mdx#_snippet_0

LANGUAGE: JSX
CODE:
```
import { permanentRedirect } from 'next/navigation'

async function fetchTeam(id) {
  const res = await fetch('https://...')
  if (!res.ok) return undefined
  return res.json()
}

export default async function Profile({ params }) {
  const { id } = await params
  const team = await fetchTeam(id)
  if (!team) {
    permanentRedirect('/login')
  }

  // ...
}
```

----------------------------------------

TITLE: Defining HTTP Method Handlers in Next.js Route Files
DESCRIPTION: This example illustrates how to define various HTTP method handlers (GET, HEAD, POST, PUT, PATCH, DELETE, OPTIONS) within a Next.js route file. Each function takes an optional `request` object, allowing for custom logic based on the incoming request. Next.js automatically handles the `OPTIONS` method if not explicitly defined.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/05-api-reference/03-file-conventions/route.mdx#_snippet_1

LANGUAGE: typescript
CODE:
```
export async function GET(request: Request) {}

export async function HEAD(request: Request) {}

export async function POST(request: Request) {}

export async function PUT(request: Request) {}

export async function DELETE(request: Request) {}

export async function PATCH(request: Request) {}

// If `OPTIONS` is not defined, Next.js will automatically implement `OPTIONS` and set the appropriate Response `Allow` header depending on the other methods defined in the Route Handler.
export async function OPTIONS(request: Request) {}
```

LANGUAGE: javascript
CODE:
```
export async function GET(request) {}

export async function HEAD(request) {}

export async function POST(request) {}

export async function PUT(request) {}

export async function DELETE(request) {}

export async function PATCH(request) {}

// If `OPTIONS` is not defined, Next.js will automatically implement `OPTIONS` and set the appropriate Response `Allow` header depending on the other methods defined in the Route Handler.
export async function OPTIONS(request) {}
```

----------------------------------------

TITLE: Dynamically Loading External Libraries (Next.js App Router)
DESCRIPTION: This snippet demonstrates client-side dynamic loading of the `fuse.js` library in a Next.js App Router component. The library is imported only after user interaction (typing in a search input), optimizing the initial bundle size and improving performance.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/02-guides/lazy-loading.mdx#_snippet_3

LANGUAGE: jsx
CODE:
```
'use client'

import { useState } from 'react'

const names = ['Tim', 'Joe', 'Bel', 'Lee']

export default function Page() {
  const [results, setResults] = useState()

  return (
    <div>
      <input
        type="text"
        placeholder="Search"
        onChange={async (e) => {
          const { value } = e.currentTarget
          // Dynamically load fuse.js
          const Fuse = (await import('fuse.js')).default
          const fuse = new Fuse(names)

          setResults(fuse.search(value))
        }}
      />
      <pre>Results: {JSON.stringify(results, null, 2)}</pre>
    </div>
  )
}
```

----------------------------------------

TITLE: Accessing Headers After - Next.js JSX (Access in Child)
DESCRIPTION: Demonstrates the recommended fix for accessing headers by moving the `cookies()` call *into* the component that needs the data derived from the headers (`Inbox`), allowing Next.js to handle it correctly within the Suspense boundary.
SOURCE: https://github.com/vercel/next.js/blob/canary/errors/next-prerender-missing-suspense.mdx#_snippet_6

LANGUAGE: jsx
CODE:
```
import { cookies } from 'next/headers'

export async function Inbox() {
  const token = (await cookies()).get('token')
  const email = await getEmail(token)
  return (
    <ul>
      {email.map((e) => (
        <EmailRow key={e.id} />
      ))}
    </ul>
  )
}
```

----------------------------------------

TITLE: Configuring Environment Variables in Next.js
DESCRIPTION: This snippet demonstrates how to define custom environment variables within the `next.config.js` file. Variables defined here, such as `customKey`, will be made available to the client-side JavaScript bundle at build time, allowing them to be accessed via `process.env`.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/05-api-reference/05-config/01-next-config-js/env.mdx#_snippet_0

LANGUAGE: JavaScript
CODE:
```
module.exports = {
  env: {
    customKey: 'my-value',
  },
}
```

----------------------------------------

TITLE: Integrating Dynamic User Component with Suspense (TypeScript)
DESCRIPTION: This snippet demonstrates how to integrate a dynamic component (`User`) into a page that uses Partial Prerendering. By wrapping the `User` component with `Suspense`, the static parts of the page are prerendered, while the dynamic `User` component is streamed, improving initial load performance.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/01-getting-started/12-partial-prerendering.mdx#_snippet_7

LANGUAGE: TSX
CODE:
```
import { Suspense } from 'react'
import { User, AvatarSkeleton } from './user'

export const experimental_ppr = true

export default function Page() {
  return (
    <section>
      <h1>This will be prerendered</h1>
      <Suspense fallback={<AvatarSkeleton />}>
        <User />
      </Suspense>
    </section>
  )
}
```

----------------------------------------

TITLE: Client-Side Navigation with useRouter in Next.js App Router (TypeScript)
DESCRIPTION: Demonstrates how to use the `useRouter` hook from `next/navigation` in a Client Component to programmatically navigate to a different route (`/dashboard`) when a button is clicked. This is suitable for event handlers and requires the 'use client' directive.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/02-guides/redirecting.mdx#_snippet_2

LANGUAGE: tsx
CODE:
```
'use client'

import { useRouter } from 'next/navigation'

export default function Page() {
  const router = useRouter()

  return (
    <button type="button" onClick={() => router.push('/dashboard')}>
      Dashboard
    </button>
  )
}
```

----------------------------------------

TITLE: Installing Project Dependencies using npm
DESCRIPTION: This command installs all required project dependencies listed in `package.json` using npm. It is a crucial step after cloning or initializing the project to ensure all necessary libraries and frameworks, including Knex, are available for the application to run correctly.
SOURCE: https://github.com/vercel/next.js/blob/canary/examples/with-knex/README.md#_snippet_3

LANGUAGE: bash
CODE:
```
npm install
```

----------------------------------------

TITLE: Calling Server Action from Client Component (TypeScript)
DESCRIPTION: This Client Component imports and calls a Server Action defined in a separate module. The 'use client' directive marks this component as a Client Component, enabling interactive client-side functionality while still leveraging server-side mutations.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/03-building-your-application/02-data-fetching/03-server-actions-and-mutations.mdx#_snippet_4

LANGUAGE: TypeScript
CODE:
```
'use client'

import { create } from './actions'

export function Button() {
  return <button onClick={() => create()}>Create</button>
}
```

----------------------------------------

TITLE: Accessing Dynamic Route Parameters (`params`) in Next.js Page
DESCRIPTION: This example illustrates how to access the `params` prop within a Next.js `page` component. The `params` prop is a promise that resolves to an object containing dynamic route segments. It requires `async/await` to destructure and retrieve the parameter values, such as `slug`.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/05-api-reference/03-file-conventions/page.mdx#_snippet_1

LANGUAGE: tsx
CODE:
```
export default async function Page({
  params,
}: {
  params: Promise<{ slug: string }>
}) {
  const { slug } = await params
}
```

LANGUAGE: jsx
CODE:
```
export default async function Page({ params }) {
  const { slug } = await params
}
```

----------------------------------------

TITLE: Implementing Server-Side Form Validation with Zod (JavaScript)
DESCRIPTION: This JavaScript snippet extends a Next.js API Route to include server-side form validation using the Zod library. It defines a Zod schema to validate the request body. The `schema.parse(req.body)` call will throw an error if validation fails, ensuring data integrity before further processing.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/02-pages/02-guides/forms.mdx#_snippet_5

LANGUAGE: js
CODE:
```
import { z } from 'zod'

const schema = z.object({
  // ...
})

export default async function handler(req, res) {
  const parsed = schema.parse(req.body)
  // ...
}
```

----------------------------------------

TITLE: Configuring Next.js Scripts in package.json (JSON)
DESCRIPTION: This JSON snippet defines the essential `scripts` for a Next.js application within its `package.json` file. It includes commands for running the development server (`dev`), building the application for production (`build`), and starting the production server (`start`). These scripts are fundamental for managing the application's lifecycle and are required for Node.js server deployments.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/01-getting-started/14-deploying.mdx#_snippet_0

LANGUAGE: JSON
CODE:
```
{
  "scripts": {
    "dev": "next dev",
    "build": "next build",
    "start": "next start"
  }
}
```

----------------------------------------

TITLE: Implementing Authentication Redirects with Next.js Middleware
DESCRIPTION: This middleware function handles user authentication and redirects based on session status. It defines protected and public routes, decrypts the user session from a cookie, and redirects unauthenticated users to the login page or authenticated users from public routes to the dashboard. It avoids database lookups for performance, relying solely on cookie-based session checks.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/02-guides/authentication.mdx#_snippet_31

LANGUAGE: TypeScript
CODE:
```
import { NextRequest, NextResponse } from 'next/server'
import { decrypt } from '@/app/lib/session'
import { cookies } from 'next/headers'

// 1. Specify protected and public routes
const protectedRoutes = ['/dashboard']
const publicRoutes = ['/login', '/signup', '/']

export default async function middleware(req: NextRequest) {
  // 2. Check if the current route is protected or public
  const path = req.nextUrl.pathname
  const isProtectedRoute = protectedRoutes.includes(path)
  const isPublicRoute = publicRoutes.includes(path)

  // 3. Decrypt the session from the cookie
  const cookie = (await cookies()).get('session')?.value
  const session = await decrypt(cookie)

  // 4. Redirect to /login if the user is not authenticated
  if (isProtectedRoute && !session?.userId) {
    return NextResponse.redirect(new URL('/login', req.nextUrl))
  }

  // 5. Redirect to /dashboard if the user is authenticated
  if (
    isPublicRoute &&
    session?.userId &&
    !req.nextUrl.pathname.startsWith('/dashboard')
  ) {
    return NextResponse.redirect(new URL('/dashboard', req.nextUrl))
  }

  return NextResponse.next()
}

// Routes Middleware should not run on
export const config = {
  matcher: ['/((?!api|_next/static|_next/image|.*\\.png$).*)']
}
```

LANGUAGE: JavaScript
CODE:
```
import { NextResponse } from 'next/server'
import { decrypt } from '@/app/lib/session'
import { cookies } from 'next/headers'

// 1. Specify protected and public routes
const protectedRoutes = ['/dashboard']
const publicRoutes = ['/login', '/signup', '/']

export default async function middleware(req) {
  // 2. Check if the current route is protected or public
  const path = req.nextUrl.pathname
  const isProtectedRoute = protectedRoutes.includes(path)
  const isPublicRoute = publicRoutes.includes(path)

  // 3. Decrypt the session from the cookie
  const cookie = (await cookies()).get('session')?.value
  const session = await decrypt(cookie)

  // 5. Redirect to /login if the user is not authenticated
  if (isProtectedRoute && !session?.userId) {
    return NextResponse.redirect(new URL('/login', req.nextUrl))
  }

  // 6. Redirect to /dashboard if the user is authenticated
  if (
    isPublicRoute &&
    session?.userId &&
    !req.nextUrl.pathname.startsWith('/dashboard')
  ) {
    return NextResponse.redirect(new URL('/dashboard', req.nextUrl))
  }

  return NextResponse.next()
}

// Routes Middleware should not run on
export const config = {
  matcher: ['/((?!api|_next/static|_next/image|.*\\.png$).*)']
}
```

----------------------------------------

TITLE: Encrypting and Decrypting Sessions with Jose in Next.js (TypeScript)
DESCRIPTION: This snippet provides functions to encrypt and decrypt session payloads using the Jose library. It ensures server-side execution with `server-only` and uses `HS256` for signing and verification. The `encrypt` function signs a payload with a 7-day expiration, while `decrypt` verifies a session string, returning the payload or logging an error on failure. It requires `SESSION_SECRET` from environment variables and a `SessionPayload` type definition.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/02-guides/authentication.mdx#_snippet_16

LANGUAGE: tsx
CODE:
```
import 'server-only'
import { SignJWT, jwtVerify } from 'jose'
import { SessionPayload } from '@/app/lib/definitions'

const secretKey = process.env.SESSION_SECRET
const encodedKey = new TextEncoder().encode(secretKey)

export async function encrypt(payload: SessionPayload) {
  return new SignJWT(payload)
    .setProtectedHeader({ alg: 'HS256' })
    .setIssuedAt()
    .setExpirationTime('7d')
    .sign(encodedKey)
}

export async function decrypt(session: string | undefined = '') {
  try {
    const { payload } = await jwtVerify(session, encodedKey, {
      algorithms: ['HS256'],
    })
    return payload
  } catch (error) {
    console.log('Failed to verify session')
  }
}
```

----------------------------------------

TITLE: Triggering On-demand Revalidation with `revalidateTag` in Next.js Server Actions
DESCRIPTION: Shows how to use `revalidateTag` within a Next.js Server Action (or Route Handler) to invalidate cached data associated with a specific tag. Calling `revalidateTag('posts')` clears all cached data previously tagged with 'posts', ensuring fresh data is fetched on subsequent requests.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/02-guides/incremental-static-regeneration.mdx#_snippet_8

LANGUAGE: ts
CODE:
```
'use server'

import { revalidateTag } from 'next/cache'

export async function createPost() {
  // Invalidate all data tagged with 'posts' in the cache
  revalidateTag('posts')
}
```

LANGUAGE: js
CODE:
```
'use server'

import { revalidateTag } from 'next/cache'

export async function createPost() {
  // Invalidate all data tagged with 'posts' in the cache
  revalidateTag('posts')
}
```

----------------------------------------

TITLE: Defining Server Actions to Accept Bound Arguments (Next.js)
DESCRIPTION: This snippet shows the server-side definition of a Server Action (`updateUser`) that is designed to receive additional arguments passed via `Function.prototype.bind` from the client. The first argument (`userId`) is the bound value, followed by the standard `FormData` object, demonstrating how the server function's signature must match the arguments passed.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/02-guides/forms.mdx#_snippet_2

LANGUAGE: TypeScript
CODE:
```
'use server'

export async function updateUser(userId: string, formData: FormData) {}
```

LANGUAGE: JavaScript
CODE:
```
'use server'

export async function updateUser(userId, formData) {}
```

----------------------------------------

TITLE: Rewriting Routes with Next.js Middleware (TypeScript)
DESCRIPTION: This TypeScript middleware function checks for an authentication token in the request cookies. If present, it rewrites the '/dashboard' path to '/auth/dashboard'; otherwise, it rewrites it to '/public/dashboard'. This ensures users are directed to the appropriate dashboard view based on their authentication status, preventing unnecessary fetches.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/05-api-reference/02-components/link.mdx#_snippet_28

LANGUAGE: TypeScript
CODE:
```
import { NextResponse } from 'next/server'

export function middleware(request: Request) {
  const nextUrl = request.nextUrl
  if (nextUrl.pathname === '/dashboard') {
    if (request.cookies.authToken) {
      return NextResponse.rewrite(new URL('/auth/dashboard', request.url))
    } else {
      return NextResponse.rewrite(new URL('/public/dashboard', request.url))
    }
  }
}
```

----------------------------------------

TITLE: Securing Next.js Server Action with Role-Based Authorization
DESCRIPTION: This snippet demonstrates how to implement an authorization check within a Next.js Server Action. It verifies the user's session and role, returning `null` early if the user is not an 'admin', thereby preventing unauthorized mutations and ensuring only privileged users can execute the action.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/02-guides/authentication.mdx#_snippet_40

LANGUAGE: ts
CODE:
```
'use server'
import { verifySession } from '@/app/lib/dal'

export async function serverAction(formData: FormData) {
  const session = await verifySession()
  const userRole = session?.user?.role

  // Return early if user is not authorized to perform the action
  if (userRole !== 'admin') {
    return null
  }

  // Proceed with the action for authorized users
}
```

LANGUAGE: js
CODE:
```
'use server'
import { verifySession } from '@/app/lib/dal'

export async function serverAction() {
  const session = await verifySession()
  const userRole = session.user.role

  // Return early if user is not authorized to perform the action
  if (userRole !== 'admin') {
    return null
  }

  // Proceed with the action for authorized users
}
```

----------------------------------------

TITLE: Configuring Open Graph Metadata with Media in Next.js (JSX)
DESCRIPTION: This snippet demonstrates how to configure comprehensive Open Graph metadata for a Next.js application, including title, description, URL, site name, images (with dimensions and alt text), videos, audio, locale, and type. It's defined within the `metadata` export in `layout.js` or `page.js`.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/05-api-reference/04-functions/generate-metadata.mdx#_snippet_23

LANGUAGE: jsx
CODE:
```
export const metadata = {
  openGraph: {
    title: 'Next.js',
    description: 'The React Framework for the Web',
    url: 'https://nextjs.org',
    siteName: 'Next.js',
    images: [
      {
        url: 'https://nextjs.org/og.png', // Must be an absolute URL
        width: 800,
        height: 600,
      },
      {
        url: 'https://nextjs.org/og-alt.png', // Must be an absolute URL
        width: 1800,
        height: 1600,
        alt: 'My custom alt',
      },
    ],
    videos: [
      {
        url: 'https://nextjs.org/video.mp4', // Must be an absolute URL
        width: 800,
        height: 600,
      },
    ],
    audio: [
      {
        url: 'https://nextjs.org/audio.mp3', // Must be an absolute URL
      },
    ],
    locale: 'en_US',
    type: 'website',
  },
}
```

----------------------------------------

TITLE: Integrating Blog Nav Links into a Next.js Layout (TypeScript)
DESCRIPTION: This server-side Next.js layout component imports and renders the `BlogNavLink` client component. It fetches featured posts asynchronously using `getFeaturedPosts` and maps them to `BlogNavLink` instances, displaying a list of links alongside the main content. This demonstrates how client components can be used within server components.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/05-api-reference/04-functions/use-selected-layout-segment.mdx#_snippet_4

LANGUAGE: TSX
CODE:
```
// Import the Client Component into a parent Layout (Server Component)\nimport { BlogNavLink } from './blog-nav-link'\nimport getFeaturedPosts from './get-featured-posts'\n\nexport default async function Layout({\n  children,\n}: {\n  children: React.ReactNode\n}) {\n  const featuredPosts = await getFeaturedPosts()\n  return (\n    <div\n>      {featuredPosts.map((post) => (\n        <div key={post.id}>\n          <BlogNavLink slug={post.slug}>{post.title}</BlogNavLink>\n        </div>\n      ))}\n      <div\n>{children}</div\n>    </div\n>  )\n}
```

----------------------------------------

TITLE: Defining Static Metadata with TypeScript in Next.js
DESCRIPTION: This example demonstrates how to define static metadata for a Next.js page or layout using a `Metadata` object in TypeScript. By exporting a `metadata` constant, you can set properties like `title` and `description` for SEO purposes, which Next.js then uses to generate the appropriate HTML `<head>` tags.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/01-getting-started/13-metadata-and-og-images.mdx#_snippet_1

LANGUAGE: TypeScript
CODE:
```
import type { Metadata } from 'next'

export const metadata: Metadata = {
  title: 'My Blog',
  description: '...', 
}

export default function Page() {}
```

----------------------------------------

TITLE: Setting Default Revalidation Time with `revalidate` in TSX
DESCRIPTION: This snippet demonstrates how to set the default revalidation time for a Next.js layout, page, or route using the `revalidate` export. It accepts `false` (default, cache indefinitely), `0` (always dynamic), or a `number` (seconds) to control caching behavior. This option does not override `revalidate` values set by individual `fetch` requests.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/05-api-reference/03-file-conventions/route-segment-config.mdx#_snippet_6

LANGUAGE: tsx
CODE:
```
export const revalidate = false
// false | 0 | number
```

----------------------------------------

TITLE: Setting Up Contentful Webhook for Next.js On-Demand Revalidation
DESCRIPTION: This URL is configured as a webhook endpoint in Contentful to trigger on-demand revalidation in a deployed Next.js application. When content is published or updated in Contentful, this endpoint is called, prompting Next.js to revalidate the affected pages, ensuring fresh content without a full redeployment. The `<YOUR_VERCEL_DEPLOYMENT_URL>` placeholder must be replaced with the actual deployment URL.
SOURCE: https://github.com/vercel/next.js/blob/canary/examples/cms-contentful/README.md#_snippet_9

LANGUAGE: HTTP
CODE:
```
https://<YOUR_VERCEL_DEPLOYMENT_URL>/api/revalidate
```

----------------------------------------

TITLE: Implementing Granular Streaming with Suspense in Next.js (TypeScript)
DESCRIPTION: This Next.js page component demonstrates granular content streaming using React's `<Suspense>` boundary. Content outside the `<Suspense>` (e.g., header) is immediately sent to the client, while the `BlogList` component inside the boundary is streamed in, displaying a `BlogListSkeleton` as a fallback. This allows parts of the page to become interactive sooner, improving user experience for data-intensive sections and requires the `dynamicIO` config option to be enabled.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/01-getting-started/08-fetching-data.mdx#_snippet_8

LANGUAGE: tsx
CODE:
```
import { Suspense } from 'react'
import BlogList from '@/components/BlogList'
import BlogListSkeleton from '@/components/BlogListSkeleton'

export default function BlogPage() {
  return (
    <div>
      {/* This content will be sent to the client immediately */}
      <header>
        <h1>Welcome to the Blog</h1>
        <p>Read the latest posts below.</p>
      </header>
      <main>
        {/* Any content wrapped in a <Suspense> boundary will be streamed */}
        <Suspense fallback={<BlogListSkeleton />}>
          <BlogList />
        </Suspense>
      </main>
    </div>
  )
}
```

----------------------------------------

TITLE: Invoking Server Actions with onClick Event Handler in Next.js
DESCRIPTION: This example demonstrates how to invoke a Next.js Server Action (`incrementLike`) using an `onClick` event handler on a button. It uses React's `useState` hook to manage and display the updated like count after the server action completes. This pattern is useful for actions not directly tied to form submissions.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/03-building-your-application/02-data-fetching/03-server-actions-and-mutations.mdx#_snippet_9

LANGUAGE: TypeScript
CODE:
```
'use client'

import { incrementLike } from './actions'
import { useState } from 'react'

export default function LikeButton({ initialLikes }: { initialLikes: number }) {
  const [likes, setLikes] = useState(initialLikes)

  return (
    <>
      <p>Total Likes: {likes}</p>
      <button
        onClick={async () => {
          const updatedLikes = await incrementLike()
          setLikes(updatedLikes)
        }}
      >
        Like
      </button>
    </>
  )
}
```

LANGUAGE: JavaScript
CODE:
```
'use client'

import { incrementLike } from './actions'
import { useState } from 'react'

export default function LikeButton({ initialLikes }) {
  const [likes, setLikes] = useState(initialLikes)

  return (
    <>
      <p>Total Likes: {likes}</p>
      <button
        onClick={async () => {
          const updatedLikes = await incrementLike()
          setLikes(updatedLikes)
        }}
      >
        Like
      </button>
    </>
  )
}
```

----------------------------------------

TITLE: Jest Configuration for Next.js with Babel
DESCRIPTION: Configures Jest for a Next.js project using Babel, including coverage collection, module name mapping for CSS, static assets, and module aliases, test path ignoring, test environment setup, and Babel transformation.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/02-guides/testing/jest.mdx#_snippet_5

LANGUAGE: javascript
CODE:
```
module.exports = {
  collectCoverage: true,
  // on node 14.x coverage provider v8 offers good speed and more or less good report
  coverageProvider: 'v8',
  collectCoverageFrom: [
    '**/*.{js,jsx,ts,tsx}',
    '!**/*.d.ts',
    '!**/node_modules/**',
    '!<rootDir>/out/**',
    '!<rootDir>/.next/**',
    '!<rootDir>/*.config.js',
    '!<rootDir>/coverage/**',
  ],
  moduleNameMapper: {
    // Handle CSS imports (with CSS modules)
    // https://jestjs.io/docs/webpack#mocking-css-modules
    '^.+\.module\.(css|sass|scss)$': 'identity-obj-proxy',

    // Handle CSS imports (without CSS modules)
    '^.+\.(css|sass|scss)$': '<rootDir>/__mocks__/styleMock.js',

    // Handle image imports
    // https://jestjs.io/docs/webpack#handling-static-assets
    '^.+\.(png|jpg|jpeg|gif|webp|avif|ico|bmp|svg)$': `<rootDir>/__mocks__/fileMock.js`,

    // Handle module aliases
    '^@/components/(.*)$': '<rootDir>/components/$1',

    // Handle @next/font
    '@next/font/(.*)': `<rootDir>/__mocks__/nextFontMock.js`,
    // Handle next/font
    'next/font/(.*)': `<rootDir>/__mocks__/nextFontMock.js`,
    // Disable server-only
    'server-only': `<rootDir>/__mocks__/empty.js`,
  },
  // Add more setup options before each test is run
  // setupFilesAfterEnv: ['<rootDir>/jest.setup.js'],
  testPathIgnorePatterns: ['<rootDir>/node_modules/', '<rootDir>/.next/'],
  testEnvironment: 'jsdom',
  transform: {
    // Use babel-jest to transpile tests with the next/babel preset
    // https://jestjs.io/docs/configuration#transform-objectstring-pathtotransformer--pathtotransformer-object
    '^.+\.(js|jsx|ts|tsx)$': ['babel-jest', { presets: ['next/babel'] }],
  },
  transformIgnorePatterns: [
    '/node_modules/',
    '^.+\.module\.(css|sass|scss)$',
  ],
}
```

----------------------------------------

TITLE: Custom Image Loader Function (App Router Client Component) (JavaScript)
DESCRIPTION: This snippet demonstrates defining a custom `loader` function for the `next/image` component within an App Router Client Component. The `imageLoader` function generates the image URL based on `src`, `width`, and `quality` parameters, allowing for integration with external image optimization services. The `'use client'` directive is necessary because the `loader` prop accepts a function.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/05-api-reference/02-components/image.mdx#_snippet_6

LANGUAGE: JavaScript
CODE:
```
'use client'

import Image from 'next/image'

const imageLoader = ({ src, width, quality }) => {
  return `https://example.com/${src}?w=${width}&q=${quality || 75}`
}

export default function Page() {
  return (
    <Image
      loader={imageLoader}
      src="me.png"
      alt="Picture of the author"
      width={500}
      height={500}
    />
  )
}
```

----------------------------------------

TITLE: Revalidating a Specific URL - Next.js Cache - TypeScript
DESCRIPTION: This example demonstrates how to use `revalidatePath` to invalidate the cache for a single, literal URL path. The revalidation occurs on the next visit to the specified URL, ensuring updated content is fetched.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/05-api-reference/04-functions/revalidatePath.mdx#_snippet_1

LANGUAGE: ts
CODE:
```
import { revalidatePath } from 'next/cache'
revalidatePath('/blog/post-1')
```

----------------------------------------

TITLE: Invoking useSearchParams Hook
DESCRIPTION: This snippet illustrates the basic invocation of the `useSearchParams` hook. It highlights that the hook does not accept any parameters and returns a read-only `URLSearchParams` object, which can then be used to access URL query string values.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/05-api-reference/04-functions/use-search-params.mdx#_snippet_1

LANGUAGE: tsx
CODE:
```
const searchParams = useSearchParams()
```

----------------------------------------

TITLE: On-demand Revalidation with `revalidatePath` in Next.js Server Actions (JavaScript)
DESCRIPTION: This Server Action demonstrates on-demand cache invalidation using `revalidatePath` in Next.js. When `createPost` is called (e.g., after adding a new post), `revalidatePath('/posts')` clears the cache for the `/posts` route, prompting the Server Component to fetch fresh data on the next request.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/02-guides/incremental-static-regeneration.mdx#_snippet_5

LANGUAGE: js
CODE:
```
'use server'

import { revalidatePath } from 'next/cache'

export async function createPost() {
  // Invalidate the /posts route in the cache
  revalidatePath('/posts')
}
```

----------------------------------------

TITLE: Enabling React Compiler in Next.js Configuration
DESCRIPTION: This configuration snippet demonstrates how to enable the React Compiler in your Next.js project by setting `experimental.reactCompiler` to `true` within `next.config.js` or `next.config.ts`. This activates the compiler to automatically optimize component rendering across your application.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/05-api-reference/05-config/01-next-config-js/reactCompiler.mdx#_snippet_1

LANGUAGE: ts
CODE:
```
import type { NextConfig } from 'next'

const nextConfig: NextConfig = {
  experimental: {
    reactCompiler: true,
  },
}

export default nextConfig
```

LANGUAGE: js
CODE:
```
/** @type {import('next').NextConfig} */
const nextConfig = {
  experimental: {
    reactCompiler: true,
  },
}

module.exports = nextConfig
```

----------------------------------------

TITLE: Setting Body Size Limit for Next.js Server Actions (JavaScript)
DESCRIPTION: This configuration option allows you to specify the maximum size of the request body for Server Actions. It helps prevent excessive resource consumption and potential DDoS attacks by limiting the amount of data processed. The limit can be expressed in bytes or common string formats like '500kb' or '3mb'.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/05-api-reference/05-config/01-next-config-js/serverActions.mdx#_snippet_1

LANGUAGE: JavaScript
CODE:
```
/** @type {import('next').NextConfig} */

module.exports = {
  experimental: {
    serverActions: {
      bodySizeLimit: '2mb',
    },
  },
}
```

----------------------------------------

TITLE: Generating Static Params for Single Dynamic Product ID (Next.js)
DESCRIPTION: This example illustrates using `generateStaticParams` to statically generate pages for a single dynamic segment, such as product IDs. It returns a predefined array of `id` parameters, enabling Next.js to create separate static pages for each specified ID at build time, like `/product/1`, `/product/2`, and `/product/3`.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/05-api-reference/04-functions/generate-static-params.mdx#_snippet_1

LANGUAGE: tsx
CODE:
```
export function generateStaticParams() {
  return [{ id: '1' }, { id: '2' }, { id: '3' }]
}

// Three versions of this page will be statically generated
// using the `params` returned by `generateStaticParams`
// - /product/1
// - /product/2
// - /product/3
export default async function Page({
  params,
}: {
  params: Promise<{ id: string }>
}) {
  const { id } = await params
  // ...
}
```

LANGUAGE: jsx
CODE:
```
export function generateStaticParams() {
  return [{ id: '1' }, { id: '2' }, { id: '3' }]
}

// Three versions of this page will be statically generated
// using the `params` returned by `generateStaticParams`
// - /product/1
// - /product/2
// - /product/3
export default async function Page({ params }) {
  const { id } = await params
  // ...
}
```

----------------------------------------

TITLE: Running Next.js Development Server with pnpm
DESCRIPTION: This command first installs all project dependencies using pnpm, then starts the Next.js development server. This allows local development and live reloading of the application, typically accessible at `http://localhost:3000`.
SOURCE: https://github.com/vercel/next.js/blob/canary/examples/cms-sanity/README.md#_snippet_13

LANGUAGE: bash
CODE:
```
pnpm install && pnpm dev
```

----------------------------------------

TITLE: Implementing Global Error Handling in Next.js with TypeScript
DESCRIPTION: This snippet demonstrates how to create a `global-error.tsx` file in Next.js to catch errors in the root layout or template. It defines a Client Component that must include `<html>` and `<body>` tags, providing a basic UI with a 'Try again' button to reset the error state. This component replaces the root layout when active.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/05-api-reference/03-file-conventions/error.mdx#_snippet_4

LANGUAGE: TypeScript
CODE:
```
'use client' // Error boundaries must be Client Components

export default function GlobalError({
  error,
  reset,
}: {
  error: Error & { digest?: string }
  reset: () => void
}) {
  return (
    // global-error must include html and body tags
    <html>
      <body>
        <h2>Something went wrong!</h2>
        <button onClick={() => reset()}>Try again</button>
      </body>
    </html>
  )
}
```

----------------------------------------

TITLE: Importing Global CSS in Next.js App Router Root Layout (JavaScript)
DESCRIPTION: This JavaScript React component defines the root layout for a Next.js App Router application. It imports `global.css`, ensuring that the defined global styles are applied to every route. The layout wraps the application's children within an HTML structure.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/01-getting-started/06-css.mdx#_snippet_7

LANGUAGE: javascript
CODE:
```
// These styles apply to every route in the application
import './global.css'

export default function RootLayout({ children }) {
  return (
    <html lang="en">
      <body>{children}</body>
    </html>
  )
}
```

----------------------------------------

TITLE: Setting Image Source with Static Import (src Prop) (JSX)
DESCRIPTION: This example demonstrates importing an image as a module and then passing the imported module directly to the `src` prop. This method automatically handles width and height inference, simplifying image usage.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/05-api-reference/02-components/image.mdx#_snippet_3

LANGUAGE: JSX
CODE:
```
import profile from './profile.png'

export default function Page() {
  return <Image src={profile} />
}
```

----------------------------------------

TITLE: Handling Authorization in Next.js Middleware (After Upgrade - TypeScript)
DESCRIPTION: This 'after' example demonstrates the updated approach for handling authorization in Next.js Middleware. Instead of returning a response body, the Middleware now redirects the user to a login page, passing the original path as a search parameter. This aligns with the new API's focus on `rewrite`/`redirect` actions.
SOURCE: https://github.com/vercel/next.js/blob/canary/errors/middleware-upgrade-guide.mdx#_snippet_4

LANGUAGE: TypeScript
CODE:
```
import { NextResponse } from 'next/server'
import type { NextRequest } from 'next/server'
import { isAuthValid } from './lib/auth'

export function middleware(request: NextRequest) {
  // Example function to validate auth
  if (isAuthValid(request)) {
    return NextResponse.next()
  }

  const loginUrl = new URL('/login', request.url)
  loginUrl.searchParams.set('from', request.nextUrl.pathname)

  return NextResponse.redirect(loginUrl)
}
```

----------------------------------------

TITLE: Updated Next.js Middleware API Usage (TypeScript)
DESCRIPTION: This code illustrates the current and recommended API signature for Next.js Middleware, where the `middleware` function receives a `request` object. It directly returns a `NextResponse` with a 403 status for a specific path, providing the correct and modern way to handle responses.
SOURCE: https://github.com/vercel/next.js/blob/canary/errors/middleware-new-signature.mdx#_snippet_1

LANGUAGE: TypeScript
CODE:
```
import { NextResponse } from 'next/server'

export function middleware(request) {
  if (request.nextUrl.pathname === '/blocked') {
    return new NextResponse(null, {
      status: 403,
    })
  }
}
```

----------------------------------------

TITLE: Refactoring Synchronous `params` and `searchParams` Access in Next.js 15 (After)
DESCRIPTION: This snippet demonstrates the corrected asynchronous access pattern for `params` and `searchParams` required in Next.js 15. The component is now `async`, and `params` and `searchParams` are `await`ed before their values are destructured, ensuring compatibility with the new Promise-based API. The `app/page.js` example shows that the `export * from` pattern remains valid as long as the exported component correctly handles the asynchronous values.
SOURCE: https://github.com/vercel/next.js/blob/canary/errors/next-prerender-sync-params.mdx#_snippet_1

LANGUAGE: jsx
CODE:
```
export default async function ComponentThatWillBeExportedAsPage({ params, searchParams }) {
  const { slug } = await params;
  const { page } = await searchParams
  return <RenderList slug={slug} page={page}>
}
```

LANGUAGE: jsx
CODE:
```
export * from '.../some-file'
```

----------------------------------------

TITLE: Styling Next.js Image Component with CSS Modules
DESCRIPTION: This example demonstrates the recommended way to style the `next/image` component using the `className` prop, typically in conjunction with CSS Modules. This approach promotes modular and scoped styling for your image components.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/05-api-reference/02-components/image.mdx#_snippet_43

LANGUAGE: jsx
CODE:
```
import styles from './styles.module.css'

export default function MyImage() {
  return <Image className={styles.image} src="/my-image.png" alt="My Image" />
}
```

----------------------------------------

TITLE: Setting Up Client-Side Instrumentation in Next.js (TypeScript)
DESCRIPTION: This code snippet demonstrates how to implement client-side instrumentation in a Next.js application using TypeScript. It includes examples for performance monitoring, analytics initialization, and global error tracking, running before the main application code starts. This setup leverages browser APIs like `performance` and `window.addEventListener` to capture metrics and errors early.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/05-api-reference/03-file-conventions/instrumentation-client.mdx#_snippet_0

LANGUAGE: TypeScript
CODE:
```
// Set up performance monitoring
performance.mark('app-init')

// Initialize analytics
console.log('Analytics initialized')

// Set up error tracking
window.addEventListener('error', (event) => {
  // Send to your error tracking service
  reportError(event.error)
})
```

----------------------------------------

TITLE: Implementing Regex Path Parameter Matching in Rewrites (JavaScript)
DESCRIPTION: This snippet demonstrates how to apply regular expressions to path parameters in the `source` for more precise matching. For instance, `:post(\d{1,})` ensures that only numeric values are matched for the `post` parameter, allowing `/old-blog/123` to rewrite to `/blog/123` but not `/old-blog/abc`.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/05-api-reference/05-config/01-next-config-js/rewrites.mdx#_snippet_7

LANGUAGE: JavaScript
CODE:
```
module.exports = {
  async rewrites() {
    return [
      {
        source: '/old-blog/:post(\d{1,})',
        destination: '/blog/:post', // Matched parameters can be used in the destination
      },
    ]
  },
}
```

----------------------------------------

TITLE: Creating a Gracefully Degrading Error Boundary in Next.js with TypeScript
DESCRIPTION: This TypeScript snippet defines a `GracefullyDegradingErrorBoundary` component for Next.js, designed to capture and preserve the current HTML content before a client-side rendering error. If an error occurs, it re-renders the captured HTML using `dangerouslySetInnerHTML` and displays a persistent notification, providing a smoother user experience by showing the last known good UI.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/05-api-reference/03-file-conventions/error.mdx#_snippet_6

LANGUAGE: TypeScript
CODE:
```
'use client'

import React, { Component, ErrorInfo, ReactNode } from 'react'

interface ErrorBoundaryProps {
  children: ReactNode
  onError?: (error: Error, errorInfo: ErrorInfo) => void
}

interface ErrorBoundaryState {
  hasError: boolean
}

export class GracefullyDegradingErrorBoundary extends Component<
  ErrorBoundaryProps,
  ErrorBoundaryState
> {
  private contentRef: React.RefObject<HTMLDivElement>

  constructor(props: ErrorBoundaryProps) {
    super(props)
    this.state = { hasError: false }
    this.contentRef = React.createRef()
  }

  static getDerivedStateFromError(_: Error): ErrorBoundaryState {
    return { hasError: true }
  }

  componentDidCatch(error: Error, errorInfo: ErrorInfo) {
    if (this.props.onError) {
      this.props.onError(error, errorInfo)
    }
  }

  render() {
    if (this.state.hasError) {
      // Render the current HTML content without hydration
      return (
        <>
          <div
            ref={this.contentRef}
            suppressHydrationWarning
            dangerouslySetInnerHTML={{
              __html: this.contentRef.current?.innerHTML || '',
            }}
          />
          <div className="fixed bottom-0 left-0 right-0 bg-red-600 text-white py-4 px-6 text-center">
            <p className="font-semibold">
              An error occurred during page rendering
            </p>
          </div>
        </>
      )
    }

    return <div ref={this.contentRef}>{this.props.children}</div>
  }
}

export default GracefullyDegradingErrorBoundary
```

----------------------------------------

TITLE: Specifying paths Array in getStaticPaths Return Object (JavaScript)
DESCRIPTION: This example details the structure of the `paths` array that `getStaticPaths` must return. Each object in the array defines a path to be pre-rendered, including `params` that match dynamic route segments and an optional `locale` for internationalized routes. The `params` values are case-sensitive.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/02-pages/04-api-reference/03-functions/get-static-paths.mdx#_snippet_2

LANGUAGE: js
CODE:
```
return {
  paths: [
    { params: { id: '1' }},
    {
      params: { id: '2' },
      // with i18n configured the locale for the path can be returned as well
      locale: "en",
    },
  ],
  fallback: ...
}
```

----------------------------------------

TITLE: Configuring Image Preload Priority in Next.js
DESCRIPTION: The `priority` boolean prop for the Next.js `Image` component indicates if the image should be preloaded, disabling lazy loading. Set to `true` for above-the-fold or Largest Contentful Paint (LCP) images to improve initial page loading performance. Default is `false`, enabling lazy loading.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/05-api-reference/02-components/image.mdx#_snippet_11

LANGUAGE: JSX
CODE:
```
<Image priority={false} />
```

----------------------------------------

TITLE: Configuring Incremental Partial Prerendering in Next.js
DESCRIPTION: This configuration snippet demonstrates how to enable incremental Partial Prerendering (PPR) across a Next.js application. By setting `experimental.ppr` to 'incremental' in `next.config.js`, the application is configured to leverage PPR for routes that explicitly opt-in. This is a global setting that applies to the entire Next.js project.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/05-api-reference/05-config/01-next-config-js/ppr.mdx#_snippet_0

LANGUAGE: TypeScript
CODE:
```
import type { NextConfig } from 'next'

const nextConfig: NextConfig = {
  experimental: {
    ppr: 'incremental',
  },
}

export default nextConfig
```

LANGUAGE: JavaScript
CODE:
```
/** @type {import('next').NextConfig} */
const nextConfig = {
  experimental: {
    ppr: 'incremental',
  },
}

module.exports = nextConfig
```

----------------------------------------

TITLE: Reading Runtime Environment Variables in Next.js App Router (TypeScript)
DESCRIPTION: This snippet demonstrates how to safely read environment variables on the server during dynamic rendering within the Next.js App Router. By using `connection()` or other Dynamic APIs (like `cookies`, `headers`), the page opts into dynamic rendering, ensuring that `process.env.MY_VALUE` is evaluated at runtime rather than build time. This allows for a singular Docker image to be promoted across environments with different variable values.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/02-guides/self-hosting.mdx#_snippet_0

LANGUAGE: tsx
CODE:
```
import { connection } from 'next/server'

export default async function Component() {
  await connection()
  // cookies, headers, and other Dynamic APIs
  // will also opt into dynamic rendering, meaning
  // this env variable is evaluated at runtime
  const value = process.env.MY_VALUE
  // ...
}
```

----------------------------------------

TITLE: Linking to Dynamic App Router Segments in Next.js
DESCRIPTION: This example illustrates how to create links to dynamic route segments using the Next.js `Link` component within the App Router. It leverages template literals to construct the `href` for paths like `/blog/[slug]/page.js`, enabling navigation to dynamically generated content. This is crucial for applications utilizing the App Router's file-system based routing for variable content.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/05-api-reference/02-components/link.mdx#_snippet_16

LANGUAGE: tsx
CODE:
```
import Link from 'next/link'

export default function Page({ posts }) {
  return (
    <ul>
      {posts.map((post) => (
        <li key={post.id}>
          <Link href={`/blog/${post.slug}`}>{post.title}</Link>
        </li>
      ))}
    </ul>
  )
}
```

LANGUAGE: jsx
CODE:
```
import Link from 'next/link'

export default function Page({ posts }) {
  return (
    <ul>
      {posts.map((post) => (
        <li key={post.id}>
          <Link href={`/blog/${post.slug}`}>{post.title}</Link>
        </li>
      ))}
    </ul>
  )
}
```

----------------------------------------

TITLE: Passing URL Object to Link Component (Pages Router - JavaScript)
DESCRIPTION: Shows how to provide a URL object to the `href` prop of the `next/link` component in the Pages Router. This method simplifies creating URLs with query parameters or dynamic route segments, as Next.js handles the string formatting. The example demonstrates linking to a static route with query parameters and a dynamic route.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/05-api-reference/02-components/link.mdx#_snippet_24

LANGUAGE: jsx
CODE:
```
import Link from 'next/link'

function Home() {
  return (
    <ul>
      <li>
        <Link
          href={{
            pathname: '/about',
            query: { name: 'test' }
          }}
        >
          About us
        </Link>
      </li>
      <li>
        <Link
          href={{
            pathname: '/blog/[slug]',
            query: { slug: 'my-post' }
          }}
        >
          Blog Post
        </Link>
      </li>
    </ul>
  )
}

export default Home
```

----------------------------------------

TITLE: Inheriting Nested Metadata in Next.js Page (JSX)
DESCRIPTION: This snippet in `app/about/page.js` demonstrates metadata inheritance. While the `title` is explicitly set and overwrites the parent, the `openGraph` fields are fully inherited from `app/layout.js` because `app/about/page.js` does not define its own `openGraph` object.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/05-api-reference/04-functions/generate-metadata.mdx#_snippet_87

LANGUAGE: jsx
CODE:
```
export const metadata = {
  title: 'About',
}

// Output:
// <title>About</title>
// <meta property="og:title" content="Acme" />
// <meta property="og:description" content="Acme is a..." />
```

----------------------------------------

TITLE: Fetching and Rendering Remote MDX with App Router (JavaScript)
DESCRIPTION: This snippet demonstrates how to fetch remote MDX content and render it using `MDXRemote` from `next-mdx-remote-client/rsc` within the Next.js App Router. It performs an asynchronous fetch request to retrieve markdown text, which is then passed to the `MDXRemote` component for rendering. This approach is suitable for dynamic content from external sources like CMS or databases.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/02-guides/mdx.mdx#_snippet_37

LANGUAGE: JavaScript
CODE:
```
import { MDXRemote } from 'next-mdx-remote-client/rsc'

export default async function RemoteMdxPage() {
  // MDX text - can be from a database, CMS, fetch, anywhere...
  const res = await fetch('https://...')
  const markdown = await res.text()
  return <MDXRemote source={markdown} />
}
```

----------------------------------------

TITLE: Configuring VS Code for Next.js Debugging (launch.json)
DESCRIPTION: This JSON configuration file (`.vscode/launch.json`) sets up various debugging profiles for a Next.js application within VS Code. It includes configurations for debugging the server-side (Node.js), client-side (Chrome/Firefox), and a combined full-stack debugging session. Parameters like `command`, `url`, `program`, and `serverReadyAction` are used to define how the debugger attaches and launches the application. It requires the Firefox Debugger extension for Firefox configurations.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/02-guides/debugging.mdx#_snippet_0

LANGUAGE: JSON
CODE:
```
{
  "version": "0.2.0",
  "configurations": [
    {
      "name": "Next.js: debug server-side",
      "type": "node-terminal",
      "request": "launch",
      "command": "npm run dev"
    },
    {
      "name": "Next.js: debug client-side",
      "type": "chrome",
      "request": "launch",
      "url": "http://localhost:3000"
    },
    {
      "name": "Next.js: debug client-side (Firefox)",
      "type": "firefox",
      "request": "launch",
      "url": "http://localhost:3000",
      "reAttach": true,
      "pathMappings": [
        {
          "url": "webpack://_N_E",
          "path": "${workspaceFolder}"
        }
      ]
    },
    {
      "name": "Next.js: debug full stack",
      "type": "node",
      "request": "launch",
      "program": "${workspaceFolder}/node_modules/next/dist/bin/next",
      "runtimeArgs": ["--inspect"],
      "skipFiles": ["<node_internals>/**"],
      "serverReadyAction": {
        "action": "debugWithEdge",
        "killOnServerStop": true,
        "pattern": "- Local:.+(https?://.+)",
        "uriFormat": "%s",
        "webRoot": "${workspaceFolder}"
      }
    }
  ]
}
```

----------------------------------------

TITLE: Forcing Static Rendering for All Paths at Runtime using dynamic (Next.js JSX)
DESCRIPTION: This snippet shows an alternative method to ensure all paths for a dynamic route are statically rendered at runtime. Setting `export const dynamic = 'force-static'` explicitly tells Next.js to render the page as static, even if it contains dynamic functions. This is suitable when you want to force static behavior for a route segment.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/05-api-reference/04-functions/generate-static-params.mdx#_snippet_8

LANGUAGE: jsx
CODE:
```
export const dynamic = 'force-static'
```

----------------------------------------

TITLE: Copying Environment Variable Example File
DESCRIPTION: This command copies the `.env.local.example` file to `.env.local`. The `.env.local` file is used to store local environment variables and is typically ignored by Git, ensuring sensitive information is not committed to version control.
SOURCE: https://github.com/vercel/next.js/blob/canary/examples/cms-prepr/README.md#_snippet_3

LANGUAGE: bash
CODE:
```
cp .env.local.example .env.local
```

----------------------------------------

TITLE: Server Action for Redirection
DESCRIPTION: Defines a Server Action that uses the `redirect` function. This action is designed to be called from a Client Component, typically via a form submission, to perform a server-side redirect based on input data.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/05-api-reference/04-functions/redirect.mdx#_snippet_4

LANGUAGE: ts
CODE:
```
'use server'

import { redirect } from 'next/navigation'

export async function navigate(data: FormData) {
  redirect(`/posts/${data.get('id')}`)
}
```

LANGUAGE: js
CODE:
```
'use server'

import { redirect } from 'next/navigation'

export async function navigate(data) {
  redirect(`/posts/${data.get('id')}`)
}
```

----------------------------------------

TITLE: Configuring Fetch Revalidation Period in Next.js (JSX)
DESCRIPTION: This snippet demonstrates how to set a revalidation period for an individual `fetch` request using the `next.revalidate` option. This ensures that the data fetched will be revalidated from the cache at most after the specified number of seconds (e.g., 3600 seconds for 1 hour), leading to fresh data and server-side re-rendering.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/04-deep-dive/caching.mdx#_snippet_7

LANGUAGE: JSX
CODE:
```
fetch(`https://...`, { next: { revalidate: 3600 } })
```

----------------------------------------

TITLE: Defining Module-Level Server Action for Client Components (JavaScript)
DESCRIPTION: This snippet defines a Server Action at the module level by placing the 'use server' directive at the top of the file. This allows all exported functions within the file to be marked as Server Actions, making them reusable in both Client and Server Components.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/03-building-your-application/02-data-fetching/03-server-actions-and-mutations.mdx#_snippet_3

LANGUAGE: JavaScript
CODE:
```
'use server'

export async function create() {}
```

----------------------------------------

TITLE: Sending Web Vitals to External Analytics Endpoint (JS)
DESCRIPTION: Provides an example of using the `useReportWebVitals` hook to send collected metric data as a JSON string to a specified external analytics URL, prioritizing `navigator.sendBeacon` and falling back to `fetch` with `keepalive`.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/05-api-reference/04-functions/use-report-web-vitals.mdx#_snippet_3

LANGUAGE: js
CODE:
```
useReportWebVitals((metric) => {
  const body = JSON.stringify(metric)
  const url = 'https://example.com/analytics'

  // Use `navigator.sendBeacon()` if available, falling back to `fetch()`.
  if (navigator.sendBeacon) {
    navigator.sendBeacon(url, body)
  } else {
    fetch(url, { body, method: 'POST', keepalive: true })
  }
})
```

----------------------------------------

TITLE: Setting a Public Environment Variable in Terminal
DESCRIPTION: This snippet demonstrates how to define a public environment variable, `NEXT_PUBLIC_ANALYTICS_ID`, in the terminal. Variables prefixed with `NEXT_PUBLIC_` are inlined into the JavaScript bundle at build time, making them accessible in the browser.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/02-guides/environment-variables.mdx#_snippet_10

LANGUAGE: Shell
CODE:
```
NEXT_PUBLIC_ANALYTICS_ID=abcdefghijk
```

----------------------------------------

TITLE: Incorrect Client-Side Usage of Node.js Module in React
DESCRIPTION: This snippet demonstrates an incorrect attempt to use a Node.js-specific module (ioredis) directly within a React component's `useEffect` hook. This will result in a 'Module Not Found' error when the code runs in the browser, as Node.js modules are not available client-side.
SOURCE: https://github.com/vercel/next.js/blob/canary/errors/module-not-found.mdx#_snippet_6

LANGUAGE: jsx
CODE:
```
// Redis is a Node.js specific library that can't run in the browser
// Trying to use it in code that runs on both Node.js and the browser will result in a module not found error for modules that ioredis relies on
// If you run into such an error it's recommended to move the code to `getStaticProps` or `getServerSideProps` as those methods guarantee that the code is only run in Node.js.
import redis from '../lib/redis'
import { useEffect, useState } from 'react'

export default function Home() {
  const [message, setMessage] = useState()
  useEffect(() => {
    redis.get('message').then((result) => {
      setMessage(result)
    })
  }, [])
  return <h1>{message}</h1>
}
```

----------------------------------------

TITLE: Integrating Session Creation in Next.js Server Action (JavaScript)
DESCRIPTION: This JavaScript Server Action `signup` demonstrates how to integrate the `createSession` function after user authentication. It creates a user session using the `user.id` and then redirects the user to the `/profile` page using Next.js's `redirect()` API, typically after form validation and database insertion.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/02-guides/authentication.mdx#_snippet_21

LANGUAGE: JavaScript
CODE:
```
import { createSession } from '@/app/lib/session'

export async function signup(state, formData) {
  // Previous steps:
  // 1. Validate form fields
  // 2. Prepare data for insertion into database
  // 3. Insert the user into the database or call an Library API

  // Current steps:
  // 4. Create user session
  await createSession(user.id)
  // 5. Redirect user
  redirect('/profile')
}
```

----------------------------------------

TITLE: Setting Absolute Page Title in Next.js Page (JSX)
DESCRIPTION: This snippet demonstrates how to use `title.absolute` in a Next.js page to define a title that overrides and ignores any `title.template` set in parent segments, resulting in an exact title.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/05-api-reference/04-functions/generate-metadata.mdx#_snippet_14

LANGUAGE: jsx
CODE:
```
export const metadata = {
  title: {
    absolute: 'About',
  },
}

// Output: <title>About</title>
```

----------------------------------------

TITLE: Defining Page Loading UI with loading.tsx in Next.js
DESCRIPTION: This component defines the loading UI for a Next.js route segment. When placed as `loading.tsx` in a folder, it automatically wraps the corresponding `page.js` and its children in a `<Suspense>` boundary, displaying this fallback while the main page content is rendered. This improves perceived performance by showing immediate feedback to the user and requires the `dynamicIO` config option to be enabled.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/01-getting-started/08-fetching-data.mdx#_snippet_6

LANGUAGE: tsx
CODE:
```
export default function Loading() {
  // Define the Loading UI here
  return <div>Loading...</div>
}
```

----------------------------------------

TITLE: Defining Inline Server Action in Server Component (TypeScript)
DESCRIPTION: This snippet demonstrates how to define an inline Server Action within a Next.js Server Component by placing the 'use server' directive inside an async function. This pattern is suitable for actions that are specific to a single component and do not need to be reused elsewhere.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/03-building-your-application/02-data-fetching/03-server-actions-and-mutations.mdx#_snippet_0

LANGUAGE: TypeScript
CODE:
```
export default function Page() {
  // Server Action
  async function create() {
    'use server'
    // Mutate data
  }

  return '...'
}
```

----------------------------------------

TITLE: Accessing Router Instance with useRouter Hook in Next.js (JSX)
DESCRIPTION: This snippet demonstrates how to use the `useRouter` hook within a React function component in Next.js to access the router instance. It creates an `ActiveLink` component that dynamically styles a link based on the current `router.asPath` and handles client-side navigation using `router.push(href)` on click. This hook is essential for implementing routing logic in functional components and cannot be used with class components.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/02-pages/04-api-reference/03-functions/use-router.mdx#_snippet_0

LANGUAGE: JSX
CODE:
```
import { useRouter } from 'next/router'

function ActiveLink({ children, href }) {
  const router = useRouter()
  const style = {
    marginRight: 10,
    color: router.asPath === href ? 'red' : 'black',
  }

  const handleClick = (e) => {
    e.preventDefault()
    router.push(href)
  }

  return (
    <a href={href} onClick={handleClick} style={style}>
      {children}
    </a>
  )
}

export default ActiveLink
```

----------------------------------------

TITLE: Using Next.js Layout Params Synchronously (TypeScript)
DESCRIPTION: This snippet shows how to use `params` synchronously in Next.js 15 layouts using the `use` hook from React. This allows for a synchronous component structure while still handling the asynchronous `params` prop.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/02-guides/upgrading/version-15.mdx#_snippet_13

LANGUAGE: typescript
CODE:
```
// Before
type Params = { slug: string }

export default function Layout({
  children,
  params,
}: {
  children: React.ReactNode
  params: Params
}) {
  const { slug } = params
}

// After
import { use } from 'react'

type Params = Promise<{ slug: string }>

export default function Layout(props: {
  children: React.ReactNode
  params: Params
}) {
  const params = use(props.params)
  const slug = params.slug
}
```

----------------------------------------

TITLE: Dynamic File System Access (JavaScript)
DESCRIPTION: This snippet demonstrates dynamic file system access using `fs.readFileSync(unknown)`. Reading files with a dynamically determined path can lead to 'path traversal' vulnerabilities if the `unknown` variable is not properly sanitized, allowing access to arbitrary files on the system.
SOURCE: https://github.com/vercel/next.js/blob/canary/turbopack/crates/turbopack-tests/tests/snapshot/dynamic-request/very-dynamic/issues/__l___lint TP1002__ require(FreeVar(Math)[__quo__r-0da151.txt#_snippet_3

LANGUAGE: JavaScript
CODE:
```
fs.readFileSync(unknown)
```

----------------------------------------

TITLE: Configuring Next.js Build Cache in Azure Pipelines
DESCRIPTION: This YAML task for Azure Pipelines uses `Cache@2` to cache the `.next/cache` directory. The cache key is generated based on the OS and `yarn.lock`, ensuring efficient caching of Next.js build artifacts before the `next build` command.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/02-guides/ci-build-caching.mdx#_snippet_8

LANGUAGE: yaml
CODE:
```
- task: Cache@2
  displayName: 'Cache .next/cache'
  inputs:
    key: next | $(Agent.OS) | yarn.lock
    path: '$(System.DefaultWorkingDirectory)/.next/cache'
```

----------------------------------------

TITLE: Enabling Caching for `GET` Route Handlers (JS)
DESCRIPTION: This example shows how to enable caching for `GET` methods in Route Handlers, which are no longer cached by default in Next.js 15. By setting `export const dynamic = 'force-static'`, the Route Handler's `GET` function will be cached.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/02-guides/upgrading/version-15.mdx#_snippet_22

LANGUAGE: js
CODE:
```
export const dynamic = 'force-static'

export async function GET() {}
```

----------------------------------------

TITLE: revalidatePath Function Signature - Next.js - TypeScript
DESCRIPTION: This snippet defines the TypeScript signature for the `revalidatePath` function. It accepts a `path` string to identify the data to revalidate and an optional `type` parameter ('page' or 'layout') to specify the revalidation scope, particularly for dynamic segments. The function does not return any value.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/05-api-reference/04-functions/revalidatePath.mdx#_snippet_0

LANGUAGE: tsx
CODE:
```
revalidatePath(path: string, type?: 'page' | 'layout'): void;
```

----------------------------------------

TITLE: Migrating Next.js Page Params & SearchParams to Async (TypeScript)
DESCRIPTION: This snippet shows how to update `params` and `searchParams` in Next.js `page.js` to be asynchronous in Next.js 15. Both props are now Promises, requiring `await` to access their properties in `generateMetadata` and the default export function.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/02-guides/upgrading/version-15.mdx#_snippet_15

LANGUAGE: typescript
CODE:
```
// Before
type Params = { slug: string }
type SearchParams = { [key: string]: string | string[] | undefined }

export function generateMetadata({
  params,
  searchParams,
}: {
  params: Params
  searchParams: SearchParams
}) {
  const { slug } = params
  const { query } = searchParams
}

export default async function Page({
  params,
  searchParams,
}: {
  params: Params
  searchParams: SearchParams
}) {
  const { slug } = params
  const { query } = searchParams
}

// After
type Params = Promise<{ slug: string }>
type SearchParams = Promise<{ [key: string]: string | string[] | undefined }>

export async function generateMetadata(props: {
  params: Params
  searchParams: SearchParams
}) {
  const params = await props.params
  const searchParams = await props.searchParams
  const slug = params.slug
  const query = searchParams.query
}

export default async function Page(props: {
  params: Params
  searchParams: SearchParams
}) {
  const params = await props.params
  const searchParams = await props.searchParams
  const slug = params.slug
  const query = searchParams.query
}
```

----------------------------------------

TITLE: Implementing React Context Provider as Client Component in Next.js
DESCRIPTION: This Client Component, marked with 'use client', creates a React context (ThemeContext) and provides a ThemeProvider that wraps its children. This is necessary because React context is not directly supported in Server Components, enabling global state sharing (e.g., theme) across client components.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/01-getting-started/07-server-and-client-components.mdx#_snippet_6

LANGUAGE: TypeScript
CODE:
```
'use client'

import { createContext } from 'react'

export const ThemeContext = createContext({})

export default function ThemeProvider({
  children,
}: {
  children: React.ReactNode
}) {
  return <ThemeContext.Provider value="dark">{children}</ThemeContext.Provider>
}
```

LANGUAGE: JavaScript
CODE:
```
'use client'

import { createContext } from 'react'

export const ThemeContext = createContext({})

export default function ThemeProvider({ children }) {
  return <ThemeContext.Provider value="dark">{children}</ThemeContext.Provider>
}
```

----------------------------------------

TITLE: Creating a Client Component in Next.js App Directory (TypeScript)
DESCRIPTION: Defines a Client Component using the `'use client'` directive, similar to components in the `pages` directory. It receives data as props, manages state and effects, and is prerendered on the server during initial page load. This component is designed to be imported into a Server Component.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/02-guides/migrating/app-router-migration.mdx#_snippet_17

LANGUAGE: tsx
CODE:
```
'use client'

// This is a Client Component (same as components in the `pages` directory)
// It receives data as props, has access to state and effects, and is
// prerendered on the server during the initial page load.
export default function HomePage({ recentPosts }) {
  return (
    <div>
      {recentPosts.map((post) => (
        <div key={post.id}>{post.title}</div>
      ))}
    </div>
  )
}
```

----------------------------------------

TITLE: Migrating Next.js Headers API to Async (TypeScript)
DESCRIPTION: This snippet illustrates the recommended asynchronous pattern for using the `headers` API in Next.js 15. The `headers()` function now returns a Promise, requiring `await` for proper access.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/02-guides/upgrading/version-15.mdx#_snippet_5

LANGUAGE: typescript
CODE:
```
import { headers } from 'next/headers'

// Before
const headersList = headers()
const userAgent = headersList.get('user-agent')

// After
const headersList = await headers()
const userAgent = headersList.get('user-agent')
```

----------------------------------------

TITLE: Integrating Session Creation in Next.js Server Action (TypeScript)
DESCRIPTION: This TypeScript Server Action `signup` demonstrates how to integrate the `createSession` function after user authentication. It creates a user session using the `user.id` and then redirects the user to the `/profile` page using Next.js's `redirect()` API, typically after form validation and database insertion.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/02-guides/authentication.mdx#_snippet_20

LANGUAGE: TypeScript
CODE:
```
import { createSession } from '@/app/lib/session'

export async function signup(state: FormState, formData: FormData) {
  // Previous steps:
  // 1. Validate form fields
  // 2. Prepare data for insertion into database
  // 3. Insert the user into the database or call an Library API

  // Current steps:
  // 4. Create user session
  await createSession(user.id)
  // 5. Redirect user
  redirect('/profile')
}
```

----------------------------------------

TITLE: Implementing URLPattern for Page Matching in Next.js Middleware
DESCRIPTION: This section illustrates the shift from `request.page` for page matching to using the web standard `URLPattern` API. The 'Before' snippet relies on `event.request.page.params`, while the 'After' snippet provides a custom `params` helper function that leverages `URLPattern` to extract route parameters, offering more accurate and standard-compliant matching.
SOURCE: https://github.com/vercel/next.js/blob/canary/errors/middleware-upgrade-guide.mdx#_snippet_8

LANGUAGE: TypeScript
CODE:
```
import { NextResponse } from 'next/server'
import type { NextRequest, NextFetchEvent } from 'next/server'

export function middleware(request: NextRequest, event: NextFetchEvent) {
  const { params } = event.request.page
  const { locale, slug } = params

  if (locale && slug) {
    const { search, protocol, host } = request.nextUrl
    const url = new URL(`${protocol}//${locale}.${host}/${slug}${search}`)
    return NextResponse.redirect(url)
  }
}
```

LANGUAGE: TypeScript
CODE:
```
import { NextResponse } from 'next/server'
import type { NextRequest } from 'next/server'

const PATTERNS = [
  [
    new URLPattern({ pathname: '/:locale/:slug' }),
    ({ pathname }) => pathname.groups,
  ],
]

const params = (url) => {
  const input = url.split('?')[0]
  let result = {}

  for (const [pattern, handler] of PATTERNS) {
    const patternResult = pattern.exec(input)
    if (patternResult !== null && 'pathname' in patternResult) {
      result = handler(patternResult)
      break
    }
  }
  return result
}

export function middleware(request: NextRequest) {
  const { locale, slug } = params(request.url)

  if (locale && slug) {
    const { search, protocol, host } = request.nextUrl
    const url = new URL(`${protocol}//${locale}.${host}/${slug}${search}`)
    return NextResponse.redirect(url)
  }
}
```

----------------------------------------

TITLE: Implementing ISR with App Router in Next.js
DESCRIPTION: This snippet demonstrates Incremental Static Regeneration using the Next.js App Router. It defines `revalidate` to update cached pages every 60 seconds, `generateStaticParams` to pre-render known blog post IDs at build time, and `dynamicParams` to control behavior for unknown paths. The `Page` component fetches and displays individual post content.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/02-guides/incremental-static-regeneration.mdx#_snippet_0

LANGUAGE: TypeScript
CODE:
```
interface Post {
  id: string
  title: string
  content: string
}

// Next.js will invalidate the cache when a
// request comes in, at most once every 60 seconds.
export const revalidate = 60

// We'll prerender only the params from `generateStaticParams` at build time.
// If a request comes in for a path that hasn't been generated,
// Next.js will server-render the page on-demand.
export const dynamicParams = true // or false, to 404 on unknown paths

export async function generateStaticParams() {
  const posts: Post[] = await fetch('https://api.vercel.app/blog').then((res) =>
    res.json()
  )
  return posts.map((post) => ({
    id: String(post.id),
  }))
}

export default async function Page({
  params,
}: {
  params: Promise<{ id: string }>
}) {
  const { id } = await params
  const post: Post = await fetch(`https://api.vercel.app/blog/${id}`).then(
    (res) => res.json()
  )
  return (
    <main>
      <h1>{post.title}</h1>
      <p>{post.content}</p>
    </main>
  )
}
```

LANGUAGE: JavaScript
CODE:
```
// Next.js will invalidate the cache when a
// request comes in, at most once every 60 seconds.
export const revalidate = 60

// We'll prerender only the params from `generateStaticParams` at build time.
// If a request comes in for a path that hasn't been generated,
// Next.js will server-render the page on-demand.
export const dynamicParams = true // or false, to 404 on unknown paths

export async function generateStaticParams() {
  const posts = await fetch('https://api.vercel.app/blog').then((res) =>
    res.json()
  )
  return posts.map((post) => ({
    id: String(post.id),
  }))
}

export default async function Page({ params }) {
  const { id } = await params
  const post = await fetch(`https://api.vercel.app/blog/${id}`).then(
    (res) => res.json()
  )
  return (
    <main>
      <h1>{post.title}</h1>
      <p>{post.content}</p>
    </main>
  )
}
```

----------------------------------------

TITLE: Returning Direct Responses from Next.js Middleware
DESCRIPTION: This example illustrates how to directly return an HTTP response from Next.js Middleware, effectively short-circuiting the request. It checks for authentication using an external `isAuthenticated` function and returns a 401 Unauthorized JSON response if authentication fails. This feature is available since Next.js v13.1.0.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/03-building-your-application/01-routing/14-middleware.mdx#_snippet_13

LANGUAGE: TypeScript
CODE:
```
import type { NextRequest } from 'next/server'
import { isAuthenticated } from '@lib/auth'

// Limit the middleware to paths starting with `/api/`
export const config = {
  matcher: '/api/:function*',
}

export function middleware(request: NextRequest) {
  // Call our authentication function to check the request
  if (!isAuthenticated(request)) {
    // Respond with JSON indicating an error message
    return Response.json(
      { success: false, message: 'authentication failed' },
      { status: 401 }
    )
  }
}
```

LANGUAGE: JavaScript
CODE:
```
import { isAuthenticated } from '@lib/auth'

// Limit the middleware to paths starting with `/api/`
export const config = {
  matcher: '/api/:function*',
}

export function middleware(request) {
  // Call our authentication function to check the request
  if (!isAuthenticated(request)) {
    // Respond with JSON indicating an error message
    return Response.json(
      { success: false, message: 'authentication failed' },
      { status: 401 }
    )
  }
}
```

----------------------------------------

TITLE: Migrating Synchronous Page Components to use `use` Hook (JSX)
DESCRIPTION: This example demonstrates the updated pattern for synchronous page components in JSX, where `params` and `searchParams` are now accessed via the `use` React hook. This change is necessary because these values are now wrapped in Promises.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/02-guides/upgrading/version-15.mdx#_snippet_18

LANGUAGE: jsx
CODE:
```
// Before
export default function Page({ params, searchParams }) {
  const { slug } = params
  const { query } = searchParams
}
```

LANGUAGE: jsx
CODE:
```
// After
import { use } from "react"

export default function Page(props) {
  const params = use(props.params)
  const searchParams = use(props.searchParams)
  const slug = params.slug
  const query = searchParams.query
}
```

----------------------------------------

TITLE: Migrating Synchronous Page Components to use `use` Hook (TSX)
DESCRIPTION: This snippet illustrates the change in synchronous page components where `params` and `searchParams` are now Promise-wrapped and must be accessed using the `use` React hook instead of direct destructuring. This ensures proper handling of asynchronous data in server components.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/02-guides/upgrading/version-15.mdx#_snippet_17

LANGUAGE: tsx
CODE:
```
'use client'

// Before
type Params = { slug: string }
type SearchParams = { [key: string]: string | string[] | undefined }

export default function Page({
  params,
  searchParams,
}: {
  params: Params
  searchParams: SearchParams
}) {
  const { slug } = params
  const { query } = searchParams
}
```

LANGUAGE: tsx
CODE:
```
import { use } from 'react'

// After
type Params = Promise<{ slug: string }>
type SearchParams = Promise<{ [key: string]: string | string[] | undefined }>

export default function Page(props: {
  params: Params
  searchParams: SearchParams
}) {
  const params = use(props.params)
  const searchParams = use(props.searchParams)
  const slug = params.slug
  const query = searchParams.query
}
```

----------------------------------------

TITLE: Displaying Remote Images in Next.js
DESCRIPTION: This code demonstrates the basic usage of the `next/image` component to display an image from a remote URL. For remote images, it is crucial to manually provide the `width` and `height` props, as Next.js cannot infer these during the build process, which helps prevent layout shifts.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/05-api-reference/02-components/image.mdx#_snippet_50

LANGUAGE: jsx
CODE:
```
import Image from 'next/image'

export default function Page() {
  return (
    <Image
      src="https://s3.amazonaws.com/my-bucket/profile.png"
      alt="Picture of the author"
      width={500}
      height={500}
    />
  )
}
```

----------------------------------------

TITLE: Implementing Error Boundary with Logging in Next.js (JSX)
DESCRIPTION: This Client Component defines an error boundary for Next.js applications, catching runtime errors and displaying a fallback UI. It uses `useEffect` to log the error to a reporting service and provides a 'Try again' button that utilizes the `reset` function to attempt re-rendering the affected segment.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/05-api-reference/03-file-conventions/error.mdx#_snippet_1

LANGUAGE: jsx
CODE:
```
'use client' // Error boundaries must be Client Components

import { useEffect } from 'react'

export default function Error({ error, reset }) {
  useEffect(() => {
    // Log the error to an error reporting service
    console.error(error)
  }, [error])

  return (
    <div>
      <h2>Something went wrong!</h2>
      <button
        onClick={
          // Attempt to recover by trying to re-render the segment
          () => reset()
        }
      >
        Try again
      </button>
    </div>
  )
}
```

----------------------------------------

TITLE: Running Next.js App in Development Mode (Yarn)
DESCRIPTION: These commands first install all project dependencies using `yarn install` and then start the Next.js development server with `yarn dev`. This provides an alternative to npm for Yarn users, making the application available at `http://localhost:3000`.
SOURCE: https://github.com/vercel/next.js/blob/canary/examples/with-elasticsearch/README.md#_snippet_5

LANGUAGE: bash
CODE:
```
yarn install
yarn dev
```

----------------------------------------

TITLE: Configuring Security Headers in Next.js
DESCRIPTION: This configuration in `next.config.js` defines security headers for a Next.js application. It applies global headers like `X-Content-Type-Options`, `X-Frame-Options`, and `Referrer-Policy` to all routes, and specific headers for the service worker (`/sw.js`) to ensure correct interpretation, prevent caching, and enforce a strict Content Security Policy.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/02-guides/progressive-web-apps.mdx#_snippet_11

LANGUAGE: JavaScript
CODE:
```
module.exports = {
  async headers() {
    return [
      {
        source: '/(.*)',
        headers: [
          {
            key: 'X-Content-Type-Options',
            value: 'nosniff',
          },
          {
            key: 'X-Frame-Options',
            value: 'DENY',
          },
          {
            key: 'Referrer-Policy',
            value: 'strict-origin-when-cross-origin',
          }
        ]
      },
      {
        source: '/sw.js',
        headers: [
          {
            key: 'Content-Type',
            value: 'application/javascript; charset=utf-8',
          },
          {
            key: 'Cache-Control',
            value: 'no-cache, no-store, must-revalidate',
          },
          {
            key: 'Content-Security-Policy',
            value: "default-src 'self'; script-src 'self'",
          }
        ]
      }
    ]
  }
}
```

----------------------------------------

TITLE: Reading Cookies from NextRequest in Next.js Route Handlers
DESCRIPTION: This snippet illustrates how to access cookies directly from the `request` object, specifically using the `NextRequest` type, within a Next.js Route Handler. It provides an alternative method to read incoming cookies without using the `cookies` function from `next/headers`.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/03-building-your-application/01-routing/13-route-handlers.mdx#_snippet_8

LANGUAGE: TypeScript
CODE:
```
import { type NextRequest } from 'next/server'

export async function GET(request: NextRequest) {
  const token = request.cookies.get('token')
}
```

LANGUAGE: JavaScript
CODE:
```
export async function GET(request) {
  const token = request.cookies.get('token')
}
```

----------------------------------------

TITLE: Configuring SWR Fallback in Next.js Layout (JS)
DESCRIPTION: Demonstrates how to configure SWR with server-provided fallback data in a Next.js Root Layout Server Component using JavaScript. The `getUser()` function is called on the server, and its result is provided as fallback data to SWR.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/02-guides/single-page-applications.mdx#_snippet_4

LANGUAGE: js
CODE:
```
import { SWRConfig } from 'swr';
import { getUser } from './user'; // some server-side function

export default function RootLayout({ children }) {
  return (
    <SWRConfig
      value={{
        fallback: {
          // We do NOT await getUser() here
          // Only components that read this data will suspend
          '/api/user': getUser(),
        },
      }}
    >
      {children}
    </SWRConfig>
  );
}
```

----------------------------------------

TITLE: Using Third-Party Client-Only Components within a Client Component in Next.js
DESCRIPTION: This Client Component (Gallery), marked with 'use client', demonstrates the direct use of a third-party component (<Carousel>) that relies on client-only features (e.g., useState). Since Gallery itself is a Client Component, the <Carousel> functions correctly without needing an explicit 'use client' directive within its own file.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/01-getting-started/07-server-and-client-components.mdx#_snippet_8

LANGUAGE: TypeScript
CODE:
```
'use client'

import { useState } from 'react'
import { Carousel } from 'acme-carousel'

export default function Gallery() {
  const [isOpen, setIsOpen] = useState(false)

  return (
    <div>
      <button onClick={() => setIsOpen(true)}>View pictures</button>
      {/* Works, since Carousel is used within a Client Component */}
      {isOpen && <Carousel />}
    </div>
  )
}
```

LANGUAGE: JavaScript
CODE:
```
'use client'

import { useState } from 'react'
import { Carousel } from 'acme-carousel'

export default function Gallery() {
  const [isOpen, setIsOpen] = useState(false)

  return (
    <div>
      <button onClick={() => setIsOpen(true)}>View pictures</button>
      {/*  Works, since Carousel is used within a Client Component */}
      {isOpen && <Carousel />}
    </div>
  )
}
```

----------------------------------------

TITLE: Accessing Environment Variables in Route Handlers (App Router)
DESCRIPTION: This JavaScript snippet demonstrates how to access environment variables, defined in .env files, within a Route Handler in a Next.js App Router application. The variables are available via process.env for server-side operations.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/02-guides/environment-variables.mdx#_snippet_3

LANGUAGE: js
CODE:
```
export async function GET() {
  const db = await myDB.connect({
    host: process.env.DB_HOST,
    username: process.env.DB_USER,
    password: process.env.DB_PASS,
  })
  // ...
}
```

----------------------------------------

TITLE: Generating RSS XML Response in Next.js Route Handler (TypeScript)
DESCRIPTION: This snippet demonstrates how a Next.js Route Handler can be used to serve non-UI content, specifically an RSS XML feed. The `GET` function returns a `Response` object with the `Content-Type` header set to `text/xml`, allowing the browser or client to interpret the response as an XML document rather than HTML.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/03-building-your-application/01-routing/13-route-handlers.mdx#_snippet_22

LANGUAGE: ts
CODE:
```
export async function GET() {
  return new Response(
    `<?xml version="1.0" encoding="UTF-8" ?>
<rss version="2.0">

<channel>
  <title>Next.js Documentation</title>
  <link>https://nextjs.org/docs</link>
  <description>The React Framework for the Web</description>
</channel>

</rss>`,
    {
      headers: {
        'Content-Type': 'text/xml',
      },
    }
  )
}
```

----------------------------------------

TITLE: Creating Route Handlers for GET Requests in App Router (JavaScript, Next.js)
DESCRIPTION: This snippet shows the basic structure for a Route Handler in the `app` directory using JavaScript. It exports an asynchronous `GET` function that takes a `Request` object. Route Handlers are used to create custom request handlers for routes, replacing traditional API Routes in the `app` directory.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/02-guides/migrating/app-router-migration.mdx#_snippet_37

LANGUAGE: JS
CODE:
```
export async function GET(request) {}
```

----------------------------------------

TITLE: Updating Next.js to Latest Version
DESCRIPTION: This command updates the Next.js framework to its latest stable version using npm. It is a prerequisite for utilizing new features introduced in Next.js 13.4 or greater, such as the `app` directory.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/02-guides/migrating/app-router-migration.mdx#_snippet_3

LANGUAGE: bash
CODE:
```
npm install next@latest
```

----------------------------------------

TITLE: Opting into PPR for a Route Segment (TypeScript)
DESCRIPTION: This snippet shows how to explicitly enable Partial Prerendering for a specific route segment in a Next.js application by exporting `experimental_ppr = true` from a `layout.tsx` file. This setting applies to all children of the route segment.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/01-getting-started/12-partial-prerendering.mdx#_snippet_3

LANGUAGE: TSX
CODE:
```
export const experimental_ppr = true

export default function Layout({ children }: { children: React.ReactNode }) {
  // ...
}
```

----------------------------------------

TITLE: Migrating Next.js Link Component Usage
DESCRIPTION: This snippet illustrates the change in the `<Link>` component's usage between Next.js 12 and 13. In Next.js 13, the `<a>` tag is automatically rendered by `<Link>`, removing the need for it as a direct child. This simplifies linking and allows props to be forwarded directly to the underlying `<a>` tag.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/02-guides/migrating/app-router-migration.mdx#_snippet_2

LANGUAGE: jsx
CODE:
```
import Link from 'next/link'

// Next.js 12: `<a>` has to be nested otherwise it's excluded
<Link href="/about">
  <a>About</a>
</Link>

// Next.js 13: `<Link>` always renders `<a>` under the hood
<Link href="/about">
  About
</Link>
```

----------------------------------------

TITLE: Server-Side Redirection with redirect Function in Next.js (TSX/JSX)
DESCRIPTION: This snippet demonstrates using the `redirect` function from `next/navigation` for server-side redirects in Next.js Server Components. It checks for the presence of an `id` and a `team` object, redirecting to `/login` or `/join` respectively if conditions are not met. This function is ideal for handling authentication or data availability checks before rendering.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/03-building-your-application/01-routing/04-linking-and-navigating.mdx#_snippet_2

LANGUAGE: tsx
CODE:
```
import { redirect } from 'next/navigation'

async function fetchTeam(id: string) {
  const res = await fetch('https://...')
  if (!res.ok) return undefined
  return res.json()
}

export default async function Profile({
  params,
}: {
  params: Promise<{ id: string }>
}) {
  const { id } = await params
  if (!id) {
    redirect('/login')
  }

  const team = await fetchTeam(id)
  if (!team) {
    redirect('/join')
  }

  // ...
}
```

LANGUAGE: jsx
CODE:
```
import { redirect } from 'next/navigation'

async function fetchTeam(id) {
  const res = await fetch('https://...')
  if (!res.ok) return undefined
  return res.json()
}

export default async function Profile({ params }) {
  const { id } = await params
  if (!id) {
    redirect('/login')
  }

  const team = await fetchTeam(id)
  if (!team) {
    redirect('/join')
  }

  // ...
}
```

----------------------------------------

TITLE: Basic Link Component Usage in Next.js (TSX)
DESCRIPTION: Provides a minimum working example of using the `next/link` component in a Next.js App Router page, including the necessary import statement.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/04-community/01-contribution-guide.mdx#_snippet_8

LANGUAGE: tsx
CODE:
```
import Link from 'next/link'

export default function Page() {
  return <Link href="/about">About</Link>
}
```

----------------------------------------

TITLE: Handling Dynamic Route Segments in Next.js Route Handlers
DESCRIPTION: This snippet illustrates how to capture dynamic route segments in a Next.js Route Handler. The `params` object, passed as an argument to the `GET` function, contains the values of the dynamic segments (e.g., `slug`), allowing the handler to respond based on the URL path.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/03-building-your-application/01-routing/13-route-handlers.mdx#_snippet_12

LANGUAGE: TypeScript
CODE:
```
export async function GET(
  request: Request,
  { params }: { params: Promise<{ slug: string }> }
) {
  const { slug } = await params // 'a', 'b', or 'c'
}
```

LANGUAGE: JavaScript
CODE:
```
export async function GET(request, { params }) {
  const { slug } = await params // 'a', 'b', or 'c'
}
```

----------------------------------------

TITLE: Importing Image Component in Next.js (JSX)
DESCRIPTION: This snippet imports the `Image` component from `next/image`, making it available for use in the current file. It is a prerequisite for using Next.js optimized image handling features.
SOURCE: https://github.com/vercel/next.js/blob/canary/examples/blog/pages/photos.mdx#_snippet_0

LANGUAGE: JSX
CODE:
```
import Image from "next/image";
```

----------------------------------------

TITLE: Tagging unstable_cache Functions for revalidateTag in Next.js (TS/JS)
DESCRIPTION: This example illustrates how to apply tags to `unstable_cache` functions using the `tags` option. This enables targeted cache revalidation for the results of the cached function, allowing `revalidateTag` to invalidate specific cached data without affecting others.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/01-getting-started/09-caching-and-revalidating.mdx#_snippet_6

LANGUAGE: TypeScript
CODE:
```
export const getUserById = unstable_cache(
  async (id: string) => {
    return db.query.users.findFirst({ where: eq(users.id, id) })
  },
  ['user'], // Needed if variables are not passed as parameters
  {
    tags: ['user'],
  }
)
```

LANGUAGE: JavaScript
CODE:
```
export const getUserById = unstable_cache(
  async (id) => {
    return db.query.users.findFirst({ where: eq(users.id, id) })
  },
  ['user'], // Needed if variables are not passed as parameters
  {
    tags: ['user'],
  }
)
```

----------------------------------------

TITLE: Accessing NextRequest Object in Next.js Middleware (TSX/JS)
DESCRIPTION: This snippet highlights the signature of the Next.js middleware function, which receives a single parameter, `request`. This parameter is an instance of `NextRequest`, providing access to details about the incoming HTTP request, such as URL, headers, and cookies.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/05-api-reference/03-file-conventions/middleware.mdx#_snippet_3

LANGUAGE: tsx
CODE:
```
import type { NextRequest } from 'next/server'

export function middleware(request: NextRequest) {
  // Middleware logic goes here
}
```

LANGUAGE: js
CODE:
```
export function middleware(request) {
  // Middleware logic goes here
}
```

----------------------------------------

TITLE: Accessing Typed Dynamic Route Parameters with useParams (TypeScript)
DESCRIPTION: This example demonstrates how to use the `useParams` hook within a Next.js Client Component to access dynamic route parameters with type annotations. It shows how `params` maps to URL segments like `/shop/[tag]/[item]`, returning an object with corresponding string values.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/05-api-reference/04-functions/use-params.mdx#_snippet_0

LANGUAGE: typescript
CODE:
```
'use client'

import { useParams } from 'next/navigation'

export default function ExampleClientComponent() {
  const params = useParams<{ tag: string; item: string }>()

  // Route -> /shop/[tag]/[item]
  // URL -> /shop/shoes/nike-air-max-97
  // `params` -> { tag: 'shoes', item: 'nike-air-max-97' }
  console.log(params)

  return '...'
}
```

----------------------------------------

TITLE: Opting out of Fetch Caching in Next.js (JavaScript)
DESCRIPTION: This snippet demonstrates how to prevent a 'fetch' request from being cached by Next.js. By setting 'cache: 'no-store'' in the 'fetch' options, the response will not be stored in the Data Cache, ensuring that fresh data is always fetched from the external source on subsequent requests. This is useful for highly dynamic data that should never be stale.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/04-deep-dive/caching.mdx#_snippet_4

LANGUAGE: JavaScript
CODE:
```
let data = await fetch('https://api.vercel.app/blog', { cache: 'no-store' })
```

----------------------------------------

TITLE: Accessing Search Params in Next.js Client Component
DESCRIPTION: This example illustrates how to access URL query parameters within a Next.js Client Component. The `useSearchParams` hook from `next/navigation` is utilized, as Client Components re-render on navigation, ensuring access to the latest search parameters, unlike static layouts.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/05-api-reference/03-file-conventions/layout.mdx#_snippet_5

LANGUAGE: tsx
CODE:
```
'use client'

import { useSearchParams } from 'next/navigation'

export default function Search() {
  const searchParams = useSearchParams()

  const search = searchParams.get('search')

  return '...'
}
```

LANGUAGE: jsx
CODE:
```
'use client'

import { useSearchParams } from 'next/navigation'

export default function Search() {
  const searchParams = useSearchParams()

  const search = searchParams.get('search')

  return '...'
}
```

----------------------------------------

TITLE: Disabling Server-Side Rendering for Components in Next.js
DESCRIPTION: This snippet shows how to use `next/dynamic` with `ssr: false` to disable server-side rendering for a specific component. This is useful for components that rely heavily on browser-only APIs or cause hydration mismatches, ensuring they are only rendered on the client.
SOURCE: https://github.com/vercel/next.js/blob/canary/errors/react-hydration-error.mdx#_snippet_1

LANGUAGE: jsx
CODE:
```
import dynamic from 'next/dynamic'

const NoSSR = dynamic(() => import('../components/no-ssr'), { ssr: false })

export default function Page() {
  return (
    <div>
      <NoSSR />
    </div>
  )
}
```

----------------------------------------

TITLE: Configuring Tailwind CSS Font Variables in Global CSS
DESCRIPTION: This CSS snippet demonstrates how to map custom CSS variables, defined by `next/font`, to Tailwind CSS's default font families (`--font-sans`, `--font-mono`). This configuration, typically placed in a global CSS file, allows Tailwind utility classes like `font-sans` to correctly apply the specified Next.js fonts.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/05-api-reference/02-components/font.mdx#_snippet_38

LANGUAGE: css
CODE:
```
@import "tailwindcss";

@theme inline {
  --font-sans: var(--font-inter);
  --font-mono: var(--font-roboto-mono);
}
```

----------------------------------------

TITLE: Activating Next.js Preview Mode with GraphCMS
DESCRIPTION: This URL format is used to activate Next.js Preview Mode, allowing users to view draft content from GraphCMS. It requires a secret token for authentication and the slug of the content to be previewed. The secret token must match the one defined in the .env.local file.
SOURCE: https://github.com/vercel/next.js/blob/canary/examples/cms-graphcms/README.md#_snippet_6

LANGUAGE: URL
CODE:
```
http://localhost:3000/api/preview?secret=<YOUR_SECRET_TOKEN>&slug=<SLUG_TO_PREVIEW>
```

----------------------------------------

TITLE: Configuring Next.js Build Cache in Travis CI
DESCRIPTION: This YAML configuration for `.travis.yml` adds `.next/cache` to the `cache.directories` list, allowing Travis CI to persist Next.js build artifacts and `node_modules` between builds for faster subsequent builds.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/02-guides/ci-build-caching.mdx#_snippet_1

LANGUAGE: yaml
CODE:
```
cache:
  directories:
    - $HOME/.cache/yarn
    - node_modules
    - .next/cache
```

----------------------------------------

TITLE: Generating Open Graph Image with External Data (TypeScript)
DESCRIPTION: This snippet demonstrates how to create an Open Graph image dynamically in Next.js by fetching post data from an external API using `fetch` and displaying the post title. It utilizes the `params` object to extract the `slug` for the API call, and sets metadata like `alt`, `size`, and `contentType` for the image.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/05-api-reference/03-file-conventions/01-metadata/opengraph-image.mdx#_snippet_20

LANGUAGE: TypeScript
CODE:
```
import { ImageResponse } from 'next/og'

export const alt = 'About Acme'
export const size = {
  width: 1200,
  height: 630,
}
export const contentType = 'image/png'

export default async function Image({ params }: { params: { slug: string } }) {
  const post = await fetch(`https://.../posts/${params.slug}`).then((res) =>
    res.json()
  )

  return new ImageResponse(
    (
      <div
        style={{
          fontSize: 48,
          background: 'white',
          width: '100%',
          height: '100%',
          display: 'flex',
          alignItems: 'center',
          justifyContent: 'center',
        }}
      >
        {post.title}
      </div>
    ),
    {
      ...size,
    }
  )
}
```

----------------------------------------

TITLE: Prefetching Dashboard Page After Login in Next.js
DESCRIPTION: Illustrates prefetching the dashboard page using `router.prefetch` in a Next.js login component. The `useEffect` hook prefetches the page on component mount, and `router.push` is used for a fast transition after a successful API login.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/02-pages/04-api-reference/03-functions/use-router.mdx#_snippet_12

LANGUAGE: JSX
CODE:
```
import { useCallback, useEffect } from 'react'
import { useRouter } from 'next/router'

export default function Login() {
  const router = useRouter()
  const handleSubmit = useCallback((e) => {
    e.preventDefault()

    fetch('/api/login', {
      method: 'POST',
      headers: { 'Content-Type': 'application/json' },
      body: JSON.stringify({
        /* Form data */
      }),
    }).then((res) => {
      // Do a fast client-side transition to the already prefetched dashboard page
      if (res.ok) router.push('/dashboard')
    })
  }, [])

  useEffect(() => {
    // Prefetch the dashboard page
    router.prefetch('/dashboard')
  }, [router])

  return (
    <form onSubmit={handleSubmit}>
      {/* Form fields */}
      <button type="submit">Login</button>
    </form>
  )
}
```

----------------------------------------

TITLE: Adding Cache Tags to Fetch Requests - TypeScript/JavaScript
DESCRIPTION: This example illustrates how to associate cache tags with data fetched using the `fetch` API. By including the `next: { tags: [...] }` option, developers can assign one or more string tags to the fetched data, enabling targeted revalidation later with `revalidateTag`.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/05-api-reference/04-functions/revalidateTag.mdx#_snippet_1

LANGUAGE: tsx
CODE:
```
fetch(url, { next: { tags: [...] } });
```

----------------------------------------

TITLE: Getting a Cookie Value with NextRequest in TypeScript
DESCRIPTION: This snippet shows how to retrieve the value of a specific cookie from the `NextRequest` object using the `get` method. It returns the cookie's value as a string or `undefined` if the cookie is not found. If multiple cookies with the same name exist, the first one is returned.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/05-api-reference/04-functions/next-request.mdx#_snippet_1

LANGUAGE: ts
CODE:
```
request.cookies.get('show-banner')
```

----------------------------------------

TITLE: Rewriting (Proxying) a URL with NextResponse in TypeScript
DESCRIPTION: This snippet illustrates how to use `NextResponse.rewrite()` to internally proxy a request to a different URL without changing the URL displayed in the browser. This is useful for URL masking or internal routing, where the client remains unaware of the underlying resource location.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/05-api-reference/04-functions/next-response.mdx#_snippet_8

LANGUAGE: TypeScript
CODE:
```
import { NextResponse } from 'next/server'

// Incoming request: /about, browser shows /about
// Rewritten request: /proxy, browser shows /about
return NextResponse.rewrite(new URL('/proxy', request.url))
```

----------------------------------------

TITLE: Implementing Incremental Static Regeneration with `fetch` in App Router (Next.js)
DESCRIPTION: This example shows how to use the `fetch()` API with the `next: { revalidate: 60 }` option in the `app` directory to cache data for 60 seconds. The `getPosts` function fetches data, and the `PostList` component then renders the fetched posts. This approach enables data revalidation similar to ISR but directly within `fetch` calls.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/02-guides/migrating/app-router-migration.mdx#_snippet_35

LANGUAGE: JSX
CODE:
```
// `app` directory

async function getPosts() {
  const res = await fetch(`https://.../posts`, { next: { revalidate: 60 } })
  const data = await res.json()

  return data.posts
}

export default async function PostList() {
  const posts = await getPosts()

  return posts.map((post) => <div>{post.name}</div>)
}
```

----------------------------------------

TITLE: Wrapping useSearchParams Component with Suspense (Next.js)
DESCRIPTION: This page component illustrates how to wrap a component that uses `useSearchParams` (like `SearchBar`) within a React `Suspense` boundary. This pattern ensures that only the dynamic part of the page is client-side rendered, preventing the entire page from being deopted during static rendering.
SOURCE: https://github.com/vercel/next.js/blob/canary/errors/deopted-into-client-rendering.mdx#_snippet_1

LANGUAGE: tsx
CODE:
```
import { Suspense } from 'react'
import SearchBar from './search-bar'

// This component passed as fallback to the Suspense boundary
// will be rendered in place of the search bar in the initial HTML.
// When the value is available during React hydration the fallback
// will be replaced with the `<SearchBar>` component.
function SearchBarFallback() {
  return <>placeholder</>
}

export default function Page() {
  return (
    <>
      <nav>
        <Suspense fallback={<SearchBarFallback />}>
          <SearchBar />
        </Suspense>
      </nav>
      <h1>Dashboard</h1>
    </>
  )
}
```

----------------------------------------

TITLE: Configuring Custom HTTP Headers in Next.js
DESCRIPTION: This snippet demonstrates the basic setup for adding custom HTTP headers in Next.js. It defines an `async headers()` function in `next.config.js` that returns an array of objects, each specifying a `source` path pattern and an array of `headers` with `key` and `value` properties to be applied to responses matching the source.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/05-api-reference/05-config/01-next-config-js/headers.mdx#_snippet_0

LANGUAGE: JavaScript
CODE:
```
module.exports = {
  async headers() {
    return [
      {
        source: '/about',
        headers: [
          {
            key: 'x-custom-header',
            value: 'my custom header value',
          },
          {
            key: 'x-another-custom-header',
            value: 'my other custom header value',
          },
        ],
      },
    ]
  },
}
```

----------------------------------------

TITLE: Generating Dynamic Open Graph Images with External Data in Next.js
DESCRIPTION: This snippet demonstrates how to dynamically generate multiple Open Graph images for a Next.js route using `generateImageMetadata` and a default `Image` component. It fetches image metadata and content based on route parameters (`params.id`) from external utility functions (`getOGImages`, `getCaptionForImage`), allowing for custom image sizes, alt text, and content type. The `generateImageMetadata` function returns an array of image properties, while the `Image` component renders the actual image content.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/05-api-reference/04-functions/generate-image-metadata.mdx#_snippet_2

LANGUAGE: TypeScript
CODE:
```
import { ImageResponse } from 'next/og'
import { getCaptionForImage, getOGImages } from '@/app/utils/images'

export async function generateImageMetadata({
  params,
}: {
  params: { id: string }
}) {
  const images = await getOGImages(params.id)

  return images.map((image, idx) => ({
    id: idx,
    size: { width: 1200, height: 600 },
    alt: image.text,
    contentType: 'image/png',
  }))
}

export default async function Image({
  params,
  id,
}: {
  params: { id: string }
  id: number
}) {
  const productId = (await params).id
  const imageId = id
  const text = await getCaptionForImage(productId, imageId)

  return new ImageResponse(
    (
      <div
        style={
          {
            // ...
          }
        }
      >
        {text}
      </div>
    )
  )
}
```

LANGUAGE: JavaScript
CODE:
```
import { ImageResponse } from 'next/og'
import { getCaptionForImage, getOGImages } from '@/app/utils/images'

export async function generateImageMetadata({ params }) {
  const images = await getOGImages(params.id)

  return images.map((image, idx) => ({
    id: idx,
    size: { width: 1200, height: 600 },
    alt: image.text,
    contentType: 'image/png',
  }))
}

export default async function Image({ params, id }) {
  const productId = (await params).id
  const imageId = id
  const text = await getCaptionForImage(productId, imageId)

  return new ImageResponse(
    (
      <div
        style={
          {
            // ...
          }
        }
      >
        {text}
      </div>
    )
  )
}
```

----------------------------------------

TITLE: Revalidating Cache Tag in Route Handler - JavaScript
DESCRIPTION: This JavaScript Route Handler illustrates how to dynamically revalidate a cache tag based on a query parameter. It imports `revalidateTag`, extracts the `tag` from the request's search parameters, calls `revalidateTag` with it, and returns a JSON response indicating successful revalidation.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/05-api-reference/04-functions/revalidateTag.mdx#_snippet_5

LANGUAGE: js
CODE:
```
import { revalidateTag } from 'next/cache'

export async function GET(request) {
  const tag = request.nextUrl.searchParams.get('tag')
  revalidateTag(tag)
  return Response.json({ revalidated: true, now: Date.now() })
}
```

----------------------------------------

TITLE: Enabling Statically Typed Links in Next.js Configuration
DESCRIPTION: This snippet demonstrates how to enable the `experimental.typedRoutes` feature in your `next.config.ts` file. Opting into this feature, along with using TypeScript, allows Next.js to generate link definitions for compile-time type checking of `href` values in `next/link` components, preventing navigation errors.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/05-api-reference/05-config/02-typescript.mdx#_snippet_3

LANGUAGE: TypeScript
CODE:
```
import type { NextConfig } from 'next'

const nextConfig: NextConfig = {
  experimental: {
    typedRoutes: true,
  },
}

export default nextConfig
```

----------------------------------------

TITLE: Configuring Description Metadata in Next.js (JSX)
DESCRIPTION: This snippet demonstrates how to set the `description` metadata field in a Next.js `layout.js` or `page.js` file. This value is used to generate the HTML `<meta name="description">` tag, providing a concise summary of the page content for search engines and social media.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/05-api-reference/04-functions/generate-metadata.mdx#_snippet_15

LANGUAGE: jsx
CODE:
```
export const metadata = {
  description: 'The React Framework for the Web',
}
```

----------------------------------------

TITLE: Programmatic Form Submission with Keyboard Shortcut
DESCRIPTION: This component demonstrates how to programmatically submit a form using the `requestSubmit()` method. It listens for the `onKeyDown` event on a textarea, specifically for the `⌘` + `Enter` keyboard shortcut, to trigger the submission of the nearest ancestor form, which will then invoke a Server Function.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/02-guides/forms.mdx#_snippet_17

LANGUAGE: tsx
CODE:
```
'use client'

export function Entry() {
  const handleKeyDown = (e: React.KeyboardEvent<HTMLTextAreaElement>) => {
    if (
      (e.ctrlKey || e.metaKey) &&
      (e.key === 'Enter' || e.key === 'NumpadEnter')
    ) {
      e.preventDefault()
      e.currentTarget.form?.requestSubmit()
    }
  }

  return (
    <div>
      <textarea name="entry" rows={20} required onKeyDown={handleKeyDown} />
    </div>
  )
}
```

LANGUAGE: jsx
CODE:
```
'use client'

export function Entry() {
  const handleKeyDown = (e) => {
    if (
      (e.ctrlKey || e.metaKey) &&
      (e.key === 'Enter' || e.key === 'NumpadEnter')
    ) {
      e.preventDefault()
      e.currentTarget.form?.requestSubmit()
    }
  }

  return (
    <div>
      <textarea name="entry" rows={20} required onKeyDown={handleKeyDown} />
    </div>
  )
}
```

----------------------------------------

TITLE: Adding Custom Loading Component with `next/dynamic` (Next.js App Router)
DESCRIPTION: This example shows how to use `next/dynamic` to asynchronously load a component (`WithCustomLoading`) and display a custom loading indicator (`<p>Loading...</p>`) while the component is being fetched in a Next.js App Router application. This improves user experience by providing immediate feedback.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/02-guides/lazy-loading.mdx#_snippet_4

LANGUAGE: jsx
CODE:
```
'use client'

import dynamic from 'next/dynamic'

const WithCustomLoading = dynamic(
  () => import('../components/WithCustomLoading'),
  {
    loading: () => <p>Loading...</p>,
  }
)

export default function Page() {
  return (
    <div>
      {/* The loading component will be rendered while  <WithCustomLoading/> is loading */}
      <WithCustomLoading />
    </div>
  )
}
```

----------------------------------------

TITLE: revalidateTag Function Signature - TypeScript
DESCRIPTION: This snippet defines the signature for the `revalidateTag` function, indicating it accepts a single `tag` parameter of type string and returns `void`. The `tag` string represents the cache tag to be revalidated and must be 256 characters or less, case-sensitive.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/05-api-reference/04-functions/revalidateTag.mdx#_snippet_0

LANGUAGE: tsx
CODE:
```
revalidateTag(tag: string): void;
```

----------------------------------------

TITLE: Registering Observability Tools in Next.js
DESCRIPTION: This function is called once when a new Next.js server instance starts. It integrates observability tools by calling `registerOTel` from `@vercel/otel`, typically used for OpenTelemetry setup. It requires the `@vercel/otel` dependency.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/05-api-reference/03-file-conventions/instrumentation.mdx#_snippet_0

LANGUAGE: TypeScript
CODE:
```
import { registerOTel } from '@vercel/otel'

export function register() {
  registerOTel('next-app')
}
```

LANGUAGE: JavaScript
CODE:
```
import { registerOTel } from '@vercel/otel'

export function register() {
  registerOTel('next-app')
}
```

----------------------------------------

TITLE: Overriding Default Fetch Cache Behavior with `fetchCache` in JavaScript
DESCRIPTION: This snippet shows how to use the `fetchCache` option to override the default caching behavior of all `fetch` requests within a Next.js layout or page. It provides various string values like `'auto'`, `'force-cache'`, or `'force-no-store'` to control whether fetches are cached or not, regardless of their individual `cache` options or proximity to Dynamic APIs.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/05-api-reference/03-file-conventions/route-segment-config.mdx#_snippet_9

LANGUAGE: js
CODE:
```
export const fetchCache = 'auto'
// 'auto' | 'default-cache' | 'only-cache'
// 'force-cache' | 'force-no-store' | 'default-no-store' | 'only-no-store'
```

----------------------------------------

TITLE: Detecting User Device Type in Next.js Middleware
DESCRIPTION: This snippet demonstrates how to use the `userAgent` helper in Next.js middleware to extract device information from the incoming request. It specifically identifies the device type (e.g., 'mobile', 'tablet', 'desktop') and sets it as a URL search parameter, allowing for dynamic content based on the user's device. It requires `NextRequest`, `NextResponse`, and `userAgent` from `next/server`.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/05-api-reference/04-functions/userAgent.mdx#_snippet_0

LANGUAGE: TypeScript
CODE:
```
import { NextRequest, NextResponse, userAgent } from 'next/server'

export function middleware(request: NextRequest) {
  const url = request.nextUrl
  const { device } = userAgent(request)

  // device.type can be: 'mobile', 'tablet', 'console', 'smarttv',
  // 'wearable', 'embedded', or undefined (for desktop browsers)
  const viewport = device.type || 'desktop'

  url.searchParams.set('viewport', viewport)
  return NextResponse.rewrite(url)
}
```

LANGUAGE: JavaScript
CODE:
```
import { NextResponse, userAgent } from 'next/server'

export function middleware(request) {
  const url = request.nextUrl
  const { device } = userAgent(request)

  // device.type can be: 'mobile', 'tablet', 'console', 'smarttv',
  // 'wearable', 'embedded', or undefined (for desktop browsers)
  const viewport = device.type || 'desktop'

  url.searchParams.set('viewport', viewport)
  return NextResponse.rewrite(url)
}
```

----------------------------------------

TITLE: Implementing Responsive Images with Remote URLs in Next.js
DESCRIPTION: This snippet shows how to display a responsive image from a remote URL using the `next/image` component. For remote images, it's essential to manually provide `width` and `height` props so Next.js can calculate the aspect ratio and prevent layout shifts during loading. The `sizes` and `style` props are used for responsiveness.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/05-api-reference/02-components/image.mdx#_snippet_47

LANGUAGE: jsx
CODE:
```
import Image from 'next/image'

export default function Page({ photoUrl }) {
  return (
    <Image
      src={photoUrl}
      alt="Picture of the author"
      sizes="100vw"
      style={{
        width: '100%',
        height: 'auto',
      }}
      width={500}
      height={300}
    />
  )
}
```

----------------------------------------

TITLE: Loading Application Scripts in Root Layout (App Router)
DESCRIPTION: Shows how to add a third-party script to the root layout in the Next.js App Router using `next/script`. This script will load and execute when any route in the application is accessed, ensuring it loads only once.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/02-guides/scripts.mdx#_snippet_1

LANGUAGE: tsx
CODE:
```
import Script from 'next/script'

export default function RootLayout({
  children,
}: {
  children: React.ReactNode
}) {
  return (
    <html lang="en">
      <body>{children}</body>
      <Script src="https://example.com/script.js" />
    </html>
  )
}
```

LANGUAGE: jsx
CODE:
```
import Script from 'next/script'

export default function RootLayout({ children }) {
  return (
    <html lang="en">
      <body>{children}</body>
      <Script src="https://example.com/script.js" />
    </html>
  )
}
```

----------------------------------------

TITLE: Implementing Form with Navigation Blocker Context
DESCRIPTION: This React form component integrates with the `useNavigationBlocker` hook to control the navigation blocking state. It sets `isBlocked` to `true` when the form content changes (indicating unsaved changes) and resets `isBlocked` to `false` when the form is submitted, allowing navigation to proceed.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/05-api-reference/02-components/link.mdx#_snippet_35

LANGUAGE: TypeScript
CODE:
```
'use client'

import { useNavigationBlocker } from '../contexts/navigation-blocker'

export default function Form() {
  const { setIsBlocked } = useNavigationBlocker()

  return (
    <form
      onSubmit={(e) => {
        e.preventDefault()
        setIsBlocked(false)
      }}
      onChange={() => setIsBlocked(true)}
    >
      <input type="text" name="name" />
      <button type="submit">Save</button>
    </form>
  )
}
```

LANGUAGE: JavaScript
CODE:
```
'use client'

import { useNavigationBlocker } from '../contexts/navigation-blocker'

export default function Form() {
  const { setIsBlocked } = useNavigationBlocker()

  return (
    <form
      onSubmit={(e) => {
        e.preventDefault()
        setIsBlocked(false)
      }}
      onChange={() => setIsBlocked(true)}
    >
      <input type="text" name="name" />
      <button type="submit">Save</button>
    </form>
  )
}
```

----------------------------------------

TITLE: Loading Variable Google Font in Next.js App Router
DESCRIPTION: This snippet demonstrates how to load a variable Google Font (`Inter`) using `next/font/google` in a Next.js App Router `layout.tsx`/`layout.js` file. It initializes the font with `latin` subsets and `swap` display, then applies it globally to the `<html>` element via `className`. This approach automatically handles font optimization without requiring explicit weight specification.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/05-api-reference/02-components/font.mdx#_snippet_17

LANGUAGE: tsx
CODE:
```
import { Inter } from 'next/font/google'

// If loading a variable font, you don't need to specify the font weight
const inter = Inter({
  subsets: ['latin'],
  display: 'swap',
})

export default function RootLayout({
  children,
}: {
  children: React.ReactNode
}) {
  return (
    <html lang="en" className={inter.className}>
      <body>{children}</body>
    </html>
  )
}
```

LANGUAGE: jsx
CODE:
```
import { Inter } from 'next/font/google'

// If loading a variable font, you don't need to specify the font weight
const inter = Inter({
  subsets: ['latin'],
  display: 'swap',
})

export default function RootLayout({ children }) {
  return (
    <html lang="en" className={inter.className}>
      <body>{children}</body>
    </html>
  )
}
```

----------------------------------------

TITLE: Nesting SubmitButton Component in a Form (TypeScript)
DESCRIPTION: This snippet demonstrates how to integrate the `SubmitButton` component, which uses `useFormStatus`, directly into a form. The `SubmitButton` automatically inherits the pending state from the `form` element it is nested within, simplifying UI state management.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/02-guides/forms.mdx#_snippet_13

LANGUAGE: TypeScript
CODE:
```
import { SubmitButton } from './button'
import { createUser } from '@/app/actions'

export function Signup() {
  return (
    <form action={createUser}>
      {/* Other form elements */}
      <SubmitButton />
    </form>
  )
}
```

----------------------------------------

TITLE: Correct Next.js Link Usage: Including Children - JSX
DESCRIPTION: This snippet illustrates the correct implementation of the next/link component in Next.js by including a child element. It provides two valid methods: directly passing text as a child, or wrapping an <a> tag as a child when legacyBehavior is enabled. This resolves the 'no children' error.
SOURCE: https://github.com/vercel/next.js/blob/canary/errors/link-no-children.mdx#_snippet_1

LANGUAGE: JavaScript
CODE:
```
import Link from 'next/link'

export default function Home() {
  return (
    <>
      <Link href="/about">To About</Link>
      // or
      <Link href="/about" legacyBehavior>
        <a>To About</a>
      </Link>
    </>
  )
}
```

----------------------------------------

TITLE: Creating Responsive Images with Static Imports in Next.js
DESCRIPTION: This example demonstrates how to make a statically imported image responsive in Next.js. By importing the image, Next.js automatically infers its width and height, and applying `sizes="100vw"` and `style={{ width: '100%', height: 'auto' }}` ensures the image scales to fill its parent while preserving its aspect ratio.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/05-api-reference/02-components/image.mdx#_snippet_46

LANGUAGE: jsx
CODE:
```
import Image from 'next/image'
import mountains from '../public/mountains.jpg'

export default function Responsive() {
  return (
    <div style={{ display: 'flex', flexDirection: 'column' }}>
      <Image
        alt="Mountains"
        // Importing an image will
        // automatically set the width and height
        src={mountains}
        sizes="100vw"
        // Make the image display full width
        // and preserve its aspect ratio
        style={{
          width: '100%',
          height: 'auto',
        }}
      />
    </div>
  )
}
```

----------------------------------------

TITLE: Revalidating a Page Path - Next.js Cache - TypeScript
DESCRIPTION: This snippet illustrates revalidating a dynamic page path, such as `/blog/[slug]`, using the 'page' type. This invalidates any URL matching the provided page file, including those within route groups, but does not affect pages nested deeper than the specified path.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/05-api-reference/04-functions/revalidatePath.mdx#_snippet_2

LANGUAGE: ts
CODE:
```
import { revalidatePath } from 'next/cache'
revalidatePath('/blog/[slug]', 'page')
// or with route groups
revalidatePath('/(main)/blog/[slug]', 'page')
```

----------------------------------------

TITLE: Setting Request and Response Headers in Next.js Middleware (JavaScript)
DESCRIPTION: This snippet illustrates how to modify both incoming request headers and outgoing response headers within a Next.js middleware function. It shows cloning existing request headers and setting new ones via `NextResponse.next()` and directly on the `response.headers` object.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/03-building-your-application/01-routing/14-middleware.mdx#_snippet_11

LANGUAGE: JavaScript
CODE:
```
import { NextResponse } from 'next/server'

export function middleware(request) {
  // Clone the request headers and set a new header `x-hello-from-middleware1`
  const requestHeaders = new Headers(request.headers)
  requestHeaders.set('x-hello-from-middleware1', 'hello')

  // You can also set request headers in NextResponse.next
  const response = NextResponse.next({
    request: {
      // New request headers
      headers: requestHeaders,
    },
  })

  // Set a new response header `x-hello-from-middleware2`
  response.headers.set('x-hello-from-middleware2', 'hello')
  return response
}
```

----------------------------------------

TITLE: Redirecting with Modified URL Parameters using NextResponse in TypeScript
DESCRIPTION: This snippet demonstrates how to dynamically construct and modify a URL before performing a redirect with `NextResponse.redirect()`. It uses `request.nextUrl.pathname` to capture the original path and append it as a 'from' search parameter to a new login URL, useful for post-login redirection.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/05-api-reference/04-functions/next-response.mdx#_snippet_7

LANGUAGE: TypeScript
CODE:
```
import { NextResponse } from 'next/server'

// Given an incoming request...
const loginUrl = new URL('/login', request.url)
// Add ?from=/incoming-url to the /login URL
loginUrl.searchParams.set('from', request.nextUrl.pathname)
// And redirect to the new URL
return NextResponse.redirect(loginUrl)
```

----------------------------------------

TITLE: Generating Static Params for Dynamic Blog Slugs (Next.js)
DESCRIPTION: This snippet demonstrates how to use `generateStaticParams` to pre-render dynamic blog post pages. It fetches a list of posts from an API and returns an array of `slug` parameters, allowing Next.js to statically generate a page for each post at build time. The `Page` component then receives these pre-generated `params`.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/05-api-reference/04-functions/generate-static-params.mdx#_snippet_0

LANGUAGE: tsx
CODE:
```
// Return a list of `params` to populate the [slug] dynamic segment
export async function generateStaticParams() {
  const posts = await fetch('https://.../posts').then((res) => res.json())

  return posts.map((post) => ({
    slug: post.slug,
  }))
}

// Multiple versions of this page will be statically generated
// using the `params` returned by `generateStaticParams`
export default async function Page({
  params,
}: {
  params: Promise<{ slug: string }>
}) {
  const { slug } = await params
  // ...
}
```

LANGUAGE: jsx
CODE:
```
// Return a list of `params` to populate the [slug] dynamic segment
export async function generateStaticParams() {
  const posts = await fetch('https://.../posts').then((res) => res.json())

  return posts.map((post) => ({
    slug: post.slug,
  }))
}

// Multiple versions of this page will be statically generated
// using the `params` returned by `generateStaticParams`
export default async function Page({ params }) {
  const { slug } = await params
  // ...
}
```

----------------------------------------

TITLE: Generating Static Params in Next.js App Directory with generateStaticParams
DESCRIPTION: This code illustrates the `generateStaticParams` function in the Next.js `app` directory, which replaces `getStaticPaths`. It returns an array of segment objects to define pre-rendered routes and fetches post data asynchronously using `getPost` before rendering the `PostLayout` component.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/02-guides/migrating/app-router-migration.mdx#_snippet_31

LANGUAGE: JSX
CODE:
```
// `app` directory
import PostLayout from '@/components/post-layout'

export async function generateStaticParams() {
  return [{ id: '1' }, { id: '2' }]
}

async function getPost(params) {
  const res = await fetch(`https://.../posts/${(await params).id}`)
  const post = await res.json()

  return post
}

export default async function Post({ params }) {
  const post = await getPost(params)

  return <PostLayout post={post} />
}
```

----------------------------------------

TITLE: Configuring TypeScript `include` for Next.js (JSON)
DESCRIPTION: This JSON snippet shows the recommended `include` array for `tsconfig.json` in a Next.js project. It ensures that Next.js's environment declaration file (`next-env.d.ts`) and application source files (`app/**/*`, `src/**/*`) are included for proper TypeScript compilation and type checking.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/02-guides/migrating/from-create-react-app.mdx#_snippet_27

LANGUAGE: json
CODE:
```
{
  "include": ["next-env.d.ts", "app/**/*", "src/**/*"]
}
```

----------------------------------------

TITLE: Creating User Session in Next.js (JavaScript)
DESCRIPTION: This JavaScript function `createSession` sets an HttpOnly, Secure, SameSite=lax cookie for user sessions in a Next.js application. It takes a `userId`, encrypts the session data, and sets an expiration date for the cookie, ensuring server-side only access and secure transmission.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/02-guides/authentication.mdx#_snippet_19

LANGUAGE: JavaScript
CODE:
```
import 'server-only'
import { cookies } from 'next/headers'

export async function createSession(userId) {
  const expiresAt = new Date(Date.now() + 7 * 24 * 60 * 60 * 1000)
  const session = await encrypt({ userId, expiresAt })
  const cookieStore = await cookies()

  cookieStore.set('session', session, {
    httpOnly: true,
    secure: true,
    expires: expiresAt,
    sameSite: 'lax',
    path: '/',
  })
}
```

----------------------------------------

TITLE: Memoizing Fetch Requests in Next.js with JavaScript
DESCRIPTION: This JavaScript snippet illustrates Next.js's automatic memoization of `fetch` requests. The `getItem` function, when invoked multiple times with identical URL and options within a single React render pass, executes the network call only once, serving cached data for subsequent calls to optimize performance.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/04-deep-dive/caching.mdx#_snippet_1

LANGUAGE: jsx
CODE:
```
async function getItem() {
  // The `fetch` function is automatically memoized and the result
  // is cached
  const res = await fetch('https://.../item/1')
  return res.json()
}

// This function is called twice, but only executed the first time
const item = await getItem() // cache MISS

// The second call could be anywhere in your route
const item = await getItem() // cache HIT
```

----------------------------------------

TITLE: Understanding Server Action Security and Dead Code Elimination (JavaScript/JSX)
DESCRIPTION: This snippet illustrates Next.js's built-in security features for Server Actions. It demonstrates that actions used in the application receive secure, encrypted IDs for client-side invocation, while unused actions are automatically removed during the build process via dead code elimination, preventing them from becoming public endpoints.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/03-building-your-application/02-data-fetching/03-server-actions-and-mutations.mdx#_snippet_17

LANGUAGE: jsx
CODE:
```
// app/actions.js
'use server'

// This action **is** used in our application, so Next.js
// will create a secure ID to allow the client to reference
// and call the Server Action.
export async function updateUserAction(formData) {}

// This action **is not** used in our application, so Next.js
// will automatically remove this code during `next build`
// and will not create a public endpoint.
export async function deleteUserAction(formData) {}
```

----------------------------------------

TITLE: Fetching Data with getServerSideProps in Next.js (JavaScript)
DESCRIPTION: This snippet illustrates how to implement `getServerSideProps` in a Next.js page component using JavaScript to fetch data from an external API (GitHub) on each request. The retrieved `repo` data is then provided as props to the `Page` component, enabling the page to display dynamic content based on the server-side fetched information.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/02-pages/03-building-your-application/03-data-fetching/03-get-server-side-props.mdx#_snippet_1

LANGUAGE: jsx
CODE:
```
export async function getServerSideProps() {
  // Fetch data from external API
  const res = await fetch('https://api.github.com/repos/vercel/next.js')
  const repo = await res.json()
  // Pass data to the page via props
  return { props: { repo } }
}

export default function Page({ repo }) {
  return (
    <main>
      <p>{repo.stargazers_count}</p>
    </main>
  )
}
```

----------------------------------------

TITLE: Static Data Fetching with fetch in Next.js App
DESCRIPTION: Demonstrates data fetching in the `app` directory using the native `fetch()` API. By default, `fetch()` requests are cached (`cache: 'force-cache'`), similar to `getStaticProps`, making it suitable for static data generation in Server Components.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/02-guides/migrating/app-router-migration.mdx#_snippet_29

LANGUAGE: JSX
CODE:
```
// `app` directory

// This function can be named anything
async function getProjects() {
  const res = await fetch(`https://...`)
  const projects = await res.json()

  return projects
}

export default async function Index() {
  const projects = await getProjects()

  return projects.map((project) => <div>{project.name}</div>)
}
```

----------------------------------------

TITLE: Updating searchParams in Next.js Client Components
DESCRIPTION: This example demonstrates how to update URL searchParams within a Next.js Client Component using both the useRouter hook and the Link component. It includes a useCallback helper, createQueryString, to safely merge new key-value pairs with existing search parameters, ensuring the URL is correctly updated upon navigation.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/05-api-reference/04-functions/use-search-params.mdx#_snippet_6

LANGUAGE: TypeScript
CODE:
```
'use client'\n\nexport default function ExampleClientComponent() {\n  const router = useRouter()\n  const pathname = usePathname()\n  const searchParams = useSearchParams()\n\n  // Get a new searchParams string by merging the current\n  // searchParams with a provided key/value pair\n  const createQueryString = useCallback(\n    (name: string, value: string) => {\n      const params = new URLSearchParams(searchParams.toString())\n      params.set(name, value)\n\n      return params.toString()\n    },\n    [searchParams]\n  )\n\n  return (\n    <>\n      <p>Sort By</p>\n\n      {/* using useRouter */}\n      <button\n        onClick={() => {\n          // <pathname>?sort=asc\n          router.push(pathname + '?' + createQueryString('sort', 'asc'))\n        }}\n      >\n        ASC\n      </button>\n\n      {/* using <Link> */}\n      <Link\n        href={\n          // <pathname>?sort=desc\n          pathname + '?' + createQueryString('sort', 'desc')\n        }\n      >\n        DESC\n      </Link>\n    </>\n  )\n}
```

LANGUAGE: JavaScript
CODE:
```
'use client'\n\nexport default function ExampleClientComponent() {\n  const router = useRouter()\n  const pathname = usePathname()\n  const searchParams = useSearchParams()\n\n  // Get a new searchParams string by merging the current\n  // searchParams with a provided key/value pair\n  const createQueryString = useCallback(\n    (name, value) => {\n      const params = new URLSearchParams(searchParams)\n      params.set(name, value)\n\n      return params.toString()\n    },\n    [searchParams]\n  )\n\n  return (\n    <>\n      <p>Sort By</p>\n\n      {/* using useRouter */}\n      <button\n        onClick={() => {\n          // <pathname>?sort=asc\n          router.push(pathname + '?' + createQueryString('sort', 'asc'))\n        }}\n      >\n        ASC\n      </button>\n\n      {/* using <Link> */}\n      <Link\n        href={\n          // <pathname>?sort=desc\n          pathname + '?' + createQueryString('sort', 'desc')\n        }\n      >\n        DESC\n      </Link>\n    </>\n  )\n}
```

----------------------------------------

TITLE: Using useSearchParams in a Client Component (Next.js)
DESCRIPTION: This component, marked as a client component, demonstrates the use of `useSearchParams` to retrieve a query parameter. When used in a statically rendered page without a Suspense boundary, this component's dynamic nature causes the entire page to deopt into client-side rendering.
SOURCE: https://github.com/vercel/next.js/blob/canary/errors/deopted-into-client-rendering.mdx#_snippet_0

LANGUAGE: tsx
CODE:
```
'use client'

import { useSearchParams } from 'next/navigation'

export default function SearchBar() {
  const searchParams = useSearchParams()

  const search = searchParams.get('search')

  // This will not be logged on the server when using static rendering
  console.log(search)

  return <>Search: {search}</>
}
```

----------------------------------------

TITLE: Revalidating a Specific Path in Next.js (JSX)
DESCRIPTION: This snippet demonstrates the use of `revalidatePath` to manually revalidate data and re-render route segments below a specified path (e.g., '/'). This operation purges both the Data Cache and the Full Route Cache, ensuring that the specified route and its children are refreshed with the latest data.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/04-deep-dive/caching.mdx#_snippet_10

LANGUAGE: JSX
CODE:
```
revalidatePath('/')
```

----------------------------------------

TITLE: Getting a Specific Cookie Value with NextResponse in TypeScript
DESCRIPTION: This snippet illustrates how to retrieve the value of a specific cookie from the `NextResponse` object using `response.cookies.get('name')`. It returns the cookie object containing its name, value, and path. If the cookie is not found, `undefined` is returned.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/05-api-reference/04-functions/next-response.mdx#_snippet_1

LANGUAGE: TypeScript
CODE:
```
// Given incoming request /home
let response = NextResponse.next()
// { name: 'show-banner', value: 'false', Path: '/home' }
response.cookies.get('show-banner')
```

----------------------------------------

TITLE: Configuring Single Path Matcher in Next.js Middleware (JavaScript)
DESCRIPTION: This snippet demonstrates how to configure a Next.js Middleware to run on a single path using the `matcher` property in the `config` export. The `/about/:path*` pattern matches any request path starting with `/about`, including subpaths, allowing for flexible routing.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/03-building-your-application/01-routing/14-middleware.mdx#_snippet_2

LANGUAGE: JavaScript
CODE:
```
export const config = {
  matcher: '/about/:path*'
}
```

----------------------------------------

TITLE: Importing and Using CSS Modules (JSX)
DESCRIPTION: Shows how to import a CSS Module file (e.g., extra.module.css) into a React component. The imported styles object is then used to apply scoped CSS classes to elements via the className prop, preventing style conflicts and avoiding manual link tags.
SOURCE: https://github.com/vercel/next.js/blob/canary/errors/no-css-tags.mdx#_snippet_1

LANGUAGE: jsx
CODE:
```
import styles from './extra.module.css'

export class Home {
  render() {
    return (
      <div>
        <button type="button" className={styles.active}>
          Open
        </button>
      </div>
    )
  }
}
```

----------------------------------------

TITLE: MongoDB Atlas Connection String Example
DESCRIPTION: This is an example of a MongoDB Atlas connection string. It includes placeholders for username, password, and the cluster address, along with query parameters for connection options. This string is essential for connecting the Next.js application to a MongoDB database.
SOURCE: https://github.com/vercel/next.js/blob/canary/examples/with-mongodb-mongoose/README.md#_snippet_3

LANGUAGE: text
CODE:
```
mongodb+srv://<username>:<password>@my-project-abc123.mongodb.net/test?retryWrites=true&w=majority
```

----------------------------------------

TITLE: Receiving Webhook Payloads in Next.js Route Handler (TypeScript)
DESCRIPTION: This snippet shows how to create a Next.js Route Handler to receive and process webhook payloads via a POST request. It reads the incoming request body as text using `request.text()` and includes basic error handling. Unlike API Routes in the Pages Router, `bodyParser` is not required for this functionality.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/03-building-your-application/01-routing/13-route-handlers.mdx#_snippet_20

LANGUAGE: ts
CODE:
```
export async function POST(request: Request) {
  try {
    const text = await request.text()
    // Process the webhook payload
  } catch (error) {
    return new Response(`Webhook error: ${error.message}`, {
      status: 400,
    })
  }

  return new Response('Success!', {
    status: 200,
  })
}
```

----------------------------------------

TITLE: Creating API Route for Form Submission (TypeScript)
DESCRIPTION: This TypeScript snippet defines a Next.js API Route (`/api/submit`) to handle form submissions. It asynchronously processes the request body, calls a `createItem` function (placeholder for database interaction), and responds with a 200 status and the created item's ID. It requires `NextApiRequest` and `NextApiResponse` types.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/02-pages/02-guides/forms.mdx#_snippet_0

LANGUAGE: ts
CODE:
```
import type { NextApiRequest, NextApiResponse } from 'next'

export default async function handler(
  req: NextApiRequest,
  res: NextApiResponse
) {
  const data = req.body
  const id = await createItem(data)
  res.status(200).json({ id })
}
```

----------------------------------------

TITLE: Migrated Dashboard Layout (Client Component)
DESCRIPTION: This snippet demonstrates migrating the `DashboardLayout` to a Client Component in the `app` directory. The `'use client'` directive marks it as a client-side component, allowing it to retain interactive behavior previously found in `pages` directory layouts. It still accepts `children` to render nested content.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/02-guides/migrating/app-router-migration.mdx#_snippet_11

LANGUAGE: jsx
CODE:
```
'use client' // this directive should be at top of the file, before any imports.

// This is a Client Component
export default function DashboardLayout({ children }) {
  return (
    <div>
      <h2>My Dashboard</h2>
      {children}
    </div>
  )
}
```

----------------------------------------

TITLE: Using Blurred Data URL for Image Placeholders in Next.js
DESCRIPTION: The `blurDataURL` prop provides a Data URL to be used as a blurred placeholder before the main image loads, typically with `placeholder="blur"`. It can be automatically generated for static images or manually provided for dynamic/remote images, with small image sizes recommended for performance.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/05-api-reference/02-components/image.mdx#_snippet_14

LANGUAGE: JSX
CODE:
```
<Image placeholder="blur" blurDataURL="..." />
```

----------------------------------------

TITLE: Creating a Page Component with Nav and Form in Next.js (TSX)
DESCRIPTION: This snippet illustrates the structure of a page component in app/page.tsx. It imports and renders Nav and Form components, demonstrating how to compose a page from smaller, reusable components. This setup is crucial for pages that might have interactive elements like forms, where navigation blocking could be applied.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/05-api-reference/02-components/link.mdx#_snippet_40

LANGUAGE: TSX
CODE:
```
import Nav from './components/nav'
import Form from './components/form'

export default function Page() {
  return (
    <div>
      <Nav />
      <main>
        <h1>Welcome to the Dashboard</h1>
        <Form />
      </main>
    </div>
  )
}
```

----------------------------------------

TITLE: Displaying User Data on Dashboard Page (Next.js)
DESCRIPTION: Defines an asynchronous React component for the dashboard page. It fetches user data using `getUser` and displays the user's name. This illustrates server-side data fetching directly within a page component, leveraging automatic deduping if `getUser` was already called in the layout.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/05-api-reference/03-file-conventions/layout.mdx#_snippet_11

LANGUAGE: typescript
CODE:
```
import { getUser } from '@/app/lib/data'
import { UserName } from '@/app/ui/user-name'

export default async function Page() {
  const user = await getUser('1')

  return (
    <div>
      <h1>Welcome {user.name}</h1>
    </div>
  )
}
```

LANGUAGE: javascript
CODE:
```
import { getUser } from '@/app/lib/data'
import { UserName } from '@/app/ui/user-name'

export default async function Page() {
  const user = await getUser('1')

  return (
    <div>
      <h1>Welcome {user.name}</h1>
    </div>
  )
}
```

----------------------------------------

TITLE: Configuring Next.js Middleware with Custom Matchers (TypeScript)
DESCRIPTION: This snippet demonstrates how to define a single root Middleware file (`middleware.ts`) in Next.js and use an exported `config` object with a `matcher` array to specify which routes the Middleware should apply to. This approach replaces nested Middleware files and allows for more efficient and explicit route filtering.
SOURCE: https://github.com/vercel/next.js/blob/canary/errors/middleware-upgrade-guide.mdx#_snippet_0

LANGUAGE: TypeScript
CODE:
```
import { NextResponse } from 'next/server'
import type { NextRequest } from 'next/server'

export function middleware(request: NextRequest) {
  return NextResponse.rewrite(new URL('/about-2', request.url))
}

// Supports both a single string value or an array of matchers
export const config = {
  matcher: ['/about/:path*', '/dashboard/:path*'],
}
```

----------------------------------------

TITLE: Using Templated Title in Next.js Child Page (JSX)
DESCRIPTION: This snippet shows how a child page's title is combined with the `title.template` defined in its parent layout. The `%s` placeholder is replaced by the child's title, resulting in a templated output.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/05-api-reference/04-functions/generate-metadata.mdx#_snippet_10

LANGUAGE: jsx
CODE:
```
export const metadata = {
  title: 'About',
}

// Output: <title>About | Acme</title>
```

----------------------------------------

TITLE: Managing Push Notification Subscriptions and UI in React
DESCRIPTION: This snippet provides the core logic for managing push notification subscriptions and unsubscriptions, sending test notifications, and rendering the corresponding UI. It includes the `subscribeToPush` (partial), `unsubscribeFromPush`, and `sendTestNotification` functions, along with the conditional rendering based on subscription status and browser support. It relies on `urlBase64ToUint8Array` and server actions like `subscribeUser`, `unsubscribeUser`, and `sendNotification`.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/02-guides/progressive-web-apps.mdx#_snippet_12

LANGUAGE: TypeScript
CODE:
```
      userVisibleOnly: true,
      applicationServerKey: urlBase64ToUint8Array(
        process.env.NEXT_PUBLIC_VAPID_PUBLIC_KEY!
      ),
    });
    setSubscription(sub);
    await subscribeUser(sub);
  }

  async function unsubscribeFromPush() {
    await subscription?.unsubscribe();
    setSubscription(null);
    await unsubscribeUser();
  }

  async function sendTestNotification() {
    if (subscription) {
      await sendNotification(message);
      setMessage('');
    }
  }

  if (!isSupported) {
    return <p>Push notifications are not supported in this browser.</p>;
  }

  return (
    <div>
      <h3>Push Notifications</h3>
      {subscription ? (
        <>
          <p>You are subscribed to push notifications.</p>
          <button onClick={unsubscribeFromPush}>Unsubscribe</button>
          <input
            type="text"
            placeholder="Enter notification message"
            value={message}
            onChange={(e) => setMessage(e.target.value)}
          />
          <button onClick={sendTestNotification}>Send Test</button>
        </>
      ) : (
        <>
          <p>You are not subscribed to push notifications.</p>
          <button onClick={subscribeToPush}>Subscribe</button>
        </>
      )}
    </div>
  );
}
```

----------------------------------------

TITLE: Configuring Next.js with next.config.mjs (ES Modules)
DESCRIPTION: This snippet shows how to use ECMAScript Modules (ESM) for Next.js configuration by naming the file `next.config.mjs`. It uses `export default` instead of `module.exports` to define the configuration object, suitable for projects requiring ESM.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/05-api-reference/05-config/01-next-config-js/index.mdx#_snippet_1

LANGUAGE: javascript
CODE:
```
// @ts-check

/**
 * @type {import('next').NextConfig}
 */
const nextConfig = {
  /* config options here */
}

export default nextConfig
```

----------------------------------------

TITLE: Configuring Parent Layout Title Template (TSX)
DESCRIPTION: This snippet shows a parent layout configuring a `title.template` which child segments would normally use. This template will be ignored by child segments that define `title.absolute`.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/05-api-reference/04-functions/generate-metadata.mdx#_snippet_11

LANGUAGE: tsx
CODE:
```
import type { Metadata } from 'next'

export const metadata: Metadata = {
  title: {
    template: '%s | Acme',
  },
}
```

----------------------------------------

TITLE: Importing Dashboard Layout into Server Layout (After Migration)
DESCRIPTION: This snippet shows how to import the migrated `DashboardLayout` (now a Client Component) into a new `layout.js` file within the `app` directory. This `layout.js` acts as a Server Component, wrapping the `children` with the `DashboardLayout`, effectively replacing the `getLayout` pattern with native nested layouts.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/02-guides/migrating/app-router-migration.mdx#_snippet_12

LANGUAGE: jsx
CODE:
```
import DashboardLayout from './DashboardLayout'

// This is a Server Component
export default function Layout({ children }) {
  return <DashboardLayout>{children}</DashboardLayout>
}
```

----------------------------------------

TITLE: Implementing a Single Shared Layout with Custom App in Next.js
DESCRIPTION: This snippet shows how to implement a single shared layout for an entire Next.js application using a Custom App (`pages/_app.js`). It imports the `Layout` component and wraps the `Component` (the current page) and its `pageProps` with it, ensuring the layout persists across page navigations and preserves component state.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/02-pages/03-building-your-application/01-routing/01-pages-and-layouts.mdx#_snippet_2

LANGUAGE: jsx
CODE:
```
import Layout from '../components/layout'

export default function MyApp({ Component, pageProps }) {
  return (
    <Layout>
      <Component {...pageProps} />
    </Layout>
  )
}
```

----------------------------------------

TITLE: Forwarding Stripe Webhooks to Local Server (Stripe CLI)
DESCRIPTION: This command uses the Stripe CLI to listen for incoming webhook events from Stripe and forward them to the specified local endpoint (`localhost:3000/api/webhooks`). This is crucial for testing webhook handling during local development, allowing the application to receive and process real-time payment events.
SOURCE: https://github.com/vercel/next.js/blob/canary/examples/with-stripe-typescript/README.md#_snippet_8

LANGUAGE: bash
CODE:
```
stripe listen --forward-to localhost:3000/api/webhooks
```

----------------------------------------

TITLE: Integrating Breadcrumbs Component in Next.js Layout
DESCRIPTION: This snippet illustrates how a Next.js layout can include a Client Component that displays dynamic pathname information. By embedding the `Breadcrumbs` Client Component, the layout can show the current path details, leveraging the Client Component's ability to re-render and access the latest pathname.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/05-api-reference/03-file-conventions/layout.mdx#_snippet_8

LANGUAGE: tsx
CODE:
```
import { Breadcrumbs } from '@/app/ui/Breadcrumbs'

export default function Layout({ children }) {
  return (
    <>
      <Breadcrumbs />
      <main>{children}</main>
    </>
  )
}
```

LANGUAGE: jsx
CODE:
```
import { Breadcrumbs } from '@/app/ui/Breadcrumbs'

export default function Layout({ children }) {
  return (
    <>
      <Breadcrumbs />
      <main>{children}</main>
    </>
  )
}
```

----------------------------------------

TITLE: Accessing Environment Variables in Next.js Components
DESCRIPTION: This JSX snippet illustrates how to access environment variables defined in `next.config.js` from a React component in Next.js. The `process.env.customKey` variable will be replaced with its configured value ('my-value') during the build process, making it available in the client-side bundle.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/05-api-reference/05-config/01-next-config-js/env.mdx#_snippet_1

LANGUAGE: JSX
CODE:
```
function Page() {
  return <h1>The value of customKey is: {process.env.customKey}</h1>
}

export default Page
```

----------------------------------------

TITLE: Installing Dependencies and Running Next.js in Development (npm)
DESCRIPTION: These commands first install all project dependencies using `npm install`, then start the Next.js development server using `npm run dev`. This makes the application accessible locally, typically at `http://localhost:3000`.
SOURCE: https://github.com/vercel/next.js/blob/canary/examples/cms-sitefinity/README.md#_snippet_4

LANGUAGE: bash
CODE:
```
npm install
npm run dev
```

----------------------------------------

TITLE: Configure Jest setup file (TypeScript)
DESCRIPTION: Configures Jest to use custom matchers from `@testing-library/jest-dom` by specifying a setup file that imports the library. This allows using matchers like `.toBeInTheDocument()` in tests.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/02-guides/testing/jest.mdx#_snippet_11

LANGUAGE: typescript
CODE:
```
setupFilesAfterEnv: ['<rootDir>/jest.setup.ts']
```

----------------------------------------

TITLE: Configuring i18n-aware Redirects in Next.js
DESCRIPTION: This snippet demonstrates how to define redirects in `next.config.js` for a Next.js application with i18n enabled. It shows examples of redirects that automatically handle locales, redirects that require manual locale prefixing using `locale: false`, and patterns for matching all locales or specific paths. The `source`, `destination`, `permanent`, and `locale` properties are key parameters.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/05-api-reference/05-config/01-next-config-js/redirects.mdx#_snippet_7

LANGUAGE: JavaScript
CODE:
```
module.exports = {
  i18n: {
    locales: ['en', 'fr', 'de'],
    defaultLocale: 'en',
  },

  async redirects() {
    return [
      {
        source: '/with-locale', // automatically handles all locales
        destination: '/another', // automatically passes the locale on
        permanent: false,
      },
      {
        // does not handle locales automatically since locale: false is set
        source: '/nl/with-locale-manual',
        destination: '/nl/another',
        locale: false,
        permanent: false,
      },
      {
        // this matches '/' since `en` is the defaultLocale
        source: '/en',
        destination: '/en/another',
        locale: false,
        permanent: false,
      },
      // it's possible to match all locales even when locale: false is set
      {
        source: '/:locale/page',
        destination: '/en/newpage',
        permanent: false,
        locale: false,
      },
      {
        // this gets converted to /(en|fr|de)/(.*) so will not match the top-level
        // `/` or `/fr` routes like /:path* would
        source: '/(.*)',
        destination: '/another',
        permanent: false,
      },
    ]
  },
}
```

----------------------------------------

TITLE: Enabling Partial Prerendering (PPR) in Next.js (JSX)
DESCRIPTION: This snippet demonstrates how to enable Partial Prerendering (PPR) for a Next.js layout or page by exporting the `experimental_ppr` constant. Setting it to `true` activates PPR, optimizing rendering performance. This option applies to both `layout.js` and `page.js` files.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/05-api-reference/03-file-conventions/route-segment-config.mdx#_snippet_1

LANGUAGE: jsx
CODE:
```
export const experimental_ppr = true
// true | false
```

----------------------------------------

TITLE: Displaying Static Images in Next.js with JSX
DESCRIPTION: This JSX code demonstrates how to display static images served from the `public` directory in a Next.js application. It utilizes the `next/image` component to render an image, dynamically constructing the `src` path based on an `id` prop. The `AvatarOfMe` function provides an example of how to use the `Avatar` component.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/05-api-reference/03-file-conventions/public-folder.mdx#_snippet_0

LANGUAGE: jsx
CODE:
```
import Image from 'next/image'

export function Avatar({ id, alt }) {
  return <Image src={`/avatars/${id}.png`} alt={alt} width="64" height="64" />
}

export function AvatarOfMe() {
  return <Avatar id="me" alt="A portrait of me" />
}
```

----------------------------------------

TITLE: Implementing Path Parameter Redirects in Next.js
DESCRIPTION: This snippet demonstrates how to use path parameters in Next.js redirects. The `:slug` parameter in the `source` path `/old-blog/:slug` captures a segment of the URL, which can then be reused in the `destination` path `/news/:slug`. This allows for dynamic redirection of paths like `/old-blog/hello-world` to `/news/hello-world`, maintaining the specific slug.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/05-api-reference/05-config/01-next-config-js/redirects.mdx#_snippet_1

LANGUAGE: JavaScript
CODE:
```
module.exports = {
  async redirects() {
    return [
      {
        source: '/old-blog/:slug',
        destination: '/news/:slug', // Matched parameters can be used in the destination
        permanent: true,
      },
    ]
  },
}
```

----------------------------------------

TITLE: Rendering Basic Not Found UI in Next.js
DESCRIPTION: This snippet demonstrates how to create a basic 'Not Found' page in Next.js using `not-found.js` or `not-found.tsx`. It renders a simple message and a link to return home when the `notFound()` function is thrown or an unmatched URL is accessed. This component does not accept any props.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/05-api-reference/03-file-conventions/not-found.mdx#_snippet_0

LANGUAGE: TypeScript
CODE:
```
import Link from 'next/link'

export default function NotFound() {
  return (
    <div>
      <h2>Not Found</h2>
      <p>Could not find requested resource</p>
      <Link href="/">Return Home</Link>
    </div>
  )
}
```

LANGUAGE: JavaScript
CODE:
```
import Link from 'next/link'

export default function NotFound() {
  return (
    <div>
      <h2>Not Found</h2>
      <p>Could not find requested resource</p>
      <Link href="/">Return Home</Link>
    </div>
  )
}
```

----------------------------------------

TITLE: Defining Static Metadata with JSX in Next.js
DESCRIPTION: This example demonstrates how to define static metadata for a Next.js page or layout using a `metadata` object in JSX. By exporting a `metadata` constant, you can set properties like `title` and `description` for SEO purposes, which Next.js then uses to generate the appropriate HTML `<head>` tags.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/01-getting-started/13-metadata-and-og-images.mdx#_snippet_2

LANGUAGE: JavaScript
CODE:
```
export const metadata = {
  title: 'My Blog',
  description: '...', 
}

export default function Page() {}
```

----------------------------------------

TITLE: Defining Metadata with Built-in SEO in App Directory (JavaScript)
DESCRIPTION: This snippet shows how to define page metadata, such as the title, using the new built-in SEO support in the Next.js `app` directory. It exports a `metadata` object to configure the page's head elements, replacing the need for `next/head`.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/02-guides/migrating/app-router-migration.mdx#_snippet_16

LANGUAGE: JavaScript
CODE:
```
export const metadata = {
  title: 'My Page Title',
}

export default function Page() {
  return '...'
}
```

----------------------------------------

TITLE: Escaping Special Characters in Rewrite Source Paths (JavaScript)
DESCRIPTION: This example highlights the importance of escaping special characters (like parentheses `(` and `)`) in the `source` path when they are meant to be matched literally rather than interpreted as regex operators. Using `\(` and `\)` ensures that paths like `/english(default)/something` are correctly matched and rewritten.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/05-api-reference/05-config/01-next-config-js/rewrites.mdx#_snippet_8

LANGUAGE: JavaScript
CODE:
```
module.exports = {
  async rewrites() {
    return [
      {
        // this will match `/english(default)/something` being requested
        source: '/english\\(default\\)/:slug',
        destination: '/en-us/:slug',
      },
    ]
  },
}
```

----------------------------------------

TITLE: Marking React Class Component as Client Component (After)
DESCRIPTION: Alternatively, this snippet demonstrates how to fix the error by keeping the Class Component but explicitly marking the file as a Client Component using the `'use client'` directive at the top. This allows the Class Component to be rendered.
SOURCE: https://github.com/vercel/next.js/blob/canary/errors/class-component-in-server-component.mdx#_snippet_2

LANGUAGE: jsx
CODE:
```
'use client'

export default class Page extends React.Component {
  render() {
    return <p>Hello world</p>
  }
}
```

----------------------------------------

TITLE: Programmatic Navigation with useRouter Hook in Next.js (TSX/JSX)
DESCRIPTION: This snippet illustrates how to use the `useRouter` hook for programmatic navigation within Next.js Client Components. It imports `useRouter` from `next/navigation` and uses `router.push('/dashboard')` to navigate to the dashboard when a button is clicked. This method is suitable for imperative navigation actions.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/03-building-your-application/01-routing/04-linking-and-navigating.mdx#_snippet_1

LANGUAGE: tsx
CODE:
```
'use client'

import { useRouter } from 'next/navigation'

export default function Page() {
  const router = useRouter()

  return (
    <button type="button" onClick={() => router.push('/dashboard')}>
      Dashboard
    </button>
  )
}
```

LANGUAGE: jsx
CODE:
```
'use client'

import { useRouter } from 'next/navigation'

export default function Page() {
  const router = useRouter()

  return (
    <button type="button" onClick={() => router.push('/dashboard')}>
      Dashboard
    </button>
  )
}
```

----------------------------------------

TITLE: Configuring `remotePatterns` for Remote Images in Next.js
DESCRIPTION: This snippet illustrates how to configure `remotePatterns` in `next.config.js` to securely allow optimization of remote images. This configuration enables specifying detailed URL patterns, including protocol, hostname, port, and pathname, to restrict image sources and enhance application security.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/05-api-reference/02-components/image.mdx#_snippet_51

LANGUAGE: js
CODE:
```
module.exports = {
  images: {
    remotePatterns: [
      {
        protocol: 'https',
        hostname: 's3.amazonaws.com',
        port: '',
        pathname: '/my-bucket/**',
        search: ''
      }
    ]
  }
}
```

----------------------------------------

TITLE: Nesting Functional Component with Link and React.forwardRef (Pages Router - TypeScript)
DESCRIPTION: Shows how to nest a functional React component as a child of `next/link` in the Pages Router. Similar to the App Router, it requires wrapping the child component with `React.forwardRef` and passing `passHref` and `legacyBehavior` props to `Link` to ensure the `href` is correctly propagated to the underlying `<a>` tag. The `MyButton` component receives `onClick`, `href`, and `ref` props, which are then passed to the native `<a>` element.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/05-api-reference/02-components/link.mdx#_snippet_21

LANGUAGE: tsx
CODE:
```
import Link from 'next/link'
import React from 'react'

// Define the props type for MyButton
interface MyButtonProps {
  onClick?: React.MouseEventHandler<HTMLAnchorElement>
  href?: string
}

// Use React.ForwardRefRenderFunction to properly type the forwarded ref
const MyButton: React.ForwardRefRenderFunction<
  HTMLAnchorElement,
  MyButtonProps
> = ({ onClick, href }, ref) => {
  return (
    <a href={href} onClick={onClick} ref={ref}>
      Click Me
    </a>
  )
}

// Use React.forwardRef to wrap the component
const ForwardedMyButton = React.forwardRef(MyButton)

export default function Home() {
  return (
    <Link href="/about" passHref legacyBehavior>
      <ForwardedMyButton />
    </Link>
  )
}
```

----------------------------------------

TITLE: Verifying User Session with DAL in Next.js
DESCRIPTION: This function verifies the user's session by retrieving and decrypting the session cookie. If the session is invalid or lacks a `userId`, the user is redirected to the login page. It uses React's `cache` API to memoize the result, preventing redundant checks during a render pass.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/02-guides/authentication.mdx#_snippet_32

LANGUAGE: TypeScript
CODE:
```
import 'server-only'

import { cookies } from 'next/headers'
import { decrypt } from '@/app/lib/session'

export const verifySession = cache(async () => {
  const cookie = (await cookies()).get('session')?.value
  const session = await decrypt(cookie)

  if (!session?.userId) {
    redirect('/login')
  }

  return { isAuth: true, userId: session.userId }
})
```

LANGUAGE: JavaScript
CODE:
```
import 'server-only'

import { cookies } from 'next/headers'
import { decrypt } from '@/app/lib/session'

export const verifySession = cache(async () => {
  const cookie = (await cookies()).get('session')?.value
  const session = await decrypt(cookie)

  if (!session.userId) {
    redirect('/login')
  }

  return { isAuth: true, userId: session.userId }
})
```

----------------------------------------

TITLE: Using useEffect in Client Component (Correct)
DESCRIPTION: This snippet demonstrates the corrected approach. By adding the `'use client'` directive at the top of the file, the component is marked as a Client Component, allowing the safe and correct usage of client-side hooks like `useEffect`.
SOURCE: https://github.com/vercel/next.js/blob/canary/errors/react-client-hook-in-server-component.mdx#_snippet_1

LANGUAGE: jsx
CODE:
```
'use client'

import { useEffect } from 'react'

export default function Example() {
  useEffect(() => {
    console.log('in useEffect')
  })
  return <p>Hello world</p>
}
```

----------------------------------------

TITLE: Implementing Server-Side Form Validation with Zod in Next.js (TypeScript)
DESCRIPTION: This snippet demonstrates server-side form validation using the Zod library within a Next.js Server Action. It defines a schema for an email field and uses `safeParse` to validate incoming `FormData`. If validation fails, it returns an object containing field-specific errors.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/01-app/02-guides/forms.mdx#_snippet_3

LANGUAGE: TypeScript
CODE:
```
'use server'

import { z } from 'zod'

const schema = z.object({
  email: z.string({
    invalid_type_error: 'Invalid Email',
  }),
})

export default async function createUser(formData: FormData) {
  const validatedFields = schema.safeParse({
    email: formData.get('email'),
  })

  // Return early if the form data is invalid
  if (!validatedFields.success) {
    return {
      errors: validatedFields.error.flatten().fieldErrors,
    }
  }

  // Mutate data
}
```

----------------------------------------

TITLE: Redirecting Users with res.redirect in Next.js API Routes (TypeScript)
DESCRIPTION: This snippet demonstrates how to redirect a user to a new URL using `res.redirect` within a Next.js API Route written in TypeScript. It performs an asynchronous operation (`addPost`), then redirects to a dynamic URL (`/post/${id}`) with a 307 (Temporary Redirect) status code. This is useful for post-mutation navigation.
SOURCE: https://github.com/vercel/next.js/blob/canary/docs/02-pages/02-guides/forms.mdx#_snippet_8

LANGUAGE: TypeScript
CODE:
```
import type { NextApiRequest, NextApiResponse } from 'next'

export default async function handler(
  req: NextApiRequest,
  res: NextApiResponse
) {
  const id = await addPost()
  res.redirect(307, `/post/${id}`)
}
```