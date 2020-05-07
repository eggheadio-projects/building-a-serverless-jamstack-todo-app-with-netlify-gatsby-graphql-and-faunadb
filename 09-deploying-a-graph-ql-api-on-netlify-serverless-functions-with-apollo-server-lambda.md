# Deploying a GraphQL API on Netlify serverless functions with apollo-server-lambda

**[📹 Video](https://egghead.io/lessons/apollo-deploying-a-graphql-api-on-netlify-serverless-functions-with-apollo-server-lambda?pl=building-a-serverless-jamstack-todo-app-with-netlify-gatsby-graphql-and-faunadb-53bb)**

**[💻 Code](https://github.com/christopherbiscardi/serverless-todo-netlify-fauna-egghead/tree/f62e42fca9d6c198c9a21e36bdddcde342019652)**

**Objective:**: Until now, all of our changes have been stored in the browser's memory only - which means that all of our todos will disappear once we refresh the browser. Let's change that!

To fix that we'll use a `graphql` server inside a `netlify` function.

* Create: `functions/graphql/` in the root of our project.

* Then run `yarn init -y` to get a new `package.json`.

👍 Since we're adding this project outside of the `packages` folder, it won't be part of our `yarn worskpaces` - that's because this package will deal with its own dependencies.

Add two dependencies:

* `yarn add apollo-server-lambda graphql`

👍 In your `package.json`, make sure that you don't name your app `graphql` as that will conflict with the `graphql` dependency.

* Create the file: `functions/graphql/graphql.js`

Add the following contents:

(at this point, the only thing this server will know how to do is return `Hello World` when queried with `hello`)

```js
// import the two dependencies
const { ApolloServer, gql } = require('apollo-server-lambda');

// Construct a schema, using GraphQL schema language
// Define a single root query type, which will return a string
const typeDefs = gql`
  type Query {
    hello: String
  }
`;

// Provide resolver functions for your schema fields
const resolvers = {
  Query: {
    hello: () => 'Hello world!',
  },
};

const server = new ApolloServer({
  typeDefs,
  resolvers,

  playground: true,
  introspection: true,
});

// AWS compatible function signature (Netlify functions are built on top of AWS functions)
exports.handler = server.createHandler();
```

Make sure to add functions to your `Netlify` dashboard too. Go to `Netlify` dashboard, **Settings** then **Functions** and set the **Functions directory** to `functions`.

Commit and push (so that `netlify` is aware of the functions).

🤔 You can learn more about `netlify` functions [here](https://docs.netlify.com/functions/overview/).

Now that our function is deployed, we'll have to update our build script in `netlify` (to install the dependencies).

* `yarn worskpace www build && cd functions/graphql && yarn`

👍 If you have more than one function, it's better to istall the dependencie via a `Makefile` (What's a [Makefile](https://opensource.com/article/18/8/what-how-makefile))

Once built, find the **Functions** endpoint in the **Netlify** dashboard. Clicking on it will take you to the `graphql` interactive playground.

Querrying for:

```js
{
  hello
}
```

should now return:

```js
{
  "data": {
    "hello": "Hello world!"
  }
}
```