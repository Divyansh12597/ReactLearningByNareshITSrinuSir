# Lecture no 35

# React `useEffect()` Hook - Complete Notes

---

# React Component Lifecycle

Every React component goes through **3 lifecycle phases**.

| Phase | Description | Example |
|--------|-------------|---------|
| **1. Mounting** | Component loads for the first time and is added to the browser. | `<App />` is rendered for the first time. |
| **2. Updating** | Component re-renders because **state** or **props** change. | `"Sachin"` changes to `"Dhoni"` or `count` changes from `0` to `1`. |
| **3. Unmounting** | Component is removed from the browser. | User closes the component or navigates to another page. |

```text
Component Created
        │
        ▼
   Mounting
        │
        ▼
   Updating (0 or many times)
        │
        ▼
   Unmounting
```

---

# Performing Operations Before and After Rendering

Suppose we want to perform some activity before or after rendering the component.

```jsx
const App = () => {

    // Code executed before rendering JSX

    return <div>Sachin</div>;

    // ❌ Wrong place
    // Code written here will never execute.
}
```

### Explanation

React immediately returns the JSX after the `return` statement.

Anything written after `return` is **unreachable code**.

If we want to perform an operation **after rendering**, React provides the **useEffect() Hook**.

---

# useEffect Hook

`useEffect()` is used to handle the **lifecycle phases** of a React component.

It allows us to perform operations like:

- Calling APIs
- Logging
- Setting Timers
- Working with Local Storage
- Adding Event Listeners
- Cleaning up resources

---

# Syntax

```jsx
React.useEffect(() => {

}, []);
```

or

```jsx
React.useEffect(
    CallbackFunction,
    DependencyArray
);
```

---

## Parameters

| Parameter | Description |
|-----------|-------------|
| Callback Function | Code that executes after rendering |
| Dependency Array | Controls when the callback should execute (Optional) |

---

# Dependency Array

The dependency array tells React **when the useEffect should execute**.

---

## Case 1 : Empty Dependency Array

```jsx
React.useEffect(() => {
    console.log("Executed");
}, []);
```

### Meaning

- Doesn't listen to any state or props.
- Executes only once.
- Runs after the component mounts.

```text
Mount
   │
   ▼
useEffect()
```

---

## Case 2 : No Dependency Array

```jsx
React.useEffect(() => {
    console.log("Executed");
});
```

### Meaning

Since no dependency array is provided, the effect executes

- After first render
- After every re-render

```text
Mount
   │
   ▼
useEffect()

State Changed
   │
   ▼
useEffect()

Props Changed
   │
   ▼
useEffect()
```

---

## Case 3 : Dependency Array with State

```jsx
React.useEffect(() => {
    console.log("Executed");
}, [count]);
```

### Meaning

The effect executes

- After first render
- Whenever `count` changes

If another state changes (like `name`), this effect will **not execute**.

---

# Example 1 - Mounting Phase

```jsx
function App() {

    debugger;

    React.useEffect(() => {
        debugger;
        console.log("After render content inside the browser");
    }, []);

    return <h1>Hello</h1>;
}
```

---

## Execution Flow

```text
App() called
      │
      ▼
debugger
      │
      ▼
React registers useEffect
(It does NOT execute immediately)
      │
      ▼
Return JSX
<h1>Hello</h1>
      │
      ▼
Browser displays UI
      │
      ▼
Now useEffect executes
```

### Output

```text
Hello displayed

Console:
After render content inside the browser
```

---

## Important Point

Many beginners think `useEffect()` executes immediately.

This is **not true**.

React first

- Executes the component function
- Creates JSX
- Updates the DOM
- Paints the UI
- Then executes `useEffect()`

---

# Example 2 - Updating Phase

```jsx
function App() {

    const [cnt, setCnt] = React.useState(0);
    const [name, setName] = React.useState("Sachin");

    React.useEffect(() => {
        console.log("After render content inside the browser");
    }, [cnt]);

    return (
        <div>

            <h1>{cnt}</h1>

            <button onClick={() => setCnt(cnt + 1)}>
                Increment
            </button>

            <h1>{name}</h1>

            <button
                onClick={() =>
                    setName(name === "Sachin" ? "Dhoni" : "Sachin")
                }>
                Change Name
            </button>

        </div>
    );
}
```

---

## Initial Render

```text
Render UI

cnt = 0
name = Sachin

↓

useEffect executes
```

Console

```text
After render content inside the browser
```

---

## Click Increment

```text
cnt changes

↓

React re-renders

↓

Dependency changed

↓

useEffect executes
```

Console

```text
After render content inside the browser
```

---

## Click Change Name

```text
name changes

↓

React re-renders

↓

Dependency = [cnt]

↓

cnt did not change

↓

useEffect does NOT execute
```

Console

```text
No Output
```

---

# Prevent useEffect on Initial Render

Sometimes we don't want the effect on the first render.

```jsx
React.useEffect(() => {

    if (cnt !== 0) {
        console.log("After render content inside the browser");
    }

}, [cnt]);
```

### Output

Initial Render

```text
No Output
```

After Increment

```text
After render content inside the browser
```

---

# Example 3 - Unmounting Phase

When a component is removed from the browser, React executes the **cleanup function**.

---

## Child Component

```jsx
const Child = () => {

    React.useEffect(() => {

        console.log("Child Mounted");

        return () => {
            console.log("I am going to unmount...");
        };

    }, []);

    return (
        <div>
            <h1>Child Component</h1>
        </div>
    );
};
```

---

## Parent Component

```jsx
function App() {

    const [check, setCheck] = React.useState(true);

    const fChange = (event) => {
        setCheck(event.target.checked);
    };

    return (
        <div>

            <h1>App Component</h1>

            <label>
                <input
                    type="checkbox"
                    checked={check}
                    onChange={fChange}
                />
                Show Child
            </label>

            <hr />

            {check && <Child />}

        </div>
    );
}
```

---

## Execution Flow

### Initial Render

```text
App Mounted
      │
      ▼
Child Mounted
      │
      ▼
Console:
Child Mounted
```

---

### User Unchecks Checkbox

```text
Child Component Removed
      │
      ▼
Cleanup Function Executes
      │
      ▼
Console:
I am going to unmount...
```

---

### User Checks Checkbox Again

```text
Child Component Mounted Again
      │
      ▼
Console:
Child Mounted
```

---

# What is Cleanup Function?

The function returned from `useEffect()` is called the **Cleanup Function**.

```jsx
React.useEffect(() => {

    console.log("Mounted");

    return () => {
        console.log("Cleanup");
    };

}, []);
```

It executes

- Before the component unmounts.
- Before the effect runs again (if dependencies change).

---

# Real Example

```jsx
React.useEffect(() => {

    const id = setInterval(() => {
        console.log("Running...");
    }, 1000);

    return () => {
        clearInterval(id);
        console.log("Interval Cleared");
    };

}, []);
```

Without cleanup, the interval would continue running even after the component is removed, causing a memory leak.

---

# Summary

| Dependency Array | When useEffect Executes |
|------------------|------------------------|
| `[]` | Only once after first render (Mounting) |
| No dependency array | After every render |
| `[count]` | After first render and whenever `count` changes |
| `[name]` | After first render and whenever `name` changes |
| `return () => {}` | Executes during unmounting and before the effect re-runs |

---

# Interview Questions

## Q1. What is useEffect?

`useEffect` is a React Hook used to perform side effects such as API calls, logging, timers, event listeners, and cleanup after rendering.

---

## Q2. When does useEffect execute?

After React finishes rendering the component.

---

## Q3. What happens if dependency array is empty?

The effect executes only once after the component mounts.

---

## Q4. What happens if dependency array is omitted?

The effect executes after every render.

---

## Q5. What happens if dependency array contains a state?

The effect executes after the first render and whenever that state changes.

---

## Q6. What is Cleanup Function?

The function returned from `useEffect()` is called the Cleanup Function.

It is used to

- Remove event listeners
- Clear timers
- Close WebSockets
- Cancel subscriptions
- Prevent memory leaks

---

# Key Points to Remember

- `useEffect()` always executes **after rendering**.
- The dependency array controls when the effect runs.
- `[]` → Runs only once.
- No dependency array → Runs after every render.
- `[state]` → Runs when that state changes.
- Cleanup function executes before unmounting and before the effect re-runs.
- Cleanup functions are used to release resources and avoid memory leaks.