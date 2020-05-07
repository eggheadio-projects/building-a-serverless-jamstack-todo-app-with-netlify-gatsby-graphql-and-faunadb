# Taking advantage of theme-ui to build a landing page on a Gatsby project without plugins

**[📹 Video](https://egghead.io/lessons/gatsby-taking-advantage-of-theme-ui-to-build-a-landing-page-on-a-gatsby-project-without-plugins?pl=building-a-serverless-jamstack-todo-app-with-netlify-gatsby-graphql-and-faunadb-53bb)**

**[💻 Code](https://github.com/christopherbiscardi/serverless-todo-netlify-fauna-egghead/tree/43b5114e0b7cb055be0aa814cf895ce036eac6fe)**

**Objective**: Let's add [Theme UI's](https://theme-ui.com/) `deep` preset for styling (this will make our site look pretty in no time!).

Navigate to the `www` directory and add two dependencies:

* `yarn add theme-ui @theme-ui/presets`

👍 if you run into an integrity issue, run `yarn cache clean` (I didn't, but Chris did)

* Create a file: `packages/www/wrap-root-element.js`

With the following contents (copied from the `Theme UI` docs):

```js
const React = require("react");
const { ThemeProvider } = require("theme-ui");

// since we know which preset we'll be using, we can import deep directly
const { deep } = require("@theme-ui/presets");

// We need to add the container's max-width to the existing presets using object spread
const tokens = {
  ...deep,
  sizes: { container: 1024 },
};

// change prop to element to work with Gatsby's wrapRootElement
module.exports = ({ element }) => (
  <ThemeProvider theme={tokens}>{element}</ThemeProvider>
);
```

Create `packages/www/gatsby-ssr.js` and `packages/www/gatsby-browser.js` (they will have the same contents):

```js
const React = require("react");
const wrapRootElement = require("./wrap-root-element");

exports.wrapRootElement = wrapRootElement;
```

🤔 Notetaker's addition: "[`gatsby-ssr.js`](https://www.gatsbyjs.org/docs/api-files-gatsby-ssr/) lets you alter the content of static HTML files as they are being Server-Side Rendered (SSR) by Gatsby and Node.js.

The above settings will allow for our theme to be available throughout the entire project.

Let's add our theme wrapper to our `index.js` page:

```js
import React from "react";
import { Container, Heading, Button, Flex } from "theme-ui";
export default props => (
  <Container>
    <Flex sx={{ flexDirection: "column", padding: 3 }}>
      <Heading as="h1">Get Stuff Done</Heading>
      <Button
        sx={{ marginTop: 2 }}
        onClick={() => {
          alert("clicked");
        }}
      >
        Log In
      </Button>
    </Flex>
  </Container>
);
```

We've added a click handler to the button, which will pop out an alert when clicked.

* We can add any custom styling to the `sx` prop.

* `add`, `commit` and `push` changes to GitHub!