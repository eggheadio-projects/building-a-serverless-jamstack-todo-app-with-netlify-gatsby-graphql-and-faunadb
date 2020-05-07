# Implementing Authentication with Netlify Identity, React hooks, and Gatsby

**[📹 Video](https://egghead.io/lessons/react-implementing-authentication-with-netlify-identity-react-hooks-and-gatsby?pl=building-a-serverless-jamstack-todo-app-with-netlify-gatsby-graphql-and-faunadb-53bb)**

**[💻 Code](https://github.com/christopherbiscardi/serverless-todo-netlify-fauna-egghead/tree/eaa5f81458f76a0e0e15e0c3545aacfb9f7fb4c0)**

**Objective**: Let's add [Netlify Identity](https://docs.netlify.com/visitor-access/identity/) to authenticate our users.

Netlify Identify is based on [GoTrue](https://github.com/netlify/gotrue) API. We'll be using the [Netlify Identity Widget](https://github.com/netlify/netlify-identity-widget) (because it's the least complicated option). See the demo in action [here](https://identity.netlify.com/).


To add it to our project:

* `yarn add netlify-identity-widget`


We'll import it to our `packages/www/src/pages/index.js` and initialize it.

```js
import netlifyIdentity from "netlify-identity-widget";

netlifyIdentity.init({});
```

We'll fire up the widget on button click:

```js
      <Button
        sx={{ marginTop: 2 }}
        onClick={() => {
          netlifyIdentity.open();
        }}
      >
```

The `index.js` should now look like [this](https://github.com/ChristopherBiscardi/serverless-todo-netlify-fauna-egghead/blob/eaa5f81458f76a0e0e15e0c3545aacfb9f7fb4c0/packages/www/src/pages/index.js).

We will have to run the widget against our Netlify deployed site (not `localhost`). Make sure to enable **Identify** in your Netlify site settings - you just need to configure this once.

👍 Note that Registration defaults to open - which is something you might want to change to "invite only" before making your site public.

👍 Your build will fail once you've added `Identity`. To fix it, comment out the `netlifyIdentity.init({});`, deploy, then uncomment it again.

👍 Use the `useEffect` hook to fire up the `Identity` on mount:

```js
  useEffect(() => {
    netlifyIdentity.init({});
  });
```

You can log the current user with:

```js
console.log(netlifyIdentity.currentUser());
```

Check out the full [`index.js` code](https://github.com/ChristopherBiscardi/serverless-todo-netlify-fauna-egghead/blob/2139683631518be8ead32b2a351d9b986cc49a8e/packages/www/src/pages/index.js).