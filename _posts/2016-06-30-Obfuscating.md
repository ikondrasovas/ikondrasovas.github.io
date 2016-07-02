---
layout: post
title: Obfuscating
published: true
---

Most of business applications contains, at some level, intelectual property it is important to pretect. The resulting from long hour of work is something we should value and keep as an strategic asset.

.NET Framework platform has some great cross-platform capabilities but they come with a cost. Reverse engineering application data seems not to be that hard (altought I never wasted my time trying to tamper any assembly). 

A couple of time ago, I had to hide a kind of authorization code residing in a .NET class library. And the question that came was how to keep that inormation safe.

Encrypting the information is something that comes over the table, but if we use it, we still have to store the key and we go back to the original problem, how to keep that key safe.

The way I see, there are two choices:
* Secure the key
* Secure the decryption algorithm

But if we must embed both in our library, the alternative is [security through obscurity](https://en.wikipedia.org/wiki/Security_through_obscurity), meaning we must find a clever way to hide either or both inside our library.

We could use a [substitution cypher](https://en.wikipedia.org/wiki/Substitution_cipher) or mathematical operation. I read many people bleaming such approaches, with the argument they are not safe and should not be even considered. In my opnion, security is like quality. High quality or low quality depends on the final usage.

#Obfuscators

![Obfuscators](..\images\obfuscation.jpg)

[Gemalto Sentinel Enveloping](https://sentinel.gemalto.com/software-monetization/sentinel-envelope/) may have a good option if you familiar with HASP SRM for licensing. However the way it works does not seems to be what I need because to protect a file with sensitive information, it must wraps the application. As we plan to protect a string that will live inside a library, it seems this will not work for us.

[PreEmptive Solutions Dotfuscator](https://www.preemptive.com/products/dotfuscator/compare-editions) is another option. It is FREE for personal use (community edition). You can use post-build commands to [obfuscate an assembly](https://msdn.microsoft.com/en-us/library/hh977082.aspx) after every compilation.

