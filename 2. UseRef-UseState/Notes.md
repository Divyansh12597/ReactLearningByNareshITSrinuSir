### React Notes:  Naresh IT Recording: 24, 25
---

# Event Handling in React

## Correct Way

```jsx
<button onClick={f1}>Click</button>
```

* `onClick={f1}` → function reference
* This is called an **event handler**

---

## Wrong Way

```jsx
<button onClick={f1()}>Click</button>
```

### Why Wrong?

If we write:

```jsx
onClick={f1()}
```

then the function executes immediately during the initial rendering of the component.

---

# Functional Components

> Functional components are static by default.
> Hooks make them dynamic.

---

# useRef()

> `useRef` is a React Hook that stores mutable values and DOM references without causing component re-renders.

---

## Important Interview Questions

| Question                  | Answer                   |
| ------------------------- | ------------------------ |
| Event handler should be?  | Function                 |
| Empty reference creation  | `React.useRef()`         |
| Type of `useRef()`        | Function                 |
| Return type of `useRef()` | Object                   |
| Return structure          | `{ current: undefined }` |
| Bind ref to element       | `<input ref={} />`       |
| Convert string to number  | `parseInt(number)`       |

---

# useState()

> `useState` is used to create and manage state in React components.
> When the state changes, the updated value is shown on the frontend.

---

# Example 1

```jsx
const App = () => {
    let n1 = 10;

    return <div>{n1}</div>;
}
```

## Output

```text
10
```

### Explanation

* During the first render:

  * `var`
  * `let`
  * `const`

values are rendered in the browser.

---

# Example 2

```jsx
const App = () => {
    let n1 = 10;

    const f1 = () => {
        n1 = 100;
        console.log(n1);
    }

    return (
        <div>
            <button onClick={f1}>Click</button>
        </div>
    );
}
```

```jsx
<App />
```

## Output

```text
100
```

### Explanation

* Value changes successfully
* Updated value appears in the console

---

# Example 3

```jsx
const App = () => {
    let n1 = 10;

    const f1 = () => {
        n1 = 100;
        console.log(n1);
    }

    return (
        <div>
            <h1>{n1}</h1>
            <button onClick={f1}>Click</button>
        </div>
    );
}
```

```jsx
<App />
```

---

## Output Analysis

### Initial Render

```text
10
```

### After Clicking Button

#### Console Output

```text
100
```

#### DOM Output

```text
10
```

---

## Important Point

> After the initial render, changing normal variables (`let`, `var`) does NOT update the DOM.

That is why React introduced **state**.

---

# Understanding the Concept Behind useState

## Custom Function Example

```javascript
function fn(init) {

    let no = init;

    const noChange = (newValue) => {
        no = newValue;
    }

    // return array
    // one variable + one function

    return [no, noChange];
}
```

---

# Function Output

## Example 1

```javascript
fn()
```

### Output

```javascript
[undefined, f]
```

---

## Example 2

```javascript
fn(100)
```

### Output

```javascript
[100, f]
```

---

# Array Destructuring

```javascript
const [x, y] = fn(100);
```

Equivalent to:

```javascript
const [x, y] = [100, f];
```

---

## Final Values

| Variable | Value    |
| -------- | -------- |
| `x`      | `100`    |
| `y`      | Function |

---

# Function Stored Inside y

```javascript
(newValue) => {
    no = newValue;
}
```

---

# Creating Our Own useState()

```javascript
function useState(init) {

    let val = init;

    const noChange = (newValue) => {
        val = newValue;
    }

    return [val, noChange];
}
```

---

# Usage

```javascript
const [a, b] = useState(100);
```

| Variable | Meaning                  |
| -------- | ------------------------ |
| `a`      | State value              |
| `b`      | Function to update value |

---

# Key Learning

## Normal Variables

```javascript
let a = 10;
```

* Changing value does NOT update UI

---

## State Variables

```javascript
const [a, setA] = useState(10);
```

* Changing value updates UI
* React re-renders component

---

# Quick Revision

| Topic                  | Key Point        |
| ---------------------- | ---------------- |
| `onClick={f1}`         | Correct          |
| `onClick={f1()}`       | Wrong            |
| `useRef()`             | No re-render     |
| `useState()`           | Causes re-render |
| Normal variable change | UI not updated   |
| State change           | UI updated       |
