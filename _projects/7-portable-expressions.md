---
layout: project
title: "PortableExpressions"
github_url: "https://github.com/omkarmoghe/portable_expressions"
languages:
  - Ruby
emoji: 🍱
order: 7
---

**TL;DR** &mdash; [A simple and flexible pure Ruby library for building and evaluating expressions.](https://github.com/omkarmoghe/portable_expressions) Fully compatible with JSON.

I wrote `PortableExpressions` because I needed a way to write math expressions that were fully JSON serializable for another project. Although that project got [sidetracked](https://www.commitstrip.com/en/2014/11/25/west-side-project-story), I decided to release the code for building and serializing these expressions as a library.

Because most (all?) math operators in Ruby are implemented as methods, what I really ended up building was a framework to express any procedure or function call. It works great for math, but can do anything that you can describe as a reduction of an array by some method. The elements in the array are the `operands`, and the method is the `operator`. Here, reduction just means `elementN.method(elementN+1).method(elementN+2)...` and is implemented using Ruby's `reduce` (aka `inject`).

```ruby
# Addition
[1, 2].reduce(:+) #=> 3

# Multiplication
[2, 2, 2].reduce(:*) #=> 8
```

The library is made up of 4 main objects, all of which are fully JSON serializable (and deserializable):

1. `Scalars`, which represent an "atom" or value that can be operated on. A `Scalar`'s `value` can be any [JSON type](https://www.w3schools.com/js/js_json_datatypes.asp).
1. `Variables`, which represent a named value stored in the `Environment` (more on this later). These allow you to defer evaluation of some object until the `Variable` is used.
1. `Expressions`, which represent an array of `operands` and an `operator`. The `operands` can be `Scalars`, `Variables`, or even other `Expressions`. The `operator` can be any Ruby symbol representing a method that all but the last `operands` must support.
1. `Environments`, which represent the `state` of the procedure and hold the values that `Variables` "point" to.

`Expressions` are stateless by default and can be evaluated by any `Environment`. A procedure can be described by one or more `Expressions` _once_ and can be used multiple times. By allowing `Expressions` to be built independently of the `Environment`, the library allows your system to get instructions (`Expressions`) from one source, and the input (`Environment`) from elsewhere.

One example of this abstraction in practice is implementing [authorization policies](https://github.com/omkarmoghe/portable_expressions#authorization-policies) via stateless `Expressions`, and then executing them on various distributed services using an `Environment` that contains the relevant, local context. Here, each service might use one or some combination of policies and inject the relevant user and other info through the `Environment`. The environment itself can be serialized so you can invert the architecture by having a centralized authorization service supports many policies, and various distributed services that request decisions through some API. The `POST` body might contain the serialized `Environment` along with info about which policy the services is requesting execution for.
