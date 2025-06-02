TITLE: Reading Context in a React Component with `use` Hook
DESCRIPTION: This snippet demonstrates how to read a context value within a functional component using the `use` Hook. It imports `use` from 'react' and then calls `use(ThemeContext)` to retrieve the current theme value from the closest `ThemeContext.Provider` above it in the component tree.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/reference/react/use.md#_snippet_2

LANGUAGE: JavaScript
CODE:
```
import { use } from 'react';

function Button() {
  const theme = use(ThemeContext);
  // ... 
```

----------------------------------------

TITLE: Implementing Optimistic UI with useOptimistic Hook in React
DESCRIPTION: This snippet demonstrates how to use the `useOptimistic` hook to immediately update the UI with an optimistic value (`newName`) while an asynchronous data mutation (`updateName`) is in progress. If the update succeeds, the UI remains updated; if it fails, it automatically reverts to the original `currentName`. It requires `currentName` and an `onUpdateName` callback.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/blog/2024/12/05/react-19.md#_snippet_6

LANGUAGE: JavaScript
CODE:
```
function ChangeName({currentName, onUpdateName}) {
  const [optimisticName, setOptimisticName] = useOptimistic(currentName);

  const submitAction = async formData => {
    const newName = formData.get("name");
    setOptimisticName(newName);
    const updatedName = await updateName(newName);
    onUpdateName(updatedName);
  };

  return (
    <form action={submitAction}>
      <p>Your name is: {optimisticName}</p>
      <p>
        <label>Change Name:</label>
        <input
          type="text"
          name="name"
          disabled={currentName !== optimisticName}
        />
      </p>
    </form>
  );
}
```

----------------------------------------

TITLE: Using Server Function with useActionState (JS)
DESCRIPTION: A client component `UpdateName` that uses the `useActionState` hook to manage the state, action handler, and pending status when calling the `updateName` server function. The form's `action` prop is set to the action handler returned by `useActionState`.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/reference/rsc/server-functions.md#_snippet_7

LANGUAGE: js
CODE:
```
"use client";

import {updateName} from './actions';

function UpdateName() {
  const [state, submitAction, isPending] = useActionState(updateName, {error: null});

  return (
    <form action={submitAction}>
      <input type="text" name="name" disabled={isPending}/>
      {state.error && <span>Failed: {state.error}</span>}
    </form>
  );
}
```

----------------------------------------

TITLE: Complete React Task Management Application
DESCRIPTION: This comprehensive Sandpack demonstrates a full-fledged React task management application. It showcases the integration of `useReducer` for complex state logic and `useContext` for global state distribution, providing a clear example of a well-structured React application with centralized state management.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/scaling-up-with-reducer-and-context.md#_snippet_33

LANGUAGE: js
CODE:
```
import AddTask from './AddTask.js';
import TaskList from './TaskList.js';
import { TasksProvider } from './TasksContext.js';

export default function TaskApp() {
  return (
    <TasksProvider>
      <h1>Day off in Kyoto</h1>
      <AddTask />
      <TaskList />
    </TasksProvider>
  );
}
```

LANGUAGE: js
CODE:
```
import { createContext, useContext, useReducer } from 'react';

const TasksContext = createContext(null);

const TasksDispatchContext = createContext(null);

export function TasksProvider({ children }) {
  const [tasks, dispatch] = useReducer(
    tasksReducer,
    initialTasks
  );

  return (
    <TasksContext.Provider value={tasks}>
      <TasksDispatchContext.Provider value={dispatch}>
        {children}
      </TasksDispatchContext.Provider>
    </TasksContext.Provider>
  );
}

export function useTasks() {
  return useContext(TasksContext);
}

export function useTasksDispatch() {
  return useContext(TasksDispatchContext);
}

function tasksReducer(tasks, action) {
  switch (action.type) {
    case 'added': {
      return [...tasks, {
        id: action.id,
        text: action.text,
        done: false
      }];
    }
    case 'changed': {
      return tasks.map(t => {
        if (t.id === action.task.id) {
          return action.task;
        } else {
          return t;
        }
      });
    }
    case 'deleted': {
      return tasks.filter(t => t.id !== action.id);
    }
    default: {
      throw Error('Unknown action: ' + action.type);
    }
  }
}

const initialTasks = [
  { id: 0, text: 'Philosopher’s Path', done: true },
  { id: 1, text: 'Visit the temple', done: false },
  { id: 2, text: 'Drink matcha', done: false }
];
```

LANGUAGE: js
CODE:
```
import { useState } from 'react';
import { useTasksDispatch } from './TasksContext.js';

export default function AddTask() {
  const [text, setText] = useState('');
  const dispatch = useTasksDispatch();
  return (
    <>
      <input
        placeholder="Add task"
        value={text}
        onChange={e => setText(e.target.value)}
      />
      <button onClick={() => {
        setText('');
        dispatch({
          type: 'added',
          id: nextId++,
          text: text,
        }); 
      }}>Add</button>
    </>
  );
}

let nextId = 3;
```

LANGUAGE: js
CODE:
```
import { useState } from 'react';
import { useTasks, useTasksDispatch } from './TasksContext.js';

export default function TaskList() {
  const tasks = useTasks();
  return (
    <ul>
      {tasks.map(task => (
        <li key={task.id}>
          <Task task={task} />
        </li>
      ))}
    </ul>
  );
}

function Task({ task }) {
  const [isEditing, setIsEditing] = useState(false);
  const dispatch = useTasksDispatch();
  let taskContent;
  if (isEditing) {
    taskContent = (
      <>
        <input
          value={task.text}
          onChange={e => {
            dispatch({
              type: 'changed',
              task: {
                ...task,
                text: e.target.value
              }
            });
          }} />
        <button onClick={() => setIsEditing(false)}>
          Save
        </button>
      </>
    );
  } else {
    taskContent = (
      <>
        {task.text}
        <button onClick={() => setIsEditing(true)}>
          Edit
        </button>
      </>
    );
  }
  return (
    <label>
      <input
        type="checkbox"
        checked={task.done}
        onChange={e => {
          dispatch({
            type: 'changed',
            task: {
              ...task,
              done: e.target.checked
            }
          });
        }}
      />
      {taskContent}
      <button onClick={() => {
        dispatch({
          type: 'deleted',
          id: task.id
        });
      }}>
        Delete
      </button>
    </label>
  );
}
```

LANGUAGE: css
CODE:
```
button { margin: 5px; }
li { list-style-type: none; }
ul, li { margin: 0; padding: 0; }
```

----------------------------------------

TITLE: Complete React Tic-Tac-Toe Game with State Management
DESCRIPTION: This comprehensive JavaScript snippet provides the full implementation of a React Tic-Tac-Toe game. It includes `Square`, `Board`, and `Game` components, demonstrating state management with `useState` for `xIsNext`, `history`, and `currentSquares`. It also defines `handleClick` for game logic, `handlePlay` for updating history, and `calculateWinner` for determining the game's outcome. This snippet forms the core logic for the game, including the initial setup for time travel.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/tutorial-tic-tac-toe.md#_snippet_74

LANGUAGE: js
CODE:
```
import { useState } from 'react';

function Square({ value, onSquareClick }) {
  return (
    <button className="square" onClick={onSquareClick}>
      {value}
    </button>
  );
}

function Board({ xIsNext, squares, onPlay }) {
  function handleClick(i) {
    if (calculateWinner(squares) || squares[i]) {
      return;
    }
    const nextSquares = squares.slice();
    if (xIsNext) {
      nextSquares[i] = 'X';
    } else {
      nextSquares[i] = 'O';
    }
    onPlay(nextSquares);
  }

  const winner = calculateWinner(squares);
  let status;
  if (winner) {
    status = 'Winner: ' + winner;
  } else {
    status = 'Next player: ' + (xIsNext ? 'X' : 'O');
  }

  return (
    <>
      <div className="status">{status}</div>
      <div className="board-row">
        <Square value={squares[0]} onSquareClick={() => handleClick(0)} />
        <Square value={squares[1]} onSquareClick={() => handleClick(1)} />
        <Square value={squares[2]} onSquareClick={() => handleClick(2)} />
      </div>
      <div className="board-row">
        <Square value={squares[3]} onSquareClick={() => handleClick(3)} />
        <Square value={squares[4]} onSquareClick={() => handleClick(4)} />
        <Square value={squares[5]} onSquareClick={() => handleClick(5)} />
      </div>
      <div className="board-row">
        <Square value={squares[6]} onSquareClick={() => handleClick(6)} />
        <Square value={squares[7]} onSquareClick={() => handleClick(7)} />
        <Square value={squares[8]} onSquareClick={() => handleClick(8)} />
      </div>
    </>
  );
}

export default function Game() {
  const [xIsNext, setXIsNext] = useState(true);
  const [history, setHistory] = useState([Array(9).fill(null)]);
  const currentSquares = history[history.length - 1];

  function handlePlay(nextSquares) {
    setHistory([...history, nextSquares]);
    setXIsNext(!xIsNext);
  }

  function jumpTo(nextMove) {
    // TODO
  }

  const moves = history.map((squares, move) => {
    let description;
    if (move > 0) {
      description = 'Go to move #' + move;
    } else {
      description = 'Go to game start';
    }
    return (
      <li key={move}>
        <button onClick={() => jumpTo(move)}>{description}</button>
      </li>
    );
  });

  return (
    <div className="game">
      <div className="game-board">
        <Board xIsNext={xIsNext} squares={currentSquares} onPlay={handlePlay} />
      </div>
      <div className="game-info">
        <ol>{moves}</ol>
      </div>
    </div>
  );
}

function calculateWinner(squares) {
  const lines = [
    [0, 1, 2],
    [3, 4, 5],
    [6, 7, 8],
    [0, 3, 6],
    [1, 4, 7],
    [2, 5, 8],
    [0, 4, 8],
    [2, 4, 6],
  ];
  for (let i = 0; i < lines.length; i++) {
    const [a, b, c] = lines[i];
    if (squares[a] && squares[a] === squares[b] && squares[a] === squares[c]) {
      return squares[a];
    }
  }
  return null;
}
```

----------------------------------------

TITLE: Lifting State Up for Accordion Components in React
DESCRIPTION: This React example demonstrates the 'lifting state up' pattern to manage the active panel in an Accordion component. The 'activeIndex' state is managed by the parent 'Accordion' component, which then passes 'isActive' and 'onShow' props to its child 'Panel' components, ensuring only one panel is open at a time.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/managing-state.md#_snippet_4

LANGUAGE: js
CODE:
```
import { useState } from 'react';

export default function Accordion() {
  const [activeIndex, setActiveIndex] = useState(0);
  return (
    <>
      <h2>Almaty, Kazakhstan</h2>
      <Panel
        title="About"
        isActive={activeIndex === 0}
        onShow={() => setActiveIndex(0)}
      >
        With a population of about 2 million, Almaty is Kazakhstan's largest city. From 1929 to 1997, it was its capital city.
      </Panel>
      <Panel
        title="Etymology"
        isActive={activeIndex === 1}
        onShow={() => setActiveIndex(1)}
      >
        The name comes from <span lang="kk-KZ">алма</span>, the Kazakh word for "apple" and is often translated as "full of apples". In fact, the region surrounding Almaty is thought to be the ancestral home of the apple, and the wild <i lang="la">Malus sieversii</i> is considered a likely candidate for the ancestor of the modern domestic apple.
      </Panel>
    </>
  );
}

function Panel({
  title,
  children,
  isActive,
  onShow
}) {
  return (
    <section className="panel">
      <h3>{title}</h3>
      {isActive ? (
        <p>{children}</p>
      ) : (
        <button onClick={onShow}>
          Show
        </button>
      )}
    </section>
  );
}
```

LANGUAGE: css
CODE:
```
h3, p { margin: 5px 0px; }
.panel {
  padding: 10px;
  border: 1px solid #aaa;
}
```

----------------------------------------

TITLE: Demonstrating Unoptimized Re-renders in React (JavaScript)
DESCRIPTION: This JavaScript component, `FriendList`, illustrates a common re-rendering scenario where `<MessageButton>` unnecessarily re-renders whenever `FriendList`'s state changes. React Compiler automatically optimizes this by memoizing the relevant parts, preventing redundant re-renders without manual `useMemo` or `React.memo`.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/react-compiler.md#_snippet_2

LANGUAGE: javascript
CODE:
```
function FriendList({ friends }) {
  const onlineCount = useFriendOnlineCount();
  if (friends.length === 0) {
    return <NoFriends />;
  }
  return (
    <div>
      <span>{onlineCount} online</span>
      {friends.map((friend) => (
        <FriendListCard key={friend.id} friend={friend} />
      ))}
      <MessageButton />
    </div>
  );
}
```

----------------------------------------

TITLE: Suspense-enabled Router Example (App.js)
DESCRIPTION: The main application component demonstrating a simplified router implementation using useState for page state and useTransition for navigation. It also includes Suspense for handling loading states and renders different page components based on the current URL.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/reference/react/useTransition.md#_snippet_30

LANGUAGE: javascript
CODE:
```
import { Suspense, useState, useTransition } from 'react';
import IndexPage from './IndexPage.js';
import ArtistPage from './ArtistPage.js';
import Layout from './Layout.js';

export default function App() {
  return (
    <Suspense fallback={<BigSpinner />}>
      <Router />
    </Suspense>
  );
}

function Router() {
  const [page, setPage] = useState('/');
  const [isPending, startTransition] = useTransition();

  function navigate(url) {
    startTransition(() => {
      setPage(url);
    });
  }

  let content;
  if (page === '/') {
    content = (
      <IndexPage navigate={navigate} />
    );
  } else if (page === '/the-beatles') {
    content = (
      <ArtistPage
        artist={{
          id: 'the-beatles',
          name: 'The Beatles',
        }}
      />
    );
  } else {
    content = <h2>Not Found</h2>;
  }
  return (
    <Layout isPending={isPending}>
      {content}
    </Layout>
  );
}

function BigSpinner() {
  return <h2>🌀 Loading...</h2>;
}
```

----------------------------------------

TITLE: Immutable Array Update using `map` and Object Spread in React
DESCRIPTION: This code demonstrates the correct, immutable way to update an object within an array in React state. It uses the `map` method to iterate over the array, creating a *new* object for the item that needs updating using the object spread syntax (`{ ...artwork, seen: nextSeen }`), and returning existing objects unchanged. This prevents direct mutation and ensures state isolation.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/updating-arrays-in-state.md#_snippet_13

LANGUAGE: javascript
CODE:
```
setMyList(myList.map(artwork => {
  if (artwork.id === artworkId) {
    // Create a *new* object with changes
    return { ...artwork, seen: nextSeen };
  } else {
    // No changes
    return artwork;
  }
}));
```

----------------------------------------

TITLE: Simplifying Form Submission with React 19 Actions and useActionState
DESCRIPTION: This React component demonstrates how to use `useActionState` with a `<form>` element to handle asynchronous data submission. It manages pending state, displays errors, and redirects upon successful name update, simplifying form handling in React 19.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/blog/2024/12/05/react-19.md#_snippet_2

LANGUAGE: JavaScript
CODE:
```
// Using <form> Actions and useActionState
function ChangeName({ name, setName }) {
  const [error, submitAction, isPending] = useActionState(
    async (previousState, formData) => {
      const error = await updateName(formData.get("name"));
      if (error) {
        return error;
      }
      redirect("/path");
      return null;
    },
    null,
  );

  return (
    <form action={submitAction}>
      <input type="text" name="name" />
      <button type="submit" disabled={isPending}>Update</button>
      {error && <p>{error}</p>}
    </form>
  );
}
```

----------------------------------------

TITLE: Applying CSS Class with className in React JSX
DESCRIPTION: This snippet demonstrates the basic usage of the `className` attribute in React JSX to assign a CSS class to an HTML element, functioning identically to the `class` attribute in standard HTML.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/reference/react-dom/components/common.md#_snippet_22

LANGUAGE: js
CODE:
```
<img className="avatar" />
```

----------------------------------------

TITLE: Basic React Functional Component Example
DESCRIPTION: This JavaScript snippet demonstrates a fundamental React functional component. It defines a `Greeting` component that accepts a `name` prop and renders an `h1` element. The `App` component then renders an instance of `Greeting` with a default 'world' value, showcasing basic component composition and rendering.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/installation.md#_snippet_0

LANGUAGE: javascript
CODE:
```
function Greeting({ name }) {
  return <h1>Hello, {name}</h1>;
}

export default function App() {
  return <Greeting name="world" />
}
```

----------------------------------------

TITLE: Declaring State with useState in React
DESCRIPTION: This JavaScript snippet demonstrates the declaration of a state variable `index` and its corresponding setter function `setIndex` using the `useState` Hook. The initial value of `index` is set to `0`, allowing the component to remember and update this value over time.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/state-a-components-memory.md#_snippet_10

LANGUAGE: JavaScript
CODE:
```
const [index, setIndex] = useState(0);
```

----------------------------------------

TITLE: Correct Event Handler Attachment in React (Passing Reference)
DESCRIPTION: This snippet demonstrates the correct way to attach an event handler in React by passing a reference to the function (`onClick={handleClick}`). This ensures that the `handleClick` function is executed only when the button is clicked, allowing the light switch functionality to work as intended.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/responding-to-events.md#_snippet_17

LANGUAGE: JavaScript
CODE:
```
export default function LightSwitch() {
  function handleClick() {
    let bodyStyle = document.body.style;
    if (bodyStyle.backgroundColor === 'black') {
      bodyStyle.backgroundColor = 'white';
    } else {
      bodyStyle.backgroundColor = 'black';
    }
  }

  return (
    <button onClick={handleClick}>
      Toggle the lights
    </button>
  );
}
```

----------------------------------------

TITLE: Complete Interactive React Tic-Tac-Toe App (JavaScript)
DESCRIPTION: This comprehensive snippet presents the full `App.js` file, combining the `Square` component with state management and event handling, and the `Board` component rendering multiple `Square` instances. It demonstrates a complete, interactive Tic-Tac-Toe board where squares display 'X' upon click.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/tutorial-tic-tac-toe.md#_snippet_25

LANGUAGE: js
CODE:
```
import { useState } from 'react';

function Square() {
  const [value, setValue] = useState(null);

  function handleClick() {
    setValue('X');
  }

  return (
    <button
      className="square"
      onClick={handleClick}
    >
      {value}
    </button>
  );
}

export default function Board() {
  return (
    <>
      <div className="board-row">
        <Square />
        <Square />
        <Square />
      </div>
      <div className="board-row">
        <Square />
        <Square />
        <Square />
      </div>
      <div className="board-row">
        <Square />
        <Square />
        <Square />
      </div>
    </>
  );
}
```

----------------------------------------

TITLE: Initial React Todo App: Direct State Mutations (JavaScript)
DESCRIPTION: This snippet showcases a React todo application where state updates are performed by directly mutating the `todos` array. This approach, while seemingly straightforward, leads to issues with React's rendering and state management. It includes the main `App.js` component, auxiliary components (`AddTodo.js`, `TaskList.js`), styling (`CSS`), and project dependencies (`package.json`).
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/updating-arrays-in-state.md#_snippet_29

LANGUAGE: javascript
CODE:
```
import { useState } from 'react';
import { useImmer } from 'use-immer';
import AddTodo from './AddTodo.js';
import TaskList from './TaskList.js';

let nextId = 3;
const initialTodos = [
  { id: 0, title: 'Buy milk', done: true },
  { id: 1, title: 'Eat tacos', done: false },
  { id: 2, title: 'Brew tea', done: false },
];

export default function TaskApp() {
  const [todos, setTodos] = useState(
    initialTodos
  );

  function handleAddTodo(title) {
    todos.push({
      id: nextId++,
      title: title,
      done: false
    });
  }

  function handleChangeTodo(nextTodo) {
    const todo = todos.find(t =>
      t.id === nextTodo.id
    );
    todo.title = nextTodo.title;
    todo.done = nextTodo.done;
  }

  function handleDeleteTodo(todoId) {
    const index = todos.findIndex(t =>
      t.id === todoId
    );
    todos.splice(index, 1);
  }

  return (
    <>
      <AddTodo
        onAddTodo={handleAddTodo}
      />
      <TaskList
        todos={todos}
        onChangeTodo={handleChangeTodo}
        onDeleteTodo={handleDeleteTodo}
      />
    </>
  );
}
```

LANGUAGE: javascript
CODE:
```
import { useState } from 'react';

export default function AddTodo({ onAddTodo }) {
  const [title, setTitle] = useState('');
  return (
    <>
      <input
        placeholder="Add todo"
        value={title}
        onChange={e => setTitle(e.target.value)}
      />
      <button onClick={() => {
        setTitle('');
        onAddTodo(title);
      }}>Add</button>
    </>
  )
}
```

LANGUAGE: javascript
CODE:
```
import { useState } => 'react';

export default function TaskList({
  todos,
  onChangeTodo,
  onDeleteTodo
}) {
  return (
    <ul>
      {todos.map(todo => (
        <li key={todo.id}>
          <Task
            todo={todo}
            onChange={onChangeTodo}
            onDelete={onDeleteTodo}
          />
        </li>
      ))}
    </ul>
  );
}

function Task({ todo, onChange, onDelete }) {
  const [isEditing, setIsEditing] = useState(false);
  let todoContent;
  if (isEditing) {
    todoContent = (
      <>
        <input
          value={todo.title}
          onChange={e => {
            onChange({
              ...todo,
              title: e.target.value
            });
          }} />
        <button onClick={() => setIsEditing(false)}>
          Save
        </button>
      </>
    );
  } else {
    todoContent = (
      <>
        {todo.title}
        <button onClick={() => setIsEditing(true)}>
          Edit
        </button>
      </>
    );
  }
  return (
    <label>
      <input
        type="checkbox"
        checked={todo.done}
        onChange={e => {
          onChange({
            ...todo,
            done: e.target.checked
          });
        }}
      />
      {todoContent}
      <button onClick={() => onDelete(todo.id)}>
        Delete
      </button>
    </label>
  );
}
```

LANGUAGE: css
CODE:
```
button { margin: 5px; }
li { list-style-type: none; }
ul, li { margin: 0; padding: 0; }
```

LANGUAGE: json
CODE:
```
{
  "dependencies": {
    "immer": "1.7.3",
    "react": "latest",
    "react-dom": "latest",
    "react-scripts": "latest",
    "use-immer": "0.5.1"
  },
  "scripts": {
    "start": "react-scripts start",
    "build": "react-scripts build",
    "test": "react-scripts test --env=jsdom",
    "eject": "react-scripts eject"
  }
}
```

----------------------------------------

TITLE: Synchronizing Inputs with Lifted State in React
DESCRIPTION: This snippet provides the solution for synchronizing two input fields by applying the 'lifting state up' pattern. The shared 'text' state and its 'handleChange' function are moved to the common parent component, which then passes them down as props to ensure both child inputs display and update the same value.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/sharing-state-between-components.md#_snippet_10

LANGUAGE: JavaScript
CODE:
```
import { useState } from 'react';

export default function SyncedInputs() {
  const [text, setText] = useState('');

  function handleChange(e) {
    setText(e.target.value);
  }

  return (
    <>
      <Input
        label="First input"
        value={text}
        onChange={handleChange}
      />
      <Input
        label="Second input"
        value={text}
        onChange={handleChange}
      />
    </>
  );
}

function Input({ label, value, onChange }) {
  return (
    <label>
      {label}
      {' '}
      <input
        value={value}
        onChange={onChange}
      />
    </label>
  );
}
```

LANGUAGE: CSS
CODE:
```
input { margin: 5px; }
label { display: block; }
```

----------------------------------------

TITLE: Integrating React Actions with HTML Forms
DESCRIPTION: This snippet demonstrates how to use the `action` prop on a standard HTML `<form/>` element in React. The `action` prop accepts a function, `search` in this case, which will be executed when the form is submitted. React manages the submission lifecycle, allowing the `search` function to be defined on either the client or server.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/blog/2024/02/15/react-labs-what-we-have-been-working-on-february-2024.md#_snippet_0

LANGUAGE: JSX
CODE:
```
<form action={search}>
  <input name="query" />
  <button type="submit">Search</button>
</form>
```

----------------------------------------

TITLE: Handling User Events with React Components (JavaScript)
DESCRIPTION: This snippet demonstrates how to define and pass event handlers as props through a component hierarchy in React. It shows an `App` component rendering a `Toolbar`, which in turn renders `Button` components, each triggering a different alert on click. It illustrates the pattern of passing functions down as `onClick` props.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/adding-interactivity.md#_snippet_0

LANGUAGE: javascript
CODE:
```
export default function App() {
  return (
    <Toolbar
      onPlayMovie={() => alert('Playing!')}
      onUploadImage={() => alert('Uploading!')}
    />
  );
}

function Toolbar({ onPlayMovie, onUploadImage }) {
  return (
    <div>
      <Button onClick={onPlayMovie}>
        Play Movie
      </Button>
      <Button onClick={onUploadImage}>
        Upload Image
      </Button>
    </div>
  );
}

function Button({ onClick, children }) {
  return (
    <button onClick={onClick}>
      {children}
    </button>
  );
}
```

----------------------------------------

TITLE: Optimized TodoList Component without Effects - React
DESCRIPTION: This section provides the optimized version of the React `TodoList` component. It demonstrates how to eliminate redundant state and `useEffect` hooks by calculating derived data (such as `activeTodos` and `visibleTodos`) directly during the rendering phase. The footer JSX is also moved inline, illustrating a cleaner and more efficient approach to data transformation in React components.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/you-might-not-need-an-effect.md#_snippet_33

LANGUAGE: javascript
CODE:
```
import { useState } from 'react';
import { initialTodos, createTodo } from './todos.js';

export default function TodoList() {
  const [todos, setTodos] = useState(initialTodos);
  const [showActive, setShowActive] = useState(false);
  const activeTodos = todos.filter(todo => !todo.completed);
  const visibleTodos = showActive ? activeTodos : todos;

  return (
    <>
      <label>
        <input
          type="checkbox"
          checked={showActive}
          onChange={e => setShowActive(e.target.checked)}
        />
        Show only active todos
      </label>
      <NewTodo onAdd={newTodo => setTodos([...todos, newTodo])} />
      <ul>
        {visibleTodos.map(todo => (
          <li key={todo.id}>
            {todo.completed ? <s>{todo.text}</s> : todo.text}
          </li>
        ))}
      </ul>
      <footer>
        {activeTodos.length} todos left
      </footer>
    </>
  );
}

function NewTodo({ onAdd }) {
  const [text, setText] = useState('');

  function handleAddClick() {
    setText('');
    onAdd(createTodo(text));
  }

  return (
    <>
      <input value={text} onChange={e => setText(e.target.value)} />
      <button onClick={handleAddClick}>
        Add
      </button>
    </>
  );
}
```

LANGUAGE: javascript
CODE:
```
let nextId = 0;

export function createTodo(text, completed = false) {
  return {
    id: nextId++,
    text,
    completed
  };
}

export const initialTodos = [
  createTodo('Get apples', true),
  createTodo('Get oranges', true),
  createTodo('Get carrots'),
];
```

LANGUAGE: css
CODE:
```
label { display: block; }
input { margin-top: 10px; }
```

----------------------------------------

TITLE: Implementing Stuck Form Inputs with Plain Variables in React
DESCRIPTION: This React component demonstrates a common issue where form input values do not update because plain JavaScript variables (`firstName`, `lastName`) are used instead of React state. These variables do not persist their values across re-renders, leading to 'stuck' inputs. The `onChange` handlers attempt to update these variables, but the UI does not reflect the changes.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/state-a-components-memory.md#_snippet_31

LANGUAGE: JavaScript
CODE:
```
export default function Form() {
  let firstName = '';
  let lastName = '';

  function handleFirstNameChange(e) {
    firstName = e.target.value;
  }

  function handleLastNameChange(e) {
    lastName = e.target.value;
  }

  function handleReset() {
    firstName = '';
    lastName = '';
  }

  return (
    <form onSubmit={e => e.preventDefault()}>
      <input
        placeholder="First name"
        value={firstName}
        onChange={handleFirstNameChange}
      />
      <input
        placeholder="Last name"
        value={lastName}
        onChange={handleLastNameChange}
      />
      <h1>Hi, {firstName} {lastName}</h1>
      <button onClick={handleReset}>Reset</button>
    </form>
  );
}
```

LANGUAGE: CSS
CODE:
```
h1 { margin-top: 10px; }
```

----------------------------------------

TITLE: Calling Component Functions Directly (Bad Practice) - React JavaScript
DESCRIPTION: This snippet illustrates an incorrect practice where a React component function is called directly as a regular JavaScript function. This bypasses React's rendering mechanism, can lead to violations of the Rules of Hooks, and prevents React from applying its optimizations and features like local state.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/reference/rules/react-calls-components-and-hooks.md#_snippet_1

LANGUAGE: JavaScript
CODE:
```
function BlogPost() {
  return <Layout>{Article()}</Layout>; // 🔴 Bad: Never call them directly
}
```

----------------------------------------

TITLE: Defining `useWindowListener` Custom Hook in React
DESCRIPTION: This custom React Hook provides a reusable way to attach and detach event listeners to the `window` object. It uses `useEffect` to add the listener when the component mounts or `eventType`/`listener` changes, and removes it during cleanup to prevent memory leaks.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/reference/react/useEffect.md#_snippet_16

LANGUAGE: JavaScript
CODE:
```
import { useState, useEffect } from 'react';

export function useWindowListener(eventType, listener) {
  useEffect(() => {
    window.addEventListener(eventType, listener);
    return () => {
      window.removeEventListener(eventType, listener);
    };
  }, [eventType, listener]);
}
```

----------------------------------------

TITLE: Connecting Chat with Dynamic Server URL and Room ID in React
DESCRIPTION: This React component demonstrates a `useEffect` hook that connects to a chat server. The effect re-runs whenever `serverUrl` or `roomId` changes, ensuring the chat connection is re-established with the updated parameters. `message` is not a dependency, so editing it does not trigger a re-connection.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/reference/react/useEffect.md#_snippet_29

LANGUAGE: JavaScript
CODE:
```
import { useState, useEffect } from 'react';
import { createConnection } from './chat.js';

function ChatRoom({ roomId }) {
  const [serverUrl, setServerUrl] = useState('https://localhost:1234');
  const [message, setMessage] = useState('');

  useEffect(() => {
    const connection = createConnection(serverUrl, roomId);
    connection.connect();
    return () => {
      connection.disconnect();
    };
  }, [serverUrl, roomId]);

  return (
    <>
      <label>
        Server URL:{' '}
        <input
          value={serverUrl}
          onChange={e => setServerUrl(e.target.value)}
        />
      </label>
      <h1>Welcome to the {roomId} room!</h1>
      <label>
        Your message:{' '}
        <input value={message} onChange={e => setMessage(e.target.value)} />
      </label>
    </>
  );
}

export default function App() {
  const [show, setShow] = useState(false);
  const [roomId, setRoomId] = useState('general');
  return (
    <>
      <label>
        Choose the chat room:{' '}
        <select
          value={roomId}
          onChange={e => setRoomId(e.target.value)}
        >
          <option value="general">general</option>
          <option value="travel">travel</option>
          <option value="music">music</option>
        </select>
        <button onClick={() => setShow(!show)}>
          {show ? 'Close chat' : 'Open chat'}
        </button>
      </label>
      {show && <hr />}
      {show && <ChatRoom roomId={roomId}/>}
    </>
  );
}
```

----------------------------------------

TITLE: Implementing TasksProvider Component
DESCRIPTION: This `TasksProvider` component serves as a central hub for task state management. It uses `useReducer` to manage tasks and provides both the `tasks` state and the `dispatch` function through `TasksContext.Provider` and `TasksDispatchContext.Provider` respectively, making them accessible to all child components.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/scaling-up-with-reducer-and-context.md#_snippet_25

LANGUAGE: js
CODE:
```
export function TasksProvider({ children }) {
  const [tasks, dispatch] = useReducer(tasksReducer, initialTasks);

  return (
    <TasksContext.Provider value={tasks}>
      <TasksDispatchContext.Provider value={dispatch}>
        {children}
      </TasksDispatchContext.Provider>
    </TasksContext.Provider>
  );
}
```

----------------------------------------

TITLE: Updating React State with Updater Function
DESCRIPTION: This example demonstrates how passing an updater function to `setAge` ensures correct state updates, even when multiple `increment` calls are batched within a single event handler. The updater function receives the latest state value, preventing issues with stale closures.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/reference/react/useState.md#_snippet_12

LANGUAGE: JavaScript
CODE:
```
import { useState } from 'react';

export default function Counter() {
  const [age, setAge] = useState(42);

  function increment() {
    setAge(a => a + 1);
  }

  return (
    <>
      <h1>Your age: {age}</h1>
      <button onClick={() => {
        increment();
        increment();
        increment();
      }}>+3</button>
      <button onClick={() => {
        increment();
      }}>+1</button>
    </>
  );
}
```

LANGUAGE: CSS
CODE:
```
button { display: block; margin: 10px; font-size: 20px; }
h1 { display: block; margin: 10px; }
```

----------------------------------------

TITLE: Using `use` with Promises and Context in React Components
DESCRIPTION: Illustrates how to import and utilize the `use` API within a React component to read values from both a Promise (e.g., `messagePromise`) and a React Context (e.g., `ThemeContext`). This demonstrates its application for asynchronous data and shared state, integrating with Suspense.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/reference/react/use.md#_snippet_1

LANGUAGE: JSX
CODE:
```
import { use } from 'react';

function MessageComponent({ messagePromise }) {
  const message = use(messagePromise);
  const theme = use(ThemeContext);
  // ...
```

----------------------------------------

TITLE: Displaying JSX Content in a React Root
DESCRIPTION: This snippet illustrates the `root.render` method's role in displaying any React node, typically JSX, into the browser DOM node managed by the React root. It's the primary way to update the UI after the root is established.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/reference/react-dom/client/createRoot.md#_snippet_3

LANGUAGE: js
CODE:
```
root.render(<App />);
```

----------------------------------------

TITLE: Implementing Chat Room Effect with useEffectEvent for Prop - React
DESCRIPTION: This snippet uses the experimental useEffectEvent hook to wrap the 'onReceiveMessage' prop function. This allows the effect to call the latest version of the prop function via the effect event ('onMessage') without needing 'onReceiveMessage' itself in the dependency array, preventing unnecessary re-connections.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/removing-effect-dependencies.md#_snippet_24

LANGUAGE: javascript
CODE:
```
function ChatRoom({ roomId, onReceiveMessage }) {
  const [messages, setMessages] = useState([]);

  const onMessage = useEffectEvent(receivedMessage => {
    onReceiveMessage(receivedMessage);
  });

  useEffect(() => {
    const connection = createConnection();
    connection.connect();
    connection.on('message', (receivedMessage) => {
      onMessage(receivedMessage);
    });
    return () => connection.disconnect();
  }, [roomId]); // ✅ All dependencies declared
  // ...
}
```

----------------------------------------

TITLE: Complete React Tic-Tac-Toe Game with Time Travel
DESCRIPTION: This comprehensive JavaScript snippet provides the full implementation of a React Tic-Tac-Toe game, including components for `Square`, `Board`, and `Game`. It incorporates state management for game history, current move tracking, and functions like `handlePlay` and `jumpTo` to enable "time travel" through past game states. It also includes the `calculateWinner` utility.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/tutorial-tic-tac-toe.md#_snippet_80

LANGUAGE: JavaScript
CODE:
```
import { useState } from 'react';

function Square({value, onSquareClick}) {
  return (
    <button className="square" onClick={onSquareClick}>
      {value}
    </button>
  );
}

function Board({ xIsNext, squares, onPlay }) {
  function handleClick(i) {
    if (calculateWinner(squares) || squares[i]) {
      return;
    }
    const nextSquares = squares.slice();
    if (xIsNext) {
      nextSquares[i] = 'X';
    } else {
      nextSquares[i] = 'O';
    }
    onPlay(nextSquares);
  }

  const winner = calculateWinner(squares);
  let status;
  if (winner) {
    status = 'Winner: ' + winner;
  } else {
    status = 'Next player: ' + (xIsNext ? 'X' : 'O');
  }

  return (
    <>
      <div className="status">{status}</div>
      <div className="board-row">
        <Square value={squares[0]} onSquareClick={() => handleClick(0)} />
        <Square value={squares[1]} onSquareClick={() => handleClick(1)} />
        <Square value={squares[2]} onSquareClick={() => handleClick(2)} />
      </div>
      <div className="board-row">
        <Square value={squares[3]} onSquareClick={() => handleClick(3)} />
        <Square value={squares[4]} onSquareClick={() => handleClick(4)} />
        <Square value={squares[5]} onSquareClick={() => handleClick(5)} />
      </div>
      <div className="board-row">
        <Square value={squares[6]} onSquareClick={() => handleClick(6)} />
        <Square value={squares[7]} onSquareClick={() => handleClick(7)} />
        <Square value={squares[8]} onSquareClick={() => handleClick(8)} />
      </div >
    </>
  );
}

export default function Game() {
  const [xIsNext, setXIsNext] = useState(true);
  const [history, setHistory] = useState([Array(9).fill(null)]);
  const [currentMove, setCurrentMove] = useState(0);
  const currentSquares = history[currentMove];

  function handlePlay(nextSquares) {
    const nextHistory = [...history.slice(0, currentMove + 1), nextSquares];
    setHistory(nextHistory);
    setCurrentMove(nextHistory.length - 1);
    setXIsNext(!xIsNext);
  }

  function jumpTo(nextMove) {
    setCurrentMove(nextMove);
    setXIsNext(nextMove % 2 === 0);
  }

  const moves = history.map((squares, move) => {
    let description;
    if (move > 0) {
      description = 'Go to move #' + move;
    } else {
      description = 'Go to game start';
    }
    return (
      <li key={move}>
        <button onClick={() => jumpTo(move)}>{description}</button>
      </li>
    );
  });

  return (
    <div className="game">
      <div className="game-board">
        <Board xIsNext={xIsNext} squares={currentSquares} onPlay={handlePlay} />
      </div>
      <div className="game-info">
        <ol>{moves}</ol>
      </div>
    </div>
  );
}

function calculateWinner(squares) {
  const lines = [
    [0, 1, 2],
    [3, 4, 5],
    [6, 7, 8],
    [0, 3, 6],
    [1, 4, 7],
    [2, 5, 8],
    [0, 4, 8],
    [2, 4, 6],
  ];
  for (let i = 0; i < lines.length; i++) {
    const [a, b, c] = lines[i];
    if (squares[a] && squares[a] === squares[b] && squares[a] === squares[c]) {
      return squares[a];
    }
  }
  return null;
}
```

----------------------------------------

TITLE: Rendering a List with Unique Keys in React (App.js)
DESCRIPTION: This React component (`List`) maps over an array of `people` to render a list of items. Each `<li>` element is assigned a unique `key` using `person.id`, which is a best practice for list rendering in React to ensure correct and efficient updates when the list changes.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/rendering-lists.md#_snippet_17

LANGUAGE: JavaScript
CODE:
```
import { people } from './data.js';
import { getImageUrl } from './utils.js';

export default function List() {
  const listItems = people.map(person =>
    <li key={person.id}>
      <img
        src={getImageUrl(person)}
        alt={person.name}
      />
      <p>
        <b>{person.name}</b>
          {' ' + person.profession + ' '}
          known for {person.accomplishment}
      </p>
    </li>
  );
  return <ul>{listItems}</ul>;
}
```

----------------------------------------

TITLE: Corrected Non-Mutative Todo Management in React
DESCRIPTION: This set of React components provides the corrected implementation of the todo application, refactoring the state update functions (handleAddTodo, handleChangeTodo, handleDeleteTodo) to use non-mutative array methods. handleAddTodo uses array spread syntax, handleChangeTodo uses Array.prototype.map(), and handleDeleteTodo uses Array.prototype.filter(), ensuring that new array references are created for each state update, which React's useState can correctly detect and trigger re-renders.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/updating-arrays-in-state.md#_snippet_25

LANGUAGE: JavaScript
CODE:
```
import { useState } from 'react';
import AddTodo from './AddTodo.js';
import TaskList from './TaskList.js';

let nextId = 3;
const initialTodos = [
  { id: 0, title: 'Buy milk', done: true },
  { id: 1, title: 'Eat tacos', done: false },
  { id: 2, title: 'Brew tea', done: false },
];

export default function TaskApp() {
  const [todos, setTodos] = useState(
    initialTodos
  );

  function handleAddTodo(title) {
    setTodos([
      ...todos,
      {
        id: nextId++,
        title: title,
        done: false
      }
    ]);
  }

  function handleChangeTodo(nextTodo) {
    setTodos(todos.map(t => {
      if (t.id === nextTodo.id) {
        return nextTodo;
      } else {
        return t;
      }
    }));
  }

  function handleDeleteTodo(todoId) {
    setTodos(
      todos.filter(t => t.id !== todoId)
    );
  }

  return (
    <>
      <AddTodo
        onAddTodo={handleAddTodo}
      />
      <TaskList
        todos={todos}
        onChangeTodo={handleChangeTodo}
        onDeleteTodo={handleDeleteTodo}
      />
    </>
  );
}
```

LANGUAGE: JavaScript
CODE:
```
import { useState } from 'react';

export default function AddTodo({ onAddTodo }) {
  const [title, setTitle] = useState('');
  return (
    <>
      <input
        placeholder="Add todo"
        value={title}
        onChange={e => setTitle(e.target.value)}
      />
      <button onClick={() => {
        setTitle('');
        onAddTodo(title);
      }}>Add</button>
    </>
  )
}
```

----------------------------------------

TITLE: Complete Tic-Tac-Toe Game Board Component - React JavaScript
DESCRIPTION: This comprehensive React component (`Board`) implements the full Tic-Tac-Toe game logic. It manages game state using `useState` hooks for `xIsNext` and `squares`, handles user clicks with `handleClick`, determines the winner using `calculateWinner`, and dynamically displays the game status and board squares.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/tutorial-tic-tac-toe.md#_snippet_54

LANGUAGE: JavaScript
CODE:
```
import { useState } from 'react';

function Square({value, onSquareClick}) {
  return (
    <button className="square" onClick={onSquareClick}>
      {value}
    </button>
  );
}

export default function Board() {
  const [xIsNext, setXIsNext] = useState(true);
  const [squares, setSquares] = useState(Array(9).fill(null));

  function handleClick(i) {
    if (calculateWinner(squares) || squares[i]) {
      return;
    }
    const nextSquares = squares.slice();
    if (xIsNext) {
      nextSquares[i] = 'X';
    } else {
      nextSquares[i] = 'O';
    }
    setSquares(nextSquares);
    setXIsNext(!xIsNext);
  }

  const winner = calculateWinner(squares);
  let status;
  if (winner) {
    status = 'Winner: ' + winner;
  } else {
    status = 'Next player: ' + (xIsNext ? 'X' : 'O');
  }

  return (
    <>
      <div className="status">{status}</div>
      <div className="board-row">
        <Square value={squares[0]} onSquareClick={() => handleClick(0)} />
        <Square value={squares[1]} onSquareClick={() => handleClick(1)} />
        <Square value={squares[2]} onSquareClick={() => handleClick(2)} />
      </div>
      <div className="board-row">
        <Square value={squares[3]} onSquareClick={() => handleClick(3)} />
        <Square value={squares[4]} onSquareClick={() => handleClick(4)} />
        <Square value={squares[5]} onSquareClick={() => handleClick(5)} />
      </div>
      <div className="board-row">
        <Square value={squares[6]} onSquareClick={() => handleClick(6)} />
        <Square value={squares[7]} onSquareClick={() => handleClick(7)} />
        <Square value={squares[8]} onSquareClick={() => handleClick(8)} />
      </div>
    </>
  );
}

function calculateWinner(squares) {
  const lines = [
    [0, 1, 2],
    [3, 4, 5],
    [6, 7, 8],
    [0, 3, 6],
    [1, 4, 7],
    [2, 5, 8],
    [0, 4, 8],
    [2, 4, 6]
  ];
  for (let i = 0; i < lines.length; i++) {
    const [a, b, c] = lines[i];
    if (squares[a] && squares[a] === squares[b] && squares[a] === squares[c]) {
      return squares[a];
    }
  }
  return null;
}
```

----------------------------------------

TITLE: Rendering a List of Scientists in React
DESCRIPTION: This React component demonstrates how to render a dynamic list of `person` objects using `map()` to create `<li>` elements. Each list item includes an image, name, profession, and accomplishment, with a unique `key` prop derived from `person.id` for efficient list reconciliation. It imports `people` data and `getImageUrl` utility.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/describing-the-ui.md#_snippet_7

LANGUAGE: JavaScript
CODE:
```
import { people } from './data.js';
import { getImageUrl } from './utils.js';

export default function List() {
  const listItems = people.map(person =>
    <li key={person.id}>
      <img
        src={getImageUrl(person)}
        alt={person.name}
      />
      <p>
        <b>{person.name}:</b>
        {' ' + person.profession + ' '}
        known for {person.accomplishment}
      </p>
    </li>
  );
  return (
    <article>
      <h1>Scientists</h1>
      <ul>{listItems}</ul>
    </article>
  );
}
```

----------------------------------------

TITLE: Importing and Exporting React Components Across Files - JavaScript
DESCRIPTION: This example illustrates how to organize React components into separate files for better maintainability. `Profile.js` exports the `Profile` component, `Gallery.js` imports `Profile` and exports `Gallery`, and `App.js` imports `Gallery` to render the main application. This modular approach promotes code reusability and readability.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/describing-the-ui.md#_snippet_1

LANGUAGE: JavaScript
CODE:
```
import Gallery from './Gallery.js';

export default function App() {
  return (
    <Gallery />
  );
}
```

LANGUAGE: JavaScript
CODE:
```
import Profile from './Profile.js';

export default function Gallery() {
  return (
    <section>
      <h1>Amazing scientists</h1>
      <Profile />
      <Profile />
      <Profile />
    </section>
  );
}
```

LANGUAGE: JavaScript
CODE:
```
export default function Profile() {
  return (
    <img
      src="https://i.imgur.com/QIrZWGIs.jpg"
      alt="Alan L. Hart"
    />
  );
}
```

LANGUAGE: CSS
CODE:
```
img { margin: 0 10px 10px 0; }
```

----------------------------------------

TITLE: Marking Client Code with 'use client' (JavaScript)
DESCRIPTION: The 'use client' directive is placed at the top of a file or function to instruct React and compatible bundlers that the code within this scope should be executed on the client side. This is essential for components that rely on browser APIs, state, or effects.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/reference/rsc/directives.md#_snippet_0

LANGUAGE: JavaScript
CODE:
```
'use client'
```

----------------------------------------

TITLE: Memoizing Expensive Calculations with React Compiler (JavaScript)
DESCRIPTION: This JavaScript example demonstrates how the React Compiler memoizes expensive function calls (`expensivelyProcessAReallyLargeArrayOfObjects`) when they occur within a React component or hook (`TableContainer`). It highlights that the compiler does not memoize standalone functions or share memoization across different components, suggesting external memoization for widely used expensive functions.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/react-compiler.md#_snippet_3

LANGUAGE: js
CODE:
```
// **Not** memoized by React Compiler, since this is not a component or hook
function expensivelyProcessAReallyLargeArrayOfObjects() { /* ... */ }

// Memoized by React Compiler since this is a component
function TableContainer({ items }) {
  // This function call would be memoized:
  const data = expensivelyProcessAReallyLargeArrayOfObjects(items);
  // ...
}
```

----------------------------------------

TITLE: Implementing Optimistic Form Updates with useOptimistic - React/JavaScript
DESCRIPTION: Demonstrates a full implementation of optimistic UI updates using `useOptimistic` for sending messages via a form. It shows how to update the UI immediately upon submission (`addOptimisticMessage`), reset the form, perform the asynchronous action (`sendMessageAction`) using `startTransition`, and manage the state with `useState`. Requires `react`, `useOptimistic`, `useState`, `useRef`, `startTransition`, and a message delivery action.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/reference/react/useOptimistic.md#_snippet_2

LANGUAGE: js
CODE:
```
import { useOptimistic, useState, useRef, startTransition } from "react";
import { deliverMessage } from "./actions.js";

function Thread({ messages, sendMessageAction }) {
  const formRef = useRef();
  function formAction(formData) {
    addOptimisticMessage(formData.get("message"));
    formRef.current.reset();
    startTransition(async () => {
      await sendMessageAction(formData);
    });
  }
  const [optimisticMessages, addOptimisticMessage] = useOptimistic(
    messages,
    (state, newMessage) => [
      {
        text: newMessage,
        sending: true
      },
      ...state,
    ]
  );

  return (
    <>
      <form action={formAction} ref={formRef}>
        <input type="text" name="message" placeholder="Hello!" />
        <button type="submit">Send</button>
      </form>
      {optimisticMessages.map((message, index) => (
        <div key={index}>
          {message.text}
          {!!message.sending && <small> (Sending...)</small>}
        </div>
      ))}
      
    </>
  );
}

export default function App() {
  const [messages, setMessages] = useState([
    { text: "Hello there!", sending: false, key: 1 }
  ]);
  async function sendMessageAction(formData) {
    const sentMessage = await deliverMessage(formData.get("message"));
    startTransition(() => {
      setMessages((messages) => [{ text: sentMessage }, ...messages]);
    })
  }
  return <Thread messages={messages} sendMessageAction={sendMessageAction} />;
}
```

----------------------------------------

TITLE: Incorrect Hook Usage Patterns in React - JavaScript
DESCRIPTION: This collection of examples illustrates various incorrect ways to use React Hooks, violating the rule that Hooks must be called at the top level of a function component or custom Hook. It shows Hooks being called inside conditions, loops, after conditional returns, within event handlers, inside useMemo, in class components, and within try/catch/finally blocks, all of which lead to errors and unpredictable behavior.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/reference/rules/rules-of-hooks.md#_snippet_1

LANGUAGE: JavaScript
CODE:
```
function Bad({ cond }) {
  if (cond) {
    // 🔴 Bad: inside a condition (to fix, move it outside!)
    const theme = useContext(ThemeContext);
  }
  // ...
}

function Bad() {
  for (let i = 0; i < 10; i++) {
    // 🔴 Bad: inside a loop (to fix, move it outside!)
    const theme = useContext(ThemeContext);
  }
  // ...
}

function Bad({ cond }) {
  if (cond) {
    return;
  }
  // 🔴 Bad: after a conditional return (to fix, move it before the return!)
  const theme = useContext(ThemeContext);
  // ...
}

function Bad() {
  function handleClick() {
    // 🔴 Bad: inside an event handler (to fix, move it outside!)
    const theme = useContext(ThemeContext);
  }
  // ...
}

function Bad() {
  const style = useMemo(() => {
    // 🔴 Bad: inside useMemo (to fix, move it outside!)
    const theme = useContext(ThemeContext);
    return createStyle(theme);
  });
  // ...
}

class Bad extends React.Component {
  render() {
    // 🔴 Bad: inside a class component (to fix, write a function component instead of a class!)
    useEffect(() => {})
    // ...
  }
}

function Bad() {
  try {
    // 🔴 Bad: inside try/catch/finally block (to fix, move it outside!)
    const [x, setX] = useState(0);
  } catch {
    const [x, setX] = useState(1);
  }
}
```

----------------------------------------

TITLE: Implementing Tic-Tac-Toe Game Logic - React - JavaScript
DESCRIPTION: This JavaScript code defines the core components and logic for a React Tic-Tac-Toe game. It includes `Square` for individual cells, `Board` for rendering the game grid and handling clicks, and `Game` for managing the overall game state (history, current player). The `calculateWinner` function determines if there's a winner.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/tutorial-tic-tac-toe.md#_snippet_65

LANGUAGE: js
CODE:
```
import { useState } from 'react';

function Square({ value, onSquareClick }) {
  return (
    <button className="square" onClick={onSquareClick}>
      {value}
    </button>
  );
}

function Board({ xIsNext, squares, onPlay }) {
  function handleClick(i) {
    if (calculateWinner(squares) || squares[i]) {
      return;
    }
    const nextSquares = squares.slice();
    if (xIsNext) {
      nextSquares[i] = 'X';
    } else {
      nextSquares[i] = 'O';
    }
    onPlay(nextSquares);
  }

  const winner = calculateWinner(squares);
  let status;
  if (winner) {
    status = 'Winner: ' + winner;
  } else {
    status = 'Next player: ' + (xIsNext ? 'X' : 'O');
  }

  return (
    <>
      <div className="status">{status}</div>
      <div className="board-row">
        <Square value={squares[0]} onSquareClick={() => handleClick(0)} />
        <Square value={squares[1]} onSquareClick={() => handleClick(1)} />
        <Square value={squares[2]} onSquareClick={() => handleClick(2)} />
      </div>
      <div className="board-row">
        <Square value={squares[3]} onSquareClick={() => handleClick(3)} />
        <Square value={squares[4]} onSquareClick={() => handleClick(4)} />
        <Square value={squares[5]} onSquareClick={() => handleClick(5)} />
      </div>
      <div className="board-row">
        <Square value={squares[6]} onSquareClick={() => handleClick(6)} />
        <Square value={squares[7]} onSquareClick={() => handleClick(7)} />
        <Square value={squares[8]} onSquareClick={() => handleClick(8)} />
      </div>
    </>
  );
}

export default function Game() {
  const [xIsNext, setXIsNext] = useState(true);
  const [history, setHistory] = useState([Array(9).fill(null)]);
  const currentSquares = history[history.length - 1];

  function handlePlay(nextSquares) {
    setHistory([...history, nextSquares]);
    setXIsNext(!xIsNext);
  }

  return (
    <div className="game">
      <div className="game-board">
        <Board xIsNext={xIsNext} squares={currentSquares} onPlay={handlePlay} />
      </div>
      <div className="game-info">
        <ol>{/*TODO*/}</ol>
      </div>
    </div>
  );
}

function calculateWinner(squares) {
  const lines = [
    [0, 1, 2],
    [3, 4, 5],
    [6, 7, 8],
    [0, 3, 6],
    [1, 4, 7],
    [2, 5, 8],
    [0, 4, 8],
    [2, 4, 6],
  ];
  for (let i = 0; i < lines.length; i++) {
    const [a, b, c] = lines[i];
    if (squares[a] && squares[a] === squares[b] && squares[a] === squares[c]) {
      return squares[a];
    }
  }
  return null;
}
```

----------------------------------------

TITLE: Creating a Basic React Component - JavaScript
DESCRIPTION: This snippet demonstrates how to define a simple React component. React components are standard JavaScript functions that return JSX markup, which describes the UI. This example creates a 'MyButton' component that renders a basic HTML button.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/index.md#_snippet_0

LANGUAGE: JavaScript
CODE:
```
function MyButton() {
  return (
    <button>I'm a button</button>
  );
}
```

----------------------------------------

TITLE: Initial Rendering of a React Component
DESCRIPTION: After creating a React root, this snippet shows how to use the `root.render()` method to display the initial React component, such as `<App />`, within the root's DOM node. This action makes the React application visible.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/reference/react-dom/client/createRoot.md#_snippet_2

LANGUAGE: js
CODE:
```
root.render(<App />);
```

----------------------------------------

TITLE: Complete Filterable Product Table React Application
DESCRIPTION: This comprehensive React application demonstrates state management and data flow for a filterable product table. It includes components like `FilterableProductTable` (managing `filterText` and `inStockOnly` state), `SearchBar` (displaying search input and checkbox), `ProductTable` (filtering and rendering products), `ProductCategoryRow`, and `ProductRow`. The example showcases how state is lifted to a common parent and passed down via props to control UI updates, providing a complete functional example of a filtered list.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/thinking-in-react.md#_snippet_5

LANGUAGE: JSX
CODE:
```
import { useState } from 'react';

function FilterableProductTable({ products }) {
  const [filterText, setFilterText] = useState('');
  const [inStockOnly, setInStockOnly] = useState(false);

  return (
    <div>
      <SearchBar 
        filterText={filterText} 
        inStockOnly={inStockOnly} />
      <ProductTable 
        products={products}
        filterText={filterText}
        inStockOnly={inStockOnly} />
    </div>
  );
}

function ProductCategoryRow({ category }) {
  return (
    <tr>
      <th colSpan="2">
        {category}
      </th>
    </tr>
  );
}

function ProductRow({ product }) {
  const name = product.stocked ? product.name :
    <span style={{ color: 'red' }}>
      {product.name}
    </span>;

  return (
    <tr>
      <td>{name}</td>
      <td>{product.price}</td>
    </tr>
  );
}

function ProductTable({ products, filterText, inStockOnly }) {
  const rows = [];
  let lastCategory = null;

  products.forEach((product) => {
    if (
      product.name.toLowerCase().indexOf(
        filterText.toLowerCase()
      ) === -1
    ) {
      return;
    }
    if (inStockOnly && !product.stocked) {
      return;
    }
    if (product.category !== lastCategory) {
      rows.push(
        <ProductCategoryRow
          category={product.category}
          key={product.category} />
      );
    }
    rows.push(
      <ProductRow
        product={product}
        key={product.name} />
    );
    lastCategory = product.category;
  });

  return (
    <table>
      <thead>
        <tr>
          <th>Name</th>
          <th>Price</th>
        </tr>
      </thead>
      <tbody>{rows}</tbody>
    </table>
  );
}

function SearchBar({ filterText, inStockOnly }) {
  return (
    <form>
      <input 
        type="text" 
        value={filterText} 
        placeholder="Search..."/>
      <label>
        <input 
          type="checkbox" 
          checked={inStockOnly} />
        {' '}
        Only show products in stock
      </label>
    </form>
  );
}

const PRODUCTS = [
  {category: "Fruits", price: "$1", stocked: true, name: "Apple"},
```

----------------------------------------

TITLE: Updating React State Immutably (JavaScript)
DESCRIPTION: This JavaScript snippet demonstrates the correct way to update a state object in React by creating a *new* object and passing it to the `setPerson` state setter. It explicitly copies all existing properties (`lastName`, `email`) and overrides only the `firstName` property, ensuring immutability.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/updating-objects-in-state.md#_snippet_13

LANGUAGE: js
CODE:
```
setPerson({
  firstName: e.target.value, // New first name from the input
  lastName: person.lastName,
  email: person.email
});
```

----------------------------------------

TITLE: Incorrect React `useEffect` Dependencies in `ChatRoom` Component (JavaScript)
DESCRIPTION: This React component demonstrates an incorrect usage of `useEffect` where `roomId` (a prop) and `serverUrl` (a state variable) are used inside the effect but are not declared in its dependency array. This causes the effect to only run once on mount, leading to stale `connection` instances that do not re-synchronize when `roomId` or `serverUrl` change, resulting in a lint error and functional bug.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/lifecycle-of-reactive-effects.md#_snippet_25

LANGUAGE: javascript
CODE:
```
import { useState, useEffect } from 'react';
import { createConnection } from './chat.js';

function ChatRoom({ roomId }) { // roomId is reactive
  const [serverUrl, setServerUrl] = useState('https://localhost:1234'); // serverUrl is reactive

  useEffect(() => {
    const connection = createConnection(serverUrl, roomId);
    connection.connect();
    return () => connection.disconnect();
  }, []); // <-- Something's wrong here!

  return (
    <>
      <label>
        Server URL:{' '}
        <input
          value={serverUrl}
          onChange={e => setSetServerUrl(e.target.value)}
        />
      </label>
      <h1>Welcome to the {roomId} room!</h1>
    </>
  );
}

export default function App() {
  const [roomId, setRoomId] = useState('general');
  return (
    <>
      <label>
        Choose the chat room:{' '}
        <select
          value={roomId}
          onChange={e => setRoomId(e.target.value)}
        >
          <option value="general">general</option>
          <option value="travel">travel</option>
          <option value="music">music</option>
        </select>
      </label>
      <hr />
      <ChatRoom roomId={roomId} />
    </>
  );
}
```

----------------------------------------

TITLE: React Chat Room Component (Fixed Notification)
DESCRIPTION: The corrected implementation of the React ChatRoom component. It fixes the stale closure bug in the delayed notification by passing the specific room ID at the time the effect was triggered as a parameter to the useEffectEvent.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/separating-events-from-effects.md#_snippet_37

LANGUAGE: js
CODE:
```
import { useState, useEffect } from 'react';
import { experimental_useEffectEvent as useEffectEvent } from 'react';
import { createConnection, sendMessage } from './chat.js';
import { showNotification } from './notifications.js';

const serverUrl = 'https://localhost:1234';

function ChatRoom({ roomId, theme }) {
  // Pass roomId as a parameter to the Effect Event
  const onConnected = useEffectEvent((connectedRoomId) => {
    showNotification('Welcome to ' + connectedRoomId, theme);
  });

  useEffect(() => {
    const connection = createConnection(serverUrl, roomId);
    connection.on('connected', () => {
      setTimeout(() => {
        // Pass the current roomId to the Effect Event
        onConnected(roomId);
      }, 2000);
    });
    connection.connect();
    return () => connection.disconnect();
  }, [roomId]);

  return <h1>Welcome to the {roomId} room!</h1>
}

export default function App() {
  const [roomId, setRoomId] = useState('general');
  const [isDark, setIsDark] = useState(false);
  return (
    <>
      <label>
        Choose the chat room:{' '}
        <select
          value={roomId}
          onChange={e => setRoomId(e.target.value)}
        >
          <option value="general">general</option>
          <option value="travel">travel</option>
          <option value="music">music</option>
        </select>
      </label>
      <label>
        <input
          type="checkbox"
          checked={isDark}
          onChange={e => setIsDark(e.target.checked)}
        />
        Use dark theme
      </label>
      <hr />
      <ChatRoom
        roomId={roomId}
        theme={isDark ? 'dark' : 'light'}
      />
    </>
  );
}
```

----------------------------------------

TITLE: Calling Hooks from React Functions vs. Regular JavaScript Functions - JavaScript
DESCRIPTION: This snippet illustrates the rule that Hooks must only be called from React function components or custom Hooks, not from regular JavaScript functions. It shows a correct usage within a FriendList component and an incorrect usage within a plain setOnlineStatus function, emphasizing that stateful logic should be visible within the component's source.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/reference/rules/rules-of-hooks.md#_snippet_2

LANGUAGE: JavaScript
CODE:
```
function FriendList() {
  const [onlineStatus, setOnlineStatus] = useOnlineStatus(); // ✅
}

function setOnlineStatus() { // ❌ Not a component or custom Hook!
  const [onlineStatus, setOnlineStatus] = useOnlineStatus();
}
```

----------------------------------------

TITLE: React Root Initialization (Initial Chat Example)
DESCRIPTION: Initializes the React application by creating a root and rendering the `App` component into the DOM element with the ID 'root'. This serves as the entry point for the first demonstration of a chat application.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/reference/react/StrictMode.md#_snippet_12

LANGUAGE: javascript
CODE:
```
import { createRoot } from 'react-dom/client';
import './styles.css';

import App from './App';

const root = createRoot(document.getElementById("root"));
root.render(<App />);
```

----------------------------------------

TITLE: React Profile Editor with State and Conditional Rendering
DESCRIPTION: This snippet presents the complete React implementation of the profile editor. It utilizes React's `useState` hook to manage the editing mode (`isEditing`) and input values (`firstName`, `lastName`). Conditional rendering is employed to dynamically display either input fields or static text based on the `isEditing` state, ensuring real-time updates of the welcome message.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/reacting-to-input-with-state.md#_snippet_14

LANGUAGE: JavaScript
CODE:
```
import { useState } from 'react';

export default function EditProfile() {
  const [isEditing, setIsEditing] = useState(false);
  const [firstName, setFirstName] = useState('Jane');
  const [lastName, setLastName] = useState('Jacobs');

  return (
    <form onSubmit={e => {
      e.preventDefault();
      setIsEditing(!isEditing);
    }}>
      <label>
        First name:{' '}
        {isEditing ? (
          <input
            value={firstName}
            onChange={e => {
              setFirstName(e.target.value)
            }}
          />
        ) : (
          <b>{firstName}</b>
        )}
      </label>
      <label>
        Last name:{' '}
        {isEditing ? (
          <input
            value={lastName}
            onChange={e => {
              setLastName(e.target.value)
            }}
          />
        ) : (
          <b>{lastName}</b>
        )}
      </label>
      <button type="submit">
        {isEditing ? 'Save' : 'Edit'} Profile
      </button>
      <p><i>Hello, {firstName} {lastName}!</i></p>
    </form>
  );
}
```

LANGUAGE: CSS
CODE:
```
label { display: block; margin-bottom: 20px; }
```

----------------------------------------

TITLE: Implementing useReducer for State Management in React
DESCRIPTION: This example demonstrates how to import and use the `useReducer` Hook within a React component. It shows the definition of a `reducer` function and how `useReducer` is called with the reducer and an initial state object, returning the current `state` and a `dispatch` function for updates.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/reference/react/useReducer.md#_snippet_1

LANGUAGE: javascript
CODE:
```
import { useReducer } from 'react';

function reducer(state, action) {
  // ...
}

function MyComponent() {
  const [state, dispatch] = useReducer(reducer, { age: 42 });
  // ...
```

----------------------------------------

TITLE: Fetching Data in a React Server Component
DESCRIPTION: This Server Component demonstrates fetching data using `async`/`await`. It awaits `note` data directly on the server, which suspends rendering. A `commentsPromise` is initiated but not awaited on the server, allowing it to be passed to a Client Component for later resolution, enabling streaming with Suspense.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/reference/rsc/server-components.md#_snippet_10

LANGUAGE: JavaScript
CODE:
```
// Server Component
import db from './database';

async function Page({id}) {
  // Will suspend the Server Component.
  const note = await db.notes.get(id);
  
  // NOTE: not awaited, will start here and await on the client. 
  const commentsPromise = db.comments.get(note.id);
  return (
    <div>
      {note}
      <Suspense fallback={<p>Loading Comments...</p>}>
        <Comments commentsPromise={commentsPromise} />
      </Suspense>
    </div>
  );
}
```

----------------------------------------

TITLE: Updating State and Triggering Re-renders in React (JavaScript)
DESCRIPTION: This React component demonstrates how state updates trigger re-renders. It uses `useState` for `isSent` and `message`. The `onSubmit` handler prevents default form submission, sets `isSent` to `true` to trigger a re-render, and calls `sendMessage`. The component conditionally renders a message or a form based on the `isSent` state, showcasing how event handlers interact with a snapshot of the state.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/state-as-a-snapshot.md#_snippet_0

LANGUAGE: JavaScript
CODE:
```
import { useState } from 'react';

export default function Form() {
  const [isSent, setIsSent] = useState(false);
  const [message, setMessage] = useState('Hi!');
  if (isSent) {
    return <h1>Your message is on its way!</h1>
  }
  return (
    <form onSubmit={(e) => {
      e.preventDefault();
      setIsSent(true);
      sendMessage(message);
    }}>
      <textarea
        placeholder="Message"
        value={message}
        onChange={e => setMessage(e.target.value)}
      />
      <button type="submit">Send</button>
    </form>
  );
}

function sendMessage(message) {
  // ...
}
```

----------------------------------------

TITLE: Implementing the useOnlineStatus Custom React Hook
DESCRIPTION: Provides the implementation for the `useOnlineStatus` custom React Hook. It uses `useState` to manage the online status and `useEffect` to subscribe and unsubscribe to browser `online` and `offline` events, returning the current online status.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/reusing-logic-with-custom-hooks.md#_snippet_3

LANGUAGE: JavaScript
CODE:
```
function useOnlineStatus() {
  const [isOnline, setIsOnline] = useState(true);
  useEffect(() => {
    function handleOnline() {
      setIsOnline(true);
    }
    function handleOffline() {
      setIsOnline(false);
    }
    window.addEventListener('online', handleOnline);
    window.addEventListener('offline', handleOffline);
    return () => {
      window.removeEventListener('online', handleOnline);
      window.removeEventListener('offline', handleOffline);
    };
  }, []);
  return isOnline;
}
```

----------------------------------------

TITLE: Passing Promise from Server Component to Client Component in React
DESCRIPTION: This Server Component (`App`) fetches data asynchronously using `fetchMessage` and passes the resulting Promise directly as a prop (`messagePromise`) to the `Message` Client Component. It wraps the `Message` component in `Suspense` to display a fallback while the Promise resolves, preventing server-side rendering from blocking.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/reference/react/use.md#_snippet_7

LANGUAGE: JavaScript
CODE:
```
import { fetchMessage } from './lib.js';
import { Message } from './message.js';

export default function App() {
  const messagePromise = fetchMessage();
  return (
    <Suspense fallback={<p>waiting for message...</p>}>
      <Message messagePromise={messagePromise} />
    </Suspense>
  );
}
```

----------------------------------------

TITLE: Using `ref` as a Prop in React 19 Function Components
DESCRIPTION: In React 19, function components can now directly accept `ref` as a prop, simplifying the process of forwarding refs to DOM elements or other components. This example shows a `MyInput` function component receiving `ref` and passing it to an `<input>` element, making `forwardRef` unnecessary for this pattern. This change streamlines ref management and component design.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/blog/2024/12/05/react-19.md#_snippet_10

LANGUAGE: JavaScript
CODE:
```
function MyInput({placeholder, ref}) {
  return <input placeholder={placeholder} ref={ref} />
}

//...
<MyInput ref={ref} />
```

----------------------------------------

TITLE: Immutable Update of Nested Object State (JavaScript - Single Call)
DESCRIPTION: This JavaScript snippet shows a more concise way to immutably update a nested object property in React within a single `setPerson` call. It uses nested spread syntax to copy existing properties of both the parent and nested objects, then overrides the specific property (`city`) that needs to be updated.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/updating-objects-in-state.md#_snippet_22

LANGUAGE: js
CODE:
```
setPerson({
  ...person, // Copy other fields
  artwork: { // but replace the artwork
    ...person.artwork, // with the same one
    city: 'New Delhi' // but in New Delhi!
  }
});
```

----------------------------------------

TITLE: Calling an Effect Event from useEffect
DESCRIPTION: Shows how to call the declared Effect Event (`onConnected`) from within a `useEffect` Hook. By moving the non-reactive logic into the Effect Event, the `theme` dependency can be removed from the `useEffect` dependency array. Effect Events themselves are not reactive and should not be included in the dependency array.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/separating-events-from-effects.md#_snippet_15

LANGUAGE: js
CODE:
```
function ChatRoom({ roomId, theme }) {
  const onConnected = useEffectEvent(() => {
    showNotification('Connected!', theme);
  });

  useEffect(() => {
    const connection = createConnection(serverUrl, roomId);
    connection.on('connected', () => {
      onConnected();
    });
    connection.connect();
    return () => connection.disconnect();
  }, [roomId]); // ✅ All dependencies declared
  // ...
```

----------------------------------------

TITLE: Correct useEffect Dependency in React
DESCRIPTION: Demonstrates the correct way to declare a reactive value (`roomId`) as a dependency for a React `useEffect` hook when it is used within the effect's logic. Explains why reactive values must be included in the dependency array.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/removing-effect-dependencies.md#_snippet_4

LANGUAGE: javascript
CODE:
```
const serverUrl = 'https://localhost:1234';

function ChatRoom({ roomId }) { // This is a reactive value
  useEffect(() => {
    const connection = createConnection(serverUrl, roomId); // This Effect reads that reactive value
    connection.connect();
    return () => connection.disconnect();
  }, [roomId]); // ✅ So you must specify that reactive value as a dependency of your Effect
  // ...
}
```

----------------------------------------

TITLE: Anti-pattern: Mirroring Props in State (Incorrect)
DESCRIPTION: This snippet illustrates an anti-pattern where a state variable `color` is initialized from a prop `messageColor`. This approach is problematic because subsequent changes to `messageColor` from the parent component will not update the `color` state, leading to stale data and unexpected behavior. State is only initialized on the first render.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/choosing-the-state-structure.md#_snippet_10

LANGUAGE: JavaScript
CODE:
```
function Message({ messageColor }) {
  const [color, setColor] = useState(messageColor);

```

----------------------------------------

TITLE: Incorrect Ref Usage: Reading/Writing During React Rendering
DESCRIPTION: This snippet illustrates incorrect usage of `useRef` where `ref.current` is accessed or modified directly within the component's rendering phase. This violates React's pure function expectation for components, potentially leading to unpredictable behavior and issues with future React features.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/reference/react/useRef.md#_snippet_7

LANGUAGE: JavaScript
CODE:
```
function MyComponent() {
  // ...
  // 🚩 Don't write a ref during rendering
  myRef.current = 123;
  // ...
  // 🚩 Don't read a ref during rendering
  return <h1>{myOtherRef.current}</h1>;
}
```

----------------------------------------

TITLE: Demonstrating Asynchronous State Update Issue in React
DESCRIPTION: These lines highlight the core problem: `setTodos` queues a state update but doesn't immediately re-render the DOM. Consequently, `scrollIntoView` is called on the `listRef.current.lastChild` before the new todo item is present in the DOM, leading to incorrect scrolling.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/manipulating-the-dom-with-refs.md#_snippet_15

LANGUAGE: javascript
CODE:
```
setTodos([ ...todos, newTodo]);
listRef.current.lastChild.scrollIntoView();
```

----------------------------------------

TITLE: Correct Memoization of Object Dependency using useMemo
DESCRIPTION: This snippet shows how to correctly memoize an object dependency by wrapping the object creation itself within `useMemo`. This ensures `searchOptions` only changes when its `text` dependency changes, allowing `visibleItems` to be properly memoized.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/reference/react/useMemo.md#_snippet_32

LANGUAGE: javascript
CODE:
```
function Dropdown({ allItems, text }) {
  const searchOptions = useMemo(() => {
    return { matchMode: 'whole-word', text };
  }, [text]); // ✅ Only changes when text changes

  const visibleItems = useMemo(() => {
    return searchItems(allItems, searchOptions);
  }, [allItems, searchOptions]); // ✅ Only changes when allItems or searchOptions changes
  // ...
```

----------------------------------------

TITLE: Optimizing React useEffect with useCallback for Function Dependencies
DESCRIPTION: Demonstrates how to use `useCallback` to memoize a function (`createOptions`) that is used as a dependency in `useEffect`. By wrapping the function with `useCallback` and specifying its own dependencies (`roomId`), the function reference remains stable across renders, preventing the effect from re-firing unnecessarily.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/reference/react/useCallback.md#_snippet_21

LANGUAGE: JavaScript
CODE:
```
function ChatRoom({ roomId }) {
  const [message, setMessage] = useState('');

  const createOptions = useCallback(() => {
    return {
      serverUrl: 'https://localhost:1234',
      roomId: roomId
    };
  }, [roomId]); // ✅ Only changes when roomId changes

  useEffect(() => {
    const options = createOptions();
    const connection = createConnection(options);
    connection.connect();
    return () => connection.disconnect();
  }, [createOptions]); // ✅ Only changes when createOptions changes
  // ...

```

----------------------------------------

TITLE: Correctly Using useEffectEvent Within a Custom Hook (React)
DESCRIPTION: This example shows the recommended way to use `useEffectEvent`. The `onTick` event handler is declared and called directly within the `useTimer` hook's Effect. This adheres to the rule that Effect Events should only be called from inside Effects and not passed as dependencies or arguments to other hooks.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/separating-events-from-effects.md#_snippet_25

LANGUAGE: js
CODE:
```
function Timer() {
  const [count, setCount] = useState(0);
  useTimer(() => {
    setCount(count + 1);
  }, 1000);
  return <h1>{count}</h1>
}

function useTimer(callback, delay) {
  const onTick = useEffectEvent(() => {
    callback();
  });

  useEffect(() => {
    const id = setInterval(() => {
      onTick(); // ✅ Good: Only called locally inside an Effect
    }, delay);
    return () => {
      clearInterval(id);
    };
  }, [delay]); // No need to specify "onTick" (an Effect Event) as a dependency
}
```

----------------------------------------

TITLE: Great: Using Purpose-Driven Custom Hooks
DESCRIPTION: This example demonstrates the ideal pattern where the component logic is simplified by extracting effects into custom hooks named after their specific, high-level use cases (`useChatRoom`, `useImpressionLog`). This makes the component more declarative and readable.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/reusing-logic-with-custom-hooks.md#_snippet_48

LANGUAGE: JavaScript
CODE:
```
function ChatRoom({ roomId }) {
  const [serverUrl, setServerUrl] = useState('https://localhost:1234');

  // ✅ Great: custom Hooks named after their purpose
  useChatRoom({ serverUrl, roomId });
  useImpressionLog('visit_chat', { roomId });
  // ...
}
```

----------------------------------------

TITLE: Correct State Update with Updater Function in React
DESCRIPTION: This snippet illustrates the correct method for updating state based on its previous value in React, especially when multiple updates occur in quick succession. By passing an updater function (`a => a + 1`) to `setAge`, React queues the updates, ensuring each update uses the most recent pending state.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/reference/react/useState.md#_snippet_11

LANGUAGE: JavaScript
CODE:
```
function handleClick() {
  setAge(a => a + 1); // setAge(42 => 43)
  setAge(a => a + 1); // setAge(43 => 44)
  setAge(a => a + 1); // setAge(44 => 45)
}
```

----------------------------------------

TITLE: Updating React State with Object Spread (JavaScript)
DESCRIPTION: This JavaScript snippet shows how to immutably update a React state object using the object spread syntax (`...person`). This concise method copies all properties from the `person` object into a new object, then overrides the `firstName` property with the new value, making state updates cleaner and more scalable.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/updating-objects-in-state.md#_snippet_14

LANGUAGE: js
CODE:
```
setPerson({
  ...person, // Copy the old fields
  firstName: e.target.value // But override this one
});
```

----------------------------------------

TITLE: Updating Item Count in React Shopping Cart
DESCRIPTION: This snippet provides the solution for the `handleIncreaseClick` function, demonstrating how to immutably update the `products` state. It uses the `map` function to iterate over the products and the object spread syntax (`...`) to create a new product object with an incremented count for the matching `productId`, ensuring state updates follow React's immutability principles.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/updating-arrays-in-state.md#_snippet_19

LANGUAGE: JavaScript
CODE:
```
import { useState } from 'react';

const initialProducts = [{
  id: 0,
  name: 'Baklava',
  count: 1,
}, {
  id: 1,
  name: 'Cheese',
  count: 5,
}, {
  id: 2,
  name: 'Spaghetti',
  count: 2,
}];

export default function ShoppingCart() {
  const [
    products,
    setProducts
  ] = useState(initialProducts)

  function handleIncreaseClick(productId) {
    setProducts(products.map(product => {
      if (product.id === productId) {
        return {
          ...product,
          count: product.count + 1
        };
      } else {
        return product;
      }
    }))
  }

  return (
    <ul>
      {products.map(product => (
        <li key={product.id}>
          {product.name}
          {' '}
          (<b>{product.count}</b>)
          <button onClick={() => {
            handleIncreaseClick(product.id);
          }}>
            +
          </button>
        </li>
      ))}
</ul>
  );
}
```

LANGUAGE: CSS
CODE:
```
button { margin: 5px; }
```

----------------------------------------

TITLE: React Chat Room Component with Effect (src/ChatRoom.js)
DESCRIPTION: A React component that manages a chat connection using useEffect. It connects to a chat server based on roomId and isEncrypted props and disconnects on cleanup. It uses useEffectEvent to handle incoming messages without needing onMessage in the effect's dependency array.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/removing-effect-dependencies.md#_snippet_68

LANGUAGE: js
CODE:
```
import { useState, useEffect } from 'react';
import { experimental_useEffectEvent as useEffectEvent } from 'react';
import {
  createEncryptedConnection,
  createUnencryptedConnection,
} from './chat.js';

export default function ChatRoom({ roomId, isEncrypted, onMessage }) {
  const onReceiveMessage = useEffectEvent(onMessage);

  useEffect(() => {
    function createConnection() {
      const options = {
        serverUrl: 'https://localhost:1234',
        roomId: roomId
      };
      if (isEncrypted) {
        return createEncryptedConnection(options);
      } else {
        return createUnencryptedConnection(options);
      }
    }

    const connection = createConnection();
    connection.on('message', (msg) => onReceiveMessage(msg));
    connection.connect();
    return () => connection.disconnect();
  }, [roomId, isEncrypted]);

  return <h1>Welcome to the {roomId} room!</h1>;
}
```

----------------------------------------

TITLE: Fixing React Profile Component by Passing Props
DESCRIPTION: This snippet presents the corrected implementation of the React profile application. The fix involves removing the problematic global `currentPerson` variable. Instead, the `person` object is passed directly as a prop from `Profile` to its child components, `Header` and `Avatar`. This ensures that each component instance receives its own specific data, making the components pure and their behavior predictable, resolving the shared state bug.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/keeping-components-pure.md#_snippet_8

LANGUAGE: js
CODE:
```
import Panel from './Panel.js';
import { getImageUrl } from './utils.js';

export default function Profile({ person }) {
  return (
    <Panel>
      <Header person={person} />
      <Avatar person={person} />
    </Panel>
  )
}

function Header({ person }) {
  return <h1>{person.name}</h1>;
}

function Avatar({ person }) {
  return (
    <img
      className="avatar"
      src={getImageUrl(person)}
      alt={person.name}
      width={50}
      height={50}
    />
  );
}
```

LANGUAGE: js
CODE:
```
import { useState } from 'react';

export default function Panel({ children }) {
  const [open, setOpen] = useState(true);
  return (
    <section className="panel">
      <button onClick={() => setOpen(!open)}>
        {open ? 'Collapse' : 'Expand'}
      </button>
      {open && children}
    </section>
  );
}
```

LANGUAGE: js
CODE:
```
import Profile from './Profile.js';

export default function App() {
  return (
    <>
      <Profile person={{
        imageId: 'lrWQx8l',
        name: 'Subrahmanyan Chandrasekhar',
      }} />
      <Profile person={{
        imageId: 'MK3eW3A',
        name: 'Creola Katherine Johnson',
      }} />
    </>
  );
}
```

LANGUAGE: js
CODE:
```
export function getImageUrl(person, size = 's') {
  return (
    'https://i.imgur.com/' +
    person.imageId +
    size +
    '.jpg'
  );
}
```

LANGUAGE: css
CODE:
```
.avatar { margin: 5px; border-radius: 50%; }
.panel {
  border: 1px solid #aaa;
  border-radius: 6px;
  margin-top: 20px;
  padding: 10px;
  width: 200px;
}
h1 { margin: 5px; font-size: 18px; }
```

----------------------------------------

TITLE: Server Component Composing with Client Component in React
DESCRIPTION: This JavaScript code shows a React Server Component (`Notes`) importing and using a Client Component (`Expandable`). This pattern allows Server Components to pass data and JSX as props to Client Components, enabling interactivity while keeping data fetching on the server.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/reference/rsc/server-components.md#_snippet_7

LANGUAGE: JavaScript
CODE:
```
// Server Component
import Expandable from './Expandable';

async function Notes() {
  const notes = await db.notes.getAll();
  return (
    <div>
      {notes.map(note => (
        <Expandable key={note.id}>
          <p note={note} />
        </Expandable>
      ))}
    </div>
  );
}
```

----------------------------------------

TITLE: Understanding Asynchronous State Updates in React
DESCRIPTION: This example highlights a crucial caveat of React's `set` functions: they do not immediately update the state variable in the currently executing code. After calling `setName('Robin')`, `console.log(name)` will still output the *old* value ('Taylor') because the state update is batched and applied for the *next* render cycle.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/reference/react/useState.md#_snippet_5

LANGUAGE: js
CODE:
```
function handleClick() {
  setName('Robin');
  console.log(name); // Still "Taylor"!
}
```

----------------------------------------

TITLE: Replacing Array Items with map() in React
DESCRIPTION: This React component illustrates how to immutably replace an item in an array stored in state. It uses the `map()` method, leveraging the item's index, to increment a specific counter while leaving others unchanged. The `setCounters` function updates the state with the new array, ensuring React's immutability best practices are followed.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/updating-arrays-in-state.md#_snippet_7

LANGUAGE: javascript
CODE:
```
import { useState } from 'react';

let initialCounters = [
  0, 0, 0
];

export default function CounterList() {
  const [counters, setCounters] = useState(
    initialCounters
  );

  function handleIncrementClick(index) {
    const nextCounters = counters.map((c, i) => {
      if (i === index) {
        // Increment the clicked counter
        return c + 1;
      } else {
        // The rest haven't changed
        return c;
      }
    });
    setCounters(nextCounters);
  }

  return (
    <ul>
      {counters.map((counter, i) => (
        <li key={i}>
          {counter}
          <button onClick={() => {
            handleIncrementClick(i);
          }}>+1</button>
        </li>
      ))}
    </ul>
  );
}
```

LANGUAGE: css
CODE:
```
button { margin: 5px; }
```

----------------------------------------

TITLE: Testing Component Rendering with `act` in React
DESCRIPTION: This example demonstrates how to use `await act()` within a test to ensure that a component (`<TestComponent />`) is rendered and all associated updates are processed before making assertions about its DOM state, such as checking if a button is disabled.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/reference/react/act.md#_snippet_1

LANGUAGE: JavaScript
CODE:
```
it ('renders with button disabled', async () => {
  await act(async () => {
    root.render(<TestComponent />)
  });
  expect(container.querySelector('button')).toBeDisabled();
});
```

----------------------------------------

TITLE: Testing React Component Rendering with `act` and ReactDOMClient
DESCRIPTION: This test demonstrates how to render a React component (`Counter`) into a DOM container using `ReactDOMClient.createRoot().render()` wrapped within `await act()`. This ensures that the component is fully rendered and its effects (like updating `document.title`) are applied before assertions are made on the DOM elements and document title.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/reference/react/act.md#_snippet_3

LANGUAGE: JavaScript
CODE:
```
import {act} from 'react';
import ReactDOMClient from 'react-dom/client';
import Counter from './Counter';

it('can render and update a counter', async () => {
  container = document.createElement('div');
  document.body.appendChild(container);
  
  // ✅ Render the component inside act().
  await act(() => {
    ReactDOMClient.createRoot(container).render(<Counter />);
  });
  
  const button = container.querySelector('button');
  const label = container.querySelector('p');
  expect(label.textContent).toBe('You clicked 0 times');
  expect(document.title).toBe('You clicked 0 times');
});
```

----------------------------------------

TITLE: Calculating Derived State During React Rendering
DESCRIPTION: This snippet shows the recommended way to derive `fullName` from `firstName` and `lastName` in React. By calculating `fullName` directly during the component's render phase, it avoids redundant state and unnecessary `useEffect` calls, leading to faster, simpler, and less error-prone code. This approach ensures the derived value is always up-to-date without extra render passes.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/you-might-not-need-an-effect.md#_snippet_1

LANGUAGE: javascript
CODE:
```
function Form() {
  const [firstName, setFirstName] = useState('Taylor');
  const [lastName, setLastName] = useState('Swift');
  // ✅ Good: calculated during rendering
  const fullName = firstName + ' ' + lastName;
  // ...
}
```

----------------------------------------

TITLE: Building Static Product Table Components in React JSX
DESCRIPTION: This comprehensive JSX snippet defines a set of interconnected React functional components designed to render a static product table. It demonstrates component composition, prop passing for data flow, and conditional rendering based on product stock status. The `PRODUCTS` array serves as the immutable data source for this non-interactive UI.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/thinking-in-react.md#_snippet_1

LANGUAGE: jsx
CODE:
```
function ProductCategoryRow({ category }) {
  return (
    <tr>
      <th colSpan="2">
        {category}
      </th>
    </tr>
  );
}

function ProductRow({ product }) {
  const name = product.stocked ? product.name :
    <span style={{ color: 'red' }}>
      {product.name}
    </span>;

  return (
    <tr>
      <td>{name}</td>
      <td>{product.price}</td>
    </tr>
  );
}

function ProductTable({ products }) {
  const rows = [];
  let lastCategory = null;

  products.forEach((product) => {
    if (product.category !== lastCategory) {
      rows.push(
        <ProductCategoryRow
          category={product.category}
          key={product.category} />
      );
    }
    rows.push(
      <ProductRow
        product={product}
        key={product.name} />
    );
    lastCategory = product.category;
  });

  return (
    <table>
      <thead>
        <tr>
          <th>Name</th>
          <th>Price</th>
        </tr>
      </thead>
      <tbody>{rows}</tbody>
    </table>
  );
}

function SearchBar() {
  return (
    <form>
      <input type="text" placeholder="Search..." />
      <label>
        <input type="checkbox" />
        {' '}
        Only show products in stock
      </label>
    </form>
  );
}

function FilterableProductTable({ products }) {
  return (
    <div>
      <SearchBar />
      <ProductTable products={products} />
    </div>
  );
}

const PRODUCTS = [
  {category: "Fruits", price: "$1", stocked: true, name: "Apple"},
  {category: "Fruits", price: "$1", stocked: true, name: "Dragonfruit"},
  {category: "Fruits", price: "$2", stocked: false, name: "Passionfruit"},
  {category: "Vegetables", price: "$2", stocked: true, name: "Spinach"},
  {category: "Vegetables", price: "$4", stocked: false, name: "Pumpkin"},
  {category: "Vegetables", price: "$1", stocked: true, name: "Peas"}
];

export default function App() {
  return <FilterableProductTable products={PRODUCTS} />;
}
```

----------------------------------------

TITLE: Defining a Server Function for Actions (JS)
DESCRIPTION: Defines a server function `updateName` that can be called from the client. It takes a name, validates it, and updates the user's name in the database. It returns an error object if the name is missing.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/reference/rsc/server-functions.md#_snippet_4

LANGUAGE: js
CODE:
```
"use server";

export async function updateName(name) {
  if (!name) {
    return {error: 'Name is required'};
  }
  await db.users.updateName(name);
}
```

----------------------------------------

TITLE: Complete Accordion Component with State Management in React
DESCRIPTION: This comprehensive JavaScript snippet defines both the `Accordion` parent component and the `Panel` child component. The `Accordion` manages the `activeIndex` state, passing `isActive` and `onShow` props to `Panel` instances. The `Panel` component conditionally renders its content or a 'Show' button based on `isActive`, triggering the `onShow` handler to update the parent's state when clicked.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/sharing-state-between-components.md#_snippet_7

LANGUAGE: JavaScript
CODE:
```
import { useState } from 'react';

export default function Accordion() {
  const [activeIndex, setActiveIndex] = useState(0);
  return (
    <>
      <h2>Almaty, Kazakhstan</h2>
      <Panel
        title="About"
        isActive={activeIndex === 0}
        onShow={() => setActiveIndex(0)}
      >
        With a population of about 2 million, Almaty is Kazakhstan's largest city. From 1929 to 1997, it was its capital city.
      </Panel>
      <Panel
        title="Etymology"
        isActive={activeIndex === 1}
        onShow={() => setActiveIndex(1)}
      >
        The name comes from <span lang="kk-KZ">алма</span>, the Kazakh word for "apple" and is often translated as "full of apples". In fact, the region surrounding Almaty is thought to be the ancestral home of the apple, and the wild <i lang="la">Malus sieversii</i> is considered a likely candidate for the ancestor of the modern domestic apple.
      </Panel>
    </>
  );
}

function Panel({
  title,
  children,
  isActive,
  onShow
}) {
  return (
    <section className="panel">
      <h3>{title}</h3>
      {isActive ? (
        <p>{children}</p>
      ) : (
        <button onClick={onShow}>
          Show
        </button>
      )}
    </section>
  );
}
```

----------------------------------------

TITLE: Migrating ReactDOM.render to createRoot in React 18
DESCRIPTION: This snippet demonstrates the migration from the legacy `ReactDOM.render` API to the new `createRoot` API in React 18. The `createRoot` API provides better ergonomics for managing roots and enables new concurrent features, which is a key change for React 18 applications.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/blog/2022/03/08/react-18-upgrade-guide.md#_snippet_2

LANGUAGE: javascript
CODE:
```
// Before
import { render } from 'react-dom';
const container = document.getElementById('app');
render(<App tab="home" />, container);
```

LANGUAGE: javascript
CODE:
```
// After
import { createRoot } from 'react-dom/client';
const container = document.getElementById('app');
const root = createRoot(container); // createRoot(container!) if you use TypeScript
root.render(<App tab="home" />);
```

----------------------------------------

TITLE: Directly Calculating Derived Data in React Components
DESCRIPTION: This snippet shows the recommended way to calculate derived data (`visibleTodos`) directly during the component's render. This approach is simpler and more efficient than using `useState` and `useEffect` for derived state, provided the `getFilteredTodos()` function is not computationally expensive.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/you-might-not-need-an-effect.md#_snippet_3

LANGUAGE: js
CODE:
```
function TodoList({ todos, filter }) {
  const [newTodo, setNewTodo] = useState('');
  // ✅ This is fine if getFilteredTodos() is not slow.
  const visibleTodos = getFilteredTodos(todos, filter);
  // ...
}
```

----------------------------------------

TITLE: Complete React Data Fetching Example with Promises and Race Condition Handling
DESCRIPTION: This comprehensive React example showcases data fetching with `useEffect` using Promises, including a `select` element to change the `person` and dynamically update the bio. It incorporates a robust cleanup mechanism with an `ignore` flag to prevent race conditions, ensuring only the latest data is displayed. The `api.js` file simulates an asynchronous data fetching function.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/reference/react/useEffect.md#_snippet_21

LANGUAGE: JavaScript
CODE:
```
import { useState, useEffect } from 'react';
import { fetchBio } from './api.js';

export default function Page() {
  const [person, setPerson] = useState('Alice');
  const [bio, setBio] = useState(null);
  useEffect(() => {
    let ignore = false;
    setBio(null);
    fetchBio(person).then(result => {
      if (!ignore) {
        setBio(result);
      }
    });
    return () => {
      ignore = true;
    }
  }, [person]);

  return (
    <>
      <select value={person} onChange={e => {
        setPerson(e.target.value);
      }}>
        <option value="Alice">Alice</option>
        <option value="Bob">Bob</option>
        <option value="Taylor">Taylor</option>
      </select>
      <hr />
      <p><i>{bio ?? 'Loading...'}</i></p>
    </>
  );
}
```

LANGUAGE: JavaScript
CODE:
```
export async function fetchBio(person) {
  const delay = person === 'Bob' ? 2000 : 200;
  return new Promise(resolve => {
    setTimeout(() => {
      resolve('This is ' + person + '’s bio.');
    }, delay);
  })
}
```

----------------------------------------

TITLE: Custom Error Handling with React 19 createRoot/hydrateRoot (JavaScript)
DESCRIPTION: This JavaScript snippet demonstrates the new `onUncaughtError` and `onCaughtError` options available in `createRoot` and `hydrateRoot` for React 19. These callbacks allow developers to implement custom error reporting logic, reducing duplicate error logs by differentiating between errors caught by an Error Boundary and those that are not.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/blog/2024/04/25/react-19-upgrade-guide.md#_snippet_5

LANGUAGE: js
CODE:
```
const root = createRoot(container, {
  onUncaughtError: (error, errorInfo) => {
    // ... log error report
  },
  onCaughtError: (error, errorInfo) => {
    // ... log error report
  }
});
```

----------------------------------------

TITLE: Defining a Basic React Profile Component
DESCRIPTION: This JavaScript snippet defines a functional React component named 'Profile'. It uses 'export default' to make the component available for import and returns JSX markup, specifically an <img> tag, demonstrating the core structure of a simple React component.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/your-first-component.md#_snippet_2

LANGUAGE: js
CODE:
```
export default function Profile() {
  return (
    <img
      src="https://i.imgur.com/MK3eW3Am.jpg"
      alt="Katherine Johnson"
    />
  )
}
```

----------------------------------------

TITLE: Custom React Hook for Data Fetching (useData)
DESCRIPTION: Defines a reusable custom Hook useData that encapsulates the logic for fetching data from a given URL using fetch and managing the state with useState and useEffect. It includes cleanup logic to prevent state updates on unmounted components.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/reusing-logic-with-custom-hooks.md#_snippet_44

LANGUAGE: javascript
CODE:
```
function useData(url) {
  const [data, setData] = useState(null);
  useEffect(() => {
    if (url) {
      let ignore = false;
      fetch(url)
        .then(response => response.json())
        .then(json => {
          if (!ignore) {
            setData(json);
          }
        });
      return () => {
        ignore = true;
      };
    }
  }, [url]);
  return data;
}
```

----------------------------------------

TITLE: Rendering Multiple Instances of a Component with Independent State in React
DESCRIPTION: This example demonstrates how a single `Counter` component JSX tag, when rendered multiple times at different positions in the render tree, results in two distinct instances, each maintaining its own isolated state for `score` and `hover`. It highlights that state is tied to the component's position, not the component definition itself.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/preserving-and-resetting-state.md#_snippet_0

LANGUAGE: JavaScript
CODE:
```
import { useState } from 'react';

export default function App() {
  const counter = <Counter />;
  return (
    <div>
      {counter}
      {counter}
    </div>
  );
}

function Counter() {
  const [score, setScore] = useState(0);
  const [hover, setHover] = useState(false);

  let className = 'counter';
  if (hover) {
    className += ' hover';
  }

  return (
    <div
      className={className}
      onPointerEnter={() => setHover(true)}
      onPointerLeave={() => setHover(false)}
    >
      <h1>{score}</h1>
      <button onClick={() => setScore(score + 1)}>
        Add one
      </button>
    </div>
  );
}
```

LANGUAGE: CSS
CODE:
```
label {
  display: block;
  clear: both;
}

.counter {
  width: 100px;
  text-align: center;
  border: 1px solid gray;
  border-radius: 4px;
  padding: 20px;
  margin: 0 20px 20px 0;
  float: left;
}

.hover {
  background: #ffffd8;
}
```

----------------------------------------

TITLE: Rendering React Components with Props (JavaScript)
DESCRIPTION: This example showcases a complete React application where the `Avatar` component receives `person` (an object) and `size` (a number) as props. The `Profile` component renders multiple `Avatar` instances, each configured with different prop values, demonstrating how props enable flexible and reusable component rendering.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/passing-props-to-a-component.md#_snippet_4

LANGUAGE: js
CODE:
```
import { getImageUrl } from './utils.js';

function Avatar({ person, size }) {
  return (
    <img
      className="avatar"
      src={getImageUrl(person)}
      alt={person.name}
      width={size}
      height={size}
    />
  );
}

export default function Profile() {
  return (
    <div>
      <Avatar
        size={100}
        person={{
          name: 'Katsuko Saruhashi',
          imageId: 'YfeOqp2'
        }}
      />
      <Avatar
        size={80}
        person={{
          name: 'Aklilu Lemma',
          imageId: 'OKS67lh'
        }}
      />
      <Avatar
        size={50}
        person={{
          name: 'Lin Lanying',
          imageId: '1bX5QH6'
        }}
      />
    </div>
  );
}
```

----------------------------------------

TITLE: Full React Application with Suspense for Data Fetching
DESCRIPTION: This comprehensive example demonstrates a React application leveraging Suspense for asynchronous data fetching. It includes an `App` component for navigation, an `ArtistPage` that uses Suspense to load `Albums`, an `Albums` component that suspends while fetching data, and a `data` utility that simulates API calls with a delay and caching.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/reference/react/Suspense.md#_snippet_2

LANGUAGE: JavaScript
CODE:
```
import { useState } from 'react';
import ArtistPage from './ArtistPage.js';

export default function App() {
  const [show, setShow] = useState(false);
  if (show) {
    return (
      <ArtistPage
        artist={{
          id: 'the-beatles',
          name: 'The Beatles',
        }}
      />
    );
  } else {
    return (
      <button onClick={() => setShow(true)}>
        Open The Beatles artist page
      </button>
    );
  }
}
```

LANGUAGE: JavaScript
CODE:
```
import { Suspense } from 'react';
import Albums from './Albums.js';

export default function ArtistPage({ artist }) {
  return (
    <>
      <h1>{artist.name}</h1>
      <Suspense fallback={<Loading />}>
        <Albums artistId={artist.id} />
      </Suspense>
    </>
  );
}

function Loading() {
  return <h2>🌀 Loading...</h2>;
}
```

LANGUAGE: JavaScript
CODE:
```
import {use} from 'react';
import { fetchData } from './data.js';

export default function Albums({ artistId }) {
  const albums = use(fetchData(`/${artistId}/albums`));
  return (
    <ul>
      {albums.map(album => (
        <li key={album.id}>
          {album.title} ({album.year})
        </li>
      ))}
    </ul>
  );
}
```

LANGUAGE: JavaScript
CODE:
```
// Note: the way you would do data fetching depends on
// the framework that you use together with Suspense.
// Normally, the caching logic would be inside a framework.

let cache = new Map();

export function fetchData(url) {
  if (!cache.has(url)) {
    cache.set(url, getData(url));
  }
  return cache.get(url);
}

async function getData(url) {
  if (url === '/the-beatles/albums') {
    return await getAlbums();
  } else {
    throw Error('Not implemented');
  }
}

async function getAlbums() {
  // Add a fake delay to make waiting noticeable.
  await new Promise(resolve => {
    setTimeout(resolve, 3000);
  });

  return [{
    id: 13,
    title: 'Let It Be',
    year: 1970
  }, {
    id: 12,
    title: 'Abbey Road',
    year: 1969
  }, {
    id: 11,
    title: 'Yellow Submarine',
    year: 1969
  }, {
    id: 10,
    title: 'The Beatles',
    year: 1968
  }, {
    id: 9,
    title: 'Magical Mystery Tour',
    year: 1967
  }, {
    id: 8,
    title: 'Sgt. Pepper\'s Lonely Hearts Club Band',
    year: 1967
  }, {
    id: 7,
    title: 'Revolver',
    year: 1966
  }, {
    id: 6,
    title: 'Rubber Soul',
    year: 1965
  }, {
    id: 5,
    title: 'Help!',
    year: 1965
  }, {
    id: 4,
    title: 'Beatles For Sale',
    year: 1964
  }, {
    id: 3,
    title: 'A Hard Day\'s Night',
    year: 1964
  }, {
    id: 2,
    title: 'With The Beatles',
    year: 1963
  }, {
    id: 1,
    title: 'Please Please Me',
    year: 1963
  }];
}
```

----------------------------------------

TITLE: Identifying Impure Reducer Functions in React JavaScript
DESCRIPTION: This JavaScript reducer function illustrates an impure state update where the `state.todos` array is directly mutated using `push`. In React's Strict Mode, this impurity will cause the reducer to run twice, leading to unexpected behavior like items being added twice, helping developers identify and fix such mistakes by enforcing purity.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/reference/react/useReducer.md#_snippet_33

LANGUAGE: js
CODE:
```
function reducer(state, action) {
  switch (action.type) {
    case 'added_todo': {
      // 🚩 Mistake: mutating state
      state.todos.push({ id: nextId++, text: action.text });
      return state;
    }
    // ...
  }
}
```

----------------------------------------

TITLE: Implementing Pure Reducer Functions in React JavaScript
DESCRIPTION: This JavaScript reducer function demonstrates a pure state update by replacing the `todos` array instead of mutating it. It uses the spread syntax (`...`) to create a new array with the added todo item, ensuring that the original state remains immutable. This approach is crucial for predictable state management and works correctly even when React calls the reducer twice in Strict Mode.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/reference/react/useReducer.md#_snippet_34

LANGUAGE: js
CODE:
```
function reducer(state, action) {
  switch (action.type) {
    case 'added_todo': {
      // ✅ Correct: replacing with new state
      return {
        ...state,
        todos: [
          ...state.todos,
          { id: nextId++, text: action.text }
        ]
      };
    }
    // ...
  }
}
```

----------------------------------------

TITLE: Understanding `useEffect` Dependency Array Behaviors in React
DESCRIPTION: This snippet illustrates the three primary behaviors of the `useEffect` hook based on its dependency array: running after every render (no array), running only on component mount (empty array `[]`), and running on mount and when specified dependencies change (`[a, b]`).
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/synchronizing-with-effects.md#_snippet_11

LANGUAGE: JavaScript
CODE:
```
useEffect(() => {
  // This runs after every render
});

useEffect(() => {
  // This runs only on mount (when the component appears)
}, []);

useEffect(() => {
  // This runs on mount *and also* if either a or b have changed since the last render
}, [a, b]);
```

----------------------------------------

TITLE: Defining a Profile React Component
DESCRIPTION: This React functional component displays detailed information about a person, including their name, profession, awards, and a dynamically sized image. It accepts 'person' and 'imageSize' as props, utilizing the 'getImageUrl' utility for image sourcing.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/passing-props-to-a-component.md#_snippet_21

LANGUAGE: JavaScript
CODE:
```
function Profile({ person, imageSize = 70 }) {
  const imageSrc = getImageUrl(person)

  return (
    <section className="profile">
      <h2>{person.name}</h2>
      <img
        className="avatar"
        src={imageSrc}
        alt={person.name}
        width={imageSize}
        height={imageSize}
      />
      <ul>
        <li>
          <b>Profession:</b> {person.profession}
        </li>
        <li>
          <b>Awards: {person.awards.length} </b>
          ({person.awards.join(', ')})
        </li>
        <li>
          <b>Discovered: </b>
          {person.discovery}
        </li>
      </ul>
    </section>
  )
}
```

----------------------------------------

TITLE: Using `useEffectEvent` for Non-Reactive Reads (React)
DESCRIPTION: This example shows how to use the experimental `useEffectEvent` Hook to read the latest `shoppingCart` value without making it a dependency of the `useEffect`. The `onVisit` event is non-reactive, allowing the effect to re-run only when `url` changes, while still accessing the current `shoppingCart.length`.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/reference/react/useEffect.md#_snippet_50

LANGUAGE: js
CODE:
```
function Page({ url, shoppingCart }) {
  const onVisit = useEffectEvent(visitedUrl => {
    logVisit(visitedUrl, shoppingCart.length)
  });

  useEffect(() => {
    onVisit(url);
  }, [url]); // ✅ All dependencies declared
  // ...
}
```

----------------------------------------

TITLE: Toggle Button Failing to Re-render with `useRef` (Problem) - JavaScript
DESCRIPTION: This React component uses `useRef` to store the `isOnRef` boolean value for a toggle button. While `isOnRef.current` is correctly updated on click, `useRef` does not trigger a re-render when its `.current` property changes. Consequently, the component's display (`{isOnRef.current ? 'On' : 'Off'}`) never updates, always showing 'Off'.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/referencing-values-with-refs.md#_snippet_13

LANGUAGE: javascript
CODE:
```
import { useRef } from 'react';

export default function Toggle() {
  const isOnRef = useRef(false);

  return (
    <button onClick={() => {
      isOnRef.current = !isOnRef.current;
    }}>
      {isOnRef.current ? 'On' : 'Off'}
    </button>
  );
}
```

----------------------------------------

TITLE: Using camelCase for HTML Attributes in React JSX
DESCRIPTION: In React JSX, most HTML and SVG attributes are written in camelCase because they are transformed into JavaScript object keys. For example, 'class' becomes 'className' and 'stroke-width' becomes 'strokeWidth', aligning with JavaScript variable naming conventions.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/writing-markup-with-jsx.md#_snippet_6

LANGUAGE: js
CODE:
```
<img 
  src="https://i.imgur.com/yXOvdOSs.jpg" 
  alt="Hedy Lamarr" 
  className="photo"
/>
```

----------------------------------------

TITLE: Illustrating Conflicting DOM Manipulation with React Refs and State
DESCRIPTION: This JavaScript snippet demonstrates how direct DOM manipulation using `ref.current.remove()` can conflict with React's state-driven rendering. It shows a component that toggles an element's visibility via `useState` and also allows forceful removal via a ref, leading to a crash when React attempts to re-render the removed element.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/manipulating-the-dom-with-refs.md#_snippet_18

LANGUAGE: javascript
CODE:
```
import { useState, useRef } from 'react';

export default function Counter() {
  const [show, setShow] = useState(true);
  const ref = useRef(null);

  return (
    <div>
      <button
        onClick={() => {
          setShow(!show);
        }}>
        Toggle with setState
      </button>
      <button
        onClick={() => {
          ref.current.remove();
        }}>
        Remove from the DOM
      </button>
      {show && <p ref={ref}>Hello world</p>}
    </div>
  );
}
```

----------------------------------------

TITLE: Problematic Form Submission with useEffect in React
DESCRIPTION: This snippet demonstrates an incorrect pattern where `sendMessage` is called inside a `useEffect` hook, triggered by changes to `showForm` or `message`. This causes the message to be sent whenever `showForm` becomes `false`, even if it's the initial state, leading to unintended message submissions (e.g., an empty message on load). The `useEffect` is observing state changes rather than reacting to a direct user event.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/you-might-not-need-an-effect.md#_snippet_53

LANGUAGE: js
CODE:
```
import { useState, useEffect } from 'react';

export default function Form() {
  const [showForm, setShowForm] = useState(true);
  const [message, setMessage] = useState('');

  useEffect(() => {
    if (!showForm) {
      sendMessage(message);
    }
  }, [showForm, message]);

  function handleSubmit(e) {
    e.preventDefault();
    setShowForm(false);
  }

  if (!showForm) {
    return (
      <>
        <h1>Thanks for using our services!</h1>
        <button onClick={() => {
          setMessage('');
          setShowForm(true);
        }}>
          Open chat
        </button>
      </>
    );
  }

  return (
    <form onSubmit={handleSubmit}>
      <textarea
        placeholder="Message"
        value={message}
        onChange={e => setMessage(e.target.value)}
      />
      <button type="submit" disabled={message === ''}>
        Send
      </button>
    </form>
  );
}

function sendMessage(message) {
  console.log('Sending message: ' + message);
}
```

LANGUAGE: css
CODE:
```
label, textarea { margin-bottom: 10px; display: block; }
```

----------------------------------------

TITLE: React Component with Immutable Array Updates (Fixed)
DESCRIPTION: This React component demonstrates the corrected approach to updating objects within arrays immutably. By using the `map` method and object spread syntax (`{ ...artwork, seen: nextSeen }`) to create new objects for updated items, it prevents direct state mutation and resolves the shared state bug, ensuring that each list's state is isolated.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/updating-arrays-in-state.md#_snippet_14

LANGUAGE: javascript
CODE:
```
import { useState } from 'react';

let nextId = 3;
const initialList = [
  { id: 0, title: 'Big Bellies', seen: false },
  { id: 1, title: 'Lunar Landscape', seen: false },
  { id: 2, title: 'Terracotta Army', seen: true },
];

export default function BucketList() {
  const [myList, setMyList] = useState(initialList);
  const [yourList, setYourList] = useState(
    initialList
  );

  function handleToggleMyList(artworkId, nextSeen) {
    setMyList(myList.map(artwork => {
      if (artwork.id === artworkId) {
        // Create a *new* object with changes
        return { ...artwork, seen: nextSeen };
      } else {
        // No changes
        return artwork;
      }
    }));
  }

  function handleToggleYourList(artworkId, nextSeen) {
    setYourList(yourList.map(artwork => {
      if (artwork.id === artworkId) {
        // Create a *new* object with changes
        return { ...artwork, seen: nextSeen };
      } else {
        // No changes
        return artwork;
      }
    }));
  }

  return (
    <>
      <h1>Art Bucket List</h1>
      <h2>My list of art to see:</h2>
      <ItemList
        artworks={myList}
        onToggle={handleToggleMyList} />
      <h2>Your list of art to see:</h2>
      <ItemList
        artworks={yourList}
        onToggle={handleToggleYourList} />
    </>
  );
}

function ItemList({ artworks, onToggle }) {
  return (
    <ul>
      {artworks.map(artwork => (

```

----------------------------------------

TITLE: Updating State in React with Setter Function
DESCRIPTION: This snippet illustrates how to update a state variable by calling its corresponding `set` function. When `setName('Robin')` is called, React will re-render the component with the new 'Robin' value for the `name` state variable, updating the UI accordingly.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/reference/react/useState.md#_snippet_4

LANGUAGE: js
CODE:
```
function handleClick() {
  setName('Robin');
}
```

----------------------------------------

TITLE: Implementing Form Actions and Status with React DOM Hooks
DESCRIPTION: This example demonstrates the use of `useActionState` to manage state updates triggered by form actions and `useFormStatus` to access the pending status of a form submission. The `Form` component uses `useActionState` for an incrementing counter, while the nested `Button` component leverages `useFormStatus` to disable itself when the form is submitting, providing immediate user feedback.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/reference/react-dom/hooks/index.md#_snippet_0

LANGUAGE: JavaScript
CODE:
```
function Form({ action }) {
  async function increment(n) {
    return n + 1;
  }
  const [count, incrementFormAction] = useActionState(increment, 0);
  return (
    <form action={action}>
      <button formAction={incrementFormAction}>Count: {count}</button>
      <Button />
    </form>
  );
}

function Button() {
  const { pending } = useFormStatus();
  return (
    <button disabled={pending} type="submit">
      Submit
    </button>
  );
}
```

----------------------------------------

TITLE: Understanding React State Updates with Multiple setNumber Calls (JavaScript)
DESCRIPTION: This React component demonstrates how `setNumber` calls within a single event handler do not immediately update the `number` state for the current render. Despite calling `setNumber(number + 1)` three times, the `number` value used in each call is the state value from the *beginning* of the render, resulting in the counter incrementing by only one per click. It highlights that state updates are batched and applied for the *next* render.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/state-as-a-snapshot.md#_snippet_2

LANGUAGE: JavaScript
CODE:
```
import { useState } from 'react';

export default function Counter() {
  const [number, setNumber] = useState(0);

  return (
    <>
      <h1>{number}</h1>
      <button onClick={() => {
        setNumber(number + 1);
        setNumber(number + 1);
        setNumber(number + 1);
      }}>+3</button>
    </>
  )
}
```

----------------------------------------

TITLE: Pure State Updater Function (Correct) - JavaScript
DESCRIPTION: This snippet demonstrates a pure updater function that correctly updates the state by creating a new array using the spread syntax (`...prevTodos`) instead of mutating the original. This ensures immutability, and even if called twice in Strict Mode, the behavior remains consistent.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/reference/react/useState.md#_snippet_43

LANGUAGE: js
CODE:
```
setTodos(prevTodos => {
  // ✅ Correct: replacing with new state
  return [...prevTodos, createTodo()];
});
```

----------------------------------------

TITLE: Fixing React setInterval with useEffect Cleanup
DESCRIPTION: This React component provides the corrected implementation for an interval-based counter. By returning a cleanup function from `useEffect` that calls `clearInterval`, it ensures that the interval is properly stopped when the component unmounts or the effect re-runs, preventing duplicate executions and maintaining correct behavior.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/synchronizing-with-effects.md#_snippet_45

LANGUAGE: JavaScript
CODE:
```
import { useState, useEffect } from 'react';

export default function Counter() {
  const [count, setCount] = useState(0);

  useEffect(() => {
    function onTick() {
      setCount(c => c + 1);
    }

    const intervalId = setInterval(onTick, 1000);
    return () => clearInterval(intervalId);
  }, []);

  return <h1>{count}</h1>;
}
```

LANGUAGE: JavaScript
CODE:
```
import { useState } from 'react';
import Counter from './Counter.js';

export default function App() {
  const [show, setShow] = useState(false);
  return (
    <>
      <button onClick={() => setShow(s => !s)}>{show ? 'Hide' : 'Show'} counter</button>
      <br />
      <hr />
      {show && <Counter />}
    </>
  );
}
```

LANGUAGE: CSS
CODE:
```
label {
  display: block;
  margin-top: 20px;
  margin-bottom: 20px;
}

body {
  min-height: 150px;
}
```

----------------------------------------

TITLE: Complete React Context Example with `use` Hook
DESCRIPTION: This comprehensive example demonstrates the full lifecycle of React Context using the `use` Hook. It includes creating a context, providing a value via `ThemeContext.Provider`, and consuming that value in nested components (`Panel` and `Button`) both directly and conditionally, showcasing the `use` Hook's versatility.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/reference/react/use.md#_snippet_5

LANGUAGE: JavaScript
CODE:
```
import { createContext, use } from 'react';

const ThemeContext = createContext(null);

export default function MyApp() {
  return (
    <ThemeContext.Provider value="dark">
      <Form />
    </ThemeContext.Provider>
  )
}

function Form() {
  return (
    <Panel title="Welcome">
      <Button show={true}>Sign up</Button>
      <Button show={false}>Log in</Button>
    </Panel>
  );
}

function Panel({ title, children }) {
  const theme = use(ThemeContext);
  const className = 'panel-' + theme;
  return (
    <section className={className}>
      <h1>{title}</h1>
      {children}
    </section>
  )
}

function Button({ show, children }) {
  if (show) {
    const theme = use(ThemeContext);
    const className = 'button-' + theme;
    return (
      <button className={className}>
        {children}
      </button>
    );
  }
  return false
}
```

----------------------------------------

TITLE: Correctly Passing Indexed handleClick with Arrow Function - React JSX
DESCRIPTION: This snippet shows the correct way to pass an event handler with an argument in React JSX using an arrow function. The arrow function `() => handleClick(0)` creates a new function that, when called by the click event, will then execute `handleClick(0)`, preventing immediate execution and infinite loops.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/tutorial-tic-tac-toe.md#_snippet_40

LANGUAGE: js
CODE:
```
export default function Board() {
  // ...
  return (
    <>
      <div className="board-row">
        <Square value={squares[0]} onSquareClick={() => handleClick(0)} />
        // ...
  );
}
```

----------------------------------------

TITLE: Accessing Latest Input State with React useRef in Asynchronous Operations
DESCRIPTION: This solution snippet shows how to use `useRef` to access the *latest* value of an input field within an asynchronous operation. While `useState` is still used for rendering the input, `textRef.current` is manually updated in the `handleChange` function. This mutable ref allows the `setTimeout` callback in `handleSend` to read the most current input text, overcoming the snapshot behavior of state.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/referencing-values-with-refs.md#_snippet_20

LANGUAGE: js
CODE:
```
import { useState, useRef } from 'react';

export default function Chat() {
  const [text, setText] = useState('');
  const textRef = useRef(text);

  function handleChange(e) {
    setText(e.target.value);
    textRef.current = e.target.value;
  }

  function handleSend() {
    setTimeout(() => {
      alert('Sending: ' + textRef.current);
    }, 3000);
  }

  return (
    <>
      <input
        value={text}
        onChange={handleChange}
      />
      <button
        onClick={handleSend}>
        Send
      </button>
    </>
  );
}
```

----------------------------------------

TITLE: Passing State as Props to Child Components in React
DESCRIPTION: This snippet illustrates how state variables (`filterText` and `inStockOnly`) managed by the parent `FilterableProductTable` component are passed down as props to its child components, `SearchBar` and `ProductTable`. This enables the child components to access and render data based on the parent's state, adhering to React's one-way data flow principle. It's a fundamental pattern for sharing state across the component hierarchy.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/thinking-in-react.md#_snippet_4

LANGUAGE: JSX
CODE:
```
<div>
  <SearchBar 
    filterText={filterText} 
    inStockOnly={inStockOnly} />
  <ProductTable 
    products={products}
    filterText={filterText}
    inStockOnly={inStockOnly} />
</div>
```

----------------------------------------

TITLE: Using React useEffect for Side Effects - JavaScript
DESCRIPTION: This snippet demonstrates the use of the `useEffect` Hook in React. Code placed inside `useEffect` runs *after* the component has rendered, making it the appropriate place for performing side effects like data fetching, subscriptions, or logging. The dependency array `[selectedItems]` ensures the effect re-runs only when `selectedItems` changes.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/reference/rules/components-and-hooks-must-be-pure.md#_snippet_2

LANGUAGE: JavaScript
CODE:
```
function Dropdown() {
  const selectedItems = new Set();
  useEffect(() => {
    // this code is inside of an Effect, so it only runs after rendering
    logForAnalytics(selectedItems);
  }, [selectedItems]);
}
```

----------------------------------------

TITLE: Managing Chat Room Connection Lifecycle with React useEffect
DESCRIPTION: The `ChatRoom` component uses the `useEffect` hook to manage the lifecycle of a chat connection. It dynamically selects either an encrypted or unencrypted connection function based on the `isEncrypted` prop. The effect connects to the room when `roomId` or `isEncrypted` changes and disconnects on cleanup, ensuring proper resource management.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/lifecycle-of-reactive-effects.md#_snippet_49

LANGUAGE: javascript
CODE:
```
import { useState, useEffect } from 'react';
import {
  createEncryptedConnection,
  createUnencryptedConnection,
} from './chat.js';

export default function ChatRoom({ roomId, isEncrypted }) {
  useEffect(() => {
    const createConnection = isEncrypted ?
      createEncryptedConnection :
      createUnencryptedConnection;
    const connection = createConnection(roomId);
    connection.connect();
    return () => connection.disconnect();
  }, [roomId, isEncrypted]);

  return <h1>Welcome to the {roomId} room!</h1>;
}
```

----------------------------------------

TITLE: Migrating PropTypes to TypeScript for React Function Components (TypeScript)
DESCRIPTION: This TypeScript snippet demonstrates the recommended modern approach for type-checking and providing default prop values in React 19. It uses a TypeScript interface to define prop types and ES6 default parameters directly in the function signature, replacing the deprecated `propTypes` and `defaultProps`.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/blog/2024/04/25/react-19-upgrade-guide.md#_snippet_7

LANGUAGE: ts
CODE:
```
interface Props {
  text?: string;
}
function Heading({text = 'Hello, world!'}: Props) {
  return <h1>{text}</h1>;
}
```

----------------------------------------

TITLE: Using Server Function with React Form Action (JS/JSX)
DESCRIPTION: Defines an asynchronous Server Function `requestUsername` that accepts form data and processes it. It then shows a React component that renders a form using this Server Function directly as its `action` prop, enabling progressive enhancement.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/reference/rsc/use-server.md#_snippet_1

LANGUAGE: JSX
CODE:
```
// App.js

async function requestUsername(formData) {
  'use server';
  const username = formData.get('username');
  // ...
}

export default function App() {
  return (
    <form action={requestUsername}>
      <input type="text" name="username" />
      <button type="submit">Request</button>
    </form>
  );
}
```

----------------------------------------

TITLE: Managing Pending State with useTransition and React Actions
DESCRIPTION: This snippet illustrates how React 19's `useTransition` hook simplifies pending state management for asynchronous operations (Actions). By wrapping the `updateName` call in `startTransition`, React automatically manages the `isPending` state, providing a more streamlined approach compared to manual `useState` management.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/blog/2024/12/05/react-19.md#_snippet_1

LANGUAGE: js
CODE:
```
// Using pending state from Actions
function UpdateName({}) {
  const [name, setName] = useState("");
  const [error, setError] = useState(null);
  const [isPending, startTransition] = useTransition();

  const handleSubmit = () => {
    startTransition(async () => {
      const error = await updateName(name);
      if (error) {
        setError(error);
        return;
      } 
      redirect("/path");
    })
  };

  return (
    <div>
      <input value={name} onChange={(event) => setName(event.target.value)} />
      <button onClick={handleSubmit} disabled={isPending}>
        Update
      </button>
      {error && <p>{error}</p>}
    </div>
  );
}
```

----------------------------------------

TITLE: Incorrect: Mutating Object State Directly in React
DESCRIPTION: This snippet demonstrates an anti-pattern in React state management: directly mutating an object held in state. React relies on reference equality to detect changes, so direct mutation will not trigger a re-render, leading to UI inconsistencies.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/reference/react/useState.md#_snippet_14

LANGUAGE: JavaScript
CODE:
```
form.firstName = 'Taylor';
```

----------------------------------------

TITLE: Main Application Component with Context and Reducer in React
DESCRIPTION: The `App.js` component serves as the root of the task application. It initializes the task state using `useReducer` and provides both the `tasks` state and the `dispatch` function to its children via `TasksContext.Provider` and `TasksDispatchContext.Provider` respectively. It also defines the `tasksReducer` logic for state transitions, handling 'added', 'changed', and 'deleted' actions.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/scaling-up-with-reducer-and-context.md#_snippet_18

LANGUAGE: JavaScript
CODE:
```
import { useReducer } from 'react';
import AddTask from './AddTask.js';
import TaskList from './TaskList.js';
import { TasksContext, TasksDispatchContext } from './TasksContext.js';

export default function TaskApp() {
  const [tasks, dispatch] = useReducer(
    tasksReducer,
    initialTasks
  );

  return (
    <TasksContext.Provider value={tasks}>
      <TasksDispatchContext.Provider value={dispatch}>
        <h1>Day off in Kyoto</h1>
        <AddTask />
        <TaskList />
      </TasksDispatchContext.Provider>
    </TasksContext.Provider>
  );
}

function tasksReducer(tasks, action) {
  switch (action.type) {
    case 'added': {
      return [...tasks, {
        id: action.id,
        text: action.text,
        done: false
      }];
    }
    case 'changed': {
      return tasks.map(t => {
        if (t.id === action.task.id) {
          return action.task;
        } else {
          return t;
        }
      });
    }
    case 'deleted': {
      return tasks.filter(t => t.id !== action.id);
    }
    default: {
      throw Error('Unknown action: ' + action.type);
    }
  }
}

const initialTasks = [
  { id: 0, text: 'Philosopher’s Path', done: true },
  { id: 1, text: 'Visit the temple', done: false },
  { id: 2, text: 'Drink matcha', done: false }
];
```

----------------------------------------

TITLE: Using Context as a Provider in React 19 (JavaScript)
DESCRIPTION: React 19 introduces a new syntax allowing `<Context>` to be rendered directly as a provider, simplifying context consumption. This change deprecates the older `<Context.Provider>` syntax, with a codemod available for migration.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/blog/2024/12/05/react-19.md#_snippet_12

LANGUAGE: JavaScript
CODE:
```
const ThemeContext = createContext('');

function App({children}) {
  return (
    <ThemeContext value="dark">
      {children}
    </ThemeContext>
  );  
}
```

----------------------------------------

TITLE: Defining Client Component with use client and useState in React JS
DESCRIPTION: This snippet demonstrates a component (`InspirationGenerator`) defined in a file marked with `'use client'`. It utilizes the client-side `useState` hook to manage internal state (the current quote index). Components and hooks used within a file marked `'use client'` are bundled and executed on the client.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/reference/rsc/use-client.md#_snippet_1

LANGUAGE: js
CODE:
```
'use client';

import { useState } from 'react';
import inspirations from './inspirations';
import FancyText from './FancyText';

export default function InspirationGenerator({children}) {
  const [index, setIndex] = useState(0);
  const quote = inspirations[index];
  const next = () => setIndex((index + 1) % inspirations.length);

  return (
    <>
      <p>Your inspirational quote is:</p>
      <FancyText text={quote} />
      <button onClick={next}>Inspire me again</button>
      {children}
    </>
  );
}
```

----------------------------------------

TITLE: Conditionally Returning JSX in React Item Component - JavaScript
DESCRIPTION: This code snippet demonstrates how to use an `if`/`else` statement within a React component to conditionally return different JSX based on a prop. If the `isPacked` prop is `true`, it returns a list item with a checkmark (✅); otherwise, it returns the item without a checkmark. This approach allows for branching logic in rendering.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/conditional-rendering.md#_snippet_1

LANGUAGE: js
CODE:
```
if (isPacked) {
  return <li className="item">{name} ✅</li>;
}
return <li className="item">{name}</li>;
```

----------------------------------------

TITLE: Updating React State Directly (Incorrect for Batched Updates)
DESCRIPTION: This example illustrates the problem of directly passing the next state value (`age + 1`) to `setAge`. When multiple `increment` calls are batched, they all capture the initial `age` value from the start of the event, leading to an incorrect final state for the '+3' button.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/reference/react/useState.md#_snippet_13

LANGUAGE: JavaScript
CODE:
```
import { useState } from 'react';

export default function Counter() {
  const [age, setAge] = useState(42);

  function increment() {
    setAge(age + 1);
  }

  return (
    <>
      <h1>Your age: {age}</h1>
      <button onClick={() => {
        increment();
        increment();
        increment();
      }}>+3</button>
      <button onClick={() => {
        increment();
      }}>+1</button>
    </>
  );
}
```

LANGUAGE: CSS
CODE:
```
button { display: block; margin: 10px; font-size: 20px; }
h1 { display: block; margin: 10px; }
```

----------------------------------------

TITLE: Declaring State with useState in React
DESCRIPTION: This snippet demonstrates the basic syntax for declaring a state variable using the `useState` React Hook. It shows how to destructure the returned array into a state variable (`state`) and its corresponding setter function (`setState`), initialized with `initialState`.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/reference/react/useState.md#_snippet_0

LANGUAGE: JavaScript
CODE:
```
const [state, setState] = useState(initialState)
```

----------------------------------------

TITLE: Fixing Misplaced State in React List (Correct ID Keying)
DESCRIPTION: This set of code snippets provides the solution to the misplaced state problem. By changing the `key` prop for list items from the array index (`i`) to a stable, unique identifier from the data (`contact.id`), React can correctly track each component instance and its associated state, even when the list order is reversed. The `Contact` component remains unchanged as the issue is with the keying in the parent list, demonstrating that the problem lies in how list items are identified, not in the individual component's state management.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/preserving-and-resetting-state.md#_snippet_32

LANGUAGE: JavaScript
CODE:
```
import { useState } from 'react';
import Contact from './Contact.js';

export default function ContactList() {
  const [reverse, setReverse] = useState(false);

  const displayedContacts = [...contacts];
  if (reverse) {
    displayedContacts.reverse();
  }

  return (
    <>
      <label>
        <input
          type="checkbox"
          checked={reverse}
          onChange={e => {
            setReverse(e.target.checked)
          }}
        />{' '}
        Show in reverse order
      </label>
      <ul>
        {displayedContacts.map(contact =>
          <li key={contact.id}>
            <Contact contact={contact} />
          </li>
        )}
      </ul>
    </>
  );
}

const contacts = [
  { id: 0, name: 'Alice', email: 'alice@mail.com' },
  { id: 1, name: 'Bob', email: 'bob@mail.com' },
  { id: 2, name: 'Taylor', email: 'taylor@mail.com' }
];
```

LANGUAGE: JavaScript
CODE:
```
import { useState } => 'react';

export default function Contact({ contact }) {
  const [expanded, setExpanded] = useState(false);
  return (
    <>
      <p><b>{contact.name}</b></p>
      {expanded &&
        <p><i>{contact.email}</i></p>
      }
      <button onClick={() => {
        setExpanded(!expanded);
      }}>
        {expanded ? 'Hide' : 'Show'} email
      </button>
    </>
  );
}
```

LANGUAGE: CSS
CODE:
```
ul, li {
  list-style: none;
  margin: 0;
  padding: 0;
}
li {
  margin-bottom: 20px;
}
label {
  display: block;
  margin: 10px 0;
}
button {
  margin-right: 10px;
  margin-bottom: 10px;
}
```

----------------------------------------

TITLE: Declaring a State Variable with useState in React
DESCRIPTION: This snippet replaces a regular variable with a state variable using the `useState` hook. It initializes `index` to 0 and provides `setIndex` as the setter function to update its value, triggering re-renders in React components. It uses array destructuring to assign the state value and its setter.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/state-a-components-memory.md#_snippet_6

LANGUAGE: JavaScript
CODE:
```
const [index, setIndex] = useState(0);
```

----------------------------------------

TITLE: Implementing React Form with State Management - JavaScript
DESCRIPTION: This snippet demonstrates a React functional component (`Form`) that manages a quiz form's state using `useState`. It includes handlers for text input (`handleTextareaChange`) and form submission (`handleSubmit`), which asynchronously interacts with a mock API (`submitForm`) to update the form's status (typing, submitting, success) and display errors. The component's UI is declaratively rendered based on its current state.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/reacting-to-input-with-state.md#_snippet_7

LANGUAGE: javascript
CODE:
```
import { useState } from 'react';

export default function Form() {
  const [answer, setAnswer] = useState('');
  const [error, setError] = useState(null);
  const [status, setStatus] = useState('typing');

  if (status === 'success') {
    return <h1>That's right!</h1>
  }

  async function handleSubmit(e) {
    e.preventDefault();
    setStatus('submitting');
    try {
      await submitForm(answer);
      setStatus('success');
    } catch (err) {
      setStatus('typing');
      setError(err);
    }
  }

  function handleTextareaChange(e) {
    setAnswer(e.target.value);
  }

  return (
    <>
      <h2>City quiz</h2>
      <p>
        In which city is there a billboard that turns air into drinkable water?
      </p>
      <form onSubmit={handleSubmit}>
        <textarea
          value={answer}
          onChange={handleTextareaChange}
          disabled={status === 'submitting'}
        />
        <br />
        <button disabled={
          answer.length === 0 ||
          status === 'submitting'
        }>
          Submit
        </button>
        {error !== null &&
          <p className="Error">
            {error.message}
          </p>
        }
      </form>
    </>
  );
}

function submitForm(answer) {
  // Pretend it's hitting the network.
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      let shouldError = answer.toLowerCase() !== 'lima'
      if (shouldError) {
        reject(new Error('Good guess but a wrong answer. Try again!'));
      } else {
        resolve();
      }
    }, 1500);
  });
}
```

----------------------------------------

TITLE: Reading Resources with `use` API in React
DESCRIPTION: This snippet demonstrates how to use the `use` API in React to read values from resources such as Promises and Context. It shows a functional component that consumes a `messagePromise` and a `ThemeContext` to retrieve data without storing them in component state.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/reference/react/apis.md#_snippet_0

LANGUAGE: javascript
CODE:
```
function MessageComponent({ messagePromise }) {
  const message = use(messagePromise);
  const theme = use(ThemeContext);
  // ...
}
```

----------------------------------------

TITLE: Building a Concurrent React Application with Suspense and Transitions
DESCRIPTION: This comprehensive example showcases a React application utilizing `Suspense` for data fetching and `startTransition` for navigation. It demonstrates how different components interact, including a custom router, data fetching utilities, and UI layouts, to create a smooth, concurrent user experience.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/reference/react/Suspense.md#_snippet_36

LANGUAGE: JavaScript
CODE:
```
import { Suspense, startTransition, useState } from 'react';
import IndexPage from './IndexPage.js';
import ArtistPage from './ArtistPage.js';
import Layout from './Layout.js';

export default function App() {
  return (
    <Suspense fallback={<BigSpinner />}>
      <Router />
    </Suspense>
  );
}

function Router() {
  const [page, setPage] = useState('/');

  function navigate(url) {
    startTransition(() => {
      setPage(url);
    });
  }

  let content;
  if (page === '/') {
    content = (
      <IndexPage navigate={navigate} />
    );
  } else if (page === '/the-beatles') {
    content = (
      <ArtistPage
        artist={{
          id: 'the-beatles',
          name: 'The Beatles',
        }}
      />
    );
  }
  return (
    <Layout>
      {content}
    </Layout>
  );
}

function BigSpinner() {
  return <h2>🌀 Loading...</h2>;
}
```

LANGUAGE: JavaScript
CODE:
```
export default function Layout({ children }) {
  return (
    <div className="layout">
      <section className="header">
        Music Browser
      </section>
      <main>
        {children}
      </main>
    </div>
  );
}
```

LANGUAGE: JavaScript
CODE:
```
export default function IndexPage({ navigate }) {
  return (
    <button onClick={() => navigate('/the-beatles')}>
      Open The Beatles artist page
    </button>
  );
}
```

LANGUAGE: JavaScript
CODE:
```
import { Suspense } from 'react';
import Albums from
```

----------------------------------------

TITLE: Fixing Race Conditions in useEffect with Cleanup - React JavaScript
DESCRIPTION: This snippet provides the corrected version of the React component, addressing the race condition bug. It introduces a cleanup function within `useEffect` that uses an `ignore` flag to prevent outdated asynchronous operations from updating the component's state, ensuring only the result of the latest fetch is applied.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/synchronizing-with-effects.md#_snippet_47

LANGUAGE: javascript
CODE:
```
import { useState, useEffect } from 'react';
import { fetchBio } from './api.js';

export default function Page() {
  const [person, setPerson] = useState('Alice');
  const [bio, setBio] = useState(null);
  useEffect(() => {
    let ignore = false;
    setBio(null);
    fetchBio(person).then(result => {
      if (!ignore) {
        setBio(result);
      }
    });
    return () => {
      ignore = true;
    }
  }, [person]);

  return (
    <>
      <select value={person} onChange={e => {
        setPerson(e.target.value);
      }}>
        <option value="Alice">Alice</option>
        <option value="Bob">Bob</option>
        <option value="Taylor">Taylor</option>
      </select>
      <hr />
      <p><i>{bio ?? 'Loading...'}</i></p>
    </>
  );
}
```

LANGUAGE: javascript
CODE:
```
export async function fetchBio(person) {
  const delay = person === 'Bob' ? 2000 : 200;
  return new Promise(resolve => {
    setTimeout(() => {
      resolve('This is ' + person + '’s bio.');
    }, delay);
  })
}
```

----------------------------------------

TITLE: Creating a React Root for a DOM Element
DESCRIPTION: This code demonstrates how to import `createRoot` from `react-dom/client` and use it to establish a React root. It targets a specific DOM element, typically identified by its ID, enabling React to manage its content.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/reference/react-dom/client/createRoot.md#_snippet_1

LANGUAGE: js
CODE:
```
import { createRoot } from 'react-dom/client';

const domNode = document.getElementById('root');
const root = createRoot(domNode);
```

----------------------------------------

TITLE: Implementing Click Counter with useState in React
DESCRIPTION: This snippet provides a complete example of a `MyButton` component that uses `useState` to implement a click counter. The `handleClick` function updates the `count` state by calling `setCount`, causing the component to re-render and display the new count.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/index.md#_snippet_19

LANGUAGE: javascript
CODE:
```
function MyButton() {
  const [count, setCount] = useState(0);

  function handleClick() {
    setCount(count + 1);
  }

  return (
    <button onClick={handleClick}>
      Clicked {count} times
    </button>
  );
}
```

----------------------------------------

TITLE: Avoiding Redundant State with useEffect in React
DESCRIPTION: This snippet demonstrates an anti-pattern in React where `useEffect` is used to derive `fullName` state from `firstName` and `lastName`. This approach is inefficient as it causes an unnecessary re-render with a stale value before updating, and introduces redundant state. It's advised against for performance and simplicity reasons.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/you-might-not-need-an-effect.md#_snippet_0

LANGUAGE: javascript
CODE:
```
function Form() {
  const [firstName, setFirstName] = useState('Taylor');
  const [lastName, setLastName] = useState('Swift');

  // 🔴 Avoid: redundant state and unnecessary Effect
  const [fullName, setFullName] = useState('');
  useEffect(() => {
    setFullName(firstName + ' ' + lastName);
  }, [firstName, lastName]);
  // ...
}
```

----------------------------------------

TITLE: Applying Key Prop to List Item in React JSX
DESCRIPTION: Demonstrates the basic syntax for adding a `key` prop to a list item (`<li>`) within a React component. The `key` prop, in this case `person.id`, is crucial for React to uniquely identify elements in a list, preventing console warnings and enabling efficient DOM reconciliation.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/rendering-lists.md#_snippet_16

LANGUAGE: JavaScript
CODE:
```
<li key={person.id}>...</li>
```

----------------------------------------

TITLE: Full React Application with Theme Context and Toggle (JavaScript)
DESCRIPTION: This comprehensive React application demonstrates the full lifecycle of a theme context. It uses `createContext` to define the theme, `useState` to manage the current theme, and `ThemeContext.Provider` to supply the theme value to child components. `useContext` is used within `Panel` and `Button` to consume the theme, allowing for dynamic styling based on the context value. A toggle button updates the theme state, re-rendering the components with the new context.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/reference/react/useContext.md#_snippet_22

LANGUAGE: JavaScript
CODE:
```
import { createContext, useContext, useState } from 'react';

const ThemeContext = createContext('light');

export default function MyApp() {
  const [theme, setTheme] = useState('light');
  return (
    <>
      <ThemeContext.Provider value={theme}>
        <Form />
      </ThemeContext.Provider>
      <Button onClick={() => {
        setTheme(theme === 'dark' ? 'light' : 'dark');
      }}>
        Toggle theme
      </Button>
    </>
  )
}

function Form({ children }) {
  return (
    <Panel title="Welcome">
      <Button>Sign up</Button>
      <Button>Log in</Button>
    </Panel>
  );
}

function Panel({ title, children }) {
  const theme = useContext(ThemeContext);
  const className = 'panel-' + theme;
  return (
    <section className={className}>
      <h1>{title}</h1>
      {children}
    </section>
  )
}

function Button({ children, onClick }) {
  const theme = useContext(ThemeContext);
  const className = 'button-' + theme;
  return (
    <button className={className} onClick={onClick}>
      {children}
    </button>
  );
}
```

----------------------------------------

TITLE: Implementing Basic Suspense Boundary in React
DESCRIPTION: This snippet illustrates the fundamental usage of React's `Suspense` component. It wraps a potentially suspending component (`<Albums />`) and displays a `Loading` fallback component until the wrapped content is fully loaded and ready to render.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/reference/react/Suspense.md#_snippet_1

LANGUAGE: JavaScript
CODE:
```
<Suspense fallback={<Loading />}>
  <Albums />
</Suspense>
```

----------------------------------------

TITLE: Displaying and Managing Tasks with Context in React
DESCRIPTION: The `TaskList` component consumes the `tasks` array from `TasksContext` and renders individual `Task` components. The nested `Task` component allows editing, marking as done, and deleting tasks. All state modifications are handled by dispatching actions via the `TasksDispatchContext`, demonstrating how child components can interact with global state without prop drilling.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/scaling-up-with-reducer-and-context.md#_snippet_21

LANGUAGE: JavaScript
CODE:
```
import { useState, useContext } from 'react';
import { TasksContext, TasksDispatchContext } from './TasksContext.js';

export default function TaskList() {
  const tasks = useContext(TasksContext);
  return (
    <ul>
      {tasks.map(task => (
        <li key={task.id}>
          <Task task={task} />
        </li>
      ))}
    </ul>
  );
}

function Task({ task }) {
  const [isEditing, setIsEditing] = useState(false);
  const dispatch = useContext(TasksDispatchContext);
  let taskContent;
  if (isEditing) {
    taskContent = (
      <>
        <input
          value={task.text}
          onChange={e => {
            dispatch({
              type: 'changed',
              task: {
                ...task,
                text: e.target.value
              }
            });
          }} />
        <button onClick={() => setIsEditing(false)}>
          Save
        </button>
      </>
    );
  } else {
    taskContent = (
      <>
        {task.text}
        <button onClick={() => setIsEditing(true)}>
          Edit
        </button>
      </>
    );
  }
  return (
    <label>
      <input
        type="checkbox"
        checked={task.done}
        onChange={e => {
          dispatch({
            type: 'changed',
            task: {
              ...task,
              done: e.target.checked
            }
          });
        }}
      />
      {taskContent}
      <button onClick={() => {
        dispatch({
          type: 'deleted',
          id: task.id
        });
      }}>
        Delete
      </button>
    </label>
  );
}
```

----------------------------------------

TITLE: Best Practice: Separating Concerns with Multiple React useEffect Hooks - JavaScript
DESCRIPTION: This example illustrates the recommended approach of using separate useEffect hooks for distinct synchronization processes. One effect handles analytics logging (logVisit), and another manages the chat connection. Both depend on roomId but operate independently, ensuring that changes to one process do not inadvertently trigger the other, improving code clarity and maintainability.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/lifecycle-of-reactive-effects.md#_snippet_14

LANGUAGE: js
CODE:
```
function ChatRoom({ roomId }) {
  useEffect(() => {
    logVisit(roomId);
  }, [roomId]);

  useEffect(() => {
    const connection = createConnection(serverUrl, roomId);
    // ...
  }, [roomId]);
  // ...
}
```

----------------------------------------

TITLE: Problematic Effect Dependency on Options Object (JavaScript)
DESCRIPTION: This JavaScript snippet illustrates a common issue in React 'useEffect' where an object ('options') is declared as a dependency. Because a new 'options' object is created on every render, the effect will re-fire constantly, leading to performance problems like continuous re-connections, even if the 'roomId' hasn't changed.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/reference/react/useMemo.md#_snippet_28

LANGUAGE: javascript
CODE:
```
  useEffect(() => {
    const connection = createConnection(options);
    connection.connect();
    return () => connection.disconnect();
  }, [options]); // 🔴 Problem: This dependency changes on every render
  // ...
```

----------------------------------------

TITLE: Transforming Array Items with map() in React
DESCRIPTION: This React component demonstrates how to immutably transform items in an array stored in state. It uses the `map()` method to create a new array where 'circle' type shapes are moved down by 50 pixels, while 'square' shapes remain unchanged. The `setShapes` function then updates the component's state with this new array, triggering a re-render.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/updating-arrays-in-state.md#_snippet_6

LANGUAGE: javascript
CODE:
```
import { useState } from 'react';

let initialShapes = [
  { id: 0, type: 'circle', x: 50, y: 100 },
  { id: 1, type: 'square', x: 150, y: 100 },
  { id: 2, type: 'circle', x: 250, y: 100 },
];

export default function ShapeEditor() {
  const [shapes, setShapes] = useState(
    initialShapes
  );

  function handleClick() {
    const nextShapes = shapes.map(shape => {
      if (shape.type === 'square') {
        // No change
        return shape;
      } else {
        // Return a new circle 50px below
        return {
          ...shape,
          y: shape.y + 50,
        };
      }
    });
    // Re-render with the new array
    setShapes(nextShapes);
  }

  return (
    <>
      <button onClick={handleClick}>
        Move circles down!
      </button>
      {shapes.map(shape => (
        <div
          key={shape.id}
          style={{
          background: 'purple',
          position: 'absolute',
          left: shape.x,
          top: shape.y,
          borderRadius:
            shape.type === 'circle'
              ? '50%' : '',
          width: 20,
          height: 20,
        }} />
      ))}
    </>
  );
}
```

LANGUAGE: css
CODE:
```
body { height: 300px; }
```

----------------------------------------

TITLE: Using Server Function Reference in Client Component (JS)
DESCRIPTION: A React Client Component (`Button`) receives a reference to a Server Function via props (`onClick`). It logs the reference object and calls the Server Function when the button is clicked, triggering server-side execution.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/reference/rsc/server-functions.md#_snippet_1

LANGUAGE: javascript
CODE:
```
"use client";

export default function Button({onClick}) { 
  console.log(onClick); 
  // {$$typeof: Symbol.for("react.server.reference"), $$id: 'createNoteAction'}
  return <button onClick={() => onClick()}>Create Empty Note</button>
}
```

----------------------------------------

TITLE: Demonstrating XSS Vulnerability with dangerouslySetInnerHTML
DESCRIPTION: Illustrates a critical security vulnerability (XSS) when `dangerouslySetInnerHTML` is used with untrusted input. It shows how malicious HTML, such as an `<img>` tag with an `onerror` attribute, can execute arbitrary JavaScript, leading to security breaches. This snippet serves as a warning against using unsanitized data.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/reference/react-dom/components/common.md#_snippet_37

LANGUAGE: js
CODE:
```
const post = {
  // Imagine this content is stored in the database.
  content: `<img src="" onerror='alert("you were hacked")'>`
};

export default function MarkdownPreview() {
  // 🔴 SECURITY HOLE: passing untrusted input to dangerouslySetInnerHTML
  const markup = { __html: post.content };
  return <div dangerouslySetInnerHTML={markup} />;
}
```

----------------------------------------

TITLE: Marking Server Functions with 'use server' (JavaScript)
DESCRIPTION: The 'use server' directive is used to mark server-side functions that can be invoked directly from client-side code. This allows for server actions or mutations to be triggered from client components, enabling server-side logic execution in response to client interactions.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/reference/rsc/directives.md#_snippet_1

LANGUAGE: JavaScript
CODE:
```
'use server'
```

----------------------------------------

TITLE: Managing Todo List State with React useState - JavaScript
DESCRIPTION: This component serves as the main application container, managing the central `todos` array state using React's `useState` hook. It defines handler functions (`handleAddTodo`, `handleChangeTodo`, `handleDeleteTodo`) that perform immutable updates on the `todos` array, then passes these functions and the state down to child components for interaction.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/reference/react/useState.md#_snippet_18

LANGUAGE: JavaScript
CODE:
```
import { useState } from 'react';
import AddTodo from './AddTodo.js';
import TaskList from './TaskList.js';

let nextId = 3;
const initialTodos = [
  { id: 0, title: 'Buy milk', done: true },
  { id: 1, title: 'Eat tacos', done: false },
  { id: 2, title: 'Brew tea', done: false },
];

export default function TaskApp() {
  const [todos, setTodos] = useState(initialTodos);

  function handleAddTodo(title) {
    setTodos([
      ...todos,
      {
        id: nextId++,
        title: title,
        done: false
      }
    ]);
  }

  function handleChangeTodo(nextTodo) {
    setTodos(todos.map(t => {
      if (t.id === nextTodo.id) {
        return nextTodo;
      } else {
        return t;
      }
    }));
  }

  function handleDeleteTodo(todoId) {
    setTodos(
      todos.filter(t => t.id !== todoId)
    );
  }

  return (
    <>
      <AddTodo
        onAddTodo={handleAddTodo}
      />
      <TaskList
        todos={todos}
        onChangeTodo={handleChangeTodo}
        onDeleteTodo={handleDeleteTodo}
      />
    </>
  );
}
```

----------------------------------------

TITLE: Importing and Using React's lazy for Dynamic Component Loading
DESCRIPTION: This example demonstrates how to import the `lazy` function from the 'react' library and use it to dynamically load a component, `MarkdownPreview`, from a separate file. The `lazy` function is passed an arrow function that returns a Promise, which resolves to the component module.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/reference/react/lazy.md#_snippet_1

LANGUAGE: JavaScript
CODE:
```
import { lazy } from 'react';

const MarkdownPreview = lazy(() => import('./MarkdownPreview.js'));
```

----------------------------------------

TITLE: Hydrating Server-Rendered React Apps with hydrateRoot
DESCRIPTION: This snippet demonstrates the use of `hydrateRoot` for server-rendered React applications. Unlike `createRoot`, `hydrateRoot` attaches React to existing server-generated HTML, preventing the re-creation of DOM nodes from scratch. This improves performance, preserves user input, and maintains focus/scroll positions in SSR applications.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/reference/react-dom/client/createRoot.md#_snippet_28

LANGUAGE: js
CODE:
```
import { hydrateRoot } from 'react-dom/client';
import App from './App.js';

hydrateRoot(
  document.getElementById('root'),
  <App />
);
```

----------------------------------------

TITLE: Conditional Rendering with if/else in React (JavaScript)
DESCRIPTION: Demonstrates how to conditionally render different components based on a boolean state (`isLoggedIn`) using a standard JavaScript `if...else` statement. The result is assigned to a variable which is then included in JSX.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/index.md#_snippet_10

LANGUAGE: js
CODE:
```
let content;
if (isLoggedIn) {
  content = <AdminPanel />;
} else {
  content = <LoginForm />;
}
return (
  <div>
    {content}
  </div>
);
```

----------------------------------------

TITLE: Stopping Synchronization (Cleanup) in React Effect
DESCRIPTION: This snippet focuses on the cleanup function returned by a React `useEffect` hook, which defines how to stop synchronizing with an external system. It demonstrates disconnecting an active connection, ensuring resources are properly released when the effect re-runs or the component unmounts.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/lifecycle-of-reactive-effects.md#_snippet_2

LANGUAGE: javascript
CODE:
```
// ...
    const connection = createConnection(serverUrl, roomId);
    connection.connect();
    return () => {
      connection.disconnect();
    };
    // ...
```

----------------------------------------

TITLE: Updating Nested Objects with Spread Syntax in React (JavaScript)
DESCRIPTION: This React component demonstrates how to update nested objects in state using the `useState` hook and the `...` spread syntax. It shows how to create new copies of the `person` object and its nested `artwork` object when handling input changes, ensuring immutability. Dependencies include React's `useState`.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/adding-interactivity.md#_snippet_14

LANGUAGE: JavaScript
CODE:
```
import { useState } from 'react';

export default function Form() {
  const [person, setPerson] = useState({
    name: 'Niki de Saint Phalle',
    artwork: {
      title: 'Blue Nana',
      city: 'Hamburg',
      image: 'https://i.imgur.com/Sd1AgUOm.jpg',
    }
  });

  function handleNameChange(e) {
    setPerson({
      ...person,
      name: e.target.value
    });
  }

  function handleTitleChange(e) {
    setPerson({
      ...person,
      artwork: {
        ...person.artwork,
        title: e.target.value
      }
    });
  }

  function handleCityChange(e) {
    setPerson({
      ...person,
      artwork: {
        ...person.artwork,
        city: e.target.value
      }
    });
  }

  function handleImageChange(e) {
    setPerson({
      ...person,
      artwork: {
        ...person.artwork,
        image: e.target.value
      }
    });
  }

  return (
    <>
      <label>
        Name:
        <input
          value={person.name}
          onChange={handleNameChange}
        />
      </label>
      <label>
        Title:
        <input
          value={person.artwork.title}
          onChange={handleTitleChange}
        />
      </label>
      <label>
        City:
        <input
          value={person.artwork.city}
          onChange={handleCityChange}
        />
      </label>
      <label>
        Image:
        <input
          value={person.artwork.image}
          onChange={handleImageChange}
        />
      </label>
      <p>
        <i>{person.artwork.title}</i>
        {' by '}
        {person.name}
        <br />
        (located in {person.artwork.city})
      </p>
      <img
        src={person.artwork.image}
        alt={person.artwork.title}
      />
    </>
  );
}
```

LANGUAGE: CSS
CODE:
```
label { display: block; }
input { margin-left: 5px; margin-bottom: 5px; }
img { width: 200px; height: 200px; }
```

----------------------------------------

TITLE: Conditional Rendering with Ternary Operator in React (JSX)
DESCRIPTION: Illustrates a more compact way to conditionally render components directly within JSX using the JavaScript ternary (`? :`) operator. This is suitable for inline conditions.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/index.md#_snippet_11

LANGUAGE: js
CODE:
```
<div>
  {isLoggedIn ? (
    <AdminPanel />
  ) : (
    <LoginForm />
  )}
</div>
```

----------------------------------------

TITLE: Adding Cleanup Function to useEffect
DESCRIPTION: This snippet shows the correct way to manage resources in `useEffect` by returning a cleanup function. This function is called by React before the effect re-runs and when the component unmounts, ensuring resources like connections are properly disconnected.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/synchronizing-with-effects.md#_snippet_17

LANGUAGE: javascript
CODE:
```
useEffect(() => {
    const connection = createConnection();
    connection.connect();
    return () => {
      connection.disconnect();
    };
  }, []);
```

----------------------------------------

TITLE: Incorrect Hook Usage Examples in React - JavaScript
DESCRIPTION: This snippet illustrates various incorrect ways to use React Hooks, leading to 'Rules of Hooks' violations. It shows examples of calling Hooks inside conditions, loops, after conditional returns, within event handlers, inside useMemo callbacks, and within class components.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/warnings/invalid-hook-call-warning.md#_snippet_1

LANGUAGE: JavaScript
CODE:
```
function Bad({ cond }) {
  if (cond) {
    // 🔴 Bad: inside a condition (to fix, move it outside!)
    const theme = useContext(ThemeContext);
  }
  // ...
}

function Bad() {
  for (let i = 0; i < 10; i++) {
    // 🔴 Bad: inside a loop (to fix, move it outside!)
    const theme = useContext(ThemeContext);
  }
  // ...
}

function Bad({ cond }) {
  if (cond) {
    return;
  }
  // 🔴 Bad: after a conditional return (to fix, move it before the return!)
  const theme = useContext(ThemeContext);
  // ...
}

function Bad() {
  function handleClick() {
    // 🔴 Bad: inside an event handler (to fix, move it outside!)
    const theme = useContext(ThemeContext);
  }
  // ...
}

function Bad() {
  const style = useMemo(() => {
    // 🔴 Bad: inside useMemo (to fix, move it outside!)
    const theme = useContext(ThemeContext);
    return createStyle(theme);
  });
  // ...
}

class Bad extends React.Component {
  render() {
    // 🔴 Bad: inside a class component (to fix, write a function component instead of a class!)
    useEffect(() => {})
    // ...
  }
}
```

----------------------------------------

TITLE: Embedding JavaScript Variables in JSX Attributes - JavaScript
DESCRIPTION: This snippet demonstrates how to use JavaScript variables as values for JSX attributes. Unlike string literals which use quotes, dynamic attribute values are enclosed in curly braces, allowing you to pass variable data like `user.imageUrl` to attributes such as `src`.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/index.md#_snippet_7

LANGUAGE: JavaScript
CODE:
```
return (
  <img
    className="avatar"
    src={user.imageUrl}
  />
);
```

----------------------------------------

TITLE: Displaying Time and Color with Clock Component (React JS)
DESCRIPTION: This React functional component, `Clock`, receives `color` and `time` as props. It renders an `<h1>` element displaying the `time` with the specified `color` applied via inline styles. This snippet demonstrates how props are used to customize a component's appearance and content.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/passing-props-to-a-component.md#_snippet_17

LANGUAGE: js
CODE:
```
export default function Clock({ color, time }) {
  return (
    <h1 style={{ color: color }}>
      {time}
    </h1>
  );
}
```

----------------------------------------

TITLE: Managing Text Input State in React
DESCRIPTION: This snippet demonstrates how to manage a string state variable for a text input in React. It uses the `useState` hook to declare `text` and `setText`, and an `onChange` handler to update the state with the input's current value. A reset button is also included.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/reference/react/useState.md#_snippet_7

LANGUAGE: JavaScript
CODE:
```
import { useState } from 'react';

export default function MyInput() {
  const [text, setText] = useState('hello');

  function handleChange(e) {
    setText(e.target.value);
  }

  return (
    <>
      <input value={text} onChange={handleChange} />
      <p>You typed: {text}</p>
      <button onClick={() => setText('hello')}>
        Reset
      </button>
    </>
  );
}
```

----------------------------------------

TITLE: Defining Component Children with React.ReactNode
DESCRIPTION: This interface defines component props where `children` can be any valid React node, including JSX elements, strings, numbers, booleans, or null. Using `React.ReactNode` provides a broad and flexible type definition for content passed as children.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/typescript.md#_snippet_16

LANGUAGE: ts
CODE:
```
interface ModalRendererProps {
  title: string;
  children: React.ReactNode;
}
```

----------------------------------------

TITLE: Resetting State with Key Prop in React
DESCRIPTION: This snippet illustrates the recommended way to reset component state when a prop changes by leveraging React's `key` prop. By passing `userId` as a `key` to the inner `Profile` component, React treats different `userId` values as distinct components, forcing a complete re-mount and state reset for `Profile` and its children, ensuring the `comment` state is automatically cleared.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/you-might-not-need-an-effect.md#_snippet_9

LANGUAGE: JavaScript
CODE:
```
export default function ProfilePage({ userId }) {
  return (
    <Profile
      userId={userId}
      key={userId}
    />
  );
}

function Profile({ userId }) {
  // ✅ This and any other state below will reset on key change automatically
  const [comment, setComment] = useState('');
  // ...
}
```

----------------------------------------

TITLE: Correctly Buying Product with Event Handler in React
DESCRIPTION: This snippet illustrates the correct approach for handling user-initiated actions, such as buying a product. Instead of using `useEffect`, the `fetch` request to `/api/buy` is placed within a `handleClick` function, which would typically be invoked by a user interaction like clicking a button. This ensures the action is triggered only when the user explicitly performs the interaction, preventing unintended duplicate calls.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/synchronizing-with-effects.md#_snippet_29

LANGUAGE: js
CODE:
```
  function handleClick() {
    // ✅ Buying is an event because it is caused by a particular interaction.
    fetch('/api/buy', { method: 'POST' });
  }
```

----------------------------------------

TITLE: Crashing Feedback Form with Conditional useState (JavaScript)
DESCRIPTION: This React component attempts to manage form state but crashes because the `useState` Hook for `message` is called conditionally inside an `else` block, violating the Rules of Hooks. It expects `isSent` and `message` to be managed.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/state-a-components-memory.md#_snippet_33

LANGUAGE: js
CODE:
```
import { useState } from 'react';

export default function FeedbackForm() {
  const [isSent, setIsSent] = useState(false);
  if (isSent) {
    return <h1>Thank you!</h1>;
  } else {
    // eslint-disable-next-line
    const [message, setMessage] = useState('');
    return (
      <form onSubmit={e => {
        e.preventDefault();
        alert(`Sending: "${message}"`);
        setIsSent(true);
      }}>
        <textarea
          placeholder="Message"
          value={message}
          onChange={e => setMessage(e.target.value)}
        />
        <br />
        <button type="submit">Send</button>
      </form>
    );
  }
}
```

----------------------------------------

TITLE: Sending Interaction-Driven POST Requests in React
DESCRIPTION: This corrected `Form` component demonstrates the proper placement of interaction-driven POST requests. The `/api/register` request is moved directly into the `handleSubmit` event handler, ensuring it only fires when the user explicitly submits the form, while the display-driven analytics request remains in an Effect.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/you-might-not-need-an-effect.md#_snippet_16

LANGUAGE: JavaScript
CODE:
```
function Form() {
  const [firstName, setFirstName] = useState('');
  const [lastName, setLastName] = useState('');

  // ✅ Good: This logic runs because the component was displayed
  useEffect(() => {
    post('/analytics/event', { eventName: 'visit_form' });
  }, []);

  function handleSubmit(e) {
    e.preventDefault();
    // ✅ Good: Event-specific logic is in the event handler
    post('/api/register', { firstName, lastName });
  }
  // ...
}
```

----------------------------------------

TITLE: Demonstrating Stale Value Bug with Suppressed useEffect (React, JS)
DESCRIPTION: A React component that tracks mouse position and allows toggling movement via a checkbox. It uses `useEffect` to add/remove a `pointermove` event listener. The effect's dependency array is empty and suppressed, causing the `handleMove` function captured by the listener to always use the initial `canMove` value, leading to a bug where the dot continues moving even when the checkbox is unchecked.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/separating-events-from-effects.md#_snippet_19

LANGUAGE: js
CODE:
```
import { useState, useEffect } from 'react';

export default function App() {
  const [position, setPosition] = useState({ x: 0, y: 0 });
  const [canMove, setCanMove] = useState(true);

  function handleMove(e) {
    if (canMove) {
      setPosition({ x: e.clientX, y: e.clientY });
    }
  }

  useEffect(() => {
    window.addEventListener('pointermove', handleMove);
    return () => window.removeEventListener('pointermove', handleMove);
    // eslint-disable-next-line react-hooks/exhaustive-deps
  }, []);

  return (
    <>
      <label>
        <input type="checkbox"
          checked={canMove}
          onChange={e => setCanMove(e.target.checked)}
        />
        The dot is allowed to move
      </label>
      <hr />
      <div style={{
        position: 'absolute',
        backgroundColor: 'pink',
        borderRadius: '50%',
        opacity: 0.6,
        transform: `translate(${position.x}px, ${position.y}px)`,
        pointerEvents: 'none',
        left: -20,
        top: -20,
        width: 40,
        height: 40,
      }} />
    </>
  );
}
```

----------------------------------------

TITLE: Initial Problematic React Input Reset Example
DESCRIPTION: This snippet demonstrates a React application where the input text unexpectedly resets when a button is pressed. This occurs because conditional rendering (`if/else`) causes the `Form` component to change its position in the DOM tree, leading React to unmount and remount it, thus resetting its internal state. The `App` component manages a `showHint` state to conditionally render a hint and toggle buttons.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/preserving-and-resetting-state.md#_snippet_20

LANGUAGE: javascript
CODE:
```
import { useState } from 'react';

export default function App() {
  const [showHint, setShowHint] = useState(false);
  if (showHint) {
    return (
      <div>
        <p><i>Hint: Your favorite city?</i></p>
        <Form />
        <button onClick={() => {
          setShowHint(false);
        }}>Hide hint</button>
      </div>
    );
  }
  return (
    <div>
      <Form />
      <button onClick={() => {
        setShowHint(true);
      }}>Show hint</button>
    </div>
  );
}

function Form() {
  const [text, setText] = useState('');
  return (
    <textarea
      value={text}
      onChange={e => setText(e.target.value)}
    />
  );
}
```

LANGUAGE: css
CODE:
```
textarea { display: block; margin: 10px 0; }
```

----------------------------------------

TITLE: Solution: Extracted useInterval and Refactored useCounter
DESCRIPTION: This Sandpack shows the solution after refactoring. It includes the main `Counter` component, the refactored `useCounter` Hook which now uses `useInterval`, and the implementation of the new `useInterval` Hook using `useEffect`.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/reusing-logic-with-custom-hooks.md#_snippet_74

LANGUAGE: javascript
CODE:
```
import { useCounter } from './useCounter.js';

export default function Counter() {
  const count = useCounter(1000);
  return <h1>Seconds passed: {count}</h1>;
}
```

LANGUAGE: javascript
CODE:
```
import { useState } from 'react';
import { useInterval } from './useInterval.js';

export function useCounter(delay) {
  const [count, setCount] = useState(0);
  useInterval(() => {
    setCount(c => c + 1);
  }, delay);
  return count;
}
```

LANGUAGE: javascript
CODE:
```
import { useEffect } from 'react';

export function useInterval(onTick, delay) {
  useEffect(() => {
    const id = setInterval(onTick, delay);
    return () => clearInterval(id);
  }, [onTick, delay]);
}
```

----------------------------------------

TITLE: Nesting React Components - JavaScript
DESCRIPTION: This code illustrates how to nest one React component within another. After defining 'MyButton', it's used inside 'MyApp' by treating it as a custom HTML-like tag. React component names must always start with a capital letter to distinguish them from standard HTML tags.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/index.md#_snippet_1

LANGUAGE: JavaScript
CODE:
```
export default function MyApp() {
  return (
    <div>
      <h1>Welcome to my app</h1>
      <MyButton />
    </div>
  );
}
```

----------------------------------------

TITLE: Updating Parent State from Child Component in React
DESCRIPTION: This `SearchBar` component receives state updater functions as props. It uses the `onChange` event handlers on its input fields to call these functions, passing the new input values (`e.target.value` or `e.target.checked`) to update the parent component's state.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/thinking-in-react.md#_snippet_10

LANGUAGE: JavaScript
CODE:
```
function SearchBar({
  filterText,
  inStockOnly,
  onFilterTextChange,
  onInStockOnlyChange
}) {
  return (
    <form>
      <input
        type="text"
        value={filterText}
        placeholder="Search..."
        onChange={(e) => onFilterTextChange(e.target.value)}
      />
      <label>
        <input
          type="checkbox"
          checked={inStockOnly}
          onChange={(e) => onInStockOnlyChange(e.target.checked)}

```

----------------------------------------

TITLE: Enabling StrictMode for an Entire React Application
DESCRIPTION: This snippet demonstrates the recommended way to enable Strict Mode for an entire React application. By wrapping the root component (`<App />`) with `<StrictMode>` during the `root.render()` call, all components within the application tree will benefit from development-only checks, helping to catch bugs related to impure rendering, missing effect cleanup, and deprecated APIs.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/reference/react/StrictMode.md#_snippet_2

LANGUAGE: javascript
CODE:
```
import { StrictMode } from 'react';\nimport { createRoot } from 'react-dom/client';\n\nconst root = createRoot(document.getElementById('root'));\nroot.render(\n  <StrictMode>\n    <App />\n  </StrictMode>\n);
```

----------------------------------------

TITLE: Defining Server Function in Server Component (JS)
DESCRIPTION: Defines an asynchronous Server Function (`createNoteAction`) within a React Server Component (`EmptyNote`) using the `'use server'` directive. This function interacts with a database (`db.notes.create()`) and is passed as a prop (`onClick`) to a Client Component (`Button`).
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/reference/rsc/server-functions.md#_snippet_0

LANGUAGE: javascript
CODE:
```
// Server Component
import Button from './Button';

function EmptyNote () {
  async function createNoteAction() {
    // Server Function
    'use server';
    
    await db.notes.create();
  }

  return <Button onClick={createNoteAction}/>;
}
```

----------------------------------------

TITLE: Full Example: Markdown Editor with Lazy Loading (React/JavaScript)
DESCRIPTION: This comprehensive React component demonstrates a Markdown editor with a lazy-loaded preview. It uses `useState` for managing input and preview visibility, `lazy` for dynamic `MarkdownPreview` loading, and `Suspense` to show a `Loading` component during the preview's initial fetch. A `delayForDemo` function simulates network latency.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/reference/react/lazy.md#_snippet_5

LANGUAGE: JavaScript
CODE:
```
import { useState, Suspense, lazy } from 'react';
import Loading from './Loading.js';

const MarkdownPreview = lazy(() => delayForDemo(import('./MarkdownPreview.js')));

export default function MarkdownEditor() {
  const [showPreview, setShowPreview] = useState(false);
  const [markdown, setMarkdown] = useState('Hello, **world**!');
  return (
    <>
      <textarea value={markdown} onChange={e => setMarkdown(e.target.value)} />
      <label>
        <input type="checkbox" checked={showPreview} onChange={e => setShowPreview(e.target.checked)} />
        Show preview
      </label>
      <hr />
      {showPreview && (
        <Suspense fallback={<Loading />}>
          <h2>Preview</h2>
          <MarkdownPreview markdown={markdown} />
        </Suspense>
      )}
    </>
  );
}

// Add a fixed delay so you can see the loading state
function delayForDemo(promise) {
  return new Promise(resolve => {
    setTimeout(resolve, 2000);
  }).then(() => promise);
}
```

----------------------------------------

TITLE: Declaring State Variables with useState in React
DESCRIPTION: This snippet demonstrates how to declare state variables in a React functional component using the `useState` hook. It shows the common array destructuring pattern `[stateVariable, setFunction]` and how to provide an initial state value. `useState` returns the current state and a function to update it.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/reference/react/useState.md#_snippet_3

LANGUAGE: js
CODE:
```
import { useState } from 'react';

function MyComponent() {
  const [age, setAge] = useState(42);
  const [name, setName] = useState('Taylor');
  // ...
}
```

----------------------------------------

TITLE: Fetching Data with useEffect using Promises in React
DESCRIPTION: This React component demonstrates fetching biographical data using `useEffect` and Promises. It initializes state for `person` and `bio`, and uses a cleanup function with an `ignore` flag to prevent race conditions by discarding stale network responses when the `person` dependency changes.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/reference/react/useEffect.md#_snippet_20

LANGUAGE: JavaScript
CODE:
```
import { useState, useEffect } from 'react';
import { fetchBio } from './api.js';

export default function Page() {
  const [person, setPerson] = useState('Alice');
  const [bio, setBio] = useState(null);

  useEffect(() => {
    let ignore = false;
    setBio(null);
    fetchBio(person).then(result => {
      if (!ignore) {
        setBio(result);
      }
    });
    return () => {
      ignore = true;
    };
  }, [person]);

  // ...
```

----------------------------------------

TITLE: ChatRoom Component with useEffect Cleanup
DESCRIPTION: This complete example demonstrates the `ChatRoom` component with a `useEffect` hook that includes a cleanup function. This ensures that the chat connection is properly disconnected when the component unmounts or is remounted, preventing resource leaks and exhibiting correct behavior in both development and production environments.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/synchronizing-with-effects.md#_snippet_18

LANGUAGE: javascript
CODE:
```
import { useState, useEffect } from 'react';
import { createConnection } from './chat.js';

export default function ChatRoom() {
  useEffect(() => {
    const connection = createConnection();
    connection.connect();
    return () => connection.disconnect();
  }, []);
  return <h1>Welcome to the chat!</h1>;
}
```

LANGUAGE: javascript
CODE:
```
export function createConnection() {
  // A real implementation would actually connect to the server
  return {
    connect() {
      console.log('✅ Connecting...');
    },
    disconnect() {
      console.log('❌ Disconnected.');
    }
  };
}
```

LANGUAGE: css
CODE:
```
input { display: block; margin-bottom: 20px; }
```

----------------------------------------

TITLE: Declaring All Reactive Dependencies in React useEffect
DESCRIPTION: This snippet demonstrates the correct way to declare all reactive values (`serverUrl` and `roomId`) as dependencies for a `useEffect` hook. When these values change, the effect will re-run, ensuring the connection is updated with the latest parameters. It highlights that props and state variables are reactive and must be included.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/reference/react/useEffect.md#_snippet_23

LANGUAGE: JavaScript
CODE:
```
function ChatRoom({ roomId }) { 
  const [serverUrl, setServerUrl] = useState('https://localhost:1234'); 

  useEffect(() => {
    const connection = createConnection(serverUrl, roomId); 
    connection.connect();
    return () => connection.disconnect();
  }, [serverUrl, roomId]); 
  // ...
}
```

----------------------------------------

TITLE: Fetching Data with useEffect (Race Condition Fix) - JavaScript/React
DESCRIPTION: This snippet shows how to fix the race condition in data fetching with `useEffect` by adding a cleanup function. A boolean flag `ignore` is used to prevent `setResults` from being called if a newer request has already been initiated, ensuring only the latest response updates the state.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/you-might-not-need-an-effect.md#_snippet_30

LANGUAGE: JavaScript
CODE:
```
function SearchResults({ query }) {
  const [results, setResults] = useState([]);
  const [page, setPage] = useState(1);
  useEffect(() => {
    let ignore = false;
    fetchResults(query, page).then(json => {
      if (!ignore) {
        setResults(json);
      }
    });
    return () => {
      ignore = true;
    };
  }, [query, page]);

  function handleNextPageClick() {
    setPage(page + 1);
  }
  // ...
}
```

----------------------------------------

TITLE: Streaming Content with React Suspense for Posts
DESCRIPTION: This snippet modifies the ProfilePage component by wrapping the <Posts /> component in a <Suspense> boundary. This allows React to stream the initial HTML for the page, displaying a <PostsGlimmer /> fallback while the data for Posts is loading, improving perceived performance.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/reference/react-dom/server/renderToReadableStream.md#_snippet_11

LANGUAGE: JavaScript
CODE:
```
function ProfilePage() {
  return (
    <ProfileLayout>
      <ProfileCover />
      <Sidebar>
        <Friends />
        <Photos />
      </Sidebar>
      <Suspense fallback={<PostsGlimmer />}>
        <Posts />
      </Suspense>
    </ProfileLayout>
  );
}
```

----------------------------------------

TITLE: Setting state after await using nested startTransition (Correct)
DESCRIPTION: Demonstrates the correct pattern for state updates after an await within an async startTransition callback. A new startTransition call is needed to wrap the state update after the await.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/reference/react/useTransition.md#_snippet_49

LANGUAGE: js
CODE:
```
startTransition(async () => {
  await someAsyncFunction();
  // ✅ Using startTransition *after* await
  startTransition(() => {
    setPage('/about');
  });
});
```

----------------------------------------

TITLE: Optimized Code Splitting with React Router Lazy Loading
DESCRIPTION: This snippet demonstrates an optimized code splitting approach using React Router's `lazy` option. This allows routes to be downloaded in parallel with rendering, ensuring that code is fetched efficiently and reducing the time users wait for content. It requires `createBrowserRouter` from React Router.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/blog/2025/02/14/sunsetting-create-react-app.md#_snippet_6

LANGUAGE: js
CODE:
```
import Home from './Home';
import Dashboard from './Dashboard';

// ✅ Routes are downloaded before rendering
const router = createBrowserRouter([
  {path: '/', lazy: () => import('./Home')},
  {path: '/dashboard', lazy: () => import('Dashboard')}
]);
```

----------------------------------------

TITLE: Coordinated Data Fetching with React Suspense (Multi-file JavaScript/CSS)
DESCRIPTION: This comprehensive multi-file example illustrates a React application using Suspense for coordinated data fetching. `ArtistPage` uses Suspense to manage the loading states of `Biography` and `Albums`, which fetch data via `use` and a simulated `fetchData` utility. The `App` component controls the visibility of the `ArtistPage`, while `Panel` provides basic styling. All components within the Suspense boundary will appear together after their data is loaded, showcasing the 'reveal together' pattern.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/reference/react/Suspense.md#_snippet_4

LANGUAGE: javascript
CODE:
```
import { useState } from 'react';
import ArtistPage from './ArtistPage.js';

export default function App() {
  const [show, setShow] = useState(false);
  if (show) {
    return (
      <ArtistPage
        artist={{
          id: 'the-beatles',
          name: 'The Beatles',
        }}
      />
    );
  } else {
    return (
      <button onClick={() => setShow(true)}>
        Open The Beatles artist page
      </button>
    );
  }
}
```

LANGUAGE: javascript
CODE:
```
import { Suspense } from 'react';
import Albums from './Albums.js';
import Biography from './Biography.js';
import Panel from './Panel.js';

export default function ArtistPage({ artist }) {
  return (
    <>
      <h1>{artist.name}</h1>
      <Suspense fallback={<Loading />}>
        <Biography artistId={artist.id} />
        <Panel>
          <Albums artistId={artist.id} />
        </Panel>
      </Suspense>
    </>
  );
}

function Loading() {
  return <h2>🌀 Loading...</h2>;
}
```

LANGUAGE: javascript
CODE:
```
export default function Panel({ children }) {
  return (
    <section className="panel">
      {children}
    </section>
  );
}
```

LANGUAGE: javascript
CODE:
```
import {use} from 'react';
import { fetchData } from './data.js';

export default function Biography({ artistId }) {
  const bio = use(fetchData(`/${artistId}/bio`));
  return (
    <section>
      <p className="bio">{bio}</p>
    </section>
  );
}
```

LANGUAGE: javascript
CODE:
```
import {use} from 'react';
import { fetchData } from './data.js';

export default function Albums({ artistId }) {
  const albums = use(fetchData(`/${artistId}/albums`));
  return (
    <ul>
      {albums.map(album => (
        <li key={album.id}>
          {album.title} ({album.year})
        </li>
      ))}
    </ul>
  );
}
```

LANGUAGE: javascript
CODE:
```
// Note: the way you would do data fetching depends on
// the framework that you use together with Suspense.
// Normally, the caching logic would be inside a framework.

let cache = new Map();

export function fetchData(url) {
  if (!cache.has(url)) {
    cache.set(url, getData(url));
  }
  return cache.get(url);
}

async function getData(url) {
  if (url === '/the-beatles/albums') {
    return await getAlbums();
  } else if (url === '/the-beatles/bio') {
    return await getBio();
  } else {
    throw Error('Not implemented');
  }
}

async function getBio() {
  // Add a fake delay to make waiting noticeable.
  await new Promise(resolve => {
    setTimeout(resolve, 1500);
  });

  return `The Beatles were an English rock band, \n    formed in Liverpool in 1960, that comprised \n    John Lennon, Paul McCartney, George Harrison \n    and Ringo Starr.`;
}

async function getAlbums() {
  // Add a fake delay to make waiting noticeable.
  await new Promise(resolve => {
    setTimeout(resolve, 3000);
  });

  return [{
    id: 13,
    title: 'Let It Be',
    year: 1970
  }, {
    id: 12,
    title: 'Abbey Road',
    year: 1969
  }, {
    id: 11,
    title: 'Yellow Submarine',
    year: 1969
  }, {
    id: 10,
    title: 'The Beatles',
    year: 1968
  }, {
    id: 9,
    title: 'Magical Mystery Tour',
    year: 1967
  }, {
    id: 8,
    title: 'Sgt. Pepper\'s Lonely Hearts Club Band',
    year: 1967
  }, {
    id: 7,
    title: 'Revolver',
    year: 1966
  }, {
    id: 6,
    title: 'Rubber Soul',
    year: 1965
  }, {
    id: 5,
    title: 'Help!',
    year: 1965
  }, {
    id: 4,
    title: 'Beatles For Sale',
    year: 1964
  }, {
    id: 3,
    title: 'A Hard Day\'s Night',
    year: 1964
  }, {
    id: 2,
    title: 'With The Beatles',
    year: 1963
  }, {
    id: 1,
    title: 'Please Please Me',
    year: 1963
  }];
}
```

LANGUAGE: css
CODE:
```
.bio { font-style: italic; }

.panel {
  border: 1px solid #aaa;
  border-radius: 6px;
  margin-top: 20px;
  padding: 10px;
}
```

----------------------------------------

TITLE: Avoiding Event-Specific Logic in React Effects (Initial)
DESCRIPTION: This snippet demonstrates an incorrect use of `useEffect` for event-specific logic. The `showNotification` call, intended for a button click, is placed in an Effect, causing it to re-trigger on page refresh if `product.isInCart` is true, leading to bugs.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/you-might-not-need-an-effect.md#_snippet_13

LANGUAGE: JavaScript
CODE:
```
function ProductPage({ product, addToCart }) {
  // 🔴 Avoid: Event-specific logic inside an Effect
  useEffect(() => {
    if (product.isInCart) {
      showNotification(`Added ${product.name} to the shopping cart!`);
    }
  }, [product]);

  function handleBuyClick() {
    addToCart(product);
  }

  function handleCheckoutClick() {
    addToCart(product);
    navigateTo('/checkout');
  }
  // ...
}
```

----------------------------------------

TITLE: Example of Direct State Mutation (JavaScript)
DESCRIPTION: This line of JavaScript code explicitly shows a direct mutation of a state property (`person.firstName`). This approach is problematic in React because it bypasses React's state update mechanism, leading to potential rendering issues and making state changes difficult to track.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/updating-objects-in-state.md#_snippet_12

LANGUAGE: js
CODE:
```
person.firstName = e.target.value;
```

----------------------------------------

TITLE: Implementing Tic-Tac-Toe Game Logic in React (JavaScript)
DESCRIPTION: This JavaScript code defines the core components for a Tic-Tac-Toe game in React. It includes `Square` for individual cells, `Board` for the game grid and turn management, and `Game` for overall game state, history, and time travel. The `calculateWinner` helper function determines the game's outcome. It uses React's `useState` hook for managing component state.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/blog/2023/03/16/introducing-react-dev.md#_snippet_0

LANGUAGE: javascript
CODE:
```
import { useState } from 'react';

function Square({ value, onSquareClick }) {
  return (
    <button className="square" onClick={onSquareClick}>
      {value}
    </button>
  );
}

function Board({ xIsNext, squares, onPlay }) {
  function handleClick(i) {
    if (calculateWinner(squares) || squares[i]) {
      return;
    }
    const nextSquares = squares.slice();
    if (xIsNext) {
      nextSquares[i] = 'X';
    } else {
      nextSquares[i] = 'O';
    }
    onPlay(nextSquares);
  }

  const winner = calculateWinner(squares);
  let status;
  if (winner) {
    status = 'Winner: ' + winner;
  } else {
    status = 'Next player: ' + (xIsNext ? 'X' : 'O');
  }

  return (
    <>
      <div className="status">{status}</div>
      <div className="board-row">
        <Square value={squares[0]} onSquareClick={() => handleClick(0)} />
        <Square value={squares[1]} onSquareClick={() => handleClick(1)} />
        <Square value={squares[2]} onSquareClick={() => handleClick(2)} />
      </div>
      <div className="board-row">
        <Square value={squares[3]} onSquareClick={() => handleClick(3)} />
        <Square value={squares[4]} onSquareClick={() => handleClick(4)} />
        <Square value={squares[5]} onSquareClick={() => handleClick(5)} />
      </div>
      <div className="board-row">
        <Square value={squares[6]} onSquareClick={() => handleClick(6)} />
        <Square value={squares[7]} onSquareClick={() => handleClick(7)} />
        <Square value={squares[8]} onSquareClick={() => handleClick(8)} />
      </div>
    </>
  );
}

export default function Game() {
  const [history, setHistory] = useState([Array(9).fill(null)]);
  const [currentMove, setCurrentMove] = useState(0);
  const xIsNext = currentMove % 2 === 0;
  const currentSquares = history[currentMove];

  function handlePlay(nextSquares) {
    const nextHistory = [...history.slice(0, currentMove + 1), nextSquares];
    setHistory(nextHistory);
    setCurrentMove(nextHistory.length - 1);
  }

  function jumpTo(nextMove) {
    setCurrentMove(nextMove);
  }

  const moves = history.map((squares, move) => {
    let description;
    if (move > 0) {
      description = 'Go to move #' + move;
    } else {
      description = 'Go to game start';
    }
    return (
      <li key={move}>
        <button onClick={() => jumpTo(move)}>{description}</button>
      </li>
    );
  });

  return (
    <div className="game">
      <div className="game-board">
        <Board xIsNext={xIsNext} squares={currentSquares} onPlay={handlePlay} />
      </div>
      <div className="game-info">
        <ol>{moves}</ol>
      </div>
    </div>
  );
}

function calculateWinner(squares) {
  const lines = [
    [0, 1, 2],
    [3, 4, 5],
    [6, 7, 8],
    [0, 3, 6],
    [1, 4, 7],
    [2, 5, 8],
    [0, 4, 8],
    [2, 4, 6]
  ];
  for (let i = 0; i < lines.length; i++) {
    const [a, b, c] = lines[i];
    if (squares[a] && squares[a] === squares[b] && squares[a] === squares[c]) {
      return squares[a];
    }
  }
  return null;
}
```

----------------------------------------

TITLE: Importing useState Hook in React
DESCRIPTION: This snippet demonstrates the essential first step for using state in a React component: importing the `useState` Hook from the 'react' library. This Hook is fundamental for managing component-specific data that can change over time.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/index.md#_snippet_17

LANGUAGE: javascript
CODE:
```
import { useState } from 'react';
```

----------------------------------------

TITLE: Calling JavaScript Functions in React JSX for Dynamic Content
DESCRIPTION: This example shows how to execute a JavaScript function directly within JSX using curly braces to generate dynamic content. The `formatDate` function is called with the `today` date object, and its returned string value is embedded into the `<h1>` element. This enables complex data formatting and presentation within the UI.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/javascript-in-jsx-with-curly-braces.md#_snippet_3

LANGUAGE: JavaScript
CODE:
```
const today = new Date();

function formatDate(date) {
  return new Intl.DateTimeFormat(
    'en-US',
    { weekday: 'long' }
  ).format(date);
}

export default function TodoList() {
  return (
    <h1>To Do List for {formatDate(today)}</h1>
  );
}
```

----------------------------------------

TITLE: Grouping Elements with Fragment Shorthand in React
DESCRIPTION: Demonstrates the basic usage of the `<>...</>` shorthand syntax to group multiple React elements without introducing an extra DOM node. This is useful when a component or JSX expression needs to return multiple children but doesn't require a parent wrapper.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/reference/react/Fragment.md#_snippet_0

LANGUAGE: javascript
CODE:
```
<>
  <OneChild />
  <AnotherChild />
</>
```

----------------------------------------

TITLE: Mutating Object in React State (Wrong) - JavaScript
DESCRIPTION: This snippet demonstrates an incorrect way to update an object in React state. Directly mutating the `obj` and then passing the same reference to `setObj` causes React to ignore the update because `Object.is` determines the new state is equal to the previous state. This leads to the UI not updating.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/reference/react/useState.md#_snippet_36

LANGUAGE: js
CODE:
```
obj.x = 10;  // 🚩 Wrong: mutating existing object
setObj(obj); // 🚩 Doesn't do anything
```

----------------------------------------

TITLE: React Form Component Using useFormInput Custom Hook
DESCRIPTION: A refactored version of the Form component that utilizes the useFormInput custom hook to manage the state and change handling for both first name and last name inputs. It demonstrates how a custom hook can encapsulate repetitive form logic.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/reusing-logic-with-custom-hooks.md#_snippet_13

LANGUAGE: js
CODE:
```
import { useFormInput } from './useFormInput.js';

export default function Form() {
  const firstNameProps = useFormInput('Mary');
  const lastNameProps = useFormInput('Poppins');

  return (
    <>
      <label>
        First name:
        <input {...firstNameProps} />
      </label>
      <label>
        Last name:
        <input {...lastNameProps} />
      </label>
      <p><b>Good morning, {firstNameProps.value} {lastNameProps.value}.</b></p>
    </>
  );
}
```

----------------------------------------

TITLE: Deferring SlowList Updates with useDeferredValue
DESCRIPTION: This snippet shows how to apply `useDeferredValue` to improve responsiveness. By creating a `deferredText` value from the input `text`, React is informed that updates to components consuming `deferredText` (like `SlowList`) can be deprioritized. This allows the input to update immediately, while the `SlowList` updates in the background, preventing UI jank during typing.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/reference/react/useDeferredValue.md#_snippet_19

LANGUAGE: js
CODE:
```
function App() {
  const [text, setText] = useState('');
  const deferredText = useDeferredValue(text);
  return (
    <>
      <input value={text} onChange={e => setText(e.target.value)} />
      <SlowList text={deferredText} />
    </>
  );
}
```

----------------------------------------

TITLE: Incorrect Event Handler Call During Render - JavaScript
DESCRIPTION: This snippet illustrates a common mistake where an event handler (`handleClick()`) is called directly during the render phase instead of being passed as a reference. This results in an infinite loop of re-renders and the "Too many re-renders" error because `handleClick()` is executed on every render, which then likely triggers a state update, causing another render.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/reference/react/useState.md#_snippet_38

LANGUAGE: js
CODE:
```
return <button onClick={handleClick()}>Click me</button>
```

----------------------------------------

TITLE: Incorrectly Mutating Nested Object in Copied Array (JavaScript)
DESCRIPTION: This snippet illustrates a common pitfall when updating arrays containing objects in React state. Although `nextList` is a copy of `list`, the objects within the array are still references to the original objects. Directly modifying `nextList[0].seen` mutates the original `list[0]` object, violating React's immutability principle and leading to unexpected behavior or rendering issues.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/updating-arrays-in-state.md#_snippet_10

LANGUAGE: javascript
CODE:
```
const nextList = [...list];
nextList[0].seen = true; // Problem: mutates list[0]
setList(nextList);
```

----------------------------------------

TITLE: Adding a Click Event Handler to a React Button
DESCRIPTION: This example demonstrates how to add an `onClick` event handler to a React button. It defines a `handleClick` function inside the `Button` component, which triggers an `alert` message when the button is clicked, and then passes this function reference to the `onClick` prop.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/responding-to-events.md#_snippet_1

LANGUAGE: js
CODE:
```
export default function Button() {
  function handleClick() {
    alert('You clicked me!');
  }

  return (
    <button onClick={handleClick}>
      Click me
    </button>
  );
}
```

LANGUAGE: css
CODE:
```
button { margin-right: 10px; }
```

----------------------------------------

TITLE: Adjusting State Directly During Rendering (Better)
DESCRIPTION: This snippet presents a more efficient method for adjusting state based on prop changes by performing the state update directly during the rendering phase. It uses a `useState` hook to store the previous `items` prop and conditionally resets `selection` if `items` has changed, avoiding the double-render issue associated with `useEffect`.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/you-might-not-need-an-effect.md#_snippet_11

LANGUAGE: JavaScript
CODE:
```
function List({ items }) {
  const [isReverse, setIsReverse] = useState(false);
  const [selection, setSelection] = useState(null);

  // Better: Adjust the state while rendering
  const [prevItems, setPrevItems] = useState(items);
  if (items !== prevItems) {
    setPrevItems(items);
    setSelection(null);
  }
  // ...
}
```

----------------------------------------

TITLE: Rendering Document Metadata in React Components
DESCRIPTION: This snippet demonstrates how React 19 allows <title>, <meta>, and <link> tags to be rendered directly within a component's JSX. React automatically hoists these metadata tags to the document's <head> section, simplifying metadata management for client-only apps, streaming SSR, and Server Components. The post prop provides dynamic content for the title and keywords.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/blog/2024/12/05/react-19.md#_snippet_16

LANGUAGE: JavaScript
CODE:
```
function BlogPost({post}) {
  return (
    <article>
      <h1>{post.title}</h1>
      <title>{post.title}</title>
      <meta name="author" content="Josh" />
      <link rel="author" href="https://twitter.com/joshcstory/" />
      <meta name="keywords" content={post.keywords} />
      <p>
        Eee equals em-see-squared...
      </p>
    </article>
  );
}
```

----------------------------------------

TITLE: Dynamic Item Transitions with ViewTransition and addTransitionType
DESCRIPTION: This main React JavaScript component demonstrates dynamic addition/removal of an item using `useState` and triggering View Transitions with specific types ('add-video-back', 'remove-video-forward', etc.) based on button clicks. It utilizes `startTransition` and `addTransitionType` along with `ViewTransition` component configured with `enter` and `exit` transition type mappings.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/reference/react/ViewTransition.md#_snippet_28

LANGUAGE: javascript
CODE:
```
import {
  unstable_ViewTransition as ViewTransition,
  unstable_addTransitionType as addTransitionType,
  useState,
  startTransition,
} from "react";
import {Video} from "./Video";
import videos from "./data"

function Item() {
  return (
    <ViewTransition enter={
        {
          "add-video-back": "slide-in-back",
          "add-video-forward": "slide-in-forward"
        }
      }
      exit={
        {
          "remove-video-back": "slide-in-forward",
          "remove-video-forward": "slide-in-back"
        }
      }>
      <Video video={videos[0]}/>
    </ViewTransition>
  );
}

export default function Component() {
  const [showItem, setShowItem] = useState(false);
  return (
    <>
      <div className="button-container">
        <button
          onClick={() => {
            startTransition(() => {
              if (showItem) {
                addTransitionType("remove-video-back")
              } else {
                addTransitionType("add-video-back")
              }
              setShowItem((prev) => !prev);
            });
          }}
        >⬅️</button>
        <button
          onClick={() => {
            startTransition(() => {
              if (showItem) {
                addTransitionType("remove-video-forward")
              } else {
                addTransitionType("add-video-forward")
              }
              setShowItem((prev) => !prev);
            });
          }}
        >➡️</button>
      </div>
      {showItem ? <Item /> : null}
    </>
  );
}
```

----------------------------------------

TITLE: Custom React Hook useChatRoom
DESCRIPTION: Defines a custom React Hook, useChatRoom, which encapsulates the logic for connecting to a chat room. It uses the useEffect hook to manage the connection lifecycle based on the provided server URL and room ID, handling connection, disconnection, and message notifications.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/reusing-logic-with-custom-hooks.md#_snippet_22

LANGUAGE: javascript
CODE:
```
export function useChatRoom({ serverUrl, roomId }) {
  useEffect(() => {
    const options = {
      serverUrl: serverUrl,
      roomId: roomId
    };
    const connection = createConnection(options);
    connection.connect();
    connection.on('message', (msg) => {
      showNotification('New message: ' + msg);
    });
    return () => connection.disconnect();
  }, [roomId, serverUrl]);
}
```

----------------------------------------

TITLE: Updating Nested Object State with Separate Handlers in React (JavaScript)
DESCRIPTION: This React component demonstrates how to update a nested object state using separate event handler functions for each input field. Each handler immutably updates its respective property, including nested ones, by creating new object copies at each level of nesting using the spread syntax (`...`) before updating the state with `setPerson`.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/updating-objects-in-state.md#_snippet_23

LANGUAGE: js
CODE:
```
import { useState } from 'react';

export default function Form() {
  const [person, setPerson] = useState({
    name: 'Niki de Saint Phalle',
    artwork: {
      title: 'Blue Nana',
      city: 'Hamburg',
      image: 'https://i.imgur.com/Sd1AgUOm.jpg',
    }
  });

  function handleNameChange(e) {
    setPerson({
      ...person,
      name: e.target.value
    });
  }

  function handleTitleChange(e) {
    setPerson({
      ...person,
      artwork: {
        ...person.artwork,
        title: e.target.value
      }
    });
  }

  function handleCityChange(e) {
    setPerson({
      ...person,
      artwork: {
        ...person.artwork,
        city: e.target.value
      }
    });
  }

  function handleImageChange(e) {
    setPerson({
      ...person,
      artwork: {
        ...person.artwork,
        image: e.target.value
      }
    });
  }

  return (
    <>
      <label>
        Name:
        <input
          value={person.name}
          onChange={handleNameChange}
        />
      </label>
      <label>
        Title:
        <input
          value={person.artwork.title}
          onChange={handleTitleChange}
        />
      </label>
      <label>
        City:
        <input
          value={person.artwork.city}
          onChange={handleCityChange}
        />
      </label>
      <label>
        Image:
        <input
          value={person.artwork.image}
          onChange={handleImageChange}
        />
      </label>
      <p>
        <i>{person.artwork.title}</i>
        {' by '}
        {person.name}
        <br />
        (located in {person.artwork.city})
      </p>
      <img 
        src={person.artwork.image} 
        alt={person.artwork.title}
      />
    </>
  );
}
```

----------------------------------------

TITLE: Interactive Client Component with useState in React
DESCRIPTION: This JavaScript code defines a React Client Component (`Expandable`) using the `useState` hook to manage its internal state. The `'use client'` directive marks it as a client-side component, allowing it to use interactive browser APIs and provide dynamic behavior.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/reference/rsc/server-components.md#_snippet_8

LANGUAGE: JavaScript
CODE:
```
// Client Component
"use client"

export default function Expandable({children}) {
  const [expanded, setExpanded] = useState(false);
  return (
    <div>
      <button
        onClick={() => setExpanded(!expanded)}
      >
        Toggle
      </button>
      {expanded && children}
    </div>
  );
}
```

----------------------------------------

TITLE: Using React 19 Resource Preloading APIs
DESCRIPTION: This JavaScript example illustrates the new React 19 APIs for optimizing resource loading. It shows how to use `preinit` for eager script execution, `preload` for fonts and stylesheets, `prefetchDNS` for DNS resolution, and `preconnect` for establishing early connections, all aimed at improving page performance.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/blog/2024/12/05/react-19.md#_snippet_20

LANGUAGE: JavaScript
CODE:
```
import { prefetchDNS, preconnect, preload, preinit } from 'react-dom'
function MyComponent() {
  preinit('https://.../path/to/some/script.js', {as: 'script' }) // loads and executes this script eagerly
  preload('https://.../path/to/font.woff', { as: 'font' }) // preloads this font
  preload('https://.../path/to/stylesheet.css', { as: 'style' }) // preloads this stylesheet
  prefetchDNS('https://...') // when you may not actually request anything from this host
  preconnect('https://...') // when you will request something but aren't sure what
}
```

----------------------------------------

TITLE: Building a Stopwatch with useState and useRef in React
DESCRIPTION: This React component implements a stopwatch using a combination of `useState` for rendering-related time values (`startTime`, `now`) and `useRef` for storing the non-rendering interval ID (`intervalRef`). This approach allows the component to update the displayed time while correctly managing the `setInterval` and `clearInterval` operations.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/reference/react/useRef.md#_snippet_6

LANGUAGE: JavaScript
CODE:
```
import { useState, useRef } from 'react';

export default function Stopwatch() {
  const [startTime, setStartTime] = useState(null);
  const [now, setNow] = useState(null);
  const intervalRef = useRef(null);

  function handleStart() {
    setStartTime(Date.now());
    setNow(Date.now());

    clearInterval(intervalRef.current);
    intervalRef.current = setInterval(() => {
      setNow(Date.now());
    }, 10);
  }

  function handleStop() {
    clearInterval(intervalRef.current);
  }

  let secondsPassed = 0;
  if (startTime != null && now != null) {
    secondsPassed = (now - startTime) / 1000;
  }

  return (
    <>
      <h1>Time passed: {secondsPassed.toFixed(3)}</h1>
      <button onClick={handleStart}>
        Start
      </button>
      <button onClick={handleStop}>
        Stop
      </button>
    </>
  );
}
```

----------------------------------------

TITLE: Fixing Chat Input with `useRef` for `setTimeout` - JavaScript
DESCRIPTION: This corrected React component uses `useRef` to store the `timeoutID`. By storing the ID in `timeoutRef.current`, its value persists across re-renders, allowing `clearTimeout` in `handleUndo` to correctly reference and clear the active timeout, preventing the 'Sent!' alert from appearing after 'Undo' is clicked.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/referencing-values-with-refs.md#_snippet_12

LANGUAGE: javascript
CODE:
```
import { useState, useRef } from 'react';

export default function Chat() {
  const [text, setText] = useState('');
  const [isSending, setIsSending] = useState(false);
  const timeoutRef = useRef(null);

  function handleSend() {
    setIsSending(true);
    timeoutRef.current = setTimeout(() => {
      alert('Sent!');
      setIsSending(false);
    }, 3000);
  }

  function handleUndo() {
    setIsSending(false);
    clearTimeout(timeoutRef.current);
  }

  return (
    <>
      <input
        disabled={isSending}
        value={text}
        onChange={e => setText(e.target.value)}
      />
      <button
        disabled={isSending}
        onClick={handleSend}>
        {isSending ? 'Sending...' : 'Send'}
      </button>
      {isSending &&
        <button onClick={handleUndo}>
          Undo
        </button>
      }
    </>
  );
}
```

----------------------------------------

TITLE: Consolidated React Context, Reducer, and Provider for Tasks
DESCRIPTION: This comprehensive `TasksContext.js` file consolidates all task-related state management logic. It defines the `TasksContext` and `TasksDispatchContext`, implements the `TasksProvider` component, includes the `tasksReducer` function for state transitions, and initializes the `initialTasks` array, centralizing the entire task state system.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/scaling-up-with-reducer-and-context.md#_snippet_27

LANGUAGE: js
CODE:
```
import { createContext, useReducer } from 'react';

export const TasksContext = createContext(null);
export const TasksDispatchContext = createContext(null);

export function TasksProvider({ children }) {
  const [tasks, dispatch] = useReducer(
    tasksReducer,
    initialTasks
  );

  return (
    <TasksContext.Provider value={tasks}>
      <TasksDispatchContext.Provider value={dispatch}>
        {children}
      </TasksDispatchContext.Provider>
    </TasksContext.Provider>
  );
}

function tasksReducer(tasks, action) {
  switch (action.type) {
    case 'added': {
      return [...tasks, {
        id: action.id,
        text: action.text,
        done: false
      }];
    }
    case 'changed': {
      return tasks.map(t => {
        if (t.id === action.task.id) {
          return action.task;
        } else {
          return t;
        }
      });
    }
    case 'deleted': {
      return tasks.filter(t => t.id !== action.id);
    }
    default: {
      throw Error('Unknown action: ' + action.type);
    }
  }
}

const initialTasks = [
  { id: 0, text: 'Philosopher’s Path', done: true },
  { id: 1, text: 'Visit the temple', done: false },
  { id: 2, text: 'Drink matcha', done: false }
];
```

----------------------------------------

TITLE: Optimizing Object Props with useMemo in React
DESCRIPTION: This snippet demonstrates how to prevent unnecessary re-renders of a `memo`-ized `Profile` component when passing an object prop. By using `useMemo`, the `person` object is memoized and only re-created if `name` or `age` change, ensuring the `Profile` component only re-renders when its `person` prop's reference actually changes.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/reference/react/memo.md#_snippet_6

LANGUAGE: JavaScript
CODE:
```
function Page() {
  const [name, setName] = useState('Taylor');
  const [age, setAge] = useState(42);

  const person = useMemo(
    () => ({ name, age }),
    [name, age]
  );

  return <Profile person={person} />;
}

const Profile = memo(function Profile({ person }) {
  // ...
});
```

----------------------------------------

TITLE: React Todo Application with Artificially Slowed Filtering
DESCRIPTION: This set of code files demonstrates a React application where the todo filtering logic is intentionally slowed down to highlight performance issues when an expensive function is re-executed on every render without memoization. It includes the main App component, the TodoList component, utility functions for todo creation and filtering, and basic styling.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/reference/react/useMemo.md#_snippet_10

LANGUAGE: JavaScript
CODE:
```
import { useState } from 'react';
import { createTodos } from './utils.js';
import TodoList from './TodoList.js';

const todos = createTodos();

export default function App() {
  const [tab, setTab] = useState('all');
  const [isDark, setIsDark] = useState(false);
  return (
    <>
      <button onClick={() => setTab('all')}>
        All
      </button>
      <button onClick={() => setTab('active')}>
        Active
      </button>
      <button onClick={() => setTab('completed')}>
        Completed
      </button>
      <br />
      <label>
        <input
          type="checkbox"
          checked={isDark}
          onChange={e => setIsDark(e.target.checked)}
        />
        Dark mode
      </label>
      <hr />
      <TodoList
        todos={todos}
        tab={tab}
        theme={isDark ? 'dark' : 'light'}
      />
    </>
  );
}
```

LANGUAGE: JavaScript
CODE:
```
import { filterTodos } from './utils.js'

export default function TodoList({ todos, theme, tab }) {
  const visibleTodos = filterTodos(todos, tab);
  return (
    <div className={theme}>
      <ul>
        <p><b>Note: <code>filterTodos</code> is artificially slowed down!</b></p>
        {visibleTodos.map(todo => (
          <li key={todo.id}>
            {todo.completed ?
              <s>{todo.text}</s> :
              todo.text
            }
          </li>
        ))}
      </ul>
    </div>
  );
}
```

LANGUAGE: JavaScript
CODE:
```
export function createTodos() {
  const todos = [];
  for (let i = 0; i < 50; i++) {
    todos.push({
      id: i,
      text: "Todo " + (i + 1),
      completed: Math.random() > 0.5
    });
  }
  return todos;
}

export function filterTodos(todos, tab) {
  console.log('[ARTIFICIALLY SLOW] Filtering ' + todos.length + ' todos for "' + tab + '" tab.');
  let startTime = performance.now();
  while (performance.now() - startTime < 500) {
    // Do nothing for 500 ms to emulate extremely slow code
  }

  return todos.filter(todo => {
    if (tab === 'all') {
      return true;
    } else if (tab === 'active') {
      return !todo.completed;
    } else if (tab === 'completed') {
      return todo.completed;
    }
  });
}
```

LANGUAGE: CSS
CODE:
```
label {
  display: block;
  margin-top: 10px;
}

.dark {
  background-color: black;
  color: white;
}

.light {
  background-color: white;
  color: black;
}
```

----------------------------------------

TITLE: Preventing Direct Prop Mutation in React Components (JavaScript)
DESCRIPTION: This snippet illustrates the incorrect practice of directly mutating a prop (`item.url`) within a React component. Mutating props can lead to inconsistent application behavior and is difficult to debug. Props should be treated as immutable snapshots.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/reference/rules/components-and-hooks-must-be-pure.md#_snippet_9

LANGUAGE: javascript
CODE:
```
function Post({ item }) {
  item.url = new Url(item.url, base); // 🔴 Bad: never mutate props directly
  return <Link url={item.url}>{item.title}</Link>;
}
```

----------------------------------------

TITLE: Misplaced State in React List (Problematic Index Keying)
DESCRIPTION: This set of code snippets demonstrates a common issue in React where component state (like 'expanded' for contact email) becomes misassociated with the wrong data item when the list order changes. This occurs because the `key` prop for list items is incorrectly set to the array index (`i`), which changes when the list is reversed, leading to state being applied to a different contact. The `ContactList` component manages the list and its reversal, while `Contact` manages its own display state.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/preserving-and-resetting-state.md#_snippet_31

LANGUAGE: JavaScript
CODE:
```
import { useState } from 'react';
import Contact from './Contact.js';

export default function ContactList() {
  const [reverse, setReverse] = useState(false);

  const displayedContacts = [...contacts];
  if (reverse) {
    displayedContacts.reverse();
  }

  return (
    <>
      <label>
        <input
          type="checkbox"
          checked={reverse}
          onChange={e => {
            setReverse(e.target.checked)
          }}
        />{' '}
        Show in reverse order
      </label>
      <ul>
        {displayedContacts.map((contact, i) =>
          <li key={i}>
            <Contact contact={contact} />
          </li>
        )}
      </ul>
    </>
  );
}

const contacts = [
  { id: 0, name: 'Alice', email: 'alice@mail.com' },
  { id: 1, name: 'Bob', email: 'bob@mail.com' },
  { id: 2, name: 'Taylor', email: 'taylor@mail.com' }
];
```

LANGUAGE: JavaScript
CODE:
```
import { useState } from 'react';

export default function Contact({ contact }) {
  const [expanded, setExpanded] = useState(false);
  return (
    <>
      <p><b>{contact.name}</b></p>
      {expanded &&
        <p><i>{contact.email}</i></p>
      }
      <button onClick={() => {
        setExpanded(!expanded);
      }}>
        {expanded ? 'Hide' : 'Show'} email
      </button>
    </>
  );
}
```

LANGUAGE: CSS
CODE:
```
ul, li {
  list-style: none;
  margin: 0;
  padding: 0;
}
li {
  margin-bottom: 20px;
}
label {
  display: block;
  margin: 10px 0;
}
button {
  margin-right: 10px;
  margin-bottom: 10px;
}
```

----------------------------------------

TITLE: Incorrect `useMemo` Usage: Missing Dependency Array (JavaScript)
DESCRIPTION: This snippet demonstrates incorrect usage of `useMemo` where the dependency array is omitted. This causes the `filterTodos` calculation to re-run on every render, defeating the purpose of memoization. It highlights a common pitfall in `useMemo` implementation.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/reference/react/useMemo.md#_snippet_43

LANGUAGE: JavaScript
CODE:
```
function TodoList({ todos, tab }) {
  // 🔴 Recalculates every time: no dependency array
  const visibleTodos = useMemo(() => filterTodos(todos, tab));
  // ...
```

----------------------------------------

TITLE: React Chat Room Component (Bugged Notification)
DESCRIPTION: Implements a React component simulating a chat room connection with a delayed welcome notification using useEffect and useEffectEvent. Contains a bug where fast room switching causes notifications to show the incorrect room name due to stale closure.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/separating-events-from-effects.md#_snippet_33

LANGUAGE: js
CODE:
```
import { useState, useEffect } from 'react';
import { experimental_useEffectEvent as useEffectEvent } from 'react';
import { createConnection, sendMessage } from './chat.js';
import { showNotification } from './notifications.js';

const serverUrl = 'https://localhost:1234';

function ChatRoom({ roomId, theme }) {
  const onConnected = useEffectEvent(() => {
    showNotification('Welcome to ' + roomId, theme);
  });

  useEffect(() => {
    const connection = createConnection(serverUrl, roomId);
    connection.on('connected', () => {
      setTimeout(() => {
        onConnected();
      }, 2000);
    });
    connection.connect();
    return () => connection.disconnect();
  }, [roomId]);

  return <h1>Welcome to the {roomId} room!</h1>
}

export default function App() {
  const [roomId, setRoomId] = useState('general');
  const [isDark, setIsDark] = useState(false);
  return (
    <>
      <label>
        Choose the chat room:{' '}
        <select
          value={roomId}
          onChange={e => setRoomId(e.target.value)}
        >
          <option value="general">general</option>
          <option value="travel">travel</option>
          <option value="music">music</option>
        </select>
      </label>
      <label>
        <input
          type="checkbox"
          checked={isDark}
          onChange={e => setIsDark(e.target.checked)}
        />
        Use dark theme
      </label>
      <hr />
      <ChatRoom
        roomId={roomId}
        theme={isDark ? 'dark' : 'light'}
      />
    </>
  );
}
```

----------------------------------------

TITLE: Adding Items to React State Array (Spread Syntax)
DESCRIPTION: This snippet illustrates the correct, immutable way to add an item to an array in React state. By using the array spread syntax (`...artists`), a new array is created containing all existing items plus the new item, ensuring React detects the state change and triggers a re-render.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/updating-arrays-in-state.md#_snippet_1

LANGUAGE: javascript
CODE:
```
setArtists( // Replace the state
  [ // with a new array
    ...artists, // that contains all the old items
    { id: nextId++, name: name } // and one new item at the end
  ]
);
```

----------------------------------------

TITLE: Reversing an Array in React State (JavaScript)
DESCRIPTION: This snippet demonstrates how to reverse an array stored in React state without directly mutating the original state. It achieves this by first creating a shallow copy of the array using the spread syntax (`[...list]`), then applying the `reverse()` method to the copy, and finally updating the state with the new array. This approach ensures immutability, which is crucial for predictable state management in React.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/updating-arrays-in-state.md#_snippet_9

LANGUAGE: javascript
CODE:
```
import { useState } from 'react';

const initialList = [
  { id: 0, title: 'Big Bellies' },
  { id: 1, title: 'Lunar Landscape' },
  { id: 2, title: 'Terracotta Army' },
];

export default function List() {
  const [list, setList] = useState(initialList);

  function handleClick() {
    const nextList = [...list];
    nextList.reverse();
    setList(nextList);
  }

  return (
    <>
      <button onClick={handleClick}>
        Reverse
      </button>
      <ul>
        {list.map(artwork => (
          <li key={artwork.id}>{artwork.title}</li>
        ))}
      </ul>
    </>
  );
}
```

----------------------------------------

TITLE: Managing Dependent State in React with Event Handlers
DESCRIPTION: This snippet presents the recommended approach for managing dependent state updates in React. Instead of chaining Effects, it calculates derived state (`isGameOver`) during rendering and updates all related state variables within a single event handler (`handlePlaceCard`). This method is more efficient, reduces unnecessary re-renders, and improves code flexibility for features like game history.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/you-might-not-need-an-effect.md#_snippet_18

LANGUAGE: JavaScript
CODE:
```
function Game() {
  const [card, setCard] = useState(null);
  const [goldCardCount, setGoldCardCount] = useState(0);
  const [round, setRound] = useState(1);

  // ✅ Calculate what you can during rendering
  const isGameOver = round > 5;

  function handlePlaceCard(nextCard) {
    if (isGameOver) {
      throw Error('Game already ended.');
    }

    // ✅ Calculate all the next state in the event handler
    setCard(nextCard);
    if (nextCard.gold) {
      if (goldCardCount <= 3) {
        setGoldCardCount(goldCardCount + 1);
      } else {
        setGoldCardCount(0);
        setRound(round + 1);
        if (round === 5) {
          alert('Good game!');
        }
      }
    }
  }

  // ...
}
```

----------------------------------------

TITLE: Fetching Data with Custom Hook (useData) - JavaScript/React
DESCRIPTION: This snippet demonstrates extracting data fetching logic into a reusable custom Hook named `useData`. This hook encapsulates the `useEffect` with cleanup logic for race conditions, making the `SearchResults` component cleaner and more declarative. The `useData` hook takes a URL and returns the fetched data, promoting better code organization and reusability.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/you-might-not-need-an-effect.md#_snippet_31

LANGUAGE: JavaScript
CODE:
```
function SearchResults({ query }) {
  const [page, setPage] = useState(1);
  const params = new URLSearchParams({ query, page });
  const results = useData(`/api/search?${params}`);

  function handleNextPageClick() {
    setPage(page + 1);
  }
  // ...
}

function useData(url) {
  const [data, setData] = useState(null);
  useEffect(() => {
    let ignore = false;
    fetch(url)
      .then(response => response.json())
      .then(json => {
        if (!ignore) {
          setData(json);
        }
      });
    return () => {
      ignore = true;
    };
  }, [url]);
  return data;
}
```

----------------------------------------

TITLE: Accessing Form Status with React useFormStatus Hook
DESCRIPTION: This React component demonstrates the `useFormStatus` hook, which allows child components to access the pending status of their parent `<form>` without prop drilling. It's useful for disabling buttons or showing loading indicators during form submission.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/blog/2024/12/05/react-19.md#_snippet_5

LANGUAGE: JavaScript
CODE:
```
import {useFormStatus} from 'react-dom';

function DesignButton() {
  const {pending} = useFormStatus();
  return <button type="submit" disabled={pending} />
}
```

----------------------------------------

TITLE: Initializing useOptimistic Hook in Component - JavaScript
DESCRIPTION: Imports the `useOptimistic` hook and demonstrates its basic usage within a functional React component (`AppContainer`), showing how to define the `updateFn` to merge the current state with an optimistic value. Requires importing `useOptimistic` from 'react'.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/reference/react/useOptimistic.md#_snippet_1

LANGUAGE: js
CODE:
```
import { useOptimistic } from 'react';

function AppContainer() {
  const [optimisticState, addOptimistic] = useOptimistic(
    state,
    // updateFn
    (currentState, optimisticValue) => {
      // merge and return new state
      // with optimistic value
    }
  );
}
```

----------------------------------------

TITLE: Managing Chat Connection with useEffect in React
DESCRIPTION: This example demonstrates how to use `useEffect` within a `ChatRoom` functional component to manage an external chat connection. The Effect connects to a chat server when the component mounts or when `serverUrl` or `roomId` dependencies change, and disconnects via a cleanup function when the component unmounts or before the Effect re-runs. It requires `useState` from React and a `createConnection` utility.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/reference/react/useEffect.md#_snippet_1

LANGUAGE: javascript
CODE:
```
import { useState, useEffect } from 'react';
import { createConnection } from './chat.js';

function ChatRoom({ roomId }) {
  const [serverUrl, setServerUrl] = useState('https://localhost:1234');

  useEffect(() => {
    const connection = createConnection(serverUrl, roomId);
    connection.connect();
    return () => {
      connection.disconnect();
    };
  }, [serverUrl, roomId]);
  // ...
}
```

----------------------------------------

TITLE: Calculating Derived State During Rendering (JavaScript)
DESCRIPTION: This example shows the correct and preferred way to handle derived state in React. Instead of using `useEffect` to update `fullName`, it is calculated directly during the component's render phase. This approach is more efficient, avoids redundant state, and aligns better with React's declarative nature, as `fullName` is always up-to-date with `firstName` and `lastName` without side effects.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/escape-hatches.md#_snippet_6

LANGUAGE: JavaScript
CODE:
```
function Form() {
  const [firstName, setFirstName] = useState('Taylor');
  const [lastName, setLastName] = useState('Swift');
  // ✅ Good: calculated during rendering
  const fullName = firstName + ' ' + lastName;
  // ...
}
```

----------------------------------------

TITLE: Defining useSelectOptions Custom Hook in React
DESCRIPTION: This custom React Hook, `useSelectOptions`, manages fetching data from a given URL and maintaining the selected item's ID. It uses `useState` for managing the list and selected ID, and `useEffect` to perform data fetching when the `url` changes, including a cleanup function to prevent state updates on unmounted components. It returns the list, selected ID, and a setter for the selected ID.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/lifecycle-of-reactive-effects.md#_snippet_59

LANGUAGE: JavaScript
CODE:
```
import { useState, useEffect } from 'react';
import { fetchData } from './api.js';

export function useSelectOptions(url) {
  const [list, setList] = useState(null);
  const [selectedId, setSelectedId] = useState('');
  useEffect(() => {
    if (url === null) {
      return;
    }

    let ignore = false;
    fetchData(url).then(result => {
      if (!ignore) {
        setList(result);
        setSelectedId(result[0].id);
      }
    });
    return () => {
      ignore = true;
    }
  }, [url]);
  return [list, selectedId, setSelectedId];
}
```

----------------------------------------

TITLE: Incorrect State Update in useEffect with Dependency - JavaScript
DESCRIPTION: This snippet demonstrates a common pitfall when updating state within a `useEffect` hook based on its previous value. By including `count` in the dependency array, the effect re-runs and resets the `setInterval` every time `count` changes, which is not the desired behavior for a continuous counter.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/reference/react/useEffect.md#_snippet_38

LANGUAGE: JavaScript
CODE:
```
function Counter() {
  const [count, setCount] = useState(0);

  useEffect(() => {
    const intervalId = setInterval(() => {
      setCount(count + 1); // You want to increment the counter every second...
    }, 1000)
    return () => clearInterval(intervalId);
  }, [count]); // 🚩 ... but specifying `count` as a dependency always resets the interval.
  // ...
}
```

----------------------------------------

TITLE: Triggering Initial Render in React
DESCRIPTION: This snippet demonstrates how to trigger the initial render of a React application. It uses `createRoot` to create a root DOM node and then calls its `render` method to mount the `Image` component into the DOM. This is the entry point for a React application.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/render-and-commit.md#_snippet_0

LANGUAGE: JavaScript
CODE:
```
import Image from './Image.js';
import { createRoot } from 'react-dom/client';

const root = createRoot(document.getElementById('root'))
root.render(<Image />);
```

----------------------------------------

TITLE: Managing Chat Room Connection with React useEffect
DESCRIPTION: This snippet demonstrates how to manage a chat room connection using React's `useEffect` hook. The `ChatRoom` component connects to a server when mounted and disconnects when unmounted or when the `roomId` dependency changes, ensuring synchronization. The `App` component manages the `roomId` state, allowing users to switch chat rooms.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/escape-hatches.md#_snippet_7

LANGUAGE: javascript
CODE:
```
import { useState, useEffect } from 'react';
import { createConnection } from './chat.js';

const serverUrl = 'https://localhost:1234';

function ChatRoom({ roomId }) {
  useEffect(() => {
    const connection = createConnection(serverUrl, roomId);
    connection.connect();
    return () => connection.disconnect();
  }, [roomId]);

  return <h1>Welcome to the {roomId} room!</h1>;
}

export default function App() {
  const [roomId, setRoomId] = useState('general');
  return (
    <>
      <label>
        Choose the chat room:{' '}
        <select
          value={roomId}
          onChange={e => setRoomId(e.target.value)}
        >
          <option value="general">general</option>
          <option value="travel">travel</option>
          <option value="music">music</option>
        </select>
      </label>
      <hr />
      <ChatRoom roomId={roomId} />
    </>
  );
}
```

----------------------------------------

TITLE: Identifying Direct Object Mutation in React State
DESCRIPTION: This code snippet illustrates the problematic pattern of directly mutating an object (`artwork.seen = nextSeen`) after finding it within a copied array. While `myNextList` is a new array, the `artwork` object itself is a reference to an item from the original `myList`, leading to unintended side effects and shared state bugs.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/updating-arrays-in-state.md#_snippet_12

LANGUAGE: javascript
CODE:
```
const myNextList = [...myList];
const artwork = myNextList.find(a => a.id === artworkId);
artwork.seen = nextSeen; // Problem: mutates an existing item
setMyList(myNextList);
```

----------------------------------------

TITLE: Exporting Server Function from Dedicated File (JS)
DESCRIPTION: Defines and exports an asynchronous Server Function (`createNote`) in a separate file using the `'use server'` directive. This function performs a database operation (`db.notes.create()`) and is intended to be imported and called from Client Components.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/reference/rsc/server-functions.md#_snippet_2

LANGUAGE: javascript
CODE:
```
"use server";

export async function createNote() {
  await db.notes.create();
}
```

----------------------------------------

TITLE: Combined React Components Example - JavaScript
DESCRIPTION: This comprehensive example combines the 'MyButton' and 'MyApp' components, showcasing how they work together in a single file. It demonstrates the full structure of a simple React application with component definition and nesting, providing a runnable context.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/index.md#_snippet_2

LANGUAGE: JavaScript
CODE:
```
function MyButton() {
  return (
    <button>
      I'm a button
    </button>
  );
}

export default function MyApp() {
  return (
    <div>
      <h1>Welcome to my app</h1>
      <MyButton />
    </div>
  );
}
```

----------------------------------------

TITLE: Artist Biography Component with Data Fetching, JavaScript
DESCRIPTION: Fetches and displays the biography for a given artist ID using React's 'use' hook and a 'fetchData' utility. It renders the biography text within a section, suspending until the data is resolved.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/reference/react/Suspense.md#_snippet_30

LANGUAGE: JavaScript
CODE:
```
import {use} from 'react';
import { fetchData } from './data.js';

export default function Biography({ artistId }) {
  const bio = use(fetchData(`/${artistId}/bio`));
  return (
    <section>
      <p className="bio">{bio}</p>
    </section>
  );
}
```

----------------------------------------

TITLE: Nesting Suspense for Staged Content Loading in React
DESCRIPTION: This JSX snippet demonstrates how to nest `Suspense` components to create a progressive loading sequence. The outer `Suspense` shows a `BigSpinner` while `Biography` loads, and once `Biography` is ready, the inner `Suspense` takes over, displaying `AlbumsGlimmer` until `Albums` finishes loading. This allows for a more granular and user-friendly loading experience.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/reference/react/Suspense.md#_snippet_7

LANGUAGE: JSX
CODE:
```
<Suspense fallback={<BigSpinner />}>
  <Biography />
  <Suspense fallback={<AlbumsGlimmer />}>
    <Panel>
      <Albums />
    </Panel>
  </Suspense>
</Suspense>
```

----------------------------------------

TITLE: React Effect with Changed Dependencies and Cleanup
DESCRIPTION: This code shows a React Effect with updated dependencies (`['travel']`). When dependencies change, React first executes the cleanup function of the *previous* Effect (if it ran) and then runs the new Effect, ensuring proper resource management like disconnecting from a chat room before connecting to another.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/synchronizing-with-effects.md#_snippet_35

LANGUAGE: javascript
CODE:
```
// Effect for the third render (roomId = "travel")
  () => {
    const connection = createConnection('travel');
    connection.connect();
    return () => connection.disconnect();
  },
  // Dependencies for the third render (roomId = "travel")
  ['travel']
```

----------------------------------------

TITLE: Correctly Updating Hook Arguments by Copying (JavaScript)
DESCRIPTION: This example demonstrates the correct way to update a value that will be passed as a hook argument. Instead of mutating the original `icon` object, a new object is created with the desired changes (`icon = { ...icon, enabled: false };`). This ensures that the hook receives a new reference, allowing memoization to correctly re-evaluate.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/reference/rules/components-and-hooks-must-be-pure.md#_snippet_18

LANGUAGE: js
CODE:
```
style = useIconStyle(icon);         // `style` is memoized based on `icon`
icon = { ...icon, enabled: false }; // Good: ✅ make a copy instead
style = useIconStyle(icon);         // new value of `style` is calculated
```

----------------------------------------

TITLE: Initializing useReducer with Direct Function Call (Inefficient)
DESCRIPTION: This snippet demonstrates an inefficient way to initialize state with `useReducer` where `createInitialState(username)` is called directly. Although `useReducer` only uses the result for the initial render, the `createInitialState` function is executed on every subsequent render, potentially leading to performance issues if it's an expensive operation.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/reference/react/useReducer.md#_snippet_19

LANGUAGE: js
CODE:
```
function createInitialState(username) {
  // ...
}

function TodoList({ username }) {
  const [state, dispatch] = useReducer(reducer, createInitialState(username));
  // ...
```

----------------------------------------

TITLE: Passing Object Inline to Component - React/JS
DESCRIPTION: Illustrates how a parent component might pass an object literal directly as a prop to a child component. This creates a new object instance on every parent render, which can cause issues if the child component's Effect depends on this object.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/removing-effect-dependencies.md#_snippet_39

LANGUAGE: JavaScript
CODE:
```
<ChatRoom
  roomId={roomId}
  options={{
    serverUrl: serverUrl,
    roomId: roomId
  }}
/>

```

----------------------------------------

TITLE: Updating Nested Object State with useState (Buggy) - React JavaScript
DESCRIPTION: This React component demonstrates a common pitfall when updating nested state objects using `useState`. Direct mutation of `shape.position.x` and `shape.position.y` within `handleMove` leads to state mutation without triggering re-renders, causing visual inconsistencies. The `handleColorChange` correctly uses object spread for immutability.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/updating-objects-in-state.md#_snippet_44

LANGUAGE: JavaScript
CODE:
```
import { useState } from 'react';
import { useImmer } from 'use-immer';
import Background from './Background.js';
import Box from './Box.js';

const initialPosition = {
  x: 0,
  y: 0
};

export default function Canvas() {
  const [shape, setShape] = useState({
    color: 'orange',
    position: initialPosition
  });

  function handleMove(dx, dy) {
    shape.position.x += dx;
    shape.position.y += dy;
  }

  function handleColorChange(e) {
    setShape({
      ...shape,
      color: e.target.value
    });
  }

  return (
    <>
      <select
        value={shape.color}
        onChange={handleColorChange}
      >
        <option value="orange">orange</option>
        <option value="lightpink">lightpink</option>
        <option value="aliceblue">aliceblue</option>
      </select>
      <Background
        position={initialPosition}
      />
      <Box
        color={shape.color}
        position={shape.position}
        onMove={handleMove}
      >
        Drag me!
      </Box>
    </>
  );
}
```

----------------------------------------

TITLE: Incorrect Ref Usage with Custom React Component (JavaScript)
DESCRIPTION: This snippet demonstrates a common mistake where a ref is passed directly to a custom component without the component explicitly forwarding it. This will result in an error because custom components do not expose their internal DOM nodes by default when a ref prop is received.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/reference/react/useRef.md#_snippet_19

LANGUAGE: js
CODE:
```
const inputRef = useRef(null);

return <MyInput ref={inputRef} />;
```

----------------------------------------

TITLE: Implementing an Interactive Counter Client Component
DESCRIPTION: This code defines a simple counter component that uses React state (`useState`) and event handlers (`onClick`) to provide user interaction. Because it relies on hooks and browser events, it must be a Client Component, indicated by the `'use client'` directive at the top.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/reference/rsc/use-client.md#_snippet_2

LANGUAGE: jsx
CODE:
```
'use client';

import { useState } from 'react';

export default function Counter({initialValue = 0}) {
  const [countValue, setCountValue] = useState(initialValue);
  const increment = () => setCountValue(countValue + 1);
  const decrement = () => setCountValue(countValue - 1);
  return (
    <>
      <h2>Count Value: {countValue}</h2>
      <button onClick={increment}>+1</button>
      <button onClick={decrement}>-1</button>
    </>
  );
}
```

----------------------------------------

TITLE: Caching Asynchronous Fetch Operations with React `cache` in JavaScript
DESCRIPTION: This example illustrates how to cache asynchronous work, specifically a `fetch` operation, using React's `cache` API. The `getData` function wraps `fetchData` to memoize the promise returned by the fetch. The first call to `getData()` initiates the fetch and caches the promise, while the subsequent `await getData()` retrieves the cached promise's result, optimizing for concurrent work and reducing wait times.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/reference/react/cache.md#_snippet_11

LANGUAGE: JavaScript
CODE:
```
async function fetchData() {
  return await fetch(`https://...`);
}

const getData = cache(fetchData);

async function MyComponent() {
  getData();
  // ... some computational work  
  await getData();
  // ...
}
```

----------------------------------------

TITLE: Complete useReducer Example for a Counter in React
DESCRIPTION: This comprehensive example demonstrates a full `useReducer` implementation for a simple counter. It includes the reducer function logic for incrementing age, the component's `useReducer` hook call, and a button that dispatches an 'incremented_age' action to update the displayed age.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/reference/react/useReducer.md#_snippet_5

LANGUAGE: javascript
CODE:
```
import { useReducer } from 'react';

function reducer(state, action) {
  if (action.type === 'incremented_age') {
    return {
      age: state.age + 1
    };
  }
  throw Error('Unknown action.');
}

export default function Counter() {
  const [state, dispatch] = useReducer(reducer, { age: 42 });

  return (
    <>
      <button onClick={() => {
        dispatch({ type: 'incremented_age' })
      }}>
        Increment age
      </button>
      <p>Hello! You are {state.age}.</p>
    </>
  );
}
```

----------------------------------------

TITLE: Connecting to Chat with Reactive Effect (roomId, theme) - React JavaScript
DESCRIPTION: This snippet demonstrates a React `useEffect` hook that connects to a chat server. The effect is reactive to both `roomId` and `theme` props, meaning it will re-connect if either value changes. It uses `createConnection` and `showNotification` utilities. The `theme` dependency causes unnecessary re-connections when only the theme changes.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/escape-hatches.md#_snippet_10

LANGUAGE: JSON
CODE:
```
{
  "dependencies": {
    "react": "latest",
    "react-dom": "latest",
    "react-scripts": "latest",
    "toastify-js": "1.12.0"
  },
  "scripts": {
    "start": "react-scripts start",
    "build": "react-scripts build",
    "test": "react-scripts test --env=jsdom",
    "eject": "react-scripts eject"
  }
}
```

LANGUAGE: JavaScript
CODE:
```
import { useState, useEffect } from 'react';
import { createConnection, sendMessage } from './chat.js';
import { showNotification } from './notifications.js';

const serverUrl = 'https://localhost:1234';

function ChatRoom({ roomId, theme }) {
  useEffect(() => {
    const connection = createConnection(serverUrl, roomId);
    connection.on('connected', () => {
      showNotification('Connected!', theme);
    });
    connection.connect();
    return () => connection.disconnect();
  }, [roomId, theme]);

  return <h1>Welcome to the {roomId} room!</h1>
}

export default function App() {
  const [roomId, setRoomId] = useState('general');
  const [isDark, setIsDark] = useState(false);
  return (
    <>
      <label>
        Choose the chat room:{' '}
        <select
          value={roomId}
          onChange={e => setRoomId(e.target.value)}
        >
          <option value="general">general</option>
          <option value="travel">travel</option>
          <option value="music">music</option>
        </select>
      </label>
      <label>
        <input
          type="checkbox"
          checked={isDark}
          onChange={e => setIsDark(e.target.checked)}
        />
        Use dark theme
      </label>
      <hr />
      <ChatRoom
        roomId={roomId}
        theme={isDark ? 'dark' : 'light'} 
      />
    </>
  );
}
```

LANGUAGE: JavaScript
CODE:
```
export function createConnection(serverUrl, roomId) {
  // A real implementation would actually connect to the server
  let connectedCallback;
  let timeout;
  return {
    connect() {
      timeout = setTimeout(() => {
        if (connectedCallback) {
          connectedCallback();
        }
      }, 100);
    },
    on(event, callback) {
      if (connectedCallback) {
        throw Error('Cannot add the handler twice.');
      }
      if (event !== 'connected') {
        throw Error('Only "connected" event is supported.');
      }
      connectedCallback = callback;
    },
    disconnect() {
      clearTimeout(timeout);
    }
  };
}
```

LANGUAGE: JavaScript
CODE:
```
import Toastify from 'toastify-js';
import 'toastify-js/src/toastify.css';

export function showNotification(message, theme) {
  Toastify({
    text: message,
    duration: 2000,
    gravity: 'top',
    position: 'right',
    style: {
      background: theme === 'dark' ? 'black' : 'white',
      color: theme === 'dark' ? 'white' : 'black',
    },
  }).showToast();
}
```

LANGUAGE: CSS
CODE:
```
label { display: block; margin-top: 10px; }
```

----------------------------------------

TITLE: Passing Data from Parent to Child in React (Recommended)
DESCRIPTION: This snippet illustrates the recommended React pattern for data flow, where the parent component fetches the data and passes it down as a prop to the child component. This ensures predictable and easier-to-trace data flow.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/you-might-not-need-an-effect.md#_snippet_26

LANGUAGE: JavaScript
CODE:
```
function Parent() {
  const data = useSomeAPI();
  // ...
  // ✅ Good: Passing data down to the child
  return <Child data={data} />;
}

function Child({ data }) {
  // ...
}
```

----------------------------------------

TITLE: React Timer Component with useEffectEvent (Solution)
DESCRIPTION: Corrected implementation of the React Timer component using `useEffectEvent` to prevent the timer interval from being cleared when the `increment` state changes. The `onTick` event handler accesses the latest `increment` value without being a dependency of the `useEffect`.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/separating-events-from-effects.md#_snippet_29

LANGUAGE: json
CODE:
```
{
  "dependencies": {
    "react": "experimental",
    "react-dom": "experimental",
    "react-scripts": "latest"
  },
  "scripts": {
    "start": "react-scripts start",
    "build": "react-scripts build",
    "test": "react-scripts test --env=jsdom",
    "eject": "react-scripts eject"
  }
}
```

LANGUAGE: javascript
CODE:
```
import { useState, useEffect } from 'react';
import { experimental_useEffectEvent as useEffectEvent } from 'react';

export default function Timer() {
  const [count, setCount] = useState(0);
  const [increment, setIncrement] = useState(1);

  const onTick = useEffectEvent(() => {
    setCount(c => c + increment);
  });

  useEffect(() => {
    const id = setInterval(() => {
      onTick();
    }, 1000);
    return () => {
      clearInterval(id);
    };
  }, []);

  return (
    <>
      <h1>
        Counter: {count}
        <button onClick={() => setCount(0)}>Reset</button>
      </h1>
      <hr />
      <p>
        Every second, increment by:
        <button disabled={increment === 0} onClick={() => {
          setIncrement(i => i - 1);
        }}>–</button>
        <b>{increment}</b>
        <button onClick={() => {
          setIncrement(i => i + 1);
        }}>+</button>
      </p>
    </>
  );
}
```

LANGUAGE: css
CODE:
```
button { margin: 10px; }
```

----------------------------------------

TITLE: Importing and Providing Multiple Contexts in a React App
DESCRIPTION: This snippet demonstrates how the main `App` component imports both `ThemeContext` and `AuthContext` from a shared file. It then uses their respective `Provider` components to wrap the `Page` component, making dynamic theme and authentication data available to all nested components.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/reference/react/createContext.md#_snippet_10

LANGUAGE: javascript
CODE:
```
// App.js
import { ThemeContext, AuthContext } from './Contexts.js';

function App() {
  // ...
  return (
    <ThemeContext.Provider value={theme}>
      <AuthContext.Provider value={currentUser}>
        <Page />
      </AuthContext.Provider>
    </ThemeContext.Provider>
  );
}
```

----------------------------------------

TITLE: Initial Challenge: Conditional Rendering with Logical AND Operator in React
DESCRIPTION: Presents the starting code for a challenge, using the logical AND operator (`&&`) to conditionally display a checkmark. The goal is to modify this code to use the ternary operator for showing an '❌' for incomplete items.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/conditional-rendering.md#_snippet_15

LANGUAGE: js
CODE:
```
function Item({ name, isPacked }) {
  return (
    <li className="item">
      {name} {isPacked && '✅'}
    </li>
  );
}

export default function PackingList() {
  return (
    <section>
      <h1>Sally Ride's Packing List</h1>
      <ul>
        <Item 
          isPacked={true} 
          name="Space suit" 
        />
        <Item 
          isPacked={true} 
          name="Helmet with a golden leaf" 
        />
        <Item 
          isPacked={false} 
          name="Photo of Tam" 
        />
      </ul>
    </section>
  );
}
```

----------------------------------------

TITLE: Initializing Next.js Project with App Router
DESCRIPTION: Use the npx command to quickly create a new Next.js project configured with the App Router. This is the recommended way to start a full-stack React application leveraging the latest React features.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/creating-a-react-app.md#_snippet_0

LANGUAGE: bash
CODE:
```
npx create-next-app@latest
```

----------------------------------------

TITLE: Correct Hook Usage in React Function Components and Custom Hooks - JavaScript
DESCRIPTION: This snippet demonstrates the correct way to call React Hooks: at the top level of a function component or a custom Hook. useState is used to manage component state (count) and custom Hook state (width), ensuring Hooks are called within the React rendering lifecycle.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/reference/rules/rules-of-hooks.md#_snippet_0

LANGUAGE: JavaScript
CODE:
```
function Counter() {
  // ✅ Good: top-level in a function component
  const [count, setCount] = useState(0);
  // ...
}

function useWindowWidth() {
  // ✅ Good: top-level in a custom Hook
  const [width, setWidth] = useState(window.innerWidth);
  // ...
}
```

----------------------------------------

TITLE: Refactored React Component Using useData Hook
DESCRIPTION: Demonstrates how to simplify the ShippingForm component by replacing the two original useEffect calls with calls to the custom useData Hook. This improves readability and makes the data flow more explicit.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/reusing-logic-with-custom-hooks.md#_snippet_45

LANGUAGE: javascript
CODE:
```
function ShippingForm({ country }) {
  const cities = useData(`/api/cities?country=${country}`);
  const [city, setCity] = useState(null);
  const areas = useData(city ? `/api/areas?city=${city}` : null);
  // ...
```

----------------------------------------

TITLE: Managing Chat Connection in React ChatRoom (Buggy)
DESCRIPTION: This React component establishes and disconnects a chat connection using `useEffect`. It takes `roomId` and `createConnection` props. The bug lies in the `useEffect` dependency array, which omits `createConnection`, preventing re-connection when the encryption method changes.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/lifecycle-of-reactive-effects.md#_snippet_42

LANGUAGE: JavaScript
CODE:
```
import { useState, useEffect } from 'react';

export default function ChatRoom({ roomId, createConnection }) {
  useEffect(() => {
    const connection = createConnection(roomId);
    connection.connect();
    return () => connection.disconnect();
    // eslint-disable-next-line react-hooks/exhaustive-deps
  }, [roomId]);

  return <h1>Welcome to the {roomId} room!</h1>;
}
```

----------------------------------------

TITLE: Passing Function to HTML Form Action Prop in React
DESCRIPTION: This example shows how to pass a JavaScript function directly to the `action` prop of a `<form>` element in React 19. This enables automatic form submission handling via React Actions, simplifying form management.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/blog/2024/12/05/react-19.md#_snippet_4

LANGUAGE: HTML
CODE:
```
<form action={actionFunction}>
```

----------------------------------------

TITLE: Mutating Object State Directly in React (Incorrect)
DESCRIPTION: This snippet demonstrates an incorrect way to update an object in React state by directly mutating its property (`position.x = 5`). This approach does not trigger a re-render in React.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/updating-objects-in-state.md#_snippet_3

LANGUAGE: js
CODE:
```
position.x = 5;
```

----------------------------------------

TITLE: Setting up React Application Root (JS)
DESCRIPTION: This JavaScript file initializes the React application by creating a root using `createRoot` and rendering the main `App` component within React's `StrictMode` for development checks.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/reference/react/useTransition.md#_snippet_42

LANGUAGE: js
CODE:
```
import React, { StrictMode } from 'react';
import { createRoot } from 'react-dom/client';
import './styles.css';
import App from './App';

const root = createRoot(document.getElementById('root'));
root.render(
  <StrictMode>
    <App />
  </StrictMode>
);
```

----------------------------------------

TITLE: React Event Handler with Multiple State Updates (JavaScript)
DESCRIPTION: This JSX snippet shows an `onClick` handler for a button that attempts to increment a `number` state variable three times using `setNumber(number + 1)`. It illustrates the concept that `number` retains its value from the current render's snapshot, meaning all three `setNumber` calls use the same initial `number` value, leading to only one effective state update for the *next* render.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/state-as-a-snapshot.md#_snippet_4

LANGUAGE: JavaScript
CODE:
```
<button onClick={() => {
  setNumber(number + 1);
  setNumber(number + 1);
  setNumber(number + 1);
}}>+3</button>
```

----------------------------------------

TITLE: Understanding Stale State After Dispatch in React
DESCRIPTION: This snippet illustrates that calling `dispatch` in React does not immediately update the `state` variable in the currently executing code. State updates are asynchronous, and `state` behaves like a snapshot, meaning `console.log(state.age)` will still reflect the old value until the next render cycle. This highlights the snapshot nature of React state.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/reference/react/useReducer.md#_snippet_24

LANGUAGE: javascript
CODE:
```
function handleClick() {
  console.log(state.age);  // 42

  dispatch({ type: 'incremented_age' }); // Request a re-render with 43
  console.log(state.age);  // Still 42!

  setTimeout(() => {
    console.log(state.age); // Also 42!
  }, 5000);
}
```

----------------------------------------

TITLE: Displaying Form Submission Errors with useActionState in React
DESCRIPTION: This snippet demonstrates how to use the `useActionState` Hook in React to display form submission errors without relying on client-side JavaScript for initial rendering. It takes a server function and an initial state, returning a state variable for messages and an action for the form's `action` prop. The server function updates the state, allowing error messages to be displayed progressively.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/reference/react-dom/components/form.md#_snippet_7

LANGUAGE: JavaScript
CODE:
```
import { useActionState } from "react";
import { signUpNewUser } from "./api";

export default function Page() {
  async function signup(prevState, formData) {
    "use server";
    const email = formData.get("email");
    try {
      await signUpNewUser(email);
      alert(`Added "${email}"`);
    } catch (err) {
      return err.toString();
    }
  }
  const [message, signupAction] = useActionState(signup, null);
  return (
    <>
      <h1>Signup for my newsletter</h1>
      <p>Signup with the same email twice to see an error</p>
      <form action={signupAction} id="signup-form">
        <label htmlFor="email">Email: </label>
        <input name="email" id="email" placeholder="react@example.com" />
        <button>Sign up</button>
        {!!message && <p>{message}</p>}
      </form>
    </>
  );
}
```

LANGUAGE: JavaScript
CODE:
```
let emails = [];

export async function signUpNewUser(newEmail) {
  if (emails.includes(newEmail)) {
    throw new Error("This email address has already been added");
  }
  emails.push(newEmail);
}
```

----------------------------------------

TITLE: Immutable Update of Nested Object State (JavaScript - Step-by-step)
DESCRIPTION: This JavaScript snippet demonstrates the step-by-step process of immutably updating a nested object property in React. It involves creating new copies of both the nested `artwork` object and the parent `person` object using the spread syntax (`...`) before calling `setPerson` to update the state.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/updating-objects-in-state.md#_snippet_21

LANGUAGE: js
CODE:
```
const nextArtwork = { ...person.artwork, city: 'New Delhi' };
const nextPerson = { ...person, artwork: nextArtwork };
setPerson(nextPerson);
```

----------------------------------------

TITLE: Avoid: Using Custom 'Lifecycle' Hooks (useMount)
DESCRIPTION: This example demonstrates an anti-pattern: using a custom 'lifecycle' hook like `useMount` to run code only on component mount. This approach hides dependencies from the React linter and can lead to stale closures or incorrect behavior when dependencies change.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/reusing-logic-with-custom-hooks.md#_snippet_46

LANGUAGE: JavaScript
CODE:
```
function ChatRoom({ roomId }) {
  const [serverUrl, setServerUrl] = useState('https://localhost:1234');

  // 🔴 Avoid: using custom "lifecycle" Hooks
  useMount(() => {
    const connection = createConnection({ roomId, serverUrl });
    connection.connect();

    post('/analytics/event', { eventName: 'visit_chat' });
  });
  // ...
}

// 🔴 Avoid: creating custom "lifecycle" Hooks
function useMount(fn) {
  useEffect(() => {
    fn();
  }, []); // 🔴 React Hook useEffect has a missing dependency: 'fn'
}
```

----------------------------------------

TITLE: Fixed StoryTray Component (Separate Rendering) - JavaScript
DESCRIPTION: This revised `StoryTray` component fixes the mutation issue by rendering the 'Create Story' item as a separate `<li>` element outside the `map` function. This approach ensures that the original `stories` prop array remains untouched, making the component pure and predictable, resolving the duplicate rendering problem.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/keeping-components-pure.md#_snippet_13

LANGUAGE: JavaScript
CODE:
```
export default function StoryTray({ stories }) {
  return (
    <ul>
      {stories.map(story => (
        <li key={story.id}>
          {story.label}
        </li>
      ))}
      <li>Create Story</li>
    </ul>
  );
}
```

----------------------------------------

TITLE: Preserving State Fields in React Reducer Updates
DESCRIPTION: This snippet highlights a crucial point when updating state in a React reducer: always ensure that all existing state fields are copied into the new state object. Forgetting to spread the previous state (`...state`) will result in only the explicitly updated fields being present in the new state, causing other parts of the state to become `undefined` and potentially breaking the application.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/reference/react/useReducer.md#_snippet_28

LANGUAGE: javascript
CODE:
```
function reducer(state, action) {
  switch (action.type) {
    case 'incremented_age': {
      return {
        ...state, // Don't forget this!
        age: state.age + 1
      };
    }
    // ...
}
```

----------------------------------------

TITLE: Correct State Update in useEffect with Updater Function - JavaScript
DESCRIPTION: This snippet provides the correct way to update state based on its previous value within a `useEffect` hook using a state updater function (`c => c + 1`). By using the updater function, `count` is no longer a dependency of the effect, preventing the `setInterval` from being cleared and re-established on every `count` change, ensuring a continuous counter.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/reference/react/useEffect.md#_snippet_39

LANGUAGE: JavaScript
CODE:
```
import { useState, useEffect } from 'react';

export default function Counter() {
  const [count, setCount] = useState(0);

  useEffect(() => {
    const intervalId = setInterval(() => {
      setCount(c => c + 1); // ✅ Pass a state updater
    }, 1000);
    return () => clearInterval(intervalId);
  }, []); // ✅ Now count is not a dependency

  return <h1>{count}</h1>;
}
```

----------------------------------------

TITLE: Attempting to Limit `useEffect` Re-runs with Empty Dependencies in React
DESCRIPTION: This snippet illustrates an attempt to optimize `useEffect` by providing an empty dependency array `[]`. While intended to run the effect only once, it leads to a `React Hook useEffect has a missing dependency` error because the `isPlaying` prop, used within the effect, is not declared as a dependency. This demonstrates a common pitfall when trying to control effect re-execution.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/synchronizing-with-effects.md#_snippet_9

LANGUAGE: js
CODE:
```
import { useState, useRef, useEffect } from 'react';

function VideoPlayer({ src, isPlaying }) {
  const ref = useRef(null);

  useEffect(() => {
    if (isPlaying) {
      console.log('Calling video.play()');
      ref.current.play();
    } else {
      console.log('Calling video.pause()');
      ref.current.pause();
    }
  }, []); // This causes an error

  return <video ref={ref} src={src} loop playsInline />;
}

export default function App() {
  const [isPlaying, setIsPlaying] = useState(false);
  const [text, setText] = useState('');
  return (
    <>
      <input value={text} onChange={e => setText(e.target.value)} />
      <button onClick={() => setIsPlaying(!isPlaying)}>
        {isPlaying ? 'Pause' : 'Play'}
      </button>
      <VideoPlayer
        isPlaying={isPlaying}
        src="https://interactive-examples.mdn.mozilla.net/media/cc0-videos/flower.mp4"
      />
    </>
  );
}
```

LANGUAGE: css
CODE:
```
input, button { display: block; margin-bottom: 20px; }
video { width: 250px; }
```

----------------------------------------

TITLE: Playing and Pausing Video with useRef in React
DESCRIPTION: This example demonstrates how to control video playback (play/pause) using `useRef` to access the `<video>` DOM node. It also integrates `useState` to manage the playback state and update the button text accordingly, and uses `onPlay`/`onPause` events to keep state synchronized.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/reference/react/useRef.md#_snippet_14

LANGUAGE: js
CODE:
```
import { useState, useRef } from 'react';

export default function VideoPlayer() {
  const [isPlaying, setIsPlaying] = useState(false);
  const ref = useRef(null);

  function handleClick() {
    const nextIsPlaying = !isPlaying;
    setIsPlaying(nextIsPlaying);

    if (nextIsPlaying) {
      ref.current.play();
    } else {
      ref.current.pause();
    }
  }

  return (
    <>
      <button onClick={handleClick}>
        {isPlaying ? 'Pause' : 'Play'}
      </button>
      <video
        width="250"
        ref={ref}
        onPlay={() => setIsPlaying(true)}
        onPause={() => setIsPlaying(false)}
      >
        <source
          src="https://interactive-examples.mdn.mozilla.net/media/cc0-videos/flower.mp4"
          type="video/mp4"
        />
      </video>
    </>
  );
}
```

LANGUAGE: css
CODE:
```
button { display: block; margin-bottom: 20px; }
```

----------------------------------------

TITLE: Refactoring React Component with Object Lookup for Data
DESCRIPTION: This advanced refactoring moves the drink-specific data into a `drinks` object, eliminating all conditional logic within the `Drink` component. The component now retrieves properties directly using `drinks[name]`, making it highly scalable and maintainable, especially when adding new drink types.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/conditional-rendering.md#_snippet_21

LANGUAGE: javascript
CODE:
```
const drinks = {
  tea: {
    part: 'leaf',
    caffeine: '15–70 mg/cup',
    age: '4,000+ years'
  },
  coffee: {
    part: 'bean',
    caffeine: '80–185 mg/cup',
    age: '1,000+ years'
  }
};

function Drink({ name }) {
  const info = drinks[name];
  return (
    <section>
      <h1>{name}</h1>
      <dl>
        <dt>Part of plant</dt>
        <dd>{info.part}</dd>
        <dt>Caffeine content</dt>
        <dd>{info.caffeine}</dd>
        <dt>Age</dt>
        <dd>{info.age}</dd>
      </dl>
    </section>
  );
}

export default function DrinkList() {
  return (
    <div>
      <Drink name="tea" />
      <Drink name="coffee" />
    </div>
  );
}
```

----------------------------------------

TITLE: Removing Items from React State Array (Filter Method)
DESCRIPTION: This snippet highlights the core logic for removing an item from an array in React state using the `filter()` method. It creates a new array containing all elements except the one matching the specified condition, ensuring immutability and proper state updates.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/updating-arrays-in-state.md#_snippet_5

LANGUAGE: javascript
CODE:
```
setArtists(
  artists.filter(a => a.id !== artist.id)
);
```

----------------------------------------

TITLE: Optimizing React Context with useCallback and useMemo
DESCRIPTION: This optimized `MyApp` component uses `useCallback` to memoize the `login` function and `useMemo` to memoize the context value object. This prevents unnecessary re-renders of components consuming `AuthContext` by ensuring the `login` function and the `contextValue` object references remain stable across `MyApp` re-renders, unless `currentUser` or `login` (its dependencies) actually change.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/reference/react/useContext.md#_snippet_28

LANGUAGE: JavaScript
CODE:
```
import { useCallback, useMemo } from 'react';

function MyApp() {
  const [currentUser, setCurrentUser] = useState(null);

  const login = useCallback((response) => {
    storeCredentials(response.credentials);
    setCurrentUser(response.user);
  }, []);

  const contextValue = useMemo(() => ({
    currentUser,
    login
  }), [currentUser, login]);

  return (
    <AuthContext.Provider value={contextValue}>
      <Page />
    </AuthContext.Provider>
  );
}
```

----------------------------------------

TITLE: React Component for Adding Items (Spread Syntax)
DESCRIPTION: This complete React component demonstrates the recommended pattern for adding new items to an array in state. It uses the `useState` hook and the array spread syntax to create a new array with the added item, ensuring immutability and proper UI updates.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/updating-arrays-in-state.md#_snippet_2

LANGUAGE: javascript
CODE:
```
import { useState } from 'react';

let nextId = 0;

export default function List() {
  const [name, setName] = useState('');
  const [artists, setArtists] = useState([]);

  return (
    <>
      <h1>Inspiring sculptors:</h1>
      <input
        value={name}
        onChange={e => setName(e.target.value)}
      />
      <button onClick={() => {
        setArtists([
          ...artists,
          { id: nextId++, name: name }
        ]);
      }}>Add</button>
      <ul>
        {artists.map(artist => (
          <li key={artist.id}>{artist.name}</li>
        ))}
      </ul>
    </>
  );
}
```

LANGUAGE: css
CODE:
```
button { margin-left: 5px; }
```

----------------------------------------

TITLE: Conditionally Returning Null to Hide React Component - JavaScript
DESCRIPTION: This snippet demonstrates how a React component can conditionally return `null` to prevent rendering any JSX. If the `isPacked` prop is `true`, the component returns `null`, effectively hiding the item from the UI. Otherwise, it renders the list item. This is useful for completely omitting a component from the render tree based on a condition.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/conditional-rendering.md#_snippet_3

LANGUAGE: js
CODE:
```
if (isPacked) {
  return null;
}
return <li className="item">{name}</li>;
```

----------------------------------------

TITLE: Solution: Conditional Rendering with Ternary Operator in React
DESCRIPTION: Provides the solution to the challenge, demonstrating the use of the conditional (ternary) operator (`? :`) to render either a checkmark ('✅') if `isPacked` is true, or a cross mark ('❌') if it's false, for an item. This offers a concise way to handle two-way conditional rendering.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/conditional-rendering.md#_snippet_16

LANGUAGE: js
CODE:
```
function Item({ name, isPacked }) {
  return (
    <li className="item">
      {name} {isPacked ? '✅' : '❌'}
    </li>
  );
}

export default function PackingList() {
  return (
    <section>
      <h1>Sally Ride's Packing List</h1>
      <ul>
        <Item 
          isPacked={true} 
          name="Space suit" 
        />
        <Item 
          isPacked={true} 
          name="Helmet with a golden leaf" 
        />
        <Item 
          isPacked={false} 
          name="Photo of Tam" 
        />
      </ul>
    </section>
  );
}
```

----------------------------------------

TITLE: React Component for Filtering and Displaying People
DESCRIPTION: This complete React component, `List`, imports `people` data and `getImageUrl` utility, then filters the `people` array to get `chemists` and maps them into `<li>` elements. Finally, it renders these list items within an `<ul>` element, demonstrating a full data flow from import to rendering.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/rendering-lists.md#_snippet_10

LANGUAGE: JavaScript
CODE:
```
import { people } from './data.js';
import { getImageUrl } from './utils.js';

export default function List() {
  const chemists = people.filter(person =>
    person.profession === 'chemist'
  );
  const listItems = chemists.map(person =>
    <li>
      <img
        src={getImageUrl(person)}
        alt={person.name}
      />
      <p>
        <b>{person.name}:</b>
        {' ' + person.profession + ' '}
        known for {person.accomplishment}
      </p>
    </li>
  );
  return <ul>{listItems}</ul>;
}
```

----------------------------------------

TITLE: State Reset Due to Changing Parent Elements in React
DESCRIPTION: This example illustrates that state is reset even if the same component type (<Counter>) is rendered, but its direct parent element changes. When the checkbox is toggled, the parent element switches between a <div> and a <section>. React perceives this as a change in the UI tree structure, leading to the unmounting of the old parent and its children (including <Counter>), and the mounting of the new parent and a fresh <Counter> instance with its state reinitialized.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/preserving-and-resetting-state.md#_snippet_9

LANGUAGE: js
CODE:
```
import { useState } from 'react';

export default function App() {
  const [isFancy, setIsFancy] = useState(false);
  return (
    <div>
      {isFancy ? (
        <div>
          <Counter isFancy={true} /> 
        </div>
      ) : (
        <section>
          <Counter isFancy={false} />
        </section>
      )}
      <label>
        <input
          type="checkbox"
          checked={isFancy}
          onChange={e => {
            setIsFancy(e.target.checked)
          }}
        />
        Use fancy styling
      </label>
    </div>
  );
}

function Counter({ isFancy }) {
  const [score, setScore] = useState(0);
  const [hover, setHover] = useState(false);

  let className = 'counter';
  if (hover) {
    className += ' hover';
  }
  if (isFancy) {
    className += ' fancy';
  }

  return (
    <div
      className={className}
      onPointerEnter={() => setHover(true)}
      onPointerLeave={() => setHover(false)}
    >
      <h1>{score}</h1>
      <button onClick={() => setScore(score + 1)}>
        Add one
      </button>
    </div>
  );
}
```

LANGUAGE: css
CODE:
```
label {
  display: block;
  clear: both;
}

.counter {
  width: 100px;
  text-align: center;
  border: 1px solid gray;
  border-radius: 4px;
  padding: 20px;
  margin: 0 20px 20px 0;
  float: left;
}

.fancy {
  border: 5px solid gold;
  color: #ff6767;
}

.hover {
  background: #ffffd8;
}
```

----------------------------------------

TITLE: Correctly Rendering React Components with root.render
DESCRIPTION: This snippet illustrates the correct way to render a React component using `root.render`. The error 'Functions are not valid as a React child' arises when a function (e.g., `App`) is passed directly instead of a React element (e.g., `<App />`). React expects an instantiated component or element, not a function reference, for rendering.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/reference/react-dom/client/createRoot.md#_snippet_26

LANGUAGE: js
CODE:
```
// 🚩 Wrong: App is a function, not a Component.
root.render(App);

// ✅ Correct: <App /> is a component.
root.render(<App />);
```

----------------------------------------

TITLE: Initializing State with useState Hook in React
DESCRIPTION: This snippet demonstrates how to initialize two state variables, `filterText` and `inStockOnly`, within the `FilterableProductTable` functional component using React's `useState` hook. `filterText` is initialized as an empty string, and `inStockOnly` as `false`, setting up the initial filtering criteria for the product table. This ensures the application starts with a clear search input and all products visible.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/thinking-in-react.md#_snippet_3

LANGUAGE: JavaScript
CODE:
```
function FilterableProductTable({ products }) {
  const [filterText, setFilterText] = useState('');
  const [inStockOnly, setInStockOnly] = useState(false);  
```

----------------------------------------

TITLE: Preventing Default Form Submission in React
DESCRIPTION: This example shows how to prevent the default page reload behavior of a form submission in React by calling `e.preventDefault()` on the event object within the `onSubmit` handler. This allows custom logic to execute without a full page refresh.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/responding-to-events.md#_snippet_15

LANGUAGE: JavaScript
CODE:
```
export default function Signup() {
  return (
    <form onSubmit={e => {
      e.preventDefault();
      alert('Submitting!');
    }}>
      <input />
      <button>Send</button>
    </form>
  );
}
```

LANGUAGE: CSS
CODE:
```
button { margin-left: 5px; }
```

----------------------------------------

TITLE: React TodoList Component Optimized with `useMemo` (Solution)
DESCRIPTION: This optimized React component uses `useMemo` to cache the `visibleTodos` list. By replacing `useEffect` and a dedicated `visibleTodos` state with `useMemo`, the `getVisibleTodos` function only re-executes when `todos` or `showActive` dependencies change, significantly improving performance by avoiding re-calculations on `text` input changes.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/you-might-not-need-an-effect.md#_snippet_37

LANGUAGE: javascript
CODE:
```
import { useState, useMemo } from 'react';
import { initialTodos, createTodo, getVisibleTodos } from './todos.js';

export default function TodoList() {
  const [todos, setTodos] = useState(initialTodos);
  const [showActive, setShowActive] = useState(false);
  const [text, setText] = useState('');
  const visibleTodos = useMemo(
    () => getVisibleTodos(todos, showActive),
    [todos, showActive]
  );

  function handleAddClick() {
    setText('');
    setTodos([...todos, createTodo(text)]);
  }

  return (
    <>
      <label>
        <input
          type="checkbox"
          checked={showActive}
          onChange={e => setShowActive(e.target.checked)}
        />
        Show only active todos
      </label>
      <input value={text} onChange={e => setText(e.target.value)} />
      <button onClick={handleAddClick}>
        Add
      </button>
      <ul>
        {visibleTodos.map(todo => (
          <li key={todo.id}>
            {todo.completed ? <s>{todo.text}</s> : todo.text}
          </li>
        ))}
      </ul>
    </>
  );
}
```

----------------------------------------

TITLE: Importing and Exporting Default Components Across Files - JavaScript
DESCRIPTION: This example illustrates how to refactor a React application by moving components into separate files. `Gallery.js` defines and default-exports the `Gallery` component, which internally uses `Profile`. `App.js` then default-imports `Gallery` and renders it, demonstrating modular component architecture.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/importing-and-exporting-components.md#_snippet_1

LANGUAGE: JavaScript
CODE:
```
import Gallery from './Gallery.js';

export default function App() {
  return (
    <Gallery />
  );
}
```

LANGUAGE: JavaScript
CODE:
```
function Profile() {
  return (
    <img
      src="https://i.imgur.com/QIrZWGIs.jpg"
      alt="Alan L. Hart"
    />
  );
}

export default function Gallery() {
  return (
    <section>
      <h1>Amazing scientists</h1>
      <Profile />
      <Profile />
      <Profile />
    </section>
  );
}
```

LANGUAGE: CSS
CODE:
```
img { margin: 0 10px 10px 0; height: 90px; }
```

----------------------------------------

TITLE: Refactored Menu Component with Derived State - React
DESCRIPTION: This refactored React component demonstrates how to avoid state duplication by storing only the `selectedId` instead of the full `selectedItem` object. The `selectedItem` is now derived during render by finding the item in the `items` array using the `selectedId`. This ensures consistency: when an item's title is updated via `handleItemChange`, the displayed 'You picked' message automatically reflects the change because `selectedItem` is re-calculated from the updated `items` array.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/choosing-the-state-structure.md#_snippet_15

LANGUAGE: js
CODE:
```
import { useState } from 'react';

const initialItems = [
  { title: 'pretzels', id: 0 },
  { title: 'crispy seaweed', id: 1 },
  { title: 'granola bar', id: 2 },
];

export default function Menu() {
  const [items, setItems] = useState(initialItems);
  const [selectedId, setSelectedId] = useState(0);

  const selectedItem = items.find(item =>
    item.id === selectedId
  );

  function handleItemChange(id, e) {
    setItems(items.map(item => {
      if (item.id === id) {
        return {
          ...item,
          title: e.target.value,
        };
      } else {
        return item;
      }
    }));
  }

  return (
    <>
      <h2>What's your travel snack?</h2>
      <ul>
        {items.map((item, index) => (
          <li key={item.id}>
            <input
              value={item.title}
              onChange={e => {
                handleItemChange(item.id, e)
              }}
            />
            {' '}
            <button onClick={() => {
              setSelectedId(item.id);
            }}>Choose</button>
          </li>
        ))}
      </ul>
      <p>You picked {selectedItem.title}.</p>
    </>
  );
}
```

LANGUAGE: css
CODE:
```
button { margin-top: 10px; }
```

----------------------------------------

TITLE: Incorrect: Nesting Component Definitions in React (JavaScript)
DESCRIPTION: This snippet demonstrates an anti-pattern in React development: defining a component (`Profile`) inside another component (`Gallery`). This practice is explicitly discouraged as it leads to performance issues and bugs related to state preservation and rendering.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/your-first-component.md#_snippet_8

LANGUAGE: javascript
CODE:
```
export default function Gallery() {
  // 🔴 Never define a component inside another component!
  function Profile() {
    // ...
  }
  // ...
}
```

----------------------------------------

TITLE: Wrapping React Components for Performance Profiling
DESCRIPTION: This example demonstrates how to wrap any React component tree with <Profiler> to enable performance measurement. The `id` prop uniquely identifies the profiled section, and the `onRender` callback is invoked with performance data upon component updates.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/reference/react/Profiler.md#_snippet_1

LANGUAGE: jsx
CODE:
```
<Profiler id="App" onRender={onRender}>
  <App />
</Profiler>
```

----------------------------------------

TITLE: Calculating Full Name from State Variables
DESCRIPTION: This JavaScript snippet shows how to derive a `fullName` string by concatenating `firstName` and `lastName` variables. This calculation occurs during the component's render phase, eliminating the need to store `fullName` as a separate state variable and ensuring it always reflects the current first and last names.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/choosing-the-state-structure.md#_snippet_9

LANGUAGE: JavaScript
CODE:
```
const fullName = firstName + ' ' + lastName;
```

----------------------------------------

TITLE: Optimizing Re-renders by Isolating Input State in React
DESCRIPTION: This optimized snippet demonstrates how to improve performance by isolating the controlled input's state into its own component, `SignupForm`. By moving the `useState` hook and the input element into `SignupForm`, only this smaller component re-renders on every keystroke, preventing unnecessary re-renders of the larger `PageContent` component.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/reference/react-dom/components/input.md#_snippet_13

LANGUAGE: JavaScript
CODE:
```
function App() {
  return (
    <>
      <SignupForm />
      <PageContent />
    </>
  );
}

function SignupForm() {
  const [firstName, setFirstName] = useState('');
  return (
    <form>
      <input value={firstName} onChange={e => setFirstName(e.target.value)} />
    </form>
  );
}
```

----------------------------------------

TITLE: Using JavaScript Variables for Dynamic Attributes in React JSX
DESCRIPTION: This example illustrates how to use JavaScript variables to dynamically set attribute values in JSX. The `src` and `alt` attributes are assigned values from `avatar` and `description` variables, respectively, by enclosing the variable names in curly braces. This allows for dynamic content rendering based on JavaScript logic.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/javascript-in-jsx-with-curly-braces.md#_snippet_1

LANGUAGE: JavaScript
CODE:
```
export default function Avatar() {
  const avatar = 'https://i.imgur.com/7vQD0fPs.jpg';
  const description = 'Gregorio Y. Zara';
  return (
    <img
      className="avatar"
      src={avatar}
      alt={description}
    />
  );
}
```

LANGUAGE: CSS
CODE:
```
.avatar { border-radius: 50%; height: 90px; }
```

----------------------------------------

TITLE: Rendering List Items with React Fragment and Key in JavaScript
DESCRIPTION: This snippet demonstrates how to render a list of React components using `Fragment` when a wrapper element is not desired, but a `key` prop is required for each item. It maps over a `posts` array, wrapping each `PostTitle` and `PostBody` with a `Fragment` and assigning a unique `key` based on `post.id`.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/reference/react/Fragment.md#_snippet_6

LANGUAGE: JavaScript
CODE:
```
function Blog() {
  return posts.map(post =>
    <Fragment key={post.id}>
      <PostTitle title={post.title} />
      <PostBody body={post.body} />
    </Fragment>
  );
}
```

----------------------------------------

TITLE: Broken StoryTray Component with Prop Mutation - JavaScript
DESCRIPTION: This `StoryTray` component demonstrates a common React anti-pattern where the `stories` prop array is directly mutated by pushing a new item. This impure function causes the 'Create Story' item to appear multiple times, especially in Strict Mode, as the component re-renders and mutates the original array repeatedly. It expects an array of `stories` and renders them in an unordered list.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/keeping-components-pure.md#_snippet_9

LANGUAGE: JavaScript
CODE:
```
export default function StoryTray({ stories }) {
  stories.push({
    id: 'create',
    label: 'Create Story'
  });

  return (
    <ul>
      {stories.map(story => (
        <li key={story.id}>
          {story.label}
        </li>
      ))}
    </ul>
  );
}
```

----------------------------------------

TITLE: Awaiting Server-Initiated Promise in a React Client Component
DESCRIPTION: This Client Component uses the `use` hook to await a promise (`commentsPromise`) that was initiated on the server. This allows the client to suspend rendering until the data is available, without blocking the initial server-rendered content, facilitating progressive loading.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/reference/rsc/server-components.md#_snippet_11

LANGUAGE: JavaScript
CODE:
```
// Client Component
"use client";
import {use} from 'react';

function Comments({commentsPromise}) {
  // NOTE: this will resume the promise from the server.
  // It will suspend until the data is available.
  const comments = use(commentsPromise);
  return comments.map(commment => <p>{comment}</p>);
}
```

----------------------------------------

TITLE: Importing and Declaring Multiple State Variables in React
DESCRIPTION: This example illustrates how to import `useState` from 'react' and declare multiple state variables within a functional component. It shows different ways to initialize state, including a direct value, a string, and an initializer function for lazy initialization.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/reference/react/useState.md#_snippet_1

LANGUAGE: JavaScript
CODE:
```
import { useState } from 'react';

function MyComponent() {
  const [age, setAge] = useState(28);
  const [name, setName] = useState('Taylor');
  const [todos, setTodos] = useState(() => createTodos());
  // ...
```

----------------------------------------

TITLE: Passing State and Event Handlers to Child Panels in React
DESCRIPTION: This React JSX snippet demonstrates how the `Accordion` parent component passes `isActive` state and an `onShow` event handler down to its `Panel` child components. The `isActive` prop determines if a panel is currently active, while `onShow` allows the child `Panel` to request a state change in the parent, setting the `activeIndex` to its corresponding index.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/sharing-state-between-components.md#_snippet_6

LANGUAGE: JavaScript
CODE:
```
<>
  <Panel
    isActive={activeIndex === 0}
    onShow={() => setActiveIndex(0)}
  >
    ...
  </Panel>
  <Panel
    isActive={activeIndex === 1}
    onShow={() => setActiveIndex(1)}
  >
    ...
  </Panel>
</>
```

----------------------------------------

TITLE: Causing Infinite Loop with useEffect in React
DESCRIPTION: This snippet demonstrates a common pitfall where `useEffect` causes an infinite re-render loop. Since Effects run after every render and setting state triggers a re-render, updating state directly within an un-depended `useEffect` will continuously re-trigger the effect. This example shows `setCount(count + 1)` inside `useEffect` without a dependency array, leading to an infinite loop.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/synchronizing-with-effects.md#_snippet_7

LANGUAGE: JavaScript
CODE:
```
const [count, setCount] = useState(0);
useEffect(() => {
  setCount(count + 1);
});
```

----------------------------------------

TITLE: React Data Fetching with useEffect using Async/Await and Race Condition Handling
DESCRIPTION: This React component demonstrates data fetching using `useEffect` with the `async`/`await` syntax for cleaner asynchronous code. It includes an internal `startFetching` async function and maintains the crucial `ignore` flag in the cleanup function to prevent race conditions, similar to the Promise-based approach. The `api.js` file provides the simulated data fetching logic.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/reference/react/useEffect.md#_snippet_22

LANGUAGE: JavaScript
CODE:
```
import { useState, useEffect } from 'react';
import { fetchBio } from './api.js';

export default function Page() {
  const [person, setPerson] = useState('Alice');
  const [bio, setBio] = useState(null);
  useEffect(() => {
    async function startFetching() {
      setBio(null);
      const result = await fetchBio(person);
      if (!ignore) {
        setBio(result);
      }
    }

    let ignore = false;
    startFetching();
    return () => {
      ignore = true;
    }
  }, [person]);

  return (
    <>
      <select value={person} onChange={e => {
        setPerson(e.target.value);
      }}>
        <option value="Alice">Alice</option>
        <option value="Bob">Bob</option>
        <option value="Taylor">Taylor</option>
      </select>
      <hr />
      <p><i>{bio ?? 'Loading...'}</i></p>
    </>
  );
}
```

LANGUAGE: JavaScript
CODE:
```
export async function fetchBio(person) {
  const delay = person === 'Bob' ? 2000 : 200;
  return new Promise(resolve => {
    setTimeout(() => {
      resolve('This is ' + person + '’s bio.');
    }, delay);
  })
}
```

----------------------------------------

TITLE: Avoiding Redundant State with Unnecessary Effects (JavaScript)
DESCRIPTION: This snippet demonstrates an anti-pattern: using `useEffect` to derive state (`fullName`) from other existing state (`firstName`, `lastName`). This creates redundant state and an unnecessary side effect. The `fullName` could be calculated directly during rendering, making the code simpler and more efficient.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/escape-hatches.md#_snippet_5

LANGUAGE: JavaScript
CODE:
```
function Form() {
  const [firstName, setFirstName] = useState('Taylor');
  const [lastName, setLastName] = useState('Swift');

  // 🔴 Avoid: redundant state and unnecessary Effect
  const [fullName, setFullName] = useState('');
  useEffect(() => {
    setFullName(firstName + ' ' + lastName);
  }, [firstName, lastName]);
  // ...
}
```

----------------------------------------

TITLE: Handling Local Mutation in React Components (JavaScript)
DESCRIPTION: This snippet demonstrates acceptable local mutation within a React component's render function. It shows how to create a local array (`items`) and push elements into it, emphasizing that this is fine because `items` is recreated on each render, preventing observable side effects.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/reference/rules/components-and-hooks-must-be-pure.md#_snippet_5

LANGUAGE: javascript
CODE:
```
function FriendList({ friends }) {
  const items = []; // ✅ Good: locally created
  for (let i = 0; i < friends.length; i++) {
    const friend = friends[i];
    items.push(
      <Friend key={friend.id} friend={friend} />
    ); // ✅ Good: local mutation is okay
  }
  return <section>{items}</section>;
}
```

----------------------------------------

TITLE: Conditionally Reading Context with `use` Hook in React
DESCRIPTION: This snippet shows the flexibility of the `use` Hook, allowing it to be called inside a conditional statement (`if`). The `theme` context value is only read if the `show` prop is true, enabling conditional rendering based on context without violating React Hook rules.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/reference/react/use.md#_snippet_4

LANGUAGE: JavaScript
CODE:
```
function HorizontalRule({ show }) {
  if (show) {
    const theme = use(ThemeContext);
    return <hr className={theme} />;
  }
  return false;
}
```

----------------------------------------

TITLE: Avoiding Linter Suppression for React useEffect Dependencies
DESCRIPTION: This snippet warns against suppressing the React linter's `exhaustive-deps` rule. Suppressing this rule can hide missing dependencies, leading to bugs where effects do not react to changes in reactive values. It emphasizes that dependencies should be correctly declared or values made non-reactive instead of suppressing warnings.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/reference/react/useEffect.md#_snippet_27

LANGUAGE: JavaScript
CODE:
```
useEffect(() => {
  // ...
  // 🔴 Avoid suppressing the linter like this:
  // eslint-ignore-next-line react-hooks/exhaustive-deps
}, []);
```

----------------------------------------

TITLE: Updating Simple Form State in React with useState
DESCRIPTION: This React component demonstrates how to manage form input state using a single object in `useState`. Each input's `onChange` handler updates a specific property of the `form` state object by creating a new object using the spread syntax (`{ ...form, property: value }`), ensuring immutability.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/reference/react/useState.md#_snippet_16

LANGUAGE: js
CODE:
```
import { useState } from 'react';

export default function Form() {
  const [form, setForm] = useState({
    firstName: 'Barbara',
    lastName: 'Hepworth',
    email: 'bhepworth@sculpture.com',
  });

  return (
    <>
      <label>
        First name:
        <input
          value={form.firstName}
          onChange={e => {
            setForm({
              ...form,
              firstName: e.target.value
            });
          }}
        />
      </label>
      <label>
        Last name:
        <input
          value={form.lastName}
          onChange={e => {
            setForm({
              ...form,
              lastName: e.target.value
            });
          }}
        />
      </label>
      <label>
        Email:
        <input
          value={form.email}
          onChange={e => {
            setForm({
              ...form,
              email: e.target.value
            });
          }}
        />
      </label>
      <p>
        {form.firstName}{' '}
        {form.lastName}{' '}
        ({form.email})
      </p>
    </>
  );
}
```

LANGUAGE: css
CODE:
```
label { display: block; }
input { margin-left: 5px; }
```

----------------------------------------

TITLE: Performing Local Mutation on a New Object - JavaScript
DESCRIPTION: This snippet demonstrates a safe way to update state using 'local mutation'. A new object `nextPosition` is created, modified, and then used to update the state via `setPosition`. This is acceptable because the mutation occurs on an object that is not yet part of the component's state.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/updating-objects-in-state.md#_snippet_8

LANGUAGE: JavaScript
CODE:
```
const nextPosition = {};
nextPosition.x = e.clientX;
nextPosition.y = e.clientY;
setPosition(nextPosition);
```

----------------------------------------

TITLE: Caching Expensive Calculations with useMemo in React
DESCRIPTION: This snippet demonstrates how to use the `useMemo` Hook to cache the result of an expensive calculation in a React component. `useMemo` helps optimize performance by preventing re-computation of values unless their dependencies change. Here, `filterTodos` is only re-executed if `todos` or `tab` change, ensuring `visibleTodos` is efficiently updated for the `TodoList` component.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/reference/react/hooks.md#_snippet_4

LANGUAGE: js
CODE:
```
function TodoList({ todos, tab, theme }) {
  const visibleTodos = useMemo(() => filterTodos(todos, tab), [todos, tab]);
  // ...
}
```

----------------------------------------

TITLE: Invalid JSX in React Component (Initial)
DESCRIPTION: This JavaScript snippet shows common issues when pasting raw HTML into a React component, such as using the `class` attribute instead of `className` and returning multiple top-level elements without a single parent wrapper. It requires conversion to valid JSX.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/writing-markup-with-jsx.md#_snippet_9

LANGUAGE: js
CODE:
```
export default function Bio() {
  return (
    <div class="intro">
      <h1>Welcome to my website!</h1>
    </div>
    <p class="summary">
      You can find my thoughts here.
      <br><br>
      <b>And <i>pictures</b></i> of scientists!
    </p>
  );
}
```

----------------------------------------

TITLE: Rendering React Application into a Root
DESCRIPTION: This snippet illustrates the essential step of rendering a React application (`<App />`) into a previously created root using `root.render()`. It highlights that without this call, the application will not be displayed, addressing a common troubleshooting scenario.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/reference/react-dom/client/createRoot.md#_snippet_23

LANGUAGE: js
CODE:
```
import { createRoot } from 'react-dom/client';
import App from './App.js';

const root = createRoot(document.getElementById('root'));
root.render(<App />);
```

----------------------------------------

TITLE: Adding Tasks with Context Dispatch in React
DESCRIPTION: The `AddTask` component manages its input field's state using `useState`. It utilizes `useContext` to access the `TasksDispatchContext`, enabling it to dispatch an 'added' action to the global task reducer when the 'Add' button is clicked, thereby adding a new task to the list.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/scaling-up-with-reducer-and-context.md#_snippet_28

LANGUAGE: js
CODE:
```
import { useState, useContext } from 'react';
import { TasksDispatchContext } from './TasksContext.js';

export default function AddTask() {
  const [text, setText] = useState('');
  const dispatch = useContext(TasksDispatchContext);
  return (
    <>
      <input
        placeholder="Add task"
        value={text}
        onChange={e => setText(e.target.value)}
      />
      <button onClick={() => {
        setText('');
        dispatch({
          type: 'added',
          id: nextId++,
          text: text,
        }); 
      }}>Add</button>
    </>
  );
}

let nextId = 3;
```

----------------------------------------

TITLE: Issue: Re-rendering Child Components Without useCallback in React
DESCRIPTION: This example demonstrates the problem that arises when a function prop (`handleSubmit`) is defined directly within a component without `useCallback`. A new function instance is created on every re-render, causing `memo` on the child component (`ShippingForm`) to fail, leading to unnecessary re-renders.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/reference/react/useCallback.md#_snippet_5

LANGUAGE: JavaScript
CODE:
```
function ProductPage({ productId, referrer, theme }) {
  // Every time the theme changes, this will be a different function...
  function handleSubmit(orderDetails) {
    post('/product/' + productId + '/buy', {
      referrer,
      orderDetails,
    });
  }
  
  return (
    <div className={theme}>
      {/* ... so ShippingForm's props will never be the same, and it will re-render every time */}
      <ShippingForm onSubmit={handleSubmit} />
    </div>
  );
}
```

----------------------------------------

TITLE: React Client-Side Application Entry Point
DESCRIPTION: This `index.js` file serves as the entry point for a client-side React application. It uses `createRoot` from `react-dom/client` to render the main `App` component into the DOM, wrapped in `StrictMode` for development checks. This setup is typical for bootstrapping a React application in a browser environment.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/reference/react/use.md#_snippet_11

LANGUAGE: JavaScript
CODE:
```
import React, { StrictMode } from 'react';
import { createRoot } from 'react-dom/client';
import './styles.css';

// TODO: update this example to use
// the Codesandbox Server Component
// demo environment once it is created
import App from './App';

const root = createRoot(document.getElementById('root'));
root.render(
  <StrictMode>
    <App />
  </StrictMode>
);
```

----------------------------------------

TITLE: Fixing Impure Component with Array Cloning and StrictMode
DESCRIPTION: This example showcases the corrected `StoryTray` component, which now clones the `stories` array using `slice()` before adding new items, ensuring component purity. Combined with `StrictMode`, this setup helps developers identify and fix side-effect bugs, such as direct prop mutation, by consistently double-rendering components during development.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/reference/react/StrictMode.md#_snippet_11

LANGUAGE: javascript
CODE:
```
import { StrictMode } from 'react';
import { createRoot } from 'react-dom/client';
import './styles.css';

import App from './App';

const root = createRoot(document.getElementById('root'));
root.render(
  <StrictMode>
    <App />
  </StrictMode>
);
```

LANGUAGE: javascript
CODE:
```
import { useState } from 'react';
import StoryTray from './StoryTray.js';

let initialStories = [
  {id: 0, label: "Ankit's Story" },
  {id: 1, label: "Taylor's Story" },
];

export default function App() {
  let [stories, setStories] = useState(initialStories)
  return (
    <div
      style={{
        width: '100%',
        height: '100%',
        textAlign: 'center',
      }}
    >
      <StoryTray stories={stories} />
    </div>
  );
}
```

LANGUAGE: javascript
CODE:
```
import { useState } from 'react';

export default function StoryTray({ stories }) {
  const [isHover, setIsHover] = useState(false);
  const items = stories.slice(); // Clone the array
  items.push({ id: 'create', label: 'Create Story' });
  return (
    <ul
      onPointerEnter={() => setIsHover(true)}
      onPointerLeave={() => setIsHover(false)}
      style={{
        backgroundColor: isHover ? '#ddd' : '#fff'
      }}
    >
      {items.map(story => (
        <li key={story.id}>
          {story.label}
        </li>
      ))}
    </ul>
  );
}
```

LANGUAGE: css
CODE:
```
ul {
  margin: 0;
  list-style-type: none;
  height: 100%;
  display: flex;
  flex-wrap: wrap;
  padding: 10px;
}

li {
  border: 1px solid #aaa;
  border-radius: 6px;
  float: left;
  margin: 5px;
  padding: 5px;
  width: 70px;
  height: 100px;
}
```

----------------------------------------

TITLE: Optimized Memoization by Moving Object Creation Inside useMemo
DESCRIPTION: This snippet presents an even better approach for memoizing calculations dependent on objects. By moving the `searchOptions` object creation directly inside the `useMemo` callback, the calculation depends only on primitive values (`allItems`, `text`), ensuring efficient memoization.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/reference/react/useMemo.md#_snippet_33

LANGUAGE: javascript
CODE:
```
function Dropdown({ allItems, text }) {
  const visibleItems = useMemo(() => {
    const searchOptions = { matchMode: 'whole-word', text };
    return searchItems(allItems, searchOptions);
  }, [allItems, text]); // ✅ Only changes when allItems or text changes
  // ...
```

----------------------------------------

TITLE: Asynchronous Rendering in Server Components
DESCRIPTION: This snippet highlights a component (`AnimatedWeatherCard`) that uses `async` and `await` for data fetching, which is specifically supported for Server Components in React. It shows how a component can `await` the result of a memoized data-fetching function like `getTemperature` to render content once the data is available.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/reference/react/cache.md#_snippet_9

LANGUAGE: JavaScript
CODE:
```
async function AnimatedWeatherCard({city}) {
	const temperature = await getTemperature(city);
	// ...
}
```

----------------------------------------

TITLE: Updating State in React Effect (Correct Approach)
DESCRIPTION: This snippet shows the correct way to update state within a useEffect hook when the new state depends on the previous state. It uses an updater function (msgs => [...msgs, receivedMessage]) passed to setMessages. This avoids reading the 'messages' state directly within the Effect, allowing 'messages' to be omitted from the dependency array and preventing unnecessary re-synchronization.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/removing-effect-dependencies.md#_snippet_19

LANGUAGE: javascript
CODE:
```
function ChatRoom({ roomId }) {
  const [messages, setMessages] = useState([]);
  useEffect(() => {
    const connection = createConnection();
    connection.connect();
    connection.on('message', (receivedMessage) => {
      setMessages(msgs => [...msgs, receivedMessage]);
    });
    return () => connection.disconnect();
  }, [roomId]); // ✅ All dependencies declared
  // ...

```

----------------------------------------

TITLE: Using startTransition for Non-Urgent Updates in React (JavaScript)
DESCRIPTION: This snippet demonstrates how to use the startTransition API in React to distinguish between urgent and non-urgent state updates. Urgent updates (like setInputValue) are applied immediately, while non-urgent updates (like setSearchQuery) wrapped in startTransition are handled as transitions, allowing them to be interrupted by more urgent interactions.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/blog/2022/03/29/react-v18.md#_snippet_1

LANGUAGE: JavaScript
CODE:
```
import { startTransition } from 'react';

// Urgent: Show what was typed
setInputValue(input);

// Mark any state updates inside as transitions
startTransition(() => {
  // Transition: Show the results
  setSearchQuery(input);
});
```

----------------------------------------

TITLE: Fixing Effect Cleanup with Strict Mode
DESCRIPTION: This set of code snippets presents the corrected version of the React application. The `useEffect` hook in the `ChatRoom` component now includes a cleanup function that disconnects the chat connection. Running this in `StrictMode` confirms that the active connection count remains stable, demonstrating how proper cleanup prevents resource leaks and ensures correct behavior, especially when components re-render or unmount.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/reference/react/StrictMode.md#_snippet_22

LANGUAGE: JavaScript
CODE:
```
import { StrictMode } from 'react';
import { createRoot } from 'react-dom/client';
import './styles.css';

import App from './App';

const root = createRoot(document.getElementById("root"));
root.render(
  <StrictMode>
    <App />
  </StrictMode>
);
```

LANGUAGE: JavaScript
CODE:
```
import { useState, useEffect } from 'react';
import { createConnection } from './chat.js';

const serverUrl = 'https://localhost:1234';

function ChatRoom({ roomId }) {
  useEffect(() => {
    const connection = createConnection(serverUrl, roomId);
    connection.connect();
    return () => connection.disconnect();
  }, [roomId]);

  return <h1>Welcome to the {roomId} room!</h1>;
}

export default function App() {
  const [roomId, setRoomId] = useState('general');
  const [show, setShow] = useState(false);
  return (
    <>
      <label>
        Choose the chat room:{' '}
        <select
          value={roomId}
          onChange={e => setRoomId(e.target.value)}
        >
          <option value="general">general</option>
          <option value="travel">travel</option>
          <option value="music">music</option>
        </select>
      </label>
      <button onClick={() => setShow(!show)}>
        {show ? 'Close chat' : 'Open chat'}
      </button>
      {show && <hr />}
      {show && <ChatRoom roomId={roomId} />}
    </>
  );
}
```

LANGUAGE: JavaScript
CODE:
```
let connections = 0;

export function createConnection(serverUrl, roomId) {
  // A real implementation would actually connect to the server
  return {
    connect() {
      console.log('✅ Connecting to "' + roomId + '" room at ' + serverUrl + '...');
      connections++;
      console.log('Active connections: ' + connections);
    },
    disconnect() {
      console.log('❌ Disconnected from "' + roomId + '" room at ' + serverUrl);
      connections--;
      console.log('Active connections: ' + connections);
    }
  };
}
```

LANGUAGE: CSS
CODE:
```
input { display: block; margin-bottom: 20px; }
button { margin-left: 10px; }
```

----------------------------------------

TITLE: Updating `useRef` and `createContext` Calls to Require Arguments (TypeScript)
DESCRIPTION: In React 19, `useRef` and `createContext` now require an argument, simplifying their type signatures and making all refs mutable. This snippet demonstrates the updated usage, showing that calling them without arguments will result in a TypeScript error, while passing `undefined` is valid.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/blog/2024/04/25/react-19-upgrade-guide.md#_snippet_34

LANGUAGE: ts
CODE:
```
// @ts-expect-error: Expected 1 argument but saw none
useRef();
// Passes
useRef(undefined);
// @ts-expect-error: Expected 1 argument but saw none
createContext();
// Passes
createContext(undefined);
```

----------------------------------------

TITLE: Implicit Children Prop Usage in JSX
DESCRIPTION: Demonstrates how the `children` prop is implicitly specified in JSX by nesting elements, allowing content to be passed inside a component. This is the most common way to define a component's inner content.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/reference/react-dom/components/common.md#_snippet_1

LANGUAGE: JSX
CODE:
```
<div><span /></div>
```

----------------------------------------

TITLE: Migrating `ReactDOM.render` to `createRoot` - JavaScript
DESCRIPTION: This JavaScript snippet illustrates the migration from the deprecated `ReactDOM.render` to the modern `ReactDOM.createRoot` API. `createRoot` provides a new way to manage React roots, enabling concurrent features and improved performance. The example shows both the old and new approaches for rendering a React application.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/blog/2024/04/25/react-19-upgrade-guide.md#_snippet_22

LANGUAGE: JavaScript
CODE:
```
// Before
import {render} from 'react-dom';
render(<App />, document.getElementById('root'));

// After
import {createRoot} from 'react-dom/client';
const root = createRoot(document.getElementById('root'));
root.render(<App />);
```

----------------------------------------

TITLE: Defining and Using React Components (JavaScript & CSS)
DESCRIPTION: This snippet defines a `Profile` React component that renders an image and then exports a `Gallery` component that renders multiple instances of the `Profile` component. It demonstrates how to compose UI using reusable components and includes basic CSS for styling the image.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/your-first-component.md#_snippet_6

LANGUAGE: javascript
CODE:
```
function Profile() {
  return (
    <img
      src="https://i.imgur.com/MK3eW3As.jpg"
      alt="Katherine Johnson"
    />
  );
}

export default function Gallery() {
  return (
    <section>
      <h1>Amazing scientists</h1>
      <Profile />
      <Profile />
      <Profile />
    </section>
  );
}
```

LANGUAGE: css
CODE:
```
img { margin: 0 10px 10px 0; height: 90px; }
```

----------------------------------------

TITLE: Fixed React Counter with Updater Function - JavaScript
DESCRIPTION: This corrected React counter component uses an updater function `setScore(s => s + 1)` to queue multiple state updates correctly. The updater function receives the *latest* pending state, ensuring that each `increment()` call builds upon the previous one, fixing the '+3' button functionality.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/adding-interactivity.md#_snippet_12

LANGUAGE: js
CODE:
```
import { useState } from 'react';

export default function Counter() {
  const [score, setScore] = useState(0);

  function increment() {
    setScore(s => s + 1);
  }

  return (
    <>
      <button onClick={() => increment()}>+1</button>
      <button onClick={() => {
        increment();
        increment();
        increment();
      }}>+3</button>
      <h1>Score: {score}</h1>
    </>
  )
}
```

----------------------------------------

TITLE: Synchronizing with External Systems using useEffect in React
DESCRIPTION: This snippet shows how to use the `useEffect` Hook to connect a React component to an external system, such as a chat room connection. `useEffect` allows components to perform side effects after rendering, like data fetching, subscriptions, or manually changing the DOM. The effect sets up a connection when `roomId` changes and cleans it up when the component unmounts or `roomId` changes again.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/reference/react/hooks.md#_snippet_3

LANGUAGE: js
CODE:
```
function ChatRoom({ roomId }) {
  useEffect(() => {
    const connection = createConnection(roomId);
    connection.connect();
    return () => connection.disconnect();
  }, [roomId]);
  // ...
```

----------------------------------------

TITLE: Sharing Memoized Data Fetches with cache in React JS
DESCRIPTION: This example illustrates the use of the `cache` utility in Server Components to memoize and share expensive operations, such as data fetches (`fetchReport`), across multiple component instances. Unlike `useMemo`, `cache` allows work to be shared globally within a server request, preventing duplicate fetches for the same data.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/reference/react/cache.md#_snippet_14

LANGUAGE: JavaScript
CODE:
```
const cachedFetchReport = cache(fetchReport);

function WeatherReport({city}) {
  const report = cachedFetchReport(city);
  // ...
}

function App() {
  const city = "Los Angeles";
  return (
    <>
      <WeatherReport city={city} />
      <WeatherReport city={city} />
    </>
  );
}
```

----------------------------------------

TITLE: Fixing Stuck Form Inputs with React useState Hook
DESCRIPTION: This corrected React component resolves the 'stuck' input issue by utilizing the `useState` hook to manage `firstName` and `lastName`. By using state variables and their corresponding setter functions (`setFirstName`, `setLastName`), React re-renders the component when the state changes, ensuring the input fields correctly reflect user input. The `handleReset` function is also updated to reset the state variables.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/state-a-components-memory.md#_snippet_32

LANGUAGE: JavaScript
CODE:
```
import { useState } from 'react';

export default function Form() {
  const [firstName, setFirstName] = useState('');
  const [lastName, setLastName] = useState('');

  function handleFirstNameChange(e) {
    setFirstName(e.target.value);
  }

  function handleLastNameChange(e) {
    setLastName(e.target.value);
  }

  function handleReset() {
    setFirstName('');
    setLastName('');
  }

  return (
    <form onSubmit={e => e.preventDefault()}>
      <input
        placeholder="First name"
        value={firstName}
        onChange={handleFirstNameChange}
      />
      <input
        placeholder="Last name"
        value={lastName}
        onChange={handleLastNameChange}
      />
      <h1>Hi, {firstName} {lastName}</h1>
      <button onClick={handleReset}>Reset</button>
    </form>
  );
}
```

LANGUAGE: CSS
CODE:
```
h1 { margin-top: 10px; }
```

----------------------------------------

TITLE: Correctly Specifying `useEffect` Dependencies for Video Playback in React
DESCRIPTION: This example shows the correct implementation of `useEffect` by including `isPlaying` in its dependency array. This ensures the effect only re-runs when the `isPlaying` prop changes, preventing unnecessary calls to `video.play()` or `video.pause()` on unrelated state updates (e.g., typing in the input field). It demonstrates how to properly manage effect re-execution based on specific prop or state changes.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/synchronizing-with-effects.md#_snippet_10

LANGUAGE: js
CODE:
```
import { useState, useRef, useEffect } from 'react';

function VideoPlayer({ src, isPlaying }) {
  const ref = useRef(null);

  useEffect(() => {
    if (isPlaying) {
      console.log('Calling video.play()');
      ref.current.play();
    } else {
      console.log('Calling video.pause()');
      ref.current.pause();
    }
  }, [isPlaying]);

  return <video ref={ref} src={src} loop playsInline />;
}

export default function App() {
  const [isPlaying, setIsPlaying] = useState(false);
  const [text, setText] = useState('');
  return (
    <>
      <input value={text} onChange={e => setText(e.target.value)} />
      <button onClick={() => setIsPlaying(!isPlaying)}>
        {isPlaying ? 'Pause' : 'Play'}
      </button>
      <VideoPlayer
        isPlaying={isPlaying}
        src="https://interactive-examples.mdn.mozilla.net/media/cc0-videos/flower.mp4"
      />
    </>
  );
}
```

LANGUAGE: css
CODE:
```
input, button { display: block; margin-bottom: 20px; }
video { width: 250px; }
```

----------------------------------------

TITLE: Modifying Existing State Object Directly (Problematic) - JavaScript
DESCRIPTION: This code snippet illustrates a problematic pattern where an existing object, assumed to be part of React state, is directly mutated. This approach bypasses React's rendering mechanism and can lead to unexpected behavior or stale UI.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/updating-objects-in-state.md#_snippet_7

LANGUAGE: JavaScript
CODE:
```
position.x = e.clientX;
position.y = e.clientY;
```

----------------------------------------

TITLE: Task State Management with Reducer and Context (React)
DESCRIPTION: This file defines the React Contexts (`TasksContext`, `TasksDispatchContext`) and a `TasksProvider` component that uses `useReducer` to manage the application's task state. It also includes the `tasksReducer` logic for handling 'added', 'changed', and 'deleted' actions, and custom hooks (`useTasks`, `useTasksDispatch`) for consuming the context.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/managing-state.md#_snippet_17

LANGUAGE: JavaScript
CODE:
```
import { createContext, useContext, useReducer } from 'react';

const TasksContext = createContext(null);
const TasksDispatchContext = createContext(null);

export function TasksProvider({ children }) {
  const [tasks, dispatch] = useReducer(
    tasksReducer,
    initialTasks
  );

  return (
    <TasksContext.Provider value={tasks}>
      <TasksDispatchContext.Provider
        value={dispatch}
      >
        {children}
      </TasksDispatchContext.Provider>
    </TasksContext.Provider>
  );
}

export function useTasks() {
  return useContext(TasksContext);
}

export function useTasksDispatch() {
  return useContext(TasksDispatchContext);
}

function tasksReducer(tasks, action) {
  switch (action.type) {
    case 'added': {
      return [...tasks, {
        id: action.id,
        text: action.text,
        done: false
      }];
    }
    case 'changed': {
      return tasks.map(t => {
        if (t.id === action.task.id) {
          return action.task;
        } else {
          return t;
        }
      });
    }
    case 'deleted': {
      return tasks.filter(t => t.id !== action.id);
    }
    default: {
      throw Error('Unknown action: ' + action.type);
    }
  }
}

const initialTasks = [
  { id: 0, text: 'Philosopher’s Path', done: true },
  { id: 1, text: 'Visit the temple', done: false },
  { id: 2, text: 'Drink matcha', done: false }
];
```

----------------------------------------

TITLE: Creating an Avatar Component (JavaScript)
DESCRIPTION: This `Avatar.js` snippet defines a reusable `Avatar` component that displays a user's image. It takes `person` and `size` props to customize the image source and dimensions, utilizing a utility function `getImageUrl` for image URL generation.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/passing-props-to-a-component.md#_snippet_14

LANGUAGE: JavaScript
CODE:
```
import { getImageUrl } from './utils.js';

export default function Avatar({ person, size }) {
  return (
    <img
      className="avatar"
      src={getImageUrl(person)}
      alt={person.name}
      width={size}
      height={size}
    />
  );
}
```

----------------------------------------

TITLE: Moving State Up to Parent Component in React
DESCRIPTION: This snippet demonstrates the initial step of lifting state up. It shows how to define the `count` state and `handleClick` function within the `MyApp` parent component, preparing to share it with child components. The `MyButton` component is shown without its own state, indicating the state has been moved.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/index.md#_snippet_22

LANGUAGE: JavaScript
CODE:
```
export default function MyApp() {
  const [count, setCount] = useState(0);

  function handleClick() {
    setCount(count + 1);
  }

  return (
    <div>
      <h1>Counters that update separately</h1>
      <MyButton />
      <MyButton />
    </div>
  );
}

function MyButton() {
  // ... we're moving code from here ...
}
```

----------------------------------------

TITLE: Implementing React Server Component with Data Fetching
DESCRIPTION: This JavaScript snippet demonstrates a server-only React component designed to run during the build or on the server. It shows how to directly interact with a data layer (like a database) and pass the processed data down to client-side components without increasing the client-side bundle size.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/creating-a-react-app.md#_snippet_3

LANGUAGE: javascript
CODE:
```
// This component runs *only* on the server (or during the build).
async function Talks({ confId }) {
  // 1. You're on the server, so you can talk to your data layer. API endpoint not required.
  const talks = await db.Talks.findAll({ confId });

  // 2. Add any amount of rendering logic. It won't make your JavaScript bundle larger.
  const videos = talks.map(talk => talk.video);

  // 3. Pass the data down to the components that will run in the browser.
  return <SearchableVideoList videos={videos} />;
}
```

----------------------------------------

TITLE: Reading React Context with useContext Hook
DESCRIPTION: This snippet shows how to consume a context value within a functional component using the `useContext` Hook. It takes the context object (e.g., `MyContext`) as an argument and returns the current context value for that context. This hook allows components to subscribe to context changes and re-render when the context value updates.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/passing-data-deeply-with-context.md#_snippet_41

LANGUAGE: JavaScript
CODE:
```
useContext(MyContext)
```

----------------------------------------

TITLE: Creating Non-Blocking Action with startTransition in React
DESCRIPTION: Illustrates how to use the startTransition function returned by useTransition to wrap state updates and asynchronous operations. This makes the updates non-blocking, allowing the UI to remain responsive while the Action is processed in the background.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/reference/react/useTransition.md#_snippet_4

LANGUAGE: JavaScript
CODE:
```
import {useState, useTransition} from 'react';
import {updateQuantity} from './api';

function CheckoutForm() {
  const [isPending, startTransition] = useTransition();
  const [quantity, setQuantity] = useState(1);

  function onSubmit(newQuantity) {
    startTransition(async function () {
      const savedQuantity = await updateQuantity(newQuantity);
      startTransition(() => {
        setQuantity(savedQuantity);
      });
    });
  }
  // ...
}
```

----------------------------------------

TITLE: Controlling Video Playback with React Refs and Event Handlers
DESCRIPTION: This React component enhances the video player by using `useRef` to directly interact with the `<video>` DOM element. The `handleClick` function now calls `play()` or `pause()` on the video ref based on the `isPlaying` state. Additionally, `onPlay` and `onPause` event handlers are added to the video element to synchronize the `isPlaying` state with native browser controls, ensuring the button's text accurately reflects the video's status.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/manipulating-the-dom-with-refs.md#_snippet_22

LANGUAGE: JavaScript
CODE:
```
import { useState, useRef } from 'react';

export default function VideoPlayer() {
  const [isPlaying, setIsPlaying] = useState(false);
  const ref = useRef(null);

  function handleClick() {
    const nextIsPlaying = !isPlaying;
    setIsPlaying(nextIsPlaying);

    if (nextIsPlaying) {
      ref.current.play();
    } else {
      ref.current.pause();
    }
  }

  return (
    <>
      <button onClick={handleClick}>
        {isPlaying ? 'Pause' : 'Play'}
      </button>
      <video
        width="250"
        ref={ref}
        onPlay={() => setIsPlaying(true)}
        onPause={() => setIsPlaying(false)}
      >
        <source
          src="https://interactive-examples.mdn.mozilla.net/media/cc0-videos/flower.mp4"
          type="video/mp4"
        />
      </video>
    </>
  )
}
```

----------------------------------------

TITLE: Complete React Application with Inverse Data Flow
DESCRIPTION: This comprehensive snippet presents the full `src/App.js` file, showcasing a React application that implements inverse data flow. It includes `FilterableProductTable` managing state and passing updater functions, `SearchBar` handling user input and updating parent state, and `ProductTable` displaying filtered products.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/thinking-in-react.md#_snippet_11

LANGUAGE: JSX
CODE:
```
import { useState } from 'react';

function FilterableProductTable({ products }) {
  const [filterText, setFilterText] = useState('');
  const [inStockOnly, setInStockOnly] = useState(false);

  return (
    <div>
      <SearchBar 
        filterText={filterText} 
        inStockOnly={inStockOnly} 
        onFilterTextChange={setFilterText} 
        onInStockOnlyChange={setInStockOnly} />
      <ProductTable 
        products={products} 
        filterText={filterText}
        inStockOnly={inStockOnly} />
    </div>
  );
}

function ProductCategoryRow({ category }) {
  return (
    <tr>
      <th colSpan="2">
        {category}
      </th>
    </tr>
  );
}

function ProductRow({ product }) {
  const name = product.stocked ? product.name :
    <span style={{ color: 'red' }}>
      {product.name}
    </span>;

  return (
    <tr>
      <td>{name}</td>
      <td>{product.price}</td>
    </tr>
  );
}

function ProductTable({ products, filterText, inStockOnly }) {
  const rows = [];
  let lastCategory = null;

  products.forEach((product) => {
    if (
      product.name.toLowerCase().indexOf(
        filterText.toLowerCase()
      ) === -1
    ) {
      return;
    }
    if (inStockOnly && !product.stocked) {
      return;
    }
    if (product.category !== lastCategory) {
      rows.push(
        <ProductCategoryRow
          category={product.category}
          key={product.category} />
      );
    }
    rows.push(
      <ProductRow
        product={product}
        key={product.name} />
    );
    lastCategory = product.category;
  });

  return (
    <table>
      <thead>
        <tr>
          <th>Name</th>
          <th>Price</th>
        </tr>
      </thead>
      <tbody>{rows}</tbody>
    </table>
  );
}

function SearchBar({
  filterText,
  inStockOnly,
  onFilterTextChange,
  onInStockOnlyChange
}) {
  return (
    <form>
      <input 
        type="text" 
        value={filterText} placeholder="Search..." 
        onChange={(e) => onFilterTextChange(e.target.value)} />
      <label>
        <input 
          type="checkbox" 
          checked={inStockOnly} 
          onChange={(e) => onInStockOnlyChange(e.target.checked)} />
        {' '}
        Only show products in stock
      </label>
    </form>
  );
}

const PRODUCTS = [
  {category: "Fruits", price: "$1", stocked: true, name: "Apple"},
  {category: "Fruits", price: "$1", stocked: true, name: "Dragonfruit"},
  {category: "Fruits", price: "$2", stocked: false, name: "Passionfruit"},
  {category: "Vegetables", price: "$2", stocked: true, name: "Spinach"},
  {category: "Vegetables", price: "$4", stocked: false, name: "Pumpkin"},
  {category: "Vegetables", price: "$1", stocked: true, name: "Peas"}
];

export default function App() {
  return <FilterableProductTable products={PRODUCTS} />;
}
```

----------------------------------------

TITLE: Corrected Request Counter Implementation in React using Updater Functions
DESCRIPTION: This snippet provides the corrected implementation for the request counter in a React component. It uses state updater functions (`setPending(p => p + 1)`, `setPending(p => p - 1)`, `setCompleted(c => c + 1)`) to ensure that state updates are based on the *latest* state value, preventing race conditions and ensuring accurate counter behavior, especially during rapid clicks or asynchronous operations. The `delay` function simulates an asynchronous operation.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/queueing-a-series-of-state-updates.md#_snippet_12

LANGUAGE: JavaScript
CODE:
```
import { useState } from 'react';

export default function RequestTracker() {
  const [pending, setPending] = useState(0);
  const [completed, setCompleted] = useState(0);

  async function handleClick() {
    setPending(p => p + 1);
    await delay(3000);
    setPending(p => p - 1);
    setCompleted(c => c + 1);
  }

  return (
    <>
      <h3>
        Pending: {pending}
      </h3>
      <h3>
        Completed: {completed}
      </h3>
      <button onClick={handleClick}>
        Buy     
      </button>
    </>
  );
}

function delay(ms) {
  return new Promise(resolve => {
    setTimeout(resolve, ms);
  });
}
```

----------------------------------------

TITLE: MailClient Component with Stable Highlight (Solution)
DESCRIPTION: This is the corrected version of the `MailClient` component. To fix the disappearing highlight, it now stores `highlightedId` (the ID of the highlighted letter) instead of the full `highlightedLetter` object. This ensures that the highlight remains stable even when the `letters` array is updated, as the ID reference remains consistent. `handleHover` and `handleStar` now operate on `letterId`.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/choosing-the-state-structure.md#_snippet_44

LANGUAGE: javascript
CODE:
```
import { useState } from 'react';
import { initialLetters } from './data.js';
import Letter from './Letter.js';

export default function MailClient() {
  const [letters, setLetters] = useState(initialLetters);
  const [highlightedId, setHighlightedId ] = useState(null);

  function handleHover(letterId) {
    setHighlightedId(letterId);
  }

  function handleStar(starredId) {
    setLetters(letters.map(letter => {
      if (letter.id === starredId) {
        return {
          ...letter,
          isStarred: !letter.isStarred
        };
      } else {
        return letter;
      }
    }));
  }

  return (
    <>
      <h2>Inbox</h2>
      <ul>
        {letters.map(letter => (
          <Letter
            key={letter.id}
            letter={letter}
            isHighlighted={
              letter.id === highlightedId
            }
            onHover={handleHover}
            onToggleStar={handleStar}
          />
        ))}
      </ul>
    </>
  );
}
```

----------------------------------------

TITLE: Correct Cache Usage: Exporting Memoized Function from Dedicated Module
DESCRIPTION: This snippet demonstrates the recommended approach for defining a memoized function using `cache`. By exporting `cache(calculateWeekReport)` from its own dedicated module (`getWeekReport.js`), it ensures that all components importing this module will access the *same* memoized function and thus the *same* cache, maximizing cache hits and reducing redundant computations.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/reference/react/cache.md#_snippet_5

LANGUAGE: JavaScript
CODE:
```
// getWeekReport.js
import {cache} from 'react';
import {calculateWeekReport} from './report';

export default cache(calculateWeekReport);
```

----------------------------------------

TITLE: Caching Calculation with useMemo in React
DESCRIPTION: This snippet demonstrates how to use the `useMemo` hook to cache the result of the `filterTodos` function. It takes a calculation function and a dependency array (`[todos, tab]`). The calculation will only re-run if `todos` or `tab` change, preventing expensive re-calculations on every render.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/reference/react/useMemo.md#_snippet_2

LANGUAGE: JavaScript
CODE:
```
import { useMemo } from 'react';

function TodoList({ todos, tab, theme }) {
  const visibleTodos = useMemo(() => filterTodos(todos, tab), [todos, tab]);
  // ...
}
```

----------------------------------------

TITLE: Implementing Chat Room Connection with React useEffect
DESCRIPTION: This snippet defines the main React application, including a `ChatRoom` component that uses `useEffect` to manage a connection to a chat server. It demonstrates how to connect and disconnect from a chat room based on `roomId` changes and how to toggle the chat room's visibility.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/lifecycle-of-reactive-effects.md#_snippet_9

LANGUAGE: JavaScript
CODE:
```
import { useState, useEffect } from 'react';
import { createConnection } from './chat.js';

const serverUrl = 'https://localhost:1234';

function ChatRoom({ roomId }) {
  useEffect(() => {
    const connection = createConnection(serverUrl, roomId);
    connection.connect();
    return () => connection.disconnect();
  }, [roomId]);
  return <h1>Welcome to the {roomId} room!</h1>;
}

export default function App() {
  const [roomId, setRoomId] = useState('general');
  const [show, setShow] = useState(false);
  return (
    <>
      <label>
        Choose the chat room:{' '}
        <select
          value={roomId}
          onChange={e => setRoomId(e.target.value)}
        >
          <option value="general">general</option>
          <option value="travel">travel</option>
          <option value="music">music</option>
        </select>
      </label>
      <button onClick={() => setShow(!show)}>
        {show ? 'Close chat' : 'Open chat'}
      </button>
      {show && <hr />}
      {show && <ChatRoom roomId={roomId} />}
    </>
  );
}
```

----------------------------------------

TITLE: Buggy Timer Component - React JS
DESCRIPTION: This React component implements a counter with adjustable increment and delay. The `setInterval` call is incorrectly placed inside a `useEffectEvent` (`onMount`), causing the interval to capture the initial `delay` value and ignore subsequent changes.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/separating-events-from-effects.md#_snippet_30

LANGUAGE: json
CODE:
```
{
  "dependencies": {
    "react": "experimental",
    "react-dom": "experimental",
    "react-scripts": "latest"
  },
  "scripts": {
    "start": "react-scripts start",
    "build": "react-scripts build",
    "test": "react-scripts test --env=jsdom",
    "eject": "react-scripts eject"
  }
}
```

LANGUAGE: js
CODE:
```
import { useState, useEffect } from 'react';
import { experimental_useEffectEvent as useEffectEvent } from 'react';

export default function Timer() {
  const [count, setCount] = useState(0);
  const [increment, setIncrement] = useState(1);
  const [delay, setDelay] = useState(100);

  const onTick = useEffectEvent(() => {
    setCount(c => c + increment);
  });

  const onMount = useEffectEvent(() => {
    return setInterval(() => {
      onTick();
    }, delay);
  });

  useEffect(() => {
    const id = onMount();
    return () => {
      clearInterval(id);
    }
  }, []);

  return (
    <>
      <h1>
        Counter: {count}
        <button onClick={() => setCount(0)}>Reset</button>
      </h1>
      <hr />
      <p>
        Increment by:
        <button disabled={increment === 0} onClick={() => {
          setIncrement(i => i - 1);
        }}>–</button>
        <b>{increment}</b>
        <button onClick={() => {
          setIncrement(i => i + 1);
        }}>+工程师</button>
      </p>
      <p>
        Increment delay:
        <button disabled={delay === 100} onClick={() => {
          setDelay(d => d - 100);
        }}>–100 ms</button>
        <b>{delay} ms</b>
        <button onClick={() => {
          setDelay(d => d + 100);
        }}>+100 ms</button>
      </p>
    </>
  );
}
```

LANGUAGE: css
CODE:
```
button { margin: 10px; }
```

----------------------------------------

TITLE: Reading Props in React Child Component
DESCRIPTION: This snippet shows how the `MyButton` child component is modified to receive `count` and `onClick` as props from its parent. Instead of managing its own state, it now displays the `count` value passed down and executes the `onClick` handler provided by the parent, ensuring synchronized updates.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/index.md#_snippet_24

LANGUAGE: JavaScript
CODE:
```
function MyButton({ count, onClick }) {
  return (
    <button onClick={onClick}>
      Clicked {count} times
    </button>
  );
}
```

----------------------------------------

TITLE: Nesting Custom React Components in JSX (JavaScript)
DESCRIPTION: This snippet illustrates how to nest custom React components, like `<Avatar />` inside a `<Card>` component. This pattern is used to compose UIs from reusable components, where the parent component can receive the nested content via the `children` prop.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/passing-props-to-a-component.md#_snippet_12

LANGUAGE: JavaScript
CODE:
```
<Card>
  <Avatar />
</Card>
```

----------------------------------------

TITLE: Implementing Theme Overrides with React Context (JavaScript)
DESCRIPTION: This snippet demonstrates how to use React Context to manage and override themes within a component tree. It defines a `ThemeContext`, provides a default 'dark' theme, and then overrides it to 'light' for the `Footer` component and its children, showcasing how context values can be dynamically changed for sub-trees.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/reference/react/useContext.md#_snippet_25

LANGUAGE: JavaScript
CODE:
```
import { createContext, useContext } from 'react';

const ThemeContext = createContext(null);

export default function MyApp() {
  return (
    <ThemeContext.Provider value="dark">
      <Form />
    </ThemeContext.Provider>
  )
}

function Form() {
  return (
    <Panel title="Welcome">
      <Button>Sign up</Button>
      <Button>Log in</Button>
      <ThemeContext.Provider value="light">
        <Footer />
      </ThemeContext.Provider>
    </Panel>
  );
}

function Footer() {
  return (
    <footer>
      <Button>Settings</Button>
    </footer>
  );
}

function Panel({ title, children }) {
  const theme = useContext(ThemeContext);
  const className = 'panel-' + theme;
  return (
    <section className={className}>
      {title && <h1>{title}</h1>}
      {children}
    </section>
  )
}

function Button({ children }) {
  const theme = useContext(ThemeContext);
  const className = 'button-' + theme;
  return (
    <button className={className}>
      {children}
    </button>
  );
}
```

LANGUAGE: CSS
CODE:
```
footer {
  margin-top: 20px;
  border-top: 1px solid #aaa;
}

.panel-light,
.panel-dark {
  border: 1px solid black;
  border-radius: 4px;
  padding: 20px;
}
.panel-light {
  color: #222;
  background: #fff;
}

.panel-dark {
  color: #fff;
  background: rgb(23, 32, 42);
}

.button-light,
.button-dark {
  border: 1px solid #777;
  padding: 5px;
  margin-right: 10px;
  margin-top: 10px;
}

.button-dark {
  background: #222;
  color: #fff;
}

.button-light {
  background: #fff;
  color: #222;
}
```

----------------------------------------

TITLE: Implementing Suspense for Data Fetching Loading State
DESCRIPTION: This JSX snippet shows how to wrap a component that fetches data (like the `Talks` component) with React's `Suspense` boundary. This allows you to define a fallback UI (like a loading indicator or skeleton) that is displayed while the data for the wrapped component is being fetched.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/creating-a-react-app.md#_snippet_4

LANGUAGE: jsx
CODE:
```
<Suspense fallback={<TalksLoading />}>
  <Talks confId={conf.id} />
</Suspense>
```

----------------------------------------

TITLE: Adding New Todo Items in React - JavaScript
DESCRIPTION: This component provides a user interface for adding new todo items. It manages the input field's value using its own `useState` hook and, upon button click, clears the input and invokes the `onAddTodo` prop, passing the new todo title to the parent component for state update.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/reference/react/useState.md#_snippet_19

LANGUAGE: JavaScript
CODE:
```
import { useState } from 'react';

export default function AddTodo({ onAddTodo }) {
  const [title, setTitle] = useState('');
  return (
    <>
      <input
        placeholder="Add todo"
        value={title}
        onChange={e => setTitle(e.target.value)}
      />
      <button onClick={() => {
        setTitle('');
        onAddTodo(title);
      }}>Add</button>
    </>
  )
}
```

----------------------------------------

TITLE: Incorrect State Mutation in React Reducer (Wrong)
DESCRIPTION: This snippet illustrates a common mistake in React reducers: directly mutating the existing state object or array. React uses `Object.is` for comparison, and if the returned state object reference is the same as the previous one, the update will be ignored, preventing the UI from re-rendering. This leads to silent failures where dispatched actions don't reflect on the screen.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/reference/react/useReducer.md#_snippet_26

LANGUAGE: javascript
CODE:
```
function reducer(state, action) {
  switch (action.type) {
    case 'incremented_age': {
      // 🚩 Wrong: mutating existing object
      state.age++;
      return state;
    }
    case 'changed_name': {
      // 🚩 Wrong: mutating existing object
      state.name = action.nextName;
      return state;
    }
    // ...
  }
}
```

----------------------------------------

TITLE: Prepending Items to React State Array (Spread Syntax)
DESCRIPTION: This snippet shows how to prepend a new item to the beginning of an array in React state using the array spread syntax. By placing the new item before `...artists`, a new array is formed with the new item at the start, maintaining immutability.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/updating-arrays-in-state.md#_snippet_3

LANGUAGE: javascript
CODE:
```
setArtists([
  { id: nextId++, name: name },
  ...artists // Put old items at the end
]);
```

----------------------------------------

TITLE: Managing Time-Dependent State with useEffect Hook - JavaScript/React
DESCRIPTION: This example shows a correct approach to displaying a continuously updating time in a React component by leveraging the `useState` and `useEffect` hooks. The `useTime` custom hook initializes the time once and then updates it every second using `setInterval` within `useEffect`, ensuring the non-idempotent `new Date()` call occurs outside the render phase. A cleanup function is provided to prevent memory leaks.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/reference/rules/components-and-hooks-must-be-pure.md#_snippet_4

LANGUAGE: js
CODE:
```
import { useState, useEffect } from 'react';

function useTime() {
  // 1. Keep track of the current date's state. `useState` receives an initializer function as its
  //    initial state. It only runs once when the hook is called, so only the current date at the
  //    time the hook is called is set first.
  const [time, setTime] = useState(() => new Date());

  useEffect(() => {
    // 2. Update the current date every second using `setInterval`.
    const id = setInterval(() => {
      setTime(new Date()); // ✅ Good: non-idempotent code no longer runs in render
    }, 1000);
    // 3. Return a cleanup function so we don't leak the `setInterval` timer.
    return () => clearInterval(id);
  }, []);

  return time;
}

export default function Clock() {
  const time = useTime();
  return <span>{time.toLocaleString()}</span>;
}
```

----------------------------------------

TITLE: Managing Reactive Connection with useEffect (JavaScript)
DESCRIPTION: Implements a React `useEffect` hook to manage a chat connection. The effect creates and connects the connection whenever `roomId` changes and includes a cleanup function to disconnect, ensuring the connection is reactive and synchronized with the `roomId` state.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/separating-events-from-effects.md#_snippet_9

LANGUAGE: js
CODE:
```
useEffect(() => {
  const connection = createConnection(serverUrl, roomId);
  connection.connect();
  return () => {
    connection.disconnect()
  };
}, [roomId]);
```

----------------------------------------

TITLE: Full React Application with Dynamic Chat Room Connection
DESCRIPTION: This comprehensive React application demonstrates a `ChatRoom` component that manages a chat connection using `useEffect`, with both `roomId` (prop) and `serverUrl` (state) as reactive dependencies. The `App` component allows users to dynamically change the `roomId`, showcasing how the `useEffect` re-synchronizes the connection.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/lifecycle-of-reactive-effects.md#_snippet_17

LANGUAGE: JavaScript
CODE:
```
import { useState, useEffect } from 'react';
import { createConnection } from './chat.js';

function ChatRoom({ roomId }) {
  const [serverUrl, setServerUrl] = useState('https://localhost:1234');

  useEffect(() => {
    const connection = createConnection(serverUrl, roomId);
    connection.connect();
    return () => connection.disconnect();
  }, [roomId, serverUrl]);

  return (
    <>
      <label>
        Server URL:{' '}
        <input
          value={serverUrl}
          onChange={e => setServerUrl(e.target.value)}
        />
      </label>
      <h1>Welcome to the {roomId} room!</h1>
    </>
  );
}

export default function App() {
  const [roomId, setRoomId] = useState('general');
  return (
    <>
      <label>
        Choose the chat room:{' '}
        <select
          value={roomId}
          onChange={e => setRoomId(e.target.value)}
        >
          <option value="general">general</option>
          <option value="travel">travel</option>
          <option value="music">music</option>
        </select>
      </label>
      <hr />
      <ChatRoom roomId={roomId} />
    </>
  );
}
```

----------------------------------------

TITLE: Implementing Conditional Rendering in React with && Operator
DESCRIPTION: Shows how to conditionally render JSX elements using JavaScript operators. This example specifically uses the logical AND ('&&') operator to display a checkmark emoji only if the 'isPacked' prop is true for an 'Item' component, demonstrating a common pattern for displaying content based on a boolean condition.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/describing-the-ui.md#_snippet_6

LANGUAGE: js
CODE:
```
function Item({ name, isPacked }) {
  return (
    <li className="item">
      {name} {isPacked && '✅'}
    </li>
  );
}

export default function PackingList() {
  return (
    <section>
      <h1>Sally Ride's Packing List</h1>
      <ul>
        <Item
          isPacked={true}
          name="Space suit"
        />
        <Item
          isPacked={true}
          name="Helmet with a golden leaf"
        />
        <Item
          isPacked={false}
          name="Photo of Tam"
        />
      </ul>
    </section>
  );
}
```

----------------------------------------

TITLE: Defining a Simple React Server Function
DESCRIPTION: This JavaScript file defines a simple server function named `incrementLike`. The `'use server'` directive at the top marks this file (or function, if placed inside) as server-only code that can be remotely invoked. The function increments a global counter and returns its new value.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/reference/rsc/use-server.md#_snippet_5

LANGUAGE: JavaScript
CODE:
```
// actions.js
'use server';

let likeCount = 0;
export default async function incrementLike() {
  likeCount++;
  return likeCount;
}
```

----------------------------------------

TITLE: Streaming HTML with onShellReady in React (JS)
DESCRIPTION: This example shows how to use the `onShellReady` callback within `renderToPipeableStream`. This callback fires when the entire shell of the application has been rendered, signaling that it's safe to set the content type header and begin streaming the response to the client.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/reference/react-dom/server/renderToPipeableStream.md#_snippet_15

LANGUAGE: js
CODE:
```
const { pipe } = renderToPipeableStream(<App />, {
  bootstrapScripts: ['/main.js'],
  onShellReady() {
    response.setHeader('content-type', 'text/html');
    pipe(response);
  }
});
```

----------------------------------------

TITLE: React Event Handler for Next Render with Substituted State Value (JavaScript)
DESCRIPTION: This snippet illustrates the `onClick` handler for a subsequent render where the `number` state has become `1`. It shows how the state value from *that* render (`1`) is substituted into the `setNumber` calls, explaining why clicking the button again will increment the counter from `1` to `2` for the following render, continuing the pattern of single increments per click.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/state-as-a-snapshot.md#_snippet_6

LANGUAGE: JavaScript
CODE:
```
<button onClick={() => {
  setNumber(1 + 1);
  setNumber(1 + 1);
  setNumber(1 + 1);
}}>+3</button>
```

----------------------------------------

TITLE: Managing Form State and DOM Updates in JavaScript
DESCRIPTION: This JavaScript snippet implements the core logic for a user profile form. It defines state variables (firstName, lastName, isEditing), event handlers for form submission and input changes, and setter functions that update the state and trigger a updateDOM call. The updateDOM function is responsible for conditionally showing/hiding input fields and text elements, and updating their content based on the current state, mimicking a reactive UI update.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/reacting-to-input-with-state.md#_snippet_21

LANGUAGE: javascript
CODE:
```
let firstName = 'Jane';
let lastName = 'Jacobs';
let isEditing = false;

function handleFormSubmit(e) {
  e.preventDefault();
  setIsEditing(!isEditing);
}

function handleFirstNameChange(e) {
  setFirstName(e.target.value);
}

function handleLastNameChange(e) {
  setLastName(e.target.value);
}

function setFirstName(value) {
  firstName = value;
  updateDOM();
}

function setLastName(value) {
  lastName = value;
  updateDOM();
}

function setIsEditing(value) {
  isEditing = value;
  updateDOM();
}

function updateDOM() {
  if (isEditing) {
    editButton.textContent = 'Save Profile';
    hide(firstNameText);
    hide(lastNameText);
    show(firstNameInput);
    show(lastNameInput);
  } else {
    editButton.textContent = 'Edit Profile';
    hide(firstNameInput);
    hide(lastNameInput);
    show(firstNameText);
    show(lastNameText);
  }
  firstNameText.textContent = firstName;
  lastNameText.textContent = lastName;
  helloText.textContent = (
    'Hello ' +
    firstName + ' ' +
    lastName + '!'
  );
}

function hide(el) {
  el.style.display = 'none';
}

function show(el) {
  el.style.display = '';
}

let form = document.getElementById('form');
let editButton = document.getElementById('editButton');
let firstNameInput = document.getElementById('firstNameInput');
let firstNameText = document.getElementById('firstNameText');
let lastNameInput = document.getElementById('lastNameInput');
let lastNameText = document.getElementById('lastNameText');
let helloText = document.getElementById('helloText');
form.onsubmit = handleFormSubmit;
firstNameInput.oninput = handleFirstNameChange;
lastNameInput.oninput = handleLastNameChange;
```

----------------------------------------

TITLE: Correctly Using Props Directly (Optimized)
DESCRIPTION: This snippet demonstrates the correct way to use props by accessing `messageColor` directly or assigning it to a constant `color`. This ensures that the component always uses the most up-to-date prop value passed from the parent, avoiding the synchronization issues associated with mirroring props in state.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/choosing-the-state-structure.md#_snippet_11

LANGUAGE: JavaScript
CODE:
```
function Message({ messageColor }) {
  const color = messageColor;

```

----------------------------------------

TITLE: Applying React Key Prop to Force Component State Reset
DESCRIPTION: This snippet shows how to add a `key` prop to a React component to force its re-creation and state reset when the key changes. In this chat application, associating the `Chat` component with `to.id` ensures that switching recipients clears the message input field, preventing accidental messages to the wrong person.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/preserving-and-resetting-state.md#_snippet_18

LANGUAGE: javascript
CODE:
```
<Chat key={to.id} contact={to} />
```

----------------------------------------

TITLE: Updating DOM with Minimal Changes (React/JavaScript)
DESCRIPTION: This example showcases React's efficient DOM updates during re-renders. The Clock component displays a time string and an input field. The parent App component uses a custom useTime hook to update the time every second, triggering re-renders of Clock. React intelligently updates only the <h1> element with the new time, leaving the <input> untouched to preserve its state (e.g., user-typed text), demonstrating its reconciliation algorithm.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/render-and-commit.md#_snippet_3

LANGUAGE: JavaScript
CODE:
```
export default function Clock({ time }) {
  return (
    <>
      <h1>{time}</h1>
      <input />
    </>
  );
}
```

LANGUAGE: JavaScript
CODE:
```
import { useState, useEffect } from 'react';
import Clock from './Clock.js';

function useTime() {
  const [time, setTime] = useState(() => new Date());
  useEffect(() => {
    const id = setInterval(() => {
      setTime(new Date());
    }, 1000);
    return () => clearInterval(id);
  }, []);
  return time;
}

export default function App() {
  const time = useTime();
  return (
    <Clock time={time.toLocaleTimeString()} />
  );
}
```

----------------------------------------

TITLE: Implementing a Buggy React Profile Component with Shared State
DESCRIPTION: This snippet shows the initial, buggy implementation of a React application displaying user profiles. The core issue lies in the `Profile` component writing to a global `currentPerson` variable, which `Header` and `Avatar` components then read from. This impure pattern causes all profile instances to display the same data when one is interacted with, as they all reference the same shared state.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/keeping-components-pure.md#_snippet_7

LANGUAGE: js
CODE:
```
import Panel from './Panel.js';
import { getImageUrl } from './utils.js';

let currentPerson;

export default function Profile({ person }) {
  currentPerson = person;
  return (
    <Panel>
      <Header />
      <Avatar />
    </Panel>
  )
}

function Header() {
  return <h1>{currentPerson.name}</h1>;
}

function Avatar() {
  return (
    <img
      className="avatar"
      src={getImageUrl(currentPerson)}
      alt={currentPerson.name}
      width={50}
      height={50}
    />
  );
}
```

LANGUAGE: js
CODE:
```
import { useState } from 'react';

export default function Panel({ children }) {
  const [open, setOpen] = useState(true);
  return (
    <section className="panel">
      <button onClick={() => setOpen(!open)}>
        {open ? 'Collapse' : 'Expand'}
      </button>
      {open && children}
    </section>
  );
}
```

LANGUAGE: js
CODE:
```
import Profile from './Profile.js';

export default function App() {
  return (
    <>
      <Profile person={{
        imageId: 'lrWQx8l',
        name: 'Subrahmanyan Chandrasekhar',
      }} />
      <Profile person={{
        imageId: 'MK3eW3A',
        name: 'Creola Katherine Johnson',
      }} />
    </>
  )
}
```

LANGUAGE: js
CODE:
```
export function getImageUrl(person, size = 's') {
  return (
    'https://i.imgur.com/' +
    person.imageId +
    size +
    '.jpg'
  );
}
```

LANGUAGE: css
CODE:
```
.avatar { margin: 5px; border-radius: 50%; }
.panel {
  border: 1px solid #aaa;
  border-radius: 6px;
  margin-top: 20px;
  padding: 10px;
  width: 200px;
}
h1 { margin: 5px; font-size: 18px; }
```

----------------------------------------

TITLE: Typing React Component Props with an Interface (TypeScript)
DESCRIPTION: This snippet illustrates the use of a TypeScript interface to define a structured type for a React component's props. Using an interface improves readability and reusability, especially for components with multiple or complex properties, and allows for JSDoc comments for better documentation.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/typescript.md#_snippet_2

LANGUAGE: tsx
CODE:
```
interface MyButtonProps {
  /** The text to display inside the button */
  title: string;
  /** Whether the button can be interacted with */
  disabled: boolean;
}

function MyButton({ title, disabled }: MyButtonProps) {
  return (
    <button disabled={disabled}>{title}</button>
  );
}

export default function MyApp() {
  return (
    <div>
      <h1>Welcome to my app</h1>
      <MyButton title="I'm a disabled button" disabled={true}/>
    </div>
  );
}
```

----------------------------------------

TITLE: Reading Context Value with useContext in React
DESCRIPTION: Demonstrates how to use the `useContext` Hook within a functional component to read the current value of a `ThemeContext`. This allows components deep in the tree to access data without prop drilling.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/reference/react/useContext.md#_snippet_2

LANGUAGE: js
CODE:
```
import { useContext } from 'react';

function Button() {
  const theme = useContext(ThemeContext);
  // ... 
```

----------------------------------------

TITLE: Implementing Conditional Rendering with If/Else in React Components - JavaScript
DESCRIPTION: This snippet shows the complete `PackingList` and `Item` components, integrating the `if`/`else` conditional rendering logic into the `Item` component. It demonstrates how the `Item` component displays a checkmark (✅) next to its name if the `isPacked` prop is `true`, and no checkmark otherwise. This illustrates a practical application of returning different JSX trees based on component state or props.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/conditional-rendering.md#_snippet_2

LANGUAGE: js
CODE:
```
function Item({ name, isPacked }) {
  if (isPacked) {
    return <li className="item">{name} ✅</li>;
  }
  return <li className="item">{name}</li>;
}

export default function PackingList() {
  return (
    <section>
      <h1>Sally Ride's Packing List</h1>
      <ul>
        <Item 
          isPacked={true} 
          name="Space suit" 
        />
        <Item 
          isPacked={true} 
          name="Helmet with a golden leaf" 
        />
        <Item 
          isPacked={false} 
          name="Photo of Tam" 
        />
      </ul>
    </section>
  );
}
```

----------------------------------------

TITLE: Complete ChatIndicator Component with Network Status
DESCRIPTION: A full example of a React component that displays the network online status using `useSyncExternalStore`. It combines the `useSyncExternalStore` hook with the `getSnapshot` and `subscribe` functions to reactively update the UI based on `navigator.onLine`.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/reference/react/useSyncExternalStore.md#_snippet_9

LANGUAGE: JavaScript
CODE:
```
import { useSyncExternalStore } from 'react';

export default function ChatIndicator() {
  const isOnline = useSyncExternalStore(subscribe, getSnapshot);
  return <h1>{isOnline ? '✅ Online' : '❌ Disconnected'}</h1>;
}

function getSnapshot() {
  return navigator.onLine;
}

function subscribe(callback) {
  window.addEventListener('online', callback);
  window.addEventListener('offline', callback);
  return () => {
    window.removeEventListener('online', callback);
    window.removeEventListener('offline', callback);
  };
}
```

----------------------------------------

TITLE: Complete React Component for Dynamic List Rendering
DESCRIPTION: This comprehensive React component demonstrates the full process of rendering a dynamic list. It defines a `people` array, uses `map()` to transform it into JSX `<li>` elements, and then renders these elements within a `<ul>` tag. This example highlights the common pattern for displaying collections of data in React.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/rendering-lists.md#_snippet_4

LANGUAGE: js
CODE:
```
const people = [
  'Creola Katherine Johnson: mathematician',
  'Mario José Molina-Pasquel Henríquez: chemist',
  'Mohammad Abdus Salam: physicist',
  'Percy Lavon Julian: chemist',
  'Subrahmanyan Chandrasekhar: astrophysicist'
];

export default function List() {
  const listItems = people.map(person =>
    <li>{person}</li>
  );
  return <ul>{listItems}</ul>;
}
```

----------------------------------------

TITLE: Managing Contact Form State in React
DESCRIPTION: This React component, `EditContact`, uses `useState` hooks to manage the name and email inputs. It allows users to save updated contact information via the `onSave` prop or reset the form to its `initialData`.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/preserving-and-resetting-state.md#_snippet_33

LANGUAGE: javascript
CODE:
```
import { useState } from 'react';

export default function EditContact({ initialData, onSave }) {
  const [name, setName] = useState(initialData.name);
  const [email, setEmail] = useState(initialData.email);
  return (
    <section>
      <label>
        Name:{' '}
        <input
          type="text"
          value={name}
          onChange={e => setName(e.target.value)}
        />
      </label>
      <label>
        Email:{' '}
        <input
          type="email"
          value={email}
          onChange={e => setEmail(e.target.value)}
        />
      </label>
      <button onClick={() => {
        const updatedData = {
          id: initialData.id,
          name: name,
          email: email
        };
        onSave(updatedData);
      }}>
        Save
      </button>
      <button onClick={() => {
        setName(initialData.name);
        setEmail(initialData.email);
      }}>
        Reset
      </button>
    </section>
  );
}
```

----------------------------------------

TITLE: Migrating `unmountComponentAtNode` to `root.unmount` - JavaScript
DESCRIPTION: This JavaScript snippet illustrates the migration from the deprecated `unmountComponentAtNode` to the `root.unmount()` method. With the introduction of `createRoot` and `hydrateRoot`, unmounting a component is now handled directly by the root instance, providing a more consistent API. The example shows the old and new ways to unmount a component.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/blog/2024/04/25/react-19-upgrade-guide.md#_snippet_26

LANGUAGE: JavaScript
CODE:
```
// Before
unmountComponentAtNode(document.getElementById('root'));

// After
root.unmount();
```

----------------------------------------

TITLE: Refactoring Feedback Form with Single Status State (React)
DESCRIPTION: This React component refactors the feedback form to use a single `status` state variable instead of multiple booleans, preventing contradictory states. The `status` can be `'typing'`, `'sending'`, or `'sent'`. The `handleSubmit` function updates the `status` through these phases. Derived constants `isSending` and `isSent` are computed from `status` for readability, ensuring they always reflect a valid state. This approach improves state management robustness.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/choosing-the-state-structure.md#_snippet_5

LANGUAGE: JavaScript
CODE:
```
import { useState } from 'react';

export default function FeedbackForm() {
  const [text, setText] = useState('');
  const [status, setStatus] = useState('typing');

  async function handleSubmit(e) {
    e.preventDefault();
    setStatus('sending');
    await sendMessage(text);
    setStatus('sent');
  }

  const isSending = status === 'sending';
  const isSent = status === 'sent';

  if (isSent) {
    return <h1>Thanks for feedback!</h1>
  }

  return (
    <form onSubmit={handleSubmit}>
      <p>How was your stay at The Prancing Pony?</p>
      <textarea
        disabled={isSending}
        value={text}
        onChange={e => setText(e.target.value)}
      />
      <br />
      <button
        disabled={isSending}
        type="submit"
      >
        Send
      </button>
      {isSending && <p>Sending...</p>}
    </form>
  );
}

// Pretend to send a message.
function sendMessage(text) {
  return new Promise(resolve => {
    setTimeout(resolve, 2000);
  });
}
```

----------------------------------------

TITLE: Illustrating State Snapshot with Substitution (JavaScript)
DESCRIPTION: This code snippet provides a mental model for understanding why the previous example's alert shows '0'. It demonstrates the 'substitution method' where the number variable is replaced with its value at the time the event handler was created, clarifying that the alert operates on the 'snapshot' of the state.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/state-as-a-snapshot.md#_snippet_8

LANGUAGE: javascript
CODE:
```
setNumber(0 + 5);
alert(0);
```

----------------------------------------

TITLE: Resolving Promise in Client Component using React `use` API
DESCRIPTION: This Client Component (`Message`) receives a Promise as a prop (`messagePromise`) from a Server Component. It uses the `use` API from React to read the resolved value of the Promise, allowing the component to render its content only after the data is available. The `'use client'` directive marks it as a Client Component.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/reference/react/use.md#_snippet_8

LANGUAGE: JavaScript
CODE:
```
// message.js
'use client';

import { use } = 'react';

export function Message({ messagePromise }) {
  const messageContent = use(messagePromise);
  return <p>Here is the message: {messageContent}</p>;
}
```

----------------------------------------

TITLE: Clearing Images During Loading in React Gallery
DESCRIPTION: This enhanced `Gallery` React component addresses the image loading behavior by applying a `key` prop to the `<img>` tag. By setting `key={image.src}`, React is forced to re-create the DOM node when the image source changes, effectively clearing the previous image during the loading of the new one.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/preserving-and-resetting-state.md#_snippet_36

LANGUAGE: javascript
CODE:
```
import { useState } from 'react';

export default function Gallery() {
  const [index, setIndex] = useState(0);
  const hasNext = index < images.length - 1;

  function handleClick() {
    if (hasNext) {
      setIndex(index + 1);
    } else {
      setIndex(0);
    }
  }

  let image = images[index];
  return (
    <>
      <button onClick={handleClick}>
        Next
      </button>
      <h3>
        Image {index + 1} of {images.length}
      </h3>
      <img key={image.src} src={image.src} />
      <p>
        {image.place}
      </p>
    </>
  );
}

let images = [{
  place: 'Penang, Malaysia',
  src: 'https://i.imgur.com/FJeJR8M.jpg'
}, {
  place: 'Lisbon, Portugal',
  src: 'https://i.imgur.com/dB2LRbj.jpg'
}, {
  place: 'Bilbao, Spain',
  src: 'https://i.imgur.com/z08o2TS.jpg'
}, {
  place: 'Valparaíso, Chile',
  src: 'https://i.imgur.com/Y3utgTi.jpg'
}, {
  place: 'Schwyz, Switzerland',
  src: 'https://i.imgur.com/JBbMpWY.jpg'
}, {
  place: 'Prague, Czechia',
  src: 'https://i.imgur.com/QwUKKmF.jpg'
}, {
  place: 'Ljubljana, Slovenia',
  src: 'https://i.imgur.com/3aIiwfm.jpg'
}];
```

----------------------------------------

TITLE: Creating Pure React Components with Props
DESCRIPTION: This React component `Recipe` is a pure function that takes `drinkers` as props and consistently returns the same JSX output based on the input. The `App` component demonstrates its usage with different `drinkers` values, highlighting the predictability of pure components.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/keeping-components-pure.md#_snippet_1

LANGUAGE: javascript
CODE:
```
function Recipe({ drinkers }) {
  return (
    <ol>    
      <li>Boil {drinkers} cups of water.</li>
      <li>Add {drinkers} spoons of tea and {0.5 * drinkers} spoons of spice.</li>
      <li>Add {0.5 * drinkers} cups of milk to boil and sugar to taste.</li>
    </ol>
  );
}

export default function App() {
  return (
    <section>
      <h1>Spiced Chai Recipe</h1>
      <h2>For two</h2>
      <Recipe drinkers={2} />
      <h2>For a gathering</h2>
      <Recipe drinkers={4} />
    </section>
  );
}
```

----------------------------------------

TITLE: Illustrating Delayed State Snapshot with Substitution (JavaScript)
DESCRIPTION: This snippet provides the mental substitution for the delayed alert scenario. It clarifies that even when setTimeout is used, the number variable inside the callback retains the value it had at the time the onClick handler was executed, reinforcing the concept of state snapshots for asynchronous operations.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/state-as-a-snapshot.md#_snippet_10

LANGUAGE: javascript
CODE:
```
setNumber(0 + 5);
setTimeout(() => {
  alert(0);
}, 3000);
```

----------------------------------------

TITLE: Declaring an Effect Event with useEffectEvent
DESCRIPTION: Demonstrates how to declare an Effect Event using the experimental `useEffectEvent` Hook. This extracts non-reactive logic, like showing a notification, from the main Effect body, allowing it to always see the latest prop and state values without causing the Effect to re-run.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/separating-events-from-effects.md#_snippet_14

LANGUAGE: js
CODE:
```
import { useEffect, useEffectEvent } from 'react';

function ChatRoom({ roomId, theme }) {
  const onConnected = useEffectEvent(() => {
    showNotification('Connected!', theme);
  });
  // ...
```

----------------------------------------

TITLE: Incrementing State Multiple Times with Updater Functions in React
DESCRIPTION: This React component demonstrates how to increment a state variable multiple times within a single event handler using updater functions. Each call to `setNumber(n => n + 1)` queues an update that depends on the previous state, ensuring the final value reflects all increments.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/queueing-a-series-of-state-updates.md#_snippet_3

LANGUAGE: javascript
CODE:
```
import { useState } from 'react';

export default function Counter() {
  const [number, setNumber] = useState(0);

  return (
    <>
      <h1>{number}</h1>
      <button onClick={() => {
        setNumber(n => n + 1);
        setNumber(n => n + 1);
        setNumber(n => n + 1);
      }}>+3</button>
    </>
  )
}
```

LANGUAGE: css
CODE:
```
button { display: inline-block; margin: 10px; font-size: 20px; }
h1 { display: inline-block; margin: 10px; width: 30px; text-align: center; }
```

----------------------------------------

TITLE: Correctly Handling Form Submission Side Effect in Event Handler
DESCRIPTION: Presents the recommended approach: placing the form submission side effects (POST request, notification) directly within the 'handleSubmit' event handler. This ensures the logic runs only when the form is submitted, avoiding issues caused by reactive dependencies changing.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/removing-effect-dependencies.md#_snippet_13

LANGUAGE: javascript
CODE:
```
function Form() {
  const theme = useContext(ThemeContext);

  function handleSubmit() {
    // ✅ Good: Event-specific logic is called from event handlers
    post('/api/register');
    showNotification('Successfully registered!', theme);
  }

  // ...
}
```

----------------------------------------

TITLE: Updating React Board Component to Receive Props
DESCRIPTION: This snippet modifies the `Board` component to accept `xIsNext`, `squares`, and `onPlay` as props. This change makes the `Board` component fully controlled by its parent `Game` component, removing its internal state management for `squares` and `xIsNext`.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/tutorial-tic-tac-toe.md#_snippet_62

LANGUAGE: JavaScript
CODE:
```
function Board({ xIsNext, squares, onPlay }) {
  function handleClick(i) {
    //...
  }
  // ...
}
```

----------------------------------------

TITLE: Initializing Primitive State in React
DESCRIPTION: Demonstrates how to initialize a primitive (number) state variable `x` with an initial value of `0` using the `useState` Hook in React.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/updating-objects-in-state.md#_snippet_0

LANGUAGE: js
CODE:
```
const [x, setX] = useState(0);
```

----------------------------------------

TITLE: Implementing Counter with useRef (No Re-render) in React
DESCRIPTION: This snippet attempts to implement a counter using the `useRef` hook. While `countRef.current` is successfully incremented, modifying a ref's `current` value does not trigger a component re-render. Consequently, the displayed count on the button will not update, illustrating why refs are not suitable for values that need to be reflected in the UI.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/referencing-values-with-refs.md#_snippet_8

LANGUAGE: JavaScript
CODE:
```
import { useRef } from 'react';

export default function Counter() {
  let countRef = useRef(0);

  function handleClick() {
    // This doesn't re-render the component!
    countRef.current = countRef.current + 1;
  }

  return (
    <button onClick={handleClick}>
      You clicked {countRef.current} times
    </button>
  );
}
```

----------------------------------------

TITLE: Preloading User Data with React `cache` in JSX
DESCRIPTION: This snippet demonstrates how to preload user data using React's `cache` API. The `getUser` function is memoized, allowing the `Page` component to initiate the data fetch early without awaiting the result. Later, the `Profile` component can call `getUser` again, and if the data is already cached, it will be retrieved instantly, reducing latency. This pattern optimizes performance by overlapping data fetching with rendering.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/reference/react/cache.md#_snippet_10

LANGUAGE: JSX
CODE:
```
const getUser = cache(async (id) => {
  return await db.user.query(id);
});

async function Profile({id}) {
  const user = await getUser(id);
  return (
    <section>
      <img src={user.profilePic} />
      <h2>{user.name}</h2>
    </section>
  );
}

function Page({id}) {
  // ✅ Good: start fetching the user data
  getUser(id);
  // ... some computational work
  return (
    <>
      <Profile id={id} />
    </>
  );
}
```

----------------------------------------

TITLE: Focusing an Input Element Using useRef in React
DESCRIPTION: This complete example demonstrates how to use `useRef` to get a reference to an `<input>` DOM node and then imperatively call its `focus()` method. A button click triggers the `handleClick` function, which accesses `inputRef.current` to focus the input field.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/reference/react-dom/components/common.md#_snippet_31

LANGUAGE: js
CODE:
```
import { useRef } from 'react';

export default function Form() {
  const inputRef = useRef(null);

  function handleClick() {
    inputRef.current.focus();
  }

  return (
    <>
      <input ref={inputRef} />
      <button onClick={handleClick}>
        Focus the input
      </button>
    </>
  );
}
```

----------------------------------------

TITLE: Focusing a DOM Element with useRef in React
DESCRIPTION: This React component demonstrates how to directly manipulate a DOM element using a ref. An `inputRef` is created with `useRef` and attached to an `<input>` element. When the button is clicked, the `handleClick` function accesses the underlying DOM node via `inputRef.current` and calls its `focus()` method, providing direct control over the DOM element.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/escape-hatches.md#_snippet_2

LANGUAGE: javascript
CODE:
```
import { useRef } from 'react';

export default function Form() {
  const inputRef = useRef(null);

  function handleClick() {
    inputRef.current.focus();
  }

  return (
    <>
      <input ref={inputRef} />
      <button onClick={handleClick}>
        Focus the input
      </button>
    </>
  );
}
```

----------------------------------------

TITLE: Representing Object References in JavaScript
DESCRIPTION: This snippet illustrates how seemingly 'nested' objects are actually separate objects referencing each other. It defines `obj1` as a standalone object containing artwork details and `obj2` as another object that includes a reference to `obj1` via its `artwork` property. This clarifies that `obj1` is not physically 'inside' `obj2`, but rather `obj2` points to `obj1`.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/updating-objects-in-state.md#_snippet_26

LANGUAGE: JavaScript
CODE:
```
let obj1 = {
  title: 'Blue Nana',
  city: 'Hamburg',
  image: 'https://i.imgur.com/Sd1AgUOm.jpg',
};

let obj2 = {
  name: 'Niki de Saint Phalle',
  artwork: obj1
};
```

----------------------------------------

TITLE: Cleaning Up Old Chat Connection on Re-render - JavaScript
DESCRIPTION: When `roomId` changes, React calls the cleanup function of the previous `useEffect` run. This snippet illustrates the disconnection from the old 'general' chat room, ensuring resources are released before a new connection is made.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/lifecycle-of-reactive-effects.md#_snippet_6

LANGUAGE: JavaScript
CODE:
```
function ChatRoom({ roomId /* "general" */ }) {
  useEffect(() => {
    const connection = createConnection(serverUrl, roomId); // Connects to the "general" room
    connection.connect();
    return () => {
      connection.disconnect(); // Disconnects from the "general" room
    };
    // ...
```

----------------------------------------

TITLE: Preventing UI Fallbacks with React startTransition
DESCRIPTION: This JavaScript function `handleNextPageClick` demonstrates using `startTransition` to mark a state update as non-urgent. By wrapping `setCurrentPage` in `startTransition`, React will prevent already displayed content from being hidden by a Suspense fallback if the update causes a component to suspend, improving user experience.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/reference/react/Suspense.md#_snippet_51

LANGUAGE: javascript
CODE:
```
function handleNextPageClick() {
  // If this update suspends, don't hide the already displayed content
  startTransition(() => {
    setCurrentPage(currentPage + 1);
  });
}
```

----------------------------------------

TITLE: Demonstrating State Update Batching in React
DESCRIPTION: This React component illustrates how state updates are batched. Despite calling `setNumber(number + 1)` three times within a single `onClick` handler, React batches these updates, resulting in only one re-render and the `number` state being incremented by one (from its initial snapshot value) rather than three, due to the fixed `number` value within that render's scope.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/queueing-a-series-of-state-updates.md#_snippet_0

LANGUAGE: js
CODE:
```
import { useState } from 'react';

export default function Counter() {
  const [number, setNumber] = useState(0);

  return (
    <>
      <h1>{number}</h1>
      <button onClick={() => {
        setNumber(number + 1);
        setNumber(number + 1);
        setNumber(number + 1);
      }}>+3</button>
    </>
  )
}
```

----------------------------------------

TITLE: Correct Immutable State Update in a Reducer
DESCRIPTION: This snippet demonstrates the correct way to update state within a reducer function by ensuring immutability. Instead of mutating the original state object, it returns a *new* state object, typically by spreading the previous state and overriding specific properties, like `age`.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/reference/react/useReducer.md#_snippet_11

LANGUAGE: javascript
CODE:
```
function reducer(state, action) {
  switch (action.type) {
    case 'incremented_age': {
      // ✅ Instead, return a new object
      return {
        ...state,
        age: state.age + 1
      };
    }

```

----------------------------------------

TITLE: Implementing Task Management with Immer Reducer in React
DESCRIPTION: This snippet defines the main application component (`TaskApp`) using `useImmerReducer` to manage a list of tasks. It includes the `tasksReducer` function, which handles 'added', 'changed', and 'deleted' actions by directly 'mutating' the `draft` state provided by Immer. Event handlers (`handleAddTask`, `handleChangeTask`, `handleDeleteTask`) dispatch actions to update the task list. Dependencies include `useImmerReducer`, `AddTask`, and `TaskList` components.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/extracting-state-logic-into-a-reducer.md#_snippet_25

LANGUAGE: JavaScript
CODE:
```
import { useImmerReducer } from 'use-immer';
import AddTask from './AddTask.js';
import TaskList from './TaskList.js';

function tasksReducer(draft, action) {
  switch (action.type) {
    case 'added': {
      draft.push({
        id: action.id,
        text: action.text,
        done: false,
      });
      break;
    }
    case 'changed': {
      const index = draft.findIndex((t) => t.id === action.task.id);
      draft[index] = action.task;
      break;
    }
    case 'deleted': {
      return draft.filter((t) => t.id !== action.id);
    }
    default: {
      throw Error('Unknown action: ' + action.type);
    }
  }
}

export default function TaskApp() {
  const [tasks, dispatch] = useImmerReducer(tasksReducer, initialTasks);

  function handleAddTask(text) {
    dispatch({
      type: 'added',
      id: nextId++,
      text: text,
    });
  }

  function handleChangeTask(task) {
    dispatch({
      type: 'changed',
      task: task,
    });
  }

  function handleDeleteTask(taskId) {
    dispatch({
      type: 'deleted',
      id: taskId,
    });
  }

  return (
    <>
      <h1>Prague itinerary</h1>
      <AddTask onAddTask={handleAddTask} />
      <TaskList
        tasks={tasks}
        onChangeTask={handleChangeTask}
        onDeleteTask={handleDeleteTask}
      />
    </>
  );
}

let nextId = 3;
const initialTasks = [
  {id: 0, text: 'Visit Kafka Museum', done: true},
  {id: 1, text: 'Watch a puppet show', done: false},
  {id: 2, text: 'Lennon Wall pic', done: false},
];
```

----------------------------------------

TITLE: Calculating Derived State from Props (Best Practice)
DESCRIPTION: This snippet illustrates the most recommended approach for managing state related to props: calculating derived state (`selection`) directly from props (`items`) and a minimal piece of state (`selectedId`). This eliminates the need for explicit state adjustments, simplifying data flow and ensuring the UI always reflects the current props without extra re-renders or complex logic.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/you-might-not-need-an-effect.md#_snippet_12

LANGUAGE: JavaScript
CODE:
```
function List({ items }) {
  const [isReverse, setIsReverse] = useState(false);
  const [selectedId, setSelectedId] = useState(null);
  // ✅ Best: Calculate everything during rendering
  const selection = items.find(item => item.id === selectedId) ?? null;
  // ...
}
```

----------------------------------------

TITLE: Splitting and Rendering Lists by Profession - React Solution
DESCRIPTION: This React component demonstrates how to split a single list of people into two distinct lists: 'Chemists' and 'Everyone Else'. It uses the `filter()` method twice on the `people` array to create separate arrays for each category, then maps over each filtered array to render the respective list items. This provides a solution to the problem of categorizing and displaying data.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/rendering-lists.md#_snippet_26

LANGUAGE: JavaScript
CODE:
```
import { people } from './data.js';
import { getImageUrl } from './utils.js';

export default function List() {
  const chemists = people.filter(person =>
    person.profession === 'chemist'
  );
  const everyoneElse = people.filter(person =>
    person.profession !== 'chemist'
  );
  return (
    <article>
      <h1>Scientists</h1>
      <h2>Chemists</h2>
      <ul>
        {chemists.map(person =>
          <li key={person.id}>
            <img
              src={getImageUrl(person)}
              alt={person.name}
            />
            <p>
              <b>{person.name}:</b>
              {' ' + person.profession + ' '}
              known for {person.accomplishment}
            </p>
          </li>
        )}
      </ul>
      <h2>Everyone Else</h2>
      <ul>
        {everyoneElse.map(person =>
          <li key={person.id}>
            <img
              src={getImageUrl(person)}
              alt={person.name}
            />
            <p>
              <b>{person.name}:</b>
              {' ' + person.profession + ' '}
              known for {person.accomplishment}
            </p>
          </li>
        )}
      </ul>
    </article>
  );
}
```

----------------------------------------

TITLE: Importing and Consuming ThemeContext in a React Component
DESCRIPTION: This example shows how a `Button` component imports `ThemeContext` from a shared `Contexts.js` file. It then uses the `useContext` hook to read the current theme value, enabling consistent styling or behavior based on the application's theme.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/reference/react/createContext.md#_snippet_9

LANGUAGE: javascript
CODE:
```
// Button.js
import { ThemeContext } from './Contexts.js';

function Button() {
  const theme = useContext(ThemeContext);
  // ...
}
```

----------------------------------------

TITLE: Implementing Symmetrical useEffect Cleanup for Connections in React
DESCRIPTION: This example illustrates the correct pattern for `useEffect` where cleanup logic is symmetrical to the setup. It establishes a connection on mount (or when `serverUrl` or `roomId` change) and disconnects it during cleanup, ensuring resources are properly managed. This pattern prevents resource leaks and maintains predictable behavior.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/reference/react/useEffect.md#_snippet_56

LANGUAGE: JavaScript
CODE:
```
  useEffect(() => {
    const connection = createConnection(serverUrl, roomId);
    connection.connect();
    return () => {
      connection.disconnect();
    };
  }, [serverUrl, roomId]);
```

----------------------------------------

TITLE: Basic `useFormStatus` Hook Destructuring - JavaScript
DESCRIPTION: This snippet demonstrates the basic destructuring of the `useFormStatus` Hook to access its returned properties: `pending`, `data`, `method`, and `action`. This hook provides real-time status information about the last form submission within a React application.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/reference/react-dom/hooks/useFormStatus.md#_snippet_0

LANGUAGE: JavaScript
CODE:
```
const { pending, data, method, action } = useFormStatus();
```

----------------------------------------

TITLE: Avoiding Delayed Parent Notification with React useEffect
DESCRIPTION: This snippet illustrates an anti-pattern where a child component notifies its parent about state changes via an `onChange` prop inside a `useEffect`. This causes a delay: the child updates its state and renders, then the effect runs, calling `onChange`, which triggers another state update and render in the parent. This results in two render passes instead of one.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/you-might-not-need-an-effect.md#_snippet_22

LANGUAGE: javascript
CODE:
```
function Toggle({ onChange }) {
  const [isOn, setIsOn] = useState(false);

  // 🔴 Avoid: The onChange handler runs too late
  useEffect(() => {
    onChange(isOn);
  }, [isOn, onChange])

  function handleClick() {
    setIsOn(!isOn);
  }

  function handleDragEnd(e) {
    if (isCloserToRightEdge(e)) {
      setIsOn(true);
    } else {
      setIsOn(false);
    }
  }

  // ...
}
```

----------------------------------------

TITLE: Correctly Passing Event Handler Reference in React JavaScript
DESCRIPTION: This React JavaScript snippet demonstrates the correct way to pass an event handler by providing a reference to the function (`handleClick`). This prevents the handler from being called during the render phase, thereby avoiding infinite re-render loops and ensuring the handler is only executed when the button is clicked.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/reference/react/useReducer.md#_snippet_31

LANGUAGE: js
CODE:
```
// ✅ Correct: passes down the event handler
return <button onClick={handleClick}>Click me</button>
```

----------------------------------------

TITLE: Mutating an Array in JavaScript
DESCRIPTION: This snippet demonstrates how to directly mutate an array by assigning a new value to an existing index. This approach modifies the original array in place, which is generally discouraged in React's state management due to potential side effects and difficulties in tracking changes.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/tutorial-tic-tac-toe.md#_snippet_44

LANGUAGE: jsx
CODE:
```
const squares = [null, null, null, null, null, null, null, null, null];
squares[0] = 'X';
// Now `squares` is ["X", null, null, null, null, null, null, null, null];
```

----------------------------------------

TITLE: Fixing Impure Component by Cloning Array in React
DESCRIPTION: This snippet demonstrates how to make a React component pure by cloning the `stories` array using `slice()` before modifying it. By pushing new items into a copy, the original `stories` prop remains unchanged, preventing unintended side effects and ensuring predictable rendering behavior.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/reference/react/StrictMode.md#_snippet_9

LANGUAGE: javascript
CODE:
```
export default function StoryTray({ stories }) {
  const items = stories.slice(); // Clone the array
  // ✅ Good: Pushing into a new array
  items.push({ id: 'create', label: 'Create Story' });
```

----------------------------------------

TITLE: Connecting to Chat with useEffectEvent for Non-Reactive Logic - React JavaScript
DESCRIPTION: This snippet refactors the previous `useEffect` to use the experimental `experimental_useEffectEvent` (aliased as `useEffectEvent`). This allows the `onConnected` callback, which reads the `theme` prop, to be non-reactive to `theme` changes. The `useEffect` now only re-runs when `roomId` changes, preventing unnecessary chat re-connections when the theme is toggled.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/escape-hatches.md#_snippet_11

LANGUAGE: JSON
CODE:
```
{
  "dependencies": {
    "react": "experimental",
    "react-dom": "experimental",
    "react-scripts": "latest",
    "toastify-js": "1.12.0"
  },
  "scripts": {
    "start": "react-scripts start",
    "build": "react-scripts build",
    "test": "react-scripts test --env=jsdom",
    "eject": "react-scripts eject"
  }
}
```

LANGUAGE: JavaScript
CODE:
```
import { useState, useEffect } from 'react';
import { experimental_useEffectEvent as useEffectEvent } from 'react';
import { createConnection, sendMessage } from './chat.js';
import { showNotification } from './notifications.js';

const serverUrl = 'https://localhost:1234';

function ChatRoom({ roomId, theme }) {
  const onConnected = useEffectEvent(() => {
    showNotification('Connected!', theme);
  });

  useEffect(() => {
    const connection = createConnection(serverUrl, roomId);
    connection.on('connected', () => {
      onConnected();
    });
    connection.connect();
    return () => connection.disconnect();
  }, [roomId]);

  return <h1>Welcome to the {roomId} room!</h1>
}

export default function App() {
  const [roomId, setRoomId] = useState('general');
  const [isDark, setIsDark] = useState(false);
```

----------------------------------------

TITLE: Solution: Preventing Unnecessary Re-renders with useCallback and memo in React
DESCRIPTION: This snippet shows the correct way to use `useCallback` to prevent unnecessary re-renders of a `memo`-wrapped child component. By wrapping `handleSubmit` in `useCallback`, the function instance remains stable across re-renders (as long as dependencies don't change), allowing `ShippingForm` to effectively skip re-rendering.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/reference/react/useCallback.md#_snippet_6

LANGUAGE: JavaScript
CODE:
```
function ProductPage({ productId, referrer, theme }) {
  // Tell React to cache your function between re-renders...
  const handleSubmit = useCallback((orderDetails) => {
    post('/product/' + productId + '/buy', {
      referrer,
      orderDetails,
    });
  }, [productId, referrer]); // ...so as long as these dependencies don't change...

  return (
    <div className={theme}>
      {/* ...ShippingForm will receive the same props and can skip re-rendering */}
      <ShippingForm onSubmit={handleSubmit} />
    </div>
  );
}
```

----------------------------------------

TITLE: Embedding JavaScript Variables as Text in React JSX
DESCRIPTION: This snippet demonstrates embedding a JavaScript variable directly into the text content of a JSX tag using curly braces. The `name` variable's value is rendered inside an `<h1>` element, allowing for dynamic text generation. This technique is useful for displaying variable data within the UI.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/javascript-in-jsx-with-curly-braces.md#_snippet_2

LANGUAGE: JavaScript
CODE:
```
export default function TodoList() {
  const name = 'Gregorio Y. Zara';
  return (
    <h1>{name}'s To Do List</h1>
  );
}
```

----------------------------------------

TITLE: Scheduling and Cleaning Up Timers with React useEffect
DESCRIPTION: This React component demonstrates the `useEffect` hook for scheduling a `setTimeout` and its corresponding cleanup using `clearTimeout`. It illustrates how effects are scheduled and cleaned up on component mount, unmount, and dependency changes, highlighting React's development mode remounting behavior and the capture of state by effects.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/synchronizing-with-effects.md#_snippet_30

LANGUAGE: JavaScript
CODE:
```
import { useState, useEffect } from 'react';

function Playground() {
  const [text, setText] = useState('a');

  useEffect(() => {
    function onTimeout() {
      console.log('⏰ ' + text);
    }

    console.log('🔵 Schedule \"' + text + '\" log');
    const timeoutId = setTimeout(onTimeout, 3000);

    return () => {
      console.log('🟡 Cancel \"' + text + '\" log');
      clearTimeout(timeoutId);
    };
  }, [text]);

  return (
    <>
      <label>
        What to log:{' '}
        <input
          value={text}
          onChange={e => setText(e.target.value)}
        />
      </label>
      <h1>{text}</h1>
    </>
  );
}

export default function App() {
  const [show, setShow] = useState(false);
  return (
    <>
      <button onClick={() => setShow(!show)}>
        {show ? 'Unmount' : 'Mount'} the component
      </button>
      {show && <hr />}
      {show && <Playground />}
    </>
  );
}
```

----------------------------------------

TITLE: Importing useState and Initializing Component State (JavaScript)
DESCRIPTION: This snippet demonstrates how to import the `useState` hook from React and use it within the `Square` component to manage its internal state. It initializes a state variable `value` to `null` and provides a `setValue` function to update it, replacing the previous `value` prop.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/tutorial-tic-tac-toe.md#_snippet_22

LANGUAGE: js
CODE:
```
import { useState } from 'react';

function Square() {
  const [value, setValue] = useState(null);

  function handleClick() {
    //...

```

----------------------------------------

TITLE: Rendering List Items with Array map() in React (JSX)
DESCRIPTION: Demonstrates how to transform an array of product objects into an array of `<li>` JSX elements using the `map()` function. Each `<li>` element is assigned a unique `key` attribute, which is crucial for React's reconciliation process.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/index.md#_snippet_14

LANGUAGE: js
CODE:
```
const listItems = products.map(product =>
  <li key={product.id}>
    {product.title}
  </li>
);

return (
  <ul>{listItems}</ul>
);
```

----------------------------------------

TITLE: Managing Chat Connection with React useEffect Dependencies - JavaScript
DESCRIPTION: This React useEffect hook manages a chat room connection, establishing it when the component mounts or roomId changes, and disconnecting on unmount or roomId change. By including roomId in the dependency array, React ensures the effect re-synchronizes whenever the roomId prop updates, maintaining a connection specific to the current room.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/lifecycle-of-reactive-effects.md#_snippet_12

LANGUAGE: js
CODE:
```
function ChatRoom({ roomId }) { 
  useEffect(() => {
    const connection = createConnection(serverUrl, roomId); 
    connection.connect();
    return () => {
      connection.disconnect();
    };
  }, [roomId]); 
  // ...
```

----------------------------------------

TITLE: Using Server Function Directly as Form Action (JS)
DESCRIPTION: A client component `UpdateName` that passes the `updateName` server function directly to the `<form action={...}>` prop. This allows React to handle the form submission and server function call automatically.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/reference/rsc/server-functions.md#_snippet_6

LANGUAGE: js
CODE:
```
"use client";

import {updateName} from './actions';

function UpdateName() {
  return (
    <form action={updateName}>
      <input type="text" name="name" />
    </form>
  )
}
```

----------------------------------------

TITLE: Declaring State Variable with useState in React
DESCRIPTION: This code shows how to declare a state variable within a functional React component using the `useState` Hook. It returns an array containing the current state value (`count`) and a function to update it (`setCount`), initialized with a default value of `0`.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/index.md#_snippet_18

LANGUAGE: javascript
CODE:
```
function MyButton() {
  const [count, setCount] = useState(0);
  // ...
```

----------------------------------------

TITLE: Embedding JavaScript Variables in JSX Text - JavaScript
DESCRIPTION: This example shows how to embed JavaScript variables directly into the text content of JSX. By enclosing a JavaScript expression or variable within curly braces (`{}`), its value is rendered as part of the UI.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/index.md#_snippet_6

LANGUAGE: JavaScript
CODE:
```
return (
  <h1>
    {user.name}
  </h1>
);
```

----------------------------------------

TITLE: Optimizing Effect Dependencies by Moving Object Inside (JavaScript)
DESCRIPTION: This React JavaScript snippet shows the most optimized approach for managing effect dependencies by moving the 'options' object creation directly inside the 'useEffect' hook. This eliminates the need for 'useMemo' and ensures that 'options' is not a dependency, as it's created within the effect's scope, making 'roomId' the sole dependency for re-firing.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/reference/react/useMemo.md#_snippet_30

LANGUAGE: javascript
CODE:
```
function ChatRoom({ roomId }) {
  const [message, setMessage] = useState('');

  useEffect(() => {
    const options = { // ✅ No need for useMemo or object dependencies!
      serverUrl: 'https://localhost:1234',
      roomId: roomId
    }
    
    const connection = createConnection(options);
    connection.connect();
    return () => connection.disconnect();
  }, [roomId]); // ✅ Only changes when roomId changes
  // ...
```

----------------------------------------

TITLE: Fetching Data with useEffect and Cleanup in React
DESCRIPTION: This snippet demonstrates how to fetch data using the `useEffect` hook in React, incorporating a cleanup mechanism to prevent race conditions. It uses an `ignore` flag to ensure that only the most recent fetch's result updates the component's state, especially when dependencies like `userId` change rapidly.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/synchronizing-with-effects.md#_snippet_24

LANGUAGE: js
CODE:
```
useEffect(() => {
  let ignore = false;

  async function startFetching() {
    const json = await fetchTodos(userId);
    if (!ignore) {
      setTodos(json);
    }
  }

  startFetching();

  return () => {
    ignore = true;
  };
}, [userId]);
```

----------------------------------------

TITLE: Mapping Array Data to JSX List Items in JavaScript
DESCRIPTION: This snippet demonstrates using the `map()` array method to transform the `people` array into a new array of JSX `<li>` elements. Each `<li>` element contains the corresponding `person` string, preparing the data for rendering as a list in React.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/rendering-lists.md#_snippet_2

LANGUAGE: js
CODE:
```
const listItems = people.map(person => <li>{person}</li>);
```

----------------------------------------

TITLE: Optimizing React Effect by Declaring Function Inside Effect Hook
DESCRIPTION: This corrected React component demonstrates how to prevent unnecessary re-runs of `useEffect` by declaring the `createOptions` function directly inside the effect. This ensures the effect only re-runs when `roomId` changes, as the function itself is no longer a changing dependency.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/reference/react/useEffect.md#_snippet_46

LANGUAGE: javascript
CODE:
```
import { useState, useEffect } from 'react';
import { createConnection } from './chat.js';

const serverUrl = 'https://localhost:1234';

function ChatRoom({ roomId }) {
  const [message, setMessage] = useState('');

  useEffect(() => {
    function createOptions() {
      return {
        serverUrl: serverUrl,
        roomId: roomId
      };
    }

    const options = createOptions();
    const connection = createConnection(options);
    connection.connect();
    return () => connection.disconnect();
  }, [roomId]);

  return (
    <>
      <h1>Welcome to the {roomId} room!</h1>
      <input value={message} onChange={e => setMessage(e.target.value)} />
    </>
  );
}

export default function App() {
  const [roomId, setRoomId] = useState('general');
  return (
    <>
      <label>
        Choose the chat room:{' '}
        <select
          value={roomId}
          onChange={e => setSetRoomId(e.target.value)}
        >
          <option value="general">general</option>
          <option value="travel">travel</option>
          <option value="music">music</option>
        </select>
      </label>
      <hr />
      <ChatRoom roomId={roomId} />
    </>
  );
}
```

----------------------------------------

TITLE: Passing Data from Child to Parent in React using Effect (Avoid)
DESCRIPTION: This snippet demonstrates an anti-pattern where a child component fetches data and passes it up to its parent component using a `useEffect` hook. This approach makes data flow difficult to trace and is generally discouraged in React.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/you-might-not-need-an-effect.md#_snippet_25

LANGUAGE: JavaScript
CODE:
```
function Parent() {
  const [data, setData] = useState(null);
  // ...
  return <Child onFetched={setData} />;
}

function Child({ onFetched }) {
  const data = useSomeAPI();
  // 🔴 Avoid: Passing data to the parent in an Effect
  useEffect(() => {
    if (data) {
      onFetched(data);
    }
  }, [onFetched, data]);
  // ...
}
```

----------------------------------------

TITLE: Providing React Context Value
DESCRIPTION: This snippet illustrates how to provide a value to a React Context. By wrapping child components with `<MyContext.Provider value={...}>`, all components within its subtree that consume `MyContext` will receive the specified `value`. This mechanism allows dynamic updates to the context value, triggering re-renders in consuming components.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/passing-data-deeply-with-context.md#_snippet_42

LANGUAGE: JSX
CODE:
```
<MyContext value={...}>
```

----------------------------------------

TITLE: Wrapping Multiple JSX Elements with React Fragments (JavaScript)
DESCRIPTION: This snippet demonstrates the correct way to return multiple adjacent JSX elements from a React component by wrapping them in a React Fragment (`<>...</>`). Fragments allow grouping a list of children without adding extra nodes to the DOM, resolving the 'Adjacent JSX elements must be wrapped' error.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/tutorial-tic-tac-toe.md#_snippet_8

LANGUAGE: js
CODE:
```
export default function Square() {
  return (
    <>
      <button className="square">X</button>
      <button className="square">X</button>
    </>
  );
}
```

----------------------------------------

TITLE: Solution Search Input Component (Ref Forwarding) - React JS
DESCRIPTION: This `SearchInput` component is modified to accept a `ref` prop and forward it to the native HTML `<input>` element. This enables parent components to gain direct access to the DOM node of the input field for imperative actions like focusing, without violating component encapsulation.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/manipulating-the-dom-with-refs.md#_snippet_39

LANGUAGE: javascript
CODE:
```
export default function SearchInput({ ref }) {
  return (
    <input
      ref={ref}
      placeholder="Looking for something?"
    />
  );
}
```

----------------------------------------

TITLE: Using Server Function with useActionState for Progressive Enhancement (JS)
DESCRIPTION: A client component `UpdateName` demonstrating the use of `useActionState` with a third argument (a permalink URL). This enables progressive enhancement by providing a fallback URL for form submissions if JavaScript hasn't loaded yet.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/reference/rsc/server-functions.md#_snippet_8

LANGUAGE: js
CODE:
```
"use client";

import {updateName} from './actions';

function UpdateName() {
  const [, submitAction] = useActionState(updateName, null, `/name/update`);

  return (
    <form action={submitAction}>
      ...
    </form>
  );
}
```

----------------------------------------

TITLE: Incorrect State Mutation in React Form (JavaScript)
DESCRIPTION: This React component demonstrates an incorrect way to update state. The `onChange` handlers directly mutate the `person` state object (`person.firstName = e.target.value;`), which can lead to unreliable UI updates and unexpected behavior in React. It initializes a `person` state object and renders input fields for first name, last name, and email, displaying the current values.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/updating-objects-in-state.md#_snippet_10

LANGUAGE: js
CODE:
```
import { useState } from 'react';

export default function Form() {
  const [person, setPerson] = useState({
    firstName: 'Barbara',
    lastName: 'Hepworth',
    email: 'bhepworth@sculpture.com'
  });

  function handleFirstNameChange(e) {
    person.firstName = e.target.value;
  }

  function handleLastNameChange(e) {
    person.lastName = e.target.value;
  }

  function handleEmailChange(e) {
    person.email = e.target.value;
  }

  return (
    <>
      <label>
        First name:
        <input
          value={person.firstName}
          onChange={handleFirstNameChange}
        />
      </label>
      <label>
        Last name:
        <input
          value={person.lastName}
          onChange={handleLastNameChange}
        />
      </label>
      <label>
        Email:
        <input
          value={person.email}
          onChange={handleEmailChange}
        />
      </label>
      <p>
        {person.firstName}{' '}
        {person.lastName}{' '}
        ({person.email})
      </p>
    </>
  );
}
```

----------------------------------------

TITLE: Controlling a Text Input with useState in React
DESCRIPTION: This snippet demonstrates how to create a basic controlled text input in React. It uses the `useState` hook to declare a `firstName` state variable, which is then bound to the input's `value` prop. The `onChange` event handler updates the `firstName` state with the input's current value, ensuring the input is always synchronized with the state.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/reference/react-dom/components/input.md#_snippet_7

LANGUAGE: JavaScript
CODE:
```
function Form() {
  const [firstName, setFirstName] = useState(''); // Declare a state variable...
  // ...
  return (
    <input
      value={firstName} // ...force the input's value to match the state variable...
      onChange={e => setFirstName(e.target.value)} // ... and update the state variable on any edits!
    />
  );
}
```

----------------------------------------

TITLE: Corrected Packing List App Component - React JavaScript
DESCRIPTION: This React component manages a list of items, demonstrating a corrected approach to item counting. Instead of separate state variables, 'total' and 'packed' counts are derived directly from the 'items' array, eliminating synchronization bugs and ensuring data consistency. This highlights the principle of deriving state.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/choosing-the-state-structure.md#_snippet_36

LANGUAGE: javascript
CODE:
```
import { useState } from 'react';
import AddItem from './AddItem.js';
import PackingList from './PackingList.js';

let nextId = 3;
const initialItems = [
  { id: 0, title: 'Warm socks', packed: true },
  { id: 1, title: 'Travel journal', packed: false },
  { id: 2, title: 'Watercolors', packed: false },
];

export default function TravelPlan() {
  const [items, setItems] = useState(initialItems);

  const total = items.length;
  const packed = items
    .filter(item => item.packed)
    .length;

  function handleAddItem(title) {
    setItems([
      ...items,
      {
        id: nextId++,
        title: title,
        packed: false
      }
    ]);
  }

  function handleChangeItem(nextItem) {
    setItems(items.map(item => {
      if (item.id === nextItem.id) {
        return nextItem;
      } else {
        return item;
      }
    }));
  }

  function handleDeleteItem(itemId) {
    setItems(
      items.filter(item => item.id !== itemId)
    );
  }

  return (
    <>  
      <AddItem
        onAddItem={handleAddItem}
      />
      <PackingList
        items={items}
        onChangeItem={handleChangeItem}
        onDeleteItem={handleDeleteItem}
      />
      <hr />
      <b>{packed} out of {total} packed!</b>
    </>
  );
}
```

----------------------------------------

TITLE: Displaying Dynamic Data and Styles in React - JavaScript
DESCRIPTION: This comprehensive example illustrates how to display dynamic user data, including images and names, and apply inline styles based on JavaScript variables. It shows the use of curly braces for both text content and attribute values, including complex expressions like string concatenation and object literals for inline styles.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/index.md#_snippet_8

LANGUAGE: JavaScript
CODE:
```
const user = {
  name: 'Hedy Lamarr',
  imageUrl: 'https://i.imgur.com/yXOvdOSs.jpg',
  imageSize: 90,
};

export default function Profile() {
  return (
    <>
      <h1>{user.name}</h1>
      <img
        className="avatar"
        src={user.imageUrl}
        alt={'Photo of ' + user.name}
        style={{
          width: user.imageSize,
          height: user.imageSize
        }}
      />
    </>
  );
}
```

----------------------------------------

TITLE: Implementing useCounter Custom React Hook
DESCRIPTION: A custom React hook that encapsulates the state (count) and effect (interval) logic for a counter, returning the current count. It uses useState and useEffect.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/reusing-logic-with-custom-hooks.md#_snippet_67

LANGUAGE: javascript
CODE:
```
import { useState, useEffect } from 'react';

export function useCounter() {
  const [count, setCount] = useState(0);
  useEffect(() => {
    const id = setInterval(() => {
      setCount(c => c + 1);
    }, 1000);
    return () => clearInterval(id);
  }, []);
  return count;
}
```

----------------------------------------

TITLE: Updating State with a New Object Literal - JavaScript
DESCRIPTION: This code provides a more concise and common way to achieve the same result as local mutation. A new object literal is directly passed to the `setPosition` function, ensuring that a fresh object is always used for state updates, which is functionally equivalent to the local mutation example.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/updating-objects-in-state.md#_snippet_9

LANGUAGE: JavaScript
CODE:
```
setPosition({
  x: e.clientX,
  y: e.clientY
});
```

----------------------------------------

TITLE: Conditional Rendering with Ternary Operator in React
DESCRIPTION: Illustrates using the JavaScript conditional (ternary) operator (`? :`) for inline conditional rendering within JSX. It renders `name + ' ✅'` if `isPacked` is true, otherwise `name`.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/conditional-rendering.md#_snippet_6

LANGUAGE: js
CODE:
```
return (
  <li className="item">
    {isPacked ? name + ' ✅' : name}
  </li>
);
```

----------------------------------------

TITLE: Managing Task List and Individual Task Actions with Context
DESCRIPTION: The `TaskList` component consumes `TasksContext` to display the current list of tasks. The nested `Task` component uses both `TasksContext` (implicitly via `task` prop) and `TasksDispatchContext` to handle user interactions like editing task text, toggling completion status, and deleting tasks by dispatching appropriate actions to the reducer.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/scaling-up-with-reducer-and-context.md#_snippet_29

LANGUAGE: js
CODE:
```
import { useState, useContext } from 'react';
import { TasksContext, TasksDispatchContext } from './TasksContext.js';

export default function TaskList() {
  const tasks = useContext(TasksContext);
  return (
    <ul>
      {tasks.map(task => (
        <li key={task.id}>
          <Task task={task} />
        </li>
      ))}
    </ul>
  );
}

function Task({ task }) {
  const [isEditing, setIsEditing] = useState(false);
  const dispatch = useContext(TasksDispatchContext);
  let taskContent;
  if (isEditing) {
    taskContent = (
      <>
        <input
          value={task.text}
          onChange={e => {
            dispatch({
              type: 'changed',
              task: {
                ...task,
                text: e.target.value
              }
            });
          }} />
        <button onClick={() => setIsEditing(false)}>
          Save
        </button>
      </>
    );
  } else {
    taskContent = (
      <>
        {task.text}
        <button onClick={() => setIsEditing(true)}>
          Edit
        </button>
      </>
    );
  }
  return (
    <label>
      <input
        type="checkbox"
        checked={task.done}
        onChange={e => {
          dispatch({
            type: 'changed',
            task: {
              ...task,
              done: e.target.checked
            }
          });
        }}
      />
      {taskContent}
      <button onClick={() => {
        dispatch({
          type: 'deleted',
          id: task.id
        });
      }}>
        Delete
      </button>
    </label>
  );
}
```

----------------------------------------

TITLE: TodoList Component with useReducer Initializer (React)
DESCRIPTION: This `TodoList` component demonstrates the efficient use of `useReducer` with an initializer function (`createInitialState`) to manage its state. It includes a `reducer` function to handle state transitions for adding todos and changing the draft input. The `createInitialState` function generates an initial list of 50 todos based on the provided `username`, ensuring this potentially expensive operation runs only once during component initialization.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/reference/react/useReducer.md#_snippet_22

LANGUAGE: js
CODE:
```
import { useReducer } from 'react';

function createInitialState(username) {
  const initialTodos = [];
  for (let i = 0; i < 50; i++) {
    initialTodos.push({
      id: i,
      text: username + "'s task #" + (i + 1)
    });
  }
  return {
    draft: '',
    todos: initialTodos,
  };
}

function reducer(state, action) {
  switch (action.type) {
    case 'changed_draft': {
      return {
        draft: action.nextDraft,
        todos: state.todos,
      };
    };
    case 'added_todo': {
      return {
        draft: '',
        todos: [{
          id: state.todos.length,
          text: state.draft
        }, ...state.todos]
      }
    }
  }
  throw Error('Unknown action: ' + action.type);
}

export default function TodoList({ username }) {
  const [state, dispatch] = useReducer(
    reducer,
    username,
    createInitialState
  );
  return (
    <>
      <input
        value={state.draft}
        onChange={e => {
          dispatch({
            type: 'changed_draft',
            nextDraft: e.target.value
          })
        }}
      />
      <button onClick={() => {
        dispatch({ type: 'added_todo' });
      }}>Add</button>
      <ul>
        {state.todos.map(item => (
          <li key={item.id}>
            {item.text}
          </li>
        ))}
      </ul>
    </>
  );
}
```

----------------------------------------

TITLE: Corrected Clock Component with Declarative Rendering - React JavaScript
DESCRIPTION: This snippet presents the corrected `Clock` component in React. Instead of direct DOM manipulation, it calculates the `className` based on the current hour and applies it declaratively to the `<h1>` element within the JSX. This aligns with React's rendering principles, making the component more predictable and testable. The `App` component and CSS remain the same.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/keeping-components-pure.md#_snippet_6

LANGUAGE: javascript
CODE:
```
export default function Clock({ time }) {
  let hours = time.getHours();
  let className;
  if (hours >= 0 && hours <= 6) {
    className = 'night';
  } else {
    className = 'day';
  }
  return (
    <h1 className={className}>
      {time.toLocaleTimeString()}
    </h1>
  );
}
```

LANGUAGE: javascript
CODE:
```
import { useState, useEffect } from 'react';
import Clock from './Clock.js';

function useTime() {
  const [time, setTime] = useState(() => new Date());
  useEffect(() => {
    const id = setInterval(() => {
      setTime(new Date());
    }, 1000);
    return () => clearInterval(id);
  }, []);
  return time;
}

export default function App() {
  const time = useTime();
  return (
    <Clock time={time} />
  );
}
```

LANGUAGE: css
CODE:
```
body > * {
  width: 100%;
  height: 100%;
}
.day {
  background: #fff;
  color: #222;
}
.night {
  background: #222;
  color: #fff;
}
```

----------------------------------------

TITLE: Incorrect Direct Function Call in React Event Handler
DESCRIPTION: This snippet highlights a common pitfall where a function is *called* immediately during component rendering instead of being *passed* as a reference to the event handler. This results in the `alert` firing on render, not on click, due to JavaScript execution within JSX curly braces.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/responding-to-events.md#_snippet_4

LANGUAGE: jsx
CODE:
```
// This alert fires when the component renders, not when clicked!
<button onClick={alert('You clicked me!')}>
```

----------------------------------------

TITLE: Marking Module as Client with use client directive in React JS
DESCRIPTION: Placing the `'use client'` directive at the very top of a file marks the module and all of its imported dependencies for client evaluation in a React Server Components environment. This example shows a component that imports client-side hooks (`useState`) and other modules, designating them all as client code.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/reference/rsc/use-client.md#_snippet_0

LANGUAGE: js
CODE:
```
'use client';

import { useState } from 'react';
import { formatDate } from './formatters';
import Button from './button';

export default function RichTextEditor({ timestamp, text }) {
  const date = formatDate(timestamp);
  // ...
  const editButton = <Button />;
  // ...
}
```

----------------------------------------

TITLE: Calling Hooks at Top Level in React Function Components and Custom Hooks - JavaScript
DESCRIPTION: This snippet demonstrates the correct way to call React Hooks. Hooks must be called at the top level of a function component or a custom Hook, before any early returns, ensuring they are not inside loops, conditions, or nested functions.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/warnings/invalid-hook-call-warning.md#_snippet_0

LANGUAGE: JavaScript
CODE:
```
function Counter() {
  // ✅ Good: top-level in a function component
  const [count, setCount] = useState(0);
  // ...
}

function useWindowWidth() {
  // ✅ Good: top-level in a custom Hook
  const [width, setWidth] = useState(window.innerWidth);
  // ...
}
```

----------------------------------------

TITLE: Managing Chat Connection in React ChatRoom (Fixed)
DESCRIPTION: This React component establishes and disconnects a chat connection using `useEffect`. It takes `roomId` and `createConnection` props. The `useEffect` dependency array now correctly includes `createConnection`, ensuring the chat reconnects when the encryption method changes.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/lifecycle-of-reactive-effects.md#_snippet_45

LANGUAGE: JavaScript
CODE:
```
import { useState, useEffect } from 'react';

export default function ChatRoom({ roomId, createConnection }) {
  useEffect(() => {
    const connection = createConnection(roomId);
    connection.connect();
    return () => connection.disconnect();
  }, [roomId, createConnection]);

  return <h1>Welcome to the {roomId} room!</h1>;
}
```

----------------------------------------

TITLE: Correctly Handling Hook Arguments by Copying (JavaScript)
DESCRIPTION: This snippet demonstrates the recommended approach for modifying values derived from hook arguments. Instead of direct mutation, a copy of the argument (`newIcon = { ...icon };`) is created and then modified. This preserves the immutability of the original argument, supporting React's local reasoning principle.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/reference/rules/components-and-hooks-must-be-pure.md#_snippet_15

LANGUAGE: js
CODE:
```
function useIconStyle(icon) {
  const theme = useContext(ThemeContext);
  const newIcon = { ...icon }; // ✅ Good: make a copy instead
  if (icon.enabled) {
    newIcon.className = computeStyle(icon, theme);
  }
  return newIcon;
}
```

----------------------------------------

TITLE: Managing External Connections with Cleanup Effects (JavaScript, CSS)
DESCRIPTION: This example illustrates how to manage connections to external systems, such as a chat server, using `useEffect` with a cleanup function. The effect establishes a connection when the component mounts and returns a function that disconnects when the component unmounts or the dependencies change. This ensures proper resource management and prevents memory leaks.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/escape-hatches.md#_snippet_4

LANGUAGE: JavaScript
CODE:
```
import { useState, useEffect } from 'react';
import { createConnection } from './chat.js';

export default function ChatRoom() {
  useEffect(() => {
    const connection = createConnection();
    connection.connect();
    return () => connection.disconnect();
  }, []);
  return <h1>Welcome to the chat!</h1>;
}
```

LANGUAGE: JavaScript
CODE:
```
export function createConnection() {
  // A real implementation would actually connect to the server
  return {
    connect() {
      console.log('✅ Connecting...');
    },
    disconnect() {
      console.log('❌ Disconnected.');
    }
  };
}
```

LANGUAGE: CSS
CODE:
```
input { display: block; margin-bottom: 20px; }
```

----------------------------------------

TITLE: Establishing Chat Connection with useEffect - JavaScript
DESCRIPTION: This `useEffect` hook connects to the chat room specified by `roomId` ('general' in this initial run). It includes a cleanup function to disconnect, which is crucial for managing connections when `roomId` changes or the component unmounts.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/lifecycle-of-reactive-effects.md#_snippet_4

LANGUAGE: JavaScript
CODE:
```
function ChatRoom({ roomId /* "general" */ }) {
  useEffect(() => {
    const connection = createConnection(serverUrl, roomId); // Connects to the "general" room
    connection.connect();
    return () => {
      connection.disconnect(); // Disconnects from the "general" room
    };
  }, [roomId]);
  // ...
```

----------------------------------------

TITLE: Implementing Tic-Tac-Toe Game Logic in React (JavaScript)
DESCRIPTION: This JavaScript code defines the core components of a React Tic-Tac-Toe game: Square for individual cells, Board for the game grid and handling clicks, and Game for managing game state, history, and player turns. It also includes a calculateWinner utility function. The useState hook is used for managing component state.
SOURCE: https://github.com/reactjs/react.dev/blob/main/src/content/learn/tutorial-tic-tac-toe.md#_snippet_83

LANGUAGE: javascript
CODE:
```
import { useState } from 'react';

function Square({ value, onSquareClick }) {
  return (
    <button className="square" onClick={onSquareClick}>
      {value}
    </button>
  );
}

function Board({ xIsNext, squares, onPlay }) {
  function handleClick(i) {
    if (calculateWinner(squares) || squares[i]) {
      return;
    }
    const nextSquares = squares.slice();
    if (xIsNext) {
      nextSquares[i] = 'X';
    } else {
      nextSquares[i] = 'O';
    }
    onPlay(nextSquares);
  }

  const winner = calculateWinner(squares);
  let status;
  if (winner) {
    status = 'Winner: ' + winner;
  } else {
    status = 'Next player: ' + (xIsNext ? 'X' : 'O');
  }

  return (
    <>
      <div className="status">{status}</div>
      <div className="board-row">
        <Square value={squares[0]} onSquareClick={() => handleClick(0)} />
        <Square value={squares[1]} onSquareClick={() => handleClick(1)} />
        <Square value={squares[2]} onSquareClick={() => handleClick(2)} />
      </div>
      <div className="board-row">
        <Square value={squares[3]} onSquareClick={() => handleClick(3)} />
        <Square value={squares[4]} onSquareClick={() => handleClick(4)} />
        <Square value={squares[5]} onSquareClick={() => handleClick(5)} />
      </div>
      <div className="board-row">
        <Square value={squares[6]} onSquareClick={() => handleClick(6)} />
        <Square value={squares[7]} onSquareClick={() => handleClick(7)} />
        <Square value={squares[8]} onSquareClick={() => handleClick(8)} />
      </div>
    </>
  );
}

export default function Game() {
  const [history, setHistory] = useState([Array(9).fill(null)]);
  const [currentMove, setCurrentMove] = useState(0);
  const xIsNext = currentMove % 2 === 0;
  const currentSquares = history[currentMove];

  function handlePlay(nextSquares) {
    const nextHistory = [...history.slice(0, currentMove + 1), nextSquares];
    setHistory(nextHistory);
    setCurrentMove(nextHistory.length - 1);
  }

  function jumpTo(nextMove) {
    setCurrentMove(nextMove);
  }

  const moves = history.map((squares, move) => {
    let description;
    if (move > 0) {
      description = 'Go to move #' + move;
    } else {
      description = 'Go to game start';
    }
    return (
      <li key={move}>
        <button onClick={() => jumpTo(move)}>{description}</button>
      </li>
    );
  });

  return (
    <div className="game">
      <div className="game-board">
        <Board xIsNext={xIsNext} squares={currentSquares} onPlay={handlePlay} />
      </div>
      <div className="game-info">
        <ol>{moves}</ol>
      </div>
    </div>
  );
}

function calculateWinner(squares) {
  const lines = [
    [0, 1, 2],
    [3, 4, 5],
    [6, 7, 8],
    [0, 3, 6],
    [1, 4, 7],
    [2, 5, 8],
    [0, 4, 8],
    [2, 4, 6],
  ];
  for (let i = 0; i < lines.length; i++) {
    const [a, b, c] = lines[i];
    if (squares[a] && squares[a] === squares[b] && squares[a] === squares[c]) {
      return squares[a];
    }
  }
  return null;
}
```