# Promise

> 類似 Observable （在知道 Promise 之前就先接觸過 RxJS 的 Observable ），負責處理異步時的回調函式以及事件監聽

**定義：**

1. Promise 物件代表一個即將完成 / 失敗的非同步操作，以及它所產生的值。 - by MDN

2. Promise 就像點餐的叫號機，當先前承諾的工作完成時，會通知取餐！ - by [1] Summer

   （個人很喜歡這個比喻，覺得淺顯易懂。）



## 筆記

### Promise 解決了什麼？(What's all the fuss about?)

JavaScript 很重要的特性就是單線程，意味著兩段 script 不能同時運作，只能一段一段進行。

以下例子使用事件來解決一些事情，我們獲得圖片、添加幾個偵聽器，之後 JavaScript 可停止執行，直至其中一個偵聽器被調用。

```javascript
var img1 = document.querySelector('.img-1');

img1.addEventListener('load', function() {
  // woo yey image loaded
});

img1.addEventListener('error', function() {
  // argh everything's broken
});
```



但遺憾的事，已上述的 sample code 為例，事件有可能在我們開始偵聽之前就發生了，因此會需要使用圖像的 "complete" 屬性來解決該問題（如下）：

```javascript
var img1 = document.querySelector('.img-1');

function loaded() {
  // woo yey image loaded
}

if (img1.complete) {
  loaded();
}
else {
  img1.addEventListener('load', loaded);
}

img1.addEventListener('error', function() {
  // argh everything's broken
});
```



但這又會出現另一個問題，如果在圖片加載完成之前已跑過 error 的 addEventListener ，我們便沒有機會偵聽到錯誤，而 DOM 並沒有為此給出解決之道。更麻煩的是這還只是加載一個圖像，如果加載一組圖像或其他更多元的運作，處理情境將更複雜。



### Events aren't always the best way

- A promise can only succeed or fail once. It cannot succeed or fail twice, neither can it switch from success to failure or vice versa.
- If a promise has succeeded or failed and you later add a success/failure callback, the correct callback will be called, even though the event took place earlier.



### Promise 術語

- **fulfilled** - The action relating to the promise succeeded
- **rejected** - The action relating to the promise failed
- **pending** - Hasn't fulfilled or rejected yet
- **settled** - Has fulfilled or rejected



### Promises arrive in JavaScript!

Here's how you create a promise:

```javascript
const promise = new Promise((resolve, reject) => {
  // do a thing, possibly async, then…

  if (/* everything turned out fine */) {
    resolve("Stuff worked!");
  }
  else {
    reject(Error("It broke"));
  }
});
```







｀











## Reference Link

[1] https://cythilya.github.io/2018/10/31/promise/

[2] https://developers.google.com/web/fundamentals/primers/promises?hl=en

[3] https://developer.mozilla.org/zh-TW/docs/Web/JavaScript/Reference/Global_Objects/Promise