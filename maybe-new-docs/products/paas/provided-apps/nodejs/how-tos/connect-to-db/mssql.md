# اتصال به دیتابیس MSSQL در برنامه‌های NodeJS

[ویدیوی آموزشی](https://files.liara.ir/liara/nodejs/nodejs-postgres.mp4)

برای اتصال به دیتابیس SQL Server، می‌توانید از پکیج `mssql` استفاده کنید، برای نصب این پکیج، کافیست تا در مسیر اصلی پروژه خود، دستور زیر را اجرا کنید:


```
npm install mssql
```

پس از آن، کافیست تا طبق [مستندات تنظیم متغیرهای محیطی](../../../../details/envs.md) اطلاعات مربوط به دیتابیس خود را وارد کنید. به عنوان مثال:

```
DB_HOST=mssql
DB_PORT=1433
DB_USER=sa
DB_PASS=4OTujub1cJeXYuRlgNFe5pHz
DB_NAME=master
```

و در برنامه به این صورت اطلاعات را خوانده و به دیتابیس متصل شوید:

```js
const express = require('express');
const sql = require('mssql');

const app = express();

const DB_HOST= process.env.DB_HOST;
const DB_PORT= process.env.DB_PORT;
const DB_USER= process.env.DB_USER;
const DB_PASS= process.env.DB_PASS;
const DB_NAME= process.env.DB_NAME;

async function connectAndQuery() {
  try {
    
    await sql.connect(`Server=${DB_HOST},${DB_PORT};Database=${DB_NAME};User Id=${DB_USER};Password=${DB_PASS};Encrypt=false`)
        
    await sql.query`CREATE TABLE test_table (id INT IDENTITY(1,1) PRIMARY KEY, name NVARCHAR(50));`;
    await sql.query`INSERT INTO test_table (name) VALUES ('Sample Record');`;

    const result = await sql.query`SELECT * FROM test_table`;
    return result.recordset;
  } catch (err) {
    console.error('Error executing SQL query:', err);
    throw err;
  } finally {
    await sql.close();
  }
}

app.get('/', async (req, res) => {
  try {
    
    const data = await connectAndQuery();
    res.send(data);
  } catch (err) {
    res.status(500).send('Error retrieving data from database.');
  }
});

const PORT = process.env.PORT || 3000;
app.listen(PORT, () => {
  console.log(`Web server is running on port ${PORT}`);
});
```

## استفاده از Connection Pooling
شما می‌توانید در برنامه NodeJS خود، قابلیت Connection Pooling را نیز فعال کنید. در Connection Pooling برنامه به جای ایجاد یک ارتباط (Connection) جدید برای انجام عملیات دیتابیسی و بستن آن پس از پایان عملیات، از ارتباط‌هایی که قبلاً ایجاد شده‌اند، استفاده می‌کند.

استفاده از Connection Pooling کارایی برنامه را افزایش می‌دهد و تاثیر بسیار زیادی در بهینه‌سازی و کاهش منابع مورد استفاده برنامه و دیتابیس دارد. بنابراین توصیه می‌شود که حتماً در حالت Production، از این قابلیت، استفاده کنید. 

برای استفاده از این قابلیت در دیتابیس MSSQL نیاز به انجام کار خاصی نیست؛ چرا که پکیج `mssql` به صورت خودکار، از این قابلیت، پشتیبانی می‌کند. صرفاً کافیست تا فیلد `pool` را به شکل زیر، به قطعه کد اتصال به دیتابیس، اضافه کنید:

```js
const sql = require('mssql');

const appPool = new sql.ConnectionPool({
    server: process.env.DB_HOST,
    port: process.env.DB_PORT,
    user: process.env.DB_USER,
    password: process.env.DB_PASS,
    database: process.env.DB_NAME,
    pool: {
        min: 10,
        max: 100,
        acquireTimeoutMillis: 15000,
    },
    options: {
      encrypt: false,
      trustServerCertificate: false
  }
});

appPool.connect().then(pool => {
    console.log(`SERVER: Connected to the db and ${pool.available} connections are available!`);
});
```

