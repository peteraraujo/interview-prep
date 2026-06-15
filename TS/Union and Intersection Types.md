### TypeScript Union and Intersection Types: A Factual Overview

TypeScript provides specific operators to combine existing types into new, more complex structures. These are known as union and intersection types. They allow developers to define precise data constraints and object shapes without duplicating interface definitions.

Below is a breakdown of how these operators function and the rules governing their implementation.

---

### Union Types

A union type dictates that a value must match exactly one of several specified types. It is defined using the vertical bar (`|`) operator.

**Mechanism and Syntax**
When a variable is assigned a union type, the TypeScript compiler permits the assignment of any value that structurally matches at least one of the constituent types.

```typescript
type Identifier = string | number;

let userId: Identifier;
userId = "ID-12345"; // Valid
userId = 98765;      // Valid
// userId = true;    // Compiler Error: Type 'boolean' is not assignable to type 'Identifier'.

```

**Type Narrowing**
Because a union type represents multiple potential data formats, the TypeScript compiler restricts property and method access. By default, a script can only access properties or methods that are common to all types within the union.

To access methods specific to a single type within the union, the code must execute a runtime check (such as `typeof`, `instanceof`, or the `in` operator) to narrow the type. Once narrowed, the compiler safely permits type-specific operations.

```typescript
function processInput(input: string | number) {
  // input.toUpperCase() // Error: Property 'toUpperCase' does not exist on type 'number'.
  
  if (typeof input === "string") {
    // Within this execution block, the compiler strictly treats 'input' as a string
    return input.toUpperCase(); 
  } else {
    // Within this execution block, the compiler strictly treats 'input' as a number
    return input.toFixed(2);
  }
}

```

### Intersection Types

An intersection type dictates that a single value must satisfy multiple type definitions simultaneously. It is defined using the ampersand (`&`) operator.

**Mechanism and Syntax**
Intersection types are primarily utilized to merge multiple object interfaces into a single, comprehensive structure. The resulting type requires every property defined across all of the intersected types.

```typescript
interface ErrorHandling {
  success: boolean;
  error?: string;
}

interface DataPayload {
  records: { id: string; content: string }[];
}

// The resulting type requires the properties of both ErrorHandling and DataPayload
type NetworkResponse = ErrorHandling & DataPayload;

const validResponse: NetworkResponse = {
  success: true,
  records: [{ id: "1", content: "Stored text" }]
}; // Valid

// const invalidResponse: NetworkResponse = {
//   success: false
// }; // Compiler Error: Property 'records' is missing.

```

**Type Conflicts**
If an intersection combines primitive types that cannot logically coexist, the TypeScript compiler resolves the resulting type to `never`. No value can be assigned to a `never` type.

Similarly, if two intersected object interfaces share the exact same property key but assign conflicting types to that key, the compiler evaluates that specific property as `never`.

```typescript
interface ComponentA {
  id: string;
}

interface ComponentB {
  id: number;
}

type CombinedComponent = ComponentA & ComponentB;
// The 'id' property is evaluated as (string & number), resulting in 'never'.
// It is structurally impossible to instantiate CombinedComponent.

```