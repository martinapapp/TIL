# Advance React

## Index
1. [Reusability](#reusability)
    - [Children prop](#children-prop)
    - [Compound Components](#compound-components)
    - [Context](#context)
    - [Composition](#composition)
    - [Render props](#render-props)
    - [Custom Hooks](#custom-hooks)
    - [Use Cases](#use-cases)

2. [Performance](#performance)
  - [How React REnders Components](#)
  - [React.StrictMode](#)
  - [Code splitting](#)
  - [useMemo() hook](#)
  - [React.memo()](#)
  - [useCallback()](#)

## Reausability

### Children prop

The `children` prop lets you pass JSX directly into a component, making it a flexible wrapper.
It's great for layout components like cards, modals, or containers where the inner content varies.
 
```jsx
function Card({ children }) {
  return <div className="card">{children}</div>;
}
 
// Usage
<Card>
  <h2>Title</h2>
  <p>Some content here.</p>
</Card >
```

**Prop drilling**:
 - Prop drilling happens when you pass props through multiple intermediate components that don't actually need them, just to get data down to a deeply nested child.
 - lot of components don't need that data
 - solutions:
    1. if the app is small (1-2 level), this is ok
    2. **compound components**
        - flattens structure
        - easy pass props to more deeply nested components
    3. **context**
        - access state directly from the components that need it 

### Compound Components

Compound components are a pattern where a parent component and several child components work together to share state implicitly. Think of how `<select>` and `<option>` work in HTML.

- use children props
- have dedicated function/styling
- make the component structure transparent
- give more control of the component

Compound Components 'flatten' the hierarchy that you would otherwise need to pass props through. Since you need to provide the children to render, the parent-most component has direct access to the "grandChild" component, to which it can pass whatever props it needs to pass directly.

```jsx
function Tabs({ children }) { ... }
Tabs.Tab = function Tab({ label, onClick }) { ... }
Tabs.Panel = function Panel({ children }) { ... }
 
// Usage
<Tabs>
  <Tabs.Tab label="Home" />
  <Tabs.Panel >Home content</Tabs.Panel >
</Tabs>
```

### Context

React Context (`.createContext()`)lets you share state across a component tree without prop drilling. Best used for
truly global data like theme, locale, or auth state - not as a general state management replacement.
Hook: `useContext()` - provides immidiate access to the value ( ~ teleports).

```jsx
const ThemeContext = React.createContext('light');
 
function App() {
  return (
    <ThemeContext.Provider value="dark">
      <Page />
    </ThemeContext.Provider>
  );
}
 
function Page() {
  const theme = useContext(ThemeContext) // 'dark'
  return <div className={theme}>...</div>
}
```


### Composition

Composition is the practice of building complex UIs by combining simple, focused components
rather than inheriting from them. In React, prefer composition over inheritance in almost every case.

```jsx
function Avatar({ user }) {
  return <img src={user.avatar} alt={user.name} />
}
 
function UserCard({ user }) {
  return (
    <div>
      <Avatar user={user} />   {/* composed in */}
      <span>{user.name}</span>
    </div>
  )
}
```

### Render props

A render prop is a function prop that a component calls to know what to render. It lets you
share stateful logic while giving the consumer full control over the output.

```jsx
function MouseTracker({ render }) {
  const [pos, setPos] = useState({ x: 0, y: 0 });
  return (
    <div onMouseMove={e => setPos({ x: e.clientX, y: e.clientY })}>
      {render(pos)}
    </div>
  );
}
 
// Usage
<MouseTracker render={({ x, y }) => <p>Mouse at {x}, {y}</p>} />
```

### Custom Hooks

Custom hooks let you extract and reuse stateful logic across components. They're plain functions
that start with `use` and can call other hooks internally.

```jsx
function useFetch(url) {
  const [data, setData] = useState(null)
  const [loading, setLoading] = useState(true)
 
  useEffect(() => {
    fetch(url)
      .then(res => res.json())
      .then(data => { setData(data) setLoading(false) })
  }, [url])
 
  return { data, loading }
}
 
// Usage
const { data, loading } = useFetch('/api/users')
```

### Use Cases

- **Children / Composition** — default starting point for most reuse needs
- **Compound Components** — when related components need to share implicit state
- **Context** — when prop drilling gets out of hand across many levels
- **Render Props / Custom Hooks** — for sharing logic; hooks are the modern preferred approach, render props are less common today but useful to know

## Performance

### How React Reenders Components

- Re-render triggers: 
    - state value change, 
    - context change, 
    - props change,
    - parent re-render

1. phase : Render
2. phase: Reconcilation
3. phase: Commit to DOM

- analogy: architect updating a building plan

| Update blueprints | Listing changes|  Build |
|-------------------|----------------|--------|
| makes new version | determine what actual changes need to be implemnted, no need to rebuild from scratch | make the actual building updtes. do as little as possible work |

| Render | Reconcialiation | Commit |
|-------------------|----------------|--------|
| all components in this branch called again. any calc, function, effect run in JS again + new virtual DOM model is created | React use "diffing algorithm"(compares the virtual DOM) to check previous version of the virtual DIM against the new version & determines what actually changes | React makes changes to the actual DOM, does the smallest amount of work |

For measure performance in Dev tools:

 - inspect/performance
 - CPU: no throttling (4x/6x slowdown)
 - Network: no throttling (Fast 4G, Slow 4G, 3G)
 - Profiler - recording

or use React Developer Tools own Profiler.


### React.StrictMode

With StrictMode find rendering bug:

- double renders all function that should be pure funciton (Double renders only happen in development mode, never in production!)
- re-runs all effects in components
- checks for far deprected React App
- eg.:  setInterval is still runing when it is hidden -> memory leak, timer is counting by 2

```
<React.StrictMode>
  <App />
</React.StrictMode >
```

### Code splitting

- for reducing downloading size
- conditionally import "heavy" code if/when needed
- splits up download times, so main features aren't blocked by slow connections
- best case: bypasses unneeded code altogether
  - dynamic input: import()
  - combine: import() + React.lazy()
  - provide fallback UI while the lazy stuff is loading: <Suspense>

```
const ProductList = REact.lazy(()=>{
  return import("./Productlist")
})

<React.Suspense fallback={spinner}>
  {conditionally render <ProductList />}
</React.Suspense>
```

### useMemo() hook

To remember calculated values between renders.

1. *avoid recalculating expnesive calcs if not necessary*
2. *maintain referential equality of a complex data types between renders*
3. *only use it for genuinely expensive calculations or to preserve referential equality*

### React.memo()

To reduce unnecessary rerenders.

It does a shallow comparison by default - meaning it only checks one level deep. If you pass a nested object, changes inside it won't be detected. The optional second argument lets you customize the comparison.

**HOC**
 - High Order Component
 - a function that has special abilities (returns a "beefed up" version of the component)
 - caches/remembers a component if the props don't change from one render to the next

 ```
  export default function Component(){
    // logic here
  }
  
  React.memo(Component, ()=>{//optional function})
 ```

### Referential Equality

- JS stores all functions in memory
  - value types (primitive data types)
  - refernce types (complex data types)
- JS manages memory for these types differently

**Value vs Reference types**
  - value/primitive types are equal if they have the same value
```
a = 1 === b = 1     //true
```

  - ref/complex types are equal if they references the exact same thing in memory

```
let a = {}
let b = {}
console.log(Object.is(a,b))
// false

let a = {num: 1}
let b = {num: 1}
console.log(Object.is(a,b))
// false

let a = {}
let b = a
console.log(Object.is(a,b))
// true
```

### useCallback() hook

For caching function definitions.

- maintain refernce to functions between rendres
  - avoid rerender with React.memo()
  - avoid too many useEffect() calls
- only use it when passing functions to memoized children (React.memo) or as a dependency in useEffect.

```
const increment = REact.useCallback(()=>{
  setCount(prevCount => prevCount + 1)
}, [setCount])
```
