## JavaScript Interview Pitfalls

JavaScript interviews are notoriously tricky because interviewers love to test you on the language's historical quirks and silent failures. They rarely test your ability to build a feature; instead, they test your understanding of how the JavaScript engine actually parses and schedules code.

Here are the most common pitfalls you will encounter in a technical interview, why they happen, and how to answer them.

### 1. The Loop Closure Trap (`var` vs `let`)
**Pitfall:**  
```js
for (var i = 0; i < 3; i++) {
  setTimeout(() => console.log(i), 100);
}
```
You expect `0, 1, 2`. It prints `3, 3, 3`.

**Explanation:**  
`var` is function-scoped, so there is only **one** `i` variable shared across the entire loop. By the time the `setTimeout` callbacks fire, the loop has already finished and `i` is `3`.

**Fix:**  
Use `let` (block-scoped). The engine creates a brand-new `i` for every iteration.

### 2. The Event Loop Execution Order
**Pitfall:**  
```js
console.log('A');

setTimeout(() => console.log('B'), 0);

Promise.resolve().then(() => console.log('C'));

console.log('D');
```
You expect `A → D → B → C`. It actually prints `A → D → C → B`.

**Explanation:**  
- **Call Stack** (synchronous): `A` and `D`  
- **Microtask Queue** (Promises): `C` (runs before any macrotask)  
- **Macrotask Queue** (`setTimeout`): `B` (runs last)

### 3. Hoisting and the Temporal Dead Zone
**Pitfall:**  
```js
console.log(name);
var name = "Alice";

console.log(age);
let age = 30;
```

**Explanation:**  
- `var` → prints `undefined` (declaration is hoisted, assignment is not).  
- `let`/`const` → throws `ReferenceError` (Temporal Dead Zone).

### 4. Object References vs. Reassignment
**Pitfall:**  
```js
function modifyUser(userObj) {
  userObj.age = 25;
  userObj = { name: "Bob", age: 50 };
}

const myUser = { name: "Alice", age: 20 };
modifyUser(myUser);
console.log(myUser);
```

**Explanation:**  
Output is `{ name: "Alice", age: 25 }`.  
- Property mutation follows the reference.  
- Reassignment only changes the local parameter pointer.

### 5. Type Coercion Oddities
**Common surprises:**
- `typeof null === 'object'` (historical bug, unfixable).  
- `NaN === NaN` → `false` (use `Number.isNaN()`).  
- `"5" + 3` → `"53"` (concatenation).  
- `"5" - 3` → `2` (coerces to number).

**Best answer:**  
Always use strict equality (`===`) and consider TypeScript for compile-time safety.

### 6. The `parseInt` Map Trap
**Pitfall:**  
```js
const numbers = ['1', '7', '11'];
const parsed = numbers.map(parseInt);
console.log(parsed); // [1, NaN, 3]
```

**Explanation:**  
`map` passes `(value, index, array)`. `parseInt` receives the index as the radix.

**Fix:**  
```js
numbers.map(num => parseInt(num, 10));
```

### 7. Automatic Semicolon Insertion (ASI)
**Pitfall:**  
```js
function getUser() {
  return 
  {
    name: "Alice"
  };
}
```

**Explanation:**  
The line break after `return` causes the engine to insert a semicolon → returns `undefined`.

**Fix:**  
Always keep the opening brace on the same line as `return`.

### 8. Structural vs. Referential Equality
**Pitfall:**  
```js
const a = [1, 2, 3];
const b = [1, 2, 3];
console.log(a === b); // false
```

**Explanation:**  
Objects/arrays are compared by reference (memory address), not content.

**Fix:**  
Use a deep equality check (custom function or `lodash.isEqual`).

### 9. The Array `typeof` Bug
**Pitfall:**  
```js
typeof [] === 'object' // true
```

**Explanation:**  
Arrays are objects with a special `.length` property.

**Fix:**  
Always use `Array.isArray(data)`.

### 10. The Floating Point Math Illusion
**Pitfall:**  
```js
console.log(0.1 + 0.2 === 0.3); // false
```

**Explanation:**  
JavaScript uses IEEE 754 double-precision floats. `0.1 + 0.2` actually equals `0.30000000000000004`.

**Fix (for money):**  
Work in integers (cents) and divide only at the final step.

These 10 pitfalls cover the vast majority of “gotcha” questions in modern JavaScript interviews. Master the explanations above and you will stand out immediately.

