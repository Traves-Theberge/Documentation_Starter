TITLE: Creating a Theme Provider in React (TSX)
DESCRIPTION: This snippet defines a `ThemeProvider` component using React Context API to manage the application's theme state (light, dark, system). It persists the theme preference in `localStorage` and dynamically applies CSS classes to the document's root element, also detecting the user's system theme preference. The `useTheme` hook is provided for consuming the theme context.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/dark-mode/vite.mdx#_snippet_0

LANGUAGE: tsx
CODE:
```
import { createContext, useContext, useEffect, useState } from "react"

type Theme = "dark" | "light" | "system"

type ThemeProviderProps = {
  children: React.ReactNode
  defaultTheme?: Theme
  storageKey?: string
}

type ThemeProviderState = {
  theme: Theme
  setTheme: (theme: Theme) => void
}

const initialState: ThemeProviderState = {
  theme: "system",
  setTheme: () => null,
}

const ThemeProviderContext = createContext<ThemeProviderState>(initialState)

export function ThemeProvider({
  children,
  defaultTheme = "system",
  storageKey = "vite-ui-theme",
  ...props
}: ThemeProviderProps) {
  const [theme, setTheme] = useState<Theme>(
    () => (localStorage.getItem(storageKey) as Theme) || defaultTheme
  )

  useEffect(() => {
    const root = window.document.documentElement

    root.classList.remove("light", "dark")

    if (theme === "system") {
      const systemTheme = window.matchMedia("(prefers-color-scheme: dark)")
        .matches
        ? "dark"
        : "light"

      root.classList.add(systemTheme)
      return
    }

    root.classList.add(theme)
  }, [theme])

  const value = {
    theme,
    setTheme: (theme: Theme) => {
      localStorage.setItem(storageKey, theme)
      setTheme(theme)
    },
  }

  return (
    <ThemeProviderContext.Provider {...props} value={value}>
      {children}
    </ThemeProviderContext.Provider>
  )
}

export const useTheme = () => {
  const context = useContext(ThemeProviderContext)

  if (context === undefined)
    throw new Error("useTheme must be used within a ThemeProvider")

  return context
}
```

----------------------------------------

TITLE: Initializing shadcn Project with CLI (Bash)
DESCRIPTION: This command initializes a new shadcn project, installing necessary dependencies, adding the `cn` utility, and configuring CSS variables for styling. It sets up the foundational structure for component integration.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/cli.mdx#_snippet_0

LANGUAGE: bash
CODE:
```
npx shadcn@latest init
```

----------------------------------------

TITLE: Initializing shadcn/ui Project in Next.js (Bash)
DESCRIPTION: This command initializes a new Next.js project or sets up an existing one with shadcn/ui. It prompts the user to choose between a standard Next.js project or a Monorepo setup, configuring the necessary files and dependencies for shadcn/ui integration.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/installation/next.mdx#_snippet_0

LANGUAGE: bash
CODE:
```
npx shadcn@latest init
```

----------------------------------------

TITLE: Initializing shadcn/ui in an Astro project
DESCRIPTION: Runs the `shadcn` CLI's `init` command to set up the necessary configuration files and directories for shadcn/ui within the Astro project. This command prepares the project to integrate shadcn/ui components.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/installation/astro.mdx#_snippet_2

LANGUAGE: bash
CODE:
```
npx shadcn@latest init
```

----------------------------------------

TITLE: Installing Input Component via Shadcn CLI (Bash)
DESCRIPTION: This command uses the Shadcn UI CLI to automatically add the Input component and its dependencies to your project, simplifying the setup process.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/input.mdx#_snippet_0

LANGUAGE: bash
CODE:
```
npx shadcn@latest add input
```

----------------------------------------

TITLE: Installing shadcn/ui Chart Component via CLI
DESCRIPTION: This command-line interface snippet shows how to quickly add the `chart.tsx` component to your project using the shadcn/ui CLI. It's the recommended method for integrating the chart component.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/chart.mdx#_snippet_1

LANGUAGE: bash
CODE:
```
npx shadcn@latest add chart
```

----------------------------------------

TITLE: Installing Shadcn UI Table Component (Bash)
DESCRIPTION: This command uses the `shadcn` CLI to add the basic `<Table />` component to your project, providing the foundational UI elements for building data tables. It sets up the necessary files and dependencies for the component.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/data-table.mdx#_snippet_0

LANGUAGE: bash
CODE:
```
npx shadcn@latest add table
```

----------------------------------------

TITLE: Creating a Reusable DataTable Component with React Table (TypeScript)
DESCRIPTION: This snippet defines a generic `DataTable` component that leverages `@tanstack/react-table` for core table functionalities and shadcn/ui's `Table` components for presentation. It accepts `columns` and `data` as props, dynamically renders headers and rows, and includes a fallback for no results.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/data-table.mdx#_snippet_4

LANGUAGE: tsx
CODE:
```
"use client"

import {
  ColumnDef,
  flexRender,
  getCoreRowModel,
  useReactTable,
} from "@tanstack/react-table"

import {
  Table,
  TableBody,
  TableCell,
  TableHead,
  TableHeader,
  TableRow,
} from "@/components/ui/table"

interface DataTableProps<TData, TValue> {
  columns: ColumnDef<TData, TValue>[]
  data: TData[]
}

export function DataTable<TData, TValue>({
  columns,
  data,
}: DataTableProps<TData, TValue>) {
  const table = useReactTable({
    data,
    columns,
    getCoreRowModel: getCoreRowModel(),
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
                      : flexRender(
                          header.column.columnDef.header,
                          header.getContext()
                        )}
                  </TableHead>
                )
              })}
            </TableRow>
          ))}
        </TableHeader>
        <TableBody>
          {table.getRowModel().rows?.length ? (
            table.getRowModel().rows.map((row) => (
              <TableRow
                key={row.id}
                data-state={row.getIsSelected() && "selected"}
              >
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

TITLE: Initializing shadcn/ui Project with CLI
DESCRIPTION: This command initiates the shadcn/ui project setup process. Running 'npx shadcn@latest init' prompts the user with a series of questions to configure the 'components.json' file, which dictates how components are installed and styled.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/changelog.mdx#_snippet_6

LANGUAGE: bash
CODE:
```
npx shadcn@latest init
```

----------------------------------------

TITLE: Enabling Sorting in TanStack React Table (TSX)
DESCRIPTION: This snippet updates the `DataTable` component to enable column sorting. It introduces `SortingState` and `setSorting` using `React.useState` to manage sorting state. The `useReactTable` hook is configured with `onSortingChange`, `getSortedRowModel`, and `state.sorting` to integrate sorting functionality.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/data-table.mdx#_snippet_10

LANGUAGE: tsx
CODE:
```
"use client"

import * as React from "react"
import {
  ColumnDef,
  SortingState,
  flexRender,
  getCoreRowModel,
  getPaginationRowModel,
  getSortedRowModel,
  useReactTable,
} from "@tanstack/react-table"

export function DataTable<TData, TValue>({
  columns,
  data,
}: DataTableProps<TData, TValue>) {
  const [sorting, setSorting] = React.useState<SortingState>([])

  const table = useReactTable({
    data,
    columns,
    getCoreRowModel: getCoreRowModel(),
    getPaginationRowModel: getPaginationRowModel(),
    onSortingChange: setSorting,
    getSortedRowModel: getSortedRowModel(),
    state: {
      sorting,
    },
  })

  return (
    <div>
      <div className="rounded-md border">
        <Table>{ ... }</Table>
      </div>
    </div>
  )
}
```

----------------------------------------

TITLE: Rendering the DataTable Component in a Page (TypeScript)
DESCRIPTION: This code demonstrates how to integrate and render the `DataTable` component within a Next.js page component. It includes an asynchronous `getData` function to simulate fetching data, which is then passed along with column definitions to the `DataTable` for display.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/data-table.mdx#_snippet_5

LANGUAGE: tsx
CODE:
```
import { Payment, columns } from "./columns"
import { DataTable } from "./data-table"

async function getData(): Promise<Payment[]> {
  // Fetch data from your API here.
  return [
    {
      id: "728ed52f",
      amount: 100,
      status: "pending",
      email: "m@example.com",
    },
    // ...
  ]
}

export default async function DemoPage() {
  const data = await getData()

  return (
    <div className="container mx-auto py-10">
      <DataTable columns={columns} data={data} />
    </div>
  )
}
```

----------------------------------------

TITLE: Adding Specific shadcn Components (Bash)
DESCRIPTION: This command adds a specified shadcn component to your project, automatically installing any required dependencies. Replace `[component]` with the name of the desired component to integrate it into your codebase.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/packages/shadcn/README.md#_snippet_1

LANGUAGE: bash
CODE:
```
npx shadcn add [component]
```

----------------------------------------

TITLE: Defining Global CSS Variables for Light and Dark Themes in CSS
DESCRIPTION: This CSS snippet defines a comprehensive set of custom properties (CSS variables) within the `:root` pseudo-class for the default (light) theme and overrides them within the `.dark` class for dark mode. These variables control various aspects of the UI's appearance, including background, foreground, component-specific colors (card, popover, primary, secondary, muted, accent, destructive), borders, inputs, rings, and chart colors. They are designed to be easily consumed by other CSS rules or JavaScript to maintain a consistent design system.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/theming.mdx#_snippet_11

LANGUAGE: css
CODE:
```
:root {
  --radius: 0.625rem;
  --background: oklch(1 0 0);
  --foreground: oklch(0.145 0 0);
  --card: oklch(1 0 0);
  --card-foreground: oklch(0.145 0 0);
  --popover: oklch(1 0 0);
  --popover-foreground: oklch(0.145 0 0);
  --primary: oklch(0.205 0 0);
  --primary-foreground: oklch(0.985 0 0);
  --secondary: oklch(0.97 0 0);
  --secondary-foreground: oklch(0.205 0 0);
  --muted: oklch(0.97 0 0);
  --muted-foreground: oklch(0.556 0 0);
  --accent: oklch(0.97 0 0);
  --accent-foreground: oklch(0.205 0 0);
  --destructive: oklch(0.577 0.245 27.325);
  --border: oklch(0.922 0 0);
  --input: oklch(0.922 0 0);
  --ring: oklch(0.708 0 0);
  --chart-1: oklch(0.646 0.222 41.116);
  --chart-2: oklch(0.6 0.118 184.704);
  --chart-3: oklch(0.398 0.07 227.392);
  --chart-4: oklch(0.828 0.189 84.429);
  --chart-5: oklch(0.769 0.188 70.08);
  --sidebar: oklch(0.985 0 0);
  --sidebar-foreground: oklch(0.145 0 0);
  --sidebar-primary: oklch(0.205 0 0);
  --sidebar-primary-foreground: oklch(0.985 0 0);
  --sidebar-accent: oklch(0.97 0 0);
  --sidebar-accent-foreground: oklch(0.205 0 0);
  --sidebar-border: oklch(0.922 0 0);
  --sidebar-ring: oklch(0.708 0 0);
}

.dark {
  --background: oklch(0.145 0 0);
  --foreground: oklch(0.985 0 0);
  --card: oklch(0.205 0 0);
  --card-foreground: oklch(0.985 0 0);
  --popover: oklch(0.205 0 0);
  --popover-foreground: oklch(0.985 0 0);
  --primary: oklch(0.922 0 0);
  --primary-foreground: oklch(0.205 0 0);
  --secondary: oklch(0.269 0 0);
  --secondary-foreground: oklch(0.985 0 0);
  --muted: oklch(0.269 0 0);
  --muted-foreground: oklch(0.708 0 0);
  --accent: oklch(0.269 0 0);
  --accent-foreground: oklch(0.985 0 0);
  --destructive: oklch(0.704 0.191 22.216);
  --border: oklch(1 0 0 / 10%);
  --input: oklch(1 0 0 / 15%);
  --ring: oklch(0.556 0 0);
  --chart-1: oklch(0.488 0.243 264.376);
  --chart-2: oklch(0.696 0.17 162.48);
  --chart-3: oklch(0.769 0.188 70.08);
  --chart-4: oklch(0.627 0.265 303.9);
  --chart-5: oklch(0.645 0.246 16.439);
  --sidebar: oklch(0.205 0 0);
  --sidebar-foreground: oklch(0.985 0 0);
  --sidebar-primary: oklch(0.488 0.243 264.376);
  --sidebar-primary-foreground: oklch(0.985 0 0);
  --sidebar-accent: oklch(0.269 0 0);
  --sidebar-accent-foreground: oklch(0.985 0 0);
  --sidebar-border: oklch(1 0 0 / 10%);
  --sidebar-ring: oklch(0.556 0 0);
}
```

----------------------------------------

TITLE: Nesting Link Component with Button asChild in TSX
DESCRIPTION: This TSX example demonstrates using the `asChild` prop on the `Button` component to render its child (`Link` in this case) with button styling, allowing for custom routing behavior.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/button.mdx#_snippet_6

LANGUAGE: tsx
CODE:
```
<Button asChild>
  <Link href="/login">Login</Link>
</Button>
```

----------------------------------------

TITLE: Installing Core Dependencies (npm)
DESCRIPTION: This command installs the essential npm packages required for `shadcn-ui` components, including `class-variance-authority`, `clsx`, `tailwind-merge`, `lucide-react`, and `tw-animate-css`. These dependencies provide styling utilities, component logic, and icon support. It's a prerequisite for using the UI components.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/installation/manual.mdx#_snippet_0

LANGUAGE: bash
CODE:
```
npm install class-variance-authority clsx tailwind-merge lucide-react tw-animate-css
```

----------------------------------------

TITLE: Building a Profile Form with shadcn/ui and React Hook Form (TSX)
DESCRIPTION: This snippet demonstrates how to create a client-side validated form using shadcn/ui's Form components, react-hook-form for state management, and Zod for schema validation. It defines a simple username field with a minimum length requirement and renders the form with an input and submit button.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/form.mdx#_snippet_6

LANGUAGE: TSX
CODE:
```
"use client"

import { zodResolver } from "@hookform/resolvers/zod"
import { useForm } from "react-hook-form"
import { z } from "zod"

import { Button } from "@/components/ui/button"
import {
  Form,
  FormControl,
  FormDescription,
  FormField,
  FormItem,
  FormLabel,
  FormMessage,
} from "@/components/ui/form"
import { Input } from "@/components/ui/input"

const formSchema = z.object({
  username: z.string().min(2, {
    message: "Username must be at least 2 characters.",
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
              <FormDescription>
                This is your public display name.
              </FormDescription>
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

TITLE: Displaying a Basic Toast Notification (TypeScript/React)
DESCRIPTION: This snippet demonstrates the most basic usage of the `toast` function from `sonner`. Calling `toast` with a string argument will display a simple, default toast notification containing that message.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/sonner.mdx#_snippet_5

LANGUAGE: tsx
CODE:
```
toast("Event has been created.")
```

----------------------------------------

TITLE: Configuring shadcn/ui components.json for packages/ui (Tailwind CSS v3)
DESCRIPTION: This configuration defines the `components.json` settings for a shared UI package (`packages/ui`) within a monorepo, specifically for Tailwind CSS v3. It includes the schema, style, RSC, and TSX settings, and explicitly points to the local `tailwind.config.ts` file. Aliases are configured to ensure proper resolution of paths for components, utilities, hooks, and libraries within the workspace.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/monorepo.mdx#_snippet_9

LANGUAGE: json
CODE:
```
{
  "$schema": "https://ui.shadcn.com/schema.json",
  "style": "new-york",
  "rsc": true,
  "tsx": true,
  "tailwind": {
    "config": "tailwind.config.ts",
    "css": "src/styles/globals.css",
    "baseColor": "zinc",
    "cssVariables": true
  },
  "iconLibrary": "lucide",
  "aliases": {
    "components": "@workspace/ui/components",
    "utils": "@workspace/ui/lib/utils",
    "hooks": "@workspace/ui/hooks",
    "lib": "@workspace/ui/lib",
    "ui": "@workspace/ui/components"
  }
}
```

----------------------------------------

TITLE: Configuring Global CSS Variables for Shadcn UI Theming
DESCRIPTION: This CSS snippet defines a comprehensive set of CSS variables for light and dark themes using the `oklch` color format. It imports Tailwind CSS and `tw-animate-css` and sets up base styles, which are crucial for consistent UI theming across the application.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/installation/manual.mdx#_snippet_2

LANGUAGE: css
CODE:
```
@import "tailwindcss";
@import "tw-animate-css";

@custom-variant dark (&:is(.dark *));

:root {
  --background: oklch(1 0 0);
  --foreground: oklch(0.145 0 0);
  --card: oklch(1 0 0);
  --card-foreground: oklch(0.145 0 0);
  --popover: oklch(1 0 0);
  --popover-foreground: oklch(0.145 0 0);
  --primary: oklch(0.205 0 0);
  --primary-foreground: oklch(0.985 0 0);
  --secondary: oklch(0.97 0 0);
  --secondary-foreground: oklch(0.205 0 0);
  --muted: oklch(0.97 0 0);
  --muted-foreground: oklch(0.556 0 0);
  --accent: oklch(0.97 0 0);
  --accent-foreground: oklch(0.205 0 0);
  --destructive: oklch(0.577 0.245 27.325);
  --destructive-foreground: oklch(0.577 0.245 27.325);
  --border: oklch(0.922 0 0);
  --input: oklch(0.922 0 0);
  --ring: oklch(0.708 0 0);
  --chart-1: oklch(0.646 0.222 41.116);
  --chart-2: oklch(0.6 0.118 184.704);
  --chart-3: oklch(0.398 0.07 227.392);
  --chart-4: oklch(0.828 0.189 84.429);
  --chart-5: oklch(0.769 0.188 70.08);
  --radius: 0.625rem;
  --sidebar: oklch(0.985 0 0);
  --sidebar-foreground: oklch(0.145 0 0);
  --sidebar-primary: oklch(0.205 0 0);
  --sidebar-primary-foreground: oklch(0.985 0 0);
  --sidebar-accent: oklch(0.97 0 0);
  --sidebar-accent-foreground: oklch(0.205 0 0);
  --sidebar-border: oklch(0.922 0 0);
  --sidebar-ring: oklch(0.708 0 0);
}

.dark {
  --background: oklch(0.145 0 0);
  --foreground: oklch(0.985 0 0);
  --card: oklch(0.145 0 0);
  --card-foreground: oklch(0.985 0 0);
  --popover: oklch(0.145 0 0);
  --popover-foreground: oklch(0.985 0 0);
  --primary: oklch(0.985 0 0);
  --primary-foreground: oklch(0.205 0 0);
  --secondary: oklch(0.269 0 0);
  --secondary-foreground: oklch(0.985 0 0);
  --muted: oklch(0.269 0 0);
  --muted-foreground: oklch(0.708 0 0);
  --accent: oklch(0.269 0 0);
  --accent-foreground: oklch(0.985 0 0);
  --destructive: oklch(0.396 0.141 25.723);
  --destructive-foreground: oklch(0.637 0.237 25.331);
  --border: oklch(0.269 0 0);
  --input: oklch(0.269 0 0);
  --ring: oklch(0.439 0 0);
  --chart-1: oklch(0.488 0.243 264.376);
  --chart-2: oklch(0.696 0.17 162.48);
  --chart-3: oklch(0.769 0.188 70.08);
  --chart-4: oklch(0.627 0.265 303.9);
  --chart-5: oklch(0.645 0.246 16.439);
  --sidebar: oklch(0.205 0 0);
  --sidebar-foreground: oklch(0.985 0 0);
  --sidebar-primary: oklch(0.488 0.243 264.376);
  --sidebar-primary-foreground: oklch(0.985 0 0);
  --sidebar-accent: oklch(0.269 0 0);
  --sidebar-accent-foreground: oklch(0.985 0 0);
  --sidebar-border: oklch(0.269 0 0);
  --sidebar-ring: oklch(0.439 0 0);
}

@theme inline {
  --color-background: var(--background);
  --color-foreground: var(--foreground);
  --color-card: var(--card);
  --color-card-foreground: var(--card-foreground);
  --color-popover: var(--popover);
  --color-popover-foreground: var(--popover-foreground);
  --color-primary: var(--primary);
  --color-primary-foreground: var(--primary-foreground);
  --color-secondary: var(--secondary);
  --color-secondary-foreground: var(--secondary-foreground);
  --color-muted: var(--muted);
  --color-muted-foreground: var(--muted-foreground);
  --color-accent: var(--accent);
  --color-accent-foreground: var(--accent-foreground);
  --color-destructive: var(--destructive);
  --color-destructive-foreground: var(--destructive-foreground);
  --color-border: var(--border);
  --color-input: var(--input);
  --color-ring: var(--ring);
  --color-chart-1: var(--chart-1);
  --color-chart-2: var(--chart-2);
  --color-chart-3: var(--chart-3);
  --color-chart-4: var(--chart-4);
  --color-chart-5: var(--chart-5);
  --radius-sm: calc(var(--radius) - 4px);
  --radius-md: calc(var(--radius) - 2px);
  --radius-lg: var(--radius);
  --radius-xl: calc(var(--radius) + 4px);
  --color-sidebar: var(--sidebar);
  --color-sidebar-foreground: var(--sidebar-foreground);
  --color-sidebar-primary: var(--sidebar-primary);
  --color-sidebar-primary-foreground: var(--sidebar-primary-foreground);
  --color-sidebar-accent: var(--sidebar-accent);
  --color-sidebar-accent-foreground: var(--sidebar-accent-foreground);
  --color-sidebar-border: var(--sidebar-border);
  --color-sidebar-ring: var(--sidebar-ring);
}

@layer base {
  * {
    @apply border-border outline-ring/50;
  }
  body {
    @apply bg-background text-foreground;
  }
}
```

----------------------------------------

TITLE: Using Custom Link Component with BreadcrumbLink in TSX
DESCRIPTION: This snippet shows how to integrate a custom link component, such as `next/link`, with `BreadcrumbLink` using the `asChild` prop. This allows the breadcrumb links to leverage functionalities from routing libraries, ensuring proper navigation behavior within your application.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/breadcrumb.mdx#_snippet_6

LANGUAGE: tsx
CODE:
```
import { Link } from "next/link"

...

<Breadcrumb>
  <BreadcrumbList>
    <BreadcrumbItem>
      <BreadcrumbLink asChild>
        <Link href="/">Home</Link>
      </BreadcrumbLink>
    </BreadcrumbItem>
    {/* ... */}
  </BreadcrumbList>
</Breadcrumb>
```

----------------------------------------

TITLE: Initializing shadcn/ui Project with CLI (Bash)
DESCRIPTION: This command initializes a `components.json` file in your project, which is essential for using the shadcn/ui CLI to add and manage components. It sets up the basic configuration required for component generation.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components-json.mdx#_snippet_0

LANGUAGE: bash
CODE:
```
npx shadcn@latest init
```

----------------------------------------

TITLE: Creating a Tailwind CSS Class Name Utility Helper in TypeScript
DESCRIPTION: This TypeScript function `cn` combines `clsx` and `tailwind-merge` to conditionally apply and intelligently merge Tailwind CSS classes. It prevents style conflicts and simplifies class management, making it a common and highly useful utility in projects utilizing Tailwind CSS.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/installation/manual.mdx#_snippet_3

LANGUAGE: typescript
CODE:
```
import { clsx, type ClassValue } from "clsx"
import { twMerge } from "tailwind-merge"

export function cn(...inputs: ClassValue[]) {
  return twMerge(clsx(inputs))
}
```

----------------------------------------

TITLE: Initializing shadcn Project Dependencies (Bash)
DESCRIPTION: This command initializes a new shadcn project by installing necessary dependencies, adding the `cn` utility, configuring `tailwind.config.js`, and setting up CSS variables. It is the essential first step for setting up a new project.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/packages/shadcn/README.md#_snippet_0

LANGUAGE: bash
CODE:
```
npx shadcn init
```

----------------------------------------

TITLE: Defining React Hook Form with Zod Resolver and Submit Handler in TSX
DESCRIPTION: This code block illustrates the complete setup of a form using `react-hook-form` with Zod for validation. It shows how to initialize the `useForm` hook with `zodResolver`, set default values, and define an `onSubmit` function to handle form submission, ensuring type-safe access to validated form values.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/form.mdx#_snippet_5

LANGUAGE: tsx
CODE:
```
"use client"

import { zodResolver } from "@hookform/resolvers/zod"
import { useForm } from "react-hook-form"
import { z } from "zod"

const formSchema = z.object({
  username: z.string().min(2, {
    message: "Username must be at least 2 characters."
  })
})

export function ProfileForm() {
  // 1. Define your form.
  const form = useForm<z.infer<typeof formSchema>>({
    resolver: zodResolver(formSchema),
    defaultValues: {
      username: ""
    }
  })

  // 2. Define a submit handler.
  function onSubmit(values: z.infer<typeof formSchema>) {
    // Do something with the form values.
    // ✅ This will be type-safe and validated.
    console.log(values)
  }
}
```

----------------------------------------

TITLE: Installing Navigation Menu via CLI (Bash)
DESCRIPTION: Installs the Shadcn UI Navigation Menu component using the command-line interface, adding it to your project with a single command.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/navigation-menu.mdx#_snippet_0

LANGUAGE: bash
CODE:
```
npx shadcn@latest add navigation-menu
```

----------------------------------------

TITLE: Installing Radio Group Component via CLI (Bash)
DESCRIPTION: This snippet demonstrates how to install the Radio Group component using the shadcn/ui CLI, which automates the setup process by adding the component's code and dependencies to your project.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/radio-group.mdx#_snippet_0

LANGUAGE: bash
CODE:
```
npx shadcn@latest add radio-group
```

----------------------------------------

TITLE: Installing Alert Component via CLI (Bash)
DESCRIPTION: This snippet demonstrates how to install the `Alert` component using the `shadcn/ui` command-line interface (CLI). It's the recommended and simplest way to add the component to your project, handling dependencies automatically.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/alert.mdx#_snippet_0

LANGUAGE: bash
CODE:
```
npx shadcn@latest add alert
```

----------------------------------------

TITLE: Adding a shadcn/ui Component (Button)
DESCRIPTION: This command uses the `shadcn/ui` CLI to add the `Button` component to the project, automatically generating the necessary component files and dependencies.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/installation/vite.mdx#_snippet_8

LANGUAGE: bash
CODE:
```
npx shadcn@latest add button
```

----------------------------------------

TITLE: Basic Dropdown Menu Usage in TSX
DESCRIPTION: This example shows the fundamental structure of a Shadcn UI Dropdown Menu, including the trigger, content, label, separator, and individual menu items, demonstrating a simple interactive menu.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/dropdown-menu.mdx#_snippet_3

LANGUAGE: tsx
CODE:
```
<DropdownMenu>
  <DropdownMenuTrigger>Open</DropdownMenuTrigger>
  <DropdownMenuContent>
    <DropdownMenuLabel>My Account</DropdownMenuLabel>
    <DropdownMenuSeparator />
    <DropdownMenuItem>Profile</DropdownMenuItem>
    <DropdownMenuItem>Billing</DropdownMenuItem>
    <DropdownMenuItem>Team</DropdownMenuItem>
    <DropdownMenuItem>Subscription</DropdownMenuItem>
  </DropdownMenuContent>
</DropdownMenu>
```

----------------------------------------

TITLE: Basic Command Menu Structure in React
DESCRIPTION: This code demonstrates the fundamental structure of a `Command` menu, including an input field, a list to display results, and categorized command items. It shows how to use `CommandInput`, `CommandList`, `CommandEmpty`, `CommandGroup`, `CommandItem`, and `CommandSeparator` to build a functional command interface.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/command.mdx#_snippet_3

LANGUAGE: tsx
CODE:
```
<Command>
  <CommandInput placeholder="Type a command or search..." />
  <CommandList>
    <CommandEmpty>No results found.</CommandEmpty>
    <CommandGroup heading="Suggestions">
      <CommandItem>Calendar</CommandItem>
      <CommandItem>Search Emoji</CommandItem>
      <CommandItem>Calculator</CommandItem>
    </CommandGroup>
    <CommandSeparator />
    <CommandGroup heading="Settings">
      <CommandItem>Profile</CommandItem>
      <CommandItem>Billing</CommandItem>
      <CommandItem>Settings</CommandItem>
    </CommandGroup>
  </CommandList>
</Command>
```

----------------------------------------

TITLE: Implementing Basic Sheet Component (TSX)
DESCRIPTION: This snippet provides a foundational example of how to construct and use the Shadcn UI Sheet component. It demonstrates the basic structure, including a `SheetTrigger` to open the sheet and `SheetContent` containing a `SheetHeader`, `SheetTitle`, and `SheetDescription`.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/sheet.mdx#_snippet_3

LANGUAGE: tsx
CODE:
```
<Sheet>
  <SheetTrigger>Open</SheetTrigger>
  <SheetContent>
    <SheetHeader>
      <SheetTitle>Are you absolutely sure?</SheetTitle>
      <SheetDescription>
        This action cannot be undone. This will permanently delete your account
        and remove your data from our servers.
      </SheetDescription>
    </SheetHeader>
  </SheetContent>
</Sheet>
```

----------------------------------------

TITLE: Basic FormField Usage with React Hook Form in TSX
DESCRIPTION: This example demonstrates how to integrate the `FormField` component with `react-hook-form`'s `useForm` hook. It shows how to pass the `control` object and `name` prop, render an `Input` component, and include `FormLabel`, `FormDescription`, and `FormMessage` for a complete form field.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/form.mdx#_snippet_1

LANGUAGE: tsx
CODE:
```
const form = useForm()

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
```

----------------------------------------

TITLE: Defining Gray Theme CSS Variables in Global Stylesheet
DESCRIPTION: This CSS snippet defines a comprehensive set of custom properties (CSS variables) for a 'Gray' theme, covering various UI elements like background, foreground, cards, popovers, primary/secondary colors, borders, and charts. It includes distinct variable sets for both light (`:root`) and dark (`.dark`) modes, using the `oklch` color format for precise color definition. These variables are intended for global use across the application to ensure consistent styling.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/theming.mdx#_snippet_12

LANGUAGE: css
CODE:
```
:root {
  --radius: 0.625rem;
  --background: oklch(1 0 0);
  --foreground: oklch(0.13 0.028 261.692);
  --card: oklch(1 0 0);
  --card-foreground: oklch(0.13 0.028 261.692);
  --popover: oklch(1 0 0);
  --popover-foreground: oklch(0.13 0.028 261.692);
  --primary: oklch(0.21 0.034 264.665);
  --primary-foreground: oklch(0.985 0.002 247.839);
  --secondary: oklch(0.967 0.003 264.542);
  --secondary-foreground: oklch(0.21 0.034 264.665);
  --muted: oklch(0.967 0.003 264.542);
  --muted-foreground: oklch(0.551 0.027 264.364);
  --accent: oklch(0.967 0.003 264.542);
  --accent-foreground: oklch(0.21 0.034 264.665);
  --destructive: oklch(0.577 0.245 27.325);
  --border: oklch(0.928 0.006 264.531);
  --input: oklch(0.928 0.006 264.531);
  --ring: oklch(0.707 0.022 261.325);
  --chart-1: oklch(0.646 0.222 41.116);
  --chart-2: oklch(0.6 0.118 184.704);
  --chart-3: oklch(0.398 0.07 227.392);
  --chart-4: oklch(0.828 0.189 84.429);
  --chart-5: oklch(0.769 0.188 70.08);
  --sidebar: oklch(0.985 0.002 247.839);
  --sidebar-foreground: oklch(0.13 0.028 261.692);
  --sidebar-primary: oklch(0.21 0.034 264.665);
  --sidebar-primary-foreground: oklch(0.985 0.002 247.839);
  --sidebar-accent: oklch(0.967 0.003 264.542);
  --sidebar-accent-foreground: oklch(0.21 0.034 264.665);
  --sidebar-border: oklch(0.928 0.006 264.531);
  --sidebar-ring: oklch(0.707 0.022 261.325);
}

.dark {
  --background: oklch(0.13 0.028 261.692);
  --foreground: oklch(0.985 0.002 247.839);
  --card: oklch(0.21 0.034 264.665);
  --card-foreground: oklch(0.985 0.002 247.839);
  --popover: oklch(0.21 0.034 264.665);
  --popover-foreground: oklch(0.985 0.002 247.839);
  --primary: oklch(0.928 0.006 264.531);
  --primary-foreground: oklch(0.21 0.034 264.665);
  --secondary: oklch(0.278 0.033 256.848);
  --secondary-foreground: oklch(0.985 0.002 247.839);
  --muted: oklch(0.278 0.033 256.848);
  --muted-foreground: oklch(0.707 0.022 261.325);
  --accent: oklch(0.278 0.033 256.848);
  --accent-foreground: oklch(0.985 0.002 247.839);
  --destructive: oklch(0.704 0.191 22.216);
  --border: oklch(1 0 0 / 10%);
  --input: oklch(1 0 0 / 15%);
  --ring: oklch(0.551 0.027 264.364);
  --chart-1: oklch(0.488 0.243 264.376);
  --chart-2: oklch(0.696 0.17 162.48);
  --chart-3: oklch(0.769 0.188 70.08);
  --chart-4: oklch(0.627 0.265 303.9);
  --chart-5: oklch(0.645 0.246 16.439);
  --sidebar: oklch(0.21 0.034 264.665);
  --sidebar-foreground: oklch(0.985 0.002 247.839);
  --sidebar-primary: oklch(0.488 0.243 264.376);
  --sidebar-primary-foreground: oklch(0.985 0.002 247.839);
  --sidebar-accent: oklch(0.278 0.033 256.848);
  --sidebar-accent-foreground: oklch(0.985 0.002 247.839);
  --sidebar-border: oklch(1 0 0 / 10%);
  --sidebar-ring: oklch(0.551 0.027 264.364);
}
```

----------------------------------------

TITLE: Configuring Zinc Theme CSS Variables in app/globals.css
DESCRIPTION: This CSS snippet defines a comprehensive set of custom properties (variables) for the Zinc theme, supporting both light and dark modes. It specifies colors for general UI elements like background, foreground, border, and input, as well as component-specific colors for cards, popovers, primary, secondary, muted, accent, destructive, ring, and chart elements. Additionally, it includes variables for sidebar theming, ensuring a consistent visual style across the application.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/theming.mdx#_snippet_10

LANGUAGE: css
CODE:
```
:root {
  --radius: 0.625rem;
  --background: oklch(1 0 0);
  --foreground: oklch(0.141 0.005 285.823);
  --card: oklch(1 0 0);
  --card-foreground: oklch(0.141 0.005 285.823);
  --popover: oklch(1 0 0);
  --popover-foreground: oklch(0.141 0.005 285.823);
  --primary: oklch(0.21 0.006 285.885);
  --primary-foreground: oklch(0.985 0 0);
  --secondary: oklch(0.967 0.001 286.375);
  --secondary-foreground: oklch(0.21 0.006 285.885);
  --muted: oklch(0.967 0.001 286.375);
  --muted-foreground: oklch(0.552 0.016 285.938);
  --accent: oklch(0.967 0.001 286.375);
  --accent-foreground: oklch(0.21 0.006 285.885);
  --destructive: oklch(0.577 0.245 27.325);
  --border: oklch(0.92 0.004 286.32);
  --input: oklch(0.92 0.004 286.32);
  --ring: oklch(0.705 0.015 286.067);
  --chart-1: oklch(0.646 0.222 41.116);
  --chart-2: oklch(0.6 0.118 184.704);
  --chart-3: oklch(0.398 0.07 227.392);
  --chart-4: oklch(0.828 0.189 84.429);
  --chart-5: oklch(0.769 0.188 70.08);
  --sidebar: oklch(0.985 0 0);
  --sidebar-foreground: oklch(0.141 0.005 285.823);
  --sidebar-primary: oklch(0.21 0.006 285.885);
  --sidebar-primary-foreground: oklch(0.985 0 0);
  --sidebar-accent: oklch(0.967 0.001 286.375);
  --sidebar-accent-foreground: oklch(0.21 0.006 285.885);
  --sidebar-border: oklch(0.92 0.004 286.32);
  --sidebar-ring: oklch(0.705 0.015 286.067);
}

.dark {
  --background: oklch(0.141 0.005 285.823);
  --foreground: oklch(0.985 0 0);
  --card: oklch(0.21 0.006 285.885);
  --card-foreground: oklch(0.985 0 0);
  --popover: oklch(0.21 0.006 285.885);
  --popover-foreground: oklch(0.985 0 0);
  --primary: oklch(0.92 0.004 286.32);
  --primary-foreground: oklch(0.21 0.006 285.885);
  --secondary: oklch(0.274 0.006 286.033);
  --secondary-foreground: oklch(0.985 0 0);
  --muted: oklch(0.274 0.006 286.033);
  --muted-foreground: oklch(0.705 0.015 286.067);
  --accent: oklch(0.274 0.006 286.033);
  --accent-foreground: oklch(0.985 0 0);
  --destructive: oklch(0.704 0.191 22.216);
  --border: oklch(1 0 0 / 10%);
  --input: oklch(1 0 0 / 15%);
  --ring: oklch(0.552 0.016 285.938);
  --chart-1: oklch(0.488 0.243 264.376);
  --chart-2: oklch(0.696 0.17 162.48);
  --chart-3: oklch(0.769 0.188 70.08);
  --chart-4: oklch(0.627 0.265 303.9);
  --chart-5: oklch(0.645 0.246 16.439);
  --sidebar: oklch(0.21 0.006 285.885);
  --sidebar-foreground: oklch(0.985 0 0);
  --sidebar-primary: oklch(0.488 0.243 264.376);
  --sidebar-primary-foreground: oklch(0.985 0 0);
  --sidebar-accent: oklch(0.274 0.006 286.033);
  --sidebar-accent-foreground: oklch(0.985 0 0);
  --sidebar-border: oklch(1 0 0 / 10%);
  --sidebar-ring: oklch(0.552 0.016 285.938);
}
```

----------------------------------------

TITLE: Composing a Basic Bar Chart with shadcn/ui and Recharts
DESCRIPTION: This snippet demonstrates how to build a basic bar chart by composing Recharts components like `Bar` and `BarChart` with shadcn/ui's custom `ChartContainer` and `ChartTooltipContent`. It highlights the compositional approach, allowing developers to integrate custom UI elements where needed without being locked into an abstraction.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/chart.mdx#_snippet_0

LANGUAGE: tsx
CODE:
```
import { Bar, BarChart } from "recharts"

import { ChartContainer, ChartTooltipContent } from "@/components/ui/charts"

export function MyChart() {
  return (
    <ChartContainer>
      <BarChart data={data}>
        <Bar dataKey="value" />
        <ChartTooltip content={<ChartTooltipContent />} />
      </BarChart>
    </ChartContainer>
  )
}
```

----------------------------------------

TITLE: Initializing shadcn/ui Project
DESCRIPTION: This command runs the `shadcn` CLI to set up and configure `shadcn/ui` within the current project, preparing it for component integration and defining configuration files.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/installation/react-router.mdx#_snippet_1

LANGUAGE: bash
CODE:
```
npx shadcn@latest init
```

----------------------------------------

TITLE: Applying CSS Variables for Theming in TSX
DESCRIPTION: This snippet demonstrates how to apply CSS variables (`--background`, `--foreground`) to HTML elements using Tailwind CSS classes. It shows the recommended approach for theming by referencing pre-defined CSS variables, ensuring consistent styling across the application. This requires `tailwind.cssVariables` to be `true` in `components.json`.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/theming.mdx#_snippet_0

LANGUAGE: tsx
CODE:
```
<div className="bg-background text-foreground" />
```

----------------------------------------

TITLE: Defining Base Stone Colors in CSS
DESCRIPTION: This CSS snippet defines a comprehensive set of base color variables for the 'Stone' palette, including `background`, `foreground`, `primary`, `secondary`, `muted`, `accent`, `destructive`, `border`, `input`, `ring`, and chart-specific colors. It provides distinct values for both light (`:root`) and dark (`.dark`) themes, using OKLCH color format for consistency across the UI.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/theming.mdx#_snippet_9

LANGUAGE: css
CODE:
```
:root {
  --radius: 0.625rem;
  --background: oklch(1 0 0);
  --foreground: oklch(0.147 0.004 49.25);
  --card: oklch(1 0 0);
  --card-foreground: oklch(0.147 0.004 49.25);
  --popover: oklch(1 0 0);
  --popover-foreground: oklch(0.147 0.004 49.25);
  --primary: oklch(0.216 0.006 56.043);
  --primary-foreground: oklch(0.985 0.001 106.423);
  --secondary: oklch(0.97 0.001 106.424);
  --secondary-foreground: oklch(0.216 0.006 56.043);
  --muted: oklch(0.97 0.001 106.424);
  --muted-foreground: oklch(0.553 0.013 58.071);
  --accent: oklch(0.97 0.001 106.424);
  --accent-foreground: oklch(0.216 0.006 56.043);
  --destructive: oklch(0.577 0.245 27.325);
  --border: oklch(0.923 0.003 48.717);
  --input: oklch(0.923 0.003 48.717);
  --ring: oklch(0.709 0.01 56.259);
  --chart-1: oklch(0.646 0.222 41.116);
  --chart-2: oklch(0.6 0.118 184.704);
  --chart-3: oklch(0.398 0.07 227.392);
  --chart-4: oklch(0.828 0.189 84.429);
  --chart-5: oklch(0.769 0.188 70.08);
  --sidebar: oklch(0.985 0.001 106.423);
  --sidebar-foreground: oklch(0.147 0.004 49.25);
  --sidebar-primary: oklch(0.216 0.006 56.043);
  --sidebar-primary-foreground: oklch(0.985 0.001 106.423);
  --sidebar-accent: oklch(0.97 0.001 106.424);
  --sidebar-accent-foreground: oklch(0.216 0.006 56.043);
  --sidebar-border: oklch(0.923 0.003 48.717);
  --sidebar-ring: oklch(0.709 0.01 56.259);
}

.dark {
  --background: oklch(0.147 0.004 49.25);
  --foreground: oklch(0.985 0.001 106.423);
  --card: oklch(0.216 0.006 56.043);
  --card-foreground: oklch(0.985 0.001 106.423);
  --popover: oklch(0.216 0.006 56.043);
  --popover-foreground: oklch(0.985 0.001 106.423);
  --primary: oklch(0.923 0.003 48.717);
  --primary-foreground: oklch(0.216 0.006 56.043);
  --secondary: oklch(0.268 0.007 34.298);
  --secondary-foreground: oklch(0.985 0.001 106.423);
  --muted: oklch(0.268 0.007 34.298);
  --muted-foreground: oklch(0.709 0.01 56.259);
  --accent: oklch(0.268 0.007 34.298);
  --accent-foreground: oklch(0.985 0.001 106.423);
  --destructive: oklch(0.704 0.191 22.216);
  --border: oklch(1 0 0 / 10%);
  --input: oklch(1 0 0 / 15%);
  --ring: oklch(0.553 0.013 58.071);
  --chart-1: oklch(0.488 0.243 264.376);
  --chart-2: oklch(0.696 0.17 162.48);
  --chart-3: oklch(0.769 0.188 70.08);
  --chart-4: oklch(0.627 0.265 303.9);
  --chart-5: oklch(0.645 0.246 16.439);
  --sidebar: oklch(0.216 0.006 56.043);
  --sidebar-foreground: oklch(0.985 0.001 106.423);
  --sidebar-primary: oklch(0.488 0.243 264.376);
  --sidebar-primary-foreground: oklch(0.985 0.001 106.423);
  --sidebar-accent: oklch(0.268 0.007 34.298);
  --sidebar-accent-foreground: oklch(0.985 0.001 106.423);
  --sidebar-border: oklch(1 0 0 / 10%);
  --sidebar-ring: oklch(0.553 0.013 58.071);
}
```

----------------------------------------

TITLE: Defining Global CSS Variables for Shadcn UI Slate Theme
DESCRIPTION: This CSS snippet defines a comprehensive set of custom properties (variables) for the Shadcn UI 'Slate' theme. It includes color definitions for various UI components and states, such as background, foreground, primary, secondary, and accent colors, along with border, input, and ring colors. It also sets a global `--radius` for rounded corners and specific chart and sidebar colors. The variables are defined for both light mode (under `:root`) and dark mode (under `.dark` class) to support theme switching.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/theming.mdx#_snippet_13

LANGUAGE: css
CODE:
```
:root {
  --radius: 0.625rem;
  --background: oklch(1 0 0);
  --foreground: oklch(0.129 0.042 264.695);
  --card: oklch(1 0 0);
  --card-foreground: oklch(0.129 0.042 264.695);
  --popover: oklch(1 0 0);
  --popover-foreground: oklch(0.129 0.042 264.695);
  --primary: oklch(0.208 0.042 265.755);
  --primary-foreground: oklch(0.984 0.003 247.858);
  --secondary: oklch(0.968 0.007 247.896);
  --secondary-foreground: oklch(0.208 0.042 265.755);
  --muted: oklch(0.968 0.007 247.896);
  --muted-foreground: oklch(0.554 0.046 257.417);
  --accent: oklch(0.968 0.007 247.896);
  --accent-foreground: oklch(0.208 0.042 265.755);
  --destructive: oklch(0.577 0.245 27.325);
  --border: oklch(0.929 0.013 255.508);
  --input: oklch(0.929 0.013 255.508);
  --ring: oklch(0.704 0.04 256.788);
  --chart-1: oklch(0.646 0.222 41.116);
  --chart-2: oklch(0.6 0.118 184.704);
  --chart-3: oklch(0.398 0.07 227.392);
  --chart-4: oklch(0.828 0.189 84.429);
  --chart-5: oklch(0.769 0.188 70.08);
  --sidebar: oklch(0.984 0.003 247.858);
  --sidebar-foreground: oklch(0.129 0.042 264.695);
  --sidebar-primary: oklch(0.208 0.042 265.755);
  --sidebar-primary-foreground: oklch(0.984 0.003 247.858);
  --sidebar-accent: oklch(0.968 0.007 247.896);
  --sidebar-accent-foreground: oklch(0.208 0.042 265.755);
  --sidebar-border: oklch(0.929 0.013 255.508);
  --sidebar-ring: oklch(0.704 0.04 256.788);
}

.dark {
  --background: oklch(0.129 0.042 264.695);
  --foreground: oklch(0.984 0.003 247.858);
  --card: oklch(0.208 0.042 265.755);
  --card-foreground: oklch(0.984 0.003 247.858);
  --popover: oklch(0.208 0.042 265.755);
  --popover-foreground: oklch(0.984 0.003 247.858);
  --primary: oklch(0.929 0.013 255.508);
  --primary-foreground: oklch(0.208 0.042 265.755);
  --secondary: oklch(0.279 0.041 260.031);
  --secondary-foreground: oklch(0.984 0.003 247.858);
  --muted: oklch(0.279 0.041 260.031);
  --muted-foreground: oklch(0.704 0.04 256.788);
  --accent: oklch(0.279 0.041 260.031);
  --accent-foreground: oklch(0.984 0.003 247.858);
  --destructive: oklch(0.704 0.191 22.216);
  --border: oklch(1 0 0 / 10%);
  --input: oklch(1 0 0 / 15%);
  --ring: oklch(0.551 0.027 264.364);
  --chart-1: oklch(0.488 0.243 264.376);
  --chart-2: oklch(0.696 0.17 162.48);
  --chart-3: oklch(0.769 0.188 70.08);
  --chart-4: oklch(0.627 0.265 303.9);
  --chart-5: oklch(0.645 0.246 16.439);
  --sidebar: oklch(0.208 0.042 265.755);
  --sidebar-foreground: oklch(0.984 0.003 247.858);
  --sidebar-primary: oklch(0.488 0.243 264.376);
  --sidebar-primary-foreground: oklch(0.984 0.003 247.858);
  --sidebar-accent: oklch(0.279 0.041 260.031);
  --sidebar-accent-foreground: oklch(0.984 0.003 247.858);
  --sidebar-border: oklch(1 0 0 / 10%);
  --sidebar-ring: oklch(0.551 0.027 264.364);
}
```

----------------------------------------

TITLE: Implementing Command Menu as a Dialog in React
DESCRIPTION: This React component demonstrates how to integrate the `Command` menu within a `CommandDialog`, allowing it to be toggled by a keyboard shortcut (Cmd+K or Ctrl+K). It manages the dialog's open state and includes the standard command menu elements like input, list, and items.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/command.mdx#_snippet_4

LANGUAGE: tsx
CODE:
```
export function CommandMenu() {
  const [open, setOpen] = React.useState(false)

  React.useEffect(() => {
    const down = (e: KeyboardEvent) => {
      if (e.key === "k" && (e.metaKey || e.ctrlKey)) {
        e.preventDefault()
        setOpen((open) => !open)
      }
    }
    document.addEventListener("keydown", down)
    return () => document.removeEventListener("keydown", down)
  }, [])

  return (
    <CommandDialog open={open} onOpenChange={setOpen}>
      <CommandInput placeholder="Type a command or search..." />
      <CommandList>
        <CommandEmpty>No results found.</CommandEmpty>
        <CommandGroup heading="Suggestions">
          <CommandItem>Calendar</CommandItem>
          <CommandItem>Search Emoji</CommandItem>
          <CommandItem>Calculator</CommandItem>
        </CommandGroup>
      </CommandList>
    </CommandDialog>
  )
}
```

----------------------------------------

TITLE: Importing Select Components (TypeScript/React)
DESCRIPTION: This snippet imports the core components of the shadcn/ui Select, including `Select`, `SelectContent`, `SelectItem`, `SelectTrigger`, and `SelectValue`, from the local UI components directory. These imports are essential for composing a functional Select component in a React application.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/select.mdx#_snippet_2

LANGUAGE: tsx
CODE:
```
import {
  Select,
  SelectContent,
  SelectItem,
  SelectTrigger,
  SelectValue,
} from "@/components/ui/select"
```

----------------------------------------

TITLE: Importing Dropdown Menu Components in TSX
DESCRIPTION: This snippet demonstrates how to import the necessary components for the Shadcn UI Dropdown Menu from the local UI components path, making them available for use in a React/TSX application.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/dropdown-menu.mdx#_snippet_2

LANGUAGE: tsx
CODE:
```
import {
  DropdownMenu,
  DropdownMenuContent,
  DropdownMenuItem,
  DropdownMenuLabel,
  DropdownMenuSeparator,
  DropdownMenuTrigger
} from "@/components/ui/dropdown-menu"
```

----------------------------------------

TITLE: Importing Pagination Components in TSX
DESCRIPTION: This code snippet shows the required import statements for all sub-components of the shadcn-ui Pagination component. These imports are essential for using the pagination elements within a TypeScript React (TSX) application.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/pagination.mdx#_snippet_1

LANGUAGE: tsx
CODE:
```
import {
  Pagination,
  PaginationContent,
  PaginationEllipsis,
  PaginationItem,
  PaginationLink,
  PaginationNext,
  PaginationPrevious,
} from "@/components/ui/pagination"
```

----------------------------------------

TITLE: Importing Context Menu Components in TSX
DESCRIPTION: This snippet demonstrates how to import the essential Context Menu components (ContextMenu, ContextMenuContent, ContextMenuItem, ContextMenuTrigger) from the shadcn/ui library into a TypeScript React file for use.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/context-menu.mdx#_snippet_2

LANGUAGE: tsx
CODE:
```
import {
  ContextMenu,
  ContextMenuContent,
  ContextMenuItem,
  ContextMenuTrigger,
} from "@/components/ui/context-menu"
```

----------------------------------------

TITLE: Configuring `components.json` for CSS Variable Theming
DESCRIPTION: This JSON snippet illustrates the configuration required in `components.json` to enable CSS variable-based theming. The key setting is `"cssVariables": true` within the `tailwind` object, which instructs the system to use CSS variables for color management. This setup is crucial for the `bg-background` and `text-foreground` classes to resolve correctly.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/theming.mdx#_snippet_1

LANGUAGE: json
CODE:
```
{
  "style": "default",
  "rsc": true,
  "tailwind": {
    "config": "",
    "css": "app/globals.css",
    "baseColor": "neutral",
    "cssVariables": true
  },
  "aliases": {
    "components": "@/components",
    "utils": "@/lib/utils",
    "ui": "@/registry/new-york-v4/ui",
    "lib": "@/lib",
    "hooks": "@/hooks"
  },
  "iconLibrary": "lucide"
}
```

----------------------------------------

TITLE: Importing and using the shadcn/ui Switch component in TSX
DESCRIPTION: This TypeScript React (TSX) snippet demonstrates how to import the `Switch` component from the local `components/ui/switch` path. It then shows how to render the `Switch` component within a functional React component named `MyPage`, illustrating its basic usage in a UI.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/installation/laravel.mdx#_snippet_2

LANGUAGE: tsx
CODE:
```
import { Switch } from "@/components/ui/switch"

const MyPage = () => {
  return (
    <div>
      <Switch />
    </div>
  )
}

export default MyPage
```

----------------------------------------

TITLE: Using Navigation Menu Item with Next.js Link (TSX)
DESCRIPTION: Illustrates how to integrate a NavigationMenuItem with Next.js's <Link /> component, ensuring proper styling by applying navigationMenuTriggerStyle() to the NavigationMenuLink for client-side navigation.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/navigation-menu.mdx#_snippet_5

LANGUAGE: tsx
CODE:
```
<NavigationMenuItem>
  <Link href="/docs" legacyBehavior passHref>
    <NavigationMenuLink className={navigationMenuTriggerStyle()}>
      Documentation
    </NavigationMenuLink>
  </Link>
</NavigationMenuItem>
```

----------------------------------------

TITLE: Implementing Email Filtering in React Table DataTable Component (TypeScript)
DESCRIPTION: This snippet modifies the `DataTable` component to include email filtering functionality. It introduces `ColumnFiltersState` and `getFilteredRowModel` from `@tanstack/react-table` and adds an `Input` component from Shadcn UI, allowing users to filter the table data based on the 'email' column.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/data-table.mdx#_snippet_12

LANGUAGE: TypeScript
CODE:
```
"use client"

import * as React from "react"
import {
  ColumnDef,
  ColumnFiltersState,
  SortingState,
  flexRender,
  getCoreRowModel,
  getFilteredRowModel,
  getPaginationRowModel,
  getSortedRowModel,
  useReactTable,
} from "@tanstack/react-table"

import { Button } from "@/components/ui/button"
import { Input } from "@/components/ui/input"

export function DataTable<TData, TValue>({
  columns,
  data,
}: DataTableProps<TData, TValue>) {
  const [sorting, setSorting] = React.useState<SortingState>([])
  const [columnFilters, setColumnFilters] = React.useState<ColumnFiltersState>(
    []
  )

  const table = useReactTable({
    data,
    columns,
    onSortingChange: setSorting,
    getCoreRowModel: getCoreRowModel(),
    getPaginationRowModel: getPaginationRowModel(),
    getSortedRowModel: getSortedRowModel(),
    onColumnFiltersChange: setColumnFilters,
    getFilteredRowModel: getFilteredRowModel(),
    state: {
      sorting,
      columnFilters,
    },
  })

  return (
    <div>
      <div className="flex items-center py-4">
        <Input
          placeholder="Filter emails..."
          value={(table.getColumn("email")?.getFilterValue() as string) ?? ""}
          onChange={(event) =>
            table.getColumn("email")?.setFilterValue(event.target.value)
          }
          className="max-w-sm"
        />
      </div>
      <div className="rounded-md border">
        <Table>{ ... }</Table>
      </div>
    </div>
  )
}
```

----------------------------------------

TITLE: Form Component Anatomy in TSX
DESCRIPTION: This snippet illustrates the basic structural components of a form using `shadcn/ui`'s `Form` and `FormField` components. It demonstrates the nesting of `FormItem`, `FormLabel`, `FormControl`, `FormDescription`, and `FormMessage` to create a well-structured and accessible form field.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/form.mdx#_snippet_0

LANGUAGE: tsx
CODE:
```
<Form>
  <FormField
    control={...}
    name="..."
    render={() => (
      <FormItem>
        <FormLabel />
        <FormControl>
          { /* Your form field */}
        </FormControl>
        <FormDescription />
        <FormMessage />
      </FormItem>
    )}
  />
</Form>
```

----------------------------------------

TITLE: Defining Custom Theme (JSON)
DESCRIPTION: This JSON configuration defines a custom `shadcn/ui` theme. It specifies a variety of CSS variables for `background`, `foreground`, `primary`, `primary-foreground`, `ring`, `sidebar-primary`, `sidebar-primary-foreground`, and `sidebar-ring` for both light and dark modes using `oklch` color format. This allows for comprehensive theme customization.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/registry/examples.mdx#_snippet_2

LANGUAGE: json
CODE:
```
{
  "$schema": "https://ui.shadcn.com/schema/registry-item.json",
  "name": "custom-theme",
  "type": "registry:theme",
  "cssVars": {
    "light": {
      "background": "oklch(1 0 0)",
      "foreground": "oklch(0.141 0.005 285.823)",
      "primary": "oklch(0.546 0.245 262.881)",
      "primary-foreground": "oklch(0.97 0.014 254.604)",
      "ring": "oklch(0.746 0.16 232.661)",
      "sidebar-primary": "oklch(0.546 0.245 262.881)",
      "sidebar-primary-foreground": "oklch(0.97 0.014 254.604)",
      "sidebar-ring": "oklch(0.746 0.16 232.661)"
    },
    "dark": {
      "background": "oklch(1 0 0)",
      "foreground": "oklch(0.141 0.005 285.823)",
      "primary": "oklch(0.707 0.165 254.624)",
      "primary-foreground": "oklch(0.97 0.014 254.604)",
      "ring": "oklch(0.707 0.165 254.624)",
      "sidebar-primary": "oklch(0.707 0.165 254.624)",
      "sidebar-primary-foreground": "oklch(0.97 0.014 254.604)",
      "sidebar-ring": "oklch(0.707 0.165 254.624)"
    }
  }
}
```

----------------------------------------

TITLE: Fetching Project Data for Sidebar Menu with SWR in TSX
DESCRIPTION: This component uses the SWR library to fetch project data. It displays a loading skeleton while data is being fetched and renders the project list once the data is available. It also includes a placeholder for handling no data.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/sidebar.mdx#_snippet_43

LANGUAGE: tsx
CODE:
```
function NavProjects() {
  const { data, isLoading } = useSWR("/api/projects", fetcher)

  if (isLoading) {
    return (
      <SidebarMenu>
        {Array.from({ length: 5 }).map((_, index) => (
          <SidebarMenuItem key={index}>
            <SidebarMenuSkeleton showIcon />
          </SidebarMenuItem>
        ))}
      </SidebarMenu>
    )
  }

  if (!data) {
    return ...
  }

  return (
    <SidebarMenu>
      {data.map((project) => (
        <SidebarMenuItem key={project.name}>
          <SidebarMenuButton asChild>
            <a href={project.url}>
              <project.icon />
              <span>{project.name}</span>
            </a>
          </SidebarMenuButton>
        </SidebarMenuItem>
      ))}
    </SidebarMenu>
  )
}
```

----------------------------------------

TITLE: Initializing shadcn/ui Project
DESCRIPTION: This command runs the `shadcn/ui` CLI initialization process, which prompts the user with questions to configure the `components.json` file, setting up the project for `shadcn/ui` components.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/installation/vite.mdx#_snippet_7

LANGUAGE: bash
CODE:
```
npx shadcn@latest init
```

----------------------------------------

TITLE: Formatting Amount Cell in Column Definition (TypeScript)
DESCRIPTION: This snippet illustrates how to customize the `header` and `cell` properties within a `ColumnDef` for the 'amount' column. It uses `Intl.NumberFormat` to format numerical amounts as currency (USD) and aligns the text to the right, enhancing data readability.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/data-table.mdx#_snippet_6

LANGUAGE: tsx
CODE:
```
export const columns: ColumnDef<Payment>[] = [
  {
    accessorKey: "amount",
    header: () => <div className="text-right">Amount</div>,
    cell: ({ row }) => {
      const amount = parseFloat(row.getValue("amount"))
      const formatted = new Intl.NumberFormat("en-US", {
        style: "currency",
        currency: "USD",
      }).format(amount)

      return <div className="text-right font-medium">{formatted}</div>
    },
  },
]
```

----------------------------------------

TITLE: Implementing a Basic Combobox Component in React/TSX
DESCRIPTION: This snippet demonstrates how to create a basic Combobox component using shadcn/ui's Popover and Command components. It includes state management for opening/closing the combobox and filtering a predefined list of frameworks. Dependencies include `lucide-react` for icons, `@/lib/utils` for utility functions, and shadcn/ui components like `Button`, `Command`, and `Popover`.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/combobox.mdx#_snippet_0

LANGUAGE: tsx
CODE:
```
"use client"\n\nimport * as React from "react"\nimport { Check, ChevronsUpDown } from "lucide-react"\n\nimport { cn } from "@/lib/utils"\nimport { Button } from "@/components/ui/button"\nimport {\n  Command,\n  CommandEmpty,\n  CommandGroup,\n  CommandInput,\n  CommandItem,\n  CommandList,\n} from "@/components/ui/command"\nimport {\n  Popover,\n  PopoverContent,\n  PopoverTrigger,\n} from "@/components/ui/popover"\n\nconst frameworks = [\n  {\n    value: "next.js",\n    label: "Next.js",\n  },\n  {\n    value: "sveltekit",\n    label: "SvelteKit",\n  },\n  {\n    value: "nuxt.js",\n    label: "Nuxt.js",\n  },\n  {\n    value: "remix",\n    label: "Remix",\n  },\n  {\n    value: "astro",\n    label: "Astro",\n  },\n]\n\nexport function ComboboxDemo() {\n  const [open, setOpen] = React.useState(false)\n  const [value, setValue] = React.useState("")\n\n  return (\n    <Popover open={open} onOpenChange={setOpen}>\n      <PopoverTrigger asChild>\n        <Button\n          variant="outline"\n          role="combobox"\n          aria-expanded={open}\n          className="w-[200px] justify-between"\n        >\n          {value\n            ? frameworks.find((framework) => framework.value === value)?.label\n            : "Select framework..."}\n          <ChevronsUpDown className="ml-2 h-4 w-4 shrink-0 opacity-50" />\n        </Button>\n      </PopoverTrigger>\n      <PopoverContent className="w-[200px] p-0">\n        <Command>\n          <CommandInput placeholder="Search framework..." />\n          <CommandList>\n            <CommandEmpty>No framework found.</CommandEmpty>\n            <CommandGroup>\n              {frameworks.map((framework) => (\n                <CommandItem\n                  key={framework.value}\n                  value={framework.value}\n                  onSelect={(currentValue) => {\n                    setValue(currentValue === value ? "" : currentValue)\n                    setOpen(false)\n                  }}\n                >\n                  <Check\n                    className={cn(\n                      "mr-2 h-4 w-4",\n                      value === framework.value ? "opacity-100" : "opacity-0"\n                    )}\n                  />\n                  {framework.label}\n                </CommandItem>\n              ))}\n            </CommandGroup>\n          </CommandList>\n        </Command>\n      </PopoverContent>\n    </Popover>\n  )\n}
```

----------------------------------------

TITLE: Wrapping Root Layout with ThemeProvider (TypeScript/React)
DESCRIPTION: This snippet demonstrates integrating the custom `ThemeProvider` into the main `RootLayout` of a Next.js application. The `suppressHydrationWarning` prop on the `html` tag is crucial to prevent hydration errors related to theme changes, and various `next-themes` props are configured for robust theme management.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/dark-mode/next.mdx#_snippet_2

LANGUAGE: tsx
CODE:
```
import { ThemeProvider } from "@/components/theme-provider"

export default function RootLayout({ children }: RootLayoutProps) {
  return (
    <>
      <html lang="en" suppressHydrationWarning>
        <head />
        <body>
          <ThemeProvider
            attribute="class"
            defaultTheme="system"
            enableSystem
            disableTransitionOnChange
          >
            {children}
          </ThemeProvider>
        </body>
      </html>
    </>
  )
}
```

----------------------------------------

TITLE: Basic Table Component Usage (shadcn/ui) in TSX
DESCRIPTION: This snippet illustrates a basic implementation of the shadcn/ui Table component in a TypeScript React (TSX) application. It demonstrates how to structure a table with a caption, header, and a single row of data, using `Table`, `TableCaption`, `TableHeader`, `TableRow`, `TableHead`, `TableBody`, and `TableCell` components to display invoice details.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/table.mdx#_snippet_2

LANGUAGE: tsx
CODE:
```
<Table>
  <TableCaption>A list of your recent invoices.</TableCaption>
  <TableHeader>
    <TableRow>
      <TableHead className="w-[100px]">Invoice</TableHead>
      <TableHead>Status</TableHead>
      <TableHead>Method</TableHead>
      <TableHead className="text-right">Amount</TableHead>
    </TableRow>
  </TableHeader>
  <TableBody>
    <TableRow>
      <TableCell className="font-medium">INV001</TableCell>
      <TableCell>Paid</TableCell>
      <TableCell>Credit Card</TableCell>
      <TableCell className="text-right">$250.00</TableCell>
    </TableRow>
  </TableBody>
</Table>
```

----------------------------------------

TITLE: Implementing Shadcn UI Sheet Component in TypeScript
DESCRIPTION: This snippet provides the full implementation of the Shadcn UI Sheet component using React and Radix UI primitives. It defines the core `Sheet` component along with `SheetTrigger`, `SheetClose`, `SheetPortal`, `SheetOverlay`, `SheetContent`, `SheetHeader`, `SheetFooter`, `SheetTitle`, and `SheetDescription`. It utilizes `class-variance-authority` for dynamic styling based on the `side` prop (top, bottom, left, right) and `cn` utility for combining class names.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/changelog.mdx#_snippet_24

LANGUAGE: tsx
CODE:
```
"use client"

import * as React from "react"
import * as SheetPrimitive from "@radix-ui/react-dialog"
import { cva, type VariantProps } from "class-variance-authority"
import { X } from "lucide-react"

import { cn } from "@/lib/utils"

const Sheet = SheetPrimitive.Root

const SheetTrigger = SheetPrimitive.Trigger

const SheetClose = SheetPrimitive.Close

const SheetPortal = ({
  className,
  ...props
}: SheetPrimitive.DialogPortalProps) => (
  <SheetPrimitive.Portal className={cn(className)} {...props} />
)
SheetPortal.displayName = SheetPrimitive.Portal.displayName

const SheetOverlay = React.forwardRef<
  React.ElementRef<typeof SheetPrimitive.Overlay>,
  React.ComponentPropsWithoutRef<typeof SheetPrimitive.Overlay>
>(({ className, ...props }, ref) => (
  <SheetPrimitive.Overlay
    className={cn(
      "fixed inset-0 z-50 bg-background/80 backdrop-blur-sm data-[state=open]:animate-in data-[state=closed]:animate-out data-[state=closed]:fade-out-0 data-[state=open]:fade-in-0",
      className
    )}
    {...props}
    ref={ref}
  />
))
SheetOverlay.displayName = SheetPrimitive.Overlay.displayName

const sheetVariants = cva(
  "fixed z-50 gap-4 bg-background p-6 shadow-lg transition ease-in-out data-[state=open]:animate-in data-[state=closed]:animate-out data-[state=closed]:duration-300 data-[state=open]:duration-500",
  {
    variants: {
      side: {
        top: "inset-x-0 top-0 border-b data-[state=closed]:slide-out-to-top data-[state=open]:slide-in-from-top",
        bottom:
          "inset-x-0 bottom-0 border-t data-[state=closed]:slide-out-to-bottom data-[state=open]:slide-in-from-bottom",
        left: "inset-y-0 left-0 h-full w-3/4 border-r data-[state=closed]:slide-out-to-left data-[state=open]:slide-in-from-left sm:max-w-sm",
        right:
          "inset-y-0 right-0 h-full w-3/4  border-l data-[state=closed]:slide-out-to-right data-[state=open]:slide-in-from-right sm:max-w-sm"
      }
    },
    defaultVariants: {
      side: "right"
    }
  }
)

interface SheetContentProps
  extends React.ComponentPropsWithoutRef<typeof SheetPrimitive.Content>,
    VariantProps<typeof sheetVariants> {}

const SheetContent = React.forwardRef<
  React.ElementRef<typeof SheetPrimitive.Content>,
  SheetContentProps
>(({ side = "right", className, children, ...props }, ref) => (
  <SheetPortal>
    <SheetOverlay />
    <SheetPrimitive.Content
      ref={ref}
      className={cn(sheetVariants({ side }), className)}
      {...props}
    >
      {children}
      <SheetPrimitive.Close className="absolute right-4 top-4 rounded-sm opacity-70 ring-offset-background transition-opacity hover:opacity-100 focus:outline-none focus:ring-2 focus:ring-ring focus:ring-offset-2 disabled:pointer-events-none data-[state=open]:bg-secondary">
        <X className="h-4 w-4" />
        <span className="sr-only">Close</span>
      </SheetPrimitive.Close>
    </SheetPrimitive.Content>
  </SheetPortal>
))
SheetContent.displayName = SheetPrimitive.Content.displayName

const SheetHeader = ({
  className,
  ...props
}: React.HTMLAttributes<HTMLDivElement>) => (
  <div
    className={cn(
      "flex flex-col space-y-2 text-center sm:text-left",
      className
    )}
    {...props}
  />
)
SheetHeader.displayName = "SheetHeader"

const SheetFooter = ({
  className,
  ...props
}: React.HTMLAttributes<HTMLDivElement>) => (
  <div
    className={cn(
      "flex flex-col-reverse sm:flex-row sm:justify-end sm:space-x-2",
      className
    )}
    {...props}
  />
)
SheetFooter.displayName = "SheetFooter"

const SheetTitle = React.forwardRef<
  React.ElementRef<typeof SheetPrimitive.Title>,
  React.ComponentPropsWithoutRef<typeof SheetPrimitive.Title>
>(({ className, ...props }, ref) => (
  <SheetPrimitive.Title
    ref={ref}
    className={cn("text-lg font-semibold text-foreground", className)}
    {...props}
  />
))
SheetTitle.displayName = SheetPrimitive.Title.displayName

const SheetDescription = React.forwardRef<
  React.ElementRef<typeof SheetPrimitive.Description>,
  React.ComponentPropsWithoutRef<typeof SheetPrimitive.Description>
>(({ className, ...props }, ref) => (
  <SheetPrimitive.Description
    ref={ref}
    className={cn("text-sm text-muted-foreground", className)}
    {...props}
  />
))
SheetDescription.displayName = SheetPrimitive.Description.displayName

export {
  Sheet,
  SheetTrigger,
  SheetClose,
  SheetContent,
  SheetHeader,
  SheetFooter,
  SheetTitle,
  SheetDescription,
}
```

----------------------------------------

TITLE: Using Alert Component (TypeScript/React)
DESCRIPTION: This example illustrates how to use the `Alert` component in a TypeScript/React application. It includes an icon, a title, and a description, demonstrating the basic structure and content of a typical alert message.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/alert.mdx#_snippet_2

LANGUAGE: tsx
CODE:
```
<Alert>
  <Terminal className="h-4 w-4" />
  <AlertTitle>Heads up!</AlertTitle>
  <AlertDescription>
    You can add components and dependencies to your app using the cli.
  </AlertDescription>
</Alert>
```

----------------------------------------

TITLE: Implementing a Basic Single Accordion (TypeScript/React)
DESCRIPTION: This TypeScript/React example showcases a minimal implementation of the Accordion component, configured as a single type and collapsible. It includes a single AccordionItem with a Trigger for interaction and Content to display information.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/accordion.mdx#_snippet_4

LANGUAGE: tsx
CODE:
```
<Accordion type="single" collapsible>
  <AccordionItem value="item-1">
    <AccordionTrigger>Is it accessible?</AccordionTrigger>
    <AccordionContent>
      Yes. It adheres to the WAI-ARIA design pattern.
    </AccordionContent>
  </AccordionItem>
</Accordion>
```

----------------------------------------

TITLE: Importing Slider Component in TSX
DESCRIPTION: This line imports the Slider component from the local UI components library, making it available for use within a TypeScript or TSX file.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/slider.mdx#_snippet_2

LANGUAGE: tsx
CODE:
```
import { Slider } from "@/components/ui/slider"
```

----------------------------------------

TITLE: Basic Context Menu Usage in TSX
DESCRIPTION: This example illustrates the fundamental structure of a Context Menu, including the trigger element and the content containing various menu items. Right-clicking the trigger will display the associated menu.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/context-menu.mdx#_snippet_3

LANGUAGE: tsx
CODE:
```
<ContextMenu>
  <ContextMenuTrigger>Right click</ContextMenuTrigger>
  <ContextMenuContent>
    <ContextMenuItem>Profile</ContextMenuItem>
    <ContextMenuItem>Billing</ContextMenuItem>
    <ContextMenuItem>Team</ContextMenuItem>
    <ContextMenuItem>Subscription</ContextMenuItem>
  </ContextMenuContent>
</ContextMenu>
```

----------------------------------------

TITLE: Forcing npm Package Installation with React 19 Compatibility
DESCRIPTION: These commands provide two methods to force the installation of npm packages, bypassing peer dependency warnings related to React 19. The `--force` flag ignores all conflicts, while `--legacy-peer-deps` specifically skips strict peer dependency checks, allowing installation despite unmet requirements.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/react-19.mdx#_snippet_2

LANGUAGE: bash
CODE:
```
npm i <package> --force

npm i <package> --legacy-peer-deps
```

----------------------------------------

TITLE: Initializing a New Project with shadcn/ui CLI (Bash)
DESCRIPTION: This command demonstrates how to initialize a new shadcn/ui project using the updated CLI. It allows for the immediate installation of specified components, such as 'sidebar-01' and 'login-01', directly during the project setup phase. This is part of the new CLI's enhanced capabilities for streamlined project creation and component integration.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/changelog.mdx#_snippet_0

LANGUAGE: bash
CODE:
```
npx shadcn init sidebar-01 login-01
```

----------------------------------------

TITLE: Using shadcn/ui Button Component in React
DESCRIPTION: This React component demonstrates how to import and use the `Button` component from `shadcn/ui` within an `App` component, displaying a clickable button centered on the screen.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/installation/vite.mdx#_snippet_9

LANGUAGE: tsx
CODE:
```
import { Button } from "@/components/ui/button"

function App() {
  return (
    <div className="flex flex-col items-center justify-center min-h-svh">
      <Button>Click me</Button>
    </div>
  )
}

export default App
```

----------------------------------------

TITLE: Installing Avatar Component via CLI (Bash)
DESCRIPTION: This command uses the shadcn CLI to automatically add and configure the Avatar component within your project, simplifying the installation process.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/avatar.mdx#_snippet_0

LANGUAGE: bash
CODE:
```
npx shadcn@latest add avatar
```

----------------------------------------

TITLE: Installing Switch Component via CLI (Bash)
DESCRIPTION: This command uses the `shadcn/ui` CLI to automatically add the `Switch` component and its dependencies to your project. It's the recommended and simplest installation method.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/switch.mdx#_snippet_0

LANGUAGE: bash
CODE:
```
npx shadcn@latest add switch
```

----------------------------------------

TITLE: Installing Resizable Component via Shadcn CLI (Bash)
DESCRIPTION: This command uses the `npx shadcn` CLI tool to automatically add the `resizable` component to your project. It handles dependency installation and component file generation, streamlining the setup process.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/resizable.mdx#_snippet_0

LANGUAGE: bash
CODE:
```
npx shadcn@latest add resizable
```

----------------------------------------

TITLE: Installing Shadcn UI Accordion via CLI (Bash)
DESCRIPTION: This Bash command utilizes the shadcn/ui CLI to streamline the installation of the Accordion component, automatically adding it and its required dependencies to your project. It's the quickest way to get started with the component.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/accordion.mdx#_snippet_0

LANGUAGE: bash
CODE:
```
npx shadcn@latest add accordion
```

----------------------------------------

TITLE: Installing Shadcn UI Sidebar Component via CLI
DESCRIPTION: This command uses the `npx shadcn@latest add` utility to automatically install the `sidebar.tsx` component into your project, simplifying the setup process for the Shadcn UI sidebar.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/sidebar.mdx#_snippet_0

LANGUAGE: bash
CODE:
```
npx shadcn@latest add sidebar
```

----------------------------------------

TITLE: Configuring tsconfig.json for Path Aliases
DESCRIPTION: This configuration adds `baseUrl` and `paths` to `tsconfig.json`, allowing absolute imports from the `src` directory using the `@/` alias, which improves module resolution for TypeScript.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/installation/vite.mdx#_snippet_3

LANGUAGE: typescript
CODE:
```
{
  "files": [],
  "references": [
    {
      "path": "./tsconfig.app.json"
    },
    {
      "path": "./tsconfig.node.json"
    }
  ],
  "compilerOptions": {
    "baseUrl": ".",
    "paths": {
      "@/*": ["./src/*"]
    }
  }
}
```

----------------------------------------

TITLE: Adding Mode Toggle Component in React/TSX
DESCRIPTION: This React/TSX component provides a UI for users to manually switch between light, dark, and system themes. It utilizes React's `useState` and `useEffect` hooks to manage the current theme state and dynamically apply the 'dark' class to the document's root element. It depends on `lucide-react` for icons and `shadcn/ui` for Button and DropdownMenu components.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/dark-mode/astro.mdx#_snippet_1

LANGUAGE: TSX
CODE:
```
import * as React from "react"\nimport { Moon, Sun } from "lucide-react"\n\nimport { Button } from "@/components/ui/button"\nimport {\n  DropdownMenu,\n  DropdownMenuContent,\n  DropdownMenuItem,\n  DropdownMenuTrigger,\n} from "@/components/ui/dropdown-menu"\n\nexport function ModeToggle() {\n  const [theme, setThemeState] = React.useState<\n    "theme-light" | "dark" | "system"\n  >("theme-light")\n\n  React.useEffect(() => {\n    const isDarkMode = document.documentElement.classList.contains("dark")\n    setThemeState(isDarkMode ? "dark" : "theme-light")\n  }, [])\n\n  React.useEffect(() => {\n    const isDark =\n      theme === "dark" ||\n      (theme === "system" &&\n        window.matchMedia("(prefers-color-scheme: dark)").matches)\n    document.documentElement.classList[isDark ? "add" : "remove"]("dark")\n  }, [theme])\n\n  return (\n    <DropdownMenu>\n      <DropdownMenuTrigger asChild>\n        <Button variant="outline" size="icon">\n          <Sun className="h-[1.2rem] w-[1.2rem] rotate-0 scale-100 transition-all dark:-rotate-90 dark:scale-0" />\n          <Moon className="absolute h-[1.2rem] w-[1.2rem] rotate-90 scale-0 transition-all dark:rotate-0 dark:scale-100" />\n          <span className="sr-only">Toggle theme</span>\n        </Button>\n      </DropdownMenuTrigger>\n      <DropdownMenuContent align="end">\n        <DropdownMenuItem onClick={() => setThemeState("theme-light")}>\n          Light\n        </DropdownMenuItem>\n        <DropdownMenuItem onClick={() => setThemeState("dark")}>\n          Dark\n        </DropdownMenuItem>\n        <DropdownMenuItem onClick={() => setThemeState("system")}>\n          System\n        </DropdownMenuItem>\n      </DropdownMenuContent>\n    </DropdownMenu>\n  )\n}
```

----------------------------------------

TITLE: Implementing Single Date Picker with Shadcn UI in React (TSX)
DESCRIPTION: This snippet demonstrates how to create a single date picker component using Shadcn UI's Popover and Calendar. It allows users to select a date, which is then formatted using 'date-fns' and displayed. Key dependencies include 'date-fns' for date formatting, 'lucide-react' for the calendar icon, and Shadcn UI's Button, Calendar, and Popover components.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/date-picker.mdx#_snippet_0

LANGUAGE: tsx
CODE:
```
"use client"

import * as React from "react"
import { format } from "date-fns"
import { Calendar as CalendarIcon } from "lucide-react"

import { cn } from "@/lib/utils"
import { Button } from "@/components/ui/button"
import { Calendar } from "@/components/ui/calendar"
import {
  Popover,
  PopoverContent,
  PopoverTrigger,
} from "@/components/ui/popover"

export function DatePickerDemo() {
  const [date, setDate] = React.useState<Date>()

  return (
    <Popover>
      <PopoverTrigger asChild>
        <Button
          variant={"outline"}
          className={cn(
            "w-[280px] justify-start text-left font-normal",
            !date && "text-muted-foreground"
          )}
        >
          <CalendarIcon className="mr-2 h-4 w-4" />
          {date ? format(date, "PPP") : <span>Pick a date</span>}
        </Button>
      </PopoverTrigger>
      <PopoverContent className="w-auto p-0">
        <Calendar
          mode="single"
          selected={date}
          onSelect={setDate}
          initialFocus
        />
      </PopoverContent>
    </Popover>
  )
}
```

----------------------------------------

TITLE: Implementing Column Visibility Toggle in TSX
DESCRIPTION: This snippet shows how to include a `DataTableViewOptions` component, which allows users to easily toggle the visibility of different columns in the data table. This feature improves the user experience by enabling customization of the table's displayed information.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/data-table.mdx#_snippet_19

LANGUAGE: TSX
CODE:
```
<DataTableViewOptions table={table} />
```

----------------------------------------

TITLE: Integrating Dialog with Context Menu in TypeScript/React
DESCRIPTION: This snippet demonstrates how to properly integrate a Dialog component within a Context Menu or Dropdown Menu. The Dialog component must encase the menu component, and `DialogTrigger` should be used with `asChild` on a `ContextMenuItem` to activate the dialog from within the menu.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/dialog.mdx#_snippet_4

LANGUAGE: tsx
CODE:
```
<Dialog>
  <ContextMenu>
    <ContextMenuTrigger>Right click</ContextMenuTrigger>
    <ContextMenuContent>
      <ContextMenuItem>Open</ContextMenuItem>
      <ContextMenuItem>Download</ContextMenuItem>
      <DialogTrigger asChild>
        <ContextMenuItem>
          <span>Delete</span>
        </ContextMenuItem>
      </DialogTrigger>
    </ContextMenuContent>
  </ContextMenu>
  <DialogContent>
    <DialogHeader>
      <DialogTitle>Are you absolutely sure?</DialogTitle>
      <DialogDescription>
        This action cannot be undone. Are you sure you want to permanently
        delete this file from our servers?
      </DialogDescription>
    </DialogHeader>
    <DialogFooter>
      <Button type="submit">Confirm</Button>
    </DialogFooter>
  </DialogContent>
</Dialog>
```

----------------------------------------

TITLE: Adding Pagination Controls to DataTable in TSX
DESCRIPTION: This snippet demonstrates the integration of a `DataTablePagination` component into a table. This component provides controls for navigating through pages, adjusting page size, and displaying the selection count, enhancing the usability of large datasets.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/data-table.mdx#_snippet_18

LANGUAGE: TSX
CODE:
```
<DataTablePagination table={table} />
```

----------------------------------------

TITLE: Setting Responsive Carousel Item Sizes
DESCRIPTION: Shows how to apply responsive sizing to carousel items using Tailwind CSS classes, setting items to 50% width on medium screens and 33% on larger screens for adaptive layouts.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/carousel.mdx#_snippet_5

LANGUAGE: tsx
CODE:
```
// 50% on small screens and 33% on larger screens.
<Carousel>
  <CarouselContent>
    <CarouselItem className="md:basis-1/2 lg:basis-1/3">...</CarouselItem>
    <CarouselItem className="md:basis-1/2 lg:basis-1/3">...</CarouselItem>
    <CarouselItem className="md:basis-1/2 lg:basis-1/3">...</CarouselItem>
  </CarouselContent>
</Carousel>
```

----------------------------------------

TITLE: Defining Payment Data Structure and Sample Data (TypeScript)
DESCRIPTION: This TypeScript snippet defines the `Payment` type, outlining the expected structure for payment records, including `id`, `amount`, `status`, and `email`. It also provides a sample `payments` array to illustrate the data format that the table will consume.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/data-table.mdx#_snippet_2

LANGUAGE: typescript
CODE:
```
type Payment = {
  id: string
  amount: number
  status: "pending" | "processing" | "success" | "failed"
  email: string
}

export const payments: Payment[] = [
  {
    id: "728ed52f",
    amount: 100,
    status: "pending",
    email: "m@example.com",
  },
  {
    id: "489e1d42",
    amount: 125,
    status: "processing",
    email: "example@gmail.com",
  },
  // ...
]
```

----------------------------------------

TITLE: Importing and Using shadcn/ui Button in React Router
DESCRIPTION: This TypeScript JSX snippet demonstrates how to import the `Button` component from `shadcn/ui` and use it within a React Router `Home` component. It also includes the `meta` function for defining route-specific metadata like title and description.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/installation/react-router.mdx#_snippet_3

LANGUAGE: tsx
CODE:
```
import { Button } from "~/components/ui/button"

import type { Route } from "./+types/home"

export function meta({}: Route.MetaArgs) {
  return [
    { title: "New React Router App" },
    { name: "description", content: "Welcome to React Router!" }
  ]
}

export default function Home() {
  return (
    <div className="flex flex-col items-center justify-center min-h-svh">
      <Button>Click me</Button>
    </div>
  )
}
```

----------------------------------------

TITLE: Adding a shadcn/ui component to Gatsby
DESCRIPTION: Demonstrates how to add a specific `shadcn/ui` component, like `Button`, to the project using the `shadcn` CLI, making it available for use.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/installation/gatsby.mdx#_snippet_6

LANGUAGE: bash
CODE:
```
npx shadcn@latest add button
```

----------------------------------------

TITLE: Adding shadcn/ui Button Component (Bash)
DESCRIPTION: This command adds the specified shadcn/ui component, in this case, the 'Button', to your project. It fetches the component's code and integrates it into your project's component directory, making it available for import and use.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/installation/next.mdx#_snippet_1

LANGUAGE: bash
CODE:
```
npx shadcn@latest add button
```

----------------------------------------

TITLE: Creating a new React project with Vite
DESCRIPTION: This command initializes a new React project using Vite. It prompts the user to select a template, with 'React + TypeScript' being the recommended choice for this guide.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/installation/vite.mdx#_snippet_0

LANGUAGE: bash
CODE:
```
npm create vite@latest
```

----------------------------------------

TITLE: Enabling Pagination in TanStack React Table (TSX)
DESCRIPTION: This snippet modifies the `useReactTable` hook configuration to enable automatic pagination. It includes `getCoreRowModel` and `getPaginationRowModel` to handle row and pagination logic, respectively. By default, this sets up pages of 10 rows.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/data-table.mdx#_snippet_8

LANGUAGE: tsx
CODE:
```
import {
  ColumnDef,
  flexRender,
  getCoreRowModel,
  getPaginationRowModel,
  useReactTable,
} from "@tanstack/react-table"

export function DataTable<TData, TValue>({
  columns,
  data,
}: DataTableProps<TData, TValue>) {
  const table = useReactTable({
    data,
    columns,
    getCoreRowModel: getCoreRowModel(),
    getPaginationRowModel: getPaginationRowModel(),
  })

  // ...
}
```

----------------------------------------

TITLE: Configuring shadcn/ui Project with components.json
DESCRIPTION: This JSON snippet defines the configuration for a shadcn/ui project, specifying styling, Tailwind CSS paths, base colors, and component/utility aliases. Users should update the values for `tailwind.css` and `aliases` to match their specific project structure.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/changelog.mdx#_snippet_22

LANGUAGE: json
CODE:
```
{
  "style": "default",
  "rsc": true,
  "tailwind": {
    "config": "tailwind.config.js",
    "css": "app/globals.css",
    "baseColor": "slate",
    "cssVariables": true
  },
  "aliases": {
    "components": "@/components",
    "utils": "@/lib/utils"
  }
}
```

----------------------------------------

TITLE: Initializing DataTable with Sorting, Filtering, and Selection in TSX
DESCRIPTION: This snippet defines the `DataTable` component, integrating `useReactTable` to manage state for sorting, column filters, visibility, and row selection. It sets up the core table model, pagination, and filtered row models, providing a foundation for interactive data tables.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/data-table.mdx#_snippet_15

LANGUAGE: TSX
CODE:
```
export function DataTable<TData, TValue>({
  columns,
  data,
}: DataTableProps<TData, TValue>) {
  const [sorting, setSorting] = React.useState<SortingState>([])
  const [columnFilters, setColumnFilters] = React.useState<ColumnFiltersState>(
    []
  )
  const [columnVisibility, setColumnVisibility] =
    React.useState<VisibilityState>({})
  const [rowSelection, setRowSelection] = React.useState({})

  const table = useReactTable({
    data,
    columns,
    onSortingChange: setSorting,
    onColumnFiltersChange: setColumnFilters,
    getCoreRowModel: getCoreRowModel(),
    getPaginationRowModel: getPaginationRowModel(),
    getSortedRowModel: getSortedRowModel(),
    getFilteredRowModel: getFilteredRowModel(),
    onColumnVisibilityChange: setColumnVisibility,
    onRowSelectionChange: setRowSelection,
    state: {
      sorting,
      columnFilters,
      columnVisibility,
      rowSelection,
    },
  })

  return (
    <div>
      <div className="rounded-md border">
        <Table />
      </div>
    </div>
  )
}
```

----------------------------------------

TITLE: Updating DataTable Component for Column Visibility
DESCRIPTION: This snippet updates the `DataTable` component to include state management for sorting, column filters, and column visibility using `@tanstack/react-table`. It adds an input for filtering by email and a dropdown menu to dynamically toggle the visibility of table columns, enhancing user control over the displayed data.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/data-table.mdx#_snippet_13

LANGUAGE: tsx
CODE:
```
"use client"

import * as React from "react"
import {
  ColumnDef,
  ColumnFiltersState,
  SortingState,
  VisibilityState,
  flexRender,
  getCoreRowModel,
  getFilteredRowModel,
  getPaginationRowModel,
  getSortedRowModel,
  useReactTable,
} from "@tanstack/react-table"

import { Button } from "@/components/ui/button"
import {
  DropdownMenu,
  DropdownMenuCheckboxItem,
  DropdownMenuContent,
  DropdownMenuTrigger,
} from "@/components/ui/dropdown-menu"

export function DataTable<TData, TValue>({
  columns,
  data,
}: DataTableProps<TData, TValue>) {
  const [sorting, setSorting] = React.useState<SortingState>([])
  const [columnFilters, setColumnFilters] = React.useState<ColumnFiltersState>(
    []
  )
  const [columnVisibility, setColumnVisibility] =
    React.useState<VisibilityState>({})

  const table = useReactTable({
    data,
    columns,
    onSortingChange: setSorting,
    onColumnFiltersChange: setColumnFilters,
    getCoreRowModel: getCoreRowModel(),
    getPaginationRowModel: getPaginationRowModel(),
    getSortedRowModel: getSortedRowModel(),
    getFilteredRowModel: getFilteredRowModel(),
    onColumnVisibilityChange: setColumnVisibility,
    state: {
      sorting,
      columnFilters,
      columnVisibility,
    },
  })

  return (
    <div>
      <div className="flex items-center py-4">
        <Input
          placeholder="Filter emails..."
          value={table.getColumn("email")?.getFilterValue() as string}
          onChange={(event) =>
            table.getColumn("email")?.setFilterValue(event.target.value)
          }
          className="max-w-sm"
        />
        <DropdownMenu>
          <DropdownMenuTrigger asChild>
            <Button variant="outline" className="ml-auto">
              Columns
            </Button>
          </DropdownMenuTrigger>
          <DropdownMenuContent align="end">
            {table
              .getAllColumns()
              .filter(
                (column) => column.getCanHide()
              )
              .map((column) => {
                return (
                  <DropdownMenuCheckboxItem
                    key={column.id}
                    className="capitalize"
                    checked={column.getIsVisible()}
                    onCheckedChange={(value) =>
                      column.toggleVisibility(!!value)
                    }
                  >
                    {column.id}
                  </DropdownMenuCheckboxItem>
                )
              })}
          </DropdownMenuContent>
        </DropdownMenu>
      </div>
      <div className="rounded-md border">
        <Table>{ ... }</Table>
      </div>
    </div>
  )
}
```

----------------------------------------

TITLE: Basic Card Component Usage (TypeScript/React)
DESCRIPTION: This snippet illustrates the basic structure and usage of the Shadcn UI Card component. It demonstrates how to compose a card using `Card`, `CardHeader`, `CardTitle`, `CardDescription`, `CardContent`, and `CardFooter` to display structured content.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/card.mdx#_snippet_2

LANGUAGE: tsx
CODE:
```
<Card>
  <CardHeader>
    <CardTitle>Card Title</CardTitle>
    <CardDescription>Card Description</CardDescription>
  </CardHeader>
  <CardContent>
    <p>Card Content</p>
  </CardContent>
  <CardFooter>
    <p>Card Footer</p>
  </CardFooter>
</Card>
```

----------------------------------------

TITLE: Adding Components with shadcn/ui CLI
DESCRIPTION: This command is used to add UI components to a shadcn/ui project. The CLI automatically resolves component dependencies, formats them according to the project's custom configuration, and integrates them into the specified directory structure.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/changelog.mdx#_snippet_9

LANGUAGE: bash
CODE:
```
npx shadcn@latest add
```

----------------------------------------

TITLE: Configuring shadcn-ui Components with components.json
DESCRIPTION: This `components.json` file configures the shadcn-ui project, specifying styling, Tailwind CSS integration, and import aliases. It also includes the `tsx` flag to opt-out of TypeScript, providing flexibility for JavaScript-only projects.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/installation/index.mdx#_snippet_1

LANGUAGE: JSON
CODE:
```
{
  "style": "default",
  "tailwind": {
    "config": "tailwind.config.js",
    "css": "src/app/globals.css",
    "baseColor": "zinc",
    "cssVariables": true
  },
  "rsc": false,
  "tsx": false,
  "aliases": {
    "utils": "~/lib/utils",
    "components": "~/components"
  }
}
```

----------------------------------------

TITLE: Adding Pagination Controls to TanStack React Table (TSX)
DESCRIPTION: This snippet adds UI controls for pagination to the `DataTable` component. It uses Shadcn UI `Button` components to navigate between pages via `table.previousPage()` and `table.nextPage()` methods, with buttons disabled when no previous or next page is available.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/data-table.mdx#_snippet_9

LANGUAGE: tsx
CODE:
```
import { Button } from "@/components/ui/button"

export function DataTable<TData, TValue>({
  columns,
  data,
}: DataTableProps<TData, TValue>) {
  const table = useReactTable({
    data,
    columns,
    getCoreRowModel: getCoreRowModel(),
    getPaginationRowModel: getPaginationRowModel(),
  })

  return (
    <div>
      <div className="rounded-md border">
        <Table>
          { // .... }
        </Table>
      </div>
      <div className="flex items-center justify-end space-x-2 py-4">
        <Button
          variant="outline"
          size="sm"
          onClick={() => table.previousPage()}
          disabled={!table.getCanPreviousPage()}
        >
          Previous
        </Button>
        <Button
          variant="outline"
          size="sm"
          onClick={() => table.nextPage()}
          disabled={!table.getCanNextPage()}
        >
          Next
        </Button>
      </div>
    </div>
  )
}
```

----------------------------------------

TITLE: Adding Sorting to Email Column Header in React Table (TypeScript)
DESCRIPTION: This snippet updates the `email` column definition to make its header sortable. It uses a `Button` component from Shadcn UI and an `ArrowUpDown` icon from `lucide-react` to provide a visual indicator and toggle sorting state (ascending/descending) for the column.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/data-table.mdx#_snippet_11

LANGUAGE: TypeScript
CODE:
```
"use client"

import { ColumnDef } from "@tanstack/react-table"
import { ArrowUpDown } from "lucide-react"

export const columns: ColumnDef<Payment>[] = [
  {
    accessorKey: "email",
    header: ({ column }) => {
      return (
        <Button
          variant="ghost"
          onClick={() => column.toggleSorting(column.getIsSorted() === "asc")}
        >
          Email
          <ArrowUpDown className="ml-2 h-4 w-4" />
        </Button>
      )
    },
  },
]
```

----------------------------------------

TITLE: Adding Components with shadcn CLI (Bash)
DESCRIPTION: This command is used to add specific components and their required dependencies to an existing shadcn project. Users can specify the component name to integrate it into their application.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/cli.mdx#_snippet_2

LANGUAGE: bash
CODE:
```
npx shadcn@latest add [component]
```

----------------------------------------

TITLE: Defining Chart Data and Configuration for Custom Legend Names (TSX)
DESCRIPTION: This snippet defines `chartData` and `chartConfig` objects. `chartData` contains browser-specific visitor statistics and fill colors, while `chartConfig` maps browser keys to human-readable labels and colors. These structures are used to provide data and configuration for chart components, enabling custom legend names based on the 'browser' key.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/chart.mdx#_snippet_30

LANGUAGE: TSX
CODE:
```
const chartData = [
  { browser: "chrome", visitors: 187, fill: "var(--color-chrome)" },
  { browser: "safari", visitors: 200, fill: "var(--color-safari)" }
]

const chartConfig = {
  chrome: {
    label: "Chrome",
    color: "hsl(var(--chart-1))"
  },
  safari: {
    label: "Safari",
    color: "hsl(var(--chart-2))"
  }
} satisfies ChartConfig
```

----------------------------------------

TITLE: Configuring Sortable and Hideable Column Header in TSX
DESCRIPTION: This snippet shows how to define a column in a React Table setup, specifically for the 'email' accessorKey. It integrates the `DataTableColumnHeader` component to make the column header sortable and hideable, enhancing user interaction with the table columns.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/data-table.mdx#_snippet_17

LANGUAGE: TSX
CODE:
```
export const columns = [
  {
    accessorKey: "email",
    header: ({ column }) => (
      <DataTableColumnHeader column={column} title="Email" />
    ),
  },
]
```

----------------------------------------

TITLE: Displaying a Basic Toast on Button Click (TypeScript/TSX)
DESCRIPTION: This React component demonstrates how to use the `useToast` hook to display a toast notification when a button is clicked. It shows how to pass `title` and `description` properties to the `toast` function.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/toast.mdx#_snippet_4

LANGUAGE: tsx
CODE:
```
export const ToastDemo = () => {
  const { toast } = useToast()

  return (
    <Button
      onClick={() => {
        toast({
          title: "Scheduled: Catch up",
          description: "Friday, February 10, 2023 at 5:57 PM",
        })
      }}
    >
      Show Toast
    </Button>
  )
}
```

----------------------------------------

TITLE: Configuring Base Color in `components.json` (JSON)
DESCRIPTION: This `components.json` snippet shows how to set the `baseColor` property within the `tailwind` configuration. This property determines the default color palette for your components, allowing you to choose from options like `gray`, `neutral`, `slate`, `stone`, or `zinc`.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/changelog.mdx#_snippet_19

LANGUAGE: json
CODE:
```
{
  "tailwind": {
    "config": "tailwind.config.js",
    "css": "app/globals.css",
    "baseColor": "zinc",
    "cssVariables": false
  }
}
```

----------------------------------------

TITLE: Migrating to Tailwind CSS `size-*` Utility
DESCRIPTION: This diff illustrates the migration from using separate `w-*` (width) and `h-*` (height) Tailwind CSS utility classes to the new `size-*` utility. The `size-*` utility, supported by `tailwind-merge`, provides a more concise way to define both width and height simultaneously.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/tailwind-v4.mdx#_snippet_5

LANGUAGE: Diff
CODE:
```
- w-4 h-4
+ size-4
```

----------------------------------------

TITLE: Configuring JavaScript Import Aliases with jsconfig.json
DESCRIPTION: The `jsconfig.json` file is used to define compiler options for JavaScript projects, specifically for configuring import aliases. The `paths` property maps custom module paths (e.g., `@/*`) to physical directories (e.g., `./*`), simplifying module imports.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/installation/index.mdx#_snippet_2

LANGUAGE: JSON
CODE:
```
{
  "compilerOptions": {
    "paths": {
      "@/*": ["./*"]
    }
  }
}
```

----------------------------------------

TITLE: Defining Primary Color CSS Variables
DESCRIPTION: This CSS snippet defines a pair of CSS variables, `--primary` and `--primary-foreground`, using the `oklch` color format. These variables establish a convention where `--primary` is intended for background colors and `--primary-foreground` for text colors, demonstrating a common pattern for accessible color pairings.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/theming.mdx#_snippet_4

LANGUAGE: css
CODE:
```
--primary: oklch(0.205 0 0);
--primary-foreground: oklch(0.985 0 0);
```

----------------------------------------

TITLE: Adding a shadcn/ui component to a project
DESCRIPTION: This command uses `npx` to execute the latest version of the shadcn/ui CLI, adding the 'Switch' component to the project. It automatically handles downloading and placing the component's source files, typically in `resources/js/components/ui/switch.tsx`.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/installation/laravel.mdx#_snippet_1

LANGUAGE: bash
CODE:
```
npx shadcn@latest add switch
```

----------------------------------------

TITLE: Adding shadcn/ui Button Component
DESCRIPTION: This command uses the `shadcn` CLI to add the `Button` component to the project, automatically generating the necessary component files and dependencies.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/installation/react-router.mdx#_snippet_2

LANGUAGE: bash
CODE:
```
npx shadcn@latest add button
```

----------------------------------------

TITLE: Creating a new Laravel project with React
DESCRIPTION: This command initializes a new Laravel project named 'my-app' and configures it to use React for frontend development, typically with Inertia.js. This is the foundational step for setting up the project environment.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/installation/laravel.mdx#_snippet_0

LANGUAGE: bash
CODE:
```
laravel new my-app --react
```

----------------------------------------

TITLE: Using React Suspense for Data Loading in Sidebar with TSX
DESCRIPTION: This component demonstrates how to integrate `React.Suspense` with the `Sidebar` to manage loading states for dynamic content. It uses `NavProjectsSkeleton` as a fallback while `NavProjects` (a server component) is fetching data, providing a smooth user experience.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/sidebar.mdx#_snippet_42

LANGUAGE: tsx
CODE:
```
function AppSidebar() {
  return (
    <Sidebar>
      <SidebarContent>
        <SidebarGroup>
          <SidebarGroupLabel>Projects</SidebarGroupLabel>
          <SidebarGroupContent>
            <React.Suspense fallback={<NavProjectsSkeleton />}>
              <NavProjects />
            </React.Suspense>
          </SidebarGroupContent>
        </SidebarGroup>
      </SidebarContent>
    </Sidebar>
  )
}
```

----------------------------------------

TITLE: Adding Actions Column to TanStack React Table (TSX)
DESCRIPTION: This snippet updates the `ColumnDef` for a TanStack React Table to include an 'actions' column. The cell renders a `DropdownMenu` component, allowing users to perform actions like copying a payment ID or viewing details. It demonstrates accessing row data via `row.original`.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/data-table.mdx#_snippet_7

LANGUAGE: tsx
CODE:
```
"use client"

import { ColumnDef } from "@tanstack/react-table"
import { MoreHorizontal } from "lucide-react"

import { Button } from "@/components/ui/button"
import {
  DropdownMenu,
  DropdownMenuContent,
  DropdownMenuItem,
  DropdownMenuLabel,
  DropdownMenuSeparator,
  DropdownMenuTrigger,
} from "@/components/ui/dropdown-menu"

export const columns: ColumnDef<Payment>[] = [
  // ...
  {
    id: "actions",
    cell: ({ row }) => {
      const payment = row.original

      return (
        <DropdownMenu>
          <DropdownMenuTrigger asChild>
            <Button variant="ghost" className="h-8 w-8 p-0">
              <span className="sr-only">Open menu</span>
              <MoreHorizontal className="h-4 w-4" />
            </Button>
          </DropdownMenuTrigger>
          <DropdownMenuContent align="end">
            <DropdownMenuLabel>Actions</DropdownMenuLabel>
            <DropdownMenuItem
              onClick={() => navigator.clipboard.writeText(payment.id)}
            >
              Copy payment ID
            </DropdownMenuItem>
            <DropdownMenuSeparator />
            <DropdownMenuItem>View customer</DropdownMenuItem>
            <DropdownMenuItem>View payment details</DropdownMenuItem>
          </DropdownMenuContent>
        </DropdownMenu>
      )
    },
  },
  // ...
]
```

----------------------------------------

TITLE: Basic Alert Dialog Component Usage in TSX
DESCRIPTION: This example shows the fundamental structure of an Alert Dialog, including the trigger, content, header with title and description, and footer with cancel and action buttons, providing a complete interactive dialog.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/alert-dialog.mdx#_snippet_3

LANGUAGE: tsx
CODE:
```
<AlertDialog>
  <AlertDialogTrigger>Open</AlertDialogTrigger>
  <AlertDialogContent>
    <AlertDialogHeader>
      <AlertDialogTitle>Are you absolutely sure?</AlertDialogTitle>
      <AlertDialogDescription>
        This action cannot be undone. This will permanently delete your account
        and remove your data from our servers.
      </AlertDialogDescription>
    </AlertDialogHeader>
    <AlertDialogFooter>
      <AlertDialogCancel>Cancel</AlertDialogCancel>
      <AlertDialogAction>Continue</AlertDialogAction>
    </AlertDialogFooter>
  </AlertDialogContent>
</AlertDialog>
```

----------------------------------------

TITLE: Adding shadcn/ui Button Component (Bash)
DESCRIPTION: This command uses the shadcn/ui CLI to add the 'Button' component to the current project. It fetches the component's code and integrates it, making it available for use in the application.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/installation/tanstack-router.mdx#_snippet_1

LANGUAGE: bash
CODE:
```
npx shadcn@canary add button
```

----------------------------------------

TITLE: Installing Checkbox Component via CLI (Bash)
DESCRIPTION: This snippet demonstrates how to install the Checkbox component using the shadcn/ui CLI, which automates the setup process by adding the component's code and dependencies to your project.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/checkbox.mdx#_snippet_0

LANGUAGE: bash
CODE:
```
npx shadcn@latest add checkbox
```

----------------------------------------

TITLE: Basic Menubar Component Structure in TSX
DESCRIPTION: This code block illustrates the fundamental JSX structure for rendering a Menubar component. It showcases how to define a menu with a trigger, content, items, separators, and shortcuts, providing a functional example of the component's usage.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/menubar.mdx#_snippet_3

LANGUAGE: tsx
CODE:
```
<Menubar>
  <MenubarMenu>
    <MenubarTrigger>File</MenubarTrigger>
    <MenubarContent>
      <MenubarItem>
        New Tab <MenubarShortcut>⌘T</MenubarShortcut>
      </MenubarItem>
      <MenubarItem>New Window</MenubarItem>
      <MenubarSeparator />
      <MenubarItem>Share</MenubarItem>
      <MenubarSeparator />
      <MenubarItem>Print</MenubarItem>
    </MenubarContent>
  </MenubarMenu>
</Menubar>
```

----------------------------------------

TITLE: Adding Toaster Component to Root Layout (TypeScript/TSX)
DESCRIPTION: This TypeScript/TSX snippet demonstrates how to import and include the `Toaster` component in your application's root layout. Placing it here ensures that all toasts can be displayed globally across your application.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/toast.mdx#_snippet_1

LANGUAGE: tsx
CODE:
```
import { Toaster } from "@/components/ui/toaster"

export default function RootLayout({ children }) {
  return (
    <html lang="en">
      <head />
      <body>
        <main>{children}</main>
        <Toaster />
      </body>
    </html>
  )
}
```

----------------------------------------

TITLE: Theming Shadcn UI Sidebar with CSS Variables
DESCRIPTION: This CSS snippet defines custom CSS variables for theming the Shadcn UI sidebar, providing distinct color palettes for both light (`:root`) and dark (`.dark`) modes. These variables control background, foreground, primary, accent, border, and ring colors, allowing for independent styling from the main application.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/sidebar.mdx#_snippet_46

LANGUAGE: css
CODE:
```
@layer base {
  :root {
    --sidebar-background: 0 0% 98%;
    --sidebar-foreground: 240 5.3% 26.1%;
    --sidebar-primary: 240 5.9% 10%;
    --sidebar-primary-foreground: 0 0% 98%;
    --sidebar-accent: 240 4.8% 95.9%;
    --sidebar-accent-foreground: 240 5.9% 10%;
    --sidebar-border: 220 13% 91%;
    --sidebar-ring: 217.2 91.2% 59.8%;
  }

  .dark {
    --sidebar-background: 240 5.9% 10%;
    --sidebar-foreground: 240 4.8% 95.9%;
    --sidebar-primary: 0 0% 98%;
    --sidebar-primary-foreground: 240 5.9% 10%;
    --sidebar-accent: 240 3.7% 15.9%;
    --sidebar-accent-foreground: 240 4.8% 95.9%;
    --sidebar-border: 240 3.7% 15.9%;
    --sidebar-ring: 217.2 91.2% 59.8%;
  }
}
```

----------------------------------------

TITLE: Installing Badge Component via CLI (Bash)
DESCRIPTION: This command installs the `Badge` component using the `shadcn/ui` CLI. It automatically adds the necessary files and configurations to your project, simplifying the setup process.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/badge.mdx#_snippet_0

LANGUAGE: bash
CODE:
```
npx shadcn@latest add badge
```

----------------------------------------

TITLE: Applying Primary Color Convention in TSX
DESCRIPTION: This TSX snippet demonstrates the application of the `--primary` and `--primary-foreground` CSS variables using Tailwind CSS classes (`bg-primary`, `text-primary-foreground`). It shows how the defined color convention is translated into practical usage within a component, ensuring consistent primary color application for backgrounds and text.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/theming.mdx#_snippet_5

LANGUAGE: tsx
CODE:
```
<div className="bg-primary text-primary-foreground">Hello</div>
```

----------------------------------------

TITLE: Initializing a Git Repository (Shell)
DESCRIPTION: This command initializes a new Git repository in the current directory. It's a standard first step before connecting a local project to a remote version control system like GitHub.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/packages/shadcn/test/fixtures/frameworks/remix-indie-stack/README.md#_snippet_6

LANGUAGE: sh
CODE:
```
git init
```

----------------------------------------

TITLE: Listing Components with Available Updates (Bash)
DESCRIPTION: Executing `npx shadcn diff` without arguments provides a list of all components in the project that have available updates from the upstream repository. This helps users quickly identify which parts of their project might need attention.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/changelog.mdx#_snippet_11

LANGUAGE: bash
CODE:
```
npx shadcn diff
```

----------------------------------------

TITLE: Creating Submenus with SidebarMenuSub (TSX)
DESCRIPTION: This snippet illustrates how to render a submenu within a `SidebarMenu` using the `SidebarMenuSub` component. Submenu items are defined using `<SidebarMenuSubItem />` and `<SidebarMenuSubButton />`.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/sidebar.mdx#_snippet_32

LANGUAGE: tsx
CODE:
```
<SidebarMenuItem>
  <SidebarMenuButton />
  <SidebarMenuSub>
    <SidebarMenuSubItem>
      <SidebarMenuSubButton />
    </SidebarMenuSubItem>
    <SidebarMenuSubItem>
      <SidebarMenuSubButton />
    </SidebarMenuSubItem>
  </SidebarMenuSub>
</SidebarMenuItem>
```

----------------------------------------

TITLE: Rendering Basic Outline Button in TSX
DESCRIPTION: This TSX snippet demonstrates how to render a `Button` component with an 'outline' variant, displaying 'Button' as its text content.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/button.mdx#_snippet_3

LANGUAGE: tsx
CODE:
```
<Button variant="outline">Button</Button>
```

----------------------------------------

TITLE: Installing Dialog Component via shadcn/ui CLI
DESCRIPTION: This command uses the shadcn/ui CLI to automatically add the Dialog component and its dependencies to your project, streamlining the setup process. It's the recommended method for quick integration.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/dialog.mdx#_snippet_0

LANGUAGE: bash
CODE:
```
npx shadcn@latest add dialog
```

----------------------------------------

TITLE: Basic Tabs Component Usage in TSX
DESCRIPTION: This example illustrates the fundamental structure for implementing the Shadcn UI Tabs component, including setting a default active tab, defining tab triggers, and rendering associated content panels.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/tabs.mdx#_snippet_3

LANGUAGE: tsx
CODE:
```
<Tabs defaultValue="account" className="w-[400px]">
  <TabsList>
    <TabsTrigger value="account">Account</TabsTrigger>
    <TabsTrigger value="password">Password</TabsTrigger>
  </TabsList>
  <TabsContent value="account">Make changes to your account here.</TabsContent>
  <TabsContent value="password">Change your password here.</TabsContent>
</Tabs>
```

----------------------------------------

TITLE: Basic Usage of Pagination Component in TSX
DESCRIPTION: This example illustrates the fundamental structure for implementing the shadcn-ui Pagination component. It demonstrates how to combine Pagination, PaginationContent, PaginationItem, PaginationPrevious, PaginationLink, PaginationEllipsis, and PaginationNext to create a functional pagination UI.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/pagination.mdx#_snippet_2

LANGUAGE: tsx
CODE:
```
<Pagination>
  <PaginationContent>
    <PaginationItem>
      <PaginationPrevious href="#" />
    </PaginationItem>
    <PaginationItem>
      <PaginationLink href="#">1</PaginationLink>
    </PaginationItem>
    <PaginationItem>
      <PaginationEllipsis />
    </PaginationItem>
    <PaginationItem>
      <PaginationNext href="#" />
    </PaginationItem>
  </PaginationContent>
</Pagination>
```

----------------------------------------

TITLE: Updating Sidebar `setOpen` Callback with Cookie Handling in React
DESCRIPTION: This TypeScript React snippet demonstrates an improved `setOpen` callback function for `SidebarProvider`. It handles both boolean values and functional updates, conditionally calls an external `setOpenProp`, and persists the sidebar's open state to a cookie for maintaining state across sessions.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/sidebar.mdx#_snippet_49

LANGUAGE: tsx
CODE:
```
const setOpen = React.useCallback(
  (value: boolean | ((value: boolean) => boolean)) => {
    const openState = typeof value === "function" ? value(open) : value
    if (setOpenProp) {
      setOpenProp(openState)
    } else {
      _setOpen(openState)
    }

    // This sets the cookie to keep the sidebar state.
    document.cookie = `${SIDEBAR_COOKIE_NAME}=${openState}; path=/; max-age=${SIDEBAR_COOKIE_MAX_AGE}`
  },
  [setOpenProp, open]
)
```

----------------------------------------

TITLE: Comprehensive List of Theming CSS Variables
DESCRIPTION: This extensive CSS snippet provides a complete list of all available CSS variables for theming, defined within the `:root` selector for light mode and the `.dark` selector for dark mode. It includes variables for various UI elements like background, foreground, card, popover, primary, secondary, muted, accent, destructive, border, input, ring, chart colors, and sidebar elements, offering granular control over the application's visual theme.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/theming.mdx#_snippet_6

LANGUAGE: css
CODE:
```
:root {
  --radius: 0.625rem;
  --background: oklch(1 0 0);
  --foreground: oklch(0.145 0 0);
  --card: oklch(1 0 0);
  --card-foreground: oklch(0.145 0 0);
  --popover: oklch(1 0 0);
  --popover-foreground: oklch(0.145 0 0);
  --primary: oklch(0.205 0 0);
  --primary-foreground: oklch(0.985 0 0);
  --secondary: oklch(0.97 0 0);
  --secondary-foreground: oklch(0.205 0 0);
  --muted: oklch(0.97 0 0);
  --muted-foreground: oklch(0.556 0 0);
  --accent: oklch(0.97 0 0);
  --accent-foreground: oklch(0.205 0 0);
  --destructive: oklch(0.577 0.245 27.325);
  --border: oklch(0.922 0 0);
  --input: oklch(0.922 0 0);
  --ring: oklch(0.708 0 0);
  --chart-1: oklch(0.646 0.222 41.116);
  --chart-2: oklch(0.6 0.118 184.704);
  --chart-3: oklch(0.398 0.07 227.392);
  --chart-4: oklch(0.828 0.189 84.429);
  --chart-5: oklch(0.769 0.188 70.08);
  --sidebar: oklch(0.985 0 0);
  --sidebar-foreground: oklch(0.145 0 0);
  --sidebar-primary: oklch(0.205 0 0);
  --sidebar-primary-foreground: oklch(0.985 0 0);
  --sidebar-accent: oklch(0.97 0 0);
  --sidebar-accent-foreground: oklch(0.205 0 0);
  --sidebar-border: oklch(0.922 0 0);
  --sidebar-ring: oklch(0.708 0 0);
}

.dark {
  --background: oklch(0.145 0 0);
  --foreground: oklch(0.985 0 0);
  --card: oklch(0.205 0 0);
  --card-foreground: oklch(0.985 0 0);
  --popover: oklch(0.269 0 0);
  --popover-foreground: oklch(0.985 0 0);
  --primary: oklch(0.922 0 0);
  --primary-foreground: oklch(0.205 0 0);
  --secondary: oklch(0.269 0 0);
  --secondary-foreground: oklch(0.985 0 0);
  --muted: oklch(0.269 0 0);
  --muted-foreground: oklch(0.708 0 0);
  --accent: oklch(0.371 0 0);
  --accent-foreground: oklch(0.985 0 0);
  --destructive: oklch(0.704 0.191 22.216);
  --border: oklch(1 0 0 / 10%);
  --input: oklch(1 0 0 / 15%);
  --ring: oklch(0.556 0 0);
  --chart-1: oklch(0.488 0.243 264.376);
  --chart-2: oklch(0.696 0.17 162.48);
  --chart-3: oklch(0.769 0.188 70.08);
  --chart-4: oklch(0.627 0.265 303.9);
  --chart-5: oklch(0.645 0.246 16.439);
  --sidebar: oklch(0.205 0 0);
  --sidebar-foreground: oklch(0.985 0 0);
  --sidebar-primary: oklch(0.488 0.243 264.376);
  --sidebar-primary-foreground: oklch(0.985 0 0);
  --sidebar-accent: oklch(0.269 0 0);
  --sidebar-accent-foreground: oklch(0.985 0 0);
  --sidebar-border: oklch(1 0 0 / 10%);
  --sidebar-ring: oklch(0.439 0 0);
}
```

----------------------------------------

TITLE: Basic Carousel Usage
DESCRIPTION: Demonstrates the fundamental structure of the shadcn/ui Carousel component, including the main container, content wrapper, individual items, and navigation buttons for basic implementation.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/carousel.mdx#_snippet_3

LANGUAGE: tsx
CODE:
```
<Carousel>
  <CarouselContent>
    <CarouselItem>...</CarouselItem>
    <CarouselItem>...</CarouselItem>
    <CarouselItem>...</CarouselItem>
  </CarouselContent>
  <CarouselPrevious />
  <CarouselNext />
</Carousel>
```

----------------------------------------

TITLE: Integrating Toaster Component in Root Layout (TypeScript/React)
DESCRIPTION: This TypeScript/React snippet imports the `Toaster` component from `@/components/ui/sonner` and integrates it into the `RootLayout` of a Next.js application. Placing `Toaster` at the root ensures it's available globally for displaying toast notifications across the application.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/sonner.mdx#_snippet_1

LANGUAGE: tsx
CODE:
```
import { Toaster } from "@/components/ui/sonner"

export default function RootLayout({ children }) {
  return (
    <html lang="en">
      <head />
      <body>
        <main>{children}</main>
        <Toaster />
      </body>
    </html>
  )
}
```

----------------------------------------

TITLE: Specifying Tailwind CSS Import Path in components.json (JSON)
DESCRIPTION: This configuration points to the CSS file responsible for importing Tailwind CSS into your project. It helps the CLI understand where your main CSS entry point is located.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components-json.mdx#_snippet_4

LANGUAGE: json
CODE:
```
{
  "tailwind": {
    "css": "styles/global.css"
  }
}
```

----------------------------------------

TITLE: Using `nameKey` Prop for Custom Chart Legend Names (TSX)
DESCRIPTION: This JSX snippet demonstrates how to apply a custom key for legend names in a `ChartLegend` component. By setting `nameKey="browser"` on `ChartLegendContent`, the legend will display names based on the 'browser' property from the chart data, such as 'Chrome' and 'Safari', instead of default keys.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/chart.mdx#_snippet_31

LANGUAGE: TSX
CODE:
```
<ChartLegend content={<ChartLegendContent nameKey="browser" />} />
```

----------------------------------------

TITLE: Basic Dialog Component Usage in TypeScript/React
DESCRIPTION: This example illustrates the fundamental structure of a shadcn/ui Dialog. It includes a `DialogTrigger` to open the dialog, and `DialogContent` containing a `DialogHeader` with a `DialogTitle` and `DialogDescription` to present information to the user.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/dialog.mdx#_snippet_3

LANGUAGE: tsx
CODE:
```
<Dialog>
  <DialogTrigger>Open</DialogTrigger>
  <DialogContent>
    <DialogHeader>
      <DialogTitle>Are you absolutely sure?</DialogTitle>
      <DialogDescription>
        This action cannot be undone. This will permanently delete your account
        and remove your data from our servers.
      </DialogDescription>
    </DialogHeader>
  </DialogContent>
</Dialog>
```

----------------------------------------

TITLE: Basic Usage of Radio Group Component (TSX)
DESCRIPTION: This example demonstrates how to render a basic RadioGroup component with two RadioGroupItem options and associated Label components, setting a default selected value for initial state.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/radio-group.mdx#_snippet_3

LANGUAGE: tsx
CODE:
```
<RadioGroup defaultValue="option-one">
  <div className="flex items-center space-x-2">
    <RadioGroupItem value="option-one" id="option-one" />
    <Label htmlFor="option-one">Option One</Label>
  </div>
  <div className="flex items-center space-x-2">
    <RadioGroupItem value="option-two" id="option-two" />
    <Label htmlFor="option-two">Option Two</Label>
  </div>
</RadioGroup>
```

----------------------------------------

TITLE: Installing Tailwind CSS and Autoprefixer
DESCRIPTION: Installs the necessary development dependencies for Tailwind CSS and Autoprefixer using npm. These packages are crucial for processing and optimizing your CSS with Tailwind.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/installation/remix.mdx#_snippet_3

LANGUAGE: bash
CODE:
```
npm install -D tailwindcss@latest autoprefixer@latest
```

----------------------------------------

TITLE: Controlling Sidebar State in React/Next.js
DESCRIPTION: This TypeScript React snippet demonstrates how to create a controlled sidebar component using `useState` to manage its open/closed state. It passes the `open` state and `setOpen` function to `SidebarProvider` to enable external control over the sidebar's visibility.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/sidebar.mdx#_snippet_45

LANGUAGE: tsx
CODE:
```
export function AppSidebar() {
  const [open, setOpen] = React.useState(false)

  return (
    <SidebarProvider open={open} onOpenChange={setOpen}>
      <Sidebar />
    </SidebarProvider>
  )
}
```

----------------------------------------

TITLE: Importing Tailwind CSS in Remix root.tsx
DESCRIPTION: Imports the global `tailwind.css` file into the `app/root.tsx` file and adds it to the `links` function. This makes the Tailwind styles available globally throughout your Remix application.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/installation/remix.mdx#_snippet_6

LANGUAGE: js
CODE:
```
import styles from "./tailwind.css?url"

export const links: LinksFunction = () => [
  { rel: "stylesheet", href: styles },
  ...(cssBundleHref ? [{ rel: "stylesheet", href: cssBundleHref }] : []),
]
```

----------------------------------------

TITLE: Migrating Sheet Component 'position' Prop to 'side' in JSX
DESCRIPTION: This code snippet illustrates the migration of the `position` prop to the `side` prop for the `Sheet` component. The `side` prop now controls the placement of the sheet (e.g., 'right'), replacing the deprecated `position` prop.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/changelog.mdx#_snippet_25

LANGUAGE: diff
CODE:
```
- <Sheet position="right" />
+ <Sheet side="right" />
```

----------------------------------------

TITLE: Initializing shadcn/ui CLI
DESCRIPTION: Executes the `shadcn` initialization command to configure shadcn/ui within the current project. This step guides you through setting up the `components.json` file.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/installation/remix.mdx#_snippet_1

LANGUAGE: bash
CODE:
```
npx shadcn@latest init
```

----------------------------------------

TITLE: Importing Tailwind CSS in src/index.css
DESCRIPTION: This CSS snippet replaces the existing content in `src/index.css` to import Tailwind CSS, making its utility classes available throughout the application.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/installation/vite.mdx#_snippet_2

LANGUAGE: css
CODE:
```
@import "tailwindcss";
```

----------------------------------------

TITLE: Configuring shadcn/ui components.json
DESCRIPTION: Illustrates the interactive prompts from the `shadcn` init command for configuring `components.json`. Users select their preferred style, base color, and whether to use CSS variables for colors.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/installation/remix.mdx#_snippet_2

LANGUAGE: txt
CODE:
```
Which style would you like to use? › New York
Which color would you like to use as base color? › Zinc
Do you want to use CSS variables for colors? › no / yes
```

----------------------------------------

TITLE: Configuring Tailwind CSS Content for Shadcn Registry (TypeScript)
DESCRIPTION: This snippet illustrates how to configure the `tailwind.config.ts` file to ensure Tailwind CSS processes styles for components located within the `registry` directory. It updates the `content` array to include all JavaScript, TypeScript, JSX, and TSX files within `./registry/**`.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/registry/getting-started.mdx#_snippet_2

LANGUAGE: typescript
CODE:
```
// tailwind.config.ts
export default {
  content: ["./registry/**/*.{js,ts,jsx,tsx}"],
}
```

----------------------------------------

TITLE: Running `shadcn diff` Command for Updates (Bash)
DESCRIPTION: This command initiates the experimental `diff` feature in shadcn/ui, allowing users to check for and track upstream updates to their installed components. It's the primary way to identify what has changed in the upstream repository.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/changelog.mdx#_snippet_10

LANGUAGE: bash
CODE:
```
npx shadcn diff
```

----------------------------------------

TITLE: Integrating Toaster Component in Root Layout (TypeScript/React) - Manual Setup
DESCRIPTION: This TypeScript/React snippet, part of the manual installation process, imports the `Toaster` component and integrates it into the `RootLayout` of a Next.js application. This step ensures the toast component is globally available after manual dependency installation.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/sonner.mdx#_snippet_3

LANGUAGE: tsx
CODE:
```
import { Toaster } from "@/components/ui/sonner"

export default function RootLayout({ children }) {
  return (
    <html lang="en">
      <head />
      <body>
        <main>{children}</main>
        <Toaster />
      </body>
    </html>
  )
}
```

----------------------------------------

TITLE: Configuring Shadcn UI Components (JSON)
DESCRIPTION: This JSON configuration file, `components.json`, defines the settings for integrating Shadcn UI components into a project. It specifies the desired style, whether to use React Server Components (RSC), TypeScript (TSX) usage, Tailwind CSS configuration (including base color and CSS variables), and aliases for common project paths like components, utilities, and UI elements. It also sets the icon library to Lucide.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/installation/manual.mdx#_snippet_4

LANGUAGE: json
CODE:
```
{
  "$schema": "https://ui.shadcn.com/schema.json",
  "style": "new-york",
  "rsc": false,
  "tsx": true,
  "tailwind": {
    "config": "",
    "css": "src/styles/globals.css",
    "baseColor": "neutral",
    "cssVariables": true,
    "prefix": ""
  },
  "aliases": {
    "components": "@/components",
    "utils": "@/lib/utils",
    "ui": "@/components/ui",
    "lib": "@/lib",
    "hooks": "@/hooks"
  },
  "iconLibrary": "lucide"
}
```

----------------------------------------

TITLE: Defining Table Columns for Payments (TypeScript)
DESCRIPTION: This code defines the `columns` array using `ColumnDef` from `@tanstack/react-table`, specifying how `Payment` data fields like `status`, `email`, and `amount` should be displayed as table headers. It's a client component, indicated by `'use client'`, and forms the core structure of the table's visual representation.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/data-table.mdx#_snippet_3

LANGUAGE: typescript
CODE:
```
"use client"

import { ColumnDef } from "@tanstack/react-table"

// This type is used to define the shape of our data.
// You can use a Zod schema here if you want.
export type Payment = {
  id: string
  amount: number
  status: "pending" | "processing" | "success" | "failed"
  email: string
}

export const columns: ColumnDef<Payment>[] = [
  {
    accessorKey: "status",
    header: "Status",
  },
  {
    accessorKey: "email",
    header: "Email",
  },
  {
    accessorKey: "amount",
    header: "Amount",
  }
]
```

----------------------------------------

TITLE: Rendering Basic Input Component (TypeScript/React)
DESCRIPTION: This snippet shows the minimal JSX required to render the `Input` component, illustrating its basic usage within a TypeScript or React application.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/input.mdx#_snippet_2

LANGUAGE: tsx
CODE:
```
<Input />
```

----------------------------------------

TITLE: Rendering Basic Textarea Component (TSX)
DESCRIPTION: This snippet demonstrates the fundamental usage of the `Textarea` component by rendering it without any additional properties. It produces a standard HTML textarea element, styled according to the shadcn/ui library's design system.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/textarea.mdx#_snippet_2

LANGUAGE: tsx
CODE:
```
<Textarea />
```

----------------------------------------

TITLE: Implementing Basic Chart Legend
DESCRIPTION: This TypeScript/React snippet demonstrates the basic usage of the `ChartLegend` component. By setting its `content` prop to an instance of `ChartLegendContent`, a default legend is rendered, automatically referencing colors from the chart configuration.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/chart.mdx#_snippet_29

LANGUAGE: tsx
CODE:
```
<ChartLegend content={<ChartLegendContent />} />
```

----------------------------------------

TITLE: Configuring shadcn/ui components.json via CLI prompts
DESCRIPTION: Shows the interactive prompts for configuring `components.json` when initializing `shadcn/ui`, including choices for TypeScript, styling, CSS variables, and import aliases.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/installation/gatsby.mdx#_snippet_5

LANGUAGE: txt
CODE:
```
Would you like to use TypeScript (recommended)? no / yes
Which style would you like to use? › Default
Which color would you like to use as base color? › Slate
Where is your global CSS file? › › ./src/styles/globals.css
Do you want to use CSS variables for colors? › no / yes
Where is your tailwind.config.js located? › tailwind.config.js
Configure the import alias for components: › @/components
Configure the import alias for utils: › @/lib/utils
Are you using React Server Components? › no
```

----------------------------------------

TITLE: Displaying Image with Fixed Aspect Ratio (TSX)
DESCRIPTION: Demonstrates how to wrap an `Image` component within the `AspectRatio` component, setting its `ratio` prop to `16 / 9` to ensure the image maintains a consistent 16:9 aspect ratio within a given width.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/aspect-ratio.mdx#_snippet_3

LANGUAGE: tsx
CODE:
```
<div className="w-[450px]">
  <AspectRatio ratio={16 / 9}>
    <Image src="..." alt="Image" className="rounded-md object-cover" />
  </AspectRatio>
</div>
```

----------------------------------------

TITLE: Styling Sidebar Elements Based on Collapsible State in React
DESCRIPTION: This TypeScript React snippet demonstrates how to conditionally hide a `SidebarGroup` component when the `Sidebar` is in `icon` collapsible mode. It utilizes Tailwind CSS's `group-data-[collapsible=icon]:hidden` utility class to apply styling based on the parent `Sidebar`'s data attribute.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/sidebar.mdx#_snippet_47

LANGUAGE: tsx
CODE:
```
<Sidebar collapsible="icon">
  <SidebarContent>
    <SidebarGroup className="group-data-[collapsible=icon]:hidden" />
  </SidebarContent>
</Sidebar>
```

----------------------------------------

TITLE: Importing Toggle Component in TypeScript/React
DESCRIPTION: Imports the `Toggle` component from the local UI components path (`@/components/ui/toggle`). This statement makes the component available for use within any TypeScript or React file.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/toggle.mdx#_snippet_2

LANGUAGE: tsx
CODE:
```
import { Toggle } from "@/components/ui/toggle"
```

----------------------------------------

TITLE: Setting Multiple Sidebar Widths with CSS Variables in TSX
DESCRIPTION: Applies custom width settings to the sidebar using CSS variables (`--sidebar-width`, `--sidebar-width-mobile`) via the `style` prop of the `SidebarProvider`. This approach is suitable for applications with multiple sidebars, ensuring proper layout spacing.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/sidebar.mdx#_snippet_10

LANGUAGE: tsx
CODE:
```
<SidebarProvider
  style={{
    "--sidebar-width": "20rem",
    "--sidebar-width-mobile": "20rem"
  }}
>
  <Sidebar />
</SidebarProvider>
```

----------------------------------------

TITLE: Applying Badge Styles to Link Component (TSX)
DESCRIPTION: Applies badge styling to a `Link` component using the `badgeVariants` helper. This creates a link that visually appears as an `outline` badge, combining navigation functionality with the aesthetic of a badge.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/badge.mdx#_snippet_4

LANGUAGE: tsx
CODE:
```
<Link className={badgeVariants({ variant: "outline" })}>Badge</Link>
```

----------------------------------------

TITLE: Defining Zod Schema for Form Validation in TSX
DESCRIPTION: This snippet demonstrates how to define a Zod schema (`formSchema`) to specify the structure and validation rules for your form data. It shows how to define a string field with minimum and maximum length constraints, ensuring type safety and data integrity.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/form.mdx#_snippet_4

LANGUAGE: tsx
CODE:
```
"use client"

import { z } from "zod"

const formSchema = z.object({
  username: z.string().min(2).max(50)
})
```

----------------------------------------

TITLE: Importing and Using shadcn/ui Button Component in TanStack Start
DESCRIPTION: This TSX code snippet demonstrates how to import the `Button` component from the shadcn/ui library using the configured path alias. It then shows a basic usage of the `Button` component within a React functional component in a TanStack Start route.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/installation/tanstack.mdx#_snippet_7

LANGUAGE: tsx
CODE:
```
import { Button } from "@/components/ui/button"

function Home() {
  const router = useRouter()
  const state = Route.useLoaderData()

  return (
    <div>
      <Button>Click me</Button>
    </div>
  )
}
```

----------------------------------------

TITLE: Integrating SidebarProvider and Trigger in Next.js Layout (TSX)
DESCRIPTION: This snippet demonstrates how to integrate the `SidebarProvider` and `SidebarTrigger` components into a Next.js `app/layout.tsx` file. It wraps the main application content with `SidebarProvider` to enable sidebar context and places `SidebarTrigger` within the `main` element to control sidebar visibility.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/sidebar.mdx#_snippet_4

LANGUAGE: tsx
CODE:
```
import { SidebarProvider, SidebarTrigger } from "@/components/ui/sidebar"
import { AppSidebar } from "@/components/app-sidebar"

export default function Layout({ children }: { children: React.ReactNode }) {
  return (
    <SidebarProvider>
      <AppSidebar />
      <main>
        <SidebarTrigger />
        {children}
      </main>
    </SidebarProvider>
  )
}
```

----------------------------------------

TITLE: Configuring TypeScript paths in Astro's tsconfig.json
DESCRIPTION: Modifies the `tsconfig.json` file to add `baseUrl` and `paths` configurations. This allows for absolute imports using the `@/*` alias, resolving to the `src` directory, which simplifies module imports.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/installation/astro.mdx#_snippet_1

LANGUAGE: typescript
CODE:
```
{
  "compilerOptions": {
    // ...
    "baseUrl": ".",
    "paths": {
      "@/*": [
        "./src/*"
      ]
    }
    // ...
  }
}
```

----------------------------------------

TITLE: Configuring TypeScript Path Aliases
DESCRIPTION: This JSON snippet demonstrates how to configure path aliases in `tsconfig.json`, specifically setting `@/*` to resolve to the project root (`./*`). This alias simplifies module imports by allowing absolute paths from the project base, improving code readability and maintainability. It's crucial for projects using TypeScript.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/installation/manual.mdx#_snippet_1

LANGUAGE: json
CODE:
```
{
  "compilerOptions": {
    "baseUrl": ".",
    "paths": {
      "@/*": ["./*"]
    }
  }
}
```

----------------------------------------

TITLE: Configuring Import Aliases in jsconfig.json
DESCRIPTION: This JSON configuration for 'jsconfig.json' demonstrates how to set up import aliases for JavaScript projects. The 'paths' property maps '@/*' to './*', simplifying module imports by allowing absolute paths from the project root.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/changelog.mdx#_snippet_5

LANGUAGE: json
CODE:
```
{
  "compilerOptions": {
    "paths": {
      "@/*": ["./*"]
    }
  }
}
```

----------------------------------------

TITLE: Adding Row Selection to Column Definitions
DESCRIPTION: This snippet modifies the `columns.tsx` file to introduce a new column definition for row selection. It utilizes a `Checkbox` component for both the header (to select/deselect all rows on the current page) and individual cells (to select/deselect specific rows), enabling interactive row selection within the table.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/data-table.mdx#_snippet_14

LANGUAGE: tsx
CODE:
```
"use client"

import { ColumnDef } from "@tanstack/react-table"

import { Badge } from "@/components/ui/badge"
import { Checkbox } from "@/components/ui/checkbox"

export const columns: ColumnDef<Payment>[] = [
  {
    id: "select",
    header: ({ table }) => (
      <Checkbox
        checked={
          table.getIsAllPageRowsSelected() ||
          (table.getIsSomePageRowsSelected() && "indeterminate")
        }
        onCheckedChange={(value) => table.toggleAllPageRowsSelected(!!value)}
        aria-label="Select all"
      />
    ),
    cell: ({ row }) => (
      <Checkbox
        checked={row.getIsSelected()}
        onCheckedChange={(value) => row.toggleSelected(!!value)}
        aria-label="Select row"
      />
    ),
    enableSorting: false,
    enableHiding: false,
  },
]
```

----------------------------------------

TITLE: Integrating ThemeProvider into Remix Root Layout
DESCRIPTION: This TSX code integrates `ThemeProvider` from `remix-themes` into the root layout of a Remix application. It defines a loader to retrieve the current theme from the session and wraps the main `App` component with `ThemeProvider`, passing the `specifiedTheme` and a `themeAction` route for theme changes. The `App` component dynamically applies the theme class to the `html` element.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/dark-mode/remix.mdx#_snippet_3

LANGUAGE: tsx
CODE:
```
import clsx from "clsx"
import { PreventFlashOnWrongTheme, ThemeProvider, useTheme } from "remix-themes"

import { themeSessionResolver } from "./sessions.server"

// Return the theme from the session storage using the loader
export async function loader({ request }: LoaderFunctionArgs) {
  const { getTheme } = await themeSessionResolver(request)
  return {
    theme: getTheme(),
  }
}
// Wrap your app with ThemeProvider.
// `specifiedTheme` is the stored theme in the session storage.
// `themeAction` is the action name that's used to change the theme in the session storage.
export default function AppWithProviders() {
  const data = useLoaderData<typeof loader>()
  return (
    <ThemeProvider specifiedTheme={data.theme} themeAction="/action/set-theme">
      <App />
    </ThemeProvider>
  )
}

export function App() {
  const data = useLoaderData<typeof loader>()
  const [theme] = useTheme()
  return (
    <html lang="en" className={clsx(theme)}>
      <head>
        <meta charSet="utf-8" />
        <meta name="viewport" content="width=device-width, initial-scale=1" />
        <Meta />
        <PreventFlashOnWrongTheme ssrTheme={Boolean(data.theme)} />
        <Links />
      </head>
      <body>
        <Outlet />
        <ScrollRestoration />
        <Scripts />
        <LiveReload />
      </body>
    </html>
  )
}
```

----------------------------------------

TITLE: Rendering Basic Checkbox Component (TypeScript/React)
DESCRIPTION: This snippet demonstrates the basic JSX usage of the `Checkbox` component, rendering a default, unchecked checkbox without any additional props or configurations.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/checkbox.mdx#_snippet_3

LANGUAGE: tsx
CODE:
```
<Checkbox />
```

----------------------------------------

TITLE: Creating Link Styled as Button with buttonVariants in TSX
DESCRIPTION: This TSX snippet shows how to apply button styling to a `Link` component by using the `buttonVariants` helper, specifically setting an 'outline' variant.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/button.mdx#_snippet_5

LANGUAGE: tsx
CODE:
```
<Link className={buttonVariants({ variant: "outline" })}>Click here</Link>
```

----------------------------------------

TITLE: Adding Icon Styling to CommandItem in TypeScript/React
DESCRIPTION: This snippet shows how to extend the `CommandItem` component to automatically style SVG icons within it. By adding specific Tailwind CSS classes (`gap-2`, `[&_svg]:pointer-events-none`, `[&_svg]:size-4`, `[&_svg]:shrink-0`), it ensures consistent sizing and spacing for icons, improving the visual presentation of command items.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/command.mdx#_snippet_5

LANGUAGE: tsx
CODE:
```
const CommandItem = React.forwardRef<
  React.ElementRef<typeof CommandPrimitive.Item>,
  React.ComponentPropsWithoutRef<typeof CommandPrimitive.Item>
>(({ className, ...props }, ref) => (
  <CommandPrimitive.Item
    ref={ref}
    className={cn(
      "... gap-2 [&_svg]:pointer-events-none [&_svg]:size-4 [&_svg]:shrink-0",
      className
    )}
    {...props}
  />
))
```

----------------------------------------

TITLE: Basic Tooltip Usage Example (TypeScript/TSX)
DESCRIPTION: This example illustrates the basic structure for implementing a `Tooltip`. It wraps the `TooltipTrigger` (the element that activates the tooltip) and `TooltipContent` (the content displayed in the tooltip) within a `TooltipProvider` for context.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/tooltip.mdx#_snippet_3

LANGUAGE: tsx
CODE:
```
<TooltipProvider>
  <Tooltip>
    <TooltipTrigger>Hover</TooltipTrigger>
    <TooltipContent>
      <p>Add to library</p>
    </TooltipContent>
  </Tooltip>
</TooltipProvider>
```

----------------------------------------

TITLE: CLI Prompts for shadcn/ui Init Command
DESCRIPTION: This text snippet outlines the interactive questions presented by the shadcn/ui CLI's 'init' command. These prompts guide the user through configuring project aspects such as styling preferences, base colors, CSS file locations, Tailwind settings, import aliases, and React Server Components usage.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/changelog.mdx#_snippet_7

LANGUAGE: txt
CODE:
```
Which style would you like to use? › Default
Which color would you like to use as base color? › Slate
Where is your global CSS file? › › app/globals.css
Do you want to use CSS variables for colors? › no / yes
Where is your tailwind.config.js located? › tailwind.config.js
Configure the import alias for components: › @/components
Configure the import alias for utils: › @/lib/utils
Are you using React Server Components? › no / yes
```

----------------------------------------

TITLE: Implementing a Theme Mode Toggle in React (TSX)
DESCRIPTION: This snippet provides a `ModeToggle` component that allows users to switch between 'light', 'dark', and 'system' themes. It utilizes the `useTheme` hook to access the `setTheme` function from the `ThemeProvider` context and integrates UI components like `Button` and `DropdownMenu` for a user-friendly theme selection interface.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/dark-mode/vite.mdx#_snippet_2

LANGUAGE: tsx
CODE:
```
import { Moon, Sun } from "lucide-react"

import { Button } from "@/components/ui/button"
import {
  DropdownMenu,
  DropdownMenuContent,
  DropdownMenuItem,
  DropdownMenuTrigger,
} from "@/components/ui/dropdown-menu"
import { useTheme } from "@/components/theme-provider"

export function ModeToggle() {
  const { setTheme } = useTheme()

  return (
    <DropdownMenu>
      <DropdownMenuTrigger asChild>
        <Button variant="outline" size="icon">
          <Sun className="h-[1.2rem] w-[1.2rem] rotate-0 scale-100 transition-all dark:-rotate-90 dark:scale-0" />
          <Moon className="absolute h-[1.2rem] w-[1.2rem] rotate-90 scale-0 transition-all dark:rotate-0 dark:scale-100" />
          <span className="sr-only">Toggle theme</span>
        </Button>
      </DropdownMenuTrigger>
      <DropdownMenuContent align="end">
        <DropdownMenuItem onClick={() => setTheme("light")}>
          Light
        </DropdownMenuItem>
        <DropdownMenuItem onClick={() => setTheme("dark")}>
          Dark
        </DropdownMenuItem>
        <DropdownMenuItem onClick={() => setTheme("system")}>
          System
        </DropdownMenuItem>
      </DropdownMenuContent>
    </DropdownMenu>
  )
}
```

----------------------------------------

TITLE: Accessing Carousel API and Tracking Slide State in React
DESCRIPTION: This snippet demonstrates how to obtain the carousel API instance using the `setApi` prop and `React.useState`. It then uses `React.useEffect` to track the current slide and total slide count, updating the state on 'select' events. This allows for dynamic display of carousel progress.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/carousel.mdx#_snippet_10

LANGUAGE: tsx
CODE:
```
import { type CarouselApi } from "@/components/ui/carousel"

export function Example() {
  const [api, setApi] = React.useState<CarouselApi>()
  const [current, setCurrent] = React.useState(0)
  const [count, setCount] = React.useState(0)

  React.useEffect(() => {
    if (!api) {
      return
    }

    setCount(api.scrollSnapList().length)
    setCurrent(api.selectedScrollSnap() + 1)

    api.on("select", () => {
      setCurrent(api.selectedScrollSnap() + 1)
    })
  }, [api])

  return (
    <Carousel setApi={setApi}>
      <CarouselContent>
        <CarouselItem>...</CarouselItem>
        <CarouselItem>...</CarouselItem>
        <CarouselItem>...</CarouselItem>
      </CarouselContent>
    </Carousel>
  )
}
```

----------------------------------------

TITLE: Making a Sidebar Group Collapsible in TSX
DESCRIPTION: This example shows how to make a `SidebarGroup` collapsible by wrapping it with the `Collapsible` component. It uses `CollapsibleTrigger` within `SidebarGroupLabel` to control the collapsible state and `CollapsibleContent` to house the group's content, allowing users to expand or collapse sections.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/sidebar.mdx#_snippet_24

LANGUAGE: tsx
CODE:
```
export function AppSidebar() {
  return (
    <Collapsible defaultOpen className="group/collapsible">
      <SidebarGroup>
        <SidebarGroupLabel asChild>
          <CollapsibleTrigger>
            Help
            <ChevronDown className="ml-auto transition-transform group-data-[state=open]/collapsible:rotate-180" />
          </CollapsibleTrigger>
        </SidebarGroupLabel>
        <CollapsibleContent>
          <SidebarGroupContent />
        </CollapsibleContent>
      </SidebarGroup>
    </Collapsible>
  )
}
```

----------------------------------------

TITLE: Enabling React Server Components Support (JSON)
DESCRIPTION: This boolean setting enables or disables support for React Server Components. When set to `true`, the CLI automatically adds the `use client` directive to client components, optimizing for RSC environments.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components-json.mdx#_snippet_8

LANGUAGE: json
CODE:
```
{
  "rsc": `true` | `false`
}
```

----------------------------------------

TITLE: Basic Calendar Component Usage with State (TypeScript/React)
DESCRIPTION: This React functional component demonstrates how to initialize and use the `Calendar` component, managing the selected date state and applying a basic border style. It uses `useState` to control the `selected` date.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/calendar.mdx#_snippet_3

LANGUAGE: tsx
CODE:
```
const [date, setDate] = React.useState<Date | undefined>(new Date())

return (
  <Calendar
    mode="single"
    selected={date}
    onSelect={setDate}
    className="rounded-md border"
  />
)
```

----------------------------------------

TITLE: Rendering Sidebar Menu Skeleton with React Server Components in TSX
DESCRIPTION: This component provides a loading skeleton for the `SidebarMenu` when data is being fetched. It uses `Array.from` to render multiple `SidebarMenuItem` components, each containing a `SidebarMenuSkeleton` to indicate a loading state.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/sidebar.mdx#_snippet_40

LANGUAGE: tsx
CODE:
```
function NavProjectsSkeleton() {
  return (
    <SidebarMenu>
      {Array.from({ length: 5 }).map((_, index) => (
        <SidebarMenuItem key={index}>
          <SidebarMenuSkeleton showIcon />
        </SidebarMenuItem>
      ))}
    </SidebarMenu>
  )
}
```

----------------------------------------

TITLE: Applying Sidebar Display Variant in TSX
DESCRIPTION: Changes the visual style and behavior of the `Sidebar` component using the `variant` prop. Options include `sidebar` (standard), `floating` (overlay), and `inset` (pushes content).
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/sidebar.mdx#_snippet_16

LANGUAGE: tsx
CODE:
```
import { Sidebar } from "@/components/ui/sidebar"

export function AppSidebar() {
  return <Sidebar variant="sidebar | floating | inset" />
}
```

----------------------------------------

TITLE: Importing badgeVariants Helper (TSX)
DESCRIPTION: Imports the `badgeVariants` helper function, which allows applying badge styling to other HTML elements or components, such as a `Link`. This is useful for creating custom elements that visually resemble badges without being the `Badge` component itself.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/badge.mdx#_snippet_3

LANGUAGE: tsx
CODE:
```
import { badgeVariants } from "@/components/ui/badge"
```

----------------------------------------

TITLE: Displaying Loading State with SidebarMenuSkeleton (TSX)
DESCRIPTION: This function demonstrates how to use `SidebarMenuSkeleton` to render a loading state for `SidebarMenu` items. It's particularly useful when fetching data with React Server Components, SWR, or react-query to provide visual feedback during loading.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/sidebar.mdx#_snippet_35

LANGUAGE: tsx
CODE:
```
function NavProjectsSkeleton() {
  return (
    <SidebarMenu>
      {Array.from({ length: 5 }).map((_, index) => (
        <SidebarMenuItem key={index}>
          <SidebarMenuSkeleton />
        </SidebarMenuItem>
      ))}
    </SidebarMenu>
  )
}
```

----------------------------------------

TITLE: Example components.json Configuration
DESCRIPTION: This 'components.json' example demonstrates a typical configuration, including settings for style, Tailwind (config, CSS, base color, CSS variables), React Server Components (RSC), and import aliases. This file serves as the central configuration for how shadcn/ui components are integrated and styled within a project.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/changelog.mdx#_snippet_8

LANGUAGE: json
CODE:
```
{
  "style": "default",
  "tailwind": {
    "config": "tailwind.config.ts",
    "css": "src/app/globals.css",
    "baseColor": "zinc",
    "cssVariables": true
  },
  "rsc": false,
  "aliases": {
    "utils": "~/lib/utils",
    "components": "~/components"
  }
}
```

----------------------------------------

TITLE: Step 1: Adding SidebarProvider and Trigger to Root Layout (TSX)
DESCRIPTION: This snippet, as part of the "Your First Sidebar" guide, instructs adding `SidebarProvider` to wrap the application and `SidebarTrigger` within the `main` content. This setup is crucial for enabling the sidebar's context and providing a mechanism to toggle its visibility.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/sidebar.mdx#_snippet_6

LANGUAGE: tsx
CODE:
```
import { SidebarProvider, SidebarTrigger } from "@/components/ui/sidebar"
import { AppSidebar } from "@/components/app-sidebar"

export default function Layout({ children }: { children: React.ReactNode }) {
  return (
    <SidebarProvider>
      <AppSidebar />
      <main>
        <SidebarTrigger />
        {children}
      </main>
    </SidebarProvider>
  )
}
```

----------------------------------------

TITLE: Creating Theme Session Resolver in Remix
DESCRIPTION: This TypeScript snippet sets up a session storage using `createCookieSessionStorage` and then creates a theme session resolver with `createThemeSessionResolver` from `remix-themes`. It configures a secure cookie for theme persistence, adapting its domain and security settings based on the production environment.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/dark-mode/remix.mdx#_snippet_2

LANGUAGE: tsx
CODE:
```
import { createThemeSessionResolver } from "remix-themes"

// You can default to 'development' if process.env.NODE_ENV is not set
const isProduction = process.env.NODE_ENV === "production"

const sessionStorage = createCookieSessionStorage({
  cookie: {
    name: "theme",
    path: "/",
    httpOnly: true,
    sameSite: "lax",
    secrets: ["s3cr3t"],
    // Set domain and secure only if in production
    ...(isProduction
      ? { domain: "your-production-domain.com", secure: true }
      : {}),
  },
})

export const themeSessionResolver = createThemeSessionResolver(sessionStorage)
```

----------------------------------------

TITLE: Integrating Autoplay Plugin with Shadcn UI Carousel
DESCRIPTION: This snippet shows how to add plugins to the Shadcn UI Carousel component using the `plugins` prop. It specifically demonstrates integrating the `Autoplay` plugin from `embla-carousel-autoplay` to enable automatic slide transitions with a specified delay.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/carousel.mdx#_snippet_12

LANGUAGE: ts
CODE:
```
import Autoplay from "embla-carousel-autoplay"

export function Example() {
  return (
    <Carousel
      plugins={[
        Autoplay({
          delay: 2000,
        }),
      ]}
    >
      // ...
    </Carousel>
  )
}
```

----------------------------------------

TITLE: Basic Breadcrumb Component Usage in TSX
DESCRIPTION: This snippet illustrates the basic structure of a `Breadcrumb` component, using `BreadcrumbList`, `BreadcrumbItem`, `BreadcrumbLink`, `BreadcrumbPage`, and `BreadcrumbSeparator` to create a simple navigation path. It demonstrates how to define the hierarchy of links.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/breadcrumb.mdx#_snippet_2

LANGUAGE: tsx
CODE:
```
<Breadcrumb>
  <BreadcrumbList>
    <BreadcrumbItem>
      <BreadcrumbLink href="/">Home</BreadcrumbLink>
    </BreadcrumbItem>
    <BreadcrumbSeparator />
    <BreadcrumbItem>
      <BreadcrumbLink href="/components">Components</BreadcrumbLink>
    </BreadcrumbItem>
    <BreadcrumbSeparator />
    <BreadcrumbItem>
      <BreadcrumbPage>Breadcrumb</BreadcrumbPage>
    </BreadcrumbItem>
  </BreadcrumbList>
</Breadcrumb>
```

----------------------------------------

TITLE: Enabling Accessibility Layer for Line Charts (TSX)
DESCRIPTION: This snippet shows how to enable the `accessibilityLayer` prop on a `LineChart` component. Activating this prop enhances the chart with keyboard navigation and screen reader support, significantly improving its accessibility for users with disabilities.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/chart.mdx#_snippet_32

LANGUAGE: TSX
CODE:
```
<LineChart accessibilityLayer />
```

----------------------------------------

TITLE: Adding a shadcn/ui component
DESCRIPTION: Uses the `shadcn` CLI to add a specific component, such as `Button`, to your project. This command fetches and integrates the component's code into your designated UI components folder.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/installation/remix.mdx#_snippet_7

LANGUAGE: bash
CODE:
```
npx shadcn@latest add button
```

----------------------------------------

TITLE: Integrating Dropdown Menu with Breadcrumb Item in TSX
DESCRIPTION: This snippet shows how to compose a `BreadcrumbItem` with a `DropdownMenu` to create a clickable breadcrumb segment that reveals a dropdown menu. This pattern is useful for displaying sub-navigation options or related actions directly within the breadcrumb path.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/breadcrumb.mdx#_snippet_4

LANGUAGE: tsx
CODE:
```
import {
  DropdownMenu,
  DropdownMenuContent,
  DropdownMenuItem,
  DropdownMenuTrigger,
} from "@/components/ui/dropdown-menu"

...

<BreadcrumbItem>
  <DropdownMenu>
    <DropdownMenuTrigger className="flex items-center gap-1">
      Components
      <ChevronDownIcon />
    </DropdownMenuTrigger>
    <DropdownMenuContent align="start">
      <DropdownMenuItem>Documentation</DropdownMenuItem>
      <DropdownMenuItem>Themes</DropdownMenuItem>
      <DropdownMenuItem>GitHub</DropdownMenuItem>
    </DropdownMenuContent>
  </DropdownMenu>
</BreadcrumbItem>
```

----------------------------------------

TITLE: shadcn init Command Options (CLI Text)
DESCRIPTION: This section outlines the various options available for the `shadcn init` command, allowing users to customize the initialization process. Options include skipping prompts, using default configurations, forcing overwrites, specifying working directories, and controlling CSS variable usage.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/cli.mdx#_snippet_1

LANGUAGE: txt
CODE:
```
Usage: shadcn init [options] [components...]

initialize your project and install dependencies

Arguments:
  components         the components to add or a url to the component.

Options:
  -y, --yes           skip confirmation prompt. (default: true)
  -d, --defaults,     use default configuration. (default: false)
  -f, --force         force overwrite of existing configuration. (default: false)
  -c, --cwd <cwd>     the working directory. defaults to the current directory. (default: "/Users/shadcn/Desktop")
  -s, --silent        mute output. (default: false)
  --src-dir           use the src directory when creating a new project. (default: false)
  --no-src-dir        do not use the src directory when creating a new project.
  --css-variables     use css variables for theming. (default: true)
  --no-css-variables  do not use css variables for theming.
  -h, --help          display help for command
```

----------------------------------------

TITLE: Importing Navigation Menu Trigger Style Utility (TSX)
DESCRIPTION: Imports the navigationMenuTriggerStyle utility function, which is used to apply the correct visual styling to a navigation menu trigger, particularly when integrating with client-side routing libraries like Next.js Link.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/navigation-menu.mdx#_snippet_4

LANGUAGE: tsx
CODE:
```
import { navigationMenuTriggerStyle } from "@/components/ui/navigation-menu"
```

----------------------------------------

TITLE: Applying Theme Colors with Tailwind CSS
DESCRIPTION: This TypeScript/React snippet demonstrates how to use theme colors with Tailwind CSS by referencing a CSS variable directly within a Tailwind utility class. The `fill-[--color-desktop]` syntax allows dynamic styling of components like `<LabelList>` based on the defined CSS theme variables.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/chart.mdx#_snippet_23

LANGUAGE: tsx
CODE:
```
<LabelList className="fill-[--color-desktop]" />
```

----------------------------------------

TITLE: Defining Chart Configuration with Labels and Colors
DESCRIPTION: This TypeScript snippet demonstrates how to define a `ChartConfig` object. This configuration holds human-readable strings like labels, icons, and color tokens, which are used for theming and displaying meaningful information within the chart.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/chart.mdx#_snippet_6

LANGUAGE: tsx
CODE:
```
import { type ChartConfig } from "@/components/ui/chart"

const chartConfig = {
  desktop: {
    label: "Desktop",
    color: "#2563eb",
  },
  mobile: {
    label: "Mobile",
    color: "#60a5fa",
  },
} satisfies ChartConfig
```

----------------------------------------

TITLE: Fetching Project Data for Sidebar Menu with React Query in TSX
DESCRIPTION: This component utilizes React Query to fetch project data. It displays a loading skeleton during data retrieval and renders the project list upon successful data fetching. A placeholder for handling no data is also included.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/sidebar.mdx#_snippet_44

LANGUAGE: tsx
CODE:
```
function NavProjects() {
  const { data, isLoading } = useQuery()

  if (isLoading) {
    return (
      <SidebarMenu>
        {Array.from({ length: 5 }).map((_, index) => (
          <SidebarMenuItem key={index}>
            <SidebarMenuSkeleton showIcon />
          </SidebarMenuItem>
        ))}
      </SidebarMenu>
    )
  }

  if (!data) {
    return ...
  }

  return (
    <SidebarMenu>
      {data.map((project) => (
        <SidebarMenuItem key={project.name}>
          <SidebarMenuButton asChild>
            <a href={project.url}>
              <project.icon />
              <span>{project.name}</span>
            </a>
          </SidebarMenuButton>
        </SidebarMenuItem>
      ))}
    </SidebarMenu>
  )
}
```

----------------------------------------

TITLE: Configuring Custom Tooltip Labels and Names
DESCRIPTION: This TypeScript/React snippet defines `chartData` and `chartConfig` to support custom tooltip labels and names. The `chartData` includes `browser` and `visitors` keys, while `chartConfig` maps `visitors` to a label and defines colors for `chrome` and `safari`, enabling flexible tooltip content.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/chart.mdx#_snippet_26

LANGUAGE: tsx
CODE:
```
const chartData = [
  { browser: "chrome", visitors: 187, fill: "var(--color-chrome)" },
  { browser: "safari", visitors: 200, fill: "var(--color-safari)" },
]

const chartConfig = {
  visitors: {
    label: "Total Visitors",
  },
  chrome: {
    label: "Chrome",
    color: "hsl(var(--chart-1))",
  },
  safari: {
    label: "Safari",
    color: "hsl(var(--chart-2))",
  },
} satisfies ChartConfig
```

----------------------------------------

TITLE: Configuring `components.json` for Utility Class Theming
DESCRIPTION: This JSON snippet outlines the configuration in `components.json` to enable theming using direct utility classes. The critical setting is `"cssVariables": false` within the `tailwind` object, which disables the use of CSS variables and relies solely on Tailwind's built-in color utilities. This allows for direct class application like `bg-zinc-950`.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/theming.mdx#_snippet_3

LANGUAGE: json
CODE:
```
{
  "style": "default",
  "rsc": true,
  "tailwind": {
    "config": "",
    "css": "app/globals.css",
    "baseColor": "slate",
    "cssVariables": false
  },
  "aliases": {
    "components": "@/components",
    "utils": "@/lib/utils",
    "ui": "@/registry/new-york-v4/ui",
    "lib": "@/lib",
    "hooks": "@/hooks"
  },
  "iconLibrary": "lucide"
}
```

----------------------------------------

TITLE: Updating vite.config.ts for Path Resolution and Plugins
DESCRIPTION: This configuration file for Vite imports necessary plugins like `react()` and `tailwindcss()` and sets up a path alias `@` to resolve to the `src` directory, enabling absolute imports within the application.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/installation/vite.mdx#_snippet_6

LANGUAGE: typescript
CODE:
```
import path from "path"
import tailwindcss from "@tailwindcss/vite"
import react from "@vitejs/plugin-react"
import { defineConfig } from "vite"

// https://vite.dev/config/
export default defineConfig({
  plugins: [react(), tailwindcss()],
  resolve: {
    alias: {
      "@": path.resolve(__dirname, "./src")
    }
  }
})
```

----------------------------------------

TITLE: Creating a Theme Provider Component (TypeScript/React)
DESCRIPTION: This TypeScript React component, `ThemeProvider`, acts as a wrapper around `next-themes`'s `NextThemesProvider`. It enables the application to manage and apply themes, ensuring client-side rendering with the `'use client'` directive for Next.js App Router compatibility.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/dark-mode/next.mdx#_snippet_1

LANGUAGE: tsx
CODE:
```
"use client"

import * as React from "react"
import { ThemeProvider as NextThemesProvider } from "next-themes"

export function ThemeProvider({
  children,
  ...props
}: React.ComponentProps<typeof NextThemesProvider>) {
  return <NextThemesProvider {...props}>{children}</NextThemesProvider>
}
```

----------------------------------------

TITLE: Defining Chart Configuration Object (shadcn/ui, TypeScript)
DESCRIPTION: Defines a `chartConfig` object, typed as `ChartConfig`, which holds metadata for chart data series like labels, icons, and colors. This configuration is decoupled from the actual chart data, allowing for shared styling and flexible data integration.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/chart.mdx#_snippet_15

LANGUAGE: tsx
CODE:
```
import { Monitor } from "lucide-react"

import { type ChartConfig } from "@/components/ui/chart"

const chartConfig = {
  desktop: {
    label: "Desktop",
    icon: Monitor,
    // A color like 'hsl(220, 98%, 61%)' or 'var(--color-name)'
    color: "#2563eb",
    // OR a theme object with 'light' and 'dark' keys
    theme: {
      light: "#2563eb",
      dark: "#dc2626"
    }
  }
} satisfies ChartConfig
```

----------------------------------------

TITLE: Basic Usage of Slider Component in TSX
DESCRIPTION: This snippet demonstrates how to render the Slider component, setting its initial defaultValue to 33, a max value of 100, and a step increment of 1 for user interaction.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/slider.mdx#_snippet_3

LANGUAGE: tsx
CODE:
```
<Slider defaultValue={[33]} max={100} step={1} />
```

----------------------------------------

TITLE: Importing Label Component (TypeScript/TSX)
DESCRIPTION: This snippet imports the `Label` component from the local `ui/label` path within your project, making it available for use in a TypeScript or TSX file. It's a common pattern for modular component imports.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/label.mdx#_snippet_2

LANGUAGE: tsx
CODE:
```
import { Label } from "@/components/ui/label"
```

----------------------------------------

TITLE: Basic ScrollArea Component Usage in TSX
DESCRIPTION: This example shows a basic implementation of the `ScrollArea` component, wrapping content that exceeds its defined dimensions. It applies Tailwind CSS classes for styling, setting a fixed height and width, rounded corners, border, and padding.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/scroll-area.mdx#_snippet_3

LANGUAGE: tsx
CODE:
```
<ScrollArea className="h-[200px] w-[350px] rounded-md border p-4">
  Jokester began sneaking into the castle in the middle of the night and leaving
  jokes all over the place: under the king's pillow, in his soup, even in the
  royal toilet. The king was furious, but he couldn't seem to stop Jokester. And
  then, one day, the people of the kingdom discovered that the jokes left by
  Jokester were so funny that they couldn't help but laugh. And once they
  started laughing, they couldn't stop.
</ScrollArea>
```

----------------------------------------

TITLE: Configuring ESLint parserOptions for Type-Aware Linting in JavaScript
DESCRIPTION: This snippet demonstrates how to configure the `parserOptions` within an ESLint configuration file. It sets the ECMAScript version, source type, and specifies the project's TypeScript configuration files, which are crucial for enabling type-aware linting rules provided by `@typescript-eslint` plugins. This setup is a prerequisite for advanced type-checking linting.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/packages/shadcn/test/fixtures/vite-with-tailwind/README.md#_snippet_0

LANGUAGE: JavaScript
CODE:
```
export default {
  // other rules...
  parserOptions: {
    ecmaVersion: 'latest',
    sourceType: 'module',
    project: ['./tsconfig.json', './tsconfig.node.json'],
    tsconfigRootDir: __dirname,
  },
}
```

----------------------------------------

TITLE: Updating Shadcn UI Components via CLI in Bash
DESCRIPTION: This snippet provides the `npx shadcn@latest` command used to update all existing Shadcn UI components. The `--all` and `--overwrite` flags ensure that all components are refreshed, potentially overwriting local modifications, hence the prior recommendation to commit changes.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/tailwind-v4.mdx#_snippet_11

LANGUAGE: Bash
CODE:
```
npx shadcn@latest add --all --overwrite
```

----------------------------------------

TITLE: Creating a new Astro project
DESCRIPTION: Initializes a new Astro project using `create-astro`, configuring it with Tailwind CSS, React support, and Git integration. This command sets up the basic project structure and installs dependencies.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/installation/astro.mdx#_snippet_0

LANGUAGE: bash
CODE:
```
npx create-astro@latest astro-app  --template with-tailwindcss --install --add react --git
```

----------------------------------------

TITLE: Creating Vertical Resizable Panels (TypeScript/React)
DESCRIPTION: This snippet illustrates how to configure a `ResizablePanelGroup` to arrange panels vertically by setting the `direction` prop to 'vertical'. This allows for height adjustments between the panels, suitable for different layout needs.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/resizable.mdx#_snippet_4

LANGUAGE: tsx
CODE:
```
import {
  ResizableHandle,
  ResizablePanel,
  ResizablePanelGroup,
} from "@/components/ui/resizable"

export default function Example() {
  return (
    <ResizablePanelGroup direction="vertical">
      <ResizablePanel>One</ResizablePanel>
      <ResizableHandle />
      <ResizablePanel>Two</ResizablePanel>
    </ResizablePanelGroup>
  )
}
```

----------------------------------------

TITLE: Adding ChartLegend to BarChart (shadcn/ui, TypeScript)
DESCRIPTION: Integrates the `ChartLegend` component into a `BarChart`, displaying a legend that describes the data series. It uses `ChartLegendContent` to render the actual legend items, providing clarity on chart data.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/chart.mdx#_snippet_14

LANGUAGE: tsx
CODE:
```
<ChartContainer config={chartConfig} className="h-[200px] w-full">
  <BarChart accessibilityLayer data={chartData}>
    <CartesianGrid vertical={false} />
    <XAxis
      dataKey="month"
      tickLine={false}
      tickMargin={10}
      axisLine={false}
      tickFormatter={(value) => value.slice(0, 3)}
    />
    <ChartTooltip content={<ChartTooltipContent />} />
    <ChartLegend content={<ChartLegendContent />} />
    <Bar dataKey="desktop" fill="var(--color-desktop)" radius={4} />
    <Bar dataKey="mobile" fill="var(--color-mobile)" radius={4} />
  </BarChart>
</ChartContainer>
```

----------------------------------------

TITLE: Refactoring CardTitle and CardDescription for Accessibility (TypeScript/React)
DESCRIPTION: This snippet shows the updated implementation of `CardTitle` and `CardDescription` components. They are now `React.forwardRef` components that render `div` elements instead of `h3` and `p` respectively, improving accessibility. They accept standard HTML div attributes and apply specific styling via `cn` utility.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/card.mdx#_snippet_3

LANGUAGE: tsx
CODE:
```
const CardTitle = React.forwardRef<
  HTMLDivElement,
  React.HTMLAttributes<HTMLDivElement>
>(({ className, ...props }, ref) => (
  <div
    ref={ref}
    className={cn("font-semibold leading-none tracking-tight", className)}
    {...props}
  />
))
CardTitle.displayName = "CardTitle"

const CardDescription = React.forwardRef<
  HTMLDivElement,
  React.HTMLAttributes<HTMLDivElement>
>(({ className, ...props }, ref) => (
  <div
    ref={ref}
    className={cn("text-sm text-muted-foreground", className)}
    {...props}
  />
))
CardDescription.displayName = "CardDescription"
```

----------------------------------------

TITLE: Importing Input Component (TypeScript/React)
DESCRIPTION: This snippet demonstrates how to import the `Input` component from the project's UI components library, making it available for use within a TypeScript or React application.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/input.mdx#_snippet_1

LANGUAGE: tsx
CODE:
```
import { Input } from "@/components/ui/input"
```

----------------------------------------

TITLE: Importing useToast Hook (TypeScript/TSX)
DESCRIPTION: This snippet shows how to import the `useToast` hook from the application's custom hooks directory. This hook provides the necessary function to trigger and display toast notifications.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/toast.mdx#_snippet_3

LANGUAGE: tsx
CODE:
```
import { useToast } from "@/hooks/use-toast"
```

----------------------------------------

TITLE: Defining CSS Variables in Registry Item JSON
DESCRIPTION: This snippet demonstrates how to define custom CSS variables for a registry item using the `cssVars` property. It allows for theme-specific variables (e.g., `font-heading`) and mode-specific variables (e.g., `light` and `dark` themes for `brand` color and `radius`). These variables can then be consumed by components within the project.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/registry/registry-item-json.mdx#_snippet_11

LANGUAGE: json
CODE:
```
{
  "cssVars": {
    "theme": {
      "font-heading": "Poppins, sans-serif"
    },
    "light": {
      "brand": "20 14.3% 4.1%",
      "radius": "0.5rem"
    },
    "dark": {
      "brand": "20 14.3% 4.1%"
    }
  }
}
```

----------------------------------------

TITLE: Adding shadcn/ui Components (CLI)
DESCRIPTION: This command adds a specified shadcn/ui component to the project. When run from an application directory (e.g., 'apps/web'), the CLI intelligently installs the component files under 'packages/ui' and updates import paths within the application.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/monorepo.mdx#_snippet_3

LANGUAGE: bash
CODE:
```
npx shadcn@canary add [COMPONENT]
```

----------------------------------------

TITLE: Theming with CSS Variables (TSX)
DESCRIPTION: This TSX snippet illustrates theming components using CSS variables, specifically `bg-background` and `text-foreground`. This approach leverages custom properties defined in CSS, providing a more centralized and maintainable way to manage design tokens.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/changelog.mdx#_snippet_17

LANGUAGE: tsx
CODE:
```
<div className="bg-background text-foreground" />
```

----------------------------------------

TITLE: Importing and using shadcn/ui Button component
DESCRIPTION: Demonstrates how to import and use the newly added `Button` component from `~/components/ui/button` within a Remix component. This shows basic usage of a shadcn/ui component in a JSX context.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/installation/remix.mdx#_snippet_8

LANGUAGE: tsx
CODE:
```
import { Button } from "~/components/ui/button"

export default function Home() {
  return (
    <div>
      <Button>Click me</Button>
    </div>
  )
}
```

----------------------------------------

TITLE: Building a Sidebar Menu with Project List in TSX
DESCRIPTION: This example illustrates how to construct a navigation menu using `SidebarMenu` within a `SidebarGroup`. It iterates over a `projects` array to render `SidebarMenuItem` components, each containing a `SidebarMenuButton` that acts as a link to a project URL, displaying its icon and name.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/sidebar.mdx#_snippet_26

LANGUAGE: tsx
CODE:
```
<Sidebar>
  <SidebarContent>
    <SidebarGroup>
      <SidebarGroupLabel>Projects</SidebarGroupLabel>
      <SidebarGroupContent>
        <SidebarMenu>
          {projects.map((project) => (
            <SidebarMenuItem key={project.name}>
              <SidebarMenuButton asChild>
                <a href={project.url}>
                  <project.icon />
                  <span>{project.name}</span>
                </a>
              </SidebarMenuButton>
            </SidebarMenuItem>
          ))}
        </SidebarMenu>
      </SidebarGroupContent>
    </SidebarGroup>
  </SidebarContent>
</Sidebar>
```

----------------------------------------

TITLE: Importing Dialog Components in TypeScript/React
DESCRIPTION: This snippet demonstrates how to import the various sub-components required to construct a shadcn/ui Dialog. These imports enable the use of Dialog, DialogContent, DialogHeader, and other related elements within a React application.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/dialog.mdx#_snippet_2

LANGUAGE: tsx
CODE:
```
import {
  Dialog,
  DialogContent,
  DialogDescription,
  DialogHeader,
  DialogTitle,
  DialogTrigger,
} from "@/components/ui/dialog"
```

----------------------------------------

TITLE: Importing shadcn/ui Components (TypeScript/TSX)
DESCRIPTION: This example demonstrates how to import a shadcn/ui component, such as 'Button', from the shared '@workspace/ui' package. Components are typically located under 'components' within this workspace.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/monorepo.mdx#_snippet_4

LANGUAGE: tsx
CODE:
```
import { Button } from "@workspace/ui/components/button"
```

----------------------------------------

TITLE: Applying Custom Tooltip Keys
DESCRIPTION: This TypeScript/React snippet shows how to configure the `ChartTooltipContent` component to use custom keys for the tooltip label and name. By setting `labelKey` to 'visitors' and `nameKey` to 'browser', the tooltip will display 'Total Visitors' as its label and 'Chrome'/'Safari' as names, based on the provided data.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/chart.mdx#_snippet_27

LANGUAGE: tsx
CODE:
```
<ChartTooltip
  content={<ChartTooltipContent labelKey="visitors" nameKey="browser" />}
/>
```

----------------------------------------

TITLE: Creating Basic Horizontal Resizable Panels (TypeScript/React)
DESCRIPTION: This example shows a fundamental setup for a horizontal resizable panel group. It includes two `ResizablePanel` components separated by a `ResizableHandle`, allowing users to adjust the width of the panels dynamically.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/resizable.mdx#_snippet_3

LANGUAGE: tsx
CODE:
```
<ResizablePanelGroup direction="horizontal">
  <ResizablePanel>One</ResizablePanel>
  <ResizableHandle />
  <ResizablePanel>Two</ResizablePanel>
</ResizablePanelGroup>
```

----------------------------------------

TITLE: Installing Tabs Component via Shadcn CLI (Bash)
DESCRIPTION: This command utilizes the Shadcn UI CLI to quickly add the Tabs component and its dependencies to your project, streamlining the installation process.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/tabs.mdx#_snippet_0

LANGUAGE: bash
CODE:
```
npx shadcn@latest add tabs
```

----------------------------------------

TITLE: Installing TanStack React Table Dependency (Bash)
DESCRIPTION: This command installs the core `@tanstack/react-table` library, which is a headless UI library essential for managing table state, including sorting, filtering, and pagination. It's a prerequisite for building advanced table functionalities.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/data-table.mdx#_snippet_1

LANGUAGE: bash
CODE:
```
npm install @tanstack/react-table
```

----------------------------------------

TITLE: Creating Inline Theme Script in Astro
DESCRIPTION: This Astro snippet defines an inline script that detects the user's preferred theme from localStorage or system preferences. It applies the 'dark' class to the document's root element and uses a MutationObserver to persist theme changes back to localStorage, ensuring the theme is remembered across sessions.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/dark-mode/astro.mdx#_snippet_0

LANGUAGE: Astro
CODE:
```
--- import '../styles/globals.css' ---\n\n<script is:inline>\n\tconst getThemePreference = () => {\n\t\tif (typeof localStorage !== 'undefined' && localStorage.getItem('theme')) {\n\t\t\treturn localStorage.getItem('theme');\n\t\t}\n\t\treturn window.matchMedia('(prefers-color-scheme: dark)').matches ? 'dark' : 'light';\n\t};\n\tconst isDark = getThemePreference() === 'dark';\n\tdocument.documentElement.classList[isDark ? 'add' : 'remove']('dark');\n\n\tif (typeof localStorage !== 'undefined') {\n\t\tconst observer = new MutationObserver(() => {\n\t\t\tconst isDark = document.documentElement.classList.contains('dark');\n\t\t\tlocalStorage.setItem('theme', isDark ? 'dark' : 'light');\n\t\t});\n\t\tobserver.observe(document.documentElement, { attributes: true, attributeFilter: ['class'] });\n\t}\n</script>\n\n<html lang="en">\n\t<body>\n          <h1>Astro</h1>\n\t</body>\n</html>
```

----------------------------------------

TITLE: Rendering Progress Component with Value (TypeScript/React)
DESCRIPTION: This JSX snippet demonstrates how to render the `Progress` component, setting its current completion `value` to 33. The `value` prop is a number between 0 and 100, indicating the progress percentage.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/progress.mdx#_snippet_3

LANGUAGE: tsx
CODE:
```
<Progress value={33} />
```

----------------------------------------

TITLE: Initializing a New shadcn/ui Monorepo Project (CLI)
DESCRIPTION: This command initializes a new shadcn/ui project. When prompted, select the 'Next.js (Monorepo)' option to set up a monorepo structure with 'web' and 'ui' workspaces, using Turborepo as the build system.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/monorepo.mdx#_snippet_0

LANGUAGE: bash
CODE:
```
npx shadcn@canary init
```

----------------------------------------

TITLE: Adding Sidebar CSS Variables for Theming (CLI)
DESCRIPTION: These CSS custom properties define the color palette for the sidebar component, including background, foreground, primary, accent, border, and ring colors for both light and dark themes. They are typically installed automatically by the CLI command.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/sidebar.mdx#_snippet_1

LANGUAGE: css
CODE:
```
@layer base {
  :root {
    --sidebar-background: 0 0% 98%;
    --sidebar-foreground: 240 5.3% 26.1%;
    --sidebar-primary: 240 5.9% 10%;
    --sidebar-primary-foreground: 0 0% 98%;
    --sidebar-accent: 240 4.8% 95.9%;
    --sidebar-accent-foreground: 240 5.9% 10%;
    --sidebar-border: 220 13% 91%;
    --sidebar-ring: 217.2 91.2% 59.8%;
  }

  .dark {
    --sidebar-background: 240 5.9% 10%;
    --sidebar-foreground: 240 4.8% 95.9%;
    --sidebar-primary: 224.3 76.3% 48%;
    --sidebar-primary-foreground: 0 0% 100%;
    --sidebar-accent: 240 3.7% 15.9%;
    --sidebar-accent-foreground: 240 4.8% 95.9%;
    --sidebar-border: 240 3.7% 15.9%;
    --sidebar-ring: 217.2 91.2% 59.8%;
  }
}
```

----------------------------------------

TITLE: shadcn add Command Options (CLI Text)
DESCRIPTION: This section details the available options for the `shadcn add` command, providing flexibility when adding components. Options include overwriting existing files, adding all available components, specifying a custom path for component installation, and controlling output verbosity.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/cli.mdx#_snippet_3

LANGUAGE: txt
CODE:
```
Usage: shadcn add [options] [components...]

add a component to your project

Arguments:
  components         the components to add or a url to the component.

Options:
  -y, --yes           skip confirmation prompt. (default: false)
  -o, --overwrite     overwrite existing files. (default: false)
  -c, --cwd <cwd>     the working directory. defaults to the current directory. (default: "/Users/shadcn/Desktop")
  -a, --all           add all available components (default: false)
  -p, --path <path>   the path to add the component to.
  -s, --silent        mute output. (default: false)
  --src-dir           use the src directory when creating a new project. (default: false)
  --no-src-dir        do not use the src directory when creating a new project.
  --css-variables     use css variables for theming. (default: true)
  --no-css-variables  do not use css variables for theming.
  -h, --help          display help for command
```

----------------------------------------

TITLE: Fetching Project Data for Sidebar Menu with React Server Components in TSX
DESCRIPTION: This asynchronous React Server Component fetches project data using `fetchProjects()`. It then maps over the fetched projects to render `SidebarMenuItem` components, each linking to a project URL and displaying its icon and name.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/sidebar.mdx#_snippet_41

LANGUAGE: tsx
CODE:
```
async function NavProjects() {
  const projects = await fetchProjects()

  return (
    <SidebarMenu>
      {projects.map((project) => (
        <SidebarMenuItem key={project.name}>
          <SidebarMenuButton asChild>
            <a href={project.url}>
              <project.icon />
              <span>{project.name}</span>
            </a>
          </SidebarMenuButton>
        </SidebarMenuItem>
      ))}
    </SidebarMenu>
  )
}
```

----------------------------------------

TITLE: Initializing Remix Project and Git Repository
DESCRIPTION: These commands perform the initial setup for a newly created Remix project. `npx remix init` runs the stack's initialization script, while the `git` commands initialize a new Git repository, stage all changes, and create an initial commit, preparing the project for version control.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/packages/shadcn/test/fixtures/frameworks/remix-indie-stack/README.md#_snippet_1

LANGUAGE: sh
CODE:
```
npx remix init
git init # if you haven't already
git add .
git commit -m "Initialize project"
```

----------------------------------------

TITLE: Adding Tailwind CSS v3 Colors to shadcn/ui Registry (JSON)
DESCRIPTION: This JSON snippet illustrates how to add new Tailwind CSS v3 colors to a `shadcn/ui` registry item. It requires defining color variables in `cssVars` for light and dark modes, and then mapping these variables to Tailwind's `theme.extend.colors` configuration, enabling the CLI to update both CSS and `tailwind.config.js` for new utility classes.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/registry/faq.mdx#_snippet_2

LANGUAGE: json
CODE:
```
{
  "$schema": "https://ui.shadcn.com/schema/registry-item.json",
  "name": "hello-world",
  "title": "Hello World",
  "type": "registry:block",
  "description": "A complex hello world component",
  "files": [
    // ...
  ],
  "cssVars": {
    "light": {
      "brand-background": "20 14.3% 4.1%",
      "brand-accent": "20 14.3% 4.1%"
    },
    "dark": {
      "brand-background": "20 14.3% 4.1%",
      "brand-accent": "20 14.3% 4.1%"
    }
  },
  "tailwind": {
    "config": {
      "theme": {
        "extend": {
          "colors": {
            "brand": {
              "DEFAULT": "hsl(var(--brand-background))",
              "accent": "hsl(var(--brand-accent))"
            }
          }
        }
      }
    }
  }
}
```

----------------------------------------

TITLE: Basic Drawer Component Usage (TSX)
DESCRIPTION: This example illustrates a fundamental implementation of the Drawer component, showcasing its structure with a trigger button, a header containing a title and description, and a footer with action buttons for submission and cancellation.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/drawer.mdx#_snippet_3

LANGUAGE: tsx
CODE:
```
<Drawer>
  <DrawerTrigger>Open</DrawerTrigger>
  <DrawerContent>
    <DrawerHeader>
      <DrawerTitle>Are you absolutely sure?</DrawerTitle>
      <DrawerDescription>This action cannot be undone.</DrawerDescription>
    </DrawerHeader>
    <DrawerFooter>
      <Button>Submit</Button>
      <DrawerClose>
        <Button variant="outline">Cancel</Button>
      </DrawerClose>
    </DrawerFooter>
  </DrawerContent>
</Drawer>
```

----------------------------------------

TITLE: Theming with Tailwind Utility Classes (TSX)
DESCRIPTION: This TSX snippet demonstrates how components can be themed directly using Tailwind CSS utility classes, such as `bg-zinc-950` for dark backgrounds and `dark:bg-white` for light backgrounds in dark mode. This method offers direct control over styling via class names.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/changelog.mdx#_snippet_15

LANGUAGE: tsx
CODE:
```
<div className="bg-zinc-950 dark:bg-white" />
```

----------------------------------------

TITLE: Defining Initial CSS Variables with @layer base in CSS
DESCRIPTION: This snippet shows the initial setup of CSS variables for background and foreground colors within a `@layer base` directive, where the color values are defined directly as `0 0% 100%` and `0 0% 3.9%` respectively, and then referenced with `hsl()` wrappers under a `@theme` directive.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/tailwind-v4.mdx#_snippet_2

LANGUAGE: CSS
CODE:
```
@layer base {
  :root {
    --background: 0 0% 100%;
    --foreground: 0 0% 3.9%;
  }
}

@theme {
  --color-background: hsl(var(--background));
  --color-foreground: hsl(var(--foreground));
}
```

----------------------------------------

TITLE: Setting Carousel Orientation
DESCRIPTION: Shows how to change the carousel's orientation to either vertical or horizontal by passing the desired value to the `orientation` prop on the `Carousel` component.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/carousel.mdx#_snippet_8

LANGUAGE: tsx
CODE:
```
<Carousel orientation="vertical | horizontal">
  <CarouselContent>
    <CarouselItem>...</CarouselItem>
    <CarouselItem>...</CarouselItem>
    <CarouselItem>...</CarouselItem>
  </CarouselContent>
</Carousel>
```

----------------------------------------

TITLE: Choosing TypeScript or JavaScript Components (JSON)
DESCRIPTION: This option determines whether components are generated as TypeScript (`true`) or JavaScript (`false`) files. Setting it to `false` will result in components with the `.jsx` file extension.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components-json.mdx#_snippet_9

LANGUAGE: json
CODE:
```
{
  "tsx": `true` | `false`
}
```

----------------------------------------

TITLE: Configuring tsconfig.app.json for IDE Path Resolution
DESCRIPTION: This snippet updates `tsconfig.app.json` with `baseUrl` and `paths` to ensure that IDEs can correctly resolve module imports using the `@/` alias, providing better development experience and autocompletion.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/installation/vite.mdx#_snippet_4

LANGUAGE: typescript
CODE:
```
{
  "compilerOptions": {
    // ...
    "baseUrl": ".",
    "paths": {
      "@/*": [
        "./src/*"
      ]
    }
    // ...
  }
}
```

----------------------------------------

TITLE: Configuring TypeScript Path Aliases for TanStack Start
DESCRIPTION: This `tsconfig.json` configuration adds a `paths` alias, mapping `@/*` to `./app/*`. This allows for cleaner absolute imports within the project, such as `import appCss from "@/styles/app.css?url"`, improving code readability and maintainability.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/installation/tanstack.mdx#_snippet_4

LANGUAGE: typescript
CODE:
```
{
  "compilerOptions": {
    "jsx": "react-jsx",
    "moduleResolution": "Bundler",
    "module": "ESNext",
    "target": "ES2022",
    "skipLibCheck": true,
    "strictNullChecks": true,
    "baseUrl": ".",
    "paths": {
      "@/*": ["./app/*"]
    }
  }
}
```

----------------------------------------

TITLE: Basic AppSidebar Component Structure (TSX)
DESCRIPTION: This code defines the `AppSidebar` component, showcasing a basic structure using `Sidebar`, `SidebarContent`, `SidebarFooter`, `SidebarGroup`, and `SidebarHeader` from the `@/components/ui/sidebar` module. It illustrates how these components are nested to form a complete sidebar layout.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/sidebar.mdx#_snippet_5

LANGUAGE: tsx
CODE:
```
import {
  Sidebar,
  SidebarContent,
  SidebarFooter,
  SidebarGroup,
  SidebarHeader,
} from "@/components/ui/sidebar"

export function AppSidebar() {
  return (
    <Sidebar>
      <SidebarHeader />
      <SidebarContent>
        <SidebarGroup />
        <SidebarGroup />
      </SidebarContent>
      <SidebarFooter />
    </Sidebar>
  )
}
```

----------------------------------------

TITLE: Applying Custom Pattern to Input OTP
DESCRIPTION: This TypeScript/React snippet illustrates how to use the `pattern` prop with `REGEXP_ONLY_DIGITS_AND_CHARS` to enforce an alphanumeric input pattern for the OTP. This allows for custom validation rules beyond just digits.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/input-otp.mdx#_snippet_5

LANGUAGE: tsx
CODE:
```
import { REGEXP_ONLY_DIGITS_AND_CHARS } from "input-otp"

...

<InputOTP
  maxLength={6}
  pattern={REGEXP_ONLY_DIGITS_AND_CHARS}
>
  <InputOTPGroup>
    <InputOTPSlot index={0} />
    {/* ... */}
  </InputOTPGroup>
</InputOTP>
```

----------------------------------------

TITLE: Defining Sample Chart Data for Recharts
DESCRIPTION: This TypeScript snippet illustrates how to structure data for a chart, providing an example with monthly desktop and mobile user counts. It emphasizes that data can be in any shape, and the `dataKey` prop is used to map specific data points to the chart elements.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/chart.mdx#_snippet_5

LANGUAGE: tsx
CODE:
```
const chartData = [
  { month: "January", desktop: 186, mobile: 80 },
  { month: "February", desktop: 305, mobile: 200 },
  { month: "March", desktop: 237, mobile: 120 },
  { month: "April", desktop: 73, mobile: 190 },
  { month: "May", desktop: 209, mobile: 130 },
  { month: "June", desktop: 214, mobile: 140 },
]
```

----------------------------------------

TITLE: Importing Carousel Components
DESCRIPTION: Imports the necessary components for the shadcn/ui Carousel, including the main Carousel container and its sub-components for content, individual items, and navigation buttons.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/carousel.mdx#_snippet_2

LANGUAGE: tsx
CODE:
```
import {
  Carousel,
  CarouselContent,
  CarouselItem,
  CarouselNext,
  CarouselPrevious,
} from "@/components/ui/carousel"
```

----------------------------------------

TITLE: Importing Sheet Components (TypeScript/TSX)
DESCRIPTION: This snippet illustrates the necessary import statements for using the various sub-components of the Shadcn UI Sheet, such as `Sheet`, `SheetContent`, and `SheetTrigger`, within a TypeScript/TSX React application. These imports make the components available for use in your JSX.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/sheet.mdx#_snippet_2

LANGUAGE: tsx
CODE:
```
import {
  Sheet,
  SheetContent,
  SheetDescription,
  SheetHeader,
  SheetTitle,
  SheetTrigger,
} from "@/components/ui/sheet"
```

----------------------------------------

TITLE: Setting Carousel Item Spacing
DESCRIPTION: Demonstrates how to create spacing between carousel items using a negative margin on `CarouselContent` (`-ml-4`) and corresponding padding on each `CarouselItem` (`pl-4`).
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/carousel.mdx#_snippet_6

LANGUAGE: tsx
CODE:
```
<Carousel>
  <CarouselContent className="-ml-4">
    <CarouselItem className="pl-4">...</CarouselItem>
    <CarouselItem className="pl-4">...</CarouselItem>
    <CarouselItem className="pl-4">...</CarouselItem>
  </CarouselContent>
</Carousel>
```

----------------------------------------

TITLE: Configuring Component Style in `components.json` (JSON)
DESCRIPTION: This `components.json` snippet demonstrates how to set the `style` property, which defines the visual foundation of your components, including shapes, icons, animations, and typography. Users can choose between predefined styles like `default` or `new-york`.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/changelog.mdx#_snippet_21

LANGUAGE: json
CODE:
```
{
  "style": "new-york"
}
```

----------------------------------------

TITLE: Implementing Collapsed Breadcrumb with Ellipsis in TSX
DESCRIPTION: This example demonstrates using the `BreadcrumbEllipsis` component to represent a collapsed state when the breadcrumb path becomes too long. It helps manage horizontal space by condensing multiple intermediate items into an ellipsis, which can then expand to reveal the full path.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/breadcrumb.mdx#_snippet_5

LANGUAGE: tsx
CODE:
```
import { BreadcrumbEllipsis } from "@/components/ui/breadcrumb"

...

<Breadcrumb>
  <BreadcrumbList>
    {/* ... */}
    <BreadcrumbItem>
      <BreadcrumbEllipsis />
    </BreadcrumbItem>
    {/* ... */}
  </BreadcrumbList>
</Breadcrumb>
```

----------------------------------------

TITLE: Styling Menu Actions Based on Active Button State in React
DESCRIPTION: This TypeScript React snippet shows how to force the visibility of a `SidebarMenuAction` when its sibling `SidebarMenuButton` is active. It uses Tailwind CSS's `peer-data-[active=true]/menu-button:opacity-100` utility to apply styling based on the `menu-button` peer's data attribute.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/sidebar.mdx#_snippet_48

LANGUAGE: tsx
CODE:
```
<SidebarMenuItem>
  <SidebarMenuButton />
  <SidebarMenuAction className="peer-data-[active=true]/menu-button:opacity-100" />
</SidebarMenuItem>
```

----------------------------------------

TITLE: Defining Custom Warning Colors in CSS
DESCRIPTION: This CSS snippet defines custom `warning` and `warning-foreground` colors using OKLCH format for both light (`:root`) and dark (`.dark`) themes. It also maps these custom variables to `--color-warning` and `--color-warning-foreground` using `@theme inline` for broader utility.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/theming.mdx#_snippet_7

LANGUAGE: css
CODE:
```
:root {
  --warning: oklch(0.84 0.16 84);
  --warning-foreground: oklch(0.28 0.07 46);
}

.dark {
  --warning: oklch(0.41 0.11 46);
  --warning-foreground: oklch(0.99 0.02 95);
}

@theme inline {
  --color-warning: var(--warning);
  --color-warning-foreground: var(--warning-foreground);
}
```

----------------------------------------

TITLE: Configuring shadcn/ui components.json for apps/web (Tailwind CSS v4)
DESCRIPTION: This configuration defines the `components.json` settings for a web application (`apps/web`) using Tailwind CSS v4. Key aspects include setting `rsc` and `tsx` to true, an empty `tailwind.config` path as required for v4, and defining aliases for component, hook, lib, and utility imports, including workspace-specific paths for `utils` and `ui`.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/monorepo.mdx#_snippet_6

LANGUAGE: json
CODE:
```
{
  "$schema": "https://ui.shadcn.com/schema.json",
  "style": "new-york",
  "rsc": true,
  "tsx": true,
  "tailwind": {
    "config": "",
    "css": "../../packages/ui/src/styles/globals.css",
    "baseColor": "zinc",
    "cssVariables": true
  },
  "iconLibrary": "lucide",
  "aliases": {
    "components": "@/components",
    "hooks": "@/hooks",
    "lib": "@/lib",
    "utils": "@workspace/ui/lib/utils",
    "ui": "@workspace/ui/components"
  }
}
```

----------------------------------------

TITLE: Adding shadcn/ui Components to Web App (Bash)
DESCRIPTION: This command adds a specific shadcn/ui component (e.g., 'button') to the designated web application within the monorepo. The `-c` flag specifies the target application directory, and the components are placed in `packages/ui/src/components`.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/templates/monorepo-next/README.md#_snippet_1

LANGUAGE: bash
CODE:
```
pnpm dlx shadcn@latest add button -c apps/web
```

----------------------------------------

TITLE: Adding CartesianGrid Component to BarChart (Recharts, TypeScript)
DESCRIPTION: Integrates the `CartesianGrid` component into a `BarChart` to display a grid. The `vertical={false}` prop disables vertical grid lines, providing a horizontal-only grid for the chart.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/chart.mdx#_snippet_8

LANGUAGE: tsx
CODE:
```
<ChartContainer config={chartConfig} className="min-h-[200px] w-full">
  <BarChart accessibilityLayer data={chartData}>
    <CartesianGrid vertical={false} />
    <Bar dataKey="desktop" fill="var(--color-desktop)" radius={4} />
    <Bar dataKey="mobile" fill="var(--color-mobile)" radius={4} />
  </BarChart>
</ChartContainer>
```

----------------------------------------

TITLE: Rendering Separator Component (TSX)
DESCRIPTION: Renders the `Separator` component in your JSX/TSX, creating a visual or semantic divider. It can be used without any props for a basic horizontal line.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/separator.mdx#_snippet_3

LANGUAGE: tsx
CODE:
```
<Separator />
```

----------------------------------------

TITLE: Importing Drawer Components in TSX
DESCRIPTION: This snippet demonstrates how to import various sub-components of the Drawer (e.g., Drawer, DrawerClose, DrawerContent) from the local UI components path, making them accessible for building the drawer interface in a React/Next.js application.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/drawer.mdx#_snippet_2

LANGUAGE: tsx
CODE:
```
import {
  Drawer,
  DrawerClose,
  DrawerContent,
  DrawerDescription,
  DrawerFooter,
  DrawerHeader,
  DrawerTitle,
  DrawerTrigger,
} from "@/components/ui/drawer"
```

----------------------------------------

TITLE: Importing Card Components (TypeScript/React)
DESCRIPTION: This snippet shows the necessary import statement for using the Shadcn UI Card components in a TypeScript React project. It imports various sub-components like `Card`, `CardHeader`, `CardTitle`, `CardDescription`, `CardContent`, and `CardFooter` from the local UI components path.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/card.mdx#_snippet_1

LANGUAGE: tsx
CODE:
```
import {
  Card,
  CardContent,
  CardDescription,
  CardFooter,
  CardHeader,
  CardTitle,
} from "@/components/ui/card"
```

----------------------------------------

TITLE: Importing Table Components (shadcn/ui) in TSX
DESCRIPTION: This snippet shows the necessary import statements for using the shadcn/ui Table components in a TypeScript React (TSX) project. It imports various sub-components like Table, TableBody, TableCaption, TableCell, TableHead, TableHeader, and TableRow from the local UI components path, typically `src/components/ui/table`.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/table.mdx#_snippet_1

LANGUAGE: tsx
CODE:
```
import {
  Table,
  TableBody,
  TableCaption,
  TableCell,
  TableHead,
  TableHeader,
  TableRow,
} from "@/components/ui/table"
```

----------------------------------------

TITLE: Basic Toggle Group Usage with Single Selection in TSX
DESCRIPTION: This example shows a basic implementation of the ToggleGroup component configured for single selection. It includes three ToggleGroupItem children, each with a distinct value.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/toggle-group.mdx#_snippet_3

LANGUAGE: tsx
CODE:
```
<ToggleGroup type="single">\n  <ToggleGroupItem value="a">A</ToggleGroupItem>\n  <ToggleGroupItem value="b">B</ToggleGroupItem>\n  <ToggleGroupItem value="c">C</ToggleGroupItem>\n</ToggleGroup>
```

----------------------------------------

TITLE: Importing CartesianGrid for Chart Grid (Recharts, TypeScript)
DESCRIPTION: Imports the `CartesianGrid` component from `recharts` to enable grid lines within a chart. This component is essential for visually structuring chart data with a grid.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/chart.mdx#_snippet_7

LANGUAGE: tsx
CODE:
```
import { Bar, BarChart, CartesianGrid } from "recharts"
```

----------------------------------------

TITLE: Importing Command Components in TypeScript/React
DESCRIPTION: This snippet imports various sub-components of the `Command` component from the shadcn/ui library, making them available for use in a React application. These components are essential for building a complete command menu.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/command.mdx#_snippet_2

LANGUAGE: tsx
CODE:
```
import {
  Command,
  CommandDialog,
  CommandEmpty,
  CommandGroup,
  CommandInput,
  CommandItem,
  CommandList,
  CommandSeparator,
  CommandShortcut,
} from "@/components/ui/command"
```

----------------------------------------

TITLE: Importing Navigation Menu Components (TSX)
DESCRIPTION: Imports various sub-components of the Shadcn UI Navigation Menu, such as NavigationMenu, NavigationMenuContent, and NavigationMenuItem, from the local UI components path for use in a React/TypeScript application.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/navigation-menu.mdx#_snippet_2

LANGUAGE: tsx
CODE:
```
import {
  NavigationMenu,
  NavigationMenuContent,
  NavigationMenuIndicator,
  NavigationMenuItem,
  NavigationMenuLink,
  NavigationMenuList,
  NavigationMenuTrigger,
  NavigationMenuViewport,
} from "@/components/ui/navigation-menu"
```

----------------------------------------

TITLE: Implementing Dark Mode Toggle Component
DESCRIPTION: This TSX component provides a UI element for toggling between light and dark modes. It utilizes `useTheme` from `remix-themes` to update the theme and displays appropriate icons (Sun/Moon) based on the current theme, offering a user-friendly way to switch the application's appearance.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/dark-mode/remix.mdx#_snippet_5

LANGUAGE: tsx
CODE:
```
import { Moon, Sun } from "lucide-react"
import { Theme, useTheme } from "remix-themes"

import { Button } from "./ui/button"
import {
  DropdownMenu,
  DropdownMenuContent,
  DropdownMenuItem,
  DropdownMenuTrigger,
} from "./ui/dropdown-menu"

export function ModeToggle() {
  const [, setTheme] = useTheme()

  return (
    <DropdownMenu>
      <DropdownMenuTrigger asChild>
        <Button variant="ghost" size="icon">
          <Sun className="h-[1.2rem] w-[1.2rem] rotate-0 scale-100 transition-all dark:-rotate-90 dark:scale-0" />
          <Moon className="absolute h-[1.2rem] w-[1.2rem] rotate-90 scale-0 transition-all dark:rotate-0 dark:scale-100" />
          <span className="sr-only">Toggle theme</span>
        </Button>
      </DropdownMenuTrigger>
      <DropdownMenuContent align="end">
        <DropdownMenuItem onClick={() => setTheme(Theme.LIGHT)}>
          Light
        </DropdownMenuItem>
        <DropdownMenuItem onClick={() => setTheme(Theme.DARK)}>
          Dark
        </DropdownMenuItem>
      </DropdownMenuContent>
    </DropdownMenu>
  )
}
```

----------------------------------------

TITLE: Using Custom Warning Colors in TSX Components
DESCRIPTION: This TSX snippet demonstrates how to apply the newly defined `warning` and `warning-foreground` utility classes to a `div` element. These classes, once configured in Tailwind CSS, will apply the custom colors defined in the CSS variables.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/theming.mdx#_snippet_8

LANGUAGE: tsx
CODE:
```
<div className="bg-warning text-warning-foreground" />
```

----------------------------------------

TITLE: Configuring Tailwind CSS for Sidebar Colors
DESCRIPTION: This JavaScript object extends the `theme.extend.colors` section in your `tailwind.config.js` file. It maps the custom CSS variables for sidebar colors to Tailwind's color system, enabling the use of utility classes like `bg-sidebar` for styling.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/sidebar.mdx#_snippet_3

LANGUAGE: javascript
CODE:
```
// ...
sidebar: {
  DEFAULT: 'hsl(var(--sidebar-background))',
  foreground: 'hsl(var(--sidebar-foreground))',
  primary: 'hsl(var(--sidebar-primary))',
  'primary-foreground': 'hsl(var(--sidebar-primary-foreground))',
  accent: 'hsl(var(--sidebar-accent))',
  'accent-foreground': 'hsl(var(--sidebar-accent-foreground))',
  border: 'hsl(var(--sidebar-border))',
  ring: 'hsl(var(--sidebar-ring))'
},
// ...
```

----------------------------------------

TITLE: Using Label Component in TSX Markup
DESCRIPTION: This TSX snippet demonstrates how to render the `Label` component, associating it with an input element using the `htmlFor` prop and providing a descriptive text. The `htmlFor` attribute links the label to its corresponding form control.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/label.mdx#_snippet_3

LANGUAGE: tsx
CODE:
```
<Label htmlFor="email">Your email address</Label>
```

----------------------------------------

TITLE: Applying Theme Colors to Chart Components (Bar)
DESCRIPTION: This TypeScript/React snippet shows how to apply a theme color to a chart component, specifically a `<Bar>` element. The `fill` prop references a CSS variable (e.g., `--color-desktop`), allowing the bar's color to be dynamically controlled by the defined theme.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/chart.mdx#_snippet_21

LANGUAGE: tsx
CODE:
```
<Bar dataKey="desktop" fill="var(--color-desktop)" />
```

----------------------------------------

TITLE: Wrapping Root Layout with ThemeProvider in React (TSX)
DESCRIPTION: This snippet demonstrates how to integrate the `ThemeProvider` component into the main application file, typically `App.tsx`. By wrapping the application's children with `ThemeProvider`, the theme context becomes available throughout the component tree. It also shows how to set a `defaultTheme` and a `storageKey` for theme persistence.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/dark-mode/vite.mdx#_snippet_1

LANGUAGE: tsx
CODE:
```
import { ThemeProvider } from "@/components/theme-provider"

function App() {
  return (
    <ThemeProvider defaultTheme="dark" storageKey="vite-ui-theme">
      {children}
    </ThemeProvider>
  )
}

export default App
```

----------------------------------------

TITLE: Implementing Basic Select Component (TypeScript/React)
DESCRIPTION: This example demonstrates the basic structure for rendering a shadcn/ui Select component. It includes a `SelectTrigger` with a placeholder, and `SelectContent` populated with `SelectItem` options for theme selection, providing a functional dropdown.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/select.mdx#_snippet_3

LANGUAGE: tsx
CODE:
```
<Select>
  <SelectTrigger className="w-[180px]">
    <SelectValue placeholder="Theme" />
  </SelectTrigger>
  <SelectContent>
    <SelectItem value="light">Light</SelectItem>
    <SelectItem value="dark">Dark</SelectItem>
    <SelectItem value="system">System</SelectItem>
  </SelectContent>
</Select>
```

----------------------------------------

TITLE: Basic Usage of Popover Component in TSX
DESCRIPTION: This example shows the fundamental structure for using the Popover component. It includes a PopoverTrigger to activate the popover and PopoverContent to display its rich content, demonstrating a basic interactive popover.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/popover.mdx#_snippet_3

LANGUAGE: tsx
CODE:
```
<Popover>
  <PopoverTrigger>Open</PopoverTrigger>
  <PopoverContent>Place content for the popover here.</PopoverContent>
</Popover>
```

----------------------------------------

TITLE: Implementing a User Dropdown in SidebarFooter in TSX
DESCRIPTION: This snippet demonstrates how to add a sticky footer to the sidebar using `SidebarFooter`, incorporating a `DropdownMenu` for user-related actions like account, billing, and sign out. It shows how to structure a `SidebarMenu` within the footer to provide contextual options at the bottom of the sidebar.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/sidebar.mdx#_snippet_21

LANGUAGE: tsx
CODE:
```
export function AppSidebar() {
  return (
    <SidebarProvider>
      <Sidebar>
        <SidebarHeader />
        <SidebarContent />
        <SidebarFooter>
          <SidebarMenu>
            <SidebarMenuItem>
              <DropdownMenu>
                <DropdownMenuTrigger asChild>
                  <SidebarMenuButton>
                    <User2 /> Username
                    <ChevronUp className="ml-auto" />
                  </SidebarMenuButton>
                </DropdownMenuTrigger>
                <DropdownMenuContent
                  side="top"
                  className="w-[--radix-popper-anchor-width]"
                >
                  <DropdownMenuItem>
                    <span>Account</span>
                  </DropdownMenuItem>
                  <DropdownMenuItem>
                    <span>Billing</span>
                  </DropdownMenuItem>
                  <DropdownMenuItem>
                    <span>Sign out</span>
                  </DropdownMenuItem>
                </DropdownMenuContent>
              </DropdownMenu>
            </SidebarMenuItem>
          </SidebarMenu>
        </SidebarFooter>
      </Sidebar>
    </SidebarProvider>
  )
}
```

----------------------------------------

TITLE: Persisting Sidebar State in Next.js with Cookies
DESCRIPTION: Demonstrates how to configure `SidebarProvider` in a Next.js `app/layout.tsx` to persist the sidebar's open/closed state across page reloads using cookies. It reads the `sidebar_state` cookie to set the initial `defaultOpen` prop.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/sidebar.mdx#_snippet_12

LANGUAGE: tsx
CODE:
```
import { cookies } from "next/headers"

import { SidebarProvider, SidebarTrigger } from "@/components/ui/sidebar"
import { AppSidebar } from "@/components/app-sidebar"

export async function Layout({ children }: { children: React.ReactNode }) {
  const cookieStore = await cookies()
  const defaultOpen = cookieStore.get("sidebar_state")?.value === "true"

  return (
    <SidebarProvider defaultOpen={defaultOpen}>
      <AppSidebar />
      <main>
        <SidebarTrigger />
        {children}
      </main>
    </SidebarProvider>
  )
}
```

----------------------------------------

TITLE: Configuring components.json for JavaScript Projects
DESCRIPTION: This JSON snippet illustrates how to configure the 'components.json' file to disable TypeScript support by setting 'tsx' to 'false'. It also defines other project-specific settings such as Tailwind configuration paths, base color, CSS variables, and import aliases.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/changelog.mdx#_snippet_4

LANGUAGE: json
CODE:
```
{
  "style": "default",
  "tailwind": {
    "config": "tailwind.config.js",
    "css": "src/app/globals.css",
    "baseColor": "zinc",
    "cssVariables": true
  },
  "rsc": false,
  "tsx": false,
  "aliases": {
    "utils": "~/lib/utils",
    "components": "~/components"
  }
}
```

----------------------------------------

TITLE: Rendering Skeleton Component (TypeScript/TSX)
DESCRIPTION: This snippet demonstrates how to render the `Skeleton` component with specific Tailwind CSS classes for width, height, and border-radius. These classes define the placeholder's visual appearance, mimicking a loading element.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/skeleton.mdx#_snippet_2

LANGUAGE: tsx
CODE:
```
<Skeleton className="w-[100px] h-[20px] rounded-full" />
```

----------------------------------------

TITLE: Using the useSidebar Hook in TypeScript
DESCRIPTION: This snippet demonstrates how to import and destructure properties from the `useSidebar` hook, which provides state and control functions for managing a sidebar's open/collapsed state, mobile responsiveness, and toggling behavior. It allows developers to access and manipulate the sidebar's visibility and state within a React component.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/sidebar.mdx#_snippet_19

LANGUAGE: tsx
CODE:
```
import { useSidebar } from "@/components/ui/sidebar"

export function AppSidebar() {
  const {
    state,
    open,
    setOpen,
    openMobile,
    setOpenMobile,
    isMobile,
    toggleSidebar,
  } = useSidebar()
}
```

----------------------------------------

TITLE: Creating Custom Sidebar Trigger with useSidebar Hook in React TSX
DESCRIPTION: This snippet shows how to build a custom sidebar toggle button using the `useSidebar` hook. It accesses the `toggleSidebar` function from the hook to programmatically control the sidebar's visibility.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/sidebar.mdx#_snippet_38

LANGUAGE: tsx
CODE:
```
import { useSidebar } from "@/components/ui/sidebar"

export function CustomTrigger() {
  const { toggleSidebar } = useSidebar()

  return <button onClick={toggleSidebar}>Toggle Sidebar</button>
}
```

----------------------------------------

TITLE: Running Initial Project Setup
DESCRIPTION: This command executes the `setup` script defined in the project's `package.json`. It typically handles post-installation tasks such as installing dependencies, setting up the database, or configuring environment variables, preparing the application for development or production use.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/packages/shadcn/test/fixtures/frameworks/remix-indie-stack/README.md#_snippet_2

LANGUAGE: sh
CODE:
```
npm run setup
```

----------------------------------------

TITLE: Applying Direct CSS Variable to Chart Configuration
DESCRIPTION: This TypeScript/React snippet illustrates how to reference a CSS variable directly in the `chartConfig`'s `color` property when the variable itself already contains a full color value (e.g., `oklch`, `hex`, `hsl`). This simplifies the color assignment compared to wrapping HSL values.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/chart.mdx#_snippet_19

LANGUAGE: tsx
CODE:
```
color: "var(--chart-1)",
```

----------------------------------------

TITLE: Applying CSS Variables to Chart Configuration (HSL)
DESCRIPTION: This TypeScript/React snippet demonstrates how to integrate CSS variables into the `chartConfig` object. The `color` property for each data series (e.g., 'desktop', 'mobile') references a CSS variable, wrapped in `hsl()` to convert the defined HSL values into a functional color for the chart.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/chart.mdx#_snippet_17

LANGUAGE: tsx
CODE:
```
const chartConfig = {
  desktop: {
    label: "Desktop",
    color: "hsl(var(--chart-1))",
  },
  mobile: {
    label: "Mobile",
    color: "hsl(var(--chart-2))",
  },
} satisfies ChartConfig
```

----------------------------------------

TITLE: Basic Navigation Menu Structure (TSX)
DESCRIPTION: Demonstrates the fundamental JSX structure for a NavigationMenu component, including NavigationMenuList, NavigationMenuItem, NavigationMenuTrigger, and NavigationMenuContent to create a simple navigation item.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/navigation-menu.mdx#_snippet_3

LANGUAGE: tsx
CODE:
```
<NavigationMenu>
  <NavigationMenuList>
    <NavigationMenuItem>
      <NavigationMenuTrigger>Item One</NavigationMenuTrigger>
      <NavigationMenuContent>
        <NavigationMenuLink>Link</NavigationMenuLink>
      </NavigationMenuContent>
    </NavigationMenuItem>
  </NavigationMenuList>
</NavigationMenu>
```

----------------------------------------

TITLE: Adding 'alert-dialog' Component (Bash)
DESCRIPTION: This is an example of using the `add` command to specifically add the 'alert-dialog' component to the project. It demonstrates the concrete syntax for adding a particular component by name.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/packages/shadcn/README.md#_snippet_2

LANGUAGE: bash
CODE:
```
npx shadcn add alert-dialog
```

----------------------------------------

TITLE: Adding a shadcn/ui Button Component
DESCRIPTION: This command uses the `shadcn` CLI to add the `Button` component to your project. It fetches the component's code and integrates it into your project's component directory, making it ready for use.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/installation/tanstack.mdx#_snippet_6

LANGUAGE: bash
CODE:
```
npx shadcn@canary add button
```

----------------------------------------

TITLE: Rendering Icon and Label in SidebarMenuButton (TSX)
DESCRIPTION: This snippet demonstrates how to include an icon and a truncated label within a `SidebarMenuButton`. It's important to wrap the label text in a `<span>` tag for proper rendering and truncation.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/sidebar.mdx#_snippet_28

LANGUAGE: tsx
CODE:
```
<SidebarMenuButton asChild>
  <a href="#">
    <Home />
    <span>Home</span>
  </a>
</SidebarMenuButton>
```

----------------------------------------

TITLE: Adding a Dropdown Menu to SidebarHeader in TSX
DESCRIPTION: This example illustrates how to integrate a `DropdownMenu` within the `SidebarHeader` component to create a sticky header with interactive elements. It uses `DropdownMenuTrigger` with `SidebarMenuButton` to allow users to select a workspace, showcasing how to combine UI components for enhanced sidebar functionality.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/sidebar.mdx#_snippet_20

LANGUAGE: tsx
CODE:
```
<Sidebar>
  <SidebarHeader>
    <SidebarMenu>
      <SidebarMenuItem>
        <DropdownMenu>
          <DropdownMenuTrigger asChild>
            <SidebarMenuButton>
              Select Workspace
              <ChevronDown className="ml-auto" />
            </SidebarMenuButton>
          </DropdownMenuTrigger>
          <DropdownMenuContent className="w-[--radix-popper-anchor-width]">
            <DropdownMenuItem>
              <span>Acme Inc</span>
            </DropdownMenuItem>
            <DropdownMenuItem>
              <span>Acme Corp.</span>
            </DropdownMenuItem>
          </DropdownMenuContent>
        </DropdownMenu>
      </SidebarMenuItem>
    </SidebarMenu>
  </SidebarHeader>
</Sidebar>
```

----------------------------------------

TITLE: Configuring Tailwind CSS Theming with CSS Variables (JSON)
DESCRIPTION: This setting controls whether components use CSS variables (`true`) or Tailwind CSS utility classes (`false`) for theming. This choice is permanent and requires re-installation of components to change.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components-json.mdx#_snippet_6

LANGUAGE: json
CODE:
```
{
  "tailwind": {
    "cssVariables": `true` | `false`
  }
}
```

----------------------------------------

TITLE: Displaying Selected Row Count in TSX
DESCRIPTION: This snippet demonstrates how to display the number of currently selected rows and the total number of filtered rows using the `table.getFilteredSelectedRowModel()` and `table.getFilteredRowModel()` APIs from React Table. It provides immediate feedback on row selection status.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/data-table.mdx#_snippet_16

LANGUAGE: TSX
CODE:
```
<div className="flex-1 text-sm text-muted-foreground">
  {table.getFilteredSelectedRowModel().rows.length} of{" "}
  {table.getFilteredRowModel().rows.length} row(s) selected.
</div>
```

----------------------------------------

TITLE: Configuring Hooks Import Alias (JSON)
DESCRIPTION: This alias defines the import path for custom hooks, like `use-media-query` or `use-toast`. It ensures that the CLI places and references these hooks according to your project's alias configurations.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components-json.mdx#_snippet_14

LANGUAGE: json
CODE:
```
{
  "aliases": {
    "hooks": "@/hooks"
  }
}
```

----------------------------------------

TITLE: Overriding Tailwind CSS v4 Theme Variables in shadcn/ui Registry (JSON)
DESCRIPTION: This JSON snippet demonstrates how to add or override Tailwind CSS v4 theme variables within a `shadcn/ui` registry item. It shows how to define custom values for properties like `text-base`, `ease-in-out`, and `font-heading` under the `cssVars.theme` object, allowing for granular control over design tokens.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/registry/faq.mdx#_snippet_3

LANGUAGE: json
CODE:
```
{
  "$schema": "https://ui.shadcn.com/schema/registry-item.json",
  "name": "hello-world",
  "title": "Hello World",
  "type": "registry:block",
  "description": "A complex hello world component",
  "files": [
    // ...
  ],
  "cssVars": {
    "theme": {
      "text-base": "3rem",
      "ease-in-out": "cubic-bezier(0.4, 0, 0.2, 1)",
      "font-heading": "Poppins, sans-serif"
    }
  }
}
```

----------------------------------------

TITLE: Specifying Tailwind CSS Config Path in components.json (JSON)
DESCRIPTION: This setting defines the path to your `tailwind.config.js` or `tailwind.config.ts` file, allowing the CLI to correctly integrate with your Tailwind CSS setup. It should be left blank for Tailwind CSS v4.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components-json.mdx#_snippet_3

LANGUAGE: json
CODE:
```
{
  "tailwind": {
    "config": "tailwind.config.js" | "tailwind.config.ts"
  }
}
```

----------------------------------------

TITLE: Listening to Carousel Events in React
DESCRIPTION: This example illustrates how to subscribe to carousel events, specifically the 'select' event, using the API instance obtained via `setApi`. The `React.useEffect` hook ensures the event listener is set up once the API is available, allowing custom logic to execute when a new slide is selected.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/carousel.mdx#_snippet_11

LANGUAGE: tsx
CODE:
```
import { type CarouselApi } from "@/components/ui/carousel"

export function Example() {
  const [api, setApi] = React.useState<CarouselApi>()

  React.useEffect(() => {
    if (!api) {
      return
    }

    api.on("select", () => {
      // Do something on select.
    })
  }, [api])

  return (
    <Carousel setApi={setApi}>
      <CarouselContent>
        <CarouselItem>...</CarouselItem>
        <CarouselItem>...</CarouselItem>
        <CarouselItem>...</CarouselItem>
      </CarouselContent>
    </Carousel>
  )
}
```

----------------------------------------

TITLE: Importing Switch Component (TypeScript)
DESCRIPTION: This snippet demonstrates how to import the `Switch` component from the `shadcn/ui` library. The import path `"@/components/ui/switch"` assumes a standard `shadcn/ui` project setup.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/switch.mdx#_snippet_2

LANGUAGE: tsx
CODE:
```
import { Switch } from "@/components/ui/switch"
```

----------------------------------------

TITLE: Importing Calendar Component (TypeScript/React)
DESCRIPTION: This line imports the `Calendar` component from the local UI components path, making it available for use within a TypeScript or React application.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/calendar.mdx#_snippet_2

LANGUAGE: tsx
CODE:
```
import { Calendar } from "@/components/ui/calendar"
```

----------------------------------------

TITLE: Importing Avatar Components for Usage (TypeScript/TSX)
DESCRIPTION: This TypeScript/TSX import statement makes the Avatar, AvatarFallback, and AvatarImage components available for use in a React application, typically from a local UI components library.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/avatar.mdx#_snippet_2

LANGUAGE: tsx
CODE:
```
import { Avatar, AvatarFallback, AvatarImage } from "@/components/ui/avatar"
```

----------------------------------------

TITLE: Importing Skeleton Component (TypeScript/TSX)
DESCRIPTION: This line imports the `Skeleton` component from the specified UI components path, making it available for use in your TSX file. Ensure the import path matches your project's structure.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/skeleton.mdx#_snippet_1

LANGUAGE: tsx
CODE:
```
import { Skeleton } from "@/components/ui/skeleton"
```

----------------------------------------

TITLE: Importing Alert Dialog Components in TSX
DESCRIPTION: This snippet demonstrates how to import the necessary components for the Alert Dialog from the local `ui/alert-dialog` path, making them available for use in a TypeScript React application.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/alert-dialog.mdx#_snippet_2

LANGUAGE: tsx
CODE:
```
import {
  AlertDialog,
  AlertDialogAction,
  AlertDialogCancel,
  AlertDialogContent,
  AlertDialogDescription,
  AlertDialogFooter,
  AlertDialogHeader,
  AlertDialogTitle,
  AlertDialogTrigger,
} from "@/components/ui/alert-dialog"
```

----------------------------------------

TITLE: Adding a shadcn/ui component (Button) to Astro
DESCRIPTION: Uses the `shadcn` CLI's `add` command to fetch and integrate a specific component, in this case, the `Button` component, into the project. This makes the component's code available locally for use.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/installation/astro.mdx#_snippet_3

LANGUAGE: bash
CODE:
```
npx shadcn@latest add button
```

----------------------------------------

TITLE: Installing Aspect Ratio Component with Shadcn CLI (Bash)
DESCRIPTION: This command utilizes the `shadcn/ui` CLI to automatically add the `AspectRatio` component and its dependencies to your project, streamlining the installation process.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/aspect-ratio.mdx#_snippet_0

LANGUAGE: bash
CODE:
```
npx shadcn@latest add aspect-ratio
```

----------------------------------------

TITLE: Installing Popover Component via shadcn/ui CLI
DESCRIPTION: This command uses the shadcn/ui CLI to automatically add the Popover component and its dependencies to your project. It's the recommended and simplest installation method for integrating the component.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/popover.mdx#_snippet_0

LANGUAGE: bash
CODE:
```
npx shadcn@latest add popover
```

----------------------------------------

TITLE: Installing Separator Component via shadcn/ui CLI (Bash)
DESCRIPTION: Uses the `shadcn/ui` CLI to automatically add the `Separator` component and its associated files to your project, simplifying the setup process.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/separator.mdx#_snippet_0

LANGUAGE: bash
CODE:
```
npx shadcn@latest add separator
```

----------------------------------------

TITLE: Installing Hover Card Component via CLI
DESCRIPTION: This command uses the shadcn/ui CLI to automatically add the Hover Card component and its dependencies to your project, simplifying the installation process.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/hover-card.mdx#_snippet_0

LANGUAGE: bash
CODE:
```
npx shadcn@latest add hover-card
```

----------------------------------------

TITLE: Installing Command Component via CLI
DESCRIPTION: This command uses the shadcn/ui CLI to automatically add the Command component and its dependencies to your project, simplifying the installation process.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/command.mdx#_snippet_0

LANGUAGE: bash
CODE:
```
npx shadcn@latest add command
```

----------------------------------------

TITLE: Installing Drawer Component via CLI (Bash)
DESCRIPTION: This command utilizes the shadcn/ui CLI to automatically add the Drawer component and its required dependencies to your project, simplifying the setup process.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/drawer.mdx#_snippet_0

LANGUAGE: bash
CODE:
```
npx shadcn@latest add drawer
```

----------------------------------------

TITLE: Installing Shadcn UI Progress Component via CLI (Bash)
DESCRIPTION: This command utilizes the `shadcn` CLI to automatically add the Progress component and its required dependencies to your project, streamlining the setup process by handling file generation and configuration.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/progress.mdx#_snippet_0

LANGUAGE: bash
CODE:
```
npx shadcn@latest add progress
```

----------------------------------------

TITLE: Installing Shadcn UI Form Component via CLI
DESCRIPTION: This command provides the simplest way to add the `shadcn/ui` form component to your project using the command-line interface. It automates the process of setting up the necessary files and configurations for the form components.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/form.mdx#_snippet_2

LANGUAGE: bash
CODE:
```
npx shadcn@latest add form
```

----------------------------------------

TITLE: Installing Tooltip Component via CLI (Bash)
DESCRIPTION: This command uses the `shadcn` CLI to automatically add the `Tooltip` component and its dependencies to the project, simplifying the setup process.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/tooltip.mdx#_snippet_0

LANGUAGE: bash
CODE:
```
npx shadcn@latest add tooltip
```

----------------------------------------

TITLE: Installing ScrollArea via CLI
DESCRIPTION: This command uses the shadcn/ui CLI to automatically add the ScrollArea component and its dependencies to your project, simplifying the setup process.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/scroll-area.mdx#_snippet_0

LANGUAGE: bash
CODE:
```
npx shadcn@latest add scroll-area
```

----------------------------------------

TITLE: Installing Alert Dialog Component via CLI
DESCRIPTION: This command uses the shadcn/ui CLI to automatically add the Alert Dialog component and its dependencies to your project, simplifying the installation process.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/alert-dialog.mdx#_snippet_0

LANGUAGE: bash
CODE:
```
npx shadcn@latest add alert-dialog
```

----------------------------------------

TITLE: Installing Textarea Component via CLI (Bash)
DESCRIPTION: This command utilizes `npx` to execute the `shadcn` CLI, automating the process of adding the `textarea` component to your project. It handles dependencies and file creation, streamlining the setup.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/textarea.mdx#_snippet_0

LANGUAGE: bash
CODE:
```
npx shadcn@latest add textarea
```

----------------------------------------

TITLE: Installing Toggle Group via shadcn/ui CLI
DESCRIPTION: This command uses the shadcn/ui CLI to automatically add the Toggle Group component and its dependencies to your project, simplifying the setup process.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/toggle-group.mdx#_snippet_0

LANGUAGE: bash
CODE:
```
npx shadcn@latest add toggle-group
```

----------------------------------------

TITLE: Serving the Registry with Next.js Development Server (Bash)
DESCRIPTION: This command starts the development server for a Next.js project. Once running, the generated registry JSON files will be served locally, typically accessible at `http://localhost:3000/r/[NAME].json`.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/registry/getting-started.mdx#_snippet_7

LANGUAGE: bash
CODE:
```
npm run dev
```

----------------------------------------

TITLE: Basic Collapsible Component Usage (TypeScript/TSX)
DESCRIPTION: Demonstrates a basic implementation of the Collapsible component, showing how to wrap content that expands and collapses when the trigger is clicked. The CollapsibleTrigger controls the visibility of the CollapsibleContent.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/collapsible.mdx#_snippet_3

LANGUAGE: tsx
CODE:
```
<Collapsible>
  <CollapsibleTrigger>Can I use this in my project?</CollapsibleTrigger>
  <CollapsibleContent>
    Yes. Free to use for personal and commercial projects. No attribution
    required.
  </CollapsibleContent>
</Collapsible>
```

----------------------------------------

TITLE: Creating Theme Change Action Route in Remix
DESCRIPTION: This TypeScript snippet defines an action route (`/routes/action.set-theme.ts`) that handles theme changes. It uses `createThemeAction` from `remix-themes` with the previously defined `themeSessionResolver` to store the user's preferred theme in the session storage, enabling theme persistence across requests.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/dark-mode/remix.mdx#_snippet_4

LANGUAGE: tsx
CODE:
```
import { createThemeAction } from "remix-themes"

import { themeSessionResolver } from "./sessions.server"

export const action = createThemeAction(themeSessionResolver)
```

----------------------------------------

TITLE: Basic Avatar Component Rendering Example (TSX)
DESCRIPTION: This TSX snippet demonstrates how to render a simple Avatar component, providing an image source and a two-character fallback that displays if the image fails to load.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/avatar.mdx#_snippet_3

LANGUAGE: tsx
CODE:
```
<Avatar>
  <AvatarImage src="https://github.com/shadcn.png" />
  <AvatarFallback>CN</AvatarFallback>
</Avatar>
```

----------------------------------------

TITLE: Manual Installation of Form Dependencies via npm
DESCRIPTION: This command lists the essential npm packages required for the `shadcn/ui` form components to function correctly. These dependencies include libraries for labels, slots, React Hook Form, its resolvers, and Zod for validation.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/form.mdx#_snippet_3

LANGUAGE: bash
CODE:
```
npm install @radix-ui/react-label @radix-ui/react-slot react-hook-form @hookform/resolvers zod
```

----------------------------------------

TITLE: Rendering Basic Sidebar Component in TSX
DESCRIPTION: Illustrates the fundamental usage of the `Sidebar` component by importing and rendering it within a functional React component. This sets up a basic collapsible sidebar.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/sidebar.mdx#_snippet_14

LANGUAGE: tsx
CODE:
```
import { Sidebar } from "@/components/ui/sidebar"

export function AppSidebar() {
  return <Sidebar />
}
```

----------------------------------------

TITLE: Adding ChartTooltip to BarChart (shadcn/ui, TypeScript)
DESCRIPTION: Integrates the `ChartTooltip` component into a `BarChart`, providing interactive tooltips when hovering over chart elements. It uses `ChartTooltipContent` to define the content displayed within the tooltip.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/chart.mdx#_snippet_12

LANGUAGE: tsx
CODE:
```
<ChartContainer config={chartConfig} className="h-[200px] w-full">
  <BarChart accessibilityLayer data={chartData}>
    <CartesianGrid vertical={false} />
    <XAxis
      dataKey="month"
      tickLine={false}
      tickMargin={10}
      axisLine={false}
      tickFormatter={(value) => value.slice(0, 3)}
    />
    <ChartTooltip content={<ChartTooltipContent />} />
    <Bar dataKey="desktop" fill="var(--color-desktop)" radius={4} />
    <Bar dataKey="mobile" fill="var(--color-mobile)" radius={4} />
  </BarChart>
</ChartContainer>
```

----------------------------------------

TITLE: Implementing Basic Input OTP Component
DESCRIPTION: This TypeScript/React example provides the basic structure for a 6-digit Input OTP component. It shows how to use `InputOTPGroup` to cluster slots and `InputOTPSeparator` for visual division, creating a standard OTP input field.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/input-otp.mdx#_snippet_4

LANGUAGE: tsx
CODE:
```
<InputOTP maxLength={6}>
  <InputOTPGroup>
    <InputOTPSlot index={0} />
    <InputOTPSlot index={1} />
    <InputOTPSlot index={2} />
  </InputOTPGroup>
  <InputOTPSeparator />
  <InputOTPGroup>
    <InputOTPSlot index={3} />
    <InputOTPSlot index={4} />
    <InputOTPSlot index={5} />
  </InputOTPGroup>
</InputOTP>
```

----------------------------------------

TITLE: Basic Hover Card Component Usage in TSX
DESCRIPTION: This example shows the fundamental structure for using the Hover Card component, where `HoverCardTrigger` defines the interactive element and `HoverCardContent` contains the content displayed on hover.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/hover-card.mdx#_snippet_3

LANGUAGE: tsx
CODE:
```
<HoverCard>
  <HoverCardTrigger>Hover</HoverCardTrigger>
  <HoverCardContent>
    The React Framework – created and maintained by @vercel.
  </HoverCardContent>
</HoverCard>
```

----------------------------------------

TITLE: Configuring shadcn/ui components.json for packages/ui (Tailwind CSS v4)
DESCRIPTION: This `components.json` configuration is tailored for a shared UI package (`packages/ui`) within a monorepo, utilizing Tailwind CSS v4. It specifies the `style`, `iconLibrary`, and `baseColor`, and importantly, leaves the `tailwind.config` path empty for v4 compatibility. Aliases are defined to correctly resolve paths for components, utilities, hooks, and libraries within the workspace.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/monorepo.mdx#_snippet_7

LANGUAGE: json
CODE:
```
{
  "$schema": "https://ui.shadcn.com/schema.json",
  "style": "new-york",
  "rsc": true,
  "tsx": true,
  "tailwind": {
    "config": "",
    "css": "src/styles/globals.css",
    "baseColor": "zinc",
    "cssVariables": true
  },
  "iconLibrary": "lucide",
  "aliases": {
    "components": "@workspace/ui/components",
    "utils": "@workspace/ui/lib/utils",
    "hooks": "@workspace/ui/hooks",
    "lib": "@workspace/ui/lib",
    "ui": "@workspace/ui/components"
  }
}
```

----------------------------------------

TITLE: Applying Theme Colors to Chart Data Objects
DESCRIPTION: This TypeScript/React snippet illustrates how to embed theme colors directly within the `chartData` array. Each data object can have a `fill` property that references a CSS variable, enabling individual data points or series to adopt colors from the defined theme.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/chart.mdx#_snippet_22

LANGUAGE: tsx
CODE:
```
const chartData = [
  { browser: "chrome", visitors: 275, fill: "var(--color-chrome)" },
  { browser: "safari", visitors: 200, fill: "var(--color-safari)" },
]
```

----------------------------------------

TITLE: Implementing Collapsible SidebarMenu (TSX)
DESCRIPTION: This example demonstrates how to make a `SidebarMenu` component collapsible. It involves wrapping the `SidebarMenu` and its `SidebarMenuSub` components within a `Collapsible` component, utilizing `CollapsibleTrigger` and `CollapsibleContent` for toggle functionality.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/sidebar.mdx#_snippet_33

LANGUAGE: tsx
CODE:
```
<SidebarMenu>
  <Collapsible defaultOpen className="group/collapsible">
    <SidebarMenuItem>
      <CollapsibleTrigger asChild>
        <SidebarMenuButton />
      </CollapsibleTrigger>
      <CollapsibleContent>
        <SidebarMenuSub>
          <SidebarMenuSubItem />
        </SidebarMenuSub>
      </CollapsibleContent>
    </SidebarMenuItem>
  </Collapsible>
</SidebarMenu>
```

----------------------------------------

TITLE: Importing and Using shadcn/ui Button Component (TSX)
DESCRIPTION: This TypeScript JSX (TSX) snippet demonstrates how to import the `Button` component from the `components/ui/button` path and render it within a Next.js functional component. It shows a basic usage of the component with 'Click me' as its children.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/installation/next.mdx#_snippet_2

LANGUAGE: tsx
CODE:
```
import { Button } from "@/components/ui/button"

export default function Home() {
  return (
    <div>
      <Button>Click me</Button>
    </div>
  )
}
```

----------------------------------------

TITLE: Updating Button Variants for Icon Styling in TSX
DESCRIPTION: This `diff` snippet illustrates a change to the `buttonVariants` definition, adding CSS classes to automatically style icons within the button, ensuring consistent sizing and preventing pointer events.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/button.mdx#_snippet_7

LANGUAGE: diff
CODE:
```
const buttonVariants = cva(
  "inline-flex ... gap-2 [&_svg]:pointer-events-none [&_svg]:size-4 [&_svg]:shrink-0"
)
```

----------------------------------------

TITLE: Importing Global Stylesheet in TanStack Start Root Route
DESCRIPTION: This TypeScript React (TSX) code snippet demonstrates how to import the global `app.css` stylesheet into the root route of a TanStack Start application. It uses a URL import for the CSS file and includes it as a stylesheet link in the document's head, ensuring global styles are applied.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/installation/tanstack.mdx#_snippet_3

LANGUAGE: tsx
CODE:
```
import type { ReactNode } from "react"
import { Outlet, createRootRoute } from "@tanstack/react-router"
import { Meta, Scripts } from "@tanstack/start"

import appCss from "@/styles/app.css?url"

export const Route = createRootRoute({
  head: () => ({
    meta: [
      {
        charSet: "utf-8",
      },
      {
        name: "viewport",
        content: "width=device-width, initial-scale=1",
      },
      {
        title: "TanStack Start Starter",
      },
    ],
    links: [
      {
        rel: "stylesheet",
        href: appCss,
      },
    ],
  }),
  component: RootComponent,
})
```

----------------------------------------

TITLE: Rendering Sidebar Menu Button as a Link in TSX
DESCRIPTION: This snippet demonstrates using the `asChild` prop with `SidebarMenuButton` to render it as an anchor (`<a>`) tag instead of its default button element. This allows the menu item to function as a navigation link, directing users to a specified URL.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/sidebar.mdx#_snippet_27

LANGUAGE: tsx
CODE:
```
<SidebarMenuButton asChild>
  <a href="#">Home</a>
</SidebarMenuButton>
```

----------------------------------------

TITLE: Committing Changes Before Component Update in Bash
DESCRIPTION: This snippet provides standard Git commands to stage all current changes and commit them. It is a crucial prerequisite before running CLI commands that might overwrite existing components, ensuring that any local modifications are saved.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/tailwind-v4.mdx#_snippet_10

LANGUAGE: Bash
CODE:
```
git add .
git commit -m "..."
```

----------------------------------------

TITLE: Configuring Components Import Alias (JSON)
DESCRIPTION: This alias defines the import path for general components in the project. It works in conjunction with your `tsconfig.json` or `jsconfig.json` to ensure components are placed and referenced correctly by the CLI.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components-json.mdx#_snippet_11

LANGUAGE: json
CODE:
```
{
  "aliases": {
    "components": "@/components"
  }
}
```

----------------------------------------

TITLE: Initializing shadcn/ui Monorepo (Bash)
DESCRIPTION: This command initializes a new shadcn/ui monorepo project using `pnpm dlx`. It sets up the basic project structure and configuration for a monorepo.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/templates/monorepo-next/README.md#_snippet_0

LANGUAGE: bash
CODE:
```
pnpm dlx shadcn@latest init
```

----------------------------------------

TITLE: Refactoring InputOTPSlot for Composition - TypeScript Diff
DESCRIPTION: This diff shows how to refactor the `InputOTPSlot` component in `input-otp.tsx` to adopt the composition pattern. It replaces `SlotProps` with `OTPInputContext`, allowing slot properties to be accessed via React's `useContext` hook, simplifying the component's interface.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/input-otp.mdx#_snippet_8

LANGUAGE: diff
CODE:
```
- import { OTPInput, SlotProps } from "input-otp"
+ import { OTPInput, OTPInputContext } from "input-otp"

 const InputOTPSlot = React.forwardRef<
   React.ElementRef<"div">,
-   SlotProps & React.ComponentPropsWithoutRef<"div">
-  >(({ char, hasFakeCaret, isActive, className, ...props }, ref) => {
+   React.ComponentPropsWithoutRef<"div"> & { index: number }
+  >(({ index, className, ...props }, ref) => {
+   const inputOTPContext = React.useContext(OTPInputContext)
+   const { char, hasFakeCaret, isActive } = inputOTPContext.slots[index]
```

----------------------------------------

TITLE: Configuring Tailwind Utility Classes in `components.json` (JSON)
DESCRIPTION: To enable theming with Tailwind utility classes, set the `tailwind.cssVariables` property to `false` in your `components.json` file. This configuration ensures that the CLI will automatically use utility classes when adding new components.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/changelog.mdx#_snippet_16

LANGUAGE: json
CODE:
```
{
  "tailwind": {
    "config": "tailwind.config.js",
    "css": "app/globals.css",
    "baseColor": "slate",
    "cssVariables": false
  }
}
```

----------------------------------------

TITLE: Configuring Tailwind CSS for Dark Mode
DESCRIPTION: This CSS snippet modifies the Tailwind CSS configuration to enable dark mode styling. By adding `:root[class~="dark"]`, it allows the `dark` class applied to the HTML element to activate dark mode styles, ensuring proper theme application.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/dark-mode/remix.mdx#_snippet_0

LANGUAGE: css
CODE:
```
.dark,
:root[class~="dark"] {
  ...;
}
```

----------------------------------------

TITLE: Migrating InputOTP Render Prop to Composition - JSX Diff
DESCRIPTION: This diff demonstrates how to migrate existing `InputOTP` usage from the `render` prop pattern to the new composition pattern. Instead of a render function, `InputOTPSlot` components are now directly nested within `InputOTPGroup` components, improving readability and maintainability.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/input-otp.mdx#_snippet_9

LANGUAGE: diff
CODE:
```
<InputOTP maxLength={6}>
  <InputOTPGroup>
    <InputOTPSlot index={0} />
    <InputOTPSlot index={1} />
    <InputOTPSlot index={2} />
  </InputOTPGroup>
  <InputOTPSeparator />
  <InputOTPGroup>
    <InputOTPSlot index={3} />
    <InputOTPSlot index={4} />
    <InputOTPSlot index={5} />
  </InputOTPGroup>
</InputOTP>
```

----------------------------------------

TITLE: Creating a Basic Sidebar Group in TSX
DESCRIPTION: This snippet demonstrates how to create a basic sidebar group using `SidebarGroup`, `SidebarGroupLabel`, `SidebarGroupAction`, and `SidebarGroupContent` components. It organizes content within a `Sidebar` and `SidebarContent` wrapper, providing a labeled section with an optional action button.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/sidebar.mdx#_snippet_23

LANGUAGE: tsx
CODE:
```
import { Sidebar, SidebarContent, SidebarGroup } from "@/components/ui/sidebar"

export function AppSidebar() {
  return (
    <Sidebar>
      <SidebarContent>
        <SidebarGroup>
          <SidebarGroupLabel>Application</SidebarGroupLabel>
          <SidebarGroupAction>
            <Plus /> <span className="sr-only">Add Project</span>
          </SidebarGroupAction>
          <SidebarGroupContent></SidebarGroupContent>
        </SidebarGroup>
      </SidebarContent>
    </Sidebar>
  )
}
```

----------------------------------------

TITLE: Adding an Action Button to Sidebar Group in TSX
DESCRIPTION: This snippet demonstrates the usage of `SidebarGroupAction` to add an interactive button to a `SidebarGroup`. The action button, titled 'Add Project', is placed alongside the group label, providing quick access to related functionality within the sidebar section.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/sidebar.mdx#_snippet_25

LANGUAGE: tsx
CODE:
```
export function AppSidebar() {
  return (
    <SidebarGroup>
      <SidebarGroupLabel asChild>Projects</SidebarGroupLabel>
      <SidebarGroupAction title="Add Project">
        <Plus /> <span className="sr-only">Add Project</span>
      </SidebarGroupAction>
      <SidebarGroupContent />
    </SidebarGroup>
  )
}
```

----------------------------------------

TITLE: Setting Sidebar Side Position in TSX
DESCRIPTION: Configures the `Sidebar` component to appear on either the `left` or `right` side of the application layout. This prop controls the alignment of the sidebar.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/sidebar.mdx#_snippet_15

LANGUAGE: tsx
CODE:
```
import { Sidebar } from "@/components/ui/sidebar"

export function AppSidebar() {
  return <Sidebar side="left | right" />
}
```

----------------------------------------

TITLE: Defining CSS Variables for Chart Theming
DESCRIPTION: This CSS snippet defines custom CSS variables for chart colors within a `:root` scope for light mode and a `.dark` scope for dark mode. These variables, such as `--chart-1` and `--chart-2`, are intended to be referenced by chart components to ensure consistent theming across the application.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/chart.mdx#_snippet_16

LANGUAGE: css
CODE:
```
@layer base {
  :root {
    --background: 0 0% 100%;
    --foreground: 240 10% 3.9%;
    // ...
    --chart-1: 12 76% 61%;
    --chart-2: 173 58% 39%;
  }

  .dark: {
    --background: 240 10% 3.9%;
    --foreground: 0 0% 100%;
    // ...
    --chart-1: 220 70% 50%;
    --chart-2: 160 60% 45%;
  }
}
```

----------------------------------------

TITLE: Setting Responsive Carousel Item Spacing
DESCRIPTION: Illustrates how to apply responsive spacing to carousel items, using smaller spacing on small screens (`-ml-2`, `pl-2`) and larger spacing on medium screens and up (`md:-ml-4`, `md:pl-4`).
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/carousel.mdx#_snippet_7

LANGUAGE: tsx
CODE:
```
<Carousel>
  <CarouselContent className="-ml-2 md:-ml-4">
    <CarouselItem className="pl-2 md:pl-4">...</CarouselItem>
    <CarouselItem className="pl-2 md:pl-4">...</CarouselItem>
    <CarouselItem className="pl-2 md:pl-4">...</CarouselItem>
  </CarouselContent>
</Carousel>
```

----------------------------------------

TITLE: Implementing Basic Chart Tooltip
DESCRIPTION: This TypeScript/React snippet demonstrates the basic implementation of a chart tooltip using the `ChartTooltip` and `ChartTooltipContent` components. The `content` prop of `ChartTooltip` is set to an instance of `ChartTooltipContent`, providing a default, functional tooltip display.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/chart.mdx#_snippet_25

LANGUAGE: tsx
CODE:
```
<ChartTooltip content={<ChartTooltipContent />} />
```

----------------------------------------

TITLE: Installing Input OTP via CLI
DESCRIPTION: This command demonstrates how to add the `input-otp` component to your project using the shadcn/ui CLI. It automates the setup process, including adding necessary files and dependencies.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/input-otp.mdx#_snippet_0

LANGUAGE: bash
CODE:
```
npx shadcn@latest add input-otp
```

----------------------------------------

TITLE: Installing Pagination Component via CLI
DESCRIPTION: This snippet demonstrates how to quickly add the shadcn-ui Pagination component to your project using the command-line interface (CLI). It simplifies the setup process by automating the necessary file additions.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/pagination.mdx#_snippet_0

LANGUAGE: bash
CODE:
```
npx shadcn@latest add pagination
```

----------------------------------------

TITLE: Installing Slider Component via shadcn/ui CLI
DESCRIPTION: This command uses the shadcn/ui CLI to automatically add the Slider component and its dependencies to your project, simplifying the setup process.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/slider.mdx#_snippet_0

LANGUAGE: bash
CODE:
```
npx shadcn@latest add slider
```

----------------------------------------

TITLE: Installing Toast Component via CLI
DESCRIPTION: This command uses the shadcn CLI to automatically add the Toast component and its dependencies to your project, simplifying the installation process.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/toast.mdx#_snippet_0

LANGUAGE: bash
CODE:
```
npx shadcn@latest add toast
```

----------------------------------------

TITLE: Installing Sheet Component via CLI (Bash)
DESCRIPTION: This snippet demonstrates how to quickly install the Shadcn UI Sheet component using the `npx shadcn@latest add` command-line interface. This method automates the setup process, including adding necessary files and dependencies.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/sheet.mdx#_snippet_0

LANGUAGE: bash
CODE:
```
npx shadcn@latest add sheet
```

----------------------------------------

TITLE: Installing Breadcrumb Component with CLI
DESCRIPTION: This snippet demonstrates how to install the `Breadcrumb` component using the `shadcn/ui` CLI. It's the recommended and simplest way to add the component to your project, automating dependency management and file setup.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/breadcrumb.mdx#_snippet_0

LANGUAGE: bash
CODE:
```
npx shadcn@latest add breadcrumb
```

----------------------------------------

TITLE: Installing Collapsible Component via CLI (Bash)
DESCRIPTION: Installs the Collapsible UI component using the shadcn/ui CLI, adding it to your project with a single command. This method simplifies the setup process by automating dependency installation and file copying.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/collapsible.mdx#_snippet_0

LANGUAGE: bash
CODE:
```
npx shadcn@latest add collapsible
```

----------------------------------------

TITLE: Updating CSS Variables with @theme inline for HSL Wrappers in CSS
DESCRIPTION: This snippet demonstrates the updated approach for CSS variables. It moves `:root` and `.dark` selectors out of `@layer base`, wraps the color values directly in `hsl()` within these selectors, and then uses `@theme inline` to reference these variables without additional `hsl()` wrappers, simplifying color access.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/tailwind-v4.mdx#_snippet_3

LANGUAGE: CSS
CODE:
```
:root {
  --background: hsl(0 0% 100%); // <-- Wrap in hsl
  --foreground: hsl(0 0% 3.9%);
}

.dark {
  --background: hsl(0 0% 3.9%); // <-- Wrap in hsl
  --foreground: hsl(0 0% 98%);
}

@theme inline {
  --color-background: var(--background); // <-- Remove hsl
  --color-foreground: var(--foreground);
}
```

----------------------------------------

TITLE: Installing Input OTP Dependencies Manually
DESCRIPTION: This command illustrates how to manually install the core `input-otp` package using npm. This step is a prerequisite for integrating the component into your project without using the shadcn/ui CLI.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/input-otp.mdx#_snippet_2

LANGUAGE: bash
CODE:
```
npm install input-otp
```

----------------------------------------

TITLE: Installing Radix UI Tabs Dependency Manually (Bash)
DESCRIPTION: This command installs the `@radix-ui/react-tabs` package, which is a core dependency for the Shadcn UI Tabs component when performing a manual installation.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/tabs.mdx#_snippet_1

LANGUAGE: bash
CODE:
```
npm install @radix-ui/react-tabs
```

----------------------------------------

TITLE: Installing Radix Aspect Ratio Dependency (Bash)
DESCRIPTION: Installs the `@radix-ui/react-aspect-ratio` package, which is the foundational dependency for the `AspectRatio` component when performing a manual setup.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/aspect-ratio.mdx#_snippet_1

LANGUAGE: bash
CODE:
```
npm install @radix-ui/react-aspect-ratio
```

----------------------------------------

TITLE: Installing Dropdown Menu Manual Dependencies
DESCRIPTION: This command installs the core `@radix-ui/react-dropdown-menu` dependency, which is required for the manual setup of the Shadcn UI Dropdown Menu component.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/dropdown-menu.mdx#_snippet_1

LANGUAGE: bash
CODE:
```
npm install @radix-ui/react-dropdown-menu
```

----------------------------------------

TITLE: Integrating SidebarRail Component in React TSX
DESCRIPTION: This snippet demonstrates the placement of the `SidebarRail` component within a `Sidebar`. The rail typically provides an area for additional controls or a dedicated space to toggle the sidebar.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/sidebar.mdx#_snippet_39

LANGUAGE: tsx
CODE:
```
<Sidebar>
  <SidebarHeader />
  <SidebarContent>
    <SidebarGroup />
  </SidebarContent>
  <SidebarFooter />
  <SidebarRail />
</Sidebar>
```

----------------------------------------

TITLE: Adding Separators to Input OTP Groups
DESCRIPTION: This TypeScript/React example demonstrates how to effectively use the `<InputOTPSeparator />` component to add visual dividers between groups of input slots. This enhances the readability and user experience for multi-part OTPs.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/input-otp.mdx#_snippet_6

LANGUAGE: tsx
CODE:
```
import {
  InputOTP,
  InputOTPGroup,
  InputOTPSeparator,
  InputOTPSlot
} from "@/components/ui/input-otp"

...

<InputOTP maxLength={4}>
  <InputOTPGroup>
    <InputOTPSlot index={0} />
    <InputOTPSlot index={1} />
  </InputOTPGroup>
  <InputOTPSeparator />
  <InputOTPGroup>
    <InputOTPSlot index={2} />
    <InputOTPSlot index={3} />
  </InputOTPGroup>
</InputOTP>
```

----------------------------------------

TITLE: Importing buttonVariants Helper for Links in TSX
DESCRIPTION: This import statement provides access to the `buttonVariants` helper function, which can be used to style a standard HTML `Link` element to visually resemble a button.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/button.mdx#_snippet_4

LANGUAGE: tsx
CODE:
```
import { buttonVariants } from "@/components/ui/button"
```

----------------------------------------

TITLE: Passing Options to Carousel
DESCRIPTION: Demonstrates how to pass configuration options to the underlying Embla Carousel instance using the `opts` prop, such as setting alignment to 'start' and enabling looping behavior.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/carousel.mdx#_snippet_9

LANGUAGE: tsx
CODE:
```
<Carousel
  opts={{
    align: "start",
    loop: true,
  }}
>
  <CarouselContent>
    <CarouselItem>...</CarouselItem>
    <CarouselItem>...</CarouselItem>
    <CarouselItem>...</CarouselItem>
  </CarouselContent>
</Carousel>
```

----------------------------------------

TITLE: Adding XAxis Component to BarChart (Recharts, TypeScript)
DESCRIPTION: Adds the `XAxis` component to a `BarChart` to display the x-axis. It configures the axis to use `month` as the data key, disables tick lines and axis lines, sets a tick margin, and formats tick values to show only the first three characters.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/chart.mdx#_snippet_10

LANGUAGE: tsx
CODE:
```
<ChartContainer config={chartConfig} className="h-[200px] w-full">
  <BarChart accessibilityLayer data={chartData}>
    <CartesianGrid vertical={false} />
    <XAxis
      dataKey="month"
      tickLine={false}
      tickMargin={10}
      axisLine={false}
      tickFormatter={(value) => value.slice(0, 3)}
    />
    <Bar dataKey="desktop" fill="var(--color-desktop)" radius={4} />
    <Bar dataKey="mobile" fill="var(--color-mobile)" radius={4} />
  </BarChart>
</ChartContainer>
```

----------------------------------------

TITLE: Adding Custom Brand Colors to Style (JSON)
DESCRIPTION: This JSON configuration defines a `shadcn/ui` style that adds a custom `brand` color to the default framework. It specifies the `brand` color using `oklch` format for both light and dark modes, allowing for specific color overrides while retaining other default styles. This is useful for integrating brand-specific colors.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/registry/examples.mdx#_snippet_3

LANGUAGE: json
CODE:
```
{
  "$schema": "https://ui.shadcn.com/schema/registry-item.json",
  "name": "custom-style",
  "type": "registry:style",
  "cssVars": {
    "light": {
      "brand": "oklch(0.99 0.00 0)"
    },
    "dark": {
      "brand": "oklch(0.14 0.00 286)"
    }
  }
}
```

----------------------------------------

TITLE: Basic Usage of Toggle Component in TypeScript/React
DESCRIPTION: Renders a basic `Toggle` component with the text 'Toggle' as its children. This demonstrates the minimal syntax required to display the component in a React application.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/toggle.mdx#_snippet_3

LANGUAGE: tsx
CODE:
```
<Toggle>Toggle</Toggle>
```

----------------------------------------

TITLE: Structuring Sidebar Content with SidebarGroup in TSX
DESCRIPTION: This example illustrates the basic usage of the `SidebarContent` component, which serves as a scrollable wrapper for the main content of the sidebar. It demonstrates how to include multiple `SidebarGroup` components within `SidebarContent` to organize different sections of the sidebar's navigation or information.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/sidebar.mdx#_snippet_22

LANGUAGE: tsx
CODE:
```
import { Sidebar, SidebarContent } from "@/components/ui/sidebar"

export function AppSidebar() {
  return (
    <Sidebar>
      <SidebarContent>
        <SidebarGroup />
        <SidebarGroup />
      </SidebarContent>
    </Sidebar>
  )
}
```

----------------------------------------

TITLE: Configuring Component Style in components.json (JSON)
DESCRIPTION: This configuration sets the visual style for components, with 'new-york' being the recommended option. This setting is permanent and cannot be altered after the initial project setup.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components-json.mdx#_snippet_2

LANGUAGE: json
CODE:
```
{
  "style": "new-york"
}
```

----------------------------------------

TITLE: Updating DropdownMenuSubTrigger Styling for Icons in TypeScript
DESCRIPTION: This snippet shows the integration of `gap-2 [&_svg]:pointer-events-none [&_svg]:size-4 [&_svg]:shrink-0` classes into the `DropdownMenuSubTrigger` component. This modification automatically applies styling to icons within sub-triggers, improving visual consistency.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/dropdown-menu.mdx#_snippet_5

LANGUAGE: tsx
CODE:
```
<DropdownMenuPrimitive.SubTrigger
  ref={ref}
  className={cn(
    "flex cursor-default gap-2 select-none items-center rounded-sm px-2 py-1.5 text-sm outline-none focus:bg-accent data-[state=open]:bg-accent [&_svg]:pointer-events-none [&_svg]:size-4 [&_svg]:shrink-0",
    inset && "pl-8",
    className
  )}
  {...props}
>
  {/* ... */}
</DropdownMenuPrimitive.SubTrigger>
```

----------------------------------------

TITLE: Configuring shadcn/ui components.json for apps/web (Tailwind CSS v3)
DESCRIPTION: This `components.json` configuration is for a web application (`apps/web`) using Tailwind CSS v3. It includes the schema, style, RSC, and TSX settings, and explicitly points to the `tailwind.config.ts` file located in the shared UI package. Aliases are set up to correctly import components, hooks, and utilities from both the application's local directories and the workspace's shared UI package.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/monorepo.mdx#_snippet_8

LANGUAGE: json
CODE:
```
{
  "$schema": "https://ui.shadcn.com/schema.json",
  "style": "new-york",
  "rsc": true,
  "tsx": true,
  "tailwind": {
    "config": "../../packages/ui/tailwind.config.ts",
    "css": "../../packages/ui/src/styles/globals.css",
    "baseColor": "zinc",
    "cssVariables": true
  },
  "iconLibrary": "lucide",
  "aliases": {
    "components": "@/components",
    "hooks": "@/hooks",
    "lib": "@/lib",
    "utils": "@workspace/ui/lib/utils",
    "ui": "@workspace/ui/components"
  }
}
```

----------------------------------------

TITLE: Importing and using shadcn/ui Button in an Astro page
DESCRIPTION: Demonstrates how to import the `Button` component from the `@/components/ui/button` path within an Astro page. It then shows how to render the component in the HTML body, illustrating its basic usage.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/installation/astro.mdx#_snippet_4

LANGUAGE: astro
CODE:
```
---
import { Button } from "@/components/ui/button"
---

<html lang="en">
	<head>
		<meta charset="utf-8" />
		<meta name="viewport" content="width=device-width" />
		<link rel="icon" type="image/svg+xml" href="/favicon.svg" />
		<meta name="generator" content={Astro.generator} />
		<title>Astro + TailwindCSS</title>
	</head>

	<body>
		<div class="grid place-items-center h-screen content-center">
			<Button>Button</Button>
		</div>
	</body>
</html>
```

----------------------------------------

TITLE: Adding Tailwind CSS v4 Colors to shadcn/ui Registry (JSON)
DESCRIPTION: This JSON snippet demonstrates how to extend Tailwind CSS v4 colors within a `shadcn/ui` registry item. It shows the addition of `brand-background` and `brand-accent` variables under the `cssVars.light` and `cssVars.dark` keys, which the CLI uses to update the project's CSS file, making new utility classes available.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/registry/faq.mdx#_snippet_1

LANGUAGE: json
CODE:
```
{
  "$schema": "https://ui.shadcn.com/schema/registry-item.json",
  "name": "hello-world",
  "title": "Hello World",
  "type": "registry:block",
  "description": "A complex hello world component",
  "files": [
    // ...
  ],
  "cssVars": {
    "light": {
      "brand-background": "20 14.3% 4.1%",
      "brand-accent": "20 14.3% 4.1%"
    },
    "dark": {
      "brand-background": "20 14.3% 4.1%",
      "brand-accent": "20 14.3% 4.1%"
    }
  }
}
```

----------------------------------------

TITLE: Configuring Sidebar Collapsible Behavior in TSX
DESCRIPTION: Enables and defines the collapsing behavior of the `Sidebar` component. Options include `offcanvas` (slides in/out), `icon` (collapses to icons), and `none` (non-collapsible).
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/sidebar.mdx#_snippet_18

LANGUAGE: tsx
CODE:
```
import { Sidebar } from "@/components/ui/sidebar"

export function AppSidebar() {
  return <Sidebar collapsible="offcanvas | icon | none" />
}
```

----------------------------------------

TITLE: Listing Available shadcn Components (Bash)
DESCRIPTION: Running the `add` command without any arguments displays a comprehensive list of all available shadcn components that can be added to the project. This is useful for discovering and exploring component options.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/packages/shadcn/README.md#_snippet_3

LANGUAGE: bash
CODE:
```
npx shadcn add
```

----------------------------------------

TITLE: Installing Tailwind CSS Dependencies
DESCRIPTION: This command installs Tailwind CSS along with its PostCSS plugin and PostCSS itself, which are essential dependencies for processing CSS with Tailwind in a TanStack Start project.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/installation/tanstack.mdx#_snippet_0

LANGUAGE: bash
CODE:
```
npm install tailwindcss @tailwindcss/postcss postcss
```

----------------------------------------

TITLE: Importing ChartTooltip Components (shadcn/ui, TypeScript)
DESCRIPTION: Imports `ChartTooltip` and `ChartTooltipContent` from `@/components/ui/chart`. These components are custom UI elements designed to provide interactive tooltips for charts, enhancing user experience on hover.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/chart.mdx#_snippet_11

LANGUAGE: tsx
CODE:
```
import { ChartTooltip, ChartTooltipContent } from "@/components/ui/chart"
```

----------------------------------------

TITLE: Importing Tailwind CSS into Application Stylesheet
DESCRIPTION: This CSS snippet imports the Tailwind CSS framework into the `app.css` file, making Tailwind's utility classes available throughout the application. The `source("../")` directive helps resolve the path to the Tailwind CSS source.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/installation/tanstack.mdx#_snippet_2

LANGUAGE: css
CODE:
```
@import "tailwindcss" source("../");
```

----------------------------------------

TITLE: Installing remix-themes Package
DESCRIPTION: This command installs the `remix-themes` package, which is a crucial dependency for implementing theme management, including dark mode, in a Remix application. It's the first step in setting up the theme functionality.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/dark-mode/remix.mdx#_snippet_1

LANGUAGE: bash
CODE:
```
npm install remix-themes
```

----------------------------------------

TITLE: After: React Component without `forwardRef` in TSX
DESCRIPTION: This snippet presents the `AccordionItem` component after migrating from `forwardRef`. It is converted to a named function, uses `React.ComponentProps` for type safety, and removes the explicit ref passing, instead adding a `data-slot` attribute for styling purposes.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/tailwind-v4.mdx#_snippet_8

LANGUAGE: TSX
CODE:
```
function AccordionItem({
  className,
  ...props
}: React.ComponentProps<typeof AccordionPrimitive.Item>) {
  return (
    <AccordionPrimitive.Item
      data-slot="accordion-item"
      className={cn("border-b last:border-b-0", className)}
      {...props}
    />
  )
}
```

----------------------------------------

TITLE: Importing Textarea Component (TypeScript/TSX)
DESCRIPTION: This line imports the `Textarea` component from the specified relative path, making it available for use within your React or Next.js application. Ensure that the import path correctly reflects your project's file structure.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/textarea.mdx#_snippet_1

LANGUAGE: tsx
CODE:
```
import { Textarea } from "@/components/ui/textarea"
```

----------------------------------------

TITLE: Importing Toast Function for Usage (TypeScript/React)
DESCRIPTION: This snippet imports the `toast` function from the `sonner` library. This is the primary function used to trigger and display toast notifications within a React or Next.js component.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/sonner.mdx#_snippet_4

LANGUAGE: tsx
CODE:
```
import { toast } from "sonner"
```

----------------------------------------

TITLE: Importing Collapsible Components (TypeScript/TSX)
DESCRIPTION: Imports the necessary Collapsible, CollapsibleContent, and CollapsibleTrigger components from the local UI library for use in a React/Next.js project. These imports are essential to utilize the component's functionality.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/collapsible.mdx#_snippet_2

LANGUAGE: tsx
CODE:
```
import {
  Collapsible,
  CollapsibleContent,
  CollapsibleTrigger,
} from "@/components/ui/collapsible"
```

----------------------------------------

TITLE: Importing Popover Components in TypeScript/React
DESCRIPTION: This snippet demonstrates how to import the necessary Popover components (Popover, PopoverContent, PopoverTrigger) from the project's UI library. These imports are essential for constructing and utilizing a popover in a React application.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/popover.mdx#_snippet_2

LANGUAGE: tsx
CODE:
```
import {
  Popover,
  PopoverContent,
  PopoverTrigger,
} from "@/components/ui/popover"
```

----------------------------------------

TITLE: Importing Chart Tooltip Components
DESCRIPTION: This TypeScript/React snippet shows the necessary import statement for using the `ChartTooltip` and `ChartTooltipContent` components. These components are essential for adding custom and configurable tooltips to charts in a Shadcn UI application.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/chart.mdx#_snippet_24

LANGUAGE: tsx
CODE:
```
import { ChartTooltip, ChartTooltipContent } from "@/components/ui/chart"
```

----------------------------------------

TITLE: Importing Radio Group and Label Components (TypeScript/TSX)
DESCRIPTION: This snippet shows the necessary import statements for bringing the Label, RadioGroup, and RadioGroupItem components into a TypeScript/TSX file, enabling their use in your React application.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/radio-group.mdx#_snippet_2

LANGUAGE: tsx
CODE:
```
import { Label } from "@/components/ui/label"
import { RadioGroup, RadioGroupItem } from "@/components/ui/radio-group"
```

----------------------------------------

TITLE: Initializing shadcn/ui in a Gatsby project
DESCRIPTION: Executes the `shadcn` init command to set up the `shadcn/ui` library within the Gatsby project, preparing it for component integration.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/installation/gatsby.mdx#_snippet_4

LANGUAGE: bash
CODE:
```
npx shadcn@latest init
```

----------------------------------------

TITLE: Creating a new Remix project
DESCRIPTION: Initializes a new Remix application using the `create-remix` CLI tool. This command sets up the basic project structure for your Remix application.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/installation/remix.mdx#_snippet_0

LANGUAGE: bash
CODE:
```
npx create-remix@latest my-app
```

----------------------------------------

TITLE: Downgrading React and React DOM to Version 18
DESCRIPTION: This command demonstrates how to downgrade `react` and `react-dom` to version 18. This serves as an alternative solution for resolving peer dependency issues when a package is not yet compatible with React 19, ensuring immediate functionality.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/react-19.mdx#_snippet_3

LANGUAGE: bash
CODE:
```
npm i react@18 react-dom@18
```

----------------------------------------

TITLE: Importing shadcn/ui Hooks and Utilities (TypeScript/TSX)
DESCRIPTION: This snippet illustrates importing hooks (e.g., 'useTheme') and utility functions (e.g., 'cn') from the '@workspace/ui' package. These shared functionalities are typically found in the 'hooks' and 'lib' directories, respectively.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/monorepo.mdx#_snippet_5

LANGUAGE: tsx
CODE:
```
import { useTheme } from "@workspace/ui/hooks/use-theme"
import { cn } from "@workspace/ui/lib/utils"
```

----------------------------------------

TITLE: Initializing shadcn/ui in TanStack Start Project
DESCRIPTION: This command runs the `shadcn` CLI's `init` command, which sets up the necessary configuration files (like `components.json`) and integrates CSS variables into your `app/styles/app.css` for shadcn/ui components.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/installation/tanstack.mdx#_snippet_5

LANGUAGE: bash
CODE:
```
npx shadcn@canary init
```

----------------------------------------

TITLE: Importing Resizable Components (TypeScript/React)
DESCRIPTION: This snippet demonstrates how to import the necessary `ResizableHandle`, `ResizablePanel`, and `ResizablePanelGroup` components from the project's UI library. These components are essential for constructing resizable layouts within a React application.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/resizable.mdx#_snippet_2

LANGUAGE: tsx
CODE:
```
import {
  ResizableHandle,
  ResizablePanel,
  ResizablePanelGroup,
} from "@/components/ui/resizable"
```

----------------------------------------

TITLE: Installing Radix UI Hover Card Dependency Manually
DESCRIPTION: This command installs the core `@radix-ui/react-hover-card` package, which is a prerequisite for the shadcn/ui Hover Card component when performing a manual installation.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/hover-card.mdx#_snippet_1

LANGUAGE: bash
CODE:
```
npm install @radix-ui/react-hover-card
```

----------------------------------------

TITLE: Installing Recharts Dependency Manually
DESCRIPTION: This command installs the core `recharts` library, which is a fundamental dependency for shadcn/ui charts. This step is necessary when opting for a manual installation of the chart components.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/chart.mdx#_snippet_3

LANGUAGE: bash
CODE:
```
npm install recharts
```

----------------------------------------

TITLE: Defining Sidebar Keyboard Shortcut in TSX
DESCRIPTION: Sets the single character that, when combined with `cmd` (Mac) or `ctrl` (Windows), triggers the opening and closing of the sidebar. The default shortcut is `cmd/ctrl + b`.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/sidebar.mdx#_snippet_11

LANGUAGE: tsx
CODE:
```
const SIDEBAR_KEYBOARD_SHORTCUT = "b"
```

----------------------------------------

TITLE: Installing Embla Carousel React Dependency
DESCRIPTION: This command installs the core 'embla-carousel-react' library, which is a prerequisite for the shadcn/ui Carousel component's functionality.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/carousel.mdx#_snippet_1

LANGUAGE: bash
CODE:
```
npm install embla-carousel-react
```

----------------------------------------

TITLE: Rendering DropdownMenu within SidebarMenuAction (TSX)
DESCRIPTION: This example demonstrates how to integrate a `DropdownMenu` component, triggered by a `SidebarMenuAction`, to display additional menu options. The `DropdownMenuTrigger` is set `asChild` to use the `SidebarMenuAction` as its trigger.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/sidebar.mdx#_snippet_31

LANGUAGE: tsx
CODE:
```
<SidebarMenuItem>
  <SidebarMenuButton asChild>
    <a href="#">
      <Home />
      <span>Home</span>
    </a>
  </SidebarMenuButton>
  <DropdownMenu>
    <DropdownMenuTrigger asChild>
      <SidebarMenuAction>
        <MoreHorizontal />
      </SidebarMenuAction>
    </DropdownMenuTrigger>
    <DropdownMenuContent side="right" align="start">
      <DropdownMenuItem>
        <span>Edit Project</span>
      </DropdownMenuItem>
      <DropdownMenuItem>
        <span>Delete Project</span>
      </DropdownMenuItem>
    </DropdownMenuContent>
  </DropdownMenu>
</SidebarMenuItem>
```

----------------------------------------

TITLE: Importing and Using shadcn/ui Button in TanStack Router (TSX)
DESCRIPTION: This TypeScript JSX snippet demonstrates how to import and use the `Button` component from shadcn/ui within a TanStack Router route file. It defines a route for the root path ('/') and renders the `Button` component inside the `App` function, showcasing its basic usage.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/installation/tanstack-router.mdx#_snippet_2

LANGUAGE: tsx
CODE:
```
import { createFileRoute } from "@tanstack/react-router"

import { Button } from "@/components/ui/button"

export const Route = createFileRoute("/")({
  component: App,
})

function App() {
  return (
    <div>
      <Button>Click me</Button>
    </div>
  )
}
```

----------------------------------------

TITLE: Defining Custom Style from Scratch (JSON)
DESCRIPTION: This JSON configuration defines a custom `shadcn/ui` style without extending the default framework (`extends: none`). It includes dependencies (`tailwind-merge`, `clsx`), adds `utils` and other components from a remote registry, and defines a comprehensive set of custom CSS variables (`main`, `bg`, `border`, `text`, `ring`) for both light and dark themes. This style can be used to build new components and themes from scratch.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/registry/examples.mdx#_snippet_1

LANGUAGE: json
CODE:
```
{
  "$schema": "https://ui.shadcn.com/schema/registry-item.json",
  "extends": "none",
  "name": "new-style",
  "type": "registry:style",
  "dependencies": ["tailwind-merge", "clsx"],
  "registryDependencies": [
    "utils",
    "https://example.com/r/button.json",
    "https://example.com/r/input.json",
    "https://example.com/r/label.json",
    "https://example.com/r/select.json"
  ],
  "cssVars": {
    "theme": {
      "font-sans": "Inter, sans-serif"
    },
    "light": {
      "main": "#88aaee",
      "bg": "#dfe5f2",
      "border": "#000",
      "text": "#000",
      "ring": "#000"
    },
    "dark": {
      "main": "#88aaee",
      "bg": "#272933",
      "border": "#000",
      "text": "#e6e6e6",
      "ring": "#fff"
    }
  }
}
```

----------------------------------------

TITLE: Adding Icon Size to shadcn/ui Button Component (TypeScript)
DESCRIPTION: This TypeScript snippet demonstrates how to extend the `buttonVariants` definition using `cva` to include an `icon` size. This addition provides a specific square variant for buttons intended to contain only an icon, ensuring consistent sizing and styling within the UI library.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/changelog.mdx#_snippet_23

LANGUAGE: tsx
CODE:
```
const buttonVariants = cva({
  variants: {
    size: {
      default: "h-10 px-4 py-2",
      sm: "h-9 rounded-md px-3",
      lg: "h-11 rounded-md px-8",
      icon: "h-10 w-10",
    },
  },
})
```

----------------------------------------

TITLE: Creating TanStack Router Project (Bash)
DESCRIPTION: This command initializes a new TanStack Router project named 'my-app'. It uses the file-router template, includes Tailwind CSS, and pre-configures shadcn/ui add-ons, streamlining the project setup.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/installation/tanstack-router.mdx#_snippet_0

LANGUAGE: bash
CODE:
```
npx create-tsrouter-app@latest my-app --template file-router --tailwind --add-ons shadcn
```

----------------------------------------

TITLE: Importing shadcn/ui Components (TypeScript/TSX)
DESCRIPTION: This import statement demonstrates how to use a shadcn/ui component (e.g., `Button`) within a TypeScript/TSX file. Components are imported from the `@workspace/ui/components` path, making them available for use in the application.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/templates/monorepo-next/README.md#_snippet_2

LANGUAGE: tsx
CODE:
```
import { Button } from "@workspace/ui/components/button"
```

----------------------------------------

TITLE: Adjusting Sheet Component Size with CSS (TSX)
DESCRIPTION: This snippet demonstrates how to customize the width of the `SheetContent` using CSS classes, specifically Tailwind CSS. By applying utility classes like `w-[400px]` and `sm:w-[540px]`, you can control the sheet's size and responsiveness.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/sheet.mdx#_snippet_4

LANGUAGE: tsx
CODE:
```
<Sheet>
  <SheetTrigger>Open</SheetTrigger>
  <SheetContent className="w-[400px] sm:w-[540px]">
    <SheetHeader>
      <SheetTitle>Are you absolutely sure?</SheetTitle>
      <SheetDescription>
        This action cannot be undone. This will permanently delete your account
        and remove your data from our servers.
      </SheetDescription>
    </SheetHeader>
  </SheetContent>
</Sheet>
```

----------------------------------------

TITLE: Installing Radix UI Alert Dialog Dependency
DESCRIPTION: This command installs the core `@radix-ui/react-alert-dialog` package, which is a prerequisite for using the shadcn/ui Alert Dialog component.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/alert-dialog.mdx#_snippet_1

LANGUAGE: bash
CODE:
```
npm install @radix-ui/react-alert-dialog
```

----------------------------------------

TITLE: Installing Radix UI Toast Dependency (npm)
DESCRIPTION: This command installs the core `@radix-ui/react-toast` package, which is a prerequisite for manually setting up the shadcn/ui Toast component.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/toast.mdx#_snippet_2

LANGUAGE: bash
CODE:
```
npm install @radix-ui/react-toast
```

----------------------------------------

TITLE: Defining Registry Items in registry.json
DESCRIPTION: This snippet shows the `items` array, which contains definitions for all components or blocks within the registry. Each item must conform to the `registry-item` schema, detailing its name, type, title, description, and associated files.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/registry/registry-json.mdx#_snippet_4

LANGUAGE: json
CODE:
```
{
  "items": [
    {
      "name": "hello-world",
      "type": "registry:block",
      "title": "Hello World",
      "description": "A simple hello world component.",
      "files": [
        {
          "path": "registry/new-york/hello-world/hello-world.tsx",
          "type": "registry:component"
        }
      ]
    }
  ]
}
```

----------------------------------------

TITLE: Implementing Disabled State for InputOTP - TypeScript
DESCRIPTION: This snippet illustrates how to implement a disabled state for the `InputOTP` component. It applies Tailwind CSS classes (`has-[:disabled]:opacity-50` and `disabled:cursor-not-allowed`) to the container and the input respectively, providing visual feedback when the component is disabled.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/input-otp.mdx#_snippet_10

LANGUAGE: typescript
CODE:
```
const InputOTP = React.forwardRef<
  React.ElementRef<typeof OTPInput>,
  React.ComponentPropsWithoutRef<typeof OTPInput>
>(({ className, containerClassName, ...props }, ref) => (
  <OTPInput
    ref={ref}
    containerClassName={cn(
      "flex items-center gap-2 has-[:disabled]:opacity-50",
      containerClassName
    )}
    className={cn("disabled:cursor-not-allowed", className)}
    {...props}
  />
))
InputOTP.displayName = "InputOTP"
```

----------------------------------------

TITLE: Integrating PaginationLink with Next.js Link Component
DESCRIPTION: This diff snippet shows how to modify the PaginationLink component to utilize Next.js's <Link /> component instead of a standard <a> tag. This change enables client-side routing for pagination links, improving navigation performance in Next.js applications.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/pagination.mdx#_snippet_3

LANGUAGE: tsx
CODE:
```
+ import Link from "next/link"

- type PaginationLinkProps = ... & React.ComponentProps<"a">
+ type PaginationLinkProps = ... & React.ComponentProps<typeof Link>

const PaginationLink = ({...props }: ) => (
  <PaginationItem>
-   <a>
+   <Link>
      // ...
-   </a>
+   </Link>
)
```

----------------------------------------

TITLE: Configuring Webpack aliases in Gatsby with gatsby-node.ts
DESCRIPTION: Defines Webpack aliases in `gatsby-node.ts` to resolve specific import paths, such as `@/components` and `@/lib/utils`, ensuring proper module resolution for components and utilities.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/installation/gatsby.mdx#_snippet_3

LANGUAGE: ts
CODE:
```
import * as path from "path"

export const onCreateWebpackConfig = ({ actions }) => {
  actions.setWebpackConfig({
    resolve: {
      alias: {
        "@/components": path.resolve(__dirname, "src/components"),
        "@/lib/utils": path.resolve(__dirname, "src/lib/utils")
      }
    }
  })
}
```

----------------------------------------

TITLE: Configuring Tailwind CSS for Accordion Animations (JavaScript)
DESCRIPTION: This JavaScript configuration snippet, intended for tailwind.config.js, defines custom keyframes for accordion-down and accordion-up animations. These animations provide smooth expand and collapse transitions for the Accordion component.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/accordion.mdx#_snippet_2

LANGUAGE: js
CODE:
```
/** @type {import('tailwindcss').Config} */
module.exports = {
  theme: {
    extend: {
      keyframes: {
        "accordion-down": {
          from: { height: "0" },
          to: { height: "var(--radix-accordion-content-height)" }
        },
        "accordion-up": {
          from: { height: "var(--radix-accordion-content-height)" },
          to: { height: "0" }
        }
      },
      animation: {
        "accordion-down": "accordion-down 0.2s ease-out",
        "accordion-up": "accordion-up 0.2s ease-out"
      }
    }
  }
}
```

----------------------------------------

TITLE: Selecting Monorepo Project Type (CLI Prompt)
DESCRIPTION: This snippet shows the interactive prompt output when running 'npx shadcn@canary init'. Users should select 'Next.js (Monorepo)' to configure the project for a monorepo setup, which includes 'web' and 'ui' workspaces.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/monorepo.mdx#_snippet_1

LANGUAGE: bash
CODE:
```
? Would you like to start a new project?
    Next.js
❯   Next.js (Monorepo)
```

----------------------------------------

TITLE: Example registry.json Configuration
DESCRIPTION: This snippet provides a complete example of a `registry.json` file, demonstrating the structure for defining a custom component registry including its schema, name, homepage, and an example item with its files.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/registry/registry-json.mdx#_snippet_0

LANGUAGE: json
CODE:
```
{
  "$schema": "https://ui.shadcn.com/schema/registry.json",
  "name": "shadcn",
  "homepage": "https://ui.shadcn.com",
  "items": [
    {
      "name": "hello-world",
      "type": "registry:block",
      "title": "Hello World",
      "description": "A simple hello world component.",
      "files": [
        {
          "path": "registry/new-york/hello-world/hello-world.tsx",
          "type": "registry:component"
        }
      ]
    }
  ]
}
```

----------------------------------------

TITLE: Configuring TypeScript paths in Gatsby tsconfig.json
DESCRIPTION: Adds `baseUrl` and `paths` configurations to the `tsconfig.json` file, enabling absolute path imports like `@/*` for easier module resolution within the project.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/installation/gatsby.mdx#_snippet_2

LANGUAGE: ts
CODE:
```
{
  "compilerOptions": {
    // ...
    "baseUrl": ".",
    "paths": {
      "@/*": [
        "./src/*"
      ]
    }
    // ...
  }
}
```

----------------------------------------

TITLE: Resolving Recharts React 19 Compatibility
DESCRIPTION: This section provides the necessary steps to make `recharts` compatible with React 19. It involves overriding a specific dependency in `package.json` and then running an installation command with a compatibility flag.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/react-19.mdx#_snippet_5

LANGUAGE: JSON
CODE:
```
"overrides": {
  "react-is": "^19.0.0-rc-69d4b800-20241021"
}
```

LANGUAGE: npm
CODE:
```
npm install --legacy-peer-deps
```

----------------------------------------

TITLE: Installing Shadcn CLI Canary Version (Bash)
DESCRIPTION: This command installs the `shadcn` CLI, specifically the `canary` version, which is necessary to access the `build` command for generating registry JSON files.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/registry/getting-started.mdx#_snippet_4

LANGUAGE: bash
CODE:
```
npm install shadcn@canary
```

----------------------------------------

TITLE: Configuring CSS Variables in `components.json` (JSON)
DESCRIPTION: To enable theming with CSS variables, set the `tailwind.cssVariables` property to `true` in your `components.json` file. This configuration instructs the CLI to use CSS variables for styling when new components are added.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/changelog.mdx#_snippet_18

LANGUAGE: json
CODE:
```
{
  "tailwind": {
    "config": "tailwind.config.js",
    "css": "app/globals.css",
    "baseColor": "slate",
    "cssVariables": true
  }
}
```

----------------------------------------

TITLE: Creating a New Remix Indie Stack Project
DESCRIPTION: This command initializes a new Remix project using the `create-remix` CLI tool, specifically leveraging the `remix-run/indie-stack` template. It sets up the project with all the pre-configured tools and dependencies of the Indie Stack, providing a ready-to-use boilerplate.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/packages/shadcn/test/fixtures/frameworks/remix-indie-stack/README.md#_snippet_0

LANGUAGE: sh
CODE:
```
npx create-remix@latest --template remix-run/indie-stack
```

----------------------------------------

TITLE: Implementing SidebarTrigger with SidebarProvider in React TSX
DESCRIPTION: This example illustrates how to use the `SidebarTrigger` component to toggle the sidebar. It emphasizes that `SidebarTrigger` must be nested within a `SidebarProvider` to function correctly.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/sidebar.mdx#_snippet_37

LANGUAGE: tsx
CODE:
```
<SidebarProvider>
  <Sidebar />
  <main>
    <SidebarTrigger />
  </main>
</SidebarProvider>
```

----------------------------------------

TITLE: Updating React Peer Dependencies for React 19
DESCRIPTION: This diff snippet illustrates the necessary changes in a `package.json` file to update the `peerDependencies` to include support for React 19. It shows how to extend the accepted React and React DOM versions to ensure compatibility with newer React releases.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/react-19.mdx#_snippet_0

LANGUAGE: diff
CODE:
```
"peerDependencies": {
-  "react": "^16.8 || ^17.0 || ^18.0",
+  "react": "^16.8 || ^17.0 || ^18.0 || ^19.0",
-  "react-dom": "^16.8 || ^17.0 || ^18.0"
+  "react-dom": "^16.8 || ^17.0 || ^18.0 || ^19.0"
},
```

----------------------------------------

TITLE: Starting Remix Development Server
DESCRIPTION: This command starts the Remix application in development mode. It initiates a local development server that automatically rebuilds assets and refreshes the browser on file changes, facilitating a rapid development workflow. It's used for local testing and development.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/packages/shadcn/test/fixtures/frameworks/remix-indie-stack/README.md#_snippet_3

LANGUAGE: sh
CODE:
```
npm run dev
```

----------------------------------------

TITLE: Configuring PostCSS for Tailwind CSS in TanStack Start
DESCRIPTION: This TypeScript configuration file for PostCSS enables the Tailwind CSS PostCSS plugin, allowing PostCSS to process and optimize Tailwind CSS directives within your project.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/installation/tanstack.mdx#_snippet_1

LANGUAGE: typescript
CODE:
```
export default {
  plugins: {
    "@tailwindcss/postcss": {},
  },
}
```

----------------------------------------

TITLE: Configuring Utility Functions Import Alias (JSON)
DESCRIPTION: This alias specifies the import path for utility functions within the project. The CLI uses this value, along with `tsconfig.json` or `jsconfig.json` paths, to correctly place and reference generated components.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components-json.mdx#_snippet_10

LANGUAGE: json
CODE:
```
{
  "aliases": {
    "utils": "@/lib/utils"
  }
}
```

----------------------------------------

TITLE: shadcn/ui CLI Prompt for React 19 Peer Dependencies
DESCRIPTION: This snippet shows the interactive prompt displayed by the `shadcn` CLI when initializing or adding components with React 19. It offers users the choice to resolve potential peer dependency issues by applying either the `--force` or `--legacy-peer-deps` flag during installation.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/react-19.mdx#_snippet_4

LANGUAGE: bash
CODE:
```
It looks like you are using React 19.
Some packages may fail to install due to peer dependency issues (see https://ui.shadcn.com/react-19).

? How would you like to proceed? › - Use arrow-keys. Return to submit.
❯   Use --force
    Use --legacy-peer-deps
```

----------------------------------------

TITLE: Example Block File Structure (Plain Text)
DESCRIPTION: This snippet provides an example of the internal file organization for a complex block, demonstrating how page.tsx, components, hooks, and utility files can be structured within the block's dedicated folder. Developers can start with a single file and expand later.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/blocks.mdx#_snippet_5

LANGUAGE: txt
CODE:
```
dashboard-01
└── page.tsx
└── components
    └── hello-world.tsx
    └── example-card.tsx
└── hooks
    └── use-hello-world.ts
└── lib
    └── format-date.ts
```

----------------------------------------

TITLE: Installing Card Component via CLI (Bash)
DESCRIPTION: This snippet demonstrates how to install the Shadcn UI Card component using the command-line interface. It uses `npx` to execute the `shadcn` CLI tool, adding the `card` component to your project. This method simplifies setup by automating file creation and configuration.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/card.mdx#_snippet_0

LANGUAGE: bash
CODE:
```
npx shadcn@latest add card
```

----------------------------------------

TITLE: Installing Button Component via CLI
DESCRIPTION: This command installs the Shadcn UI Button component using the command-line interface, adding it to your project with necessary configurations.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/button.mdx#_snippet_0

LANGUAGE: bash
CODE:
```
npx shadcn@latest add button
```

----------------------------------------

TITLE: Enabling Tailwind CSS and PostCSS in Remix
DESCRIPTION: Modifies the `remix.config.js` file to enable Tailwind CSS and PostCSS integration within the Remix build process. This ensures that your Tailwind styles are correctly compiled and applied.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/installation/remix.mdx#_snippet_5

LANGUAGE: js
CODE:
```
/** @type {import('@remix-run/dev').AppConfig} */
export default {
  ...
  tailwind: true,
  postcss: true,
  ...
};
```

----------------------------------------

TITLE: Installing Label Component via CLI (Bash)
DESCRIPTION: This command uses the `shadcn` CLI to automatically add the Label component and its dependencies to your project, simplifying the installation process by handling file creation and configuration.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/label.mdx#_snippet_0

LANGUAGE: bash
CODE:
```
npx shadcn@latest add label
```

----------------------------------------

TITLE: Customizing Breadcrumb Separator in TSX
DESCRIPTION: This example demonstrates how to use a custom component, such as the `Slash` icon from `lucide-react`, as the separator in a `Breadcrumb`. By passing the custom component as `children` to `BreadcrumbSeparator`, you can achieve unique visual styles for the navigation path.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/breadcrumb.mdx#_snippet_3

LANGUAGE: tsx
CODE:
```
import { Slash } from "lucide-react"

...

<Breadcrumb>
  <BreadcrumbList>
    <BreadcrumbItem>
      <BreadcrumbLink href="/">Home</BreadcrumbLink>
    </BreadcrumbItem>
    <BreadcrumbSeparator>
      <Slash />
    </BreadcrumbSeparator>
    <BreadcrumbItem>
      <BreadcrumbLink href="/components">Components</BreadcrumbLink>
    </BreadcrumbItem>
  </BreadcrumbList>
</Breadcrumb>
```

----------------------------------------

TITLE: Adding Badges to SidebarMenuItem (TSX)
DESCRIPTION: This snippet illustrates the use of the `SidebarMenuBadge` component to render a badge within a `SidebarMenuItem`. This is useful for displaying counts, notifications, or other small pieces of information next to a menu item.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/sidebar.mdx#_snippet_34

LANGUAGE: tsx
CODE:
```
<SidebarMenuItem>
  <SidebarMenuButton />
  <SidebarMenuBadge>24</SidebarMenuBadge>
</SidebarMenuItem>
```

----------------------------------------

TITLE: Integrating SidebarMenuAction with SidebarMenuButton (TSX)
DESCRIPTION: This snippet shows how to use `SidebarMenuAction` within a `SidebarMenuItem` alongside a `SidebarMenuButton`. It highlights that `SidebarMenuAction` functions independently, allowing for separate clickable links and action buttons within the same menu item.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/sidebar.mdx#_snippet_30

LANGUAGE: tsx
CODE:
```
<SidebarMenuItem>
  <SidebarMenuButton asChild>
    <a href="#">
      <Home />
      <span>Home</span>
    </a>
  </SidebarMenuButton>
  <SidebarMenuAction>
    <Plus /> <span className="sr-only">Add Project</span>
  </SidebarMenuAction>
</SidebarMenuItem>
```

----------------------------------------

TITLE: Setting Tailwind CSS Utility Class Prefix (JSON)
DESCRIPTION: This configuration defines a custom prefix for Tailwind CSS utility classes, ensuring that components added by the CLI will adhere to this naming convention. This helps prevent conflicts with existing CSS.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components-json.mdx#_snippet_7

LANGUAGE: json
CODE:
```
{
  "tailwind": {
    "prefix": "tw-"
  }
}
```

----------------------------------------

TITLE: Updating Tailwind CSS Config for Input OTP Animations
DESCRIPTION: This JavaScript snippet shows the required modifications to your `tailwind.config.js` file. It adds `caret-blink` keyframes and animation, which are crucial for the visual feedback of the Input OTP component's cursor.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/input-otp.mdx#_snippet_1

LANGUAGE: js
CODE:
```
/** @type {import('tailwindcss').Config} */
module.exports = {
  theme: {
    extend: {
      keyframes: {
        "caret-blink": {
          "0%,70%,100%": { opacity: "1" },
          "20%,50%": { opacity: "0" }
        }
      },
      animation: {
        "caret-blink": "caret-blink 1.25s ease-out infinite"
      }
    }
  }
}
```

----------------------------------------

TITLE: Installing Radix UI Select Dependency (npm, Bash)
DESCRIPTION: This command installs `@radix-ui/react-select`, the foundational primitive for the shadcn/ui Select component, as a direct dependency in your project. It's a prerequisite for manual integration of the Select component.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/select.mdx#_snippet_1

LANGUAGE: bash
CODE:
```
npm install @radix-ui/react-select
```

----------------------------------------

TITLE: Configuring UI Components Import Alias (JSON)
DESCRIPTION: This alias specifically determines the installation directory for `ui` components. It allows customization of where these specific components are placed, leveraging your project's path configurations.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components-json.mdx#_snippet_12

LANGUAGE: json
CODE:
```
{
  "aliases": {
    "ui": "@/app/ui"
  }
}
```

----------------------------------------

TITLE: Setting Session Secrets for Fly.io Apps (Shell)
DESCRIPTION: These commands set the `SESSION_SECRET` environment variable for both production and staging Fly.io applications. The secret is generated using `openssl rand -hex 32` for security, and it's crucial for session management within the Remix application.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/packages/shadcn/test/fixtures/frameworks/remix-indie-stack/README.md#_snippet_8

LANGUAGE: sh
CODE:
```
fly secrets set SESSION_SECRET=$(openssl rand -hex 32) --app indie-stack-template
fly secrets set SESSION_SECRET=$(openssl rand -hex 32) --app indie-stack-template-staging
```

----------------------------------------

TITLE: Setting Base Color for Tailwind CSS in components.json (JSON)
DESCRIPTION: This property determines the default color palette used for generating components. The chosen base color, such as 'gray' or 'zinc', cannot be changed after the initial project initialization.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components-json.mdx#_snippet_5

LANGUAGE: json
CODE:
```
{
  "tailwind": {
    "baseColor": "gray" | "neutral" | "slate" | "stone" | "zinc"
  }
}
```

----------------------------------------

TITLE: Updating Project Dependencies using pnpm
DESCRIPTION: This snippet provides a `pnpm` command to update specific project dependencies to their latest versions. It targets `@radix-ui/*`, `cmdk`, `lucide-react`, `recharts`, `tailwind-merge`, and `clsx`, ensuring the project uses the most recent compatible packages.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/tailwind-v4.mdx#_snippet_6

LANGUAGE: Bash
CODE:
```
pnpm up "@radix-ui/*" cmdk lucide-react recharts tailwind-merge clsx --latest
```

----------------------------------------

TITLE: Overriding Tailwind CSS Variables - JSON
DESCRIPTION: This JSON configuration overrides default Tailwind CSS variables within the `theme` object for a shadcn/ui registry item. It customizes spacing and defines specific breakpoint values for responsive design, providing fine-grained control over layout and typography.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/registry/examples.mdx#_snippet_7

LANGUAGE: json
CODE:
```
{
  "$schema": "https://ui.shadcn.com/schema/registry-item.json",
  "name": "custom-theme",
  "type": "registry:theme",
  "cssVars": {
    "theme": {
      "spacing": "0.2rem",
      "breakpoint-sm": "640px",
      "breakpoint-md": "768px",
      "breakpoint-lg": "1024px",
      "breakpoint-xl": "1280px",
      "breakpoint-2xl": "1536px"
    }
  }
}
```

----------------------------------------

TITLE: Updating DropdownMenuItem Styling for Icons in TypeScript
DESCRIPTION: This snippet illustrates the addition of `gap-2 [&_svg]:pointer-events-none [&_svg]:size-4 [&_svg]:shrink-0` classes to the `DropdownMenuItem` component. This update automatically styles icons within dropdown menu items, ensuring consistent sizing and spacing.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/dropdown-menu.mdx#_snippet_4

LANGUAGE: tsx
CODE:
```
const DropdownMenuItem = React.forwardRef<
  React.ElementRef<typeof DropdownMenuPrimitive.Item>,
  React.ComponentPropsWithoutRef<typeof DropdownMenuPrimitive.Item> & {
    inset?: boolean
  }
>(({ className, inset, ...props }, ref) => (
  <DropdownMenuPrimitive.Item
    ref={ref}
    className={cn(
      "relative ... gap-2 [&_svg]:pointer-events-none [&_svg]:size-4 [&_svg]:shrink-0",
      inset && "pl-8",
      className
    )}
    {...props}
  />
))
```

----------------------------------------

TITLE: Configuring PostCSS for Tailwind CSS
DESCRIPTION: Creates a `postcss.config.js` file to integrate Tailwind CSS and Autoprefixer as PostCSS plugins. This configuration ensures your CSS is correctly processed by these tools.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/installation/remix.mdx#_snippet_4

LANGUAGE: js
CODE:
```
export default {
  plugins: {
    tailwindcss: {},
    autoprefixer: {},
  },
}
```

----------------------------------------

TITLE: Adding Custom CSS Rules in Registry Item JSON
DESCRIPTION: This JSON snippet illustrates how to embed custom CSS rules directly into a registry item using the `css` property. It supports various CSS constructs such as `@layer base` for global styles, `@layer components` for component-specific styles, `@utility` for custom utility classes, and `@keyframes` for animations. This allows for fine-grained control over the styling of the registry item.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/registry/registry-item-json.mdx#_snippet_12

LANGUAGE: json
CODE:
```
{
  "css": {
    "@layer base": {
      "body": {
        "font-size": "var(--text-base)",
        "line-height": "1.5"
      }
    },
    "@layer components": {
      "button": {
        "background-color": "var(--color-primary)",
        "color": "var(--color-white)"
      }
    },
    "@utility text-magic": {
      "font-size": "var(--text-base)",
      "line-height": "1.5"
    },
    "@keyframes wiggle": {
      "0%, 100%": {
        "transform": "rotate(-3deg)"
      },
      "50%": {
        "transform": "rotate(3deg)"
      }
    }
  }
}
```

----------------------------------------

TITLE: Defining OKLCH Color Directly as CSS Variable
DESCRIPTION: This CSS snippet shows an alternative way to define a chart color variable using the `oklch` color space directly. This allows for more precise color definition without needing to wrap the variable in a color space function when used in `chartConfig`.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/chart.mdx#_snippet_18

LANGUAGE: css
CODE:
```
--chart-1: oklch(70% 0.227 154.59);
```

----------------------------------------

TITLE: Importing Tabs Component in TSX
DESCRIPTION: This snippet demonstrates how to import the necessary `Tabs`, `TabsContent`, `TabsList`, and `TabsTrigger` components from the Shadcn UI library for use in a TypeScript/TSX application.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/tabs.mdx#_snippet_2

LANGUAGE: tsx
CODE:
```
import { Tabs, TabsContent, TabsList, TabsTrigger } from "@/components/ui/tabs"
```

----------------------------------------

TITLE: Rendering Basic Badge Component (TSX)
DESCRIPTION: Renders a `Badge` component with an `outline` variant. This demonstrates a fundamental usage of the component, displaying simple text within the badge's visual style.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/badge.mdx#_snippet_2

LANGUAGE: tsx
CODE:
```
<Badge variant="outline">Badge</Badge>
```

----------------------------------------

TITLE: Importing Alert Components (TypeScript/React)
DESCRIPTION: This code snippet shows the necessary import statement for the `Alert`, `AlertDescription`, and `AlertTitle` components. These components are essential for constructing an alert UI element within a TypeScript/React application.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/alert.mdx#_snippet_1

LANGUAGE: tsx
CODE:
```
import { Alert, AlertDescription, AlertTitle } from "@/components/ui/alert"
```

----------------------------------------

TITLE: Adding Registry Build Script to package.json (JSON)
DESCRIPTION: This snippet demonstrates how to add a custom script, `registry:build`, to the `scripts` section of your `package.json` file. This script executes the `shadcn build` command, automating the process of generating registry JSON files.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/registry/getting-started.mdx#_snippet_5

LANGUAGE: json
CODE:
```
{
  "scripts": {
    "registry:build": "shadcn build"
  }
}
```

----------------------------------------

TITLE: Installing Radix UI Slider Dependency via npm
DESCRIPTION: This command manually installs the core @radix-ui/react-slider package, which is a prerequisite for the shadcn/ui Slider component, providing its underlying functionality.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/slider.mdx#_snippet_1

LANGUAGE: bash
CODE:
```
npm install @radix-ui/react-slider
```

----------------------------------------

TITLE: Installing Registry Item with shadcn CLI
DESCRIPTION: This command demonstrates how to install a registry item using the `shadcn` CLI's `add` command. It requires the full URL of the registry item, which can be a local or remote registry entry, to fetch and integrate the component into the project.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/registry/getting-started.mdx#_snippet_8

LANGUAGE: bash
CODE:
```
npx shadcn@latest add http://localhost:3000/r/hello-world.json
```

----------------------------------------

TITLE: Adding Chart Specific CSS Colors (Manual Installation)
DESCRIPTION: This CSS snippet defines custom CSS variables for chart colors, supporting both light and dark themes. These variables are crucial for the visual styling and theming of the charts and should be added to your project's base CSS file as part of the manual installation process.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/chart.mdx#_snippet_4

LANGUAGE: css
CODE:
```
@layer base {
  :root {
    --chart-1: 12 76% 61%;
    --chart-2: 173 58% 39%;
    --chart-3: 197 37% 24%;
    --chart-4: 43 74% 66%;
    --chart-5: 27 87% 67%;
  }

  .dark {
    --chart-1: 220 70% 50%;
    --chart-2: 160 60% 45%;
    --chart-3: 30 80% 55%;
    --chart-4: 280 65% 60%;
    --chart-5: 340 75% 55%;
  }
}
```

----------------------------------------

TITLE: Setting Carousel Item Size (Equal Basis)
DESCRIPTION: Illustrates how to set the size of carousel items using the `basis-1/3` utility class, making each item occupy one-third of the carousel's width for a consistent layout.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/carousel.mdx#_snippet_4

LANGUAGE: tsx
CODE:
```
// 33% of the carousel width.
<Carousel>
  <CarouselContent>
    <CarouselItem className="basis-1/3">...</CarouselItem>
    <CarouselItem className="basis-1/3">...</CarouselItem>
    <CarouselItem className="basis-1/3">...</CarouselItem>
  </CarouselContent>
</Carousel>
```

----------------------------------------

TITLE: Using Switch Component in JSX (TypeScript)
DESCRIPTION: This snippet shows the basic JSX usage of the `Switch` component. It renders a simple toggle control without any additional props or event handlers.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/switch.mdx#_snippet_3

LANGUAGE: tsx
CODE:
```
<Switch />
```

----------------------------------------

TITLE: Installing Vaul Dependency Manually (Bash)
DESCRIPTION: This command installs 'vaul', the foundational library upon which the shadcn/ui Drawer component is built, as a direct dependency in your project.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/drawer.mdx#_snippet_1

LANGUAGE: bash
CODE:
```
npm install vaul
```

----------------------------------------

TITLE: Manually Adding Sidebar CSS Variables for Theming
DESCRIPTION: These CSS custom properties define the color palette for the sidebar component, including background, foreground, primary, accent, border, and ring colors for both light and dark themes. This block should be manually copied into your `app/globals.css` file if not using the CLI.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/sidebar.mdx#_snippet_2

LANGUAGE: css
CODE:
```
@layer base {
  :root {
    --sidebar-background: 0 0% 98%;
    --sidebar-foreground: 240 5.3% 26.1%;
    --sidebar-primary: 240 5.9% 10%;
    --sidebar-primary-foreground: 0 0% 98%;
    --sidebar-accent: 240 4.8% 95.9%;
    --sidebar-accent-foreground: 240 5.9% 10%;
    --sidebar-border: 220 13% 91%;
    --sidebar-ring: 217.2 91.2% 59.8%;
  }

  .dark {
    --sidebar-background: 240 5.9% 10%;
    --sidebar-foreground: 240 4.8% 95.9%;
    --sidebar-primary: 224.3 76.3% 48%;
    --sidebar-primary-foreground: 0 0% 100%;
    --sidebar-accent: 240 3.7% 15.9%;
    --sidebar-accent-foreground: 240 4.8% 95.9%;
    --sidebar-border: 240 3.7% 15.9%;
    --sidebar-ring: 217.2 91.2% 59.8%;
  }
}
```

----------------------------------------

TITLE: Adding Chart Specific CSS Colors (CLI Installation)
DESCRIPTION: This CSS snippet defines custom CSS variables for chart colors, supporting both light and dark themes. These variables are crucial for the visual styling and theming of the charts and should be added to your project's base CSS file as part of the CLI installation process.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/chart.mdx#_snippet_2

LANGUAGE: css
CODE:
```
@layer base {
  :root {
    --chart-1: 12 76% 61%;
    --chart-2: 173 58% 39%;
    --chart-3: 197 37% 24%;
    --chart-4: 43 74% 66%;
    --chart-5: 27 87% 67%;
  }

  .dark {
    --chart-1: 220 70% 50%;
    --chart-2: 160 60% 45%;
    --chart-3: 30 80% 55%;
    --chart-4: 280 65% 60%;
    --chart-5: 340 75% 55%;
  }
}
```

----------------------------------------

TITLE: Defining a Complex Component in shadcn/ui Registry (JSON)
DESCRIPTION: This snippet illustrates the structure of a complex `shadcn/ui` registry item, detailing how to bundle multiple file types (pages, components, hooks, utilities, and configuration files) under a single component name. It specifies the `path`, `type`, and optional `target` for each file, demonstrating a comprehensive component definition.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/registry/faq.mdx#_snippet_0

LANGUAGE: json
CODE:
```
{
  "$schema": "https://ui.shadcn.com/schema/registry-item.json",
  "name": "hello-world",
  "title": "Hello World",
  "type": "registry:block",
  "description": "A complex hello world component",
  "files": [
    {
      "path": "registry/new-york/hello-world/page.tsx",
      "type": "registry:page",
      "target": "app/hello/page.tsx"
    },
    {
      "path": "registry/new-york/hello-world/components/hello-world.tsx",
      "type": "registry:component"
    },
    {
      "path": "registry/new-york/hello-world/components/formatted-message.tsx",
      "type": "registry:component"
    },
    {
      "path": "registry/new-york/hello-world/hooks/use-hello.ts",
      "type": "registry:hook"
    },
    {
      "path": "registry/new-york/hello-world/lib/format-date.ts",
      "type": "registry:utils"
    },
    {
      "path": "registry/new-york/hello-world/hello.config.ts",
      "type": "registry:file",
      "target": "~/hello.config.ts"
    }
  ]
}
```

----------------------------------------

TITLE: Displaying Mode Toggle in Astro
DESCRIPTION: This Astro snippet demonstrates how to import and render the `ModeToggle` React component within an Astro page. The `client:load` directive ensures that the React component is hydrated and interactive on the client-side as soon as the page loads, enabling the theme toggle functionality.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/dark-mode/astro.mdx#_snippet_2

LANGUAGE: Astro
CODE:
```
--- import '../styles/globals.css'\nimport { ModeToggle } from '@/components/ModeToggle';\n---\n\n<!-- Inline script -->\n\n<html lang="en">\n\t<body>\n          <h1>Astro</h1>\n          <ModeToggle client:load />\n\t</body>\n</html>
```

----------------------------------------

TITLE: Defining Complex CSS Utility - JSON
DESCRIPTION: This JSON configuration defines a complex CSS utility class using the `@utility` directive for a shadcn/ui registry item. It creates a `scrollbar-hidden` utility that hides the scrollbar using `::-webkit-scrollbar`, useful for custom scrollable areas.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/registry/examples.mdx#_snippet_11

LANGUAGE: json
CODE:
```
{
  "$schema": "https://ui.shadcn.com/schema/registry-item.json",
  "name": "custom-component",
  "type": "registry:component",
  "css": {
    "@utility scrollbar-hidden": {
      "scrollbar-hidden": {
        "&::-webkit-scrollbar": {
          "display": "none"
        }
      }
    }
  }
}
```

----------------------------------------

TITLE: Installing Carousel via CLI
DESCRIPTION: This command installs the Carousel component using the shadcn/ui CLI, adding it to your project with necessary dependencies and configurations.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/carousel.mdx#_snippet_0

LANGUAGE: bash
CODE:
```
npx shadcn@latest add carousel
```

----------------------------------------

TITLE: Installing Calendar Component via CLI (Bash)
DESCRIPTION: This `npx` command uses the shadcn/ui CLI to automatically add the `Calendar` component and its required dependencies to your project, simplifying the installation process.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/calendar.mdx#_snippet_0

LANGUAGE: bash
CODE:
```
npx shadcn@latest add calendar
```

----------------------------------------

TITLE: Installing Menubar Component using CLI
DESCRIPTION: This command utilizes the shadcn/ui CLI to automatically add the Menubar component and its necessary dependencies to your project, streamlining the installation process.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/menubar.mdx#_snippet_0

LANGUAGE: bash
CODE:
```
npx shadcn@latest add menubar
```

----------------------------------------

TITLE: Installing Table Component via CLI (shadcn/ui)
DESCRIPTION: This snippet demonstrates how to install the shadcn/ui Table component using the command-line interface (CLI). It uses `npx shadcn@latest add table` to automatically add the component to your project, simplifying the setup process.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/table.mdx#_snippet_0

LANGUAGE: bash
CODE:
```
npx shadcn@latest add table
```

----------------------------------------

TITLE: Defining Custom Block (JSON)
DESCRIPTION: This JSON configuration defines a custom `shadcn/ui` block named `login-01`. It includes a description, specifies registry dependencies (`button`, `card`, `input`, `label`), and defines the files that comprise the block, including their paths, content (simplified for example), types (`registry:page`, `registry:component`), and target locations. This block can be installed via `npx shadcn add`.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/registry/examples.mdx#_snippet_4

LANGUAGE: json
CODE:
```
{
  "$schema": "https://ui.shadcn.com/schema/registry-item.json",
  "name": "login-01",
  "type": "registry:block",
  "description": "A simple login form.",
  "registryDependencies": ["button", "card", "input", "label"],
  "files": [
    {
      "path": "blocks/login-01/page.tsx",
      "content": "import { LoginForm } ...",
      "type": "registry:page",
      "target": "app/login/page.tsx"
    },
    {
      "path": "blocks/login-01/components/login-form.tsx",
      "content": "...",
      "type": "registry:component"
    }
  ]
}
```

----------------------------------------

TITLE: Disabling React Server Components (RSC) in `components.json` (JSON)
DESCRIPTION: To opt out of React Server Components support, set the `rsc` property to `false` in your `components.json` file. This configuration automatically manages the `use client` directive for components, making them compatible with frameworks that do not support RSC.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/changelog.mdx#_snippet_20

LANGUAGE: json
CODE:
```
{
  "rsc": false
}
```

----------------------------------------

TITLE: Importing XAxis for Chart X-Axis (Recharts, TypeScript)
DESCRIPTION: Imports the `XAxis` component from `recharts`, which is used to define and render the horizontal axis of a chart. It's a prerequisite for displaying data along the x-dimension.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/chart.mdx#_snippet_9

LANGUAGE: tsx
CODE:
```
import { Bar, BarChart, CartesianGrid, XAxis } from "recharts"
```

----------------------------------------

TITLE: Step 3: Adding SidebarMenu with Dynamic Items (TSX)
DESCRIPTION: This snippet enhances the `AppSidebar` by integrating `SidebarMenu` within a `SidebarGroup`, populating it with dynamic menu items. It demonstrates how to define a list of navigation items with icons and titles, then render them using `SidebarMenuItem` and `SidebarMenuButton` components.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/sidebar.mdx#_snippet_8

LANGUAGE: tsx
CODE:
```
import { Calendar, Home, Inbox, Search, Settings } from "lucide-react"

import {
  Sidebar,
  SidebarContent,
  SidebarGroup,
  SidebarGroupContent,
  SidebarGroupLabel,
  SidebarMenu,
  SidebarMenuButton,
  SidebarMenuItem,
} from "@/components/ui/sidebar"

// Menu items.
const items = [
  {
    title: "Home",
    url: "#",
    icon: Home,
  },
  {
    title: "Inbox",
    url: "#",
    icon: Inbox,
  },
  {
    title: "Calendar",
    url: "#",
    icon: Calendar,
  },
  {
    title: "Search",
    url: "#",
    icon: Search,
  },
  {
    title: "Settings",
    url: "#",
    icon: Settings,
  },
]

export function AppSidebar() {
  return (
    <Sidebar>
      <SidebarContent>
        <SidebarGroup>
          <SidebarGroupLabel>Application</SidebarGroupLabel>
          <SidebarGroupContent>
            <SidebarMenu>
              {items.map((item) => (
                <SidebarMenuItem key={item.title}>
                  <SidebarMenuButton asChild>
                    <a href={item.url}>
                      <item.icon />
                      <span>{item.title}</span>
                    </a>
                  </SidebarMenuButton>
                </SidebarMenuItem>
              ))}
            </SidebarMenu>
          </SidebarGroupContent>
        </SidebarGroup>
      </SidebarContent>
    </Sidebar>
  )
}
```

----------------------------------------

TITLE: Installing Context Menu via CLI
DESCRIPTION: This command installs the Context Menu component using the shadcn/ui CLI, automatically adding the necessary files and configurations to your project.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/context-menu.mdx#_snippet_0

LANGUAGE: bash
CODE:
```
npx shadcn@latest add context-menu
```

----------------------------------------

TITLE: Installing Dropdown Menu via CLI
DESCRIPTION: This command installs the Shadcn UI Dropdown Menu component using the command-line interface (CLI), adding it to your project.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/dropdown-menu.mdx#_snippet_0

LANGUAGE: bash
CODE:
```
npx shadcn@latest add dropdown-menu
```

----------------------------------------

TITLE: Installing Skeleton Component via CLI (Bash)
DESCRIPTION: This command uses the `shadcn` CLI to automatically add the Skeleton component to your project, handling dependencies and file creation. It's the recommended and most straightforward installation method.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/skeleton.mdx#_snippet_0

LANGUAGE: bash
CODE:
```
npx shadcn@latest add skeleton
```

----------------------------------------

TITLE: Installing Sonner via CLI (Bash)
DESCRIPTION: This command uses `npx` to add the Sonner component to a shadcn/ui project. It automates the setup process, including adding necessary files and configurations for the toast component.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/sonner.mdx#_snippet_0

LANGUAGE: bash
CODE:
```
npx shadcn@latest add sonner
```

----------------------------------------

TITLE: Installing Toggle Component via CLI
DESCRIPTION: Installs the Shadcn UI Toggle component using the command-line interface. This method automates the setup, adding the component and its dependencies to your project with pre-configured settings.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/toggle.mdx#_snippet_0

LANGUAGE: bash
CODE:
```
npx shadcn@latest add toggle
```

----------------------------------------

TITLE: Installing Select Component via shadcn/ui CLI (Bash)
DESCRIPTION: This command utilizes the shadcn/ui command-line interface to automatically add the Select component, along with its required dependencies and configuration, to your project. It simplifies the setup process by handling file generation and imports.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/select.mdx#_snippet_0

LANGUAGE: bash
CODE:
```
npx shadcn@latest add select
```

----------------------------------------

TITLE: Importing Chart Legend Components
DESCRIPTION: This TypeScript/React snippet provides the import statement for the `ChartLegend` and `ChartLegendContent` components. These are necessary for adding a customizable legend to charts, allowing users to understand the different data series represented.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/chart.mdx#_snippet_28

LANGUAGE: tsx
CODE:
```
import { ChartLegend, ChartLegendContent } from "@/components/ui/chart"
```

----------------------------------------

TITLE: Step 2: Creating a Basic AppSidebar Component (TSX)
DESCRIPTION: This code snippet, part of the "Your First Sidebar" tutorial, demonstrates the initial creation of the `AppSidebar` component. It imports `Sidebar` and `SidebarContent` and nests `SidebarContent` within `Sidebar`, establishing the fundamental structure for the sidebar.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/sidebar.mdx#_snippet_7

LANGUAGE: tsx
CODE:
```
import { Sidebar, SidebarContent } from "@/components/ui/sidebar"

export function AppSidebar() {
  return (
    <Sidebar>
      <SidebarContent />
    </Sidebar>
  )
}
```

----------------------------------------

TITLE: Running Prettier Formatting Script (Shell)
DESCRIPTION: This command executes the Prettier formatting script defined in the project's package.json, applying consistent code style across all files. It serves as a manual alternative to editor-based auto-formatting on save.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/packages/shadcn/test/fixtures/frameworks/remix-indie-stack/README.md#_snippet_12

LANGUAGE: Shell
CODE:
```
npm run format
```

----------------------------------------

TITLE: Creating a React Router Project
DESCRIPTION: This command initializes a new React Router project named `my-app` using the latest version of `create-react-router`, setting up the basic project structure.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/installation/react-router.mdx#_snippet_0

LANGUAGE: bash
CODE:
```
npx create-react-router@latest my-app
```

----------------------------------------

TITLE: Creating a new Gatsby project
DESCRIPTION: Initializes a new Gatsby project using the `create-gatsby` CLI tool. This is the first step to set up a Gatsby application.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/installation/gatsby.mdx#_snippet_0

LANGUAGE: bash
CODE:
```
npm init gatsby
```

----------------------------------------

TITLE: Defining Custom Style Extending shadcn/ui (JSON)
DESCRIPTION: This JSON configuration defines a custom `shadcn/ui` style that extends the default framework. It specifies dependencies like `@tabler/icons-react`, includes existing registry items (`login-01`, `calendar`, `editor`), and sets custom CSS variables for `font-sans` and a `brand` color for both light and dark modes. This style is applied during `npx shadcn init`.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/registry/examples.mdx#_snippet_0

LANGUAGE: json
CODE:
```
{
  "$schema": "https://ui.shadcn.com/schema/registry-item.json",
  "name": "example-style",
  "type": "registry:style",
  "dependencies": ["@tabler/icons-react"],
  "registryDependencies": [
    "login-01",
    "calendar",
    "https://example.com/r/editor.json"
  ],
  "cssVars": {
    "theme": {
      "font-sans": "Inter, sans-serif"
    },
    "light": {
      "brand": "20 14.3% 4.1%"
    },
    "dark": {
      "brand": "20 14.3% 4.1%"
    }
  }
}
```

----------------------------------------

TITLE: Importing Button Component in TSX
DESCRIPTION: This import statement makes the `Button` component available for use in a TypeScript React (TSX) file, typically from the project's UI components directory.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/button.mdx#_snippet_2

LANGUAGE: tsx
CODE:
```
import { Button } from "@/components/ui/button"
```

----------------------------------------

TITLE: Importing ScrollArea Component in TSX
DESCRIPTION: This snippet demonstrates how to import the `ScrollArea` component from the local UI components path, making it available for use in a TypeScript React file.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/scroll-area.mdx#_snippet_2

LANGUAGE: tsx
CODE:
```
import { ScrollArea } from "@/components/ui/scroll-area"
```

----------------------------------------

TITLE: Importing Progress Component (TypeScript/React)
DESCRIPTION: This line imports the `Progress` component from the local `components/ui/progress` path, making it available for use within a TypeScript or React application. It's a standard ES module import statement.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/progress.mdx#_snippet_2

LANGUAGE: tsx
CODE:
```
import { Progress } from "@/components/ui/progress"
```

----------------------------------------

TITLE: Installing Tailwind CSS for Vite
DESCRIPTION: This command installs the necessary Tailwind CSS packages for integration with a Vite project, including `tailwindcss` itself and the `@tailwindcss/vite` plugin.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/installation/vite.mdx#_snippet_1

LANGUAGE: bash
CODE:
```
npm install tailwindcss @tailwindcss/vite
```

----------------------------------------

TITLE: Importing Input OTP Components in TypeScript/React
DESCRIPTION: This TypeScript/React snippet demonstrates how to import the necessary sub-components (`InputOTP`, `InputOTPGroup`, `InputOTPSeparator`, `InputOTPSlot`) from the shadcn/ui library. These imports are essential for composing the Input OTP component in your application.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/input-otp.mdx#_snippet_3

LANGUAGE: tsx
CODE:
```
import {
  InputOTP,
  InputOTPGroup,
  InputOTPSeparator,
  InputOTPSlot
} from "@/components/ui/input-otp"
```

----------------------------------------

TITLE: Importing Hover Card Components in TSX
DESCRIPTION: This snippet demonstrates how to import the necessary components (`HoverCard`, `HoverCardContent`, `HoverCardTrigger`) for the Hover Card from the local UI components path in a TypeScript React project.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/hover-card.mdx#_snippet_2

LANGUAGE: tsx
CODE:
```
import {
  HoverCard,
  HoverCardContent,
  HoverCardTrigger,
} from "@/components/ui/hover-card"
```

----------------------------------------

TITLE: Importing Tooltip Components (TypeScript/TSX)
DESCRIPTION: This snippet demonstrates how to import the necessary `Tooltip` components (`Tooltip`, `TooltipContent`, `TooltipProvider`, `TooltipTrigger`) from the project's UI library for use in a TypeScript/TSX file.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/tooltip.mdx#_snippet_2

LANGUAGE: tsx
CODE:
```
import {
  Tooltip,
  TooltipContent,
  TooltipProvider,
  TooltipTrigger,
} from "@/components/ui/tooltip"
```

----------------------------------------

TITLE: Configuring Resizable Handle Visibility (TypeScript/React)
DESCRIPTION: This example demonstrates how to explicitly show the resizable handle by adding the `withHandle` prop to the `ResizableHandle` component. This provides a clear visual indicator for users to interact with the panel resizing functionality.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/resizable.mdx#_snippet_5

LANGUAGE: tsx
CODE:
```
import {
  ResizableHandle,
  ResizablePanel,
  ResizablePanelGroup,
} from "@/components/ui/resizable"

export default function Example() {
  return (
    <ResizablePanelGroup direction="horizontal">
      <ResizablePanel>One</ResizablePanel>
      <ResizableHandle withHandle />
      <ResizablePanel>Two</ResizablePanel>
    </ResizablePanelGroup>
  )
}
```

----------------------------------------

TITLE: Using SidebarSeparator in React TSX
DESCRIPTION: This snippet demonstrates the basic usage of the `SidebarSeparator` component within a `Sidebar` structure. It shows how to place separators in the header, content, and between groups to visually divide sections.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/sidebar.mdx#_snippet_36

LANGUAGE: tsx
CODE:
```
<Sidebar>
  <SidebarHeader />
  <SidebarSeparator />
  <SidebarContent>
    <SidebarGroup />
    <SidebarSeparator />
    <SidebarGroup />
  </SidebarContent>
</Sidebar>
```

----------------------------------------

TITLE: Wrapping Main Content for Inset Sidebar Variant in TSX
DESCRIPTION: Demonstrates the required usage of the `SidebarInset` component when the `Sidebar` is set to the `inset` variant. `SidebarInset` ensures that the main content correctly adjusts its layout to accommodate the inset sidebar.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/sidebar.mdx#_snippet_17

LANGUAGE: tsx
CODE:
```
<SidebarProvider>
  <Sidebar variant="inset" />
  <SidebarInset>
    <main>{children}</main>
  </SidebarInset>
</SidebarProvider>
```

----------------------------------------

TITLE: Installing Radix UI Toggle Dependency Manually
DESCRIPTION: Installs the core @radix-ui/react-toggle dependency using npm. This package provides the underlying primitive functionality for the Toggle component when performing a manual installation.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/toggle.mdx#_snippet_1

LANGUAGE: bash
CODE:
```
npm install @radix-ui/react-toggle
```

----------------------------------------

TITLE: Installing Collapsible Component Dependencies (npm)
DESCRIPTION: Installs the required `@radix-ui/react-collapsible` dependency for manual setup of the Collapsible component. This is a prerequisite step before copying the component's source code into your project.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/collapsible.mdx#_snippet_1

LANGUAGE: bash
CODE:
```
npm install @radix-ui/react-collapsible
```

----------------------------------------

TITLE: Configuring Single Sidebar Width in TSX
DESCRIPTION: Defines the fixed width for a single sidebar in both desktop and mobile views using constant variables. These variables are typically used when the application has only one sidebar.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/sidebar.mdx#_snippet_9

LANGUAGE: tsx
CODE:
```
const SIDEBAR_WIDTH = "16rem"
const SIDEBAR_WIDTH_MOBILE = "18rem"
```

----------------------------------------

TITLE: Defining Registry Item Name - JSON
DESCRIPTION: This snippet shows the `name` property, which serves as a unique identifier for the item within the registry. It is essential for referencing and managing individual components or blocks.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/registry/registry-item-json.mdx#_snippet_2

LANGUAGE: json
CODE:
```
{
  "name": "hello-world"
}
```

----------------------------------------

TITLE: Defining Human-Readable Registry Item Title - JSON
DESCRIPTION: This snippet illustrates the `title` property, providing a concise, human-readable name for the registry item. This title is typically displayed in UIs or documentation for easy identification.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/registry/registry-item-json.mdx#_snippet_3

LANGUAGE: json
CODE:
```
{
  "title": "Hello World"
}
```

----------------------------------------

TITLE: Installing Block with Overridden Primitives - JSON
DESCRIPTION: This JSON configuration installs the `login-01` block from the shadcn/ui registry and overrides its default `button`, `input`, and `label` primitives with custom ones specified by external URLs. This allows for consistent styling and behavior across components.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/registry/examples.mdx#_snippet_5

LANGUAGE: json
CODE:
```
{
  "$schema": "https://ui.shadcn.com/schema/registry-item.json",
  "name": "custom-login",
  "type": "registry:block",
  "registryDependencies": [
    "login-01",
    "https://example.com/r/button.json",
    "https://example.com/r/input.json",
    "https://example.com/r/label.json"
  ]
}
```

----------------------------------------

TITLE: Importing Checkbox Component (TypeScript/React)
DESCRIPTION: This snippet illustrates how to import the `Checkbox` component from the local UI library path (`@/components/ui/checkbox`), making it available for use in a TypeScript/React application.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/checkbox.mdx#_snippet_2

LANGUAGE: tsx
CODE:
```
import { Checkbox } from "@/components/ui/checkbox"
```

----------------------------------------

TITLE: Installing Radix UI Switch Dependency (Bash)
DESCRIPTION: This command manually installs the core `@radix-ui/react-switch` dependency, which is a prerequisite for the `shadcn/ui` Switch component. This step is part of the manual installation process.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/switch.mdx#_snippet_1

LANGUAGE: bash
CODE:
```
npm install @radix-ui/react-switch
```

----------------------------------------

TITLE: Installing Radix UI Accordion Dependency (Bash)
DESCRIPTION: This Bash command installs @radix-ui/react-accordion, the foundational dependency for the shadcn/ui Accordion component. This step is necessary when opting for a manual installation process.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/accordion.mdx#_snippet_1

LANGUAGE: bash
CODE:
```
npm install @radix-ui/react-accordion
```

----------------------------------------

TITLE: Installing Radix UI Toggle Group Dependency Manually
DESCRIPTION: This command installs the core @radix-ui/react-toggle-group package, which is a prerequisite for manually integrating the Toggle Group component into your project.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/toggle-group.mdx#_snippet_1

LANGUAGE: bash
CODE:
```
npm install @radix-ui/react-toggle-group
```

----------------------------------------

TITLE: Installing Radix UI Scroll Area Dependency
DESCRIPTION: This command installs the core `@radix-ui/react-scroll-area` package, which is a prerequisite for manually setting up the shadcn/ui ScrollArea component.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/scroll-area.mdx#_snippet_1

LANGUAGE: bash
CODE:
```
npm install @radix-ui/react-scroll-area
```

----------------------------------------

TITLE: Installing Menubar Dependencies Manually
DESCRIPTION: This command installs the core `@radix-ui/react-menubar` package, which is a fundamental prerequisite for using the Menubar component. It is an essential step when performing a manual installation.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/menubar.mdx#_snippet_1

LANGUAGE: bash
CODE:
```
npm install @radix-ui/react-menubar
```

----------------------------------------

TITLE: Installing Radix UI Radio Group Dependency Manually (Bash)
DESCRIPTION: This command installs the core @radix-ui/react-radio-group dependency, which is a prerequisite for the manual setup of the Radio Group component and provides the underlying primitive functionality.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/radio-group.mdx#_snippet_1

LANGUAGE: bash
CODE:
```
npm install @radix-ui/react-radio-group
```

----------------------------------------

TITLE: Installing Radix UI Popover Dependency Manually
DESCRIPTION: This command installs the core @radix-ui/react-popover package, which is the underlying primitive for the shadcn/ui Popover component. This step is necessary when performing a manual installation of the component.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/popover.mdx#_snippet_1

LANGUAGE: bash
CODE:
```
npm install @radix-ui/react-popover
```

----------------------------------------

TITLE: Configuring Gatsby project with TypeScript and Tailwind CSS
DESCRIPTION: Illustrates the interactive prompts and typical responses when configuring a new Gatsby project to use TypeScript and Tailwind CSS during the `npm init gatsby` process.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/installation/gatsby.mdx#_snippet_1

LANGUAGE: txt
CODE:
```
✔ What would you like to call your site?
· your-app-name
✔ What would you like to name the folder where your site will be created?
· your-app-name
✔ Will you be using JavaScript or TypeScript?
· TypeScript
✔ Will you be using a CMS?
· Choose whatever you want
✔ Would you like to install a styling system?
· Tailwind CSS
✔ Would you like to install additional features with other plugins?
· Choose whatever you want
✔ Shall we do this? (Y/n) · Yes
```

----------------------------------------

TITLE: Adding a New Category to Shadcn UI Registry (TSX)
DESCRIPTION: This snippet demonstrates how to add a new category to the `registryCategories` array in `apps/www/registry/registry-categories.ts`. It shows the structure for defining a category with `name`, `slug`, and `hidden` properties, which helps organize blocks in the registry. This is a prerequisite for categorizing new UI blocks.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/blocks.mdx#_snippet_9

LANGUAGE: tsx
CODE:
```
export const registryCategories = [
  // ...
  {
    "name": "Input",
    "slug": "input",
    "hidden": false
  }
]
```

----------------------------------------

TITLE: Installing Project Dependencies with pnpm
DESCRIPTION: This command installs all necessary project dependencies using pnpm, leveraging the monorepo's workspace configuration.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/CONTRIBUTING.md#_snippet_3

LANGUAGE: bash
CODE:
```
pnpm install
```

----------------------------------------

TITLE: Installing Project Dependencies with pnpm (Bash)
DESCRIPTION: This command uses pnpm to install all required project dependencies as defined in the package.json file. Running this is essential to ensure all necessary packages are available for building and running the shadcn/ui project locally.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/blocks.mdx#_snippet_2

LANGUAGE: bash
CODE:
```
pnpm install
```

----------------------------------------

TITLE: Defining a Custom Registry Item (Full Example) - JSON
DESCRIPTION: This snippet illustrates the complete structure of a `registry-item.json` file, used for defining custom registry items. It includes properties like name, type, title, description, associated files (components, hooks), and CSS variables for theme customization, providing a comprehensive definition for a 'hello-world' block.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/registry/registry-item-json.mdx#_snippet_0

LANGUAGE: json
CODE:
```
{
  "$schema": "https://ui.shadcn.com/schema/registry-item.json",
  "name": "hello-world",
  "type": "registry:block",
  "title": "Hello World",
  "description": "A simple hello world component.",
  "files": [
    {
      "path": "registry/new-york/hello-world/hello-world.tsx",
      "type": "registry:component"
    },
    {
      "path": "registry/new-york/hello-world/use-hello-world.ts",
      "type": "registry:hook"
    }
  ],
  "cssVars": {
    "theme": {
      "font-heading": "Poppins, sans-serif"
    },
    "light": {
      "brand": "20 14.3% 4.1%"
    },
    "dark": {
      "brand": "20 14.3% 4.1%"
    }
  }
}
```

----------------------------------------

TITLE: Adding Custom Animations - JSON
DESCRIPTION: This JSON configuration defines a custom CSS animation and integrates it into the theme variables for a shadcn/ui registry item. It sets up a `wiggle` keyframe animation and assigns it to a CSS variable `--animate-wiggle`, enabling its use in components.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/registry/examples.mdx#_snippet_13

LANGUAGE: json
CODE:
```
{
  "$schema": "https://ui.shadcn.com/schema/registry-item.json",
  "name": "custom-component",
  "type": "registry:component",
  "cssVars": {
    "theme": {
      "--animate-wiggle": "wiggle 1s ease-in-out infinite"
    }
  },
  "css": {
    "@keyframes wiggle": {
      "0%, 100%": {
        "transform": "rotate(-3deg)"
      },
      "50%": {
        "transform": "rotate(3deg)"
      }
    }
  }
}
```

----------------------------------------

TITLE: Customizing Sidebar State Cookie Name in TSX
DESCRIPTION: Allows changing the default cookie name used by `SidebarProvider` to store the sidebar's open/closed state. The default name is `sidebar_state`.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/sidebar.mdx#_snippet_13

LANGUAGE: tsx
CODE:
```
const SIDEBAR_COOKIE_NAME = "sidebar_state"
```

----------------------------------------

TITLE: Defining Chart Colors Directly with Hex Values
DESCRIPTION: This TypeScript/React snippet demonstrates defining chart colors directly within the `chartConfig` using hex color codes. This method provides a straightforward way to assign specific colors to chart series without relying on CSS variables, suitable for static color assignments.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/chart.mdx#_snippet_20

LANGUAGE: tsx
CODE:
```
const chartConfig = {
  desktop: {
    label: "Desktop",
    color: "#2563eb",
  },
} satisfies ChartConfig
```

----------------------------------------

TITLE: Installing Context Menu Dependencies Manually
DESCRIPTION: This command installs the core @radix-ui/react-context-menu primitive, which is a required dependency for the shadcn/ui Context Menu component when performing a manual installation.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/context-menu.mdx#_snippet_1

LANGUAGE: bash
CODE:
```
npm install @radix-ui/react-context-menu
```

----------------------------------------

TITLE: Installing Radix UI Dialog Dependency Manually
DESCRIPTION: This command installs the core `@radix-ui/react-dialog` package, which is a foundational dependency for the shadcn/ui Dialog component when performing a manual installation. It's a prerequisite before copying the component's source code.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/dialog.mdx#_snippet_1

LANGUAGE: bash
CODE:
```
npm install @radix-ui/react-dialog
```

----------------------------------------

TITLE: Installing Radix UI Navigation Menu Dependencies (Bash)
DESCRIPTION: Installs the core @radix-ui/react-navigation-menu dependency, which is a prerequisite for the manual setup of the Shadcn UI Navigation Menu component.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/navigation-menu.mdx#_snippet_1

LANGUAGE: bash
CODE:
```
npm install @radix-ui/react-navigation-menu
```

----------------------------------------

TITLE: Installing Avatar Component Dependencies Manually (Bash)
DESCRIPTION: This command installs the @radix-ui/react-avatar package, which is a required dependency for manually integrating the Avatar component into a project.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/avatar.mdx#_snippet_1

LANGUAGE: bash
CODE:
```
npm install @radix-ui/react-avatar
```

----------------------------------------

TITLE: Migrating from `tailwindcss-animate` to `tw-animate-css` in CSS
DESCRIPTION: This diff illustrates the necessary change in a global CSS file to migrate animation libraries. It shows the removal of the `@plugin 'tailwindcss-animate'` directive and its replacement with `@import "tw-animate-css"`, reflecting the deprecation of the former in favor of the latter.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/tailwind-v4.mdx#_snippet_9

LANGUAGE: Diff
CODE:
```
- @plugin 'tailwindcss-animate';
+ @import "tw-animate-css";
```

----------------------------------------

TITLE: Building the Block Registry (Bash)
DESCRIPTION: This command triggers the build process for the block registry. It should be run after updating block definitions in registry-blocks.ts to ensure changes are compiled and available for preview or publication. It is also a prerequisite before capturing screenshots or submitting a pull request.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/blocks.mdx#_snippet_7

LANGUAGE: bash
CODE:
```
pnpm registry:build
```

----------------------------------------

TITLE: Defining Registry Item Files in JSON
DESCRIPTION: This snippet demonstrates how to define the files associated with a registry item using the `files` property. Each file entry specifies its `path`, `type` (e.g., `registry:page`, `registry:component`, `registry:hook`, `registry:file`), and an optional `target` property, which is required for `registry:page` and `registry:file` types to indicate the destination path within a project. The `~` symbol refers to the project root.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/registry/registry-item-json.mdx#_snippet_9

LANGUAGE: json
CODE:
```
{
  "files": [
    {
      "path": "registry/new-york/hello-world/page.tsx",
      "type": "registry:page",
      "target": "app/hello/page.tsx"
    },
    {
      "path": "registry/new-york/hello-world/hello-world.tsx",
      "type": "registry:component"
    },
    {
      "path": "registry/new-york/hello-world/use-hello-world.ts",
      "type": "registry:hook"
    },
    {
      "path": "registry/new-york/hello-world/.env",
      "type": "registry:file",
      "target": "~/.env"
    }
  ]
}
```

----------------------------------------

TITLE: Viewing Specific Component Changes with `shadcn diff` (Bash)
DESCRIPTION: To inspect detailed changes for a particular component, append its name (e.g., 'alert') to the `diff` command. This provides a granular view of modifications, helping users understand what exactly has been updated.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/changelog.mdx#_snippet_13

LANGUAGE: bash
CODE:
```
npx shadcn diff alert
```

----------------------------------------

TITLE: Applying Custom Tailwind Prefix in TSX
DESCRIPTION: This snippet demonstrates how the shadcn/ui CLI automatically applies a custom Tailwind prefix (e.g., 'tw-') to utility classes within components, allowing seamless integration into projects that already use Tailwind CSS with a different prefix or design system.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/changelog.mdx#_snippet_2

LANGUAGE: tsx
CODE:
```
<AlertDialog className="tw-grid tw-gap-4 tw-border tw-bg-background tw-shadow-lg" />
```

----------------------------------------

TITLE: Installing next-themes package (Bash)
DESCRIPTION: This command installs the `next-themes` library, a crucial dependency for implementing dark mode in a Next.js application. It uses npm, the default package manager for Node.js, to add the package to your project's dependencies.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/dark-mode/next.mdx#_snippet_0

LANGUAGE: bash
CODE:
```
npm install next-themes
```

----------------------------------------

TITLE: Logging In with Cypress Utility (TypeScript)
DESCRIPTION: This Cypress utility command (`cy.login()`) is used within End-to-End tests to simulate a user login without going through the actual login flow. After execution, the test context is set as if a new user is logged in, streamlining authenticated feature testing.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/packages/shadcn/test/fixtures/frameworks/remix-indie-stack/README.md#_snippet_10

LANGUAGE: ts
CODE:
```
cy.login();
// you are now logged in as a new user
```

----------------------------------------

TITLE: Cloning the shadcn/ui Repository
DESCRIPTION: This command clones the shadcn/ui repository from GitHub to your local machine. Replace `your-username` with your actual GitHub username.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/CONTRIBUTING.md#_snippet_0

LANGUAGE: bash
CODE:
```
git clone https://github.com/your-username/ui.git
```

----------------------------------------

TITLE: Installing cmdk Dependency Manually
DESCRIPTION: This command installs the core `cmdk` library, which is a prerequisite for the shadcn/ui Command component, when performing a manual installation.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/command.mdx#_snippet_1

LANGUAGE: bash
CODE:
```
npm install cmdk
```

----------------------------------------

TITLE: Installing Radix UI Label Dependency (npm)
DESCRIPTION: This command installs the core `@radix-ui/react-label` package, which is a prerequisite for the `shadcn/ui` Label component when performing a manual installation, using the npm package manager.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/label.mdx#_snippet_1

LANGUAGE: bash
CODE:
```
npm install @radix-ui/react-label
```

----------------------------------------

TITLE: Installing Checkbox Dependencies Manually (npm)
DESCRIPTION: This snippet shows how to manually install the core dependency for the Checkbox component, `@radix-ui/react-checkbox`, using npm, which is a prerequisite for manual integration.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/checkbox.mdx#_snippet_1

LANGUAGE: bash
CODE:
```
npm install @radix-ui/react-checkbox
```

----------------------------------------

TITLE: Configuring Library Functions Import Alias (JSON)
DESCRIPTION: This alias sets the import path for common library functions, such as `format-date` or `generate-id`. It helps the CLI organize and reference these functions correctly within the project structure.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components-json.mdx#_snippet_13

LANGUAGE: json
CODE:
```
{
  "aliases": {
    "lib": "@/lib"
  }
}
```

----------------------------------------

TITLE: Importing ChartLegend Components (shadcn/ui, TypeScript)
DESCRIPTION: Imports `ChartLegend` and `ChartLegendContent` from `@/components/ui/chart`. These components are custom UI elements for displaying a legend that explains the different data series in a chart.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/chart.mdx#_snippet_13

LANGUAGE: tsx
CODE:
```
import { ChartLegend, ChartLegendContent } from "@/components/ui/chart"
```

----------------------------------------

TITLE: Importing Toggle Group Components in TSX
DESCRIPTION: This snippet demonstrates how to import the ToggleGroup and ToggleGroupItem components from your local shadcn/ui components path, making them ready for use in a React component.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/toggle-group.mdx#_snippet_2

LANGUAGE: tsx
CODE:
```
import { ToggleGroup, ToggleGroupItem } from "@/components/ui/toggle-group"
```

----------------------------------------

TITLE: Adding Custom Base Styles - JSON
DESCRIPTION: This JSON configuration defines base CSS styles using the `@layer base` directive for a shadcn/ui registry item. It sets custom font sizes for `h1` and `h2` elements, ensuring consistent typography across the application's foundational styles.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/registry/examples.mdx#_snippet_8

LANGUAGE: json
CODE:
```
{
  "$schema": "https://ui.shadcn.com/schema/registry-item.json",
  "name": "custom-style",
  "type": "registry:style",
  "css": {
    "@layer base": {
      "h1": {
        "font-size": "var(--text-2xl)"
      },
      "h2": {
        "font-size": "var(--text-xl)"
      }
    }
  }
}
```

----------------------------------------

TITLE: Importing Menubar Components in TSX
DESCRIPTION: This snippet demonstrates how to import the various sub-components of the Menubar from the local UI components path. These imports are crucial for utilizing the Menubar in a React or Next.js application.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/menubar.mdx#_snippet_2

LANGUAGE: tsx
CODE:
```
import {
  Menubar,
  MenubarContent,
  MenubarItem,
  MenubarMenu,
  MenubarSeparator,
  MenubarShortcut,
  MenubarTrigger
} from "@/components/ui/menubar"
```

----------------------------------------

TITLE: Importing Separator Component (TypeScript/TSX)
DESCRIPTION: Imports the `Separator` component from the local `shadcn/ui` components path, making it available for use within your React or Next.js application's JSX/TSX files.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/separator.mdx#_snippet_2

LANGUAGE: tsx
CODE:
```
import { Separator } from "@/components/ui/separator"
```

----------------------------------------

TITLE: Updating shadcn/ui Project Aliases in components.json (JSON)
DESCRIPTION: This JSON configuration snippet illustrates how to update an existing shadcn/ui project's `components.json` file to align with the new CLI's requirements. It defines crucial import aliases for various code categories like 'components', 'utils', 'ui', 'lib', and 'hooks', ensuring proper module resolution and compatibility with the updated tooling. Users should replace '@' with their custom prefix if using a different alias.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/changelog.mdx#_snippet_1

LANGUAGE: json
CODE:
```
{
  "$schema": "https://ui.shadcn.com/schema.json",
  "style": "new-york",
  "tailwind": {
    // ...
  },
  "aliases": {
    "components": "@/components",
    "utils": "@/lib/utils",
    "ui": "@/components/ui",
    "lib": "@/lib",
    "hooks": "@/hooks"
  }
}
```

----------------------------------------

TITLE: Initializing registry.json for Shadcn Registry (JSON)
DESCRIPTION: This snippet demonstrates the initial structure of the `registry.json` file, which is essential for using the `shadcn` CLI to build a component registry. It defines the schema, a unique name for the registry, its homepage, and an empty array for component items.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/registry/getting-started.mdx#_snippet_0

LANGUAGE: json
CODE:
```
{
  "$schema": "https://ui.shadcn.com/schema/registry.json",
  "name": "acme",
  "homepage": "https://acme.com",
  "items": [
    // ...
  ]
}
```

----------------------------------------

TITLE: Creating a Basic React Component for Shadcn Registry (TSX)
DESCRIPTION: This code snippet provides an example of a simple React component, `<HelloWorld />`, intended to be added to the component registry. It imports a `Button` component and renders 'Hello World' within it, showcasing a basic component structure.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/registry/getting-started.mdx#_snippet_1

LANGUAGE: tsx
CODE:
```
import { Button } from "@/components/ui/button"

export function HelloWorld() {
  return <Button>Hello World</Button>
}
```

----------------------------------------

TITLE: Resolving npm ERESOLVE Error with React 19
DESCRIPTION: This snippet displays a common `npm ERESOLVE` error message that occurs when installing a package that does not explicitly list React 19 as a compatible peer dependency. It indicates a conflict where the root project uses React 19, but a dependency expects an older version.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/react-19.mdx#_snippet_1

LANGUAGE: bash
CODE:
```
npm error code ERESOLVE
npm error ERESOLVE unable to resolve dependency tree
npm error
npm error While resolving: my-app@0.1.0
npm error Found: react@19.0.0-rc-69d4b800-20241021
npm error node_modules/react
npm error   react@"19.0.0-rc-69d4b800-20241021" from the root project
```

----------------------------------------

TITLE: Defining Custom Theme Variables - JSON
DESCRIPTION: This JSON configuration defines custom CSS variables within the `theme` object for a shadcn/ui registry item. It sets specific font families for headings and a custom box shadow for cards, allowing for consistent theming across the application.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/registry/examples.mdx#_snippet_6

LANGUAGE: json
CODE:
```
{
  "$schema": "https://ui.shadcn.com/schema/registry-item.json",
  "name": "custom-theme",
  "type": "registry:theme",
  "cssVars": {
    "theme": {
      "font-heading": "Inter, sans-serif",
      "shadow-card": "0 0 0 1px rgba(0, 0, 0, 0.1)"
    }
  }
}
```

----------------------------------------

TITLE: Defining NPM Package Dependencies for Registry Item - JSON
DESCRIPTION: This snippet demonstrates the `dependencies` property, which lists external npm packages required by the registry item. It allows specifying package names and optional versions, ensuring all necessary third-party libraries are identified.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/registry/registry-item-json.mdx#_snippet_7

LANGUAGE: json
CODE:
```
{
  "dependencies": [
    "@radix-ui/react-accordion",
    "zod",
    "lucide-react",
    "name@1.0.2"
  ]
}
```

----------------------------------------

TITLE: Starting the Development Server (Bash)
DESCRIPTION: This command starts the local development server for the www application, allowing you to preview your changes and new blocks in a browser. It enables live reloading for a smooth development experience.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/blocks.mdx#_snippet_3

LANGUAGE: bash
CODE:
```
pnpm www:dev
```

----------------------------------------

TITLE: Updating Chart Color References in JavaScript/TypeScript
DESCRIPTION: This diff shows how to simplify chart color definitions in a JavaScript/TypeScript configuration. Since theme colors now inherently include `hsl()` values, the redundant `hsl()` wrapper can be removed, allowing direct use of CSS variables like `var(--chart-1)`.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/tailwind-v4.mdx#_snippet_4

LANGUAGE: Diff
CODE:
```
const chartConfig = {
  desktop: {
    label: "Desktop",
-    color: "hsl(var(--chart-1))",
+    color: "var(--chart-1)",
  },
  mobile: {
    label: "Mobile",
-   color: "hsl(var(--chart-2))",
+   color: "var(--chart-2)",
  },
} satisfies ChartConfig
```

----------------------------------------

TITLE: Registering a New Block in registry-blocks.tsx (TSX)
DESCRIPTION: This TypeScript/TSX code snippet demonstrates how to add a new block definition to the registry-blocks.ts file. It includes essential metadata such as name, author, title, description, type, registryDependencies, dependencies, files (with path, type, and optional target), and categories, which are crucial for the block to be recognized and built by the system.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/blocks.mdx#_snippet_6

LANGUAGE: tsx
CODE:
```
export const blocks = [
  // ...
  {
    name: "dashboard-01",
    author: "shadcn (https://ui.shadcn.com)",
    title: "Dashboard",
    description: "A simple dashboard with a hello world component.",
    type: "registry:block",
    registryDependencies: ["input", "button", "card"],
    dependencies: ["zod"],
    files: [
      {
        path: "blocks/dashboard-01/page.tsx",
        type: "registry:page",
        target: "app/dashboard/page.tsx"
      },
      {
        path: "blocks/dashboard-01/components/hello-world.tsx",
        type: "registry:component"
      },
      {
        path: "blocks/dashboard-01/components/example-card.tsx",
        type: "registry:component"
      },
      {
        path: "blocks/dashboard-01/hooks/use-hello-world.ts",
        type: "registry:hook"
      },
      {
        path: "blocks/dashboard-01/lib/format-date.ts",
        type: "registry:lib"
      }
    ],
    categories: ["dashboard"]
  }
]
```

----------------------------------------

TITLE: Example Output of `shadcn diff` Command (Text)
DESCRIPTION: This text snippet illustrates the typical output when running `npx shadcn diff`, showing a clear list of components (e.g., 'button', 'toast') that have updates, along with the specific files associated with those components.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/changelog.mdx#_snippet_12

LANGUAGE: txt
CODE:
```
The following components have updates available:
- button
  - /path/to/my-app/components/ui/button.tsx
- toast
  - /path/to/my-app/components/ui/use-toast.ts
  - /path/to/my-app/components/ui/toaster.tsx
```

----------------------------------------

TITLE: Adding a Component Definition to registry.json (JSON)
DESCRIPTION: This snippet shows how to extend the `registry.json` file by adding a new component definition to its `items` array. It specifies the component's name, type, title, description, and the relative path and type of its associated file.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/registry/getting-started.mdx#_snippet_3

LANGUAGE: json
CODE:
```
{
  "$schema": "https://ui.shadcn.com/schema/registry.json",
  "name": "acme",
  "homepage": "https://acme.com",
  "items": [
    {
      "name": "hello-world",
      "type": "registry:block",
      "title": "Hello World",
      "description": "A simple hello world component.",
      "files": [
        {
          "path": "registry/new-york/hello-world/hello-world.tsx",
          "type": "registry:component"
        }
      ]
    }
  ]
}
```

----------------------------------------

TITLE: Defining JSON Schema for components.json (JSON)
DESCRIPTION: This snippet defines the JSON Schema for the `components.json` file, providing validation and auto-completion capabilities for the configuration. It links to the official shadcn/ui schema.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components-json.mdx#_snippet_1

LANGUAGE: json
CODE:
```
{
  "$schema": "https://ui.shadcn.com/schema.json"
}
```

----------------------------------------

TITLE: Importing Aspect Ratio and Image Components (TSX)
DESCRIPTION: Imports the `Image` component from `next/image` and the `AspectRatio` component from the local `shadcn/ui` path, making them available for use in a React/Next.js application.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/aspect-ratio.mdx#_snippet_2

LANGUAGE: tsx
CODE:
```
import Image from "next/image"

import { AspectRatio } from "@/components/ui/aspect-ratio"
```

----------------------------------------

TITLE: Importing Accordion Components (TypeScript/React)
DESCRIPTION: This TypeScript/React snippet demonstrates how to import the core Accordion components (Accordion, AccordionContent, AccordionItem, AccordionTrigger) from the local shadcn/ui path. These imports are essential for using the component in a React application.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/accordion.mdx#_snippet_3

LANGUAGE: tsx
CODE:
```
import {
  Accordion,
  AccordionContent,
  AccordionItem,
  AccordionTrigger
} from "@/components/ui/accordion"
```

----------------------------------------

TITLE: Navigating to Application Directory (Bash)
DESCRIPTION: Before adding components, navigate into the specific application directory (e.g., 'apps/web') where the components will be used. This ensures the CLI installs components and updates import paths correctly for that application.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/monorepo.mdx#_snippet_2

LANGUAGE: bash
CODE:
```
cd apps/web
```

----------------------------------------

TITLE: Running Documentation Locally
DESCRIPTION: This command starts the development server for the `www` workspace, which hosts the project documentation. It allows local viewing and editing of MDX documentation files.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/CONTRIBUTING.md#_snippet_11

LANGUAGE: bash
CODE:
```
pnpm --filter=www dev
```

----------------------------------------

TITLE: shadcn build Command Options (CLI Text)
DESCRIPTION: This section describes the options available for the `shadcn build` command, allowing users to customize the build process. Options include specifying the path to the `registry.json` file and defining a custom output directory for the generated JSON files.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/cli.mdx#_snippet_5

LANGUAGE: txt
CODE:
```
Usage: shadcn build [options] [registry]

build components for a shadcn registry

Arguments:
  registry             path to registry.json file (default: "./registry.json")

Options:
  -o, --output <path>  destination directory for json files (default: "./public/r")
  -c, --cwd <cwd>      the working directory. defaults to the current directory. (default:
                       "/Users/shadcn/Code/shadcn/ui/packages/shadcn")
  -h, --help          display help for command
```

----------------------------------------

TITLE: Creating a New Feature Branch (Bash)
DESCRIPTION: This command creates and switches to a new Git branch, named username/my-new-block, specifically for your contribution. It ensures that your changes are isolated and can be easily managed during the development and pull request process.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/blocks.mdx#_snippet_1

LANGUAGE: bash
CODE:
```
git checkout -b username/my-new-block
```

----------------------------------------

TITLE: Importing Badge Component (TSX)
DESCRIPTION: Imports the `Badge` component from the specified path, making it available for use in your React or Next.js application. This is a prerequisite for rendering the component.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/badge.mdx#_snippet_1

LANGUAGE: tsx
CODE:
```
import { Badge } from "@/components/ui/badge"
```

----------------------------------------

TITLE: Customizing shadcn Build Output Directory (Bash)
DESCRIPTION: This example demonstrates how to use the `--output` option with the `build` command to specify a custom destination directory for the generated registry JSON files, overriding the default location.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/cli.mdx#_snippet_6

LANGUAGE: bash
CODE:
```
npx shadcn@latest build --output ./public/registry
```

----------------------------------------

TITLE: Importing Breadcrumb Components in TSX
DESCRIPTION: This snippet shows the necessary imports for the `Breadcrumb` component and its sub-components from the `@/components/ui/breadcrumb` path. These imports are essential for constructing a breadcrumb navigation in a TypeScript React project.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/breadcrumb.mdx#_snippet_1

LANGUAGE: tsx
CODE:
```
import {
  Breadcrumb,
  BreadcrumbItem,
  BreadcrumbLink,
  BreadcrumbList,
  BreadcrumbPage,
  BreadcrumbSeparator,
} from "@/components/ui/breadcrumb"
```

----------------------------------------

TITLE: Configuring Tailwind CSS in Registry Item JSON
DESCRIPTION: This JSON snippet illustrates the use of the `tailwind` property to extend Tailwind CSS configuration within a registry item. It shows how to add custom colors, define keyframes for animations, and link those keyframes to animation utilities. This property is deprecated for Tailwind v4 projects, where `cssVars.theme` should be used instead.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/registry/registry-item-json.mdx#_snippet_10

LANGUAGE: json
CODE:
```
{
  "tailwind": {
    "config": {
      "theme": {
        "extend": {
          "colors": {
            "brand": "hsl(var(--brand))"
          },
          "keyframes": {
            "wiggle": {
              "0%, 100%": { "transform": "rotate(-3deg)" },
              "50%": { "transform": "rotate(3deg)" }
            }
          },
          "animation": {
            "wiggle": "wiggle 1s ease-in-out infinite"
          }
        }
      }
    }
  }
}
```

----------------------------------------

TITLE: Overriding Tailwind CSS v3 Theme Variables in shadcn/ui Registry (JSON)
DESCRIPTION: This JSON snippet illustrates how to override Tailwind CSS v3 theme variables within a `shadcn/ui` registry item. It shows how to extend the `tailwind.config.theme.extend` object to modify existing theme properties, such as `text.base`, providing a way to customize Tailwind's default theme values.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/registry/faq.mdx#_snippet_4

LANGUAGE: json
CODE:
```
{
  "$schema": "https://ui.shadcn.com/schema/registry-item.json",
  "name": "hello-world",
  "title": "Hello World",
  "type": "registry:block",
  "description": "A complex hello world component",
  "files": [
    // ...
  ],
  "tailwind": {
    "config": {
      "theme": {
        "extend": {
          "text": {
            "base": "3rem"
          }
        }
      }
    }
  }
}
```

----------------------------------------

TITLE: Navigating to the Project Directory
DESCRIPTION: This command changes the current directory to the `ui` project root after cloning the repository.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/CONTRIBUTING.md#_snippet_1

LANGUAGE: bash
CODE:
```
cd ui
```

----------------------------------------

TITLE: Specifying Schema for registry.json
DESCRIPTION: This snippet shows how to use the `$schema` property to link the `registry.json` file to its official JSON Schema definition, ensuring validation and auto-completion capabilities.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/registry/registry-json.mdx#_snippet_1

LANGUAGE: json
CODE:
```
{
  "$schema": "https://ui.shadcn.com/schema/registry.json"
}
```

----------------------------------------

TITLE: Adding Custom Component Styles - JSON
DESCRIPTION: This JSON configuration defines component-specific CSS styles using the `@layer components` directive for a shadcn/ui registry item. It styles a `card` component with custom background color, border-radius, padding, and box-shadow, ensuring a consistent look for UI elements.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/registry/examples.mdx#_snippet_9

LANGUAGE: json
CODE:
```
{
  "$schema": "https://ui.shadcn.com/schema/registry-item.json",
  "name": "custom-card",
  "type": "registry:component",
  "css": {
    "@layer components": {
      "card": {
        "background-color": "var(--color-white)",
        "border-radius": "var(--rounded-lg)",
        "padding": "var(--spacing-6)",
        "box-shadow": "var(--shadow-xl)"
      }
    }
  }
}
```

----------------------------------------

TITLE: Defining Simple CSS Utility - JSON
DESCRIPTION: This JSON configuration defines a simple CSS utility class using the `@utility` directive for a shadcn/ui registry item. It creates a `content-auto` utility that sets `content-visibility` to `auto`, optimizing rendering performance for off-screen content.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/registry/examples.mdx#_snippet_10

LANGUAGE: json
CODE:
```
{
  "$schema": "https://ui.shadcn.com/schema/registry-item.json",
  "name": "custom-component",
  "type": "registry:component",
  "css": {
    "@utility content-auto": {
      "content-visibility": "auto"
    }
  }
}
```

----------------------------------------

TITLE: Before: React Component with `forwardRef` in TSX
DESCRIPTION: This snippet shows a React component (`AccordionItem`) before migration, demonstrating the use of `React.forwardRef`. It wraps the primitive component to pass a ref down, allowing parent components to access the underlying DOM element, and includes `displayName` for debugging.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/tailwind-v4.mdx#_snippet_7

LANGUAGE: TSX
CODE:
```
const AccordionItem = React.forwardRef<
  React.ElementRef<typeof AccordionPrimitive.Item>,
  React.ComponentPropsWithoutRef<typeof AccordionPrimitive.Item>
>(({ className, ...props }, ref) => (
  <AccordionPrimitive.Item
    ref={ref}
    className={cn("border-b last:border-b-0", className)}
    {...props}
  />
))
AccordionItem.displayName = "AccordionItem"
```

----------------------------------------

TITLE: Running the shadcn-ui Package Locally
DESCRIPTION: This command starts the development server for the `shadcn-ui` package, enabling local development and testing of the core UI components.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/CONTRIBUTING.md#_snippet_5

LANGUAGE: bash
CODE:
```
pnpm --filter=shadcn-ui dev
```

----------------------------------------

TITLE: Running Shadcn Registry Build Script (Bash)
DESCRIPTION: This command executes the `registry:build` script defined in `package.json`, triggering the `shadcn build` process. This action generates the necessary registry JSON files, typically in the `public/r` directory by default.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/registry/getting-started.mdx#_snippet_6

LANGUAGE: bash
CODE:
```
npm run registry:build
```

----------------------------------------

TITLE: Installing Radix UI Progress Dependency Manually (npm)
DESCRIPTION: This command installs the `@radix-ui/react-progress` package, which is a foundational dependency for the Shadcn UI Progress component. It provides the core primitive for building accessible progress indicators.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/progress.mdx#_snippet_1

LANGUAGE: bash
CODE:
```
npm install @radix-ui/react-progress
```

----------------------------------------

TITLE: Installing Sheet Component Dependencies (Bash)
DESCRIPTION: This snippet shows how to manually install the core dependency, `@radix-ui/react-dialog`, required for the Shadcn UI Sheet component. This is a prerequisite step for integrating the component without using the CLI.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/sheet.mdx#_snippet_1

LANGUAGE: bash
CODE:
```
npm install @radix-ui/react-dialog
```

----------------------------------------

TITLE: Installing Calendar Manual Dependencies (NPM)
DESCRIPTION: This command installs the core npm packages, `react-day-picker` and `date-fns`, which are essential prerequisites for manually setting up the `Calendar` component in your project.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/calendar.mdx#_snippet_1

LANGUAGE: bash
CODE:
```
npm install react-day-picker@8.10.1 date-fns
```

----------------------------------------

TITLE: Building shadcn Registry Files (Bash)
DESCRIPTION: This command generates the necessary registry JSON files by reading the `registry.json` file and outputting them to the `public/r` directory. This process is crucial for making components discoverable and usable.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/cli.mdx#_snippet_4

LANGUAGE: bash
CODE:
```
npx shadcn@latest build
```

----------------------------------------

TITLE: Installing Radix UI Slot Dependency Manually
DESCRIPTION: This command installs @radix-ui/react-slot, a required dependency for the Shadcn UI Button component when performing a manual installation.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/button.mdx#_snippet_1

LANGUAGE: bash
CODE:
```
npm install @radix-ui/react-slot
```

----------------------------------------

TITLE: Installing Radix UI Separator Dependency (npm)
DESCRIPTION: Installs the `@radix-ui/react-separator` package, which is the underlying primitive component for the `shadcn/ui` Separator, as part of a manual installation process.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/separator.mdx#_snippet_1

LANGUAGE: bash
CODE:
```
npm install @radix-ui/react-separator
```

----------------------------------------

TITLE: Testing shadcn CLI Commands in a Specific Application
DESCRIPTION: This command allows testing specific `shadcn` CLI commands (e.g., `init`, `add`) against a designated application directory, specified by the `-c` flag.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/CONTRIBUTING.md#_snippet_9

LANGUAGE: bash
CODE:
```
pnpm shadcn <init | add | ...> -c ~/Desktop/my-app
```

----------------------------------------

TITLE: Specifying Registry Item Type - JSON
DESCRIPTION: This snippet shows the `type` property, which categorizes the registry item (e.g., `registry:block`, `registry:component`, `registry:hook`). This type determines how the item is resolved and its target path within a project.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/registry/registry-item-json.mdx#_snippet_5

LANGUAGE: json
CODE:
```
{
  "type": "registry:block"
}
```

----------------------------------------

TITLE: Specifying Registry Item Author - JSON
DESCRIPTION: This snippet illustrates the `author` property, used to specify the creator of the registry item. This can be an individual's name and email, providing attribution for the component.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/registry/registry-item-json.mdx#_snippet_6

LANGUAGE: json
CODE:
```
{
  "author": "John Doe <john@doe.com>"
}
```

----------------------------------------

TITLE: Running the shadcn.com Website Locally
DESCRIPTION: This command starts the development server for the `www` workspace, which hosts the ui.shadcn.com website. It allows local testing of the main site.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/CONTRIBUTING.md#_snippet_4

LANGUAGE: bash
CODE:
```
pnpm --filter=www dev
```

----------------------------------------

TITLE: Creating Persistent Volumes for Fly.io Apps (Shell)
DESCRIPTION: These commands create persistent volumes named 'data' with a size of 1GB for both the production and staging Fly.io applications. These volumes are used to store the SQLite database, ensuring data persistence across deployments.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/packages/shadcn/test/fixtures/frameworks/remix-indie-stack/README.md#_snippet_9

LANGUAGE: sh
CODE:
```
fly volumes create data --size 1 --app indie-stack-template
fly volumes create data --size 1 --app indie-stack-template-staging
```

----------------------------------------

TITLE: Creating Fly.io Applications (Shell)
DESCRIPTION: These commands create two new applications on Fly.io, one for the production environment (`indie-stack-template`) and one for staging (`indie-stack-template-staging`). The names must match the `app` setting in the `fly.toml` file for successful deployment.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/packages/shadcn/test/fixtures/frameworks/remix-indie-stack/README.md#_snippet_5

LANGUAGE: sh
CODE:
```
fly apps create indie-stack-template
fly apps create indie-stack-template-staging
```

----------------------------------------

TITLE: Running the shadcn CLI in Development Mode
DESCRIPTION: This command starts the development script for the `shadcn` CLI, preparing it for local testing and interaction.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/CONTRIBUTING.md#_snippet_7

LANGUAGE: bash
CODE:
```
pnpm shadcn:dev
```

----------------------------------------

TITLE: Creating an Open in v0 Button Component (React/JSX)
DESCRIPTION: This React component renders a button that, when clicked, opens a specified URL in v0. It uses a custom `Button` component and includes an SVG icon for the v0 logo. The `url` prop is used to construct the target URL for the v0 API endpoint.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/registry/open-in-v0.mdx#_snippet_0

LANGUAGE: jsx
CODE:
```
import { Button } from "@/components/ui/button"

export function OpenInV0Button({ url }: { url: string }) {
  return (
    <Button
      aria-label="Open in v0"
      className="h-8 gap-1 rounded-[6px] bg-black px-3 text-xs text-white hover:bg-black hover:text-white dark:bg-white dark:text-black"
      asChild
    >
      <a
        href={`https://v0.dev/chat/api/open?url=${url}`}
        target="_blank"
        rel="noreferrer"
      >
        Open in{" "}
        <svg
          viewBox="0 0 40 20"
          fill="none"
          xmlns="http://www.w3.org/2000/svg"
          className="h-5 w-5 text-current"
        >
          <path
            d="M23.3919 0H32.9188C36.7819 0 39.9136 3.13165 39.9136 6.99475V16.0805H36.0006V6.99475C36.0006 6.90167 35.9969 6.80925 35.9898 6.71766L26.4628 16.079C26.4949 16.08 26.5272 16.0805 26.5595 16.0805H36.0006V19.7762H26.5595C22.6964 19.7762 19.4788 16.6139 19.4788 12.7508V3.68923H23.3919V12.7508C23.3919 12.9253 23.4054 13.0977 23.4316 13.2668L33.1682 3.6995C33.0861 3.6927 33.003 3.68923 32.9188 3.68923H23.3919V0Z"
            fill="currentColor"
          ></path>
          <path
            d="M13.7688 19.0956L0 3.68759H5.53933L13.6231 12.7337V3.68759H17.7535V17.5746C17.7535 19.6705 15.1654 20.6584 13.7688 19.0956Z"
            fill="currentColor"
          ></path>
        </svg>
      </a>
    </Button>
  )
}
```

----------------------------------------

TITLE: Installing Node.js Type Definitions
DESCRIPTION: This command installs the `@types/node` package as a development dependency, providing TypeScript type definitions for Node.js APIs, which is necessary for resolving paths in `vite.config.ts`.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/installation/vite.mdx#_snippet_5

LANGUAGE: bash
CODE:
```
npm install -D @types/node
```

----------------------------------------

TITLE: Applying Utility Classes for Theming in TSX
DESCRIPTION: This snippet shows how to apply theming using direct Tailwind CSS utility classes, specifically for background colors in light and dark modes. It uses `bg-zinc-950` for light mode and `dark:bg-white` for dark mode. This approach is used when `tailwind.cssVariables` is set to `false` in `components.json`.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/theming.mdx#_snippet_2

LANGUAGE: tsx
CODE:
```
<div className="bg-zinc-950 dark:bg-white" />
```

----------------------------------------

TITLE: Adding Custom Metadata to Registry Items
DESCRIPTION: This snippet illustrates the `meta` property, which allows for the inclusion of arbitrary key-value pairs as additional metadata for a registry item. This property provides flexibility to store any extra information relevant to the item that isn't covered by other predefined properties.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/registry/registry-item-json.mdx#_snippet_15

LANGUAGE: json
CODE:
```
{
  "meta": { "foo": "bar" }
}
```

----------------------------------------

TITLE: Building the Component Registry
DESCRIPTION: This command updates the component registry by building it. It should be run after adding or modifying components to ensure changes are reflected.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/CONTRIBUTING.md#_snippet_12

LANGUAGE: bash
CODE:
```
pnpm build:registry
```

----------------------------------------

TITLE: Defining Functional CSS Utilities - JSON
DESCRIPTION: This JSON configuration defines a functional CSS utility using the `@utility` directive for a shadcn/ui registry item. It creates a `tab-*` utility that dynamically sets `tab-size` based on a CSS variable, allowing for flexible tab indentation.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/registry/examples.mdx#_snippet_12

LANGUAGE: json
CODE:
```
{
  "$schema": "https://ui.shadcn.com/schema/registry-item.json",
  "name": "custom-component",
  "type": "registry:component",
  "css": {
    "@utility tab-*": {
      "tab-size": "var(--tab-size-*)"
    }
  }
}
```

----------------------------------------

TITLE: New Block Directory Structure (Plain Text)
DESCRIPTION: This plain text snippet illustrates the required directory path and naming convention for a new block within the shadcn/ui project. New block folders must be created in kebab-case under apps/www/registry/new-york/blocks.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/blocks.mdx#_snippet_4

LANGUAGE: txt
CODE:
```
apps
└── www
    └── registry
        └── new-york
            └── blocks
                └── dashboard-01
```

----------------------------------------

TITLE: Updating input-otp Package - Bash
DESCRIPTION: This command updates the `input-otp` package to its latest version using npm. This is a crucial first step to ensure compatibility with the new composition pattern and other features.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/input-otp.mdx#_snippet_7

LANGUAGE: bash
CODE:
```
npm install input-otp@latest
```

----------------------------------------

TITLE: Changelog: Updating day_outside Class Color (TypeScript/React)
DESCRIPTION: This snippet illustrates a change in the `day_outside` class definition within the `Calendar` component's styling, specifically modifying its text and background colors to enhance contrast for dates outside the current month.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/calendar.mdx#_snippet_4

LANGUAGE: tsx
CODE:
```
"day_outside:
        "day-outside text-muted-foreground aria-selected:bg-accent/50 aria-selected:text-muted-foreground",
```

----------------------------------------

TITLE: Marking SidebarMenuButton as Active (TSX)
DESCRIPTION: This example illustrates the use of the `isActive` prop to visually mark a `SidebarMenuButton` component as the currently active menu item. This prop applies specific styling to indicate its active state.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/sidebar.mdx#_snippet_29

LANGUAGE: tsx
CODE:
```
<SidebarMenuButton asChild isActive>
  <a href="#">Home</a>
</SidebarMenuButton>
```

----------------------------------------

TITLE: Opting Out of TypeScript via CLI Prompt
DESCRIPTION: This console output shows the interactive prompt from the shadcn/ui CLI during project setup, specifically demonstrating how a user can choose 'no' to opt out of TypeScript, indicating support for JavaScript-only projects.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/changelog.mdx#_snippet_3

LANGUAGE: txt
CODE:
```
Would you like to use TypeScript (recommended)? no
```

----------------------------------------

TITLE: Using the Open in v0 Button Component (React/JSX)
DESCRIPTION: This snippet demonstrates how to use the `OpenInV0Button` component by passing a registry item URL as a prop. This allows for easy integration of the 'Open in v0' functionality into a web application.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/registry/open-in-v0.mdx#_snippet_1

LANGUAGE: jsx
CODE:
```
<OpenInV0Button url="https://example.com/r/hello-world.json" />
```

----------------------------------------

TITLE: Installing React Resizable Panels Dependency (NPM)
DESCRIPTION: This command installs the core `react-resizable-panels` library, which is a prerequisite for using the `Resizable` component. It should be run manually before copying the component source code if not using the CLI installation method.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/resizable.mdx#_snippet_1

LANGUAGE: bash
CODE:
```
npm install react-resizable-panels
```

----------------------------------------

TITLE: Installing Sonner and Dependencies Manually (Bash)
DESCRIPTION: This command installs the `sonner` library and `next-themes` using npm. These dependencies are required for the manual setup of the Sonner component, with `next-themes` likely used for theme integration.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/sonner.mdx#_snippet_2

LANGUAGE: bash
CODE:
```
npm install sonner next-themes
```

----------------------------------------

TITLE: Installing Radix UI Tooltip Dependency (Bash)
DESCRIPTION: This command manually installs the core `@radix-ui/react-tooltip` package, which is a prerequisite for using the `Tooltip` component.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/tooltip.mdx#_snippet_1

LANGUAGE: bash
CODE:
```
npm install @radix-ui/react-tooltip
```

----------------------------------------

TITLE: Capturing Block Screenshots (Bash)
DESCRIPTION: This command automates the process of capturing screenshots of your block, generating both light and dark mode images. These screenshots are necessary for the block's preview on the website and are typically required before submitting a pull request.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/blocks.mdx#_snippet_8

LANGUAGE: bash
CODE:
```
pnpm registry:capture
```

----------------------------------------

TITLE: Running Tests with pnpm
DESCRIPTION: This command executes all tests within the repository using Vitest, ensuring code quality and functionality. It should be run from the repository's root directory before submitting a pull request to verify that all existing and new features are working correctly.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/CONTRIBUTING.md#_snippet_13

LANGUAGE: bash
CODE:
```
pnpm test
```

----------------------------------------

TITLE: Importing and using shadcn/ui Button in a Gatsby React component
DESCRIPTION: Provides an example of importing the `Button` component from `shadcn/ui` and rendering it within a React functional component in a Gatsby application.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/installation/gatsby.mdx#_snippet_7

LANGUAGE: tsx
CODE:
```
import { Button } from "@/components/ui/button"

export default function Home() {
  return (
    <div>
      <Button>Click me</Button>
    </div>
  )
}
```

----------------------------------------

TITLE: Fixing `useSidebar` Hook Error Message Typo in TypeScript
DESCRIPTION: This `diff` snippet shows a correction in the error message for the `useSidebar` hook. It changes the expected parent component from 'Sidebar' to 'SidebarProvider', ensuring the error message accurately reflects the hook's dependency.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/components/sidebar.mdx#_snippet_50

LANGUAGE: diff
CODE:
```
-  throw new Error("useSidebar must be used within a Sidebar.")
+  throw new Error("useSidebar must be used within a SidebarProvider.")
```

----------------------------------------

TITLE: Authenticating with Fly.io CLI (Shell)
DESCRIPTION: This command initiates the authentication process for the Fly.io command-line interface, allowing users to sign up or log in to their Fly.io account. It's a prerequisite for deploying applications to Fly.io.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/packages/shadcn/test/fixtures/frameworks/remix-indie-stack/README.md#_snippet_4

LANGUAGE: sh
CODE:
```
fly auth signup
```

----------------------------------------

TITLE: Running Tests for the shadcn CLI
DESCRIPTION: This command executes the test suite for the `shadcn` CLI package, ensuring its functionality and stability.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/CONTRIBUTING.md#_snippet_10

LANGUAGE: bash
CODE:
```
pnpm --filter=shadcn test
```

----------------------------------------

TITLE: Starting the Registry Development Server for CLI Testing
DESCRIPTION: This command starts the development server for the `www` workspace (registry), ensuring components are up-to-date before testing the CLI.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/CONTRIBUTING.md#_snippet_6

LANGUAGE: bash
CODE:
```
pnpm www:dev
```

----------------------------------------

TITLE: Setting Registry Homepage in registry.json
DESCRIPTION: This snippet demonstrates the `homepage` property, which defines the main URL for your component registry. Like the `name` property, it's used for metadata and informational purposes.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/registry/registry-json.mdx#_snippet_3

LANGUAGE: json
CODE:
```
{
  "homepage": "https://acme.com"
}
```

----------------------------------------

TITLE: Defining Registry Name in registry.json
DESCRIPTION: This snippet illustrates the `name` property, which is used to specify a unique identifier for your component registry. This name is typically used for data attributes and other metadata within the system.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/registry/registry-json.mdx#_snippet_2

LANGUAGE: json
CODE:
```
{
  "name": "acme"
}
```

----------------------------------------

TITLE: Cleaning Up User After Cypress Tests (TypeScript)
DESCRIPTION: This `afterEach` block in Cypress tests ensures that the user created for a test is automatically deleted from the database after each test run. This practice helps maintain a clean local database and ensures test isolation, preventing side effects between tests.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/packages/shadcn/test/fixtures/frameworks/remix-indie-stack/README.md#_snippet_11

LANGUAGE: ts
CODE:
```
afterEach(() => {
  cy.cleanupUser();
});
```

----------------------------------------

TITLE: Specifying Schema for Registry Item - JSON
DESCRIPTION: This snippet demonstrates the use of the `$schema` property, which links the `registry-item.json` file to its defining JSON Schema. This property is crucial for enabling validation and ensuring the file adheres to the expected structure and data types.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/registry/registry-item-json.mdx#_snippet_1

LANGUAGE: json
CODE:
```
{
  "$schema": "https://ui.shadcn.com/schema/registry-item.json"
}
```

----------------------------------------

TITLE: Categorizing Registry Items in JSON
DESCRIPTION: This snippet demonstrates the use of the `categories` property to organize and classify registry items. By assigning categories like 'sidebar' or 'dashboard', items can be easily grouped and filtered, improving discoverability and management within the registry.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/registry/registry-item-json.mdx#_snippet_14

LANGUAGE: json
CODE:
```
{
  "categories": ["sidebar", "dashboard"]
}
```

----------------------------------------

TITLE: Providing CLI Documentation for Registry Items
DESCRIPTION: This snippet shows how to use the `docs` property to provide custom documentation or a message that will be displayed when the registry item is installed via the CLI. This is useful for conveying important setup instructions or reminders to users, such as adding environment variables.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/registry/registry-item-json.mdx#_snippet_13

LANGUAGE: json
CODE:
```
{
  "docs": "Remember to add the FOO_BAR environment variable to your .env file."
}
```

----------------------------------------

TITLE: Defining Registry Item Dependencies - JSON
DESCRIPTION: This snippet shows the `registryDependencies` property, used to specify dependencies on other items within the same or different registries. It supports referencing shadcn/ui components by name or custom registry items by URL, with automatic resolution by the CLI.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/registry/registry-item-json.mdx#_snippet_8

LANGUAGE: json
CODE:
```
{
  "registryDependencies": [
    "button",
    "input",
    "select",
    "https://example.com/r/editor.json"
  ]
}
```

----------------------------------------

TITLE: Testing the shadcn CLI Locally
DESCRIPTION: This command executes the `shadcn` CLI locally, allowing you to interact with it and verify its basic functionality.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/CONTRIBUTING.md#_snippet_8

LANGUAGE: bash
CODE:
```
pnpm shadcn
```

----------------------------------------

TITLE: TanStack Router SVG Icon Definition (JSX/SVG)
DESCRIPTION: Defines the SVG icon for TanStack Router, intended for use within a `LinkedCard` component. It specifies the SVG namespace, viewBox, dimensions, fill color, and the complex path data representing the icon's unique shape.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/installation/index.mdx#_snippet_0

LANGUAGE: XML
CODE:
```
<svg
      xmlns="http://www.w3.org/2000/svg"
      viewBox="0 0 24 24"
      className="w-10 h-10"
      fill="currentColor"
    >
      <path d="M6.93 13.688a.343.343 0 0 1 .468.132l.063.106c.48.851.98 1.66 1.5 2.426a35.65 35.65 0 0 0 2.074 2.742.345.345 0 0 1-.039.484l-.074.066c-2.543 2.223-4.191 2.665-4.953 1.333-.746-1.305-.477-3.672.808-7.11a.344.344 0 0 1 .153-.18ZM17.75 16.3a.34.34 0 0 1 .395.27l.02.1c.628 3.286.187 4.93-1.325 4.93-1.48 0-3.36-1.402-5.649-4.203a.327.327 0 0 1-.074-.222c0-.188.156-.34.344-.34h.121a32.984 32.984 0 0 0 2.809-.098c1.07-.086 2.191-.23 3.359-.437zm.871-6.977a.353.353 0 0 1 .445-.21l.102.034c3.262 1.11 4.504 2.332 3.719 3.664-.766 1.305-2.993 2.254-6.684 2.848a.362.362 0 0 1-.238-.047.343.343 0 0 1-.125-.476l.062-.106a34.07 34.07 0 0 0 1.367-2.523c.477-.989.93-2.051 1.352-3.184zM7.797 8.34a.362.362 0 0 1 .238.047.343.343 0 0 1 .125.476l-.062.106a34.088 34.088 0 0 0-1.367 2.523c-.477.988-.93 2.051-1.352 3.184a.353.353 0 0 1-.445.21l-.102-.034C1.57 13.742.328 12.52 1.113 11.188 1.88 9.883 4.106 8.934 7.797 8.34Zm5.281-3.984c2.543-2.223 4.192-2.664 4.953-1.332.746 1.304.477 3.671-.808 7.109a.344.344 0 0 1-.153.18.343.343 0 0 1-.468-.133l-.063-.106a34.64 34.64 0 0 0-1.5-2.426 35.65 35.65 0 0 0-2.074-2.742.345.345 0 0 1 .039-.484ZM7.285 2.274c1.48 0 3.364 1.402 5.649 4.203a.349.349 0 0 1 .078.218.348.348 0 0 1-.348.344l-.117-.004a34.584 34.584 0 0 0-2.809.102 35.54 35.54 0 0 0-3.363.437.343.343 0 0 1-.394-.273l-.02-.098c-.629-3.285-.188-4.93 1.324-4.93Zm2.871 5.812h3.688a.638.638 0 0 1 .55.316l1.848 3.22a.644.644 0 0 1 0 .628l-1.847 3.223a.638.638 0 0 1-.551.316h-3.688a.627.627 0 0 1-.547-.316L7.758 12.25a.644.644 0 0 1 0-.629L9.61 8.402a.627.627 0 0 1 .546-.316Zm3.23.793a.638.638 0 0 1 .552.316l1.39 2.426a.644.644 0 0 1 0 .629l-1.39 2.43a.638.638 0 0 1-.551.316h-2.774a.627.627 0 0 1-.546-.316l-1.395-2.43a.644.644 0 0 1 0-.629l1.395-2.426a.627.627 0 0 1 .546-.316Zm-.491.867h-1.79a.624.624 0 0 0-.546.316l-.899 1.56a.644.644 0 0 0 0 .628l.899 1.563a.632.632 0 0 0 .547.316h1.789a.632.632 0 0 0 .547-.316l.898-1.563a.644.644 0 0 0 0-.629l-.898-1.558a.624.624 0 0 0-.547-.317Zm-.477.828c.227 0 .438.121.547.317l.422.73a.625.625 0 0 1 0 .629l-.422.734a.627.627 0 0 1-.547.317h-.836a.632.632 0 0 1-.547-.317l-.422-.734a.625.625 0 0 1 0-.629l.422-.73a.632.632 0 0 1 .547-.317zm-.418.817a.548.548 0 0 0-.473.273.547.547 0 0 0 0 .547.544.544 0 0 0 .473.27.544.544 0 0 0 .473-.27.547.547 0 0 0 0-.547.548.548 0 0 0-.473-.273Zm-4.422.546h.98M18.98 7.75c.391-1.895.477-3.344.223-4.398-.148-.63-.422-1.137-.84-1.508-.441-.39-1-.582-1.625-.582-1.035 0-2.12.472-3.281 1.367a14.9 14.9 0 0 0-1.473 1.316 1.206 1.206 0 0 0-.136-.144c-1.446-1.285-2.66-2.082-3.7-2.39-.617-.184-1.195-.2-1.722-.024-.559.187-1.004.574-1.317 1.117-.515.894-.652 2.074-.46 3.527.078.59.214 1.235.402 1.934a1.119 1.119 0 0 0-.215.047C3.008 8.62 1.71 9.269.926 10.015c-.465.442-.77.938-.883 1.481-.113.578 0 1.156.312 1.7.516.894 1.465 1.597 2.817 2.155.543.223 1.156.426 1.844.61a1.023 1.023 0 0 0-.07.226c-.391 1.891-.477 3.344-.223
```

----------------------------------------

TITLE: Defining Detailed Registry Item Description - JSON
DESCRIPTION: This snippet demonstrates the `description` property, which allows for a more detailed explanation of the registry item's purpose and functionality. It provides additional context beyond what the title conveys.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/registry/registry-item-json.mdx#_snippet_4

LANGUAGE: json
CODE:
```
{
  "description": "A simple hello world component."
}
```

----------------------------------------

TITLE: Example `diff` Output for `alert` Component (Diff)
DESCRIPTION: This `diff` snippet shows a specific change to the `alertVariants` definition for the 'alert' component. It highlights a modification from 'relative w-full rounded-lg border' to 'relative w-full pl-12 rounded-lg border', indicating an added left padding.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/changelog.mdx#_snippet_14

LANGUAGE: diff
CODE:
```
const alertVariants = cva(
- "relative w-full rounded-lg border",
+ "relative w-full pl-12 rounded-lg border"
)
```

----------------------------------------

TITLE: Defining Gatsby Logo SVG
DESCRIPTION: This SVG snippet defines the vector graphic for the Gatsby logo. It includes attributes like `viewBox`, `xmlns`, `className` for styling, `fill` color, and a `path` element containing the specific SVG path data for the logo. It is typically used within a `LinkedCard` component to display the Gatsby icon.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/tailwind-v4.mdx#_snippet_0

LANGUAGE: SVG
CODE:
```
<svg
      viewBox="0 0 24 24"
      xmlns="http://www.w3.org/2000/svg"
      className="w-10 h-10"
      fill="currentColor"
    >
      <title>Gatsby</title>
      <path d="M12 0C5.4 0 0 5.4 0 12s5.4 12 12 12 12-5.4 12-12S18.6 0 12 0zm0 2.571c3.171 0 5.915 1.543 7.629 3.858l-1.286 1.115C16.886 5.572 14.571 4.286 12 4.286c-3.343 0-6.171 2.143-7.286 5.143l9.857 9.857c2.486-.857 4.373-3 4.973-5.572h-4.115V12h6c0 4.457-3.172 8.228-7.372 9.17L2.83 9.944C3.772 5.743 7.543 2.57 12 2.57zm-9.429 9.6l9.344 9.258c-2.4-.086-4.801-.943-6.601-2.743-1.8-1.8-2.743-4.201-2.743-6.515z" />
    </svg>
```

----------------------------------------

TITLE: Creating a New Git Branch
DESCRIPTION: This command creates a new Git branch named `my-new-branch` and switches to it. It's recommended to work on a new branch for contributions.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/CONTRIBUTING.md#_snippet_2

LANGUAGE: bash
CODE:
```
git checkout -b my-new-branch
```

----------------------------------------

TITLE: Cloning the shadcn/ui Repository (Bash)
DESCRIPTION: This command initiates the cloning of the official shadcn/ui GitHub repository to your local machine. It is the foundational step for setting up your development environment to contribute new blocks or components.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/blocks.mdx#_snippet_0

LANGUAGE: bash
CODE:
```
git clone https://github.com/shadcn-ui/ui.git
```

----------------------------------------

TITLE: Adding a Remote Git Repository (Shell)
DESCRIPTION: This command adds a new remote repository, typically named 'origin', to the local Git configuration. The `<ORIGIN_URL>` should be replaced with the URL of the GitHub repository where the project will be hosted.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/packages/shadcn/test/fixtures/frameworks/remix-indie-stack/README.md#_snippet_7

LANGUAGE: sh
CODE:
```
git remote add origin <ORIGIN_URL>
```

----------------------------------------

TITLE: Defining React Logo SVG
DESCRIPTION: This SVG snippet defines the vector graphic for the React logo. Similar to the Gatsby logo, it specifies `viewBox`, `xmlns`, `className`, `fill` color, and a complex `path` element for the React icon. It is designed to be embedded in UI components like `LinkedCard` for visual representation.
SOURCE: https://github.com/shadcn-ui/ui/blob/main/apps/www/content/docs/tailwind-v4.mdx#_snippet_1

LANGUAGE: SVG
CODE:
```
<svg
      role="img"
      viewBox="0 0 24 24"
      xmlns="http://www.w3.org/2000/svg"
      className="w-10 h-10"
      fill="currentColor"
    >
      <title>React</title>
      <path d="M14.23 12.004a2.236 2.236 0 0 1-2.235 2.236 2.236 2.236 0 0 1-2.236-2.236 2.236 2.236 0 0 1 2.235-2.236 2.236 2.236 0 0 1 2.236 2.236zm2.648-10.69c-1.346 0-3.107.96-4.888 2.622-1.78-1.653-3.542-2.602-4.887-2.602-.41 0-.783.093-1.106.278-1.375.793-1.683 3.264-.973 6.365C1.98 8.917 0 10.42 0 12.004c0 1.59 1.99 3.097 5.043 4.03-.704 3.113-.39 5.588.988 6.38.32.187.69.275 1.102.275 1.345 0 3.107-.96 4.888-2.624 1.78 1.654 3.542 2.603 4.887 2.603.41 0 .783-.09 1.106-.275 1.374-.792 1.683-3.263.973-6.365C22.02 15.096 24 13.59 24 12.004c0-1.59-1.99-3.097-5.043-4.032.704-3.11.39-5.587-.988-6.38-.318-.184-.688-.277-1.092-.278zm-.005 1.09v.006c.225 0 .406.044.558.127.666.382.955 1.835.73 3.704-.054.46-.142.945-.25 1.44-.96-.236-2.006-.417-3.107-.534-.66-.905-1.345-1.727-2.035-2.447 1.592-1.48 3.087-2.292 4.105-2.295zm-9.77.02c1.012 0 2.514.808 4.11 2.28-.686.72-1.37 1.537-2.02 2.442-1.107.117-2.154.298-3.113.538-.112-.49-.195-.964-.254-1.42-.23-1.868.054-3.32.714-3.707.19-.09.4-.127.563-.132zm4.882 3.05c.455.468.91.992 1.36 1.564-.44-.02-.89-.034-1.345-.034-.46 0-.915.01-1.36.034.44-.572.895-1.096 1.345-1.565zM12 8.1c.74 0 1.477.034 2.202.093.406.582.802 1.203 1.183 1.86.372.64.71 1.29 1.018 1.946-.308.655-.646 1.31-1.013 1.95-.38.66-.773 1.288-1.18 1.87-.728.063-1.466.098-2.21.098-.74 0-1.477-.035-2.202-.093-.406-.582-.802-1.204-1.183-1.86-.372-.64-.71-1.29-1.018-1.946.303-.657.646-1.313 1.013-1.954.38-.66.773-1.286 1.18-1.868.728-.064 1.466-.098 2.21-.098zm-3.635.254c-.24.377-.48.763-.704 1.16-.225.39-.435.782-.635 1.174-.265-.656-.49-1.31-.676-1.947.64-.15 1.315-.283 2.015-.386zm7.26 0c.695.103 1.365.23 2.006.387-.18.632-.405 1.282-.66 1.933-.2-.39-.41-.783-.64-1.174-.225-.392-.465-.774-.705-1.146zm3.063.675c.484.15.944.317 1.375.498 1.732.74 2.852 1.708 2.852 2.476-.005.768-1.125 1.74-2.857 2.475-.42.18-.88.342-1.355.493-.28-.958-.646-1.956-1.1-2.98.45-1.017.81-2.01 1.085-2.964zm-13.395.004c.278.96.645 1.957 1.1 2.98-.45 1.017-.812 2.01-1.086 2.964-.484-.15-.944-.318-1.37-.5-1.732-.737-2.852-1.706-2.852-2.474 0-.768 1.12-1.742 2.852-2.476.42-.18.88-.342 1.356-.494zm11.678 4.28c.265.657.49 1.312.676 1.948-.64.157-1.316.29-2.016.39.24-.375.48-.762.705-1.158.225-.39.435-.788.636-1.18zm-9.945.02c.2.392.41.783.64 1.175.23.39.465.772.705 1.143-.695-.102-1.365-.23-2.006-.386.18-.63.406-1.282.66-1.933zM17.92 16.32c.112.493.2.968.254 1.423.23 1.868-.054 3.32-.714 3.708-.147.09-.338.128-.563.128-1.012 0-2.514-.807-4.11-2.28.686-.72 1.37-1.536 2.02-2.44 1.107-.118 2.154-.3 3.113-.54zm-11.83.01c.96.234 2.006.415 3.107.532.66.905 1.345 1.727 2.035 2.446-1.595 1.483-3.092 2.295-4.11 2.295-.22-.005-.406-.05-.553-.132-.666-.38-.955-1.834-.73-3.703.054-.46.142-.944.25-1.438zm4.56.64c.44.02.89.034 1.345.034.46 0 .915-.01 1.36-.034-.44.572-.895 1.095-1.345 1.565-.455-.47-.91-.993-1.36-1.565z" />
    </svg>
```