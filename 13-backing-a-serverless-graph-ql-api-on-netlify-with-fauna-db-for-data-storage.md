# Backing a Serverless GraphQL API on Netlify with FaunaDB for data storage

**[📹 Video](https://egghead.io/lessons/graphql-backing-a-serverless-graphql-api-on-netlify-with-faunadb-for-data-storage?pl=building-a-serverless-jamstack-todo-app-with-netlify-gatsby-graphql-and-faunadb-53bb)**

**[💻 Code](https://github.com/christopherbiscardi/serverless-todo-netlify-fauna-egghead/tree/4d1208f06a2b31bba0cae6a76fa821cf0e0b128f)**

**Objective:** Time to persist our data with a database called [FaunaDB](https://fauna.com/)!

* `yarn add faunadb` (make sure to add it to `functions/graphql/package.json`)

Create a free account, then create a new database and add a new `todos` collection.

We're not entirely sure how we'll interact with our data yet. Let's create a test directory with an `index.js` file in it : `packages/www/fauna-test/index.js`


```js
const faunadb = require("faunadb");
const q = faunadb.query;

var client = new faunadb.Client({ secret: process.env.FAUNA });
```

We'll have to initialize a new faunadb key, which we will put into a file that stores our environmental variables. To get the key, go to the FaunaDB console, click **Security** and create a new **Server** key.

👍 You can store the key in an `.envrc` file if you use [`direnv`](https://github.com/direnv/direnv) (which Chris recommends), or you can use [`dotenv`](https://www.npmjs.com/package/dotenv) npm package.

Go [here](https://github.com/ChristopherBiscardi/serverless-todo-netlify-fauna-egghead/blob/765d1b81da0e03f81d716bda4717159e5a3aa175/packages/www/fauna-test/index.js) for the full contents of this `index.js` file.

To add data to our collection, we'll use the Create query:

```js
const results = await client.query(
  q.Create(q.Collection("todos"), {
    data: {
      text: "my super cool todo!",
      done: false,
      owner: "user-test-1"
    }
  })
);
// to get the todo's id
console.log(results.ref.id);
```

Once you run `node index.js` you should see your data in the `faunadb` console.

To mark one of the todos as done, we'll use the Update query:

```js
const results = await client.query(
  q.Update(q.Ref(q.Collection("todos"), "256957931386307081"), {
    data: {
      done: true
    }
  })
);
console.log(results);
  ```

  Back in `faunaDB` We'll create a **New Index** called `todos_by_user` - so that we can return todos for a specific user only (using the Paginate query).

  ```js
  const results = await client.query(
    q.Paginate(q.Match(q.Index("todos_by_user"), "user-test"))
  );
  console.log(results);
  ```

  Returns a JSON object `data` that contains our `results` array.

  Next, we need to update our `graphql.js` with the new `faunadb` querying.

  Head over [here](https://github.com/ChristopherBiscardi/serverless-todo-netlify-fauna-egghead/blob/765d1b81da0e03f81d716bda4717159e5a3aa175/functions/graphql/graphql.js) for the full contents of the file.

  Before we deploy, we need to add the env variable to `netlify`. In the Netlify console, go to **Settings**, **Build & Deploy**, then **Environment**, and add the `FAUNA` key.

  Congratulations! You have finished the course 🎉!