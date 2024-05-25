# تنظیم TrustedProxies برای فریم‌ورک Express JS

برای تنظیم این قابلیت در برنامه خود، کافیست تا قطعه کد زیر را به برنامه، اضافه کنید:

```js
const express = require('express');
const app = express();

app.set('trust proxy', 1);

// usage example:
app.get('/', (req, res) => {
  const ip = req.ip;
  res.send(`Client IP: ${ip}`);
});
```

