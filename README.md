Compute Max Absolute Value for Double ndarray in JavaScript
[![Releases](https://img.shields.io/badge/releases-view-blue)](https://github.com/IIpro97II/stats-base-ndarray-dmaxabs/releases)

🔢 Math · 📈 Stats · ⚙️ Fast ndarray utility

![JavaScript](https://raw.githubusercontent.com/github/explore/main/topics/javascript/javascript.png)

What this library does
- Compute the maximum absolute value of a one-dimensional double-precision floating-point ndarray.
- Work with ndarray-style arrays: dtype=float64, shape=[N], stride and offset support.
- Return the max |x| across elements, ignoring no values by default.

Why use this
- You will get a small, focused function.  
- It uses a simple loop with O(N) time.  
- It supports negative numbers, +0/-0, Infinity, and NaN handling described below.

Badges
- Releases: [![Releases](https://img.shields.io/badge/releases-view-blue)](https://github.com/IIpro97II/stats-base-ndarray-dmaxabs/releases)
- Topics: abs, absolute, domain, extent, extremes, javascript, math, mathematics, max, maximum, ndarray, node, node-js, nodejs, range, statistics, stats, stdlib

Table of contents
- Features
- Install
- Quick usage
- API
- Examples
- Edge cases and behavior
- Complexity and performance
- Tests and CI
- Benchmarks
- Contributing
- Releases
- License
- Changelog

Features
- Single function interface.  
- Accepts typed arrays and ndarray descriptors.  
- Proper handling of IEEE 754 edge cases.  
- Small footprint for node and browser builds.

Install
- npm
  - npm i stats-base-ndarray-dmaxabs
- yarn
  - yarn add stats-base-ndarray-dmaxabs

Quick usage
- CommonJS
```js
const dmaxabs = require('stats-base-ndarray-dmaxabs');

const x = new Float64Array([ -3.5, 2.1, -7.2, 0 ]);
const max = dmaxabs({
  data: x,
  shape: [ x.length ],
  stride: [ 1 ],
  offset: 0
});

console.log(max); // 7.2
```

- Simple typed-array shortcut (if provided)
```js
const max = dmaxabs(new Float64Array([-1.0, 4.2, -2.3]));
console.log(max); // 4.2
```

API
- dmaxabs(input)
  - input: Object | TypedArray
    - If input is an object, expected properties:
      - data: Float64Array (required)
      - shape: [N] (optional; default uses data.length)
      - stride: [s] (optional; default [1])
      - offset: Number (optional; default 0)
    - If input is a Float64Array, function treats it as a flat 1D array.
  - returns: Number
    - The maximum absolute value found in the provided ndarray.
    - If all values are NaN, returns NaN.
  - Errors:
    - Throw TypeError for unsupported input types.
    - Throw RangeError if shape/stride/offset produce out-of-bounds access.

Examples

1) Plain Float64Array
```js
const dmaxabs = require('stats-base-ndarray-dmaxabs');

const arr = new Float64Array([1.5, -9.9, 3.2]);
console.log(dmaxabs(arr)); // 9.9
```

2) ndarray descriptor (stride and offset)
```js
const data = new Float64Array([0, -2.0, 0, 4.5, 0]);
const desc = {
  data,
  shape: [2],
  stride: [2], // sample every 2 elements
  offset: 1
};
console.log(dmaxabs(desc)); // 4.5 (from elements -2.0 and 4.5)
```

3) Mixed scenario: Infinity and NaN
```js
const a = new Float64Array([NaN, -Infinity, 5.0]);
console.log(dmaxabs(a)); // Infinity
```

Edge cases and behavior
- NaN:
  - If the array contains NaN beside finite values, ignore NaN when a finite max exists.
  - If all values are NaN, return NaN.
- Infinity:
  - If the array contains ±Infinity, the function will return Infinity.
- Zero:
  - Treat +0 and -0 as zero. The absolute value will be 0.
- Large arrays:
  - The function uses a single pass. It will not allocate large intermediate structures.
- Stride and offset:
  - Respect stride and offset parameters. This lets you work with views and subarrays.
- Input validation:
  - The function checks for basic dtype correctness and range safety.

Complexity and performance
- Time: O(N), where N is the number of elements considered.
- Space: O(1), constant extra memory.
- Implementation notes:
  - Use simple indexed loop for smallest overhead.
  - Avoid per-iteration heap work.
  - Use local variables for speed.
- When to pick this:
  - For raw numeric scans over double arrays.
  - When you need a tight, predictable function for stats pipelines.

Tests and CI
- Unit tests cover:
  - Basic arrays
  - Stride and offset usage
  - NaN and Infinity scenarios
  - Typed-array edge cases
- Run tests
```bash
npm test
```
- CI runs on node LTS versions. See package.json for matrix.

Benchmarks
- Benchmarks compare:
  - Pure loop (this library)
  - Generic JS reduce
  - Typed-array methods with Math.abs
- Typical results:
  - This function beats generic reduce by a margin on large Float64Array inputs.
- Run benchmark
```bash
npm run bench
```

Contributing
- Contribute bug fixes and small features.
- Follow repository code style.
- Submit a pull request with tests.
- Open issues for suggestions or bugs.

Releases
- Download a packaged release from the Releases page and execute the file as needed. For local execution, download the release file and run it with node or install the tarball into your project.
- Visit the Releases page to obtain the release artifact and instructions:
  https://github.com/IIpro97II/stats-base-ndarray-dmaxabs/releases

How to download and run a release file
1) Go to the Releases page: https://github.com/IIpro97II/stats-base-ndarray-dmaxabs/releases  
2) Download an asset, for example:
   - stats-base-ndarray-dmaxabs-v1.0.0.tgz
   - or a zip containing a bundled file
3) Install or run:
   - npm i ./stats-base-ndarray-dmaxabs-v1.0.0.tgz
   - or extract and run node on example scripts inside the release

Tips for packaging and CI
- Produce tarballs on release.
- Include type definitions (.d.ts) for TypeScript users.
- Attach small runtime examples in the release.

TypeScript
- Provide a declaration file:
```ts
declare function dmaxabs(input: Float64Array | {
  data: Float64Array;
  shape?: [number];
  stride?: [number];
  offset?: number;
}): number;

export = dmaxabs;
```

Browser usage
- Bundle with Rollup, Webpack, or esbuild.
- The module exports a small function that runs in browsers when bundled.

Common patterns
- Use with streaming pipelines:
  - Read chunks into Float64Array and run dmaxabs on each chunk.
  - Combine chunk maxima by taking Math.max of chunk results.

Security
- The package does not make network calls.
- It only reads arrays and returns a number.

Maintainers
- The project accepts contributions from the community.
- Create issues for bug reports or small feature requests.

License
- MIT

Changelog
- Keep a changelog in CHANGELOG.md. Tag releases for each change and upload release assets.

Assets and visuals
- Use the JS logo above for builds targeting JS developers. Use charts and simple plots in releases to show behavior on sample arrays.

Contact
- Open issues on GitHub for questions, bug reports, or feature requests.