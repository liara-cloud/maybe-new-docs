# اتصال به دیتابیس PostgreSQL در برنامه‌های NodeJS

[ویدیوی آموزشی](https://files.liara.ir/liara/nodejs/nodejs-postgres.mp4)

برای اتصال به دیتابیس Postgres، می‌توانید از پکیج `pg` استفاده کنید، برای نصب این پکیج، کافیست تا در مسیر اصلی پروژه خود، دستور زیر را اجرا کنید:

```
npm install pg
```

پس از آن، کافیست تا طبق [مستندات تنظیم متغیرهای محیطی](../../../../details/envs.md) اطلاعات مربوط به دیتابیس خود را وارد کنید. به عنوان مثال:

```
PG_URI=postgresql://root:jpR53iIAhADgqsJ3ufVE1v94@pgo:5432/postgres
```

و در برنامه به این صورت اطلاعات را خوانده و به دیتابیس متصل شوید:

```js
const express = require('express');
const { Pool } = require('pg');

const app = express();

const dbURI = process.env.PG_URI;

const pool = new Pool({
  connectionString: dbURI,
});

pool.connect((err) => {
  if (err) {
    console.error('Error connecting to PostgreSQL database:', err);
    app.locals.dbConnected = false;
    return;
  }
  console.log('Connected to PostgreSQL database successfully');
  app.locals.dbConnected = true;
});

app.get('/', (req, res) => {
  res.send(app.locals.dbConnected ? 'Connection to database successful.' : 'Error connecting to database.');
});

const PORT = process.env.PORT || 3000;
app.listen(PORT, () => {
  console.log(`Web server is running on port ${PORT}`);
});

process.on('SIGINT', () => {
  pool.end();
  console.log('Database connection closed');
  process.exit();
});
```

## استفاده از Connection Pooling
مفهوم Connection pooling به معنای استفاده از یک مجموعه اتصالات از پیش ساخته شده به پایگاه داده است. این تکنیک باعث می‌شود به جای ایجاد و بستن مکرر اتصالات، از اتصالات موجود در مجموعه استفاده شود که کارایی را افزایش می‌دهد.


> [!TIP]
> همچنین بخوانید: [قابلیت Conneciton Pooling چیست؟](../../../../../dbaas/details/connection-pool.md)



برای فعال‌سازی این قابلیت در دیتابیس PostgreSQL بایستی پکیج `pg-pool` را با استفاده از دستور زیر در پروژه خود نصب کنید:

```
npm install pg-pool
```

در نهایت، کافیست تا در مرحله اتصال به دیتابیس یک‌سری آپشن اضافه کنید تا امکان استفاده از Connection Pooling فراهم شود:

```js
const express = require('express');
const { Pool } = require('pg');

const app = express();

const pool = new Pool({
  user: process.env.DB_USER,
  host: process.env.DB_HOST,
  database: process.env.DB_NAME,
  password: process.env.DB_PASS,
  port: process.env.DB_PORT,
  max: 20, // maximum number of connections in the pool
  idleTimeoutMillis: 30000, // how long a client is allowed to remain idle before being closed
  connectionTimeoutMillis: 2000, // how long to wait for a connection to be established
});

pool.connect((err) => {
  if (err) {
    console.error('Error connecting to PostgreSQL database:', err);
    app.locals.dbConnected = false;
    return;
  }
  console.log('Connected to PostgreSQL database successfully');
  app.locals.dbConnected = true;
});

app.get('/', (req, res) => {
  res.send(app.locals.dbConnected ? 'Connection to database successful.' : 'Error connecting to database.');
});

const PORT = process.env.PORT || 3000;
app.listen(PORT, () => {
  console.log(`Web server is running on port ${PORT}`);
});

process.on('SIGINT', () => {
  pool.end();
  console.log('Database connection closed');
  process.exit();
});
```

