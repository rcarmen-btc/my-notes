# JavaScript 
```html
<script type="text/javascript" src="script.js">
	alert("Here I am") // Pause and print
	document.write("<p>Hello World!") // Write in html file
</script>
<noscript>
	Ur browser doesn't support js as fawk.
</noscript>

<p><a href="js01.htm" onclick="alert('Here I am'); return false;">Click Me</a></p>
  
```

### Consol Loggin
```js
console.log("String")
```

### Comments
```js
// Comment

/*
	Comments
*/
```

### String constants
It's same, but double quotes for HTML, single for JS
```js
x = 'x' // same
x = "x" // same
```

### Numeric constants
No integers in JS

### Comparison operators
```js
j = true
j === 0 // are they same type?
false
j !=== 0 // are they same type?
true
```

### Functions
```js
function product(a, b) {
	value = a + b;
	return value;
}

console.log('Product = ' + product(3, 4));

> 7
```

### Scope - global (default)
```js
gl = 100
function check() {
	gl = 0
}
check()

console.log(gl)

> 0
```

Otherwise
```js
gl = 100
function check() {
	var gl = 0 // it's local
}
check()

console.log(gl)

> 0
```

### Lists and ass stucutures
allmost like in python


## Control structures
---
### if
```js
if (blah) {
	blah;
}
else if (blah) {
	blah;
}
else {
	blah;
}
```

### while
```js
while (blah) {
	blah;
	break;
	continue;
}
```

### for
```js
for (var c = 1; c <= 5; c++) {
	blah;
	break;
	continue;
}

var array = [...];

for (item in array) {
	blah
}
```

