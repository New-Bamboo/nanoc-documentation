# This project is no longer being maintained

Please don't use it. Thank you.

# Embedded Documentation

This is a documentation resource built with [nanoc][nanoc].

Orginally forked from the excellent [GitHub Documentation](https://github.com/github/developer.github.com). Thanks!

## Setup

Ruby 1.9 is required to build the site.

Get the nanoc gem, plus kramdown for markdown parsing:

    bundle install

You can see the available commands with nanoc:

    nanoc -h

Nanoc has [some nice documentation](http://nanoc.stoneship.org/docs/3-getting-started/) to get you started.  Though if you're mainly concerned with editing or adding content, you won't need to know much about nanoc.

[nanoc]: http://nanoc.stoneship.org/

## Installation

The project is set up such that you can drop all the files into "#{Rails.root}/docs".

Copy all the gems from the Gemfile into you project's Gemfile. The recommended way is to wrap them all in a group:

    group :documentation do
      ...
    end

Then delete the Gemfile in the docs directory.

At this point you can compile the docs with:

    (cd docs && nanoc compile)

This will compile the files into "#{Rails.root}/public/docs", so that they are served up as static files through your Rails app.

## Styleguide

Not sure how to structure the docs?  Here's what the structure of the
API docs should look like:

    # API title

    ## API endpoint title

        [VERB] /path/to/endpoint.json

    ### Parameters

    name
    : description

    ### Input (request json body)

    <%= json :field => "sample value" %>

    ### Response

    <%= headers 200, :pagination => true, 'X-Custom-Header' => "value" %>
    <%= json :resource_name %>

**Note**: We're using [Kramdown Markdown extensions](http://kramdown.rubyforge.org/syntax.html), such as definition lists.

### JSON Responses

We specify the JSON responses in ruby so that we don't have to write
them by hand all over the docs.  You can render the JSON for a resource
like this:

```erb
<%= json :issue %>
```

This looks up `NewBamboo::Resources::ISSUE` in `lib/resources.rb`.

Some actions return arrays.  You can modify the JSON by passing a block:

```erb
<%= json(:issue) { |hash| [hash] } %>
```

### Terminal blocks

You can specify terminal blocks with `pre.terminal` elements.  It'd be
nice if Markdown could do this more cleanly...

    <pre class="terminal">
    $ curl foobar
    ....
    </pre>

This isn't a `curl` tutorial though, I'm not sure every API call needs
to show how to access it with `curl`.

## Development

During development you can use Guard to automatically re-compile any changes:

    (cd docs && guard)
