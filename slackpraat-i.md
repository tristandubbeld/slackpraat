# Slackpraat I

## Subjects
[Data flow in React](#data-flow-in-react)<br>
[Destructuring](#destructuring)<br>
[Functional component](#functional-component)<br>
[Passing data](#passing-data)<br>
[ReactCSSTransitionGroup](#reactcsstransitiongroup)<br>
[Sonarqube error](#sonarqube-error)<br>
[ngrok](#ngrok) TODO<br>
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

## ReactCSSTransitionGroup

Renders a `<span>` around the child. This troubles grids and layouts. But you can add a className to the `ReactCSSTransitionGroup`:

```javascript
<ReactCSSTransitionGroup className="o-layout">
```

## Sonarqube error

`rename this function to match the regular expression ^[a-z][a-za-z0-9]*$ (s100)`

We turned this off in our project, since components should start with a capital letter:

- `<component />` compiles to `React.createElement('component')` (html tag)
- `<Component />` compiles to `React.createElement(Component)`
- `<obj.component />` compiles to `React.createElement(obj.component)`

Thoughts?

## ngrok

TODO

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
