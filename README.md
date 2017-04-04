# React Element Portal

[![travis](https://travis-ci.org/zapier/react-element-portal.svg?branch=master)](https://travis-ci.org/zapier/react-element-portal)

Render a React component inline, but target a DOM element (or elements) by id or selector.

## Why?

If you're making a shiny new React app where you use React everywhere, for every page, and for the entirety of every page, then you probably don't need this. But if you live in an imperfect world, where you have a server-generated header/footer or some static blog pages, or anything else not fully controlled by React, you can use an `ElementPortal` to control those things from inside a single root React element.

## Install

```bash
npm install react-element-portal --save
```

## Usage with vanilla React

Let's say we get this from the server:

```html
<html>
  <body>
    <!-- Header generated by server -->
    <div id="header">
      <a href="/">Home</a>
      <h1>My Sorta Cool App</h1>
      <div id="user">Joe</div>
    </div>
    <!-- Container for React to do its thing -->
    <div id="app"></div>
  </body>
</html>
```

Even though we don't control the header, we can pretend like parts of it are owned by a single React root element.

```js
import ElementPortal from 'react-element-portal';

ReactDOM.render(
  // Just rendering a single React element.
  <div>
    {/* Use some React to spice up our header. */}
    <ElementPortal id="user">
      <div>
        <Menu>
          <Label>Joe</Label>
          <Items>
            <Item>Upgrade</Item>
            <Item>Settings</Item>
            <Item>Support</Item>
          </Items>
        </Menu>
      </div>
    </ElementPortal>
    {/* And render our main app as a sibling. */}
    <div>
      <h1>My App</h1>
      <p>This is my main app and gets rendered to #app.</p>
    </div>
  </div>,
  document.getElementById('app')
);
```

You can also use a selector instead of an id.

```js
<ElementPortal selector=".header .user">
  <div>
    ...
  </div>
</ElementPortal>
```

## Passing context to your ElementPortal
Context from your main tree is passed down automatically to your `ElementPortal`. For example, if you use Redux, the `store` context will not get lost, and using `connect` will behave as expected in the children of your `ElementPortal`.
