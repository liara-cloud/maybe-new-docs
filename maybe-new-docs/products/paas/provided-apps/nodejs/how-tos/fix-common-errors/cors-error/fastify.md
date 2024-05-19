# رفع خطای CORS در فریم‌ورک Fastify
برای رفع این خطا در فریم‌ورک Fastify، در ابتدا باید ماژول‌های `cors` و `fastify-express` را با اجرای دستور زیر، نصب کنید:

```
npm install cors fastify-express
```

در نهایت، می‌توانید از قطعه کد زیر در برنامه خود، استفاده کنید:

```js
await fastify.register(require('fastify-express'))
fastify.use(require('cors')())
```
برای کسب اطلاعات بیشتر می‌توانید [مستندات رسمی این فریم‌ورک](https://www.fastify.io/docs/latest/Middleware/) را مطالعه کنید.

