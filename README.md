# Javascript Destructuring
The destructuring assignment syntax is a JavaScript expression that makes it possible to unpack values from arrays, or properties from objects, into distinct variables.

<ul>
  <li><a href="#array-destructuring" title="Array destructuring">Array destructuring</a></li>
  <li><a href="#object-destructuring" title="Object destructuring">Object destructuring</a></li>
</ul>

```javascript
let a, b, rest;
[a, b] = [10, 20];
console.log(a); // 10
console.log(b); // 20

[a, b, ...rest] = [10, 20, 30, 40, 50];
console.log(a); // 10
console.log(b); // 20
console.log(rest); // [30, 40, 50]

({ a, b } = { a: 10, b: 20 });
console.log(a); // 10
console.log(b); // 20


// Stage 4(finished) proposal
({a, b, ...rest} = {a: 10, b: 20, c: 30, d: 40});
console.log(a); // 10
console.log(b); // 20
console.log(rest); // {c: 30, d: 40}
```

## Array destructuring

#### Basic variable assignment
```javascript
const foo = ['one', 'two', 'three'];

const [red, yellow, green] = foo;
console.log(red); // "one"
console.log(yellow); // "two"
console.log(green); // "three"
```

#### Assignment separate from declaration
```javascript
let a, b;

[a, b] = [1, 2];
console.log(a); // 1
console.log(b); // 2
```

#### Default values
```javascript
let a, b;

[a=5, b=7] = [1];
console.log(a); // 1
console.log(b); // 7
```

#### Swapping variable
```javascript
let a = 1;
let b = 3;

[a, b] = [b, a];
console.log(a); // 3
console.log(b); // 1

const arr = [1,2,3];
[arr[2], arr[1]] = [arr[1], arr[2]];
console.log(arr); // [1,3,2]
```

#### Parsing an array returned from a function
```javascript
function f() {
  return [1, 2];
}

let a, b; 
[a, b] = f(); 
console.log(a); // 1
console.log(b); // 2
```

#### Ignoring some returned values
```javascript
function f() {
  return [1, 2, 3];
}

const [a, , b] = f();
console.log(a); // 1
console.log(b); // 3

// You can also ignore all returned values:
[,,] = f();
```

#### Assigning the rest of an array to a variable
```javascript
const [a, ...b] = [1, 2, 3];
console.log(a); // 1
console.log(b); // [2, 3]
```

## Object destructuring

#### Basic assignment
```javascript
const o = {p: 42, q: true};
const {p, q} = o;

console.log(p); // 42
console.log(q); // true 
```

#### Assignment without declaration
```javascript
let a, b;

({a, b} = {a: 1, b: 2});
```

**Notes:** The parentheses ( ... ) around the assignment statement are required when using object literal destructuring assignment without a declaration.

#### Assigning to new variable names
```javascript
const o = {p: 42, q: true};
const {p: foo, q: bar} = o;
 
console.log(foo); // 42 
console.log(bar); // true
```

#### Default values
```javascript
const {a = 10, b = 5} = {a: 3};

console.log(a); // 3
console.log(b); // 5
```

#### Assigning to new variables names and providing default values
```javascript
const {a: aa = 10, b: bb = 5} = {a: 3};

console.log(aa); // 3
console.log(bb); // 5
```

#### Unpacking fields from objects passed as function parameter
```javascript
const user = {
  id: 42,
  displayName: 'jdoe',
  fullName: {
    firstName: 'John',
    lastName: 'Doe'
  }
};

function userId({id}) {
  return id;
}

function whois({displayName, fullName: {firstName: name}}) {
  return `${displayName} is ${name}`;
}

console.log(userId(user)); // 42
console.log(whois(user));  // "jdoe is John"
```

#### Setting a function parameter's default value
```javascript
function drawChart({size = 'big', coords = {x: 0, y: 0}, radius = 25} = {}) {
  console.log(size, coords, radius);
  // do some chart drawing
}

drawChart({
  coords: {x: 18, y: 30},
  radius: 30
});
```

#### Nested object and array destructuring
```javascript
const metadata = {
  title: 'Scratchpad',
  translations: [
    {
      locale: 'de',
      localization_tags: [],
      last_edit: '2014-04-14T08:43:37',
      url: '/de/docs/Tools/Scratchpad',
      title: 'JavaScript-Umgebung'
    }
  ],
  url: '/en-US/docs/Tools/Scratchpad'
};

let {
  title: englishTitle, // rename
  translations: [
    {
       title: localeTitle, // rename
    },
  ],
} = metadata;

console.log(englishTitle); // "Scratchpad"
console.log(localeTitle);  // "JavaScript-Umgebung"
```

#### For of iteration and destructuring
```javascript
const people = [
  {
    name: 'Mike Smith',
    family: {
      mother: 'Jane Smith',
      father: 'Harry Smith',
      sister: 'Samantha Smith'
    },
    age: 35
  },
  {
    name: 'Tom Jones',
    family: {
      mother: 'Norah Jones',
      father: 'Richard Jones',
      brother: 'Howard Jones'
    },
    age: 25
  }
];

for (const {name: n, family: {father: f}} of people) {
  console.log('Name: ' + n + ', Father: ' + f);
}

// "Name: Mike Smith, Father: Harry Smith"
// "Name: Tom Jones, Father: Richard Jones"
```

#### Computed object property names and destructuring
```javascript
let key = 'z';
let {[key]: foo} = {z: 'bar'};

console.log(foo); // "bar"
```

#### Rest in Object Destructuring
```javascript
let {a, b, ...rest} = {a: 10, b: 20, c: 30, d: 40}
a; // 10 
b; // 20 
rest; // { c: 30, d: 40 }
```

#### Combined Array and Object Destructuring
```javascript
const props = [
  { id: 1, name: 'Fizz'},
  { id: 2, name: 'Buzz'},
  { id: 3, name: 'FizzBuzz'}
];

const [,, { name }] = props;

console.log(name); // "FizzBuzz"
```

#### The prototype chain is looked up when the object is deconstructed 
```javascript
let obj = {self: '123'};
obj.__proto__.prot = '456';
const {self, prot} = obj;
// self "123"
// prot "456"（Access to the prototype chain）
```
