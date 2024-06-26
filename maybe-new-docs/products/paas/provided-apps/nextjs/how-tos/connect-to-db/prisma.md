# اتصال به دیتابیس با استفاده از Prisma در برنامه‌های NodeJS 

[ویدیوی آموزشی](https://files.liara.ir/liara/prisma/prisma.mp4)

> [!NOTE]
> پروژه و کدهای مورد استفاده در ویدیوی فوق در [اینجا](https://github.com/liara-cloud/nodejs-getting-started/tree/prisma) قابل مشاهده و دسترسی هستند.


نرم‌افزار [Prisma](https://www.prisma.io/) یک ORM برای NodeJS و زبان TypeScript است که بسیاری از مشکل‌های ORMهای دیگر در آن برطرف شده و به شما امکان اتصال و اجرای کوئری بر روی دیتابیس‌های PostgreSQL, MySQL, SQL Server, SQLite و MongoDB را می‌دهد.

برای استفاده از این ORM، در ابتدا باید با اجرای دستور زیر در خط فرمان سیستم خود، فایل‌های Migration را ایجاد کنید:

```
npx prisma migrate dev --name init --create-only
```

سپس، باید در بخش `scripts` فایل `package.json`، اسکریپت `build` را به‌صورت زیر بنویسید:

```json
"scripts": {
  "build": "npx prisma generate",
},
```

اگر که از قبل اسکریپت `build` را تعریف کرده بودید؛ می‌توانید با استفاده از عملگر `&&` دستور مربوط به Prisma را نیز، به اسکریپت خود اضافه کنید، به عنوان مثال:

```json
"scripts": {
  "build": "npx prisma generate && webpack --watch"
},
```

همچنین باید متغیر `DATABASE_URL` را طبق مستندات [تنظیم متغیرهای محیطی در برنامه](../../../../details/envs.md)، به برنامه خود اضافه کنید.

```
DATABASE_URL=postgresql://USERNAME:PASSWORD@HOST:PORT/postgres
```

> [!TIP]
> اگر که از [دیتابیس لیارا](../../../../../dbaas/about.md) استفاده می‌کنید؛ توصیه می‌شود هم برنامه و هم دیتابیس را در یک [شبکه خصوصی](../../../../details/private-networks.md) قرار دهید و برای ایجاد اتصال ایمن و مطمئن از URI شبکه خصوصی دیتابیس استفاده کنید.


در ادامه، باید طبق [مستندات مربوط به ایجاد دیسک](../../../../disks/manage/create.md)، یک دیسک به نام `prisma` (یا هر نام دلخواه دیگری) در برنامه خود ایجاد کنید.

Liara CLI

یک فایل `liara.json` در مسیر اصلی پروژه خود ایجاد کنید، و با استفاده از قطعه کد زیر، آن دیسک را به برنامه متصل کنید:

```json
{
    "disks":[
        {
            "name": "prisma",
            "mountTo": "prisma/migrations"
        }
    ]
}
```

> [!IMPORTANT]
> در نظر داشته باشید که باید در فیلد `name` نام دیسکی که ایجاد کرده‌اید را وارد کنید.

درنهایت می‌توانید با اجرای دستور `liara deploy`، پروژه‌ی خود را در لیارا مستقر کنید.

Liara Console

در ابتدا کافیست تا پروژه را در Console مستقر کنید و در مرحله اتصال دیسک به برنامه، مسیر دیسک را `prisma/migrations` قرار دهید. و ادامه فرایند استقرار را جلو ببرید.

پس از استقرار موفق پروژه می‌توانید دستور زیر را برای اعمال Migrationها در خط فرمان سیستم خود اجرا کنید:

```
liara shell -c "npx prisma migrate deploy"
```

> [!NOTE]
> اگر که در اجرای دستور فوق با خطای `ReadOnly` مواجه شدید کافیست تا [فایل‌سیستم ReadObly](../../../../details/file-system.md) برنامه خود را موقتاً به `Writable` تغییر داده، دستور را اجرا کرده و سپس آن را مجدداً به حالت قبلی، برگردانید.
