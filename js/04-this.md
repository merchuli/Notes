# this

判斷 this 的四個規則：new 綁定、明確綁定、隱含綁定、預設綁定（依優先權排列）



## new 綁定

this 會指向 new 出來的物件。

```javascript
function Cat(param) {
  this.name = param;
  this.sayHi = function() {
    console.log(`Hi, name: ${name} - this.name: ${this.name} - param: ${param}`);
    console.log(this === kitten);
  }
}

const kitten = new Cat('Kitty');
///////////////////////////////////
kitten.sayHi();
// Hi, name:  - this.name: Kitty - param: Kitty
// true
```



## 明確綁定(Explicit Binding)

使用 call, apply, bind，明確指出要綁給 this 的物件。

### Call

將第一個參數設定為函式內的 context，意即設定第一個參數為函式內 this 的值。

```javascript
let cat = {
  name: 'Kitty'
};

let dog = {
  name: 'Snoopy'
};

function sayHi(location, color) {
  console.log(`I am ${this.name}, from ${location}. My favorite color is ${color}.`);
}

sayHi.call(cat, 'Japan', 'Pink'); // I am Kitty, from Japan. My favorite color is Pink.
sayHi.call(dog, 'the US', 'white'); // I am Snoopy, from the US. My favorite color is white.
```



### Apply

與 Call 的差異只在於傳參數的方法，call 將參數一一傳入，而 apply 將參數用陣列的方式傳入。

```javascript
let cat = {
  name: 'Kitty'
};

let dog = {
  name: 'Snoopy'
};

function sayHi(location, color) {
  console.log(`I am ${this.name}, from ${location}. My favorite color is ${color}.`);
}

sayHi.apply(cat, ['Japan', 'Pink']); // I am Kitty, from Japan. My favorite color is Pink.
sayHi.apply(dog, ['the US', 'white']); // I am Snoopy, from the US. My favorite color is white.
```



### Bind

在執行函式前，綁定要指定的物件，這樣 this 就會是這個物件。

```javascript
let cat = {
  name: 'Kitty'
};

let dog = {
  name: 'Snoopy'
};

function sayHi() {
  console.log(`I am ${this.name}.`);
}

const catSayHi = sayHi.bind(cat);
const dogSayHi = sayHi.bind(dog);

catSayHi(); // I am Kitty.
dogSayHi(); // I am Snoopy.
```

Or

```javascript
const obj = {
  msg: 'Hi!'
};

setTimeout(function() {
  console.log(`in setTimeout: ${this.msg}`);
}.bind(obj), 2000);
```



## 隱含綁定(Implicit Binding)

當函式為物件的方法（method）時，在執行階段 this 就會被綁定至該物件

```javascript
const user = {
  name: 'Jack',
  sayHi: prompt
}

function prompt() {
  console.log(this.name);
}

user.sayHi(); // 'Jack'
```



```javascript
const anotherUser = {
  name: 'Not Jack!',
  sayHi: prompt
}

const user = {
  name: 'Jack',
  anotherUser: anotherUser
}

function prompt() {
  console.log(this.name);
}

user.anotherUser.sayHi(); // 'Not Jack!'
```

### 隱含的失去：

1. 函式被 copy by reference (Case 1)

2. 參數傳遞中的 callback (Case 2)

3. DOM Event 的事件綁定 (Case 3)

   

Case 1:

```javascript
function foo() {
  console.log(this.a);
}

var obj = {
  a: 2,
  foo: foo
};

var bar = obj.foo;  // bar: f foo() { console.log(this.a); }
var a = 'oops, global';
bar(); // 'oops, global', 如同預設綁定的狀況
```



Case 2:

```javascript
function foo() {
  console.log(this.a);
}

function doFoo(fn) {
  fn();
}

var obj = {
  a: 2,
  foo: foo
};

var a = 'oops, global';

doFoo(obj.foo); // 'oops, global'
```



Case 3:

```javascript
// Dom element
<button id="button">Click Me!</button>

// Js
var el = document.getElementById('button');

el.addEventListener('click', function(e) {
  console.log(this); // "<button id='button'>Click Me!</button>"
});
```



## 預設綁定(Default Binding)

預設為全域物件，瀏覽器下就是指 window，但在嚴格模式(strict mode)下為 undefined。

```javascript
function sayHello() {
  console.log(this.hello);
}

var hello = 'Hello World';
sayHello(); // Hello World，如果是嚴格模式就是 undefined


/////////////////////////////////////////////////////
function sayHello() {
  console.log(this.hello);
}

function sayByebye(fn) {
    var hello = 'byebye';
    fn();
}

var hello = 'Hello World';
sayByebye(sayHello); // Hello World，如果是嚴格模式就是 undefined
```



## 綁定的例外特別篇

```javascript
const name = 'Apple';
const obj = {
  name: 'Jack',
  sayHi: function() {
    console.log(`Hi, I am ${this.name}.`);
  }
}

obj.sayHi(); // Hi, I am Jack.
setTimeout(obj.sayHi, 1000); // Hi, I am Apple.
```

setTimeout(obj.sayHi, 1000);

因為隱含的失去中的函式是另一個函式的參考而失去了原先 this 的綁定。



```javascript
const name = 'Apple';
const obj = {
  name: 'Jack',
  sayHi: function() {
    setTimeout(function() {
      console.log(`Hi, I am ${this.name}.`);   
    }, 1000);
  }
}

obj.sayHi();
// 1 秒後
// Hi, I am Apple.
///////////////////////////////////////////
// 另外用變數儲存當時的 this 環境
const name = 'Apple';
const obj = {
  name: 'Jack',
  sayHi: function() {
    const that = this;
    setTimeout(function() {
      console.log(`Hi, I am ${that.name}.`);   
    }, 1000);
  }
}

obj.sayHi();
// 1 秒後
// Hi, I am Jack.
//////////////////////////////////////////
//  使用 arrow function, 但不一定適用所有情況
const name = 'Apple';
const obj = {
  name: 'Jack',
  sayHi: function() {
    setTimeout(() => {
      console.log(`Hi, I am ${this.name}.`);   
    }, 1000);
  }
}

obj.sayHi();
// 1 秒後
// Hi, I am Jack.
///////////////////////////////////////////
///////////////////////////////////////////
const name = 'Apple';
const obj = {
  name: 'Jack',
  sayHi: setTimeout(() => {
    console.log(`Hi, I am ${this.name}.`);   
  }, 1000)
}
// 1 秒後直接印出 Hi, I am Apple.
```



## Summary

1. this 是 function 執行時所屬的物件，this 是在執行時期做綁定，其值和函式在哪裡被呼叫有關。
2. 判斷 this 的值的四個規則：new > explicit > implicit > default



## Reference Link

[1] https://ithelp.ithome.com.tw/articles/10204472

[2] https://www.youtube.com/watch?v=BlT6pCG2M1I [00'00 - 13'50]

