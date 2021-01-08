---
layout: post
title: Intro to React Hooks
---

Seems like React Hooks are all the rage these days so I thought it would be a good idea to do some guided learning on React Hooks.

Currently, I'm going through the Udemy course, [Modern React with Redux [2020 Update]](https://www.udemy.com/share/101WcYA0MYcFxaQXw=/) by Stephen Grider. Stephen a fantastic teacher and I highly recommend his courses.

## What are Hooks?

According to the official React documentation:

_Hooks are a new addition in React 16.8. They let you use state and other React features without writing a class._

_With Hooks, you can extract stateful logic from a component so it can be tested independently and reused. Hooks allow you to reuse stateful logic without changing your component hiearchy._

_Hooks let you split one component into smaller functions based on what pieces are related, rather than forcing a split based on lifecycle methods._

For this introductory post, we'll just focus on the first section. As you get further understanding of Hooks and start using it more, you'll start to understand the other points.

We will focus on how Hooks allow you to use state and other React features without writing a class.

When React first came out before the advent of Hooks, we had class based components and function components. Function components are more lightweight because they're just JavaScript functions where as class based components got more baggage added on. Also, with class based components, you get access to some things that function components just don't have, for example, state and lifecycle methods.

Please check out this link for more details on the [motivation behind Hooks](https://reactjs.org/docs/hooks-intro.html).

Fast track to February 2019, Hooks was released with React v16.

Along with it, you get these functions like `useState()` and `useEffect()`. These take care of state and lifecycle methods in function components, respectively.

## useState()

Simply put, `useState()` brings the power of state to your function components. Since function components didn't have state before, it had no way of preserving any local variables between re-renders. Function components can only take props from its parent and act on those.

Here is a simple example of `useState()`:

```jsx
const Search = () => {
  const [term, setTerm] = useState("React Hooks");

  const onInputChange = (event) => {
    setTerm(event.target.value);
  };

  return (
    <input className="input" value={term} onChange={onInputChange}></input>
  );
};
```

Search is a function component which returns a `<input>` element. It has an onChange attribute which is set to a function called onInputChange. Whenever we type into this input field, the onInputChange gets invoked.

Before we go any further, let's talk about this line of code:

```jsx
const [term, setTerm] = useState("React Hooks");
```

First, `const [term, setTerm]` is what is called destructuring assignment. What we're saying is that we know `useState()` returns an array object and we want the first element of that array, take its value and assign it to a new constant called term. Next, we want to take the second element of that array, take its value and assign it to a new constant called setTerm.

`useState()` function takes only one argument and that is the value to set as the initial value. In our example, the initial value of term will be "React Hooks".

In class based components, you can think of term as something equivalent how it is defined in the code below:

```jsx
constructor(props) {
  super(props);
  this.state = {
    term: "React Hooks"
  };
}
```

Basically, term is the piece of state that we care about.

setTerm is the function that allows us to set the value of term to anything.

Referring to the code above, whenever we invoke `onInputChange()`, we are using setTerm to set the value of term to the current value inside the input element.

Then in the return, we can see that we are using term as the value for value property of our input element, making it a [controlled component](https://reactjs.org/docs/forms.html#controlled-components).

As a recap, `useState()` gives function components the ability to use the state feature of class based components. You can both read as well as set the state value.

## useEffect()

Whenever developers need to make an API call to fetch some data, they usually put it inside the `componentDidMount()` method. The reason is that at this point, the UI has been fully rendered.

Since function components don't have lifecycle methods like componentDidMount, `useEffect()` was introduced to help with this problem.

From React documentation:

_When you call useEffect, you're telling React to run your "effect" function after flushing changes to the DOM. Effects are declared inside the component so they have access to its props and state. By default, React runs the effects after every render - including the first render._

_Effects may also optionally specify how to "clean up" after them by returning a function. For example, this component uses an effect to subscribe to a friend's online status, and cleans up by unsubscribing from it._

Below is a simple example of useEffect:

```jsx
const Search = () => {
  const [term, setTerm] = useState("React Hooks");
  const [results, setResults] = useState([]);

  useEffect(() => {
    const search = async () => {
      if (term) {
        const { data } = await axios.get("https://en.wikipedia.org/w/api.php", {
          params: {
            action: "query",
            list: "search",
            origin: "*",
            format: "json",
            srsearch: term,
          },
        });
        setResults(data.query.search);
      }
    };
    search();
  }, [term]);

  // rest of the code removed for brevity
};
```

`useEffect()` can take up to two parameters. The first is the callback function that gets invoked. By default, the code inside the callback function will always get invoked at initial render. Depending on what gets placed in the second parameter, different situations can occur:

1. If second parameter is an empty array literal, or `[]`, then the callback function will only be invoked _once_
1. If the second parameter is omitted, then the callback function will _invoke once during initial render and then again at each subsequent re-render._
1. If the second parameter is an array with values (either props or state), then the callback function will _invoke once during initial render and then again only if the values inside the array changed._

In our example above, we are saying we want to invoke the callback function to make the data call using Axios if term is truthy and that term has changed. We are also using async and await here because they allow cleaner way to handle ES6 promises. Once we get the data back, we then use `setResults()` to set our state value.

`useEffect()` can return a function as well. This function is used to do any clean up work. For example, if you added an event listener to a DOM element, the return function is a good place to add code to remove the event listener.

As a recap, `useEffect()` allows function components to run code after changes have been flushed to the DOM. This point in time is perfect for making API calls.
