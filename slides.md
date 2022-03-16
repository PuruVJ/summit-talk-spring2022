---
# try also 'default' to start simple
theme: default
# random image from a curated Unsplash collection by Anthony
# like them? see https://unsplash.com/collections/94734566/slidev
background: https://source.unsplash.com/collection/94734566/1920x1080
# apply any windi css classes to the current slide
class: 'text-center'
# https://sli.dev/custom/highlighters.html
highlighter: shiki
# show line numbers in code blocks
lineNumbers: true
# some information about the slides, markdown enabled
# persist drawings in exports and build
drawings:
  persist: false
---

<!--prettier-disable-->

# macOS Web

## Why I moved from React to Svelte

<div class="abs-br m-6 flex gap-2">
  <a href="https://github.com/puruvj" target="_blank" alt="GitHub"
    class="text-xl icon-btn opacity-50 !border-none !hover:text-white">
    <carbon-logo-github />
  </a>
</div>

---
layout: center
---

# What is macOS Web?

Fun project I made almost a year ago

<img src="/macos-latest.png" class="w-4/5" />

<!--
You can have `style` tag in markdown to override the style for the current page.
Learn more: https://sli.dev/guide/syntax#embedded-styles
-->

---
layout: fact
image: https://source.unsplash.com/collection/94734566/1920x1080
---

# Early days (React)

## (And Snowpack + MaterialUI + Framer Motion)

---
layout: two-cols
---

# Early Days (React)

- Built with React
- Styling Solution done using @mui/styles (v4, deprecated in v5)
- Dock animation done with Framer Motion

## Features:

- Dock with animation
- Static Toolbar(Non-interactive)

::right::

<img src="/first-version-demo.gif" h="1/1" w="[auto]" transform="~" class="translate-y-[-2rem] translate-x-[2rem]" scale="103" />

---
layout: quote
---

# The size should not be big, right?

<!-- For only 2 features and such little interactivity, the size shouldn't be big, right? -->

---
layout: fact
---

# It was <span class="text-blue-200">146KB <span v-click>min+br</span></span> <span v-click>ONLY JS</span>

---
layout: center
---

![](/macos-1-meme.jpeg)

---
layout: center
---

# And even the size wasn't the biggest issue...

---
layout: center
---

# Network Requests were!

<img src="/react-1-network-requests.png" class="h-[50vh]" />

<!-- As you can see, we have a giant list of requests. This is the nature of Snowpack by default. Snowpack, even in prod ships separate files as ESM. Which is not great with these many files, as even with HTTP2, these many requests are simply not as fast as one single minified file. -->

---
layout: fact
---

## And the dock animation was janky.

<!--

Dock animation works by changing the width of the individual images. So, changing widths of 7 images at once, listening to mouse hover event, and then the actual width change causing re-layout makes it drops frames. Not to mention, the whole song and dance of React's Virtual DOM.

The GIF you saw before looks smooth because it is captured on a macbook pro, but the machine I developed this version on was 700USD windows machine that frankly wasn't very good -->

---
layout: fact
---

# Hello Vite

<!-- Around February of last year, I found out about this fun little thing you might've heard of: Vite. Vite is a all-in-one bundler/dev server/build tool combined into one little delightful package! Vite is very similar to Snowpack, but much more polished, and it bundles everything into file in production. Do you see where I'm going with this? -->

---
layout: image-right
image: /react-vite-network-requests.png
---

# Size reduction by 16KB

