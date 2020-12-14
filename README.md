# lru-ttl-cache
Super fast in memory LRU and TTL cache

## Installation
```
npm i -S lru-ttl-cache
```

## Why to use:
- Fast in memory cache
- Use both TTL (Time To Live) and LRU (Least Recently Used)
- Could set an optional memory-size for each element and set a maximum size (bytes) for the whole cache
- Do not use a "setTimeout" for each element witch decrease the performance
- Do not use "setInterval"
- Uses a “Map” instead of traditional “Object”. This increase performance regardless inserts and removes
- Could use any JavaScript type as a key (String, Numbers, BigInt, Objects, …)
- Optimized for V8 engine (used by nodeJS and other backend frameworks)

## Usage
```javascript
const LRU_TTL= require('lru-ttl-cache');

// Create the cache
const cache= new LRU_TTL(); // or: new LRU_TTL(options)

// Add value to cache
cache.set('key', mixedValue); // MixedValue could be any javascript type
cache.set(mixedKey, mixedValue); // could use any javascript variable as a key

// Could optionally add the mixedValue size in memory.
// This enables us to remove elements based on total used memory.
// Useful when dealing with text content or files
cache.set(mixedKey, mixedValue, bytes);

// Get element from cache
cache.get('key');
cache.get(mixedKey);

// Remove least used element
var element= cache.pop();

/**
 * Get value from the cache, or create one if not found
 * @return value if options.upsert is a Sync function
 * @return Promise if options.upsert is an Async function
 */
 // if options.upsert is a Sync function
var value= myCache.upsert(key);
// if options.upsert is an Async function
var value= await myCache.upsert(key);

// Remove element from the cache
cache.delete('key');
cache.delete(mixedKey);

// Remove all elements
cache.clear();

// Check if the cache has a key
<Boolean> cache.has('key');
<Boolean> cache.has(mixedKey);

// Get the size of the cache
var entriesCount= cache.size;

// Get estimated cache size in bytes
var bytes= cache.bytes

// Get all current kies
<Iterator(key)> cache.keys();

// Get all values
<Iterator(value)> cache.values();

// Get all entries
<Iterator([key, value])> cache.entries();

// ForEach
cache.forEach(function callback(value, key){});

// For ... of
for([key, value] of cache){
	/* Your logic */
}

// Change cache configuration
cache.setConfig(options);
```

## Options:

```javascript
const options= {
	/**
	 * Set the maximum entries in the cache for the LRU algorithm
	 * remove the least used element when this is exceeded
	 * Set to "0" to disable cache (Example: for development mode)
	 * @default  Infinity
	 */
	max: Infinity,
	/**
	 * Set the TTL (ms) of each element
	 * Removes the element if the ttl is exceeded
	 * The TTL is reset when getting ( via cache.get )
	 * or updating an element ( via cache.set )
	 * @default Infinity
	 */
	ttl: Infinity,
	/**
	 * Set the element as the last used when calling the cache.get method
	 * @default true
	 */
	refreshOnGet: true,
	/**
	 * Maximum cache size in bytes
	 * to use this, you need to add the size of each entry
	 * via catch.set(key, value, bytes)
	 * @type {Number}
	 */
	maxBytes: Infinity,
	/**
	 * Create item if not exists when using cache.upsert(key)
	 * Could be function or async function
	 * @type {Function, AsyncFunction}
	 */
	 upsert: function(key){ /*Logic*/ }
};
```

## Use as TTL cache (Time to live)
```javascript
const LRU_TTL= require('lru-ttl-cache');

const cache= new LRU_TTL({
	/**
	 * Set the TTL (time to live in milliseconds)
	 * Setting this to "Infinity" will disable the TTL behaviour of the cache
	 * @type Positive integer
	 */
	ttl: 3600000,
	/**
	 * Set this to false if you prefer not to refresh element
	 * when getting it. @default true
	 * @type Boolean
	 */
	refreshOnGet: true
});
```

## Use as LRU cache (Least recently used)
```javascript
const LRU_TTL= require('lru-ttl-cache');

const cache= new LRU_TTL({
	/**
	 * Set the maximum items count in the cache.
	 * When this value exceeded, the least used item will be removed
	 * Setting this to "Infinity" will disable the LRU behaviour of the cache
	 * @type Number
	 * @default Infinity
	 */
	max: 200,
	/**
	 * Set this to false if you prefer not to refresh element
	 * when getting it. @default true
	 * @type Boolean
	 */
	refreshOnGet: true // Set this to false if you prefer not to refresh element when getting it. @default true
});
```

## Use as LRU cache with max bytes
```javascript
const LRU_TTL= require('lru-ttl-cache');

const cache= new LRU_TTL({
	/**
	 * Set the maximum memory size (in bytes) for the cache
	 * Will remove the least used elements if this value is exceeded
	 * Setting this to "Infinity" will disable this behaviour
	 * @type Number
	 * @default Infinity
	 */
	maxBytes: 2**20, // 1MB
});

// MUST set a size of each element when insering or updating it
// otherwise the size will be null
cache.set('key', value, bytes);

```

## Use cache with multiple behaviours

Just set the required configuration as options.

# Credits

Khalid RAFIK

# Let's contribute
Open an issue or contact me: khalid.rfk@gmail.com

# Support
Open an issue or contact me: khalid.rfk@gmail.com

# Licence

MIT License

Copyright (c) 2020 rafikalid

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.
