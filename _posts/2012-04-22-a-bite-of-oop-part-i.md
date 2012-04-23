---
layout: post
title: "A bite of OOP (Part I)"
---

I talked with friends on OOP when having dinner yesterday. We argued about the OOP and its implementation in dynamic languages like Python, Ruby and static languages like Java.

In this article, I don't want to make a flame war on which one is better but try introducing the original form of Object-oriented and why languages like C++/Java doesn't really take it.

---

# Part I: Runtime Dispatching
Let's take an example of invoking an object's virtual method in C++:
<script src="https://gist.github.com/2464783.js?file=animal.cpp"></script>
Experienced C++ programmers know what compiler do with the virtual methods:

* Each object have a pointer to virtual table of its class.
* The method "Speak" is translated into an index in vtable. No matter what the subclass is, the "Speak" method will always locate the same slot.

This is how C++ implements Polymorphism. By doing this the vtable for each class is built in the compile time. No changes can be applied in the runtime.

While with the OOP model like Objective-C, you can call an object's method in a fancy way:
<script src="https://gist.github.com/2464783.js?file=animal.m"></script>

Hey wait! They Objective-C and C++ are almost same! They must have a similar model!
OK, let me show you some magics:
<script src="https://gist.github.com/2464783.js?file=oc_runtime_animal.m"></script>

As you see, here the method "new", "Speak" identifiers(SEL in Objective-C) are created at the runtime! It is significant different from C++, which are determined at compile time. This kind of mechanism is usually called Runtime Dispatching or Messaging.

Any advantages for doing this?

Say we're writing some wrappers for the data saved in CSV files. In C++ you have to bind each column with hand-writing methods. The code are mostly duplicated but you have to write it everywhere. (JavaBean is another example).

But with runtime dispatching you can achieve it in an easy way. The method matching is done in the runtime, one thing we can do is to capture the "Method Missing" event. Then we can forward missing `[obj attr]` method call to `[obj getField:attr]`. ([Detail example](http://langexplr.blogspot.com/2008/02/handling-call-to-missing-method-in_06.html))

The example above is just one points of so many features of runtime dispatching, in part II I'll talk about some really cool features like runtime class declaration, accessing class's metadata, etc.
