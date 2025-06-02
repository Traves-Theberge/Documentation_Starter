TITLE: Creating a Minimal ProseMirror Editor in JavaScript
DESCRIPTION: Demonstrates how to create a basic ProseMirror editor by importing the schema, creating an editor state, and initializing an editor view. This minimal example creates an empty document conforming to the schema with a default selection at the start.
SOURCE: https://github.com/prosemirror/website/blob/master/markdown/guide/intro.md#2025-04-23_snippet_0

LANGUAGE: javascript
CODE:
```
import {schema} from "prosemirror-schema-basic"
import {EditorState} from "prosemirror-state"
import {EditorView} from "prosemirror-view"

let state = EditorState.create({schema})
let view = new EditorView(document.body, {state})
```

----------------------------------------

TITLE: Setting up a basic ProseMirror editor with example-setup in JavaScript
DESCRIPTION: This code snippet demonstrates how to set up a basic ProseMirror editor using the example-setup package. It imports the necessary modules, creates a schema that extends the basic schema with list nodes, and then initializes an editor with various plugins for a complete editing experience.
SOURCE: https://github.com/prosemirror/website/blob/master/pages/examples/basic/index.md#2025-04-23_snippet_0

LANGUAGE: javascript
CODE:
```
PART(code)
```

----------------------------------------

TITLE: Creating a Collaborative Editor with ProseMirror collab Module in JavaScript
DESCRIPTION: This code demonstrates how to set up a collaborative editor using ProseMirror's collab module. It creates an editor view with the collab plugin, tracks local changes, receives remote changes, and communicates with the central authority to synchronize document state across multiple editors.
SOURCE: https://github.com/prosemirror/website/blob/master/markdown/guide/collab.md#2025-04-23_snippet_1

LANGUAGE: javascript
CODE:
```
import {EditorState} from "prosemirror-state"
import {EditorView} from "prosemirror-view"
import {schema} from "prosemirror-schema-basic"
import collab from "prosemirror-collab"

function collabEditor(authority, place) {
  let view = new EditorView(place, {
    state: EditorState.create({
      doc: authority.doc,
      plugins: [collab.collab({version: authority.steps.length})]
    }),
    dispatchTransaction(transaction) {
      let newState = view.state.apply(transaction)
      view.updateState(newState)
      let sendable = collab.sendableSteps(newState)
      if (sendable)
        authority.receiveSteps(sendable.version, sendable.steps,
                               sendable.clientID)
    }
  })

  authority.onNewSteps.push(function() {
    let newData = authority.stepsSince(collab.getVersion(view.state))
    view.dispatch(
      collab.receiveTransaction(view.state, newData.steps, newData.clientIDs))
  })

  return view
}
```

----------------------------------------

TITLE: Adding History and Keymap Plugins to ProseMirror
DESCRIPTION: Demonstrates how to extend the editor functionality using plugins. This example adds undo/redo capability with the history plugin and registers keyboard shortcuts for these functions with the keymap plugin.
SOURCE: https://github.com/prosemirror/website/blob/master/markdown/guide/intro.md#2025-04-23_snippet_2

LANGUAGE: javascript
CODE:
```
// (Omitted repeated imports)
import {undo, redo, history} from "prosemirror-history"
import {keymap} from "prosemirror-keymap"

let state = EditorState.create({
  schema,
  plugins: [
    history(),
    keymap({"Mod-z": undo, "Mod-y": redo})
  ]
})
let view = new EditorView(document.body, {state})
```

----------------------------------------

TITLE: Defining a Basic Schema with Node Types in JavaScript
DESCRIPTION: Example of creating a simple ProseMirror schema with basic node types. The schema defines a document structure where the document contains paragraphs, and paragraphs contain text.
SOURCE: https://github.com/prosemirror/website/blob/master/markdown/guide/schema.md#2025-04-23_snippet_0

LANGUAGE: javascript
CODE:
```
const trivialSchema = new Schema({
  nodes: {
    doc: {content: "paragraph+"},
    paragraph: {content: "text*"},
    text: {inline: true},
    /* ... and so on */
  }
})
```

----------------------------------------

TITLE: Implementing Central Authority for ProseMirror Collaborative Editing in JavaScript
DESCRIPTION: This code defines an Authority class that tracks document versions, accepts changes from editors, and provides a way for editors to receive changes since a given version. It handles the central coordination of collaborative editing by managing steps, client IDs, and document state.
SOURCE: https://github.com/prosemirror/website/blob/master/markdown/guide/collab.md#2025-04-23_snippet_0

LANGUAGE: javascript
CODE:
```
class Authority {
  constructor(doc) {
    this.doc = doc
    this.steps = []
    this.stepClientIDs = []
    this.onNewSteps = []
  }

  receiveSteps(version, steps, clientID) {
    if (version != this.steps.length) return

    // Apply and accumulate new steps
    steps.forEach(step => {
      this.doc = step.apply(this.doc).doc
      this.steps.push(step)
      this.stepClientIDs.push(clientID)
    })
    // Signal listeners
    this.onNewSteps.forEach(function(f) { f() })
  }

  stepsSince(version) {
    return {
      steps: this.steps.slice(version),
      clientIDs: this.stepClientIDs.slice(version)
    }
  }
}
```

----------------------------------------

TITLE: Creating and Examining Editor State in ProseMirror (JavaScript)
DESCRIPTION: Demonstrates how to create a basic editor state using ProseMirror and examine its document and selection properties. The code imports the basic schema and EditorState, creates a new state, and logs its initial document content and selection position.
SOURCE: https://github.com/prosemirror/website/blob/master/markdown/guide/state.md#2025-04-23_snippet_0

LANGUAGE: javascript
CODE:
```
import {schema} from "prosemirror-schema-basic"
import {EditorState} from "prosemirror-state"

let state = EditorState.create({schema})
console.log(state.doc.toString()) // An empty paragraph
console.log(state.selection.from) // 1, the start of the paragraph
```

----------------------------------------

TITLE: Creating a Document with ProseMirror Schema
DESCRIPTION: A JavaScript example demonstrating how to programmatically create a ProseMirror document by using the schema's node and text methods. It creates a document with two paragraphs separated by a horizontal rule.
SOURCE: https://github.com/prosemirror/website/blob/master/markdown/guide/doc.md#2025-04-23_snippet_1

LANGUAGE: javascript
CODE:
```
import {schema} from "prosemirror-schema-basic"

// (The null arguments are where you can specify attributes, if necessary.)
let doc = schema.node("doc", null, [
  schema.node("paragraph", null, [schema.text("One.")]),
  schema.node("horizontal_rule"),
  schema.node("paragraph", null, [schema.text("Two!")])
])
```

----------------------------------------

TITLE: Implementing Basic Editor Commands with ProseMirror
DESCRIPTION: Shows how to add basic editing functionality by implementing common commands. This example adds the baseKeymap which provides expected behavior for keys like Enter and Delete, along with previously configured undo/redo functionality.
SOURCE: https://github.com/prosemirror/website/blob/master/markdown/guide/intro.md#2025-04-23_snippet_3

LANGUAGE: javascript
CODE:
```
// (Omitted repeated imports)
import {baseKeymap} from "prosemirror-commands"

let state = EditorState.create({
  schema,
  plugins: [
    history(),
    keymap({"Mod-z": undo, "Mod-y": redo}),
    keymap(baseKeymap)
  ]
})
let view = new EditorView(document.body, {state})
```

----------------------------------------

TITLE: Defining Menu Items for ProseMirror in JavaScript
DESCRIPTION: This code creates menu items for a basic ProseMirror menu with text formatting (strong, emphasis) and block type (paragraph, heading) options. Each menu item has a DOM element and a command to execute when clicked.
SOURCE: https://github.com/prosemirror/website/blob/master/pages/examples/menu/index.md#2025-04-23_snippet_2

LANGUAGE: javascript
CODE:
```
// Helper function to create menu items
function item(command, content) {
  let dom = document.createElement("button")
  dom.appendChild(typeof content === "string" ? document.createTextNode(content) : content)
  return {command, dom}
}

// Create an icon element for font style buttons
function fontIcon(text, name) {
  let span = document.createElement("span")
  span.className = "font-icon " + name
  span.textContent = text
  return span
}

// A set of basic menu items
let menu = [
  item(toggleMark(schema.marks.strong), fontIcon("B", "strong")),
  item(toggleMark(schema.marks.em), fontIcon("i", "em")),
  item(setBlockType(schema.nodes.paragraph), "paragraph"),
  item(setBlockType(schema.nodes.heading, {level: 1}), "heading")
]
```

----------------------------------------

TITLE: Implementing ProseMirror-based Markdown View in JavaScript
DESCRIPTION: This code defines a ProseMirrorView class that integrates a WYSIWYG editor using ProseMirror for editing markdown content. It handles converting between markdown text and ProseMirror documents, sets up plugins for the editor, and provides the same interface as the MarkdownView.
SOURCE: https://github.com/prosemirror/website/blob/master/pages/examples/markdown/index.md#2025-04-23_snippet_1

LANGUAGE: javascript
CODE:
```
class ProseMirrorView {
  constructor(target, content) {
    // Create a new editor state with the markdown schema and plugins
    this.state = EditorState.create({
      doc: markdownParser.parse(content),
      plugins: [keymap(baseKeymap)]
    })
    // Create an editor view in the target element
    this.view = new EditorView(target, {
      state: this.state,
      dispatchTransaction: transaction => {
        this.state = this.state.apply(transaction)
        this.view.updateState(this.state)
      }
    })
  }

  get content() {
    return markdownSerializer.serialize(this.state.doc)
  }

  focus() { this.view.focus() }
  destroy() { this.view.destroy() }

  update(content) {
    let newDoc = markdownParser.parse(content)
    if (!this.state.doc.eq(newDoc)) {
      this.view.dispatch(this.state.tr.replaceWith(0, this.state.doc.content.size, newDoc.content))
      return true
    }
    return false
  }
}
```

----------------------------------------

TITLE: Manipulating Selection in ProseMirror Transactions (JavaScript)
DESCRIPTION: Demonstrates how selections are automatically mapped through transaction steps and how to explicitly set a new selection. The example shows selection position changes when deleting content and manually setting a selection.
SOURCE: https://github.com/prosemirror/website/blob/master/markdown/guide/state.md#2025-04-23_snippet_2

LANGUAGE: javascript
CODE:
```
let tr = state.tr
console.log(tr.selection.from) // → 10
tr.delete(6, 8)
console.log(tr.selection.from) // → 8 (moved back)
tr.setSelection(TextSelection.create(tr.doc, 3))
console.log(tr.selection.from) // → 3
```

----------------------------------------

TITLE: Creating a Schema with Node Groups in JavaScript
DESCRIPTION: Demonstrates how to use node groups in a ProseMirror schema. This example creates a 'block' group containing paragraph and blockquote nodes, allowing for more flexible content expressions.
SOURCE: https://github.com/prosemirror/website/blob/master/markdown/guide/schema.md#2025-04-23_snippet_1

LANGUAGE: javascript
CODE:
```
const groupSchema = new Schema({
  nodes: {
    doc: {content: "block+"},
    paragraph: {group: "block", content: "text*"},
    blockquote: {group: "block", content: "block+"},
    text: {}
  }
})
```

----------------------------------------

TITLE: Defining Star Schema with Inline Nodes and Marks in ProseMirror
DESCRIPTION: This snippet creates a more advanced schema that includes inline 'star' nodes and various marks like 'shouting' and 'link'. It demonstrates how to define node groups, restrict mark usage, and configure DOM rendering and parsing for custom elements.
SOURCE: https://github.com/prosemirror/website/blob/master/pages/examples/schema/index.md#2025-04-23_snippet_3

LANGUAGE: javascript
CODE:
```
const starSchema = new Schema({
  nodes: {
    doc: {content: "block+"},
    paragraph: {
      group: "block",
      content: "inline*",
      toDOM: () => ["p", 0]
    },
    boring_paragraph: {
      group: "block",
      content: "text*",
      marks: "",
      toDOM: () => ["p", {class: "boring"}, 0]
    },
    text: {group: "inline"},
    star: {
      group: "inline",
      inline: true,
      toDOM: () => ["star", "★"],
      parseDOM: [{tag: "star"}]
    }
  },
  marks: {
    shouting: {
      toDOM: () => ["shouting", 0],
      parseDOM: [{tag: "shouting"}]
    },
    link: {
      attrs: {href: {}},
      toDOM: spec => ["a", {href: spec.href}, 0],
      parseDOM: [{tag: "a", getAttrs: dom => ({href: dom.href})}],
      inclusive: false
    }
  }
})
```

----------------------------------------

TITLE: Implementing Command with Optional Dispatch in JavaScript
DESCRIPTION: An improved version of the deleteSelection command that handles the optional dispatch argument. This pattern allows the command to be used both for execution and for querying whether the command is applicable in the current state.
SOURCE: https://github.com/prosemirror/website/blob/master/markdown/guide/commands.md#2025-04-23_snippet_1

LANGUAGE: javascript
CODE:
```
function deleteSelection(state, dispatch) {
  if (state.selection.empty) return false
  if (dispatch) dispatch(state.tr.deleteSelection())
  return true
}
```

----------------------------------------

TITLE: Creating and Applying Transactions in ProseMirror (JavaScript)
DESCRIPTION: Shows how to create a transaction from the current state, modify document content by inserting text, and apply the transaction to generate a new state. The example demonstrates how document size changes after text insertion.
SOURCE: https://github.com/prosemirror/website/blob/master/markdown/guide/state.md#2025-04-23_snippet_1

LANGUAGE: javascript
CODE:
```
let tr = state.tr
console.log(tr.doc.content.size) // 25
tr.insertText("hello") // Replaces selection with 'hello'
let newState = state.apply(tr)
console.log(tr.doc.content.size) // 30
```

----------------------------------------

TITLE: Implementing Document Linting Functions in ProseMirror
DESCRIPTION: A function that analyzes a ProseMirror document and returns an array of detected problems. It iterates through all document nodes, checking for issues like empty blocks, trailing whitespace, and duplicate headings, and provides appropriate fix methods.
SOURCE: https://github.com/prosemirror/website/blob/master/pages/examples/lint/index.md#2025-04-23_snippet_0

LANGUAGE: javascript
CODE:
```
function lint(doc) {
  let result = []
  let headers = Object.create(null)

  function record(node, from, to, message, fix) {
    result.push({node, from, to, message, fix})
  }

  doc.descendants((node, pos) => {
    if (node.isText) {
      // Warn about suspicious characters
      let m, re = /\w[\^\|\$%\*][\w\[\]\(\),]/g
      while (m = re.exec(node.text))
        record(node, pos + m.index, pos + m.index + 1,
               "Suspicious character: " + m[0][1])

      // Warn about repeated words
      let word = /\w+/g, lastWord = null, lastWordPos = 0
      while (m = word.exec(node.text)) {
        if (m[0].toLowerCase() == lastWord)
          record(node, pos + lastWordPos, pos + word.lastIndex,
                 "Duplicate word: " + m[0],
                 deleteRange(pos + lastWordPos, pos + word.lastIndex))
        lastWord = m[0].toLowerCase()
        lastWordPos = m.index
      }
    } else if (node.type.name == "heading") {
      let plain = node.content.content[0]?.textContent.trim() || ""
      if (plain in headers)
        record(node, pos, pos + node.nodeSize,
               "Duplicate heading: " + plain,
               deleteRange(pos, pos + node.nodeSize))
      headers[plain] = pos
    } else if (node.type.name == "image" && !node.attrs.alt) {
      record(node, pos, pos + node.nodeSize,
             "Image without alt text",
             addAlt(pos))
    } else if (node.type.name == "paragraph" && node.content.size == 0) {
      record(node, pos, pos + node.nodeSize,
             "Empty paragraph",
             deleteRange(pos, pos + node.nodeSize))
    } else if (node.type.name == "blockquote" && node.childCount == 0) {
      record(node, pos, pos + node.nodeSize,
             "Empty blockquote",
             deleteRange(pos, pos + node.nodeSize))
    } else if (node.type.name == "heading" && node.attrs.level > 2 &&
               node.textContent.length > 100) {
      record(node, pos, pos + node.nodeSize,
             "Long heading")
    } else if (node.isTextblock && node.content.size > 0) {
      let last = node.content.content[node.content.content.length - 1]
      if (last.isText && /\s$/.test(last.text)) {
        record(node, pos + node.nodeSize - 2, pos + node.nodeSize - 1,
               "Trailing whitespace",
               deleteRange(pos + node.nodeSize - 2, pos + node.nodeSize - 1))
      }
    }
  })

  return result
}
```

----------------------------------------

TITLE: Handling ProseMirror Transactions with Custom Dispatch Function
DESCRIPTION: Shows how to hook into ProseMirror's state transaction system by defining a custom dispatchTransaction function. This example logs document size changes and demonstrates the explicit state update pattern required in ProseMirror.
SOURCE: https://github.com/prosemirror/website/blob/master/markdown/guide/intro.md#2025-04-23_snippet_1

LANGUAGE: javascript
CODE:
```
// (Imports omitted)

let state = EditorState.create({schema})
let view = new EditorView(document.body, {
  state,
  dispatchTransaction(transaction) {
    console.log("Document size went from", transaction.before.content.size,
                "to", transaction.doc.content.size)
    let newState = view.state.apply(transaction)
    view.updateState(newState)
  }
})
```

----------------------------------------

TITLE: Defining a Schema with Marks in JavaScript
DESCRIPTION: Shows how to define marks in a ProseMirror schema. This example creates strong and emphasis marks that can be applied to text in paragraphs but not in headings.
SOURCE: https://github.com/prosemirror/website/blob/master/markdown/guide/schema.md#2025-04-23_snippet_2

LANGUAGE: javascript
CODE:
```
const markSchema = new Schema({
  nodes: {
    doc: {content: "block+"},
    paragraph: {group: "block", content: "text*", marks: "_"},
    heading: {group: "block", content: "text*", marks: ""},
    text: {inline: true}
  },
  marks: {
    strong: {},
    em: {}
  }
})
```

----------------------------------------

TITLE: Implementing toggleLink Command for Star Schema in ProseMirror
DESCRIPTION: This snippet defines a custom command for toggling links in the star schema. It checks if a link is being added or removed, prompts for a URL if adding, and then applies or removes the link mark accordingly.
SOURCE: https://github.com/prosemirror/website/blob/master/pages/examples/schema/index.md#2025-04-23_snippet_5

LANGUAGE: javascript
CODE:
```
function toggleLink(state, dispatch) {
  let {doc, selection} = state
  if (selection.empty) return false
  let attrs = null
  if (!doc.rangeHasMark(selection.from, selection.to, state.schema.marks.link)) {
    attrs = {href: prompt("Link to where?", "")}
    if (!attrs.href) return false
  }
  return toggleMark(state.schema.marks.link, attrs)(state, dispatch)
}
```

----------------------------------------

TITLE: Creating a Basic ProseMirror Plugin (JavaScript)
DESCRIPTION: Demonstrates how to create a simple ProseMirror plugin that adds event handling to the editor. The example creates a plugin that logs when keys are pressed and initializes an editor state with this plugin.
SOURCE: https://github.com/prosemirror/website/blob/master/markdown/guide/state.md#2025-04-23_snippet_3

LANGUAGE: javascript
CODE:
```
let myPlugin = new Plugin({
  props: {
    handleKeyDown(view, event) {
      console.log("A key was pressed!")
      return false // We did not handle this
    }
  }
})

let state = EditorState.create({schema, plugins: [myPlugin]})
```

----------------------------------------

TITLE: Initializing ProseMirror with Content from DOM
DESCRIPTION: Demonstrates how to initialize a ProseMirror editor with existing content by parsing DOM nodes. This uses the DOMParser mechanism to convert HTML content from an element with ID 'content' into a ProseMirror document structure based on the schema.
SOURCE: https://github.com/prosemirror/website/blob/master/markdown/guide/intro.md#2025-04-23_snippet_4

LANGUAGE: javascript
CODE:
```
import {DOMParser} from "prosemirror-model"
import {EditorState} from "prosemirror-state"
import {schema} from "prosemirror-schema-basic"

let content = document.getElementById("content")
let state = EditorState.create({
  doc: DOMParser.fromSchema(schema).parse(content)
})
```

----------------------------------------

TITLE: Setting CodeMirror Selection from ProseMirror
DESCRIPTION: This method handles setting the CodeMirror selection when ProseMirror tries to put the selection inside the node. It ensures the CodeMirror selection matches the position passed in.
SOURCE: https://github.com/prosemirror/website/blob/master/pages/examples/codemirror/index.md#2025-04-23_snippet_2

LANGUAGE: javascript
CODE:
```
PART(nodeview_setSelection)
```

----------------------------------------

TITLE: Defining TrackState for Change Tracking in ProseMirror
DESCRIPTION: This code snippet defines the TrackState class, which is used to track the commit history in a ProseMirror editor. It maintains an array of commits, each containing steps, inverses, and metadata.
SOURCE: https://github.com/prosemirror/website/blob/master/pages/examples/track/index.md#2025-04-23_snippet_0

LANGUAGE: javascript
CODE:
```
class TrackState {
  constructor(commits) {
    this.commits = commits
  }

  addCommit(message, time, steps, inverseSteps) {
    this.commits.push({
      message, time, steps,
      inverseSteps,
      hidden: false
    })
  }
}
```

----------------------------------------

TITLE: Implementing ProseMirror State Management with Redux-like Flow
DESCRIPTION: Example showing how to integrate ProseMirror's transaction system with a Redux-like state management architecture, including state updates and UI refresh logic.
SOURCE: https://github.com/prosemirror/website/blob/master/markdown/guide/view.md#2025-04-23_snippet_1

LANGUAGE: javascript
CODE:
```
let appState = {
  editor: EditorState.create({schema}),
  score: 0
}

let view = new EditorView(document.body, {
  state: appState.editor,
  dispatchTransaction(transaction) {
    update({type: "EDITOR_TRANSACTION", transaction})
  }
})

function update(event) {
  if (event.type == "EDITOR_TRANSACTION")
    appState.editor = appState.editor.apply(event.transaction)
  else if (event.type == "SCORE_POINT")
    appState.score++
  draw()
}

function draw() {
  document.querySelector("#score").textContent = appState.score
  view.updateState(appState.editor)
}
```

----------------------------------------

TITLE: Configuring Editor View Switching in JavaScript
DESCRIPTION: This code sets up radio buttons that allow users to switch between the raw markdown textarea view and the WYSIWYG ProseMirror view. It handles synchronizing content between the views when switching and initializes the editor with default content.
SOURCE: https://github.com/prosemirror/website/blob/master/pages/examples/markdown/index.md#2025-04-23_snippet_2

LANGUAGE: javascript
CODE:
```
let place = document.querySelector("#editor")
let view = new MarkdownView(place, "# Hello\n\nThis is a comment written in [Markdown](http://commonmark.org).\n\n* You can use formatting like **emphasis** and lists\n* You can add [links](http://example.com)\n* Even `inline code` and code blocks are supported\n\n        And that's about it\n")

function update(type) {
  if (view.constructor.name == type.name) return
  let content = view.content
  view.destroy()
  view = new type(place, content)
  view.focus()
}

for (let input of document.querySelectorAll("input[type=radio]")) {
  input.addEventListener("click", () => update(input.value == "markdown" ? MarkdownView : ProseMirrorView))
}
```

----------------------------------------

TITLE: Creating a Menu Plugin for ProseMirror in JavaScript
DESCRIPTION: This code defines a plugin that creates and manages a MenuView for a ProseMirror editor. The plugin handles initializing the MenuView, updating it when the editor state changes, and destroying it when the editor is destroyed.
SOURCE: https://github.com/prosemirror/website/blob/master/pages/examples/menu/index.md#2025-04-23_snippet_1

LANGUAGE: javascript
CODE:
```
function menuPlugin(items) {
  return new Plugin({
    view(editorView) {
      let menuView = new MenuView(items, editorView)
      editorView.dom.parentNode.insertBefore(menuView.dom, editorView.dom)
      return menuView
    }
  })
}
```

----------------------------------------

TITLE: Defining Basic Text Schema in ProseMirror
DESCRIPTION: This snippet defines a simple ProseMirror schema that allows only text content. It creates a document node that can contain one or more paragraph nodes, which in turn can contain text.
SOURCE: https://github.com/prosemirror/website/blob/master/pages/examples/schema/index.md#2025-04-23_snippet_0

LANGUAGE: javascript
CODE:
```
const textSchema = new Schema({
  nodes: {
    doc: {content: "paragraph+"},
    paragraph: {content: "text*", toDOM: () => ["p", 0]},
    text: {}
  }
})
```

----------------------------------------

TITLE: Updating CodeMirror Content from ProseMirror
DESCRIPTION: This method handles updates coming from ProseMirror, such as undo actions. It compares the old and new content to generate a minimal replacement for the changed range in the inner editor.
SOURCE: https://github.com/prosemirror/website/blob/master/pages/examples/codemirror/index.md#2025-04-23_snippet_4

LANGUAGE: javascript
CODE:
```
PART(nodeview_update)
```

----------------------------------------

TITLE: Implementing Commit Reversion in ProseMirror
DESCRIPTION: This function reverts a specific commit by rebasing the inverted form of its steps over all intermediate steps. It handles the complexities of moving changes across each other, which can sometimes lead to unintuitive results in complicated scenarios.
SOURCE: https://github.com/prosemirror/website/blob/master/pages/examples/track/index.md#2025-04-23_snippet_2

LANGUAGE: javascript
CODE:
```
function revertCommit(state, commit) {
  let trackState = trackPlugin.getState(state)
  let index = trackState.commits.indexOf(commit)
  let nextCommits = trackState.commits.slice(index + 1)
  let steps = commit.inverseSteps.slice()

  for (let i = index + 1; i < trackState.commits.length; i++) {
    let inv = trackState.commits[i].inverseSteps
    for (let j = inv.length - 1; j >= 0; j--)
      steps = Transform.mapStep(steps, inv[j])
    for (let j = 0; j < steps.length; j++)
      inv = Transform.mapStep(inv, steps[j])
  }

  let tr = state.tr
  for (let i = 0; i < steps.length; i++) tr.step(steps[i])
  return tr
}
```

----------------------------------------

TITLE: Adding Node Attributes in JavaScript
DESCRIPTION: Demonstrates how to define node attributes in a ProseMirror schema. This example adds a 'level' attribute to heading nodes with a default value of 1.
SOURCE: https://github.com/prosemirror/website/blob/master/markdown/guide/schema.md#2025-04-23_snippet_3

LANGUAGE: javascript
CODE:
```
  heading: {
    content: "text*",
    attrs: {level: {default: 1}}
  }
```

----------------------------------------

TITLE: Handling Inner Transactions in ProseMirror Footnote
DESCRIPTION: Implements the dispatchInner method to handle transactions in the footnote sub-editor. This method applies the changes from the inner editor to the outer document, handling appended transactions and avoiding infinite loops.
SOURCE: https://github.com/prosemirror/website/blob/master/pages/examples/footnote/index.md#2025-04-23_snippet_3

LANGUAGE: javascript
CODE:
```
dispatchInner(tr) {
  let {state, transactions} = this.innerView.state.applyTransaction(tr)
  this.innerView.updateState(state)

  if (!tr.getMeta("fromOutside")) {
    let outerTr = this.outerView.state.tr, offsetMap = StepMap.offset(this.getPos() + 1)
    for (let i = 0; i < transactions.length; i++) {
      let steps = transactions[i].steps
      for (let j = 0; j < steps.length; j++)
        outerTr.step(steps[j].map(offsetMap))
    }
    if (outerTr.docChanged) this.outerView.dispatch(outerTr)
  }
}
```

----------------------------------------

TITLE: Basic Image Node View Implementation in ProseMirror
DESCRIPTION: Creates a custom node view for image nodes with click event handling. Demonstrates basic node view creation and event management.
SOURCE: https://github.com/prosemirror/website/blob/master/markdown/guide/view.md#2025-04-23_snippet_6

LANGUAGE: javascript
CODE:
```
let view = new EditorView({
  state,
  nodeViews: {
    image(node) { return new ImageView(node) }
  }
})

class ImageView {
  constructor(node) {
    // The editor will use this as the node's DOM representation
    this.dom = document.createElement("img")
    this.dom.src = node.attrs.src
    this.dom.addEventListener("click", e => {
      console.log("You clicked me!")
      e.preventDefault()
    })
  }

  stopEvent() { return true }
}
```

----------------------------------------

TITLE: Initializing CodeMirror Node View in ProseMirror
DESCRIPTION: This snippet sets up the basic structure of a CodeMirror node view for ProseMirror. It creates a CodeMirror editor with custom extensions and key bindings, and sets up an update listener for synchronization.
SOURCE: https://github.com/prosemirror/website/blob/master/pages/examples/codemirror/index.md#2025-04-23_snippet_0

LANGUAGE: javascript
CODE:
```
PART(nodeview_start)
```

----------------------------------------

TITLE: Applying a ReplaceStep in JavaScript
DESCRIPTION: This snippet demonstrates how to create and apply a ReplaceStep to delete content between positions 3 and 5 in a document.
SOURCE: https://github.com/prosemirror/website/blob/master/markdown/guide/transform.md#2025-04-23_snippet_0

LANGUAGE: javascript
CODE:
```
console.log(myDoc.toString()) // → p("hello")
// A step that deletes the content between positions 3 and 5
let step = new ReplaceStep(3, 5, Slice.empty)
let result = step.apply(myDoc)
console.log(result.doc.toString()) // → p("heo")
```

----------------------------------------

TITLE: Synchronizing CodeMirror Updates to ProseMirror
DESCRIPTION: This function forwards updates from CodeMirror to ProseMirror when the code editor is focused. It translates document and selection changes into ProseMirror transactions.
SOURCE: https://github.com/prosemirror/website/blob/master/pages/examples/codemirror/index.md#2025-04-23_snippet_1

LANGUAGE: javascript
CODE:
```
PART(nodeview_forwardUpdate)
```

----------------------------------------

TITLE: Creating and Using a Transform in JavaScript
DESCRIPTION: This example shows how to create a Transform object, perform multiple operations (delete and split), and access the resulting document and step count.
SOURCE: https://github.com/prosemirror/website/blob/master/markdown/guide/transform.md#2025-04-23_snippet_1

LANGUAGE: javascript
CODE:
```
let tr = new Transform(myDoc)
tr.delete(5, 7) // Delete between position 5 and 7
tr.split(5)     // Split the parent node at position 5
console.log(tr.doc.toString()) // The modified document
console.log(tr.steps.length)   // → 2
```

----------------------------------------

TITLE: Implementing Fold Toggle Functionality in ProseMirror
DESCRIPTION: This function dispatches transactions to toggle the folded state of a section. It also ensures the selection is moved outside of a section when it's being folded to maintain usability.
SOURCE: https://github.com/prosemirror/website/blob/master/pages/examples/fold/index.md#2025-04-23_snippet_3

LANGUAGE: javascript
CODE:
```
PART(setFolding)
```

----------------------------------------

TITLE: Implementing a Tooltip Component for ProseMirror
DESCRIPTION: This snippet defines a Tooltip class that creates and manages a tooltip DOM element. It handles the tooltip's creation, positioning, and updates based on the editor's state changes. The positioning is calculated using ProseMirror's coordsAtPos method.
SOURCE: https://github.com/prosemirror/website/blob/master/pages/examples/tooltip/index.md#2025-04-23_snippet_1

LANGUAGE: JavaScript
CODE:
```
class Tooltip {
  constructor(view) {
    this.tooltip = document.createElement("div")
    this.tooltip.className = "tooltip"
    view.dom.parentNode.appendChild(this.tooltip)
    this.update(view, null)
  }

  update(view, lastState) {
    let state = view.state
    // Don't do anything if the document/selection didn't change
    if (lastState && lastState.doc.eq(state.doc) &&
        lastState.selection.eq(state.selection)) return

    // Hide the tooltip if the selection is empty
    if (state.selection.empty) {
      this.tooltip.style.display = "none"
      return
    }

    // Otherwise, reposition it and update its content
    this.tooltip.style.display = ""
    let {from, to} = state.selection
    // These are in screen coordinates
    let start = view.coordsAtPos(from), end = view.coordsAtPos(to)
    // The box in which the tooltip is positioned, to use as base
    let box = this.tooltip.offsetParent.getBoundingClientRect()
    // Find a center-ish x position from the selection endpoints (when
    // crossing lines, end may be more to the left than start, so take
    // the min of their left sides and the max of their right sides).
    let left = Math.max((start.left + end.left) / 2, start.left, end.left)
    left = (left - box.left - this.tooltip.offsetWidth / 2) + "px"
    this.tooltip.style.left = left
    this.tooltip.style.bottom = (box.bottom - start.top) + "px"
    this.tooltip.textContent =
      state.doc.textBetween(from, to, " ")
  }

  destroy() { this.tooltip.remove() }
}
```

----------------------------------------

TITLE: Creating a MenuView Component for ProseMirror in JavaScript
DESCRIPTION: This code creates a MenuView class that renders menu items in a menu bar and handles click events to trigger the corresponding commands. It also manages updating which menu items are visible based on their applicability to the current editor state.
SOURCE: https://github.com/prosemirror/website/blob/master/pages/examples/menu/index.md#2025-04-23_snippet_0

LANGUAGE: javascript
CODE:
```
class MenuView {
  constructor(items, editorView) {
    this.items = items
    this.editorView = editorView

    this.dom = document.createElement("div")
    this.dom.className = "menubar"
    items.forEach(({dom}) => this.dom.appendChild(dom))
    this.update()

    this.dom.addEventListener("mousedown", e => {
      e.preventDefault()
      editorView.focus()
      items.forEach(({command, dom}) => {
        if (dom.contains(e.target))
          command(editorView.state, editorView.dispatch, editorView)
      })
    })
  }

  update() {
    this.items.forEach(({command, dom}) => {
      let active = command(this.editorView.state, null, this.editorView)
      dom.style.display = active ? "" : "none"
    })
  }

  destroy() { this.dom.remove() }
}
```

----------------------------------------

TITLE: Handling File Input Events for Image Upload in ProseMirror
DESCRIPTION: Event handler for file input changes that initiates image uploads. It performs validation checks on selected files before starting the upload process.
SOURCE: https://github.com/prosemirror/website/blob/master/pages/examples/upload/index.md#2025-04-23_snippet_2

LANGUAGE: javascript
CODE:
```
document.querySelector("#image-upload").addEventListener("change", e => {
  if (view.isDestroyed) return
  let file = e.target.files[0]
  if (!file) return
  if (!file.type.startsWith("image/")) return
  if (file.size < 20 || file.size > 10000000) return

  startImageUpload(file, view, view.state.selection.from)
})
```

----------------------------------------

TITLE: Implementing Textarea-based Markdown View in JavaScript
DESCRIPTION: This code defines a MarkdownView class that wraps a textarea element for editing markdown content. It provides methods to update the content, get the current content, and focus the editor.
SOURCE: https://github.com/prosemirror/website/blob/master/pages/examples/markdown/index.md#2025-04-23_snippet_0

LANGUAGE: javascript
CODE:
```
class MarkdownView {
  constructor(target, content) {
    this.textarea = target.appendChild(document.createElement("textarea"))
    this.textarea.value = content
  }

  get content() { return this.textarea.value }
  focus() { this.textarea.focus() }
  destroy() { this.textarea.remove() }

  // :: (string) → bool
  // Update the editor's content, and return a boolean that indicates
  // whether anything changed.
  update(content) {
    if (this.textarea.value != content) {
      this.textarea.value = content
      return true
    }
    return false
  }
}
```

----------------------------------------

TITLE: Defining Note Schema with Groups in ProseMirror
DESCRIPTION: This snippet creates a more complex schema with notes that can be optionally grouped. It defines custom DOM elements for notes and note groups, and includes toDOM and parseDOM methods for proper rendering and parsing.
SOURCE: https://github.com/prosemirror/website/blob/master/pages/examples/schema/index.md#2025-04-23_snippet_1

LANGUAGE: javascript
CODE:
```
const noteSchema = new Schema({
  nodes: {
    doc: {content: "(note | notegroup)+"},
    notegroup: {
      content: "note+",
      toDOM: () => ["notegroup", 0],
      parseDOM: [{tag: "notegroup"}]
    },
    note: {
      content: "text*",
      toDOM: () => ["note", 0],
      parseDOM: [{tag: "note"}]
    },
    text: {}
  }
})
```

----------------------------------------

TITLE: Implementing Command with View Access in JavaScript
DESCRIPTION: A command that interacts with the editor's DOM. This example demonstrates how commands can use the optional third parameter (view) to perform actions that require access to the DOM, such as visual effects or positioning dialogs.
SOURCE: https://github.com/prosemirror/website/blob/master/markdown/guide/commands.md#2025-04-23_snippet_2

LANGUAGE: javascript
CODE:
```
function blinkView(_state, dispatch, view) {
  if (dispatch) {
    view.dom.style.background = "yellow"
    setTimeout(() => view.dom.style.background = "", 1000)
  }
  return true
}
```

----------------------------------------

TITLE: Updating Footnote Node View in ProseMirror
DESCRIPTION: Implements the update method for the footnote node view. This method handles updates from the outer editor, carefully finding and applying differences to maintain cursor position when possible.
SOURCE: https://github.com/prosemirror/website/blob/master/pages/examples/footnote/index.md#2025-04-23_snippet_4

LANGUAGE: javascript
CODE:
```
update(node) {
  if (!node.sameMarkup(this.node)) return false
  this.node = node
  if (this.innerView) {
    let state = this.innerView.state
    let start = node.content.findDiffStart(state.doc.content)
    if (start != null) {
      let {a: endA, b: endB} = node.content.findDiffEnd(state.doc.content)
      let overlap = start - Math.min(endA, endB)
      if (overlap > 0) { endA += overlap; endB += overlap }
      this.innerView.dispatch(
        state.tr
          .replace(start, endB, node.slice(start, endA))
          .setMeta("fromOutside", true))
    }
  }
  return true
}
```

----------------------------------------

TITLE: Creating a Folding Plugin for ProseMirror
DESCRIPTION: This code defines a plugin that tracks and manages folding decorations. It maintains the fold state across editor updates, handles transactions with folding metadata, and installs the foldable node view.
SOURCE: https://github.com/prosemirror/website/blob/master/pages/examples/fold/index.md#2025-04-23_snippet_2

LANGUAGE: javascript
CODE:
```
PART(plugin)
```

----------------------------------------

TITLE: Creating Visual Decorations for ProseMirror Linter
DESCRIPTION: This function generates decorations for highlighting detected problems in the document. It creates a gutter icon and applies a background decoration to the problematic text, with styles applied via CSS.
SOURCE: https://github.com/prosemirror/website/blob/master/pages/examples/lint/index.md#2025-04-23_snippet_2

LANGUAGE: javascript
CODE:
```
function diagnosticToDecorations(diagnostic) {
  let result = []
  diagnostic.forEach(prob => {
    let from = prob.from, to = prob.to
    let widget = document.createElement("div")
    widget.className = "lint-icon"
    widget.problem = prob
    widget.title = prob.message
    result.push(Decoration.widget({widget, side: 1}).range(from))
    if (from != to)
      result.push(Decoration.inline(from, to, {class: "lint-annotation"}))
  })
  return result
}
```

----------------------------------------

TITLE: Mapping Positions with StepMap in JavaScript
DESCRIPTION: This snippet illustrates how to create a StepMap from a ReplaceStep and use it to map positions from the old document to the new one.
SOURCE: https://github.com/prosemirror/website/blob/master/markdown/guide/transform.md#2025-04-23_snippet_2

LANGUAGE: javascript
CODE:
```
let step = new ReplaceStep(4, 6, Slice.empty) // Delete 4-5
let map = step.getMap()
console.log(map.map(8)) // → 6
console.log(map.map(2)) // → 2 (nothing changes before the change)
```

----------------------------------------

TITLE: Creating ProseMirror Linter Plugin
DESCRIPTION: A plugin implementation that integrates linting into ProseMirror. It tracks document changes, updates decorations, and handles icon click events to select issues or apply fixes when double-clicked.
SOURCE: https://github.com/prosemirror/website/blob/master/pages/examples/lint/index.md#2025-04-23_snippet_3

LANGUAGE: javascript
CODE:
```
class LintPlugin {
  constructor(view) {
    this.view = view
    this.problems = lint(view.state.doc)
    this.decorations = DecorationSet.create(view.state.doc, diagnosticToDecorations(this.problems))

    this.update(view, null)
  }

  update(view, oldState) {
    if (oldState && oldState.doc.eq(view.state.doc)) return false
    this.problems = lint(view.state.doc)
    this.decorations = DecorationSet.create(view.state.doc, diagnosticToDecorations(this.problems))
    return true
  }
}

function handleClick(view, pos, event) {
  if (!event.target.problem) return false
  let prob = event.target.problem
  if (event.type == "dblclick") {
    if (prob.fix) prob.fix(view)
    return true
  }
  view.dispatch(view.state.tr.setSelection(TextSelection.create(view.state.doc, prob.from, prob.to)))
  return true
}

let lintPlugin = new Plugin({
  key: new PluginKey("lint"),
  state: {
    init(_, {doc}) { return {doc, problems: lint(doc), decorations: []} },
    apply(tr, prev, oldState, state) {
      if (tr.docChanged) {
        let problems = lint(tr.doc)
        return {doc: tr.doc, problems, decorations: diagnosticToDecorations(problems)}
      }
      return prev
    }
  },
  props: {
    decorations(state) {
      return this.getState(state).decorations
    },
    handleClick
  }
})
```

----------------------------------------

TITLE: Handling Cursor Movement from ProseMirror to CodeMirror
DESCRIPTION: This function creates arrow key handlers for the outer editor to manage cursor movement from ProseMirror into the CodeMirror editor when the cursor is at the end of a textblock adjacent to a code block.
SOURCE: https://github.com/prosemirror/website/blob/master/pages/examples/codemirror/index.md#2025-04-23_snippet_6

LANGUAGE: javascript
CODE:
```
PART(arrowHandlers)
```

----------------------------------------

TITLE: Setting Up ProseMirror Editor with Footnotes
DESCRIPTION: Creates a ProseMirror editor instance with the custom footnote schema and node view. This setup enables footnote functionality in the editor.
SOURCE: https://github.com/prosemirror/website/blob/master/pages/examples/footnote/index.md#2025-04-23_snippet_6

LANGUAGE: javascript
CODE:
```
window.view = new EditorView(document.querySelector("#editor"), {
  state: EditorState.create({schema, plugins: [exampleSetup({schema})]}),
  nodeViews: {
    footnote(node, view, getPos) { return new FootnoteView(node, view, getPos) }
  }
})
```

----------------------------------------

TITLE: Implementing Asynchronous Image Upload in ProseMirror
DESCRIPTION: Function that handles the complete image upload process, from creating a placeholder to replacing it with the actual image when upload completes. It manages document transactions to maintain placeholder positions.
SOURCE: https://github.com/prosemirror/website/blob/master/pages/examples/upload/index.md#2025-04-23_snippet_3

LANGUAGE: javascript
CODE:
```
function startImageUpload(file, view, pos) {
  // A fresh object to act as the ID for this upload
  let id = {}

  // Replace the selection with a placeholder
  let tr = view.state.tr
  if (!tr.selection.empty) tr.deleteSelection()
  tr.setMeta(placeholderPlugin, {add: {id, pos}})
  view.dispatch(tr)

  uploadFile(file).then(url => {
    let pos = findPlaceholder(view.state, id)
    // If the content around the placeholder has been deleted, drop
    // the image
    if (pos == null) return
    // Otherwise, insert it at the placeholder's position, and remove
    // the placeholder
    view.dispatch(view.state.tr
                 .replaceWith(pos, pos, schema.nodes.image.create({src: url}))
                 .setMeta(placeholderPlugin, {remove: {id}}))
  }, () => {
    // On failure, just clean up the placeholder
    view.dispatch(view.state.tr.setMeta(placeholderPlugin, {remove: {id}}))
  })
}
```

----------------------------------------

TITLE: Implementing Basic Command: Delete Selection in JavaScript
DESCRIPTION: A simple ProseMirror command that deletes the current selection. The command checks if a selection exists, and if so, dispatches a transaction to delete it. It demonstrates the basic command pattern of checking applicability and performing an action.
SOURCE: https://github.com/prosemirror/website/blob/master/markdown/guide/commands.md#2025-04-23_snippet_0

LANGUAGE: javascript
CODE:
```
function deleteSelection(state, dispatch) {
  if (state.selection.empty) return false
  dispatch(state.tr.deleteSelection())
  return true
}
```

----------------------------------------

TITLE: Creating a Placeholder Decoration Plugin in ProseMirror
DESCRIPTION: A plugin that manages widget decorations representing image upload placeholders. It maintains a decoration set and provides methods to add and remove decorations by ID.
SOURCE: https://github.com/prosemirror/website/blob/master/pages/examples/upload/index.md#2025-04-23_snippet_0

LANGUAGE: javascript
CODE:
```
function placeholderPlugin() {
  return new Plugin({
    state: {
      init() { return DecorationSet.empty },
      apply(tr, set) {
        // Adjust decoration positions to changes made by the transaction
        set = set.map(tr.mapping, tr.doc)
        // See if the transaction adds or removes any placeholders
        let action = tr.getMeta(this)
        if (action && action.add) {
          let widget = document.createElement("placeholder")
          let deco = Decoration.widget(action.add.pos, widget, {id: action.add.id})
          set = set.add(tr.doc, [deco])
        } else if (action && action.remove) {
          set = set.remove(set.find(null, null,
                                      spec => spec.id == action.remove.id))
        }
        return set
      }
    },
    props: {
      decorations(state) { return this.getState(state) }
    }
  })
}
```

----------------------------------------

TITLE: Interactive Image Node View with Alt Text Editing in ProseMirror
DESCRIPTION: Extends the image node view to allow alt text editing through click interactions. Shows how to update node attributes using transactions.
SOURCE: https://github.com/prosemirror/website/blob/master/markdown/guide/view.md#2025-04-23_snippet_7

LANGUAGE: javascript
CODE:
```
let view = new EditorView({
  state,
  nodeViews: {
    image(node, view, getPos) { return new ImageView(node, view, getPos) }
  }
})

class ImageView {
  constructor(node, view, getPos) {
    this.dom = document.createElement("img")
    this.dom.src = node.attrs.src
    this.dom.alt = node.attrs.alt
    this.dom.addEventListener("click", e => {
      e.preventDefault()
      let alt = prompt("New alt text:", "")
      if (alt) view.dispatch(view.state.tr.setNodeMarkup(getPos(), null, {
        src: node.attrs.src,
        alt
      }))
    })
  }

  stopEvent() { return true }
}
```

----------------------------------------

TITLE: Configuring ProseMirror Editor View Props
DESCRIPTION: Demonstration of setting up ProseMirror editor view props, including read-only behavior and event handling configuration.
SOURCE: https://github.com/prosemirror/website/blob/master/markdown/guide/view.md#2025-04-23_snippet_2

LANGUAGE: javascript
CODE:
```
let view = new EditorView({
  state: myState,
  editable() { return false }, // Enables read-only behavior
  handleDoubleClick() { console.log("Double click!") }
})
```

----------------------------------------

TITLE: Creating a ProseMirror Plugin with State Management (JavaScript)
DESCRIPTION: Shows how to create a plugin that maintains its own state within the editor state. This example implements a transaction counter plugin that increments its value each time a transaction is applied.
SOURCE: https://github.com/prosemirror/website/blob/master/markdown/guide/state.md#2025-04-23_snippet_4

LANGUAGE: javascript
CODE:
```
let transactionCounter = new Plugin({
  state: {
    init() { return 0 },
    apply(tr, value) { return value + 1 }
  }
})

function getTransactionCount(state) {
  return transactionCounter.getState(state)
}
```

----------------------------------------

TITLE: Creating a ProseMirror Plugin for Tooltip Management
DESCRIPTION: This snippet defines a ProseMirror plugin that manages the lifecycle of a tooltip. It creates a plugin view that handles the creation, updating, and destruction of the tooltip based on the editor's state.
SOURCE: https://github.com/prosemirror/website/blob/master/pages/examples/tooltip/index.md#2025-04-23_snippet_0

LANGUAGE: JavaScript
CODE:
```
new Plugin({
  view(editorView) { return new Tooltip(editorView) }
})
```

----------------------------------------

TITLE: Creating Schema with Section Nodes in ProseMirror
DESCRIPTION: This code defines a modified schema that structures content as sections, each containing a heading followed by arbitrary blocks. It extends the basic schema to support the folding functionality.
SOURCE: https://github.com/prosemirror/website/blob/master/pages/examples/fold/index.md#2025-04-23_snippet_0

LANGUAGE: javascript
CODE:
```
PART(schema)
```

----------------------------------------

TITLE: Finalizing CodeMirror Node View Setup
DESCRIPTION: This snippet completes the setup of the CodeMirror node view, including methods for destroying the view and ignoring mutations.
SOURCE: https://github.com/prosemirror/website/blob/master/pages/examples/codemirror/index.md#2025-04-23_snippet_5

LANGUAGE: javascript
CODE:
```
PART(nodeview_end)
```

----------------------------------------

TITLE: Defining Footnote Schema in ProseMirror
DESCRIPTION: Defines the schema for footnote nodes in ProseMirror. Footnotes are implemented as inline nodes with content, marked as atoms for special handling.
SOURCE: https://github.com/prosemirror/website/blob/master/pages/examples/footnote/index.md#2025-04-23_snippet_0

LANGUAGE: javascript
CODE:
```
schema: {
  nodes: {
    doc: {content: "block+"},
    paragraph: {group: "block", content: "inline*"},
    text: {group: "inline"},
    footnote: {
      inline: true,
      group: "inline",
      content: "inline*",
      atom: true,
      toDOM: () => ["footnote", 0],
      parseDOM: [{tag: "footnote"}]
    }
  }
}
```

----------------------------------------

TITLE: Finalizing Footnote Node View in ProseMirror
DESCRIPTION: Implements the destroy method and event handling for the footnote node view. This includes cleanup on destruction and defining which events should be handled by the outer editor.
SOURCE: https://github.com/prosemirror/website/blob/master/pages/examples/footnote/index.md#2025-04-23_snippet_5

LANGUAGE: javascript
CODE:
```
destroy() {
  if (this.innerView) this.closeInner()
}

stopEvent(event) {
  return this.innerView && this.innerView.dom.contains(event.target)
}

ignoredClick(event) {
  return this.innerView && this.innerView.dom.contains(event.target)
}
```

----------------------------------------

TITLE: Initializing Footnote Node View in ProseMirror
DESCRIPTION: Sets up the basic structure for a custom node view to handle footnotes. This includes creating DOM elements and initializing variables for the node view.
SOURCE: https://github.com/prosemirror/website/blob/master/pages/examples/footnote/index.md#2025-04-23_snippet_1

LANGUAGE: javascript
CODE:
```
class FootnoteView {
  constructor(node, view, getPos) {
    this.node = node
    this.outerView = view
    this.getPos = getPos

    this.dom = document.createElement("footnote")
    this.open = false
    this.innerView = null
  }

  selectNode() {
    this.dom.classList.add("ProseMirror-selectednode")
    if (!this.open) this.open = this.openInner()
  }

  deselectNode() {
    this.dom.classList.remove("ProseMirror-selectednode")
    if (this.open) {
      this.closeInner()
      this.open = false
    }
  }
```

----------------------------------------

TITLE: Creating a ProseMirror Plugin for Change Tracking
DESCRIPTION: This snippet defines a ProseMirror plugin that observes transactions and updates the TrackState. It processes commit transactions by extracting the commit message from the transaction's meta property.
SOURCE: https://github.com/prosemirror/website/blob/master/pages/examples/track/index.md#2025-04-23_snippet_1

LANGUAGE: javascript
CODE:
```
const trackPlugin = new Plugin({
  state: {
    init() { return new TrackState([]) },
    apply(tr, trackState) {
      let newState = trackState
      let commitMessage = tr.getMeta(trackPlugin)
      if (commitMessage) {
        let startState = tr.startState
        let steps = tr.steps.slice()
        newState = new TrackState(trackState.commits.slice())
        newState.addCommit(commitMessage, Date.now(), steps,
                          invertedSteps(steps, startState.doc, tr.doc))
      }
      return newState
    }
  }
})
```

----------------------------------------

TITLE: Creating Document Slices in JavaScript with ProseMirror
DESCRIPTION: This JavaScript code demonstrates how to create slices from a ProseMirror document using the slice method. It shows how to extract different parts of the document and check their open depths.
SOURCE: https://github.com/prosemirror/website/blob/master/markdown/guide/doc.md#2025-04-23_snippet_3

LANGUAGE: javascript
CODE:
```
// doc holds two paragraphs, containing text "a" and "b"
let slice1 = doc.slice(0, 3) // The first paragraph
console.log(slice1.openStart, slice1.openEnd) // → 0 0
let slice2 = doc.slice(1, 5) // From start of first paragraph
                             // to end of second
console.log(slice2.openStart, slice2.openEnd) // → 1 1
```

----------------------------------------

TITLE: Custom Paragraph Node View with Empty State Styling in ProseMirror
DESCRIPTION: Implements a paragraph node view that adds special styling for empty paragraphs. Demonstrates content DOM handling and node updates.
SOURCE: https://github.com/prosemirror/website/blob/master/markdown/guide/view.md#2025-04-23_snippet_8

LANGUAGE: javascript
CODE:
```
let view = new EditorView({
  state,
  nodeViews: {
    paragraph(node) { return new ParagraphView(node) }
  }
})

class ParagraphView {
  constructor(node) {
    this.dom = this.contentDOM = document.createElement("p")
    if (node.content.size == 0) this.dom.classList.add("empty")
  }

  update(node) {
    if (node.content.size > 0) this.dom.classList.remove("empty")
    else this.dom.classList.add("empty")
    return true
  }
}
```

----------------------------------------

TITLE: Demonstrating HTML Document Structure in ProseMirror
DESCRIPTION: This HTML snippet illustrates the structure of a simple document in ProseMirror, showing how paragraphs, blockquotes, and images are represented.
SOURCE: https://github.com/prosemirror/website/blob/master/markdown/guide/doc.md#2025-04-23_snippet_2

LANGUAGE: html
CODE:
```
<p>One</p>
<blockquote><p>Two<img src="..."></p></blockquote>
```

----------------------------------------

TITLE: Setting up ProseMirror Editor with Custom Dinosaur Functionality
DESCRIPTION: This snippet creates the ProseMirror editor state and view with the custom dinosaur schema and menu items. It uses the example setup and extends it with the custom functionality.
SOURCE: https://github.com/prosemirror/website/blob/master/pages/examples/dino/index.md#2025-04-23_snippet_4

LANGUAGE: javascript
CODE:
```
import {exampleSetup, buildMenuItems} from "prosemirror-example-setup"
import {EditorState} from "prosemirror-state"
import {EditorView} from "prosemirror-view"

let menu = buildMenuItems(dinoSchema)
menu.insertMenu.content.push(dinoItem)
menu.insertMenu.content.push(dinoItems)

window.view = new EditorView(document.querySelector("#editor"), {
  state: EditorState.create({
    doc: doc,
    plugins: exampleSetup({schema: dinoSchema, menuContent: menu.fullMenu})
  })
})
```

----------------------------------------

TITLE: Defining Custom Dinosaur Node Spec in JavaScript
DESCRIPTION: This snippet defines a custom node spec for dinosaurs in ProseMirror. It specifies the node's attributes, inline status, group, and how it should be rendered in the DOM.
SOURCE: https://github.com/prosemirror/website/blob/master/pages/examples/dino/index.md#2025-04-23_snippet_0

LANGUAGE: javascript
CODE:
```
const dinoNodeSpec = {
  attrs: {type: {default: "brontosaurus"}},
  inline: true,
  group: "inline",
  draggable: true,

  toDOM: node => ["img", {
    "dino-type": node.attrs.type,
    src: "/img/dino/" + node.attrs.type + ".png",
    title: node.attrs.type,
    class: "dinosaur"
  }],
  parseDOM: [{
    tag: "img[dino-type]",
    getAttrs: dom => {
      let type = dom.getAttribute("dino-type")
      return type ? {type} : false
    }
  }]
}
```

----------------------------------------

TITLE: Implementing makeNoteGroup Command in ProseMirror
DESCRIPTION: This snippet defines a custom editing command that creates a note group around selected notes. It checks if the selection is valid for grouping and then wraps the selected notes in a new notegroup node.
SOURCE: https://github.com/prosemirror/website/blob/master/pages/examples/schema/index.md#2025-04-23_snippet_2

LANGUAGE: javascript
CODE:
```
function makeNoteGroup(state, dispatch) {
  let range = state.selection
  let from = range.$from.start(1), to = range.$to.end(1)
  let applicable = false
  state.doc.nodesBetween(from, to, (node, pos) => {
    if (!node.isTextblock) return false
    if (!applicable && node.type.name == "note") applicable = true
  })
  if (!applicable) return false
  if (dispatch)
    dispatch(state.tr.wrap(range, [{type: state.schema.nodes.notegroup}]))
  return true
}
```

----------------------------------------

TITLE: Importing ProseMirror State Module - CommonJS
DESCRIPTION: Demonstrates how to import and initialize the EditorState module using CommonJS require syntax. Creates a new editor state instance with a schema configuration.
SOURCE: https://github.com/prosemirror/website/blob/master/markdown/ref_intro.md#2025-04-23_snippet_0

LANGUAGE: javascript
CODE:
```
var EditorState = require("prosemirror-state").EditorState
var state = EditorState.create({schema: mySchema})
```

----------------------------------------

TITLE: Creating Custom Schema with Dinosaur Node in JavaScript
DESCRIPTION: This snippet extends the basic schema with the custom dinosaur node. It then uses this schema to parse HTML content into a ProseMirror document.
SOURCE: https://github.com/prosemirror/website/blob/master/pages/examples/dino/index.md#2025-04-23_snippet_1

LANGUAGE: javascript
CODE:
```
import {Schema} from "prosemirror-model"
import {schema} from "prosemirror-schema-basic"

const dinoSchema = new Schema({
  nodes: schema.spec.nodes.addBefore("image", "dino", dinoNodeSpec),
  marks: schema.spec.marks
})

let content = document.querySelector("#content")
let doc = DOMParser.fromSchema(dinoSchema).parse(content)
```

----------------------------------------

TITLE: Importing ProseMirror State Module - ES6
DESCRIPTION: Shows how to import and initialize the EditorState module using ES6 import syntax. Creates a new editor state instance with a schema configuration.
SOURCE: https://github.com/prosemirror/website/blob/master/markdown/ref_intro.md#2025-04-23_snippet_1

LANGUAGE: javascript
CODE:
```
import {EditorState} from "prosemirror-state"
let state = EditorState.create({schema: mySchema})
```

----------------------------------------

TITLE: Implementing Foldable Node View in ProseMirror
DESCRIPTION: This code creates a node view that displays sections with a foldable interface. It shows a header with a button that toggles folding, and responds to decorations with the 'foldSection' property to hide or show content.
SOURCE: https://github.com/prosemirror/website/blob/master/pages/examples/fold/index.md#2025-04-23_snippet_1

LANGUAGE: javascript
CODE:
```
PART(nodeview)
```

----------------------------------------

TITLE: Handling Key Events in CodeMirror Node View
DESCRIPTION: This keymap handles cursor movement across the edges of the inner editor, as well as undo, redo, and ctrl-enter actions. It allows the user to move the selection out of the code editor when appropriate.
SOURCE: https://github.com/prosemirror/website/blob/master/pages/examples/codemirror/index.md#2025-04-23_snippet_3

LANGUAGE: javascript
CODE:
```
PART(nodeview_keymap)
```

----------------------------------------

TITLE: Defining Node DOM Representation in ProseMirror Schema
DESCRIPTION: Example schema definition with toDOM field for a paragraph node. The toDOM function returns an array describing how to render the node as a p element with a hole (0) for its content.
SOURCE: https://github.com/prosemirror/website/blob/master/markdown/guide/schema.md#2025-04-23_snippet_4

LANGUAGE: javascript
CODE:
```
const schema = new Schema({
  nodes: {
    doc: {content: "paragraph+"},
    paragraph: {
      content: "text*",
      toDOM(node) { return ["p", 0] }
    },
    text: {}
  }
})
```

----------------------------------------

TITLE: Styling ProseMirror Box Components with CSS
DESCRIPTION: CSS styles defining the appearance of box components used in the ProseMirror editor interface, including color, display, border-radius, padding and margin properties.
SOURCE: https://github.com/prosemirror/website/blob/master/markdown/guide/view.md#2025-04-23_snippet_0

LANGUAGE: css
CODE:
```
.box {
  color: white;
  display: inline-block;
  border-radius: 5px;
  padding: 6px 10px;
  margin: 3px 0;
  vertical-align: top;
}
```

----------------------------------------

TITLE: Implementing Dinosaur Insertion Command in JavaScript
DESCRIPTION: This snippet defines a command for inserting dinosaurs into the document. It creates a new node of type 'dino' with the specified dinosaur type and inserts it at the current selection.
SOURCE: https://github.com/prosemirror/website/blob/master/pages/examples/dino/index.md#2025-04-23_snippet_2

LANGUAGE: javascript
CODE:
```
function insertDino(type) {
  return function(state, dispatch) {
    let {$from} = state.selection, index = $from.index()
    if (!$from.parent.canReplaceWith(index, index, dinoSchema.nodes.dino))
      return false
    if (dispatch)
      dispatch(state.tr.replaceSelectionWith(dinoSchema.nodes.dino.create({type})))
    return true
  }
}
```

----------------------------------------

TITLE: HTML Example of DOM Tree Structure
DESCRIPTION: An HTML example showing how a paragraph with markup is represented as a tree in the DOM, with nested elements for strong and emphasized text.
SOURCE: https://github.com/prosemirror/website/blob/master/markdown/guide/doc.md#2025-04-23_snippet_0

LANGUAGE: html
CODE:
```
<p>This is <strong>strong text with <em>emphasis</em></strong></p>
```

----------------------------------------

TITLE: Configuring Star Schema Keymap in ProseMirror
DESCRIPTION: This snippet defines a custom keymap for the star schema. It includes commands for toggling the 'shouting' mark, inserting a star node, and toggling links with a URL prompt.
SOURCE: https://github.com/prosemirror/website/blob/master/pages/examples/schema/index.md#2025-04-23_snippet_4

LANGUAGE: javascript
CODE:
```
const starKeymap = keymap({
  "Mod-b": toggleMark(schema.marks.shouting),
  "Mod-l": toggleLink,
  "Mod-Space": insertStar
})
```

----------------------------------------

TITLE: Implementing Purple Text Decoration Plugin in ProseMirror
DESCRIPTION: Creates a ProseMirror plugin that applies purple color styling to all text in the document using inline decorations. Demonstrates basic decoration creation and application.
SOURCE: https://github.com/prosemirror/website/blob/master/markdown/guide/view.md#2025-04-23_snippet_4

LANGUAGE: javascript
CODE:
```
let purplePlugin = new Plugin({
  props: {
    decorations(state) {
      return DecorationSet.create(state.doc, [
        Decoration.inline(0, state.doc.content.size, {style: "color: purple"})
      ])
    }
  }
})
```

----------------------------------------

TITLE: Implementing Fix Utilities for ProseMirror Linter
DESCRIPTION: Helper functions that create fix commands for document linting issues. These include functions to delete ranges of text, set attributes, and insert nodes, which are used by the linter to automatically correct detected problems.
SOURCE: https://github.com/prosemirror/website/blob/master/pages/examples/lint/index.md#2025-04-23_snippet_1

LANGUAGE: javascript
CODE:
```
function deleteRange(from, to) {
  return view => {
    let tr = view.state.tr.delete(from, to)
    view.dispatch(tr)
  }
}

function addAlt(pos) {
  return view => {
    let alt = prompt("Alt text", "")
    if (alt)
      view.dispatch(view.state.tr.setNodeMarkup(pos, null, {alt, src: view.state.doc.nodeAt(pos).attrs.src}))
  }
}
```

----------------------------------------

TITLE: Creating Speckled Background Decoration Plugin in ProseMirror
DESCRIPTION: Implements a plugin that adds yellow background decorations at regular intervals throughout the document. Shows how to maintain decoration state and map it through document changes.
SOURCE: https://github.com/prosemirror/website/blob/master/markdown/guide/view.md#2025-04-23_snippet_5

LANGUAGE: javascript
CODE:
```
let specklePlugin = new Plugin({
  state: {
    init(_, {doc}) {
      let speckles = []
      for (let pos = 1; pos < doc.content.size; pos += 4)
        speckles.push(Decoration.inline(pos - 1, pos, {style: "background: yellow"}))
      return DecorationSet.create(doc, speckles)
    },
    apply(tr, set) { return set.map(tr.mapping, tr.doc) }
  },
  props: {
    decorations(state) { return specklePlugin.getState(state) }
  }
})
```

----------------------------------------

TITLE: Creating Dinosaur Menu Items in JavaScript
DESCRIPTION: This snippet creates menu items for inserting different types of dinosaurs. It uses the insertDino command and defines icons and labels for each dinosaur type.
SOURCE: https://github.com/prosemirror/website/blob/master/pages/examples/dino/index.md#2025-04-23_snippet_3

LANGUAGE: javascript
CODE:
```
import {MenuItem} from "prosemirror-menu"

const dinoItem = new MenuItem({
  title: "Insert dinosaur",
  label: "Dino",
  enable(state) { return insertDino("brontosaurus")(state) },
  run: insertDino("brontosaurus")
})

const dinoItems = new MenuItemGroup([
  new MenuItem({title: "Brontosaurus", label: "🦕", select: "dino", attrs: {type: "brontosaurus"}}),
  new MenuItem({title: "Stegosaurus", label: "🦖", select: "dino", attrs: {type: "stegosaurus"}}),
  new MenuItem({title: "Tyrannosaurus", label: "🦖", select: "dino", attrs: {type: "tyrannosaurus"}}),
  new MenuItem({title: "Pterodactyl", label: "🦅", select: "dino", attrs: {type: "pterodactyl"}})
])
```

----------------------------------------

TITLE: Using Transform Mapping in JavaScript
DESCRIPTION: This example demonstrates how to use the mapping accumulated by a Transform object to map positions through multiple steps, including the use of bias.
SOURCE: https://github.com/prosemirror/website/blob/master/markdown/guide/transform.md#2025-04-23_snippet_3

LANGUAGE: javascript
CODE:
```
let tr = new Transform(myDoc)
tr.split(10)    // split a node, +2 tokens at 10
tr.delete(2, 5) // -3 tokens at 2
console.log(tr.mapping.map(15)) // → 14
console.log(tr.mapping.map(6))  // → 3
console.log(tr.mapping.map(10)) // → 9

console.log(tr.mapping.map(10, -1)) // → 7
```

----------------------------------------

TITLE: Opening Footnote Sub-Editor in ProseMirror
DESCRIPTION: Implements the openInner method to create a sub-editor for the footnote content. This includes setting up DOM elements, creating a new EditorView, and handling key bindings.
SOURCE: https://github.com/prosemirror/website/blob/master/pages/examples/footnote/index.md#2025-04-23_snippet_2

LANGUAGE: javascript
CODE:
```
openInner() {
  let tooltip = this.dom.appendChild(document.createElement("div"))
  tooltip.className = "footnote-tooltip"
  this.innerView = new EditorView(tooltip, {
    state: EditorState.create({
      doc: this.node.content,
      plugins: [keymap({
        "Mod-z": () => undo(this.outerView.state, this.outerView.dispatch),
        "Mod-y": () => redo(this.outerView.state, this.outerView.dispatch)
      })]
    }),
    dispatchTransaction: this.dispatchInner.bind(this),
    handleDOMEvents: {
      mousedown: () => {
        if (this.outerView.hasFocus()) this.innerView.focus()
      }
    }
  })
  tooltip.addEventListener("mousedown", e => e.preventDefault())
  return () => tooltip.remove()
}
```

----------------------------------------

TITLE: Creating a ProseMirror Plugin with Size Limit
DESCRIPTION: Example of creating a ProseMirror plugin that implements a maximum size limit through the props system.
SOURCE: https://github.com/prosemirror/website/blob/master/markdown/guide/view.md#2025-04-23_snippet_3

LANGUAGE: javascript
CODE:
```
function maxSizePlugin(max) {
  return new Plugin({
    props: {
      editable(state) { return state.doc.content.size < max }
    }
  })
}
```

----------------------------------------

TITLE: Using Transaction Metadata in ProseMirror Plugins (JavaScript)
DESCRIPTION: Demonstrates how to use transaction metadata to mark transactions for special handling by plugins. The example shows a transaction counter that ignores transactions marked with specific metadata.
SOURCE: https://github.com/prosemirror/website/blob/master/markdown/guide/state.md#2025-04-23_snippet_5

LANGUAGE: javascript
CODE:
```
let transactionCounter = new Plugin({
  state: {
    init() { return 0 },
    apply(tr, value) {
      if (tr.getMeta(transactionCounter)) return value
      else return value + 1
    }
  }
})

function markAsUncounted(tr) {
  tr.setMeta(transactionCounter, true)
}
```

----------------------------------------

TITLE: Implementing insertStar Command for Star Schema in ProseMirror
DESCRIPTION: This snippet defines a custom command for inserting a star node in the star schema. It checks if a star can be inserted at the current position and replaces the selection with a new star node if possible.
SOURCE: https://github.com/prosemirror/website/blob/master/pages/examples/schema/index.md#2025-04-23_snippet_6

LANGUAGE: javascript
CODE:
```
function insertStar(state, dispatch) {
  let type = state.schema.nodes.star
  let {$from} = state.selection
  if (!$from.parent.canReplaceWith($from.index(), $from.index(), type)) return false
  dispatch(state.tr.replaceSelectionWith(type.create()))
  return true
}
```

----------------------------------------

TITLE: Finding Placeholder Position in ProseMirror Document
DESCRIPTION: A utility function that finds the current position of a placeholder decoration by its ID. Returns null if the placeholder no longer exists in the document.
SOURCE: https://github.com/prosemirror/website/blob/master/pages/examples/upload/index.md#2025-04-23_snippet_1

LANGUAGE: javascript
CODE:
```
function findPlaceholder(state, id) {
  let decos = placeholderPlugin.getState(state)
  let found = decos.find(null, null, spec => spec.id == id)
  return found.length ? found[0].from : null
}
```

----------------------------------------

TITLE: Embedding ProseMirror Collaborative Editor in HTML
DESCRIPTION: This HTML snippet embeds the ProseMirror collaborative editor example into the page. It uses a custom HTML tag '@HTML' to indicate where the editor should be inserted.
SOURCE: https://github.com/prosemirror/website/blob/master/pages/examples/collab/index.md#2025-04-23_snippet_0

LANGUAGE: HTML
CODE:
```
@HTML
```

----------------------------------------

TITLE: Defining Mark Parsing Rules in ProseMirror Schema
DESCRIPTION: Example of parseDOM rules for an emphasis mark. These rules define how to recognize emphasis in pasted content by matching em tags, i tags, and inline font-style CSS.
SOURCE: https://github.com/prosemirror/website/blob/master/markdown/guide/schema.md#2025-04-23_snippet_5

LANGUAGE: javascript
CODE:
```
  parseDOM: [
    {tag: "em"},                 // Match <em> nodes
    {tag: "i"},                  // and <i> nodes
    {style: "font-style=italic"} // and inline 'font-style: italic'
  ]
```

----------------------------------------

TITLE: Running ProseMirror Development Server in Bash
DESCRIPTION: Command to start a development server for the ProseMirror website on port 8888, which serves files from public/ directory and includes the collaborative editing backend.
SOURCE: https://github.com/prosemirror/website/blob/master/README.md#2025-04-23_snippet_2

LANGUAGE: bash
CODE:
```
npm run devserver -- --port 8888
```

----------------------------------------

TITLE: Building ProseMirror Website Documentation in Bash
DESCRIPTION: Command to build the ProseMirror documentation and demo JavaScript bundles using make, which populates the public/ directory with the website files.
SOURCE: https://github.com/prosemirror/website/blob/master/README.md#2025-04-23_snippet_1

LANGUAGE: bash
CODE:
```
make
```

----------------------------------------

TITLE: Installing Dependencies for ProseMirror Website in Bash
DESCRIPTION: Command to install all Node.js dependencies for the ProseMirror website project using npm.
SOURCE: https://github.com/prosemirror/website/blob/master/README.md#2025-04-23_snippet_0

LANGUAGE: bash
CODE:
```
npm install
```TITLE: Creating a Minimal ProseMirror Editor in JavaScript
DESCRIPTION: Demonstrates how to create a basic ProseMirror editor by importing the schema, creating an editor state, and initializing an editor view. This minimal example creates an empty document conforming to the schema with a default selection at the start.
SOURCE: https://github.com/prosemirror/website/blob/master/markdown/guide/intro.md#2025-04-23_snippet_0

LANGUAGE: javascript
CODE:
```
import {schema} from "prosemirror-schema-basic"
import {EditorState} from "prosemirror-state"
import {EditorView} from "prosemirror-view"

let state = EditorState.create({schema})
let view = new EditorView(document.body, {state})
```

----------------------------------------

TITLE: Setting up a basic ProseMirror editor with example-setup in JavaScript
DESCRIPTION: This code snippet demonstrates how to set up a basic ProseMirror editor using the example-setup package. It imports the necessary modules, creates a schema that extends the basic schema with list nodes, and then initializes an editor with various plugins for a complete editing experience.
SOURCE: https://github.com/prosemirror/website/blob/master/pages/examples/basic/index.md#2025-04-23_snippet_0

LANGUAGE: javascript
CODE:
```
PART(code)
```

----------------------------------------

TITLE: Creating a Collaborative Editor with ProseMirror collab Module in JavaScript
DESCRIPTION: This code demonstrates how to set up a collaborative editor using ProseMirror's collab module. It creates an editor view with the collab plugin, tracks local changes, receives remote changes, and communicates with the central authority to synchronize document state across multiple editors.
SOURCE: https://github.com/prosemirror/website/blob/master/markdown/guide/collab.md#2025-04-23_snippet_1

LANGUAGE: javascript
CODE:
```
import {EditorState} from "prosemirror-state"
import {EditorView} from "prosemirror-view"
import {schema} from "prosemirror-schema-basic"
import collab from "prosemirror-collab"

function collabEditor(authority, place) {
  let view = new EditorView(place, {
    state: EditorState.create({
      doc: authority.doc,
      plugins: [collab.collab({version: authority.steps.length})]
    }),
    dispatchTransaction(transaction) {
      let newState = view.state.apply(transaction)
      view.updateState(newState)
      let sendable = collab.sendableSteps(newState)
      if (sendable)
        authority.receiveSteps(sendable.version, sendable.steps,
                               sendable.clientID)
    }
  })

  authority.onNewSteps.push(function() {
    let newData = authority.stepsSince(collab.getVersion(view.state))
    view.dispatch(
      collab.receiveTransaction(view.state, newData.steps, newData.clientIDs))
  })

  return view
}
```

----------------------------------------

TITLE: Adding History and Keymap Plugins to ProseMirror
DESCRIPTION: Demonstrates how to extend the editor functionality using plugins. This example adds undo/redo capability with the history plugin and registers keyboard shortcuts for these functions with the keymap plugin.
SOURCE: https://github.com/prosemirror/website/blob/master/markdown/guide/intro.md#2025-04-23_snippet_2

LANGUAGE: javascript
CODE:
```
// (Omitted repeated imports)
import {undo, redo, history} from "prosemirror-history"
import {keymap} from "prosemirror-keymap"

let state = EditorState.create({
  schema,
  plugins: [
    history(),
    keymap({"Mod-z": undo, "Mod-y": redo})
  ]
})
let view = new EditorView(document.body, {state})
```

----------------------------------------

TITLE: Defining a Basic Schema with Node Types in JavaScript
DESCRIPTION: Example of creating a simple ProseMirror schema with basic node types. The schema defines a document structure where the document contains paragraphs, and paragraphs contain text.
SOURCE: https://github.com/prosemirror/website/blob/master/markdown/guide/schema.md#2025-04-23_snippet_0

LANGUAGE: javascript
CODE:
```
const trivialSchema = new Schema({
  nodes: {
    doc: {content: "paragraph+"},
    paragraph: {content: "text*"},
    text: {inline: true},
    /* ... and so on */
  }
})
```

----------------------------------------

TITLE: Implementing Central Authority for ProseMirror Collaborative Editing in JavaScript
DESCRIPTION: This code defines an Authority class that tracks document versions, accepts changes from editors, and provides a way for editors to receive changes since a given version. It handles the central coordination of collaborative editing by managing steps, client IDs, and document state.
SOURCE: https://github.com/prosemirror/website/blob/master/markdown/guide/collab.md#2025-04-23_snippet_0

LANGUAGE: javascript
CODE:
```
class Authority {
  constructor(doc) {
    this.doc = doc
    this.steps = []
    this.stepClientIDs = []
    this.onNewSteps = []
  }

  receiveSteps(version, steps, clientID) {
    if (version != this.steps.length) return

    // Apply and accumulate new steps
    steps.forEach(step => {
      this.doc = step.apply(this.doc).doc
      this.steps.push(step)
      this.stepClientIDs.push(clientID)
    })
    // Signal listeners
    this.onNewSteps.forEach(function(f) { f() })
  }

  stepsSince(version) {
    return {
      steps: this.steps.slice(version),
      clientIDs: this.stepClientIDs.slice(version)
    }
  }
}
```

----------------------------------------

TITLE: Creating and Examining Editor State in ProseMirror (JavaScript)
DESCRIPTION: Demonstrates how to create a basic editor state using ProseMirror and examine its document and selection properties. The code imports the basic schema and EditorState, creates a new state, and logs its initial document content and selection position.
SOURCE: https://github.com/prosemirror/website/blob/master/markdown/guide/state.md#2025-04-23_snippet_0

LANGUAGE: javascript
CODE:
```
import {schema} from "prosemirror-schema-basic"
import {EditorState} from "prosemirror-state"

let state = EditorState.create({schema})
console.log(state.doc.toString()) // An empty paragraph
console.log(state.selection.from) // 1, the start of the paragraph
```

----------------------------------------

TITLE: Creating a Document with ProseMirror Schema
DESCRIPTION: A JavaScript example demonstrating how to programmatically create a ProseMirror document by using the schema's node and text methods. It creates a document with two paragraphs separated by a horizontal rule.
SOURCE: https://github.com/prosemirror/website/blob/master/markdown/guide/doc.md#2025-04-23_snippet_1

LANGUAGE: javascript
CODE:
```
import {schema} from "prosemirror-schema-basic"

// (The null arguments are where you can specify attributes, if necessary.)
let doc = schema.node("doc", null, [
  schema.node("paragraph", null, [schema.text("One.")]),
  schema.node("horizontal_rule"),
  schema.node("paragraph", null, [schema.text("Two!")])
])
```

----------------------------------------

TITLE: Implementing Basic Editor Commands with ProseMirror
DESCRIPTION: Shows how to add basic editing functionality by implementing common commands. This example adds the baseKeymap which provides expected behavior for keys like Enter and Delete, along with previously configured undo/redo functionality.
SOURCE: https://github.com/prosemirror/website/blob/master/markdown/guide/intro.md#2025-04-23_snippet_3

LANGUAGE: javascript
CODE:
```
// (Omitted repeated imports)
import {baseKeymap} from "prosemirror-commands"

let state = EditorState.create({
  schema,
  plugins: [
    history(),
    keymap({"Mod-z": undo, "Mod-y": redo}),
    keymap(baseKeymap)
  ]
})
let view = new EditorView(document.body, {state})
```

----------------------------------------

TITLE: Defining Menu Items for ProseMirror in JavaScript
DESCRIPTION: This code creates menu items for a basic ProseMirror menu with text formatting (strong, emphasis) and block type (paragraph, heading) options. Each menu item has a DOM element and a command to execute when clicked.
SOURCE: https://github.com/prosemirror/website/blob/master/pages/examples/menu/index.md#2025-04-23_snippet_2

LANGUAGE: javascript
CODE:
```
// Helper function to create menu items
function item(command, content) {
  let dom = document.createElement("button")
  dom.appendChild(typeof content === "string" ? document.createTextNode(content) : content)
  return {command, dom}
}

// Create an icon element for font style buttons
function fontIcon(text, name) {
  let span = document.createElement("span")
  span.className = "font-icon " + name
  span.textContent = text
  return span
}

// A set of basic menu items
let menu = [
  item(toggleMark(schema.marks.strong), fontIcon("B", "strong")),
  item(toggleMark(schema.marks.em), fontIcon("i", "em")),
  item(setBlockType(schema.nodes.paragraph), "paragraph"),
  item(setBlockType(schema.nodes.heading, {level: 1}), "heading")
]
```

----------------------------------------

TITLE: Implementing ProseMirror-based Markdown View in JavaScript
DESCRIPTION: This code defines a ProseMirrorView class that integrates a WYSIWYG editor using ProseMirror for editing markdown content. It handles converting between markdown text and ProseMirror documents, sets up plugins for the editor, and provides the same interface as the MarkdownView.
SOURCE: https://github.com/prosemirror/website/blob/master/pages/examples/markdown/index.md#2025-04-23_snippet_1

LANGUAGE: javascript
CODE:
```
class ProseMirrorView {
  constructor(target, content) {
    // Create a new editor state with the markdown schema and plugins
    this.state = EditorState.create({
      doc: markdownParser.parse(content),
      plugins: [keymap(baseKeymap)]
    })
    // Create an editor view in the target element
    this.view = new EditorView(target, {
      state: this.state,
      dispatchTransaction: transaction => {
        this.state = this.state.apply(transaction)
        this.view.updateState(this.state)
      }
    })
  }

  get content() {
    return markdownSerializer.serialize(this.state.doc)
  }

  focus() { this.view.focus() }
  destroy() { this.view.destroy() }

  update(content) {
    let newDoc = markdownParser.parse(content)
    if (!this.state.doc.eq(newDoc)) {
      this.view.dispatch(this.state.tr.replaceWith(0, this.state.doc.content.size, newDoc.content))
      return true
    }
    return false
  }
}
```

----------------------------------------

TITLE: Manipulating Selection in ProseMirror Transactions (JavaScript)
DESCRIPTION: Demonstrates how selections are automatically mapped through transaction steps and how to explicitly set a new selection. The example shows selection position changes when deleting content and manually setting a selection.
SOURCE: https://github.com/prosemirror/website/blob/master/markdown/guide/state.md#2025-04-23_snippet_2

LANGUAGE: javascript
CODE:
```
let tr = state.tr
console.log(tr.selection.from) // → 10
tr.delete(6, 8)
console.log(tr.selection.from) // → 8 (moved back)
tr.setSelection(TextSelection.create(tr.doc, 3))
console.log(tr.selection.from) // → 3
```

----------------------------------------

TITLE: Creating a Schema with Node Groups in JavaScript
DESCRIPTION: Demonstrates how to use node groups in a ProseMirror schema. This example creates a 'block' group containing paragraph and blockquote nodes, allowing for more flexible content expressions.
SOURCE: https://github.com/prosemirror/website/blob/master/markdown/guide/schema.md#2025-04-23_snippet_1

LANGUAGE: javascript
CODE:
```
const groupSchema = new Schema({
  nodes: {
    doc: {content: "block+"},
    paragraph: {group: "block", content: "text*"},
    blockquote: {group: "block", content: "block+"},
    text: {}
  }
})
```

----------------------------------------

TITLE: Defining Star Schema with Inline Nodes and Marks in ProseMirror
DESCRIPTION: This snippet creates a more advanced schema that includes inline 'star' nodes and various marks like 'shouting' and 'link'. It demonstrates how to define node groups, restrict mark usage, and configure DOM rendering and parsing for custom elements.
SOURCE: https://github.com/prosemirror/website/blob/master/pages/examples/schema/index.md#2025-04-23_snippet_3

LANGUAGE: javascript
CODE:
```
const starSchema = new Schema({
  nodes: {
    doc: {content: "block+"},
    paragraph: {
      group: "block",
      content: "inline*",
      toDOM: () => ["p", 0]
    },
    boring_paragraph: {
      group: "block",
      content: "text*",
      marks: "",
      toDOM: () => ["p", {class: "boring"}, 0]
    },
    text: {group: "inline"},
    star: {
      group: "inline",
      inline: true,
      toDOM: () => ["star", "★"],
      parseDOM: [{tag: "star"}]
    }
  },
  marks: {
    shouting: {
      toDOM: () => ["shouting", 0],
      parseDOM: [{tag: "shouting"}]
    },
    link: {
      attrs: {href: {}},
      toDOM: spec => ["a", {href: spec.href}, 0],
      parseDOM: [{tag: "a", getAttrs: dom => ({href: dom.href})}],
      inclusive: false
    }
  }
})
```

----------------------------------------

TITLE: Implementing Command with Optional Dispatch in JavaScript
DESCRIPTION: An improved version of the deleteSelection command that handles the optional dispatch argument. This pattern allows the command to be used both for execution and for querying whether the command is applicable in the current state.
SOURCE: https://github.com/prosemirror/website/blob/master/markdown/guide/commands.md#2025-04-23_snippet_1

LANGUAGE: javascript
CODE:
```
function deleteSelection(state, dispatch) {
  if (state.selection.empty) return false
  if (dispatch) dispatch(state.tr.deleteSelection())
  return true
}
```

----------------------------------------

TITLE: Creating and Applying Transactions in ProseMirror (JavaScript)
DESCRIPTION: Shows how to create a transaction from the current state, modify document content by inserting text, and apply the transaction to generate a new state. The example demonstrates how document size changes after text insertion.
SOURCE: https://github.com/prosemirror/website/blob/master/markdown/guide/state.md#2025-04-23_snippet_1

LANGUAGE: javascript
CODE:
```
let tr = state.tr
console.log(tr.doc.content.size) // 25
tr.insertText("hello") // Replaces selection with 'hello'
let newState = state.apply(tr)
console.log(tr.doc.content.size) // 30
```

----------------------------------------

TITLE: Implementing Document Linting Functions in ProseMirror
DESCRIPTION: A function that analyzes a ProseMirror document and returns an array of detected problems. It iterates through all document nodes, checking for issues like empty blocks, trailing whitespace, and duplicate headings, and provides appropriate fix methods.
SOURCE: https://github.com/prosemirror/website/blob/master/pages/examples/lint/index.md#2025-04-23_snippet_0

LANGUAGE: javascript
CODE:
```
function lint(doc) {
  let result = []
  let headers = Object.create(null)

  function record(node, from, to, message, fix) {
    result.push({node, from, to, message, fix})
  }

  doc.descendants((node, pos) => {
    if (node.isText) {
      // Warn about suspicious characters
      let m, re = /\w[\^\|\$%\*][\w\[\]\(\),]/g
      while (m = re.exec(node.text))
        record(node, pos + m.index, pos + m.index + 1,
               "Suspicious character: " + m[0][1])

      // Warn about repeated words
      let word = /\w+/g, lastWord = null, lastWordPos = 0
      while (m = word.exec(node.text)) {
        if (m[0].toLowerCase() == lastWord)
          record(node, pos + lastWordPos, pos + word.lastIndex,
                 "Duplicate word: " + m[0],
                 deleteRange(pos + lastWordPos, pos + word.lastIndex))
        lastWord = m[0].toLowerCase()
        lastWordPos = m.index
      }
    } else if (node.type.name == "heading") {
      let plain = node.content.content[0]?.textContent.trim() || ""
      if (plain in headers)
        record(node, pos, pos + node.nodeSize,
               "Duplicate heading: " + plain,
               deleteRange(pos, pos + node.nodeSize))
      headers[plain] = pos
    } else if (node.type.name == "image" && !node.attrs.alt) {
      record(node, pos, pos + node.nodeSize,
             "Image without alt text",
             addAlt(pos))
    } else if (node.type.name == "paragraph" && node.content.size == 0) {
      record(node, pos, pos + node.nodeSize,
             "Empty paragraph",
             deleteRange(pos, pos + node.nodeSize))
    } else if (node.type.name == "blockquote" && node.childCount == 0) {
      record(node, pos, pos + node.nodeSize,
             "Empty blockquote",
             deleteRange(pos, pos + node.nodeSize))
    } else if (node.type.name == "heading" && node.attrs.level > 2 &&
               node.textContent.length > 100) {
      record(node, pos, pos + node.nodeSize,
             "Long heading")
    } else if (node.isTextblock && node.content.size > 0) {
      let last = node.content.content[node.content.content.length - 1]
      if (last.isText && /\s$/.test(last.text)) {
        record(node, pos + node.nodeSize - 2, pos + node.nodeSize - 1,
               "Trailing whitespace",
               deleteRange(pos + node.nodeSize - 2, pos + node.nodeSize - 1))
      }
    }
  })

  return result
}
```

----------------------------------------

TITLE: Handling ProseMirror Transactions with Custom Dispatch Function
DESCRIPTION: Shows how to hook into ProseMirror's state transaction system by defining a custom dispatchTransaction function. This example logs document size changes and demonstrates the explicit state update pattern required in ProseMirror.
SOURCE: https://github.com/prosemirror/website/blob/master/markdown/guide/intro.md#2025-04-23_snippet_1

LANGUAGE: javascript
CODE:
```
// (Imports omitted)

let state = EditorState.create({schema})
let view = new EditorView(document.body, {
  state,
  dispatchTransaction(transaction) {
    console.log("Document size went from", transaction.before.content.size,
                "to", transaction.doc.content.size)
    let newState = view.state.apply(transaction)
    view.updateState(newState)
  }
})
```

----------------------------------------

TITLE: Defining a Schema with Marks in JavaScript
DESCRIPTION: Shows how to define marks in a ProseMirror schema. This example creates strong and emphasis marks that can be applied to text in paragraphs but not in headings.
SOURCE: https://github.com/prosemirror/website/blob/master/markdown/guide/schema.md#2025-04-23_snippet_2

LANGUAGE: javascript
CODE:
```
const markSchema = new Schema({
  nodes: {
    doc: {content: "block+"},
    paragraph: {group: "block", content: "text*", marks: "_"},
    heading: {group: "block", content: "text*", marks: ""},
    text: {inline: true}
  },
  marks: {
    strong: {},
    em: {}
  }
})
```

----------------------------------------

TITLE: Implementing toggleLink Command for Star Schema in ProseMirror
DESCRIPTION: This snippet defines a custom command for toggling links in the star schema. It checks if a link is being added or removed, prompts for a URL if adding, and then applies or removes the link mark accordingly.
SOURCE: https://github.com/prosemirror/website/blob/master/pages/examples/schema/index.md#2025-04-23_snippet_5

LANGUAGE: javascript
CODE:
```
function toggleLink(state, dispatch) {
  let {doc, selection} = state
  if (selection.empty) return false
  let attrs = null
  if (!doc.rangeHasMark(selection.from, selection.to, state.schema.marks.link)) {
    attrs = {href: prompt("Link to where?", "")}
    if (!attrs.href) return false
  }
  return toggleMark(state.schema.marks.link, attrs)(state, dispatch)
}
```

----------------------------------------

TITLE: Creating a Basic ProseMirror Plugin (JavaScript)
DESCRIPTION: Demonstrates how to create a simple ProseMirror plugin that adds event handling to the editor. The example creates a plugin that logs when keys are pressed and initializes an editor state with this plugin.
SOURCE: https://github.com/prosemirror/website/blob/master/markdown/guide/state.md#2025-04-23_snippet_3

LANGUAGE: javascript
CODE:
```
let myPlugin = new Plugin({
  props: {
    handleKeyDown(view, event) {
      console.log("A key was pressed!")
      return false // We did not handle this
    }
  }
})

let state = EditorState.create({schema, plugins: [myPlugin]})
```

----------------------------------------

TITLE: Initializing ProseMirror with Content from DOM
DESCRIPTION: Demonstrates how to initialize a ProseMirror editor with existing content by parsing DOM nodes. This uses the DOMParser mechanism to convert HTML content from an element with ID 'content' into a ProseMirror document structure based on the schema.
SOURCE: https://github.com/prosemirror/website/blob/master/markdown/guide/intro.md#2025-04-23_snippet_4

LANGUAGE: javascript
CODE:
```
import {DOMParser} from "prosemirror-model"
import {EditorState} from "prosemirror-state"
import {schema} from "prosemirror-schema-basic"

let content = document.getElementById("content")
let state = EditorState.create({
  doc: DOMParser.fromSchema(schema).parse(content)
})
```

----------------------------------------

TITLE: Setting CodeMirror Selection from ProseMirror
DESCRIPTION: This method handles setting the CodeMirror selection when ProseMirror tries to put the selection inside the node. It ensures the CodeMirror selection matches the position passed in.
SOURCE: https://github.com/prosemirror/website/blob/master/pages/examples/codemirror/index.md#2025-04-23_snippet_2

LANGUAGE: javascript
CODE:
```
PART(nodeview_setSelection)
```

----------------------------------------

TITLE: Defining TrackState for Change Tracking in ProseMirror
DESCRIPTION: This code snippet defines the TrackState class, which is used to track the commit history in a ProseMirror editor. It maintains an array of commits, each containing steps, inverses, and metadata.
SOURCE: https://github.com/prosemirror/website/blob/master/pages/examples/track/index.md#2025-04-23_snippet_0

LANGUAGE: javascript
CODE:
```
class TrackState {
  constructor(commits) {
    this.commits = commits
  }

  addCommit(message, time, steps, inverseSteps) {
    this.commits.push({
      message, time, steps,
      inverseSteps,
      hidden: false
    })
  }
}
```

----------------------------------------

TITLE: Implementing ProseMirror State Management with Redux-like Flow
DESCRIPTION: Example showing how to integrate ProseMirror's transaction system with a Redux-like state management architecture, including state updates and UI refresh logic.
SOURCE: https://github.com/prosemirror/website/blob/master/markdown/guide/view.md#2025-04-23_snippet_1

LANGUAGE: javascript
CODE:
```
let appState = {
  editor: EditorState.create({schema}),
  score: 0
}

let view = new EditorView(document.body, {
  state: appState.editor,
  dispatchTransaction(transaction) {
    update({type: "EDITOR_TRANSACTION", transaction})
  }
})

function update(event) {
  if (event.type == "EDITOR_TRANSACTION")
    appState.editor = appState.editor.apply(event.transaction)
  else if (event.type == "SCORE_POINT")
    appState.score++
  draw()
}

function draw() {
  document.querySelector("#score").textContent = appState.score
  view.updateState(appState.editor)
}
```

----------------------------------------

TITLE: Configuring Editor View Switching in JavaScript
DESCRIPTION: This code sets up radio buttons that allow users to switch between the raw markdown textarea view and the WYSIWYG ProseMirror view. It handles synchronizing content between the views when switching and initializes the editor with default content.
SOURCE: https://github.com/prosemirror/website/blob/master/pages/examples/markdown/index.md#2025-04-23_snippet_2

LANGUAGE: javascript
CODE:
```
let place = document.querySelector("#editor")
let view = new MarkdownView(place, "# Hello\n\nThis is a comment written in [Markdown](http://commonmark.org).\n\n* You can use formatting like **emphasis** and lists\n* You can add [links](http://example.com)\n* Even `inline code` and code blocks are supported\n\n        And that's about it\n")

function update(type) {
  if (view.constructor.name == type.name) return
  let content = view.content
  view.destroy()
  view = new type(place, content)
  view.focus()
}

for (let input of document.querySelectorAll("input[type=radio]")) {
  input.addEventListener("click", () => update(input.value == "markdown" ? MarkdownView : ProseMirrorView))
}
```

----------------------------------------

TITLE: Creating a Menu Plugin for ProseMirror in JavaScript
DESCRIPTION: This code defines a plugin that creates and manages a MenuView for a ProseMirror editor. The plugin handles initializing the MenuView, updating it when the editor state changes, and destroying it when the editor is destroyed.
SOURCE: https://github.com/prosemirror/website/blob/master/pages/examples/menu/index.md#2025-04-23_snippet_1

LANGUAGE: javascript
CODE:
```
function menuPlugin(items) {
  return new Plugin({
    view(editorView) {
      let menuView = new MenuView(items, editorView)
      editorView.dom.parentNode.insertBefore(menuView.dom, editorView.dom)
      return menuView
    }
  })
}
```

----------------------------------------

TITLE: Defining Basic Text Schema in ProseMirror
DESCRIPTION: This snippet defines a simple ProseMirror schema that allows only text content. It creates a document node that can contain one or more paragraph nodes, which in turn can contain text.
SOURCE: https://github.com/prosemirror/website/blob/master/pages/examples/schema/index.md#2025-04-23_snippet_0

LANGUAGE: javascript
CODE:
```
const textSchema = new Schema({
  nodes: {
    doc: {content: "paragraph+"},
    paragraph: {content: "text*", toDOM: () => ["p", 0]},
    text: {}
  }
})
```

----------------------------------------

TITLE: Updating CodeMirror Content from ProseMirror
DESCRIPTION: This method handles updates coming from ProseMirror, such as undo actions. It compares the old and new content to generate a minimal replacement for the changed range in the inner editor.
SOURCE: https://github.com/prosemirror/website/blob/master/pages/examples/codemirror/index.md#2025-04-23_snippet_4

LANGUAGE: javascript
CODE:
```
PART(nodeview_update)
```

----------------------------------------

TITLE: Implementing Commit Reversion in ProseMirror
DESCRIPTION: This function reverts a specific commit by rebasing the inverted form of its steps over all intermediate steps. It handles the complexities of moving changes across each other, which can sometimes lead to unintuitive results in complicated scenarios.
SOURCE: https://github.com/prosemirror/website/blob/master/pages/examples/track/index.md#2025-04-23_snippet_2

LANGUAGE: javascript
CODE:
```
function revertCommit(state, commit) {
  let trackState = trackPlugin.getState(state)
  let index = trackState.commits.indexOf(commit)
  let nextCommits = trackState.commits.slice(index + 1)
  let steps = commit.inverseSteps.slice()

  for (let i = index + 1; i < trackState.commits.length; i++) {
    let inv = trackState.commits[i].inverseSteps
    for (let j = inv.length - 1; j >= 0; j--)
      steps = Transform.mapStep(steps, inv[j])
    for (let j = 0; j < steps.length; j++)
      inv = Transform.mapStep(inv, steps[j])
  }

  let tr = state.tr
  for (let i = 0; i < steps.length; i++) tr.step(steps[i])
  return tr
}
```

----------------------------------------

TITLE: Adding Node Attributes in JavaScript
DESCRIPTION: Demonstrates how to define node attributes in a ProseMirror schema. This example adds a 'level' attribute to heading nodes with a default value of 1.
SOURCE: https://github.com/prosemirror/website/blob/master/markdown/guide/schema.md#2025-04-23_snippet_3

LANGUAGE: javascript
CODE:
```
  heading: {
    content: "text*",
    attrs: {level: {default: 1}}
  }
```

----------------------------------------

TITLE: Handling Inner Transactions in ProseMirror Footnote
DESCRIPTION: Implements the dispatchInner method to handle transactions in the footnote sub-editor. This method applies the changes from the inner editor to the outer document, handling appended transactions and avoiding infinite loops.
SOURCE: https://github.com/prosemirror/website/blob/master/pages/examples/footnote/index.md#2025-04-23_snippet_3

LANGUAGE: javascript
CODE:
```
dispatchInner(tr) {
  let {state, transactions} = this.innerView.state.applyTransaction(tr)
  this.innerView.updateState(state)

  if (!tr.getMeta("fromOutside")) {
    let outerTr = this.outerView.state.tr, offsetMap = StepMap.offset(this.getPos() + 1)
    for (let i = 0; i < transactions.length; i++) {
      let steps = transactions[i].steps
      for (let j = 0; j < steps.length; j++)
        outerTr.step(steps[j].map(offsetMap))
    }
    if (outerTr.docChanged) this.outerView.dispatch(outerTr)
  }
}
```

----------------------------------------

TITLE: Basic Image Node View Implementation in ProseMirror
DESCRIPTION: Creates a custom node view for image nodes with click event handling. Demonstrates basic node view creation and event management.
SOURCE: https://github.com/prosemirror/website/blob/master/markdown/guide/view.md#2025-04-23_snippet_6

LANGUAGE: javascript
CODE:
```
let view = new EditorView({
  state,
  nodeViews: {
    image(node) { return new ImageView(node) }
  }
})

class ImageView {
  constructor(node) {
    // The editor will use this as the node's DOM representation
    this.dom = document.createElement("img")
    this.dom.src = node.attrs.src
    this.dom.addEventListener("click", e => {
      console.log("You clicked me!")
      e.preventDefault()
    })
  }

  stopEvent() { return true }
}
```

----------------------------------------

TITLE: Initializing CodeMirror Node View in ProseMirror
DESCRIPTION: This snippet sets up the basic structure of a CodeMirror node view for ProseMirror. It creates a CodeMirror editor with custom extensions and key bindings, and sets up an update listener for synchronization.
SOURCE: https://github.com/prosemirror/website/blob/master/pages/examples/codemirror/index.md#2025-04-23_snippet_0

LANGUAGE: javascript
CODE:
```
PART(nodeview_start)
```

----------------------------------------

TITLE: Applying a ReplaceStep in JavaScript
DESCRIPTION: This snippet demonstrates how to create and apply a ReplaceStep to delete content between positions 3 and 5 in a document.
SOURCE: https://github.com/prosemirror/website/blob/master/markdown/guide/transform.md#2025-04-23_snippet_0

LANGUAGE: javascript
CODE:
```
console.log(myDoc.toString()) // → p("hello")
// A step that deletes the content between positions 3 and 5
let step = new ReplaceStep(3, 5, Slice.empty)
let result = step.apply(myDoc)
console.log(result.doc.toString()) // → p("heo")
```

----------------------------------------

TITLE: Synchronizing CodeMirror Updates to ProseMirror
DESCRIPTION: This function forwards updates from CodeMirror to ProseMirror when the code editor is focused. It translates document and selection changes into ProseMirror transactions.
SOURCE: https://github.com/prosemirror/website/blob/master/pages/examples/codemirror/index.md#2025-04-23_snippet_1

LANGUAGE: javascript
CODE:
```
PART(nodeview_forwardUpdate)
```

----------------------------------------

TITLE: Creating and Using a Transform in JavaScript
DESCRIPTION: This example shows how to create a Transform object, perform multiple operations (delete and split), and access the resulting document and step count.
SOURCE: https://github.com/prosemirror/website/blob/master/markdown/guide/transform.md#2025-04-23_snippet_1

LANGUAGE: javascript
CODE:
```
let tr = new Transform(myDoc)
tr.delete(5, 7) // Delete between position 5 and 7
tr.split(5)     // Split the parent node at position 5
console.log(tr.doc.toString()) // The modified document
console.log(tr.steps.length)   // → 2
```

----------------------------------------

TITLE: Implementing Fold Toggle Functionality in ProseMirror
DESCRIPTION: This function dispatches transactions to toggle the folded state of a section. It also ensures the selection is moved outside of a section when it's being folded to maintain usability.
SOURCE: https://github.com/prosemirror/website/blob/master/pages/examples/fold/index.md#2025-04-23_snippet_3

LANGUAGE: javascript
CODE:
```
PART(setFolding)
```

----------------------------------------

TITLE: Implementing a Tooltip Component for ProseMirror
DESCRIPTION: This snippet defines a Tooltip class that creates and manages a tooltip DOM element. It handles the tooltip's creation, positioning, and updates based on the editor's state changes. The positioning is calculated using ProseMirror's coordsAtPos method.
SOURCE: https://github.com/prosemirror/website/blob/master/pages/examples/tooltip/index.md#2025-04-23_snippet_1

LANGUAGE: JavaScript
CODE:
```
class Tooltip {
  constructor(view) {
    this.tooltip = document.createElement("div")
    this.tooltip.className = "tooltip"
    view.dom.parentNode.appendChild(this.tooltip)
    this.update(view, null)
  }

  update(view, lastState) {
    let state = view.state
    // Don't do anything if the document/selection didn't change
    if (lastState && lastState.doc.eq(state.doc) &&
        lastState.selection.eq(state.selection)) return

    // Hide the tooltip if the selection is empty
    if (state.selection.empty) {
      this.tooltip.style.display = "none"
      return
    }

    // Otherwise, reposition it and update its content
    this.tooltip.style.display = ""
    let {from, to} = state.selection
    // These are in screen coordinates
    let start = view.coordsAtPos(from), end = view.coordsAtPos(to)
    // The box in which the tooltip is positioned, to use as base
    let box = this.tooltip.offsetParent.getBoundingClientRect()
    // Find a center-ish x position from the selection endpoints (when
    // crossing lines, end may be more to the left than start, so take
    // the min of their left sides and the max of their right sides).
    let left = Math.max((start.left + end.left) / 2, start.left, end.left)
    left = (left - box.left - this.tooltip.offsetWidth / 2) + "px"
    this.tooltip.style.left = left
    this.tooltip.style.bottom = (box.bottom - start.top) + "px"
    this.tooltip.textContent =
      state.doc.textBetween(from, to, " ")
  }

  destroy() { this.tooltip.remove() }
}
```

----------------------------------------

TITLE: Creating a MenuView Component for ProseMirror in JavaScript
DESCRIPTION: This code creates a MenuView class that renders menu items in a menu bar and handles click events to trigger the corresponding commands. It also manages updating which menu items are visible based on their applicability to the current editor state.
SOURCE: https://github.com/prosemirror/website/blob/master/pages/examples/menu/index.md#2025-04-23_snippet_0

LANGUAGE: javascript
CODE:
```
class MenuView {
  constructor(items, editorView) {
    this.items = items
    this.editorView = editorView

    this.dom = document.createElement("div")
    this.dom.className = "menubar"
    items.forEach(({dom}) => this.dom.appendChild(dom))
    this.update()

    this.dom.addEventListener("mousedown", e => {
      e.preventDefault()
      editorView.focus()
      items.forEach(({command, dom}) => {
        if (dom.contains(e.target))
          command(editorView.state, editorView.dispatch, editorView)
      })
    })
  }

  update() {
    this.items.forEach(({command, dom}) => {
      let active = command(this.editorView.state, null, this.editorView)
      dom.style.display = active ? "" : "none"
    })
  }

  destroy() { this.dom.remove() }
}
```

----------------------------------------

TITLE: Handling File Input Events for Image Upload in ProseMirror
DESCRIPTION: Event handler for file input changes that initiates image uploads. It performs validation checks on selected files before starting the upload process.
SOURCE: https://github.com/prosemirror/website/blob/master/pages/examples/upload/index.md#2025-04-23_snippet_2

LANGUAGE: javascript
CODE:
```
document.querySelector("#image-upload").addEventListener("change", e => {
  if (view.isDestroyed) return
  let file = e.target.files[0]
  if (!file) return
  if (!file.type.startsWith("image/")) return
  if (file.size < 20 || file.size > 10000000) return

  startImageUpload(file, view, view.state.selection.from)
})
```

----------------------------------------

TITLE: Implementing Textarea-based Markdown View in JavaScript
DESCRIPTION: This code defines a MarkdownView class that wraps a textarea element for editing markdown content. It provides methods to update the content, get the current content, and focus the editor.
SOURCE: https://github.com/prosemirror/website/blob/master/pages/examples/markdown/index.md#2025-04-23_snippet_0

LANGUAGE: javascript
CODE:
```
class MarkdownView {
  constructor(target, content) {
    this.textarea = target.appendChild(document.createElement("textarea"))
    this.textarea.value = content
  }

  get content() { return this.textarea.value }
  focus() { this.textarea.focus() }
  destroy() { this.textarea.remove() }

  // :: (string) → bool
  // Update the editor's content, and return a boolean that indicates
  // whether anything changed.
  update(content) {
    if (this.textarea.value != content) {
      this.textarea.value = content
      return true
    }
    return false
  }
}
```

----------------------------------------

TITLE: Defining Note Schema with Groups in ProseMirror
DESCRIPTION: This snippet creates a more complex schema with notes that can be optionally grouped. It defines custom DOM elements for notes and note groups, and includes toDOM and parseDOM methods for proper rendering and parsing.
SOURCE: https://github.com/prosemirror/website/blob/master/pages/examples/schema/index.md#2025-04-23_snippet_1

LANGUAGE: javascript
CODE:
```
const noteSchema = new Schema({
  nodes: {
    doc: {content: "(note | notegroup)+"},
    notegroup: {
      content: "note+",
      toDOM: () => ["notegroup", 0],
      parseDOM: [{tag: "notegroup"}]
    },
    note: {
      content: "text*",
      toDOM: () => ["note", 0],
      parseDOM: [{tag: "note"}]
    },
    text: {}
  }
})
```

----------------------------------------

TITLE: Implementing Command with View Access in JavaScript
DESCRIPTION: A command that interacts with the editor's DOM. This example demonstrates how commands can use the optional third parameter (view) to perform actions that require access to the DOM, such as visual effects or positioning dialogs.
SOURCE: https://github.com/prosemirror/website/blob/master/markdown/guide/commands.md#2025-04-23_snippet_2

LANGUAGE: javascript
CODE:
```
function blinkView(_state, dispatch, view) {
  if (dispatch) {
    view.dom.style.background = "yellow"
    setTimeout(() => view.dom.style.background = "", 1000)
  }
  return true
}
```

----------------------------------------

TITLE: Updating Footnote Node View in ProseMirror
DESCRIPTION: Implements the update method for the footnote node view. This method handles updates from the outer editor, carefully finding and applying differences to maintain cursor position when possible.
SOURCE: https://github.com/prosemirror/website/blob/master/pages/examples/footnote/index.md#2025-04-23_snippet_4

LANGUAGE: javascript
CODE:
```
update(node) {
  if (!node.sameMarkup(this.node)) return false
  this.node = node
  if (this.innerView) {
    let state = this.innerView.state
    let start = node.content.findDiffStart(state.doc.content)
    if (start != null) {
      let {a: endA, b: endB} = node.content.findDiffEnd(state.doc.content)
      let overlap = start - Math.min(endA, endB)
      if (overlap > 0) { endA += overlap; endB += overlap }
      this.innerView.dispatch(
        state.tr
          .replace(start, endB, node.slice(start, endA))
          .setMeta("fromOutside", true))
    }
  }
  return true
}
```

----------------------------------------

TITLE: Creating a Folding Plugin for ProseMirror
DESCRIPTION: This code defines a plugin that tracks and manages folding decorations. It maintains the fold state across editor updates, handles transactions with folding metadata, and installs the foldable node view.
SOURCE: https://github.com/prosemirror/website/blob/master/pages/examples/fold/index.md#2025-04-23_snippet_2

LANGUAGE: javascript
CODE:
```
PART(plugin)
```

----------------------------------------

TITLE: Creating Visual Decorations for ProseMirror Linter
DESCRIPTION: This function generates decorations for highlighting detected problems in the document. It creates a gutter icon and applies a background decoration to the problematic text, with styles applied via CSS.
SOURCE: https://github.com/prosemirror/website/blob/master/pages/examples/lint/index.md#2025-04-23_snippet_2

LANGUAGE: javascript
CODE:
```
function diagnosticToDecorations(diagnostic) {
  let result = []
  diagnostic.forEach(prob => {
    let from = prob.from, to = prob.to
    let widget = document.createElement("div")
    widget.className = "lint-icon"
    widget.problem = prob
    widget.title = prob.message
    result.push(Decoration.widget({widget, side: 1}).range(from))
    if (from != to)
      result.push(Decoration.inline(from, to, {class: "lint-annotation"}))
  })
  return result
}
```

----------------------------------------

TITLE: Mapping Positions with StepMap in JavaScript
DESCRIPTION: This snippet illustrates how to create a StepMap from a ReplaceStep and use it to map positions from the old document to the new one.
SOURCE: https://github.com/prosemirror/website/blob/master/markdown/guide/transform.md#2025-04-23_snippet_2

LANGUAGE: javascript
CODE:
```
let step = new ReplaceStep(4, 6, Slice.empty) // Delete 4-5
let map = step.getMap()
console.log(map.map(8)) // → 6
console.log(map.map(2)) // → 2 (nothing changes before the change)
```

----------------------------------------

TITLE: Creating ProseMirror Linter Plugin
DESCRIPTION: A plugin implementation that integrates linting into ProseMirror. It tracks document changes, updates decorations, and handles icon click events to select issues or apply fixes when double-clicked.
SOURCE: https://github.com/prosemirror/website/blob/master/pages/examples/lint/index.md#2025-04-23_snippet_3

LANGUAGE: javascript
CODE:
```
class LintPlugin {
  constructor(view) {
    this.view = view
    this.problems = lint(view.state.doc)
    this.decorations = DecorationSet.create(view.state.doc, diagnosticToDecorations(this.problems))

    this.update(view, null)
  }

  update(view, oldState) {
    if (oldState && oldState.doc.eq(view.state.doc)) return false
    this.problems = lint(view.state.doc)
    this.decorations = DecorationSet.create(view.state.doc, diagnosticToDecorations(this.problems))
    return true
  }
}

function handleClick(view, pos, event) {
  if (!event.target.problem) return false
  let prob = event.target.problem
  if (event.type == "dblclick") {
    if (prob.fix) prob.fix(view)
    return true
  }
  view.dispatch(view.state.tr.setSelection(TextSelection.create(view.state.doc, prob.from, prob.to)))
  return true
}

let lintPlugin = new Plugin({
  key: new PluginKey("lint"),
  state: {
    init(_, {doc}) { return {doc, problems: lint(doc), decorations: []} },
    apply(tr, prev, oldState, state) {
      if (tr.docChanged) {
        let problems = lint(tr.doc)
        return {doc: tr.doc, problems, decorations: diagnosticToDecorations(problems)}
      }
      return prev
    }
  },
  props: {
    decorations(state) {
      return this.getState(state).decorations
    },
    handleClick
  }
})
```

----------------------------------------

TITLE: Handling Cursor Movement from ProseMirror to CodeMirror
DESCRIPTION: This function creates arrow key handlers for the outer editor to manage cursor movement from ProseMirror into the CodeMirror editor when the cursor is at the end of a textblock adjacent to a code block.
SOURCE: https://github.com/prosemirror/website/blob/master/pages/examples/codemirror/index.md#2025-04-23_snippet_6

LANGUAGE: javascript
CODE:
```
PART(arrowHandlers)
```

----------------------------------------

TITLE: Setting Up ProseMirror Editor with Footnotes
DESCRIPTION: Creates a ProseMirror editor instance with the custom footnote schema and node view. This setup enables footnote functionality in the editor.
SOURCE: https://github.com/prosemirror/website/blob/master/pages/examples/footnote/index.md#2025-04-23_snippet_6

LANGUAGE: javascript
CODE:
```
window.view = new EditorView(document.querySelector("#editor"), {
  state: EditorState.create({schema, plugins: [exampleSetup({schema})]}),
  nodeViews: {
    footnote(node, view, getPos) { return new FootnoteView(node, view, getPos) }
  }
})
```

----------------------------------------

TITLE: Implementing Asynchronous Image Upload in ProseMirror
DESCRIPTION: Function that handles the complete image upload process, from creating a placeholder to replacing it with the actual image when upload completes. It manages document transactions to maintain placeholder positions.
SOURCE: https://github.com/prosemirror/website/blob/master/pages/examples/upload/index.md#2025-04-23_snippet_3

LANGUAGE: javascript
CODE:
```
function startImageUpload(file, view, pos) {
  // A fresh object to act as the ID for this upload
  let id = {}

  // Replace the selection with a placeholder
  let tr = view.state.tr
  if (!tr.selection.empty) tr.deleteSelection()
  tr.setMeta(placeholderPlugin, {add: {id, pos}})
  view.dispatch(tr)

  uploadFile(file).then(url => {
    let pos = findPlaceholder(view.state, id)
    // If the content around the placeholder has been deleted, drop
    // the image
    if (pos == null) return
    // Otherwise, insert it at the placeholder's position, and remove
    // the placeholder
    view.dispatch(view.state.tr
                 .replaceWith(pos, pos, schema.nodes.image.create({src: url}))
                 .setMeta(placeholderPlugin, {remove: {id}}))
  }, () => {
    // On failure, just clean up the placeholder
    view.dispatch(view.state.tr.setMeta(placeholderPlugin, {remove: {id}}))
  })
}
```

----------------------------------------

TITLE: Implementing Basic Command: Delete Selection in JavaScript
DESCRIPTION: A simple ProseMirror command that deletes the current selection. The command checks if a selection exists, and if so, dispatches a transaction to delete it. It demonstrates the basic command pattern of checking applicability and performing an action.
SOURCE: https://github.com/prosemirror/website/blob/master/markdown/guide/commands.md#2025-04-23_snippet_0

LANGUAGE: javascript
CODE:
```
function deleteSelection(state, dispatch) {
  if (state.selection.empty) return false
  dispatch(state.tr.deleteSelection())
  return true
}
```

----------------------------------------

TITLE: Creating a Placeholder Decoration Plugin in ProseMirror
DESCRIPTION: A plugin that manages widget decorations representing image upload placeholders. It maintains a decoration set and provides methods to add and remove decorations by ID.
SOURCE: https://github.com/prosemirror/website/blob/master/pages/examples/upload/index.md#2025-04-23_snippet_0

LANGUAGE: javascript
CODE:
```
function placeholderPlugin() {
  return new Plugin({
    state: {
      init() { return DecorationSet.empty },
      apply(tr, set) {
        // Adjust decoration positions to changes made by the transaction
        set = set.map(tr.mapping, tr.doc)
        // See if the transaction adds or removes any placeholders
        let action = tr.getMeta(this)
        if (action && action.add) {
          let widget = document.createElement("placeholder")
          let deco = Decoration.widget(action.add.pos, widget, {id: action.add.id})
          set = set.add(tr.doc, [deco])
        } else if (action && action.remove) {
          set = set.remove(set.find(null, null,
                                      spec => spec.id == action.remove.id))
        }
        return set
      }
    },
    props: {
      decorations(state) { return this.getState(state) }
    }
  })
}
```

----------------------------------------

TITLE: Interactive Image Node View with Alt Text Editing in ProseMirror
DESCRIPTION: Extends the image node view to allow alt text editing through click interactions. Shows how to update node attributes using transactions.
SOURCE: https://github.com/prosemirror/website/blob/master/markdown/guide/view.md#2025-04-23_snippet_7

LANGUAGE: javascript
CODE:
```
let view = new EditorView({
  state,
  nodeViews: {
    image(node, view, getPos) { return new ImageView(node, view, getPos) }
  }
})

class ImageView {
  constructor(node, view, getPos) {
    this.dom = document.createElement("img")
    this.dom.src = node.attrs.src
    this.dom.alt = node.attrs.alt
    this.dom.addEventListener("click", e => {
      e.preventDefault()
      let alt = prompt("New alt text:", "")
      if (alt) view.dispatch(view.state.tr.setNodeMarkup(getPos(), null, {
        src: node.attrs.src,
        alt
      }))
    })
  }

  stopEvent() { return true }
}
```

----------------------------------------

TITLE: Configuring ProseMirror Editor View Props
DESCRIPTION: Demonstration of setting up ProseMirror editor view props, including read-only behavior and event handling configuration.
SOURCE: https://github.com/prosemirror/website/blob/master/markdown/guide/view.md#2025-04-23_snippet_2

LANGUAGE: javascript
CODE:
```
let view = new EditorView({
  state: myState,
  editable() { return false }, // Enables read-only behavior
  handleDoubleClick() { console.log("Double click!") }
})
```

----------------------------------------

TITLE: Creating a ProseMirror Plugin with State Management (JavaScript)
DESCRIPTION: Shows how to create a plugin that maintains its own state within the editor state. This example implements a transaction counter plugin that increments its value each time a transaction is applied.
SOURCE: https://github.com/prosemirror/website/blob/master/markdown/guide/state.md#2025-04-23_snippet_4

LANGUAGE: javascript
CODE:
```
let transactionCounter = new Plugin({
  state: {
    init() { return 0 },
    apply(tr, value) { return value + 1 }
  }
})

function getTransactionCount(state) {
  return transactionCounter.getState(state)
}
```

----------------------------------------

TITLE: Creating a ProseMirror Plugin for Tooltip Management
DESCRIPTION: This snippet defines a ProseMirror plugin that manages the lifecycle of a tooltip. It creates a plugin view that handles the creation, updating, and destruction of the tooltip based on the editor's state.
SOURCE: https://github.com/prosemirror/website/blob/master/pages/examples/tooltip/index.md#2025-04-23_snippet_0

LANGUAGE: JavaScript
CODE:
```
new Plugin({
  view(editorView) { return new Tooltip(editorView) }
})
```

----------------------------------------

TITLE: Creating Schema with Section Nodes in ProseMirror
DESCRIPTION: This code defines a modified schema that structures content as sections, each containing a heading followed by arbitrary blocks. It extends the basic schema to support the folding functionality.
SOURCE: https://github.com/prosemirror/website/blob/master/pages/examples/fold/index.md#2025-04-23_snippet_0

LANGUAGE: javascript
CODE:
```
PART(schema)
```

----------------------------------------

TITLE: Finalizing CodeMirror Node View Setup
DESCRIPTION: This snippet completes the setup of the CodeMirror node view, including methods for destroying the view and ignoring mutations.
SOURCE: https://github.com/prosemirror/website/blob/master/pages/examples/codemirror/index.md#2025-04-23_snippet_5

LANGUAGE: javascript
CODE:
```
PART(nodeview_end)
```

----------------------------------------

TITLE: Defining Footnote Schema in ProseMirror
DESCRIPTION: Defines the schema for footnote nodes in ProseMirror. Footnotes are implemented as inline nodes with content, marked as atoms for special handling.
SOURCE: https://github.com/prosemirror/website/blob/master/pages/examples/footnote/index.md#2025-04-23_snippet_0

LANGUAGE: javascript
CODE:
```
schema: {
  nodes: {
    doc: {content: "block+"},
    paragraph: {group: "block", content: "inline*"},
    text: {group: "inline"},
    footnote: {
      inline: true,
      group: "inline",
      content: "inline*",
      atom: true,
      toDOM: () => ["footnote", 0],
      parseDOM: [{tag: "footnote"}]
    }
  }
}
```

----------------------------------------

TITLE: Finalizing Footnote Node View in ProseMirror
DESCRIPTION: Implements the destroy method and event handling for the footnote node view. This includes cleanup on destruction and defining which events should be handled by the outer editor.
SOURCE: https://github.com/prosemirror/website/blob/master/pages/examples/footnote/index.md#2025-04-23_snippet_5

LANGUAGE: javascript
CODE:
```
destroy() {
  if (this.innerView) this.closeInner()
}

stopEvent(event) {
  return this.innerView && this.innerView.dom.contains(event.target)
}

ignoredClick(event) {
  return this.innerView && this.innerView.dom.contains(event.target)
}
```

----------------------------------------

TITLE: Initializing Footnote Node View in ProseMirror
DESCRIPTION: Sets up the basic structure for a custom node view to handle footnotes. This includes creating DOM elements and initializing variables for the node view.
SOURCE: https://github.com/prosemirror/website/blob/master/pages/examples/footnote/index.md#2025-04-23_snippet_1

LANGUAGE: javascript
CODE:
```
class FootnoteView {
  constructor(node, view, getPos) {
    this.node = node
    this.outerView = view
    this.getPos = getPos

    this.dom = document.createElement("footnote")
    this.open = false
    this.innerView = null
  }

  selectNode() {
    this.dom.classList.add("ProseMirror-selectednode")
    if (!this.open) this.open = this.openInner()
  }

  deselectNode() {
    this.dom.classList.remove("ProseMirror-selectednode")
    if (this.open) {
      this.closeInner()
      this.open = false
    }
  }
```

----------------------------------------

TITLE: Creating a ProseMirror Plugin for Change Tracking
DESCRIPTION: This snippet defines a ProseMirror plugin that observes transactions and updates the TrackState. It processes commit transactions by extracting the commit message from the transaction's meta property.
SOURCE: https://github.com/prosemirror/website/blob/master/pages/examples/track/index.md#2025-04-23_snippet_1

LANGUAGE: javascript
CODE:
```
const trackPlugin = new Plugin({
  state: {
    init() { return new TrackState([]) },
    apply(tr, trackState) {
      let newState = trackState
      let commitMessage = tr.getMeta(trackPlugin)
      if (commitMessage) {
        let startState = tr.startState
        let steps = tr.steps.slice()
        newState = new TrackState(trackState.commits.slice())
        newState.addCommit(commitMessage, Date.now(), steps,
                          invertedSteps(steps, startState.doc, tr.doc))
      }
      return newState
    }
  }
})
```

----------------------------------------

TITLE: Creating Document Slices in JavaScript with ProseMirror
DESCRIPTION: This JavaScript code demonstrates how to create slices from a ProseMirror document using the slice method. It shows how to extract different parts of the document and check their open depths.
SOURCE: https://github.com/prosemirror/website/blob/master/markdown/guide/doc.md#2025-04-23_snippet_3

LANGUAGE: javascript
CODE:
```
// doc holds two paragraphs, containing text "a" and "b"
let slice1 = doc.slice(0, 3) // The first paragraph
console.log(slice1.openStart, slice1.openEnd) // → 0 0
let slice2 = doc.slice(1, 5) // From start of first paragraph
                             // to end of second
console.log(slice2.openStart, slice2.openEnd) // → 1 1
```

----------------------------------------

TITLE: Custom Paragraph Node View with Empty State Styling in ProseMirror
DESCRIPTION: Implements a paragraph node view that adds special styling for empty paragraphs. Demonstrates content DOM handling and node updates.
SOURCE: https://github.com/prosemirror/website/blob/master/markdown/guide/view.md#2025-04-23_snippet_8

LANGUAGE: javascript
CODE:
```
let view = new EditorView({
  state,
  nodeViews: {
    paragraph(node) { return new ParagraphView(node) }
  }
})

class ParagraphView {
  constructor(node) {
    this.dom = this.contentDOM = document.createElement("p")
    if (node.content.size == 0) this.dom.classList.add("empty")
  }

  update(node) {
    if (node.content.size > 0) this.dom.classList.remove("empty")
    else this.dom.classList.add("empty")
    return true
  }
}
```

----------------------------------------

TITLE: Demonstrating HTML Document Structure in ProseMirror
DESCRIPTION: This HTML snippet illustrates the structure of a simple document in ProseMirror, showing how paragraphs, blockquotes, and images are represented.
SOURCE: https://github.com/prosemirror/website/blob/master/markdown/guide/doc.md#2025-04-23_snippet_2

LANGUAGE: html
CODE:
```
<p>One</p>
<blockquote><p>Two<img src="..."></p></blockquote>
```

----------------------------------------

TITLE: Setting up ProseMirror Editor with Custom Dinosaur Functionality
DESCRIPTION: This snippet creates the ProseMirror editor state and view with the custom dinosaur schema and menu items. It uses the example setup and extends it with the custom functionality.
SOURCE: https://github.com/prosemirror/website/blob/master/pages/examples/dino/index.md#2025-04-23_snippet_4

LANGUAGE: javascript
CODE:
```
import {exampleSetup, buildMenuItems} from "prosemirror-example-setup"
import {EditorState} from "prosemirror-state"
import {EditorView} from "prosemirror-view"

let menu = buildMenuItems(dinoSchema)
menu.insertMenu.content.push(dinoItem)
menu.insertMenu.content.push(dinoItems)

window.view = new EditorView(document.querySelector("#editor"), {
  state: EditorState.create({
    doc: doc,
    plugins: exampleSetup({schema: dinoSchema, menuContent: menu.fullMenu})
  })
})
```

----------------------------------------

TITLE: Defining Custom Dinosaur Node Spec in JavaScript
DESCRIPTION: This snippet defines a custom node spec for dinosaurs in ProseMirror. It specifies the node's attributes, inline status, group, and how it should be rendered in the DOM.
SOURCE: https://github.com/prosemirror/website/blob/master/pages/examples/dino/index.md#2025-04-23_snippet_0

LANGUAGE: javascript
CODE:
```
const dinoNodeSpec = {
  attrs: {type: {default: "brontosaurus"}},
  inline: true,
  group: "inline",
  draggable: true,

  toDOM: node => ["img", {
    "dino-type": node.attrs.type,
    src: "/img/dino/" + node.attrs.type + ".png",
    title: node.attrs.type,
    class: "dinosaur"
  }],
  parseDOM: [{
    tag: "img[dino-type]",
    getAttrs: dom => {
      let type = dom.getAttribute("dino-type")
      return type ? {type} : false
    }
  }]
}
```

----------------------------------------

TITLE: Implementing makeNoteGroup Command in ProseMirror
DESCRIPTION: This snippet defines a custom editing command that creates a note group around selected notes. It checks if the selection is valid for grouping and then wraps the selected notes in a new notegroup node.
SOURCE: https://github.com/prosemirror/website/blob/master/pages/examples/schema/index.md#2025-04-23_snippet_2

LANGUAGE: javascript
CODE:
```
function makeNoteGroup(state, dispatch) {
  let range = state.selection
  let from = range.$from.start(1), to = range.$to.end(1)
  let applicable = false
  state.doc.nodesBetween(from, to, (node, pos) => {
    if (!node.isTextblock) return false
    if (!applicable && node.type.name == "note") applicable = true
  })
  if (!applicable) return false
  if (dispatch)
    dispatch(state.tr.wrap(range, [{type: state.schema.nodes.notegroup}]))
  return true
}
```

----------------------------------------

TITLE: Importing ProseMirror State Module - CommonJS
DESCRIPTION: Demonstrates how to import and initialize the EditorState module using CommonJS require syntax. Creates a new editor state instance with a schema configuration.
SOURCE: https://github.com/prosemirror/website/blob/master/markdown/ref_intro.md#2025-04-23_snippet_0

LANGUAGE: javascript
CODE:
```
var EditorState = require("prosemirror-state").EditorState
var state = EditorState.create({schema: mySchema})
```

----------------------------------------

TITLE: Creating Custom Schema with Dinosaur Node in JavaScript
DESCRIPTION: This snippet extends the basic schema with the custom dinosaur node. It then uses this schema to parse HTML content into a ProseMirror document.
SOURCE: https://github.com/prosemirror/website/blob/master/pages/examples/dino/index.md#2025-04-23_snippet_1

LANGUAGE: javascript
CODE:
```
import {Schema} from "prosemirror-model"
import {schema} from "prosemirror-schema-basic"

const dinoSchema = new Schema({
  nodes: schema.spec.nodes.addBefore("image", "dino", dinoNodeSpec),
  marks: schema.spec.marks
})

let content = document.querySelector("#content")
let doc = DOMParser.fromSchema(dinoSchema).parse(content)
```

----------------------------------------

TITLE: Importing ProseMirror State Module - ES6
DESCRIPTION: Shows how to import and initialize the EditorState module using ES6 import syntax. Creates a new editor state instance with a schema configuration.
SOURCE: https://github.com/prosemirror/website/blob/master/markdown/ref_intro.md#2025-04-23_snippet_1

LANGUAGE: javascript
CODE:
```
import {EditorState} from "prosemirror-state"
let state = EditorState.create({schema: mySchema})
```

----------------------------------------

TITLE: Implementing Foldable Node View in ProseMirror
DESCRIPTION: This code creates a node view that displays sections with a foldable interface. It shows a header with a button that toggles folding, and responds to decorations with the 'foldSection' property to hide or show content.
SOURCE: https://github.com/prosemirror/website/blob/master/pages/examples/fold/index.md#2025-04-23_snippet_1

LANGUAGE: javascript
CODE:
```
PART(nodeview)
```

----------------------------------------

TITLE: Handling Key Events in CodeMirror Node View
DESCRIPTION: This keymap handles cursor movement across the edges of the inner editor, as well as undo, redo, and ctrl-enter actions. It allows the user to move the selection out of the code editor when appropriate.
SOURCE: https://github.com/prosemirror/website/blob/master/pages/examples/codemirror/index.md#2025-04-23_snippet_3

LANGUAGE: javascript
CODE:
```
PART(nodeview_keymap)
```

----------------------------------------

TITLE: Defining Node DOM Representation in ProseMirror Schema
DESCRIPTION: Example schema definition with toDOM field for a paragraph node. The toDOM function returns an array describing how to render the node as a p element with a hole (0) for its content.
SOURCE: https://github.com/prosemirror/website/blob/master/markdown/guide/schema.md#2025-04-23_snippet_4

LANGUAGE: javascript
CODE:
```
const schema = new Schema({
  nodes: {
    doc: {content: "paragraph+"},
    paragraph: {
      content: "text*",
      toDOM(node) { return ["p", 0] }
    },
    text: {}
  }
})
```

----------------------------------------

TITLE: Styling ProseMirror Box Components with CSS
DESCRIPTION: CSS styles defining the appearance of box components used in the ProseMirror editor interface, including color, display, border-radius, padding and margin properties.
SOURCE: https://github.com/prosemirror/website/blob/master/markdown/guide/view.md#2025-04-23_snippet_0

LANGUAGE: css
CODE:
```
.box {
  color: white;
  display: inline-block;
  border-radius: 5px;
  padding: 6px 10px;
  margin: 3px 0;
  vertical-align: top;
}
```

----------------------------------------

TITLE: Implementing Dinosaur Insertion Command in JavaScript
DESCRIPTION: This snippet defines a command for inserting dinosaurs into the document. It creates a new node of type 'dino' with the specified dinosaur type and inserts it at the current selection.
SOURCE: https://github.com/prosemirror/website/blob/master/pages/examples/dino/index.md#2025-04-23_snippet_2

LANGUAGE: javascript
CODE:
```
function insertDino(type) {
  return function(state, dispatch) {
    let {$from} = state.selection, index = $from.index()
    if (!$from.parent.canReplaceWith(index, index, dinoSchema.nodes.dino))
      return false
    if (dispatch)
      dispatch(state.tr.replaceSelectionWith(dinoSchema.nodes.dino.create({type})))
    return true
  }
}
```

----------------------------------------

TITLE: HTML Example of DOM Tree Structure
DESCRIPTION: An HTML example showing how a paragraph with markup is represented as a tree in the DOM, with nested elements for strong and emphasized text.
SOURCE: https://github.com/prosemirror/website/blob/master/markdown/guide/doc.md#2025-04-23_snippet_0

LANGUAGE: html
CODE:
```
<p>This is <strong>strong text with <em>emphasis</em></strong></p>
```

----------------------------------------

TITLE: Configuring Star Schema Keymap in ProseMirror
DESCRIPTION: This snippet defines a custom keymap for the star schema. It includes commands for toggling the 'shouting' mark, inserting a star node, and toggling links with a URL prompt.
SOURCE: https://github.com/prosemirror/website/blob/master/pages/examples/schema/index.md#2025-04-23_snippet_4

LANGUAGE: javascript
CODE:
```
const starKeymap = keymap({
  "Mod-b": toggleMark(schema.marks.shouting),
  "Mod-l": toggleLink,
  "Mod-Space": insertStar
})
```

----------------------------------------

TITLE: Implementing Purple Text Decoration Plugin in ProseMirror
DESCRIPTION: Creates a ProseMirror plugin that applies purple color styling to all text in the document using inline decorations. Demonstrates basic decoration creation and application.
SOURCE: https://github.com/prosemirror/website/blob/master/markdown/guide/view.md#2025-04-23_snippet_4

LANGUAGE: javascript
CODE:
```
let purplePlugin = new Plugin({
  props: {
    decorations(state) {
      return DecorationSet.create(state.doc, [
        Decoration.inline(0, state.doc.content.size, {style: "color: purple"})
      ])
    }
  }
})
```

----------------------------------------

TITLE: Implementing Fix Utilities for ProseMirror Linter
DESCRIPTION: Helper functions that create fix commands for document linting issues. These include functions to delete ranges of text, set attributes, and insert nodes, which are used by the linter to automatically correct detected problems.
SOURCE: https://github.com/prosemirror/website/blob/master/pages/examples/lint/index.md#2025-04-23_snippet_1

LANGUAGE: javascript
CODE:
```
function deleteRange(from, to) {
  return view => {
    let tr = view.state.tr.delete(from, to)
    view.dispatch(tr)
  }
}

function addAlt(pos) {
  return view => {
    let alt = prompt("Alt text", "")
    if (alt)
      view.dispatch(view.state.tr.setNodeMarkup(pos, null, {alt, src: view.state.doc.nodeAt(pos).attrs.src}))
  }
}
```

----------------------------------------

TITLE: Creating Speckled Background Decoration Plugin in ProseMirror
DESCRIPTION: Implements a plugin that adds yellow background decorations at regular intervals throughout the document. Shows how to maintain decoration state and map it through document changes.
SOURCE: https://github.com/prosemirror/website/blob/master/markdown/guide/view.md#2025-04-23_snippet_5

LANGUAGE: javascript
CODE:
```
let specklePlugin = new Plugin({
  state: {
    init(_, {doc}) {
      let speckles = []
      for (let pos = 1; pos < doc.content.size; pos += 4)
        speckles.push(Decoration.inline(pos - 1, pos, {style: "background: yellow"}))
      return DecorationSet.create(doc, speckles)
    },
    apply(tr, set) { return set.map(tr.mapping, tr.doc) }
  },
  props: {
    decorations(state) { return specklePlugin.getState(state) }
  }
})
```

----------------------------------------

TITLE: Creating Dinosaur Menu Items in JavaScript
DESCRIPTION: This snippet creates menu items for inserting different types of dinosaurs. It uses the insertDino command and defines icons and labels for each dinosaur type.
SOURCE: https://github.com/prosemirror/website/blob/master/pages/examples/dino/index.md#2025-04-23_snippet_3

LANGUAGE: javascript
CODE:
```
import {MenuItem} from "prosemirror-menu"

const dinoItem = new MenuItem({
  title: "Insert dinosaur",
  label: "Dino",
  enable(state) { return insertDino("brontosaurus")(state) },
  run: insertDino("brontosaurus")
})

const dinoItems = new MenuItemGroup([
  new MenuItem({title: "Brontosaurus", label: "🦕", select: "dino", attrs: {type: "brontosaurus"}}),
  new MenuItem({title: "Stegosaurus", label: "🦖", select: "dino", attrs: {type: "stegosaurus"}}),
  new MenuItem({title: "Tyrannosaurus", label: "🦖", select: "dino", attrs: {type: "tyrannosaurus"}}),
  new MenuItem({title: "Pterodactyl", label: "🦅", select: "dino", attrs: {type: "pterodactyl"}})
])
```

----------------------------------------

TITLE: Using Transform Mapping in JavaScript
DESCRIPTION: This example demonstrates how to use the mapping accumulated by a Transform object to map positions through multiple steps, including the use of bias.
SOURCE: https://github.com/prosemirror/website/blob/master/markdown/guide/transform.md#2025-04-23_snippet_3

LANGUAGE: javascript
CODE:
```
let tr = new Transform(myDoc)
tr.split(10)    // split a node, +2 tokens at 10
tr.delete(2, 5) // -3 tokens at 2
console.log(tr.mapping.map(15)) // → 14
console.log(tr.mapping.map(6))  // → 3
console.log(tr.mapping.map(10)) // → 9

console.log(tr.mapping.map(10, -1)) // → 7
```

----------------------------------------

TITLE: Opening Footnote Sub-Editor in ProseMirror
DESCRIPTION: Implements the openInner method to create a sub-editor for the footnote content. This includes setting up DOM elements, creating a new EditorView, and handling key bindings.
SOURCE: https://github.com/prosemirror/website/blob/master/pages/examples/footnote/index.md#2025-04-23_snippet_2

LANGUAGE: javascript
CODE:
```
openInner() {
  let tooltip = this.dom.appendChild(document.createElement("div"))
  tooltip.className = "footnote-tooltip"
  this.innerView = new EditorView(tooltip, {
    state: EditorState.create({
      doc: this.node.content,
      plugins: [keymap({
        "Mod-z": () => undo(this.outerView.state, this.outerView.dispatch),
        "Mod-y": () => redo(this.outerView.state, this.outerView.dispatch)
      })]
    }),
    dispatchTransaction: this.dispatchInner.bind(this),
    handleDOMEvents: {
      mousedown: () => {
        if (this.outerView.hasFocus()) this.innerView.focus()
      }
    }
  })
  tooltip.addEventListener("mousedown", e => e.preventDefault())
  return () => tooltip.remove()
}
```

----------------------------------------

TITLE: Creating a ProseMirror Plugin with Size Limit
DESCRIPTION: Example of creating a ProseMirror plugin that implements a maximum size limit through the props system.
SOURCE: https://github.com/prosemirror/website/blob/master/markdown/guide/view.md#2025-04-23_snippet_3

LANGUAGE: javascript
CODE:
```
function maxSizePlugin(max) {
  return new Plugin({
    props: {
      editable(state) { return state.doc.content.size < max }
    }
  })
}
```

----------------------------------------

TITLE: Using Transaction Metadata in ProseMirror Plugins (JavaScript)
DESCRIPTION: Demonstrates how to use transaction metadata to mark transactions for special handling by plugins. The example shows a transaction counter that ignores transactions marked with specific metadata.
SOURCE: https://github.com/prosemirror/website/blob/master/markdown/guide/state.md#2025-04-23_snippet_5

LANGUAGE: javascript
CODE:
```
let transactionCounter = new Plugin({
  state: {
    init() { return 0 },
    apply(tr, value) {
      if (tr.getMeta(transactionCounter)) return value
      else return value + 1
    }
  }
})

function markAsUncounted(tr) {
  tr.setMeta(transactionCounter, true)
}
```

----------------------------------------

TITLE: Implementing insertStar Command for Star Schema in ProseMirror
DESCRIPTION: This snippet defines a custom command for inserting a star node in the star schema. It checks if a star can be inserted at the current position and replaces the selection with a new star node if possible.
SOURCE: https://github.com/prosemirror/website/blob/master/pages/examples/schema/index.md#2025-04-23_snippet_6

LANGUAGE: javascript
CODE:
```
function insertStar(state, dispatch) {
  let type = state.schema.nodes.star
  let {$from} = state.selection
  if (!$from.parent.canReplaceWith($from.index(), $from.index(), type)) return false
  dispatch(state.tr.replaceSelectionWith(type.create()))
  return true
}
```

----------------------------------------

TITLE: Finding Placeholder Position in ProseMirror Document
DESCRIPTION: A utility function that finds the current position of a placeholder decoration by its ID. Returns null if the placeholder no longer exists in the document.
SOURCE: https://github.com/prosemirror/website/blob/master/pages/examples/upload/index.md#2025-04-23_snippet_1

LANGUAGE: javascript
CODE:
```
function findPlaceholder(state, id) {
  let decos = placeholderPlugin.getState(state)
  let found = decos.find(null, null, spec => spec.id == id)
  return found.length ? found[0].from : null
}
```

----------------------------------------

TITLE: Embedding ProseMirror Collaborative Editor in HTML
DESCRIPTION: This HTML snippet embeds the ProseMirror collaborative editor example into the page. It uses a custom HTML tag '@HTML' to indicate where the editor should be inserted.
SOURCE: https://github.com/prosemirror/website/blob/master/pages/examples/collab/index.md#2025-04-23_snippet_0

LANGUAGE: HTML
CODE:
```
@HTML
```

----------------------------------------

TITLE: Defining Mark Parsing Rules in ProseMirror Schema
DESCRIPTION: Example of parseDOM rules for an emphasis mark. These rules define how to recognize emphasis in pasted content by matching em tags, i tags, and inline font-style CSS.
SOURCE: https://github.com/prosemirror/website/blob/master/markdown/guide/schema.md#2025-04-23_snippet_5

LANGUAGE: javascript
CODE:
```
  parseDOM: [
    {tag: "em"},                 // Match <em> nodes
    {tag: "i"},                  // and <i> nodes
    {style: "font-style=italic"} // and inline 'font-style: italic'
  ]
```

----------------------------------------

TITLE: Running ProseMirror Development Server in Bash
DESCRIPTION: Command to start a development server for the ProseMirror website on port 8888, which serves files from public/ directory and includes the collaborative editing backend.
SOURCE: https://github.com/prosemirror/website/blob/master/README.md#2025-04-23_snippet_2

LANGUAGE: bash
CODE:
```
npm run devserver -- --port 8888
```

----------------------------------------

TITLE: Building ProseMirror Website Documentation in Bash
DESCRIPTION: Command to build the ProseMirror documentation and demo JavaScript bundles using make, which populates the public/ directory with the website files.
SOURCE: https://github.com/prosemirror/website/blob/master/README.md#2025-04-23_snippet_1

LANGUAGE: bash
CODE:
```
make
```

----------------------------------------

TITLE: Installing Dependencies for ProseMirror Website in Bash
DESCRIPTION: Command to install all Node.js dependencies for the ProseMirror website project using npm.
SOURCE: https://github.com/prosemirror/website/blob/master/README.md#2025-04-23_snippet_0

LANGUAGE: bash
CODE:
```
npm install
```