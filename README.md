react-burger-menu [![Build Status](https://travis-ci.org/negomi/react-burger-menu.svg?branch=master)](https://travis-ci.org/negomi/react-burger-menu)
=================

An off-canvas sidebar React component with a collection of effects and styles using CSS transitions and SVG path animations.

## Demo & Examples

Live demo: [negomi.github.io/react-burger-menu](http://negomi.github.io/react-burger-menu/)

To build the examples locally, run:

```
npm install
npm start
```

Then open [`localhost:8000`](http://localhost:8000) in a browser.

## Tests

The test suite uses [Mocha](http://mochajs.org/), [Chai](http://chaijs.com/) and [Sinon](http://sinonjs.org/), with [jsdom](https://github.com/tmpvar/jsdom).

*You will need at least Node v4.0.0 (or [io.js](https://iojs.org/en/index.html)) to run the tests, due to jsdom depending on it.*

To run the tests once, run:

```
npm test
```

To run them with a watcher, TDD style, run:

```
npm run tdd
```

## Installation

The easiest way to use react-burger-menu is to install it from npm and include it in your own React build process (using [Browserify](http://browserify.org), [Webpack](http://webpack.github.io/), etc).

*WARNING: Elastic and Bubble are not currently supported with Webpack builds due to their Snap.svg dependency.*

You can also use the standalone build by including `dist/react-burger-menu.js` in your page. If you use this, make sure you have already included React, and it is available as a global variable.

If you're using React 0.14:

```
npm install react-burger-menu --save
```

If you're using React 0.13.3:

```
npm install react-burger-menu@1.1.6 --save
```

## Usage

Items for the sidebar should be passed as child elements of the component using JSX.

*If you're using this component with react-router, check out [this info](https://github.com/negomi/react-burger-menu/wiki/FAQ#why-doesnt-the-link-component-from-react-router-work) in the wiki.*

``` javascript
var Menu = require('react-burger-menu').nameOfAnimation;

var Example = React.createClass({
  showSettings: function(event) {
    event.preventDefault();
    .
    .
    .
  },
  render: function() {
    return (
      <Menu>
        <a id="home" className="menu-item" href="/">Home</a>
        <a id="about" className="menu-item" href="/about">About</a>
        <a id="contact" className="menu-item" href="/contact">Contact</a>
        <a onClick={ this.showSettings } className="menu-item--small" href="">Settings</a>
      </Menu>
    );
  }
});

```

### Animations

* `slide`
* `stack`
* `elastic`
* `bubble`
* `push`
* `pushRotate`
* `scaleDown`
* `scaleRotate`
* `fallDown`

### Properties

Some animations require certain other elements to be on your page:

* **Page wrapper** - an element wrapping the rest of the content on your page, placed after the menu component

  ``` javascript
  <Menu pageWrapId={ "page-wrap" } />
  <main id="page-wrap">
    .
    .
    .
  </main>
  ```

* **Outer container** - an element containing everything, including the menu component
  ``` javascript
  <div id="outer-container">
    <Menu pageWrapId={ "page-wrap" } outerContainerId={ "outer-container" } />
    <main id="page-wrap">
      .
      .
      .
    </main>
  </div>
  ```

If you are using an animation that requires either/both of these elements, you need to give the element an ID, and pass that ID to the menu component as the `pageWrapId` and `outerContainerId` props respectively.

Check this table to see which animations require these elements:

Animation | `pageWrapId` | `outerContainerId`
--- | :---: | :---:
`slide` | |
`stack` | |
`elastic` | &#x2713; | &#x2713;
`bubble` | |
`push` | &#x2713; | &#x2713;
`pushRotate` |  &#x2713;  |  &#x2713;
`scaleDown` |  &#x2713;  |  &#x2713;
`scaleRotate` |  &#x2713;  |  &#x2713;
`fallDown` |  &#x2713;  |  &#x2713;

#### Position

The menu opens from the left by default. To have it open from the right, use the `right` prop. It's just a boolean so you don't need to specify a value. Then set the position of the button using CSS.

``` javascript
<Menu right />
```

#### Width

You can specify the width of the menu (in pixels) with the `width` prop. The default is `300`.

``` javascript
<Menu width={ 280 } />
```

#### Open state

You can control whether the sidebar is open or closed with the `isOpen` prop. This is useful if you need to close the menu after a user clicks on an item in it, for example, or if you want to open the menu from some other button in addition to the standard burger icon. The default value is `false`.

``` javascript
<Menu isOpen />
```

#### State change

You can detect whether the sidebar is open or closed by passing a callback function to `onStateChange`. The callback will receive an object containing the new state as its first argument.

``` javascript
var isMenuOpen = function(state) {
  return state.isOpen;
};

<Menu onStateChange={ isMenuOpen } />
```

#### Overlay

You can turn off the default overlay with `noOverlay`.

``` javascript
<Menu noOverlay />
```

#### Custom icons

You can replace the default bars that make up the burger and cross icons with custom `ReactElement`s. Pass them as the `customBurgerIcon` and `customCrossIcon` props respectively.

```javascript
<Menu customBurgerIcon={ <img src="img/icon.svg" /> } />
<Menu customCrossIcon={ <img src="img/cross.svg" /> } />
```

You should adjust their size using the `.bm-burger-button` and `.bm-cross-button` classes, but the element itself will have the class `.bm-icon` or `.bm-cross` if you need to access it directly.

#### Custom ID

There is also an optional `id` prop, which will simply add an ID to the rendered menu's outermost element. This is not required for any functionality.

``` javascript
<Menu id={ "sidebar" } />
```

### Styling

All the animations are handled internally by the component. However, the visual styles (colors, fonts etc.) are not, and need to be supplied, either with CSS or with a JavaScript object passed as the `styles` prop.

#### CSS

The component has the following helper classes:

``` css
/* Position and sizing of burger button */
.bm-burger-button {
  position: fixed;
  width: 36px;
  height: 30px;
  left: 36px;
  top: 36px;
}

/* Color/shape of burger icon bars */
.bm-burger-bars {
  background: #373a47;
}

/* Position and sizing of clickable cross button */
.bm-cross-button {
  height: 24px;
  width: 24px;
}

/* Color/shape of close button cross */
.bm-cross {
  background: #bdc3c7;
}

/* General sidebar styles */
.bm-menu {
  background: #373a47;
  padding: 2.5em 1.5em 0;
  font-size: 1.15em;
}

/* Morph shape necessary with bubble or elastic */
.bm-morph-shape {
  fill: #373a47;
}

/* Wrapper for item list */
.bm-item-list {
  color: #b8b7ad;
  padding: 0.8em;
}

/* Styling of overlay */
.bm-overlay {
  background: rgba(0, 0, 0, 0.3);
}
```

#### JavaScript

The same styles can be written as a JavaScript object like this:

```javascript
var styles = {
  bmBurgerButton: {
    position: 'fixed',
    width: '36px',
    height: '30px',
    left: '36px',
    top: '36px'
  },
  bmBurgerBars: {
    background: '#373a47'
  },
  bmCrossButton: {
    height: '24px',
    width: '24px'
  },
  bmCross: {
    background: '#bdc3c7'
  },
  bmMenu: {
    background: '#373a47',
    padding: '2.5em 1.5em 0',
    fontSize: '1.15em'
  },
  bmMorphShape: {
    fill: '#373a47'
  },
  bmItemList: {
    color: '#b8b7ad',
    padding: '0.8em'
  },
  bmOverlay: {
    background: 'rgba(0, 0, 0, 0.3)'
  }
}

<Menu styles={ styles } />
```

### Browser Support

Because this project uses CSS3 features, it's only meant for modern browsers. Some browsers currently fail to apply some of the animations correctly.

Chrome and Firefox have full support, but Safari and IE have strange behavior for some of the menus.

### License

MIT
