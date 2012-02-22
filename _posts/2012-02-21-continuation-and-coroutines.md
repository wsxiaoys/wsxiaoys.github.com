---
layout: post
title: "Continuation and coroutine"
---

Lots of people <s>doesn't</s>*don't* really understand the differences <s>of</s>*between* continuation and coroutine. Actually they serve the same target that provide<r>s</r> an abstract of code's execution flow. Here I'll show you how they can be implemented by each other.
#### Coroutine in call/cc
Coroutine has two primitives at least: yield and fork. Fork starts a new coroutine and yield suspends the current coroutine and switch to another one.
<script src="https://gist.github.com/1880669.js?file=coroutine.scm"></script>

#### Call/cc in coroutine
Here I'll use Lua as the host language.(Since it has the native coroutines support).
Coroutine is actually less powerful than continuation.

To implement call/cc, we need coroutine primitives *clone* to make a copy of a given coroutine.

The point is to make one coroutine <s>can</s> be resumed from the same point more than once.
<script src="https://gist.github.com/1880669.js?file=callcc.lua"></script>

The lua code is a modified from [callcc.lua](https://github.com/torus/lua-call-cc/blob/master/callcc.lua)
