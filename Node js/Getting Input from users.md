To access input from command prompt, we use process in node.

`console.log(process.argv)` // array of 3 elements, 3rd element is the input provided by the user

ex:

`console.log(process.argv[2])`

running the command `node app.js dhanush`

we get output as `dhanush`

##### Argument parsing with Yargs Library

 Installation : `npm i yargs@latest`

app.js
```
const  yargs = require('yargs')
console.log(yargs.argv)
```

output:
```
node app.js Dhanush
{ _: [ 'Dhanush' ], '$0': 'app.js' }
```

output: 
```
node app.js --name=Dhanush
{ _: [], name: 'Dhanush', '$0': 'app.js' }
```

