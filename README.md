# wasm-simd

This repository intends to help write efficient WASM SIMD programs.

[Instructions](Instructions.md) describes the performance of various WASM SIMD instructions on x64/arm64.

[Shuffles](Shuffles.md) describes the performance of various WASM SIMD byte shuffles on x64/arm64.

The performance information collected here is targeting v8 x64/arm64 as the most frequently used browser/architecture combination.

# Why?

Writing cross-platform SIMD code is challenging - WASM SIMD provides a nice abstraction that guarantees the same results between platforms, but unifying behavior comes at a performance cost. Some operations are fast on all platforms, whereas some other operations represent "performance cliffs" - they might be fast on one platform and slow on another platform.

Being aware of the microarchitectural differences helps when writing cross-platform code, and these guides intend to help navigate the space of WASM SIMD performance by guiding the programmers to use efficient instructions and avoid inefficient or imbalanced instructions.
