## Building our extension

The web is replete with great learning resources, but it's oftentimes difficult to capture your thoughts when reading through dense topics. Research suggests that writing down notes while you read helps [improve knowledge retention](https://www.researchgate.net/publication/277951569_The_Effects_of_Note-Taking_Skills_Instruction_on_Elementary_Students%27_Reading).

So, for this tutorial, we'll build a simple extension that saves sticky notes on any webpage. The sticky notes will persist between visit sessions, so you can always revisit your notes. Here's a simple mockup of what we'll build:

![alt text](mockup.png)

## Create React App

The best way to create a new react app is with `npx create-react-app`
..

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

Here is what our `manifest.json` should look like:

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

Our popup script will communicate to the content script (and vice versa) by using the `chrome.storage` api as an intermediary data store.
![Communication between scripts](communication.png)

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
