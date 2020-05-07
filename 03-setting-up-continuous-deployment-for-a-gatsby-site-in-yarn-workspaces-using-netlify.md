# Setting up continuous deployment for a Gatsby site in yarn workspaces using Netlify

**[📹 Video](https://egghead.io/lessons/gatsby-setting-up-continuous-deployment-for-a-gatsby-site-in-yarn-workspaces-using-netlify?pl=building-a-serverless-jamstack-todo-app-with-netlify-gatsby-graphql-and-faunadb-53bb)**

**Objective**: Let's deploy!

We'll be deploying to [Netlify](https://www.netlify.com/), since it enables:
- static site hosting
- lambda functions

Go to Netlify and:

* Click on **New site from Git**, log into GitHub and add the repository - we'll be deploying from the `master` branch.

**Build settings**:

* Build command: `yarn workspace www build`
* Publish directory: `packages/www/public`

Click **Deploy site**!

👍 Go to **Site settings** and **Change site name**.

From now on, whenever we push changes to the GitHub repo, they will automatically be deployed to the live Netlify site as well!
