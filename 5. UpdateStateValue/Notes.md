## Recording lecture No 31

# React State Updates (useState) - Summary Notes

## useState

```jsx
const [cnt, setCnt] = React.useState(0);
```

- `cnt` → Current state value.
- `setCnt()` → Function used to update the state.
- Calling `setCnt()` **does not immediately change** the state.

---

# React State is Asynchronous

```jsx
setCnt(cnt + 1);
console.log(cnt);
```

Output:

```text
0
```

**Reason:**

- `setCnt()` schedules a state update.
- React updates the state **after the current event handler finishes**.

---

# When does React Re-render?

React re-renders **after the event handler completes**.

Example:

```jsx
const fClick = () => {
    setCnt(cnt + 1);
    console.log("Inside Function");
};
```

Execution Flow:

```text
Button Click
      │
      ▼
Event Handler Starts
      │
      ▼
setCnt() called
      │
      ▼
Event Handler Ends
      │
      ▼
React updates state
      │
      ▼
React Re-renders Component
      │
      ▼
UI Updates
```

---

# React Batching (Automatic Batching)

React groups multiple state updates together and performs **only one render**.

Example:

```jsx
setCnt(...);
setCnt(...);
setCnt(...);
setCnt(...);
```

React performs:

```text
One Render
```

instead of:

```text
Render
Render
Render
Render
```

This improves performance.

---

# Why doesn't this work?

```jsx
const fClick = () => {
    for(let i = 0; i < 5; i++) {
        setCnt(cnt + 1);
    }
}
```

Assume:

```text
cnt = 0
```

JavaScript executes:

```text
setCnt(1)
setCnt(1)
setCnt(1)
setCnt(1)
setCnt(1)
```

because `cnt` remains `0` during the entire loop.

Final Result:

```text
cnt = 1
```

---

# Why does Functional Update work?

```jsx
const fClick = () => {
    for(let i = 0; i < 5; i++) {
        setCnt(val => val + 1);
    }
}
```

React stores the updater functions:

```text
+1
+1
+1
+1
+1
```

Later React processes them:

```text
Start = 0

0 → 1
1 → 2
2 → 3
3 → 4
4 → 5
```

Final Result:

```text
cnt = 5
```

---

# Difference

## Normal Update

```jsx
setCnt(cnt + 1);
```

JavaScript calculates immediately.

```text
cnt = 0

↓

0 + 1

↓

setCnt(1)
```

Multiple calls become:

```text
setCnt(1)
setCnt(1)
setCnt(1)
```

Final State:

```text
1
```

---

## Functional Update

```jsx
setCnt(prev => prev + 1);
```

JavaScript **does not execute the callback immediately**.

React stores the callback and executes it later using the **latest state**.

```text
Current State

↓

prev + 1

↓

Updated State

↓

Next Callback receives updated state
```

Final State:

```text
5
```

---

# What is `prev` (or `val`)?

```jsx
setCnt(prev => prev + 1);
```

`prev` is provided by React.

It represents the **latest state value** when React processes the update queue.

Example:

```text
Initial State = 0

Callback 1
prev = 0
Return 1

Callback 2
prev = 1
Return 2

Callback 3
prev = 2
Return 3
```

---

# React Update Queue

Every `setCnt()` call is stored in React's Update Queue.

Example:

```jsx
setCnt(prev => prev + 1);
setCnt(prev => prev + 1);
setCnt(prev => prev + 1);
```

Queue:

```text
+-----------------------+
| prev => prev + 1      |
| prev => prev + 1      |
| prev => prev + 1      |
+-----------------------+
```

After the event handler finishes:

React processes the queue.

Then React performs **one re-render**.

---

# Complete Flow

```text
Button Click
      │
      ▼
React calls Event Handler
      │
      ▼
for Loop Executes
      │
      ▼
setCnt() adds updates to Queue
      │
      ▼
Event Handler Finishes
      │
      ▼
React Processes Update Queue
      │
      ▼
State Updated
      │
      ▼
React Re-renders Component
      │
      ▼
Browser Updates UI
```

---

# Important Rule

✅ Use this when the new state depends on the previous state.

```jsx
setCnt(prev => prev + 1);
```

❌ Avoid this for multiple consecutive updates.

```jsx
setCnt(cnt + 1);
```

---




# JavaScript Timers (`setTimeout()` & `setInterval()`)

## What are Timers?

JavaScript normally executes code **line by line**.

Sometimes we want to:

- Execute code **after some delay**.
- Execute code **repeatedly after a fixed interval**.

JavaScript provides two built-in timer functions:

- `setTimeout()` → Executes **once** after a delay.
- `setInterval()` → Executes **repeatedly** after a fixed interval.

---

# 1. setTimeout()

## Definition

`setTimeout()` executes a function **only once** after a specified delay.

### Syntax

```javascript
setTimeout(function, delayInMilliseconds);
```

OR

```javascript
setTimeout(() => {
    // Code
}, 1000);
```

---

## Example

```javascript
console.log("Start");

setTimeout(() => {
    console.log("Hello");
}, 3000);

console.log("End");
```

### Output

```text
Start
End

(After 3 seconds)

Hello
```

### Execution Flow

```text
Start
   │
   ▼
setTimeout() Registered
   │
   ▼
End
   │
   ▼
Wait 3 Seconds
   │
   ▼
Hello
```

---

## Important Points

- Executes only **once**.
- Does **not block** JavaScript execution.
- Returns a **Timer ID**.
- Can be cancelled using `clearTimeout()`.

---

## Cancel setTimeout()

```javascript
const timer = setTimeout(() => {
    console.log("Hello");
}, 5000);

clearTimeout(timer);
```

Result:

```text
Hello will never be printed.
```

---

# 2. setInterval()

## Definition

`setInterval()` executes a function **repeatedly** after every specified interval until stopped.

### Syntax

```javascript
setInterval(function, delayInMilliseconds);
```

OR

```javascript
setInterval(() => {
    // Code
}, 1000);
```

---

## Example

```javascript
setInterval(() => {
    console.log("Hello");
}, 1000);
```

### Output

```text
1 sec → Hello
2 sec → Hello
3 sec → Hello
4 sec → Hello
...
```

It continues forever until stopped.

---

## Stop setInterval()

```javascript
const id = setInterval(() => {
    console.log("Hello");
}, 1000);

clearInterval(id);
```

---

## Example with Counter

```javascript
let count = 1;

const id = setInterval(() => {

    console.log(count);

    count++;

    if (count > 5) {
        clearInterval(id);
    }

}, 1000);
```

### Output

```text
1
2
3
4
5
```

After printing **5**, the interval stops.

---

# Difference Between setTimeout() and setInterval()

| setTimeout() | setInterval() |
|---------------|---------------|
| Executes only once | Executes repeatedly |
| Used for delayed execution | Used for repeated execution |
| Stops automatically after one execution | Continues until `clearInterval()` is called |
| Cancel using `clearTimeout()` | Cancel using `clearInterval()` |

---

# Browser Timer Timeline

## setTimeout()

```text
Time

0 sec
│
├── Code Starts
├── Timer Registered
├── Remaining Code Executes
│
│
│
3 sec
│
└── Callback Executes Once
```

---

## setInterval()

```text
Time

0 sec
│
├── Interval Registered
│
1 sec
├── Callback Executes
│
2 sec
├── Callback Executes
│
3 sec
├── Callback Executes
│
4 sec
├── Callback Executes
│
...
```

---

# Real Life Example

## setTimeout()

Think of setting an alarm **once**.

```text
Wake me up at 7:00 AM.

🔔 Rings Once
```

---

## setInterval()

Think of a school bell.

```text
Every 45 minutes

🔔 Bell Rings

🔔 Bell Rings

🔔 Bell Rings
```

It continues until the school ends.

---

# Using Timers in React

Example: Digital Clock

```jsx
function App() {

    const [time, setTime] = React.useState(new Date());

    React.useEffect(() => {

        const id = setInterval(() => {
            setTime(new Date());
        }, 1000);

        return () => clearInterval(id);

    }, []);

    return <h1>{time.toLocaleTimeString()}</h1>;
}
```

The component updates every second.

---

# Common Interview Questions

### Q1. What is the difference between `setTimeout()` and `setInterval()`?

**Answer:**

- `setTimeout()` executes the callback **only once** after a specified delay.
- `setInterval()` executes the callback **repeatedly** after every specified interval until stopped.

---

### Q2. How do you stop a timer?

For `setTimeout()`:

```javascript
clearTimeout(timerId);
```

For `setInterval()`:

```javascript
clearInterval(intervalId);
```

---

# Summary

## setTimeout()

- Executes once.
- Used for delayed execution.
- Returns a Timer ID.
- Cancel using `clearTimeout()`.

---

## setInterval()

- Executes repeatedly.
- Used for repeated tasks.
- Returns an Interval ID.
- Cancel using `clearInterval()`.

---

# Key Points to Remember

- Both are Browser Timer APIs.
- Time is specified in **milliseconds**.

```text
1000 ms = 1 second
2000 ms = 2 seconds
5000 ms = 5 seconds
```

- JavaScript does **not wait** for the timer to finish.
- The callback executes **after** the specified delay.
- Always clear intervals when they are no longer needed to avoid unnecessary memory usage.

---

# Keywords (Interview)

- Timer API
- Callback Function
- Delay
- Event Loop
- Timer Queue
- `setTimeout()`
- `setInterval()`
- `clearTimeout()`
- `clearInterval()`
- Asynchronous JavaScript
- Browser Timers
# Keywords (Interview)

- useState
- State
- State Setter Function
- Asynchronous State Update
- State Update Queue
- Functional State Update
- Previous State (`prev`)
- Automatic Batching (React 18)
- Re-render
- Event Handler
- JSX
- Virtual DOM