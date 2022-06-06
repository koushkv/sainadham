---
title: How I Make This Website
description: >
  The ever-changing workflow that I use to build and maintain this website.
---

Maintaing and updating this website has become a hobby of mine, and I'm always
making changes to the way I manage it.
This page is (hopefully) up-to-date documentation on the current process,
both for myself and for anyone else who might be interested.

# Source Code

The source code for this website lives in a git repository that you can find
[here on GitHub](https://github.com/vikove/journal).

# Editing Layouts and Content

I use [Hugo](https://gohugo.io), a static website generator, to turn
[some HTML layouts](https://github.com/vikove/journal/tree/master/layouts)
and [some Markdown](https://github.com/vikove/journal/tree/master/siteContent/content)
into a bunch of HTML that I can serve as this website.

# Builds and Deploys

I host these static files on [Netlify](https://www.netlify.com/).
Each time I push a new commit to the `master` branch in GitHub, Netlify sees
that and kicks off a new build, which compiles a few lambda functions
and builds the site with Hugo.

If the build fails, I get an email with a link to some logs that I can dig
through to help me diagnose the failure. If the build succeeds, an updated
version of the site is published!

# Analytics

I don't use any client-side analytics/tracking code.
I've tried out a bunch of options for that, and found that they all gave me way
more info than I needed and subjected visitors to my website to unreasonable
amounts of third-party code.

For a while I stopped tracking viewership at all, but it is nice to have a
sense of how many people are reading my posts and which ones are getting the
most attention, so I recently started using
[Netlify Analytics](https://www.netlify.com/products/analytics/),
which is fully server-side and doesn't collect unnecessary information from
visitors.
