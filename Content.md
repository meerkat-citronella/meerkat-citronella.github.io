## Building our extension

The web is replete with great learning resources, but it's oftentimes difficult to capture your thoughts when reading through dense topics. Research suggests that writing down notes while you read helps [improve knowledge retention](https://www.researchgate.net/publication/277951569_The_Effects_of_Note-Taking_Skills_Instruction_on_Elementary_Students%27_Reading).

So, for this tutorial, we'll build a simple extension that saves sticky notes on any webpage. The sticky notes will persist between visit sessions, so you can always revisit your notes. Here's a simple mockup of what we'll build:

![alt text](mockup.png)

## Create React App

The best way to create a new react app is with `npx create-react-app`
..

## Chrome Extension Configuration

Chrome Extensions may seem daunting, but it's really all just JavaScript. A Chrome Extension really is just a set of JavaScript files that run alongside normal webpages. If you know how to use JavaScript, you know how to make Chrome extensions.

Chrome extensions are composed of 3 main JavaScript components: Content Scripts, Popup Scripts, and Background Scripts. Additionally, every chrome extension must include a `manifest.json` that tells Chrome how to run your extension.

![Chrome Extension Architecture](chrome extension.png)

- `manifest.json`- Used to instruct Chrome how to run your scripts.

-
-

We'll break down each script in the following sections.

### Creating the manifest

Every Chrome Extension comes packages with a file called `manifest.json`. The manifest is used to orchestrate the Chrome Extension and instruct Chrome how and where to run your JavaScript components.

### Content Script

### Popup script

### Create the Sticky Note Component

## Inject JS to the Webpage

### Modify Index.js

### Add the Extension Manifest

## Create a Settings Panel

### Create a browserAction Popup

## Persist Data in Chrome Storage

### Create a Background Script

Since our extension is very simple, we won't require a backround script. However, if you wanted to use advanced techniques such as message passing, you can create a script called `background.js`

### Make the Sticky Note a Shadow Component

## Publish to the Chrome Store

### Instructions

### Improving Your Odds of Getting Approved
