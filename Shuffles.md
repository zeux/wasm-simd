# Shuffles

If you read Instructions comparison, you may have noticed that v8x16.shuffle has a range of 1-16 instructions generated. What's up with that?

Shuffles are... complicated!

The short story is that on both x64 and arm64, v8 will try to pattern-match shuffle byte masks with one of the masks supported by the code generator natively. Note that "natively" doesn't mean it's just a single instruction - it can be more for more complex shuffles.

When the pattern matching fails, v8 resorts to a bruteforce solution, which is pretty slow/sad - on x64 it involves constructing two 16 byte masks for `pshufb` on the stack, doing two shuffles and or-ing the result (16 instructions!), on arm64 it involves constructing a 16-byte table lookup mask in the register using a lot of 16-bit immediate moves and using a table lookup (>10 instructions, not sure what the exact count is?).

Both fallback paths are *slow* so you want to avoid them at all costs, which means that (unfortunately) you need to know which masks pattern-match well - this is complex!

Because describing the entire possibility space is way too hard, let's look at shuffles that are supported efficiently on both x64 and arm64.

## Splats

These shuffle masks replicate one input lane (8/16/32-bit) to all output lanes. They map to 1-2 instructions on x64 and 1 instruction on arm64.
Notably, 64-bit splats are *not* supported!

Examples for duplicating lane 0:

- 8-bit: `{0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0, 0}`
- 16-bit: `{0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1, 0, 1}`
- 16-bit: `{0, 1, 2, 3, 0, 1, 2, 3, 0, 1, 2, 3, 0, 1, 2, 3}`

## Swizzles

These shuffle masks achieve an arbitrary swizzle (lane permutation) for the input vector for 32-bit lanes. They map to 1 instruction on x64 and (sadly) 5 instructions on arm64.

Example for `wzxy` swizzle:

- `{12, 13, 14, 15, 8, 9, 10, 11, 0, 1, 2, 3, 4, 5, 6, 7}`

## Unpacks

These shuffle masks map to 1 instruction on x64 and 1 instruction on arm64.
They allow you to alternate between 8/16/32/64-bit elements from both vectors, e.g. 32-bit low unpack transforms (a0 a1 a2 a3) + (b0 b1 b2 b3) into (a0 b0 a1 b1).

64-bit:
- low: `{0, 1, 2, 3, 4, 5, 6, 7, 16, 17, 18, 19, 20, 21, 22, 23}`
- high: `{8, 9, 10, 11, 12, 13, 14, 15, 24, 25, 26, 27, 28, 29, 30, 31}`
 
32-bit:
- low: `{0, 1, 2, 3, 16, 17, 18, 19, 4, 5, 6, 7, 20, 21, 22, 23}`
- high: `{8, 9, 10, 11, 24, 25, 26, 27, 12, 13, 14, 15, 28, 29, 30, 31}`
   
16-bit:
- low: `{0, 1, 16, 17, 2, 3, 18, 19, 4, 5, 20, 21, 6, 7, 22, 23}`
- high: `{8, 9, 24, 25, 10, 11, 26, 27, 12, 13, 28, 29, 14, 15, 30, 31}`

8-bit:
- low: `{0, 16, 1, 17, 2, 18, 3, 19, 4, 20, 5, 21, 6, 22, 7, 23}`
- high: `{8, 24, 9, 25, 10, 26, 11, 27, 12, 28, 13, 29, 14, 30, 15, 31}`


## Concats

These shuffle masks map to 1 instruction on x64 and 1 instruction on arm64.
These shuffle masks allow you to concatenate two parts of the input registers, taking a suffix of the first register (e.g. last 10 bytes) and the prefix of the second register (e.g. first 6 bytes).

Example:

- `{6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21}`

## Unzips

These shuffle masks map to 4-6 instructions on x64 (1 for 32-bit) and 1 instruction on arm64.
They allow you to alternate between 8/16/32-bit elements from both vectors, but instead of working on low/high halves of the input vector, they output odd/even elements from each vector sequentially.

32-bit:
- even: `{0, 1, 2, 3, 8, 9, 10, 11, 16, 17, 18, 19, 24, 25, 26, 27}`
- odd: `{4, 5, 6, 7, 12, 13, 14, 15, 20, 21, 22, 23, 28, 29, 30, 31}`

16-bit:
- even: `{0, 1, 4, 5, 8, 9, 12, 13, 16, 17, 20, 21, 24, 25, 28, 29}`
- odd: `{2, 3, 6, 7, 10, 11, 14, 15, 18, 19, 22, 23, 26, 27, 30, 31}`

8-bit:
- even: `{0, 2, 4, 6, 8, 10, 12, 14, 16, 18, 20, 22, 24, 26, 28, 30}`
- odd: `{1, 3, 5, 7, 9, 11, 13, 15, 17, 19, 21, 23, 25, 27, 29, 31}`

## Transposes

These shuffle masks map to 4-5 instructions on x64 and 1 instruction on arm64.
They allow you to alternate between 8-bit elements from both vectors, but instead of working on low/high halves of the input vector, they output odd/even elements, and instead of outputting elements from each vector sequentially they alternate between the two vectors. Who comes up with this stuff?

8-bit:
- even: `{0, 16, 2, 18, 4, 20, 6, 22, 8, 24, 10, 26, 12, 28, 14, 30}`
- odd: `{1, 17, 3, 19, 5, 21, 7, 23, 9, 25, 11, 27, 13, 29, 15, 31}`

## Reverses

These shuffle masks map to 4-6 instructions on x64 and 1 instruction on arm64.
They allow you to reverse the order of bytes in each 64/32/16-bit lane. Happy endian swapping.

- 64-bit: `{7, 6, 5, 4, 3, 2, 1, 0, 15, 14, 13, 12, 11, 10, 9, 8}`
- 32-bit: `{3, 2, 1, 0, 7, 6, 5, 4, 11, 10, 9, 8, 15, 14, 13, 12}`
- 16-bit: `{1, 0, 3, 2, 5, 4, 7, 6, 9, 8, 11, 10, 13, 12, 15, 14}`

Note: When https://bugs.chromium.org/p/v8/issues/detail?id=10117 gets fixed I'm expecting that most shuffle instructions above that map to ~5 x64 instructions become much less interesting for cross-platform code, and the set of fast portable shuffles can be reduced further.
