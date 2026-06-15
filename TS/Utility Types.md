### Utility Types
TypeScript provides a set of globally available utility types designed to facilitate common type transformations. These utilities operate as generic types; they accept an existing type as an argument and construct a new type based on a specific, predefined rule.

Below is a breakdown of the primary utility types used to manipulate data structures in TypeScript.

---

### Modifying Optionality

These utilities alter whether the properties within an interface or object type are strictly required or optional.

#### `Partial<Type>`

Constructs a type where all properties of the given `Type` are set to optional (appending the `?` modifier). This is typically used when defining the payload for an update operation where a user might only submit a subset of the total fields.

```typescript
interface User {
  id: number;
  name: string;
  email: string;
}

// All properties become optional: { id?: number; name?: string; email?: string; }
type UserUpdatePayload = Partial<User>;

const update: UserUpdatePayload = {
  name: "New Name" // Valid, id and email are not required
};

```

#### `Required<Type>`

The strict opposite of `Partial`. It constructs a type where all properties of the given `Type` are set to required, explicitly removing any `?` modifiers present in the original definition.

```typescript
interface Configuration {
  host?: string;
  port?: number;
}

// All properties become required: { host: string; port: number; }
type StrictConfiguration = Required<Configuration>;

```

### Modifying Mutability

#### `Readonly<Type>`

Constructs a type where all properties of the given `Type` are set to readonly. Once an object of this type is initialized, the TypeScript compiler will throw an error if any code attempts to reassign the value of its properties.

```typescript
interface DatabaseConfig {
  connectionString: string;
}

type ImmutableConfig = Readonly<DatabaseConfig>;

const config: ImmutableConfig = { connectionString: "postgres://..." };
// config.connectionString = "new-string"; // Compiler Error

```

### Structural Selection

These utilities construct new types by extracting or stripping specific properties from an existing, larger type.

#### `Pick<Type, Keys>`

Constructs a type by explicitly selecting a set of properties (`Keys`) from an existing `Type`. `Keys` must be a string literal or a union of string literals representing valid keys of the original type.

```typescript
interface Product {
  id: string;
  title: string;
  description: string;
  price: number;
}

// Constructs a type with only title and price
type ProductPreview = Pick<Product, "title" | "price">;

const preview: ProductPreview = {
  title: "Laptop",
  price: 999
};

```

#### `Omit<Type, Keys>`

Constructs a type by explicitly removing a set of properties (`Keys`) from an existing `Type`. It retains all other properties.

```typescript
interface Employee {
  id: string;
  name: string;
  socialSecurityNumber: string;
  department: string;
}

// Constructs a type excluding the sensitive data
type PublicEmployeeProfile = Omit<Employee, "socialSecurityNumber">;

const profile: PublicEmployeeProfile = {
  id: "E-100",
  name: "Jane Doe",
  department: "Engineering"
};

```

### Object Construction

#### `Record<Keys, Type>`

Constructs an object type whose property keys are defined by `Keys` and whose property values are defined by `Type`. This is the standard utility for defining strictly typed dictionaries or mapping a specific set of keys to a specific data structure.

```typescript
type Environment = "development" | "staging" | "production";

interface ServerConfig {
  url: string;
  timeout: number;
}

// Enforces that every environment has a defined configuration
type EnvironmentMap = Record<Environment, ServerConfig>;

const servers: EnvironmentMap = {
  development: { url: "localhost", timeout: 5000 },
  staging: { url: "staging.api.com", timeout: 3000 },
  production: { url: "api.com", timeout: 1000 }
};

```