#### What are Custom Hooks?

- **Custom Hooks** allow you to extract and reuse logic across multiple components.
- They are regular JavaScript functions, but their names must start with `use` to follow the Hooks convention.
- Custom Hooks make it easy to share stateful logic (like fetching data, form handling, or subscribing to a service) between different components without duplicating code.

#### When to use Custom Hooks?

- When you have common logic that is shared across multiple components (e.g., handling window size, data fetching, form input, etc.).
- To avoid code duplication and improve readability.

#### Example of a Custom Hook

Let’s create a custom hook called `useFetchData` that fetches data form server and returns two states `isFetching` and `data`.

example:
```
useFetchData.js

import { useState, useEffect } from 'react';

// Custom Hook: useFetchData
function useFetchData(params) {
  const [data, setData] = useState(params.initialValue);
  const [isFetching, setIsFetching] = useState(true);

  useEffect(() => {
    ... logic to fetch data...
    ... params.doSomething()...
    setData(response)
    setIsFetching(false)
  }, []);

  return {
	  data,
	  isFetching,
	  setData,
	  setIsFetching 
  } ;
}

// Example Component using the custom hook
function App() {
  const initialData = ;
  const doSomething = ()=>{
  }
  
  const {data,isFetching} = useFetchData({initialValue = "",doSomething}); 
  
  return <div> {data} </div>;
}

export default useFetchData;

```
#### How it works:

- **State and Effect**: The `useFetchData` custom hook uses `useState` to store the current data and `useEffect` to get the data from server.
- **Reusable**: This logic can now be reused in any component by simply calling `useFetchData()`.
- By using it in other component it will make a copy of the state and effects such that all the `userFetchData()` calls are independent hooks.

#### Benefits of Custom Hooks:

- **Reusability**: Extract common logic and use it in multiple places.
- **Abstraction**: Separate logic from the component's UI rendering.
- **Readability**: Cleaner, more maintainable code.

With custom hooks, you can create flexible, reusable logic that fits perfectly into Reacts function component model!