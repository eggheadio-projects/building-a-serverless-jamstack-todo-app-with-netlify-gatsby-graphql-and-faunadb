# Fixing the Netlify Identity Widget when it prevents clicks after login

**[📹 Video](https://egghead.io/lessons/netlify-fixing-the-netlify-identity-widget-when-it-prevents-clicks-after-login?pl=building-a-serverless-jamstack-todo-app-with-netlify-gatsby-graphql-and-faunadb-53bb)**

**[💻 Code](https://github.com/christopherbiscardi/serverless-todo-netlify-fauna-egghead/tree/93da6fac63fde4d1a5024412272a2b91616e30b2)**

**Objective**:Let's make our `Identity` widget a little bit less buggy (and actually clickable), by closing it everytime after we've finished interacting with it.

Our `useEffect` code should look like this:

```js
  React.useEffect(() => {
    netlifyIdentity.init({});
  });
  netlifyIdentity.on("login", user => {
    netlifyIdentity.close();
    setUser(user);
  });
  netlifyIdentity.on("logout", () => {
    netlifyIdentity.close();
    setUser();
  });
```

Go [here](https://github.com/ChristopherBiscardi/serverless-todo-netlify-fauna-egghead/blob/93da6fac63fde4d1a5024412272a2b91616e30b2/packages/www/identity-context.js) to see the entire code for `identity-context.js`.