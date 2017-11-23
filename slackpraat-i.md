# Slackpraat I

## Subjects
[Data flow in React](#data-flow-in-react)<br>
[Destructuring](#destructuring)<br>
[Functional component](#functional-component)<br>
[Passing data](#passing-data)<br>
[ReactCSSTransitionGroup](#reactcsstransitiongroup)<br>
[Sonarqube error](#sonarqube-error)<br>
[ngrok](#ngrok)<br>
[Showing and hiding components or html](#showing-and-hiding-components-or-html)

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