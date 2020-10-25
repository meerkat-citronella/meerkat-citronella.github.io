Hello!

Today, we are going to be creating a chrome extension using react, and going over some of the key techniques we used to create it. The app we will be creating is a sticky notes app that allows you to save sticky notes on a web page, and saves both the note and its position on the page to the chrome storage api. For a full breakdown of our code and a step by step guide to building this app, see the accompanying video. This video is just for demoing the app, and calling attention to some of the key techniques we employed.

The React portion of the app is pretty straightforward, nothing too out of the ordinary. We used React hooks to build it.

To package the React app as a chrome extension, we employed a technique to prevent the build script from splitting the built code into different chunks, put it all into a single file, and then loaded this file as a content script into by specifying it in manifest.json.

We utilize the chrome.stoage.local api, which is wonderfully intuitive and its asynchronous get() and set() calls work really well with React's state management system.

The most challenging part of the build process was getting React to render different components in different html pages. We needed the StickyNotes rendered as a content script, but the PopupComponent rendered into popup.html. To accomplish this, we created a sort of switch in index.js, which condtionally renders React based on what it is getting from the DOM.

As you can see, we have a fully functioning notes app, that persists data across browsing sessions thanks to the chrome storage api. Happy hacking!
