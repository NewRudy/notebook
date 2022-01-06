# React

[consept](https://reactjs.org/docs/hello-world.html)

[toc]

## Introducing JSX

A syntax extension to JavaScript.

## Rendering Elements

An element describes what you want to see on the screen.

## Conponents and Props

function components

```reac
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}
```

class components

```react
class Welcome extends React.Component {
  render() {
    return <h1>Hello, {this.props.name}</h1>;
  }
}
```

Always start component names with a capital letter.

All React components must act like pure functions with respect to their props.

## State and LifeCycle

State is similar to props, but it is private and fully controlled by the component.

convert a function component:

1. create an ES6 class with the same name that extends React.Component.
2. Add a simgle empty method to it called render()
3. Move the body into the `render` 
4. Replace props with this.props in the render() body
5. Delete empty function delaration

The render method will be called cach time an update happens, but as long as we rendering.

Class components should always call the base constructor with props.

In applications with many components, it's very important to free up resources taken by the components when they are destroyed.

- Do not modify state directly, instead use setState()
- State updates may be asychronous

```react
// Wrong
this.setState({
    counter: this.state.counter + this.props.increment,
})
// Correct
this.setState((state, props) => {
    counter: state.counter + props.increment,
})
```

The data flows down.

State is not accessible to any component other than the one that owns and sets it, a component may choose to apss its state down as props to its child components.

```react
<FormatteDate data={this.state.data} />
```

If you imagine a component tree as a water fall of props, each component's state is like an additional water source that joins it at an arbitrary point but also flows down.

## Handling Events

- React events are named using camelCase, rather than lowercase
- With JSX you pass a function as the event handler, rather than a string

preventDefault

this: bind, arrow function(In most cases, this is fine, if this callback is passed as a prop to lower components, those components might do an extra re-rendering)

```react
<button onClick={(e) => this.deleteRow(id, e)}>Delete Row</button>
<button onClick={this.deleteRow.bind(this, id)}>Delete Row</button>
```

## Conditional rendering

function (if) \ Element variables  \  logical && operator

whenever conditions become too complex, it might be a good time to extrct a component.

## Lists and keys

A component that accepts an array of numbers and outputs a list of elements.

```react
function NumberList(props) {
  const numbers = props.numbers;
  const listItems = numbers.map((number) =>
    <li key={number.toString()}>
      {number}
    </li>
  );
  return (
    <ul>{listItems}</ul>
  );
}

const numbers = [1, 2, 3, 4, 5];
ReactDOM.render(
  <NumberList numbers={numbers} />,
  document.getElementById('root')
);
```

Keys help React identify which items have changed, are added, or are removed.

If you choose not to assign an explicit key to list items then React will default to using indexes as keys, however the order of items may change.

Keys only make sense in the context of the surrounding array.

## Forms

Form elements naturally keep some internal state.

An input form element whose value is controlled by React in this way is called a "controlled component".

## Lifting state up

oftern, several components need to reflect the same changing data. We recommend lifting the shared state up to their closest common ancestor.

