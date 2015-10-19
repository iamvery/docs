---
name: Concepts
desc: Learn about Pakyow concepts.
---

Pakyow is a brand new approach to building websites and web apps. Its primary
goal is to be a framework that anyone can use, despite their level of
experience. At the same time, we strive to make Pakyow a serious contender in
the crowded world of web-based frameworks.

## Rationale

Pakyow was born to fill the gap between a traditional, server-rendered approach
(e.g. Ruby on Rails) and a more modern, client-side approach (e.g. Ember.js).

Many projects don't fit well into either approach and instead use a hybrid
client / server approach, with some aspects being rendered on the server and
some on the client. This forces business logic to be located both on the server
and on the client, increasing complexity.

The hybrid approach, as well as a full client-side approach, also fail to
deliver in terms of progressive enhancement. Because the view is not fully
rendered before being presented in the browser, the experience does not degrade
well when JavaScript is unavailable for some reason.

**This breaks the web**.

Websites and web apps that prioritize content should also prioritize progressive
enhancement. Isomorphic JavaScript, where code runs both on the server and the
client, promises to answer this problem, but always seems like an afterthought.

Pakyow makes it possible to build traditional, server-driven websites and web
apps that perform for the modern web. All business logic stays on the server,
the experience degrades well, all while still offering modern features such as
auto-updating views (when everything is working).
