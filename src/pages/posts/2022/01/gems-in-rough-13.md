---
setup: |
  import Layout from '@layouts/Post.astro'
  import Box from '@components/Box.astro'
  import STweet from '@components/STweet.astro'
title: "Gems in the rough #13"
description: "Why I’ve recently backtracked on configurations for CSS and image-handling."
author: Bryce Wray
date: 2022-01-04T13:09:00-06:00
lastmod: 2022-02-18T12:13:00-06:00
#initTextEditor: Ulysses
discussionId: "2022-01-gems-in-rough-13"
featured_image: "gemstones-sung-jin-cho-0d3qxUozE-0-unsplash_7315x4881.jpg"
featured_image_width: 7315
featured_image_height: 4881
featured_image_alt: "Colorful gemstones"
featured_image_caption: |
  <span class="caption">Image: <a href="https://unsplash.com/@mbuff?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">Sung Jin Cho</a>; <a href="https://unsplash.com/s/photos/gemstones?utm_source=unsplash&utm_medium=referral&utm_content=creditCopyText">Unsplash</a></span>
---

<Box cssClass="blueBox">
Each entry in the “Gems in the rough” series is a collection of tips, explanations, and/or idle observations which I hope will be at least somewhat useful to those of you with websites built by [static site generators](https://jamstack.org/generators).
</Box>

As it turns out, this edition of “Gems in the rough” is mostly about why I decided to revert this website to a couple of earlier configurations. I share these stories on the chance that they might help you justify similar choices, should they be necessary.

## Why I returned to internal CSS

Last year, I posted [two](/posts/2021/02/tailwind-head-hugo-pipes/) [articles](/posts/2021/03/tailwind-head-eleventy/) about putting a site’s CSS in the `head`, an approach generally called *internal CSS*. Not long thereafter, I [backtracked](/posts/2021/05/gems-in-rough-05/#its-all-about-the-cache) to my more traditional approach of using separate CSS files (*external CSS*) in an effort to be friendlier to the caching in visitors’ browsers.

Unfortunately, this didn’t work so well with the [long-known](https://arstechnica.com/civis/viewtopic.php?f=19&t=614905) overly aggressive caching of the Safari/[Webkit](https://webkit.org/) combo that’s the [only real browser choice Apple allows on iOS](https://infrequently.org/2021/08/webkit-ios-deep-dive/). Compared to more modern browser combos like Chrome/[Blink](https://www.chromium.org/blink) and Firefox/[Gecko](https://developer.mozilla.org/en-US/docs/Glossary/Gecko), Safari/Webkit tends to hang onto a site’s old content much longer than it should. This behavior sometimes “broke” my site on Safari/Webkit whenever I made certain significant styling changes.[^1]

In such instances, I couldn’t realistically expect my Safari-using visitors to hassle with manually dumping their browsers’ caches, nor did I want to execute a *host*-side cache dump.[^2] Either would resolve the issue each time it arose, but neither was practical.

The bottom line: I’m once again using internal CSS here and probably will keep doing so indefinitely, since I suspect there’s zero likelihood for the foreseeable future that Apple will stop requiring Webkit for iOS browsers.

<Box cssClass="yellowBox">
**Update, 2022-01-14**: Well, as it turned out, "indefinitely" was pushing it a bit. After a few days of what one might call “[CLS](https://web.dev/cls/) hell” whenever one goes from one page to another within the site, due to web fonts’ being loaded anew on each page, I reverted the site to **external** CSS. (Sigh.)
</Box>

<Box cssClass="yellowBox">
**Further update, 2022-02-01**: The actual solution was obvious---give up web fonts. Thus, today (at least for now), I did, and re-reverted the site to internal CSS *without* "CLS hell." We'll see how that works.
</Box>

<Box cssClass="yellowBox">
**Still further update, 2022-02-18**: "After further review," I'm now back to web fonts **and** back to internal CSS. (Well, *mostly* internal CSS.) I explain it in [this section](/posts/2022/02/gems-in-rough-14/#making-web-fonts-work-with-internal-css) of the [next](/posts/2022/02/gems-in-rough-14/) “Gems in the rough.”
</Box>

## A wave-off on AVIFs

A few months ago, I requested and received access to the beta test of [Cloudinary](https://cloudinary.com)’s ability to provide [AVIF](https://en.wikipedia.org/wiki/AVIF) images as one of the choices from its `f_auto` (auto-format) parameter. AVIFs generally are considerably smaller than competing formats’ versions and are supported by a growing number of browsers, at least on the desktop.

However, last week, I opted back out of the beta after reading the following in a Cloudinary blog post, “[The ROI of Adopting AVIF for Websites](https://cloudinary.com/blog/the_roi_of_adopting_avif_for_websites),” which confirmed my own anecdotal observations since being on the beta:

> .&nbsp;.&nbsp;. despite its rising popularity over the past year, AV1 *[the format behind AVIF]* has yet to win support on Safari, which relies on the OS rather than WebKit for decoding images. Additionally, **since AVIF decoding consumes a lot of CPU and battery**, mobile devices are taking a while to jump on the AVIF bandwagon. *[Emphasis added.]*

While AVIFs have notable advantages, I feel that the horsepower they require when certain devices try to decode them is a bit much at this time. Still, I’ll keep an eye on how browsers and devices evolve to handle AVIFs.

## Late-breaking news about Eleventy 1.0

As I was finishing this, I saw the following tweets from the [Eleventy account](https://twitter.com/eleven_ty) regarding the still-in-beta Eleventy 1.0:

<STweet id="1478407921264377858" />

<STweet id="1478436909957402628" />

This will be (is?[^3]) very, very cool stuff (as [Slinkity](https://slinkity.dev) fans already know). [Here’s just one example from that aforementioned updated documentation](https://www.11ty.dev/docs/languages/custom/#example-add-sass-support-to-eleventy), specifically showing how to add [Sass](https://sass-lang.com) support to an Eleventy 1.x project—and, yes, *without* all the hassles of the `package.json` method I’ve typically used in my own Eleventy-based endeavors.

[^1]:	Not even cache-targeting techniques like those about which I’ve [previously written](/posts/2020/12/hashing-out-cache-busting-fix-eleventy/) seem to work (reliably) where Safari/Webkit is concerned.

[^2]:	A host-side cache dump is simple enough to do in most cases, but it then penalizes *all* your visitors—not just the iOS users—by causing reduced performance for a while.

[^3]:	Kinda depends on whether you want to wait for Version 1.0 to reach general availability. I’ve found the 1.0 beta quite stable so far in my own admittedly limited testing, but YMMV.
