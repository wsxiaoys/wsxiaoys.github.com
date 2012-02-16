---
layout: post
title: "R7RS comment period is open!"
---

It has been 10 months <strike>from</strike> _since_ the announcement of 1st draft of R7RS. The 6th draft
finally came out <strike>on</strike> yesterday and a public comment period is open.

I'm copying this [message](http://lists.scheme-reports.org/pipermail/scheme-reports/2012-February/001816.html) by Marc Feeley:

    This message is being posted to various lists to inform members of the Scheme community on the development of R7RS.
    I am pleased to announce that the sixth draft version of R7RS ("small" language) has been completed by working group 1 and is now available at the following URL:

    http://trac.sacrideo.us/wg/raw-attachment/wiki/WikiStart/r7rs-draft-6.pdf

    A copy will also be posted on schemers.org .
    Other documents produced by working group 1, including previous drafts and progress reports are available at the following URL:
    http://www.scheme-reports.org/2012/working-group-1.html

    The editors of working groups 1 and 2, in consultation with the Scheme language steering committee, have provided a mechanism for comment and discussion.  For details, including instructions on how to submit a formal comment, please see this document:
    http://www.scheme-reports.org/2012/process1.html
    The comment period is now open and will continue until June 30, 2012.

    The steering committee thanks the editors for their intensive work on the draft R7RS, and looks forward to the public comment period on this sixth draft.
    Enjoy!

    For the Scheme language Steering Committee,
    -- Marc Feeley

For many programmers, a dream language's feature list may contain<strike>s</strike>:

* First-class function
* Function closure
* Garbage collection
* Hygienic macros
* Pattern matching
* etc

Scheme is such a language. For years it's considered as a language for education (Actually it's perfect in teaching class like compiler principles). But in other fields, it's not really popular, for reasons of lacking the modular development support, no official implemention, hard to manage third party codes, etc.

R7RS tries to fix these shortages. To keep <strike>the simplicity</strike> _it simple_ and powerful, the editors divided the whole language into a small one and a large one.

The R7RS-small basically adds module syntax and a few selected feature to R5RS. The large one tries to give a standard interface for third party libraries in different fields, like networking, object-oriented programming, etc. So a completely R7RS implementation should be considered battery-included.

For those never played with Scheme but want to have a taste, I strongly suggest reading the [draft](http://trac.sacrideo.us/wg/raw-attachment/wiki/WikiStart/r7rs-draft-6.pdf) and watch this [introduction(video)](http://vimeo.com/29391029) by John Cowan.
