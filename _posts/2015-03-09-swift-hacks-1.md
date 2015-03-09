---
layout: post
title: 'Swift Hacks #1'
---
Occasionally I get caught up trying to deal with some real world puzzle type situation
in the context of a programming language of interest. In the past, this happened most
often on IRC and the results were generally lost to the log archive. More recently, 
however, these sessions get kicked off by posts on [/r/swift][rswift] or questions
on [Stack Overflow][].

It occurred to me that it might be nice to have some of the results referenced from my
own personal site as well. In general, the results are unrefined and often come with a big
asterisk referring to the questionable nature of the results. As such, "hack" seems an apt
term to use in these cases.

Today's "Swift Hack" revolves around the desire to use enumerations with associated
raw values that are not `String` or `Int` literals. Have a look at the code which will be
followed by several clarifying comments.

Please note that the contents of this gist are intended for use in a Playground in Xcode.

{% gist ztj/c1ae212cef5ed3554703 %}

This approach is pretty scary, since it involves parsing a string. The upshot is that
even though we have no compile time protection, there is still runtime protection in the
form of an early abort of our process if the inputs are not valid for the types. It is
possible to make this even more explicit, but, as I said... it's a hack.

The quick explanation of what I'm doing is

1. We're trying to make an enum with raw values of a struct type, `RegionStruct`
1. You can't do that, normally due to the requirement that raw values be literals.
1. It's even worse as they have to be one a subset of the literals we're familiar with.
1. We made our `RegionStruct` implement `StringLiteralConvertible` which was a bit of a task in and of itself.
1. We also had to make it `Equatable` for the sake of full enumeration compatibility.
1. And hey, it worked, but, we have to rely on `String` parsing during runtime when the enum is loaded to make it all work.

So there you have it. As long as you are okay with early runtime failure in lieu of proper
compilation time failure, you can do this sort of thing with just about any type that you
can make literally convertible from String or Int or whatever else can be used in the scope
of a raw value enumeration.

[rswift]: http://reddit.com/r/swift "/r/swift"
[Stack Overflow]: http://stackoverflow.com/