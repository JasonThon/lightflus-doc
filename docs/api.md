# Overview

Lightflus provide Typescript API for all developers.

## API NOTES

Lightflus is implemented by *Rust* and powered by *Deno* (A JS runtime, backed by V8 engine). However, for the
purpose of performance, we turn to use Deno as an embedded module, not a runtime container. This means, We **support
limited numbers of third-party libs** and **user-defined types cannot be imported correctly** now (We are trying to find
a new way). Moreover, maybe some features of JavaScript/TypeScript is also not supported yet.

Third-party libs we supported are listed in the following table:

| Lib | Npm Link | Description |
|:----|:---------|:------------|

## TYPESCRIPT VS RUST

Typescript's type system is huge distinct from Rust. We have to clarify the type mapping between Typescript and Rust.

For now, all mapping relations between Typescript and Rust in Lightflus are as follows:

| Type in Typescript | Type in Rust                          | Description                                                                                                               |
|:-------------------|:--------------------------------------|:--------------------------------------------------------------------------------------------------------------------------|
| number             | u32, u64, i32, i64, f32, f64, u8, u16 | `number` in Typescript represents any number type value in Rust.                                                          |
| bigint             | i64                                   | `bigint` in Typescript will be recognized as `i64` in Rust                                                                |
| string             | String, str                           | All native types are supported                                                                                            |
| object             | BTreeMap                              | object can be represented as map in Rust. However, the value will be serialized as byte array                             |
| boolean            | bool                                  | All native types are supported                                                                                            |                                                                             |
| void               | void                                  | All native types are supported                                                                                            |
| enum               | u32                                   | `enum` in Typescript will be recognized as unsigned integer                                                               |
| array              | Vec                                   | `array` in Typescript will be recognized as `Vec` in Rust. The generic type `K` must be the types that Lightflus supports |
| Tuple2             | Tuple2                                | The generic types `(K,V)` of `Tuple2` must be Lightflus supported types.                                                  | 

## DATAFLOW TRANSFORMATIONS

Operators transform Dataflow into a new one. Users can write a program to combine these operators ino a sophisticated
dataflow topologies

### MAP

**Dataflow -> Dataflow**

Takes one element and produce a new element.

#### Example

```typescript

```

### FLATMAP

**Dataflow -> Dataflow**

Takes one element and produces zero, one, or more elements

#### Example

```typescript
// let stream: Dataflow<string> = ....
stream.flatMap(v => v.split("."))

```

### FILTER

**Dataflow -> Dataflow**

Evaluates a boolean function for each element and retains those for which the function returns true

#### Example

```typescript
// let stream: Dataflow<string> = ....
stream.filter(v => v.contains("foo"))
```

### KEYBY

**Dataflow -> KeyedDataflow**

Logically partitions a stream into disjoint partitions. All records with the same key are assigned to the same
partition.

#### Example

```typescript
// let stream: Dataflow<object> = ....
stream.keyBy(v => v.id)
```

### REDUCE

**KeyedDataflow -> Dataflow**

Combines the current element with the last reduced value and emits the new value.

#### Example

```typescript
// let stream: Dataflow<object> = ....
stream.keyBy(v => v.id)
    .reduce((agg, current) => {
        agg.amount += current.amount;
        return agg
    })
```

### WINDOW

**KeyedDataflow → WindowedDataflow**

Lightflus support three kinds of window

* FixedWindow
* SlidingWindow
* SessionWindow

#### Example

```typescript

```

### WINDOW JOIN

**WindowedDataflow → WindowedDataflow**

A window-join joins the elements of two streams that share a common key and lie in the same window.

The general usage can be summarized as follows:

```typescript
// let stream: Dataflow<object> = ....
// let otherStream: Dataflow<object> = ....
// let keyExtractor: (v: object) => object = ....
let joinedWindow = stream.join(otherStream)
    .where(keyExtractor)
    .equalTo(keyExtractor)
    .window(FixedWindow.size(10));
// joinedWindow...
```

Some notes on semantics:

* The creation of pairwise combinations of elements of the two streams behaves like an inner-join, meaning elements from
  one stream will not be emitted if they don’t have a corresponding element from the other stream to be joined with.
* Those elements that do get joined will have as their timestamp the largest timestamp that still lies in the respective
  window. For example a window with [5, 10) as its boundaries would result in the joined elements having 9 as their
  timestamp.

In the following sections we are going to use some example sceneries to help you to understand how window join works.

#### FIXED WINDOW JOIN

#### Sliding WINDOW JOIN

#### Session WINDOW JOIN

