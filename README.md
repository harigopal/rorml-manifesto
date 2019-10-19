# Rails on ReasonML (RoRML)

## Introduction

This is a follow-up to Jasim A Basheer's post titled _Rails on Ocaml_. It expands on the ideas presented there to create a more concrete list of suggestions that could be used to build a web-framework that learns from what Ruby-on-Rails does best. This article suggests the use of ReasonML to build this new framework, given that it is a strongly-typed functional programming language that mangages to the pragmatic about its choices.

This article is split into sections that cover the major features that are _expected_ from a modern web-framework. However, this does not mean that _all_ these features must be present for the framework to be viable. Rails collected these features over time, and continues to do so(conventions for file storage were introduced in Rails 5.2), and relied on third-party packages to supply what was missing.

## What's covered?

1. Interacting with Databases
2. Maintaining Databases
3. Routing Requests
4. Session Management
5. Rendering HTML
6. Asset Management
7. File Storage
8. Building an API
9. Testing
10. Security
11. Logging
12. Internationalization
13. REPL
14. CLI
15. Supporting Extensions
16. Utility Functions
17. Documentation

## Testing

```
[TODO] Are there any more modern alternatives to the Capybara for "system" tests?
```

The test framework needs to take care of a few things:

1. It should be straight-forward to _set up_ the conditions required to run tests. This usually involves supplying a method to seed data to a test database.
2. It should be possible to write steps and expectations in a way that feels natural.
3. The test framework should take care of browser-integration.

### Setting up a test

```
TODO: There is much more to testing that seeding. The framework needs to create the database from the current schema, and needs to supply some way to clean the database between tests. Rails does this by wrapping each test in a database transaction and the rolling it back at the end - and it does so by default, without any extra configuration.

There's probably more that I'm forgetting or am plain unaware of.

This section also needs examples.
```

In my opinion, Rails has very basic support for _seeding_ test data. Thankfully, there many alternatives that fill the gap, with the most popular being [factory_bot](https://github.com/thoughtbot/factory_bot), which allows developers to create _templates_ of _models_ which can be easily used within individual tests to create their own database environment.

### Writing tests

Rails comes with a testing framework called Minitest.

```
[TODO] I've never really used Minitest - I can't really comment on it. Must research.
```

A common alternative is _RSpec_, which offers a syntax that is natural to write and closely follows actions that _we_ would take when manually testing a feature in a browser. While Rails supports testing of all layers of the application, it certainly encourages writing _system_ tests using Capybara. Non-system tests are generally required only for code that doesn't get executed because of a user's action - background jobs, for example.

The presence of a tightly integrated test framework that _knows_ how to interact with each of the layers of the application is _essential_ to making the process possible. And while types will protect developers from making _clerical_ errors, it will not protect against mistakes made in business logic that are inevitable in large, long-lived codebases.

### Running tests on the browser

Here's are the key benefits of using Capybara, according to their documentation:

> - No setup necessary for Rails and Rack application. Works out of the box.
> - Intuitive API which mimics the language an actual user would use.
> - Switch the backend your tests run against from fast headless mode to an actual browser with no changes to your tests.
> - Powerful synchronization features mean you never have to manually wait for asynchronous processes to complete.

These features will have to be kept in-tact. This combination of features allows devs to write tests quickly with minimum fuss.

## Utility Functions

```
[TODO] This section needs examples. Also, are there existing ReasonML packages that attempt to do this already?
```

> Typed FP expands our taste in programming because like how Ruby and Rails showed how _programming is writing_, Typed FP shows how _programming is mathematics_.

The reality is that programming is both writing _and_ mathematics. What programmers need is a syntax to express their ideas and intent in a manner that is both _fluent_ and _sound_. I believe that ReasonML goes a long way in supplying the _soundness_ that is lacking in a language like Ruby.

However, the ReasonML language cannot (and probably should not) attempt to provide a large library of functions for the sake of expressiveness - a lot of these functions will also be very domain-specific.

_Rails_ uses [Active Support](https://guides.rubyonrails.org/active_support_core_extensions.html) to extend the _language_ so that developers have access to a syntax that can be both expressive and succinct. An equivalent library should be packaged with the framework.

### List of requirements:

1. Include a package that contains utility functions and extensions to the standard library.

When written like _that_, it doesn't seem quite important, but the reality is that using Rails without ActiveSupport would feel _crippling_. Most Rails developers are so used to the additions that ActiveSupport brings that they don't know [where Ruby ends and Rails begins](https://railshurts.com/quiz/).
