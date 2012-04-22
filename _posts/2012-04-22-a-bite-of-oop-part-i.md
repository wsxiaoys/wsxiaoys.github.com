---
layout: post
title: "A bite of OOP (Part I)"
---

I talked with friends on OOP when having dinner yesterday. We argued the OOP and its implementation in dynamic languages like Python, Ruby and static languages like Java.

I don't want to make a flame war on which one is better but try introducing the origin form of Object-oriented and why languages like C++/Java doesn't really take it.

# Part I: Runtime Dispatching
Let's take an example of calling an object's virtual method in C++:
<script src="https://gist.github.com/2464783.js?file=animal.cpp"></script>
Experienced C++ programmers know what compiler do for you with the virtual methods: Each object will have a pointer to its virtual table. The method "Speak" is translated into an index in vtable. Whatever the subclass is, the "Speak" method will always locate the same slot. This is how C++ implements Polymorphism. By doing this the vtable for each class is built in the compile time. No changes can be applied in the runtime.

While with the OOP model like Objective-C, you can call an object's method in a fancy way:
<script src="https://gist.github.com/2464783.js?file=animal.m"></script>

Hey wait! They Obj-C and C++ are almost same! They must use similar model!
OK, let me show you some magics:
<script src="https://gist.github.com/2464783.js?file=oc_runtime_animal.m"></script>

As you see, here the method "new", "Speak" identifiers(SEL in Objective-c) are created at the runtime! This is significant different from C++, which are determined at compile time. This is usually called Runtime Dispatching. Any advantages for doing this?

Say we're writing some wrappers for the data saved in csv. In C++ you have to bind each column with hand-writing methods. The code are mostly duplicated but you have to write it everywhere. (JavaBean is another example).

But with runtime dispatching you can do it in a easy way. The method matching is done in the runtime, one thing we can do is to capture the "Method Missing" event. Then we can forward missing `[obj attr]` method call to `[obj getField:attr]`. ([Detail example](http://langexplr.blogspot.com/2008/02/handling-call-to-missing-method-in_06.html))

In part II I'll talk about more runtime behavior in such an OOP system.
