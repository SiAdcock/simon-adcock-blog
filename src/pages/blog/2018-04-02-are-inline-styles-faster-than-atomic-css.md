---
contentType: blog
path: /are-inline-styles-faster-than-atomic-css
title: Are inline styles faster than atomic CSS?
date: '2018-04-02T19:28:52+01:00'
attachments: []
related: []
---
First, a couple of definitions.

In this article, I’m using *inline styles* to mean styles applied directly to the `style` attribute of a HTML element.

[*Atomic CSS*](https://acss.io/) is a styling strategy resulting in the total atomisation of CSS classes. A CSS class may contain no more than one CSS rule. To apply a particular style rule to an element, the developer must add the appropriate class. Atomisation may be automated, with atomic CSS classes constructed and applied to HTML elements at build time.

Atomic CSS appears sometimes to be the holy grail of CSS performance. Implemented correctly, it produces the minimum possible style rules for a particular design. CSS weight and parse time is as low as it can get.

With popular libraries and frameworks like [Styletron](https://github.com/rtsao/styletron) and [Tailwind](https://tailwindcss.com/) implementing its principles behind user-friendly APIs, there is no harm to the developer experience either. Without the cascade to worry about, your CSS can be a lot more predictable.

While looking at Tailwind, a friend of mine commented that by making styles so granular, you might as well be writing inline styles.

So what's the harm in using inline styles exclusively, anyway? Well...

# The cascade

If you use inline styles exclusively, you lose the *cascading* part of Cascading StyleSheets. Is this such a bad thing? 

This subject is a can of hot potatoes, so I’d like to close the lid on it pretty quickly. Given that in an “exclusively inline” approach you’ll never need to override styles, the fact that inline styles have high specificity is moot. Atomic CSS breaks the cascade also, so maybe this is not such a compelling reason to avoid inline styles.

# Leveraging browser cache

While external stylesheets can be cached effectively by the browser, inline styles can’t, at least not to the same extent. This is the stock answer we often reach for when resisting inline styles, and it has stood for a number of years. 

However more recently this answer has become less relevant, as we increasingly look to add our critical styles (and in some cases, all our styles) to a `<style>` block within our HTML. This eliminates a server round trip and reduces the time to first contentful paint.

# Browser vendors say…

Browser vendors warn us about using inline styles. Albeit indirectly, Google recommend [not to use inline styles](https://developers.google.com/speed/docs/insights/OptimizeCSSDelivery#CSSattributes) as part of their PageSpeed Insights guidelines. Mozilla [recommend defining styles in separate files](https://developer.mozilla.org/en-US/docs/Web/HTML/Global_attributes/style) on their MDN site.

But browser vendors don’t explicitly say we can’t use inline styles, so what exactly is my point?

The point is that the vendors don’t consider inline styles to be a high priority feature that they should optimise and micro-optimise for. As such, if they had to make a choice that would be bad for inline styles or bad for, say, CSS classes, they would most likely choose to penalise inline styles. 

Note that this point is second-guessing the browser vendors’ approach to implementation, so should be taken with a pinch of salt.

# Performance

Inspired by a [Stack Overflow answer](https://stackoverflow.com/a/42726924), I ran a quick test to compare the performance of a page using Atomic CSS principles versus a page built exclusively with inline styles.

Each test page contained 100 `<p>` tags. On one page, all `<p>` tags had an inlined style attribute changing the font size. On the other, all `<p>` tags had a class that applied a `font-size` style rule.

I ran a performance test against each page using Chrome dev tools. I ran the same test 5 times and took the mean and median for each.

The page with atomic classes took 30.02ms (mean) to parse the HTML, run layout, recalculate styles and paint. The median time was 28.50ms.

The page with inline styles took 39.32ms mean (31% slower) and 36.50ms median (28% slower).

The majority of the discrepancy was in the paint time, which took twice as long in the inline styles page as in the Atomic CSS page. The style recalculation was quicker in the inline styles page.

# Conclusion

While there’s still more research to be done, I wouldn’t discourage inline styles entirely; they can often be the right choice, especially when dynamically applying styles with JavaScript. 

However, I stand on firmer ground now when I advocate Atomic CSS over exclusively using inline styles. Inline styles definitely carry a performance hit and I’d be interested to hear if anyone has conducted a test like this, or taken it further.
