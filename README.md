# Change Array by copy

Provides additional methods on `Array.prototype` and `TypedArray.prototype` to enable changes on the array by returning a new copy of it with the change.

## Status

This proposal is currently at [Stage 2].

- [Candidate spec text][spec]
- [Candidate polyfill][poly]

[Stage 2]: https://github.com/tc39/proposals#stage-2
[spec]: https://tc39.es/proposal-change-array-by-copy/
[poly]: ./polyfill.js


## Champions

- Robin Ricard (Bloomberg)
- Ashley Claymore (Bloomberg)

## Reviewers

- [Jordan Harband](https://github.com/ljharb)
- [Justin Ridgewell](https://github.com/jridgewell)
- [Sergey Rubanov](https://github.com/chicoxyzzy)

## Overview

This proposal introduces the following function properties to `Array.prototype`:

- `Array.prototype.withReversed() -> Array`
- `Array.prototype.withSorted(compareFn) -> Array`
- `Array.prototype.withSpliced(start, deleteCount, ...items) -> Array`
- `Array.prototype.withAt(index, value) -> Array`

All of those methods keep the target Array untouched and returns a copy of it with the change performed instead.

They will also be added to TypedArrays:

- `TypedArray.prototype.withReversed() -> TypedArray`
- `TypedArray.prototype.withSorted(compareFn) -> TypedArray`
- `TypedArray.prototype.withSpliced(start, deleteCount, ...items) -> TypedArray`
- `TypedArray.prototype.withAt(index, value) -> TypedArray`

These methods will then be avaliable on subclasses of `TypedArray`. i.e. the following:

- `Int8Array`
- `Uint8Array`
- `Uint8ClampedArray`
- `Int16Array`
- `Uint16Array`
- `Int32Array`
- `Uint32Array`
- `Float32Array`
- `Float64Array`
- `BigInt64Array`
- `BigUint64Array`

### Example

```js
const sequence = [1, 2, 3];
sequence.withReversed(); // => [3, 2, 1]
sequence; // => [1, 2, 3]

const outOfOrder = new Uint8Array([3, 1, 2]);
outOfOrder.withSorted(); // => Uint8Array [1, 2, 3]
outOfOrder; // => Uint8Array [3, 1, 2]

const correctionNeeded = [1, 1, 3];
correctionNeeded.withAt(1, 2); // => [1, 2, 3]
correctionNeeded; // => [1, 1, 3]
```

## Motivation

The [`Tuple.prototype`][tuple-proto] introduces these functions as a way to deal with the immutable aspect of the Tuples in [Record & Tuple][r-t]. While Arrays are not immutable by nature, this style of programming can be beneficial to users dealing with frozen arrays for instance.

This proposal notably makes it easier to write code able to deal with Arrays and Tuples interchangeably.

## Relationship with [Record & Tuple][r-t]

While this proposal is derived from [Record & Tuple][r-t], it should progress independently.

If web compatibility prescribes it, property names defined in this proposal are going to be changed. Those changes should be reflected on [`Tuple.prototype`][tuple-proto].

[tuple-proto]: https://tc39.es/proposal-record-tuple/#sec-properties-of-the-tuple-prototype-object
[r-t]: https://github.com/tc39/proposal-record-tuple
