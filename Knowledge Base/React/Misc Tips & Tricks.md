##### If a component is not re-rendering on change of a state after initialization
For example if a third party text editor is not changing color mode even after switching to dark mode etc.. you can create a dynamic key and pass it as key to the component. React will now reinitialize this component every time the state changes.
```jsx
const Component = ()=>{
	const { mode } = useTheme();
    const editorKey = `editor-${mode}`;

	return <Edtior key={editorKey} />
}
```

