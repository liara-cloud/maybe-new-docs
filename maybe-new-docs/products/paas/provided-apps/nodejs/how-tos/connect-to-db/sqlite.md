# اتصال به دیتابیس SQLite در برنامه‌های NodeJS

[ویدیوی آموزشی](https://files.liara.ir/liara/nodejs/nodejs-sqlite.mp4)

برای اتصال موفق به دیتابیس SQLite در برنامه‌های NodeJS کافیست تا گام‌های زیر را طی کنید:

1) با اجرای دستور زیر، ماژول مورد نیاز SQLite را نصب کنید:

```
npm install sqlite3
```

2) در مسیر اصلی پروژه، یک دایرکتوری به نام `db` (یا هر نام دلخواه دیگری) ایجاد کنید.

3) از قطعه کد زیر در برنامه اصلی خود، استفاده کنید (می‌توانید آن را با توجه به نیاز خود، تغییر دهید):

```
const express = require('express');
const sqlite3 = require('sqlite3').verbose();

const app = express();
const db = new sqlite3.Database('db/database.db');

db.serialize(() => {
  db.run("CREATE TABLE IF NOT EXISTS lorem (info TEXT)");
  const stmt = db.prepare("INSERT INTO lorem VALUES (?)");
  for (let i = 0; i < 10; i++) {
      stmt.run("Ipsum " + i);
  }
  stmt.finalize();
});

app.once('started', () => {
  app.get('/', async (req, res) => {
    db.all("SELECT rowid AS id, info FROM lorem", (err, rows) => {
      if (err) {
        return res.status(500).send('Error retrieving data from database');
      }
      res.send(rows.map(row => `${row.id}: ${row.info}`).join('<br>'));
    });
  });
});

app.listen(3000, () => {
  console.log('Server is running on port 3000');
  app.emit('started');
});
```
4) در بخش [**دیسک‌ها**](../../../../disks/about.md) برنامه خود در لیارا، یک دیسک جدید با نام `database` و اندازه مدنظرتان ایجاد کنید.

5) در مسیر اصلی پروژه، یک فایل به نام `liara.json` ایجاد کنید و قطعه کد زیر را، درون آن، قرار دهید:

```
{
    "disks": [
        {
            "name": "database",
            "mountTo": "db"
        }
    ]
}
```

6) برنامه خود را با استفاده از دستور `liara deploy` در لیارا مستقر کنید.


