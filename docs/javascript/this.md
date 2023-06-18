<div dir="rtl">

## this

هرگونه دسترسی به ‍`this` داخل یک فانکشن بستگی به این داره که این فانکشن کجا و چطور داره استفاده می‌شه. معمولا به این کجا و چطور ‍`calling context` میگن.

به طور مثال:

```ts
function foo() {
  console.log(this);
}

foo(); // logs out the global e.g. `window` in browsers
let bar = {
  foo
}
bar.foo(); // Logs out `bar` as `foo` was called on `bar`
```
بنابراین حواسمون باید به استفاده از `this` باشه. مثلا وقتی داریم از ‍`this` داخل متدهای یه کلاس استفاده می‌کنیم برای اینکه ارتباط `this` با `calling context` رو قطع کنیم و `this` دقیقا به کلاس اشاره کنه باید از `bind` داخل سازنده کلاس استفاده کنیم یا از `arrow function` استفاده کنیم. [در این مورد بیشتر بخونید][arrow].

[arrow]:../arrow-functions.md

</div>