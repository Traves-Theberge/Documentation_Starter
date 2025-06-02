TITLE: Starting Next.js Development Server (Shell)
DESCRIPTION: This command starts the development server for a Next.js application, typically accessible at `http://localhost:3000`. It enables features like hot-reloading, allowing real-time updates in the browser as code changes are made. This command is essential for local development and testing of Next.js projects.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/blog/what-is-nextjs.mdx#_snippet_1

LANGUAGE: Shell
CODE:
```
npm run dev
```

----------------------------------------

TITLE: Adding a Tailwind CSS Class Helper Function in TypeScript
DESCRIPTION: This TypeScript snippet defines a `cn` utility function in `lib/utils.ts` to simplify conditional Tailwind CSS class management. It leverages `clsx` for combining class values and `tailwind-merge` for intelligently merging conflicting Tailwind classes, ensuring clean and efficient class application in components. This helper is crucial for dynamic styling based on component state or props.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/installation/manual.mdx#_snippet_6

LANGUAGE: typescript
CODE:
```
import { clsx, type ClassValue } from "clsx";
import { twMerge } from "tailwind-merge";

export function cn(...inputs: ClassValue[]) {
  return twMerge(clsx(inputs));
}
```

----------------------------------------

TITLE: Starting React App with npm (Bash)
DESCRIPTION: This command starts the development server for the React application using npm. It serves the application locally, usually accessible at `http://localhost:3000/`.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/blog/mui-box.mdx#_snippet_2

LANGUAGE: bash
CODE:
```
npm start
```

----------------------------------------

TITLE: Starting the React Development Server
DESCRIPTION: This command initiates the development server for the React application, typically making it accessible via a web browser at `http://localhost:3000`. It compiles the project and provides live reloading for development.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/blog/install-tailwind-react.mdx#_snippet_3

LANGUAGE: javascript
CODE:
```
npm run start
```

----------------------------------------

TITLE: Creating a New Next.js Application via CLI (Bash)
DESCRIPTION: This snippet provides the essential bash commands to initialize a new Next.js project using `create-next-app`, navigate into the project directory, and start the development server. It requires NPX (shipped with npm 5.2.0+), npm v6.1, or yarn to be installed as prerequisites.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/blog/next-js-app.mdx#_snippet_0

LANGUAGE: bash
CODE:
```
npx create-next-app
// Follow the instructions to create your first Next.js project.
cd <project-name>
npm run dev
```

----------------------------------------

TITLE: Configuring Global CSS Variables for Theming in MagicUI
DESCRIPTION: This CSS snippet defines global CSS variables for a MagicUI project within `globals.css`. It sets up a comprehensive color palette for both light and dark themes, utilizing Tailwind CSS's `@apply` directive for base styles. These variables control background, foreground, muted, popover, border, input, card, primary, secondary, accent, destructive colors, and border radius, enabling consistent theming across components.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/installation/manual.mdx#_snippet_5

LANGUAGE: css
CODE:
```
@tailwind base;
@tailwind components;
@tailwind utilities;

@layer base {
  :root {
    --background: 0 0% 100%;
    --foreground: 222.2 47.4% 11.2%;

    --muted: 210 40% 96.1%;
    --muted-foreground: 215.4 16.3% 46.9%;

    --popover: 0 0% 100%;
    --popover-foreground: 222.2 47.4% 11.2%;

    --border: 214.3 31.8% 91.4%;
    --input: 214.3 31.8% 91.4%;

    --card: 0 0% 100%;
    --card-foreground: 222.2 47.4% 11.2%;

    --primary: 222.2 47.4% 11.2%;
    --primary-foreground: 210 40% 98%;

    --secondary: 210 40% 96.1%;
    --secondary-foreground: 222.2 47.4% 11.2%;

    --accent: 210 40% 96.1%;
    --accent-foreground: 222.2 47.4% 11.2%;

    --destructive: 0 100% 50%;
    --destructive-foreground: 210 40% 98%;

    --ring: 215 20.2% 65.1%;

    --radius: 0.5rem;
  }

  .dark {
    --background: 224 71% 4%;
    --foreground: 213 31% 91%;

    --muted: 223 47% 11%;
    --muted-foreground: 215.4 16.3% 56.9%;

    --accent: 216 34% 17%;
    --accent-foreground: 210 40% 98%;

    --popover: 224 71% 4%;
    --popover-foreground: 215 20.2% 65.1%;

    --border: 216 34% 17%;
    --input: 216 34% 17%;

    --card: 224 71% 4%;
    --card-foreground: 213 31% 91%;

    --primary: 210 40% 98%;
    --primary-foreground: 222.2 47.4% 1.2%;

    --secondary: 222.2 47.4% 11.2%;
    --secondary-foreground: 210 40% 98%;

    --destructive: 0 63% 31%;
    --destructive-foreground: 210 40% 98%;

    --ring: 216 34% 17%;

    --radius: 0.5rem;
  }
}

@layer base {
  * {
    @apply border-border;
  }
  body {
    @apply bg-background text-foreground;
    font-feature-settings:
      "rlig" 1,
      "calt" 1;
  }
}
```

----------------------------------------

TITLE: Destructuring Props in React Functional Components
DESCRIPTION: This snippet demonstrates how to use object destructuring to extract specific props (`name`, `age`) from the `props` object passed to a React component. This practice improves code readability and reduces verbosity by directly accessing required properties, making it clearer which props are being used.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/blog/react-component-best-practices.mdx#_snippet_1

LANGUAGE: JavaScript
CODE:
```
const {name, age} = props
```

----------------------------------------

TITLE: Creating a Basic Card Component in React with Tailwind CSS
DESCRIPTION: This snippet defines a reusable `Card` functional component in React using TypeScript. It structures a simple card UI with a title, description, and tags, styled entirely with Tailwind CSS classes. This component serves as an individual item within a grid layout.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/blog/tailwind-css-grid.mdx#_snippet_1

LANGUAGE: tsx
CODE:
```
import { FC } from "react";
const Card: FC<any> = () => {
  return (
    <div className="max-w-sm rounded overflow-hidden shadow-md bg-white">
      <div className="px-6 py-4">
        <div className="font-bold text-xl mb-2">Mountain Sunset</div>
        <p className="text-gray-700 text-base">
          Lorem ipsum dolor sit amet, consectetur adipisicing elit. Voluptatibus
          quia, nulla! Maiores et perferendis eaque, exercitationem praesentium
          nihil.
        </p>
      </div>
      <div className="px-6 pt-4 pb-2">
        <span className="inline-block bg-gray-200 rounded-full px-3 py-1 text-sm font-semibold text-gray-700 mr-2 mb-2">
          #photography
        </span>
        <span className="inline-block bg-gray-200 rounded-full px-3 py-1 text-sm font-semibold text-gray-700 mr-2 mb-2">
          #travel
        </span>
        <span className="inline-block bg-gray-200 rounded-full px-3 py-1 text-sm font-semibold text-gray-700 mb-2">
          #winter
        </span>
      </div>
    </div>
  );
};
export default Card;
```

----------------------------------------

TITLE: Implementing Conditional Rendering in React
DESCRIPTION: This snippet demonstrates conditional rendering in React using a ternary operator. It fetches a list of products and displays either a component (doSomething()) if products exist or a 'No products' message if the list is empty. The useState and useEffect hooks manage component state and side effects.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/blog/react-design-patterns.mdx#_snippet_3

LANGUAGE: tsx
CODE:
```
export default function App() {
  let [products, setProducts] = useState([]);
  useEffect(() => {
    // fetch a list of products from the server
  }, []);
  return (
    <div className="App">
      <h1>Products</h1>
      {products.length > 0 ? doSomething() : <p>No products</p>}
    </div>
  );
}
```

----------------------------------------

TITLE: Implementing Render Prop Pattern for Temperature Conversion in React
DESCRIPTION: This snippet demonstrates the render prop pattern in React to create a flexible temperature conversion input. It defines an `Input` component that manages its own state and uses `renderKelvin` and `renderFahrenheit` props to display converted temperatures. The `App` component then passes specific rendering logic for Kelvin and Fahrenheit displays to the `Input` component, allowing for reusable logic and presentation separation.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/blog/react-design-patterns.mdx#_snippet_7

LANGUAGE: TypeScript
CODE:
```
import React, { useState } from "react";

function Input(props) {
  const [value, setValue] = useState("");
  return (
    <>
      <input value={value} onChange={(e) => setValue(e.target.value)} />
      {props.renderKelvin({ value: value + 273.15 })}
      {props.renderFahrenheit({ value: (value * 9) / 5 + 32 })}
    </>
  );
}

export default function App() {
  return (
    <Input
      renderKelvin={({ value }) => <div className="temp">{value}K</div>}
      renderFahrenheit={({ value }) => <div className="temp">{value}°F</div>}
    />
  );
}
```

----------------------------------------

TITLE: Configuring TypeScript Path Aliases in tsconfig.json (JSON)
DESCRIPTION: This JSON snippet demonstrates how to configure path aliases in `tsconfig.json` to simplify import statements. Specifically, it sets up the `@` alias to resolve to the project's root directory, allowing for cleaner absolute imports like `@/components/Button`. This configuration helps manage module resolution in large projects.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/installation/manual.mdx#_snippet_3

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

TITLE: Applying Hover Transition with CSS
DESCRIPTION: This CSS snippet demonstrates how to create a smooth transition effect for the background-color property of a button element when it is hovered over. It uses the `transition` property to define the duration and timing function of the change, and the `:hover` pseudo-class to specify the new background color.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/blog/ui-animation.mdx#_snippet_0

LANGUAGE: CSS
CODE:
```
.button {
  transition: background-color 0.3s ease-in-out;
}
.button:hover {
  background-color: #007bff;
}
```

----------------------------------------

TITLE: Creating New Branch for Changes - Bash
DESCRIPTION: This command creates and switches to a new Git branch named `my-new-branch`. It's good practice to work on a separate branch to isolate your changes and facilitate pull requests.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/CONTRIBUTING.md#_snippet_2

LANGUAGE: Bash
CODE:
```
git checkout -b my-new-branch
```

----------------------------------------

TITLE: Configuring TypeScript Base URL and Paths in tsconfig.json
DESCRIPTION: This TypeScript configuration snippet adds `baseUrl` and `paths` to the `compilerOptions` in `tsconfig.json`. This allows for absolute path imports, such as `@/` mapping to the `src` directory, improving module resolution for both the compiler and IDEs.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/installation/vite.mdx#_snippet_2

LANGUAGE: ts
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

TITLE: Initializing shadcn/ui in Next.js Project
DESCRIPTION: This `npx` command initializes a new shadcn/ui project or sets up an existing one within a Next.js application. It guides the user through interactive prompts to configure style, base color, and CSS variable usage.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/installation/next.mdx#_snippet_0

LANGUAGE: bash
CODE:
```
npx shadcn@latest init
```

----------------------------------------

TITLE: Installing Core React and TypeScript Dependencies
DESCRIPTION: This command installs or updates essential packages for a React and TypeScript project, including `react`, `react-dom`, `typescript`, and their respective type definitions. It ensures the project has the necessary libraries for development.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/blog/install-tailwind-react.mdx#_snippet_2

LANGUAGE: javascript
CODE:
```
npm install react react-dom typescript @types/react @types/react-dom
```

----------------------------------------

TITLE: Configuring PostCSS for Tailwind CSS Production Optimization
DESCRIPTION: This `postcss.config.js` snippet configures PostCSS plugins to optimize Tailwind CSS for production builds. It conditionally includes `cssnano` for CSS minification and purging of unused styles only when `NODE_ENV` is 'production', ensuring smaller bundle sizes while maintaining full utility class availability during development.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/blog/install-tailwind-react.mdx#_snippet_11

LANGUAGE: javascript
CODE:
```
module.exports = {
  plugins: {
    tailwindcss: {},
    autoprefixer: {},
    ...(process.env.NODE_ENV === "production"
      ? {
          cssnano: {},
        }
      : {}),
  },
};
```

----------------------------------------

TITLE: Installing Material-UI Core Components with Yarn
DESCRIPTION: This Yarn command adds the core Material-UI library (@mui/material) and its styling engine dependencies (@emotion/react, @emotion/styled) to your project, providing the necessary packages for Material-UI components.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/blog/mui-table.mdx#_snippet_2

LANGUAGE: bash
CODE:
```
yarn add @mui/material @emotion/react @emotion/styled
```

----------------------------------------

TITLE: Installing Material UI Packages with npm
DESCRIPTION: This bash command installs the core Material UI library along with its required peer dependencies, @emotion/react and @emotion/styled, which are essential for styling. It is the foundational step to integrate Material UI into any React project, enabling access to its comprehensive component library.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/blog/material-ui-react.mdx#_snippet_0

LANGUAGE: bash
CODE:
```
npm install @mui/material @emotion/react @emotion/styled
```

----------------------------------------

TITLE: Configuring Path Aliases in tsconfig.json (TypeScript)
DESCRIPTION: This TypeScript JSON configuration snippet adds `baseUrl` and `paths` to `tsconfig.json`, enabling absolute path imports like `@/` for source files, improving module resolution and code readability.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/installation/astro.mdx#_snippet_7

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

TITLE: Configuring Peer Dependencies in package.json
DESCRIPTION: This JSON snippet defines peer dependencies for a React component library, specifying that `react` and `react-dom` must be pre-installed in the consuming project with versions greater than or equal to 16.0.0. Peer dependencies ensure compatibility and prevent multiple instances of common libraries.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/blog/create-react-component-library.mdx#_snippet_0

LANGUAGE: JSON
CODE:
```
peerDependencies: {
  "react": ">=16.0.0",
  "react-dom": ">=16.0.0"
}
```

----------------------------------------

TITLE: Creating a Basic React Hero Component
DESCRIPTION: This JavaScript snippet defines a functional React component named `Hero`. It renders a `div` with a title, subtitle, and a call-to-action button, applying styles via `Hero.css`. The component is then exported for use in other parts of the application.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/blog/react-hero-component.mdx#_snippet_8

LANGUAGE: javascript
CODE:
```
import React from "react";
import "./Hero.css";

const Hero = () => (
  <div className="hero">
    <h1 className="hero-title">Welcome to Our Website</h1>
    <p className="hero-subtitle">Discover amazing content</p>
    <button className="hero-cta">Get Started</button>
  </div>
);

export default Hero;
```

----------------------------------------

TITLE: Setting up React Redux Provider Pattern for Global State
DESCRIPTION: This code illustrates how the Provider pattern is implemented using `react-redux` to make the Redux store available to all components in the application. It wraps the root `App` component with the `Provider` component, passing the Redux `store` as a prop. This allows descendant components to access the global state without prop drilling.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/blog/react-design-patterns.mdx#_snippet_1

LANGUAGE: tsx
CODE:
```
import React from "react";
import ReactDOM from "react-dom";
import { Provider } from "react-redux";
import store from "./store";
import App from "./App";
const rootElement = document.getElementById("root");
ReactDOM.render(
  <Provider store={store}>
    <App />
  </Provider>,
  rootElement,
);
```

----------------------------------------

TITLE: Defining a Functional React Component with TypeScript Props - TypeScript
DESCRIPTION: This snippet illustrates how to define a functional React component using TypeScript, including the definition of an interface for its props. It ensures type safety for the `text` prop, making the component's expected inputs clear and preventing common type-related errors during development.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/blog/react-tips.mdx#_snippet_3

LANGUAGE: TypeScript
CODE:
```
import * as React from 'react';
interface Props {
  text: string;
}
const MyComponent: React.FC<Props> = ({ text }) => {
  return (
    <div>
      {text}
    </div>
  );
};
```

----------------------------------------

TITLE: Styling HTML Header with Tailwind CSS Utility Classes
DESCRIPTION: This HTML snippet illustrates how to apply various Tailwind CSS utility classes to style a header and navigation bar. It uses classes for background color (`bg-gray-800`), text color (`text-white`), padding (`py-4`), layout (`flex`, `justify-between`, `items-center`), and spacing (`space-x-4`) to create a responsive and visually appealing component.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/blog/tailwind-landing-page.mdx#_snippet_3

LANGUAGE: html
CODE:
```
<header class="bg-gray-800 text-white py-4">
  <nav class="container mx-auto flex justify-between items-center">
    <a href="#" class="text-xl font-bold">Your Logo</a>
    <ul class="flex space-x-4">
      <li><a href="#" class="hover:text-gray-400">Home</a></li>
      <li><a href="#" class="hover:text-gray-400">Features</a></li>
      <li><a href="#" class="hover:text-gray-400">Pricing</a></li>
      <li><a href="#" class="hover:text-gray-400">Contact</a></li>
    </ul>
  </nav>
</header>
```

----------------------------------------

TITLE: Applying Strategy Design Pattern in React
DESCRIPTION: This example illustrates the Strategy design pattern in React, where a parent component (Strategy) accepts children as a prop, allowing dynamic behavior changes from the outside. It demonstrates how to encapsulate different behaviors (represented by ChildComp) and make them interchangeable, promoting the open-closed principle.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/blog/react-design-patterns.mdx#_snippet_4

LANGUAGE: tsx
CODE:
```
const Strategy = ({ children }) => {
  return <div>{children}</div>;
};
const ChildComp = () => {
  return <div>ChildComp</div>;
};
<Strategy children={<ChildComp />} />;
```

----------------------------------------

TITLE: Structuring a Container Component in React
DESCRIPTION: This snippet demonstrates the structure of a Container Component, which is responsible for data fetching, state management, and handling business logic. It imports and renders a `PresentationalComponent`, passing data and event handlers as props. This pattern separates concerns, with the container managing 'how things work' and the presentational component managing 'how things look'.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/blog/react-design-patterns.mdx#_snippet_2

LANGUAGE: tsx
CODE:
```
import React, { Component } from "react";
import PresentationalComponent from "./PresentationalComponent";
class ContainerComponent extends Component {
  constructor(props) {
    super(props);
    this.state = {
      // Initialize state
    };
  }
  componentDidMount() {
    // Fetch data or perform initialization logic
  }
  handleEvent = () => {
    // Handle events and update state if needed
  };
  render() {
    return (
      <div>
        <PresentationalComponent
          prop1={this.state.data}
          prop2="Some value"
          onClick={this.handleEvent}
        />
      </div>
    );
  }
}
export default ContainerComponent;
```

----------------------------------------

TITLE: Configuring Tailwind CSS Content Paths
DESCRIPTION: This JavaScript snippet updates the `tailwind.config.js` file to specify the content paths where Tailwind CSS should scan for class names. The `content` array ensures that Tailwind only generates CSS for classes actually used in `.js`, `.jsx`, `.ts`, and `.tsx` files within the `src` directory, optimizing bundle size.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/blog/install-tailwind-react.mdx#_snippet_6

LANGUAGE: javascript
CODE:
```
/** @type {import('tailwindcss').Config} */
module.exports = {
  content: ["./src/**/*.{js,jsx,ts,tsx}"],
  theme: {
    extend: {},
  },
  plugins: [],
};
```

----------------------------------------

TITLE: Creating a Column-Based Grid Gallery in React with Tailwind CSS
DESCRIPTION: This snippet defines the `CardGallery` component, which acts as a container for multiple `Card` components. It uses Tailwind CSS's `grid` and `grid-col-3` classes to arrange cards in a three-column layout. This demonstrates the basic setup for a column-first grid.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/blog/tailwind-css-grid.mdx#_snippet_2

LANGUAGE: tsx
CODE:
```
import { FC } from "react";
import Card from "./Card";
const CardGallery: FC<any> = () => {
  return (
    <div className="grid grid-col-3">
      <Card />
      <Card />
      <Card />
      <Card />
    </div>
  );
};
export default CardGallery;
```

----------------------------------------

TITLE: Configuring Tailwind CSS with Custom Theme and Plugins (JavaScript)
DESCRIPTION: This JavaScript configuration file for Tailwind CSS defines custom theme settings, including extended colors, border radii, and font families, using CSS variables for dynamic theming. It also includes keyframe animations for UI components and integrates the `tailwindcss-animate` plugin for enhanced animation capabilities. This setup provides a robust foundation for consistent styling and interactive elements.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/installation/manual.mdx#_snippet_4

LANGUAGE: javascript
CODE:
```
const { fontFamily } = require("tailwindcss/defaultTheme");

/** @type {import('tailwindcss').Config} */
module.exports = {
  darkMode: ["class"],
  content: ["app/**/*.{ts,tsx}", "components/**/*.{ts,tsx}"],
  theme: {
    container: {
      center: true,
      padding: "2rem",
      screens: {
        "2xl": "1400px"
      }
    },
    extend: {
      colors: {
        border: "hsl(var(--border))",
        input: "hsl(var(--input))",
        ring: "hsl(var(--ring))",
        background: "hsl(var(--background))",
        foreground: "hsl(var(--foreground))",
        primary: {
          DEFAULT: "hsl(var(--primary))",
          foreground: "hsl(var(--primary-foreground))"
        },
        secondary: {
          DEFAULT: "hsl(var(--secondary))",
          foreground: "hsl(var(--secondary-foreground))"
        },
        destructive: {
          DEFAULT: "hsl(var(--destructive))",
          foreground: "hsl(var(--destructive-foreground))"
        },
        muted: {
          DEFAULT: "hsl(var(--muted))",
          foreground: "hsl(var(--muted-foreground))"
        },
        accent: {
          DEFAULT: "hsl(var(--accent))",
          foreground: "hsl(var(--accent-foreground))"
        },
        popover: {
          DEFAULT: "hsl(var(--popover))",
          foreground: "hsl(var(--popover-foreground))"
        },
        card: {
          DEFAULT: "hsl(var(--card))",
          foreground: "hsl(var(--card-foreground))"
        }
      },
      borderRadius: {
        lg: `var(--radius)`,
        md: `calc(var(--radius) - 2px)`,
        sm: "calc(var(--radius) - 4px)"
      },
      fontFamily: {
        sans: ["var(--font-sans)", ...fontFamily.sans]
      },
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
  },
  plugins: [require("tailwindcss-animate")]
};
```

----------------------------------------

TITLE: Creating Interactive Button Animation with Framer Motion (React)
DESCRIPTION: This JavaScript (React) snippet uses the Framer Motion library to add interactive scale animations to a button component. The `whileHover` prop scales the button up when hovered, and `whileTap` scales it down when clicked, providing visual feedback to the user.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/blog/ui-animation.mdx#_snippet_1

LANGUAGE: JavaScript
CODE:
```
import { motion } from "framer-motion";
const Button = () => {
  return (
    <motion.button whileHover={{ scale: 1.1 }} whileTap={{ scale: 0.9 }}>
      Click me
    </motion.button>
  );
};
```

----------------------------------------

TITLE: Cloning MagicUI Repository - Bash
DESCRIPTION: This command clones your forked MagicUI repository to your local machine, replacing <YOUR_USERNAME> with your GitHub username. It's the first step after forking to get the project files locally.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/CONTRIBUTING.md#_snippet_0

LANGUAGE: Bash
CODE:
```
git clone https://github.com/<YOUR_USERNAME>/magicui.git
```

----------------------------------------

TITLE: Using React.Fragment in a Functional Component - JavaScript
DESCRIPTION: This snippet demonstrates how to use `React.Fragment` to group multiple JSX elements without introducing an additional DOM node. It shows a functional React component returning a fragment containing a heading and a paragraph, improving DOM structure and readability.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/blog/react-tips.mdx#_snippet_0

LANGUAGE: JavaScript
CODE:
```
const MyComponent = () => {
  return (
    <React.Fragment>
      <h1>My React Component</h1>
      <p>This is an example of using React Fragment in a functional component.</p>
    </React.Fragment>
  );
};
```

----------------------------------------

TITLE: Creating Complex Layouts with Tailwind Typography and Components in React
DESCRIPTION: This React component illustrates how to build a more sophisticated UI layout by combining Tailwind CSS font size utilities with other styling classes for typography, spacing, and component styling. It demonstrates creating a heading, a descriptive paragraph, and an interactive button, showcasing Tailwind's utility-first approach for rapid custom design.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/blog/tailwind-font-size.mdx#_snippet_2

LANGUAGE: tsx
CODE:
```
import "./App.css";
import "./index.css";
function App() {
  return (
    <div className="w-full flex flex-col items-center justify-center h-screen bg-gray-100 gap-4">
      <div className="max-w-xl text-center">
        <h1 className="text-5xl font-bold underline text-center mb-4">
          Welcome to Tailwind CSS
        </h1>
        <p className="text-xl text-gray-600">
          This is a sample text layout using Tailwind CSS, an utility-first CSS
          framework for rapidly building custom designs.
        </p>
        <div className="mt-2">
          <button className="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded">
            Get Started
          </button>
        </div>
      </div>
    </div>
  );
}
export default App;
```

----------------------------------------

TITLE: Implementing Compound Component Pattern in React
DESCRIPTION: This example demonstrates the Compound Component pattern in React, where a CompoundComponentParent orchestrates multiple child components (CompoundComponentA, CompoundComponentB). This pattern allows components to work together cohesively, with the parent managing the overall structure and the children contributing specific functionalities, enhancing UI design and interaction.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/blog/react-design-patterns.mdx#_snippet_6

LANGUAGE: tsx
CODE:
```
import React from "react";
import CompoundComponentParent from "./CompoundComponentParent";
import { CompoundComponentA, CompoundComponentB } from "./CompoundComponents";
const App = () => {
  return (
    <CompoundComponentParent>
      <CompoundComponentA propA="Value for A" />
      <CompoundComponentB propB="Value for B" />
    </CompoundComponentParent>
  );
};
export default App;
```

----------------------------------------

TITLE: Importing Global CSS into App Component
DESCRIPTION: This TypeScript/JavaScript snippet, typically placed in `App.tsx`, imports the `index.css` file into the main application component. This ensures that the global styles, including the imported Tailwind CSS, are applied throughout the entire React project.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/blog/install-tailwind-react.mdx#_snippet_8

LANGUAGE: javascript
CODE:
```
import "./index.css";
```

----------------------------------------

TITLE: Creating a New React App with Next.js
DESCRIPTION: This command initializes a new Next.js project, named 'mui-table', which serves as the foundational React application for integrating Material-UI components. It sets up the necessary project structure and dependencies.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/blog/mui-table.mdx#_snippet_0

LANGUAGE: bash
CODE:
```
npx create-next-app@latest mui-table
```

----------------------------------------

TITLE: Structuring React Application with Hooks
DESCRIPTION: This snippet shows the basic structure of a React application utilizing functional components and hooks. It imports and renders a MyHooksComponent, demonstrating how App serves as a root component to integrate other hook-based components, highlighting the modern approach to state and lifecycle management in React.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/blog/react-design-patterns.mdx#_snippet_5

LANGUAGE: tsx
CODE:
```
import React from "react";
import MyHooksComponent from "./MyHooksComponent";
const App = () => {
  return (
    <div>
      <h1>React Hooks Example</h1>
      <MyHooksComponent />
    </div>
  );
};
export default App;
```

----------------------------------------

TITLE: SaaS Landing Page Layout with Video and Actions in JSX
DESCRIPTION: This JSX snippet defines a flexible container for a SaaS landing page section, showcasing a video demonstration alongside interactive components for downloading the repository and previewing the live application. It leverages TailwindCSS classes for responsive styling and custom React components (`RepoDownload`, `TemplatePreview`) for specific actions.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/templates/saas.mdx#_snippet_0

LANGUAGE: JSX
CODE:
```
<div className="flex max-w-[800px] flex-col gap-4">
  <video
    autoPlay
    loop
    muted
    src="/saas-demo.mp4"
    className="w-full rounded-xl border"
  />
  <div className="flex w-full flex-col gap-2 sm:flex-row">
    <RepoDownload repo={"saas-template"} owner={"magicuidesign"} />
    <TemplatePreview href="https://saas-magicui.vercel.app/">
      Live Preview
    </TemplatePreview>
  </div>
</div>
```

----------------------------------------

TITLE: Installing and Initializing Tailwind CSS (Bash)
DESCRIPTION: These commands install Tailwind CSS, PostCSS, and Autoprefixer as development dependencies. The `npx tailwindcss init -p` command then generates the `tailwind.config.js` and `postcss.config.js` configuration files, essential for Tailwind CSS setup.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/installation/vite.mdx#_snippet_1

LANGUAGE: bash
CODE:
```
npm install -D tailwindcss postcss autoprefixer

npx tailwindcss init -p
```

----------------------------------------

TITLE: Applying Predefined Tailwind Font Sizes in React
DESCRIPTION: This React component demonstrates the application of Tailwind CSS's predefined font size utility classes, ranging from text-xs to text-9xl. It showcases how to quickly and consistently style text elements with various sizes, promoting a cohesive UI design without manual pixel-based sizing.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/blog/tailwind-font-size.mdx#_snippet_1

LANGUAGE: tsx
CODE:
```
import "./App.css";
import "./index.css";
function App() {
  return (
    <div className="w-full flex flex-col items-center justify-center h-screen bg-gray-100 gap-4">
      <p className="text-xs">Text size xs</p>
      <p className="text-sm">Text size sm</p>
      <p className="text-base">Text size base</p>
      <p className="text-lg">Text size lg</p>
      <p className="text-xl">Text size xl</p>
      <p className="text-2xl">Text size 2xl</p>
      <p className="text-3xl">Text size 3xl</p>
      <p className="text-4xl">Text size 4xl</p>
      <p className="text-5xl">Text size 5xl</p>
      <p className="text-6xl">Text size 6xl</p>
      <p className="text-7xl">Text size 7xl</p>
      <p className="text-8xl">Text size 8xl</p>
      <p className="text-9xl">Text size 9xl</p>
    </div>
  );
}
export default App;
```

----------------------------------------

TITLE: Implementing a Basic Fade Transition with MUI in JavaScript
DESCRIPTION: This snippet demonstrates how to implement a basic fade transition using MUI's `Transition` component. It utilizes the `useState` hook to control the visibility of a `div` element, which fades in and out when a button is toggled. The `in` prop determines the component's visibility, and the `timeout` prop sets the animation duration to 500 milliseconds.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/blog/mui-transitions.mdx#_snippet_0

LANGUAGE: JavaScript
CODE:
```
import { Transition } from "@mui/material";
import { useState } from "react";

function MyComponent() {
  const [show, setShow] = useState(false);

  return (
    <div>
      <button onClick={() => setShow(!show)}>Toggle</button>
      <Transition in={show} timeout={500}>
        <div>This component will fade in and out.</div>
      </Transition>
    </div>
  );
}
```

----------------------------------------

TITLE: Creating a Basic React Button Component in TypeScript
DESCRIPTION: This TypeScript snippet defines a functional React component named `Button` along with its `ButtonProps` interface. It accepts `children` (React nodes) and an `onClick` function, rendering a simple HTML button. This component serves as a reusable UI element within the library.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/blog/create-react-component-library.mdx#_snippet_1

LANGUAGE: TypeScript
CODE:
```
import React from 'react';
export interface ButtonProps {
children: React.ReactNode;
onClick: () => void;
}
export const Button = ({ children, onClick }: ButtonProps) => {
return <button onClick={onClick}>{children}</button>;
};
```

----------------------------------------

TITLE: Extending Tailwind CSS Font Sizes in Configuration
DESCRIPTION: This JavaScript configuration snippet shows how to extend Tailwind CSS's default font size scale by modifying the `tailwind.config.js` file. New custom sizes like `xxs` and `xxl` are added within the `theme.extend.fontSize` property, making them available as utility classes in the project.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/blog/tailwind-font-size.mdx#_snippet_4

LANGUAGE: JavaScript
CODE:
```
module.exports = {
  theme: {
    extend: {
      fontSize: {
        xxs: "0.625rem",
        xxl: "1.75rem"
      }
    }
  }
};
```

----------------------------------------

TITLE: Initializing a New React Project with TypeScript using yarn - Shell
DESCRIPTION: This command, an alternative to `npx`, initializes a new React project named 'my-app' using Yarn, pre-configured with TypeScript support. It provides a convenient way to set up a type-safe React development environment using the Yarn package manager.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/blog/react-tips.mdx#_snippet_2

LANGUAGE: Shell
CODE:
```
yarn create react-app my-app --template typescript
```

----------------------------------------

TITLE: Creating a Tailwind CSS Card Component in React
DESCRIPTION: This snippet defines a functional React component named `Card` that renders a UI card styled entirely with Tailwind CSS utility classes. It demonstrates the basic structure of a card with an image placeholder, title, description, and tags, showcasing Tailwind's responsive and utility-first approach for styling.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/blog/install-tailwind-react.mdx#_snippet_9

LANGUAGE: javascript
CODE:
```
import { FC } from "react";
const Card: FC<any> = () => {
  return (
    <div className="max-w-sm rounded overflow-hidden shadow-md bg-white">
      <div className="px-6 py-4">
        <div className="font-bold text-xl mb-2">Mountain Sunset</div>
        <p className="text-gray-700 text-base">
          Lorem ipsum dolor sit amet, consectetur adipisicing elit. Voluptatibus
          quia, nulla! Maiores et perferendis eaque, exercitationem praesentium
          nihil.
        </p>
      </div>
      <div className="px-6 pt-4 pb-2">
        <span className="inline-block bg-gray-200 rounded-full px-3 py-1 text-sm font-semibold text-gray-700 mr-2 mb-2">
          #photography
        </span>
        <span className="inline-block bg-gray-200 rounded-full px-3 py-1 text-sm font-semibold text-gray-700 mr-2 mb-2">
          #travel
        </span>
        <span className="inline-block bg-gray-200 rounded-full px-3 py-1 text-sm font-semibold text-gray-700 mb-2">
          #winter
        </span>
      </div>
    </div>
  );
};
export default Card;
```

----------------------------------------

TITLE: Exporting Button Component from Library Entry Point (TypeScript)
DESCRIPTION: This TypeScript snippet from `index.ts` serves as the main entry point for the component library, re-exporting the `Button` component and its `ButtonProps` interface from the `./components/Button` module. This makes them accessible to consumers importing from the library's root.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/blog/create-react-component-library.mdx#_snippet_2

LANGUAGE: TypeScript
CODE:
```
export { Button, ButtonProps } from "./components/Button"
```

----------------------------------------

TITLE: Use TweetCard in Next.js RSC
DESCRIPTION: Demonstrates how to integrate and render the TweetCard component within a React Server Component (RSC) in a Next.js 13+ application by importing the component and passing a tweet ID.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/tweet-card.mdx#_snippet_1

LANGUAGE: tsx
CODE:
```
import { TweetCard } from "@/components/magicui/tweet-card";

export default async function App() {
  return <TweetCard id="1441032681968212480" />;
}
```

----------------------------------------

TITLE: Defining Core CSS Styles for a Hero Component
DESCRIPTION: This CSS snippet defines the foundational styles for a Hero component, targeting specific classes for the main container, title, subtitle, and call-to-action button. It sets properties like background, text alignment, font sizes, colors, and padding to create a visually appealing hero section, commonly used in React applications.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/blog/react-hero-component.mdx#_snippet_9

LANGUAGE: CSS
CODE:
```
.hero {
  background: url("hero-bg.jpg") no-repeat center center;
  background-size: cover;
  text-align: center;
  padding: 50px 20px;
}

.hero-title {
  font-size: 3rem;
  color: #fff;
}

.hero-subtitle {
  font-size: 1.5rem;
  color: #ddd;
}

.hero-cta {
  background-color: #3498db;
  color: #fff;
  border: none;
  padding: 10px 20px;
  font-size: 1rem;
  cursor: pointer;
}
```

----------------------------------------

TITLE: Initializing a New React Project with TypeScript using npx - Shell
DESCRIPTION: This command initializes a new React project named 'my-app' using Create React App, pre-configured with TypeScript support. It's a quick way to set up a type-safe React development environment from scratch.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/blog/react-tips.mdx#_snippet_1

LANGUAGE: Shell
CODE:
```
npx create-react-app my-app --template typescript
```

----------------------------------------

TITLE: Adding Build Script for Library Bundling with tsup in package.json
DESCRIPTION: This JSON snippet adds a `build` script to the `package.json` file, utilizing `tsup` to bundle the component library. The command `tsup src/index.ts --dts` compiles the `index.ts` entry point and generates TypeScript declaration files (`.d.ts`), preparing the library for distribution.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/blog/create-react-component-library.mdx#_snippet_4

LANGUAGE: JSON
CODE:
```
scripts: {
"build": "tsup src/index.ts --dts"
}
```

----------------------------------------

TITLE: Configuring TypeScript Base URL and Paths in tsconfig.app.json
DESCRIPTION: This snippet modifies `tsconfig.app.json` to include `baseUrl` and `paths` within `compilerOptions`. This configuration is crucial for IDEs to correctly resolve absolute imports, specifically mapping `@/` to the `src` directory, enhancing development experience.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/installation/vite.mdx#_snippet_3

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

TITLE: Implementing Responsive Border Radius in React with Tailwind CSS
DESCRIPTION: This React component demonstrates the application of responsive border-radius classes from Tailwind CSS. The card's roundedness changes from 'rounded-md' on small screens to 'md:rounded-lg' at the medium breakpoint and 'lg:rounded-xl' at the large breakpoint, showcasing dynamic styling based on screen size. It requires Tailwind CSS to be configured in the project.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/blog/tailwind-border-radius.mdx#_snippet_3

LANGUAGE: tsx
CODE:
```
import "./App.css";
import "./index.css";
function App() {
  return (
    <div className="w-full flex flex-rows items-center justify-center h-screen bg-gray-100 gap-4">
      <div className="bg-white p-6 rounded-md md:rounded-lg lg:rounded-xl shadow-md">
        <h2 className="text-xl font-bold">Responsive Card Title</h2>
        <p className="text-gray-700">
          This card adjusts its border radius based on the screen size.
        </p>
      </div>
    </div>
  );
}
export default App;
```

----------------------------------------

TITLE: Initializing magicui Project Dependencies - Bash
DESCRIPTION: This command initializes a new project with magicui, installing required dependencies like `framer-motion`, configuring `tailwind.config.js`, and setting up CSS variables.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/packages/cli/README.md#_snippet_0

LANGUAGE: bash
CODE:
```
npx magicui-cli init
```

----------------------------------------

TITLE: Configuring CSS Transitions with React Transition Component
DESCRIPTION: This snippet demonstrates how to use the React `Transition` component to apply CSS classes for animation. It specifies `in` for visibility, `timeout` for duration, and `classes` to map animation states (entering, exiting) to custom CSS class names like 'fade-in' and 'fade-out'.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/blog/mui-transitions.mdx#_snippet_11

LANGUAGE: javascript
CODE:
```
<Transition
  in={show}
  timeout={500}
  component="div"
  classes={{
    entering: "fade-in",
    exiting: "fade-out"
  }}
>
  {/* Your content */}
</Transition>
```

----------------------------------------

TITLE: Creating a Basic Material UI Card Component (JSX)
DESCRIPTION: This snippet demonstrates how to construct a simple Material UI card using core MUI components like Card, CardContent, CardActions, Button, and Typography. It showcases structuring content within the card, applying basic styling via the `sx` prop, and rendering the card within a React application. Dependencies include `@mui/material` and a local `styles.css` file.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/blog/mui-card.mdx#_snippet_5

LANGUAGE: JSX
CODE:
```
import {
  Card,
  CardActions,
  CardContent,
  Button,
  Typography,
} from "@mui/material";
import "./styles.css";

const BasicCard = () => {
  return (
    <Card sx={{ maxWidth: 400 }}>
      <CardContent>
        <Typography
          sx={{ fontSize: 24, mb: 2, textAlign: "center" }}
          variant="h2"
          color="text.secondary"
          gutterBottom
        >
          Insightful Design Tip
        </Typography>
        <Typography
          sx={{ fontSize: 18, mb: 1.5 }}
          variant="h5"
          color="text.secondary"
        >
          "MagicUI is a free and open-source UI library that we designed
          specifically for design engineers."
        </Typography>
        <Typography sx={{ mb: 1 }} variant="body2">
          "Best UI Library" by MagicUI.
        </Typography>
      </CardContent>
      <CardActions>
        <Button size="small">Read More</Button>
      </CardActions>
    </Card>
  );
};

const App = () => {
  return (
    <div className="App">
      <h1>Basic card</h1>
      <BasicCard />
    </div>
  );
};

export default App;
```

----------------------------------------

TITLE: Applying Animation to HTML Element in CSS
DESCRIPTION: This CSS snippet applies a custom animation named 'fadeInRotate' to the HTML element identified by '#animated-text'. It configures the animation to run for 2 seconds, use an 'ease-in-out' timing function for smooth acceleration and deceleration, and repeat infinitely, creating a continuous effect.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/blog/text-animation-css.mdx#_snippet_1

LANGUAGE: CSS
CODE:
```
#animated-text {
  animation: fadeInRotate 2s ease-in-out infinite;
}
```

----------------------------------------

TITLE: Overriding MUI Box Component with Span in React
DESCRIPTION: This snippet demonstrates how to override the default DOM element generated by Material UI's Box component. By setting the `component` prop to 'span', it renders a `<span>` element instead of the default `<div>`, wrapping a Material UI Button. It requires `@mui/material/Box` and `@mui/material/Button` dependencies for proper functionality.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/blog/mui-box.mdx#_snippet_8

LANGUAGE: jsx
CODE:
```
import * as React from "react";
import Box from "@mui/material/Box";
import Button from "@mui/material/Button";

export default function BoxComponent() {
  return (
    <Box component="span" sx={{ p: 2, border: "1px dashed grey" }}>
      <Button>Save</Button>
    </Box>
  );
}
```

----------------------------------------

TITLE: Creating a New Next.js Application (Shell)
DESCRIPTION: This shell command initializes a new Next.js project using the latest version of the framework. It sets up the basic project structure, including necessary files and dependencies, allowing developers to quickly begin building their web application. This is the first step in setting up a Next.js development environment.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/blog/what-is-nextjs.mdx#_snippet_0

LANGUAGE: Shell
CODE:
```
npx create-next-app@latest
```

----------------------------------------

TITLE: Use a shadcn/ui Component (TSX)
DESCRIPTION: Example of importing and using a shadcn/ui component, like the Button, within a Remix route component.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/installation/remix.mdx#_snippet_8

LANGUAGE: tsx
CODE:
```
import { Button } from "~/components/ui/button";

export default function Home() {
  return (
    <div>
      <Button>Click me</Button>
    </div>
  );
}
```

----------------------------------------

TITLE: Using BlurFade Component with Images (TypeScript React)
DESCRIPTION: This example demonstrates how to wrap multiple image elements with the `BlurFade` component to apply a blur fade-in/out animation. The `BlurFade` component animates its children, providing a smooth visual transition for the enclosed content based on its configurable props like duration, delay, and direction.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/blur-fade.mdx#_snippet_2

LANGUAGE: tsx
CODE:
```
<BlurFade>
  <img src="https://picsum.photos/300/200?random=1" alt="Sample 1" />
  <img src="https://picsum.photos/300/200?random=2" alt="Sample 2" />
  <img src="https://picsum.photos/300/200?random=3" alt="Sample 3" />
</BlurFade>
```

----------------------------------------

TITLE: Managing List Animations with React TransitionGroup
DESCRIPTION: This snippet demonstrates using `TransitionGroup` to manage animations for dynamic lists of components. It wraps `CSSTransition` for each item, allowing individual elements to animate when added or removed, using a common 'fade' class prefix for transitions.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/blog/mui-transitions.mdx#_snippet_15

LANGUAGE: javascript
CODE:
```
<TransitionGroup>
  {items.map((item) => (
    <CSSTransition key={item.id} timeout={500} classNames="fade">
      {/* Your component */}
    </CSSTransition>
  ))}
</TransitionGroup>
```

----------------------------------------

TITLE: Installing Node.js Types for Vite (Bash)
DESCRIPTION: This command installs the `@types/node` package as a development dependency. It provides TypeScript type definitions for Node.js, which is necessary for resolving the `path` module in `vite.config.ts` without type errors.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/installation/vite.mdx#_snippet_4

LANGUAGE: bash
CODE:
```
npm i -D @types/node
```

----------------------------------------

TITLE: Adding Specific shadcn-ui Component via magicui CLI - Bash
DESCRIPTION: This command allows users to install a specific shadcn-ui component, such as 'button', using the magicui CLI by specifying the `--shadcn` flag.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/packages/cli/README.md#_snippet_8

LANGUAGE: bash
CODE:
```
npx magicui-cli add --shadcn button
```

----------------------------------------

TITLE: Creating a Basic Material-UI Table Component in React
DESCRIPTION: This React component demonstrates how to build a simple Material-UI table. It imports necessary Material-UI components, defines a `createData` helper function for rows, populates sample data, and renders the table structure using `TableContainer`, `TableHead`, `TableBody`, `TableRow`, and `TableCell`.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/blog/mui-table.mdx#_snippet_4

LANGUAGE: jsx
CODE:
```
import * as React from "react";
import Table from "@mui/material/Table";
import TableBody from "@mui/material/TableBody";
import TableCell from "@mui/material/TableCell";
import TableContainer from "@mui/material/TableContainer";
import TableHead from "@mui/material/TableHead";
import TableRow from "@mui/material/TableRow";
import Paper from "@mui/material/Paper";

function createData(
  name: string,
  calories: number,
  fat: number,
  carbs: number,
  protein: number
) {
  return { name, calories, fat, carbs, protein };
}

const rows = [
  createData("Frozen yoghurt", 159, 6.0, 24, 4.0),
  createData("Ice cream sandwich", 237, 9.0, 37, 4.3),
  createData("Eclair", 262, 16.0, 24, 6.0),
  createData("Cupcake", 305, 3.7, 67, 4.3),
  createData("Gingerbread", 356, 16.0, 49, 3.9),
];

export default function BasicTable() {
  return (
    <TableContainer component={Paper}>
      <Table sx={{ minWidth: 650 }} aria-label="simple table">
        <TableHead>
          <TableRow>
            <TableCell>Dessert (100g serving)</TableCell>
            <TableCell align="right">Calories</TableCell>
            <TableCell align="right">Fat&nbsp;(g)</TableCell>
            <TableCell align="right">Carbs&nbsp;(g)</TableCell>
            <TableCell align="right">Protein&nbsp;(g)</TableCell>
          </TableRow>
        </TableHead>
        <TableBody>
          {rows.map((row) => (
            <TableRow
              key={row.name}
              sx={{ "&:last-child td, &:last-child th": { border: 0 } }}
            >
              <TableCell component="th" scope="row">
                {row.name}
              </TableCell>
              <TableCell align="right">{row.calories}</TableCell>
              <TableCell align="right">{row.fat}</TableCell>
              <TableCell align="right">{row.carbs}</TableCell>
              <TableCell align="right">{row.protein}</TableCell>
            </TableRow>
          ))}
        </TableBody>
      </Table>
    </TableContainer>
  );
}
```

----------------------------------------

TITLE: Implementing Conditional Rendering with MUI Transition - JSX
DESCRIPTION: This example demonstrates how to integrate conditional rendering within an MUI `Transition` component. The inner component (`<div>`) will only be rendered and animated when the `show` state is `true`, allowing for dynamic appearance and disappearance based on a specific condition.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/blog/mui-transitions.mdx#_snippet_4

LANGUAGE: JSX
CODE:
```
<Transition in={show} timeout={500}>
    
  {show && (
    <div>This component will appear or disappear based on the show state.</div>
  )}
</Transition>
```

----------------------------------------

TITLE: Defining a Primary Color Variable in Sass
DESCRIPTION: This snippet demonstrates how to define a reusable variable in Sass. Variables allow for consistent styling across a project by storing values like colors, fonts, or sizes, which can then be referenced and easily updated throughout the stylesheet.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/blog/react-hero-component.mdx#_snippet_0

LANGUAGE: SCSS
CODE:
```
$primary-color: #3498db;
```

----------------------------------------

TITLE: Integrating MUI and MagicUI Buttons in React
DESCRIPTION: This snippet demonstrates how to use both Material-UI (MUI) and MagicUI button components within a single React component. It showcases the seamless integration of MagicUI components with existing MUI projects, highlighting their responsive design capabilities.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/blog/mui-transitions.mdx#_snippet_23

LANGUAGE: javascript
CODE:
```
import { Button } from "@mui/material";
import { MagicUIButton } from "@magicui/react";

function MyComponent() {
  return (
    <div>
      <Button variant="contained">MUI Button</Button>
      <MagicUIButton variant="primary">MagicUI Button</MagicUIButton>
    </div>
  );
}
```

----------------------------------------

TITLE: Configuring Magic UI MCP Server Manually (JSON)
DESCRIPTION: This JSON configuration snippet is used to manually add the Magic UI MCP server to an IDE's configuration file. It specifies that the server should be launched using npx to execute the latest @magicuidesign/mcp package, enabling the IDE to access Magic UI components for AI-assisted generation.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/mcp.mdx#_snippet_5

LANGUAGE: json
CODE:
```
{
  "mcpServers": {
    "@magicuidesign/mcp": {
      "command": "npx",
      "args": ["-y", "@magicuidesign/mcp@latest"]
    }
  }
}
```

----------------------------------------

TITLE: Creating Responsive Text Layouts with Tailwind CSS in React
DESCRIPTION: This React component demonstrates how to make text responsive using Tailwind CSS utility classes. It applies `text-3xl` for mobile screens and scales up to `md:text-5xl` for larger screens, showcasing Tailwind's mobile-first approach. It also includes a basic button and overall page layout.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/blog/tailwind-font-size.mdx#_snippet_3

LANGUAGE: TypeScript
CODE:
```
import "./App.css";
import "./index.css";
function App() {
  return (
    <div className="w-full flex flex-col items-center justify-center h-screen bg-gray-100 gap-4">
      <div className="max-w-xl text-center">
        <h1 className="text-3xl font-bold underline text-center mb-4 md:text-5xl">
          Welcome to Tailwind CSS
        </h1>
        <p className="text-lg text-gray-600 md:text-2xl">
          This is a sample text layout using Tailwind CSS, an utility-first CSS
          framework for rapidly building custom designs.
        </p>
        <div className="mt-2">
          <button className="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded">
            Get Started
          </button>
        </div>
      </div>
    </div>
  );
}
export default App;
```

----------------------------------------

TITLE: Defining Global Tailwind CSS Styles (CSS)
DESCRIPTION: This CSS snippet defines the core Tailwind CSS directives, importing base, components, and utilities styles into a `globals.css` file. This file is typically imported once to apply global styling.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/installation/astro.mdx#_snippet_4

LANGUAGE: css
CODE:
```
@tailwind base;
@tailwind components;
@tailwind utilities;
```

----------------------------------------

TITLE: Initializing Next.js Project with shadcn/ui (bash)
DESCRIPTION: This command initializes a new Next.js project or sets up an existing one for use with shadcn/ui components. It's the first step in integrating MagicUI, which shares the same installation process.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/installation/index.mdx#_snippet_0

LANGUAGE: bash
CODE:
```
npx shadcn@latest init
```

----------------------------------------

TITLE: Creating Laravel Project with Inertia and React (Bash)
DESCRIPTION: This command initializes a new Laravel project named 'my-app' using the Laravel installer. It includes TypeScript support, Breeze starter kit, React stack for Inertia, initializes a Git repository, and runs without interactive prompts.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/installation/laravel.mdx#_snippet_0

LANGUAGE: bash
CODE:
```
laravel new my-app --typescript --breeze --stack=react --git --no-interaction
```

----------------------------------------

TITLE: Enable Tailwind and PostCSS in Remix Config (JS)
DESCRIPTION: Modify the `remix.config.js` file to enable Tailwind CSS and PostCSS integration.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/installation/remix.mdx#_snippet_5

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

TITLE: Creating React Project with Vite (Bash)
DESCRIPTION: This command initializes a new React project using the latest version of Vite. It sets up the basic project structure and necessary dependencies for a React application.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/installation/vite.mdx#_snippet_0

LANGUAGE: bash
CODE:
```
npm create vite@latest
```

----------------------------------------

TITLE: Installing React Router DOM for Navigation
DESCRIPTION: This command installs the 'react-router-dom' package, which is essential for handling client-side routing in a React application. It enables navigation between different pages or components without full page reloads.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/blog/react-header.mdx#_snippet_2

LANGUAGE: Shell
CODE:
```
npm install react-router-dom
```

----------------------------------------

TITLE: Configuring MUI Transition with `in` Prop - JSX
DESCRIPTION: This example shows how to use the `Transition` component with the `in` prop, which controls the visibility and triggers the animation. When `in` is `true`, the component appears; when `false`, it disappears. The `timeout` prop sets the duration of the transition in milliseconds.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/blog/mui-transitions.mdx#_snippet_2

LANGUAGE: JSX
CODE:
```
<Transition in={show} timeout={500}>
    {/* Your component */}
</Transition>
```

----------------------------------------

TITLE: Using Custom Tailwind CSS Font Sizes in React Component
DESCRIPTION: This React component demonstrates the usage of custom font sizes defined in `tailwind.config.js`. It applies the `text-xxs` and `text-xxl` utility classes to paragraph elements, showcasing how to integrate extended font sizes into a UI.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/blog/tailwind-font-size.mdx#_snippet_5

LANGUAGE: TypeScript
CODE:
```
function CustomFontSizeComponent() {
  return (
    <div className="text-center">
      <p className="text-xxs">This is extra extra small text.</p>
      <p className="text-xxl">This is extra extra large text.</p>
    </div>
  );
}
export default CustomFontSizeComponent;
```

----------------------------------------

TITLE: Integrating a React Card Component into App.tsx
DESCRIPTION: This code imports the previously defined `Card` component into the main `App` component of a React application. It renders the `Card` within a full-width, centered container, demonstrating how to compose UI elements and apply global styling using Tailwind CSS for layout.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/blog/install-tailwind-react.mdx#_snippet_10

LANGUAGE: javascript
CODE:
```
import "./App.css";
import Card from "./components/Card";
import "./index.css";
function App() {
  return (
    <div className="w-full flex flex-col items-center justify-center h-screen bg-gray-100">
      <Card />
    </div>
  );
}
export default App;
```

----------------------------------------

TITLE: Applying Custom Styles with sx Prop to MUI Box (JSX)
DESCRIPTION: This snippet illustrates how to use the `sx` prop on a Material UI `Box` component to apply inline styles. It sets the background color to the theme's `primary.main` color and applies a margin of 1 unit, demonstrating a concise way to style components.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/blog/mui-box.mdx#_snippet_7

LANGUAGE: jsx
CODE:
```
<Box component="span" sx={{ backgroundColor: "primary.main", margin: 1 }}>
  This is a box with a primary color background and a small margin
</Box>
```

----------------------------------------

TITLE: Creating a New Astro Project (Bash)
DESCRIPTION: This command initializes a new Astro project, prompting the user for project details and configuration options.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/installation/astro.mdx#_snippet_0

LANGUAGE: bash
CODE:
```
npm create astro@latest
```

----------------------------------------

TITLE: Create Remix Project (Bash)
DESCRIPTION: Start a new Remix project using the `create-remix` command-line tool.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/installation/remix.mdx#_snippet_0

LANGUAGE: bash
CODE:
```
npx create-remix@latest my-app
```

----------------------------------------

TITLE: Creating a Row-Based Grid Gallery in React with Tailwind CSS
DESCRIPTION: This snippet modifies the `CardGallery` component to create a row-based grid layout. By applying `grid-rows-3` and `grid-flow-col` alongside `gap-4`, cards are stacked vertically in three rows before moving to the next column, demonstrating a row-first grid flow.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/blog/tailwind-css-grid.mdx#_snippet_4

LANGUAGE: tsx
CODE:
```
import { FC } from "react";
import Card from "./Card";
const CardGallery: FC<any> = () => {
  return (
    <div className="grid grid-rows-3 grid-flow-col gap-4">
      <Card />
      <Card />
      <Card />
      <Card />
    </div>
  );
};
export default CardGallery;
```

----------------------------------------

TITLE: Installing MUI React Components via npm
DESCRIPTION: This command installs the core Material UI package along with its peer dependencies, @emotion/react and @emotion/styled, which are required for styling. This is the first step to integrate MUI components into a React project.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/blog/mui-react.mdx#_snippet_0

LANGUAGE: Shell
CODE:
```
npm install @mui/material @emotion/react @emotion/styled
```

----------------------------------------

TITLE: Basic Dock Component Usage (TSX)
DESCRIPTION: This snippet illustrates the basic JSX structure for rendering the `Dock` component. It shows how to nest `DockIcon` components within `Dock`, and how to place individual icon components (e.g., `Home`, `Settings`, `Search`) inside `DockIcon` to create the interactive dock elements.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/dock.mdx#_snippet_2

LANGUAGE: tsx
CODE:
```
<Dock>
  <DockIcon>
    <Home />
    <Settings />
    <Search />
  </DockIcon>
</Dock>
```

----------------------------------------

TITLE: Importing Orbiting Circles and Icons in TSX
DESCRIPTION: This snippet imports the `OrbitingCircles` component from the magicui library and several icon components (`File`, `Settings`, `Search`) from `lucide-react`. These imports are necessary to use the orbiting circles and display icons within them in a React/Next.js application.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/orbiting-circles.mdx#_snippet_2

LANGUAGE: tsx
CODE:
```
import { OrbitingCircles } from "@/components/magicui/orbiting-circles";
import { File, Settings, Search } from "lucide-react";
```

----------------------------------------

TITLE: Defining CSS Class for Active Element Entering Transition
DESCRIPTION: This CSS snippet defines the active state for an element during its entering transition. It sets `opacity: 1;` and applies a `transition` property to smoothly animate the opacity over 500 milliseconds, creating a fade-in effect.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/blog/mui-transitions.mdx#_snippet_17

LANGUAGE: css
CODE:
```
.fade-enter-active {
	opacity: 1;

  transition: opacity 500ms;
}
```

----------------------------------------

TITLE: Implementing a Basic High-Order Component (HOC) in React
DESCRIPTION: This snippet demonstrates the basic structure of a High-Order Component (HOC) in React. HOCs are functions that take a component and return a new component with enhanced functionality, promoting code reusability and separation of concerns. This example shows a simple HOC that wraps a `DecoratedComponent`.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/blog/react-design-patterns.mdx#_snippet_0

LANGUAGE: tsx
CODE:
```
import React, { Component } from "react";
const HigherOrderComponent = (DecoratedComponent) => {
  class HOC extends Component {
    render() {
      return <DecoratedComponent />;
    }
  }
  return HOC;
};
```

----------------------------------------

TITLE: Defining Keyframes for Fade-In and Rotate Animation in CSS
DESCRIPTION: This CSS '@keyframes' rule defines the 'fadeInRotate' animation sequence, specifying the element's appearance at different stages. It transitions the text from invisible and unrotated (0%) to fully visible with a 180-degree rotation (50%), and finally to fully visible with a 360-degree rotation (100%), creating a combined fading and spinning effect.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/blog/text-animation-css.mdx#_snippet_2

LANGUAGE: CSS
CODE:
```
@keyframes fadeInRotate {
  0% {
    opacity: 0;
    transform: rotate(0deg);
  }
  50% {
    opacity: 1;
    transform: rotate(180deg);
  }
  100% {
    opacity: 1;
    transform: rotate(360deg);
  }
}
```

----------------------------------------

TITLE: Basic Tailwind CSS Grid Layout in React HTML
DESCRIPTION: This snippet illustrates the fundamental usage of Tailwind CSS Grid within a React component's JSX. It defines a parent `div` containing two grid examples: one with three columns (`grid-cols-3`) and another with three rows (`grid-rows-3`), both applying a uniform gap of 2 units (`gap-2`) between grid items. This demonstrates how to quickly structure elements into 2D layouts using Tailwind's utility classes.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/blog/tailwind-css-grid.mdx#_snippet_0

LANGUAGE: HTML
CODE:
```
<div>
  <div className="grid grid-cols-3 gap-2"></div>
  <div className="grid grid-rows-3 gap-2"></div>
</div>
```

----------------------------------------

TITLE: Adding a shadcn/ui Component (Button) to Next.js
DESCRIPTION: This `npx` command adds a specific component, in this case, the `Button`, from the shadcn/ui library to the current Next.js project. It integrates the component's source files and dependencies, making it ready for use.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/installation/next.mdx#_snippet_3

LANGUAGE: bash
CODE:
```
npx shadcn@latest add button
```

----------------------------------------

TITLE: Implementing Hover-Based Border Radius Change in React with Tailwind
DESCRIPTION: This snippet illustrates how to create an interactive card component in React where the border-radius changes on hover. Initially, the card has 'rounded-lg' corners, which transition to 'rounded-none' (sharp corners) when the user hovers over it, demonstrating dynamic styling with Tailwind CSS.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/blog/tailwind-border-radius.mdx#_snippet_2

LANGUAGE: tsx
CODE:
```
import "./App.css";
import "./index.css";
function App() {
  return (
    <div className="w-full flex flex-rows items-center justify-center h-screen bg-gray-100 gap-4">
      <div className="bg-white p-6 rounded-lg shadow-md hover:rounded-none">
        <h2 className="text-xl font-bold">Card Title</h2>
        <p className="text-gray-700">This is a card with a border radius.</p>
      </div>
    </div>
  );
}
export default App;
```

----------------------------------------

TITLE: Using Warp Background Component with Children in TypeScript/React
DESCRIPTION: This snippet illustrates how to embed content within the `WarpBackground` component, demonstrating its use as a wrapper for other React elements to apply the warp effect to its children.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/warp-background.mdx#_snippet_2

LANGUAGE: tsx
CODE:
```
<WarpBackground>
  <div className="w-80">
    <p>Warp Background</p>
    <p>This is a component that creates a warp background effect.</p>
  </div>
</WarpBackground>
```

----------------------------------------

TITLE: Defining CSS Class for Element Exiting State
DESCRIPTION: This CSS snippet defines the initial state for an element as it begins to exit a transition. Setting `opacity: 1;` ensures the element is fully visible at the start of its exiting animation.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/blog/mui-transitions.mdx#_snippet_18

LANGUAGE: css
CODE:
```
.fade-exit {
  opacity: 1;
}
```

----------------------------------------

TITLE: Custom Styled Material UI Box in React (JSX)
DESCRIPTION: This example shows a more complete React application setup, rendering an MUI `Box` component. It imports `Box` from `@material-ui/core` and applies custom inline styles using the `sx` prop for padding and a dashed border, then renders the `App` component to the DOM.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/blog/mui-box.mdx#_snippet_6

LANGUAGE: jsx
CODE:
```
import React from "react";
import ReactDOM from "react-dom";
import { Box } from "@material-ui/core";
import "./styles.css";

function App() {
  return (
    <Box component="section" sx={{ p: 20, border: "1px dashed grey" }}>
      This is an MUI Box
    </Box>
  );
}

const rootElement = document.getElementById("root");
ReactDOM.render(<App />, rootElement);
```

----------------------------------------

TITLE: Installing Core Dependencies (Bash)
DESCRIPTION: This command installs essential npm packages required for the project's styling and utility functions. These include tailwindcss-animate for animations, class-variance-authority for managing component variants, clsx for conditionally joining class names, and tailwind-merge for merging Tailwind CSS classes without conflicts.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/installation/manual.mdx#_snippet_0

LANGUAGE: bash
CODE:
```
npm install tailwindcss-animate class-variance-authority clsx tailwind-merge
```

----------------------------------------

TITLE: Configure PostCSS (JS)
DESCRIPTION: Create or update the `postcss.config.js` file to include Tailwind CSS and Autoprefixer plugins.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/installation/remix.mdx#_snippet_4

LANGUAGE: js
CODE:
```
export default {
  plugins: {
    tailwindcss: {},
    autoprefixer: {},
  },
};
```

----------------------------------------

TITLE: Generating Tailwind CSS Configuration Files (Bash)
DESCRIPTION: This command initializes Tailwind CSS, creating `tailwind.config.js` and `postcss.config.js` files. The `-p` flag ensures that PostCSS configuration is also generated, which is crucial for integrating Tailwind CSS with other PostCSS plugins.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/blog/tailwind-landing-page.mdx#_snippet_1

LANGUAGE: bash
CODE:
```
npx tailwindcss init -p
```

----------------------------------------

TITLE: Formatting Code with pnpm
DESCRIPTION: This command automatically formats the codebase according to predefined style guidelines, ensuring consistency and readability across the project. It should be run before committing changes.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/CONTRIBUTING.md#_snippet_14

LANGUAGE: bash
CODE:
```
pnpm format:write
```

----------------------------------------

TITLE: Configuring Gatsby project settings
DESCRIPTION: Details the interactive prompts and typical responses when configuring a new Gatsby project, including site name, folder, choice of TypeScript, CMS integration, styling system (Tailwind CSS), and additional features.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/installation/gatsby.mdx#_snippet_1

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

TITLE: Installing Material-UI Core Components with npm
DESCRIPTION: This npm command adds the core Material-UI library (@mui/material) along with its styling engine dependencies (@emotion/react, @emotion/styled) to your project, enabling the use of Material-UI components.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/blog/mui-table.mdx#_snippet_1

LANGUAGE: bash
CODE:
```
npm install @mui/material @emotion/react @emotion/styled
```

----------------------------------------

TITLE: Creating a new Gatsby project
DESCRIPTION: Initializes a new Gatsby project using the `create-gatsby` command. This command sets up the basic project structure and necessary files for a Gatsby application.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/installation/gatsby.mdx#_snippet_0

LANGUAGE: bash
CODE:
```
npm init gatsby
```

----------------------------------------

TITLE: Importing and Using shadcn/ui Button Component (TypeScript/React)
DESCRIPTION: This TypeScript/React snippet demonstrates how to import the `Button` component from the `shadcn/ui` library and use it within a React functional component. It shows a basic usage of the button with 'Click me' as its text content.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/installation/laravel.mdx#_snippet_4

LANGUAGE: tsx
CODE:
```
import { Button } from "@/Components/ui/button";

export default function Home() {
  return (
    <div>
      <Button>Click me</Button>
    </div>
  );
}
```

----------------------------------------

TITLE: Installing Tailwind CSS Dependencies (Bash)
DESCRIPTION: This command installs Tailwind CSS, PostCSS, and Autoprefixer as development dependencies. These packages are essential for compiling and processing Tailwind CSS in your project, enabling features like automatic vendor prefixing and custom CSS transformations.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/blog/tailwind-landing-page.mdx#_snippet_0

LANGUAGE: bash
CODE:
```
npm install -D tailwindcss postcss autoprefixer
```

----------------------------------------

TITLE: Initialize shadcn/ui (Bash)
DESCRIPTION: Run the shadcn/ui initialization command to configure the project settings.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/installation/remix.mdx#_snippet_1

LANGUAGE: bash
CODE:
```
npx shadcn@latest init
```

----------------------------------------

TITLE: Initializing shadcn/ui Project (Bash)
DESCRIPTION: This command executes the `shadcn-ui` initialization script, which guides the user through setting up the UI component library in their project. It configures essential files and directories for shadcn/ui integration.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/installation/vite.mdx#_snippet_6

LANGUAGE: bash
CODE:
```
npx shadcn@latest init
```

----------------------------------------

TITLE: Adding React Integration to Astro (Bash)
DESCRIPTION: This command uses the Astro CLI to install and configure React support within an existing Astro project. Users should answer 'Yes' to all subsequent prompts.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/installation/astro.mdx#_snippet_2

LANGUAGE: bash
CODE:
```
npx astro add react
```

----------------------------------------

TITLE: Initializing a React and TypeScript Project
DESCRIPTION: This command initializes a new React project named 'my-tailwind-app' using Create React App, pre-configured with the TypeScript template. It sets up the basic project structure and necessary dependencies for a React and TypeScript application.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/blog/install-tailwind-react.mdx#_snippet_0

LANGUAGE: javascript
CODE:
```
npx create-react-app my-tailwind-app --template typescript
```

----------------------------------------

TITLE: Using Bento Grid and Bento Card Components (TypeScript/TSX)
DESCRIPTION: This snippet demonstrates the basic JSX structure for rendering the `BentoGrid` component, which acts as a container for `BentoCard` elements. It shows how to compose a simple Bento Grid layout.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/bento-grid.mdx#_snippet_2

LANGUAGE: tsx
CODE:
```
<BentoGrid>
  <BentoCard />
</BentoGrid>
```

----------------------------------------

TITLE: Default Next.js Project Folder Structure (JavaScript)
DESCRIPTION: This snippet illustrates the default folder structure of a newly created Next.js project. It highlights the core Next.js-specific folders: `/app` (or `/pages` in older versions), `/public`, and `/styles`, which are crucial for the application's functionality and should not be renamed without configuration adjustments.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/blog/next-js-app.mdx#_snippet_1

LANGUAGE: javascript
CODE:
```
# other files and folders, .gitignore, package.json...
/app
├── api
│   └── hello.js
├── page.js
/public
├── favicon.ico
├── vercel.svg
/styles
├── globals.css
└── Home.module.css
```

----------------------------------------

TITLE: Defining CSS Root Color Variables
DESCRIPTION: These CSS variables define a set of HSL color values within the `:root` pseudo-class, intended for use in the rainbow animation. They provide the base colors for the animated effect.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/rainbow-button.mdx#_snippet_1

LANGUAGE: css
CODE:
```
:root {
  --color-1: 0 100% 63%;
  --color-2: 270 100% 63%;
  --color-3: 210 100% 63%;
  --color-4: 195 100% 63%;
  --color-5: 90 100% 63%;
}
```

----------------------------------------

TITLE: Defining CSS Keyframes for Fade Animations
DESCRIPTION: This CSS snippet defines two keyframe animations: `fadeIn` and `fadeOut`. `fadeIn` transitions an element's opacity from 0 to 1, while `fadeOut` transitions it from 1 to 0, providing smooth visual effects for entering and exiting elements.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/blog/mui-transitions.mdx#_snippet_14

LANGUAGE: css
CODE:
```
@keyframes fadeIn {
  from {
    opacity: 0;
  }
  to {
    opacity: 1;
  }
}

@keyframes fadeOut {
  from {
    opacity: 1;
  }
  to {
    opacity: 0;
  }
}
```

----------------------------------------

TITLE: Configuring TypeScript path aliases in Gatsby
DESCRIPTION: Adds `baseUrl` and `paths` configurations to the `tsconfig.json` file. This enables absolute path imports like `@/*` for source files, simplifying module resolution within the project.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/installation/gatsby.mdx#_snippet_2

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

TITLE: Installing Tailwind CSS and Peer Dependencies
DESCRIPTION: This command installs Tailwind CSS along with its peer dependencies: PostCSS and Autoprefixer. PostCSS enables Tailwind to generate CSS using JavaScript, while Autoprefixer automatically adds vendor prefixes for cross-browser compatibility.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/blog/install-tailwind-react.mdx#_snippet_4

LANGUAGE: javascript
CODE:
```
npm install tailwindcss postcss autoprefixer
```

----------------------------------------

TITLE: Rebuilding Android App Binary
DESCRIPTION: This command rebuilds and runs the React Native application on an Android emulator or device. It's required after installing libraries with native Android dependencies to ensure they are correctly linked and available within the app.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/blog/react-native-libraries.mdx#_snippet_3

LANGUAGE: Bash
CODE:
```
npm run android
```

----------------------------------------

TITLE: Linking iOS Native Code with npx pod-install
DESCRIPTION: This command uses `npx` to execute `pod-install`, which runs `pod install` in the iOS directory of a React Native project. It's used to link native iOS dependencies managed by CocoaPods after installing a library.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/blog/react-native-libraries.mdx#_snippet_1

LANGUAGE: Bash
CODE:
```
npx pod-install
```

----------------------------------------

TITLE: Configuring shadcn-ui components.json
DESCRIPTION: Outlines the interactive prompts for configuring `components.json` during the `shadcn-ui` setup. This covers preferences such as TypeScript usage, styling, base color, global CSS file path, Tailwind CSS configuration, and import aliases.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/installation/gatsby.mdx#_snippet_5

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

TITLE: Importing and Using Shadcn Button in Astro (Astro)
DESCRIPTION: This Astro component snippet demonstrates how to import the `Button` component from `shadcn/ui` and then use it within an Astro page's HTML structure, displaying 'Hello World'.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/installation/astro.mdx#_snippet_10

LANGUAGE: astro
CODE:
```
---
import { Button } from "@/components/ui/button"
---

<html lang="en">
	<head>
		<title>Astro</title>
	</head>
	<body>
		<Button>Hello World</Button>
	</body>
</html>
```

----------------------------------------

TITLE: Using RainbowButton Component
DESCRIPTION: This JSX snippet demonstrates how to render the `RainbowButton` component. It accepts children, which will be displayed as the button's text, and can be customized with various props like `variant` and `size`.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/rainbow-button.mdx#_snippet_4

LANGUAGE: tsx
CODE:
```
<RainbowButton>Rainbow Button</RainbowButton>
```

----------------------------------------

TITLE: Basic File Tree Structure (TSX)
DESCRIPTION: This snippet illustrates a basic hierarchical structure using the `Tree`, `Folder`, and `File` components. It demonstrates how to nest folders and files to represent a directory structure within a React/TSX application.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/file-tree.mdx#_snippet_2

LANGUAGE: tsx
CODE:
```
<Tree>\n  <Folder>\n    <Folder>\n      <File>layout.tsx</File>\n      <File>page.tsx</File>\n    </Folder>\n  </Folder>\n</Tree>
```

----------------------------------------

TITLE: Importing and Using shadcn/ui Button in Next.js
DESCRIPTION: This TypeScript React (TSX) code demonstrates how to import the `Button` component from the shadcn/ui library into a Next.js application. It shows a basic functional component rendering the imported `Button` with sample text.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/installation/next.mdx#_snippet_4

LANGUAGE: tsx
CODE:
```
import { Button } from "@/components/ui/button";

export default function Home() {
  return (
    <div>
      <Button>Click me</Button>
    </div>
  );
}
```

----------------------------------------

TITLE: Installing React Native Webview Library
DESCRIPTION: This command installs the `react-native-webview` library into your React Native project using npm. It's a common first step for integrating third-party UI components or modules. After installation, platform-specific linking (e.g., CocoaPods for iOS, Gradle for Android) and app rebuilding may be required.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/blog/best-react-native-ui-library.mdx#_snippet_0

LANGUAGE: Shell
CODE:
```
npm install react-native-webview
```

----------------------------------------

TITLE: Importing and Using shadcn/ui Button Component (TSX)
DESCRIPTION: This TSX snippet demonstrates how to import and use the `Button` component from shadcn/ui within a React functional component. It shows a basic usage of the `Button` component, rendering it with 'Click me' as its child text.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/installation/vite.mdx#_snippet_9

LANGUAGE: tsx
CODE:
```
import { Button } from "@/components/ui/button";

export default function Home() {
  return (
    <div>
      <Button>Click me</Button>
    </div>
  );
}
```

----------------------------------------

TITLE: Installing ScriptCopyBtn via CLI
DESCRIPTION: This snippet demonstrates how to install the `ScriptCopyBtn` component using the `shadcn/ui` command-line interface (CLI). This is the recommended and most straightforward installation method.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/script-copy-btn.mdx#_snippet_0

LANGUAGE: bash
CODE:
```
npx shadcn@latest add "https://magicui.design/r/script-copy-btn"
```

----------------------------------------

TITLE: Installing MorphingText Component via CLI
DESCRIPTION: This snippet demonstrates how to add the `MorphingText` component to your project using the `shadcn` CLI. It fetches the component directly from the specified Magic UI registry URL, simplifying the installation process.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/morphing-text.mdx#_snippet_0

LANGUAGE: bash
CODE:
```
npx shadcn@latest add "https://magicui.design/r/morphing-text"
```

----------------------------------------

TITLE: Installing Interactive Hover Button via CLI (Bash)
DESCRIPTION: This snippet demonstrates how to install the InteractiveHoverButton component using the shadcn/ui CLI. Executing this command fetches and integrates the component's code into your project, streamlining the setup process.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/interactive-hover-button.mdx#_snippet_0

LANGUAGE: bash
CODE:
```
npx shadcn@latest add "https://magicui.design/r/interactive-hover-button"
```

----------------------------------------

TITLE: Adding a Shadcn UI Component (Bash)
DESCRIPTION: This command uses the `shadcn` CLI to add a specific component, like 'Button', to the project, making it available for import and use in Astro components.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/installation/astro.mdx#_snippet_9

LANGUAGE: bash
CODE:
```
npx shadcn@latest add button
```

----------------------------------------

TITLE: Installing CodeComparison Component via CLI
DESCRIPTION: This command installs the CodeComparison component using the `shadcn` CLI, adding it to your project. It fetches the component from the specified MagicUI design URL, streamlining the setup process.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/code-comparison.mdx#_snippet_0

LANGUAGE: bash
CODE:
```
npx shadcn@latest add "https://magicui.design/r/code-comparison"
```

----------------------------------------

TITLE: Installing Terminal Component via CLI (Bash)
DESCRIPTION: This command uses `npx` to add the `Terminal` component from MagicUI to your project. It leverages the `shadcn` CLI for easy integration, fetching the component from the specified URL. This is the recommended and quickest way to get started.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/terminal.mdx#_snippet_0

LANGUAGE: bash
CODE:
```
npx shadcn@latest add "https://magicui.design/r/terminal"
```

----------------------------------------

TITLE: Installing Dock Component via CLI (Bash)
DESCRIPTION: This snippet demonstrates how to install the MagicUI Dock component using the `npx shadcn@latest add` command-line interface. This is the recommended and easiest way to add the component to your project, automatically handling dependencies and file placement.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/dock.mdx#_snippet_0

LANGUAGE: bash
CODE:
```
npx shadcn@latest add "https://magicui.design/r/dock"
```

----------------------------------------

TITLE: Installing Warp Background Component via CLI
DESCRIPTION: This snippet demonstrates how to install the Warp Background component using the `shadcn` CLI, which simplifies adding UI components to your project by fetching them from a specified URL.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/warp-background.mdx#_snippet_0

LANGUAGE: bash
CODE:
```
npx shadcn@latest add "https://magicui.design/r/warp-background"
```

----------------------------------------

TITLE: Installing Neon Gradient Card via CLI
DESCRIPTION: This snippet provides the command-line interface (CLI) command to add the Neon Gradient Card component to your project using `npx shadcn`.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/neon-gradient-card.mdx#_snippet_0

LANGUAGE: bash
CODE:
```
npx shadcn@latest add "https://magicui.design/r/neon-gradient-card"
```

----------------------------------------

TITLE: Installing Shine Border via CLI
DESCRIPTION: This command uses `npx shadcn` to add the Shine Border component to your project, simplifying the installation process by fetching the component from the specified URL.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/shine-border.mdx#_snippet_0

LANGUAGE: bash
CODE:
```
npx shadcn@latest add "https://magicui.design/r/shine-border"
```

----------------------------------------

TITLE: Adding shadcn-ui components to Gatsby
DESCRIPTION: Demonstrates how to add a specific `shadcn-ui` component, such as the `Button`, to the project using the `shadcn@latest add` command. This makes the component available for use in your Gatsby application.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/installation/gatsby.mdx#_snippet_6

LANGUAGE: bash
CODE:
```
npx shadcn@latest add button
```

----------------------------------------

TITLE: Installing Number Ticker Component via CLI (Bash)
DESCRIPTION: This command-line interface (CLI) snippet demonstrates how to quickly add the `NumberTicker` component to your project using `npx shadcn`. It fetches the component directly from the specified MagicUI design URL, streamlining the installation process.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/number-ticker.mdx#_snippet_0

LANGUAGE: bash
CODE:
```
npx shadcn@latest add "https://magicui.design/r/number-ticker"
```

----------------------------------------

TITLE: Installing Lens Component using CLI
DESCRIPTION: This command uses `npx shadcn` to add the Lens component to your project. It fetches the component from the specified MagicUI design URL, automating the setup process and integrating it into your development environment.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/lens.mdx#_snippet_0

LANGUAGE: bash
CODE:
```
npx shadcn@latest add "https://magicui.design/r/lens"
```

----------------------------------------

TITLE: Install Animated Beam Component via CLI
DESCRIPTION: Installs the Animated Beam component using the shadcn/ui CLI extension for magicui, fetching it directly from the specified URL. This is the recommended method for adding the component to a project.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/animated-beam.mdx#_snippet_0

LANGUAGE: bash
CODE:
```
npx shadcn@latest add "https://magicui.design/r/animated-beam"
```

----------------------------------------

TITLE: Adding a Header to Material UI Card with CardHeader (JSX)
DESCRIPTION: This snippet illustrates the use of the `CardHeader` component to add contextual information to a Material UI Card. It shows how to include an avatar, title, subheader, and an action button within the card's header.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/blog/mui-card.mdx#_snippet_1

LANGUAGE: jsx
CODE:
```
import { CardHeader, Avatar, IconButton } from "@mui/material";
import Card from "@mui/material/Card";
<Card>
  <CardHeader
    avatar={<Avatar aria-label="card-name">C</Avatar>}
    title="Card Title"
    subheader="Subheader"
    action={<IconButton aria-label="settings"></IconButton>}
  />
</Card>;
```

----------------------------------------

TITLE: Installing Component via CLI - Bash
DESCRIPTION: This Bash command demonstrates how users can install the new MagicUI component using the `shadcn` CLI tool. It fetches the component from a specified URL and adds it to the user's project.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/CONTRIBUTING.md#_snippet_9

LANGUAGE: Bash
CODE:
```
npx shadcn@latest add "https://magicui.design/r/example-component"
```

----------------------------------------

TITLE: Installing Rainbow Button via CLI
DESCRIPTION: This command uses the `shadcn` CLI to automatically add the `RainbowButton` component to your project, simplifying the installation process.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/rainbow-button.mdx#_snippet_0

LANGUAGE: bash
CODE:
```
npx shadcn@latest add "https://magicui.design/r/rainbow-button"
```

----------------------------------------

TITLE: Installing FlipText Component via CLI
DESCRIPTION: This command uses `npx shadcn` to add the `FlipText` component directly from the MagicUI repository. It's the recommended way for quick setup, automating the process of adding the component to your project.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/flip-text.mdx#_snippet_0

LANGUAGE: bash
CODE:
```
npx shadcn@latest add "https://magicui.design/r/flip-text"
```

----------------------------------------

TITLE: Installing Ripple Component via CLI (Bash)
DESCRIPTION: This snippet provides the command-line interface (CLI) instruction to add the Ripple component to your project using the `shadcn` utility. This is the recommended and easiest way to integrate the component.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/ripple.mdx#_snippet_0

LANGUAGE: bash
CODE:
```
npx shadcn@latest add "https://magicui.design/r/ripple"
```

----------------------------------------

TITLE: Installing MagicUI SpinningText Component via CLI (Bash)
DESCRIPTION: This snippet provides the command-line interface (CLI) instruction to quickly add the `SpinningText` component from MagicUI to your project using `npx shadcn`.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/spinning-text.mdx#_snippet_0

LANGUAGE: bash
CODE:
```
npx shadcn@latest add \"https://magicui.design/r/spinning-text\"
```

----------------------------------------

TITLE: Adding 'bento-grid' magicui Component - Bash Example
DESCRIPTION: An example demonstrating how to add the 'bento-grid' component using the `magicui-cli add` command, which will also install its dependencies.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/packages/cli/README.md#_snippet_4

LANGUAGE: bash
CODE:
```
npx magicui-cli add bento-grid
```

----------------------------------------

TITLE: Installing HyperText Component via CLI (Bash)
DESCRIPTION: This `npx shadcn` command adds the `HyperText` component from the `magicui.design` repository to your project. It's the recommended method for integrating the component quickly.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/hyper-text.mdx#_snippet_0

LANGUAGE: bash
CODE:
```
npx shadcn@latest add "https://magicui.design/r/hyper-text"
```

----------------------------------------

TITLE: Installing iPhone 15 Pro Component via CLI (Bash)
DESCRIPTION: This snippet provides the command-line instruction to install the `Iphone15Pro` component using `npx shadcn`. This method automates the process of adding the component to your project from the specified MagicUI repository.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/iphone-15-pro.mdx#_snippet_0

LANGUAGE: bash
CODE:
```
npx shadcn@latest add "https://magicui.design/r/iphone-15-pro"
```

----------------------------------------

TITLE: Installing Lucide React Icon Library (Bash)
DESCRIPTION: This command installs the `lucide-react` package, which is the recommended icon library for projects using the `default` style. It provides a collection of customizable SVG icons for React applications.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/installation/manual.mdx#_snippet_1

LANGUAGE: bash
CODE:
```
npm install lucide-react
```

----------------------------------------

TITLE: Importing Tailwind CSS into Main Stylesheet
DESCRIPTION: This CSS snippet, intended for `src/index.css`, imports Tailwind's base styles, component styles, and utility classes into the project. These directives are processed by PostCSS to inject the generated Tailwind CSS into the application's main stylesheet.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/blog/install-tailwind-react.mdx#_snippet_7

LANGUAGE: css
CODE:
```
@tailwind base;
@tailwind components;
@tailwind utilities;
```

----------------------------------------

TITLE: Implementing Fade Transition in MagicUI (React)
DESCRIPTION: Demonstrates how to use the Fade component within a Transition wrapper to create a simple fade-in and fade-out effect. This is ideal for elements that appear or disappear, providing a smooth visual cue. The in prop controls visibility, and timeout sets the animation duration.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/blog/mui-transitions.mdx#_snippet_5

LANGUAGE: JavaScript
CODE:
```
<Transition in={show} timeout={500}>\n  <Fade in={show}>{/* Your content */}</Fade>\n</Transition>
```

----------------------------------------

TITLE: Initializing Shadcn UI Project (Bash)
DESCRIPTION: This command runs the `shadcn` init command, setting up the necessary configuration and dependencies for using Shadcn UI components within the project.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/installation/astro.mdx#_snippet_8

LANGUAGE: bash
CODE:
```
npx shadcn@latest init
```

----------------------------------------

TITLE: Adding Actions to Material UI Card with CardActions (JSX)
DESCRIPTION: This snippet demonstrates how to add action buttons or other interactive elements to the bottom of a Material UI Card using the `CardActions` component. It typically contains buttons or links related to the card's content.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/blog/mui-card.mdx#_snippet_4

LANGUAGE: jsx
CODE:
```
import { CardActions, Button } from "@mui/material";
import Card from "@mui/material/Card";
<Card>
  <CardActions>
    <Button>Click here</Button>
  </CardActions>
</Card>;
```

----------------------------------------

TITLE: Implementing Custom JavaScript Transitions with React
DESCRIPTION: This snippet shows how to use the React `Transition` component with custom JavaScript handlers for `onEnter` and `onExit` events. It allows for programmatic control over animations, providing more flexibility than pure CSS transitions, with a `timeout` of 1000ms.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/blog/mui-transitions.mdx#_snippet_20

LANGUAGE: javascript
CODE:
```
<Transition in={show} timeout={1000} onEnter={handleEnter} onExit={handleExit}>
  {/* Your content */}
</Transition>
```

----------------------------------------

TITLE: Customizing MUI Transition with Multiple Props - JSX
DESCRIPTION: This snippet illustrates how to customize an MUI `Transition` using various props. `timeout` sets the duration, `easing` specifies the animation function, `mountOnEnter` mounts the component before animation, `unmountOnExit` unmounts it after animation, and `appear` animates the initial appearance.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/blog/mui-transitions.mdx#_snippet_3

LANGUAGE: JSX
CODE:
```
<Transition
  in={show}
  timeout={1000}
  easing="easeInOutCubic"
  mountOnEnter
  unmountOnExit
  appear
>
   {/* Your component */}
</Transition>
```

----------------------------------------

TITLE: Displaying Multiple Border Radius Variations in React with Tailwind
DESCRIPTION: This code expands on the previous example by showcasing multiple buttons, each demonstrating a different Tailwind CSS border-radius utility class: 'rounded' (default), 'rounded-md', 'rounded-full', and no rounding. It allows for a visual comparison of various corner styles.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/blog/tailwind-border-radius.mdx#_snippet_1

LANGUAGE: tsx
CODE:
```
import "./App.css";
import "./index.css";
function App() {
  return (
    <div className="w-full flex flex-rows items-center justify-center h-screen bg-gray-100 gap-4">
      <button className="bg-blue-500 text-white px-4 py-2">
        No Round Corners
      </button>
      <button className="bg-blue-500 text-white px-4 py-2 rounded">
        'rounded' Corners
      </button>
      <button className="bg-blue-500 text-white px-4 py-2 rounded-md">
        'rounded-md' Corners
      </button>
      <button className="bg-blue-500 text-white px-4 py-2 rounded-full">
        'rounded-full' Corners
      </button>
    </div>
  );
}
export default App;
```

----------------------------------------

TITLE: Importing and using shadcn-ui Button component
DESCRIPTION: Illustrates how to import the `Button` component from `@/components/ui/button` and integrate it into a React component. This snippet shows a basic usage example within a Gatsby page.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/installation/gatsby.mdx#_snippet_7

LANGUAGE: tsx
CODE:
```
import { Button } from "@/components/ui/button";

export default function Home() {
  return (
    <div>
      <Button>Click me</Button>
    </div>
  );
}
```

----------------------------------------

TITLE: Rendering Primary Button with MagicUI (React)
DESCRIPTION: This JavaScript (React) snippet demonstrates how to use a pre-built `Button` component from the MagicUI library. It imports the `Button` component and renders it with a `variant="primary"` prop, showcasing how UI libraries simplify component integration and maintain design consistency.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/blog/ui-animation.mdx#_snippet_2

LANGUAGE: JavaScript
CODE:
```
import { Button } from "magicui";
const MyComponent = () => {
  return <Button variant="primary">Click me</Button>;
};
```

----------------------------------------

TITLE: Rendering Icon Cloud with Images (TypeScript/React)
DESCRIPTION: This snippet demonstrates how to integrate the `IconCloud` component into a React application, wrapping it within a `div` and passing an array of image URLs to the `images` prop to display them in the cloud.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/icon-cloud.mdx#_snippet_2

LANGUAGE: tsx
CODE:
```
<div className="relative overflow-hidden">
  <IconCloud images={images} />
</div>
```

----------------------------------------

TITLE: Importing InteractiveGridPattern Component (TypeScript/TSX)
DESCRIPTION: This snippet shows the necessary import statement for using the `InteractiveGridPattern` component in a TypeScript or TSX file. It specifies the module path from which the component is exported, making it available for use in your application.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/interactive-grid-pattern.mdx#_snippet_1

LANGUAGE: tsx
CODE:
```
import { InteractiveGridPattern } from "@/components/magicui/interactive-grid-pattern";
```

----------------------------------------

TITLE: Defining JavaScript Handler for Element Exit Animation
DESCRIPTION: This JavaScript function, `handleExit`, is called when an element begins its exiting transition. It immediately sets the element's scale to 1 and then, after a 100ms delay, scales it down to 0.8, creating a subtle 'pop-out' effect.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/blog/mui-transitions.mdx#_snippet_22

LANGUAGE: javascript
CODE:
```
function handleExit(node) {
  node.style.transform = "scale(1)";

  setTimeout(() => {
    node.style.transform = "scale(0.8)";
  }, 100);
}
```

----------------------------------------

TITLE: Configuring Vite Path Aliases (TypeScript)
DESCRIPTION: This `vite.config.ts` configuration sets up a path alias, mapping `@` to the `src` directory using Node.js's `path` module. This allows for cleaner absolute imports in the application, such as `import Component from '@/components/Component'`, improving code readability and maintainability.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/installation/vite.mdx#_snippet_5

LANGUAGE: typescript
CODE:
```
import path from "path";
import react from "@vitejs/plugin-react";
import { defineConfig } from "vite";

export default defineConfig({
  plugins: [react()],
  resolve: {
    alias: {
      "@": path.resolve(__dirname, "./src")
    }
  }
});
```

----------------------------------------

TITLE: Adding MagicUI Globe Component (bash)
DESCRIPTION: This command adds a specific MagicUI component, 'Globe', to the project. It fetches the component definition from the provided URL and integrates it into the project's component directory, making it available for import.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/installation/index.mdx#_snippet_1

LANGUAGE: bash
CODE:
```
npx shadcn@latest add "https://magicui.design/r/globe.json"
```

----------------------------------------

TITLE: Basic Usage of Terminal Component (TypeScript/React)
DESCRIPTION: This example illustrates the basic structure for using the `Terminal` component. It nests `TypingAnimation` and `AnimatedSpan` components to create a sequence of animated text outputs, simulating a command-line interface. The `children` prop of `Terminal` accepts `ReactNode` content.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/terminal.mdx#_snippet_2

LANGUAGE: tsx
CODE:
```
<Terminal>
  <TypingAnimation>
    <AnimatedSpan>Hello, world!</AnimatedSpan>
    <TypingAnimation>MagicUI is awesome!</TypingAnimation>
  </TypingAnimation>
</Terminal>
```

----------------------------------------

TITLE: Creating a Reusable Button Style Mixin in Sass
DESCRIPTION: This snippet defines a Sass mixin named `button-style` that accepts a `$color` parameter. Mixins encapsulate a group of CSS properties, allowing them to be reused across multiple elements by simply including the mixin, promoting the DRY principle and reducing code redundancy.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/blog/react-hero-component.mdx#_snippet_2

LANGUAGE: SCSS
CODE:
```
@mixin button-style($color) { background-color: $color; border: none; padding: 10px; }
```

----------------------------------------

TITLE: Initializing shadcn/ui Project (Bash)
DESCRIPTION: This command executes the latest `shadcn` initialization script using `npx`. It sets up the necessary configuration files and directories for integrating shadcn/ui components into the project.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/installation/laravel.mdx#_snippet_1

LANGUAGE: bash
CODE:
```
npx shadcn@latest init
```

----------------------------------------

TITLE: Nesting Selectors in Sass for Improved Readability
DESCRIPTION: This example illustrates Sass's nesting feature, which allows CSS selectors to be nested within each other. This visually represents the hierarchical relationship between parent and child elements, making the stylesheet more organized and readable by reducing repetitive selector declarations.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/blog/react-hero-component.mdx#_snippet_1

LANGUAGE: SCSS
CODE:
```
.hero { .title { font-size: 2rem; } }
```

----------------------------------------

TITLE: Installing Radix UI React Icons Library (Bash)
DESCRIPTION: This command installs the `@radix-ui/react-icons` package, which is the recommended icon library for projects adopting the `new-york` style. It provides a set of high-quality, accessible icons from Radix UI.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/installation/manual.mdx#_snippet_2

LANGUAGE: bash
CODE:
```
npm install @radix-ui/react-icons
```

----------------------------------------

TITLE: Using Aurora Text Component
DESCRIPTION: Demonstrates how to render the AuroraText component in a TSX file by wrapping the desired text content. The component will apply the defined aurora effect to the 'Aurora Text' string, leveraging the previously imported component.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/aurora-text.mdx#_snippet_3

LANGUAGE: tsx
CODE:
```
<AuroraText>Aurora Text</AuroraText>
```

----------------------------------------

TITLE: Defining a Storybook Story for the React Button Component (TypeScript)
DESCRIPTION: This TypeScript snippet creates a Storybook story for the `Button` component, allowing it to be visualized and tested in isolation. It sets the story title, links it to the `Button` component, and defines a default story named `Default` that renders the button with 'Click me!' text.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/blog/create-react-component-library.mdx#_snippet_3

LANGUAGE: TypeScript
CODE:
```
// ./src/components/Button/button.stories.tsx
import { Button } from "./button";
export default {
title: "Button",
component: Button,
};
export const Default = () => <Button>Click me!</Button>;
```

----------------------------------------

TITLE: Using Text Reveal Component in TSX
DESCRIPTION: This snippet demonstrates how to render the `TextReveal` component, wrapping the desired text content that will fade in as the user scrolls. Place this component where you want the reveal effect to occur.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/text-reveal.mdx#_snippet_2

LANGUAGE: tsx
CODE:
```
<TextReveal>Magic UI will change the way you design.</TextReveal>
```

----------------------------------------

TITLE: Installing Material UI with Yarn (Bash)
DESCRIPTION: This command installs the `@material-ui/core` package, providing Material UI components, into the React project using Yarn. It's an alternative to npm for package management.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/blog/mui-box.mdx#_snippet_4

LANGUAGE: bash
CODE:
```
yarn add @material-ui/core
```

----------------------------------------

TITLE: Implementing Grow Transition in MagicUI (React)
DESCRIPTION: Illustrates the usage of the Grow component for creating a growing or shrinking effect. This transition is effective for emphasizing the appearance or disappearance of UI elements. The in prop manages the transition state, and timeout defines its duration.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/blog/mui-transitions.mdx#_snippet_6

LANGUAGE: JavaScript
CODE:
```
<Transition in={show} timeout={500}>\n  <Grow in={show}>{/* Your content */}</Grow>\n</Transition>
```

----------------------------------------

TITLE: Importing Global CSS in Astro Page (TypeScript)
DESCRIPTION: This Astro page snippet demonstrates how to import the `globals.css` file within the frontmatter of an Astro component, ensuring that global styles are applied to the page.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/installation/astro.mdx#_snippet_5

LANGUAGE: typescript
CODE:
```
---
import '@/styles/globals.css'
---
```

----------------------------------------

TITLE: Importing ScriptCopyBtn Component
DESCRIPTION: This code snippet shows the necessary import statement to bring the `ScriptCopyBtn` component into your TypeScript/React project. It assumes the component is located within the `@/components/magicui/` path.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/script-copy-btn.mdx#_snippet_1

LANGUAGE: tsx
CODE:
```
import { ScriptCopyBtn } from "@/components/magicui/script-copy-btn";
```

----------------------------------------

TITLE: Importing BlurFade Component (TypeScript React)
DESCRIPTION: This snippet shows how to import the `BlurFade` component into a TypeScript React project. It specifies the relative path to the component within the `@/components/magicui` directory, making it available for use in JSX.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/blur-fade.mdx#_snippet_1

LANGUAGE: tsx
CODE:
```
import { BlurFade } from "@/components/magicui/blur-fade";
```

----------------------------------------

TITLE: Integrating Custom CSS Animation with MagicUI Transition (React)
DESCRIPTION: Explains how to wrap a standard HTML element with a custom CSS class inside a MagicUI Transition component. This setup allows for applying custom CSS keyframe animations to the element, providing greater flexibility beyond built-in transitions. The in prop still controls the overall transition state.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/blog/mui-transitions.mdx#_snippet_9

LANGUAGE: JavaScript
CODE:
```
<Transition in={show} timeout={500}>\n  <div className=\"animated-element\">{/* Your content */}</div>\n</Transition>
```

----------------------------------------

TITLE: Integrating Ripple Component (TypeScript/TSX)
DESCRIPTION: This snippet shows how to integrate the `Ripple` component into your application's JSX/TSX structure. It's typically placed within a container that has `relative` positioning and `overflow-hidden` to properly display the animated effect.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/ripple.mdx#_snippet_3

LANGUAGE: tsx
CODE:
```
<div className="relative h-[500px] w-full overflow-hidden">
  <Ripple />
</div>
```

----------------------------------------

TITLE: Initializing shadcn/ui with Default Settings
DESCRIPTION: This command initializes a shadcn/ui project in Next.js using predefined default settings. The `-d` flag automatically selects 'new-york' style, 'zinc' base color, and enables CSS variables, bypassing interactive configuration prompts.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/installation/next.mdx#_snippet_1

LANGUAGE: bash
CODE:
```
npx shadcn@latest init -d
```

----------------------------------------

TITLE: Adding Specific magicui Components - Bash
DESCRIPTION: This command adds a specified magicui component to the project, automatically installing any required dependencies for that component.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/packages/cli/README.md#_snippet_3

LANGUAGE: bash
CODE:
```
npx magicui-cli add [component]
```

----------------------------------------

TITLE: Adding shadcn/ui Button Component (Bash)
DESCRIPTION: This command adds the `Button` component from the shadcn/ui library to the project. It fetches the component's source code and integrates it into the local codebase, making it available for use in the application.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/installation/vite.mdx#_snippet_8

LANGUAGE: bash
CODE:
```
npx shadcn@latest add button
```

----------------------------------------

TITLE: Using Pulsating Button Component (TypeScript/React)
DESCRIPTION: This JSX snippet demonstrates how to render the `PulsatingButton` component with basic text content. It can be placed anywhere within your React component's render method to display the animated button, accepting children as its content.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/pulsating-button.mdx#_snippet_3

LANGUAGE: tsx
CODE:
```
<PulsatingButton>Pulsating Button</PulsatingButton>
```

----------------------------------------

TITLE: Importing Globe Component in TSX
DESCRIPTION: This line imports the `Globe` component from its designated path within the project's `magicui` components directory. It makes the component available for use in TypeScript React files.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/globe.mdx#_snippet_2

LANGUAGE: tsx
CODE:
```
import { Globe } from "@/components/magicui/globe";
```

----------------------------------------

TITLE: Importing Aurora Text Component
DESCRIPTION: Imports the AuroraText component from its designated module path, typically located within the project's components/magicui directory. This step is necessary to make the component available for use in a TypeScript React (TSX) file.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/aurora-text.mdx#_snippet_2

LANGUAGE: tsx
CODE:
```
import { AuroraText } from "@/components/magicui/aurora-text";
```

----------------------------------------

TITLE: Basic Material UI Box Component Usage (JavaScript)
DESCRIPTION: This snippet demonstrates importing the `Box` component from `@mui/system` and using it within a React functional component. The `Box` is rendered as a `section` HTML element with a margin of 1 unit, displaying simple text content.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/blog/mui-box.mdx#_snippet_5

LANGUAGE: javascript
CODE:
```
import { Box } from "@mui/system";

export default function BoxExample() {
  return (
    <Box component="section" m={1}>
      This is a box
    </Box>
  );
}
```

----------------------------------------

TITLE: Initializing shadcn-ui Project - Bash
DESCRIPTION: This command initializes a shadcn-ui project, which can coexist with magicui. It's used for setting up the base shadcn-ui configuration.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/packages/cli/README.md#_snippet_1

LANGUAGE: bash
CODE:
```
npx shadcn-ui init
```

----------------------------------------

TITLE: Setting up Webpack aliases in gatsby-node.ts
DESCRIPTION: Defines Webpack aliases in `gatsby-node.ts` using the `onCreateWebpackConfig` API. This ensures proper module loading by resolving specific import paths like `@/components` and `@/lib/utils` during the Gatsby build process.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/installation/gatsby.mdx#_snippet_3

LANGUAGE: ts
CODE:
```
import * as path from "path";

export const onCreateWebpackConfig = ({ actions }) => {
  actions.setWebpackConfig({
    resolve: {
      alias: {
        "@/components": path.resolve(__dirname, "src/components"),
        "@/lib/utils": path.resolve(__dirname, "src/lib/utils")
      }
    }
  });
};
```

----------------------------------------

TITLE: Wrapping Core Content in Material UI Card with CardContent (JSX)
DESCRIPTION: This snippet shows how to use the `CardContent` component to wrap the main textual or interactive content of a Material UI Card. It serves as a container for the primary information displayed within the card.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/blog/mui-card.mdx#_snippet_3

LANGUAGE: jsx
CODE:
```
import { CardContent } from "@mui/material";
import Card from "@mui/material/Card";
<Card>
  <CardContent>{/* Card Contents */}</CardContent>
</Card>;
```

----------------------------------------

TITLE: Implementing Zoom Transition in MagicUI (React)
DESCRIPTION: Demonstrates the Zoom component for creating a zooming in or out effect on UI elements. This transition adds a dynamic and engaging visual to content appearance or disappearance. The in prop controls the zoom state, and timeout sets the animation duration.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/blog/mui-transitions.mdx#_snippet_8

LANGUAGE: JavaScript
CODE:
```
<Transition in={show} timeout={500}>\n  <Zoom in={show}>{/* Your content */}</Zoom>\n</Transition>
```

----------------------------------------

TITLE: Implementing Slide Transition in MagicUI (React)
DESCRIPTION: Shows how to apply a sliding effect using the Slide component, allowing elements to enter or exit from a specified direction. The direction prop (e.g., 'right') controls the slide origin, while in and timeout manage the animation. Useful for dynamic content reveals.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/blog/mui-transitions.mdx#_snippet_7

LANGUAGE: JavaScript
CODE:
```
<Transition in={show} timeout={500}>\n  <Slide direction=\"right\" in={show}>\n    {/* Your content */}\n  </Slide>\n</Transition>
```

----------------------------------------

TITLE: Creating New MagicUI Component - TypeScript
DESCRIPTION: This TypeScript code defines a basic React functional component named `ExampleComponent`. It serves as the starting point for a new MagicUI component, to be placed in `registry/magicui/example-component.tsx`.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/CONTRIBUTING.md#_snippet_6

LANGUAGE: TypeScript
CODE:
```
import React from 'react'

export default function ExampleComponent() {
  return (
    <div>
      This is your component.
    </div>
  )
}
```

----------------------------------------

TITLE: Installing VideoText Component via CLI
DESCRIPTION: This snippet demonstrates how to install the `VideoText` component using the `shadcn/ui` CLI. It adds the component to your project, handling dependencies and file setup automatically.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/video-text.mdx#_snippet_0

LANGUAGE: bash
CODE:
```
npx shadcn@latest add "https://magicui.design/r/video-text"
```

----------------------------------------

TITLE: Initializing shadcn-ui in Gatsby
DESCRIPTION: Executes the `shadcn-ui` initialization command to integrate the component library into the Gatsby project. This command sets up the necessary configuration and dependencies for shadcn-ui.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/installation/gatsby.mdx#_snippet_4

LANGUAGE: bash
CODE:
```
npx shadcn@latest init
```

----------------------------------------

TITLE: Installing Animated Shiny Text Component (CLI) - Bash
DESCRIPTION: Command to install the Animated Shiny Text component using the shadcn/ui CLI extension for magicui. This command fetches and adds the component files to your project.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/animated-shiny-text.mdx#_snippet_0

LANGUAGE: bash
CODE:
```
npx shadcn@latest add "https://magicui.design/r/animated-shiny-text"
```

----------------------------------------

TITLE: Installing Scroll-Based Velocity Component via CLI
DESCRIPTION: This command uses `npx` to add the `scroll-based-velocity` component from `shadcn/ui`'s latest version, simplifying the installation process for the component.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/scroll-based-velocity.mdx#_snippet_0

LANGUAGE: bash
CODE:
```
npx shadcn@latest add "https://magicui.design/r/scroll-based-velocity"
```

----------------------------------------

TITLE: Disabling Tailwind Base Styles in Astro Config (JavaScript)
DESCRIPTION: This JavaScript configuration snippet for `astro.config.mjs` sets `applyBaseStyles` to `false` within the Tailwind integration. This prevents duplicate base styles when `globals.css` already imports them.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/installation/astro.mdx#_snippet_6

LANGUAGE: javascript
CODE:
```
export default defineConfig({
  integrations: [
    tailwind({
      applyBaseStyles: false,
    }),
  ],
});
```

----------------------------------------

TITLE: Importing Material UI Button Component in React
DESCRIPTION: This JavaScript import statement brings the 'Button' component specifically from the '@mui/material' package into a React application. Once imported, developers can use this component in their JSX code to render a Material UI styled button, leveraging its pre-built design and functionality.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/blog/material-ui-react.mdx#_snippet_1

LANGUAGE: javascript
CODE:
```
import Button from '@mui/material/Button';
```

----------------------------------------

TITLE: Creating a Basic Rounded Button in React with Tailwind
DESCRIPTION: This snippet demonstrates how to create a simple React button component and apply a default border-radius using Tailwind CSS's 'rounded' utility class. It sets up a basic layout and styles the button with a blue background, white text, padding, and rounded corners.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/blog/tailwind-border-radius.mdx#_snippet_0

LANGUAGE: tsx
CODE:
```
import "./App.css";
import "./index.css";
function App() {
  return (
    <div className="w-full flex flex-rows items-center justify-center h-screen bg-gray-100 gap-4">
      <button className="bg-blue-500 text-white px-4 py-2 rounded">
        'rounded'
      </button>
    </div>
  );
}
export default App;
```

----------------------------------------

TITLE: Using Animated Circular Progress Bar Component (TypeScript/React)
DESCRIPTION: This snippet demonstrates the basic usage of the `AnimatedCircularProgressBar` component as a self-closing JSX tag. It renders the component with its default properties.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/animated-circular-progress-bar.mdx#_snippet_2

LANGUAGE: tsx
CODE:
```
<AnimatedCircularProgressBar />
```

----------------------------------------

TITLE: Installing Manual Dependencies for Globe
DESCRIPTION: This command installs the core npm packages, `cobe` and `motion`, which are essential prerequisites for the 'Globe' component to function correctly when installed manually.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/globe.mdx#_snippet_1

LANGUAGE: bash
CODE:
```
npm install cobe motion
```

----------------------------------------

TITLE: Importing and Using Globe Component in Next.js (TSX)
DESCRIPTION: This TypeScript JSX snippet demonstrates how to import the 'Globe' component, previously added via the 'add' command, into a Next.js page. It shows the component being rendered within a simple 'div' element, illustrating its basic usage after installation.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/installation/index.mdx#_snippet_2

LANGUAGE: tsx
CODE:
```
import { Globe } from "@/components/ui/globe";

export default function Home() {
  return (
    <div>
      <Globe />
    </div>
  );
}
```

----------------------------------------

TITLE: Render Animated Beam Component
DESCRIPTION: Renders the `AnimatedBeam` component, connecting two elements (`fromRef` and `toRef`) within a specified container (`containerRef`). This demonstrates the basic usage of the component with required references.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/animated-beam.mdx#_snippet_2

LANGUAGE: tsx
CODE:
```
<AnimatedBeam containerRef={containerRef} fromRef={fromRef} toRef={toRef} />
```

----------------------------------------

TITLE: Installing React Native Library with npm
DESCRIPTION: This command installs the `react-native-webview` library into a React Native project using npm. It's the standard way to add new packages from the npm registry to your project dependencies.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/blog/react-native-libraries.mdx#_snippet_0

LANGUAGE: Bash
CODE:
```
npm install react-native-webview
```

----------------------------------------

TITLE: Adding Pulsating Button CSS Animations (CSS)
DESCRIPTION: This CSS snippet defines the `@keyframes pulse` animation, which creates the pulsating effect by animating the `box-shadow` property. It should be added to your global CSS file to enable the visual animation for the button, utilizing CSS variables for customization.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/pulsating-button.mdx#_snippet_1

LANGUAGE: css
CODE:
```
@theme inline {
  --animate-pulse: pulse var(--duration) ease-out infinite;

  @keyframes pulse {
    0%,
    100% {
      box-shadow: 0 0 0 0 var(--pulse-color);
    }
    50% {
      box-shadow: 0 0 0 8px var(--pulse-color);
    }
  }
}
```

----------------------------------------

TITLE: Adding Global CSS Animations for RetroGrid
DESCRIPTION: These CSS animations define the `grid` keyframes for the animated scrolling effect of the RetroGrid. They should be added to your global CSS file (e.g., `app/globals.css`) to enable the visual animation.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/retro-grid.mdx#_snippet_1

LANGUAGE: css
CODE:
```
@theme inline {
  --animate-grid: grid 15s linear infinite;

  @keyframes grid {
    0% {
      transform: translateY(-50%);
    }
    100% {
      transform: translateY(0);
    }
  }
}
```

----------------------------------------

TITLE: Defining CSS Class for Fade-Out Animation
DESCRIPTION: This CSS snippet defines the styling for an element exiting a transition. It sets `opacity: 1;` as the starting point and applies a `fadeOut` keyframe animation over 0.5 seconds with an ease-in-out timing function.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/blog/mui-transitions.mdx#_snippet_13

LANGUAGE: css
CODE:
```
.fade-out {
  opacity: 1;
  animation: fadeOut 0.5s ease-in-out;
}
```

----------------------------------------

TITLE: Adding Marquee CSS Animations
DESCRIPTION: These CSS keyframe animations define the horizontal and vertical scrolling behavior for the Marquee component. They should be added to a global CSS file, typically `app/globals.css`, to enable the infinite scroll effect.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/marquee.mdx#_snippet_1

LANGUAGE: css
CODE:
```
@theme inline {
  --animate-marquee: marquee var(--duration) infinite linear;
  --animate-marquee-vertical: marquee-vertical var(--duration) linear infinite;

  @keyframes marquee {
    from {
      transform: translateX(0);
    }
    to {
      transform: translateX(calc(-100% - var(--gap)));
    }
  }
  @keyframes marquee-vertical {
    from {
      transform: translateY(0);
    }
    to {
      transform: translateY(calc(-100% - var(--gap)));
    }
  }
}
```

----------------------------------------

TITLE: Using Animated Gradient Text Component in TSX
DESCRIPTION: This snippet demonstrates how to render the `AnimatedGradientText` component within a React application. By wrapping text content, the component applies an animated gradient background, enhancing the visual appeal of the text.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/animated-gradient-text.mdx#_snippet_3

LANGUAGE: tsx
CODE:
```
<AnimatedGradientText>Animated Gradient Text</AnimatedGradientText>
```

----------------------------------------

TITLE: Using Interactive Hover Button Component (TypeScript/React)
DESCRIPTION: This example shows how to render the InteractiveHoverButton component within a TypeScript/React application. The text 'Interactive Hover Button' is passed as children, which will be displayed as the button's label.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/interactive-hover-button.mdx#_snippet_2

LANGUAGE: tsx
CODE:
```
<InteractiveHoverButton>Interactive Hover Button</InteractiveHoverButton>
```

----------------------------------------

TITLE: Using SparklesText Component (TypeScript/TSX)
DESCRIPTION: This example demonstrates how to render the `SparklesText` component, wrapping the desired text content to apply the sparkling effect. The text 'Sparkles Text' will be displayed with animated stars, utilizing the component's default properties.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/sparkles-text.mdx#_snippet_2

LANGUAGE: tsx
CODE:
```
<SparklesText>Sparkles Text</SparklesText>
```

----------------------------------------

TITLE: Using HyperText Component (TSX)
DESCRIPTION: Illustrates the basic rendering of the `HyperText` component with 'Hover me' as its child content. This will display the text with the component's characteristic scrambling animation, especially on hover.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/hyper-text.mdx#_snippet_3

LANGUAGE: tsx
CODE:
```
<HyperText>Hover me</HyperText>
```

----------------------------------------

TITLE: Rendering Ripple Button Component (TypeScript React)
DESCRIPTION: This TSX snippet demonstrates the basic usage of the `RippleButton` component. By wrapping content like 'Ripple Button' within the component tags, it will render a button that exhibits the animated ripple effect upon user interaction.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/ripple-button.mdx#_snippet_3

LANGUAGE: tsx
CODE:
```
<RippleButton>Ripple Button</RippleButton>
```

----------------------------------------

TITLE: Creating Local Environment File - Bash
DESCRIPTION: This command creates a `.env.local` file and populates it with the `NEXT_PUBLIC_APP_URL` variable, setting the local development server URL. This file is used for environment-specific configurations.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/CONTRIBUTING.md#_snippet_4

LANGUAGE: Bash
CODE:
```
touch .env.local && echo "NEXT_PUBLIC_APP_URL=http://localhost:3000" > .env.local
```

----------------------------------------

TITLE: Using AnimatedList Component with Child Elements
DESCRIPTION: This example illustrates how to integrate the `AnimatedList` component into your TSX/React application. You can wrap any HTML or React elements inside `AnimatedList`, and each child will be animated in sequence with a configurable delay.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/animated-list.mdx#_snippet_2

LANGUAGE: tsx
CODE:
```
<AnimatedList>
  <p>Item 1</p>
  <p>Item 2</p>
  <p>Item 3</p>
</AnimatedList>
```

----------------------------------------

TITLE: Adding Aurora Text CSS Animations
DESCRIPTION: Defines the @keyframes aurora animation for the text effect, specifying background position and transform changes over time. This animation is then applied via a CSS custom property --animate-aurora within a @theme inline block, which should be added to a global CSS file like app/globals.css.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/aurora-text.mdx#_snippet_1

LANGUAGE: css
CODE:
```
@theme inline {
  --animate-aurora: aurora 8s ease-in-out infinite alternate;

  @keyframes aurora {
    0% {
      background-position: 0% 50%;
      transform: rotate(-5deg) scale(0.9);
    }
    25% {
      background-position: 50% 100%;
      transform: rotate(5deg) scale(1.1);
    }
    50% {
      background-position: 100% 50%;
      transform: rotate(-3deg) scale(0.95);
    }
    75% {
      background-position: 50% 0%;
      transform: rotate(3deg) scale(1.05);
    }
    100% {
      background-position: 0% 50%;
      transform: rotate(-5deg) scale(0.9);
    }
  }
}
```

----------------------------------------

TITLE: Adding Global CSS Animations for Meteors
DESCRIPTION: This CSS block defines the `@keyframes meteor` animation and a custom property `--animate-meteor` for the meteor shower effect. It should be added to your global CSS file (e.g., `app/globals.css`) to enable the visual animation.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/meteors.mdx#_snippet_1

LANGUAGE: css
CODE:
```
@theme inline {
  --animate-meteor: meteor 5s linear infinite;

  @keyframes meteor {
    0% {
      transform: rotate(var(--angle)) translateX(0);
      opacity: 1;
    }
    70% {
      opacity: 1;
    }
    100% {
      transform: rotate(var(--angle)) translateX(-500px);
      opacity: 0;
    }
  }
}
```

----------------------------------------

TITLE: Embedding Media in Material UI Card with CardMedia (JSX)
DESCRIPTION: This snippet demonstrates how to embed images or other media within a Material UI Card using the `CardMedia` component. It shows how to specify an image source and a title for the media element, along with inline styling.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/blog/mui-card.mdx#_snippet_2

LANGUAGE: jsx
CODE:
```
<Card>
  <CardMedia
    style={{ paddingTop: "24px" }}
    image="./background.png"
    title="Background image"
  />
</Card>
```

----------------------------------------

TITLE: Rendering Particles Component (TypeScript/React)
DESCRIPTION: This snippet illustrates how to render the `Particles` component within a React application. It places the component inside a `div` with specific styling to create a visible container for the particles.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/particles.mdx#_snippet_2

LANGUAGE: tsx
CODE:
```
<div className="relative overflow-hidden h-[500px] w-full">
  <Particles />
</div>
```

----------------------------------------

TITLE: Rendering GridPattern Component (TypeScript/TSX)
DESCRIPTION: This snippet demonstrates how to render the `GridPattern` component within a `div` element. The `div` is styled with Tailwind CSS classes to provide a relative position, fixed height, full width, and hidden overflow, creating a suitable container for the background pattern.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/grid-pattern.mdx#_snippet_2

LANGUAGE: tsx
CODE:
```
<div className="relative h-[500px] w-full overflow-hidden">
  <GridPattern />
</div>
```

----------------------------------------

TITLE: Rendering ScrollProgress Component (TSX)
DESCRIPTION: This snippet illustrates how to render the `ScrollProgress` component within your TSX code. Once imported, the component can be used as a self-closing tag, and it accepts standard HTMLDivElement properties in addition to a `className` prop for styling.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/scroll-progress.mdx#_snippet_2

LANGUAGE: tsx
CODE:
```
<ScrollProgress />
```

----------------------------------------

TITLE: Using Neon Gradient Card Component (TSX)
DESCRIPTION: This example demonstrates how to render the `NeonGradientCard` component, wrapping child elements within it. The `p-4` class applies padding, and the `p` and `span` tags serve as placeholder content.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/neon-gradient-card.mdx#_snippet_3

LANGUAGE: tsx
CODE:
```
<NeonGradientCard>
  <div className="p-4">
    <p>Hello</p>
    <span>Hover me</span>
  </div>
</NeonGradientCard>
```

----------------------------------------

TITLE: Applying CoolMode to Elements in TSX
DESCRIPTION: This example illustrates how to apply the 'Cool Mode' effect to a DOM element. By wrapping an element, such as a `<button>`, with the `<CoolMode>` component, the interactive particle effect will be triggered on interactions with the wrapped element.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/cool-mode.mdx#_snippet_2

LANGUAGE: tsx
CODE:
```
<CoolMode>
  <button>Click me</button>
</CoolMode>
```

----------------------------------------

TITLE: Basic Usage of ShimmerButton Component in TSX
DESCRIPTION: Demonstrates the basic usage of the `ShimmerButton` component as a React JSX element. It renders a button with the text 'Shimmer Button' and applies its unique shimmering effect, ready to be placed within a React component's render method.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/shimmer-button.mdx#_snippet_3

LANGUAGE: tsx
CODE:
```
<ShimmerButton>Shimmer Button</ShimmerButton>
```

----------------------------------------

TITLE: Installing File Tree Component via CLI (Bash)
DESCRIPTION: This snippet demonstrates how to install the MagicUI File Tree component using the `npx shadcn` command-line interface. It adds the component to your project, handling dependencies and setup automatically.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/file-tree.mdx#_snippet_0

LANGUAGE: bash
CODE:
```
npx shadcn@latest add "https://magicui.design/r/file-tree"
```

----------------------------------------

TITLE: Installing Line Shadow Text Component (CLI)
DESCRIPTION: This command uses the `npx shadcn` utility to automatically add the Line Shadow Text component to your project, streamlining the setup process. It fetches the component from the specified Magic UI registry URL.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/line-shadow-text.mdx#_snippet_0

LANGUAGE: bash
CODE:
```
npx shadcn@latest add "https://magicui.design/r/line-shadow-text"
```

----------------------------------------

TITLE: Install ScratchToReveal Component via CLI (Bash)
DESCRIPTION: This command uses the `shadcn` CLI to add the `ScratchToReveal` component from the MagicUI design repository. It's the quickest way to integrate the component into your project.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/scratch-to-reveal.mdx#_snippet_0

LANGUAGE: bash
CODE:
```
npx shadcn@latest add "https://magicui.design/r/scratch-to-reveal"
```

----------------------------------------

TITLE: Installing HeroVideoDialog via CLI (Bash)
DESCRIPTION: This command uses `npx shadcn` to add the HeroVideoDialog component to your project, simplifying the installation process by fetching the component from the magicui.design registry.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/hero-video-dialog.mdx#_snippet_0

LANGUAGE: bash
CODE:
```
npx shadcn@latest add "https://magicui.design/r/hero-video-dialog"
```

----------------------------------------

TITLE: Installing Pulsating Button via CLI (Bash)
DESCRIPTION: This command uses `npx shadcn` to add the Pulsating Button component to your project, simplifying the installation process by fetching it directly from the MagicUI design repository.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/pulsating-button.mdx#_snippet_0

LANGUAGE: bash
CODE:
```
npx shadcn@latest add "https://magicui.design/r/pulsating-button"
```

----------------------------------------

TITLE: Installing Animated Subscribe Button with CLI (Bash)
DESCRIPTION: This snippet demonstrates how to install the `AnimatedSubscribeButton` component using the `shadcn` CLI. This command automatically adds the component and its required dependencies to your project, streamlining the setup process.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/animated-subscribe-button.mdx#_snippet_0

LANGUAGE: bash
CODE:
```
npx shadcn@latest add "https://magicui.design/r/animated-subscribe-button"
```

----------------------------------------

TITLE: Installing Icon Cloud with CLI (Bash)
DESCRIPTION: This snippet demonstrates how to install the Icon Cloud component using the `shadcn` CLI, which simplifies adding components to your project by fetching them from a specified URL.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/icon-cloud.mdx#_snippet_0

LANGUAGE: bash
CODE:
```
npx shadcn@latest add "https://magicui.design/r/icon-cloud"
```

----------------------------------------

TITLE: Installing Meteors Component via CLI
DESCRIPTION: This command uses `npx shadcn` to add the Meteors component from magicui.design to your project. It's the recommended way for quick installation.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/meteors.mdx#_snippet_0

LANGUAGE: bash
CODE:
```
npx shadcn@latest add "https://magicui.design/r/meteors"
```

----------------------------------------

TITLE: Installing Grid Pattern via CLI (Bash)
DESCRIPTION: This snippet demonstrates how to install the Grid Pattern component using the `shadcn` CLI, which fetches the component from the specified URL. This is the recommended method for quick setup.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/grid-pattern.mdx#_snippet_0

LANGUAGE: bash
CODE:
```
npx shadcn@latest add "https://magicui.design/r/grid-pattern"
```

----------------------------------------

TITLE: Installing Bootstrap via npm
DESCRIPTION: This command installs the Bootstrap package as a dependency in your React project. It's the foundational step for integrating Bootstrap's styling and JavaScript functionalities, typically used with build tools like Webpack.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/blog/react-bootstrap.mdx#_snippet_1

LANGUAGE: Shell
CODE:
```
npm install bootstrap
```

----------------------------------------

TITLE: Rendering Word Rotate Component - TSX
DESCRIPTION: This TSX snippet demonstrates how to render the `WordRotate` component. The `words` prop accepts an array of strings, which the component will sequentially display with a vertical rotation animation.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/word-rotate.mdx#_snippet_3

LANGUAGE: tsx
CODE:
```
<WordRotate words={["Word", "Rotate"]} />
```

----------------------------------------

TITLE: Using Meteors Component in TSX
DESCRIPTION: This snippet demonstrates how to embed the `Meteors` component within a `div` element. The parent `div` provides a relative position and defines the dimensions and overflow behavior for the meteor effect to be contained within.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/meteors.mdx#_snippet_3

LANGUAGE: tsx
CODE:
```
<div className="relative overflow-hidden h-[500px] w-full max-w-[350px]">
  <Meteors />
</div>
```

----------------------------------------

TITLE: Rendering ScriptCopyBtn Component
DESCRIPTION: This snippet illustrates how to render the `ScriptCopyBtn` component within a TSX/React application. Once imported, the component can be used as a self-closing JSX tag.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/script-copy-btn.mdx#_snippet_2

LANGUAGE: tsx
CODE:
```
<ScriptCopyBtn />
```

----------------------------------------

TITLE: Starting the Development Server for a React App
DESCRIPTION: This command initiates the local development server for your React application, typically making it accessible in a web browser at localhost:3000. It compiles and serves your application for development purposes.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/blog/mui-table.mdx#_snippet_3

LANGUAGE: bash
CODE:
```
npm run dev
```

----------------------------------------

TITLE: Importing Marquee Component
DESCRIPTION: This line imports the `Marquee` component from the `magicui` library, making it available for use in a React or Next.js application. The import path should be adjusted based on your project's setup.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/marquee.mdx#_snippet_2

LANGUAGE: tsx
CODE:
```
import { Marquee } from "@/components/magicui/marquee";
```

----------------------------------------

TITLE: Importing AnimatedList Component in TypeScript/React
DESCRIPTION: This snippet shows the necessary import statement to bring the `AnimatedList` component into your TypeScript/React file, allowing you to use it within your application.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/animated-list.mdx#_snippet_1

LANGUAGE: tsx
CODE:
```
import { AnimatedList } from "@/components/magicui/animated-list";
```

----------------------------------------

TITLE: Importing Dock Components and Icons (TSX)
DESCRIPTION: This TypeScript/React snippet shows the necessary import statements for using the `Dock` and `DockIcon` components from MagicUI, along with example icons (`Home`, `Settings`, `Search`) from `lucide-react`. These imports are prerequisites for rendering the dock in a React application.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/dock.mdx#_snippet_1

LANGUAGE: tsx
CODE:
```
import { Dock, DockIcon } from "@/components/magicui/dock";
import { Home, Settings, Search } from "lucide-react";
```

----------------------------------------

TITLE: Importing File Tree Components (TypeScript/TSX)
DESCRIPTION: This snippet shows the necessary import statement for using the `Tree`, `Folder`, and `File` components from the MagicUI file-tree library in a TypeScript/TSX project. These components are essential for constructing a file tree structure.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/file-tree.mdx#_snippet_1

LANGUAGE: tsx
CODE:
```
import { File, Folder, Tree } from "@/components/magicui/file-tree";
```

----------------------------------------

TITLE: Importing Pulsating Button Component (TypeScript/React)
DESCRIPTION: This import statement brings the `PulsatingButton` component into your TypeScript/React file, making it available for use in your application. The path `"@/components/magicui/pulsating-button"` assumes a typical Next.js or similar project structure with path aliases.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/pulsating-button.mdx#_snippet_2

LANGUAGE: tsx
CODE:
```
import { PulsatingButton } from "@/components/magicui/pulsating-button";
```

----------------------------------------

TITLE: Importing Pointer Component in TSX
DESCRIPTION: This snippet demonstrates how to import the `Pointer` component from its module path within a TypeScript React (TSX) project. This is the essential first step to making the component available for use in your application's UI.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/pointer.mdx#_snippet_1

LANGUAGE: tsx
CODE:
```
import { Pointer } from "@/components/magicui/pointer";
```

----------------------------------------

TITLE: Importing Text Animate Component in TSX
DESCRIPTION: This snippet shows the standard import statement for the `TextAnimate` component. It assumes a project setup where `@/components/magicui/text-animate` resolves to the component's file, typically found in Next.js or similar environments using path aliases.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/text-animate.mdx#_snippet_1

LANGUAGE: tsx
CODE:
```
import { TextAnimate } from "@/components/magicui/text-animate";
```

----------------------------------------

TITLE: Importing MorphingText Component in TSX
DESCRIPTION: This code snippet shows the standard way to import the `MorphingText` component into a TypeScript React (TSX) file. It makes the component available for use within your application by referencing its module path.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/morphing-text.mdx#_snippet_1

LANGUAGE: tsx
CODE:
```
import { MorphingText } from "@/components/magicui/morphing-text";
```

----------------------------------------

TITLE: Adding CSS Animations for Line Shadow Text
DESCRIPTION: This CSS block defines the `line-shadow` keyframe animation and applies it within an `@theme inline` context. The animation continuously shifts the `background-position` to create the moving line shadow effect, which is crucial for the visual functionality of the Line Shadow Text component.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/line-shadow-text.mdx#_snippet_1

LANGUAGE: css
CODE:
```
@theme inline {
  --animate-line-shadow: line-shadow 15s linear infinite;

  @keyframes line-shadow {
    0% {
      background-position: 0 0;
    }
    100% {
      background-position: 100% -100%;
    }
  }
}
```

----------------------------------------

TITLE: Importing Shiny Button Component (TSX)
DESCRIPTION: This snippet shows the necessary import statement to bring the `ShinyButton` component into your React/TSX file. The path `"@/components/magicui/shiny-button"` should be adjusted to match your project's alias or relative path.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/shiny-button.mdx#_snippet_1

LANGUAGE: tsx
CODE:
```
import { ShinyButton } from "@/components/magicui/shiny-button";
```

----------------------------------------

TITLE: Running MagicUI Development Server - pnpm
DESCRIPTION: This command starts the MagicUI development server, typically accessible at `http://localhost:3000`. It allows you to preview your changes and test components locally.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/CONTRIBUTING.md#_snippet_5

LANGUAGE: Bash
CODE:
```
pnpm dev
```

----------------------------------------

TITLE: Installing Cool Mode via CLI
DESCRIPTION: This snippet demonstrates how to install the 'Cool Mode' component using the `shadcn` CLI. It fetches the component from the specified URL and adds it to your project, simplifying the setup process.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/cool-mode.mdx#_snippet_0

LANGUAGE: bash
CODE:
```
npx shadcn@latest add "https://magicui.design/r/cool-mode"
```

----------------------------------------

TITLE: Install Confetti Component via CLI
DESCRIPTION: This command uses the `shadcn` CLI to automatically add the Confetti component from `magicui.design` to your project, handling dependencies and file setup.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/confetti.mdx#_snippet_0

LANGUAGE: bash
CODE:
```
npx shadcn@latest add "https://magicui.design/r/confetti"
```

----------------------------------------

TITLE: Installing Smooth Cursor Component via CLI
DESCRIPTION: This command installs the Smooth Cursor component using the `shadcn` CLI, fetching it directly from the magicui.design repository. It's the recommended and easiest way to add the component to your project.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/smooth-cursor.mdx#_snippet_0

LANGUAGE: bash
CODE:
```
npx shadcn@latest add "https://magicui.design/r/smooth-cursor"
```

----------------------------------------

TITLE: Installing Magic Card via CLI
DESCRIPTION: This command installs the Magic Card component using the `shadcn` CLI, fetching it directly from the MagicUI registry. It's the recommended way to add the component to your project.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/magic-card.mdx#_snippet_0

LANGUAGE: bash
CODE:
```
npx shadcn@latest add "https://magicui.design/r/magic-card"
```

----------------------------------------

TITLE: Installing BlurFade Component via CLI (Bash)
DESCRIPTION: This snippet demonstrates how to install the `BlurFade` component using the `shadcn` CLI. It adds the component directly from the MagicUI design repository, simplifying the setup process for React projects.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/blur-fade.mdx#_snippet_0

LANGUAGE: bash
CODE:
```
npx shadcn@latest add "https://magicui.design/r/blur-fade"
```

----------------------------------------

TITLE: Installing Dot Pattern Component via CLI (Bash)
DESCRIPTION: This command uses the `shadcn/ui` CLI to add the `DotPattern` component to your project. It fetches the component directly from the MagicUI registry, providing a quick and automated installation process for integrating the background pattern.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/dot-pattern.mdx#_snippet_0

LANGUAGE: bash
CODE:
```
npx shadcn@latest add "https://magicui.design/r/dot-pattern"
```

----------------------------------------

TITLE: Installing Globe Component via CLI
DESCRIPTION: This command uses the `shadcn` CLI to automatically add the 'Globe' component to your project. It fetches the component's code from the specified MagicUI design URL, streamlining the installation process.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/globe.mdx#_snippet_0

LANGUAGE: bash
CODE:
```
npx shadcn@latest add "https://magicui.design/r/globe"
```

----------------------------------------

TITLE: Installing Bento Grid via CLI
DESCRIPTION: This command installs the Bento Grid component using the `shadcn` CLI, fetching it from the specified MagicUI design URL. It's the recommended method for quick setup.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/bento-grid.mdx#_snippet_0

LANGUAGE: bash
CODE:
```
npx shadcn@latest add "https://magicui.design/r/bento-grid"
```

----------------------------------------

TITLE: Installing Marquee Component via CLI
DESCRIPTION: This command installs the Marquee component using the `shadcn` CLI, fetching it directly from the magicui.design repository. It's the recommended way for quick setup and integration into your project.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/marquee.mdx#_snippet_0

LANGUAGE: bash
CODE:
```
npx shadcn@latest add "https://magicui.design/r/marquee"
```

----------------------------------------

TITLE: Installing Avatar Circles Component (CLI)
DESCRIPTION: This command uses `npx shadcn@latest` to add the `avatar-circles` component from `magicui.design` to your project. This is the recommended and most straightforward method for installation.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/avatar-circles.mdx#_snippet_0

LANGUAGE: bash
CODE:
```
npx shadcn@latest add "https://magicui.design/r/avatar-circles"
```

----------------------------------------

TITLE: Installing Shimmer Button using CLI
DESCRIPTION: This command installs the Shimmer Button component using the shadcn CLI, fetching it directly from the MagicUI design repository. It simplifies the setup process by automating the component's integration.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/shimmer-button.mdx#_snippet_0

LANGUAGE: bash
CODE:
```
npx shadcn@latest add "https://magicui.design/r/shimmer-button"
```

----------------------------------------

TITLE: Installing Box Reveal Component via CLI
DESCRIPTION: This snippet demonstrates how to install the Box Reveal component using the `npx shadcn` command-line interface, adding it directly to your project.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/box-reveal.mdx#_snippet_0

LANGUAGE: bash
CODE:
```
npx shadcn@latest add "https://magicui.design/r/box-reveal"
```

----------------------------------------

TITLE: Installing Safari Component via CLI (Bash)
DESCRIPTION: This command uses the `shadcn/ui` CLI to automatically add the Safari component and its dependencies to your project, simplifying the setup process.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/safari.mdx#_snippet_0

LANGUAGE: bash
CODE:
```
npx shadcn@latest add "https://magicui.design/r/safari"
```

----------------------------------------

TITLE: Basic Usage of Text Animate Component in TSX
DESCRIPTION: This snippet illustrates a basic usage of the `TextAnimate` component. It applies a 'blurInUp' animation effect, splitting the text 'Blur in by word' into individual words for animation. The `animation` and `by` props control the visual effect and text segmentation.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/text-animate.mdx#_snippet_2

LANGUAGE: tsx
CODE:
```
<TextAnimate animation="blurInUp" by="word">
  Blur in by word
</TextAnimate>
```

----------------------------------------

TITLE: Basic Usage of Box Reveal Component in TSX
DESCRIPTION: This snippet demonstrates the basic JSX usage of the `BoxReveal` component, wrapping text content that will be revealed by the animation.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/box-reveal.mdx#_snippet_2

LANGUAGE: tsx
CODE:
```
<BoxReveal>Box Reveal</BoxReveal>
```

----------------------------------------

TITLE: Basic Usage of NumberTicker Component (TypeScript/TSX)
DESCRIPTION: This TypeScript/TSX snippet demonstrates the basic usage of the `NumberTicker` component. By setting the `value` prop to `100`, it renders the ticker, animating the number from its default `startValue` (0) up to the specified target of 100.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/number-ticker.mdx#_snippet_2

LANGUAGE: tsx
CODE:
```
<NumberTicker value={100} />
```

----------------------------------------

TITLE: Rendering Android Component in TSX
DESCRIPTION: This snippet illustrates the basic usage of the `Android` component as a self-closing JSX tag within a TypeScript React (TSX) application, rendering the default Android mockup.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/android.mdx#_snippet_2

LANGUAGE: tsx
CODE:
```
<Android />
```

----------------------------------------

TITLE: Updating UI Component Registry in TypeScript
DESCRIPTION: This snippet demonstrates how to register a new UI component, 'example-component', in the `registry-ui.ts` file. It includes metadata like name, type, title, description, dependencies, file paths, and custom CSS variables and keyframes required for the component's styling and animation.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/CONTRIBUTING.md#_snippet_11

LANGUAGE: typescript
CODE:
```
export const ui: Registry = [
  // ... existing components ...
  {
    name: "example-component",
    type: "registry:ui",
    title: "Example Component",
    description:
      "A versatile component that can be used to display various types of content such as text, images, or videos.",
    dependencies: ["motion"],
    files: [
      {
        path: "registry/magicui/example-component.tsx",
        type: "registry:ui",
        target: "components/magicui/example-component.tsx",
      },
    ],
    // Add CSS variables for the component
    cssVars: {
      theme: {
        "animate-example": "example var(--duration) infinite linear",
      },
    },
    // Add CSS keyframes for the component
    css: {
      "@keyframes example": {
        from: {
          transform: "translateX(0)",
        },
        to: {
          transform: "translateX(calc(-100% - var(--gap)))",
        },
      },
    },
  },
];
```

----------------------------------------

TITLE: Installing Motion Dependency Manually
DESCRIPTION: This command installs the `motion` library, a required dependency for the `VelocityScroll` component, using `npm`.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/scroll-based-velocity.mdx#_snippet_1

LANGUAGE: bash
CODE:
```
npm install motion
```

----------------------------------------

TITLE: Creating a React Project with create-react-app (Sass)
DESCRIPTION: This command initializes a new React application named 'my-app' using the `create-react-app` tool. It sets up the basic project structure and dependencies, serving as the foundation for integrating Sass.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/blog/react-hero-component.mdx#_snippet_6

LANGUAGE: bash
CODE:
```
npx create-react-app my-app
```

----------------------------------------

TITLE: Import Global CSS in root.tsx (JS)
DESCRIPTION: Import the global `tailwind.css` file into the `app/root.tsx` file and add it to the document links.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/installation/remix.mdx#_snippet_6

LANGUAGE: js
CODE:
```
import styles from "./tailwind.css?url";

export const links: LinksFunction = () => [
  { rel: "stylesheet", href: styles },
  ...(cssBundleHref ? [{ rel: "stylesheet", href: cssBundleHref }] : []),
];
```

----------------------------------------

TITLE: Defining JavaScript Handler for Element Enter Animation
DESCRIPTION: This JavaScript function, `handleEnter`, is called when an element begins its entering transition. It immediately scales the element down to 0.8 and then, after a 100ms delay, scales it back to 1, creating a subtle 'pop-in' effect.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/blog/mui-transitions.mdx#_snippet_21

LANGUAGE: javascript
CODE:
```
function handleEnter(node) {
  node.style.transform = "scale(0.8)";
  setTimeout(() => {
    node.style.transform = "scale(1)";
  }, 100);
}
```

----------------------------------------

TITLE: Defining CSS Class for Fade-In Animation Start
DESCRIPTION: This CSS snippet defines the initial state for an element entering a transition. The `opacity: 0;` ensures the element starts completely transparent, preparing it for a fade-in effect.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/blog/mui-transitions.mdx#_snippet_12

LANGUAGE: css
CODE:
```
.fade-in {
  opacity: 0;
}
```

----------------------------------------

TITLE: Importing FlipText Component in TSX
DESCRIPTION: This snippet imports the `FlipText` component from its module path, making it available for use in a React/Next.js application. The path `"@/components/magicui/flip-text"` indicates a typical project structure for components.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/flip-text.mdx#_snippet_1

LANGUAGE: tsx
CODE:
```
import { FlipText } from "@/components/magicui/flip-text";
```

----------------------------------------

TITLE: Importing MUI Transition Component - JavaScript
DESCRIPTION: This snippet demonstrates how to import the `Transition` component from MUI's `@mui/material/transitions` package. This is the first step required to use MUI's transition functionalities in a React application.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/blog/mui-transitions.mdx#_snippet_1

LANGUAGE: JavaScript
CODE:
```
import { Transition } from "@mui/material/transitions";
```

----------------------------------------

TITLE: Importing Bento Grid Components (TypeScript/TSX)
DESCRIPTION: This snippet imports the `BentoCard` and `BentoGrid` components from the local MagicUI components path, making them available for use in a React/Next.js application.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/bento-grid.mdx#_snippet_1

LANGUAGE: tsx
CODE:
```
import { BentoCard, BentoGrid } from "@/components/magicui/bento-grid";
```

----------------------------------------

TITLE: Importing Animated Circular Progress Bar Component (TypeScript/React)
DESCRIPTION: This line imports the `AnimatedCircularProgressBar` component from its module path within a TypeScript/React project. It makes the component available for use in the current file.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/animated-circular-progress-bar.mdx#_snippet_1

LANGUAGE: tsx
CODE:
```
import { AnimatedCircularProgressBar } from "@/components/magicui/animated-circular-progress-bar";
```

----------------------------------------

TITLE: Importing VideoText Component in TSX
DESCRIPTION: This snippet shows the standard way to import the `VideoText` component into a TypeScript React (TSX) file. It makes the component available for use within your application.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/video-text.mdx#_snippet_1

LANGUAGE: tsx
CODE:
```
import { VideoText } from "@/registry/magicui/video-text";
```

----------------------------------------

TITLE: Import Animated Beam Component
DESCRIPTION: Imports the `AnimatedBeam` component from its module path within the project's components directory. This line is necessary to use the component in a React/Next.js file.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/animated-beam.mdx#_snippet_1

LANGUAGE: tsx
CODE:
```
import { AnimatedBeam } from "@/components/magicui/animated-beam";
```

----------------------------------------

TITLE: Importing Neon Gradient Card Component (TSX)
DESCRIPTION: This snippet shows the standard import statement for the `NeonGradientCard` component, assuming it's located in the `@/components/magicui/neon-gradient-card` path within your TypeScript/React project.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/neon-gradient-card.mdx#_snippet_2

LANGUAGE: tsx
CODE:
```
import { NeonGradientCard } from "@/components/magicui/neon-gradient-card";
```

----------------------------------------

TITLE: Defining Custom CSS Keyframe Animation for MagicUI Elements
DESCRIPTION: Provides the CSS definition for a custom 'bounce' keyframe animation. This animation scales an element to create a bouncing effect. It is designed to be applied to an element via a class (e.g., animated-element) and used in conjunction with the MagicUI Transition component for controlled animation.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/blog/mui-transitions.mdx#_snippet_10

LANGUAGE: CSS
CODE:
```
@keyframes bounce {\n  0% {\n    transform: scale(1);\n  }\n  \u00a0\u00a050% {\n    transform: scale(1.2);\n  }\n\n  100% {\n    transform: scale(1);\n  }\n}\n\n.animated-element {\n  \u00a0\u00a0animation: bounce 1s ease-in-out;\n}
```

----------------------------------------

TITLE: Adding Gaps to a Column-Based Grid in React with Tailwind CSS
DESCRIPTION: This snippet enhances the `CardGallery` component by adding a `gap-4` class to the grid container. This Tailwind CSS utility provides spacing between grid items, improving visual separation and readability within the column-based layout.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/blog/tailwind-css-grid.mdx#_snippet_3

LANGUAGE: tsx
CODE:
```
import { FC } from "react";
import Card from "./Card";
const CardGallery: FC<any> = () => {
  return (
    <div className="grid grid-col-3 gap-4">
      <Card />
      <Card />
      <Card />
      <Card />
    </div>
  );
};
export default CardGallery;
```

----------------------------------------

TITLE: Installing Magic UI MCP CLI for Cursor (Bash)
DESCRIPTION: This snippet demonstrates how to install the Magic UI Model Context Protocol (MCP) CLI for the Cursor IDE. It uses npx to execute the latest version of the @magicuidesign/cli package, which sets up the necessary integration for AI-assisted code generation within Cursor.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/mcp.mdx#_snippet_0

LANGUAGE: bash
CODE:
```
npx @magicuidesign/cli@latest install cursor
```

----------------------------------------

TITLE: Importing Sass into a React Component
DESCRIPTION: This line imports the `App.scss` stylesheet into a React component, enabling the use of Sass styles within that component. This is how Sass files are linked in React applications after installation.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/blog/react-hero-component.mdx#_snippet_7

LANGUAGE: tsx
CODE:
```
import "./App.scss";
```

----------------------------------------

TITLE: Install react-tweet Dependency
DESCRIPTION: Use npm to install the necessary 'react-tweet' package, which is the underlying library for the Tweet Card components.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/tweet-card.mdx#_snippet_0

LANGUAGE: bash
CODE:
```
npm install react-tweet
```

----------------------------------------

TITLE: Using AvatarCircles Component (TypeScript/React)
DESCRIPTION: This JSX snippet demonstrates how to render the `AvatarCircles` component. It accepts `numPeople` to display a numerical count and `avatarUrls` (an array of strings) to provide the image sources for the avatars.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/avatar-circles.mdx#_snippet_2

LANGUAGE: tsx
CODE:
```
<AvatarCircles numPeople={99} avatarUrls={avatars} />
```

----------------------------------------

TITLE: Exporting Next.js to Static HTML (Shell)
DESCRIPTION: This command converts a Next.js application into static HTML, CSS, and JavaScript files, making it suitable for deployment on static hosting platforms without requiring a Node.js server. This process generates a `out` directory containing the static assets. It's particularly useful for static websites or pre-rendered content.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/blog/what-is-nextjs.mdx#_snippet_2

LANGUAGE: Shell
CODE:
```
next export
```

----------------------------------------

TITLE: Initializing Tailwind CSS Configuration Files
DESCRIPTION: This command generates the `tailwind.config.js` and `postcss.config.js` files in the project root. These configuration files are essential for customizing Tailwind CSS and PostCSS behavior to suit specific project requirements.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/blog/install-tailwind-react.mdx#_snippet_5

LANGUAGE: javascript
CODE:
```
npx tailwindcss init -p
```

----------------------------------------

TITLE: Creating a React Project with create-react-app (CSS)
DESCRIPTION: This command initializes a new React application named 'my-app' using the `create-react-app` tool. It sets up the basic project structure and dependencies required for a React development environment.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/blog/react-hero-component.mdx#_snippet_4

LANGUAGE: bash
CODE:
```
npx create-react-app my-app
```

----------------------------------------

TITLE: Adding All magicui Components - Bash
DESCRIPTION: This command uses the `--all` flag to install all available magicui components into the project simultaneously, along with their respective dependencies.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/packages/cli/README.md#_snippet_5

LANGUAGE: bash
CODE:
```
npx magicui-cli add --all
```

----------------------------------------

TITLE: Rebuilding iOS App Binary
DESCRIPTION: This command rebuilds and runs the React Native application on an iOS simulator or device. It's necessary after linking native code or making changes to native dependencies to ensure the new library is properly integrated.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/blog/react-native-libraries.mdx#_snippet_2

LANGUAGE: Bash
CODE:
```
npm run ios
```

----------------------------------------

TITLE: Using RetroGrid Component
DESCRIPTION: Demonstrates how to embed the `RetroGrid` component within a `div` container. The container is styled to be `relative`, have a fixed height, full width, and `overflow-hidden` to properly contain the animated grid effect.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/retro-grid.mdx#_snippet_3

LANGUAGE: tsx
CODE:
```
<div className="relative h-[500px] w-full overflow-hidden">
  <RetroGrid />
</div>
```

----------------------------------------

TITLE: Adding CSS Animations for Animated Gradient Text
DESCRIPTION: This CSS block defines the `--animate-gradient` custom property and the `gradient` keyframe animation. These styles are crucial for the animated background effect of the text component and must be added to your global CSS file, typically `app/globals.css`.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/animated-gradient-text.mdx#_snippet_1

LANGUAGE: css
CODE:
```
@theme inline {
  --animate-gradient: gradient 8s linear infinite;

  @keyframes gradient {
    to {
      background-position: var(--bg-size, 300%) 0;
    }
  }
}
```

----------------------------------------

TITLE: Defining Ripple Button CSS Animations (CSS)
DESCRIPTION: This CSS block defines the `@keyframes` for the `rippling` animation, which controls the visual expansion and fading of the ripple effect. It also sets a CSS variable `--animate-rippling` for easy application, and should be included in a global stylesheet like `app/globals.css`.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/ripple-button.mdx#_snippet_1

LANGUAGE: css
CODE:
```
@theme inline {
  --animate-rippling: rippling var(--duration) ease-out;

  @keyframes rippling {
    0% {
      opacity: 1;
    }
    100% {
      transform: scale(2);
      opacity: 0;
    }
  }
}
```

----------------------------------------

TITLE: Defining Rainbow CSS Animation
DESCRIPTION: This CSS block defines the `rainbow` keyframe animation, which shifts the `background-position` to create a continuous rainbow effect. The `--animate-rainbow` custom property applies this animation with a configurable speed.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/rainbow-button.mdx#_snippet_2

LANGUAGE: css
CODE:
```
@theme inline {
  --animate-rainbow: rainbow var(--speed, 2s) infinite linear;

  @keyframes rainbow {
    0% {
      background-position: 0%;
    }
    100% {
      background-position: 200%;
    }
  }
}
```

----------------------------------------

TITLE: Using CardActionArea with Material UI Card (JSX)
DESCRIPTION: This snippet demonstrates how to make an area within a Material UI Card clickable using the `CardActionArea` component. It wraps the interactive contents of the card, making them responsive to user interaction.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/blog/mui-card.mdx#_snippet_0

LANGUAGE: jsx
CODE:
```
import { CardActionArea } from "@mui/material";
import Card from "@mui/material/Card";
<Card>
  <CardActionArea>{/* Card Contents */}</CardActionArea>
</Card>;
```

----------------------------------------

TITLE: Navigating into the Project Directory
DESCRIPTION: This command changes the current directory in the terminal to the newly created 'my-tailwind-app' project folder. This is a prerequisite for running further commands within the project context.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/blog/install-tailwind-react.mdx#_snippet_1

LANGUAGE: javascript
CODE:
```
cd my-tailwind-app
```

----------------------------------------

TITLE: Adding Tailwind CDN to HTML File
DESCRIPTION: This snippet demonstrates how to include the Tailwind CSS CDN link within the <head> section of an HTML document. Placing the script tag here ensures that Tailwind CSS is loaded and available before the page content is rendered, enabling the use of Tailwind utility classes throughout the HTML body.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/blog/tailwind-cdn-html.mdx#_snippet_0

LANGUAGE: html
CODE:
```
<!doctype html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Tailwind CDN Example</title>
    <script src="https://cdn.tailwindcss.com"></script>
  </head>
  <body>
    <!-- Your content goes here -->
  </body>
</html>
```

----------------------------------------

TITLE: Install Tailwind CSS and Autoprefixer (Bash)
DESCRIPTION: Install Tailwind CSS and Autoprefixer as development dependencies using npm.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/installation/remix.mdx#_snippet_3

LANGUAGE: bash
CODE:
```
npm add -D tailwindcss@latest autoprefixer@latest
```

----------------------------------------

TITLE: Integrating React Bootstrap Components in a React App
DESCRIPTION: This snippet demonstrates how to integrate React Bootstrap components like `Container`, `Row`, and `Col` into a React application. It shows the basic structure for using these components to create a responsive layout. The `react-bootstrap` package must be installed as a prerequisite to use these components.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/blog/react-bootstrap.mdx#_snippet_0

LANGUAGE: javascript
CODE:
```
import { Container, Row, Col } from "react-bootstrap";
function App() {
  return (
    <Container>
      <Row>
        <Col>Column 1</Col>
        <Col>Column 2</Col>
      </Row>
    </Container>
  );
}
export default App;
```

----------------------------------------

TITLE: Displaying Multiple Tweets in a Column Layout using JSX
DESCRIPTION: This snippet illustrates how to display multiple tweets within a responsive column layout using a `div` container with Tailwind CSS classes. The `columns-3` class arranges the content into three columns, while `gap-4` adds spacing. Each `Tweet` component requires a unique `id` prop to embed a specific tweet, showcasing user testimonials or related social media content.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/story.mdx#_snippet_1

LANGUAGE: JSX
CODE:
```
<div className="columns-3 gap-4 [&>*]:mb-4">
  <Tweet id="1722659583464968612" />
  <Tweet id="1668442365625790465" />
  <Tweet id="1684613797984800768" />
  <Tweet id="1722690049374830639" />
  <Tweet id="1684676668445622272" />
  <Tweet id="1668640878225764354" />
  <Tweet id="1668506160620769280" />
  <Tweet id="1681477151042797568" />
</div>
```

----------------------------------------

TITLE: Defining CSS Class for Active Element Exiting Transition
DESCRIPTION: This CSS snippet defines the active state for an element during its exiting transition. It sets `opacity: 0;` and applies a `transition` property to smoothly animate the opacity over 500 milliseconds, creating a fade-out effect.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/blog/mui-transitions.mdx#_snippet_19

LANGUAGE: css
CODE:
```
.fade-exit-active {
  opacity: 0;
  transition: opacity 500ms;
}
```

----------------------------------------

TITLE: Importing Dot Pattern Component (TypeScript/TSX)
DESCRIPTION: This snippet demonstrates the standard ES module import syntax for bringing the `DotPattern` component into your React or Next.js application. The path `"@/components/magicui/dot-pattern"` assumes a typical alias setup for your component directory.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/dot-pattern.mdx#_snippet_1

LANGUAGE: tsx
CODE:
```
import { DotPattern } from "@/components/magicui/dot-pattern";
```

----------------------------------------

TITLE: Importing Interactive Hover Button Component (TypeScript/React)
DESCRIPTION: This code snippet illustrates how to import the InteractiveHoverButton component into a TypeScript/React application. This import statement makes the component accessible for use within other React components, typically referencing a local path like '@/components/magicui/'.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/interactive-hover-button.mdx#_snippet_1

LANGUAGE: tsx
CODE:
```
import { InteractiveHoverButton } from "@/components/magicui/interactive-hover-button";
```

----------------------------------------

TITLE: Defining SpringConfig Type for SmoothCursor Animation
DESCRIPTION: This TypeScript interface defines the `SpringConfig` type, which is used to customize the physics-based animation behavior of the `SmoothCursor`. It includes properties like `damping`, `stiffness`, `mass`, and `restDelta` to fine-tune the animation's responsiveness and settling behavior.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/smooth-cursor.mdx#_snippet_4

LANGUAGE: typescript
CODE:
```
interface SpringConfig {
  damping: number; // Controls how quickly the animation settles
  stiffness: number; // Controls the spring stiffness
  mass: number; // Controls the virtual mass of the animated object
  restDelta: number; // Controls the threshold at which animation is considered complete
}
```

----------------------------------------

TITLE: Installing Motion Dependency (Bash)
DESCRIPTION: Installs the `motion` library, a crucial dependency for the `HyperText` component's animation effects, using the npm package manager.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/hyper-text.mdx#_snippet_1

LANGUAGE: bash
CODE:
```
npm install motion
```

----------------------------------------

TITLE: Using Animated Subscribe Button Component (TypeScript/React)
DESCRIPTION: This snippet illustrates how to render the `AnimatedSubscribeButton` component within your application. The component requires exactly two children elements: the first represents the unsubscribed state (e.g., 'Follow'), and the second represents the subscribed state (e.g., 'Subscribed'). The button animates between these two states upon interaction.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/animated-subscribe-button.mdx#_snippet_2

LANGUAGE: tsx
CODE:
```
<AnimatedSubscribeButton>
  <span>Follow</span>
  <span>Subscribed</span>
</AnimatedSubscribeButton>
```

----------------------------------------

TITLE: Adding All shadcn-ui Components via magicui CLI - Bash
DESCRIPTION: This command installs all available shadcn-ui components using the magicui CLI, leveraging both the `--shadcn` and `--all` flags.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/packages/cli/README.md#_snippet_9

LANGUAGE: bash
CODE:
```
npx magicui-cli add --shadcn --all
```

----------------------------------------

TITLE: Adding Ripple CSS Animations (CSS)
DESCRIPTION: This CSS snippet defines the `@keyframes` for the `ripple` animation and applies it via a custom property `--animate-ripple` within a `@theme inline` block. These animations are crucial for the visual effect of the Ripple component and should be added to your global CSS file.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/ripple.mdx#_snippet_1

LANGUAGE: css
CODE:
```
@theme inline {
  --animate-ripple: ripple var(--duration, 2s) ease calc(var(--i, 0) * 0.2s)
    infinite;

  @keyframes ripple {
    0%,
    100% {
      transform: translate(-50%, -50%) scale(1);
    }
    50% {
      transform: translate(-50%, -50%) scale(0.9);
    }
  }
}
```

----------------------------------------

TITLE: Adding Shine Border CSS Animations
DESCRIPTION: This CSS block defines the `@keyframes shine` animation and a custom property `--animate-shine` for the animated background border effect. These styles should be added to your global CSS file to enable the visual animation.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/shine-border.mdx#_snippet_1

LANGUAGE: css
CODE:
```
@theme inline {
  --animate-shine: shine var(--duration) infinite linear;

  @keyframes shine {
    0% {
      background-position: 0% 0%;
    }
    50% {
      background-position: 100% 100%;
    }
    to {
      background-position: 0% 0%;
    }
  }
}
```

----------------------------------------

TITLE: Listing Available magicui Components - Bash
DESCRIPTION: Running the `add` command without any arguments displays a list of all available magicui components that can be added to the project.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/packages/cli/README.md#_snippet_7

LANGUAGE: bash
CODE:
```
npx magicui-cli add
```

----------------------------------------

TITLE: Using Pointer Component in TSX
DESCRIPTION: This snippet shows the basic JSX usage of the `Pointer` component. Once imported, it can be rendered directly within your React application's JSX to display the interactive pointer effect on hover.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/pointer.mdx#_snippet_2

LANGUAGE: tsx
CODE:
```
<Pointer />
```

----------------------------------------

TITLE: Including Tailwind CSS CDN in HTML
DESCRIPTION: This HTML snippet demonstrates how to include Tailwind CSS directly via a CDN link in the `<head>` section of your `index.html` file. This method allows for quick setup and styling without a full build process, providing immediate access to Tailwind's utility classes.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/blog/tailwind-landing-page.mdx#_snippet_2

LANGUAGE: html
CODE:
```
<link
  href="https://cdn.jsdelivr.net/npm/tailwindcss/dist/tailwind.min.css"
  rel="stylesheet"
/>
```

----------------------------------------

TITLE: Importing CoolMode Component in TSX
DESCRIPTION: This code snippet shows the standard way to import the `CoolMode` component into a TypeScript React file. It makes the component accessible for use within your JSX/TSX code, typically from a local path within your project.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/cool-mode.mdx#_snippet_1

LANGUAGE: tsx
CODE:
```
import { CoolMode } from "@/components/magicui/cool-mode";
```

----------------------------------------

TITLE: Importing Flickering Grid Component in TSX
DESCRIPTION: This snippet shows the standard import statement for the `FlickeringGrid` component. It allows you to use the component within your React/Next.js applications by importing it from its defined path, typically within a `components/magicui` directory.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/flickering-grid.mdx#_snippet_1

LANGUAGE: tsx
CODE:
```
import { FlickeringGrid } from "@/components/magicui/flickering-grid";
```

----------------------------------------

TITLE: Importing SmoothCursor Component in React
DESCRIPTION: This snippet demonstrates how to import the `SmoothCursor` component from its module path. This import statement is necessary before the component can be used in any React functional component or page.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/smooth-cursor.mdx#_snippet_2

LANGUAGE: tsx
CODE:
```
import { SmoothCursor } from "@/components/ui/smooth-cursor";
```

----------------------------------------

TITLE: Importing Icon Cloud Component (TypeScript/React)
DESCRIPTION: This snippet shows how to import the `IconCloud` component from the `magicui` library into a TypeScript/React project, making it available for use in your JSX.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/icon-cloud.mdx#_snippet_1

LANGUAGE: tsx
CODE:
```
import { IconCloud } from "@/components/magicui/icon-cloud";
```

----------------------------------------

TITLE: Importing Ripple Button Component (TypeScript React)
DESCRIPTION: This `import` statement is used in a TypeScript React file to bring the `RippleButton` component into scope. It allows the component to be rendered and utilized within your application, assuming the specified path matches your project's component structure.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/ripple-button.mdx#_snippet_2

LANGUAGE: tsx
CODE:
```
import { RippleButton } from "@/components/magicui/ripple-button";
```

----------------------------------------

TITLE: Importing Meteors Component in TSX
DESCRIPTION: This line imports the `Meteors` component from the specified path, making it available for use in your React/Next.js application. Ensure the import path matches your project's structure.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/meteors.mdx#_snippet_2

LANGUAGE: tsx
CODE:
```
import { Meteors } from "@/components/magicui/meteors";
```

----------------------------------------

TITLE: Installing Interactive Grid Pattern via CLI (Bash)
DESCRIPTION: This snippet demonstrates how to add the `interactive-grid-pattern` component to your project using the `shadcn` CLI. This command automates the process of fetching and integrating the component's code into your setup.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/interactive-grid-pattern.mdx#_snippet_0

LANGUAGE: bash
CODE:
```
npx shadcn@latest add "https://magicui.design/r/interactive-grid-pattern"
```

----------------------------------------

TITLE: Installing Ripple Button via Shadcn CLI (Bash)
DESCRIPTION: This `npx` command utilizes the `shadcn` CLI to add the `RippleButton` component to your project. It fetches the component definition from the specified MagicUI design URL, automating the setup process.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/ripple-button.mdx#_snippet_0

LANGUAGE: bash
CODE:
```
npx shadcn@latest add "https://magicui.design/r/ripple-button"
```

----------------------------------------

TITLE: Installing Material UI with npm (Bash)
DESCRIPTION: This command installs the `@material-ui/core` package, which contains the core Material UI components, into the React project using npm. It's a necessary step to use Material UI components.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/blog/mui-box.mdx#_snippet_3

LANGUAGE: bash
CODE:
```
npm install @material-ui/core
```

----------------------------------------

TITLE: Rendering MagicUI Text Animation Components in React
DESCRIPTION: This JavaScript snippet demonstrates how to import and render `FadeIn` and `Typewriter` components from the MagicUI library within a React functional component. It illustrates basic usage for displaying animated text elements on a web page.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/blog/text-animation-css.mdx#_snippet_3

LANGUAGE: JavaScript
CODE:
```
import { FadeIn, Typewriter } from "magicui";

function MyComponent() {
  return (
    <div>
      <FadeIn>Welcome to my website!</FadeIn>
      <Typewriter text="This is a typing effect." />
    </div>
  );
}
```

----------------------------------------

TITLE: Installing React Virtualized Library
DESCRIPTION: This command installs the `react-virtualized` library, a popular solution for implementing list virtualization in React applications. It's added as a dependency to the project, enabling efficient rendering of large lists by only displaying visible items.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/blog/react-component-best-practices.mdx#_snippet_0

LANGUAGE: Shell
CODE:
```
npm install react-virtualized --save
```

----------------------------------------

TITLE: Initializing React Project with Create React App
DESCRIPTION: This command initializes a new React application named 'my-app' using Create React App. It sets up the basic project structure and installs necessary dependencies for a React development environment.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/blog/react-header.mdx#_snippet_0

LANGUAGE: Shell
CODE:
```
npx create-react-app my-app
```

----------------------------------------

TITLE: Installing Animated Grid Pattern via CLI
DESCRIPTION: Provides the command-line interface (CLI) method for adding the AnimatedGridPattern component to a project using `npx shadcn`.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/animated-grid-pattern.mdx#_snippet_0

LANGUAGE: bash
CODE:
```
npx shadcn@latest add "https://magicui.design/r/animated-grid-pattern"
```

----------------------------------------

TITLE: Installing Flickering Grid Component via CLI
DESCRIPTION: This snippet demonstrates how to install the Flickering Grid component using the `shadcn/ui` CLI. It fetches the component from the specified URL and adds it to your project, simplifying the setup process.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/flickering-grid.mdx#_snippet_0

LANGUAGE: bash
CODE:
```
npx shadcn@latest add "https://magicui.design/r/flickering-grid"
```

----------------------------------------

TITLE: Installing Particles Component via CLI (Bash)
DESCRIPTION: This snippet demonstrates how to install the Particles component using the `shadcn` CLI. It adds the component directly to your project from the MagicUI repository.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/particles.mdx#_snippet_0

LANGUAGE: bash
CODE:
```
npx shadcn@latest add "https://magicui.design/r/particles"
```

----------------------------------------

TITLE: Installing Animated Gradient Text Component via CLI
DESCRIPTION: This command uses `npx shadcn` to add the `AnimatedGradientText` component to your project, simplifying the installation process by fetching it from the specified MagicUI design URL.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/animated-gradient-text.mdx#_snippet_0

LANGUAGE: bash
CODE:
```
npx shadcn@latest add "https://magicui.design/r/animated-gradient-text"
```

----------------------------------------

TITLE: Installing Animated Circular Progress Bar via CLI
DESCRIPTION: This command installs the `AnimatedCircularProgressBar` component using the `shadcn` CLI, fetching it from the specified MagicUI design URL. It's the recommended way for quick setup.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/animated-circular-progress-bar.mdx#_snippet_0

LANGUAGE: bash
CODE:
```
npx shadcn@latest add "https://magicui.design/r/animated-circular-progress-bar"
```

----------------------------------------

TITLE: Installing Typing Animation Component via CLI (Bash)
DESCRIPTION: This snippet demonstrates how to install the `TypingAnimation` component using the `shadcn/ui` CLI. It adds the component directly to your project, simplifying the setup process.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/typing-animation.mdx#_snippet_0

LANGUAGE: bash
CODE:
```
npx shadcn@latest add "https://magicui.design/r/typing-animation"
```

----------------------------------------

TITLE: Adding Tailwind CSS Integration to Astro (Bash)
DESCRIPTION: This command integrates Tailwind CSS into an Astro project using the Astro CLI, setting up necessary configurations for styling.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/installation/astro.mdx#_snippet_3

LANGUAGE: bash
CODE:
```
npx astro add tailwind
```

----------------------------------------

TITLE: Fixing Linting Issues with pnpm
DESCRIPTION: This command automatically attempts to fix linting errors and warnings detected in the codebase, improving code quality and adherence to best practices. It's recommended to run this before committing.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/CONTRIBUTING.md#_snippet_15

LANGUAGE: bash
CODE:
```
pnpm lint:fix
```

----------------------------------------

TITLE: Use ClientTweetCard on Client Side
DESCRIPTION: Shows how to use the ClientTweetCard component in a client-side React component. It requires the "use client"; directive and involves importing the component and providing a tweet ID.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/tweet-card.mdx#_snippet_2

LANGUAGE: tsx
CODE:
```
"use client";

import { ClientTweetCard } from "@/components/magicui/client-tweet-card";

export default function App() {
  return <ClientTweetCard id="1441032681968212480" />;
}
```

----------------------------------------

TITLE: Installing Motion Dependency Manually - Bash
DESCRIPTION: This `npm install` command adds the `motion` library, a crucial dependency for the Word Rotate component's animation capabilities, to your project when opting for manual installation.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/word-rotate.mdx#_snippet_1

LANGUAGE: bash
CODE:
```
npm install motion
```

----------------------------------------

TITLE: Interactive Configuration Prompts for components.json
DESCRIPTION: This snippet displays the typical interactive questions presented during the `shadcn init` process to configure the `components.json` file. Users are prompted to choose a style, a base color, and whether to use CSS variables for styling.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/installation/next.mdx#_snippet_2

LANGUAGE: txt
CODE:
```
Which style would you like to use? › New York
Which color would you like to use as base color? › Zinc
Do you want to use CSS variables for colors? › no / yes
```

----------------------------------------

TITLE: Rendering Shiny Button Component (TSX)
DESCRIPTION: This example illustrates how to use the `ShinyButton` component within your TSX code. The text 'Shiny Button' is passed as children, which will be displayed inside the button. Additional props like `className` can be used for styling.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/shiny-button.mdx#_snippet_2

LANGUAGE: tsx
CODE:
```
<ShinyButton>Shiny Button</ShinyButton>
```

----------------------------------------

TITLE: Using Line Shadow Text Component (TSX)
DESCRIPTION: This example illustrates how to render the `LineShadowText` component in a TSX file, wrapping the desired text content. The component will display "Magic UI" with the animated line shadow effect, applying the styles and animations defined elsewhere.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/line-shadow-text.mdx#_snippet_3

LANGUAGE: tsx
CODE:
```
<LineShadowText>Magic UI</LineShadowText>
```

----------------------------------------

TITLE: Rendering Typing Animation Component (TypeScript/TSX)
DESCRIPTION: This snippet demonstrates how to render the `TypingAnimation` component with basic text content. The `children` prop defines the text that will be animated.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/typing-animation.mdx#_snippet_2

LANGUAGE: tsx
CODE:
```
<TypingAnimation>Typing Animation</TypingAnimation>
```

----------------------------------------

TITLE: Basic Usage of iPhone 15 Pro Component (TSX)
DESCRIPTION: This snippet demonstrates how to render the `Iphone15Pro` component in a TSX file. When used without specific props, the component will display with its default `width` and `height`.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/iphone-15-pro.mdx#_snippet_2

LANGUAGE: tsx
CODE:
```
<Iphone15Pro />
```

----------------------------------------

TITLE: Using MagicCard Component with Children (TSX)
DESCRIPTION: This snippet demonstrates how to use the `MagicCard` component by wrapping content within it. The `div` element with `p` and `span` tags will be rendered inside the card, benefiting from its spotlight and border highlight effects.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/magic-card.mdx#_snippet_2

LANGUAGE: tsx
CODE:
```
<MagicCard>
  <div className="p-4">
    <p>Hello World</p>
    <span>Hover me</span>
  </div>
</MagicCard>
```

----------------------------------------

TITLE: Installing Styled Components for CSS-in-JS Styling
DESCRIPTION: This command installs the 'styled-components' library, a popular tool for writing CSS directly within JavaScript files. It facilitates component-scoped styling, improving maintainability and organization of styles in React applications.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/blog/react-header.mdx#_snippet_3

LANGUAGE: Shell
CODE:
```
npm install styled-components
```

----------------------------------------

TITLE: Importing CSS into a React Component
DESCRIPTION: This line imports the `App.css` stylesheet into a React component, making its styles available for use within that component. It's a standard way to link external CSS files in React applications.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/blog/react-hero-component.mdx#_snippet_5

LANGUAGE: tsx
CODE:
```
import "./App.css";
```

----------------------------------------

TITLE: Defining a Custom Spacing Calculation Function in Sass
DESCRIPTION: This example demonstrates a Sass function `calculate-spacing` that takes two parameters, `$base` and `$multiplier`, and returns their product. Sass functions enable dynamic styling by performing calculations and manipulations directly within the stylesheet, improving efficiency and reducing errors.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/blog/react-hero-component.mdx#_snippet_3

LANGUAGE: SCSS
CODE:
```
@function calculate-spacing($base, $multiplier) { @return $base * $multiplier; }
```

----------------------------------------

TITLE: Configuring Astro Project Setup (Text)
DESCRIPTION: This snippet shows the typical interactive prompts and expected answers when configuring a new Astro project, covering project location, template, TypeScript usage, dependency installation, and Git initialization.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/installation/astro.mdx#_snippet_1

LANGUAGE: txt
CODE:
```
- Where should we create your new project? ./your-app-name
- How would you like to start your new project? Choose a template
- Do you plan to write TypeScript? Yes
- How strict should TypeScript be? Strict
- Install dependencies? Yes
- Initialize a new git repository? (optional) Yes/No
```

----------------------------------------

TITLE: Install Canvas Confetti Dependency
DESCRIPTION: This command installs the `canvas-confetti` package, which is a core dependency required for the Confetti component to function. This step is part of the manual installation process.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/confetti.mdx#_snippet_1

LANGUAGE: bash
CODE:
```
npm install canvas-confetti
```

----------------------------------------

TITLE: Installing Magic UI MCP CLI for Cline (Bash)
DESCRIPTION: This snippet demonstrates how to install the Magic UI Model Context Protocol (MCP) CLI for the Cline IDE. It uses npx to execute the latest version of the @magicuidesign/cli package, which sets up the necessary integration for AI-assisted code generation within Cline.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/mcp.mdx#_snippet_3

LANGUAGE: bash
CODE:
```
npx @magicuidesign/cli@latest install cline
```

----------------------------------------

TITLE: Installing MagicUI CLI
DESCRIPTION: This command installs the necessary dependencies for the MagicUI Command Line Interface (CLI) tool, making it ready for execution. It's the first step to set up the CLI locally.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/CONTRIBUTING.md#_snippet_17

LANGUAGE: bash
CODE:
```
pnpm run install:cli
```

----------------------------------------

TITLE: Creating a Basic HTML Page with Tailwind CSS
DESCRIPTION: This snippet demonstrates how to create a simple HTML page and integrate Tailwind CSS using its CDN. It showcases the application of various Tailwind utility classes to style a container, heading, paragraph, and button, illustrating basic layout and typography.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/blog/tailwind-cdn-html.mdx#_snippet_1

LANGUAGE: html
CODE:
```
<!doctype html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <title>Tailwind CDN Example</title>
    <script src="https://cdn.tailwindcss.com"></script>
  </head>
  <body>
    <div class="container mx-auto p-4">
      <h1 class="text-4xl font-bold text-center text-gray-800">
        Hello, Tailwind!
      </h1>
      <p class="mt-4 text-lg text-gray-600">
        This is a paragraph styled with Tailwind CSS.
      </p>
      <button class="mt-4 px-4 py-2 bg-blue-500 text-white rounded">
        Click Me
      </button>
    </div>
  </body>
</html>
```

----------------------------------------

TITLE: Installing Project Dependencies - pnpm
DESCRIPTION: This command installs all necessary project dependencies using pnpm. It ensures that all required packages for building and running MagicUI are available in your local environment.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/CONTRIBUTING.md#_snippet_3

LANGUAGE: Bash
CODE:
```
pnpm i
```

----------------------------------------

TITLE: Installing MagicUI with npm
DESCRIPTION: This command demonstrates how to install the MagicUI library using the npm package manager. It is the first step required to set up MagicUI in your project, making its components available for use.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/blog/animation-libraries.mdx#_snippet_0

LANGUAGE: bash
CODE:
```
npm install magicui
```

----------------------------------------

TITLE: Running React Application with Tailwind CSS
DESCRIPTION: This command starts the development server for a React application, allowing you to preview the application in a web browser. It is essential for verifying that Tailwind CSS has been correctly configured and applied to your project.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/blog/tailwind-font-size.mdx#_snippet_0

LANGUAGE: bash
CODE:
```
npm start
```

----------------------------------------

TITLE: Rendering InteractiveGridPattern Component (TypeScript/TSX)
DESCRIPTION: This snippet illustrates how to render the `InteractiveGridPattern` component within a React/TSX application. It's wrapped in a `div` with Tailwind CSS classes to define its dimensions and overflow behavior, demonstrating a common way to embed the pattern.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/interactive-grid-pattern.mdx#_snippet_2

LANGUAGE: tsx
CODE:
```
<div className="relative h-[500px] w-full overflow-hidden">
  <InteractiveGridPattern />
</div>
```

----------------------------------------

TITLE: Basic Usage of MagicUI SpinningText Component (TSX)
DESCRIPTION: This snippet illustrates how to render the `SpinningText` component in a React application, wrapping the desired text content. The component will animate the enclosed text in a circular motion with default properties.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/spinning-text.mdx#_snippet_2

LANGUAGE: tsx
CODE:
```
<SpinningText>learn more • earn more • grow more •</SpinningText>
```

----------------------------------------

TITLE: Defining Animated Text Element in HTML
DESCRIPTION: This HTML snippet defines a basic heading element with a unique ID, 'animated-text'. This ID serves as a crucial selector for applying specific CSS animation rules, making the text a target for dynamic visual effects.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/blog/text-animation-css.mdx#_snippet_0

LANGUAGE: HTML
CODE:
```
<h1 id="animated-text">Animated Text</h1>
```

----------------------------------------

TITLE: Importing MagicUI Components in React
DESCRIPTION: This JavaScript snippet shows how to import specific components (Button, Card, Modal) from the MagicUI library into a React application. This allows developers to use these pre-built animated UI elements within their React components.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/blog/animation-libraries.mdx#_snippet_1

LANGUAGE: javascript
CODE:
```
import { Button, Card, Modal } from "magicui";
```

----------------------------------------

TITLE: Default Spring Configuration for SmoothCursor Animation
DESCRIPTION: This constant defines the default values for the `SpringConfig` object used by the `SmoothCursor` component. These values provide a balanced and smooth animation out-of-the-box, controlling the damping, stiffness, mass, and the animation's completion threshold.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/smooth-cursor.mdx#_snippet_5

LANGUAGE: typescript
CODE:
```
const defaultSpringConfig = {
  damping: 45,
  stiffness: 400,
  mass: 1,
  restDelta: 0.001
};
```

----------------------------------------

TITLE: Use Border Beam Component in JSX
DESCRIPTION: Wrap the BorderBeam component within a container element with relative positioning and overflow hidden to display the animated border effect.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/border-beam.mdx#_snippet_2

LANGUAGE: tsx
CODE:
```
<div className="relative h-[500px] w-full overflow-hidden">
  <BorderBeam />
</div>
```

----------------------------------------

TITLE: Using ShineBorder Component in JSX
DESCRIPTION: This JSX snippet demonstrates how to embed the `ShineBorder` component within a `div` container. The container provides necessary styling (relative positioning, height, width, and overflow) for the animated border effect to be properly rendered and visible.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/shine-border.mdx#_snippet_3

LANGUAGE: tsx
CODE:
```
<div className="relative h-[500px] w-full overflow-hidden">
  <ShineBorder />
</div>
```

----------------------------------------

TITLE: Basic Marquee Component Usage
DESCRIPTION: This snippet demonstrates the basic usage of the `Marquee` component, wrapping multiple `<span>` elements as children. The component will infinitely scroll these children horizontally by default, creating a continuous display.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/marquee.mdx#_snippet_3

LANGUAGE: tsx
CODE:
```
<Marquee>
  <span>Next.js</span>
  <span>React</span>
  <span>TypeScript</span>
  <span>Tailwind CSS</span>
</Marquee>
```

----------------------------------------

TITLE: Using MorphingText Component with Text Array
DESCRIPTION: This example illustrates how to render the `MorphingText` component, passing an array of strings to the `texts` prop. The component will dynamically transition between these provided text values, creating a visual morphing effect.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/morphing-text.mdx#_snippet_2

LANGUAGE: tsx
CODE:
```
<MorphingText texts={["Hello", "World"]} />
```

----------------------------------------

TITLE: Integrating Dot Pattern Component into JSX/TSX
DESCRIPTION: This example shows how to render the `DotPattern` component within a JSX/TSX structure. It's placed inside a `div` with Tailwind CSS classes to define its dimensions and ensure the pattern is contained and clipped, making it suitable for background effects.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/dot-pattern.mdx#_snippet_2

LANGUAGE: tsx
CODE:
```
<div className="relative h-[500px] w-full overflow-hidden">
  <DotPattern />
</div>
```

----------------------------------------

TITLE: Integrating SmoothCursor Component into a React Page
DESCRIPTION: This example shows how to integrate the `SmoothCursor` component into a React page. By placing `<SmoothCursor />` within the JSX, the smooth cursor animation will be active across the entire page content. It should be placed at the top level of your application or page layout.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/smooth-cursor.mdx#_snippet_1

LANGUAGE: tsx
CODE:
```
import { SmoothCursor } from "@/components/ui/smooth-cursor";

export default function Page() {
  return (
    <>
      <SmoothCursor />
      {/* Your page content */}
    </>
  );
}
```

----------------------------------------

TITLE: Adding CSS Orbit Animations
DESCRIPTION: This CSS block defines the `--animate-orbit` custom property and the `@keyframes orbit` animation. These animations are crucial for the circular motion of the `OrbitingCircles` component, using CSS variables for dynamic control over duration, angle, and radius.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/orbiting-circles.mdx#_snippet_1

LANGUAGE: css
CODE:
```
@theme inline {
  --animate-orbit: orbit calc(var(--duration) * 1s) linear infinite;

  @keyframes orbit {
    0% {
      transform: rotate(calc(var(--angle) * 1deg))
        translateY(calc(var(--radius) * 1px)) rotate(calc(var(--angle) * -1deg));
    }
    100% {
      transform: rotate(calc(var(--angle) * 1deg + 360deg))
        translateY(calc(var(--radius) * 1px))
        rotate(calc((var(--angle) * -1deg) - 360deg));
    }
  }
}
```

----------------------------------------

TITLE: Implementing Orbiting Circles Component in TSX
DESCRIPTION: This example demonstrates how to use the `OrbitingCircles` component within a React application. It shows two instances: one with default settings and another with a custom `radius` and `reverse` animation, containing various Lucide icons as children.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/orbiting-circles.mdx#_snippet_3

LANGUAGE: tsx
CODE:
```
<div className="relative overflow-hidden h-[500px] w-full">
  <OrbitingCircles>
    <File />
    <Settings />
    <File />
  </OrbitingCircles>
  <OrbitingCircles radius={100} reverse>
    <File />
    <Settings />
    <File />
    <Search />
  </OrbitingCircles>
</div>
```

----------------------------------------

TITLE: Using Lens Component with an Image in JSX
DESCRIPTION: This example illustrates how to embed an `img` element within the `Lens` component. The `img` element serves as the content that will be magnified when the lens is interacted with, providing an interactive zoom feature.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/lens.mdx#_snippet_2

LANGUAGE: tsx
CODE:
```
<Lens>\n  <img src="/images/lens-demo.jpg" alt="Lens Demo" />\n</Lens>
```

----------------------------------------

TITLE: Using Animated Shiny Text Component - TSX
DESCRIPTION: Example of how to use the AnimatedShinyText component in a TSX file. Wrap the desired text content within the component tags to apply the shiny effect.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/animated-shiny-text.mdx#_snippet_3

LANGUAGE: tsx
CODE:
```
<AnimatedShinyText>Animated Shiny Text</AnimatedShinyText>
```

----------------------------------------

TITLE: Importing Terminal Components (TypeScript/React)
DESCRIPTION: This snippet demonstrates how to import the `Terminal`, `AnimatedSpan`, and `TypingAnimation` components from the MagicUI library. These components are essential for constructing an animated terminal interface in a React application.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/terminal.mdx#_snippet_1

LANGUAGE: tsx
CODE:
```
import {
  AnimatedSpan,
  Terminal,
  TypingAnimation,
} from "@/components/magicui/terminal";
```

----------------------------------------

TITLE: Installing Magic UI MCP CLI for Windsurf (Bash)
DESCRIPTION: This snippet demonstrates how to install the Magic UI Model Context Protocol (MCP) CLI for the Windsurf IDE. It uses npx to execute the latest version of the @magicuidesign/cli package, which sets up the necessary integration for AI-assisted code generation within Windsurf.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/mcp.mdx#_snippet_1

LANGUAGE: bash
CODE:
```
npx @magicuidesign/cli@latest install windsurf
```

----------------------------------------

TITLE: Configure components.json (Text)
DESCRIPTION: Example output showing the questions asked during the shadcn/ui initialization process to configure `components.json`.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/installation/remix.mdx#_snippet_2

LANGUAGE: txt
CODE:
```
Which style would you like to use? › New York
Which color would you like to use as base color? › Zinc
Do you want to use CSS variables for colors? › no / yes
```

----------------------------------------

TITLE: Setting Up React App with create-react-app (Bash)
DESCRIPTION: This command initializes a new React application named 'my-app' using `create-react-app` and then navigates into the newly created project directory. It's a prerequisite for developing React applications.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/blog/mui-box.mdx#_snippet_0

LANGUAGE: bash
CODE:
```
npx create-react-app my-app && cd my-app
```

----------------------------------------

TITLE: Configuring shadcn/ui components.json (CLI)
DESCRIPTION: This snippet shows the interactive command-line prompts for configuring `components.json` during shadcn/ui initialization. Users select their preferred style, base color, and whether to use CSS variables for colors, customizing the component library's appearance.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/installation/vite.mdx#_snippet_7

LANGUAGE: txt
CODE:
```
Which style would you like to use? › New York
Which color would you like to use as base color? › Zinc
Do you want to use CSS variables for colors? › no / yes
```

----------------------------------------

TITLE: Adding CSS Animations for Shimmer Button
DESCRIPTION: Defines global CSS animations (`shimmer-slide` and `spin-around`) required for the Shimmer Button's visual effects, including movement and rotation, within a `@theme inline` block. These animations control the shimmering light and the overall button appearance, ensuring the visual effect functions correctly.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/shimmer-button.mdx#_snippet_1

LANGUAGE: css
CODE:
```
@theme inline {
  --animate-shimmer-slide: shimmer-slide var(--speed) ease-in-out infinite
    alternate;
  --animate-spin-around: spin-around calc(var(--speed) * 2) infinite linear;

  @keyframes shimmer-slide {
    to {
      transform: translate(calc(100cqw - 100%), 0);
    }
  }
  @keyframes spin-around {
    0% {
      transform: translateZ(0) rotate(0);
    }
    15%,
    35% {
      transform: translateZ(0) rotate(90deg);
    }
    65%,
    85% {
      transform: translateZ(0) rotate(270deg);
    }
    100% {
      transform: translateZ(0) rotate(360deg);
    }
  }
}
```

----------------------------------------

TITLE: Adding CSS Animations for Neon Gradient Card
DESCRIPTION: This CSS snippet defines the `background-position-spin` keyframe animation and applies it via a theme variable. These animations are crucial for the dynamic neon gradient effect and should be added to a global CSS file like `app/globals.css`.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/neon-gradient-card.mdx#_snippet_1

LANGUAGE: css
CODE:
```
@theme inline {
  --animate-background-position-spin: background-position-spin 3000ms infinite
    alternate;

  @keyframes background-position-spin {
    0% {
      background-position: top center;
    }
    100% {
      background-position: bottom center;
    }
  }
}
```

----------------------------------------

TITLE: Updating Documentation Sidebar - TypeScript
DESCRIPTION: This TypeScript object literal defines an entry for the new component in the documentation sidebar configuration (`config/docs.ts`). It includes the component's title, href, and a 'New' label for navigation.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/CONTRIBUTING.md#_snippet_8

LANGUAGE: TypeScript
CODE:
```
{
    title: "Example Component",
    href: `/docs/components/example-component`,
    items: [],
    label: "New",
}
```

----------------------------------------

TITLE: Rendering AnimatedGridPattern Component
DESCRIPTION: Demonstrates how to render the `AnimatedGridPattern` component in a TSX file, using its default configuration.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/animated-grid-pattern.mdx#_snippet_2

LANGUAGE: tsx
CODE:
```
<AnimatedGridPattern />
```

----------------------------------------

TITLE: Rendering Flickering Grid Component in TSX
DESCRIPTION: This snippet demonstrates how to render the `FlickeringGrid` component in a TSX file. Once imported, the component can be used as a self-closing JSX tag to display the flickering grid background in your application.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/flickering-grid.mdx#_snippet_2

LANGUAGE: tsx
CODE:
```
<FlickeringGrid />
```

----------------------------------------

TITLE: Render Confetti Component in JSX
DESCRIPTION: This JSX snippet demonstrates how to render the `Confetti` component within a React application. When rendered, it will display the confetti animation on the page.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/confetti.mdx#_snippet_3

LANGUAGE: tsx
CODE:
```
<Confetti />
```

----------------------------------------

TITLE: Basic Usage of SmoothCursor Component in JSX
DESCRIPTION: This snippet illustrates the most basic usage of the `SmoothCursor` component. Once imported, it can be rendered as a self-closing JSX tag to enable the default smooth cursor animation on the page.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/smooth-cursor.mdx#_snippet_3

LANGUAGE: tsx
CODE:
```
<SmoothCursor />
```

----------------------------------------

TITLE: Rendering CodeComparison Component in TSX
DESCRIPTION: This snippet illustrates how to render the `CodeComparison` component, passing `beforeCode` and `afterCode` props. These props are crucial for providing the actual code content that the component will display and compare.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/code-comparison.mdx#_snippet_2

LANGUAGE: tsx
CODE:
```
<CodeComparison beforeCode={beforeCode} afterCode={afterCode} />
```

----------------------------------------

TITLE: Add a shadcn/ui Component (Bash)
DESCRIPTION: Use the shadcn/ui CLI to add a specific component, such as the Button, to the project.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/installation/remix.mdx#_snippet_7

LANGUAGE: bash
CODE:
```
npx shadcn@latest add button
```

----------------------------------------

TITLE: Adding shadcn/ui Button Component (Bash)
DESCRIPTION: This command uses `npx` to add the `Button` component from the `shadcn/ui` library to the current project. It fetches the component's source code and integrates it into the project's component directory.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/installation/laravel.mdx#_snippet_3

LANGUAGE: bash
CODE:
```
npx shadcn@latest add button
```

----------------------------------------

TITLE: Rendering HeroVideoDialog with Props (TypeScript/React)
DESCRIPTION: This example shows how to render the `HeroVideoDialog` component, configuring its visual presentation and video content using `className`, `animationStyle`, `videoSrc`, `thumbnailSrc`, and `thumbnailAlt` props.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/hero-video-dialog.mdx#_snippet_2

LANGUAGE: tsx
CODE:
```
<HeroVideoDialog
  className="block dark:hidden"
  animationStyle="from-center"
  videoSrc="https://www.example.com/dummy-video"
  thumbnailSrc="https://www.example.com/dummy-thumbnail.png"
  thumbnailAlt="Dummy Video Thumbnail"
/>
```

----------------------------------------

TITLE: Defining CSS Class for Element Entering State
DESCRIPTION: This CSS snippet defines the initial state for an element as it begins to enter a transition. Setting `opacity: 0;` ensures the element is invisible at the start of its entering animation.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/blog/mui-transitions.mdx#_snippet_16

LANGUAGE: css
CODE:
```
.fade-enter {
  opacity: 0;
}
```

----------------------------------------

TITLE: Installing Magic UI MCP CLI for Roo-Cline (Bash)
DESCRIPTION: This snippet demonstrates how to install the Magic UI Model Context Protocol (MCP) CLI for the Roo-Cline IDE. It uses npx to execute the latest version of the @magicuidesign/cli package, which sets up the necessary integration for AI-assisted code generation within Roo-Cline.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/mcp.mdx#_snippet_4

LANGUAGE: bash
CODE:
```
npx @magicuidesign/cli@latest install roo-cline
```

----------------------------------------

TITLE: Configuring shadcn-ui components.json for magicui Integration - JSON
DESCRIPTION: This JSON snippet shows the necessary additions to the `components.json` file to integrate magicui components into an existing shadcn-ui project, by adding `ui` and `magicui` aliases.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/packages/cli/README.md#_snippet_2

LANGUAGE: json
CODE:
```
{
  "$schema": "https://ui.shadcn.com/schema.json",
  "style": "default",
  "rsc": true,
  "tsx": true,
  "tailwind": {
    "config": "tailwind.config.js",
    "css": "app/globals.css",
    "baseColor": "slate",
    "cssVariables": true
  },
  "aliases": {
    "components": "@/components",
    "utils": "@/lib/utils",
+   "ui": "@/components/ui",
+   "magicui": "@/components/magicui"
  }
}
```

----------------------------------------

TITLE: Rendering VideoText Component in TSX
DESCRIPTION: This snippet illustrates how to render the `VideoText` component within a React application. It wraps the component in a container with specific styling and passes a video source URL and text content.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/video-text.mdx#_snippet_2

LANGUAGE: tsx
CODE:
```
<div className="relative h-[500px] w-full overflow-hidden">
  <VideoText src="https://cdn.magicui.design/ocean-small.webm">OCEAN</VideoText>
</div>
```

----------------------------------------

TITLE: Rendering FlipText Component in TSX
DESCRIPTION: This snippet demonstrates how to render the `FlipText` component, passing 'Flip Text' as its children. The component will apply a character animation to the provided text, creating a dynamic visual effect.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/flip-text.mdx#_snippet_2

LANGUAGE: tsx
CODE:
```
<FlipText>Flip Text</FlipText>
```

----------------------------------------

TITLE: Rendering VelocityScroll Component (TSX)
DESCRIPTION: This snippet demonstrates how to render the `VelocityScroll` component, wrapping content that will be animated based on scroll velocity. The content 'Scroll Based Velocity' will be displayed and animated.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/scroll-based-velocity.mdx#_snippet_3

LANGUAGE: tsx
CODE:
```
<VelocityScroll>Scroll Based Velocity</VelocityScroll>
```

----------------------------------------

TITLE: Embedding a Single Tweet using JSX
DESCRIPTION: This snippet demonstrates how to embed a single tweet into a web page using the `Tweet` JSX component. It requires a valid `id` prop, which corresponds to the unique identifier of the tweet to be displayed. This component is typically used in React or Next.js environments to dynamically render social media content.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/story.mdx#_snippet_0

LANGUAGE: JSX
CODE:
```
<Tweet id="1668408059125702661" />
```

----------------------------------------

TITLE: Importing Box Reveal Component in TSX
DESCRIPTION: This snippet shows the necessary import statement to bring the `BoxReveal` component into your TypeScript React (TSX) file, typically from a local components path.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/box-reveal.mdx#_snippet_1

LANGUAGE: tsx
CODE:
```
import { BoxReveal } from "@/components/magicui/box-reveal";
```

----------------------------------------

TITLE: Importing AvatarCircles Component (TypeScript/React)
DESCRIPTION: This line imports the `AvatarCircles` component from the specified local path, making it available for use within a TypeScript or React application. It's a prerequisite for rendering the component.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/avatar-circles.mdx#_snippet_1

LANGUAGE: tsx
CODE:
```
import { AvatarCircles } from "@/components/magicui/avatar-circles";
```

----------------------------------------

TITLE: Importing SparklesText Component (TypeScript/TSX)
DESCRIPTION: This snippet imports the `SparklesText` component from the local `magicui` components directory, making it available for use in your React or Next.js application. It is a prerequisite for using the component in any TSX file.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/sparkles-text.mdx#_snippet_1

LANGUAGE: tsx
CODE:
```
import { SparklesText } from "@/components/magicui/sparkles-text";
```

----------------------------------------

TITLE: Importing GridPattern Component (TypeScript/TSX)
DESCRIPTION: This snippet shows the necessary import statement for the `GridPattern` component. It assumes the component is located in the `@/components/magicui` directory, which is a common setup for `shadcn` based projects.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/grid-pattern.mdx#_snippet_1

LANGUAGE: tsx
CODE:
```
import { GridPattern } from "@/components/magicui/grid-pattern";
```

----------------------------------------

TITLE: Importing Lens Component in TypeScript/React
DESCRIPTION: This snippet demonstrates how to import the `Lens` component from the local `magicui/lens` path. This makes the component available for use within your TypeScript/React application, typically at the top of a component file.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/lens.mdx#_snippet_1

LANGUAGE: tsx
CODE:
```
import { Lens } from "@/components/magicui/lens";
```

----------------------------------------

TITLE: Importing iPhone 15 Pro Component (TSX)
DESCRIPTION: This snippet illustrates the required import statement for the `Iphone15Pro` component in a TypeScript React (TSX) file. Users should adjust the import path to align with their project's file structure.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/iphone-15-pro.mdx#_snippet_1

LANGUAGE: tsx
CODE:
```
import { Iphone15Pro } from "@/components/magicui/iphone-15-pro";
```

----------------------------------------

TITLE: Importing AnimatedGridPattern Component
DESCRIPTION: Imports the `AnimatedGridPattern` component from the local components directory, making it available for use in a React/TypeScript file.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/animated-grid-pattern.mdx#_snippet_1

LANGUAGE: tsx
CODE:
```
import { AnimatedGridPattern } from "@/components/magicui/animated-grid-pattern";
```

----------------------------------------

TITLE: Importing Safari Component (TypeScript React)
DESCRIPTION: This snippet demonstrates the standard way to import the `Safari` component into a TypeScript React file, making it accessible for rendering.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/safari.mdx#_snippet_1

LANGUAGE: tsx
CODE:
```
import { Safari } from "@/components/magicui/safari";
```

----------------------------------------

TITLE: Importing Animated Gradient Text Component in TSX
DESCRIPTION: This import statement brings the `AnimatedGradientText` component into your TypeScript React file. It specifies the path to the component within your project's `components/magicui` directory, making it available for use in your JSX/TSX code.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/animated-gradient-text.mdx#_snippet_2

LANGUAGE: tsx
CODE:
```
import { AnimatedGradientText } from "@/components/magicui/animated-gradient-text";
```

----------------------------------------

TITLE: Importing Line Shadow Text Component (TSX)
DESCRIPTION: This snippet demonstrates the standard way to import the `LineShadowText` component into a TypeScript React (TSX) file. It makes the component available for use within your React application, typically from a local components directory.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/line-shadow-text.mdx#_snippet_2

LANGUAGE: tsx
CODE:
```
import { LineShadowText } from "@/components/magicui/line-shadow-text";
```

----------------------------------------

TITLE: Importing Text Reveal Component in TSX
DESCRIPTION: This snippet imports the `TextReveal` component from the `magicui` library, making it available for use in your React or Next.js application. Ensure the import path matches your project's setup.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/text-reveal.mdx#_snippet_1

LANGUAGE: tsx
CODE:
```
import { TextReveal } from "@/components/magicui/text-reveal";
```

----------------------------------------

TITLE: Importing NumberTicker Component (TypeScript/TSX)
DESCRIPTION: This TypeScript/TSX snippet shows how to import the `NumberTicker` component from its module path. This import statement is a necessary prerequisite for using the component within a React or Next.js application, making it available for rendering.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/number-ticker.mdx#_snippet_1

LANGUAGE: tsx
CODE:
```
import { NumberTicker } from "@/components/magicui/number-ticker";
```

----------------------------------------

TITLE: Importing ShineBorder Component in TSX
DESCRIPTION: This line imports the `ShineBorder` component from the specified local path, making it available for use within your React or Next.js application's JSX/TSX files.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/shine-border.mdx#_snippet_2

LANGUAGE: tsx
CODE:
```
import { ShineBorder } from "@/registry/magicui/shine-border";
```

----------------------------------------

TITLE: Importing Warp Background Component in TypeScript/React
DESCRIPTION: This snippet shows the necessary import statement to bring the `WarpBackground` component into a TypeScript/React file, assuming a path alias for `@/components/magicui`.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/warp-background.mdx#_snippet_1

LANGUAGE: tsx
CODE:
```
import { WarpBackground } from "@/components/magicui/warp-background";
```

----------------------------------------

TITLE: Importing Typing Animation Component (TypeScript/TSX)
DESCRIPTION: This snippet shows how to import the `TypingAnimation` component from its module path. It's a prerequisite for using the component in your React/Next.js application.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/typing-animation.mdx#_snippet_1

LANGUAGE: tsx
CODE:
```
import { TypingAnimation } from "@/components/magicui/typing-animation";
```

----------------------------------------

TITLE: Importing ScrollProgress Component (TypeScript/TSX)
DESCRIPTION: This snippet shows the necessary import statement to bring the `ScrollProgress` component into your TypeScript or TSX file. It specifies the module path from where the component is exported, making it available for use in your application.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/scroll-progress.mdx#_snippet_1

LANGUAGE: tsx
CODE:
```
import { ScrollProgress } from "@/components/magicui/scroll-progress";
```

----------------------------------------

TITLE: Import ScratchToReveal Component (TypeScript/TSX)
DESCRIPTION: This import statement makes the `ScratchToReveal` component available for use within your TypeScript or TSX files, typically in a React or Next.js application.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/scratch-to-reveal.mdx#_snippet_1

LANGUAGE: tsx
CODE:
```
import { ScratchToReveal } from "@/components/magicui/scratch-to-reveal";
```

----------------------------------------

TITLE: Importing Word Rotate Component - TypeScript/TSX
DESCRIPTION: This TypeScript import statement makes the `WordRotate` component available for use in your React/TSX files. The path assumes a typical project structure where `magicui` components are located under `@/components`.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/word-rotate.mdx#_snippet_2

LANGUAGE: tsx
CODE:
```
import { WordRotate } from "@/components/magicui/word-rotate";
```

----------------------------------------

TITLE: Importing ShimmerButton Component in TSX
DESCRIPTION: Imports the `ShimmerButton` component from the local `magicui` components directory, making it available for use in a TypeScript React application. This is the first step to integrate the component into a React project, allowing it to be rendered in JSX.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/shimmer-button.mdx#_snippet_2

LANGUAGE: tsx
CODE:
```
import { ShimmerButton } from "@/components/magicui/shimmer-button";
```

----------------------------------------

TITLE: Creating Showcase MDX File
DESCRIPTION: This snippet demonstrates the frontmatter structure for a new MDX file used to define a showcase entry. It includes metadata such as title, description, image path, external link, featured status, and affiliation details for a website.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/CONTRIBUTING.md#_snippet_16

LANGUAGE: mdx
CODE:
```
---
title: website-name.com
description: Website description
image: /showcase/website-name.png
href: https://website-name.com
featured: true
affiliation: YC S25, raised $10M
---
```

----------------------------------------

TITLE: Building MagicUI CLI
DESCRIPTION: This command compiles and bundles the MagicUI CLI for production use, optimizing it for performance and distribution. It prepares the CLI for release.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/CONTRIBUTING.md#_snippet_19

LANGUAGE: bash
CODE:
```
pnpm run build:cli
```

----------------------------------------

TITLE: Building MagicUI Registry via pnpm
DESCRIPTION: This command initiates the build process for the MagicUI registry, compiling and preparing all registered components and examples for deployment or use. It's a crucial step after making changes to the registry files.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/CONTRIBUTING.md#_snippet_13

LANGUAGE: bash
CODE:
```
pnpm build:registry
```

----------------------------------------

TITLE: Installing Sparkles Text Component via CLI (Bash)
DESCRIPTION: This command uses `npx` to add the `SparklesText` component from `magicui.design` to your project, leveraging the `shadcn/ui` CLI for easy integration. It automates the process of fetching and adding the component's source code.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/sparkles-text.mdx#_snippet_0

LANGUAGE: bash
CODE:
```
npx shadcn@latest add "https://magicui.design/r/sparkles-text"
```

----------------------------------------

TITLE: Installing Orbiting Circles Component via CLI
DESCRIPTION: This command uses `npx shadcn` to add the Orbiting Circles component to your project, simplifying the installation process. It fetches the component directly from the magicui.design repository, automating the setup.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/orbiting-circles.mdx#_snippet_0

LANGUAGE: bash
CODE:
```
npx shadcn@latest add "https://magicui.design/r/orbiting-circles"
```

----------------------------------------

TITLE: Installing Word Rotate Component via CLI - Bash
DESCRIPTION: This `npx shadcn` command automates the installation of the Word Rotate component, including its dependencies, directly into your project, simplifying the setup process.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/word-rotate.mdx#_snippet_0

LANGUAGE: bash
CODE:
```
npx shadcn@latest add "https://magicui.design/r/word-rotate"
```

----------------------------------------

TITLE: Installing Text Reveal Component using CLI
DESCRIPTION: This command uses the `shadcn` CLI to automatically add the Text Reveal component to your project, including its dependencies and configuration, simplifying the setup process.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/text-reveal.mdx#_snippet_0

LANGUAGE: bash
CODE:
```
npx shadcn@latest add "https://magicui.design/r/text-reveal"
```

----------------------------------------

TITLE: Installing Pointer Component via CLI
DESCRIPTION: This command uses the `shadcn` CLI to add the `Pointer` component from the `magicui.design` registry to your project. It automates the process of fetching and integrating the component, including its dependencies and styles.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/pointer.mdx#_snippet_0

LANGUAGE: bash
CODE:
```
npx shadcn@latest add "https://magicui.design/r/pointer"
```

----------------------------------------

TITLE: Install Border Beam Component using CLI
DESCRIPTION: Use the shadcn/ui CLI with the magicui registry to automatically add the Border Beam component and its dependencies to your project.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/border-beam.mdx#_snippet_0

LANGUAGE: bash
CODE:
```
npx shadcn@latest add "https://magicui.design/r/border-beam"
```

----------------------------------------

TITLE: Installing AnimatedList Component via CLI
DESCRIPTION: This snippet demonstrates how to add the `AnimatedList` component to your project using the `shadcn/ui` command-line interface (CLI). This is the recommended and most straightforward installation method.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/animated-list.mdx#_snippet_0

LANGUAGE: bash
CODE:
```
npx shadcn@latest add "https://magicui.design/r/animated-list"
```

----------------------------------------

TITLE: Installing Aurora Text Component via CLI
DESCRIPTION: Installs the Aurora Text component using the shadcn CLI, fetching it from the magicui.design repository. This command simplifies the integration process by automating the download and setup.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/aurora-text.mdx#_snippet_0

LANGUAGE: bash
CODE:
```
npx shadcn@latest add "https://magicui.design/r/aurora-text"
```

----------------------------------------

TITLE: Rendering Safari Component with URL (TypeScript React)
DESCRIPTION: This example shows how to render the `Safari` component, passing a `url` prop to specify the website to be displayed within the mockup browser window.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/safari.mdx#_snippet_2

LANGUAGE: tsx
CODE:
```
<Safari url="https://magicui.design" />
```

----------------------------------------

TITLE: Render ScratchToReveal Component with Props (TypeScript/TSX)
DESCRIPTION: This example demonstrates how to use the `ScratchToReveal` component, specifying its `width`, `height`, and `minScratchPercentage`. The `children` prop defines the content revealed after scratching.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/scratch-to-reveal.mdx#_snippet_2

LANGUAGE: tsx
CODE:
```
<ScratchToReveal width={300} height={300} minScratchPercentage={50}>
  Scratch To Reveal
</ScratchToReveal>
```

----------------------------------------

TITLE: Rendering Globe Component in TSX
DESCRIPTION: This JSX snippet demonstrates how to render the `Globe` component within a React application. It instantiates the component, making the interactive globe visible in the UI.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/globe.mdx#_snippet_3

LANGUAGE: tsx
CODE:
```
<Globe />
```

----------------------------------------

TITLE: Running MagicUI CLI in Development Mode
DESCRIPTION: This command starts the MagicUI CLI in development mode, typically enabling hot-reloading and verbose logging for easier debugging and iterative development. It connects to a local registry on port 3000.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/CONTRIBUTING.md#_snippet_18

LANGUAGE: bash
CODE:
```
pnpm run dev:cli
```

----------------------------------------

TITLE: Adding Required CSS Animations for Shiny Text - CSS
DESCRIPTION: CSS code defining the @keyframes shiny-text animation and applying it via a CSS variable --animate-shiny-text. These animations create the light glare effect for the shiny text component. Add this to your global CSS file.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/animated-shiny-text.mdx#_snippet_1

LANGUAGE: css
CODE:
```
@theme inline {
  --animate-shiny-text: shiny-text 8s infinite;

  @keyframes shiny-text {
    0%,
    90%,
    100% {
      background-position: calc(-100% - var(--shiny-width)) 0;
    }
    30%,
    60% {
      background-position: calc(100% + var(--shiny-width)) 0;
    }
  }
}
```

----------------------------------------

TITLE: Importing Animated Subscribe Button (TypeScript/React)
DESCRIPTION: This snippet shows the necessary import statement for integrating the `AnimatedSubscribeButton` component into a TypeScript or React project. Ensure that the import path is updated to correctly match your project's specific file structure.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/animated-subscribe-button.mdx#_snippet_1

LANGUAGE: tsx
CODE:
```
import { AnimatedSubscribeButton } from "@/components/magicui/animated-subscribe-button";
```

----------------------------------------

TITLE: Importing Animated Shiny Text Component - TSX
DESCRIPTION: Imports the AnimatedShinyText component from the specified path. This line is required to use the component in your TSX/React files.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/animated-shiny-text.mdx#_snippet_2

LANGUAGE: tsx
CODE:
```
import { AnimatedShinyText } from "@/components/magicui/animated-shiny-text";
```

----------------------------------------

TITLE: Importing MagicUI SpinningText Component (TSX)
DESCRIPTION: This snippet demonstrates the standard import statement for the `SpinningText` component in a TypeScript React application, making it available for use within your JSX/TSX files.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/spinning-text.mdx#_snippet_1

LANGUAGE: tsx
CODE:
```
import { SpinningText } from \"@/components/magicui/spinning-text\";
```

----------------------------------------

TITLE: Importing HyperText Component (TypeScript/TSX)
DESCRIPTION: Imports the `HyperText` component from its specified module path. This line is essential for making the component available for use within a TypeScript/TSX file.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/hyper-text.mdx#_snippet_2

LANGUAGE: tsx
CODE:
```
import { HyperText } from "@/components/magicui/hyper-text";
```

----------------------------------------

TITLE: Importing HeroVideoDialog Component (TypeScript/React)
DESCRIPTION: This snippet demonstrates how to import the `HeroVideoDialog` component from its local path, making it ready for use within a TypeScript or React application.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/hero-video-dialog.mdx#_snippet_1

LANGUAGE: tsx
CODE:
```
import { HeroVideoDialog } from "@/components/magicui/hero-video-dialog";
```

----------------------------------------

TITLE: Importing Particles Component (TypeScript/React)
DESCRIPTION: This snippet shows the standard way to import the `Particles` component into a TypeScript or React file, making it available for use in your application.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/particles.mdx#_snippet_1

LANGUAGE: tsx
CODE:
```
import { Particles } from "@/components/magicui/particles";
```

----------------------------------------

TITLE: Import Confetti Component in TypeScript/React
DESCRIPTION: This import statement brings the `Confetti` component into scope, allowing it to be used within a TypeScript or React application. The path assumes a specific project structure, typically found in `src/components/magicui/confetti`.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/confetti.mdx#_snippet_2

LANGUAGE: tsx
CODE:
```
import { Confetti } from "@/components/magicui/confetti";
```

----------------------------------------

TITLE: Importing Ripple Component (TypeScript/TSX)
DESCRIPTION: This snippet demonstrates how to import the `Ripple` component from the specified path. This import statement makes the component available for use within your TypeScript or TSX files.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/ripple.mdx#_snippet_2

LANGUAGE: tsx
CODE:
```
import { Ripple } from "@/components/magicui/ripple";
```

----------------------------------------

TITLE: Importing Android Component in TSX
DESCRIPTION: This snippet shows how to import the `Android` component from the `magicui` library into a TypeScript React (TSX) project, making it available for use in your application.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/android.mdx#_snippet_1

LANGUAGE: tsx
CODE:
```
import { Android } from "@/components/magicui/android";
```

----------------------------------------

TITLE: Importing VelocityScroll Component (TSX)
DESCRIPTION: This snippet imports the `VelocityScroll` component from its module path, making it available for use in a React/Next.js application.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/scroll-based-velocity.mdx#_snippet_2

LANGUAGE: tsx
CODE:
```
import { VelocityScroll } from "@/components/magicui/scroll-based-velocity";
```

----------------------------------------

TITLE: Navigating into React Project Directory
DESCRIPTION: After creating a new React project, this command changes the current directory to the newly created project folder, 'my-app', allowing further operations within the project context.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/blog/react-header.mdx#_snippet_1

LANGUAGE: Shell
CODE:
```
cd my-app
```

----------------------------------------

TITLE: Registering Example Component in TypeScript
DESCRIPTION: This snippet shows how to register an example or demo for a component, 'example-component-demo', in the `registry-examples.ts` file. It links the example to its corresponding UI component via `registryDependencies` and specifies its file path.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/CONTRIBUTING.md#_snippet_12

LANGUAGE: typescript
CODE:
```
export const examples: Registry = [
  // ... existing examples ...
  {
    name: "example-component-demo",
    description: "An example of the example-component",
    type: "registry:example",
    registryDependencies: ["example-component"],
    files: [
      {
        path: "registry/example/example-component-demo.tsx",
        type: "registry:example",
      },
    ],
  },
];
```

----------------------------------------

TITLE: Configuring shadcn/ui components.json (Text Output)
DESCRIPTION: This snippet shows the interactive prompts displayed during the `shadcn` initialization process. Users are asked to select a style, a base color, and whether to use CSS variables for colors, which configures the `components.json` file.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/installation/laravel.mdx#_snippet_2

LANGUAGE: txt
CODE:
```
Which style would you like to use?
Which color would you like to use as base color?
Do you want to use CSS variables for colors? › yes
```

----------------------------------------

TITLE: Installing ScrollProgress via CLI (Bash)
DESCRIPTION: This snippet demonstrates how to add the `ScrollProgress` component to your project using the `shadcn` CLI. This command fetches and integrates the component's source code into your local project structure, simplifying the setup process.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/scroll-progress.mdx#_snippet_0

LANGUAGE: bash
CODE:
```
npx shadcn@latest add "https://magicui.design/r/scroll-progress"
```

----------------------------------------

TITLE: Installing RetroGrid via CLI
DESCRIPTION: This command uses `npx shadcn` to add the RetroGrid component to your project, simplifying the installation process by fetching it directly from the MagicUI registry.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/retro-grid.mdx#_snippet_0

LANGUAGE: bash
CODE:
```
npx shadcn@latest add "https://magicui.design/r/retro-grid"
```

----------------------------------------

TITLE: Installing Shiny Button via CLI (Bash)
DESCRIPTION: This command demonstrates how to install the Shiny Button component using the `shadcn` CLI. It fetches the component from the specified URL and integrates it into your project, simplifying the setup process.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/shiny-button.mdx#_snippet_0

LANGUAGE: bash
CODE:
```
npx shadcn@latest add "https://magicui.design/r/shiny-button"
```

----------------------------------------

TITLE: Installing Text Animate Component via CLI
DESCRIPTION: This snippet demonstrates how to install the `TextAnimate` component using the `shadcn` CLI. This command fetches and adds the component's code to your project, simplifying the setup process.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/text-animate.mdx#_snippet_0

LANGUAGE: bash
CODE:
```
npx shadcn@latest add "https://magicui.design/r/text-animate"
```

----------------------------------------

TITLE: Installing Android Component via CLI
DESCRIPTION: This snippet demonstrates how to add the Android component to your project using the `shadcn` command-line interface (CLI), simplifying the installation process.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/android.mdx#_snippet_0

LANGUAGE: bash
CODE:
```
npx shadcn@latest add "https://magicui.design/r/android"
```

----------------------------------------

TITLE: Adding Component CSS Animations - CSS
DESCRIPTION: This CSS code block defines custom CSS variables and keyframe animations required for the `example` component. These styles should be added to the global CSS file (e.g., `app/globals.css`) to enable the component's visual effects.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/CONTRIBUTING.md#_snippet_10

LANGUAGE: CSS
CODE:
```
--animate-example: example var(--duration) infinite linear;

@keyframes example {
  from {
    transform: translateX(0);
  }
  to {
    transform: translateX(calc(-100% - var(--gap)));
  }
}
```

----------------------------------------

TITLE: Creating Component Demo - TypeScript
DESCRIPTION: This TypeScript code creates a demo component (`ExampleComponentDemo`) that imports and renders the `ExampleComponent`. It provides a basic example for showcasing the new component within the MagicUI documentation.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/CONTRIBUTING.md#_snippet_7

LANGUAGE: TypeScript
CODE:
```
import ExampleComponent from '@/registry/magicui/example-component'

export default function ExampleComponentDemo() {
  return (
    <div className="relative justify-center">
      <ExampleComponent />
    </div>
  )
}
```

----------------------------------------

TITLE: Starting React App with Yarn (Bash)
DESCRIPTION: This command starts the development server for the React application using Yarn. It typically launches the app in the browser at `http://localhost:3000/`.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/blog/mui-box.mdx#_snippet_1

LANGUAGE: bash
CODE:
```
yarn start
```

----------------------------------------

TITLE: Navigating to Project Directory - Bash
DESCRIPTION: This command changes the current directory to `magicui`, which is the root of the cloned repository. It's essential to be in this directory for subsequent project operations.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/CONTRIBUTING.md#_snippet_1

LANGUAGE: Bash
CODE:
```
cd magicui
```

----------------------------------------

TITLE: Importing RetroGrid Component
DESCRIPTION: Imports the `RetroGrid` component from the specified local path (`@/components/magicui/retro-grid`), making it available for use within your React or Next.js application.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/retro-grid.mdx#_snippet_2

LANGUAGE: tsx
CODE:
```
import { RetroGrid } from "@/components/magicui/retro-grid";
```

----------------------------------------

TITLE: Importing MagicCard Component (TSX)
DESCRIPTION: This line imports the `MagicCard` component from its defined path within the project's registry. It's the first step to using the component in a React/Next.js application.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/magic-card.mdx#_snippet_1

LANGUAGE: tsx
CODE:
```
import { MagicCard } from "@/registry/magicui/magic-card";
```

----------------------------------------

TITLE: Importing RainbowButton Component
DESCRIPTION: This line imports the `RainbowButton` component from its module path, making it available for use in a React or Next.js application. It's a prerequisite for rendering the button.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/rainbow-button.mdx#_snippet_3

LANGUAGE: tsx
CODE:
```
import { RainbowButton } from "@/components/magicui/rainbow-button";
```

----------------------------------------

TITLE: Importing CodeComparison Component in TSX
DESCRIPTION: This snippet demonstrates how to import the `CodeComparison` component from its module path within a TypeScript React (TSX) file. This is a prerequisite step before the component can be used in your application's UI.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/code-comparison.mdx#_snippet_1

LANGUAGE: tsx
CODE:
```
import { CodeComparison } from "@/components/magicui/code-comparison";
```

----------------------------------------

TITLE: Import Border Beam Component
DESCRIPTION: Import the BorderBeam component from the magicui components directory to use it in your React/Next.js application.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/components/border-beam.mdx#_snippet_1

LANGUAGE: tsx
CODE:
```
import { BorderBeam } from "@/components/magicui/border-beam";
```

----------------------------------------

TITLE: Adding magicui Example & Demo Components - Bash
DESCRIPTION: This command, with the `--example` flag, allows users to select and install example and demo components as seen on the magicui website.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/packages/cli/README.md#_snippet_6

LANGUAGE: bash
CODE:
```
npx magicui-cli add --example
```

----------------------------------------

TITLE: Installing Magic UI MCP CLI for Claude (Bash)
DESCRIPTION: This snippet demonstrates how to install the Magic UI Model Context Protocol (MCP) CLI for the Claude IDE. It uses npx to execute the latest version of the @magicuidesign/cli package, which sets up the necessary integration for AI-assisted code generation within Claude.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/content/docs/mcp.mdx#_snippet_2

LANGUAGE: bash
CODE:
```
npx @magicuidesign/cli@latest install claude
```

----------------------------------------

TITLE: Releasing MagicUI CLI
DESCRIPTION: This command executes the release process for the MagicUI CLI, which typically involves versioning, packaging, and publishing the tool. It's the final step before making the CLI available to users.
SOURCE: https://github.com/magicuidesign/magicui/blob/main/CONTRIBUTING.md#_snippet_20

LANGUAGE: bash
CODE:
```
pnpm run release:cli
```