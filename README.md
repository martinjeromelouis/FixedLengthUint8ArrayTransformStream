# Introduction
FixedLengthUint8ArrayTransformStream is a DENO open-source library (MIT license) that transforms a stream of variable-length Uint8Arrays into a stream of fixed-length Uint8Arrays.

This can for instance be useful for decrypting a stream.

DO NOT USE THIS PACKAGE ANY MORE. PLEASE USE deno_simple_utilities INSTEAD.

# Import
import { FixedLengthUint8ArrayTransformStream } from "https://deno.land/x/fixed_length_uint8array_transform_stream@v1.0.1/mod.ts";
# API
## class FixedLengthUint8ArrayTransformStream extends TransformStream<Uint8Array, Uint8Array>
### constructor(firstChunkLength: number, otherChunksLength: number, allowMultiples, transformFirstChunk, transformOtherChunks, transformLastChunk)
#### @param firstChunkLength

The length of the first chunk. If 0, the first chunk is handled like other chunks.

#### @param otherChunksLength

The length of each chunk except the first one if firstChunkLength > 0 and the last one if its length is strictly inferior to otherChunksLength.

#### @param allowMultiples

Applies to all chunks except the first one if firstChunkLength > 0 and the last one if its length is trictly inferior to otherChunksLength. If false (default value), then the afore-mentioned chunks will be of length otherChunksLengths. If true, then the afore-mentioned chunks will be of length k*otherChunksLengths (where k is a strictly positive integer).

#### @param transformFirstChunk

A callback that will be called for the first chunk if firstChunkLength > 0 and there are at least firstChunkLength bytes in the stream. Return modified chunk if needed. Default is identity function.

#### @param transformOtherChunks

A callback that will be called for all chunks except the first one if firstChunkLength > 0 and the last one if its length is strictly inferior to otherChunksLength. The _eos parameter is true if the passed chunk is the last one. Return modified chunk if needed. Default is identity function.

#### @param transformLastChunk

A callback that will be called for the last chunk if its length is strictly inferior to otherChunksLength. Note : if there is one single chunk whose length is inferior to firstChunkLength, this callback is called for that chunk. Return modified chunk if needed. Default is identity function.

#### @throws RangeError
Throws if firstChunkLength is not a positive integer or otherChunksLength is not a strictly positive integer.

# Example
```ts
import { readableStreamFromIterable } from "https://deno.land/std@0.181.0/streams/readable_stream_from_iterable.ts";
import { FixedLengthUint8ArrayTransformStream } from "https://deno.land/x/fixed_length_uint8array_transform_stream@v1.0.2/mod.ts";
const inputStream = readableStreamFromIterable<Uint8Array>([
  new Uint8Array(12),
  new Uint8Array(44),
  new Uint8Array(8),
]);
const outputStream = inputStream.pipeThrough(
  new FixedLengthUint8ArrayTransformStream(13, 16, false, true),
);

for await (const chunk of outputStream) console.log(chunk.length);
```
This code is available at https://github.com/martinjeromelouis/FixedLengthUint8ArrayTransformStream/blob/master/examples/examples.ts.

# Tests
See https://github.com/martinjeromelouis/FixedLengthUint8ArrayTransformStream/blob/master/tests/FixedLengthUint8ArrayTransformStream_test.ts

# License
MIT
