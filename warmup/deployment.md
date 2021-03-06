---
name: Heroku Deployment
desc: Deploying your Pakyow project to Heroku.
---

It's time to deploy our warmup project to Heroku!

First, our code must be in version control. Initialize a Git repository by
running the following command:

```console
git init
```

Now, stage the project files:

```console
git add .
```

And commit them:

```console
git commit -m "initial commit"
```

Your project is ready to deploy! If you don't already have a Heroku account,
create one [here](https://api.heroku.com/signup/devcenter). Then [install the
toolbelt](https://toolbelt.heroku.com/) so you have the appropriate command-line
deployment tools.

Next, create your Heroku environment (following any prompts you receive):

```console
heroku create
```

Add Redis to the Heroku environment:

```console
heroku addons:create heroku-redis:hobby-dev
```

Finally, push the project code up to Heroku:

```console
git push heroku master
```

Over the next minute or two, Heroku will build the project and make it available
for use. When it's finished, type the following command to open your Pakyow
project in a web browser:

```console
heroku open
```

Celebrate. Hug your cat. You deserve it!
