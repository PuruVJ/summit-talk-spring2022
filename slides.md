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

Vite bundles everything into one big bundle, removing the extra `import`, `export` statements, and the headers per file aren't sent too.
<!-- 16KB minified + brotli reduction is very very good, or just changing the bundler. It was also possible due to Vite's asset handling, so rather than.

-->

---
layout: two-cols
---

```js
import FinderIcon from '../assets/app-icons/finder.webp';
import CalculatorIcon from '../assets/app-icons/calculator.webp';
import CalendarIcon from '../assets/app-icons/calendar.webp';
// ...

const appsConfig = {
  finder: {
    icon: FinderIcon,
    name: 'Finder',
  },
  calculator: {
    icon: CalculatorIcon,
    name: 'Calculator',
  },
  calendar: {
    icon: CalendarIcon,
    name: 'Calendar',
  },

  // ...
}
```

::right::

<v-click>

```js
const appsConfig = {
  finder: {
    icon: '/assets/app-icons/finder.webp',
    name: 'Finder',
  },
  calculator: {
    icon: '/assets/app-icons/calculator.webp,
    name: 'Calculator',
  },
  calendar: {
    icon: '/assets/app-icons/calendar.webp',
    name: 'Calendar',
  },

  // ...
}
```

</v-click>

<style>
.slidev-layout {
  gap: 2rem;
  align-items: center;
}
</style>

<!--
And, because of how Vite handles assets, it's possible to use the image by a path relative to public folder directly. With that feature, the code went from this *Queue the code snippet* to this: ->

And, because, I followed the convention of storing the icons as the same name as key in the config object, I got rid of even the icon property ->
-->

---
layout: center
---

```js
const appsConfig = {
  finder: {
    name: 'Finder',
  },
  calculator: {
    name: 'Calculator',
  },
  calendar: {
    name: 'Calendar',
  },

  // ...
}
```

<v-click>

```svelte
<img src="/assets/app-icons/{app}.webp" />
```

</v-click>

<!--

So the code finally is simply this:

And the usage is as follows

As you can see, just using Vite made quite a big difference.

-->

---
layout: fact
---

# Era of Preact

<v-click>

## (And all it took was changing one file 😉)

</v-click>

<!--

In React ecosystem, the most recommended method to increase perf is to move to Preact. So that's what I did.

-->

---
layout: two-cols
---

# vite.config.ts

::right::

```diff
- import reactRefresh from '@vitejs/plugin-react-refresh';
+ import preact from '@preact/preset-vite';
import { defineConfig } from 'vite';

export default defineConfig({
- plugins: [reactRefresh()],
+ plugins: [preact()],
- esbuild: {
-   jsxInject: `import React from 'react';`,
- },
});
```

<!--

All I had to do was install the `@preactjs/preset-vite`, and replace the `reactRefresh` with `preact`, imported from `@preact/preset-vite`. And it's done!

This plugin automatically did the proper aliasing of `react` and `react-dom` to `preact/compat`, so I did not even have to change the imports or anything.

-->

---
layout: quote
---

# It. Just. Worked.

<!--
A Huge shoutout to Preact team for making it so easy!
-->

---
layout: quote
---

# Bundle size went down by 37KB!

### Bringing it to 110KB

<!--
Yes, bundle size before was 130KB, so the math doesn't exactly work. I had added a few more features, which resulted in a bigger bundle size. But, it's still a huge difference.
-->

---
layout: fact
---

# Goodbye styled-components 🏳

<!-- Writing styles as JS object is not fun. Not at all. So, after I moved the app to Preact, I also migrated the styles to styled-components, so I could write some sane CSS.

Folks, CSS as JS objects is overrated

-->

---
layout: center
---

<img src="/css-js-objects-skeletor-meme.jpeg" h="[50vh]">

---

# Looks much better

```tsx
const DockItemButton = styled.button`
  height: 100%;
  width: auto !important;

  cursor: default !important;

  transition: all 200ms ease-in;

  transform-origin: bottom;

  display: flex;
  align-items: flex-end;

  img {
    width: 57.6px;
    height: auto;
  }
`;

// Usage
<DockItemButton aria-label={`Launch ${appName}`} onClick={action}>
  ...
</DockItemButton>
```

<!--
It looks much better than JSS.
-->

---
layout: quote
---

# But...

<!-- It had its issues. -->

---

# Issues with Styled Components

- CSS in JS
- Need VSCode extension
- Not super great performance
- Doesn't minify
- 12KB min+gzip

<!-- CSS in JS - Not really great experience in general. Something about writing CSS inside the .ts feels wrong. Uncanny.

- Need VSCode extension - Remember the code snippet you saw before of styled-components? Notice the CSS in it was not syntax highlighted? This is true for VSCode as well. You need styled components extension to get the highlight, intellisense and all the things that make CSS support great

- Not super great performance - Because Styled Components create the styles using JS, it always slower than just providing the plain CSS on demand. OFC, this point is not very important for this project, as I never saw styled-components as a runtime perf bottleneck
-->

---
layout: center
---

# Doesn't minify

<img src="/styled-comps-unminified.png" style="background-color: white" />

<!-- What you are seeing os the final index.js bundle of the app. As you can see, there is a lot of minified js, and then you have the raw CSS written inside the js. Frankly, it looks ugly and makes the bundle size bigger for no reason. 

And yes, I tried the babel-plugin-styled-components to minify the template CSS. Didn't work with Vite, so had to make do without it.

-->

---
layout: quote
---

# So finally I got rid of Styled Components

<v-click>

## And the results were good!!

</v-click>

<!--

All the styles were moved to plain old SCSS modules.

-->

---
layout: fact
---

# <span text="red-300">110KB</span><span class="font-mono"> -> </span><span text="orange-400">85KB</span>

<v-click>

## (And added .css of 3.3KB)

</v-click>

<!-- This move brought the bundle size down from 110KB to 85KB, with a sprinkle of 3.3KB of CSS.

But more than that, it was now easier for browser too. All needed now was to parse the CSS in background and be prepared. The JS had no more control over the CSS, hence it was overall more performant than before.
-->

---
layout: fact
---

# Go Code splitting 💔🖖

<!--

Finally, I was at a point where I couldn't make the app smaller by removing dependencies anymore. Those dependencies were extremely essential for the app to work. 

So finally, I resorted to code splitting the app away using React Suspense. Up until now, all the apps were loaded on the initial load only, which wasn't needed if you don't even open the apps

-->

---

```tsx
import { AppID } from '__/stores/apps.store';
import { lazy } from 'react';

type AppNexusProps = {
  appID: AppID,
  isBeingDragged: boolean,
};

const Calculator = lazy(() => import('./Calculator/Calculator'));
const VSCode = lazy(() => import('./VSCode/VSCode'));
const Calendar = lazy(() => import('./Calendar/Calendar'));

const PlaceholderApp = lazy(() => import('./Placeholder/Placeholder'));

export const AppNexus = ({ appID, isBeingDragged }: AppNexusProps) => {
  if (appID === 'calculator') return <Calculator />;
  if (appID === 'vscode') return <VSCode isBeingDragged={isBeingDragged} />;
  if (appID === 'calendar') return <Calendar />;

  return <PlaceholderApp appID={appID} />;
};
```

<!--

This here is the `AppNexus` component. Why Nexus? Nexus in science fiction and fantasy is a world between worlds, a room with doors in it, to leads to other worlds. This is what AppNexus does. This component is responsible for loading the right app when it is requested.

-->

---
layout: fact
---

# <span text="orange-400">85KB</span><span class="font-mono"> -> </span><span text="green-300">62KB</span>

<!--

This move brought down the bundle size down immensely. Just 62KB on first load was a really good comparison. It's literally 2.5x smaller than the initial version.

-->

---
layout: quote
---

# Move to Preact

<v-click>

# Styled Components <span font="mono">-></span> CSS Modules

</v-click>

<v-click>

# Code Splitting

</v-click>

<v-click>

# Get rid of dependencies

</v-click>

<!--

Summarising, these are all the tricks I applied to reduce bundle sizes

-->

---
layout: center
---

# package.json

```json
"dependencies": {
  "@mdi/js": "^5.9.55",
  "@reach/slider": "^0.15.0",
  "@rooks/use-raf": "^4.11.2",
  "clsx": "^1.1.1",
  "date-fns": "^2.22.1",
  "framer-motion": "^4.1.17",
  "immer": "^9.0.3",
  "jotai": "^1.0.0",
  "preact": "^10.5.13",
  "react-rnd": "^10.3.3",
  "react-roving-tabindex": "^3.1.0"
}
```

<!--

Here's the dependencies until now. These are the lowest this project has ever had. 

Now, this is Svelte Summit, and I still haven't gotten around to Svelte. So, here it is.

-->

---
layout: fact
---

# The golden age of <span>Svelte</span>

<style>
  span {
    background:linear-gradient(to right, #BF953F, #FCF6BA, #B38728, #FBF5B7, #AA771C);

    -webkit-background-clip: text;
    -webkit-text-fill-color: transparent; 
    -moz-background-clip: text;
    -moz-text-fill-color: transparent;
  }
</style>

<!--

Finally, we're on Svelte. Let's talk about it now. 

-->

---
layout: quote
---

# 12th Aug <span font="mono">-></span> 13th Aug

<!--

On 12th of August, I started migrating the app from React to Svelte. I started with copying the plain JS files first, then the React ones as `.svelte` files. I would paste the content of the React file, and start removing the JSX-specific code from it. File by file.

And by the night of 13th, I was fully done with the refactor. Everything done in just 2 days. This is giant benefit of the modern web development. We maye argue howone framework is better than others, but in the grand scheme of things all are similar enough, and it is a boon for people like me who like to migrate things over to other libraries.

-->

---
layout: fact
---

# It was very hard.

<h2 opacity="0">Hidden as a placeholder</h2>

<!-- 

It was very very hard. It was a super gruelling process, very tiring, and I won't do it ever again!

-->

---
layout: fact
---

# ~~It was very hard.~~

## Just kidding 😉

<!--

It was a very, very smooth process. Unbelievably smooth. I was very surprised when the whole process got over in just 2 days!

-->

---
layout: quote
---

# The Process

