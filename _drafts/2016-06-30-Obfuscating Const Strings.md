---
layout: post
title: Obfuscating Const Strings
published: true
---

How we can hide a const string containing sensitive data?

#Introduction

We basically have two choices:
* Secure the key
* Secure the decryption algorithm

But if we must embed both in our library, the alternative is [security through obscurity](https://en.wikipedia.org/wiki/Security_through_obscurity), meaning we must find a clever way to hide either or both inside our library.

We could use a [substitution cypher](https://en.wikipedia.org/wiki/Substitution_cipher) or mathematical operation...

#Obfuscators

[Gemalto Sentinel Enveloping](https://sentinel.gemalto.com/software-monetization/sentinel-envelope/) could have a good option, since we already use HASP SRM for licensing. However the way it works does not seem to be what we need, since to protect a file with sensitive information, it must wraps the application. As we plan to protect a const string that will live inside a library, it seems this will not work for us.

[PreEmptive Solutions Dotfuscator](https://www.preemptive.com/products/dotfuscator/compare-editions) is another option. It is FREE for personal use (community edition). You can use post-build commands to [obfuscate an assembly](https://msdn.microsoft.com/en-us/library/hh977082.aspx).


#Testing
In case we choose an Obfuscation tool it is important to test if it works properly on const strings. There may be some cases [like this one](http://stackoverflow.com/questions/20053539/should-hasp-vendor-code-be-encrypted-obfuscated) reporting problems while using Enveloping.
