## Context API notes


- You need to create context using createContext
- then you have to create Context Provider which provide context to your components
- then you can craeate custom hook which consumes your context

## Example..........................................

```javascript
import { createContext, useContext } from "react";

export const TodoContext = createContext({ });

export const TododContextProvider = TodoContext.Provider;

export default function useTodo(){
  return useContext(TodoContext)
}
```

- then you need to wrap your provider to your parent components where you need to share your states
- You can pass functions, states to the provider which you want to share across comonents using value props.

## Example..........................................


```javascript
import { useState, useEffect } from 'react'
import './App.css'
import { TododContextProvider } from './context/todo'
import TodoForm from './components/TodoForm'
import TodoItem from './components/TodoItem'

function App() {
  const [todos, setTodos] = useState([])

  const updateTodo = (id, todo) => {
    setTodos((prev) => prev.map((prevTodo) => (prevTodo.id === id ? todo : prevTodo)))
  }

  const deleteTodo = (id) => {
    setTodos(
      (prev) => prev.filter((todo) => todo.id !== id)
    )
  }

  const toggleComplete = (id) => {
    setTodos(
      (prev) => prev.map((todo) => todo.id === id ? { ...todo, completed: !todo.completed } : todo)
    ) 
  }

  // Load existing todos in local storage
  useEffect(() => {
    const todos = JSON.parse(localStorage.getItem("todos"));

    if(todos && todos.length > 0){
      setTodos(todos)
    }
  }, []);

  // // add todos in local storage
  useEffect(() => {
    localStorage.setItem("todos", JSON.stringify(todos));
  }, [todos]);
  
  return (
    <TododContextProvider value={{ todos, setTodos, updateTodo, deleteTodo, toggleComplete }}>
      <div className="w-full max-w-2xl mx-auto shadow-md rounded-lg px-4 py-3 text-white">
        <div className="bg-[#172842] min-h-screen py-8">
          <h1 className="text-2xl font-bold text-center mb-8 mt-2">Manage Your Todos</h1>
          <div className="mb-4">
            <TodoForm />
          </div>
          <div className="flex flex-wrap gap-y-3">
            {
              todos.map((todo) => {
                return(
                  <div key={todo.id}>
                    <TodoItem todo={todo} />
                  </div>
                )
              })
            }
          </div>
        </div>
      </div>
    </TododContextProvider>
  )
}

export default App
```

### How You can set State and cosume state in components?

```javascript
const { todos, setTodos, updateTodo, deleteTodo, toggleComplete } = useTodo();
```
