<div dir="rtl">

## Closure

بهترین چیزی که جاوااسکریپت تاحالا داشته، ‍`closure‍‍` هستش. یه فانکشن در جاوااسکریپت که به هر متغییر خارج از اسکوپ خودش دسترسی داره. اما فهمیدن کلوژر با این یه جمله امکان پذیر نیست و بهتره که یه مثال در این مورد ببینیم:

```ts
function outerFunction(arg) {
    var variableInOuterFunction = arg;

    function bar() {
        console.log(variableInOuterFunction); // Access a variable from the outer scope
    }

    // Call the local function to demonstrate that it has access to arg
    bar();
}

outerFunction("hello closure"); // logs hello closure!
```
 واضحه که فانکشن داخلی به یه متغییر (variableInOuterFunction) از اسکوپ بیرون دسترسی داره. متغییری که در اسکوپ بیرونی تعریف شده به فانکشن داخلی وصله (bound in)  به همین خاطره که به این مفهوم میگن **closure**. خود این مفهوم به اندازه کافی ساده و شهودیه.

حالا نکته جالبتر : فانکشن داخلی به متغییر خارج از اسکوپ خودش دسترسی داره *حتی بعد اینکه فانکشن بیرونی return شده یا به عبارتی terminate شده*. این به این خاطره که متغییر به فاکنش داخلی وصل شده و دیگه به فانکشن بیرون وابسته نیست. دوباره به یه مثال توجه کنید:

```ts
function outerFunction(arg) {
    var variableInOuterFunction = arg;
    return function() {
        console.log(variableInOuterFunction);
    }
}

var innerFunction = outerFunction("hello closure!");

// Note the outerFunction has returned
innerFunction(); // logs hello closure!
```

### ولی چرا این موضوع خیلی خفنه؟
این قابلیت به شما اجازه میده به راحتی آبجکت بسازید به طور مثال [‍revealing module pattern](https://medium.com/@Rahulx1/revealing-module-pattern-tips-e3442d4e352) رو در نظر بگیرید.

```ts
function createCounter() {
    let val = 0;
    return {
        increment() { val++ },
        getVal() { return val }
    }
}

let counter = createCounter();
counter.increment();
console.log(counter.getVal()); // 1
counter.increment();
console.log(counter.getVal()); // 2
```
اگه بخوایم نمونه های بزرتر رو مثال بزنیم closure باعث میشه چیزی مثل nodejs امکان پذیر باشه به طور مثال نمونه کد زیر برای ساخت یه سرور nodejs استفاده میشه.

```ts
// Pseudo code to explain the concept
server.on(function handler(req, res) {
    loadData(req.id).then(function(data) {
        // the `res` has been closed over and is available
        res.send(data);
    })
});
```

</div>