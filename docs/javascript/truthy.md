<div dir="rtl">

## Truthy

جاوااسکریپت یه مفهومی داره به اسم `truthy`. همونطور که میدونید مقادیر مختلف توی یه سری جاها به `true` تبدیل میشن مثل وقتی که داریم از `if` یا عملگرهای `||` و `&&` استفاده می کنیم. به این چیزا توی جاوااسکریپت میگیم `truthy`. به طور مثال تمام اعداد غیر از صفر `truthy` هستن.

```ts
if (123) { // Will be treated like `true`
  console.log('Any number other than 0 is truthy');
}
```
و هر چیزی که `truthy` نیست رو بهش میگیم `falsy`.

برای اینکه یه مرجعی داشته باشید که چی truthy هست و چی نیست از جدول زیر میتونید استفاده کنید..

| Variable Type   | When it is *falsy*       | When it is *truthy*      |
|-----------------|--------------------------|--------------------------|
| `boolean`       | `false`                  | `true`                   |
| `string`        | `''` (empty string)      | any other string         |
| `number`        | `0`  `NaN`               | any other number         |
| `null`          | always                   | never                    |
| `undefined`     | always                   | never                    |
| هر آبجکتی به جز  `{}`,`[]` | never | always |


### being explicit (واقعا معنای خوبی براش پیدا نکردم)

> The `!!` pattern

خیلی متداوله که شما به صورت ضمنی و با یه هدفی بخوای با یه مقداری به صورت `boolean` رفتار کنی و به یه `boolean` واقعی تبدیلش کنی. یعنی واقعا true یا false باشه . برای اینکه این تبدیل رو انجام بدی لازمه که قبل اسم متغییرت `!!` رو بزاری. در اصل داریم از عملگر`!` دوبار پشت سر هم استفاده میکنیم. دفعه اول مقدار متغییر ما به `boolean` تبدیل میشه اما با مقدار مخالفش. یعنی اگر `truthy` بوده به false واگر `falsy` بوده به true تبدیل میشه. وقتی برای بار دوم ازش استفاده میکنیم هر مقداری رو که داریم دوباره toggle میکنیم و به مقدار واقعی میرسیم.

استفاده از این پترن تو خیلی از جاها متداوله.

```js
// Direct variables
const hasName = !!name;

// As members of objects
const someObj = {
  hasName: !!name
}

// e.g. in ReactJS JSX
{!!someName && <div>{someName}</div>}
```

</div>