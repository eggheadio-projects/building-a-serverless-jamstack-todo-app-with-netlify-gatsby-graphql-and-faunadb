# Connecting a Todo app to a serverless GraphQL API using Apollo Client and solving CORS

**[📹 Video](https://egghead.io/lessons/apollo-connecting-a-todo-app-to-a-serverless-graphql-api-using-apollo-client-and-solving-cors?pl=building-a-serverless-jamstack-todo-app-with-netlify-gatsby-graphql-and-faunadb-53bb)**

**[💻 Code](https://github.com/christopherbiscardi/serverless-todo-netlify-fauna-egghead/tree/81568a11fa45c4c3e896761e965554234dd0a7c2)**

**Objective:** Connect our client to the `graphql` API using Apollo Client.

🤔 Notetaker's addition: What is [Apollo Client](https://www.apollographql.com/blog/apollo-client-1-0-a-flexible-community-focused-javascript-graphql-client-2253b90e6c84) In short: "Apollo Client is a client-side library that leverages the power of a GraphQL API to handle data fetching and management for you, so that you can spend less time plumbing data and more on building your application."

* Run: `yarn add @apollo/client` in the `www` folder.

* Modify: `packages/www/gatsby-browser.js` and import an `apollo/client` instance:

```js
const {
  ApolloProvider,
  ApolloClient,
  HttpLink,
  InMemoryCache
} = require("@apollo/client");
```

Configure the client:

```js
const client = new ApolloClient({
  cache: new InMemoryCache(),
  link: new HttpLink({
    uri:
    // add your netlify functions url
      "https://[netlify-url].netlify.app/.netlify/functions/graphql"
  })
});
```

Set up the `ApolloProvider`. This will allow us to use `useQuery` and `useMutation`.

```js
exports.wrapRootElement = ({ element }) => {
  return (
    <ApolloProvider client={client}>
      {wrapRootElement({ element })}
    </ApolloProvider>
  );
};
```

Back in our `dashboard.js` we'll have to replace the reducer actions with mutations.

* We'll import `import { gql, useMutation, useQuery } from "@apollo/client";`

* We'll have to write three mutations: `ADD_TODO`, `GET_TODOS` and `UPDATE_TODO_DONE`.

Head over [here](https://github.com/ChristopherBiscardi/serverless-todo-netlify-fauna-egghead/blob/81568a11fa45c4c3e896761e965554234dd0a7c2/packages/www/src/components/dashboard.js) for the complete `dashboard.js`.

To enable CORS (Cross-Origin Resource Sharing), add these options to the `graphql.js` handler:
```js
exports.handler = server.createHandler({
  cors: {
    origin: "*",
    credentials: true
  }
});
```

Then commit just the `graphqls.js` file first (don't commit the rest of the changes until you have done that). Once that is successfully deployed, commit, and push the rest.