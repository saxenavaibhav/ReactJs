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

2. Example above behind the scenes does teh following:

- Define a React element.

```
const reactElement = {
    type = 'a',
    props: {
        id: 'some-link',
        'data-number'-20,
        href="http://google.com"
    },
    children: 'Read more on Google',
}

```

- Create a root Element

```
const containerDOMElement = document.querySelector("#root")
```

- Create the element

```
const domELement = document.createElement(reactElement.type)
```

- Update properties

```
domELement.innerText=reactElement.children and so on

```

- Put it in the container

```containerDOMElement.appendChild(domElement)

```

3. JSX

```
import React from 'react';
import { createRoot } from 'react-dom/client';

//// Old way:
// const element = React.createElement(
//   'p',
//   {
//     id: 'hello',
//   },
//   'Hello World!'
// );

// New way:
const element = ( //This parentheses is optional
  <p id="hello">
    Hello World!
  </p>
);

const container = document.querySelector('#root');
const root = createRoot(container);
root.render(element);
```

Why do we use JSX? As our chunk of markup grows, it becomes increasingly clear that JSX is just easier to read.

If we try to run this JSX code in the browser, we'll get an error. JavaScript engines don't understand JSX, they only understand JavaScript. And so we need to "compile" our code into plain JS.

This is most commonly done as part of a build step, using a tool like Babel;

The JSX we write gets converted into React.createElement. By the time our code is running in the user's browser, all of the JSX has been zapped out, and we're left with a JS file full of nested React.createElement calls.

The process of converting JSX into browser-friendly JS is sometimes referred to as “transpiling” instead of “compiling”.

- Can we omit importing React?

When the JSX is compiled into plain JS, that <p> tag becomes a React.createElement call! It's obfuscated? by the JSX.

In earlier versions of React, you'd get an error if you forgot to include the React import:

```
Error: React is not defined
```

because of React.createElement

With React 17, the React team introduced a new “JSX transformer”, used by Babel and other compilers. Essentially, it automatically injects the import during the build process.

4. Expression Slots {}
   We can put into JS expression in {} and it will be forwarded when React transpiles into JS.

so {if (myList.length > 3 "great}

wont work as its not an edxpression.

```
const element = React.createElement(
'div',
{ id: 'hello' },
'Hello World!',
if (myList.length > 3 "great //Won't make sense
);
```

but

```
const element = React.createElement(
'div',
{ id: 'hello' },
'Hello World!',
myList.length //Makes sense
);
```

When working with React, we're allowed to put expressions in our JSX, but we're not allowed to put statements

an expression is a bit of JavaScript code that produces a value.

For example, these are all expressions:

1 → produces 1
"hello" → produces "hello"
5 \* 10 → produces 50
num > 100 → produces either true or false
isHappy ? "🙂" : "🙁" → produces an emoji
[1, 2, 3].pop() → produces the number 3

A JavaScript program is made up of statements. Each statement is an instruction to the computer to do a particular thing.

Here are some examples of statements in JavaScript:

let hi = 5;
A JavaScript program is made up of statements. Each statement is an instruction to the computer to do a particular thing.

Here are some examples of statements in JavaScript:

let hi = 5;

Want to know whether a chunk of JS is an expression or a statement? Try to log it out!

console.log(/_ Some chunk of JS here _/);
If it runs, the code is an expression. If you get an error, it's a statement (or, possibly, invalid JS).

5. JSX comments

```
{/* Some comment! */}
```

We can use the same trick for dynamic attribute values!

Here's an example:

const uniqueId = 'content-wrapper';

const element = (

  <main id={uniqueId}>
    Hello World
  </main>
);

As we saw above, the squiggly brackets ({}) allow us to create an expression slot. This time, we're creating a slot for the value of the id attribute.

Here's how it compiles:

```
const element = React.createElement(
  'main',
  {
    id: uniqueId,
  },
  'Hello World'
);
```

For comparison, here's what it looks like without an expression slot, when the value for id is fixed:

```
const element = (

  <main id="content-wrapper">
    Hello World
  </main>
);

const compiledElement = React.createElement(
'main',
{
id: "content-wrapper",
},
'Hello World'
);
```

At run-time, React will automatically convert types as needed when supplying attributes in expression slots.

For example, these two elements are identical:

// This works:

```
<input required="true" />

// And so does this!
<input required={true} />
```

6. jsx vs HTML

- Reserved Keywords
  Reserved words are keywords with built-in functionality. Because they do something already, we can't use them in our JSX.

For example:
`const while = 10;`

If we run this code, we'll get a syntax error, because while is a reserved word. It's used for “while loops”

Because JSX gets transformed into JS, we can't use any reserved words in our JSX. And this is a problem, because HTML attributes sometimes overlap with JavaScript reserved words.

Consider this JSX:

```
const element = (
  <div>
    <label for="name">
      Name:
    </label>
    <input
      id="name"
      class="fun-input"
    />
  </div>
);
```

If we compile this into JavaScript, we'll discover that we're using two reserved words:

```
for
class
```

To work around this conflict, React uses slight variations on these two terms:

for --> htmlFor
class --> className

- Self CLosing Tag
  All open tags must be closed

- Case sensetive tags
  All tage must be lowercase.

- Case sensetive attributes
  In JSX, our attributes need to be “camelCase”?.

```
onclick → onClick
tabindex → tabIndex
```

There are two exceptions to the "camelCasing" of attributes: data attributes and ARIA attributes.

```
data-test-id="close-dialog-button"
aria-label="Close dialog"
```

- Inline Styles
  In JSX, style takes an object:

```
const element = (
  <h1 style={{ fontSize: '2rem' }}>
    Hello World!
  </h1>
);
```

we need two sets of squiggly brackets.

We need two sets of squiggly brackets:

- The outer set creates an "expression slot" in the JSX, to hold a JavaScript expression
- The inner set creates a JavaScript object, which holds our CSS declarations

React will automatically apply the px suffix for certain CSS properties

```
<div
  style={{
    width: 200, // Equivalent to `width: 200px`
    paddingTop: 8, // Equivalent to `padding-top: 8px`
  }}
>
```

7. Adding whitespaces in JSX

Add a space using {' '}

#Components

Separation of Concerns

![Alt text](https://courses.joshwcomeau.com/react-mats/separation-of-concerns.jpg "a title")

Instead of separating our application into markup (written in HTML), styles (written in CSS), and logic (written in JS), we organize our application into components.

With React, components are the main mechanism of reuse. Instead of partials for HTML, classes for CSS, and functions for JavaScript, we create a component that bundles up all 3, and allows us to create a library of high-level reusable UI elements.

1. Creating Components
   In React, components are defined as JavaScript functions. They can also be defined using the class keyword, though this is considered a legacy alternative that isn't recommended in modern React applications.
   Typically, React components return one or more React elements.

```
function FriendlyGreeting() {
  return (
    <p
      style={{
        fontSize: '1.25rem',
        textAlign: 'center',
        color: 'sienna',
      }}
    >
      Greetings, weary traveller!
    </p>
  );
}
```

```
React components need to start with a Capital Letter. This is how the JSX compiler can tell whether we're trying to render a built-in HTML tag, or a custom React component.
```
