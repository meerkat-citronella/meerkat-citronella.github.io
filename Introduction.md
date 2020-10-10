# Introduction

## Browsers are the future!

Google Chrome is now the most popular web browser of all time, and user adoption of Chrome has accelerated over the past several years. Chrome has over 2 billion active users, and it owns 65% of market share with claims from Google of over 2 billion active Chrome browsers in 2016. Chrome is wonderful for a handful of reasons:

1. It looks and runs the same on most computers
2. It's easy to install
3. You can turbocharge Chrome using Extensions.

Regarding point number 3, Chrome extensions are an incredible way for developers to deliver delightful software to users. If you've ever watched a Mr. Beast video, you're probability familiar with [Honey](https://www.youtube.com/watch?v=aNv1qZ54YzQ)? Their software is almost entirely based around browser extensions, and Paypal just bought them for **\$4 billion**! [Grammarly](https://www.grammarly.com), another Chrome extension, recently raised \$90 million and is now valued at over \$**1 billion**! With a great idea and some hard work, perhaps you too can build a unicorn startup in Chrome :)

## Extensions are great, but not simple to build with React

In the year 2020, there are countless JavaScript frameworks that make developing for the web easier than ever. ReactJS happens to be [one of the most in-demand JS frameworks](https://twitter.com/AlexReibman/status/1203047332515926017/photo/1) for building beautiful frontend UIs. Unfortunately, Chrome extensions don't work the same as traditional web applications. At [DocIt](https://chrome.google.com/webstore/detail/docit/fmceajdookgglbnmeonlcedeoajmchpn?hl=en-US), we spent countless days toiling away with the Chrome Extension API and React when we built our Chrome Extension. Long story short, _you can't just use create-react-app, click export, and expect a working extension_.

In this tutorial, we will guide you step-by-step in building your own React-based Chrome Extension. Along the way, we will build a sticky note extension that lets users write, save, and pin notes to any webpage. [You can access the code for this extension here](https://github.com/meerkat-citronella/react-chrome-sticky-note-extension). Most of our code will be written in React, but we will also use a fair amount of vanilla JavaScript.

### Skill level

Everyone! Beginners as well as seasoned experts will learn

### Prerequisites

To get started with this tutorial, you should be familiar with:

- Vanilla JS
- React + React Hooks
- Yarn
- HTML and the DOM API

In this tutorial, you will learn:

- Turn React to Chrome-compatible JS
- Extension Background Scripts and Popups
- Modify web pages with Content Scripts and the Shadow DOM
- Publishing your Chrome Extension to the public and expedite review approval
