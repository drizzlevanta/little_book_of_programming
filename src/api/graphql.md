# GraphQL

## GraphQL Error Design

### Two types of approaches:

### References

Two good articles on best practices on GraphQL error design:

- [A Guide to GraphQL Errors](https://productionreadygraphql.com/2020-08-01-guide-to-graphql-errors)
- [The Good, the Bad and the Ugly](https://escape.tech/blog/graphql-errors-the-good-the-bad-and-the-ugly/)

### Best Practices

- The `error` field: The `error` field has no schema. A potential issue with the `error` field is that when they are present, the corresponding field should be null. This can potentially be a deal breaker if you're wanting to have errors returned as part of a mutation, but want to query for data on the result anyways. A common example of this is the server sending back the actual state of a resource after a mutation that had errors. With top-level errors this can't be done since the whole mutation field should be `null!`.
- GraphQL errors encode **exceptional** scenarios: like a service being down, unauthentication, rate limit, timeout, syntax, or some other internal failure.
- Errors which are part of the API domain should be captured within that domain.
- User facing or actionable errors should be returned as data.

In summary:

- Critical errors that cannot be fixed by clients (e.g. a database error) - returned in `error` field
- Recoverable errors that can be fixed by clients (e.g. invalid input data) - returned as `data`

### Use Union Result Types

Example:

```rust
type Mutation {
  register(email: String!, password: String!): RegisterResult!
}

// Use different types for all possible actionable errors
union RegisterResult = User | ValidationError | ProfessionalEmailRequired

type User {
  id: ID!
  email: String!
}

// Malformed inputs
type ValidationError {
  // Allow several errors at the same time!
  fieldErrors: [FieldError!]!
}

type FieldError {
  path: String! // Shows where the errors are to be displayed in UI
  message: String!
}

// Business specific errors (e.g. banned email providers)
type ProfessionalEmailRequired {
  provider: String!
}
```
