---
layout: post
title: "A Codewalk for Arc3's first class macro (function as macro)"
---

Arc is a lisp dialect created by [Paul Graham](http://en.wikipedia.org/wiki/Paul_Graham_\(computer_programmer\)). It's a tiny lisp implementation built on the top of [Racket](http://www.racket-lang.org).

First class macro is one of the coolest designs in Arc. Generally to say macros in arc are functions that takes symbols as input and output the transformed S-expressions. Below is a snippet that implements _and_ primitive with _if_ syntax.

<script src="https://gist.github.com/2819094.js?file=macro_example.scm"></script>

In Arc, a macro is simply a tagged vector with its function, its structure can be expressed by `#(tagged mac <function #name>)`. When compiling a source file, for each list that might be a function call or a macro call, it invokes the following function:

<script src="https://gist.github.com/2819094.js?file=ac_ac-call.scm"></script>

Here it detects whether a symbol indicates a macro with `ac-macro?`. `ac-macro?` returns the macro function if the symbol do exist in the environment and tagged with `mac`. After that, it invokes the macro function and compiles its output.

This is quite straight forward, but how does Arc define the macro? See following example:

<script src="https://gist.github.com/2819094.js?file=macro_example2.scm"></script>

Here assign is a similar primitive to _set!_ but will create global binding if the identifier doesn't exist in the current scope. As you see the macro declaration is just a global binding whose value is a tagged list. So the problem is, how does Arc make use of a runtime created object in the compile-time?

<script src="https://gist.github.com/2819094.js?file=ac-aload1.scm"></script>

Yes, Arc treats itself as an _interpreter_, evaluating every form exactly after it got compiled, which means that the runtime and compile-time environments are combined into one. This is apparently different from other scheme's implementation.

Arc's macro system is quite simple, but it also has drawbacks, e.g. A nested macro declaration doesn't affect the inner forms:

<script src="https://gist.github.com/2819094.js?file=ac-drawbacks.scm"></script>

Rather than return nil, it generates following error:

    Error: "Function call on inappropriate object #(tagged mac #<procedure: identity>) (nil)"

The reason is that in the compiling process, `(identity dummy)` is not handled as an macro call since the symbol _identity_ has not been registered in the runtime. So take care when using it :).

BTW: You might want to give a try to Arc in the [Browser](http://tryarc.org).