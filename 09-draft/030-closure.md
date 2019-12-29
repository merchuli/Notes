# Closure

閉包是一種資料結構，包含函式以及記住函式被建立時當下環境。

> 由於"閉包"這個字詞有多層意義，你可以說它是一種技術，或是一種資料結構，或是這種有記憶環境值的函式。



## 典型範例圖解

一個典型的Closure(閉包)會長這個樣子：

```typescript
const varGlobal = 'x';

function outer(paramOuter){
  const varOuter ='y';

  function inner(paramInner){
    const varInner ='z';

    console.log(varGlobal);   // x
    console.log(varOuter);    // y
    console.log(varInner);    // z
    console.log(paramOuter);  // a
    console.log(paramInner);  // b
  }

  return inner;
}

const func = outer('a');  // func = function inner
func('b');
```



### 圖解

用簡單的圖來表示上面的例子，內部(Inner)函式被回傳後，除了自己本身的程式碼外，也會捕抓到了環境的變數值，記住了執行當時的環境：![closure](../images/closure/closure.png)



### 案例

閉包的最大特點(賣點)就是它會記憶函式建立時的環境，也就是內部函式所能存取得到的作用域連鎖中的所有變數當下的值。但閉包是**參照(refer)而非複製**這些值。

它用了異步的回調函式：

```javascript
//錯誤示範
function counter() {
    let i = 0
    for (i = 0; i< 5; i++) {
        setTimeout(function() {
            console.log('counter is ' + i)
        }, 1000)
    }
}

counter()

// answer: 5, 5, 5, 5, 5
```



解法一：

```javascript
function counter(x) {
  setTimeout(function() {
      console.log('counter is ' + x)
  }, 1000)
}

for(let i=0; i < 5 ; i++){
  counter(i)
}

// answer: 0, 1, 2, 3, 4
```

> 解除原先`counter`中閉包的結構，讓`counter`函式中確保不會再帶有函式裡面的變數值，而是用一個傳入參數值讓`setTimeout`中的回調函式去作對應的輸出，因為每次傳入`counter`函式的`x`值都不同，也就能控制回調函式所能存取得到的環境值，藉此確保異步回調的輸出每次都是不同的。



解法二：

```javascript
function print(i){
  return function(){
    console.log('counter is ' + i)
  }
}

function counter() {
  let i = 0
  for (i = 0; i< 5; i++) {
      setTimeout(print(i), 1000)
  }
}

counter()
```

> 此法是想辦法鎖住`setTimeout`中回調函式的環境值

？？？？？？看不懂啊啊啊啊？？？？？



解法三：

```javascript
for (let i = 0; i < 5; i++) {
  (function(value) {
    setTimeout(function(){
      console.log('counter is ' + value);
    }, 1000)
  })(i)
}
```

>第三種解決是要用IIFE(立即呼叫函式表達式)的樣式，這個樣式已經是很特殊的用法了，IIFE也有儲存閉包的環境狀態的功用。
>
>註: 在for迴圈中的語法稱為"Immediately-Invoked Function Expression"（立即被呼叫的函式語樣）或簡稱為"IIFE"



## 閉包的記憶環境例外變數

閉包只有以下兩個外部函式的環境變數是不會自動記憶的。

- this: 執行外部函式時的 this 值
- arguments: 函式執行時的一個隱藏偽陣列物件



## 結論

1. 所有函式建立時都會產生閉包
2. 閉包所記憶的環境，其原理是來自作用域連鎖的設計，內部函式可以看(獲取)到外部函式的變數值與傳入參數值。
3. 函式的`this`值與`arguments`值並不屬於作用域連鎖，所以不包含在閉包記憶的環境中。





> > Reference 3 所提及的
> >
> > 可以進階模組模式～
> >
> > setTimeout 的解法 => 改成 let 既然是要為每次迭代建立區塊範疇，更好的解法就是使用 **let**，let 會在每次迭代時重新宣告變數 i，並將上一次迭代的結果作為這一次的初始值。

## Reference Link

[1] https://eyesofkids.gitbooks.io/javascript-start-from-es6/content/part4/closure.html

[2] https://pjchender.blogspot.tw/2016/05/javascriptclosures.html

[3] https://cythilya.github.io/2018/10/22/closure/

[4] https://github.com/getify/You-Dont-Know-JS/blob/master/scope%20%26%20closures/ch5.md



