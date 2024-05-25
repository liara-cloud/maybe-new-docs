# تنظیم TrustedProxies برای فریم‌ورک Koa

برای تنظیم این قابلیت در برنامه خود، کافیست تا قطعه کد زیر را به برنامه، اضافه کنید:

```js
const Koa = require('koa');
const app = new Koa();
app.proxy = true; 

// usage example:
app.use(async ctx => {
  const ip = ctx.request.ip;
  ctx.body = `Client IP: ${ip}`;
});
```

یا می‌توانید از قطعه کد زیر، استفاده کنید:

```js
const Koa = require('koa');
const app = new Koa({ proxy: true });

// usage example:
app.use(async ctx => {
  const ip = ctx.request.ip;
  ctx.body = `Client IP: ${ip}`;
});
```

