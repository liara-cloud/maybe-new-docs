# اتصال به دیتابیس SQLite با استفاده از Sequelize

برای اتصال اصولی به دیتابیس [SQLite](https://www.sqlite.org/)، بایستی گام‌های زیر را به ترتیب انجام دهید تا برنامه با موفقیت در لیارا، مستقر شود:

1) ماژول مربوط به دیتابیس SQLite را با اجرای دستور زیر، نصب کنید:

```
npm install --save sqlite3
```

2) سپس، با استفاده از قطعه کد زیر، به دیتابیس SQLite متصل شوید (حتماً دیتابیس باید درون یک دایرکتوری باشد، می‌توانید نام دایرکتوری و دیتابیس را در قطعه کد زیر، تغییر دهید):

```js
const express = require('express');
const { Sequelize } = require('sequelize');

const app = express();
const sequelize = new Sequelize({
    dialect: 'sqlite',
    storage: 'db/database.sqlite',
    logging: (...msg) => console.log(msg),
    pool: {
        max: 5,
        min: 0,
        acquire: 30000,
        idle: 10000
    }
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

3) طبق [مستندات ایجاد دیسک](../../../../../disks/manage/create.md) در برنامه NodeJS لیارا، یک دیسک جدید به نام `database` و با اندازه مدنظر خود، ایجاد کنید.

Liara CLI 

4) در مسیر اصلی پروژه، یک فایل به نام `liara.json` ایجاد کنید و قطعه کد زیر را درون آن، قرار دهید (مقدار فیلدها با توجه به نام دیسک و دایرکتوری دیتابیس شما، متفاوت است):

```json
{
    "disks": [
        {
            "name": "database",
            "mountTo": "db"
        }
    ]
}
```

5) با اجرای دستور `liara deploy` برنامه خود را در لیارا مستقر کنید.

Liara Console

4) برنامه خود را در Console مستقر کنید و در مرحله اتصال دیسک به برنامه، مسیر دایرکتوری دیتابیس خود را به صورت نسبی مشخص کنید.

5) ادامه فرایند استقرار را با توجه به نیازهای پروژه خود، جلو ببرید.
