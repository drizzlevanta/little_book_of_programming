# Immutability
## Static
ES6 includes static members and so does TypeScript. The static members of a class are accessed using the class name and dot notation, without creating an object e.g. <ClassName>.<StaticMember>.

The static members can be defined by using the keyword static. Consider the following example of a class with static property.

## Const vs Readonly
A const variable cannot be re-assigned, just like a readonly property.

Essentially, when you define a **property**, you can use readonly to prevent re-assignment. This is actually only a compile-time check.

When you define a const **variable** (and target a more recent version of JavaScript to preserve const in the output), the check is also made at runtime.

However, "cannot be re-assigned" is not the same as immutability.
```typescript
const myArr = [1, 2, 3];

// Not allowed
myArr = [4, 5, 6]

// Perfectly fine
myArr.push(4);

// Perfectly fine
myArr[0] = 9;
```

## Readonly Array
When you declare any array as const, you can perform operations on array which may change the array elements. for ex.
```typescript

const Arr = [1,2,3];

Arr[0] = 10;   //OK
Arr.push(12); // OK
Arr.pop(); //Ok

//But
Arr = [4,5,6] // ERROR

```
But in case of readonly Array you can not change the array as shown above.

```typescript

arr1 : readonly Array<number> = [10,11,12];

arr1.pop();    //ERROR
arr1.push(15); //ERROR
arr1[0] = 1;   //ERROR
```