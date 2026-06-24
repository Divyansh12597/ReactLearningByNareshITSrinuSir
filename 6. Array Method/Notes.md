# 📚 JavaScript Array Methods Notes

> Array methods are built-in JavaScript functions used to add, remove, search, modify, and iterate over arrays.

---

# 1. push()

## Definition

The `push()` method adds one or more elements to the **end** of an array.

## Syntax

```javascript
array.push(element1, element2, ...);
```

## Returns

Returns the **new length** of the array.

## Example

```javascript
let fruits = ["Apple", "Banana"];

fruits.push("Mango");

console.log(fruits);
```

### Output

```javascript
["Apple", "Banana", "Mango"]
```

---

# 2. pop()

## Definition

The `pop()` method removes the **last element** from an array.

## Syntax

```javascript
array.pop();
```

## Returns

Returns the removed element.

## Example

```javascript
let fruits = ["Apple", "Banana", "Mango"];

let removed = fruits.pop();

console.log(removed);
console.log(fruits);
```

### Output

```javascript
Mango

["Apple", "Banana"]
```

---

# 3. shift()

## Definition

The `shift()` method removes the **first element** from an array.

## Syntax

```javascript
array.shift();
```

## Returns

Returns the removed element.

## Example

```javascript
let fruits = ["Apple", "Banana", "Mango"];

let removed = fruits.shift();

console.log(removed);
console.log(fruits);
```

### Output

```javascript
Apple

["Banana", "Mango"]
```

---

# 4. unshift()

## Definition

The `unshift()` method adds one or more elements to the **beginning** of an array.

## Syntax

```javascript
array.unshift(element1, element2, ...);
```

## Returns

Returns the new length of the array.

## Example

```javascript
let fruits = ["Banana", "Mango"];

fruits.unshift("Apple");

console.log(fruits);
```

### Output

```javascript
["Apple", "Banana", "Mango"]
```

---

# 5. indexOf()

## Definition

Returns the **index** of the first matching element.

If not found, it returns **-1**.

## Syntax

```javascript
array.indexOf(value);
```

## Example

```javascript
let fruits = ["Apple", "Banana", "Mango"];

console.log(fruits.indexOf("Banana"));
console.log(fruits.indexOf("Orange"));
```

### Output

```javascript
1

-1
```

---

# 6. includes()

## Definition

Checks whether an array contains a particular value.

## Syntax

```javascript
array.includes(value);
```

## Returns

Returns

- `true`
- `false`

## Example

```javascript
let fruits = ["Apple", "Banana", "Mango"];

console.log(fruits.includes("Apple"));
console.log(fruits.includes("Orange"));
```

### Output

```javascript
true

false
```

---

# 7. forEach()

## Definition

Loops through every element of an array.

Used when you want to perform an operation on each item.

**It does NOT return a new array.**

## Syntax

```javascript
array.forEach(function(value, index) {

});
```

## Example

```javascript
let numbers = [10, 20, 30];

numbers.forEach(function(item) {
    console.log(item);
});
```

### Output

```javascript
10
20
30
```

### Arrow Function

```javascript
numbers.forEach(item => console.log(item));
```

---

# 8. map()

## Definition

Creates a **new array** by modifying every element.

Original array remains unchanged.

## Syntax

```javascript
array.map(function(value, index) {

});
```

## Example

```javascript
let numbers = [1, 2, 3];

let result = numbers.map(function(item) {
    return item * 2;
});

console.log(result);
```

### Output

```javascript
[2, 4, 6]
```

### Arrow Function

```javascript
let result = numbers.map(item => item * 2);
```

---

# 9. filter()

## Definition

Creates a **new array** containing only the elements that satisfy the condition.

## Syntax

```javascript
array.filter(function(value) {

});
```

## Example

```javascript
let numbers = [10, 15, 20, 25, 30];

let result = numbers.filter(function(item) {
    return item >= 20;
});

console.log(result);
```

### Output

```javascript
[20, 25, 30]
```

### Arrow Function

```javascript
let result = numbers.filter(item => item >= 20);
```

---

# 10. sort()

## Definition

Sorts the elements of an array.

By default, it sorts values as **strings**.

## Syntax

```javascript
array.sort();
```

## Example (Strings)

```javascript
let fruits = ["Orange", "Apple", "Banana"];

fruits.sort();

console.log(fruits);
```

### Output

```javascript
["Apple", "Banana", "Orange"]
```

---

## Sorting Numbers (Ascending)

```javascript
let numbers = [20, 5, 100, 40];

numbers.sort((a, b) => a - b);

console.log(numbers);
```

### Output

```javascript
[5, 20, 40, 100]
```

---

## Sorting Numbers (Descending)

```javascript
let numbers = [20, 5, 100, 40];

numbers.sort((a, b) => b - a);

console.log(numbers);
```

### Output

```javascript
[100, 40, 20, 5]
```

---

# 📌 Difference Between forEach(), map(), and filter()

| Feature | forEach() | map() | filter() |
|----------|-----------|--------|-----------|
| Returns New Array | ❌ No | ✅ Yes | ✅ Yes |
| Modifies Original Array | ❌ No | ❌ No | ❌ No |
| Used For | Looping | Transforming Data | Filtering Data |

---

# 📌 Difference Between push(), pop(), shift(), and unshift()

| Method | Operation | Position |
|---------|-----------|-----------|
| push() | Add | End |
| pop() | Remove | End |
| shift() | Remove | Beginning |
| unshift() | Add | Beginning |

---

# 📌 Difference Between indexOf() and includes()

| Method | Returns |
|----------|----------|
| indexOf() | Index or -1 |
| includes() | true / false |

---

# 💡 Interview Questions

### Q1. Which methods modify the original array?

✅ Modifies Original Array

- push()
- pop()
- shift()
- unshift()
- sort()

❌ Does NOT Modify Original Array

- indexOf()
- includes()
- forEach()
- map()
- filter()

---

### Q2. Which methods return a new array?

- map()
- filter()

---

### Q3. Which method is used only for looping?

- forEach()

---

### Q4. Why should we use `sort((a, b) => a - b)` for numbers?

Because the default `sort()` converts values to strings and compares them lexicographically.

Example:

```javascript
[100, 5, 20].sort();
```

Output:

```javascript
[100, 20, 5]
```

Correct numeric sort:

```javascript
[100, 5, 20].sort((a, b) => a - b);
```

Output:

```javascript
[5, 20, 100]
```

---

# 📝 Quick Revision Table

| Method | Purpose | Returns |
|---------|---------|----------|
| push() | Add element at end | New length |
| pop() | Remove last element | Removed element |
| shift() | Remove first element | Removed element |
| unshift() | Add element at beginning | New length |
| indexOf() | Find index | Index / -1 |
| includes() | Check existence | true / false |
| forEach() | Loop through array | undefined |
| map() | Create modified array | New array |
| filter() | Create filtered array | New array |
| sort() | Sort array | Sorted array |

---

# 🎯 Memory Trick

```text
push      ➜ Add at End

pop       ➜ Remove from End

unshift   ➜ Add at Beginning

shift     ➜ Remove from Beginning

indexOf   ➜ Find Index

includes  ➜ Find True/False

forEach   ➜ Loop Only

map       ➜ Modify Every Item

filter    ➜ Keep Matching Items

sort      ➜ Arrange Items
```