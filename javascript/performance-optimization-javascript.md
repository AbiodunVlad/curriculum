# JavaScript Performance Optimization Topic Outline

⚠️ **_This topic outline is intended to be a comprehensive self-guided workshop. You may come back to the various activities over the course of a week. See respective time estimates for each [activity](#activities) below._** ⚠️ 

## Prerequisites

- [JavaScript Fundamentals](https://github.com/Techtonica/curriculum/blob/main/javascript/javascript-1-variables.md)
- [JavaScript Functions](https://github.com/Techtonica/curriculum/blob/main/javascript/javascript-2-functions.md)
- [JavaScript Arrays, Objects and Loops](https://github.com/Techtonica/curriculum/blob/main/javascript/javascript-3-arrays-objects-loops.md)
- [Runtime Complexity](https://github.com/Techtonica/curriculum/blob/main/runtime-complexity/runtime-complexity.md)
- [Web APIs](https://github.com/Techtonica/curriculum/blob/main/web-apis/web-apis.md)


## Table of Contents

- [Objectives](#objectives)
    - [Motivation and Real-World Application](#motivation-and-real-world-application)
    - [Specific Things to Learn](#specific-things-to-learn)
- [Lesson](#lesson)
    - [Understanding JavaScript Performance](#understanding-javascript-performance)
    - [Measuring Performance](#measuring-performance)
    - [Memory Management](#memory-management)
    - [Optimizing DOM Operations](#optimizing-dom-operations)
    - [Efficient Data Structures and Algorithms](#efficient-data-structures-and-algorithms)
    - [Asynchronous JavaScript Optimization](#asynchronous-javascript-optimization)
    - [Network Optimization](#network-optimization)
- [Activities](#activities)
- [Common Mistakes / Misconceptions](#common-mistakes--misconceptions)


## Objectives

By the end of this topic, participants should be able to:

- Identify performance bottlenecks in JavaScript applications
- Use browser developer tools to measure and analyze performance
- Implement techniques to optimize JavaScript code execution
- Apply best practices for DOM manipulation and rendering
- Optimize memory usage and prevent memory leaks
- Improve network performance for JavaScript applications
- Make data-informed decisions about performance trade-offs


## Motivation and Real-World Application

Performance optimization is a critical skill for software engineers, especially in web development. Slow websites and applications lead to poor user experience, reduced engagement, and lost revenue. According to studies, 53% of mobile users abandon sites that take longer than 3 seconds to load, and conversion rates drop by 7% for every additional second of load time.

As a software engineer, your ability to create performant applications will:

- Improve user satisfaction and retention
- Increase conversion rates for business applications
- Reduce server costs and infrastructure requirements
- Make applications more accessible to users with slower devices or connections
- Give you a competitive edge in the job market, as performance optimization is a highly valued skill


Real-world examples include how:

- Pinterest reduced their wait time by 40% and increased both search engine traffic and sign-ups by 15%
- The Financial Times found that a 1-second delay in page load time led to a 4.9% drop in article views
- Walmart saw a 2% conversion increase for every 1 second of improvement in page load


## Specific Things to Learn

- JavaScript engine internals and execution context
- Performance measurement tools and metrics
- Memory management and garbage collection
- DOM performance optimization techniques
- Efficient data structures and algorithms in JavaScript
- Asynchronous programming optimization
- Network request optimization
- JavaScript bundling and loading strategies
- Web Workers and multi-threading
- Rendering performance optimization


## Lesson

### Understanding JavaScript Performance

#### How JavaScript Engines Work

JavaScript engines (like V8 in Chrome) execute code through several phases:

```javascript
// This code goes through these phases:
// 1. Parsing: The code is parsed into an Abstract Syntax Tree (AST)
// 2. Compilation: The AST is compiled to bytecode
// 3. Optimization: Hot code paths are optimized with Just-In-Time (JIT) compilation
// 4. Execution: The code runs

function calculateTotal(items) {
  return items.reduce((sum, item) => sum + item.price, 0);
}

const cart = [
  { name: "Laptop", price: 999 },
  { name: "Headphones", price: 99 },
  { name: "Mouse", price: 29 }
];

console.log(calculateTotal(cart)); // 1127
```

#### The Call Stack and Memory Heap

JavaScript uses a call stack to track execution and a memory heap to store variables:

```javascript
// Example of call stack in action
function greet(name) {
  // greet() is added to the call stack
  const greeting = `Hello, ${name}!`;
  return greeting;
  // greet() is removed from the call stack when it returns
}

function welcome() {
  // welcome() is added to the call stack
  const message = greet("Developer");
  console.log(message);
  // welcome() is removed from the call stack when it completes
}

welcome();
```

### Measuring Performance

#### Using Performance API

The Performance API provides precise timing measurements:

```javascript
// Measure how long a function takes to execute
function measurePerformance(fn, ...args) {
  const start = performance.now();
  const result = fn(...args);
  const end = performance.now();
  console.log(`Execution time: ${end - start} ms`);
  return result;
}

// Example function to measure
function findPrimes(max) {
  const sieve = Array(max).fill(true);
  sieve[0] = sieve[1] = false;
  
  for (let i = 2; i <= Math.sqrt(max); i++) {
    if (sieve[i]) {
      for (let j = i * i; j < max; j += i) {
        sieve[j] = false;
      }
    }
  }
  
  return sieve.reduce((primes, isPrime, num) => {
    if (isPrime) primes.push(num);
    return primes;
  }, []);
}

// Measure performance
measurePerformance(findPrimes, 100000);
```

#### Chrome DevTools Performance Tab

Learn to use Chrome DevTools to profile JavaScript performance:

```javascript
// This is a demonstration of how to analyze this code with DevTools
// In a real browser environment, you would:
// 1. Open DevTools (F12)
// 2. Go to the Performance tab
// 3. Click Record
// 4. Perform the action you want to analyze
// 5. Stop recording and analyze the results

console.log("To analyze performance in Chrome DevTools:");
console.log("1. Press F12 to open DevTools");
console.log("2. Go to the Performance tab");
console.log("3. Click the record button (circle)");
console.log("4. Perform the action you want to analyze");
console.log("5. Click the stop button");
console.log("6. Analyze the flame chart to identify bottlenecks");

// Example of what you might analyze
function simulateHeavyOperation() {
  console.log("This would be a CPU-intensive operation in a real app");
  
  // In a real scenario, you'd see this in the flame chart
  // if it was taking significant time
  let sum = 0;
  for (let i = 0; i < 1000000; i++) {
    sum += i;
  }
  
  return sum;
}

simulateHeavyOperation();
```

### Memory Management

#### Understanding Garbage Collection

JavaScript automatically manages memory through garbage collection:

```javascript
// Objects are garbage collected when they're no longer reachable
let user = { name: "John" }; // Object is created and referenced by 'user'

user = null; // The object is now unreachable and eligible for garbage collection

// The JavaScript engine will eventually free the memory used by the object
```

#### Identifying Memory Leaks

Common causes of memory leaks and how to find them:

```javascript
// Example 1: Accidental global variables
function createGlobal() {
  leakyVariable = "I'm leaking into global scope!"; // Missing 'let', 'const', or 'var'
}

// Example 2: Forgotten event listeners
function setupButton() {
  const button = document.createElement('button');
  
  // This event listener keeps a reference to the button
  // even if the button is removed from the DOM
  button.addEventListener('click', function() {
    // Do something with button
    console.log(button.innerHTML);
  });
  
  return button;
}

// Example 3: Closures holding references
function createLargeDataProcessor() {
  const largeData = new Array(1000000).fill('data');
  
  return function process() {
    // This closure keeps a reference to largeData
    // even if it's only used once
    return largeData[0];
  };
}

// To fix these issues:
// 1. Always declare variables with let/const
// 2. Remove event listeners when elements are removed
// 3. Be careful with closures and large data
```

#### Using Chrome DevTools Memory Tab

```javascript
// In Chrome DevTools, you can take heap snapshots to find memory leaks
console.log("To analyze memory usage in Chrome DevTools:");
console.log("1. Open DevTools (F12)");
console.log("2. Go to the Memory tab");
console.log("3. Select 'Heap snapshot' and click 'Take snapshot'");
console.log("4. Perform actions in your app");
console.log("5. Take another snapshot");
console.log("6. Compare snapshots to identify retained objects");

// Example of what you might analyze
function simulateMemoryLeak() {
  console.log("This would create a memory leak in a real app");
  
  // In a real scenario, this would show up in heap snapshots
  // as growing retained memory
  window = window || global;
  if (!window.leakyArray) {
    window.leakyArray = [];
  }
  
  // This would keep adding data to the global array
  // without ever cleaning it up
  console.log("In a browser, this would add 10,000 objects to a global array");
  // window.leakyArray.push(...Array(10000).fill().map(() => ({ data: new Array(1000).fill('x') })));
}

simulateMemoryLeak();
```

### Optimizing DOM Operations

#### Minimizing DOM Manipulation

The DOM is often a performance bottleneck:

```javascript
// Inefficient: Multiple DOM operations
function addItemsInefficient(items) {
  const list = document.getElementById('itemList');
  
  // This causes multiple reflows and repaints
  items.forEach(item => {
    const li = document.createElement('li');
    li.textContent = item;
    list.appendChild(li); // DOM is modified on each iteration
  });
}

// Efficient: Batch DOM operations
function addItemsEfficient(items) {
  const list = document.getElementById('itemList');
  const fragment = document.createDocumentFragment();
  
  // Create all elements in memory first
  items.forEach(item => {
    const li = document.createElement('li');
    li.textContent = item;
    fragment.appendChild(li); // No DOM modification yet
  });
  
  // Single DOM operation
  list.appendChild(fragment);
}

// Even more efficient: Use innerHTML for static content
function addItemsMostEfficient(items) {
  const list = document.getElementById('itemList');
  
  // Single DOM operation with string concatenation
  list.innerHTML = items.map(item => `<li>${item}</li>`).join('');
}
```

#### Virtual DOM Concepts

Understanding how frameworks like React optimize rendering:

```javascript
// Simplified virtual DOM concept
class VirtualDOM {
  constructor() {
    this.virtualTree = null;
    this.realDOM = document.getElementById('app');
  }
  
  render(newVirtualTree) {
    const patches = this.diff(this.virtualTree, newVirtualTree);
    this.patch(this.realDOM, patches);
    this.virtualTree = newVirtualTree;
  }
  
  diff(oldTree, newTree) {
    // In a real implementation, this would compare
    // the old and new virtual trees to find differences
    console.log('Calculating difference between trees');
    return { type: 'update', changes: {} };
  }
  
  patch(dom, patches) {
    // In a real implementation, this would apply
    // only the necessary changes to the real DOM
    console.log('Applying minimal changes to real DOM');
  }
}

// This is how frameworks like React work under the hood
// They only update what actually changed, not the entire DOM
```

### Efficient Data Structures and Algorithms

#### Choosing the Right Data Structure

Different data structures have different performance characteristics:

```javascript
// Example: Finding items

// Array - O(n) lookup time
function findInArray(array, item) {
  const start = performance.now();
  
  const index = array.indexOf(item);
  
  const end = performance.now();
  console.log(`Array lookup took ${end - start} ms`);
  return index !== -1;
}

// Object - O(1) lookup time
function findInObject(object, key) {
  const start = performance.now();
  
  const exists = key in object;
  
  const end = performance.now();
  console.log(`Object lookup took ${end - start} ms`);
  return exists;
}

// Set - O(1) lookup time
function findInSet(set, item) {
  const start = performance.now();
  
  const exists = set.has(item);
  
  const end = performance.now();
  console.log(`Set lookup took ${end - start} ms`);
  return exists;
}

// Compare performance with large data
const size = 100000;
const target = 99999;

const array = Array(size).fill().map((_, i) => i);
const object = Object.fromEntries(array.map(i => [i, true]));
const set = new Set(array);

findInArray(array, target);
findInObject(object, target);
findInSet(set, target);
```

#### Algorithm Optimization

Improving algorithm efficiency:

```javascript
// Inefficient: O(n²) nested loops
function findDuplicatesInefficient(array) {
  const start = performance.now();
  
  const duplicates = [];
  
  for (let i = 0; i < array.length; i++) {
    for (let j = i + 1; j < array.length; j++) {
      if (array[i] === array[j] && !duplicates.includes(array[i])) {
        duplicates.push(array[i]);
      }
    }
  }
  
  const end = performance.now();
  console.log(`Inefficient algorithm took ${end - start} ms`);
  return duplicates;
}

// Efficient: O(n) using a Set
function findDuplicatesEfficient(array) {
  const start = performance.now();
  
  const seen = new Set();
  const duplicates = new Set();
  
  for (const item of array) {
    if (seen.has(item)) {
      duplicates.add(item);
    } else {
      seen.add(item);
    }
  }
  
  const end = performance.now();
  console.log(`Efficient algorithm took ${end - start} ms`);
  return [...duplicates];
}

// Test with a large array
const testArray = Array(10000).fill().map(() => Math.floor(Math.random() * 1000));

findDuplicatesInefficient(testArray);
findDuplicatesEfficient(testArray);
```

### Asynchronous JavaScript Optimization

#### Promises and Async/Await

Efficient asynchronous code:

```javascript
// Sequential execution (slower)
async function fetchDataSequential(ids) {
  const start = performance.now();
  
  const results = [];
  for (const id of ids) {
    // Each request waits for the previous one to complete
    const result = await fetch(`https://jsonplaceholder.typicode.com/posts/${id}`);
    const data = await result.json();
    results.push(data);
  }
  
  const end = performance.now();
  console.log(`Sequential fetching took ${end - start} ms`);
  return results;
}

// Parallel execution (faster)
async function fetchDataParallel(ids) {
  const start = performance.now();
  
  // Create all promises at once
  const promises = ids.map(id => 
    fetch(`https://jsonplaceholder.typicode.com/posts/${id}`)
      .then(response => response.json())
  );
  
  // Wait for all to complete
  const results = await Promise.all(promises);
  
  const end = performance.now();
  console.log(`Parallel fetching took ${end - start} ms`);
  return results;
}

// Test both approaches
const ids = [1, 2, 3, 4, 5];
// In a real environment, you would see the parallel version
// complete much faster than the sequential version
```

#### Debouncing and Throttling

Controlling the frequency of function execution:

```javascript
// Debounce: Execute function only after a certain amount of time has passed
// since it was last invoked
function debounce(func, wait) {
  let timeout;
  
  return function executedFunction(...args) {
    const later = () => {
      clearTimeout(timeout);
      func(...args);
    };
    
    clearTimeout(timeout);
    timeout = setTimeout(later, wait);
  };
}

// Throttle: Execute function at most once per specified time period
function throttle(func, limit) {
  let inThrottle;
  
  return function executedFunction(...args) {
    if (!inThrottle) {
      func(...args);
      inThrottle = true;
      setTimeout(() => {
        inThrottle = false;
      }, limit);
    }
  };
}

// Example usage for search input
const searchInput = { value: '' };

// Without debounce (would trigger on every keystroke)
function searchWithoutDebounce() {
  console.log(`Searching for: ${searchInput.value}`);
  // This would make an API call on every keystroke
}

// With debounce (waits until typing stops)
const debouncedSearch = debounce(function() {
  console.log(`Searching for: ${searchInput.value} (debounced)`);
  // This makes an API call only after typing stops for 300ms
}, 300);

// Example usage for scroll events
// Without throttle (would trigger hundreds of times during scrolling)
function handleScrollWithoutThrottle() {
  console.log('Scroll event fired');
  // This would run too frequently during scrolling
}

// With throttle (runs at most once every 100ms)
const throttledScroll = throttle(function() {
  console.log('Scroll event fired (throttled)');
  // This runs at most once every 100ms during scrolling
}, 100);

// Simulate typing in search box
searchInput.value = 'a';
searchWithoutDebounce(); // Immediate search
debouncedSearch();       // Waits for 300ms

searchInput.value = 'ap';
searchWithoutDebounce(); // Another immediate search
debouncedSearch();       // Resets the 300ms timer

searchInput.value = 'app';
searchWithoutDebounce(); // Yet another immediate search
debouncedSearch();       // Resets the 300ms timer again

// In a real environment, only one search would happen after typing stops
```

### Network Optimization

#### Lazy Loading

Loading resources only when needed:

```javascript
// Basic implementation of lazy loading images
function lazyLoadImages() {
  // Select all images with data-src attribute
  const lazyImages = document.querySelectorAll('img[data-src]');
  
  // Create an intersection observer
  const observer = new IntersectionObserver((entries, observer) => {
    entries.forEach(entry => {
      // If the image is in the viewport
      if (entry.isIntersecting) {
        const img = entry.target;
        
        // Replace the src with the data-src
        img.src = img.dataset.src;
        
        // Remove the data-src attribute
        img.removeAttribute('data-src');
        
        // Stop observing this image
        observer.unobserve(img);
      }
    });
  });
  
  // Observe each lazy image
  lazyImages.forEach(img => {
    observer.observe(img);
  });
}

// Example HTML structure:
// <img src="placeholder.jpg" data-src="actual-image.jpg" alt="Lazy loaded image">
```

#### Code Splitting

Breaking code into smaller chunks:

```javascript
// Without code splitting (loads everything upfront)
import { heavyFeature1, heavyFeature2, heavyFeature3 } from './features';

function app() {
  // All features are loaded, even if not used immediately
  const button1 = document.getElementById('feature1');
  const button2 = document.getElementById('feature2');
  const button3 = document.getElementById('feature3');
  
  button1.addEventListener('click', heavyFeature1);
  button2.addEventListener('click', heavyFeature2);
  button3.addEventListener('click', heavyFeature3);
}

// With code splitting (loads features on demand)
function appWithCodeSplitting() {
  const button1 = document.getElementById('feature1');
  const button2 = document.getElementById('feature2');
  const button3 = document.getElementById('feature3');
  
  button1.addEventListener('click', async () => {
    // Feature 1 is loaded only when button is clicked
    const { heavyFeature1 } = await import('./feature1');
    heavyFeature1();
  });
  
  button2.addEventListener('click', async () => {
    // Feature 2 is loaded only when button is clicked
    const { heavyFeature2 } = await import('./feature2');
    heavyFeature2();
  });
  
  button3.addEventListener('click', async () => {
    // Feature 3 is loaded only when button is clicked
    const { heavyFeature3 } = await import('./feature3');
    heavyFeature3();
  });
}

// In modern bundlers like webpack, this creates separate chunks
// that are loaded only when needed
```

## Activities

### Activity 1: Performance Audit (30 minutes)

1. Choose a website you frequently use
2. Open Chrome DevTools and go to the Performance tab
3. Record a performance profile while performing a common action (e.g., scrolling, clicking a button)
4. Analyze the results, focusing on:
  - Long tasks (shown in red)
  - JavaScript execution time
  - Layout and rendering time
5. Take screenshots of any bottlenecks you identify
6. Write a brief summary of what you found and what could be improved


### Activity 2: Memory Leak Detective (45 minutes)

1. Clone this repository with a sample application containing memory leaks:

```plaintext
git clone https://github.com/example/memory-leak-demo
cd memory-leak-demo
npm install
npm start
```
2. Open the application in Chrome
3. Use the Memory tab in DevTools to take heap snapshots:
    - Take an initial snapshot
    - Perform actions in the app that might cause leaks (e.g., opening/closing modals)
    - Take another snapshot
    - Compare snapshots to identify retained objects
4. Find at least two memory leaks in the application
5. Fix the leaks and verify your fixes with new heap snapshots


### Activity 3: Optimize a Slow Function (40 minutes)
1. Analyze this inefficient function:

```javascript
// Inefficient function to find prime numbers
function findPrimesInefficient(max) {
  const primes = [];
  
  // For each number up to max
  for (let num = 2; num <= max; num++) {
    let isPrime = true;
    
    // Check if it's divisible by any number
    for (let i = 2; i < num; i++) {
      if (num % i === 0) {
        isPrime = false;
        break;
      }
    }
    
    if (isPrime) {
      primes.push(num);
    }
  }
  
  return primes;
}

// Test the function with a large number
console.time('Inefficient');
findPrimesInefficient(10000);
console.timeEnd('Inefficient');

// Your task: Optimize this function
// Implement the Sieve of Eratosthenes algorithm
function findPrimesOptimized(max) {
  // Your optimized implementation here
  // Hint: Use the Sieve of Eratosthenes algorithm
}

// Test your optimized function
console.time('Your optimized version');
findPrimesOptimized(10000);
console.timeEnd('Your optimized version');
```
2. Implement the optimized version using the Sieve of Eratosthenes algorithm
3. Compare the execution times
4. Explain why your solution is more efficient


### Activity 4: DOM Performance Challenge (60 minutes)

1. Create an application that renders a list of 10,000 items
2. Implement it in three different ways:
    - Direct DOM manipulation (adding elements one by one)
    - Using document fragments
    - Using virtual DOM concepts (you can use a simple implementation or a library like React)
3. Measure the performance of each approach using `performance.now()`
4. Create a visualization (chart or table) comparing the results
5. Write a brief explanation of why one approach performs better than the others


### Activity 5: Real-world Optimization Project (90 minutes)

1. Choose a small web application you've built or find an open-source project
2. Run a performance audit using Lighthouse in Chrome DevTools
3. Identify at least three performance issues
4. Implement fixes for these issues, focusing on:
    - JavaScript optimization
    - Asset loading
    - DOM performance
5. Run another Lighthouse audit to measure your improvements
6. Create a before/after comparison showing the impact of your changes


## Common Mistakes / Misconceptions

### Misconception 1: "Premature optimization is always bad"

While it's true that premature optimization can lead to unnecessary complexity, ignoring performance entirely until the end can result in architectural decisions that are difficult to change. Instead, be aware of performance implications during development, but focus deep optimization efforts on measured bottlenecks.

### Misconception 2: "Modern browsers are fast enough, so optimization doesn't matter"

While browsers have become incredibly efficient, JavaScript performance still matters, especially on mobile devices or in countries with slower internet connections. A performant application provides a better user experience for everyone and can significantly impact business metrics.

### Misconception 3: "More memory is always better than more CPU time"

This trade-off depends on the specific use case. For mobile devices with limited memory, optimizing for memory usage might be more important. For computation-heavy applications, CPU optimization might be the priority. Always measure both aspects and optimize based on your target environment.

### Misconception 4: "Micro-optimizations always lead to better performance"

Focusing on micro-optimizations like using `for` loops instead of `forEach` often yields minimal real-world benefits and can make code less readable. Focus on algorithmic improvements and architectural optimizations first, which typically have much larger impacts.

### Misconception 5: "You should always use the latest JavaScript features for better performance"

Newer features aren't always faster. For example, the spread operator (`...`) is convenient but can be slower than alternatives like `Array.prototype.concat()` for large arrays. Always measure performance in your specific context.

### Misconception 6: "Virtual DOM is always faster than direct DOM manipulation"

Virtual DOM frameworks like React are optimized for developer productivity and maintainability, not necessarily raw performance. For simple UI updates, direct DOM manipulation can sometimes be faster. Choose technologies based on your project's specific needs.

### Misconception 7: "You need to write perfect code from the start"

Performance optimization is an iterative process. Start with clean, maintainable code, measure performance, identify bottlenecks, and then optimize those specific areas. This approach leads to both performant and maintainable applications.
