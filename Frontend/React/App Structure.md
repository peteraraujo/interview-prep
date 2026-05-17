## Structuring a Frontend Application



Structuring a frontend application effectively is critical for scalability, maintainability, and team collaboration. A well-organized codebase reduces cognitive load, isolates bugs, and prevents tight coupling between modules.

While specific frameworks influence some decisions, modern component-based applications generally follow a standard set of architectural patterns.







### Architectural Approaches



Historically, frontend applications were structured by file type (for example, placing all UI components in one folder, all network requests in another, and all state logic in a third). This works for small projects but becomes difficult to navigate as the codebase grows.

The modern industry standard is **Feature-Driven** (or Domain-Driven) Architecture. In this approach, files are grouped by the business feature they belong to (for example, authentication, user profile, or checkout). This encapsulates all logic, UI, and state for a specific feature into a single, modular directory.



### A Standard Feature-Driven Folder Structure



A robust and scalable application typically starts with a `src` directory containing the following structure:



```plaintext
src/
├── assets/         # Static files (images, fonts, global CSS)
├── components/     # Shared, reusable UI components (Buttons, Modals, Inputs)
├── config/         # Environment variables and global configurations
├── features/       # Feature-based modules (The core of the app)
├── hooks/          # Shared, generic custom hooks (e.g., useWindowSize, useDebounce)
├── layouts/        # Page layout wrappers (e.g., DashboardLayout, AuthLayout)
├── pages/          # Route-level components mapping to URLs
├── services/       # Global API clients and network interceptors
├── store/          # Global client-state setup (Redux, Zustand)
├── utils/          # Pure helper functions and formatters
└── App.js          # Root application component
```







### Deep Dive into Core Directories



#### 1\. The features/ Directory



This directory contains the bulk of the application's complexity. Each folder inside `features/` represents a specific domain of the application.



For example, a `features/auth/` directory might contain:



* `/components`: Login and Registration forms specific to authentication
* `/api`: API calls related strictly to authentication (e.g., `loginUser`, `resetPassword`)
* `/hooks`: Custom hooks managing authentication state
* `/utils`: Helpers for validating JWT tokens



By keeping these files co-located, a feature can be developed, tested, and updated in isolation without affecting the rest of the application.







#### 2\. The components/ Directory



Unlike feature-specific components, this global directory is strictly for **dumb**, reusable UI elements. These components contain no business logic and are not tied to any specific domain. They receive data via props and emit events via callbacks. Examples include `PrimaryButton`, `Card`, `Tooltip`, and `Spinner`.







#### 3\. The pages/ (or routes/) Directory



Files in this directory represent the actual views associated with a specific URL route (for example, `/login`, `/dashboard`). Page components act as orchestrators: they contain minimal UI code themselves and instead import complex components from the `features/` directory, fetch necessary data, and pass that data down to the UI.







#### 4\. The services/ (or api/) Directory

This layer isolates the application from the backend. It typically contains:

* The base HTTP client setup (for example, configuring Axios with base URLs)
* Request/response interceptors (for example, automatically attaching authorization headers)
* Global error handling logic for network failures







### Managing State and Data Flow



A well-structured frontend also enforces a strict separation of state:



* **Server State**: Handled within the `features/` directory using data-fetching libraries (such as TanStack Query). Caching and fetching logic stays close to the feature that requires it.
* **Global Client State**: Handled in the `store/` directory. This is reserved strictly for state that must be accessed globally across completely disparate features (for example, a dark mode toggle or an active user session).
* **Local UI State**: Handled directly within individual components using standard state hooks.



By enforcing these boundaries, the application remains modular. If a specific feature needs to be refactored or removed, developers can simply delete the corresponding folder in the `features/` directory without breaking the broader application ecosystem.

