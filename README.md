# Building APIs in Next.js

## Concepts

In Next.js, there is a concept of API Routes, a solution to build your API with Next.js. 

Files inside the `pages/api` folder are mapped to `/api/*` and are treated as an API endpoint.


## Getting Started 

Create a Next.js project with TypeScript:

```
pnpm create next-app --typescript
```

The following API route `pages/api/post.ts` returns a JSON response with a status code of `200` with a list of of posts:

```tsx
import type { NextRequest } from 'next/server'

export const config = {
  runtime: 'experimental-edge',
}

export default async function handler(request: NextRequest) {
  return new Response(
    JSON.stringify({
      name: "Hi, I'm Zaiste",
    }),
    {
      status: 200,
      headers: {
        'content-type': 'application/json',
      },
    }
  )
}
```

You can use the [retes](https://github.com/kreteshq/retes) library to simplify this code

```
import type { NextRequest } from 'next/server'

export const config = {
  runtime: 'experimental-edge',
}

export default async function handler(request: NextRequest) {
  return Response.OK({ name: "Hi, I'm Zaiste" });
}
```

## REST



## GraphQL

`next-graphql-server` is an easy to use Next.js library for creating performant GraphQL endpoints on top of [Next.js API Routes](https://nextjs.org/docs/api-routes/introduction).

Start building GraphQL servers with Next.js.


Add `next-graphql-server` as a dependency to your Next.js project:

```
pnpm add next-graphql-server
```

## Usage

`next-graphql-server` uses [Next.js API Routes](https://nextjs.org/docs/api-routes/introduction). Create the `pages/api/graphql.ts` with the following content:

**Note**: The file in `pages/api` must be named `graphql`

### with `graphql`

Add `graphql` 

```
pnpm add graphql
```

```ts
import { createGraphQLHandler } from "next-graphql-server";
import {
  GraphQLObjectType,
  GraphQLSchema,
  GraphQLString,
} from "graphql";

const schema = new GraphQLSchema({
  query: new GraphQLObjectType({
    name: "Query",
    fields: () => ({
      hello: {
        type: GraphQLString,
        resolve: () => "world",
      },
    }),
  }),
});

const handler = createGraphQLHandler(schema);
export default handler;
```

### with `@graphql-tools` 

Add `@graphql-tools/schema`

```
pnpm add @graphql-tools/schema
```

In `pages/api/graphql.ts` define your handler as shown below:

```ts
import { createGraphQLHandler } from "next-graphql-server";
import { makeExecutableSchema } from "@graphql-tools/schema";

export const schema = makeExecutableSchema({
  typeDefs: /* GraphQL */ `
    type Query {
      hello: String!
    }
  `,
  resolvers: {
    Query: {
      hello: () => 'World',
    },
  },
});

const handler = createGraphQLHandler(schema);
export default handler;
```

### with Pothos

```tsx
import SchemaBuilder from 'pothos';

const builder = new SchemaBuilder({});

builder.queryType({
  fields: (t) => ({
    hello: t.string({
      args: {
        name: t.arg.string(),
      },
      resolve: (_parent, { name }) => `Hello, ${name || 'World'}`,
    }),
  }),
});


const schema = builder.toSchema();

const handler = createGraphQLHandler(schema);
export default handler;
```
