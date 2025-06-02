TITLE: Initializing CodeMirror Minimal Editor JavaScript
DESCRIPTION: Demonstrates the minimal setup for a CodeMirror editor by importing core packages (`@codemirror/state`, `@codemirror/view`, `@codemirror/commands`), creating an initial `EditorState` with a document and basic keymap, and then instantiating an `EditorView` to display the state within the document body. Requires the specified CodeMirror packages.
SOURCE: https://github.com/codemirror/website/blob/main/site/docs/guide/index.md#_snippet_0

LANGUAGE: javascript
CODE:
```
import {EditorState} from "@codemirror/state"
import {EditorView, keymap} from "@codemirror/view"
import {defaultKeymap} from "@codemirror/commands"

let startState = EditorState.create({
  doc: "Hello World",
  extensions: [keymap.of(defaultKeymap)]
})

let view = new EditorView({
  state: startState,
  parent: document.body
})
```

----------------------------------------

TITLE: Adding Javascript Language Support to CodeMirror Editor - Javascript
DESCRIPTION: This snippet shows how to add language-specific functionality, such as syntax highlighting and smart indentation, by including a language package extension in the `extensions` array. It demonstrates adding Javascript support using `@codemirror/lang-javascript` and configuring it for TypeScript syntax. It requires imports from `@codemirror/lang-javascript` and potentially other core CodeMirror packages.
SOURCE: https://github.com/codemirror/website/blob/main/site/examples/basic/index.md#_snippet_2

LANGUAGE: javascript
CODE:
```
import {javascript} from "@codemirror/lang-javascript"
import {EditorView, basicSetup} from "codemirror"

const view = new EditorView({
  doc: "Start document",
  parent: document.body,
  extensions: [
    basicSetup,
    javascript({typescript: true})
  ]
})
```

----------------------------------------

TITLE: Initializing Basic CodeMirror Editor - Javascript
DESCRIPTION: This snippet shows how to create a minimal CodeMirror editor instance using the `EditorView` class and the `basicSetup` bundle. It sets the initial document content and specifies a parent DOM element for the editor. Required imports are from `codemirror` and `@codemirror/view`.
SOURCE: https://github.com/codemirror/website/blob/main/site/examples/basic/index.md#_snippet_0

LANGUAGE: javascript
CODE:
```
import {basicSetup} from "codemirror"
import {EditorView} from "@codemirror/view"

const view = new EditorView({
  doc: "Start document",
  parent: document.body,
  extensions: [basicSetup]
})
```

----------------------------------------

TITLE: Updating CodeMirror State Dispatching Transaction JavaScript
DESCRIPTION: Explains the correct pattern for modifying the editor state. It demonstrates creating a `Transaction` object from the current state using `state.update()` with desired changes (e.g., `{changes: {from: 0, insert: "0"}}`). This transaction is then dispatched to the `EditorView` using `view.dispatch()`, which updates the view to reflect the new state contained within the transaction.
SOURCE: https://github.com/codemirror/website/blob/main/site/docs/guide/index.md#_snippet_3

LANGUAGE: javascript
CODE:
```
// (Assume view is an EditorView instance holding the document "123".)
let transaction = view.state.update({changes: {from: 0, insert: "0"}})
console.log(transaction.state.doc.toString()) // "0123"
// At this point the view still shows the old state.
view.dispatch(transaction)
// And now it shows the new state.
```

----------------------------------------

TITLE: Initializing CodeMirror Editor with Detailed Extensions - Javascript
DESCRIPTION: This example demonstrates creating an editor by explicitly listing individual standard extensions rather than using the `basicSetup` bundle. This allows for precise control over features like history, selection drawing, special character highlighting, syntax highlighting, bracket matching, folding, autocompletion, and various keymaps. It requires imports from numerous `@codemirror/*` packages.
SOURCE: https://github.com/codemirror/website/blob/main/site/examples/basic/index.md#_snippet_1

LANGUAGE: javascript
CODE:
```
import {Extension, EditorState} from "@codemirror/state"
import {
  EditorView, keymap, highlightSpecialChars, drawSelection,
  highlightActiveLine, dropCursor, rectangularSelection,
  crosshairCursor, lineNumbers, highlightActiveLineGutter
} from "@codemirror/view"
import {
  defaultHighlightStyle, syntaxHighlighting, indentOnInput,
  bracketMatching, foldGutter, foldKeymap
} from "@codemirror/language"
import {
  defaultKeymap, history, historyKeymap
} from "@codemirror/commands"
import {
  searchKeymap, highlightSelectionMatches
} from "@codemirror/search"
import {
  autocompletion, completionKeymap, closeBrackets,
  closeBracketsKeymap
} from "@codemirror/autocomplete"
import {lintKeymap} from "@codemirror/lint"

const view = new EditorView({
  doc: "Start document",
  parent: document.body,
  extensions: [
    // A line number gutter
    lineNumbers(),
    // A gutter with code folding markers
    foldGutter(),
    // Replace non-printable characters with placeholders
    highlightSpecialChars(),
    // The undo history
    history(),
    // Replace native cursor/selection with our own
    drawSelection(),
    // Show a drop cursor when dragging over the editor
    dropCursor(),
    // Allow multiple cursors/selections
    EditorState.allowMultipleSelections.of(true),
    // Re-indent lines when typing specific input
    indentOnInput(),
    // Highlight syntax with a default style
    syntaxHighlighting(defaultHighlightStyle),
    // Highlight matching brackets near cursor
    bracketMatching(),
    // Automatically close brackets
    closeBrackets(),
    // Load the autocompletion system
    autocompletion(),
    // Allow alt-drag to select rectangular regions
    rectangularSelection(),
    // Change the cursor to a crosshair when holding alt
    crosshairCursor(),
    // Style the current line specially
    highlightActiveLine(),
    // Style the gutter for current line specially
    highlightActiveLineGutter(),
    // Highlight text that matches the selected text
    highlightSelectionMatches(),
    keymap.of([
      // Closed-brackets aware backspace
      ...closeBracketsKeymap,
      // A large set of basic bindings
      ...defaultKeymap,
      // Search-related keys
      ...searchKeymap,
      // Redo/undo keys
      ...historyKeymap,
      // Code folding bindings
      ...foldKeymap,
      // Autocompletion keys
      ...completionKeymap,
      // Keys related to the linter system
      ...lintKeymap
    ])
  ]
})
```

----------------------------------------

TITLE: Initializing CodeMirror Basic Setup JavaScript
DESCRIPTION: Provides a simpler way to set up a CodeMirror editor using the `codemirror` package and its `basicSetup` utility, which bundles common extensions. It also demonstrates adding a language-specific extension (`@codemirror/lang-javascript`) for syntax highlighting and related features. Requires the `codemirror` and language packages.
SOURCE: https://github.com/codemirror/website/blob/main/site/docs/guide/index.md#_snippet_1

LANGUAGE: javascript
CODE:
```
import {EditorView, basicSetup} from "codemirror"
import {javascript} from "@codemirror/lang-javascript"

let view = new EditorView({
  extensions: [basicSetup, javascript()],
  parent: document.body
})
```

----------------------------------------

TITLE: Creating Basic CodeMirror 6 EditorView (JavaScript)
DESCRIPTION: Demonstrates the minimal code required to instantiate a CodeMirror 6 `EditorView`. It imports `EditorView` from `@codemirror/view` and creates a new instance, attaching it to `document.body` using the `parent` configuration option.
SOURCE: https://github.com/codemirror/website/blob/main/site/docs/migration/index.md#_snippet_0

LANGUAGE: javascript
CODE:
```
import {EditorView} from "@codemirror/view"
let view = new EditorView({parent: document.body})
```

----------------------------------------

TITLE: Initializing CodeMirror Editor with JavaScript
DESCRIPTION: Imports necessary modules from CodeMirror packages and creates a new EditorView instance. This view is configured with the basicSetup extensions and the JavaScript language support, and is attached to the document body.
SOURCE: https://github.com/codemirror/website/blob/main/site/examples/bundle/index.md#_snippet_0

LANGUAGE: javascript
CODE:
```
import {EditorView, basicSetup} from "codemirror"
import {javascript} from "@codemirror/lang-javascript"

let editor = new EditorView({
  extensions: [basicSetup, javascript()],
  parent: document.body
})
```

----------------------------------------

TITLE: Updating Compartment Configuration (JavaScript)
DESCRIPTION: Defines a JavaScript function setTabSize that takes an editor view and a size. It dispatches a transaction to the view, using Compartment.reconfigure on the tabSize compartment to change the editor's configured tab size dynamically.
SOURCE: https://github.com/codemirror/website/blob/main/site/examples/config/index.md#_snippet_1

LANGUAGE: JavaScript
CODE:
```
function setTabSize(view, size) {
  view.dispatch({
    effects: tabSize.reconfigure(EditorState.tabSize.of(size))
  })
}
```

----------------------------------------

TITLE: Initializing CodeMirror State with Compartments (JavaScript)
DESCRIPTION: Creates an initial CodeMirror editor state using EditorState.create. It includes basicSetup, a language configuration (Python), and a tab size configuration, each wrapped in a Compartment to allow for dynamic reconfiguration later.
SOURCE: https://github.com/codemirror/website/blob/main/site/examples/config/index.md#_snippet_0

LANGUAGE: JavaScript
CODE:
```
import {basicSetup, EditorView} from "codemirror"
import {EditorState, Compartment} from "@codemirror/state"
import {python} from "@codemirror/lang-python"

let language = new Compartment, tabSize = new Compartment

let state = EditorState.create({
  extensions: [
    basicSetup,
    language.of(python()),
    tabSize.of(EditorState.tabSize.of(8))
  ]
})

let view = new EditorView({
  state,
  parent: document.body
})
```

----------------------------------------

TITLE: Simulating fromTextArea Functionality in CodeMirror 6 JavaScript
DESCRIPTION: Provides a JavaScript function that manually replicates the behavior of the deprecated `CodeMirror.fromTextArea` function from version 5. It creates a new CodeMirror 6 `EditorView`, inserts its DOM element before the target textarea, hides the original textarea, and adds an event listener to the textarea's form to sync the editor's content back to the textarea's value upon form submission.
SOURCE: https://github.com/codemirror/website/blob/main/site/docs/migration/index.md#_snippet_10

LANGUAGE: javascript
CODE:
```
function editorFromTextArea(textarea, extensions) {
  let view = new EditorView({doc: textarea.value, extensions})
  textarea.parentNode.insertBefore(view.dom, textarea)
  textarea.style.display = "none"
  if (textarea.form) textarea.form.addEventListener("submit", () => {
    textarea.value = view.state.doc.toString()
  })
  return view
}
```

----------------------------------------

TITLE: Dynamically Configuring Extensions with Compartment in JavaScript
DESCRIPTION: Demonstrates how to use a CodeMirror `Compartment` to manage a part of the editor's configuration that needs to be changed dynamically after initialization. It shows creating a compartment, including an extension wrapped in `compartment.of()` initially, and then dispatching a transaction with `compartment.reconfigure()` to update that extension, specifically changing the tab size.
SOURCE: https://github.com/codemirror/website/blob/main/site/docs/migration/index.md#_snippet_9

LANGUAGE: javascript
CODE:
```
let tabSize = new Compartment

let view = new EditorView({
  extensions: [
    // ...
    tabSize.of(EditorState.tabSize.of(2))
  ],
  // ...
})

function setTabSize(size) {
  view.dispatch({
    effects: tabSize.reconfigure(EditorState.tabSize.of(size))
  })
}
```

----------------------------------------

TITLE: Toggling Extension with Private Compartment (TypeScript)
DESCRIPTION: A TypeScript function toggleWith returns an extension that binds a key command. The command uses a private Compartment to dynamically enable or disable the provided extension by reconfiguring its content between [] and the extension itself.
SOURCE: https://github.com/codemirror/website/blob/main/site/examples/config/index.md#_snippet_2

LANGUAGE: TypeScript
CODE:
```
import {Extension, Compartment} from "@codemirror/state"
import {keymap, EditorView} from "@codemirror/view"

export function toggleWith(key: string, extension: Extension) {
  let myCompartment = new Compartment
  function toggle(view: EditorView) {
    let on = myCompartment.get(view.state) == extension
    view.dispatch({
      effects: myCompartment.reconfigure(on ? [] : extension)
    })
    return true
  }
  return [
    myCompartment.of([]),
    keymap.of([{key, run: toggle}])
  ]
}
```

----------------------------------------

TITLE: Creating Breakpoint State Field CodeMirror (TS)
DESCRIPTION: This snippet defines a `StateField` extension to manage breakpoint markers. The field holds a `RangeSet` of breakpoint positions, mapping existing breakpoints through document changes during transactions and applying effects (`toggleBreakpoint`) that explicitly add or remove breakpoints based on their position.
SOURCE: https://github.com/codemirror/website/blob/main/site/examples/gutter/index.md#_snippet_2

LANGUAGE: TypeScript
CODE:
```
import {StateField, StateEffect, RangeSet} from "@codemirror/state";

export const toggleBreakpoint = StateEffect.define<{pos: number}>({
  map(val, changes) {
    return {pos: changes.mapPos(val.pos, 1)}; // Map position, assoc 1 for inclusion
  }
});

const breakpointState = StateField.define<RangeSet<null>>({
  create() {
    return RangeSet.empty;
  },
  update(set, tr) {
    set = set.map(tr.changes);
    for (const effect of tr.effects) {
      if (effect.is(toggleBreakpoint)) {
        const {pos} = effect.value;
        // Simple toggle logic: add if not there, remove if there
        let found = false;
        set.between(pos, pos, () => { found = true; });
        if (found) {
          set = set.update({filter: (from, to) => from !== pos || to !== pos}).ranges;
        } else {
          set = set.update({add: [{from: pos, to: pos}]}).ranges;
        }
      }
    }
    return set;
  }
});
```

----------------------------------------

TITLE: Configuring CodeMirror Extension Precedence JavaScript
DESCRIPTION: Illustrates the impact of extension precedence on how extensions are applied. It defines multiple keymap extensions with the same key binding (`Ctrl-Space`) and shows how `Prec.high` is used to give a specific keymap higher priority, ensuring it's executed before others, regardless of its position in the extensions array. Requires `@codemirror/view`, `@codemirror/state`.
SOURCE: https://github.com/codemirror/website/blob/main/site/docs/guide/index.md#_snippet_4

LANGUAGE: javascript
CODE:
```
import {keymap} from "@codemirror/view"
import {EditorState, Prec} from "@codemirror/state"

function dummyKeymap(tag) {
  return keymap.of([{
    key: "Ctrl-Space",
    run() { console.log(tag); return true }
  }])
}

let state = EditorState.create({extensions: [
  dummyKeymap("A"),
  dummyKeymap("B"),
  Prec.high(dummyKeymap("C"))
]})
```

----------------------------------------

TITLE: Enabling CodeMirror Syntax Highlighting Extension JavaScript
DESCRIPTION: Demonstrates how to integrate a defined `HighlightStyle` (like `myHighlightStyle`) into the CodeMirror editor configuration. It shows wrapping the style in the `syntaxHighlighting` function to create an editor extension that enables syntax highlighting.
SOURCE: https://github.com/codemirror/website/blob/main/site/examples/styling/index.md#_snippet_5

LANGUAGE: javascript
CODE:
```
import {syntaxHighlighting} from "@codemirror/language"

// In your extensions...
syntaxHighlighting(myHighlightStyle)
```

----------------------------------------

TITLE: Inserting Text with CodeMirror Transaction (JavaScript)
DESCRIPTION: This snippet demonstrates inserting text at a specific position (the start) in the CodeMirror document. It dispatches a transaction containing a `changes` object that specifies the insertion point using `from` and the text to `insert`. This requires an initialized CodeMirror `EditorView` instance.
SOURCE: https://github.com/codemirror/website/blob/main/site/examples/change/index.md#_snippet_0

LANGUAGE: javascript
CODE:
```
view.dispatch({
  changes: {from: 0, insert: "#!/usr/bin/env node\n"}
})
```

----------------------------------------

TITLE: Defining Custom Facet with Computed Values - CodeMirror Javascript
DESCRIPTION: This snippet demonstrates defining a custom facet using `Facet.define`. It then shows how to provide values to this facet via extensions, including a static value (`"hello"`) and a value dynamically computed from the state's document (`state.doc.lines`). The `compute` method is used for dependencies that trigger recomputation. Requires importing `Facet` and `EditorState` from `@codemirror/state`.
SOURCE: https://github.com/codemirror/website/blob/main/site/docs/guide/index.md#_snippet_10

LANGUAGE: javascript
CODE:
```
let info = Facet.define<string>()
let state = EditorState.create({
  doc: "abc\ndef",
  extensions: [
    info.of("hello"),
    info.compute(["doc"], state => `lines: ${state.doc.lines}`)
  ]
})
console.log(state.facet(info))
// ["hello", "lines: 2"]
```

----------------------------------------

TITLE: Defining a CodeMirror Highlight Style JavaScript
DESCRIPTION: Illustrates how to create a `HighlightStyle` using `HighlightStyle.define` to customize syntax highlighting. It demonstrates associating Lezer highlighting tags (`tags.keyword`, `tags.comment`) with specific CSS style properties like color and font style.
SOURCE: https://github.com/codemirror/website/blob/main/site/examples/styling/index.md#_snippet_4

LANGUAGE: javascript
CODE:
```
import {tags} from "@lezer/highlight"
import {HighlightStyle} from "@codemirror/language"

const myHighlightStyle = HighlightStyle.define([
  {tag: tags.keyword, color: "#fc6"},
  {tag: tags.comment, color: "#f5d", fontStyle: "italic"}
])
```

----------------------------------------

TITLE: Implementing CodeMirror View Plugin (JavaScript)
DESCRIPTION: Creates a CodeMirror `ViewPlugin` using `ViewPlugin.fromClass`. The plugin adds a `div` element to the editor's DOM in the constructor, displaying the initial document length. The `update` method checks if the document changed and updates the displayed length if necessary. The `destroy` method removes the added DOM element.
SOURCE: https://github.com/codemirror/website/blob/main/site/docs/guide/index.md#_snippet_18

LANGUAGE: JavaScript
CODE:
```
import {ViewPlugin} from "@codemirror/view"

const docSizePlugin = ViewPlugin.fromClass(class {
  constructor(view) {
    this.dom = view.dom.appendChild(document.createElement("div"))
    this.dom.style.cssText =
      "position: absolute; inset-block-start: 2px; inset-inline-end: 5px"
    this.dom.textContent = view.state.doc.length
  }

  update(update) {
    if (update.docChanged)
      this.dom.textContent = update.state.doc.length
  }

  destroy() { this.dom.remove() }
})
```

----------------------------------------

TITLE: Providing and Reading Facet Values - CodeMirror Javascript
DESCRIPTION: This snippet shows how extensions can contribute values to CodeMirror facets (`tabSize` and `changeFilter`) during state creation using the `.of()` method. It then demonstrates retrieving the resolved facet value from the state using the `state.facet()` method. Requires importing `EditorState` from `@codemirror/state`.
SOURCE: https://github.com/codemirror/website/blob/main/site/docs/guide/index.md#_snippet_9

LANGUAGE: javascript
CODE:
```
import {EditorState} from "@codemirror/state"

let state = EditorState.create({
  extensions: [
    EditorState.tabSize.of(16),
    EditorState.changeFilter.of(() => true)
  ]
})
console.log(state.facet(EditorState.tabSize)) // 16
console.log(state.facet(EditorState.changeFilter)) // [() => true]
```

----------------------------------------

TITLE: Removing Decorations via StateEffect using CodeMirror 6 JavaScript Extension
DESCRIPTION: Provides a JavaScript function that removes decorations within a specified range using the `StateField` extension for decorations. It dispatches a transaction with a `StateEffect` (`filterMarks`) containing a predicate function. This predicate function is applied by the custom `markField`'s update logic to filter the existing decorations, removing those that fall within the given range.
SOURCE: https://github.com/codemirror/website/blob/main/site/docs/migration/index.md#_snippet_13

LANGUAGE: javascript
CODE:
```
function removeMarks(a, b) {
  view.dispatch({
    effects: filterMarks.of((from, to) => to <= a || from >= b)
  })
}
```

----------------------------------------

TITLE: Demonstrating Incorrect Immutable State Modification JavaScript
DESCRIPTION: This snippet shows a common mistake when working with CodeMirror's immutable state: directly assigning a new value to a property like `state.doc`. This action will not update the editor state or view because the `EditorState` object is treated as immutable. Modifications must be done through transactions.
SOURCE: https://github.com/codemirror/website/blob/main/site/docs/guide/index.md#_snippet_2

LANGUAGE: javascript
CODE:
```
let state = EditorState.create({doc: "123"})
// BAD WRONG NO GOOD CODE:
state.doc = Text.of("abc") // <- DON'T DO THIS
```

----------------------------------------

TITLE: Managing State Field with Effects (JavaScript)
DESCRIPTION: Demonstrates how to manage a CodeMirror `StateField` using `StateEffect`. It defines a `setFullScreenMode` effect and a `fullScreenMode` field. The field's `update` function iterates through transaction effects, updating the field's value if a `setFullScreenMode` effect is encountered.
SOURCE: https://github.com/codemirror/website/blob/main/site/docs/guide/index.md#_snippet_17

LANGUAGE: JavaScript
CODE:
```
import {StateField, StateEffect} from "@codemirror/state"

let setFullScreenMode = StateEffect.define<boolean>()

let fullScreenMode = StateField.define({
  create() { return false },
  update(value, tr) {
    for (let e of tr.effects)
      if (e.is(setFullScreenMode)) value = e.value
    return value
  }
})
```

----------------------------------------

TITLE: Modifying CodeMirror 6 Selection via Transactions (CM5 vs CM6) (JavaScript)
DESCRIPTION: Maps CodeMirror 5 methods for changing the selection (`setCursor`, `setSelection`, `setSelections`, `extendSelectionsBy`) to CodeMirror 6's transaction-based approach. It shows how to update the `selection` property in the dispatched transaction spec using simple objects or `EditorSelection.create`.
SOURCE: https://github.com/codemirror/website/blob/main/site/docs/migration/index.md#_snippet_7

LANGUAGE: javascript
CODE:
```
cm.setCursor(pos) → cm.dispatch({selection: {anchor: pos}})

cm.setSelection(anchor, head) → cm.dispatch({selection: {anchor, head}})

cm.setSelections(ranges) → cm.dispatch({
  selection: EditorSelection.create(ranges)
})

cm.extendSelectionsBy(f) ⤳ cm.dispatch({
  selection: EditorSelection.create(
    cm.state.selection.ranges.map(r => r.extend(f(r))))
})
```

----------------------------------------

TITLE: Modifying CodeMirror 6 Document Content via Transactions (CM5 vs CM6) (JavaScript)
DESCRIPTION: Shows how to perform document modifications in CodeMirror 6, contrasting the direct method calls of CM5 (`replaceRange`, `setValue`, `replaceSelection`) with CM6's transaction-based system using `cm.dispatch`. It demonstrates creating change objects for the `changes` property in the transaction spec.
SOURCE: https://github.com/codemirror/website/blob/main/site/docs/migration/index.md#_snippet_6

LANGUAGE: javascript
CODE:
```
cm.replaceRange(text, from, to) → cm.dispatch({
  changes: {from, to, insert: text}
})

cm.setValue(text) → cm.dispatch({
  changes: {from: 0, to: cm.state.doc.length, insert: text}
})
// Or, if the entire state (undo history, etc) should be reset
cm.setState(EditorState.create({doc: text, extensions: ...}))

cm.replaceSelection(text) → cm.dispatch(cm.state.replaceSelection(text))
```

----------------------------------------

TITLE: Accessing CodeMirror 6 Document Content (CM5 vs CM6) (JavaScript)
DESCRIPTION: Maps common CodeMirror 5 methods for accessing document content (`getValue`, `getRange`, `getLine`, `lineCount`) to their corresponding CodeMirror 6 equivalents, typically accessed through the `state.doc` or `state` object.
SOURCE: https://github.com/codemirror/website/blob/main/site/docs/migration/index.md#_snippet_4

LANGUAGE: javascript
CODE:
```
cm.getValue() → cm.state.doc.toString()

cm.getRange(a, b) → cm.state.sliceDoc(a, b)

cm.getLine(n) → cm.state.doc.line(n + 1).text

cm.lineCount() → cm.state.doc.lines
```

----------------------------------------

TITLE: Adding Decorations via StateEffect using CodeMirror 6 JavaScript Extension
DESCRIPTION: Shows how to add specific decorations to the editor state using the `StateField` extension defined previously. It demonstrates creating a `Decoration.mark` instance and dispatching a transaction with a `StateEffect` (`addMarks`) containing the decoration's range, which is then processed by the custom `markField`'s update function to add the decoration.
SOURCE: https://github.com/codemirror/website/blob/main/site/docs/migration/index.md#_snippet_12

LANGUAGE: javascript
CODE:
```
const strikeMark = Decoration.mark({
  attributes: {style: "text-decoration: line-through"}
})
view.dispatch({
  effects: addMarks.of([strikeMark.range(1, 4)])
})
```

----------------------------------------

TITLE: Basic Override Completion Source (JS/TS)
DESCRIPTION: Provides a simple completion source function that matches words before the cursor using a regex (`matchBefore`) and returns a static list of options. It checks if completion was explicitly triggered (`context.explicit`) before suggesting completions for an empty match and includes a recommended `validFor` regex.
SOURCE: https://github.com/codemirror/website/blob/main/site/examples/autocompletion/index.md#_snippet_1

LANGUAGE: TypeScript
CODE:
```
import { CompletionContext, CompletionResult, Completion } from "@codemirror/autocomplete";

// A basic completion source function
function myCompletions(context: CompletionContext): CompletionResult | null {
  // Match word characters before the cursor
  let word = context.matchBefore(/\w*$/);

  // If no word is found or it's an empty match and not explicit, return null
  if (!word || (word.from === word.to && !context.explicit)) {
    return null;
  }

  // Return a list of simple options
  return {
    from: word.from,
    options: [
      { label: "hello" },
      { label: "world" },
      { label: "CodeMirror" }
    ],
    // Specify validFor regex to indicate when results are still valid based on input
    validFor: /^\w*$/
  };
}
```

----------------------------------------

TITLE: Accessing CodeMirror 6 DOM Elements (CM5 vs CM6) (JavaScript)
DESCRIPTION: Maps CodeMirror 5 methods for accessing the editor's DOM elements (`focus`, `hasFocus`, `getWrapperElement`, `getScrollerElement`, `getInputField`) to their corresponding properties or methods on the CodeMirror 6 `EditorView` instance.
SOURCE: https://github.com/codemirror/website/blob/main/site/docs/migration/index.md#_snippet_8

LANGUAGE: javascript
CODE:
```
cm.focus() → cm.focus()

cm.hasFocus() → cm.hasFocus

cm.getWrapperElement() → cm.dom

cm.getScrollerElement() → cm.scrollDOM

// This is always a contentEditable element
cm.getInputField() → cm.contentDOM
```

----------------------------------------

TITLE: Defining a CodeMirror Dark Theme JavaScript
DESCRIPTION: Demonstrates how to create a custom dark theme for CodeMirror using `EditorView.theme`. It shows how to style various parts of the editor, including the main container, content area, cursor, selection, and gutters, using CSS-like properties within a JavaScript object.
SOURCE: https://github.com/codemirror/website/blob/main/site/examples/styling/index.md#_snippet_2

LANGUAGE: javascript
CODE:
```
import {EditorView} from "@codemirror/view"

let myTheme = EditorView.theme({
  "&": {
    color: "white",
    backgroundColor: "#034"
  },
  ".cm-content": {
    caretColor: "#0e9"
  },
  "&.cm-focused .cm-cursor": {
    borderLeftColor: "#0e9"
  },
  "&.cm-focused .cm-selectionBackground, ::selection": {
    backgroundColor: "#074"
  }
  ,
  ".cm-gutters": {
    backgroundColor: "#045",
    color: "#ddd",
    border: "none"
  }
}, {dark: true})
```

----------------------------------------

TITLE: Defining Breakpoint CodeMirror Gutter Extension (TS)
DESCRIPTION: This snippet defines the breakpoint gutter extension by combining the breakpoint state field and the gutter configuration. It uses the `markers` option to display markers based on the state field's `RangeSet` and includes `domEventHandlers` to add a click listener to the gutter for toggling breakpoints via a state effect.
SOURCE: https://github.com/codemirror/website/blob/main/site/examples/gutter/index.md#_snippet_3

LANGUAGE: TypeScript
CODE:
```
import {gutter, GutterMarker, EditorView} from "@codemirror/view";
import {StateField, StateEffect, RangeSet} from "@codemirror/state";
import {breakpointState, toggleBreakpoint} from "./breakpointState"; // Assuming import from the state file

class BreakpointMarker extends GutterMarker {
  toDOM() {
    const element = document.createElement("div");
    element.style.width = "10px"; // Example size
    element.style.height = "10px";
    element.style.borderRadius = "50%";
    element.style.backgroundColor = "red";
    element.style.cursor = "pointer";
    return element;
  }
}

const breakpointGutter = [
  breakpointState, // Include the state field
  gutter({
    markers: view => view.state.field(breakpointState, false), // Get markers from state
    domEventHandlers: {
      mousedown(view, line, event) {
        // Find the line head position and dispatch the effect
        const linePos = line.from;
        view.dispatch({ effects: toggleBreakpoint.of({ pos: linePos }) });
        return true; // Handled
      }
    },
    initialSpacer: () => new BreakpointMarker() // Use marker for spacer
  })
];
```

----------------------------------------

TITLE: Initializing a CodeMirror Editor View for Collaboration - TypeScript
DESCRIPTION: Asynchronous function `createPeer` demonstrates how to initialize a CodeMirror editor view for a collaborative session. It first fetches the initial document state and version from the authority using `getDocument`, then creates an `EditorState` with the basic setup and the custom `peerExtension`, and finally creates an `EditorView` with this state.
SOURCE: https://github.com/codemirror/website/blob/main/site/examples/collab/index.md#_snippet_4

LANGUAGE: TypeScript
CODE:
```
async function createPeer(connection: Connection) {
  let {version, doc} = await getDocument(connection)
  let state = EditorState.create({
    doc,
    extensions: [basicSetup, peerExtension(version, connection)]
  })
  return new EditorView({state})
}
```

----------------------------------------

TITLE: Defining CodeMirror Keymap (JavaScript)
DESCRIPTION: Creates a CodeMirror extension using the `keymap.of` facet to define custom key bindings. It binds the "Alt-c" key combination to a command function that dispatches a transaction to replace the current selection with a question mark. The function returns `true` to indicate successful execution.
SOURCE: https://github.com/codemirror/website/blob/main/site/docs/guide/index.md#_snippet_15

LANGUAGE: JavaScript
CODE:
```
let myKeyExtension = keymap.of([
  {
    key: "Alt-c",
    run: view => {
      view.dispatch(view.state.replaceSelection("?"))
      return true
    }
  }
])
```

----------------------------------------

TITLE: Implementing CodeMirror Collab Peer Extension - TypeScript
DESCRIPTION: Creates a CodeMirror view plugin (`peerExtension`) that manages the collaborative state and communication for a peer (client). It continuously polls the server for updates using `pullUpdates` and applies them with `receiveUpdates`. It also monitors local changes to push them to the server using `pushUpdates`. It handles scheduling and retries for pushing updates.
SOURCE: https://github.com/codemirror/website/blob/main/site/examples/collab/index.md#_snippet_3

LANGUAGE: TypeScript
CODE:
```
function peerExtension(startVersion: number, conn: Connection) {
  let plugin = ViewPlugin.fromClass(class implements PluginValue {
    version: number
    pushing = false
    pulling = false

    constructor(view: EditorView) {
      this.version = startVersion
      this.pull() // Start pulling updates
    }

    update(update: ViewUpdate) {
      if (update.docChanged) {
        this.push() // Push changes when the document changes
      }
    }

    async pull() {
      if (this.pulling) return
      this.pulling = true
      while (this.pulling) {
        try {
          let result = await pullUpdates(conn, this.version)
          if (result.updates && result.updates.length) {
            let updates = result.updates.map((u: any) => ({changes: ChangeSet.fromJSON(u.changes), clientID: u.clientID}))
            this.version += updates.length
            this.pushing = true // Prevent concurrent push
            this.view.dispatch(receiveUpdates(this.view.state, updates))
            this.pushing = false
            this.push() // Push again if more changes came in
          } else if (result.error) {
            console.error(result.error)
            this.pulling = false
          }
        } catch(e) {
          console.error(e)
          this.pulling = false
        }
      }
    }

    async push() {
      // Schedule a push whenever there are unconfirmed changes
      if (this.pushing || unconfirmed(this.view.state).length == 0) return
      this.pushing = true
      let updates = unconfirmed(this.view.state).map((u: any) => ({changes: u.changes.toJSON(), clientID: u.clientID}))
      try {
        let result = await pushUpdates(conn, this.version, updates)
        if (result.error) console.error(result.error)
        else if (unconfirmed(this.view.state).length) setTimeout(() => this.push(), 100) // Try again
      } catch(e) {
        console.error(e)
        if (unconfirmed(this.view.state).length) setTimeout(() => this.push(), 100) // Try again
      } finally {
        this.pushing = false
      }
    }

    destroy() {
      this.pulling = false
    }

    get view() { return this._view }
    _view!: EditorView
  })

  return [collab({startVersion}), plugin]
}
```

----------------------------------------

TITLE: Mapping Document Position after Changes - CodeMirror Javascript
DESCRIPTION: This snippet illustrates how to track cursor or range positions across document modifications in CodeMirror. It creates an editor state, applies changes (deleting characters and inserting new ones), and then uses `changes.mapPos` to find where a position from the original document ends up in the modified document. It requires importing `EditorState` from `@codemirror/state`.
SOURCE: https://github.com/codemirror/website/blob/main/site/docs/guide/index.md#_snippet_5

LANGUAGE: javascript
CODE:
```
import {EditorState} from "@codemirror/state"

let state = EditorState.create({doc: "1234"})
// Delete "23" and insert at "0" at the start.
let tr = state.update({changes: [{from: 1, to: 3}, {from: 0, insert: "0"}]})
// The position at the end of the old document is at 3 in the new document.
console.log(tr.changes.mapPos(4))
```

----------------------------------------

TITLE: Generating CodeMirror Zebra Stripe Line Decorations (TypeScript)
DESCRIPTION: A helper function that iterates over the visible lines in the editor view and generates CodeMirror `line` decorations for every Nth line, where N is the provided step size. It uses a `RangeSetBuilder` for efficient decoration creation and applies the `cm-zebraStripe` class attribute.
SOURCE: https://github.com/codemirror/website/blob/main/site/examples/zebra/index.md#_snippet_3

LANGUAGE: TypeScript
CODE:
```
import {Decoration, DecorationSet, EditorView} from "@codemirror/view";
import {RangeSetBuilder} from "@codemirror/state";

const stripeDeco = (view: EditorView, step: number) => {
  const builder = new RangeSetBuilder<Decoration>();
  for (const {from, to} of view.visibleRanges) {
    for (let pos = from; pos <= to;) {
      const line = view.state.doc.lineAt(pos);
      if ((line.number - 1) % step === 0) {
        builder.add(line.from, line.from, Decoration.line({attributes: {class: "cm-zebraStripe"}}));
      }
      pos = line.to + 1;
    }
  }
  return builder.finish();
};
```

----------------------------------------

TITLE: Defining CodeMirror Zebra Stripes View Plugin (TypeScript)
DESCRIPTION: Defines a CodeMirror `ViewPlugin` responsible for managing and updating the zebra stripe decorations. It registers itself to provide decorations and includes an `update` method to recompute decorations whenever the document content, viewport, or the `stepSize` facet value changes, ensuring the stripes stay current and visible.
SOURCE: https://github.com/codemirror/website/blob/main/site/examples/zebra/index.md#_snippet_4

LANGUAGE: TypeScript
CODE:
```
import {ViewPlugin, Decoration, EditorView} from "@codemirror/view";
import {EditorState} from "@codemirror/state";
import {stripeDeco, stepSize} from "./zebra"; // Assuming imports

const showStripes = ViewPlugin.define(view => ({
  decorations: stripeDeco(view, view.state.facet(stepSize)),
  update(update) {
    if (update.docChanged || update.viewportChanged || update.state.facet(stepSize) !== update.startState.facet(stepSize)) {
       this.decorations = stripeDeco(update.view, update.view.state.facet(stepSize));
    }
  }
}), {
  decorations: v => v.decorations
});
```

----------------------------------------

TITLE: Adding JavaScript Syntax Highlighting to CodeMirror 6 (JavaScript)
DESCRIPTION: Illustrates how to enable syntax highlighting for JavaScript in CodeMirror 6. It imports the `javascript` language package and the `syntaxHighlighting` and `defaultHighlightStyle` extensions from `@codemirror/language`, showing examples of how to add them to the editor's `extensions` list.
SOURCE: https://github.com/codemirror/website/blob/main/site/docs/migration/index.md#_snippet_2

LANGUAGE: javascript
CODE:
```
import {syntaxHighlighting, defaultHighlightStyle} from "@codemirror/language"
import {javascript} from "@codemirror/lang-javascript"

  // Add these extensions
  javascript(),
  syntaxHighlighting(defaultHighlightStyle),
```

----------------------------------------

TITLE: Creating CodeMirror Theme (JavaScript)
DESCRIPTION: Defines a CodeMirror theme using `EditorView.theme`. It sets the default text color to 'darkorange' for `.cm-content` and 'orange' when the editor is focused (`&.cm-focused .cm-content`). The `&` symbol is used to target the editor wrapper element where the theme's unique class is added.
SOURCE: https://github.com/codemirror/website/blob/main/site/docs/guide/index.md#_snippet_12

LANGUAGE: JavaScript
CODE:
```
import {EditorView} from "@codemirror/view"

let view = new EditorView({
  extensions: EditorView.theme({
    ".cm-content": {color: "darkorange"},
    "&.cm-focused .cm-content": {color: "orange"}
  })
})
```

----------------------------------------

TITLE: Replacing All Tabs with Spaces in CodeMirror (JavaScript)
DESCRIPTION: This code iterates through the document text to find all tab characters (`\t`) and constructs an array of change specifications. Each change object defines the replacement of a single tab with two spaces. Finally, it dispatches a single transaction containing this array of changes to update the document. Requires a CodeMirror `EditorView` instance.
SOURCE: https://github.com/codemirror/website/blob/main/site/examples/change/index.md#_snippet_1

LANGUAGE: javascript
CODE:
```
let text = view.state.doc.toString(), pos = 0
let changes = []
for (let next; (next = text.indexOf("\t", pos)) > -1;) {
  changes.push({from: next, to: next + 1, insert: "  "})
  pos = next + 1
}
view.dispatch({changes})
```

----------------------------------------

TITLE: Accessing Document Line Information - CodeMirror Javascript
DESCRIPTION: This example demonstrates how to query a CodeMirror `Text` document for details about specific lines. It uses `doc.line(lineNumber)` to get information by a 1-based line number and `doc.lineAt(offset)` to find the line containing a given character offset. Requires importing the `Text` class from `@codemirror/state`.
SOURCE: https://github.com/codemirror/website/blob/main/site/docs/guide/index.md#_snippet_6

LANGUAGE: javascript
CODE:
```
import {Text} from "@codemirror/state"

let doc = Text.of(["line 1", "line 2", "line 3"])
// Get information about line 2
console.log(doc.line(2)) // {from: 7, to: 13, ...}
// Get the line around position 15
console.log(doc.lineAt(15)) // {from: 14, to: 20, ...}
```

----------------------------------------

TITLE: Applying Transformation per Selection Range - CodeMirror Javascript
DESCRIPTION: This example uses the `state.changeByRange` method to apply a specific operation (uppercasing the selected text) to each selection range in the state. It defines a function that takes a range, extracts its content, transforms it, creates a change object for the transformation, and defines the resulting range. Requires importing `EditorState` and `EditorSelection` from `@codemirror/state`.
SOURCE: https://github.com/codemirror/website/blob/main/site/docs/guide/index.md#_snippet_8

LANGUAGE: javascript
CODE:
```
import {EditorState, EditorSelection} from "@codemirror/state"

let state = EditorState.create({doc: "abcd", selection: {anchor: 1, head: 3}})
// Upcase the selection
let tr = state.update(state.changeByRange(range => {
  let upper = state.sliceDoc(range.from, range.to).toUpperCase()
  return {
    changes: {from: range.from, to: range.to, insert: upper},
    range: EditorSelection.range(range.from, range.from + upper.length)
  }
}))
console.log(tr.state.doc.toString()) // "aBCd"
```

----------------------------------------

TITLE: Replacing CodeMirror Selection with String (JavaScript)
DESCRIPTION: This snippet uses the `replaceSelection` method on the editor's state to replace each currently selected range in the CodeMirror document with the specified string ("★"). This helper method automatically handles updating the cursor/selection position to the end of the inserted text. Requires a CodeMirror `EditorView` instance.
SOURCE: https://github.com/codemirror/website/blob/main/site/examples/change/index.md#_snippet_2

LANGUAGE: javascript
CODE:
```
view.dispatch(view.state.replaceSelection("★"))
```

----------------------------------------

TITLE: Configuring CodeMirror 6 with History and Keymaps (JavaScript)
DESCRIPTION: Shows how to add history and default key bindings to a CodeMirror 6 `EditorView` by providing an `extensions` array in the configuration. It imports necessary modules from `@codemirror/view` and `@codemirror/commands` and includes `history()` and `keymap.of(...)` extensions.
SOURCE: https://github.com/codemirror/website/blob/main/site/docs/migration/index.md#_snippet_1

LANGUAGE: javascript
CODE:
```
import {keymap, EditorView} from "@codemirror/view"
import {defaultKeymap, history, historyKeymap} from "@codemirror/commands"

let view = new EditorView({
  extensions: [
    history(),
    keymap.of([...defaultKeymap, ...historyKeymap]),
  ],
  parent: document.body
})
```

----------------------------------------

TITLE: Defining Empty Line CodeMirror Gutter (TS)
DESCRIPTION: This snippet demonstrates how to create a custom gutter that highlights empty lines. It defines an anonymous `GutterMarker` subclass responsible for rendering the marker element ('ø') and configures the gutter using the `lineMarker` option to check if a line is empty and return an instance of the marker for such lines. It also includes an initial spacer.
SOURCE: https://github.com/codemirror/website/blob/main/site/examples/gutter/index.md#_snippet_1

LANGUAGE: TypeScript
CODE:
```
import {gutter, GutterMarker} from "@codemirror/view";

const emptyLineGutter = gutter({
  lineMarker: (view, line) => {
    return line.length == 0 ? new class extends GutterMarker {
      toDOM() {
        const element = document.createElement("span");
        element.textContent = "ø";
        element.style.opacity = "0.5"; // Optional styling
        return element;
      }
    } : null;
  },
  initialSpacer: () => new class extends GutterMarker { // Anonymous marker for spacer
    toDOM() {
      const element = document.createElement("span");
      element.textContent = "ø"; // Or just a dummy element with width
      element.style.visibility = "hidden"; // Make it invisible
      return element;
    }
  }
});
```

----------------------------------------

TITLE: Changing Selection: After Document Change (CodeMirror, JavaScript)
DESCRIPTION: This snippet dispatches a transaction that includes both a document change (`changes: {from: 10, insert: "*"}`) and a selection change (`selection: {anchor: 11}`). It inserts an asterisk at position 10 and then places the cursor at position 11, which is immediately after the inserted character in the modified document.
SOURCE: https://github.com/codemirror/website/blob/main/site/examples/selection/index.md#_snippet_2

LANGUAGE: javascript
CODE:
```
view.dispatch({
  changes: {from: 10, insert: "*"},
  selection: {anchor: 11}
})
```

----------------------------------------

TITLE: Defining StateField Extension for Managing Decorations in CodeMirror 6 JavaScript
DESCRIPTION: Illustrates how to create a custom CodeMirror 6 `StateField` extension to manage a set of editor decorations. It defines the field's initial state as empty decorations, an `update` function that applies document changes and handles custom effects (`addMarks`, `filterMarks`) to modify the decoration set, and specifies that this field provides decorations to the `EditorView`.
SOURCE: https://github.com/codemirror/website/blob/main/site/docs/migration/index.md#_snippet_11

LANGUAGE: javascript
CODE:
```
import {StateField, StateEffect} from "@codemirror/state"
import {EditorView, Decoration} from "@codemirror/view"

// Effects can be attached to transactions to communicate with the extension
const addMarks = StateEffect.define(), filterMarks = StateEffect.define()

// This value must be added to the set of extensions to enable this
const markField = StateField.define({
  // Start with an empty set of decorations
  create() { return Decoration.none },
  // This is called whenever the editor updates—it computes the new set
  update(value, tr) {
    // Move the decorations to account for document changes
    value = value.map(tr.changes)
    // If this transaction adds or removes decorations, apply those changes
    for (let effect of tr.effects) {
      if (effect.is(addMarks)) value = value.update({add: effect.value, sort: true})
      else if (effect.is(filterMarks)) value = value.update({filter: effect.value})
    }
    return value
  },
  // Indicate that this field provides a set of decorations
  provide: f => EditorView.decorations.from(f)
})
```

----------------------------------------

TITLE: Using Multiple Selections and Replacing Them - CodeMirror Javascript
DESCRIPTION: This snippet demonstrates setting up an editor state with multiple selection ranges (a range and a cursor) by providing them during state creation and enabling multiple selections via an extension. It then shows how to use `state.replaceSelection` to replace the content of all selected ranges with a single string. Requires importing `EditorState` and `EditorSelection` from `@codemirror/state`.
SOURCE: https://github.com/codemirror/website/blob/main/site/docs/guide/index.md#_snippet_7

LANGUAGE: javascript
CODE:
```
import {EditorState, EditorSelection} from "@codemirror/state"

let state = EditorState.create({
  doc: "hello",
  selection: EditorSelection.create([
    EditorSelection.range(0, 4),
    EditorSelection.cursor(5)
  ]),
  extensions: EditorState.allowMultipleSelections.of(true)
})
console.log(state.selection.ranges.length) // 2

let tr = state.update(state.replaceSelection("!"))
console.log(tr.state.doc.toString()) // "!o!"
```

----------------------------------------

TITLE: Modifying CodeMirror Ranges with changeByRange (JavaScript)
DESCRIPTION: This example demonstrates the use of the `changeByRange` helper, which is useful for applying specific modifications to each selection range independently. It takes a callback function that receives a range and returns the changes to apply to that range, along with the range's new selection position. Here, it wraps each selected range in underscores. Requires a CodeMirror `EditorView` instance and `EditorSelection`.
SOURCE: https://github.com/codemirror/website/blob/main/site/examples/change/index.md#_snippet_3

LANGUAGE: javascript
CODE:
```
view.dispatch(view.state.changeByRange(range => ({
  changes: [{from: range.from, insert: "_"}, {from: range.to, insert: "_"}],
  range: EditorSelection.range(range.from, range.to + 2)
})))
```

----------------------------------------

TITLE: Syntax-Aware JSDoc Completion Source (JS/TS)
DESCRIPTION: Implements a completion source that inspects the syntax tree to provide JSDoc tag suggestions (`@param`, `@returns`, etc.) specifically within JavaScript block comments (`/** ... */`). It adjusts the completion range (`from`) based on the typed input after an '@' or at a valid position and includes a `validFor` regex.
SOURCE: https://github.com/codemirror/website/blob/main/site/examples/autocompletion/index.md#_snippet_2

LANGUAGE: TypeScript
CODE:
```
import { CompletionContext, CompletionResult, Completion } from "@codemirror/autocomplete";
import { syntaxTree } from "@codemirror/language";

// Define JSDoc tag completions
const jsDocTags: Completion[] = [
  { label: "param", type: "keyword", detail: "{type} name description" },
  { label: "returns", type: "keyword", detail: "{type} description" },
  { label: "typedef", type: "keyword", detail: "{type} name" },
  { label: "template", type: "keyword", detail: "<name>" },
  { label: "throws", type: "keyword", detail: "{type} description" }
];

// JSDoc completion source function
function completeJSDoc(context: CompletionContext): CompletionResult | null {
  // Resolve the syntax node before the cursor
  let nodeBefore = syntaxTree(context.state).resolveInner(context.pos, -1);

  // Check if we are inside a block comment that starts with /**
  if (nodeBefore.type.name !== "BlockComment" ||
      context.state.sliceDoc(nodeBefore.from, nodeBefore.from + 3) !== "/**") {
    return null;
  }

  // Get the text from the start of the comment line up to the cursor
  let line = context.state.sliceDoc(nodeBefore.from, context.pos);

  // Look for an '@' followed by optional word characters at the end of the line segment
  let tagMatch = line.match(/@(\w*)$/);

  let from;
  let validForRegex = /^\w*$/; // Default regex matches the typed tag name

  if (tagMatch) {
    // If '@tag' is matched, completion starts after the '@'
    from = nodeBefore.from + tagMatch.index + 1;
  } else {
    // If no '@tag' pattern immediately before cursor, check if explicit completion was requested
    if (!context.explicit) return null;

    // If explicit, check if the position is valid for starting a new tag (e.g., after *, space, tab, or '@')
     let charBefore = context.state.sliceDoc(context.pos - 1, context.pos);
     if (charBefore !== '*' && charBefore !== ' ' && charBefore !== '\t' && charBefore !== '@') {
          return null; // Not a position where a tag typically starts
      }

    // If explicit and at a valid position, start completion from the current cursor position
    from = context.pos;
    // The validFor regex still matches the characters typed *after* the 'from' position.
  }

  // Return the completions with the calculated 'from' position and 'validFor' regex
  return {
    from,
    options: jsDocTags,
    validFor: validForRegex
  };
}
```

----------------------------------------

TITLE: Changing Selection: Create Multiple Selections (CodeMirror, JavaScript)
DESCRIPTION: This code dispatches a transaction to set multiple selections using `EditorSelection.create`. It defines two selection ranges (`range(4, 5)`, `range(6, 7)`) and one cursor selection (`cursor(8)`). The second argument (`1`) specifies that the second range (index 1) should be the main selection.
SOURCE: https://github.com/codemirror/website/blob/main/site/examples/selection/index.md#_snippet_1

LANGUAGE: javascript
CODE:
```
view.dispatch({
  selection: EditorSelection.create([
    EditorSelection.range(4, 5),
    EditorSelection.range(6, 7),
    EditorSelection.cursor(8)
  ], 1)
})
```

----------------------------------------

TITLE: Enabling JSDoc Completion Extension (JS/TS)
DESCRIPTION: Shows how to create a CodeMirror extension by attaching the `completeJSDoc` completion source to the `autocomplete` data facet of the standard `javascript` language package. This makes the source active when editing JavaScript code.
SOURCE: https://github.com/codemirror/website/blob/main/site/examples/autocompletion/index.md#_snippet_3

LANGUAGE: TypeScript
CODE:
```
import { javascript } from "@codemirror/lang-javascript";
import { autocompletion } from "@codemirror/autocomplete";

// Assuming completeJSDoc is defined elsewhere
// This creates an extension that registers the completeJSDoc source
const jsDocCompletions = javascript().data.of({
  autocomplete: completeJSDoc
});

// This extension can then be included in the editor configuration
// basicSetup, jsDocCompletions, ...

```

----------------------------------------

TITLE: Installing CodeMirror and Rollup Dependencies
DESCRIPTION: Uses npm to install the core CodeMirror library, the JavaScript language package, the Rollup bundler, and the necessary Rollup plugin for resolving Node.js style modules.
SOURCE: https://github.com/codemirror/website/blob/main/site/examples/bundle/index.md#_snippet_1

LANGUAGE: shell
CODE:
```
npm i codemirror @codemirror/lang-javascript
npm i rollup @rollup/plugin-node-resolve
```

----------------------------------------

TITLE: Defining CodeMirror State Field (JavaScript)
DESCRIPTION: Defines a CodeMirror `StateField` called `countDocChanges` to track the number of document changes. The `create` function initializes the field to 0, and the `update` function increments the value by 1 if the transaction (`tr`) indicates a document change (`tr.docChanged`).
SOURCE: https://github.com/codemirror/website/blob/main/site/docs/guide/index.md#_snippet_16

LANGUAGE: JavaScript
CODE:
```
import {EditorState, StateField} from "@codemirror/state"

let countDocChanges = StateField.define({
  create() { return 0 },
  update(value, tr) { return tr.docChanged ? value + 1 : value }
})

let state = EditorState.create({extensions: countDocChanges})
state = state.update({changes: {from: 0, insert: "."}}).state
console.log(state.field(countDocChanges)) // 1
```

----------------------------------------

TITLE: Example Usage of ToggleWith (JavaScript)
DESCRIPTION: Shows how to use the toggleWith helper function to create an extension that toggles an editor attribute (background color) when the "Mod-o" key is pressed.
SOURCE: https://github.com/codemirror/website/blob/main/site/examples/config/index.md#_snippet_3

LANGUAGE: JavaScript
CODE:
```
toggleWith("Mod-o", EditorView.editorAttributes.of({
  style: "background: yellow"
}))
```

----------------------------------------

TITLE: Customizing CodeMirror Line Numbers Format (JS/TS)
DESCRIPTION: This snippet shows how to modify the default behavior of the built-in `lineNumbers` extension. It uses the `formatNumber` configuration option to provide a custom function that formats the line number value before it is displayed in the gutter, in this specific example converting the number to its hexadecimal representation.
SOURCE: https://github.com/codemirror/website/blob/main/site/examples/gutter/index.md#_snippet_4

LANGUAGE: JavaScript
CODE:
```
const hexLineNumbers = lineNumbers({
  formatNumber: n => n.toString(16)
})
```

----------------------------------------

TITLE: Appending Extensions to Configuration (JavaScript)
DESCRIPTION: Defines a JavaScript function injectExtension that dispatches a transaction using StateEffect.appendConfig. This effect adds the provided extension to the end of the editor's top-level configuration, which persists until a full reconfiguration occurs.
SOURCE: https://github.com/codemirror/website/blob/main/site/examples/config/index.md#_snippet_5

LANGUAGE: JavaScript
CODE:
```
function injectExtension(view, extension) {
  view.dispatch({
    effects: StateEffect.appendConfig.of(extension)
  })
}
```

----------------------------------------

TITLE: Registering Language-Specific Completion Source (JS/TS)
DESCRIPTION: Demonstrates how to associate a custom completion source function (`myCompletions`) with a specific language (`myLanguage`) using the `autocomplete` key in the `languageData` facet of the language configuration.
SOURCE: https://github.com/codemirror/website/blob/main/site/examples/autocompletion/index.md#_snippet_0

LANGUAGE: TypeScript
CODE:
```
myLanguage.data.of({
  autocomplete: myCompletions
})
```

----------------------------------------

TITLE: Accessing CodeMirror 6 Selection Information (CM5 vs CM6) (JavaScript)
DESCRIPTION: Provides mappings from CodeMirror 5 methods for accessing selection details (`getCursor`, `listSelections`, `getSelection`, `getSelections`, `somethingSelected`) to their CodeMirror 6 counterparts, which involve accessing the `state.selection` object.
SOURCE: https://github.com/codemirror/website/blob/main/site/docs/migration/index.md#_snippet_5

LANGUAGE: javascript
CODE:
```
cm.getCursor() → cm.state.selection.main.head

cm.listSelections() → cm.state.selection.ranges

cm.getSelection() → cm.state.sliceDoc(
  cm.state.selection.main.from,
  cm.state.selection.main.to)

cm.getSelections() → cm.state.selection.ranges.map(
  r => cm.state.sliceDoc(r.from, r.to))

cm.somethingSelected() → cm.state.selection.ranges.some(r => !r.empty)
```

----------------------------------------

TITLE: Defining CodeMirror Zebra Stripe Step Size Facet (TypeScript)
DESCRIPTION: Defines a CodeMirror `Facet` named `stepSize` to allow configuration of the distance between zebra stripes. It accepts multiple `number` values and combines them by taking the minimum, defaulting to `2` if no values are provided, ensuring a consistent configuration source.
SOURCE: https://github.com/codemirror/website/blob/main/site/examples/zebra/index.md#_snippet_1

LANGUAGE: TypeScript
CODE:
```
import {Facet} from "@codemirror/state";

const stepSize = Facet.define<number, number>({
  combine: values => values.length ? Math.min(...values) : 2
});
```

----------------------------------------

TITLE: Creating CodeMirror Zebra Stripes Extension (TypeScript)
DESCRIPTION: Exports the main function, `zebraStripes`, which serves as the entry point for the extension. This function takes an optional configuration object for the stripe step size and returns an array containing the `baseTheme`, the configured `stepSize` facet value (if provided), and the `showStripes` view plugin, composing the extension's functionality.
SOURCE: https://github.com/codemirror/website/blob/main/site/examples/zebra/index.md#_snippet_2

LANGUAGE: TypeScript
CODE:
```
import {StateEffect, StateField, StateFacet, Extension} from "@codemirror/state";
import {EditorView, ViewPlugin} from "@codemirror/view";
import {stripeDeco, stepSize} from "./zebra"; // Assuming imports

function zebraStripes(config: { step?: number } = {}): Extension {
  return [
    baseTheme, // Assuming baseTheme is defined and imported
    config.step != null ? stepSize.of(config.step) : [],
    showStripes // Assuming showStripes is defined and imported
  ];
}
```

----------------------------------------

TITLE: Converting CodeMirror Positions (CM5 {line, ch} <-> CM6 offset) (JavaScript)
DESCRIPTION: Provides JavaScript functions to convert between the CodeMirror 5 `{line, ch}` position format and the CodeMirror 6 offset format. `posToOffset` takes a CM6 document and CM5 position to get an offset, while `offsetToPos` takes a CM6 document and offset to get a CM5 position.
SOURCE: https://github.com/codemirror/website/blob/main/site/docs/migration/index.md#_snippet_3

LANGUAGE: javascript
CODE:
```
function posToOffset(doc, pos) {
  return doc.line(pos.line + 1).from + pos.ch
}
function offsetToPos(doc, offset) {
  let line = doc.lineAt(offset)
  return {line: line.number - 1, ch: offset - line.from}
}
```

----------------------------------------

TITLE: Adding Basic Custom CodeMirror Gutter (JS/TS)
DESCRIPTION: This snippet shows how to include a custom gutter in the editor's extensions array. It places the custom gutter after the built-in line number gutter and assigns a CSS class for styling, although it notes that a minimum width might be required for visibility if no content is present.
SOURCE: https://github.com/codemirror/website/blob/main/site/examples/gutter/index.md#_snippet_0

LANGUAGE: JavaScript
CODE:
```
extensions: [lineNumbers(), gutter({class: "cm-mygutter"})]
```

----------------------------------------

TITLE: Configuring Collab Extension with Shared Effects - TypeScript
DESCRIPTION: Configures the CodeMirror `collab` extension using the `sharedEffects` option. This option takes a function that filters transaction effects, including only specific custom effects like `markRegion` in the updates shared with other collaborative peers.
SOURCE: https://github.com/codemirror/website/blob/main/site/examples/collab/index.md#_snippet_6

LANGUAGE: TypeScript
CODE:
```
import {collab} from "@codemirror/collab"

let markCollab = {
  // ...
  sharedEffects: tr => tr.effects.filter(e => e.is(markRegion))
}
```

----------------------------------------

TITLE: Changing Selection: Move Cursor to Start (CodeMirror, JavaScript)
DESCRIPTION: This snippet dispatches a transaction to the CodeMirror `view` object. It sets the `selection` property of the transaction spec to an object specifying the desired anchor position (0), effectively moving the cursor to the beginning of the document.
SOURCE: https://github.com/codemirror/website/blob/main/site/examples/selection/index.md#_snippet_0

LANGUAGE: javascript
CODE:
```
view.dispatch({selection: {anchor: 0}})
```

----------------------------------------

TITLE: Setting Minimum Height CodeMirror Content/Gutter JavaScript
DESCRIPTION: Configures a minimum height for the editor by applying a CSS `minHeight` rule to the `.cm-content` and `.cm-gutter` elements via EditorView.theme. This ensures the editor area and its associated gutter maintain a minimum vertical size, accommodating CSS limitations where applying `min-height` to the wrapper element is insufficient for this purpose. Requires the EditorView class.
SOURCE: https://github.com/codemirror/website/blob/main/site/examples/styling/index.md#_snippet_7

LANGUAGE: javascript
CODE:
```
const minHeightEditor = EditorView.theme({
  ".cm-content, .cm-gutter": {minHeight: "200px"}
})
```

----------------------------------------

TITLE: Setting Fixed Height and Overflow Auto CodeMirror JavaScript
DESCRIPTION: Applies CSS rules using EditorView.theme to control the editor's overall height and its internal scroller. It sets the main editor element's height to "300px" and the scroller element's overflow property to "auto", causing scrollbars to appear if content overflows vertically. Requires the EditorView class from CodeMirror.
SOURCE: https://github.com/codemirror/website/blob/main/site/examples/styling/index.md#_snippet_6

LANGUAGE: javascript
CODE:
```
const fixedHeightEditor = EditorView.theme({
  "&": {height: "300px"},
  ".cm-scroller": {overflow: "auto"}
})
```

----------------------------------------

TITLE: Replacing Top-Level Configuration (JavaScript)
DESCRIPTION: Defines a JavaScript function deconfigure that dispatches a transaction using StateEffect.reconfigure. This effect replaces the editor's entire top-level configuration with a new set of extensions (in this case, an empty array), effectively removing most functionality.
SOURCE: https://github.com/codemirror/website/blob/main/site/examples/config/index.md#_snippet_4

LANGUAGE: JavaScript
CODE:
```
import {StateEffect} from "@codemirror/state"

export function deconfigure(view) {
  view.dispatch({
    effects: StateEffect.reconfigure.of([])
  })
}
```

----------------------------------------

TITLE: Defining Custom StateEffect for Shared Regions - TypeScript
DESCRIPTION: Defines a custom CodeMirror `StateEffect` named `markRegion` to represent a marked region within the document. It requires a `map` function for effects that refer to document positions, which adjusts the positions correctly when the document undergoes changes.
SOURCE: https://github.com/codemirror/website/blob/main/site/examples/collab/index.md#_snippet_5

LANGUAGE: TypeScript
CODE:
```
import {StateEffect} from "@codemirror/state"

const markRegion = StateEffect.define<{from: number, to: number}>({
  map({from, to}, changes) {
    from = changes.mapPos(from, 1)
    to = changes.mapPos(to, -1)
    return from < to ? {from, to} : undefined
  }
})
```

----------------------------------------

TITLE: Defining a CodeMirror Base Theme JavaScript
DESCRIPTION: Shows how to create a base theme using `EditorView.baseTheme`, typically used by extensions adding new DOM elements. It illustrates styling a custom class (`.cm-o-replacement`) and using `&light` and `&dark` placeholders to provide different background colors based on the editor's theme.
SOURCE: https://github.com/codemirror/website/blob/main/site/examples/styling/index.md#_snippet_3

LANGUAGE: javascript
CODE:
```
import {EditorView} from "@codemirror/view"

let baseTheme = EditorView.baseTheme({
  ".cm-o-replacement": {
    display: "inline-block",
    width: ".5em",
    height: ".5em",
    borderRadius: ".25em"
  },
  "&light .cm-o-replacement": {
    backgroundColor: "#04c"
  },
  "&dark .cm-o-replacement": {
    backgroundColor: "#5bf"
  }
})
```

----------------------------------------

TITLE: Defining CodeMirror Base Theme (JavaScript)
DESCRIPTION: Creates a base theme extension for CodeMirror using `EditorView.baseTheme`. It defines background colors for a custom selector (`.cm-mySelector`), using `&dark` and `&light` placeholders to apply styles conditionally based on whether a dark or light theme is active.
SOURCE: https://github.com/codemirror/website/blob/main/site/docs/guide/index.md#_snippet_13

LANGUAGE: JavaScript
CODE:
```
import {EditorView} from "@codemirror/view"

// This again produces an extension value
let myBaseTheme = EditorView.baseTheme({
  "&dark .cm-mySelector": { background: "dimgrey" },
  "&light .cm-mySelector": { background: "ghostwhite" }
})
```

----------------------------------------

TITLE: Defining German Phrases Map - JavaScript
DESCRIPTION: Defines a JavaScript object (`germanPhrases`) mapping English phrases used in CodeMirror core modules to their German translations. This object is intended to be used to provide internationalization data to the editor state.
SOURCE: https://github.com/codemirror/website/blob/main/site/examples/translate/index.md#_snippet_0

LANGUAGE: JavaScript
CODE:
```
const germanPhrases = {
  // Common core phrases
  "Loading...": "Lädt...",
  "Cancel": "Abbrechen",

  // Search phrases
  "Replace": "Ersetzen",
  "replace": "ersetzen",
  "Replace All": "Alle ersetzen",
  "replace all": "alle ersetzen",
  "Too many matches. Try the next one instead?": "Zu viele Treffer. Lieber den nächsten probieren?",
  "Replace next?": "Nächsten ersetzen?",

  // Selection phrases
  "Selection": "Auswahl",

  // Folding phrases
  "Fold Line": "Zeile falten",
  "Unfold Line": "Zeile entfalten",

  // Linting phrases
  "Diagnostic": "Diagnose", // singular
  "Diagnostics": "Diagnosen" // plural

  // ... other phrases from CodeMirror core packages
};
```

----------------------------------------

TITLE: Defining CodeMirror Zebra Stripe Base Theme (TypeScript)
DESCRIPTION: Sets up a base theme for CodeMirror to style lines with the class `cm-zebraStripe`. It provides different background colors for light and dark editor themes, allowing user themes to potentially override these styles. This is a foundational step for applying the visual effect.
SOURCE: https://github.com/codemirror/website/blob/main/site/examples/zebra/index.md#_snippet_0

LANGUAGE: TypeScript
CODE:
```
import {EditorView} from "@codemirror/view";
import {EditorSelection, EditorState} from "@codemirror/state";

const baseTheme = EditorView.baseTheme({
  "&light .cm-zebraStripe": {backgroundColor: "#eee"},
  "&dark .cm-zebraStripe": {backgroundColor: "#1b1b1b"}
});
```

----------------------------------------

TITLE: Bundling CodeMirror with Rollup Command
DESCRIPTION: Executes the Rollup command from the node_modules bin directory. It specifies the entry file, the output format (iife), the output filename, and loads the node-resolve plugin.
SOURCE: https://github.com/codemirror/website/blob/main/site/examples/bundle/index.md#_snippet_2

LANGUAGE: shell
CODE:
```
node_modules/.bin/rollup editor.mjs -f iife -o editor.bundle.js \
  -p @rollup/plugin-node-resolve
```

----------------------------------------

TITLE: Styling CodeMirror with Traditional CSS
DESCRIPTION: Shows how to apply styles to CodeMirror's main elements (`cm-editor`, `cm-content`) using standard CSS selectors. It includes examples for focus outline and content font family, highlighting the need to match or exceed the specificity of injected styles.
SOURCE: https://github.com/codemirror/website/blob/main/site/examples/styling/index.md#_snippet_0

LANGUAGE: css
CODE:
```
.cm-editor.cm-focused { outline: 2px solid cyan }
.cm-editor .cm-content { font-family: "Consolas" }
```

----------------------------------------

TITLE: CodeMirror Editor DOM Structure HTML
DESCRIPTION: Illustrates the basic HTML structure created by the CodeMirror editor view. It shows the main wrapper (cm-editor), the scrollable container (cm-scroller), the editable content area (cm-content), and individual line elements (cm-line). This structure is managed internally by CodeMirror, and user code should typically use decorations instead of direct DOM manipulation.
SOURCE: https://github.com/codemirror/website/blob/main/site/docs/guide/index.md#_snippet_11

LANGUAGE: html
CODE:
```
<div class="cm-editor [theme scope classes]">
  <div class="cm-scroller">
    <div class="cm-content" contenteditable="true">
      <div class="cm-line">Content goes here</div>
      <div class="cm-line">...</div>
    </div>
  </div>
</div>
```

----------------------------------------

TITLE: CodeMirror Editor DOM Structure HTML
DESCRIPTION: Provides a simplified representation of the HTML structure of a CodeMirror editor instance. It shows the key CSS classes (`cm-editor`, `cm-scroller`, `cm-content`, `cm-line`, `cm-gutters`, etc.) used for styling and understanding the element hierarchy.
SOURCE: https://github.com/codemirror/website/blob/main/site/examples/styling/index.md#_snippet_1

LANGUAGE: html
CODE:
```
<div class="cm-editor [cm-focused] [generated classes]">
  <div class="cm-scroller">
    <div class="cm-gutters">
      <div class="cm-gutter [...]">
        <!-- One gutter element for each line -->
        <div class="cm-gutterElement">...</div>
      </div>
    </div>
    <div class="cm-content" contenteditable="true">
      <!-- The actual document content -->
      <div class="cm-line">Content goes here</div>
      <div class="cm-line">...</div>
    </div>
    <div class="cm-selectionLayer">
      <!-- Positioned rectangles to draw the selection -->
      <div class="cm-selectionBackground"></div>
    </div>
    <div class="cm-cursorLayer">
      <!-- Positioned elements to draw cursors -->
      <div class="cm-cursor"></div>
    </div>
  </div>
</div>
```

----------------------------------------

TITLE: Styling CodeMirror Content (CSS)
DESCRIPTION: Provides a standard CSS rule to style the content area (`.cm-content`) of a CodeMirror editor. Including `.cm-editor` in the selector matches the precedence of injected styles, ensuring the rule is applied correctly.
SOURCE: https://github.com/codemirror/website/blob/main/site/docs/guide/index.md#_snippet_14

LANGUAGE: CSS
CODE:
```
.cm-editor .cm-content { color: purple; }
```

----------------------------------------

TITLE: Applying Translated Phrases to EditorState - JavaScript
DESCRIPTION: Demonstrates how to include a custom translation map, like `germanPhrases`, in the CodeMirror editor configuration. The `EditorState.phrases.of()` method creates an extension that provides the translations to the editor state, making them available via `state.phrase()`.
SOURCE: https://github.com/codemirror/website/blob/main/site/examples/translate/index.md#_snippet_1

LANGUAGE: JavaScript
CODE:
```
import { EditorState } from "@codemirror/state";

// Assuming germanPhrases is defined elsewhere, e.g., imported
// import { germanPhrases } from "./translate"; // Example import

const state = EditorState.create({
  // ... other configuration
  extensions: [
    // Add the phrases facet with the germanTranslations map
    EditorState.phrases.of(germanPhrases)
    // ... other extensions
  ]
});

// Within a component or plugin, you can then use:
// state.phrase("Loading..."); // Would return "Lädt..."
```

----------------------------------------

TITLE: Handling Authority Messages - CodeMirror Collab Example
DESCRIPTION: Implements the message handling logic for the central authority (simulated by a web worker). It processes `pullUpdates` to provide changes since a client's version, `pushUpdates` to accept and integrate new changes, and `getDocument` to provide the initial state for new peers. It uses postMessage for communication.
SOURCE: https://github.com/codemirror/website/blob/main/site/examples/collab/index.md#_snippet_1

LANGUAGE: TypeScript
CODE:
```
let pending: ((value: unknown) => void)[] = []

self.onmessage = async e => {
  let resp: any = {id: e.data.id}
  let clientID = e.data.clientID
  try {
    if (e.data.type == "pullUpdates") {
      let version = e.data.version
      if (version < updates.length) {
        resp.updates = updates.slice(version)
      } else if (version > updates.length) {
        resp.error = "Acked version higher than server version"
      } else { // Wait for updates
        await new Promise(f => pending.push(f))
        resp.updates = updates.slice(version)
      }
    } else if (e.data.type == "pushUpdates") {
      if (e.data.version != updates.length) {
        resp.error = "Version mismatch"
      } else {
        for (let update of e.data.updates) {
          updates.push(update)
          document = update.changes.apply(document)
        }
        for (let f of pending) f(null)
        pending.length = 0
      }
    } else if (e.data.type == "getDocument") {
      resp.version = updates.length
      resp.doc = document
    } else {
      resp.error = "Unknown request type " + e.data.type
    }
  } catch(e: any) {
    resp.error = e && e.message || String(e)
  }
  (e.data.port || self).postMessage(resp)
}
```

----------------------------------------

TITLE: Defining Authority State - CodeMirror Collab Example
DESCRIPTION: Defines the state structure for the central authority (server) in the collaborative editing setup. It includes an array of updates (ChangeSet with client ID) and the current document state. This state is crucial for new peers joining the session and for processing incoming updates.
SOURCE: https://github.com/codemirror/website/blob/main/site/examples/collab/index.md#_snippet_0

LANGUAGE: TypeScript
CODE:
```
let updates: {changes: ChangeSet, clientID: string}[] = []
let document = ""
```

----------------------------------------

TITLE: Creating Peer-Side Communication Wrappers - CodeMirror Collab Example
DESCRIPTION: Defines asynchronous helper functions (`pullUpdates`, `pushUpdates`, `getDocument`) that wrap the underlying communication mechanism (simulated by a `Connection` class interacting with a web worker) to provide a Promise-based API. These are used by the peer's view plugin to interact with the central authority.
SOURCE: https://github.com/codemirror/website/blob/main/site/examples/collab/index.md#_snippet_2

LANGUAGE: TypeScript
CODE:
```
async function pullUpdates(conn: Connection, version: number) {
  return conn.request<any>({type: "pullUpdates", version})
}

async function pushUpdates(conn: Connection, version: number, updates: readonly {changes: ChangeSet, clientID: string}[]) {
  return conn.request<any>({type: "pushUpdates", version, updates})
}

async function getDocument(conn: Connection) {
  return conn.request<any>({type: "getDocument"})
}
```

----------------------------------------

TITLE: Initializing CodeMirror with Minimal Setup
DESCRIPTION: Imports necessary modules from CodeMirror packages and creates a new EditorView instance using minimalSetup. This reduces the number of default extensions included, resulting in a smaller bundle size.
SOURCE: https://github.com/codemirror/website/blob/main/site/examples/bundle/index.md#_snippet_5

LANGUAGE: javascript
CODE:
```
import {EditorView, minimalSetup} from "codemirror"

let editor = new EditorView({
  extensions: minimalSetup,
  parent: document.body
})
```

----------------------------------------

TITLE: Defining Rollup Configuration File (JavaScript)
DESCRIPTION: Provides an alternative way to configure Rollup using a JavaScript file (rollup.config.mjs). It imports the node-resolve plugin and exports an object defining the input, output (file and format), and plugins.
SOURCE: https://github.com/codemirror/website/blob/main/site/examples/bundle/index.md#_snippet_3

LANGUAGE: javascript
CODE:
```
import {nodeResolve} from "@rollup/plugin-node-resolve"
export default {
  input: "./editor.mjs",
  output: {
    file: "./editor.bundle.js",
    format: "iife"
  },
  plugins: [nodeResolve()]
}
```

----------------------------------------

TITLE: Including Bundled Script in HTML
DESCRIPTION: A minimal HTML document demonstrating how to include the generated editor.bundle.js file using a script tag to load the bundled CodeMirror editor into the page.
SOURCE: https://github.com/codemirror/website/blob/main/site/examples/bundle/index.md#_snippet_4

LANGUAGE: html
CODE:
```
<!doctype html>
<meta charset=utf8>
<h1>CodeMirror!</h1>
<script src="editor.bundle.js"></script>
```

----------------------------------------

TITLE: Configuring Rollup for IE11 - Javascript
DESCRIPTION: Configure Rollup to bundle Javascript files (`./editor.js`) and compile them down to ES5 for IE11 compatibility. It uses `@rollup/plugin-babel` for syntax transformation and `@rollup/plugin-node-resolve` to handle node module resolution. The output is a UMD format file (`./www/editor.js`). This setup is necessary because CodeMirror is written in ES2018/ES2015 syntax.
SOURCE: https://github.com/codemirror/website/blob/main/site/examples/ie11/index.md#_snippet_0

LANGUAGE: javascript
CODE:
```
import babel from "@rollup/plugin-babel";
import resolve from "@rollup/plugin-node-resolve";

export default {
  input: "./editor.js",
  output: {
    file: "./www/editor.js",
    format: "umd"
  },
  plugins: [
    babel(),
    resolve()
  ]
};
```

----------------------------------------

TITLE: Styling CodeMirror Editor Height (CSS)
DESCRIPTION: This CSS snippet sets a fixed height for the CodeMirror editor element. It ensures the editor's visible area is constrained to 400 pixels on the page.
SOURCE: https://github.com/codemirror/website/blob/main/site/examples/million/index.md#_snippet_0

LANGUAGE: CSS
CODE:
```
.cm-editor { height: 400px }
```

----------------------------------------

TITLE: Building CodeMirror Website - Shell
DESCRIPTION: Provides shell commands to build the CodeMirror website from the source repository. It requires Node.js and expects the repository to be checked out within `codemirror/dev`. The steps install dependencies, build the project, and show where the output is located.
SOURCE: https://github.com/codemirror/website/blob/main/README.md#_snippet_0

LANGUAGE: Shell
CODE:
```
npm i
npm run build
ls output # ← the website is in there
```