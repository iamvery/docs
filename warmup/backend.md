---
name: Add the Back-End
---

Next we'll write the back-end code necessary to present our data. No
changes will be required to the views we've already created, leaving our
prototype intact.

<p class="callout">
  To see the prototype at any point (even once the back-end is added),
  run the application in the `prototype` environment. This can be done
  by stopping the application server (Ctrl-C) and restarting it in the
  prototype environment: pakyow server prototype
</p>

<h2 id="integration">Twitter Integration</h2>

We'll be interacting with Twitter through their developer API. They provide a
Ruby library to make this easier. There are a few steps to get
everything setup, but walking through it will show you how easy it is to work
with a Pakyow back-end.

First we need to add the Twitter gem to our `Gemfile`. The entire file should
look like this:

    ruby:
    source "http://rubygems.org"

    gem "pakyow", "0.9.0"

    # application server
    gem "puma"

    gem "twitter"

Stop the server (Ctrl-C) and run `bundle install` to install the gem. Then require
it at the top of `app.rb`:

    ruby:
    require 'twitter'

Next, you'll need to register the application with Twitter. You can do that
[here](https://dev.twitter.com/apps/new). It doesn't matter what you use as the
Name, Description, and Website; Callback URL can be left blank.

Now let's add the credentials to our configuration. Open `app.rb` and add the
following code to the `global` configure block so it's available to both our
development and production environments (be sure and use the credentials
provided to you by Twitter):

    ruby:
    app.consumer_key        = "YOUR_CONSUMER_KEY"
    app.consumer_secret     = "YOUR_CONSUMER_SECRET"
    app.access_token        = "YOUR_ACCESS_TOKEN"
    app.access_token_secret = "YOUR_ACCESS_SECRET"

The entire file should look like this:

    ruby:
    require 'bundler/setup'

    require 'pakyow'
    require 'twitter'

    Pakyow::App.define do
      configure :global do
        app.name = "Pakyow Warmup"

        app.consumer_key        = "YOUR_CONSUMER_KEY"
        app.consumer_secret     = "YOUR_CONSUMER_SECRET"
        app.access_token        = "YOUR_ACCESS_TOKEN"
        app.access_token_secret = "YOUR_ACCESS_SECRET"
      end

      configure :development do
        # put development config here
      end

      configure :prototype do
        # an environment for running the front-end prototype with no backend
        app.ignore_routes = true
      end

      configure :staging do
        # put your staging config here
      end

      configure :production do
        # put your production config here
      end
    end

Your application is ready to talk with Twitter!

<h2 id="helper">Twitter Helper</h2>

Our application needs an easy way to create a Twitter client using the
credentials we defined in our configuration. Because we'll need access to the
client throughout our back-end code, let's create a helper.

Open `app/lib/helpers.rb` and add the following code inside the `Helpers` module:

    ruby:
    def client
      @client ||= configure
    end

    protected

    def configure
      Twitter::REST::Client.new do |client|
        client.consumer_key         = config.app.consumer_key
        client.consumer_secret      = config.app.consumer_secret
        client.access_token         = config.app.access_token
        client.access_token_secret  = config.app.access_token_secret
      end
    end

When we call the `client` method from our back-end code, it will configure a new
instance of the Twitter client and return it. We'll use this object to fetch our
data from Twitter.

<h2 id="routing">Routing</h2>

Now we're ready to write the routing code. This app will have two routes, each
corresponding with one of the pages we created earlier. Because both routes are
dealing with a "tweet" object, it's best to define this using the [REST
architecture](http://en.wikipedia.org/wiki/Representational_state_transfer).
Pakyow makes this super easy.

Open `app/lib/routes.rb` and add the following code:

    ruby:
    restful :tweet, '/' do
    end

This defines the restful object and mounts it at the '/' path. Next, create a
route that responds to the 'list' REST method:

    ruby:
    restful :tweet, '/' do
      list do
      end
    end

The code block for the list route will be invoked when we make a GET request
to '/'. We also need a route that presents information about a particular tweet.
This falls under the 'show' REST method:

    ruby:
    restful :tweet, '/' do
      list do
      end

      show do
      end
    end

Now let's add our logic to these empty route definitions.

<h2 id="logic">Presentation Logic</h2>

The first thing the `list` route will do is fetch the tweets that will be
displayed. We can use the `client` helper we defined earlier to search for
10 recent tweets containing #programming:

    ruby:
    tweets = client.search("#programming", result_type: "recent").take(10).to_a

Now, we can apply the data to the view. Pakyow automatically maps the request
path to a path in the views directory; here it maps '/' to the root view
directory. All that's left is to apply the data:

    ruby:
    view.scope(:tweet).apply(tweets)

This bit of code finds and applies data to the node that represents a `tweet`.
The `apply` method alters the structure of the view to match the structure of
the data being applied. In this case, it repeats the `tweet` node once for
every tweet. Once the structures match, the data is easily mapped to the view.

The full list route should look like this:

    ruby:
    list do
      tweets = client.search("#programming", result_type: "recent").take(10).to_a
      view.scope(:tweet).apply(tweets)
    end

Fire up the app (if it isn't already running) and go to [localhost:3000](http://localhost:3000).
You'll see that the tweets are
pulled from Twitter and applied to the view. The only problem is some of the
data wasn't applied correctly; there's no avatar and the user name isn't being
presented properly.

We can solve this using bindings. Bindings sit between the back-end and
front-end code and help apply data to the view. Open `app/lib/bindings.rb` and
define the following binding for our `avatar` prop:

    ruby:
    scope :tweet do
      binding :avatar do
        { src: bindable.user.profile_image_url }
      end
    end

This binding finds the `profile_image_url` in the tweet data being
applied and maps it to the `src` attribute. It will be invoked whenever
data is applied to the `avatar` prop of a `tweet` scope. Reload the
default page of the app in your browser and you'll see that the avatars
are now properly presented.

Let's also define a binding for the `user` prop, so the user name is
presented:

    ruby:
    binding :user do
      bindable.user.name
    end

Unlike the `avatar` binding which mapped data to the `src` attribute,
this binding simply returns the user > name data as the content for
the `user` prop. Reload the browser and you'll see the user names are
now being presented correctly.

All of our data is now being presented. There's still one problem --
that is, the "View Details" link needs to go to the proper URI. Define
the following binding for the `show` prop:

    ruby:
    binding :show do
      { href: router.group(:tweet).path(:show, tweet_id: bindable.id) }
    end

This binding uses the router path helper to create a URI to the restful
`show` method for the tweet. Reload the browser and click on a link. It
does take us to the right place, but we still see the prototype. Let's
add our back-end logic:

    ruby:
    tweet = client.status(params[:tweet_id])
    view.scope(:tweet).apply(tweet)

Reload your browser and you'll see the show view, with data. You'll notice
that the bindings we defined earlier are already being used here.

There are three things on the show page that weren't included in the
index page: the retweet / favorite counts, and the view on twitter
link. The data for the counts both just happen to be mapped correctly,
so there's nothing to do there. However, for the twitter link to
work we must define one last binding:

    ruby:
    binding :twitter do
      { href: "http://twitter.com/#{bindable.user.screen_name}/status/#{bindable.id}" }
    end

Reload again and you'll see the twitter link now works properly.

<h2 id="wrapup">Back-End Wrapup</h2>

We just added a back-end to our application with the following steps:

1. Defined restful routes for our application
2. Added presentation logic that applies data to the views
3. Created bindings to help map data in the right place

Development is complete; let's ship it!
