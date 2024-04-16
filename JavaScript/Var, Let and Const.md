
#### Var 
It has the Global scope or function scoped which means variables defined outside the function can be accessed globally, and variables defined inside a particular function can be accessed within the function.

```
var a =10
function f(){
	var b =20
	console.log(a,b)
}
fun();
console.lo(a);
```

Output:
10 20
10

Even though the variable b is in a function scope, it is accessible both inside and outside. 

## let 
The 'let' an improved version of the 'var'. It is introduced in the ES6. These variables has the block scope. It can’t be accessible outside the particular code block ({block}).

```
function f{
	if(true){
	let y = 20;
	console.log(y);
	}
	console.log(y); //ReferenceError
}
f()
```


## Const
const is also introduced in ES6 and stands for "constant." Variables declared with const are block-scoped, just like let, but they have an additional characteristic, their value cannot be reassigned once it has been assigned. In other words, const variables are  **immutable** . However, **Objects and arrays are still mutable**.
Overall, Constant reference cannot be altered. Thus, you cannot re-assign a new value (whether another array or otherwise) to that constant. However, you can alter the contents of that array itself


```
function f(){
	const z = 30;
	console.log(z); //Output:30
	z = 40; // TypeError:Assignment to constant
}
example();
```



## To summarize:

- Use **var** if you want function-scoped variables that can be hoisted.
- Use **let** if you want block-scoped variables that can be reassigned.
- Use **const** if you want block-scoped variables that are constant and cannot be reassigned.



### References
- https://www.geeksforgeeks.org/difference-between-var-let-and-const-keywords-in-javascript/
- https://www.linkedin.com/pulse/var-vs-let-const-easiest-explanation-ever-asit-rohan-dass/
