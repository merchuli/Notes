# MDN Javascript

>記錄一下沒使用過的用法

1. Map [1]

   // 待補使用情境

2. Set [2]

   // 待補使用情境

3. () => ({})

   ```javascript
   const someName = () => ({
   	...
   });
   ```

4. Object.entries() [4]

5. Destructuring Assignment [5]

   ```javascript
   const duration = {
   	startDate: '',
   	endDate: '',
     others: {
       excludedDates: [],
     },
   };
   
   //// Using destructuring assignment
   const {duration: {startDate, endDate, others: {excludedDates}} = this;
   //// => get startDate, endDate, excludedDates       
   ```

   

   













## Reference

[1] https://pjchender.github.io/2018/07/30/js-javascript-map/

[2] https://developer.mozilla.org/zh-TW/docs/Web/JavaScript/Reference/Global_Objects/Set

[3]

[4] https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Object/entries

[5] https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Destructuring_assignment