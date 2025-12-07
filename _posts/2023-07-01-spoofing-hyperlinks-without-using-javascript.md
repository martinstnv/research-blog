---
layout: single
title: Spoofing Hyperlinks Without Using Javascript 
date: 2023-07-01
classes: wide
---

> A clever way to to spoof hyperlink locations without using JavaScirpt.

Typically, controlling a link’s behavior, showing one URL on hover but opening another on click, is done using JavaScript. Common approaches include overriding the click event with `event.preventDefault()` and manually redirecting via `window.location.href`, or dynamically changing tooltips and hrefs to simulate different hover and click URLs.

However, I’ve discovered an interesting approach that uses only HTML and CSS to override the destination of a hyperlink, achieving a similar effect without any JavaScript.

It turns out that if you nest a form inside a hyperlink, the form’s action takes precedence over the parent `<a>` element.

```html
<a href="https://first.com">
    <form action="https://second.com">
        <button type="submit"> Click me </button>
    </form>
</a>
```

In this example, hovering over the element shows `https://first.com` in the browser tooltip, but clicking the button actually submits the form and navigates to `https://second.com`.

To make the effect even more seamless, you can use a bit of CSS trickery to style the button so it looks like normal text:

```css
button {
    background: none !important;
    border: none;
    padding: 0 !important;
    cursor: pointer;
    color: -webkit-link;
    text-decoration: underline;
}
```

This way, users can be deceived into visiting an unintended page by exploiting their trust in the browser’s default behavior.

Originally, I was exploring ways to spoof hyperlink destinations in emails, a technique that, if feasible, could have significantly increased the risk of phishing attacks for users. However, just as JavaScript is typically disallowed in email clients, forms are also restricted, making this approach impractical in that context.

Nonetheless, this method can be applied in environments where JavaScript is restricted or disabled, allowing it to bypass safeguards and potentially compromise the integrity of a website.
