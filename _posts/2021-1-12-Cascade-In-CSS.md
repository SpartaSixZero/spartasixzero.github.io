---
layout: post
title: Cascade in CSS
---

Cascading Style Sheets, or CSS is one of three core technologies behind any basic webpage, the other two being Hypertext Markup Language (or HTML) and JavaScript.

Simply put, HTML gives your page structure you can think of HTML as the skeleton of your page. JavaScript allows for some more advanced interactions to happen on your page. Last but not least, CSS allows you to define the look and feel of your page.

At the core of CSS, we have this concept of the Cascade. Unfortunately for me, my CSS knowledge was limited at the time so when the concept of Cascade came up in an interview, I found myself not knowing the right answer.

Determined to overcome this obstacle, I've set myself on a path to learn more about CSS and Cascade the right way. My learning of CSS has been 3 fold:

1. I went through an informative book, [CSS: The Definitive Guide: Visual Presentation for the Web by Eric A. Meyer and Estelle Weyl](https://www.amazon.com/gp/product/1449393195/ref=ppx_yo_dt_b_asin_title_o04_s00?ie=UTF8&psc=1)
1. [I studied the resources related to CSS in MDN](https://developer.mozilla.org/en-US/docs/Learn/CSS) (By the way, MDN has an amazing set of learning articles on HTML, CSS, and JavaScript for a structured learning experience along with great exercises to test your knowledge every step of the way)
1. I purchased the Udemy course [The Web Developer Bootcamp by Colt Steele](https://www.udemy.com/course/the-web-developer-bootcamp/). This course helped me a lot with CSS but also solidified some concepts I'm more comfortable with like JavaScript, DOM, and HTML.

Now that I've completed the course and feel more comfortable with CSS, I can help others learn about Cascade, the core concept behind CSS.

## What is Cascade?

According to MDN, _"The **cascade** is an algorithm that defines how to combine property values originating from different sources."_

According to CSS: The Definitive Guide, _"...the cascade itself-the process by which conflicting declarations are sorted out and from which the final document presentation is determined"_

Here is my own understanding of Cascade. I'll start with an example. Let's say you have the HTML elements like below:

```html
<div>
  <p id="my-paragraph">Hello World</p>
</div>
```

And you also have these styles in a css stylesheet:

```css
#my-paragraph {
  color: blue;
}

div p {
  color: red;
}
```

You can also find the [example in code pen here](https://codepen.io/sunmark14/pen/WNGapJr)

In this case, what should the color of Hello World text be?

If you said blue, you're right. In our example, both CSS selectors can select our p element. The first one being an ID selector which matches the id of our `<p>` element and the second using the [descendant combinator](https://developer.mozilla.org/en-US/docs/Web/CSS/Descendant_combinator) to find a `<p>` element which is a descendant of a `<div>` element.

So why does ID selector take precedence in this case? The answer is a concept called **specificity**. In this blog post, I'll mainly focus on the concept of specificity but know that inheritance (which is the mechanism that a style applied to an element can also get applied to its descendants) as well as origin of the style declarations (which can be either user-agent stylesheets, Author stylesheets, and user stylesheets) will also contribute to the Cascade.

## Specificity

According to CSS: The Definite Guide, _"When an element has two or more conflicting property declarations, the one with the highest specificity will win out."_

To calculate specificity, you can imagine a value with 4 single digits in 4 columns:

Thousands | Hundreds | Tens | Ones

Below are some notes taken from _CSS: The Definitive Guide_

1. Thousands: Score one in this column if the declaration is inside a style attribute, aka inline styles. Such declarations don't have selectors, so their specificity is always simply 1000.

1. Hundreds: Score one in this column for each ID selector contained inside the overall selector.

1. Tens: Score one in this column for each class selector, attribute selector, pseudo-class contained inside the overall selector.

1. Ones: Score on in this column for each element selector or pseudo-element contained inside the overall selector.

Specificity applies to us because in our stylesheet above, both selectors define a style for color. If we changed the CSS to be like this below, then specificity would not matter since there isn't a conflicting style property and both selectors along with styles in them will take affect.

```css
#my-paragraph {
  color: blue;
}

div p {
  border: 5px solid red;
}
```

Now, back to our original example, let's calculate the specificity for each of the selectors, starting with this one:

```css
div p {
  color: red;
}
```

If we refer back to our list, we don't have any inline styles so nothing in the thousands position. We also don't have any ID selectors so nothing in the hundreds position. Nothing in the tens position as well since we lack class selectors, attribute selectors, or pseudo-class selectors. We can see that there are 2 element selectors for this particular select set. This means our final specificity is

`0002`

Let's now look at the other selector:

```css
#my-paragraph {
  color: blue;
}
```

Since this selector has an ID selector, we put a 1 in the hundreds position, ending up with this value for specificity:

`0100`

According to the definition from above, the ID selector wins in precedence because it has a higher specificity value.

In summary, Cascade is a key concept in CSS. Cascade is a way to determine which styles actually take affect when you have conflicting styles. Specificity is a key part of the Cascade but so is inheritance and origin of the stylesheet. I will talk about those in a later blog post. With a solid understanding of how Cascade works, an engineer can troubleshoot CSS bugs with no trouble.
