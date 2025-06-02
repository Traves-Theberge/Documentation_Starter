TITLE: Installing Framer Motion with npm
DESCRIPTION: Provides the command line instruction to install the Framer Motion library using the npm package manager.
SOURCE: https://github.com/grx7/framer-motion/blob/main/packages/framer-motion/README.md#_snippet_1

LANGUAGE: shell
CODE:
```
npm install framer-motion
```

----------------------------------------

TITLE: Basic Animation with Framer Motion JSX
DESCRIPTION: Demonstrates a basic animation using the motion.div component to animate the horizontal position (x property) to 0.
SOURCE: https://github.com/grx7/framer-motion/blob/main/packages/framer-motion/README.md#_snippet_0

LANGUAGE: jsx
CODE:
```
<motion.div animate={{ x: 0 }} />
```

----------------------------------------

TITLE: Using Framer Motion in a React Component (JSX)
DESCRIPTION: Shows how to import the motion component and use it within a React functional component to animate the opacity based on a boolean prop.
SOURCE: https://github.com/grx7/framer-motion/blob/main/packages/framer-motion/README.md#_snippet_2

LANGUAGE: jsx
CODE:
```
import { motion } from "framer-motion"

export const MyComponent = ({ isVisible }) => (
    <motion.div animate={{ opacity: isVisible ? 1 : 0 }} />
)
```

----------------------------------------

TITLE: Implementing Optimized Framer Motion Animation with Server Rendering and Hydration (React)
DESCRIPTION: Demonstrates using Framer Motion's optimized appear animation (`startOptimizedAppearAnimation`) in a React application that involves server-side rendering (`ReactDOMServer.renderToString`) and client-side hydration (`ReactDOM.hydrateRoot`). It sets up a `motion.div` component with an opacity animation, uses a `motionValue` to track animation progress and perform checks, and hydrates the root element mid-animation. Includes checks for expected opacity values during the animation lifecycle.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/optimized-appear/interrupt-tween-opacity.html#_snippet_1

LANGUAGE: javascript
CODE:
```
const { motion, animateStyle, startOptimizedAppearAnimation, optimizedAppearDataAttribute, motionValue, } = window.Motion
const { matchOpacity } = window.Assert
const root = document.getElementById("root")
const duration = 0.5
const opacity = motionValue(0)

opacity.on("render", (v) => {
  if (v < 0.495) {
    showError(
      document.getElementById("box"),
      "opacity should never be less than 0.5"
    )
  }
})

// This is the tree to be rendered "server" and client-side.
const Component = React.createElement(motion.div, {
  id: "box",
  initial: { opacity: 0 },
  animate: { opacity: 1 },
  transition: { duration, ease: "linear" },
  style: { opacity },
  /**
   * On animation start, check the values we expect to see here
   */
  onAnimationStart: () => {
    const { opacity: initialOpacity } = window.getComputedStyle(box)
    const opacityAsNumber = parseFloat(initialOpacity)
    if (opacityAsNumber < 0.4 || opacityAsNumber > 0.6) {
      showError(
        box,
        `opacity should be roughly less than 0.5 at animation start`
      )
    }
  },
  [optimizedAppearDataAttribute]: "a"
})

// Emulate server rendering of element
root.innerHTML = ReactDOMServer.renderToString(Component)

startOptimizedAppearAnimation(
  document.getElementById("box"),
  "opacity",
  [0, 1],
  {
    duration: duration * 1000,
    ease: "linear"
  },
  (animation) => {
    // Hydrate root mid-way through animation
    setTimeout(() => {
      ReactDOM.hydrateRoot(root, Component)
    }, (duration * 1000) / 2)
  }
)
```

----------------------------------------

TITLE: Animating React Component with Framer Motion
DESCRIPTION: Demonstrates using Framer Motion within a React component to animate opacity and layout. It includes state management, effect hooks, server-side rendering emulation, and starting a Web Animations API (WAAPI) animation for optimized appearance.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/optimized-appear/defer-handoff-layout-opacity.html#_snippet_1

LANGUAGE: javascript
CODE:
```
const { motion, animateStyle, animate, startOptimizedAppearAnimation, optimizedAppearDataAttribute, motionValue, frame, } = window.Motion
const { matchViewportBox } = window.Assert
const root = document.getElementById("root")
const duration = 0.5
const x = motionValue(0)
let isFirstFrame = true
function Component() {
  const \[top, setTop\] = React.useState(0)
  React.useEffect(() => {
    setTimeout(() => {
      setTop(100)
    }, 250)
  }, \[\])
  return React.createElement(motion.div, {
    id: "box",
    initial: { opacity: 0 },
    animate: { opacity: 1 },
    transition: {
      duration,
      ease: "linear",
      layout: { duration: 10 },
    },
    style: {
      x,
      top,
      position: "relative",
      background: top ? "red" : "blue",
    },
    layout: true,
    onLayoutAnimationStart: () => {
      requestAnimationFrame(() => {
        const box = document.getElementById("box")
        if (
          box.style.opacity === window.getComputedStyle(box).opacity
        ) {
          /**
           * If style.opacity and computed style.opacity are the same,
           * it means the optimised opacity animation was cancelled by
           * the layout animation.
           */
          showError(
            "style attr and computed style should be slightly different"
          )
        }
      })
    },
    \[optimizedAppearDataAttribute\]: "a",
    children: "Content",
  })
}
// Emulate server rendering of element
root.innerHTML = ReactDOMServer.renderToString(
  React.createElement(Component)
)
// Start WAAPI animation
const animation = startOptimizedAppearAnimation(
  document.getElementById("box"),
  "opacity",
  \[0, 1\],
  {
    duration: duration * 1000,
    ease: "linear",
  },
  (animation) => {
    setTimeout(() => {
      ReactDOM.hydrateRoot(
        root,
        React.createElement(Component)
      )
    }, (duration * 1000) / 4)
  }
)
```

----------------------------------------

TITLE: Creating and Animating Elements with Layout IDs (JavaScript)
DESCRIPTION: Uses a library (likely Framer Motion) to create DOM elements, associate them with 'projection' nodes using `layoutId`, handle updates, unmount an old element, and assert their final viewport positions after layout changes.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/shared-relative-new-child.html#_snippet_1

LANGUAGE: javascript
CODE:
```
const { createNode } = window.Animate
const { matchViewportBox, matchVisibility, matchOpacity, matchBorderRadius,
} = window.Assert
const { frame } = window.Projection
const box = document.getElementById("box-a")
const boxProjection = createNode(box, undefined, { layoutId: "a" })
boxProjection.willUpdate()
const newBox = document.createElement("div")
newBox.id = "box-b"
document.body.appendChild(newBox)
const newBoxProjection = createNode(newBox, undefined, { layoutId: "a", })
const child = document.createElement("div")
child.id = "child"
newBox.appendChild(child)
const childProjection = createNode(child, newBoxProjection, { layoutId: "child", })
childProjection.willUpdate()
boxProjection.unmount()
document.body.removeChild(box)
newBoxProjection.root.didUpdate()
frame.postRender(() => {
matchViewportBox(newBox, { bottom: 400, left: 100, right: 400, top: 150, })
matchViewportBox(child, { bottom: 210, left: 340, right: 390, top: 160, })
})
```

----------------------------------------

TITLE: Defining Animated Box Component (React/Framer Motion)
DESCRIPTION: Defines a React functional component that renders a `motion.div` element. It uses `useState` and `useLayoutEffect` for state management, applies initial and animate properties for animation, configures transition properties including layout animation, uses a motion value for style, enables layout animation, and includes callbacks to check for animation conflicts.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/optimized-appear/defer-handoff-layout-uselayouteffect.html#_snippet_2

LANGUAGE: javascript
CODE:
```
function Component() {
  const \[top, setTop\] = React.useState(0)
  React.useLayoutEffect(() => {
    setTop(100)
  }, \[\])
  return React.createElement(motion.div, {
    id: "box",
    initial: { x: 0, opacity: 0 },
    animate: { x: 100, opacity: 1 },
    transition: {
      duration,
      ease: "linear",
      layout: { ease: () => 0, duration: 10 },
    },
    style: {
      x,
      top,
      position: "relative",
      background: top ? "red" : "blue",
    },
    layout: true,
    onLayoutAnimationStart: () => {
      requestAnimationFrame(() => {
        const box = document.getElementById("box")
        const { top } = box.getBoundingClientRect()
        if (top !== 100) {
          showError(
            box,
            \`layout animation overridden by optimised animation\`
          )
        }
      })
    },
    onAnimationComplete: () => {
      const box = document.getElementById("box")
      const { left } = box.getBoundingClientRect()
      if (left !== 200) {
        showError(
          box,
          \`optimised animation conflict with layout measurements\`
        )
      }
    },
    \[optimizedAppearDataAttribute\]: "a",
    children: "Content",
  })
}
```

----------------------------------------

TITLE: Defining React Component with Framer Motion (JavaScript)
DESCRIPTION: Creates a React element using `React.createElement` and Framer Motion's `motion.div`. It configures initial and animate properties for opacity, transition details, links the `motionValue` to the style, includes an `onAnimationStart` check, and adds the optimized appear data attribute.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/optimized-appear/interrupt-delay-before.html#_snippet_2

LANGUAGE: javascript
CODE:
```
// This is the tree to be rendered "server" and client-side.
const Component = React.createElement(motion.div, {
  id: "box",
  initial: { opacity: 0 },
  animate: { opacity: 1 },
  transition: { duration, ease: "linear", delay },
  style: { opacity },
  /**
   * On animation start, check the values we expect to see here
   */
  onAnimationStart: () => {
    matchOpacity(document.getElementById("box"), 0)
    requestAnimationFrame(() => {
      matchOpacity(document.getElementById("box"), 0)
    })
  },
  \[optimizedAppearDataAttribute\]: "a",
})
```

----------------------------------------

TITLE: Defining React Component with Framer Motion Animations (JavaScript)
DESCRIPTION: Defines a React functional component that uses `motion.div` for animation. It includes initial and animate properties, a transition, a `motionValue` for the `x` style, and nested `motion.div` with layout animation. It also includes an `onAnimationStart` handler to check for animation cancellation issues.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/optimized-appear/defer-handoff-layout-ancestor.html#_snippet_2

LANGUAGE: JavaScript
CODE:
```
function Component() {
  const \[top, setTop\] = React.useState(0)
  React.useEffect(() => {
    setTimeout(() => {
      setTop(100)
    }, 100)
  }, \[\])
  return React.createElement(motion.div, {
    id: "optimised-box",
    className: "box",
    initial: {
      x: 0,
      opacity: 0,
      backgroundColor: "#00f",
    },
    animate: {
      x: 100,
      opacity: 1,
      backgroundColor: "#00f",
    },
    transition: {
      duration,
      ease: "linear",
    },
    style: {
      x,
      position: "relative",
    },
    onAnimationStart: () => {
      setTimeout(() => {
        const box = document.getElementById("optimised-box")
        console.log(
          box.style.opacity,
          window.getComputedStyle(box).opacity
        )
        if (
          box.style.opacity === window.getComputedStyle(box).opacity
        ) {
          showError(
            box,
            \`Optimised opacity animation cancelled by child layout animations\`
          )
        }
        if (
          box.style.backgroundColor === window.getComputedStyle(box).backgroundColor
        ) {
          showError(
            box,
            \`Optimised background-color animation cancelled by child layout animations\`
          )
        }
        if (!window.Assert.xTransformEquals(box)) {
          showError(
            box,
            \`Optimised transform NOT animation cancelled by child layout animations\`
          )
        }
      }, 150)
    },
    \[optimizedAppearDataAttribute\]: "a",
    children: React.createElement(motion.div, {
      id: "layout-box",
      className: "box",
      transition: {
        duration,
        ease: "linear",
        layout: {
          ease: () => 1,
          duration: 10
        },
      },
      style: {
        top,
        position: "relative",
        background: top ? "red" : "blue",
      },
      layout: true,
      children: "Content",
    }),
  })
}
```

----------------------------------------

TITLE: Setting Up Projection Nodes for Nested Elements (JavaScript)
DESCRIPTION: Initializes necessary functions from global objects, creates projection nodes for existing and newly created nested HTML elements, linking them via parent-child relationships and shared layout IDs. It triggers updates and performs checks on the state of the projection system after setup.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/perf-shared-deep.html#_snippet_1

LANGUAGE: JavaScript
CODE:
```
const { createNode } = window.Animate
const { matchViewportBox, checkFrame } = window.Assert
const { frame } = window.Projection
const duration = 10
const a = document.getElementById("a")
const aProjection = createNode( a, undefined, { layoutId: "box" }, { duration } )
const a2 = document.getElementById("a-2")
const a2Projection = createNode( a2, aProjection, { layoutId: "2" }, { duration } )
const a3 = document.getElementById("a-3")
const a3Projection = createNode( a3, a2Projection, { layoutId: "3" }, { duration } )
aProjection.willUpdate()
a2Projection.willUpdate()
a3Projection.willUpdate()
const b = document.createElement("div")
b.id = "b"
document.body.appendChild(b)
const bProjection = createNode( b, undefined, { layoutId: "box" }, { duration } )
const b2 = document.createElement("div")
b2.id = "b-2"
b.appendChild(b2)
const b2Projection = createNode( b2, bProjection, { layoutId: "2" }, { duration } )
const b3 = document.createElement("div")
b3.id = "b-3"
b2.appendChild(b3)
const b3Projection = createNode( b3, b2Projection, { layoutId: "3" }, { duration } )
aProjection.root.didUpdate()
/**
 * Shared element transition nodes are currently all recalculated,
 * it would be good to investigate in the future if there's further
 * safe optimisations we can make here.
 */
requestAnimationFrame(() => {
  requestAnimationFrame(() => {
    console.log(window.ProjectionFrames)
    checkFrame(a, 1, { totalNodes: 7, resolvedTargetDeltas: 3, recalculatedProjection: 6, })
  })
})
```

----------------------------------------

TITLE: Applying Projection and Asserting Layout (JavaScript)
DESCRIPTION: Retrieves utility functions from window.Projection, window.Undo, and window.Assert. Gets DOM elements, captures their initial bounding boxes, creates projection nodes for them, sets a border radius value on one projection, groups the projections, triggers layout updates, changes the container's flex direction, and finally, in a post-render callback, asserts the viewport box, opacity, and border radius of the elements against expected values.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/flexbox-siblings-layout-group.html#_snippet_1

LANGUAGE: JavaScript
CODE:
```
const { frame, nodeGroup } = window.Projection
const { createNode } = window.Undo
const { matchOpacity, matchBorderRadius, matchViewportBox } = window.Assert
const container = document.getElementById("container")
const a = document.getElementById("a")
const b = document.getElementById("b")
const aOrigin = a.getBoundingClientRect()
const bOrigin = b.getBoundingClientRect()
const aProjection = createNode(a)
const bProjection = createNode(b)
aProjection.setValue("borderRadius", 20)
const group = nodeGroup()
group.add(aProjection)
group.add(bProjection)
aProjection.willUpdate()
container.style.flexDirection = "column-reverse"
aProjection.root.didUpdate()
frame.postRender(() => {
  window.scrollTo(0, 0)
  matchViewportBox(a, aOrigin)
  matchViewportBox(b, bOrigin)
  matchOpacity(a, 1)
  matchOpacity(b, 1)
  matchBorderRadius(a, "13.3333% / 10%")
  matchBorderRadius(b, "")
})
```

----------------------------------------

TITLE: Framer Motion Optimized Appear Animation with React SSR/Hydration
DESCRIPTION: Sets up a Framer Motion `motionValue` to track animation progress, defines a React component using `motion.div` with initial and animate states, server-renders the component to HTML, and then initiates an optimized appear animation. It includes checks during the animation and hydrates the React tree client-side partway through the animation.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/optimized-appear/interrupt-tween-transforms.html#_snippet_1

LANGUAGE: javascript
CODE:
```
const { motion, animateStyle, startOptimizedAppearAnimation, optimizedAppearDataAttribute, motionValue, } = window.Motion
const { matchViewportBox } = window.Assert
const root = document.getElementById("root")
const duration = 0.5
const y = motionValue(0)
let isFirstFrame = true
y.on("change", (latest) => {
  if (latest < 50) {
    showError(
      document.getElementById("box"),
      \`y transform should never be less than 50, but was ${latest}\`
    )
  }
  if (isFirstFrame && latest === 100) {
    showError(
      document.getElementById("box"),
      \`y transform shouldn't be 100 on the first frame\`
    )
  }
  isFirstFrame = false
})
// This is the tree to be rendered "server" and client-side.
const Component = React.createElement(motion.div, {
  id: "box",
  initial: { y: 0, scale: 1 },
  animate: { y: 100, scale: 2 },
  transition: { duration, ease: "linear" },
  style: { y },
  /**
   * On animation start, check the values we expect to see here
   */
  onAnimationStart: () => {
    const { top, left } = document
      .getElementById("box")
      .getBoundingClientRect()
    if (top < 120 || top > 130 || left < 70 || left > 85) {
      showError(box, \`unexpected viewport box\`)
    }
  },
  \[optimizedAppearDataAttribute\]: "a",
})
// Emulate server rendering of element
root.innerHTML = ReactDOMServer.renderToString(Component)
// Start Motion One animation
const animation = startOptimizedAppearAnimation(
  document.getElementById("box"),
  "transform",
  \["translateY(0px) scale(1)", "translateY(100px) scale(2)"\],
  {
    duration: duration * 1000,
    ease: "linear",
  },
  (animation) => {
    // Hydrate root mid-way through animation
    setTimeout(() => {
      ReactDOM.hydrateRoot(root, Component)
    }, (duration * 1000) / 2)
  }
)
```

----------------------------------------

TITLE: Initializing Projection Nodes and Handling Scroll (JavaScript)
DESCRIPTION: Initializes projection nodes for the scroller and sticky elements using a library (likely Framer Motion). It gets DOM elements, creates nodes with specific configurations (layoutScroll, layoutRoot), stores the initial sticky position, triggers layout updates, scrolls the container, and uses a timeout to assert the sticky element's position after the scroll.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/sticky-element-scroll.html#_snippet_1

LANGUAGE: javascript
CODE:
```
const { createNode } = window.Undo
const { matchViewportBox, matchVisibility, matchOpacity, addPageScroll, } = window.Assert
const { frame } = window.Projection

const scroller = document.getElementById("scroller")
const scrollerProjection = createNode(scroller, undefined, { layoutScroll: true, })
const sticky = document.getElementById("sticky")
const stickyProjection = createNode(sticky, scrollerProjection, { layoutRoot: true, })

const stickyOrigin = sticky.getBoundingClientRect()

stickyProjection.willUpdate()
scroller.scrollTo(0, 100)
stickyProjection.root.didUpdate()

setTimeout(() => {
  matchViewportBox(sticky, stickyOrigin)
}, 50)
```

----------------------------------------

TITLE: Initializing Projection Nodes with JavaScript
DESCRIPTION: Retrieves DOM elements and uses a library (likely Framer Motion's Animate and Projection) to create hierarchical projection nodes for the parent, mid, and child elements. It then triggers updates on these nodes and schedules post-render checks using the frame object to assert layout correctness.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/animate-relative-nested.html#_snippet_1

LANGUAGE: javascript
CODE:
```
const { createNode, relativeEase } = window.Animate
const { matchViewportBox } = window.Assert
const { frame } = window.Projection

const parent = document.getElementById("parent")
const mid = document.getElementById("mid")
const child = document.getElementById("child")

const childOrigin = child.getBoundingClientRect()

const parentProjection = createNode(
  parent,
  undefined,
  {}
  // { duration: 2 }
  // { duration: 200, ease: relativeEase() }
)

const midProjection = createNode(
  mid,
  parentProjection,
  {}
  // { duration: 2 }
  // { duration: 200, ease: relativeEase() }
)

const childProjection = createNode(
  child,
  midProjection,
  {}
  // { duration: 2 }
  // { duration: 0 }
)

parentProjection.willUpdate()
midProjection.willUpdate()
childProjection.willUpdate()

parent.classList.add("b")

parentProjection.root.didUpdate()

frame.postRender(() => {
  frame.postRender(() => {
    matchViewportBox(child, childOrigin)
  })
})
```

----------------------------------------

TITLE: Manipulating Layout with Projection (JavaScript)
DESCRIPTION: Uses a projection library (likely related to Framer Motion) to create nodes for parent and child elements, apply transformations (scale, x), update their state, and then assert their viewport box matches their original position after a class change.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/transform-nested-parent-layout-change-scale-child-layout-change-transform.html#_snippet_1

LANGUAGE: javascript
CODE:
```
const { createNode } = window.Undo
const { matchViewportBox } = window.Assert
const { frame } = window.Projection
const parent = document.getElementById("parent")
const child = document.getElementById("child")
const parentProjection = createNode(parent)
const childProjection = createNode(child, parentProjection)
parentProjection.setValue("scale", 2)
parentProjection.setValue("x", 400)
childProjection.setValue("scale", 0.5)
childProjection.setValue("x", -100)
frame.postRender(() => {
  const parentOrigin = parent.getBoundingClientRect()
  const childOrigin = child.getBoundingClientRect()
  parentProjection.willUpdate()
  childProjection.willUpdate()
  parent.classList.add("b")
  childProjection.root.didUpdate()
  frame.postRender(() => {
    matchViewportBox(parent, parentOrigin)
    matchViewportBox(child, childOrigin)
  })
})
```

----------------------------------------

TITLE: Using Projection Nodes for Element Manipulation (JavaScript)
DESCRIPTION: Demonstrates the use of a projection library (likely Framer Motion's internal projection system) to create and manage projection nodes for DOM elements. It shows creating nodes, establishing parent-child relationships, triggering updates, mounting nodes, and asserting their final viewport positions after layout changes.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/new-element-concurrent.html#_snippet_1

LANGUAGE: javascript
CODE:
```
const { createNode } = window.Undo
const { matchViewportBox } = window.Assert
const { frame, HTMLProjectionNode } = window.Projection

const box = document.getElementById("box")
const boxProjection = createNode(box)
const boxOrigin = box.getBoundingClientRect()

const a = document.createElement("div")
a.id = "child"

// Render phase
const aProjection = createNode(
  a,
  boxProjection,
  { layout: true },
  "a"
)
const bProjection = new HTMLProjectionNode({}, boxProjection)

// Snapshot
boxProjection.willUpdate()

// Commit
box.appendChild(a)
box.classList.add("b")

// First layout effect
boxProjection.root.didUpdate()

// A/B mounts
aProjection.mount(a)

frame.postRender(() => {
  matchViewportBox(box, boxOrigin)
  matchViewportBox(a, {
    bottom: 70,
    left: 20,
    right: 70,
    top: 20,
  })
})
```

----------------------------------------

TITLE: Implementing Framer Motion Optimized Appear Animation with SSR (JavaScript)
DESCRIPTION: Imports necessary Framer Motion and assertion utilities, sets up a motion value for opacity with a validation check, defines a React component using motion.div with initial/animate opacity and a transition, server-renders the component, initiates an optimized appear animation, and then hydrates the root after a delay to simulate a real-world SSR hydration scenario.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/optimized-appear/persist.html#_snippet_1

LANGUAGE: javascript
CODE:
```
const { motion, animateStyle, startOptimizedAppearAnimation, optimizedAppearDataAttribute, motionValue, } = window.Motion
const { matchOpacity } = window.Assert
const root = document.getElementById("root")
const duration = 0.5
const opacity = motionValue(0)
opacity.onChange((v) => {
  if (v < 1) {
    showError(
      document.getElementById("box"),
      "opacity should never be less than 1"
    )
  }
})
// This is the tree to be rendered "server" and client-side.
const Component = React.createElement(motion.div, {
  id: "box",
  initial: { opacity: 0 },
  animate: { opacity: 1 },
  transition: { duration, ease: "linear" },
  style: { opacity },
  /**
   * On animation start, check the values we expect to see here
   */
  onAnimationStart: () => {
    console.log(
      getComputedStyle(document.getElementById("box")).opacity
    )
    matchOpacity(document.getElementById("box"), 1)
  },
  [optimizedAppearDataAttribute]: "a",
})
// Emulate server rendering of element
root.innerHTML = ReactDOMServer.renderToString(Component)
startOptimizedAppearAnimation(
  document.getElementById("box"),
  "opacity",
  [0, 1],
  {
    duration: duration * 1000,
    ease: "linear",
  },
  (animation) => {
    /**
     * Give it time to commit the finished animation
     */
    setTimeout(() => {
      // Hydrate root mid-way through animation
      ReactDOM.hydrateRoot(root, Component)
    }, duration * 1000 + 1000)
  }
)
```

----------------------------------------

TITLE: Animating Layout Changes with Framer Motion Projection
DESCRIPTION: Uses Framer Motion's Projection API to create nodes for parent and child elements. It scales the parent, then updates the parent's class to change its layout, and finally asserts that the original viewport positions of both elements are maintained after the layout change, likely demonstrating layout projection capabilities.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/transform-nested-parent-scale-child-layout-change.html#_snippet_1

LANGUAGE: javascript
CODE:
```
const { createNode } = window.Undo
const { matchViewportBox } = window.Assert
const { frame } = window.Projection
const parent = document.getElementById("parent")
const child = document.getElementById("child")
const parentProjection = createNode(parent)
const childProjection = createNode(child, parentProjection)
parentProjection.setValue("scale", 2)
frame.postRender(() => {
  const parentOrigin = parent.getBoundingClientRect()
  const childOrigin = child.getBoundingClientRect()
  childProjection.willUpdate()
  parent.classList.add("b")
  childProjection.root.didUpdate()
  frame.postRender(() => {
    matchViewportBox(parent, parentOrigin)
    matchViewportBox(child, childOrigin)
  })
})
```

----------------------------------------

TITLE: Dynamic Box Creation and Sequential Animation (JavaScript)
DESCRIPTION: Generates 100 box elements dynamically using JavaScript, appends them to the DOM, and applies two sequential animation sequences to each box using the Web Animations API, modifying properties like rotation, color, width, and translation.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/benchmarks/cold-start-waapi.html#_snippet_1

LANGUAGE: javascript
CODE:
```
// Create boxes
const numBoxes = 100
let html = ""
for (let i = 0; i < numBoxes; i++) {
  html += `<div><div class="box"></div></div>`
}
document.querySelector(".container").innerHTML = html
const boxes = document.querySelectorAll(".box")
setTimeout(() => {
  boxes.forEach((box) => {
    const animation = box.animate(
      {
        rotate: Math.random() * 360 + "deg",
        backgroundColor: "#f00",
        width: Math.random() * 100 + "%",
        translate: "5px 0"
      },
      {
        duration: 1000,
        fill: "both"
      }
    )
    animation.onfinish = () => {
      requestAnimationFrame(() => {
        animation.commitStyles()
        animation.cancel()
      })
    }
  })
  setTimeout(() => {
    boxes.forEach((box) => {
      const animation = box.animate(
        {
          width: Math.random() * 100 + "px",
          translate: "50% 0"
        },
        {
          duration: 1000,
          fill: "both"
        }
      )
      animation.onfinish = () => {
        requestAnimationFrame(() => {
          animation.commitStyles()
          animation.cancel()
        })
      }
    })
  }, 1500)
}, 1000)
```

----------------------------------------

TITLE: Applying Projection and Layout Assertions (JavaScript)
DESCRIPTION: Utilizes custom utilities (Undo, Assert, Projection) to create a projection node for a DOM element. It then uses postRender callbacks to perform layout assertions, update the projection (scaling the box), modify the element's class, and assert the layout again after the projection transformation.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/transform-single-layout-change-with-scale-change.html#_snippet_1

LANGUAGE: JavaScript
CODE:
```
const { createNode } = window.Undo
const { matchViewportBox } = window.Assert
const { frame } = window.Projection
const box = document.getElementById("box")
const boxProjection = createNode(box)
const boxOrigin = box.getBoundingClientRect()
frame.postRender(() => {
  matchViewportBox(box, boxOrigin)
  boxProjection.willUpdate()
  boxProjection.setValue("scale", 2)
  box.classList.add("b")
  const transformedBox = {
    top: -50,
    left: -50,
    right: 150,
    bottom: 150,
  }
  boxProjection.root.didUpdate()
  frame.postRender(() => matchViewportBox(box, transformedBox))
})
```

----------------------------------------

TITLE: Implementing Optimized Appear Animation with Framer Motion (JavaScript)
DESCRIPTION: Demonstrates using Framer Motion's optimized appear animation feature. It defines a React component using `motion.div`, server-renders it, starts an optimized appear animation, and then hydrates the root with the same component, including checks during the animation lifecycle. Requires Framer Motion, React, and ReactDOM.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/optimized-appear/interrupt-delay-before-accelerated.html#_snippet_1

LANGUAGE: JavaScript
CODE:
```
const { motion, animateStyle, startOptimizedAppearAnimation, optimizedAppearDataAttribute, motionValue, } = window.Motion
const { matchOpacity } = window.Assert
const root = document.getElementById("root")
const duration = 0.25
const delay = 0.5

// This is the tree to be rendered "server" and client-side.
const Component = React.createElement(motion.div, {
  id: "box",
  initial: { opacity: 0 },
  animate: { opacity: 1 },
  transition: { duration, ease: "linear", delay },
  // style: { opacity },
  /**
   * On animation start, check the values we expect to see here
   */
  onAnimationStart: () => {
    matchOpacity(document.getElementById("box"), 0)
    requestAnimationFrame(() => {
      matchOpacity(document.getElementById("box"), 0)
    })
  },
  \[optimizedAppearDataAttribute\]: "a",
})

// Emulate server rendering of element
root.innerHTML = ReactDOMServer.renderToString(Component)

startOptimizedAppearAnimation(
  document.getElementById("box"),
  "opacity",
  \[0, 1\],
  {
    duration: duration * 1000,
    ease: "linear",
    delay: delay * 1000,
  },
  (animation) => {
    // Hydrate root mid-way through delay
    setTimeout(() => {
      ReactDOM.hydrateRoot(root, Component)
      const { opacity: initialOpacity } = window.getComputedStyle(box)
      if (initialOpacity !== "0") {
        showError(box, \`opacity should have animated\`)
      }
    }, 300)
  }
)
```

----------------------------------------

TITLE: Animating and Projecting Elements with JavaScript
DESCRIPTION: Initializes projection nodes for parent and child elements using 'createNode', retrieves initial element position, triggers layout updates by adding/removing CSS classes, updates projections using 'willUpdate' and 'didUpdate', and uses 'frame.postRender' and 'setTimeout' for timing and ensuring updates occur after rendering. It also includes an assertion using 'matchViewportBox'.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/animate-nested-scale-correction.html#_snippet_1

LANGUAGE: javascript
CODE:
```
const { createNode, relativeEase } = window.Animate
const { matchViewportBox } = window.Assert
const { frame } = window.Projection

const parent = document.getElementById("parent")
const a = document.getElementById("a")
const b = document.getElementById("b")

const parentProjection = createNode(
  parent,
  undefined,
  {},
  { duration: 0.1 }
)
const aProjection = createNode(
  a,
  parentProjection,
  {},
  { duration: 0.1 }
)
const bProjection = createNode(
  b,
  parentProjection,
  {},
  { duration: 0.1 }
)

const aOrigin = a.getBoundingClientRect()

parentProjection.willUpdate()
aProjection.willUpdate()
bProjection.willUpdate()
a.classList.add("open")
parentProjection.root.didUpdate()

frame.postRender(() => {
  frame.postRender(() => {
    parentProjection.willUpdate()
    aProjection.willUpdate()
    bProjection.willUpdate()
    a.classList.remove("open")
    parentProjection.root.didUpdate()

    setTimeout(() => {
      parentProjection.willUpdate()
      aProjection.willUpdate()
      bProjection.willUpdate()
      b.classList.add("open")
      parentProjection.root.didUpdate()

      frame.postRender(() => {
        frame.postRender(() => {
          matchViewportBox(a, aOrigin)
        })
      })
    }, 120)
  })
})
```

----------------------------------------

TITLE: Framer Motion Optimized Appear Animation Setup (JavaScript)
DESCRIPTION: Initializes Framer Motion components, sets up a motion value to track animation progress, defines a React component using motion.div with initial and animate properties, and includes checks during the animation lifecycle. It prepares the component for server-side rendering and subsequent client-side hydration.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/optimized-appear/interrupt-tween-x.html#_snippet_1

LANGUAGE: javascript
CODE:
```
const { motion, animateStyle, startOptimizedAppearAnimation, optimizedAppearDataAttribute, motionValue, } = window.Motion
const { matchViewportBox } = window.Assert
const root = document.getElementById("root")
const duration = 0.5
const x = motionValue(0)
let isFirstFrame = true
x.onChange((latest) => {
  if (latest < 50) {
    showError(
      document.getElementById("box"),
      `x transform should never be less than 50`
    )
  }
  if (latest === 100 && isFirstFrame) {
    showError(
      document.getElementById("box"),
      `x transform shouldn't be 100 on the first frame`
    )
  }
  isFirstFrame = false
})
// This is the tree to be rendered "server" and client-side.
const Component = React.createElement(motion.div, {
  id: "box",
  initial: { x: 0 },
  animate: { x: 100 },
  transition: { duration, ease: "linear" },
  style: { x },
  /**
   * On animation start, check the values we expect to see here
   */
  onAnimationStart: () => {
    const { top, left } = document
      .getElementById("box")
      .getBoundingClientRect()
    // Fuzzy to be permissive towards Cypress runner
    if (left < 135 || left > 165) {
      showError(box, `unexpected viewport box`)
    }
  },
  \[optimizedAppearDataAttribute\]: "a",
  children: "Content",
})
```

----------------------------------------

TITLE: Manipulating Layout and Scrolling with JavaScript
DESCRIPTION: Uses Framer Motion's Projection API (via `createNode`) to track element layout. Modifies element classes, scrolls the window, and asserts layout properties after updates and scrolling.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/single-element-layout-change-with-child-page-scroll.html#_snippet_1

LANGUAGE: javascript
CODE:
```
const { createNode } = window.Undo
const { matchViewportBox, addPageScroll } = window.Assert
const { frame } = window.Projection
const box = document.getElementById("box")
const boxProjection = createNode(box)
const child = document.getElementById("child")
const childProjection = createNode(child, boxProjection)
const boxOrigin = box.getBoundingClientRect()
const childOrigin = child.getBoundingClientRect()
boxProjection.willUpdate()
childProjection.willUpdate()
box.classList.add("b")
const scrollOffset = \[50, 100\]
window.scrollTo(...scrollOffset)
boxProjection.root.didUpdate()
frame.postRender(() => {
  matchViewportBox(box, addPageScroll(boxOrigin, ...scrollOffset))
  matchViewportBox(
    child, addPageScroll(childOrigin, ...scrollOffset)
  )
})
```

----------------------------------------

TITLE: Testing Element Projection and Layout Changes (JavaScript)
DESCRIPTION: Uses functions from `window.Undo`, `window.Assert`, and `window.Projection` to get DOM elements, create projection nodes, apply a CSS class to change the layout, and then assert that the elements' viewport boxes match their original positions in a post-render step.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/flexbox-siblings-to-grid.html#_snippet_1

LANGUAGE: javascript
CODE:
```
const { createNode } = window.Undo
const { matchViewportBox } = window.Assert
const { frame } = window.Projection
const container = document.getElementById("container")
const a = document.getElementById("a")
const b = document.getElementById("b")
const aOrigin = a.getBoundingClientRect()
const bOrigin = b.getBoundingClientRect()
const aProjection = createNode(a)
const bProjection = createNode(b)
aProjection.willUpdate()
bProjection.willUpdate()
container.classList.add("as-grid")
aProjection.root.didUpdate()
frame.postRender(() => {
  matchViewportBox(a, aOrigin)
  matchViewportBox(b, bOrigin)
})
```

----------------------------------------

TITLE: React Motion Component Definition
DESCRIPTION: Defines a React functional component using `React.createElement` and `motion.div`. It utilizes Framer Motion's `initial`, `animate`, `transition`, `style`, and `layout` props, including specific transition settings for layout. It also includes `onLayoutAnimationStart` and `onAnimationComplete` callbacks to check for animation conflicts and uses `useState` and `useEffect` for state management.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/optimized-appear/defer-handoff-layout.html#_snippet_2

LANGUAGE: javascript
CODE:
```
function Component() {
  const [top, setTop] = React.useState(0)
  React.useEffect(() => {
    setTimeout(() => {
      setTop(100)
    }, 200)
  }, [])
  return React.createElement(motion.div, {
    id: "box",
    initial: { x: 0, opacity: 0 },
    animate: { x: 100, opacity: 1 },
    transition: { duration, ease: "linear", layout: { ease: () => 0, duration: 10 }, },
    style: { x, top, position: "relative", background: top ? "red" : "blue", },
    layout: true,
    onLayoutAnimationStart: () => {
      requestAnimationFrame(() => {
        const box = document.getElementById("box")
        const { top } = box.getBoundingClientRect()
        if (top !== 100) {
          showError(
            box,
            `layout animation overridden by optimised animation`
          )
        }
      })
    },
    onAnimationComplete: () => {
      const box = document.getElementById("box")
      const { left } = box.getBoundingClientRect()
      if (left !== 200) {
        showError(
          box,
          `optimised animation conflict with layout measurements`
        )
      }
    },
    [optimizedAppearDataAttribute]: "a",
    children: "Content",
  })
}
```

----------------------------------------

TITLE: Framer Motion SSR and Hydration Test (JavaScript)
DESCRIPTION: This script sets up a test environment using Framer Motion, React, and ReactDOMServer/hydrateRoot. It defines a motion component, server-renders it, starts an optimized appear animation, tracks a motion value ('y') with validation checks, and hydrates the root element mid-animation to test the transition from server-rendered static content to a client-side interactive component.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/optimized-appear/portal.html#_snippet_1

LANGUAGE: JavaScript
CODE:
```
const { motion, animateStyle, startOptimizedAppearAnimation, optimizedAppearDataAttribute, motionValue, } = window.Motion
const { matchViewportBox } = window.Assert
const root = document.getElementById("root")
const duration = 0.5
const y = motionValue(0)
let isFirstFrame = true
y.on("change", (latest) => {
  if (latest < 50) {
    showError(
      document.getElementById("box"),
      `y transform should never be less than 50, but was ${latest}`
    )
  }
  if (isFirstFrame && latest === 100) {
    showError(
      document.getElementById("box"),
      `y transform shouldn't be 100 on the first frame`
    )
  }
  isFirstFrame = false
})
// This is the tree to be rendered "server" and client-side.
const Component = React.createElement(motion.div, {
  id: "box",
  initial: { y: 0, scale: 1 },
  animate: { y: 100, scale: 2 },
  transition: { duration, ease: "linear" },
  style: { y },
  /**
   * On animation start, check the values we expect to see here
   */
  onAnimationStart: () => {
    const { top, left } = document
      .getElementById("box")
      .getBoundingClientRect()
    if (top < 120 || top > 130 || left < 70 || left > 85) {
      showError(box, `unexpected viewport box`)
    }
  },
  [optimizedAppearDataAttribute]: "a",
})
// Emulate server rendering of element
root.innerHTML = ReactDOMServer.renderToString(Component)
// Start Motion One animation
const animation = startOptimizedAppearAnimation(
  document.getElementById("box"),
  "transform",
  ["translateY(0px) scale(1)", "translateY(100px) scale(2)"],
  {
    duration: duration * 1000,
    ease: "linear",
  },
  (animation) => {
    // Hydrate root mid-way through animation
    setTimeout(() => {
      ReactDOM.createRoot(
        document.getElementById("portal")
      ).render(
        React.createElement(motion.div, {
          id: "box-2",
          initial: { y: 0 },
          animate: { y: 100, scale: 2 },
          transition: { duration, ease: "linear" },
          style: {
            width: 100,
            height: 100,
            background: "red",
          },
        })
      )
      ReactDOM.hydrateRoot(root, Component)
    }, (duration * 1000) / 2)
  }
)
```

----------------------------------------

TITLE: Framer Motion Layout and Optimized Appear Test Component (JavaScript)
DESCRIPTION: A React component utilizing Framer Motion to create a box element that undergoes layout and optimized appear animations. It includes initial and animate properties, a transition configuration, style overrides using a motion value and state, and lifecycle callbacks to check element position during and after animations.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/optimized-appear/defer-handoff-layout-useeffect.html#_snippet_1

LANGUAGE: javascript
CODE:
```
const { motion, animateStyle, animate, startOptimizedAppearAnimation, optimizedAppearDataAttribute, motionValue, frame, } = window.Motion
const { matchViewportBox } = window.Assert
const root = document.getElementById("root")
const duration = 0.5
const x = motionValue(0)
let isFirstFrame = true
function Component() {
  const \[top, setTop\] = React.useState(0)
  React.useEffect(() => {
    setTop(100)
  }, \[])
  return React.createElement(motion.div, {
    id: "box",
    initial: { x: 0, opacity: 0 },
    animate: { x: 100, opacity: 1 },
    transition: {
      duration,
      ease: "linear",
      layout: { ease: () => 0, duration: 10 },
    },
    style: {
      x,
      top,
      position: "relative",
      background: top ? "red" : "blue",
    },
    layout: true,
    onLayoutAnimationStart: () => {
      requestAnimationFrame(() => {
        const box = document.getElementById("box")
        const { top } = box.getBoundingClientRect()
        if (top !== 100) {
          showError(
            box,
            `layout animation overridden by optimised animation`
          )
        }
      })
    },
    onAnimationComplete: () => {
      const box = document.getElementById("box")
      const { left } = box.getBoundingClientRect()
      console.log(left)
      if (left !== 200) {
        showError(
          box,
          `optimised animation conflict with layout measurements`
        )
      }
    },
    \[optimizedAppearDataAttribute\]: "a",
    children: "Content",
  })
}
// Emulate server rendering of element
root.innerHTML = ReactDOMServer.renderToString(
  React.createElement(Component)
)
// Start optimised opacity animation
startOptimizedAppearAnimation(
  document.getElementById("box"),
  "opacity",
  \[0, 1\],
  {
    duration: duration * 1000,
    ease: "linear",
  }
)
// Start WAAPI animation
const animation = startOptimizedAppearAnimation(
  document.getElementById("box"),
  "transform",
  \["translateX(0px)", "translateX(100px)"\],
  {
    duration: duration * 1000,
    ease: "linear",
  },
  (animation) => {
    setTimeout(() => {
      ReactDOM.hydrateRoot(
        root,
        React.createElement(Component)
      )
    }, (duration * 1000) / 2)
  }
)
```

----------------------------------------

TITLE: Implementing Framer Motion Layout and Optimized Animations (JavaScript)
DESCRIPTION: Defines a React component using Framer Motion for layout and optimized appear animations. It simulates server-side rendering, hydrates the component, and initiates various animations including transform, opacity, and background color.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/optimized-appear/defer-handoff-layout-child.html#_snippet_1

LANGUAGE: javascript
CODE:
```
const { motion, animateStyle, animate, startOptimizedAppearAnimation, optimizedAppearDataAttribute, motionValue, frame, } = window.Motion
const { matchViewportBox, xTransformEquals } = window.Assert
const root = document.getElementById("root")
const duration = 0.5
const x = motionValue(0)
let isFirstFrame = true
function Component() {
  const \[top, setTop\] = React.useState(0)
  React.useEffect(() => {
    setTimeout(() => {
      setTop(100)
    }, 100)
  }, \[\])
  return React.createElement(motion.div, {
    id: "container",
    className: "box",
    transition: {
      duration,
      ease: "linear",
      layout: {
        ease: () => 1,
        duration: 10
      },
    },
    style: {
      top,
      position: "relative",
      background: top ? "red" : "blue",
    },
    layout: true,
    children: "Content",
    children: React.createElement(motion.div, {
      id: "optimised-box",
      className: "box",
      initial: {
        x: 0,
        opacity: 0,
        backgroundColor: "#f00",
      },
      animate: {
        x: 100,
        opacity: 1,
        backgroundColor: "#00f",
      },
      transition: {
        duration,
        ease: "linear",
      },
      style: {
        x,
        position: "relative",
      },
      onAnimationStart: () => {
        setTimeout(() => {
          const box = document.getElementById("optimised-box")
          if (
            box.style.opacity === window.getComputedStyle(box).opacity ||
            box.style.backgroundColor === window.getComputedStyle(box)
            .backgroundColor ||
            xTransformEquals(box)
          ) {
            showError(
              box,
              \`Optimised animations cancelled by ancestor layout animations\`
            )
          }
        }, 150)
      },
      \[optimizedAppearDataAttribute\]: "a",
      children: "Content",
    }),
  })
}
// Emulate server rendering of element
root.innerHTML = ReactDOMServer.renderToString(
  React.createElement(Component)
)
// Start optimised opacity animation
startOptimizedAppearAnimation(
  document.getElementById("optimised-box"),
  "opacity",
  \[0, 1\],
  {
    duration: duration * 1000,
    ease: "linear",
  }
)
startOptimizedAppearAnimation(
  document.getElementById("optimised-box"),
  "backgroundColor",
  \["#f00", "#00f"\],
  {
    duration: duration * 1000,
    ease: "linear",
  }
)
// Start WAAPI animation
const animation = startOptimizedAppearAnimation(
  document.getElementById("optimised-box"),
  "transform",
  \["translateX(0px)", "translateX(100px)"\],
  {
    duration: duration * 1000,
    ease: "linear",
  },
  (animation) => {
    setTimeout(() => {
      ReactDOM.hydrateRoot(
        root,
        React.createElement(Component)
      )
    }, (duration * 1000) / 4)
  }
)
```

----------------------------------------

TITLE: React Component with Framer Motion Animation and Hydration
DESCRIPTION: Defines a React component using Framer Motion for animation, renders it to an HTML string server-side, and then hydrates it client-side. Includes logic to start WAAPI animations and perform layout checks on animation start.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/optimized-appear/start-after-hydration.html#_snippet_1

LANGUAGE: javascript
CODE:
```
const { motion, animateStyle, startOptimizedAppearAnimation, optimizedAppearDataAttribute, motionValue, } = window.Motion
const { matchViewportBox } = window.Assert
const root = document.getElementById("root")
const duration = 1
// This is the tree to be rendered "server" and client-side.
const Component = React.createElement(motion.div, {
  id: "box",
  initial: { x: 0, opacity: 0.5 },
  animate: { x: 100, opacity: 1 },
  transition: { duration, ease: "linear" },
  /**
   * On animation start, check the values we expect to see here
   */
  onAnimationStart: () => {
    setTimeout(() => {
      // Start WAAPI animation
      startOptimizedAppearAnimation(
        document.getElementById("box"),
        "transform",
        ["translateX(0px)", "translateX(100px)"],
        {
          duration: duration * 1000,
          ease: "linear",
        }
      )
      // Start WAAPI animation
      startOptimizedAppearAnimation(
        document.getElementById("box"),
        "opacity",
        [0.5, 1],
        {
          duration: duration * 1000,
          ease: "linear",
        }
      )
      requestAnimationFrame(() => {
        const { top, left } = document
          .getElementById("box")
          .getBoundingClientRect()
        // Fuzzy to be permissive towards Cypress runner
        if (left < 130) {
          showError(box, `unexpected viewport box`)
        }
      })
    }, 500)
  },
  [optimizedAppearDataAttribute]: "a",
  children: "Content",
})
// Emulate server rendering of element
root.innerHTML = ReactDOMServer.renderToString(Component)
ReactDOM.hydrateRoot(root, Component)
```

----------------------------------------

TITLE: Framer Motion Optimized Appear Animation Test Setup (JavaScript)
DESCRIPTION: This JavaScript code sets up a React component using Framer Motion, simulates server-side rendering, initiates optimized appear animations for opacity and transform, and includes logic to test animation interruption and client-side hydration.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/optimized-appear/defer-handoff.html#_snippet_1

LANGUAGE: javascript
CODE:
```
const { motion, animateStyle, animate, startOptimizedAppearAnimation, optimizedAppearDataAttribute, motionValue, frame, } = window.Motion
const { matchViewportBox } = window.Assert
const root = document.getElementById("root")
const duration = 2
const x = motionValue(0)
let isFirstFrame = true
// This is the tree to be rendered "server" and client-side.
const Component = React.createElement(motion.div, {
  id: "box",
  initial: { x: 0, opacity: 0 },
  animate: { x: 100, opacity: 1 },
  transition: { duration, ease: "linear" },
  style: { x },
  /**
   * On animation start, check the values we expect to see here
   */
  onAnimationStart: () => {
    const box = document.getElementById("box")
    box.style.backgroundColor = "green"
    setTimeout(() => {
      /**
       * This animation interrupts the optimised animation. Notably, we are animating
       * x in the optimised transform animation and only scale here. This ensures
       * that any transform can force the cancellation of the optimised animation on transform,
       * not just those involved in the original animation.
       */
      animate(
        box,
        { scale: 2, opacity: 0.1 },
        { duration: 0.3, ease: "linear" }
      ).then(() => {
        frame.postRender(() => {
          if (getComputedStyle(box).opacity !== "0.1") {
            showError(
              document.getElementById("box"),
              `opacity animation didn't interrupt optimised animation. Opacity was ${ getComputedStyle(box).opacity } instead of 0.1.`
            )
          }
          const { width, left } = box.getBoundingClientRect()
          if (Math.round(width) !== 200) {
            showError(
              document.getElementById("box"),
              `scale animation didn't interrupt optimised animation. Width was ${width}px instead of 200px.`
            )
          }
          if (left <= 100) {
            showError(
              document.getElementById("box"),
              `scale animation incorrectly interrupted optimised animation. Left was ${left}px instead of 100px.`
            )
          }
        })
      })
    }, 100)
  },
  [optimizedAppearDataAttribute]: "a",
  children: "Content",
})
// Emulate server rendering of element
root.innerHTML = ReactDOMServer.renderToString(Component)
// Start optimised opacity animation
startOptimizedAppearAnimation(
  document.getElementById("box"),
  "opacity",
  [0, 1],
  {
    duration: duration * 1000,
    ease: "linear",
  }
)
// Start WAAPI animation
const animation = startOptimizedAppearAnimation(
  document.getElementById("box"),
  "transform",
  ["translateX(0px)", "translateX(100px)"],
  {
    duration: duration * 1000,
    ease: "linear",
  },
  (animation) => {
    setTimeout(() => {
      ReactDOM.hydrateRoot(root, Component)
    }, (duration * 1000) / 2)
  }
)
```

----------------------------------------

TITLE: Starting Optimized Transform Animation and Hydration WAAPI
DESCRIPTION: Initiates an optimized WAAPI animation for the 'transform' property (translateX) and provides a callback function that hydrates the root element with the React component after half the animation duration, demonstrating a transition from server-rendered to client-rendered state.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/optimized-appear/persist-optimised-animation.html#_snippet_5

LANGUAGE: javascript
CODE:
```
// Start WAAPI animation
const animation = startOptimizedAppearAnimation(
  document.getElementById("box"),
  "transform",
  \["translateX(0px)", "translateX(100px)"\],
  { duration: duration * 1000, ease: "linear" },
  (animation) => {
    setTimeout(() => {
      ReactDOM.hydrateRoot(root, Component)
    }, (duration * 1000) / 2)
  }
)
```

----------------------------------------

TITLE: Starting Optimized Transform Animation and Hydrating (Framer Motion/ReactDOM)
DESCRIPTION: Starts an optimized appear animation for the transform property (translateX) of the box element. It also includes a callback that triggers client-side hydration using `ReactDOM.hydrateRoot` after a delay, demonstrating the interaction between optimized animations and hydration.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/optimized-appear/defer-handoff-layout-uselayouteffect.html#_snippet_5

LANGUAGE: javascript
CODE:
```
// Start WAAPI animation
const animation = startOptimizedAppearAnimation(
  document.getElementById("box"),
  "transform",
  \["translateX(0px)", "translateX(100px)"\],
  {
    duration: duration * 1000,
    ease: "linear",
  },
  (animation) => {
    setTimeout(() => {
      ReactDOM.hydrateRoot(
        root,
        React.createElement(Component)
      )
    }, (duration * 1000) / 2)
  }
)
```

----------------------------------------

TITLE: Defining React Component with Framer Motion (JavaScript)
DESCRIPTION: Creates a React functional component that renders a Framer Motion div. It uses `initial` and `animate` props for animation, a `motionValue` for the x-transform, and includes a blocking `useLayoutEffect` for testing purposes. It also sets up a change listener on the `motionValue`.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/optimized-appear/resync-delay.html#_snippet_1

LANGUAGE: javascript
CODE:
```
const { motion, animateStyle, startOptimizedAppearAnimation, optimizedAppearDataAttribute, motionValue, } = window.Motion
const { matchViewportBox } = window.Assert
const root = document.getElementById("root")
const delay = 0.25
const duration = 0.5
const x = motionValue(0)
x.on("change", (latest) => {
  if (latest < 100) {
    showError(
      document.getElementById("box"),
      `x transform should never be less than 100`
    )
  }
})
// This is the tree to be rendered "server" and client-side.
const Component = React.createElement(() => {
  React.useLayoutEffect(() => {
    const startTime = performance.now()
    while (performance.now() - startTime < 500) {}
  })
  return React.createElement(motion.div, {
    id: "box",
    initial: { x: 0, opacity: 0 },
    animate: { x: 100, opacity: 1 },
    transition: { delay, duration, ease: "linear" },
    style: { x },
    /**
     * On animation start, check the values we expect to see here
     */
    onAnimationStart: () => {
      // matchViewportBox(document.getElementById("box"), {
      // top: 100,
      // right: 250,
      // left: 150,
      // bottom: 200,
      // })
    },
    \[optimizedAppearDataAttribute\]: "a",
  })
})
```

----------------------------------------

TITLE: Defining React Component with Framer Motion Animation
DESCRIPTION: Creates a React element using motion.div with initial and animate properties for position and opacity, a linear transition, and a linked motion value for the x-coordinate. Includes complex onAnimationStart logic to check animation state, block the main thread, and trigger a conflicting animation.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/optimized-appear/defer-handoff-block.html#_snippet_2

LANGUAGE: javascript
CODE:
```
// This is the tree to be rendered "server" and client-side.
const Component = React.createElement(motion.div, {
  id: "box",
  initial: { x: 0, opacity: 0 },
  animate: { x: xTarget, opacity: 1 },
  transition: { duration, ease: "linear" },
  style: { x },
  /**
   * On animation start, check the values we expect to see here
   */
  onAnimationStart: () => {
    const box = document.getElementById("box")
    box.style.backgroundColor = "green"
    setTimeout(() => {
      frame.postRender(() => {
        /**
         * The frame visible after the infinite loop
         * and before motion renders again
         */
        frame.preRender(() => {
          const left = box.getBoundingClientRect().left
          if (left < 200) {
            showError(
              document.getElementById("box"),
              `Stutter detected`
            )
          }
        })
      })
      /**
       * By blocking the main thread here, we ensure that
       * the keyframes are resolved a good duration after the
       * animation was initialised.
       */
      frame.postRender(() => {
        console.log(
          "Blocking main thread before keyframe resolution"
        )
        const startTime = performance.now()
        while (performance.now() - startTime < 1000) {}
      })
      /**
       * This animation interrupts the optimised animation. Notably, we are animating
       * x in the optimised transform animation and only scale here. This ensures
       * that any transform can force the cancellation of the optimised animation on transform,
       * not just those involved in the original animation.
       */
      const options = {
        duration: 0.5,
        ease: "linear"
      }
      let frameCounter = 0
      box.style.backgroundColor = "red"
      animate(
        box,
        { scale: [1, 1] },
        {
          ...options,
          onUpdate: () => {
            frameCounter++
            console.log(
              getComputedStyle(box).transform,
              box.style.transform,
              box.getBoundingClientRect().left
            )
          },
        }
      )
    }, 100)
  },
  [optimizedAppearDataAttribute]: "a",
  children: "Content",
})
```

----------------------------------------

TITLE: Defining Framer Motion React Component
DESCRIPTION: Creates a React element using `React.createElement` that represents a Framer Motion animated div. It defines initial and animate states, a transition, links a motion value to the style, includes an `onAnimationStart` callback for validation, and sets a data attribute for optimized appear animations.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/optimized-appear/persist-optimised-animation.html#_snippet_2

LANGUAGE: javascript
CODE:
```
// This is the tree to be rendered "server" and client-side.
const Component = React.createElement(motion.div, {
  id: "box",
  initial: { x: 0, opacity: 0 },
  animate: { x: 100, opacity: 1 },
  transition: { duration, ease: "linear" },
  style: { x },
  /**
   * On animation start, check the values we expect to see here
   */
  onAnimationStart: () => {
    const box = document.getElementById("box")
    box.style.backgroundColor = "green"
    setTimeout(() => {
      box.style.transform = "scale(2)"
      const { width } = box.getBoundingClientRect()
      if (width !== 100) {
        showError(
          document.getElementById("box"),
          \`Setting transform became visible, which means optimised animation has been prematurely cancelled. Width was ${width}px instead of 200px.\`
        )
      }
    }, 100)
  },
  \[optimizedAppearDataAttribute\]: "a",
  children: "Content",
})
```

----------------------------------------

TITLE: Animating Layout Transition with Framer Motion (JavaScript)
DESCRIPTION: Demonstrates creating animation nodes for elements using a library like Framer Motion. It sets initial properties, unmounts the old node, mounts the new one with the same layoutId to trigger a transition, and performs assertions on the final state after rendering.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/shared-promote-new-mix-remove.html#_snippet_1

LANGUAGE: javascript
CODE:
```
const { createNode } = window.Animate
const { matchViewportBox, matchVisibility, matchOpacity, matchBorderRadius, } = window.Assert
const { frame } = window.Projection
const box = document.getElementById("box-a")
const boxProjection = createNode(box, undefined, { layoutId: "a" })
boxProjection.setValue("opacity", 0.8)
boxProjection.willUpdate()
const newBox = document.createElement("div")
newBox.id = "box-b"
document.body.appendChild(newBox)
const newBoxProjection = createNode(newBox, undefined, { layoutId: "a", })
newBoxProjection.setValue("borderRadius", 20)
boxProjection.unmount()
document.body.removeChild(box)
newBoxProjection.root.didUpdate()
frame.postRender(() => {
  const midBox = { bottom: 250, left: 50, right: 200, top: 50 }
  matchViewportBox(newBox, midBox)
  matchVisibility(newBox, "visible")
  /**
   * Should animate from the old opacity to the new one
   * IMPORTANT: Don't make the previous opacity something non-default
   */
  matchOpacity(newBox, 0.9)
  matchBorderRadius(newBox, "6.66667% / 5%")
})
```

----------------------------------------

TITLE: Framer Motion Optimized Appear vs Layout Animation - JavaScript
DESCRIPTION: React component and setup demonstrating how Framer Motion's optimized appear animations (started before hydration) interact with sibling layout animations. It tests if optimized animations are cancelled when a layout animation occurs on a sibling element.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/optimized-appear/defer-handoff-layout-sibling.html#_snippet_1

LANGUAGE: javascript
CODE:
```
const { motion, animateStyle, animate, startOptimizedAppearAnimation, optimizedAppearDataAttribute, motionValue, frame, } = window.Motion
const { matchViewportBox } = window.Assert
const root = document.getElementById("root")
const duration = 0.5
const x = motionValue(0)
let isFirstFrame = true

function Component() {
  const \[top, setTop\] = React.useState(0)

  React.useEffect(() => {
    setTimeout(() => {
      setTop(100)
    }, 250)
  }, \[\])

  return React.createElement("div", {
    id: "container",
    children: \[ React.createElement(motion.div, {
      id: "optimised-box",
      className: "box",
      initial: {
        x: 0,
        opacity: 0,
        backgroundColor: "#00f",
      },
      animate: {
        x: 100,
        opacity: 1,
        backgroundColor: "#00f",
      },
      transition: {
        duration,
        ease: "linear",
      },
      style: {
        x,
        position: "relative",
      },
      onAnimationComplete: () => {
        const box = document.getElementById("optimised-box")
        const color = window.getComputedStyle(box).backgroundColor
        if (color !== "rgb(255, 0, 0)") {
          console.log(color)
          showError(
            box,
            \`optimised animations cancelled by sibling layout animations\`
          )
        }
      },
      \[optimizedAppearDataAttribute\]: "a",
      children: "Content",
    }), React.createElement(motion.div, {
      id: "layout-box",
      className: "box",
      transition: {
        duration,
        ease: "linear",
        layout: {
          ease: () => 1,
          duration: 10
        },
      },
      style: {
        top,
        position: "relative",
        background: top ? "red" : "blue",
      },
      layout: true,
      children: "Content",
    }), \],
  })
}

// Emulate server rendering of element
root.innerHTML = ReactDOMServer.renderToString(
  React.createElement(Component)
)

// Start optimised opacity animation
startOptimizedAppearAnimation(
  document.getElementById("optimised-box"),
  "opacity",
  \[0, 1\],
  {
    duration: duration \* 1000,
    ease: "linear",
  }
)

// Start optimised background-color animation
// This optimised animation is red -> red whereas the animation on the
// motion.div is blue -> blue. Therefore, if the optimised animations
// cancelled, the rendered color will be blue.
startOptimizedAppearAnimation(
  document.getElementById("optimised-box"),
  "backgroundColor",
  \["#f00", "#f00"\],
  {
    duration: duration \* 1000,
    ease: "linear",
  }
)

// Start WAAPI animation
const animation = startOptimizedAppearAnimation(
  document.getElementById("optimised-box"),
  "transform",
  \["translateX(0px)", "translateX(100px)"\],
  {
    duration: duration \* 1000,
    ease: "linear",
  },
  (animation) => {
    setTimeout(() => {
      ReactDOM.hydrateRoot(
        root,
        React.createElement(Component)
      )
    }, (duration \* 1000) / 4)
  }
)
```

----------------------------------------

TITLE: Animating a 3D Box Geometry with Framer Motion 3D
DESCRIPTION: This snippet demonstrates how to apply a simple animation to a 3D box geometry component using Framer Motion 3D within a React Three Fiber application. The `animate` prop is used to define the target animation state.
SOURCE: https://github.com/grx7/framer-motion/blob/main/packages/framer-motion-3d/README.md#_snippet_0

LANGUAGE: JSX
CODE:
```
<motion.boxGeometry animate={{ x: 0 }} />
```

----------------------------------------

TITLE: Creating and Animating Projection Nodes (JavaScript)
DESCRIPTION: Demonstrates creating projection nodes for existing and new DOM elements using `createNode`. It sets animated values like opacity, border radius, and rotation, and uses `frame.postRender` to assert the final state of the elements after layout updates.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/shared-promote-new-mix-rotate.html#_snippet_1

LANGUAGE: javascript
CODE:
```
const { createNode } = window.Animate
const { matchViewportBox, matchVisibility, matchOpacity, matchBorderRadius, matchRotate,
} = window.Assert
const { frame } = window.Projection
const box = document.getElementById("box-a")
const boxProjection = createNode(box, undefined, { layoutId: "a" })
boxProjection.willUpdate()
const newBox = document.createElement("div")
newBox.id = "box-b"
document.body.appendChild(newBox)
const newBoxProjection = createNode(newBox, undefined, { layoutId: "a", })
boxProjection.setValue("opacity", 0.8)
newBoxProjection.setValue("borderRadius", 20)
newBoxProjection.setValue("rotate", 40)
newBoxProjection.root.didUpdate()
frame.postRender(() => {
  const bbox = newBox.getBoundingClientRect()
  matchViewportBox(box, bbox)
  matchVisibility(box, "visible")
  matchVisibility(newBox, "visible")
  matchOpacity(box, 0.8)
  matchOpacity(newBox, 1)
  matchRotate(box, 20)
  matchRotate(newBox, 20)
  matchBorderRadius(box, "6.66667% / 5%")
  matchBorderRadius(newBox, "6.66667% / 5%")
})
```

----------------------------------------

TITLE: Creating and Animating Elements with Framer Motion
DESCRIPTION: Generates a specified number of div elements containing a 'box' div, inserts them into the '.container' element, selects all elements with the class 'box', and then applies two sequences of animations using Framer Motion's `animate` function with delays, demonstrating animating properties like rotation, color, width (with different units), and horizontal position.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/benchmarks/cold-start-framer-motion.html#_snippet_1

LANGUAGE: javascript
CODE:
```
// Create boxes const numBoxes = 200 let html = `` for (let i = 0; i < numBoxes; i++) { html += `<div><div class="box"></div></div>` } document.querySelector(".container").innerHTML = html const { animate } = Motion const boxes = document.querySelectorAll(".box") setTimeout(() => { // Cold start (read from DOM) boxes.forEach((box) => animate( box, { rotate: Math.random() * 360, backgroundColor: "#f00", width: Math.random() * 100 + "%", x: 5 }, { ease: "linear", duration: 1 } ) ) setTimeout(() => { // Value conversion boxes.forEach((box) => animate( box, { width: Math.random() * 100 + "px", x: "10%" }, { ease: "linear", duration: 1 } ) ) }, 1500) }, 1000)
```

----------------------------------------

TITLE: Server-Side Rendering React Component
DESCRIPTION: Simulates server-side rendering by converting the React component to an HTML string using `ReactDOMServer.renderToString` and setting it as the inner HTML of the root element.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/optimized-appear/persist-optimised-animation.html#_snippet_3

LANGUAGE: javascript
CODE:
```
// Emulate server rendering of element
root.innerHTML = ReactDOMServer.renderToString(Component)
```

----------------------------------------

TITLE: Starting Optimized Opacity Animation (Framer Motion)
DESCRIPTION: Initiates an optimized appear animation for the opacity property of the box element, animating it from 0 to 1 over a specified duration with a linear ease.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/optimized-appear/defer-handoff-layout-uselayouteffect.html#_snippet_4

LANGUAGE: javascript
CODE:
```
// Start optimised opacity animation
startOptimizedAppearAnimation(
  document.getElementById("box"),
  "opacity",
  \[0, 1\],
  {
    duration: duration * 1000,
    ease: "linear",
  }
)
```

----------------------------------------

TITLE: Start Optimized Transform Animation and Hydrate
DESCRIPTION: Initiates an optimized animation for the 'transform' property (translateX) of the box element using `startOptimizedAppearAnimation`. It also includes a callback that triggers client-side hydration of the React component tree using `ReactDOM.hydrateRoot` after a delay.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/optimized-appear/defer-handoff-layout.html#_snippet_5

LANGUAGE: javascript
CODE:
```
// Start WAAPI animation
const animation = startOptimizedAppearAnimation(
  document.getElementById("box"),
  "transform",
  ["translateX(0px)", "translateX(100px)"],
  {
    duration: duration * 1000,
    ease: "linear",
  },
  (animation) => {
    setTimeout(() => {
      ReactDOM.hydrateRoot(
        root,
        React.createElement(Component)
      )
    }, (duration * 1000) / 4)
  }
)
```

----------------------------------------

TITLE: Animating Properties with Framer Motion (JavaScript)
DESCRIPTION: Uses Framer Motion's `animate` function to animate the opacity and background color of the selected elements with a specified duration and infinite repetition.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/benchmarks/pregenerated-background-color.html#_snippet_4

LANGUAGE: javascript
CODE:
```
Motion.animate(
  boxes,
  { opacity: [0, 1], backgroundColor: ["#f00", "#00f"] },
  { duration: duration / 1000, repeat: Infinity }
)
```

----------------------------------------

TITLE: Layout Projection and Assertion Setup (JavaScript)
DESCRIPTION: Initializes variables from global objects (Undo, Assert, Projection), selects a sticky element, creates a projection node for it, scrolls the page, creates and appends a fixed element, creates a projection node for the fixed element, and sets a timeout to assert viewport box matches after a delay.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/sticky-shared-to-fixed-page-scroll-stick.html#_snippet_1

LANGUAGE: javascript
CODE:
```
const { createNode } = window.Undo
const { matchViewportBox, matchVisibility, matchOpacity, addPageScroll,
} = window.Assert
const { frame } = window.Projection
const sticky = document.querySelector(".sticky")
const stickyProjection = createNode(sticky, undefined, { layoutId: "sticky", })
const stickyOrigin = sticky.getBoundingClientRect()
stickyProjection.willUpdate()
const scrollOffset = \[50, 300\]
window.scrollTo(...scrollOffset)
const fixed = document.createElement("div")
fixed.classList.add("fixed")
document.body.appendChild(fixed)
const fixedProjection = createNode(fixed, undefined, { layoutId: "sticky", })
fixedProjection.root.didUpdate()
setTimeout(() => {
  matchViewportBox(sticky, addPageScroll(stickyOrigin, 50, 300))
  matchViewportBox(fixed, addPageScroll(stickyOrigin, 50, 300))
}, 50)
```

----------------------------------------

TITLE: Starting Optimized Transform Animation (JavaScript)
DESCRIPTION: Initiates an optimized appear animation for the 'transform' property of the box element using `startOptimizedAppearAnimation`. It animates from 'translateX(0px)' to 'translateX(100px)' and hydrates the React component after the animation duration.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/optimized-appear/resync-delay.html#_snippet_2

LANGUAGE: javascript
CODE:
```
// Emulate server rendering of element
root.innerHTML = ReactDOMServer.renderToString(Component)
const options = {
  delay: delay * 1000,
  duration: duration * 1000,
  ease: "linear",
}
startOptimizedAppearAnimation(
  document.getElementById("box"),
  "transform",
  ["translateX(0px)", "translateX(100px)"],
  options,
  (animation) => {
    setTimeout(() => {
      ReactDOM.hydrateRoot(root, Component)
    }, duration * 1000)
  }
)
```

----------------------------------------

TITLE: Implementing Optimized Appear Animation with Framer Motion/React - JavaScript
DESCRIPTION: Initializes dependencies from global objects, defines spring physics parameters, creates a React component using `motion.div` with initial/animate states and a spring transition, includes an animation start check, renders the component server-side, generates spring keyframes based on physics, and starts a Motion One optimized appear animation with hydration triggered mid-animation.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/optimized-appear/interrupt-spring.html#_snippet_1

LANGUAGE: javascript
CODE:
```
const { motion, animateStyle, startOptimizedAppearAnimation, optimizedAppearDataAttribute, motionValue, spring, } = window.Motion
const { matchViewportBox, matchOpacity } = window.Assert
const root = document.getElementById("root")
const stiffness = 300
const damping = 40
const mass = 1
// This is the tree to be rendered "server" and client-side.
const Component = React.createElement(motion.div, {
  id: "box",
  initial: { y: 0, scale: 1 },
  animate: { y: 100, scale: 2 },
  transition: { type: "spring", stiffness, damping, mass },
  /**
   * On animation start, check the values we expect to see here
   */
  onAnimationStart: () => {
    const { top, left } = document
      .getElementById("box")
      .getBoundingClientRect()
    if (top < 110 || top > 140 || left < 60 || left > 90) {
      showError(box, `unexpected viewport box`)
    }
  },
  style: { willChange: "opacity" },
  \[optimizedAppearDataAttribute\]: "a",
})
// Emulate server rendering of element
root.innerHTML = ReactDOMServer.renderToString(Component)
const springTimeResolution = 10
function generateSpringKeyframes(from, to) {
  let t = 0
  const keyframes = \[\]
  const springAnimation = spring({
    keyframes: \[from, to\],
    stiffness,
    damping,
    mass,
  })
  let state = { done: false, value: from }
  while (!state.done) {
    state = springAnimation.next(t)
    keyframes.push(state.value)
    t += springTimeResolution
  }
  return keyframes
}
const yKeyframes = generateSpringKeyframes(0, 100)
const scaleKeyframes = generateSpringKeyframes(1, 2)
const maxKeyframes = Math.max(
  yKeyframes.length,
  scaleKeyframes.length
)
const transformOptions = {
  duration: maxKeyframes * springTimeResolution,
  ease: "linear",
}
const transformKeyframes = \[\]
for (let i = 0; i < maxKeyframes; i++) {
  transformKeyframes.push(
    `translateY(${ yKeyframes\[Math.min(i, yKeyframes.length - 1)\] }px) scale(${ scaleKeyframes\[Math.min(i, scaleKeyframes.length - 1)\] })`
  )
}
// Start Motion One animations
const animation = startOptimizedAppearAnimation(
  document.getElementById("box"),
  "transform",
  transformKeyframes,
  transformOptions,
  (animation) => {
    // Hydrate root mid-way through animation
    setTimeout(() => {
      ReactDOM.hydrateRoot(root, Component)
    }, 100)
  }
)
```

----------------------------------------

TITLE: Start Optimized Opacity Animation
DESCRIPTION: Initiates an optimized animation for the 'opacity' property of the box element using `startOptimizedAppearAnimation` with a linear ease and specified duration.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/optimized-appear/defer-handoff-layout.html#_snippet_4

LANGUAGE: javascript
CODE:
```
// Start optimised opacity animation
startOptimizedAppearAnimation(
  document.getElementById("box"),
  "opacity",
  [0, 1],
  {
    duration: duration * 1000,
    ease: "linear",
  }
)
```

----------------------------------------

TITLE: Starting Optimized Appear Animation (JavaScript)
DESCRIPTION: Initiates Framer Motion's `startOptimizedAppearAnimation` on the server-rendered element. It targets the 'opacity' property, specifies the animation range (0 to 1), duration, ease, and delay, and provides a callback function to execute upon animation completion.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/optimized-appear/interrupt-delay-before.html#_snippet_4

LANGUAGE: javascript
CODE:
```
startOptimizedAppearAnimation(
  document.getElementById("box"),
  "opacity",
  \[0, 1\],
  {
    duration: duration * 1000,
    ease: "linear",
    delay: delay * 1000,
  },
  (animation) => {
    // Hydrate root mid-way through delay
    setTimeout(() => {
      ReactDOM.hydrateRoot(root, Component)
      const { opacity: initialOpacity } = window.getComputedStyle(box)
      if (initialOpacity !== "0") {
        showError(box, `opacity should have animated`)
      }
    }, 300)
  }
)
```

----------------------------------------

TITLE: Benchmarking Framer Motion Mix Function in JavaScript
DESCRIPTION: This JavaScript snippet demonstrates and benchmarks the `mix` function from Framer Motion, creating an interpolator for mixing various unit types and measuring the performance of a million interpolation calls.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/benchmarks/mix-object-greensock.html#_snippet_1

LANGUAGE: javascript
CODE:
```
const { mix } = window.Motion

/**
 * Create an interpolate function that mixes unit values.
 */
const mixer = mix(
  [100, "50vh", "100px", "#fff"],
  [0, "0vh", "0px", "#000"]
)

const numRuns = 1000000
let startTime = performance.now()

for (let i = 0; i < numRuns; i++) {
  mixer(i / numRuns)
}

const finish = performance.now() - startTime

console.log(`Total time: ${finish}ms`)
```

----------------------------------------

TITLE: Starting Optimized Opacity Animation WAAPI
DESCRIPTION: Initiates an optimized Web Animations API (WAAPI) animation for the 'opacity' property of the 'box' element, transitioning from 0 to 1 over the specified duration with a linear ease.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/optimized-appear/persist-optimised-animation.html#_snippet_4

LANGUAGE: javascript
CODE:
```
// Start optimised opacity animation
startOptimizedAppearAnimation(
  document.getElementById("box"),
  "opacity",
  \[0, 1\],
  { duration: duration * 1000, ease: "linear" }
)
```

----------------------------------------

TITLE: Starting Optimized Appear Animations (JavaScript)
DESCRIPTION: Demonstrates starting optimized appear animations for `opacity` and `backgroundColor` properties on the target element using `startOptimizedAppearAnimation`. It highlights a potential conflict with the `backgroundColor` animation defined in the component's `animate` prop.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/optimized-appear/defer-handoff-layout-ancestor.html#_snippet_4

LANGUAGE: JavaScript
CODE:
```
// Start optimised opacity animation
startOptimizedAppearAnimation(
  document.getElementById("optimised-box"),
  "opacity",
  \[0, 1\],
  {
    duration: duration * 1000,
    ease: "linear",
  }
)

// Start optimised background-color animation
// This optimised animation is red -> red whereas the animation on the
// motion.div is blue -> blue. Therefore, if the optimised animations
// cancelled, the rendered color will be blue.
startOptimisedAppearAnimation(
  document.getElementById("optimised-box"),
  "backgroundColor",
  \["#f00", "#f00"\]
  {
    duration: duration * 1000,
    ease: "linear",
  }
)
```

----------------------------------------

TITLE: Initializing and Testing Element Projection (JavaScript)
DESCRIPTION: Retrieves necessary functions from global objects, gets the box element, creates a projection node, records its initial position, updates the element's class, updates the projection node, and schedules a post-render check to match the viewport box.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/single-element-layout-change.html#_snippet_1

LANGUAGE: javascript
CODE:
```
const { createNode } = window.Undo
const { matchViewportBox } = window.Assert
const { frame } = window.Projection
const box = document.getElementById("box")
const boxProjection = createNode(box)
const boxOrigin = box.getBoundingClientRect()
boxProjection.willUpdate()
box.classList.add("b")
boxProjection.root.didUpdate()
frame.postRender(() => {
  matchViewportBox(box, boxOrigin)
})
```

----------------------------------------

TITLE: Creating and Animating Boxes with JavaScript and Framer Motion
DESCRIPTION: Generates a specified number of box elements, adds them to the DOM, selects them, and then uses Framer Motion's `animate` function within a `setTimeout` to apply random rotation, background color, width, and x-position animations to each box.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/benchmarks/warm-start-framer-motion.html#_snippet_1

LANGUAGE: javascript
CODE:
```
// Create boxes
const numBoxes = 100
let html = ``
for (let i = 0; i < numBoxes; i++) {
  html += `<div><div class="box"></div></div>`
}
document.querySelector(".container").innerHTML = html

const { animate } = Motion
const boxes = document.querySelectorAll(".box")

setTimeout(() => {
  // Warm start
  boxes.forEach((box) =>
    animate(
      box,
      {
        rotate: [0, Math.random() * 360],
        backgroundColor: ["#fff", "#f00"],
        width: ["0%", Math.random() * 100 + "%"],
        x: [0, 5]
      },
      {
        easing: "linear",
        duration: 1
      }
    )
  )
}, 1000)
```

----------------------------------------

TITLE: Setting up Framer Motion Projection Test (JavaScript)
DESCRIPTION: Initializes Framer Motion projection nodes for the parent, middle, and child elements. It captures the child's initial position, applies the 'b' class to trigger the animation, updates projections, and then uses a timeout to remove the 'b' class, triggering another update. Finally, it asserts the child's position relative to the middle element after the animation.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/animate-relative-skip-parent.html#_snippet_1

LANGUAGE: javascript
CODE:
```
const { createNode, relativeEase } = window.Animate
const { matchViewportBox } = window.Assert
const { frame } = window.Projection
const parent = document.getElementById("parent")
const mid = document.getElementById("mid")
const child = document.getElementById("child")
const childOrigin = child.getBoundingClientRect()
const parentProjection = createNode(
  parent,
  undefined,
  { layoutScroll: true },
  { duration: 0.01 }
)
const midProjection = createNode(
  mid,
  parentProjection,
  {},
  { duration: 0.01, delay: 0 }
)
const childProjection = createNode(
  child,
  midProjection,
  {},
  { duration: 10, delay: 0.1 }
  // { duration: 0 }
)
parentProjection.willUpdate()
midProjection.willUpdate()
childProjection.willUpdate()
parent.classList.add("b")
parentProjection.root.didUpdate()
setTimeout(() => {
  parentProjection.willUpdate()
  midProjection.willUpdate()
  childProjection.willUpdate()
  parent.classList.remove("b")
  parentProjection.root.didUpdate()
  frame.postRender(() => {
    matchViewportBox(child, childOrigin, 0.5)
  })
}, 150)
```

----------------------------------------

TITLE: Testing Framer Motion Layout Projection (JavaScript)
DESCRIPTION: Uses internal Framer Motion testing utilities (Undo, Assert, Projection) to create projection nodes for existing and newly created elements (#box-a and #box-b). It simulates a layout update, sets a value (rotate), and then uses assertions to check the final viewport box, visibility, and opacity of the elements after the update.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/shared-promote-new-rotate.html#_snippet_1

LANGUAGE: javascript
CODE:
```
const { createNode } = window.Undo
const { matchViewportBox, matchVisibility, matchOpacity } = window.Assert
const { frame } = window.Projection
const box = document.getElementById("box-a")
const boxProjection = createNode(box, undefined, { layoutId: "a" })
const boxOrigin = box.getBoundingClientRect()
boxProjection.willUpdate()
const newBox = document.createElement("div")
newBox.id = "box-b"
document.body.appendChild(newBox)
const newBoxProjection = createNode(newBox, undefined, { layoutId: "a", crossfade: false, })
newBoxProjection.setValue("rotate", 45)
newBoxProjection.root.didUpdate()
setTimeout(() => {
  matchViewportBox(box, boxOrigin)
  matchViewportBox(newBox, boxOrigin)
  matchVisibility(box, "hidden")
  matchVisibility(newBox, "visible")
  matchOpacity(newBox, 1)
}, 50)
```

----------------------------------------

TITLE: Emulate Server-Side Rendering
DESCRIPTION: Simulates server-side rendering by rendering the React component to an HTML string using `ReactDOMServer.renderToString` and setting it as the inner HTML of the root element.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/optimized-appear/defer-handoff-layout.html#_snippet_3

LANGUAGE: javascript
CODE:
```
// Emulate server rendering of element
root.innerHTML = ReactDOMServer.renderToString(
  React.createElement(Component)
)
```

----------------------------------------

TITLE: GSAP Interpolate Function Example
DESCRIPTION: Demonstrates the use of `gsap.utils.interpolate` to mix unit values (CSS variables and pixel values). It then runs a loop to test the function and measures performance.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/benchmarks/mix-unit-value-greensock.html#_snippet_1

LANGUAGE: javascript
CODE:
```
/** * Create an interpolate function that mixes unit values. */
const px = gsap.utils.interpolate(
  "var(--test-1, 1) 10px",
  "var(--test-9, 3) 60px"
)
const numRuns = 10
let startTime = performance.now()
for (let i = 0; i < numRuns; i++) {
  console.log(px(i / numRuns))
}
console.log(`First run: ${performance.now() - startTime}ms`)
```

----------------------------------------

TITLE: Run Bootstrap Command (Shell)
DESCRIPTION: Executes the 'bootstrap' target defined in the project's Makefile to set up the local development environment, typically involving installing dependencies and linking packages.
SOURCE: https://github.com/grx7/framer-motion/blob/main/CONTRIBUTING.md#_snippet_0

LANGUAGE: Shell
CODE:
```
make bootstrap
```

----------------------------------------

TITLE: GSAP Interpolate Utility Performance Test
DESCRIPTION: Demonstrates the usage of `gsap.utils.interpolate` to create a function that mixes unit values and measures its performance over a large number of iterations.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/benchmarks/mix-array-greensock.html#_snippet_1

LANGUAGE: javascript
CODE:
```
/\*\*\n \* Create an interpolate function that mixes unit values.\n \*\/\nconst px = gsap.utils.interpolate(
  [100, 100, 100, 100],
  [0, 0, 0, 0]
);

const numRuns = 1000000;
let startTime = performance.now();

for (let i = 0; i < numRuns; i++) {
  px(i / numRuns);
}

console.log(`First run: ${performance.now() - startTime}ms`);
```

----------------------------------------

TITLE: Emulating Server-Side Rendering with ReactDOMServer
DESCRIPTION: Renders the defined React component to an HTML string using ReactDOMServer.renderToString and sets it as the innerHTML of the root element, simulating server-side rendering output.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/optimized-appear/defer-handoff-block.html#_snippet_3

LANGUAGE: javascript
CODE:
```
// Emulate server rendering of element
root.innerHTML = ReactDOMServer.renderToString(Component)
```

----------------------------------------

TITLE: Server-Side Rendering Component (ReactDOMServer)
DESCRIPTION: Simulates server-side rendering by using `ReactDOMServer.renderToString` to render the `Component` to an HTML string and setting this string as the inner HTML of the root element.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/optimized-appear/defer-handoff-layout-uselayouteffect.html#_snippet_3

LANGUAGE: javascript
CODE:
```
// Emulate server rendering of element
root.innerHTML = ReactDOMServer.renderToString(
  React.createElement(Component)
)
```

----------------------------------------

TITLE: Benchmarking Framer Motion Mix Function
DESCRIPTION: Imports the `mix` function from Framer Motion, creates a specific interpolation function `px` for mixing values between 0 and 100, and then runs a performance benchmark over 100 million iterations to measure its speed.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/benchmarks/mix-number-value-framer-motion.html#_snippet_1

LANGUAGE: javascript
CODE:
```
const { mix } = window.Motion
/**
 * Create an interpolate function that mixes unit values.
 */
const px = mix(0, 100)
const numRuns = 100000000
let startTime = performance.now()
for (let i = 0; i < numRuns; i++) {
  px(i / numRuns)
}
console.log(`First run: ${performance.now() - startTime}ms`)

```

----------------------------------------

TITLE: Initializing Motion Variables and State (JavaScript)
DESCRIPTION: Imports necessary utilities from Framer Motion and an assertion library. It defines constants for animation timing, creates a motionValue for opacity, and sets up a listener to verify the initial opacity value during rendering.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/optimized-appear/interrupt-delay-before.html#_snippet_1

LANGUAGE: javascript
CODE:
```
const { motion, animateStyle, startOptimizedAppearAnimation, optimizedAppearDataAttribute, motionValue, } = window.Motion
const { matchOpacity } = window.Assert
const root = document.getElementById("root")
const duration = 0.25
const delay = 0.5
const opacity = motionValue(0)
let opacityHasChanged = false
opacity.on("render", (v) => {
  if (!opacityHasChanged) {
    if (v > 0) {
      showError(
        document.getElementById("box"),
        `opacity should not start animating beyond 0 (started at ${v})`
      )
    }
  }
  opacityHasChanged = true
})
```

----------------------------------------

TITLE: Framer Motion Optimized Appear Animation Test (JavaScript)
DESCRIPTION: Sets up a test for Framer Motion's optimized appear animation. It uses a motion value for opacity, defines a React component, emulates server-side rendering, starts the animation, and hydrates the root mid-animation to check behavior. Includes checks for unexpected opacity values.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/optimized-appear/interrupt-delay-after.html#_snippet_1

LANGUAGE: javascript
CODE:
```
const { motion, animateStyle, startOptimizedAppearAnimation, optimizedAppearDataAttribute, motionValue, } = window.Motion
const { matchOpacity } = window.Assert
const root = document.getElementById("root")
const duration = 0.5
const opacity = motionValue(0)
let opacityHasChanged = false
opacity.on("render", (v) => {
  if (!opacityHasChanged) {
    if (v > 0.6) {
      showError(
        document.getElementById("box"),
        \`opacity should not start animating beyond 0.6 (started at ${v})\`
      )
    }
  }
  opacityHasChanged = true
  if (v < 0.5) {
    showError(
      document.getElementById("box"),
      "opacity should never be less than 0.25"
    )
  }
})
// This is the tree to be rendered "server" and client-side.
const Component = React.createElement(motion.div, {
  id: "box",
  initial: { opacity: 0 },
  animate: { opacity: 1 },
  transition: { duration, ease: "linear", delay: 0.25 },
  style: { opacity },
  \[optimizedAppearDataAttribute\]: "a",
})
// Emulate server rendering of element
root.innerHTML = ReactDOMServer.renderToString(Component)
const box = document.getElementById("box")
function checkOpacity() {}
startOptimizedAppearAnimation(
  box,
  "opacity",
  \[0, 1\],
  {
    duration: duration * 1000,
    ease: "linear",
    delay: 250,
  },
  (animation) => {
    // Hydrate root mid-way through animation
    setTimeout(() => {
      ReactDOM.hydrateRoot(root, Component)
      const { opacity: initialOpacity } = window.getComputedStyle(box)
      if (initialOpacity === "0") {
        showError(box, \`opacity should have animated\`)
      }
    }, 500)
  }
)
```

----------------------------------------

TITLE: Starting Optimized Opacity Appear Animation
DESCRIPTION: Initiates a Web Animations API (WAAPI) optimized animation for the opacity property of the #box element, transitioning from 0 to 1 over a specified duration with a linear ease.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/optimized-appear/defer-handoff-block.html#_snippet_4

LANGUAGE: javascript
CODE:
```
// Start optimised opacity animation
startOptimizedAppearAnimation(
  document.getElementById("box"),
  "opacity",
  [0, 1],
  {
    duration: duration * 1000,
    ease: "linear",
  }
)
```

----------------------------------------

TITLE: Framer Motion Color Interpolation Performance Test (JavaScript)
DESCRIPTION: Demonstrates the usage of Framer Motion's `interpolate` function to create a color mixer and measures the time taken to perform multiple interpolation calculations.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/benchmarks/mix-color-value-framer-motion.html#_snippet_1

LANGUAGE: javascript
CODE:
```
const { interpolate, mixColor } = window.Motion
/** * Create an interpolate function that mixes unit values. */
const mixer = interpolate(
  [0, 1],
  ["rgba(255, 255, 255, 0)", "rgba(0, 0, 0, 0)"]
)
const numRuns = 10
let startTime = performance.now()
for (let i = 0; i < numRuns; i++) {
  console.log(mixer(i / numRuns))
}
const finish = performance.now() - startTime
console.log(`Total time: ${finish}ms`)
```

----------------------------------------

TITLE: Styling Container and Elements with CSS
DESCRIPTION: Defines CSS rules for basic styling, layout (flexbox and grid), positioning, dimensions, and background colors for a container and its child elements. Includes rules for handling overflow and highlighting elements with a specific data attribute.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/flexbox-siblings-to-grid-page-scroll-interrupt.html#_snippet_0

LANGUAGE: css
CODE:
```
body { padding: 0; margin: 0; }
#container { display: flex; position: relative; top: 100px; left: 100px; width: 300px; height: 200px; }
#container.as-grid { display: grid; grid-template-columns: 50px auto; }
#a { background: #00cc88; grid-column: 2/3; }
#b { background: #0077ff; grid-column: 1/2; }
#container > div { height: 200px; flex: 1; }
#trigger-overflow { width: 1px; height: 1px; position: absolute; top: 2000px; left: 2000px; }
\[data-layout-correct="false"\] { background: #dd1144 !important; opacity: 0.5; }
```

----------------------------------------

TITLE: Styling Elements for Layout Test - CSS
DESCRIPTION: Defines basic CSS styles for the body and various box and child elements used in the layout test. It sets positioning, dimensions, background colors, and includes a class to trigger a layout change.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/shared-promote-needs-reset.html#_snippet_0

LANGUAGE: CSS
CODE:
```
body { padding: 0; margin: 0; } #box-a { position: absolute; top: 0px; left: 0px; width: 100px; height: 100px; background-color: #00cc88; } #box-b { position: absolute; top: 100px; left: 100px; width: 100px; height: 100px; background-color: #09f; } #mid-a, #mid-b { position: relative; width: 50px; height: 50px; } #child-a { position: absolute; top: 50px; width: 50px; height: 50px; background-color: #09f; } #child-b { position: absolute; top: 100px; width: 50px; height: 50px; background-color: #00cc88; } #child-a.moved { top: 100px; } #trigger-overflow { width: 1px; height: 1px; position: absolute; top: 2000px; left: 2000px; } \[data-layout-correct="false"\] { background: #dd1144 !important; opacity: 0.5; }
```

----------------------------------------

TITLE: Animating Element with Framer Motion Projection
DESCRIPTION: Uses Framer Motion's Projection API to create a projection node for a DOM element, set its initial rotation, update the rotation, trigger a layout change by adding a class, update the projection again, and verify the element's position relative to its original viewport box after the layout change.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/single-element-layout-change-rotate-change.html#_snippet_1

LANGUAGE: javascript
CODE:
```
const { createNode } = window.Undo
const { matchViewportBox } = window.Assert
const { frame } = window.Projection
const box = document.getElementById("box")
const boxProjection = createNode(box)
boxProjection.setValue("rotate", 75)
requestAnimationFrame(() => {
  const boxOrigin = box.getBoundingClientRect()
  boxProjection.setValue("rotate", 45)
  requestAnimationFrame(() => {
    boxProjection.willUpdate()
    box.classList.add("b")
    boxProjection.setValue("rotate", 75)
    boxProjection.root.didUpdate()
    frame.postRender(() => {
      matchViewportBox(box, boxOrigin)
    })
  })
})
```

----------------------------------------

TITLE: Styling Layout Elements with CSS
DESCRIPTION: Defines basic styles for body, boxes, buttons, and child elements, including positioning, dimensions, background colors, and a class for button width change. Also includes a rule for highlighting elements with incorrect layout.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/shared-promote-target.html#_snippet_0

LANGUAGE: css
CODE:
```
body { padding: 0; margin: 0; } #box-a { position: absolute; left: 100px; top: 100px; width: 200px; height: 200px; background-color: #00cc88; } #box-b { position: absolute; left: 600px; top: 100px; width: 200px; height: 200px; background-color: #09f; } #button { width: 100px; height: 100px; top: 0; left: 0; background-color: rgb(132, 0, 255); } #button.b { width: 300px; } #child-a, #child-b { position: absolute; left: 0; top: 0; width: 50px; height: 50px; background-color: yellow; } #trigger-overflow { width: 1px; height: 1px; position: absolute; top: 2000px; left: 2000px; } \[data-layout-correct="false"\] { background: #dd1144 !important; opacity: 0.5; }
```

----------------------------------------

TITLE: Styling Screen and Box Elements (CSS)
DESCRIPTION: Defines basic styles for the body, screen containers, and box elements. Includes styles for fixed positioning, scrolling, dimensions, background colors, and a specific style for elements marked with `data-layout-correct="false"` to indicate layout issues.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/shared-scroll-a-b.html#_snippet_0

LANGUAGE: CSS
CODE:
```
body { padding: 0; margin: 0; } .screen { width: 100%; height: 100%; position: fixed; inset: 0; overflow: hidden; } .scroll { overflow-y: scroll; } #box { margin-top: 1000px; width: 100px; height: 100px; background-color: #0088ff; } #box-b { position: absolute; top: 100px; left: 100px; width: 200px; height: 200px; background-color: #0088ff; } \[data-layout-correct="false"\] { background: #dd1144 !important; opacity: 0.5; }
```

----------------------------------------

TITLE: Importing Libraries and Initializing Variables JavaScript
DESCRIPTION: Imports necessary objects from global libraries (Motion and Assert) and initializes variables including the root DOM element, animation duration, a Framer Motion motion value for tracking position, and a flag.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/optimized-appear/persist-optimised-animation.html#_snippet_1

LANGUAGE: javascript
CODE:
```
const { motion, animateStyle, animate, startOptimizedAppearAnimation, optimizedAppearDataAttribute, motionValue, frame, } = window.Motion
const { matchViewportBox } = window.Assert
const root = document.getElementById("root")
const duration = 2
const x = motionValue(0)
let isFirstFrame = true
```

----------------------------------------

TITLE: Starting Optimized Transform Animation and Hydration
DESCRIPTION: Initiates a WAAPI optimized animation for the transform property (specifically translateX) of the #box element. Includes a callback that triggers ReactDOM.hydrateRoot after a delay, attaching React event handlers and logic to the server-rendered HTML.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/optimized-appear/defer-handoff-block.html#_snippet_5

LANGUAGE: javascript
CODE:
```
// Start WAAPI animation
const animation = startOptimizedAppearAnimation(
  document.getElementById("box"),
  "transform",
  ["translateX(0px)", `translateX(${xTarget}px)`],
  {
    duration: duration * 1000,
    ease: "linear",
  },
  (animation) => {
    setTimeout(() => {
      ReactDOM.hydrateRoot(root, Component)
    }, (duration * 1000) / 4)
  }
)
```

----------------------------------------

TITLE: Styling for Scroll and Box Elements (CSS)
DESCRIPTION: Provides basic styling for the body, a scrollable container (#scroll), a box element (#box), and a trigger element for overflow. Includes a rule to highlight elements where layout correction is disabled.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/element-scroll.html#_snippet_0

LANGUAGE: css
CODE:
```
body { padding: 0; margin: 0; } #scroll { overflow: scroll; position: relative; height: 200px; width: 500px; } #box { width: 100px; height: 100px; background: #00cc88; } .trigger-overflow { width: 1px; height: 1px; position: absolute; top: 2000px; left: 2000px; } \[data-layout-correct="false"\] { background: #dd1144 !important; opacity: 0.5; }
```

----------------------------------------

TITLE: Styling for Scroll and Box Elements (CSS)
DESCRIPTION: Defines basic styles for the body, a scrollable container (#scroll), a box element (#box) within it, and a helper element (.trigger-overflow) to force scrolling. Includes a style to visually indicate elements where layout correction is disabled.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/element-page-scroll-non-zero.html#_snippet_0

LANGUAGE: css
CODE:
```
body { padding: 0; margin: 0; }
#scroll { overflow: scroll; position: relative; height: 200px; width: 500px; top: 100px; }
#box { width: 200px; height: 200px; background: #00cc88; }
.trigger-overflow { width: 1px; height: 1px; position: absolute; top: 2000px; left: 2000px; }
\[data-layout-correct="false"\] { background: #dd1144 !important; opacity: 0.5; }
```

----------------------------------------

TITLE: Starting WAAPI Transform Animation and Hydrating Root (JavaScript)
DESCRIPTION: Starts a WAAPI animation for the `transform` property using `startOptimizedAppearAnimation`. It includes a callback that triggers `ReactDOM.hydrateRoot` after a delay, simulating the client-side hydration process during which animation cancellation might occur.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/optimized-appear/defer-handoff-layout-ancestor.html#_snippet_5

LANGUAGE: JavaScript
CODE:
```
// Start WAAPI animation
const animation = startOptimisedAppearAnimation(
  document.getElementById("optimised-box"),
  "transform",
  \["translateX(0px)", "translateX(100px)"\],
  {
    duration: duration * 1000,
    ease: "linear",
  },
  (animation) => {
    setTimeout(() => {
      ReactDOM.hydrateRoot(
        root,
        React.createElement(Component)
      )
    }, (duration * 1000) / 4)
  }
)
```

----------------------------------------

TITLE: Framer Motion SSR Hydration Logic (JavaScript)
DESCRIPTION: Sets up a React component using Framer Motion with initial and animate properties, including a `motionValue` for tracking. It then renders this component server-side, starts parallel WAAPI animations for transform and opacity, and finally hydrates the client-side React tree after a delay.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/optimized-appear/resync.html#_snippet_1

LANGUAGE: javascript
CODE:
```
const { motion, animateStyle, startOptimizedAppearAnimation, optimizedAppearDataAttribute, motionValue, frame, } = window.Motion;
const { matchViewportBox } = window.Assert;
const root = document.getElementById("root");
const duration = 1;
const x = motionValue(0);
x.onChange((latest) => {
  if (latest < 50) {
    showError(
      document.getElementById("box"),
      `x transform should never be less than 50`
    );
  }
});
// This is the tree to be rendered "server" and client-side.
const Component = React.createElement(() => {
  React.useLayoutEffect(() => {
    const startTime = performance.now();
    while (performance.now() - startTime < 200) {}
  });
  return React.createElement(motion.div, {
    id: "box",
    initial: { x: 0, opacity: 0 },
    animate: { x: 100, opacity: 1 },
    transition: { duration, ease: "linear" },
    style: { x },
    /**
     * On animation start, check the values we expect to see here
     */
    onAnimationStart: () => {
      frame.postRender(() => {
        frame.postRender(() => {
          const box = document.getElementById("box");
          if (!box) return;
          const { opacity } = window.getComputedStyle(box);
          if (parseFloat(opacity) < 0.65) {
            showError(
              box,
              "Resync failed with opacity: " + opacity
            );
          }
        });
      });
    },
    [optimizedAppearDataAttribute]: "a",
  });
});
// Emulate server rendering of element
root.innerHTML = ReactDOMServer.renderToString(Component);
// Start WAAPI animations
const animation = startOptimizedAppearAnimation(
  document.getElementById("box"),
  "transform",
  ["translateX(0px)", "translateX(100px)"],
  {
    duration: duration * 1000,
    ease: "linear",
  }
);
const opacityAnimation = startOptimizedAppearAnimation(
  document.getElementById("box"),
  "opacity",
  [0, 1],
  {
    duration: duration * 1000,
    ease: "linear",
  },
  () => {
    setTimeout(() => {
      ReactDOM.hydrateRoot(root, Component);
    }, (duration * 1000) / 2);
  }
);
```

----------------------------------------

TITLE: Importing Framer Motion and React Utilities (JavaScript)
DESCRIPTION: Imports necessary functions and objects from the global Framer Motion (`window.Motion`) and assertion (`window.Assert`) libraries, and initializes a motion value for tracking animation state.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/optimized-appear/defer-handoff-layout-uselayouteffect.html#_snippet_1

LANGUAGE: javascript
CODE:
```
const { motion, animateStyle, animate, startOptimizedAppearAnimation, optimizedAppearDataAttribute, motionValue, frame, } = window.Motion
const { matchViewportBox } = window.Assert
const root = document.getElementById("root")
const duration = 0.5
const x = motionValue(0)
let isFirstFrame = true
```

----------------------------------------

TITLE: Setting up Layout Projections and Animation with JavaScript
DESCRIPTION: Uses a library (Animate, Assert, Projection) to create layout nodes for existing and newly created DOM elements. It sets up shared layout IDs, triggers layout updates, appends new elements to the DOM, and includes a timeout to simulate a class change and assert the final position of an element.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/shared-promote-target.html#_snippet_1

LANGUAGE: javascript
CODE:
```
const { createNode } = window.Animate
const { matchViewportBox, matchVisibility, matchOpacity } = window.Assert
const { frame } = window.Projection

const a = document.getElementById("box-a")
const childA = document.getElementById("child-a")
const c = document.getElementById("button")

const aProjection = createNode(
  a,
  undefined,
  { layoutId: "box" },
  { duration: 0.1 }
)

const childAProjection = createNode(
  childA,
  aProjection,
  { layoutId: "child" },
  { duration: 0.1 }
)

const cProjection = createNode(
  c,
  undefined,
  { layoutId: "foo" },
  { duration: 0.1 }
)

aProjection.willUpdate()
childAProjection.willUpdate()

const b = document.createElement("div")
const childB = document.createElement("div")
b.id = "box-b"
childB.id = "child-b"
b.appendChild(childB)
document.body.appendChild(b)

const bProjection = createNode(
  b,
  undefined,
  { layoutId: "box" },
  { duration: 0.1 }
)

const childBProjection = createNode(
  childB,
  bProjection,
  { layoutId: "child" },
  { duration: 0.1 }
)

aProjection.root.didUpdate()

// After the shared animation finished
setTimeout(() => {
  cProjection.willUpdate()
  c.classList.add("b")
  cProjection.root.didUpdate()

  matchViewportBox(childB, {
    left: 600,
    top: 100,
    height: 50,
    width: 50,
  })
}, 200)
```

----------------------------------------

TITLE: Styling Elements with CSS
DESCRIPTION: Defines basic styles for body, screen containers, and specific box elements (#box, #box-b), including positioning, dimensions, background color, and overflow behavior. Also includes a style for elements with data-layout-correct='false'.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/shared-scroll-a-b-animate.html#_snippet_0

LANGUAGE: css
CODE:
```
body { padding: 0; margin: 0; }
.screen { width: 100%; height: 100%; position: fixed; inset: 0; overflow: hidden; }
.scroll { overflow-y: scroll; }
#box { margin-top: 1000px; width: 100px; height: 100px; background-color: #0088ff; }
#box-b { position: absolute; top: 100px; left: 100px; width: 200px; height: 200px; background-color: #0088ff; }
[data-layout-correct="false"] { background: #dd1144 !important; opacity: 0.5; }
```

----------------------------------------

TITLE: Manipulating Elements and Checking Projection with JavaScript
DESCRIPTION: Uses `window.Animate`, `window.Assert`, and `window.Projection` libraries to create projection nodes for parent, child, and grandchild elements. Modifies the parent's class, triggers updates, and uses `requestAnimationFrame` to log projection frames and assert element state.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/perf-parent-child-static-grandchild.html#_snippet_1

LANGUAGE: javascript
CODE:
```
const { createNode, relativeEase } = window.Animate
const { matchViewportBox, checkFrame } = window.Assert
const { frame } = window.Projection
const parent = document.getElementById("parent")
const parentProjection = createNode(
  parent,
  undefined,
  {},
  { duration: 0.1 }
)
const child = document.getElementById("child")
const childProjection = createNode(
  child,
  parentProjection,
  {},
  { duration: 0.1 }
)
const grandChild = document.getElementById("grandChild")
const grandChildProjection = createNode(
  grandChild,
  childProjection,
  {},
  { duration: 0.1 }
)
parentProjection.willUpdate()
childProjection.willUpdate()
grandChildProjection.willUpdate()
parent.classList.add("b")
parentProjection.root.didUpdate()
requestAnimationFrame(() => {
  requestAnimationFrame(() => {
    console.log(window.ProjectionFrames)
    checkFrame(parent, 1, {
      totalNodes: 4,
      resolvedTargetDeltas: 3,
      recalculatedProjection: 3,
    })
  })
})
```

----------------------------------------

TITLE: Styling Basic Layout and Fixed Element CSS
DESCRIPTION: Defines basic styles for the body and a container, sets up a fixed position element, includes a hidden element to trigger overflow, and styles elements with a specific data attribute to indicate incorrect layout.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/fixed-page-scroll.html#_snippet_0

LANGUAGE: CSS
CODE:
```
body { padding: 0; margin: 0; } #container { width: 100px; height: 100px; } #fixed { position: fixed; top: 0; left: 0; width: 500px; height: 50px; background: #00cc88; } #trigger-overflow { width: 1px; height: 1px; position: absolute; top: 2000px; left: 2000px; } [data-layout-correct="false"] { background: #dd1144 !important; opacity: 0.5; }
```

----------------------------------------

TITLE: Styling Container and Box Elements (CSS)
DESCRIPTION: Defines basic CSS styles for the page body, a flex container, and nested box elements, setting layout, dimensions, and initial appearance.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/benchmarks/cold-start-waapi.html#_snippet_0

LANGUAGE: css
CODE:
```
body { padding: 0; margin: 0; } .container { padding: 100px; width: 100%; display: flex; flex-wrap: wrap; } .container > div { width: 100px; height: 100px; } .box { width: 10px; height: 100px; background-color: #fff; }
```

----------------------------------------

TITLE: Styling Layout Elements with CSS
DESCRIPTION: Defines basic CSS rules for the body, a container element, and its child elements, including flexbox layout properties and a rule to visually indicate an incorrect layout state.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/flexbox-siblings.html#_snippet_0

LANGUAGE: css
CODE:
```
body { padding: 0; margin: 0; } #container { display: flex; position: relative; top: 100px; left: 100px; width: 300px; height: 200px; } #a { background: #00cc88; } #b { background: blue; } #container > div { height: 200px; flex: 1; } #trigger-overflow { width: 1px; height: 1px; position: absolute; top: 2000px; left: 2000px; } [data-layout-correct="false"] { background: #dd1144 !important; opacity: 0.5; }
```

----------------------------------------

TITLE: Styling Layout Elements with CSS
DESCRIPTION: Defines basic CSS styles for various elements including body, container, fixed, child, and a hidden trigger element. Sets dimensions, positioning, background colors, and flexbox properties for layout testing.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/fixed-child-layout-change.html#_snippet_0

LANGUAGE: css
CODE:
```
body { padding: 0; margin: 0; } #container { width: 100px; height: 100px; } #fixed { position: fixed; top: 0; left: 0; width: 500px; height: 100px; background: #00cc88; display: flex; align-items: flex-start; } #child { width: 100px; height: 100px; background: #0077ff; } #trigger-overflow { width: 1px; height: 1px; position: absolute; top: 2000px; left: 2000px; } \[data-layout-correct="false"\] { background: #dd1144 !important; opacity: 0.5; }
```

----------------------------------------

TITLE: Styling for Layout and Box Element (CSS)
DESCRIPTION: Defines basic CSS rules for the body padding, the dimensions and background color of a box element, and a style to highlight elements with incorrect layout data.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/optimized-appear/interrupt-delay-before.html#_snippet_0

LANGUAGE: css
CODE:
```
body { padding: 100px; margin: 0; } #box { width: 100px; height: 100px; background-color: #0077ff; } \[data-layout-correct="false"\] { background: #dd1144 !important; opacity: 1 !important; }
```

----------------------------------------

TITLE: Emulating Server-Side Rendering (JavaScript)
DESCRIPTION: Simulates server-side rendering by using `ReactDOMServer.renderToString` to render the React component into an HTML string and setting this string as the `innerHTML` of the root DOM element.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/optimized-appear/interrupt-delay-before.html#_snippet_3

LANGUAGE: javascript
CODE:
```
// Emulate server rendering of element
root.innerHTML = ReactDOMServer.renderToString(Component)
```

----------------------------------------

TITLE: Manual Rotation Animation Loop (JavaScript)
DESCRIPTION: Implements a manual animation loop using `requestAnimationFrame` to rotate elements based on elapsed time.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/benchmarks/pregenerated-background-color.html#_snippet_2

LANGUAGE: javascript
CODE:
```
function rotate(timestamp) {
  const elapsed = timestamp - startTime
  boxes.forEach((box, index) => {
    box.style.transform = `rotate(${elapsed * rotateRate}deg)`
  })
  requestAnimationFrame(rotate)
}
```

----------------------------------------

TITLE: Implementing Framer Motion Optimized Appear Animation with React (JavaScript)
DESCRIPTION: Sets up a React component using Framer Motion's `motion.div` to animate opacity. It includes an `onAnimationStart` check and uses `startOptimizedAppearAnimation` to handle the animation, emulating server rendering and hydrating the component mid-animation.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/optimized-appear/interrupt-tween-opacity-waapi.html#_snippet_1

LANGUAGE: javascript
CODE:
```
const { motion, animateStyle, startOptimizedAppearAnimation, optimizedAppearDataAttribute, motionValue, } = window.Motion
const { matchOpacity } = window.Assert
const root = document.getElementById("root")
const duration = 0.5

// This is the tree to be rendered "server" and client-side.
const Component = React.createElement(() => {
  React.useLayoutEffect(() => {
    const startTime = performance.now()
    while (performance.now() - startTime < 100) {}
  })

  return React.createElement(motion.div, {
    id: "box",
    initial: { opacity: 0 },
    animate: { opacity: 1 },
    transition: { duration, ease: "linear" },
    /**
     * On animation start, check the values we expect to see here
     */
    onAnimationStart: () => {
      const { opacity: initialOpacity } = window.getComputedStyle(box)
      const opacityAsNumber = parseFloat(initialOpacity)
      if (opacityAsNumber < 0.4 || opacityAsNumber > 0.6) {
        showError(
          box,
          `opacity should be roughly less than 0.5 at animation start`
        )
      }
    },
    \[optimizedAppearDataAttribute\]: "a",
  })
})

// Emulate server rendering of element
root.innerHTML = ReactDOMServer.renderToString(Component)

startOptimizedAppearAnimation(
  document.getElementById("box"),
  "opacity",
  \[0, 1\],
  {
    duration: duration * 1000,
    ease: "linear",
  },
  (animation) => {
    // Hydrate root mid-way through animation
    setTimeout(() => {
      ReactDOM.hydrateRoot(root, Component)
    }, (duration * 1000) / 2)
  }
)
```

----------------------------------------

TITLE: Benchmarking Motion Mix Function - JavaScript
DESCRIPTION: This snippet benchmarks the performance of the `mix` function from the `window.Motion` library. It creates an interpolation function with complex string inputs and executes it 100,000 times, then reports the total time taken to evaluate its efficiency.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/benchmarks/mix-complex-value-framer-motion.html#_snippet_1

LANGUAGE: JavaScript
CODE:
```
const { mix } = window.Motion
/**
 * Create an interpolate function that mixes unit values.
 */
const mixer = mix(
  "rgba(255, 255, 255, 0) 100px 40% 20px rgba(255, 255, 255, 0) var(--test) 67% 20vh 1vw",
  "rgba(0, 0, 0, 0) 0px 0% 100px rgba(255, 255, 255, 0) var(--test) 45% 0vh 1vw"
)
const numRuns = 100000
let startTime = performance.now()
for (let i = 0; i < numRuns; i++) {
  mixer(i / numRuns)
}
const finish = performance.now() - startTime
console.log(`Total time: ${finish}ms`)
```

----------------------------------------

TITLE: Basic CSS Layout Styling
DESCRIPTION: Defines fundamental styles for the body, a container using flexbox, direct children of the container, and a specific box element. It sets padding, margin, width, height, display properties, and background color.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/benchmarks/mix-color-value-greensock.html#_snippet_0

LANGUAGE: css
CODE:
```
body { padding: 0; margin: 0; } .container { padding: 100px; width: 100%; display: flex; flex-wrap: wrap; } .container > div { width: 100px; height: 100px; } .box { width: 10px; height: 100px; background-color: #fff; }
```

----------------------------------------

TITLE: Styling Elements for Framer Motion Layout (CSS)
DESCRIPTION: Defines basic CSS styles for the body and two box elements (#box-a, #box-b) used in the Framer Motion layout examples. Includes positioning, dimensions, background colors, and a rule for incorrect layout states.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/shared-promote-new-mix-rotate.html#_snippet_0

LANGUAGE: css
CODE:
```
body { padding: 0; margin: 0; } #box-a { width: 100px; height: 100px; background-color: #00cc88; } #box-b { position: absolute; top: 100px; left: 100px; width: 200px; height: 300px; background-color: #09f; } #trigger-overflow { width: 1px; height: 1px; position: absolute; top: 2000px; left: 2000px; } \[data-layout-correct="false"\] { background: #dd1144 !important; opacity: 0.5; }
```

----------------------------------------

TITLE: Setting Up Projections and Layout Test (JavaScript)
DESCRIPTION: Imports necessary functions from global objects, creates DOM elements and their corresponding 'projection' nodes, appends elements to the document, sets scroll position, and performs layout assertions using 'matchViewportBox' after a delay.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/shared-scroll-b-a-animate.html#_snippet_1

LANGUAGE: javascript
CODE:
```
const { createNode } = window.Animate
const { matchViewportBox, matchVisibility, matchOpacity } = window.Assert
const { frame } = window.Projection
const screen = document.querySelector(".screen")
const screenProjection = createNode(screen)
const box = document.getElementById("box-b")
const boxProjection = createNode(box, screenProjection, { layoutId: "box", })
boxProjection.willUpdate()
const scrollScreen = document.createElement("div")
scrollScreen.classList.add("screen", "scroll")
document.body.appendChild(scrollScreen)
const newBox = document.createElement("div")
newBox.id = "box"
scrollScreen.appendChild(newBox)
scrollScreen.scrollTop = 1000
const scrollScreenProjection = createNode(scrollScreen, undefined)
const newBoxProjection = createNode(
 newBox, scrollScreenProjection, { layoutId: "box", }
)
boxProjection.root.didUpdate()
setTimeout(() => {
 const measuredBox = box.getBoundingClientRect()
 matchViewportBox(box, measuredBox)
 matchViewportBox(newBox, measuredBox)
 const expected = { bottom: 480, height: 150, left: 50, right: 200, top: 330, width: 150, x: 50, y: 330, }
 matchViewportBox(box, expected)
}, 50)
```

----------------------------------------

TITLE: Styling Layout and Box Elements (CSS)
DESCRIPTION: Defines basic CSS styles for the page body, a flexbox container, and box elements. Includes a rule to visually indicate elements where layout correction might be incorrect.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/optimized-appear/defer-handoff-layout-child.html#_snippet_0

LANGUAGE: css
CODE:
```
body { padding: 100px; margin: 0; }
#container { display: flex; flex-direction: column; }
.box { width: 100px; height: 100px; background-color: #0077ff; }
\[data-layout-correct="false"\] { background: #dd1144 !important; opacity: 1 !important; }
```

----------------------------------------

TITLE: Basic CSS Layout Styling
DESCRIPTION: Provides fundamental CSS rules for body padding/margin, sets up a flex container, and defines dimensions for child elements and a specific box class.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/benchmarks/mix-array-greensock.html#_snippet_0

LANGUAGE: css
CODE:
```
body { padding: 0; margin: 0; } .container { padding: 100px; width: 100%; display: flex; flex-wrap: wrap; } .container > div { width: 100px; height: 100px; } .box { width: 10px; height: 100px; background-color: #fff; }
```

----------------------------------------

TITLE: Styling Basic Layout Elements (CSS)
DESCRIPTION: Defines fundamental CSS rules for the body, a container div, and its direct children to establish a basic layout structure with padding, margins, and dimensions.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/benchmarks/mix-unit-value-framer-motion.html#_snippet_0

LANGUAGE: css
CODE:
```
body { padding: 0; margin: 0; } .container { padding: 100px; width: 100%; display: flex; flex-wrap: wrap; } .container > div { width: 100px; height: 100px; } .box { width: 10px; height: 100px; background-color: #fff; }
```

----------------------------------------

TITLE: Styling Box Element and Layout Triggers (CSS)
DESCRIPTION: Defines basic styles for a box element, including size, background, and absolute positioning. It also includes styles for a trigger element and a data attribute used for layout correction checks.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/transform-single-with-scale-change.html#_snippet_0

LANGUAGE: css
CODE:
```
body { padding: 0; margin: 0; } #box { width: 100px; height: 100px; background-color: #00cc88; } #box.b { width: 200px; position: absolute; top: 100px; left: 200px; padding: 20px; } #trigger-overflow { width: 1px; height: 1px; position: absolute; top: 2000px; left: 2000px; } \[data-layout-correct="false"\] { background: #dd1144 !important; opacity: 0.5; }
```

----------------------------------------

TITLE: Styling Page and Box Elements (CSS)
DESCRIPTION: Defines basic CSS rules for the body, a container flexbox, wrapper divs, and the box elements, including padding, margins, dimensions, display properties, and background color.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/benchmarks/cold-start-anime.html#_snippet_0

LANGUAGE: css
CODE:
```
body { padding: 0; margin: 0; } .container { padding: 100px; width: 100%; display: flex; flex-wrap: wrap; } .container > div { width: 100px; height: 100px; } .box { width: 10%; height: 100px; background-color: #fff; }
```

----------------------------------------

TITLE: Basic CSS Layout Styling
DESCRIPTION: Defines basic styles for the body, a container, and child elements, including padding, margins, display properties, and dimensions.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/benchmarks/mix-unit-value-greensock.html#_snippet_0

LANGUAGE: css
CODE:
```
body { padding: 0; margin: 0; } .container { padding: 100px; width: 100%; display: flex; flex-wrap: wrap; } .container > div { width: 100px; height: 100px; } .box { width: 10px; height: 100px; background-color: #fff; }
```

----------------------------------------

TITLE: Styling Elements for Framer Motion Layout Testing (CSS)
DESCRIPTION: Defines basic styles for the body and specific elements ('a', 'b', 'trigger-overflow') used in a Framer Motion layout test. Includes a rule to highlight elements where layout correction is disabled.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/perf-shared-single.html#_snippet_0

LANGUAGE: css
CODE:
```
body { padding: 0; margin: 0; }
#a { width: 100px; height: 100px; background-color: #00cc88; }
#b { width: 200px; height: 200px; background-color: #0077ff; position: absolute; top: 50px; left: 50px; }
#trigger-overflow { width: 1px; height: 1px; position: absolute; top: 2000px; left: 2000px; }
[data-layout-correct="false"] { background: #dd1144 !important; opacity: 0.5; }
```

----------------------------------------

TITLE: Using Projection Library for Layout Updates (JavaScript)
DESCRIPTION: Initializes projection nodes for parent and child elements using a projection library. Applies a class change to trigger layout updates and then uses `requestAnimationFrame` to log projection frames and assert expected states after the layout change.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/perf-parent-child.html#_snippet_1

LANGUAGE: javascript
CODE:
```
const { createNode, relativeEase } = window.Animate
const { matchViewportBox, checkFrame } = window.Assert
const { frame } = window.Projection
const parent = document.getElementById("parent")
const parentProjection = createNode(
 parent,
 undefined,
 {},
 { duration: 0.1 }
)
const child = document.getElementById("child")
const childProjection = createNode(
 child,
 parentProjection,
 {},
 { duration: 0.1 }
)
parentProjection.willUpdate()
childProjection.willUpdate()
parent.classList.add("b")
parentProjection.root.didUpdate()
requestAnimationFrame(() => {
 requestAnimationFrame(() => {
 console.log(window.ProjectionFrames)
 checkFrame(parent, 1, {
 totalNodes: 3,
 resolvedTargetDeltas: 2,
 recalculatedProjection: 2,
 })
 })
})
```

----------------------------------------

TITLE: Styling Body and Box CSS
DESCRIPTION: Provides basic styling for the HTML body to remove default padding/margin, styles a sticky box element, defines a size element, and includes a rule for elements with incorrect layout states.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/sticky-scroll-change-with-stick.html#_snippet_0

LANGUAGE: css
CODE:
```
body { padding: 0; margin: 0; } #box { width: 100px; height: 100px; background-color: #00cc88; position: sticky; top: 0px; } #size { height: 100px; } #trigger-overflow { width: 1px; height: 1px; position: absolute; top: 2000px; left: 2000px; } \[data-layout-correct="false"\] { background: #dd1144 !important; opacity: 0.5; }
```

----------------------------------------

TITLE: Styling Basic Layout with CSS
DESCRIPTION: Defines basic styles for the body, a container element, its direct children, and a specific 'box' class, setting padding, margins, dimensions, display properties, and background color for layout and initial appearance.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/benchmarks/cold-start-framer-motion.html#_snippet_0

LANGUAGE: css
CODE:
```
body { padding: 0; margin: 0; } .container { padding: 50px; width: 80%; display: flex; flex-wrap: wrap; } .container > div { width: 50px; height: 50px; } .box { width: 10px; height: 50px; background-color: #fff; }
```

----------------------------------------

TITLE: Styling Container and Boxes with CSS
DESCRIPTION: Defines basic styles for the body, a container element, and individual box elements, setting padding, margins, display properties, and dimensions.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/benchmarks/warm-start-framer-motion.html#_snippet_0

LANGUAGE: css
CODE:
```
body { padding: 0; margin: 0; } .container { padding: 100px; width: 100%; display: flex; flex-wrap: wrap; } .container > div { width: 100px; height: 100px; } .box { width: 10%; height: 100px; background-color: #fff; }
```

----------------------------------------

TITLE: Styling Basic Layout Elements with CSS
DESCRIPTION: This CSS snippet defines basic styles for the body, a container, elements within the container, and a box class, setting padding, margin, dimensions, display properties, and background color.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/benchmarks/mix-object-greensock.html#_snippet_0

LANGUAGE: css
CODE:
```
body { padding: 0; margin: 0; } .container { padding: 100px; width: 100%; display: flex; flex-wrap: wrap; } .container > div { width: 100px; height: 100px; } .box { width: 10px; height: 100px; background-color: #fff; }
```

----------------------------------------

TITLE: Basic CSS Styling
DESCRIPTION: Provides fundamental CSS rules for the body and container elements, setting padding, margins, display properties, and dimensions for child elements.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/benchmarks/mix-color-value-framer-motion.html#_snippet_0

LANGUAGE: css
CODE:
```
body { padding: 0; margin: 0; } .container { padding: 100px; width: 100%; display: flex; flex-wrap: wrap; } .container > div { width: 100px; height: 100px; } .box { width: 10px; height: 100px; background-color: #fff; }
```

----------------------------------------

TITLE: Styling Elements for Layout Test (CSS)
DESCRIPTION: Defines basic styles for the body, a box element in its default and modified states (.b), a trigger element for overflow, and a style for indicating incorrect layout.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/single-element-layout-change-skew.html#_snippet_0

LANGUAGE: css
CODE:
```
body { padding: 0; margin: 0; } #box { width: 100px; height: 100px; background-color: #00cc88; } #box.b { width: 200px; position: absolute; top: 100px; left: 200px; padding: 20px; } #trigger-overflow { width: 1px; height: 1px; position: absolute; top: 2000px; left: 2000px; } \[data-layout-correct="false"\] { background: #dd1144 !important; opacity: 0.5; }
```

----------------------------------------

TITLE: Starting Optimized Opacity Animation (JavaScript)
DESCRIPTION: Initiates an optimized appear animation for the 'opacity' property of the box element using `startOptimizedAppearAnimation`. It animates from 0 to 1 using the same timing options as the transform animation.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/optimized-appear/resync-delay.html#_snippet_3

LANGUAGE: javascript
CODE:
```
startOptimizedAppearAnimation(
  document.getElementById("box"),
  "opacity",
  [0, 1],
  options
)
```

----------------------------------------

TITLE: CSS Styles for Layout and State Changes
DESCRIPTION: Defines initial styles for parent and child elements and a 'b' class that modifies the parent's layout, position, and display properties. Includes styles for a grandchild element and an element to trigger overflow.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/perf-parent-static-child-static-grandchild.skip.html#_snippet_0

LANGUAGE: css
CODE:
```
body { padding: 0; margin: 0; } #parent { width: 100px; height: 100px; background-color: #00cc88; } #child { width: 50px; height: 50px; background-color: #0077ff; } #parent.b { width: 200px; position: absolute; top: 100px; left: 200px; padding: 20px; display: flex; justify-content: flex-end; } #grandChild { width: 100px; height: 100px; background-color: black; } #trigger-overflow { width: 1px; height: 1px; position: absolute; top: 2000px; left: 2000px; } \[data-layout-correct="false"\] { background: #dd1144 !important; opacity: 0.5; }
```

----------------------------------------

TITLE: Styling Scrollable and Sticky Elements (CSS)
DESCRIPTION: Provides CSS rules for basic page layout, a fixed-size scrollable container, a sticky-positioned element, and its child. Includes styles for dimensions, overflow, background colors, flex alignment, and a specific style for elements marked with `data-layout-correct="false"`.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/sticky-element-scroll-child.html#_snippet_0

LANGUAGE: css
CODE:
```
body { padding: 0; margin: 0; } #position { width: 1px; height: 200px; } #scroller { width: 200px; height: 200px; overflow: scroll; margin-left: 100px; } #sticky { position: sticky; top: 0; left: 0; width: 100%; height: 50px; background: #0088ff; display: flex; justify-content: flex-start; } #sticky.b { justify-content: flex-end; } #child { width: 50px; height: 50px; background: #00cc88; } #content { width: 100%; height: 400px; } #trigger-overflow { width: 1px; height: 1px; position: absolute; top: 2000px; left: 2000px; } \[data-layout-correct="false"\] { background: #dd1144 !important; opacity: 0.5; }
```

----------------------------------------

TITLE: Styling Page Elements (CSS)
DESCRIPTION: Defines basic body padding/margin, styles for an element with id "box", and styles for elements with a specific data attribute indicating layout correction failure.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/optimized-appear/interrupt-delay-before-accelerated.html#_snippet_0

LANGUAGE: CSS
CODE:
```
body { padding: 100px; margin: 0; } #box { width: 100px; height: 100px; background-color: #0077ff; } \[data-layout-correct="false"\] { background: #dd1144 !important; opacity: 1 !important; }
```

----------------------------------------

TITLE: Styling for Layout and Animation Test (CSS)
DESCRIPTION: Provides basic styling for the test page, including padding for the body, dimensions and initial background for the animated box, and a specific style to highlight elements where layout correctness is flagged as false.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/optimized-appear/defer-handoff-layout-useeffect.html#_snippet_0

LANGUAGE: css
CODE:
```
body { padding: 100px; margin: 0; } #box { width: 100px; height: 100px; background-color: #0077ff; } \[data-layout-correct="false"\] { background: #dd1144 !important; opacity: 1 !important; }
```

----------------------------------------

TITLE: Styling Basic Layout Elements with CSS
DESCRIPTION: Defines fundamental CSS rules for the body, a container element, direct children of the container, and a specific box class, controlling padding, margin, dimensions, display properties, and background color.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/benchmarks/mix-array-framer-motion.html#_snippet_0

LANGUAGE: css
CODE:
```
body { padding: 0; margin: 0; } .container { padding: 100px; width: 100%; display: flex; flex-wrap: wrap; } .container > div { width: 100px; height: 100px; } .box { width: 10px; height: 100px; background-color: #fff; }
```

----------------------------------------

TITLE: Styling Scroll Container and Sticky Element (CSS)
DESCRIPTION: Defines CSS rules for basic body styling, a placeholder element (#position), a scrollable container (#scroller), a sticky element (#sticky) with a child (#child), content area (#content), an overflow trigger (#trigger-overflow), and a style for incorrect layout states.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/sticky-element-scroll.html#_snippet_0

LANGUAGE: css
CODE:
```
body { padding: 0; margin: 0; }
#position { width: 1px; height: 200px; }
#scroller { width: 200px; height: 200px; overflow: scroll; margin-left: 100px; }
#sticky { position: sticky; top: 0; left: 0; width: 100%; height: 50px; background: #0088ff; display: flex; align-items: flex-start; }
#child { width: 50px; height: 50px; background: #00cc88; }
#content { width: 100%; height: 400px; }
#trigger-overflow { width: 1px; height: 1px; position: absolute; top: 2000px; left: 2000px; }
[data-layout-correct="false"] { background: #dd1144 !important; opacity: 0.5; }
```

----------------------------------------

TITLE: Styling Elements with CSS
DESCRIPTION: Defines basic body styles, initial and modified styles for an element with id "parent", a hidden element to trigger overflow, and styles for elements with a specific data attribute.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/animate-undo-layout-change.html#_snippet_0

LANGUAGE: CSS
CODE:
```
body { padding: 0; margin: 0; } #parent { width: 100px; height: 100px; background-color: #00cc88; } #parent.b { width: 110px; height: 110px; } #trigger-overflow { width: 1px; height: 1px; position: absolute; top: 2000px; left: 2000px; } \[data-layout-correct="false"\] { background: #dd1144 !important; opacity: 0.5; }
```

----------------------------------------

TITLE: Styling Elements for Relative Positioning Test (CSS)
DESCRIPTION: Defines the initial styles for the parent, middle, and child elements, including dimensions, colors, positioning, and flex properties. It also includes styles for the 'b' class which represents the animated state of the parent and middle elements, and a style for incorrect layout state.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/animate-relative-skip-parent.html#_snippet_0

LANGUAGE: css
CODE:
```
body { padding: 0; margin: 0; }
#parent { position: relative; width: 200px; height: 200px; padding: 50px; background-color: #00cc88; }
#mid { width: 100px; height: 100px; background-color: white; display: flex; align-items: flex-start; justify-content: flex-start; position: absolute; top: 50px; left: 50px; }
#parent.b { width: 500px; height: 500px; }
.b #mid { justify-content: flex-end; }
#child { width: 50px; height: 50px; background-color: #0077ff; }
\[data-layout-correct="false"\] { background: #dd1144 !important; opacity: 0.5; }
```

----------------------------------------

TITLE: Styling for Optimized Appear Test (CSS)
DESCRIPTION: Provides basic styling for the body and the test box element. Includes a rule to highlight elements with `data-layout-correct="false"`.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/optimized-appear/interrupt-delay-after.html#_snippet_0

LANGUAGE: css
CODE:
```
body { padding: 100px; margin: 0; } #box { width: 100px; height: 100px; background-color: #0077ff; } \[data-layout-correct="false"\] { background: #dd1144 !important; opacity: 1 !important; }
```

----------------------------------------

TITLE: Styling Basic Elements with CSS
DESCRIPTION: Defines basic styles for the body, a box element, a child element, and a trigger element. Includes a style for elements with a specific data attribute to indicate layout issues.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/animate-single-element.html#_snippet_0

LANGUAGE: css
CODE:
```
body { padding: 0; margin: 0; } #box { width: 100px; height: 100px; background-color: #00cc88; } #child { width: 50px; height: 50px; background-color: #0077ff; } #box.b { width: 200px; position: absolute; top: 100px; left: 200px; padding: 20px; } #trigger-overflow { width: 1px; height: 1px; position: absolute; top: 2000px; left: 2000px; } \[data-layout-correct="false"\] { background: #dd1144 !important; opacity: 0.5; }
```

----------------------------------------

TITLE: Initializing Animation Node and Matching Viewport Box in JavaScript
DESCRIPTION: Initializes an animation node for a DOM element using `window.Animate.createNode`, gets its initial bounding box, updates the node's internal state, and asserts its viewport position using `window.Assert.matchViewportBox`.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/animate-single-element.html#_snippet_1

LANGUAGE: javascript
CODE:
```
const { createNode } = window.Animate
const { matchViewportBox } = window.Assert
const box = document.getElementById("box")
const boxProjection = createNode(box)
const boxOrigin = box.getBoundingClientRect()
boxProjection.willUpdate()
boxProjection.root.didUpdate()
matchViewportBox(box, boxOrigin)
```

----------------------------------------

TITLE: Styling Layout Elements with CSS
DESCRIPTION: Defines basic styles for the body, a container, and a box element. It uses flexbox for alignment within the container and includes specific styles for a modified container state (`#container.b`). Also includes styles for an overflow trigger and an element indicating incorrect layout.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/single-element-layout-change-skew-container.html#_snippet_0

LANGUAGE: css
CODE:
```
body { padding: 0; margin: 0; }
#container { width: 100px; height: 200px; display: flex; align-items: flex-end; }
#container.b { align-items: flex-start; }
#box { width: 100px; height: 100px; background-color: #00cc88; }
#trigger-overflow { width: 1px; height: 1px; position: absolute; top: 2000px; left: 2000px; }
[data-layout-correct="false"] { background: #dd1144 !important; opacity: 0.5; }
```

----------------------------------------

TITLE: Basic CSS Styles for Animation Elements
DESCRIPTION: Defines basic styles for the body and the animated #box element, including a specific style for elements marked as layout incorrect.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/optimized-appear/defer-handoff-block.html#_snippet_0

LANGUAGE: css
CODE:
```
body { margin: 0; } #box { width: 100px; height: 100px; background-color: #0077ff; } \[data-layout-correct="false"\] { background: #dd1144 !important; opacity: 1 !important; }
```

----------------------------------------

TITLE: Styling Layout Elements with CSS
DESCRIPTION: Defines basic styles for body, and specific elements (#a, #b, #scroller, #trigger-overflow) including positioning, dimensions, colors, and flexbox properties. Also includes a style for elements with `data-layout-correct='false'` to indicate layout issues.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/shared-transform-parents-animate-2.html#_snippet_0

LANGUAGE: css
CODE:
```
body { padding: 0; margin: 0; }
#a { width: 300px; height: 300px; position: absolute; top: 0; left: 0; background-color: #00cc88; }
#b { width: 200px; height: 200px; background-color: #ffcc00; }
#scroller { position: absolute; top: 200px; left: 10px; width: 500px; height: 200px; display: flex; justify-content: flex-end; align-items: flex-end; background-color: #09f; }
#trigger-overflow { width: 1px; height: 1px; position: absolute; top: 2000px; left: 2000px; }
[data-layout-correct="false"] { background: #dd1144 !important; opacity: 0.5; }
```

----------------------------------------

TITLE: Styling for Layout Animation Test - CSS
DESCRIPTION: Defines basic styles for the page body, container, and box elements used in the Framer Motion layout animation test, including a rule to highlight incorrect layout state.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/optimized-appear/defer-handoff-layout-sibling.html#_snippet_0

LANGUAGE: css
CODE:
```
body { padding: 100px; margin: 0; } #container { display: flex; flex-direction: column; } .box { width: 100px; height: 100px; background-color: #0077ff; } \[data-layout-correct="false"\] { background: #dd1144 !important; opacity: 1 !important; }
```

----------------------------------------

TITLE: Styling Layout Elements (CSS)
DESCRIPTION: Defines basic body styles, container and box dimensions/layout properties, and a specific style for elements with `data-layout-correct="false"`. It sets up the visual structure for the layout test.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/single-element-layout-change-perspective-container.html#_snippet_0

LANGUAGE: css
CODE:
```
body { padding: 0; margin: 0; } #container { width: 100px; height: 200px; display: flex; align-items: flex-end; transform-style: preserve-3d; background-color: blue; } #container.b { align-items: flex-start; } #box { width: 100px; height: 100px; background-color: #00cc88; padding: 10px; display: flex; align-items: flex-start; } .b #box { align-items: flex-end; } #box-child { width: 50px; height: 50px; background-color: #cc00cc; } #trigger-overflow { width: 1px; height: 1px; position: absolute; top: 2000px; left: 2000px; } \[data-layout-correct="false"\] { background: #dd1144 !important; opacity: 0.5; }
```

----------------------------------------

TITLE: Using GSAP Interpolate for Color Mixing (JavaScript)
DESCRIPTION: Demonstrates the usage of GSAP's `utils.interpolate` to create a function that mixes between two color values. It then runs this interpolation function multiple times in a loop and measures the execution time.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/benchmarks/mix-color-value-greensock.html#_snippet_1

LANGUAGE: javascript
CODE:
```
/\*\*\n \* Create an interpolate function that mixes unit values.\n \*\/\nconst px = gsap.utils.interpolate(
  "rgba(255, 255, 255, 0)",
  "rgba(0, 0, 0, 0)"
)
const numRuns = 10
let startTime = performance.now()
for (let i = 0; i < numRuns; i++) {
  console.log(px(i / numRuns))
}
console.log(`First run: ${performance.now() - startTime}ms`)
```

----------------------------------------

TITLE: Styling and Layout CSS
DESCRIPTION: Defines basic styles for body, parent, and child elements, including size, color, and layout properties. Includes specific rules for a modified state (`.b`) and a hidden trigger element.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/perf-single.html#_snippet_0

LANGUAGE: css
CODE:
```
body { padding: 0; margin: 0; }
#parent { width: 100px; height: 100px; background-color: #00cc88; }
#child { width: 50px; height: 50px; background-color: #0077ff; }
#parent.b { width: 200px; position: absolute; top: 100px; left: 200px; padding: 20px; display: flex; justify-content: flex-end; }
.b #child { width: 100px; height: 100px; }
#trigger-overflow { width: 1px; height: 1px; position: absolute; top: 2000px; left: 2000px; }
\[data-layout-correct="false"\] { background: #dd1144 !important; opacity: 0.5; }
```

----------------------------------------

TITLE: Styling Elements with CSS
DESCRIPTION: Defines basic styles for the body, a generic box element, a modified 'open' state for the box, a flex container parent, and a specific style for elements with a 'data-layout-correct' attribute set to 'false'.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/animate-nested-scale-correction.html#_snippet_0

LANGUAGE: css
CODE:
```
body { padding: 0; margin: 0; }
.box { width: 100px; height: 100px; background-color: #00cc88; }
.box.open { height: 200px; }
#parent { display: flex; }
[data-layout-correct="false"] { background: #dd1144 !important; opacity: 0.5; }
```

----------------------------------------

TITLE: Styling Elements with CSS
DESCRIPTION: Defines basic styles for body, box, and child elements, including dimensions, background colors, and positioning for a specific class 'b'. Also includes a rule for elements with `data-layout-correct="false"` to indicate layout issues.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/animate-single-element-layout-change-with-child.html#_snippet_0

LANGUAGE: css
CODE:
```
body { padding: 0; margin: 0; } #box { width: 100px; height: 100px; background-color: #00cc88; } #child { width: 50px; height: 50px; background-color: #0077ff; } #box.b { width: 200px; position: absolute; top: 100px; left: 200px; padding: 20px; } #trigger-overflow { width: 1px; height: 1px; position: absolute; top: 2000px; left: 2000px; } \[data-layout-correct=\"false\"\] { background: #dd1144 !important; opacity: 0.5; }
```

----------------------------------------

TITLE: Using GSAP interpolate with Mixed Units
DESCRIPTION: Demonstrates how to use `gsap.utils.interpolate` to create a function that can interpolate between complex string values containing various units (px, %, vh, vw) and CSS variables. Includes a simple loop to test its performance.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/benchmarks/mix-complex-value-greensock.html#_snippet_1

LANGUAGE: javascript
CODE:
```
/**
 * Create an interpolate function that mixes unit values.
 */
const px = gsap.utils.interpolate(
  "rgba(255, 255, 255, 0) 100px 40% 20px rgba(255, 255, 255, 0) var(--test) 67% 20vh 1vw",
  "rgba(0, 0, 0, 0) 0px 0% 100px rgba(255, 255, 255, 0) var(--test) 45% 0vh 1vw"
)
const numRuns = 10
let startTime = performance.now()
for (let i = 0; i < numRuns; i++) {
  px(i / numRuns)
}
console.log(`First run: ${performance.now() - startTime}ms`)
```

----------------------------------------

TITLE: Styling Layout Test Elements - CSS
DESCRIPTION: Defines basic styles for the body and two box elements (#box-a, #box-b) used in a layout test. Includes a hidden element (#trigger-overflow) to test overflow behavior and a style for elements marked as having incorrect layout.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/shared-promote-new-mix-interrupt.html#_snippet_0

LANGUAGE: css
CODE:
```
body { padding: 0; margin: 0; }
#box-a { width: 100px; height: 100px; background-color: #00cc88; }
#box-b { position: absolute; top: 100px; left: 100px; width: 200px; height: 300px; background-color: #09f; }
#trigger-overflow { width: 1px; height: 1px; position: absolute; top: 2000px; left: 2000px; }
\[data-layout-correct="false"\] { background: #dd1144 !important; opacity: 0.5; }
```

----------------------------------------

TITLE: Styling for Layout Projection Containers and Elements (CSS)
DESCRIPTION: Defines basic styles for the body, screen containers (fixed and scrollable), and specific box elements used in layout projection tests. Includes a style for indicating incorrect layout.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/shared-scroll-b-a.html#_snippet_0

LANGUAGE: css
CODE:
```
body { padding: 0; margin: 0; }
.screen { width: 100%; height: 100%; position: fixed; inset: 0; overflow: hidden; }
.scroll { overflow-y: scroll; }
#box { margin-top: 1000px; width: 100px; height: 100px; background-color: #0088ff; }
#box-b { position: absolute; top: 100px; left: 100px; width: 200px; height: 200px; background-color: #0088ff; }
[data-layout-correct="false"] { background: #dd1144 !important; opacity: 0.5; }
```

----------------------------------------

TITLE: JavaScript Scroll and Layout Projection Test (JavaScript)
DESCRIPTION: Uses a projection library to create nodes for a scroll container and a box, sets a property (borderRadius), simulates scroll changes, updates the projection tree, and then uses assertion functions to verify the final viewport position, opacity, and border radius of the box after rendering.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/element-scroll-non-zero.html#_snippet_1

LANGUAGE: javascript
CODE:
```
const { createNode } = window.Undo
const { matchViewportBox, addPageScroll, matchOpacity, matchBorderRadius, } = window.Assert
const { frame } = window.Projection
const scroll = document.getElementById("scroll")
const box = document.getElementById("box")
scroll.scrollLeft = 100
const boxOrigin = box.getBoundingClientRect()
const scrollProjection = createNode(scroll, undefined, { layoutScroll: true, })
const boxProjection = createNode(box, scrollProjection)
boxProjection.setValue("borderRadius", 20)
boxProjection.willUpdate()
scroll.scrollLeft = 50
boxProjection.root.didUpdate()
frame.postRender(() => {
  matchViewportBox(box, addPageScroll(boxOrigin, -50, 0))
  matchOpacity(box, 1)
  matchBorderRadius(box, "20px")
})
```

----------------------------------------

TITLE: Creating and Updating Layout Projections (JavaScript)
DESCRIPTION: Initializes projection nodes for existing and newly created DOM elements using a layout library. It sets up layout IDs, enables crossfading, updates a property (borderRadius), triggers updates, and uses postRender hooks to assert element properties and trigger further updates and promotions.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/shared-nested-promote-new-mix-interrupt.html#_snippet_1

LANGUAGE: JavaScript
CODE:
```
const { createNode } = window.Animate
const { matchViewportBox, matchVisibility, matchOpacity, matchBorderRadius,
} = window.Assert
const { frame } = window.Projection
const box = document.getElementById("box-a")
const boxProjection = createNode(box, undefined, { layoutId: "a", crossfade: true, })
const child = document.getElementById("child-a")
const childProjection = createNode(child, boxProjection, { layoutId: "b", crossfade: true, })
boxProjection.willUpdate()
childProjection.willUpdate()
const newBox = document.createElement("div")
newBox.id = "box-b"
document.body.appendChild(newBox)
const newBoxProjection = createNode(newBox, undefined, { layoutId: "a", crossfade: true, })
const newChild = document.createElement("div")
newChild.id = "child-b"
newBox.appendChild(newChild)
const newChildProjection = createNode(newChild, newBoxProjection, { layoutId: "b", crossfade: true, })
newBoxProjection.setValue("borderRadius", 20)
newBoxProjection.root.didUpdate()
let midBox = { bottom: 100, left: 50, right: 125, top: 50 }
frame.postRender(() => {
  matchViewportBox(child, midBox)
  matchViewportBox(newChild, midBox)
  matchVisibility(child, "visible")
  matchVisibility(newChild, "visible")
  matchOpacity(box, 1)
  matchOpacity(newBox, 1)
  matchOpacity(child, 1)
  matchOpacity(newChild, 1)
  boxProjection.willUpdate()
  childProjection.willUpdate()
  newBoxProjection.willUpdate()
  newChildProjection.willUpdate()
  boxProjection.promote()
  childProjection.promote()
  newBoxProjection.root.didUpdate()
  midBox = { bottom: 75, left: 25, right: 87.5, top: 25 }
  frame.postRender(() => {
    matchViewportBox(child, midBox)
    matchViewportBox(newChild, midBox)
    matchVisibility(child, "visible")
    matchVisibility(newChild, "visible")
    matchOpacity(box, 1)
    matchOpacity(newBox, 1)
    matchOpacity(child, 1)
    matchOpacity(newChild, 1)
  })
})
```

----------------------------------------

TITLE: Generating and Animating Boxes with GSAP (JavaScript)
DESCRIPTION: Generates 100 div elements containing a '.box', injects them into the DOM, selects the '.box' elements, and applies two sequential GSAP animations with delays. The first animation uses percentage width, while the second demonstrates animating width with pixel units and x position with percentage.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/benchmarks/cold-start-gsap.html#_snippet_1

LANGUAGE: javascript
CODE:
```
// Create boxes
const numBoxes = 100
let html = ``
for (let i = 0; i < numBoxes; i++) {
  html += `<div><div class="box"></div></div>`
}
document.querySelector(".container").innerHTML = html
const boxes = document.querySelectorAll(".box")
setTimeout(() => {
  // Cold start (read from DOM)
  boxes.forEach((box) => gsap.to(box, {
    rotate: Math.random() * 360,
    backgroundColor: "#f00",
    width: Math.random() * 100 + "%",
    x: 5,
    duration: 1,
  })
  )
  setTimeout(() => {
    // Unit conversion
    boxes.forEach((box) => gsap.to(box, {
      width: Math.random() * 100 + "px",
      x: "50%",
      duration: 1,
    })
    )
  }, 1500)
}, 1000)
```

----------------------------------------

TITLE: JavaScript Projection Node Setup and Layout Check
DESCRIPTION: Initializes projection nodes for parent, child, and grandchild elements using a projection library. It applies a class ('b') to the parent to trigger a layout change and then uses `requestAnimationFrame` to check the resulting projection calculations and frame data.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/perf-parent-static-child-static-grandchild.skip.html#_snippet_1

LANGUAGE: javascript
CODE:
```
const { createNode, relativeEase } = window.Animate
const { matchViewportBox, checkFrame } = window.Assert
const { frame } = window.Projection

const parent = document.getElementById("parent")
const parentProjection = createNode(
  parent,
  undefined,
  {},
  { duration: 10 }
)

const child = document.getElementById("child")
const childProjection = createNode(
  child,
  parentProjection,
  {},
  { duration: 0.0001 }
)

const grandChild = document.getElementById("grandChild")
const grandChildProjection = createNode(
  grandChild,
  childProjection,
  {},
  { duration: 0.0001 }
)

parentProjection.willUpdate()
childProjection.willUpdate()
grandChildProjection.willUpdate()

parent.classList.add("b")

parentProjection.root.didUpdate()

requestAnimationFrame(() => {
  requestAnimationFrame(() => {
    console.log(window.ProjectionFrames)
    checkFrame(parent, 2, {
      totalNodes: 4,
      resolvedTargetDeltas: 3,
      recalculatedProjection: 3,
    })
  })
})
```

----------------------------------------

TITLE: Styling Elements for Animation (CSS)
DESCRIPTION: Defines basic layout for the body and styles for the animated boxes, including initial state (opacity 0) and performance optimization (`will-change`).
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/benchmarks/pregenerated-background-color.html#_snippet_0

LANGUAGE: css
CODE:
```
body { display: flex; flex-wrap: wrap; gap: 10px; height: 100vh; overflow: hidden; }
.box { width: 50px; height: 50px; background-color: #f00; opacity: 0; will-change: auto !important; }
```

----------------------------------------

TITLE: Styling Elements with CSS
DESCRIPTION: Defines basic styles for the body, a box element, an overlay, an overflow trigger, and a data attribute selector. Includes padding, margin, dimensions, background color, positioning (sticky, absolute), and opacity.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/sticky-scroll-no-layout-change-stick.html#_snippet_0

LANGUAGE: CSS
CODE:
```
body { padding: 0; margin: 0; } #box { width: 100px; height: 100px; background-color: #00cc88; } #overlay { position: sticky; top: 0px; } #trigger-overflow { width: 1px; height: 1px; position: absolute; top: 2000px; left: 2000px; } [data-layout-correct="false"] { background: #dd1144 !important; opacity: 0.5; }
```

----------------------------------------

TITLE: Defining Container and Element Styles (CSS)
DESCRIPTION: Provides CSS rules for basic page styling, a container with flex and grid variations, child elements (#a, #b), an overflow trigger, and a style for elements marked as layout-incorrect.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/flexbox-siblings-to-grid.html#_snippet_0

LANGUAGE: css
CODE:
```
body { padding: 0; margin: 0; }
#container { display: flex; position: relative; top: 100px; left: 100px; width: 300px; height: 200px; }
#container.as-grid { display: grid; grid-template-columns: 50px auto; }
#a { background: #00cc88; grid-column: 2/3; }
#b { background: #0077ff; grid-column: 1/2; }
#container > div { height: 200px; flex: 1; }
#trigger-overflow { width: 1px; height: 1px; position: absolute; top: 2000px; left: 2000px; }
\[data-layout-correct="false"\] { background: #dd1144 !important; opacity: 0.5; }
```

----------------------------------------

TITLE: Setting up Layout Projections and Assertions (JavaScript)
DESCRIPTION: Initializes layout projection nodes for existing and newly created DOM elements using Framer Motion's internal `Animate` and `Projection` tools. It sets initial values like opacity, border radius, and rotation on the projection nodes and includes a `frame.postRender` callback to perform various assertions using `Assert` functions.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/shared-promote-new-mix-rotate-scale-correction.html#_snippet_1

LANGUAGE: javascript
CODE:
```
const { createNode } = window.Animate
const { matchViewportBox, matchVisibility, matchOpacity, matchBorderRadius, matchRotate, } = window.Assert
const { frame } = window.Projection
const container = document.getElementById("container-a")
const containerProjection = createNode(container, undefined, { layoutId: "container", })
const box = document.getElementById("box-a")
const boxProjection = createNode(box, containerProjection, { layoutId: "box", })
containerProjection.willUpdate()
boxProjection.willUpdate()
const newContainer = document.createElement("div")
newContainer.id = "container-b"
document.body.appendChild(newContainer)
const newContainerProjection = createNode(newContainer, undefined, { layoutId: "container", })
const newBox = document.createElement("div")
newBox.id = "box-b"
newContainer.appendChild(newBox)
const newBoxProjection = createNode(
 newBox, newContainerProjection, { layoutId: "box" }
)
boxProjection.setValue("opacity", 0.8)
newBoxProjection.setValue("borderRadius", 20)
newBoxProjection.setValue("rotate", 40)
newBoxProjection.root.didUpdate()
frame.postRender(() => {
 const bbox = newBox.getBoundingClientRect()
 matchViewportBox(box, bbox)
 matchVisibility(box, "visible")
 matchVisibility(newBox, "visible")
 matchOpacity(box, 0.8)
 matchOpacity(newBox, 1)
 matchRotate(box, 20)
 matchRotate(newBox, 20)
 matchBorderRadius(box, "6.66667% / 5%")
 matchBorderRadius(newBox, "6.66667% / 5%")
})
```

----------------------------------------

TITLE: Styling Elements for Layout Testing - CSS
DESCRIPTION: Defines basic styles for body, boxes, and child elements, including positioning and dimensions, and a rule for indicating incorrect layout.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/shared-transform-parents.html#_snippet_0

LANGUAGE: css
CODE:
```
body { padding: 0; margin: 0; } #box-a { width: 200px; height: 200px; position: absolute; left: 100px; top: 100px; background-color: #00cc88; } #box-b { position: absolute; top: 100px; left: 600px; width: 200px; height: 200px; background-color: #09f; } .child { position: relative; top: 20px; left: 20px; width: 100px; height: 100px; background-color: #ffcc00; } #trigger-overflow { width: 1px; height: 1px; position: absolute; top: 2000px; left: 2000px; } \[data-layout-correct="false"\] { background: #dd1144 !important; opacity: 0.5; }
```

----------------------------------------

TITLE: Creating and Manipulating Projection Nodes (JavaScript)
DESCRIPTION: Initializes projection nodes for existing and newly created DOM elements using Framer Motion's projection library, sets values like rotation, performs updates, and includes post-render assertions for testing layout and transforms.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/shared-promote-new-mix-rotate-layout.html#_snippet_1

LANGUAGE: javascript
CODE:
```
const { createNode } = window.Animate
const { matchViewportBox, matchVisibility, matchOpacity, matchBorderRadius, matchRotate,
} = window.Assert
const { frame } = window.Projection

const parentA = document.getElementById("parent-a")
const parentAProjection = createNode(parentA, undefined, { layoutId: "parent", })

const childA = document.getElementById("child-a")
const childAProjection = createNode(childA, parentAProjection, { layoutId: "child", })

childAProjection.setValue("rotate", 45)
childAProjection.render()
childAProjection.willUpdate()
parentAProjection.willUpdate()

const parentB = document.createElement("div")
parentB.id = "parent-b"
parentB.classList.add("parent")
document.body.appendChild(parentB)

const parentBProjection = createNode(parentB, undefined, { layoutId: "parent", })
parentBProjection.setValue("rotate", 45)
// This emulates setting rotate to style, immediately rendering
// the parent.
parentBProjection.render()

const childB = document.createElement("div")
childB.id = "child-b"
childB.classList.add("child")
parentB.appendChild(childB)

const childBProjection = createNode(childB, parentBProjection, { layoutId: "child", })

childAProjection.root.didUpdate()

frame.postRender(() => {
  const parentBbox = parentB.getBoundingClientRect()
  matchViewportBox(parentA, parentBbox)
  const childBbox = childB.getBoundingClientRect()
  // TODO: Nested transforms not expected to match currently
  // matchViewportBox(childA, childBbox)
  matchRotate(parentA, 22.5)
  matchRotate(childA, 22.5)
  matchRotate(parentB, 22.5)
  matchRotate(childB, 22.5)
})
```

----------------------------------------

TITLE: Styling Elements for Layout Testing (CSS)
DESCRIPTION: Defines basic styles for the body, container, and box elements used in the layout projection example. Includes positioning, dimensions, background colors, and a rule to highlight incorrect layout states.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/shared-promote-new-mix-rotate-scale-correction.html#_snippet_0

LANGUAGE: css
CODE:
```
body { padding: 0; margin: 0; } #container-a { position: relative; width: 300px; height: 300px; } #box-a { width: 100px; height: 100px; background-color: #00cc88; } #container-b { position: relative; width: 300px; height: 600px; } #box-b { position: absolute; top: 100px; left: 100px; width: 200px; height: 300px; background-color: #09f; } #trigger-overflow { width: 1px; height: 1px; position: absolute; top: 2000px; left: 2000px; } \[data-layout-correct="false"\] { background: #dd1144 !important; opacity: 0.5; }
```

----------------------------------------

TITLE: Styling Elements with CSS
DESCRIPTION: Defines basic layout and appearance for `body`, `#box`, and `#child` elements, including size, position, background color, and padding. It also includes styles for modified states (`.b` class) and a specific data attribute state.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/single-element-with-child-layout-change.html#_snippet_0

LANGUAGE: CSS
CODE:
```
body { padding: 0; margin: 0; } #box { width: 100px; height: 100px; background-color: #00cc88; } #child { width: 50px; height: 50px; background-color: #0077ff; } #box.b { width: 200px; position: absolute; top: 100px; left: 200px; padding: 20px; } #child.b { width: 100px; position: absolute; top: 150px; left: 250px; padding: 10px; } #trigger-overflow { width: 1px; height: 1px; position: absolute; top: 2000px; left: 2000px; } [data-layout-correct="false"] { background: #dd1144 !important; opacity: 0.5; }
```

----------------------------------------

TITLE: Testing Layout Projection (JavaScript)
DESCRIPTION: Uses `window.Undo` and `window.Assert` utilities (likely from a testing framework) to create projection nodes for DOM elements. It applies 3D transformations and then uses `requestAnimationFrame` to update the layout, add a class, and finally assert that the viewport boxes match their original positions after the update, likely testing layout projection correctness.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/single-element-layout-change-perspective-container.html#_snippet_1

LANGUAGE: javascript
CODE:
```
const { createNode } = window.Undo
const { matchViewportBox, matchSkewX } = window.Assert
const { frame } = window.Projection
const container = document.getElementById("container")
const containerProjection = createNode(container, undefined, { layout: false, })
const box = document.getElementById("box")
const boxProjection = createNode(box, containerProjection)
const boxChild = document.getElementById("box-child")
const boxChildProjection = createNode(boxChild, boxProjection)
containerProjection.setValue("rotateX", 40)
containerProjection.setValue("transformPerspective", 500)
boxProjection.setValue("rotateX", 10)
boxProjection.setValue("z", 20)
requestAnimationFrame(() => {
  const containerOrigin = container.getBoundingClientRect()
  const boxOrigin = box.getBoundingClientRect()
  const boxChildOrigin = boxChild.getBoundingClientRect()
  boxProjection.willUpdate()
  boxChildProjection.willUpdate()
  containerProjection.willUpdate()
  container.classList.add("b")
  requestAnimationFrame(() => {
    containerProjection.root.didUpdate()
    frame.postRender(() => {
      matchViewportBox(container, containerOrigin)
      matchViewportBox(box, boxOrigin)
      matchViewportBox(boxChild, boxChildOrigin)
    })
  })
})
```

----------------------------------------

TITLE: Animating and Projecting Elements with JavaScript
DESCRIPTION: Uses a library (likely Framer Motion) to create projection nodes for parent, mid, and child elements, link them hierarchically, trigger updates, change a class to alter layout, and perform layout assertions after rendering to verify positions.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/animate-relative-instant.html#_snippet_1

LANGUAGE: javascript
CODE:
```
const { createNode, relativeEase } = window.Animate
const { matchViewportBox } = window.Assert
const { frame } = window.Projection

const parent = document.getElementById("parent")
const mid = document.getElementById("mid")
const child = document.getElementById("child")

const parentProjection = createNode(
  parent,
  undefined,
  {},
  { duration: 200, ease: relativeEase() }
)

const midProjection = createNode(
  mid,
  parentProjection,
  {},
  { duration: 200, ease: relativeEase() }
)

const childProjection = createNode(
  child,
  midProjection,
  {},
  { duration: 0 }
)

parentProjection.willUpdate()
midProjection.willUpdate()
childProjection.willUpdate()

parent.classList.add("b")

parentProjection.root.didUpdate()

frame.postRender(() => {
  frame.postRender(() => {
    matchViewportBox(mid, {
      bottom: 50,
      left: 50,
      right: 100,
      top: 0,
    })
    matchViewportBox(child, {
      bottom: 50,
      left: 50,
      right: 100,
      top: 0,
    })
  })
})
```

----------------------------------------

TITLE: Defining Basic Layout and Styles (CSS)
DESCRIPTION: Sets up basic body styles, defines dimensions and colors for #box and #child elements, adds a class-based style change for #box.b, and includes a hidden element for triggering overflow and a style for incorrect layout states.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/single-element-layout-change-with-child-rotate.html#_snippet_0

LANGUAGE: css
CODE:
```
body { padding: 0; margin: 0; } #box { position: absolute; top: 100px; left: 300px; width: 200px; height: 200px; background-color: #00cc88; } #child { width: 50px; height: 50px; background-color: #0077ff; } #box.b { top: 200px; } #box.b #child { position: absolute; top: 150px; } #trigger-overflow { width: 1px; height: 1px; position: absolute; top: 2000px; left: 2000px; } \[data-layout-correct="false"\] { background: #dd1144 !important; opacity: 0.5; }
```

----------------------------------------

TITLE: Testing Layout Projection with Framer Motion (JavaScript)
DESCRIPTION: Uses Framer Motion's Projection library to create projection nodes for parent and child elements. It applies transformations (scale, x), updates the parent's class to trigger layout changes, and uses `postRender` to verify the final positions against original bounding boxes using `matchViewportBox` from the Assert library.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/transform-nested-parent-layout-change-scale-child-layout-change.html#_snippet_1

LANGUAGE: javascript
CODE:
```
const { createNode } = window.Undo
const { matchViewportBox } = window.Assert
const { frame } = window.Projection
const parent = document.getElementById("parent")
const child = document.getElementById("child")
const parentProjection = createNode(parent)
const childProjection = createNode(child, parentProjection)
parentProjection.setValue("scale", 2)
parentProjection.setValue("x", 400)
frame.postRender(() => {
  const parentOrigin = parent.getBoundingClientRect()
  const childOrigin = child.getBoundingClientRect()
  parentProjection.willUpdate()
  childProjection.willUpdate()
  parent.classList.add("b")
  childProjection.root.didUpdate()
  frame.postRender(() => {
    matchViewportBox(parent, parentOrigin)
    matchViewportBox(child, childOrigin)
  })
})
```

----------------------------------------

TITLE: Animating Element Layout with Framer Motion (JavaScript)
DESCRIPTION: Uses Framer Motion's Projection system to create nodes for a container and a box. It sets a rotation value on the container, updates the layout, changes the container's class to 'b' (which affects alignment via CSS), updates the layout again, and then asserts that the box's viewport position matches its original position after the layout changes. Requires window.Undo, window.Assert, and window.Projection.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/single-element-layout-change-rotate-container.html#_snippet_1

LANGUAGE: JavaScript
CODE:
```
const { createNode } = window.Undo
const { matchViewportBox } = window.Assert
const { frame } = window.Projection
const container = document.getElementById("container")
const containerProjection = createNode(container)
const box = document.getElementById("box")
const boxProjection = createNode(box, containerProjection)
containerProjection.setValue("rotate", 180)
requestAnimationFrame(() => {
  const boxOrigin = box.getBoundingClientRect()
  boxProjection.willUpdate()
  containerProjection.willUpdate()
  container.classList.add("b")
  requestAnimationFrame(() => {
    containerProjection.root.didUpdate()
    frame.postRender(() => {
      matchViewportBox(box, boxOrigin)
    })
  })
})
```

----------------------------------------

TITLE: Manipulating Element Projections with JavaScript
DESCRIPTION: Uses a projection library to create nodes for DOM elements, set properties like border radius, trigger layout updates, change the container's flex direction, and assert the final visual state of the elements after rendering.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/flexbox-siblings.html#_snippet_1

LANGUAGE: javascript
CODE:
```
const { createNode } = window.Undo
const { matchOpacity, matchBorderRadius, matchViewportBox } = window.Assert
const { frame } = window.Projection
const container = document.getElementById("container")
const a = document.getElementById("a")
const b = document.getElementById("b")
const aOrigin = a.getBoundingClientRect()
const bOrigin = b.getBoundingClientRect()
const aProjection = createNode(a)
const bProjection = createNode(b)
aProjection.setValue("borderRadius", 20)
aProjection.willUpdate()
bProjection.willUpdate()
container.style.flexDirection = "column-reverse"
aProjection.root.didUpdate()
frame.postRender(() => {
  matchViewportBox(a, aOrigin)
  matchViewportBox(b, bOrigin)
  matchOpacity(a, 1)
  matchOpacity(b, 1)
  matchBorderRadius(a, "13.3333% / 10%")
  matchBorderRadius(b, "")
})
```

----------------------------------------

TITLE: Styling Layout Elements with CSS
DESCRIPTION: Defines basic styles for body, container, fixed, child, and overflow trigger elements, including positioning, dimensions, background colors, and flexbox properties. Also includes a style for elements with `data-layout-correct="false"`.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/fixed-child-from-static.html#_snippet_0

LANGUAGE: CSS
CODE:
```
body { padding: 0; margin: 0; } #container { width: 100px; height: 100px; } #fixed { position: static; top: 0; left: 0; width: 500px; height: 100px; background: #00cc88; display: flex; align-items: flex-start; } #child { width: 100px; height: 100px; background: #0077ff; } #trigger-overflow { width: 1px; height: 1px; position: absolute; top: 2000px; left: 2000px; } \[data-layout-correct="false"\] { background: #dd1144 !important; opacity: 0.5; }
```

----------------------------------------

TITLE: Styling for Scroll Container and Box (CSS)
DESCRIPTION: Defines basic CSS styles for the page body, a scrollable container (#scroll), a box element (#box), a hidden element (.trigger-overflow) to force scrollbars, and a style for elements indicating an incorrect layout state.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/element-scroll-non-zero.html#_snippet_0

LANGUAGE: css
CODE:
```
body { padding: 0; margin: 0; }
#scroll { overflow: scroll; position: relative; height: 200px; width: 500px; }
#box { width: 100px; height: 100px; background: #00cc88; }
.trigger-overflow { width: 1px; height: 1px; position: absolute; top: 2000px; left: 2000px; }
\[data-layout-correct="false"\] { background: #dd1144 !important; opacity: 0.5; }
```

----------------------------------------

TITLE: Creating and Testing Projection Nodes with Framer Motion
DESCRIPTION: Initializes projection nodes for elements using window.Animate.createNode and window.Projection.frame. It simulates scrolling, creates new elements, and uses window.Assert functions (matchViewportBox) to test their layout and visibility after updates.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/shared-scroll-a-b-animate.html#_snippet_1

LANGUAGE: javascript
CODE:
```
const { createNode } = window.Animate
const { matchViewportBox, matchVisibility, matchOpacity } = window.Assert
const { frame } = window.Projection
const scrollScreen = document.querySelector(".screen")
const scrollScreenProjection = createNode(scrollScreen, undefined, { layoutScroll: true, })
scrollScreen.scrollTop = 1000
const box = document.getElementById("box")
const boxProjection = createNode(box, scrollScreenProjection, { layoutId: "box", })
boxProjection.willUpdate()
const screen = document.createElement("div")
screen.classList.add("screen")
document.body.appendChild(screen)
const screenProjection = createNode(screen, undefined)
const newBox = document.createElement("div")
newBox.id = "box-b"
screen.appendChild(newBox)
const newBoxProjection = createNode(newBox, screenProjection, { layoutId: "box", })
boxProjection.root.didUpdate()
setTimeout(() => {
  const measuredBox = box.getBoundingClientRect()
  matchViewportBox(box, measuredBox)
  matchViewportBox(newBox, measuredBox)
  const expected = {
    bottom: 480,
    height: 150,
    left: 50,
    right: 200,
    top: 330,
    width: 150,
    x: 50,
    y: 330,
  }
  matchViewportBox(box, expected)
}, 50)
```

----------------------------------------

TITLE: Updating Element Projections and Asserting Layout in JavaScript
DESCRIPTION: Uses window.Undo, window.Assert, and window.Projection to create and update projection nodes for parent and child elements. It captures initial bounding boxes, modifies the parent element's class, triggers projection updates, and asserts the viewport box of both elements after rendering.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/nested-layout-change-scale-correction.html#_snippet_1

LANGUAGE: javascript
CODE:
```
const { createNode } = window.Undo
const { matchViewportBox } = window.Assert
const { frame } = window.Projection
const parent = document.getElementById("parent")
const child = document.getElementById("child")
const parentProjection = createNode(parent)
const childProjection = createNode(child, parentProjection)
const parentOrigin = parent.getBoundingClientRect()
const childOrigin = child.getBoundingClientRect()
parentProjection.willUpdate()
childProjection.willUpdate()
parent.classList.add("b")
parentProjection.root.didUpdate()
frame.postRender(() => {
  matchViewportBox(parent, parentOrigin)
  matchViewportBox(child, childOrigin)
})
```

----------------------------------------

TITLE: Styling Elements with CSS
DESCRIPTION: This CSS snippet defines various styles for different elements using IDs and classes. It sets basic margins and padding for the body, dimensions and background for a box, dimensions for a size element, and positioning, dimensions, and background for an overlay. It also includes styles for a trigger element positioned far off-screen and a rule for elements with a specific data attribute.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/sticky-child-scroll-change.html#_snippet_0

LANGUAGE: css
CODE:
```
body { padding: 0; margin: 0; } #box { width: 100px; height: 100px; background-color: #00cc88; } #size { height: 100px; } #overlay { background-color: black; position: sticky; top: 0px; height: 200px; width: 200px; } #overlay.b #box { position: relative; } #trigger-overflow { width: 1px; height: 1px; position: absolute; top: 2000px; left: 2000px; } \[data-layout-correct="false"\] { background: #dd1144 !important; opacity: 0.5; }
```

----------------------------------------

TITLE: Testing Framer Motion Projection with Scroll and Layout Change (JavaScript)
DESCRIPTION: Initializes Framer Motion projection nodes for a scrollable element and a box within it. It simulates a scroll, records the box's initial position, updates the box's position and border radius, triggers layout updates, and then uses assertion functions to verify the box's final position, opacity, and border radius after the layout change and scroll.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/element-scroll-layout-change.html#_snippet_1

LANGUAGE: JavaScript
CODE:
```
const { createNode } = window.Undo
const { matchViewportBox, matchOpacity, matchBorderRadius, addPageScroll,
} = window.Assert
const { frame } = window.Projection
const scroll = document.getElementById("scroll")
const box = document.getElementById("box")
scroll.scrollTop = 50
const boxOrigin = box.getBoundingClientRect()
const scrollProjection = createNode(scroll, undefined, { layoutScroll: true,
})
const boxProjection = createNode(box, scrollProjection)
boxProjection.setValue("borderRadius", 20)
boxProjection.willUpdate()
box.classList.add("b")
boxProjection.root.didUpdate()
frame.postRender(() => {
matchViewportBox(box, boxOrigin)
matchOpacity(box, 1)
matchBorderRadius(box, "20%")
})
```

----------------------------------------

TITLE: Initializing Framer Motion Projection Nodes and Asserting Layout
DESCRIPTION: Initializes Framer Motion Projection nodes for 'box' and 'child' elements, linking the child to the box. It sets a border radius value on the child, triggers layout updates, adds a class to the box to change its style/position, and then uses `frame.postRender` to assert the final viewport position, opacity, and border radius of both elements.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/animate-single-element-layout-change-with-child.html#_snippet_1

LANGUAGE: javascript
CODE:
```
const { createNode, relativeEase } = window.Animate
const { frame } = window.Projection
const { matchViewportBox, matchOpacity, matchBorderRadius } = window.Assert
const box = document.getElementById("box")
const boxProjection = createNode(
  box,
  undefined,
  {},
  { ease: relativeEase() }
)
const child = document.getElementById("child")
const childProjection = createNode(
  child,
  boxProjection,
  {},
  { ease: relativeEase() }
)
const boxOrigin = box.getBoundingClientRect()
const childOrigin = child.getBoundingClientRect()
childProjection.setValue("borderRadius", 20)
boxProjection.willUpdate()
childProjection.willUpdate()
box.classList.add("b")
boxProjection.root.didUpdate()
frame.postRender(() => {
  frame.postRender(() => {
    matchViewportBox(box, {
      top: 50,
      right: 270,
      bottom: 170,
      left: 100,
    })
    matchViewportBox(child, {
      top: 60,
      right: 160,
      bottom: 110,
      left: 110,
    })
    matchOpacity(box, 1)
    matchOpacity(child, 1)
    matchBorderRadius(box, 0)
    matchBorderRadius(child, "40%")
  })
})
```

----------------------------------------

TITLE: Creating and Updating Projection Nodes with Framer Motion
DESCRIPTION: Demonstrates how to create projection nodes for DOM elements (`#box`, `#child`) using `createNode`, update their state by adding classes (`.b`), trigger a projection update, and then verify their final position using `matchViewportBox` within a `postRender` callback. Requires `window.Undo`, `window.Assert`, and `window.Projection`.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/single-element-with-child-layout-change.html#_snippet_1

LANGUAGE: JavaScript
CODE:
```
const { createNode } = window.Undo
const { matchViewportBox } = window.Assert
const { frame } = window.Projection
const box = document.getElementById("box")
const boxProjection = createNode(box)
const child = document.getElementById("child")
const childProjection = createNode(child, boxProjection)
const boxOrigin = box.getBoundingClientRect()
const childOrigin = child.getBoundingClientRect()
boxProjection.willUpdate()
childProjection.willUpdate()
box.classList.add("b")
child.classList.add("b")
boxProjection.root.didUpdate()
frame.postRender(() => {
  matchViewportBox(box, boxOrigin)
  matchViewportBox(child, childOrigin)
})
```

----------------------------------------

TITLE: Testing Framer Motion Layout and Skew (JavaScript)
DESCRIPTION: Uses Framer Motion's projection utilities to create a node for a box element, apply skew transformations, trigger a class change that affects layout, and then assert the viewport box and skew values after the update.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/single-element-layout-change-skew.html#_snippet_1

LANGUAGE: javascript
CODE:
```
const { createNode } = window.Undo
const { matchViewportBox, matchSkewX } = window.Assert
const { frame } = window.Projection
const box = document.getElementById("box")
const boxProjection = createNode(box)
boxProjection.setValue("skewX", 20)
boxProjection.setValue("skewY", -20)
requestAnimationFrame(() => {
  const boxOrigin = box.getBoundingClientRect()
  boxProjection.willUpdate()
  box.classList.add("b")
  boxProjection.root.didUpdate()
  frame.postRender(() => {
    matchViewportBox(box, boxOrigin)
    matchSkewX(box, 20)
  })
})
```

----------------------------------------

TITLE: Styling Basic Layout Elements (CSS)
DESCRIPTION: Defines basic body styles, sets up a flex container with positioning and dimensions, styles two child elements (#a, #b) with background colors, sets child height and flex properties, and defines a style for elements with data-layout-correct="false".
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/flexbox-siblings-layout-group.html#_snippet_0

LANGUAGE: CSS
CODE:
```
body { padding: 0; margin: 0; overflow: hidden; } #container { display: flex; position: relative; top: 100px; left: 100px; width: 300px; height: 200px; } #a { background: #00cc88; } #b { background: blue; } #container > div { height: 200px; flex: 1; } \[data-layout-correct="false"\] { background: #dd1144 !important; opacity: 0.5; }
```

----------------------------------------

TITLE: Styling Elements for Layout Testing (CSS)
DESCRIPTION: Defines basic styles for body, a box element, an overlay, and a trigger element. Includes a class 'b' for the overlay to reposition the box and a style for elements with `data-layout-correct="false"`.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/sticky-child-scroll-change-offset.html#_snippet_0

LANGUAGE: CSS
CODE:
```
body { padding: 0; margin: 0; } #box { width: 100px; height: 100px; background-color: #00cc88; } #size { height: 100px; } #overlay { background-color: black; position: sticky; top: 0px; height: 200px; width: 200px; } #overlay.b #box { position: relative; left: 300px; top: 300px; } #trigger-overflow { width: 1px; height: 1px; position: absolute; top: 2000px; left: 2000px; } \[data-layout-correct="false"\] { background: #dd1144 !important; opacity: 0.5; }
```

----------------------------------------

TITLE: Testing Sticky Positioning with Projection (JavaScript)
DESCRIPTION: Initializes projection nodes for the scroller, sticky element, and child using a library (likely related to Framer Motion). It then simulates scrolling the container, adds a class to the sticky element, triggers layout updates, and uses an assertion function to check the child's position after a short delay.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/sticky-element-scroll-child.html#_snippet_1

LANGUAGE: javascript
CODE:
```
const { createNode } = window.Undo
const { matchViewportBox, matchVisibility, matchOpacity, addPageScroll, } = window.Assert
const { frame } = window.Projection

const scroller = document.getElementById("scroller")
const scrollerProjection = createNode(scroller, undefined, { layoutScroll: true, })

const sticky = document.getElementById("sticky")
const stickyProjection = createNode(sticky, scrollerProjection, { layoutRoot: true, })

const child = document.getElementById("child")
const childProjection = createNode(child, stickyProjection)

const childOrigin = child.getBoundingClientRect()

stickyProjection.willUpdate()
childProjection.willUpdate()

scroller.scrollTo(0, 100)
sticky.classList.add("b")

stickyProjection.root.didUpdate()

setTimeout(() => {
  matchViewportBox(child, childOrigin)
}, 50)
```

----------------------------------------

TITLE: Creating Projection Node and Scrolling JavaScript
DESCRIPTION: Utilizes Framer Motion's internal utilities (`Undo`, `Assert`, `Projection`) to create a projection node for a specific DOM element, simulates scrolling the window, updates the projection's state after scrolling, and asserts the element's position within the viewport.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/sticky-scroll-change-with-stick.html#_snippet_1

LANGUAGE: javascript
CODE:
```
const { createNode } = window.Undo
const { matchViewportBox, addPageScroll } = window.Assert
const { frame } = window.Projection
const box = document.getElementById("box")
const boxProjection = createNode(box, undefined, { layoutRoot: true, })
requestAnimationFrame(() => {
  boxProjection.willUpdate()
  const scrollOffset = [50, 150]
  window.scrollTo(...scrollOffset)
  boxProjection.root.didUpdate()
  matchViewportBox(box, {
    top: 0,
    left: -50,
    bottom: 100,
    right: 50,
  })
})
```

----------------------------------------

TITLE: Animating Box SkewX with RequestAnimationFrame
DESCRIPTION: Uses window.Undo, window.Assert, and window.Projection utilities to create a projection node for a box element. It animates the skewX property using nested requestAnimationFrame calls, adds a class to the box, and performs assertions after rendering.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/single-element-layout-change-skew-change.html#_snippet_1

LANGUAGE: JavaScript
CODE:
```
const { createNode } = window.Undo
const { matchViewportBox, matchSkewX } = window.Assert
const { frame } = window.Projection
const box = document.getElementById("box")
const boxProjection = createNode(box)
boxProjection.setValue("skewX", 10)
requestAnimationFrame(() => {
  const boxOrigin = box.getBoundingClientRect()
  boxProjection.setValue("skewX", 30)
  requestAnimationFrame(() => {
    boxProjection.willUpdate()
    box.classList.add("b")
    boxProjection.setValue("skewX", 10)
    boxProjection.root.didUpdate()
    frame.postRender(() => {
      matchViewportBox(box, boxOrigin)
      matchSkewX(box, 10)
    })
  })
})
```

----------------------------------------

TITLE: Testing Layout Transition with Framer Motion (JavaScript)
DESCRIPTION: Uses Framer Motion's projection system (via window.Undo, window.Assert, window.Projection) to test a layout transition between two elements (#box-a and #box-b) with the same layoutId. It creates projection nodes, manipulates their visibility, updates projections, and asserts their positions and visibility before and after the transition.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/shared-block-update-promote-new.html#_snippet_1

LANGUAGE: javascript
CODE:
```
const { createNode } = window.Undo
const { matchViewportBox, matchVisibility, matchOpacity } = window.Assert
const { frame } = window.Projection
const box = document.getElementById("box-a")
const boxProjection = createNode(box, undefined, { layoutId: "a", crossfade: false, })
const boxOrigin = box.getBoundingClientRect()
boxProjection.root.blockUpdate()
boxProjection.willUpdate()
const newBox = document.createElement("div")
newBox.id = "box-b"
document.body.appendChild(newBox)
const newBoxProjection = createNode(newBox, undefined, { layoutId: "a", crossfade: false, })
boxProjection.root.didUpdate()
const newBoxOrigin = { bottom: 400, height: 300, left: 100, right: 300, top: 100, width: 200, }
setTimeout(() => {
  matchViewportBox(box, boxOrigin)
  matchViewportBox(newBox, newBoxOrigin)
  matchVisibility(box, "hidden")
  matchVisibility(newBox, "visible")
  matchOpacity(newBox, 1)
  boxProjection.willUpdate()
  newBoxProjection.willUpdate()
  boxProjection.promote()
  boxProjection.root.didUpdate()
  frame.postRender(() => {
    matchViewportBox(box, newBoxOrigin)
    matchViewportBox(newBox, newBoxOrigin)
    matchVisibility(box, "visible")
    matchVisibility(newBox, "hidden")
    matchOpacity(box, 1)
  })
}, 50)
```

----------------------------------------

TITLE: Initializing and Testing Layout Projection Nodes (JavaScript)
DESCRIPTION: Uses custom functions (`createNode`, `matchViewportBox`) to set up projection nodes for the overlay and box elements. It captures the initial box position, applies a scroll offset to the window, updates the projection tree, and verifies the box's final position relative to the viewport after scrolling.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/single-element-page-scroll-animated-overlay.html#_snippet_1

LANGUAGE: javascript
CODE:
```
const { createNode } = window.Undo const { matchViewportBox, addPageScroll } = window.Assert const { frame } = window.Projection const overlay = document.getElementById("overlay") const overlayProjection = createNode(overlay, undefined, { layoutScroll: true, layout: true, }) const box = document.getElementById("box") const boxProjection = createNode(box, overlayProjection) const boxOrigin = box.getBoundingClientRect() overlayProjection.willUpdate() boxProjection.willUpdate() const scrollOffset = \[50, 100\] window.scrollTo(...scrollOffset) boxProjection.root.didUpdate() matchViewportBox(box, { top: 0, left: 0, bottom: 100, right: 100 })
```

----------------------------------------

TITLE: Styling for Layout and Box Element (CSS)
DESCRIPTION: Defines basic padding for the body, styles a box element with a fixed size and background color, and includes a rule to visually highlight elements that fail a layout correctness check.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/optimized-appear/persist.html#_snippet_0

LANGUAGE: css
CODE:
```
body { padding: 100px; margin: 0; } #box { width: 100px; height: 100px; background-color: #0077ff; } [data-layout-correct="false"] { background: #dd1144 !important; opacity: 1 !important; }
```

----------------------------------------

TITLE: Styling Elements for Layout Test (CSS)
DESCRIPTION: Defines basic styles for body, box, and child elements, including a modified state '.b' for the box and an element to trigger overflow. Also includes a style for elements marked as layout-incorrect.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/single-element-layout-change-with-child.html#_snippet_0

LANGUAGE: css
CODE:
```
body { padding: 0; margin: 0; } #box { width: 100px; height: 100px; background-color: #00cc88; } #child { width: 50px; height: 50px; background-color: #0077ff; } #box.b { width: 200px; position: absolute; top: 100px; left: 200px; padding: 20px; } #trigger-overflow { width: 1px; height: 1px; position: absolute; top: 2000px; left: 2000px; } \[data-layout-correct="false"\] { background: #dd1144 !important; opacity: 0.5; }
```

----------------------------------------

TITLE: Testing Element Projection and Scrolling with JavaScript
DESCRIPTION: Uses helper functions from `window.Undo`, `window.Assert`, and `window.Projection` to create projection nodes for 'fixed' and 'child' elements, simulate scrolling, change the 'fixed' element's position and justification, update projections, and assert their viewport boxes after a delay.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/fixed-child-from-static.html#_snippet_1

LANGUAGE: JavaScript
CODE:
```
const { createNode } = window.Undo
const { matchViewportBox, matchVisibility, matchOpacity, addPageScroll,
} = window.Assert
const { frame } = window.Projection
const fixed = document.getElementById("fixed")
const fixedProjection = createNode(fixed, undefined, { layoutScroll: true, layout: true,
})
const fixedOrigin = fixed.getBoundingClientRect()
const child = document.getElementById("child")
const childProjection = createNode(child, fixedProjection)
const childOrigin = child.getBoundingClientRect()
childProjection.willUpdate()
fixedProjection.willUpdate()
const scrollDistance = 100
window.scrollTo(scrollDistance, scrollDistance)
fixed.style.position = "fixed"
fixed.style.justifyContent = "flex-end"
fixedProjection.root.didUpdate()
setTimeout(() => {
  matchViewportBox(fixed, { top: -100, left: -100, right: 400, bottom: 0,
  })
  matchViewportBox(child, { top: -100, left: -100, right: 0, bottom: 0,
  })
}, 50)
```

----------------------------------------

TITLE: Styling Basic Elements with CSS
DESCRIPTION: Defines basic styles for the body, and specific styles for elements with IDs "box", "child", and "trigger-overflow". It also includes a class-specific style for "#box.b" and a data-attribute selector style.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/single-element.html#_snippet_0

LANGUAGE: css
CODE:
```
body { padding: 0; margin: 0; } #box { width: 100px; height: 100px; background-color: #00cc88; } #child { width: 50px; height: 50px; background-color: #0077ff; } #box.b { width: 200px; position: absolute; top: 100px; left: 200px; padding: 20px; } #trigger-overflow { width: 1px; height: 1px; position: absolute; top: 2000px; left: 2000px; } [data-layout-correct="false"] { background: #dd1144 !important; opacity: 0.5; }
```

----------------------------------------

TITLE: Setting up Projection Nodes and Testing Layout (JavaScript)
DESCRIPTION: Initializes projection nodes for existing and newly created screen and box elements using functions from `window.Undo` and `window.Projection`. It manipulates the scroll position of a screen element and performs layout matching assertions using `window.Assert.matchViewportBox` after a short delay.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/shared-scroll-a-b.html#_snippet_1

LANGUAGE: JavaScript
CODE:
```
const { createNode } = window.Undo
const { matchViewportBox, matchVisibility, matchOpacity } = window.Assert
const { frame } = window.Projection
const scrollScreen = document.querySelector(".screen")
const scrollScreenProjection = createNode(scrollScreen, undefined, { layoutScroll: true, })
scrollScreen.scrollTop = 1000
const box = document.getElementById("box")
const boxProjection = createNode(box, scrollScreenProjection, { layoutId: "box", })
const boxOrigin = box.getBoundingClientRect()
boxProjection.willUpdate()
const screen = document.createElement("div")
screen.classList.add("screen")
document.body.appendChild(screen)
const screenProjection = createNode(screen, undefined)
const newBox = document.createElement("div")
newBox.id = "box-b"
screen.appendChild(newBox)
const newBoxProjection = createNode(newBox, screenProjection, { layoutId: "box", })
boxProjection.root.didUpdate()
setTimeout(() => {
  matchViewportBox(box, boxOrigin)
  matchViewportBox(newBox, boxOrigin)
  matchViewportBox(box, newBox)
}, 50)
```

----------------------------------------

TITLE: Testing Fixed Element Layout with Framer Motion (JavaScript)
DESCRIPTION: Uses Framer Motion's internal utilities (`Undo`, `Assert`, `Projection`) to create projection nodes for fixed and child elements, simulate scrolling and style changes, and assert their final positions relative to the viewport. This tests how Framer Motion handles layout updates and scrolling for fixed elements.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/fixed-child-page-scroll-layout-change.html#_snippet_1

LANGUAGE: javascript
CODE:
```
const { createNode } = window.Undo
const { matchViewportBox, matchVisibility, matchOpacity, addPageScroll,
} = window.Assert
const { frame } = window.Projection

const fixed = document.getElementById("fixed")
const fixedProjection = createNode(fixed, undefined, { layoutScroll: true, layout: true, })
const fixedOrigin = fixed.getBoundingClientRect()

const child = document.getElementById("child")
const childProjection = createNode(child, fixedProjection)
const childOrigin = child.getBoundingClientRect()

childProjection.willUpdate()
fixedProjection.willUpdate()

const scrollDistance = 100
window.scrollTo(scrollDistance, scrollDistance)

fixed.style.justifyContent = "flex-end"
fixed.style.top = "50px"
fixed.style.left = "50px"

fixedProjection.root.didUpdate()

setTimeout(() => {
  matchViewportBox(fixed, fixedOrigin)
  matchViewportBox(child, {
    top: 0,
    left: 0,
    right: 100,
    bottom: 100,
  })
}, 50)
```

----------------------------------------

TITLE: Testing Layout and Projections with JavaScript
DESCRIPTION: Uses a testing library to manipulate DOM elements, create projection nodes, change CSS classes to switch layout modes (flex to grid and back), scroll the window, and assert visual properties like viewport box, opacity, and border-radius after layout updates and scrolling.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/flexbox-siblings-to-grid-page-scroll-interrupt.html#_snippet_1

LANGUAGE: javascript
CODE:
```
const { createNode } = window.Undo
const { matchOpacity, matchBorderRadius, matchViewportBox, addPageScroll,
} = window.Assert
const { frame } = window.Projection
const container = document.getElementById("container")
const a = document.getElementById("a")
const b = document.getElementById("b")
const aOrigin = a.getBoundingClientRect()
const bOrigin = b.getBoundingClientRect()
const aProjection = createNode(a)
const bProjection = createNode(b)
aProjection.setValue("borderRadius", 20)
aProjection.willUpdate()
bProjection.willUpdate()
container.classList.add("as-grid")
window.scrollTo(50, 100)
aProjection.root.didUpdate()
frame.postRender(() => {
  matchViewportBox(a, addPageScroll(aOrigin, 50, 100))
  matchViewportBox(b, addPageScroll(bOrigin, 50, 100))
  aProjection.willUpdate()
  bProjection.willUpdate()
  container.classList.remove("as-grid")
  window.scrollTo(0, 0)
  aProjection.root.didUpdate()
  frame.postRender(() => {
    matchViewportBox(a, aOrigin)
    matchViewportBox(b, bOrigin)
    matchOpacity(a, 1)
    matchOpacity(b, 1)
    matchBorderRadius(a, "20px")
    matchBorderRadius(b, "")
  })
})
```

----------------------------------------

TITLE: Benchmarking Framer Motion Mix Function Performance in JavaScript
DESCRIPTION: Imports the `mix` function from the global `window.Motion` object, initializes a mixer for interpolating between two arrays of mixed unit values, executes the mixer function one million times, and logs the total execution time to the console.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/benchmarks/mix-array-framer-motion.html#_snippet_1

LANGUAGE: javascript
CODE:
```
const { mix } = window.Motion
/** * Create an interpolate function that mixes unit values. */
const mixer = mix(
  [100, "50vh", "100px", "#fff"],
  [0, "0vh", "0px", "#000"]
)
const numRuns = 1000000
let startTime = performance.now()
for (let i = 0; i < numRuns; i++) {
  mixer(i / numRuns)
}
const finish = performance.now() - startTime
console.log(`Total time: ${finish}ms`)
```

----------------------------------------

TITLE: Setting up Layout Projections and Assertions - JavaScript
DESCRIPTION: Initializes layout projection nodes for two elements (#box-a and dynamically created #box-b) using internal animation/projection utilities. It sets initial values like opacity and border radius, appends the new element, and then uses assertion functions within a post-render callback to verify the elements' viewport box, visibility, opacity, and border radius against expected values.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/shared-promote-new-mix.html#_snippet_1

LANGUAGE: javascript
CODE:
```
const { createNode } = window.Animate;
const { matchViewportBox, matchVisibility, matchOpacity, matchBorderRadius,
} = window.Assert;
const { frame } = window.Projection;
const box = document.getElementById("box-a");
const boxProjection = createNode(box, undefined, { layoutId: "a" });
boxProjection.willUpdate();
const newBox = document.createElement("div");
newBox.id = "box-b";
document.body.appendChild(newBox);
const newBoxProjection = createNode(newBox, undefined, { layoutId: "a", });
boxProjection.setValue("opacity", 0.8);
newBoxProjection.setValue("borderRadius", 20);
newBoxProjection.root.didUpdate();
frame.postRender(() => {
  const midBox = { bottom: 250, left: 50, right: 200, top: 50 };
  matchViewportBox(box, midBox);
  matchViewportBox(newBox, midBox);
  matchVisibility(box, "visible");
  matchVisibility(newBox, "visible");
  matchOpacity(box, 0.8);
  matchOpacity(newBox, 1);
  matchBorderRadius(box, "6.66667% / 5%");
  matchBorderRadius(newBox, "6.66667% / 5%");
});
```

----------------------------------------

TITLE: Implementing Layout Animation with Projection (JavaScript)
DESCRIPTION: Uses functions from `window.Undo`, `window.Assert`, and `window.Projection` to create projection nodes for existing and new elements. It appends the new element, sets a shared `layoutId`, triggers an update, and uses assertions to verify the final state after a potential layout animation.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/shared-promote-new.html#_snippet_1

LANGUAGE: javascript
CODE:
```
const { createNode } = window.Undo
const { matchViewportBox, matchVisibility, matchOpacity } = window.Assert
const { frame } = window.Projection
const box = document.getElementById("box-a")
const boxProjection = createNode(box, undefined, { layoutId: "a" })
const boxOrigin = box.getBoundingClientRect()
boxProjection.willUpdate()
const newBox = document.createElement("div")
newBox.id = "box-b"
document.body.appendChild(newBox)
const newBoxProjection = createNode(newBox, undefined, { layoutId: "a", crossfade: false, })
newBoxProjection.root.didUpdate()
setTimeout(() => {
  matchViewportBox(box, boxOrigin)
  matchViewportBox(newBox, boxOrigin)
  matchVisibility(box, "hidden")
  matchVisibility(newBox, "visible")
  matchOpacity(newBox, 1)
}, 50)
```

----------------------------------------

TITLE: Animating and Testing Layout with JavaScript
DESCRIPTION: Initializes animatable nodes for the 'box' and 'child' elements using a library's createNode function, linking the child node to the box node. Sets an initial rotation value on the box node. Uses requestAnimationFrame to trigger updates, add a CSS class 'b' to the box (which changes its position and the child's position via CSS), and then asserts the final viewport positions of both elements using matchViewportBox within a post-render callback.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/single-element-layout-change-with-child-rotate-animate.html#_snippet_1

LANGUAGE: javascript
CODE:
```
const { createNode } = window.Animate
const { matchViewportBox } = window.Assert
const { frame } = window.Projection
const box = document.getElementById("box")
const boxProjection = createNode(
 box,
 undefined,
 {},
 { duration: 5, ease: () => 0.5 }
)
const child = document.getElementById("child")
const childProjection = createNode(
 child,
 boxProjection,
 {},
 { duration: 5, ease: () => 1 }
)
boxProjection.setValue("rotate", 45)
requestAnimationFrame(() => {
 boxProjection.willUpdate()
 childProjection.willUpdate()
 box.classList.add("b")
 requestAnimationFrame(() => {
 boxProjection.root.didUpdate()
 frame.postRender(() => {
 matchViewportBox(box, {
 bottom: 391.4213562011719,
 height: 282.84271240234375,
 left: 258.5786437988281,
 right: 541.42138671875,
 top: 108.57864379882812,
 width: 282.8427429199219,
 })
 matchViewportBox(child, {
 bottom: 285.3553466796875,
 height: 70.710693359375,
 left: 258.5786437988281,
 right: 329.289306640625,
 top: 214.6446533203125,
 width: 70.71066284179688,
 })
 })
 })
})
```

----------------------------------------

TITLE: Styling for Layout Test Elements (CSS)
DESCRIPTION: Defines basic styling for the body, a box element used for animation, and a data attribute used to indicate layout correctness issues.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/optimized-appear/interrupt-tween-x.html#_snippet_0

LANGUAGE: css
CODE:
```
body { padding: 100px; margin: 0; } #box { width: 100px; height: 100px; background-color: #0077ff; } \[data-layout-correct="false"\] { background: #dd1144 !important; opacity: 1 !important; }
```

----------------------------------------

TITLE: Styling Layout Test Elements (CSS)
DESCRIPTION: Defines basic CSS styles for the body to remove padding/margin, styles for two test boxes (#box-a and #box-b) including position and dimensions, an element to trigger overflow, and a style rule to highlight elements marked as having incorrect layout.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/shared-promote-new-rotate.html#_snippet_0

LANGUAGE: css
CODE:
```
body { padding: 0; margin: 0; } #box-a { width: 100px; height: 100px; background-color: #00cc88; } #box-b { position: absolute; top: 100px; left: 100px; width: 200px; height: 300px; background-color: #09f; } #trigger-overflow { width: 1px; height: 1px; position: absolute; top: 2000px; left: 2000px; } \[data-layout-correct="false"\] { background: #dd1144 !important; opacity: 0.5; }
```

----------------------------------------

TITLE: Creating and Updating Node Projection in JavaScript
DESCRIPTION: Uses functions from `window.Undo` and `window.Assert` to create a node projection for an element with ID "box". It captures the initial bounding box, triggers update cycles on the projection, and then asserts the viewport box matches the original.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/single-element.html#_snippet_1

LANGUAGE: javascript
CODE:
```
const { createNode } = window.Undo
const { matchViewportBox } = window.Assert
const box = document.getElementById("box")
const boxProjection = createNode(box)
const boxOrigin = box.getBoundingClientRect()
boxProjection.willUpdate()
boxProjection.root.didUpdate()
matchViewportBox(box, boxOrigin)
```

----------------------------------------

TITLE: Basic CSS Layout and Box Styling
DESCRIPTION: Defines fundamental CSS rules for the body, a flexbox container, child elements within the container, and a specific box class. It sets properties like padding, margin, width, height, display, flex-wrap, and background color to structure and style elements.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/benchmarks/mix-number-value-greensock.html#_snippet_0

LANGUAGE: css
CODE:
```
body { padding: 0; margin: 0; } .container { padding: 100px; width: 100%; display: flex; flex-wrap: wrap; } .container > div { width: 100px; height: 100px; } .box { width: 10px; height: 100px; background-color: #fff; }
```

----------------------------------------

TITLE: Testing Fixed Element Projection (JavaScript)
DESCRIPTION: Uses framer-motion's internal testing utilities (Undo, Assert, Projection) to create a projection node for a fixed element, simulate scrolling and style changes, and then assert its viewport position after a delay.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/fixed-page-scroll-layout-change.html#_snippet_1

LANGUAGE: JavaScript
CODE:
```
const { createNode } = window.Undo
const { matchViewportBox, matchVisibility, matchOpacity, addPageScroll, } = window.Assert
const { frame } = window.Projection
const fixed = document.getElementById("fixed")
const fixedProjection = createNode(fixed, undefined, { layoutScroll: true, layout: true, })
const fixedOrigin = fixed.getBoundingClientRect()
fixedProjection.willUpdate()
const scrollDistance = 100
window.scrollTo(scrollDistance, scrollDistance)
fixed.style.top = "50px"
fixed.style.left = "50px"
fixedProjection.root.didUpdate()
setTimeout(() => { matchViewportBox(fixed, fixedOrigin) }, 50)
```

----------------------------------------

TITLE: Initializing and Updating Projections with JavaScript
DESCRIPTION: Uses custom libraries (Undo, Assert, Projection) to create and update projection nodes for specific DOM elements ('overlay', 'box'). It retrieves element dimensions, scrolls the window to a specific offset, updates the projection root, and asserts the box's position within the viewport.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/sticky-scroll-no-layout-change-stick.html#_snippet_1

LANGUAGE: JavaScript
CODE:
```
const { createNode } = window.Undo
const { matchViewportBox, addPageScroll } = window.Assert
const { frame } = window.Projection
const overlay = document.getElementById("overlay")
const overlayProjection = createNode(overlay, undefined, { layoutRoot: true, layout: true, })
const box = document.getElementById("box")
const boxProjection = createNode(box, overlayProjection)
const boxOrigin = box.getBoundingClientRect()
boxProjection.willUpdate()
const scrollOffset = [50, 100]
window.scrollTo(...scrollOffset)
boxProjection.root.didUpdate()
matchViewportBox(box, { top: 0, left: -50, bottom: 100, right: 50 })
```

----------------------------------------

TITLE: Styling for Test Elements (CSS)
DESCRIPTION: Defines basic CSS styles for the body and a box element used in the test. It also includes a rule to visually indicate incorrect layout states.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/optimized-appear/defer-handoff.html#_snippet_0

LANGUAGE: css
CODE:
```
body { padding: 100px; margin: 0; } #box { width: 100px; height: 100px; background-color: #0077ff; } \[data-layout-correct="false"\] { background: #dd1144 !important; opacity: 1 !important; }
```

----------------------------------------

TITLE: Styling Elements for Layout Test (CSS)
DESCRIPTION: Defines basic CSS styles for the body, a box element (#box), and a trigger element. Includes styles for the box's initial state and a modified state when the class 'b' is applied, which changes its size, position, and padding.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/single-element-layout-change-rotate.html#_snippet_0

LANGUAGE: css
CODE:
```
body { padding: 0; margin: 0; }
#box { width: 100px; height: 100px; background-color: #00cc88; }
#box.b { width: 200px; position: absolute; top: 100px; left: 200px; padding: 20px; }
#trigger-overflow { width: 1px; height: 1px; position: absolute; top: 2000px; left: 2000px; }
\[data-layout-correct="false"\] { background: #dd1144 !important; opacity: 0.5; }
```

----------------------------------------

TITLE: Styling Basic Layout and Elements with CSS
DESCRIPTION: Defines fundamental CSS rules for body padding/margin, box dimensions and colors, and specific styles for elements based on a data attribute. Includes styles for nested elements and a hidden trigger element.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/perf-neighbours.html#_snippet_0

LANGUAGE: css
CODE:
```
body { padding: 0; margin: 0; } .box { width: 100px; height: 100px; background-color: #00cc88; display: flex; justify-content: center; align-items: center; } .box.open { height: 200px; } .b { width: 50px; height: 50px; background-color: white; display: flex; justify-content: center; align-items: center; } .c { width: 25px; height: 25px; background-color: #00cc88; } #trigger-overflow { width: 1px; height: 1px; position: absolute; top: 2000px; left: 2000px; } \[data-layout-correct="false"\] { background: #dd1144 !important; opacity: 0.5; }
```

----------------------------------------

TITLE: Styling Page and Box Elements (CSS)
DESCRIPTION: Defines basic CSS styles for the body, a flex container, the container's direct children, and the inner '.box' elements, setting padding, margins, display properties, and initial dimensions/background.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/benchmarks/cold-start-gsap.html#_snippet_0

LANGUAGE: css
CODE:
```
body { padding: 0; margin: 0; } .container { padding: 100px; width: 100%; display: flex; flex-wrap: wrap; } .container > div { width: 100px; height: 100px; } .box { width: 10px; height: 100px; background-color: #fff; }
```

----------------------------------------

TITLE: Creating and Updating Projection Nodes with Framer Motion (JS)
DESCRIPTION: Initializes projection nodes for specific DOM elements using `window.Animate.createNode`. Sets up parent-child relationships between nodes and triggers updates. Includes post-render checks using `window.Assert.checkFrame`.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/perf-neighbours.html#_snippet_1

LANGUAGE: javascript
CODE:
```
const { createNode, relativeEase } = window.Animate
const { matchViewportBox, checkFrame } = window.Assert
const { frame } = window.Projection
const duration = 10
const topEl = document.getElementById("top")
const topProjection = createNode(topEl, undefined, {}, { duration })
const bottom = document.getElementById("bottom")
const bottomProjection = createNode(
 bottom, undefined, {}, { duration }
)
const b = document.querySelector(".b")
const bProjection = createNode(
 b, bottomProjection, {}, { duration }
)
const c = document.querySelector(".c")
const cProjection = createNode(c, bProjection, {}, { duration })

topProjection.willUpdate()
bottomProjection.willUpdate()
bProjection.willUpdate()
cProjection.willUpdate()

topEl.classList.add("open")

topProjection.root.didUpdate()

frame.postRender(() => {
 frame.postRender(() => {
 console.log(window.ProjectionFrames)
 checkFrame(topEl, 2, {
 totalNodes: 5,
 resolvedTargetDeltas: 3,
 recalculatedProjection: 3,
 })
 })
})
```

----------------------------------------

TITLE: Initializing Framer Motion Projection Nodes in JavaScript
DESCRIPTION: Initializes Framer Motion projection nodes for specific DOM elements ('overlay' and 'box'). It sets up the overlay as a layout root, scrolls the window to a specific offset, captures the initial bounding box of the 'box' element, and triggers projection updates and viewport box matching.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/sticky-child.html#_snippet_1

LANGUAGE: javascript
CODE:
```
const { createNode } = window.Undo
const { matchViewportBox, addPageScroll } = window.Assert
const { frame } = window.Projection
const overlay = document.getElementById("overlay")
const overlayProjection = createNode(overlay, undefined, { layoutRoot: true, })
const box = document.getElementById("box")
const boxProjection = createNode(box, overlayProjection)
const scrollOffset = [50, 150]
window.scrollTo(...scrollOffset)
const boxOrigin = box.getBoundingClientRect()
boxProjection.willUpdate()
boxProjection.root.didUpdate()
matchViewportBox(box, boxOrigin)
```

----------------------------------------

TITLE: Testing Layout Projection with Scrolling (JavaScript)
DESCRIPTION: Uses Framer Motion's projection system and assertion utilities to test layout correctness. It initializes projection nodes for elements, simulates page scrolling, changes an element's position from fixed to static, updates projections, and asserts the final viewport box positions.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/fixed-child-to-static.html#_snippet_1

LANGUAGE: JavaScript
CODE:
```
const { createNode } = window.Undo
const { matchViewportBox, matchVisibility, matchOpacity, addPageScroll,
} = window.Assert
const { frame } = window.Projection
const fixed = document.getElementById("fixed")
const fixedProjection = createNode(fixed, undefined, { layoutScroll: true, layout: true,
})
const fixedOrigin = fixed.getBoundingClientRect()
const child = document.getElementById("child")
const childProjection = createNode(child, fixedProjection)
const childOrigin = child.getBoundingClientRect()
childProjection.willUpdate()
fixedProjection.willUpdate()
const scrollDistance = 100
window.scrollTo(scrollDistance, scrollDistance)
fixed.style.position = "static"
fixed.style.justifyContent = "flex-end"
fixedProjection.root.didUpdate()
setTimeout(() => {
 matchViewportBox(fixed, fixedOrigin)
 matchViewportBox(child, { top: 0, left: 0, right: 100, bottom: 100,
 })
}, 50)
```

----------------------------------------

TITLE: Styling Page Elements and Scroll Container (CSS)
DESCRIPTION: Defines basic body padding and margin, sets up a scrollable container with fixed dimensions and relative positioning, styles a box element within it, and includes specific styles for the scroll container when a class 'b' is added, altering overflow and position. It also styles a hidden element used to trigger overflow and a rule for elements with a specific data attribute.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/element-scroll-to-layout.html#_snippet_0

LANGUAGE: CSS
CODE:
```
body { padding: 0; margin: 0; } #scroll { overflow: scroll; position: relative; height: 200px; width: 500px; } #box { width: 100px; height: 100px; background: #00cc88; } #scroll.b { overflow: visible; top: 200px; left: 100px; } #scroll.b .trigger-overflow { display: none; } .trigger-overflow { width: 1px; height: 1px; position: absolute; top: 2000px; left: 2000px; } [data-layout-correct="false"] { background: #dd1144 !important; opacity: 0.5; }
```

----------------------------------------

TITLE: Basic CSS Styling
DESCRIPTION: Provides basic styling for the page body, a box element (#box), and a specific data attribute state ([data-layout-correct="false"]) used for indicating layout issues.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/optimized-appear/defer-handoff-layout.html#_snippet_0

LANGUAGE: css
CODE:
```
body { padding: 100px; margin: 0; }
#box { width: 100px; height: 100px; background-color: #0077ff; }
[data-layout-correct="false"] { background: #dd1144 !important; opacity: 1 !important; }
```

----------------------------------------

TITLE: Setting up Framer Motion Projections and Scrolling (JavaScript)
DESCRIPTION: Initializes Framer Motion projection nodes for a scroll container, a box, and a button. Sets the scroll position of the container, applies a scale transformation to the box, renders the visual elements, captures initial bounding boxes, and then updates the projections and verifies the layout using assertion functions within a requestAnimationFrame loop.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/single-element-element-scroll-scale.html#_snippet_1

LANGUAGE: JavaScript
CODE:
```
const { createNode } = window.Undo const { matchViewportBox } = window.Assert const { frame } = window.Projection const scroll = document.getElementById("scroll") const scrollProjection = createNode(scroll, undefined, { layoutScroll: true, }) const box = document.getElementById("box") const boxProjection = createNode(box, scrollProjection) const button = document.getElementById("button") const buttonProjection = createNode(button, boxProjection) const scrollDistance = 100 scroll.scrollTop = scrollDistance scroll.scrollLeft = scrollDistance boxProjection.setValue("scale", 2) boxProjection.options.visualElement.render() const boxOrigin = box.getBoundingClientRect() const buttonOrigin = button.getBoundingClientRect() requestAnimationFrame(() => { buttonProjection.willUpdate() boxProjection.willUpdate() boxProjection.root.didUpdate() matchViewportBox(box, boxOrigin) matchViewportBox(button, buttonOrigin) })
```

----------------------------------------

TITLE: Creating and Testing Framer Motion Projections (JavaScript)
DESCRIPTION: Uses Framer Motion's Animate and Projection APIs to create projection nodes for two elements (#box-a and #box-b). It sets a layoutId and duration, updates properties, and then uses assertion functions (matchViewportBox, matchVisibility, matchOpacity, matchBorderRadius) after a timeout to verify the final state of the elements' projections.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/shared-mix-finish.html#_snippet_1

LANGUAGE: JavaScript
CODE:
```
const { createNode } = window.Animate
const { matchViewportBox, matchVisibility, matchOpacity, matchBorderRadius, } = window.Assert
const { frame } = window.Projection
const box = document.getElementById("box-a")
const boxProjection = createNode(
 box,
 undefined,
 { layoutId: "a" },
 { duration: 0.05 }
)
boxProjection.willUpdate()
const newBox = document.createElement("div")
newBox.id = "box-b"
document.body.appendChild(newBox)
const newBoxProjection = createNode(
 newBox,
 undefined,
 { layoutId: "a", },
 { duration: 0.05 }
)
newBoxProjection.setValue("borderRadius", 20)
newBoxProjection.root.didUpdate()
const finBox = { bottom: 400, left: 100, right: 300, top: 100 }
setTimeout(() => {
 matchViewportBox(box, finBox)
 matchViewportBox(newBox, finBox)
 matchVisibility(box, "visible")
 matchVisibility(newBox, "visible")
 // should be 0/1 instead of approximate to 0/1
 matchOpacity(box, 0)
 matchOpacity(newBox, 1)
 /**\n * First box has an active transform applied whereas the second\n * box doesn't. As such only the first box needs a border-radius that\n * corrects for scale distortion.\n */
 matchBorderRadius(box, "10% / 6.66667%")
 matchBorderRadius(newBox, "20px")
}, 150)
```

----------------------------------------

TITLE: Creating and Updating Projection Nodes with JavaScript
DESCRIPTION: Uses window.Animate and window.Projection to create projection nodes for parent and child elements, updates their state by adding a class to the parent, and performs checks within nested requestAnimationFrame calls.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/perf-static-parent-child.skip.html#_snippet_1

LANGUAGE: javascript
CODE:
```
const { createNode, relativeEase } = window.Animate
const { matchViewportBox, checkFrame } = window.Assert
const { frame } = window.Projection
const parent = document.getElementById("parent")
const parentProjection = createNode(
  parent,
  undefined,
  {},
  { duration: 0.0001 }
)
const child = document.getElementById("child")
const childProjection = createNode(
  child,
  parentProjection,
  {},
  { duration: 0.1 }
)
parentProjection.willUpdate()
childProjection.willUpdate()
parent.classList.add("b")
parentProjection.root.didUpdate()
requestAnimationFrame(() => {
  requestAnimationFrame(() => {
    console.log(window.ProjectionFrames)
    checkFrame(parent, 2, {
      totalNodes: 3,
      resolvedTargetDeltas: 1,
      recalculatedProjection: 1,
    })
  })
})
```

----------------------------------------

TITLE: Initializing Layout Projection Nodes and Testing Position (JavaScript)
DESCRIPTION: Retrieves necessary objects and DOM elements, creates hierarchical projection nodes for the nested elements using a custom easing function, triggers layout updates, applies a class change to the parent, and asserts the final viewport position of the grandchild after rendering.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/animate-relative-nested-deep.html#_snippet_1

LANGUAGE: javascript
CODE:
```
const { frame, frameData } = window.Projection
const { createNode, relativeEase } = window.Animate
const { matchViewportBox } = window.Assert

const parent = document.getElementById("parent")
const mid = document.getElementById("mid")
const child = document.getElementById("child")
const grandchild = document.getElementById("grandchild")

const childOrigin = child.getBoundingClientRect()

let prevTimestamp = 0
let count = 0

const frameEasing = (t) => {
  // Increment ease if new frame
  if (prevTimestamp !== frameData.timestamp) {
    count++
  }
  prevTimestamp = frameData.timestamp

  return count < 2 ? t : 0.5
}

const parentProjection = createNode(
  parent,
  undefined,
  {},
  { duration: 200, ease: frameEasing }
)

const midProjection = createNode(
  mid,
  parentProjection,
  {},
  { duration: 200, ease: frameEasing }
)

const childProjection = createNode(
  child,
  midProjection,
  {},
  { duration: 200, ease: frameEasing }
)

const grandchildProjection = createNode(
  grandchild,
  childProjection,
  {},
  { duration: 200, ease: frameEasing }
)

parentProjection.willUpdate()
midProjection.willUpdate()
childProjection.willUpdate()
grandchildProjection.willUpdate()

parent.classList.add("b")

parentProjection.root.didUpdate()

frame.postRender(() => {
  frame.postRender(() => {
    matchViewportBox(
      grandchild,
      {
        bottom: 71,
        height: 20,
        left: 51,
        right: 71,
        top: 51,
        width: 20,
      },
      2
    )
  })
})
```

----------------------------------------

TITLE: Testing Layout Projection with Framer Motion (JavaScript)
DESCRIPTION: Uses Framer Motion's projection system to test layout changes. It creates projection nodes for a box and its child, modifies the box's class to trigger a layout change, updates projections, asserts the new layout, and then reverts the change, asserting the layout again.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/single-element-with-child-block-update.html#_snippet_1

LANGUAGE: javascript
CODE:
```
const { createNode } = window.Undo
const { matchViewportBox } = window.Assert
const { frame } = window.Projection
const box = document.getElementById("box")
const boxProjection = createNode(box)
const child = document.getElementById("child")
const childProjection = createNode(child, boxProjection)
const boxOrigin = box.getBoundingClientRect()
const childOrigin = child.getBoundingClientRect()
boxProjection.root.blockUpdate()
boxProjection.willUpdate()
childProjection.willUpdate()
box.classList.add("b") // should unblock update
boxProjection.root.didUpdate()
frame.postRender(() => {
  const boxNewLayout = {
    bottom: 240,
    left: 200,
    right: 440,
    top: 100,
  }
  const childNewLayout = {
    bottom: 170,
    left: 220,
    right: 270,
    top: 120,
  }
  matchViewportBox(box, boxNewLayout)
  matchViewportBox(child, childNewLayout)
  boxProjection.willUpdate()
  childProjection.willUpdate()
  box.classList.remove("b")
  boxProjection.root.didUpdate()
  frame.postRender(() => {
    matchViewportBox(box, boxNewLayout)
    matchViewportBox(child, childNewLayout)
  })
})
```

----------------------------------------

TITLE: Styling Elements for Layout Testing (CSS)
DESCRIPTION: Defines basic styles for the body and two box elements (#box-a, #box-b) used in layout tests. Includes styles for positioning, dimensions, background color, and a special rule for elements with data-layout-correct="false".
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/shared-mix-finish.html#_snippet_0

LANGUAGE: CSS
CODE:
```
body { padding: 0; margin: 0; } #box-a { width: 100px; height: 100px; background-color: #00cc88; } #box-b { position: absolute; top: 100px; left: 100px; width: 200px; height: 300px; background-color: #09f; } #trigger-overflow { width: 1px; height: 1px; position: absolute; top: 2000px; left: 2000px; } \[data-layout-correct="false"\] { background: #dd1144 !important; opacity: 0.5; }
```

----------------------------------------

TITLE: Styling Elements for Layout Test - CSS
DESCRIPTION: Defines basic styles for the body and two box elements used in a layout test, including initial dimensions, background colors, and positioning for #box-b. Also includes styles for a hidden element and a visual indicator for incorrect layout.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/shared-promote-new-mix.html#_snippet_0

LANGUAGE: css
CODE:
```
body { padding: 0; margin: 0; } #box-a { width: 100px; height: 100px; background-color: #00cc88; } #box-b { position: absolute; top: 100px; left: 100px; width: 200px; height: 300px; background-color: #09f; } #trigger-overflow { width: 1px; height: 1px; position: absolute; top: 2000px; left: 2000px; } \[data-layout-correct="false"\] { background: #dd1144 !important; opacity: 0.5; }
```

----------------------------------------

TITLE: Animating Layout with Framer Motion Projection in JavaScript
DESCRIPTION: Uses Framer Motion's Projection API to create projection nodes for existing elements (#box-a, #box-b, .child), sets initial values, and schedules post-render updates. It then dynamically creates a new child element, appends it to #box-b, creates a projection node for it with a shared layout ID, and schedules post-render assertions to match viewport box and opacity.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/shared-transform-parents-animate.html#_snippet_1

LANGUAGE: javascript
CODE:
```
const { createNode } = window.Animate
const { matchViewportBox, matchVisibility, matchOpacity } = window.Assert
const { frame } = window.Projection
const a = document.getElementById("box-a")
const b = document.getElementById("box-b")
const child = document.querySelector(".child")
const aProjection = createNode(a)
const bProjection = createNode(b)
const childProjection = createNode(child, aProjection, { layoutId: "child", })
aProjection.setValue("x", 100)
bProjection.setValue("x", -100)
frame.postRender(() => {
  aProjection.willUpdate()
  bProjection.willUpdate()
  childProjection.willUpdate()
  const newChild = document.createElement("div")
  newChild.classList.add("child")
  b.appendChild(newChild)
  const newChildProjection = createNode(newChild, bProjection, { layoutId: "child", })
  newChildProjection.root.didUpdate()
  frame.postRender(() => {
    matchViewportBox(child, newChild.getBoundingClientRect())
    matchOpacity(child, 1)
    matchOpacity(newChild, 1)
  })
})
```

----------------------------------------

TITLE: Styling Layout Elements with CSS
DESCRIPTION: Defines basic styles for body, parent, child, and grandchild elements, including size, color, positioning, and flexbox properties. Includes styles for a modified state (`.b`) and an overflow trigger.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/perf-parent-child-static-grandchild.html#_snippet_0

LANGUAGE: css
CODE:
```
body { padding: 0; margin: 0; } #parent { width: 100px; height: 100px; background-color: #00cc88; } #child { width: 50px; height: 50px; background-color: #0077ff; } #parent.b { width: 200px; position: absolute; top: 100px; left: 200px; padding: 20px; display: flex; justify-content: flex-end; } .b #child { width: 100px; height: 100px; } #grandChild { width: 100px; height: 100px; } #trigger-overflow { width: 1px; height: 1px; position: absolute; top: 2000px; left: 2000px; } \[data-layout-correct="false"\] { background: #dd1144 !important; opacity: 0.5; }
```

----------------------------------------

TITLE: Testing Element Projection and Layout Updates
DESCRIPTION: Uses window.Undo, window.Assert, and window.Projection (likely custom libraries) to create projection nodes for DOM elements (#box, #child). It captures initial bounding boxes, applies CSS classes (.b) to trigger layout changes, updates projections, and uses frame.postRender and matchViewportBox to assert that the elements' viewport positions match their original positions after the layout updates, potentially testing a layout correction mechanism.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/nested-layout-change-mid-projection.html#_snippet_1

LANGUAGE: javascript
CODE:
```
const { createNode } = window.Undo const { matchViewportBox } = window.Assert const { frame } = window.Projection const box = document.getElementById("box") const boxProjection = createNode(box) const child = document.getElementById("child") const childProjection = createNode(child, boxProjection) const boxOrigin = box.getBoundingClientRect() const childOrigin = child.getBoundingClientRect() boxProjection.willUpdate() childProjection.willUpdate() box.classList.add("b") child.classList.add("b") boxProjection.root.didUpdate() frame.postRender(() => { matchViewportBox(box, boxOrigin) matchViewportBox(child, childOrigin) // Second render childProjection.willUpdate() child.classList.add("b") boxProjection.root.didUpdate() frame.postRender(() => { matchViewportBox(box, boxOrigin) matchViewportBox(child, childOrigin) }) })
```

----------------------------------------

TITLE: Initializing Framer Motion Projection Nodes and Handling Scroll (JavaScript)
DESCRIPTION: Initializes Framer Motion projection nodes for a scroller, a sticky element, and a child element using utilities like `createNode`. It demonstrates appending a new child element, scrolling the container, updating the layout root, and asserting element positions after a delay.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/sticky-element-scroll-shared-child.html#_snippet_1

LANGUAGE: javascript
CODE:
```
const { createNode } = window.Undo
const { matchViewportBox, matchVisibility, matchOpacity, addPageScroll,
} = window.Assert
const { frame } = window.Projection
const scroller = document.getElementById("scroller")
const scrollerProjection = createNode(scroller, undefined, { layoutScroll: true,
})
const sticky = document.getElementById("sticky")
const stickyProjection = createNode(sticky, scrollerProjection)
const child = document.querySelector(".child")
const childProjection = createNode(child, stickyProjection, { layoutId: "child",
})
const childOrigin = child.getBoundingClientRect()
stickyProjection.willUpdate()
childProjection.willUpdate()
const newChild = document.createElement("div")
newChild.classList.add("child")
sticky.appendChild(newChild)
const newChildProjection = createNode(newChild, stickyProjection, { layoutId: "child",
})
scroller.scrollTo(0, 100)
stickyProjection.root.didUpdate()
setTimeout(() => {
 matchViewportBox(child, childOrigin)
 matchViewportBox(newChild, childOrigin)
}, 50)
```

----------------------------------------

TITLE: Styling Elements with CSS
DESCRIPTION: This CSS snippet provides basic styling for various elements on a page. It sets up a sticky box element, defines dimensions for other elements, and includes a rule to highlight elements where layout correction is incorrect.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/sticky-scroll-change-no-stick.html#_snippet_0

LANGUAGE: css
CODE:
```
body { padding: 0; margin: 0; }
#box {
  width: 100px;
  height: 100px;
  background-color: #00cc88;
  position: sticky;
  top: 0px;
}
#size { height: 100px; }
#trigger-overflow {
  width: 1px;
  height: 1px;
  position: absolute;
  top: 2000px;
  left: 2000px;
}
[data-layout-correct="false"] {
  background: #dd1144 !important;
  opacity: 0.5;
}
```

----------------------------------------

TITLE: Creating and Updating Projection Node with JavaScript
DESCRIPTION: This JavaScript snippet demonstrates how to create a projection node for a DOM element using a library (likely Framer Motion). It initializes the node, scrolls the window, updates the node's projection, and then asserts its position within the viewport.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/sticky-scroll-change-no-stick.html#_snippet_1

LANGUAGE: javascript
CODE:
```
const { createNode } = window.Undo
const { matchViewportBox, addPageScroll } = window.Assert
const { frame } = window.Projection
const box = document.getElementById("box")
const boxProjection = createNode(box, undefined, { layoutRoot: true, })
requestAnimationFrame(() => {
  boxProjection.willUpdate()
  const scrollOffset = [50, 50]
  window.scrollTo(...scrollOffset)
  boxProjection.root.didUpdate()
  matchViewportBox(box, {
    top: 50,
    left: -50,
    bottom: 150,
    right: 50,
  })
})
```

----------------------------------------

TITLE: Styling Elements for Layout Testing (CSS)
DESCRIPTION: Defines basic styles for body, a box element, a button, a trigger element, and a scroll container. Includes positioning, dimensions, background colors, and overflow properties used for layout and scrolling tests.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/single-element-element-scroll-scale.html#_snippet_0

LANGUAGE: CSS
CODE:
```
body { padding: 0; margin: 0; } #box { width: 300px; height: 100px; position: absolute; top: 200px; left: 50%; } #button { position: absolute; inset: 0; background-color: #00cc88; } #trigger-overflow { width: 1px; height: 1px; position: absolute; top: 2000px; left: 2000px; } [data-layout-correct="false"] { background: #dd1144 !important; opacity: 0.5; } #scroll { position: relative; width: 800px; height: 400px; overflow: scroll; }
```

----------------------------------------

TITLE: Manipulating Element Layout and Scroll with JavaScript
DESCRIPTION: Uses functions from global objects (likely part of a library) to create 'projection' nodes for DOM elements. It gets initial bounding boxes, adds the 'b' class to change styles, scrolls the window, updates projections, and performs post-render checks using 'matchViewportBox' and 'addPageScroll'. Requires 'window.Undo', 'window.Assert', and 'window.Projection'.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/single-element-with-child-layout-change-page-scroll.html#_snippet_1

LANGUAGE: javascript
CODE:
```
const { createNode } = window.Undo
const { matchViewportBox, addPageScroll } = window.Assert
const { frame } = window.Projection
const box = document.getElementById("box")
const boxProjection = createNode(box)
const child = document.getElementById("child")
const childProjection = createNode(child, boxProjection)
const boxOrigin = box.getBoundingClientRect()
const childOrigin = child.getBoundingClientRect()
boxProjection.willUpdate()
childProjection.willUpdate()
box.classList.add("b")
child.classList.add("b")
const scrollOffset = [50, 100]
window.scrollTo(...scrollOffset)
boxProjection.root.didUpdate()
frame.postRender(() => {
  matchViewportBox(box, addPageScroll(boxOrigin, ...scrollOffset))
  matchViewportBox(
    child, addPageScroll(childOrigin, ...scrollOffset)
  )
})
```

----------------------------------------

TITLE: Emulating Server-Side Rendering (JavaScript)
DESCRIPTION: Emulates server-side rendering by using `ReactDOMServer.renderToString` to generate the initial HTML string for the component and setting it as the `innerHTML` of the root element.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/optimized-appear/defer-handoff-layout-ancestor.html#_snippet_3

LANGUAGE: JavaScript
CODE:
```
// Emulate server rendering of element
root.innerHTML = ReactDOMServer.renderToString(
  React.createElement(Component)
)
```

----------------------------------------

TITLE: Testing Framer Motion mix Function (JavaScript)
DESCRIPTION: Utilizes the `window.Motion.mix` function to create an interpolator for CSS unit values (variables and pixels) and performs a loop to test its output and measure performance.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/benchmarks/mix-unit-value-framer-motion.html#_snippet_1

LANGUAGE: javascript
CODE:
```
const { mix } = window.Motion

/**
 * Create an interpolate function that mixes unit values.
 */
const px = mix("var(--test-1) 10px", "var(--test-9) 60px")

const numRuns = 10
let startTime = performance.now()

for (let i = 0; i < numRuns; i++) {
  console.log(i / numRuns)
  console.log(px(i / numRuns))
}

console.log(`First run: ${performance.now() - startTime}ms`)
```

----------------------------------------

TITLE: Framer Motion Projection Test with Scrolling (JavaScript)
DESCRIPTION: Initializes Framer Motion's projection system for a scrollable container and a box element. It sets initial scroll positions, creates projection nodes, updates a value (borderRadius) on the box projection, changes scroll positions, triggers updates, and uses post-render assertions to check the box's viewport position, opacity, and border radius.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/element-page-scroll-non-zero.html#_snippet_1

LANGUAGE: javascript
CODE:
```
const { createNode } = window.Undo
const { matchViewportBox, matchOpacity, matchBorderRadius, addPageScroll,
} = window.Assert
const { frame } = window.Projection
const scroll = document.getElementById("scroll")
const box = document.getElementById("box")
scroll.scrollLeft = 100
window.scrollTo(100, 100)
const boxOrigin = box.getBoundingClientRect()
const scrollProjection = createNode(scroll, undefined, { layoutScroll: true,
})
const boxProjection = createNode(box, scrollProjection)
boxProjection.setValue("borderRadius", 20)
boxProjection.willUpdate()
scroll.scrollLeft = 50
window.scrollTo(50, 50)
boxProjection.root.didUpdate()
frame.postRender(() => {
matchViewportBox(box, addPageScroll(boxOrigin, -100, -50))
matchOpacity(box, 1)
matchBorderRadius(box, "20px")
})
```

----------------------------------------

TITLE: Testing Layout Projection with Scrolling (JavaScript)
DESCRIPTION: Uses Framer Motion's internal testing utilities (`Undo`, `Assert`, `Projection`) to create projection nodes for elements, simulate scrolling, update projections, and assert the final viewport position of the box.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/sticky-child-scroll-change-offset.html#_snippet_1

LANGUAGE: JavaScript
CODE:
```
const { createNode } = window.Undo
const { matchViewportBox, addPageScroll } = window.Assert
const { frame } = window.Projection
const overlay = document.getElementById("overlay")
const overlayProjection = createNode(overlay, undefined, { layoutRoot: true, layout: true, })
const box = document.getElementById("box")
const boxProjection = createNode(box, overlayProjection)
const boxOrigin = box.getBoundingClientRect()
boxProjection.willUpdate()
overlay.classList.add("b")
const scrollOffset = \[50, 150\]
window.scrollTo(...scrollOffset)
boxProjection.root.didUpdate()
frame.postRender(() => {
  matchViewportBox(box, {
    top: 0,
    left: -50,
    bottom: 100,
    right: 50,
  })
})
```

----------------------------------------

TITLE: Styling Elements for Layout Projection (CSS)
DESCRIPTION: Defines basic styles for parent and child elements, including initial states and a modified state (`.b`) used for layout changes. Includes styles for overflow triggering and visual indicators for incorrect layout.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/perf-parent-child.html#_snippet_0

LANGUAGE: css
CODE:
```
body { padding: 0; margin: 0; } #parent { width: 100px; height: 100px; background-color: #00cc88; } #child { width: 50px; height: 50px; background-color: #0077ff; } #parent.b { width: 200px; position: absolute; top: 100px; left: 200px; padding: 20px; display: flex; justify-content: flex-end; } .b #child { width: 100px; height: 100px; } #trigger-overflow { width: 1px; height: 1px; position: absolute; top: 2000px; left: 2000px; } \[data-layout-correct="false"\] { background: #dd1144 !important; opacity: 0.5; }
```

----------------------------------------

TITLE: Setting Up Projection Nodes and Assertions (JavaScript)
DESCRIPTION: Uses helper functions from window.Animate, window.Assert, and window.Projection to create projection nodes for existing and newly created DOM elements. It sets specific properties (opacity, borderRadius, skewX) on these nodes and then uses frame.postRender to assert the final computed styles and layout properties of the elements.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/shared-promote-new-mix-skew.html#_snippet_1

LANGUAGE: javascript
CODE:
```
const { createNode } = window.Animate
const { matchViewportBox, matchVisibility, matchOpacity, matchBorderRadius, matchSkewX, } = window.Assert
const { frame } = window.Projection
const box = document.getElementById("box-a")
const boxProjection = createNode(box, undefined, { layoutId: "a" })
boxProjection.willUpdate()
const newBox = document.createElement("div")
newBox.id = "box-b"
document.body.appendChild(newBox)
const newBoxProjection = createNode(newBox, undefined, { layoutId: "a", })
boxProjection.setValue("opacity", 0.8)
newBoxProjection.setValue("borderRadius", 20)
newBoxProjection.setValue("skewX", 40)
newBoxProjection.root.didUpdate()
frame.postRender(() => {
  const bbox = newBox.getBoundingClientRect()
  matchViewportBox(box, bbox)
  matchVisibility(box, "visible")
  matchVisibility(newBox, "visible")
  matchOpacity(box, 0.8)
  matchOpacity(newBox, 1)
  matchSkewX(box, 40)
  matchSkewX(newBox, 40)
  matchBorderRadius(box, "6.66667% / 5%")
  matchBorderRadius(newBox, "6.66667% / 5%")
})
```

----------------------------------------

TITLE: DOM Manipulation, Projection, and Assertion Script
DESCRIPTION: This script retrieves DOM elements, uses functions from window.Undo and window.Assert (likely test utilities), creates "projection" nodes for elements 'a' and 'b', modifies a property (borderRadius), triggers a layout change by adding a class and scrolling, updates projections, and finally asserts the viewport box, opacity, and border radius of the elements after the changes and scroll.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/flexbox-siblings-to-grid-page-scroll.html#_snippet_1

LANGUAGE: JavaScript
CODE:
```
const { createNode } = window.Undo\nconst { matchOpacity, matchBorderRadius, matchViewportBox, addPageScroll, } = window.Assert\nconst { frame } = window.Projection\nconst container = document.getElementById("container")\nconst a = document.getElementById("a")\nconst b = document.getElementById("b")\nconst aOrigin = a.getBoundingClientRect()\nconst bOrigin = b.getBoundingClientRect()\nconst aProjection = createNode(a)\nconst bProjection = createNode(b)\naProjection.setValue("borderRadius", 20)\naProjection.willUpdate()\nbProjection.willUpdate()\ncontainer.classList.add("as-grid")\nwindow.scrollTo(50, 100)\naProjection.root.didUpdate()\nframe.postRender(() => {\n  matchViewportBox(a, addPageScroll(aOrigin, 50, 100))\n  matchViewportBox(b, addPageScroll(bOrigin, 50, 100))\n  matchOpacity(a, 1)\n  matchOpacity(b, 1)\n  matchBorderRadius(a, "13.3333% / 10%")\n  matchBorderRadius(b, "")\n})
```

----------------------------------------

TITLE: Testing Framer Motion Projection and Scroll (JavaScript)
DESCRIPTION: Uses Framer Motion's Projection and Undo utilities to create a projection node for an element, modify the element's class and position, scroll the window, and then assert that the element's viewport box matches its expected position after scrolling, using matchViewportBox and addPageScroll.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/single-element-layout-change-page-scroll.html#_snippet_1

LANGUAGE: javascript
CODE:
```
const { createNode } = window.Undo
const { matchViewportBox, addPageScroll } = window.Assert
const { frame } = window.Projection
const box = document.getElementById("box")
const boxProjection = createNode(box)
const boxOrigin = box.getBoundingClientRect()
boxProjection.willUpdate()
box.classList.add("b")
const scrollOffset = [50, 100]
window.scrollTo(...scrollOffset)
boxProjection.root.didUpdate()
frame.postRender(() => {
  matchViewportBox(box, addPageScroll(boxOrigin, ...scrollOffset))
})
```

----------------------------------------

TITLE: Setting up Layout Projection Nodes and Testing (JavaScript)
DESCRIPTION: Initializes projection nodes for screen containers and box elements using custom functions (`createNode`). Manipulates the DOM to create a new scrollable screen and a box within it. Performs layout matching tests after a delay using `matchViewportBox`.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/shared-scroll-b-a.html#_snippet_1

LANGUAGE: javascript
CODE:
```
const { createNode } = window.Undo
const { matchViewportBox, matchVisibility, matchOpacity } = window.Assert
const { frame } = window.Projection
const screen = document.querySelector(".screen")
const screenProjection = createNode(screen)
const box = document.getElementById("box-b")
const boxOrigin = box.getBoundingClientRect()
const boxProjection = createNode(box, screenProjection, { layoutId: "box", })
boxProjection.willUpdate()
const scrollScreen = document.createElement("div")
scrollScreen.classList.add("screen", "scroll")
document.body.appendChild(scrollScreen)
const newBox = document.createElement("div")
newBox.id = "box"
scrollScreen.appendChild(newBox)
scrollScreen.scrollTop = 1000
console.log(scrollScreen.scrollTop)
const scrollScreenProjection = createNode(scrollScreen, undefined)
const newBoxProjection = createNode(
  newBox, scrollScreenProjection, { layoutId: "box", }
)
boxProjection.root.didUpdate()
setTimeout(() => {
  matchViewportBox(box, boxOrigin)
  matchViewportBox(newBox, boxOrigin)
  matchViewportBox(box, newBox)
}, 50)
```

----------------------------------------

TITLE: Styling Box and Layout Correction - CSS
DESCRIPTION: Defines basic padding for the body, styles for the #box element including dimensions and background color, and a rule to visually highlight elements marked with `data-layout-correct="false"`.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/optimized-appear/interrupt-spring.html#_snippet_0

LANGUAGE: css
CODE:
```
body { padding: 100px; margin: 0; } #box { width: 100px; height: 100px; background-color: #0077ff; } \[data-layout-correct="false"\] { background: #dd1144 !important; opacity: 1 !important; }
```

----------------------------------------

TITLE: Testing Framer Motion Layout and Scroll Projection (JavaScript)
DESCRIPTION: This JavaScript code snippet uses a library (likely Framer Motion) to create projection nodes for a scrollable container and a box element within it. It updates a property (borderRadius) on the box's projection, simulates scrolling the container, updates the projection tree, and then asserts the final position, opacity, and border radius of the box after the next render cycle.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/element-scroll.html#_snippet_1

LANGUAGE: javascript
CODE:
```
const { createNode } = window.Undo
const { matchViewportBox, addPageScroll, matchOpacity, matchBorderRadius, } = window.Assert
const { frame } = window.Projection
const scroll = document.getElementById("scroll")
const box = document.getElementById("box")
const boxOrigin = box.getBoundingClientRect()
const scrollProjection = createNode(scroll, undefined, { layoutScroll: true, })
const boxProjection = createNode(box, scrollProjection)
boxProjection.setValue("borderRadius", 20)
boxProjection.willUpdate()
scroll.scrollLeft = 50
boxProjection.root.didUpdate()
frame.postRender(() => {
 matchViewportBox(box, addPageScroll(boxOrigin, 50, 0))
 matchOpacity(box, 1)
 matchBorderRadius(box, "20px")
})
```

----------------------------------------

TITLE: Styling for Layout Testing (CSS)
DESCRIPTION: Defines basic body styles, a position helper, and styles for sticky and fixed elements used in layout testing. Includes a trigger element for overflow and a style for incorrect layout states.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/sticky-shared-to-fixed-page-scroll-stick.html#_snippet_0

LANGUAGE: css
CODE:
```
body { padding: 0; margin: 0; } #position { width: 1px; height: 200px; } .sticky { position: sticky; top: 100px; width: 100%; height: 200px; background: #0088ff; } .fixed { position: fixed; top: 0; left: 0; width: 300px; height: 300px; background: #0088ff; } #trigger-overflow { width: 1px; height: 1px; position: absolute; top: 2000px; left: 2000px; } \[data-layout-correct="false"\] { background: #dd1144 !important; opacity: 0.5; }
```

----------------------------------------

TITLE: Testing Element Projection and Layout (JavaScript)
DESCRIPTION: Utilizes window utilities for Undo, Assert, and Projection. It creates a projection node for a box element, sets its scale, and uses a post-render hook to assert the element's viewport box matches an expected transformed state after projection updates.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/transform-single-with-scale.html#_snippet_1

LANGUAGE: javascript
CODE:
```
const { createNode } = window.Undo
const { matchViewportBox } = window.Assert
const { frame } = window.Projection
const box = document.getElementById("box")
const boxProjection = createNode(box)
const boxOrigin = box.getBoundingClientRect()
boxProjection.setValue("scale", 2)
frame.postRender(() => {
  const transformedBox = {
    top: -50,
    left: -50,
    right: 150,
    bottom: 150,
  }
  matchViewportBox(box, transformedBox)
  boxProjection.willUpdate()
  boxProjection.root.didUpdate()
  matchViewportBox(box, transformedBox)
})
```

----------------------------------------

TITLE: Styling Basic Elements for Layout Testing (CSS)
DESCRIPTION: Defines basic styles for body, two box elements (#box-a, #box-b), a trigger element (#trigger-overflow), and a rule for incorrect layout state. These styles set dimensions, positioning, background colors, and visual indicators for testing.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/shared-promote-new-mix-skew.html#_snippet_0

LANGUAGE: css
CODE:
```
body { padding: 0; margin: 0; } #box-a { width: 100px; height: 100px; background-color: #00cc88; } #box-b { position: absolute; top: 100px; left: 100px; width: 200px; height: 300px; background-color: #09f; } #trigger-overflow { width: 1px; height: 1px; position: absolute; top: 2000px; left: 2000px; } \[data-layout-correct="false"\] { background: #dd1144 !important; opacity: 0.5; }
```

----------------------------------------

TITLE: Setting up Framer Motion Projections for Layout Test (JavaScript)
DESCRIPTION: Initializes Framer Motion nodes (projections) for two elements (`#box-a` and `#box-b`) intended to share a layout ID. It simulates a layout transition by unmounting the first element's projection, removing the element, adding the second element, setting values on its projection, and then performing assertions on the final state after rendering.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/shared-promote-new-mix-rotate-remove.html#_snippet_1

LANGUAGE: JavaScript
CODE:
```
const { createNode } = window.Animate
const { matchViewportBox, matchVisibility, matchOpacity, matchBorderRadius, matchRotate,
} = window.Assert
const { frame } = window.Projection
const box = document.getElementById("box-a")
const boxProjection = createNode(box, undefined, { layoutId: "a" })
boxProjection.willUpdate()
const newBox = document.createElement("div")
newBox.id = "box-b"
document.body.appendChild(newBox)
const newBoxProjection = createNode(newBox, undefined, {
  layoutId: "a",
})
newBoxProjection.setValue("borderRadius", 20)
newBoxProjection.setValue("rotate", 40)
boxProjection.unmount()
document.body.removeChild(box)
newBoxProjection.root.didUpdate()
frame.postRender(() => {
  matchVisibility(newBox, "visible")
  matchOpacity(newBox, 1)
  matchRotate(newBox, 20)
  matchBorderRadius(newBox, "6.66667% / 5%")
})
```

----------------------------------------

TITLE: Styling Elements for Layout Transition (CSS)
DESCRIPTION: Defines basic CSS styles for the body and two box elements (`#box-a`, `#box-b`) involved in a layout transition. Includes dimensions, positioning, background colors, and styles for an overflow trigger and layout error highlighting.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/shared-promote-new.html#_snippet_0

LANGUAGE: css
CODE:
```
body { padding: 0; margin: 0; } #box-a { width: 100px; height: 100px; background-color: #00cc88; } #box-b { position: absolute; top: 100px; left: 100px; width: 200px; height: 300px; background-color: #09f; } #trigger-overflow { width: 1px; height: 1px; position: absolute; top: 2000px; left: 2000px; } \[data-layout-correct="false"\] { background: #dd1144 !important; opacity: 0.5; }
```

----------------------------------------

TITLE: Setting Up Layout Projection Test (JavaScript)
DESCRIPTION: Initializes projection nodes for the container, fixed, and child elements using a library's API (likely Framer Motion). It establishes parent-child relationships between nodes, simulates horizontal scrolling on the container, triggers layout updates, and uses assertions to verify the viewport positions of the fixed and child elements after the scroll.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/fixed-within-element-scroll.html#_snippet_1

LANGUAGE: JavaScript
CODE:
```
const { createNode } = window.Undo
const { matchViewportBox, matchVisibility, matchOpacity, addPageScroll,
} = window.Assert
const { frame } = window.Projection
const container = document.getElementById("container")
const containerProjection = createNode(container, undefined, { layoutScroll: true, layout: false,
})
const fixed = document.getElementById("fixed")
const fixedProjection = createNode(fixed, containerProjection, { layoutScroll: true, layout: true,
})
const fixedOrigin = fixed.getBoundingClientRect()
const child = document.getElementById("child")
const childProjection = createNode(child, fixedProjection)
const childOrigin = child.getBoundingClientRect()
childProjection.willUpdate()
fixedProjection.willUpdate()
container.scrollLeft = 200
fixedProjection.root.didUpdate()
setTimeout(() => {
  matchViewportBox(fixed, fixedOrigin)
  matchViewportBox(child, {
    top: 0,
    left: 0,
    right: 100,
    bottom: 100,
  })
}, 50)
```

----------------------------------------

TITLE: Styling Layout Elements with CSS
DESCRIPTION: Defines basic styles for body, parent, and child elements, including positioning, layout (flexbox), and a special style for incorrect layout states. Also includes a hidden element to trigger overflow.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/transform-nested-parent-scale-child-layout-change.html#_snippet_0

LANGUAGE: css
CODE:
```
body { padding: 0; margin: 0; } #parent { width: 100px; height: 100px; background-color: #00cc88; position: absolute; top: 100px; left: 100px; display: flex; justify-content: flex-start; align-items: flex-start; } #parent.b { justify-content: flex-end; align-items: flex-end; } #child { width: 50px; height: 50px; background-color: #0077ff; } #trigger-overflow { width: 1px; height: 1px; position: absolute; top: 2000px; left: 2000px; } \[data-layout-correct="false"\] { background: #dd1144 !important; opacity: 0.5; }
```

----------------------------------------

TITLE: Testing Layout with JavaScript and Projection Library
DESCRIPTION: Uses functions from window.Undo, window.Assert, and window.Projection to create and manipulate DOM nodes (.sticky, .fixed). It calculates initial positions, scrolls the window, appends a new fixed element, and then uses setTimeout to assert viewport box positions after scrolling.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/sticky-shared-to-fixed-page-scroll-no-stick.html#_snippet_1

LANGUAGE: javascript
CODE:
```
const { createNode } = window.Undo
const { matchViewportBox, matchVisibility, matchOpacity, addPageScroll,
} = window.Assert
const { frame } = window.Projection
const sticky = document.querySelector(".sticky")
const stickyProjection = createNode(sticky, undefined, { layoutId: "sticky", })
const stickyOrigin = sticky.getBoundingClientRect()
stickyProjection.willUpdate()
const scrollOffset = \[50, 50\]
window.scrollTo(...scrollOffset)
const fixed = document.createElement("div")
fixed.classList.add("fixed")
document.body.appendChild(fixed)
const fixedProjection = createNode(fixed, undefined, { layoutId: "sticky", })
fixedProjection.root.didUpdate()
setTimeout(() => {
 matchViewportBox(sticky, addPageScroll(stickyOrigin, 50, 50))
 matchViewportBox(fixed, addPageScroll(stickyOrigin, 50, 50))
}, 50)
```

----------------------------------------

TITLE: Setting up Framer Motion Projections and Testing Layout (JavaScript)
DESCRIPTION: Initializes Framer Motion nodes (projections) for box and child elements, captures their initial positions, applies a class change to the box to trigger a layout update, updates projections, and asserts their positions match the original after the layout change using a post-render hook.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/single-element-layout-change-with-child.html#_snippet_1

LANGUAGE: javascript
CODE:
```
const { createNode } = window.Undo
const { matchViewportBox } = window.Assert
const { frame } = window.Projection
const box = document.getElementById("box")
const boxProjection = createNode(box)
const child = document.getElementById("child")
const childProjection = createNode(child, boxProjection)
const boxOrigin = box.getBoundingClientRect()
const childOrigin = child.getBoundingClientRect()
boxProjection.willUpdate()
childProjection.willUpdate()
box.classList.add("b")
boxProjection.root.didUpdate()
frame.postRender(() => {
  matchViewportBox(box, boxOrigin)
  matchViewportBox(child, childOrigin)
})
```

----------------------------------------

TITLE: Basic Layout and Scroll Styles (CSS)
DESCRIPTION: Defines CSS rules for body padding/margin, element dimensions, scroll behavior, sticky positioning, background colors, and a style for indicating incorrect layout.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/sticky-element-scroll-shared-child.html#_snippet_0

LANGUAGE: css
CODE:
```
body { padding: 0; margin: 0; } #position { width: 1px; height: 200px; } #scroller { width: 200px; height: 200px; overflow: scroll; margin-left: 100px; } #sticky { position: sticky; top: 0; left: 0; width: 100%; height: 50px; background: #0088ff; display: flex; justify-content: space-between; } .child { width: 50px; height: 50px; background: #00cc88; } #content { width: 100%; height: 400px; } #trigger-overflow { width: 1px; height: 1px; position: absolute; top: 2000px; left: 2000px; } \[data-layout-correct="false"\] { background: #dd1144 !important; opacity: 0.5; }
```

----------------------------------------

TITLE: Setting up Framer Motion Projections and Animation - JavaScript
DESCRIPTION: Initializes Framer Motion Projection nodes for existing and newly created DOM elements. It sets up a hierarchy of projections, promotes them for animation, and schedules post-render updates to trigger layout changes and verify the results.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/shared-promote-needs-reset.html#_snippet_1

LANGUAGE: JavaScript
CODE:
```
const { createNode } = window.Animate
const { matchViewportBox, matchVisibility, matchOpacity, matchBorderRadius, } = window.Assert
const { frame } = window.Projection
const box = document.getElementById("box-a")
const boxProjection = createNode(box, undefined, { layoutId: "a", crossfade: true, })
const mid = document.getElementById("mid-a")
const midProjection = createNode(mid, boxProjection, { layoutId: "container", crossfade: true, })
const child = document.getElementById("child-a")
const childProjection = createNode(child, midProjection, { layoutId: "b", crossfade: true, })
boxProjection.willUpdate()
midProjection.willUpdate()
childProjection.willUpdate()
// mount & promote B
const newBox = document.createElement("div")
newBox.id = "box-b"
document.body.appendChild(newBox)
const newBoxProjection = createNode(newBox, undefined, { layoutId: "a", crossfade: true, })
const newMid = document.createElement("div")
newMid.id = "mid-b"
newBox.appendChild(newMid)
const newMidProjection = createNode(newMid, newBoxProjection, { layoutId: "container", crossfade: true, })
const newChild = document.createElement("div")
newChild.id = "child-b"
newMid.appendChild(newChild)
const newChildProjection = createNode(newChild, newMidProjection, { layoutId: "b", crossfade: true, })
const newBoxOrigin = newBox.getBoundingClientRect()
const newChildOrigin = newChild.getBoundingClientRect()
newBoxProjection.root.didUpdate()
// promote A and skip layout updates
boxProjection.root.blockUpdate()
boxProjection.willUpdate()
midProjection.willUpdate()
childProjection.willUpdate()
newBoxProjection.willUpdate()
newMidProjection.willUpdate()
newChildProjection.willUpdate()
boxProjection.promote({ needsReset: true })
midProjection.promote({ needsReset: true })
childProjection.promote({ needsReset: true })
childProjection.root.didUpdate()
frame.postRender(() => {
// only update child layout
childProjection.willUpdate()
child.classList.add("moved")
childProjection.root.didUpdate()
frame.postRender(() => {
const midChildBox = {
left: 0,
right: 50,
top: 75,
bottom: 125,
}
matchViewportBox(child, midChildBox)
})
})
```

----------------------------------------

TITLE: Styling Layout Elements (CSS)
DESCRIPTION: Defines basic styles, positioning, dimensions, and background colors for the elements used in the layout animation example. Includes a rule to visually indicate incorrect layout states.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/shared-relative-new-child.html#_snippet_0

LANGUAGE: css
CODE:
```
body { padding: 0; margin: 0; }
#box-a { position: absolute; top: 300px; left: 100px; width: 300px; height: 100px; background-color: #00cc88; }
#box-b { position: absolute; top: 0px; left: 100px; width: 300px; height: 400px; background-color: #8855ff; }
#child { position: absolute; top: 10px; right: 10px; width: 50px; height: 50px; background-color: #0077ff; }
#trigger-overflow { width: 1px; height: 1px; position: absolute; top: 2000px; left: 2000px; }
[data-layout-correct="false"] { background: #dd1144 !important; opacity: 0.5; }
```

----------------------------------------

TITLE: Testing Layout Projection with JavaScript
DESCRIPTION: Utilizes a projection library to create nodes for 'fixed' and 'child' elements. Simulates window scrolling, updates the 'fixed' element's position and content alignment, triggers layout updates, and asserts the final viewport positions of both elements.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/fixed-child-layout-change.html#_snippet_1

LANGUAGE: javascript
CODE:
```
const { createNode } = window.Undo
const { matchViewportBox, matchVisibility, matchOpacity, addPageScroll, } = window.Assert
const { frame } = window.Projection
const scrollDistance = 100
window.scrollTo(scrollDistance, scrollDistance)
const fixed = document.getElementById("fixed")
const fixedProjection = createNode(fixed, undefined, { layoutScroll: true, layout: true, })
const fixedOrigin = fixed.getBoundingClientRect()
const child = document.getElementById("child")
const childProjection = createNode(child, fixedProjection)
const childOrigin = child.getBoundingClientRect()
childProjection.willUpdate()
fixedProjection.willUpdate()
fixed.style.justifyContent = "flex-end"
fixed.style.top = "50px"
fixed.style.left = "50px"
fixedProjection.root.didUpdate()
setTimeout(() => {
  matchViewportBox(fixed, fixedOrigin)
  matchViewportBox(child, { top: 0, left: 0, right: 100, bottom: 100, })
}, 50)
```

----------------------------------------

TITLE: Testing Element Projection and Scaling (JavaScript)
DESCRIPTION: Uses Framer Motion's Projection and assertion utilities to create a projection node for a box element. It tests the element's viewport box before and after applying a scale transformation, asserting the expected layout changes.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/transform-single-with-scale-change.html#_snippet_1

LANGUAGE: javascript
CODE:
```
const { createNode } = window.Undo
const { matchViewportBox } = window.Assert
const { frame } = window.Projection
const box = document.getElementById("box")
const boxProjection = createNode(box)
const boxOrigin = box.getBoundingClientRect()
frame.postRender(() => {
  matchViewportBox(box, boxOrigin)
  boxProjection.willUpdate()
  boxProjection.setValue("scale", 2)
  const transformedBox = { top: -50, left: -50, right: 150, bottom: 150, }
  boxProjection.root.didUpdate()
  frame.postRender(() => {
    matchViewportBox(box, transformedBox)
  })
})
```

----------------------------------------

TITLE: Styling Elements with CSS
DESCRIPTION: Defines basic styles for body, parent, child, and grandchild elements, including dimensions, colors, and margins. It also includes a modified state for the parent element (`#parent.b`) with absolute positioning, padding, display flex, and justify-content properties, along with styles for a hidden overflow trigger and a data attribute selector.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/perf-static-parent-child-grandchild.skip.html#_snippet_0

LANGUAGE: css
CODE:
```
body { padding: 0; margin: 0; } #parent { width: 100px; height: 100px; background-color: #00cc88; } #child { width: 50px; height: 50px; background-color: #0077ff; } #parent.b { width: 200px; position: absolute; top: 100px; left: 200px; padding: 20px; display: flex; justify-content: flex-end; } #grandChild { width: 100px; height: 100px; } #trigger-overflow { width: 1px; height: 1px; position: absolute; top: 2000px; left: 2000px; } \[data-layout-correct="false"\] { background: #dd1144 !important; opacity: 0.5; }
```

----------------------------------------

TITLE: Styling Nested Elements with CSS
DESCRIPTION: Defines basic styles for the body and nested parent, mid, and child elements, including layout properties (position, display, flex), dimensions, background colors, and a specific style for a data attribute used for layout correction indication.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/animate-relative-nested.html#_snippet_0

LANGUAGE: css
CODE:
```
body { padding: 0; margin: 0; } #parent { position: relative; width: 200px; height: 200px; background-color: #00cc88; display: flex; align-items: center; justify-content: center; } #mid { width: 50px; height: 50px; background-color: white; display: flex; align-items: center; justify-content: center; } .b #mid { width: 100px; height: 100px; } #child { width: 50px; height: 50px; background-color: #0077ff; } [data-layout-correct="false"] { background: #dd1144 !important; opacity: 0.5; }
```

----------------------------------------

TITLE: Styling Parent and Child Elements with CSS
DESCRIPTION: Defines initial and modified styles for #parent and #child elements, including dimensions, colors, positioning, display properties, and a style for layout correctness checks.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/perf-static-parent-child.skip.html#_snippet_0

LANGUAGE: css
CODE:
```
body { padding: 0; margin: 0; }
#parent { width: 100px; height: 100px; background-color: #00cc88; }
#child { width: 50px; height: 50px; background-color: #0077ff; }
#parent.b { width: 200px; position: absolute; top: 100px; left: 200px; padding: 20px; display: flex; justify-content: flex-end; }
.b #child { width: 100px; height: 100px; }
#trigger-overflow { width: 1px; height: 1px; position: absolute; top: 2000px; left: 2000px; }
\[data-layout-correct="false"\] { background: #dd1144 !important; opacity: 0.5; }
```

----------------------------------------

TITLE: Styling Layout Elements (CSS)
DESCRIPTION: Defines basic styles for the body and nested parent, mid, and child elements, including positioning and dimensions. Includes a rule to modify the 'mid' element's position when the parent has class 'b' and a rule to highlight incorrect layout elements.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/animate-relative-mixed-transition.html#_snippet_0

LANGUAGE: css
CODE:
```
body { padding: 0; margin: 0; } #parent { position: relative; width: 200px; height: 200px; background-color: #00cc88; } #mid { position: absolute; width: auto; height: auto; left: 0; top: 0; } .b #mid { left: 100px; } #child { width: 50px; height: 50px; background-color: #0077ff; } \[data-layout-correct="false"\] { background: #dd1144 !important; opacity: 0.5; }
```

----------------------------------------

TITLE: Styling Basic Elements with CSS
DESCRIPTION: Defines basic styles for the body, parent, and child elements, including dimensions, background colors, and positioning. It also includes a style for a modified parent class (#parent.b) and a hidden element (#trigger-overflow), plus a rule for elements with data-layout-correct="false".
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/nested-layout-change-scale-correction.html#_snippet_0

LANGUAGE: css
CODE:
```
body { padding: 0; margin: 0; } #parent { width: 200px; height: 200px; background-color: #00cc88; } #child { width: 100px; height: 100px; background-color: #09f; } #parent.b { width: 400px; } #trigger-overflow { width: 1px; height: 1px; position: absolute; top: 2000px; left: 2000px; } [data-layout-correct="false"] { background: #dd1144 !important; opacity: 0.5; }
```

----------------------------------------

TITLE: Creating and Updating Projection Nodes with JavaScript
DESCRIPTION: Uses functions from `window.Undo`, `window.Assert`, and `window.Projection` to create and manipulate projection nodes. It gets DOM elements for an overlay and a box, creates projection nodes for them (linking the box node to the overlay node), gets the box's initial bounding rectangle, updates the box projection, scrolls the window, updates the root projection, and finally asserts the box's viewport position.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/sticky-scroll-no-layout-change.html#_snippet_1

LANGUAGE: javascript
CODE:
```
const { createNode } = window.Undo
const { matchViewportBox, addPageScroll } = window.Assert
const { frame } = window.Projection

const overlay = document.getElementById("overlay")
const overlayProjection = createNode(overlay, undefined, {
  layoutScroll: true,
  layout: false,
})

const box = document.getElementById("box")
const boxProjection = createNode(
  box,
  overlayProjection
  // undefined,
  // { duration: 1 }
)

const boxOrigin = box.getBoundingClientRect()

boxProjection.willUpdate()

const scrollOffset = [50, 100]
window.scrollTo(...scrollOffset)

boxProjection.root.didUpdate()

matchViewportBox(box, {
  top: 200,
  left: -50,
  bottom: 300,
  right: 50,
})
```

----------------------------------------

TITLE: Styling Box Element with CSS
DESCRIPTION: Defines basic styles for the body and a box element, including initial size, background, and absolute positioning with padding when a class 'b' is added. Also includes a hidden element and a style for incorrect layout state.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/transform-single-layout-change-with-scale.html#_snippet_0

LANGUAGE: css
CODE:
```
body { padding: 0; margin: 0; } #box { width: 100px; height: 100px; background-color: #00cc88; } #box.b { width: 200px; position: absolute; top: 100px; left: 200px; padding: 20px; } #trigger-overflow { width: 1px; height: 1px; position: absolute; top: 2000px; left: 2000px; } \[data-layout-correct="false"\] { background: #dd1144 !important; opacity: 0.5; }
```

----------------------------------------

TITLE: Initializing Framer Motion Projections and Assertions (JavaScript)
DESCRIPTION: Imports necessary utilities from Framer Motion (Undo, Assert, Projection), retrieves DOM elements, sets initial scroll position, creates projection nodes for the scroll container and a box element, sets a value on the box projection, triggers updates, modifies the scroll element's class, and uses `frame.postRender` to assert the box's viewport position, opacity, and border radius after layout changes, specifically noting not to animate from the previous position if scroll changed.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/element-scroll-to-layout.html#_snippet_1

LANGUAGE: JavaScript
CODE:
```
const { createNode } = window.Undo
const { matchViewportBox, matchOpacity, matchBorderRadius, addPageScroll,
} = window.Assert
const { frame } = window.Projection
const scroll = document.getElementById("scroll")
const box = document.getElementById("box")
const boxOrigin = box.getBoundingClientRect()
scroll.scrollLeft = 50
const scrollProjection = createNode(scroll, undefined, { layoutScroll: true,
})
const boxProjection = createNode(box, scrollProjection)
boxProjection.setValue("borderRadius", 20)
boxProjection.willUpdate()
scroll.classList.add("b")
boxProjection.root.didUpdate()
/**
 * Don't animate the box from its previous position if the scroll has changed.
 */
frame.postRender(() => {
  matchViewportBox(box, boxOrigin)
  matchOpacity(box, 1)
  matchBorderRadius(box, "20%")
})
```

----------------------------------------

TITLE: Styling Elements for Layout Test (CSS)
DESCRIPTION: Defines basic styles for the body and a box element, including a modified state '.b' and a trigger element for overflow testing. Also includes a style for indicating layout correctness.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/single-element-layout-change.html#_snippet_0

LANGUAGE: css
CODE:
```
body { padding: 0; margin: 0; }
#box { width: 100px; height: 100px; background-color: #00cc88; }
#box.b { width: 200px; position: absolute; top: 100px; left: 200px; padding: 20px; }
#trigger-overflow { width: 1px; height: 1px; position: absolute; top: 2000px; left: 2000px; }
\[data-layout-correct="false"\] { background: #dd1144 !important; opacity: 0.5; }
```

----------------------------------------

TITLE: Animating Element with Framer Motion Projection (JavaScript)
DESCRIPTION: Uses Framer Motion's Undo and Projection features to create a projection node for the '#box' element. It sets a rotation value, captures the initial layout, updates the element's class to trigger a layout change, updates the projection, and then asserts the layout correctness after rendering.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/single-element-layout-change-rotate.html#_snippet_1

LANGUAGE: javascript
CODE:
```
const { createNode } = window.Undo
const { matchViewportBox } = window.Assert
const { frame } = window.Projection
const box = document.getElementById("box")
const boxProjection = createNode(box)
boxProjection.setValue("rotate", 45)
requestAnimationFrame(() => {
  const boxOrigin = box.getBoundingClientRect()
  boxProjection.willUpdate()
  box.classList.add("b")
  boxProjection.root.didUpdate()
  frame.postRender(() => {
    matchViewportBox(box, boxOrigin)
  })
})
```

----------------------------------------

TITLE: Styling Parent and Child Elements (CSS)
DESCRIPTION: Defines basic styles for parent and child elements, including initial dimensions and background colors. It also includes a class 'b' to modify the parent's layout and position, and styles for the child within the modified parent.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/animate-relative-interrupt.html#_snippet_0

LANGUAGE: css
CODE:
```
body { padding: 0; margin: 0; } #parent { width: 100px; height: 100px; background-color: #00cc88; } #child { width: 50px; height: 50px; background-color: #0077ff; } #parent.b { width: 200px; position: absolute; top: 100px; left: 200px; padding: 20px; display: flex; justify-content: flex-end; } .b #child { width: 100px; height: 100px; } #trigger-overflow { width: 1px; height: 1px; position: absolute; top: 2000px; left: 2000px; } \[data-layout-correct="false"\] { background: #dd1144 !important; opacity: 0.5; }
```

----------------------------------------

TITLE: Setting up Framer Motion Projection Nodes and Testing (JavaScript)
DESCRIPTION: Initializes Framer Motion projection nodes for existing ('a') and newly created ('b') DOM elements. It triggers layout updates and uses assertion functions (`checkFrame`) within `requestAnimationFrame` loops to verify the state of the projection tree.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/perf-shared-single.html#_snippet_1

LANGUAGE: javascript
CODE:
```
const { createNode } = window.Animate
const { matchViewportBox, checkFrame } = window.Assert
const { frame } = window.Projection
const a = document.getElementById("a")
const aProjection = createNode(
  a,
  undefined,
  { layoutId: "box" },
  { duration: 0.1 }
)
aProjection.willUpdate()
const b = document.createElement("b")
b.id = "b"
document.body.appendChild(b)
const bProjection = createNode(
  b,
  undefined,
  { layoutId: "box" },
  { duration: 0.1 }
)
aProjection.root.didUpdate()
requestAnimationFrame(() => {
  requestAnimationFrame(() => {
    console.log(window.ProjectionFrames)
    checkFrame(a, 1, {
      totalNodes: 3,
      resolvedTargetDeltas: 1, // We only need to resolve a target for the lead node
      recalculatedProjection: 2, // But recalculate a projection for both
    })
  })
})
```

----------------------------------------

TITLE: Styling Elements for Layout Test (CSS)
DESCRIPTION: Defines initial and modified styles for `#box` and `#child` elements, including dimensions, positioning, and padding, used to test layout changes. Includes styles for a trigger element to force overflow and visual indicators for incorrect layout assertion results.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/single-element-with-child-layout-change-interrupt.html#_snippet_0

LANGUAGE: css
CODE:
```
body { padding: 0; margin: 0; }
#box { width: 100px; height: 100px; background-color: #00cc88; }
#child { width: 50px; height: 50px; background-color: #0077ff; }
#box.b { width: 200px; position: absolute; top: 100px; left: 200px; padding: 20px; }
#child.b { width: 100px; position: absolute; top: 10px; left: 10px; padding: 10px; }
#trigger-overflow { width: 1px; height: 1px; position: absolute; top: 2000px; left: 2000px; }
[data-layout-correct="false"] { background: #dd1144 !important; opacity: 0.5; }
```

----------------------------------------

TITLE: Styling Layout Elements (CSS)
DESCRIPTION: Defines basic styles for the body and specific elements used in a layout test, including dimensions, positioning (fixed), background colors, and flexbox alignment. Also includes a rule for highlighting incorrect layout states.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/fixed-child-to-static.html#_snippet_0

LANGUAGE: CSS
CODE:
```
body { padding: 0; margin: 0; } #container { width: 100px; height: 100px; } #fixed { position: fixed; top: 0; left: 0; width: 500px; height: 100px; background: #00cc88; display: flex; align-items: flex-start; } #child { width: 100px; height: 100px; background: #0077ff; } #trigger-overflow { width: 1px; height: 1px; position: absolute; top: 2000px; left: 2000px; } [data-layout-correct="false"] { background: #dd1144 !important; opacity: 0.5; }
```

----------------------------------------

TITLE: Styling Elements for Layout Test (CSS)
DESCRIPTION: Defines basic styles for the body, box, and child elements, including a modified state for the box (`#box.b`) used in the layout test. Also includes a trigger element and a style for incorrect layout states.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/single-element-with-child-block-update.html#_snippet_0

LANGUAGE: css
CODE:
```
body { padding: 0; margin: 0; } #box { width: 100px; height: 100px; background-color: #00cc88; } #child { width: 50px; height: 50px; background-color: #0077ff; } #box.b { width: 200px; position: absolute; top: 100px; left: 200px; padding: 20px; } #trigger-overflow { width: 1px; height: 1px; position: absolute; top: 2000px; left: 2000px; } \[data-layout-correct="false"\] { background: #dd1144 !important; opacity: 0.5; }
```

----------------------------------------

TITLE: Styling Layout Elements (CSS)
DESCRIPTION: Defines basic styles for the body, screen containers, scrollable areas, and specific box elements (#box, #box-b). Includes a rule to highlight elements with incorrect layout data.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/shared-scroll-b-a-animate.html#_snippet_0

LANGUAGE: css
CODE:
```
body { padding: 0; margin: 0; } .screen { width: 100%; height: 100%; position: fixed; inset: 0; overflow: hidden; } .scroll { overflow-y: scroll; } #box { margin-top: 1000px; width: 100px; height: 100px; background-color: #0088ff; } #box-b { position: absolute; top: 100px; left: 100px; width: 200px; height: 200px; background-color: #0088ff; } \[data-layout-correct="false"\] { background: #dd1144 !important; opacity: 0.5; }
```

----------------------------------------

TITLE: Styling Layout Elements (CSS)
DESCRIPTION: Defines basic styles for body, parent, and child elements, including positioning, dimensions, background colors, and flexbox properties. Includes a class 'b' for alternative layout and a data attribute selector for incorrect layout indication.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/transform-nested-parent-layout-change-scale-child-layout-change-transform.html#_snippet_0

LANGUAGE: css
CODE:
```
body { padding: 0; margin: 0; }
#parent { width: 100px; height: 100px; background-color: #00cc88; position: absolute; top: 100px; left: 100px; display: flex; justify-content: flex-start; align-items: flex-start; }
#parent.b { justify-content: flex-end; align-items: flex-end; top: 200px; left: 200px; }
#child { width: 50px; height: 50px; background-color: #0077ff; }
#trigger-overflow { width: 1px; height: 1px; position: absolute; top: 2000px; left: 2000px; }
[data-layout-correct="false"] { background: #dd1144 !important; opacity: 0.5; }
```

----------------------------------------

TITLE: Styling Layout Elements (CSS)
DESCRIPTION: Defines basic CSS styles for the body, a container, a fixed element, a child element within the fixed element, and a trigger element for overflow. Includes positioning, dimensions, background colors, flexbox properties, and a style to indicate incorrect layout.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/fixed-child-page-scroll.html#_snippet_0

LANGUAGE: css
CODE:
```
body { padding: 0; margin: 0; } #container { width: 100px; height: 100px; } #fixed { position: fixed; top: 0; left: 0; width: 500px; height: 100px; background: #00cc88; display: flex; align-items: flex-start; } #child { width: 100px; height: 100px; background: #0077ff; } #trigger-overflow { width: 1px; height: 1px; position: absolute; top: 2000px; left: 2000px; } \[data-layout-correct="false"\] { background: #dd1144 !important; opacity: 0.5; }
```

----------------------------------------

TITLE: Styling Elements for Layout Testing (CSS)
DESCRIPTION: Defines basic styles for body, parent, and child elements, including positioning, dimensions, background colors, and flexbox properties. It includes a class 'b' for the parent to change its layout and position, and a rule for elements with `data-layout-correct="false"` to indicate layout errors.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/transform-nested-parent-layout-change-scale-child-layout-change.html#_snippet_0

LANGUAGE: css
CODE:
```
body { padding: 0; margin: 0; } #parent { width: 100px; height: 100px; background-color: #00cc88; position: absolute; top: 100px; left: 100px; display: flex; justify-content: flex-start; align-items: flex-start; } #parent.b { justify-content: flex-end; align-items: flex-end; top: 200px; left: 200px; } #child { width: 50px; height: 50px; background-color: #0077ff; } #trigger-overflow { width: 1px; height: 1px; position: absolute; top: 2000px; left: 2000px; } [data-layout-correct="false"] { background: #dd1144 !important; opacity: 0.5; }
```

----------------------------------------

TITLE: Styling Elements for Layout Test (CSS)
DESCRIPTION: Defines basic styles for parent and child elements, including initial dimensions and background colors. It also includes a class 'b' to modify dimensions, position, and layout properties for the parent and child, and a hidden element to trigger overflow.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/animate-relative-parent-delayed.html#_snippet_0

LANGUAGE: css
CODE:
```
body { padding: 0; margin: 0; } #parent { width: 100px; height: 100px; background-color: #00cc88; } #child { width: 50px; height: 50px; background-color: #0077ff; } #parent.b { width: 200px; position: absolute; top: 100px; left: 200px; padding: 20px; display: flex; justify-content: flex-end; } .b #child { width: 100px; height: 100px; } #trigger-overflow { width: 1px; height: 1px; position: absolute; top: 2000px; left: 2000px; } \[data-layout-correct="false"\] { background: #dd1144 !important; opacity: 0.5; }
```

----------------------------------------

TITLE: Styling Fixed and Child Elements (CSS)
DESCRIPTION: Defines basic CSS styles for the body, a container, a fixed element, a child element, and a trigger for overflow. Includes styles for positioning, dimensions, background colors, display properties, and a data attribute selector for testing.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/fixed-child-page-scroll-layout-change.html#_snippet_0

LANGUAGE: css
CODE:
```
body { padding: 0; margin: 0; } #container { width: 100px; height: 100px; } #fixed { position: fixed; top: 0; left: 0; width: 500px; height: 100px; background: #00cc88; display: flex; align-items: flex-start; } #child { width: 100px; height: 100px; background: #0077ff; } #trigger-overflow { width: 1px; height: 1px; position: absolute; top: 2000px; left: 2000px; } \[data-layout-correct="false"\] { background: #dd1144 !important; opacity: 0.5; }
```

----------------------------------------

TITLE: Styling Box and Layout Trigger (CSS)
DESCRIPTION: Defines basic styles for a box element (#box), including its initial state and a modified state (#box.b), and a small trigger element (#trigger-overflow) positioned far off-screen. It also includes a style for elements with data-layout-correct="false" to visually indicate layout issues.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/transform-single-layout-change-with-translate.html#_snippet_0

LANGUAGE: CSS
CODE:
```
body { padding: 0; margin: 0; }
#box { width: 100px; height: 100px; background-color: #00cc88; }
#box.b { width: 200px; position: absolute; top: 100px; left: 200px; padding: 20px; }
#trigger-overflow { width: 1px; height: 1px; position: absolute; top: 2000px; left: 2000px; }
\[data-layout-correct="false"\] { background: #dd1144 !important; opacity: 0.5; }
```

----------------------------------------

TITLE: Initializing Projection Nodes and Testing Layout (JavaScript)
DESCRIPTION: Initializes projection nodes for a box and a button using `window.Undo` and `window.Projection`. Scrolls the window, applies a scale transformation to the box, renders the visual element, and then uses `requestAnimationFrame` to update projections and assert their viewport positions against original bounding rectangles.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/single-element-page-scroll-scale.html#_snippet_1

LANGUAGE: javascript
CODE:
```
const { createNode } = window.Undo
const { matchViewportBox, addPageScroll } = window.Assert
const { frame } = window.Projection
const box = document.getElementById("box")
const boxProjection = createNode(box)
const button = document.getElementById("button")
const buttonProjection = createNode(button, boxProjection)
const scrollDistance = 100
window.scrollTo(scrollDistance, scrollDistance)
boxProjection.setValue("scale", 2)
boxProjection.options.visualElement.render()
const boxOrigin = box.getBoundingClientRect()
const buttonOrigin = button.getBoundingClientRect()
requestAnimationFrame(() => {
  buttonProjection.willUpdate()
  boxProjection.willUpdate()
  boxProjection.root.didUpdate()
  matchViewportBox(box, boxOrigin)
  matchViewportBox(button, buttonOrigin)
})
```

----------------------------------------

TITLE: Importing Dependencies and Initializing Variables (JavaScript)
DESCRIPTION: Imports necessary functions and objects from the global `window.Motion` and `window.Assert` objects, gets the root element, defines animation duration, and initializes a `motionValue`.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/optimized-appear/defer-handoff-layout-ancestor.html#_snippet_1

LANGUAGE: JavaScript
CODE:
```
const { motion, animateStyle, animate, startOptimizedAppearAnimation, optimizedAppearDataAttribute, motionValue, frame, } = window.Motion
const { matchViewportBox } = window.Assert
const root = document.getElementById("root")
const duration = 0.5
const x = motionValue(0)
let isFirstFrame = true
```

----------------------------------------

TITLE: JavaScript Setup and Imports
DESCRIPTION: Imports necessary modules and variables from global objects (window.Motion, window.Assert), gets the root DOM element, and defines constants and a motion value used throughout the script.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/optimized-appear/defer-handoff-layout.html#_snippet_1

LANGUAGE: javascript
CODE:
```
const { motion, animateStyle, animate, startOptimizedAppearAnimation, optimizedAppearDataAttribute, motionValue, frame, } = window.Motion
const { matchViewportBox } = window.Assert
const root = document.getElementById("root")
const duration = 0.5
const x = motionValue(0)
let isFirstFrame = true
```

----------------------------------------

TITLE: Styling Layout Elements with CSS
DESCRIPTION: Defines basic styles for body, box, and child elements, including size, background color, and positioning. Includes styles for a modified state ('b' class) and an overflow trigger.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/single-element-layout-change-with-child-page-scroll.html#_snippet_0

LANGUAGE: css
CODE:
```
body { padding: 0; margin: 0; } #box { width: 100px; height: 100px; background-color: #00cc88; } #child { width: 50px; height: 50px; background-color: #0077ff; } #box.b { width: 200px; position: absolute; top: 100px; left: 200px; padding: 20px; } #child.b { width: 100px; position: absolute; top: 150px; left: 250px; padding: 10px; } #trigger-overflow { width: 1px; height: 1px; position: absolute; top: 2000px; left: 2000px; } \[data-layout-correct="false"\] { background: #dd1144 !important; opacity: 0.5; }
```

----------------------------------------

TITLE: Setting up Projection Nodes and Verifying Layout (JavaScript)
DESCRIPTION: Imports necessary functions, retrieves DOM elements, and creates a hierarchy of projection nodes for the parent, mid, and child elements. It triggers updates, applies a class to the parent to change layout, and uses 'matchViewportBox' within 'frame.postRender' to assert the final positions of the mid and child elements.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/animate-relative-mixed-transition.html#_snippet_1

LANGUAGE: javascript
CODE:
```
const { createNode, relativeEase } = window.Animate
const { matchViewportBox } = window.Assert
const { frame } = window.Projection

const parent = document.getElementById("parent")
const mid = document.getElementById("mid")
const child = document.getElementById("child")

const parentProjection = createNode(
  parent,
  undefined,
  {},
  { duration: 0 }
)
const midProjection = createNode(
  mid,
  parentProjection,
  {},
  { duration: 0 }
)
const childProjection = createNode(
  child,
  midProjection,
  {},
  { type: false }
)

parentProjection.willUpdate()
midProjection.willUpdate()
childProjection.willUpdate()

parent.classList.add("b")

parentProjection.root.didUpdate()

frame.postRender(() => {
  frame.postRender(() => {
    matchViewportBox(mid, {
      bottom: 50,
      left: 100,
      right: 150,
      top: 0,
    })
    matchViewportBox(child, {
      bottom: 50,
      left: 100,
      right: 150,
      top: 0,
    })
  })
})
```

----------------------------------------

TITLE: Testing Element Projection and Layout Changes (JavaScript)
DESCRIPTION: Uses functions from 'Undo', 'Assert', and 'Projection' libraries to create a projection node for an element, apply CSS class changes to trigger layout updates, and assert the element's viewport box after updates. It performs two layout changes and assertions using 'frame.postRender'.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/single-element-block-update.html#_snippet_1

LANGUAGE: javascript
CODE:
```
const { createNode } = window.Undo
const { matchViewportBox } = window.Assert
const { frame } = window.Projection
const box = document.getElementById("box")
const boxProjection = createNode(box)
const boxOrigin = box.getBoundingClientRect()

boxProjection.root.blockUpdate()
boxProjection.willUpdate()
box.classList.add("b") // should unblock update
boxProjection.root.didUpdate()

frame.postRender(() => {
  const boxNewLayout = {
    left: 200,
    top: 100,
    right: 440,
    bottom: 240,
  }
  matchViewportBox(box, boxNewLayout)

  boxProjection.willUpdate()
  box.classList.remove("b")
  boxProjection.root.didUpdate()

  frame.postRender(() => {
    matchViewportBox(box, boxNewLayout)
  })
})
```

----------------------------------------

TITLE: Testing Fixed Element Projection with Scroll in Framer Motion
DESCRIPTION: Uses internal Framer Motion utilities (Undo, Assert, Projection) to create a projection node for a fixed element, simulate page scrolling, update the projection tree, and then assert the fixed element's viewport position after a short delay.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/fixed-page-scroll.html#_snippet_1

LANGUAGE: JavaScript
CODE:
```
const { createNode } = window.Undo
const { matchViewportBox, matchVisibility, matchOpacity, addPageScroll,
} = window.Assert
const { frame } = window.Projection
const fixed = document.getElementById("fixed")
const fixedProjection = createNode(fixed, undefined, { layoutScroll: true, layout: true, })
const fixedOrigin = fixed.getBoundingClientRect()
fixedProjection.willUpdate()
const scrollDistance = 100
window.scrollTo(scrollDistance, scrollDistance)
fixedProjection.root.didUpdate()
setTimeout(() => {
matchViewportBox(fixed, fixedOrigin)
}, 50)
```

----------------------------------------

TITLE: Animating Elements with Projection Nodes (JavaScript)
DESCRIPTION: Initializes projection nodes for parent and child elements using a library. It captures initial positions, applies a class to change layout, triggers layout updates, and then uses `matchViewportBox` within `frame.postRender` callbacks to assert or verify element positions after layout changes and animations.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/animate-relative-interrupt.html#_snippet_1

LANGUAGE: javascript
CODE:
```
const { createNode, relativeEase } = window.Animate
const { matchViewportBox } = window.Assert
const { frame } = window.Projection
const parent = document.getElementById("parent")
const child = document.getElementById("child")
const parentOrigin = parent.getBoundingClientRect()
const childOrigin = child.getBoundingClientRect()
const parentProjection = createNode(
  parent,
  undefined,
  {},
  { duration: 1000, ease: relativeEase() }
)
const childProjection = createNode(
  child,
  parentProjection,
  {},
  { duration: 200, ease: relativeEase() }
)
parentProjection.willUpdate()
childProjection.willUpdate()
parent.classList.add("b")
parentProjection.root.didUpdate()
frame.postRender(() => {
  matchViewportBox(parent, parentOrigin)
  matchViewportBox(child, childOrigin)
  frame.postRender(() => {
    matchViewportBox(parent, { top: 50, bottom: 170, left: 100, right: 270, })
    matchViewportBox(child, { top: 60, bottom: 135, left: 160, right: 235, })
    parentProjection.willUpdate()
    childProjection.willUpdate()
    parent.classList.remove("b")
    parentProjection.root.didUpdate()
    frame.postRender(() => {
      frame.postRender(() => {
        matchViewportBox(parent, { top: 25, bottom: 135, height: 110, left: 50, right: 185, })
        matchViewportBox(child, { top: 30, bottom: 92.5, height: 62.5, left: 80, right: 142.5, })
      })
    })
  })
})
```

----------------------------------------

TITLE: Animating Element Projection with JavaScript
DESCRIPTION: Uses window.Animate and window.Projection to create a projection node for an element, modifies its class to trigger a style change, updates the projection, and then reverts the class and matches the element's viewport box against its original position after rendering frames.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/animate-undo-layout-change.html#_snippet_1

LANGUAGE: JavaScript
CODE:
```
const { createNode, relativeEase } = window.Animate
const { matchViewportBox } = window.Assert
const { frame } = window.Projection
const parent = document.getElementById("parent")
const parentOrigin = parent.getBoundingClientRect()
const parentProjection = createNode(
 parent, undefined, {}, { duration: 2 }
)
parentProjection.willUpdate()
parent.classList.add("b")
parentProjection.root.didUpdate()
frame.postRender(() => {
 parentProjection.willUpdate()
 parent.classList.remove("b")
 parentProjection.root.didUpdate()
 frame.postRender(() => {
 matchViewportBox(parent, parentOrigin, 0.5)
 })
})
```

----------------------------------------

TITLE: Initializing Framer Motion Projection Nodes and Testing Layout (JavaScript)
DESCRIPTION: Retrieves DOM elements, initializes Framer Motion Projection nodes for parent and child, applies a class 'b' to trigger layout changes, updates the projection tree, and uses `matchViewportBox` in `postRender` callbacks to assert the final layout state.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/animate-relative-parent-delayed.html#_snippet_1

LANGUAGE: javascript
CODE:
```
const { createNode } = window.Animate
const { matchViewportBox } = window.Assert
const { frame } = window.Projection
const parent = document.getElementById("parent")
const child = document.getElementById("child")
const parentOrigin = parent.getBoundingClientRect()
const parentProjection = createNode(
 parent,
 undefined,
 {},
 { delay: 1000 }
)
const childProjection = createNode(child, parentProjection)
parentProjection.willUpdate()
childProjection.willUpdate()
parent.classList.add("b")
parentProjection.root.didUpdate()
frame.postRender(() => {
 frame.postRender(() => {
 matchViewportBox(parent, parentOrigin)
 matchViewportBox(child, {
 bottom: 85,
 height: 75,
 left: 60,
 right: 135,
 top: 10,
 width: 75,
 x: 60,
 })
 })
})
```

----------------------------------------

TITLE: Initializing Framer Motion Projection Nodes (JavaScript)
DESCRIPTION: Initializes Framer Motion projection nodes for an overlay and a box element, simulates scrolling by a specific offset, updates the projection tree, and asserts the box's position relative to the viewport after the update.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/single-element-page-scroll-overlay.html#_snippet_1

LANGUAGE: javascript
CODE:
```
const { createNode } = window.Undo
const { matchViewportBox, addPageScroll } = window.Assert
const { frame } = window.Projection
const overlay = document.getElementById("overlay")
const overlayProjection = createNode(overlay, undefined, { layoutScroll: true, layout: false, })
const box = document.getElementById("box")
const boxProjection = createNode(box, overlayProjection)
const boxOrigin = box.getBoundingClientRect()
boxProjection.willUpdate()
const scrollOffset = [50, 100]
window.scrollTo(...scrollOffset)
boxProjection.root.didUpdate()
matchViewportBox(box, { top: 0, left: 0, bottom: 100, right: 100 })
```

----------------------------------------

TITLE: Testing Element Projection and Layout Updates with JavaScript
DESCRIPTION: Uses a library (likely Framer Motion's internal projection system) to create projection nodes for a container and a box. It applies skew transformations to the container and then triggers layout updates and post-render checks to verify the box's position and the container's skew after a class change.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/single-element-layout-change-skew-container.html#_snippet_1

LANGUAGE: javascript
CODE:
```
const { createNode } = window.Undo
const { matchViewportBox, matchSkewX } = window.Assert
const { frame } = window.Projection

const container = document.getElementById("container")
const containerProjection = createNode(container)
const box = document.getElementById("box")
const boxProjection = createNode(box, containerProjection)

containerProjection.setValue("skewX", 20)
containerProjection.setValue("skewY", -20)

requestAnimationFrame(() => {
  const boxOrigin = box.getBoundingClientRect()
  boxProjection.willUpdate()
  containerProjection.willUpdate()
  container.classList.add("b")

  requestAnimationFrame(() => {
    containerProjection.root.didUpdate()
    frame.postRender(() => {
      matchViewportBox(box, boxOrigin)
      matchSkewX(container, 20)
    })
  })
})
```

----------------------------------------

TITLE: Manipulating Box Element with JavaScript and Framer Motion Utilities
DESCRIPTION: Uses utilities from `window.Undo`, `window.Assert`, and `window.Projection` to create a projection node for a box element, set its scale, match its viewport box, update its state, add a CSS class, and perform subsequent updates and viewport matching within `frame.postRender` callbacks.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/transform-single-layout-change-with-scale.html#_snippet_1

LANGUAGE: javascript
CODE:
```
const { createNode } = window.Undo const { matchViewportBox } = window.Assert const { frame } = window.Projection const box = document.getElementById("box") const boxProjection = createNode(box) const boxOrigin = box.getBoundingClientRect() boxProjection.setValue("scale", 2) frame.postRender(() => { const transformedBox = { top: -50, left: -50, right: 150, bottom: 150, } matchViewportBox(box, transformedBox) boxProjection.willUpdate() box.classList.add("b") boxProjection.root.didUpdate() frame.postRender(() => { matchViewportBox(box, transformedBox) }) })
```

----------------------------------------

TITLE: Animating Element Layout with Projection (JavaScript)
DESCRIPTION: Uses window.Undo, window.Assert, and window.Projection utilities to create projection nodes for #box and #child elements. It applies a rotation value to the box projection, triggers a layout update by adding a class, and then asserts that the viewport box positions match their original positions after the update and rendering.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/single-element-layout-change-with-child-rotate.html#_snippet_1

LANGUAGE: javascript
CODE:
```
const { createNode } = window.Undo; const { matchViewportBox } = window.Assert; const { frame } = window.Projection; const box = document.getElementById("box"); const boxProjection = createNode(box); const child = document.getElementById("child"); const childProjection = createNode(child, boxProjection); boxProjection.setValue("rotate", 45); requestAnimationFrame(() => { const boxOrigin = box.getBoundingClientRect(); const childOrigin = child.getBoundingClientRect(); boxProjection.willUpdate(); childProjection.willUpdate(); box.classList.add("b"); requestAnimationFrame(() => { boxProjection.root.didUpdate(); frame.postRender(() => { matchViewportBox(box, boxOrigin); matchViewportBox(child, childOrigin); }); }); });
```

----------------------------------------

TITLE: Testing Layout Projection with Framer Motion - JavaScript
DESCRIPTION: Uses a library (likely Framer Motion) to create projection nodes for elements, manipulate their values, and test layout consistency after DOM changes by appending a new child element.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/shared-transform-parents.html#_snippet_1

LANGUAGE: javascript
CODE:
```
const { createNode } = window.Undo
const { matchViewportBox, matchVisibility, matchOpacity } = window.Assert
const { frame } = window.Projection

const a = document.getElementById("box-a")
const b = document.getElementById("box-b")
const child = document.querySelector(".child")

const aProjection = createNode(a)
const bProjection = createNode(b)
const childProjection = createNode(child, aProjection, {
  layoutId: "child",
})

aProjection.setValue("x", 100)
bProjection.setValue("x", -100)

frame.postRender(() => {
  aProjection.willUpdate()
  bProjection.willUpdate()
  childProjection.willUpdate()

  const childOrigin = child.getBoundingClientRect()

  const newChild = document.createElement("div")
  newChild.classList.add("child")
  b.appendChild(newChild)

  const newChildProjection = createNode(newChild, bProjection, {
    layoutId: "child",
  })

  newChildProjection.root.didUpdate()

  frame.postRender(() => {
    const newChildOrigin = newChild.getBoundingClientRect()
    matchViewportBox(child, childOrigin)
    matchViewportBox(newChild, childOrigin)
    matchOpacity(child, 1)
    matchOpacity(newChild, 0)
  })
})
```

----------------------------------------

TITLE: Basic Layout and Grid CSS
DESCRIPTION: Defines basic body/container styles, a flex layout, a grid layout variation (.as-grid), styles for elements 'a' and 'b' within the grid, a hidden element to trigger overflow, and a style for incorrect layout state. It sets up the visual structure and potential layout changes.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/flexbox-siblings-to-grid-page-scroll.html#_snippet_0

LANGUAGE: CSS
CODE:
```
body { padding: 0; margin: 0; }\n#container { display: flex; position: relative; top: 100px; left: 100px; width: 300px; height: 200px; }\n#container.as-grid { display: grid; grid-template-columns: 50px auto; }\n#a { background: #00cc88; grid-column: 2/3; }\n#b { background: #0077ff; grid-column: 1/2; }\n#container > div { height: 200px; flex: 1; }\n#trigger-overflow { width: 1px; height: 1px; position: absolute; top: 2000px; left: 2000px; }\n\[data-layout-correct="false"\] { background: #dd1144 !important; opacity: 0.5; }
```

----------------------------------------

TITLE: Initializing Projection Nodes (JavaScript)
DESCRIPTION: Initializes projection nodes for parent and child elements using custom libraries (Animate, Assert, Projection) and triggers layout updates. Includes commented-out code for viewport box matching assertions.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/animate-relative.html#_snippet_1

LANGUAGE: javascript
CODE:
```
const { createNode, relativeEase } = window.Animate
const { matchViewportBox } = window.Assert
const { frame } = window.Projection

const parent = document.getElementById("parent")
const child = document.getElementById("child")

const parentProjection = createNode(
  parent,
  undefined,
  {},
  { ease: relativeEase() }
)

const childProjection = createNode(
  child,
  parentProjection,
  {},
  { delay: 1000, ease: relativeEase() }
)

parentProjection.willUpdate()
childProjection.willUpdate()

parent.classList.add("b")

parentProjection.root.didUpdate()

// frame.postRender(() => {
//   frame.postRender(() => {
//     matchViewportBox(parent, {
//       bottom: 170,
//       left: 100,
//       right: 270,
//       top: 50,
//     })
//     matchViewportBox(child, {
//       bottom: 100,
//       left: 100,
//       right: 150,
//       top: 50,
//     })
//   })
// })
```

----------------------------------------

TITLE: Testing Box Layout with Projection (JavaScript)
DESCRIPTION: Uses window objects (Undo, Assert, Projection) to create a projection node for a box element. It sets an initial x-value, then uses frame.postRender to apply a class (b) to the box, update the projection, and assert its viewport box matches a predefined transformed state. A second postRender call re-asserts the state.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/transform-single-layout-change-with-translate.html#_snippet_1

LANGUAGE: JavaScript
CODE:
```
const { createNode } = window.Undo
const { matchViewportBox } = window.Assert
const { frame } = window.Projection
const box = document.getElementById("box")
const boxProjection = createNode(box)
const boxOrigin = box.getBoundingClientRect()
boxProjection.setValue("x", 100)
frame.postRender(() => {
  const transformedBox = { top: 0, left: 100, right: 200, bottom: 100, }
  matchViewportBox(box, transformedBox)
  boxProjection.willUpdate()
  box.classList.add("b")
  boxProjection.root.didUpdate()
  frame.postRender(() => {
    matchViewportBox(box, transformedBox)
  })
})
```

----------------------------------------

TITLE: Animating and Asserting Layout with JavaScript
DESCRIPTION: Uses custom libraries (`Animate`, `Assert`, `Projection`) to create layout nodes/projections for elements (#a, #scroller). It manipulates the DOM by adding a new element (#b) to #scroller, creates a projection for it, updates the layout, and then performs layout assertions using `matchViewportBox`.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/shared-transform-parents-animate-2.html#_snippet_1

LANGUAGE: javascript
CODE:
```
const { createNode } = window.Animate
const { matchViewportBox, matchVisibility, matchOpacity } = window.Assert
const { frame } = window.Projection
const a = document.getElementById("a")
const scroller = document.getElementById("scroller")
const aProjection = createNode(a, undefined, { layoutId: "a" })
const scrollerProjection = createNode(scroller)
scrollerProjection.setValue("x", -200)
frame.postRender(() => {
  aProjection.willUpdate()
  const b = document.createElement("div")
  b.id = "b"
  scroller.appendChild(b)
  const bProjection = createNode(b, scrollerProjection, { layoutId: "a", })
  aProjection.root.didUpdate()
  frame.postRender(() => {
    matchViewportBox(a, b.getBoundingClientRect())
    matchViewportBox(a, { bottom: 350, height: 250, left: 55, right: 305, top: 100, width: 250, x: 55, y: 100, })
  })
})
```

----------------------------------------

TITLE: Manipulating Element Projection and Asserting Position (JavaScript)
DESCRIPTION: This JavaScript snippet demonstrates how to select a DOM element, interact with custom libraries (Undo, Assert, Projection) likely related to layout or animation, and modify the element's class to change its styling (from sticky to fixed). It captures the element's initial position, creates a projection node, updates the projection after the class change, and finally uses setTimeout to assert the element's position against its original bounding box after the layout change has potentially settled.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/sticky-to-fixed.html#_snippet_1

LANGUAGE: javascript
CODE:
```
const { createNode } = window.Undo
const { matchViewportBox, matchVisibility, matchOpacity, addPageScroll,
} = window.Assert
const { frame } = window.Projection
const sticky = document.querySelector(".sticky")
const stickyProjection = createNode(sticky, undefined)
const stickyOrigin = sticky.getBoundingClientRect()
stickyProjection.willUpdate()
sticky.classList.add("b")
stickyProjection.root.didUpdate()
setTimeout(() => {
 matchViewportBox(sticky, stickyOrigin)
}, 50)
```

----------------------------------------

TITLE: JavaScript Layout Assertion with Undo/Assert
DESCRIPTION: Uses custom window objects (Undo and Assert) to create a projected node, capture its initial position, simulate page scrolling, update the node's projection, and assert its final position relative to the viewport after the scroll.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/single-element-page-scroll.html#_snippet_1

LANGUAGE: javascript
CODE:
```
const { createNode } = window.Undo const { matchViewportBox, addPageScroll } = window.Assert const box = document.getElementById("box") const boxProjection = createNode(box) const boxOrigin = box.getBoundingClientRect() boxProjection.willUpdate() const scrollOffset = \[50, 100\] window.scrollTo(...scrollOffset) boxProjection.root.didUpdate() matchViewportBox(box, addPageScroll(boxOrigin, ...scrollOffset))
```

----------------------------------------

TITLE: Starting Manual Rotation Animation (JavaScript)
DESCRIPTION: Initializes the start time and begins the manual rotation animation loop by calling `requestAnimationFrame`. This function is commented out in the provided code.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/benchmarks/pregenerated-background-color.html#_snippet_3

LANGUAGE: javascript
CODE:
```
function animateRotation() {
  startTime = performance.now()
  requestAnimationFrame(rotate)
}
// animateRotation()
```

----------------------------------------

TITLE: Styling Body and Box Elements (CSS)
DESCRIPTION: Defines basic CSS styles for the body to add padding and remove margin, styles a box element with dimensions and background color, and adds a rule to highlight elements where layout correction is explicitly disabled.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/optimized-appear/defer-handoff-layout-uselayouteffect.html#_snippet_0

LANGUAGE: css
CODE:
```
body { padding: 100px; margin: 0; } #box { width: 100px; height: 100px; background-color: #0077ff; } \[data-layout-correct="false"\] { background: #dd1144 !important; opacity: 1 !important; }
```

----------------------------------------

TITLE: Styling Basic Layout Elements - CSS
DESCRIPTION: Defines fundamental styles for the page body, a flexible container, and child box elements, setting dimensions, layout properties, and background colors. This provides a basic visual structure for the page.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/benchmarks/mix-complex-value-framer-motion.html#_snippet_0

LANGUAGE: CSS
CODE:
```
body { padding: 0; margin: 0; }
.container { padding: 100px; width: 100%; display: flex; flex-wrap: wrap; }
.container > div { width: 100px; height: 100px; }
.box { width: 10px; height: 100px; background-color: #fff; }
```

----------------------------------------

TITLE: Styling Elements with CSS
DESCRIPTION: Defines basic styles for body, position, and sticky elements, including sticky and fixed positioning, dimensions, background, and opacity. It also includes a rule for elements with a specific data attribute.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/sticky-to-fixed-page-scroll.skip.html#_snippet_0

LANGUAGE: css
CODE:
```
body { padding: 0; margin: 0; } #position { width: 1px; height: 200px; } .sticky { position: sticky; top: 100px; width: 100%; height: 200px; background: #0088ff; } .sticky.b { position: fixed; top: 0; left: 0; width: 300px; height: 300px; } #trigger-overflow { width: 1px; height: 1px; position: absolute; top: 2000px; left: 2000px; } \[data-layout-correct="false"\] { background: #dd1144 !important; opacity: 0.5; }
```

----------------------------------------

TITLE: Styling Basic Elements with CSS
DESCRIPTION: Defines basic styles for body, box, size, overlay, and a trigger element, including positioning and background colors. Also includes a style for elements with a specific data attribute to indicate layout correctness.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/sticky-child.html#_snippet_0

LANGUAGE: css
CODE:
```
body { padding: 0; margin: 0; }
#box { width: 100px; height: 100px; background-color: #00cc88; }
#size { height: 100px; }
#overlay { background-color: black; position: sticky; top: 0px; height: 200px; width: 200px; }
#trigger-overflow { width: 1px; height: 1px; position: absolute; top: 2000px; left: 2000px; }
[data-layout-correct="false"] { background: #dd1144 !important; opacity: 0.5; }
```

----------------------------------------

TITLE: Styling Layout Test Elements (CSS)
DESCRIPTION: Defines basic styles for the body, container, fixed, and child elements used in the layout projection test. Includes dimensions, positioning, overflow properties, background colors, and a specific style for indicating incorrect layout.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/fixed-within-element-scroll.html#_snippet_0

LANGUAGE: CSS
CODE:
```
body { padding: 0; margin: 0; }
#container { width: 100px; height: 100px; overflow: scroll; }
#content { width: 400px; height: 100px; }
#fixed { position: fixed; top: 0; left: 0; width: 500px; height: 100px; background: #00cc88; display: flex; align-items: flex-start; }
#child { width: 100px; height: 100px; background: #0077ff; }
#trigger-overflow { width: 1px; height: 1px; position: absolute; top: 2000px; left: 2000px; }
\[data-layout-correct="false"\] { background: #dd1144 !important; opacity: 0.5; }
```

----------------------------------------

TITLE: Styling Nested Elements for Layout Test (CSS)
DESCRIPTION: Defines basic box model styling for nested divs (#parent, #mid, #child, #grandchild) and a class (.b) that modifies the parent's position and the grandchild's relative position. Includes a rule to visually indicate incorrect layout results.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/animate-relative-nested-deep.html#_snippet_0

LANGUAGE: css
CODE:
```
body { padding: 0; margin: 0; } #parent { position: relative; width: 200px; height: 200px; background-color: #00cc88; display: flex; align-items: flex-start; justify-content: flex-start; } #mid { width: 100px; height: 100px; background-color: white; display: flex; align-items: flex-start; justify-content: flex-start; } #parent.b { top: 100px; left: 100px; } #child { width: 80px; height: 80px; background-color: #0077ff; } #grandchild { width: 20px; height: 20px; background-color: black; position: relative; } .b #grandchild { top: 1px; left: 1px; } \[data-layout-correct="false"\] { background: #dd1144 !important; opacity: 0.5; }
```

----------------------------------------

TITLE: Styling Elements for Animation Testing (CSS)
DESCRIPTION: Defines basic styles for the body, a container, and box elements used in the animation tests, including a style to highlight incorrect layout states.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/optimized-appear/defer-handoff-layout-ancestor.html#_snippet_0

LANGUAGE: CSS
CODE:
```
body { padding: 100px; margin: 0; } #container { display: flex; flex-direction: column; } .box { width: 100px; height: 100px; background-color: #0077ff; } \[data-layout-correct="false"\] { background: #dd1144 !important; opacity: 1 !important; }
```

----------------------------------------

TITLE: Styling Elements for Layout Testing (CSS)
DESCRIPTION: Defines basic styles for the body, a main box element, a child element, and a trigger element. Includes a class '.b' to modify the box's layout and a style for elements with 'data-layout-correct' attribute for visual feedback.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/single-element-block-update.html#_snippet_0

LANGUAGE: css
CODE:
```
body { padding: 0; margin: 0; } #box { width: 100px; height: 100px; background-color: #00cc88; } #child { width: 50px; height: 50px; background-color: #0077ff; } #box.b { width: 200px; position: absolute; top: 100px; left: 200px; padding: 20px; } #trigger-overflow { width: 1px; height: 1px; position: absolute; top: 2000px; left: 2000px; } \[data-layout-correct="false"\] { background: #dd1144 !important; opacity: 0.5; }
```

----------------------------------------

TITLE: Styling Layout Elements CSS
DESCRIPTION: Provides basic CSS styles for the body, a scrollable container, an inner container, and two box elements used in the layout projection test. Includes positioning, dimensions, and background colors.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/element-scroll-remove.html#_snippet_0

LANGUAGE: CSS
CODE:
```
body { padding: 0; margin: 0; } #scroll { overflow: scroll; position: relative; height: 500px; width: 500px; } #container { position: relative; } #box-1, #box-2 { position: absolute; top: 100px; width: 100px; height: 100px; background: #00cc88; } .trigger-overflow { width: 1px; height: 1px; position: absolute; top: 2000px; left: 2000px; } [data-layout-correct="false"] { background: #dd1144 !important; opacity: 0.5; }
```

----------------------------------------

TITLE: Styling Elements for Layout Transitions (CSS)
DESCRIPTION: Provides basic CSS rules for elements used in layout transition tests, including dimensions, background colors, positioning, and a rule to highlight elements potentially miscalculated by the layout system.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/perf-shared-deep.html#_snippet_0

LANGUAGE: CSS
CODE:
```
body { padding: 0; margin: 0; } #a { width: 100px; height: 100px; background-color: #00cc88; } #b { width: 200px; height: 200px; background-color: #0077ff; position: absolute; top: 50px; left: 50px; } #a-2, #b-2 { width: 50px; height: 50px; background-color: #fff; } #a-3, #b-3 { width: 25px; height: 25px; background-color: #000; } #trigger-overflow { width: 1px; height: 1px; position: absolute; top: 2000px; left: 2000px; } \[data-layout-correct="false"\] { background: #dd1144 !important; opacity: 0.5; }
```

----------------------------------------

TITLE: Styling Layout Elements with CSS
DESCRIPTION: Defines basic CSS styles for body, two main boxes (#box-a, #box-b), a child element (.child), and a trigger element (#trigger-overflow). Includes styles for highlighting incorrect layout states.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/shared-transform-parents-animate.html#_snippet_0

LANGUAGE: css
CODE:
```
body { padding: 0; margin: 0; } #box-a { width: 200px; height: 200px; position: absolute; left: 100px; top: 100px; background-color: #00cc88; } #box-b { position: absolute; top: 100px; left: 600px; width: 200px; height: 200px; background-color: #09f; } .child { position: relative; top: 20px; left: 20px; width: 100px; height: 100px; background-color: #ffcc00; } #trigger-overflow { width: 1px; height: 1px; position: absolute; top: 2000px; left: 2000px; } \[data-layout-correct="false"\] { background: #dd1144 !important; opacity: 0.5; }
```

----------------------------------------

TITLE: Styling for SSR Animation Test (CSS)
DESCRIPTION: Provides basic styling for the test page, including padding, margin, box dimensions, background color, and a specific style for elements marked with `data-layout-correct="false"` to indicate layout issues.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/optimized-appear/resync.html#_snippet_0

LANGUAGE: css
CODE:
```
body { padding: 100px; margin: 0; } #box { width: 100px; height: 100px; background-color: #0077ff; } [data-layout-correct="false"] { background: #dd1144 !important; opacity: 1 !important; }
```

----------------------------------------

TITLE: Styling Layout Elements with CSS
DESCRIPTION: Defines basic body styles, dimensions for position testing, and styles for sticky and fixed positioned elements, including background color and size. Also includes a rule for an overflow trigger and a rule to highlight incorrect layout states.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/sticky-shared-to-fixed-page-scroll-no-stick.html#_snippet_0

LANGUAGE: css
CODE:
```
body { padding: 0; margin: 0; } #position { width: 1px; height: 200px; } .sticky { position: sticky; top: 100px; width: 100%; height: 200px; background: #0088ff; } .fixed { position: fixed; top: 0; left: 0; width: 300px; height: 300px; background: #0088ff; } #trigger-overflow { width: 1px; height: 1px; position: absolute; top: 2000px; left: 2000px; } \[data-layout-correct="false"\] { background: #dd1144 !important; opacity: 0.5; }
```

----------------------------------------

TITLE: Styling Elements for Layout Testing (CSS)
DESCRIPTION: Defines basic styles for the body, a box element (#box) with a variant (.b), a small overflow trigger element, and a style for elements marked as layout incorrect.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/transform-single-with-scale.html#_snippet_0

LANGUAGE: css
CODE:
```
body { padding: 0; margin: 0; }
#box { width: 100px; height: 100px; background-color: #00cc88; }
#box.b { width: 200px; position: absolute; top: 100px; left: 200px; padding: 20px; }
#trigger-overflow { width: 1px; height: 1px; position: absolute; top: 2000px; left: 2000px; }
\[data-layout-correct="false"\] { background: #dd1144 !important; opacity: 0.5; }
```

----------------------------------------

TITLE: Styling Layout Elements with CSS
DESCRIPTION: Defines basic styling for body, parent, mid, and child elements, including positioning, dimensions, background colors, and a state-dependent style for the mid element. Also includes a style for elements marked as layout incorrect.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/animate-relative-instant.html#_snippet_0

LANGUAGE: css
CODE:
```
body { padding: 0; margin: 0; }
#parent { position: relative; width: 200px; height: 200px; background-color: #00cc88; }
#mid { position: absolute; width: auto; height: auto; left: 0; top: 0; }
.b #mid { left: 100px; }
#child { width: 50px; height: 50px; background-color: #0077ff; }
[data-layout-correct="false"] { background: #dd1144 !important; opacity: 0.5; }
```

----------------------------------------

TITLE: Styling Elements for Layout Test (CSS)
DESCRIPTION: Defines basic styles for the body and a box element, including initial state and a modified state (.b class) used for testing layout changes and positioning. Includes a hidden element to trigger overflow and a style for indicating layout errors.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/single-element-layout-change-page-scroll.html#_snippet_0

LANGUAGE: css
CODE:
```
body { padding: 0; margin: 0; }
#box { width: 100px; height: 100px; background-color: #00cc88; }
#box.b { width: 200px; position: absolute; top: 100px; left: 200px; padding: 20px; }
#trigger-overflow { width: 1px; height: 1px; position: absolute; top: 2000px; left: 2000px; }
\[data-layout-correct="false"\] { background: #dd1144 !important; opacity: 0.5; }
```

----------------------------------------

TITLE: Styling Basic Elements and Box with CSS
DESCRIPTION: Defines basic body styles, initial styles for an element with id "box", modified styles for "box" when it has class "b", styles for a hidden overflow trigger element, and styles for elements with a specific data attribute.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/single-element-layout-change-skew-change.html#_snippet_0

LANGUAGE: CSS
CODE:
```
body { padding: 0; margin: 0; } #box { width: 100px; height: 100px; background-color: #00cc88; } #box.b { width: 200px; position: absolute; top: 100px; left: 200px; padding: 20px; } #trigger-overflow { width: 1px; height: 1px; position: absolute; top: 2000px; left: 2000px; } [data-layout-correct="false"] { background: #dd1144 !important; opacity: 0.5; }
```

----------------------------------------

TITLE: Styling Elements with CSS
DESCRIPTION: Defines basic CSS styles for the page body, a specific box element (#box), and a data attribute used to indicate layout correction status.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/optimized-appear/start-after-hydration.html#_snippet_0

LANGUAGE: css
CODE:
```
body { padding: 100px; margin: 0; } #box { width: 100px; height: 100px; background-color: #0077ff; } \[data-layout-correct="false"\] { background: #dd1144 !important; opacity: 1 !important; }
```

----------------------------------------

TITLE: Styling Elements with CSS Classes
DESCRIPTION: Defines basic styles for body, #box, and #child elements, including dimensions, background colors, and positioning. It also includes styles for class variations (.b, .c) and a hidden element (#trigger-overflow), plus a data attribute selector for layout correctness checks.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/nested-layout-change-mid-projection.html#_snippet_0

LANGUAGE: css
CODE:
```
body { padding: 0; margin: 0; } #box { width: 100px; height: 100px; background-color: #00cc88; } #child { width: 50px; height: 50px; background-color: #0077ff; } #box.b { width: 200px; position: absolute; top: 100px; left: 200px; padding: 20px; } #child.b { width: 100px; position: absolute; top: 10px; left: 10px; padding: 10px; } #child.c { left: 50px; width: 25px; } #trigger-overflow { width: 1px; height: 1px; position: absolute; top: 2000px; left: 2000px; } [data-layout-correct="false"] { background: #dd1144 !important; opacity: 0.5; }
```

----------------------------------------

TITLE: Styling Elements with CSS
DESCRIPTION: Defines basic styles for the body, a main box element, and a child element. Includes specific styles applied when the box has the class 'b' and a hidden element to potentially test overflow behavior. Also includes a style for elements with a 'data-layout-correct' attribute set to false.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/single-element-layout-change-with-child-rotate-animate.html#_snippet_0

LANGUAGE: css
CODE:
```
body { padding: 0; margin: 0; } #box { position: absolute; top: 100px; left: 300px; width: 200px; height: 200px; background-color: #00cc88; } #child { width: 50px; height: 50px; background-color: #0077ff; } #box.b { top: 200px; } #box.b #child { position: absolute; top: 150px; } #trigger-overflow { width: 1px; height: 1px; position: absolute; top: 2000px; left: 2000px; } [data-layout-correct="false"] { background: #dd1144 !important; opacity: 0.5; }
```

----------------------------------------

TITLE: Styling Layout Containers and Elements (CSS)
DESCRIPTION: Defines basic styles for the body, a container element (#container), a box element (#box), and a hidden trigger element (#trigger-overflow). Includes a class 'b' to modify container alignment and a style for elements with data-layout-correct="false".
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/single-element-layout-change-rotate-container.html#_snippet_0

LANGUAGE: CSS
CODE:
```
body { padding: 0; margin: 0; } #container { width: 100px; height: 200px; display: flex; align-items: flex-end; } #container.b { align-items: flex-start; } #box { width: 100px; height: 100px; background-color: #00cc88; } #trigger-overflow { width: 1px; height: 1px; position: absolute; top: 2000px; left: 2000px; } \[data-layout-correct="false"\] { background: #dd1144 !important; opacity: 0.5; }
```

----------------------------------------

TITLE: Testing Framer Motion Layout Projection with Scroll and Element Swapping JavaScript
DESCRIPTION: JavaScript code that sets up Framer Motion projections for elements within a scrollable container. It tests layout correctness by changing scroll position, unmounting and mounting different elements with the same layoutId, and using assertion functions like `matchViewportBox`.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/element-scroll-remove.html#_snippet_1

LANGUAGE: JavaScript
CODE:
```
const { createNode } = window.Undo
const { matchViewportBox, matchOpacity, matchBorderRadius, addPageScroll, } = window.Assert
const { frame } = window.Projection
const scroll = document.getElementById("scroll")
const container = document.getElementById("container")
const box = document.getElementById("box-1")
const scrollProjection = createNode(scroll, undefined, { layoutId: "scroll", layoutScroll: true, })
const containerProjection = createNode(
 container,
 scrollProjection,
 { layoutId: "container" }
)
const boxProjection = createNode(box, containerProjection, { layoutId: "a", })
scroll.scrollTop = 100
// unmount box-1, mount box-2
boxProjection.willUpdate()
boxProjection.unmount()
container.removeChild(box)
const box2 = document.createElement("div")
box2.id = "box-2"
container.appendChild(box2)
const box2Projection = createNode(box2, containerProjection, { layoutId: "b", })
box2Projection.root.didUpdate()
matchViewportBox(box2, { top: 0, bottom: 100, left: 0, right: 100 })
// update the scroll while box-1 is unmounted
scroll.scrollTop = 50
// unmount box-2, mount box-1
box2Projection.willUpdate()
box2Projection.unmount()
container.removeChild(box2)
const box1 = document.createElement("div")
box1.id = "box-1"
container.appendChild(box1)
const box1Projection = createNode(box1, containerProjection, { layoutId: "a", })
box1Projection.root.didUpdate()
matchViewportBox(box1, { top: 50, bottom: 150, left: 0, right: 100, })
```

----------------------------------------

TITLE: Testing Framer Motion Projection Layout - JavaScript
DESCRIPTION: Uses Framer Motion's Animate, Assert, and Projection APIs to create projection nodes for DOM elements. It sets up a layout test, modifies a projection value (borderRadius), and uses frame.postRender to assert viewport box positions of the elements.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/shared-promote-new-mix-interrupt.html#_snippet_1

LANGUAGE: javascript
CODE:
```
const { createNode } = window.Animate
const { matchViewportBox, matchVisibility, matchOpacity, matchBorderRadius, } = window.Assert
const { frame } = window.Projection
const box = document.getElementById("box-a")
const boxProjection = createNode(box, undefined, { layoutId: "a", crossfade: true, })
boxProjection.willUpdate()
const newBox = document.createElement("div")
newBox.id = "box-b"
document.body.appendChild(newBox)
const newBoxProjection = createNode(newBox, undefined, { layoutId: "a", crossfade: true, })
newBoxProjection.setValue("borderRadius", 20)
newBoxProjection.root.didUpdate()
let midBox = {
 bottom: 250,
 left: 50,
 right: 200,
 top: 50
}
frame.postRender(() => {
 matchViewportBox(box, midBox)
 matchViewportBox(newBox, midBox)
})
// matchVisibility(box, "visible")
// matchVisibility(newBox, "visible")
// matchOpacity(box, 1)
// matchOpacity(newBox, 1)
// matchBorderRadius(box, "6.66667% / 5%")
// matchBorderRadius(newBox, "6.66667% / 5%")
// newBoxProjection.willUpdate()
// boxProjection.promote()
// newBoxProjection.root.didUpdate()
// midBox = { // bottom: 175, // left: 25, // right: 150, // top: 25, // }
// matchViewportBox(box, midBox)
// matchViewportBox(newBox, midBox)
// matchVisibility(box, "visible")
// matchVisibility(newBox, "visible")
// matchOpacity(box, 1)
// matchOpacity(newBox, 1)
// matchBorderRadius(box, "4% / 3.33333%")
// matchBorderRadius(newBox, "4% / 3.33333%")
```

----------------------------------------

TITLE: Importing Framer Motion and Assert Utilities
DESCRIPTION: Imports necessary functions and objects from global Motion and Assert objects, and initializes variables used for animation control and element references.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/optimized-appear/defer-handoff-block.html#_snippet_1

LANGUAGE: javascript
CODE:
```
const { motion, animateStyle, animate, startOptimizedAppearAnimation, optimizedAppearDataAttribute, motionValue, frame, } = window.Motion
const { matchViewportBox } = window.Assert
const root = document.getElementById("root")
const duration = 4
const x = motionValue(0)
const xTarget = 500
let isFirstFrame = true
```

----------------------------------------

TITLE: Manipulating DOM and Projection with JavaScript
DESCRIPTION: Uses global objects `Undo`, `Assert`, and `Projection` to interact with a sticky element. It creates a projection node, updates its state, scrolls the window, modifies the element's class, and asserts its viewport position after a delay.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/sticky-to-fixed-page-scroll.skip.html#_snippet_1

LANGUAGE: javascript
CODE:
```
const { createNode } = window.Undo
const { matchViewportBox, matchVisibility, matchOpacity, addPageScroll,
} = window.Assert
const { frame } = window.Projection
const sticky = document.querySelector(".sticky")
const stickyProjection = createNode(sticky, undefined)
stickyProjection.willUpdate()
const scrollOffset = \[50, 150\]
window.scrollTo(...scrollOffset)
const stickyOrigin = sticky.getBoundingClientRect()
sticky.classList.add("b")
stickyProjection.root.didUpdate()
setTimeout(() => {
  matchViewportBox(sticky, stickyOrigin)
}, 50)
```

----------------------------------------

TITLE: Creating and Asserting Element Projections in JavaScript
DESCRIPTION: This JavaScript snippet initializes variables from custom libraries (Undo, Assert, Projection). It retrieves DOM elements for an overlay and a box, then creates 'projection' nodes for them using createNode, establishing a parent-child relationship. It simulates scrolling the window and then uses matchViewportBox to assert that the box element is positioned within a specific bounding box relative to the viewport after the layout updates.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/sticky-child-scroll-change.html#_snippet_1

LANGUAGE: javascript
CODE:
```
const { createNode } = window.Undo
const { matchViewportBox, addPageScroll } = window.Assert
const { frame } = window.Projection
const overlay = document.getElementById("overlay")
const overlayProjection = createNode(overlay, undefined, { layoutRoot: true, layout: true, })
const box = document.getElementById("box")
const boxProjection = createNode(box, overlayProjection)
boxProjection.willUpdate()
const scrollOffset = [50, 150]
window.scrollTo(...scrollOffset)
boxProjection.root.didUpdate()
matchViewportBox(box, { top: 0, left: -50, bottom: 100, right: 50 })
```

----------------------------------------

TITLE: Run Framer Motion Vanilla Dev Environment (Shell)
DESCRIPTION: Execute this command from the root directory of the motion project to start the development server for the Vanilla environment. This allows access to various tests and benchmark files.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/README.md#_snippet_0

LANGUAGE: Shell
CODE:
```
yarn dev
```

----------------------------------------

TITLE: Basic CSS Layout Styling
DESCRIPTION: Defines fundamental styles for the body, a container using flexbox, and specific dimensions for child elements and a 'box' class.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/benchmarks/mix-complex-value-greensock.html#_snippet_0

LANGUAGE: css
CODE:
```
body { padding: 0; margin: 0; } .container { padding: 100px; width: 100%; display: flex; flex-wrap: wrap; } .container > div { width: 100px; height: 100px; } .box { width: 10px; height: 100px; background-color: #fff; }
```

----------------------------------------

TITLE: Styling Elements with CSS
DESCRIPTION: This CSS snippet defines styles for various HTML elements, including basic body reset, dimensions for a position marker, sticky and fixed positioning for elements with the class 'sticky', an off-screen trigger element, and visual indicators for layout correctness issues. It uses properties like position, top, left, width, height, background, opacity, margin, and padding.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/sticky-to-fixed.html#_snippet_0

LANGUAGE: css
CODE:
```
body { padding: 0; margin: 0; }
#position { width: 1px; height: 200px; }
.sticky { position: sticky; top: 100px; width: 100%; height: 200px; background: #0088ff; }
.sticky.b { position: fixed; top: 0; left: 0; width: 300px; height: 300px; }
#trigger-overflow { width: 1px; height: 1px; position: absolute; top: 2000px; left: 2000px; }
\[data-layout-correct="false"\] { background: #dd1144 !important; opacity: 0.5; }
```

----------------------------------------

TITLE: Styling Basic Elements with CSS
DESCRIPTION: Defines basic styles for the body, a box element (`#box`), and a specific data attribute (`[data-layout-correct="false"]`) used for layout correction indicators. Sets padding, margin, dimensions, background colors, and opacity.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/optimized-appear/interrupt-tween-opacity.html#_snippet_0

LANGUAGE: css
CODE:
```
body { padding: 100px; margin: 0; } #box { width: 100px; height: 100px; background-color: #0077ff; } \[data-layout-correct=\"false\"\] { background: #dd1144 !important; opacity: 1 !important; }
```

----------------------------------------

TITLE: Styling for Test Elements (CSS)
DESCRIPTION: Defines basic styles for the body and a box element used in the test, including a specific style to highlight elements with incorrect layout data.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/optimized-appear/portal.html#_snippet_0

LANGUAGE: CSS
CODE:
```
body { padding: 100px; margin: 0; } #box { width: 100px; height: 100px; background-color: #0077ff; } \[data-layout-correct="false"\] { background: #dd1144 !important; opacity: 1 !important; }
```

----------------------------------------

TITLE: Styling for Framer Motion Box and Layout Correction
DESCRIPTION: Provides basic CSS styling for the body, the animated box element (#box), and a specific rule to visually indicate elements where layout correction might be disabled or incorrect.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/optimized-appear/interrupt-tween-transforms.html#_snippet_0

LANGUAGE: css
CODE:
```
body { padding: 100px; margin: 0; } #box { width: 100px; height: 100px; background-color: #0077ff; } \[data-layout-correct="false"\] { background: #dd1144 !important; opacity: 1 !important; }
```

----------------------------------------

TITLE: Styling for Framer Motion Box and Layout Correction (CSS)
DESCRIPTION: Defines basic CSS styles for the page body, the animated box element, and a specific style rule to highlight elements marked with `data-layout-correct="false"`.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/optimized-appear/interrupt-tween-opacity-waapi.html#_snippet_0

LANGUAGE: css
CODE:
```
body { padding: 100px; margin: 0; } #box { width: 100px; height: 100px; background-color: #0077ff; } \[data-layout-correct="false"\] { background: #dd1144 !important; opacity: 1 !important; }
```

----------------------------------------

TITLE: Styling Basic Elements CSS
DESCRIPTION: Defines basic CSS styles for the body, an element with the ID 'box', and a rule to highlight elements with a specific data attribute used for layout correctness checks.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/optimized-appear/persist-optimised-animation.html#_snippet_0

LANGUAGE: css
CODE:
```
body { padding: 100px; margin: 0; } #box { width: 100px; height: 100px; background-color: #0077ff; } \[data-layout-correct="false"\] { background: #dd1144 !important; opacity: 1 !important; }
```

----------------------------------------

TITLE: Emulate Server Render and Start WAAPI Animation (JavaScript)
DESCRIPTION: Emulates server-side rendering by setting the root element's innerHTML with the rendered component string. It then starts a Web Animations API (WAAPI) animation for the 'transform' property, timing the client-side hydration to occur halfway through the WAAPI animation.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/optimized-appear/interrupt-tween-x.html#_snippet_2

LANGUAGE: javascript
CODE:
```
// Emulate server rendering of element
root.innerHTML = ReactDOMServer.renderToString(Component)
// Start WAAPI animation
const animation = startOptimizedAppearAnimation(
  document.getElementById("box"),
  "transform",
  ["translateX(0px)", "translateX(100px)"],
  {
    duration: duration * 1000,
    ease: "linear",
  },
  (animation) => {
    setTimeout(() => {
      ReactDOM.hydrateRoot(root, Component)
    }, (duration * 1000) / 2)
  }
)
```

----------------------------------------

TITLE: Basic Page Layout CSS
DESCRIPTION: Defines basic styling for the page body, a container element, and nested divs/boxes, setting padding, margins, display properties, and initial dimensions.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/benchmarks/warm-start-gsap.html#_snippet_0

LANGUAGE: css
CODE:
```
body { padding: 0; margin: 0; } .container { padding: 100px; width: 100%; display: flex; flex-wrap: wrap; } .container > div { width: 100px; height: 100px; } .box { width: 10px; height: 100px; background-color: #fff; }
```

----------------------------------------

TITLE: Creating and Updating Projection Nodes (JavaScript)
DESCRIPTION: Uses Framer Motion's Undo, Assert, and Projection utilities. It creates projection nodes for fixed and child elements, simulates page scrolling, updates the layout of the fixed element, and then asserts the expected viewport positions of both elements after the update.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/fixed-child-page-scroll.html#_snippet_1

LANGUAGE: javascript
CODE:
```
const { createNode } = window.Undo
const { matchViewportBox, matchVisibility, matchOpacity, addPageScroll,
} = window.Assert
const { frame } = window.Projection
const fixed = document.getElementById("fixed")
const fixedProjection = createNode(fixed, undefined, { layoutScroll: true, layout: true,
})
const fixedOrigin = fixed.getBoundingClientRect()
const child = document.getElementById("child")
const childProjection = createNode(child, fixedProjection)
const childOrigin = child.getBoundingClientRect()
childProjection.willUpdate()
fixedProjection.willUpdate()
const scrollDistance = 100
window.scrollTo(scrollDistance, scrollDistance)
fixed.style.justifyContent = "flex-end"
fixedProjection.root.didUpdate()
setTimeout(() => {
matchViewportBox(fixed, fixedOrigin)
matchViewportBox(child, {
top: 0,
left: 0,
right: 100,
bottom: 100,
})
}, 50)
```

----------------------------------------

TITLE: JavaScript DOM Manipulation and Projection
DESCRIPTION: Uses libraries (Animate, Assert, Projection) to get DOM elements, create a projection node, update it, add a class to the parent, and perform frame checks using `requestAnimationFrame`.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/perf-single.html#_snippet_1

LANGUAGE: javascript
CODE:
```
const { createNode, relativeEase } = window.Animate
const { matchViewportBox, checkFrame } = window.Assert
const { frame } = window.Projection
const parent = document.getElementById("parent")
const parentProjection = createNode(
 parent,
 undefined,
 {},
 { duration: 0.1 }
)
parentProjection.willUpdate()
parent.classList.add("b")
parentProjection.root.didUpdate()
requestAnimationFrame(() => {
 requestAnimationFrame(() => {
 console.log(window.ProjectionFrames)
 checkFrame(parent, 1, {
 totalNodes: 2,
 resolvedTargetDeltas: 1,
 recalculatedProjection: 1,
 })
 })
})
```

----------------------------------------

TITLE: Initializing Animation Variables (JavaScript)
DESCRIPTION: Selects the elements to animate and defines variables for animation timing and rotation calculation.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/benchmarks/pregenerated-background-color.html#_snippet_1

LANGUAGE: javascript
CODE:
```
const boxes = document.querySelectorAll(".box")
let startTime = 0
const duration = 10000
const rotateRate = 360 / duration
```

----------------------------------------

TITLE: Styling Basic Elements and Layout with CSS
DESCRIPTION: Defines basic styles for the body, a box element, position helper, an overlay with sticky positioning, a trigger element for overflow, and a style for elements with a specific data attribute. Sets padding and margin to zero for the body, dimensions and background for the box, height for position, sticky positioning for the overlay, and absolute positioning for the trigger. Also styles elements with `data-layout-correct="false"` to highlight them.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/sticky-scroll-no-layout-change.html#_snippet_0

LANGUAGE: css
CODE:
```
body { padding: 0; margin: 0; } #box { width: 100px; height: 100px; background-color: #00cc88; } #position { height: 300px; } #overlay { position: sticky; top: 0px; left: 0px; } #trigger-overflow { width: 1px; height: 1px; position: absolute; top: 2000px; left: 2000px; } \[data-layout-correct="false"\] { background: #dd1144 !important; opacity: 0.5; }
```

----------------------------------------

TITLE: Styling Layout Elements (CSS)
DESCRIPTION: Defines basic body styles, container dimensions, fixed positioning for an element, a trigger for overflow, and styles for elements with a specific data attribute.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/fixed-page-scroll-layout-change.html#_snippet_0

LANGUAGE: CSS
CODE:
```
body { padding: 0; margin: 0; } #container { width: 100px; height: 100px; } #fixed { position: fixed; top: 0; left: 0; width: 500px; height: 50px; background: #00cc88; } #trigger-overflow { width: 1px; height: 1px; position: absolute; top: 2000px; left: 2000px; } \[data-layout-correct="false"\] { background: #dd1144 !important; opacity: 0.5; }
```

----------------------------------------

TITLE: Performance Test for GSAP interpolate Utility
DESCRIPTION: This JavaScript snippet measures the execution time of the GSAP `interpolate` utility function. It creates an interpolator between 0 and 100, then calls it 100 million times, logging the total time taken. This requires the GSAP library to be included.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/benchmarks/mix-number-value-greensock.html#_snippet_1

LANGUAGE: javascript
CODE:
```
/\*\*\n \* Create an interpolate function that mixes unit values.\n \*\/\nconst px = gsap.utils.interpolate(0, 100)\nconst numRuns = 100000000\nlet startTime = performance.now()\nfor (let i = 0; i < numRuns; i++) {\n  px(i / numRuns)\n}\nconsole.log(\`First run: ${performance.now() - startTime}ms\`)
```

----------------------------------------

TITLE: Basic CSS Styling for Layout
DESCRIPTION: Defines fundamental CSS rules for the body, a container, and child elements to establish a basic page layout with padding, margin, flex display, and dimensions.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/benchmarks/mix-number-value-framer-motion.html#_snippet_0

LANGUAGE: css
CODE:
```
body { padding: 0; margin: 0; } .container { padding: 100px; width: 100%; display: flex; flex-wrap: wrap; } .container > div { width: 100px; height: 100px; } .box { width: 10px; height: 100px; background-color: #fff; }
```

----------------------------------------

TITLE: Styling Elements with CSS
DESCRIPTION: Defines basic styles for elements with IDs 'box' and 'child', including dimensions, background colors, and padding. It also includes a style for a hidden element used to trigger overflow and a style for incorrect layout states.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/new-element-concurrent.html#_snippet_0

LANGUAGE: css
CODE:
```
body { padding: 0; margin: 0; }
#box { width: 100px; height: 100px; background-color: #00cc88; }
#child { width: 50px; height: 50px; background-color: #0077ff; }
#box.b { height: 200px; position: absolute; top: 100px; left: 200px; padding: 20px; }
#trigger-overflow { width: 1px; height: 1px; position: absolute; top: 2000px; left: 2000px; }
\[data-layout-correct="false"\] { background: #dd1144 !important; opacity: 0.5; }
```

----------------------------------------

TITLE: Styling Elements for Layout Animation (CSS)
DESCRIPTION: Defines basic CSS styles for two box elements (#box-a, #box-b) and a trigger element. It includes positioning, dimensions, background color, and a style for indicating incorrect layout states.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/shared-promote-new-mix-remove.html#_snippet_0

LANGUAGE: css
CODE:
```
body { padding: 0; margin: 0; } #box-a { width: 100px; height: 100px; background-color: #00cc88; } #box-b { position: absolute; top: 100px; left: 100px; width: 200px; height: 300px; background-color: #09f; } #trigger-overflow { width: 1px; height: 1px; position: absolute; top: 2000px; left: 2000px; } \[data-layout-correct="false"\] { background: #dd1144 !important; opacity: 0.5; }
```

----------------------------------------

TITLE: Styling Body and Box Element (CSS)
DESCRIPTION: Defines basic styling for the body and a box element, including padding, margin, width, height, background color, and a special style for elements indicating incorrect layout states.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/optimized-appear/resync-delay.html#_snippet_0

LANGUAGE: css
CODE:
```
body { padding: 100px; margin: 0; } #box { width: 100px; height: 100px; background-color: #0077ff; } \[data-layout-correct="false"\] { background: #dd1144 !important; opacity: 1 !important; }
```

----------------------------------------

TITLE: Styling Elements with CSS
DESCRIPTION: Defines basic styles for the body, a box element (#box), a modified state of the box (#box.b), a hidden trigger element (#trigger-overflow), and a style for elements with a specific data attribute indicating incorrect layout.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/single-element-layout-change-rotate-change.html#_snippet_0

LANGUAGE: css
CODE:
```
body { padding: 0; margin: 0; }
#box { width: 100px; height: 100px; background-color: #00cc88; }
#box.b { width: 200px; position: absolute; top: 100px; left: 200px; padding: 20px; }
#trigger-overflow { width: 1px; height: 1px; position: absolute; top: 2000px; left: 2000px; }
\[data-layout-correct="false"\] { background: #dd1144 !important; opacity: 0.5; }
```

----------------------------------------

TITLE: Styling Box and Layout Elements (CSS)
DESCRIPTION: Defines basic styles for the page body, a #box element, a modified version of the box (#box.b), a hidden element to trigger overflow, and a style for elements indicating incorrect layout. These styles prepare the DOM for the JavaScript manipulation.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/transform-single-layout-change-with-scale-change.html#_snippet_0

LANGUAGE: CSS
CODE:
```
body { padding: 0; margin: 0; }
#box { width: 100px; height: 100px; background-color: #00cc88; }
#box.b { width: 200px; position: absolute; top: 100px; left: 200px; padding: 20px; }
#trigger-overflow { width: 1px; height: 1px; position: absolute; top: 2000px; left: 2000px; }
\[data-layout-correct="false"\] { background: #dd1144 !important; opacity: 0.5; }
```

----------------------------------------

TITLE: Styling Elements for Scroll and Layout Test (CSS)
DESCRIPTION: Defines basic styles for the body, a scrollable container (#scroll), a movable box (#box), a class to change the box's position (#box.b), an element to force overflow, and a style for elements indicating layout issues.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/element-scroll-layout-change.html#_snippet_0

LANGUAGE: CSS
CODE:
```
body { padding: 0; margin: 0; } #scroll { overflow: scroll; position: relative; height: 200px; width: 500px; } #box { position: absolute; left: 0px; top: 0px; width: 100px; height: 100px; background: #00cc88; } #box.b { left: 100px; } .trigger-overflow { width: 1px; height: 1px; position: absolute; top: 2000px; left: 2000px; } \[data-layout-correct="false"\] { background: #dd1144 !important; opacity: 0.5; }
```

----------------------------------------

TITLE: Testing Layout Projection with Framer Motion Utilities (JavaScript)
DESCRIPTION: Uses internal Framer Motion utilities (`createNode`, `matchViewportBox`, `frame`) to test layout projection. It creates projection nodes for elements, applies CSS class changes to modify layout, updates projections, asserts element positions against original bounds, reverts class changes, updates projections again, and performs a final assertion.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/single-element-with-child-layout-change-interrupt.html#_snippet_1

LANGUAGE: javascript
CODE:
```
const { createNode } = window.Undo
const { matchViewportBox } = window.Assert
const { frame } = window.Projection
const box = document.getElementById("box")
const boxProjection = createNode(box)
const child = document.getElementById("child")
const childProjection = createNode(child, boxProjection)
const boxOrigin = box.getBoundingClientRect()
const childOrigin = child.getBoundingClientRect()
boxProjection.willUpdate()
childProjection.willUpdate()
box.classList.add("b")
child.classList.add("b")
boxProjection.root.didUpdate()
frame.postRender(() => {
  matchViewportBox(box, boxOrigin)
  matchViewportBox(child, childOrigin)
  // Second render
  boxProjection.willUpdate()
  childProjection.willUpdate()
  box.classList.remove("b")
  child.classList.remove("b")
  boxProjection.root.didUpdate()
  frame.postRender(() => {
    matchViewportBox(box, boxOrigin)
    matchViewportBox(child, childOrigin)
  })
})
```

----------------------------------------

TITLE: Creating and Updating Projection Nodes with JavaScript
DESCRIPTION: Initializes variables by destructuring properties from global objects (Animate, Assert, Projection). It then retrieves DOM elements by ID and creates a hierarchy of projection nodes using `createNode`, linking child nodes to parent nodes. It triggers `willUpdate` on the nodes, modifies the parent element's class, calls `didUpdate` on the root node, and uses nested `requestAnimationFrame` calls to log projection frames and assert the state of the parent node's projection.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/perf-static-parent-child-grandchild.skip.html#_snippet_1

LANGUAGE: javascript
CODE:
```
const { createNode, relativeEase } = window.Animate
const { matchViewportBox, checkFrame } = window.Assert
const { frame } = window.Projection

const parent = document.getElementById("parent")
const parentProjection = createNode(
 parent, undefined, {}, { duration: 0.0001 }
)
const child = document.getElementById("child")
const childProjection = createNode(
 child, parentProjection, {}, { duration: 0.1 }
)
const grandChild = document.getElementById("grandChild")
const grandChildProjection = createNode(
 grandChild, childProjection, {}, { duration: 0.1 }
)

parentProjection.willUpdate()
childProjection.willUpdate()
grandChildProjection.willUpdate()

parent.classList.add("b")

parentProjection.root.didUpdate()

requestAnimationFrame(() => {
 requestAnimationFrame(() => {
 console.log(window.ProjectionFrames)
 checkFrame(parent, 1, {
 totalNodes: 4,
 resolvedTargetDeltas: 3,
 recalculatedProjection: 3,
 })
 })
})
```

----------------------------------------

TITLE: Styling Layout Elements (CSS)
DESCRIPTION: Defines basic styles for the body, a box element, an overlay, a hidden element to force overflow, and a style to indicate incorrect layout state.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/single-element-page-scroll-overlay.html#_snippet_0

LANGUAGE: css
CODE:
```
body { padding: 0; margin: 0; }
#box { width: 100px; height: 100px; background-color: #00cc88; }
#overlay { position: fixed; inset: 0; }
#trigger-overflow { width: 1px; height: 1px; position: absolute; top: 2000px; left: 2000px; }
\[data-layout-correct="false"\] { background: #dd1144 !important; opacity: 0.5; }
```

----------------------------------------

TITLE: Styling Body and Box with CSS
DESCRIPTION: Defines basic styles for the page body and a box element, including padding, margin, dimensions, background color, and a special rule for elements with `data-layout-correct="false"` to indicate layout issues.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/optimized-appear/defer-handoff-layout-opacity.html#_snippet_0

LANGUAGE: css
CODE:
```
body { padding: 100px; margin: 0; } #box { width: 100px; height: 100px; background-color: #0077ff; } \[data-layout-correct="false"\] { background: #dd1144 !important; opacity: 1 !important; }
```

----------------------------------------

TITLE: Styling Elements with CSS
DESCRIPTION: Defines CSS rules for the body, a box element, and a child element. Includes base styles and modified styles applied when the 'b' class is added, affecting width, position, and padding. Also includes a rule to trigger overflow and a rule to highlight incorrect layout.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/single-element-with-child-layout-change-page-scroll.html#_snippet_0

LANGUAGE: css
CODE:
```
body { padding: 0; margin: 0; }
#box { width: 100px; height: 100px; background-color: #00cc88; }
#child { width: 50px; height: 50px; background-color: #0077ff; }
#box.b { width: 200px; position: absolute; top: 100px; left: 200px; padding: 20px; }
#child.b { width: 100px; position: absolute; top: 150px; left: 250px; padding: 10px; }
#trigger-overflow { width: 1px; height: 1px; position: absolute; top: 2000px; left: 2000px; }
[data-layout-correct="false"] { background: #dd1144 !important; opacity: 0.5; }
```

----------------------------------------

TITLE: Basic CSS Styling and Layout Definitions
DESCRIPTION: Defines fundamental styles for body, a box element, its variant, a hidden trigger, and an element indicating layout issues. Includes padding, margin, dimensions, background, positioning, and opacity.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/single-element-page-scroll.html#_snippet_0

LANGUAGE: css
CODE:
```
body { padding: 0; margin: 0; } #box { width: 100px; height: 100px; background-color: #00cc88; } #box.b { width: 200px; position: absolute; top: 100px; left: 200px; padding: 20px; } #trigger-overflow { width: 1px; height: 1px; position: absolute; top: 2000px; left: 2000px; } \[data-layout-correct="false"\] { background: #dd1144 !important; opacity: 0.5; }
```

----------------------------------------

TITLE: Styling Elements for Layout Projection (CSS)
DESCRIPTION: Defines basic styles for the body and several box/child elements used in the layout projection example. Includes styles for initial elements (#box-a, #child-a) and target elements (#box-b, #child-b), plus a trigger element and a data attribute style for incorrect layout.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/shared-nested-promote-new-mix-interrupt.html#_snippet_0

LANGUAGE: CSS
CODE:
```
body { padding: 0; margin: 0; } #box-a { width: 100px; height: 100px; background-color: #00cc88; } #box-b { position: absolute; top: 100px; left: 100px; width: 200px; height: 300px; background-color: #09f; } #child-a { width: 50px; height: 50px; background-color: #09f; } #child-b { width: 100px; height: 50px; background-color: #00cc88; } #trigger-overflow { width: 1px; height: 1px; position: absolute; top: 2000px; left: 2000px; } [data-layout-correct="false"] { background: #dd1144 !important; opacity: 0.5; }
```

----------------------------------------

TITLE: Styling Parent and Child Elements (CSS)
DESCRIPTION: Defines basic styles for parent and child elements, including size, background color, and layout properties for a 'b' class state. Includes styles for overflow triggering and incorrect layout indication.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/animate-relative.html#_snippet_0

LANGUAGE: css
CODE:
```
body { padding: 0; margin: 0; }
#parent { width: 100px; height: 100px; background-color: #00cc88; }
#child { width: 50px; height: 50px; background-color: #0077ff; }
#parent.b { width: 200px; position: absolute; top: 100px; left: 200px; padding: 20px; display: flex; justify-content: flex-end; }
.b #child { width: 100px; height: 100px; }
#trigger-overflow { width: 1px; height: 1px; position: absolute; top: 2000px; left: 2000px; }
\[data-layout-correct="false"\] { background: #dd1144 !important; opacity: 0.5; }
```

----------------------------------------

TITLE: GSAP Element Creation and Animation
DESCRIPTION: Creates multiple div elements with a nested box class, adds them to the DOM, sets initial GSAP properties using gsap.set, and then animates them with random rotation, color, width, and x-position using gsap.to after a delay.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/benchmarks/warm-start-gsap.html#_snippet_1

LANGUAGE: javascript
CODE:
```
// Create boxes
const numBoxes = 100
let html = ``
for (let i = 0; i < numBoxes; i++) {
  html += `<div><div class="box"></div></div>`
}
document.querySelector(".container").innerHTML = html
const boxes = document.querySelectorAll(".box")
gsap.set(boxes, {
  rotate: 0,
  backgroundColor: "#fff",
  width: "0%",
  x: 0,
})
setTimeout(() => {
  // Warm start
  boxes.forEach((box) =>
    gsap.to(box, {
      rotate: Math.random() * 360,
      backgroundColor: "#f00",
      width: Math.random() * 100 + "%",
      x: 5,
      duration: 1,
    })
  )
  // setTimeout(() => {
  //   // Unit conversion
  //   boxes.forEach((box) =>
  //     gsap.to(box, {
  //       width: Math.random() * 100 + "px",
  //       x: "50%",
  //       duration: 1,
  //       easing: "linear",
  //     })
  //   )
  // }, 1500)
}, 1000)
```

----------------------------------------

TITLE: Styling Elements for Framer Motion Layout Test (CSS)
DESCRIPTION: Defines basic CSS styles for the body and two box elements (`#box-a`, `#box-b`) used in a Framer Motion layout test. Includes a hidden element (`#trigger-overflow`) to potentially test scrolling behavior and a style for elements marked as having incorrect layout.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/shared-promote-new-mix-rotate-remove.html#_snippet_0

LANGUAGE: CSS
CODE:
```
body { padding: 0; margin: 0; } #box-a { width: 100px; height: 100px; background-color: #00cc88; } #box-b { position: absolute; top: 100px; left: 100px; width: 200px; height: 300px; background-color: #09f; } #trigger-overflow { width: 1px; height: 1px; position: absolute; top: 2000px; left: 2000px; } \[data-layout-correct="false"\] { background: #dd1144 !important; opacity: 0.5; }
```

----------------------------------------

TITLE: Styling Elements for Projection Test (CSS)
DESCRIPTION: Defines basic styles for the body, parent, and child elements used in the projection test, including dimensions, layout properties (flexbox), background colors, and a hidden element to trigger overflow.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/shared-promote-new-mix-rotate-layout.html#_snippet_0

LANGUAGE: css
CODE:
```
body { padding: 0; margin: 0; width: 100vw; height: 100vh; display: flex; justify-content: center; align-items: center; } .parent { background: #363636; display: flex; justify-content: flex-end; align-items: flex-end; } .child { background: #ff0055; } #parent-a { width: 100px; height: 100px; } #child-a { width: 50px; height: 50px; } #parent-b { width: 400px; height: 200px; } #child-b { width: 100px; height: 100px; } #trigger-overflow { width: 1px; height: 1px; position: absolute; top: 2000px; left: 2000px; } \[data-layout-correct="false"\] { background: #09f !important; opacity: 0.5; }
```

----------------------------------------

TITLE: Creating and Animating Elements with Anime.js (JavaScript)
DESCRIPTION: Dynamically creates 100 box elements and inserts them into the DOM. It then uses setTimeout to trigger two sequences of animations on these boxes via the anime.js library, demonstrating property animation, random values, and unit conversion.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/benchmarks/cold-start-anime.html#_snippet_1

LANGUAGE: javascript
CODE:
```
// Create boxes
const numBoxes = 100
let html = ``
for (let i = 0; i < numBoxes; i++) {
  html += `<div><div class="box"></div></div>`
}
document.querySelector(".container").innerHTML = html
const boxes = document.querySelectorAll(".box")
setTimeout(() => {
  // Cold start (read from DOM)
  boxes.forEach((box) =>
    anime({
      targets: box,
      rotate: Math.random() * 360,
      backgroundColor: "#f00",
      width: Math.random() * 100 + "%",
      translateX: 5,
      duration: 1000,
      easing: "linear",
    })
  )
  setTimeout(() => {
    // Unit conversion
    boxes.forEach((box) =>
      anime({
        targets: box,
        width: Math.random() * 100 + "px",
        translateX: "50%",
        duration: 1000,
        easing: "linear",
      })
    )
  }, 1500)
}, 1000)
```

----------------------------------------

TITLE: Defining Basic Element Styles (CSS)
DESCRIPTION: Defines basic CSS styles for the body, two box elements (#box-a, #box-b), a hidden trigger element, and a style for indicating incorrect layout.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/shared-block-update-promote-new.html#_snippet_0

LANGUAGE: css
CODE:
```
body { padding: 0; margin: 0; } #box-a { width: 100px; height: 100px; background-color: #00cc88; } #box-b { position: absolute; top: 100px; left: 100px; width: 200px; height: 300px; background-color: #09f; } #trigger-overflow { width: 1px; height: 1px; position: absolute; top: 2000px; left: 2000px; } \[data-layout-correct="false"\] { background: #dd1144 !important; opacity: 0.5; }
```

----------------------------------------

TITLE: Styling Layout Elements for Projection Test (CSS)
DESCRIPTION: Provides essential CSS rules for the body, box, and overlay elements used in the layout projection test. Includes fixed positioning for the overlay, dimensions and background for the box, and a hidden element designed to force page overflow.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/single-element-page-scroll-animated-overlay.html#_snippet_0

LANGUAGE: css
CODE:
```
body { padding: 0; margin: 0; } #box { width: 100px; height: 100px; background-color: #00cc88; } #overlay { position: fixed; inset: 0; } #trigger-overflow { width: 1px; height: 1px; position: absolute; top: 2000px; left: 2000px; } \[data-layout-correct="false"\] { background: #dd1144 !important; opacity: 0.5; }
```

----------------------------------------

TITLE: Styling Layout Elements (CSS)
DESCRIPTION: Defines basic styles for the body, a box element, a button element, a trigger element, and a data attribute selector. Sets padding, margin, dimensions, positioning, background color, and opacity.
SOURCE: https://github.com/grx7/framer-motion/blob/main/dev/html/public/projection/single-element-page-scroll-scale.html#_snippet_0

LANGUAGE: css
CODE:
```
body { padding: 0; margin: 0; } #box { width: 300px; height: 100px; position: absolute; top: 200px; left: 50%; } #button { position: absolute; inset: 0; background-color: #00cc88; } #trigger-overflow { width: 1px; height: 1px; position: absolute; top: 2000px; left: 2000px; } \[data-layout-correct="false"\] { background: #dd1144 !important; opacity: 0.5; }
```