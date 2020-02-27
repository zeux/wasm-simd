This file has performance information for x64/ARM64 that you can expect out of WASM SIMD.

For each SIMD instruction, you can see the number of instructions that v8 generates.

Note that while instruction count isn't always indicative of performance, it can give you an insight into what instructions to use and what to avoid.

The instruction counts are assuming AVX2 / AArch64 codegen respectively.

# Instructions

| Instruction                | v8 x64                    | v8 arm64              |
| ---------------------------|---------------------------|-----------------------|
| `v128.load`                |                         1 |                       |
| `v128.store`               |                         1 |                       |
| `v128.const`               |                       N/A |                       |
| `i8x16.splat`              |                         3 |                       |
| `v8x16.load_splat`         |                         3 |                       |
| `i8x16.extract_lane_s`     |                         2 |                       |
| `i8x16.extract_lane_u`     |                         1 |                       |
| `i8x16.replace_lane`       |                         1 |                       |
| `i16x8.splat`              |                         3 |                       |
| `v16x8.load_splat`         |                         3 |                       |
| `i16x8.extract_lane_s`     |                         2 |                       |
| `i16x8.extract_lane_u`     |                         1 |                       |
| `i16x8.replace_lane`       |                         1 |                       |
| `i32x4.splat`              |                         2 |                       |
| `v32x4.load_splat`         |                         1 |                       |
| `i32x4.extract_lane`       |                         1 |                       |
| `i32x4.replace_lane`       |                         1 |                       |
| `i64x2.splat`              |                         1 |                       |
| `i64x2.extract_lane`       |                         2 |                       |
| `i64x2.replace_lane`       |                         2 |                       |
| `f32x4.splat`              |                         2 |                       |
| `f32x4.extract_lane`       |                         2 |                       |
| `f32x4.replace_lane`       |                         1 |                       |
| `f64x2.splat`              |                         2 |                       |
| `v64x2.load_splat`         |                         1 |                       |
| `f64x2.extract_lane`       |                         2 |                       |
| `f64x2.replace_lane`       |                         2 |                       |
| `i8x16.eq`                 |                         1 |                       |
| `i8x16.ne`                 |                         3 |                       |
| `i8x16.lt_s`               |                         1 |                       |
| `i8x16.lt_u`               |                         4 |                       |
| `i8x16.gt_s`               |                         1 |                       |
| `i8x16.gt_u`               |                         4 |                       |
| `i8x16.le_s`               |                         2 |                       |
| `i8x16.le_u`               |                         2 |                       |
| `i8x16.ge_s`               |                         2 |                       |
| `i8x16.ge_u`               |                         2 |                       |
| `i16x8.eq`                 |                         1 |                       |
| `i16x8.ne`                 |                         3 |                       |
| `i16x8.lt_s`               |                         1 |                       |
| `i16x8.lt_u`               |                         4 |                       |
| `i16x8.gt_s`               |                         1 |                       |
| `i16x8.gt_u`               |                         4 |                       |
| `i16x8.le_s`               |                         2 |                       |
| `i16x8.le_u`               |                         2 |                       |
| `i16x8.ge_s`               |                         2 |                       |
| `i16x8.ge_u`               |                         2 |                       |
| `i32x4.eq`                 |                         1 |                       |
| `i32x4.ne`                 |                         3 |                       |
| `i32x4.lt_s`               |                         1 |                       |
| `i32x4.lt_u`               |                         4 |                       |
| `i32x4.gt_s`               |                         1 |                       |
| `i32x4.gt_u`               |                         4 |                       |
| `i32x4.le_s`               |                         2 |                       |
| `i32x4.le_u`               |                         2 |                       |
| `i32x4.ge_s`               |                         2 |                       |
| `i32x4.ge_u`               |                         2 |                       |
| `f32x4.eq`                 |                         1 |                       |
| `f32x4.ne`                 |                         1 |                       |
| `f32x4.lt`                 |                         1 |                       |
| `f32x4.gt`                 |                         1 |                       |
| `f32x4.le`                 |                         1 |                       |
| `f32x4.ge`                 |                         1 |                       |
| `f64x2.eq`                 |                         1 |                       |
| `f64x2.ne`                 |                         1 |                       |
| `f64x2.lt`                 |                         1 |                       |
| `f64x2.gt`                 |                         1 |                       |
| `f64x2.le`                 |                         1 |                       |
| `f64x2.ge`                 |                         1 |                       |
| `v128.not`                 |                       2-3 |                       |
| `v128.and`                 |                         1 |                       |
| `v128.andnot`              |                         1 |                       |
| `v128.or`                  |                         1 |                       |
| `v128.xor`                 |                         1 |                       |
| `v128.bitselect`           |                         4 |                       |
| `i8x16.neg`                |                         2 |                       |
| `i8x16.any_true`           |                         4 |                       |
| `i8x16.all_true`           |                         6 |                       |
| `i8x16.shl`                |                           |                       |
| `i8x16.shr_s`              |                           |                       |
| `i8x16.shr_u`              |                           |                       |
| `i8x16.add`                |                           |                       |
| `i8x16.add_saturate_s`     |                           |                       |
| `i8x16.add_saturate_u`     |                           |                       |
| `i8x16.sub`                |                           |                       |
| `i8x16.sub_saturate_s`     |                           |                       |
| `i8x16.sub_saturate_u`     |                           |                       |
| `i8x16.min_s`              |                           |                       |
| `i8x16.min_u`              |                           |                       |
| `i8x16.max_s`              |                           |                       |
| `i8x16.max_u`              |                           |                       |
| `i8x16.avgr_u`             |                           |                       |
| `i8x16.abs`                |                           |                       |
| `i16x8.neg`                |                           |                       |
| `i16x8.any_true`           |                           |                       |
| `i16x8.all_true`           |                           |                       |
| `i16x8.shl`                |                           |                       |
| `i16x8.shr_s`              |                           |                       |
| `i16x8.shr_u`              |                           |                       |
| `i16x8.add`                |                           |                       |
| `i16x8.add_saturate_s`     |                           |                       |
| `i16x8.add_saturate_u`     |                           |                       |
| `i16x8.sub`                |                           |                       |
| `i16x8.sub_saturate_s`     |                           |                       |
| `i16x8.sub_saturate_u`     |                           |                       |
| `i16x8.mul`                |                           |                       |
| `i16x8.min_s`              |                           |                       |
| `i16x8.min_u`              |                           |                       |
| `i16x8.max_s`              |                           |                       |
| `i16x8.max_u`              |                           |                       |
| `i16x8.avgr_u`             |                           |                       |
| `i16x8.abs`                |                           |                       |
| `i32x4.neg`                |                           |                       |
| `i32x4.any_true`           |                           |                       |
| `i32x4.all_true`           |                           |                       |
| `i32x4.shl`                |                           |                       |
| `i32x4.shr_s`              |                           |                       |
| `i32x4.shr_u`              |                           |                       |
| `i32x4.add`                |                           |                       |
| `i32x4.sub`                |                           |                       |
| `i32x4.mul`                |                           |                       |
| `i32x4.min_s`              |                           |                       |
| `i32x4.min_u`              |                           |                       |
| `i32x4.max_s`              |                           |                       |
| `i32x4.max_u`              |                           |                       |
| `i32x4.abs`                |                           |                       |
| `i64x2.neg`                |                           |                       |
| `i64x2.shl`                |                           |                       |
| `i64x2.shr_s`              |                           |                       |
| `i64x2.shr_u`              |                           |                       |
| `i64x2.add`                |                           |                       |
| `i64x2.sub`                |                           |                       |
| `i64x2.mul`                |                           |                       |
| `f32x4.abs`                |                           |                       |
| `f32x4.neg`                |                           |                       |
| `f32x4.sqrt`               |                           |                       |
| `f32x4.add`                |                           |                       |
| `f32x4.sub`                |                           |                       |
| `f32x4.mul`                |                           |                       |
| `f32x4.div`                |                           |                       |
| `f32x4.min`                |                           |                       |
| `f32x4.max`                |                           |                       |
| `f64x2.abs`                |                           |                       |
| `f64x2.neg`                |                           |                       |
| `f64x2.sqrt`               |                           |                       |
| `f64x2.add`                |                           |                       |
| `f64x2.sub`                |                           |                       |
| `f64x2.mul`                |                           |                       |
| `f64x2.div`                |                           |                       |
| `f64x2.min`                |                           |                       |
| `f64x2.max`                |                           |                       |
| `i32x4.trunc_sat_f32x4_s`  |                           |                       |
| `i32x4.trunc_sat_f32x4_u`  |                           |                       |
| `f32x4.convert_i32x4_s`    |                           |                       |
| `f32x4.convert_i32x4_u`    |                           |                       |
| `v8x16.swizzle`            |                           |                       |
| `v8x16.shuffle`            |                           |                       |
| `i16x8.load8x8_s`          |                           |                       |
| `i16x8.load8x8_u`          |                           |                       |
| `i32x4.load16x4_s`         |                           |                       |
| `i32x4.load16x4_u`         |                           |                       |
| `i64x2.load32x2_s`         |                           |                       |
| `i64x2.load32x2_u`         |                           |                       |
| `i8x16.narrow_i16x8_s`     |                           |                       |
| `i8x16.narrow_i16x8_u`     |                           |                       |
| `i16x8.narrow_i32x4_s`     |                           |                       |
| `i16x8.narrow_i32x4_u`     |                           |                       |
| `i16x8.widen_low_i8x16_s`  |                           |                       |
| `i16x8.widen_high_i8x16_s` |                           |                       |
| `i16x8.widen_low_i8x16_u`  |                           |                       |
| `i16x8.widen_high_i8x16_u` |                           |                       |
| `i32x4.widen_low_i16x8_s`  |                           |                       |
| `i32x4.widen_high_i16x8_s` |                           |                       |
| `i32x4.widen_low_i16x8_u`  |                           |                       |
| `i32x4.widen_high_i16x8_u` |                           |                       |

# Shuffles
