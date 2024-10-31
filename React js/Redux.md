##### What is Redux?
A state management system for cross - component or app-wise state.

Disadvantages with context:
- complex setup and management
- sometimes it may lead to deeply nested providers

example:
```
<AuthContexProvider>
	<ThemeContextProvider>
		<UIInteractionContextProvider>
			....
		</UIInteractionContextProvider>
	</ThemeContextProvider>
</AuthContextProvider>
```

##### How Redux works?

- It has a Central Data (state) Store where the components can subscribe, whenever the data changes components can update.
- component never directly update the data.
- Reducer function is responsible for mutating the data. (not  `useReduce()`)
- component dispatches action to the Reducer function, Reducer will then forward it to the CDS and update

