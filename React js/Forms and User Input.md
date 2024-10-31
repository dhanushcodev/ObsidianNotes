Main concepts:
- Form submission
- Input Validation

##### Managing and Getting User input via state and Generic Handlers

example:
```
const [enteredValues,setEnteredValues] = useState({
	email: '',
	password: ''
});


function handelInputChange(identifier,event){
	setEnteredValues(prevValues => ({
		...prevValues,
		[identifier]: event.target.value   //new key or update with the identifer
	}))
}


<input
	id='email'
	type='email'
	name='email'
	onChange={(event)=> handelInputChange('email',event)}
	value={enteredValues.email}
/>

```

##### Getting user input via Ref's

example:
```
const email = useRef();
const password = useRef();

function handelSubmit(event){
	event.preventDefault();
	const enteredEmail = email.current.value;
	const enteredPassword = password.current.value;
}

<input
	id='email'
	type='email'
	name='email'
	ref={email}
/>

```

##### Getting values via Form Data and Native Browser APIs

- we can use browsers built in `FormData()` API to get the form data.
- we need to have name attribute for all the input fields to this to work.

example:
```
function handelSubmit(event){
	event.preventDefault();
	const fd = new FormData(event.target);
	const data = Object.fromEntries(fd.entries());
	//for multiple data with same name these are lost in above data
	const multiSelectData = fd.getAll('multiSelect');
}
```

Resetting the Form
```
function handelSubmit(event){
	event.preventDefault();
	const enteredEmail = email.current.value;
	const enteredPassword = password.current.value;

	event.target.reset();
}
```

##### Validating input via Built-in validation prop

```
<input
	type='email'
	name='email'
	required // this will take card of validation of email
/>

<input
	type='password'
	name='password'
	required
	minLength={6}
/>
```
