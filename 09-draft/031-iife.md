# IIFE

>An **IIFE** (Immediately Invoked Function Expression) is a [JavaScript](https://developer.mozilla.org/en-US/docs/Glossary/JavaScript) [function](https://developer.mozilla.org/en-US/docs/Glossary/Function) that runs as soon as it is defined.

```javascript
(function () {
  statements
})();
```

- It's a design pattern - [Self-Executing Anonymous Function](https://developer.mozilla.org/en-US/docs/Glossary/Self-Executing_Anonymous_Function) 
  - The anonymous function with lexical scope enclosed within the [`Grouping Operator`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Grouping) `()`. This prevents accessing variables within the IIFE idiom as well as polluting the global scope.
  - Creating the immediately invoked function expression `()` through which the JavaScript engine will directly interpret the function.



## Use Cases

### Avoid polluting the global namespace

If we have some initiation code that we don't need to use again, we could use the IIFE pattern. As we will not reuse the code again, using IIFE in this case is better than using a function declaration or a function expression.

```javascript
(function () {
  // some initiation code
  let firstVariable;
  let secondVariable;
})();

// firstVariable and secondVariable will be discarded after the function is executed.
```



### The module pattern

```javascript
const makeWithdraw = balance => (function(copyBalance) {
  let balance = copyBalance; // This variable is private
  let doBadThings = function() {
    console.log("I will do bad things with your money");
  };
  doBadThings();
  return {
    withdraw: function(amount) {
      if (balance >= amount) {
        balance -= amount;
        return balance;
      } else {
        return "Insufficient money";
      }
    },
  }
})(balance);

const firstAccount = makeWithdraw(100); // "I will do bad things with your money"
console.log(firstAccount.balance); // undefined
console.log(firstAccount.withdraw(20)); // 80
console.log(firstAccount.withdraw(30)); // 50
console.log(firstAccount.doBadThings); // undefined, this method is private
const secondAccount = makeWithdraw(20);
secondAccount.withdraw(30); // "Insufficient money"
secondAccount.withdraw(20);  // 0
```









## Reference

[1] https://developer.mozilla.org/en-US/docs/Glossary/IIFE (已讀)

[2] https://pjchender.blogspot.com/2016/05/javascriptiifesimmediately-invoked.html （粗略讀過）

