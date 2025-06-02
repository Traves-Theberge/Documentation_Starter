TITLE: Basic react-markdown usage example
DESCRIPTION: A simple example demonstrating how to use the react-markdown component to render markdown content in a React application.
SOURCE: https://github.com/remarkjs/react-markdown/blob/main/readme.md#2025-04-20_snippet_3

LANGUAGE: jsx
CODE:
```
import React from 'react'
import {createRoot} from 'react-dom/client'
import Markdown from 'react-markdown'

const markdown = '# Hi, *Pluto*!'

createRoot(document.body).render(<Markdown>{markdown}</Markdown>)
```

----------------------------------------

TITLE: Using react-markdown with remark-gfm plugin
DESCRIPTION: Example showing how to use the react-markdown component with the remark-gfm plugin to add support for additional markdown features like footnotes, strikethrough, tables, tasklists, and direct URLs.
SOURCE: https://github.com/remarkjs/react-markdown/blob/main/readme.md#2025-04-20_snippet_4

LANGUAGE: jsx
CODE:
```
import React from 'react'
import {createRoot} from 'react-dom/client'
import Markdown from 'react-markdown'
import remarkGfm from 'remark-gfm'

const markdown = `Just a link: www.nasa.gov.`

createRoot(document.body).render(
  <Markdown remarkPlugins={[remarkGfm]}>{markdown}</Markdown>
)
```

----------------------------------------

TITLE: Customizing Component Rendering in React Markdown
DESCRIPTION: Example showing how to override default HTML components in react-markdown. This snippet demonstrates mapping h1 elements to h2 and customizing em elements with red text styling.
SOURCE: https://github.com/remarkjs/react-markdown/blob/main/readme.md#2025-04-20_snippet_15

LANGUAGE: javascript
CODE:
```
<Markdown
  components={{
    // Map `h1` (`# heading`) to use `h2`s.
    h1: 'h2',
    // Rewrite `em`s (`*like so*`) to `i` with a red foreground color.
    em(props) {
      const {node, ...rest} = props
      return <i style={{color: 'red'}} {...rest} />
    }
  }}
/>
```

----------------------------------------

TITLE: Custom Components with Syntax Highlighting in React Markdown
DESCRIPTION: This example demonstrates how to use custom components to override default element rendering in React Markdown. It implements syntax highlighting for code blocks using react-syntax-highlighter with the Prism theme.
SOURCE: https://github.com/remarkjs/react-markdown/blob/main/readme.md#2025-04-20_snippet_9

LANGUAGE: javascript
CODE:
```
import React from 'react'
import {createRoot} from 'react-dom/client'
import Markdown from 'react-markdown'
import {Prism as SyntaxHighlighter} from 'react-syntax-highlighter'
import {dark} from 'react-syntax-highlighter/dist/esm/styles/prism'

// Did you know you can use tildes instead of backticks for code in markdown? ✨
const markdown = `Here is some JavaScript code:

~~~js
console.log('It works!')
~~~
`

createRoot(document.body).render(
  <Markdown
    children={markdown}
    components={{
      code(props) {
        const {children, className, node, ...rest} = props
        const match = /language-(\w+)/.exec(className || '')
        return match ? (
          <SyntaxHighlighter
            {...rest}
            PreTag="div"
            children={String(children).replace(/\n$/, '')}
            language={match[1]}
            style={dark}
          />
        ) : (
          <code {...rest} className={className}>
            {children}
          </code>
        )
      }
    }}
  />
)
```

----------------------------------------

TITLE: Using remark-gfm Plugin with React Markdown
DESCRIPTION: This snippet demonstrates how to use the remark-gfm plugin with React Markdown to support GitHub Flavored Markdown features like strikethrough, tables, tasklists and URL auto-linking. The example renders a markdown string with these features.
SOURCE: https://github.com/remarkjs/react-markdown/blob/main/readme.md#2025-04-20_snippet_5

LANGUAGE: javascript
CODE:
```
import React from 'react'
import {createRoot} from 'react-dom/client'
import Markdown from 'react-markdown'
import remarkGfm from 'remark-gfm'

const markdown = `A paragraph with *emphasis* and **strong importance**.

> A block quote with ~strikethrough~ and a URL: https://reactjs.org.

* Lists
* [ ] todo
* [x] done

A table:

| a | b |
| - | - |
`

createRoot(document.body).render(
  <Markdown remarkPlugins={[remarkGfm]}>{markdown}</Markdown>
)
```

----------------------------------------

TITLE: Rendering HTML in Markdown with rehype-raw
DESCRIPTION: This example shows how to use rehype-raw plugin to render HTML embedded within markdown. This approach should only be used in trusted environments as it bypasses the default HTML escaping in React Markdown, which could pose security risks.
SOURCE: https://github.com/remarkjs/react-markdown/blob/main/readme.md#2025-04-20_snippet_13

LANGUAGE: javascript
CODE:
```
import React from 'react'
import {createRoot} from 'react-dom/client'
import Markdown from 'react-markdown'
import rehypeRaw from 'rehype-raw'

const markdown = `<div class="note">

Some *emphasis* and <strong>strong</strong>!

</div>`

createRoot(document.body).render(
  <Markdown rehypePlugins={[rehypeRaw]}>{markdown}</Markdown>
)
```

----------------------------------------

TITLE: Math Equations in React Markdown with KaTeX
DESCRIPTION: This example demonstrates how to render mathematical equations in React Markdown using remark-math for parsing math syntax and rehype-katex for rendering it with KaTeX. It includes importing the KaTeX CSS for proper styling.
SOURCE: https://github.com/remarkjs/react-markdown/blob/main/readme.md#2025-04-20_snippet_11

LANGUAGE: javascript
CODE:
```
import React from 'react'
import {createRoot} from 'react-dom/client'
import Markdown from 'react-markdown'
import rehypeKatex from 'rehype-katex'
import remarkMath from 'remark-math'
import 'katex/dist/katex.min.css' // `rehype-katex` does not import the CSS for you

const markdown = `The lift coefficient ($C_L$) is a dimensionless coefficient.`

createRoot(document.body).render(
  <Markdown remarkPlugins={[remarkMath]} rehypePlugins={[rehypeKatex]}>
    {markdown}
  </Markdown>
)
```

----------------------------------------

TITLE: Recommended Pattern for Markdown in JSX
DESCRIPTION: Best practice for handling line endings and whitespace when using markdown in JSX. This approach avoids indentation and whitespace issues by storing markdown in a variable and passing it as an expression.
SOURCE: https://github.com/remarkjs/react-markdown/blob/main/readme.md#2025-04-20_snippet_16

LANGUAGE: javascript
CODE:
```
// If you write actual markdown in your code, put your markdown in a variable;
// **do not indent markdown**:
const markdown = `
# This is perfect!
`

// Pass the value as an expression as an only child:
const result = <Markdown>{markdown}</Markdown>
```

----------------------------------------

TITLE: Using Components with React-Markdown
DESCRIPTION: Example showing how to use custom components with React-Markdown, demonstrating the transition from renderers to components API.
SOURCE: https://github.com/remarkjs/react-markdown/blob/main/changelog.md#2025-04-20_snippet_3

LANGUAGE: javascript
CODE:
```
<Markdown
  components={{
    // Use a fancy hr
    hr: ({node, ...props}) => <MyFancyRule {...props} />
  }}
>{`***`}</Markdown>
```

----------------------------------------

TITLE: Configuring remark-gfm Plugin with Options
DESCRIPTION: This example shows how to pass options to the remark-gfm plugin by using an array structure where the first element is the plugin and the second is the options object. It demonstrates configuring the strikethrough syntax to only allow double tildes.
SOURCE: https://github.com/remarkjs/react-markdown/blob/main/readme.md#2025-04-20_snippet_7

LANGUAGE: javascript
CODE:
```
import React from 'react'
import {createRoot} from 'react-dom/client'
import Markdown from 'react-markdown'
import remarkGfm from 'remark-gfm'

const markdown = 'This ~is not~ strikethrough, but ~~this is~~!'

createRoot(document.body).render(
  <Markdown remarkPlugins={[[remarkGfm, {singleTilde: false}]]}>
    {markdown}
  </Markdown>
)
```

----------------------------------------

TITLE: Using Rehype Plugins with React-Markdown
DESCRIPTION: Demonstrates how to use rehype plugins for syntax highlighting in React-Markdown.
SOURCE: https://github.com/remarkjs/react-markdown/blob/main/changelog.md#2025-04-20_snippet_4

LANGUAGE: javascript
CODE:
```
import rehypeHighlight from 'rehype-highlight'

<Markdown rehypePlugins={[rehypeHighlight]}>{`~~~js
console.log(1)
~~~`}</Markdown>
```

----------------------------------------

TITLE: HTML Support in React-Markdown
DESCRIPTION: Shows how to handle HTML content in markdown using rehype-raw and rehype-sanitize plugins for safe HTML processing.
SOURCE: https://github.com/remarkjs/react-markdown/blob/main/changelog.md#2025-04-20_snippet_5

LANGUAGE: javascript
CODE:
```
import Markdown from 'react-markdown'
import rehypeRaw from 'rehype-raw'
import rehypeSanitize from 'rehype-sanitize'

<Markdown rehypePlugins={[rehypeRaw, rehypeSanitize]}>{`# Hello, <i>world</i>!`}</Markdown>
```

----------------------------------------

TITLE: JSX Equivalent of remark-gfm Rendering
DESCRIPTION: This snippet shows the equivalent JSX output when using remark-gfm plugin with React Markdown. It demonstrates how the markdown is transformed into React components, including emphasis, blockquotes, task lists, and table structures.
SOURCE: https://github.com/remarkjs/react-markdown/blob/main/readme.md#2025-04-20_snippet_6

LANGUAGE: javascript
CODE:
```
<>
  <p>
    A paragraph with <em>emphasis</em> and <strong>strong importance</strong>.
  </p>
  <blockquote>
    <p>
      A block quote with <del>strikethrough</del> and a URL:{' '}
      <a href="https://reactjs.org">https://reactjs.org</a>.
    </p>
  </blockquote>
  <ul className="contains-task-list">
    <li>Lists</li>
    <li className="task-list-item">
      <input type="checkbox" disabled /> todo
    </li>
    <li className="task-list-item">
      <input type="checkbox" disabled checked /> done
    </li>
  </ul>
  <p>A table:</p>
  <table>
    <thead>
      <tr>
        <th>a</th>
        <th>b</th>
      </tr>
    </thead>
  </table>
</>
```

----------------------------------------

TITLE: Installing react-markdown with npm
DESCRIPTION: Command to install the react-markdown package using npm in a Node.js environment (version 16+).
SOURCE: https://github.com/remarkjs/react-markdown/blob/main/readme.md#2025-04-20_snippet_0

LANGUAGE: sh
CODE:
```
npm install react-markdown
```

----------------------------------------

TITLE: Template Literal Indentation Issue
DESCRIPTION: Example showing how indentation inside template literals can cause unexpected markdown rendering. Indentation is preserved, causing text to be rendered as a code block rather than a heading.
SOURCE: https://github.com/remarkjs/react-markdown/blob/main/readme.md#2025-04-20_snippet_19

LANGUAGE: javascript
CODE:
```
<Markdown>{`
    # This is **not** a heading, it's an indented code block
`}</Markdown>
```

----------------------------------------

TITLE: Using Template Literals with React Markdown
DESCRIPTION: Example showing how to use template literals to pass markdown to the Markdown component. This approach preserves line endings but requires careful handling of indentation.
SOURCE: https://github.com/remarkjs/react-markdown/blob/main/readme.md#2025-04-20_snippet_18

LANGUAGE: javascript
CODE:
```
<Markdown>{`
# Hi

This is a paragraph.
`}</Markdown>
```

----------------------------------------

TITLE: Incorrect Markdown Usage in JSX
DESCRIPTION: Example of a common mistake when writing markdown directly in JSX. This approach doesn't work because JSX collapses whitespace and line endings.
SOURCE: https://github.com/remarkjs/react-markdown/blob/main/readme.md#2025-04-20_snippet_17

LANGUAGE: javascript
CODE:
```
<Markdown>
  # Hi

  This is **not** a paragraph.
</Markdown>
```

----------------------------------------

TITLE: Migrating disallowedTypes to disallowedElements in React-Markdown
DESCRIPTION: Example showing how to update the disallowedTypes property to the new disallowedElements property for filtering image elements.
SOURCE: https://github.com/remarkjs/react-markdown/blob/main/changelog.md#2025-04-20_snippet_7

LANGUAGE: javascript
CODE:
```
<Markdown
  // Skip images
  disallowedTypes={['image']}
>{`![alt text](./image.url)`}</Markdown>
```

LANGUAGE: javascript
CODE:
```
<Markdown
  // Skip images
  disallowedElements={['img']}
>{`![alt text](./image.url)`}</Markdown>
```

----------------------------------------

TITLE: Migrating allowNode to allowElement in React-Markdown
DESCRIPTION: Example showing how to update the allowNode property to the new allowElement property for filtering heading elements.
SOURCE: https://github.com/remarkjs/react-markdown/blob/main/changelog.md#2025-04-20_snippet_8

LANGUAGE: javascript
CODE:
```
<Markdown
  // Skip h1
  allowNode={(node) => node.type !== 'heading' || node.depth !== 1}
>{`# main heading`}</Markdown>
```

LANGUAGE: javascript
CODE:
```
<Markdown
  // Skip h1
  allowElement={(element) => element.tagName !== 'h1'}
>{`# main heading`}</Markdown>
```