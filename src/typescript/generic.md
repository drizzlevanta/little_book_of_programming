# Generic

## `keyof` Type Operator

The keyof operator takes an object type and produces a string or numeric literal union of its keys.

The following type P is the same type as type `P = "x" | "y"`:

```typescript
type Point = { x: number; y: number };
type P = keyof Point;
```

Another example:

```typescript
type Arrayish = { [n: number]: unknown };
type A = keyof Arrayish;

//type A = number;
```

## Indexed Access types

We can use an indexed access type to look up a specific property on another type:

```typescript
type Person = { age: number; name: string; alive: boolean };
type Age = Person["age"];

//type Age = number;
```

The indexing type is itself a type, so we can use unions, keyof, or other types entirely:

```typescript
type I1 = Person["age" | "name"];

type I1 = string | number;

type I2 = Person[keyof Person];

type I2 = string | number | boolean;

type AliveOrName = "alive" | "name";
type I3 = Person[AliveOrName];

type I3 = string | boolean;
```

Another example of indexing with an arbitrary type is using number to get the type of an array’s elements. We can combine this with typeof to conveniently capture the element type of an array literal:

```typescript
const MyArray = [
  { name: "Alice", age: 15 },
  { name: "Bob", age: 23 },
  { name: "Eve", age: 38 },
];

type Person = (typeof MyArray)[number];

// type Person = {
//   name: string;
//   age: number;
// };
```

## Mapped types

A mapped type is a generic type which uses a union of PropertyKeys (frequently created via a keyof) to iterate through keys to create a type:

```typescript
type OptionsFlags<Type> = {
  [Property in keyof Type]: boolean;
};

type Features = {
  darkMode: () => void;
  newUserProfile: () => void;
};

type FeatureOptions = OptionsFlags<Features>;

// type FeatureOptions = {
//     darkMode: boolean;
//     newUserProfile: boolean;
// }
```

### Mapping modifiers

There are two additional modifiers which can be applied during mapping: readonly and ? which affect mutability and optionality respectively.

You can remove or add these modifiers by prefixing with - or +. If you don’t add a prefix, then + is assumed.

```typescript
// Removes 'readonly' attributes from a type's properties
type CreateMutable<Type> = {
  -readonly [Property in keyof Type]: Type[Property];
};

type LockedAccount = {
  readonly id: string;
  readonly name: string;
};

type UnlockedAccount = CreateMutable<LockedAccount>;

type UnlockedAccount = {
  id: string;
  name: string;
};
```

## Generic Constraints

Typescript cannot narrow down the property type in this case:

```typescript
const testObj = { x: 10, y: "Hello", z: true };

function getProperty<T>(obj: T, key: keyof T) {
  return obj[key];
}

const xValue = getProperty(testObj, "x");
// const xValue: string | number | boolean

const yValue = getProperty(testObj, "y");
// const yValue: string | number | boolean
```

Use `extends` as generic Constraints

```typescript
const testObj = { x: 10, y: "Hello", z: true };

function getProperty<T, K extends keyof T>(obj: T, key: K) {
  return obj[key];
}

const xValue = getProperty(testObj, "x");
// const xValue: number
const yValue = getProperty(testObj, "y");
// const yValue: string
```

## Conditional types

```typescript
type IsString<T> = T extends string ? true : false;
```
