---
name: Development Tools
---

Pakyow provides several tools useful during development.

## Server

The server command runs a local instance of a Pakyow application.

    console:
    pakyow server [environment]

If environment is not specified, the `default_environment` defined in the application will be used.

When starting the server, Pakyow will try the following handlers in order:

  - puma
  - thin
  - mongrel
  - webrick

You can run a specific handler by setting the `server.handler` [config option](/docs/configuration).

## Console

The console command loads an application into a REPL (like IRB).

    console:
    pakyow console [environment]

If environment is not specified, the `default_environment` defined in the app will be used. Once started, you can execute Ruby code against your app. If a file is changed, the session can be reloaded, like so:

    irb:
    reload
    Reloading...

## Rake Tasks

Several rake tasks are included with the `pakyow-rake` gem. To use them, add `pakyow-rake` to your `Gemfile` and require it at the top of `Rakefile`. Here's a list of tasks it adds to your app:

    console:
    rake --tasks
    rake pakyow:bindings[view_path]  # List bindings across all views, or a specific view path
    rake pakyow:prepare              # Prepare the app by configuring and loading code
    rake pakyow:routes               # List all routes (method, path, group[name])
    rake pakyow:stage                # Stage the app by preparing and loading routes / views
