# Hoisting

提升一詞很容易造成誤解：例如，提升看起來是單純地將變數和函式宣告，移動到程式的區塊頂端，然而並非如此。變數和函數的宣告會在編譯階段就被放入記憶體，但實際位置和程式碼中完全一樣。

>提升（Hoisting）是在 [ECMAScript® 2015 Language Specification](http://www.ecma-international.org/ecma-262/6.0/index.html) 裡面找不到的專有名詞。它是一種釐清 JaveScript 在執行階段內文如何運行的思路（尤其是在創建和執行階段）。



## 範例

在執行任何程式碼前，JavaScript 會把函式宣告放進記憶體裡面，這樣做的優點是：可以在程式碼宣告該函式之前使用它。

### 函式

```javascript
function catName(name) {
  console.log("My cat's name is " + name);
}

catName("Tigger");
/*
上面程式的結果是: "My cat's name is Tigger"
*/
```

即使我們函式的程式碼之前就先呼叫它，程式碼仍然可以運作。這是出於 JavaScript 內文執行的運作原理。

```javascript
catName("Chloe");

function catName(name) {
  console.log("My cat's name is " + name);
}
/*
上面程式的結果是: "My cat's name is Chloe"
*/
```



### 變數

JavaScript 僅提升宣告的部分，而不是初始化。

```javascript
num = 6;
num + 7;
var num; 
/* 只要 num 有被宣告，就不會有錯誤 */
```



如果在使用該變數後才宣告和初始化，那麼該值將是 undefined。

```javascript
var x = 1; // 初始化 x
console.log(x + " " + y);  // '1 undefined'
var y = 2;
//上下的程式結果都一樣

var x = 1; // 初始化 x
var y; // 宣告 y
console.log(x + " " + y);  // '1 undefined'
y = 2; // 初始化 y
```



## Reference Link

[1] https://developer.mozilla.org/zh-TW/docs/Glossary/Hoisting

