# Slackpraat I

## Subjects
[Data flow in React](#data-flow-in-react)<br>
[Destructuring](#destructuring)<br>
[Functional component](#functional-component)<br>
[Passing data](#passing-data)<br>
[ReactCSSTransitionGroup](#reactcsstransitiongroup)<br>
[Sonarqube error](#sonarqube-error)<br>
[ngrok](#ngrok)<br>
[Showing and hiding components or html](#showing-and-hiding-components-or-html)<br>
[Less complex render functions](#less-complex-render-functions)<br>
[What to use as key](#what-to-use-as-key)<br>
[To chain or not to chain](#to-chain-or-not-to-chain)

## Data flow in React

> Remember: React is all about one-way data flow down the component hierarchy.

> React’s one-way data flow (also called one-way binding) keeps everything modular and fast.

#### Links:

[Thinking in React (React docs)](https://reactjs.org/docs/thinking-in-react.html)

## Destructuring

Destructuring properties:
```javascript
const { isFIAWidget, change, controls } = this.props;

render (
  <Component
    isWidget={isFIAWidget}
    change={change}
    type={controls.insuranceAgentType}
  />
);
```

Destructuring controls inside properties:
```javascript
const { isFIAWidget, change, controls: { insuranceAgentType } } = this.props;

render (
  <Component
    isWidget={isFIAWidget}
    change={change}
    type={insuranceAgentType}
  />
);
```

## Functional component

- Basic component
- No state
- "Dumb" component

```javascript
import './Name.scss';
import React, { PropTypes } from 'react';

const Name = () => {

  return (
    <div className="class-name">component</div>
  );
};

Name.propTypes = {};

export default Name;
```

## Passing data

#### Ways of passing data from JSON to components:

`Header.json`
```json
{
  "header": {
    "menuItems": [],
    "searchBar": {
      "placeholder": "Zoeken...",
      "icon": "search-2"
    }
  }
}
```

`Header.jsx`
```javascript
<SearchBar {...this.props.searchBar} />

// or

<SearchBar
  placeholder={this.props.searchBar.placeholder}
  icon={this.props.searchBar.icon}
/>

// or with destructuring

const Header = ({ menuItems, searchBar }) => {

  // so you can use
  <SearchBar {...searchBar} />

  // or
  <SearchBar placeholder={searchBar.placeholder} />

}
```

`Header.jsx` as class based component
```javascript

class Header extends Component {

  // stuff

  render() {
    const { searchBar } = this.props;

    return (
      <SearchBar {...searchBar} />
    );
  }
}
```

## CSSTransitionGroup

Renders a `<span>` around the child. This troubles grids and layouts. But you can add a className to the `CSSTransitionGroup`:

```javascript
<CSSTransitionGroup className="o-layout">
```

Sometimes the span interferes with your layout. In these cases you can even change the wrapper element:
```javascript
<CSSTransitionGroup component="ul" className="o-layout">
```

Please note that this only applies to CCSTranstitionGroup v1. React-transition-group was largely refactored in version 2.
More information about react-transition-group v1 can be found here: https://github.com/reactjs/react-transition-group/tree/v1-stable

## Sonarqube error

`rename this function to match the regular expression ^[a-z][a-za-z0-9]*$ (s100)`

We turned this off in our project, since components should start with a capital letter:

- `<component />` compiles to `React.createElement('component')` (html tag)
- `<Component />` compiles to `React.createElement(Component)`
- `<obj.component />` compiles to `React.createElement(obj.component)`

Thoughts?

## ngrok

Great little tool for bypassing firewalls. Very helpful for testing on mobile devices and/or showing your work to colleagues that don't have access to your test environment.

Start by installing the npm package (globally)
```
npm install ngrok -g
```

You can run ngrok directly from your command line:

```
ngrok http -host-header=<localhost> <local port>

// Forwarding your local sitecore instance
ngrok http -host-header=www.localeneco.nl 80

// Forwarding storybook
ngrok http -host-header=localhost 9003
```

This will show a terminal interface providing you with a url that you can share or open on your mobile devices. In the example below the url is `http://352cdb00.ngrok.io`
```
ngrok by @inconshreveable                                                                                                                                                                                                                           (Ctrl+C to quit)

Session Status                online
Version                       2.2.8
Region                        United States (us)
Web Interface                 http://127.0.0.1:4040
Forwarding                    http://352cdb00.ngrok.io -> localhost:9003
Forwarding                    https://352cdb00.ngrok.io -> localhost:9003

Connections                   ttl     opn     rt1     rt5     p50     p90
                              0       3       0.00    0.00    0.00    0.00

HTTP Requests
-------------

GET /favicon.ico                                200 OK
GET /static/fonts/etelka-light.woff2            200 OK
GET /static/fonts/etelkamediumpro-webfont.woff2 200 OK
GET /__webpack_hmr
GET /static/preview.bundle.js                   200 OK
GET /iframe.html                                200 OK
GET /static/manager.bundle.js                   200 OK
GET /                                           200 OK
```

For more information on ngrok: https://ngrok.com/
Happy testing!


## Showing and hiding components or html

`if`

```javascript
{boolean && (
  <Component />
)}
```

`if else`

```javascript
{boolean ? (
  <ComponentTrue />
) : (
  <ComponentFalse />
)}
```

It says boolean, but does not have to be a boolean. Can be a string / object / whatever. It will be `true` when it has a value and `false` when the value is `null`.

## Less complex render functions

Inside the components render function:

```javascript
{boolean && this.renderSpecificComponent()}
```

Outside the components render function is another function, called `renderSpecificComponent`:

```javascript
renderSpecificComponent() {
  const { property } = this.props;

  return (
    <div>Specific stuff</div>
  );
}
```

Splits the logic, makes it easier to read and you won't get Sonarqube complex function issues: `Function has a complexity of 11 which is greater than 10 authorized.`

Also, from stackoverflow:
> A function in the render method will be created each render which is a slight performance hit. It's also messy if you put them in the render, which is a much bigger reason, you shouldn't have to scroll through code in render to see the html output. Always put them on the class instead.

> For stateless components, it's probably best to keep functions outside of the main function and pass in props instead, otherwise the function will be created each render too. I haven't tested performance so I don't know if this is a micro-optimization but it's worth noting.

#### Links

[React functions inside render](https://stackoverflow.com/questions/41369296/react-functions-inside-render)

## What to use as key

How we used to do it (this is an anti pattern):

```javascript
array.map((item, index) => {
  return (
    <Component key={index} stuff={item} />
  );
});
```

Why is this wrong? Because the order of your rendered components could change.

### Solution?

To discuss

#### Links

[Index as a key is an anti-pattern](https://medium.com/@robinpokorny/index-as-a-key-is-an-anti-pattern-e0349aece318)

## To chain or not to chain

How would you write this?

```javascript

values = {
  "question-1": "10",
  "question-2": "20"
}

// no chain

const entries = Object.entries(values);

const scores = entries.map(entry => {
  return parseInt(entry[1], 10);
});

const total = scores.reduce((acc, cur) => {
  return acc + cur;
});

// yes chain

const total = Object.entries(values)
  .map(entry => {
    return parseInt(entry[1], 10);
  })
  .reduce((acc, cur) => {
    return acc + cur;
  });
```
