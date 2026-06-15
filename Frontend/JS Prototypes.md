### JavaScript Prototype System

JavaScript utilizes a prototype system to share properties and methods between objects. Rather than copying methods to every new instance, objects maintain a hidden link to another object. This linked object is called the prototype.

---

### The Internal Prototype Reference

Every JavaScript object contains a hidden internal property, designated in the language specification as `[[Prototype]]`. This internal property holds a direct reference to another object, or to `null`.

When a script attempts to read a property on an object, the JavaScript engine follows a strict sequence:

1. It checks if the requested property exists directly on the target object.
2. If the property is missing, the engine inspects the object referenced by the `[[Prototype]]`.
3. If the property is found on the prototype object, it is executed or returned.
4. If the property is missing on the prototype, the engine inspects *that* object's prototype.

This sequential lookup process is known as the **prototype chain**. The traversal continues until the engine finds the property or reaches an object whose `[[Prototype]]` is `null`. If `null` is reached, the lookup returns `undefined`.

---

### Establishing the Prototype Chain: Three Methods

There are three primary ways developers establish prototype links between objects in JavaScript.

#### 1. Direct Assignment with `Object.create()`

The most explicit way to construct a prototype chain is using `Object.create()`. This method creates a new, empty object and explicitly sets its `[[Prototype]]` to an existing object provided as an argument.

```javascript
const sharedBehaviors = {
  calculateTotal: function(amount) {
    return amount * 1.05;
  }
};

// Create a new object with 'sharedBehaviors' as its prototype
const order = Object.create(sharedBehaviors);
order.id = 1001;

console.log(order.id); // 1001 (found on the object itself)
console.log(order.calculateTotal(50)); // 52.5 (found on the prototype)

```

In this execution, `order` does not possess the `calculateTotal` method. The JavaScript engine delegates the execution to `sharedBehaviors`.

#### 2. Constructor Functions

Prior to the introduction of modern class syntax, functions were used alongside the `new` keyword to generate objects with shared prototypes. Every function in JavaScript automatically possesses a property named `.prototype`.

When a function is invoked with the `new` keyword, the engine creates a new object and automatically sets that new object's `[[Prototype]]` to the constructor function's `.prototype` object.

```javascript
function User(username) {
  // Properties unique to the specific object
  this.username = username; 
}

// Methods attached to the shared prototype
User.prototype.getUsername = function() {
  return this.username;
};

const firstUser = new User("admin_system");
const secondUser = new User("guest_viewer");

console.log(firstUser.getUsername()); // "admin_system"

```

Here, `firstUser` and `secondUser` are distinct objects in memory. However, they both contain a `[[Prototype]]` reference pointing to `User.prototype`. The `getUsername` function exists only once in system memory, shared by both objects.

#### 3. Modern Class Syntax

The `class` keyword provides an alternative syntax for defining objects and their prototypes. It does not introduce a new object-creation model; it relies entirely on the existing prototype chain under the surface.

```javascript
class Server {
  constructor(ipAddress) {
    this.ipAddress = ipAddress;
  }

  // This method is automatically assigned to Server.prototype
  ping() {
    return `Pinging ${this.ipAddress}`;
  }
}

const mainServer = new Server("192.168.1.1");
console.log(mainServer.ping()); // "Pinging 192.168.1.1"

```

The result is structurally identical to the constructor function method. The `ping` method is stored on the prototype, not on the `mainServer` object itself.

---

### Dynamic Modification

Because the prototype system operates via active memory references rather than static copying, modifying a prototype object immediately impacts all objects linked to it.

```javascript
class Database {}
const dbInstance = new Database();

// Modifying the prototype after the object is instantiated
Database.prototype.connect = function() {
  return "Connection established.";
};

// The existing instance immediately has access to the new method
console.log(dbInstance.connect()); // "Connection established."

```

When `dbInstance.connect()` is called, the JavaScript engine traverses the chain, finds the newly added method on `Database.prototype`, and executes it. This dynamic property sharing is the defining characteristic of the JavaScript prototype system.