# React Fundamentals

## Index
- [What is React?](#what-is-react)
    - [Library vs Framework](#library-vs-framework)
    - [Virtual DOM vs Real DOM](#virtual-dom-vs-real-dom)
    - [JSX & Components](#jsx--components)
    - [React Philosophy & Architecture](#react-philosophy--architecture)
    - [Data Management: Props & State](#data-management-props--state)
    - [Refs & Context](#refs--context)
    - [Advanced Patterns & Logic](#advanced-patterns--logic)
    - [Optimization & Performance](#optimization--performance)
- [Data Driven React](#data-driven-react)
    - [Component Props](#component-props)
    - [The Key Prop](#the-key-prop)
    - [Modern Context: Data-Driven Performance](#modern-context:-data-driven-performance)
- [React State and Interaction](#react-state-and-interaction)
   - [Static vs. Dynamic Web Applications](#static-vs-dynamic-web-applications)
   - [Props vs. State](#props-vs-state)
   - [The useState Hook](#the-usestate-hook)
   - [React Forms and Interaction Patterns](#react-forms-and-interaction-patterns)
   - [React Data Flow and Effects](#react-data-flow-and-effects)

---

## What is React?
React is a composable and declarative JavaScript library introduced in 2013 by Meta. It focuses on the "View" layer of the MVC (Model-View-Controller) pattern and maintains a large ecosystem.

### Library vs Framework

| Library | Framework |
| --- | --- |
| Collection of reusable code | Predetermined architecture (follows a pattern) |
| Reduces writing from scratch | Work within strict boundaries |
| Few boundaries on implementation | Specific right/wrong ways to use it |
| User controls how and when it is used | Less suitable for small, simple projects |

> **React Limitations:** As a library, it lacks built-in standards for routing or state management. Solution: Use React-based frameworks like Next.js for structural standardization.


### Virtual DOM vs Real DOM

| Virtual DOM | Real DOM |
| --- | --- |
| Cannot directly update HTML | Directly updates and manipulates HTML |
| Acts as a copy/blueprint of the real DOM | Creates a new DOM/full repaint on update |
| Updates only what is necessary (Diffing) | Object-based representation of the HTML doc |
| Synced via react-dom | Can be slow with frequent complex updates |

### JSX & Components
**JSX (JavaScript XML):** An extension that allows for HTML-like template syntax inside JavaScript files.

- **Elements:** JSX produces objects that represent the UI.
  `const element = <div>React</div>;`
- **Components:** A function that returns a React element.
  `const Component = () => { return <div>React</div>; };`
- **PascalCase:** Custom components must always start with a capital letter to be recognized by React.

### React Philosophy & Architecture
- **Declarative vs. Imperative:** *Imperative:* Manual step-by-step DOM manipulation (e.g., jQuery). *Declarative:* Describe the desired state; React handles the underlying logic.
- **Composability (The LEGO/Michelangelo Analogy):** Complex systems are built from small, interchangeable pieces. Like LEGO bricks, changing one definition updates the "statue" everywhere that brick is used.
- **Fragments:** Use `<>...</>` to group elements without adding extra div nodes to the HTML.

### Data Management: Props & State
- **Props (Parent to Child):** Data passed down the tree; they are read-only (Pure Components).
- **Child to Parent:** Accomplished by passing a function prop that the child calls to "lift" data.
- **Prop Drilling:** Passing data through multiple levels to reach a target component.
    - *Pro:* Explicit tracing.
    - *Cons:* Can become messy and lead to accidental data manipulation.
- **State:** Local data managed inside a component. Changes trigger a re-render.

### Refs & Context
- **Refs (useRef):** Values persist across renders but do not trigger re-renders.
    - **Modern Update (React 19+):** Ref callbacks can now return a cleanup function. You can also pass `ref` as a standard prop to any functional component — `forwardRef` is no longer needed.
- **Context API:** Shares values implicitly across the component tree to avoid prop drilling.
    - **Modern Update:** Use `<Context>` directly instead of the older `<Context.Provider>` syntax.

### Advanced Patterns & Logic
- **HOC (Higher Order Components):** A function that takes a component and returns an enhanced version.
- **Portals:** Renders children into a DOM node outside the parent hierarchy (common for Modals).
- **Controlled vs. Uncontrolled:** *Controlled:* React state manages the input value. *Uncontrolled:* The DOM/User manages the input value.
- **Modern Update — Actions (React 19+):** Use `useActionState` to handle form Pending (loading) and Error states automatically, significantly reducing the need for manual Controlled Components in standard data entry forms.

### Optimization & Performance
- **Virtual DOM:** Calculates the minimum changes needed to sync with the real DOM.
- **Modern Update — The React Compiler:** Manual optimization tools like `useMemo` and `useCallback` are largely deprecated. The React Compiler automatically optimizes re-renders at build-time.

---

## Data Driven React

### Component Props
Props make components more reusable by allowing data to flow from parents to children.

- **Passing Props:** Data is passed to the component at the call site as attributes.
- **Native Elements:** You cannot pass custom props to native DOM elements (e.g., `<div customProp="value">`). Native elements only accept standard HTML attributes.
- **Receiving Props:** Props are received as a single object parameter in the functional component.
- **Destructuring:** To keep code clean, you can destructure the props object directly in the parameter list.


### The Key Prop
The "key" is a special string attribute that must be included when creating lists of elements in React.

- **Tracking Changes:** React uses keys to identify which items in a list have changed, been added, or been removed.
- **Performance:** Without unique keys, React cannot perform granular updates and must re-render the entire list.
- **Warning:** If a key is missing during an array map, React will trigger a console warning, defaulting to the array index, which is unstable for dynamic lists.
- **Best Practice:** Always use a unique ID from your data (such as a database UUID) rather than the array index. This is critical if the list can be sorted, filtered, or deleted.


### Modern Context: Data-Driven Performance
- **Automatic Key Detection:** While explicit keys remain mandatory for dynamic arrays, the React Compiler can identify stable identities in certain static lists at build-time. Manual keys are still the industry standard for any data fetched from an API.
- **Server-Driven Props:** In the era of Server Components, props are often serialized to be sent from the server to the client. Props must be "plain objects" (POJOs) so they can be stringified and sent over the network.
- **Identity Stability:** Modern React architecture relies on stable keys to maintain "Client State" (like scroll position or input focus) when "Server Data" updates.


---

## React State and Interaction

### Static vs. Dynamic Web Applications
- **Static Web Apps:** Read-only environments where user-driven changes to data do not occur (e.g., blogs, news sites, recipe pages).
- **Dynamic Web Apps:** Read and write environments that are highly interactive. The UI changes based on user data (e.g., banking portals, Airbnb, e-commerce sites).

### Props vs. State

**State** refers to values managed internally by the component.
- **Analogy:** Think of a light switch and a bulb. The "state" is whether the switch is ON or OFF; the "view" (the light) reacts accordingly.
- **Component Memory:** State values are "remembered" across render cycles.
- **Re-rendering:** React "reacts" to state changes. Calling a state setter function triggers a re-render to keep the UI in sync with the data.
- **Logic:** `View = Function(State)`

**Props** refer to data passed into a component from its parent.
- **Analogy:** Like function parameters or instructions "from above."
- **Immutability:** The component receiving props is not allowed to modify them. Props are read-only.

### The useState Hook
Simply changing a local variable does not trigger a re-render. To update the UI, you must use the `useState` hook.

- **Returns:** An array containing the current **State Value** and a **Setter Function**.
- **Setter Function:** Can accept a direct new value or a callback function (useful when the new state depends on the previous state).
- **Complex State:** When working with arrays or objects, never mutate the original. Always provide a brand new version (using the spread operator) so React can detect the change.

---

## React Forms and Interaction Patterns

### React Forms and Data Collection
Forms are the primary interface for user communication with a database.

- **Legacy Approach:** Accessing data through the `name` attribute of input elements and manual submission handling.
- **Modern Declarative Approach:** React handles most steps automatically, focusing on the data state rather than manual DOM selection.
- **FormData API:** Use `Object.fromEntries(new FormData(event.currentTarget))` to convert form data into a clean JavaScript object instantly, avoiding the need for manual state updates for every input field.

### Conditional Rendering
React allows for dynamic UI changes based on specific conditions using standard JavaScript logic.

- **Logical AND (&&):** Evaluates the left side first. If true, it renders the right side. If false, the right side is ignored. Order is critical for correct evaluation.
- **Ternary Operator:** Used for "either/or" logic.
- **Note on Zero:** React may render `0` as a visible character if it is passed as a falsy value (e.g., `count && <Component />` where count is 0).

---

## React Data Flow and Effects

### Data Flow: Downwards Only
Data in React follows a strict unidirectional flow from parent to child.

- **Re-rendering:** When state changes within a component, that component and all of its nested children will re-render to reflect the new data.
- **Flow Constraints:** Data cannot be passed directly to siblings or upwards to parents.
- **Lifting State Up:** To share data between siblings, the state must be moved to their closest common ancestor, which then passes the data back down via props.

### Functional Programming Principles in React
- **Pure Components:** Given the same inputs (props), they always produce the same output (UI). They should not modify variables outside their scope.
- **Immutability:** State and props should never be modified directly. Always use setter functions to replace old state with a brand new version so React can detect changes and trigger proper re-renders.
- **Avoiding Side Effects:** Components should ideally only calculate the UI. Logic affecting external systems must be isolated to keep the rendering process "clean."

### Controlled Components
A component is "Controlled" when React state serves as the "single source of truth" for form inputs.

- **Mechanism:** The input `value` is bound to a state variable, and an `onChange` event updates that state. Every keystroke is captured and processed by React.
- **Uncontrolled Components:** The DOM handles its own internal state, similar to traditional HTML behavior.
- **Use Cases:** Controlled components are essential for instant field validation, real-time search filtering, and managing dynamic button states (e.g., disabling a button until an input meets specific criteria).

### The useEffect Hook
React's core responsibilities are rendering UI and maintaining state. Side effects (API calls, subscriptions, external systems) must be isolated using `useEffect`.

- **The Problem:** Fetching data directly in the component body creates an infinite loop: fetching updates state → state triggers re-render → re-render triggers another fetch.
- **The Solution:** `useEffect` wraps the side effect, guaranteeing it only runs after the component has mounted to the DOM.
- **Dependencies:** The dependency array `[]` determines when the effect re-runs.
    - **Empty `[]`:** Runs once on mount.
    - **With variables `[data]`:** Runs only when those specific values change.
    - **No array:** Runs on every render (rarely desired).
- **Cleanup:** The function returned by the effect clears listeners or cancels fetches.

### Modern 2026 Context
- **Transitioning from useEffect:** For standard data fetching, the 2026 standard uses **Server Components** (async/await on the server) or the **use() API** (unwraps promises directly in render, usable inside loops and conditionals). These patterns handle asynchronous data loading natively without the boilerplate or infinite loop risks of `useEffect`.
- **Automatic Form Actions:** React 19+ introduces **Actions** with `useActionState` to handle form submissions and loading states automatically.
