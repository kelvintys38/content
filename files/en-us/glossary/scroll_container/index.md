---
title: Scroll container
slug: Glossary/Scroll_container
page-type: glossary-definition
sidebar: glossarysidebar
---

A **scroll container** is an element box in which content can be scrolled, whether or not scroll bars are present. A user agent adds scroll bars to an element box to make it a scroll container when the CSS {{cssxref("overflow")}} property is set to `scroll` or when `overflow` is set to `auto` _and_ the content overflows the container.

When the content of an element box overflows its bounding box, users can use scroll bars to scroll through the clipped content that is otherwise hidden from view.

A scroll container includes a scrollport and scroll bars.

## Scrollport

The scrollport is the visible part of a scroll container and coincides with the padding box of the scroll container. The scroll bars are used to move content in and out of the scrollport so that the content can be viewed.

## See also

- [Learn: Overflowing content](/en-US/docs/Learn_web_development/Core/Styling_basics/Overflow)
- [Scroll snapping](/en-US/docs/Glossary/Scroll_snap), including [scroll snap container](/en-US/docs/Glossary/Scroll_snap#scroll_snap_container)
- [CSS overflow](/en-US/docs/Web/CSS/CSS_overflow) module
- [CSS overscroll behavior](/en-US/docs/Web/CSS/CSS_overscroll_behavior) module
- [CSS scroll snap](/en-US/docs/Web/CSS/CSS_scroll_snap) module
