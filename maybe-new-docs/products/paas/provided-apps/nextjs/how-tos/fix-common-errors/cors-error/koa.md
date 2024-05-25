# رفع خطای CORS در فریم‌ورک Koa
برای رفع این خطا در فریم‌ورک Koa، در ابتدا باید ماژول `cors` را با اجرای دستور زیر، نصب کنید:

```
npm install @koa/cors
```

در نهایت، می‌توانید در برنامه خود، مانند قطعه کد زیر، عمل کنید:

```js
const Koa = require('koa');
const cors = require('@koa/cors');
const app = new Koa();

// Enable CORS for all origins
app.use(cors());

app.listen(3000, () => {
  console.log('Server is running on port 3000');
});
```