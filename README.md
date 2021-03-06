# Lodux

A framework for simple state management and rapid development, built on top of redux.

### How to use it

Lodux uses a ```connect``` function which is a wrapper of the redux connect function, abstracting management of reducers and actions for simple stores.

To connect a component to a lodux store, you must define a ```mapDataToProps``` and ```mapActToProps``` function.

```mapDataToProps``` takes in props and defines the store's data and structure.

```mapActToProps``` takes in an ```act``` function and props. The "act" function is a wrapper around redux's ```dispatch``` function which dispatches developer-defined actions. However, passing an action object into the ```act``` function will automatically dispatch the action and modify the store.

```act``` takes in an object with the following properties:

```
{
  path: the object path to modify: this can be anarray of object properties or a string path separated by "."
  cmd: a CRUD command to apply to the object at the specified path - commands follow immutability-helper syntax:       https://github.com/kolodny/immutability-helper
}
```

Below is an example of an act object:

```
{
  path: ['todo', 'items', 3] || 'todo.items.3' // accesses {todo: items[3]}
  cmd: {$set: "hello world"} // sets {todo: items[3]} to "hello world"
}
```

This would set the item at path {todo: items[3]} to "hello world".

### Example

The scaffold of an example implementation is below:

```
class Todo extends Component {

  ...

}

const mapDataToProps = (props) => {
  return {
    items: ["one", "two", "three"]
  };
}

const mapActToProps = (act, props) => {
  return {
    addItem: (item) => {
      act( {path: 'items', cmd: {$push: [item]}} );
    },
    removeItem: (index) => {
      act( {path: 'items', cmd: {$splice: [[index, 1]]} });
    },
    setItem: (index, value) => {
      act( {path: `items.${index}`, cmd: {$set: value}} );
    }
  }
}

export default connect(mapDataToProps, mapActToProps)(Todo);
```
