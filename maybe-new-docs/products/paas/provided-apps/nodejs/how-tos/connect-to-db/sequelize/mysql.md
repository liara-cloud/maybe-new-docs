# اتصال به دیتابیس MySQL با استفاده از Sequelize

در ابتدا، بایستی ماژول مربوط به `sequelize` و دیتابیس `MySQL` را با اجرای دستور زیر، نصب کنید:

```
npm install --save sequelize mysql2 
```

پس از آن، کافیست تا طبق [مستندات تنظیم متغیرهای محیطی](../../../../details/envs.md) اطلاعات مربوط به دیتابیس خود را وارد کنید. به عنوان مثال:

```
MYSQL_URI=mysql://root:lnhJ9VUqkaa86ynCtopRaqOU@maria:3306/jovial_chebyshev
```

در نهایت، می‌توانید با استفاده از قطعه کد زیر، به دیتابیس خود، متصل شوید:

```js
const express = require('express');
const { Sequelize } = require('sequelize');

const app = express();
const sequelize = new Sequelize(process.env.MYSQL_URI, {
  logging: (...msg) => console.log(msg),
}); 

async function testDatabaseConnection() {
  try {
    await sequelize.authenticate();
    return 'Connection has been established successfully.';
  } catch (error) {
    return `Unable to connect to the database: ${error}`;
  }
}

app.get('/', async (req, res) => {
  const result = await testDatabaseConnection();
  res.send(result);
});

app.listen(3000, () => {
  console.log('Server is running on port 3000');
});
```

## قابلیت Connection Pooling

در قطعه کد ارائه شده، قابلیت Conneciton Pooling، تعبیه شده است؛ البته اگر که قصد دارید تا مقادیر آن را خودتان پیکربندی کنید، کافیست که قطعه کد زیر را به برنامه خود، اضافه کنید:

```js
const sequelize = new Sequelize(/* database confs */, {
  // other confs
  pool: {
    max: 5,
    min: 0,
    acquire: 30000,
    idle: 10000
  }
});
```

## لاگ‌گیری دیتابیس

در کد ارائه داده شده، با استفاده از قطعه کد زیر، لاگ‌های کامل مربوط به دیتابیس، در بخش [گزارشات نرم‌افزار برنامه](../../../../../details/observations/software.md)، به شما نمایش داده می‌شوند:

```js
logging: (...msg) => console.log(msg),
```

اگر که قصد دارید از loggerهای دیگری مثل `Winston` استفاده کنید؛ کافیست تا کد زیر را جایگزین قطعه کد فوق، کنید:

```js
logging: msg => logger.debug(msg),
```






