---
name: Deploy to Heroku
desc: Deploying your Pakyow project to Heroku.
---

We'll be deploying our application to Heroku. The process is
summarized below, but you can read about this in more detail
[here](https://devcenter.heroku.com/articles/git).

First, our code must be in version control. Initialize a Git repository:

```console
git init
Initialized empty Git repository in .git
```

Stage the new files:

```console
git add .
```

And commit the staged files:

```console
git commit -m "initial commit"
[master (root-commit) d483d20] initial commit
 24 files changed, 1306 insertions(+)
 create mode 100644 Gemfile
 create mode 100644 Gemfile.lock
 create mode 100644 README.md
 create mode 100644 app.rb
 create mode 100644 app/lib/bindings.rb
 create mode 100644 app/lib/helpers.rb
 create mode 100644 app/lib/routes.rb
 create mode 100644 app/views/_intro.md
 create mode 100644 app/views/_templates/pakyow.html
 create mode 100644 app/views/index.html
 create mode 100644 app/views/show.html
 create mode 100644 config.ru
 create mode 100755 public/css/pakyow-css/CHANGES
 create mode 100755 public/css/pakyow-css/README.md
 create mode 100755 public/css/pakyow-css/VERSION
 create mode 100755 public/css/pakyow-css/examples/extension.css
 create mode 100755 public/css/pakyow-css/examples/structure-fluid.html
 create mode 100755 public/css/pakyow-css/examples/structure.html
 create mode 100755 public/css/pakyow-css/examples/styled.html
 create mode 100755 public/css/pakyow-css/reset.css
 create mode 100755 public/css/pakyow-css/structure.css
 create mode 100755 public/css/pakyow-css/style.css
 create mode 100755 public/css/pakyow-css/syntax.css
 create mode 100644 public/favicon.ico
 create mode 100644 rakefile
 ```

Your project is all ready to deploy! If you don't already have a Heroku
account, create one [here](https://api.heroku.com/signup/devcenter).
Then [install the toolbelt](https://toolbelt.heroku.com/).

Next, create your Heroku application (follow any prompts you receive):

```console
heroku create
```

Make a note of the outputted URL, this is how you will access your app.
Finally, push the code to Heroku:

```console
git push heroku master
```

Go to the app URL and find your app running in the cloud!

```console
heroku open
```
