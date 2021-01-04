---
layout: post
title: Accessibility with labels
---

According to MDN: _"Accessibility (often abbreviated to A11y—as in "a" then 11 characters then "y") in Web development means enabling as many people as possible to use Web sites, even when those people's abilities are limited in some way."_

`<label>` elements are a must have in every web page to make it more accessible. Every `<input>` element should have a matching `<label>` element. When you add a `<label>` element and link it with an `<input>` element, you enable 2 things:

1. If the user is using a screen reader, if the user focuses on the `<input>` element, the screen reader will now read the content of the `<label>`. This greatly improves accessibility for those with a vision disability.
1. If you click the `<label>` element, it will actually focus the `<input>` element. This provides a greater hit area making it easier to use for those with a touch-screen device.

For more details, [MDN page on label element](https://developer.mozilla.org/en-US/docs/Web/HTML/Element/label)

Accessibility or A11y, should be included in the design of every project from the start.
