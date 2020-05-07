# Initialize a new Gatsby project with a gitignore in yarn workspaces

**[📹 Video](https://egghead.io/lessons/gatsby-initialize-a-new-gatsby-project-with-a-gitignore-in-yarn-workspaces?pl=building-a-serverless-jamstack-todo-app-with-netlify-gatsby-graphql-and-faunadb-53bb)**

**[💻 Code](https://github.com/christopherbiscardi/serverless-todo-netlify-fauna-egghead/tree/ef1942a300bc248a27e5667dd6ce61194837d30d)**

**Objective**: Let's setup `yarn workspaces`.

We will be working on a `gatsby` project using a `yarn workspaces` setup.

* `yarn init -y`

Initialize a new project using `yarn`. The `-y` flag is to get the default `package.json` setup.

```js
{
  "name": "My amazing app",
  "version": "1.0.0",
  "main": "index.js",
  "repository": "github@repo",
  "author": "ME",
  "license": "MIT"
}
```

👍 If you're using VSCode as your text editor, you can open the contents of your current folder (in VSCode) with:

* `code .`

🤔 Notetaker's addition: [`yarn workspaces`](https://classic.yarnpkg.com/en/docs/workspaces/): are used when you're working with multiple packages in a single project. With `yarn workspaces` you can:

-  set up a single `node_modules` without repetitions.
- change the code of one of your packages and have those changes instantly visible to the other packages that use it.

Add some extra configuration to the `package.json` file in order to use `yarn workspaces`:

```js
// required in your yarn workspace root package.json
  "private": true,

// pointing to any npm package inside the packages directory
  "workspaces": [
    "packages/*"
  ]
```

Create a `packages` directory. Create another directory inside `packages` called `www` and initialize a new project with:

* `yarn init -y`

Your project's tree structure should look like this:
```
├── README.md
├── package.json
└── packages
    └── www
        └── package.json
```

Install dependencies:

* `yarn add gatsby react react-dom`

The `package.json` inside `www` should now look like this:

```js
{
  "name": "www",
  "version": "1.0.0",
  "main": "index.js",
  "license": "MIT",
  "dependencies": {
    "gatsby": "^2.21.9",
    "react": "^16.13.1",
    "react-dom": "^16.13.1"
  }
}
```

Add a couple of scripts:

```js
  "scripts": {
    "develop": "gatsby develop",
    "build": "gatsby build",
    "clean": "gatsby clean"
  },

```

`develop` for development, `build` for production and `clean` will remove any caches.

Create a `gatsby-config.js` file at the root of your `gatsby` project.

```js
module.exports = {};
```

Then create a `src/pages/index.js` (you'll have to create the two directories along with the file).

Add the following to the `index.js` file (don't forget to import `React`!):

```js
import React from "react";
export default props => (
  <div>
    <h1>My Amazing Site</h1>
  </div>
);
```

Which exports a simple `react` component.

To run the project, go to the root of your project and type:

* `yarn workspace www develop`


 Your site will now be live on: `http://localhost:8000/`

Create a `.gitignore` file at the root of the project and add:

```bash
.cache
public
```

 Push to Github.

 * `git browse`

 👍 To open the repository in the browser (if you've installed `hub`).