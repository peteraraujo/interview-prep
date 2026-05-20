## Producer/Consumer Pattern

The Producer/Consumer pattern is a classic concurrent programming design that safely passes data between threads or processes running at different speeds. It decouples data production from data consumption by placing a shared buffer between them.

### The Three Components
- **Producer:** Thread or process that generates data, tasks, or events.
- **Buffer (Queue):** Shared, fixed-size First-In-First-Out (FIFO) data structure that temporarily holds items.
- **Consumer:** Thread or process that removes items from the buffer and processes them.

### The Restaurant Kitchen Analogy
- **Chefs (Producers)** cook meals.
- **Pass (Buffer)** is a heated shelf with limited capacity (e.g., exactly 10 plates).
- **Waiters (Consumers)** take plates from the pass and deliver them to tables.

Without the buffer, direct handoffs would fail whenever speeds differ.

### The Speed Mismatch Problem
The pattern handles two critical failure states:

- **Producer Too Fast (Buffer Overflow):** The buffer fills completely. The producer is blocked until the consumer removes an item and frees space.
- **Consumer Too Fast (Starvation):** The buffer empties. The consumer is blocked until the producer adds a new item.

### Why This Pattern Is Useful
- **Decoupling:** Producers and consumers have no direct knowledge of each other (e.g., a web scraper can enqueue HTML pages while a separate parser consumes them).
- **Load Leveling:** Sudden spikes (e.g., 10,000 user registrations in one second) are absorbed by the buffer, allowing slower consumers (e.g., a database) to process items at a steady, safe pace.