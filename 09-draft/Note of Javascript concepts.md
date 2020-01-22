# JS 概念釐清

## Overview

- 名詞定義
- Immutable & Mutable
- Async & Await
- Promise
- Reference




## 名詞定義

Plain: ????

Reactivity:



## Immutable & Mutable

Immutable => pass by value

mutable => pass by reference (e.g: array, object, collection)



## Async & Await [1]

>ES6 出現 Promise 語法，ES7 出現 async、await 語法

#### 同步與非同步

我自己也超容易混淆 sync 跟 async 的定義

同步 (Sync) 模式：每個任務必須按照順序執行，後面的任務必須等待前面的任務執行完成

非同步 (Async) 模式：後面的任務不用等前面的，各自執行各自的任務

>同步：在「*同一個步道*」比賽「接力賽跑」，當棒子沒有交給我，我就得等你，不能跑。
>
>非同步：在「*不 ( 非 ) 同步道*」比賽「賽跑」，誰都不等誰，只要輪到我跑，我就開始跑。



#### Async &Await

只要 function 標記為 async，就表示裡頭可以撰寫 await 的同步語法

可以解決過去巢狀非同步操作的缺點 (ex: 輸入文字之層層疊疊 callback)，增加程式碼的可閱讀性

```javascript
async function a(){
  await b();
  .....       // 等 b() 完成後才會執行
  await c();
  .....       // 等 c() 完成後才會執行
  await new Promise(resolve=>{
    .....
  });
  .....       // 上方的 promise 完成後才會執行
}
a();
a().then(()=>{
  .....       // 等 a() 完成後接著執行
});
```





## Promise [2]



## Reference

[1] Async & Await

https://www.oxxostudio.tw/articles/201908/js-async-await.html

[2] Promise

https://developers.google.com/web/fundamentals/primers/promises?hl=en

[3] Reactivity

https://www.youtube.com/watch?v=7Cjb7Xj8fEI

