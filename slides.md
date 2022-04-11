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

It's a project I made for fun. Started 1 and half year ago, kept iterating on it, kept adding features

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

# It was <span class="text-red-300">146KB <span v-click>min+br</span></span> <span v-click>ONLY JS</span>

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

# <span text="orange-400">85KB</span><span class="font-mono"> -> </span><span text="yellow-300">62KB</span>

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
layout: fact
---

# Results

---
layout: fact
---

# 🥁 

---
layout: fact
---

# <span text="yellow-300">62KB</span><span class="font-mono"> -> </span><span text="green-300">28.5KB</span>

<v-click>

## 🤯🤯🤯

</v-click>

<!--

This brought the size down to 28.5KB!!

This is literally less than half of before. And that is just from moving to Svelte!!

-->

---
layout: center
---

![](/to-infinity-and-beyond-meme.jpeg)

<!--

And the runtime performance increased drastically. Drastically. Earlier, there was still slight jank in how fast the menus opened up, in how the dock animation worked. But now it was all gone. Everything was super smooth.

-->

---
layout: fact
---

# The Process

<!--

Overall moving to Svelte was a very smooth experience. Here are a few snippets of the whole process.

-->

---

# Local State

```js
import { useState } from 'preact/hooks';

const [xPos, setXPos] = useState(0);
const [yPos, setYPos] = useState(0);
const [isMenuVisible, setIsMenuVisible] = useState(false);
```

<v-click>

Became this 👇

```js
let xPos = 0;
let yPos = 0;
let isMenuVisible = false;
```

</v-click>

<!-- 

This...

Became this..

Already the boilerplate reduction begins at the most basic concept of modern web dev: Local State.

-->

---
layout: quote
---

# What about global state?

<!--

Earlier I was using Jotai in the Preact version. The reason for choosing Jotai was how minimalistic it feels. It feels very very close to Svelte stores, compared to anything else in the React ecosystem.

-->

---

# Jotai syntax

```js
import { atom, useAtom } from 'jotai';

// Create global store
const themeAtom = atom('light');

// Inside the component
const [theme, setTheme] = useAtom(themeAtom);

// Accessing `theme`
console.log(theme);

// Changing `theme`
setTheme('dark');
```

<!--

OFC, it doesn't look anything like svelte stores or state, it looks closer to `useState` hook, but compared to what you get in React ecosystem, with Redux, Context API etc, this is really great.

-->

---

# Global state in Svelte

```js
import { writable } from 'svelte/store';

const theme = writable('light');

// Accessing `theme`
console.log($theme);

// Changing `theme`
$theme = 'dark';
```

<!--

This became this. Not a lot of difference but if you repeat it over and over in different places, it actually makes the code much smaller. Just import and use. That's it. No need to declare `theme, setTheme` variables.

-->

---

# What about Styles?

Styles were already plain `.scss`, so all it took was inlining them into the components directly by copy pasting.

<!--

That's it!

-->

---
layout: fact
---

# Observations

<!--

Finally, let's talk about my observations, what all I learned from this process.

-->


---

# Svelte code is much simpler!

<v-click>

<img class="w-3/5" src="https://www.puruvj.dev/media/macos-preact-to-svelte--svelte-simplicity-tweet.png" />

</v-click>

<!--

Svelte being an extension of HTML rather than JavaScript already made it much simpler than the previous codebase in JSX. All indentation was reduced by 2 levels automatically

Plus the constructs in it like the blocks and all made it even more cleaner looking.

Heck I even tweeted it out.

As you can see here, the code on right is larger. That's because of how little code svelte requires, compared to JSX. There's also lesser dependencies on the code on the right. No ternaries, so `clsx` for dynamic classes, just use svelte's features and get instantantly beautiful code.

And to top it off, some components got smaller too, some by as much as 50%, and at least 3 components weren't even needed, they were just merged into bigger components directly, and overall code stayed simple enough only. It was an absolute win win.

-->

---
layout: fact
---

# Svelte Motion = 🔥

<!--

macOS is an Operating system, and no OS is complete without animations. This is where Svelte's built in Motion stores and transitions come in.

Svelte's Motion stores are extremely lightweight, and extremely powerful. They power the dock animation when you hover over it, the window close and open animations, dialog open and close animations, and using them is extremely easy.

Previously I was using Framer Motion. Framer Motion is a great library and my go-to thing for animations in P/React. But there's just one little issue with it: It's not exactly small 🥲

-->

---

![](/framer-motion-bundlephobia.png)

<!--

Framer Motion v4 stands at 32KB gzip. For comparison, that is nearly as much as the whole app after moving to Svelte.

Oh, and another advantage of moving off of the React ecosystem then: Framer Motion v6, the latest one is 10kb more than framer motion v4, gzip and minified. So, if I had stayed on React until now, the size would probably have increased without me doing anything.

So yeah, I got off at the right time 😅

But ofc, this is not shade at framer motion. Framer motion can do a thousand things under the sun. It's size is indicative of its capabilities. There's no such thing as a good tool or a bad tool. There's only such thing as the right tool for the job.

-->

---
layout: fact
---

# Framer Motion was overqualified

<!--

Framer Motion was wayyyy too overqualified for my use case. I didn't need layout animations, or staggered animations or any of that thing, all I needed were very simple animation primitives, like entry and exit animations, spring and tweened animations. Nothing too fancy

-->

---
layout: quote
---

# And Svelte provides these out of the box 🤩

---
layout: center
---

![](/dock-bounce-animation.gif)

<!--

Let's have a look at an example. When you click on any of the app icons on the dock, the icon bounces up and down. Let's compare the code for this in framer motion and with svelte motion.

-->

---
layout: two-cols
---

```js {1-11|1|4|5|6|7|8|1-11}
const [animateObj, setAnimateObj] = useState({ translateY: ['0%', '0%', '0%'] });

<motion.span
  onTap={() => setAnimateObj({ translateY: ['0%', '-39.2%', '0%'] })}
  initial={false}
  animate={animateObj}
  transition={{ type: 'spring', duration: 0.7 }}
  transformTemplate={({ translateY }) => `translateY(${translateY})`}
>
  {/* Markup */}
</motion.span>;
```

::right::

```svelte {1-22|2-5|18|19|7-15|11|14|1-22}
<script>
  const appOpenIconBounceTransform = tweened(0, {
    duration: 400,
    easing: sineInOut,
  });

  async function openApp(e: MouseEvent) {
    /* State related stuff */

    // Animate the icon
    await appOpenIconBounceTransform.set(-39.2);

    // Now animate it back to its place
    appOpenIconBounceTransform.set(0);
  }
</script>

<button on:click={openApp}>
  <span style="transform: translate3d(0, {$appOpenIconBounceTransform}%, 0)">
    <!-- Stuff -->
  </span>
</button>
```

<style>
  .slidev-layout {
    gap: 2rem;
  }

  code {
    font-size: 0.5rem;
    line-height: 0.7 !important;
  }
</style>

<!--

On the left, you have the code in framer motion. And on the right it's the svelte motion code. Clearly, framer motion one looks much smaller. But, let's read both of them once.

- Hit next -

Here we have an object `animateObj`. This contains the CSS properties to be manipulated, hence we have `translateY` as an array here. The array contains the keyframes. Right now it's all `0` to indicate to not animate initially.

- Hit next -

Next up we have an event listener on the `span`. Whenever you click or tap on it, it will trigger the animation. The keyframes become: 0% initially, then should reach -39.2% and then return to 0% again. This makes the app icon bounce.

- Hit next -

Next up, don't let the animation run on component mount.

- Hit next -

Pass the animateObj to the motion component to let it know what to animate. In this case, it's only the `translateY` property.

- Hit next -

Next up, defining the animation type. I want this to be a spring animation, and should go on for 700ms. Easy.

- Hit next -

And finally, `transformTemplate` to make sure the `transform3d` is not applied, which is what framer motion does by default. 

- Hit next -

Let's move on to the svelte one on the right

- Hit next -

First up, defining a tweened store. Because I want to control the duration of the animation, it is a `tweened` and not a `spring`. However, to give it a natural feel, it uses the `sinInOut` easing.

- Hit next -

Next up we got the button, which on click will trigger the animation.

- Hit next -

This is the `span` that will actually be transformed. You can see the transform style applied to it.

- Hit next -

And, coming back to the actual animation implementation, the `openApp` function. This is what actually handles the bounce animation. And, if you see the bounce animation, technically it's just two lines. Line number 11.

- Hit next -

We tell it to change the value to -39.2. Notice the `await` keyword. We are waiting for the animation to finish before we move on.

- Hit next -

When the animation to go above is finished, we move on to the next part, which is bringing it down. And that is simply setting it to 0.

Notice how intuitive this version is. It legit reads like this: "Go up to -39.2, and when you are done with that, come back down". As simple as that. It's easy to scan and make sense of. Framer motion one is smaller, but if I wanted to make the animation more bouncy, or extreme, I would have to search a lot of framer motion docs to figure out how to do, whereas in the svelte one, I would be confident changing it.

-->

---
layout: fact
---

# Those transitions 😍

<!--

Transitions in Svelte are basically animations to be run when some DOM is created or destroyed. This is nearly impossible to do in P/React without relying on external libraries like Framer Motion. But in Svelte, this is in-built and extremelyyyyyyy easy to do.

-->

---

# Window close transition

```svelte
<script>
  function windowCloseTransition(el, { duration = 300 }) {
    const existingTransform = getComputedStyle(el).transform;

    return {
      duration,
      easing: sineInOut,
      css: (t) => `opacity: ${t}; transform: ${existingTransform} scale(${t})`,
    };
  }
</script>

<section out:windowCloseTransition></section>
```

<!--

So, ofc, I put in transitions everywhere I could. Earlier, when you opened/closed any app, there was no transition. Now, when you close a window, it collapses into itself while closing, giving an awesome transition.

This little piece code allows the window to close with a transition. I added this animation when I moved to Svelte version, because the react version with Framer Motion just felt too complex, so I didn't even bother with it.

-->

---
layout: center
---

<img src="/window-close-animation.gif" h="[50vh]" />

<!--

And this is what you get. Looks good for just 13 lines of code.

Ofc, it looks janky due to GIF's low framerate. But if you try it yourselves, it looks good.

-->

---
layout: fact
---

# package.json?

<!--

Remember, I showed you the package.json that the React version had? 

-->

---
layout: two-cols
---

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

::right::

<v-click>

```json {1-6|5}
"dependencies": {
  "@mdi/js": "^5.9.55",
  "date-fns": "^2.23.0",
  "popmotion": "^9.4.0",
  "svelte-drag": "^1.3.1"
}
```

</v-click>

<style>
  .slidev-layout {
    gap: 2rem;
  }
</style>

<!--

This is the package.json of the preact version. Only the dependencies property.

- Hit next -

And this is it now. 2.75x smaller.

But, lemme blow your mind even more. That dependency you see at the bottom?

- Hit next -

I created it. A month before I did the transition from React to Svelte. I created it as a fun exercise, because svelte ecosystem didn't have a tiny draggable library with the features I wanted. And it is just 2KB in size.

So, this is a homecooked library, and very very small, so we can safely remove it from the comparison.

-->

---
layout: fact
---

# 3.67x smaller

<!--

That means the number of dependencies is actually 3.67x smaller, than before, in sheer number. And no, it isn't because I repurposed the app logic to not use the dependencies, it's that most of the stuff is already built into Svelte. I didn't do a lot of work.

-->

---
layout: fact
---

# <span text="red-300">146KB</span><span class="font-mono"> -> </span><span text="green-300">28.5KB</span>

<style>
  .slidev-layout {
    padding: 2rem;
  }
</style>

<!--

So, in the end, this brings us to the final bundle size comparison. From 146KB to 28.5KB, this led to a reduction of more than 5 times in bundle size.

But hey, that's not even the most important part. Initially, bundle size and performance were the things I always gave the highest priority. Not anymore. Now it's the readability and simplicity aspect that I value the most. Svelte gives me a lot of confidence. If I ever want to add something to it, I'll be able to do so very confidently. The code is very very readable and simple.

-->

---
layout: fact
---

# Final words

<!--

Really, moving to Svelte made me realize how extremely powerful it is. A lot of React developers treat Svelte as a toy, and I can't blame them, I was one of them once. But really, it isn't a toy. In fact I'd say it's more powerful than React. Yes, less control over everything than React, but it is a better choice for 95% of developers than React anyway.

-->

---

<h1>Thank you!</h1>