# اتصال به دیتابیس MongoDB در برنامه‌های NodeJS

[ویدیوی آموزشی](https://files.liara.ir/liara/nodejs/nodejs-mongodb.mp4)

برای اتصال به دیتابیس MongoDB، می‌توانید از پکیج `mongoose` استفاده کنید، برای نصب این پکیج، کافیست تا در مسیر اصلی پروژه خود، دستور زیر را اجرا کنید:


```
npm install mongoose
```

پس از آن، کافیست تا طبق [مستندات تنظیم متغیرهای محیطی](../../../../details/envs.md) اطلاعات مربوط به دیتابیس خود را وارد کنید. به عنوان مثال:

```
MONGODB_URI=mongodb://root:fWy9QPKw8CxTUGCMd6esZSlu@mongo:27017/my-app?authSource=admin
```

و در برنامه به این صورت اطلاعات را خوانده و به دیتابیس متصل شوید:

```js
const express = require('express');
const mongoose = require('mongoose');

const app = express();

mongoose.connect(process.env.MONGODB_URI, {
})
.then(() => {
  console.log('connected to db successfully');
})
.catch((err) => {
  console.error('error in connecting to db:', err);
});


app.get('/', (req, res) => {
  res.send(mongoose.connection.readyState === 1 ? 'connection to db successfull' : 'error in connecting to db');
});

const PORT = process.env.PORT || 3000;
app.listen(PORT, () => {
  console.log(`server is listening on port ${PORT}`);
});

process.on('SIGINT', () => {
  mongoose.connection.close(() => {
    console.log('db disconnected.');
    process.exit();
  });
});
```

## استفاده از Connection Pooling

مفهوم Connection pooling به معنای استفاده از یک مجموعه اتصالات از پیش ساخته شده به پایگاه داده است. این تکنیک باعث می‌شود به جای ایجاد و بستن مکرر اتصالات، از اتصالات موجود در مجموعه استفاده شود که کارایی را افزایش می‌دهد.


> [!TIP]
> همچنین بخوانید: [قابلیت Conneciton Pooling چیست؟](../../../../../dbaas/details/connection-pool.md)


برای استفاده از این قابلیت در دیتابیس MongoDB نیاز به انجام کار خاصی نیست؛ چرا که پکیج `mongoose` به صورت خودکار، از این قابلیت، پشتیبانی می‌کند. صرفاً کافیست تا در آپشن‌های اتصال به دیتابیس، مقدار فیلد maxPoolSize را مشخص کنید:


```js
const express = require('express');
const mongoose = require('mongoose');

const app = express();

// Create a connection pool
const dbOptions = {
  maxPoolSize: 10, // Set the pool size as needed
};

mongoose.connect(process.env.MONGODB_URI, dbOptions)
  .then(() => {
    console.log('connected to db successfully');
  })
  .catch((err) => {
    console.error('error in connecting to db:', err);
  });

app.get('/', (req, res) => {
  res.send(mongoose.connection.readyState === 1 ? 'connection to db successful' : 'error in connecting to db');
});

const PORT = process.env.PORT || 3000;
app.listen(PORT, () => {
  console.log(`server is listening on port ${PORT}`);
});

```
