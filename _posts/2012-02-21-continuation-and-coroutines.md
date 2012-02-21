---
layout: post
title: "Continuation and coroutine"
---

Lots of people doesn't really understand the differences of continuation and coroutine. Actually they serve the same target that provide an abstract of code's execution flow. Here I'll show you how they can be implemented by each other.
#### Coroutine in call/cc
Coroutine has two primitives at least: yield and fork. Fork starts a new coroutine and yield suspends the current coroutine and switch to another one.

{% highlight scheme linenos %}
;; Holds all coroutines.
(define coroutine-queue '())

;; Push to the *tail* of the queue
(define (push x)
 (set! coroutine-queue (append coroutine-queue (list x))))

;; Pop the *first* element and return it
(define (pop)
  (let ((x (car coroutine-queue)))
    (set! coroutine-queue (cdr coroutine-queue))
    x))

(define (fork proc)
  (call/cc (lambda (k)
             (push k)  ; push the current continuation for future resuming.
             (proc))))

(define (yield x)
  (call/cc (lambda (k)
             (push k)
             ((pop) x))))  ;; pop the first continuation and resume it.
{% endhighlight %}

#### Call/cc in coroutine
Here I'll use Lua as the host language.(Since it has the native coroutines support).
Coroutine is actually less powerful than continuation.

To implement call/cc, we need coroutine primitives *clone* to make a copy of a given coroutine.

The point is to make one coroutine can be resumed from the same point more than once.

{% highlight lua linenos %}
do
  local pc, cont, arg

  function callcc(proc)
    cont = pc
    pc = coroutine.create(proc)  -- update pc/cont
    arg = nil

    local r
    _, r = coroutine.yield() -- switch to pc procedure we just created
    return r -- coroutine result as call/cc result
 end

  function make_cont(cont)
    local current = cont
    return function(x)
      -- we don't actually have *clone* in lua
      pc = current and coroutine.clone(current)
      cont = nil
      arg = x
      coroutine.yield()
    end
  end

  -- We need a main loop for dispatching next cont procedure
  function main(proc) 
    pc = coroutine.create(proc) -- register the initial procedure
    cont = nil
    arg = nil
    while (pc and coroutine.status(pc) ~= "dead") do
      -- resume the current pc with cont/arg
      coroutine.resume(pc, make_cont(cont), arg) 
    end
  end
end
{% endhighlight %}

The lua code is a modified from [callcc.lua](https://github.com/torus/lua-call-cc/blob/master/callcc.lua)
