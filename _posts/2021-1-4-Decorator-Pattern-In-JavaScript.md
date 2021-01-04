---
layout: post
title: Decorator Pattern in JavaScript
---

The Decorator pattern is another design pattern that I came across while reading the book, [Learning JavaScript Design Patterns](https://addyosmani.com/resources/essentialjsdesignpatterns/book/) by Addy Osmani. The Decorator pattern is a structural design pattern which helps promote code-reuse.

The Decorator pattern provides the ability to add features to existing classes. You want to use this pattern when you want to add additional functionality to an existing class without changing the internals of that class. Usually, these added functionality are not critical or necessary to that class, otherwise the functionality would be baked in from the start.

The `connect()` function from react-redux is a great example of the Decorator pattern. The `connect()` function takes a React component and the returned object becomes a connected component which now has access to Redux state as well as functions it can use to dispatch actions to the Redux store.
