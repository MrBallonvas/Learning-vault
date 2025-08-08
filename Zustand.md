### Create store
**`Function 'create' using for react and return hook`**
```javascript
import create from 'zustand'
const useStore = create((set) => ({
	count: 0,
	increment: () => set((state) => ({ count: state.count + 1 })),
	decrement: () => set((state) => ({ count: state.count - 1 })),
	reset: () => set({ count: 0 })
}))

export default useStore
```
**`Function 'createStore' using for vanilla apps and returns store`**
### Without rerendering
**`Shallow help you to 'shallow' compare`**
```javascript
const { decrement, increment, reset } = useStore(
	({ decrement, increment, reset }) => ({ decrement, increment, reset }),
	/* 👇 */
	shallow 
)
```
**`shallow(a,b) return true if top levels of a and b are equals`**

### Memoising selectors
**`you need useCallback to memoization`**
```javascript
const todoById = useStore(useCallback(state => state.todos[id], [id]))
```

### Reading state in operation
**`use 'get' to get state from every place of store`**
```javascript
const useStore = create((set, get) => ({
	todos: [],
	removeTodo(id) {
		const newTodos = get().todos.filter(t => t.id !== id)
		set({ todos: newTodos })
	}
}))
```

### Temporary subsription
**``**
```javascript
const useStore = create(set => ({ count: 0 }))
function Counter() {
	const countRef = useRef(useStore.getState().count)
	useEffect(() => {
	   const unsubscribe = useStore.subscribe(
		   state => (countRef.current = state.count)   
       )
	   return () => {     
		   unsubscribe()
	   }
	}
, [])}
```

### Long time storage
**`if you need save your state for many sessions`**
```javascript
import create from 'zustand'
import { persist } from 'zustand/middleware'
const useStore = create(
	persist( (set, get) => ({   
		todos: [],   
		addTodo(newTodo) {     
			const newTodos = [...get().todos, newTodo]     
			set({ todos: newTodos })   
		}
	},
	{
		name: "todos-storage",
		getStorage: () => sessionStorage
	}
)))
```

## Middlewares
### combine
**`lets separate actions and values`**

```javascript
import { create } from 'zustand';
import { combine } from 'zustand/middleware';

const useStore = create(
  combine(
    { count: 0 },
    (set) => ({
      increment: () => set((state) => ({ count: state.count + 1 })),
      decrement: () => set((state) => ({ count: state.count - 1 })),
    })
  )
);
```

### immer
**`lets perform immutable updates`**

### devtools
**`lets add state to redux devtools without redux`**

### persist
**`lets persists a store state across page reloads`**

