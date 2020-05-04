# Implementing the types and resolvers for a serverless GraphQL Todo API

**[📹 Video](https://egghead.io/lessons/graphql-implementing-the-types-and-resolvers-for-a-serverless-graphql-todo-api?pl=building-a-serverless-jamstack-todo-app-with-netlify-gatsby-graphql-and-faunadb-53bb)**

**[💻 Code](https://github.com/christopherbiscardi/serverless-todo-netlify-fauna-egghead/tree/db36c6518b5cbf130b27b21cd2e3d6e2363bf110)**

**Objective:** our server should handle: add todos, display todos and toggle todos.

Our server logic lives in `functions/graphql/graphql.js`.

```js
  type Query {
    // returns an array of todos
    // The exclamation mark means it's required.
    todos: [Todo]!
  }
  type Todo {
    // we'll use the id to reference a specific todo
    id: ID!
    text: String!
    done: Boolean!
  }
  type Mutation {
    addTodo(text: String!): Todo
    updateTodoDone(id: ID!): Todo
  }

const todos = {};
let todoIndex = 0;
// Provide resolver functions for your schema fields
const resolvers = {
  Query: {
    todos: () => {
      // return all todos
      return Object.values(todos);
    }
  },
  Mutation: {
    // we'll be ignoring the first argument
    addTodo: (_, { text }) => {
      todoIndex++;
      const id = `key-${todoIndex}`;
      // each todo will consist of an id, the text and a toggle value done: true, done: false
      todos[id] = { id, text, done: false };
      return todos[id];
    },
    updateTodoDone: (_, { id }) => {
      todos[id].done = true;
      return todos[id];
    }
  }
};
```

To see the final `graphql.js` code, head over [here](https://github.com/ChristopherBiscardi/serverless-todo-netlify-fauna-egghead/blob/db36c6518b5cbf130b27b21cd2e3d6e2363bf110/functions/graphql/graphql.js).

You can test whether your query is valid in the `graphql` playground.

```js
{
  todos {
    id
    text
    done
  }
}
```

Should return:

```js
{
  "data": {
    "todos": []
  }
}
```