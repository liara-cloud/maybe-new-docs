# استفاده از متغیرهای محیطی 

برای استفاده از متغیرهای محیطی در برنامه خود، در ابتدا باید طبق [مستندات تنظیم متغیرهای محیطی](../../../details/envs.md)، متغیرهای محیطی را به برنامه خود، اضافه کنید.

در ادامه، شما می‌توانید با استفاده از دستور `.process.env.` به متغیرهای محیطی خود در برنامه NodeJS، دسترسی داشته باشید؛ به عنوان مثال:

```js
console.log(`app listening on port 8000 on ${process.env.LIARA_URL}`) 
```


[گام بعدی - اتصال به دیتابیس](./connect-to-db/about.md)