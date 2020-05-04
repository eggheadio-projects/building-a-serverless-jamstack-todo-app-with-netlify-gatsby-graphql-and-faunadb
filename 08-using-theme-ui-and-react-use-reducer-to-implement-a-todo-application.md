# Using theme-ui and React useReducer to implement a todo application

**[📹 Video](https://egghead.io/lessons/react-using-theme-ui-and-react-usereducer-to-implement-a-todo-application?pl=building-a-serverless-jamstack-todo-app-with-netlify-gatsby-graphql-and-faunadb-53bb)**

**[💻 Code](https://github.com/christopherbiscardi/serverless-todo-netlify-fauna-egghead/tree/66442bb589c918eb0c776dbcc9675ea00da65a63)**

**Objective:** Let's add the functionality of adding todos (via an `input` element of sorts).

Move the dashboard code from `app.js` to its own `dashboard.js` file (`packages/www/src/components/dashboard.js`).

Add the `input` and `label` elements from `theme-ui`.

```js
import {
  Container,
  Flex,
  Heading,
  Button,
  Input,
  Label,
  NavLink,
  Checkbox,
} from "theme-ui";
```

Store the `input` element reference with `useRef`.

* `const inputRef = useRef();`

Add a `label`, `input` and a `button` all wrapped in a `Flex` container that acts as a `form`.

```js
<Flex
  as="form"
  onSubmit={onClick}
>
  <Label sx={{ display: "flex" }}>
    <span>Add&nbsp;Todo</span>
    <Input ref={inputRef} sx={{ marginLeft: 1 }} />
  </Label>
  <Button sx={{ marginLeft: 1 }}>Submit</Button>
</Flex>
```

Add the extra styles using the `sx` prop.

The todos will be stored as arrays of objects with two properties:

```js
{
  done: Boolean,
  value: String
}
```

The existing todos will be displayed as checkboxes:

```js
<Flex sx={{ flexDirection: "column" }}>
  <ul sx={{ listStyleType: "none" }}>
    {todos.map((todo, i) => (
      <Flex
        as="li"
        onClick={() => {
          dispatch({
            type: "toggleTodoDone",
            payload: i
          });
        }}
      >
        <Checkbox checked={todo.done} />
        <span>{todo.value}</span>
      </Flex>
    ))}
  </ul>
</Flex>
```

We'll set the initial state to an empty array.

* `const [todos, dispatch] = useReducer(todosReducer, []);`

To access and change our state, we'll use `useReducer`. Instead of `setState` function, we'll need a `dispatch` function. We'll have to write a `todosReducer` to handle the dispatch logic.

Our reducer will handle two actions, adding todos and marking todos as done:

```js
const todosReducer = (state, action) => {
  switch (action.type) {
    case "addTodo":
      return [{ done: false, value: action.payload }, ...state];
    case "toggleTodoDone":
    // never mutate the original state
      const newState = [...state];
      newState[action.payload] = {
        done: !state[action.payload].done,
        value: state[action.payload].value
      };
      return newState;
  }
};
```

Let's add an `onSubmit` function for when a new todo is submitted:

```js
onSubmit={e => {
  e.preventDefault();
  dispatch({ type: "addTodo", payload: inputRef.current.value });
  // resets the value to an empty string once submitted
  inputRef.current.value = "";
}}
```

And let's add an `onClick` function for each checkbox:

```js
<Flex
  as="li"
  onClick={() => {
    dispatch({
      type: "toggleTodoDone",
      payload: i
    });
  }}
>
```
👍 Note that the `onClick` event is set on the entire `li` element and not just the `checkbox`, which means clicking anywhere on that row will toggle the to-do (we can also use the spacebar for toggling).

Check out the full [`dashboard.js`](https://github.com/ChristopherBiscardi/serverless-todo-netlify-fauna-egghead/blob/66442bb589c918eb0c776dbcc9675ea00da65a63/packages/www/src/components/dashboard.js) file.

And the [`app.js`](https://github.com/ChristopherBiscardi/serverless-todo-netlify-fauna-egghead/blob/66442bb589c918eb0c776dbcc9675ea00da65a63/packages/www/src/pages/app.js).