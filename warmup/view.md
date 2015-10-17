---
name: Start with the View
desc: Building the front-end of your Pakyow project using view-first development.
---

Pakyow encourages a view-first development process. The goal of this process
is to use the app in the browser as early as possible. Pakyow accomplishes this
by allowing a front-end developer or designer to create a navigable prototype without
writing any back-end code. This means that even with a limited skillset, you can
create an app that your team (and project stakeholders) can interact with.

Views are easy in Pakyow, and are just HTML. There's no extended syntax. All of the
benefits of traditional template languages, like layouts and partials,
are retained.

There are three parts to a view:

1. Template: defines the reusable structure for a view
2. Page: implements a template with page-specific content
3. Partial: a reusable piece that can be included into a Page or Template

View building begins with a page. When fulfilling a request, Pakyow
first identifies the page to use based on the request path. For example,
a request for '/' would map to the 'index.html' page.

The page implements a single template. If a template isn't specified, Pakyow
uses the default template (named `pakyow.html`). By default, all templates are
defined in the `app/views/_templates` directory. Open the root project directory
into a text editor and have a look around.

Every Pakyow template defines one or more containers. Later on in the process,
we'll create pages that define content for these containers. Containers are
defined with an inline comment:

```html
<!-- @container container_name -->
```

A nameless `default` container is defined for you in the `pakyow.html`
template. During construction, Pakyow composes page content with the
template to create the final view sent back in the response.

<p class="callout">
You might notice the div that contains a `container-1` class. This
class is defined in the pakyow-css library included with every generated
app. Explaining the library is beyond the scope of this warmup, but you
can read more <a href="http://github.com/pakyow/pakyow-css">here</a>.
<br>
<br>
There are several uses of pakyow-css throughout this warmup. Anytime
you see a class name on a node, it's safe to assume it is related to
pakyow-css and is simply to define some default styling.
</p>

<h2 id="creatingviews">Creating Views</h2>

Let's create an index page for our app. Since we're building a Twitter
reader, this page will contain a list of recent tweets from Twitter,
showing the user name, avatar, and contents of each tweets. Each item
will also link to a page that will show more details about it.

Create `app/views/index.html` with the following content:

```html
<div class="container-2 margin-t">
  <img src="http://placehold.it/50x50" class="float-l margin-r">

  <a href="/show" class="float-r">
    View Details
  </a>

  <h4>
    User's Name
  </h4>

  <p>
    Tweet text goes here
  </p>
</div>
```

Reload the browser and you'll see the code we added. Huzzah!

Clicking on the view details link takes us to a broken page, so let's
fix that. Create `app/views/show.html` with the following content:

```html
<a href="/">
  Back to list
</a>

<div class="margin-t">
  <img src="http://placehold.it/50x50" class="float-l margin-r">

  <h4>
    User's Name
  </h4>

  <p>
    Tweet text goes here
  </p>

  <p>
    Retweeted n times;
    Favorited n times;
    <a href="#">
      View on twitter
    </a>
  </p>
</div>
```

Reload the browser and you'll see the new view.

The final thing this app needs is an explanation. Let's use another aspect of
view building, partials, to add this to our index view. First, define
content for the partial by creating a `_intro.html` file in the `app/views`
directory with the following content:

```html
<h1 class="divide">
  Pakyow Warmup
</h1>

<p>
  Just a fun little warmup using <a href="http://pakyow.org">Pakyow</a>.
</p>
```

Next, include the partial into the index page by adding this HTML
to the top of `index.html`:

```html
<!-- @include intro -->
```

Reload the index page in your browser and you'll see the intro has been
composed into the template. This intro could be included into any page,
such as `show.html`. Create it once, use it many times!

<h2 id="data">Data Knowledge</h2>

At this point we have created a navigable prototype of our app, and we
didn't write a single line of back-end code! The next step is to build
the back-end that will populate our views with data. None of this
back-end code will be mixed in to our view. Instead, it will be added
as a layer that interacts with our view.

For this to work we want the view to describe the data it intends to
present. Looking at the `index.html` view it's pretty easy to identify the
data that's being represented. For example, we can see that the `div`
represents a tweet. Since this is a *type* of data we label it as a scope:

```html
<div data-scope="tweet" class="container-2 margin-t">
```

We also know the `img` will present an avatar. Since this is an
*attribute* of a tweet, we label it as a prop:

```html
<img data-prop="avatar" src="http://placehold.it/50x50" class="float-l margin-r">
```

We continue labeling significant nodes (show link, user name, and text)
and end up with the following code:

```html
<!-- @include intro -->

<div data-scope="tweet" class="container-2 margin-t">
  <img data-prop="avatar" src="http://placehold.it/50x50" class="float-l margin-r">

  <a data-prop="show" href="/show" class="float-r">
    View Details
  </a>

  <h4 data-prop="user">
    User's Name
  </h4>

  <p data-prop="text">
    Tweet text goes here
  </p>
</div>
```

These attributes gives Pakyow the knowledge it needs to properly apply our
data to the view. Pakyow keeps the view completely separate from the
back-end, allowing the front-end to fully define how the data in the
application should be presented. If you haven't already, add the scopes
and props to your `index.html` view. Let's also describe the data presented
by `show.html`:

```html
<a href="/">
  Back to list
</a>

<div data-scope="tweet" class="margin-t">
  <img data-prop="avatar" src="http://placehold.it/50x50" class="float-l margin-r">

  <h4 data-prop="user">
    User's Name
  </h4>

  <p data-prop="text">
    Tweet text goes here
  </p>

  <p>
    Retweeted <span data-prop="retweet_count">n</span> times;
    Favorited <span data-prop="favorite_count">n</span> times;
    <a data-prop="twitter" href="#">
      View on twitter
    </a>
  </p>
</div>
```

<h2 id="front-end">Front-End Wrapup</h2>

At this point we've done the following:

1. Created a navigable prototype of our application
2. Made our views knowledgable of the data they will present

Click around, admire our progress, and let's move on.
