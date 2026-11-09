# Understanding Type Narrowing in TypeScript



A union type like `string | number` can hold more than one kind of value, so TypeScript only lets you use what is safe for every member of the union. Type narrowing is how you prove which type you actually have, so TypeScript allows the methods and properties of that specific type.

## Narrowing with `typeof`

TypeScript follows your control flow. Inside the `if`, `value` is a `string`. After the `if` returns, TypeScript knows only a `number` is left.

```ts
function double(value: string | number): string | number {
  if (typeof value === "string") {
    return value.repeat(2);
  }
  return value * 2;
}
```

`typeof` works for primitives such as strings, numbers, and booleans, but it can't tell one class or object shape from another.

## Narrowing with `instanceof`

`instanceof` checks the prototype chain, so it works with classes but not with interfaces or type aliases, which don't exist at runtime. It is most useful in `catch` blocks, where the caught value is `unknown`.

```ts
try {
  JSON.parse("{");
} catch (error: unknown) {
  if (error instanceof Error) {
    console.log(error.message);
  }
}
```

## Narrowing with `in`

The `in` operator checks whether a property exists on an object, which lets you tell object types apart by their fields.

```ts
type Circle = { radius: number };
type Square = { side: number };

function getArea(shape: Circle | Square): number {
  if ("radius" in shape) {
    return Math.PI * shape.radius ** 2;
  }
  return shape.side ** 2;
}
```


Each operator checks something different: `typeof` for primitives, `instanceof` for class instances, and `in` for object shapes. Narrowing lets you handle unions safely without forcing the type with `as`.

