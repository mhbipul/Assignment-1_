# Type Aliases vs Interfaces in TypeScript

## Introduction

Both `type` and `interface` describe the shape of data in TypeScript, and for plain objects they often behave the same way. The differences show up when you extend, merge, or combine types, so it helps to choose on purpose.

## Extending and Combining

An interface grows with `extends`. A type alias combines with the intersection operator `&`.

```ts
interface Student { id: number; name: string }
interface CollegeStudent extends Student { semester: number }

type Teacher = { id: number; subject: string };
type Mentor = Teacher & { experience: number };
```

If two properties conflict, `extends` reports an error immediately. With `&`, the conflicting property silently becomes `never`, which can hide a mistake until later.

## Declaration Merging

Only interfaces can be declared twice with the same name, and TypeScript merges them into one.

```ts
interface Course { title: string }
interface Course { students: number }
```

Declaring a type alias twice with the same name is an error.

## What Only `type` Can Do

A type alias can name a union, a tuple, or a mapped type, which an interface cannot express.

```ts
type Status = "active" | "inactive";
type Coordinate = readonly [number, number, string];
```

A class can `implement` either an interface or an object type alias, so that is not a deciding factor.

## Conclusion

I use an `interface` for object shapes that other code may extend or merge, and a `type` for unions, tuples, and computed types. When both would work, I follow the style the codebase already uses.

