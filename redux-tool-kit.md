## Redux Toolkit notes

- First You need to create store where everything will be store like global storage.
- use configureStore to create Store.

```javascript
import { configureStore } from "@reduxjs/toolkit";

export const store = configureStore({});
```

- Next you need to create your reducers
- In redux toolkit they known as slices.
- You can use createSlice to create your slices.
- Then you have define your intialState using variable e.g. **intialState**. It can be array, objects, array of objects or anything.
- To create slice you need to define three things in your slice
 1. slice name 
 2. intial state
 3. reducers
- reducers will contain properties and functions.
- your reducers have access of two properties. state and action
- state will give you current state's values which has been stored in store.
- action will provider you values which you are sending when you are calling methods.


```javascript
export const todoSlice = createSlice({
  name: 'todo',
  intialState: intialState,
  reducers:{
    addTodo: (state, action) => {
      const todo = { id: nanoid(), title: action.payload.title }
      state.todos.push(todo)
    },
    removeTodo: (state,action) => {
      state.todos = state.todos.filter((todo)=>{ todo.id != action.payload.id })
    }
  }
})
```
- Then you have to export your functionalities which you have define in reducers.
- Also You need to export all reducers.

```javascript
export const { addTodo, removeTodo } = todoSlice.actions

export default todoSlice.reducer
```

- Then you to add your reducers to your store file.

```javascript
import { configureStore } from "@reduxjs/toolkit";
import todoReducer from '../features/todo/todoSlice';


export const store = configureStore({
  reducer: todoReducer
});+

```

- Then you need wrap your top level component with reduce provider and also pass prop value of store as store

```javascript
import { Provider } from 'react-redux'
import { store } from './App/store'

ReactDOM.createRoot(document.getElementById('root')).render(
  <Provider store={store}>
    <App />
  </Provider>,
)
```

### How to use state and set state?

- There are two hooks which we can use for set state and get state.
 1. **dispatch** - When need to update our state we use dispatch. dispatch use reducer to set state in store. useDispatch is used.
    ```javascript
      import { useDispatch } from 'react-redux'
      import { addTodo } from '../features/todo/todoSlice'

      const [input, setInput] = useState('')
      const dispatch = useDispatch()

      const addTodoHandler = (e) => {
        e.preventDefault()
        dispatch(addTodo(input))
        setInput('')
      }
    ```
  2. **selector** - When we need state in our component then we need to use useSelector. We have access **state** variable from where we can get our states.
      ```javascript
        const todos = useSelector(state => state.todos)
      ```