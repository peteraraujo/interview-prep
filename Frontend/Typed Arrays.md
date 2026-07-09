### JavaScript Typed Arrays

**Architecture**
Typed Arrays are constructed using two separate objects:

* **ArrayBuffer:** A generic object representing a fixed-length contiguous block of raw binary data in memory. An `ArrayBuffer` cannot be read or modified directly.
* **View:** An object that provides a specific numeric data type context (such as 8-bit integers or 32-bit floats) to translate, read, and write the raw binary data stored within the `ArrayBuffer`.

**Standard Array vs. Typed Array**

* **Data Types:** Standard arrays can store mixed data types (strings, numbers, objects) simultaneously. Typed arrays strictly enforce a single numeric data type for all elements.
* **Memory Allocation:** Standard arrays dynamically resize. Typed arrays have a fixed length determined exactly at the time of creation. They lack modification methods such as `push()`, `pop()`, or `splice()`.
* **Performance:** Typed arrays guarantee contiguous memory allocation. This structure is required by browser APIs that process raw binary data, including WebGL, Web Audio, and WebSocket binary payloads.

**Common View Types**
Views dictate exactly how the raw bytes in the buffer are mathematically interpreted.

* **`Int8Array` / `Uint8Array**`: 8-bit signed and unsigned integers. `Uint8Array` is the standard format for manipulating image pixel data or raw network streams.
* **`Int16Array` / `Uint16Array**`: 16-bit signed and unsigned integers.
* **`Int32Array` / `Uint32Array**`: 32-bit signed and unsigned integers.
* **`Float32Array`**: 32-bit floating-point numbers. This is the mandatory format for passing vertex and color data to the WebGL rendering engine.
* **`Float64Array`**: 64-bit floating-point numbers.
* **`BigInt64Array` / `BigUint64Array**`: 64-bit signed and unsigned integers, utilizing the JavaScript `BigInt` primitive to represent numbers larger than the standard integer limit.

**DataView**
`DataView` is a specialized, low-level interface for `ArrayBuffers`. While standard views (like `Int32Array`) enforce a single data type across the entire buffer using the local computer's native endianness, `DataView` allows a developer to read and write multiple different number types at arbitrary byte offsets within the same buffer while explicitly controlling the endianness (byte order). This is strictly necessary when decoding complex binary files or network protocols built on different hardware architectures.