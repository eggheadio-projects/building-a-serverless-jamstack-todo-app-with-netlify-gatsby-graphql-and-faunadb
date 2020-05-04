# Using Netlify Identity to control access to a Gatsby client-side dashboard route

**[📹 Video](https://egghead.io/lessons/gatsby-using-netlify-identity-to-control-access-to-a-gatsby-client-side-dashboard-route?pl=building-a-serverless-jamstack-todo-app-with-netlify-gatsby-graphql-and-faunadb-53bb)**

**[💻 Code](https://github.com/christopherbiscardi/serverless-todo-netlify-fauna-egghead/tree/0f164ce70ff02f7aedaa3020a15074575eda0b90)**

**Objective**: Let's add a dashboard to our app that will only be available to logged in users.

```js
import { Router, Link } from "@reach/router";` to `index.js`
```

then import [`NavLink`](https://theme-ui.com/components/nav-link/) from `theme-ui`.

We'll be adding a link to `Home`, `Dashboard` and `Log out` (for logged in users only):

```js
<Flex as="nav">
  <NavLink as={Link} to="/" p={2}>
    Home
  </NavLink>
  <NavLink as={Link} to={"/app"} p={2}>
    Dashboard
  </NavLink>
  {user && (
    <NavLink
      href="#!"
      p={2}
      onClick={() => {
        netlifyIdentity.logout();
      }}
    >
      Log out {user.user_metadata.full_name}
    </NavLink>
  )}
</Flex>
```

We'll improve our `useEffect` function from last time, to allow for `login` and `logout` events. We'll be setting the `user` to the current user on login, and clearing the `user` on logout.

```js
  React.useEffect(() => {
    netlifyIdentity.init({});
  });
  netlifyIdentity.on("login", user => {
    setUser(user);
  });
  netlifyIdentity.on("logout", () => {
    setUser();
  });
```

To create the dashboard page, we'll install a `gatsby` plugin:

* `yarn add gatsby-plugin-create-client-paths`

Whenever you add a `gatsby` plugin, you need to configure it in `gatsby-config`:

```js
module.exports = {
  plugins: [
    {
      resolve: `gatsby-plugin-create-client-paths`,

      // we'll let the plugin handle everything under the app prefix
      options: { prefixes: [`/app/*`] }
    }
  ]
};
```

Let's create the entry point to our client-side app: `packages/www/src/pages/app.js`

We'll use [Reach Router](https://reach.tech/router) which ships with `gatsby`.

The `app.js` file also requires the `Identity` init logic (like in `index.js`), but to keep our code DRY, we'll pull this logic out into a separate file using `React Context`.

Create a file: `packages/www/identity-context.js`

```js
const React = require("react");
const netlifyIdentity = require("netlify-identity-widget");

const IdentityContext = React.createContext({});

exports.IdentityContext = IdentityContext;

const IdentityProvider = props => {
  const [user, setUser] = React.useState();

  React.useEffect(() => {
    netlifyIdentity.init({});
  });
  netlifyIdentity.on("login", user => {
    setUser(user);
  });
  netlifyIdentity.on("logout", () => setUser());

  return (
    <IdentityContext.Provider value={{ identity: netlifyIdentity, user }}>
      {props.children}
    </IdentityContext.Provider>
  );
};

exports.Provider = IdentityProvider;
```

👍 Make sure to remove the `Identity` code from `index.js`. (Go [here](https://github.com/ChristopherBiscardi/serverless-todo-netlify-fauna-egghead/blob/0f164ce70ff02f7aedaa3020a15074575eda0b90/packages/www/src/pages/index.js) to see the entire code for `index.js`)

Next, add the provider to the `wrap-root-element`:

`const { Provider } = require("./identity-context");`

Wrap the `ThemeProvider`:

```js
 <Provider>
    <ThemeProvider theme={tokens}>{element}</ThemeProvider>
  </Provider>
```

We'll have to add the changes to `app.js` as well. You can check out the full code [here](https://github.com/ChristopherBiscardi/serverless-todo-netlify-fauna-egghead/blob/66442bb589c918eb0c776dbcc9675ea00da65a63/packages/www/src/pages/app.js).
