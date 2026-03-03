## Core Concepts

### 1. The Schema & Type System

Everything in GraphQL starts with a **Schema**. It defines what data is available and the relationships between different data types using the Schema Definition Language (SDL).

```graphql
type Author {
  id: ID!
  name: String
  books: [Book]
}

type Book {
  title: String
  genre: String
}
```

### 2. Queries (Read)

In GraphQL, the client defines the structure of the response. This prevents **Over-fetching** (getting more data than you need) and **Under-fetching** (not getting enough, leading to multiple API calls).

### 3. Mutations (Write)

While queries are for fetching data, **Mutations** are used to create, update, or delete data. They follow a similar syntax to queries, allowing you to get the updated object back in the same request.

### 4. Resolvers

The "brain" of the operation. Resolvers are functions on the server that tell GraphQL how to fetch the data for a specific field, whether it’s from a database, a third-party API, or a cache.

---

## GraphQL vs. REST: Key Differences

| Feature           | REST                                    | GraphQL                                |
| ----------------- | --------------------------------------- | -------------------------------------- |
| **Data Fetching** | Multiple endpoints (`/users`, `/posts`) | Single endpoint (`/graphql`)           |
| **Payload**       | Fixed by the server                     | Defined by the client                  |
| **Versioning**    | Often requires versioned URLs (v1, v2)  | "Versionless" (just add new fields)    |
| **Errors**        | Uses HTTP status codes (404, 500)       | Always returns 200 OK (errors in body) |

---

## Why Use It? (And Why Not?)

### The Pros

- **Performance:** Drastically reduces the number of network requests.
- **Strongly Typed:** The schema acts as a contract between the frontend and backend, reducing integration bugs.
- **Developer Experience:** Tools like **GraphiQL** or **Apollo Studio** allow you to explore the API and test queries with auto-complete.

### The Cons

- **Complexity:** It adds a layer of abstraction that might be overkill for simple applications.
- **Caching:** Because every request is a `POST` to a single endpoint, standard HTTP caching (like browser or CDN caching) is harder to implement.
- **N+1 Problem:** If not handled correctly with tools like **DataLoader**, fetching nested data can trigger hundreds of database queries.

---

## Popular Ecosystem Tools

- **Apollo Client/Server:** The most popular suite for managing GraphQL data.
- **Relay:** A high-performance framework by Meta for React.
- **Hasura / Prisma:** Tools that auto-generate GraphQL APIs from your database.
