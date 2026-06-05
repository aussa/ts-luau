# ts-luau

`ts-luau` is a general-purpose TypeScript-to-Luau/Lua compiler. It compiles TypeScript code into functionally equivalent Luau/Lua structures. 

## Features

- **Direct Relative Imports**: Compiles ES module imports directly into standard relative `require()` calls (e.g. `require("./module")`).
- **Flexible Environment Polyfills**: Standard runtime libraries (such as the asynchronous Promise implementation) are fully environment-agnostic, running correctly on any standard Lua VM.
- **Type Checking**: Easily verify your types during compilation with lightweight custom declaration files.

## Installation

To build `ts-luau` from source:

1. Clone this repository.
2. Install dependencies:
   ```bash
   npm install
   ```
3. Compile the `ts-luau` compiler:
   ```bash
   npm run build
   ```

## Compiling Your TypeScript Project

To compile a TypeScript project using `ts-luau`:

1. **Define a `tsconfig.json`** at the root of your project:
   ```json
   {
     "compilerOptions": {
       "target": "ESNext",
       "module": "CommonJS",
       "moduleDetection": "force",
       "moduleResolution": "Node",
       "noLib": true,
       "strict": true,
       "allowSyntheticDefaultImports": true,
       "rootDir": "src",
       "outDir": "out"
     },
     "rbxts": {
       "type": "standalone"
     }
   }
   ```

2. **Project Directory Structure**:
   Place all your `.ts` source files inside the directory specified by `"rootDir"` (usually `src`).
   ```text
   your-mod-project/
   ├── tsconfig.json
   └── src/
       └── mod.ts
   ```

3. **Run the Compiler Command**:
   Open a terminal, navigate to your project directory (containing `tsconfig.json`), and run one of the following commands:

   * **Option A: Run directly from TypeScript source (`.ts`) using `npx tsx`**:
     ```bash
     npx tsx /path/to/ts-luau/src/CLI/cli.ts --project ./
     ```
   * **Option B: Run from the compiled JavaScript build (`.js`)**:
     ```bash
     node /path/to/ts-luau/out/CLI/cli.js --project ./
     ```
   
   * *Replace `/path/to/ts-luau` with the actual path to where you cloned this repository.*
   * *`--project ./` tells the compiler to read the config and source files from the current folder.*

4. **Compilation Output**:
   The compiler will search for all `.ts` files inside `"rootDir"` (`src/`), compile them, and write the output Luau files to `"outDir"` (`out/`) under the same structure:
   ```text
   your-mod-project/
   ├── tsconfig.json
   ├── src/
   │   └── mod.ts
   └── out/
       └── mod.luau  <-- Your compiled Luau code is saved here!
   ```

## Differences from standard JS/TS

- **Length Operator**: To get the size/length of a string, array, map, or set, use `.size()` (e.g. `myString.size()`), which compiles to the standard Lua length operator `#` or size check.
- **Byte Access**: Use `str.byte(index)` (1-based index) to obtain the byte character value of a string, which compiles directly to `string.byte(str, index)`.
