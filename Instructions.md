# Instructions

For each Wasm SIMD instruction, you can see the number of instructions that v8 generates.

Note that while instruction count isn't always indicative of performance, it can give you an insight into what instructions to use and what to avoid.

The instruction counts are assuming AVX2 / AArch64 codegen respectively.

This table is based on analyzing v8 source as of February 28th 2020; note that v8 continues to improve the codegen and some of these instruction counts are going to be improved in the future.

| Instruction                | v8 x64                    | v8 arm64              |
| ---------------------------|---------------------------|-----------------------|
| `v128.load`                |                         1 |                     1 |
| `v128.store`               |                         1 |                     1 |
| `v128.const`               |                       N/A |                   N/A |
| `i8x16.splat`              |                         3 |                     1 |
| `v8x16.load_splat`         |                         3 |                     1 |
| `i8x16.extract_lane_s`     |                         2 |                     1 |
| `i8x16.extract_lane_u`     |                         1 |                     1 |
| `i8x16.replace_lane`       |                         1 |                   1-2 |
| `i16x8.splat`              |                         3 |                     1 |
| `v16x8.load_splat`         |                         3 |                     1 |
| `i16x8.extract_lane_s`     |                         2 |                     1 |
| `i16x8.extract_lane_u`     |                         1 |                     1 |
| `i16x8.replace_lane`       |                         1 |                   1-2 |
| `i32x4.splat`              |                         2 |                     1 |
| `v32x4.load_splat`         |                         1 |                     1 |
| `i32x4.extract_lane`       |                         1 |                     1 |
| `i32x4.replace_lane`       |                         1 |                   1-2 |
| `i64x2.splat`              |                         1 |                     1 |
| `i64x2.extract_lane`       |                         1 |                     1 |
| `i64x2.replace_lane`       |                         1 |                   1-2 |
| `f32x4.splat`              |                         2 |                     1 |
| `f32x4.extract_lane`       |                         2 |                     1 |
| `f32x4.replace_lane`       |                         1 |                   1-2 |
| `f64x2.splat`              |                         2 |                     1 |
| `v64x2.load_splat`         |                         1 |                     1 |
| `f64x2.extract_lane`       |                         2 |                     1 |
| `f64x2.replace_lane`       |                         2 |                   1-2 |
| `i8x16.eq`                 |                         1 |                     1 |
| `i8x16.ne`                 |                         3 |                     2 |
| `i8x16.lt_s`               |                         1 |                     1 |
| `i8x16.lt_u`               |                         4 |                     1 |
| `i8x16.gt_s`               |                         1 |                     1 |
| `i8x16.gt_u`               |                         4 |                     1 |
| `i8x16.le_s`               |                         2 |                     1 |
| `i8x16.le_u`               |                         2 |                     1 |
| `i8x16.ge_s`               |                         2 |                     1 |
| `i8x16.ge_u`               |                         2 |                     1 |
| `i16x8.eq`                 |                         1 |                     1 |
| `i16x8.ne`                 |                         3 |                     2 |
| `i16x8.lt_s`               |                         1 |                     1 |
| `i16x8.lt_u`               |                         4 |                     1 |
| `i16x8.gt_s`               |                         1 |                     1 |
| `i16x8.gt_u`               |                         4 |                     1 |
| `i16x8.le_s`               |                         2 |                     1 |
| `i16x8.le_u`               |                         2 |                     1 |
| `i16x8.ge_s`               |                         2 |                     1 |
| `i16x8.ge_u`               |                         2 |                     1 |
| `i32x4.eq`                 |                         1 |                     1 |
| `i32x4.ne`                 |                         3 |                     2 |
| `i32x4.lt_s`               |                         1 |                     1 |
| `i32x4.lt_u`               |                         4 |                     1 |
| `i32x4.gt_s`               |                         1 |                     1 |
| `i32x4.gt_u`               |                         4 |                     1 |
| `i32x4.le_s`               |                         2 |                     1 |
| `i32x4.le_u`               |                         2 |                     1 |
| `i32x4.ge_s`               |                         2 |                     1 |
| `i32x4.ge_u`               |                         2 |                     1 |
| `f32x4.eq`                 |                         1 |                     1 |
| `f32x4.ne`                 |                         1 |                     2 |
| `f32x4.lt`                 |                         1 |                     1 |
| `f32x4.gt`                 |                         1 |                     1 |
| `f32x4.le`                 |                         1 |                     1 |
| `f32x4.ge`                 |                         1 |                     1 |
| `f64x2.eq`                 |                         1 |                     1 |
| `f64x2.ne`                 |                         1 |                     2 |
| `f64x2.lt`                 |                         1 |                     1 |
| `f64x2.gt`                 |                         1 |                     1 |
| `f64x2.le`                 |                         1 |                     1 |
| `f64x2.ge`                 |                         1 |                     1 |
| `v128.not`                 |                       2-3 |                     1 |
| `v128.and`                 |                         1 |                     1 |
| `v128.andnot`              |                         1 |                     1 |
| `v128.or`                  |                         1 |                     1 |
| `v128.xor`                 |                         1 |                     1 |
| `v128.bitselect`           |                         4 |                     1 |
| `i8x16.neg`                |                         2 |                     1 |
| `i8x16.any_true`           |                         3 |                     4 |
| `i8x16.all_true`           |                         5 |                     4 |
| `i8x16.shl`                |                      5-10 |                   1-3 |
| `i8x16.shr_s`              |                       5-9 |                   1-4 |
| `i8x16.shr_u`              |                       5-9 |                   1-4 |
| `i8x16.add`                |                         1 |                     1 |
| `i8x16.add_saturate_s`     |                         1 |                     1 |
| `i8x16.add_saturate_u`     |                         1 |                     1 |
| `i8x16.sub`                |                         1 |                     1 |
| `i8x16.sub_saturate_s`     |                         1 |                     1 |
| `i8x16.sub_saturate_u`     |                         1 |                     1 |
| `i8x16.min_s`              |                         1 |                     1 |
| `i8x16.min_u`              |                         1 |                     1 |
| `i8x16.max_s`              |                         1 |                     1 |
| `i8x16.max_u`              |                         1 |                     1 |
| `i8x16.avgr_u`             |                         1 |                     1 |
| `i8x16.abs`                |                         1 |                     1 |
| `i16x8.neg`                |                         2 |                     1 |
| `i16x8.any_true`           |                         3 |                     4 |
| `i16x8.all_true`           |                         5 |                     4 |
| `i16x8.shl`                |                       1-3 |                   1-3 |
| `i16x8.shr_s`              |                       1-3 |                   1-4 |
| `i16x8.shr_u`              |                       1-3 |                   1-4 |
| `i16x8.add`                |                         1 |                     1 |
| `i16x8.add_saturate_s`     |                         1 |                     1 |
| `i16x8.add_saturate_u`     |                         1 |                     1 |
| `i16x8.sub`                |                         1 |                     1 |
| `i16x8.sub_saturate_s`     |                         1 |                     1 |
| `i16x8.sub_saturate_u`     |                         1 |                     1 |
| `i16x8.mul`                |                         1 |                     1 |
| `i16x8.min_s`              |                         1 |                     1 |
| `i16x8.min_u`              |                         1 |                     1 |
| `i16x8.max_s`              |                         1 |                     1 |
| `i16x8.max_u`              |                         1 |                     1 |
| `i16x8.avgr_u`             |                         1 |                     1 |
| `i16x8.abs`                |                         1 |                     1 |
| `i32x4.neg`                |                         3 |                     1 |
| `i32x4.any_true`           |                         3 |                     4 |
| `i32x4.all_true`           |                         5 |                     4 |
| `i32x4.shl`                |                       1-3 |                   1-3 |
| `i32x4.shr_s`              |                       1-3 |                   1-4 |
| `i32x4.shr_u`              |                       1-3 |                   1-4 |
| `i32x4.add`                |                         1 |                     1 |
| `i32x4.sub`                |                         1 |                     1 |
| `i32x4.mul`                |                         1 |                     1 |
| `i32x4.min_s`              |                         1 |                     1 |
| `i32x4.min_u`              |                         1 |                     1 |
| `i32x4.max_s`              |                         1 |                     1 |
| `i32x4.max_u`              |                         1 |                     1 |
| `i32x4.abs`                |                         1 |                     1 |
| `i64x2.neg`                |                         2 |                     1 |
| `i64x2.shl`                |                       1-3 |                   1-3 |
| `i64x2.shr_s`              |                         8 |                   1-4 |
| `i64x2.shr_u`              |                       1-3 |                   1-4 |
| `i64x2.add`                |                         1 |                     1 |
| `i64x2.sub`                |                         1 |                     1 |
| `i64x2.mul`                |                        10 |                     7 |
| `f32x4.abs`                |                         3 |                     1 |
| `f32x4.neg`                |                         3 |                     1 |
| `f32x4.sqrt`               |                         1 |                     1 |
| `f32x4.add`                |                         1 |                     1 |
| `f32x4.sub`                |                         1 |                     1 |
| `f32x4.mul`                |                         1 |                     1 |
| `f32x4.div`                |                         1 |                     1 |
| `f32x4.min`                |                         6 |                     1 |
| `f32x4.max`                |                         6 |                     1 |
| `f64x2.abs`                |                         3 |                     1 |
| `f64x2.neg`                |                         3 |                     1 |
| `f64x2.sqrt`               |                         1 |                     1 |
| `f64x2.add`                |                         1 |                     1 |
| `f64x2.sub`                |                         1 |                     1 |
| `f64x2.mul`                |                         1 |                     1 |
| `f64x2.div`                |                         1 |                     1 |
| `f64x2.min`                |                         8 |                     1 |
| `f64x2.max`                |                         9 |                     1 |
| `i32x4.trunc_sat_f32x4_s`  |                         7 |                     1 |
| `i32x4.trunc_sat_f32x4_u`  |                        13 |                     1 |
| `f32x4.convert_i32x4_s`    |                         1 |                     1 |
| `f32x4.convert_i32x4_u`    |                         8 |                     1 |
| `v8x16.swizzle`            |                         4 |                     1 |
| `v8x16.shuffle`            |                      1-11 |                 1-12? |
| `i16x8.load8x8_s`          |                       N/A |                   N/A |
| `i16x8.load8x8_u`          |                       N/A |                   N/A |
| `i32x4.load16x4_s`         |                       N/A |                   N/A |
| `i32x4.load16x4_u`         |                       N/A |                   N/A |
| `i64x2.load32x2_s`         |                       N/A |                   N/A |
| `i64x2.load32x2_u`         |                       N/A |                   N/A |
| `i8x16.narrow_i16x8_s`     |                         1 |                   2-3 |
| `i8x16.narrow_i16x8_u`     |                         1 |                   2-3 |
| `i16x8.narrow_i32x4_s`     |                         1 |                   2-3 |
| `i16x8.narrow_i32x4_u`     |                         1 |                   2-3 |
| `i16x8.widen_low_i8x16_s`  |                         1 |                     1 |
| `i16x8.widen_high_i8x16_s` |                         2 |                     1 |
| `i16x8.widen_low_i8x16_u`  |                         1 |                     1 |
| `i16x8.widen_high_i8x16_u` |                         2 |                     1 |
| `i32x4.widen_low_i16x8_s`  |                         1 |                     1 |
| `i32x4.widen_high_i16x8_s` |                         2 |                     1 |
| `i32x4.widen_low_i16x8_u`  |                         1 |                     1 |
| `i32x4.widen_high_i16x8_u` |                         2 |                     1 |

Notes:

- The instruction counts above ignore extra moves that may happen between instructions; see https://bugs.chromium.org/p/v8/issues/detail?id=10116
- All 16 or 32-bit shifts are 1 instruction on x64 when the shift operand is an immediate and 3 when it's not
- All 8-bit shifts are 5 instructions on x64 when the shift operand is an immediate and 9-10 when it's not
- All shifts are 1 instruction on arm64 when the shift operand is an immediate and 3-4 when it's not
