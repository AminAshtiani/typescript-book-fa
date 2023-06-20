<div dir="rtl">

## اعداد

وقتی که در هر زبان برنامه نویسی با اعداد سر و کار دارید باید از ویژگی های خاص اون زبان در مورد اعداد اگاه باشید. الان هم میخوایم یه سری اطلاعات مهم در مورد اعداد توی جاوااسکریپت ارائه بدیم که هر برنامه نویسی باید ازش آگاه باشه.

### انواع اصلی
جاوااسکریپت فقط یه مدل عدد داره و اون هم عدد اعشاری ۶۴ بیتی هستش که بهش  `Number` میگیم. در ادامه در مورد محدودیت‌های این عدد و راهکارهایی که براش داریم صحبت می‌کنیم.

### بر مبنای ده
برای کسانی که با doubles / float در زبانهای دیگه کار کرده‌اند، احتمالا میدونن که عدداعشاری دو دویی به درستی به مبنای ده مپ نمیشه. یه مثال کوچیک ولی خیلی معروف در این زمینه در جاوااسکریپت رو در قطعه کد زیر می‌بینیم: 


```js
console.log(.1 + .2); // 0.30000000000000004
```

> برای محاسبات مبنای ده که میخواید دقت درستی داشته باشه و مثل مثال بالا نباشه از `big.js` استفاده کنید که پایین تر در موردش صحبت می‌کنیم.

### عدد صحیح
محدودیت اعداد صحیح رو میتونیم با استفاده از `Number.MAX_SAFE_INTEGER` و `Number.MIN_SAFE_INTEGER` ببینیم.

```js
console.log({max: Number.MAX_SAFE_INTEGER, min: Number.MIN_SAFE_INTEGER});
// {max: 9007199254740991, min: -9007199254740991}
```
همانطور که در مثال بالا دیدیم عددها در جاوااسکریپت یه مقدار *safe* دارن. به عددی میتونیم بگیم *safe* که در این باز قرار بگیره والبته *گرد نشده باشه* چه زمانی این گرد کردن اتفاق میوفته؟ از نظر جاوا اسکریپت تمام اعداد داخل این بازه و البته حاصل جمع این دو کران با `+1 \ -1`  safe هستن یعنی زمان استفاده از اینها به مشکل نمی خوریم اما همانطور که در مثال پایین می‌بینید  وقتی از این دو کران خارج میشه جاوااسکریپت شروع به گرد کردن اعداد میکنه که قطعا شما رو دچار مشکل می‌کنه

```js
console.log(Number.MAX_SAFE_INTEGER + 1 === Number.MAX_SAFE_INTEGER + 2); // true!
console.log(Number.MIN_SAFE_INTEGER - 1 === Number.MIN_SAFE_INTEGER - 2); // true!

console.log(Number.MAX_SAFE_INTEGER);      // 9007199254740991
console.log(Number.MAX_SAFE_INTEGER + 1);  // 9007199254740992 - Correct
console.log(Number.MAX_SAFE_INTEGER + 2);  // 9007199254740992 - Rounded!
console.log(Number.MAX_SAFE_INTEGER + 3);  // 9007199254740994 - Rounded - correct by luck
console.log(Number.MAX_SAFE_INTEGER + 4);  // 9007199254740996 - Rounded!
```
حالا چطور میتونیم از safe بودن عددمون مطمئن باشیم؟ برای اینکه بتونیم عددمون رو چک کنیم باید از یه متد که روی ‍‍`Number` تعریف شده استفاده کنیم. اسم این متد `isSafeInteger` هستش: 

```js
// Safe value
console.log(Number.isSafeInteger(Number.MAX_SAFE_INTEGER)); // true

// Unsafe value
console.log(Number.isSafeInteger(Number.MAX_SAFE_INTEGER + 1)); // false

// Because it might have been rounded to it due to overflow
console.log(Number.isSafeInteger(Number.MAX_SAFE_INTEGER + 10)); // false
```
> واقعیت اینه که جاوااسکریپت ساپورت [BigInt](https://developers.google.com/web/updates/2018/05/bigint) رو خواهد داشت. اما تا اون موقع برای محاسبات خودتون با اعداد صحیح اگر دقت بیشتری نیاز دارید بهتره که از `big.js` که پایین‌تر در موردش صحبت می کنیم استفاده کنیم.

### big.js
هر موقع که دارید محاسبات مالی انجام می‌دید (به طور مثال GST calculation, پول و پول خوردش و ....) از کتابخانه ای مثل [big.js](https://github.com/MikeMcl/big.js/) باید استفاده کنیم تا دقت و جزئیات رو بهمون بده. این کتابخانه و امثال این کتابخانه برای این موضوع طراحی شدن که : 
* محاسبات مبنای ده با دقت زیاد
* استفاده از اعداد خارج از بازه‌ای که ‍`Number` در اختیار شما میزاره

تصبش که خیلی ساده است:
```bash
npm install big.js @types/big.js
```

و یه مثال ساده و سریع از این کتابخونه:

```js
import { Big } from 'big.js';

export const foo = new Big('111.11111111111111111111');
export const bar = foo.plus(new Big('0.00000000000000000001'));

// To get a number:
const x: number = Number(bar.toString()); // Loses the precision
```

> به خاطر داشته باشید که از این کتابخونه برای محاسبات UI و مواردی که روی پرفورمنس اثر داره مثل کشیدن canvas و chartها استفاده نکنید.

### NaN
بعضی وقتا هم حاصل محاسبات عددی شما قابل ارائه به صورت عدد نیست ، یعنی اصلا عدد نیست و محاسبه شما یه ایرادی داره. جاوااسکریپت یه مقدار خاص داره که اون رو به شما میده که بهش میگیم `NaN`. یه مثال کلاسیک از این موضوع عدد مختلطه: 

```js
console.log(Math.sqrt(-1)); // NaN
```

توجه: نمیتونیم از `NaN` در تساوی استفاده کنیم چون نتیجه درستی نداره. بهتره که از `Number.isNaN` استفاده کنیم.
```js
// Don't do this
console.log(NaN === NaN); // false!!

// Do this
console.log(Number.isNaN(NaN)); // true
```

### بی‌نهایت
هر عددی خارج کران بالا و پایینی که بالاتر بهش اشاره کردیم به صورت استاتیک در `Number.MAX_VALUE` و ‍`-Number.MAX_VALUE` می‌گنجه.

```js
console.log(Number.MAX_VALUE);  // 1.7976931348623157e+308
console.log(-Number.MAX_VALUE); // -1.7976931348623157e+308
```
همانطور که در مثال پایین می‌بینید مقادیر خارج این بازه جمع و تفریق هم روشون تاثیر نداره :)

```js
console.log(Number.MAX_VALUE + 1 == Number.MAX_VALUE);   // true!
console.log(-Number.MAX_VALUE - 1 == -Number.MAX_VALUE); // true!
```
مقادیری که در موردشون صحبت کردیم رو به صورت یه مقدار ویژه هم میتونیم نمایش بدیم که بهش `Infinity/-Infinity` میگیم.

```js
console.log(Number.MAX_VALUE + 1e292);  // Infinity
console.log(-Number.MAX_VALUE - 1e292); // -Infinity
```
البته اگر چیزی از ریاضی یادتون مونده باشه میدونید که حاصل برخی محاسبات ریاضی هم نتیجه بی‌نهایت داره که توی جاوااسکریپت از همین مقداری که  گفتیم استفاده میشه:

```js
console.log( 1 / 0); // Infinity
console.log(-1 / 0); // -Infinity
```

شما میتونید از همین `Infinity` استفاده کنید یا از دوتا مقدار استاتیک که روی `Nubmer` استفاده شده استفاده کنید.

```js
console.log(Number.POSITIVE_INFINITY === Infinity);  // true
console.log(Number.NEGATIVE_INFINITY === -Infinity); // true
```

خوش بختانه عملگرهای مقایسه‌ای در برابر Infinity رفتار قابل اعتمادی دارن و مشکلی پیش نمیاد.

```js
console.log( Infinity >  1); // true
console.log(-Infinity < -1); // true
```

### بی‌نهایت کوچیک
کوچیکترین عدد غیر صفر هم در Number به صورت یه مقدار استاتیک `Number.MIN_VALUE` در دسترسه.
The smallest non-zero value representable in Number is available as static `Number.MIN_VALUE`

```js
console.log(Number.MIN_VALUE);  // 5e-324
```
اعداد کوچیک تر از این مقدار به صورت خودکار به 0 تبدیل میشن.

```js
console.log(Number.MIN_VALUE / 10);  // 0
```
> چرا؟ همون طور که اعداد بزرگتر از ‍`Number.MAX_VALUE` به INFINITY تبدیل میشن،منطقیش اینه که مقادیر کوچیک تر از `Number.MIN_VALUE` هم به 0 تبدیل بشن. 

</div>