TITLE: Deleting Vercel Integration Log Drain using Vercel SDK (TypeScript)
DESCRIPTION: Demonstrates how to delete an integration log drain using the `Vercel` class instance from the `@vercel/sdk`. Requires providing the log drain `id` and optionally `teamId` or `slug`. The log drain can only be deleted if the integration owns it when using an OAuth2 Token.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/logdrains/README.md#_snippet_6

LANGUAGE: typescript
CODE:
```
import { Vercel } from "@vercel/sdk";

const vercel = new Vercel({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  await vercel.logDrains.deleteIntegrationLogDrain({
    id: "<id>",
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
    slug: "my-team-url-slug"
  });


}

run();
```

----------------------------------------

TITLE: Retrieving Vercel Domains using Standalone Function (TypeScript)
DESCRIPTION: This snippet shows how to retrieve domains using the standalone `domainsGetDomains` function from `@vercel/sdk/funcs`. It utilizes a `VercelCore` instance for potential tree-shaking benefits and demonstrates calling the function with parameters and handling the result, including checking for errors.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/domains/README.md#_snippet_13

LANGUAGE: typescript
CODE:
```
import { VercelCore } from "@vercel/sdk/core.js";
import { domainsGetDomains } from "@vercel/sdk/funcs/domainsGetDomains.js";

// Use `VercelCore` for best tree-shaking performance.
// You can create one instance of it to use across an application.
const vercel = new VercelCore({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const res = await domainsGetDomains(vercel, {
    limit: 20,
    since: 1609499532000,
    until: 1612264332000,
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
    slug: "my-team-url-slug",
  });

  if (!res.ok) {
    throw res.error;
  }

  const { value: result } = res;

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Updating Vercel Project Domain using SDK Instance (TypeScript)
DESCRIPTION: This snippet demonstrates how to update a Vercel project domain's configuration using an instance of the `Vercel` class from the `@vercel/sdk`. It shows how to initialize the SDK with a bearer token and then call the `projects.updateProjectDomain` method with the required parameters like project ID/name, domain, team details, and the request body specifying updates like `gitBranch` and `redirect`.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/projects/README.md#_snippet_14

LANGUAGE: typescript
CODE:
```
import { Vercel } from "@vercel/sdk";

const vercel = new Vercel({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const result = await vercel.projects.updateProjectDomain({
    idOrName: "<value>",
    domain: "www.example.com",
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
    slug: "my-team-url-slug",
    requestBody: {
      gitBranch: null,
      redirect: "foobar.com",
      redirectStatusCode: 307,
    },
  });

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Update Edge Config using Vercel Class (TypeScript)
DESCRIPTION: This snippet demonstrates how to update a Vercel Edge Config using an instance of the main `Vercel` class. It requires initializing the class with a bearer token and then calling the `edgeConfig.updateEdgeConfig` method with the necessary parameters like `edgeConfigId`, `teamId`, `slug`, and the `requestBody` containing the update.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/edgeconfig/README.md#_snippet_6

LANGUAGE: typescript
CODE:
```
import { Vercel } from "@vercel/sdk";

const vercel = new Vercel({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const result = await vercel.edgeConfig.updateEdgeConfig({
    edgeConfigId: "<id>",
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
    slug: "my-team-url-slug",
    requestBody: {
      slug: "<value>"
    }
  });

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Initializing CreateProjectDefaultResourceConfig in TypeScript
DESCRIPTION: This snippet demonstrates how to import the CreateProjectDefaultResourceConfig type and create an instance of it in TypeScript, showing the initialization of the required `functionDefaultRegions` field. It serves as a basic example for configuring default resource settings for a Vercel project.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/models/createprojectdefaultresourceconfig.md#_snippet_0

LANGUAGE: typescript
CODE:
```
import { CreateProjectDefaultResourceConfig } from "@vercel/sdk/models/createprojectop.js";

let value: CreateProjectDefaultResourceConfig = {
  functionDefaultRegions: [
    "<value>",
  ],
};
```

----------------------------------------

TITLE: Create Vercel Project with Vercel Class (TypeScript)
DESCRIPTION: Demonstrates how to create a new Vercel project using an instance of the main `Vercel` class. It shows how to initialize the SDK with a bearer token and pass configuration options like team ID, slug, and the project name in the request body.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/projects/README.md#_snippet_4

LANGUAGE: typescript
CODE:
```
import { Vercel } from "@vercel/sdk";

const vercel = new Vercel({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const result = await vercel.projects.createProject({
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
    slug: "my-team-url-slug",
    requestBody: {
      name: "a-project-name",
    },
  });

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Recommended TypeScript Compiler Options
DESCRIPTION: Provides the recommended settings for the `compilerOptions` in a `tsconfig.json` file when using this SDK with TypeScript. These settings ensure proper static type support for features like async iterables, streams, and fetch-related APIs by targeting ES2020 or higher and including necessary library definitions.
SOURCE: https://github.com/vercel/sdk/blob/main/RUNTIMES.md#_snippet_0

LANGUAGE: json
CODE:
```
{
  "compilerOptions": {
    "target": "es2020", // or higher
    "lib": ["es2020", "dom", "dom.iterable"]
  }
}
```

----------------------------------------

TITLE: Create Vercel Project with Standalone Function (TypeScript)
DESCRIPTION: Illustrates creating a Vercel project using the standalone `projectsCreateProject` function with a `VercelCore` instance for optimized tree-shaking. It includes passing configuration options and basic error handling.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/projects/README.md#_snippet_5

LANGUAGE: typescript
CODE:
```
import { VercelCore } from "@vercel/sdk/core.js";
import { projectsCreateProject } from "@vercel/sdk/funcs/projectsCreateProject.js";

// Use `VercelCore` for best tree-shaking performance.
// You can create one instance of it to use across an application.
const vercel = new VercelCore({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const res = await projectsCreateProject(vercel, {
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
    slug: "my-team-url-slug",
    requestBody: {
      name: "a-project-name",
    },
  });

  if (!res.ok) {
    throw res.error;
  }

  const { value: result } = res;

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Getting Edge Config Item using Standalone Function (TypeScript)
DESCRIPTION: This example demonstrates using the standalone `edgeConfigGetEdgeConfigItem` function from the Vercel SDK. It utilizes a `VercelCore` instance for potential tree-shaking benefits and shows how to handle the result, including error checking.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/edgeconfig/README.md#_snippet_19

LANGUAGE: typescript
CODE:
```
import { VercelCore } from "@vercel/sdk/core.js";
import { edgeConfigGetEdgeConfigItem } from "@vercel/sdk/funcs/edgeConfigGetEdgeConfigItem.js";

// Use `VercelCore` for best tree-shaking performance.
// You can create one instance of it to use across an application.
const vercel = new VercelCore({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const res = await edgeConfigGetEdgeConfigItem(vercel, {
    edgeConfigId: "<id>",
    edgeConfigItemKey: "<value>",
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
    slug: "my-team-url-slug",
  });

  if (!res.ok) {
    throw res.error;
  }

  const { value: result } = res;

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Example Usage of UpdateProjectDomainRequestBody in TypeScript
DESCRIPTION: This snippet demonstrates how to create an instance of the UpdateProjectDomainRequestBody type in TypeScript, assigning example values to its fields such as gitBranch, redirect, and redirectStatusCode.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/models/updateprojectdomainrequestbody.md#_snippet_0

LANGUAGE: TypeScript
CODE:
```
import { UpdateProjectDomainRequestBody } from "@vercel/sdk/models/updateprojectdomainop.js";

let value: UpdateProjectDomainRequestBody = {
  gitBranch: null,
  redirect: "foobar.com",
  redirectStatusCode: 307,
};
```

----------------------------------------

TITLE: Promoting Deployment using Standalone Function (TypeScript)
DESCRIPTION: Illustrates using the standalone `projectsRequestPromote` function with a `VercelCore` instance, which is recommended for better tree-shaking. The function takes the `VercelCore` instance and an options object containing `projectId`, `deploymentId`, and optional `teamId` or `slug`. The example shows how to check the result for success or failure.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/projects/README.md#_snippet_41

LANGUAGE: typescript
CODE:
```
import { VercelCore } from "@vercel/sdk/core.js";
import { projectsRequestPromote } from "@vercel/sdk/funcs/projectsRequestPromote.js";

// Use `VercelCore` for best tree-shaking performance.
// You can create one instance of it to use across an application.
const vercel = new VercelCore({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const res = await projectsRequestPromote(vercel, {
    projectId: "<id>",
    deploymentId: "<id>",
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
    slug: "my-team-url-slug",
  });

  if (!res.ok) {
    throw res.error;
  }

  const { value: result } = res;

  
}

run();
```

----------------------------------------

TITLE: Possible Values for CreateRecordRequestBodyDnsType in TypeScript
DESCRIPTION: Lists the valid string literal values that the CreateRecordRequestBodyDnsType can represent. These are standard DNS record types.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/models/createrecordrequestbodydnstype.md#_snippet_1

LANGUAGE: typescript
CODE:
```
"A" | "AAAA" | "ALIAS" | "CAA" | "CNAME" | "HTTPS" | "MX" | "SRV" | "TXT" | "NS"
```

----------------------------------------

TITLE: Creating Auth Token using Standalone Function (TypeScript)
DESCRIPTION: This example illustrates creating an authentication token using the standalone `authenticationCreateAuthToken` function from the SDK's core module. It utilizes `VercelCore` for better tree-shaking and demonstrates how to pass the core instance and parameters, including basic error handling.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/authentication/README.md#_snippet_5

LANGUAGE: typescript
CODE:
```
import { VercelCore } from "@vercel/sdk/core.js";
import { authenticationCreateAuthToken } from "@vercel/sdk/funcs/authenticationCreateAuthToken.js";

// Use `VercelCore` for best tree-shaking performance.
// You can create one instance of it to use across an application.
const vercel = new VercelCore({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const res = await authenticationCreateAuthToken(vercel, {
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
    slug: "my-team-url-slug",
    requestBody: {
      name: "<value>",
    },
  });

  if (!res.ok) {
    throw res.error;
  }

  const { value: result } = res;

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Deleting Vercel Alias using Standalone Function (TypeScript)
DESCRIPTION: This snippet shows how to delete a Vercel alias using the standalone aliasesDeleteAlias function. It utilizes a VercelCore instance for potential tree-shaking benefits and includes basic error handling for the API response.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/aliases/README.md#_snippet_9

LANGUAGE: typescript
CODE:
```
import { VercelCore } from "@vercel/sdk/core.js";
import { aliasesDeleteAlias } from "@vercel/sdk/funcs/aliasesDeleteAlias.js";

// Use `VercelCore` for best tree-shaking performance.
// You can create one instance of it to use across an application.
const vercel = new VercelCore({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const res = await aliasesDeleteAlias(vercel, {
    aliasId: "<id>",
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
    slug: "my-team-url-slug",
  });

  if (!res.ok) {
    throw res.error;
  }

  const { value: result } = res;

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Creating Project Environment Variable using Vercel SDK Instance
DESCRIPTION: This snippet demonstrates how to create or update a project environment variable using the `createProjectEnv` method on an instance of the `Vercel` class. It requires importing the `Vercel` class and providing a bearer token for authentication. The method takes an object specifying the project ID or name, optional upsert flag, team details, and the environment variable details in the `requestBody`.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/projects/README.md#_snippet_26

LANGUAGE: typescript
CODE:
```
import { Vercel } from "@vercel/sdk";

const vercel = new Vercel({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const result = await vercel.projects.createProjectEnv({
    idOrName: "prj_XLKmu1DyR1eY7zq8UgeRKbA7yVLA",
    upsert: "true",
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
    slug: "my-team-url-slug",
    requestBody: {
      key: "API_URL",
      value: "https://api.vercel.com",
      type: "plain",
      target: [
        "preview"
      ],
      gitBranch: "feature-1",
      comment: "database connection string for production",
      customEnvironmentIds: [
        "env_1234567890"
      ]
    }
  });

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Creating Edge Config using Vercel SDK Class Instance (TypeScript)
DESCRIPTION: This snippet demonstrates how to create a Vercel Edge Config using the main Vercel class instance. It requires initializing the class with a bearer token and then calling the edgeConfig.createEdgeConfig method with the team ID, slug, and request body.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/edgeconfig/README.md#_snippet_2

LANGUAGE: typescript
CODE:
```
import { Vercel } from "@vercel/sdk";

const vercel = new Vercel({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const result = await vercel.edgeConfig.createEdgeConfig({
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
    slug: "my-team-url-slug",
    requestBody: {
      slug: "<value>",
    },
  });

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Listing User Events using Vercel SDK TypeScript
DESCRIPTION: This snippet demonstrates how to initialize the Vercel SDK with a bearer token and call the `user.listUserEvents` method to retrieve user events. It shows how to pass various parameters like limit, time ranges, types, and IDs to filter the results. The response is logged to the console.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/user/README.md#_snippet_0

LANGUAGE: typescript
CODE:
```
import { Vercel } from "@vercel/sdk";

const vercel = new Vercel({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const result = await vercel.user.listUserEvents({
    limit: 20,
    since: "2019-12-08T10:00:38.976Z",
    until: "2019-12-09T23:00:38.976Z",
    types: "login,team-member-join,domain-buy",
    userId: "aeIInYVk59zbFF2SxfyxxmuO",
    principalId: "aeIInYVk59zbFF2SxfyxxmuO",
    projectIds: "aeIInYVk59zbFF2SxfyxxmuO",
    withPayload: "true",
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
    slug: "my-team-url-slug",
  });

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Creating SubmitInvoicePeriod Instance in TypeScript
DESCRIPTION: This snippet demonstrates how to create an instance of the SubmitInvoicePeriod type in TypeScript. It shows the required 'start' and 'end' fields, which must be Date objects, representing the beginning and end of the billing period.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/models/submitinvoiceperiod.md#_snippet_0

LANGUAGE: typescript
CODE:
```
import { SubmitInvoicePeriod } from "@vercel/sdk/models/submitinvoiceop.js";

let value: SubmitInvoicePeriod = {
  start: new Date("2023-04-03T11:32:44.508Z"),
  end: new Date("2025-04-19T03:10:20.338Z")
};
```

----------------------------------------

TITLE: Creating Event using Vercel Instance - TypeScript
DESCRIPTION: This snippet demonstrates how to dispatch an event to Vercel using the `createEvent` method available on an initialized `Vercel` instance. It requires importing the `Vercel` class from `@vercel/sdk` and providing a bearer token for authentication. The example dispatches an `installation.updated` event.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/marketplace/README.md#_snippet_4

LANGUAGE: typescript
CODE:
```
import { Vercel } from "@vercel/sdk";

const vercel = new Vercel({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  await vercel.marketplace.createEvent({
    integrationConfigurationId: "<id>",
    requestBody: {
      event: {
        type: "installation.updated",
      },
    },
  });


}

run();
```

----------------------------------------

TITLE: Allowed Values for CreateProjectNodeVersion Type
DESCRIPTION: Lists the valid string literal values that can be assigned to the `CreateProjectNodeVersion` type. These represent the supported Node.js versions for Vercel projects.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/models/createprojectnodeversion.md#_snippet_1

LANGUAGE: typescript
CODE:
```
"22.x" | "20.x" | "18.x" | "16.x" | "14.x" | "12.x" | "10.x" | "8.10.x"
```

----------------------------------------

TITLE: Issue Certificate using Vercel SDK Standalone Function (TypeScript)
DESCRIPTION: Illustrates using the standalone certsIssueCert function from the Vercel SDK for better tree-shaking. Requires initializing VercelCore with a bearer token and passing the instance to the function. Includes error handling for the result.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/certs/README.md#_snippet_5

LANGUAGE: typescript
CODE:
```
import { VercelCore } from "@vercel/sdk/core.js";
import { certsIssueCert } from "@vercel/sdk/funcs/certsIssueCert.js";

// Use `VercelCore` for best tree-shaking performance.
// You can create one instance of it to use across an application.
const vercel = new VercelCore({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const res = await certsIssueCert(vercel, {
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
    slug: "my-team-url-slug",
    requestBody: {},
  });

  if (!res.ok) {
    throw res.error;
  }

  const { value: result } = res;

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Creating Project Environment Variable using Standalone Function
DESCRIPTION: This snippet shows how to create or update a project environment variable using the standalone `projectsCreateProjectEnv` function from the Vercel SDK core. This approach is recommended for better tree-shaking. It requires importing `VercelCore` and the specific function, creating a `VercelCore` instance, and passing the instance along with the configuration object (project details, upsert flag, team details, and environment variable details) to the function. It includes basic error handling.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/projects/README.md#_snippet_27

LANGUAGE: typescript
CODE:
```
import { VercelCore } from "@vercel/sdk/core.js";
import { projectsCreateProjectEnv } from "@vercel/sdk/funcs/projectsCreateProjectEnv.js";

// Use `VercelCore` for best tree-shaking performance.
// You can create one instance of it to use across an application.
const vercel = new VercelCore({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const res = await projectsCreateProjectEnv(vercel, {
    idOrName: "prj_XLKmu1DyR1eY7zq8UgeRKbA7yVLA",
    upsert: "true",
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
    slug: "my-team-url-slug",
    requestBody: {
      key: "API_URL",
      value: "https://api.vercel.com",
      type: "plain",
      target: [
        "preview"
      ],
      gitBranch: "feature-1",
      comment: "database connection string for production",
      customEnvironmentIds: [
        "env_1234567890"
      ]
    }
  });

  if (!res.ok) {
    throw res.error;
  }

  const { value: result } = res;

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Using Standalone Function for Domain Creation or Transfer
DESCRIPTION: Illustrates the usage of the standalone domainsCreateOrTransferDomain function with a VercelCore instance. This approach is recommended for better tree-shaking. The example shows how to pass parameters and handle the function's result, including checking for errors.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/domains/README.md#_snippet_15

LANGUAGE: typescript
CODE:
```
import { VercelCore } from "@vercel/sdk/core.js";
import { domainsCreateOrTransferDomain } from "@vercel/sdk/funcs/domainsCreateOrTransferDomain.js";

// Use `VercelCore` for best tree-shaking performance.
// You can create one instance of it to use across an application.
const vercel = new VercelCore({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const res = await domainsCreateOrTransferDomain(vercel, {
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
    slug: "my-team-url-slug",
    requestBody: {
      name: "example.com",
      method: "transfer-in",
      authCode: "fdhfr820ad#@FAdlj$$",
      expectedPrice: 8,
    },
  });

  if (!res.ok) {
    throw res.error;
  }

  const { value: result } = res;

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Get Domain Transfer Status using Standalone Function (TypeScript)
DESCRIPTION: Shows how to use the standalone domainsGetDomainTransfer function with a VercelCore instance. This approach is recommended for better tree-shaking performance. Requires importing VercelCore and the specific function.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/domains/README.md#_snippet_7

LANGUAGE: typescript
CODE:
```
import { VercelCore } from "@vercel/sdk/core.js";
import { domainsGetDomainTransfer } from "@vercel/sdk/funcs/domainsGetDomainTransfer.js";

// Use `VercelCore` for best tree-shaking performance.
// You can create one instance of it to use across an application.
const vercel = new VercelCore({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const res = await domainsGetDomainTransfer(vercel, {
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
    slug: "my-team-url-slug",
    domain: "example.com",
  });

  if (!res.ok) {
    throw res.error;
  }

  const { value: result } = res;

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Updating Vercel Project using SDK (TypeScript)
DESCRIPTION: This snippet demonstrates initializing the Vercel SDK with a bearer token and then calling the `updateProject` method to modify an existing Vercel project. It requires the project's `idOrName` and can include a `requestBody` object to specify the fields to update, such as the project `name`. The updated project details are logged.
SOURCE: https://github.com/vercel/sdk/blob/main/README.md#_snippet_12

LANGUAGE: typescript
CODE:
```
import { Vercel } from "@vercel/sdk";

const vercel = new Vercel({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const result = await vercel.projects.updateProject({
    idOrName: "prj_12HKQaOmR5t5Uy6vdcQsNIiZgHGB",
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
    slug: "my-team-url-slug",
    requestBody: {
      name: "a-project-name",
    },
  });

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Update Edge Config Schema using Standalone Function (TypeScript)
DESCRIPTION: This snippet shows how to update an Edge Config's schema using the standalone `edgeConfigPatchEdgeConfigSchema` function from the Vercel SDK's core module. It requires passing a `VercelCore` instance and the configuration object, including the Edge Config ID and the new schema definition.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/edgeconfig/README.md#_snippet_15

LANGUAGE: typescript
CODE:
```
import { VercelCore } from "@vercel/sdk/core.js";
import { edgeConfigPatchEdgeConfigSchema } from "@vercel/sdk/funcs/edgeConfigPatchEdgeConfigSchema.js";

// Use `VercelCore` for best tree-shaking performance.
// You can create one instance of it to use across an application.
const vercel = new VercelCore({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const res = await edgeConfigPatchEdgeConfigSchema(vercel, {
    edgeConfigId: "<id>",
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
    slug: "my-team-url-slug",
    requestBody: {
      definition: "<value>",
    },
  });

  if (!res.ok) {
    throw res.error;
  }

  const { value: result } = res;

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: List Team Members using Vercel SDK Class (TypeScript)
DESCRIPTION: Demonstrates how to use the `getTeamMembers` method on an instance of the main `Vercel` class to fetch a paginated list of team members. It shows how to initialize the SDK with a bearer token and pass parameters like limit, time ranges, and role.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/teams/README.md#_snippet_0

LANGUAGE: typescript
CODE:
```
import { Vercel } from "@vercel/sdk";

const vercel = new Vercel({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const result = await vercel.teams.getTeamMembers({
    limit: 20,
    since: 1540095775951,
    until: 1540095775951,
    role: "OWNER",
  });

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Example Usage of UpdateRecordRequestBody in TypeScript
DESCRIPTION: This snippet demonstrates how to create an instance of the UpdateRecordRequestBody type, showing how to populate various fields including basic properties like name, value, type, and ttl, as well as nested objects for srv and https records. It requires importing the type from the @vercel/sdk.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/models/updaterecordrequestbody.md#_snippet_0

LANGUAGE: typescript
CODE:
```
import { UpdateRecordRequestBody } from "@vercel/sdk/models/updaterecordop.js";

let value: UpdateRecordRequestBody = {
  name: "example-1",
  value: "google.com",
  type: "A",
  ttl: 60,
  srv: {
    target: "example2.com.",
    weight: 748755,
    port: 23554,
    priority: 402103,
  },
  https: {
    priority: 198927,
    target: "example2.com.",
  },
  comment: "used to verify ownership of domain"
};
```

----------------------------------------

TITLE: Uploading File Stream with Vercel SDK (TypeScript)
DESCRIPTION: Demonstrates how to upload a file using the Vercel SDK's `uploadArtifact` method by passing a file handle obtained via `node:fs.openAsBlob` as the `requestBody`. This approach streams the file content, preventing excessive memory usage for large files. Requires the `@vercel/sdk` package and a JavaScript runtime supporting `openAsBlob` (like Node.js v20+).
SOURCE: https://github.com/vercel/sdk/blob/main/README.md#_snippet_13

LANGUAGE: typescript
CODE:
```
import { Vercel } from "@vercel/sdk";
import { openAsBlob } from "node:fs";

const vercel = new Vercel({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const result = await vercel.artifacts.uploadArtifact({
    contentLength: 4504.13,
    xArtifactDuration: 400,
    xArtifactClientCi: "VERCEL",
    xArtifactClientInteractive: 0,
    xArtifactTag: "Tc0BmHvJYMIYJ62/zx87YqO0Flxk+5Ovip25NY825CQ=",
    hash: "12HKQaOmR5t5Uy6vdcQsNIiZgHGB",
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
    slug: "my-team-url-slug",
    requestBody: await openAsBlob("example.file"),
  });

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Instantiating RequestAccessToTeamJoinedFrom Object in TypeScript
DESCRIPTION: This snippet demonstrates how to create an instance of the `RequestAccessToTeamJoinedFrom` type from the `@vercel/sdk` library. It shows the necessary import statement and how to initialize an object with example values for its fields, such as `origin`, `commitId`, `repoId`, `repoPath`, `gitUserId`, and `gitUserLogin`, illustrating the structure required for representing access request details originating from a team join event.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/models/requestaccesstoteamjoinedfrom.md#_snippet_0

LANGUAGE: typescript
CODE:
```
import { RequestAccessToTeamJoinedFrom } from "@vercel/sdk/models/requestaccesstoteamop.js";

let value: RequestAccessToTeamJoinedFrom = {
  origin: "github",
  commitId: "f498d25d8bd654b578716203be73084b31130cd7",
  repoId: "67753070",
  repoPath: "jane-doe/example",
  gitUserId: 103053343,
  gitUserLogin: "jane-doe"
};
```

----------------------------------------

TITLE: Retrieving Deployment with Vercel SDK Class - TypeScript
DESCRIPTION: This snippet demonstrates how to retrieve Vercel deployment information by calling the `getDeployment` method on an instance of the `Vercel` class. It requires providing either the deployment ID or URL via the `idOrUrl` parameter, and optionally includes parameters like `withGitRepoInfo`, `teamId`, and `slug` for more detailed results or team context.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/deployments/README.md#_snippet_4

LANGUAGE: typescript
CODE:
```
import { Vercel } from "@vercel/sdk";

const vercel = new Vercel({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const result = await vercel.deployments.getDeployment({
    idOrUrl: "dpl_89qyp1cskzkLrVicDaZoDbjyHuDJ",
    withGitRepoInfo: "true",
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
    slug: "my-team-url-slug",
  });

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Editing Vercel Project Environment Variable using Vercel SDK Standalone Function (TypeScript)
DESCRIPTION: Shows how to edit a Vercel project environment variable using the standalone `projectsEditProjectEnv` function from `@vercel/sdk/funcs`. It requires a `VercelCore` instance and the same parameters as the class method, demonstrating a tree-shaking friendly approach. It also includes basic error handling.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/projects/README.md#_snippet_33

LANGUAGE: TypeScript
CODE:
```
import { VercelCore } from "@vercel/sdk/core.js";
import { projectsEditProjectEnv } from "@vercel/sdk/funcs/projectsEditProjectEnv.js";

// Use `VercelCore` for best tree-shaking performance.
// You can create one instance of it to use across an application.
const vercel = new VercelCore({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const res = await projectsEditProjectEnv(vercel, {
    idOrName: "prj_XLKmu1DyR1eY7zq8UgeRKbA7yVLA",
    id: "XMbOEya1gUUO1ir4",
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
    slug: "my-team-url-slug",
    requestBody: {
      key: "GITHUB_APP_ID",
      target: [
        "preview"
      ],
      gitBranch: "feature-1",
      type: "plain",
      value: "bkWIjbnxcvo78",
      customEnvironmentIds: [
        "env_1234567890"
      ],
      comment: "database connection string for production"
    }
  });

  if (!res.ok) {
    throw res.error;
  }

  const { value: result } = res;

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Installing @vercel/sdk with PNPM (Bash)
DESCRIPTION: Installs the @vercel/sdk package using the pnpm package manager.
SOURCE: https://github.com/vercel/sdk/blob/main/README.md#_snippet_1

LANGUAGE: Bash
CODE:
```
pnpm add @vercel/sdk
```

----------------------------------------

TITLE: Retrieving Deployment with Vercel SDK Standalone Function - TypeScript
DESCRIPTION: This snippet shows how to use the standalone `deploymentsGetDeployment` function from `@vercel/sdk/funcs` to fetch deployment details. It utilizes a `VercelCore` instance for potential tree-shaking benefits and passes the instance along with the deployment parameters (`idOrUrl`, `withGitRepoInfo`, `teamId`, `slug`) to the function. The example includes basic error handling.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/deployments/README.md#_snippet_5

LANGUAGE: typescript
CODE:
```
import { VercelCore } from "@vercel/sdk/core.js";
import { deploymentsGetDeployment } from "@vercel/sdk/funcs/deploymentsGetDeployment.js";

// Use \`VercelCore\` for best tree-shaking performance.
// You can create one instance of it to use across an application.
const vercel = new VercelCore({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const res = await deploymentsGetDeployment(vercel, {
    idOrUrl: "dpl_89qyp1cskzkLrVicDaZoDbjyHuDJ",
    withGitRepoInfo: "true",
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
    slug: "my-team-url-slug",
  });

  if (!res.ok) {
    throw res.error;
  }

  const { value: result } = res;

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Creating a CreateWebhookRequestBody instance in TypeScript
DESCRIPTION: This snippet demonstrates how to create an instance of the CreateWebhookRequestBody type, importing it from the Vercel SDK and providing values for the required 'url' and 'events' fields.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/models/createwebhookrequestbody.md#_snippet_0

LANGUAGE: typescript
CODE:
```
import { CreateWebhookRequestBody } from "@vercel/sdk/models/createwebhookop.js";

let value: CreateWebhookRequestBody = {
  url: "https://thin-gazebo.name",
  events: [
    "budget.reached",
  ],
};
```

----------------------------------------

TITLE: Creating Edge Config Token using Vercel SDK instance (TypeScript)
DESCRIPTION: This snippet demonstrates how to create a new token for an existing Vercel Edge Config using the `Vercel` class instance. It requires providing the Edge Config ID, optional team ID/slug, and a label for the new token in the request body.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/edgeconfig/README.md#_snippet_26

LANGUAGE: TypeScript
CODE:
```
import { Vercel } from "@vercel/sdk";

const vercel = new Vercel({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const result = await vercel.edgeConfig.createEdgeConfigToken({
    edgeConfigId: "<id>",
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
    slug: "my-team-url-slug",
    requestBody: {
      label: "<value>",
    },
  });

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Installing @vercel/sdk with NPM (Bash)
DESCRIPTION: Installs the @vercel/sdk package using the npm package manager.
SOURCE: https://github.com/vercel/sdk/blob/main/README.md#_snippet_0

LANGUAGE: Bash
CODE:
```
npm add @vercel/sdk
```

----------------------------------------

TITLE: Create DNS Record using Vercel SDK Standalone Function (TypeScript)
DESCRIPTION: Shows how to create a DNS record using the standalone `dnsCreateRecord` function from `@vercel/sdk/funcs`. It requires importing `VercelCore` (for tree-shaking) and `dnsCreateRecord`. The function is called with the `VercelCore` instance and an options object containing `domain`, `teamId`, `slug`, and the `requestBody`. It also includes error handling.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/dns/README.md#_snippet_3

LANGUAGE: typescript
CODE:
```
import { VercelCore } from "@vercel/sdk/core.js";
import { dnsCreateRecord } from "@vercel/sdk/funcs/dnsCreateRecord.js";

// Use \`VercelCore\` for best tree-shaking performance.
// You can create one instance of it to use across an application.
const vercel = new VercelCore({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const res = await dnsCreateRecord(vercel, {
    domain: "example.com",
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
    slug: "my-team-url-slug",
    requestBody: {
      type: "CNAME",
      ttl: 60,
      https: {
        priority: 10,
        target: "host.example.com",
        params: "alpn=h2,h3",
      },
      comment: "used to verify ownership of domain",
    },
  });

  if (!res.ok) {
    throw res.error;
  }

  const { value: result } = res;

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Listing Access Group Projects using Standalone Function (TypeScript)
DESCRIPTION: This example shows how to list access group projects using the standalone `accessGroupsListAccessGroupProjects` function from the Vercel SDK core. This method is recommended for better tree-shaking performance. It requires a `VercelCore` instance and parameters to specify the access group and pagination.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/accessgroups/README.md#_snippet_13

LANGUAGE: typescript
CODE:
```
import { VercelCore } from "@vercel/sdk/core.js";
import { accessGroupsListAccessGroupProjects } from "@vercel/sdk/funcs/accessGroupsListAccessGroupProjects.js";

// Use `VercelCore` for best tree-shaking performance.
// You can create one instance of it to use across an application.
const vercel = new VercelCore({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const res = await accessGroupsListAccessGroupProjects(vercel, {
    idOrName: "ag_pavWOn1iLObbXLRiwVvzmPrTWyTf",
    limit: 20,
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
    slug: "my-team-url-slug",
  });

  if (!res.ok) {
    throw res.error;
  }

  const { value: result } = res;

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Delete Vercel Auth Token using Vercel SDK Class - TypeScript
DESCRIPTION: Demonstrates how to invalidate a Vercel authentication token using the `deleteAuthToken` method on an instance of the `Vercel` class from the @vercel/sdk. Requires a bearer token for initialization and the ID of the token to delete.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/authentication/README.md#_snippet_8

LANGUAGE: typescript
CODE:
```
import { Vercel } from "@vercel/sdk";

const vercel = new Vercel({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const result = await vercel.authentication.deleteAuthToken({
    tokenId: "5d9f2ebd38ddca62e5d51e9c1704c72530bdc8bfdd41e782a6687c48399e8391",
  });

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Submitting Invoice using Standalone Function (TypeScript)
DESCRIPTION: This snippet illustrates how to use the standalone `marketplaceSubmitInvoice` function with `VercelCore` for potentially better tree-shaking. It shows initializing `VercelCore` with a bearer token and calling the function directly, including basic error handling.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/marketplace/README.md#_snippet_9

LANGUAGE: typescript
CODE:
```
import { VercelCore } from "@vercel/sdk/core.js";
import { marketplaceSubmitInvoice } from "@vercel/sdk/funcs/marketplaceSubmitInvoice.js";

// Use `VercelCore` for best tree-shaking performance.
// You can create one instance of it to use across an application.
const vercel = new VercelCore({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const res = await marketplaceSubmitInvoice(vercel, {
    integrationConfigurationId: "<id>",
    requestBody: {
      invoiceDate: new Date("2023-06-05T08:54:16.353Z"),
      period: {
        start: new Date("2023-07-26T14:15:15.601Z"),
        end: new Date("2025-10-08T09:35:48.520Z"),
      },
      items: [
        {
          billingPlanId: "<id>",
          name: "<value>",
          price: "905.89",
          quantity: 1684.76,
          units: "<value>",
          total: "<value>"
        },
        {
          billingPlanId: "<id>",
          name: "<value>",
          price: "84.05",
          quantity: 9130.95,
          units: "<value>",
          total: "<value>"
        }
      ]
    }
  });

  if (!res.ok) {
    throw res.error;
  }

  const { value: result } = res;

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Initializing models.SixtyTwo (TypeScript)
DESCRIPTION: This snippet demonstrates how to initialize an instance of the `models.SixtyTwo` data model in TypeScript, including nested objects like `newOwner` with detailed properties.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/models/payload.md#_snippet_61

LANGUAGE: typescript
CODE:
```
const value: models.SixtyTwo = {
  userId: "<id>",
  integrationId: "<id>",
  configurationId: "<id>",
  integrationSlug: "<value>",
  newOwner: {
    billing: {
      plan: "hobby"
    },
    blocked: 7057.27,
    createdAt: 2477.04,
    deploymentSecret: "<value>",
    email: "Berta_Gerlach66@gmail.com",
    id: "<id>",
    platformVersion: 2389.59,
    stagingPrefix: "<value>",
    sysToken: "<value>",
    type: "user",
    username: "Abdullah_Stark",
    updatedAt: 5779.7,
    version: "northstar"
  }
};
```

----------------------------------------

TITLE: Updating a Vercel Project using Standalone Function (TypeScript)
DESCRIPTION: Demonstrates how to use a Vercel SDK standalone function (`projectsUpdateProject`) with a `VercelCore` instance. It shows how to pass parameters and handle the function's `Result` type using a switch statement to check for success (`res.ok`) or specific error types like `SDKValidationError`.
SOURCE: https://github.com/vercel/sdk/blob/main/FUNCTIONS.md#_snippet_0

LANGUAGE: typescript
CODE:
```
import { VercelCore } from "@vercel/sdk/core.js";
import { projectsUpdateProject } from "@vercel/sdk/funcs/projectsUpdateProject.js";
import { SDKValidationError } from "@vercel/sdk/models/sdkvalidationerror.js";

// Use `VercelCore` for best tree-shaking performance.
// You can create one instance of it to use across an application.
const vercel = new VercelCore({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const res = await projectsUpdateProject(vercel, {
    idOrName: "prj_12HKQaOmR5t5Uy6vdcQsNIiZgHGB",
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
    slug: "my-team-url-slug",
    requestBody: {
      name: "a-project-name",
    },
  });

  switch (true) {
    case res.ok:
      // The success case will be handled outside of the switch block
      break;
    case res.error instanceof SDKValidationError:
      // Pretty-print validation errors.
      return console.log(res.error.pretty());
    case res.error instanceof Error:
      return console.log(res.error);
    default:
      // TypeScript's type checking will fail on the following line if the above
      // cases were not exhaustive.
      res.error satisfies never;
      throw new Error("Assertion failed: expected error checks to be exhaustive: " + res.error);
  }


  const { value: result } = res;

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Override Vercel SDK Server URL in TypeScript
DESCRIPTION: This snippet shows how to override the default server URL used by the Vercel SDK client. You can specify a custom URL by passing the `serverURL` option during the SDK client initialization. Subsequent API calls made through this client instance will use the provided URL instead of the default.
SOURCE: https://github.com/vercel/sdk/blob/main/README.md#_snippet_17

LANGUAGE: typescript
CODE:
```
import { Vercel } from "@vercel/sdk";

const vercel = new Vercel({
  serverURL: "https://api.vercel.com",
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const result = await vercel.accessGroups.readAccessGroup({
    idOrName: "ag_1a2b3c4d5e6f7g8h9i0j",
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
    slug: "my-team-url-slug",
  });

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Updating Domain using Vercel SDK (Class Instance) - TypeScript
DESCRIPTION: Demonstrates how to update or move an apex domain using the `patchDomain` method via an instance of the `Vercel` class from the Vercel SDK. It shows initialization with a bearer token and calling the method with domain details and update operation.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/domains/README.md#_snippet_16

LANGUAGE: typescript
CODE:
```
import { Vercel } from "@vercel/sdk";

const vercel = new Vercel({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const result = await vercel.domains.patchDomain({
    domain: "tight-secrecy.info",
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
    slug: "my-team-url-slug",
    requestBody: {
      op: "update"
    }
  });

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Instantiate models.AddProjectMemberRequestBody2 - TypeScript
DESCRIPTION: This snippet demonstrates how to create an instance of the models.AddProjectMemberRequestBody2 type in TypeScript, showing example values for the uid, username, email, and role properties.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/models/addprojectmemberrequestbody.md#_snippet_1

LANGUAGE: typescript
CODE:
```
const value: models.AddProjectMemberRequestBody2 = {
  uid: "ndlgr43fadlPyCtREAqxxdyFK",
  username: "example",
  email: "entity@example.com",
  role: "ADMIN"
};
```

----------------------------------------

TITLE: Getting Edge Config Schema using Standalone Function (TypeScript)
DESCRIPTION: This snippet shows how to fetch an Edge Config schema using the standalone `edgeConfigGetEdgeConfigSchema` function, which is suitable for better tree-shaking. It takes a `VercelCore` instance and an options object containing `edgeConfigId` and optionally `teamId` or `slug`, including basic error handling.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/edgeconfig/README.md#_snippet_13

LANGUAGE: typescript
CODE:
```
import { VercelCore } from "@vercel/sdk/core.js";
import { edgeConfigGetEdgeConfigSchema } from "@vercel/sdk/funcs/edgeConfigGetEdgeConfigSchema.js";

// Use `VercelCore` for best tree-shaking performance.
// You can create one instance of it to use across an application.
const vercel = new VercelCore({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const res = await edgeConfigGetEdgeConfigSchema(vercel, {
    edgeConfigId: "<id>",
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
    slug: "my-team-url-slug",
  });

  if (!res.ok) {
    throw res.error;
  }

  const { value: result } = res;

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Get Authentication Token using Vercel SDK (TypeScript)
DESCRIPTION: Demonstrates how to initialize the Vercel SDK with a bearer token and fetch an authentication token using the `authentication.getAuthToken` method. It logs the result to the console. Requires the `@vercel/sdk` package.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/authentication/README.md#_snippet_6

LANGUAGE: typescript
CODE:
```
import { Vercel } from "@vercel/sdk";

const vercel = new Vercel({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>"
});

async function run() {
  const result = await vercel.authentication.getAuthToken({
    tokenId: "5d9f2ebd38ddca62e5d51e9c1704c72530bdc8bfdd41e782a6687c48399e8391"
  });

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Unpausing a Vercel Project using Vercel SDK
DESCRIPTION: This snippet demonstrates how to unpause a Vercel project using an instance of the Vercel class from the @vercel/sdk package. It requires the project ID and optionally team ID or slug.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/projects/README.md#_snippet_46

LANGUAGE: typescript
CODE:
```
import { Vercel } from "@vercel/sdk";

const vercel = new Vercel({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  await vercel.projects.unpauseProject({
    projectId: "<id>",
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
    slug: "my-team-url-slug",
  });


}

run();
```

----------------------------------------

TITLE: Update Integration Deployment Action - Vercel Class - TypeScript
DESCRIPTION: This snippet demonstrates how to use the Vercel SDK's main class to update a deployment integration action. It requires initializing the Vercel client with a bearer token and then calling the `updateIntegrationDeploymentAction` method with the necessary parameters.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/integrations/README.md#_snippet_0

LANGUAGE: typescript
CODE:
```
import { Vercel } from "@vercel/sdk";

const vercel = new Vercel({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  await vercel.integrations.updateIntegrationDeploymentAction({
    deploymentId: "<id>",
    integrationConfigurationId: "<id>",
    resourceId: "<id>",
    action: "<value>",
  });


}

run();
```

----------------------------------------

TITLE: Moving Project Domain using Vercel SDK Class (TypeScript)
DESCRIPTION: Demonstrates how to move a project's domain using the `moveProjectDomain` method of the `Vercel` class instance. Requires importing `Vercel` from `@vercel/sdk`. Shows how to instantiate the class with a bearer token and call the method with project details, domain, team ID, and slug.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/projects/README.md#_snippet_20

LANGUAGE: typescript
CODE:
```
import { Vercel } from "@vercel/sdk";

const vercel = new Vercel({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const result = await vercel.projects.moveProjectDomain({
    idOrName: "<value>",
    domain: "www.example.com",
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
    slug: "my-team-url-slug",
  });

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Listing Project Promote Aliases using Vercel SDK Standalone Function (TypeScript)
DESCRIPTION: This snippet shows how to list project promote aliases using the standalone `projectsListPromoteAliases` function. It utilizes an instance of `VercelCore` for potentially better tree-shaking and passes the instance along with the parameters (`projectId`, `limit`, `since`, `until`, `teamId`, `slug`) to the function. It also includes basic error handling.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/projects/README.md#_snippet_43

LANGUAGE: typescript
CODE:
```
import { VercelCore } from "@vercel/sdk/core.js";
import { projectsListPromoteAliases } from "@vercel/sdk/funcs/projectsListPromoteAliases.js";

// Use `VercelCore` for best tree-shaking performance.
// You can create one instance of it to use across an application.
const vercel = new VercelCore({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>"
});

async function run() {
  const res = await projectsListPromoteAliases(vercel, {
    projectId: "<id>",
    limit: 20,
    since: 1609499532000,
    until: 1612264332000,
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
    slug: "my-team-url-slug"
  });

  if (!res.ok) {
    throw res.error;
  }

  const { value: result } = res;

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Listing Deployments using Standalone Function (TypeScript)
DESCRIPTION: This snippet shows how to list deployments using the standalone `deploymentsGetDeployments` function, which is recommended for better tree-shaking. It requires an instance of `VercelCore` and passes the filtering parameters as the second argument.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/deployments/README.md#_snippet_17

LANGUAGE: typescript
CODE:
```
import { VercelCore } from "@vercel/sdk/core.js";
import { deploymentsGetDeployments } from "@vercel/sdk/funcs/deploymentsGetDeployments.js";

// Use `VercelCore` for best tree-shaking performance.
// You can create one instance of it to use across an application.
const vercel = new VercelCore({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const res = await deploymentsGetDeployments(vercel, {
    app: "docs",
    from: 1612948664566,
    limit: 10,
    projectId: "QmXGTs7mvAMMC7WW5ebrM33qKG32QK3h4vmQMjmY",
    target: "production",
    to: 1612948664566,
    users: "kr1PsOIzqEL5Xg6M4VZcZosf,K4amb7K9dAt5R2vBJWF32bmY",
    since: 1540095775941,
    until: 1540095775951,
    state: "BUILDING,READY",
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
    slug: "my-team-url-slug",
  });

  if (!res.ok) {
    throw res.error;
  }

  const { value: result } = res;

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Retrieving Edge Config Backup using Standalone Function (TypeScript)
DESCRIPTION: Shows how to use the standalone `edgeConfigGetEdgeConfigBackup` function with `VercelCore` for potentially better tree-shaking. The function takes a `VercelCore` instance and an options object including Edge Config ID, backup version ID, and optional team details.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/edgeconfig/README.md#_snippet_29

LANGUAGE: typescript
CODE:
```
import { VercelCore } from "@vercel/sdk/core.js";
import { edgeConfigGetEdgeConfigBackup } from "@vercel/sdk/funcs/edgeConfigGetEdgeConfigBackup.js";

// Use `VercelCore` for best tree-shaking performance.
// You can create one instance of it to use across an application.
const vercel = new VercelCore({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const res = await edgeConfigGetEdgeConfigBackup(vercel, {
    edgeConfigId: "<id>",
    edgeConfigBackupVersionId: "<id>",
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
    slug: "my-team-url-slug",
  });

  if (!res.ok) {
    throw res.error;
  }

  const { value: result } = res;

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Requesting Team Access using Standalone Function - Typescript
DESCRIPTION: Shows how to request access to a Vercel team using the standalone `teamsRequestAccessToTeam` function from `@vercel/sdk/funcs`. This approach uses `VercelCore` for potentially better tree-shaking and demonstrates handling the result, including error checking.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/teams/README.md#_snippet_5

LANGUAGE: Typescript
CODE:
```
import { VercelCore } from "@vercel/sdk/core.js";
import { teamsRequestAccessToTeam } from "@vercel/sdk/funcs/teamsRequestAccessToTeam.js";

// Use \`VercelCore\` for best tree-shaking performance.
// You can create one instance of it to use across an application.
const vercel = new VercelCore({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const res = await teamsRequestAccessToTeam(vercel, {
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
    requestBody: {
      joinedFrom: {
        origin: "github",
        commitId: "f498d25d8bd654b578716203be73084b31130cd7",
        repoId: "67753070",
        repoPath: "jane-doe/example",
        gitUserId: 103053343,
        gitUserLogin: "jane-doe",
      },
    },
  });

  if (!res.ok) {
    throw res.error;
  }

  const { value: result } = res;

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Purchasing Domain using Standalone Function (TypeScript)
DESCRIPTION: Illustrates how to use the `VercelCore` instance and the specific `domainsBuyDomain` standalone function for potentially better tree-shaking. Shows how to pass the core instance and the domain purchase parameters, including basic error handling.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/domains/README.md#_snippet_1

LANGUAGE: typescript
CODE:
```
import { VercelCore } from "@vercel/sdk/core.js";
import { domainsBuyDomain } from "@vercel/sdk/funcs/domainsBuyDomain.js";

// Use `VercelCore` for best tree-shaking performance.
// You can create one instance of it to use across an application.
const vercel = new VercelCore({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const res = await domainsBuyDomain(vercel, {
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
    slug: "my-team-url-slug",
    requestBody: {
      name: "example.com",
      expectedPrice: 10,
      renew: true,
      country: "US",
      orgName: "Acme Inc.",
      firstName: "Jane",
      lastName: "Doe",
      address1: "340 S Lemon Ave Suite 4133",
      city: "San Francisco",
      state: "CA",
      postalCode: "91789",
      phone: "+1.4158551452",
      email: "jane.doe@someplace.com",
    },
  });

  if (!res.ok) {
    throw res.error;
  }

  const { value: result } = res;

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Get Edge Config Token using Standalone Function (TypeScript)
DESCRIPTION: Illustrates how to use the VercelCore class and the standalone edgeConfigGetEdgeConfigToken function for potentially better tree-shaking. It shows how to initialize VercelCore, pass the instance to the standalone function along with required parameters, handle the result, and check for errors.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/edgeconfig/README.md#_snippet_25

LANGUAGE: typescript
CODE:
```
import { VercelCore } from "@vercel/sdk/core.js";
import { edgeConfigGetEdgeConfigToken } from "@vercel/sdk/funcs/edgeConfigGetEdgeConfigToken.js";

// Use `VercelCore` for best tree-shaking performance.
// You can create one instance of it to use across an application.
const vercel = new VercelCore({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const res = await edgeConfigGetEdgeConfigToken(vercel, {
    edgeConfigId: "<id>",
    token: "<value>",
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
    slug: "my-team-url-slug",
  });

  if (!res.ok) {
    throw res.error;
  }

  const { value: result } = res;

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Create DNS Record using Vercel SDK Class (TypeScript)
DESCRIPTION: Demonstrates how to create a DNS record using the `vercel.dns.createRecord` method on an instance of the `Vercel` class. It requires importing `Vercel` and initializing it with a bearer token. The method takes parameters like `domain`, `teamId`, `slug`, and a `requestBody` object defining the record type, TTL, and specific record data (like HTTPS parameters for CNAME).
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/dns/README.md#_snippet_2

LANGUAGE: typescript
CODE:
```
import { Vercel } from "@vercel/sdk";

const vercel = new Vercel({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const result = await vercel.dns.createRecord({
    domain: "example.com",
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
    slug: "my-team-url-slug",
    requestBody: {
      type: "CNAME",
      ttl: 60,
      https: {
        priority: 10,
        target: "host.example.com",
        params: "alpn=h2,h3",
      },
      comment: "used to verify ownership of domain",
    },
  });

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Update Edge Config using Standalone Function (TypeScript)
DESCRIPTION: This snippet shows how to update a Vercel Edge Config using the standalone `edgeConfigUpdateEdgeConfig` function for better tree-shaking. It requires initializing `VercelCore` with a bearer token and passing the instance along with the update parameters (`edgeConfigId`, `teamId`, `slug`, `requestBody`) to the function. It also includes basic error handling.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/edgeconfig/README.md#_snippet_7

LANGUAGE: typescript
CODE:
```
import { VercelCore } from "@vercel/sdk/core.js";
import { edgeConfigUpdateEdgeConfig } from "@vercel/sdk/funcs/edgeConfigUpdateEdgeConfig.js";

// Use `VercelCore` for best tree-shaking performance.
// You can create one instance of it to use across an application.
const vercel = new VercelCore({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const res = await edgeConfigUpdateEdgeConfig(vercel, {
    edgeConfigId: "<id>",
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
    slug: "my-team-url-slug",
    requestBody: {
      slug: "<value>"
    }
  });

  if (!res.ok) {
    throw res.error;
  }

  const { value: result } = res;

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Add Project Domain using Vercel SDK Standalone Function (TypeScript)
DESCRIPTION: Shows how to add a domain using the standalone `projectsAddProjectDomain` function from `@vercel/sdk/funcs`. It utilizes `VercelCore` for potential tree-shaking benefits and demonstrates handling the result, including error checking.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/projects/README.md#_snippet_19

LANGUAGE: TypeScript
CODE:
```
import { VercelCore } from "@vercel/sdk/core.js";
import { projectsAddProjectDomain } from "@vercel/sdk/funcs/projectsAddProjectDomain.js";

// Use \`VercelCore\` for best tree-shaking performance.
// You can create one instance of it to use across an application.
const vercel = new VercelCore({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const res = await projectsAddProjectDomain(vercel, {
    idOrName: "<value>",
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
    slug: "my-team-url-slug",
    requestBody: {
      name: "www.example.com",
      gitBranch: null,
      redirect: "foobar.com",
      redirectStatusCode: 307,
    },
  });

  if (!res.ok) {
    throw res.error;
  }

  const { value: result } = res;

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Create Deployment using Standalone Function (TypeScript)
DESCRIPTION: Illustrates how to create a Vercel deployment using the standalone deploymentsCreateDeployment function from the core SDK module. This approach is recommended for better tree-shaking performance. It shows initializing VercelCore and passing it along with the deployment configuration to the function.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/deployments/README.md#_snippet_7

LANGUAGE: typescript
CODE:
```
import { VercelCore } from "@vercel/sdk/core.js";
import { deploymentsCreateDeployment } from "@vercel/sdk/funcs/deploymentsCreateDeployment.js";

// Use `VercelCore` for best tree-shaking performance.
// You can create one instance of it to use across an application.
const vercel = new VercelCore({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const res = await deploymentsCreateDeployment(vercel, {
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
    slug: "my-team-url-slug",
    requestBody: {
      deploymentId: "dpl_2qn7PZrx89yxY34vEZPD31Y9XVj6",
      files: [
        {
          file: "folder/file.js",
        },
        {
          file: "folder/file.js",
        },
      ],
      gitMetadata: {
        remoteUrl: "https://github.com/vercel/next.js",
        commitAuthorName: "kyliau",
        commitMessage: "add method to measure Interaction to Next Paint (INP) (#36490)",
        commitRef: "main",
        commitSha: "dc36199b2234c6586ebe05ec94078a895c707e29",
        dirty: true,
      },
      gitSource: {
        ref: "main",
        repoId: 123456789,
        sha: "a1b2c3d4e5f6g7h8i9j0k1l2m3n4o5p6q7r8s9t0",
        type: "github",
      },
      meta: {
        "foo": "bar",
      },
      name: "my-instant-deployment",
      project: "my-deployment-project",
      projectSettings: {
        buildCommand: "next build",
        installCommand: "pnpm install",
      },
      target: "production",
    },
  });

  if (!res.ok) {
    throw res.error;
  }

  const { value: result } = res;

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Listing User Events using Standalone Function Vercel SDK TypeScript
DESCRIPTION: This example illustrates using the standalone `userListUserEvents` function with `VercelCore` for optimized bundle size. It initializes `VercelCore` and calls the function, passing the core instance and parameters. It includes basic error handling by checking the `res.ok` property and extracting the result.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/user/README.md#_snippet_1

LANGUAGE: typescript
CODE:
```
import { VercelCore } from "@vercel/sdk/core.js";
import { userListUserEvents } from "@vercel/sdk/funcs/userListUserEvents.js";

// Use `VercelCore` for best tree-shaking performance.
// You can create one instance of it to use across an application.
const vercel = new VercelCore({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const res = await userListUserEvents(vercel, {
    limit: 20,
    since: "2019-12-08T10:00:38.976Z",
    until: "2019-12-09T23:00:38.976Z",
    types: "login,team-member-join,domain-buy",
    userId: "aeIInYVk59zbFF2SxfyxxmuO",
    principalId: "aeIInYVk59zbFF2SxfyxxmuO",
    projectIds: "aeIInYVk59zbFF2SxfyxxmuO",
    withPayload: "true",
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
    slug: "my-team-url-slug",
  });

  if (!res.ok) {
    throw res.error;
  }

  const { value: result } = res;

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Retrieve Bypass IP Rules using Vercel Class - TypeScript
DESCRIPTION: This snippet demonstrates how to retrieve system bypass rules for a Vercel project using the `getBypassIp` method available on an instance of the `Vercel` class. It shows how to import the class, instantiate it with a bearer token, and call the method with necessary parameters like `projectId`, `limit`, `teamId`, and `slug`. The result is then logged to the console.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/security/README.md#_snippet_10

LANGUAGE: typescript
CODE:
```
import { Vercel } from "@vercel/sdk";

const vercel = new Vercel({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const result = await vercel.security.getBypassIp({
    projectId: "<id>",
    limit: 10,
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
    slug: "my-team-url-slug",
  });

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Get Authenticated User using Vercel SDK Standalone Function (TypeScript)
DESCRIPTION: Shows how to use the standalone `userGetAuthUser` function with `VercelCore` for better tree-shaking. It initializes `VercelCore` with a bearer token and calls the function, demonstrating how to check the result and handle potential errors.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/user/README.md#_snippet_3

LANGUAGE: TypeScript
CODE:
```
import { VercelCore } from "@vercel/sdk/core.js";
import { userGetAuthUser } from "@vercel/sdk/funcs/userGetAuthUser.js";

// Use `VercelCore` for best tree-shaking performance.
// You can create one instance of it to use across an application.
const vercel = new VercelCore({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const res = await userGetAuthUser(vercel);

  if (!res.ok) {
    throw res.error;
  }

  const { value: result } = res;

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Initializing Vercel SDK and Reading Access Group (TypeScript)
DESCRIPTION: This snippet demonstrates how to initialize the Vercel SDK client by providing a bearer token for authentication. It then shows an example of calling the `readAccessGroup` method to retrieve details for a specific access group, requiring parameters like `idOrName`, `teamId`, and `slug`. The result is logged to the console.
SOURCE: https://github.com/vercel/sdk/blob/main/README.md#_snippet_10

LANGUAGE: typescript
CODE:
```
import { Vercel } from "@vercel/sdk";

const vercel = new Vercel({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const result = await vercel.accessGroups.readAccessGroup({
    idOrName: "ag_1a2b3c4d5e6f7g8h9i0j",
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
    slug: "my-team-url-slug",
  });

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Creating a Team using Standalone Function (VercelCore)
DESCRIPTION: Shows how to create a Vercel Team using the standalone `teamsCreateTeam` function, which is recommended for better tree-shaking. Requires importing `VercelCore` and `teamsCreateTeam`. An instance of `VercelCore` is passed to the function along with the team details. Includes basic error handling.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/teams/README.md#_snippet_21

LANGUAGE: typescript
CODE:
```
import { VercelCore } from "@vercel/sdk/core.js";
import { teamsCreateTeam } from "@vercel/sdk/funcs/teamsCreateTeam.js";

// Use \`VercelCore\` for best tree-shaking performance.
// You can create one instance of it to use across an application.
const vercel = new VercelCore({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const res = await teamsCreateTeam(vercel, {
    slug: "a-random-team",
    name: "A Random Team",
  });

  if (!res.ok) {
    throw res.error;
  }

  const { value: result } = res;

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Listing Vercel Deployments using SDK (TypeScript)
DESCRIPTION: This example shows how to initialize the Vercel SDK with a bearer token and then use the `getDeployments` method to list deployments. It includes various optional parameters to filter the results, such as `app`, `limit`, `projectId`, `target`, `teamId`, and others. The fetched deployment list is printed to the console.
SOURCE: https://github.com/vercel/sdk/blob/main/README.md#_snippet_11

LANGUAGE: typescript
CODE:
```
import { Vercel } from "@vercel/sdk";

const vercel = new Vercel({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const result = await vercel.deployments.getDeployments({
    app: "docs",
    from: 1612948664566,
    limit: 10,
    projectId: "QmXGTs7mvAMMC7WW5ebrM33qKG32QK3h4vmQMjmY",
    target: "production",
    to: 1612948664566,
    users: "kr1PsOIzqEL5Xg6M4VZcZosf,K4amb7K9dAt5R2vBJWF32bmY",
    since: 1540095775941,
    until: 1540095775951,
    state: "BUILDING,READY",
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
    slug: "my-team-url-slug",
  });

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Update Installation Integration Edge Config using Vercel SDK (TypeScript)
DESCRIPTION: Demonstrates how to update the Edge Config for a Vercel integration installation using the main `Vercel` SDK class. It shows initialization with a bearer token and calling the `marketplace.updateInstallationIntegrationEdgeConfig` method.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/marketplace/README.md#_snippet_32

LANGUAGE: typescript
CODE:
```
import { Vercel } from "@vercel/sdk";

const vercel = new Vercel({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const result = await vercel.marketplace.updateInstallationIntegrationEdgeConfig({
    integrationConfigurationId: "<id>",
    resourceId: "<id>",
  });

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Getting Invoice Details with Standalone Function - TypeScript
DESCRIPTION: This snippet shows how to use the standalone `marketplaceGetInvoice` function for potentially better tree-shaking. It requires an instance of `VercelCore` and calls the function directly, handling the result and potential errors.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/marketplace/README.md#_snippet_11

LANGUAGE: typescript
CODE:
```
import { VercelCore } from "@vercel/sdk/core.js";
import { marketplaceGetInvoice } from "@vercel/sdk/funcs/marketplaceGetInvoice.js";

// Use `VercelCore` for best tree-shaking performance.
// You can create one instance of it to use across an application.
const vercel = new VercelCore({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const res = await marketplaceGetInvoice(vercel, {
    integrationConfigurationId: "<id>",
    invoiceId: "<id>",
  });

  if (!res.ok) {
    throw res.error;
  }

  const { value: result } = res;

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Initializing RequestBody4 in TypeScript
DESCRIPTION: This snippet demonstrates how to create an instance of the RequestBody4 type, which is used for defining a DNS record. It shows how to set the required fields like 'name', 'type', and 'value', as well as optional fields like 'ttl' and 'comment'.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/models/requestbody4.md#_snippet_0

LANGUAGE: typescript
CODE:
```
import { RequestBody4 } from "@vercel/sdk/models/createrecordop.js";

let value: RequestBody4 = {
  name: "subdomain",
  type: "HTTPS",
  ttl: 60,
  value: "0 issue \"letsencrypt.org\"",
  comment: "used to verify ownership of domain"
};
```

----------------------------------------

TITLE: Updating Project Data Cache using Standalone Function (TypeScript)
DESCRIPTION: Illustrates how to use the standalone `projectsUpdateProjectDataCache` function with the `VercelCore` class for potentially better tree-shaking. It shows initializing `VercelCore`, calling the specific function directly, and handling the response including error checking.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/projects/README.md#_snippet_1

LANGUAGE: typescript
CODE:
```
import { VercelCore } from "@vercel/sdk/core.js";
import { projectsUpdateProjectDataCache } from "@vercel/sdk/funcs/projectsUpdateProjectDataCache.js";

// Use `VercelCore` for best tree-shaking performance.
// You can create one instance of it to use across an application.
const vercel = new VercelCore({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const res = await projectsUpdateProjectDataCache(vercel, {
    projectId: "prj_12HKQaOmR5t5Uy6vdcQsNIiZgHGB",
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
    slug: "my-team-url-slug",
    requestBody: {
      disabled: true,
    },
  });

  if (!res.ok) {
    throw res.error;
  }

  const { value: result } = res;

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Reading Access Group using Vercel SDK (TypeScript)
DESCRIPTION: Demonstrates how to read a specific access group using the main `Vercel` class instance. It requires initializing the SDK with a bearer token and calling the `accessGroups.readAccessGroup` method with the access group's ID or name, team ID, and slug. The result is logged to the console.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/accessgroups/README.md#_snippet_0

LANGUAGE: typescript
CODE:
```
import { Vercel } from "@vercel/sdk";

const vercel = new Vercel({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const result = await vercel.accessGroups.readAccessGroup({
    idOrName: "ag_1a2b3c4d5e6f7g8h9i0j",
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
    slug: "my-team-url-slug",
  });

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Possible Values for RequestBodyType in Vercel SDK (TypeScript)
DESCRIPTION: Lists the valid string literal values that the RequestBodyType can accept, corresponding to standard DNS record types.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/models/requestbodytype.md#_snippet_1

LANGUAGE: TypeScript
CODE:
```
"A" | "AAAA" | "ALIAS" | "CAA" | "CNAME" | "HTTPS" | "MX" | "SRV" | "TXT" | "NS"
```

----------------------------------------

TITLE: Initializing CreateAuthTokenRequest in TypeScript
DESCRIPTION: This snippet demonstrates how to create an instance of the `CreateAuthTokenRequest` type, showing the structure and example values for its fields, including `teamId`, `slug`, and the nested `requestBody` with a `name` property. It requires importing the type from the `@vercel/sdk/models/createauthtokenop.js` module.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/models/createauthtokenrequest.md#_snippet_0

LANGUAGE: typescript
CODE:
```
import { CreateAuthTokenRequest } from "@vercel/sdk/models/createauthtokenop.js";

let value: CreateAuthTokenRequest = {
  teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
  slug: "my-team-url-slug",
  requestBody: {
    name: "<value>",
  },
};
```

----------------------------------------

TITLE: Removing Bypass IP with Vercel Class (TypeScript)
DESCRIPTION: This snippet demonstrates how to remove a bypass IP rule using an instance of the main Vercel class. It requires the project ID, team ID, and team slug.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/security/README.md#_snippet_14

LANGUAGE: typescript
CODE:
```
import { Vercel } from "@vercel/sdk";

const vercel = new Vercel({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const result = await vercel.security.removeBypassIp({
    projectId: "<id>",
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
    slug: "my-team-url-slug",
  });

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Get Teams using Vercel SDK Standalone Function - TypeScript
DESCRIPTION: This example illustrates fetching teams using the standalone `teamsGetTeams` function, which is recommended with `VercelCore` for optimal tree-shaking. It shows how to initialize `VercelCore`, pass the instance to the function along with pagination options, and handle the result including potential errors.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/teams/README.md#_snippet_19

LANGUAGE: TypeScript
CODE:
```
import { VercelCore } from "@vercel/sdk/core.js";
import { teamsGetTeams } from "@vercel/sdk/funcs/teamsGetTeams.js";

// Use `VercelCore` for best tree-shaking performance.
// You can create one instance of it to use across an application.
const vercel = new VercelCore({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const res = await teamsGetTeams(vercel, {
    limit: 20,
    since: 1540095775951,
    until: 1540095775951,
  });

  if (!res.ok) {
    throw res.error;
  }

  const { value: result } = res;

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Initializing ProjectMembership in Typescript
DESCRIPTION: This snippet demonstrates how to import the ProjectMembership type from the Vercel SDK and declare a variable of this type, initializing it as an empty object.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/models/projectmembership.md#_snippet_0

LANGUAGE: typescript
CODE:
```
import { ProjectMembership } from "@vercel/sdk/models/userevent.js";

let value: ProjectMembership = {};
```

----------------------------------------

TITLE: Getting Edge Configs using Vercel SDK Standalone Function (TypeScript)
DESCRIPTION: Illustrates fetching all Vercel Edge Configs using the tree-shakable standalone function `edgeConfigGetEdgeConfigs` with the `VercelCore` instance. Requires a bearer token and handles potential errors.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/edgeconfig/README.md#_snippet_1

LANGUAGE: typescript
CODE:
```
import { VercelCore } from "@vercel/sdk/core.js";
import { edgeConfigGetEdgeConfigs } from "@vercel/sdk/funcs/edgeConfigGetEdgeConfigs.js";

// Use `VercelCore` for best tree-shaking performance.
// You can create one instance of it to use across an application.
const vercel = new VercelCore({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const res = await edgeConfigGetEdgeConfigs(vercel, {
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
    slug: "my-team-url-slug",
  });

  if (!res.ok) {
    throw res.error;
  }

  const { value: result } = res;

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Pause Vercel Project using Standalone Function (TypeScript)
DESCRIPTION: Illustrates pausing a Vercel project using the standalone `projectsPauseProject` function from `@vercel/sdk/funcs`. This approach uses `VercelCore` for better tree-shaking. It requires a `VercelCore` instance and an options object including `projectId` and optionally `teamId` or `slug`.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/projects/README.md#_snippet_45

LANGUAGE: TypeScript
CODE:
```
import { VercelCore } from "@vercel/sdk/core.js";
import { projectsPauseProject } from "@vercel/sdk/funcs/projectsPauseProject.js";

// Use \`VercelCore\` for best tree-shaking performance.
// You can create one instance of it to use across an application.
const vercel = new VercelCore({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>"
});

async function run() {
  const res = await projectsPauseProject(vercel, {
    projectId: "<id>",
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
    slug: "my-team-url-slug"
  });

  if (!res.ok) {
    throw res.error;
  }

  const { value: result } = res;

  
}

run();
```

----------------------------------------

TITLE: Uploading Vercel Artifact using Standalone Function (TypeScript)
DESCRIPTION: Shows how to upload a cache artifact using the standalone `artifactsUploadArtifact` function from `@vercel/sdk/funcs`. This approach uses a `VercelCore` instance and is recommended for better tree-shaking. It requires similar artifact details as the instance method.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/artifacts/README.md#_snippet_5

LANGUAGE: typescript
CODE:
```
import { VercelCore } from "@vercel/sdk/core.js";
import { artifactsUploadArtifact } from "@vercel/sdk/funcs/artifactsUploadArtifact.js";
import { openAsBlob } from "node:fs";

// Use `VercelCore` for best tree-shaking performance.
// You can create one instance of it to use across an application.
const vercel = new VercelCore({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const res = await artifactsUploadArtifact(vercel, {
    contentLength: 4504.13,
    xArtifactDuration: 400,
    xArtifactClientCi: "VERCEL",
    xArtifactClientInteractive: 0,
    xArtifactTag: "Tc0BmHvJYMIYJ62/zx87YqO0Flxk+5Ovip25NY825CQ=",
    hash: "12HKQaOmR5t5Uy6vdcQsNIiZgHGB",
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
    slug: "my-team-url-slug",
    requestBody: await openAsBlob("example.file"),
  });

  if (!res.ok) {
    throw res.error;
  }

  const { value: result } = res;

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Creating GetDeploymentsRequest Object in TypeScript
DESCRIPTION: This snippet demonstrates how to create an instance of the `GetDeploymentsRequest` type from the Vercel SDK, populating it with various properties like application name, date ranges, limits, project ID, target environment, user IDs, states, and team information.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/models/getdeploymentsrequest.md#_snippet_0

LANGUAGE: typescript
CODE:
```
import { GetDeploymentsRequest } from "@vercel/sdk/models/getdeploymentsop.js";

let value: GetDeploymentsRequest = {
  app: "docs",
  from: 1612948664566,
  limit: 10,
  projectId: "QmXGTs7mvAMMC7WW5ebrM33qKG32QK3h4vmQMjmY",
  target: "production",
  to: 1612948664566,
  users: "kr1PsOIzqEL5Xg6M4VZcZosf,K4amb7K9dAt5R2vBJWF32bmY",
  since: 1540095775941,
  until: 1540095775951,
  state: "BUILDING,READY",
  teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
  slug: "my-team-url-slug"
};
```

----------------------------------------

TITLE: Record Artifact Events using Vercel SDK (Standalone Function)
DESCRIPTION: Illustrates how to record cache usage events using the standalone `artifactsRecordEvents` function. This approach is recommended for better tree-shaking and requires passing a `VercelCore` instance as the first argument, followed by the parameters for the event recording.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/artifacts/README.md#_snippet_1

LANGUAGE: typescript
CODE:
```
import { VercelCore } from "@vercel/sdk/core.js";
import { artifactsRecordEvents } from "@vercel/sdk/funcs/artifactsRecordEvents.js";

// Use \`VercelCore\` for best tree-shaking performance.
// You can create one instance of it to use across an application.
const vercel = new VercelCore({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const res = await artifactsRecordEvents(vercel, {
    xArtifactClientCi: "VERCEL",
    xArtifactClientInteractive: 0,
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
    slug: "my-team-url-slug",
    requestBody: [
      {
        sessionId: "<id>",
        source: "LOCAL",
        event: "HIT",
        hash: "12HKQaOmR5t5Uy6vdcQsNIiZgHGB",
        duration: 400,
      },
    ],
  });

  if (!res.ok) {
    throw res.error;
  }

  const { value: result } = res;

  
}

run();
```

----------------------------------------

TITLE: Initializing UpdateFirewallConfigRequestBody3 in TypeScript
DESCRIPTION: This snippet demonstrates how to create an instance of the UpdateFirewallConfigRequestBody3 type in TypeScript, populating it with example values required to update a custom firewall rule. It shows the structure for specifying the action, rule ID, and the new rule configuration including name, active status, condition groups, and action.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/models/updatefirewallconfigrequestbody3.md#_snippet_0

LANGUAGE: typescript
CODE:
```
import { UpdateFirewallConfigRequestBody3 } from "@vercel/sdk/models/updatefirewallconfigop.js";

let value: UpdateFirewallConfigRequestBody3 = {
  action: "rules.update",
  id: "<id>",
  value: {
    name: "<value>",
    active: false,
    conditionGroup: [
      {
        conditions: [
          {
            type: "user_agent",
            op: "ex",
          },
        ],
      },\n    ],
    action: {},
  },
};
```

----------------------------------------

TITLE: Listing possible values for GetDeploymentEventsResponseBodyDeploymentsResponseType (TypeScript)
DESCRIPTION: This snippet defines the union of string literals that constitute the valid values for the `GetDeploymentEventsResponseBodyDeploymentsResponseType` type, representing different types of deployment events.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/models/getdeploymenteventsresponsebodydeploymentsresponsetype.md#_snippet_1

LANGUAGE: typescript
CODE:
```
"delimiter" | "command" | "stdout" | "stderr" | "exit" | "deployment-state" | "middleware" | "middleware-invocation" | "edge-function-invocation" | "metric" | "report" | "fatal"
```

----------------------------------------

TITLE: Example Usage of GetProjectMembersResponseBody2 in TypeScript
DESCRIPTION: This snippet demonstrates how to instantiate and populate a GetProjectMembersResponseBody2 object in TypeScript, showing the expected structure for a paginated list of project members with example data.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/models/getprojectmembersresponsebody2.md#_snippet_0

LANGUAGE: typescript
CODE:
```
import { GetProjectMembersResponseBody2 } from "@vercel/sdk/models/getprojectmembersop.js";

let value: GetProjectMembersResponseBody2 = {
  members: [
    {
      avatar: "123a6c5209bc3778245d011443644c8d27dc2c50",
      email: "jane.doe@example.com",
      role: "ADMIN",
      computedProjectRole: "ADMIN",
      uid: "zTuNVUXEAvvnNN3IaqinkyMw",
      username: "jane-doe",
      name: "Jane Doe",
      createdAt: 1588720733602,
      teamRole: "CONTRIBUTOR"
    }
  ],
  pagination: {
    hasNext: false,
    count: 20,
    next: 1540095775951,
    prev: 1540095775951
  }
};
```

----------------------------------------

TITLE: Creating a NewEnvVar Object in TypeScript
DESCRIPTION: This snippet shows how to create a `NewEnvVar` object instance. It imports the type and initializes an object with example values for properties such as creation/update timestamps, keys, IDs, owner/creator information, associated projects, type (e.g., 'encrypted'), and target environments.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/models/newenvvar.md#_snippet_0

LANGUAGE: typescript
CODE:
```
import { NewEnvVar } from "@vercel/sdk/models/userevent.js";

let value: NewEnvVar = {
  created: new Date("2021-02-10T13:11:49.180Z"),
  key: "my-api-key",
  ownerId: "team_LLHUOMOoDlqOp8wPE4kFo9pE",
  id: "env_XCG7t7AIHuO2SBA8667zNUiM",
  createdBy: "2qDDuGFTWXBLDNnqZfWPDp1A",
  deletedBy: "2qDDuGFTWXBLDNnqZfWPDp1A",
  updatedBy: "2qDDuGFTWXBLDNnqZfWPDp1A",
  createdAt: 1609492210000,
  deletedAt: 1609492210000,
  updatedAt: 1609492210000,
  projectId: [
    "prj_2WjyKQmM8ZnGcJsPWMrHRHrE",
    "prj_2WjyKQmM8ZnGcJsPWMrasEFg"
  ],
  type: "encrypted",
  target: [
    "production"
  ]
};
```

----------------------------------------

TITLE: Possible Values - CreateRecordRequestBodyDnsRequest10Type - TypeScript
DESCRIPTION: Lists the valid string literal values that the CreateRecordRequestBodyDnsRequest10Type can accept, representing the supported DNS record types.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/models/createrecordrequestbodydnsrequest10type.md#_snippet_1

LANGUAGE: typescript
CODE:
```
"A" | "AAAA" | "ALIAS" | "CAA" | "CNAME" | "HTTPS" | "MX" | "SRV" | "TXT" | "NS"
```

----------------------------------------

TITLE: Initializing Populated ListAccessGroupsResponseBody2 - TypeScript
DESCRIPTION: This snippet shows how to initialize a variable of type `models.ListAccessGroupsResponseBody2` with example data, including a list of access groups and pagination details, illustrating a typical successful response.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/models/listaccessgroupsresponsebody.md#_snippet_1

LANGUAGE: typescript
CODE:
```
const value: models.ListAccessGroupsResponseBody2 = {
  accessGroups: [
    {
      isDsyncManaged: false,
      name: "my-access-group",
      createdAt: "1588720733602",
      teamId: "team_123a6c5209bc3778245d011443644c8d27dc2c50",
      updatedAt: "1588720733602",
      accessGroupId: "ag_123a6c5209bc3778245d011443644c8d27dc2c50",
      membersCount: 5,
      projectsCount: 2,
      teamRoles: [
        "DEVELOPER",
        "BILLING"
      ],
      teamPermissions: [
        "CreateProject"
      ]
    }
  ],
  pagination: {
    count: 6573.69,
    next: "<value>"
  }
};
```

----------------------------------------

TITLE: Fetching Account Info using Vercel SDK (TypeScript)
DESCRIPTION: This snippet demonstrates how to fetch account information using the `getAccountInfo` method available on the `vercel.marketplace` object when using the main Vercel SDK instance. It requires initializing the SDK with a bearer token and providing the integration configuration ID.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/marketplace/README.md#_snippet_0

LANGUAGE: typescript
CODE:
```
import { Vercel } from "@vercel/sdk";

const vercel = new Vercel({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const result = await vercel.marketplace.getAccountInfo({
    integrationConfigurationId: "<id>",
  });

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Constructing InviteUserToTeamRequest Object (TypeScript)
DESCRIPTION: This snippet demonstrates how to create an instance of the InviteUserToTeamRequest type, showing the required fields such as teamId and the nested requestBody which includes user identification (uid or email) and an array of projects with assigned roles.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/models/inviteusertoteamrequest.md#_snippet_0

LANGUAGE: typescript
CODE:
```
import { InviteUserToTeamRequest } from "@vercel/sdk/models/inviteusertoteamop.js";

let value: InviteUserToTeamRequest = {
  teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
  requestBody: {
    uid: "kr1PsOIzqEL5Xg6M4VZcZosf",
    email: "john@example.com",
    projects: [
      {
        projectId: "prj_ndlgr43fadlPyCtREAqxxdyFK",
        role: "ADMIN"
      }
    ]
  }
};
```

----------------------------------------

TITLE: Deleting Edge Config Tokens using Vercel SDK Class (TypeScript)
DESCRIPTION: This snippet demonstrates how to delete one or more tokens from an existing Edge Config using the `deleteEdgeConfigTokens` method of the `Vercel` class instance. It requires providing the Edge Config ID, optional team ID or slug, and a request body containing the list of tokens to delete.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/edgeconfig/README.md#_snippet_22

LANGUAGE: typescript
CODE:
```
import { Vercel } from "@vercel/sdk";

const vercel = new Vercel({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  await vercel.edgeConfig.deleteEdgeConfigTokens({
    edgeConfigId: "<id>",
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
    slug: "my-team-url-slug",
    requestBody: {
      tokens: [],
    },
  });


}

run();
```

----------------------------------------

TITLE: Remove DNS Record using Vercel Class (TypeScript)
DESCRIPTION: Demonstrates how to remove a DNS record using the `removeRecord` method available on the `dns` property of an initialized `Vercel` instance. Requires importing the `Vercel` class and providing a bearer token. The method takes an object with `domain`, `recordId`, and optional `teamId` or `slug`.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/dns/README.md#_snippet_6

LANGUAGE: typescript
CODE:
```
import { Vercel } from "@vercel/sdk";

const vercel = new Vercel({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const result = await vercel.dns.removeRecord({
    domain: "example.com",
    recordId: "rec_V0fra8eEgQwEpFhYG2vTzC3K",
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
    slug: "my-team-url-slug",
  });

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Exchange SSO Token using Vercel SDK (TypeScript)
DESCRIPTION: Demonstrates how to exchange an SSO authorization code for an OIDC token using the `Vercel` class instance from the Vercel SDK. This is typically used in the Open in Provider flow. Requires the authorization code, client ID, and client secret.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/authentication/README.md#_snippet_0

LANGUAGE: TypeScript
CODE:
```
import { Vercel } from "@vercel/sdk";

const vercel = new Vercel();

async function run() {
  const result = await vercel.authentication.exchangeSsoToken({
    code: "<value>",
    clientId: "<id>",
    clientSecret: "<value>"
  });

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Fetch Domain Config using VercelCore and Standalone Function (TypeScript)
DESCRIPTION: Shows how to fetch domain configuration using the VercelCore class and a standalone function for better tree-shaking. Instantiate VercelCore and pass the instance along with parameters to the domainsGetDomainConfig function, including basic error handling.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/domains/README.md#_snippet_9

LANGUAGE: typescript
CODE:
```
import { VercelCore } from "@vercel/sdk/core.js";
import { domainsGetDomainConfig } from "@vercel/sdk/funcs/domainsGetDomainConfig.js";

// Use `VercelCore` for best tree-shaking performance.
// You can create one instance of it to use across an application.
const vercel = new VercelCore({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const res = await domainsGetDomainConfig(vercel, {
    domain: "example.com",
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
    slug: "my-team-url-slug",
  });

  if (!res.ok) {
    throw res.error;
  }

  const { value: result } = res;

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Using Standalone Function to Get Edge Config Items (TypeScript)
DESCRIPTION: Shows how to use the standalone `edgeConfigGetEdgeConfigItems` function with `VercelCore` for better tree-shaking. It performs the same action as the class method but uses a functional approach and includes error handling.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/edgeconfig/README.md#_snippet_11

LANGUAGE: typescript
CODE:
```
import { VercelCore } from "@vercel/sdk/core.js";
import { edgeConfigGetEdgeConfigItems } from "@vercel/sdk/funcs/edgeConfigGetEdgeConfigItems.js";

// Use `VercelCore` for best tree-shaking performance.
// You can create one instance of it to use across an application.
const vercel = new VercelCore({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const res = await edgeConfigGetEdgeConfigItems(vercel, {
    edgeConfigId: "<id>",
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
    slug: "my-team-url-slug",
  });

  if (!res.ok) {
    throw res.error;
  }

  const { value: result } = res;

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Initializing UpdateResourceSecretsByIdSecrets Object in TypeScript
DESCRIPTION: This snippet demonstrates how to create an instance of the UpdateResourceSecretsByIdSecrets type in TypeScript, showing the structure with the required 'name' and 'value' fields. It requires importing the type from the Vercel SDK.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/models/updateresourcesecretsbyidsecrets.md#_snippet_0

LANGUAGE: typescript
CODE:
```
import { UpdateResourceSecretsByIdSecrets } from "@vercel/sdk/models/updateresourcesecretsbyidop.js";

let value: UpdateResourceSecretsByIdSecrets = {
  name: "<value>",
  value: "<value>"
};
```

----------------------------------------

TITLE: Creating VerifyProjectDomainRequest Object in TypeScript
DESCRIPTION: This snippet demonstrates how to import and instantiate a VerifyProjectDomainRequest object with example values for its required and optional fields. It shows the basic structure needed to define a request to verify a project domain.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/models/verifyprojectdomainrequest.md#_snippet_0

LANGUAGE: typescript
CODE:
```
import { VerifyProjectDomainRequest } from "@vercel/sdk/models/verifyprojectdomainop.js";

let value: VerifyProjectDomainRequest = {
  idOrName: "prj_12HKQaOmR5t5Uy6vdcQsNIiZgHGB",
  domain: "example.com",
  teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
  slug: "my-team-url-slug"
};
```

----------------------------------------

TITLE: Possible Values for CreateProjectProjectsFramework in TypeScript
DESCRIPTION: This snippet lists all the valid string literal values that the `CreateProjectProjectsFramework` type or enum can represent. Each string corresponds to a specific project framework supported by the Vercel SDK.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/models/createprojectprojectsframework.md#_snippet_1

LANGUAGE: typescript
CODE:
```
"blitzjs" | "nextjs" | "gatsby" | "remix" | "react-router" | "astro" | "hexo" | "eleventy" | "docusaurus-2" | "docusaurus" | "preact" | "solidstart-1" | "solidstart" | "dojo" | "ember" | "vue" | "scully" | "ionic-angular" | "angular" | "polymer" | "svelte" | "sveltekit" | "sveltekit-1" | "ionic-react" | "create-react-app" | "gridsome" | "umijs" | "sapper" | "saber" | "stencil" | "nuxtjs" | "redwoodjs" | "hugo" | "jekyll" | "brunch" | "middleman" | "zola" | "hydrogen" | "vite" | "vitepress" | "vuepress" | "parcel" | "fasthtml" | "sanity-v3" | "sanity" | "storybook"
```

----------------------------------------

TITLE: Calling getDeploymentEvents with Vercel SDK (TypeScript)
DESCRIPTION: This snippet demonstrates how to use the `getDeploymentEvents` method via the main `Vercel` class instance. It shows client instantiation, calling the method with various parameters, and handling the asynchronous result.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/deployments/README.md#_snippet_0

LANGUAGE: typescript
CODE:
```
import { Vercel } from "@vercel/sdk";

const vercel = new Vercel({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const result = await vercel.deployments.getDeploymentEvents({
    idOrUrl: "dpl_5WJWYSyB7BpgTj3EuwF37WMRBXBtPQ2iTMJHJBJyRfd",
    follow: 1,
    limit: 100,
    name: "bld_cotnkcr76",
    since: 1540095775941,
    until: 1540106318643,
    statusCode: "5xx",
    delimiter: 1,
    builds: 1,
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
    slug: "my-team-url-slug",
  });

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Rerequesting a Vercel Check using Standalone Function (TypeScript)
DESCRIPTION: Illustrates the usage of the standalone `checksRerequestCheck` function from `@vercel/sdk/funcs`. This approach is recommended for better tree-shaking performance and requires passing a `VercelCore` instance and an options object containing the deployment ID, check ID, and optional team details.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/checks/README.md#_snippet_9

LANGUAGE: typescript
CODE:
```
import { VercelCore } from "@vercel/sdk/core.js";
import { checksRerequestCheck } from "@vercel/sdk/funcs/checksRerequestCheck.js";

// Use `VercelCore` for best tree-shaking performance.
// You can create one instance of it to use across an application.
const vercel = new VercelCore({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const res = await checksRerequestCheck(vercel, {
    deploymentId: "dpl_2qn7PZrx89yxY34vEZPD31Y9XVj6",
    checkId: "check_2qn7PZrx89yxY34vEZPD31Y9XVj6",
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
    slug: "my-team-url-slug",
  });

  if (!res.ok) {
    throw res.error;
  }

  const { value: result } = res;

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Example Usage of GetConfigurationResponseBody2 in TypeScript
DESCRIPTION: This snippet demonstrates how to declare and initialize a variable of type GetConfigurationResponseBody2, populating it with example data. It shows the structure and expected data types for various fields within the object, including nested objects and arrays.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/models/getconfigurationresponsebody2.md#_snippet_0

LANGUAGE: typescript
CODE:
```
import { GetConfigurationResponseBody2 } from "@vercel/sdk/models/getconfigurationop.js";

let value: GetConfigurationResponseBody2 = {
  projectSelection: "all",
  transferRequest: {
    kind: "transfer-to-marketplace",
    requestId: "<id>",
    transferId: "<id>",
    requester: {
      name: "<value>"
    },
    createdAt: 5624.99,
    expiresAt: 9059.84
  },
  projects: [
    "prj_xQxbutw1HpL6HLYPAzt5h75m8NjO"
  ],
  completedAt: 1558531915505,
  createdAt: 1558531915505,
  id: "icfg_3bwCLgxL8qt5kjRLcv2Dit7F",
  integrationId: "oac_xzpVzcUOgcB1nrVlirtKhbWV",
  ownerId: "kr1PsOIzqEL5Xg6M4VZcZosf",
  source: "marketplace",
  slug: "slack",
  teamId: "team_nLlpyC6RE1qxydlFKbrxDlud",
  type: "integration-configuration",
  updatedAt: 1558531915505,
  userId: "kr1PsOIzqEL5Xg6M4VZcZosf",
  scopes: [
    "read:project",
    "read-write:log-drain"
  ],
  disabledAt: 1558531915505,
  deletedAt: 1558531915505,
  deleteRequestedAt: 1558531915505
};
```

----------------------------------------

TITLE: Instantiating CreateProjectEnvRequest in TypeScript
DESCRIPTION: This snippet demonstrates how to create an instance of the CreateProjectEnvRequest object, populating it with example data for creating or updating project environment variables. It shows the structure including project identification, upsert option, team details, and the array of environment variable objects.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/models/createprojectenvrequest.md#_snippet_0

LANGUAGE: typescript
CODE:
```
import { CreateProjectEnvRequest } from "@vercel/sdk/models/createprojectenvop.js";

let value: CreateProjectEnvRequest = {
  idOrName: "prj_XLKmu1DyR1eY7zq8UgeRKbA7yVLA",
  upsert: "true",
  teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
  slug: "my-team-url-slug",
  requestBody: [
    {
      key: "API_URL",
      value: "https://api.vercel.com",
      type: "plain",
      target: [
        "preview"
      ],
      gitBranch: "feature-1",
      comment: "database connection string for production",
      customEnvironmentIds: [
        "env_1234567890"
      ]
    }
  ]
};
```

----------------------------------------

TITLE: Initializing DeleteTeamRequest in TypeScript
DESCRIPTION: This snippet demonstrates how to create an instance of the DeleteTeamRequest type, importing it from the Vercel SDK and providing example values for its properties. It shows how to specify the team to delete using either its ID or slug, and optionally set a new default team.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/models/deleteteamrequest.md#_snippet_0

LANGUAGE: typescript
CODE:
```
import { DeleteTeamRequest } from "@vercel/sdk/models/deleteteamop.js";

let value: DeleteTeamRequest = {
  newDefaultTeamId: "team_LLHUOMOoDlqOp8wPE4kFo9pE",
  teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
  slug: "my-team-url-slug",
  requestBody: {},
};
```

----------------------------------------

TITLE: Deleting a Project using Standalone Function (TypeScript)
DESCRIPTION: This example shows how to delete a Vercel project using the standalone `projectsDeleteProject` function. This approach is recommended for better tree-shaking performance and requires passing a `VercelCore` instance and the project details. It includes basic error handling.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/projects/README.md#_snippet_9

LANGUAGE: TypeScript
CODE:
```
import { VercelCore } from "@vercel/sdk/core.js";
import { projectsDeleteProject } from "@vercel/sdk/funcs/projectsDeleteProject.js";

// Use `VercelCore` for best tree-shaking performance.
// You can create one instance of it to use across an application.
const vercel = new VercelCore({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const res = await projectsDeleteProject(vercel, {
    idOrName: "prj_12HKQaOmR5t5Uy6vdcQsNIiZgHGB",
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
    slug: "my-team-url-slug",
  });

  if (!res.ok) {
    throw res.error;
  }

  const { value: result } = res;

  
}

run();
```

----------------------------------------

TITLE: Requesting User Deletion with Standalone Function (TypeScript)
DESCRIPTION: Illustrates how to use the standalone `userRequestDelete` function from `@vercel/sdk/funcs/userRequestDelete.js`. It utilizes `VercelCore` for tree-shaking benefits and includes error handling to throw if the request is not successful.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/user/README.md#_snippet_5

LANGUAGE: typescript
CODE:
```
import { VercelCore } from "@vercel/sdk/core.js";
import { userRequestDelete } from "@vercel/sdk/funcs/userRequestDelete.js";

// Use `VercelCore` for best tree-shaking performance.
// You can create one instance of it to use across an application.
const vercel = new VercelCore({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const res = await userRequestDelete(vercel, {});

  if (!res.ok) {
    throw res.error;
  }

  const { value: result } = res;

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: List Project Members using Vercel SDK - TypeScript
DESCRIPTION: Initializes the Vercel SDK with a bearer token and calls the `projectMembers.getProjectMembers` method to retrieve a list of members for a specific project, team, and time range. It then logs the result. Requires the `@vercel/sdk` package.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/projectmembers/README.md#_snippet_0

LANGUAGE: TypeScript
CODE:
```
import { Vercel } from "@vercel/sdk";

const vercel = new Vercel({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const result = await vercel.projectMembers.getProjectMembers({
    idOrName: "prj_pavWOn1iLObbXLRiwVvzmPrTWyTf",
    limit: 20,
    since: 1540095775951,
    until: 1540095775951,
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
    slug: "my-team-url-slug",
  });

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Possible Values for UpdateFirewallConfigRequestBodyType - TypeScript
DESCRIPTION: This snippet lists all the valid string literal values that the UpdateFirewallConfigRequestBodyType can represent, corresponding to different parameters from incoming traffic that can be used in WAF rules.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/models/updatefirewallconfigrequestbodytype.md#_snippet_1

LANGUAGE: typescript
CODE:
```
"host" | "path" | "method" | "header" | "query" | "cookie" | "target_path" | "raw_path" | "ip_address" | "region" | "protocol" | "scheme" | "environment" | "user_agent" | "geo_continent" | "geo_country" | "geo_country_region" | "geo_city" | "geo_as_number" | "ja4_digest" | "ja3_digest" | "rate_limit_api_id"
```

----------------------------------------

TITLE: Possible Values for GetDeploymentsSource (TypeScript)
DESCRIPTION: Lists the allowed string literal values for the `GetDeploymentsSource` type, representing different sources for a Vercel deployment.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/models/getdeploymentssource.md#_snippet_1

LANGUAGE: typescript
CODE:
```
"api-trigger-git-deploy" | "cli" | "clone/repo" | "git" | "import" | "import/repo" | "redeploy" | "v0-web"
```

----------------------------------------

TITLE: QueryParamType Valid Values (TypeScript)
DESCRIPTION: Defines the possible string literal values that the QueryParamType can hold. These values represent different statuses of a domain for price checking.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/models/queryparamtype.md#_snippet_1

LANGUAGE: typescript
CODE:
```
"new" | "renewal" | "transfer" | "redemption"
```

----------------------------------------

TITLE: Possible Values for GetProjectsPlan Type (TypeScript)
DESCRIPTION: Lists the allowed literal string values that can be assigned to a variable of type GetProjectsPlan, defining the available project plan levels.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/models/getprojectsplan.md#_snippet_1

LANGUAGE: typescript
CODE:
```
"pro" | "enterprise" | "hobby"
```

----------------------------------------

TITLE: Possible Values - RemoveProjectEnvTargetProjectsResponse1 - TypeScript
DESCRIPTION: Lists the valid string literal values that can be assigned to a variable of type RemoveProjectEnvTargetProjectsResponse1.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/models/removeprojectenvtargetprojectsresponse1.md#_snippet_1

LANGUAGE: typescript
CODE:
```
"production" | "preview" | "development"
```

----------------------------------------

TITLE: Defining GetTeamMembersTeamsResponseRole Values (TypeScript)
DESCRIPTION: Specifies the union of string literal types that constitute the `GetTeamMembersTeamsResponseRole` type, listing all valid roles.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/models/getteammembersteamsresponserole.md#_snippet_1

LANGUAGE: typescript
CODE:
```
"ADMIN" | "PROJECT_DEVELOPER" | "PROJECT_VIEWER"
```

----------------------------------------

TITLE: Initializing ListDeploymentAliasesProtectionBypass2 TypeScript
DESCRIPTION: Demonstrates initializing an object of type `models.ListDeploymentAliasesProtectionBypass2` including `createdAt`, `lastUpdatedAt`, `lastUpdatedBy`, `access`, and `scope`. This type seems to represent a user-scoped bypass with an explicit access status.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/models/listdeploymentaliasesprotectionbypass.md#_snippet_1

LANGUAGE: typescript
CODE:
```
const value: models.ListDeploymentAliasesProtectionBypass2 = {
  createdAt: 4557.27,
  lastUpdatedAt: 4308.56,
  lastUpdatedBy: "<value>",
  access: "granted",
  scope: "user"
};
```

----------------------------------------

TITLE: Assigning Alias with Vercel SDK Class (TypeScript)
DESCRIPTION: Demonstrates how to assign an alias to a Vercel deployment using an instance of the `Vercel` class. It requires a bearer token for authentication and specifies the deployment ID, team ID, slug, and the desired alias in the request body. The result of the operation is logged to the console.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/aliases/README.md#_snippet_2

LANGUAGE: typescript
CODE:
```
import { Vercel } from "@vercel/sdk";

const vercel = new Vercel({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const result = await vercel.aliases.assignAlias({
    id: "<id>",
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
    slug: "my-team-url-slug",
    requestBody: {
      alias: "my-alias.vercel.app",
      redirect: null,
    },
  });

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Fetching Project Environment Variables using Standalone Function (TypeScript)
DESCRIPTION: This snippet demonstrates fetching project environment variables using the standalone projectsGetProjectEnv function along with VercelCore. This approach is recommended for better tree-shaking performance. It shows how to handle the result or potential errors returned by the function.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/projects/README.md#_snippet_29

LANGUAGE: typescript
CODE:
```
import { VercelCore } from "@vercel/sdk/core.js";
import { projectsGetProjectEnv } from "@vercel/sdk/funcs/projectsGetProjectEnv.js";

// Use `VercelCore` for best tree-shaking performance.
// You can create one instance of it to use across an application.
const vercel = new VercelCore({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const res = await projectsGetProjectEnv(vercel, {
    idOrName: "prj_XLKmu1DyR1eY7zq8UgeRKbA7yVLA",
    id: "<id>",
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
    slug: "my-team-url-slug",
  });

  if (!res.ok) {
    throw res.error;
  }

  const { value: result } = res;

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Creating Pagination Object in TypeScript
DESCRIPTION: Demonstrates how to import and create an instance of the Pagination object from @vercel/sdk, showing the required fields and their types.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/models/pagination.md#_snippet_0

LANGUAGE: TypeScript
CODE:
```
import { Pagination } from "@vercel/sdk/models/pagination.js";

let value: Pagination = {
  count: 20,
  next: 1540095775951,
  prev: 1540095775951,
};
```

----------------------------------------

TITLE: Updating Vercel Project Domain using Standalone Function (TypeScript)
DESCRIPTION: This snippet shows how to update a Vercel project domain using the standalone `projectsUpdateProjectDomain` function from `@vercel/sdk/funcs`. It utilizes `VercelCore` for better tree-shaking. The function is called with the `VercelCore` instance and an options object containing project details and the update request body. It also includes basic error handling.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/projects/README.md#_snippet_15

LANGUAGE: typescript
CODE:
```
import { VercelCore } from "@vercel/sdk/core.js";
import { projectsUpdateProjectDomain } from "@vercel/sdk/funcs/projectsUpdateProjectDomain.js";

// Use `VercelCore` for best tree-shaking performance.
// You can create one instance of it to use across an application.
const vercel = new VercelCore({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const res = await projectsUpdateProjectDomain(vercel, {
    idOrName: "<value>",
    domain: "www.example.com",
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
    slug: "my-team-url-slug",
    requestBody: {
      gitBranch: null,
      redirect: "foobar.com",
      redirectStatusCode: 307,
    },
  });

  if (!res.ok) {
    throw res.error;
  }

  const { value: result } = res;

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Example Usage of CreateAccessGroupProjectRequestBody in TypeScript
DESCRIPTION: This snippet demonstrates how to import and create an instance of the CreateAccessGroupProjectRequestBody type with example values for the required fields: projectId and role.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/models/createaccessgroupprojectrequestbody.md#_snippet_0

LANGUAGE: typescript
CODE:
```
import { CreateAccessGroupProjectRequestBody } from "@vercel/sdk/models/createaccessgroupprojectop.js";

let value: CreateAccessGroupProjectRequestBody = {
  projectId: "prj_ndlgr43fadlPyCtREAqxxdyFK",
  role: "ADMIN",
};
```

----------------------------------------

TITLE: Instantiating CreateAuthTokenRequestBody Model in Typescript
DESCRIPTION: This snippet demonstrates how to create an instance of the `CreateAuthTokenRequestBody` model from the `@vercel/sdk/models/createauthtokenop.js` module. It shows the minimum required field `name` being set.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/models/createauthtokenrequestbody.md#_snippet_0

LANGUAGE: typescript
CODE:
```
import { CreateAuthTokenRequestBody } from "@vercel/sdk/models/createauthtokenop.js";

let value: CreateAuthTokenRequestBody = {
  name: "<value>"
};
```

----------------------------------------

TITLE: Uploading File using Vercel Class (TypeScript)
DESCRIPTION: Demonstrates how to upload a file using the `uploadFile` method on an instance of the `Vercel` class. It requires a bearer token for authentication and specifies team details via `teamId` and `slug`.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/deployments/README.md#_snippet_10

LANGUAGE: typescript
CODE:
```
import { Vercel } from "@vercel/sdk";

const vercel = new Vercel({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const result = await vercel.deployments.uploadFile({
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
    slug: "my-team-url-slug",
  });

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Creating UpdateCheckMetrics Instance in TypeScript
DESCRIPTION: This snippet demonstrates how to initialize an object conforming to the UpdateCheckMetrics type in TypeScript, showing the structure for the required web vital metrics (fcp, lcp, cls, tbt) with example values and sources.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/models/updatecheckmetrics.md#_snippet_0

LANGUAGE: typescript
CODE:
```
import { UpdateCheckMetrics } from "@vercel/sdk/models/updatecheckop.js";

let value: UpdateCheckMetrics = {
  fcp: {
    value: 5468.59,
    source: "web-vitals"
  },
  lcp: {
    value: 1319.71,
    source: "web-vitals"
  },
  cls: {
    value: 3110.66,
    source: "web-vitals"
  },
  tbt: {
    value: 4415.23,
    source: "web-vitals"
  }
};
```

----------------------------------------

TITLE: Listing Vercel Auth Tokens using Standalone Function (TypeScript)
DESCRIPTION: Shows how to list authentication tokens using the standalone `authenticationListAuthTokens` function from the Vercel SDK core. It initializes `VercelCore` for better tree-shaking and handles the result, including potential errors.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/authentication/README.md#_snippet_3

LANGUAGE: typescript
CODE:
```
import { VercelCore } from "@vercel/sdk/core.js";
import { authenticationListAuthTokens } from "@vercel/sdk/funcs/authenticationListAuthTokens.js";

// Use `VercelCore` for best tree-shaking performance.
// You can create one instance of it to use across an application.
const vercel = new VercelCore({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>"
});

async function run() {
  const res = await authenticationListAuthTokens(vercel);

  if (!res.ok) {
    throw res.error;
  }

  const { value: result } = res;

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Updating a Project with Vercel SDK (TypeScript)
DESCRIPTION: This snippet illustrates how to update an existing Vercel project using the SDK. It requires the @vercel/sdk dependency and authentication via a bearer token. The project can be identified by its ID or name, and the example shows how to update the project's name within the request body.
SOURCE: https://github.com/vercel/sdk/blob/main/USAGE.md#_snippet_1

LANGUAGE: typescript
CODE:
```
import { Vercel } from "@vercel/sdk";

const vercel = new Vercel({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const result = await vercel.projects.updateProject({
    idOrName: "prj_12HKQaOmR5t5Uy6vdcQsNIiZgHGB",
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
    slug: "my-team-url-slug",
    requestBody: {
      name: "a-project-name",
    },
  });

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Setting Vercel Firewall Config using Vercel Class (TypeScript)
DESCRIPTION: This example demonstrates how to set the Vercel firewall configuration using the putFirewallConfig method of the Vercel class from the @vercel/sdk. It requires a bearer token for authentication and specifies the project or team ID/slug and the firewall settings in the request body.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/security/README.md#_snippet_2

LANGUAGE: typescript
CODE:
```
import { Vercel } from "@vercel/sdk";

const vercel = new Vercel({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const result = await vercel.security.putFirewallConfig({
    projectId: "<id>",
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
    slug: "my-team-url-slug",
    requestBody: {
      firewallEnabled: true,
    },
  });

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Querying Artifacts using Vercel SDK Class (TypeScript)
DESCRIPTION: This snippet shows how to initialize the Vercel SDK using the `Vercel` class and then call the `artifacts.artifactQuery` method to retrieve artifacts based on provided hashes. It requires a bearer token for authentication and specifies team and slug details.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/artifacts/README.md#_snippet_10

LANGUAGE: typescript
CODE:
```
import { Vercel } from "@vercel/sdk";

const vercel = new Vercel({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const result = await vercel.artifacts.artifactQuery({
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
    slug: "my-team-url-slug",
    requestBody: {
      hashes: [
        "12HKQaOmR5t5Uy6vdcQsNIiZgHGB",
        "34HKQaOmR5t5Uy6vasdasdasdasd"
      ]
    }
  });

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Example of CreateProjectEnvRequestBody2 Array in TypeScript
DESCRIPTION: Illustrates the structure for an array of CreateProjectEnvRequestBody2 objects, used to define multiple environment variables for a Vercel project in a single request.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/models/createprojectenvrequestbody.md#_snippet_1

LANGUAGE: typescript
CODE:
```
const value: models.CreateProjectEnvRequestBody2[] = [
  {
    key: "API_URL",
    value: "https://api.vercel.com",
    type: "plain",
    target: [
      "preview",
    ],
    gitBranch: "feature-1",
    comment: "database connection string for production",
    customEnvironmentIds: [
      "env_1234567890",
    ],
  },
];
```

----------------------------------------

TITLE: Creating a CreateLogDrainRequest instance in TypeScript
DESCRIPTION: This snippet demonstrates how to create an instance of the `CreateLogDrainRequest` object in TypeScript, populating it with example values for `teamId`, `slug`, and the nested `requestBody` fields like `name`, `secret`, `deliveryFormat`, and `url`.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/models/createlogdrainrequest.md#_snippet_0

LANGUAGE: typescript
CODE:
```
import { CreateLogDrainRequest } from "@vercel/sdk/models/createlogdrainop.js";

let value: CreateLogDrainRequest = {
  teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
  slug: "my-team-url-slug",
  requestBody: {
    name: "My first log drain",
    secret: "a1Xsfd325fXcs",
    deliveryFormat: "json",
    url: "https://example.com/log-drain",
  },
};
```

----------------------------------------

TITLE: Creating CreateEventRequestBody Instance in TypeScript
DESCRIPTION: This snippet demonstrates how to create an instance of the `CreateEventRequestBody` type in TypeScript, showing the required `event` field with its nested properties like `type`, `productId`, and `resourceId`. It requires importing the type from the SDK models.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/models/createeventrequestbody.md#_snippet_0

LANGUAGE: typescript
CODE:
```
import { CreateEventRequestBody } from "@vercel/sdk/models/createeventop.js";

let value: CreateEventRequestBody = {
  event: {
    type: "resource.updated",
    productId: "<id>",
    resourceId: "<id>",
  },
};
```

----------------------------------------

TITLE: Possible Values for CancelDeploymentFramework (TypeScript)
DESCRIPTION: This snippet lists all the valid string literal values that the `CancelDeploymentFramework` type can accept, representing the supported web frameworks for deployment cancellation within the Vercel platform.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/models/canceldeploymentframework.md#_snippet_1

LANGUAGE: typescript
CODE:
```
"blitzjs" | "nextjs" | "gatsby" | "remix" | "react-router" | "astro" | "hexo" | "eleventy" | "docusaurus-2" | "docusaurus" | "preact" | "solidstart-1" | "solidstart" | "dojo" | "ember" | "vue" | "scully" | "ionic-angular" | "angular" | "polymer" | "svelte" | "sveltekit" | "sveltekit-1" | "ionic-react" | "create-react-app" | "gridsome" | "umijs" | "sapper" | "saber" | "stencil" | "nuxtjs" | "redwoodjs" | "hugo" | "jekyll" | "brunch" | "middleman" | "zola" | "hydrogen" | "vite" | "vitepress" | "vuepress" | "parcel" | "fasthtml" | "sanity-v3" | "sanity" | "storybook"
```

----------------------------------------

TITLE: Creating a RequestBody1 Object in TypeScript
DESCRIPTION: This snippet demonstrates how to import the `RequestBody1` type and create an instance of it with example values for defining a DNS A record. It shows the required and optional fields.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/models/requestbody1.md#_snippet_0

LANGUAGE: typescript
CODE:
```
import { RequestBody1 } from "@vercel/sdk/models/createrecordop.js";

let value: RequestBody1 = {
  name: "subdomain",
  type: "A",
  ttl: 60,
  value: "192.0.2.42",
  comment: "used to verify ownership of domain",
};
```

----------------------------------------

TITLE: Instantiating CreateCheckRequestBody in TypeScript
DESCRIPTION: This snippet demonstrates how to create an instance of the `CreateCheckRequestBody` type, showing the required and optional fields with example values.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/models/createcheckrequestbody.md#_snippet_0

LANGUAGE: TypeScript
CODE:
```
import { CreateCheckRequestBody } from "@vercel/sdk/models/createcheckop.js";

let value: CreateCheckRequestBody = {
  name: "Performance Check",
  path: "/",
  blocking: true,
  detailsUrl: "http://example.com",
  externalId: "1234abc",
  rerequestable: true,
};
```

----------------------------------------

TITLE: Possible Values for CreateRecordRequestBodyDnsRequest8Type in TypeScript
DESCRIPTION: Defines the union of string literal types that constitute the `CreateRecordRequestBodyDnsRequest8Type`. These represent the valid DNS record types supported by the Vercel SDK operation.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/models/createrecordrequestbodydnsrequest8type.md#_snippet_1

LANGUAGE: typescript
CODE:
```
"A" | "AAAA" | "ALIAS" | "CAA" | "CNAME" | "HTTPS" | "MX" | "SRV" | "TXT" | "NS"
```

----------------------------------------

TITLE: List Team Members using Standalone Function (TypeScript)
DESCRIPTION: Illustrates the usage of the standalone `teamsGetTeamMembers` function with `VercelCore` for potentially better tree-shaking. It shows how to initialize `VercelCore`, pass the instance and parameters to the function, and handle the result including error checking.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/teams/README.md#_snippet_1

LANGUAGE: typescript
CODE:
```
import { VercelCore } from "@vercel/sdk/core.js";
import { teamsGetTeamMembers } from "@vercel/sdk/funcs/teamsGetTeamMembers.js";

// Use `VercelCore` for best tree-shaking performance.
// You can create one instance of it to use across an application.
const vercel = new VercelCore({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const res = await teamsGetTeamMembers(vercel, {
    limit: 20,
    since: 1540095775951,
    until: 1540095775951,
    role: "OWNER",
  });

  if (!res.ok) {
    throw res.error;
  }

  const { value: result } = res;

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Create Custom Environment using Vercel SDK (TypeScript)
DESCRIPTION: Demonstrates how to create a custom environment for a Vercel project by instantiating the Vercel class and calling the createCustomEnvironment method. Requires a bearer token and project/team details as parameters.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/environment/README.md#_snippet_0

LANGUAGE: TypeScript
CODE:
```
import { Vercel } from "@vercel/sdk";

const vercel = new Vercel({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const result = await vercel.environment.createCustomEnvironment({
    idOrName: "<value>",
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
    slug: "my-team-url-slug",
  });

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Creating SubmitBillingDataRequestBody Instance (TypeScript)
DESCRIPTION: This snippet shows how to import and instantiate the `SubmitBillingDataRequestBody` type from the Vercel SDK, populating it with example data for timestamps, periods, billing items, and usage details. It demonstrates the nested structure required for the request body.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/models/submitbillingdatarequestbody.md#_snippet_0

LANGUAGE: typescript
CODE:
```
import { SubmitBillingDataRequestBody } from "@vercel/sdk/models/submitbillingdataop.js";

let value: SubmitBillingDataRequestBody = {
  timestamp: new Date("2023-04-30T22:37:34.257Z"),
  eod: new Date("2025-06-29T07:32:07.538Z"),
  period: {
    start: new Date("2024-09-03T00:54:32.604Z"),
    end: new Date("2024-05-12T10:42:22.084Z"),
  },
  billing: {
    items: [
      {
        billingPlanId: "<id>",
        name: "<value>",
        price: "530.99",
        quantity: 1553.68,
        units: "<value>",
        total: "<value>",
      },
    ],
  },
  usage: [
    {
      name: "<value>",
      type: "interval",
      units: "<value>",
      dayValue: 4779.38,
      periodValue: 7753.8,
    },
  ],
};
```

----------------------------------------

TITLE: Creating Vercel Access Group Project using Vercel SDK (Standalone Function) - TypeScript
DESCRIPTION: Illustrates the use of the standalone `accessGroupsCreateAccessGroupProject` function for creating an access group project. This approach is recommended for better tree-shaking performance. It requires an instance of `VercelCore` and includes error handling for the API response.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/accessgroups/README.md#_snippet_15

LANGUAGE: typescript
CODE:
```
import { VercelCore } from "@vercel/sdk/core.js";
import { accessGroupsCreateAccessGroupProject } from "@vercel/sdk/funcs/accessGroupsCreateAccessGroupProject.js";

// Use `VercelCore` for best tree-shaking performance.
// You can create one instance of it to use across an application.
const vercel = new VercelCore({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const res = await accessGroupsCreateAccessGroupProject(vercel, {
    accessGroupIdOrName: "<value>",
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
    slug: "my-team-url-slug",
    requestBody: {
      projectId: "prj_ndlgr43fadlPyCtREAqxxdyFK",
      role: "ADMIN",
    },
  });

  if (!res.ok) {
    throw res.error;
  }

  const { value: result } = res;

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Delete Domain using Standalone Function - TypeScript
DESCRIPTION: This snippet shows how to use the standalone `domainsDeleteDomain` function for deleting a domain. This approach is recommended for better tree-shaking performance. It requires importing `VercelCore` and the specific function. The function takes a `VercelCore` instance and an options object containing the domain name and optional team details.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/domains/README.md#_snippet_19

LANGUAGE: typescript
CODE:
```
import { VercelCore } from "@vercel/sdk/core.js";
import { domainsDeleteDomain } from "@vercel/sdk/funcs/domainsDeleteDomain.js";

// Use `VercelCore` for best tree-shaking performance.
// You can create one instance of it to use across an application.
const vercel = new VercelCore({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const res = await domainsDeleteDomain(vercel, {
    domain: "example.com",
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
    slug: "my-team-url-slug",
  });

  if (!res.ok) {
    throw res.error;
  }

  const { value: result } = res;

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Getting Marketplace Member using Vercel SDK (TypeScript)
DESCRIPTION: Demonstrates how to use the main Vercel class to fetch details for a marketplace member. Requires a bearer token for authentication and the integration configuration and member IDs.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/marketplace/README.md#_snippet_2

LANGUAGE: typescript
CODE:
```
import { Vercel } from "@vercel/sdk";

const vercel = new Vercel({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const result = await vercel.marketplace.getMember({
    integrationConfigurationId: "<id>",
    memberId: "<id>",
  });

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Delete Deployment using Vercel Class (TypeScript)
DESCRIPTION: Demonstrates how to delete a Vercel deployment by instantiating the Vercel class with a bearer token and calling the deployments.deleteDeployment method. The deployment can be identified by its 'id' or 'url', optionally specifying 'teamId' or 'slug'.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/deployments/README.md#_snippet_18

LANGUAGE: TypeScript
CODE:
```
import { Vercel } from "@vercel/sdk";

const vercel = new Vercel({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const result = await vercel.deployments.deleteDeployment({
    id: "dpl_5WJWYSyB7BpgTj3EuwF37WMRBXBtPQ2iTMJHJBJyRfd",
    url: "https://files-orcin-xi.vercel.app/",
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
    slug: "my-team-url-slug",
  });

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Accepting Project Transfer Request with Standalone Function (TypeScript)
DESCRIPTION: This example shows how to use the standalone `projectsAcceptProjectTransferRequest` function, which is suitable for better tree-shaking. It takes a `VercelCore` instance and an options object containing the transfer `code`, `teamId`, `slug`, and optional `requestBody`. The code includes basic error handling and logs the successful result.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/projects/README.md#_snippet_37

LANGUAGE: typescript
CODE:
```
import { VercelCore } from "@vercel/sdk/core.js";
import { projectsAcceptProjectTransferRequest } from "@vercel/sdk/funcs/projectsAcceptProjectTransferRequest.js";

// Use `VercelCore` for best tree-shaking performance.
// You can create one instance of it to use across an application.
const vercel = new VercelCore({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const res = await projectsAcceptProjectTransferRequest(vercel, {
    code: "<value>",
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
    slug: "my-team-url-slug",
    requestBody: {
      newProjectName: "a-project-name",
    },
  });

  if (!res.ok) {
    throw res.error;
  }

  const { value: result } = res;

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Get Vercel Configurations using Standalone Function (TypeScript)
DESCRIPTION: Illustrates how to fetch integration configurations using the standalone `integrationsGetConfigurations` function from the Vercel SDK core. It requires an instance of `VercelCore` for tree-shaking benefits and demonstrates handling the result, including error checking.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/integrations/README.md#_snippet_3

LANGUAGE: typescript
CODE:
```
import { VercelCore } from "@vercel/sdk/core.js";
import { integrationsGetConfigurations } from "@vercel/sdk/funcs/integrationsGetConfigurations.js";

// Use `VercelCore` for best tree-shaking performance.
// You can create one instance of it to use across an application.
const vercel = new VercelCore({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const res = await integrationsGetConfigurations(vercel, {
    view: "account",
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
    slug: "my-team-url-slug",
  });

  if (!res.ok) {
    throw res.error;
  }

  const { value: result } = res;

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Removing Custom Environment using Vercel SDK Class (TypeScript)
DESCRIPTION: Demonstrates how to remove a custom environment for a Vercel project using an instance of the main `Vercel` class. It requires specifying the project identifier, environment identifier, and optionally team details.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/environment/README.md#_snippet_8

LANGUAGE: typescript
CODE:
```
import { Vercel } from "@vercel/sdk";

const vercel = new Vercel({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const result = await vercel.environment.removeCustomEnvironment({
    idOrName: "<value>",
    environmentSlugOrId: "<id>",
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
    slug: "my-team-url-slug",
  });

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Updating Vercel Project using Standalone Function (TypeScript)
DESCRIPTION: Shows how to update a Vercel project using the standalone `projectsUpdateProject` function. It utilizes a `VercelCore` instance for potential tree-shaking benefits and includes basic error handling for the function's result.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/projects/README.md#_snippet_7

LANGUAGE: typescript
CODE:
```
import { VercelCore } from "@vercel/sdk/core.js";
import { projectsUpdateProject } from "@vercel/sdk/funcs/projectsUpdateProject.js";

// Use `VercelCore` for best tree-shaking performance.
// You can create one instance of it to use across an application.
const vercel = new VercelCore({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const res = await projectsUpdateProject(vercel, {
    idOrName: "prj_12HKQaOmR5t5Uy6vdcQsNIiZgHGB",
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
    slug: "my-team-url-slug",
    requestBody: {
      name: "a-project-name",
    },
  });

  if (!res.ok) {
    throw res.error;
  }

  const { value: result } = res;

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Getting Marketplace Member using Standalone Function (TypeScript)
DESCRIPTION: Illustrates fetching marketplace member details using the standalone marketplaceGetMember function with VercelCore for improved tree-shaking. Requires a VercelCore instance and the integration configuration and member IDs.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/marketplace/README.md#_snippet_3

LANGUAGE: typescript
CODE:
```
import { VercelCore } from "@vercel/sdk/core.js";
import { marketplaceGetMember } from "@vercel/sdk/funcs/marketplaceGetMember.js";

// Use `VercelCore` for best tree-shaking performance.
// You can create one instance of it to use across an application.
const vercel = new VercelCore({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const res = await marketplaceGetMember(vercel, {
    integrationConfigurationId: "<id>",
    memberId: "<id>",
  });

  if (!res.ok) {
    throw res.error;
  }

  const { value: result } = res;

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Deleting Edge Config Tokens using Standalone Function (TypeScript)
DESCRIPTION: This snippet shows how to perform the same operation using the standalone `edgeConfigDeleteEdgeConfigTokens` function from the `@vercel/sdk/funcs` module. It takes a `VercelCore` instance and the parameters as arguments, offering better tree-shaking performance. It also includes basic error handling.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/edgeconfig/README.md#_snippet_23

LANGUAGE: typescript
CODE:
```
import { VercelCore } from "@vercel/sdk/core.js";
import { edgeConfigDeleteEdgeConfigTokens } from "@vercel/sdk/funcs/edgeConfigDeleteEdgeConfigTokens.js";

// Use `VercelCore` for best tree-shaking performance.
// You can create one instance of it to use across an application.
const vercel = new VercelCore({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const res = await edgeConfigDeleteEdgeConfigTokens(vercel, {
    edgeConfigId: "<id>",
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
    slug: "my-team-url-slug",
    requestBody: {
      tokens: [],
    },
  });

  if (!res.ok) {
    throw res.error;
  }

  const { value: result } = res;

  
}

run();
```

----------------------------------------

TITLE: Instantiating RequestBody6 Type in TypeScript
DESCRIPTION: This snippet demonstrates how to create an instance of the RequestBody6 type, which is used to specify the details for a new DNS record, including its name, type, TTL, value, MX priority, and an optional comment.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/models/requestbody6.md#_snippet_0

LANGUAGE: typescript
CODE:
```
import { RequestBody6 } from "@vercel/sdk/models/createrecordop.js";

let value: RequestBody6 = {
  name: "subdomain",
  type: "AAAA",
  ttl: 60,
  value: "10 mail.example.com.",
  mxPriority: 10,
  comment: "used to verify ownership of domain",
};
```

----------------------------------------

TITLE: Initializing Srv Type in TypeScript
DESCRIPTION: This snippet demonstrates how to create an instance of the Srv type, which is used for SRV DNS record updates. It shows the required fields: target, weight, port, and priority, and assigns example values.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/models/srv.md#_snippet_0

LANGUAGE: TypeScript
CODE:
```
import { Srv } from "@vercel/sdk/models/updaterecordop.js";

let value: Srv = {
  target: "example2.com.",
  weight: 60512,
  port: 60544,
  priority: 101861,
};
```

----------------------------------------

TITLE: Get Cert By ID using Vercel SDK (TypeScript)
DESCRIPTION: Demonstrates how to retrieve a certificate by its ID using the main `Vercel` class from the Vercel SDK. It shows initialization with a bearer token and calling the `certs.getCertById` method with required parameters like `id`, `teamId`, and `slug`.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/certs/README.md#_snippet_0

LANGUAGE: typescript
CODE:
```
import { Vercel } from "@vercel/sdk";

const vercel = new Vercel({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const result = await vercel.certs.getCertById({
    id: "<id>",
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
    slug: "my-team-url-slug",
  });

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Creating a Vercel Webhook using the SDK Class (TypeScript)
DESCRIPTION: This snippet demonstrates how to create a Vercel webhook using the main `Vercel` class from the `@vercel/sdk`. It initializes the SDK with a bearer token and calls the `webhooks.createWebhook` method with team ID, slug, and webhook details (URL and events).
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/webhooks/README.md#_snippet_0

LANGUAGE: typescript
CODE:
```
import { Vercel } from "@vercel/sdk";

const vercel = new Vercel({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const result = await vercel.webhooks.createWebhook({
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
    slug: "my-team-url-slug",
    requestBody: {
      url: "https://woeful-yin.biz",
      events: [],
    },
  });

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Listing Valid RecordType Values in TypeScript
DESCRIPTION: Provides a list of all valid string values that the RecordType type can represent, corresponding to standard DNS record types like A, AAAA, CNAME, MX, etc.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/models/recordtype.md#_snippet_1

LANGUAGE: typescript
CODE:
```
"A" | "AAAA" | "ALIAS" | "CAA" | "CNAME" | "HTTPS" | "MX" | "SRV" | "TXT" | "NS"
```

----------------------------------------

TITLE: Getting Invoice Details with Vercel Class - TypeScript
DESCRIPTION: This snippet demonstrates how to fetch invoice details using the main `Vercel` class instance. It requires initializing the SDK with a bearer token and calling the `marketplace.getInvoice` method with the integration configuration ID and invoice ID.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/marketplace/README.md#_snippet_10

LANGUAGE: typescript
CODE:
```
import { Vercel } from "@vercel/sdk";

const vercel = new Vercel({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const result = await vercel.marketplace.getInvoice({
    integrationConfigurationId: "<id>",
    invoiceId: "<id>",
  });

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: General Pattern for Calling Standalone Functions (TypeScript)
DESCRIPTION: Illustrates the common pattern for calling a Vercel SDK standalone function and handling its `Result<Value, Error>` return type. It shows how to check the `result.ok` property to determine success or failure and access either the `result.value` or `result.error`.
SOURCE: https://github.com/vercel/sdk/blob/main/FUNCTIONS.md#_snippet_1

LANGUAGE: typescript
CODE:
```
import { Core } from "<sdk-package-name>";
import { fetchSomething } from "<sdk-package-name>/funcs/fetchSomething.js";

const client = new Core();

async function run() {
  const result = await fetchSomething(client, { id: "123" });
  if (!result.ok) {
    // You can throw the error or handle it. It's your choice now.
    throw result.error;
  }

  console.log(result.value);
}

run();
```

----------------------------------------

TITLE: Using Vercel SDK Class to Get Edge Config Items (TypeScript)
DESCRIPTION: Demonstrates how to use the main `Vercel` class from the SDK to fetch items from a Vercel Edge Config. It requires a bearer token for authentication and specifies the Edge Config ID, team ID, or slug.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/edgeconfig/README.md#_snippet_10

LANGUAGE: typescript
CODE:
```
import { Vercel } from "@vercel/sdk";

const vercel = new Vercel({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const result = await vercel.edgeConfig.getEdgeConfigItems({
    edgeConfigId: "<id>",
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
    slug: "my-team-url-slug",
  });

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Creating a Vercel Webhook using a Standalone Function (TypeScript)
DESCRIPTION: This snippet shows how to create a Vercel webhook using the standalone `webhooksCreateWebhook` function from `@vercel/sdk/funcs`. It utilizes `VercelCore` for potentially better tree-shaking and passes the core instance along with the webhook details to the function. It also includes basic error handling.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/webhooks/README.md#_snippet_1

LANGUAGE: typescript
CODE:
```
import { VercelCore } from "@vercel/sdk/core.js";
import { webhooksCreateWebhook } from "@vercel/sdk/funcs/webhooksCreateWebhook.js";

// Use `VercelCore` for best tree-shaking performance.
// You can create one instance of it to use across an application.
const vercel = new VercelCore({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const res = await webhooksCreateWebhook(vercel, {
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
    slug: "my-team-url-slug",
    requestBody: {
      url: "https://woeful-yin.biz",
      events: [],
    },
  });

  if (!res.ok) {
    throw res.error;
}

  const { value: result } = res;

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Checking Team Access Request Status with Standalone Function
DESCRIPTION: Illustrates using the standalone `teamsGetTeamAccessRequest` function with `VercelCore` for improved tree-shaking. Includes error handling for the API response.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/teams/README.md#_snippet_7

LANGUAGE: typescript
CODE:
```
import { VercelCore } from "@vercel/sdk/core.js";
import { teamsGetTeamAccessRequest } from "@vercel/sdk/funcs/teamsGetTeamAccessRequest.js";

// Use `VercelCore` for best tree-shaking performance.
// You can create one instance of it to use across an application.
const vercel = new VercelCore({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const res = await teamsGetTeamAccessRequest(vercel, {
    userId: "<id>",
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
  });

  if (!res.ok) {
    throw res.error;
  }

  const { value: result } = res;

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Initializing GitRepo2 (GitHub) - TypeScript
DESCRIPTION: Shows how to create an instance of the `models.GitRepo2` type, used for GitHub repositories, including properties like `org`, `repo`, and `repoId`.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/models/gitrepo.md#_snippet_1

LANGUAGE: typescript
CODE:
```
const value: models.GitRepo2 = {
  org: "<value>",
  repo: "<value>",
  repoId: 2474.39,
  type: "github",
  repoOwnerId: 2550.62,
  path: "/usr/src",
  defaultBranch: "<value>",
  name: "<value>",
  private: false,
  ownerType: "user"
};
```

----------------------------------------

TITLE: Initializing Vercel EditProjectEnvRequest in TypeScript
DESCRIPTION: This snippet demonstrates how to create an instance of the `EditProjectEnvRequest` type from the Vercel SDK. It shows the structure and example values for the properties required to specify which project and environment variable to edit, along with the new configuration details.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/models/editprojectenvrequest.md#_snippet_0

LANGUAGE: typescript
CODE:
```
import { EditProjectEnvRequest } from "@vercel/sdk/models/editprojectenvop.js";

let value: EditProjectEnvRequest = {
  idOrName: "prj_XLKmu1DyR1eY7zq8UgeRKbA7yVLA",
  id: "XMbOEya1gUUO1ir4",
  teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
  slug: "my-team-url-slug",
  requestBody: {
    key: "GITHUB_APP_ID",
    target: [
      "preview"
    ],
    gitBranch: "feature-1",
    type: "plain",
    value: "bkWIjbnxcvo78",
    customEnvironmentIds: [
      "env_1234567890"
    ],
    comment: "database connection string for production"
  }
};
```

----------------------------------------

TITLE: Updating Vercel Check using Standalone Function (TypeScript)
DESCRIPTION: Update an existing check using the standalone `checksUpdateCheck` function for better tree-shaking. This method requires passing an instance of `VercelCore` and the update parameters. The example includes error handling.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/checks/README.md#_snippet_7

LANGUAGE: typescript
CODE:
```
import { VercelCore } from "@vercel/sdk/core.js";
import { checksUpdateCheck } from "@vercel/sdk/funcs/checksUpdateCheck.js";

// Use `VercelCore` for best tree-shaking performance.
// You can create one instance of it to use across an application.
const vercel = new VercelCore({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const res = await checksUpdateCheck(vercel, {
    deploymentId: "dpl_2qn7PZrx89yxY34vEZPD31Y9XVj6",
    checkId: "check_2qn7PZrx89yxY34vEZPD31Y9XVj6",
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
    slug: "my-team-url-slug",
    requestBody: {
      name: "Performance Check",
      path: "/",
      detailsUrl: "https://example.com/check/run/1234abc",
      output: {
        metrics: {
          fcp: {
            value: 1200,
            previousValue: 900,
            source: "web-vitals"
          },
          lcp: {
            value: 1200,
            previousValue: 1000,
            source: "web-vitals"
          },
          cls: {
            value: 4,
            previousValue: 2,
            source: "web-vitals"
          },
          tbt: {
            value: 3000,
            previousValue: 3500,
            source: "web-vitals"
          },
          virtualExperienceScore: {
            value: 30,
            previousValue: 35,
            source: "web-vitals"
          }
        }
      },
      externalId: "1234abc"
    }
  });

  if (!res.ok) {
    throw res.error;
  }

  const { value: result } = res;

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Creating an InlinedFile Model (TypeScript)
DESCRIPTION: Demonstrates how to create an instance of the `models.InlinedFile` type. This model represents a file whose content is directly included as data.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/models/files.md#_snippet_0

LANGUAGE: typescript
CODE:
```
const value: models.InlinedFile = {
  data: "<value>",
  file: "folder/file.js"
};
```

----------------------------------------

TITLE: Adding Bypass IP Rule using Vercel SDK Class (TypeScript)
DESCRIPTION: This snippet demonstrates how to use the `addBypassIp` method via an instance of the `Vercel` class from the `@vercel/sdk`. It shows initializing the SDK with a bearer token and calling the method with project and team identifiers.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/security/README.md#_snippet_12

LANGUAGE: typescript
CODE:
```
import { Vercel } from "@vercel/sdk";

const vercel = new Vercel({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const result = await vercel.security.addBypassIp({
    projectId: "<id>",
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
    slug: "my-team-url-slug",
  });

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Deleting Team Invite Code using Standalone Function (TypeScript)
DESCRIPTION: Shows how to delete a team invite code using the standalone `teamsDeleteTeamInviteCode` function from `@vercel/sdk/funcs`. This approach uses `VercelCore` for better tree-shaking and requires passing the `VercelCore` instance and the parameters (`inviteId`, `teamId`) to the function. Includes basic error handling.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/teams/README.md#_snippet_25

LANGUAGE: typescript
CODE:
```
import { VercelCore } from "@vercel/sdk/core.js";
import { teamsDeleteTeamInviteCode } from "@vercel/sdk/funcs/teamsDeleteTeamInviteCode.js";

// Use \`VercelCore\` for best tree-shaking performance.
// You can create one instance of it to use across an application.
const vercel = new VercelCore({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const res = await teamsDeleteTeamInviteCode(vercel, {
    inviteId: "2wn2hudbr4chb1ecywo9dvzo7g9sscs6mzcz8htdde0txyom4l",
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
  });

  if (!res.ok) {
    throw res.error;
  }

  const { value: result } = res;

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Deleting Vercel Webhook using Standalone Function (TypeScript)
DESCRIPTION: This snippet demonstrates deleting a Vercel webhook using the standalone webhooksDeleteWebhook function. This approach is recommended for better tree-shaking performance and requires an instance of VercelCore and the webhook details (ID, team ID, slug). It also includes basic error handling.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/webhooks/README.md#_snippet_7

LANGUAGE: typescript
CODE:
```
import { VercelCore } from "@vercel/sdk/core.js";
import { webhooksDeleteWebhook } from "@vercel/sdk/funcs/webhooksDeleteWebhook.js";

// Use `VercelCore` for best tree-shaking performance.
// You can create one instance of it to use across an application.
const vercel = new VercelCore({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const res = await webhooksDeleteWebhook(vercel, {
    id: "<id>",
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
    slug: "my-team-url-slug",
  });

  if (!res.ok) {
    throw res.error;
  }

  const { value: result } = res;

  
}

run();
```

----------------------------------------

TITLE: Example Usage of UpdateResourceSecretsRequestBody in TypeScript
DESCRIPTION: This snippet demonstrates how to create an instance of the UpdateResourceSecretsRequestBody object in TypeScript, showing the structure for the 'secrets' array with 'name' and 'value' properties.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/models/updateresourcesecretsrequestbody.md#_snippet_0

LANGUAGE: typescript
CODE:
```
import { UpdateResourceSecretsRequestBody } from "@vercel/sdk/models/updateresourcesecretsop.js";

let value: UpdateResourceSecretsRequestBody = {
  secrets: [
    {
      name: "<value>",
      value: "<value>",
    },
  ],
};
```

----------------------------------------

TITLE: Initializing models.GetActiveAttackStatusResponseBody2 Object with Example Data (TypeScript)
DESCRIPTION: Initializes a variable named `value` with an object conforming to the `models.GetActiveAttackStatusResponseBody2` type. This example includes sample data for the `anomalies` array, demonstrating the expected structure and types of nested properties like `ownerId`, `projectId`, `startTime`, `endTime`, `atMinute`, and `affectedHostMap`.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/models/getactiveattackstatusresponsebody.md#_snippet_1

LANGUAGE: typescript
CODE:
```
const value: models.GetActiveAttackStatusResponseBody2 = {
  anomalies: [
    {
      ownerId: "<id>",
      projectId: "<id>",
      startTime: 259.76,
      endTime: 4119.11,
      atMinute: 9166.33,
      affectedHostMap: {
        "key": {},
      },
    },
  ],
};
```

----------------------------------------

TITLE: Download Artifact using Standalone Function - TypeScript
DESCRIPTION: Shows how to download a cache artifact using the standalone `artifactsDownloadArtifact` function. This approach is recommended for better tree-shaking performance and requires importing from `@vercel/sdk/core.js` and `@vercel/sdk/funcs/artifactsDownloadArtifact.js`. It takes a `VercelCore` instance and an options object with parameters like `hash`, `teamId`, `slug`, `xArtifactClientCi`, and `xArtifactClientInteractive`. The function returns a result object indicating success (`ok`) or failure (`error`), with the downloaded artifact data in the `value` property on success.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/artifacts/README.md#_snippet_7

LANGUAGE: typescript
CODE:
```
import { VercelCore } from "@vercel/sdk/core.js";
import { artifactsDownloadArtifact } from "@vercel/sdk/funcs/artifactsDownloadArtifact.js";

// Use `VercelCore` for best tree-shaking performance.
// You can create one instance of it to use across an application.
const vercel = new VercelCore({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const res = await artifactsDownloadArtifact(vercel, {
    xArtifactClientCi: "VERCEL",
    xArtifactClientInteractive: 0,
    hash: "12HKQaOmR5t5Uy6vdcQsNIiZgHGB",
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
    slug: "my-team-url-slug",
  });

  if (!res.ok) {
    throw res.error;
  }

  const { value: result } = res;

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Instantiating PutFirewallConfigSecurityRules in TypeScript
DESCRIPTION: This snippet demonstrates how to create an instance of the PutFirewallConfigSecurityRules type in TypeScript. It shows the required fields and provides example values for each property, including nested structures like conditionGroup and conditions. It requires importing the type from the @vercel/sdk/models module.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/models/putfirewallconfigsecurityrules.md#_snippet_0

LANGUAGE: typescript
CODE:
```
import { PutFirewallConfigSecurityRules } from "@vercel/sdk/models/putfirewallconfigop.js";

let value: PutFirewallConfigSecurityRules = {
  id: "<id>",
  name: "<value>",
  active: false,
  conditionGroup: [
    {
      conditions: [
        {
          type: "host",
          op: "neq"
        }
      ]
    }
  ],
  action: {}
};
```

----------------------------------------

TITLE: Updating Firewall Config using Vercel SDK Instance - TypeScript
DESCRIPTION: Shows how to update the firewall configuration for a Vercel project using the `vercel.security.updateFirewallConfig` method on a `Vercel` SDK instance. It demonstrates setting a managed rule to active.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/security/README.md#_snippet_4

LANGUAGE: typescript
CODE:
```
import { Vercel } from "@vercel/sdk";

const vercel = new Vercel({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const result = await vercel.security.updateFirewallConfig({
    projectId: "<id>",
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
    slug: "my-team-url-slug",
    requestBody: {
      action: "managedRules.update",
      id: "<id>",
      value: {
        active: true,
      },
    },
  });

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Example Usage of GetDomainsResponseBody in TypeScript
DESCRIPTION: This snippet demonstrates how to instantiate and populate a `GetDomainsResponseBody` object in TypeScript, showing the expected structure for a successful response when retrieving a list of domains using the Vercel SDK. It includes examples of nested objects like `domains` and `pagination`.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/models/getdomainsresponsebody.md#_snippet_0

LANGUAGE: TypeScript
CODE:
```
import { GetDomainsResponseBody } from "@vercel/sdk/models/getdomainsop.js";

let value: GetDomainsResponseBody = {
  domains: [
    {
      verified: true,
      nameservers: [
        "ns1.nameserver.net",
        "ns2.nameserver.net"
      ],
      intendedNameservers: [
        "ns1.vercel-dns.com",
        "ns2.vercel-dns.com"
      ],
      customNameservers: [
        "ns1.nameserver.net",
        "ns2.nameserver.net"
      ],
      creator: {
        username: "vercel_user",
        email: "demo@example.com",
        id: "ZspSRT4ljIEEmMHgoDwKWDei"
      },
      teamId: "<id>",
      createdAt: 1613602938882,
      boughtAt: 1613602938882,
      expiresAt: 1613602938882,
      id: "EmTbe5CEJyTk2yVAHBUWy4A3sRusca3GCwRjTC1bpeVnt1",
      name: "example.com",
      orderedAt: 1613602938882,
      renew: true,
      serviceType: "zeit.world",
      transferredAt: 1613602938882,
      transferStartedAt: 1613602938882,
      userId: "<id>"
    }
  ],
  pagination: {
    count: 20,
    next: 1540095775951,
    prev: 1540095775951
  }
};
```

----------------------------------------

TITLE: Fetching Webhooks using Standalone Function (TypeScript)
DESCRIPTION: This example demonstrates fetching webhooks using the standalone `webhooksGetWebhooks` function with a `VercelCore` instance. This approach is recommended for better tree-shaking. It includes basic error handling and logs the successful result.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/webhooks/README.md#_snippet_3

LANGUAGE: typescript
CODE:
```
import { VercelCore } from "@vercel/sdk/core.js";
import { webhooksGetWebhooks } from "@vercel/sdk/funcs/webhooksGetWebhooks.js";

// Use `VercelCore` for best tree-shaking performance.
// You can create one instance of it to use across an application.
const vercel = new VercelCore({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const res = await webhooksGetWebhooks(vercel, {
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
    slug: "my-team-url-slug",
  });

  if (!res.ok) {
    throw res.error;
  }

  const { value: result } = res;

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Initializing a StatusRequest Object in TypeScript
DESCRIPTION: This snippet demonstrates how to import the StatusRequest type and create an instance of it, populating the 'teamId' and 'slug' fields. It shows the basic structure for providing team context for status operations.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/models/statusrequest.md#_snippet_0

LANGUAGE: TypeScript
CODE:
```
import { StatusRequest } from "@vercel/sdk/models/statusop.js";

let value: StatusRequest = {
  teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
  slug: "my-team-url-slug"
};
```

----------------------------------------

TITLE: Creating a PatchTeamRequest object in TypeScript
DESCRIPTION: This snippet shows how to create an instance of the PatchTeamRequest type in TypeScript, populating its required 'teamId' and 'requestBody' fields, as well as the optional 'slug'. It demonstrates the structure of the request body with various properties for updating team settings.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/models/patchteamrequest.md#_snippet_0

LANGUAGE: TypeScript
CODE:
```
import { PatchTeamRequest } from "@vercel/sdk/models/patchteamop.js";

let value: PatchTeamRequest = {
  teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
  slug: "my-team-url-slug",
  requestBody: {
    description:
      "Our mission is to make cloud computing accessible to everyone",
    emailDomain: "example.com",
    name: "My Team",
    previewDeploymentSuffix: "example.dev",
    regenerateInviteCode: true,
    saml: {
      enforced: true,
    },
    slug: "my-team",
    enablePreviewFeedback: "on",
    enableProductionFeedback: "on",
    sensitiveEnvironmentVariablePolicy: "on",
    remoteCaching: {
      enabled: true,
    },
    hideIpAddresses: false,
    hideIpAddressesInLogDrains: false,
  },
};
```

----------------------------------------

TITLE: Example of CreateProjectEnvRequestBody1 in TypeScript
DESCRIPTION: Demonstrates the structure and required fields for a single CreateProjectEnvRequestBody1 object, used to define a single environment variable for a Vercel project.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/models/createprojectenvrequestbody.md#_snippet_0

LANGUAGE: typescript
CODE:
```
const value: models.CreateProjectEnvRequestBody1 = {
  key: "API_URL",
  value: "https://api.vercel.com",
  type: "plain",
  target: [
    "preview",
  ],
  gitBranch: "feature-1",
  comment: "database connection string for production",
  customEnvironmentIds: [
    "env_1234567890",
  ],
};
```

----------------------------------------

TITLE: Creating Edge Config Token using Standalone Function (TypeScript)
DESCRIPTION: This snippet shows how to create an Edge Config token using the standalone `edgeConfigCreateEdgeConfigToken` function from the Vercel SDK core. It utilizes a `VercelCore` instance for better tree-shaking and requires the Edge Config ID, optional team ID/slug, and a label for the token. It also includes basic error handling.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/edgeconfig/README.md#_snippet_27

LANGUAGE: TypeScript
CODE:
```
import { VercelCore } from "@vercel/sdk/core.js";
import { edgeConfigCreateEdgeConfigToken } from "@vercel/sdk/funcs/edgeConfigCreateEdgeConfigToken.js";

// Use `VercelCore` for best tree-shaking performance.
// You can create one instance of it to use across an application.
const vercel = new VercelCore({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const res = await edgeConfigCreateEdgeConfigToken(vercel, {
    edgeConfigId: "<id>",
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
    slug: "my-team-url-slug",
    requestBody: {
      label: "<value>",
    },
  });

  if (!res.ok) {
    throw res.error;
  }

  const { value: result } = res;

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Update Firewall Config - Insert Rule (Type 2)
DESCRIPTION: This request body type is used to insert a new firewall rule. It includes the 'rules.insert' action and a detailed structure for the new rule, including name, active status, condition groups, and action.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/models/updatefirewallconfigrequestbody.md#_snippet_1

LANGUAGE: typescript
CODE:
```
const value: models.UpdateFirewallConfigRequestBody2 = {
  action: "rules.insert",
  value: {
    name: "<value>",
    active: false,
    conditionGroup: [
      {
        conditions: [
          {
            type: "cookie",
            op: "lte",
          },
        ],
      },
    ],
    action: {},
  },
};
```

----------------------------------------

TITLE: Checking Domain Status using Vercel Class (TypeScript)
DESCRIPTION: This snippet shows how to initialize the Vercel SDK using the main `Vercel` class and call the `domains.checkDomainStatus` method to check the status of a domain. It requires a bearer token for authentication and demonstrates handling the asynchronous result.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/domains/README.md#_snippet_4

LANGUAGE: typescript
CODE:
```
import { Vercel } from "@vercel/sdk";

const vercel = new Vercel({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const result = await vercel.domains.checkDomainStatus({
    name: "example.com",
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
    slug: "my-team-url-slug",
  });

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Download Artifact using Vercel Class - TypeScript
DESCRIPTION: Demonstrates how to download a cache artifact using the `downloadArtifact` method on an instance of the `Vercel` class. Requires the `@vercel/sdk` package. The method takes parameters like `hash`, `teamId`, `slug`, `xArtifactClientCi`, and `xArtifactClientInteractive` to identify and configure the download request. The result is an object containing the downloaded artifact data or information.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/artifacts/README.md#_snippet_6

LANGUAGE: typescript
CODE:
```
import { Vercel } from "@vercel/sdk";

const vercel = new Vercel({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const result = await vercel.artifacts.downloadArtifact({
    xArtifactClientCi: "VERCEL",
    xArtifactClientInteractive: 0,
    hash: "12HKQaOmR5t5Uy6vdcQsNIiZgHGB",
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
    slug: "my-team-url-slug",
  });

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Promoting Deployment using Vercel SDK Instance (TypeScript)
DESCRIPTION: Demonstrates how to use the `requestPromote` method on a `Vercel` instance to promote a deployment to production. This method requires the `projectId` and `deploymentId`, and optionally accepts `teamId` or `slug`. Note that this operation does not rebuild the deployment.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/projects/README.md#_snippet_40

LANGUAGE: typescript
CODE:
```
import { Vercel } from "@vercel/sdk";

const vercel = new Vercel({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  await vercel.projects.requestPromote({
    projectId: "<id>",
    deploymentId: "<id>",
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
    slug: "my-team-url-slug",
  });


}

run();
```

----------------------------------------

TITLE: Removing Project Domain using Standalone Function (TypeScript)
DESCRIPTION: This snippet shows how to remove a domain using the standalone `projectsRemoveProjectDomain` function from the SDK's core module. This approach is recommended for better tree-shaking and uses a `VercelCore` instance. It includes error handling for the API call.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/projects/README.md#_snippet_17

LANGUAGE: typescript
CODE:
```
import { VercelCore } from "@vercel/sdk/core.js";
import { projectsRemoveProjectDomain } from "@vercel/sdk/funcs/projectsRemoveProjectDomain.js";

// Use `VercelCore` for best tree-shaking performance.
// You can create one instance of it to use across an application.
const vercel = new VercelCore({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const res = await projectsRemoveProjectDomain(vercel, {
    idOrName: "<value>",
    domain: "www.example.com",
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
    slug: "my-team-url-slug",
  });

  if (!res.ok) {
    throw res.error;
  }

  const { value: result } = res;

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Exchange SSO Token using Vercel SDK Class (TypeScript)
DESCRIPTION: Demonstrates how to use the `exchangeSsoToken` method available on the `marketplace` property of a `Vercel` instance. This method exchanges an OAuth authorization code for an OIDC token as part of the SSO flow. Requires the authorization code, client ID, and client secret.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/marketplace/README.md#_snippet_22

LANGUAGE: TypeScript
CODE:
```
import { Vercel } from "@vercel/sdk";

const vercel = new Vercel();

async function run() {
  const result = await vercel.marketplace.exchangeSsoToken({
    code: "<value>",
    clientId: "<id>",
    clientSecret: "<value>",
  });

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Instantiating GetCheckMetrics in TypeScript
DESCRIPTION: This snippet demonstrates how to create an object that conforms to the GetCheckMetrics type in TypeScript, showing the structure for required web vital metrics (fcp, lcp, cls, tbt) with example values and sources.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/models/getcheckmetrics.md#_snippet_0

LANGUAGE: typescript
CODE:
```
import { GetCheckMetrics } from "@vercel/sdk/models/getcheckop.js";

let value: GetCheckMetrics = {
  fcp: {
    value: 7476.5,
    source: "web-vitals",
  },
  lcp: {
    value: 8285.04,
    source: "web-vitals",
  },
  cls: {
    value: 6045.59,
    source: "web-vitals",
  },
  tbt: {
    value: 4162.19,
    source: "web-vitals",
  },
};
```

----------------------------------------

TITLE: Getting a Vercel Check using Standalone Function (TypeScript)
DESCRIPTION: This snippet shows how to fetch Vercel check details using the standalone `checksGetCheck` function from the core SDK. It utilizes a `VercelCore` instance for potential tree-shaking benefits and handles the result using a `res.ok` check.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/checks/README.md#_snippet_5

LANGUAGE: typescript
CODE:
```
import { VercelCore } from "@vercel/sdk/core.js";
import { checksGetCheck } from "@vercel/sdk/funcs/checksGetCheck.js";

// Use \`VercelCore\` for best tree-shaking performance.
// You can create one instance of it to use across an application.
const vercel = new VercelCore({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const res = await checksGetCheck(vercel, {
    deploymentId: "dpl_2qn7PZrx89yxY34vEZPD31Y9XVj6",
    checkId: "check_2qn7PZrx89yxY34vEZPD31Y9XVj6",
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
    slug: "my-team-url-slug",
  });

  if (!res.ok) {
    throw res.error;
  }

  const { value: result } = res;

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Listing Access Group Projects using Vercel SDK Instance (TypeScript)
DESCRIPTION: This snippet demonstrates how to list projects associated with a Vercel access group by calling the `listAccessGroupProjects` method on an initialized `Vercel` SDK instance. It requires a bearer token for authentication and accepts parameters to identify the access group and control pagination.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/accessgroups/README.md#_snippet_12

LANGUAGE: typescript
CODE:
```
import { Vercel } from "@vercel/sdk";

const vercel = new Vercel({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const result = await vercel.accessGroups.listAccessGroupProjects({
    idOrName: "ag_pavWOn1iLObbXLRiwVvzmPrTWyTf",
    limit: 20,
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
    slug: "my-team-url-slug",
  });

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Joining a Team using Vercel SDK Instance (TypeScript)
DESCRIPTION: This snippet demonstrates how to join a team using an instance of the `Vercel` class from the SDK. It shows initialization with a bearer token and calling the `teams.joinTeam` method with `teamId` and `inviteCode`.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/teams/README.md#_snippet_8

LANGUAGE: typescript
CODE:
```
import { Vercel } from "@vercel/sdk";

const vercel = new Vercel({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const result = await vercel.teams.joinTeam({
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
    requestBody: {
      inviteCode: "fisdh38aejkeivn34nslfore9vjtn4ls",
    },
  });

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Listing Deployments using Vercel Class Instance (TypeScript)
DESCRIPTION: This snippet demonstrates how to list deployments by instantiating the main `Vercel` class and calling the `deployments.getDeployments` method. It requires a bearer token for authentication and accepts various parameters to filter the results.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/deployments/README.md#_snippet_16

LANGUAGE: typescript
CODE:
```
import { Vercel } from "@vercel/sdk";

const vercel = new Vercel({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const result = await vercel.deployments.getDeployments({
    app: "docs",
    from: 1612948664566,
    limit: 10,
    projectId: "QmXGTs7mvAMMC7WW5ebrM33qKG32QK3h4vmQMjmY",
    target: "production",
    to: 1612948664566,
    users: "kr1PsOIzqEL5Xg6M4VZcZosf,K4amb7K9dAt5R2vBJWF32bmY",
    since: 1540095775941,
    until: 1540095775951,
    state: "BUILDING,READY",
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
    slug: "my-team-url-slug",
  });

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Example Usage of GetTeamMembersRequest in TypeScript
DESCRIPTION: This snippet demonstrates how to create an instance of the GetTeamMembersRequest object with example values for filtering team members by limit, time range (since/until), and role. It shows the necessary import statement from the Vercel SDK.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/models/getteammembersrequest.md#_snippet_0

LANGUAGE: typescript
CODE:
```
import { GetTeamMembersRequest } from "@vercel/sdk/models/getteammembersop.js";

let value: GetTeamMembersRequest = {
  limit: 20,
  since: 1540095775951,
  until: 1540095775951,
  role: "OWNER"
};
```

----------------------------------------

TITLE: Initializing DeployHooks object in TypeScript
DESCRIPTION: This snippet demonstrates how to import and initialize a DeployHooks object using the Vercel SDK. It shows the basic structure required to create an instance of the DeployHooks type with placeholder values for required fields like id, name, ref, and url.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/models/deployhooks.md#_snippet_0

LANGUAGE: typescript
CODE:
```
import { DeployHooks } from "@vercel/sdk/models/updateprojectdatacacheop.js";

let value: DeployHooks = {
  id: "<id>",
  name: "<value>",
  ref: "<value>",
  url: "https://valuable-remark.name/"
};
```

----------------------------------------

TITLE: Create Deployment using Vercel Class (TypeScript)
DESCRIPTION: Demonstrates how to create a new Vercel deployment using the main Vercel class instance. It shows how to initialize the SDK with a bearer token and call the createDeployment method with various configuration options like team ID, slug, files, git metadata, and project settings.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/deployments/README.md#_snippet_6

LANGUAGE: typescript
CODE:
```
import { Vercel } from "@vercel/sdk";

const vercel = new Vercel({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const result = await vercel.deployments.createDeployment({
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
    slug: "my-team-url-slug",
    requestBody: {
      deploymentId: "dpl_2qn7PZrx89yxY34vEZPD31Y9XVj6",
      files: [
        {
          file: "folder/file.js",
        },
        {
          file: "folder/file.js",
        },
      ],
      gitMetadata: {
        remoteUrl: "https://github.com/vercel/next.js",
        commitAuthorName: "kyliau",
        commitMessage: "add method to measure Interaction to Next Paint (INP) (#36490)",
        commitRef: "main",
        commitSha: "dc36199b2234c6586ebe05ec94078a895c707e29",
        dirty: true,
      },
      gitSource: {
        ref: "main",
        repoId: 123456789,
        sha: "a1b2c3d4e5f6g7h8i9j0k1l2m3n4o5p6q7r8s9t0",
        type: "github",
      },
      meta: {
        "foo": "bar",
      },
      name: "my-instant-deployment",
      project: "my-deployment-project",
      projectSettings: {
        buildCommand: "next build",
        installCommand: "pnpm install",
      },
      target: "production",
    },
  });

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Accepting Project Transfer Request with Vercel SDK (TypeScript)
DESCRIPTION: This snippet demonstrates how to accept a project transfer request using an instance of the main `Vercel` class. It requires providing the transfer `code`, the target `teamId`, the team `slug`, and an optional `requestBody` to specify a `newProjectName`. The result of the operation is logged to the console.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/projects/README.md#_snippet_36

LANGUAGE: typescript
CODE:
```
import { Vercel } from "@vercel/sdk";

const vercel = new Vercel({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const result = await vercel.projects.acceptProjectTransferRequest({
    code: "<value>",
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
    slug: "my-team-url-slug",
    requestBody: {
      newProjectName: "a-project-name",
    },
  });

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Add Project Member using Vercel Class (TypeScript)
DESCRIPTION: This snippet demonstrates how to add a member to a Vercel project using the `addProjectMember` method of the `projectMembers` property on a `Vercel` class instance. It requires a bearer token for authentication and specifies project details and the member's information and role.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/projectmembers/README.md#_snippet_2

LANGUAGE: typescript
CODE:
```
import { Vercel } from "@vercel/sdk";

const vercel = new Vercel({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const result = await vercel.projectMembers.addProjectMember({
    idOrName: "prj_pavWOn1iLObbXLRiwVvzmPrTWyTf",
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
    slug: "my-team-url-slug",
    requestBody: {
      uid: "ndlgr43fadlPyCtREAqxxdyFK",
      username: "example",
      email: "entity@example.com",
      role: "ADMIN"
    }
  });

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Checking Domain Status using Standalone Function (TypeScript)
DESCRIPTION: This example illustrates using the standalone `domainsCheckDomainStatus` function for checking domain status, which is recommended for better tree-shaking. It initializes a `VercelCore` instance and passes it along with parameters to the function, demonstrating error handling and result extraction.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/domains/README.md#_snippet_5

LANGUAGE: typescript
CODE:
```
import { VercelCore } from "@vercel/sdk/core.js";
import { domainsCheckDomainStatus } from "@vercel/sdk/funcs/domainsCheckDomainStatus.js";

// Use `VercelCore` for best tree-shaking performance.
// You can create one instance of it to use across an application.
const vercel = new VercelCore({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const res = await domainsCheckDomainStatus(vercel, {
    name: "example.com",
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
    slug: "my-team-url-slug",
  });

  if (!res.ok) {
    throw res.error;
  }

  const { value: result } = res;

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Initializing UpdateProjectProtectionBypassProtectionBypass1 Model in TypeScript
DESCRIPTION: This snippet demonstrates how to create an instance of the UpdateProjectProtectionBypassProtectionBypass1 model in TypeScript, showing the required fields and example values. It requires importing the model definition from the Vercel SDK.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/models/updateprojectprotectionbypassprotectionbypass1.md#_snippet_0

LANGUAGE: typescript
CODE:
```
import { UpdateProjectProtectionBypassProtectionBypass1 } from "@vercel/sdk/models/updateprojectprotectionbypassop.js";

let value: UpdateProjectProtectionBypassProtectionBypass1 = {
  createdAt: 2784.96,
  createdBy: "<value>",
  scope: "integration-automation-bypass",
  integrationId: "<id>",
};
```

----------------------------------------

TITLE: Listing Access Group Members using Standalone Function (TypeScript)
DESCRIPTION: Shows how to use the standalone accessGroupsListAccessGroupMembers function with a VercelCore instance. This approach is recommended for better tree-shaking. It requires passing the VercelCore instance and parameters, and includes basic error handling before processing the result.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/accessgroups/README.md#_snippet_7

LANGUAGE: typescript
CODE:
```
import { VercelCore } from "@vercel/sdk/core.js";
import { accessGroupsListAccessGroupMembers } from "@vercel/sdk/funcs/accessGroupsListAccessGroupMembers.js";

// Use `VercelCore` for best tree-shaking performance.
// You can create one instance of it to use across an application.
const vercel = new VercelCore({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const res = await accessGroupsListAccessGroupMembers(vercel, {
    idOrName: "ag_pavWOn1iLObbXLRiwVvzmPrTWyTf",
    limit: 20,
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
    slug: "my-team-url-slug",
  });

  if (!res.ok) {
    throw res.error;
  }

  const { value: result } = res;

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Delete Vercel Auth Token using Standalone Function - TypeScript
DESCRIPTION: Shows how to invalidate a Vercel authentication token using the standalone `authenticationDeleteAuthToken` function with a `VercelCore` instance for better tree-shaking. Requires a `VercelCore` instance initialized with a bearer token and the ID of the token to delete. Includes error handling.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/authentication/README.md#_snippet_9

LANGUAGE: typescript
CODE:
```
import { VercelCore } from "@vercel/sdk/core.js";
import { authenticationDeleteAuthToken } from "@vercel/sdk/funcs/authenticationDeleteAuthToken.js";

// Use \`VercelCore\` for best tree-shaking performance.
// You can create one instance of it to use across an application.
const vercel = new VercelCore({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const res = await authenticationDeleteAuthToken(vercel, {
    tokenId: "5d9f2ebd38ddca62e5d51e9c1704c72530bdc8bfdd41e782a6687c48399e8391",
  });

  if (!res.ok) {
    throw res.error;
  }

  const { value: result } = res;

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Retrieve Projects using Vercel SDK Class (TypeScript)
DESCRIPTION: Demonstrates how to use the `getProjects` method via the `Vercel` class instance. It shows how to initialize the SDK with a bearer token and call the method with various filtering parameters.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/projects/README.md#_snippet_2

LANGUAGE: typescript
CODE:
```
import { Vercel } from "@vercel/sdk";

const vercel = new Vercel({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>"
});

async function run() {
  const result = await vercel.projects.getProjects({
    gitForkProtection: "1",
    repoUrl: "https://github.com/vercel/next.js",
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
    slug: "my-team-url-slug"
  });

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Moving Project Domain using Vercel SDK Standalone Function (TypeScript)
DESCRIPTION: Shows how to use the standalone `projectsMoveProjectDomain` function for moving a project's domain, which is recommended for better tree-shaking. Requires importing `VercelCore` and `projectsMoveProjectDomain`. Demonstrates creating a `VercelCore` instance and passing it to the function along with project details. Includes error handling.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/projects/README.md#_snippet_21

LANGUAGE: typescript
CODE:
```
import { VercelCore } from "@vercel/sdk/core.js";
import { projectsMoveProjectDomain } from "@vercel/sdk/funcs/projectsMoveProjectDomain.js";

// Use `VercelCore` for best tree-shaking performance.
// You can create one instance of it to use across an application.
const vercel = new VercelCore({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const res = await projectsMoveProjectDomain(vercel, {
    idOrName: "<value>",
    domain: "www.example.com",
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
    slug: "my-team-url-slug",
  });

  if (!res.ok) {
    throw res.error;
  }

  const { value: result } = res;

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Fetching Integration Log Drains with Vercel Class (TypeScript)
DESCRIPTION: This snippet demonstrates how to retrieve integration log drains using an instance of the main `Vercel` class from the `@vercel/sdk`. It requires providing a bearer token and optionally a team ID or slug to filter the results.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/logdrains/README.md#_snippet_2

LANGUAGE: typescript
CODE:
```
import { Vercel } from "@vercel/sdk";

const vercel = new Vercel({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const result = await vercel.logDrains.getIntegrationLogDrains({
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
    slug: "my-team-url-slug",
  });

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Update Attack Challenge Mode using Standalone Function (TypeScript)
DESCRIPTION: This example demonstrates how to update the Attack Challenge mode setting using the standalone `securityUpdateAttackChallengeMode` function from the SDK's core module. It shows initializing `VercelCore`, calling the function with the core instance and parameters, checking the result for errors, and handling the successful response.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/security/README.md#_snippet_1

LANGUAGE: typescript
CODE:
```
import { VercelCore } from "@vercel/sdk/core.js";
import { securityUpdateAttackChallengeMode } from "@vercel/sdk/funcs/securityUpdateAttackChallengeMode.js";

// Use `VercelCore` for best tree-shaking performance.
// You can create one instance of it to use across an application.
const vercel = new VercelCore({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const res = await securityUpdateAttackChallengeMode(vercel, {
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
    slug: "my-team-url-slug",
    requestBody: {
      projectId: "<id>",
      attackModeEnabled: true,
    },
  });

  if (!res.ok) {
    throw res.error;
  }

  const { value: result } = res;

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: List Access Groups using Vercel SDK Client (TypeScript)
DESCRIPTION: This example demonstrates how to list Vercel access groups using the standard Vercel SDK client. It initializes the Vercel client with a bearer token and then calls the `accessGroups.listAccessGroups` method with various filtering and pagination parameters. The result is logged to the console.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/accessgroups/README.md#_snippet_8

LANGUAGE: typescript
CODE:
```
import { Vercel } from "@vercel/sdk";

const vercel = new Vercel({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const result = await vercel.accessGroups.listAccessGroups({
    projectId: "prj_pavWOn1iLObbx3RowVvzmPrTWyTf",
    search: "example",
    membersLimit: 20,
    projectsLimit: 20,
    limit: 20,
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
    slug: "my-team-url-slug",
  });

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Creating a Check using Standalone Function (TypeScript)
DESCRIPTION: Shows how to create a new Vercel Check using the standalone checksCreateCheck function, which is recommended for better tree-shaking. It requires an instance of VercelCore and the check configuration parameters. The example includes basic error handling for the function call.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/checks/README.md#_snippet_1

LANGUAGE: typescript
CODE:
```
import { VercelCore } from "@vercel/sdk/core.js";
import { checksCreateCheck } from "@vercel/sdk/funcs/checksCreateCheck.js";

// Use `VercelCore` for best tree-shaking performance.
// You can create one instance of it to use across an application.
const vercel = new VercelCore({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const res = await checksCreateCheck(vercel, {
    deploymentId: "dpl_2qn7PZrx89yxY34vEZPD31Y9XVj6",
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
    slug: "my-team-url-slug",
    requestBody: {
      name: "Performance Check",
      path: "/",
      blocking: true,
      detailsUrl: "http://example.com",
      externalId: "1234abc",
      rerequestable: true,
    },
  });

  if (!res.ok) {
    throw res.error;
  }

  const { value: result } = res;

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Check Domain Price using Standalone Function (TypeScript)
DESCRIPTION: This example shows how to use the standalone `domainsCheckDomainPrice` function with a `VercelCore` instance for potentially better tree-shaking. It takes the core instance and an options object with domain details. Error handling is demonstrated.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/domains/README.md#_snippet_3

LANGUAGE: typescript
CODE:
```
import { VercelCore } from "@vercel/sdk/core.js";
import { domainsCheckDomainPrice } from "@vercel/sdk/funcs/domainsCheckDomainPrice.js";

// Use `VercelCore` for best tree-shaking performance.
// You can create one instance of it to use across an application.
const vercel = new VercelCore({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const res = await domainsCheckDomainPrice(vercel, {
    name: "example.com",
    type: "new",
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
    slug: "my-team-url-slug",
  });

  if (!res.ok) {
    throw res.error;
  }

  const { value: result } = res;

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Example models.Two2 Object in TypeScript
DESCRIPTION: This snippet provides an example of the models.Two2 type, illustrating how to structure an object for creating a project environment variable. It shows a similar structure to models.Two1, defining properties like key, value, type, target, gitBranch, comment, and customEnvironmentIds.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/models/createprojectenvrequestbody2.md#_snippet_1

LANGUAGE: typescript
CODE:
```
const value: models.Two2 = {
  key: "API_URL",
  value: "https://api.vercel.com",
  type: "plain",
  target: [
    "preview",
  ],
  gitBranch: "feature-1",
  comment: "database connection string for production",
  customEnvironmentIds: [
    "env_1234567890",
  ],
};

```

----------------------------------------

TITLE: Creating CreateOrTransferDomainRequestBody3 for Transfer-in (TypeScript)
DESCRIPTION: This snippet demonstrates how to create an instance of the CreateOrTransferDomainRequestBody3 object in TypeScript, populating it with example values required for a domain transfer-in operation, including the domain name, method, authorization code, and expected price.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/models/createortransferdomainrequestbody3.md#_snippet_0

LANGUAGE: typescript
CODE:
```
import { CreateOrTransferDomainRequestBody3 } from "@vercel/sdk/models/createortransferdomainop.js";

let value: CreateOrTransferDomainRequestBody3 = {
  name: "example.com",
  method: "transfer-in",
  authCode: "fdhfr820ad#@FAdlj$$",
  expectedPrice: 8,
};
```

----------------------------------------

TITLE: Possible values for CreateProjectEnv2Target in TypeScript
DESCRIPTION: This snippet shows the union of string literals that define the valid values for the `CreateProjectEnv2Target` type. These represent the standard Vercel environments.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/models/createprojectenv2target.md#_snippet_1

LANGUAGE: typescript
CODE:
```
"production" | "preview" | "development"
```

----------------------------------------

TITLE: Listing Aliases using Vercel SDK Standalone Function (TypeScript)
DESCRIPTION: This example illustrates how to use the `aliasesListAliases` standalone function, which is recommended for better tree-shaking performance. It requires an instance of `VercelCore` and accepts similar filtering options as the class method. The snippet also demonstrates how to handle the function's result, checking for errors before accessing the value.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/aliases/README.md#_snippet_5

LANGUAGE: typescript
CODE:
```
import { VercelCore } from "@vercel/sdk/core.js";
import { aliasesListAliases } from "@vercel/sdk/funcs/aliasesListAliases.js";

// Use `VercelCore` for best tree-shaking performance.
// You can create one instance of it to use across an application.
const vercel = new VercelCore({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const res = await aliasesListAliases(vercel, {
    domain: "my-test-domain.com",
    from: 1540095775951,
    limit: 10,
    projectId: "prj_12HKQaOmR5t5Uy6vdcQsNIiZgHGB",
    since: 1540095775941,
    until: 1540095775951,
    rollbackDeploymentId: "dpl_XXX",
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
    slug: "my-team-url-slug",
  });

  if (!res.ok) {
    throw res.error;
  }

  const { value: result } = res;

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Importing and Initializing Metadata Type (TypeScript)
DESCRIPTION: This snippet demonstrates how to import the `Metadata` type from the `@vercel/sdk` package and declare a variable of this type, initializing it as an empty object. This type is used to represent metadata associated with certain operations, such as edge config backups.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/models/metadata.md#_snippet_0

LANGUAGE: TypeScript
CODE:
```
import { Metadata } from "@vercel/sdk/models/getedgeconfigbackupop.js";

let value: Metadata = {};
```

----------------------------------------

TITLE: Listing Deployments with Vercel SDK (TypeScript)
DESCRIPTION: This snippet demonstrates how to list deployments associated with the authenticated user or team using the Vercel SDK. It requires the @vercel/sdk dependency and a bearer token for authentication. The example shows various optional parameters like app name, project ID, target environment, state, and time filters.
SOURCE: https://github.com/vercel/sdk/blob/main/USAGE.md#_snippet_0

LANGUAGE: typescript
CODE:
```
import { Vercel } from "@vercel/sdk";

const vercel = new Vercel({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const result = await vercel.deployments.getDeployments({
    app: "docs",
    from: 1612948664566,
    limit: 10,
    projectId: "QmXGTs7mvAMMC7WW5ebrM33qKG32QK3h4vmQMjmY",
    target: "production",
    to: 1612948664566,
    users: "kr1PsOIzqEL5Xg6M4VZcZosf,K4amb7K9dAt5R2vBJWF32bmY",
    since: 1540095775941,
    until: 1540095775951,
    state: "BUILDING,READY",
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
    slug: "my-team-url-slug",
  });

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Instantiating models.CreateProjectEnv11 object (TypeScript)
DESCRIPTION: This snippet shows how to create an example object of type `models.CreateProjectEnv11`. It demonstrates the required properties like `key`, `value`, `type`, `target`, and optional properties such as `gitBranch`, `comment`, and `customEnvironmentIds` for configuring a project environment variable.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/models/createprojectenvrequestbody1.md#_snippet_0

LANGUAGE: typescript
CODE:
```
const value: models.CreateProjectEnv11 = {
  key: "API_URL",
  value: "https://api.vercel.com",
  type: "plain",
  target: [
    "preview",
  ],
  gitBranch: "feature-1",
  comment: "database connection string for production",
  customEnvironmentIds: [
    "env_1234567890",
  ],
};
```

----------------------------------------

TITLE: Update Custom Environment using Vercel Class - TypeScript
DESCRIPTION: Demonstrates how to update a custom environment using an instance of the main `Vercel` class. It shows initializing the class with a bearer token and calling the `environment.updateCustomEnvironment` method with the required parameters like ID/name, environment slug/ID, team ID, and slug.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/environment/README.md#_snippet_6

LANGUAGE: typescript
CODE:
```
import { Vercel } from "@vercel/sdk";

const vercel = new Vercel({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const result = await vercel.environment.updateCustomEnvironment({
    idOrName: "<value>",
    environmentSlugOrId: "<id>",
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
    slug: "my-team-url-slug",
  });

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Editing Vercel Project Environment Variable using Vercel SDK Class (TypeScript)
DESCRIPTION: Demonstrates how to edit a specific environment variable for a Vercel project using the `vercel.projects.editProjectEnv` method from the `@vercel/sdk`. It requires a `Vercel` instance initialized with a bearer token and takes parameters like project ID/name, environment variable ID, team ID/slug, and a request body defining the variable's properties.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/projects/README.md#_snippet_32

LANGUAGE: TypeScript
CODE:
```
import { Vercel } from "@vercel/sdk";

const vercel = new Vercel({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const result = await vercel.projects.editProjectEnv({
    idOrName: "prj_XLKmu1DyR1eY7zq8UgeRKbA7yVLA",
    id: "XMbOEya1gUUO1ir4",
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
    slug: "my-team-url-slug",
    requestBody: {
      key: "GITHUB_APP_ID",
      target: [
        "preview"
      ],
      gitBranch: "feature-1",
      type: "plain",
      value: "bkWIjbnxcvo78",
      customEnvironmentIds: [
        "env_1234567890"
      ],
      comment: "database connection string for production"
    }
  });

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Initialize AddProjectMemberRequestBody3 in TypeScript
DESCRIPTION: This snippet demonstrates how to create and initialize an instance of the `AddProjectMemberRequestBody3` type in TypeScript, providing example values for its fields like `uid`, `username`, `email`, and `role`. It shows the basic structure for constructing the request body.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/models/addprojectmemberrequestbody3.md#_snippet_0

LANGUAGE: typescript
CODE:
```
import { AddProjectMemberRequestBody3 } from "@vercel/sdk/models/addprojectmemberop.js";

let value: AddProjectMemberRequestBody3 = {
  uid: "ndlgr43fadlPyCtREAqxxdyFK",
  username: "example",
  email: "entity@example.com",
  role: "ADMIN"
};
```

----------------------------------------

TITLE: Initializing models.GetDeploymentGitRepo3 TypeScript
DESCRIPTION: This snippet demonstrates how to create an instance of the `models.GetDeploymentGitRepo3` type in TypeScript, which is used to represent a Bitbucket repository configuration. It shows the required properties like owner, repoUuid, slug, type, workspaceUuid, path, defaultBranch, name, private status, and owner type.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/models/responsebodygitrepo.md#_snippet_2

LANGUAGE: typescript
CODE:
```
const value: models.GetDeploymentGitRepo3 = {
  owner: "<value>",
  repoUuid: "<id>",
  slug: "<value>",
  type: "bitbucket",
  workspaceUuid: "<id>",
  path: "/opt/include",
  defaultBranch: "<value>",
  name: "<value>",
  private: false,
  ownerType: "team"
};
```

----------------------------------------

TITLE: Initializing ResponseBodyItems in TypeScript
DESCRIPTION: Demonstrates how to create and initialize an instance of the ResponseBodyItems type with example values for its required fields.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/models/responsebodyitems.md#_snippet_0

LANGUAGE: typescript
CODE:
```
import { ResponseBodyItems } from "@vercel/sdk/models/getedgeconfigbackupop.js";

let value: ResponseBodyItems = {
  updatedAt: 2293.73,
  value: false,
  createdAt: 5115.86,
};
```

----------------------------------------

TITLE: Updating Vercel Check using SDK Class (TypeScript)
DESCRIPTION: Update an existing check using the Vercel SDK's main class instance. This endpoint requires an OAuth2 bearer token for authentication. The example demonstrates updating various check details including performance metrics.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/checks/README.md#_snippet_6

LANGUAGE: typescript
CODE:
```
import { Vercel } from "@vercel/sdk";

const vercel = new Vercel({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const result = await vercel.checks.updateCheck({
    deploymentId: "dpl_2qn7PZrx89yxY34vEZPD31Y9XVj6",
    checkId: "check_2qn7PZrx89yxY34vEZPD31Y9XVj6",
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
    slug: "my-team-url-slug",
    requestBody: {
      name: "Performance Check",
      path: "/",
      detailsUrl: "https://example.com/check/run/1234abc",
      output: {
        metrics: {
          fcp: {
            value: 1200,
            previousValue: 900,
            source: "web-vitals"
          },
          lcp: {
            value: 1200,
            previousValue: 1000,
            source: "web-vitals"
          },
          cls: {
            value: 4,
            previousValue: 2,
            source: "web-vitals"
          },
          tbt: {
            value: 3000,
            previousValue: 3500,
            source: "web-vitals"
          },
          virtualExperienceScore: {
            value: 30,
            previousValue: 35,
            source: "web-vitals"
          }
        }
      },
      externalId: "1234abc"
    }
  });

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Example Request Body for Specific Deployment Types (TypeScript)
DESCRIPTION: Demonstrates the structure of the request body for `models.GetProjectsTrustedIps1`, used to specify trusted IPs for production deployment URLs and all previews. Includes a list of IP addresses and the protection mode.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/models/getprojectstrustedips.md#_snippet_0

LANGUAGE: typescript
CODE:
```
const value: models.GetProjectsTrustedIps1 = {
  deploymentType: "prod_deployment_urls_and_all_previews",
  addresses: [
    {
      value: "<value>"
    }
  ],
  protectionMode: "additional"
};
```

----------------------------------------

TITLE: Initializing CreateProjectSecurity Object - TypeScript
DESCRIPTION: This snippet demonstrates how to import the `CreateProjectSecurity` type from the Vercel SDK and initialize a variable of this type. This object is used to configure security settings for a new project.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/models/createprojectsecurity.md#_snippet_0

LANGUAGE: typescript
CODE:
```
import { CreateProjectSecurity } from "@vercel/sdk/models/createprojectop.js";

let value: CreateProjectSecurity = {};
```

----------------------------------------

TITLE: Fetch Domain Config using Vercel Class (TypeScript)
DESCRIPTION: Demonstrates how to fetch domain configuration using the main Vercel class. Instantiate the Vercel class with a bearer token and call the domains.getDomainConfig method, providing the domain, teamId, and slug.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/domains/README.md#_snippet_8

LANGUAGE: typescript
CODE:
```
import { Vercel } from "@vercel/sdk";

const vercel = new Vercel({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const result = await vercel.domains.getDomainConfig({
    domain: "example.com",
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
    slug: "my-team-url-slug",
  });

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Updating Domain using Vercel SDK (Standalone Function) - TypeScript
DESCRIPTION: Illustrates how to use the standalone `domainsPatchDomain` function from the Vercel SDK core for updating or moving an apex domain. It shows initializing `VercelCore` and calling the function directly, including error handling.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/domains/README.md#_snippet_17

LANGUAGE: typescript
CODE:
```
import { VercelCore } from "@vercel/sdk/core.js";
import { domainsPatchDomain } from "@vercel/sdk/funcs/domainsPatchDomain.js";

// Use \`VercelCore\` for best tree-shaking performance.
// You can create one instance of it to use across an application.
const vercel = new VercelCore({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>"
});

async function run() {
  const res = await domainsPatchDomain(vercel, {
    domain: "tight-secrecy.info",
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
    slug: "my-team-url-slug",
    requestBody: {
      op: "update"
    }
  });

  if (!res.ok) {
    throw res.error;
  }

  const { value: result } = res;

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Retrieving Vercel Domains using SDK Class (TypeScript)
DESCRIPTION: This snippet demonstrates how to retrieve a list of domains using the `Vercel` class instance from the `@vercel/sdk`. It shows how to initialize the SDK with a bearer token and call the `domains.getDomains` method with optional parameters like `limit`, `since`, `until`, `teamId`, and `slug`.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/domains/README.md#_snippet_12

LANGUAGE: typescript
CODE:
```
import { Vercel } from "@vercel/sdk";

const vercel = new Vercel({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const result = await vercel.domains.getDomains({
    limit: 20,
    since: 1609499532000,
    until: 1612264332000,
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
    slug: "my-team-url-slug",
  });

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Fetching Edge Config Backups with Standalone Function (TypeScript)
DESCRIPTION: Initializes `VercelCore` for better tree-shaking and calls the standalone `edgeConfigGetEdgeConfigBackups` function with the core instance and parameters. Includes error handling and extracts the result value. Requires `@vercel/sdk/core.js` and `@vercel/sdk/funcs/edgeConfigGetEdgeConfigBackups.js`.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/edgeconfig/README.md#_snippet_31

LANGUAGE: typescript
CODE:
```
import { VercelCore } from "@vercel/sdk/core.js";
import { edgeConfigGetEdgeConfigBackups } from "@vercel/sdk/funcs/edgeConfigGetEdgeConfigBackups.js";

// Use `VercelCore` for best tree-shaking performance.
// You can create one instance of it to use across an application.
const vercel = new VercelCore({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const res = await edgeConfigGetEdgeConfigBackups(vercel, {
    edgeConfigId: "<id>",
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
    slug: "my-team-url-slug",
  });

  if (!res.ok) {
    throw res.error;
  }

  const { value: result } = res;

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Inviting User to Team using Vercel SDK (TypeScript)
DESCRIPTION: Demonstrates how to invite a user to a Vercel team using the standard `Vercel` SDK instance. It shows how to initialize the SDK with a bearer token and call the `teams.inviteUserToTeam` method with team ID, user identifier (ID and email), and project roles.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/teams/README.md#_snippet_2

LANGUAGE: TypeScript
CODE:
```
import { Vercel } from "@vercel/sdk";

const vercel = new Vercel({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const result = await vercel.teams.inviteUserToTeam({
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
    requestBody: {
      uid: "kr1PsOIzqEL5Xg6M4VZcZosf",
      email: "john@example.com",
      projects: [
        {
          projectId: "prj_ndlgr43fadlPyCtREAqxxdyFK",
          role: "ADMIN"
        },
        {
          projectId: "prj_ndlgr43fadlPyCtREAqxxdyFK",
          role: "ADMIN"
        }
      ]
    }
  });

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Example Usage of ListUserEventsResponseBody in TypeScript
DESCRIPTION: This snippet demonstrates how to instantiate and populate a ListUserEventsResponseBody object in TypeScript, showing the expected structure and data types for the 'events' array and nested objects like 'entities' and 'payload'. It includes the necessary import statement for the type definition.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/models/listusereventsresponsebody.md#_snippet_0

LANGUAGE: typescript
CODE:
```
import { ListUserEventsResponseBody } from "@vercel/sdk/models/listusereventsop.js";

let value: ListUserEventsResponseBody = {
  events: [
    {
      id: "uev_bfmMjiMnXfnPbT97dGdpJbCN",
      text: "You logged in via GitHub",
      entities: [
        {
          type: "author",
          start: 0,
          end: 3,
        },
      ],
      createdAt: 1632859321020,
      userId: "zTuNVUXEAvvnNN3IaqinkyMw",
      principalId: "<id>",
      payload: {
        projectName: "<value>",
        passwordProtection: "all",
        oldPasswordProtection: {
          deploymentType: "all",
        },
      },
    },
  ],
};
```

----------------------------------------

TITLE: Example Usage of CreateAccessGroupProjectResponseBody in TypeScript
DESCRIPTION: This snippet demonstrates how to create an instance of the `CreateAccessGroupProjectResponseBody` type with example values. It shows the required fields and their expected types, illustrating how to represent the response data for an access group project association.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/models/createaccessgroupprojectresponsebody.md#_snippet_0

LANGUAGE: typescript
CODE:
```
import { CreateAccessGroupProjectResponseBody } from "@vercel/sdk/models/createaccessgroupprojectop.js";

let value: CreateAccessGroupProjectResponseBody = {
  teamId: "<id>",
  accessGroupId: "<id>",
  projectId: "<id>",
  role: "PROJECT_VIEWER",
  createdAt: "1724294650285",
  updatedAt: "1746765514026"
};
```

----------------------------------------

TITLE: Add Project Domain using Vercel SDK Class (TypeScript)
DESCRIPTION: Demonstrates how to add a domain to a Vercel project using the `Vercel` class instance from the `@vercel/sdk`. It shows initialization with a bearer token and calling the `projects.addProjectDomain` method with project details and domain configuration.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/projects/README.md#_snippet_18

LANGUAGE: TypeScript
CODE:
```
import { Vercel } from "@vercel/sdk";

const vercel = new Vercel({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const result = await vercel.projects.addProjectDomain({
    idOrName: "<value>",
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
    slug: "my-team-url-slug",
    requestBody: {
      name: "www.example.com",
      gitBranch: null,
      redirect: "foobar.com",
      redirectStatusCode: 307,
    },
  });

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Deleting Access Group Project using Vercel Class (TypeScript)
DESCRIPTION: Demonstrates how to delete an access group project using the deleteAccessGroupProject method available on an instance of the main Vercel class. Requires a bearer token for authentication.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/accessgroups/README.md#_snippet_20

LANGUAGE: typescript
CODE:
```
import { Vercel } from "@vercel/sdk";

const vercel = new Vercel({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  await vercel.accessGroups.deleteAccessGroupProject({
    accessGroupIdOrName: "ag_1a2b3c4d5e6f7g8h9i0j",
    projectId: "prj_ndlgr43fadlPyCtREAqxxdyFK",
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
    slug: "my-team-url-slug",
  });


}

run();
```

----------------------------------------

TITLE: Removing Vercel Certificate using Standalone Function (TypeScript)
DESCRIPTION: Shows how to use the VercelCore class and the standalone certsRemoveCert function for removing a certificate, which is recommended for better tree-shaking. It illustrates passing the VercelCore instance and parameters, checking for errors, and handling the successful result.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/certs/README.md#_snippet_3

LANGUAGE: typescript
CODE:
```
import { VercelCore } from "@vercel/sdk/core.js";
import { certsRemoveCert } from "@vercel/sdk/funcs/certsRemoveCert.js";

// Use `VercelCore` for best tree-shaking performance.
// You can create one instance of it to use across an application.
const vercel = new VercelCore({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const res = await certsRemoveCert(vercel, {
    id: "<id>",
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
    slug: "my-team-url-slug",
  });

  if (!res.ok) {
    throw res.error;
  }

  const { value: result } = res;

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Creating ExchangeSsoTokenRequestBody Instance (TypeScript)
DESCRIPTION: This snippet demonstrates how to create an instance of the ExchangeSsoTokenRequestBody type in TypeScript, showing the required fields (code, clientId, clientSecret) needed to initiate the SSO token exchange process.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/models/exchangessotokenrequestbody.md#_snippet_0

LANGUAGE: typescript
CODE:
```
import { ExchangeSsoTokenRequestBody } from "@vercel/sdk/models/exchangessotokenop.js";

let value: ExchangeSsoTokenRequestBody = {
  code: "<value>",
  clientId: "<id>",
  clientSecret: "<value>"
};
```

----------------------------------------

TITLE: Example Usage of GetAliasDeployment Type
DESCRIPTION: This snippet demonstrates how to import the GetAliasDeployment type and create an instance of it with example values for its required fields (id, url) and the optional meta field.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/models/getaliasdeployment.md#_snippet_0

LANGUAGE: TypeScript
CODE:
```
import { GetAliasDeployment } from "@vercel/sdk/models/getaliasop.js";

let value: GetAliasDeployment = {
  id: "dpl_5m8CQaRBm3FnWRW1od3wKTpaECPx",
  url: "my-instant-deployment-3ij3cxz9qr.now.sh",
  meta: "{}"
};
```

----------------------------------------

TITLE: List Access Groups using Standalone Function (TypeScript)
DESCRIPTION: This example demonstrates how to list Vercel access groups using the standalone function approach with `VercelCore` for improved tree-shaking. It initializes `VercelCore` and calls the `accessGroupsListAccessGroups` function, handling potential errors and logging the successful result.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/accessgroups/README.md#_snippet_9

LANGUAGE: typescript
CODE:
```
import { VercelCore } from "@vercel/sdk/core.js";
import { accessGroupsListAccessGroups } from "@vercel/sdk/funcs/accessGroupsListAccessGroups.js";

// Use `VercelCore` for best tree-shaking performance.
// You can create one instance of it to use across an application.
const vercel = new VercelCore({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const res = await accessGroupsListAccessGroups(vercel, {
    projectId: "prj_pavWOn1iLObbx3RowVvzmPrTWyTf",
    search: "example",
    membersLimit: 20,
    projectsLimit: 20,
    limit: 20,
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
    slug: "my-team-url-slug",
  });

  if (!res.ok) {
    throw res.error;
  }

  const { value: result } = res;

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Example Usage of Members Type in TypeScript
DESCRIPTION: This snippet demonstrates how to import and initialize an object of the `Members` type from the @vercel/sdk, showing the required fields and their typical values.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/models/members.md#_snippet_0

LANGUAGE: TypeScript
CODE:
```
import { Members } from "@vercel/sdk/models/listaccessgroupmembersop.js";

let value: Members = {
  email: "Felicity63@yahoo.com",
  uid: "<id>",
  username: "Brianne81",
  teamRole: "BILLING"
};
```

----------------------------------------

TITLE: Retrieving Vercel Alias using Vercel SDK Class (TypeScript)
DESCRIPTION: Demonstrates how to retrieve a Vercel Alias using the `getAlias` method available on the `aliases` property of a `Vercel` SDK instance. It requires a bearer token for authentication and accepts various parameters to specify the target alias.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/aliases/README.md#_snippet_6

LANGUAGE: typescript
CODE:
```
import { Vercel } from "@vercel/sdk";

const vercel = new Vercel({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const result = await vercel.aliases.getAlias({
    from: 1540095775951,
    idOrAlias: "example.vercel.app",
    projectId: "prj_12HKQaOmR5t5Uy6vdcQsNIiZgHGB",
    since: 1540095775941,
    until: 1540095775951,
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
    slug: "my-team-url-slug"
  });

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Deleting Edge Config using Vercel Class (TypeScript)
DESCRIPTION: Demonstrates how to delete an Edge Config by ID using the deleteEdgeConfig method of an instantiated Vercel class. Requires a bearer token for authentication and the edgeConfigId. Optional parameters include teamId or slug.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/edgeconfig/README.md#_snippet_8

LANGUAGE: typescript
CODE:
```
import { Vercel } from "@vercel/sdk";

const vercel = new Vercel({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  await vercel.edgeConfig.deleteEdgeConfig({
    edgeConfigId: "<id>",
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
    slug: "my-team-url-slug",
  });


}

run();
```

----------------------------------------

TITLE: Update Access Group Project using Vercel SDK Class - TypeScript
DESCRIPTION: Demonstrates updating an access group project using the updateAccessGroupProject method available on an instance of the Vercel class. Requires providing the access group ID or name, project ID, team ID, slug, and the request body specifying the desired role.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/accessgroups/README.md#_snippet_18

LANGUAGE: typescript
CODE:
```
import { Vercel } from "@vercel/sdk";

const vercel = new Vercel({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const result = await vercel.accessGroups.updateAccessGroupProject({
    accessGroupIdOrName: "ag_1a2b3c4d5e6f7g8h9i0j",
    projectId: "prj_ndlgr43fadlPyCtREAqxxdyFK",
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
    slug: "my-team-url-slug",
    requestBody: {
      role: "ADMIN",
    },
  });

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Retrieving Deployment File Contents using Vercel SDK (TypeScript)
DESCRIPTION: Demonstrates how to initialize the Vercel SDK with a bearer token and call the `deployments.getDeploymentFileContents` method on a Vercel instance to retrieve the content of a specific file within a deployment. It shows the required parameters like deployment ID, file ID, team ID, and slug.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/deployments/README.md#_snippet_14

LANGUAGE: typescript
CODE:
```
import { Vercel } from "@vercel/sdk";

const vercel = new Vercel({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>"
});

async function run() {
  const result = await vercel.deployments.getDeploymentFileContents({
    id: "<id>",
    fileId: "<id>",
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
    slug: "my-team-url-slug"
  });

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Creating an ImportResourceRequest instance in TypeScript
DESCRIPTION: This snippet demonstrates how to import the ImportResourceRequest type from the Vercel SDK and initialize a variable with the required fields 'integrationConfigurationId' and 'resourceId'.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/models/importresourcerequest.md#_snippet_0

LANGUAGE: typescript
CODE:
```
import { ImportResourceRequest } from "@vercel/sdk/models/importresourceop.js";

let value: ImportResourceRequest = {
  integrationConfigurationId: "<id>",
  resourceId: "<id>"
};
```

----------------------------------------

TITLE: Update Firewall Config - Update CRS (Type 6)
DESCRIPTION: This request body type is used to update the configuration of a Core Rule Set (CRS). It specifies the 'crs.update' action, the CRS ID (e.g., 'sf'), and the updated CRS settings.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/models/updatefirewallconfigrequestbody.md#_snippet_5

LANGUAGE: typescript
CODE:
```
const value: models.UpdateFirewallConfigRequestBody6 = {
  action: "crs.update",
  id: "sf",
  value: {
    active: false,
    action: "deny",
  },
};
```

----------------------------------------

TITLE: Instantiating GetDeploymentRoutes2 Type - TypeScript
DESCRIPTION: This snippet demonstrates how to import and instantiate the `GetDeploymentRoutes2` type from the `@vercel/sdk` library in TypeScript. It shows a basic example assigning a literal object with the required `handle` field to a variable of this type.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/models/getdeploymentroutes2.md#_snippet_0

LANGUAGE: typescript
CODE:
```
import { GetDeploymentRoutes2 } from "@vercel/sdk/models/getdeploymentop.js";

let value: GetDeploymentRoutes2 = {
  handle: "resource"
};
```

----------------------------------------

TITLE: Uploading File using Standalone Function (TypeScript)
DESCRIPTION: Shows how to use the standalone `deploymentsUploadFile` function with a `VercelCore` instance, which is recommended for better tree-shaking. The example includes basic error handling by checking the `ok` property of the response.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/deployments/README.md#_snippet_11

LANGUAGE: typescript
CODE:
```
import { VercelCore } from "@vercel/sdk/core.js";
import { deploymentsUploadFile } from "@vercel/sdk/funcs/deploymentsUploadFile.js";

// Use `VercelCore` for best tree-shaking performance.
// You can create one instance of it to use across an application.
const vercel = new VercelCore({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const res = await deploymentsUploadFile(vercel, {
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
    slug: "my-team-url-slug",
  });

  if (!res.ok) {
    throw res.error;
  }

  const { value: result } = res;

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Example models.RemoveProjectEnvContentHintProjects1 (redis-url)
DESCRIPTION: This snippet provides an example structure for the `models.RemoveProjectEnvContentHintProjects1` type. It represents a content hint for removing a Redis URL environment variable, requiring the specific `type` value 'redis-url' and the `storeId` of the Redis instance.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/models/removeprojectenvresponsebodyprojectscontenthint.md#_snippet_0

LANGUAGE: typescript
CODE:
```
const value: models.RemoveProjectEnvContentHintProjects1 = {
  type: "redis-url",
  storeId: "<id>"
};
```

----------------------------------------

TITLE: Possible Values for GetProjectsFunctionDefaultMemoryType in TypeScript
DESCRIPTION: This snippet lists the valid string literal values that can be assigned to a variable of type `GetProjectsFunctionDefaultMemoryType`, defining the available memory types.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/models/getprojectsfunctiondefaultmemorytype.md#_snippet_1

LANGUAGE: typescript
CODE:
```
"standard_legacy" | "standard" | "performance"
```

----------------------------------------

TITLE: Example Usage - BuyDomainDomainsResponseBody - TypeScript
DESCRIPTION: This snippet demonstrates how to create an instance of the `BuyDomainDomainsResponseBody` type. It shows the expected structure and data types for the `domain` field, which is of type `models.BuyDomainDomain`. This example requires importing the `BuyDomainDomainsResponseBody` type from the `@vercel/sdk/models/buydomainop.js` module.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/models/buydomaindomainsresponsebody.md#_snippet_0

LANGUAGE: typescript
CODE:
```
import { BuyDomainDomainsResponseBody } from "@vercel/sdk/models/buydomainop.js";

let value: BuyDomainDomainsResponseBody = {
  domain: {
    uid: "<id>",
    ns: [
      "<value>"
    ],
    verified: false,
    created: 8058.73,
    pending: false,
  },
};
```

----------------------------------------

TITLE: Creating Edge Config using Vercel SDK Standalone Function (TypeScript)
DESCRIPTION: This snippet shows the alternative approach using a standalone function (edgeConfigCreateEdgeConfig) for creating an Edge Config. It utilizes VercelCore for potentially better tree-shaking and includes basic error handling for the result.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/edgeconfig/README.md#_snippet_3

LANGUAGE: typescript
CODE:
```
import { VercelCore } from "@vercel/sdk/core.js";
import { edgeConfigCreateEdgeConfig } from "@vercel/sdk/funcs/edgeConfigCreateEdgeConfig.js";

// Use `VercelCore` for best tree-shaking performance.
// You can create one instance of it to use across an application.
const vercel = new VercelCore({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const res = await edgeConfigCreateEdgeConfig(vercel, {
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
    slug: "my-team-url-slug",
    requestBody: {
      slug: "<value>",
    },
  });

  if (!res.ok) {
    throw res.error;
  }

  const { value: result } = res;

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Delete Domain using Vercel Instance - TypeScript
DESCRIPTION: This snippet demonstrates how to delete a domain using the `deleteDomain` method available on the `domains` property of a `Vercel` instance. It requires importing the `Vercel` class and providing a bearer token for authentication. The method takes an object with the domain name, optional team ID, and slug.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/domains/README.md#_snippet_18

LANGUAGE: typescript
CODE:
```
import { Vercel } from "@vercel/sdk";

const vercel = new Vercel({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const result = await vercel.domains.deleteDomain({
    domain: "example.com",
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
    slug: "my-team-url-slug",
  });

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Using Vercel SDK Class to Create or Transfer Domain
DESCRIPTION: Demonstrates how to use the Vercel SDK's main class instance to call the domains.createOrTransferDomain method. It shows how to configure the request body for transferring a domain, including specifying the domain name, transfer method, auth code, and expected price.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/domains/README.md#_snippet_14

LANGUAGE: typescript
CODE:
```
import { Vercel } from "@vercel/sdk";

const vercel = new Vercel({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const result = await vercel.domains.createOrTransferDomain({
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
    slug: "my-team-url-slug",
    requestBody: {
      name: "example.com",
      method: "transfer-in",
      authCode: "fdhfr820ad#@FAdlj$$",
      expectedPrice: 8,
    },
  });

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Reading Access Group using Standalone Function (TypeScript)
DESCRIPTION: Shows how to read an access group using the standalone `accessGroupsReadAccessGroup` function from the core SDK module. It requires initializing `VercelCore` and passing the instance along with the access group details (ID or name, team ID, slug) to the function. It includes error handling and logs the result. This method is recommended for better tree-shaking.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/accessgroups/README.md#_snippet_1

LANGUAGE: typescript
CODE:
```
import { VercelCore } from "@vercel/sdk/core.js";
import { accessGroupsReadAccessGroup } from "@vercel/sdk/funcs/accessGroupsReadAccessGroup.js";

// Use `VercelCore` for best tree-shaking performance.
// You can create one instance of it to use across an application.
const vercel = new VercelCore({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const res = await accessGroupsReadAccessGroup(vercel, {
    idOrName: "ag_1a2b3c4d5e6f7g8h9i0j",
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
    slug: "my-team-url-slug",
  });

  if (!res.ok) {
    throw res.error;
  }

  const { value: result } = res;

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Updating DNS Record with Vercel SDK (Class Instance) - TypeScript
DESCRIPTION: Demonstrates updating a DNS record using the Vercel class instance from the @vercel/sdk. It shows how to initialize the SDK with a bearer token and call the dns.updateRecord method with the record ID, team details, and the updated record body.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/dns/README.md#_snippet_4

LANGUAGE: typescript
CODE:
```
import { Vercel } from "@vercel/sdk";

const vercel = new Vercel({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const result = await vercel.dns.updateRecord({
    recordId: "rec_2qn7pzrx89yxy34vezpd31y9",
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
    slug: "my-team-url-slug",
    requestBody: {
      name: "example-1",
      value: "google.com",
      type: "A",
      ttl: 60,
      srv: {
        target: "example2.com.",
        weight: 97604,
        port: 570172,
        priority: 199524,
      },
      https: {
        priority: 35000,
        target: "example2.com.",
      },
      comment: "used to verify ownership of domain",
    },
  });

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Creating Auth Token using Vercel Class (TypeScript)
DESCRIPTION: This snippet demonstrates how to create a new authentication token using an instance of the Vercel class. It shows how to instantiate the class with a bearer token and call the `authentication.createAuthToken` method with required parameters like team ID, slug, and request body.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/authentication/README.md#_snippet_4

LANGUAGE: typescript
CODE:
```
import { Vercel } from "@vercel/sdk";

const vercel = new Vercel({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const result = await vercel.authentication.createAuthToken({
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
    slug: "my-team-url-slug",
    requestBody: {
      name: "<value>",
    },
  });

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Retrieving Vercel Alias using Standalone Function (TypeScript)
DESCRIPTION: Illustrates how to retrieve a Vercel Alias using the standalone `aliasesGetAlias` function. This method is recommended for better tree-shaking and requires passing a `VercelCore` instance and an options object containing the alias details.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/aliases/README.md#_snippet_7

LANGUAGE: typescript
CODE:
```
import { VercelCore } from "@vercel/sdk/core.js";
import { aliasesGetAlias } from "@vercel/sdk/funcs/aliasesGetAlias.js";

// Use `VercelCore` for best tree-shaking performance.
// You can create one instance of it to use across an application.
const vercel = new VercelCore({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>"
});

async function run() {
  const res = await aliasesGetAlias(vercel, {
    from: 1540095775951,
    idOrAlias: "example.vercel.app",
    projectId: "prj_12HKQaOmR5t5Uy6vdcQsNIiZgHGB",
    since: 1540095775941,
    until: 1540095775951,
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
    slug: "my-team-url-slug"
  });

  if (!res.ok) {
    throw res.error;
  }

  const { value: result } = res;

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Exchange SSO Token using Standalone Function (TypeScript)
DESCRIPTION: Shows how to use the standalone `marketplaceExchangeSsoToken` function with a `VercelCore` instance. This approach is recommended for better tree-shaking performance. It performs the same SSO token exchange as the method on the main class, requiring the core instance, authorization code, client ID, and client secret. Includes error handling.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/marketplace/README.md#_snippet_23

LANGUAGE: TypeScript
CODE:
```
import { VercelCore } from "@vercel/sdk/core.js";
import { marketplaceExchangeSsoToken } from "@vercel/sdk/funcs/marketplaceExchangeSsoToken.js";

// Use `VercelCore` for best tree-shaking performance.
// You can create one instance of it to use across an application.
const vercel = new VercelCore();

async function run() {
  const res = await marketplaceExchangeSsoToken(vercel, {
    code: "<value>",
    clientId: "<id>",
    clientSecret: "<value>",
  });

  if (!res.ok) {
    throw res.error;
  }

  const { value: result } = res;

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Exchange SSO Token using Standalone Function (TypeScript)
DESCRIPTION: Shows how to perform the SSO token exchange using the standalone `authenticationExchangeSsoToken` function with a `VercelCore` instance for better tree-shaking. This method also requires the authorization code, client ID, and client secret, and includes error handling.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/authentication/README.md#_snippet_1

LANGUAGE: TypeScript
CODE:
```
import { VercelCore } from "@vercel/sdk/core.js";
import { authenticationExchangeSsoToken } from "@vercel/sdk/funcs/authenticationExchangeSsoToken.js";

// Use \`VercelCore\` for best tree-shaking performance.
// You can create one instance of it to use across an application.
const vercel = new VercelCore();

async function run() {
  const res = await authenticationExchangeSsoToken(vercel, {
    code: "<value>",
    clientId: "<id>",
    clientSecret: "<value>"
  });

  if (!res.ok) {
    throw res.error;
  }

  const { value: result } = res;

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Update Custom Environment using Standalone Function - TypeScript
DESCRIPTION: Illustrates how to update a custom environment using the standalone `environmentUpdateCustomEnvironment` function from the SDK. It utilizes `VercelCore` for better tree-shaking and includes error handling for the function's result.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/environment/README.md#_snippet_7

LANGUAGE: typescript
CODE:
```
import { VercelCore } from "@vercel/sdk/core.js";
import { environmentUpdateCustomEnvironment } from "@vercel/sdk/funcs/environmentUpdateCustomEnvironment.js";

// Use `VercelCore` for best tree-shaking performance.
// You can create one instance of it to use across an application.
const vercel = new VercelCore({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const res = await environmentUpdateCustomEnvironment(vercel, {
    idOrName: "<value>",
    environmentSlugOrId: "<id>",
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
    slug: "my-team-url-slug",
  });

  if (!res.ok) {
    throw res.error;
  }

  const { value: result } = res;

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Initializing Secrets Object in TypeScript
DESCRIPTION: This snippet demonstrates how to import the `Secrets` type from the Vercel SDK and initialize an object of this type. The `Secrets` type is used to represent a secret with required `name` and `value` string properties.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/models/secrets.md#_snippet_0

LANGUAGE: TypeScript
CODE:
```
import { Secrets } from "@vercel/sdk/models/updateintegrationdeploymentactionop.js";

let value: Secrets = {
  name: "<value>",
  value: "<value>"
};
```

----------------------------------------

TITLE: Delete Team using Vercel SDK Class
DESCRIPTION: Demonstrates how to delete a team using the deleteTeam method on an instance of the Vercel class. This requires initializing the Vercel client with a bearer token and providing the team identifier (ID or slug) and an optional new default team ID.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/teams/README.md#_snippet_22

LANGUAGE: typescript
CODE:
```
import { Vercel } from "@vercel/sdk";

const vercel = new Vercel({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const result = await vercel.teams.deleteTeam({
    newDefaultTeamId: "team_LLHUOMOoDlqOp8wPE4kFo9pE",
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
    slug: "my-team-url-slug",
    requestBody: {},
  });

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Possible values for Role type (TypeScript)
DESCRIPTION: Lists the allowed string literal values that the `Role` type can hold. This represents a union type definition.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/models/role.md#_snippet_1

LANGUAGE: TypeScript
CODE:
```
"OWNER" | "MEMBER" | "DEVELOPER" | "SECURITY" | "BILLING" | "VIEWER" | "CONTRIBUTOR"
```

----------------------------------------

TITLE: Retrieve Firewall Config using Vercel Class (TypeScript)
DESCRIPTION: This snippet demonstrates how to retrieve a firewall configuration for a Vercel project using the `getFirewallConfig` method on an instance of the `Vercel` class. It requires importing the class, creating an instance with a bearer token, and providing the project ID, team ID, slug, and config version.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/security/README.md#_snippet_6

LANGUAGE: typescript
CODE:
```
import { Vercel } from "@vercel/sdk";

const vercel = new Vercel({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const result = await vercel.security.getFirewallConfig({
    projectId: "<id>",
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
    slug: "my-team-url-slug",
    configVersion: "<value>",
  });

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Getting Edge Configs using Vercel SDK Class (TypeScript)
DESCRIPTION: Demonstrates how to fetch all Vercel Edge Configs using the primary Vercel SDK class instance. Requires a bearer token for authentication and accepts optional team ID and slug parameters.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/edgeconfig/README.md#_snippet_0

LANGUAGE: typescript
CODE:
```
import { Vercel } from "@vercel/sdk";

const vercel = new Vercel({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const result = await vercel.edgeConfig.getEdgeConfigs({
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
    slug: "my-team-url-slug",
  });

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Creating RemoveRecordRequest Object in TypeScript
DESCRIPTION: This snippet demonstrates how to import the `RemoveRecordRequest` type and create an instance of it in TypeScript. It shows how to populate the required `domain` and `recordId` fields, as well as the optional `teamId` and `slug` fields, preparing the object for use in a record removal API call.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/models/removerecordrequest.md#_snippet_0

LANGUAGE: typescript
CODE:
```
import { RemoveRecordRequest } from "@vercel/sdk/models/removerecordop.js";

let value: RemoveRecordRequest = {
  domain: "example.com",
  recordId: "rec_V0fra8eEgQwEpFhYG2vTzC3K",
  teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
  slug: "my-team-url-slug"
};
```

----------------------------------------

TITLE: Inviting User to Team using Standalone Function (TypeScript)
DESCRIPTION: Shows how to invite a user using the standalone `teamsInviteUserToTeam` function from the core SDK. It illustrates initializing `VercelCore` for better tree-shaking and calling the function with the core instance and parameters. It also includes basic error handling.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/teams/README.md#_snippet_3

LANGUAGE: TypeScript
CODE:
```
import { VercelCore } from "@vercel/sdk/core.js";
import { teamsInviteUserToTeam } from "@vercel/sdk/funcs/teamsInviteUserToTeam.js";

// Use \`VercelCore\` for best tree-shaking performance.
// You can create one instance of it to use across an application.
const vercel = new VercelCore({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const res = await teamsInviteUserToTeam(vercel, {
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
    requestBody: {
      uid: "kr1PsOIzqEL5Xg6M4VZcZosf",
      email: "john@example.com",
      projects: [
        {
          projectId: "prj_ndlgr43fadlPyCtREAqxxdyFK",
          role: "ADMIN"
        },
        {
          projectId: "prj_ndlgr43fadlPyCtREAqxxdyFK",
          role: "ADMIN"
        }
      ]
    }
  });

  if (!res.ok) {
    throw res.error;
  }

  const { value: result } = res;

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Initializing CreateProjectGitProviderOptions in TypeScript
DESCRIPTION: This snippet demonstrates how to create an instance of the CreateProjectGitProviderOptions type and set the 'createDeployments' field. The 'createDeployments' field controls whether the Vercel bot automatically creates GitHub deployments, although repository-dispatch events are recommended instead.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/models/createprojectgitprovideroptions.md#_snippet_0

LANGUAGE: typescript
CODE:
```
import { CreateProjectGitProviderOptions } from "@vercel/sdk/models/createprojectop.js";

let value: CreateProjectGitProviderOptions = {
  createDeployments: "disabled",
};
```

----------------------------------------

TITLE: Creating ResponseBodyProject Instance in TypeScript
DESCRIPTION: This snippet demonstrates how to import and create a basic instance of the ResponseBodyProject type, showing the required 'id' and 'name' fields.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/models/responsebodyproject.md#_snippet_0

LANGUAGE: TypeScript
CODE:
```
import { ResponseBodyProject } from "@vercel/sdk/models/getdeploymentop.js";

let value: ResponseBodyProject = {
  id: "<id>",
  name: "<value>"
};
```

----------------------------------------

TITLE: Defining FilterProjectEnvsTargetProjectsResponse1 Array in TypeScript
DESCRIPTION: This snippet shows how to declare and initialize a variable of type `models.FilterProjectEnvsTargetProjectsResponse1[]`, representing a list of target environments, with an array containing the string "production".
SOURCE: https://github.com/vercel/sdk/blob/main/docs/models/filterprojectenvsresponsebodyprojectsresponsetarget.md#_snippet_0

LANGUAGE: typescript
CODE:
```
const value: models.FilterProjectEnvsTargetProjectsResponse1[] = [
  "production",
];
```

----------------------------------------

TITLE: Instantiating ListPromoteAliasesRequest Object in TypeScript
DESCRIPTION: This snippet demonstrates how to create an instance of the ListPromoteAliasesRequest type from the @vercel/sdk package. It shows how to import the type and initialize an object with example values for various fields like projectId, limit, since, until, teamId, and slug.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/models/listpromotealiasesrequest.md#_snippet_0

LANGUAGE: typescript
CODE:
```
import { ListPromoteAliasesRequest } from "@vercel/sdk/models/listpromotealiasesop.js";

let value: ListPromoteAliasesRequest = {
  projectId: "<id>",
  limit: 20,
  since: 1609499532000,
  until: 1612264332000,
  teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
  slug: "my-team-url-slug",
};
```

----------------------------------------

TITLE: Instantiating GetEdgeConfigSchemaRequest in TypeScript
DESCRIPTION: This snippet demonstrates how to create an instance of the `GetEdgeConfigSchemaRequest` type in TypeScript. It shows how to import the type and initialize an object with example values for `edgeConfigId`, `teamId`, and `slug`.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/models/getedgeconfigschemarequest.md#_snippet_0

LANGUAGE: typescript
CODE:
```
import { GetEdgeConfigSchemaRequest } from "@vercel/sdk/models/getedgeconfigschemaop.js";

let value: GetEdgeConfigSchemaRequest = {
  edgeConfigId: "<id>",
  teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
  slug: "my-team-url-slug",
};
```

----------------------------------------

TITLE: Update Firewall Config - Enable/Disable (Type 1)
DESCRIPTION: This request body type is used to enable or disable the firewall feature entirely. It specifies the 'firewallEnabled' action and a boolean value.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/models/updatefirewallconfigrequestbody.md#_snippet_0

LANGUAGE: typescript
CODE:
```
const value: models.UpdateFirewallConfigRequestBody1 = {
  action: "firewallEnabled",
  value: false,
};
```

----------------------------------------

TITLE: Instantiating GetDeploymentGitSourceDeployments11 Type (TypeScript)
DESCRIPTION: Demonstrates how to create an instance of the `GetDeploymentGitSourceDeployments11` type, showing the required fields and their expected data types. This example initializes the type with placeholder values for `ref`, `sha`, and `projectId`, and sets the `type` to 'gitlab'.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/models/getdeploymentgitsourcedeployments11.md#_snippet_0

LANGUAGE: typescript
CODE:
```
import { GetDeploymentGitSourceDeployments11 } from "@vercel/sdk/models/getdeploymentop.js";

let value: GetDeploymentGitSourceDeployments11 = {
  type: "gitlab",
  ref: "<value>",
  sha: "<value>",
  projectId: 3996.33,
};
```

----------------------------------------

TITLE: Initializing GitSource2 in TypeScript
DESCRIPTION: Demonstrates how to import and create an instance of the GitSource2 type, populating it with example values for a GitHub source. Requires importing GitSource2 from the @vercel/sdk models.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/models/gitsource2.md#_snippet_0

LANGUAGE: TypeScript
CODE:
```
import { GitSource2 } from "@vercel/sdk/models/createdeploymentop.js";

let value: GitSource2 = {
  org: "vercel",
  ref: "main",
  repo: "next.js",
  sha: "a1b2c3d4e5f6g7h8i9j0k1l2m3n4o5p6q7r8s9t0",
  type: "github"
};
```

----------------------------------------

TITLE: Delete Deployment using Standalone Function (TypeScript)
DESCRIPTION: Shows how to delete a Vercel deployment using the standalone deploymentsDeleteDeployment function, which is suitable for better tree-shaking. It requires a VercelCore instance and an options object containing parameters like 'id', 'url', 'teamId', or 'slug' to identify the deployment. Includes basic error handling.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/deployments/README.md#_snippet_19

LANGUAGE: TypeScript
CODE:
```
import { VercelCore } from "@vercel/sdk/core.js";
import { deploymentsDeleteDeployment } from "@vercel/sdk/funcs/deploymentsDeleteDeployment.js";

// Use `VercelCore` for best tree-shaking performance.
// You can create one instance of it to use across an application.
const vercel = new VercelCore({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const res = await deploymentsDeleteDeployment(vercel, {
    id: "dpl_5WJWYSyB7BpgTj3EuwF37WMRBXBtPQ2iTMJHJBJyRfd",
    url: "https://files-orcin-xi.vercel.app/",
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
    slug: "my-team-url-slug",
  });

  if (!res.ok) {
    throw res.error;
  }

  const { value: result } = res;

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Instantiating models.FilterProjectEnvsContentHintProjects1 (redis-url) in TypeScript
DESCRIPTION: Demonstrates how to create an instance of the `models.FilterProjectEnvsContentHintProjects1` type, representing a Redis URL content hint. This type requires specifying the `type` as 'redis-url' and providing a `storeId`.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/models/filterprojectenvsresponsebodycontenthint.md#_snippet_0

LANGUAGE: typescript
CODE:
```
const value: models.FilterProjectEnvsContentHintProjects1 = {
  type: "redis-url",
  storeId: "<id>",
};
```

----------------------------------------

TITLE: Fetching Integration Log Drains with Standalone Function (TypeScript)
DESCRIPTION: This snippet shows how to fetch integration log drains using the standalone `logDrainsGetIntegrationLogDrains` function from `@vercel/sdk/funcs`. This method is recommended for better tree-shaking and requires passing an instance of `VercelCore` and the options object.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/logdrains/README.md#_snippet_3

LANGUAGE: typescript
CODE:
```
import { VercelCore } from "@vercel/sdk/core.js";
import { logDrainsGetIntegrationLogDrains } from "@vercel/sdk/funcs/logDrainsGetIntegrationLogDrains.js";

// Use \`VercelCore\` for best tree-shaking performance.
// You can create one instance of it to use across an application.
const vercel = new VercelCore({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const res = await logDrainsGetIntegrationLogDrains(vercel, {
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
    slug: "my-team-url-slug",
  });

  if (!res.ok) {
    throw res.error;
  }

  const { value: result } = res;

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Deleting Edge Config Schema using Vercel SDK (Standalone Function) - TypeScript
DESCRIPTION: Shows how to delete an Edge Config schema using the standalone `edgeConfigDeleteEdgeConfigSchema` function from `@vercel/sdk/funcs`. This approach uses `VercelCore` for better tree-shaking and involves passing the core instance and parameters to the function. Includes basic error handling.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/edgeconfig/README.md#_snippet_17

LANGUAGE: typescript
CODE:
```
import { VercelCore } from "@vercel/sdk/core.js";
import { edgeConfigDeleteEdgeConfigSchema } from "@vercel/sdk/funcs/edgeConfigDeleteEdgeConfigSchema.js";

// Use `VercelCore` for best tree-shaking performance.
// You can create one instance of it to use across an application.
const vercel = new VercelCore({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const res = await edgeConfigDeleteEdgeConfigSchema(vercel, {
    edgeConfigId: "<id>",
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
    slug: "my-team-url-slug",
  });

  if (!res.ok) {
    throw res.error;
  }

  const { value: result } = res;

  
}

run();
```

----------------------------------------

TITLE: Defining GetDeploymentsTarget Union Type in TypeScript
DESCRIPTION: Illustrates the union type definition for GetDeploymentsTarget, showing that its value must be one of the string literals 'production' or 'staging', defining the allowed environments.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/models/getdeploymentstarget.md#_snippet_1

LANGUAGE: typescript
CODE:
```
"production" | "staging"
```

----------------------------------------

TITLE: Updating Vercel Team using Vercel Class (TypeScript)
DESCRIPTION: Demonstrates how to update a Vercel Team's information using the `patchTeam` method on an instance of the main `Vercel` class. It requires initializing the class with a bearer token and then calling the method on the `teams` property.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/teams/README.md#_snippet_16

LANGUAGE: typescript
CODE:
```
import { Vercel } from "@vercel/sdk";

const vercel = new Vercel({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const result = await vercel.teams.patchTeam({
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
    slug: "my-team-url-slug",
    requestBody: {
      description: "Our mission is to make cloud computing accessible to everyone",
      emailDomain: "example.com",
      name: "My Team",
      previewDeploymentSuffix: "example.dev",
      regenerateInviteCode: true,
      saml: {
        enforced: true,
      },
      slug: "my-team",
      enablePreviewFeedback: "on",
      enableProductionFeedback: "on",
      sensitiveEnvironmentVariablePolicy: "on",
      remoteCaching: {
        enabled: true,
      },
      hideIpAddresses: false,
      hideIpAddressesInLogDrains: false,
    },
  });

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Get Edge Config Token using Vercel Class (TypeScript)
DESCRIPTION: Demonstrates how to initialize the Vercel SDK using the main Vercel class with a bearer token and call the edgeConfig.getEdgeConfigToken method to retrieve an Edge Config token. It shows how to provide necessary parameters like edgeConfigId, token, teamId, and slug, and how to handle the asynchronous result.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/edgeconfig/README.md#_snippet_24

LANGUAGE: typescript
CODE:
```
import { Vercel } from "@vercel/sdk";

const vercel = new Vercel({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const result = await vercel.edgeConfig.getEdgeConfigToken({
    edgeConfigId: "<id>",
    token: "<value>",
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
    slug: "my-team-url-slug",
  });

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Getting a Vercel Check using Vercel Class (TypeScript)
DESCRIPTION: This snippet demonstrates how to retrieve details for a specific Vercel check using the `Vercel` class instance. It requires initializing the class with a bearer token and then calling the `checks.getCheck` method with the deployment ID, check ID, and optionally team ID or slug.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/checks/README.md#_snippet_4

LANGUAGE: typescript
CODE:
```
import { Vercel } from "@vercel/sdk";

const vercel = new Vercel({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const result = await vercel.checks.getCheck({
    deploymentId: "dpl_2qn7PZrx89yxY34vEZPD31Y9XVj6",
    checkId: "check_2qn7PZrx89yxY34vEZPD31Y9XVj6",
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
    slug: "my-team-url-slug",
  });

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Creating UpdateProjectDefinitions Instance (TypeScript)
DESCRIPTION: This snippet demonstrates how to import the `UpdateProjectDefinitions` type from the Vercel SDK and create an instance of it. It shows the required fields (`host`, `path`, `schedule`) populated with example values for defining a cron job.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/models/updateprojectdefinitions.md#_snippet_0

LANGUAGE: typescript
CODE:
```
import { UpdateProjectDefinitions } from "@vercel/sdk/models/updateprojectop.js";

let value: UpdateProjectDefinitions = {
  host: "vercel.com",
  path: "/api/crons/sync-something?hello=world",
  schedule: "0 0 * * *"
};
```

----------------------------------------

TITLE: Initializing Vercel SDK RequestPromoteRequest in TypeScript
DESCRIPTION: This snippet demonstrates how to create an instance of the RequestPromoteRequest type from the Vercel SDK. It shows how to import the type and initialize an object with required and optional fields like projectId, deploymentId, teamId, and slug.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/models/requestpromoterequest.md#_snippet_0

LANGUAGE: typescript
CODE:
```
import { RequestPromoteRequest } from "@vercel/sdk/models/requestpromoteop.js";

let value: RequestPromoteRequest = {
  projectId: "<id>",
  deploymentId: "<id>",
  teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
  slug: "my-team-url-slug"
};
```

----------------------------------------

TITLE: Creating a Check using Vercel SDK Class (TypeScript)
DESCRIPTION: Demonstrates how to create a new Vercel Check using the main Vercel class instance. It shows how to initialize the SDK with a bearer token and call the checks.createCheck method with required parameters like deployment ID, team ID, slug, and the check's configuration details in the request body.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/checks/README.md#_snippet_0

LANGUAGE: typescript
CODE:
```
import { Vercel } from "@vercel/sdk";

const vercel = new Vercel({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const result = await vercel.checks.createCheck({
    deploymentId: "dpl_2qn7PZrx89yxY34vEZPD31Y9XVj6",
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
    slug: "my-team-url-slug",
    requestBody: {
      name: "Performance Check",
      path: "/",
      blocking: true,
      detailsUrl: "http://example.com",
      externalId: "1234abc",
      rerequestable: true,
    },
  });

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Initializing EdgeRequest in TypeScript
DESCRIPTION: This snippet demonstrates how to import the `EdgeRequest` type from the `@vercel/sdk` and initialize a variable with an object conforming to the `EdgeRequest` structure. It shows setting the required `currentThreshold` field.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/models/edgerequest.md#_snippet_0

LANGUAGE: typescript
CODE:
```
import { EdgeRequest } from "@vercel/sdk/models/userevent.js";

let value: EdgeRequest = {
  currentThreshold: 4078.48,
};
```

----------------------------------------

TITLE: Remove Project Member using Standalone Function - TypeScript
DESCRIPTION: Illustrates how to remove a project member using the standalone `projectMembersRemoveProjectMember` function with a `VercelCore` instance for better tree-shaking. Includes error handling.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/projectmembers/README.md#_snippet_5

LANGUAGE: typescript
CODE:
```
import { VercelCore } from "@vercel/sdk/core.js";
import { projectMembersRemoveProjectMember } from "@vercel/sdk/funcs/projectMembersRemoveProjectMember.js";

// Use `VercelCore` for best tree-shaking performance.
// You can create one instance of it to use across an application.
const vercel = new VercelCore({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const res = await projectMembersRemoveProjectMember(vercel, {
    idOrName: "prj_pavWOn1iLObbXLRiwVvzmPrTWyTf",
    uid: "ndlgr43fadlPyCtREAqxxdyFK",
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
    slug: "my-team-url-slug",
  });

  if (!res.ok) {
    throw res.error;
  }

  const { value: result } = res;

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Initialize UpdateProjectContentHint14 for Integration Store Secret (TypeScript)
DESCRIPTION: This snippet demonstrates how to create an instance of the `UpdateProjectContentHint14` type with the `integration-store-secret` type. It requires importing the type from `@vercel/sdk/models/updateprojectop.js` and provides the necessary fields (`storeId`, `integrationId`, `integrationProductId`, `integrationConfigurationId`) for this specific hint type.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/models/updateprojectcontenthint14.md#_snippet_0

LANGUAGE: typescript
CODE:
```
import { UpdateProjectContentHint14 } from "@vercel/sdk/models/updateprojectop.js";

let value: UpdateProjectContentHint14 = {
  type: "integration-store-secret",
  storeId: "<id>",
  integrationId: "<id>",
  integrationProductId: "<id>",
  integrationConfigurationId: "<id>"
};
```

----------------------------------------

TITLE: List DNS Records using Vercel SDK Instance (TypeScript)
DESCRIPTION: Demonstrates how to retrieve DNS records for a domain using the `getRecords` method on a `Vercel` SDK instance. It shows how to initialize the SDK with a bearer token and pass parameters like domain, limit, time range, and team identifiers.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/dns/README.md#_snippet_0

LANGUAGE: TypeScript
CODE:
```
import { Vercel } from "@vercel/sdk";

const vercel = new Vercel({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const result = await vercel.dns.getRecords({
    domain: "example.com",
    limit: "20",
    since: "1609499532000",
    until: "1612264332000",
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
    slug: "my-team-url-slug",
  });

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Update Project Protection Bypass using Vercel SDK Class (TypeScript)
DESCRIPTION: Demonstrates how to update the deployment protection bypass for a Vercel project using the main `Vercel` class instance. It initializes the SDK with a bearer token and calls the `projects.updateProjectProtectionBypass` method with required parameters.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/projects/README.md#_snippet_38

LANGUAGE: typescript
CODE:
```
import { Vercel } from "@vercel/sdk";

const vercel = new Vercel({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const result = await vercel.projects.updateProjectProtectionBypass({
    idOrName: "<value>",
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
    slug: "my-team-url-slug",
    requestBody: {},
  });

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Initializing GetEdgeConfigBackupRequest in TypeScript
DESCRIPTION: This snippet demonstrates how to create an instance of the GetEdgeConfigBackupRequest object in TypeScript, populating it with required and optional fields for making a request.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/models/getedgeconfigbackuprequest.md#_snippet_0

LANGUAGE: typescript
CODE:
```
import { GetEdgeConfigBackupRequest } from "@vercel/sdk/models/getedgeconfigbackupop.js";

let value: GetEdgeConfigBackupRequest = {
  edgeConfigId: "<id>",
  edgeConfigBackupVersionId: "<id>",
  teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
  slug: "my-team-url-slug"
};
```

----------------------------------------

TITLE: Creating CreateDeploymentRequestBody Instance (TypeScript)
DESCRIPTION: This snippet shows how to create an instance of the `CreateDeploymentRequestBody` type, populating it with example data for various fields required to define a deployment request.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/models/createdeploymentrequestbody.md#_snippet_0

LANGUAGE: TypeScript
CODE:
```
import { CreateDeploymentRequestBody } from "@vercel/sdk/models/createdeploymentop.js";

let value: CreateDeploymentRequestBody = {
  deploymentId: "dpl_2qn7PZrx89yxY34vEZPD31Y9XVj6",
  files: [
    {
      file: "folder/file.js"
    }
  ],
  gitMetadata: {
    remoteUrl: "https://github.com/vercel/next.js",
    commitAuthorName: "kyliau",
    commitMessage:
      "add method to measure Interaction to Next Paint (INP) (#36490)",
    commitRef: "main",
    commitSha: "dc36199b2234c6586ebe05ec94078a895c707e29",
    dirty: true
  },
  gitSource: {
    org: "vercel",
    ref: "main",
    repo: "next.js",
    sha: "a1b2c3d4e5f6g7h8i9j0k1l2m3n4o5p6q7r8s9t0",
    host: "electric-minority.name",
    type: "github-custom-host"
  },
  meta: {
    "foo": "bar"
  },
  name: "my-instant-deployment",
  project: "my-deployment-project",
  projectSettings: {
    buildCommand: "next build",
    installCommand: "pnpm install"
  },
  target: "production"
};
```

----------------------------------------

TITLE: Creating an UploadedFile Model (TypeScript)
DESCRIPTION: Demonstrates how to create an instance of the `models.UploadedFile` type. This model represents a file that is referenced by its path, implying it will be uploaded separately.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/models/files.md#_snippet_1

LANGUAGE: typescript
CODE:
```
const value: models.UploadedFile = {
  file: "folder/file.js"
};
```

----------------------------------------

TITLE: Initializing GetProjectEnvResponseBody1 Type in TypeScript
DESCRIPTION: This snippet shows how to import the GetProjectEnvResponseBody1 type and create an instance of it with example values for its properties: `decrypted`, `type`, and `key`. This demonstrates the basic structure and usage of the type.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/models/getprojectenvresponsebody1.md#_snippet_0

LANGUAGE: typescript
CODE:
```
import { GetProjectEnvResponseBody1 } from "@vercel/sdk/models/getprojectenvop.js";

let value: GetProjectEnvResponseBody1 = {
  decrypted: false,
  type: "secret",
  key: "<key>",
};
```

----------------------------------------

TITLE: Update Team Member using Vercel SDK Class (TypeScript)
DESCRIPTION: This snippet shows how to update a team member's details using the `Vercel` class instance. It requires importing the `Vercel` class from `@vercel/sdk`. The `updateTeamMember` method is called on the `teams` property of the Vercel instance, requiring the team member's UID, the team ID, and a request body specifying the updates (e.g., confirmation status, project roles).
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/teams/README.md#_snippet_10

LANGUAGE: typescript
CODE:
```
import { Vercel } from "@vercel/sdk";

const vercel = new Vercel({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const result = await vercel.teams.updateTeamMember({
    uid: "ndfasllgPyCtREAqxxdyFKb",
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
    requestBody: {
      confirmed: true,
      projects: [
        {
          projectId: "prj_ndlgr43fadlPyCtREAqxxdyFK",
          role: "ADMIN"
        },
        {
          projectId: "prj_ndlgr43fadlPyCtREAqxxdyFK",
          role: "ADMIN"
        }
      ]
    }
  });

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Initializing OneHundredAndThirtyOne Model (TypeScript)
DESCRIPTION: This snippet shows how to initialize an instance of the `models.OneHundredAndThirtyOne` model. It includes nested properties for project name, role, and invited user name.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/models/payload.md#_snippet_130

LANGUAGE: typescript
CODE:
```
const value: models.OneHundredAndThirtyOne = {
  project: {
    name: "<value>",
    role: "PROJECT_VIEWER",
    invitedUserName: "<value>"
  }
};
```

----------------------------------------

TITLE: Creating A Record Request Body (RequestBody1) - TypeScript
DESCRIPTION: Demonstrates the structure for a request body to create a DNS A record using models.RequestBody1. It includes common fields like name, type, ttl, value, and comment.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/models/createrecordrequestbody.md#_snippet_0

LANGUAGE: typescript
CODE:
```
const value: models.RequestBody1 = {
  name: "subdomain",
  type: "A",
  ttl: 60,
  value: "192.0.2.42",
  comment: "used to verify ownership of domain"
};
```

----------------------------------------

TITLE: Initializing PutFirewallConfigCrs Object in TypeScript
DESCRIPTION: This snippet shows how to create an instance of the `PutFirewallConfigCrs` type. It defines the configuration for various custom rulesets (like sd, ma, lfi, etc.), specifying whether each is active and the action to take (e.g., 'log', 'deny'). This object is used to configure the firewall's custom ruleset behavior.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/models/putfirewallconfigcrs.md#_snippet_0

LANGUAGE: typescript
CODE:
```
import { PutFirewallConfigCrs } from "@vercel/sdk/models/putfirewallconfigop.js";

let value: PutFirewallConfigCrs = {
  sd: {
    active: false,
    action: "log"
  },
  ma: {
    active: false,
    action: "log"
  },
  lfi: {
    active: false,
    action: "deny"
  },
  rfi: {
    active: false,
    action: "deny"
  },
  rce: {
    active: false,
    action: "log"
  },
  php: {
    active: false,
    action: "log"
  },
  gen: {
    active: false,
    action: "deny"
  },
  xss: {
    active: false,
    action: "log"
  },
  sqli: {
    active: false,
    action: "log"
  },
  sf: {
    active: false,
    action: "log"
  },
  java: {
    active: false,
    action: "log"
  }
};
```

----------------------------------------

TITLE: Get Authentication Token using Standalone Function (TypeScript)
DESCRIPTION: Shows how to use the standalone `authenticationGetAuthToken` function with a `VercelCore` instance for better tree-shaking. It retrieves an authentication token, handles potential errors, and logs the successful result. Requires `@vercel/sdk/core.js` and `@vercel/sdk/funcs/authenticationGetAuthToken.js`.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/authentication/README.md#_snippet_7

LANGUAGE: typescript
CODE:
```
import { VercelCore } from "@vercel/sdk/core.js";
import { authenticationGetAuthToken } from "@vercel/sdk/funcs/authenticationGetAuthToken.js";

// Use `VercelCore` for best tree-shaking performance.
// You can create one instance of it to use across an application.
const vercel = new VercelCore({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>"
});

async function run() {
  const res = await authenticationGetAuthToken(vercel, {
    tokenId: "5d9f2ebd38ddca62e5d51e9c1704c72530bdc8bfdd41e782a6687c48399e8391"
  });

  if (!res.ok) {
    throw res.error;
  }

  const { value: result } = res;

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Get Edge Config Tokens using Vercel SDK Class - TypeScript
DESCRIPTION: Demonstrates how to fetch Edge Config tokens using an instance of the main `Vercel` class from the SDK. Requires importing `Vercel` and providing a bearer token during initialization. The `getEdgeConfigTokens` method is called on the `edgeConfig` property of the instance, requiring `edgeConfigId`, `teamId`, or `slug`.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/edgeconfig/README.md#_snippet_20

LANGUAGE: typescript
CODE:
```
import { Vercel } from "@vercel/sdk";

const vercel = new Vercel({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const result = await vercel.edgeConfig.getEdgeConfigTokens({
    edgeConfigId: "<id>",
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
    slug: "my-team-url-slug",
  });

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Get Team Info using Vercel Class - TypeScript
DESCRIPTION: Demonstrates how to fetch team information using the `getTeam` method available on an instance of the main `Vercel` class. It requires initializing `Vercel` with a bearer token and then calling `teams.getTeam` with the team's slug and ID.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/teams/README.md#_snippet_14

LANGUAGE: typescript
CODE:
```
import { Vercel } from "@vercel/sdk";

const vercel = new Vercel({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const result = await vercel.teams.getTeam({
    slug: "my-team-url-slug",
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
  });

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Uploading Certificate using Vercel SDK Standalone Function (TypeScript)
DESCRIPTION: This example illustrates uploading an SSL certificate using the standalone `certsUploadCert` function, which is recommended for better tree-shaking. It utilizes a `VercelCore` instance for authentication and passes the instance along with team ID, slug, and certificate details (ca, key, cert) to the function. It includes basic error handling and logs the successful result.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/certs/README.md#_snippet_7

LANGUAGE: typescript
CODE:
```
import { VercelCore } from "@vercel/sdk/core.js";
import { certsUploadCert } from "@vercel/sdk/funcs/certsUploadCert.js";

// Use `VercelCore` for best tree-shaking performance.
// You can create one instance of it to use across an application.
const vercel = new VercelCore({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const res = await certsUploadCert(vercel, {
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
    slug: "my-team-url-slug",
    requestBody: {
      ca: "<value>",
      key: "<key>",
      cert: "<value>",
    },
  });

  if (!res.ok) {
    throw res.error;
  }

  const { value: result } = res;

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Creating DeleteConfigurationRequest Object (TypeScript)
DESCRIPTION: This snippet demonstrates how to import the DeleteConfigurationRequest type from the Vercel SDK and initialize an object with example values for its required 'id' field and optional 'teamId' or 'slug' fields.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/models/deleteconfigurationrequest.md#_snippet_0

LANGUAGE: typescript
CODE:
```
import { DeleteConfigurationRequest } from "@vercel/sdk/models/deleteconfigurationop.js";

let value: DeleteConfigurationRequest = {
  id: "<id>",
  teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
  slug: "my-team-url-slug"
};
```

----------------------------------------

TITLE: Example Usage of PatchV1InstallationsIntegrationConfigurationIdResourcesResourceIdExperimentationItemsItemIdRequest in TypeScript
DESCRIPTION: This snippet shows how to import the PatchV1InstallationsIntegrationConfigurationIdResourcesResourceIdExperimentationItemsItemIdRequest type and create an instance of it, providing placeholder values for the required properties: integrationConfigurationId, resourceId, and itemId.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/models/patchv1installationsintegrationconfigurationidresourcesresourceidexperimentationitemsitemidrequest.md#_snippet_0

LANGUAGE: typescript
CODE:
```
import {
  PatchV1InstallationsIntegrationConfigurationIdResourcesResourceIdExperimentationItemsItemIdRequest,
} from "@vercel/sdk/models/patchv1installationsintegrationconfigurationidresourcesresourceidexperimentationitemsitemidop.js";

let value:
  PatchV1InstallationsIntegrationConfigurationIdResourcesResourceIdExperimentationItemsItemIdRequest =
    {
      integrationConfigurationId: "<id>",
      resourceId: "<id>",
      itemId: "<id>"
    };
```

----------------------------------------

TITLE: Assigning a RoutesHandle Value in TypeScript
DESCRIPTION: Demonstrates how to import the RoutesHandle type from @vercel/sdk and assign one of its valid string literal values to a variable.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/models/routeshandle.md#_snippet_0

LANGUAGE: typescript
CODE:
```
import { RoutesHandle } from "@vercel/sdk/models/createdeploymentop.js";

let value: RoutesHandle = "hit";
```

----------------------------------------

TITLE: Importing Resource using Standalone Function (TypeScript)
DESCRIPTION: This snippet shows how to import a resource using the standalone `marketplaceImportResource` function and a `VercelCore` instance, which is recommended for better tree-shaking. It demonstrates checking the result for errors using `res.ok` and accessing the value with `res.value`.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/marketplace/README.md#_snippet_21

LANGUAGE: TypeScript
CODE:
```
import { VercelCore } from "@vercel/sdk/core.js";
import { marketplaceImportResource } from "@vercel/sdk/funcs/marketplaceImportResource.js";

// Use `VercelCore` for best tree-shaking performance.
// You can create one instance of it to use across an application.
const vercel = new VercelCore({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const res = await marketplaceImportResource(vercel, {
    integrationConfigurationId: "<id>",
    resourceId: "<id>",
  });

  if (!res.ok) {
    throw res.error;
  }

  const { value: result } = res;

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Retrieve Projects using Vercel SDK Standalone Function (TypeScript)
DESCRIPTION: Illustrates the use of the standalone `projectsGetProjects` function from the Vercel SDK. This approach is recommended for better tree-shaking performance and requires passing a `VercelCore` instance.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/projects/README.md#_snippet_3

LANGUAGE: typescript
CODE:
```
import { VercelCore } from "@vercel/sdk/core.js";
import { projectsGetProjects } from "@vercel/sdk/funcs/projectsGetProjects.js";

// Use `VercelCore` for best tree-shaking performance.
// You can create one instance of it to use across an application.
const vercel = new VercelCore({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>"
});

async function run() {
  const res = await projectsGetProjects(vercel, {
    gitForkProtection: "1",
    repoUrl: "https://github.com/vercel/next.js",
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
    slug: "my-team-url-slug"
  });

  if (!res.ok) {
    throw res.error;
  }

  const { value: result } = res;

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Updating Vercel Invoice using Vercel SDK Class (TypeScript)
DESCRIPTION: Demonstrates how to update a Vercel invoice (specifically requesting a refund) using the `updateInvoice` method of a `Vercel` class instance from the `@vercel/sdk`. Requires `integrationConfigurationId`, `invoiceId`, and a `requestBody` specifying the action, reason, and total.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/marketplace/README.md#_snippet_12

LANGUAGE: typescript
CODE:
```
import { Vercel } from "@vercel/sdk";

const vercel = new Vercel({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  await vercel.marketplace.updateInvoice({
    integrationConfigurationId: "<id>",
    invoiceId: "<id>",
    requestBody: {
      action: "refund",
      reason: "<value>",
      total: "<value>"
    }
  });


}

run();
```

----------------------------------------

TITLE: Creating a Team using Vercel SDK Instance
DESCRIPTION: Demonstrates how to create a new Vercel Team by calling the `createTeam` method on an initialized instance of the `Vercel` class. Requires importing `Vercel` and providing a bearer token during initialization. The method takes an object with `slug` and optional `name`.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/teams/README.md#_snippet_20

LANGUAGE: typescript
CODE:
```
import { Vercel } from "@vercel/sdk";

const vercel = new Vercel({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const result = await vercel.teams.createTeam({
    slug: "a-random-team",
    name: "A Random Team",
  });

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Instantiating models.GetProjectsLink4 Type (TypeScript)
DESCRIPTION: Demonstrates how to create an instance of the `models.GetProjectsLink4` type, showing the expected structure for the `deployHooks` array which contains objects with `id`, `name`, `ref`, and `url` properties.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/models/getprojectslink.md#_snippet_3

LANGUAGE: typescript
CODE:
```
const value: models.GetProjectsLink4 = {
  deployHooks: [
    {
      id: "<id>",
      name: "<value>",
      ref: "<value>",
      url: "https://austere-encouragement.org/"
    }
  ]
};
```

----------------------------------------

TITLE: Example Usage of RemoveProjectEnvContentHintProjectsResponse7 in TypeScript
DESCRIPTION: This snippet demonstrates how to import and instantiate the `RemoveProjectEnvContentHintProjectsResponse7` type. It shows the required fields (`type` and `storeId`) needed to create a valid object of this type.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/models/removeprojectenvcontenthintprojectsresponse7.md#_snippet_0

LANGUAGE: typescript
CODE:
```
import { RemoveProjectEnvContentHintProjectsResponse7 } from "@vercel/sdk/models/removeprojectenvop.js";

let value: RemoveProjectEnvContentHintProjectsResponse7 = {
  type: "postgres-url-non-pooling",
  storeId: "<id>",
};
```

----------------------------------------

TITLE: Defining Possible Values for GetAllChecksStatus Type in TypeScript
DESCRIPTION: This snippet illustrates the union type definition for `GetAllChecksStatus`, listing the allowed string literal values: 'registered', 'running', and 'completed'.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/models/getallchecksstatus.md#_snippet_1

LANGUAGE: typescript
CODE:
```
"registered" | "running" | "completed"
```

----------------------------------------

TITLE: Retrieving Vercel Project Environment Variables using Standalone Function (TypeScript)
DESCRIPTION: Shows how to use the `VercelCore` class for better tree-shaking and call the standalone `projectsFilterProjectEnvs` function directly. It retrieves environment variables for a project with similar filtering options as the instance method. Requires `@vercel/sdk/core.js` and `@vercel/sdk/funcs/projectsFilterProjectEnvs.js`. Includes error handling.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/projects/README.md#_snippet_25

LANGUAGE: typescript
CODE:
```
import { VercelCore } from "@vercel/sdk/core.js";
import { projectsFilterProjectEnvs } from "@vercel/sdk/funcs/projectsFilterProjectEnvs.js";

// Use `VercelCore` for best tree-shaking performance.
// You can create one instance of it to use across an application.
const vercel = new VercelCore({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const res = await projectsFilterProjectEnvs(vercel, {
    idOrName: "prj_XLKmu1DyR1eY7zq8UgeRKbA7yVLA",
    gitBranch: "feature-1",
    decrypt: "true",
    source: "vercel-cli:pull",
    customEnvironmentId: "env_123abc4567",
    customEnvironmentSlug: "my-custom-env",
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
    slug: "my-team-url-slug",
  });

  if (!res.ok) {
    throw res.error;
  }

  const { value: result } = res;

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Update Attack Challenge Mode using Vercel SDK Class (TypeScript)
DESCRIPTION: This example demonstrates how to update the Attack Challenge mode setting for a project using the main `Vercel` class from the SDK. It shows initialization, calling the `security.updateAttackChallengeMode` method with required parameters (teamId, slug, requestBody), and handling the result.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/security/README.md#_snippet_0

LANGUAGE: typescript
CODE:
```
import { Vercel } from "@vercel/sdk";

const vercel = new Vercel({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const result = await vercel.security.updateAttackChallengeMode({
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
    slug: "my-team-url-slug",
    requestBody: {
      projectId: "<id>",
      attackModeEnabled: true,
    },
  });

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Deleting Edge Config Schema using Vercel SDK (Class Method) - TypeScript
DESCRIPTION: Demonstrates how to delete an Edge Config schema by initializing the `Vercel` class with a bearer token and calling the `edgeConfig.deleteEdgeConfigSchema` method. Requires `edgeConfigId`, `teamId`, and `slug` parameters to identify the target Edge Config.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/edgeconfig/README.md#_snippet_16

LANGUAGE: typescript
CODE:
```
import { Vercel } from "@vercel/sdk";

const vercel = new Vercel({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  await vercel.edgeConfig.deleteEdgeConfigSchema({
    edgeConfigId: "<id>",
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
    slug: "my-team-url-slug",
  });


}

run();
```

----------------------------------------

TITLE: Pause Vercel Project using Vercel Class (TypeScript)
DESCRIPTION: Demonstrates how to pause a Vercel project using the `pauseProject` method on a `Vercel` class instance from the `@vercel/sdk`. Requires the project `id` and optionally `teamId` or `slug` for team projects. The request targets a project via its ID in the URL.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/projects/README.md#_snippet_44

LANGUAGE: TypeScript
CODE:
```
import { Vercel } from "@vercel/sdk";

const vercel = new Vercel({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  await vercel.projects.pauseProject({
    projectId: "<id>",
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
    slug: "my-team-url-slug"
  });


}

run();
```

----------------------------------------

TITLE: Creating GetDeploymentsResponseBody Example Instance (TypeScript)
DESCRIPTION: This snippet demonstrates how to create and initialize a variable of type GetDeploymentsResponseBody with example data, showcasing the expected structure including pagination and deployment details.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/models/getdeploymentsresponsebody.md#_snippet_0

LANGUAGE: typescript
CODE:
```
import { GetDeploymentsResponseBody } from "@vercel/sdk/models/getdeploymentsop.js";

let value: GetDeploymentsResponseBody = {
  pagination: {
    count: 20,
    next: 1540095775951,
    prev: 1540095775951,
  },
  deployments: [
    {
      uid: "dpl_2euZBFqxYdDMDG1jTrHFnNZ2eUVa",
      name: "docs",
      url: "docs-9jaeg38me.vercel.app",
      created: 1609492210000,
      defaultRoute: "/docs",
      deleted: 1609492210000,
      undeleted: 1609492210000,
      softDeletedByRetention: true,
      source: "cli",
      state: "READY",
      readyState: "READY",
      type: "LAMBDAS",
      creator: {
        uid: "eLrCnEgbKhsHyfbiNR7E8496",
        email: "example@example.com",
        username: "johndoe",
        githubLogin: "johndoe",
        gitlabLogin: "johndoe",
      },
      target: "production",
      createdAt: 1609492210000,
      buildingAt: 1609492210000,
      ready: 1609492210000,
      inspectorUrl:
        "https://vercel.com/acme/nextjs/J1hXN00qjUeoYfpEEf7dnDtpSiVq",
    },
  ],
};
```

----------------------------------------

TITLE: Removing Project Environment Variable using Standalone Function (TypeScript)
DESCRIPTION: This snippet shows how to delete a project environment variable using the standalone `projectsRemoveProjectEnv` function from the Vercel SDK core. This approach is recommended for better tree-shaking. It requires passing a `VercelCore` instance and an options object containing the project identifier (`idOrName`), environment variable identifier (`id`), and optional parameters like `customEnvironmentId`, `teamId`, and `slug`. It also includes error handling.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/projects/README.md#_snippet_31

LANGUAGE: typescript
CODE:
```
import { VercelCore } from "@vercel/sdk/core.js";
import { projectsRemoveProjectEnv } from "@vercel/sdk/funcs/projectsRemoveProjectEnv.js";

// Use `VercelCore` for best tree-shaking performance.
// You can create one instance of it to use across an application.
const vercel = new VercelCore({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const res = await projectsRemoveProjectEnv(vercel, {
    idOrName: "prj_XLKmu1DyR1eY7zq8UgeRKbA7yVLA",
    id: "XMbOEya1gUUO1ir4",
    customEnvironmentId: "env_123abc4567",
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
    slug: "my-team-url-slug",
  });

  if (!res.ok) {
    throw res.error;
  }

  const { value: result } = res;

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Update Project Protection Bypass using Standalone Function (TypeScript)
DESCRIPTION: Shows how to perform the project protection bypass update using the standalone `projectsUpdateProjectProtectionBypass` function for better tree-shaking. It requires an instance of `VercelCore` and handles potential errors from the function call.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/projects/README.md#_snippet_39

LANGUAGE: typescript
CODE:
```
import { VercelCore } from "@vercel/sdk/core.js";
import { projectsUpdateProjectProtectionBypass } from "@vercel/sdk/funcs/projectsUpdateProjectProtectionBypass.js";

// Use `VercelCore` for best tree-shaking performance.
// You can create one instance of it to use across an application.
const vercel = new VercelCore({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const res = await projectsUpdateProjectProtectionBypass(vercel, {
    idOrName: "<value>",
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
    slug: "my-team-url-slug",
    requestBody: {},
  });

  if (!res.ok) {
    throw res.error;
  }

  const { value: result } = res;

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Purchasing Domain using Vercel SDK (TypeScript)
DESCRIPTION: Demonstrates how to initialize the Vercel SDK and call the `domains.buyDomain` method with required parameters like domain name, price, renewal status, and contact information. Shows how to handle the result of the asynchronous operation.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/domains/README.md#_snippet_0

LANGUAGE: typescript
CODE:
```
import { Vercel } from "@vercel/sdk";

const vercel = new Vercel({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const result = await vercel.domains.buyDomain({
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
    slug: "my-team-url-slug",
    requestBody: {
      name: "example.com",
      expectedPrice: 10,
      renew: true,
      country: "US",
      orgName: "Acme Inc.",
      firstName: "Jane",
      lastName: "Doe",
      address1: "340 S Lemon Ave Suite 4133",
      city: "San Francisco",
      state: "CA",
      postalCode: "91789",
      phone: "+1.4158551452",
      email: "jane.doe@someplace.com",
    },
  });

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Retrieving Vercel Project Environment Variables using Vercel SDK Instance (TypeScript)
DESCRIPTION: Demonstrates how to initialize the Vercel SDK with a bearer token and call the `projects.filterProjectEnvs` method to retrieve environment variables for a specific project, filtered by ID/name, branch, decryption, source, custom environment, and team details. Requires the `@vercel/sdk` package.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/projects/README.md#_snippet_24

LANGUAGE: typescript
CODE:
```
import { Vercel } from "@vercel/sdk";

const vercel = new Vercel({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const result = await vercel.projects.filterProjectEnvs({
    idOrName: "prj_XLKmu1DyR1eY7zq8UgeRKbA7yVLA",
    gitBranch: "feature-1",
    decrypt: "true",
    source: "vercel-cli:pull",
    customEnvironmentId: "env_123abc4567",
    customEnvironmentSlug: "my-custom-env",
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
    slug: "my-team-url-slug",
  });

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Example Usage of PatchTeamRequestBody in TypeScript
DESCRIPTION: This snippet demonstrates how to create an instance of the `PatchTeamRequestBody` type from the `@vercel/sdk/models/patchteamop.js` module. It shows the structure of the object and provides example values for various properties like description, emailDomain, name, slug, and nested objects like `saml` and `remoteCaching`, illustrating how to prepare data for patching a team.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/models/patchteamrequestbody.md#_snippet_0

LANGUAGE: TypeScript
CODE:
```
import { PatchTeamRequestBody } from "@vercel/sdk/models/patchteamop.js";

let value: PatchTeamRequestBody = {
  description: "Our mission is to make cloud computing accessible to everyone",
  emailDomain: "example.com",
  name: "My Team",
  previewDeploymentSuffix: "example.dev",
  regenerateInviteCode: true,
  saml: {
    enforced: true,
  },
  slug: "my-team",
  enablePreviewFeedback: "on",
  enableProductionFeedback: "on",
  sensitiveEnvironmentVariablePolicy: "on",
  remoteCaching: {
    enabled: true,
  },
  hideIpAddresses: false,
  hideIpAddressesInLogDrains: false,
};
```

----------------------------------------

TITLE: Removing Team Member using Standalone Function - TypeScript
DESCRIPTION: This example illustrates how to use the standalone `teamsRemoveTeamMember` function along with `VercelCore` for potentially better tree-shaking. It shows instantiating `VercelCore`, calling the standalone function with the core instance and parameters, and handling the result including potential errors.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/teams/README.md#_snippet_13

LANGUAGE: typescript
CODE:
```
import { VercelCore } from "@vercel/sdk/core.js";
import { teamsRemoveTeamMember } from "@vercel/sdk/funcs/teamsRemoveTeamMember.js";

// Use `VercelCore` for best tree-shaking performance.
// You can create one instance of it to use across an application.
const vercel = new VercelCore({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const res = await teamsRemoveTeamMember(vercel, {
    uid: "ndlgr43fadlPyCtREAqxxdyFK",
    newDefaultTeamId: "team_nllPyCtREAqxxdyFKbbMDlxd",
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
  });

  if (!res.ok) {
    throw res.error;
  }

  const { value: result } = res;

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Example Usage of RecordEventsRequest in TypeScript
DESCRIPTION: This snippet demonstrates how to create an instance of the RecordEventsRequest model. It shows the structure and required fields, including optional client details, team information, and an array of event objects in the requestBody. It requires importing RecordEventsRequest from the Vercel SDK models.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/models/recordeventsrequest.md#_snippet_0

LANGUAGE: typescript
CODE:
```
import { RecordEventsRequest } from "@vercel/sdk/models/recordeventsop.js";

let value: RecordEventsRequest = {
  xArtifactClientCi: "VERCEL",
  xArtifactClientInteractive: 0,
  teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
  slug: "my-team-url-slug",
  requestBody: [
    {
      sessionId: "<id>",
      source: "LOCAL",
      event: "MISS",
      hash: "12HKQaOmR5t5Uy6vdcQsNIiZgHGB",
      duration: 400,
    },
  ],
};
```

----------------------------------------

TITLE: Getting Edge Config Item using Vercel Class (TypeScript)
DESCRIPTION: This snippet shows how to initialize the Vercel SDK using the Vercel class and retrieve a specific item from an Edge Config by its ID and key. It requires a bearer token for authentication and optionally accepts team ID or slug.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/edgeconfig/README.md#_snippet_18

LANGUAGE: typescript
CODE:
```
import { Vercel } from "@vercel/sdk";

const vercel = new Vercel({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const result = await vercel.edgeConfig.getEdgeConfigItem({
    edgeConfigId: "<id>",
    edgeConfigItemKey: "<value>",
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
    slug: "my-team-url-slug",
  });

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Using deploymentsGetDeploymentEvents Standalone Function (TypeScript)
DESCRIPTION: This example illustrates using the `VercelCore` class and the specific `deploymentsGetDeploymentEvents` standalone function directly. It shows how to pass the core instance and parameters, and how to check the result for errors, which is beneficial for tree-shaking.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/deployments/README.md#_snippet_1

LANGUAGE: typescript
CODE:
```
import { VercelCore } from "@vercel/sdk/core.js";
import { deploymentsGetDeploymentEvents } from "@vercel/sdk/funcs/deploymentsGetDeploymentEvents.js";

// Use `VercelCore` for best tree-shaking performance.
// You can create one instance of it to use across an application.
const vercel = new VercelCore({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const res = await deploymentsGetDeploymentEvents(vercel, {
    idOrUrl: "dpl_5WJWYSyB7BpgTj3EuwF37WMRBXBtPQ2iTMJHJBJyRfd",
    follow: 1,
    limit: 100,
    name: "bld_cotnkcr76",
    since: 1540095775941,
    until: 1540106318643,
    statusCode: "5xx",
    delimiter: 1,
    builds: 1,
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
    slug: "my-team-url-slug",
  });

  if (!res.ok) {
    throw res.error;
  }

  const { value: result } = res;

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Defining Values for RemoveProjectEnvTargetProjects1 Type
DESCRIPTION: This snippet shows the union of string literal types that constitute the `RemoveProjectEnvTargetProjects1` type. These values represent the possible environment targets: 'production', 'preview', and 'development'.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/models/removeprojectenvtargetprojects1.md#_snippet_1

LANGUAGE: typescript
CODE:
```
"production" | "preview" | "development"
```

----------------------------------------

TITLE: Possible Values for GetProjectsProjectsFunctionDefaultMemoryType
DESCRIPTION: This snippet lists the allowed string literal values that can be assigned to a variable of type GetProjectsProjectsFunctionDefaultMemoryType.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/models/getprojectsprojectsfunctiondefaultmemorytype.md#_snippet_1

LANGUAGE: typescript
CODE:
```
"standard_legacy" | "standard" | "performance"
```

----------------------------------------

TITLE: Getting a Domain using Standalone Function (TypeScript)
DESCRIPTION: This example shows how to use the standalone domainsGetDomain function with VercelCore for improved tree-shaking. It illustrates instantiating VercelCore, calling the function with the core instance and parameters, and includes basic error handling by checking the 'ok' property of the result.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/domains/README.md#_snippet_11

LANGUAGE: typescript
CODE:
```
import { VercelCore } from "@vercel/sdk/core.js";
import { domainsGetDomain } from "@vercel/sdk/funcs/domainsGetDomain.js";

// Use `VercelCore` for best tree-shaking performance.
// You can create one instance of it to use across an application.
const vercel = new VercelCore({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const res = await domainsGetDomain(vercel, {
    domain: "example.com",
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
    slug: "my-team-url-slug",
  });

  if (!res.ok) {
    throw res.error;
  }

  const { value: result } = res;

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Getting Edge Config using Vercel Class (TypeScript)
DESCRIPTION: Demonstrates how to initialize the Vercel SDK using the main Vercel class and fetch Edge Config data using the instance method `edgeConfig.getEdgeConfig`. Requires a bearer token for authentication.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/edgeconfig/README.md#_snippet_4

LANGUAGE: typescript
CODE:
```
import { Vercel } from "@vercel/sdk";

const vercel = new Vercel({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const result = await vercel.edgeConfig.getEdgeConfig({
    edgeConfigId: "<id>",
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
    slug: "my-team-url-slug",
  });

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Initializing PutFirewallConfigSf Object in TypeScript
DESCRIPTION: This snippet demonstrates how to create an instance of the `PutFirewallConfigSf` object in TypeScript. It requires importing the class from the `@vercel/sdk/models/putfirewallconfigop.js` module and shows how to set the `active` and `action` properties.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/models/putfirewallconfigsf.md#_snippet_0

LANGUAGE: typescript
CODE:
```
import { PutFirewallConfigSf } from "@vercel/sdk/models/putfirewallconfigop.js";

let value: PutFirewallConfigSf = {
  active: false,
  action: "log"
};
```

----------------------------------------

TITLE: Deleting Vercel Configuration using Standalone Function (TypeScript)
DESCRIPTION: Shows how to delete a Vercel configuration using the standalone integrationsDeleteConfiguration function. This method is recommended for tree-shaking and requires a VercelCore instance and the configuration parameters (id, teamId, slug). It includes basic error handling.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/integrations/README.md#_snippet_7

LANGUAGE: typescript
CODE:
```
import { VercelCore } from "@vercel/sdk/core.js";
import { integrationsDeleteConfiguration } from "@vercel/sdk/funcs/integrationsDeleteConfiguration.js";

// Use `VercelCore` for best tree-shaking performance.
// You can create one instance of it to use across an application.
const vercel = new VercelCore({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const res = await integrationsDeleteConfiguration(vercel, {
    id: "<id>",
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
    slug: "my-team-url-slug",
  });

  if (!res.ok) {
    throw res.error;
  }

  const { value: result } = res;

  
}

run();
```

----------------------------------------

TITLE: Creating a CreateDeploymentRequest object (TypeScript)
DESCRIPTION: This snippet demonstrates how to import and instantiate a CreateDeploymentRequest object, populating it with example data for various fields including team information, file details, git metadata, and project settings.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/models/createdeploymentrequest.md#_snippet_0

LANGUAGE: typescript
CODE:
```
import { CreateDeploymentRequest } from "@vercel/sdk/models/createdeploymentop.js";

let value: CreateDeploymentRequest = {
  teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
  slug: "my-team-url-slug",
  requestBody: {
    deploymentId: "dpl_2qn7PZrx89yxY34vEZPD31Y9XVj6",
    files: [
      {
        file: "folder/file.js",
      },
    ],
    gitMetadata: {
      remoteUrl: "https://github.com/vercel/next.js",
      commitAuthorName: "kyliau",
      commitMessage:
        "add method to measure Interaction to Next Paint (INP) (#36490)",
      commitRef: "main",
      commitSha: "dc36199b2234c6586ebe05ec94078a895c707e29",
      dirty: true,
    },
    gitSource: {
      ref: "main",
      repoUuid: "123e4567-e89b-12d3-a456-426614174000",
      sha: "a1b2c3d4e5f6g7h8i9j0k1l2m3n4o5p6q7r8s9t0",
      type: "bitbucket",
      workspaceUuid: "987e6543-e21b-12d3-a456-426614174000",
    },
    meta: {
      "foo": "bar",
    },
    name: "my-instant-deployment",
    project: "my-deployment-project",
    projectSettings: {
      buildCommand: "next build",
      installCommand: "pnpm install",
    },
    target: "production",
  },
};
```

----------------------------------------

TITLE: Creating a Two1 Object (TypeScript)
DESCRIPTION: This snippet demonstrates how to create an object that conforms to the `Two1` type. It shows the required fields (`key`, `value`, `type`, `target`) and includes examples of optional fields like `gitBranch`, `comment`, and `customEnvironmentIds`. This structure is used when defining environment variables programmatically.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/models/two1.md#_snippet_0

LANGUAGE: TypeScript
CODE:
```
import { Two1 } from "@vercel/sdk/models/createprojectenvop.js";

let value: Two1 = {
  key: "API_URL",
  value: "https://api.vercel.com",
  type: "plain",
  target: [
    "preview",
  ],
  gitBranch: "feature-1",
  comment: "database connection string for production",
  customEnvironmentIds: [
    "env_1234567890",
  ],
};
```

----------------------------------------

TITLE: Example Usage of InviteUserToTeamRequestBody in TypeScript
DESCRIPTION: This snippet demonstrates how to create an instance of the InviteUserToTeamRequestBody type with example values for inviting a user by uid and email, specifying their role and associated projects.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/models/inviteusertoteamrequestbody.md#_snippet_0

LANGUAGE: TypeScript
CODE:
```
import { InviteUserToTeamRequestBody } from "@vercel/sdk/models/inviteusertoteamop.js";

let value: InviteUserToTeamRequestBody = {
  uid: "kr1PsOIzqEL5Xg6M4VZcZosf",
  email: "john@example.com",
  projects: [
    {
      projectId: "prj_ndlgr43fadlPyCtREAqxxdyFK",
      role: "ADMIN"
    }
  ]
};
```

----------------------------------------

TITLE: Instantiating DeleteAuthTokenRequest in TypeScript
DESCRIPTION: This snippet demonstrates how to create an instance of the DeleteAuthTokenRequest model in TypeScript. It requires importing the DeleteAuthTokenRequest class and providing a value for the mandatory 'tokenId' field.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/models/deleteauthtokenrequest.md#_snippet_0

LANGUAGE: typescript
CODE:
```
import { DeleteAuthTokenRequest } from "@vercel/sdk/models/deleteauthtokenop.js";

let value: DeleteAuthTokenRequest = {
  tokenId: "5d9f2ebd38ddca62e5d51e9c1704c72530bdc8bfdd41e782a6687c48399e8391"
};
```

----------------------------------------

TITLE: List Checks using Vercel Class (TypeScript)
DESCRIPTION: This example demonstrates how to list all checks for a Vercel deployment using the Vercel class instance. It requires a bearer token and the deployment details (ID, team ID, or slug).
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/checks/README.md#_snippet_2

LANGUAGE: typescript
CODE:
```
import { Vercel } from "@vercel/sdk";

const vercel = new Vercel({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const result = await vercel.checks.getAllChecks({
    deploymentId: "dpl_2qn7PZrx89yxY34vEZPD31Y9XVj6",
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
    slug: "my-team-url-slug",
  });

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Creating GetProjectsRequest Instance in TypeScript
DESCRIPTION: This snippet demonstrates how to create an instance of the GetProjectsRequest type and initialize its properties with example values. It shows the structure required for making a request to get projects.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/models/getprojectsrequest.md#_snippet_0

LANGUAGE: typescript
CODE:
```
import { GetProjectsRequest } from "@vercel/sdk/models/getprojectsop.js";

let value: GetProjectsRequest = {
  gitForkProtection: "1",
  repoUrl: "https://github.com/vercel/next.js",
  teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
  slug: "my-team-url-slug"
};
```

----------------------------------------

TITLE: Initializing GetDeploymentGitSource1 in TypeScript
DESCRIPTION: This snippet demonstrates how to create an object that conforms to the GetDeploymentGitSource1 type, specifying the source type and repository ID. It requires importing the type from the Vercel SDK models.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/models/getdeploymentgitsource1.md#_snippet_0

LANGUAGE: typescript
CODE:
```
import { GetDeploymentGitSource1 } from "@vercel/sdk/models/getdeploymentop.js";

let value: GetDeploymentGitSource1 = {
  type: "github",
  repoId: 2047.24,
};
```

----------------------------------------

TITLE: Instantiating UpdateAccessGroupRequest in TypeScript
DESCRIPTION: This snippet demonstrates how to create an instance of the `UpdateAccessGroupRequest` type. It shows the required import statement and provides example values for the `idOrName`, `teamId`, `slug`, and the nested `requestBody` properties, including examples for adding and removing members and specifying project roles.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/models/updateaccessgrouprequest.md#_snippet_0

LANGUAGE: typescript
CODE:
```
import { UpdateAccessGroupRequest } from "@vercel/sdk/models/updateaccessgroupop.js";

let value: UpdateAccessGroupRequest = {
  idOrName: "<value>",
  teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
  slug: "my-team-url-slug",
  requestBody: {
    name: "My access group",
    projects: [
      {
        projectId: "prj_ndlgr43fadlPyCtREAqxxdyFK",
        role: "ADMIN"
      }
    ],
    membersToAdd: [
      "usr_1a2b3c4d5e6f7g8h9i0j",
      "usr_2b3c4d5e6f7g8h9i0j1k"
    ],
    membersToRemove: [
      "usr_1a2b3c4d5e6f7g8h9i0j",
      "usr_2b3c4d5e6f7g8h9i0j1k"
    ]
  }
};
```

----------------------------------------

TITLE: Example Usage of CreateEdgeConfigTokenRequest in TypeScript
DESCRIPTION: This snippet demonstrates how to instantiate the `CreateEdgeConfigTokenRequest` object in TypeScript, showing the structure for providing the required `edgeConfigId` and `requestBody` (containing the token label), as well as optional `teamId` or `slug`.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/models/createedgeconfigtokenrequest.md#_snippet_0

LANGUAGE: typescript
CODE:
```
import { CreateEdgeConfigTokenRequest } from "@vercel/sdk/models/createedgeconfigtokenop.js";

let value: CreateEdgeConfigTokenRequest = {
  edgeConfigId: "<id>",
  teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
  slug: "my-team-url-slug",
  requestBody: {
    label: "<value>"
  }
};
```

----------------------------------------

TITLE: Example Usage of PatchEdgeConfigSchemaRequestBody in TypeScript
DESCRIPTION: This snippet demonstrates how to create an instance of the PatchEdgeConfigSchemaRequestBody type in TypeScript, showing the required 'definition' field.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/models/patchedgeconfigschemarequestbody.md#_snippet_0

LANGUAGE: typescript
CODE:
```
import { PatchEdgeConfigSchemaRequestBody } from "@vercel/sdk/models/patchedgeconfigschemaop.js";

let value: PatchEdgeConfigSchemaRequestBody = {
  definition: "<value>",
};
```

----------------------------------------

TITLE: Initializing CreateProjectEnv object in TypeScript
DESCRIPTION: This snippet imports the `CreateProjectEnv` type from the Vercel SDK and demonstrates how to initialize a variable of this type. It shows the basic structure for defining a plain text environment variable with a specified key and value.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/models/createprojectenv.md#_snippet_0

LANGUAGE: typescript
CODE:
```
import { CreateProjectEnv } from "@vercel/sdk/models/createprojectop.js";

let value: CreateProjectEnv = {
  type: "plain",
  value: "<value>",
  key: "<key>"
};
```

----------------------------------------

TITLE: Initialize GetProjectsContentHint8 Type - TypeScript
DESCRIPTION: This snippet demonstrates how to import and initialize an instance of the `GetProjectsContentHint8` type. It shows the required fields `type` and `storeId` being assigned example values.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/models/getprojectscontenthint8.md#_snippet_0

LANGUAGE: typescript
CODE:
```
import { GetProjectsContentHint8 } from "@vercel/sdk/models/getprojectsop.js";

let value: GetProjectsContentHint8 = {
  type: "postgres-prisma-url",
  storeId: "<id>",
};
```

----------------------------------------

TITLE: Updating DNS Record with Vercel SDK (Standalone Function) - TypeScript
DESCRIPTION: Shows how to update a DNS record using the standalone dnsUpdateRecord function from @vercel/sdk/funcs/dnsUpdateRecord.js. It utilizes VercelCore for potential tree-shaking benefits and demonstrates handling the result, including error checking.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/dns/README.md#_snippet_5

LANGUAGE: typescript
CODE:
```
import { VercelCore } from "@vercel/sdk/core.js";
import { dnsUpdateRecord } from "@vercel/sdk/funcs/dnsUpdateRecord.js";

// Use `VercelCore` for best tree-shaking performance.
// You can create one instance of it to use across an application.
const vercel = new VercelCore({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const res = await dnsUpdateRecord(vercel, {
    recordId: "rec_2qn7pzrx89yxy34vezpd31y9",
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
    slug: "my-team-url-slug",
    requestBody: {
      name: "example-1",
      value: "google.com",
      type: "A",
      ttl: 60,
      srv: {
        target: "example2.com.",
        weight: 97604,
        port: 570172,
        priority: 199524,
      },
      https: {
        priority: 35000,
        target: "example2.com.",
      },
      comment: "used to verify ownership of domain",
    },
  });

  if (!res.ok) {
    throw res.error;
  }

  const { value: result } = res;

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Configuring SQL Injection Protection with Vercel SDK TypeScript
DESCRIPTION: This snippet demonstrates how to create an instance of the PutFirewallConfigSqli object to define settings for SQL injection protection. It shows how to set the 'active' status and the 'action' to take when an attack is detected.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/models/putfirewallconfigsqli.md#_snippet_0

LANGUAGE: typescript
CODE:
```
import { PutFirewallConfigSqli } from "@vercel/sdk/models/putfirewallconfigop.js";

let value: PutFirewallConfigSqli = {
  active: false,
  action: "deny"
};
```

----------------------------------------

TITLE: Creating Event using Standalone Function - TypeScript
DESCRIPTION: This snippet shows how to use the standalone `marketplaceCreateEvent` function to dispatch an event. It requires importing `VercelCore` and `marketplaceCreateEvent` specifically, which is recommended for better tree-shaking. The function takes a `VercelCore` instance and the event payload as arguments and demonstrates checking the result.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/marketplace/README.md#_snippet_5

LANGUAGE: typescript
CODE:
```
import { VercelCore } from "@vercel/sdk/core.js";
import { marketplaceCreateEvent } from "@vercel/sdk/funcs/marketplaceCreateEvent.js";

// Use `VercelCore` for best tree-shaking performance.
// You can create one instance of it to use across an application.
const vercel = new VercelCore({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const res = await marketplaceCreateEvent(vercel, {
    integrationConfigurationId: "<id>",
    requestBody: {
      event: {
        type: "installation.updated",
      },
    },
  });

  if (!res.ok) {
    throw res.error;
  }

  const { value: result } = res;

  
}

run();
```

----------------------------------------

TITLE: Requesting User Deletion with Vercel Class (TypeScript)
DESCRIPTION: Demonstrates how to initiate a user deletion request by calling the `requestDelete` method on an instance of the `Vercel` class. It requires a bearer token for authentication and logs the result of the asynchronous operation.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/user/README.md#_snippet_4

LANGUAGE: typescript
CODE:
```
import { Vercel } from "@vercel/sdk";

const vercel = new Vercel({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const result = await vercel.user.requestDelete({});

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Initializing GitSource9 Model in TypeScript
DESCRIPTION: This snippet demonstrates how to import the GitSource9 type and create an instance of it, populating its properties like type, ref, sha, and repoId. This model represents a source from a Git repository.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/models/gitsource9.md#_snippet_0

LANGUAGE: TypeScript
CODE:
```
import { GitSource9 } from "@vercel/sdk/models/canceldeploymentop.js";

let value: GitSource9 = {
  type: "github",
  ref: "<value>",
  sha: "<value>",
  repoId: 3080.91,
};
```

----------------------------------------

TITLE: Update Vercel Access Group using Standalone Function - TypeScript
DESCRIPTION: Shows how to update a Vercel access group using the standalone `accessGroupsUpdateAccessGroup` function from `@vercel/sdk/funcs`. This approach uses `VercelCore` for better tree-shaking. It takes the `VercelCore` instance and an options object as arguments. Includes basic error handling by checking the `ok` property of the result.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/accessgroups/README.md#_snippet_3

LANGUAGE: typescript
CODE:
```
import { VercelCore } from "@vercel/sdk/core.js";
import { accessGroupsUpdateAccessGroup } from "@vercel/sdk/funcs/accessGroupsUpdateAccessGroup.js";

// Use `VercelCore` for best tree-shaking performance.
// You can create one instance of it to use across an application.
const vercel = new VercelCore({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const res = await accessGroupsUpdateAccessGroup(vercel, {
    idOrName: "<value>",
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
    slug: "my-team-url-slug",
    requestBody: {
      name: "My access group",
      projects: [
        {
          projectId: "prj_ndlgr43fadlPyCtREAqxxdyFK",
          role: "ADMIN"
        }
      ],
      membersToAdd: [
        "usr_1a2b3c4d5e6f7g8h9i0j",
        "usr_2b3c4d5e6f7g8h9i0j1k"
      ],
      membersToRemove: [
        "usr_1a2b3c4d5e6f7g8h9i0j",
        "usr_2b3c4d5e6f7g8h9i0j1k"
      ]
    }
  });

  if (!res.ok) {
    throw res.error;
  }

  const { value: result } = res;

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Initialize CreateProjectContentHint14 (TypeScript)
DESCRIPTION: This snippet demonstrates how to import the `CreateProjectContentHint14` type and initialize a variable with an example object. The object represents a content hint of type 'integration-store-secret' and includes placeholder IDs for the store, integration, product, and configuration.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/models/createprojectcontenthint14.md#_snippet_0

LANGUAGE: typescript
CODE:
```
import { CreateProjectContentHint14 } from "@vercel/sdk/models/createprojectop.js";

let value: CreateProjectContentHint14 = {
  type: "integration-store-secret",
  storeId: "<id>",
  integrationId: "<id>",
  integrationProductId: "<id>",
  integrationConfigurationId: "<id>"
};
```

----------------------------------------

TITLE: Example Usage: Update Firewall Configuration Request Body (TypeScript)
DESCRIPTION: Demonstrates how to create an instance of `UpdateFirewallConfigRequestBody1` to disable the firewall. It shows the required `action` and `value` fields.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/models/updatefirewallconfigrequestbody1.md#_snippet_0

LANGUAGE: typescript
CODE:
```
import { UpdateFirewallConfigRequestBody1 } from "@vercel/sdk/models/updatefirewallconfigop.js";

let value: UpdateFirewallConfigRequestBody1 = {
  action: "firewallEnabled",
  value: false,
};
```

----------------------------------------

TITLE: Creating CheckDomainStatusResponseBody Example
DESCRIPTION: Demonstrates how to create an instance of the `CheckDomainStatusResponseBody` type. It shows importing the type and initializing a variable with a sample value.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/models/checkdomainstatusresponsebody.md#_snippet_0

LANGUAGE: typescript
CODE:
```
import { CheckDomainStatusResponseBody } from "@vercel/sdk/models/checkdomainstatusop.js";

let value: CheckDomainStatusResponseBody = {
  available: false,
};
```

----------------------------------------

TITLE: Initializing GetIntegrationLogDrainsRequest in TypeScript
DESCRIPTION: This snippet demonstrates how to create an instance of the GetIntegrationLogDrainsRequest object in TypeScript, providing example values for the team identifier or slug. This object is used to specify the target team when fetching integration log drains.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/models/getintegrationlogdrainsrequest.md#_snippet_0

LANGUAGE: typescript
CODE:
```
import { GetIntegrationLogDrainsRequest } from "@vercel/sdk/models/getintegrationlogdrainsop.js";

let value: GetIntegrationLogDrainsRequest = {
  teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
  slug: "my-team-url-slug"
};
```

----------------------------------------

TITLE: Example Deployment Metadata Object (JSON)
DESCRIPTION: An example of the `meta` property, which allows attaching arbitrary key-value metadata to a Vercel deployment. This example shows a simple key 'foo' with value 'bar'.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/models/createdeploymentrequestbody.md#_snippet_1

LANGUAGE: JSON
CODE:
```
{"foo": "bar"}
```

----------------------------------------

TITLE: Initializing OneHundredAndFortySeven Type in TypeScript
DESCRIPTION: This snippet demonstrates how to import and initialize a variable of the OneHundredAndFortySeven type. It shows the required fields (team, configuration, peering) and provides placeholder values for their properties.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/models/onehundredandfortyseven.md#_snippet_0

LANGUAGE: typescript
CODE:
```
import { OneHundredAndFortySeven } from "@vercel/sdk/models/userevent.js";

let value: OneHundredAndFortySeven = {
  team: {
    id: "<id>",
    name: "<value>"
  },
  configuration: {
    id: "<id>"
  },
  peering: {
    id: "<id>"
  }
};
```

----------------------------------------

TITLE: Assigning Alias with Vercel SDK Standalone Function (TypeScript)
DESCRIPTION: Shows how to assign an alias using the standalone `aliasesAssignAlias` function from the Vercel SDK core. This approach utilizes a `VercelCore` instance for potential tree-shaking benefits and handles the result using a success/error structure. It requires a bearer token and the same parameters as the class-based method.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/aliases/README.md#_snippet_3

LANGUAGE: typescript
CODE:
```
import { VercelCore } from "@vercel/sdk/core.js";
import { aliasesAssignAlias } from "@vercel/sdk/funcs/aliasesAssignAlias.js";

// Use `VercelCore` for best tree-shaking performance.
// You can create one instance of it to use across an application.
const vercel = new VercelCore({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const res = await aliasesAssignAlias(vercel, {
    id: "<id>",
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
    slug: "my-team-url-slug",
    requestBody: {
      alias: "my-alias.vercel.app",
      redirect: null,
    },
  });

  if (!res.ok) {
    throw res.error;
  }

  const { value: result } = res;

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Verify Project Domain using Standalone Function (TypeScript)
DESCRIPTION: Shows how to use the standalone `projectsVerifyProjectDomain` function for verifying a project domain. This approach uses `VercelCore` for better tree-shaking and passes the core instance to the function.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/projects/README.md#_snippet_23

LANGUAGE: typescript
CODE:
```
import { VercelCore } from "@vercel/sdk/core.js";
import { projectsVerifyProjectDomain } from "@vercel/sdk/funcs/projectsVerifyProjectDomain.js";

// Use `VercelCore` for best tree-shaking performance.
// You can create one instance of it to use across an application.
const vercel = new VercelCore({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const res = await projectsVerifyProjectDomain(vercel, {
    idOrName: "prj_12HKQaOmR5t5Uy6vdcQsNIiZgHGB",
    domain: "example.com",
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
    slug: "my-team-url-slug",
  });

  if (!res.ok) {
    throw res.error;
  }

  const { value: result } = res;

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Example Usage of CreateProjectTrustedIps1 in TypeScript
DESCRIPTION: This snippet demonstrates how to create an instance of the CreateProjectTrustedIps1 type, showing the required fields and their structure. It requires importing the type from the @vercel/sdk package.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/models/createprojecttrustedips1.md#_snippet_0

LANGUAGE: typescript
CODE:
```
import { CreateProjectTrustedIps1 } from "@vercel/sdk/models/createprojectop.js";

let value: CreateProjectTrustedIps1 = {
  deploymentType: "all",
  addresses: [
    {
      value: "<value>"
    }
  ],
  protectionMode: "exclusive"
};
```

----------------------------------------

TITLE: Retrieve Deployment Files using Standalone Function (TypeScript)
DESCRIPTION: Illustrates how to use the standalone `deploymentsListDeploymentFiles` function, which is recommended for better tree-shaking performance. This function requires an instance of `VercelCore` and the deployment parameters (ID, team ID, slug). The example includes basic error handling and logs the successful result.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/deployments/README.md#_snippet_13

LANGUAGE: typescript
CODE:
```
import { VercelCore } from "@vercel/sdk/core.js";
import { deploymentsListDeploymentFiles } from "@vercel/sdk/funcs/deploymentsListDeploymentFiles.js";

// Use `VercelCore` for best tree-shaking performance.
// You can create one instance of it to use across an application.
const vercel = new VercelCore({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>"
});

async function run() {
  const res = await deploymentsListDeploymentFiles(vercel, {
    id: "<id>",
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
    slug: "my-team-url-slug"
  });

  if (!res.ok) {
    throw res.error;
  }

  const { value: result } = res;

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Example Usage of PutFirewallConfigXss in TypeScript
DESCRIPTION: This snippet demonstrates how to import the PutFirewallConfigXss type and create an instance of it with example configuration values for active status and action.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/models/putfirewallconfigxss.md#_snippet_0

LANGUAGE: TypeScript
CODE:
```
import { PutFirewallConfigXss } from "@vercel/sdk/models/putfirewallconfigop.js";

let value: PutFirewallConfigXss = {
  active: false,
  action: "deny"
};
```

----------------------------------------

TITLE: Initialize GetFirewallConfigConditionGroup in TypeScript
DESCRIPTION: This snippet demonstrates how to create an instance of the `GetFirewallConfigConditionGroup` type. It shows the required `conditions` field, which is an array of condition objects. The example initializes the array with a single condition specifying `type` as 'geo_as_number' and `op` as 'sub'. This requires importing the type from `@vercel/sdk/models/getfirewallconfigop.js`.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/models/getfirewallconfigconditiongroup.md#_snippet_0

LANGUAGE: typescript
CODE:
```
import { GetFirewallConfigConditionGroup } from "@vercel/sdk/models/getfirewallconfigop.js";

let value: GetFirewallConfigConditionGroup = {
  conditions: [
    {
      type: "geo_as_number",
      op: "sub"
    }
  ]
};
```

----------------------------------------

TITLE: Defining Literal Values for UpdateProjectProjectsResponse200ApplicationJSONType in TypeScript
DESCRIPTION: This snippet specifies the allowed literal string values ('promote' and 'rollback') that can be assigned to variables of the `UpdateProjectProjectsResponse200ApplicationJSONType` type.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/models/updateprojectprojectsresponse200applicationjsontype.md#_snippet_1

LANGUAGE: typescript
CODE:
```
"promote" | "rollback"
```

----------------------------------------

TITLE: Cancel Deployment with Vercel Class (TypeScript)
DESCRIPTION: Demonstrates how to cancel a Vercel deployment using the `cancelDeployment` method of the `Vercel` class from `@vercel/sdk`. Requires a bearer token for authentication and the deployment's ID. Optionally accepts `teamId` or `slug` for team context.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/deployments/README.md#_snippet_8

LANGUAGE: typescript
CODE:
```
import { Vercel } from "@vercel/sdk";

const vercel = new Vercel({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const result = await vercel.deployments.cancelDeployment({
    id: "dpl_5WJWYSyB7BpgTj3EuwF37WMRBXBtPQ2iTMJHJBJyRfd",
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
    slug: "my-team-url-slug",
  });

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Removing Project Environment Variable using Vercel SDK Instance (TypeScript)
DESCRIPTION: This snippet demonstrates how to delete a project environment variable using the `removeProjectEnv` method available on the `projects` property of a `Vercel` SDK instance. It requires initializing the `Vercel` class with a bearer token and then calling the method with the project identifier (`idOrName`), the environment variable identifier (`id`), and optionally `customEnvironmentId`, `teamId`, and `slug`.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/projects/README.md#_snippet_30

LANGUAGE: typescript
CODE:
```
import { Vercel } from "@vercel/sdk";

const vercel = new Vercel({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const result = await vercel.projects.removeProjectEnv({
    idOrName: "prj_XLKmu1DyR1eY7zq8UgeRKbA7yVLA",
    id: "XMbOEya1gUUO1ir4",
    customEnvironmentId: "env_123abc4567",
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
    slug: "my-team-url-slug",
  });

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Create Custom Environment using Standalone Function (TypeScript)
DESCRIPTION: Shows how to use the standalone environmentCreateCustomEnvironment function with a VercelCore instance for improved tree-shaking. The example includes handling the result and checking for errors.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/environment/README.md#_snippet_1

LANGUAGE: TypeScript
CODE:
```
import { VercelCore } from "@vercel/sdk/core.js";
import { environmentCreateCustomEnvironment } from "@vercel/sdk/funcs/environmentCreateCustomEnvironment.js";

// Use `VercelCore` for best tree-shaking performance.
// You can create one instance of it to use across an application.
const vercel = new VercelCore({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const res = await environmentCreateCustomEnvironment(vercel, {
    idOrName: "<value>",
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
    slug: "my-team-url-slug",
  });

  if (!res.ok) {
    throw res.error;
  }

  const { value: result } = res;

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Initializing ContentHint11 in TypeScript
DESCRIPTION: This snippet demonstrates how to import and initialize an instance of the `ContentHint11` type. It shows the required structure including the `type` field set to 'postgres-password' and a placeholder for the `storeId`.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/models/contenthint11.md#_snippet_0

LANGUAGE: typescript
CODE:
```
import { ContentHint11 } from "@vercel/sdk/models/updateprojectdatacacheop.js";

let value: ContentHint11 = {
  type: "postgres-password",
  storeId: "<id>",
};
```

----------------------------------------

TITLE: List Project Members using Standalone Function - TypeScript
DESCRIPTION: Initializes `VercelCore` for tree-shaking benefits and calls the standalone `projectMembersGetProjectMembers` function, passing the core instance and parameters. It checks the result for errors and logs the successful response value. Requires `@vercel/sdk/core.js` and `@vercel/sdk/funcs/projectMembersGetProjectMembers.js`.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/projectmembers/README.md#_snippet_1

LANGUAGE: TypeScript
CODE:
```
import { VercelCore } from "@vercel/sdk/core.js";
import { projectMembersGetProjectMembers } from "@vercel/sdk/funcs/projectMembersGetProjectMembers.js";

// Use `VercelCore` for best tree-shaking performance.
// You can create one instance of it to use across an application.
const vercel = new VercelCore({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const res = await projectMembersGetProjectMembers(vercel, {
    idOrName: "prj_pavWOn1iLObbXLRiwVvzmPrTWyTf",
    limit: 20,
    since: 1540095775951,
    until: 1540095775951,
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
    slug: "my-team-url-slug",
  });

  if (!res.ok) {
    throw res.error;
  }

  const { value: result } = res;

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Checking Artifact Status using Vercel Class - TypeScript
DESCRIPTION: Demonstrates how to check the status of Vercel artifacts by creating an instance of the main `Vercel` class and calling the `artifacts.status` method with team and slug identifiers. Requires a bearer token for authentication.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/artifacts/README.md#_snippet_2

LANGUAGE: typescript
CODE:
```
import { Vercel } from "@vercel/sdk";

const vercel = new Vercel({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const result = await vercel.artifacts.status({
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
    slug: "my-team-url-slug",
  });

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Creating Access Group with Vercel SDK Class (TypeScript)
DESCRIPTION: This snippet shows how to create an access group using the `vercel.accessGroups.createAccessGroup` method on an instance of the `Vercel` class. It requires initializing the `Vercel` class with a bearer token. The method takes parameters for the team ID, slug, and a request body specifying the access group's name, associated projects with roles, and members to add. The result of the API call is logged to the console.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/accessgroups/README.md#_snippet_10

LANGUAGE: typescript
CODE:
```
import { Vercel } from "@vercel/sdk";

const vercel = new Vercel({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const result = await vercel.accessGroups.createAccessGroup({
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
    slug: "my-team-url-slug",
    requestBody: {
      name: "My access group",
      projects: [
        {
          projectId: "prj_ndlgr43fadlPyCtREAqxxdyFK",
          role: "ADMIN"
        }
      ],
      membersToAdd: [
        "usr_1a2b3c4d5e6f7g8h9i0j",
        "usr_2b3c4d5e6f7g8h9i0j1k"
      ]
    }
  });

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Updating Vercel Team using Standalone Function (TypeScript)
DESCRIPTION: Illustrates how to update a Vercel Team using the standalone `teamsPatchTeam` function. This approach is recommended for better tree-shaking performance and requires passing an instance of `VercelCore` and the update parameters to the function.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/teams/README.md#_snippet_17

LANGUAGE: typescript
CODE:
```
import { VercelCore } from "@vercel/sdk/core.js";
import { teamsPatchTeam } from "@vercel/sdk/funcs/teamsPatchTeam.js";

// Use `VercelCore` for best tree-shaking performance.
// You can create one instance of it to use across an application.
const vercel = new VercelCore({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const res = await teamsPatchTeam(vercel, {
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
    slug: "my-team-url-slug",
    requestBody: {
      description: "Our mission is to make cloud computing accessible to everyone",
      emailDomain: "example.com",
      name: "My Team",
      previewDeploymentSuffix: "example.dev",
      regenerateInviteCode: true,
      saml: {
        enforced: true,
      },
      slug: "my-team",
      enablePreviewFeedback: "on",
      enableProductionFeedback: "on",
      sensitiveEnvironmentVariablePolicy: "on",
      remoteCaching: {
        enabled: true,
      },
      hideIpAddresses: false,
      hideIpAddressesInLogDrains: false,
    },
  });

  if (!res.ok) {
    throw res.error;
  }

  const { value: result } = res;

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Initializing CreateOrTransferDomainResponseBody in TypeScript
DESCRIPTION: This snippet demonstrates how to create an instance of the `CreateOrTransferDomainResponseBody` type, populating it with example data for a domain object. It shows the expected structure and types for the various fields within the response body, including nested objects like `creator` and arrays like `nameservers`. It requires importing the type from the `@vercel/sdk/models` module.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/models/createortransferdomainresponsebody.md#_snippet_0

LANGUAGE: TypeScript
CODE:
```
import { CreateOrTransferDomainResponseBody } from "@vercel/sdk/models/createortransferdomainop.js";

let value: CreateOrTransferDomainResponseBody = {
  domain: {
    verified: true,
    nameservers: [
      "ns1.nameserver.net",
      "ns2.nameserver.net"
    ],
    intendedNameservers: [
      "ns1.vercel-dns.com",
      "ns2.vercel-dns.com"
    ],
    customNameservers: [
      "ns1.nameserver.net",
      "ns2.nameserver.net"
    ],
    creator: {
      username: "vercel_user",
      email: "demo@example.com",
      id: "ZspSRT4ljIEEmMHgoDwKWDei"
    },
    name: "example.com",
    boughtAt: 1613602938882,
    createdAt: 1613602938882,
    expiresAt: 1613602938882,
    id: "EmTbe5CEJyTk2yVAHBUWy4A3sRusca3GCwRjTC1bpeVnt1",
    orderedAt: 1613602938882,
    renew: true,
    serviceType: "zeit.world",
    transferredAt: 1613602938882,
    transferStartedAt: 1613602938882,
    userId: "<id>",
    teamId: "<id>"
  }
};
```

----------------------------------------

TITLE: Creating ALIAS Record Request Body (RequestBody9) - TypeScript
DESCRIPTION: Provides the request body structure for creating a DNS ALIAS record using models.RequestBody9. It includes name, type, ttl, value (the target hostname), and comment.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/models/createrecordrequestbody.md#_snippet_8

LANGUAGE: typescript
CODE:
```
const value: models.RequestBody9 = {
  name: "subdomain",
  type: "ALIAS",
  ttl: 60,
  value: "ns1.example.com",
  comment: "used to verify ownership of domain"
};
```

----------------------------------------

TITLE: Initializing CreateProjectProjectsOidcTokenClaims Object in TypeScript
DESCRIPTION: This snippet demonstrates how to create an instance of the `CreateProjectProjectsOidcTokenClaims` type in TypeScript, assigning placeholder values to all its required fields. This shows the expected structure for providing OIDC token claims when interacting with the Vercel SDK for project operations.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/models/createprojectprojectsoidctokenclaims.md#_snippet_0

LANGUAGE: typescript
CODE:
```
import { CreateProjectProjectsOidcTokenClaims } from "@vercel/sdk/models/createprojectop.js";

let value: CreateProjectProjectsOidcTokenClaims = {
  iss: "<value>",
  sub: "<value>",
  scope: "<value>",
  aud: "<value>",
  owner: "<value>",
  ownerId: "<id>",
  project: "<value>",
  projectId: "<id>",
  environment: "<value>"
};
```

----------------------------------------

TITLE: Deleting a Project using Vercel SDK Instance (TypeScript)
DESCRIPTION: This example demonstrates how to delete a Vercel project using the `deleteProject` method available on the `projects` property of a `Vercel` instance. It requires the project's ID or name, and optionally a team ID or slug.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/projects/README.md#_snippet_8

LANGUAGE: TypeScript
CODE:
```
import { Vercel } from "@vercel/sdk";

const vercel = new Vercel({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  await vercel.projects.deleteProject({
    idOrName: "prj_12HKQaOmR5t5Uy6vdcQsNIiZgHGB",
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
    slug: "my-team-url-slug",
  });


}

run();
```

----------------------------------------

TITLE: Initializing Vercel Deployments Object (TypeScript)
DESCRIPTION: This snippet demonstrates how to import the Deployments type from the Vercel SDK and initialize a variable with a sample Deployments object, showing the expected data structure and properties.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/models/deployments.md#_snippet_0

LANGUAGE: typescript
CODE:
```
import { Deployments } from "@vercel/sdk/models/getdeploymentsop.js";

let value: Deployments = {
  uid: "dpl_2euZBFqxYdDMDG1jTrHFnNZ2eUVa",
  name: "docs",
  url: "docs-9jaeg38me.vercel.app",
  created: 1609492210000,
  defaultRoute: "/docs",
  deleted: 1609492210000,
  undeleted: 1609492210000,
  softDeletedByRetention: true,
  source: "cli",
  state: "READY",
  readyState: "READY",
  type: "LAMBDAS",
  creator: {
    uid: "eLrCnEgbKhsHyfbiNR7E8496",
    email: "example@example.com",
    username: "johndoe",
    githubLogin: "johndoe",
    gitlabLogin: "johndoe"
  },
  target: "production",
  createdAt: 1609492210000,
  buildingAt: 1609492210000,
  ready: 1609492210000,
  inspectorUrl: "https://vercel.com/acme/nextjs/J1hXN00qjUeoYfpEEf7dnDtpSiVq"
};
```

----------------------------------------

TITLE: Update URL Protection Bypass using Vercel SDK Class (TypeScript)
DESCRIPTION: Demonstrates how to update the URL protection bypass setting using the `patchUrlProtectionBypass` method available on an instance of the main `Vercel` class. Requires initializing the class with a bearer token and providing the target URL's ID, team ID, and slug.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/aliases/README.md#_snippet_10

LANGUAGE: typescript
CODE:
```
import { Vercel } from "@vercel/sdk";

const vercel = new Vercel({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const result = await vercel.aliases.patchUrlProtectionBypass({
    id: "<id>",
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
    slug: "my-team-url-slug"
  });

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Example models.GetDeploymentEventsResponseBody1 Object (TypeScript)
DESCRIPTION: This snippet demonstrates how to create an instance of the `models.GetDeploymentEventsResponseBody1` type. It shows the expected structure for a 'stdout' type event, including properties like `type`, `created`, and a nested `payload` object with deployment details.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/models/getdeploymenteventsdeploymentsresponsebody.md#_snippet_0

LANGUAGE: typescript
CODE:
```
const value: models.GetDeploymentEventsResponseBody1 = {
  type: "stdout",
  created: 3746.26,
  payload: {
    deploymentId: "<id>",
    id: "<id>",
    date: 9083.71,
    serial: "<value>",
  },
};
```

----------------------------------------

TITLE: Initializing DeleteDomainRequest in TypeScript
DESCRIPTION: This snippet demonstrates how to import the DeleteDomainRequest type and create an instance of it with example values for the domain, team ID, and slug. This object is used to specify the parameters for a domain deletion operation.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/models/deletedomainrequest.md#_snippet_0

LANGUAGE: typescript
CODE:
```
import { DeleteDomainRequest } from "@vercel/sdk/models/deletedomainop.js";

let value: DeleteDomainRequest = {
  domain: "example.com",
  teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
  slug: "my-team-url-slug",
};
```

----------------------------------------

TITLE: Retrieve Configuration using Standalone Function (TypeScript)
DESCRIPTION: This snippet illustrates how to use the standalone `integrationsGetConfiguration` function with `VercelCore` for potentially better tree-shaking. It shows initializing `VercelCore`, calling the function with the core instance and configuration parameters, handling the result, and checking for errors.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/integrations/README.md#_snippet_5

LANGUAGE: typescript
CODE:
```
import { VercelCore } from "@vercel/sdk/core.js";
import { integrationsGetConfiguration } from "@vercel/sdk/funcs/integrationsGetConfiguration.js";

// Use `VercelCore` for best tree-shaking performance.
// You can create one instance of it to use across an application.
const vercel = new VercelCore({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const res = await integrationsGetConfiguration(vercel, {
    id: "icfg_cuwj0AdCdH3BwWT4LPijCC7t",
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
    slug: "my-team-url-slug",
  });

  if (!res.ok) {
    throw res.error;
  }

  const { value: result } = res;

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Importing and Assigning GetDeploymentRoutesHandle in TypeScript
DESCRIPTION: This snippet demonstrates how to import the GetDeploymentRoutesHandle type from the Vercel SDK and assign one of its valid string literal values to a variable.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/models/getdeploymentrouteshandle.md#_snippet_0

LANGUAGE: typescript
CODE:
```
import { GetDeploymentRoutesHandle } from "@vercel/sdk/models/getdeploymentop.js";

let value: GetDeploymentRoutesHandle = "filesystem";
```

----------------------------------------

TITLE: Example Usage of GetAliasResponseBody in TypeScript
DESCRIPTION: This snippet demonstrates how to create and initialize an object conforming to the GetAliasResponseBody interface/type, showing the expected structure and data types for its properties.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/models/getaliasresponsebody.md#_snippet_0

LANGUAGE: typescript
CODE:
```
import { GetAliasResponseBody } from "@vercel/sdk/models/getaliasop.js";

let value: GetAliasResponseBody = {
  alias: "my-alias.vercel.app",
  created: new Date("2017-04-26T23:00:34.232Z"),
  createdAt: 1540095775941,
  creator: {
    uid: "96SnxkFiMyVKsK3pnoHfx3Hz",
    email: "john-doe@gmail.com",
    username: "john-doe"
  },
  deletedAt: 1540095775941,
  deployment: {
    id: "dpl_5m8CQaRBm3FnWRW1od3wKTpaECPx",
    url: "my-instant-deployment-3ij3cxz9qr.now.sh",
    meta: "{}"
  },
  deploymentId: "dpl_5m8CQaRBm3FnWRW1od3wKTpaECPx",
  projectId: "prj_12HKQaOmR5t5Uy6vdcQsNIiZgHGB",
  uid: "<id>",
  updatedAt: 1540095775941
};
```

----------------------------------------

TITLE: Example Usage: UpdateResourceSecretsByIdRequestBody (TypeScript)
DESCRIPTION: This snippet demonstrates how to create an instance of the `UpdateResourceSecretsByIdRequestBody` type with example data for the `secrets` field. It shows the required structure for providing secret names and values.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/models/updateresourcesecretsbyidrequestbody.md#_snippet_0

LANGUAGE: typescript
CODE:
```
import { UpdateResourceSecretsByIdRequestBody } from "@vercel/sdk/models/updateresourcesecretsbyidop.js";

let value: UpdateResourceSecretsByIdRequestBody = {
  secrets: [
    {
      name: "<value>",
      value: "<value>"
    }
  ]
};
```

----------------------------------------

TITLE: Possible Values for GetDeploymentResponseBodyReadyState in TypeScript
DESCRIPTION: Lists the possible string literal values that the `GetDeploymentResponseBodyReadyState` type can hold, representing different states of a deployment.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/models/getdeploymentresponsebodyreadystate.md#_snippet_1

LANGUAGE: typescript
CODE:
```
"QUEUED" | "BUILDING" | "ERROR" | "INITIALIZING" | "READY" | "CANCELED"
```

----------------------------------------

TITLE: Deleting Vercel Alias using Vercel SDK Instance (TypeScript)
DESCRIPTION: This snippet demonstrates how to delete a Vercel alias by creating an instance of the Vercel class and calling the aliases.deleteAlias method. It requires a bearer token for authentication and parameters specifying the alias and team.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/aliases/README.md#_snippet_8

LANGUAGE: typescript
CODE:
```
import { Vercel } from "@vercel/sdk";

const vercel = new Vercel({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const result = await vercel.aliases.deleteAlias({
    aliasId: "<id>",
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
    slug: "my-team-url-slug",
  });

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Configuring Custom HTTP Client with Hooks (TypeScript)
DESCRIPTION: This snippet demonstrates how to initialize the Vercel SDK with a custom HTTP client, providing a custom fetcher function and adding hooks to modify requests (e.g., add headers, set timeout) and handle request errors (e.g., logging).
SOURCE: https://github.com/vercel/sdk/blob/main/README.md#_snippet_18

LANGUAGE: typescript
CODE:
```
import { Vercel } from "@vercel/sdk";
import { HTTPClient } from "@vercel/sdk/lib/http";

const httpClient = new HTTPClient({
  // fetcher takes a function that has the same signature as native `fetch`.
  fetcher: (request) => {
    return fetch(request);
  }
});

httpClient.addHook("beforeRequest", (request) => {
  const nextRequest = new Request(request, {
    signal: request.signal || AbortSignal.timeout(5000)
  });

  nextRequest.headers.set("x-custom-header", "custom value");

  return nextRequest;
});

hTTPClient.addHook("requestError", (error, request) => {
  console.group("Request Error");
  console.log("Reason:", `${error}`);
  console.log("Endpoint:", `${request.method} ${request.url}`);
  console.groupEnd();
});

const sdk = new Vercel({ httpClient });
```

----------------------------------------

TITLE: Initializing PutFirewallConfigMitigate Type in TypeScript
DESCRIPTION: This snippet demonstrates importing the `PutFirewallConfigMitigate` type from the Vercel SDK and initializing a variable with an example configuration, setting the `action` property to 'rate_limit'. This type is used for defining mitigation actions in firewall configurations.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/models/putfirewallconfigmitigate.md#_snippet_0

LANGUAGE: typescript
CODE:
```
import { PutFirewallConfigMitigate } from "@vercel/sdk/models/putfirewallconfigop.js";

let value: PutFirewallConfigMitigate = {
  action: "rate_limit",
};
```

----------------------------------------

TITLE: Example Usage: GetEdgeConfigBackupsResponseBody in TypeScript
DESCRIPTION: This snippet demonstrates how to create an instance of the GetEdgeConfigBackupsResponseBody type in TypeScript, showing the expected structure including backups and pagination details. It requires importing the type definition from the Vercel SDK models.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/models/getedgeconfigbackupsresponsebody.md#_snippet_0

LANGUAGE: typescript
CODE:
```
import { GetEdgeConfigBackupsResponseBody } from "@vercel/sdk/models/getedgeconfigbackupsop.js";

let value: GetEdgeConfigBackupsResponseBody = {
  backups: [
    {
      id: "<id>",
      lastModified: 5313.89,
    },
  ],
  pagination: {
    hasNext: false,
  },
};
```

----------------------------------------

TITLE: TeamRole Possible Values (TypeScript Union Type)
DESCRIPTION: This snippet defines the possible string literal values that the `TeamRole` type can hold, representing different roles within a team.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/models/teamrole.md#_snippet_1

LANGUAGE: typescript
CODE:
```
"OWNER" | "MEMBER" | "DEVELOPER" | "SECURITY" | "BILLING" | "VIEWER" | "CONTRIBUTOR"
```

----------------------------------------

TITLE: Reading Access Group Project using Vercel SDK Class (TypeScript)
DESCRIPTION: Demonstrates how to read an access group project using the Vercel class instance from the @vercel/sdk. It requires a bearer token for authentication and specifies the access group, project, team, and slug. The result is logged to the console.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/sdks/accessgroups/README.md#_snippet_16

LANGUAGE: typescript
CODE:
```
import { Vercel } from "@vercel/sdk";

const vercel = new Vercel({
  bearerToken: "<YOUR_BEARER_TOKEN_HERE>",
});

async function run() {
  const result = await vercel.accessGroups.readAccessGroupProject({
    accessGroupIdOrName: "ag_1a2b3c4d5e6f7g8h9i0j",
    projectId: "prj_ndlgr43fadlPyCtREAqxxdyFK",
    teamId: "team_1a2b3c4d5e6f7g8h9i0j1k2l",
    slug: "my-team-url-slug",
  });

  // Handle the result
  console.log(result);
}

run();
```

----------------------------------------

TITLE: Initializing GitSource12 Model in TypeScript
DESCRIPTION: This snippet demonstrates how to initialize an instance of the `GitSource12` type in TypeScript. It shows the required properties like `type`, `ref`, `sha`, `workspaceUuid`, and `repoUuid` with placeholder values, illustrating the structure needed to define a Git source.
SOURCE: https://github.com/vercel/sdk/blob/main/docs/models/gitsource12.md#_snippet_0

LANGUAGE: typescript
CODE:
```
import { GitSource12 } from "@vercel/sdk/models/canceldeploymentop.js";

let value: GitSource12 = {
  type: "bitbucket",
  ref: "<value>",
  sha: "<value>",
  workspaceUuid: "<id>",
  repoUuid: "<id>",
};
```