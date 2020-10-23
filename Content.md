## Building our extension

The web is replete with great learning resources, but it's oftentimes difficult to capture your thoughts when reading through dense topics. Research suggests that writing down notes while you read helps [improve knowledge retention](https://www.researchgate.net/publication/277951569_The_Effects_of_Note-Taking_Skills_Instruction_on_Elementary_Students%27_Reading).

So, for this tutorial, we'll build a simple extension that saves sticky notes on any webpage. The sticky notes will persist between visit sessions, so you can always revisit your notes. Here's a simple mockup of what we'll build:

![alt text](mockup.png)

## Create React App

The best way to create a new react app is with `npx create-react-app`
..

We are going to focus first on just building the react app, and then add the chrome extension functionality later.

Run `npx create-react-app [YOUR_APP_NAME]` (we named ours react-chrome-sticky-note-extension) from a command line instance, and `cd` into the resulting directory, and open your favorite code editor (we use VSCode). This will give you the following boilerplate file structure:

![boilerplate react directory structure](react-boilerplate-dir-structure.png)

Right off the bat we are going to download a module to help us out with styling our app. Frmo the root directory of your app (i.e. YOUR_APP_NAME), install styled-components into your app, with either `yarn add styled-components` or `npm install styled-components`

Let's start with the file `App.js`. This will be the main component for our app. Let's go ahead and rename it, to something like `StickyNotes.js`. Also change the name of the component being rendered in the file in a similar fashion, from `App` to `StickyNotes`. Adjust the corresponding import in `index.js` (or let VSCode automatically updated it for you). Go ahead delete all the boilerplate in the return statement of the `StickyNotes` component.

Next, we are going to make some styled components to use in our `StickyNotes` component. At the top of `StickyNotes.js` add the following:

StickyNotes.js

```jsx
// import the module
import styled from "styled-components";

// the components
const Container = styled.div`
  z-index: 2;
  border: 1px solid grey;
  position: absolute;
  background: white;
  top: ${(props) => props.y + "px"};
  left: ${(props) => props.x + "px"};
`;

const Header = styled.div`
  height: 20px;
  background-color: papayawhip;
`;

const StyledButton = styled.button`
  height: 20px;
  border: none;
  opacity: 0.5;
  float: right;
`;

const StyledTextArea = styled.textarea`
  color: dark grey;
  height: 200px;
  width: 200px;
  border: none;
  background-color: hsla(0, 0%, 100%, 0.2);
`;
```

if you're unfamiliar with styled-components, it's a very popular styling solution for React and you can read more about it here.
Note that in `Container` we are passing props. More on this later.

We are building this app with React Hooks. If you are unfamiliar with Hooks, you can read about them here. Hooks seem to be here to stay, and wonderfully abstract away a lot the extraneous typing that has to happen in a React class component.

Let's create a stateful variable to hold our notes data. We will use the `useState` React hook:

StickyNotes.js:

```jsx
// import useState
import React, { useState } from 'react'

const StickyNotes = () => {
  const [notes, setNotes] = useState([])

  ...

}
```

First, let's create the funcionality that let's us add a sticky note. We will be using shift + click. Let's add a listener to the component:

StickyNotes.js:

```jsx
// add useEffect to imports
import React, { useState, useEffect } from "react";

...

const StickyNotes = () => {
  const [notes, setNotes] = useState([])

  // listen for shift + click to add note
  useEffect(() => {
    const clickListener = (e) => {
      if (e.shiftKey) {
        setNotes((prevNotes) => [...prevNotes, { x: e.pageX, y: e.pageY }]);
      }
    }
    document.addEventListener("click", clickListener);
    return () => document.removeEventListener("click", clickListener);
  }, []);

  ...

}
```

Few things to note about this `useEffect` function. Note that we are removing the listener on `useEffect` return. You can read more about cleaning up effects here. You should always remove your listeners in React, as subsequent renders will keep adding listeners unless they are removed. Note that we are using a named function for the listener callback, another necessity if we want to remove the listener (needs a reference a named function). Lastly, note the empty dependency array `[]` as the second and final argument of the `useEffect` callback function: this is intentional, as we only need to set the listener once, on the first render. You can read more about dependency arrays in regards to `useEffect` here.

For the `setNotes` function call, we are making use of JavaScript's wonderfully expressive object literal notation, and using the splat operator `...` to set our `notes` variable. You can read more about object destructuring and the splat operator here. `e.pageX, e.pageY` are the pixel coordinates of the click on the page.

Remember the props we set up to be passed to our `Container` styled component above? Let's bring that into play. Let's set up our `return` statement:

StickyNotes.js:

```jsx
...

const StickyNotes = () => {

  ...

  return (
    <div>
      {notes.map((note) => (
        <Container x={note.x} y={note.y}>
          <Header>
            <StyledButton>X</StyledButton>
          </Header>
          <StyledTextArea />
        </Container>
      ))}
    </div>
  );
}

```

The coordinates we gleaned from our shift + click listener are passed via props to the `Container` styled component, which then uses those coordinates to absolutely position itself on the page! You should now have some functionality that looks like this:

![basic sticky note functionality](basic-note.gif)

Although we are able to enter text into the `textarea`, it is not being saved at all. To do this, we need to turn the `textarea` into a controlled component. You can read more about React controlled components here. This is standard practice for React forms. We will save the note text from `textarea` alongside the coordinate data in the `notes` variable.

In `StickyNotes.js`, update the component return statement to:

```jsx
...
const StickyNotes = () => {

  ...

  return (
    <div>
      {notes.map((note) => {
        const handleChange = (e) => {
          const editedText = e.target.value;
          setNotes((prevNotes) =>
            prevNotes.reduce(
              (acc, cv) =>
                cv.x === note.x && cv.y === note.y
                  ? acc.push({ ...cv, note: editedText }) && acc
                  : acc.push(cv) && acc,
              []
            )
          );
        };

        return (
          <Container x={note.x} y={note.y}>
            <Header>
              <StyledButton>X</StyledButton>
            </Header>
            <StyledTextArea onChange={handleChange} />
          </Container>
        );
      })}
    </div>
  );
}
```

We again make use of the splat notation, as well as terneray operators (read more here) and the `&&` notation. `setNotes` here is identifying the note that is being edited (by comparing the coordinates of the note (`cv`) with the coordinates in the locally-scoped `note` variable) and adding (or editing) the `note` property.

The `textarea` is now a controlled component. You can log `notes` to the console, and see that is updating as you edit a note.

Next, let's add functionality for the delete button. Change the component `return` statement to add the following:

StickyNotes.js:

```jsx
...
return (
    <div>
      {notes.map((note) => {

        ...

        const handleDelete = () => {
          setNotes((prevNotes) =>
            prevNotes.reduce(
              (acc, cv) =>
                cv.x === note.x && cv.y === note.y ? acc : acc.push(cv) && acc,
              []
            )
          );
        };

        return (
          ...
              <Header>
                <StyledButton onClick={handleDelete}>X</StyledButton>
              </Header>
          ...
        );
      })}
    </div>
  );
```

This is the core functionality of the app! We can add notes, edit them, and delete them, and it is all saved in a single stateful variable. Next we'll turn this humble React app into a Chrome extension.

## Chrome Extension Configuration

Chrome Extensions may seem daunting, but it's really all just JavaScript. A Chrome Extension really is just a set of JavaScript files that run alongside normal webpages. If you know how to use JavaScript, you know how to make Chrome extensions.

Before we start coding, let's break down how Chrome Extensions are configured.

Extensions are generally composed of 3 main JavaScript components: Content Scripts, Popup Scripts, and Background Scripts. Additionally, every chrome extension must include a `manifest.json` that tells Chrome how to run your extension.

However, things are a bit different when we're working with React. When you build a React app, all of the code gets bundled into a single js file by Webpack. Hence, all of our our JS that affects UI (content scripts and popup scripts) will be bundled into a single file, `main.js`.

![Chrome Extension Architecture](ChromeExtension.png)

- `manifest.json`- Used to instruct Chrome how to run your scripts.

- `background.js`- A script that runs behind the scenes from your active web page. This script is usually used for advanced techniques such as message passing or running async computations.

- Popup- Popup is the window that launches when you press the extension button on the Chrome Toolbar.

  - `popup.html`- The HTML document that renders when you click the extension button. `popup.js` will be used here.

  - `popup.js`- Code that is excecuted in the popup window. This script will automatically be bundled into `main.js`.

- `contentScript.js`- A content script is a js file injected into the web page that the user is viewing (i.e. wikipedia.org, google.com, etc.). This script will be responsible for making any changes to the current wepbage being viewed. This script will automatically be bundled into `main.js`.

- Assets- Any images, css, and other files used by your extension.

We'll break down each script in the following sections.

### Creating the manifest

Every Chrome Extension comes packages with a file called `manifest.json`. The manifest is used to orchestrate the Chrome Extension and instruct Chrome how and where to run your JavaScript components.

First, create a `manifest.json` and add it to the `public` directory. Here is what our `manifest.json` should look like:

```json
{
  "name": "React Sticky Notes App",
  "version": "1.0.0",
  "manifest_version": 2,
  "description": "Lets you annotate web pages and persists those annotations across page visits.",
  "content_scripts": [
    {
      "matches": ["<all_urls>"],
      "js": ["./main.js"],
      "css": ["/main.css"]
    }
  ],
  "browser_action": {
    "default_popup": "./popup.html"
  },
  "permissions": ["storage"]
}
```

#### Metadata

- `name`: The name will appear when on Chrome Extension Store as well as when you select or hover your extension.
- `version`: The version of your extension. You can start at any number; however, every time you push an official update to the Chrome Store, you must increment your version.
- `description`: Short description of what your app does. Note: This description is entirely separate from the description you will provide on the Chrome store page.

Your metadata will be visible at the url `chrome://extensions`
![Metadata in the extensions tab](name.png)

#### Scripts and permissions

- `content_scripts`:

  - `matches`: This is a required field that tells Chrome which pages to run your extension on. Since we are interested in using our extension everywhere on the web, we leave this field as `["<all_urls>"]`. Otherwise, you can specify a [string matching pattern](https://developer.chrome.com/extensions/match_patterns).
  - `js`: Since we are interested in rendering sticky notes onto our web pages, we need to inject JavaScript onto our page to do it. As mentioned earlier, Webpack will bundle all of the JS in our project into a single `main.js` file. So, we will make this field `["main.js"]`.
  - `css`: Our React code also needs css to make the components look good. So, we need to include the `main.css` file created by Webpack.

- `browser_action`: Browser action is used to specify details that are associated with the button on the Chrome Toolbar.

  - `default_popup`: The popup action can render an HTML document. We will create a special HTML file called `popup.html`with instructions to run `main.js` and render our React Components.

- `permissions`: Listing permissions in this array gives you access to certain API operations such as `chrome.storage` and `chrome.bookmarks`. For our extension, we will only need access to `chrome.storage`, so we will use `storage`. Note: When you publish to the Chrome store, **you cannot request more permissions than your extension actually uses.** If you do, your application will be rejected by Chrome store reviewers.

Our popup script will communicate to the content script (and vice versa) by using the `chrome.storage` API as an intermediary data store.
![Communication between scripts](communication.png)

## Make React Compatible with Chrome

Remember at the beginning we mentioned that extensions are just JavaScript? This is true, but not all JS is created equal. Most JS projects are a mix of JSX, Typescript, JSONs, and other assets spread across multiple files. If you try moving your files into your extension's package, Chrome won't know how to read your files and fail to run your extension.

TODO:
As mentioned earlier, a valid Chrome extension package needs a `manifest.json` and set of files and assets to use.

### Prevent JavaScript file splitting

By default, Create React App is configured with Webpack. According to Webpack's documentation: "Code splitting is one of the most compelling features of webpack. This feature allows you to split your code into various bundles which can then be loaded on demand or in parallel."

Unfortunately for us, code splitting makes it difficult to package our code into an extension.

Additionally, Webpack adds random hashes to the files it builds. (This is to prompt browser to re-fetch files that may have changed between builds instead of relying on cached files). However, this also poses an issue for us because unless we account for the hash changes, we'll have to change our `manifest.json` to match the new files names every time we build.

![file_splitting](file_splitting.png)

To prevent Webpack from making our extension unusable, we need to run a script to prevent code-splitting. This script allows us to without ejecting from Create React App.

```js
// build-non-split.js
const rewire = require("rewire");
const defaults = rewire("react-scripts/scripts/build.js");
let config = defaults.__get__("config");

config.optimization.splitChunks = {
  cacheGroups: {
    default: false,
  },
};

config.optimization.runtimeChunk = false;
```

Next, we need to modify our `package.json` to prevent code splitting and hashing.

```json
// package.json
  ...
  "scripts": {
    "start": "react-scripts start",
    "build": "react-scripts build",
    "build:extension": "node ./scripts/build-non-split.js && yarn build:clean",
    "build:clean": "cd build && mv static/js/*.js main.js && mv static/css/*.css main.css && rm -r static",
    "test": "react-scripts test",
    "eject": "react-scripts eject"
  },
  ...
```

We modified `package.json` to include 2 additional commands: `build:extension` and `build:clean`.

1. `build:extension` will run the `build-non-split.js` file and prevent Webpack from splitting the JS.
2. Next, `build:clean` will rename the bundled js and css into a single set of files called `main.js` and `main.css` respectively. Finally, we remove the `static` directory, since it is no longer needed.

From now on, when we are building our extension, we should run `yarn build:extension`.

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
