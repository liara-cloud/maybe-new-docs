# استقرار برنامه در لیارا

برای استقرار موفق برنامه خود در لیارا، پس از ساخت برنامه؛ کافیست تا مراحل زیر را طی کنید:

Liara CLI

در مسیر اصلی پروژه، یک فایل به نام `liaraignore.` یا `gitignore.` ایجاد کنید و درون آن، اسامی تمامی فایل‌ها یا پوشه‌هایی که قصد ندارید در لیارا آپلود شوند را، وارد کنید؛ به عنوان مثال، نیازی به آپلود دایرکتوری `node_modules` به همراه محتوای آن نیست؛ چرا که لیارا در حین استقرار برنامه، آن را برای شما می‌سازد؛ پس بایستی نام این دایرکتوری در یکی از دو فایل فوق، نوشته شود؛ قطعه کد قرار گرفته در لینک زیر، یک `gitignore.` عالی برای برنامه‌های NodeJS است که می‌توانید از آن، استفاده کنید:

[نمونه فایل `gitignore.` برای برنامه‌های NodeJS](https://github.com/liara-cloud/gitignore-templates/blob/master/nodejs/.gitignore)

همچنین، پروژه شما باید شامل فایل `package.json` باشد؛ لیارا، در حین فرایند استقرار، به صورت خودکار این فایل را پیدا می‌کند و عملیات زیر را انجام می‌دهد:

- لیارا، تمامی ماژول‌ها و وابستگی‌های برنامه که در فیلدهای `dependencies` و `devDependencies` قرار گرفته‌اند را با استفاده از دستور `npm install` نصب می‌کند.

- اگر که در این فایل، اسکریپت `build` را تعریف کرده باشید، لیارا با اجرای دستور `npm run build` آن را، اجرا می‌کند.

- لیارا برای اجرای برنامه، از اسکریپت `start` استفاده می‌کند؛ پس باید حتماً این اسکریپت را در فایل `package.json` تعریف کنید. 

قطعه کد زیر، یک نمونه از فایل `package.json` است که در آن اسکریپت `start` تعریف شده است:

```json
{
  "name": "node-getting-started",
  "version": "1.0.0",
  "description": "simple web server with expressjs",
  "main": "server.js",
  "scripts": {
    "start": "node server.js"
  },
  "license": "ISC",
  "dependencies": {
    "express": "^4.17.1"
  },
  "devDependencies": {
    "eslint": "^8",
    "eslint-config-next": "14.0.4"
  }
}
```
در مسیر اصلی پروژه، یک فایل به نام `liara.json` ایجاد کنید و قطعه کد زیر را در آن قرار دهید.

```json
{
    "app": "my-web-app", 
    "port": 3005,
    "platform": "node",
}
```

در قطعه کد فوق، در فیلد `app` باید شناسه برنامه خود را به جای `my-web-app` وارد کنید و در فیلد `port` باید پورتی را وارد کنید که برنامه‌تان در آن، به درخواست کاربران گوش می‌دهد. فیلد `platform` هم پلتفرم برنامه شما را که `NodeJS` است، مشخص می‌کند.

لیارا در جهت استقرار سریع‌تر،  برای نصب پکیج‌های `npm`، از `mirror` اختصاصی خود استفاده می‌کند؛ از همین رو، ممکن است که در نصب برخی از پکیج‌های جدید، دچار مشکل شود. برای رفع این مشکل، می‌توانید mirror لیارا را با قراردادن قطعه کد زیر در فایل `liara.json`، غیرفعال کنید:

```json
{
    "node": {
    "mirror": false
  }
}
```

برای تعیین نسخه NodeJS برنامه خود نیز، کافیست تا در فایل `liara.json` فیلد `version` را قرار دهید:

```json
{
    "node": {
    "version": "22"
  }
}
```

> [!TIP]
> همچنین بخوانید: [نسخه‌های قابل ارائه NodeJS در لیارا](./choose-version.md)

به صورت پیش‌فرض، منطقه‌ی زمانی برنامه‌تان بر روی Asia/Tehran تنظیم شده است؛ برای تغییر مقدار پیش‌فرض، می‌توانید از پارامتر `timezone` در فایل `liara.json` استفاده کنید؛ به عنوان مثال:

```json
{
    "node": {
    "timezone": "America/Los_Angeles"
  }
}
```
در نهایت، یک فایل `liara.json` می‌تواند مانند قطعه کد زیر باشد:

```json
{
    "app": "my-web-app", 
    "port": 3005,
    "platform": "node",
    "node": {
    "mirror": false,
    "timezone": "America/Los_Angeles",
    "version": "22"
    }
}
```

> [!TIP]
> همچنین بخوانید: [تعیین موقعیت build برنامه](../../../details/build-location.md)

اکنون کافیست تا در مسیر اصلی پروژه و جایی که فایل `liara.json` قرار دارد؛ دستور زیر را اجرا کنید و منتظر بمانید تا عملیات استقرار تمام شود:

```
liara deploy
```

در حین فرایند استقرار، می‌توانید در ترمینال خود، لاگ‌های مربوط به آن را مشاهده بفرمایید.

Liara Console

در ابتدا، بایستی تمام فایل‌ها و پوشه‌هایی که قصد ندارید در لیارا آپلود شوند را، از پروژه پاک کنید. به عنوان مثال، باید پوشه `node_modules` را در پروژه خود پاک کنید؛ چرا که لیارا در حین فرایند استقرار، آن را برای شما ایجاد خواهد کرد. به صورت کلی، اگر که در پروژه خود فایلی به نام `gitignore.` دارید، کافیست تا فایل‌های و دایرکتوری‌های اشاره شده در این فایل را، از پروژه خود پاک کنید.

همچنین، پروژه شما باید شامل فایل `package.json` باشد؛ لیارا، در حین فرایند استقرار، به صورت خودکار این فایل را پیدا می‌کند و عملیات زیر را انجام می‌دهد:

- لیارا، تمامی ماژول‌ها و وابستگی‌های برنامه که در فیلدهای `dependencies` و `devDependencies` قرار گرفته‌اند را با استفاده از دستور `npm install` نصب می‌کند.

- اگر که در این فایل، اسکریپت `build` را تعریف کرده باشید، لیارا با اجرای دستور `npm run build` آن را، اجرا می‌کند.

- لیارا برای اجرای برنامه، از اسکریپت `start` استفاده می‌کند؛ پس باید حتماً این اسکریپت را در فایل `package.json` تعریف کنید. 

قطعه کد زیر، یک نمونه از فایل `package.json` است که در آن اسکریپت `start` تعریف شده است:

```json
{
    "name": "node-getting-started",
  "version": "1.0.0",
  "description": "simple web server with expressjs",
  "main": "server.js",
  "scripts": {
      "start": "node server.js"
  },
  "license": "ISC",
  "dependencies": {
      "express": "^4.17.1"
  },
  "devDependencies": {
      "eslint": "^8",
    "eslint-config-next": "14.0.4"
  }
}
```

در ادامه، بایستی پوشه پروژه خود را درون یک فایل `zip` قرار بدهید؛ سپس در برنامه خود، بر روی گزینه **استقرار جدید** کلیک کرده؛ فایل `zip` را بکشید و در باکس آپلود تعبیه شده، رها کنید تا وارد مرحله بعدی استقرار شوید.

![deploy app using console](https://files.liara.ir/liara/docs/deploy-app-using-console.gif)

پس از آپلود پروژه، باید شخصی‌سازی‌های پروژه نظیر انتخاب نسخه، تعیین منطقه زمانی، اتصال دیسک و ... را در برنامه خود، لحاظ کنید:

![build nodejs app on liara](https://files.liara.ir/liara/docs/build-nodejs-app-on-liara.gif)

در انتها، به صورت مستقیم به [صفحه تاریخچه برنامه](../../../details/private-registery.md) هدایت می‌شوید که می‌توانید لاگ‌های مربوط به استقرار را در آن، مشاهده بفرمایید. 

پس از استقرار می‌توانید [رویدادها](../../../details/events.md) و [گزارشات](../../../details/observations/about.md) مربوط به برنامه را بررسی کنید.

[گام بعدی - استفاده از متغیرهای محیطی](./set-envs.md)