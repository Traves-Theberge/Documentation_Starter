TITLE: Updating Tailwind CSS to Latest Version via npm
DESCRIPTION: This Bash command updates the Tailwind CSS package to its latest stable version using npm. It is a prerequisite for accessing new features and improvements introduced in recent releases, such as the `forced-color-adjust` utilities in v3.4.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/blog/tailwindcss-v3-4/index.mdx#_snippet_16

LANGUAGE: Bash
CODE:
```
$ npm install tailwindcss@latest
```

----------------------------------------

TITLE: Initializing Tailwind CSS Configuration in ES Module
DESCRIPTION: This JavaScript code initializes the Tailwind CSS configuration in an ES module. It defines the content, theme, and plugins for Tailwind CSS. The `export default` statement makes the configuration available for use in other modules.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/blog/tailwindcss-v3-3/index.mdx#_snippet_1

LANGUAGE: JavaScript
CODE:
```
/** @type {import('tailwindcss').Config} */
export default {
  content: [],
  theme: {
    extend: {},
  },
  plugins: [],
};
```

----------------------------------------

TITLE: Applying Single-Side Padding with Tailwind CSS
DESCRIPTION: This HTML snippet illustrates how to use specific Tailwind CSS utilities like `pt-<number>`, `pr-<number>`, `pb-<number>`, and `pl-<number>` to control padding on individual sides of an element. Each example targets a different side (top, right, bottom, left) to provide granular control over spacing.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/padding.mdx#_snippet_2

LANGUAGE: html
CODE:
```
<!-- [!code classes:pt-6,pr-4,pb-8,pl-2] -->\n<div class="pt-6 ...">pt-6</div>\n<div class="pr-4 ...">pr-4</div>\n<div class="pb-8 ...">pb-8</div>\n<div class="pl-2 ...">pl-2</div>
```

----------------------------------------

TITLE: Leveraging Modern CSS Features in Tailwind CSS v4.0
DESCRIPTION: This snippet demonstrates how Tailwind CSS v4.0 utilizes modern CSS features like native cascade layers (@layer), color-mix() for color manipulation, and registered custom properties (@property) for advanced styling capabilities. It shows examples of utility classes defined using these features, such as margin-inline for logical properties and color-mix() for opacity adjustments.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/blog/tailwindcss-v4/index.mdx#_snippet_3

LANGUAGE: CSS
CODE:
```
@layer theme, base, components, utilities;

@layer utilities {
  .mx-6 {
    margin-inline: calc(var(--spacing) * 6);
  }
  .bg-blue-500\/50 {
    background-color: color-mix(in oklab, var(--color-blue-500) 50%, transparent);
  }
}

@property --tw-gradient-from {
  syntax: "<color>";
  inherits: false;
  initial-value: #0000;
}
```

----------------------------------------

TITLE: Overriding Dark Mode Variant with Class Selector in CSS
DESCRIPTION: This CSS snippet overrides Tailwind CSS's default `dark` variant behavior. Instead of relying on `prefers-color-scheme`, it configures `dark:*` utilities to apply when an element with the `.dark` class (or any of its descendants) is present in the HTML tree. This allows for manual dark mode toggling via class manipulation.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/dark-mode.mdx#_snippet_1

LANGUAGE: css
CODE:
```
@import "tailwindcss";

@custom-variant dark (&:where(.dark, .dark *));
```

----------------------------------------

TITLE: Importing Tailwind CSS Base Layers
DESCRIPTION: This CSS snippet demonstrates the core imports when `tailwindcss` is included in a project. It defines the CSS layers for theme, base, components, and utilities, and then imports the respective CSS files into their designated layers, setting up the foundational structure for Tailwind CSS.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/theme.mdx#_snippet_8

LANGUAGE: css
CODE:
```
@layer theme, base, components, utilities;

@import "./theme.css" layer(theme);
@import "./preflight.css" layer(base);
@import "./utilities.css" layer(utilities);
```

----------------------------------------

TITLE: Importing Tailwind CSS v4.0 in CSS
DESCRIPTION: This CSS line imports the entire Tailwind CSS framework into a project's stylesheet. In v4.0, this single @import rule replaces the previous @tailwind directives, streamlining the process of including Tailwind's base styles, components, and utilities.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/blog/tailwindcss-v4/index.mdx#_snippet_6

LANGUAGE: CSS
CODE:
```
@import "tailwindcss";
```

----------------------------------------

TITLE: Installing Headless UI v2.0 for React
DESCRIPTION: This command installs the latest version of the `@headlessui/react` package from npm, adding Headless UI v2.0 to your project. It is the first step to integrate the library and access its new features.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/blog/headless-ui-v2/index.mdx#_snippet_0

LANGUAGE: sh
CODE:
```
npm install @headlessui/react@latest
```

----------------------------------------

TITLE: Implementing Min-Width Container Queries with Tailwind CSS
DESCRIPTION: This snippet demonstrates how to use native container queries in Tailwind CSS v4.0 without the need for a plugin. It applies a `@container` class to the parent element and uses `@sm:` and `@lg:` variants to define responsive grid column changes based on the container's width.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/blog/tailwindcss-v4/index.mdx#_snippet_16

LANGUAGE: HTML
CODE:
```
<div class="@container">
  <div class="grid grid-cols-1 @sm:grid-cols-3 @lg:grid-cols-4">
    <!-- ... -->
  </div>
</div>
```

----------------------------------------

TITLE: Applying Dark Mode Styles to HTML Elements with Tailwind CSS
DESCRIPTION: This HTML snippet demonstrates how to apply dark mode specific styles using Tailwind CSS's `dark` variant. It shows how to change background colors (`dark:bg-gray-800`), text colors (`dark:text-white`, `dark:text-gray-400`), and other properties when the user's system is in dark mode. The `dark:` prefix conditionally applies the utility class based on the `prefers-color-scheme` media query.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/dark-mode.mdx#_snippet_0

LANGUAGE: HTML
CODE:
```
<!-- [!code word:dark\:bg-gray-800] -->
<!-- prettier-ignore -->
<div class="bg-white dark:bg-gray-800 rounded-lg px-6 py-8 ring shadow-xl ring-gray-900/5">
  <div>
    <span class="inline-flex items-center justify-center rounded-md bg-indigo-500 p-2 shadow-lg">
      <svg class="h-6 w-6 stroke-white" ...>
        <!-- ... -->
      </svg>
    </span>
  </div>
  <!-- prettier-ignore -->
  <!-- [!code word:dark\:text-white] -->
  <h3 class="text-gray-900 dark:text-white mt-5 text-base font-medium tracking-tight ">Writes upside-down</h3>
  <!-- prettier-ignore -->
  <!-- [!code word:dark\:text-gray-400] -->
  <p class="text-gray-500 dark:text-gray-400 mt-2 text-sm ">
    The Zero Gravity Pen can be used to write in any orientation, including upside-down. It even works in outer space.
  </p>
</div>
```

----------------------------------------

TITLE: Installing Tailwind CSS v3.1 via npm
DESCRIPTION: This command installs the latest version of Tailwind CSS using npm, updating an existing project or setting up a new one with the most recent features and bug fixes.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/blog/tailwindcss-v3-1/index.mdx#_snippet_0

LANGUAGE: sh
CODE:
```
npm install tailwindcss@latest
```

----------------------------------------

TITLE: Correct Mobile-First Text Alignment with Tailwind CSS - HTML
DESCRIPTION: This HTML snippet showcases the recommended mobile-first approach for responsive text alignment in Tailwind CSS. `text-center` applies centering to all screen sizes by default, while `sm:text-left` then overrides this to left-align text specifically on screens 640px and wider, demonstrating proper progressive enhancement.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/responsive-design.mdx#_snippet_6

LANGUAGE: HTML
CODE:
```
<!-- This will center text on mobile, and left align it on screens 640px and wider -->
<div class="text-center sm:text-left"></div>
```

----------------------------------------

TITLE: Responsive Layout for Marketing Component in HTML
DESCRIPTION: This HTML snippet demonstrates building a responsive marketing page component using Tailwind CSS. It utilizes md:flex to switch from a stacked layout on small screens to a side-by-side layout on medium screens, adjusting image dimensions and container width accordingly.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/responsive-design.mdx#_snippet_4

LANGUAGE: HTML
CODE:
```
<div class="mx-auto max-w-md overflow-hidden rounded-xl bg-white shadow-md md:max-w-2xl">
  <div class="md:flex">
    <div class="md:shrink-0">
      <img
        class="h-48 w-full object-cover md:h-full md:w-48"
        src="/img/building.jpg"
        alt="Modern building architecture"
      />
    </div>
    <div class="p-8">
      <div class="text-sm font-semibold tracking-wide text-indigo-500 uppercase">Company retreats</div>
      <a href="#" class="mt-1 block text-lg leading-tight font-medium text-black hover:underline">
        Incredible accommodation for your team
      </a>
      <p class="mt-2 text-gray-500">
```

----------------------------------------

TITLE: Applying Prefixed Utility Classes in Tailwind CSS v4 HTML
DESCRIPTION: In Tailwind CSS v4, prefixes are applied at the beginning of class names, similar to variants. This HTML snippet illustrates how to use a configured prefix (e.g., `tw:`) with utility classes like `tw:flex`, `tw:bg-red-500`, and `tw:hover:bg-red-600`.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/upgrade-guide.mdx#_snippet_33

LANGUAGE: HTML
CODE:
```
<div class="tw:flex tw:bg-red-500 tw:hover:bg-red-600">
  <!-- ... -->
</div>
```

----------------------------------------

TITLE: Ensuring Accessibility for Unstyled Lists with ARIA Role
DESCRIPTION: This HTML snippet addresses accessibility concerns for unstyled lists by adding `role="list"` to the `<ul>` element. This ensures that screen readers like VoiceOver correctly announce the element as a list, even when `list-style: none` is applied, maintaining semantic meaning for assistive technologies.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/preflight.mdx#_snippet_7

LANGUAGE: HTML
CODE:
```
<ul role="list">
  <li>One</li>
  <li>Two</li>
  <li>Three</li>
</ul>
```

----------------------------------------

TITLE: Creating Dynamic Dialog Transitions with Headless UI and React
DESCRIPTION: This example illustrates how to implement a dialog with distinct enter and leave transition styles using stacked data attributes. It uses `data-[closed]:data-[enter]:-translate-x-8` for entering from the left and `data-[closed]:data-[leave]:translate-x-8` for leaving to the right, showcasing advanced transition control.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/blog/2024-06-21-headless-ui-v2-1/index.mdx#_snippet_1

LANGUAGE: jsx
CODE:
```
import { Dialog } from "@headlessui/react";
import { useState } from "react";

function Example() {
  let [isOpen, setIsOpen] = useState(false);

  return (
    <>
      <button onClick={() => setIsOpen(true)}>Open dialog</button>
      <Dialog
        open={isOpen}
        onClose={() => setIsOpen(false)}
        // [!code highlight:8]
        transition
        className={`
          transition duration-300 ease-out
          data-[closed]:opacity-0
          data-[closed]:data-[enter]:-translate-x-8
          data-[closed]:data-[leave]:translate-x-8
        `}
      >
        {/* Dialog content… */}
      </Dialog>
    </>
  );
}
```

----------------------------------------

TITLE: Sorting Tailwind CSS Classes with Prettier in HTML
DESCRIPTION: This snippet illustrates the automatic class sorting functionality provided by the official Prettier plugin for Tailwind CSS. It demonstrates how the plugin reorders utility classes within an HTML element's `class` attribute to follow a consistent and recommended order, improving readability and maintainability. The 'Before' state shows unsorted classes, while the 'After' state displays the same classes sorted by the plugin.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/editor-setup.mdx#_snippet_0

LANGUAGE: HTML
CODE:
```
<!-- Before -->
<button class="text-white px-4 sm:px-8 py-2 sm:py-3 bg-sky-700 hover:bg-sky-800">Submit</button>

<!-- After -->
<button class="bg-sky-700 px-4 py-2 text-white hover:bg-sky-800 sm:px-8 sm:py-3">Submit</button>
```

----------------------------------------

TITLE: Adding Single-Side Margin with Tailwind CSS (HTML)
DESCRIPTION: This snippet demonstrates how to apply margin to a single side of an element using Tailwind CSS utilities. It uses `mt-<number>` for top margin, `mr-<number>` for right margin, `mb-<number>` for bottom margin, and `ml-<number>` for left margin. The number indicates the margin size.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/margin.mdx#_snippet_2

LANGUAGE: HTML
CODE:
```
<!-- [!code classes:mt-6,mr-4,mb-8,ml-2] -->
<div class="mt-6 ...">mt-6</div>
<div class="mr-4 ...">mr-4</div>
<div class="mb-8 ...">mb-8</div>
<div class="ml-2 ...">ml-2</div>
```

----------------------------------------

TITLE: Using Dynamic Grid Column Utilities - HTML
DESCRIPTION: This HTML snippet demonstrates the enhanced flexibility of Tailwind CSS v4.0, allowing dynamic values for utilities like `grid-cols-*`. Users can now specify arbitrary column counts (e.g., `grid-cols-15`) without prior configuration, simplifying grid layout creation.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/blog/tailwindcss-v4/index.mdx#_snippet_13

LANGUAGE: HTML
CODE:
```
<div class="grid grid-cols-15">
  <!-- ... -->
</div>
```

----------------------------------------

TITLE: Defining Minimum Width Container Query for Large Breakpoint in CSS
DESCRIPTION: This CSS snippet defines a container query that applies styles when the container's width is greater than or equal to 32rem (512px). It corresponds to the `@lg` container breakpoint in Tailwind CSS, enabling responsive design based on parent container dimensions.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/hover-focus-and-other-states.mdx#_snippet_171

LANGUAGE: CSS
CODE:
```
@container (width >= 32rem)
```

----------------------------------------

TITLE: Defining Minimum Width Container Query for 5XL Breakpoint in CSS
DESCRIPTION: This CSS snippet defines a container query that applies styles when the container's width is greater than or equal to 64rem (1024px). It corresponds to the `@5xl` container breakpoint in Tailwind CSS, enabling responsive design based on parent container dimensions.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/hover-focus-and-other-states.mdx#_snippet_176

LANGUAGE: CSS
CODE:
```
@container (width >= 64rem)
```

----------------------------------------

TITLE: Styling Hover State with Tailwind (HTML)
DESCRIPTION: Shows how to apply styles to an element's hover state using Tailwind CSS state variants in pure HTML. The `hover:` prefix is used before a utility class (e.g., `hover:bg-sky-700`) to apply that style only when the element is hovered, serving as the HTML equivalent of the React example.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/styling-with-utility-classes.mdx#_snippet_5

LANGUAGE: html
CODE:
```
<!-- [!code word:hover\:bg-sky-700] -->
<button class="bg-sky-500 hover:bg-sky-700 ...">Save changes</button>
```

----------------------------------------

TITLE: Defining Minimum Width Container Query for 6XL Breakpoint in CSS
DESCRIPTION: This CSS snippet defines a container query that applies styles when the container's width is greater than or equal to 72rem (1152px). It corresponds to the `@6xl` container breakpoint in Tailwind CSS, enabling responsive design based on parent container dimensions.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/hover-focus-and-other-states.mdx#_snippet_177

LANGUAGE: CSS
CODE:
```
@container (width >= 72rem)
```

----------------------------------------

TITLE: Defining `@3xs` Container Query in CSS
DESCRIPTION: This CSS container query applies styles when the container's width is 16rem (256px) or greater. Unlike media queries that target the viewport, container queries enable component-level responsiveness, allowing elements to adapt to their immediate parent's dimensions.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/hover-focus-and-other-states.mdx#_snippet_166

LANGUAGE: CSS
CODE:
```
@container (width >= 16rem)
```

----------------------------------------

TITLE: Applying Focus Ring Utilities in HTML
DESCRIPTION: This HTML snippet demonstrates the new `ring` utilities in Tailwind CSS, which provide a better alternative to the `outline` property for focus styles. Classes like `focus:ring-2`, `focus:ring-blue-300`, and `focus:ring-opacity-50` apply a customizable box-shadow ring on focus without affecting layout, improving accessibility and aesthetics.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/blog/tailwindcss-v2/index.mdx#_snippet_9

LANGUAGE: html
CODE:
```
<button class="focus:ring-opacity-50 focus:ring-2 focus:ring-blue-300 focus:outline-none ...">
  <!-- ... -->
</button>
```

----------------------------------------

TITLE: Importing Tailwind CSS in Main CSS File (CLI Flow) - CSS
DESCRIPTION: This CSS snippet imports the Tailwind CSS framework into your main application stylesheet, preparing it for compilation using the Tailwind CSS CLI.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/blog/tailwindcss-v4-alpha/index.mdx#_snippet_18

LANGUAGE: css
CODE:
```
/* [!code filename:app.css] */
@import "tailwindcss";
```

----------------------------------------

TITLE: Replacing Deprecated border-opacity-* with Tailwind CSS Opacity Modifiers
DESCRIPTION: This snippet demonstrates replacing the deprecated `border-opacity-*` utility with the modern Tailwind CSS opacity modifier syntax, such as `border-black/50`, for controlling border color opacity.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/upgrade-guide.mdx#_snippet_7

LANGUAGE: Tailwind CSS
CODE:
```
border-opacity-*
```

LANGUAGE: Tailwind CSS
CODE:
```
border-black/50
```

----------------------------------------

TITLE: Adding Viewport Meta Tag in HTML
DESCRIPTION: This snippet demonstrates how to include the viewport meta tag in the <head> of an HTML document. This tag is crucial for proper responsive behavior across different devices, ensuring the page scales correctly.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/responsive-design.mdx#_snippet_2

LANGUAGE: HTML
CODE:
```
<meta name="viewport" content="width=device-width, initial-scale=1.0" />
```

----------------------------------------

TITLE: Targeting Container Query Ranges in Tailwind CSS HTML
DESCRIPTION: This HTML snippet shows how to target a specific range of container sizes by combining regular and max-width container query variants. The `@sm:@max-md:flex-col` class applies a style only when the container is between its small and medium breakpoints.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/responsive-design.mdx#_snippet_16

LANGUAGE: html
CODE:
```
<div class="@container">
  <div class="flex flex-row @sm:@max-md:flex-col">
    <!-- ... -->
  </div>
</div>
```

----------------------------------------

TITLE: Mapping Props to Static Class Name Variants in JSX with Tailwind
DESCRIPTION: This JSX code further illustrates the best practice of mapping component props to complete, static Tailwind class names. This approach allows for flexible styling, such as applying different color shades or text colors based on a single prop, while ensuring all class names are detectable by Tailwind's plain-text scanner.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/detecting-classes-in-source-files.mdx#_snippet_5

LANGUAGE: JSX
CODE:
```
function Button({ color, children }) {
  const colorVariants = {
    blue: "bg-blue-600 hover:bg-blue-500 text-white",
    red: "bg-red-500 hover:bg-red-400 text-white",
    yellow: "bg-yellow-300 hover:bg-yellow-400 text-black"
  };

  return <button className={`${colorVariants[color]} ...`}>{children}</button>;
}
```

----------------------------------------

TITLE: Migrating Vite Configuration to Tailwind CSS v4 Plugin (TypeScript)
DESCRIPTION: This snippet illustrates how to configure Vite to use the new dedicated `@tailwindcss/vite` plugin for Tailwind CSS v4. This migration is recommended for Vite users to achieve improved performance and a better developer experience. It involves importing the new plugin and adding it to the `plugins` array in `vite.config.ts`.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/upgrade-guide.mdx#_snippet_2

LANGUAGE: ts
CODE:
```
import { defineConfig } from "vite";
import tailwindcss from "@tailwindcss/vite";

export default defineConfig({
  plugins: [
    tailwindcss()
  ]
});
```

----------------------------------------

TITLE: Configuring PostCSS for Tailwind CSS v4.0 - JavaScript
DESCRIPTION: This JavaScript snippet for `postcss.config.js` demonstrates the simplified configuration for Tailwind CSS v4.0. It shows that the `postcss-import` plugin is no longer needed, as Tailwind CSS now provides built-in `@import` support, streamlining the PostCSS setup.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/blog/tailwindcss-v4/index.mdx#_snippet_10

LANGUAGE: JavaScript
CODE:
```
export default {
  plugins: [
    "@tailwindcss/postcss"
  ]
};
```

----------------------------------------

TITLE: Configuring Design Tokens with @theme - CSS
DESCRIPTION: This CSS snippet illustrates the new CSS-first configuration approach in Tailwind CSS v4.0, using the `@theme` directive. It allows defining design tokens like fonts, breakpoints, colors, and easing functions directly within the CSS file, eliminating the need for a separate `tailwind.config.js`.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/blog/tailwindcss-v4/index.mdx#_snippet_11

LANGUAGE: CSS
CODE:
```
@import "tailwindcss";

@theme {
  --font-display: "Satoshi", "sans-serif";

  --breakpoint-3xl: 1920px;

  --color-avocado-100: oklch(0.99 0 0);
  --color-avocado-200: oklch(0.98 0.04 113.22);
  --color-avocado-300: oklch(0.94 0.11 115.03);
  --color-avocado-400: oklch(0.92 0.19 114.08);
  --color-avocado-500: oklch(0.84 0.18 117.33);
  --color-avocado-600: oklch(0.53 0.12 118.34);

  --ease-fluid: cubic-bezier(0.3, 0, 0, 1);
  --ease-snappy: cubic-bezier(0.2, 0, 0, 1);

  /* ... */
}
```

----------------------------------------

TITLE: Defining Custom Theme Variables with @theme in CSS
DESCRIPTION: This CSS snippet shows how to define custom theme values like font families, breakpoints, and colors directly within a main.css file using the @theme directive. These CSS variables replace the need for a JavaScript configuration file, making Tailwind CSS feel more CSS-native.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/blog/tailwindcss-v4-alpha/index.mdx#_snippet_4

LANGUAGE: css
CODE:
```
/* [!code filename:main.css] */
@import "tailwindcss";

@theme {
  --font-family-display: "Satoshi", "sans-serif";

  --breakpoint-3xl: 1920px;

  --color-neon-pink: oklch(71.7% 0.25 360);
  --color-neon-lime: oklch(91.5% 0.258 129);
  --color-neon-cyan: oklch(91.3% 0.139 195.8);
}
```

----------------------------------------

TITLE: Defining a Custom CSS Component Class with Tailwind Layers
DESCRIPTION: Illustrates how to create a custom CSS class (`.btn-primary`) using Tailwind's `@layer components` directive and CSS variables derived from the Tailwind theme. This approach is useful for simpler components where a full template partial might be overkill. The HTML shows how to apply this custom class.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/styling-with-utility-classes.mdx#_snippet_31

LANGUAGE: html
CODE:
```
<!-- [!code filename:HTML] -->
<button class="btn-primary">Save changes</button>
```

LANGUAGE: css
CODE:
```
/* [!code filename:CSS] */
@import "tailwindcss";

@layer components {
  .btn-primary {
    border-radius: calc(infinity * 1px);
    background-color: var(--color-violet-500);
    padding-inline: --spacing(5);
    padding-block: --spacing(2);
    font-weight: var(--font-weight-semibold);
    color: var(--color-white);
    box-shadow: var(--shadow-md);
    &:hover {
      @media (hover: hover) {
        background-color: var(--color-violet-700);
      }
    }
  }
}
```

----------------------------------------

TITLE: Transitioning Outline Color with Tailwind CSS HTML
DESCRIPTION: Demonstrates how to handle `outline-color` transitions in Tailwind CSS v4. The first button shows the default transition behavior, while the second button explicitly sets the outline color unconditionally to prevent unwanted transitions from the default color.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/upgrade-guide.mdx#_snippet_42

LANGUAGE: HTML
CODE:
```
<button class="transition hover:outline-2 hover:outline-cyan-500"></button>
<button class="outline-cyan-500 transition hover:outline-2"></button>
```

----------------------------------------

TITLE: Styling with peer-has-checked in JSX/React
DESCRIPTION: This JSX snippet demonstrates how to use Tailwind CSS's `peer` class and `peer-has-checked` variant to conditionally hide an element. The `div` containing the SVG is hidden when the sibling `label`'s descendant `input` (which is also marked `peer`) is checked. It shows a practical example of a to-do list item where a delete icon disappears upon checking the task.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/hover-focus-and-other-states.mdx#_snippet_19

LANGUAGE: JSX
CODE:
```
<div className="mx-auto max-w-md border-x border-x-gray-200 px-4 py-6 dark:border-x-gray-800 dark:bg-gray-950/10">
      <fieldset className="space-y-3">
        <legend className="text-base font-semibold text-gray-900 dark:text-white">Today</legend>
        <div className="grid grid-cols-[1fr_24px] items-center gap-6">
          <label className="peer grid grid-cols-[auto_1fr] items-center gap-3 rounded-md px-2 hover:bg-gray-100 dark:hover:bg-white/5">
            <input
              type="checkbox"
              className="peer size-3.5 appearance-none rounded-sm border border-gray-300 accent-pink-500 checked:appearance-auto dark:border-gray-600 dark:accent-pink-600"
              defaultChecked
            />
            <span className="text-gray-700 select-none peer-checked:text-gray-400 peer-checked:line-through dark:text-gray-300">
              Create a to do list
            </span>
          </label>
          <div className="size-[26px] rounded-md p-1 peer-has-checked:hidden hover:bg-red-50 hover:text-pink-500">
            <svg fill="none" viewBox="0 0 24 24" strokeWidth="1.5" stroke="currentColor">
              <path strokeLinecap="round" strokeLinejoin="round" d="M6 18L18 6M6 6l12 12" />
            </svg>
          </div>
        </div>
        <div className="grid grid-cols-[1fr_24px] items-center gap-6">
          <label className="peer grid grid-cols-[auto_1fr] items-center gap-3 rounded-md px-2 hover:bg-gray-100 dark:hover:bg-white/5">
            <input
              type="checkbox"
              className="peer size-3.5 appearance-none rounded-sm border border-gray-300 accent-pink-500 checked:appearance-auto dark:border-gray-600 dark:accent-pink-600"
            />
            <span className="text-gray-700 select-none peer-checked:text-gray-400 peer-checked:line-through dark:text-gray-300">
              Check off first item
            </span>
          </label>
          <div className="size-[26px] rounded-md p-1 peer-has-checked:hidden hover:bg-red-50 hover:text-pink-500">
            <svg fill="none" viewBox="0 0 24 24" strokeWidth="1.5" stroke="currentColor">
              <path strokeLinecap="round" strokeLinejoin="round" d="M6 18L18 6M6 6l12 12" />
            </svg>
          </div>
        </div>
        <div className="grid grid-cols-[1fr_24px] items-center gap-6">
          <label className="peer grid grid-cols-[auto_1fr] items-center gap-3 rounded-md px-2 hover:bg-gray-100 dark:hover:bg-white/5">
            <input
              type="checkbox"
              className="peer size-3.5 appearance-none rounded-sm border border-gray-300 accent-pink-500 checked:appearance-auto dark:border-gray-600 dark:accent-pink-600"
            />
            <span className="text-gray-700 select-none peer-checked:text-gray-400 peer-checked:line-through dark:text-gray-300">
              Investigate race condition
            </span>
          </label>
          <div className="block size-[26px] rounded-md p-1 peer-has-checked:hidden hover:bg-red-50 hover:text-pink-500">
            <svg fill="none" viewBox="0 0 24 24" strokeWidth="1.5" stroke="currentColor">
              <path strokeLinecap="round" strokeLinejoin="round" d="M6 18L18 6M6 6l12 12" />
            </svg>
          </div>
        </div>
      </fieldset>
    </div>
```

----------------------------------------

TITLE: Avoiding Dynamic Class Names from Props in JSX with Tailwind
DESCRIPTION: This JSX code shows an incorrect method of constructing Tailwind class names using string interpolation with component props. Since Tailwind scans files as plain text, it cannot resolve `bg-${color}-600` into complete class names like `bg-blue-600`, resulting in missing styles.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/detecting-classes-in-source-files.mdx#_snippet_3

LANGUAGE: JSX
CODE:
```
function Button({ color, children }) {
  return <button className={`bg-${color}-600 hover:bg-${color}-500 ...`}>{children}</button>;
}
```

----------------------------------------

TITLE: Simplifying HTML Form Input with Headless UI React
DESCRIPTION: This snippet shows how Headless UI v2.0 simplifies the creation of accessible form fields. By using `Field`, `Label`, `Input`, and `Description` components, the tedious work of wiring up IDs and `aria-*` attributes is automated, resulting in cleaner and more maintainable code.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/blog/headless-ui-v2/index.mdx#_snippet_4

LANGUAGE: jsx
CODE:
```
import { Description, Field, Input, Label } from "@headlessui/react";

function Example() {
  return (
    <Field>
      <Label>Name</Label>
      <Input name="your_name" />
      <Description>Use your real name so people will recognize you.</Description>
    </Field>
  );
}
```

----------------------------------------

TITLE: Implementing Basic Container Queries in Tailwind CSS HTML
DESCRIPTION: This HTML snippet illustrates the basic usage of Tailwind CSS container queries. It marks a parent element with `@container` and then applies responsive styles to its children using `@md:flex-row`, which changes the flex direction when the container reaches its medium size.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/responsive-design.mdx#_snippet_14

LANGUAGE: html
CODE:
```
<div class="@container">
  <div class="flex flex-col @md:flex-row">
    <!-- ... -->
  </div>
</div>
```

----------------------------------------

TITLE: Using Arbitrary Container Query Values in HTML
DESCRIPTION: This HTML snippet demonstrates the use of arbitrary container query values, such as `@min-[475px]`, for one-off responsive styling without modifying the theme. When the `@container` element's width is at least `475px`, the child element's flex direction will change from `flex-col` to `flex-row`, providing flexible, on-the-fly breakpoint control.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/responsive-design.mdx#_snippet_20

LANGUAGE: html
CODE:
```
<div class="@container">
  <div class="flex flex-col @min-[475px]:flex-row">
    <!-- ... -->
  </div>
</div>
```

----------------------------------------

TITLE: Applying Max-width Container Queries in Tailwind CSS HTML
DESCRIPTION: This HTML snippet demonstrates how to use max-width container query variants in Tailwind CSS. The `@max-md:flex-col` class applies a style (changes flex direction to column) when the container's width is below its medium breakpoint.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/responsive-design.mdx#_snippet_15

LANGUAGE: html
CODE:
```
<div class="@container">
  <div class="flex flex-row @max-md:flex-col">
    <!-- ... -->
  </div>
</div>
```

----------------------------------------

TITLE: Applying box-border Utility in HTML
DESCRIPTION: Demonstrates the use of the `box-border` utility in HTML to set an element's `box-sizing` to `border-box`. This ensures that an element's specified width and height include its padding and border, affecting the internal content area. It illustrates how a 128px x 128px element with padding and border maintains its declared dimensions.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/box-sizing.mdx#_snippet_0

LANGUAGE: html
CODE:
```
<!-- [!code classes:box-border] -->
<div class="box-border size-32 border-4 p-4 ...">
  <!-- ... -->
</div>
```

----------------------------------------

TITLE: Basic Element Positioning with Tailwind CSS in HTML
DESCRIPTION: This HTML snippet illustrates how to use Tailwind CSS positioning utilities such as `top-0`, `left-0`, `inset-x-0`, `inset-y-0`, `inset-0`, `right-0`, and `bottom-0` to control the placement of absolutely positioned child elements within a relative parent. It covers pinning to corners, spanning edges, and filling the parent.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/top-right-bottom-left.mdx#_snippet_1

LANGUAGE: HTML
CODE:
```
<!-- [!code classes:inset-x-0,inset-y-0,inset-0,right-0,left-0,top-0,bottom-0] -->
<!-- Pin to top left corner -->
<div class="relative size-32 ...">
  <div class="absolute top-0 left-0 size-16 ...">01</div>
</div>

<!-- Span top edge -->
<div class="relative size-32 ...">
  <div class="absolute inset-x-0 top-0 h-16 ...">02</div>
</div>

<!-- Pin to top right corner -->
<div class="relative size-32 ...">
  <div class="absolute top-0 right-0 size-16 ...">03</div>
</div>

<!-- Span left edge -->
<div class="relative size-32 ...">
  <div class="absolute inset-y-0 left-0 w-16 ...">04</div>
</div>

<!-- Fill entire parent -->
<div class="relative size-32 ...">
  <div class="absolute inset-0 ...">05</div>
</div>

<!-- Span right edge -->
<div class="relative size-32 ...">
  <div class="absolute inset-y-0 right-0 w-16 ...">06</div>
</div>

<!-- Pin to bottom left corner -->
<div class="relative size-32 ...">
  <div class="absolute bottom-0 left-0 size-16 ...">07</div>
</div>

<!-- Span bottom edge -->
<div class="relative size-32 ...">
  <div class="absolute inset-x-0 bottom-0 h-16 ...">08</div>
</div>

<!-- Pin to bottom right corner -->
<div class="relative size-32 ...">
  <div class="absolute right-0 bottom-0 size-16 ...">09</div>
</div>
```

----------------------------------------

TITLE: Applying Typography Styles with Tailwind CSS Prose Classes (HTML)
DESCRIPTION: This HTML snippet demonstrates how to apply typographic styles to a block of content using the @tailwindcss/typography plugin. By adding the 'prose' class (and responsive variants like 'lg:prose-xl') to an <article> element, all nested vanilla HTML content (like <h1> and <p> tags) will automatically receive beautiful, well-formatted typographic defaults. This eliminates the need for extensive custom CSS to style rich-text content.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/blog/tailwindcss-typography/index.mdx#_snippet_0

LANGUAGE: HTML
CODE:
```
<article class="prose lg:prose-xl">
  <h1>Garlic bread with cheese: What the science tells us</h1>
  <p>
    For years parents have espoused the health benefits of eating garlic bread with cheese to their children, with the
    food earning such an iconic status in our culture that kids will often dress up as warm, cheesy loaf for Halloween.
  </p>
  <p>
    But a recent study shows that the celebrated appetizer may be linked to a series of rabies cases springing up around
    the country.
  </p>
  <!-- ... -->
</article>
```

----------------------------------------

TITLE: Styling Sibling Elements with peer-invalid for Form Validation in HTML
DESCRIPTION: This example demonstrates how to apply styles to a sibling element based on the state of a `peer` element, specifically using `peer-invalid` for form validation. The `peer` class is added to the input field, and a subsequent sibling paragraph uses `peer-invalid:visible` to show an error message when the input is invalid. This pattern allows for dynamic UI updates without JavaScript.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/hover-focus-and-other-states.mdx#_snippet_31

LANGUAGE: html
CODE:
```
<div className="mx-auto max-w-md border-x border-x-gray-200 px-6 pt-6 pb-5 dark:border-x-gray-800 dark:bg-gray-950/10">
  <form>
    <div>
      <label htmlFor="email-2" className="block text-sm font-medium text-gray-700 dark:text-gray-300">
        Email
      </label>
      <div className="mt-1">
        <input
          type="email"
          name="email"
          id="email-2"
          className="peer block w-full rounded-md border border-gray-300 px-3 py-2 placeholder-gray-400 shadow-sm invalid:border-pink-500 invalid:text-pink-600 focus:border-sky-500 focus:outline focus:outline-sky-500 focus:invalid:border-pink-500 focus:invalid:outline-pink-500 disabled:border-gray-200 disabled:bg-gray-50 disabled:text-gray-500 disabled:shadow-none sm:text-sm"
          defaultValue="george@krugerindustrial."
          placeholder="you@example.com"
        />
        <p className="invisible mt-2 text-sm text-pink-600 peer-invalid:visible">
          Please provide a valid email address.
        </p>
      </div>
    </div>
  </form>
</div>
```

LANGUAGE: html
CODE:
```
<!-- [!code classes:peer-invalid:visible] -->
<!-- [!code classes:peer] -->
<form>
  <label class="block">
    <span class="...">Email</span>
    <input type="email" class="peer ..." />
    <p class="invisible peer-invalid:visible ...">Please provide a valid email address.</p>
  </label>
</form>
```

----------------------------------------

TITLE: HTML Example for Class-based Dark Mode Toggling
DESCRIPTION: This HTML snippet demonstrates how the `dark` class is applied to the `<html>` element to activate dark mode utilities. When the `dark` class is present, `dark:bg-black` will override `bg-white`, changing the background color. This setup requires JavaScript to dynamically add or remove the `dark` class.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/dark-mode.mdx#_snippet_2

LANGUAGE: html
CODE:
```
<html class="dark">
  <body>
    <div class="bg-white dark:bg-black">
      <!-- ... -->
    </div>
  </body>
</html>
```

----------------------------------------

TITLE: Applying Tailwind CSS Color Utilities in JSX
DESCRIPTION: This JSX snippet demonstrates the application of various Tailwind CSS color utilities within a React component structure. It shows how to set background, border, text, and outline colors, including dark mode variations, to style a notification-like UI element.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/colors.mdx#_snippet_3

LANGUAGE: javascript
CODE:
```
<div className="bg-black/5 p-8 text-sm">
  <div className="mx-auto flex max-w-md items-center gap-4 rounded-lg bg-white p-6 shadow-md outline outline-black/5 dark:bg-gray-800">
    <span className="inline-flex shrink-0 rounded-full border border-pink-300 bg-pink-100 p-2 dark:border-pink-300/10 dark:bg-pink-400/10">
      <svg
        xmlns="http://www.w3.org/2000/svg"
        fill="none"
        viewBox="0 0 24 24"
        strokeWidth={1.5}
        className="size-6 stroke-pink-700 dark:stroke-pink-500"
      >
        <path
          strokeLinecap="round"
          strokeLinejoin="round"
          d="M14.857 17.082a23.848 23.848 0 0 0 5.454-1.31A8.967 8.967 0 0 1 18 9.75V9A6 6 0 0 0 6 9v.75a8.967 8.967 0 0 1-2.312 6.022c1.733.64 3.56 1.085 5.455 1.31m5.714 0a24.255 24.255 0 0 1-5.714 0m5.714 0a3 3 0 1 1-5.714 0"
        />
      </svg>
    </span>
    <div>
      <p className="text-gray-700 dark:text-gray-400">
        <span className="font-medium text-gray-950 dark:text-white">Tom Watson</span> mentioned you in{" "}
        <span className="font-medium text-gray-950 dark:text-white">Logo redesign</span>
      </p>
      <p className="mt-1 text-gray-500">9:37am</p>
    </div>
  </div>
</div>
```

----------------------------------------

TITLE: Avoiding Dynamic Class Names in HTML with Tailwind
DESCRIPTION: This HTML snippet illustrates an incorrect way to construct class names dynamically using interpolation. Tailwind CSS scans files as plain text and cannot understand string concatenation, meaning `text-red-600` and `text-green-600` will not be detected, leading to missing styles.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/detecting-classes-in-source-files.mdx#_snippet_1

LANGUAGE: HTML
CODE:
```
<div class="text-{{ error ? 'red' : 'green' }}-600"></div>
```

----------------------------------------

TITLE: Simplifying Transform and Filter Class Usage (Before/After)
DESCRIPTION: Illustrates the simplification of transform and filter composition in Just-in-Time mode. The `transform`, `filter`, and `backdrop-filter` classes are no longer required to enable their respective sub-utilities, as these features are now automatically enabled.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/blog/tailwindcss-2-2/index.mdx#_snippet_24

LANGUAGE: HTML
CODE:
```
<div class="transform scale-50 filter grayscale backdrop-filter backdrop-blur-sm"> <!-- [!code --] -->
<div class="scale-50 grayscale backdrop-blur-sm"> <!-- [!code ++] -->
</div>
```

----------------------------------------

TITLE: Applying Default Transitions in HTML
DESCRIPTION: This HTML snippet demonstrates the simplified usage of transitions in Tailwind CSS v2.0 when default duration and timing functions are configured. With defaults set in `tailwind.config.js`, only the `transition` class is needed to apply the predefined transition properties, streamlining component styling.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/blog/tailwindcss-v2/index.mdx#_snippet_29

LANGUAGE: html
CODE:
```
<button class="transition ...">Count them again</button>
```

----------------------------------------

TITLE: Installing Tailwind CSS with PostCSS
DESCRIPTION: This command installs the latest versions of Tailwind CSS and its PostCSS plugin using npm. This setup allows Tailwind CSS to be used as a PostCSS plugin within your build pipeline, compatible with various PostCSS-enabled environments.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/blog/tailwindcss-v4-1/index.mdx#_snippet_2

LANGUAGE: sh
CODE:
```
npm install tailwindcss@latest @tailwindcss/postcss@latest
```

----------------------------------------

TITLE: Styling UI Component with Tailwind CSS in HTML
DESCRIPTION: This snippet provides a standard HTML example of applying Tailwind CSS utility classes to create a card component. It mirrors the JSX example but uses standard `class` attributes, showcasing the direct application of utilities for layout, appearance, spacing, and text styling, including dark mode variants.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/styling-with-utility-classes.mdx#_snippet_1

LANGUAGE: HTML
CODE:
```
<!-- prettier-ignore -->
<div class="mx-auto flex max-w-sm items-center gap-x-4 rounded-xl bg-white p-6 shadow-lg outline outline-black/5 dark:bg-slate-800 dark:shadow-none dark:-outline-offset-1 dark:outline-white/10">
  <img class="size-12 shrink-0" src="/img/logo.svg" alt="ChitChat Logo" />
  <div>
    <div class="text-xl font-medium text-black dark:text-white">ChitChat</div>
    <p class="text-gray-500 dark:text-gray-400">You have a new message!</p>
  </div>
</div>
```

----------------------------------------

TITLE: Demonstrating Automatic Class Sorting with Prettier in HTML
DESCRIPTION: This HTML example illustrates how the Prettier plugin for Tailwind CSS automatically sorts utility classes. The 'Before' section shows unsorted classes, while the 'After' section demonstrates the same classes reordered according to Tailwind's recommended class order, improving readability and consistency.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/blog/automatic-class-sorting-with-prettier/index.mdx#_snippet_0

LANGUAGE: html
CODE:
```
<!-- Before -->
<button class="text-white px-4 sm:px-8 py-2 sm:py-3 bg-sky-700 hover:bg-sky-800">...</button>

<!-- After -->
<button class="bg-sky-700 px-4 py-2 text-white hover:bg-sky-800 sm:px-8 sm:py-3">...</button>
```

----------------------------------------

TITLE: Creating a Reusable React Component with Tailwind CSS
DESCRIPTION: Demonstrates how to build a reusable React component (`VacationCard`) using Tailwind CSS utility classes for styling. Shows how props can be used to make the component dynamic, allowing for easy reuse with different content.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/styling-with-utility-classes.mdx#_snippet_30

LANGUAGE: jsx
CODE:
```
export function VacationCard({ img, imgAlt, eyebrow, title, pricing, url }) {
  return (
    <div>
      <img className="rounded-lg" src={img} alt={imgAlt} />
      <div className="mt-4">
        <div className="text-xs font-bold text-sky-500">{eyebrow}</div>
        <div className="mt-1 font-bold text-gray-700">
          <a href={url} className="hover:underline">
            {title}
          </a>
        </div>
        <div className="mt-2 text-sm text-gray-600">{pricing}</div>
      </div>
    </div>
  );
}
```

----------------------------------------

TITLE: Configuring PostCSS Plugin for Tailwind CSS v4.0
DESCRIPTION: This JavaScript snippet shows a minimal PostCSS configuration file that adds the @tailwindcss/postcss plugin. This configuration is essential for PostCSS to process Tailwind CSS directives and generate the final CSS output, simplifying the setup by reducing boilerplate.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/blog/tailwindcss-v4/index.mdx#_snippet_5

LANGUAGE: JavaScript
CODE:
```
export default {
  plugins: ["@tailwindcss/postcss"]
};
```

----------------------------------------

TITLE: Importing Fonts and Tailwind CSS with @import
DESCRIPTION: This CSS snippet demonstrates the correct order for `@import` statements: external font imports (like Google Fonts) must precede other imports, such as `@import "tailwindcss"`. It also shows how to define a custom font variable (`--font-roboto`) within the `@theme` block after importing the font, making it available for use in Tailwind CSS.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/font-family.mdx#_snippet_5

LANGUAGE: CSS
CODE:
```
@import url("https://fonts.googleapis.com/css2?family=Roboto&display=swap"); /* [!code highlight] */
@import "tailwindcss";

@theme {
  --font-roboto: "Roboto", sans-serif; /* [!code highlight] */
}
```

----------------------------------------

TITLE: Basic Font Size Application in HTML
DESCRIPTION: This HTML snippet demonstrates the straightforward application of Tailwind CSS font size utility classes directly to paragraph elements. It shows how different `text-` classes can be used to control the font size of text content.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/font-size.mdx#_snippet_2

LANGUAGE: HTML
CODE:
```
<!-- [!code classes:text-sm,text-base,text-lg,text-xl,text-2xl] -->
<p class="text-sm ...">The quick brown fox ...</p>
<p class="text-base ...">The quick brown fox ...</p>
<p class="text-lg ...">The quick brown fox ...</p>
<p class="text-xl ...">The quick brown fox ...</p>
<p class="text-2xl ...">The quick brown fox ...</p>
```

----------------------------------------

TITLE: Styling UI Component with Tailwind CSS in JSX
DESCRIPTION: This snippet demonstrates how to apply Tailwind CSS utility classes directly to HTML elements within a JSX context (like React). It shows the structure of a card component with styling for layout, appearance, spacing, and text, including handling dark mode styles and embedding an SVG.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/styling-with-utility-classes.mdx#_snippet_0

LANGUAGE: JSX
CODE:
```
<div className="mx-auto flex max-w-sm items-center gap-x-4 rounded-xl bg-white p-6 shadow-lg outline outline-black/5 dark:bg-slate-800 dark:shadow-none dark:-outline-offset-1 dark:outline-white/10">
  <svg className="size-12 shrink-0" viewBox="0 0 40 40">
    <defs>
      <linearGradient x1="50%" y1="0%" x2="50%" y2="100%" id="a">
        <stop stopColor="#2397B3" offset="0%"></stop>
        <stop stopColor="#13577E" offset="100%"></stop>
      </linearGradient>
      <linearGradient x1="50%" y1="0%" x2="50%" y2="100%" id="b">
        <stop stopColor="#73DFF2" offset="0%"></stop>
        <stop stopColor="#47B1EB" offset="100%"></stop>
      </linearGradient>
    </defs>
    <g fill="none" fillRule="evenodd">
      <path
        d="M28.872 22.096c.084.622.128 1.258.128 1.904 0 7.732-6.268 14-14 14-2.176 0-4.236-.496-6.073-1.382l-6.022 2.007c-1.564.521-3.051-.966-2.53-2.53l2.007-6.022A13.944 13.944 0 0 1 1 24c0-7.331 5.635-13.346 12.81-13.95A9.967 9.967 0 0 0 13 14c0 5.523 4.477 10 10 10a9.955 9.955 0 0 0 5.872-1.904z"
        fill="url(#a)"
        transform="translate(1 1)"
      ></path>
      <path
        d="M35.618 20.073l2.007 6.022c.521 1.564-.966 3.051-2.53 2.53l-6.022-2.007A13.944 13.944 0 0 1 23 28c-7.732 0-14-6.268-14-14S15.268 0 23 0s14 6.268 14 14c0 2.176-.496 4.236-1.382 6.073z"
        fill="url(#b)"
        transform="translate(1 1)"
      ></path>
      <path
        d="M18 17a2 2 0 1 0 0-4 2 2 0 0 0 0 4zM24 17a2 2 0 1 0 0-4 2 2 0 0 0 0 4zM30 17a2 2 0 1 0 0-4 2 2 0 0 0 0 4z"
        fill="#FFF"
      ></path>
    </g>
  </svg>
  <div>
    <div className="text-xl font-medium text-black dark:text-white">ChitChat</div>
    <p className="text-gray-500 dark:text-gray-400">You have a new message!</p>
  </div>
</div>
```

----------------------------------------

TITLE: Defining Shared Theme Variables in a Separate CSS File
DESCRIPTION: This CSS snippet demonstrates how to define a collection of shared theme variables (e.g., spacing, fonts, colors) within a dedicated CSS file. This approach facilitates reusability and consistency across multiple projects by centralizing design tokens in a single, importable file.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/theme.mdx#_snippet_21

LANGUAGE: css
CODE:
```
@theme {
  --*: initial;

  --spacing: 4px;

  --font-body: Inter, sans-serif;

  --color-lagoon: oklch(0.72 0.11 221.19);
  --color-coral: oklch(0.74 0.17 40.24);
  --color-driftwood: oklch(0.79 0.06 74.59);
  --color-tide: oklch(0.49 0.08 205.88);
  --color-dusk: oklch(0.82 0.15 72.09);
}
```

----------------------------------------

TITLE: Using Arbitrary Container Query Values in HTML with Tailwind CSS
DESCRIPTION: This HTML snippet demonstrates the use of arbitrary values for container queries in Tailwind CSS. The @[618px]:flex syntax allows applying styles based on a precise pixel value for the container's width, providing flexibility beyond predefined breakpoints. This is useful for specific design requirements that don't align with the default scale.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/blog/tailwindcss-v3-2/index.mdx#_snippet_34

LANGUAGE: HTML
CODE:
```
<div class="@container">
  <div class="block @[618px]:flex">
    <!-- ... -->
  </div>
</div>
```

----------------------------------------

TITLE: Integrating Tailwind CSS v4.0 with Vite
DESCRIPTION: This TypeScript configuration snippet demonstrates how to integrate Tailwind CSS v4.0 into a Vite project using the dedicated @tailwindcss/vite plugin. It imports defineConfig from Vite and the tailwindcss plugin, then adds the plugin to the plugins array within the Vite configuration, offering improved performance over the PostCSS plugin for Vite users.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/blog/tailwindcss-v4/index.mdx#_snippet_7

LANGUAGE: TypeScript
CODE:
```
import { defineConfig } from "vite";
import tailwindcss from "@tailwindcss/vite";

export default defineConfig({
  plugins: [
    tailwindcss()
  ]
});
```

----------------------------------------

TITLE: Applying Sizing with `size-*` Utility in HTML
DESCRIPTION: This snippet demonstrates the new `size-*` utility in Tailwind CSS, which allows setting both width and height simultaneously. It replaces the need for separate `h-*` and `w-*` classes, offering a more concise way to define element dimensions. The example shows how `size-10` achieves the same result as `h-10 w-10`.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/blog/tailwindcss-v3-4/index.mdx#_snippet_6

LANGUAGE: HTML
CODE:
```
<div>
  <img class="h-10 w-10" ...>
  <img class="h-12 w-12" ...>
  <img class="h-14 w-14" ...>
  <img class="size-10" ...>
  <img class="size-12" ...>
  <img class="size-14" ...>
</div>
```

----------------------------------------

TITLE: Applying Horizontal Padding with Tailwind CSS
DESCRIPTION: This HTML snippet demonstrates the use of the `px-<number>` utility, specifically `px-8`, to apply horizontal padding to an element. This class simultaneously controls both the left and right padding, providing symmetrical spacing along the x-axis.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/padding.mdx#_snippet_3

LANGUAGE: html
CODE:
```
<!-- [!code classes:px-8] -->\n<div class="px-8 ...">px-8</div>
```

----------------------------------------

TITLE: Applying Media Query for Dark Color Scheme in CSS
DESCRIPTION: This snippet defines a media query that applies styles when the user's operating system or browser is set to prefer a dark color scheme, enabling dark mode support.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/hover-focus-and-other-states.mdx#_snippet_194

LANGUAGE: CSS
CODE:
```
@media (prefers-color-scheme: dark)
```

----------------------------------------

TITLE: Implementing a Dialog (Modal) with Headless UI in React
DESCRIPTION: This React component demonstrates how to create a modal dialog using Headless UI's `Dialog` component. It manages the dialog's open/closed state with `useState` and includes an overlay, title, description, and action buttons. It's suitable for traditional modals or full-page take-over UIs.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/blog/headless-ui-v1/index.mdx#_snippet_0

LANGUAGE: jsx
CODE:
```
import { useState } from "react";
import { Dialog } from "@headlessui/react";

function MyDialog() {
  let [isOpen, setIsOpen] = useState(true);

  return (
    <Dialog open={isOpen} onClose={setIsOpen}>
      <Dialog.Overlay />

      <Dialog.Title>Deactivate account</Dialog.Title>
      <Dialog.Description>This will permanently deactivate your account</Dialog.Description>

      <p>
        Are you sure you want to deactivate your account? All of your data will be permanently removed. This action
        cannot be undone.
      </p>

      <button onClick={() => setIsOpen(false)}>Deactivate</button>
      <button onClick={() => setIsOpen(false)}>Cancel</button>
    </Dialog>
  );
}
```

----------------------------------------

TITLE: Adding Custom Colors with @theme in CSS
DESCRIPTION: This code demonstrates how to add new custom colors to your Tailwind CSS project using the `@theme` directive. These custom colors become available under the `--color-*` theme namespace, allowing you to use them with utility classes like `bg-midnight`.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/colors.mdx#_snippet_12

LANGUAGE: css
CODE:
```
@import "tailwindcss";

@theme {
  --color-midnight: #121063;
  --color-tahiti: #3ab7bf;
  --color-bermuda: #78dcca;
}
```

----------------------------------------

TITLE: Building Component with Tailwind Utilities (React)
DESCRIPTION: Demonstrates building a responsive component using Tailwind CSS utility classes within a React/JSX context. It showcases responsive variants (@sm:), spacing (space-y, gap-x), layout (flex, items-center), and state variants (hover:, active:) for styling.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/styling-with-utility-classes.mdx#_snippet_2

LANGUAGE: javascript
CODE:
```
<div className="mx-auto max-w-sm space-y-2 rounded-xl bg-white px-8 py-8 shadow-lg ring ring-black/5 @sm:flex @sm:items-center @sm:space-y-0 @sm:gap-x-6 @sm:py-4">
  <img
    className="mx-auto block h-24 rounded-full @sm:mx-0 @sm:shrink-0"
    src={erinLindford.src}
    alt="Woman's Face"
  />
  <div className="space-y-2 text-center @sm:text-left">
    <div className="space-y-0.5">
      <p className="text-lg font-semibold text-black">Erin Lindford</p>
      <p className="font-medium text-gray-500">Product Engineer</p>
    </div>
    <button className="rounded-full border border-purple-200 px-4 py-1 text-sm font-semibold text-purple-600 hover:border-transparent hover:bg-purple-600 hover:text-white active:bg-purple-700">
      Message
    </button>
  </div>
</div>
```

----------------------------------------

TITLE: Including Tailwind Directives in CSS
DESCRIPTION: This CSS snippet demonstrates the standard Tailwind CSS directives used to inject Tailwind's base styles, components, and utility classes into a CSS file. These directives are typically placed at the top of a main CSS file to enable Tailwind's features.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/blog/tailwindcss-1-6/index.mdx#_snippet_8

LANGUAGE: css
CODE:
```
@tailwind base;
@tailwind components;
@tailwind utilities;
```

----------------------------------------

TITLE: Implementing Combobox List Virtualization with Headless UI (JSX)
DESCRIPTION: This JSX code snippet demonstrates how to implement list virtualization for a Headless UI Combobox component using the new `virtual` prop, powered by TanStack Virtual. It shows how to filter a large dataset of items (e.g., 1000 people) and efficiently render them within the `ComboboxOptions` using a render prop, significantly improving performance for comboboxes with many options. It requires `@headlessui/react`, `@heroicons/react/20/solid`, and `react`.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/blog/headless-ui-v2/index.mdx#_snippet_6

LANGUAGE: jsx
CODE:
```
import { Combobox, ComboboxButton, ComboboxInput, ComboboxOption, ComboboxOptions } from "@headlessui/react";
import { ChevronDownIcon } from "@heroicons/react/20/solid";
import { useState } from "react";

const people = [
  { id: 1, name: "Rossie Abernathy" },
  { id: 2, name: "Juana Abshire" },
  { id: 3, name: "Leonel Abshire" },
  { id: 4, name: "Llewellyn Abshire" },
  { id: 5, name: "Ramon Abshire" },
  // ...up to 1000 people
];

function Example() {
  const [query, setQuery] = useState("");
  const [selected, setSelected] = useState(people[0]);

  const filteredPeople =
    query === ""
      ? people
      : people.filter((person) => {
          return person.name.toLowerCase().includes(query.toLowerCase());
        });

  return (
    <Combobox
      value={selected}
      // [!code highlight:2]
      virtual={{ options: filteredPeople }}
      onChange={(value) => setSelected(value)}
      onClose={() => setQuery("")}
    >
      <div>
        <ComboboxInput displayValue={(person) => person?.name} onChange={(event) => setQuery(event.target.value)} />
        <ComboboxButton>
          <ChevronDownIcon />
        </ComboboxButton>
      </div>
      <ComboboxOptions>
        // [!code highlight:6]
        {({ option: person }) => (
          <ComboboxOption key={person.id} value={person}>
            {person.name}
          </ComboboxOption>
        )}
      </ComboboxOptions>
    </Combobox>
  );
}
```

----------------------------------------

TITLE: Tailwind CSS Display Utilities Example (JSX)
DESCRIPTION: Illustrates the application of `inline`, `inline-block`, and `block` Tailwind CSS classes within a React component to control text and element wrapping and flow.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/display.mdx#_snippet_3

LANGUAGE: JSX
CODE:
```
<div className="mx-auto max-w-xs px-4 text-sm leading-6 text-gray-500 sm:text-base sm:leading-7 dark:text-gray-300">
      When controlling the flow of text, using the CSS property{" "}
      <span className="inline rounded bg-sky-100 font-mono text-sm font-bold text-gray-900 dark:bg-white/15 dark:text-gray-200">
        display: inline
      </span>{" "}
      will cause the text inside the element to wrap normally.
      <br />
      <br />
      While using the property{" "}
      <span className="inline-block rounded bg-sky-100 font-mono text-sm font-bold text-gray-900 dark:bg-white/15 dark:text-gray-200">
        display: inline-block
      </span>{" "}
      will wrap the element to prevent the text inside from extending beyond its parent.
      <br />
      <br />
      Lastly, using the property{" "}
      <span className="block rounded bg-sky-100 font-mono text-sm font-bold text-gray-900 dark:bg-white/15 dark:text-gray-200">
        display: block
      </span>{" "}
      will put the element on its own line and fill its parent.
    </div>
```

----------------------------------------

TITLE: Combining Dark Mode, Hover, and Responsive Breakpoints in HTML
DESCRIPTION: This HTML snippet demonstrates the combination of dark mode, hover states, and responsive breakpoints in Tailwind CSS. The `lg:dark:hover:bg-gray-50` class applies a specific background color on hover, in dark mode, and only on large screens and above, showcasing advanced conditional styling.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/blog/tailwindcss-v2/index.mdx#_snippet_7

LANGUAGE: html
CODE:
```
<button class="lg:dark:bg-white lg:dark:hover:bg-gray-50 ...">
  <!-- ... -->
</button>
```

----------------------------------------

TITLE: Updating PostCSS Configuration for Tailwind CSS v4 (JavaScript)
DESCRIPTION: This snippet shows the updated `postcss.config.mjs` for Tailwind CSS v4. In v4, the PostCSS plugin is now in `@tailwindcss/postcss`, replacing the direct `tailwindcss` package. Additionally, `postcss-import` and `autoprefixer` can be removed as their functionalities are now handled automatically by Tailwind CSS v4.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/upgrade-guide.mdx#_snippet_1

LANGUAGE: js
CODE:
```
export default {
  plugins: {
    "@tailwindcss/postcss": {}
  }
};
```

----------------------------------------

TITLE: Installing Tailwind CSS v4.0 with PostCSS
DESCRIPTION: This shell command installs Tailwind CSS v4.0 along with its PostCSS plugin using npm. It's the first step in setting up Tailwind CSS for a project, providing the core framework and the necessary PostCSS integration for build processes.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/blog/tailwindcss-v4/index.mdx#_snippet_4

LANGUAGE: Shell
CODE:
```
npm i tailwindcss @tailwindcss/postcss;
```

----------------------------------------

TITLE: Applying Styles Based on Specific Data Attribute Value in HTML
DESCRIPTION: This example illustrates how to apply Tailwind CSS styles when a `data-*` attribute has a specific value. Using arbitrary values like `data-[size=large]:p-8` ensures that the padding style is applied only if the `data-size` attribute's value is exactly 'large'.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/hover-focus-and-other-states.mdx#_snippet_80

LANGUAGE: HTML
CODE:
```
<!-- Will apply -->
<div data-size="large" class="data-[size=large]:p-8">
  <!-- ... -->
</div>

<!-- Will not apply -->
<div data-size="medium" class="data-[size=large]:p-8">
  <!-- ... -->
</div>
```

----------------------------------------

TITLE: Styling Active Menu Items with Render Props in Headless UI (React)
DESCRIPTION: This snippet specifically highlights the use of a render prop within a Headless UI `Menu.Item` component in React. The render prop provides access to the `active` state, enabling conditional styling of menu items (e.g., changing background color and text color when active). This pattern is crucial for accessing internal component state for UI customization.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/blog/building-react-and-vue-support-for-tailwind-ui/index.mdx#_snippet_3

LANGUAGE: JSX
CODE:
```
<Menu.Item>
  {({ active }) => (
    <a className={`${active && "bg-blue-500 text-white"} ...`} href="/documentation">
      Documentation
    </a>
  )}
</Menu.Item>
```

----------------------------------------

TITLE: Simplified HTML Example of peer-has-checked
DESCRIPTION: This HTML snippet provides a simplified illustration of the `peer-has-checked` variant in Tailwind CSS. It shows how an SVG element can be conditionally hidden (`peer-has-checked:hidden`) when a sibling `label` element, marked with `peer`, contains a checked checkbox input. This highlights the core functionality of styling based on a peer's descendant's state.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/hover-focus-and-other-states.mdx#_snippet_20

LANGUAGE: HTML
CODE:
```
<!-- [!code classes:peer-has-checked:hidden,peer] -->
<div>
  <label class="peer ...">
    <input type="checkbox" name="todo[1]" checked />
    Create a to do list
  </label>
  <svg class="peer-has-checked:hidden ..."><!-- ... --></svg>
</div>
```

----------------------------------------

TITLE: JavaScript for Three-Way Theme Toggling (Light, Dark, System)
DESCRIPTION: This JavaScript snippet provides logic for a three-way theme toggle, supporting light, dark, and system-preferred modes. It checks `localStorage` for a saved theme preference and falls back to `window.matchMedia` for system preference if no explicit theme is set. It then applies or removes the `dark` class on the `<html>` element, and includes examples for explicitly setting light, dark, or system themes via `localStorage`.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/dark-mode.mdx#_snippet_5

LANGUAGE: javascript
CODE:
```
document.documentElement.classList.toggle(
  "dark",
  localStorage.theme === "dark" ||
    (!("theme" in localStorage) && window.matchMedia("(prefers-color-scheme: dark)").matches)
);

// Whenever the user explicitly chooses light mode
localStorage.theme = "light";

// Whenever the user explicitly chooses dark mode
localStorage.theme = "dark";

// Whenever the user explicitly chooses to respect the OS preference
localStorage.removeItem("theme");
```

----------------------------------------

TITLE: Applying Inert Variant to Form Elements in JSX
DESCRIPTION: This snippet demonstrates how to apply the `inert` variant in Tailwind CSS to a `fieldset` element within a React/JSX form. The `inert` attribute makes the `fieldset` and its contents non-interactive, while `inert:opacity-25` visually dims it, indicating its inactive state. It showcases a notification preferences form with custom radio and checkbox inputs.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/hover-focus-and-other-states.mdx#_snippet_87

LANGUAGE: JSX
CODE:
```
<form className="mx-auto max-w-md space-y-6 border-x border-x-gray-200 p-8 dark:border-x-gray-800">
  <legend>Notification preferences</legend>
  <fieldset name="resale" defaultValue="permit">
    <div className="flex items-center gap-x-3">
      <input
        readOnly
        id="push-everything"
        name="push-notifications"
        type="radio"
        className="relative size-4 appearance-none rounded-full border border-gray-300 bg-white before:absolute before:inset-1 before:rounded-full before:bg-white not-checked:before:hidden checked:border-indigo-600 checked:bg-indigo-600 focus-visible:outline-2 focus-visible:outline-offset-2 focus-visible:outline-indigo-600 disabled:border-gray-300 disabled:bg-gray-100 disabled:before:bg-gray-400 dark:border-gray-700 dark:bg-gray-950 forced-colors:appearance-auto forced-colors:before:hidden"
      />
      <label htmlFor="push-everything" className="block text-sm/6 font-medium text-gray-900 dark:text-white">
        Custom
      </label>
    </div>
    <fieldset className="mt-6 space-y-6 pl-8 inert:opacity-25" inert>
      <div className="flex gap-3">
        <div className="flex h-6 shrink-0 items-center">
          <div className="group grid size-4 grid-cols-1">
            <input
              defaultChecked
              id="comments"
              name="comments"
              type="checkbox"
              aria-describedby="comments-description"
              className="col-start-1 row-start-1 appearance-none rounded-sm border border-gray-300 bg-white checked:border-indigo-600 checked:bg-indigo-600 indeterminate:border-indigo-600 indeterminate:bg-indigo-600 focus-visible:outline-2 focus-visible:outline-offset-2 focus-visible:outline-indigo-600 disabled:border-gray-300 disabled:bg-gray-100 disabled:checked:bg-gray-100 dark:border-gray-700 dark:bg-gray-950 forced-colors:appearance-auto"
            />
            <svg
              fill="none"
              viewBox="0 0 14 14"
              className="pointer-events-none col-start-1 row-start-1 size-3.5 self-center justify-self-center stroke-white group-has-disabled:stroke-gray-950/25"
            >
              <path
                d="M3 8L6 11L11 3.5"
                strokeWidth={2}
                strokeLinecap="round"
                strokeLinejoin="round"
                className="opacity-0 group-has-checked:opacity-100"
              />
              <path
                d="M3 7H11"
                strokeWidth={2}
                strokeLinecap="round"

```

----------------------------------------

TITLE: Applying Focus Border Color with Tailwind CSS (HTML/JSX)
DESCRIPTION: This snippet demonstrates how to apply a specific border color when an element is in its focus state using Tailwind CSS. The `focus:border-pink-600` utility class changes the input field's border to pink-600 upon focus, enhancing user interaction feedback. This requires an interactive element like an input field. No specific dependencies beyond Tailwind CSS are required.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/border-color.mdx#_snippet_7

LANGUAGE: JSX
CODE:
```
<label className="mx-auto block max-w-xs">
  <span className="text-sm font-medium text-gray-900 dark:text-gray-200">Email address</span>
  <input
    type="text"
    placeholder="jane@example.com"
    className="block w-full rounded-lg border-2 border-gray-700 px-3 py-2 font-sans text-sm leading-5 text-gray-500 focus:border-pink-600 focus:outline-none dark:bg-gray-900 dark:text-gray-400 dark:placeholder:text-gray-600"
  />
</label>
```

LANGUAGE: HTML
CODE:
```
<!-- [!code classes:focus:border-pink-600] -->
<input class="border-2 border-gray-700 focus:border-pink-600 ..." />
```

----------------------------------------

TITLE: Implementing Responsive Columns with React/JSX and Tailwind CSS
DESCRIPTION: This snippet showcases a responsive multi-column layout using Tailwind CSS classes within a React/JSX structure. It dynamically adjusts the number of columns and gap based on the container's breakpoint (@sm), demonstrating container queries. It also includes image elements with aspect ratios and object-fit properties for visual presentation.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/columns.mdx#_snippet_8

LANGUAGE: JavaScript
CODE:
```
<div className="@container relative">\n  <div className="absolute inset-0 -top-8 -bottom-8 grid grid-cols-2 gap-4 @sm:grid-cols-3 @sm:gap-8">\n    <Stripes border="x" />\n    <Stripes border="x" />\n    <Stripes border="x" className="@max-sm:hidden" />\n  </div>\n  <div className="relative columns-2 gap-4 *:mb-4 @sm:-mb-8 @sm:columns-3 @sm:gap-8 @sm:*:mb-8">\n    <img\n      className="aspect-3/2 rounded-lg bg-black/5 object-cover outline -outline-offset-1 outline-black/10 dark:outline-0"\n      src="https://images.unsplash.com/photo-1454496522488-7a8e488e8606?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=2952&q=80"\n    />\n    <img\n      className="aspect-square rounded-lg bg-black/5 object-cover outline -outline-offset-1 outline-black/10 dark:outline-0"\n      src="https://images.unsplash.com/photo-1434394354979-a235cd36269d?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=2902&q=80"\n    />\n    <img\n      className="aspect-square rounded-lg bg-black/5 object-cover outline -outline-offset-1 outline-black/10 dark:outline-0"\n      src="https://images.unsplash.com/photo-1491904768633-2b7e3e7fede5?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=3131&q=80"\n    />\n    <img\n      className="aspect-square rounded-lg bg-black/5 object-cover outline -outline-offset-1 outline-black/10 dark:outline-0"\n      src="https://images.unsplash.com/photo-1463288889890-a56b2853c40f?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=3132&q=80"\n    />\n    <img\n      className="aspect-3/2 rounded-lg bg-black/5 object-cover outline -outline-offset-1 outline-black/10 dark:outline-0"\n      src="https://images.unsplash.com/photo-1611605645802-c21be743c321?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=2940&q=80"\n    />\n    <img\n      className="aspect-square rounded-lg bg-black/5 object-cover outline -outline-offset-1 outline-black/10 dark:outline-0"\n      src="https://images.unsplash.com/photo-1498603993951-8a027a8a8f84?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=2936&q=80"\n    />\n    <img\n      className="aspect-square rounded-lg bg-black/5 object-cover outline -outline-offset-1 outline-black/10 dark:outline-0"\n      src="https://images.unsplash.com/photo-1526400473556-aac12354f3db?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=2940&q=80"\n    />\n    <img\n      className="aspect-square rounded-lg bg-black/5 object-cover outline -outline-offset-1 outline-black/10 dark:outline-0"\n      src="https://images.unsplash.com/photo-1617369120004-4fc70312c5e6?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=1587&q=80"\n    />\n    <img\n      className="aspect-3/2 rounded-lg bg-black/5 object-cover outline -outline-offset-1 outline-black/10 dark:outline-0"\n      src="https://images.unsplash.com/photo-1518892096458-a169843d7f7f?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=2940&q=80"\n    />\n  </div>\n</div>
```

----------------------------------------

TITLE: Setting Inline Start Border Width with Numeric Value in Tailwind CSS
DESCRIPTION: Applies a border width defined by a numeric pixel value to the inline-start side of an element. For example, `border-s-2` would set `border-inline-start-width: 2px;`.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/border-width.mdx#_snippet_22

LANGUAGE: css
CODE:
```
border-inline-start-width: <number>px;
```

----------------------------------------

TITLE: Using Arbitrary Values and CSS Variables for Opacity in HTML
DESCRIPTION: This HTML snippet illustrates the use of arbitrary values and CSS variables to define color opacity in Tailwind CSS. The `bg-pink-500/[71.37%]` class applies a precise 71.37% opacity, while `bg-cyan-400/(--my-alpha-value)` demonstrates using a CSS variable for dynamic opacity control. This flexibility allows for advanced customization beyond predefined utility classes.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/colors.mdx#_snippet_6

LANGUAGE: HTML
CODE:
```
<div class="bg-pink-500/[71.37%]"><!-- ... --></div>
<div class="bg-cyan-400/(--my-alpha-value)"><!-- ... --></div>
```

----------------------------------------

TITLE: Styling Required Field Asterisk with Tailwind CSS `::after`
DESCRIPTION: This snippet demonstrates how to use Tailwind CSS `after` variants to style the `::after` pseudo-element. It adds a red asterisk (`*`) after a label's text, commonly used to indicate a required input field. The `after:content-['*']`, `after:ml-0.5`, and `after:text-red-500` classes apply the content, margin, and color respectively.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/hover-focus-and-other-states.mdx#_snippet_38

LANGUAGE: HTML
CODE:
```
<label>
  <span class="text-gray-700 after:ml-0.5 after:text-red-500 after:content-['*'] ...">Email</span>
  <input type="email" name="email" class="..." placeholder="you@example.com" />
</label>
```

----------------------------------------

TITLE: Defining Container Query for @max-xl (Width < 36rem) in CSS
DESCRIPTION: This snippet defines a container query associated with the @max-xl breakpoint, applying styles when the container's width is less than 36 rem. It's used for responsive design based on the parent container's size rather than the viewport.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/hover-focus-and-other-states.mdx#_snippet_186

LANGUAGE: CSS
CODE:
```
@container (width < 36rem)
```

----------------------------------------

TITLE: Styling Disabled Input Wrapper with :has() in JSX
DESCRIPTION: This snippet demonstrates how to style a wrapper element based on the `:disabled` state of its child `<input>` using the CSS `:has()` pseudo-class within a Tailwind CSS class. It allows for conditional styling of the parent `<span>` without JavaScript. The `has-[:disabled]:opacity-50` class applies `opacity-50` to the `<span>` when the nested `<input>` is disabled.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/blog/tailwindcss-v3-4/index.mdx#_snippet_3

LANGUAGE: jsx
CODE:
```
export function Input({ ... }) {
  return (
    <span className="has-[:disabled]:opacity-50 ...">
      <input ... />
    </span>
  )
}
```

----------------------------------------

TITLE: Type Hinting for Arbitrary Values in HTML
DESCRIPTION: This HTML snippet demonstrates the new type-hinting syntax for arbitrary values in Tailwind CSS. By prefixing the value with its type (e.g., `color:`), developers can explicitly clarify the intended CSS property, resolving ambiguities when using CSS variables or other custom values.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/blog/tailwindcss-2-2/index.mdx#_snippet_17

LANGUAGE: HTML
CODE:
```
<div class="text-[color:var(--mystery-var)]"></div>
```

----------------------------------------

TITLE: Migrating from `@screen` to `screen()` function
DESCRIPTION: Illustrates the transition from the deprecated `@screen` at-rule to the new `screen()` function within standard `@media` rules for defining responsive styles in CSS.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/blog/tailwindcss-2-2/index.mdx#_snippet_21

LANGUAGE: CSS
CODE:
```
@screen sm { /* [!code --] */
@media screen(sm) { /* [!code ++] */
  /* ... */
}
```

----------------------------------------

TITLE: Responsive Grid Layout with Tailwind CSS and Breakpoints (JSX)
DESCRIPTION: This JSX snippet demonstrates a responsive grid layout using Tailwind CSS utility classes within a React component context. The grid changes from 2 columns to 3 columns at the `@sm` breakpoint using the `@sm:grid-cols-3` class. This requires Tailwind CSS configured with container queries or standard breakpoints.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/styling-with-utility-classes.mdx#_snippet_9

LANGUAGE: JSX
CODE:
```
<div className="grid grid-cols-2 gap-4 text-center font-mono font-medium text-white @sm:grid-cols-3">
  <div className="rounded-lg bg-sky-500 p-4">01</div>
  <div className="rounded-lg bg-sky-500 p-4">02</div>
  <div className="rounded-lg bg-sky-500 p-4">03</div>
  <div className="rounded-lg bg-sky-500 p-4">04</div>
  <div className="rounded-lg bg-sky-500 p-4">05</div>
  <div className="rounded-lg bg-sky-500 p-4">06</div>
</div>
```

----------------------------------------

TITLE: Applying Dark Mode Styles in HTML with Tailwind CSS
DESCRIPTION: Demonstrates how to use Tailwind CSS `dark:` prefixed utility classes alongside regular classes to apply different styles based on the user's color scheme preference. Shows examples for background color and text color.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/styling-with-utility-classes.mdx#_snippet_12

LANGUAGE: html
CODE:
```
<!-- [!code word:dark\:bg-gray-800] -->
<!-- prettier-ignore -->
<div class="bg-white dark:bg-gray-800 rounded-lg px-6 py-8 ring shadow-xl ring-gray-900/5">
  <div>
    <span class="inline-flex items-center justify-center rounded-md bg-indigo-500 p-2 shadow-lg">
      <svg
        class="h-6 w-6 text-white"

        fill="none"
        viewBox="0 0 24 24"
        stroke="currentColor"
        aria-hidden="true"
      >
        <!-- ... -->
      </svg>
    </span>
  </div>
  <!-- prettier-ignore -->
  <!-- [!code word:dark\:text-white] -->
  <h3 class="text-gray-900 dark:text-white mt-5 text-base font-medium tracking-tight ">Writes upside-down</h3>
  <!-- prettier-ignore -->
  <!-- [!code word:dark\:text-gray-400] -->
  <p class="text-gray-500 dark:text-gray-400 mt-2 text-sm ">
    The Zero Gravity Pen can be used to write in any orientation, including upside-down. It even works in outer space.
  </p>
</div>
```

----------------------------------------

TITLE: Using the New Line-Height Shorthand in Tailwind CSS
DESCRIPTION: This HTML snippet demonstrates the new line-height shorthand syntax in Tailwind CSS, allowing you to set both font-size and line-height with a single utility class. It shows the transition from the old `text-lg leading-7` syntax to the new `text-lg/7` syntax.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/blog/tailwindcss-v3-3/index.mdx#_snippet_30

LANGUAGE: HTML
CODE:
```
<p class="text-lg leading-7 ..."><!-- [!code --] --><p class="text-lg/7 ..."><!-- [!code ++] -->
  So I started to walk into the water. I won't lie to you boys, I was terrified. But I pressed on, and as I made my way
  past the breakers a strange calm came over me. I don't know if it was divine intervention or the kinship of all living
  things but I tell you Jerry at that moment, I <em>was</em> a marine biologist.
</p>
```

----------------------------------------

TITLE: Implementing Tabs with Headless UI in Vue
DESCRIPTION: This snippet illustrates how to implement a tabbed interface in Vue using Headless UI components. It includes both the template structure for `TabGroup`, `TabList`, `Tab`, `TabPanels`, and `TabPanel`, and the script section for importing and registering these components within a Vue component.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/blog/headless-ui-v1-4/index.mdx#_snippet_1

LANGUAGE: html
CODE:
```
<template>
  <TabGroup>
    <TabList>
      <Tab>Tab 1</Tab>
      <Tab>Tab 2</Tab>
      <Tab>Tab 3</Tab>
    </TabList>
    <TabPanels>
      <TabPanel>Content 1</TabPanel>
      <TabPanel>Content 2</TabPanel>
      <TabPanel>Content 3</TabPanel>
    </TabPanels>
  </TabGroup>
</template>

<script>
  import { TabGroup, TabList, Tab, TabPanels, TabPanel } from '@headlessui/vue'

  export default {
    components: {
      TabGroup,
      TabList,
      Tab,
      TabPanels,
      TabPanel,
    },
  }
</script>
```

----------------------------------------

TITLE: Implementing Max-Width Container Queries with Tailwind CSS
DESCRIPTION: This example illustrates the use of the new `@max-*` variant for max-width container queries in Tailwind CSS v4.0. The `@container` class is applied to the parent, and `@max-md:grid-cols-1` changes the grid layout when the container's width is below the 'md' breakpoint.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/blog/tailwindcss-v4/index.mdx#_snippet_17

LANGUAGE: HTML
CODE:
```
<div class="@container">
  <div class="grid grid-cols-3 @max-md:grid-cols-1">
    <!-- ... -->
  </div>
</div>
```

----------------------------------------

TITLE: Styling on Focus with Tailwind CSS (HTML)
DESCRIPTION: This snippet shows how to style an input element when it gains focus using the `focus` variant. The `focus:border-blue-400` class changes the border color to blue-400 when the input is focused, overriding the default `border-gray-300`.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/hover-focus-and-other-states.mdx#_snippet_228

LANGUAGE: HTML
CODE:
```
<input class="border-gray-300 focus:border-blue-400 ..." />
```

----------------------------------------

TITLE: Styling on Active State with Tailwind CSS (HTML)
DESCRIPTION: This snippet shows how to style an element when it is being pressed (e.g., clicked or tapped) using the `active` variant. The `active:bg-blue-600` class changes the button's background to blue-600 while it is active, from its default `bg-blue-500`.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/hover-focus-and-other-states.mdx#_snippet_231

LANGUAGE: HTML
CODE:
```
<button class="bg-blue-500 active:bg-blue-600 ...">Submit</button>
```

----------------------------------------

TITLE: Applying Responsive Width Utilities in HTML
DESCRIPTION: This HTML snippet illustrates how to apply responsive width utility classes using Tailwind CSS. By default, the image has a width of w-16, which changes to md:w-32 on medium screens and lg:w-48 on large screens, showcasing conditional styling based on breakpoints.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/responsive-design.mdx#_snippet_3

LANGUAGE: HTML
CODE:
```
<img class="w-16 md:w-32 lg:w-48" src="..." />
```

----------------------------------------

TITLE: Creating Sticky Table Headers with Tailwind CSS
DESCRIPTION: This example demonstrates how to create a table with a sticky header row using Tailwind CSS. It utilizes `border-separate` and `border-spacing-0` on the table, combined with `sticky top-0` on the table headers, to achieve a persistent bottom border under the headings while scrolling. The `overflow-auto` on the parent div enables scrolling within the table.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/blog/tailwindcss-v3-1/index.mdx#_snippet_11

LANGUAGE: JSX
CODE:
```
<div className="isolate h-72 overflow-auto rounded-xl">
  <table className="min-w-full border-separate border-spacing-0">
    <thead className="bg-gray-50">
      <tr>
        <th
          scope="col"
          className="bg-opacity-75 sticky top-0 z-10 border-b border-gray-300 bg-gray-50 py-3.5 pr-3 pl-4 text-left text-sm font-semibold text-gray-900 backdrop-blur backdrop-filter sm:pl-6 lg:pl-8"
        >
          <>Name</>
        </th>
        <th
          scope="col"
          className="bg-opacity-75 sticky top-0 z-10 hidden border-b border-gray-300 bg-gray-50 px-3 py-3.5 text-left text-sm font-semibold text-gray-900 backdrop-blur backdrop-filter lg:table-cell"
        >
          <>Email</>
        </th>
        <th
          scope="col"
          className="bg-opacity-75 sticky top-0 z-10 border-b border-gray-300 bg-gray-50 px-3 py-3.5 text-left text-sm font-semibold text-gray-900 backdrop-blur backdrop-filter"
        >
          <>Role</>
        </th>
      </tr>
    </thead>
    <tbody className="bg-white">
      <tr>
        <td className="border-b border-gray-200 py-4 pr-3 pl-4 text-sm font-medium whitespace-nowrap text-gray-900 sm:pl-6 lg:pl-8">
          <>Courtney Henry</>
        </td>
        <td className="hidden border-b border-gray-200 px-3 py-4 text-sm whitespace-nowrap text-gray-500 lg:table-cell">
          <>courtney.henry@example.com</>
        </td>
        <td className="border-b border-gray-200 px-3 py-4 text-sm whitespace-nowrap text-gray-500">Admin</td>
      </tr>
      <tr>
        <td className="border-b border-gray-200 py-4 pr-3 pl-4 text-sm font-medium whitespace-nowrap text-gray-900 sm:pl-6 lg:pl-8">
          <>Tom Cook</>
        </td>
        <td className="hidden border-b border-gray-200 px-3 py-4 text-sm whitespace-nowrap text-gray-500 lg:table-cell">
          <>tom.cook@example.com</>
        </td>
        <td className="border-b border-gray-200 px-3 py-4 text-sm whitespace-nowrap text-gray-500">Member</td>
```

----------------------------------------

TITLE: Applying Color Utilities in HTML with Tailwind CSS
DESCRIPTION: This HTML snippet demonstrates Tailwind's recommended approach for handling color adjustments using utility classes directly in the markup. It illustrates how to apply different shades for states like hover, promoting a utility-first workflow over traditional preprocessor color functions.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/compatibility.mdx#_snippet_4

LANGUAGE: html
CODE:
```
<button class="bg-indigo-500 hover:bg-indigo-600 ...">
  <!-- ... -->
</button>
```

----------------------------------------

TITLE: Applying Styles to All Descendants with JSX/React
DESCRIPTION: This JSX snippet demonstrates how to use the `**` variant in Tailwind CSS within a React component to style all descendants. Specifically, it applies `size-12` and `rounded-full` to `img` elements with the `data-avatar` attribute, regardless of their nesting level within the `ul` element.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/hover-focus-and-other-states.mdx#_snippet_91

LANGUAGE: jsx
CODE:
```
<div className="px-4">
  <ul
    role="list"
    className="mx-auto max-w-md border-x border-x-gray-200 p-2 **:data-avatar:size-12 **:data-avatar:rounded-full dark:border-x-gray-800 dark:bg-gray-950/10"
  >
    <li className="group/item relative flex items-center justify-between rounded-xl p-4 hover:bg-gray-100 dark:hover:bg-white/5">
      <div className="flex gap-4">
        <div className="flex-shrink-0">
          <img
            data-avatar
            src="https://images.unsplash.com/photo-1494790108377-be9c29b29330?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=facearea&facepad=2&w=256&h=256&q=80"
            alt=""
          />
        </div>
        <div className="w-full text-sm leading-6">
          <a href="#" className="font-semibold text-gray-900 dark:text-white">
            <span className="absolute inset-0 rounded-xl" aria-hidden="true"></span>Leslie Abbott
          </a>
          <div className="text-gray-500">Co-Founder / CEO</div>
        </div>
      </div>
    </li>
    <li className="group/item relative flex items-center justify-between rounded-xl p-4 hover:bg-gray-100 dark:hover:bg-white/5">
      <div className="flex gap-4">
        <div className="flex-shrink-0">
          <img
            data-avatar
            src="https://images.unsplash.com/photo-1500648767791-00dcc994a43e?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=facearea&facepad=2&w=256&h=256&q=80"
            alt=""
          />
        </div>
        <div className="w-full text-sm leading-6">
          <a href="#" className="font-semibold text-gray-900 dark:text-white">
            <span className="absolute inset-0 rounded-xl" aria-hidden="true"></span>Hector Adams
          </a>
          <div className="text-gray-500">VP, Marketing</div>
        </div>
      </div>
    </li>
    <li className="group/item relative flex items-center justify-between rounded-xl p-4 hover:bg-gray-100 dark:hover:bg-white/5">
      <div className="flex gap-4">
        <div className="flex-shrink-0">
          <img
            data-avatar
            src="https://images.unsplash.com/photo-1507003211169-0a1dd7228f2d?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=facearea&facepad=2&w=256&h=256&q=80"
            alt=""
          />
        </div>
        <div className="w-full text-sm leading-6">
          <a href="#" className="font-semibold text-gray-900 dark:text-white">
            <span className="absolute inset-0 rounded-xl" aria-hidden="true"></span>Blake Alexander
          </a>
          <div className="text-gray-500">Account Coordinator</div>
        </div>
      </div>
    </li>
  </ul>
</div>
```

----------------------------------------

TITLE: Defining Container Query for @max-5xl (Width < 64rem) in CSS
DESCRIPTION: This snippet defines a container query associated with the @max-5xl breakpoint, applying styles when the container's width is less than 64 rem. It's used for responsive design based on the parent container's size rather than the viewport.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/hover-focus-and-other-states.mdx#_snippet_190

LANGUAGE: CSS
CODE:
```
@container (width < 64rem)
```

----------------------------------------

TITLE: Defining Minimum Width Container Query for 4XL Breakpoint in CSS
DESCRIPTION: This CSS snippet defines a container query that applies styles when the container's width is greater than or equal to 56rem (896px). It corresponds to the `@4xl` container breakpoint in Tailwind CSS, enabling responsive design based on parent container dimensions.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/hover-focus-and-other-states.mdx#_snippet_175

LANGUAGE: CSS
CODE:
```
@container (width >= 56rem)
```

----------------------------------------

TITLE: Applying Group-Has Variant in Tailwind CSS (JSX/HTML)
DESCRIPTION: This detailed JSX/HTML snippet showcases the 'group' class applied to parent 'div' elements and the 'group-has-[a]:block' utility class on an SVG icon. The SVG icon is initially hidden ('hidden') but becomes visible ('block') only when an '<a>' (anchor) tag is present as a descendant within the 'group' parent, illustrating conditional styling based on descendant elements.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/hover-focus-and-other-states.mdx#_snippet_17

LANGUAGE: HTML
CODE:
```
<div className="mx-auto grid max-w-md divide-y divide-gray-100 border-x border-x-gray-200 text-gray-700 dark:divide-gray-800 dark:border-x-gray-800 dark:bg-gray-950/10 dark:text-gray-300">
  <div className="group grid grid-cols-[32px_1fr_auto] items-center gap-x-4 px-4 py-4 pt-6">
    <img
      className="size-[32px] rounded-full"
      src="https://spotlight.tailwindui.com/_next/image?url=%2F_next%2Fstatic%2Fmedia%2Favatar.51a13c67.jpg&w=128&q=80"
      alt=""
    />
    {/* This is not an h4 because we're converting h4's to links in MDX files */}
    <div className="font-semibold text-gray-900 dark:text-white">Spencer Sharp</div>
    <svg
      fill="none"
      viewBox="0 0 24 24"
      strokeWidth="1.5"
      stroke="currentColor"
      className="hidden size-4 group-has-[a]:block"
    >
      <path strokeLinecap="round" strokeLinejoin="round" d="M4.5 19.5l15-15m0 0H8.25m11.25 0v11.25" />
    </svg>
    <p className="col-start-2 text-sm">
      Product Designer at{" "}
      <a href="#" className="dark;text-blue-400 text-blue-500 underline">
        planeteria.tech
      </a>
    </p>
  </div>
  <div className="group grid grid-cols-[32px_1fr_auto] items-center gap-x-4 px-4 py-4">
    <img
      className="size-[32px] rounded-full"
      src="https://images.unsplash.com/photo-1539571696357-5a69c17a67c6?q=80&w=256&h=256&auto=format&fit=crop&ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D"
      alt=""
    />
    {/* This is not an h4 because we're converting h4's to links in MDX files */}
    <div className="font-semibold text-gray-900 dark:text-white">Casey Jordan</div>
    <svg
      fill="none"
      viewBox="0 0 24 24"
      strokeWidth="1.5"
      stroke="currentColor"
      className="hidden size-4 group-has-[a]:block"
    >
      <path strokeLinecap="round" strokeLinejoin="round" d="M4.5 19.5l15-15m0 0H8.25m11.25 0v11.25" />
    </svg>
    <p className="col-start-2 text-sm">Just happy to be here.</p>
  </div>
  <div className="group grid grid-cols-[32px_1fr_auto] items-center gap-x-4 px-4 py-4">
    <img
      className="size-[32px] rounded-full"
      src="https://images.unsplash.com/photo-1590895340509-793cb98788c9?q=80&w=256&h=256&&auto=format&fit=crop&ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D"
      alt=""
    />
    {/* This is not an h4 because we're converting h4's to links in MDX files */}
    <div className="font-semibold text-gray-900 dark:text-white">Alex Reed</div>
    <svg
      fill="none"
      viewBox="0 0 24 24"
      strokeWidth="1.5"
      stroke="currentColor"
      className="hidden size-4 group-has-[a]:block"
    >
      <path strokeLinecap="round" strokeLinejoin="round" d="M4.5 19.5l15-15m0 0H8.25m11.25 0v11.25" />
    </svg>
    <p className="col-start-2 text-sm">
      A multidisciplinary designer, working at the intersection of art and technology. <br />
      <br />
      <a href="#" className="dark;text-blue-400 text-blue-500 underline">
        alex-reed.com
      </a>
    </p>
  </div>
  <div className="group grid grid-cols-[32px_1fr_auto] items-center gap-x-4 px-4 py-4 pb-6">
    <img
      className="size-[32px] rounded-full"
      src="https://images.unsplash.com/photo-1517841905240-472988babdf9?q=80&w=256&h=256&auto=format&fit=crop&ixlib=rb-4.0.3&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D"
      alt=""
    />
    {/* This is not an h4 because we're converting h4's to links in MDX files */}
    <div className="font-semibold text-gray-900 dark:text-white">Taylor Bailey</div>
    <svg
      fill="none"
      viewBox="0 0 24 24"
      strokeWidth="1.5"
      stroke="currentColor"
      className="hidden size-4 group-has-[a]:block"
    >
      <path strokeLinecap="round" strokeLinejoin="round" d="M4.5 19.5l15-15m0 0H8.25m11.25 0v11.25" />
    </svg>
    <p className="col-start-2 text-sm">Pushing pixels. Slinging divs.</p>
  </div>
</div>
```

----------------------------------------

TITLE: Applying Responsive Grid Columns in HTML with Tailwind CSS
DESCRIPTION: This HTML snippet shows how to apply responsive grid column classes in Tailwind CSS using breakpoint prefixes. The `grid-cols-2` class applies by default, and the `sm:grid-cols-3` class overrides it at the `sm` breakpoint and above, changing the grid to 3 columns. This requires a Tailwind CSS setup.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/styling-with-utility-classes.mdx#_snippet_10

LANGUAGE: HTML
CODE:
```
<!-- [!code classes:sm:grid-cols-3] -->
<div class="grid grid-cols-2 sm:grid-cols-3">
  <!-- ... -->
</div>
```

----------------------------------------

TITLE: Rotating SVG Icon with Group ARIA Sort State in HTML
DESCRIPTION: This HTML snippet demonstrates how to use `group-aria-*` modifiers in Tailwind CSS to style child elements based on a parent's ARIA attribute. An SVG icon's rotation is controlled by the `aria-sort` state of its parent `<th>` element, showcasing conditional styling for nested elements.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/blog/tailwindcss-v3-2/index.mdx#_snippet_9

LANGUAGE: HTML
CODE:
```
<!-- [!code word:group-aria-[sort=ascending\]\:rotate-0] -->
<!-- [!code word:group-aria-[sort=descending\]\:rotate-180] -->
<table>
  <thead>
    <tr>
      <th aria-sort="ascending" class="group">
        Invoice #
        <svg
          class="group-aria-[sort=ascending]:rotate-0 group-aria-[sort=descending]:rotate-180"
        >
          <!-- ... -->
        </svg>
      </th>
      <!-- ... -->
    </tr>
  </thead>
  <!-- ... -->
</table>
```

----------------------------------------

TITLE: Applying Group Data Attribute Variant (Will Apply) in HTML
DESCRIPTION: This HTML snippet demonstrates the `group-data-*` variant. The `p-8` utility will be applied to the inner div because its parent element has the `group` class and a `data-size` attribute matching `large`, as specified in `group-data-[size=large]:p-8`.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/blog/tailwindcss-v3-2/index.mdx#_snippet_17

LANGUAGE: HTML
CODE:
```
<div data-size="large" class="group">
  <div class="group-data-[size=large]:p-8">
    <!-- Will apply `p-8` -->
  </div>
</div>
```

----------------------------------------

TITLE: Migrating CSS Imports from @tailwind Directives (CSS)
DESCRIPTION: This snippet shows the new method for importing Tailwind CSS in v4. Unlike v3, which used `@tailwind` directives (e.g., `@tailwind base;`), v4 now uses a standard CSS `@import` statement. This simplifies the integration of Tailwind CSS into your stylesheets.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/upgrade-guide.mdx#_snippet_4

LANGUAGE: css
CODE:
```
@import "tailwindcss";
```

----------------------------------------

TITLE: Styling on Focus-Visible with Tailwind CSS (HTML)
DESCRIPTION: This snippet demonstrates styling an element when it has been focused specifically via keyboard navigation, using the `focus-visible` variant. The `focus-visible:outline-2` class applies a 2-pixel outline when the button is focused by keyboard.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/hover-focus-and-other-states.mdx#_snippet_230

LANGUAGE: HTML
CODE:
```
<button class="focus-visible:outline-2 ...">Submit</button>
```

----------------------------------------

TITLE: Implementing Floating Labels with Peer and Placeholder-Shown in HTML
DESCRIPTION: This HTML example illustrates how to create a floating label effect using a combination of `peer-*` variants and the new `placeholder-shown` pseudo-class variant. The label's position adjusts based on whether the input has a placeholder shown or is focused, providing a dynamic UI.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/blog/tailwindcss-2-2/index.mdx#_snippet_11

LANGUAGE: HTML
CODE:
```
<div class="relative">
  <input id="name" class="peer ..." />
  <label for="name" class="peer-placeholder-shown:top-4 peer-focus:top-0 ..."> Name </label>
</div>
```

----------------------------------------

TITLE: Updating CSS Variable Syntax in Tailwind CSS v4 Arbitrary Values
DESCRIPTION: Tailwind CSS v4 updates the syntax for using CSS variables in arbitrary values to resolve ambiguity. This HTML snippet demonstrates the migration from the old square bracket syntax (e.g., `bg-[--brand-color]`) to the new parenthesis syntax (e.g., `bg-(--brand-color)`).
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/upgrade-guide.mdx#_snippet_39

LANGUAGE: HTML
CODE:
```
<div class="bg-[--brand-color]"></div>
<div class="bg-(--brand-color)"></div>
```

----------------------------------------

TITLE: Creating a Custom Dropdown Menu with Headless UI Menu Component (React)
DESCRIPTION: This example illustrates how to build a custom dropdown menu using the Headless UI `Menu` component in React. It utilizes compound components like `Menu.Button`, `Menu.Items`, and `Menu.Item` to manage dropdown state and accessibility. The `Menu.Item` component uses a render prop to expose the `active` state, allowing for dynamic styling based on user interaction.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/blog/building-react-and-vue-support-for-tailwind-ui/index.mdx#_snippet_2

LANGUAGE: JSX
CODE:
```
import { Menu } from "@headlessui/react";

function MyDropdown() {
  return (
    <Menu as="div" className="relative">
      <Menu.Button className="rounded bg-blue-600 px-4 py-2 text-white ...">Options</Menu.Button>
      <Menu.Items className="absolute right-0 mt-1">
        <Menu.Item>
          {({ active }) => (
            <a className={`${active && "bg-blue-500 text-white"} ...`} href="/account-settings">
              Account settings
            </a>
          )}
        </Menu.Item>
        <Menu.Item>
          {({ active }) => (
            <a className={`${active && "bg-blue-500 text-white"} ...`} href="/documentation">
              Documentation
            </a>
          )}
        </Menu.Item>
        <Menu.Item disabled>
          <span className="opacity-75 ...">Invite a friend (coming soon!)</span>
        </Menu.Item>
      </Menu.Items>
    </Menu>
  );
}
```

----------------------------------------

TITLE: Payment Method Selection UI with HTML and Tailwind CSS
DESCRIPTION: This snippet defines the HTML structure for selecting payment methods (Apple Pay, Credit Card) using radio buttons. It applies Tailwind CSS classes for layout, styling, and interactive states, particularly using `has-[:checked]` variants to change appearance when a radio button is selected.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/blog/tailwindcss-v3-4/index.mdx#_snippet_2

LANGUAGE: HTML
CODE:
```
<label
  htmlFor="apple-pay"
  className="grid grid-cols-[24px_1fr_auto] items-center gap-6 rounded-lg p-4 text-slate-700 ring-2 ring-transparent hover:bg-slate-100 has-[:checked]:bg-indigo-50 has-[:checked]:text-indigo-900 has-[:checked]:ring-indigo-500"
>
  <svg className="w-8" viewBox="0 0 24 11" fill="none">
    <path d="M4.38526 1.86704C4.10401 2.19606 3.65392 2.45565 3.20387 2.41854C3.14762 1.97367 3.36793 1.50091 3.62579 1.20892C3.90704 0.870635 4.39932 0.62962 4.79781 0.611084C4.84468 1.07453 4.66182 1.52871 4.38526 1.86704ZM4.79312 2.50663C4.14146 2.46956 3.5836 2.87272 3.27418 2.87272C2.96012 2.87272 2.48659 2.52517 1.97092 2.53443C1.30056 2.5437 0.677025 2.91906 0.33479 3.51694C-0.368428 4.71265 0.151978 6.48308 0.831712 7.45632C1.16457 7.9383 1.56306 8.46662 2.0881 8.44809C2.58507 8.42955 2.78195 8.12834 3.38204 8.12834C3.98677 8.12834 4.1
```

----------------------------------------

TITLE: Using Complete Class Names in HTML with Tailwind
DESCRIPTION: This HTML snippet demonstrates the correct approach for applying conditional classes in Tailwind CSS. By ensuring that the full class names (`text-red-600`, `text-green-600`) are present as complete strings, Tailwind's plain-text scanner can successfully detect and generate the corresponding CSS.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/detecting-classes-in-source-files.mdx#_snippet_2

LANGUAGE: HTML
CODE:
```
<div class="{{ error ? 'text-red-600' : 'text-green-600' }}"></div>
```

----------------------------------------

TITLE: Custom Focus Styles with JSX and Tailwind CSS
DESCRIPTION: This JSX snippet demonstrates how to remove the default browser outline using `outline-none` on a `textarea` and apply custom focus styles to its parent container using `focus-within:outline-2` and `focus-within:outline-indigo-600`. It also shows a button with its own `focus:outline` styles, emphasizing the importance of accessibility when removing default outlines.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/outline-style.mdx#_snippet_5

LANGUAGE: jsx
CODE:
```
<div className="mx-auto flex max-w-md flex-col rounded-lg outline-1 outline-gray-300 focus-within:outline-2 focus-within:outline-indigo-600 dark:bg-white/5 dark:outline-transparent dark:focus-within:outline-indigo-500">
  <textarea className="w-full resize-none p-2 outline-none" placeholder="Leave a comment..." />
  <button
    className="mr-2 mb-2 inline-flex items-center self-end rounded-md bg-indigo-600 px-3 py-1.5 text-sm font-semibold text-white shadow-xs hover:bg-indigo-500 focus:outline-2 focus:outline-offset-2 focus:outline-indigo-600"
    type="button"
  >
    Post
  </button>
</div>
```

----------------------------------------

TITLE: Applying Group Hover Styling in HTML
DESCRIPTION: This HTML snippet demonstrates the use of the `group-hover` utility in Tailwind CSS v2.0. By applying `group` to a parent element, child elements can react to the parent's hover state, enabling more complex interactive UI components. `group-hover` and `focus-within` are now enabled by default for relevant utilities.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/blog/tailwindcss-v2/index.mdx#_snippet_26

LANGUAGE: html
CODE:
```
<div class="group ...">
  <span class="group-hover:text-blue-600 ...">Da ba dee da ba daa</span>
</div>
```

----------------------------------------

TITLE: Tailwind CSS Display Utilities Example (HTML)
DESCRIPTION: Demonstrates the use of `inline`, `inline-block`, and `block` Tailwind CSS classes directly in HTML to manage how text and elements are displayed and flow within a document.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/display.mdx#_snippet_4

LANGUAGE: HTML
CODE:
```
<!-- [!code classes:inline,inline-block,block] -->
<p>
  When controlling the flow of text, using the CSS property <span class="inline">display: inline</span> will cause the
  text inside the element to wrap normally.
</p>
<p>
  While using the property <span class="inline-block">display: inline-block</span> will wrap the element to prevent the
  text inside from extending beyond its parent.
</p>
<p>
  Lastly, using the property <span class="block">display: block</span> will put the element on its own line and fill its
  parent.
</p>
```

----------------------------------------

TITLE: Referencing Theme Variables in HTML Arbitrary Values
DESCRIPTION: This HTML snippet demonstrates how to use the exposed native CSS variables directly within Tailwind's arbitrary value syntax. This eliminates the need for the theme() function, simplifying the process of applying theme-defined values to properties like padding or margin.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/blog/tailwindcss-v4-alpha/index.mdx#_snippet_9

LANGUAGE: html
CODE:
```
<!-- [!code filename:index.html] -->
<div class="p-[calc(var(--spacing-6)-1px)]">
  <!-- ... -->
</div>
```

----------------------------------------

TITLE: HTML Example for Data Attribute-based Dark Mode Toggling
DESCRIPTION: This HTML snippet illustrates how setting `data-theme="dark"` on the `<html>` element activates dark mode utilities. Similar to the class-based approach, `dark:bg-black` will apply when this attribute is present. This method offers semantic clarity for theme management.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/dark-mode.mdx#_snippet_4

LANGUAGE: html
CODE:
```
<html data-theme="dark">
  <body>
    <div class="bg-white dark:bg-black">
      <!-- ... -->
    </div>
  </body>
</html>
```

----------------------------------------

TITLE: Styling Odd/Even Table Rows with Tailwind CSS in Svelte
DESCRIPTION: This Svelte snippet demonstrates how to apply `odd` and `even` background colors to table rows using Tailwind CSS classes. It iterates over a `people` array, dynamically applying `odd:bg-white`, `even:bg-gray-50`, and their dark mode equivalents to each row. This allows for visually distinguishing alternating rows in a table.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/hover-focus-and-other-states.mdx#_snippet_8

LANGUAGE: Svelte
CODE:
```
<!-- [!code classes:odd:bg-white,even:bg-gray-50,dark:odd:bg-gray-900/50,dark:even:bg-gray-950] -->
<table>
  <!-- ... -->
  <tbody>
    {#each people as person}
      <!-- Use different background colors for odd and even rows -->
      <tr class="odd:bg-white even:bg-gray-50 dark:odd:bg-gray-900/50 dark:even:bg-gray-950">
        <td>{person.name}</td>
        <td>{person.title}</td>
        <td>{person.email}</td>
      </tr>
    {/each}
  </tbody>
</table>
```

----------------------------------------

TITLE: Aligning Dropdown Menu Items with Subgrid in Tailwind CSS HTML
DESCRIPTION: This example illustrates the practical application of `grid-cols-subgrid` for aligning items within a dropdown menu. By using subgrid, items with icons can be aligned with those without, ensuring consistent text alignment across all menu options, which is crucial for a clean UI.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/blog/tailwindcss-v3-4/index.mdx#_snippet_10

LANGUAGE: HTML
CODE:
```
<div role="menu" class="grid grid-cols-[auto_1fr]">
  <!-- [!code word:grid-cols-subgrid] -->
  <a href="#" class="col-span-2 grid-cols-subgrid">
    <svg class="mr-2">...</svg>
    <span class="col-start-2">Account</span>
  </a>
  <a href="#" class="col-span-2 grid-cols-subgrid">
    <svg class="mr-2">...</svg>
    <span class="col-start-2">Settings</span>
  </a>
  <a href="#" class="col-span-2 grid-cols-subgrid">
    <span class="col-start-2">Sign out</span>
  </a>
</div>
```

----------------------------------------

TITLE: Exporting Page Metadata in JSX
DESCRIPTION: Exports constants `title` and `description` which define the page's primary heading and a brief summary of its content. These exports are typically used by a layout or routing system to display page-specific metadata.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/columns.mdx#_snippet_1

LANGUAGE: JSX
CODE:
```
export const title = "columns";
export const description = "Utilities for controlling the number of columns within an element.";
```

----------------------------------------

TITLE: Defining Purple Oklch Color Palette in CSS
DESCRIPTION: This snippet defines a comprehensive set of purple color shades using CSS custom properties and the Oklch color model. These variables are essential for maintaining a unified purple color scheme across a project, facilitating easy updates and consistency.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/colors.mdx#_snippet_22

LANGUAGE: CSS
CODE:
```
--color-purple-50: oklch(0.977 0.014 308.299);
--color-purple-100: oklch(0.946 0.033 307.174);
--color-purple-200: oklch(0.902 0.063 306.703);
--color-purple-300: oklch(0.827 0.119 306.383);
--color-purple-400: oklch(0.714 0.203 305.504);
--color-purple-500: oklch(0.627 0.265 303.9);
--color-purple-600: oklch(0.558 0.288 302.321);
--color-purple-700: oklch(0.496 0.265 301.924);
--color-purple-800: oklch(0.438 0.218 303.724);
--color-purple-900: oklch(0.381 0.176 304.987);
--color-purple-950: oklch(0.291 0.149 302.717);
```

----------------------------------------

TITLE: Specifying Ring Color in Tailwind CSS v4 (HTML)
DESCRIPTION: This HTML snippet demonstrates how to explicitly add `ring-blue-500` when using the `ring` utility in Tailwind CSS v4. The default ring color changed from `blue-500` to `currentColor`, requiring explicit color specification to preserve the v3 behavior.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/upgrade-guide.mdx#_snippet_28

LANGUAGE: HTML
CODE:
```
<button class="focus:ring-3 focus:ring-blue-500 ...">
  <!-- ... -->
</button>
```

----------------------------------------

TITLE: Replacing Deprecated divide-opacity-* with Tailwind CSS Opacity Modifiers
DESCRIPTION: This snippet demonstrates replacing the deprecated `divide-opacity-*` utility with the modern Tailwind CSS opacity modifier syntax, such as `divide-black/50`, for controlling divide color opacity.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/upgrade-guide.mdx#_snippet_8

LANGUAGE: Tailwind CSS
CODE:
```
divide-opacity-*
```

LANGUAGE: Tailwind CSS
CODE:
```
divide-black/50
```

----------------------------------------

TITLE: Mapping Props to Static Class Names in JSX with Tailwind
DESCRIPTION: This JSX snippet demonstrates the recommended way to handle conditional styling with props in Tailwind CSS. By mapping prop values to complete, static class name strings within an object, Tailwind's plain-text scanner can easily detect all possible class names at build-time, ensuring proper CSS generation.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/detecting-classes-in-source-files.mdx#_snippet_4

LANGUAGE: JSX
CODE:
```
function Button({ color, children }) {
  const colorVariants = {
    blue: "bg-blue-600 hover:bg-blue-500",
    red: "bg-red-600 hover:bg-red-500"
  };

  return <button className={`${colorVariants[color]} ...`}>{children}</button>;
}
```

----------------------------------------

TITLE: Responsive Visibility with not-sr-only (HTML)
DESCRIPTION: This example illustrates how to use the `not-sr-only` utility class, often with a responsive prefix like `sm:`, to conditionally make an element visible. When combined with `sr-only`, `sm:not-sr-only` ensures the element is hidden on small screens but becomes visible on screens larger than the `sm` breakpoint, providing flexible control over element visibility for both sighted users and screen readers.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/display.mdx#_snippet_16

LANGUAGE: html
CODE:
```
<!-- [!code classes:sm:not-sr-only] -->
<a href="#">
  <svg><!-- ... --></svg>
  <span class="sr-only sm:not-sr-only">Settings</span>
</a>
```

----------------------------------------

TITLE: Applying Background Colors in HTML
DESCRIPTION: This snippet demonstrates how to apply various background colors to HTML elements using Tailwind CSS utility classes such as `bg-blue-500`, `bg-cyan-500`, and `bg-pink-500`. These classes directly control the `background-color` CSS property.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/background-color.mdx#_snippet_0

LANGUAGE: html
CODE:
```
<!-- [!code classes:bg-blue-500,bg-cyan-500,bg-pink-500] -->
<button class="bg-blue-500 ...">Button A</button>
<button class="bg-cyan-500 ...">Button B</button>
<button class="bg-pink-500 ...">Button C</button>
```

----------------------------------------

TITLE: Basic Text Color Usage in React/JSX
DESCRIPTION: This React/JSX snippet demonstrates applying basic text color utilities like `text-blue-600` and `dark:text-sky-400` to a paragraph element, showcasing how to set text color for different themes within a React component.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/color.mdx#_snippet_1

LANGUAGE: jsx
CODE:
```
<div className="relative text-center text-xl leading-6 font-medium">
  <p className="text-blue-600 dark:text-sky-400">The quick brown fox jumps over the lazy dog.</p>
</div>
```

----------------------------------------

TITLE: Updating Vue Components to Script Setup Syntax - Vue 3
DESCRIPTION: This snippet illustrates how to refactor a Vue 3 single-file component to utilize the modern `<script setup>` syntax. It demonstrates importing reactive state (`ref`) from Vue and UI components from `@headlessui/vue` and `@heroicons/vue/solid`, making them implicitly available to the template. This approach significantly reduces boilerplate by removing the need for explicit component registration.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/blog/2022-05-23-headless-ui-v1-6-tailwind-ui-team-management/index.mdx#_snippet_5

LANGUAGE: html
CODE:
```
<template>
  <Listbox as="div" v-model="selected">
    <!-- ... -->
  </Listbox>
</template>

<script setup>
  import { ref } from "vue";
  import { Listbox, ListboxButton, ListboxLabel, ListboxOption, ListboxOptions } from "@headlessui/vue";
  import { CheckIcon, SelectorIcon } from "@heroicons/vue/solid";

  const people = [
    { id: 1, name: "Wade Cooper" },
    // ...
  ];

  const selected = ref(people[3]);
</script>
```

----------------------------------------

TITLE: Installing Tailwind CSS v3.0 with npm
DESCRIPTION: This shell command installs the latest version of Tailwind CSS (v3.0), PostCSS, and Autoprefixer as development dependencies using npm. These packages are crucial for compiling and processing Tailwind CSS in a project, enabling the use of its utility-first classes.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/blog/tailwindcss-v3/index.mdx#_snippet_1

LANGUAGE: Shell
CODE:
```
npm install -D tailwindcss@latest postcss autoprefixer
```

----------------------------------------

TITLE: Preserving Default Ring Styles in Tailwind CSS v4 (CSS)
DESCRIPTION: This CSS snippet shows how to add theme variables to preserve the Tailwind CSS v3 default ring width and color in v4. By defining `--default-ring-width` and `--default-ring-color` within `@theme`, the `ring` utility will behave as it did in v3, though this is for compatibility only.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/upgrade-guide.mdx#_snippet_29

LANGUAGE: CSS
CODE:
```
@theme {
  --default-ring-width: 3px;
  --default-ring-color: var(--color-blue-500);
}
```

----------------------------------------

TITLE: Customizing Tailwind CSS Color Palette in JavaScript
DESCRIPTION: This JavaScript snippet shows how to customize the default color palette in `tailwind.config.js` using the new `tailwindcss/colors` module. It imports specific color sets like `trueGray`, `indigo`, `rose`, and `amber` to define a custom theme color configuration, allowing developers to curate their own palette.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/blog/tailwindcss-v2/index.mdx#_snippet_2

LANGUAGE: js
CODE:
```
const colors = require("tailwindcss/colors");

module.exports = {
  theme: {
    colors: {
      gray: colors.trueGray,
      indigo: colors.indigo,
      red: colors.rose,
      yellow: colors.amber
    }
  }
};
```

----------------------------------------

TITLE: Applying the Hidden Utility with Tailwind CSS in HTML
DESCRIPTION: This HTML snippet shows how to use the `hidden` Tailwind CSS utility class to remove an element from the document. Applying `class="hidden"` to a `div` element effectively sets its `display` property to `none`, making it disappear from the layout and not affect other elements' positioning.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/display.mdx#_snippet_14

LANGUAGE: html
CODE:
```
<!-- [!code classes:hidden] -->
<div class="flex ...">
  <div class="hidden ...">01</div>
  <div>02</div>
  <div>03</div>
</div>
```

----------------------------------------

TITLE: Styling on Focus-Within with Tailwind CSS (HTML)
DESCRIPTION: This snippet illustrates how to apply styles to a parent element when it or any of its descendants has focus, using the `focus-within` variant. The `focus-within:shadow-lg` class adds a large shadow to the div if the nested input is focused.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/hover-focus-and-other-states.mdx#_snippet_229

LANGUAGE: HTML
CODE:
```
<div class="focus-within:shadow-lg ...">
  <input type="text" />
</div>
```

----------------------------------------

TITLE: Defining Custom Design Tokens with @theme Directive (CSS)
DESCRIPTION: The @theme directive allows you to define custom design tokens such as fonts, breakpoints, colors, and easing functions directly within your CSS. These tokens can then be referenced elsewhere in your styles.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/functions-and-directives.mdx#_snippet_1

LANGUAGE: CSS
CODE:
```
@theme {
  --font-display: "Satoshi", "sans-serif";

  --breakpoint-3xl: 120rem;

  --color-avocado-100: oklch(0.99 0 0);
  --color-avocado-200: oklch(0.98 0.04 113.22);
  --color-avocado-300: oklch(0.94 0.11 115.03);
  --color-avocado-400: oklch(0.92 0.19 114.08);
  --color-avocado-500: oklch(0.84 0.18 117.33);
  --color-avocado-600: oklch(0.53 0.12 118.34);

  --ease-fluid: cubic-bezier(0.3, 0, 0, 1);
  --ease-snappy: cubic-bezier(0.2, 0, 0, 1);

  /* ... */
}
```

----------------------------------------

TITLE: Building Component with Tailwind Utilities (HTML)
DESCRIPTION: Demonstrates building a responsive component using Tailwind CSS utility classes in pure HTML. It shows responsive variants (sm:), spacing (gap), layout (flex, items-center), and state variants (hover:, active:) for styling, serving as the HTML equivalent of the React example.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/styling-with-utility-classes.mdx#_snippet_3

LANGUAGE: html
CODE:
```
<!-- [!code classes:sm:flex-row,sm:py-4,sm:gap-6,sm:mx-0,sm:shrink-0,sm:text-left,sm:items-center] -->
<!-- [!code classes:hover:text-white,hover:bg-purple-600,hover:border-transparent,active:bg-purple-700] -->
<div class="flex flex-col gap-2 p-8 sm:flex-row sm:items-center sm:gap-6 sm:py-4 ...">
  <img class="mx-auto block h-24 rounded-full sm:mx-0 sm:shrink-0" src="/img/erin-lindford.jpg" alt="" />
  <div class="space-y-2 text-center sm:text-left">
    <div class="space-y-0.5">
      <p class="text-lg font-semibold text-black">Erin Lindford</p>
      <p class="font-medium text-gray-500">Product Engineer</p>
    </div>
    <!-- prettier-ignore -->
    <button class="border-purple-200 text-purple-600 hover:border-transparent hover:bg-purple-600 hover:text-white active:bg-purple-700 ...">
      Message
    </button>
  </div>
</div>
```

----------------------------------------

TITLE: Fetching All Post Previews with getStaticProps in Next.js
DESCRIPTION: This `getStaticProps` function is responsible for fetching all available post previews at build time. It maps the full post data to lightweight objects, extracting only the title and a cleaned link. This optimized data is then passed as props to the page component, enabling efficient client-side determination of next and previous posts.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/blog/building-the-tailwind-blog/index.mdx#_snippet_9

LANGUAGE: JavaScript
CODE:
```
import getAllPostPreviews from "@/getAllPostPreviews";

export async function getStaticProps() {
  return {
    props: {
      posts: getAllPostPreviews().map((post) => ({
        title: post.module.meta.title,
        link: post.link.substr(1),
      })),
    },
  };
}
```

----------------------------------------

TITLE: Overriding Default Theme Colors in CSS
DESCRIPTION: This CSS snippet illustrates how to completely override a set of default theme variables, such as colors, by setting their namespace to initial (e.g., --color-*: initial) within the @theme directive. This clears the default values, allowing for a full redefinition of the color palette.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/blog/tailwindcss-v4-alpha/index.mdx#_snippet_6

LANGUAGE: css
CODE:
```
/* [!code filename:main.css] */
@import "tailwindcss";

@theme {
  --color-*: initial;

  --color-gray-50: #f8fafc;
  --color-gray-100: #f1f5f9;
  --color-gray-200: #e2e8f0;
  /* ... */
  --color-green-800: #3f6212;
  --color-green-900: #365314;
  --color-green-950: #1a2e05;
}
```

----------------------------------------

TITLE: Installing Tailwind CSS v4 Alpha with Vite
DESCRIPTION: This command installs the alpha version of Tailwind CSS v4 and its new Vite plugin. It's the first step to integrate the new engine into a Vite-based project.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/blog/tailwindcss-v4-alpha/index.mdx#_snippet_11

LANGUAGE: Shell
CODE:
```
npm install tailwindcss@next @tailwindcss/vite@next
```

----------------------------------------

TITLE: Positioning Flex Items Horizontally with flex-row (HTML)
DESCRIPTION: Use `flex-row` to position flex items horizontally in the same direction as text. This utility applies `flex-direction: row;` to the element.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/flex-direction.mdx#_snippet_0

LANGUAGE: html
CODE:
```
<!-- [!code classes:flex-row] -->
<div class="flex flex-row ...">
  <div>01</div>
  <div>02</div>
  <div>03</div>
</div>
```

----------------------------------------

TITLE: Applying Fixed Widths with Tailwind CSS in HTML
DESCRIPTION: This HTML snippet illustrates the direct application of Tailwind CSS `w-<number>` utilities to `div` elements. It shows how to set fixed widths like `w-96`, `w-80`, and `w-64` using class attributes, demonstrating the straightforward way to control element dimensions in a static HTML context.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/width.mdx#_snippet_4

LANGUAGE: html
CODE:
```
<!-- [!code classes:w-96,w-80,w-64,w-48,w-40,w-32,w-24] -->
<div class="w-96 ...">w-96</div>
<div class="w-80 ...">w-80</div>
<div class="w-64 ...">w-64</div>
<div class="w-48 ...">w-48</div>
<div class="w-40 ...">w-40</div>
<div class="w-32 ...">w-32</div>
<div class="w-24 ...">w-24</div>
```

----------------------------------------

TITLE: Applying Diverse Arbitrary Values in HTML
DESCRIPTION: This HTML snippet shows the versatility of Tailwind CSS's arbitrary value notation across different CSS properties. It applies a custom background color, font size, and pseudo-element content, demonstrating how to break out of predefined design tokens for specific styling needs.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/adding-custom-styles.mdx#_snippet_3

LANGUAGE: HTML
CODE:
```
<div class="bg-[#bada55] text-[22px] before:content-['Festivus']">
  <!-- ... -->
</div>
```

----------------------------------------

TITLE: Configuring @tailwindcss/forms Plugin (JavaScript)
DESCRIPTION: This JavaScript snippet demonstrates how to include the @tailwindcss/forms plugin in the tailwind.config.js file. Adding this line to the plugins array enables the plugin's base styles, allowing form elements to be easily styled with utility classes. This is a prerequisite for the utility-friendly form styling.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/blog/tailwindcss-v2/index.mdx#_snippet_13

LANGUAGE: JavaScript
CODE:
```
module.exports = {
  // ...
  plugins: [require("@tailwindcss/forms")],
};
```

----------------------------------------

TITLE: Applying `forced-colors` Variant to Theme Selector in JSX
DESCRIPTION: This JSX snippet demonstrates how to build a theme selection form that adapts to `forced-colors` mode using Tailwind CSS. It applies `forced-colors:border-0` to remove borders and `forced-colors:appearance-auto` to restore default radio button appearance, ensuring accessibility and proper rendering when forced colors are active. Hidden `p` tags become visible in forced color mode to display theme names.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/hover-focus-and-other-states.mdx#_snippet_56

LANGUAGE: jsx
CODE:
```
<div className="mx-auto max-w-sm border-x border-x-gray-200 px-6 pt-6 pb-4 text-gray-900 dark:border-x-gray-800 dark:bg-gray-950/10 dark:text-white">
  <form>
    <legend> Choose a theme: </legend>
    <div className="mt-4 grid grid-flow-col">
      <label htmlFor="theme-1" className="text-sm font-medium text-gray-700 dark:text-white">
        <div className="relative grid h-16 w-16 items-center justify-center rounded-xl border border-transparent bg-transparent text-white hover:bg-gray-50 has-checked:border-cyan-500 has-checked:bg-cyan-50 has-checked:text-cyan-50 dark:text-gray-800 dark:hover:bg-gray-800 dark:has-checked:bg-cyan-950 dark:has-checked:text-cyan-950 forced-colors:border-0">
          <input
            type="radio"
            name="themes"
            id="theme-1"
            className="appearance-none forced-colors:appearance-auto"
            defaultChecked
          />
          <p className="hidden forced-colors:block">Cyan</p>
          <div className="absolute top-3 left-3 h-6 w-6 rounded-full bg-cyan-200 forced-colors:hidden"></div>
          <div className="absolute right-3 bottom-3 h-6 w-6 rounded-full bg-cyan-500 ring-2 ring-current forced-colors:hidden"></div>
        </div>
      </label>
      <label htmlFor="theme-2" className="text-sm font-medium text-gray-700 dark:text-white">
        <div className="relative grid h-16 w-16 items-center justify-center rounded-xl border border-transparent bg-transparent text-white hover:bg-gray-50 has-checked:border-blue-500 has-checked:bg-blue-50 has-checked:text-blue-50 dark:text-gray-800 dark:hover:bg-gray-800 dark:has-checked:bg-blue-950 dark:has-checked:text-blue-950 forced-colors:border-0">
          <input
            type="radio"
            name="themes"
            id="theme-2"
            className="appearance-none forced-colors:appearance-auto"
          />
          <p className="hidden forced-colors:block">Blue</p>
          <div className="absolute top-3 left-3 h-6 w-6 rounded-full bg-blue-200 forced-colors:hidden"></div>
          <div className="absolute right-3 bottom-3 h-6 w-6 rounded-full bg-blue-500 ring-2 ring-current forced-colors:hidden"></div>
        </div>
      </label>
      <label htmlFor="theme-3" className="text-sm font-medium text-gray-700 dark:text-white">
        <div className="relative grid h-16 w-16 items-center justify-center rounded-xl border border-transparent bg-transparent text-white hover:bg-gray-50 has-checked:border-indigo-500 has-checked:bg-indigo-50 has-checked:text-indigo-50 dark:text-gray-800 dark:hover:bg-gray-800 dark:has-checked:bg-indigo-950 dark:has-checked:text-indigo-950 forced-colors:border-0">
          <input
            type="radio"
            name="themes"
            id="theme-3"
            className="appearance-none forced-colors:appearance-auto"
          />
          <p className="hidden forced-colors:block">Indigo</p>
          <div className="absolute top-3 left-3 h-6 w-6 rounded-full bg-indigo-200 forced-colors:hidden"></div>
          <div className="absolute right-3 bottom-3 h-6 w-6 rounded-full bg-indigo-500 ring-2 ring-current forced-colors:hidden"></div>
        </div>
      </label>
      <label htmlFor="theme-4" className="text-sm font-medium text-gray-700 dark:text-white">
        <div className="relative grid h-16 w-16 items-center justify-center rounded-xl border border-transparent bg-transparent text-white hover:bg-gray-50 has-checked:border-purple-500 has-checked:bg-purple-50 has-checked:text-purple-50 dark:text-gray-800 dark:hover:bg-gray-800 dark:has-checked:bg-purple-950 dark:has-checked:text-purple-950 forced-colors:border-0">
          <input
            type="radio"
            name="themes"
            id="theme-4"
            className="appearance-none forced-colors:appearance-auto"
          />
          <p className="hidden forced-colors:block">Purple</p>
          <div className="absolute top-3 left-3 h-6 w-6 rounded-full bg-purple-200 forced-colors:hidden"></div>
          <div className="absolute right-3 bottom-3 h-6 w-6 rounded-full bg-purple-500 ring-2 ring-current forced-colors:hidden"></div>
        </div>
      </label>
    </div>
  </form>
</div>
```

----------------------------------------

TITLE: Defining Custom Color Theme Variable in Tailwind CSS
DESCRIPTION: This CSS snippet demonstrates how to define a new custom color, `--color-mint-500`, using the `@theme` directive in `app.css`. This definition instructs Tailwind CSS to generate corresponding utility classes like `bg-mint-500`, `text-mint-500`, or `fill-mint-500`, making the new color available for use throughout the project.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/theme.mdx#_snippet_0

LANGUAGE: css
CODE:
```
/* [!code filename:app.css] */
@import "tailwindcss";

/* [!code highlight:4] */
@theme {
  --color-mint-500: oklch(0.72 0.11 178);
}
```

----------------------------------------

TITLE: Referencing CSS Variables as Arbitrary Values in HTML
DESCRIPTION: This HTML snippet illustrates how to use CSS variables as arbitrary values within Tailwind CSS. The `fill-(--my-brand-color)` syntax is a shorthand for `fill-[var(--my-brand-color)]`, allowing dynamic styling based on custom CSS properties.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/adding-custom-styles.mdx#_snippet_4

LANGUAGE: HTML
CODE:
```
<div class="fill-(--my-brand-color) ...">
  <!-- ... -->
</div>
```

----------------------------------------

TITLE: Implementing Scroll Snapping with snap-end in React/JSX
DESCRIPTION: This React/JSX snippet demonstrates how to create a horizontally scrollable container where child elements snap to their end when scrolled. It utilizes Tailwind CSS classes like `snap-x`, `snap-mandatory`, and `overflow-x-auto` on the parent container, and `shrink-0`, `snap-end`, `scroll-mx-6` on the child elements to achieve the snapping effect. It includes visual indicators and image elements to illustrate the behavior.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/scroll-snap-align.mdx#_snippet_5

LANGUAGE: jsx
CODE:
```
<div className="relative">
  <div className="mr-6 mb-6 flex items-end justify-end pt-10">
    <div className="dark:highlight-white/10 mr-2 rounded bg-indigo-50 px-1.5 font-mono text-[0.625rem] leading-6 text-indigo-600 ring-1 ring-indigo-600 ring-inset dark:bg-indigo-500 dark:text-white dark:ring-0">
      snap point
    </div>
    <div className="absolute top-0 right-6 bottom-0 border-l border-indigo-500"></div>
  </div>
  <div className="relative flex w-full snap-x snap-mandatory gap-6 overflow-x-auto pb-14">
    <div className="shrink-0 snap-end scroll-mx-6">
      <div className="w-3 shrink-0 sm:-mr-[2px] sm:w-10"></div>
    </div>
    <div className="shrink-0 snap-end scroll-mx-6">
      <img
        className="h-40 w-80 shrink-0 rounded-lg bg-white"
        src="https://images.unsplash.com/photo-1604999565976-8913ad2ddb7c?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=320&h=160&q=80"
      />
    </div>
    <div className="shrink-0 snap-end scroll-mx-6">
      <img
        className="h-40 w-80 shrink-0 rounded-lg bg-white"
        src="https://images.unsplash.com/photo-1540206351-d6465b3ac5c1?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=320&h=160&q=80"
      />
    </div>
    <div className="shrink-0 snap-end scroll-mx-6">
      <img
        className="h-40 w-80 shrink-0 rounded-lg bg-white"
        src="https://images.unsplash.com/photo-1622890806166-111d7f6c7c97?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=320&h=160&q=80"
      />
    </div>
    <div className="shrink-0 snap-end scroll-mx-6">
      <img
        className="h-40 w-80 shrink-0 rounded-lg bg-white"
        src="https://images.unsplash.com/photo-1590523277543-a94d2e4eb00b?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=320&h=160&q=80"
      />
    </div>
    <div className="shrink-0 snap-end scroll-mx-6">
      <img
        className="h-40 w-80 shrink-0 rounded-lg bg-white"
        src="https://images.unsplash.com/photo-1575424909138-46b05e5919ec?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=320&h=160&q=80"
      />
    </div>
    <div className="shrink-0 snap-end scroll-mx-6 pr-6">
      <img
        className="h-40 w-80 shrink-0 rounded-lg bg-white"
        src="https://images.unsplash.com/photo-1559333086-b0a56225a93c?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=320&h=160&q=80"
      />
    </div>
  </div>
</div>
```

----------------------------------------

TITLE: Using Custom DescriptionList Components in JSX
DESCRIPTION: This example demonstrates the usage of custom React components (`DescriptionList`, `DescriptionTerm`, `DescriptionDetails`) to render a description list. This approach provides a cleaner, component-based API, abstracting away the underlying HTML and Tailwind CSS for easier maintenance and reuse.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/blog/2024-05-24-catalyst-application-layouts/index.mdx#_snippet_3

LANGUAGE: jsx
CODE:
```
import { DescriptionDetails, DescriptionList, DescriptionTerm } from "@/components/description-list";

function Example() {
  return (
    <DescriptionList>
      <DescriptionTerm>Customer</DescriptionTerm>
      <DescriptionDetails>Michael Foster</DescriptionDetails>

      <DescriptionTerm>Event</DescriptionTerm>
      <DescriptionDetails>Bear Hug: Live in Concert</DescriptionDetails>

      {/* ... */}
    </DescriptionList>
  );
}
```

----------------------------------------

TITLE: Implementing Sticky Headers with Tailwind CSS HTML
DESCRIPTION: This HTML snippet demonstrates how to apply `sticky` and `top-0` Tailwind CSS classes to a `div` element, making it stick to the top of its container when scrolled. This pattern is commonly used for section headers in long lists or directories, ensuring they remain visible as the user scrolls through the content. The `...` indicates additional content within the sticky header.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/position.mdx#_snippet_7

LANGUAGE: HTML
CODE:
```
<!-- [!code classes:sticky,top-0] -->
<div>
  <div>
    <div class="sticky top-0 ...">A</div>
    <div>
      <div>
        <img src="/img/andrew.jpg" />
        <strong>Andrew Alfred</strong>
      </div>
      <div>
        <img src="/img/aisha.jpg" />
        <strong>Aisha Houston</strong>
      </div>
      <!-- ... -->
    </div>
  </div>
  <div>
    <div class="sticky top-0">B</div>
    <div>
      <div>
        <img src="/img/bob.jpg" />
        <strong>Bob Alfred</strong>
      </div>
      <!-- ... -->
    </div>
  </div>
  <!-- ... -->
</div>
```

----------------------------------------

TITLE: Applying Conditional Scrollbars with overflow-auto in HTML
DESCRIPTION: This HTML snippet illustrates the basic usage of the `overflow-auto` Tailwind CSS utility class. Applying `overflow-auto` to a `div` element ensures that scrollbars appear only when the content within that element overflows its boundaries, providing a cleaner UI when scrolling is not necessary.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/overflow.mdx#_snippet_3

LANGUAGE: html
CODE:
```
<!-- [!code classes:overflow-auto] -->
<div class="overflow-auto ...">
  <!-- ... -->
</div>
```

----------------------------------------

TITLE: Applying Container Query Units in HTML
DESCRIPTION: This HTML snippet shows how to use container query length units, specifically `cqw` (container query width), as arbitrary values within Tailwind CSS utility classes. The `w-[50cqw]` class sets the width of the inner `div` to 50% of its `@container` parent's width, allowing for responsive sizing relative to the container rather than the viewport.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/responsive-design.mdx#_snippet_21

LANGUAGE: html
CODE:
```
<div class="@container">
  <div class="w-[50cqw]">
    <!-- ... -->
  </div>
</div>
```

----------------------------------------

TITLE: Applying Padding When CSS hanging-punctuation Not Supported with Tailwind CSS not-supports Variant (HTML, CSS)
DESCRIPTION: This snippet illustrates the `not-supports` variant in Tailwind CSS, which applies styles when a specific CSS feature is *not* supported by the browser. The HTML shows the class, and the CSS demonstrates how it compiles to an `@supports not` rule, applying horizontal padding if `hanging-punctuation` is not supported.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/blog/tailwindcss-v4/index.mdx#_snippet_25

LANGUAGE: HTML
CODE:
```
<!-- [!code filename:HTML] -->
<!-- [!code classes:not-supports-hanging-punctuation:px-4] -->
<div class="not-supports-hanging-punctuation:px-4">
  <!-- ... -->
</div>
```

LANGUAGE: CSS
CODE:
```
/* [!code filename:CSS] */
.not-supports-hanging-punctuation\:px-4 {
  @supports not (hanging-punctuation: var(--tw)) {
    padding-inline: calc(var(--spacing) * 4);
  }
}
```

----------------------------------------

TITLE: React Example with LTR and RTL Direction
DESCRIPTION: This React component demonstrates the use of `dir="ltr"` and `dir="rtl"` to display content in both left-to-right and right-to-left directions. It uses Tailwind CSS classes for styling, including `ms-3` for margin-inline-start, which adapts automatically in RTL layouts.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/blog/tailwindcss-v3-3/index.mdx#_snippet_5

LANGUAGE: JavaScript
CODE:
```
<div className="mx-auto grid max-w-lg grid-cols-1 gap-x-6 gap-y-10 sm:grid-cols-2">
  <div dir="ltr">
    <p className="mb-4 text-sm font-medium">Left-to-right</p>
    <div className="group flex items-center">
      <img
        className="h-12 w-12 shrink-0 rounded-full"
        src="https://images.unsplash.com/photo-1472099645785-5658abf4ff4e?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=facearea&facepad=2&w=256&h=256&q=80"
        alt=""
      />
      <div className="ms-3">
        <p className="text-sm font-medium text-slate-700 group-hover:text-slate-900 dark:text-slate-300 dark:group-hover:text-white">
          <>Tom Cook</>
        </p>
        <p className="text-sm font-medium text-slate-500 group-hover:text-slate-700 dark:group-hover:text-slate-300">
          <>Director of Operations</>
        </p>
      </div>
    </div>
  </div>
  <div dir="rtl">
    <p className="mb-4 text-sm font-medium">Right-to-left</p>
    <div className="group flex items-center">
      <img
        className="h-12 w-12 shrink-0 rounded-full"
        src="https://images.unsplash.com/photo-1563833717765-00462801314e?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=facearea&facepad=2&w=256&h=256&q=80"
        alt=""
      />
      <div className="ms-3">
        <p className="text-sm font-medium text-slate-700 group-hover:text-slate-900 dark:text-slate-300 dark:group-hover:text-white">
          <>تامر كرم</>
        </p>
        <p className="text-sm font-medium text-slate-500 group-hover:text-slate-700 dark:group-hover:text-slate-300">
          <>الرئيس التنفيذي</>
        </p>
      </div>
    </div>
  </div>
</div>
```

----------------------------------------

TITLE: Cascading Disabled States in Headless UI React Forms
DESCRIPTION: This example demonstrates how Headless UI's `Fieldset` and `Field` components can manage disabled states, similar to native HTML `<fieldset>`. It shows how to conditionally disable a `Field` based on the selection of another input, and how `data-disabled` attributes allow for fine-tuned styling of disabled elements like labels.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/blog/headless-ui-v2/index.mdx#_snippet_5

LANGUAGE: jsx
CODE:
```
import { Button, Description, Field, Fieldset, Input, Label, Legend, Select } from "@headlessui/react";
import { regions } from "./countries";

export function Example() {
  const [country, setCountry] = useState(null);

  return (
    <form action="/shipping">
      <Fieldset>
        <Legend>Shipping details</Legend>
        <Field>
          <Label>Street address</Label>
          <Input name="address" />
        </Field>
        <Field>
          <Label>Country</Label>
          <Description>We currently only ship to North America.</Description>
          <Select name="country" value={country} onChange={(event) => setCountry(event.target.value)}>
            <option></option>
            <option>Canada</option>
            <option>Mexico</option>
            <option>United States</option>
          </Select>
        </Field>
        // [!code highlight:4]
        <Field disabled={!country}>
          <Label className="data-[disabled]:opacity-40">State/province</Label>
          <Select name="region" className="data-[disabled]:opacity-50">
            <option></option>
            {country && regions[country].map((region) => <option>{region}</option>)}
          </Select>
        </Field>
        <Button>Submit</Button>
      </Fieldset>
    </form>
  );
}
```

----------------------------------------

TITLE: Configuring Tailwind CSS with TypeScript Types
DESCRIPTION: This JavaScript configuration file for Tailwind CSS includes a JSDoc type annotation to enable first-party TypeScript type checking. This provides IDE support for the configuration object, making it easier to define content paths, extend themes, and add plugins with autocompletion and validation.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/blog/tailwindcss-v3-1/index.mdx#_snippet_1

LANGUAGE: js
CODE:
```
/** @type {import('tailwindcss').Config} */
module.exports = {
  content: [
    // ...
  ],
  theme: {
    extend: {},
  },
  plugins: []
};
```

----------------------------------------

TITLE: Styling Parent Based on Checked Descendant State (Tailwind CSS)
DESCRIPTION: This HTML snippet demonstrates how to use the `has-checked` pseudo-class in Tailwind CSS to apply styles to a parent `<label>` element when its nested radio input is checked. It showcases conditional styling for background, text, and ring colors, including dark mode variations.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/hover-focus-and-other-states.mdx#_snippet_16

LANGUAGE: HTML
CODE:
```
<label
  class="has-checked:bg-indigo-50 has-checked:text-indigo-900 has-checked:ring-indigo-200 dark:has-checked:bg-indigo-950 dark:has-checked:text-indigo-200 dark:has-checked:ring-indigo-900 ..."
>
  <svg fill="currentColor">
    <!-- ... -->
  </svg>
  Google Pay
  <input type="radio" class="checked:border-indigo-500 ..." />
</label>
```

----------------------------------------

TITLE: Building a Custom Dropdown with Headless UI and React
DESCRIPTION: This snippet demonstrates how to create a custom dropdown menu using the `@headlessui/react` library. It leverages `Menu`, `Menu.Button`, and `Menu.Items` components to provide built-in accessibility features like keyboard navigation and ARIA attribute management. The example shows how to style the components using Tailwind CSS utility classes and includes active and disabled states for menu items, abstracting away complex accessibility logic.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/blog/headless-ui-unstyled-accessible-ui-components/index.mdx#_snippet_0

LANGUAGE: jsx
CODE:
```
import { Menu } from "@headlessui/react";

function MyDropdown() {
  return (
    <Menu as="div" className="relative">
      <Menu.Button className="rounded bg-blue-600 px-4 py-2 text-white ...">Options</Menu.Button>
      <Menu.Items className="absolute right-0 mt-1">
        <Menu.Item>
          {({ active }) => (
            <a className={`${active && "bg-blue-500 text-white"} ...`} href="/account-settings">
              Account settings
            </a>
          )}
        </Menu.Item>
        <Menu.Item>
          {({ active }) => (
            <a className={`${active && "bg-blue-500 text-white"} ...`} href="/documentation">
              Documentation
            </a>
          )}
        </Menu.Item>
        <Menu.Item disabled>
          <span className="opacity-75 ...">Invite a friend (coming soon!)</span>
        </Menu.Item>
      </Menu.Items>
    </Menu>
  );
}
```

----------------------------------------

TITLE: Conditionally Styling Specific Children with Data Attributes and * Variant in JSX
DESCRIPTION: This JSX example illustrates how to combine the `*` variant with data attribute selectors (`data-[slot]`) to conditionally apply styles to specific direct children. The `Field` component uses `data-[slot=description]:*:mt-4` to add top margin only to children with `data-slot="description"`, like the `Description` component. This enables advanced conditional styling from a parent component without complex arbitrary variants.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/blog/tailwindcss-v3-4/index.mdx#_snippet_5

LANGUAGE: jsx
CODE:
```
function Field({ children }) {
  return (
    <div className="data-[slot=description]:*:mt-4 ...">
      {children}
    </div>
  )
}

function Description({ children }) {
  return (
    <p data-slot="description" ...>{children}</p>
  )
}

function Example() {
  return (
    <Field>
      <Label>First name</Label>
      <Input />
      <Description>Please tell me you know your own name.</Description>
    </Field>
  )
}
```

----------------------------------------

TITLE: Styling Form Elements with Tailwind CSS Variants (JSX)
DESCRIPTION: This JSX code demonstrates styling `input` elements using Tailwind CSS pseudo-class variants like `invalid`, `focus`, and `disabled` to visually indicate different states without explicit conditional logic. It shows a form with username, email, and password fields, where the username is disabled and the email has an invalid default value, allowing users to observe state-based styling changes.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/hover-focus-and-other-states.mdx#_snippet_10

LANGUAGE: JSX
CODE:
```
<div className="mx-auto max-w-md border-x border-x-gray-200 px-6 py-5 dark:border-x-gray-800 dark:bg-gray-950/10">
  <form>
    <div>
      <label htmlFor="username" className="block text-sm font-medium text-gray-700 dark:text-gray-300">
        Username
      </label>
      <div className="mt-1">
        <input
          type="text"
          name="username"
          id="username"
          className="block w-full rounded-md border border-gray-300 px-3 py-2 placeholder-gray-400 shadow-sm invalid:border-pink-500 invalid:text-pink-600 focus:border-sky-500 focus:outline focus:outline-sky-500 focus:invalid:border-pink-500 focus:invalid:outline-pink-500 disabled:border-gray-200 disabled:bg-gray-50 disabled:text-gray-500 disabled:shadow-none sm:text-sm dark:disabled:border-gray-700 dark:disabled:bg-gray-800/20"
          defaultValue="tbone"
          disabled
        />
      </div>
    </div>
    <div className="mt-6">
      <label htmlFor="email" className="block text-sm font-medium text-gray-700 dark:text-gray-300">
        Email
      </label>
      <div className="mt-1">
        <input
          type="email"
          name="email"
          id="email-1"
          className="block w-full rounded-md border border-gray-300 px-3 py-2 placeholder-gray-400 shadow-sm invalid:border-pink-500 invalid:text-pink-600 focus:border-sky-500 focus:outline focus:outline-sky-500 focus:invalid:border-pink-500 focus:invalid:outline-pink-500 disabled:border-gray-200 disabled:bg-gray-50 disabled:text-gray-500 disabled:shadow-none sm:text-sm dark:disabled:border-gray-700 dark:disabled:bg-gray-800/20"
          defaultValue="george@krugerindustrial."
          placeholder="you@example.com"
        />
      </div>
    </div>
    <div className="mt-6">
      <label htmlFor="password" className="block text-sm font-medium text-gray-700 dark:text-gray-300">
        Password
      </label>
      <div className="mt-1">
        <input
          type="password"
          name="password"
          id="password"
          autoComplete="none"
          className="block w-full rounded-md border border-gray-300 px-3 py-2 placeholder-gray-400 shadow-sm invalid:border-pink-500 invalid:text-pink-600 focus:border-sky-500 focus:outline focus:outline-sky-500 focus:invalid:border-pink-500 focus:invalid:outline-pink-500 disabled:border-gray-200 disabled:bg-gray-50 disabled:text-gray-500 disabled:shadow-none sm:text-sm dark:disabled:border-gray-700 dark:disabled:bg-gray-800/20"
          defaultValue="Bosco"
        />
      </div>
    </div>
    <div className="mt-6 text-right">
      <button className="rounded-md bg-sky-500 px-5 py-2.5 text-sm leading-5 font-semibold text-white hover:bg-sky-700">
        Save changes
      </button>
    </div>
  </form>
</div>
```

----------------------------------------

TITLE: Specifying Grid Columns with Tailwind CSS (JSX)
DESCRIPTION: This JSX snippet demonstrates how to create a grid layout with a specified number of equally sized columns using Tailwind CSS utilities. It shows a parent `div` with `grid grid-cols-1` and an inner `div` with `grid grid-cols-4` to arrange items within the grid.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/grid-template-columns.mdx#_snippet_0

LANGUAGE: jsx
CODE:
```
<div className="grid grid-cols-1">
  <Stripes border className="col-start-1 row-start-1 rounded-lg" />
  <div className="col-start-1 row-start-1 grid grid-cols-4 gap-4 rounded-lg text-center font-mono text-sm leading-6 font-bold text-white">
    <div className="rounded-lg bg-fuchsia-500 p-4">01</div>
    <div className="rounded-lg bg-fuchsia-500 p-4">02</div>
    <div className="rounded-lg bg-fuchsia-500 p-4">03</div>
    <div className="rounded-lg bg-fuchsia-500 p-4">04</div>
    <div className="rounded-lg bg-fuchsia-500 p-4">05</div>
    <div className="rounded-lg bg-fuchsia-500 p-4">06</div>
    <div className="rounded-lg bg-fuchsia-500 p-4">07</div>
    <div className="rounded-lg bg-fuchsia-500 p-4">08</div>
    <div className="rounded-lg bg-fuchsia-500 p-4">09</div>
  </div>
</div>
```

----------------------------------------

TITLE: Applying Tailwind CSS State Variants to Input (HTML)
DESCRIPTION: This HTML snippet showcases the direct application of Tailwind CSS utility classes with pseudo-class variants (`invalid`, `focus`, `disabled`, `dark:disabled`) to an `<input>` element. It highlights how these variants automatically apply styles based on the input's state, simplifying template logic and reducing the need for manual conditional styling.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/hover-focus-and-other-states.mdx#_snippet_11

LANGUAGE: HTML
CODE:
```
<input
  type="text"
  value="tbone"
  disabled
  class="invalid:border-pink-500 invalid:text-pink-600 focus:border-sky-500 focus:outline focus:outline-sky-500 focus:invalid:border-pink-500 focus:invalid:outline-pink-500 disabled:border-gray-200 disabled:bg-gray-50 disabled:text-gray-500 disabled:shadow-none dark:disabled:border-gray-700 dark:disabled:bg-gray-800/20 ..."
/>
```

----------------------------------------

TITLE: Applying Outline on Focus State with Tailwind CSS HTML
DESCRIPTION: This example illustrates how to conditionally apply an outline to an element when it receives focus. The `focus:outline-2` utility class is used to set a 2px outline width specifically on the focus state, enhancing accessibility and user feedback.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/outline-width.mdx#_snippet_1

LANGUAGE: html
CODE:
```
<!-- [!code classes:focus:outline-2] -->
<button class="outline-offset-2 outline-sky-500 focus:outline-2 ...">Save Changes</button>
```

----------------------------------------

TITLE: Implementing Tabs with Headless UI in React
DESCRIPTION: This snippet demonstrates how to create a basic tab interface using the `Tab` component from `@headlessui/react`. It shows the declarative structure for `Tab.Group`, `Tab.List`, `Tab`, `Tab.Panels`, and `Tab.Panel` to manage tab navigation and content display, abstracting away accessibility concerns.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/blog/headless-ui-v1-4/index.mdx#_snippet_0

LANGUAGE: jsx
CODE:
```
import { Tab } from '@headlessui/react'

function MyTabs() {
  return (
    <Tab.Group>
      <Tab.List>
        <Tab>Tab 1</Tab>
        <Tab>Tab 2</Tab>
        <Tab>Tab 3</Tab>
      </Tab.List>
      <Tab.Panels>
        <Tab.Panel>Content 1</Tab.Panel>
        <Tab.Panel>Content 2</Tab.Panel>
        <Tab.Panel>Content 3</Tab.Panel>
      </Tab.Panels>
    </Tab.Group>
  )
}
```

----------------------------------------

TITLE: Replacing Deprecated bg-opacity-* with Tailwind CSS Opacity Modifiers
DESCRIPTION: This snippet demonstrates replacing the deprecated `bg-opacity-*` utility with the modern Tailwind CSS opacity modifier syntax, such as `bg-black/50`, for controlling background color opacity.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/upgrade-guide.mdx#_snippet_5

LANGUAGE: Tailwind CSS
CODE:
```
bg-opacity-*
```

LANGUAGE: Tailwind CSS
CODE:
```
bg-black/50
```

----------------------------------------

TITLE: Using theme() for Breakpoints with CSS Variables in Tailwind CSS
DESCRIPTION: Shows how to use the `theme()` function for media queries in Tailwind CSS v4, specifically for breakpoints. It highlights the change from dot notation to CSS variable names within the `theme()` function for compatibility in contexts where CSS variables are not directly supported.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/upgrade-guide.mdx#_snippet_44

LANGUAGE: CSS
CODE:
```
@media (width >= theme(screens.xl)) { 
@media (width >= theme(--breakpoint-xl)) { 
  /* ... */
}
```

----------------------------------------

TITLE: Conditionally Hiding 'Required' Text with `peer-optional`
DESCRIPTION: This HTML snippet illustrates the use of the `peer-optional` variant in Tailwind CSS to conditionally hide a 'Required' notice for form fields that are optional. By combining `peer` on the input and `peer-optional:hidden` on the sibling div, the text is only visible for required fields.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/blog/tailwindcss-v3-1/index.mdx#_snippet_16

LANGUAGE: HTML
CODE:
```
<form>
  <div>
    <label for="email" ...>Email</label>
    <div>
      <input required class="peer ..." id="email" />
      <!-- [!code word:peer-optional\:hidden] -->
      <div class="peer-optional:hidden ...">Required</div>
    </div>
  </div>
  <div>
    <label for="name" ...>Name</label>
    <div>
      <input class="peer ..." id="name" />
```

----------------------------------------

TITLE: Conditional Styling with supports-[display:grid] (HTML)
DESCRIPTION: This HTML snippet demonstrates using the `supports-[...]` variant to apply `grid` display only if the browser supports CSS Grid. The `flex` class is applied by default, providing a fallback for browsers without grid support.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/blog/tailwindcss-v3-2/index.mdx#_snippet_3

LANGUAGE: html
CODE:
```
<div class="flex supports-[display:grid]:grid ...">
  <!-- ... -->
</div>
```

----------------------------------------

TITLE: Styling Child Elements Based on Parent State (Tailwind CSS Group Variant)
DESCRIPTION: This snippet demonstrates the `group` variant for styling child elements based on the parent's state. By adding `group` to the parent `<a>` tag and `group-hover:text-white` or `group-hover:stroke-white` to children, the text and SVG icon change color when the entire card is hovered, enabling complex interactive components.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/hover-focus-and-other-states.mdx#_snippet_23

LANGUAGE: jsx
CODE:
```
<a
      href="#"
      className="group mx-auto block max-w-xs space-y-3 rounded-lg bg-white p-4 shadow-lg ring-1 ring-gray-900/5 hover:bg-sky-500 hover:ring-sky-500 dark:bg-white/5 dark:ring-white/10"
    >
      <div className="flex items-center space-x-3">
        <svg className="h-6 w-6 stroke-sky-500 group-hover:stroke-white" fill="none" viewBox="0 0 24 24">
          <path
            strokeLinecap="round"
            strokeLinejoin="round"
            strokeWidth="2"
            d="M11 19H6.931A1.922 1.922 0 015 17.087V8h12.069C18.135 8 19 8.857 19 9.913V11"
          />
          <path
            strokeLinecap="round"
            strokeLinejoin="round"
            strokeWidth="2"
            d="M14 7.64L13.042 6c-.36-.616-1.053-1-1.806-1H7.057C5.921 5 5 5.86 5 6.92V11M17 15v4M19 17h-4"
          />
        </svg>
        {/* This is not an h3 because we're converting h3's to links in MDX files */}
        <div className="text-sm font-semibold text-gray-900 group-hover:text-white dark:text-white">New project</div>
      </div>
      <p className="text-sm text-gray-500 group-hover:text-white dark:text-gray-400">
        Create a new project from a variety of starting templates.
      </p>
    </a>
```

LANGUAGE: html
CODE:
```
<!-- [!code classes:group-hover:stroke-white] -->
<!-- [!code classes:group-hover:text-white] -->
<!-- [!code classes:group-hover:text-white] -->
<!-- [!code classes:group] -->
<a href="#" class="group ...">
  <div>
    <svg class="stroke-sky-500 group-hover:stroke-white ..." fill="none" viewBox="0 0 24 24">
      <!-- ... -->
    </svg>
    <h3 class="text-gray-900 group-hover:text-white ...">New project</h3>
  </div>
  <p class="text-gray-500 group-hover:text-white ...">Create a new project from a variety of starting templates.</p>
</a>
```

----------------------------------------

TITLE: Installing Tailwind CSS v3.4
DESCRIPTION: Installs the latest version of Tailwind CSS using npm, providing access to all new features introduced in v3.4.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/blog/tailwindcss-v3-4/index.mdx#_snippet_0

LANGUAGE: sh
CODE:
```
npm install tailwindcss@latest
```

----------------------------------------

TITLE: Installing Tailwind CSS v4.1 with CLI via npm
DESCRIPTION: This shell command installs the latest versions of `tailwindcss` and `@tailwindcss/cli` using npm. This setup is suitable for projects where Tailwind CSS is managed directly via its command-line interface, allowing for standalone compilation and other CLI-specific features.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/blog/tailwindcss-v4-1/index.mdx#_snippet_30

LANGUAGE: Shell
CODE:
```
npm install tailwindcss@latest @tailwindcss/cli@latest
```

----------------------------------------

TITLE: Generated CSS for Dynamic Spacing Utilities - CSS
DESCRIPTION: This generated CSS snippet shows how Tailwind CSS v4.0 derives dynamic spacing utilities. It defines a `--spacing` CSS variable and then uses `calc()` to generate utility classes like `mt-8`, `w-17`, and `pr-29` based on multiples of this variable, enabling arbitrary spacing values out of the box.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/blog/tailwindcss-v4/index.mdx#_snippet_15

LANGUAGE: CSS
CODE:
```
@layer theme {
  :root {
    --spacing: 0.25rem;
  }
}

@layer utilities {
  .mt-8 {
    margin-top: calc(var(--spacing) * 8);
  }
  .w-17 {
    width: calc(var(--spacing) * 17);
  }
  .pr-29 {
    padding-right: calc(var(--spacing) * 29);
  }
}
```

----------------------------------------

TITLE: Migrating Space-Between Layouts to Flex/Grid in HTML
DESCRIPTION: This HTML snippet demonstrates migrating from `space-y-*` utilities to a flexbox layout with `gap` in Tailwind CSS v4. It shows how to replace the `space-y-4` class with `flex flex-col gap-4` for more robust and performant spacing, especially when `space-y-*` causes issues.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/upgrade-guide.mdx#_snippet_21

LANGUAGE: HTML
CODE:
```
<div class="space-y-4 p-4"> <!-- [!code --] -->
<div class="flex flex-col gap-4 p-4"> <!-- [!code ++] -->
  <label for="name">Name</label>
  <input type="text" name="name" />
</div>
```

----------------------------------------

TITLE: Accessing Theme Variables in CSS - Generated CSS
DESCRIPTION: This snippet shows the generated CSS output from the `@theme` configuration, where all defined design tokens are automatically converted into CSS custom properties (variables) under the `:root` selector. This enables runtime access and manipulation of theme values directly in CSS or JavaScript.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/blog/tailwindcss-v4/index.mdx#_snippet_12

LANGUAGE: CSS
CODE:
```
:root {
  --font-display: "Satoshi", "sans-serif";

  --breakpoint-3xl: 1920px;

  --color-avocado-100: oklch(0.99 0 0);
  --color-avocado-200: oklch(0.98 0.04 113.22);
  --color-avocado-300: oklch(0.94 0.11 115.03);
  --color-avocado-400: oklch(0.92 0.19 114.08);
  --color-avocado-500: oklch(0.84 0.18 117.33);
  --color-avocado-600: oklch(0.53 0.12 118.34);

  --ease-fluid: cubic-bezier(0.3, 0, 0, 1);
  --ease-snappy: cubic-bezier(0.2, 0, 0, 1);

  /* ... */
}
```

----------------------------------------

TITLE: Adjusting Variant Stacking Order in Tailwind CSS v4 HTML
DESCRIPTION: Tailwind CSS v4 changes variant stacking order from right-to-left to left-to-right. This HTML snippet shows how to update order-sensitive stacked variants, such as direct child (`*`) and typography plugin variants, by reversing their order to match the new left-to-right application.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/upgrade-guide.mdx#_snippet_38

LANGUAGE: HTML
CODE:
```
<ul class="py-4 first:*:pt-0 last:*:pb-0">
<ul class="py-4 *:first:pt-0 *:last:pb-0">
  <li>One</li>
  <li>Two</li>
  <li>Three</li>
</ul>
```

----------------------------------------

TITLE: Using Arbitrary Grid Columns - HTML - Tailwind CSS
DESCRIPTION: Shows how to define a custom grid column layout using arbitrary values. The `grid-cols-[24rem_2.5rem_minmax(0,1fr)]` class sets a specific, non-standard grid template.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/styling-with-utility-classes.mdx#_snippet_17

LANGUAGE: HTML
CODE:
```
<div class="grid grid-cols-[24rem_2.5rem_minmax(0,1fr)]">
  <!-- ... -->
</div>
```

----------------------------------------

TITLE: Incorrect Mobile Styling with sm:text-center - HTML
DESCRIPTION: This HTML snippet demonstrates a common mistake in Tailwind CSS responsive design. Using `sm:text-center` will only apply text centering on screens 640px and wider, failing to center text on smaller, mobile screens. It highlights that `sm:` targets the small breakpoint and above, not just small screens.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/responsive-design.mdx#_snippet_5

LANGUAGE: HTML
CODE:
```
<!-- This will only center text on screens 640px and wider, not on small screens -->
<div class="sm:text-center"></div>
```

----------------------------------------

TITLE: Initializing Tailwind CSS Configuration in TypeScript
DESCRIPTION: This TypeScript code initializes the Tailwind CSS configuration with type checking. It imports the `Config` type from 'tailwindcss' and uses it to define the configuration object. The `satisfies Config` assertion ensures that the configuration object conforms to the expected type.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/blog/tailwindcss-v3-3/index.mdx#_snippet_2

LANGUAGE: TypeScript
CODE:
```
import type { Config } from 'tailwindcss'

export default {
  content: [],
  theme: {
    extend: {},
  },
  plugins: [],
} satisfies Config
```

----------------------------------------

TITLE: Applying Dark Mode Styles in HTML
DESCRIPTION: This HTML snippet illustrates how to apply dark mode specific styles using the `dark:` prefix in Tailwind CSS. Classes like `dark:bg-black` and `dark:text-white` ensure that elements adapt their appearance when the user's system dark mode is active, providing a seamless user experience.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/blog/tailwindcss-v2/index.mdx#_snippet_4

LANGUAGE: html
CODE:
```
<div class="bg-white dark:bg-black">
  <h1 class="text-gray-900 dark:text-white">Dark mode</h1>
  <p class="text-gray-500 dark:text-gray-300">The feature you've all been waiting for.</p>
</div>
```

----------------------------------------

TITLE: Filtering Combobox Results in React
DESCRIPTION: This snippet demonstrates how to implement a basic string comparison filter for the Headless UI `Combobox` component in React. It manages selected person and query states using `useState`, dynamically filtering the `people` array based on the query input. This allows users to quickly narrow down options in a large dataset.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/blog/headless-ui-v1-5/index.mdx#_snippet_0

LANGUAGE: jsx
CODE:
```
import { useState } from 'react'
import { Combobox } from '@headlessui/react'

const people = [
  'Wade Cooper',
  'Arlene McCoy',
  'Devon Webb',
  'Tom Cook',
  'Tanya Fox',
  'Hellen Schmidt',
]

function MyCombobox() {
  const [selectedPerson, setSelectedPerson] = useState(people[0])
  const [query, setQuery] = useState('')

  const filteredPeople =
    query === ''
      ? people
      : people.filter((person) => {
          return person.toLowerCase().includes(query.toLowerCase())
        })

  return (
    <Combobox value={selectedPerson} onChange={setSelectedPerson}>
      <Combobox.Input onChange={(event) => setQuery(event.target.value)} />
      <Combobox.Options>
        {filteredPeople.map((person) => (
          <Combobox.Option key={person} value={person}>
            {person}
          </Combobox.Option>
        ))}
      </Combobox.Options>
    </Combobox>
  )
}
```

----------------------------------------

TITLE: Combining Dark Mode with Hover States in HTML
DESCRIPTION: This HTML snippet demonstrates how to combine dark mode utilities with hover states in Tailwind CSS. The `dark:hover:bg-gray-50` class ensures that the button's background changes on hover specifically when dark mode is enabled, providing interactive styling for both light and dark themes.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/blog/tailwindcss-v2/index.mdx#_snippet_5

LANGUAGE: html
CODE:
```
<button class="bg-gray-900 hover:bg-gray-800 dark:bg-white dark:hover:bg-gray-50">
  <!-- ... -->
</button>
```

----------------------------------------

TITLE: Imperatively Closing Popover with Scoped Slot in Vue
DESCRIPTION: This snippet demonstrates how to imperatively close a Headless UI `Popover` component in Vue using the `close` function passed via a scoped slot. This enables custom control over when the popover closes, such as after an asynchronous operation like fetching data. The `accept` method calls the `close` function after the `fetch` operation.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/blog/headless-ui-v1-4/index.mdx#_snippet_7

LANGUAGE: html
CODE:
```
<template>
  <Popover>
    <PopoverButton>Solutions</PopoverButton>

    <PopoverPanel v-slot="{ close }">
      <button @click="accept(close)">Read and accept</button>
    </PopoverPanel>
  </Popover>
</template>

<script>
  import { Popover, PopoverButton, PopoverPanel } from '@headlessui/vue'

  export default {
    components: { Popover, PopoverButton, PopoverPanel },
    setup() {
      return {
        accept: async (close) => {
          await fetch('/accept-terms', { method: 'POST' })
          close()
        },
      }
    },
  }
</script>
```

----------------------------------------

TITLE: Styling Sibling Elements with Peer Variants in HTML
DESCRIPTION: This HTML snippet demonstrates how to use Tailwind's `peer-*` variants to style a sibling element based on the state of a preceding input. When the checkbox is checked, the `span` element's background color changes. This feature requires Just-in-Time mode.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/blog/tailwindcss-2-2/index.mdx#_snippet_9

LANGUAGE: HTML
CODE:
```
<label>
  <input type="checkbox" class="peer sr-only" />
  <span class="h-4 w-4 bg-gray-200 peer-checked:bg-blue-500">
    <!-- ... -->
  </span>
</label>
```

----------------------------------------

TITLE: Applying Skewed Background Effect with Tailwind CSS `::before`
DESCRIPTION: This example illustrates using Tailwind CSS `before` variants to create a decorative, skewed background effect behind text. Classes like `before:absolute`, `before:-inset-1`, `before:block`, `before:-skew-y-3`, and `before:bg-pink-500` are applied to the `::before` pseudo-element to achieve this visual style, making the text appear highlighted.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/hover-focus-and-other-states.mdx#_snippet_39

LANGUAGE: HTML
CODE:
```
<blockquote class="text-center text-2xl font-semibold text-gray-900 italic dark:text-white">
  When you look
  <span class="relative inline-block before:absolute before:-inset-1 before:block before:-skew-y-3 before:bg-pink-500">
    <span class="relative text-white dark:text-gray-950">annoyed</span>
  </span>
  all the time, people think that you're busy.
</blockquote>
```

----------------------------------------

TITLE: Composing Tailwind CSS Variants in HTML
DESCRIPTION: This snippet illustrates the enhanced composability of Tailwind CSS v4 variants. It shows how `group-*` can be combined with `has-*` and `focus` to create complex, dynamic selectors like `group-has-[&:focus]:opacity-100`, demonstrating the framework's shift towards more flexible and powerful variant combinations.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/blog/tailwindcss-v4-alpha/index.mdx#_snippet_2

LANGUAGE: HTML
CODE:
```
<div class="group">
  <div class="group-has-[&:focus]:opacity-100">
  <div class="group-has-focus:opacity-100">
      <!-- ... -->
    </div>
  </div>
</div>
```

----------------------------------------

TITLE: Defining Container Query Ranges with Tailwind CSS
DESCRIPTION: This snippet demonstrates how to combine `@min-*` and `@max-*` variants to create container query ranges. The `@min-md:@max-xl:hidden` utility hides the element only when the container's width is between the 'md' and 'xl' breakpoints, providing fine-grained control over responsiveness.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/blog/tailwindcss-v4/index.mdx#_snippet_18

LANGUAGE: HTML
CODE:
```
<div class="@container">
  <div class="flex @min-md:@max-xl:hidden">
    <!-- ... -->
  </div>
</div>
```

----------------------------------------

TITLE: Applying Custom Focus Styles with Box Shadow and Transparent Outline (HTML)
DESCRIPTION: This snippet demonstrates how to apply custom focus styles using `focus:shadow-outline` in conjunction with the updated `focus:outline-none` class. The `outline-none` class now renders a transparent outline by default, ensuring custom box-shadow-based focus styles are visible in Windows high contrast mode, improving accessibility.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/blog/tailwindcss-1-9/index.mdx#_snippet_3

LANGUAGE: HTML
CODE:
```
<button class="focus:shadow-outline focus:outline-none ...">
  <!-- ... -->
</button>
```

----------------------------------------

TITLE: Using `outline-white` and `outline-black` for General Purpose Focus Indicators (HTML)
DESCRIPTION: This example illustrates the use of `outline-white` and `outline-black` utilities to create unobtrusive focus indicators. These utilities render a 2px dotted outline with a 2px offset, providing clear visual feedback for keyboard users while maintaining sufficient contrast on both dark and light backgrounds.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/blog/tailwindcss-1-9/index.mdx#_snippet_4

LANGUAGE: HTML
CODE:
```
<!-- Use `outline-white` on dark backgrounds -->
<div class="bg-gray-900">
  <button class="focus:outline-white ...">
    <!-- ... -->
  </button>
</div>

<!-- Use `outline-black` on light backgrounds -->
<div class="bg-white">
  <button class="focus:outline-black ...">
    <!-- ... -->
  </button>
</div>
```

----------------------------------------

TITLE: Using Arbitrary Opacity Values in HTML
DESCRIPTION: This HTML snippet demonstrates how to use arbitrary opacity values with the new shorthand syntax by enclosing the desired opacity in square brackets. This allows for precise control over the alpha channel, even for values not defined in the default opacity scale.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/blog/tailwindcss-2-2/index.mdx#_snippet_14

LANGUAGE: HTML
CODE:
```
<div class="bg-red-500/[0.31]"></div>
```

----------------------------------------

TITLE: Styling with aria-disabled TailwindCSS Variant (CSS)
DESCRIPTION: This variant targets elements where the `aria-disabled` attribute is set to `true`, allowing for specific styling of disabled accessible components in TailwindCSS.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/hover-focus-and-other-states.mdx#_snippet_214

LANGUAGE: CSS
CODE:
```
&[aria-disabled="true"]
```

----------------------------------------

TITLE: Filtering Combobox Results in Vue
DESCRIPTION: This snippet illustrates how to implement a basic string comparison filter for the Headless UI `Combobox` component in Vue. It uses `ref` for reactive state and a `computed` property for filtering the `people` array based on the query input. This provides a dynamic and efficient way to filter options within a Vue application's template.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/blog/headless-ui-v1-5/index.mdx#_snippet_1

LANGUAGE: html
CODE:
```
<script setup>
  import { ref, computed } from 'vue'
  import { Combobox, ComboboxInput, ComboboxOptions, ComboboxOption } from '@headlessui/vue'

  const people = [
    'Wade Cooper',
    'Arlene McCoy',
    'Devon Webb',
    'Tom Cook',
    'Tanya Fox',
    'Hellen Schmidt',
  ]
  const selectedPerson = ref(people[0])
  const query = ref('')

  const filteredPeople = computed(() =>
    query.value === ''
      ? people
      : people.filter((person) => {
          return person.toLowerCase().includes(query.value.toLowerCase())
        })
  )
</script>

<template>
  <Combobox v-model="selectedPerson">
    <ComboboxInput @change="query = $event.target.value" />
    <ComboboxOptions>
      <ComboboxOption v-for="person in filteredPeople" :key="person" :value="person">
        {{ person }}
      </ComboboxOption>
    </ComboboxOptions>
  </Combobox>
</template>

```

----------------------------------------

TITLE: Defining Custom Variants with Multiple Nested Rules in Tailwind CSS
DESCRIPTION: This snippet demonstrates creating a custom variant that incorporates multiple levels of nested rules. The `any-hover` variant combines a media query (`@media (any-hover: hover)`) with a pseudo-class (`&:hover`) to apply styles only when a hover interaction is possible and the element is being hovered.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/adding-custom-styles.mdx#_snippet_45

LANGUAGE: CSS
CODE:
```
@custom-variant any-hover {
  @media (any-hover: hover) {
    &:hover {
      @slot;
    }
  }
}
```

----------------------------------------

TITLE: Referencing Theme Variable in Inline HTML Styles
DESCRIPTION: This HTML example shows how a theme variable, `--color-mint-500`, can be directly referenced as a standard CSS variable using `var()` within inline styles. This provides flexibility for applying design tokens in scenarios where a utility class might not be suitable, such as arbitrary values or dynamic styles.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/theme.mdx#_snippet_2

LANGUAGE: html
CODE:
```
<!-- [!code filename:HTML] -->
<!-- [!code word:var(--color-mint-500)] -->
<div style="background-color: var(--color-mint-500)">
  <!-- ... -->
</div>
```

----------------------------------------

TITLE: Styling Checkboxes with Tailwind CSS Utilities (HTML)
DESCRIPTION: This HTML snippet shows how to style a checkbox using Tailwind CSS utility classes after integrating the @tailwindcss/forms plugin. It applies classes for size, shape, border, text color, and focus states, resulting in a rounded checkbox with an indigo focus ring and checked state. Requires the @tailwindcss/forms plugin to normalize base styles.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/blog/tailwindcss-v2/index.mdx#_snippet_12

LANGUAGE: HTML
CODE:
```
<input
  type="checkbox"
  class="focus:ring-opacity-50 h-4 w-4 rounded border-gray-300 text-indigo-500 focus:border-indigo-300 focus:ring-2 focus:ring-indigo-200"
/>
```

----------------------------------------

TITLE: Applying Logical Margins with Tailwind CSS
DESCRIPTION: This snippet demonstrates the use of Tailwind CSS logical margin utilities, `ms-<number>` (margin-inline-start) and `me-<number>` (margin-inline-end). It shows how these utilities adjust spacing based on text direction (`ltr` for left-to-right and `rtl` for right-to-left), providing consistent layout regardless of writing mode.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/margin.mdx#_snippet_6

LANGUAGE: html
CODE:
```
<!-- [!code classes:ms-8,me-8] -->
<!-- [!code word:dir="ltr"] -->
<!-- [!code word:dir="rtl"] -->
<div>
  <div dir="ltr">
    <div class="ms-8 ...">ms-8</div>
    <div class="me-8 ...">me-8</div>
  </div>
  <div dir="rtl">
    <div class="ms-8 ...">ms-8</div>
    <div class="me-8 ...">me-8</div>
  </div>
</div>
```

----------------------------------------

TITLE: Applying Logical Border Radius with Tailwind CSS
DESCRIPTION: This snippet demonstrates how to apply border radius using Tailwind CSS logical properties like `rounded-s-lg`. These properties adapt to text direction, applying to the start (left in LTR, right in RTL) of an element. It shows examples for both left-to-right (LTR) and right-to-left (RTL) contexts using the `dir` attribute.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/border-radius.mdx#_snippet_4

LANGUAGE: React
CODE:
```
<div className="grid grid-cols-2 place-items-center gap-x-4">
  <div className="flex flex-col items-start gap-y-4" dir="ltr">
    <p className="text-sm font-medium">Left-to-right</p>
    <div className="size-16 rounded-s-lg bg-blue-500 p-4"></div>
  </div>
  <div className="flex flex-col items-start gap-y-4" dir="rtl">
    <p className="text-sm font-medium">Right-to-left</p>
    <div className="size-16 rounded-s-lg bg-blue-500 p-4"></div>
  </div>
</div>
```

LANGUAGE: HTML
CODE:
```
<!-- [!code classes:rounded-s-lg] -->
<!-- [!code word:dir="ltr"] -->
<!-- [!code word:dir="rtl"] -->
<div dir="ltr">
  <div class="rounded-s-lg ..."></div>
</div>

<div dir="rtl">
  <div class="rounded-s-lg ..."></div>
</div>
```

----------------------------------------

TITLE: Styling Disabled Elements in CSS
DESCRIPTION: Applies styles to user interface elements that are in a disabled state. This pseudo-class is commonly used for form controls like buttons or input fields that are currently inactive and cannot be interacted with.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/hover-focus-and-other-states.mdx#_snippet_127

LANGUAGE: css
CODE:
```
&:disabled
```

----------------------------------------

TITLE: Applying Transition Utilities to a Button in HTML
DESCRIPTION: This HTML snippet demonstrates how to apply Tailwind CSS transition utilities to a button. It uses `transition` for default properties, `delay-150`, `duration-300`, and `ease-in-out` for timing, and hover effects for transform and background color changes.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/transition-property.mdx#_snippet_9

LANGUAGE: HTML
CODE:
```
<button class="bg-blue-500 transition delay-150 duration-300 ease-in-out hover:-translate-y-1 hover:scale-110 hover:bg-indigo-500 ...">
  Save Changes
</button>
```

----------------------------------------

TITLE: Styling Checked Elements in CSS
DESCRIPTION: Applies styles to radio buttons, checkboxes, or option elements that are in a checked or toggled state. This pseudo-class is essential for visually indicating selected states in forms.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/hover-focus-and-other-states.mdx#_snippet_129

LANGUAGE: css
CODE:
```
&:checked
```

----------------------------------------

TITLE: Styling Radio Button Labels with Tailwind CSS and Pointer Variants (JSX)
DESCRIPTION: This JSX snippet demonstrates styling a radio button label using Tailwind CSS, including responsive adjustments for coarse pointer devices (e.g., touchscreens) via the `pointer-coarse:p-4` utility. It sets up a flexible layout, applies various background, text, ring, and focus styles, and visually hides the native radio input.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/blog/tailwindcss-v4-1/index.mdx#_snippet_16

LANGUAGE: JSX
CODE:
```
<label className="flex items-center justify-center rounded-md bg-white p-2 text-sm font-semibold text-gray-900 uppercase ring-1 ring-gray-300 not-data-focus:not-has-checked:ring-inset hover:bg-gray-50 has-checked:bg-indigo-600 has-checked:text-white has-checked:ring-0 has-checked:hover:bg-indigo-500 has-focus-visible:outline-2 has-focus-visible:outline-offset-2 has-focus-visible:outline-indigo-600 data-focus:ring-2 data-focus:ring-indigo-600 data-focus:ring-offset-2 data-focus:has-checked:ring-2 sm:flex-1 dark:bg-transparent dark:text-white dark:ring-white/20 dark:hover:bg-gray-950/50 pointer-coarse:p-4">
          <input type="radio" name="memory-option" value="128 GB" className="sr-only" />
          <span>128 GB</span>
        </label>
```

----------------------------------------

TITLE: Installing Tailwind CSS with Vite
DESCRIPTION: This command installs the latest versions of Tailwind CSS and its dedicated Vite plugin via npm. This integration is optimized for projects built with Vite, providing a seamless development experience.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/blog/tailwindcss-v4-1/index.mdx#_snippet_1

LANGUAGE: sh
CODE:
```
npm install tailwindcss@latest @tailwindcss/vite@latest
```

----------------------------------------

TITLE: Configuring Tailwind CSS Purge Safelist in JavaScript
DESCRIPTION: Illustrates how to use the `safelist` option in `tailwind.config.js` to prevent specific utility classes from being removed during the purging process. This is crucial for classes used dynamically or from external sources, ensuring they are always included in the production CSS. Note that JIT mode requires complete class names, not regular expressions.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/blog/tailwindcss-2-2/index.mdx#_snippet_27

LANGUAGE: javascript
CODE:
```
module.exports = {
  purge: {
    content: ["./src/**/*.html"],
    safelist: [
      "bg-blue-500",
      "text-center",
      "hover:opacity-100",
      // ...
      "lg:text-right"
    ]
  },
  // ...
};
```

----------------------------------------

TITLE: Managing Motion with `motion-reduce` and `motion-safe` in HTML
DESCRIPTION: This example contrasts `motion-reduce` and `motion-safe` variants for handling transitions and hover effects. `motion-reduce` is used to explicitly 'undo' motion styles (e.g., `transition-none`), while `motion-safe` applies motion styles only when it's safe to do so, often resulting in cleaner and more concise code for adding animations.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/hover-focus-and-other-states.mdx#_snippet_53

LANGUAGE: html
CODE:
```
<!-- [!code classes:motion-reduce:hover:translate-y-0] -->
<!-- [!code classes:motion-reduce:transition-none] -->
<!-- [!code classes:motion-safe:hover:-translate-x-0.5] -->
<!-- [!code classes:motion-safe:transition] -->
<!-- Using `motion-reduce` can mean lots of "undoing" styles -->
<button class="transition hover:-translate-y-0.5 motion-reduce:transition-none motion-reduce:hover:translate-y-0 ...">
  Save changes
</button>

<!-- Using `motion-safe` is less code in these situations -->
<button class="motion-safe:transition motion-safe:hover:-translate-x-0.5 ...">Save changes</button>
```

----------------------------------------

TITLE: Configuring PostCSS Plugin for Tailwind CSS v4 Alpha - JavaScript
DESCRIPTION: This JavaScript configuration for postcss.config.js adds the @tailwindcss/postcss plugin to your PostCSS setup, enabling it to process Tailwind CSS directives in your stylesheets.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/blog/tailwindcss-v4-alpha/index.mdx#_snippet_15

LANGUAGE: js
CODE:
```
// [!code filename:postcss.config.js]
module.exports = {
  plugins: {
    "@tailwindcss/postcss": {},
  },
};
```

----------------------------------------

TITLE: Styling Last Child Elements with Tailwind CSS (Svelte)
DESCRIPTION: This snippet demonstrates how to style an element if it is the last child of its parent, using the `last` variant. The `last:pb-0` class removes bottom padding from the last list item within the `<ul>`.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/hover-focus-and-other-states.mdx#_snippet_235

LANGUAGE: Svelte
CODE:
```
<ul>
  {#each people as person}
    <li class="py-4 last:pb-0 ...">
      <!-- ... -->
    </li>
  {/each}
</ul>
```

----------------------------------------

TITLE: Applying Arbitrary CSS Properties in HTML
DESCRIPTION: This HTML snippet demonstrates how to apply completely arbitrary CSS properties using Tailwind CSS's square bracket notation. It allows developers to use any CSS property not included out-of-the-box, such as `mask-type:luminance`, directly within their HTML.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/adding-custom-styles.mdx#_snippet_5

LANGUAGE: HTML
CODE:
```
<div class="[mask-type:luminance]">
  <!-- ... -->
</div>
```

----------------------------------------

TITLE: Hiding Spinner with `motion-reduce` in React/JSX
DESCRIPTION: This snippet demonstrates how to use the `motion-reduce:hidden` Tailwind CSS variant on an SVG spinner. When the user's system preference is set to 'prefers-reduced-motion: reduce', the spinner animation is automatically hidden, providing a more accessible experience by minimizing non-essential motion.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/hover-focus-and-other-states.mdx#_snippet_51

LANGUAGE: jsx
CODE:
```
<div className="flex items-center justify-center">
      <button
        type="button"
        className="inline-flex items-center rounded-md bg-indigo-500 px-4 py-2 text-sm leading-6 font-semibold text-white shadow"
        disabled
      >
        <svg
          className="mr-3 -ml-1 h-5 w-5 animate-spin text-white motion-reduce:hidden"
          fill="none"
          viewBox="0 0 24 24"
        >
          <circle className="opacity-25" cx="12" cy="12" r="10" stroke="currentColor" strokeWidth="4"></circle>
          <path
            className="opacity-75"
            fill="currentColor"
            d="M4 12a8 8 0 018-8V0C5.373 0 0 5.373 0 12h4zm2 5.291A7.962 7.962 0 014 12H0c0 3.042 1.135 5.824 3 7.938l3-2.647z"
          ></path>
        </svg>
        Processing...
      </button>
    </div>
```

----------------------------------------

TITLE: Defining Minimum Width Container Query for 3XL Breakpoint in CSS
DESCRIPTION: This CSS snippet defines a container query that applies styles when the container's width is greater than or equal to 48rem (768px). It corresponds to the `@3xl` container breakpoint in Tailwind CSS, enabling responsive design based on parent container dimensions.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/hover-focus-and-other-states.mdx#_snippet_174

LANGUAGE: CSS
CODE:
```
@container (width >= 48rem)
```

----------------------------------------

TITLE: Applying Uniform Gap with Tailwind CSS
DESCRIPTION: This snippet demonstrates how to use the `gap-<number>` utility, specifically `gap-4`, to apply a uniform gutter between both rows and columns in a grid layout. It sets a 16px gap (assuming default Tailwind spacing) between all grid items, ensuring consistent spacing.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/gap.mdx#_snippet_0

LANGUAGE: html
CODE:
```
<!-- [!code classes:gap-4] -->
<div class="grid grid-cols-2 gap-4">
  <div>01</div>
  <div>02</div>
  <div>03</div>
  <div>04</div>
</div>
```

----------------------------------------

TITLE: Styling Form Elements for Pointer Devices with Tailwind CSS
DESCRIPTION: This snippet demonstrates how to use Tailwind CSS's `pointer-coarse` variant to adjust the layout and padding of a form's radio button group when the user is interacting with a low-precision input device like a touchscreen. It shows a memory option selector where the grid columns and padding change for coarse pointers.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/blog/tailwindcss-v4-1/index.mdx#_snippet_15

LANGUAGE: HTML
CODE:
```
<fieldset aria-label="Choose a memory option" className="mx-auto max-w-md">
  <div className="flex items-center justify-between">
    <div className="text-sm/6 font-medium text-gray-900 dark:text-white">RAM</div>
    <a href="#" className="text-sm/6 font-medium text-indigo-600 hover:text-indigo-500 dark:text-indigo-400">
      See performance specs
    </a>
  </div>
  <div className="mt-4 grid grid-cols-6 gap-2 max-sm:grid-cols-3 pointer-coarse:mt-6 pointer-coarse:grid-cols-3 pointer-coarse:gap-4">
    <label className="flex items-center justify-center rounded-md bg-white p-2 text-sm font-semibold text-gray-900 uppercase ring-1 ring-gray-300 not-data-focus:not-has-checked:ring-inset hover:bg-gray-50 has-checked:bg-indigo-600 has-checked:text-white has-checked:ring-0 has-checked:hover:bg-indigo-500 has-focus-visible:outline-2 has-focus-visible:outline-offset-2 has-focus-visible:outline-indigo-600 data-focus:ring-2 data-focus:ring-indigo-600 data-focus:ring-offset-2 data-focus:has-checked:ring-2 sm:flex-1 dark:bg-transparent dark:text-white dark:ring-white/20 dark:hover:bg-gray-950/50 pointer-coarse:p-4">
      <input type="radio" name="memory-option" value="4 GB" className="sr-only" />
      <span>4 GB</span>
    </label>
    <label className="flex items-center justify-center rounded-md bg-white p-2 text-sm font-semibold text-gray-900 uppercase ring-1 ring-gray-300 not-data-focus:not-has-checked:ring-inset hover:bg-gray-50 has-checked:bg-indigo-600 has-checked:text-white has-checked:ring-0 has-checked:hover:bg-indigo-500 has-focus-visible:outline-2 has-focus-visible:outline-offset-2 has-focus-visible:outline-indigo-600 data-focus:ring-2 data-focus:ring-indigo-600 data-focus:ring-offset-2 data-focus:has-checked:ring-2 sm:flex-1 dark:bg-transparent dark:text-white dark:ring-white/20 dark:hover:bg-gray-950/50 pointer-coarse:p-4">
      <input type="radio" name="memory-option" value="8 GB" className="sr-only" defaultChecked />
      <span>8 GB</span>
    </label>
    <label className="flex items-center justify-center rounded-md bg-white p-2 text-sm font-semibold text-gray-900 uppercase ring-1 ring-gray-300 not-data-focus:not-has-checked:ring-inset hover:bg-gray-50 has-checked:bg-indigo-600 has-checked:text-white has-checked:ring-0 has-checked:hover:bg-indigo-500 has-focus-visible:outline-2 has-focus-visible:outline-offset-2 has-focus-visible:outline-indigo-600 data-focus:ring-2 data-focus:ring-indigo-600 data-focus:ring-offset-2 data-focus:has-checked:ring-2 sm:flex-1 dark:bg-transparent dark:text-white dark:ring-white/20 dark:hover:bg-gray-950/50 pointer-coarse:p-4">
      <input type="radio" name="memory-option" value="16 GB" className="sr-only" />
      <span>16 GB</span>
    </label>
    <label className="flex items-center justify-center rounded-md bg-white p-2 text-sm font-semibold text-gray-900 uppercase ring-1 ring-gray-300 not-data-focus:not-has-checked:ring-inset hover:bg-gray-50 has-checked:bg-indigo-600 has-checked:text-white has-checked:ring-0 has-checked:hover:bg-indigo-500 has-focus-visible:outline-2 has-focus-visible:outline-offset-2 has-focus-visible:outline-indigo-600 data-focus:ring-2 data-focus:ring-indigo-600 data-focus:ring-offset-2 data-focus:has-checked:ring-2 sm:flex-1 dark:bg-transparent dark:text-white dark:ring-white/20 dark:hover:bg-gray-950/50 pointer-coarse:p-4">
      <input type="radio" name="memory-option" value="32 GB" className="sr-only" />
      <span>32 GB</span>
    </label>
    <label className="flex items-center justify-center rounded-md bg-white p-2 text-sm font-semibold text-gray-900 uppercase ring-1 ring-gray-300 not-data-focus:not-has-checked:ring-inset hover:bg-gray-50 has-checked:bg-indigo-600 has-checked:text-white has-checked:ring-0 has-checked:hover:bg-indigo-500 has-focus-visible:outline-2 has-focus-visible:outline-offset-2 has-focus-visible:outline-indigo-600 data-focus:ring-2 data-focus:ring-indigo-600 data-focus:ring-offset-2 data-focus:has-checked:ring-2 sm:flex-1 dark:bg-transparent dark:text-white dark:ring-white/20 dark:hover:bg-gray-950/50 pointer-coarse:p-4">
      <input type="radio" name="memory-option" value="64 GB" className="sr-only" />
      <span>64 GB</span>
    </label>
  </div>
</fieldset>
```

----------------------------------------

TITLE: Styling Elements by Type and Position with :nth-of-type() in Svelte
DESCRIPTION: This snippet demonstrates how to apply Tailwind CSS classes to elements based on their position among siblings of the same type using the `:nth-of-type` variant. It allows for precise styling of specific elements or patterns within a list, such as applying `mx-6` to the 3rd element and `mx-7` to elements matching `3n+1`.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/hover-focus-and-other-states.mdx#_snippet_244

LANGUAGE: Svelte
CODE:
```
<nav>
  <img src="/logo.svg" alt="Vandelay Industries" />
  {#each links as link}
    <a href="#" class="mx-2 nth-of-type-3:mx-6 nth-of-type-[3n+1]:mx-7 ...">
      <!-- ... -->
    </a>
  {/each}
  <button>More</button>
</nav>
```

----------------------------------------

TITLE: Migrating Custom Utilities to Tailwind CSS v4's @utility API
DESCRIPTION: This snippet illustrates the change in defining custom utilities from Tailwind CSS v3 to v4. It shows the deprecated `@layer utilities` syntax and its replacement, the new `@utility` API, which aligns with native CSS cascade layers for defining custom utility classes.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/upgrade-guide.mdx#_snippet_36

LANGUAGE: CSS
CODE:
```
@layer utilities {
  .tab-4 {
    tab-size: 4;
  }
}
@utility tab-4 {
  tab-size: 4;
}
```

----------------------------------------

TITLE: Creating Message: JavaScript API Client
DESCRIPTION: This JavaScript snippet illustrates publishing a new message using an ApiClient. It initializes the client with a token and then calls the messages.create method, passing conversation_id and message as parameters.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/blog/2022-12-15-protocol-api-documentation-template/index.mdx#_snippet_1

LANGUAGE: js
CODE:
```
import ApiClient from '@example/protocol-api'
const client = new ApiClient(token)
await client.messages.create({
  conversation_id: 'xgQQXg3hrtjh7AvZ',
  message: 'You\'re what the French call \'les incompetents.\'',
})
```

----------------------------------------

TITLE: Overriding Default Tailwind Colors in CSS
DESCRIPTION: This snippet illustrates how to override any of Tailwind's default colors by defining new theme variables with the same name within the `@theme` directive. This allows you to customize the default color palette to match your specific design requirements.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/colors.mdx#_snippet_13

LANGUAGE: css
CODE:
```
@import "tailwindcss";

@theme {
  --color-gray-50: oklch(0.984 0.003 247.858);
  --color-gray-100: oklch(0.968 0.007 247.896);
  --color-gray-200: oklch(0.929 0.013 255.508);
  --color-gray-300: oklch(0.869 0.022 252.894);
  --color-gray-400: oklch(0.704 0.04 256.788);
  --color-gray-500: oklch(0.554 0.046 257.417);
  --color-gray-600: oklch(0.446 0.043 257.281);
  --color-gray-700: oklch(0.372 0.044 257.287);
  --color-gray-800: oklch(0.279 0.041 260.031);
  --color-gray-900: oklch(0.208 0.042 265.755);
  --color-gray-950: oklch(0.129 0.042 264.695);
}
```

----------------------------------------

TITLE: Styling Popover Element with Open Variant in HTML
DESCRIPTION: This HTML snippet demonstrates how the `open` variant in Tailwind CSS can be used to style a popover element. It specifically shows how to transition the popover's opacity from 0 to 100 when it is in the open state, leveraging the `:popover-open` pseudo-class.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/hover-focus-and-other-states.mdx#_snippet_86

LANGUAGE: HTML
CODE:
```
<!-- [!code classes:open:opacity-100] -->
<div>
  <button popovertarget="my-popover">Open Popover</button>
  <div popover id="my-popover" class="opacity-0 open:opacity-100 ...">
    <!-- ... -->
  </div>
</div>
```

----------------------------------------

TITLE: Hiding Spinner with `motion-reduce` in HTML
DESCRIPTION: This HTML snippet demonstrates the direct application of the `motion-reduce:hidden` class to an SVG element. This class ensures that the `animate-spin` utility is disabled, effectively hiding the spinner, when the user's operating system has 'prefers-reduced-motion' enabled, enhancing accessibility.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/hover-focus-and-other-states.mdx#_snippet_52

LANGUAGE: html
CODE:
```
<!-- [!code classes:motion-reduce:hidden] -->
<button type="button" class="bg-indigo-500 ..." disabled>
  <svg class="animate-spin motion-reduce:hidden ..." viewBox="0 0 24 24"><!-- ... --></svg>
  Processing...
</button>
```

----------------------------------------

TITLE: Applying Basic Border Colors in HTML with Tailwind CSS
DESCRIPTION: This HTML snippet illustrates the application of basic border colors using Tailwind CSS classes like `border-indigo-500`, `border-purple-500`, and `border-sky-500`. It shows how to set a border width of `border-4` and then specify the color for different `div` elements.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/border-color.mdx#_snippet_1

LANGUAGE: HTML
CODE:
```
<!-- [!code classes:border-indigo-500,border-purple-500,border-sky-500] -->
<div class="border-4 border-indigo-500 ..."></div>
<div class="border-4 border-purple-500 ..."></div>
<div class="border-4 border-sky-500 ..."></div>
```

----------------------------------------

TITLE: Styling Native Form Controls with Tailwind CSS
DESCRIPTION: This snippet demonstrates how to style native HTML form elements like file inputs and checkboxes using Tailwind CSS utility classes. It highlights the use of `file:` modifiers for file input buttons and the `accent-color` property for checkboxes, allowing for custom appearance without JavaScript.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/blog/tailwindcss-v3/index.mdx#_snippet_7

LANGUAGE: HTML
CODE:
```
<!-- [!code word:file\:mr-4] -->
<!-- [!code word:file\:py-2] -->
<!-- [!code word:file\:px-4] -->
<!-- [!code word:file\:rounded-full] -->
<!-- [!code word:file\:border-0] -->
<!-- [!code word:file\:text-sm] -->
<!-- [!code word:file\:font-semibold] -->
<!-- [!code word:file\:bg-violet-50] -->
<!-- [!code word:file\:text-violet-700] -->
<!-- [!code word:hover\:file\:bg-violet-100] -->
<form>
  <div class="flex items-center space-x-6">
    <div class="shrink-0">
      <img
        class="h-16 w-16 rounded-full object-cover"
        src="https://images.unsplash.com/photo-1580489944761-15a19d654956?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=1361&q=80"
        alt="Current profile photo"
      />
    </div>
    <label class="block">
      <span class="sr-only">Choose profile photo</span>
      <input
        type="file"
        class="block w-full text-sm text-slate-500 file:mr-4 file:rounded-full file:border-0 file:bg-violet-50 file:py-2 file:text-sm file:font-semibold file:text-violet-700 hover:file:bg-violet-100"
      />
    </label>
  </div>
  <label class="mt-6 flex items-center justify-center space-x-2 text-sm font-medium text-slate-600">
    <!-- [!code word:accent-violet-500] -->
    <input type="checkbox" class="accent-violet-500" checked />
    <span>Yes, send me all your stupid updates</span>
  </label>
</form>
```

----------------------------------------

TITLE: Adding Dividers Between Children with Tailwind CSS (JSX)
DESCRIPTION: This JSX example shows how to use Tailwind CSS `divide-x-4` utility to add a horizontal border between child elements. The `divide-x-4` class applies a 4-pixel border along the x-axis (horizontally) between direct children within a flex or grid container.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/border-width.mdx#_snippet_5

LANGUAGE: jsx
CODE:
```
<div className="mx-auto grid max-w-lg grid-cols-3 divide-x-4 divide-indigo-500 rounded-lg text-center font-mono text-sm leading-6 font-bold text-gray-400">
  <div className="p-4 outline-1 -outline-offset-1 outline-gray-900/20 outline-dashed dark:outline-white/20">01</div>
  <div className="p-4 outline-1 -outline-offset-1 outline-gray-900/20 outline-dashed dark:outline-white/20">02</div>
  <div className="p-4 outline-1 -outline-offset-1 outline-gray-900/20 outline-dashed dark:outline-white/20">03</div>
</div>
```

----------------------------------------

TITLE: Creating a Block-Level Grid Container with Tailwind CSS
DESCRIPTION: This snippet demonstrates how to use the `grid` utility class in Tailwind CSS to create a block-level grid container. It defines a 3x3 grid using `grid-cols-3` and `grid-rows-3`, with `gap-4` for spacing between grid items. This utility is essential for building complex, responsive layouts.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/display.mdx#_snippet_8

LANGUAGE: JSX
CODE:
```
<div className="relative grid grid-cols-3 grid-rows-3 gap-4 rounded-lg text-center font-mono text-sm leading-6 font-bold text-white">
      <div className="absolute inset-0">
        <Stripes border className="h-full rounded-lg" />
      </div>
      <div className="relative rounded-lg bg-fuchsia-500 p-4">01</div>
      <div className="relative rounded-lg bg-fuchsia-500 p-4">02</div>
      <div className="relative rounded-lg bg-fuchsia-500 p-4">03</div>
      <div className="relative rounded-lg bg-fuchsia-500 p-4">04</div>
      <div className="relative rounded-lg bg-fuchsia-500 p-4">05</div>
      <div className="relative rounded-lg bg-fuchsia-500 p-4">06</div>
      <div className="relative rounded-lg bg-fuchsia-500 p-4">07</div>
      <div className="relative rounded-lg bg-fuchsia-500 p-4">08</div>
      <div className="relative rounded-lg bg-fuchsia-500 p-4">09</div>
    </div>
```

LANGUAGE: HTML
CODE:
```
<div class="grid grid-cols-3 grid-rows-3 gap-4">
  <!-- ... -->
</div>
```

----------------------------------------

TITLE: Defining Tailwind CSS Color Palette Variables
DESCRIPTION: This snippet defines a wide range of color variables using the Oklch color space for various shades of slate, gray, zinc, neutral, and stone, along with standard black and white. These variables are used throughout Tailwind CSS for consistent theming.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/theme.mdx#_snippet_30

LANGUAGE: CSS
CODE:
```
--color-slate-700: oklch(0.372 0.044 257.287);
--color-slate-800: oklch(0.279 0.041 260.031);
--color-slate-900: oklch(0.208 0.042 265.755);
--color-slate-950: oklch(0.129 0.042 264.695);

--color-gray-50: oklch(0.985 0.002 247.839);
--color-gray-100: oklch(0.967 0.003 264.542);
--color-gray-200: oklch(0.928 0.006 264.531);
--color-gray-300: oklch(0.872 0.01 258.338);
--color-gray-400: oklch(0.707 0.022 261.325);
--color-gray-500: oklch(0.551 0.027 264.364);
--color-gray-600: oklch(0.446 0.03 256.802);
--color-gray-700: oklch(0.373 0.034 259.733);
--color-gray-800: oklch(0.278 0.033 256.848);
--color-gray-900: oklch(0.21 0.034 264.665);
--color-gray-950: oklch(0.13 0.028 261.692);

--color-zinc-50: oklch(0.985 0 0);
--color-zinc-100: oklch(0.967 0.001 286.375);
--color-zinc-200: oklch(0.92 0.004 286.32);
--color-zinc-300: oklch(0.871 0.006 286.286);
--color-zinc-400: oklch(0.705 0.015 286.067);
--color-zinc-500: oklch(0.552 0.016 285.938);
--color-zinc-600: oklch(0.442 0.017 285.786);
--color-zinc-700: oklch(0.37 0.013 285.805);
--color-zinc-800: oklch(0.274 0.006 286.033);
--color-zinc-900: oklch(0.21 0.006 285.885);
--color-zinc-950: oklch(0.141 0.005 285.823);

--color-neutral-50: oklch(0.985 0 0);
--color-neutral-100: oklch(0.97 0 0);
--color-neutral-200: oklch(0.922 0 0);
--color-neutral-300: oklch(0.87 0 0);
--color-neutral-400: oklch(0.708 0 0);
--color-neutral-500: oklch(0.556 0 0);
--color-neutral-600: oklch(0.439 0 0);
--color-neutral-700: oklch(0.371 0 0);
--color-neutral-800: oklch(0.269 0 0);
--color-neutral-900: oklch(0.205 0 0);
--color-neutral-950: oklch(0.145 0 0);

--color-stone-50: oklch(0.985 0.001 106.423);
--color-stone-100: oklch(0.97 0.001 106.424);
--color-stone-200: oklch(0.923 0.003 48.717);
--color-stone-300: oklch(0.869 0.005 56.366);
--color-stone-400: oklch(0.709 0.01 56.259);
--color-stone-500: oklch(0.553 0.013 58.071);
--color-stone-600: oklch(0.444 0.011 73.639);
--color-stone-700: oklch(0.374 0.01 67.558);
--color-stone-800: oklch(0.268 0.007 34.298);
--color-stone-900: oklch(0.216 0.006 56.043);
--color-stone-950: oklch(0.147 0.004 49.25);

--color-black: #000;
--color-white: #fff;
```

----------------------------------------

TITLE: Defining Custom Container Query for Dynamic Width in CSS
DESCRIPTION: This snippet defines a flexible container query allowing for custom width values. It applies styles when the container's width is less than a dynamically specified value, enabling highly customizable responsive design.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/hover-focus-and-other-states.mdx#_snippet_193

LANGUAGE: CSS
CODE:
```
@container (width < ...)
```

----------------------------------------

TITLE: Styling Based on Parent State (group-hover)
DESCRIPTION: Illustrates how to use the `group-hover` variant to style a child element when a specific parent element is hovered. The parent needs the `group` class.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/styling-with-utility-classes.mdx#_snippet_22

LANGUAGE: HTML
CODE:
```
<!-- [!code filename:HTML] -->
<!-- [!code classes:group,group-hover:underline] -->
<a href="#" class="group rounded-lg p-8">
  <!-- ... -->
  <span class="group-hover:underline">Read more…</span>
</a>
```

LANGUAGE: CSS
CODE:
```
/* [!code filename:Simplified CSS] */
@media (hover: hover) {
  a:hover span {
    text-decoration-line: underline;
  }
}
```

----------------------------------------

TITLE: Customizing Container Utility in Tailwind CSS v4 (CSS)
DESCRIPTION: This CSS snippet shows how to customize the `container` utility in Tailwind CSS v4. Unlike v3, which had direct configuration options, v4 requires extending the utility using the `@utility` directive to apply custom styles like `margin-inline: auto` and `padding-inline: 2rem`.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/upgrade-guide.mdx#_snippet_24

LANGUAGE: CSS
CODE:
```
@utility container {
  margin-inline: auto;
  padding-inline: 2rem;
}
```

----------------------------------------

TITLE: Stacking Tailwind Variants for Complex Conditions
DESCRIPTION: Demonstrates how multiple Tailwind variants can be stacked to target highly specific conditions, such as changing the background color on hover, at the medium breakpoint, and in dark mode (`dark:md:hover:bg-fuchsia-600`).
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/hover-focus-and-other-states.mdx#_snippet_3

LANGUAGE: HTML
CODE:
```
<!-- [!code classes:dark:md:hover:bg-fuchsia-600] -->
<button class="dark:md:hover:bg-fuchsia-600 ...">Save changes</button>
```

----------------------------------------

TITLE: Specifying Border Color in Tailwind CSS v4 (HTML)
DESCRIPTION: This HTML snippet demonstrates the change in default border color for `border-*` and `divide-*` utilities in Tailwind CSS v4. To update projects, a specific color like `border-gray-200` must now be explicitly added, as the default changed from `gray-200` to `currentColor`.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/upgrade-guide.mdx#_snippet_25

LANGUAGE: HTML
CODE:
```
<div class="border border-gray-200 px-2 py-3 ...">
  <!-- ... -->
</div>
```

----------------------------------------

TITLE: Customizing Typography Elements with HTML Modifiers in Tailwind CSS
DESCRIPTION: This HTML snippet showcases the new HTML-based customization API, allowing specific elements within prose content to be styled directly. Modifiers like `prose-img:rounded-xl`, `prose-headings:underline`, and `prose-a:text-blue-600` apply targeted styles without needing custom CSS.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/blog/tailwindcss-typography-v0-5/index.mdx#_snippet_3

LANGUAGE: html
CODE:
```
<article class="prose prose-img:rounded-xl prose-headings:underline prose-a:text-blue-600">
  {{ markdown }}
</article>
```

----------------------------------------

TITLE: Applying Styles within a Breakpoint Range (md to xl) - HTML
DESCRIPTION: This HTML snippet demonstrates how to apply a utility class (`flex`) only within a specific breakpoint range using stacked responsive variants. The `md:max-xl:flex` class ensures that `display: flex` is active exclusively from the `md` breakpoint up to, but not including, the `xl` breakpoint, providing fine-grained control over responsive layouts.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/responsive-design.mdx#_snippet_7

LANGUAGE: HTML
CODE:
```
<div class="md:max-xl:flex">
  <!-- ... -->
</div>
```

----------------------------------------

TITLE: Configuring Prettier to Use Tailwind CSS Plugin
DESCRIPTION: This JSON snippet shows how to add the `prettier-plugin-tailwindcss` to your Prettier configuration file (e.g., `.prettierrc`). Including the plugin in the `plugins` array enables Prettier to automatically sort Tailwind CSS classes when formatting files.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/blog/automatic-class-sorting-with-prettier/index.mdx#_snippet_2

LANGUAGE: json
CODE:
```
{
  "plugins": ["prettier-plugin-tailwindcss"]
}
```

----------------------------------------

TITLE: Styling File Input Buttons with Tailwind CSS
DESCRIPTION: This snippet demonstrates how to style the button part of a file input using Tailwind CSS's `file` variant. It applies various utility classes like `file:bg-violet-50` for background, `file:text-violet-700` for text color, and `file:rounded-full` for border-radius, along with hover and dark mode styles.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/hover-focus-and-other-states.mdx#_snippet_42

LANGUAGE: HTML
CODE:
```
<input
  type="file"
  class="file:mr-4 file:rounded-full file:border-0 file:bg-violet-50 file:px-4 file:py-2 file:text-sm file:font-semibold file:text-violet-700 hover:file:bg-violet-100 dark:file:bg-violet-600 dark:file:text-violet-100 dark:hover:file:bg-violet-500 ..."
/>
```

----------------------------------------

TITLE: Applying Dynamic Viewport Height with Tailwind CSS
DESCRIPTION: This HTML snippet demonstrates how to apply the `h-dvh` utility class from Tailwind CSS to a `div` element. This class sets the height of the element to 100% of the dynamic viewport height, which adjusts for browser UI changes on mobile devices, providing a more reliable full-height layout.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/blog/tailwindcss-v3-4/index.mdx#_snippet_1

LANGUAGE: HTML
CODE:
```
<!-- [!code word:h-dvh] -->
<div class="h-dvh">
  <!-- ... -->
</div>
```

----------------------------------------

TITLE: Constraining Images and Videos in CSS
DESCRIPTION: This CSS snippet defines default responsive behavior for `img` and `video` elements, ensuring they do not overflow their parent containers while maintaining their aspect ratio. It sets their maximum width to 100% and height to auto, making them responsive by default.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/preflight.mdx#_snippet_10

LANGUAGE: CSS
CODE:
```
img,
video {
  max-width: 100%;
  height: auto;
}
```

----------------------------------------

TITLE: Setting Max Width to 7XL Container Size in Tailwind CSS
DESCRIPTION: Sets the maximum width to the predefined `--container-7xl` variable, equivalent to 80rem (1280px). This provides the largest predefined fixed maximum width for elements.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/max-width.mdx#_snippet_14

LANGUAGE: CSS
CODE:
```
max-width: var(--container-7xl); /* 80rem (1280px) */
```

----------------------------------------

TITLE: Mapping border-e-* color to Tailwind CSS
DESCRIPTION: This snippet shows the mapping of the CSS logical property border-inline-end-color to its corresponding Tailwind CSS class.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/blog/tailwindcss-v3-3/index.mdx#_snippet_20

LANGUAGE: HTML
CODE:
```
<td className="pl-4 sm:pl-0">
            <code>{"border-e-*"}</code>
          </td>
          <td>
            <code>border-inline-end-color</code>
          </td>
          <td className="pr-4 sm:pr-0">
            <code>{"border-r-*"}</code>
          </td>
```

----------------------------------------

TITLE: Applying Background Image Position Utilities in Tailwind CSS (JSX)
DESCRIPTION: This JSX snippet demonstrates the use of various Tailwind CSS background position utilities (e.g., `bg-top-left`, `bg-top`, `bg-top-right`, `bg-left`, `bg-center`) to control the placement of a background image within multiple `div` elements. Each example showcases a different background position, along with responsive classes for layout and hover effects to reveal the full image.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/background-position.mdx#_snippet_3

LANGUAGE: JSX
CODE:
```
<div className="w-full">
  <div className="flex snap-x scroll-p-4 items-end overflow-x-scroll overflow-y-hidden p-8 pt-16 sm:grid sm:grid-cols-3 sm:gap-16">
    <div className="relative w-32 shrink-0 snap-start snap-always sm:w-auto">
      <p className="absolute inset-x-0 top-0 mb-3 -translate-y-8 text-center font-mono text-xs font-medium text-gray-500 dark:text-gray-400">
        bg-top-left
      </p>
      <div className="group relative mx-auto h-20 w-20 rounded-lg">
        <div className="relative z-10 h-full w-full rounded-md bg-[url(https://images.unsplash.com/photo-1554629947-334ff61d85dc?ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&ixlib=rb-1.2.1&auto=format&fit=crop&w=1000&h=1000&q=90)] bg-[size:8rem] bg-top-left outline -outline-offset-1 outline-black/10"></div>
        <img
          className="absolute top-0 left-0 h-32 w-32 max-w-none overflow-hidden rounded-md opacity-0 sm:group-hover:opacity-25"
          src="https://images.unsplash.com/photo-1554629947-334ff61d85dc?ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&ixlib=rb-1.2.1&auto=format&fit=crop&w=1000&h=1000&q=90"
        />
      </div>
    </div>
    <div className="relative w-32 shrink-0 snap-start snap-always sm:w-auto">
      <p className="absolute inset-x-0 top-0 mb-3 -translate-y-8 text-center font-mono text-xs font-medium text-gray-500 dark:text-gray-400">
        bg-top
      </p>
      <div className="group relative mx-auto h-20 w-20 rounded-lg">
        <div className="relative z-10 h-full w-full rounded-md bg-[url(https://images.unsplash.com/photo-1554629947-334ff61d85dc?ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&ixlib=rb-1.2.1&auto=format&fit=crop&w=1000&h=1000&q=90)] bg-[size:8rem] bg-top outline -outline-offset-1 outline-black/10"></div>
        <img
          className="absolute top-0 -left-6 h-32 w-32 max-w-none overflow-hidden rounded-md opacity-0 sm:group-hover:opacity-25"
          src="https://images.unsplash.com/photo-1554629947-334ff61d85dc?ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&ixlib=rb-1.2.1&auto=format&fit=crop&w=1000&h=1000&q=90"
        />
      </div>
    </div>
    <div className="relative w-32 shrink-0 snap-start snap-always sm:w-auto">
      <p className="absolute inset-x-0 top-0 mb-3 -translate-y-8 text-center font-mono text-xs font-medium text-gray-500 dark:text-gray-400">
        bg-top-right
      </p>
      <div className="group relative mx-auto h-20 w-20 rounded-lg">
        <div className="relative z-10 h-full w-full rounded-md bg-[url(https://images.unsplash.com/photo-1554629947-334ff61d85dc?ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&ixlib=rb-1.2.1&auto=format&fit=crop&w=1000&h=1000&q=90)] bg-[size:8rem] bg-top-right outline -outline-offset-1 outline-black/10"></div>
        <img
          className="absolute top-0 right-0 h-32 w-32 max-w-none overflow-hidden rounded-md opacity-0 sm:group-hover:opacity-25"
          src="https://images.unsplash.com/photo-1554629947-334ff61d85dc?ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&ixlib=rb-1.2.1&auto
```

----------------------------------------

TITLE: Using Configured Data Attribute Shortcut in HTML
DESCRIPTION: This HTML snippet shows how to use a custom data attribute shortcut defined in the Tailwind configuration. The `data-checked:underline` class will apply an underline style to the div because its `data-ui` attribute contains the 'checked' value, matching the configured shortcut.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/blog/tailwindcss-v3-2/index.mdx#_snippet_16

LANGUAGE: HTML
CODE:
```
<div data-ui="checked active" class="data-checked:underline">
  <!-- ... -->
</div>
```

----------------------------------------

TITLE: Using Inline Styles for Dynamic Values in JSX
DESCRIPTION: Explains using inline styles in JSX components to set CSS properties like background color and text color based on dynamic values passed as props.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/styling-with-utility-classes.mdx#_snippet_24

LANGUAGE: JSX
CODE:
```
// [!code filename:branded-button.jsx]
export function BrandedButton({ buttonColor, textColor, children }) {
  return (
    <button
      style={{
        // [!code highlight:3]
        backgroundColor: buttonColor,
        color: textColor,
      }}
      className="rounded-md px-3 py-1.5 font-medium"
    >
      {children}
    </button>
  );
}
```

----------------------------------------

TITLE: Styling Invalid Inputs with :invalid in HTML
DESCRIPTION: This snippet shows how to apply a Tailwind CSS class to an input element when its content is invalid according to its validation rules, using the `invalid` variant. This is essential for guiding users, typically by applying a `border-red-500` to highlight errors.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/hover-focus-and-other-states.mdx#_snippet_255

LANGUAGE: HTML
CODE:
```
<input required class="border invalid:border-red-500 ..." />
```

----------------------------------------

TITLE: Applying Arbitrary Breakpoint Values in Tailwind CSS HTML
DESCRIPTION: This HTML snippet demonstrates the use of one-off, arbitrary breakpoint values directly in Tailwind CSS utility classes. It uses `min-[320px]:text-center` and `max-[600px]:bg-sky-300` to apply styles based on specific pixel values, useful for unique, non-reusable breakpoints.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/responsive-design.mdx#_snippet_13

LANGUAGE: html
CODE:
```
<div class="max-[600px]:bg-sky-300 min-[320px]:text-center">
  <!-- ... -->
</div>
```

----------------------------------------

TITLE: Applying Relative Positioning with Tailwind CSS (HTML/JSX)
DESCRIPTION: This example illustrates the use of the `relative` utility in Tailwind CSS, which positions an element according to the normal document flow but allows offsets to be applied relative to its original position. Crucially, relatively positioned elements establish a new positioning context, acting as a reference for absolutely positioned child elements.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/position.mdx#_snippet_1

LANGUAGE: jsx
CODE:
```
<div className="relative text-sm leading-6 font-medium">\n  <div className="rounded-lg border border-sky-700/10 bg-sky-400/20 p-4 dark:border-0 dark:bg-blue-900/70">\n    <div className="relative h-32 border border-sky-700/10 bg-sky-400/20 p-4 dark:border-0 dark:bg-blue-400/20">\n      <p className="text-sky-700 dark:text-white">Relative parent</p>\n      <div className="absolute bottom-0 left-0 rounded-lg bg-sky-500 p-4 text-white shadow-lg dark:bg-blue-500">\n        <p>Absolute child</p>\n      </div>\n    </div>\n  </div>\n</div>
```

LANGUAGE: html
CODE:
```
<!-- [!code classes:relative] -->\n<div class="relative ...">\n  <p>Relative parent</p>\n  <div class="absolute bottom-0 left-0 ...">\n    <p>Absolute child</p>\n  </div>\n</div>
```

----------------------------------------

TITLE: Implementing Anchor Positioning with Headless UI Menu in React
DESCRIPTION: This React component demonstrates how to use the new `anchor` prop on `MenuItems` to control the positioning of a dropdown. It leverages the integrated Floating UI to ensure dropdowns remain visible and allows fine-tuning placement with CSS variables like `--anchor-gap` and `--anchor-padding`.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/blog/headless-ui-v2/index.mdx#_snippet_1

LANGUAGE: jsx
CODE:
```
import { Menu, MenuButton, MenuItem, MenuItems } from "@headlessui/react";

function Example() {
  return (
    <Menu>
      <MenuButton>Options</MenuButton>
      <MenuItems
        // [!code highlight:3]
        anchor="bottom start"
        className="[--anchor-gap:8px] [--anchor-padding:8px]"
      >
        <MenuItem>
          <button>Edit</button>
        </MenuItem>
        <MenuItem>
          <button>Duplicate</button>
        </MenuItem>
        <hr />
        <MenuItem>
          <button>Archive</button>
        </MenuItem>
        <MenuItem>
          <button>Delete</button>
        </MenuItem>
      </MenuItems>
    </Menu>
  );
}
```

----------------------------------------

TITLE: Customizing Tailwind CSS Theme with @theme Directive
DESCRIPTION: This CSS snippet demonstrates how to customize Tailwind CSS's default theme using the `@theme` directive. It allows defining custom design tokens such as font families, breakpoints, color palettes, and easing functions, providing a centralized way to manage design system variables.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/adding-custom-styles.mdx#_snippet_0

LANGUAGE: CSS
CODE:
```
@theme {
  --font-display: "Satoshi", "sans-serif";

  --breakpoint-3xl: 120rem;

  --color-avocado-100: oklch(0.99 0 0);
  --color-avocado-200: oklch(0.98 0.04 113.22);
  --color-avocado-300: oklch(0.94 0.11 115.03);
  --color-avocado-400: oklch(0.92 0.19 114.08);
  --color-avocado-500: oklch(0.84 0.18 117.33);
  --color-avocado-600: oklch(0.53 0.12 118.34);

  --ease-fluid: cubic-bezier(0.3, 0, 0, 1);
  --ease-snappy: cubic-bezier(0.2, 0, 0, 1);

  /* ... */
}
```

----------------------------------------

TITLE: Defining `@sm` Container Query in CSS
DESCRIPTION: This CSS container query applies styles when the container's width is 24rem (384px) or greater. It allows for more granular control over component layouts within different container sizes, improving modularity in design systems.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/hover-focus-and-other-states.mdx#_snippet_169

LANGUAGE: CSS
CODE:
```
@container (width >= 24rem)
```

----------------------------------------

TITLE: Applying Dynamic Data Attribute Variants - HTML
DESCRIPTION: This HTML snippet illustrates the ability to target custom boolean data attributes dynamically in Tailwind CSS v4.0. The `data-current:opacity-100` variant applies styles when the `data-current` attribute is present, removing the need for explicit variant configuration.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/blog/tailwindcss-v4/index.mdx#_snippet_14

LANGUAGE: HTML
CODE:
```
<div data-current class="opacity-75 data-current:opacity-100">
  <!-- ... -->
</div>
```

----------------------------------------

TITLE: Demonstrating Absolute vs. Static Positioning in HTML
DESCRIPTION: This HTML snippet, rendered via a JSX-like structure, visually demonstrates the difference between static and absolute positioning using Tailwind CSS classes. It highlights how an absolutely positioned child element is taken out of the normal document flow, affecting the layout of its siblings.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/position.mdx#_snippet_2

LANGUAGE: HTML
CODE:
```
<div className="space-y-8">
  <div>
    <p className="mb-4 text-sm font-medium text-gray-500">With static positioning</p>
    <div className="relative text-sm leading-6 font-medium">
      <div className="relative rounded-lg border border-indigo-700/10 bg-indigo-400/20 p-4 dark:border-0 dark:bg-indigo-900/80">
        <p className="-mt-2 mb-2 text-indigo-700 dark:text-indigo-200">Relative parent</p>
        <div className="static flex h-32 flex-col justify-between border border-indigo-700/10 bg-indigo-400/20 p-4 dark:border-0">
          <p className="text-indigo-700 dark:text-indigo-200">Static parent</p>
          <div className="flex gap-4">
            <div className="bottom-0 left-0 rounded-lg bg-indigo-500 p-4 text-white shadow-lg">
              <p>Static child?</p>
            </div>
            <div className="rounded-lg bg-indigo-100 p-4 text-indigo-600 shadow-lg">
              <p>Static sibling</p>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
  <div>
    <p className="mb-4 text-sm font-medium text-gray-500">With absolute positioning</p>
    <div className="relative text-sm leading-6 font-medium">
      <div className="relative rounded-lg border border-indigo-700/10 bg-indigo-400/20 p-4 dark:border-0 dark:bg-indigo-900/80">
        <p className="-mt-2 mb-2 text-indigo-700 dark:text-indigo-200">Relative parent</p>
        <div className="static flex h-32 flex-col justify-between border border-indigo-700/10 bg-indigo-400/20 p-4 dark:border-0">
          <p className="text-indigo-700 dark:text-indigo-200">Static parent</p>
          <div className="flex gap-4">
            <div className="absolute top-0 right-0 rounded-lg bg-indigo-500 p-4 text-white shadow-lg">
              <p>Absolute child</p>
            </div>
            <div className="rounded-lg bg-indigo-100 p-4 text-indigo-600 shadow-lg">
              <p>Static sibling</p>
            </div>
          </div>
        </div>
      </div>
    </div>
  </div>
</div>
```

----------------------------------------

TITLE: Implementing Sticky Table Headers with Tailwind CSS (HTML)
DESCRIPTION: This HTML snippet demonstrates a table structure styled with Tailwind CSS to achieve sticky headers. It uses `border-separate` and `border-spacing-0` on the table element, along with `sticky top-0 z-10` on the table headers, to ensure the headers and their borders remain visible at the top of the viewport during vertical scrolling. This approach is crucial for correct border behavior with sticky elements.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/blog/tailwindcss-v3-1/index.mdx#_snippet_12

LANGUAGE: html
CODE:
```
<!-- [!code word:border-separate] -->
<!-- [!code word:border-spacing-0] -->
<table class="border-separate border-spacing-0">
  <thead class="bg-gray-50">
    <tr>
      <th class="sticky top-0 z-10 border-b border-gray-300 ...">Name</th>
      <th class="sticky top-0 z-10 border-b border-gray-300 ...">Email</th>
      <th class="sticky top-0 z-10 border-b border-gray-300 ...">Role</th>
    </tr>
  </thead>
  <tbody class="bg-white">
    <tr>
      <td class="border-b border-gray-200 ...">Courtney Henry</td>
      <td class="border-b border-gray-200 ...">courtney.henry@example.com</td>
      <td class="border-b border-gray-200 ...">Admin</td>
    </tr>
    <!-- ... -->
  </tbody>
</table>
```

----------------------------------------

TITLE: Adding Dividers Between Children with Tailwind CSS (HTML)
DESCRIPTION: This HTML snippet demonstrates the application of the `divide-x-4` utility to insert borders between child elements. It effectively creates a visual separation along the horizontal axis for elements within a grid layout.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/border-width.mdx#_snippet_6

LANGUAGE: html
CODE:
```
<!-- [!code classes:divide-x-4] -->
<div class="grid grid-cols-3 divide-x-4">
  <div>01</div>
  <div>02</div>
  <div>03</div>
</div>
```

----------------------------------------

TITLE: Building Custom Checkbox with Headless UI React
DESCRIPTION: This snippet demonstrates how to create a custom checkbox using Headless UI's `Checkbox` component. It shows how to apply styling based on the checked state and focus, and how to associate it with a `Label` and `Description` for accessibility. The `CheckmarkIcon` is used for visual feedback when checked.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/blog/headless-ui-v2/index.mdx#_snippet_2

LANGUAGE: jsx
CODE:
```
import { Checkbox, Description, Field, Label } from "@headlessui/react";
import { CheckmarkIcon } from "./icons/checkmark";
import clsx from "clsx";

function Example() {
  return (
    <Field>
      // [!code highlight:11]
      <Checkbox
        defaultChecked
        className={clsx(
          "size-4 rounded border bg-white dark:bg-white/5",
          "data-[checked]:border-transparent data-[checked]:bg-blue-500",
          "focus:outline-none data-[focus]:outline-2 data-[focus]:outline-offset-2 data-[focus]:outline-blue-500",
        )}
      >
        <CheckmarkIcon className="stroke-white opacity-0 group-data-[checked]:opacity-100" />
      </Checkbox>
      <div>
        <Label>Enable beta features</Label>
        <Description>This will give you early access to any awesome new features we're developing.</Description>
      </div>
    </Field>
  );
}
```

----------------------------------------

TITLE: Truncating Text with Tailwind CSS `truncate` Utility
DESCRIPTION: Use the `truncate` utility to prevent text from wrapping and automatically add an ellipsis (...) to overflowing text. This utility combines `overflow: hidden`, `text-overflow: ellipsis`, and `white-space: nowrap` for a concise solution to text truncation.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/text-overflow.mdx#_snippet_1

LANGUAGE: HTML
CODE:
```
<!-- [!code classes:truncate] -->
<p class="truncate">The longest word in any of the major...</p>
```

----------------------------------------

TITLE: Applying Fixed Opacity to Background Colors in HTML
DESCRIPTION: This HTML snippet demonstrates how to apply fixed opacity levels to background colors using Tailwind CSS utility classes. Each `div` element uses a `bg-sky-500/XX` class, where `XX` represents a percentage value for the alpha channel, ranging from 10% to 100%. This allows for precise control over the transparency of the background color.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/colors.mdx#_snippet_5

LANGUAGE: HTML
CODE:
```
<div>
  <div class="bg-sky-500/10"></div>
  <div class="bg-sky-500/20"></div>
  <div class="bg-sky-500/30"></div>
  <div class="bg-sky-500/40"></div>
  <div class="bg-sky-500/50"></div>
  <div class="bg-sky-500/60"></div>
  <div class="bg-sky-500/70"></div>
  <div class="bg-sky-500/80"></div>
  <div class="bg-sky-500/90"></div>
  <div class="bg-sky-500/100"></div>
</div>
```

----------------------------------------

TITLE: Applying Overridden Tailwind CSS Breakpoint in HTML
DESCRIPTION: This HTML snippet demonstrates the effect of an overridden `sm` breakpoint in Tailwind CSS. The `sm:grid-cols-3` class will now apply its styles when the viewport reaches the custom `30rem` breakpoint, showcasing how custom theme variables influence responsive utility classes.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/theme.mdx#_snippet_13

LANGUAGE: html
CODE:
```
<div class="grid grid-cols-1 sm:grid-cols-3">
  <!-- ... -->
</div>
```

----------------------------------------

TITLE: Setting Percentage-Based Widths with Tailwind CSS
DESCRIPTION: This snippet demonstrates how to apply percentage-based widths to elements using Tailwind CSS utilities like `w-1/2`, `w-2/5`, `w-1/3`, and `w-full`. These utilities are ideal for creating responsive layouts where elements occupy a specific fraction of their parent's width. The examples show various fractional widths and a full-width element, illustrating how to distribute space within a flex container.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/width.mdx#_snippet_5

LANGUAGE: jsx
CODE:
```
<div className="space-y-4 font-mono text-xs font-bold text-white">
  <div className="flex gap-x-4">
    <div className="w-1/2 rounded-lg bg-violet-500 px-4 py-2 text-center">w-1/2</div>
    <div className="w-1/2 rounded-lg bg-violet-500 px-4 py-2 text-center">w-1/2</div>
  </div>
  <div className="flex gap-x-4">
    <div className="w-2/5 rounded-lg bg-violet-500 px-4 py-2 text-center">w-2/5</div>
    <div className="w-3/5 rounded-lg bg-violet-500 px-4 py-2 text-center">w-3/5</div>
  </div>
  <div className="flex gap-x-4">
    <div className="w-1/3 rounded-lg bg-violet-500 px-4 py-2 text-center">w-1/3</div>
    <div className="w-2/3 rounded-lg bg-violet-500 px-4 py-2 text-center">w-2/3</div>
  </div>
  <div className="hidden gap-x-4 sm:flex">
    <div className="w-1/4 rounded-lg bg-violet-500 px-4 py-2 text-center">w-1/4</div>
    <div className="w-3/4 rounded-lg bg-violet-500 px-4 py-2 text-center">w-3/4</div>
  </div>
  <div className="hidden gap-x-4 sm:flex">
    <div className="w-1/5 rounded-lg bg-violet-500 px-4 py-2 text-center">w-1/5</div>
    <div className="w-4/5 rounded-lg bg-violet-500 px-4 py-2 text-center">w-4/5</div>
  </div>
  <div className="hidden gap-x-4 sm:flex">
    <div className="w-1/6 rounded-lg bg-violet-500 px-4 py-2 text-center">w-1/6</div>
    <div className="w-5/6 rounded-lg bg-violet-500 px-4 py-2 text-center">w-5/6</div>
  </div>
  <div className="w-full rounded-lg bg-violet-500 px-4 py-2 text-center font-mono text-white">w-full</div>
</div>
```

LANGUAGE: html
CODE:
```
<!-- [!code classes:w-1/2,w-2/5,w-3/5,w-1/3,w-2/3,w-1/4,w-3/4,w-1/5,w-4/5,w-1/6,w-5/6,w-full] -->
<div class="flex ...">
  <div class="w-1/2 ...">w-1/2</div>
  <div class="w-1/2 ...">w-1/2</div>
</div>
<div class="flex ...">
  <div class="w-2/5 ...">w-2/5</div>
  <div class="w-3/5 ...">w-3/5</div>
</div>
<div class="flex ...">
  <div class="w-1/3 ...">w-1/3</div>
  <div class="w-2/3 ...">w-2/3</div>
</div>
<div class="flex ...">
  <div class="w-1/4 ...">w-1/4</div>
  <div class="w-3/4 ...">w-3/4</div>
</div>
<div class="flex ...">
  <div class="w-1/5 ...">w-1/5</div>
  <div class="w-4/5 ...">w-4/5</div>
</div>
<div class="flex ...">
  <div class="w-1/6 ...">w-1/6</div>
  <div class="w-5/6 ...">w-5/6</div>
</div>
<div class="w-full ...">w-full</div>
```

----------------------------------------

TITLE: Implementing Horizontal Scroll Snapping with JSX and Tailwind CSS
DESCRIPTION: This JSX snippet demonstrates how to create a horizontally scrollable container where child elements snap to their start. It utilizes Tailwind CSS classes such as `snap-x` and `snap-mandatory` on the parent container and `snap-start` along with `scroll-mx-6` on individual image wrappers to ensure precise alignment upon scrolling.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/scroll-snap-align.mdx#_snippet_3

LANGUAGE: JSX
CODE:
```
<div className="relative">
  <div className="mb-6 ml-6 flex items-end justify-start pt-10">
    <div className="dark:highlight-white/10 ml-2 rounded bg-indigo-50 px-1.5 font-mono text-[0.625rem] leading-6 text-indigo-600 ring-1 ring-indigo-600 ring-inset dark:bg-indigo-500 dark:text-white dark:ring-0">
      snap point
    </div>
    <div className="absolute top-0 bottom-0 left-6 border-l border-indigo-500"></div>
  </div>
  <div className="relative flex w-full snap-x snap-mandatory gap-6 overflow-x-auto pb-14">
    <div className="shrink-0 snap-start scroll-mx-6">
      <div className="w-0 shrink-0"></div>
    </div>
    <div className="shrink-0 snap-start scroll-mx-6">
      <img
        className="h-40 w-80 shrink-0 rounded-lg bg-white"
        src="https://images.unsplash.com/photo-1604999565976-8913ad2ddb7c?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=320&h=160&q=80"
      />
    </div>
    <div className="shrink-0 snap-start scroll-mx-6">
      <img
        className="h-40 w-80 shrink-0 rounded-lg bg-white"
        src="https://images.unsplash.com/photo-1540206351-d6465b3ac5c1?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=320&h=160&q=80"
      />
    </div>
    <div className="shrink-0 snap-start scroll-mx-6">
      <img
        className="h-40 w-80 shrink-0 rounded-lg bg-white"
        src="https://images.unsplash.com/photo-1622890806166-111d7f6c7c97?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=320&h=160&q=80"
      />
    </div>
    <div className="shrink-0 snap-start scroll-mx-6">
      <img
        className="h-40 w-80 shrink-0 rounded-lg bg-white"
        src="https://images.unsplash.com/photo-1590523277543-a94d2e4eb00b?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=320&h=160&q=80"
      />
    </div>
    <div className="shrink-0 snap-start scroll-mx-6">
      <img
        className="h-40 w-80 shrink-0 rounded-lg bg-white"
        src="https://images.unsplash.com/photo-1575424909138-46b05e5919ec?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=crop&w=320&h=160&q=80"
      />
    </div>
    <div className="shrink-0 snap-start scroll-mx-6">
      <div className="h-40 w-3 shrink-0 sm:-ml-[2px] sm:w-96"></div>
    </div>
  </div>
</div>
```

----------------------------------------

TITLE: Creating a Table with Separate Borders using Tailwind CSS in JSX
DESCRIPTION: This JSX snippet demonstrates how to construct an HTML table using React components and apply Tailwind CSS utilities, including `border-separate`, to ensure each cell has its own distinct border. It also includes styling for dark mode and responsive padding.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/border-collapse.mdx#_snippet_1

LANGUAGE: jsx
CODE:
```
<div className="px-4 py-8 sm:px-8">
  <table className="w-full border-separate border border-gray-400 bg-white text-sm dark:border-gray-500 dark:bg-gray-800">
    <thead className="bg-gray-50 dark:bg-gray-700">
      <tr>
        <th className="w-1/2 border border-gray-300 p-4 text-left font-semibold text-gray-900 dark:border-gray-600 dark:text-gray-200">
          State
        </th>
        <th className="w-1/2 border border-gray-300 p-4 text-left font-semibold text-gray-900 dark:border-gray-600 dark:text-gray-200">
          City
        </th>
      </tr>
    </thead>
    <tbody>
      <tr>
        <td className="border border-gray-300 p-4 text-gray-500 dark:border-gray-700 dark:text-gray-400">
          Indiana
        </td>
        <td className="border border-gray-300 p-4 text-gray-500 dark:border-gray-700 dark:text-gray-400">
          Indianapolis
        </td>
      </tr>
      <tr>
        <td className="border border-gray-300 p-4 text-gray-500 dark:border-gray-700 dark:text-gray-400">Ohio</td>
        <td className="border border-gray-300 p-4 text-gray-500 dark:border-gray-700 dark:text-gray-400">
          Columbus
        </td>
      </tr>
      <tr>
        <td className="border border-gray-300 p-4 text-gray-500 dark:border-gray-700 dark:text-gray-400">
          Michigan
        </td>
        <td className="border border-gray-300 p-4 text-gray-500 dark:border-gray-700 dark:text-gray-400">
          Detroit
        </td>
      </tr>
    </tbody>
  </table>
</div>
```

----------------------------------------

TITLE: Shorthand Color Opacity Syntax in HTML
DESCRIPTION: This HTML snippet demonstrates the new shorthand syntax for defining color opacity directly within the color utility. It shows the transition from the verbose `bg-opacity-25` utility to the more concise `bg-red-500/25` syntax, simplifying color alpha channel adjustments.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/blog/tailwindcss-2-2/index.mdx#_snippet_12

LANGUAGE: HTML
CODE:
```
<div class="bg-red-500 bg-opacity-25"> <!-- [!code --] -->
<div class="bg-red-500/25"> <!-- [!code ++] -->
</div>
```

----------------------------------------

TITLE: Implementing Scroll-Triggered Entrance Animations with React and Framer Motion
DESCRIPTION: This React component demonstrates how to apply scroll-triggered entrance animations to elements using custom `FadeIn` and `FadeInStagger` components, which wrap Framer Motion functionalities. It's used here to animate a heading and a list of client logos, making elements fade in as they enter the viewport. The `faster` prop on `FadeInStagger` can adjust the animation speed.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/blog/2023-08-07-meet-studio-our-new-agency-template/index.mdx#_snippet_1

LANGUAGE: jsx
CODE:
```
function Clients() {
  return (
    <div className="mt-24 rounded-4xl bg-neutral-950 py-20 sm:mt-32 sm:py-32 lg:mt-56">
      <Container>
        <FadeIn className="flex items-center gap-x-8">
          <h2 className="font-display text-center text-sm font-semibold tracking-wider text-white sm:text-left">
            We’ve worked with hundreds of amazing people
          </h2>
          <div className="h-px flex-auto bg-neutral-800" />
        </FadeIn>
        <FadeInStagger faster>
          <ul role="list" className="mt-10 grid grid-cols-2 gap-x-8 gap-y-10 lg:grid-cols-4">
            {clients.map(([client, logo]) => (
              <li key={client}>
                <FadeIn>
                  <Image src={logo} alt={client} unoptimized />
                </FadeIn>
              </li>
            ))}
          </ul>
        </FadeInStagger>
      </Container>
    </div>
  );
}
```

----------------------------------------

TITLE: Applying Vertical Padding with Tailwind CSS
DESCRIPTION: This HTML snippet showcases the `py-<number>` utility, such as `py-8`, for applying vertical padding to an element. This utility class simultaneously controls both the top and bottom padding, ensuring symmetrical spacing along the y-axis.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/padding.mdx#_snippet_4

LANGUAGE: html
CODE:
```
<!-- [!code classes:py-8] -->\n<div class="py-8 ...">py-8</div>
```

----------------------------------------

TITLE: Using Named Container Queries in Tailwind CSS HTML
DESCRIPTION: This HTML snippet demonstrates how to use named container queries for more complex layouts. By naming a container with `@container/main`, child elements can target specific containers using variants like `@sm/main:flex-col`, allowing styles to depend on a distant parent's size.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/responsive-design.mdx#_snippet_17

LANGUAGE: html
CODE:
```
<div class="@container/main">
  <!-- ... -->
  <div class="flex flex-row @sm/main:flex-col">
    <!-- ... -->
  </div>
</div>
```

----------------------------------------

TITLE: Defining Blog Post Metadata (JavaScript)
DESCRIPTION: This JavaScript object defines the metadata for a blog post, including its title, description, publication date, authors, a featured image, and an excerpt. The 'excerpt' field uses JSX to embed a 'Link' component, providing a rich text summary for the post.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/blog/introducing-linting-for-tailwindcss-intellisense/index.mdx#_snippet_1

LANGUAGE: JavaScript
CODE:
```
export const meta = {
  title: "Introducing linting for Tailwind CSS IntelliSense",
  description:
    "Today we’re releasing a new version of the Tailwind CSS IntelliSense extension for Visual Studio Code that adds Tailwind-specific linting to both your CSS and your markup.",
  date: "2020-06-23T18:52:03Z",
  authors: [bradlc],
  image,
  excerpt: (
    <>
      Today we’re releasing a new version of the{" "}
      <Link href="https://marketplace.visualstudio.com/items?itemName=bradlc.vscode-tailwindcss">
        Tailwind CSS IntelliSense extension for Visual Studio Code
      </Link>{" "}
      that adds Tailwind-specific linting to both your CSS and your markup.
    </>
  )
};
```

----------------------------------------

TITLE: Customizing Breakpoints in Tailwind CSS
DESCRIPTION: This CSS snippet demonstrates how to customize default Tailwind CSS breakpoints and define new ones using `--breakpoint-*` CSS variables within the `@theme` block. It updates the `2xl` breakpoint and introduces `xs` and `3xl` breakpoints, allowing for more granular control over responsive design.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/responsive-design.mdx#_snippet_9

LANGUAGE: css
CODE:
```
@import "tailwindcss";

@theme {
  --breakpoint-xs: 30rem;
  --breakpoint-2xl: 100rem;
  --breakpoint-3xl: 120rem;
}
```

----------------------------------------

TITLE: Demonstrating Logical Padding with JSX
DESCRIPTION: This JSX snippet illustrates the application of `ps-8` and `pe-8` Tailwind CSS utilities for logical padding in both left-to-right (LTR) and right-to-left (RTL) text directions. It shows how these utilities adapt to the text flow, with `ps-8` applying padding to the start and `pe-8` to the end of the inline axis, regardless of the physical left/right.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/padding.mdx#_snippet_5

LANGUAGE: JSX
CODE:
```
<div className="grid grid-cols-2 place-items-center gap-x-4">
  <div className="flex flex-col items-start gap-y-4">
    <p className="text-sm font-medium">Left-to-right</p>
    <div className="flex overflow-hidden rounded-lg bg-indigo-500 font-mono text-sm leading-6 font-bold text-white">
      <Stripes noColor className="min-h-full w-8 rounded-s-lg text-white/50" />
      <div className="p-4">ps-8</div>
    </div>
    <div className="mt-4 flex overflow-hidden rounded-lg bg-indigo-500 font-mono text-sm leading-6 font-bold text-white">
      <div className="p-4">pe-8</div>
      <Stripes noColor className="min-h-full w-8 rounded-e-lg text-white/50" />
    </div>
  </div>
  <div className="flex flex-col items-end gap-y-4">
    <p className="text-sm font-medium">Right-to-left</p>
    <div className="flex overflow-hidden rounded-lg bg-indigo-500 font-mono text-sm leading-6 font-bold text-white">
      <div className="p-4">ps-8</div>
      <Stripes noColor className="min-h-full w-8 rounded-e-lg text-white/50" />
    </div>
    <div className="mt-4 flex overflow-hidden rounded-lg bg-indigo-500 font-mono text-sm leading-6 font-bold text-white">
      <Stripes noColor className="min-h-full w-8 rounded-s-lg text-white/50" />
      <div className="p-4">pe-8</div>
    </div>
  </div>
</div>
```

----------------------------------------

TITLE: Safely Centering Flex Items with `justify-center-safe` in HTML
DESCRIPTION: This HTML snippet illustrates the `justify-center-safe` utility, which centers flex items but intelligently switches to `start` alignment when the container overflows. This prevents content from overflowing in both directions, ensuring visibility and a better user experience on smaller screens.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/blog/tailwindcss-v4-1/index.mdx#_snippet_21

LANGUAGE: html
CODE:
```
<ul class="flex justify-center-safe gap-2 ...">
  <li>Sales</li>
  <li>Marketing</li>
  <li>SEO</li>
  <!-- ... -->
</ul>
```

----------------------------------------

TITLE: Applying `forced-color-adjust-none` in HTML
DESCRIPTION: This HTML snippet demonstrates the use of the `forced-color-adjust-none` utility class within a `div` element. This utility prevents the browser's forced colors mode from altering the specified colors within that element, ensuring critical design elements, like product color choices, retain their intended appearance. It requires Tailwind CSS v3.4 or later.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/blog/tailwindcss-v3-4/index.mdx#_snippet_15

LANGUAGE: HTML
CODE:
```
<fieldset>
  <legend>Choose a color</legend>
  <div class="forced-color-adjust-none ...">
    <label>
      <input class="sr-only" type="radio" name="color-choice" value="white" />
      <span class="sr-only">White</span>
      <span class="size-6 rounded-full bg-white"></span>
    </label>
    <label>
      <input class="sr-only" type="radio" name="color-choice" value="gray" />
      <span class="sr-only">Gray</span>
      <span class="size-6 rounded-full bg-gray-300"></span>
    </label>
    <!-- ... -->
  </div>
</fieldset>
```

----------------------------------------

TITLE: Centering Items with `items-center` in Tailwind CSS (HTML/JSX)
DESCRIPTION: This snippet demonstrates how to use the `items-center` utility in Tailwind CSS to vertically center flex items along the container's cross axis. It includes both a React JSX example and a plain HTML example for implementation.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/align-items.mdx#_snippet_2

LANGUAGE: tsx
CODE:
```
<div className="grid grid-cols-1">
  <Stripes border className="col-start-1 row-start-1 rounded-lg" />
  <div className="col-start-1 row-start-1 flex w-full items-center gap-4 rounded-lg text-center font-mono text-sm leading-6 font-bold text-white">
    <div className="flex flex-1 items-center justify-center rounded-lg bg-violet-500 py-4">01</div>
    <div className="flex flex-1 items-center justify-center rounded-lg bg-violet-500 py-12">02</div>
    <div className="flex flex-1 items-center justify-center rounded-lg bg-violet-500 py-8">03</div>
  </div>
</div>
```

LANGUAGE: html
CODE:
```
<!-- [!code classes:items-center] -->
<div class="flex items-center ...">
  <div class="py-4">01</div>
  <div class="py-12">02</div>
  <div class="py-8">03</div>
</div>
```

----------------------------------------

TITLE: Integrating Headless UI Listbox with HTML Forms (JSX)
DESCRIPTION: This snippet demonstrates how to integrate a Headless UI 'Listbox' component into a standard HTML form. By adding a 'name' prop to the 'Listbox', Headless UI automatically generates a hidden input field that syncs with the component's value, enabling easy data submission to a server via a POST request.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/blog/2022-05-23-headless-ui-v1-6-tailwind-ui-team-management/index.mdx#_snippet_2

LANGUAGE: jsx
CODE:
```
<form action="/projects/1/assignee" method="post">
  <Listbox
    value={selectedPerson}
    onChange={setSelectedPerson}
    name="assignee"
  >
    {/* ... */}
  </Listbox>
  <button>Submit</button>
</form>
```

----------------------------------------

TITLE: Making Images Inline with Tailwind CSS Utility
DESCRIPTION: This HTML snippet demonstrates how to override Preflight's default `display: block` for images by applying the `inline` utility class. This allows an image to behave as an inline element, useful for specific layout requirements where an image needs to flow with text.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/preflight.mdx#_snippet_9

LANGUAGE: HTML
CODE:
```
<img class="inline" src="..." alt="..." />
```

----------------------------------------

TITLE: Enabling Transitions Only for Motion-Safe Users in HTML
DESCRIPTION: This HTML snippet illustrates the `motion-safe` variant, which applies styles only when the user has *not* opted for reduced motion. The `motion-safe:transition` class ensures that a transition is applied only to users who are not sensitive to motion, allowing for explicit opt-in to animations and transitions.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/blog/tailwindcss-1-6/index.mdx#_snippet_3

LANGUAGE: html
CODE:
```
<div class="duration-150 ease-in-out motion-safe:transition ... ..."></div>
```

----------------------------------------

TITLE: Supporting Reduced Motion with Tailwind CSS
DESCRIPTION: This HTML snippet shows how to use `motion-reduce` variants to conditionally disable transitions and transforms for users who prefer reduced motion. It ensures a smoother experience for accessibility-conscious users.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/transition-property.mdx#_snippet_10

LANGUAGE: HTML
CODE:
```
<button class="transform transition hover:-translate-y-1 motion-reduce:transition-none motion-reduce:hover:transform-none ...">
  <!-- ... -->
</button>
```

----------------------------------------

TITLE: Defining OKLCH Color Variables for Tailwind CSS
DESCRIPTION: This CSS snippet defines a comprehensive set of custom properties for various color palettes using the OKLCH color model. Each variable, named with a color and shade (e.g., `--color-cyan-500`), provides a specific color value in the perceptually uniform OKLCH format, ensuring consistent and accessible color usage across a design system. These variables serve as foundational color tokens for a framework like Tailwind CSS.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/theme.mdx#_snippet_29

LANGUAGE: CSS
CODE:
```
--color-cyan-300: oklch(0.865 0.127 207.078);
  --color-cyan-400: oklch(0.789 0.154 211.53);
  --color-cyan-500: oklch(0.715 0.143 215.221);
  --color-cyan-600: oklch(0.609 0.126 221.723);
  --color-cyan-700: oklch(0.52 0.105 223.128);
  --color-cyan-800: oklch(0.45 0.085 224.283);
  --color-cyan-900: oklch(0.398 0.07 227.392);
  --color-cyan-950: oklch(0.302 0.056 229.695);

  --color-sky-50: oklch(0.977 0.013 236.62);
  --color-sky-100: oklch(0.951 0.026 236.824);
  --color-sky-200: oklch(0.901 0.058 230.902);
  --color-sky-300: oklch(0.828 0.111 230.318);
  --color-sky-400: oklch(0.746 0.16 232.661);
  --color-sky-500: oklch(0.685 0.169 237.323);
  --color-sky-600: oklch(0.588 0.158 241.966);
  --color-sky-700: oklch(0.5 0.134 242.749);
  --color-sky-800: oklch(0.443 0.11 240.79);
  --color-sky-900: oklch(0.391 0.09 240.876);
  --color-sky-950: oklch(0.293 0.066 243.157);

  --color-blue-50: oklch(0.97 0.014 254.604);
  --color-blue-100: oklch(0.932 0.032 255.585);
  --color-blue-200: oklch(0.882 0.059 254.128);
  --color-blue-300: oklch(0.809 0.105 251.813);
  --color-blue-400: oklch(0.707 0.165 254.624);
  --color-blue-500: oklch(0.623 0.214 259.815);
  --color-blue-600: oklch(0.546 0.245 262.881);
  --color-blue-700: oklch(0.488 0.243 264.376);
  --color-blue-800: oklch(0.424 0.199 265.638);
  --color-blue-900: oklch(0.379 0.146 265.522);
  --color-blue-950: oklch(0.282 0.091 267.935);

  --color-indigo-50: oklch(0.962 0.018 272.314);
  --color-indigo-100: oklch(0.93 0.034 272.788);
  --color-indigo-200: oklch(0.87 0.065 274.039);
  --color-indigo-300: oklch(0.785 0.115 274.713);
  --color-indigo-400: oklch(0.673 0.182 276.935);
  --color-indigo-500: oklch(0.585 0.233 277.117);
  --color-indigo-600: oklch(0.511 0.262 276.966);
  --color-indigo-700: oklch(0.457 0.24 277.023);
  --color-indigo-800: oklch(0.398 0.195 277.366);
  --color-indigo-900: oklch(0.359 0.144 278.697);
  --color-indigo-950: oklch(0.257 0.09 281.288);

  --color-violet-50: oklch(0.969 0.016 293.756);
  --color-violet-100: oklch(0.943 0.029 294.588);
  --color-violet-200: oklch(0.894 0.057 293.283);
  --color-violet-300: oklch(0.811 0.111 293.571);
  --color-violet-400: oklch(0.702 0.183 293.541);
  --color-violet-500: oklch(0.606 0.25 292.717);
  --color-violet-600: oklch(0.541 0.281 293.009);
  --color-violet-700: oklch(0.491 0.27 292.581);
  --color-violet-800: oklch(0.432 0.232 292.759);
  --color-violet-900: oklch(0.38 0.189 293.745);
  --color-violet-950: oklch(0.283 0.141 291.089);

  --color-purple-50: oklch(0.977 0.014 308.299);
  --color-purple-100: oklch(0.946 0.033 307.174);
  --color-purple-200: oklch(0.902 0.063 306.703);
  --color-purple-300: oklch(0.827 0.119 306.383);
  --color-purple-400: oklch(0.714 0.203 305.504);
  --color-purple-500: oklch(0.627 0.265 303.9);
  --color-purple-600: oklch(0.558 0.288 302.321);
  --color-purple-700: oklch(0.496 0.265 301.924);
  --color-purple-800: oklch(0.438 0.218 303.724);
  --color-purple-900: oklch(0.381 0.176 304.987);
  --color-purple-950: oklch(0.291 0.149 302.717);

  --color-fuchsia-50: oklch(0.977 0.017 320.058);
  --color-fuchsia-100: oklch(0.952 0.037 318.852);
  --color-fuchsia-200: oklch(0.903 0.076 319.62);
  --color-fuchsia-300: oklch(0.833 0.145 321.434);
  --color-fuchsia-400: oklch(0.74 0.238 322.16);
  --color-fuchsia-500: oklch(0.667 0.295 322.15);
  --color-fuchsia-600: oklch(0.591 0.293 322.896);
  --color-fuchsia-700: oklch(0.518 0.253 323.949);
  --color-fuchsia-800: oklch(0.452 0.211 324.591);
  --color-fuchsia-900: oklch(0.401 0.17 325.612);
  --color-fuchsia-950: oklch(0.293 0.136 325.661);

  --color-pink-50: oklch(0.971 0.014 343.198);
  --color-pink-100: oklch(0.948 0.028 342.258);
  --color-pink-200: oklch(0.899 0.061 343.231);
  --color-pink-300: oklch(0.823 0.12 346.018);
  --color-pink-400: oklch(0.718 0.202 349.761);
  --color-pink-500: oklch(0.656 0.241 354.308);
  --color-pink-600: oklch(0.592 0.249 0.584);
  --color-pink-700: oklch(0.525 0.223 3.958);
  --color-pink-800: oklch(0.459 0.187 3.815);
  --color-pink-900: oklch(0.408 0.153 2.432);
  --color-pink-950: oklch(0.284 0.109 3.907);

  --color-rose-50: oklch(0.969 0.015 12.422);
  --color-rose-100: oklch(0.941 0.03 12.58);
  --color-rose-200: oklch(0.892 0.058 10.001);
  --color-rose-300: oklch(0.81 0.117 11.638);
  --color-rose-400: oklch(0.712 0.194 13.428);
  --color-rose-500: oklch(0.645 0.246 16.439);
  --color-rose-600: oklch(0.586 0.253 17.585);
  --color-rose-700: oklch(0.514 0.222 16.935);
  --color-rose-800: oklch(0.455 0.188 13.697);
  --color-rose-900: oklch(0.41 0.159 10.272);
  --color-rose-950: oklch(0.271 0.105 12.094);

  --color-slate-50: oklch(0.984 0.003 247.858);
  --color-slate-100: oklch(0.968 0.007 247.896);
  --color-slate-200: oklch(0.929 0.013 255.508);
  --color-slate-300: oklch(0.869 0.022 252.894);
  --color-slate-400: oklch(0.704 0.04 256.788);
  --color-slate-500: oklch(0.554 0.046 257.417);
  --color-slate-600: oklch(0.446 0.043 257.281);
```

----------------------------------------

TITLE: Integrating Theme Variables with Framer Motion in JSX
DESCRIPTION: This JSX snippet illustrates how native CSS variables, derived from the Tailwind theme, can be directly utilized within JavaScript UI libraries like Framer Motion. This avoids the need for resolveConfig(), streamlining the integration of design tokens into animation properties.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/blog/tailwindcss-v4-alpha/index.mdx#_snippet_10

LANGUAGE: jsx
CODE:
```
// [!code filename:JSX]
import { motion } from "framer-motion";

export const MyComponent = () => (
  <motion.div initial={{ y: "var(--spacing-8)" }} animate={{ y: 0 }} exit={{ y: "var(--spacing-8)" }}>
    {children}
  </motion.div>
);
```

----------------------------------------

TITLE: Displaying an Image in JSX (React/Next.js)
DESCRIPTION: This snippet demonstrates how to render an image using a custom `Image` component, likely from a component library like Next.js or a custom component. It takes `src` for the image source and `alt` for accessibility, providing a descriptive text for the image.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/blog/tailwindcss-v4-alpha/index.mdx#_snippet_1

LANGUAGE: JSX
CODE:
```
<Image src={card} alt="Tailwind CSS v4.0-alpha" />
```

----------------------------------------

TITLE: Applying Logical Borders with Tailwind CSS (HTML)
DESCRIPTION: This HTML snippet illustrates the use of Tailwind CSS logical properties such as `border-s-4`. It demonstrates how the `border-inline-start-width` property dynamically adjusts to the text direction, applying the border to the left in `ltr` (left-to-right) contexts and to the right in `rtl` (right-to-left) contexts.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/border-width.mdx#_snippet_4

LANGUAGE: html
CODE:
```
<!-- [!code word:dir="ltr"] -->
<!-- [!code word:dir="rtl"] -->
<!-- [!code classes:border-s-4] -->
<div dir="ltr">
  <div class="border-s-4 ..."></div>
</div>
<div dir="rtl">
  <div class="border-s-4 ..."></div>
</div>
```

----------------------------------------

TITLE: Implementing Continuous CSS Animations with Negative Delay (JSX)
DESCRIPTION: This `Logo` component demonstrates a CSS-driven animation technique where logos continuously move across the screen and pause on hover. It leverages a negative `animation-delay` to offset the starting position of each logo within a shared animation keyframe, allowing them to appear at different points in their cycle. The `group-hover:[animation-play-state:running]` class dynamically controls the animation's play state based on the parent's hover, eliminating the need for JavaScript state management.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/blog/2024-09-12-radiant-a-beautiful-new-marketing-site-template/index.mdx#_snippet_5

LANGUAGE: JSX
CODE:
```
// [!code filename:logo-timeline.tsx]
function Logo({
  label,
  src,
  className,
}: {
  label: string
  src: string
  className: string
}) {
  return (
    <div
      className={clsx(
        className,
        'absolute top-2 grid grid-cols-[1rem,1fr] items-center gap-2 whitespace-nowrap px-3 py-1',
        'rounded-full bg-gradient-to-t from-gray-800 from-50% to-gray-700 ring-1 ring-inset ring-white/10',
        // [!code highlight:2]
        '[--move-x-from:-100%] [--move-x-to:calc(100%+100cqw)] [animation-iteration-count:infinite] [animation-name:move-x] [animation-play-state:paused] [animation-timing-function:linear] group-hover:[animation-play-state:running]',
      )}
    >
      <img alt="" src={src} className="size-4" />
      <span className="text-sm/6 font-medium text-white">{label}</span>
    </div>
  )
}

export function LogoTimeline() {
  return (
    /* ... */
    <Row>
      <Logo
        label="Loom"
        src="./logo-timeline/loom.svg"
        // [!code highlight:2]
        className="[animation-delay:-26s] [animation-duration:30s]"
      />
      <Logo
        label="Gmail"
        src="./logo-timeline/gmail.svg"
        // [!code highlight:2]
        className="[animation-delay:-8s] [animation-duration:30s]"
      />
    </Row>
    /* ... */
```

----------------------------------------

TITLE: Styling Required Inputs with :required in HTML
DESCRIPTION: This snippet demonstrates how to apply a Tailwind CSS class to an input element when it is marked as required using the `required` variant. This is crucial for form validation, visually indicating mandatory fields with a `border-red-500`.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/hover-focus-and-other-states.mdx#_snippet_253

LANGUAGE: HTML
CODE:
```
<input required class="border required:border-red-500 ..." />
```

----------------------------------------

TITLE: Setting All Border Widths with Custom Property in Tailwind CSS
DESCRIPTION: Applies a border width defined by a CSS custom property (variable) to all sides of an element. This enables dynamic sizing and consistency across components by referencing a centrally defined value.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/border-width.mdx#_snippet_11

LANGUAGE: css
CODE:
```
border-width: var(<custom-property>);
```

----------------------------------------

TITLE: Defining Container Query for @max-7xl (Width < 80rem) in CSS
DESCRIPTION: This snippet defines a container query associated with the @max-7xl breakpoint, applying styles when the container's width is less than 80 rem. It's used for responsive design based on the parent container's size rather than the viewport.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/hover-focus-and-other-states.mdx#_snippet_192

LANGUAGE: CSS
CODE:
```
@container (width < 80rem)
```

----------------------------------------

TITLE: Implementing Named Groups in Svelte
DESCRIPTION: This Svelte snippet demonstrates how to use named `group` variants (e.g., `group/item`, `group/edit`) to apply conditional styles to child elements based on the hover state of their parent. It shows how `group-hover/item:visible` makes an element visible when the `group/item` parent is hovered, and similar effects for `group-hover/edit`.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/hover-focus-and-other-states.mdx#_snippet_25

LANGUAGE: Svelte
CODE:
```
<ul role="list">
  {#each people as person}
    <li class="group/item ...">
      <!-- ... -->
      <a class="group/edit invisible group-hover/item:visible ..." href="tel:{person.phone}">
        <span class="group-hover/edit:text-gray-700 ...">Call</span>
        <svg class="group-hover/edit:translate-x-0.5 group-hover/edit:text-gray-500 ..."><!-- ... --></svg>
      </a>
    </li>
  {/each}
</ul>
```

----------------------------------------

TITLE: Using Tailwind's `theme()` Function in CSS
DESCRIPTION: This CSS snippet illustrates the use of Tailwind's `theme()` function to retrieve values from the `tailwind.config.js` file. It applies configured border-radius, background color, and text color to a `.select2-dropdown` element, centralizing design token management.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/blog/tailwindcss-v3-1/index.mdx#_snippet_4

LANGUAGE: CSS
CODE:
```
.select2-dropdown {
  border-radius: theme(borderRadius.lg);
  background-color: theme(colors.gray.100);
  color: theme(colors.gray.900);
}
/* ... */
```

----------------------------------------

TITLE: Safelisting Tailwind CSS Utilities with Brace Expansion Ranges
DESCRIPTION: This snippet demonstrates the powerful use of brace expansion within `@source inline()` to generate multiple classes and variants efficiently. It shows how to generate a range of red background colors (e.g., `bg-red-100` to `bg-red-900`) along with their `hover` variants, providing a concise way to include numerous related utilities.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/detecting-classes-in-source-files.mdx#_snippet_12

LANGUAGE: CSS
CODE:
```
@import "tailwindcss";
@source inline("{hover:,}bg-red-{50,{100..900..100},950}");
```

LANGUAGE: Generated CSS
CODE:
```
.bg-red-50 {
  background-color: var(--color-red-50);
}
.bg-red-100 {
  background-color: var(--color-red-100);
}
.bg-red-200 {
  background-color: var(--color-red-200);
}

/* ... */

.bg-red-800 {
  background-color: var(--color-red-800);
}
.bg-red-900 {
  background-color: var(--color-red-900);
}
.bg-red-950 {
  background-color: var(--color-red-950);
}
@media (hover: hover) {
  .hover\:bg-red-50:hover {
    background-color: var(--color-red-50);
  }

  /* ... */

  .hover\:bg-red-950:hover {
    background-color: var(--color-red-950);
  }
}
```

----------------------------------------

TITLE: Defining Tailwind CSS Custom Properties
DESCRIPTION: This snippet defines a comprehensive set of CSS custom properties (variables) used within Tailwind CSS. It includes definitions for various drop shadow sizes, blur levels, perspective values, aspect ratios, cubic-bezier easing functions, and animation properties, providing a consistent styling foundation.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/theme.mdx#_snippet_40

LANGUAGE: CSS
CODE:
```
--drop-shadow-sm: 0 1px 2px rgb(0 0 0 / 0.15);
--drop-shadow-md: 0 3px 3px rgb(0 0 0 / 0.12);
--drop-shadow-lg: 0 4px 4px rgb(0 0 0 / 0.15);
--drop-shadow-xl: 0 9px 7px rgb(0 0 0 / 0.1);
--drop-shadow-2xl: 0 25px 25px rgb(0 0 0 / 0.15);

--blur-xs: 4px;
--blur-sm: 8px;
--blur-md: 12px;
--blur-lg: 16px;
--blur-xl: 24px;
--blur-2xl: 40px;
--blur-3xl: 64px;

--perspective-dramatic: 100px;
--perspective-near: 300px;
--perspective-normal: 500px;
--perspective-midrange: 800px;
--perspective-distant: 1200px;

--aspect-video: 16 / 9;

--ease-in: cubic-bezier(0.4, 0, 1, 1);
--ease-out: cubic-bezier(0, 0, 0.2, 1);
--ease-in-out: cubic-bezier(0.4, 0, 0.2, 1);

--animate-spin: spin 1s linear infinite;
--animate-ping: ping 1s cubic-bezier(0, 0, 0.2, 1) infinite;
--animate-pulse: pulse 2s cubic-bezier(0.4, 0, 0.6, 1) infinite;
--animate-bounce: bounce 1s infinite;
```

----------------------------------------

TITLE: Applying Styles with Data Attribute Variant (Will Apply) in HTML
DESCRIPTION: This HTML snippet illustrates how Tailwind's `data-*` variant applies styles. The `p-8` utility will be applied because the `data-size` attribute on the div matches the `large` value specified in the `data-[size=large]:p-8` class.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/blog/tailwindcss-v3-2/index.mdx#_snippet_12

LANGUAGE: HTML
CODE:
```
<!-- Will apply -->
<div data-size="large" class="data-[size=large]:p-8">
  <!-- ... -->
</div>
```

----------------------------------------

TITLE: Applying Arbitrary CSS Properties with Modifiers in HTML
DESCRIPTION: This HTML snippet shows how to combine arbitrary CSS properties with interactive modifiers in Tailwind CSS. It applies a default `mask-type` and changes it on hover, highlighting the ability to control custom CSS properties based on element states.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/adding-custom-styles.mdx#_snippet_6

LANGUAGE: HTML
CODE:
```
<div class="[mask-type:luminance] hover:[mask-type:alpha]">
  <!-- ... -->
</div>
```

----------------------------------------

TITLE: Applying Basic Font Weights in JSX/React
DESCRIPTION: This JSX snippet demonstrates how to apply various Tailwind CSS font-weight utility classes (font-light, font-normal, font-medium, font-semibold, font-bold) to paragraph elements within a React component to control their visual weight. Each paragraph is accompanied by a label indicating the applied class, showcasing the effect of each utility.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/font-weight.mdx#_snippet_0

LANGUAGE: jsx
CODE:
```
<div className="flex flex-col gap-8">
  <div>
    <span className="mb-3 font-mono text-xs font-medium text-gray-500 dark:text-gray-400">font-light</span>
    <p className="text-lg font-light text-gray-900 dark:text-gray-200">
      The quick brown fox jumps over the lazy dog.
    </p>
  </div>
  <div>
    <span className="mb-3 font-mono text-xs font-medium text-gray-500 dark:text-gray-400">font-normal</span>
    <p className="text-lg font-normal text-gray-900 dark:text-gray-200">
      The quick brown fox jumps over the lazy dog.
    </p>
  </div>
  <div>
    <span className="mb-3 font-mono text-xs font-medium text-gray-500 dark:text-gray-400">font-medium</span>
    <p className="text-lg font-medium text-gray-900 dark:text-gray-200">
      The quick brown fox jumps over the lazy dog.
    </p>
  </div>
  <div>
    <span className="mb-3 font-mono text-xs font-medium text-gray-500 dark:text-gray-400">font-semibold</span>
    <p className="text-lg font-semibold text-gray-900 dark:text-gray-200">
      The quick brown fox jumps over the lazy dog.
    </p>
  </div>
  <div>
    <span className="mb-3 font-mono text-xs font-medium text-gray-500 dark:text-gray-400">font-bold</span>
    <p className="text-lg font-bold text-gray-900 dark:text-gray-200">
      The quick brown fox jumps over the lazy dog.
    </p>
  </div>
</div>
```

----------------------------------------

TITLE: Applying User-Driven Form Validation Styles with Tailwind CSS
DESCRIPTION: This HTML snippet demonstrates how to use the new `user-valid` and `user-invalid` variants in Tailwind CSS to apply border styles (green for valid, red for invalid) to input fields only after the user has interacted with them, preventing immediate invalid state display on page load. The `required` attribute ensures validation is active.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/blog/tailwindcss-v4-1/index.mdx#_snippet_29

LANGUAGE: HTML
CODE:
```
<input required class="border user-valid:border-green-500" />
<input required class="border user-invalid:border-red-500" />
```

----------------------------------------

TITLE: Applying Conditional Styles with ARIA Checked in HTML
DESCRIPTION: This HTML snippet demonstrates how to conditionally apply Tailwind CSS styles based on the `aria-checked` attribute. When `aria-checked` is `true`, the element's background color changes to blue-600, otherwise it remains gray-600. This allows for dynamic styling based on accessibility states.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/blog/tailwindcss-v3-2/index.mdx#_snippet_5

LANGUAGE: HTML
CODE:
```
<span class="bg-gray-600 aria-checked:bg-blue-600" aria-checked="true" role="checkbox">
  <!-- ... -->
</span>
```

----------------------------------------

TITLE: Implementing Multiselect Combobox in Headless UI (React)
DESCRIPTION: This snippet demonstrates how to enable multiselect functionality for the `Combobox` component in Headless UI. By adding the `multiple` prop and binding an array to the `value` prop, users can select multiple options. It requires `useState` for managing the array of selected items.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/blog/2022-05-23-headless-ui-v1-6-tailwind-ui-team-management/index.mdx#_snippet_0

LANGUAGE: jsx
CODE:
```
function MyCombobox({ items }) {
  const [selectedItems, setSelectedItems] = useState([]);

  return (
    <Combobox value={selectedItems} onChange={setSelectedItems} multiple>
      {selectedItems.length > 0 && (
        <ul>
          {selectedItems.map((item) => (
            <li key={item}>{item}</li>
          ))}
        </ul>
      )}
      <Combobox.Input />
      <Combobox.Options>
        {items.map((item) => (
          <Combobox.Option key={item} value={item}>
            {item}
          </Combobox.Option>
        ))}
      </Combobox.Options>
    </Combobox>
  );
}
```

----------------------------------------

TITLE: Targeting a Single Breakpoint (md only) - HTML
DESCRIPTION: This HTML snippet illustrates how to apply a utility class (`flex`) specifically for a single breakpoint. By combining the `md` responsive variant with the `max-lg` variant (`md:max-lg:flex`), the `display: flex` style is active only when the screen size is within the `md` breakpoint range and less than the `lg` breakpoint, effectively targeting only the `md` size.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/responsive-design.mdx#_snippet_8

LANGUAGE: HTML
CODE:
```
<div class="md:max-lg:flex">
  <!-- ... -->
</div>
```

----------------------------------------

TITLE: Defining Pink Oklch Color Palette in CSS
DESCRIPTION: This snippet defines a spectrum of pink color shades using CSS custom properties and the Oklch color function. These variables are instrumental in establishing a cohesive pink color scheme across a web project, allowing for flexible and consistent design application.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/colors.mdx#_snippet_24

LANGUAGE: CSS
CODE:
```
--color-pink-50: oklch(0.971 0.014 343.198);
--color-pink-100: oklch(0.948 0.028 342.258);
--color-pink-200: oklch(0.899 0.061 343.231);
--color-pink-300: oklch(0.823 0.12 346.018);
--color-pink-400: oklch(0.718 0.202 349.761);
--color-pink-500: oklch(0.656 0.241 354.308);
--color-pink-600: oklch(0.592 0.249 0.584);
--color-pink-700: oklch(0.525 0.223 3.958);
--color-pink-800: oklch(0.459 0.187 3.815);
--color-pink-900: oklch(0.408 0.153 2.432);
--color-pink-950: oklch(0.284 0.109 3.907);
```

----------------------------------------

TITLE: Applying Tailwind Classes in Custom CSS
DESCRIPTION: This CSS snippet shows how to apply Tailwind CSS utility classes within custom CSS rules using the `@apply` directive. It defines styles for `select2` components, demonstrating how to integrate Tailwind's design system into specific UI elements.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/blog/tailwindcss-v3-1/index.mdx#_snippet_3

LANGUAGE: CSS
CODE:
```
.select2-dropdown {
  @apply rounded-b-lg shadow-md;
}
.select2-search {
  @apply rounded border border-gray-300;
}
.select2-results__group {
  @apply text-lg font-bold text-gray-900;
}
/* ... */
```

----------------------------------------

TITLE: Implementing Container Queries with Tailwind CSS HTML
DESCRIPTION: This snippet shows how to use Tailwind CSS's container query variants to style an element based on the width of its parent container, rather than the viewport. It sets up a container (@container) and applies a flex direction change (@md:flex-row) when the container reaches a medium size.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/hover-focus-and-other-states.mdx#_snippet_49

LANGUAGE: html
CODE:
```
<div class="@container">
  <div class="flex flex-col @md:flex-row">
    <!-- ... -->
  </div>
</div>
```

----------------------------------------

TITLE: Applying Directional Styles with RTL/LTR Variants in HTML
DESCRIPTION: This snippet illustrates how to use `ltr:` and `rtl:` variants in Tailwind CSS to apply styles conditionally based on the text direction of the layout. For example, `ltr:ml-3 rtl:mr-3` ensures proper spacing whether the layout is left-to-right or right-to-left, crucial for multi-directional interfaces.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/hover-focus-and-other-states.mdx#_snippet_83

LANGUAGE: HTML
CODE:
```
<div class="group flex items-center">
  <img class="h-12 w-12 shrink-0 rounded-full" src="..." alt="" />
  <div class="ltr:ml-3 rtl:mr-3">
    <p class="text-gray-700 group-hover:text-gray-900 ...">...</p>
    <p class="text-gray-500 group-hover:text-gray-700 ...">...</p>
  </div>
</div>
```

----------------------------------------

TITLE: Applying Custom Container Query Variant in HTML
DESCRIPTION: This HTML snippet illustrates how to use a custom container query variant, `@8xl:flex-row`, defined in the Tailwind CSS theme. By applying `@container` to a parent `div`, its child elements can respond to the custom `8xl` breakpoint, changing their layout from `flex-col` to `flex-row` when the container reaches the specified size.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/responsive-design.mdx#_snippet_19

LANGUAGE: html
CODE:
```
<div class="@container">
  <div class="flex flex-col @8xl:flex-row">
    <!-- ... -->
  </div>
</div>
```

----------------------------------------

TITLE: Styling User-Invalid Inputs with :user-invalid in HTML
DESCRIPTION: This snippet illustrates how to apply a Tailwind CSS class to an input element when it is invalid and the user has interacted with it, using the `user-invalid` variant. This variant is useful for delaying error messages until the user has attempted to input data, applying a `border-red-500`.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/hover-focus-and-other-states.mdx#_snippet_257

LANGUAGE: HTML
CODE:
```
<input required class="border user-invalid:border-red-500" />
```

----------------------------------------

TITLE: Centering and Padding Tailwind Containers
DESCRIPTION: This HTML snippet demonstrates how to properly center a Tailwind CSS container and add horizontal padding. It uses the `mx-auto` utility for automatic horizontal margins and `px-4` for padding, addressing the default behavior of Tailwind's `container` utility.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/max-width.mdx#_snippet_36

LANGUAGE: html
CODE:
```
<!-- [!code classes:mx-auto,px-4] -->
<div class="container mx-auto px-4">
  <!-- ... -->
</div>
```

----------------------------------------

TITLE: Applying Dark Mode Styles in Tailwind CSS HTML
DESCRIPTION: This HTML snippet illustrates how to apply distinct styles for light and dark modes using Tailwind CSS. Utilities without a variant target light mode, while those prefixed with `dark:` provide specific overrides for dark mode, affecting background and text colors based on the user's `prefers-color-scheme` setting.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/hover-focus-and-other-states.mdx#_snippet_50

LANGUAGE: html
CODE:
```
<!-- [!code classes:dark:bg-gray-900] -->
<!-- [!code classes:dark:text-white] -->
<!-- [!code classes:dark:text-gray-400] -->
<div class="bg-white dark:bg-gray-900 ...">
  <!-- ... -->
  <h3 class="text-gray-900 dark:text-white ...">Writes upside-down</h3>
  <p class="text-gray-500 dark:text-gray-400 ...">
    The Zero Gravity Pen can be used to write in any orientation, including upside-down. It even works in outer space.
  </p>
</div>
```

----------------------------------------

TITLE: Credit Card Payment Option UI with Tailwind CSS
DESCRIPTION: This HTML structure defines the Credit Card payment option, including its SVG icon, label, and radio button input. It applies Tailwind CSS classes for consistent styling, hover effects, and checked states, mirroring the Apple Pay option for a unified user experience.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/hover-focus-and-other-states.mdx#_snippet_14

LANGUAGE: HTML
CODE:
```
<label
            htmlFor="credit-card"
            className="grid grid-cols-[24px_1fr_auto] items-center gap-6 rounded-lg p-4 ring-1 ring-transparent hover:bg-gray-100 has-checked:bg-indigo-50 has-checked:text-indigo-800 has-checked:ring-indigo-200 dark:hover:bg-white/5 dark:has-checked:bg-indigo-950 dark:has-checked:text-indigo-200 dark:has-checked:ring-indigo-900"
          >
            <svg className="w-8" viewBox="0 0 24 13" fill="none">
              <rect stroke="currentColor" x="0.5" y="0.5" width="23" height="11" rx="1.5" />
              <path
                fill="currentColor"
                d="M16.539 3.18591C16.0742 3.01652 15.5828 2.93152 15.088 2.93491C13.488 2.93491 12.358 3.74091 12.35 4.89791C12.34 5.74791 13.153 6.22691 13.768 6.51091C14.399 6.80291 14.61 6.98691 14.608 7.24791C14.604 7.64491 14.104 7.82491 13.639 7.82491C13 7.82491 12.651 7.73591 12.114 7.51291L11.915 7.41991L11.688 8.75191C12.077 8.91391 12.778 9.05291 13.502 9.06491C15.203 9.06491 16.315 8.26391 16.328 7.03291C16.342 6.35391 15.902 5.84091 14.976 5.41691C14.413 5.14191 14.064 4.95791 14.064 4.67891C14.064 4.43191 14.363 4.16791 14.988 4.16791C15.404 4.15785 15.8174 4.23589 16.201 4.39691L16.351 4.46391L16.578 3.17691L16.539 3.18591ZM20.691 3.04291H19.441C19.052 3.04291 18.759 3.14991 18.589 3.53591L16.185 8.98191H17.886L18.226 8.08891L20.302 8.09091C20.351 8.29991 20.501 8.98191 20.501 8.98191H22.001L20.691 3.04291ZM10.049 2.99291H11.67L10.656 8.93491H9.03705L10.049 2.99091V2.99291ZM5.93405 6.26791L6.10205 7.09291L7.68605 3.04291H9.40305L6.85205 8.97391H5.13905L3.73905 3.95191C3.71637 3.8691 3.66312 3.79798 3.59005 3.75291C3.08545 3.49225 2.55079 3.29444 1.99805 3.16391L2.02005 3.03891H4.62905C4.98305 3.05291 5.26805 3.16391 5.36305 3.54191L5.93305 6.27091L5.93405 6.26791ZM18.691 6.87391L19.337 5.21191C19.329 5.22991 19.47 4.86891 19.552 4.64591L19.663 5.15891L20.038 6.87291H18.69L18.691 6.87391Z"
              />
            </svg>
            Credit Card
            <input
              name="payment_method"
              id="credit-card"
              value="credit-card"
              type="radio"
              className="box-content h-1.5 w-1.5 appearance-none rounded-full border-[5px] border-white bg-white bg-clip-padding ring-1 ring-gray-950/20 outline-none checked:border-indigo-500 checked:ring-indigo-500"
            />
          </label>
```

----------------------------------------

TITLE: Compiling Tailwind CSS with New CLI and JIT Mode
DESCRIPTION: This command demonstrates the usage of the new high-performance Tailwind CLI. It compiles your CSS, outputs it to `dist/tailwind.css`, enables watch mode for automatic rebuilding on changes, activates Just-in-Time (JIT) mode for on-demand compilation, and purges unused CSS by scanning HTML files within the `src` directory.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/blog/tailwindcss-2-2/index.mdx#_snippet_1

LANGUAGE: sh
CODE:
```
npx tailwindcss -o dist/tailwind.css --watch --jit --purge="./src/**/*.html"
```

----------------------------------------

TITLE: Defining Container Query for @max-4xl (Width < 56rem) in CSS
DESCRIPTION: This snippet defines a container query associated with the @max-4xl breakpoint, applying styles when the container's width is less than 56 rem. It's used for responsive design based on the parent container's size rather than the viewport.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/hover-focus-and-other-states.mdx#_snippet_189

LANGUAGE: CSS
CODE:
```
@container (width < 56rem)
```

----------------------------------------

TITLE: Upgrading Tailwind CSS to Latest Version (Bash)
DESCRIPTION: This bash command demonstrates how to upgrade your Tailwind CSS installation to the latest version using npm. Running `npm install tailwindcss@latest` will update the `tailwindcss` package in your project, allowing you to access new features and improvements like those introduced in v2.1. This is a standard procedure for incremental upgrades.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/blog/tailwindcss-2-1/index.mdx#_snippet_5

LANGUAGE: bash
CODE:
```
npm install tailwindcss@latest
```

----------------------------------------

TITLE: Styling Checked Checkboxes/Radio Buttons with :checked in HTML
DESCRIPTION: This snippet shows how to apply a Tailwind CSS class to a checkbox or radio button when it is in a checked state using the `checked` variant. It allows for visual feedback, such as changing the background color to `bg-blue-500` when the input is selected.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/hover-focus-and-other-states.mdx#_snippet_249

LANGUAGE: HTML
CODE:
```
<input type="checkbox" class="appearance-none checked:bg-blue-500 ..." />
```

----------------------------------------

TITLE: Overriding Component Styles with Utility Classes in HTML
DESCRIPTION: This HTML snippet demonstrates how utility classes can override component styles. The `card` class applies the base component styles, while `rounded-none` overrides the `border-radius` defined in the `.card` component, resulting in square corners for the element.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/adding-custom-styles.mdx#_snippet_20

LANGUAGE: HTML
CODE:
```
<div class="card rounded-none">
  <!-- ... -->
</div>
```

----------------------------------------

TITLE: Hiding Elements Visually with sr-only (HTML)
DESCRIPTION: This snippet demonstrates the use of the `sr-only` utility class in Tailwind CSS. Applying `sr-only` to an element, such as a `<span>` tag, visually hides it from sighted users while ensuring it remains accessible to screen readers. This is useful for providing descriptive text for icons or other non-textual elements without cluttering the visual interface.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/display.mdx#_snippet_15

LANGUAGE: html
CODE:
```
<!-- [!code classes:sr-only] -->
<a href="#">
  <svg><!-- ... --></svg>
  <span class="sr-only">Settings</span>
</a>
```

----------------------------------------

TITLE: Ignoring Pointer Events in HTML with Tailwind CSS
DESCRIPTION: This HTML example demonstrates the usage of `pointer-events-auto` and `pointer-events-none` classes. The first `div` with `pointer-events-auto` allows its child SVG icon to be interactive, while the second `div` with `pointer-events-none` makes its child SVG icon ignore pointer events, effectively making it unclickable. This is useful for controlling user interaction with specific UI elements.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/pointer-events.mdx#_snippet_1

LANGUAGE: html
CODE:
```
<!-- [!code classes:pointer-events-none,pointer-events-auto] -->
<div class="relative ...">
  <div class="pointer-events-auto absolute ...">
    <svg class="absolute h-5 w-5 text-gray-400">
      <!-- ... -->
    </svg>
  </div>
  <input type="text" placeholder="Search" class="..." />
</div>

<div class="relative ...">
  <div class="pointer-events-none absolute ...">
    <svg class="absolute h-5 w-5 text-gray-400">
      <!-- ... -->
    </svg>
  </div>
  <input type="text" placeholder="Search" class="..." />
</div>
```

----------------------------------------

TITLE: Setting a 16/9 Aspect Ratio for a Video (HTML)
DESCRIPTION: This snippet illustrates how to apply a 16/9 aspect ratio to an `<iframe>` element, commonly used for videos, using the `aspect-video` utility class in Tailwind CSS. This ensures the video maintains the standard widescreen proportion.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/aspect-ratio.mdx#_snippet_1

LANGUAGE: HTML
CODE:
```
<!-- [!code classes:aspect-video] -->
<iframe class="aspect-video ..." src="https://www.youtube.com/embed/dQw4w9WgXcQ"></iframe>
```

----------------------------------------

TITLE: Implementing Conic and Radial Gradients with Tailwind CSS
DESCRIPTION: This example showcases the new `bg-conic-*` and `bg-radial-*` utilities in Tailwind CSS for creating conic and radial gradients. These utilities integrate seamlessly with existing `from-*`, `via-*`, and `to-*` color stops, and support modifiers for color interpolation and arbitrary values to control gradient position and other details.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/blog/tailwindcss-v4/index.mdx#_snippet_22

LANGUAGE: HTML
CODE:
```
<div class="size-24 rounded-full bg-conic/[in_hsl_longer_hue] from-red-600 to-red-600"></div>
<div class="size-24 rounded-full bg-radial-[at_25%_25%] from-white to-zinc-900 to-75%"></div>
```

----------------------------------------

TITLE: Stacking Arbitrary and Built-in Variants in Svelte/HTML
DESCRIPTION: This example illustrates stacking an arbitrary variant `[&.is-dragging]` with a built-in `active` variant. The class `[&.is-dragging]:active:cursor-grabbing` applies `cursor: grabbing` when the element has `is-dragging` and is active. The generated CSS shows the nested selectors.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/hover-focus-and-other-states.mdx#_snippet_94

LANGUAGE: Svelte
CODE:
```
<!-- [!code classes:[&.is-dragging]:active:cursor-grabbing] -->
<ul role="list">
  {#each items as item}
    <li class="[&.is-dragging]:active:cursor-grabbing">{item}</li>
  {/each}
</ul>
```

LANGUAGE: CSS
CODE:
```
.\\[\&\\.is-dragging\\]\\:active\\:cursor-grabbing {
  &.is-dragging {
    &:active {
      cursor: grabbing;
    }
  }
}
```

----------------------------------------

TITLE: Applying Colored Box Shadows in HTML
DESCRIPTION: This HTML snippet illustrates the application of colored box shadows using Tailwind CSS utility classes directly in HTML. It uses `shadow-lg` for shadow size and `shadow-cyan-500/50`, `shadow-blue-500/50`, and `shadow-indigo-500/50` to define the color and opacity of the shadows for different buttons. This demonstrates the concise syntax for adding colored shadows.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/blog/tailwindcss-v3/index.mdx#_snippet_3

LANGUAGE: HTML
CODE:
```
<!-- [!code word:shadow-lg shadow-cyan-500/50] -->
<button class="bg-cyan-500 shadow-lg shadow-cyan-500/50 ...">Subscribe</button>
<!-- [!code word:shadow-lg shadow-blue-500/50] -->
<button class="bg-blue-500 shadow-lg shadow-blue-500/50 ...">Subscribe</button>
<!-- [!code word:shadow-lg shadow-indigo-500/50] -->
<button class="bg-indigo-500 shadow-lg shadow-indigo-500/50 ...">Subscribe</button>
```

----------------------------------------

TITLE: Reversing Child Order with Space Utilities in Tailwind CSS
DESCRIPTION: This example demonstrates how to correctly apply spacing when child elements are rendered in reverse order using `flex-row-reverse`. The `space-x-reverse` utility ensures that the `space-x-4` margin is applied to the correct side of each element, maintaining proper visual separation.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/margin.mdx#_snippet_8

LANGUAGE: html
CODE:
```
<!-- [!code classes:flex-row-reverse,space-x-4,space-x-reverse] -->
<div class="flex flex-row-reverse space-x-4 space-x-reverse ...">
  <div>01</div>
  <div>02</div>
  <div>03</div>
</div>
```

----------------------------------------

TITLE: Overriding Tailwind CSS Default Breakpoint
DESCRIPTION: This CSS snippet shows how to override a default Tailwind CSS theme variable, specifically `--breakpoint-sm`. By redefining it to `30rem` within the `@theme` block, the `sm:` variant will now trigger at this new viewport size instead of its default, allowing for custom responsive design breakpoints.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/theme.mdx#_snippet_12

LANGUAGE: css
CODE:
```
@import "tailwindcss";

@theme {
  --breakpoint-sm: 30rem;
}
```

----------------------------------------

TITLE: Replacing Deprecated ring-opacity-* with Tailwind CSS Opacity Modifiers
DESCRIPTION: This snippet demonstrates replacing the deprecated `ring-opacity-*` utility with the modern Tailwind CSS opacity modifier syntax, such as `ring-black/50`, for controlling ring color opacity.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/upgrade-guide.mdx#_snippet_9

LANGUAGE: Tailwind CSS
CODE:
```
ring-opacity-*
```

LANGUAGE: Tailwind CSS
CODE:
```
ring-black/50
```

----------------------------------------

TITLE: Defining Tailwind CSS Font Weight Variables
DESCRIPTION: This snippet defines CSS custom properties for various font weights, mapping descriptive names (e.g., thin, normal, bold) to numerical values. These variables are used to apply consistent font thickness across text elements.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/theme.mdx#_snippet_35

LANGUAGE: CSS
CODE:
```
--font-weight-thin: 100;
--font-weight-extralight: 200;
--font-weight-light: 300;
--font-weight-normal: 400;
--font-weight-medium: 500;
--font-weight-semibold: 600;
--font-weight-bold: 700;
--font-weight-extrabold: 800;
--font-weight-black: 900;
```

----------------------------------------

TITLE: Configuring Tailwind CSS with CSS-based Theme
DESCRIPTION: This CSS snippet demonstrates a future concept for configuring Tailwind CSS directly within a CSS file, replacing JavaScript-based configuration. It shows how to import Tailwind CSS, include custom fonts, and define custom color and font-family variables within a `:theme` block, simplifying the setup process.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/blog/2023-07-18-tailwind-connect-2023-recap/index.mdx#_snippet_7

LANGUAGE: css
CODE:
```
@import "tailwindcss";
@import "./fonts" layer(base);

:theme {
  --colors-neon-pink: oklch(71.7% 0.25 360);
  --colors-neon-lime: oklch(91.5% 0.258 129);
  --colors-neon-cyan: oklch(91.3% 0.139 195.8);

  --font-family-sans: "Inter", sans-serif;
  --font-family-display: "Satoshi", sans-serif;
}
```

----------------------------------------

TITLE: Using Implicit `in-*` Variants in HTML
DESCRIPTION: This HTML snippet demonstrates the `in-*` variant, which applies styles based on the state of *any* parent element, without needing an explicit `group` class. Specifically, `in-focus:opacity-100` makes the child element fully opaque when any of its ancestors are in a focused state. It also shows a comparison with the `group-focus` variant.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/hover-focus-and-other-states.mdx#_snippet_30

LANGUAGE: HTML
CODE:
```
<div tabindex="0">
  <div class="opacity-50 in-focus:opacity-100">
    <!-- ... -->
  </div>
</div>
```

----------------------------------------

TITLE: Styling User-Validated Inputs with :user-valid in HTML
DESCRIPTION: This snippet demonstrates how to apply a Tailwind CSS class to an input element when it is valid and the user has interacted with it, using the `user-valid` variant. This provides more precise feedback than `:valid`, only showing success after user engagement, e.g., with a `border-green-500`.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/hover-focus-and-other-states.mdx#_snippet_256

LANGUAGE: HTML
CODE:
```
<input required class="border user-valid:border-green-500" />
```

----------------------------------------

TITLE: Applying Conditional Styles with aria-checked in HTML
DESCRIPTION: This snippet demonstrates how to apply conditional styling using the `aria-checked` variant in Tailwind CSS. When the `aria-checked` attribute is set to `true`, the `bg-sky-700` class is applied, overriding the default `bg-gray-600` background. This allows for dynamic styling based on the accessibility state of an element.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/hover-focus-and-other-states.mdx#_snippet_72

LANGUAGE: HTML
CODE:
```
<!-- [!code classes:aria-checked:bg-sky-700] -->
<div aria-checked="true" class="bg-gray-600 aria-checked:bg-sky-700">
  <!-- ... -->
</div>
```

----------------------------------------

TITLE: Enabling Dark Mode in Tailwind CSS Configuration
DESCRIPTION: This JavaScript snippet demonstrates how to enable dark mode in Tailwind CSS by setting the `darkMode` property to `media` in `tailwind.config.js`. This configuration allows Tailwind to automatically apply dark mode styles based on the user's operating system preference.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/blog/tailwindcss-v2/index.mdx#_snippet_3

LANGUAGE: js
CODE:
```
module.exports = {
  darkMode: "media",
  // ...
};
```

----------------------------------------

TITLE: Styling Headless UI Listbox Options with Tailwind CSS Data Attributes (JSX)
DESCRIPTION: This snippet demonstrates how to style Headless UI `Listbox.Option` components using the new `data-headlessui-state` attributes and the `@headlessui/tailwindcss` plugin. It applies conditional Tailwind CSS classes based on the `ui-active` and `ui-selected` states, allowing for CSS-only styling without render props. The `key` and `value` props are used for component identification and data binding, while `CheckIcon` is conditionally displayed based on selection.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/blog/2022-09-09-new-personal-website-heroicons-2-headless-ui-v17/index.mdx#_snippet_3

LANGUAGE: jsx
CODE:
```
<Listbox.Option
  key={person.id}
  value={person}
  className="ui-active:bg-blue-500 ui-active:text-white ui-not-active:bg-white ui-not-active:text-black"
>
  <CheckIcon className="ui-selected:block hidden" />
  {person.name}
</Listbox.Option>
```

----------------------------------------

TITLE: Applying Hover Styles to Nested Elements with Tailwind CSS
DESCRIPTION: This HTML snippet illustrates the use of advanced Tailwind CSS arbitrary variants to apply hover styles to a deeply nested element within a list. It targets the first paragraph inside the second list item on hover, changing its text color to indigo.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/blog/tailwindcss-v3-1/index.mdx#_snippet_26

LANGUAGE: HTML
CODE:
```
<!-- [!code word:hover\:[&>li\:nth-child(2)>div>p\:first-child\]\:text-indigo-500] -->
<ul
  role="list"
  class="space-y-4 [&>*]:rounded-lg [&>*]:bg-white [&>*]:p-4 [&>*]:shadow hover:[&>li:nth-child(2)>div>p:first-child]:text-indigo-500"
>
  <!-- ... -->
  <li class="flex">
    <img class="h-10 w-10 rounded-full" src="..." alt="" />
    <div class="ml-3 overflow-hidden">
      <p class="text-sm font-medium text-slate-900">Floyd Miles</p>
      <p class="truncate text-sm text-slate-500">floyd.miles@example.com</p>
    </div>
  </li>
  <!-- ... -->
</ul>
```

----------------------------------------

TITLE: Applying Conic Gradients with Tailwind CSS (HTML/JSX)
DESCRIPTION: This snippet demonstrates how to apply conic gradients using Tailwind CSS. It includes examples of a default conic gradient (`bg-conic`), a conic gradient rotated by 180 degrees (`bg-conic-180`), and a decreasing conic gradient (`bg-conic/decreasing`). Color stops are defined using `from-*`, `via-*`, and `to-*` utilities.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/background-image.mdx#_snippet_4

LANGUAGE: JavaScript
CODE:
```
<div className="flex justify-around p-8 sm:p-12">
  <div className="size-18 rounded-full bg-conic from-blue-600 to-sky-400 to-50% sm:size-24"></div>
  <div className="size-18 rounded-full bg-conic-180 from-indigo-600 via-indigo-50 to-indigo-600 sm:size-24"></div>
  <div className="size-18 rounded-full bg-conic/decreasing from-violet-700 via-lime-300 to-violet-700 sm:size-24"></div>
</div>
```

LANGUAGE: HTML
CODE:
```
<div class="size-24 rounded-full bg-conic from-blue-600 to-sky-400 to-50%"></div>
<div class="size-24 rounded-full bg-conic-180 from-indigo-600 via-indigo-50 to-indigo-600"></div>
<div class="size-24 rounded-full bg-conic/decreasing from-violet-700 via-lime-300 to-violet-700"></div>
```

----------------------------------------

TITLE: Implementing Horizontal Scroll Snap with Tailwind CSS HTML
DESCRIPTION: This HTML snippet demonstrates how to create a horizontally scrollable container with scroll snapping behavior using Tailwind CSS utilities. The `snap-x` class is applied to the parent container to enable horizontal snapping, and `snap-center` is applied to child elements (images) to ensure they snap to the center of the scroll area. This setup provides a smooth, controlled scrolling experience for image galleries or carousels.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/blog/tailwindcss-v3/index.mdx#_snippet_4

LANGUAGE: HTML
CODE:
```
<!-- [!code word:snap-x] -->
<!-- [!code word:snap-center] -->
<div class="snap-x ...">
  <div class="snap-center ...">
    <img src="https://images.unsplash.com/photo-1604999565976-8913ad2ddb7c?auto=format&fit=crop&w=320&h=160&q=80" />
  </div>
  <div class="snap-center ...">
    <img src="https://images.unsplash.com/photo-1540206351-d6465b3ac5c1?auto=format&fit=crop&w=320&h=160&q=80" />
  </div>
  <div class="snap-center ...">
    <img src="https://images.unsplash.com/photo-1622890806166-111d7f6c7c97?auto=format&fit=crop&w=320&h=160&q=80" />
  </div>
  <div class="snap-center ...">
    <img src="https://images.unsplash.com/photo-1590523277543-a94d2e4eb00b?auto=format&fit=crop&w=320&h=160&q=80" />
  </div>
  <div class="snap-center ...">
    <img src="https://images.unsplash.com/photo-1575424909138-46b05e5919ec?auto=format&fit=crop&w=320&h=160&q=80" />
  </div>
  <div class="snap-center ...">
    <img src="https://images.unsplash.com/photo-1559333086-b0a56225a93c?auto=format&fit=crop&w=320&h=160&q=80" />
  </div>
</div>
```

----------------------------------------

TITLE: Defining Calendar Grid Cells with Tailwind CSS in JSX
DESCRIPTION: This snippet illustrates a basic `div` element used to construct the grid structure of the calendar. It applies Tailwind CSS classes for column and row placement (`col-start`, `row-start`), borders (`border-r`, `border-b`), and dark mode styling (`dark:border-gray-200/5`). These divs typically serve as visual separators or background cells in the grid.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/overflow.mdx#_snippet_17

LANGUAGE: JSX
CODE:
```
<div className="col-start-2 row-start-14 border-r border-b border-gray-100 dark:border-gray-200/5"></div>
```

----------------------------------------

TITLE: Using Native Aspect Ratio with Tailwind CSS
DESCRIPTION: This snippet demonstrates how to apply the native `aspect-ratio` CSS property using Tailwind CSS utility classes, specifically `aspect-video`. This allows elements, such as iframes, to maintain a consistent aspect ratio (e.g., 16:9 for video) as their width changes, ensuring proper scaling and layout.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/blog/tailwindcss-v3/index.mdx#_snippet_9

LANGUAGE: HTML
CODE:
```
<!-- [!code word:aspect-video] -->
<iframe class="aspect-video w-full ..." src="https://www.youtube.com/..."></iframe>
```

----------------------------------------

TITLE: Applying bg-cover Utility for Filling Container
DESCRIPTION: This snippet demonstrates the `bg-cover` utility, which scales a background image to fill the entire background layer, potentially cropping the image. It includes both a JSX example for React components and a plain HTML example.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/background-size.mdx#_snippet_1

LANGUAGE: jsx
CODE:
```
<div className="relative mx-auto flex w-56 items-center justify-center overflow-hidden rounded-lg sm:w-96">
      <div className="absolute inset-0">
        <Stripes border className="h-full rounded-lg" />
      </div>
      <div className="relative z-10 h-48 w-full bg-[url(https://images.unsplash.com/photo-1554629947-334ff61d85dc?ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&ixlib=rb-1.2.1&auto=format&fit=crop&w=512&h=640&q=80)] bg-cover bg-center bg-no-repeat"></div>
    </div>
```

LANGUAGE: html
CODE:
```
<!-- [!code classes:bg-cover] -->
<div class="bg-[url(/img/mountains.jpg)] bg-cover bg-center"></div>
```

----------------------------------------

TITLE: Combining Box Shadow with Focus Ring Utilities in HTML
DESCRIPTION: This HTML snippet demonstrates how Tailwind CSS `ring` utilities automatically combine with regular `box-shadow` utilities. The `shadow-sm` and `focus:ring-2` classes will both render together, allowing for complex visual effects where an element has both a static shadow and a dynamic focus ring.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/blog/tailwindcss-v2/index.mdx#_snippet_11

LANGUAGE: html
CODE:
```
<button class="shadow-sm focus:ring-2 ...">
  <!-- Both the shadow and ring will render together -->
</button>
```

----------------------------------------

TITLE: Using Logical `start-0` Property with Tailwind CSS (HTML)
DESCRIPTION: This HTML snippet demonstrates the `start-0` Tailwind CSS utility, which corresponds to the `inset-inline-start` logical property. It shows how `start-0` positions an element at the beginning of the inline direction, adapting automatically for both left-to-right (`ltr`) and right-to-left (`rtl`) text directions without needing separate `left-0` or `right-0` classes.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/top-right-bottom-left.mdx#_snippet_5

LANGUAGE: html
CODE:
```
<!-- [!code classes:start-0] -->
<div dir="ltr">
  <div class="relative size-32 ...">
    <div class="absolute start-0 top-0 size-14 ..."></div>
  </div>
  <div>
    <div dir="rtl">
      <div class="relative size-32 ...">
        <div class="absolute start-0 top-0 size-14 ..."></div>
      </div>
      <div></div>
    </div>
  </div>
</div>
```

----------------------------------------

TITLE: Disabling Transitions for Reduced Motion Preference in HTML
DESCRIPTION: This HTML snippet demonstrates the use of the `motion-reduce` variant to conditionally disable CSS transitions. When a user has the `prefers-reduced-motion` media feature enabled, the `motion-reduce:transition-none` class will apply, preventing potentially problematic motion for sensitive users. This enhances accessibility by respecting user preferences.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/blog/tailwindcss-1-6/index.mdx#_snippet_2

LANGUAGE: html
CODE:
```
<div class="transition duration-150 ease-in-out motion-reduce:transition-none ... ..."></div>
```

----------------------------------------

TITLE: Hiding Focus Outline in JSX with Tailwind CSS
DESCRIPTION: This JSX snippet demonstrates how to use the `focus:outline-hidden` utility class on an input field within a React component. This class hides the default browser outline when the element is focused, while ensuring the outline remains visible in forced colors mode for accessibility.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/outline-style.mdx#_snippet_3

LANGUAGE: JavaScript
CODE:
```
<input
      type="text"
      placeholder="Your full name"
      className="mx-auto block w-full max-w-xs border-b-2 border-gray-300 bg-gray-50 px-2 py-2 text-sm text-gray-800 focus:border-indigo-600 focus:outline-hidden dark:border-white/15 dark:bg-white/5 dark:text-white dark:focus:border-indigo-500"
    />
```

----------------------------------------

TITLE: Centering Flex Items with Tailwind CSS HTML
DESCRIPTION: This example shows the `justify-center` utility, which centers flex items along the main axis of their container. It's useful for horizontally centering a group of elements within a flexbox layout.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/justify-content.mdx#_snippet_1

LANGUAGE: HTML
CODE:
```
<!-- [!code filename:justify-center] -->
<!-- [!code classes:justify-center] -->
<div class="flex justify-center ...">
  <div>01</div>
  <div>02</div>
  <div>03</div>
  <div>04</div>
</div>
```

----------------------------------------

TITLE: Applying Nested Variants in Custom CSS
DESCRIPTION: This CSS snippet illustrates how to apply multiple, nested Tailwind variants using the `@variant` directive. It defines a default background, then applies a `dark` variant, and within that, a `hover` variant, setting the background to black only when both dark mode and hover states are active.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/adding-custom-styles.mdx#_snippet_24

LANGUAGE: CSS
CODE:
```
.my-element {
  background: white;

  @variant dark {
    @variant hover {
      background: black;
    }
  }
}
```

----------------------------------------

TITLE: Applying Horizontal Scroll with Tailwind CSS HTML
DESCRIPTION: This HTML snippet illustrates the core Tailwind CSS utility `overflow-x-scroll` applied to a `div` element. This class enables horizontal scrolling for the element's content, making it suitable for layouts where content might exceed the viewport width. The `<!-- ... -->` indicates that additional content would be placed inside this div to demonstrate the scrolling effect.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/overflow.mdx#_snippet_9

LANGUAGE: HTML
CODE:
```
<!-- [!code classes:overflow-x-scroll] -->
<div class="overflow-x-scroll ...">
  <!-- ... -->
</div>
```

----------------------------------------

TITLE: Applying Transitions to HTML Elements with Headless UI's Transition Component and React
DESCRIPTION: This snippet shows how to use the `<Transition>` component from Headless UI to apply transitions to any regular HTML element or component. It leverages the new data attribute API, specifically `data-[closed]:opacity-0`, to control the fade-in and fade-out effects based on the `show` prop.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/blog/2024-06-21-headless-ui-v2-1/index.mdx#_snippet_2

LANGUAGE: jsx
CODE:
```
import { Transition } from "@headlessui/react";
import { useState } from "react";

function Example() {
  const [isShowing, setIsShowing] = useState(false);

  return (
    <>
      <button onClick={() => setIsShowing((isShowing) => !isShowing)}>Toggle</button>
      // [!code highlight:6]
      <Transition show={isShowing}>
        <div className="transition duration-300 data-[closed]:opacity-0">I will fade in and out</div>
      </Transition>
    </>
  );
}
```

----------------------------------------

TITLE: Setting Fixed Maximum Height with Tailwind CSS
DESCRIPTION: This snippet demonstrates how to use `max-h-<number>` utilities to set a fixed maximum height for elements based on Tailwind's default spacing scale. It shows various examples like `max-h-80`, `max-h-64`, and `max-h-24` to illustrate different height constraints.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/max-height.mdx#_snippet_0

LANGUAGE: HTML
CODE:
```
<!-- [!code classes:max-h-80,max-h-64,max-h-48,max-h-40,max-h-32,max-h-24,max-h-full] -->
<div class="h-96 ...">
  <div class="h-full max-h-80 ...">max-h-80</div>
  <div class="h-full max-h-64 ...">max-h-64</div>
  <div class="h-full max-h-48 ...">max-h-48</div>
  <div class="h-full max-h-40 ...">max-h-40</div>
  <div class="h-full max-h-32 ...">max-h-32</div>
  <div class="h-full max-h-24 ...">max-h-24</div>
</div>
```

----------------------------------------

TITLE: Setting Max Width to 100% Dynamic Viewport Width in Tailwind CSS
DESCRIPTION: Sets the maximum width to 100% of the dynamic viewport width (dvw). This unit accounts for dynamic toolbars in mobile browsers.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/max-width.mdx#_snippet_18

LANGUAGE: CSS
CODE:
```
max-width: 100dvw;
```

----------------------------------------

TITLE: Defining Custom Base Styles with Tailwind's @layer Directive
DESCRIPTION: Illustrates the use of Tailwind CSS's @layer base directive to add custom default styles for specific HTML elements, such as h1 and h2, ensuring they are integrated into Tailwind's base layer for consistent styling.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/adding-custom-styles.mdx#_snippet_18

LANGUAGE: CSS
CODE:
```
@layer base {
  h1 {
    font-size: var(--text-2xl);
  }

  h2 {
    font-size: var(--text-xl);
  }
}
```

----------------------------------------

TITLE: Styling with Named Peer Variants in HTML
DESCRIPTION: This snippet demonstrates how to use `peer/{name}` and `peer-checked/{name}` classes in HTML to style elements based on the state of a specific named peer. It uses radio buttons to show how a label and a descriptive div can react to the checked state of a uniquely identified input.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/hover-focus-and-other-states.mdx#_snippet_33

LANGUAGE: HTML
CODE:
```
<fieldset>
  <legend>Published status</legend>

  <input id="draft" class="peer/draft" type="radio" name="status" checked />
  <label for="draft" class="peer-checked/draft:text-sky-500">Draft</label>

  <input id="published" class="peer/published" type="radio" name="status" />
  <label for="published" class="peer-checked/published:text-sky-500">Published</label>
  <div class="hidden peer-checked/draft:block">Drafts are only visible to administrators.</div>
  <div class="hidden peer-checked/published:block">Your post will be publicly visible on your site.</div>
</fieldset>
```

----------------------------------------

TITLE: Defining Violet Oklch Color Palette in CSS
DESCRIPTION: This snippet outlines various violet color shades as CSS custom properties, leveraging the Oklch color space. These definitions enable developers to easily access and apply a consistent violet palette throughout their stylesheets, promoting design uniformity.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/colors.mdx#_snippet_21

LANGUAGE: CSS
CODE:
```
--color-violet-50: oklch(0.969 0.016 293.756);
--color-violet-100: oklch(0.943 0.029 294.588);
--color-violet-200: oklch(0.894 0.057 293.283);
--color-violet-300: oklch(0.811 0.111 293.571);
--color-violet-400: oklch(0.702 0.183 293.541);
--color-violet-500: oklch(0.606 0.25 292.717);
--color-violet-600: oklch(0.541 0.281 293.009);
--color-violet-700: oklch(0.491 0.27 292.581);
--color-violet-800: oklch(0.432 0.232 292.759);
--color-violet-900: oklch(0.38 0.189 293.745);
--color-violet-950: oklch(0.283 0.141 291.089);
```

----------------------------------------

TITLE: Removing Text Decoration with Tailwind CSS
DESCRIPTION: Use the `no-underline` utility to remove any existing text decoration from an element. This utility applies the CSS property `text-decoration-line: none;`.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/text-decoration-line.mdx#_snippet_3

LANGUAGE: html
CODE:
```
<!-- [!code classes:no-underline] -->
<p class="no-underline">The quick brown fox...</p>
```

----------------------------------------

TITLE: Exposing Theme Values as Native CSS Variables
DESCRIPTION: This CSS output snippet shows how Tailwind CSS makes all defined theme values available as native CSS variables within the :root selector in the compiled dist/main.css file. This allows developers to directly reference these theme values in any custom CSS without needing special Tailwind functions.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/blog/tailwindcss-v4-alpha/index.mdx#_snippet_8

LANGUAGE: css
CODE:
```
/* [!code filename:dist/main.css] */
:root {
  --color-gray-50: #f8fafc;
  --color-gray-100: #f1f5f9;
  --color-gray-200: #e2e8f0;
  /* ... */
  --color-green-800: #3f6212;
  --color-green-900: #365314;
  --color-green-950: #1a2e05;
}
```

----------------------------------------

TITLE: Implementing Basic Container Queries in HTML with Tailwind CSS
DESCRIPTION: This snippet demonstrates the basic syntax for applying container queries using the @tailwindcss/container-queries plugin. It shows how to define a container element with @container and then apply responsive styles within it using @lg:flex, which applies flex when the container reaches the lg breakpoint. This differentiates container queries from standard media queries.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/blog/tailwindcss-v3-2/index.mdx#_snippet_32

LANGUAGE: HTML
CODE:
```
<div class="@container">
  <div class="block @lg:flex">
    <!-- ... -->
  </div>
</div>
```

----------------------------------------

TITLE: Applying Hover Background Color with Tailwind CSS (HTML)
DESCRIPTION: This snippet demonstrates how to apply a different background color on hover using Tailwind CSS utility classes. The hover:bg-fuchsia-500 class changes the button's background to fuchsia when the user hovers over it, while bg-indigo-500 sets the default background.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/background-color.mdx#_snippet_2

LANGUAGE: html
CODE:
```
<!-- [!code classes:hover:bg-fuchsia-500] -->
<button class="bg-indigo-500 hover:bg-fuchsia-500 ...">Save changes</button>
```

----------------------------------------

TITLE: Applying Text Color on Hover in HTML
DESCRIPTION: This HTML snippet illustrates how to implement hover-specific text color changes using Tailwind CSS classes such as `hover:text-blue-600` and `dark:hover:text-blue-400` on an anchor element, enhancing user interaction.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/color.mdx#_snippet_6

LANGUAGE: html
CODE:
```
<p class="...">
  Oh I gotta get on that
  <a class="underline hover:text-blue-600 dark:hover:text-blue-400" href="https://en.wikipedia.org/wiki/Internet">internet</a>,
  I'm late on everything!
</p>
```

----------------------------------------

TITLE: Conditionally Applying Tailwind Classes in React to Avoid Conflicts
DESCRIPTION: Provides an example in React/JSX showing how to use conditional logic (a ternary operator) to apply only one of two potentially conflicting Tailwind classes (`grid` or `flex`) based on a component prop (`gridLayout`). This is a common pattern in component-based frameworks to prevent style conflicts.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/styling-with-utility-classes.mdx#_snippet_33

LANGUAGE: jsx
CODE:
```
// [!code filename:example.jsx]
// [!code word:gridLayout\ \?\ "grid"\ \:\ "flex"]
export function Example({ gridLayout }) {
  return <div className={gridLayout ? "grid" : "flex"}>{/* ... */}</div>;
}
```

----------------------------------------

TITLE: Generating Multiple Classes with Brace Expansion in Tailwind CSS
DESCRIPTION: Illustrates using brace expansion within `@source inline()` to generate a range of utility classes and their variants. This example generates various `bg-red` shades (50, 100-900, 950) and their `hover` variants, providing a concise way to safelist multiple related classes.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/blog/tailwindcss-v4-1/index.mdx#_snippet_24

LANGUAGE: css
CODE:
```
@import "tailwindcss";
@source inline("{hover:,}bg-red-{50,{100..900..100},950}");
```

LANGUAGE: css
CODE:
```
.bg-red-50 {
  background-color: var(--color-red-50);
}
.bg-red-100 {
  background-color: var(--color-red-100);
}
.bg-red-200 {
  background-color: var(--color-red-200);
}

/* ... */

.bg-red-800 {
  background-color: var(--color-red-800);
}
.bg-red-900 {
  background-color: var(--color-red-900);
}
.bg-red-950 {
  background-color: var(--color-red-950);
}
@media (hover: hover) {
  .hover\:bg-red-50:hover {
    background-color: var(--color-red-50);
  }

  /* ... */

  .hover\:bg-red-950:hover {
    background-color: var(--color-red-950);
  }
}
```

----------------------------------------

TITLE: Using flex-1 for Basic Flex Item Growth and Shrinkage in HTML
DESCRIPTION: Use `flex-<number>` utilities like `flex-1` to allow a flex item to grow and shrink as needed, ignoring its initial size. This example demonstrates how `flex-1` makes items expand to fill available space within a flex container.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/flex.mdx#_snippet_0

LANGUAGE: html
CODE:
```
<!-- [!code word:flex-1] -->
<div class="flex">
  <div class="w-14 flex-none ...">01</div>
  <div class="w-64 flex-1 ...">02</div>
  <div class="w-32 flex-1 ...">03</div>
</div>
```

----------------------------------------

TITLE: Installing Tailwind CSS v4 Alpha for CLI - npm
DESCRIPTION: This npm command installs the alpha version of Tailwind CSS and its CLI package, providing the necessary tools for direct command-line compilation of your CSS.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/blog/tailwindcss-v4-alpha/index.mdx#_snippet_17

LANGUAGE: sh
CODE:
```
npm install tailwindcss@next @tailwindcss/cli@next
```

----------------------------------------

TITLE: Using Theme Variables in Arbitrary HTML Values
DESCRIPTION: This example illustrates how to incorporate Tailwind CSS theme variables into arbitrary values within HTML, particularly in conjunction with the `calc()` CSS function. It shows how to dynamically adjust properties, such as creating a concentric border radius by subtracting a pixel value from a theme variable.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/theme.mdx#_snippet_25

LANGUAGE: html
CODE:
```
<div class="relative rounded-xl">
  <div class="absolute inset-px rounded-[calc(var(--radius-xl)-1px)]">
    <!-- ... -->
  </div>
  <!-- ... -->
</div>
```

----------------------------------------

TITLE: Applying Arbitrary Values for Positioning in HTML
DESCRIPTION: This HTML snippet illustrates the use of Tailwind CSS's square bracket notation to apply an arbitrary `top` value. This method allows for precise, pixel-perfect positioning, similar to inline styles, while still enabling the use of Tailwind's utility class system.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/adding-custom-styles.mdx#_snippet_1

LANGUAGE: HTML
CODE:
```
<div class="top-[117px]">
  <!-- ... -->
</div>
```

----------------------------------------

TITLE: Importing Tailwind CSS in main.css
DESCRIPTION: This snippet demonstrates the basic method of including Tailwind CSS into a project using a standard CSS @import statement in the main.css file. This is the initial step to integrate the framework.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/blog/tailwindcss-v4-alpha/index.mdx#_snippet_3

LANGUAGE: css
CODE:
```
/* [!code filename:main.css] */
@import "tailwindcss";
```

----------------------------------------

TITLE: Creating a User List with Headless UI Dropdown Menu in React
DESCRIPTION: This React component displays a list of 'people' data, where each person's entry includes an image, name, email, and an interactive dropdown menu. It utilizes `@headlessui/react` for the `Menu` and `Transition` components to manage dropdown state and animations, and `@heroicons/react/solid` for the vertical dots icon. The `classNames` utility (assumed to be imported or defined elsewhere) is used for conditional Tailwind CSS styling based on menu item active state.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/blog/building-react-and-vue-support-for-tailwind-ui/index.mdx#_snippet_5

LANGUAGE: javascript
CODE:
```
import { Menu, Transition } from "@headlessui/react";
import { DotsVerticalIcon } from "@heroicons/react/solid";
import { Fragment } from "react";

const people = [
  {
    name: "Calvin Hawkins",
    email: "calvin.hawkins@example.com",
    image:
      "https://images.unsplash.com/photo-1491528323818-fdd1faba62cc?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=facearea&facepad=2&w=256&h=256&q=80",
  },
  {
    name: "Kristen Ramos",
    email: "kristen.ramos@example.com",
    image:
      "https://images.unsplash.com/photo-1550525811-e5869dd03032?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=facearea&facepad=2&w=256&h=256&q=80",
  },
  {
    name: "Ted Fox",
    email: "ted.fox@example.com",
    image:
      "https://images.unsplash.com/photo-1500648767791-00dcc994a43e?ixlib=rb-1.2.1&ixid=eyJhcHBfaWQiOjEyMDd9&auto=format&fit=facearea&facepad=2&w=256&h=256&q=80",
  },
];

export default function Example() {
  return (
    <ul className="divide-y divide-gray-200">
      {people.map((person) => (
        <li key={person.email} className="flex py-4">
          <img className="h-10 w-10 rounded-full" src={person.image.src} alt="" />
          <div className="ml-3">
            <p className="text-sm font-medium text-gray-900">{person.name}</p>
            <p className="text-sm text-gray-500">{person.email}</p>
          </div>
          <Menu as="div" className="relative ml-3 inline-block text-left">
            {({ open }) => (
              <>
                <div>
                  <Menu.Button className="flex items-center rounded-full bg-gray-100 text-gray-400 hover:text-gray-600 focus:ring-2 focus:ring-indigo-500 focus:ring-offset-2 focus:ring-offset-gray-100 focus:outline-none">
                    <span className="sr-only">Open options</span>
                    <DotsVerticalIcon className="h-5 w-5" aria-hidden="true" />
                  </Menu.Button>
                </div>

                <Transition
                  show={open}
                  as={Fragment}
                  enter="transition ease-out duration-100"
                  enterFrom="transform opacity-0 scale-95"
                  enterTo="transform opacity-100 scale-100"
                  leave="transition ease-in duration-75"
                  leaveFrom="transform opacity-100 scale-100"
                  leaveTo="transform opacity-0 scale-95"
                >
                  <Menu.Items
                    static
                    className="ring-opacity-5 absolute right-0 mt-2 w-56 origin-top-right rounded-md bg-white shadow-lg ring-1 ring-black focus:outline-none"
                  >
                    <div className="py-1">
                      <Menu.Item>
                        {({ active }) => (
                          <a
                            href="#"
                            className={classNames(
                              active ? "bg-gray-100 text-gray-900" : "text-gray-700",
                              "block px-4 py-2 text-sm",
                            )}
                          >
                            View details
                          </a>
                        )}
                      </Menu.Item>
                      <Menu.Item>
                        {({ active }) => (
                          <a
                            href="#"
                            className={classNames(
                              active ? "bg-gray-100 text-gray-900" : "text-gray-700",
                              "block px-4 py-2 text-sm",
                            )}
                          >
                            Send message
                          </a>
                        )}
                      </Menu.Item>
                    </div>
                  </Menu.Items>
                </Transition>
              </>
            )}
          </Menu>
        </li>
      ))}
    </ul>
  );
}
```

----------------------------------------

TITLE: Restoring Default Placeholder Color in Tailwind CSS v4 (CSS)
DESCRIPTION: This CSS snippet provides base styles to restore the Tailwind CSS v3 default placeholder color behavior in v4. By setting `color: var(--color-gray-400)` for `input::placeholder` and `textarea::placeholder` within `@layer base`, the placeholder text will use the configured `gray-400` color.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/upgrade-guide.mdx#_snippet_30

LANGUAGE: CSS
CODE:
```
@layer base {
  input::placeholder,
  textarea::placeholder {
    color: var(--color-gray-400);
  }
}
```

----------------------------------------

TITLE: Applying Basic Border Radius Utilities in HTML
DESCRIPTION: This HTML snippet demonstrates the application of basic Tailwind CSS border-radius utility classes such as `rounded-sm`, `rounded-md`, `rounded-lg`, and `rounded-xl` to `div` elements. Each class applies a predefined border radius size, visually showcasing the effect.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/border-radius.mdx#_snippet_1

LANGUAGE: HTML
CODE:
```
<!-- [!code classes:rounded-sm,rounded-md,rounded-lg,rounded-xl] -->
<div class="rounded-sm ..."></div>
<div class="rounded-md ..."></div>
<div class="rounded-lg ..."></div>
<div class="rounded-xl ..."></div>
```

----------------------------------------

TITLE: Applying Tailwind CSS `overflow-x-auto` Utility in HTML
DESCRIPTION: This HTML snippet illustrates the fundamental usage of the `overflow-x-auto` utility class from Tailwind CSS. Applying this class to a `div` element ensures that its content will scroll horizontally if it overflows the element's boundaries, providing a simple and effective way to manage horizontal content overflow.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/overflow.mdx#_snippet_5

LANGUAGE: html
CODE:
```
<!-- [!code classes:overflow-x-auto] -->
<div class="overflow-x-auto ...">
  <!-- ... -->
</div>
```

----------------------------------------

TITLE: Setting Gradient Color Stops with Tailwind CSS (HTML/JSX)
DESCRIPTION: This snippet demonstrates how to define the colors for gradient stops using Tailwind CSS utilities. It shows the use of `from-*`, `via-*`, and `to-*` classes to specify the starting, middle, and ending colors of a gradient, respectively. This is applicable to all gradient types.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/background-image.mdx#_snippet_5

LANGUAGE: JavaScript
CODE:
```
<div className="mx-5">
  <div className="relative h-[3.625rem]">
    <div className="absolute top-0 left-px -ml-4 flex h-12 flex-col items-center">
      <svg viewBox="0 0 32 34" className="w-8 flex-none fill-indigo-500 drop-shadow">
        <use href="#gradient-color-stop" />
      </svg>
      <div className="mt-2 h-2 w-0.5 bg-gray-900/30 dark:bg-white/30"></div>
    </div>
    <div className="absolute top-0 left-1/2 -ml-4 flex h-12 flex-col items-center">
      <svg viewBox="0 0 32 34" className="w-8 flex-none fill-purple-500 drop-shadow">
        <use href="#gradient-color-stop" />
      </svg>
      <div className="mt-2 h-2 w-0.5 bg-gray-900/30 dark:bg-white/30"></div>
    </div>
    <div className="absolute top-0 right-px -mr-4 flex h-12 flex-col items-center">
      <svg viewBox="0 0 32 34" className="w-8 flex-none fill-pink-500 drop-shadow">
        <use href="#gradient-color-stop" />
      </svg>
      <div className="mt-2 h-2 w-0.5 bg-gray-900/30 dark:bg-white/30"></div>
    </div>
  </div>
  <div className="h-10 rounded-lg bg-gradient-to-r from-indigo-500 via-purple-500 to-pink-500"></div>
</div>
```

LANGUAGE: HTML
CODE:
```
<div class="bg-gradient-to-r from-indigo-500 via-purple-500 to-pink-500 ..."></div>
```

----------------------------------------

TITLE: Defining `max-2xl` Media Query Breakpoint in CSS
DESCRIPTION: This CSS media query defines the `max-2xl` breakpoint, applying styles when the viewport width is less than 96rem (1536px). It targets screens smaller than the `2xl` breakpoint, useful for specific adjustments on very large displays.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/hover-focus-and-other-states.mdx#_snippet_164

LANGUAGE: CSS
CODE:
```
@media (width < 96rem)
```

----------------------------------------

TITLE: Using Headless UI Listbox as Uncontrolled Component with defaultValue (React)
DESCRIPTION: This snippet illustrates how to use Headless UI's Listbox component as an uncontrolled component in React by providing a 'defaultValue' instead of a 'value'. This approach is beneficial for traditional HTML forms or APIs that collect state via FormData, simplifying state management. It requires the @headlessui/react dependency.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/blog/2022-09-09-new-personal-website-heroicons-2-headless-ui-v17/index.mdx#_snippet_2

LANGUAGE: jsx
CODE:
```
import { Listbox } from "@headlessui/react";

const people = [
  { id: 1, name: "Durward Reynolds" },
  { id: 2, name: "Kenton Towne" },
  { id: 3, name: "Therese Wunsch" },
  { id: 4, name: "Benedict Kessler" },
  { id: 5, name: "Katelyn Rohan" }
];

function Example() {
  return (
    <form action="/projects/1/assignee" method="post">
      <Listbox name="assignee" defaultValue={people[0]}>
        <Listbox.Button>{({ value }) => value.name}</Listbox.Button>
        <Listbox.Options>
          {people.map((person) => (
            <Listbox.Option key={person.id} value={person}>
              {person.name}
            </Listbox.Option>
          ))}
        </Listbox.Options>
      </Listbox>
      <button>Submit</button>
    </form>
  );
}
```

----------------------------------------

TITLE: Defining Object Fit Utilities in Tailwind CSS API Table
DESCRIPTION: This JSX snippet defines the `ApiTable` component, which is used to display the available `object-fit` utility classes in Tailwind CSS. It lists each utility class (e.g., `object-contain`, `object-cover`) and its corresponding CSS property value (e.g., `object-fit: contain;`). This table serves as a reference for developers to quickly understand the mapping between Tailwind classes and standard CSS.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/object-fit.mdx#_snippet_0

LANGUAGE: JavaScript
CODE:
```
<ApiTable
  rows={[
    ["object-contain", "object-fit: contain;"],
    ["object-cover", "object-fit: cover;"],
    ["object-fill", "object-fit: fill;"],
    ["object-none", "object-fit: none;"],
    ["object-scale-down", "object-fit: scale-down;"],
  ]}
/>
```

----------------------------------------

TITLE: Applying Styles with Data Attribute Variant (Will Not Apply) in HTML
DESCRIPTION: This HTML snippet demonstrates when Tailwind's `data-*` variant will not apply styles. The `p-8` utility will not be applied because the `data-size` attribute on the div (`medium`) does not match the `large` value specified in the `data-[size=large]:p-8` class.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/blog/tailwindcss-v3-2/index.mdx#_snippet_13

LANGUAGE: HTML
CODE:
```
<!-- Will not apply -->
<div data-size="medium" class="data-[size=large]:p-8">
  <!-- ... -->
</div>
```

----------------------------------------

TITLE: Balancing Headline Text with `text-balance` in HTML
DESCRIPTION: This snippet demonstrates the `text-balance` utility, which leverages `text-wrap: balance` CSS property to automatically optimize the wrapping of headline text. It helps prevent awkward line breaks and ensures a visually balanced appearance for headings, improving readability without manual adjustments.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/blog/tailwindcss-v3-4/index.mdx#_snippet_7

LANGUAGE: HTML
CODE:
```
<article>
  <h3 class="text-balance ...">Beloved Manhattan soup stand closes<h3>
  <p>New Yorkers are facing the winter chill...</p>
</article>
```

----------------------------------------

TITLE: Applying `outline-offset` in HTML
DESCRIPTION: This HTML snippet demonstrates the direct application of `outline-offset` utility classes (`outline-offset-0`, `outline-offset-2`, `outline-offset-4`) to button elements, controlling the outline's distance from the element's border.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/outline-offset.mdx#_snippet_2

LANGUAGE: HTML
CODE:
```
<!-- [!code classes:outline-offset-0,outline-offset-2,outline-offset-4] -->
<button class="outline-2 outline-offset-0 ...">Button A</button>
<button class="outline-2 outline-offset-2 ...">Button B</button>
<button class="outline-2 outline-offset-4 ...">Button C</button>
```

----------------------------------------

TITLE: Using @tailwindui/react Transition Component in React
DESCRIPTION: This example illustrates the usage of the `<Transition>` component from `@tailwindui/react` to implement utility-first enter/leave animations in a React application. It leverages React's `useState` hook to manage the `isOpen` state, which controls the `show` prop of the `<Transition>` component. The `enter`, `enterFrom`, `enterTo`, `leave`, `leaveFrom`, and `leaveTo` props accept Tailwind CSS utility classes to define the transition behavior, mirroring the Vue.js approach.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/blog/utility-friendly-transitions-with-tailwindui-react/index.mdx#_snippet_1

LANGUAGE: JSX
CODE:
```
import { Transition } from "@tailwindui/react";
import { useState } from "react";

function MyComponent() {
  const [isOpen, setIsOpen] = useState(false);

  return (
    <div>
      <button onClick={() => setIsOpen(!isOpen)}>Toggle</button>
      <Transition
        show={isOpen}
        enter="transition-opacity duration-75"
        enterFrom="opacity-0"
        enterTo="opacity-100"
        leave="transition-opacity duration-150"
        leaveFrom="opacity-100"
        leaveTo="opacity-0"
      >
        {/* Will fade in and out */}
      </Transition>
    </div>
  );
}
```

----------------------------------------

TITLE: Applying Tailwind CSS Background Color Utilities in HTML
DESCRIPTION: This HTML snippet demonstrates the application of various Tailwind CSS background color utility classes ('bg-sky-50' through 'bg-sky-950') to 'div' elements. Each 'div' is styled with a different shade from the Sky color palette, illustrating the 11 steps of a default Tailwind color scale for visual representation.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/colors.mdx#_snippet_2

LANGUAGE: HTML
CODE:
```
<!-- [!code classes:bg-sky-50,bg-sky-100,bg-sky-200,bg-sky-300,bg-sky-400,bg-sky-500,bg-sky-600,bg-sky-700,bg-sky-800,bg-sky-900,bg-sky-950] -->
<div>
  <div class="bg-sky-50"></div>
  <div class="bg-sky-100"></div>
  <div class="bg-sky-200"></div>
  <div class="bg-sky-300"></div>
  <div class="bg-sky-400"></div>
  <div class="bg-sky-500"></div>
  <div class="bg-sky-600"></div>
  <div class="bg-sky-700"></div>
  <div class="bg-sky-800"></div>
  <div class="bg-sky-900"></div>
  <div class="bg-sky-950"></div>
</div>
```

----------------------------------------

TITLE: Default Tailwind CSS Color Palette in OKLCH
DESCRIPTION: This CSS snippet defines the default color palette for Tailwind CSS using CSS custom properties within an `@theme` block. Each color (e.g., red, orange, amber) is provided with 11 shades (from 50 to 950), specified using the oklch color function, which offers perceptual uniformity and better color manipulation. These variables can be used throughout a CSS project to maintain a consistent design system.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/colors.mdx#_snippet_17

LANGUAGE: CSS
CODE:
```
/* [!code filename:CSS] */
@theme {
  --color-red-50: oklch(0.971 0.013 17.38);
  --color-red-100: oklch(0.936 0.032 17.717);
  --color-red-200: oklch(0.885 0.062 18.334);
  --color-red-300: oklch(0.808 0.114 19.571);
  --color-red-400: oklch(0.704 0.191 22.216);
  --color-red-500: oklch(0.637 0.237 25.331);
  --color-red-600: oklch(0.577 0.245 27.325);
  --color-red-700: oklch(0.505 0.213 27.518);
  --color-red-800: oklch(0.444 0.177 26.899);
  --color-red-900: oklch(0.396 0.141 25.723);
  --color-red-
```

----------------------------------------

TITLE: Importing Tailwind CSS in Main CSS File
DESCRIPTION: This CSS snippet imports the Tailwind CSS framework into your main stylesheet. It's a crucial step to make Tailwind's utilities available for use in your project after installation and configuration.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/blog/tailwindcss-v4-alpha/index.mdx#_snippet_13

LANGUAGE: CSS
CODE:
```
@import "tailwindcss";
```

----------------------------------------

TITLE: Installing Tailwind CSS with CLI
DESCRIPTION: This command installs the latest versions of Tailwind CSS and its command-line interface (CLI) tool using npm. This setup is ideal for projects that use the Tailwind CLI directly for processing CSS.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/blog/tailwindcss-v4-1/index.mdx#_snippet_0

LANGUAGE: sh
CODE:
```
npm install tailwindcss@latest @tailwindcss/cli@latest
```

----------------------------------------

TITLE: Styling Interactive States with Tailwind CSS
DESCRIPTION: Provides an example of applying styles for hover, focus, and active states using respective Tailwind variants (`hover:`, `focus:`, `active:`). This allows for comprehensive interactive styling directly within the HTML.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/hover-focus-and-other-states.mdx#_snippet_4

LANGUAGE: HTML
CODE:
```
<!-- [!code classes:hover:bg-violet-600,focus:outline-2,focus:outline-offset-2,focus:outline-violet-500,active:bg-violet-700] -->
<!-- prettier-ignore -->
<button class="bg-violet-500 hover:bg-violet-600 focus:outline-2 focus:outline-offset-2 focus:outline-violet-500 active:bg-violet-700 ...">
  Save changes
</button>
```

LANGUAGE: JSX
CODE:
```
<div className="grid place-items-center">
      <button
        type="button"
        className="rounded-full bg-violet-500 px-5 py-2 text-sm leading-5 font-semibold text-white hover:bg-violet-600 focus:outline-2 focus:outline-offset-2 focus:outline-violet-500 active:bg-violet-700"
      >
        Save changes
      </button>
    </div>
```

----------------------------------------

TITLE: Simplified CSS Variable Color Configuration in Tailwind
DESCRIPTION: This JavaScript snippet for `tailwind.config.js` demonstrates the simplified method for defining colors using CSS variables. Instead of a function, a format string with an `<alpha-value>` placeholder is used, which Tailwind automatically replaces with the correct opacity, reducing boilerplate.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/blog/tailwindcss-v3-1/index.mdx#_snippet_9

LANGUAGE: JavaScript
CODE:
```
module.exports = {
  theme: {
    colors: {
      primary: "rgb(var(--color-primary) / <alpha-value>)",
      secondary: "rgb(var(--color-secondary) / <alpha-value>)",
      // ...
    },
  },
};
```

----------------------------------------

TITLE: Applying Dark Mode with Responsive Breakpoints in HTML
DESCRIPTION: This HTML snippet shows how to apply dark mode styles conditionally based on responsive breakpoints in Tailwind CSS. The `lg:dark:bg-black` class ensures that the background color changes to black in dark mode only on large screens and above, allowing for responsive dark mode designs.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/blog/tailwindcss-v2/index.mdx#_snippet_6

LANGUAGE: html
CODE:
```
<div class="lg:bg-white lg:dark:bg-black ...">
  <!-- ... -->
</div>
```

----------------------------------------

TITLE: Setting Horizontal and Vertical Border Widths with Tailwind CSS (HTML)
DESCRIPTION: This snippet demonstrates how to apply border widths simultaneously to horizontal or vertical sides of an element using Tailwind CSS. The `border-x-4` utility applies a border to both the left and right sides, while `border-y-4` applies it to both the top and bottom sides. This provides a convenient way to style opposing borders.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/border-width.mdx#_snippet_2

LANGUAGE: HTML
CODE:
```
<!-- [!code classes:border-x-4,border-y-4] -->
<div class="border-x-4 border-indigo-500 ..."></div>
<div class="border-y-4 border-indigo-500 ..."></div>
```

----------------------------------------

TITLE: Applying Italic Style with Tailwind CSS (HTML)
DESCRIPTION: This HTML snippet illustrates the use of the `italic` utility class to make text italic. It shows a basic paragraph element with the `italic` class applied, indicating how to achieve italic text styling directly in HTML using Tailwind CSS.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/font-style.mdx#_snippet_1

LANGUAGE: HTML
CODE:
```
<!-- [!code classes:italic] -->
<p class="italic ...">The quick brown fox ...</p>
```

----------------------------------------

TITLE: Centering Text with Tailwind CSS
DESCRIPTION: This snippet shows how to center text within an HTML element using the `text-center` utility class provided by Tailwind CSS. Apply this class to any block-level element to horizontally center its text content.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/text-align.mdx#_snippet_2

LANGUAGE: html
CODE:
```
<!-- [!code classes:text-center] -->
<p class="text-center">So I started to walk into the water...</p>
```

----------------------------------------

TITLE: Applying Conditional Scrollbars with overflow-auto in React/JSX
DESCRIPTION: This snippet demonstrates how to use the `overflow-auto` utility in a React/JSX component to conditionally add scrollbars to a container. It creates a list of contacts within a fixed-height `div`, allowing vertical scrolling if content exceeds the height. The `h-72` class sets a fixed height, making the `overflow-auto` class effective for managing content overflow.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/overflow.mdx#_snippet_2

LANGUAGE: jsx
CODE:
```
<div className="relative mx-auto flex h-72 max-w-sm flex-col divide-y divide-gray-200 overflow-auto rounded-xl bg-white shadow-lg ring-1 ring-black/5 dark:divide-gray-200/5 dark:bg-gray-800">
      <div className="flex items-center gap-4 p-4">
        <img
          className="h-12 w-12 rounded-full"
          src="https://images.unsplash.com/photo-1501196354995-cbb51c65aaea?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=facearea&facepad=4&w=256&h=256&q=80"
        />
        <div className="flex flex-col">
          <strong className="text-sm font-medium text-gray-900 dark:text-gray-200">Andrew Alfred</strong>
          <span className="text-sm font-medium text-gray-500 dark:text-gray-400">Technical advisor</span>
        </div>
      </div>
      <div className="flex items-center gap-4 p-4">
        <img
          className="h-12 w-12 rounded-full"
          src="https://images.unsplash.com/photo-1531123897727-8f129e1688ce?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=facearea&facepad=4&w=256&h=256&q=80"
        />
        <div className="flex flex-col">
          <strong className="text-sm font-medium text-gray-900 dark:text-gray-200">Debra Houston</strong>
          <span className="text-sm font-medium text-gray-500 dark:text-gray-400">Analyst</span>
        </div>
      </div>
      <div className="flex items-center gap-4 p-4">
        <img
          className="h-12 w-12 rounded-full"
          src="https://images.unsplash.com/photo-1517841905240-472988babdf9?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=facearea&facepad=4&w=256&h=256&q=80"
        />
        <div className="flex flex-col">
          <strong className="text-sm font-medium text-gray-900 dark:text-gray-200">Jane White</strong>
          <span className="text-sm font-medium text-gray-500 dark:text-gray-400">Director, Marketing</span>
        </div>
      </div>
      <div className="flex items-center gap-4 p-4">
        <img
          className="h-12 w-12 rounded-full"
          src="https://images.unsplash.com/photo-1531427186611-ecfd6d936c79?ixlib=rb-1.2.1&ixid=MnwxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8&auto=format&fit=facearea&facepad=4&w=256&h=256&q=80"
        />
        <div className="flex flex-col">
          <strong className="text-sm font-medium text-gray-900 dark:text-gray-200">Ray Flint</strong>
          <span className="text-sm font-medium text-gray-500 dark:text-gray-400">Technical Advisor</span>
        </div>
      </div>
    </div>
```

----------------------------------------

TITLE: Right Aligning Text with Tailwind CSS
DESCRIPTION: This snippet illustrates how to right align text within an HTML element using the `text-right` utility class from Tailwind CSS. Attach this class to the desired element to align its contained text to the right margin.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/text-align.mdx#_snippet_1

LANGUAGE: html
CODE:
```
<!-- [!code classes:text-right] -->
<p class="text-right">So I started to walk into the water...</p>
```

----------------------------------------

TITLE: Applying Hardware Acceleration with transform-gpu (HTML)
DESCRIPTION: This snippet demonstrates how to apply hardware acceleration to an HTML element using the `transform-gpu` utility class in Tailwind CSS. This forces the browser to render the transformation using the GPU, which can improve performance for certain transitions. The `transform-cpu` utility can be used to revert to CPU rendering if needed.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/transform.mdx#_snippet_0

LANGUAGE: HTML
CODE:
```
<div class="scale-150 transform-gpu">\n  <!-- ... -->\n</div>
```

----------------------------------------

TITLE: Enabling Dark Mode for Typography with Tailwind CSS
DESCRIPTION: This HTML snippet demonstrates how to apply dark mode styles to an article's typography. By adding `dark:prose-invert` to the `article` element, the typography colors automatically adapt to a dark theme when the `dark` class is active on the body.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/blog/tailwindcss-typography-v0-5/index.mdx#_snippet_1

LANGUAGE: html
CODE:
```
<body class="bg-white dark:bg-gray-900">
  <article class="prose dark:prose-invert">{{ markdown }}</article>
</body>
```

----------------------------------------

TITLE: Setting Flex Basis with Percentage Fractions in HTML
DESCRIPTION: Shows how to apply `basis-<fraction>` utilities like `basis-1/3` and `basis-2/3` to set the initial size of flex items using percentage values. This provides a straightforward way to create proportional layouts within a flex container.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/flex-basis.mdx#_snippet_2

LANGUAGE: html
CODE:
```
<div class="flex flex-row">
  <div class="basis-1/3">01</div>
  <div class="basis-2/3">02</div>
</div>
```

----------------------------------------

TITLE: Handling Fractions with Ratio Data Type in Tailwind CSS
DESCRIPTION: This example illustrates how to handle fractional values using the CSS `ratio` data type with `--value()`. This signals Tailwind to treat the value and modifier as a single unit, enabling utilities like `aspect-square`, `aspect-3/4`, and `aspect-[7/9]` for `aspect-ratio`.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/adding-custom-styles.mdx#_snippet_41

LANGUAGE: CSS
CODE:
```
@utility aspect-* {
  /* prettier-ignore */
  aspect-ratio: --value(--aspect-ratio-*, ratio, [ratio]);
}
```

----------------------------------------

TITLE: Justifying Items to the End with Tailwind CSS
DESCRIPTION: This snippet demonstrates the `justify-end` utility in Tailwind CSS, which aligns flex items to the end of the container's main axis. It ensures that content is pushed towards the right (for a row-direction flex container) or bottom (for a column-direction flex container).
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/justify-content.mdx#_snippet_3

LANGUAGE: html
CODE:
```
<div class="flex justify-end ...">
  <div>01</div>
  <div>02</div>
  <div>03</div>
  <div>03</div>
</div>
```

----------------------------------------

TITLE: Demonstrating `peer` Limitation: Styling Previous Siblings in HTML
DESCRIPTION: This HTML snippet illustrates a common pitfall: the `peer` class and its variants can only target *subsequent* siblings in the DOM. Here, `peer` is on the `input`, but `peer-invalid:text-red-500` is on a *previous* sibling `span`. This configuration will not work as intended because CSS's subsequent-sibling combinator does not allow styling of preceding elements.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/hover-focus-and-other-states.mdx#_snippet_32

LANGUAGE: html
CODE:
```
<!-- [!code classes:peer-invalid:text-red-500] -->
<!-- [!code classes:peer] -->
<label>
  <span class="peer-invalid:text-red-500 ...">Email</span>
  <input type="email" class="peer ..." />
</label>
```

----------------------------------------

TITLE: Allowing Flex Item Wrapping with flex-wrap in HTML
DESCRIPTION: This snippet illustrates the `flex-wrap` utility in Tailwind CSS, which allows flex items to wrap onto new lines when the container's width is insufficient to accommodate them on a single line. This is the default wrapping behavior for flex containers, enabling responsive layouts.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/flex-wrap.mdx#_snippet_1

LANGUAGE: HTML
CODE:
```
<!-- [!code classes:flex-wrap] -->
<div class="flex flex-wrap">
  <div>01</div>
  <div>02</div>
  <div>03</div>
</div>
```

----------------------------------------

TITLE: Understanding Specificity with Tailwind CSS Child Selectors
DESCRIPTION: This snippet demonstrates a common pitfall when using Tailwind CSS's `*` variant: direct utility classes on child elements cannot override styles applied via the `*` variant on the parent. This is due to the higher specificity of the generated child selector, meaning `*:bg-sky-50` on the `<ul>` will take precedence over `bg-red-50` on the `<li>`.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/hover-focus-and-other-states.mdx#_snippet_90

LANGUAGE: html
CODE:
```
<!-- [!code classes:*:bg-sky-50] -->
<!-- [!code classes:bg-red-50] -->
<ul class="*:bg-sky-50 ...">
  <li class="bg-red-50 ...">Sales</li>
  <li>Marketing</li>
  <li>SEO</li>
  <!-- ... -->
</ul>
```

----------------------------------------

TITLE: Applying Dark Mode Variant in Custom CSS
DESCRIPTION: This CSS snippet demonstrates how to use the `@variant dark` directive to apply specific styles when the dark mode preference is active. It sets the background of `.my-element` to white by default and overrides it to black when the `dark` variant is active.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/adding-custom-styles.mdx#_snippet_22

LANGUAGE: CSS
CODE:
```
.my-element {
  background: white;

  @variant dark {
    background: black;
  }
}
```

----------------------------------------

TITLE: Exporting Page Metadata in TSX
DESCRIPTION: This snippet exports `title` and `description` constants, which define the page's title and a brief explanation of its content. These exports are typically used by a page rendering system to set document titles and provide meta descriptions for SEO or internal documentation.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/mask-composite.mdx#_snippet_1

LANGUAGE: TypeScript
CODE:
```
export const title = "mask-composite";
export const description = "Utilities for controlling how multiple masks are combined together.";
```

----------------------------------------

TITLE: Defining Page Metadata for Documentation in TypeScript
DESCRIPTION: Exports `title` and `description` constants, which serve as metadata for the documentation page. These values typically populate the page's title and meta description, aiding in navigation and search engine optimization.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/object-position.mdx#_snippet_1

LANGUAGE: TypeScript
CODE:
```
export const title = "object-position";
export const description =
  "Utilities for controlling how a replaced element's content should be positioned within its container.";
```

----------------------------------------

TITLE: Creating Inline Hyperlink in JSX Text
DESCRIPTION: This JSX snippet demonstrates embedding a hyperlink within a paragraph using the Next.js Link component. It allows users to navigate to the Tailwind UI website directly from the descriptive text, enhancing readability and user experience.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/blog/tailwind-ui-ecommerce/index.mdx#_snippet_3

LANGUAGE: JSX
CODE:
```
<Link href="https://tailwindui.com">Tailwind UI</Link>
```

----------------------------------------

TITLE: Using @config for Client Stylesheet (CSS)
DESCRIPTION: This CSS snippet illustrates how to use the `@config` directive to link a client-facing stylesheet to its specific Tailwind CSS configuration file. It imports Tailwind's core layers, customized by `tailwind.client.config.js`, allowing for separate styling contexts within a single project.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/blog/tailwindcss-v3-2/index.mdx#_snippet_2

LANGUAGE: css
CODE:
```
@config "./tailwind.client.config.js";
@tailwind base;
@tailwind components;
@tailwind utilities;
```

----------------------------------------

TITLE: Implementing Subgrid Layout with Tailwind CSS HTML
DESCRIPTION: This snippet demonstrates how to use the `grid-cols-subgrid` utility in Tailwind CSS to allow a child element to inherit the grid columns from its parent. This enables precise placement of the child's own children within the parent's grid structure, facilitating complex and aligned layouts.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/blog/tailwindcss-v3-4/index.mdx#_snippet_9

LANGUAGE: HTML
CODE:
```
<div class="grid grid-cols-4 gap-4">
  <!-- ... -->
  <!-- [!code word:grid-cols-subgrid] -->
  <div class="col-span-3 grid grid-cols-subgrid gap-4">
    <div class="col-start-2">06</div>
  </div>
  <!-- ... -->
</div>
```

----------------------------------------

TITLE: Applying Dark Mode Styles with Tailwind CSS in HTML
DESCRIPTION: This HTML snippet illustrates the application of Tailwind CSS `dark:` variants to conditionally style elements based on the active theme. It showcases how `dark:bg-gray-800`, `dark:text-white`, and `dark:text-gray-400` are used to change background and text colors in dark mode, providing a concise example of responsive theming.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/colors.mdx#_snippet_8

LANGUAGE: html
CODE:
```
<!-- [!code word:dark\:bg-gray-800] -->
<!-- prettier-ignore -->
<div class="bg-white dark:bg-gray-800 rounded-lg px-6 py-8 ring shadow-xl ring-gray-900/5">
  <div>
    <span class="inline-flex items-center justify-center rounded-md bg-indigo-500 p-2 shadow-lg">
      <svg class="h-6 w-6 stroke-white" ...>
        <!-- ... -->
      </svg>
    </span>
  </div>
  <!-- prettier-ignore -->
  <!-- [!code word:dark\:text-white] -->
  <h3 class="text-gray-900 dark:text-white mt-5 text-base font-medium tracking-tight ">Writes upside-down</h3>
  <!-- prettier-ignore -->
  <!-- [!code word:dark\:text-gray-400] -->
  <p class="text-gray-500 dark:text-gray-400 mt-2 text-sm ">
    The Zero Gravity Pen can be used to write in any orientation, including upside-down. It even works in outer space.
  </p>
</div>
```

----------------------------------------

TITLE: Setting Font Size and Line Height with Tailwind CSS (HTML)
DESCRIPTION: This HTML snippet illustrates the direct application of Tailwind CSS utility classes such as `text-sm/6`, `text-sm/7`, and `text-sm/8` to paragraph elements. These classes combine font size (`text-sm`) with specific line-height values, providing a concise way to control text appearance in standard HTML.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/font-size.mdx#_snippet_4

LANGUAGE: html
CODE:
```
<!-- [!code classes:text-sm/6,text-sm/7,text-sm/8] -->
<p class="text-sm/6 ...">So I started to walk into the water...</p>
<p class="text-sm/7 ...">So I started to walk into the water...</p>
<p class="text-sm/8 ...">So I started to walk into the water...</p>
```

----------------------------------------

TITLE: Applying Multiple Filters - HTML - Tailwind CSS
DESCRIPTION: Demonstrates applying multiple Tailwind CSS filter utility classes (`blur-sm` and `grayscale`) to a single HTML element. These classes compose together to apply both effects simultaneously.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/styling-with-utility-classes.mdx#_snippet_14

LANGUAGE: HTML
CODE:
```
<div class="blur-sm grayscale">
  <!-- ... -->
</div>
```

----------------------------------------

TITLE: Defining Minimum Width Container Query for 7XL Breakpoint in CSS
DESCRIPTION: This CSS snippet defines a container query that applies styles when the container's width is greater than or equal to 80rem (1280px). It corresponds to the `@7xl` container breakpoint in Tailwind CSS, enabling responsive design based on parent container dimensions.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/hover-focus-and-other-states.mdx#_snippet_178

LANGUAGE: CSS
CODE:
```
@container (width >= 80rem)
```

----------------------------------------

TITLE: Implementing Sidebar Layout with Catalyst (JSX)
DESCRIPTION: This snippet demonstrates how to use the `SidebarLayout` component from Catalyst to structure an application with a collapsible sidebar for mobile screens. It requires `SidebarLayout`, `Navbar`, and `Sidebar` components, allowing for custom content within the sidebar, navbar, and main page area.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/blog/2024-05-24-catalyst-application-layouts/index.mdx#_snippet_0

LANGUAGE: jsx
CODE:
```
import { SidebarLayout } from "@/components/sidebar-layout";
import { Navbar } from "@/components/navbar";
import { Sidebar } from "@/components/sidebar";

function Example({ children }) {
  return (
    <SidebarLayout
      sidebar={<Sidebar>{/* Sidebar menu */}</Sidebar>}
      navbar={<Navbar>{/* Navbar for mobile screens */}</Navbar>}
    >
      {/* Your page content */}
    </SidebarLayout>
  );
}
```

----------------------------------------

TITLE: Importing Tailwind CSS with @import Directive (CSS)
DESCRIPTION: Use the @import directive to inline import CSS files into your stylesheet, including the main Tailwind CSS file itself. This makes Tailwind's base styles, components, and utilities available.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/functions-and-directives.mdx#_snippet_0

LANGUAGE: CSS
CODE:
```
@import "tailwindcss";
```

----------------------------------------

TITLE: Controlling Gradient Color Interpolation in Tailwind CSS
DESCRIPTION: This snippet demonstrates how to apply color interpolation modifiers (`/srgb`, `/oklch`) to linear gradients in Tailwind CSS. Using `oklch` can produce more vivid gradients, especially when `from-*` and `to-*` colors are visually distant, while `srgb` provides standard interpolation. OKLAB is the default in v4.0.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/blog/tailwindcss-v4/index.mdx#_snippet_21

LANGUAGE: HTML
CODE:
```
<div class="bg-linear-to-r/srgb from-indigo-500 to-teal-400">...</div>
<div class="bg-linear-to-r/oklch from-indigo-500 to-teal-400">...</div>
```

----------------------------------------

TITLE: Tailwind CSS Equivalent for Hover
DESCRIPTION: Shows how Tailwind CSS achieves conditional styling by using separate utility classes for default (`.bg-sky-500`) and hover states (`.hover\:bg-sky-700:hover`). Each class is responsible for a specific style or state.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/hover-focus-and-other-states.mdx#_snippet_2

LANGUAGE: CSS
CODE:
```
.bg-sky-500 {
  background-color: #0ea5e9;
}

.hover\:bg-sky-700:hover {
  background-color: #0369a1;
}
```

----------------------------------------

TITLE: Preventing Flex Item Growth/Shrinkage with flex-none in JSX
DESCRIPTION: This JSX snippet demonstrates how to use the `flex-none` utility class within a React component to prevent specific flex items from growing or shrinking. It showcases two fixed-size `flex-none` items and one `flex-1` item that expands to fill available space, illustrating the combined effect of these Tailwind CSS flex properties.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/flex.mdx#_snippet_3

LANGUAGE: jsx
CODE:
```
<div className="grid grid-cols-1">
  <Stripes border className="col-start-1 row-start-1 rounded-lg" />
  <div className="col-start-1 row-start-1 flex gap-4 rounded-lg font-mono text-sm leading-6 font-bold text-white">
    <div className="flex-none last:pr-8 sm:last:pr-0">
      <div className="flex h-14 w-14 items-center justify-center rounded-lg bg-indigo-500 p-4">01</div>
    </div>
    <div className="flex-none last:pr-8 sm:last:pr-0">
      <div className="flex w-32 items-center justify-center rounded-lg bg-indigo-500 p-4">02</div>
    </div>
    <div className="flex-1 last:pr-8 sm:last:pr-0">
      <div className="flex items-center justify-center rounded-lg bg-indigo-300 p-4 dark:bg-indigo-800 dark:text-indigo-400">
        03
      </div>
    </div>
  </div>
</div>
```

----------------------------------------

TITLE: Adjusting Form Control Size Based on Content in HTML
DESCRIPTION: This snippet demonstrates how to use the `field-sizing-content` utility class in HTML to make a textarea automatically adjust its size based on its content. This is useful for creating dynamic form controls that expand or shrink as the user types.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/field-sizing.mdx#_snippet_0

LANGUAGE: HTML
CODE:
```
<!-- [!code classes:field-sizing-content] -->
<textarea class="field-sizing-content ..." rows="2">
  Latex Salesman, Vanderlay Industries
</textarea>
```

LANGUAGE: JSX
CODE:
```
<textarea
      rows="2"
      defaultValue="Latex Salesman, Vanderlay Industries"
      className="mx-auto block field-sizing-content rounded-md p-2 text-sm text-gray-950 outline-1 outline-gray-900/10 focus:outline-2 focus:outline-gray-900 dark:bg-gray-950/25 dark:text-white dark:outline-1 dark:outline-white/5 dark:focus:outline-white/20"
    />
```

----------------------------------------

TITLE: Applying Text Color on Hover in React/JSX
DESCRIPTION: This React/JSX snippet demonstrates how to change text color on hover using Tailwind CSS classes like `hover:text-blue-600` and `dark:hover:text-blue-400` applied to an anchor tag, providing interactive styling within a React component.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/docs/color.mdx#_snippet_5

LANGUAGE: jsx
CODE:
```
<p className="text-center text-xl font-medium text-gray-900 dark:text-gray-200">
  Oh I gotta get on that{" "}
  <a
    href="https://en.wikipedia.org/wiki/Internet"
    target="_blank"
    className="underline hover:text-blue-600 dark:hover:text-blue-400"
  >
    internet
  </a>
  , I'm late on everything!
</p>
```

----------------------------------------

TITLE: Sorting Modifiers After Plain Utilities in HTML
DESCRIPTION: This example demonstrates that Tailwind CSS modifiers, such as `hover:` and `focus:`, are grouped and sorted after all plain utility classes. The plain `scale-125` and `opacity-50` classes appear first, followed by their respective `hover:` modified versions.
SOURCE: https://github.com/tailwindlabs/tailwindcss.com/blob/main/src/blog/automatic-class-sorting-with-prettier/index.mdx#_snippet_6

LANGUAGE: HTML
CODE:
```
<div class="hover:opacity-75 opacity-50 hover:scale-150 scale-125"> <!-- [!code --] -->
<div class="scale-125 opacity-50 hover:scale-150 hover:opacity-75"> <!-- [!code ++] -->
    <!-- ... -->
  </div>
</div>
```