## Promises in JavaScript

A Promise represents the eventual completion (or failure) of an asynchronous operation and its resulting value. It exists in one of three states: **pending**, **fulfilled** (success), or **rejected** (error).

### Instance Methods (Handling a Single Promise)
These are chained directly onto a Promise to handle its outcome.

- **`.then()`**  
  Executes when the Promise resolves successfully. It receives the resolved value.

  ```js
  fetch('https://api.example.com/data')
    .then(response => response.json())
    .then(data => console.log('Success:', data));
  ```

- **`.catch()`**  
  Executes if the Promise rejects or if any error is thrown in a preceding `.then()`.

  ```js
  fetch('https://api.example.com/data')
    .then(response => response.json())
    .catch(error => console.error('Failed to fetch:', error));
  ```

- **`.finally()`**  
  Executes regardless of fulfillment or rejection. Commonly used for cleanup (e.g., hiding a loading spinner).

  ```js
  let isLoading = true;

  fetch('https://api.example.com/data')
    .then(data => console.log(data))
    .catch(error => console.error(error))
    .finally(() => {
      isLoading = false;
      console.log('Operation finished.');
    });
  ```

### Static Methods (Handling Multiple Promises)
These methods operate on an array (or iterable) of Promises to manage concurrency.

- **`Promise.all()`** — "All or Nothing"  
  Waits for **all** Promises to fulfill. Rejects immediately if any one fails.  
  Best for dependent data where the entire operation must succeed.

  ```js
  Promise.all([fetch('/users'), fetch('/posts')])
    .then(([users, posts]) => console.log('Both loaded!'))
    .catch(error => console.error('One or both failed.'));
  ```

- **`Promise.allSettled()`** — "Report Card"  
  Waits for **all** Promises to settle (fulfilled or rejected). Returns an array describing every outcome.  
  Best for independent tasks where some failures are acceptable.

  ```js
  const promises = [fetch('/api/good'), fetch('/api/bad')];

  Promise.allSettled(promises)
    .then(results => {
      results.forEach(result => console.log(result.status)); // "fulfilled" or "rejected"
    });
  ```

  ##### Output
  ```js
    [
        { status: "fulfilled", value: 1 },
        { status: "rejected", reason: "error" },
        { status: "fulfilled", value: 3 }
    ]
  ```

- **`Promise.race()`** — "First to Finish"  
  Returns the result of the first Promise to settle (fulfill **or** reject). Others are ignored.  
  Best for implementing timeouts.

  ```js
  const fetchData = fetch('/slow-api');
  const timeout = new Promise((_, reject) => setTimeout(() => reject('Timeout!'), 5000));

  Promise.race([fetchData, timeout])
    .then(data => console.log('Data loaded fast enough!'))
    .catch(error => console.error(error));
  ```

- **`Promise.any()`** — "First Success"  
  Returns the first Promise that fulfills. Ignores rejections unless **all** Promises reject (throws `AggregateError`).  
  Best for pinging redundant services and using the first successful response.

  ```js
  Promise.any([
    fetch('https://us.api.com'),
    fetch('https://eu.api.com'),
    fetch('https://asia.api.com')
  ])
    .then(response => console.log('Got a successful response!'))
    .catch(error => console.error('All servers failed.'));
  ```

