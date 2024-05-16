# اتصال به دیتابیس MariaDB/MySQL در برنامه‌های NodeJS

[ویدیوی آموزشی](https://files.liara.ir/liara/nodejs/nodejs-mysql.mp4)

روش‌ها و کتابخانه‌های مختلفی برای اتصال به MySQL/MariaDB در NodeJS وجود دارد. یکی از این راه‌ها، استفاده از پکیج `mysql` است که در ابتدا، با اجرای دستور زیر، باید آن را نصب کنید:

```
npm install mysql
```

پس از آن، کافیست تا طبق [مستندات تنظیم متغیرهای محیطی](../../../../details/envs.md) اطلاعات مربوط به دیتابیس خود را وارد کنید. به عنوان مثال:

```
DB_HOST=olympus.liara.cloud
DB_PORT=34008
DB_NAME=festive_villani
DB_USER=root
DB_PASS=d0NRg5Rh7TzbAzJVl7SyeMAB
```

و در برنامه به این صورت اطلاعات را خوانده و به دیتابیس متصل شوید:

```js
const express = require('express');
const mysql = require('mysql');

const app = express();

const dbConfig = {
  host: process.env.DB_HOST,
  port: process.env.DB_PORT,
  database: process.env.DB_NAME,
  user: process.env.DB_USER,
  password: process.env.DB_PASS
};
const connection = mysql.createConnection(dbConfig);

connection.connect((err) => {
  if (err) {
    console.error('error in connecting to db:', err);
    app.locals.dbConnected = false;
    return;
  }

  console.log('connected to db successfully');
  app.locals.dbConnected = true;
});

app.get('/', (req, res) => {
  res.send(app.locals.dbConnected ? 'connected to db successfully' : 'error in connecting to db!');
});

const PORT = process.env.PORT || 3000;
app.listen(PORT, () => {
  console.log(`server is listening on port ${PORT}`);
});

process.on('SIGINT', () => {
  connection.end();
  console.log('db disconnected.');
  process.exit();
});
```

## استفاده از Connection Pooling
شما می‌توانید در برنامه NodeJS خود، قابلیت Connection Pooling را نیز فعال کنید. در Connection Pooling برنامه به جای ایجاد یک ارتباط (Connection) جدید برای انجام عملیات دیتابیسی و بستن آن پس از پایان عملیات، از ارتباط‌هایی که قبلاً ایجاد شده‌اند، استفاده می‌کند.

استفاده از Connection Pooling کارایی برنامه را افزایش می‌دهد و تاثیر بسیار زیادی در بهینه‌سازی و کاهش منابع مورد استفاده برنامه و دیتابیس دارد. بنابراین توصیه می‌شود که حتماً در حالت Production، از این قابلیت، استفاده کنید. 

برای فعال‌سازی این قابلیت در دیتابیس MySQL یا MariaDB کافیست تا پکیج mysql2 را با استفاده از دستور زیر در پروژه خود نصب کنید:

```
npm install mysql2 
```

سپس، می‌توانید مانند قطعه کد زیر، از Connection Pooling استفاده کنید:

```js
const mysql = require('mysql2/promise');

const dbConfig = {
  host: process.env.DB_HOST,
  port: process.env.DB_PORT,
  database: process.env.DB_NAME,
  user: process.env.DB_USER,
  password: process.env.DB_PASS,
  waitForConnections: true,
  connectionLimit: 10,
  queueLimit: 0,
};
const pool = mysql.createPool(dbConfig);


(async () => {
  try {
    const connection = await pool.getConnection();
    console.log('Connected to the MySQL server.');
    connection.release();
  } catch (error) {
    console.error('Error connecting to the MySQL server:', error);
  }
})();
```
