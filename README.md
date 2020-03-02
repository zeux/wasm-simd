# wasm-simd

This repository intends to help write efficient Wasm SIMD programs.

The performance information collected here is targeting v8 x64/arm64 as the most frequently used browser/architecture combination.

# Why?

Writing cross-platform SIMD code is challenging - Wasm SIMD provides a nice abstraction that guarantees the same results between platforms, but unifying behavior comes at a performance cost. Some operations are fast on all platforms, whereas some other operations represent "performance cliffs" - they might be fast on one platform and slow on another platform.

Being aware of the microarchitectural differences helps when writing cross-platform code, and these guides intend to help navigate the space of Wasm SIMD performance by guiding the programmers to use efficient instructions and avoid inefficient or imbalanced instructions.

# What?

[Instructions](Instructions.md) describes the performance of various Wasm SIMD instructions on x64/arm64.

[Shuffles](Shuffles.md) describes the performance of various Wasm SIMD byte shuffles on x64/arm64.

# Inspecting assembly output

The best way to analyze the performance of a Wasm SIMD kernel today is to study the assembly output for the target architecture. Using a debug v8 build (which is easy to install using https://github.com/GoogleChromeLabs/jsvu), the following command will produce the assembly output for the Wasm code that is compiled by the JS module/program:

```
$ v8-debug --experimental-wasm-simd --print-wasm-code --no-liftoff --single-threaded --no-debug-code input.js
```
