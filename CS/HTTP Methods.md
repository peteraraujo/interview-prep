## HTTP Methods

### Core Concepts
- **Safe**: Read-only. Does not change server state. Can be called repeatedly with no side effects.
- **Idempotent**: Repeating the exact same request produces the same result. Safe for retries (e.g., network failures).

| Method | CRUD     | Safe? | Idempotent? | Use Case                     |
|--------|----------|-------|-------------|------------------------------|
| GET    | Read     | Yes   | Yes         | Retrieve resource            |
| POST   | Create   | No    | No          | Create new resource          |
| PUT    | Update   | No    | Yes         | Replace entire resource      |
| PATCH  | Update   | No    | No (usually)| Partial update               |
| DELETE | Delete   | No    | Yes         | Remove resource              |

### Primary HTTP Methods

#### GET (Retrieve)
- Fetches a resource.
- Data sent via URL path or query params. **No request body**.
- Highly cacheable.
- Spring Boot: `@GetMapping`
- Example: `GET /users/123` or `GET /users?role=admin`

#### POST (Create)
- Creates a new resource or triggers a process.
- Data sent in **request body** (usually JSON).
- **Not idempotent** — repeated calls create duplicates.
- Spring Boot: `@PostMapping`
- Example: `POST /users` with `{"name": "Alice"}`

#### PUT (Replace)
- Completely replaces an existing resource.
- Must send full resource representation.
- **Idempotent** — repeated calls yield same final state.
- Spring Boot: `@PutMapping`
- Example: `PUT /users/123` with full user object

#### PATCH (Modify)
- Applies partial updates.
- Only send changed fields.
- Not strictly idempotent (depends on implementation).
- Spring Boot: `@PatchMapping`
- Example: `PATCH /users/123` with `{"email": "new@email.com"}`

#### DELETE (Remove)
- Deletes a resource.
- **Idempotent** — repeated calls have no additional effect.
- Spring Boot: `@DeleteMapping`
- Example: `DELETE /users/123`

### Secondary HTTP Methods
- **OPTIONS**: Returns supported methods for a URL. Critical for **CORS preflight** requests.
- **HEAD**: Like GET but returns only headers (no body). Used to check existence or file size.
- **TRACE**: Diagnostic loop-back (often disabled for security).

### Key Interview Takeaways
- **Idempotency** is critical for reliable retry logic in distributed systems.
- Use **POST** for creation, **PUT** for full replacement, **PATCH** for partial updates.
- **GET** must never cause side effects (strict safety).
- In Spring Boot, prefer `@RequestMapping` with method specification or the shortcut annotations (`@GetMapping`, etc.).
- Always design APIs with clear expectations around safety and idempotency for frontend and retry handling.

