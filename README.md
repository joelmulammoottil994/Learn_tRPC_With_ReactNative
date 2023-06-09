# tRPC_With_ReactNative
tRPC can be used with React Native to build end-to-end typesafe APIs . You can use tRPC to define your API's procedures using TypeScript functions and then use the generated types to ensure that your client-side code is calling your API correctly.

Here's an example of how you might set up tRPC in a React Native app:

1. Install the `@trpc/client`, `@trpc/server`, and `@trpc/react-query` packages by running the following command:
```sh
npm install @trpc/client @trpc/server @trpc/react-query
```
2. In your code, import the `createTRPCReact` function from the `@trpc/react-query` package and create a `trpc` object by calling this function with your API's type signature:
```javascript
import { createTRPCReact } from '@trpc/react-query';
import type { AppRouter } from '../path/to/router';

export const trpc = createTRPCReact<AppRouter>();
```
3. Use the `trpc` object to create strongly-typed hooks for calling your API's procedures. For example:
```javascript
import React from 'react';
import { View, Text } from 'react-native';
import { trpc } from './utils/trpc';

const MyComponent = () => {
  const hello = trpc.hello.useQuery({ text: 'client' });

  if (!hello.data) return <Text>Loading...</Text>;

  return <Text>{hello.data.greeting}</Text>;
};

```

In this example, we use the `useQuery` hook generated by tRPC to call the `hello` procedure on our API and display its result.

Here's a small example of how you might use tRPC with React Native to display a greeting from an API:

```javascript
import React from 'react';
import { View, Text } from 'react-native';
import { createTRPCReactNative } from '@trpc/react-query';
import type { AppRouter } from '../path/to/router';

const trpc = createTRPCReactNative<AppRouter>();

const MyComponent = () => {
  const hello = trpc.hello.useQuery({ text: 'client' });

  if (!hello.data) return <Text>Loading...</Text>;

  return <Text>{hello.data.greeting}</Text>;
};
```

In this example, we use the `createTRPCReactNative` function to create a `trpc` object with our API's type signature. We then use the `useQuery` hook generated by tRPC to call the `hello` procedure on our API and display its result using a `Text` component from React Native.

`AppRouter` is a TypeScript type that represents the type signature of your tRPC API. It is used to define the shape of your API's procedures and their input and output types.

Here's an example of how you might define an `AppRouter` type for a simple tRPC API:

```typescript
import { z } from 'zod';
import { createRouter } from '@trpc/server';

export const appRouter = createRouter()
  .query('hello', {
    input: z.object({
      text: z.string(),
    }),
    resolve({ input }) {
      return {
        greeting: `Hello ${input.text}`,
      };
    },
  });

export type AppRouter = typeof appRouter;
```

In this example, we use the `createRouter` function from the `@trpc/server` package to create a tRPC router with a single `hello` query. This query takes an input object with a `text` property and returns an object with a `greeting` property.

We then use the `typeof` keyword to define the `AppRouter` type as the type of our `appRouter` object. This allows us to use the `AppRouter` type to represent the type signature of our API when creating our `trpc` object in our client-side code.

