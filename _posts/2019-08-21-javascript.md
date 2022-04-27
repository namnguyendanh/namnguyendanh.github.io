---
title:  "Javascript"
categories: 
    - programming
tags: 
    - web
header:
    image: /images/2019-08-21-javascript/header.png
---

## Học Javascript từ đầu ở đâu ?
- Xem video [này](https://www.youtube.com/watch?v=PkZNo7MFNFg). Dưới phần comments có note lại các chủ đề trong video.

## Basics 
### Chạy Javascript
Hầu hết OS đều có web browser hỗ trợ javascript. Nhấn `ctrl + shift + j` để hiện màn hình console trên browser.

```html
<script>
    console.log("Hello world");
<script/>
```

Một cách khác là sử dụng trang codepen.io 

### Comment
```javascript
// in-line comment
/*
multi-line comment
*/
```

### Data Types:
```javascript
/* Data Types:
underfined, null, boolean, string, symbol, number, and object
*/

var myName = "Đặng Quang Minh"
myName = 20
let ourName = "Hello World"
const pi = 3.14

console.log(typeof pi); // will print: "number"
// var is used in global scope, let is only used in local scope (let and const appeared in ES6 - 2015)

let a = 2;
a--;
a+=1;
a*=5;
a/=5;

let myStr = "My name is \"Đặng Quang Minh\" ";
myStr = 'My name is "Đặng Quang Minh"';
// Quite like Python syntax 
myStr = `'My name is "Đặng Quang Minh"'`;

let myStrLength = myStr.length;
```

- Sau mỗi dòng đều nên kết thúc với `;`
- Case Sensitive -> camelCase
- String is immutable

#### Array
- Nested Array
```javascript
array.pop();
array.shift(); //pop front
array.unshift(elem); //add to front
array.push(elem); //add to tail
```

### Function
```javascript
function print(a, b){
    console.log(a + b);
}
```

### If-else Statement
```javascript
3 == '3' // return true
3 === '3' // return false

if
else if
else
```

Note:
- Luôn dùng `===` để so sánh thay vì `==`, `!==` thay vì `!=`
- Còn lại giống C
- Ternary Operator
### Switch-case Statement
```
switch(){
    case: ... break;
    ...
    default: ... break;

}
```
Note:
- Thêm `break` trong trường hợp default là good practice.
- Giống C

### Object
- Giống Dictionary trong Python
- Nested Objects
- `myObj.hasOwnProperty(checkProp)`
- Có thể access dưới dạng `.` giống như access property của class.

### While Loop
- Giống C
### For Loop 
- Giống C

### Arrow Function (Anonymous Function)
```javascript
function a(){
    return new Date();
}
var magic = a();

// arrow function
var magic = () => {
    return new Date();
}
// or even shorter
var magic = () => new Date();
```

### Rest Operator (đọc thêm)

### Spread Operator (đọc thêm)

### Destructuring Assignment
```javascript
var voxel = {x:3.6; y:7.4, z:6.54};

const {x: a, y: b, z: c} = voxel; // a=3.6, b=7.4, c=6.54

// for swapping
[a, b] = [b, a]

// for skipping element
[a, b, , c] = [1,2,3,4] // skip 3

// remove first two element
const [a, b, ...arr] = list; // list = [1,2,3,4,5,6]
return arr; // arr = [3,4,5,6]
```

### Template literal
```javascript
const person = {
    name: "Đặng Quang Minh"
};
const greeting = `Hello, my name is ${person.name}`
```

### Class


### Import, Export, Require
- import default
## References:
- https://www.youtube.com/watch?v=PkZNo7MFNFg