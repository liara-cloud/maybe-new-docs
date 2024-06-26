# استفاده از متغیرهای محیطی 


برای استفاده از متغیرهای محیطی در برنامه خود، در ابتدا باید طبق [مستندات تنظیم متغیرهای محیطی](../../../details/envs.md)، متغیرهای محیطی را به برنامه خود، اضافه کنید. در نظر داشته باشید که نام تمامی متغیرهای محیطی برنامه‌تان باید با عبارت `_NEXT_PUBLIC` شروع شود تا در نهایت به درستی در برنامه بارگذاری شوند.

در ادامه، شما می‌توانید با استفاده از دستور `.process.env` به متغیرهای محیطی خود در برنامه NextJS، دسترسی داشته باشید؛ به عنوان مثال:

```js
console.log(`app listening on ${process.env.NEXT_PUBLIC_LIARA_URL}`) 
```

## بارگذاری متغیرهای محیطی در سرور
در NextJS، متغیرهای محیطی که با `_NEXT_PUBLIC` شروع می‌شوند به صورت خودکار به کد سمت کلاینت منتقل می‌شوند و در مرورگر کاربر قابل دسترسی هستند. برای متغیرهایی که باید محرمانه بمانند و فقط در سرور استفاده شوند، نباید از پیشوند `_NEXT_PUBLIC` استفاده کنید.

شما می‌توانید برای بارگذاری متغیرهای محیطی خود یک فایل به نام `env.production.` در مسیر اصلی پروژه خود ایجاد کنید و متغیرهای محیطی خود را در آنجا تعریف کرده و در پروژه خود استفاده کنید. پس از انجام این کارها، می‌توانید پروژه خود را در لیارا مستقر کنید. 

در روش بارگذاری متغیرهای محیطی در سرور، نیازی به استفاده از پیشوند `_NEXT_PUBLIC`  نیست.

> [!NOTE]
> برای بارگذاری صحیح متغیرهای محیطی در برنامه، باید برنامه یک‌بار ری‌استارت شود.

[گام بعدی - اتصال به دیتابیس](./connect-to-db/about.md)