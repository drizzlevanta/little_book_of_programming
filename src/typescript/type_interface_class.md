# Type, Interface and Class

## Record vs Map

Contrary to a Record which is a type, a Map is a full-fledged data structure for storing the key-value pairs. It provides dedicated methods for adding, retrieving, and deleting entries from the Map.

## `Record<string, string>`

`Record<string, string>` is semantically equivalent to index signature

```typescript
{
  [key: string]: string;
};
```

However, with the index signature, one can customize it further (e.g., restrict certain key names or mix other properties), while Record is strictly a mapping.

```typescript
type Example3 = {
  [key: string]: string;
  specialKey?: number; // Adds a specific key with its own type
};
```

When to use each:

- Use `Record<string, string>` for clean, simple mappings with no additional properties.
- Use `{ [key: string]: string }` when you need flexibility or additional customization.
