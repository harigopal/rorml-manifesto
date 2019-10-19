# Rails on Ocaml

## Introduction

This is a follow-up to Jasim A Basheer's post titled _Rails on Ocaml_. It expands on the ideas presented there to create a more concrete list of suggestions that could be used to build a web-framework that learns from what Ruby-on-Rails does best. This article suggests the use of ReasonML to build this new framework, given that it is a strongly-typed functional programming language that mangages to the pragmatic about its choices.

This article is split into sections that cover the major features that are expected from a modern web-framework.

## What's covered?

1. Interacting with Databases
2. Maintaining Databases
3. Routing Requests
4. Rendering HTML
5. Asset Management
6. Building an API
7. Testing
8. Security
9. Logging
10. Aesthetics
11. Internationalization
12. REPL
13. CLI
14. Supporting Extensions


## Testing

Something Jasim said in his article does not sit right with me:

> I don’t care about it deeply enough — fewer bugs and stable programs is sort of disconnected from my day-to-day existence, and let the bean counters care about it if they have to.

I am not a bean counter, but I've been working on the same web application for the past six years, and over that time, it has changed it's shape and purpose more times that I can remember.

It started as a content delivery system focused on teaching first-time entrepreneurs how to build startups (hence the name of [the company I work for](https://www.sv.co)), and is currently [an open-source white-labeled learning management system](https://www.pupilfirst.com) that advocates the ideology that true learning occurs through a cycle of action and feedback / critique from experts.

We've had quite a variety of audience using the platform through those six years, and very rarely have they complained about the platform failing or doing something unexpected, and that's entirely because of how easy testing is, and how the _community_ has evolved to _expect_ tests for even the simplest of applications.

### Tests are also code.

Whether a programmer writes tests depends on:

1. Whether it's easy to write tests - to express steps and expectations _naturally_.
2. Whether the tests run reliably, and reasonably quickly.

Our tests are written using RSpec, but that choice was arbitrary, and was made at a point of time when we didn't have any experience with Rails' default - Minitest. A comparison between the two will have to be made at some point of time, but RSpec offers a syntax that is natural to write and closely follows actions that _we_ would take when manually testing a process in a browser. Most of our tests are _integration_ tests, - we generally write non-integration tests _only_ for code that doesn't get executed because of a user's action - background jobs.

The presence of a tightly integrated test framework that _knows_ how to interact with each of the layers of the application is _essential_ to making the process possible. And while types will protect developers from making _mistakes of accounting_ **[what was the phrase Jasim used?]**, it will not protect against mistakes made in business logic that are inevitable in large, long-lived codebases.

### List of requirements:

**[TODO]** Boil requirements down to a list.

1. A
2. B

## Aesthetics

**[TODO]** This part is important, but deviates from the topic at hand, and I'm not sure how to rope it in.

> Typed FP expands our taste in programming because like how Ruby and Rails showed how _programming is writing_, Typed FP shows how _programming is mathematics_.

The reality is that programming is both writing _and_ mathematics. What programmers need is a syntax to express their ideas and intent in a manner that is both _fluent_ and _sound_. I believe that ReasonML goes a long way in supplying the _soundness_ that is lacking in a language like Ruby, but I wonder whether _fluency_ is a major concern for ReasonML's language developers. Here's a simple example:

```reason
let  x  =  Js.Array.join([|"1",  "2"|]);
let  x  =  Js.Array.joinWith("",  [|"1",  "2"|]);
```

The first statement will throw a warning, because `Js.Array.join` is deprecated. I'm not sure _why_, and I don't want to _guess_, but I know which one I'd prefer to write to just _join_ two strings together.

Yes, `joinWith` can do exactly what `join` can, but in _Ruby_ land, the idea of deprecating `join` would not fly. In fact, it'd probably just stay as a function that uses `joinWith` internally:

```reason
let join = t => t |> joinWith("");
```

Now these kinds of convenience functions, don't have to a part of the standard library. _Rails_ uses [Active Support](https://guides.rubyonrails.org/active_support_core_extensions.html) to extend the _language_ so that we programmers have access to a syntax that can be both expressive and succinct.

### List of requirements:

1. Include a package that contains utility functions and extensions to the standard library.

**[TODO]** When written like _that_, it doesn't seem that important, but the reality is that using Rails without ActiveSupport would feel _crippling_.
