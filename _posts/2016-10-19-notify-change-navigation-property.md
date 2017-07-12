---
layout: post
title: How to Notify Changes on Navigation Properties
published: false
---

Suppose you have a ViewModel, and one of the properties of this ViewModel is an ObservableCollection. 
How can you get a notification on this ViewModel about a property change of an item of this collection.

We know that raising a property changed even on the collection setter does not work, since we are not interested in changes of the collection itself, but in changes of its inner elements.

 
