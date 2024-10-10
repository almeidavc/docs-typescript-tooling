# Part 1: Standalone Typescript App

## The `module` option

- The `module` option determines the module system of the emitted Javascript code. Using this setting, you can write code with module system x and output code using module system y
- The default value is `commonjs`
- With the value `nodenext`, the compiler emits both cjs and esm modules depending on the file extension and, if the file extension is ambigous e.g. ".ts" from the nearest `package.json`. If the package.json contains `type: "module"` then the ts compiler emits esm modules

## The `moduleResolution` option

- How imports/exports are resolved when the code is actually run depends on the runtime.
  - In browser environments, importing "./a.js" imports the file "./a.js"
  - In some runtimes, importing folder x imports the file "x/index.js"
- When it comes to modules, typescript's main job is to ensure that imports (in the output files) resolve successfully at runtime
- In order to this, it needs to know what algorithm is going to be used to resolve modules at runtime
- The `moduleResolution` option specifies how the target runtime will resolve imports/exports

## FAQ

### Why do I need to include the file extension in imports?

- This is the case when `"moduleResolution": "node16"` or `"moduleResolution": "nodenext"`
- Why is this necessary?
  - Typescript does **not** ever change the module specifier, e.g.`"math.mjs"` in `import { add } from "./math.mjs"`, of an import statement
  - The target host or runtime is Node.js v16+ in this case. The Node.js v16+ runtime requires that paths include the file extension. It throws an error if you omit it. See https://www.typescriptlang.org/docs/handbook/modules/appendices/esm-cjs-interop.html#different-module-resolution-algorithms

### Main app and dependency x use different module systems

- Node.js v16 supports cjs and esm modules side-by-side in the same project using a set of interop rules
- compiled `utils` package uses cjs and compiled `main` package uses esm (due to `"type": "module"`)

## Troubleshooting

### Error [ERR_REQUIRE_ESM]: require() of ES Module

- In Node.js v16+, you can't require an esm module in commonjs using `require()`. You have to use `await import()`
- On the other hand, you can import a commonjs module in esm using import statements.
- The conclusion is simple: transpile your code to esm. Transpiling your code to esm ensures that it is compatible with all types of packages wether they use commonjs or esm. On the other hand, if you transpile your code to commonjs, you can't depend packages that only support esm
- How do you configure typescript to emit esm modules? Simply include `"type": "module"` in your package.json

## Open Questions

- Static vs dynamic imports
- What's the difference between `require()` and `await import()`
- How to support both cjs, and esm consumers

### References

- https://www.typescriptlang.org/docs/handbook/modules/theory.html
- https://stackoverflow.com/questions/71463698/why-we-need-nodenext-typescript-compiler-option-when-we-have-esnext
- https://dev.to/snyk/building-an-npm-package-compatible-with-esm-and-cjs-in-2024-88m
- https://www.totaltypescript.com/tsconfig-cheat-sheet
