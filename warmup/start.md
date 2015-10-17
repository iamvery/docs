---
name: Get Moving
desc: Taking your first steps with Pakyow.
---

First, make sure Pakyow is installed. You can find [details here](/docs/getting-started/installation).

Pakyow ships with command-line tools that help you generate and manage
your projects. These tools make creating a new project really easy.
Let's create one for this warmup:

```console
pakyow new warmup
Generating project: warmup
Running `bundle install`
Done!
```

Pakyow creates the project structure for us, allowing us to get right
to work. Move into the new warmup directory and start the app server by
running the following commands:

```console
cd warmup
pakyow server
```

Go to [localhost:3000](http://localhost:3000) to see your app running!
