1. React application using only JS

```
import React from 'react';
import { createRoot } from 'react-dom/client';

// 2. Create a React element
const element = React.createElement(
'p',
{ id: 'hello' },
'Hello World!'
);

// 3. Render the application
const container = document.querySelector('#root');
const root = createRoot(container);
root.render(element);

```

#### Imports

- We're importing the core React library from the `react` dependency.
- createRoot function from `react-dom`.

There are two separate packages because React itself is “platform agnostic”. We have the core react package, and then there are different platform-specific renderers:

- react-dom for the web
- react-native for mobile (iOS / Android) or desktop (Windows / macOS) applications
- react-three-fiber for 3D scenes using WebGL and Three.js

Every platform has its own “primitives”, the set of built-in elements we use to create our UI. On the web, the primitives are HTML elements like `div` and `p` and `button`. By contrast, React Native doesn't have divs, it has Text and View and Pressable.

All of these platforms will use the same core React framework, which comes from the react package. But when it comes to actually turning all of the business logic into a user interface, we need the correct bindings for our platform.

**What is DOM?**

The DOM is the living, breathing structure that makes up a live website / web application. When we visit a website, the browser turns the static HTML into the DOM.

To use an analogy: HTML is the blueprint for a specific car, and the DOM is the car itself, zipping around the city.

Here's another way to look at it:

When you right-click and “View source”, you view the HTML, a static document that describes what will be constructed.
When you right-click and “Inspect element”, to open the Elements pane, you're interacting with the DOM. You can change attributes and watch the UI update in response.

Last few lines:

```
const container = document.querySelector('#root');
const root = createRoot(container);
root.render(element);
```

- document.querySelector is the modern version of document.getElementById.

You can think of the render function as a machine that converts React elements into DOM nodes. In this case, our React element describes a paragraph tag, with an ID, and some text inside. render will turn that description into the following DOM structure:

```
<p id="hello">
  Hello World!
</p>
```

With that DOM element created, it then adds it to the page at the specified root. In essence, this code takes a JavaScript-based description of some HTML, and uses it to produce real-world DOM nodes.

2. Build Systems

React project above with index.js and index.html. index.html does not import index.js (Index.js uses JSX and browser does not understand jsx) as React uses a build system.
index.js along with all dependencies (React, ReactDOM) are bundled into files such as 2.123.chunk.js etc and script tage are dynamically injected into the index HTML. Tihs file runs into the browser.
