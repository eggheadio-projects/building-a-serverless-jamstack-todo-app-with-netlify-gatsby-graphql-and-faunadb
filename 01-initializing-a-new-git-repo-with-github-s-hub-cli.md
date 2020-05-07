# Initializing a new git repo with Github's hub CLI

**[📹 Video](https://egghead.io/lessons/git-initializing-a-new-git-repo-with-github-s-hub-cli?pl=building-a-serverless-jamstack-todo-app-with-netlify-gatsby-graphql-and-faunadb-53bb)**

**[💻 Code](https://github.com/christopherbiscardi/serverless-todo-netlify-fauna-egghead/tree/6675157984acc6972c038b09be20c54e2588fc6e)**

**Objective**: Let's initialize our project.

* `git init .`

Initialize a new repository.

* `echo "Serverless TODOs" > README.md`

Add a readme file.

* `git add README.md`
* `git commit -m "initial commit"`

To add and commit the README file to the repository.

* `which git`

To see which binary your `git` is aliased to. (For me it was `/usr/local/bin/git`, for Chris, `git` is alised to `hub`).

🤔 Install the command-line tool [Hub](https://github.com/github/hub) for easier Github wrangling.

* `brew install hub`

👍 If you are using `homebrew` as your package manager.

* `hub create`

This will create a github repository and output the repository link.

👍 You'll have to add your username and password when using `hub` for the first time.

👍 If you want to make a link clickable in your command line (at least in `iterm`), press `cmd` key then click.

* `git push -u origin master`

Push your local changes to your Github repository.
