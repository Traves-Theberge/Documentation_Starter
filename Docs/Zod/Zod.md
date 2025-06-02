TITLE: Defining and Parsing a Basic Zod Schema in TypeScript
DESCRIPTION: This snippet shows how to define a simple Zod object schema for a User with a name property. It then demonstrates parsing an untrusted input object against this schema, resulting in a validated and type-safe data object.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/index.mdx#_snippet_0

LANGUAGE: ts
CODE:
```
import { z } from "zod/v4";

const User = z.object({
  name: z.string(),
});

// some untrusted data...
const input = { /* stuff */ };

// the parsed result is validated and type safe!
const data = User.parse(input);

// so you can use it with confidence :)
console.log(data.name);
```

----------------------------------------

TITLE: Defining and Parsing a Basic Zod Object Schema in TypeScript
DESCRIPTION: This snippet demonstrates how to define a simple object schema using Zod's `z.object` and `z.string` methods. It then shows how to use the `.parse()` method to validate and type-safely extract data from an untrusted input object.
SOURCE: https://github.com/colinhacks/zod/blob/main/README.md#_snippet_0

LANGUAGE: ts
CODE:
```
import { z } from "zod/v4";

const User = z.object({
  name: z.string(),
});

// some untrusted data...
const input = { /* stuff */ };

// the parsed result is validated and type safe!
const data = User.parse(input);

// so you can use it with confidence :)
console.log(data.name);
```

----------------------------------------

TITLE: Defining a Zod Object Schema with String and Number Types in TypeScript
DESCRIPTION: This snippet illustrates how to define a Zod schema for a 'Player' object. It uses `z.object` to define the structure and `z.string` and `z.number` to specify the types for the `username` and `xp` properties, respectively.
SOURCE: https://github.com/colinhacks/zod/blob/main/README.md#_snippet_2

LANGUAGE: ts
CODE:
```
import { z } from "zod/v4"; 

const Player = z.object({ 
  username: z.string(),
  xp: z.number()
});
```

----------------------------------------

TITLE: Configure TypeScript Strict Mode for Zod
DESCRIPTION: Shows the required `compilerOptions` setting in `tsconfig.json` to enable strict mode, which is a prerequisite for using Zod effectively with TypeScript.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/packages/v3.mdx#_snippet_0

LANGUAGE: ts
CODE:
```
{
  // ...
  "compilerOptions": {
    // ...
    "strict": true
  }
}
```

----------------------------------------

TITLE: Installing Zod via npm
DESCRIPTION: This command shows the standard way to install the Zod library using the npm package manager. It adds Zod as a dependency to your project.
SOURCE: https://github.com/colinhacks/zod/blob/main/README.md#_snippet_1

LANGUAGE: sh
CODE:
```
npm install zod
```

----------------------------------------

TITLE: Parsing Data Synchronously with Zod (TypeScript)
DESCRIPTION: Explains how to use the `.parse()` method to validate data against a schema. If validation succeeds, it returns a deep clone of the input; otherwise, it throws a `ZodError`.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/basics.mdx#_snippet_2

LANGUAGE: ts
CODE:
```
Player.parse({ username: "billie", xp: 100 }); 
// => returns { username: "billie", xp: 100 }
```

----------------------------------------

TITLE: Parsing Data Synchronously with Zod .parse (TypeScript)
DESCRIPTION: Demonstrates how to use the .parse() method on a Zod schema. It shows successful parsing for valid data and how it throws an error for invalid data. The method returns the parsed value if successful.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/parsing.mdx#_snippet_0

LANGUAGE: TypeScript
CODE:
```
const stringSchema = z.string();

stringSchema.parse("fish"); // => returns "fish"
stringSchema.parse(12); // throws error
```

----------------------------------------

TITLE: Extracting Schema Type with z.infer (TypeScript)
DESCRIPTION: Demonstrates how to use `z.infer<typeof mySchema>` to extract the inferred TypeScript type from a simple Zod schema, enabling type-safe variable assignments.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/packages/v3.mdx#_snippet_144

LANGUAGE: TypeScript
CODE:
```
const A = z.string();
type A = z.infer<typeof A>; // string

const u: A = 12; // TypeError
const u: A = "asdf"; // compiles
```

----------------------------------------

TITLE: Making Schema Optional with .optional in TypeScript
DESCRIPTION: A convenience method that returns an optional version of a schema. This makes the schema accept `undefined` in addition to the base type.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/packages/v3.mdx#_snippet_131

LANGUAGE: TypeScript
CODE:
```
const optionalString = z.string().optional(); // string | undefined

// equivalent to
z.optional(z.string());
```

----------------------------------------

TITLE: Create and Infer Zod Object Schema Type
DESCRIPTION: Shows how to define an object schema with a string property using `z.object`, parse data against it, and use `z.infer` to extract the TypeScript type definition from the schema.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/packages/v3.mdx#_snippet_4

LANGUAGE: ts
CODE:
```
import { z } from "zod";

const User = z.object({
  username: z.string(),
});

User.parse({ username: "Ludwig" });

// extract the inferred type
type User = z.infer<typeof User>;
// { username: string }
```

----------------------------------------

TITLE: Defining Optional Properties with Zod (TypeScript)
DESCRIPTION: Shows the equivalent Zod code for defining a schema with required and optional properties, highlighting Zod's more concise and declarative syntax using `z.object` and chaining the `.optional()` method directly onto the schema definition.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/packages/v3.mdx#_snippet_154

LANGUAGE: ts
CODE:
```
const C = z.object({
  foo: z.string(),
  bar: z.number().optional(),
});

type C = z.infer<typeof C>;
// returns { foo: string; bar?: number | undefined }
```

----------------------------------------

TITLE: Adding Custom Validation with Zod's refine (TypeScript)
DESCRIPTION: Demonstrates how to use the `.refine` method to add a custom validation function to a schema, including providing a custom error message.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/packages/v3.mdx#_snippet_112

LANGUAGE: ts
CODE:
```
const myString = z.string().refine((val) => val.length <= 255, {
  message: "String can't be more than 255 characters",
});
```

----------------------------------------

TITLE: Parsing Data with a Zod Object Schema in TypeScript
DESCRIPTION: This example demonstrates using the `.parse()` method on a previously defined Zod schema (`Player`) to validate an input object. If the input matches the schema, the validated and type-safe data is returned.
SOURCE: https://github.com/colinhacks/zod/blob/main/README.md#_snippet_3

LANGUAGE: ts
CODE:
```
Player.parse({ username: "billie", xp: 100 }); 
// => returns { username: "billie", xp: 100 }
```

----------------------------------------

TITLE: Handling Zod Validation Errors with safeParse (TypeScript)
DESCRIPTION: To avoid a `try/catch` block, use the `.safeParse()` method which returns a result object. This snippet shows how to check the `success` property and access either the `error` (a `ZodError`) or the parsed `data`.
SOURCE: https://github.com/colinhacks/zod/blob/main/README.md#_snippet_6

LANGUAGE: TypeScript
CODE:
```
const result = Player.parse({ username: 42, xp: "100" });
if (!result.success) {
  result.error;   // ZodError instance
} else {
  result.data;    // { username: string; xp: number }
}
```

----------------------------------------

TITLE: Defining Zod Array Schema
DESCRIPTION: Demonstrates how to create a Zod schema for an array containing elements of a specific type, showing two equivalent syntaxes.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/api.mdx#_snippet_70

LANGUAGE: TypeScript
CODE:
```
const stringArray = z.array(z.string()); // or z.string().array()
```

LANGUAGE: TypeScript
CODE:
```
const stringArray = z.array(z.string());
```

----------------------------------------

TITLE: Making a Zod Schema Nullable (Zod)
DESCRIPTION: Illustrates how to modify an existing Zod schema to accept `null` inputs using either the `z.nullable()` utility function or the `.nullable()` method.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/api.mdx#_snippet_56

LANGUAGE: TypeScript
CODE:
```
z.nullable(z.literal("yoda")); // or z.literal("yoda").nullable()
```

----------------------------------------

TITLE: Using Zod .default()
DESCRIPTION: Demonstrates how Zod's `.default()` method provides a fallback value when the input is `undefined`. The default value must match the schema's *output* type and short-circuits the parsing process.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/api.mdx#_snippet_109

LANGUAGE: TypeScript
CODE:
```
const schema = z.string().transform(val => val.length).default(0);
schema.parse(undefined); // => 0
```

----------------------------------------

TITLE: Create and Parse Zod String Schema
DESCRIPTION: Demonstrates how to import Zod, define a basic string schema, and use the `parse` and `safeParse` methods to validate input, showing both success and failure cases.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/packages/v3.mdx#_snippet_3

LANGUAGE: ts
CODE:
```
import { z } from "zod";

// creating a schema for strings
const mySchema = z.string();

// parsing
mySchema.parse("tuna"); // => "tuna"
mySchema.parse(12); // => throws ZodError

// "safe" parsing (doesn't throw error if validation fails)
mySchema.safeParse("tuna"); // => { success: true; data: "tuna" }
mySchema.safeParse(12); // => { success: false; error: ZodError }
```

----------------------------------------

TITLE: Handling Zod Validation Errors with safeParse (TypeScript)
DESCRIPTION: Demonstrates using the `.safeParse()` method, which returns a result object indicating success or failure without throwing an error. The result is a discriminated union for easy type-safe handling.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/basics.mdx#_snippet_5

LANGUAGE: ts
CODE:
```
const result = Player.parse({ username: 42, xp: "100" });
if (!result.success) {
  result.error;   // ZodError instance
} else {
  result.data;    // { username: string; xp: number }
}
```

----------------------------------------

TITLE: Safely Parsing Data Synchronously with Zod .safeParse (TypeScript)
DESCRIPTION: Illustrates the .safeParse() method, which does not throw errors on validation failure. Instead, it returns an object indicating success or failure, containing either the parsed data or a ZodError instance. Examples show results for both valid and invalid input.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/parsing.mdx#_snippet_2

LANGUAGE: TypeScript
CODE:
```
stringSchema.safeParse(12);
// => { success: false; error: ZodError }

stringSchema.safeParse("billie");
// => { success: true; data: 'billie' }
```

----------------------------------------

TITLE: Using Zod Schema .parse() Method (TypeScript)
DESCRIPTION: Demonstrates the `.parse()` method available on all Zod schemas. This method validates the input data against the schema. If valid, it returns the parsed value (a deep clone); otherwise, it throws a Zod error.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/packages/v3.mdx#_snippet_106

LANGUAGE: TypeScript
CODE:
```
const stringSchema = z.string();

stringSchema.parse("fish"); // => returns "fish"
stringSchema.parse(12); // throws error
```

----------------------------------------

TITLE: Importing Zod v4 (TypeScript)
DESCRIPTION: Shows the specific import path "zod/v4" used during the transition period for Zod version 4.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/basics.mdx#_snippet_1

LANGUAGE: ts
CODE:
```
import { z } from "zod/v4"
```

----------------------------------------

TITLE: Inferring Type from Zod Object Schema (TypeScript)
DESCRIPTION: Demonstrates how to define a Zod object schema and extract its inferred TypeScript type using `z.infer<typeof schema>`. The extracted type can then be used for type-checking variables.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/basics.mdx#_snippet_7

LANGUAGE: TypeScript
CODE:
```
const Player = z.object({
  username: z.string(),
  xp: z.number()
});

// extract the inferred type
type Player = z.infer<typeof Player>;

// use it in your code
const player: Player = { username: "billie", xp: 100 };
```

----------------------------------------

TITLE: Accessing Zod v4 Error Issues (TypeScript)
DESCRIPTION: Demonstrates using safeParse with zod/v4 to handle potential validation errors. It shows how to access the error object when success is false and inspect the structured details within the .issues array, including the expected type, error code, path, and message.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/error-customization.mdx#_snippet_0

LANGUAGE: TypeScript
CODE:
```
import { z } from "zod/v4";

const result = z.string().safeParse(12); // { success: false, error: ZodError }
result.error.issues;
// [
//   {
//     expected: 'string',
//     code: 'invalid_type',
//     path: [],
//     message: 'Invalid input: expected string, received number'
//   }
// ]
```

----------------------------------------

TITLE: Using Zod's safeParse Method (TypeScript)
DESCRIPTION: Demonstrates the basic usage of the `.safeParse` method, which returns an object indicating success or failure instead of throwing an error on validation failure.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/packages/v3.mdx#_snippet_108

LANGUAGE: ts
CODE:
```
stringSchema.safeParse(12);
// => { success: false; error: ZodError }

stringSchema.safeParse("billie");
// => { success: true; data: 'billie' }
```

----------------------------------------

TITLE: Using Zod Mini Parsing Methods (TypeScript)
DESCRIPTION: Shows how to use the standard parsing methods (parse, safeParse, parseAsync, safeParseAsync) which are identical in Zod Mini and standard Zod v4.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/v4/index.mdx#_snippet_13

LANGUAGE: ts
CODE:
```
import { z } from "zod/v4-mini";

z.string().parse("asdf");
z.string().safeParse("asdf");
await z.string().parseAsync("asdf");
await z.string().safeParseAsync("asdf");
```

----------------------------------------

TITLE: Defining Zod Primitive Schemas (TypeScript)
DESCRIPTION: Demonstrates how to define schemas for basic primitive types like string, number, boolean, etc., using Zod.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/api.mdx#_snippet_0

LANGUAGE: ts
CODE:
```
import { z } from "zod/v4";

// primitive types
z.string();
z.number();
z.bigint();
z.boolean();
z.symbol();
z.undefined();
z.null();
```

----------------------------------------

TITLE: Installing Zod Library via Package Managers
DESCRIPTION: This snippet provides commands for installing the standard release of the Zod library using popular JavaScript/TypeScript package managers like npm, Deno, Yarn, Bun, and pnpm.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/index.mdx#_snippet_1

LANGUAGE: sh
CODE:
```
npm install zod       # npm
deno add npm:zod      # deno
yarn add zod          # yarn
bun add zod           # bun
pnpm add zod          # pnpm
```

----------------------------------------

TITLE: Defining Basic Object Schema and Inferring Type - Zod TypeScript
DESCRIPTION: Shows how to define a basic Zod object schema with required properties using z.object(). It also demonstrates how to extract the static TypeScript type inferred from the schema using z.infer.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/packages/v3.mdx#_snippet_52

LANGUAGE: typescript
CODE:
```
// all properties are required by default
const Dog = z.object({
  name: z.string(),
  age: z.number(),
});

// extract the inferred type like this
type Dog = z.infer<typeof Dog>;

// equivalent to:
type Dog = {
  name: string;
  age: number;
};
```

----------------------------------------

TITLE: Setting Default Values with Zod Default (TypeScript)
DESCRIPTION: Demonstrates the basic usage of the `.default()` method to provide a static fallback value that is used when the input value is `undefined`.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/packages/v3.mdx#_snippet_126

LANGUAGE: TypeScript
CODE:
```
const stringWithDefault = z.string().default("tuna");

stringWithDefault.parse(undefined); // => "tuna"
```

----------------------------------------

TITLE: Common Methods of Zod ZodType Base Class (TypeScript)
DESCRIPTION: This snippet lists various methods available on all Zod schema instances, inherited from the 'z.ZodType' base class. It categorizes methods for parsing data, applying refinements, wrapping schemas (e.g., optional, array), adding metadata, and utility functions.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/packages/zod.mdx#_snippet_2

LANGUAGE: TypeScript
CODE:
```
import { z } from "zod/v4";

const mySchema = z.string();

// parsing
mySchema.parse(data);
mySchema.safeParse(data);
mySchema.parseAsync(data);
mySchema.safeParseAsync(data);


// refinements
mySchema.refine(refinementFunc);
mySchema.superRefine(refinementFunc); // deprecated, use `.check()`
mySchema.overwrite(overwriteFunc);

// wrappers
mySchema.optional();
mySchema.nonoptional();
mySchema.nullable();
mySchema.nullish();
mySchema.default(defaultValue);
mySchema.array();
mySchema.or(otherSchema);
mySchema.transform(transformFunc);
mySchema.catch(catchValue);
mySchema.pipe(otherSchema);
mySchema.readonly();

// metadata and registries
mySchema.register(registry, metadata);
mySchema.describe(description);
mySchema.meta(metadata);

// utilities
mySchema.check(checkOrFunction);
mySchema.clone(def);
mySchema.brand<T>();
mySchema.isOptional(); // boolean
mySchema.isNullable(); // boolean
```

----------------------------------------

TITLE: Using Zod's safeParseAsync Method (TypeScript)
DESCRIPTION: Provides an example of using the asynchronous version of `safeParse`, which is necessary when the schema includes asynchronous validations or transforms.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/packages/v3.mdx#_snippet_110

LANGUAGE: ts
CODE:
```
await stringSchema.safeParseAsync("billie");
```

----------------------------------------

TITLE: Setting Static Default Value with Zod/Zod Mini TypeScript
DESCRIPTION: Demonstrates how to set a static default value for a schema using `.default()` in Zod or `z._default()` in Zod Mini. This value is used when the parsed input is `undefined`.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/api.mdx#_snippet_107

LANGUAGE: TypeScript
CODE:
```
const defaultTuna = z.string().default("tuna");

defaultTuna.parse(undefined); // => "tuna"
```

LANGUAGE: TypeScript
CODE:
```
const defaultTuna = z._default(z.string(), "tuna");

defaultTuna.parse(undefined); // => "tuna"
```

----------------------------------------

TITLE: Defining a Zod Schema (TypeScript)
DESCRIPTION: Demonstrates how to define a basic object schema using `z.object` with string and number types in both standard Zod and Zod Mini.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/basics.mdx#_snippet_0

LANGUAGE: ts
CODE:
```
import { z } from "zod/v4"; 

const Player = z.object({ 
  username: z.string(),
  xp: z.number()
});
```

LANGUAGE: ts
CODE:
```
import { z } from "zod/v4-mini"

const Player = z.object({ 
  username: z.string(),
  xp: z.number()
});
```

----------------------------------------

TITLE: Handling Zod Validation Errors with try/catch (TypeScript)
DESCRIPTION: When validation fails, the `.parse()` method will throw a `ZodError` instance. This snippet demonstrates catching the error and accessing the granular validation issues via `err.issues`.
SOURCE: https://github.com/colinhacks/zod/blob/main/README.md#_snippet_5

LANGUAGE: TypeScript
CODE:
```
try {
  Player.parse({ username: 42, xp: "100" });
} catch(err){
  if(error instanceof z.ZodError){
    err.issues; 
    /* [
      {
        expected: 'string',
        code: 'invalid_type',
        path: [ 'username' ],
        message: 'Invalid input: expected string'
      },
      {
        expected: 'number',
        code: 'invalid_type',
        path: [ 'xp' ],
        message: 'Invalid input: expected number'
      }
    ] */
  }
}
```

----------------------------------------

TITLE: Handling Zod .safeParse Results with Discriminated Union (TypeScript)
DESCRIPTION: Demonstrates how to effectively handle the result object returned by .safeParse(). It shows using the success property to check the validation outcome and access either the data or the error property based on the result.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/parsing.mdx#_snippet_3

LANGUAGE: TypeScript
CODE:
```
const result = stringSchema.safeParse("billie");
if (!result.success) {
  // handle error then return
  result.error;
} else {
  // do something
  result.data;
}
```

----------------------------------------

TITLE: Common String Validations in Zod
DESCRIPTION: Illustrates various common string validation methods available in Zod, such as checking length, pattern, start/end, inclusion, and case, for both standard Zod and Zod Mini.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/api.mdx#_snippet_10

LANGUAGE: ts
CODE:
```
z.string().max(5);
z.string().min(5);
z.string().length(5);
z.string().regex(/^[a-z]+$/);
z.string().startsWith("aaa");
z.string().endsWith("zzz");
z.string().includes("---");
z.string().uppercase();
z.string().lowercase();
```

LANGUAGE: ts zod/v4-mini
CODE:
```
z.string().check(z.max(5));
z.string().check(z.min(5));
z.string().check(z.length(5));
z.string().check(z.regex(/^[a-z]+$/));
z.string().check(z.startsWith("aaa"));
z.string().check(z.endsWith("zzz"));
z.string().check(z.includes("---"));
z.string().check(z.uppercase());
z.string().check(z.lowercase());
```

----------------------------------------

TITLE: Handling Zod Validation Errors with safeParseAsync (TypeScript)
DESCRIPTION: Shows how to use the asynchronous `.safeParseAsync()` method for schemas with async operations. It returns a Promise resolving to a result object similar to `.safeParse()`. 
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/basics.mdx#_snippet_6

LANGUAGE: ts
CODE:
```
const schema = z.string().refine(async (val) => val.length <= 8);

await schema.safeParseAsync("hello");
// => { success: true; data: "hello" }
```

----------------------------------------

TITLE: Making Schema Nullish with .nullish in TypeScript
DESCRIPTION: A convenience method that returns a "nullish" version of a schema. Nullish schemas will accept both `undefined` and `null`. This is equivalent to chaining `.nullable().optional()`.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/packages/v3.mdx#_snippet_133

LANGUAGE: TypeScript
CODE:
```
const nullishString = z.string().nullish(); // string | null | undefined

// equivalent to
z.string().nullable().optional();
```

----------------------------------------

TITLE: Creating Array Schema with .array in TypeScript
DESCRIPTION: A convenience method that returns an array schema for the given type. This creates a Zod schema that validates arrays where all elements conform to the base schema.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/packages/v3.mdx#_snippet_134

LANGUAGE: TypeScript
CODE:
```
const stringArray = z.string().array(); // string[]

// equivalent to
z.array(z.string());
```

----------------------------------------

TITLE: Defining a Basic Object Schema with Zod v4 (TypeScript)
DESCRIPTION: This snippet shows how to import the 'z' object from 'zod/v4' and define a basic object schema using 'z.object'. It demonstrates defining properties with specific types and constraints like string, number (integer, positive), and email.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/packages/zod.mdx#_snippet_0

LANGUAGE: TypeScript
CODE:
```
import { z } from "zod/v4";

const schema = z.object({
  name: z.string(),
  age: z.number().int().positive(),
  email: z.string().email(),
});
```

----------------------------------------

TITLE: Making a Zod Schema Optional (Zod)
DESCRIPTION: Illustrates how to modify an existing Zod schema to accept `undefined` inputs using either the `z.optional()` utility function or the `.optional()` method.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/api.mdx#_snippet_52

LANGUAGE: TypeScript
CODE:
```
z.optional(z.literal("yoda")); // or z.literal("yoda").optional()
```

----------------------------------------

TITLE: Using Top-Level String Format Functions in Zod
DESCRIPTION: Demonstrates the new top-level functions for common string formats like email, UUIDs, IPs, URLs, etc., available directly on the `z` module in Zod v4. These replace the deprecated method equivalents (`z.string().email()`).
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/v4/index.mdx#_snippet_32

LANGUAGE: TypeScript
CODE:
```
z.email();
z.uuidv4();
z.uuidv7();
z.uuidv8();
z.ipv4();
z.ipv6();
z.cidrv4();
z.cidrv6();
z.url();
z.e164();
z.base64();
z.base64url();
z.jwt();
z.ascii();
z.utf8();
z.lowercase();
z.iso.date();
z.iso.datetime();
z.iso.duration();
z.iso.time();
```

----------------------------------------

TITLE: Handling Enum Type Inference with Zod
DESCRIPTION: Highlights a common pitfall when defining Zod enums from variables and shows how to fix it using `as const` to ensure correct type inference of the literal string values.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/api.mdx#_snippet_44

LANGUAGE: ts
CODE:
```
const fish = ["Salmon", "Tuna", "Trout"];
  
const FishEnum = z.enum(fish);
type FishEnum = z.infer<typeof FishEnum>; // string
```

LANGUAGE: ts
CODE:
```
const fish = ["Salmon", "Tuna", "Trout"] as const;

const FishEnum = z.enum(fish);
type FishEnum = z.infer<typeof FishEnum>; // "Salmon" | "Tuna" | "Trout"
```

----------------------------------------

TITLE: Applying Array Validations in Zod
DESCRIPTION: Provides examples of common array-specific validations like minimum length, maximum length, and exact length using Zod methods.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/api.mdx#_snippet_72

LANGUAGE: TypeScript
CODE:
```
z.array(z.string()).min(5); // must contain 5 or more items
z.array(z.string()).max(5); // must contain 5 or fewer items
z.array(z.string()).length(5); // must contain 5 items exactly
```

LANGUAGE: TypeScript
CODE:
```
z.array(z.string()).check(z.minLength(5)); // must contain 5 or more items
z.array(z.string()).check(z.maxLength(5)); // must contain 5 or fewer items
z.array(z.string()).check(z.length(5)); // must contain 5 items exactly
```

----------------------------------------

TITLE: Setting Size Constraints on Zod Array Schemas (TypeScript)
DESCRIPTION: Illustrates using the .min(), .max(), and .length() methods to define minimum, maximum, or exact size constraints for Zod array schemas.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/packages/v3.mdx#_snippet_75

LANGUAGE: TypeScript
CODE:
```
z.string().array().min(5); // must contain 5 or more items
z.string().array().max(5); // must contain 5 or fewer items
z.string().array().length(5); // must contain 5 items exactly
```

----------------------------------------

TITLE: Asynchronous Refinement
DESCRIPTION: Demonstrates how to define an asynchronous refinement function, which is necessary for operations like checking database existence. Notes that `parseAsync` must be used with async refinements.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/api.mdx#_snippet_99

LANGUAGE: ts
CODE:
```
const userId = z.string().refine(async (id) => {
  // verify that ID exists in database
  return true;
});
```

----------------------------------------

TITLE: Making Schema Nullable with .nullable Method - Zod TypeScript
DESCRIPTION: Shows the alternative method syntax `.nullable()` called directly on a schema. This is equivalent to using the z.nullable() function and makes the schema accept the original type or null.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/packages/v3.mdx#_snippet_50

LANGUAGE: typescript
CODE:
```
const E = z.string().nullable(); // equivalent to nullableString
type E = z.infer<typeof E>; // string | null
```

----------------------------------------

TITLE: Creating Nullable Schema with z.nullable - Zod TypeScript
DESCRIPTION: Demonstrates how to create a schema that accepts a specific type or null using the z.nullable() function. It shows examples of parsing both a valid value and null.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/packages/v3.mdx#_snippet_49

LANGUAGE: typescript
CODE:
```
const nullableString = z.nullable(z.string());
nullableString.parse("asdf"); // => "asdf"
nullableString.parse(null); // => null
```

----------------------------------------

TITLE: Customizing Errors with Function (Zod 4)
DESCRIPTION: Using the unified `error` parameter with a function that inspects the issue code to provide specific error messages in Zod 4.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/v4/changelog.mdx#_snippet_6

LANGUAGE: typescript
CODE:
```
z.string({
  error: (issue) => {
    if (issue.code === "too_small") {
      return `Value must be >${issue.minimum}`
    }
  },
});
```

----------------------------------------

TITLE: Accessing Specific Field Errors from Flattened Zod Output (TypeScript)
DESCRIPTION: This snippet illustrates how to access the validation errors for specific fields from the `fieldErrors` object returned by `z.flattenError()`. It shows examples of accessing the error arrays for the 'string' and 'numbers' fields, demonstrating how to retrieve the list of validation messages associated with each field.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/error-formatting.mdx#_snippet_8

LANGUAGE: typescript
CODE:
```
flattened.fieldErrors.string; // => [ 'Invalid input: expected string, received number' ]
flattened.fieldErrors.numbers; // => [ 'Invalid input: expected number, received string' ]
```

----------------------------------------

TITLE: Define Zod Primitive and Empty Schemas
DESCRIPTION: Lists the various built-in Zod schemas for primitive JavaScript types (string, number, boolean, etc.), empty types (undefined, null, void), catch-all types (any, unknown), and the never type.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/packages/v3.mdx#_snippet_5

LANGUAGE: ts
CODE:
```
import { z } from "zod";

// primitive values
z.string();
z.number();
z.bigint();
z.boolean();
z.date();
z.symbol();

// empty types
z.undefined();
z.null();
z.void(); // accepts undefined

// catch-all types
// allows any value
z.any();
z.unknown();

// never type
// allows no values
z.never();
```

----------------------------------------

TITLE: Safely Parsing Data Asynchronously with Zod .safeParseAsync (TypeScript)
DESCRIPTION: Shows the asynchronous version of .safeParse(). This method is used when the schema involves asynchronous operations and you want to avoid throwing errors on validation failure, returning a promise that resolves to the success/failure result object.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/parsing.mdx#_snippet_4

LANGUAGE: TypeScript
CODE:
```
await stringSchema.safeParseAsync("billie");
```

----------------------------------------

TITLE: Common Number Validations in Zod
DESCRIPTION: Lists various built-in validation methods available for number schemas in Zod, including range checks (`gt`, `gte`, `lt`, `lte`), integer check (`int`), sign checks (`positive`, `nonnegative`, `negative`, `nonpositive`), divisibility (`multipleOf`), and finiteness/safety checks (`finite`, `safe`).
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/packages/v3.mdx#_snippet_27

LANGUAGE: TypeScript
CODE:
```
z.number().gt(5);
z.number().gte(5); // alias .min(5)
z.number().lt(5);
z.number().lte(5); // alias .max(5)

z.number().int(); // value must be an integer

z.number().positive(); //     > 0
z.number().nonnegative(); //  >= 0
z.number().negative(); //     < 0
z.number().nonpositive(); //  <= 0

z.number().multipleOf(5); // Evenly divisible by 5. Alias .step(5)

z.number().finite(); // value must be finite, not Infinity or -Infinity
z.number().safe(); // value must be between Number.MIN_SAFE_INTEGER and Number.MAX_SAFE_INTEGER
```

----------------------------------------

TITLE: Basic Email Validation in Zod
DESCRIPTION: Shows the simplest way to validate an email address using Zod's built-in `email()` method, which uses a strict default regex.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/api.mdx#_snippet_13

LANGUAGE: ts
CODE:
```
z.email();
```

----------------------------------------

TITLE: Piping Schema into Transform with Zod/Zod Mini TypeScript
DESCRIPTION: Illustrates how to combine a schema with a transform using the `pipe` method or function. This pattern is useful for validating data first, then transforming the validated output. The example pipes a string schema into a transform that calculates the string's length.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/api.mdx#_snippet_103

LANGUAGE: TypeScript
CODE:
```
const stringToLength = z.string().pipe(z.transform(val => val.length));

stringToLength.parse("hello"); // => 5
```

LANGUAGE: TypeScript
CODE:
```
const stringToLength = z.pipe(z.string(), z.transform(val => val.length));

z.parse(stringToLength, "hello"); // => 5
```

----------------------------------------

TITLE: Omitting Keys from Object Schema in Zod
DESCRIPTION: Shows how to create a new object schema by excluding a specified set of keys from an existing schema using `.omit()` in standard Zod or `z.omit()` in Zod Mini. The example omits the 'id' key.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/api.mdx#_snippet_65

LANGUAGE: TypeScript
CODE:
```
const RecipeNoId = Recipe.omit({ id: true });
```

LANGUAGE: TypeScript
CODE:
```
const RecipeNoId = z.omit(Recipe, { id: true });
```

----------------------------------------

TITLE: Defining Zod Discriminated Union
DESCRIPTION: Shows how to define a discriminated union schema in Zod using `z.discriminatedUnion()`, specifying the discriminator key and an array of object schemas for each variant.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/api.mdx#_snippet_78

LANGUAGE: ts
CODE:
```
const MyResult = z.discriminatedUnion("status", [
  z.object({ status: z.literal("success"), data: z.string() }),
  z.object({ status: z.literal("failed"), error: z.string() }),
]);
```

----------------------------------------

TITLE: Validating Optional URL Input with Union and Literal in Zod
DESCRIPTION: Illustrates how to validate an optional form input that should be either a valid URL, null, undefined, or an empty string. It uses `z.union` with `z.string().url().nullish()` and `z.literal("")` and demonstrates validation outcomes with `safeParse`.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/packages/v3.mdx#_snippet_80

LANGUAGE: TypeScript
CODE:
```
const optionalUrl = z.union([z.string().url().nullish(), z.literal("")]);

console.log(optionalUrl.safeParse(undefined).success); // true
console.log(optionalUrl.safeParse(null).success); // true
console.log(optionalUrl.safeParse("").success); // true
console.log(optionalUrl.safeParse("https://zod.dev").success); // true
console.log(optionalUrl.safeParse("not a valid url").success); // false
```

----------------------------------------

TITLE: Omitting Object Properties with .omit - Zod TypeScript
DESCRIPTION: Demonstrates how to create a new schema by excluding a specified subset of properties from an existing object schema using the `.omit()` method, similar to TypeScript's `Omit` utility.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/packages/v3.mdx#_snippet_59

LANGUAGE: typescript
CODE:
```
const NoIDRecipe = Recipe.omit({ id: true });

type NoIDRecipe = z.infer<typeof NoIDRecipe>;
// => { name: string, ingredients: string[] }
```

----------------------------------------

TITLE: Creating Basic Union Schema with z.union in Zod
DESCRIPTION: Demonstrates how to create a basic union schema in Zod using the `z.union` method to accept multiple types. It shows how to define a schema that accepts either a string or a number and provides examples of successful parsing.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/packages/v3.mdx#_snippet_78

LANGUAGE: TypeScript
CODE:
```
const stringOrNumber = z.union([z.string(), z.number()]);

stringOrNumber.parse("foo"); // passes
stringOrNumber.parse(14); // passes
```

----------------------------------------

TITLE: Handling Zod Async Validation with safeParseAsync (TypeScript)
DESCRIPTION: If your schema uses asynchronous APIs like `async` refinements or transforms, you must use the `.safeParseAsync()` method. This snippet demonstrates using it with an async `refine`.
SOURCE: https://github.com/colinhacks/zod/blob/main/README.md#_snippet_7

LANGUAGE: TypeScript
CODE:
```
const schema = z.string().refine(async (val) => val.length <= 8);

await schema.safeParseAsync("hello");
// => { success: true; data: "hello" }
```

----------------------------------------

TITLE: Parsing Data with Async Refinements using parseAsync in TypeScript
DESCRIPTION: This snippet shows how to use the `.parseAsync()` method when a Zod schema includes asynchronous operations, such as an `async` refinement. It validates a string input against a schema that asynchronously checks its length.
SOURCE: https://github.com/colinhacks/zod/blob/main/README.md#_snippet_4

LANGUAGE: ts
CODE:
```
const schema = z.string().refine(async (val) => val.length <= 8);

await schema.parseAsync("hello");
// => "hello"
```

----------------------------------------

TITLE: Zod String Validations and Transforms (TypeScript)
DESCRIPTION: Demonstrates various built-in validation and transformation methods available for Zod string schemas, including length constraints, format checks (email, URL, UUID, etc.), and case/whitespace manipulation.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/packages/v3.mdx#_snippet_12

LANGUAGE: typescript
CODE:
```
// validations
z.string().max(5);
z.string().min(5);
z.string().length(5);
z.string().email();
z.string().url();
z.string().emoji();
z.string().uuid();
z.string().nanoid();
z.string().cuid();
z.string().cuid2();
z.string().ulid();
z.string().regex(regex);
z.string().includes(string);
z.string().startsWith(string);
z.string().endsWith(string);
z.string().datetime(); // ISO 8601; by default only `Z` timezone allowed
z.string().ip(); // defaults to allow both IPv4 and IPv6
z.string().cidr(); // defaults to allow both IPv4 and IPv6

// transforms
z.string().trim(); // trim whitespace
z.string().toLowerCase(); // toLowerCase
z.string().toUpperCase(); // toUpperCase

// added in Zod 3.23
z.string().date(); // ISO date format (YYYY-MM-DD)
z.string().time(); // ISO time format (HH:mm:ss[.SSSSSS] or HH:mm)
z.string().duration(); // ISO 8601 duration
z.string().base64();
```

----------------------------------------

TITLE: Making Specific Object Properties Optional with .partial - Zod TypeScript
DESCRIPTION: Demonstrates how to use the `.partial()` method with an argument to make only a specified subset of properties optional, while keeping others required.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/packages/v3.mdx#_snippet_62

LANGUAGE: typescript
CODE:
```
const optionalEmail = user.partial({
  email: true,
});
/*
{
  email?: string | undefined;
  username: string
}
*/
```

----------------------------------------

TITLE: Creating Union Schema with .or in TypeScript
DESCRIPTION: A convenience method for creating union types. This combines the current schema with another schema, allowing data to match either type.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/packages/v3.mdx#_snippet_136

LANGUAGE: TypeScript
CODE:
```
const stringOrNumber = z.string().or(z.number()); // string | number

// equivalent to
z.union([z.string(), z.number()]);
```

----------------------------------------

TITLE: Inferring Input and Output Types with Zod Transform (TypeScript)
DESCRIPTION: Shows how to define a Zod schema that includes a `.transform()` operation, causing the input and output types to differ. It demonstrates extracting the distinct input type using `z.input<typeof schema>` and the output type using `z.output<typeof schema>` (which is equivalent to `z.infer` in this case).
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/basics.mdx#_snippet_8

LANGUAGE: TypeScript
CODE:
```
const mySchema = z.string().transform((val) => val.length);

type MySchemaIn = z.input<typeof mySchema>;
// => string

type MySchemaOut = z.output<typeof mySchema>; // equivalent to z.infer<typeof mySchema>
// number
```

----------------------------------------

TITLE: Validating Finite Numbers with Zod
DESCRIPTION: Explains the basic usage of `z.number()` for validating any finite number, excluding `NaN` and `Infinity`.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/api.mdx#_snippet_32

LANGUAGE: TypeScript
CODE:
```
const schema = z.number();

schema.parse(3.14);      // ✅
schema.parse(NaN);       // ❌
schema.parse(Infinity);  // ❌
```

----------------------------------------

TITLE: Making Schema Nullable with .nullable in TypeScript
DESCRIPTION: A convenience method that returns a nullable version of a schema. This makes the schema accept `null` in addition to the base type.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/packages/v3.mdx#_snippet_132

LANGUAGE: TypeScript
CODE:
```
const nullableString = z.string().nullable(); // string | null

// equivalent to
z.nullable(z.string());
```

----------------------------------------

TITLE: Creating Basic Union Schema with .or Method in Zod
DESCRIPTION: Shows an alternative, more convenient way to create a basic union schema in Zod using the `.or()` method chained onto an existing schema. This achieves the same result as `z.union` for two types.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/packages/v3.mdx#_snippet_79

LANGUAGE: TypeScript
CODE:
```
const stringOrNumber = z.string().or(z.number());
```

----------------------------------------

TITLE: Basic URL Validation in Zod
DESCRIPTION: Demonstrates how to validate a string as a WHATWG-compatible URL using Zod's `url()` method and provides examples of valid and invalid inputs.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/api.mdx#_snippet_20

LANGUAGE: ts
CODE:
```
const schema = z.url();

schema.parse("https://example.com"); // ✅
schema.parse("http://localhost"); // ✅

schema.parse("mailto:noreply@zod.dev"); // ❌
schema.parse("sup"); // ❌
```

----------------------------------------

TITLE: Parsing Data Asynchronously with Zod (TypeScript)
DESCRIPTION: Shows how to use the `.parseAsync()` method for schemas that include asynchronous operations like async refinements or transforms. This method returns a Promise.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/basics.mdx#_snippet_3

LANGUAGE: ts
CODE:
```
const schema = z.string().refine(async (val) => val.length <= 8);

await schema.parseAsync("hello");
// => "hello"
```

----------------------------------------

TITLE: Defining and Using Zod Union Schema
DESCRIPTION: Explains how to create a Zod schema representing a union type (logical OR) and demonstrates parsing values against the union schema.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/api.mdx#_snippet_75

LANGUAGE: TypeScript
CODE:
```
const stringOrNumber = z.union([z.string(), z.number()]);
// string | number

stringOrNumber.parse("foo"); // passes
stringOrNumber.parse(14); // passes
```

----------------------------------------

TITLE: Installing Zod Mini via npm
DESCRIPTION: Install the Zod library version 3.25.0 or higher to access the 'zod/v4-mini' variant.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/packages/mini.mdx#_snippet_0

LANGUAGE: sh
CODE:
```
npm install zod@^3.25.0
```

----------------------------------------

TITLE: Making Object Property Optional with .optional - Zod TypeScript
DESCRIPTION: Shows the convenient method syntax `.optional()` called directly on a schema within an object definition. This makes the specific property optional, allowing it to be present with the defined type or absent/undefined.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/packages/v3.mdx#_snippet_47

LANGUAGE: typescript
CODE:
```
const user = z.object({
  username: z.string().optional(),
});
type C = z.infer<typeof user>; // { username?: string | undefined };
```

----------------------------------------

TITLE: Inferring Zod Schema Type with z.infer (TypeScript)
DESCRIPTION: Zod can infer a static TypeScript type from your schema definitions. Use the `z.infer<>` utility to extract this type and use it for type annotations in your code.
SOURCE: https://github.com/colinhacks/zod/blob/main/README.md#_snippet_8

LANGUAGE: TypeScript
CODE:
```
const Player = z.object({ 
  username: z.string(),
  xp: z.number()
});

// extract the inferred type
type Player = z.infer<typeof Player>;

// use it in your code
const player: Player = { username: "billie", xp: 100 };
```

----------------------------------------

TITLE: Parsing Invalid Data and Inspecting Zod Error Issues (TypeScript)
DESCRIPTION: Attempts to parse invalid data against the defined schema using `safeParse`. The resulting `result.error!.issues` array contains detailed information about each validation failure, including the expected type, error code, path, and message.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/error-formatting.mdx#_snippet_1

LANGUAGE: ts
CODE:
```
const result = schema.safeParse({
  username: 1234,
  favoriteNumbers: [1234, "4567"],
  extraKey: 1234,
});

result.error!.issues;
```

----------------------------------------

TITLE: Using Function for Dynamic Zod Default (TypeScript)
DESCRIPTION: Shows how to pass a function to `.default()` to generate a new default value each time it is needed (i.e., when the input is `undefined`), useful for dynamic values like random numbers or timestamps.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/packages/v3.mdx#_snippet_127

LANGUAGE: TypeScript
CODE:
```
const numberWithRandomDefault = z.number().default(Math.random);

numberWithRandomDefault.parse(undefined); // => 0.4413456736055323
numberWithRandomDefault.parse(undefined); // => 0.1871840107401901
numberWithRandomDefault.parse(undefined); // => 0.7223408162401552
```

----------------------------------------

TITLE: Customizing Boolean Schema Error Messages in Zod
DESCRIPTION: Shows how to provide custom error messages for `required_error` and `invalid_type_error` when defining a boolean schema using `z.boolean()`. This allows tailoring error feedback for missing or incorrectly typed inputs.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/packages/v3.mdx#_snippet_31

LANGUAGE: TypeScript
CODE:
```
const isActive = z.boolean({
  required_error: "isActive is required",
  invalid_type_error: "isActive must be a boolean"
});
```

----------------------------------------

TITLE: Defining Basic Transform with Zod/Zod Mini TypeScript
DESCRIPTION: Demonstrates how to create a basic transform schema using `z.transform` in Zod and Zod Mini. This transform accepts any input and converts it to a string. Shows how to parse different input types.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/api.mdx#_snippet_101

LANGUAGE: TypeScript
CODE:
```
const castToString = z.transform((val) => String(val));

castToString.parse("asdf"); // => "asdf"
castToString.parse(123); // => "123"
castToString.parse(true); // => "true"
```

LANGUAGE: TypeScript
CODE:
```
const castToString = z.transform((val) => String(val));

z.parse(castToString, "asdf"); // => "asdf"
z.parse(castToString, 123); // => "123"
z.parse(castToString, true); // => "true"
```

----------------------------------------

TITLE: Validating IP Addresses with Zod
DESCRIPTION: Illustrates the default behavior of `z.string().ip()`, which validates strings as either IPv4 or IPv6 addresses. Includes examples of valid and invalid IP formats.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/packages/v3.mdx#_snippet_22

LANGUAGE: TypeScript
CODE:
```
const ip = z.string().ip();

ip.parse("192.168.1.1"); // pass
ip.parse("84d5:51a0:9114:1855:4cfa:f2d7:1f12:7003"); // pass
ip.parse("84d5:51a0:9114:1855:4cfa:f2d7:1f12:192.168.1.1"); // pass

ip.parse("256.1.1.1"); // fail
ip.parse("84d5:51a0:9114:gggg:4cfa:f2d7:1f12:7003"); // fail
```

----------------------------------------

TITLE: Chaining Transforms and Validations with Zod's .pipe() (TypeScript)
DESCRIPTION: Shows how to use the `.pipe()` method to chain a schema with a transform (`.transform()`) to another schema for subsequent validation of the transformed value.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/packages/v3.mdx#_snippet_143

LANGUAGE: TypeScript
CODE:
```
z.string()
  .transform((val) => val.length)
  .pipe(z.number().min(5));
```

----------------------------------------

TITLE: Enabling Passthrough for Unknown Keys in Zod Object (TypeScript)
DESCRIPTION: Demonstrates using the .passthrough() method on a Zod object schema to prevent the stripping of unknown keys during parsing, allowing them to remain in the output.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/packages/v3.mdx#_snippet_68

LANGUAGE: TypeScript
CODE:
```
person.passthrough().parse({
  name: "bob dylan",
  extraKey: 61,
});
// => { name: "bob dylan", extraKey: 61 }
```

----------------------------------------

TITLE: Accessing Zod Validation Issues (TypeScript)
DESCRIPTION: Demonstrates how to use `safeParse` to handle validation errors and access the detailed `issues` array from the `ZodError` object when validation fails, providing granular information about the validation problems.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/packages/v3.mdx#_snippet_151

LANGUAGE: ts
CODE:
```
const result = z
  .object({
    name: z.string(),
  })
  .safeParse({ name: 12 });

if (!result.success) {
  result.error.issues;
  /* [
      {
        "code": "invalid_type",
        "expected": "string",
        "received": "number",
        "path": [ "name" ],
        "message": "Expected string, received number"
      }
  ] */
}
```

----------------------------------------

TITLE: Handling Zod Validation Errors with try/catch (TypeScript)
DESCRIPTION: Illustrates how to catch `ZodError` instances thrown by the `.parse()` method when validation fails, providing access to detailed validation issues. Includes examples for both standard Zod and Zod Mini error types.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/basics.mdx#_snippet_4

LANGUAGE: ts
CODE:
```
try {
  Player.parse({ username: 42, xp: "100" });
} catch(err){
  if(error instanceof z.ZodError){
    err.issues; 
    /* [
      {
        expected: 'string',
        code: 'invalid_type',
        path: [ 'username' ],
        message: 'Invalid input: expected string'
      },
      {
        expected: 'number',
        code: 'invalid_type',
        path: [ 'xp' ],
        message: 'Invalid input: expected number'
      }
    ] */
  }
}
```

LANGUAGE: ts
CODE:
```
try {
  Player.parse({ username: 42, xp: "100" });
} catch(err){
  if(error instanceof z.core.$ZodError){
    err.issues; 
    /* [
      {
        expected: 'string',
        code: 'invalid_type',
        path: [ 'username' ],
        message: 'Invalid input: expected string'
      },
      {
        expected: 'number',
        code: 'invalid_type',
        path: [ 'xp' ],
        message: 'Invalid input: expected number'
      }
    ] */
  }
}
```

----------------------------------------

TITLE: Simple Zod Schema Definition and Parsing in TypeScript
DESCRIPTION: Shows a minimal example of importing Zod, defining a basic boolean schema, and parsing a value. This snippet is used to measure the core bundle size of Zod.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/v4/index.mdx#_snippet_10

LANGUAGE: ts
CODE:
```
import { z } from "zod/v4";

const schema = z.boolean();

schema.parse(true);
```

----------------------------------------

TITLE: Defining Schema-Level Error Message in Zod
DESCRIPTION: Sets a specific error message directly within the schema definition. This error message has the highest precedence among all customization methods.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/error-customization.mdx#_snippet_18

LANGUAGE: TypeScript
CODE:
```
z.string("Not a string!");
```

----------------------------------------

TITLE: Using Top-Level Zod String Format APIs (v4)
DESCRIPTION: Demonstrates the new top-level APIs for validating specific string formats like email, UUID, URL, etc., which replace the deprecated method forms on `z.string()` for better tree-shaking and less verbosity.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/v4/changelog.mdx#_snippet_16

LANGUAGE: TypeScript
CODE:
```
z.email();
z.uuid();
z.url();
z.emoji();         // validates a single emoji character
z.base64();
z.base64url();
z.nanoid();
z.cuid();
z.cuid2();
z.ulid();
z.ipv4();
z.ipv6();
z.cidrv4();          // ip range
z.cidrv6();          // ip range
z.iso.date();
z.iso.time();
z.iso.datetime();
z.iso.duration();
```

----------------------------------------

TITLE: Defining Zod Schemas with Extend in TypeScript
DESCRIPTION: Demonstrates defining simple Zod object schemas and extending one from another using `z.object` and `.extend()`. This pattern is used to show the reduction in `tsc` instantiations in Zod v4.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/v4/index.mdx#_snippet_7

LANGUAGE: ts
CODE:
```
import { z } from "zod/v4";

export const A = z.object({
  a: z.string(),
  b: z.string(),
  c: z.string(),
  d: z.string(),
  e: z.string(),
});

export const B = A.extend({
  f: z.string(),
  g: z.string(),
  h: z.string(),
});
```

----------------------------------------

TITLE: Basic UUID Validation in Zod
DESCRIPTION: Shows the basic usage of Zod's `uuid()` method to validate a string as a universally unique identifier.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/api.mdx#_snippet_17

LANGUAGE: ts
CODE:
```
z.uuid();
```

----------------------------------------

TITLE: Zod Discriminated Union with Explicit Key Example
DESCRIPTION: Provides an example of defining a Zod discriminated union schema, explicitly specifying the discriminator key as the first argument to `z.discriminatedUnion()`, as discussed in the documentation regarding automatic key detection.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/api.mdx#_snippet_79

LANGUAGE: ts
CODE:
```
const MyResult = z.discriminatedUnion("status", [
  z.object({ status: z.literal("success"), data: z.string() }),
  z.object({ status: z.literal("failed"), error: z.string() }),
]);
```

----------------------------------------

TITLE: Using z.stringbool() for String to Boolean Coercion in Zod
DESCRIPTION: Demonstrates the usage of the new `z.stringbool()` function for coercing specific string values (like "true", "yes", "1", "false", "no", "0") into boolean primitives. It shows examples of parsing various truthy and falsy string representations.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/v4/index.mdx#_snippet_37

LANGUAGE: TypeScript
CODE:
```
const strbool = z.stringbool();

strbool.parse("true")         // => true
strbool.parse("1")            // => true
strbool.parse("yes")          // => true
strbool.parse("on")           // => true
strbool.parse("y")            // => true
strbool.parse("enable")       // => true

strbool.parse("false");       // => false
strbool.parse("0");            // => false
strbool.parse("no");           // => false
strbool.parse("off");          // => false
strbool.parse("n");            // => false
strbool.parse("disabled");     // => false

strbool.parse(/* anything else */); // ZodError<[{ code: "invalid_value" }]>
```

----------------------------------------

TITLE: Creating Optional Schema with z.optional - Zod TypeScript
DESCRIPTION: Demonstrates how to wrap an existing schema to make it optional using the z.optional() function. The resulting schema will accept the original type or undefined. It also shows how to parse undefined and infer the resulting type.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/packages/v3.mdx#_snippet_46

LANGUAGE: typescript
CODE:
```
const schema = z.optional(z.string());

schema.parse(undefined); // => returns undefined
type A = z.infer<typeof schema>; // string | undefined
```

----------------------------------------

TITLE: Using Top-Level Zod Object Modifiers (v4)
DESCRIPTION: Demonstrates the new top-level functions `z.strictObject()` and `z.looseObject()` in Zod v4, which replace the deprecated `.strict()` and `.passthrough()` methods for defining object behavior regarding unknown keys.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/v4/changelog.mdx#_snippet_24

LANGUAGE: TypeScript
CODE:
```
// Zod 3
z.object({ name: z.string() }).strict();
z.object({ name: z.string() }).passthrough();

// Zod 4
z.strictObject({ name: z.string() });
z.looseObject({ name: z.string() });
```

----------------------------------------

TITLE: Defining Schemas for Template Literal Types with z.templateLiteral() in Zod
DESCRIPTION: Illustrates how to use the new `z.templateLiteral()` function to create Zod schemas that validate strings matching TypeScript template literal patterns. It shows examples combining literal strings, other schemas (like `z.string()`, `z.number()`), and enums.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/v4/index.mdx#_snippet_34

LANGUAGE: TypeScript
CODE:
```
const hello = z.templateLiteral(["hello, ", z.string()]);
// `hello, ${string}`

const cssUnits = z.enum(["px", "em", "rem", "%"]);
const css = z.templateLiteral([z.number(), cssUnits]);
// `${number}px` | `${number}em` | `${number}rem` | `${number}%`

const email = z.templateLiteral([
  z.string().min(1),
  "@",
  z.string().max(64),
]);
// `${string}@${string}` (the min/max refinements are enforced!)
```

----------------------------------------

TITLE: Transforming Data with Zod Transform (TypeScript)
DESCRIPTION: Basic example demonstrating the use of the `.transform()` method to modify the parsed value. The provided function receives the validated value and returns the transformed result.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/packages/v3.mdx#_snippet_121

LANGUAGE: TypeScript
CODE:
```
const stringToNumber = z.string().transform((val) => val.length);

stringToNumber.parse("string"); // => 6
```

----------------------------------------

TITLE: Defining Date Schema with Zod
DESCRIPTION: Illustrates how to create a Zod schema for `Date` instances and use `safeParse`. Note that Zod validates `Date` objects, not date strings.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/api.mdx#_snippet_40

LANGUAGE: ts
CODE:
```
z.date().safeParse(new Date()); // success: true
z.date().safeParse("2022-01-12T00:00:00.000Z"); // success: false
```

----------------------------------------

TITLE: Creating Zod Tuple Schemas (TypeScript)
DESCRIPTION: Shows how to create a Zod tuple schema with a fixed number of elements, each potentially having a different type, and displays the resulting inferred type.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/packages/v3.mdx#_snippet_76

LANGUAGE: TypeScript
CODE:
```
const athleteSchema = z.tuple([
  z.string(), // name
  z.number(), // jersey number
  z.object({
    pointsScored: z.number(),
  }), // statistics
]);

type Athlete = z.infer<typeof athleteSchema>;
// type Athlete = [string, number, { pointsScored: number }]
```

----------------------------------------

TITLE: Formatting Zod Validation Errors (TypeScript)
DESCRIPTION: Shows how to use the `.format()` method on a `ZodError` object obtained from `safeParse` to convert the validation issues into a nested, more user-friendly object structure, often useful for presenting errors to users.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/packages/v3.mdx#_snippet_152

LANGUAGE: ts
CODE:
```
const result = z
  .object({
    name: z.string(),
  })
  .safeParse({ name: 12 });

if (!result.success) {
  const formatted = result.error.format();
  /* {
    name: { _errors: [ 'Expected string, received number' ] }
  } */

  formatted.name?._errors;
  // => ["Expected string, received number"]
}
```

----------------------------------------

TITLE: Using Zod v4 .prefault() for Zod 3 Default Behavior
DESCRIPTION: Introduces the new `.prefault()` API in Zod v4, which replicates the Zod 3 `.default()` behavior by parsing the default value before applying transformations.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/v4/changelog.mdx#_snippet_23

LANGUAGE: TypeScript
CODE:
```
// Zod 3
const schema = z.string()
  .transform(val => val.length)
  .prefault("tuna");
schema.parse(undefined); // => 4
```

----------------------------------------

TITLE: Using parseAsync with Asynchronous Refinements
DESCRIPTION: Explains that schemas containing asynchronous refinements must be parsed using the `parseAsync` method (or `safeParseAsync`) and provides examples for both Zod and Zod Mini.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/api.mdx#_snippet_100

LANGUAGE: ts
CODE:
```
const result = await userId.parseAsync("abc123");
```

LANGUAGE: ts
CODE:
```
const result = await z.parseAsync(userId, "abc123");
```

----------------------------------------

TITLE: Parsing Data Asynchronously with Zod .parseAsync (TypeScript)
DESCRIPTION: Shows how to use the .parseAsync() method, necessary when the schema includes asynchronous operations like async refinements. It includes defining a schema with an async refinement and demonstrates successful and failed asynchronous parsing.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/parsing.mdx#_snippet_1

LANGUAGE: TypeScript
CODE:
```
const stringSchema = z.string().refine(async (val) => val.length <= 8);

await stringSchema.parseAsync("hello"); // => returns "hello"
await stringSchema.parseAsync("hello world"); // => throws error
```

----------------------------------------

TITLE: Custom Error Message for Refinement
DESCRIPTION: Shows how to provide a custom error message string when a refinement fails by passing an options object with the `error` property. Applicable to both Zod and Zod Mini.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/api.mdx#_snippet_94

LANGUAGE: ts
CODE:
```
const myString = z.string().refine((val) => val.length > 8, { 
  error: "Too short!" 
});
```

LANGUAGE: ts
CODE:
```
const myString = z.string().check(
  z.refine((val) => val.length > 8, { error: "Too short!" })
);
```

----------------------------------------

TITLE: Defining Enum Schema from TypeScript Enum with Zod
DESCRIPTION: Shows how to create a Zod enum schema directly from an existing TypeScript string enum. This is a convenient way to reuse existing enum definitions.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/api.mdx#_snippet_45

LANGUAGE: ts
CODE:
```
enum Fish {
  Salmon = "Salmon",
  Tuna = "Tuna",
  Trout = "Trout",
}

const FishEnum = z.enum(Fish);
```

----------------------------------------

TITLE: Validating String Starts With in Zod
DESCRIPTION: Demonstrates how to use the `startsWith` method to validate if a string begins with a specific substring in both standard Zod and Zod Mini.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/api.mdx#_snippet_8

LANGUAGE: ts
CODE:
```
z.string().startsWith("fourscore")
```

LANGUAGE: ts zod/v4-mini
CODE:
```
z.string().check(z.startsWith("fourscore"))
```

----------------------------------------

TITLE: Understanding Input, Output, and Inferred Types with Transforms (TypeScript)
DESCRIPTION: Explains the difference between input, output, and inferred types when a Zod schema includes a transform. It shows how to use `z.input`, `z.output`, and `z.infer` to access these distinct types.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/packages/v3.mdx#_snippet_145

LANGUAGE: TypeScript
CODE:
```
const stringToNumber = z.string().transform((val) => val.length);

// ⚠️ Important: z.infer returns the OUTPUT type!
type input = z.input<typeof stringToNumber>; // string
type output = z.output<typeof stringToNumber>; // number

// equivalent to z.output!
type inferred = z.infer<typeof stringToNumber>; // number
```

----------------------------------------

TITLE: Chaining Zod Omit and Extend Methods in TypeScript
DESCRIPTION: Illustrates a complex scenario where `.omit()` and `.extend()` methods are repeatedly chained on a Zod object schema. This pattern previously caused compiler issues and slow performance in Zod v3 but is significantly faster in Zod v4.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/v4/index.mdx#_snippet_9

LANGUAGE: ts
CODE:
```
import { z } from "zod/v4";

export const a = z.object({
  a: z.string(),
  b: z.string(),
  c: z.string(),
});

export const b = a.omit({
  a: true,
  b: true,
  c: true,
});

export const c = b.extend({
  a: z.string(),
  b: z.string(),
  c: z.string(),
});

export const d = c.omit({
  a: true,
  b: true,
  c: true,
});

export const e = d.extend({
  a: z.string(),
  b: z.string(),
  c: z.string(),
});

export const f = e.omit({
  a: true,
  b: true,
  c: true,
});

export const g = f.extend({
  a: z.string(),
  b: z.string(),
  c: z.string(),
});

export const h = g.omit({
  a: true,
  b: true,
  c: true,
});

export const i = h.extend({
  a: z.string(),
  b: z.string(),
  c: z.string(),
});

export const j = i.omit({
  a: true,
  b: true,
  c: true,
});

export const k = j.extend({
  a: z.string(),
  b: z.string(),
  c: z.string(),
});

export const l = k.omit({
  a: true,
  b: true,
  c: true,
});

export const m = l.extend({
  a: z.string(),
  b: z.string(),
  c: z.string(),
});

export const n = m.omit({
  a: true,
  b: true,
  c: true,
});

export const o = n.extend({
  a: z.string(),
  b: z.string(),
  c: z.string(),
});

export const p = o.omit({
  a: true,
  b: true,
  c: true,
});

export const q = p.extend({
  a: z.string(),
  b: z.string(),
  c: z.string(),
});
```

----------------------------------------

TITLE: Applying Number Validations (Zod) with Zod
DESCRIPTION: Lists various number-specific validation methods available directly on `z.number()` schemas in the standard Zod library.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/api.mdx#_snippet_33

LANGUAGE: TypeScript
CODE:
```
z.number().gt(5);
z.number().gte(5);                     // alias .min(5)
z.number().lt(5);
z.number().lte(5);                     // alias .max(5)
z.number().positive();       
z.number().nonnegative();    
z.number().negative(); 
z.number().nonpositive(); 
z.number().multipleOf(5);              // alias .step(5)
```

----------------------------------------

TITLE: Chaining Methods on Zod String Schema (TypeScript)
DESCRIPTION: This example illustrates Zod's chainable API by applying multiple methods directly to a string schema instance. It shows how to set minimum and maximum length constraints and apply a transformation like converting the string to lowercase.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/packages/zod.mdx#_snippet_1

LANGUAGE: TypeScript
CODE:
```
z.string()
  .min(5)
  .max(10)
  .toLowerCase();
```

----------------------------------------

TITLE: Defining Zod Coercion Schemas (TypeScript)
DESCRIPTION: Shows how to create schemas that attempt to coerce input data to the specified primitive type using `z.coerce`.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/api.mdx#_snippet_1

LANGUAGE: ts
CODE:
```
z.coerce.string();    // String(input)
z.coerce.number();    // Number(input)
z.coerce.boolean();   // Boolean(input)
z.coerce.bigint();    // BigInt(input)
```

----------------------------------------

TITLE: Zod Date Validation (TypeScript)
DESCRIPTION: Explains the `z.string().date()` method, which validates strings strictly in the `YYYY-MM-DD` format.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/packages/v3.mdx#_snippet_19

LANGUAGE: typescript
CODE:
```
const date = z.string().date();

date.parse("2020-01-01"); // pass
date.parse("2020-1-1"); // fail
date.parse("2020-01-32"); // fail
```

----------------------------------------

TITLE: Coerce Various Types to String Schema
DESCRIPTION: Provides examples of `z.coerce.string()` parsing different input types like numbers, booleans, undefined, and null, illustrating how JavaScript's `String()` function handles these coercions.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/packages/v3.mdx#_snippet_7

LANGUAGE: ts
CODE:
```
schema.parse(12); // => "12"
schema.parse(true); // => "true"
schema.parse(undefined); // => "undefined"
schema.parse(null); // => "null"
```

----------------------------------------

TITLE: Validating TypeScript String/Mixed Enum with Zod
DESCRIPTION: Demonstrates using `z.nativeEnum()` to validate values against a TypeScript enum that contains string members or a mix of string and numeric members. Shows which inputs pass validation.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/packages/v3.mdx#_snippet_43

LANGUAGE: ts
CODE:
```
enum Fruits {
  Apple = "apple",
  Banana = "banana",
  Cantaloupe, // you can mix numerical and string enums
}

const FruitEnum = z.nativeEnum(Fruits);
type FruitEnum = z.infer<typeof FruitEnum>; // Fruits

FruitEnum.parse(Fruits.Apple); // passes
FruitEnum.parse(Fruits.Cantaloupe); // passes
FruitEnum.parse("apple"); // passes
FruitEnum.parse("banana"); // passes
FruitEnum.parse(0); // passes
FruitEnum.parse("Cantaloupe"); // fails
```

----------------------------------------

TITLE: Replace Zod 3 `errorMap` with Zod 4 `error` Function (diff)
DESCRIPTION: Shows how the Zod 3 `errorMap` function for custom error mapping is replaced by the unified `error` parameter in Zod 4, which also accepts a function to handle different validation issue codes.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/v4/index.mdx#_snippet_41

LANGUAGE: diff
CODE:
```
// Zod 3 
- z.string({
-  errorMap: (issue, ctx) => {
-    if (issue.code === "too_small") {
-      return { message: `Value must be >${issue.minimum}` };
-    }
-    return { message: ctx.defaultError };
-  },
- });

// Zod 4
+ z.string({
+  error: (issue) => {
+    if (issue.code === "too_small") {
+      return `Value must be >${issue.minimum}`
+    }
+  },
+ });
```

----------------------------------------

TITLE: Chaining Optional and Array Methods in Zod (TypeScript)
DESCRIPTION: Highlights the difference in inferred types and validation behavior based on the order of chaining the .optional() and .array() methods.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/packages/v3.mdx#_snippet_72

LANGUAGE: TypeScript
CODE:
```
z.string().optional().array(); // (string | undefined)[]
z.string().array().optional(); // string[] | undefined
```

----------------------------------------

TITLE: Making a Zod Schema Nullish (Zod)
DESCRIPTION: Illustrates how to modify an existing Zod schema to accept both `null` and `undefined` inputs using the `z.nullish()` utility function.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/api.mdx#_snippet_60

LANGUAGE: TypeScript
CODE:
```
const nullishYoda = z.nullish(z.literal("yoda"));
```

----------------------------------------

TITLE: Validating Dates with Zod
DESCRIPTION: Demonstrates basic validation of Date instances using `z.date()`. Shows that a valid Date object passes, while a date string does not.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/packages/v3.mdx#_snippet_32

LANGUAGE: ts
CODE:
```
z.date().safeParse(new Date()); // success: true
z.date().safeParse("2022-01-12T00:00:00.000Z"); // success: false
```

----------------------------------------

TITLE: Using z.preprocess for Input Transformation in Zod TypeScript
DESCRIPTION: Explains how to use `z.preprocess` to transform input data *before* it is validated by a schema. This is useful for coercing input types. The example preprocesses a value, attempting to parse it as an integer before validating it against an integer schema.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/api.mdx#_snippet_106

LANGUAGE: TypeScript
CODE:
```
const coercedInt = z.preprocess((val) => {
  if (typeof val === "string") {
    return Number.parseInt(val);
  }
  return val;
}, z.int());
```

----------------------------------------

TITLE: Using .transform() Method on Schema in Zod TypeScript
DESCRIPTION: Illustrates the convenience `.transform()` method available directly on Zod schemas. This is a shorthand for piping a schema into a transform. The example transforms a string schema to return its length.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/api.mdx#_snippet_104

LANGUAGE: TypeScript
CODE:
```
const stringToLength = z.string().transform(val => val.length); 
```

----------------------------------------

TITLE: Validating Common String Formats in Zod
DESCRIPTION: Provides a list of schema methods available in Zod for validating strings against common formats such as email, UUID, URL, emoji, base64, nanoid, cuid, ulid, IP addresses, and ISO date/time formats.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/api.mdx#_snippet_12

LANGUAGE: ts
CODE:
```
z.email();
z.uuid();
z.url();
z.emoji();         // validates a single emoji character
z.base64();
z.base64url();
z.nanoid();
z.cuid();
z.cuid2();
z.ulid();
z.ipv4();
z.ipv6();
z.cidrv4();        // ipv4 CIDR block
z.cidrv6();        // ipv6 CIDR block
z.iso.date();
z.iso.time();
z.iso.datetime();
z.iso.duration();
```

----------------------------------------

TITLE: Define Schema with Custom Error String
DESCRIPTION: Demonstrates how to provide a simple custom error message string directly when defining a Zod schema.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/error-customization.mdx#_snippet_2

LANGUAGE: ts
CODE:
```
z.string("Not a string!");
```

----------------------------------------

TITLE: Validating TypeScript Numeric Enum with Zod
DESCRIPTION: Shows how to use `z.nativeEnum()` to validate values against a standard TypeScript numeric enum. Includes examples of passing and failing validations for enum members and their numeric values.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/packages/v3.mdx#_snippet_42

LANGUAGE: ts
CODE:
```
enum Fruits {
  Apple,
  Banana,
}

const FruitEnum = z.nativeEnum(Fruits);
type FruitEnum = z.infer<typeof FruitEnum>; // Fruits

FruitEnum.parse(Fruits.Apple); // passes
FruitEnum.parse(Fruits.Banana); // passes
FruitEnum.parse(0); // passes
FruitEnum.parse(1); // passes
FruitEnum.parse(3); // fails
```

----------------------------------------

TITLE: Email Validation with Built-in Regexes in Zod
DESCRIPTION: Demonstrates how to use Zod's exported built-in regexes (`z.regexes`) for email validation, including the default, HTML5, RFC 5322, and Unicode patterns.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/api.mdx#_snippet_16

LANGUAGE: ts
CODE:
```
// Zod's default email regex
z.email();
z.email({ pattern: z.regexes.email }); // equivalent

// the regex used by browsers to validate input[type=email] fields
// https://developer.mozilla.org/en-US/docs/Web/HTML/Element/input/email
z.email({ pattern: z.regexes.html5Email });

// the classic emailregex.com regex (RFC 5322)
z.email({ pattern: z.regexes.rfc5322Email });

// a loose regex that allows Unicode (good for intl emails)
z.email({ pattern: z.regexes.unicodeEmail });
```

----------------------------------------

TITLE: Defining Zod Null and Undefined Schemas (TypeScript)
DESCRIPTION: Demonstrates how to define schemas specifically for the JavaScript `null` and `undefined` literal values using `z.null()`, `z.undefined()`, and `z.void()`.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/api.mdx#_snippet_5

LANGUAGE: ts
CODE:
```
z.null();
z.undefined();
z.void(); // equivalent to z.undefined()
```

----------------------------------------

TITLE: Coercing Inputs to Zod Dates
DESCRIPTION: Demonstrates using `z.coerce.date()` (available since Zod 3.20) to attempt to convert various input types (strings, Date objects) into a Date instance before validation. Shows examples of valid and invalid inputs.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/packages/v3.mdx#_snippet_35

LANGUAGE: ts
CODE:
```
const dateSchema = z.coerce.date();
type DateSchema = z.infer<typeof dateSchema>;
// type DateSchema = Date

/* valid dates */
console.log(dateSchema.safeParse("2023-01-10T00:00:00.000Z").success); // true
console.log(dateSchema.safeParse("2023-01-10").success); // true
console.log(dateSchema.safeParse("1/10/23").success); // true
console.log(dateSchema.safeParse(new Date("1/10/23")).success); // true

/* invalid dates */
console.log(dateSchema.safeParse("2023-13-10").success); // false
console.log(dateSchema.safeParse("0000-00-00").success); // false
```

----------------------------------------

TITLE: Validating Time Strings with Zod
DESCRIPTION: Demonstrates the basic usage of `z.string().time()` to validate strings in HH:MM or HH:MM:SS[.s+] format. It shows examples of valid and invalid time strings, highlighting that timezone offsets are not allowed.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/packages/v3.mdx#_snippet_20

LANGUAGE: TypeScript
CODE:
```
const time = z.string().time();

time.parse("00:00:00"); // pass
time.parse("09:52:31"); // pass
time.parse("09:52"); // pass
time.parse("23:59:59.9999999"); // pass (arbitrary precision)

time.parse("00:00:00.123Z"); // fail (no `Z` allowed)
time.parse("00:00:00.123+02:00"); // fail (no offsets allowed)
```

----------------------------------------

TITLE: Making All Object Properties Optional with .partial - Zod TypeScript
DESCRIPTION: Shows how to create a new schema where all properties from the original object schema are made optional using the `.partial()` method, similar to TypeScript's `Partial` utility.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/packages/v3.mdx#_snippet_61

LANGUAGE: typescript
CODE:
```
const partialUser = user.partial();
// { email?: string | undefined; username?: string | undefined }
```

----------------------------------------

TITLE: Defining a Zod String Enum
DESCRIPTION: Shows the standard way to define a Zod enum with a fixed set of string values by passing an array directly to `z.enum()`. Includes type inference example.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/packages/v3.mdx#_snippet_36

LANGUAGE: ts
CODE:
```
const FishEnum = z.enum(["Salmon", "Tuna", "Trout"]);
type FishEnum = z.infer<typeof FishEnum>;
// 'Salmon' | 'Tuna' | 'Trout'
```

----------------------------------------

TITLE: Defining a Zod Strict Object Schema (TypeScript)
DESCRIPTION: Defines a Zod schema for an object with a string `username` and an array of numbers `favoriteNumbers`. `strictObject` ensures no extra keys are allowed.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/error-formatting.mdx#_snippet_0

LANGUAGE: ts
CODE:
```
import { z } from "zod/v4";

const schema = z.strictObject({
  username: z.string(),
  favoriteNumbers: z.array(z.number()),
});
```

----------------------------------------

TITLE: Making a Zod Schema Optional (Zod Mini)
DESCRIPTION: Shows how to make an existing Zod Mini schema accept `undefined` inputs using the `z.optional()` utility function.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/api.mdx#_snippet_53

LANGUAGE: TypeScript
CODE:
```
z.optional(z.literal("yoda"));
```

----------------------------------------

TITLE: Making Nested Object Properties Optional with .deepPartial - Zod TypeScript
DESCRIPTION: Illustrates how to apply optionality recursively to all properties within an object schema, including nested objects and arrays of objects, using the `.deepPartial()` method.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/packages/v3.mdx#_snippet_63

LANGUAGE: typescript
CODE:
```
const user = z.object({
  username: z.string(),
  location: z.object({
    latitude: z.number(),
    longitude: z.number(),
  }),
  strings: z.array(z.object({ value: z.string() })),
});

const deepPartialUser = user.deepPartial();

/*
{
  username?: string | undefined,
  location?: {
    latitude?: number | undefined;
    longitude?: number | undefined;
  } | undefined,
  strings?: { value?: string}[]
}
*/
```

----------------------------------------

TITLE: Validating Generic JSON - Zod - TypeScript
DESCRIPTION: Provides a schema definition that can validate any value conforming to the JSON specification (primitives, objects, arrays). It uses `z.union`, `z.array`, `z.record`, and `z.lazy` to handle the recursive nature of JSON structure.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/packages/v3.mdx#_snippet_93

LANGUAGE: TypeScript
CODE:
```
const literalSchema = z.union([z.string(), z.number(), z.boolean(), z.null()]);
type Literal = z.infer<typeof literalSchema>;
type Json = Literal | { [key: string]: Json } | Json[];
const jsonSchema: z.ZodType<Json> = z.lazy(() =>
  z.union([literalSchema, z.array(jsonSchema), z.record(jsonSchema)])
);

jsonSchema.parse(data);
```

----------------------------------------

TITLE: z.record() Dropping Single Argument Usage - Zod 3 vs Zod 4
DESCRIPTION: Shows that z.record() in Zod v4 requires both key and value schemas, unlike Zod v3 which allowed specifying only the value schema.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/v4/changelog.mdx#_snippet_36

LANGUAGE: TypeScript
CODE:
```
// Zod 3
z.record(z.string()); // ✅

// Zod 4
z.record(z.string()); // ❌
z.record(z.string(), z.string()); // ✅
```

----------------------------------------

TITLE: Parsing with Zod Coerced Boolean Schema (TypeScript)
DESCRIPTION: Illustrates how `z.coerce.boolean()` handles various truthy and falsy inputs, showing which values are coerced to `true` and `false`.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/api.mdx#_snippet_3

LANGUAGE: ts
CODE:
```
const schema = z.coerce.boolean(); // Boolean(input)

schema.parse("tuna"); // => true
schema.parse("true"); // => true
schema.parse("false"); // => true
schema.parse(1); // => true
schema.parse([]); // => true

schema.parse(0); // => false
schema.parse(""); // => false
schema.parse(undefined); // => false
schema.parse(null); // => false
```

----------------------------------------

TITLE: Custom Errors as Direct String Arguments (Zod & Zod Mini)
DESCRIPTION: Examples of applying custom error messages as direct string arguments to various Zod schema types and methods in both standard Zod and Zod Mini.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/error-customization.mdx#_snippet_4

LANGUAGE: ts
CODE:
```
z.string("Bad!");
z.string().min(5, "Too short!");
z.uuid("Bad UUID!");
z.iso.date("Bad date!");
z.array(z.string(), "Bad array!");
z.array(z.string()).min(5, "Too few items!");
z.set(z.string(), "Bad set!");
```

LANGUAGE: ts
CODE:
```
z.string("Bad!");
z.string().check(z.minLength(5, "Too short!"));
z.uuid("Bad UUID!");
z.iso.date("Bad date!");
z.array(z.string(), "Bad array!");
z.array(z.string()).check(z.minLength(5, "Too few items!"));
z.set(z.string(), "Bad set!");
```

----------------------------------------

TITLE: Pretty-Printing Zod Errors with z.prettifyError in TypeScript
DESCRIPTION: Demonstrates how to use the `z.prettifyError()` function to convert a `ZodError` instance into a human-readable, multi-line string format. Shows an example error object and its conversion.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/v4/index.mdx#_snippet_30

LANGUAGE: TypeScript
CODE:
```
const myError = new z.ZodError([
  {
    code: 'unrecognized_keys',
    keys: [ 'extraField' ],
    path: [],
    message: 'Unrecognized key: "extraField"'
  },
  {
    expected: 'string',
    code: 'invalid_type',
    path: [ 'username' ],
    message: 'Invalid input: expected string, received number'
  },
  {
    origin: 'number',
    code: 'too_small',
    minimum: 0,
    inclusive: true,
    path: [ 'favoriteNumbers', 1 ],
    message: 'Too small: expected number to be >=0'
  }
]);

z.prettifyError(myError);
```

----------------------------------------

TITLE: Applying Multiple Refinements (Continuable)
DESCRIPTION: Illustrates applying multiple `.refine()` or `z.refine()` checks to a schema. By default, Zod continues validation even after a failure, collecting all issues. Includes example output of the `issues` array.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/api.mdx#_snippet_95

LANGUAGE: ts
CODE:
```
const myString = z.string()
  .refine((val) => val.length > 8)
  .refine((val) => val === val.toLowerCase());
  

const result = myString.safeParse("OH NO");
result.error.issues;
/* [
  { "code": "custom", "message": "Too short!" },
  { "code": "custom", "message": "Must be lowercase" }
] */
```

LANGUAGE: ts
CODE:
```
const myString = z.string().check(
  z.refine((val) => val.length > 8),
  z.refine((val) => val === val.toLowerCase())
);

const result = z.safeParse(myString, "OH NO");
result.error.issues;
/* [
  { "code": "custom", "message": "Too short!" },
  { "code": "custom", "message": "Must be lowercase" }
] */
```

----------------------------------------

TITLE: Using a Zod Record Schema
DESCRIPTION: Provides an example of how to use a Zod record schema (`UserStore`) to validate assignments to an object. It shows a valid assignment and an invalid assignment that would result in a TypeScript error.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/packages/v3.mdx#_snippet_85

LANGUAGE: TypeScript
CODE:
```
const userStore: UserStore = {};

userStore["77d2586b-9e8e-4ecf-8b21-ea7e0530eadd"] = {
  name: "Carlotta",
}; // passes

userStore["77d2586b-9e8e-4ecf-8b21-ea7e0530eadd"] = {
  whatever: "Ice cream sundae",
}; // TypeError
```

----------------------------------------

TITLE: Applying Date Validations in Zod
DESCRIPTION: Shows how to apply minimum and maximum date constraints to a Zod date schema using `min` and `max` methods (or `minimum`/`maximum` in Zod Mini). Custom error messages can be included.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/api.mdx#_snippet_42

LANGUAGE: ts
CODE:
```
z.date().min(new Date("1900-01-01"), { error: "Too old!" });
z.date().max(new Date(), { error: "Too young!" });
```

LANGUAGE: ts zod/v4-mini
CODE:
```
z.date().check(z.minimum(new Date("1900-01-01"), { error: "Too old!" }));
z.date().check(z.maximum(new Date(), { error: "Too young!" });
```

----------------------------------------

TITLE: Creating Intersection Schema with .and in TypeScript
DESCRIPTION: A convenience method for creating intersection types. This combines the current schema with another schema, requiring data to match both types simultaneously.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/packages/v3.mdx#_snippet_137

LANGUAGE: TypeScript
CODE:
```
const nameAndAge = z
  .object({ name: z.string() })
  .and(z.object({ age: z.number() })); // { name: string } & { age: number }

// equivalent to
z.intersection(z.object({ name: z.string() }), z.object({ age: z.number() }));
```

----------------------------------------

TITLE: Zod Intersection of Object Types
DESCRIPTION: Demonstrates using `z.intersection()` to combine two Zod object schemas into a single schema that requires properties from both original objects. Notes that `extend` is often preferred for objects.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/api.mdx#_snippet_82

LANGUAGE: ts
CODE:
```
const Person = z.intersection({ name: z.string() });
type Person = z.infer<typeof Person>;

const Employee = z.intersection({ role: z.string() });
type Employee = z.infer<typeof Employee>;

const EmployedPerson = z.intersection(Person, Employee);
type EmployedPerson = z.infer<typeof EmployedPerson>;
// Person & Employee
```

----------------------------------------

TITLE: Customizing Error Per-Parse in Zod
DESCRIPTION: Provides a custom error map directly to the `.safeParse()` method. This allows for dynamic error messages based on the specific issue (`iss` object). This has higher precedence than the global error map.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/error-customization.mdx#_snippet_14

LANGUAGE: TypeScript
CODE:
```
const result = schema.safeParse(12, {
  error: (iss) => {
    if (iss.code === "invalid_type") {
      return `invalid type, expected ${iss.expected}`;
    }
    if (iss.code === "too_small") {
      return `minimum is ${iss.minimum}`;
    }
    // ...
  }
})
```

----------------------------------------

TITLE: Validating `as const` Object with Zod Native Enum
DESCRIPTION: Explains and shows how `z.nativeEnum()` can be used to validate values against an object defined with the `as const` assertion, inferring a union type from its property values.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/packages/v3.mdx#_snippet_44

LANGUAGE: ts
CODE:
```
const Fruits = {
  Apple: "apple",
  Banana: "banana",
  Cantaloupe: 3,
} as const;

const FruitEnum = z.nativeEnum(Fruits);
type FruitEnum = z.infer<typeof FruitEnum>; // "apple" | "banana" | 3

FruitEnum.parse("apple"); // passes
FruitEnum.parse("banana"); // passes
FruitEnum.parse(3); // passes
FruitEnum.parse("Cantaloupe"); // fails
```

----------------------------------------

TITLE: Configuring Global Error Map in Zod
DESCRIPTION: Sets a global custom error map using `z.config()`. This map has lower precedence than schema-level or per-parse errors. The `iss` object provides details about the validation issue.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/error-customization.mdx#_snippet_13

LANGUAGE: TypeScript
CODE:
```
z.config({
  customError: (iss) => {
    return "globally modified error";
  }
});
```

----------------------------------------

TITLE: Custom Error as a Function
DESCRIPTION: Shows how to provide a function to the `error` parameter to generate a dynamic error message at parse time.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/error-customization.mdx#_snippet_6

LANGUAGE: ts
CODE:
```
z.string({ error: ()=>`[${Date.now()}]: Validation failure.` });
```

----------------------------------------

TITLE: Providing Dynamic Catch Value with .catch Function in TypeScript
DESCRIPTION: Optionally, you can pass a function into .catch that will be re-executed whenever a default value needs to be generated. A `ctx` object containing the caught error will be passed into this function.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/packages/v3.mdx#_snippet_130

LANGUAGE: TypeScript
CODE:
```
const numberWithRandomCatch = z.number().catch((ctx) => {
  ctx.error; // the caught ZodError
  return Math.random();
});

numberWithRandomCatch.parse("sup"); // => 0.4413456736055323
numberWithRandomCatch.parse("sup"); // => 0.1871840107401901
numberWithRandomCatch.parse("sup"); // => 0.7223408162401552
```

----------------------------------------

TITLE: Accessing Enum Values - Zod 4 vs Zod 3 (Removed)
DESCRIPTION: Shows the canonical way to access enum values from a Zod enum schema in Zod v4 and highlights methods removed from Zod v3.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/v4/changelog.mdx#_snippet_27

LANGUAGE: TypeScript
CODE:
```
ColorSchema.enum.Red; // ✅ => "Red" (canonical API)
ColorSchema.Enum.Red; // ❌ removed
ColorSchema.Values.Red; // ❌ removed
```

----------------------------------------

TITLE: Using z.enum() with Native Enum - Zod 4
DESCRIPTION: Demonstrates the preferred way to create a Zod enum schema from a native TypeScript enum in Zod v4, replacing the deprecated z.nativeEnum().
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/v4/changelog.mdx#_snippet_26

LANGUAGE: TypeScript
CODE:
```
enum Color {
  Red = "red",
  Green = "green",
  Blue = "blue",
}

const ColorSchema = z.enum(Color); // ✅
```

----------------------------------------

TITLE: Default Zod Object Behavior - Stripping Unknown Keys (TypeScript)
DESCRIPTION: Illustrates the default behavior of Zod object schemas where unrecognized keys in the input data are removed during the parsing process.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/packages/v3.mdx#_snippet_67

LANGUAGE: TypeScript
CODE:
```
const person = z.object({
  name: z.string(),
});

person.parse({
  name: "bob dylan",
  extraKey: 61,
});
// => { name: "bob dylan" }
// extraKey has been stripped
```

----------------------------------------

TITLE: Extending Object Schema with .extend - Zod TypeScript
DESCRIPTION: Illustrates how to add new properties to an existing object schema using the `.extend()` method. This creates a new schema that includes all properties from the original plus the new ones.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/packages/v3.mdx#_snippet_55

LANGUAGE: typescript
CODE:
```
const DogWithBreed = Dog.extend({
  breed: z.string(),
});
```

----------------------------------------

TITLE: Using Zod's superRefine Method (TypeScript)
DESCRIPTION: Shows how to use the more versatile `.superRefine` method to add multiple custom validation issues with specific ZodIssueCodes and access the validation context (`ctx`).
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/packages/v3.mdx#_snippet_118

LANGUAGE: ts
CODE:
```
const Strings = z.array(z.string()).superRefine((val, ctx) => {
  if (val.length > 3) {
    ctx.addIssue({
      code: z.ZodIssueCode.too_big,
      maximum: 3,
      type: "array",
      inclusive: true,
      message: "Too many items 😡",
    });
  }

  if (val.length !== new Set(val).size) {
    ctx.addIssue({
      code: z.ZodIssueCode.custom,
      message: `No duplicates allowed.`,
    });
  }
});
```

----------------------------------------

TITLE: Disallowing Unknown Keys in Zod Object with Strict (TypeScript)
DESCRIPTION: Shows how to use the .strict() method on a Zod object schema to explicitly disallow unknown keys, causing a ZodError to be thrown if any are present in the input.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/packages/v3.mdx#_snippet_69

LANGUAGE: TypeScript
CODE:
```
const person = z
  .object({
    name: z.string(),
  })
  .strict();

person.parse({
  name: "bob dylan",
  extraKey: 61,
});
// => throws ZodError
```

----------------------------------------

TITLE: Create Custom Schema with Validation
DESCRIPTION: Explains how to use `z.custom()` to create a schema for a specific TypeScript type (like a template string literal) and provide a validation function.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/api.mdx#_snippet_130

LANGUAGE: ts
CODE:
```
const px = z.custom<`${number}px`>((val) => {
  return typeof val === "string" ? /^\d+px$/.test(val) : false;
});

type px = z.infer<typeof px>; // `${number}px`

px.parse("42px"); // "42px"
px.parse("42vw"); // throws;
```

----------------------------------------

TITLE: Inferring Zod Input/Output Types with z.input/z.output (TypeScript)
DESCRIPTION: For schemas where the input and output types diverge (e.g., due to `.transform()`), you can extract the types independently using `z.input<>` and `z.output<>`. `z.output<>` is equivalent to `z.infer<>`.
SOURCE: https://github.com/colinhacks/zod/blob/main/README.md#_snippet_9

LANGUAGE: TypeScript
CODE:
```
const mySchema = z.string().transform((val) => val.length);

type MySchemaIn = z.input<typeof mySchema>;
// => string

type MySchemaOut = z.output<typeof mySchema>; // equivalent to z.infer<typeof mySchema>
// number
```

----------------------------------------

TITLE: Customizing Type/Required Errors (Zod 4)
DESCRIPTION: Using the unified `error` parameter with a function to provide custom messages for both required and invalid type errors in Zod 4.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/v4/changelog.mdx#_snippet_4

LANGUAGE: typescript
CODE:
```
z.string({
  error: (issue) => issue.input === undefined 
    ? "This field is required" 
    : "Not a string" 
});
```

----------------------------------------

TITLE: Configuring strict mode in tsconfig.json (TypeScript)
DESCRIPTION: This snippet demonstrates how to enable the `strict` compiler option within your `tsconfig.json` file. Enabling `strict` mode is a mandatory requirement for using Zod and is considered a best practice for all TypeScript projects to ensure type safety.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/index.mdx#_snippet_3

LANGUAGE: JSON
CODE:
```
// tsconfig.json
{
  // ...
  "compilerOptions": {
    // ...
    "strict": true
  }
}
```

----------------------------------------

TITLE: Basic String Length Refinement
DESCRIPTION: Demonstrates applying a custom validation function to a string schema to check its length using `.refine()` in standard Zod and `.check()` with `z.refine()` in Zod Mini.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/api.mdx#_snippet_93

LANGUAGE: ts
CODE:
```
const myString = z.string().refine((val) => val.length <= 255);
```

LANGUAGE: ts
CODE:
```
const myString = z.string().check(z.refine((val) => val.length <= 255));
```

----------------------------------------

TITLE: Defining String Enum Schema with Zod
DESCRIPTION: Creates a Zod schema that validates inputs against a fixed array of string values using `z.enum`. Demonstrates successful and failed parsing examples.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/api.mdx#_snippet_43

LANGUAGE: ts
CODE:
```
const FishEnum = z.enum(["Salmon", "Tuna", "Trout"]);

FishEnum.parse("Salmon"); // => "Salmon"
FishEnum.parse("Swordfish"); // => ❌
```

----------------------------------------

TITLE: Applying Min/Max Constraints to Zod Dates
DESCRIPTION: Illustrates how to apply minimum and maximum date constraints to a Zod date schema using the `.min()` and `.max()` methods, including custom error messages.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/packages/v3.mdx#_snippet_34

LANGUAGE: ts
CODE:
```
z.date().min(new Date("1900-01-01"), { message: "Too old" });
z.date().max(new Date(), { message: "Too young!" });
```

----------------------------------------

TITLE: Coercion Methods for Zod Primitives
DESCRIPTION: Lists the available `z.coerce` methods for coercing input values to various primitive types (string, number, boolean, bigint, date) using their respective JavaScript constructors.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/packages/v3.mdx#_snippet_9

LANGUAGE: ts
CODE:
```
z.coerce.string(); // String(input)
z.coerce.number(); // Number(input)
z.coerce.boolean(); // Boolean(input)
z.coerce.bigint(); // BigInt(input)
z.coerce.date(); // new Date(input)
```

----------------------------------------

TITLE: Defining Zod Tuple Schema
DESCRIPTION: Illustrates how to define a Zod schema for a fixed-length tuple with different schema types for each position, and how to infer its TypeScript type.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/api.mdx#_snippet_73

LANGUAGE: TypeScript
CODE:
```
const MyTuple = z.tuple([
  z.string(),
  z.number(),
  z.boolean()
]);

type MyTuple = z.infer<typeof MyTuple>;
// [string, number, boolean]
```

----------------------------------------

TITLE: Picking Object Properties with .pick - Zod TypeScript
DESCRIPTION: Shows how to create a new schema containing only a specified subset of properties from an existing object schema using the `.pick()` method, similar to TypeScript's `Pick` utility.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/packages/v3.mdx#_snippet_58

LANGUAGE: typescript
CODE:
```
const JustTheName = Recipe.pick({ name: true });
type JustTheName = z.infer<typeof JustTheName>;
// => { name: string }
```

----------------------------------------

TITLE: Initializing Zod Object with Partial Properties (TypeScript)
DESCRIPTION: Creates a Zod object schema where all properties are initially marked as optional using the .partial() method.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/packages/v3.mdx#_snippet_64

LANGUAGE: TypeScript
CODE:
```
const user = z
  .object({
    email: z.string(),
    username: z.string(),
  })
  .partial();
// { email?: string | undefined; username?: string | undefined }
```

----------------------------------------

TITLE: Using Zod .catch() with Static Value (Zod)
DESCRIPTION: Demonstrates how Zod's `.catch()` method provides a static fallback value that is returned when the schema fails validation. This example uses the standard Zod syntax.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/api.mdx#_snippet_113

LANGUAGE: TypeScript
CODE:
```
const numberWithCatch = z.number().catch(42);

numberWithCatch.parse(5); // => 5
numberWithCatch.parse("tuna"); // => 42
```

----------------------------------------

TITLE: Defining Boolean Schema with Zod
DESCRIPTION: Shows how to create a Zod schema for boolean values and parse them. This is used to ensure an input is strictly a boolean `true` or `false`.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/api.mdx#_snippet_39

LANGUAGE: ts
CODE:
```
z.boolean().parse(true); // => true
z.boolean().parse(false); // => false
```

----------------------------------------

TITLE: Customize Type/Required Errors with Zod 4 `error` Function (diff)
DESCRIPTION: Illustrates the Zod 4 update where `invalid_type_error` and `required_error` parameters are replaced by a single `error` parameter that accepts a function to dynamically generate error messages based on the issue type.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/v4/index.mdx#_snippet_40

LANGUAGE: diff
CODE:
```
// Zod 3
- z.string({ 
-   required_error: "This field is required" 
-   invalid_type_error: "Not a string", 
- });

// Zod 4 
+ z.string({ error: (issue) => issue.input === undefined ? 
+  "This field is required" :
+  "Not a string" 
+ });
```

----------------------------------------

TITLE: Customize Min/Max Error with Zod 4 `error` (diff)
DESCRIPTION: Demonstrates the Zod 4 change from using the `message` parameter to the new unified `error` parameter for customizing validation error messages on string constraints like `min`.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/v4/index.mdx#_snippet_39

LANGUAGE: diff
CODE:
```
- z.string().min(5, { message: "Too short." });
+ z.string().min(5, { error: "Too short." });
```

----------------------------------------

TITLE: Correct Generic Function for Zod Schemas using z.ZodTypeAny (TypeScript)
DESCRIPTION: Presents the correct method for writing a generic function that accepts any Zod schema using `T extends z.ZodTypeAny`. This preserves the specific schema subclass type, allowing for proper type inference and method access.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/packages/v3.mdx#_snippet_147

LANGUAGE: TypeScript
CODE:
```
function inferSchema<T extends z.ZodTypeAny>(schema: T) {
  return schema;
}

inferSchema(z.string());
// => ZodString
```

----------------------------------------

TITLE: Applying Methods to Recursive Zod Schemas in TypeScript
DESCRIPTION: Shows that recursive schemas defined using the Zod v4 getter pattern are regular `ZodObject` instances and support standard Zod methods like `.pick()`, `.partial()`, and `.extend()`.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/v4/index.mdx#_snippet_27

LANGUAGE: TypeScript
CODE:
```
Post.pick({ title: true })
Post.partial();
Post.extend({ publishDate: z.date() });
```

----------------------------------------

TITLE: Interleaving Transforms and Refinements (TypeScript)
DESCRIPTION: Demonstrates chaining a `.transform` method before a `.refine` method on a Zod schema.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/packages/v3.mdx#_snippet_117

LANGUAGE: ts
CODE:
```
z.string()
  .transform((val) => val.length)
  .refine((val) => val > 25);
```

----------------------------------------

TITLE: Defining Zod Literal Array Schema (TypeScript)
DESCRIPTION: Shows how to create a schema that accepts one of several specified literal values by passing an array to `z.literal()`.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/api.mdx#_snippet_6

LANGUAGE: ts
CODE:
```
const colors = z.literal(["red", "green", "blue"]);

colors.parse("green"); // ✅
colors.parse("yellow"); // ❌
```

----------------------------------------

TITLE: Chain Methods on Coerced String Schema
DESCRIPTION: Shows that a schema created with `z.coerce.string()` is a standard `ZodString` instance, allowing chaining of typical string validation methods like `email()` and `min()`.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/packages/v3.mdx#_snippet_8

LANGUAGE: ts
CODE:
```
z.coerce.string().email().min(5);
```

----------------------------------------

TITLE: Customizing Error Path in Refinements
DESCRIPTION: Shows how to specify the `path` property in the options object to control where the validation issue appears in the error object, useful for object-level validations like password confirmation.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/api.mdx#_snippet_97

LANGUAGE: ts
CODE:
```
const passwordForm = z
  .object({
    password: z.string(),
    confirm: z.string(),
  })
  .refine((data) => data.password === data.confirm, {
    message: "Passwords don't match",
    path: ["confirm"], // path of error
  });
```

LANGUAGE: ts
CODE:
```
const passwordForm = z
  .object({
    password: z.string(),
    confirm: z.string(),
  })
  .check(z.refine((data) => data.password === data.confirm, {
    message: "Passwords don't match",
    path: ["confirm"], // path of error
  }));
```

----------------------------------------

TITLE: Validating Integers with Zod
DESCRIPTION: Explains how to use `z.int()` to validate safe integers and `z.int32()` to validate numbers within the 32-bit integer range.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/api.mdx#_snippet_36

LANGUAGE: TypeScript
CODE:
```
z.int();     // restricts to safe integer range
z.int32();   // restrict to int32 range
```

----------------------------------------

TITLE: Implementing Async Zod Transforms (TypeScript)
DESCRIPTION: Provides an example of using an `async` function within `.transform()` to perform asynchronous operations, such as fetching data based on an ID. Note that schemas with async transforms require using `parseAsync()` or `safeParseAsync()`.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/packages/v3.mdx#_snippet_125

LANGUAGE: TypeScript
CODE:
```
const IdToUser = z
  .string()
  .uuid()
  .transform(async (id) => {
    return await getUserById(id);
  });
```

----------------------------------------

TITLE: Validating String Starts With with Custom Error in Zod
DESCRIPTION: Shows how to provide a custom error message when a string fails the `startsWith` validation in both standard Zod and Zod Mini.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/api.mdx#_snippet_9

LANGUAGE: ts
CODE:
```
z.string().startsWith("fourscore", {error: "Nice try, buddy"})
```

LANGUAGE: ts zod/v4-mini
CODE:
```
z.string().check(z.startsWith("fourscore", {error: "Nice try, buddy"}))
```

----------------------------------------

TITLE: Defining Recursive Object Types in Zod v4 (Getters) in TypeScript
DESCRIPTION: Explains the Zod v4 pattern for defining recursive object types using getter properties. The getter returns the schema itself, allowing for types like a category with subcategories of the same type.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/v4/index.mdx#_snippet_25

LANGUAGE: TypeScript
CODE:
```
const Category = z.object({
  name: z.string(),
  get subcategories(){
    return z.array(Category)
  }
});

type Category = z.infer<typeof Category>;
// { name: string; subcategories: Category[] }
```

----------------------------------------

TITLE: Defining Recursive Zod Object (Category) - TypeScript
DESCRIPTION: Demonstrates how to define a self-referential Zod object schema for a category using a getter to reference itself within an array of subcategories.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/api.mdx#_snippet_66

LANGUAGE: TypeScript
CODE:
```
const Category = z.object({
  name: z.string(),
  get subcategories(){
    return z.array(Category)
  }
});

type Category = z.infer<typeof Category>;
// { name: string; subcategories: Category[] }
```

----------------------------------------

TITLE: Customizing Email Validation Regex with z.email() in Zod
DESCRIPTION: Shows how to use the `pattern` option with `z.email()` to specify a custom regular expression for email validation. It includes examples using predefined regexes available in `z.regexes`.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/v4/index.mdx#_snippet_33

LANGUAGE: TypeScript
CODE:
```
// Zod's default email regex (Gmail rules)
// see colinhacks.com/essays/reasonable-email-regex
z.email(); // z.regexes.email

// the regex used by browsers to validate input[type=email] fields
// https://developer.mozilla.org/en-US/docs/Web/HTML/Element/input/email
z.email({ pattern: z.regexes.html5Email });

// the classic emailregex.com regex (RFC 5322)
z.email({ pattern: z.regexes.rfc5322Email });

// a loose regex that allows Unicode (good for intl emails)
z.email({ pattern: z.regexes.unicodeEmail });
```

----------------------------------------

TITLE: Creating Record Schema with z.record in Zod
DESCRIPTION: Demonstrates how to create a Zod schema for a JavaScript `Record` type using `z.record`. This is useful for validating objects where keys are of one type (e.g., string) and values are of another (e.g., a Zod object schema). It also shows how to infer the TypeScript type.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/packages/v3.mdx#_snippet_84

LANGUAGE: TypeScript
CODE:
```
const User = z.object({ name: z.string() });

const UserStore = z.record(z.string(), User);
type UserStore = z.infer<typeof UserStore>;
// => Record<string, { name: string }>
```

----------------------------------------

TITLE: Exploring Issue Context Properties (Zod & Zod Mini)
DESCRIPTION: Shows common properties available on the issue context object (`iss`) within the error function, such as `code`, `input`, `inst`, and `path`.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/error-customization.mdx#_snippet_8

LANGUAGE: ts
CODE:
```
z.string({
  error: (iss) => {
    iss.code; // the issue code
    iss.input; // the input data
    iss.inst; // the schema/check that originated this issue
    iss.path; // the path of the error
  }
});
```

LANGUAGE: ts
CODE:
```
z.string({
  error: (iss) => {
    iss.inst;
    iss.code; // the issue code
    iss.input; // the input data
    iss.inst; // the schema/check that originated this issue
    iss.path; // the path of the error
  }
});
```

----------------------------------------

TITLE: Using Asynchronous Refinements with refine (TypeScript)
DESCRIPTION: Provides an example of defining an asynchronous validation function within `.refine`, noting that `.parseAsync` must be used when async refinements are present.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/packages/v3.mdx#_snippet_116

LANGUAGE: ts
CODE:
```
const userId = z.string().refine(async (id) => {
  // verify that ID exists in database
  return true;
});
```

----------------------------------------

TITLE: Chaining Zod Methods Before Transform (TypeScript)
DESCRIPTION: Illustrates the correct order for chaining Zod methods. Methods that operate on the base schema type (like `.email()` on a string schema) must be applied before `.transform()`, as `.transform()` changes the schema instance to a `ZodEffects` type.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/packages/v3.mdx#_snippet_122

LANGUAGE: TypeScript
CODE:
```
const emailToDomain = z
  .string()
  .email()
  .transform((val) => val.split("@")[1]);

emailToDomain.parse("colinhacks@example.com"); // => example.com
```

----------------------------------------

TITLE: Basic Zod Datetime Validation (TypeScript)
DESCRIPTION: Explains the default behavior of `z.string().datetime()`, which validates ISO 8601 strings, allowing arbitrary sub-second precision but disallowing timezone offsets by default.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/packages/v3.mdx#_snippet_15

LANGUAGE: typescript
CODE:
```
const datetime = z.string().datetime();

datetime.parse("2020-01-01T00:00:00Z"); // pass
datetime.parse("2020-01-01T00:00:00.123Z"); // pass
datetime.parse("2020-01-01T00:00:00.123456Z"); // pass (arbitrary precision)
datetime.parse("2020-01-01T00:00Z"); // pass (hours and minutes only)
datetime.parse("2020-01-01T00:00:00+02:00"); // fail (no offsets allowed)
```

----------------------------------------

TITLE: Zod Optional and Nullable Converted to JSON Schema OneOf Null (TypeScript)
DESCRIPTION: Shows how Zod's `optional()` and `nullable()` wrappers are converted in JSON Schema using the `oneOf` keyword to include the `{ type: "null" }` possibility alongside the base type.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/json-schema.mdx#_snippet_7

LANGUAGE: ts
CODE:
```
z.optional(z.string());
// => { oneOf: [{ type: "string" }, { type: "null" }] }

z.nullable(z.string());
// => { oneOf: [{ type: "string" }, { type: "null" }] }
```

----------------------------------------

TITLE: Refining Zod String (New Syntax) - TypeScript
DESCRIPTION: Illustrates the new syntax for the `.refine()` method in Zod. The error message is now provided via an `error` property within the options object passed as the second argument, accepting an `issue` object to generate the message.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/v4/changelog.mdx#_snippet_48

LANGUAGE: typescript
CODE:
```
const longString = z.string().refine((val) => val.length > 10, {
  error: (issue) => `${issue.input} is not more than 10 characters`,
});
```

----------------------------------------

TITLE: Implementing Async Transforms with Zod/Zod Mini TypeScript
DESCRIPTION: Shows how to define and use asynchronous transforms. Async transforms are useful for operations like fetching data based on input. Note that when using async transforms, you must use the `parseAsync` or `safeParseAsync` methods.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/api.mdx#_snippet_105

LANGUAGE: TypeScript
CODE:
```
const idToUser = z
  .string()
  .transform(async (id) => {
    // fetch user from database
    return db.getUserById(id); 
  });

const user = await idToUser.parseAsync("abc123");
```

LANGUAGE: TypeScript
CODE:
```
const idToUser = z.pipe(
  z.string(),
  z.transform(async (id) => {
    // fetch user from database
    return db.getUserById(id); 
  }));

const user = await idToUser.parse("abc123");
```

----------------------------------------

TITLE: Define Readonly Object Schema (Zod)
DESCRIPTION: Demonstrates how to define a Zod schema for an object and mark it as readonly using the `.readonly()` method, showing the inferred TypeScript type.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/api.mdx#_snippet_120

LANGUAGE: ts
CODE:
```
const ReadonlyUser = z.object({ name: z.string() }).readonly();
type ReadonlyUser = z.infer<typeof ReadonlyUser>;
// Readonly<{ name: string }>
```

----------------------------------------

TITLE: Coerce Input to Zod String Schema
DESCRIPTION: Demonstrates using `z.coerce.string()` to create a schema that attempts to convert the input value to a string before validation, showing examples with string and number inputs.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/packages/v3.mdx#_snippet_6

LANGUAGE: ts
CODE:
```
const schema = z.coerce.string();
schema.parse("tuna"); // => "tuna"
schema.parse(12); // => "12"
```

----------------------------------------

TITLE: Coerce Input to Zod Boolean Schema
DESCRIPTION: Illustrates how `z.coerce.boolean()` works, explaining that it uses JavaScript's `Boolean()` function, which coerces any truthy value to `true` and any falsy value to `false`, providing examples for various inputs.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/packages/v3.mdx#_snippet_10

LANGUAGE: ts
CODE:
```
const schema = z.coerce.boolean(); // Boolean(input)

schema.parse("tuna"); // => true
schema.parse("true"); // => true
schema.parse("false"); // => true
schema.parse(1); // => true
schema.parse([]); // => true

schema.parse(0); // => false
schema.parse(""); // => false
schema.parse(undefined); // => false
schema.parse(null); // => false
```

----------------------------------------

TITLE: Defining and Parsing a String Record Schema with Zod
DESCRIPTION: This snippet demonstrates how to create a Zod schema for a JavaScript object where both keys and values are strings using `z.record()`. It also shows how to use the `.parse()` method to validate an input object against this schema.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/api.mdx#_snippet_83

LANGUAGE: TypeScript
CODE:
```
const IdCache = z.record(z.string(), z.string());
type IdCache = z.infer<typeof IdCache>; // Record<string, string>

IdCache.parse({
  carlotta: "77d2586b-9e8e-4ecf-8b21-ea7e0530eadd",
  jimmie: "77d2586b-9e8e-4ecf-8b21-ea7e0530eadd",
});
```

----------------------------------------

TITLE: Customizing Zod String Schema Errors (TypeScript)
DESCRIPTION: Shows how to provide custom error messages when defining a Zod string schema using the configuration object for `required_error` and `invalid_type_error`.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/packages/v3.mdx#_snippet_13

LANGUAGE: typescript
CODE:
```
const name = z.string({
  required_error: "Name is required",
  invalid_type_error: "Name must be a string"
});
```

----------------------------------------

TITLE: Custom Errors via Params Object (Zod & Zod Mini)
DESCRIPTION: Examples of applying custom error messages using a params object with an `error` property for various Zod schema types and methods in both standard Zod and Zod Mini.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/error-customization.mdx#_snippet_5

LANGUAGE: ts
CODE:
```
z.string({ error: "Bad!" });
z.string().min(5, { error: "Too short!" });
z.uuid({ error: "Bad UUID!" });
z.iso.date({ error: "Bad date!" });
z.array(z.string(), { error: "Bad array!" });
z.array(z.string()).min(5, { error: "Too few items!" });
z.set(z.string(), { error: "Bad set!" });
```

LANGUAGE: ts
CODE:
```
z.string({ error: "Bad!" });
z.string().check(z.minLength(5, { error: "Too short!" }));
z.uuid({ error: "Bad UUID!" });
z.iso.date({ error: "Bad date!" });
z.array(z.string(), { error: "Bad array!" });
z.array(z.string()).check(z.minLength(5, { error: "Too few items!" }));
z.set(z.string(), { error: "Bad set!" });
```

----------------------------------------

TITLE: Custom Error Function with Issue Context (Zod & Zod Mini)
DESCRIPTION: Examples demonstrating how the error function receives an issue context object (`iss`) which can be used to customize the message based on input or other validation details.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/error-customization.mdx#_snippet_7

LANGUAGE: ts
CODE:
```
z.string({
  error: (iss) => iss.input===undefined ? "Field is required." : "Invalid input."
});
```

LANGUAGE: ts
CODE:
```
z.string({
  error: (iss) => iss.input===undefined ? "Field is required." : "Invalid input."
});
```

----------------------------------------

TITLE: Setting Dynamic Default Value with Zod/Zod Mini TypeScript
DESCRIPTION: Shows how to provide a function to `.default()` or `z._default()` to generate a default value dynamically. The function is executed each time a default value is needed for `undefined` input.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/api.mdx#_snippet_108

LANGUAGE: TypeScript
CODE:
```
const randomDefault = z.number().default(Math.random);

randomDefault.parse(undefined);    // => 0.4413456736055323
randomDefault.parse(undefined);    // => 0.1871840107401901
randomDefault.parse(undefined);    // => 0.7223408162401552
```

LANGUAGE: TypeScript
CODE:
```
const randomDefault = z._default(z.number(), Math.random);

z.parse(randomDefault, undefined); // => 0.4413456736055323
z.parse(randomDefault, undefined); // => 0.1871840107401901
z.parse(randomDefault, undefined); // => 0.7223408162401552
```

----------------------------------------

TITLE: Converting Zod Error to Nested Tree Structure with `z.treeifyError` (TypeScript)
DESCRIPTION: Uses `z.treeifyError` to transform a flat array of error issues into a nested object structure that mirrors the schema. This makes it easier to access errors specific to a particular path within the validated data. The resulting object includes `errors`, `properties`, and `items` fields for navigation.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/error-formatting.mdx#_snippet_2

LANGUAGE: ts
CODE:
```
const tree = z.treeifyError(result.error);
```

----------------------------------------

TITLE: Using Zod's .readonly() on Collections (TypeScript)
DESCRIPTION: Illustrates the application of the `.readonly()` method to various Zod collection schemas, including arrays, tuples, maps, and sets, showing how it results in their respective readonly TypeScript types.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/packages/v3.mdx#_snippet_142

LANGUAGE: TypeScript
CODE:
```
z.array(z.string()).readonly();
// readonly string[]

z.tuple([z.string(), z.number()]).readonly();
// readonly [string, number]

z.map(z.string(), z.date()).readonly();
// ReadonlyMap<string, Date>

z.set(z.string()).readonly();
// ReadonlySet<string>
```

----------------------------------------

TITLE: Define Zod Literal Schemas
DESCRIPTION: Demonstrates how to create schemas that only accept specific literal values using `z.literal()`, including examples for strings, numbers, bigints, booleans, and symbols, and shows how to access the literal value.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/packages/v3.mdx#_snippet_11

LANGUAGE: ts
CODE:
```
const tuna = z.literal("tuna");
const twelve = z.literal(12);
const twobig = z.literal(2n); // bigint literal
const tru = z.literal(true);

const terrificSymbol = Symbol("terrific");
const terrific = z.literal(terrificSymbol);

// retrieve literal value
tuna.value; // "tuna"
```

----------------------------------------

TITLE: Common String Transforms in Zod
DESCRIPTION: Shows how to apply common string transformations like trimming whitespace and changing case using Zod's built-in methods for both standard Zod and Zod Mini.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/api.mdx#_snippet_11

LANGUAGE: ts
CODE:
```
z.string().trim(); // trim whitespace
z.string().toLowerCase(); // toLowerCase
z.string().toUpperCase(); // toUpperCase
```

LANGUAGE: ts zod/v4-mini
CODE:
```
z.string().check(z.trim()); // trim whitespace
z.string().check(z.toLowerCase()); // toLowerCase
z.string().check(z.toUpperCase()); // toUpperCase
```

----------------------------------------

TITLE: Implementing Async Functions with z.function().implementAsync() - Zod 4
DESCRIPTION: Introduces the new implementAsync() method in Zod v4 for defining Zod-validated asynchronous functions.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/v4/changelog.mdx#_snippet_31

LANGUAGE: TypeScript
CODE:
```
myFunction.implementAsync(async (input) => {
  return `Hello ${input.name}, you are ${input.age} years old.`;
});
```

----------------------------------------

TITLE: Customizing Min Length Error (Zod 4)
DESCRIPTION: Using the unified `error` parameter to customize the error message for a minimum length validation in Zod 4.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/v4/changelog.mdx#_snippet_2

LANGUAGE: typescript
CODE:
```
z.string().min(5, { error: "Too short." });
```

----------------------------------------

TITLE: Implementing Zod Function Schema (TypeScript)
DESCRIPTION: Uses the `.implement()` method on a Zod function schema to wrap a standard function. The returned function automatically validates its arguments against the schema's input definition before execution and its return value against the output definition.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/api.mdx#_snippet_134

LANGUAGE: TypeScript
CODE:
```
const computeTrimmedLength = MyFunction.implement((input) => {
  // TypeScript knows x is a string!
  return input.trim().length;
});
```

----------------------------------------

TITLE: Using New Number Format Functions in Zod
DESCRIPTION: Demonstrates the new top-level functions for defining number schemas with predefined constraints corresponding to common fixed-width integer and float types. These functions return `ZodNumber` instances with appropriate min/max bounds.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/v4/index.mdx#_snippet_35

LANGUAGE: TypeScript
CODE:
```
z.int();      // [Number.MIN_SAFE_INTEGER, Number.MAX_SAFE_INTEGER],
z.float32();  // [-3.4028234663852886e38, 3.4028234663852886e38]
z.float64();  // [-1.7976931348623157e308, 1.7976931348623157e308]
z.int32();    // [-2147483648, 2147483647]
z.uint32();   // [0, 4294967295]
```

----------------------------------------

TITLE: Defining and Registering Zod Schemas with Circular References - TypeScript
DESCRIPTION: This snippet defines two Zod object schemas, `User` and `Post`, which have circular references to each other. It then registers both schemas in the global Zod registry (`z.globalRegistry`) using unique IDs.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/json-schema.mdx#_snippet_26

LANGUAGE: typescript
CODE:
```
import { z } from "zod/v4";

const User = z.object({
  name: z.string(),
  get posts(){
    return z.array(Post);
  }
});

const Post = z.object({
  title: z.string(),
  content: z.string(),
  get author(){
    return User;
  }
});

z.globalRegistry.add(User, {id: "User"});
z.globalRegistry.add(Post, {id: "Post"});
```

----------------------------------------

TITLE: Interleaving Zod Transforms and Refinements (TypeScript)
DESCRIPTION: Shows that `.transform()` and `.refine()` methods can be chained together and are executed sequentially in the order they are declared, allowing for complex validation and transformation pipelines.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/packages/v3.mdx#_snippet_124

LANGUAGE: TypeScript
CODE:
```
const nameToGreeting = z
  .string()
  .transform((val) => val.toUpperCase())
  .refine((val) => val.length > 15)
  .transform((val) => `Hello ${val}`)
  .refine((val) => val.indexOf("!") === -1);
```

----------------------------------------

TITLE: Parsing Data with Correctly Typed Zod Schemas (TypeScript)
DESCRIPTION: Demonstrates how to define a function that parses data using a correctly typed schema, allowing TypeScript to infer the precise output type based on the input schema.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/library-authors.mdx#_snippet_13

LANGUAGE: typescript
CODE:
```
function parseData<T extends z.ZodTypeAny>(data: unknown, schema: T): z.output<T> {
  return schema.parse(data);
}

parseData("sup", z.string());
// => string
```

----------------------------------------

TITLE: Defining Recursive Schema with Effects - Zod - TypeScript
DESCRIPTION: Explains how to handle recursive schemas when ZodEffects (like `.refine` or `.transform`) are involved. This requires explicitly defining both the input and output types using `z.ZodType<Output, z.ZodTypeDef, Input>` and applying the effects to the base schema.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/packages/v3.mdx#_snippet_92

LANGUAGE: TypeScript
CODE:
```
const isValidId = (id: string): id is `${string}/${string}` =>
  id.split("/").length === 2;

const baseSchema = z.object({
  id: z.string().refine(isValidId)
});

type Input = z.input<typeof baseSchema> & {
  children: Input[];
};

type Output = z.output<typeof baseSchema> & {
  children: Output[];
};

const schema: z.ZodType<Output, z.ZodTypeDef, Input> = baseSchema.extend({
  children: z.lazy(() => schema.array())
});
```

----------------------------------------

TITLE: Converting Zod Schema to JSON Schema (Basic) in TypeScript
DESCRIPTION: Illustrates how to use the `z.toJSONSchema()` function from Zod v4 to convert a basic Zod object schema into its corresponding JSON Schema representation. Requires importing `z` from 'zod/v4'.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/v4/index.mdx#_snippet_23

LANGUAGE: TypeScript
CODE:
```
import { z } from "zod/v4";

const mySchema = z.object({name: z.string(), points: z.number()});

z.toJSONSchema(mySchema);
// => {
//   type: "object",
//   properties: {
//     name: {type: "string"},
//     points: {type: "number"},
//   },
//   required: ["name", "points"],
// }
```

----------------------------------------

TITLE: Providing Catch Value with .catch in TypeScript
DESCRIPTION: Use .catch() to provide a "catch value" to be returned in the event of a parsing error. The provided value will be returned directly when parsing fails.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/packages/v3.mdx#_snippet_129

LANGUAGE: TypeScript
CODE:
```
const numberWithCatch = z.number().catch(42);

numberWithCatch.parse(5); // => 5
numberWithCatch.parse("tuna"); // => 42
```

----------------------------------------

TITLE: Converting Zod Object Schema to JSON Schema (TypeScript)
DESCRIPTION: Demonstrates how to use `z.toJSONSchema()` to convert a simple Zod object schema into its equivalent JSON Schema representation. Shows the resulting JSON structure.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/json-schema.mdx#_snippet_0

LANGUAGE: ts
CODE:
```
import { z } from "zod/v4";

const schema = z.object({
  name: z.string(),
  age: z.number(),
});

z.toJSONSchema(schema)
// => {
//   type: 'object',
//   properties: { name: { type: 'string' }, age: { type: 'number' } },
//   required: [ 'name', 'age' ]
// }
```

----------------------------------------

TITLE: Extracting Inner Schema from ZodOptional (Zod)
DESCRIPTION: Demonstrates how to retrieve the original schema that was wrapped by a `ZodOptional` instance using the `.unwrap()` method.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/api.mdx#_snippet_54

LANGUAGE: TypeScript
CODE:
```
optionalYoda.unwrap(); // ZodLiteral<"yoda">
```

----------------------------------------

TITLE: Defining Mutually Recursive Zod Objects (User, Post) - TypeScript
DESCRIPTION: Shows how to define two Zod object schemas that reference each other, creating mutually recursive types using getters.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/api.mdx#_snippet_67

LANGUAGE: TypeScript
CODE:
```
const User = z.object({
  email: z.email(),
  get posts(){
    return z.array(Post)
  }
});

const Post = z.object({
  title: z.string(),
  get author(){
    return User
  }
});
```

----------------------------------------

TITLE: UUID Validation with Version and Convenience Methods in Zod
DESCRIPTION: Explains how to validate a UUID against a specific version (v1-v8) using the `version` parameter or convenience methods like `uuidv4()`, `uuidv6()`, and `uuidv7()`.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/api.mdx#_snippet_18

LANGUAGE: ts
CODE:
```
// supports "v1", "v2", "v3", "v4", "v5", "v6", "v7", "v8"
z.uuid({ version: "v4" });

// for convenience
z.uuidv4();
z.uuidv6();
z.uuidv7();
```

----------------------------------------

TITLE: Validating ISO 8601 Datetimes (No Offset) with Zod
DESCRIPTION: Demonstrates the default behavior of `z.iso.datetime()` which enforces ISO 8601 format and does not allow timezone offsets.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/api.mdx#_snippet_23

LANGUAGE: TypeScript
CODE:
```
const datetime = z.iso.datetime();

datetime.parse("2020-01-01T00:00:00Z"); // ✅
datetime.parse("2020-01-01T00:00:00.123Z"); // ✅
datetime.parse("2020-01-01T00:00:00.123456Z"); // ✅ (arbitrary precision)
datetime.parse("2020-01-01T00:00:00+02:00"); // ❌ (no offsets allowed)
```

----------------------------------------

TITLE: Defining Zod Literal Schemas (TypeScript)
DESCRIPTION: Shows how to create schemas that match specific literal values using `z.literal()` for strings, numbers, booleans, bigints, and symbols.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/api.mdx#_snippet_4

LANGUAGE: ts
CODE:
```
const tuna = z.literal("tuna");
const twelve = z.literal(12);
const twobig = z.literal(2n);
const tru = z.literal(true);
const terrific = z.literal(Symbol("terrific"));
```

----------------------------------------

TITLE: Ensuring Non-Empty Zod Array with Nonempty Method (TypeScript)
DESCRIPTION: Shows how to use the .nonempty() method to ensure that a Zod array schema requires at least one element, and notes its effect on the inferred type.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/packages/v3.mdx#_snippet_73

LANGUAGE: TypeScript
CODE:
```
const nonEmptyStrings = z.string().array().nonempty();
// the inferred type is now
// [string, ...string[]]

nonEmptyStrings.parse([]); // throws: "Array cannot be empty"
nonEmptyStrings.parse(["Ariana Grande"]); // passes
```

----------------------------------------

TITLE: Using Zod 4 `z.discriminatedUnion` with Complex Types (TypeScript)
DESCRIPTION: Shows how Zod 4's `z.discriminatedUnion` now supports more complex discriminator types like unions, pipes, and nested objects, allowing for more flexible schema definitions.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/v4/index.mdx#_snippet_42

LANGUAGE: typescript
CODE:
```
const MyResult = z.discriminatedUnion("status", [
  // simple literal
  z.object({ status: z.literal("aaa"), data: z.string() }),
  // union discriminator
  z.object({ status: z.union([z.literal("bbb"), z.literal("ccc")]) }),
  // pipe discriminator
  z.object({ status: z.object({ value: z.literal("fail") }) }),
]);
```

----------------------------------------

TITLE: Using Zod .catch() with Static Value (Zod Mini)
DESCRIPTION: Shows how Zod's `.catch()` method provides a static fallback value that is returned when the schema fails validation, using the Zod Mini syntax.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/api.mdx#_snippet_114

LANGUAGE: TypeScript
CODE:
```
const numberWithCatch = z.catch(z.number(), 42);

numberWithCatch.parse(5); // => 5
numberWithCatch.parse("tuna"); // => 42
```

----------------------------------------

TITLE: Validating During Zod Transform (TypeScript)
DESCRIPTION: Demonstrates how to perform validation checks simultaneously with transformation using the `ctx` object within the `.transform()` function. Issues can be added via `ctx.addIssue()`, and returning `z.NEVER` indicates a validation failure and exits the transform.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/packages/v3.mdx#_snippet_123

LANGUAGE: TypeScript
CODE:
```
const numberInString = z.string().transform((val, ctx) => {
  const parsed = parseInt(val);
  if (isNaN(parsed)) {
    ctx.addIssue({
      code: z.ZodIssueCode.custom,
      message: "Not a number"
    });

    // This is a special symbol you can use to
    // return early from the transform function.
    // It has type `never` so it does not affect the
    // inferred return type.
    return z.NEVER;
  }
  return parsed;
});
```

----------------------------------------

TITLE: Validating IP Addresses (IPv4 and IPv6) with Zod
DESCRIPTION: Provides examples for using `z.ipv4()` and `z.ipv6()` to validate strings as valid IP version 4 or version 6 addresses.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/api.mdx#_snippet_30

LANGUAGE: TypeScript
CODE:
```
const ipv4 = z.ipv4();
v4.parse("192.168.0.0"); // ✅

const ipv6 = z.ipv6();
v6.parse("2001:db8:85a3::8a2e:370:7334"); // ✅
```

----------------------------------------

TITLE: URL Validation with Protocol Constraint in Zod
DESCRIPTION: Explains how to restrict the valid protocols for a URL by providing a regular expression to the `protocol` parameter of the `url()` method.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/api.mdx#_snippet_22

LANGUAGE: ts
CODE:
```
const schema = z.url({ protocol: /^https$/ });

schema.parse("https://example.com"); // ✅
schema.parse("http://example.com"); // ❌
```

----------------------------------------

TITLE: Applying Type Refinements with Zod (TypeScript)
DESCRIPTION: Shows how to use a type predicate within `.superRefine()` to narrow the type of the validated value, allowing TypeScript to correctly infer the type for subsequent chained methods like `.refine()`. Note that `z.NEVER` is returned to satisfy typing, but the validation result depends on whether `ctx.addIssue` is called.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/packages/v3.mdx#_snippet_120

LANGUAGE: TypeScript
CODE:
```
const schema = z
  .object({
    first: z.string(),
    second: z.number(),
  })
  .nullable()
  .superRefine((arg, ctx): arg is { first: string; second: number } => {
    if (!arg) {
      ctx.addIssue({
        code: z.ZodIssueCode.custom, // customize your issue
        message: "object should exist"
      });
    }

    return z.NEVER; // The return value is not used, but we need to return something to satisfy the typing
  })
  // here, TS knows that arg is not null
  .refine((arg) => arg.first === "bob", "`first` is not `bob`!");
```

----------------------------------------

TITLE: Replacing Zod .and() with Intersection or Extend - TypeScript
DESCRIPTION: Shows how to achieve schema intersection in Zod v4 after the `.and()` method was dropped. The recommended alternatives are using the top-level `z.intersection()` function or, when applicable, the `.extend()` method for objects.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/v4/changelog.mdx#_snippet_15

LANGUAGE: typescript
CODE:
```
z.object({ a: z.string() }).and(z.object({ b: z.number() })); // ❌

// use z.intersection
z.intersection(z.object({ a: z.string() }), z.object({ b: z.number() })); // ✅
// or .extend() when possible
z.object({ a: z.string() }).extend(z.object({ b: z.number() })); // ✅
```

----------------------------------------

TITLE: Example ZodError with Custom Message
DESCRIPTION: Shows the structure of a ZodError object when a custom error message string is used, highlighting where the custom message appears.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/error-customization.mdx#_snippet_3

LANGUAGE: ts
CODE:
```
z.string("Not a string!").parse(12);
// ❌ throws ZodError {
//   issues: [
//     {
//       expected: 'string',
//       code: 'invalid_type',
//       path: [],
//       message: 'Not a string!'   <-- 👀 custom error message
//     }
//   ]
// }
```

----------------------------------------

TITLE: Using Zod 4 `.overwrite()` Method (TypeScript)
DESCRIPTION: Introduces the Zod 4 `.overwrite()` method, which is used for transformations that do not change the inferred type. Unlike `.transform()`, it returns the original schema type, making the output introspectable and compatible with features like JSON Schema conversion.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/v4/index.mdx#_snippet_47

LANGUAGE: typescript
CODE:
```
z.number().overwrite(val => val ** 2).max(100);
// => ZodNumber
```

----------------------------------------

TITLE: Making All Zod Object Properties Required (TypeScript)
DESCRIPTION: Applies the .required() method to a partial Zod object schema, making all its properties mandatory.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/packages/v3.mdx#_snippet_65

LANGUAGE: TypeScript
CODE:
```
const requiredUser = user.required();
// { email: string; username: string }
```

----------------------------------------

TITLE: Mapping Zod v3 to v4 Issue Types - TypeScript
DESCRIPTION: Provides a mapping of Zod v3 issue type names to their corresponding Zod v4 names or indicates if they were merged or dropped. This helps users understand how to update their error handling code from v3 to v4.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/v4/changelog.mdx#_snippet_11

LANGUAGE: typescript
CODE:
```
import { z } from "zod/v4"; // v3

export type IssueFormats =
  | z.ZodInvalidTypeIssue // ♻️ renamed to z.core.$ZodIssueInvalidType
  | z.ZodTooBigIssue  // ♻️ renamed to z.core.$ZodIssueTooBig
  | z.ZodTooSmallIssue // ♻️ renamed to z.core.$ZodIssueTooSmall
  | z.ZodInvalidStringIssue // ♻️ z.core.$ZodIssueInvalidStringFormat
  | z.ZodNotMultipleOfIssue // ♻️ renamed to z.core.$ZodIssueNotMultipleOf
  | z.ZodUnrecognizedKeysIssue // ♻️ renamed to z.core.$ZodIssueUnrecognizedKeys
  | z.ZodInvalidUnionIssue // ♻️ renamed to z.core.$ZodIssueInvalidUnion
  | z.ZodCustomIssue // ♻️ renamed to z.core.$ZodIssueCustom
  | z.ZodInvalidEnumValueIssue // ❌ merged in z.core.$ZodIssueInvalidValue
  | z.ZodInvalidLiteralIssue // ❌ merged into z.core.$ZodIssueInvalidValue
  | z.ZodInvalidUnionDiscriminatorIssue // ❌ throws an Error at schema creation time
  | z.ZodInvalidArgumentsIssue // ❌ z.function throws ZodError directly
  | z.ZodInvalidReturnTypeIssue // ❌ z.function throws ZodError directly
  | z.ZodInvalidDateIssue // ❌ merged into invalid_type
  | z.ZodInvalidIntersectionTypesIssue // ❌ removed (throws regular Error)
  | z.ZodNotFiniteIssue // ❌ infinite values no longer accepted (invalid_type)
```

----------------------------------------

TITLE: Validating Specific IP Versions with Zod
DESCRIPTION: Demonstrates using the `version` option with `z.string().ip()` to validate strings specifically as IPv4 (`v4`) or IPv6 (`v6`) addresses.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/packages/v3.mdx#_snippet_23

LANGUAGE: TypeScript
CODE:
```
const ipv4 = z.string().ip({ version: "v4" });
ipv4.parse("84d5:51a0:9114:1855:4cfa:f2d7:1f12:7003"); // fail

const ipv6 = z.string().ip({ version: "v6" });
ipv6.parse("192.168.1.1"); // fail
```

----------------------------------------

TITLE: Using Zod 4 `z.literal` with Multiple Values (TypeScript)
DESCRIPTION: Demonstrates the Zod 4 enhancement allowing `z.literal` to accept an array of values, simplifying the definition of schemas that match one of several specific literal values, compared to the Zod 3 approach using `z.union` of multiple `z.literal`s.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/v4/index.mdx#_snippet_44

LANGUAGE: typescript
CODE:
```
const httpCodes = z.literal([ 200, 201, 202, 204, 206, 207, 208, 226 ]);

// previously in Zod 3:
const httpCodes = z.union([
  z.literal(200),
  z.literal(201),
  z.literal(202),
  z.literal(204),
  z.literal(206),
  z.literal(207),
  z.literal(208),
  z.literal(226)
]);
```

----------------------------------------

TITLE: Generating Human-Readable Zod Error String with `z.prettifyError` (TypeScript)
DESCRIPTION: Uses `z.prettifyError` to generate a multi-line, human-readable string representation of the Zod error. This format is suitable for displaying validation errors directly to users, indicating the error message and the path where the error occurred.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/error-formatting.mdx#_snippet_4

LANGUAGE: ts
CODE:
```
const pretty = z.prettifyError(result.error);
```

----------------------------------------

TITLE: Using Zod .catch() with Function (Zod)
DESCRIPTION: Illustrates how Zod's `.catch()` method can accept a function that is executed to generate a fallback value when validation fails. The function receives context about the error in the standard Zod syntax.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/api.mdx#_snippet_115

LANGUAGE: TypeScript
CODE:
```
const numberWithRandomCatch = z.number().catch((ctx) => {
  ctx.error; // the caught ZodError
  return Math.random();
});

numberWithRandomCatch.parse("sup"); // => 0.4413456736055323
numberWithRandomCatch.parse("sup"); // => 0.1871840107401901
numberWithRandomCatch.parse("sup"); // => 0.7223408162401552
```

----------------------------------------

TITLE: Creating a Zod Record Schema with Union Keys
DESCRIPTION: This example shows how to define a Zod schema for a record where the keys can be a union of string, number, or symbol types using `z.union()` for the key schema. The value type is set to `z.unknown()`.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/api.mdx#_snippet_84

LANGUAGE: TypeScript
CODE:
```
const Keys = z.union([z.string(), z.number(), z.symbol()]);
const AnyObject = z.record(Keys, z.unknown());
// Record<string | number | symbol, unknown>
```

----------------------------------------

TITLE: Custom Error Message for Zod Array Nonempty (TypeScript)
DESCRIPTION: Demonstrates how to provide a custom error message when using the .nonempty() method on a Zod array schema.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/packages/v3.mdx#_snippet_74

LANGUAGE: TypeScript
CODE:
```
// optional custom error message
const nonEmptyStrings = z.string().array().nonempty({
  message: "Can't be empty!",
});
```

----------------------------------------

TITLE: Zod Intersection of Union Types
DESCRIPTION: Shows how to use `z.intersection()` to create a schema that represents the intersection of two Zod union schemas, resulting in a schema that accepts values present in both unions.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/api.mdx#_snippet_81

LANGUAGE: ts
CODE:
```
const a = z.union([z.number(), z.string()]);
const b = z.union([z.number(), z.boolean()]);
const c = z.intersection(a, b);

type c = z.infer<typeof c>; // => number
```

----------------------------------------

TITLE: Replacing Deprecated z.string().cidr() with Specific Methods (v4)
DESCRIPTION: Demonstrates the replacement of the deprecated `z.string().cidr()` method with specific `z.cidrv4()` and `z.cidrv6()` methods in Zod v4. Combine them with `z.union()` if necessary.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/v4/changelog.mdx#_snippet_19

LANGUAGE: TypeScript
CODE:
```
z.string().cidr() // ❌
z.cidrv4() // ✅
z.cidrv6() // ✅
```

----------------------------------------

TITLE: Validating ISO 8601 Dates with Zod
DESCRIPTION: Shows how to use `z.iso.date()` to validate strings strictly in the `YYYY-MM-DD` format.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/api.mdx#_snippet_27

LANGUAGE: TypeScript
CODE:
```
const date = z.iso.date();

date.parse("2020-01-01"); // ✅
date.parse("2020-1-1"); // ❌
date.parse("2020-01-32"); // ❌
```

----------------------------------------

TITLE: Example: Per-Parse Error Precedence
DESCRIPTION: Illustrates setting a custom error message when parsing a value using the `.parse()` method. This error takes precedence over global and locale errors.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/error-customization.mdx#_snippet_19

LANGUAGE: TypeScript
CODE:
```
z.string().parse(12, {
  error: (iss) => "My custom error"
});
```

----------------------------------------

TITLE: Defining Nested Zod Discriminated Unions
DESCRIPTION: Illustrates how to define discriminated unions where one of the variants is itself a discriminated union, demonstrating Zod's ability to handle nested structures efficiently.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/api.mdx#_snippet_80

LANGUAGE: ts
CODE:
```
const BaseError = { status: z.literal("failed"), message: z.string() };
const MyErrors = z.discriminatedUnion("code", [
  z.object({ ...BaseError, code: z.literal(400) }),
  z.object({ ...BaseError, code: z.literal(401) }),
  z.object({ ...BaseError, code: z.literal(500) }),
]);

const MyResult = z.discriminatedUnion("status", [
  z.object({ status: z.literal("success"), data: z.string() }),
  MyErrors
]);
```

----------------------------------------

TITLE: Installing Zod v3.25+ with npm
DESCRIPTION: Command to install the required version of the Zod library using npm, which supports Zod Mini and v4 features.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/v4/index.mdx#_snippet_11

LANGUAGE: sh
CODE:
```
npm install zod@^3.25.0
```

----------------------------------------

TITLE: Validating File Instances with Zod in TypeScript
DESCRIPTION: Introduces the `z.file()` schema type for validating `File` instances. Demonstrates how to apply constraints like minimum size (`.min()`), maximum size (`.max()`), and MIME type (`.type()`) to the file schema.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/v4/index.mdx#_snippet_28

LANGUAGE: TypeScript
CODE:
```
const fileSchema = z.file();

fileSchema.min(10_000); // minimum .size (bytes)
fileSchema.max(1_000_000); // maximum .size (bytes)
fileSchema.type("image/png"); // MIME type
```

----------------------------------------

TITLE: Aborting Validation on Refinement Failure
DESCRIPTION: Explains how to make a refinement non-continuable using the `abort: true` option. If this refinement fails, Zod stops processing subsequent checks. Includes example output showing only the first error.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/api.mdx#_snippet_96

LANGUAGE: ts
CODE:
```
const myString = z.string()
  .refine((val) => val.length > 8, { abort: true })
  .refine((val) => val === val.toLowerCase());


const result = myString.safeParse("OH NO");
result.error!.issues;
// => [{ "code": "custom", "message": "Too short!" }]
```

LANGUAGE: ts
CODE:
```
const myString = z.string().check(
  z.refine((val) => val.length > 8, { abort: true }),
  z.refine((val) => val === val.toLowerCase(), { abort: true })
);

const result = z.safeParse(myString, "OH NO");
result.error!.issues;
// [ { "code": "custom", "message": "Too short!" }]
```

----------------------------------------

TITLE: Using .describe() and .meta() for Schema Description in TypeScript
DESCRIPTION: Shows the equivalence between the older `.describe()` method and the preferred `.meta()` method for adding a simple description to a Zod schema. Both methods add descriptive text for documentation purposes.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/v4/index.mdx#_snippet_22

LANGUAGE: TypeScript
CODE:
```
z.string().describe("An email address");

// equivalent to
z.string().meta({ description: "An email address" });
```

----------------------------------------

TITLE: Using the toJSONSchema Configuration Object
DESCRIPTION: Demonstrates the basic syntax for passing a configuration object as the second argument to the `z.toJSONSchema` function to customize the conversion process.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/json-schema.mdx#_snippet_8

LANGUAGE: ts
CODE:
```
z.toJSONSchema(schema, {
  // ...params
})
```

----------------------------------------

TITLE: Using New BigInt Format Functions in Zod
DESCRIPTION: Shows the new top-level functions for defining `bigint` schemas with predefined constraints corresponding to 64-bit integer types. These functions return `ZodBigInt` instances suitable for values exceeding JavaScript's safe integer limit.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/v4/index.mdx#_snippet_36

LANGUAGE: TypeScript
CODE:
```
z.int64();    // [-9223372036854775808n, 92233372036854775807n]
z.uint64();   // [0n, 18446744073709551615n]
```

----------------------------------------

TITLE: Validating Specific CIDR Versions with Zod
DESCRIPTION: Demonstrates using the `version` option with `z.string().cidr()` to validate CIDR ranges specifically for IPv4 (`v4`) or IPv6 (`v6`).
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/packages/v3.mdx#_snippet_25

LANGUAGE: TypeScript
CODE:
```
const ipv4Cidr = z.string().cidr({ version: "v4" });
ipv4Cidr.parse("84d5:51a0:9114:1855:4cfa:f2d7:1f12:7003"); // fail

const ipv6Cidr = z.string().cidr({ version: "v6" });
ipv6Cidr.parse("192.168.1.1"); // fail
```

----------------------------------------

TITLE: Customizing Zod Date Error Messages
DESCRIPTION: Shows how to customize the error messages for required and invalid type errors when defining a Zod date schema.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/packages/v3.mdx#_snippet_33

LANGUAGE: ts
CODE:
```
const myDateSchema = z.date({
  required_error: "Please select a date and time",
  invalid_type_error: "That's not a date!"
});
```

----------------------------------------

TITLE: Creating Zod Enum Subsets with Extract/Exclude
DESCRIPTION: Illustrates how to create new Zod enum schemas that are subsets of an existing one using the `.extract()` (include only specified values) and `.exclude()` (exclude specified values) methods.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/packages/v3.mdx#_snippet_41

LANGUAGE: ts
CODE:
```
const FishEnum = z.enum(["Salmon", "Tuna", "Trout"]);
const SalmonAndTrout = FishEnum.extract(["Salmon", "Trout"]);
const TunaOnly = FishEnum.exclude(["Salmon", "Trout"]);
```

----------------------------------------

TITLE: Setting Custom Error Path with refine (TypeScript)
DESCRIPTION: Shows how to use the `path` option within the `.refine` parameters to specify where the validation error should appear in the error object's path.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/packages/v3.mdx#_snippet_114

LANGUAGE: ts
CODE:
```
const passwordForm = z
  .object({
    password: z.string(),
    confirm: z.string(),
  })
  .refine((data) => data.password === data.confirm, {
    message: "Passwords don't match",
    path: ["confirm"], // path of error
  });

passwordForm.parse({ password: "asdf", confirm: "qwer" });
```

----------------------------------------

TITLE: Customizing Truthy/Falsy Values for z.stringbool() in Zod
DESCRIPTION: Shows how to customize the set of strings considered truthy or falsy by `z.stringbool()` using the `truthy` and `falsy` options. This allows tailoring the coercion behavior to specific application requirements.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/v4/index.mdx#_snippet_38

LANGUAGE: TypeScript
CODE:
```
z.stringbool({
  truthy: ["yes", "true"],
  falsy: ["no", "false"]
})
```

----------------------------------------

TITLE: Example: Global Error Map Precedence
DESCRIPTION: Illustrates setting a global custom error map using `z.config()`. This error is used if no schema-level or per-parse error is defined.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/error-customization.mdx#_snippet_20

LANGUAGE: TypeScript
CODE:
```
z.config({
  customError: (iss) => "My custom error"
});
```

----------------------------------------

TITLE: Zod Branded Type Inferred Type
DESCRIPTION: Illustrates the inferred TypeScript type of a Zod schema after applying the `.brand()` method, showing the addition of the `$brand` property which enables nominal type checking.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/api.mdx#_snippet_119

LANGUAGE: TypeScript
CODE:
```
const Cat = z.object({ name: z.string() }).brand<"Cat">();
type Cat = z.infer<typeof Cat>; // { name: string } & z.$brand<"Cat">
```

----------------------------------------

TITLE: Zod v4 .default() Behavior Change
DESCRIPTION: Illustrates the new behavior of `.default()` in Zod v4, where it short-circuits parsing for `undefined` input and the default value must match the *output type*.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/v4/changelog.mdx#_snippet_21

LANGUAGE: TypeScript
CODE:
```
const schema = z.string()
  .transform(val => val.length)
  .default(0); // should be a number
schema.parse(undefined); // => 0
```

----------------------------------------

TITLE: Correctly Inferring Zod Schema Type with ZodTypeAny
DESCRIPTION: Presents the recommended approach for writing generic functions that accept Zod schemas by using `T extends z.ZodTypeAny`, which preserves the specific schema subclass type information. Includes an example showing the correct type inference.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/generic-functions.mdx#_snippet_1

LANGUAGE: TypeScript
CODE:
```
function inferSchema<T extends z.ZodTypeAny>(schema: T) {
  return schema;
}
```

LANGUAGE: TypeScript
CODE:
```
inferSchema(z.string());
// => ZodString
```

----------------------------------------

TITLE: Using the Zod Global Metadata Registry (TypeScript)
DESCRIPTION: Illustrates how to add schemas and common JSON Schema-compatible metadata to the built-in global registry provided by Zod v4.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/v4/index.mdx#_snippet_20

LANGUAGE: ts
CODE:
```
z.globalRegistry.add(z.string(), { 
  id: "email_address",
  title: "Email address",
  description: "Provide your email",
  examples: ["naomie@example.com"],
  extraKey: "Additional properties are also allowed"
});
```

----------------------------------------

TITLE: Fixing any Type in Generic Parse Functions with z.infer (TypeScript)
DESCRIPTION: Provides the solution to the `any` type issue in generic parse functions. By adding a type cast `as z.infer<T>` to the result of `schema.parse(data)`, the function correctly infers the output type of the schema.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/packages/v3.mdx#_snippet_149

LANGUAGE: TypeScript
CODE:
```
function parseData<T extends z.ZodTypeAny>(data: unknown, schema: T) {
  return schema.parse(data) as z.infer<T>;
  //                        ^^^^^^^^^^^^^^ <- add this
}

parseData("sup", z.string());
// => string
```

----------------------------------------

TITLE: Defining Basic Zod Function Schema (TypeScript)
DESCRIPTION: Demonstrates how to create a basic Zod function schema using `z.function()`. This schema represents a function with unknown arguments and return type. It also shows how to infer the TypeScript type from the schema.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/packages/v3.mdx#_snippet_97

LANGUAGE: TypeScript
CODE:
```
const myFunction = z.function();

type myFunction = z.infer<typeof myFunction>;
// => ()=>unknown
```

----------------------------------------

TITLE: Inferring Schema Type with ZodType Generic (v4)
DESCRIPTION: Demonstrates how the updated `ZodType` generic structure in Zod 4 allows generic functions involving `z.ZodType` to infer schema types more intuitively, such as correctly inferring `z.ZodString`.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/v4/changelog.mdx#_snippet_40

LANGUAGE: ts
CODE:
```
function inferSchema<T extends z.ZodType>(schema: T): T {
  return schema;
};

inferSchema(z.string()); // z.ZodString
```

----------------------------------------

TITLE: Per-Parse Error Customization
DESCRIPTION: Shows how to provide a custom error function directly to the `parse` method options for error customization specific to a single parsing operation.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/error-customization.mdx#_snippet_10

LANGUAGE: ts
CODE:
```
const schema = z.string()

schema.parse(12, {
  error: iss => "per-parse custom error"
});
```

----------------------------------------

TITLE: Converting Nullable and Optional Zod Types to JSON Schema in TypeScript
DESCRIPTION: Demonstrates how Zod's `null()`, `undefined()`, `optional()`, and `nullable()` types are converted into their corresponding JSON Schema representations, showing that both optional and nullable types result in a `oneOf` schema including the base type and `null`.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/json-schema.mdx#_snippet_22

LANGUAGE: ts
CODE:
```
z.null(); // => { type: "null" }
z.undefined(); // => { type: "null" }

z.optional(z.string());
// => { oneOf: [{ type: "string" }, { type: "null" }] }

z.nullable(z.string());
// => { oneOf: [{ type: "string" }, { type: "null" }] }
```

----------------------------------------

TITLE: Using Zod .default() with Transformations
DESCRIPTION: Contrasts `.default()` with `.prefault()`, showing that `.default()` returns the value directly without applying transformations when the input is `undefined`.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/api.mdx#_snippet_112

LANGUAGE: TypeScript
CODE:
```
const b = z.string().trim().toUpperCase().default("  tuna  ");
b.parse(undefined); // => "  tuna  "
```

----------------------------------------

TITLE: Using Zod .catch() with Function (Zod Mini)
DESCRIPTION: Demonstrates how Zod's `.catch()` method can accept a function to generate a fallback value on validation failure, using the Zod Mini syntax. The function receives context including the input value and issues.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/api.mdx#_snippet_116

LANGUAGE: TypeScript
CODE:
```
const numberWithRandomCatch = z.catch(z.number(), (ctx) => {
  ctx.value;   // the input value
  ctx.issues;  // the caught validation issue
  return Math.random();
});

z.parse(numberWithRandomCatch, "sup"); // => 0.4413456736055323
z.parse(numberWithRandomCatch, "sup"); // => 0.1871840107401901
z.parse(numberWithRandomCatch, "sup"); // => 0.7223408162401552
```

----------------------------------------

TITLE: Validating Local ISO 8601 Datetimes with Zod
DESCRIPTION: Illustrates how to use the `{ local: true }` option with `z.iso.datetime()` to validate timezone-less ISO 8601 datetime strings.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/api.mdx#_snippet_25

LANGUAGE: TypeScript
CODE:
```
const schema = z.iso.datetime({ local: true });
schema.parse("2020-01-01T00:00:00"); // ✅
```

----------------------------------------

TITLE: Converting Zod Schema with Metadata to JSON Schema in TypeScript
DESCRIPTION: Shows how metadata added via `.describe()` or `.meta()` on individual schema properties is automatically included in the generated JSON Schema when using `z.toJSONSchema()`. Demonstrates conversion for a more complex object schema.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/v4/index.mdx#_snippet_24

LANGUAGE: TypeScript
CODE:
```
const mySchema = z.object({
  firstName: z.string().describe("Your first name"),
  lastName: z.string().meta({ title: "last_name" }),
  age: z.number().meta({ examples: [12, 99] })
});

z.toJSONSchema(mySchema);
// => {
//   type: 'object',
//   properties: {
//     firstName: { type: 'string', description: 'Your first name' },
//     lastName: { type: 'string', title: 'last_name' },
//     age: { type: 'number', examples: [ 12, 99 ] }
//   },
//   required: [ 'firstName', 'lastName', 'age' ]
// }

```

----------------------------------------

TITLE: Fixing Parsed Data Type Inference with z.infer
DESCRIPTION: Provides the solution to the parsed type inference issue by adding a type cast using `z.infer<T>` to the return value of `schema.parse()`, ensuring the correct inferred type is returned. Includes an example showing the corrected type inference.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/generic-functions.mdx#_snippet_3

LANGUAGE: TypeScript
CODE:
```
function parseData<T extends z.ZodTypeAny>(data: unknown, schema: T) {
  return schema.parse(data) as z.infer<T>;
  //                        ^^^^^^^^^^^^^^ <- add this
}
```

LANGUAGE: TypeScript
CODE:
```
parseData("sup", z.string());
// => string
```

----------------------------------------

TITLE: Creating Custom Zod Schema with Validation (TypeScript)
DESCRIPTION: Demonstrates how to create a custom Zod schema using `z.custom()` for types not natively supported, such as template string literals. It includes a validation function to check if the input string matches the expected format (`/^d+px$/`). Also shows type inference and parsing.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/packages/v3.mdx#_snippet_103

LANGUAGE: TypeScript
CODE:
```
const px = z.custom<`${number}px`>((val) => {
  return typeof val === "string" ? /^\d+px$/.test(val) : false;
});

type px = z.infer<typeof px>; // `${number}px`

px.parse("42px"); // "42px"
px.parse("42vw"); // throws;
```

----------------------------------------

TITLE: Common BigInt Validations in Zod
DESCRIPTION: Lists various built-in validation methods available for BigInt schemas in Zod, including range checks (`gt`, `gte`, `lt`, `lte`), sign checks (`positive`, `nonnegative`, `negative`, `nonpositive`), and divisibility (`multipleOf`).
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/packages/v3.mdx#_snippet_29

LANGUAGE: TypeScript
CODE:
```
z.bigint().gt(5n);
z.bigint().gte(5n); // alias `.min(5n)`
z.bigint().lt(5n);
z.bigint().lte(5n); // alias `.max(5n)`

z.bigint().positive(); // > 0n
z.bigint().nonnegative(); // >= 0n
z.bigint().negative(); // < 0n
z.bigint().nonpositive(); // <= 0n

z.bigint().multipleOf(5n); // Evenly divisible by 5n.
```

----------------------------------------

TITLE: Issue Inferring Parsed Data Type with ZodTypeAny
DESCRIPTION: Illustrates a common problem when using `z.ZodTypeAny` in a generic function that parses data, where the return type of `schema.parse()` is incorrectly inferred as `any`. Includes an example demonstrating this issue.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/generic-functions.mdx#_snippet_2

LANGUAGE: TypeScript
CODE:
```
function parseData<T extends z.ZodTypeAny>(data: unknown, schema: T) {
  return schema.parse(data);
}
```

LANGUAGE: TypeScript
CODE:
```
parseData("sup", z.string());
// => any
```

----------------------------------------

TITLE: Using Zod .prefault() with Transformations
DESCRIPTION: Shows how `.prefault()` allows the provided value to pass through subsequent transformations and refinements defined in the schema when the input is `undefined`.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/api.mdx#_snippet_111

LANGUAGE: TypeScript
CODE:
```
const a = z.string().trim().toUpperCase().prefault("  tuna  ");
a.parse(undefined); // => "TUNA"
```

----------------------------------------

TITLE: Converting Zod Arrays and Tuples to JSON Schema in TypeScript
DESCRIPTION: Illustrates the conversion of Zod array and tuple schemas to JSON Schema. It shows how basic arrays map to `type: "array"` with `items`, how min/max constraints are added, and how tuples are represented using `prefixItems` and `additionalItems` for rest elements.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/json-schema.mdx#_snippet_23

LANGUAGE: ts
CODE:
```
z.array(z.string()); 
// => { type: "array", items: { type: "string" } }

z.array(z.string()).min(1).max(10); 
// => { type: "array", items: { type: "string" }, minItems: 1, maxItems: 10 }

// Supported via `type` and `prefixItems`
z.tuple([ z.string(),  z.number( ]); 
// => { type: "array", prefixItems: [{ type: "string" }, { type: "number" }] }

z.tuple([z.string()]).rest(z.boolean()); 
// => { type: "array", prefixItems: [{ type: "string" }], additionalItems: { type: "boolean" } }
```

----------------------------------------

TITLE: Template Literal Schema Examples
DESCRIPTION: Provides various examples of using `z.templateLiteral` with different combinations of string literals and schemas, demonstrating its flexibility.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/api.mdx#_snippet_127

LANGUAGE: ts
CODE:
```
z.templateLiteral([ "hi there" ]); 
// `hi there`

z.templateLiteral([ "email: ", z.string()]); 
// `email: ${string}`

z.templateLiteral([ "high", z.literal(5) ]); 
// `high5`

z.templateLiteral([ z.nullable(z.literal("grassy")) ]); 
// `grassy` | `null`

z.templateLiteral([ z.number(), z.enum(["px", "em", "rem"]) ]); 
// `${number}px` | `${number}em` | `${number}rem`
```

----------------------------------------

TITLE: Implementing Zod Function Schema with .implement() (TypeScript)
DESCRIPTION: Illustrates how to use the `.implement()` method on a Zod function schema to create a new function that automatically validates inputs and outputs according to the schema. The example defines a function that trims a string and returns its length.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/packages/v3.mdx#_snippet_99

LANGUAGE: TypeScript
CODE:
```
const trimmedLength = z
  .function()
  .args(z.string()) // accepts an arbitrary number of arguments
  .returns(z.number())
  .implement((x) => {
    // TypeScript knows x is a string!
    return x.trim().length;
  });

trimmedLength("sandwich"); // => 8
trimmedLength(" asdf "); // => 4
```

----------------------------------------

TITLE: Accessing Object Property Schemas with .shape - Zod TypeScript
DESCRIPTION: Demonstrates how to access the individual Zod schemas defined for each property within an object schema using the `.shape` property.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/packages/v3.mdx#_snippet_53

LANGUAGE: typescript
CODE:
```
Dog.shape.name; // => string schema
Dog.shape.age; // => number schema
```

----------------------------------------

TITLE: Defining BigInt Schema with Zod
DESCRIPTION: Creates a basic Zod schema for validating BigInt values. This is the fundamental way to define a BigInt type validator.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/api.mdx#_snippet_37

LANGUAGE: ts
CODE:
```
z.bigint();
```

----------------------------------------

TITLE: Resolving Recursive Zod Type Error with Annotation - TypeScript
DESCRIPTION: Provides a solution to the implicit any type error in recursive Zod schemas by explicitly adding a type annotation to the getter function.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/api.mdx#_snippet_69

LANGUAGE: TypeScript
CODE:
```
const Activity = z.object({
  name: z.string(),
  get subactivities(): z.ZodUnion<[z.ZodNull, typeof Activity]> { // ✅
    return z.union([z.null(), Activity]); 
  },
});
```

----------------------------------------

TITLE: Using metadata with toJSONSchema (Zod)
DESCRIPTION: Illustrates how to attach metadata to a Zod schema using the `.meta()` convenience method and how this metadata is included in the resulting JSON Schema when `toJSONSchema` is called.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/json-schema.mdx#_snippet_11

LANGUAGE: ts
CODE:
```
import { z } from "zod/v4";

// `.meta()` is a convenience method for registering a schema in `z.globalRegistry`
const emailSchema = z.string().meta({ 
  title: "Email address",
  description: "Your email address",
});

z.toJSONSchema(emailSchema);
// => { type: "string", title: "Email address", description: "Your email address", ... }
```

----------------------------------------

TITLE: Validating ISO 8601 Datetimes (Allow Offset) with Zod
DESCRIPTION: Shows how to configure `z.iso.datetime()` with the `{ offset: true }` option to allow timezone offsets in the ISO 8601 string.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/api.mdx#_snippet_24

LANGUAGE: TypeScript
CODE:
```
const datetime = z.iso.datetime({ offset: true });

datetime.parse("2020-01-01T00:00:00+02:00"); // ✅
datetime.parse("2020-01-01T00:00:00.123+02:00"); // ✅ (millis optional)
datetime.parse("2020-01-01T00:00:00.123+0200"); // ✅ (millis optional)
datetime.parse("2020-01-01T00:00:00.123+02"); // ✅ (only offset hours)
datetime.parse("2020-01-01T00:00:00Z"); // ✅ (Z still supported)
```

----------------------------------------

TITLE: Constraining Generic Zod Schema Inputs (TypeScript)
DESCRIPTION: Explains how to use the generic parameters of the `ZodType` class (`Output`, `Def`, `Input`) to constrain the types of schemas that can be passed to a generic function, ensuring type safety for specific schema operations.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/packages/v3.mdx#_snippet_150

LANGUAGE: TypeScript
CODE:
```
function makeSchemaOptional<T extends z.ZodType<string>>(schema: T) {
  return schema.optional();
}

makeSchemaOptional(z.string());
// works fine

makeSchemaOptional(z.number());
// Error: 'ZodNumber' is not assignable to parameter of type 'ZodType<string, ZodTypeDef, string>'
```

----------------------------------------

TITLE: Parsing Data with Zod Mini Schemas
DESCRIPTION: Demonstrates the standard parsing methods available on Zod Mini schemas, including synchronous and asynchronous parsing with and without error handling.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/packages/mini.mdx#_snippet_2

LANGUAGE: ts
CODE:
```
import { z } from "zod/v4-mini"

const mySchema = z.string();

mySchema.parse('asdf')
await mySchema.parseAsync('asdf')
mySchema.safeParse('asdf')
await mySchema.safeParseAsync('asdf')
```

----------------------------------------

TITLE: Incorrectly Inferring Zod Schema Type in TypeScript
DESCRIPTION: Shows an initial, incorrect attempt to write a generic function accepting a Zod schema using `z.ZodType<T>`, which loses specific schema subclass information. Includes an example demonstrating the resulting loss of type specificity.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/generic-functions.mdx#_snippet_0

LANGUAGE: TypeScript
CODE:
```
function inferSchema<T>(schema: z.ZodType<T>) {
  return schema;
}
```

LANGUAGE: TypeScript
CODE:
```
inferSchema(z.string());
// => ZodType<string>
```

----------------------------------------

TITLE: Using Zod .prefault()
DESCRIPTION: Illustrates how Zod's `.prefault()` method provides a value that is parsed when the input is `undefined`. The prefault value must match the schema's *input* type and does not short-circuit parsing, allowing transformations to apply.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/api.mdx#_snippet_110

LANGUAGE: TypeScript
CODE:
```
z.string().transform(val => val.length).prefault("tuna");
schema.parse(undefined); // => 4
```

----------------------------------------

TITLE: Parsing Promise Schema - Zod - TypeScript
DESCRIPTION: Explains the two-step validation process for promise schemas: synchronous validation of the input being a Promise instance, followed by asynchronous validation of the resolved value. Demonstrates how parsing works with non-promises and promises resolving to correct/incorrect types.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/packages/v3.mdx#_snippet_95

LANGUAGE: TypeScript
CODE:
```
numberPromise.parse("tuna");
// ZodError: Non-Promise type: string

numberPromise.parse(Promise.resolve("tuna"));
// => Promise<number>

const test = async () => {
  await numberPromise.parse(Promise.resolve("tuna"));
  // ZodError: Non-number type: string

  await numberPromise.parse(Promise.resolve(3.14));
  // => 3.14
};
```

----------------------------------------

TITLE: Example Usage of Implemented Zod Function (TypeScript)
DESCRIPTION: Demonstrates calling the implemented Zod function with valid inputs. The function executes the underlying logic after successful input validation and returns the result.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/api.mdx#_snippet_135

LANGUAGE: TypeScript
CODE:
```
trimmedLength("sandwich"); // => 8
trimmedLength(" asdf "); // => 4
```

----------------------------------------

TITLE: Adding Issues to ZodError in v4 - TypeScript
DESCRIPTION: Demonstrates the recommended way to add new issues to a `ZodError` instance in Zod v4. Instead of using deprecated methods like `.addIssue()`, users should directly push new issue objects into the `err.issues` array.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/v4/changelog.mdx#_snippet_14

LANGUAGE: typescript
CODE:
```
myError.issues.push({ 
  // new issue
});
```

----------------------------------------

TITLE: Extracting Values for Zod Enum Schema
DESCRIPTION: Shows how to create a new Zod enum schema that includes only specific values from the original set using the `.extract()` method. This method is available in standard Zod.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/api.mdx#_snippet_48

LANGUAGE: ts
CODE:
```
const FishEnum = z.enum(["Salmon", "Tuna", "Trout"]);
const SalmonAndTroutOnly = FishEnum.extract(["Salmon", "Trout"]);
```

----------------------------------------

TITLE: Extracting Parameters and Return Type from Zod Function Schema (TypeScript)
DESCRIPTION: Shows how to access the input schema (parameters) using `.parameters()` and the return type schema using `.returnType()` from a Zod function schema. This allows inspecting the defined types.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/packages/v3.mdx#_snippet_101

LANGUAGE: TypeScript
CODE:
```
myFunction.parameters();
// => ZodTuple<[ZodString, ZodNumber]>

myFunction.returnType();
// => ZodBoolean
```

----------------------------------------

TITLE: Defining Zod Enum from `as const` Tuple
DESCRIPTION: Demonstrates defining a Zod enum using a tuple of strings with the `as const` assertion, which is an alternative to passing a literal array.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/packages/v3.mdx#_snippet_37

LANGUAGE: ts
CODE:
```
const VALUES = ["Salmon", "Tuna", "Trout"] as const;
const FishEnum = z.enum(VALUES);
```

----------------------------------------

TITLE: Constraining Zod Schema Inputs in Generic Functions
DESCRIPTION: Explains how to use the generic parameters of `ZodType` (specifically `Output` in this example) to limit the types of schemas that can be passed to a generic function, providing type safety for function arguments. Includes examples showing a valid call and a call that results in a TypeScript error due to the constraint.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/generic-functions.mdx#_snippet_5

LANGUAGE: TypeScript
CODE:
```
function makeSchemaOptional<T extends z.ZodType<string>>(schema: T) {
  return schema.optional();
}
```

LANGUAGE: TypeScript
CODE:
```
makeSchemaOptional(z.string());
// works fine
```

LANGUAGE: TypeScript
CODE:
```
makeSchemaOptional(z.number());
// Error: 'ZodNumber' is not assignable to parameter of type 'ZodType<string, ZodTypeDef, string>'
```

----------------------------------------

TITLE: Representing Non-Empty Array Type with z.tuple() - Zod 4
DESCRIPTION: Shows how to achieve the Zod v3 non-empty array type inference ([string, ...string[]]) in Zod v4 using z.tuple() with a rest argument.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/v4/changelog.mdx#_snippet_29

LANGUAGE: TypeScript
CODE:
```
z.tuple([z.string()], z.string());
// => [string, ...string[]]
```

----------------------------------------

TITLE: Defining Zod Record Schema with Enum Keys
DESCRIPTION: This snippet illustrates how to create a Zod schema for an object with a fixed set of keys ('id', 'name', 'email') using `z.enum()` as the key schema. The values for these keys are expected to be strings.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/api.mdx#_snippet_85

LANGUAGE: TypeScript
CODE:
```
const Keys = z.enum(["id", "name", "email"]);
const Person = z.record(Keys, z.string());
// { id: string; name: string; email: string }
```

----------------------------------------

TITLE: Define Basic Template Literal Schema
DESCRIPTION: Introduces the `z.templateLiteral` API for defining schemas that validate against TypeScript template literal types, showing a simple example.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/api.mdx#_snippet_126

LANGUAGE: ts
CODE:
```
const schema = z.templateLiteral("hello, ", z.string(), "!");
// `hello, ${string}!`
```

----------------------------------------

TITLE: Adding Description with .describe in TypeScript
DESCRIPTION: Use .describe() to add a `description` property to the resulting schema. This can be useful for documenting a field, for example in a JSON Schema using a library like `zod-to-json-schema`.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/packages/v3.mdx#_snippet_128

LANGUAGE: TypeScript
CODE:
```
const documentedString = z
  .string()
  .describe("A useful bit of text, if you know what to do with it.");
documentedString.description; // A useful bit of text…
```

----------------------------------------

TITLE: Defining and Implementing z.function() - Zod 4 vs Zod 3
DESCRIPTION: Compares the API for defining and implementing Zod-validated functions in Zod v4 and v3. Zod v4 uses a factory pattern with input/output defined upfront, while v3 used chained methods.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/v4/changelog.mdx#_snippet_30

LANGUAGE: TypeScript
CODE:
```
const myFunction = z.function({
  input: [z.object({
    name: z.string(),
    age: z.number().int(),
  })],
  output: z.string(),
});

myFunction.implement((input) => {
  return `Hello ${input.name}, you are ${input.age} years old.`;
});
```

LANGUAGE: TypeScript
CODE:
```
const myFunction = z.function()
  .args(z.object({
    name: z.string(),
    age: z.number().int(),
  }))
  .returns(z.string());

myFunction.implement((input) => {
  return `Hello ${input.name}, you are ${input.age} years old.`;
});
```

----------------------------------------

TITLE: Intersecting Union Types - Zod - TypeScript
DESCRIPTION: Shows how to use `z.intersection` to find the common type between two union schemas. The inferred type of the resulting schema will be the intersection of the types in the unions.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/packages/v3.mdx#_snippet_90

LANGUAGE: TypeScript
CODE:
```
const a = z.union([z.number(), z.string()]);
const b = z.union([z.number(), z.boolean()]);
const c = z.intersection(a, b);

type c = z.infer<typeof c>; // => number
```

----------------------------------------

TITLE: Making a Zod Schema Nullable (Zod Mini)
DESCRIPTION: Shows how to make an existing Zod Mini schema accept `null` inputs using the `z.nullable()` utility function.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/api.mdx#_snippet_57

LANGUAGE: TypeScript
CODE:
```
const nullableYoda = z.nullable(z.literal("yoda"));
```

----------------------------------------

TITLE: Creating Branded Schema with .brand in TypeScript
DESCRIPTION: Use .brand<T>() to simulate nominal typing with Zod schemas. This attaches a 'brand' to the inferred type, preventing plain data structures from being assigned to the branded type.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/packages/v3.mdx#_snippet_139

LANGUAGE: TypeScript
CODE:
```
const Cat = z.object({ name: z.string() }).brand<"Cat">();
type Cat = z.infer<typeof Cat>;

const petCat = (cat: Cat) => {};

// this works
const simba = Cat.parse({ name: "simba" });
petCat(simba);

// this doesn't
petCat({ name: "fido" });
```

----------------------------------------

TITLE: Defining a Standalone Transform with z.transform
DESCRIPTION: Shows how to create a standalone transformation schema using the new `z.transform` function in Zod 4, which can be used independently or composed with other schemas.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/v4/changelog.mdx#_snippet_44

LANGUAGE: ts
CODE:
```
import { z } from "zod/v4";

const schema = z.transform(input => String(input));

schema.parse(12); // => "12"
```

----------------------------------------

TITLE: Zod Datetime Validation with Offset (TypeScript)
DESCRIPTION: Shows how to configure `z.string().datetime()` with `{ offset: true }` to allow ISO 8601 strings that include timezone offsets.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/packages/v3.mdx#_snippet_16

LANGUAGE: typescript
CODE:
```
const datetime = z.string().datetime({ offset: true });

datetime.parse("2020-01-01T00:00:00+02:00"); // pass
datetime.parse("2020-01-01T00:00+02:00"); // pass
datetime.parse("2020-01-01T00:00:00.123+02:00"); // pass (millis optional)
datetime.parse("2020-01-01T00:00:00.123+0200"); // pass (millis optional)
datetime.parse("2020-01-01T00:00:00.123+02"); // pass (only offset hours)
datetime.parse("2020-01-01T00:00:00Z"); // pass (Z still supported)
```

----------------------------------------

TITLE: Using Zod .brand() for Nominal Typing
DESCRIPTION: Shows how Zod's `.brand()` method can be used to simulate nominal typing in TypeScript, preventing assignment between structurally identical types that have different brands.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/api.mdx#_snippet_118

LANGUAGE: TypeScript
CODE:
```
const Cat = z.object({ name: z.string() }).brand<"Cat">();
const Dog = z.object({ name: z.string() }).brand<"Dog">();

type Cat = z.infer<typeof Cat>; // { name: string } & z.$brand<"Cat">
type Dog = z.infer<typeof Dog>; // { name: string } & z.$brand<"Dog">

const pluto = Dog.parse({ name: "pluto" });
const simba: Cat = pluto; // ❌ not allowed
```

----------------------------------------

TITLE: Defining and Parsing Stringbool Schema in Zod
DESCRIPTION: Creates a Zod schema that parses various string representations of boolean values (like "true", "1", "yes", "on") into actual boolean primitives. Shows default behavior and example parsing results.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/api.mdx#_snippet_49

LANGUAGE: TypeScript
CODE:
```
const strbool = z.stringbool();

strbool.parse("true")         // => true
strbool.parse("1")            // => true
strbool.parse("yes")          // => true
strbool.parse("on")           // => true
strbool.parse("y")            // => true
strbool.parse("enable")       // => true

strbool.parse("false");       // => false
strbool.parse("0");           // => false
strbool.parse("no");          // => false
strbool.parse("off");         // => false
strbool.parse("n");           // => false
strbool.parse("disabled");    // => false

strbool.parse(/* anything else */); // ZodError<[{ code: "invalid_value" }]>
```

----------------------------------------

TITLE: Replacing Deprecated z.string().ip() with Specific Methods (v4)
DESCRIPTION: Shows how the deprecated `z.string().ip()` method is replaced by separate `z.ipv4()` and `z.ipv6()` methods in Zod v4. Use `z.union()` if you need to accept both IP versions.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/v4/changelog.mdx#_snippet_18

LANGUAGE: TypeScript
CODE:
```
z.string().ip() // ❌
z.ipv4() // ✅
z.ipv6() // ✅
```

----------------------------------------

TITLE: Parsing with Zod Coerced String Schema (TypeScript)
DESCRIPTION: Provides examples of parsing various input types using a `z.coerce.string()` schema, demonstrating how Zod converts them to strings.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/api.mdx#_snippet_2

LANGUAGE: ts
CODE:
```
const schema = z.coerce.string();

schema.parse("tuna");    // => "tuna"
schema.parse(42);        // => "42"
schema.parse(true);      // => "true"
schema.parse(null);      // => "null"
```

----------------------------------------

TITLE: Validating IP Blocks (CIDR) with Zod
DESCRIPTION: Shows how to use `z.string().cidrv4()` and `z.string().cidrv6()` to validate strings formatted using CIDR notation for IPv4 and IPv6 address ranges.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/api.mdx#_snippet_31

LANGUAGE: TypeScript
CODE:
```
const cidrv4 = z.string().cidrv4();
cidrv4.parse("192.168.0.0/24"); // ✅

const cidrv6 = z.string().cidrv6();
cidrv6.parse("2001:db8::/32"); // ✅
```

----------------------------------------

TITLE: Zod 4 Record with Enum Keys
DESCRIPTION: Shows the updated behavior of `z.record` with enum key schemas in Zod version 4. The inferred type is no longer partial, and Zod enforces exhaustiveness during parsing, ensuring all enum keys are present.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/v4/changelog.mdx#_snippet_38

LANGUAGE: ts
CODE:
```
const myRecord = z.record(z.enum(["a", "b", "c"]), z.number());
// { a: number; b: number; c: number; }
```

----------------------------------------

TITLE: Implementing Transform with Validation Context in Zod TypeScript
DESCRIPTION: Shows how to use the validation context (`ctx`) within a Zod transform. This example attempts to parse a value as an integer and reports a custom validation issue using `ctx.issues.push` if parsing fails. It uses `z.NEVER` to indicate a validation failure without affecting the inferred return type.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/api.mdx#_snippet_102

LANGUAGE: TypeScript
CODE:
```
const coercedInt = z.transform((val, ctx) => {
  try {
    const parsed = Number.parseInt(String(val));
    return parsed;
  } catch (e) {
    ctx.issues.push({
      code: "custom",
      message: "Not a number",
      input: val,
    });

    // this is a special constant with type `never`
    // returning it lets you exit the transform without impacting the inferred return type
    return z.NEVER; 
  }
});
```

----------------------------------------

TITLE: Inlining Reused Schemas in Zod to JSON Schema (TypeScript)
DESCRIPTION: Shows the default behavior of `z.toJSONSchema` when a schema part is reused multiple times. By default, the reused schema (`z.string()` in this case) is inlined at each location in the output JSON Schema.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/json-schema.mdx#_snippet_18

LANGUAGE: ts
CODE:
```
const name = z.string();
const User = z.object({
  firstName: name,
  lastName: name
});

z.toJSONSchema(User);
// => {
//   type: 'object',
//   properties: { 
//     firstName: { type: 'string' }, 
//     lastName: { type: 'string' } 
//   },
//   required: [ 'firstName', 'lastName' ]
// }
```

----------------------------------------

TITLE: Customizing Date Schema Error Messages in Zod
DESCRIPTION: Explains how to provide a custom error message function when defining a date schema. This allows for more specific feedback based on the validation issue.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/api.mdx#_snippet_41

LANGUAGE: ts
CODE:
```
z.date({
  error: issue => issue.input === undefined ? "Required" : "Invalid date"
});
```

----------------------------------------

TITLE: Customizing Error Message for Zod Custom Schema (TypeScript)
DESCRIPTION: Illustrates how to provide a custom error message as the second argument to `z.custom()`. This allows specifying a more informative error when validation fails, similar to the `params` parameter in `.refine()`.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/packages/v3.mdx#_snippet_105

LANGUAGE: TypeScript
CODE:
```
z.custom<...>((val) => ..., "custom error message");
```

----------------------------------------

TITLE: Validating Unknown Keys with Catchall Schema in Zod Object (TypeScript)
DESCRIPTION: Illustrates using the .catchall() method with a schema to validate all unknown keys in the input data against the provided schema.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/packages/v3.mdx#_snippet_70

LANGUAGE: TypeScript
CODE:
```
const person = z
  .object({
    name: z.string(),
  })
  .catchall(z.number());

person.parse({
  name: "bob dylan",
  validExtraKey: 61, // works fine
});

person.parse({
  name: "bob dylan",
  validExtraKey: false, // fails
});
// => throws ZodError
```

----------------------------------------

TITLE: Validating ISO 8601 Times (Constrained Precision) with Zod
DESCRIPTION: Shows how to use the `{ precision: number }` option with `z.iso.time()` to limit the allowed sub-second decimal places in the time string.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/api.mdx#_snippet_29

LANGUAGE: TypeScript
CODE:
```
const time = z.iso.time({ precision: 3 });

time.parse("00:00:00.123"); // ✅
time.parse("00:00:00.123456"); // ❌
time.parse("00:00:00"); // ❌
```

----------------------------------------

TITLE: Email Validation with Custom Pattern in Zod
DESCRIPTION: Explains how to override the default email validation regex by providing a custom pattern to the `email()` method.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/api.mdx#_snippet_15

LANGUAGE: ts
CODE:
```
z.email({ pattern: /your regex here/ });
```

----------------------------------------

TITLE: GUID Validation in Zod
DESCRIPTION: Introduces the `guid()` method for validating any UUID-like identifier that does not necessarily conform to the strict RFC 4122 byte 8 constraint.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/api.mdx#_snippet_19

LANGUAGE: ts
CODE:
```
z.guid();
```

----------------------------------------

TITLE: Aborting Early in Zod SuperRefine (TypeScript)
DESCRIPTION: Demonstrates how to stop validation early within a `superRefine` function by adding an issue with the `fatal: true` flag and returning `z.NEVER` to prevent subsequent checks from running.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/packages/v3.mdx#_snippet_119

LANGUAGE: TypeScript
CODE:
```
const schema = z.number().superRefine((val, ctx) => {
  if (val < 10) {
    ctx.addIssue({
      code: z.ZodIssueCode.custom,
      message: "should be >= 10",
      fatal: true,
    });

    return z.NEVER;
  }

  if (val !== 12) {
    ctx.addIssue({
      code: z.ZodIssueCode.custom,
      message: "should be twelve",
    });
  }
});
```

----------------------------------------

TITLE: Defining and Parsing a Zod Map Schema
DESCRIPTION: This snippet shows how to create a Zod schema for a JavaScript `Map` where keys are strings and values are numbers using `z.map()`. It includes an example of creating a `Map` instance and parsing it with the schema.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/api.mdx#_snippet_87

LANGUAGE: TypeScript
CODE:
```
const StringNumberMap = z.map(z.string(), z.number());
type StringNumberMap = z.infer<typeof StringNumberMap>; // Map<string, number>

const myMap: StringNumberMap = new Map();
myMap.set("one", 1);
myMap.set("two", 2);

StringNumberMap.parse(myMap);
```

----------------------------------------

TITLE: Creating Promise Schema - Zod - TypeScript
DESCRIPTION: Shows how to define a schema that validates a JavaScript Promise using `z.promise`. The schema checks that the input is a Promise and that its resolved value matches the provided schema.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/packages/v3.mdx#_snippet_94

LANGUAGE: TypeScript
CODE:
```
const numberPromise = z.promise(z.number());
```

----------------------------------------

TITLE: Applying Validation Methods to Set Schemas in Zod
DESCRIPTION: Illustrates various validation methods available for Zod Set schemas, such as `nonempty()`, `min()`, `max()`, and `size()`, to enforce constraints on the number of items in the set.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/packages/v3.mdx#_snippet_88

LANGUAGE: TypeScript
CODE:
```
z.set(z.string()).nonempty(); // must contain at least one item
z.set(z.string()).min(5); // must contain 5 or more items
z.set(z.string()).max(5); // must contain 5 or fewer items
z.set(z.string()).size(5); // must contain 5 items exactly
```

----------------------------------------

TITLE: Validating ISO 8601 Times (No Offset) with Zod
DESCRIPTION: Demonstrates `z.iso.time()` for validating strings in `HH:MM:SS[.s+]` format, allowing arbitrary sub-second precision but no timezone offsets.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/api.mdx#_snippet_28

LANGUAGE: TypeScript
CODE:
```
const time = z.iso.time();

time.parse("00:00:00"); // ✅
time.parse("09:52:31"); // ✅
time.parse("23:59:59.9999999"); // ✅ (arbitrary precision)

time.parse("00:00:00.123Z"); // ❌ (no `Z` allowed)
time.parse("00:00:00.123+02:00"); // ❌ (no offsets allowed)
```

----------------------------------------

TITLE: Converting Zod Objects to JSON Schema in TypeScript
DESCRIPTION: Shows how Zod object schemas are converted to JSON Schema, including standard objects with required properties, strict and loose interfaces controlling additional properties, handling of optional properties using the '?' syntax, and converting record types.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/json-schema.mdx#_snippet_24

LANGUAGE: ts
CODE:
```
z.object({ name: z.string() });
// => { type: "object", properties: { name: { type: "string" }}, required: ["name"] }

z.strictInterface({ name: z.string() })
// => { ..., additionalProperties: {not: {}} }

z.looseInterface({ name: z.string() })
// => { ..., additionalProperties: {} }

z.object({ "name?": z.string() });
// => { type: "object", properties: { name: { type: "string" }}, required: [] }

z.record(z.string(), z.string());
// => { type: "object", propertyNames: { type: "string" }, additionalProperties: { type: "string" } }
```

----------------------------------------

TITLE: Adding and Getting Schemas from a Zod Registry (TypeScript)
DESCRIPTION: Demonstrates how to add a schema and its associated metadata to a custom registry and how to retrieve the metadata for a given schema.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/v4/index.mdx#_snippet_18

LANGUAGE: ts
CODE:
```
const emailSchema = z.string().email();

myRegistry.add(emailSchema, { title: "Email address", description: "..." });
myRegistry.get(emailSchema);
// => { title: "Email address", ... }
```

----------------------------------------

TITLE: Zod Datetime Validation with Local Time (TypeScript)
DESCRIPTION: Demonstrates using `z.string().datetime({ local: true })` to validate ISO 8601 strings that do not include any timezone information (unqualified datetimes).
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/packages/v3.mdx#_snippet_17

LANGUAGE: typescript
CODE:
```
const schema = z.string().datetime({ local: true });
schema.parse("2020-01-01T00:00:00"); // pass
schema.parse("2020-01-01T00:00"); // pass
```

----------------------------------------

TITLE: Validating Class Instance - Zod - TypeScript
DESCRIPTION: Demonstrates using `z.instanceof` to create a schema that validates whether an input value is an instance of a specific JavaScript class. This is useful for validating objects from external libraries or custom classes.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/packages/v3.mdx#_snippet_96

LANGUAGE: TypeScript
CODE:
```
class Test {
  name: string;
}

const TestSchema = z.instanceof(Test);

const blob: any = "whatever";
TestSchema.parse(new Test()); // passes
TestSchema.parse(blob); // throws
```

----------------------------------------

TITLE: Defining Object Intersection - Zod - TypeScript
DESCRIPTION: Demonstrates how to create a schema that is the intersection of two object schemas using `z.intersection` or the `.and()` method. The resulting type includes properties from both input schemas. Note that `.merge()` is often preferred for object merging.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/packages/v3.mdx#_snippet_89

LANGUAGE: TypeScript
CODE:
```
const Person = z.object({
  name: z.string()
});

const Employee = z.object({
  role: z.string()
});

const EmployedPerson = z.intersection(Person, Employee);

// equivalent to:
const EmployedPerson = Person.and(Employee);
```

----------------------------------------

TITLE: Defining and Parsing a Zod Set Schema
DESCRIPTION: This example demonstrates how to create a Zod schema for a JavaScript `Set` containing numbers using `z.set()`. It shows how to create a `Set` instance and validate it using the `.parse()` method.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/api.mdx#_snippet_88

LANGUAGE: TypeScript
CODE:
```
const NumberSet = z.set(z.number());
type NumberSet = z.infer<typeof NumberSet>; // Set<number>

const mySet: NumberSet = new Set();
mySet.add(1);
mySet.add(2);
NumberSet.parse(mySet);
```

----------------------------------------

TITLE: Adding Rest Argument to Zod Tuple Schema (TypeScript)
DESCRIPTION: Demonstrates using the .rest() method to add a variadic argument schema to a Zod tuple, allowing for additional elements of a specified type.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/packages/v3.mdx#_snippet_77

LANGUAGE: TypeScript
CODE:
```
const variadicTuple = z.tuple([z.string()]).rest(z.number());
const result = variadicTuple.parse(["hello", 1, 2, 3]);
// => [string, ...number[]];
```

----------------------------------------

TITLE: Extracting Values from Zod Literal Schema (TypeScript)
DESCRIPTION: Demonstrates how to access the set of allowed literal values from a Zod literal schema using the `.values` property.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/api.mdx#_snippet_7

LANGUAGE: ts
CODE:
```
colors.values; // => Set<"red" | "green" | "blue">
```

----------------------------------------

TITLE: Configuring Zod Locales for Error Messages in TypeScript
DESCRIPTION: Shows how to configure the global locale for Zod error messages using the `z.config()` function and the `z.locales` API. Currently, only the English locale is available.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/v4/index.mdx#_snippet_29

LANGUAGE: TypeScript
CODE:
```
import { z } from "zod/v4";

// configure English locale (default)
z.config(z.locales.en());
```

----------------------------------------

TITLE: Defining Any and Unknown Schemas in Zod
DESCRIPTION: Explains how to create schemas that mirror TypeScript's `any` and `unknown` types, allowing any value to pass validation for these specific schema instances.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/api.mdx#_snippet_62

LANGUAGE: TypeScript
CODE:
```
// allows any values
z.any(); // inferred type: `any`
z.unknown(); // inferred type: `unknown`
```

----------------------------------------

TITLE: Create Custom Schema with Custom Error Message
DESCRIPTION: Shows how to pass a second argument to `z.custom()` to provide a custom error message or other options, similar to the `.refine` method.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/api.mdx#_snippet_132

LANGUAGE: ts
CODE:
```
z.custom<...>((val) => ..., "custom error message");
```

----------------------------------------

TITLE: Using .check() in zod/v4-mini
DESCRIPTION: Demonstrates the `.check()` method available in `zod/v4-mini`, which is used to compose multiple validations and transforms (referred to as 'checks') onto a schema.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/v4/changelog.mdx#_snippet_43

LANGUAGE: ts
CODE:
```
import { z } from "zod/v4-mini";

z.string().check(
  z.minLength(10),
  z.maxLength(100),
  z.toLowerCase(),
  z.trim(),
);
```

----------------------------------------

TITLE: Applying BigInt Validations in Zod
DESCRIPTION: Demonstrates various built-in validation methods available for BigInt schemas in both standard Zod and Zod Mini. Includes checks for greater than, less than, positive/negative, and multiples.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/api.mdx#_snippet_38

LANGUAGE: ts
CODE:
```
z.bigint().gt(5n);
z.bigint().gte(5n);                    // alias `.min(5n)`
z.bigint().lt(5n);
z.bigint().lte(5n);                    // alias `.max(5n)`
z.bigint().positive(); 
z.bigint().nonnegative(); 
z.bigint().negative(); 
z.bigint().nonpositive(); 
z.bigint().multipleOf(5n);             // alias `.step(5n)`
```

LANGUAGE: ts zod/v4-mini
CODE:
```
z.bigint().check(z.gt(5n));
z.bigint().check(z.gte(5n));           // alias `.min(5n)`
z.bigint().check(z.lt(5n));
z.bigint().check(z.lte(5n));           // alias `.max(5n)`
z.bigint().check(z.positive()); 
z.bigint().check(z.nonnegative()); 
z.bigint().check(z.negative()); 
z.bigint().check(z.nonpositive()); 
z.bigint().check(z.multipleOf(5n));    // alias `.step(5n)`
```

----------------------------------------

TITLE: Merging Discriminated Union Schemas in Zod
DESCRIPTION: Explains how to merge the options from two or more existing discriminated union schemas into a new discriminated union schema. This is achieved by accessing the `.options` array of each union and using array destructuring.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/packages/v3.mdx#_snippet_83

LANGUAGE: TypeScript
CODE:
```
const A = z.discriminatedUnion("status", [
  /* options */
]);
const B = z.discriminatedUnion("status", [
  /* options */
]);

const AB = z.discriminatedUnion("status", [...A.options, ...B.options]);
```

----------------------------------------

TITLE: Example ZodError with Custom Path (JSON)
DESCRIPTION: Displays the structure of a `ZodError` object when a custom `path` is specified for a validation issue using `.refine`.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/packages/v3.mdx#_snippet_115

LANGUAGE: json
CODE:
```
ZodError {
  issues: [{
    "code": "custom",
    "path": [ "confirm" ],
    "message": "Passwords don't match"
  }]
}
```

----------------------------------------

TITLE: ZodType Generic Parameters Structure
DESCRIPTION: Shows the generic parameters (`Output`, `Def`, `Input`) defined on the `ZodType` class, which can be used to constrain the types of schemas accepted by generic functions.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/generic-functions.mdx#_snippet_4

LANGUAGE: TypeScript
CODE:
```
class ZodType<
  Output = any,
  Def extends ZodTypeDef = ZodTypeDef,
  Input = Output
> { ... }
```

----------------------------------------

TITLE: Setting Global Locale using z.locales in Zod
DESCRIPTION: Configures Zod to use a specific locale for error messages by accessing the locale object directly from `z.locales` and passing it to `z.config()`. Shows examples for both the standard `zod/v4` and the `zod/v4-mini` packages.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/error-customization.mdx#_snippet_17

LANGUAGE: TypeScript
CODE:
```
import { z } from "zod/v4";

z.config(z.locales.en());
```

LANGUAGE: TypeScript
CODE:
```
import { z } from "zod/v4-mini"

z.config(z.locales.en());
```

----------------------------------------

TITLE: Listing All First-Party Zod Schema Subclasses ($ZodTypes) (TypeScript)
DESCRIPTION: Defines the `$ZodTypes` union type, which encompasses all standard, built-in schema subclasses provided by `zod/v4/core`. This type is useful for type checking and discrimination.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/packages/core.mdx#_snippet_2

LANGUAGE: ts
CODE:
```
export type $ZodTypes =
  | $ZodString
  | $ZodNumber
  | $ZodBigInt
  | $ZodBoolean
  | $ZodDate
  | $ZodSymbol
  | $ZodUndefined
  | $ZodNullable
  | $ZodNull
  | $ZodAny
  | $ZodUnknown
  | $ZodNever
  | $ZodVoid
  | $ZodArray
  | $ZodObject
  | $ZodUnion
  | $ZodIntersection
  | $ZodTuple
  | $ZodRecord
  | $ZodMap
  | $ZodSet
  | $ZodLiteral
  | $ZodEnum
  | $ZodPromise
  | $ZodLazy
  | $ZodOptional
  | $ZodDefault
  | $ZodTemplateLiteral
  | $ZodCustom
  | $ZodTransform
  | $ZodNonOptional
  | $ZodReadonly
  | $ZodNaN
  | $ZodPipe
  | $ZodSuccess
  | $ZodCatch
  | $ZodFile;
```

----------------------------------------

TITLE: Extracting Inner Schema from ZodNullable (Zod)
DESCRIPTION: Demonstrates how to retrieve the original schema that was wrapped by a `ZodNullable` instance using the `.unwrap()` method.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/api.mdx#_snippet_58

LANGUAGE: TypeScript
CODE:
```
nullableYoda.unwrap(); // ZodLiteral<"yoda">
```

----------------------------------------

TITLE: Example: Locale Error Map Precedence
DESCRIPTION: Illustrates setting a locale using `z.config()`. The locale's error messages are used if no schema-level, per-parse, or global custom error map is defined.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/error-customization.mdx#_snippet_21

LANGUAGE: TypeScript
CODE:
```
z.config(z.locales.en());
```

----------------------------------------

TITLE: Accessing Zod Enum Options via `.options`
DESCRIPTION: Demonstrates how to retrieve the list of allowable values from a Zod enum schema as a tuple using the `.options` property.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/packages/v3.mdx#_snippet_40

LANGUAGE: ts
CODE:
```
FishEnum.options; // ["Salmon", "Tuna", "Trout"];
```

----------------------------------------

TITLE: URL Validation with Hostname Constraint in Zod
DESCRIPTION: Shows how to restrict the valid hostnames for a URL by providing a regular expression to the `hostname` parameter of the `url()` method.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/api.mdx#_snippet_21

LANGUAGE: ts
CODE:
```
const schema = z.url({ hostname: /^example\.com$/ });

schema.parse("https://example.com"); // ✅
schema.parse("https://zombo.com"); // ❌
```

----------------------------------------

TITLE: Accessing Zod v4-mini Error Issues (TypeScript)
DESCRIPTION: Illustrates using safeParse with zod/v4-mini to handle validation errors. It shows how to access the error object, which is an instance of z.core.$ZodError, and inspect the .issues array, highlighting the potentially more concise default message compared to zod/v4.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/error-customization.mdx#_snippet_1

LANGUAGE: TypeScript
CODE:
```
import { z } from "zod/v4-mini";

const result = z.string().safeParse(12); // { success: false, error: z.core.$ZodError }
result.error.issues;
// [
//   {
//     expected: 'string',
//     code: 'invalid_type',
//     path: [],
//     message: 'Invalid input'
//   }
// ]
```

----------------------------------------

TITLE: Unwrapping Optional Schema with .unwrap - Zod TypeScript
DESCRIPTION: Illustrates how to retrieve the original schema that was wrapped by ZodOptional using the `.unwrap()` method. This is useful for accessing the base schema definition.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/packages/v3.mdx#_snippet_48

LANGUAGE: typescript
CODE:
```
const stringSchema = z.string();
const optionalString = stringSchema.optional();
optionalString.unwrap() === stringSchema; // true
```

----------------------------------------

TITLE: Defining Zod Variadic Tuple Schema
DESCRIPTION: Shows how to define a Zod tuple schema that includes a variadic 'rest' argument at the end, allowing for a variable number of elements of a specific type.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/api.mdx#_snippet_74

LANGUAGE: TypeScript
CODE:
```
const variadicTuple = z.tuple([z.string()], z.number());
// => [string, ...number[]];
```

----------------------------------------

TITLE: Using Zod Format Classes as Types or Checks (TypeScript)
DESCRIPTION: Shows how certain Zod classes, like `z.email()`, can function both as standalone schema types for parsing and as checks applied to existing schemas.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/packages/core.mdx#_snippet_11

LANGUAGE: typescript
CODE:
```
// as a type
z.email().parse("user@example.com");

// as a check
z.string().check(z.email()).parse("user@example.com")
```

----------------------------------------

TITLE: Defining Zod Function Schema with Input and Output (TypeScript)
DESCRIPTION: Defines a Zod function schema using `z.function()`, specifying the expected types for both input parameters (as an array or tuple) and the return value. This schema can then be used to create validated functions.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/api.mdx#_snippet_133

LANGUAGE: TypeScript
CODE:
```
const MyFunction = z.function({
  input: [z.string()], // parameters (must be an array or a ZodTuple)
  output: z.number()  // return type
});
```

----------------------------------------

TITLE: Define JSON Schema with z.json()
DESCRIPTION: Shows the simple convenience API `z.json()` for creating a Zod schema that validates any value that is JSON-encodable.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/api.mdx#_snippet_128

LANGUAGE: ts
CODE:
```
const jsonSchema = z.json();
```

----------------------------------------

TITLE: Accessing Inner Schema of Zod Array
DESCRIPTION: Shows how to retrieve the schema definition for the elements contained within a Zod array schema, illustrating the difference between standard Zod and Zod Mini.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/api.mdx#_snippet_71

LANGUAGE: TypeScript
CODE:
```
stringArray.unwrap(); // => string schema
```

LANGUAGE: TypeScript
CODE:
```
stringArray.def.element; // => string schema
```

----------------------------------------

TITLE: Using Zod .preprocess() for Type Coercion (TypeScript)
DESCRIPTION: Illustrates the use of `z.preprocess()` to apply a transformation to the input value *before* Zod's validation occurs. This is useful for tasks like type coercion, as shown by converting any input value to a string before validating it against `z.string()`.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/packages/v3.mdx#_snippet_102

LANGUAGE: TypeScript
CODE:
```
const castToString = z.preprocess((val) => String(val), z.string());
```

----------------------------------------

TITLE: Setting Global Locale in Zod
DESCRIPTION: Configures Zod to use a specific locale for error messages by importing the locale object and passing it to `z.config()`. Shows examples for both the standard `zod/v4` and the `zod/v4-mini` packages.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/error-customization.mdx#_snippet_15

LANGUAGE: TypeScript
CODE:
```
import { z } from "zod/v4";
import en from "zod/v4/locales/en"

z.config(en());
```

LANGUAGE: TypeScript
CODE:
```
import { z } from "zod/v4-mini"
import en from "zod/v4/locales/en"

z.config(en());
```

----------------------------------------

TITLE: Generating JSON Schema with Input/Output Option (io) in Zod
DESCRIPTION: Demonstrates how the `z.toJSONSchema` function generates JSON Schema based on a Zod schema. By default, it represents the output type. The `io: "input"` option can be used to generate the schema for the input type instead, which is useful for schemas with transformations, pipes, or coercions where input and output types differ.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/json-schema.mdx#_snippet_21

LANGUAGE: typescript
CODE:
```
const mySchema = z.string().transform(val => val.length).pipe(z.number());
// ZodPipe

const jsonSchema = z.toJSONSchema(mySchema); 
// => { type: "number" }

const jsonSchema = z.toJSONSchema(mySchema, { io: "input" }); 
// => { type: "string" }
```

----------------------------------------

TITLE: Referencing Inferred Types in Zod Registry Metadata (TypeScript)
DESCRIPTION: Demonstrates how to reference the inferred output type of a Zod schema within the registry's metadata type using `z.$output`. This allows metadata fields, like `examples`, to be type-checked against the schema's output type. It shows adding schemas with metadata referencing their inferred types.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/metadata.mdx#_snippet_13

LANGUAGE: TypeScript
CODE:
```
import { z } from "zod/v4";

type MyMeta = { examples: z.$output[] };
const myRegistry = z.registry<MyMeta>();

myRegistry.add(z.string(), { examples: ["hello", "world"] });
myRegistry.add(z.number(), { examples: [1, 2, 3] });
```

----------------------------------------

TITLE: Creating a Partial Record Schema with Enum Keys using Zod
DESCRIPTION: This example demonstrates how to use `z.partialRecord()` with an enum key schema to create a Zod schema for an object where the specified keys ('id', 'name', 'email') are optional. This replicates the Zod 3 behavior for enum keys.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/api.mdx#_snippet_86

LANGUAGE: TypeScript
CODE:
```
const Keys = z.enum(["id", "name", "email"]).or(z.never()); 
const Person = z.partialRecord(Keys, z.string());
// { id?: string; name?: string; email?: string }
```

----------------------------------------

TITLE: Defining Schemas (Zod Mini vs Standard Zod)
DESCRIPTION: Compare how schemas are defined in Zod Mini using functional composition versus the standard Zod using method chaining. Zod Mini favors functions for better tree-shaking.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/packages/mini.mdx#_snippet_1

LANGUAGE: ts
CODE:
```
import { z } from "zod/v4-mini"

const mySchema = z.nullable(z.optional(z.string()));
```

LANGUAGE: ts
CODE:
```
import { z } from "zod/v4";

const mySchema = z.string().optional().nullable();
```

----------------------------------------

TITLE: Accessing Check-Specific Issue Properties
DESCRIPTION: Demonstrates accessing properties specific to a particular check (like `minimum` and `inclusive` for a `min` check) from the issue context object (`iss`) within the error function.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/error-customization.mdx#_snippet_9

LANGUAGE: ts
CODE:
```
z.string().min(5, {
  error: (iss) => {
    // ...the same as above
    iss.minimum; // the minimum value
    iss.inclusive; // whether the minimum is inclusive
    return `Password must have ${iss.minimum} characters or more`;
  }
});
```

----------------------------------------

TITLE: Customizing Type/Required Errors (Zod 3)
DESCRIPTION: Using the deprecated `required_error` and `invalid_type_error` parameters to customize messages in Zod 3.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/v4/changelog.mdx#_snippet_5

LANGUAGE: typescript
CODE:
```
z.string({
  required_error: "This field is required",
  invalid_type_error: "Not a string", 
});
```

----------------------------------------

TITLE: Applying Size Constraints to Zod Set Schema (Zod)
DESCRIPTION: This snippet shows how to add size constraints to a Zod set schema using the `.min()`, `.max()`, and `.size()` methods available in the standard Zod library. These methods validate the number of items in the set.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/api.mdx#_snippet_89

LANGUAGE: TypeScript
CODE:
```
z.set(z.string()).min(5); // must contain 5 or more items
z.set(z.string()).max(5); // must contain 5 or fewer items
z.set(z.string()).size(5); // must contain 5 items exactly
```

----------------------------------------

TITLE: Defining Basic Recursive Schema - Zod - TypeScript
DESCRIPTION: Illustrates how to define a recursive schema in Zod using `z.lazy` for properties that reference the schema itself. A manual type definition (`z.ZodType`) is required due to TypeScript's limitations with inferring recursive types. Includes an example of parsing data against the recursive schema.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/packages/v3.mdx#_snippet_91

LANGUAGE: TypeScript
CODE:
```
const baseCategorySchema = z.object({
  name: z.string()
});

type Category = z.infer<typeof baseCategorySchema> & {
  subcategories: Category[];
};

const categorySchema: z.ZodType<Category> = baseCategorySchema.extend({
  subcategories: z.lazy(() => categorySchema.array())
});

categorySchema.parse({
  name: "People",
  subcategories: [
    {
      name: "Politicians",
      subcategories: [
        {
          name: "Presidents",
          subcategories: []
        }
      ]
    }
  ]
}); // passes
```

----------------------------------------

TITLE: Example Error Handling for Zod Function (TypeScript)
DESCRIPTION: Shows calling the implemented Zod function with an invalid input type. This triggers a `ZodError` because the input does not match the schema definition.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/api.mdx#_snippet_136

LANGUAGE: TypeScript
CODE:
```
computeTrimmedLength(42); // throws ZodError
```

----------------------------------------

TITLE: Accessing Options of a Discriminated Union Schema in Zod
DESCRIPTION: Shows how to access the array of individual schema options that compose a discriminated union schema using the `.options` property.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/packages/v3.mdx#_snippet_82

LANGUAGE: TypeScript
CODE:
```
myUnion.options; // [ZodObject<...>, ZodObject<...>]
```

----------------------------------------

TITLE: Zod Numeric Types Converted to JSON Schema (TypeScript)
DESCRIPTION: Shows how Zod numeric schema types like `number`, `float32`, `float64`, `int`, and `int32` are converted to their corresponding JSON Schema types (`number` or `integer`) and constraints.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/json-schema.mdx#_snippet_5

LANGUAGE: ts
CODE:
```
// number
z.number(); // => { type: "number" }
z.float32(); // => { type: "number", exclusiveMinimum: ..., exclusiveMaximum: ... }
z.float64(); // => { type: "number", exclusiveMinimum: ..., exclusiveMaximum: ... }

// integer
z.int(); // => { type: "integer" }
z.int32(); // => { type: "integer", exclusiveMinimum: ..., exclusiveMaximum: ... }
```

----------------------------------------

TITLE: Listing All Zod String Format Subclasses ($ZodStringFormatTypes) (TypeScript)
DESCRIPTION: Defines the `$ZodStringFormatTypes` union type, listing all subclasses of `$ZodString` that implement specific string formats like UUID, Email, URL, etc. These are used for validating string content.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/packages/core.mdx#_snippet_4

LANGUAGE: ts
CODE:
```
export type $ZodStringFormatTypes =
  | $ZodGUID
  | $ZodUUID
  | $ZodEmail
  | $ZodURL
  | $ZodEmoji
  | $ZodNanoID
  | $ZodCUID
  | $ZodCUID2
  | $ZodULID
  | $ZodXID
  | $ZodKSUID
  | $ZodISODateTime
  | $ZodISODate
  | $ZodISOTime
  | $ZodISODuration
  | $ZodIPv4
  | $ZodIPv6
  | $ZodCIDRv4
  | $ZodCIDRv6
  | $ZodBase64
  | $ZodBase64URL
  | $ZodE164
  | $ZodJWT
```

----------------------------------------

TITLE: Retrieving Schema Metadata with .meta() (TypeScript)
DESCRIPTION: Shows how to call the `.meta()` method without arguments on a schema instance to retrieve the metadata previously registered with it in the global registry.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/metadata.mdx#_snippet_9

LANGUAGE: TypeScript
CODE:
```
emailSchema.meta();
// => { id: "email_address", title: "Email address", ... }
```

----------------------------------------

TITLE: Converting Zod Special Types to JSON Schema in TypeScript
DESCRIPTION: Demonstrates the JSON Schema output for Zod's special types: `any()`, `unknown()`, and `never()`, showing how they map to empty objects or schemas with negation.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/json-schema.mdx#_snippet_25

LANGUAGE: ts
CODE:
```
z.any(); // => {}
z.unknown(); // => {}
z.never(); // => { not: {} }
```

----------------------------------------

TITLE: Customizing Zod String Validation Method Errors (TypeScript)
DESCRIPTION: Illustrates how to pass a custom error message as a second argument to specific Zod string validation methods like `min`, `max`, `email`, etc.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/packages/v3.mdx#_snippet_14

LANGUAGE: typescript
CODE:
```
z.string().min(5, { message: "Must be 5 or more characters long" });
z.string().max(5, { message: "Must be 5 or fewer characters long" });
z.string().length(5, { message: "Must be exactly 5 characters long" });
z.string().email({ message: "Invalid email address" });
z.string().url({ message: "Invalid url" });
z.string().emoji({ message: "Contains non-emoji characters" });
z.string().uuid({ message: "Invalid UUID" });
z.string().includes("tuna", { message: "Must include tuna" });
z.string().startsWith("https://", { message: "Must provide secure URL" });
z.string().endsWith(".com", { message: "Only .com domains allowed" });
z.string().datetime({ message: "Invalid datetime string! Must be UTC." });
z.string().date({ message: "Invalid date string!" });
z.string().time({ message: "Invalid time string!" });
z.string().ip({ message: "Invalid IP address" });
z.string().cidr({ message: "Invalid CIDR" });
```

----------------------------------------

TITLE: Converting Zod Registry to JSON Schema - TypeScript
DESCRIPTION: This snippet demonstrates how to convert the schemas registered in the global Zod registry into a single JSON Schema object. The resulting schema includes definitions for each registered schema under a 'schemas' property, with interlinked references ('$ref') using the registered IDs.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/json-schema.mdx#_snippet_27

LANGUAGE: typescript
CODE:
```
z.toJSONSchema(z.globalRegistry);
```

----------------------------------------

TITLE: Customizing Errors with errorMap (Zod 3)
DESCRIPTION: Using the `errorMap` function to customize error messages based on the issue and context in Zod 3.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/v4/changelog.mdx#_snippet_7

LANGUAGE: typescript
CODE:
```
z.string({
  errorMap: (issue, ctx) => {
    if (issue.code === "too_small") {
      return { message: `Value must be >${issue.minimum}` };
    }
    return { message: ctx.defaultError };
  },
});
```

----------------------------------------

TITLE: Creating Discriminated Union Schema in Zod
DESCRIPTION: Demonstrates how to create a discriminated union schema using `z.discriminatedUnion`. This method requires a common 'discriminator' key among the union options, enabling more efficient parsing and better error reporting compared to basic unions. The example uses 'status' as the discriminator key.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/packages/v3.mdx#_snippet_81

LANGUAGE: TypeScript
CODE:
```
const myUnion = z.discriminatedUnion("status", [
  z.object({ status: z.literal("success"), data: z.string() }),
  z.object({ status: z.literal("failed"), error: z.instanceof(Error) })
]);

myUnion.parse({ status: "success", data: "yippie ki yay" });
```

----------------------------------------

TITLE: Using Zod's .readonly() on Objects (TypeScript)
DESCRIPTION: Demonstrates how to use the `.readonly()` method on a Zod object schema. This freezes the parsed output object and marks its inferred type as readonly, preventing modification.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/packages/v3.mdx#_snippet_141

LANGUAGE: TypeScript
CODE:
```
const schema = z.object({ name: z.string() }).readonly();
type schema = z.infer<typeof schema>;
// Readonly<{name: string}>

const result = schema.parse({ name: "fido" });
result.name = "simba"; // error
```

----------------------------------------

TITLE: z.array().nonempty() Type Inference - Zod 3 vs Zod 4
DESCRIPTION: Illustrates the change in inferred type for z.array().nonempty() between Zod v3 and v4. In v4, it behaves like .min(1) and does not change the inferred type to a tuple.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/v4/changelog.mdx#_snippet_28

LANGUAGE: TypeScript
CODE:
```
const NonEmpty = z.array(z.string()).nonempty();

type NonEmpty = z.infer<typeof NonEmpty>; 
// Zod 3: [string, ...string[]]
// Zod 4: string[]
```

----------------------------------------

TITLE: Validating ISO 8601 Datetimes (Constrained Precision) with Zod
DESCRIPTION: Explains how to use the `{ precision: number }` option with `z.iso.datetime()` to limit the allowed sub-second decimal places.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/api.mdx#_snippet_26

LANGUAGE: TypeScript
CODE:
```
const datetime = z.iso.datetime({ precision: 3 });

datetime.parse("2020-01-01T00:00:00.123Z"); // ✅
datetime.parse("2020-01-01T00:00:00Z"); // ❌
datetime.parse("2020-01-01T00:00:00.123456Z"); // ❌
```

----------------------------------------

TITLE: Throwing Error on Cycles in Zod to JSON Schema (TypeScript)
DESCRIPTION: Demonstrates how to configure `z.toJSONSchema` using the `cycles` option set to "throw" to cause an error when a cyclical reference is detected in the schema.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/json-schema.mdx#_snippet_17

LANGUAGE: ts
CODE:
```
z.toJSONSchema(User, { cycles: "throw" });
// => throws Error
```

----------------------------------------

TITLE: Converting Unrepresentable Zod Types to Any (TypeScript)
DESCRIPTION: Shows how to use the `unrepresentable` option set to "any" in `z.toJSONSchema` to convert unrepresentable Zod types into an empty object `{}` (JSON Schema equivalent of `unknown`) instead of throwing an error.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/json-schema.mdx#_snippet_15

LANGUAGE: ts
CODE:
```
z.toJSONSchema(z.bigint(), { unrepresentable: "any" });
// => {}
```

----------------------------------------

TITLE: Customizing Number Schema Error Messages in Zod
DESCRIPTION: Shows how to provide custom error messages for `required_error` and `invalid_type_error` when defining a number schema using `z.number()`. This allows tailoring error feedback for missing or incorrectly typed inputs.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/packages/v3.mdx#_snippet_26

LANGUAGE: TypeScript
CODE:
```
const age = z.number({
  required_error: "Age is required",
  invalid_type_error: "Age must be a number"
});
```

----------------------------------------

TITLE: Accessing Zod Native Enum Underlying Object
DESCRIPTION: Shows how to access the original TypeScript enum or `as const` object that was passed to `z.nativeEnum()` using the `.enum` property of the resulting schema.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/packages/v3.mdx#_snippet_45

LANGUAGE: ts
CODE:
```
FruitEnum.enum.Apple; // "apple"
```

----------------------------------------

TITLE: Registering Schema with z.globalRegistry using .register() (TypeScript)
DESCRIPTION: Shows how to register a schema and its metadata with the global Zod registry using the `.register()` method.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/metadata.mdx#_snippet_7

LANGUAGE: TypeScript
CODE:
```
import { z } from "zod/v4";

const emailSchema = z.email().register(z.globalRegistry, { 
  id: "email_address",
  title: "Email address",
  description: "Your email address",
  examples: ["first.last@example.com"]
});
```

----------------------------------------

TITLE: Referencing Reused Schemas with $defs in Zod to JSON Schema (TypeScript)
DESCRIPTION: Demonstrates how to use the `reused` option set to "ref" in `z.toJSONSchema`. This extracts reused schema parts into the `$defs` section of the JSON Schema and references them using `$ref`.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/json-schema.mdx#_snippet_19

LANGUAGE: ts
CODE:
```
z.toJSONSchema(User, { reused: "ref" });
// => {
//   type: 'object',
//   properties: {
//     firstName: { '$ref': '#/$defs/__schema0' },
//     lastName: { '$ref': '#/$defs/__schema0' }
//   },
//   required: [ 'firstName', 'lastName' ],
//   '$defs': { __schema0: { type: 'string' } }
// }
```

----------------------------------------

TITLE: Accessing Enum Values from Zod Schema
DESCRIPTION: Demonstrates how to access the allowed enum values from a Zod enum schema. In standard Zod, this is via the `.enum` property; in Zod Mini, it's via `.def.entries`.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/api.mdx#_snippet_46

LANGUAGE: ts
CODE:
```
const FishEnum = z.enum(["Salmon", "Tuna", "Trout"]);

FishEnum.enum;
// => { Salmon: "Salmon", Tuna: "Tuna", Trout: "Trout" }
```

LANGUAGE: ts zod/v4-mini
CODE:
```
const FishEnum = z.enum(["Salmon", "Tuna", "Trout"]);

FishEnum.def.entries;
// => { Salmon: "Salmon", Tuna: "Tuna", Trout: "Trout" }
```

----------------------------------------

TITLE: z.refine() Ignoring Type Predicates - Zod 4 vs Zod 3
DESCRIPTION: Shows that in Zod v4, type predicates passed to .refine() no longer narrow the inferred type of the schema, unlike in Zod v3.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/v4/changelog.mdx#_snippet_32

LANGUAGE: TypeScript
CODE:
```
const mySchema = z.unknown().refine((val): val is string => {
  return typeof val === "string"
});

type MySchema = z.infer<typeof mySchema>; 
// Zod 3: `string`
// Zod 4: still `unknown`
```

----------------------------------------

TITLE: Validating Time Strings with Precision in Zod
DESCRIPTION: Shows how to use the `precision` option with `z.string().time()` to constrain the allowable decimal precision in the seconds part of the time string.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/packages/v3.mdx#_snippet_21

LANGUAGE: TypeScript
CODE:
```
const time = z.string().time({ precision: 3 });

time.parse("00:00:00.123"); // pass
time.parse("00:00:00.123456"); // fail
time.parse("00:00:00"); // fail
time.parse("00:00"); // fail
```

----------------------------------------

TITLE: Accessing Errors in Zod Treeified Error Structure (TypeScript)
DESCRIPTION: Demonstrates how to access specific error messages within the nested structure produced by `z.treeifyError`. Optional chaining (`?.`) is used to safely navigate the object tree and retrieve error arrays associated with specific paths like `username` or `favoriteNumbers[1]`.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/error-formatting.mdx#_snippet_3

LANGUAGE: ts
CODE:
```
tree.properties?.username?.errors;
// => ["Invalid input: expected string, received number"]

tree.properties?.favoriteNumbers?.items?.[1]?.errors;
// => ["Invalid input: expected number, received string"];
```

----------------------------------------

TITLE: Creating a Zod Metadata Registry (TypeScript)
DESCRIPTION: Shows how to create a new schema registry in Zod v4 using z.registry(), specifying the expected type for the metadata associated with schemas.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/v4/index.mdx#_snippet_17

LANGUAGE: ts
CODE:
```
import { z } from "zod/v4";

const myRegistry = z.registry<{ title: string; description: string }>();
```

----------------------------------------

TITLE: Accessing Options of Zod Union Schema
DESCRIPTION: Shows how to access the individual schema options defined within a Zod union schema, highlighting the difference between standard Zod and Zod Mini.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/api.mdx#_snippet_76

LANGUAGE: TypeScript
CODE:
```
stringOrNumber.options; // [ZodString, ZodNumber]
```

LANGUAGE: TypeScript
CODE:
```
stringOrNumber.def.options; // [ZodString, ZodNumber]
```

----------------------------------------

TITLE: Using Zod Schema .parseAsync() Method (TypeScript)
DESCRIPTION: Shows the `.parseAsync()` method, which is required when a schema includes asynchronous operations like async refinements or transforms. It returns a Promise that resolves with the parsed value if valid or rejects with an error if invalid.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/packages/v3.mdx#_snippet_107

LANGUAGE: TypeScript
CODE:
```
const stringSchema = z.string().refine(async (val) => val.length <= 8);

await stringSchema.parseAsync("hello"); // => returns "hello"
await stringSchema.parseAsync("hello world"); // => throws error
```

----------------------------------------

TITLE: Upgrading Zod using npm
DESCRIPTION: Instructs users on how to upgrade their Zod dependency to version 3.25.0 or higher using the npm package manager, which includes the Zod 4 release.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/v4/index.mdx#_snippet_0

LANGUAGE: shell
CODE:
```
npm upgrade zod@^3.25.0
```

----------------------------------------

TITLE: Zod Null and Undefined Converted to JSON Schema Null (TypeScript)
DESCRIPTION: Explains that Zod's `null()` and `undefined()` schemas are both converted to the JSON Schema type `{ type: "null" }`.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/json-schema.mdx#_snippet_6

LANGUAGE: ts
CODE:
```
z.null(); 
// => { type: "null" }

z.undefined(); 
// => { type: "null" }
```

----------------------------------------

TITLE: Parse and Attempt Modification of Readonly Schema Result (Zod Mini)
DESCRIPTION: Demonstrates parsing data with a readonly schema using `z.parse` in Zod Mini and shows that attempting to modify the resulting object throws a TypeError because the result is frozen.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/api.mdx#_snippet_125

LANGUAGE: ts
CODE:
```
const result = z.parse(ReadonlyUser, { name: "fido" });
result.name = "simba"; // throws TypeError
```

----------------------------------------

TITLE: Zod Types Not Representable in JSON Schema (TypeScript)
DESCRIPTION: Lists Zod schema types that do not have a direct or reasonable equivalent in the JSON Schema standard and therefore cannot be converted using `z.toJSONSchema()`. These types are marked with ❌.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/json-schema.mdx#_snippet_1

LANGUAGE: ts
CODE:
```
z.bigint(); // ❌
z.int64(); // ❌
z.symbol(); // ❌
z.void(); // ❌
z.date(); // ❌
z.map(); // ❌
z.set(); // ❌
z.file(); // ❌
z.transform(); // ❌
z.nan(); // ❌
z.custom(); // ❌
```

----------------------------------------

TITLE: Adding Metadata to Zod Schema with .meta() in TypeScript
DESCRIPTION: Demonstrates how to attach arbitrary metadata like ID, title, description, and examples to a Zod schema using the `.meta()` method. This method is immutable, returning a new schema instance.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/v4/index.mdx#_snippet_21

LANGUAGE: TypeScript
CODE:
```
z.string().meta({
  id: "email_address",
  title: "Email address",
  description: "Provide your email",
  examples: ["naomie@example.com"],
  // ...
});
```

----------------------------------------

TITLE: Define Readonly Object Schema (Zod Mini)
DESCRIPTION: Demonstrates how to define a Zod schema for an object and mark it as readonly using the `z.readonly()` function in Zod Mini, showing the inferred TypeScript type.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/api.mdx#_snippet_121

LANGUAGE: ts
CODE:
```
const ReadonlyUser = z.readonly(z.object({ name: z.string() }));
type ReadonlyUser = z.infer<typeof ReadonlyUser>;
// Readonly<{ name: string }>
```

----------------------------------------

TITLE: Importing Zod v4 Module
DESCRIPTION: Shows how to import the Zod library specifically from the "/v4" subpath, which is required to use the Zod 4 version after upgrading.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/v4/index.mdx#_snippet_1

LANGUAGE: typescript
CODE:
```
import { z } from "zod/v4";
```

----------------------------------------

TITLE: Importing Zod 4
DESCRIPTION: How to import the Zod library specifically from the v4 subpath after upgrading.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/v4/changelog.mdx#_snippet_1

LANGUAGE: typescript
CODE:
```
import { z } from "zod/v4";
```

----------------------------------------

TITLE: Zod v4 z.coerce Input Type Change
DESCRIPTION: Highlights the change in the input type for schemas created with `z.coerce`, which is now `unknown` in Zod v4, compared to the specific type in Zod 3.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/v4/changelog.mdx#_snippet_20

LANGUAGE: TypeScript
CODE:
```
const schema = z.coerce.string();
type schemaInput = z.input<typeof schema>;

// Zod 3: string;
// Zod 4: unknown;
```

----------------------------------------

TITLE: Defining Template Literal Schemas in Zod
DESCRIPTION: Demonstrates how to create schemas that validate strings matching a template literal pattern, combining literal strings with other schemas like `string`, `number`, or enums. Refinements on inner schemas are enforced.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/api.mdx#_snippet_64

LANGUAGE: TypeScript
CODE:
```
const hello = z.templateLiteral(["hello, ", z.string()]);
// `hello, ${string}`

const cssUnits = z.enum(["px", "em", "rem", "%"]);
const css = z.templateLiteral([z.number(), cssUnits ]);
// `${number}px` | `${number}em` | `${number}rem` | `${number}%`

const email = z.templateLiteral([
  z.string().min(1),
  "@",
  z.string().max(64),
]);
// `${string}@${string}` (the min/max refinements are enforced!)
```

----------------------------------------

TITLE: Discriminating Issue Types in Per-Parse Error Function
DESCRIPTION: Demonstrates how to use the `code` property of the issue context object (`iss`) within a per-parse error function to provide different custom messages based on the specific type of validation error.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/error-customization.mdx#_snippet_12

LANGUAGE: ts
CODE:
```
const result = schema.safeParse(12, {
  error: (iss) => {
    if (iss.code === "invalid_type") {
      return `invalid type, expected ${iss.expected}`;
    }
    if (iss.code === "too_small") {
      return `minimum is ${iss.minimum}`;
    }
    // ...
  }
})
```

----------------------------------------

TITLE: Converting Zod Registry to JSON Schema with Absolute URIs - TypeScript
DESCRIPTION: This snippet shows how to convert the registered schemas into a JSON Schema object while customizing the format of the '$ref' properties. It uses the 'uri' option in `z.toJSONSchema` with a function that generates absolute URIs based on the schema IDs.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/json-schema.mdx#_snippet_28

LANGUAGE: typescript
CODE:
```
z.toJSONSchema(z.globalRegistry, {
  uri: (id) => `https://example.com/${id}.json`
});
```

----------------------------------------

TITLE: Creating Promise Schema with .promise in TypeScript
DESCRIPTION: A convenience method for promise types. This creates a Zod schema that validates Promises resolving to the base schema's type.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/packages/v3.mdx#_snippet_135

LANGUAGE: TypeScript
CODE:
```
const stringPromise = z.string().promise(); // Promise<string>

// equivalent to
z.promise(z.string());
```

----------------------------------------

TITLE: Applying Number Validations (Zod Mini) with Zod
DESCRIPTION: Lists various number-specific validation methods available using the `.check()` method with helper functions in the Zod Mini library.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/api.mdx#_snippet_34

LANGUAGE: TypeScript
CODE:
```
z.number().check(z.gt(5));
z.number().check(z.gte(5));            // alias .min(5)
z.number().check(z.lt(5));
z.number().check(z.lte(5));            // alias .max(5)
z.number().check(z.positive()); 
z.number().check(z.nonnegative()); 
z.number().check(z.negative()); 
z.number().check(z.nonpositive()); 
z.number().check(z.multipleOf(5));     // alias .step(5)
```

----------------------------------------

TITLE: Setting the target JSON Schema version
DESCRIPTION: Shows how to use the `target` parameter in the configuration object to specify the desired JSON Schema draft version for the output.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/json-schema.mdx#_snippet_10

LANGUAGE: ts
CODE:
```
z.toJSONSchema(schema, { target: "draft-7" });
z.toJSONSchema(schema, { target: "draft-2020-12" });
```

----------------------------------------

TITLE: Defining Zod Function Schema with Args and Return Type (TypeScript)
DESCRIPTION: Shows how to define a Zod function schema with specific argument types using `.args()` and a specific return type using `.returns()`. The example uses `z.string()`, `z.number()`, and `z.boolean()`. It also demonstrates type inference.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/packages/v3.mdx#_snippet_98

LANGUAGE: TypeScript
CODE:
```
const myFunction = z
  .function()
  .args(z.string(), z.number()) // accepts an arbitrary number of arguments
  .returns(z.boolean());

type myFunction = z.infer<typeof myFunction>;
// => (arg0: string, arg1: number)=>boolean
```

----------------------------------------

TITLE: Composing Zod 4 Discriminated Unions (TypeScript)
DESCRIPTION: Illustrates the new ability in Zod 4 to compose discriminated unions by including one discriminated union schema (`MyErrors`) as a member of another (`MyResult`), enabling more modular schema design.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/v4/index.mdx#_snippet_43

LANGUAGE: typescript
CODE:
```
const BaseError = z.object({ status: z.literal("failed"), message: z.string() });
const MyErrors = z.discriminatedUnion("code", [
  BaseError.extend({ code: z.literal(400) }),
  BaseError.extend({ code: z.literal(401) }),
  BaseError.extend({ code: z.literal(500) })
]);

const MyResult = z.discriminatedUnion("status", [
  z.object({ status: z.literal("success"), data: z.string() }),
  MyErrors
]);
```

----------------------------------------

TITLE: Merging Object Schemas with .merge - Zod TypeScript
DESCRIPTION: Demonstrates how to combine two object schemas into a single schema using the `.merge()` method. Properties in the second schema override properties with the same key in the first schema.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/packages/v3.mdx#_snippet_56

LANGUAGE: typescript
CODE:
```
const BaseTeacher = z.object({ students: z.array(z.string()) });
const HasID = z.object({ id: z.string() });

const Teacher = BaseTeacher.merge(HasID);
type Teacher = z.infer<typeof Teacher>; // => { students: string[], id: string }
```

----------------------------------------

TITLE: Per-Parse Error Precedence
DESCRIPTION: Illustrates that schema-level custom error messages have higher precedence than custom error functions provided in the `parse` method options.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/error-customization.mdx#_snippet_11

LANGUAGE: ts
CODE:
```
const schema = z.string({ error: "highest priority" });
const result = schema.safeParse(12, {
  error: (iss) => "lower priority"
})

result.error.issues;
// [{ message: "highest priority", ... }]
```

----------------------------------------

TITLE: Creating Map Schema with z.map in Zod
DESCRIPTION: Shows how to create a Zod schema for a JavaScript `Map` using `z.map`. It takes schemas for the map's keys and values as arguments and demonstrates how to infer the corresponding TypeScript `Map` type.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/packages/v3.mdx#_snippet_86

LANGUAGE: TypeScript
CODE:
```
const stringNumberMap = z.map(z.string(), z.number());

type StringNumberMap = z.infer<typeof stringNumberMap>;
// type StringNumberMap = Map<string, number>
```

----------------------------------------

TITLE: Generic Parse Function Resulting in any Type (TypeScript)
DESCRIPTION: Highlights a potential issue when writing a generic parse function using `z.ZodTypeAny`. Due to TypeScript inference behavior, the result of `schema.parse(data)` might be typed as `any`.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/packages/v3.mdx#_snippet_148

LANGUAGE: TypeScript
CODE:
```
function parseData<T extends z.ZodTypeAny>(data: unknown, schema: T) {
  return schema.parse(data);
}

parseData("sup", z.string());
// => any
```

----------------------------------------

TITLE: Recursive Zod Object Type Error (Implicit Any) - TypeScript
DESCRIPTION: Illustrates a common TypeScript error (ts(7023)) that can occur with recursive Zod schemas when the getter's return type cannot be inferred, often due to complex types like unions.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/api.mdx#_snippet_68

LANGUAGE: TypeScript
CODE:
```
const Activity = z.object({
  name: z.string(),
  get subactivities() {
    // ^ ❌ 'subactivities' implicitly has return type 'any' because it does not 
    // // have a return type annotation and is referenced directly or indirectly 
    // in one of its return expressions.ts(7023)

    return z.union([z.null(), Activity]);
  },
});
```

----------------------------------------

TITLE: Extracting Inner Schema from ZodOptional (Zod Mini)
DESCRIPTION: Shows how to access the original schema wrapped by a `ZodMiniOptional` instance via the `.def.innerType` property.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/api.mdx#_snippet_55

LANGUAGE: TypeScript
CODE:
```
optionalYoda.def.innerType; // ZodMiniLiteral<"yoda">
```

----------------------------------------

TITLE: Applying Schema Checks (Zod Mini vs Standard Zod)
DESCRIPTION: Illustrates the difference in applying checks like min/max length, regex, and refinements. Standard Zod uses method chaining, while Zod Mini uses the `.check()` method with check functions.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/packages/mini.mdx#_snippet_3

LANGUAGE: ts
CODE:
```
import { z } from "zod/v4";

z.string()
  .min(5)
  .max(10)
  .refine(val => val.includes("@"))
  .trim()
```

LANGUAGE: ts
CODE:
```
import { z } from "zod/v4-mini"

z.string().check(
  z.minLength(5), 
  z.maxLength(10),
  z.refine(val => val.includes("@")),
  z.trim()
);
```

----------------------------------------

TITLE: Performing Operations on a Zod Registry (TypeScript)
DESCRIPTION: Shows how to add a schema with metadata, check for its existence, retrieve its metadata, and remove it from a Zod registry.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/metadata.mdx#_snippet_1

LANGUAGE: TypeScript
CODE:
```
const mySchema = z.string();

myRegistry.add(mySchema, { description: "A cool schema!"});
myRegistry.has(mySchema); // => true
myRegistry.get(mySchema); // => { description: "A cool schema!" }
myRegistry.remove(mySchema);
```

----------------------------------------

TITLE: Simulating Nominal Typing in TypeScript
DESCRIPTION: TypeScript's type system is structural. This example shows how structurally equivalent types are considered the same, allowing assignment between them.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/packages/v3.mdx#_snippet_138

LANGUAGE: TypeScript
CODE:
```
type Cat = { name: string };
type Dog = { name: string };

const petCat = (cat: Cat) => {};
const fido: Dog = { name: "fido" };
petCat(fido); // works fine
```

----------------------------------------

TITLE: Customizing Refine Params with a Function (TypeScript)
DESCRIPTION: Illustrates using a function as the second argument to `.refine` to dynamically generate refinement parameters, such as creating an error message based on the input value.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/packages/v3.mdx#_snippet_113

LANGUAGE: ts
CODE:
```
const longString = z.string().refine(
  (val) => val.length > 10,
  (val) => ({ message: `${val} is not more than 10 characters` })
);
```

----------------------------------------

TITLE: Creating Set Schema with z.set in Zod
DESCRIPTION: Demonstrates how to create a Zod schema for a JavaScript `Set` using `z.set`. It takes a schema for the set's elements as an argument and shows how to infer the corresponding TypeScript `Set` type.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/packages/v3.mdx#_snippet_87

LANGUAGE: TypeScript
CODE:
```
const numberSet = z.set(z.number());
type NumberSet = z.infer<typeof numberSet>;
// type NumberSet = Set<number>
```

----------------------------------------

TITLE: Zod v4 z.unknown() and z.any() Type Inference Change
DESCRIPTION: Illustrates how `z.unknown()` and `z.any()` are no longer inferred as 'key optional' within object types in Zod v4, changing the resulting inferred type structure.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/v4/changelog.mdx#_snippet_25

LANGUAGE: TypeScript
CODE:
```
const mySchema = z.object({
  a: z.any(),
  b: z.unknown()
});
// Zod 3: { a?: any; b?: unknown };
// Zod 4: { a: any; b: unknown };
```

----------------------------------------

TITLE: Customizing Specific Number Validation Error Messages in Zod
DESCRIPTION: Demonstrates how to provide a custom error message for a specific number validation method, such as `lte`, by passing a second argument to the validation method.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/packages/v3.mdx#_snippet_28

LANGUAGE: TypeScript
CODE:
```
z.number().lte(5, { message: "this👏is👏too👏big" });
```

----------------------------------------

TITLE: Creating a Custom Zod Registry with Metadata Type (TypeScript)
DESCRIPTION: Another example demonstrating the creation of a Zod registry that enforces a specific type for the associated metadata.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/metadata.mdx#_snippet_12

LANGUAGE: TypeScript
CODE:
```
import { z } from "zod/v4";

const myRegistry = z.registry<{ description: string };>();
```

----------------------------------------

TITLE: Registering Schema with z.globalRegistry using .meta() (TypeScript)
DESCRIPTION: Demonstrates the recommended approach using the `.meta()` method to register a schema and its metadata with the global Zod registry.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/metadata.mdx#_snippet_8

LANGUAGE: TypeScript
CODE:
```
const emailSchema = z.email().meta({ 
  id: "email_address",
  title: "Email address",
  description: "Please enter a valid email address"
});
```

----------------------------------------

TITLE: Validating Class Instances with Zod `instanceof`
DESCRIPTION: This snippet demonstrates how to use `z.instanceof()` to create a Zod schema that checks if the input value is an instance of a given class (`Test` in this example). It shows successful and failed parsing examples.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/api.mdx#_snippet_92

LANGUAGE: TypeScript
CODE:
```
class Test {
  name: string;
}

const TestSchema = z.instanceof(Test);

TestSchema.parse(new Test()); // ✅
TestSchema.parse("whatever"); // ❌
```

----------------------------------------

TITLE: Creating Enum from Object Keys with .keyof - Zod TypeScript
DESCRIPTION: Shows how to generate a ZodEnum schema whose valid values are the keys of an existing object schema using the `.keyof()` method.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/packages/v3.mdx#_snippet_54

LANGUAGE: typescript
CODE:
```
const keySchema = Dog.keyof();
keySchema; // ZodEnum<["name", "age"]>
```

----------------------------------------

TITLE: Accessing Zod Enum Values via `.enum`
DESCRIPTION: Shows how to access the underlying object containing the enum values for autocompletion and reference using the `.enum` property of a Zod enum schema.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/packages/v3.mdx#_snippet_39

LANGUAGE: ts
CODE:
```
FishEnum.enum.Salmon; // => autocompletes

FishEnum.enum;
/*
=> {
  Salmon: "Salmon",
  Tuna: "Tuna",
  Trout: "Trout",
}
*/
```

----------------------------------------

TITLE: Making Specific Zod Object Properties Required (TypeScript)
DESCRIPTION: Uses the .required() method with an argument to specify which properties of a partial Zod object schema should become required.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/packages/v3.mdx#_snippet_66

LANGUAGE: TypeScript
CODE:
```
const requiredEmail = user.required({
  email: true,
});
/*
{
  email: string;
  username?: string | undefined;
}
*/
```

----------------------------------------

TITLE: Customizing JSON Schema Output with Override Option (TypeScript)
DESCRIPTION: Explains how to use the `override` option in `z.toJSONSchema` to provide a callback function. This callback receives the Zod schema and the default generated JSON Schema, allowing direct modification of the JSON Schema output.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/json-schema.mdx#_snippet_20

LANGUAGE: ts
CODE:
```
const mySchema = /* ... */
z.toJSONSchema(mySchema, {
  override: (ctx)=>{
    ctx.zodSchema; // the original Zod schema
    ctx.jsonSchema; // the default JSON Schema

    // directly modify
    ctx.jsonSchema.whatever = "sup";
  }
});
```

----------------------------------------

TITLE: Example Error Output with Custom Path
DESCRIPTION: Provides an example of the `issues` array structure when a refinement uses the `path` option, demonstrating how the error is associated with a specific field.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/api.mdx#_snippet_98

LANGUAGE: ts
CODE:
```
const result = passwordForm.safeParse({ password: "asdf", confirm: "qwer" });
result.error.issues;
/* [{
  "code": "custom",
  "path": [ "confirm" ],
  "message": "Passwords don't match"
}] */
```

LANGUAGE: ts
CODE:
```
const result = z.safeParse(passwordForm, { password: "asdf", confirm: "qwer" });
result.error.issues;
/* [{
  "code": "custom",
  "path": [ "confirm" ],
  "message": "Passwords don't match"
}] */
```

----------------------------------------

TITLE: Comparing Zod Mini and Zod v4 API Styles (TypeScript)
DESCRIPTION: Demonstrates the difference in API style between Zod Mini (functional wrappers) and standard Zod v4 (method chaining) for common operations like optional, union, and extend.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/v4/index.mdx#_snippet_12

LANGUAGE: ts
CODE:
```
import { z } from "zod/v4-mini";

z.optional(z.string());

z.union([z.string(), z.number()]);

z.extend(z.object({ /* ... */ }), { age: z.number() });
```

LANGUAGE: ts
CODE:
```
import { z } from "zod/v4";

z.string().optional();

z.string().or(z.number());

z.object({ /* ... */ }).extend({ age: z.number() });
```

----------------------------------------

TITLE: Zod Datetime Validation with Precision (TypeScript)
DESCRIPTION: Illustrates using the `precision` option with `z.string().datetime()` to enforce a specific number of decimal places for sub-second precision.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/packages/v3.mdx#_snippet_18

LANGUAGE: typescript
CODE:
```
const datetime = z.string().datetime({ precision: 3 });

datetime.parse("2020-01-01T00:00:00.123Z"); // pass
datetime.parse("2020-01-01T00:00:00Z"); // fail
datetime.parse("2020-01-01T00:00Z"); // fail
datetime.parse("2020-01-01T00:00:00.123456Z"); // fail
```

----------------------------------------

TITLE: Defining Mutually Recursive Types in Zod v4 (Getters) in TypeScript
DESCRIPTION: Demonstrates how to define mutually recursive types in Zod v4, where two types reference each other. This is achieved using getter properties that return the schema of the other type.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/v4/index.mdx#_snippet_26

LANGUAGE: TypeScript
CODE:
```
const User = z.object({
  email: z.email(),
  get posts(){
    return z.array(Post)
  }
});

const Post = z.object({
  title: z.string(),
  get author(){
    return User
  }
});
```

----------------------------------------

TITLE: Excluding Values from Zod Enum Schema
DESCRIPTION: Illustrates how to create a new Zod enum schema that excludes specific values from the original set using the `.exclude()` method. This method is available in standard Zod.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/api.mdx#_snippet_47

LANGUAGE: ts
CODE:
```
const FishEnum = z.enum(["Salmon", "Tuna", "Trout"]);
const TunaOnly = FishEnum.exclude(["Salmon", "Trout"]);
```

----------------------------------------

TITLE: Handling Cycles with $ref in Zod to JSON Schema (TypeScript)
DESCRIPTION: Illustrates how `z.toJSONSchema` handles cyclical schema references by default. It uses the `$ref` property in the generated JSON Schema to point back to the root schema.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/json-schema.mdx#_snippet_16

LANGUAGE: ts
CODE:
```
const User = z.object({
  name: z.string(),
  get friend() {
    return User;
  }
});

toJSONSchema(User);
// => {
//   type: 'object',
//   properties: { name: { type: 'string' }, friend: { '$ref': '#' } },
//   required: [ 'name', 'friend' ]
// }
```

----------------------------------------

TITLE: Implementing Zod Function Schema with Inferred Return Type (TypeScript)
DESCRIPTION: Demonstrates implementing a Zod function schema using `.implement()` without explicitly defining the return type via `.returns()`. Zod infers the return type from the function's implementation. The example shows a function returning an array containing the input string's length.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/packages/v3.mdx#_snippet_100

LANGUAGE: TypeScript
CODE:
```
const myFunction = z
  .function()
  .args(z.string())
  .implement((arg) => {
    return [arg.length];
  });

myFunction; // (arg: string)=>number[]
```

----------------------------------------

TITLE: Correctly Accepting Zod Schemas (Preserves Type Info) (TypeScript)
DESCRIPTION: Provides the correct method for defining a function that accepts a Zod schema, using a generic parameter that extends the core type (`T extends z.core.$ZodType`), which preserves specific subclass type information.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/library-authors.mdx#_snippet_11

LANGUAGE: typescript
CODE:
```
function inferSchema<T extends z.core.$ZodType>(schema: T) {
  return schema;
}
```

----------------------------------------

TITLE: Using .describe() Method for Schema Description (TypeScript)
DESCRIPTION: Explains that `.describe()` is a shorthand for registering a schema in `z.globalRegistry` with only a `description` field, provided for compatibility with older Zod versions.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/metadata.mdx#_snippet_11

LANGUAGE: TypeScript
CODE:
```
const emailSchema = z.email();
emailSchema.describe("An email address");

// equivalent to
emailSchema.meta({ description: "An email address" });
```

----------------------------------------

TITLE: Defining Zod Function Schema with Input Only (TypeScript)
DESCRIPTION: Defines a Zod function schema using `z.function()`, specifying only the expected types for input parameters. Omitting the `output` field means only input validation will be performed by functions implemented from this schema.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/api.mdx#_snippet_137

LANGUAGE: TypeScript
CODE:
```
const MyFunction = z.function({
  input: [z.string()], // parameters (must be an array or a ZodTuple)
});
```

----------------------------------------

TITLE: Invalid Zod Enum Definition from Array
DESCRIPTION: Provides an example of an incorrect way to define a Zod enum by passing a mutable array, which Zod cannot use to infer exact values.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/packages/v3.mdx#_snippet_38

LANGUAGE: ts
CODE:
```
const fish = ["Salmon", "Tuna", "Trout"];
const FishEnum = z.enum(fish);
```

----------------------------------------

TITLE: Validating NaN with Zod
DESCRIPTION: Shows how to use the specific `z.nan()` schema to validate if a value is exactly `NaN`.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/api.mdx#_snippet_35

LANGUAGE: TypeScript
CODE:
```
z.nan().parse(NaN);              // ✅
z.nan().parse("anything else");  // ❌
```

----------------------------------------

TITLE: Apply Readonly to Various Schema Types (Zod)
DESCRIPTION: Shows how the `.readonly()` method affects the inferred TypeScript types for different Zod schema types including objects, arrays, tuples, maps, and sets.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/api.mdx#_snippet_122

LANGUAGE: ts
CODE:
```
z.object({ name: z.string() }).readonly(); // { readonly name: string }
z.array(z.string()).readonly(); // readonly string[]
z.tuple([z.string(), z.number()]).readonly(); // readonly [string, number]
z.map(z.string(), z.date()).readonly(); // ReadonlyMap<string, Date>
z.set(z.string()).readonly(); // ReadonlySet<string>
```

----------------------------------------

TITLE: .transform() Returns ZodPipe (v4)
DESCRIPTION: Highlights that in Zod 4, the `.transform()` method on a schema now returns an instance of `ZodPipe`, indicating a composition of the original schema and the transform.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/v4/changelog.mdx#_snippet_45

LANGUAGE: ts
CODE:
```
z.string().transform(val => val); // ZodPipe<ZodString, ZodTransform>
```

----------------------------------------

TITLE: Using Zod's safeParseAsync Alias .spa (TypeScript)
DESCRIPTION: Shows the convenient alias `.spa` which can be used as a shorthand for `.safeParseAsync`.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/packages/v3.mdx#_snippet_111

LANGUAGE: ts
CODE:
```
await stringSchema.spa("billie");
```

----------------------------------------

TITLE: z.preprocess() Returns ZodPipe (v4)
DESCRIPTION: Notes that similar to `.transform()`, the `z.preprocess()` function in Zod 4 now also returns a `ZodPipe` instance, representing the composition of the preprocess step and the subsequent schema.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/v4/changelog.mdx#_snippet_46

LANGUAGE: ts
CODE:
```
z.preprocess(val => val, z.string()); // ZodPipe<ZodTransform, ZodString>
```

----------------------------------------

TITLE: Checking safeParse Error Type
DESCRIPTION: Demonstrates that the error object returned by `.safeParse()` in Zod 4 does not extend the native JavaScript `Error` class for performance reasons.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/v4/changelog.mdx#_snippet_8

LANGUAGE: typescript
CODE:
```
const result = z.string().safeParse(12); 
result.error! instanceof Error; // => false
```

----------------------------------------

TITLE: Creating Zod Array Schemas (TypeScript)
DESCRIPTION: Demonstrates two equivalent syntaxes for creating a Zod array schema that validates an array of strings.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/packages/v3.mdx#_snippet_71

LANGUAGE: TypeScript
CODE:
```
const stringArray = z.array(z.string());

// equivalent
const stringArray = z.string().array();
```

----------------------------------------

TITLE: Example Output of z.prettifyError in TypeScript
DESCRIPTION: Provides an example of the formatted output string generated by the `z.prettifyError()` function for the sample `ZodError` shown in the previous snippet. Illustrates the structure of the pretty-printed error message.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/v4/index.mdx#_snippet_31

LANGUAGE: TypeScript
CODE:
```
✖ Unrecognized key: "extraField"
✖ Invalid input: expected string, received number
  → at username
✖ Invalid input: expected number, received string
  → at favoriteNumbers[1]
```

----------------------------------------

TITLE: Deprecated vs. New Zod String Format API Usage
DESCRIPTION: Illustrates the deprecated method syntax (`z.string().email()`) compared to the recommended new top-level API (`z.email()`) for string format validation in Zod v4.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/v4/changelog.mdx#_snippet_17

LANGUAGE: TypeScript
CODE:
```
z.string().email(); // ❌ deprecated
z.email(); // ✅ 
```

----------------------------------------

TITLE: ZodType Generic Structure Change (v3 vs v4)
DESCRIPTION: Illustrates the change in the generic structure of the base `ZodType` class between Zod 3 and Zod 4. The `Def` generic is removed, and the `Input` generic now defaults to `unknown` instead of `Output`.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/v4/changelog.mdx#_snippet_39

LANGUAGE: ts
CODE:
```
// Zod 3
class ZodType<Output, Def extends z.ZodTypeDef, Input = Output> {
  // ...
}

// Zod 4
class ZodType<Output = unknown, Input = unknown> {
  // ...
}
```

----------------------------------------

TITLE: Making a Zod Schema Nullish (Zod Mini)
DESCRIPTION: Shows how to make an existing Zod Mini schema accept both `null` and `undefined` inputs using the `z.nullish()` utility function.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/api.mdx#_snippet_61

LANGUAGE: TypeScript
CODE:
```
const nullishYoda = z.nullish(z.literal("yoda"));
```

----------------------------------------

TITLE: Validating CIDR Ranges with Zod
DESCRIPTION: Shows the default behavior of `z.string().cidr()`, which validates strings as IP address ranges using CIDR notation for both IPv4 and IPv6.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/packages/v3.mdx#_snippet_24

LANGUAGE: TypeScript
CODE:
```
const cidr = z.string().cidr();
cidr.parse("192.168.0.0/24"); // pass
cidr.parse("2001:db8::/32"); // pass
```

----------------------------------------

TITLE: Customizing NaN Schema Error Messages in Zod
DESCRIPTION: Shows how to provide custom error messages for `required_error` and `invalid_type_error` when defining a NaN schema using `z.nan()`. This allows tailoring error feedback for missing or incorrectly typed inputs.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/packages/v3.mdx#_snippet_30

LANGUAGE: TypeScript
CODE:
```
const isNaN = z.nan({
  required_error: "isNaN is required",
  invalid_type_error: "isNaN must be 'not a number'"
});
```

----------------------------------------

TITLE: Zod GlobalMeta Interface Definition (TypeScript)
DESCRIPTION: Provides the TypeScript interface definition for the metadata accepted by Zod's built-in `z.globalRegistry`.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/metadata.mdx#_snippet_6

LANGUAGE: TypeScript
CODE:
```
export interface GlobalMeta {
  id?: string;
  title?: string;
  description?: string;
  examples?: T[]; // T is the output type of the schema you are registering
  [k: string]: unknown; // accepts other properties too
}
```

----------------------------------------

TITLE: Incorrect Generic Function for Zod Schemas (TypeScript)
DESCRIPTION: Illustrates an incorrect approach to writing a generic function that accepts a Zod schema. Using `z.ZodType<T>` loses the specific schema subclass information, limiting type inference.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/packages/v3.mdx#_snippet_146

LANGUAGE: TypeScript
CODE:
```
function inferSchema<T>(schema: z.ZodType<T>) {
  return schema;
}

inferSchema(z.string());
// => ZodType<string>
```

----------------------------------------

TITLE: Checking parse Error Type
DESCRIPTION: Demonstrates that the error thrown by `.parse()` in Zod 4 still extends the native JavaScript `Error` class.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/v4/changelog.mdx#_snippet_9

LANGUAGE: typescript
CODE:
```
try {
  z.string().parse(12);
} catch (err) {
  console.log(err instanceof Error); // => true
}
```

----------------------------------------

TITLE: Unwrapping Nullable Schema with .unwrap - Zod TypeScript
DESCRIPTION: Illustrates how to retrieve the original schema that was wrapped by ZodNullable using the `.unwrap()` method. This allows access to the base schema definition.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/packages/v3.mdx#_snippet_51

LANGUAGE: typescript
CODE:
```
const stringSchema = z.string();
const nullableString = stringSchema.nullable();
nullableString.unwrap() === stringSchema; // true
```

----------------------------------------

TITLE: Static .create() Factories Dropped - Zod 3 (Removed)
DESCRIPTION: Mentions that static .create() methods on Zod classes have been removed in Zod v4; schema creation now relies solely on standalone factory functions (e.g., z.string()).
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/v4/changelog.mdx#_snippet_35

LANGUAGE: TypeScript
CODE:
```
z.ZodString.create(); // ❌
```

----------------------------------------

TITLE: Configuring Case Sensitivity for Zod Stringbool
DESCRIPTION: Shows how to make the `z.stringbool` schema case-sensitive by setting the `case` option to "sensitive". By default, the comparison is case-insensitive.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/api.mdx#_snippet_51

LANGUAGE: TypeScript
CODE:
```
z.stringbool({
  case: "sensitive"
});
```

----------------------------------------

TITLE: Available Check Functions in Zod Mini
DESCRIPTION: Lists the common check functions available in Zod Mini that are typically used with the `.check()` method. These functions replace the method-based checks in standard Zod.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/packages/mini.mdx#_snippet_4

LANGUAGE: ts
CODE:
```
z.lt(value);
z.lte(value); // alias: z.maximum()
z.gt(value);
z.gte(value); // alias: z.minimum()
z.positive();
z.negative();
z.nonpositive();
z.nonnegative();
z.multipleOf(value);
z.maxSize(value);
z.minSize(value);
z.size(value);
z.maxLength(value);
z.minLength(value);
z.length(value);
z.regex(regex);
z.lowercase();
z.uppercase();
z.includes(value);
z.startsWith(value);
z.endsWith(value);
z.property(key, schema);
z.mime(value);

// custom checks
z.refine()
z.check()   // replaces .superRefine()

// mutations (these do not change the inferred types)
z.overwrite(value => newValue);
z.normalize();
z.trim();
z.toLowerCase();
z.toUpperCase();
```

----------------------------------------

TITLE: Object Parsing Benchmark Results (Zod 3 vs Zod 4)
DESCRIPTION: Displays the output of the Zod object parsing benchmark using the Moltar library benchmark, comparing the performance of Zod 3 and Zod 4 and showing Zod 4 is significantly faster.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/v4/index.mdx#_snippet_6

LANGUAGE: shell
CODE:
```
$ pnpm bench object-moltar
benchmark      time (avg)             (min … max)       p75       p99      p999
------------------------------------------------- -----------------------------
• z.object() safeParse
------------------------------------------------- -----------------------------
zod3          805 µs/iter     (771 µs … 2'802 µs)    804 µs    928 µs  2'802 µs
zod4          124 µs/iter     (118 µs … 1'236 µs)    119 µs    231 µs    329 µs

summary for z.object() safeParse
  zod4
   6.5x faster than zod3
```

----------------------------------------

TITLE: Configuring Zod Peer Dependency in package.json
DESCRIPTION: This snippet shows how to declare Zod as a peer dependency in your library's package.json file. This ensures that users of your library provide their own Zod installation, preventing version conflicts.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/library-authors.mdx#_snippet_0

LANGUAGE: json
CODE:
```
{
  // ...
  "peerDependencies": {
    "zod": "^3.25.0"
  }
}
```

----------------------------------------

TITLE: Incorrectly Accepting Zod Schemas (Loses Type Info) (TypeScript)
DESCRIPTION: Presents an incorrect approach to defining a function that accepts a Zod schema, which results in losing specific subclass type information due to the generic parameter not extending the core type correctly.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/library-authors.mdx#_snippet_9

LANGUAGE: typescript
CODE:
```
import * as z from "zod/v4";

function inferSchema<T>(schema: z.core.$ZodType<T>) {
  return schema;
}
```

----------------------------------------

TITLE: Parse and Attempt Modification of Readonly Schema Result (Zod)
DESCRIPTION: Demonstrates parsing data with a readonly schema and shows that attempting to modify the resulting object throws a TypeError because the result is frozen.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/api.mdx#_snippet_124

LANGUAGE: ts
CODE:
```
const result = ReadonlyUser.parse({ name: "fido" });
result.name = "simba"; // throws TypeError
```

----------------------------------------

TITLE: Customizing Truthy/Falsy Values for Zod Stringbool
DESCRIPTION: Demonstrates how to configure the `z.stringbool` schema to accept a custom set of truthy and falsy string values instead of the default options.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/api.mdx#_snippet_50

LANGUAGE: TypeScript
CODE:
```
z.stringbool({
  truthy: ["yes", "true"],
  falsy: ["no", "false"]
})
```

----------------------------------------

TITLE: Defining a Never Schema in Zod
DESCRIPTION: Shows how to create a schema using `z.never()` which represents a type that can never occur. Any value passed to this schema will fail validation.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/api.mdx#_snippet_63

LANGUAGE: TypeScript
CODE:
```
z.never(); // inferred type: `never`
```

----------------------------------------

TITLE: Underlying JSON Schema Union Definition
DESCRIPTION: Illustrates the complex union schema definition that `z.json()` represents internally, showing it recursively allows strings, numbers, booleans, null, arrays, and records.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/api.mdx#_snippet_129

LANGUAGE: ts
CODE:
```
const jsonSchema = z.lazy(() => {
  return z.union([
    z.string(params), 
    z.number(), 
    z.boolean(), 
    z.null(), 
    z.array(jsonSchema), 
    z.record(z.string(), jsonSchema)
  ]);
});
```

----------------------------------------

TITLE: Loading Locales in Zod Mini
DESCRIPTION: Shows how to explicitly load a locale, such as English, in Zod Mini using `z.config()`, as no locale is loaded by default.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/packages/mini.mdx#_snippet_8

LANGUAGE: ts
CODE:
```
import { z } from "zod/v4-mini"

z.config(z.locales.en());
```

----------------------------------------

TITLE: Creating Custom Zod Schema Without Validation (TypeScript)
DESCRIPTION: Shows how to create a custom Zod schema using `z.custom()` without providing a validation function. In this case, the schema will accept any value, which can be risky as no validation is performed.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/packages/v3.mdx#_snippet_104

LANGUAGE: TypeScript
CODE:
```
z.custom<{ arg: string }>(); // performs no validation
```

----------------------------------------

TITLE: Discriminating Zod Checks by Type (TypeScript)
DESCRIPTION: Illustrates how to use a `switch` statement on the `_zod.def.check` property to determine the specific type of a `$ZodChecks` instance and handle different check types accordingly.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/packages/core.mdx#_snippet_8

LANGUAGE: typescript
CODE:
```
const check = {} as z.$ZodChecks;
const def = check._zod.def;

switch (def.check) {
  case "less_than":
  case "greater_than":
    // ...
    break;
}
```

----------------------------------------

TITLE: Importing from zod/v4/core Sub-package
DESCRIPTION: Shows how to import utilities and types directly from the new `zod/v4/core` sub-package, which contains code shared between different Zod distributions.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/v4/changelog.mdx#_snippet_41

LANGUAGE: ts
CODE:
```
import { z } from "zod/v4/core";

function handleError(iss: z.$ZodError) {
  // do stuff
}
```

----------------------------------------

TITLE: Traversing Zod Schemas Using the Definition Property (TypeScript)
DESCRIPTION: Provides an example function `walk` that demonstrates how to traverse Zod schemas. It shows casting a schema to `$ZodTypes` and using the `_zod.def.type` property within a switch statement to handle different schema types.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/packages/core.mdx#_snippet_3

LANGUAGE: ts
CODE:
```
export function walk(_schema: z.$ZodType) {
  const schema = _schema as z.$ZodTypes;
  const def = schema._zod.def;
  switch (def.type) {
    case "string": {
      // ...
      break;
    }
    case "object": {
      // ...
      break;
    }
  }
}
```

----------------------------------------

TITLE: Defining the Base Zod Check Class (TypeScript)
DESCRIPTION: Shows the basic structure of the `$ZodCheck` base class, which serves as the foundation for all Zod checks. It includes an internal `_zod` property.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/packages/core.mdx#_snippet_6

LANGUAGE: typescript
CODE:
```
export class $ZodCheck<in T = unknown> {
  _zod: { /* internals */}
}
```

----------------------------------------

TITLE: Registering Schema Metadata Inline with .register() (TypeScript)
DESCRIPTION: Shows how the `.register()` method can be used inline during schema definition to associate metadata with nested schemas within an object.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/metadata.mdx#_snippet_4

LANGUAGE: TypeScript
CODE:
```
const mySchema = z.object({
  name: z.string().register(myRegistry, { description: "The user's name" }),
  age: z.number().register(myRegistry, { description: "The user's age" })
})
```

----------------------------------------

TITLE: Handling safeParse Results with Discriminated Union (TypeScript)
DESCRIPTION: Shows how to check the `success` property of the object returned by `.safeParse` to conditionally handle either the validated data or the validation error.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/packages/v3.mdx#_snippet_109

LANGUAGE: ts
CODE:
```
const result = stringSchema.safeParse("billie");
if (!result.success) {
  // handle error then return
  result.error;
} else {
  // do something
  result.data;
}
```

----------------------------------------

TITLE: Understanding Zod Metadata Immutability (TypeScript)
DESCRIPTION: Illustrates that metadata is tied to a specific schema instance, and immutable Zod methods like `.refine()` return a new instance that does not inherit the metadata from the original.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/metadata.mdx#_snippet_10

LANGUAGE: TypeScript
CODE:
```
const A = z.string().meta({ description: "A cool string" });
A.meta(); // => { description: "A cool string" }

const B = A.refine(_ => true);
B.meta(); // => undefined
```

----------------------------------------

TITLE: Zod String Formats Converted to JSON Schema Format (TypeScript)
DESCRIPTION: Shows Zod string schema types that are converted to JSON Schema using the `format` keyword, providing examples like email, date-time, UUID, and URL.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/json-schema.mdx#_snippet_2

LANGUAGE: ts
CODE:
```
// Supported via `format`
z.email(); // => { type: "string", format: "email" }
z.iso.datetime(); // => { type: "string", format: "date-time" }
z.iso.date(); // => { type: "string", format: "date" }
z.iso.time(); // => { type: "string", format: "time" }
z.iso.duration(); // => { type: "string", format: "duration" }
z.ipv4(); // => { type: "string", format: "ipv4" }
z.ipv6(); // => { type: "string", format: "ipv6" }
z.uuid(); // => { type: "string", format: "uuid" }
z.guid(); // => { type: "string", format: "uuid" }
z.url(); // => { type: "string", format: "uri" }
```

----------------------------------------

TITLE: Install Zod Canary Version
DESCRIPTION: Provides commands to install the latest canary build of the Zod library, which includes changes from the most recent commit, using various package managers.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/packages/v3.mdx#_snippet_2

LANGUAGE: sh
CODE:
```
npm install zod@canary       # npm
deno add npm:zod@canary      # deno
yarn add zod@canary          # yarn
bun add zod@canary           # bun
pnpm add zod@canary          # pnpm
```

----------------------------------------

TITLE: Create Custom Schema Without Validation
DESCRIPTION: Warns about creating a custom schema using `z.custom()` without providing a validation function, noting that it will allow any value.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/api.mdx#_snippet_131

LANGUAGE: ts
CODE:
```
z.custom<{ arg: string }>(); // performs no validation
```

----------------------------------------

TITLE: Defining the Base Zod Schema Class $ZodType (TypeScript)
DESCRIPTION: Shows the basic structure of the `$ZodType` class, which serves as the base for all Zod schemas. It includes generic parameters for `Output` and `Input` types and an internal `_zod` property.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/packages/core.mdx#_snippet_1

LANGUAGE: ts
CODE:
```
export class $ZodType<Output = unknown, Input = unknown> {
  _zod: { /* internals */}
}
```

----------------------------------------

TITLE: Enforcing Metadata Type in Zod Registry (TypeScript)
DESCRIPTION: Illustrates how TypeScript enforces the defined metadata type when adding schemas to a Zod registry, showing a valid and an invalid example.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/metadata.mdx#_snippet_2

LANGUAGE: TypeScript
CODE:
```
myRegistry.add(mySchema, { description: "A cool schema!" }); // ✅
myRegistry.add(mySchema, { description: 123 }); // ❌
```

----------------------------------------

TITLE: Registering Zod Mini Schemas
DESCRIPTION: Shows how to register a Zod Mini schema within a registry using the `.register()` method, useful for metadata management.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/packages/mini.mdx#_snippet_5

LANGUAGE: ts
CODE:
```
const myReg = z.registry<{title: string}>();

z.string().register(myReg, { title: "My cool string schema" });
```

----------------------------------------

TITLE: Zod String Formats Converted to JSON Schema Pattern (TypeScript)
DESCRIPTION: Lists Zod string schema types that are converted to JSON Schema using the `pattern` keyword when a specific `format` or `contentEncoding` is not applicable.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/json-schema.mdx#_snippet_4

LANGUAGE: ts
CODE:
```
z.base64url();
z.cuid();
z.regex();
z.emoji();
z.nanoid();
z.cuid2();
z.ulid();
z.cidrv4();
z.cidrv6();
```

----------------------------------------

TITLE: Branding Zod Mini Schemas
DESCRIPTION: Demonstrates how to apply a brand to a Zod Mini schema using the `.brand()` method for creating branded types.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/packages/mini.mdx#_snippet_6

LANGUAGE: ts
CODE:
```
import { z } from "zod/v4-mini"

const USD = z.string().brand("USD");
```

----------------------------------------

TITLE: Lazily Loading and Setting Locale in Zod
DESCRIPTION: Demonstrates how to dynamically import a locale file based on a string identifier and then configure Zod to use it. Useful for internationalization in applications where the locale might change or needs to be loaded on demand.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/error-customization.mdx#_snippet_16

LANGUAGE: TypeScript
CODE:
```
import { z } from "zod/v4";

async function loadLocale(locale: string) {
  const { default: locale } = await import(`zod/v4/locales/${locale}`);
  z.config(locale());
};

await loadLocale("fr");
```

----------------------------------------

TITLE: ctx.path Removed in superRefine Context - Zod 4
DESCRIPTION: Indicates that the ctx.path property is no longer available within the context object provided to .superRefine() callbacks in Zod v4.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/v4/changelog.mdx#_snippet_33

LANGUAGE: TypeScript
CODE:
```
z.string().superRefine((val, ctx) => {
  ctx.path; // ❌ no longer available
});
```

----------------------------------------

TITLE: Cloning Zod Mini Schemas
DESCRIPTION: Explains how to create an identical copy of a Zod Mini schema using the `.clone()` method, providing the schema's definition.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/packages/mini.mdx#_snippet_7

LANGUAGE: ts
CODE:
```
const mySchema = z.string()

mySchema.clone(mySchema._zod.def);
```

----------------------------------------

TITLE: Zod 3 `.transform()` Output Type (TypeScript)
DESCRIPTION: Shows that in Zod 3, the `.transform()` method changes the schema's type to `ZodPipe` or `ZodTransform`, making the output type non-introspectable at runtime.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/v4/index.mdx#_snippet_48

LANGUAGE: typescript
CODE:
```
const Squared = z.number().transform(val => val ** 2);
// => ZodPipe<ZodNumber, ZodTransform>
```

----------------------------------------

TITLE: Zod 3 Refinement Chaining Limitation (TypeScript)
DESCRIPTION: Shows a limitation in Zod 3 where chaining methods like `.min()` after `.refine()` was not possible because `.refine()` returned a different type (`ZodEffects`) that didn't have the original schema methods.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/v4/index.mdx#_snippet_45

LANGUAGE: typescript
CODE:
```
z.string()
  .refine(val => val.includes("@"))
  .min(5);
// ^ ❌ Property 'min' does not exist on type ZodEffects<ZodString, string, string>
```

----------------------------------------

TITLE: Constraining Accepted Schema to a Specific Type (e.g., Object) (TypeScript)
DESCRIPTION: Shows how to define a function that only accepts Zod schemas of a specific subclass, such as `ZodObject`, by directly typing the schema parameter.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/library-authors.mdx#_snippet_14

LANGUAGE: typescript
CODE:
```

import * as z from "zod/v4";

// only accepts object schemas
function inferSchema<T>(schema: z.core.$ZodObject) {
  return schema;
}
```

----------------------------------------

TITLE: Zod Mini Bundle Size Example (TypeScript)
DESCRIPTION: A simple example demonstrating a basic schema definition and parsing using Zod Mini, used to illustrate its smaller bundle size compared to standard Zod.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/v4/index.mdx#_snippet_16

LANGUAGE: ts
CODE:
```
import { z } from "zod/v4-mini";

const schema = z.boolean();
schema.parse(false);
```

----------------------------------------

TITLE: Registering Schema with .register() Method (TypeScript)
DESCRIPTION: Demonstrates using the `.register()` method on a schema instance to add it to a registry, noting that this method returns the original schema instance.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/metadata.mdx#_snippet_3

LANGUAGE: TypeScript
CODE:
```
const mySchema = z.string();

mySchema.register(myRegistry, { description: "A cool schema!" });
// => mySchema
```

----------------------------------------

TITLE: Apply Readonly to Various Schema Types (Zod Mini)
DESCRIPTION: Shows how the `z.readonly()` function affects the inferred TypeScript types for different Zod Mini schema types including objects, arrays, tuples, maps, and sets.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/api.mdx#_snippet_123

LANGUAGE: ts
CODE:
```
z.readonly(z.object({ name: z.string() })); // { readonly name: string }
z.readonly(z.array(z.string())); // readonly string[]
z.readonly(z.tuple([z.string(), z.number()])); // readonly [string, number]
z.readonly(z.map(z.string(), z.date())); // ReadonlyMap<string, Date>
z.readonly(z.set(z.string())); // ReadonlySet<string>
```

----------------------------------------

TITLE: Default Error for Unrepresentable Zod Types (TypeScript)
DESCRIPTION: Demonstrates the default behavior of `z.toJSONSchema` when encountering an unrepresentable type like `z.bigint()`, which is to throw an error.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/json-schema.mdx#_snippet_14

LANGUAGE: ts
CODE:
```
z.toJSONSchema(z.bigint());
// => throws Error
```

----------------------------------------

TITLE: Refining Zod String (Old Syntax) - TypeScript
DESCRIPTION: Demonstrates the old, no longer supported syntax for the `.refine()` method on a Zod string schema. It shows how a separate function was used as the second argument to generate the error message based on the input value.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/v4/changelog.mdx#_snippet_47

LANGUAGE: typescript
CODE:
```
const longString = z.string().refine(
  (val) => val.length > 10,
  (val) => ({ message: `${val} is not more than 10 characters` })
);
```

----------------------------------------

TITLE: Result of Correct Schema Acceptance (TypeScript)
DESCRIPTION: Illustrates the type inferred by TypeScript when calling the correctly defined `inferSchema` function, showing that it returns the specific `ZodString` type, allowing access to subclass methods.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/library-authors.mdx#_snippet_12

LANGUAGE: typescript
CODE:
```
inferSchema(z.string());
// => ZodString
```

----------------------------------------

TITLE: Extracting Inner Schema from ZodNullable (Zod Mini)
DESCRIPTION: Shows how to access the original schema wrapped by a `ZodMiniNullable` instance via the `.def.innerType` property.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/api.mdx#_snippet_59

LANGUAGE: TypeScript
CODE:
```
nullableYoda.def.innerType; // ZodMiniLiteral<"yoda">
```

----------------------------------------

TITLE: Applying Size Constraints to Zod Set Schema (Zod Mini)
DESCRIPTION: This snippet demonstrates how to apply size constraints to a Zod set schema when using Zod Mini. It uses the `.check()` method along with utility functions like `z.minSize()`, `z.maxSize()`, and `z.size()`.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/api.mdx#_snippet_90

LANGUAGE: TypeScript
CODE:
```
z.set(z.string()).check(z.minSize(5)); // must contain 5 or more items
z.set(z.string()).check(z.maxSize(5)); // must contain 5 or fewer items
z.set(z.string()).check(z.size(5)); // must contain 5 items exactly
```

----------------------------------------

TITLE: Using metadata with toJSONSchema (Zod Mini)
DESCRIPTION: Demonstrates how to attach metadata to a Zod schema in Zod Mini using the `.register()` method with `z.globalRegistry`, showing that this metadata is included in the JSON Schema output from `toJSONSchema`.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/json-schema.mdx#_snippet_12

LANGUAGE: ts
CODE:
```
import { z } from "zod/v4";

// `.meta()` is a convenience method for registering a schema in `z.globalRegistry`
const emailSchema = z.string().register(z.globalRegistry, { 
  title: "Email address",
  description: "Your email address",
});

z.toJSONSchema(emailSchema);
// => { type: "string", title: "Email address", description: "Your email address", ... }
```

----------------------------------------

TITLE: Comparing Zod Mini and Zod v4 Refinement Methods (TypeScript)
DESCRIPTION: Illustrates the difference in applying multiple refinements using the .check() method in Zod Mini versus method chaining in standard Zod v4.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/v4/index.mdx#_snippet_14

LANGUAGE: ts
CODE:
```
import { z } from "zod/v4-mini";

z.array(z.number()).check(
  z.minLength(5), 
  z.maxLength(10),
  z.refine(arr => arr.includes(5))
);
```

LANGUAGE: ts
CODE:
```
import { z } from "zod/v4";

z.array(z.number())
  .min(5)
  .max(10)
  .refine(arr => arr.includes(5));
```

----------------------------------------

TITLE: Constraining Accepted Schema by Inferred Output Type (TypeScript)
DESCRIPTION: Explains how to define a function that accepts any Zod schema but constrains the *inferred output type* of that schema, providing examples of valid and invalid schema inputs based on the output type constraint.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/library-authors.mdx#_snippet_15

LANGUAGE: typescript
CODE:
```

import * as z from "zod/v4";

// only accepts object schemas
function inferSchema<T extends z.core.$ZodType<string>>(schema: T) {
  return schema;
}

inferSchema(z.string()); // ✅ 

inferSchema(z.number()); 
// ❌ The types of '_zod.output' are incompatible between these types. 
// // Type 'number' is not assignable to type 'string'
```

----------------------------------------

TITLE: Zod 4 Refinement Chaining Fix (TypeScript)
DESCRIPTION: Demonstrates that in Zod 4, refinements are stored internally, allowing methods like `.min()` to be chained directly after `.refine()`, fixing the limitation present in Zod 3.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/v4/index.mdx#_snippet_46

LANGUAGE: typescript
CODE:
```
z.string()
  .refine(val => val.includes("@"))
  .min(5); // ✅
```

----------------------------------------

TITLE: Zod 3 Record with Enum Keys
DESCRIPTION: Demonstrates the behavior of `z.record` when using an enum as the key schema in Zod version 3, resulting in a partial type where keys are optional.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/v4/changelog.mdx#_snippet_37

LANGUAGE: ts
CODE:
```
const myRecord = z.record(z.enum(["a", "b", "c"]), z.number()); 
// { a?: number; b?: number; c?: number; }
```

----------------------------------------

TITLE: Zod String Formats Converted to JSON Schema Content Encoding (TypeScript)
DESCRIPTION: Demonstrates how the Zod `base64()` schema is converted to JSON Schema using the `contentEncoding` keyword.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/json-schema.mdx#_snippet_3

LANGUAGE: ts
CODE:
```
z.base64(); // => { type: "string", contentEncoding: "base64" }
```

----------------------------------------

TITLE: Using Zod .spa Alias for safeParseAsync (TypeScript)
DESCRIPTION: Demonstrates the convenient alias .spa() for the .safeParseAsync() method. It performs the same asynchronous safe parsing operation but provides a shorter syntax.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/parsing.mdx#_snippet_5

LANGUAGE: TypeScript
CODE:
```
await stringSchema.spa("billie");
```

----------------------------------------

TITLE: Comparing Zod v3 vs v4 Error Map Precedence - TypeScript
DESCRIPTION: Illustrates how the precedence of error maps passed to `.parse()` versus defined on the schema has changed from Zod v3 to v4. In v4, the schema-level error map now takes precedence over the contextual error map provided during parsing.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/v4/changelog.mdx#_snippet_13

LANGUAGE: typescript
CODE:
```
const mySchema = z.string({ error: () => "Schema-level error" });

// in Zod 3
mySchema.parse(12, { error: () => "Contextual error" }); // => "Contextual error"

// in Zod 4
mySchema.parse(12, { error: () => "Contextual error" }); // => "Schema-level error"
```

----------------------------------------

TITLE: Zod Default Email Regex
DESCRIPTION: Displays the default regular expression used by Zod's `email()` validation method.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/api.mdx#_snippet_14

LANGUAGE: ts
CODE:
```
/^(?!\.)(?!.*\.\.)([a-z0-9_'+\-\.]*)[a-z0-9_+-]@([a-z0-9][a-z0-9\-]*\.)+[a-z]{2,}$/i
```

----------------------------------------

TITLE: Defining Base Schema for Partial - Zod TypeScript
DESCRIPTION: Defines a sample Zod object schema that will be used as the base for demonstrating the `.partial()` method.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/packages/v3.mdx#_snippet_60

LANGUAGE: typescript
CODE:
```
const user = z.object({
  email: z.string(),
  username: z.string(),
});
// { email: string; username: string }
```

----------------------------------------

TITLE: Defining TypeScript Discriminated Union
DESCRIPTION: Demonstrates a standard TypeScript discriminated union type and a function to handle it, showing how TypeScript narrows the type based on the discriminator key.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/api.mdx#_snippet_77

LANGUAGE: ts
CODE:
```
type MyResult =
  | { status: "success"; data: string }
  | { status: "failed"; error: string };

function handleResult(result: MyResult){
  if(result.status === "success"){
    result.data; // string
  } else {
    result.error; // string
  }
}
```

----------------------------------------

TITLE: Importing Zod 3 and Zod 4 Simultaneously
DESCRIPTION: This example shows how to import both Zod 3 and Zod 4 using their respective versioned subpaths ('zod/v3' and 'zod/v4/core'). This allows your library to work with schemas from either version.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/library-authors.mdx#_snippet_4

LANGUAGE: ts
CODE:
```
import * as z3 from "zod/v3";
import * as z4 from "zod/v4/core";

type Schema = z3.ZodTypeAny | z4.$ZodType;

function acceptUserSchema(schema: z3.ZodTypeAny | z4.$ZodType) {
  // ...
}
```

----------------------------------------

TITLE: Defining Zod v4 Base Issue Interface - TypeScript
DESCRIPTION: Defines the `$ZodIssueBase` interface, which is the common structure for all Zod v4 issues. It includes properties like `code`, `input`, `path`, and `message`, ensuring compatibility with common error handling logic from Zod v3.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/v4/changelog.mdx#_snippet_12

LANGUAGE: typescript
CODE:
```
export interface $ZodIssueBase {
  readonly code?: string;
  readonly input?: unknown;
  readonly path: PropertyKey[];
  readonly message: string;
}
```

----------------------------------------

TITLE: Introspecting Zod Schemas with Future-Proofing (TypeScript)
DESCRIPTION: Shows a common pattern for introspecting Zod schemas using a switch statement on the schema type and advises on handling unknown types in the default case to future-proof the code against new schema additions.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/library-authors.mdx#_snippet_8

LANGUAGE: typescript
CODE:
```
const schema = {} as z.$ZodTypes;
const def = schema._zod.def;
switch (def.type) {
  case "string":
    // ...
    break;
  case "object":
    // ...
    break;
  default:
    console.warn(`Unknown schema type: ${def.type}`);
    // reasonable fallback behavior
}
```

----------------------------------------

TITLE: Understanding Zod Branded Type Structure in TypeScript
DESCRIPTION: Under the hood, Zod's .brand method works by attaching a symbol-keyed property to the inferred type using an intersection type. This symbol serves as the 'brand'.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/packages/v3.mdx#_snippet_140

LANGUAGE: TypeScript
CODE:
```
const Cat = z.object({ name: z.string() }).brand<"Cat">();
type Cat = z.infer<typeof Cat>;
// {name: string} & {[symbol]: "Cat"}
```

----------------------------------------

TITLE: Defining Zod v4 Issue Formats Type - TypeScript
DESCRIPTION: Defines the union type `IssueFormats` using the new Zod v4 issue types from `z.core`. This type represents the possible shapes of validation errors in Zod v4, including new types for map/set/record keys/elements.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/v4/changelog.mdx#_snippet_10

LANGUAGE: typescript
CODE:
```
import { z } from "zod/v4"; // v4

type IssueFormats = 
  | z.core.$ZodIssueInvalidType
  | z.core.$ZodIssueTooBig
  | z.core.$ZodIssueTooSmall
  | z.core.$ZodIssueInvalidStringFormat
  | z.core.$ZodIssueNotMultipleOf
  | z.core.$ZodIssueUnrecognizedKeys
  | z.core.$ZodIssueInvalidValue
  | z.core.$ZodIssueInvalidUnion
  | z.core.$ZodIssueInvalidKey // new: used for z.record/z.map 
  | z.core.$ZodIssueInvalidElement // new: used for z.map/z.set
  | z.core.$ZodIssueCustom;
```

----------------------------------------

TITLE: Importing Core Zod Components (TypeScript)
DESCRIPTION: Demonstrates how to import the core `z` object from `zod/v4/core` and shows examples of accessing base classes like `$ZodType`, `$ZodCheck`, `$ZodError`, common subclasses, and utility functions.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/packages/core.mdx#_snippet_0

LANGUAGE: ts
CODE:
```
import { z } from "zod/v4/core";

// the base class for all Zod schemas
z.$ZodType;

// subclasses of $ZodType that implement common parsers
z.$ZodString
z.$ZodObject
z.$ZodArray
// ...

// the base class for all Zod checks
z.$ZodCheck;

// subclasses of $ZodCheck that implement common checks
z.$ZodCheckMinLength
z.$ZodCheckMaxLength

// the base class for all Zod errors
z.$ZodError;

// issue formats (types only)
{} as z.$ZodIssue;

// utils
z.util.isValidJWT(...);
```

----------------------------------------

TITLE: Zod 3 .default() Behavior (Legacy)
DESCRIPTION: Shows the old behavior of `.default()` in Zod 3, where it parsed the default value and required it to match the *input type* of the schema.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/v4/changelog.mdx#_snippet_22

LANGUAGE: TypeScript
CODE:
```
// Zod 3
const schema = z.string()
  .transform(val => val.length)
  .default("tuna");
schema.parse(undefined); // => 4
```

----------------------------------------

TITLE: Configure Zod Peer Dependency (JSON)
DESCRIPTION: Example package.json configuration showing how to declare 'zod/v4/core' as a peer dependency with a flexible version range ('*') when building a tool that accepts Zod schemas.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/packages/core.mdx#_snippet_15

LANGUAGE: json
CODE:
```
{
  "peerDependencies": {
    "zod/v4/core": "*"
  }
}
```

----------------------------------------

TITLE: Result of Incorrect Schema Acceptance (TypeScript)
DESCRIPTION: Shows the type inferred by TypeScript when calling the incorrectly defined `inferSchema` function, demonstrating that it returns the base `z.core.$ZodType<string>` instead of the specific `ZodString` type.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/library-authors.mdx#_snippet_10

LANGUAGE: typescript
CODE:
```
inferSchema(z.string());
// => z.core.$ZodType<string>
```

----------------------------------------

TITLE: Accessing Core Utilities via z.core Namespace
DESCRIPTION: Illustrates accessing the contents of the `zod/v4/core` sub-package via the `z.core` namespace, which is re-exported from the main `zod/v4` package for convenience.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/v4/changelog.mdx#_snippet_42

LANGUAGE: ts
CODE:
```
import { z } from "zod/v4";

function handleError(iss: z.core.$ZodError) {
  // do stuff
}
```

----------------------------------------

TITLE: Using the .register() Method on a Zod Schema (TypeScript)
DESCRIPTION: Shows the convenient .register() method available on schemas to add them to a registry, noting that this method modifies the schema in place.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/v4/index.mdx#_snippet_19

LANGUAGE: ts
CODE:
```
emailSchema.register(myRegistry, { title: "Email address", description: "..." })
// => returns emailSchema
```

----------------------------------------

TITLE: Identifying Unrepresentable Zod Types in JSON Schema (TypeScript)
DESCRIPTION: Lists Zod types that do not have a direct equivalent in JSON Schema. By default, attempting to convert these types using `toJSONSchema` will result in an error.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/json-schema.mdx#_snippet_13

LANGUAGE: ts
CODE:
```
z.bigint(); // ❌
z.int64(); // ❌
z.symbol(); // ❌
z.void(); // ❌
z.date(); // ❌
z.map(); // ❌
z.set(); // ❌
z.file(); // ❌
z.transform(); // ❌
z.nan(); // ❌
z.custom(); // ❌
```

----------------------------------------

TITLE: Discriminating Zod String Format Checks (TypeScript)
DESCRIPTION: Demonstrates how to use a nested `switch` statement to discriminate between different string format checks by examining the `_zod.def.format` property after identifying a check as a "string_format".
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/packages/core.mdx#_snippet_10

LANGUAGE: typescript
CODE:
```
const check = {} as z.$ZodChecks;
const def = check._zod.def;

switch (def.check) {
  case "less_than":
  case "greater_than":
  // ...
  case "string_format":
    {
      const formatCheck = check as z.$ZodStringFormatChecks;
      const formatCheckDef = formatCheck._zod.def;

      switch (formatCheckDef.format) {
        case "email":
        case "url":
          // do stuff
      }
    }
    break;
}
```

----------------------------------------

TITLE: Creating a Generic Zod Registry (TypeScript)
DESCRIPTION: Explains how to create a Zod registry without specifying a metadata type, allowing it to function as a simple collection of schemas.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/metadata.mdx#_snippet_5

LANGUAGE: TypeScript
CODE:
```
const myRegistry = z.registry();

myRegistry.add(z.string());
myRegistry.add(z.number());
```

----------------------------------------

TITLE: Array Parsing Benchmark Results (Zod 3 vs Zod 4)
DESCRIPTION: Displays the output of the Zod array parsing benchmark, comparing the performance of Zod 3 and Zod 4 and showing Zod 4 is significantly faster.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/v4/index.mdx#_snippet_5

LANGUAGE: shell
CODE:
```
$ pnpm bench array
runtime: node v22.13.0 (arm64-darwin)

benchmark      time (avg)             (min … max)       p75       p99      p999
------------------------------------------------- -----------------------------
• z.array() parsing
------------------------------------------------- -----------------------------
zod3          147 µs/iter       (137 µs … 767 µs)    140 µs    246 µs    520 µs
zod4       19'817 ns/iter    (18'125 ns … 436 µs) 19'125 ns 44'500 ns    137 µs

summary for z.array() parsing
  zod4
   7.43x faster than zod3
```

----------------------------------------

TITLE: ToJSONSchemaParams Interface Definition
DESCRIPTION: Defines the TypeScript interface for the configuration object accepted by `z.toJSONSchema`, listing all available parameters and their types with brief descriptions.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/json-schema.mdx#_snippet_9

LANGUAGE: ts
CODE:
```
interface ToJSONSchemaParams {
  /** The JSON Schema version to target.
   * - `"draft-2020-12"` — Default. JSON Schema Draft 2020-12
   * - `"draft-7"` — Default. JSON Schema Draft 7 */
  target?: "draft-7" | "draft-2020-12";

  /** A registry used to look up metadata for each schema. 
   * Any schema with an `id` property will be extracted as a $def. */
  metadata?: $ZodRegistry<Record<string, any>>;

  /** How to handle unrepresentable types.
   * - `"throw"` — Default. Unrepresentable types throw an error
   * - `"any"` — Unrepresentable types become `{}` */
  unrepresentable?: "throw" | "any";

  /** How to handle cycles.
   * - `"ref"` — Default. Cycles will be broken using $defs
   * - `"throw"` — Cycles will throw an error if encountered */
  cycles?: "ref" | "throw";

  /* How to handle reused schemas.
   * - `"inline"` — Default. Reused schemas will be inlined
   * - `"ref"` — Reused schemas will be extracted as $defs */
  reused?: "ref" | "inline";

  /** A function used to convert `id` values to URIs to be used in *external* $refs.
   *
   * Default is `(id) => id`.
   */
  uri?: (id: string) => string;
}
```

----------------------------------------

TITLE: Constraining Zod Registry to Specific Schema Types (TypeScript)
DESCRIPTION: Illustrates how to restrict the types of Zod schemas that can be added to a registry by providing a second generic argument to `z.registry()`. This example constrains the registry to only accept `z.ZodString` schemas, preventing other types like `z.ZodNumber` from being added.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/metadata.mdx#_snippet_14

LANGUAGE: TypeScript
CODE:
```
import { z } from "zod/v4";

const myRegistry = z.registry<{ description: string }, z.ZodString>();

myRegistry.add(z.string(), { description: "A number" }); // ✅
myRegistry.add(z.number(), { description: "A number" }); // ❌ 
//             ^ 'ZodNumber' is not assignable to parameter of type 'ZodString'
```

----------------------------------------

TITLE: Creating a Zod Registry with Metadata Type (TypeScript)
DESCRIPTION: Demonstrates how to create a Zod registry that enforces a specific TypeScript type for the associated metadata.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/metadata.mdx#_snippet_0

LANGUAGE: TypeScript
CODE:
```
import { z } from "zod/v4";

const myRegistry = z.registry<{ description: string }>();
```

----------------------------------------

TITLE: Accepting Zod/Zod Mini Schemas using Zod Core (TypeScript)
DESCRIPTION: Demonstrates how library code should import from `zod/v4/core` to define functions that can accept schemas from both `zod/v4` and `zod/v4-mini`, ensuring compatibility by building against shared interfaces.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/library-authors.mdx#_snippet_6

LANGUAGE: typescript
CODE:
```
// library code
import * as z from "zod/v4/core";

export function acceptObjectSchema<T extends z.$ZodObject>(schema: T){
  // parse data
  z.parse(schema, { /* somedata */});
  // inspect internals
  schema._zod.def.shape;
}
```

----------------------------------------

TITLE: Install Zod via Package Managers
DESCRIPTION: Provides commands to install the latest stable version of the Zod library using npm, deno, yarn, bun, and pnpm.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/packages/v3.mdx#_snippet_1

LANGUAGE: sh
CODE:
```
npm install zod       # npm
deno add npm:zod      # deno
yarn add zod          # yarn
bun add zod          # bun
pnpm add zod          # pnpm
```

----------------------------------------

TITLE: Configure Zod Peer and Dev Dependencies (JSON)
DESCRIPTION: Example package.json configuration showing how to declare 'zod' as a peer dependency ('*') and a development dependency ('^3.25.0') for testing purposes.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/packages/core.mdx#_snippet_16

LANGUAGE: json
CODE:
```
{
  "peerDependencies": {
    "zod": "*"
  },
  "devDependencies": {
    "zod": "^3.25.0"
  }
}
```

----------------------------------------

TITLE: Defining and Parsing a Zod Promise Schema (Deprecated)
DESCRIPTION: This snippet shows how to create a Zod schema for a Promise that is expected to resolve to a number using the deprecated `z.promise()` method. It illustrates the two-step validation process and how parsing works with both non-Promise and Promise inputs.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/api.mdx#_snippet_91

LANGUAGE: TypeScript
CODE:
```
const numberPromise = z.promise(z.number());
```

LANGUAGE: TypeScript
CODE:
```
numberPromise.parse("tuna");
// ZodError: Non-Promise type: string

numberPromise.parse(Promise.resolve("tuna"));
// => Promise<number>

const test = async () => {
  await numberPromise.parse(Promise.resolve("tuna"));
  // ZodError: Non-number type: string

  await numberPromise.parse(Promise.resolve(3.14));
  // => 3.14
};
```

----------------------------------------

TITLE: Importing Zod 4 Core from Subpath
DESCRIPTION: Library code should import Zod 4 specifically from the '/v4/core' subpath instead of the package root. This practice helps future-proof your code against major version changes.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/library-authors.mdx#_snippet_3

LANGUAGE: ts
CODE:
```
import * as z4 from "zod/v4/core";
```

----------------------------------------

TITLE: Using a Library Function with Zod v4 and Zod v4 Mini (TypeScript)
DESCRIPTION: Illustrates how a user of a library built with `zod/v4/core` can pass schemas from either `zod/v4` or `zod/v4-mini` to the library's functions.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/library-authors.mdx#_snippet_7

LANGUAGE: typescript
CODE:
```
// user code
import { acceptObjectSchema } from "your-library";

// Zod 4
import * as z from "zod/v4";
acceptObjectSchema(z.object({ name: z.string() }));

// Zod 4 Mini
import * as zm from "zod/v4-mini";
acceptObjectSchema(zm.object({ name: zm.string() }))
```

----------------------------------------

TITLE: TypeScript Structural Typing Example
DESCRIPTION: Provides a basic example of TypeScript's structural typing where two types with the same structure are considered compatible, setting the context for why branded types are needed to simulate nominal typing.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/api.mdx#_snippet_117

LANGUAGE: TypeScript
CODE:
```
type Cat = { name: string };
type Dog = { name: string };

const pluto: Dog = { name: "pluto" };
const simba: Cat = fido; // works fine
```

----------------------------------------

TITLE: Converting Zod Error to Nested Object with Deprecated `z.formatError` (TypeScript)
DESCRIPTION: (Deprecated) Uses the deprecated `z.formatError` function to convert a Zod error into a nested object structure. Similar to `treeifyError`, it mirrors the schema structure but uses `_errors` fields to store error messages at each level. This function is superseded by `z.treeifyError`.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/error-formatting.mdx#_snippet_5

LANGUAGE: ts
CODE:
```
const formatted = z.formatError(result.error);
```

----------------------------------------

TITLE: Listing Top-Level Refinements in Zod Mini (TypeScript)
DESCRIPTION: Provides a comprehensive list of the top-level functional refinement functions available in Zod Mini, corresponding to common Zod methods.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/v4/index.mdx#_snippet_15

LANGUAGE: ts
CODE:
```
import { z } from "zod/v4-mini";

// custom checks
z.refine();

// first-class checks
z.lt(value);
z.lte(value); // alias: z.maximum()
z.gt(value);
z.gte(value); // alias: z.minimum()
z.positive();
z.negative();
z.nonpositive();
z.nonnegative();
z.multipleOf(value);
z.maxSize(value);
z.minSize(value);
z.size(value);
z.maxLength(value);
z.minLength(value);
z.length(value);
z.regex(regex);
z.lowercase();
z.uppercase();
z.includes(value);
z.startsWith(value);
z.endsWith(value);
z.property(key, schema); // for object schemas; check `input[key]` against `schema`
z.mime(value); // for file schemas (see below)

// overwrites (these *do not* change the inferred type!)
z.overwrite(value => newValue);
z.normalize();
z.trim();
z.toLowerCase();
z.toUpperCase();
```

----------------------------------------

TITLE: Differentiating Zod 3 and Zod 4 Schemas at Runtime
DESCRIPTION: This snippet demonstrates how to check if a schema object is from Zod 4 by looking for the presence of the '_zod' property. Zod 3 schemas use the '_def' property instead.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/library-authors.mdx#_snippet_5

LANGUAGE: ts
CODE:
```
import type * as z3 from "zod/v3";
import type * as v4 from "zod/v4/core";

declare const schema: z3.ZodTypeAny | v4.$ZodType;

if ("_zod" in schema) {
  schema._zod.def; // Zod 4 schema
} else {
  schema._def; // Zod 3 schema
}
```

----------------------------------------

TITLE: Defining the Base Zod Error Class (TypeScript)
DESCRIPTION: Shows the basic structure of the `$ZodError` base class, which is used for Zod errors. It implements the `Error` interface but does not extend the native `Error` class for performance reasons.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/packages/core.mdx#_snippet_12

LANGUAGE: typescript
CODE:
```
export class $ZodError<T = unknown> implements Error {
 public issues: $ZodIssue[];
}
```

----------------------------------------

TITLE: Accessing Errors in Deprecated Zod Formatted Error Structure (TypeScript)
DESCRIPTION: (Deprecated) Shows how to access specific error messages within the nested structure produced by the deprecated `z.formatError`. Optional chaining (`?.`) is used to safely navigate the object tree and retrieve error arrays stored in `_errors` fields at specific paths.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/error-formatting.mdx#_snippet_6

LANGUAGE: ts
CODE:
```
formatted?.username?._errors;
// => ["Invalid input: expected string, received number"]

formatted?.favoriteNumbers?.[1]?._errors;
// => ["Invalid input: expected number, received string"]
```

----------------------------------------

TITLE: refine() Dropping Function as Second Argument - Zod 3 (Removed)
DESCRIPTION: Notes the removal of the Zod v3 overload for .refine() that accepted a separate function for generating the error message.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/v4/changelog.mdx#_snippet_34

LANGUAGE: TypeScript
CODE:
```
const longString = z.string().refine(
  (val) => val.length > 10,
  (val) => ({ message: `${val} is not more than 10 characters` })
);
```

----------------------------------------

TITLE: Updating Peer Dependency for Zod 4 Support
DESCRIPTION: To indicate support for Zod 4, update the minimum version in your peer dependency declaration to ^3.25.0, as Zod 4 is available from this version onwards.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/library-authors.mdx#_snippet_2

LANGUAGE: json
CODE:
```
{
  // ...
  "peerDependencies": {
    "zod": "^3.25.0"
  }
}
```

----------------------------------------

TITLE: Run Zod Build
DESCRIPTION: Executes the build script defined in `package.json`. This command typically deletes the `lib` directory and recompiles source files from `src` into `lib`.
SOURCE: https://github.com/colinhacks/zod/blob/main/CONTRIBUTING.md#_snippet_5

LANGUAGE: bash
CODE:
```
pnpm build
```

----------------------------------------

TITLE: Upgrading Zod using pnpm
DESCRIPTION: This command uses the pnpm package manager to upgrade the 'zod' package to version 3.25.0 or higher within the 3.x range. It's a standard way to update dependencies in a project using pnpm.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/v4/index.mdx#_snippet_49

LANGUAGE: sh
CODE:
```
pnpm upgrade zod@^3.25.0
```

----------------------------------------

TITLE: Customizing Min Length Error (Zod 3)
DESCRIPTION: Using the deprecated `message` parameter to customize the error message for a minimum length validation in Zod 3.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/v4/changelog.mdx#_snippet_3

LANGUAGE: typescript
CODE:
```
z.string().min(5, { message: "Too short." });
```

----------------------------------------

TITLE: Build Benchmarking Code (Shell)
DESCRIPTION: Executes the 'build-bench' script defined in the package.json, which typically involves compiling the code using tsc with extended diagnostics.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/tsc/README.md#_snippet_1

LANGUAGE: Shell
CODE:
```
npm run build-bench
```

----------------------------------------

TITLE: Execute Playground Script
DESCRIPTION: Runs the `playground.ts` script and watches for changes. This command is useful for simple experimentation and testing code snippets interactively.
SOURCE: https://github.com/colinhacks/zod/blob/main/CONTRIBUTING.md#_snippet_10

LANGUAGE: bash
CODE:
```
pnpm play
```

----------------------------------------

TITLE: Flattening Zod Validation Errors with z.flattenError() (TypeScript)
DESCRIPTION: This snippet demonstrates how to use the deprecated `z.flattenError()` function to process a Zod validation error (`result.error`). It shows the function call and the expected structure of the resulting flattened error object, which separates top-level errors (`formErrors`) from field-specific errors (`fieldErrors`). This function is primarily useful for schemas that are only one level deep.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/error-formatting.mdx#_snippet_7

LANGUAGE: typescript
CODE:
```
const flattened = z.flattenError(result.error);
// { errors: string[], properties: { [key: string]: string[] } }

{
  formErrors: [ 'Unrecognized key: "extraKey"' ],
  fieldErrors: {
    username: [ 'Invalid input: expected string, received number' ],
    favoriteNumbers: [ 'Invalid input: expected number, received string' ]
  }
}
```

----------------------------------------

TITLE: Upgrading to Zod 4
DESCRIPTION: Command to upgrade the Zod package to version 3.25.0 or later, which includes Zod 4.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/v4/changelog.mdx#_snippet_0

LANGUAGE: bash
CODE:
```
npm upgrade zod@^3.25.0
```

----------------------------------------

TITLE: Defining Base Schema for Pick/Omit - Zod TypeScript
DESCRIPTION: Defines a sample Zod object schema that will be used as the base for demonstrating the `.pick()` and `.omit()` methods.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/packages/v3.mdx#_snippet_57

LANGUAGE: typescript
CODE:
```
const Recipe = z.object({
  id: z.string(),
  name: z.string(),
  ingredients: z.array(z.string()),
});
```

----------------------------------------

TITLE: Adding Zod to devDependencies for Development
DESCRIPTION: To satisfy the peer dependency requirement during your library's development, you should also add Zod to your devDependencies with the same version range.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/library-authors.mdx#_snippet_1

LANGUAGE: json
CODE:
```
{
  "peerDependencies": {
    "zod": "^3.25.0"
  },
  "devDependencies": {
    "zod": "^3.25.0"
  }
}
```

----------------------------------------

TITLE: Installing Zod Canary Version via Package Managers
DESCRIPTION: This snippet provides commands for installing the canary (latest development) version of the Zod library using popular JavaScript/TypeScript package managers like npm, Deno, Yarn, Bun, and pnpm.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/index.mdx#_snippet_2

LANGUAGE: sh
CODE:
```
npm install zod@canary       # npm
deno add npm:zod@canary      # deno
yarn add zod@canary          # yarn
bun add zod@canary           # bun
pnpm add zod@canary          # pnpm
```

----------------------------------------

TITLE: Define Zod Issue Union Type (TypeScript)
DESCRIPTION: Defines a union type representing all possible specific Zod validation issue subtypes that can occur during schema parsing or validation.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/packages/core.mdx#_snippet_14

LANGUAGE: typescript
CODE:
```
export type $ZodIssue =
  | $ZodIssueInvalidType
  | $ZodIssueTooBig
  | $ZodIssueTooSmall
  | $ZodIssueInvalidStringFormat
  | $ZodIssueNotMultipleOf
  | $ZodIssueUnrecognizedKeys
  | $ZodIssueInvalidUnion
  | $ZodIssueInvalidKey
  | $ZodIssueInvalidElement
  | $ZodIssueInvalidValue
  | $ZodIssueCustom;
```

----------------------------------------

TITLE: Applying Multiple Zod Checks (TypeScript)
DESCRIPTION: Demonstrates how to chain multiple `.check()` methods onto a Zod schema and how to access the list of applied checks via the internal `_zod.def.checks` property.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/packages/core.mdx#_snippet_5

LANGUAGE: typescript
CODE:
```
const schema = z.string().check(z.email()).check(z.min(5));
// => $ZodString

schema._zod.def.checks;
// => [$ZodCheckEmail, $ZodCheckMinLength]
```

----------------------------------------

TITLE: Run Specific Vitest Test File
DESCRIPTION: Runs only the test files that match the provided `<file>` argument. Useful for focusing on specific tests during development.
SOURCE: https://github.com/colinhacks/zod/blob/main/CONTRIBUTING.md#_snippet_8

LANGUAGE: bash
CODE:
```
pnpm test <file>
```

----------------------------------------

TITLE: Define Zod Issue Base Interface (TypeScript)
DESCRIPTION: Defines the base interface for all Zod validation issues, including properties for an optional code, the input value, the path to the invalid data, and a descriptive message.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/packages/core.mdx#_snippet_13

LANGUAGE: typescript
CODE:
```
export interface $ZodIssueBase {
  readonly code?: string;
  readonly input?: unknown;
  readonly path: PropertyKey[];
  readonly message: string;
}
```

----------------------------------------

TITLE: Run Vitest Tests in Watch Mode
DESCRIPTION: Executes all test files using Vitest and watches for file changes, rerunning tests automatically.
SOURCE: https://github.com/colinhacks/zod/blob/main/CONTRIBUTING.md#_snippet_7

LANGUAGE: bash
CODE:
```
pnpm test:watch
```

----------------------------------------

TITLE: Run All Vitest Tests
DESCRIPTION: Executes all test files using Vitest and generates a coverage badge. This should be run before submitting a pull request.
SOURCE: https://github.com/colinhacks/zod/blob/main/CONTRIBUTING.md#_snippet_6

LANGUAGE: bash
CODE:
```
pnpm test
```

----------------------------------------

TITLE: Create New Feature Branch (Commented)
DESCRIPTION: Creates and switches to a new Git branch named `your-feature-name` based off the `master` branch. This command is part of a commented-out section describing pull request submission steps.
SOURCE: https://github.com/colinhacks/zod/blob/main/CONTRIBUTING.md#_snippet_2

LANGUAGE: bash
CODE:
```
git checkout -b your-feature-name
```

----------------------------------------

TITLE: Listing Zod String Format Check Subclasses (TypeScript)
DESCRIPTION: Defines the `$ZodStringFormatChecks` union type, listing the specific subclasses of `$ZodCheckStringFormat` used for validating various string formats like email, URL, UUID, etc.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/packages/core.mdx#_snippet_9

LANGUAGE: typescript
CODE:
```
export type $ZodStringFormatChecks =
  | $ZodCheckRegex
  | $ZodCheckLowerCase
  | $ZodUpperCase
  | $ZodIncludes
  | $ZodStartsWith
  | $ZodEndsWith
  | $ZodGUID
  | $ZodUUID
  | $ZodEmail
  | $ZodURL
  | $ZodEmoji
  | $ZodNanoID
  | $ZodCUID
  | $ZodCUID2
  | $ZodULID
  | $ZodXID
  | $ZodKSUID
  | $ZodISODateTime
  | $ZodISODate
  | $ZodISOTime
  | $ZodISODuration
  | $ZodIPv4
  | $ZodIPv6
  | $ZodCIDRv4
  | $ZodCIDRv6
  | $ZodBase64
  | $ZodBase64URL
  | $ZodE164
  | $ZodJWT;
```

----------------------------------------

TITLE: Generate Random Zod Schemas (Shell)
DESCRIPTION: Runs a Node.js script to generate random Zod schemas for performance testing. Pregenerated schemas are also available in src/index.ts.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/tsc/README.md#_snippet_0

LANGUAGE: Shell
CODE:
```
node generateRandomSchemas.js
```

----------------------------------------

TITLE: Listing Common Zod Check Subclasses (TypeScript)
DESCRIPTION: Defines the `$ZodChecks` union type, listing the various first-party subclasses of `$ZodCheck` available for common validation tasks like size, format, and value comparisons.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/packages/core.mdx#_snippet_7

LANGUAGE: typescript
CODE:
```
export type $ZodChecks =
  | $ZodCheckLessThan
  | $ZodCheckGreaterThan
  | $ZodCheckMultipleOf
  | $ZodCheckNumberFormat
  | $ZodCheckBigIntFormat
  | $ZodMaxSize
  | $ZodMinSize
  | $ZodSizeEquals
  | $ZodMaxLength
  | $ZodMinLength
  | $ZodLengthEquals
  | $ZodProperty
  | $ZodMimeType
  | $ZodOverwrite
  | $ZodCheckStringFormat
```

----------------------------------------

TITLE: Navigate to Zod Directory
DESCRIPTION: Changes the current directory to the cloned Zod repository folder.
SOURCE: https://github.com/colinhacks/zod/blob/main/CONTRIBUTING.md#_snippet_1

LANGUAGE: bash
CODE:
```
cd zod
```

----------------------------------------

TITLE: Link Local Zod Copy (Shell)
DESCRIPTION: Links a local development copy of the Zod library into the current project's node_modules, allowing modifications to be tested directly. Remember to rebuild Zod in between modifications.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/tsc/README.md#_snippet_2

LANGUAGE: Shell
CODE:
```
npm link
```

----------------------------------------

TITLE: Push Feature Branch (Commented)
DESCRIPTION: Pushes the local `your-feature-name` branch to the `origin` remote repository. This command is part of a commented-out section describing pull request submission steps.
SOURCE: https://github.com/colinhacks/zod/blob/main/CONTRIBUTING.md#_snippet_3

LANGUAGE: bash
CODE:
```
git push origin your-feature-name
```

----------------------------------------

TITLE: Running Zod TSC Benchmark in Shell
DESCRIPTION: Provides a shell command to navigate to the Zod compiler benchmarks directory and run a specific benchmark (`object-with-extend`) to verify the performance improvements discussed.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/v4/index.mdx#_snippet_8

LANGUAGE: sh
CODE:
```
$ cd packages/tsc
$ pnpm bench object-with-extend
```

----------------------------------------

TITLE: Cloning Zod Repository
DESCRIPTION: Clones the forked Zod repository from GitHub using SSH. Replace `{your_username}` with your actual GitHub username.
SOURCE: https://github.com/colinhacks/zod/blob/main/CONTRIBUTING.md#_snippet_0

LANGUAGE: bash
CODE:
```
git clone git@github.com:{your_username}/zod.git
```

----------------------------------------

TITLE: String Parsing Benchmark Results (Zod 3 vs Zod 4)
DESCRIPTION: Displays the output of the Zod string parsing benchmark, comparing the performance of Zod 3 and Zod 4 and showing Zod 4 is significantly faster.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/v4/index.mdx#_snippet_4

LANGUAGE: shell
CODE:
```
$ pnpm bench string
runtime: node v22.13.0 (arm64-darwin)

benchmark      time (avg)             (min … max)       p75       p99      p999
------------------------------------------------- -----------------------------
• z.string().parse
------------------------------------------------- -----------------------------
zod3          363 µs/iter       (338 µs … 683 µs)    351 µs    467 µs    572 µs
zod4       24'674 ns/iter    (21'083 ns … 235 µs) 24'209 ns 76'125 ns    120 µs

summary for z.string().parse
  zod4
   14.71x faster than zod3
```

----------------------------------------

TITLE: Run Filtered Vitest Tests in Workspace
DESCRIPTION: Runs test files within a specific workspace (`<ws>`) that match the provided `<file>` argument. For example, `"enum"` will match `"enum.test.ts"`.
SOURCE: https://github.com/colinhacks/zod/blob/main/CONTRIBUTING.md#_snippet_9

LANGUAGE: bash
CODE:
```
pnpm test --filter <ws> <file>
```

----------------------------------------

TITLE: Install Dependencies with pnpm
DESCRIPTION: Installs all project dependencies using the pnpm package manager. This is a required step after cloning the repository.
SOURCE: https://github.com/colinhacks/zod/blob/main/CONTRIBUTING.md#_snippet_4

LANGUAGE: bash
CODE:
```
pnpm i
```

----------------------------------------

TITLE: Running a Specific Zod Benchmark
DESCRIPTION: Shows the command to execute a specific benchmark script within the Zod repository using pnpm, where <name> is the identifier for the desired benchmark.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/v4/index.mdx#_snippet_3

LANGUAGE: shell
CODE:
```
pnpm bench <name>
```

----------------------------------------

TITLE: Setting up Zod v4 for Benchmarks
DESCRIPTION: Provides shell commands to clone the Zod repository, navigate into the directory, switch to the v4 branch, and install dependencies using pnpm, preparing the environment to run benchmarks.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/v4/index.mdx#_snippet_2

LANGUAGE: shell
CODE:
```
git clone git@github.com:colinhacks/zod.git
cd zod
git switch v4
pnpm install
```

----------------------------------------

TITLE: Defining Optional Properties with io-ts (TypeScript)
DESCRIPTION: Provides an example of defining a schema with required and optional properties using the io-ts library, demonstrating the pattern of combining `t.type` for required fields, `t.partial` for optional fields, and `t.intersection` to merge them.
SOURCE: https://github.com/colinhacks/zod/blob/main/packages/docs/content/packages/v3.mdx#_snippet_153

LANGUAGE: ts
CODE:
```
import * as t from "io-ts";

const A = t.type({
  foo: t.string,
});

const B = t.partial({
  bar: t.number,
});

const C = t.intersection([A, B]);

type C = t.TypeOf<typeof C>;
// returns { foo: string; bar?: number | undefined }
```