---
layout: post
title: Template Method Pattern In React
---

Over the weekend, I was reading a fantastic book on Ruby by Sandi Metz, [Practical Object-Oriented Design in Ruby](https://www.poodr.com/). In chapter 6 named Acquiring Behavior Through Inheritance, she mentioned the usage of the Template Method pattern. When I read the word "pattern", I immediately thought of the book, [Design Patterns: Elements of Reusable Object-Oriented Software](https://www.amazon.com/Design-Patterns-Elements-Reusable-Object-Oriented/dp/0201633612). Without any pause, I grabbed that book from my bookshelf and started searching for the Template Method pattern.

Here is a basic overview of the Template Method pattern from the GoF book: _"Define the skeleton of an algorithm in an operation, deferring some steps to subclasses. Template Method lets subclasses redefine certain steps of an algorithm without changing the algorithm's structure"_

Under the heading Applicability, there is this bullet:

- to control subclasses extensions. You can define a template method that calls "hook" operations (See consequences) at specific points, thereby permitting extensions only at those points.

When I read the word "hook", it got me thinking about another "Hook", React Hooks. With that in mind, I kept reading.

Under the heading Structure, I see a helpful diagram illustrating this pattern:

![Structure of Template Method](https://upload.wikimedia.org/wikipedia/commons/2/2a/W3sDesign_Template_Method_Design_Pattern_UML.jpg)

Under the heading Consequences, there is this section

Template methods call the following kinds of operations:

- hook operations, which provide default behavior that subclasses can extend if necessary. A hook operation often does nothing by default.

Essentially, the `templateMethod()` provides the structure of an algorithm. In my mind, it knows when certain methods/functions have to be called. The AbstractClass also declares primitive methods but these are empty in the AbstractClass and are meant for the SubClasses to implement. This is how the SubClasses provide the variant code while any invariant code or code that doesn't change across SubClasses stay inside the AbstractClass.

The GoF book also mentioned a helpful analogy to help you remember this pattern: _"Template method leads to an inverted control structure that's sometimes referred to as the 'Hollywood principle,' that is, 'Don't call us, we'll call you'"_

Now, you might be thinking, "Mark, you put React in the title. But you only briefly mentioned React Hooks. How do these relate?"

Let's think for a minute. If you're not familiar with basics of React Hooks, I recommend first reading my blog post to get a good intro. If you are familiar, then read on!

[Intro to React Hooks](https://spartasixzero.github.io/Intro-To-React-Hooks/)

The `useEffect` hook is one of the core hooks. `useEffect` provides you an arrow function where you can put your own custom code.

According to the React Docs: _"By using this Hook, you tell React that your component needs to do something after render"_

But here is the magic of React: you, as the consumer of React don't know exactly when in the React codebase that React invokes the custom code in your `useEffect` hook. You, as the consumer, are basically giving control of the structure of the algorithm to React.

If you think about what React Hooks were meant to replace. It's the lifecycle methods like `componentDidMount`, `componentDidUpdate` and `componentWillUnmount`. If you think about these three lifecycle methods, they are actually the primitive methods from the diagram above where the AbstractClass declared that the SubClasses can overwrite if they so choose.

The mind-blown moment is when you realize that a popular front-end library like React heavily leverages a tried-and-true design pattern which has been out since 1994. More reason that every developer should know their design patterns!
