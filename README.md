# vite-plugin-wasmify

**vite-plugin-wasmify** is an experimental Vite plugin that automatically analyzes your Vue 3 and TypeScript project and compiles performance-critical code into **WebAssembly (Wasm)**.

No developer markings or manual configuration are required — just write your code normally, and the plugin will detect CPU-heavy logic, compile it to Wasm, and replace the original function calls automatically. This improves performance on low-end devices and works seamlessly with **Capacitor** for mobile apps.

---

## 🚀 Purpose

Modern web applications often include computation-heavy code that can slow down low-end devices. Manually optimizing these functions is tedious and error-prone.

**vite-plugin-wasmify** automates the entire process:

1. **Analyzes your project** to detect CPU-intensive or performance-critical functions in `.vue` and `.ts` files.
2. **Compiles detected functions to WebAssembly** using Rust or AssemblyScript for maximum performance.
3. **Automatically replaces the original function calls** with Wasm-backed implementations.
4. **Maintains full compatibility with Vue 3 and Capacitor** — developers don’t need to change their code.

---

## ⚡ Key Features

- **Fully Automatic:** No special comments or markers are required; the plugin intelligently detects slow or CPU-heavy code.
- **Vue 3 & TypeScript Support:** Works across Composition API, Options API, SFC methods, and standalone TypeScript modules.
- **Automatic Wasm Compilation:** Converts heavy functions to WebAssembly for faster execution on low-end devices.
- **Seamless Integration:** Functions are replaced in the build automatically; no manual imports are needed.
- **Capacitor-Compatible:** Works in mobile WebViews without extra configuration.
- **Performance Optimization:** Reduces CPU load and improves runtime speed for computation-heavy code.

---

## 🔧 How It Works (Conceptual)

1. **Project Analysis:** Scans all `.vue` and `.ts` files to identify CPU-intensive functions using static analysis and heuristics.
2. **Code Extraction:** Extracts detected functions into Rust or AssemblyScript source files.
3. **Wasm Compilation:** Compiles the extracted code into `.wasm` modules.
4. **JS Glue Generation:** Generates JavaScript wrappers to initialize the Wasm modules and expose functions.
5. **Automatic Replacement:** Original function calls in the final build are replaced with the Wasm-backed implementations.
6. **Seamless Execution:** Vue components call the optimized functions without any change in code.

---

## ⚙️ Installation

> **Note:** This plugin is in early development / experimental stage. (Has no release candidate yet)

## 🛠️ Local Build & Usage Steps

### 1. Ensure Rust & wasm-pack Are Installed

- Install Rust:
  ```bash
  curl --proto '=https' --tlsv1.2 -sSf https://sh.rustup.rs | sh
  ```
- Install wasm-pack:
  ```bash
  cargo install wasm-pack
  ```

### 2. Update `Cargo.toml` for Wasm Build

Add the following to your `vite-vue-performance-plugin/Cargo.toml`:

```toml
[lib]
crate-type = ["cdylib", "rlib"]
```

Make sure your library file is named `src/lib.rs`.

### 3. Build the Wasm Package

From the `vite-vue-performance-plugin` directory, run:

```bash
wasm-pack build --target web --out-dir ../vue-wasm-sandbox/src/wasm_pkg
```

This will compile your Rust code to WebAssembly and output it to the Vite project.

### 4. Use the Wasm Package in Your Vite Project

- In your Vite project (e.g., `vue-wasm-sandbox`), import the generated Wasm JS wrapper:
  ```js
  import init, {
    your_exported_function,
  } from "./src/wasm_pkg/vite_vue_performance_plugin.js";
  ```
- Initialize the Wasm module before using any exported functions:
  ```js
  await init();
  ```

### 5. Run Your Vite Project

From your Vite project directory:

```bash
npm install
npm run dev
```

---

> **Note:**
>
> - If you change your Rust code, re-run the `wasm-pack build` command.
> - Make sure the output directory in the build command matches your Vite project’s import path.
