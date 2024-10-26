# Array

1.  Basic Initialization

    ```js
    let arr = Array(5).fill(0); // [0, 0, 0, 0, 0]
    let gameState = Array(9).fill(""); // ["", "", "", "", "", "", "", "", ""]
    let arr = Array(5)
      .fill()
      .map((_, i) => i + 1); // [1, 2, 3, 4, 5]
    let randomArr = Array(5)
      .fill()
      .map(() => Math.floor(Math.random() * 10)); // e.g., [2, 7, 1, 9, 4]
    let matrix = Array(3)
      .fill()
      .map(() => Array(3).fill(0));
    // [
    //   [0, 0, 0],
    //   [0, 0, 0],
    //   [0, 0, 0]
    // ]

    let arr = Array.from({ length: 5 }, (_, i) => i); // [0, 1, 2, 3, 4]
    let evenArr = Array.from({ length: 5 }, (_, i) => i * 2); // [0, 2, 4, 6, 8]

    // Gotcha
    // let matrix = Array(3).fill().map(() => Array(3).fill(0)); VS let matrix = Array(3).fill(Array(3).fill(0));
    // 1st is correct , Problem with 2nd is all three rows will reference the same array.

    let arr = Array(3);
    console.log(arr); // Output: [ <3 empty items> ] => can’t directly use many array methods like .map() or .forEach() on them.

    let arr = Array(3).fill(0);
    console.log(arr); // Output: [0, 0, 0]

    // Nullish coalescing operator (??)
    const foo = null ?? "default string";
    console.log(foo); // Expected output: "default string"
    const baz = 0 ?? 42;
    console.log(baz); // Expected output: 0
    ```

# Set

1. Set - Base

   ```js
   const st1 = new Set([1, 2, 3]);
   const st1 = new Set();

   st1.add(num);
   st1.delete(num); // true/false
   st1.has(num); // true/false
   st1.size;

   // Using .keys()
   for (const key of st1.keys()) {
     console.log("Key:", key);
   } // Key: 1 Key: 2 Key: 3

   // Using .values()
   for (const key of st1.values()) {
     console.log("Value:", key);
   } // Value: 1 Value: 2 Value: 3

   // $$$$$$$ keys and values returns same i.e values $$$$$$$$$

   // Using .entries()
   for (const entry of st1.entries()) {
     // entry => [key, value]
     console.log("Entry:", entry);
   } // Entry: [1, 1] Entry: [2, 2] Entry: [3, 3]

   st1.forEach((value, valueAgain, set) => {
     // valueAgain => key for Map and index for Array
     // value: current value
   });
   ```

2. Set Iterators

   ```js
   // Using .keys() - returns an iterator with the values
   const keysIterator = st1.keys();
   console.log(keysIterator.next().value); // 1
   console.log(keysIterator.next().value); // 2
   console.log(keysIterator.next().value); // 3

   // Using .values() - returns an iterator with the values
   const valuesIterator = st1.values();
   console.log(valuesIterator.next().value); // 1
   console.log(valuesIterator.next().value); // 2

   // Using .entries() - returns an iterator with [key, value] pairs
   const entriesIterator = st1.entries();
   console.log(entriesIterator.next().value); // [1, 1]
   console.log(entriesIterator.next().value); // [2, 2]
   ```

3. Set <=> Array

```js
const myArr = Array.from(mySet1); // Set => Array
const myArr = [...mySet2]; // Set => Array

const mySet2 = new Set([1, 2, 3, 4]); // Array =>  Set
```

# Map

1. Basic

   ```js
   const map1 = new Map();

   map1.set("a", 1);
   map1.get("a"); // Returns undefined if 'a' is not there in the map1
   map1.delete("b");
   map1.size;
   map1.clear();

   // $$$$$$$ wrongMap
   const wrongMap = new Map();
   wrongMap["bla"] = "blaa";
   wrongMap["bla2"] = "blaaa2";

   console.log(wrongMap); // Map { bla: 'blaa', bla2: 'blaaa2' }
   wrongMap.has("bla"); // false
   wrongMap.delete("bla"); // false
   ```

2. Methods

   ```js
   const myMap = new Map();
   // values(), keys(), entries(), forEach()

   // $$$$$$$ No direct forEach on Object $$$$$$$$$
   ```

3. Map <=> Array

   ```js
   const kvArray = [
     ["key1", "value1"],
     ["key2", "value2"],
   ];
   const myMap = new Map(kvArray);

   const kvArray = Array.from(myMap); // Same as kvArray
   const kvArray = [...myMap]; // Same as kvArray

   Array.from(myMap.keys()); // ["key1", "key2"]

   const first = new Map([
     [1, "one"],
     [2, "two"],
     [3, "three"],
   ]);
   const second = new Map([
     [1, "uno"],
     [2, "dos"],
   ]);
   const merged = new Map([...first, ...second]);
   ```

4. WeakMap & WeakSet
   A WeakSet is a collection of garbage-collectable values, including objects and non-registered symbols.
   A WeakMap is a collection of key/value pairs whose keys must be objects or non-registered symbols

   ```js
   const ws = new WeakSet();
   const foo = {};
   const bar = {};
   ws.add(foo);
   ws.add(bar);
   ws.has(foo); // true
   ws.has(bar); // true

   const wm1 = new WeakMap();
   const o1 = {};
   const o2 = function () {};
   wm1.set(o1, 37);
   wm1.set(o2, "azerty");
   ```

# Object

1. Object vs Map
   Map -> A Map is safe to use with user-provided keys and values.
   Object -> Setting user-provided key-value pairs on an Object may allow an attacker to override the object's prototype

   Map -> A Map's keys can be any value (including functions, objects, or any primitive).
   Object -> The keys of an Object must be either a String or a Symbol.

   Map -> Performs better in scenarios involving frequent additions and removals of key-value pairs.
   Object -> Not optimized for frequent additions and removals of key-value pairs.

   ```js
   const obj = { a: 1, b: 2, c: 3 };

   for (const key in obj) {
     if (obj.hasOwnProperty(key)) {
       // Check if the property is a direct property of the object
       console.log(`Key: ${key}, Value: ${obj[key]}`);
     }
   }

   for (const key of Object.keys(obj)) {
     console.log("key:", key);
   } // key: 1 key: 2 key: 3

   for (const value of Object.values(obj)) {
     console.log("key:", key);
   } // Value: 1 Value: 2 Value: 3

   for (const [key, value] of Object.entries(obj)) {
   }
   ```
