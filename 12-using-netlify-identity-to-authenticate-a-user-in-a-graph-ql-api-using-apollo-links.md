# Using Netlify Identity to authenticate a user in a GraphQL API using apollo links

**[📹 Video](https://egghead.io/lessons/netlify-using-netlify-identity-to-authenticate-a-user-in-a-graphql-api-using-apollo-links?pl=building-a-serverless-jamstack-todo-app-with-netlify-gatsby-graphql-and-faunadb-53bb)**

**[💻 Code](https://github.com/christopherbiscardi/serverless-todo-netlify-fauna-egghead/tree/e013a3719e5ace08cab1e0d658338520f875ad1f)**

**Objective:** Currently, a logged-in user is able to see all the todos (from other users too). That's because our server isn't aware of the authentication. Let's change that!

Run:

* `yarn add apollo-link-context` to get the `setContext` function

We need a valid bearer token with the authorization object to manage access control on the todos.

Go to `gatsby-browser.js` and add:

* `const { setContext } = require("apollo-link-context");`

and

* `const netlifyIdentity = require("netlify-identity-widget");`

Add the `authLink`:

```js
const authLink = setContext((_, { headers }) => {
  const user = netlifyIdentity.currentUser();
  const token = user.token.access_token;

  // return the headers to the context so httpLink can read them
  return {
    headers: {
      ...headers,
      Authorization: token ? `Bearer ${token}` : ""
    }
  };
});

```

and move out the `httpLink`:

```js
const httpLink = new HttpLink({
  uri:
    "https://serverless-todo-netlify-fauna-egghead.netlify.com/.netlify/functions/graphql"
});
const client = new ApolloClient({
  cache: new InMemoryCache(),
  link: authLink.concat(httpLink)
});
```

The `gatsby-browser.js` file should look like [this](https://github.com/ChristopherBiscardi/serverless-todo-netlify-fauna-egghead/blob/e013a3719e5ace08cab1e0d658338520f875ad1f/packages/www/gatsby-browser.js).

We'll have to make some changes to the `graphql.js` file as well:

```js
const resolvers = {
  Query: {
    todos: (parent, args, { user }) => {
      if (!user) {
        return [];
      } else {
        return Object.values(todos);
      }
    }
  },
```

```js
  context: ({ context }) => {
    if (context.clientContext.user) {
      return { user: context.clientContext.user.sub };
    } else {
      return {};
    }
  }
```

From now only requests with a valid bearer token will return todos. You can check whether the bearer token was succesfuly added by going to the **Network** tab in your browser's developer tools. There should be a new `authorization: Bearer` section in the request's `Header`.