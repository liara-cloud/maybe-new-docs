# استقرار برنامه‌های Strapi با SQLite در لیارا

[ویدیوی آموزشی](https://files.liara.ir/liara/strapi/strapi-sqlite.mp4)

> [!TIP]
> پروژه و کدهای مورد استفاده در ویدیوی فوق در [اینجا](https://github.com/liara-cloud/strapi-getting-started/tree/master) قابل مشاهده و دسترسی هستند.

پس از ایجاد برنامه NodeJS، باید متغیرهای محیطی موجود در فایل `env.` برنامه Strapi خود را طبق [مستندات اضافه کردن متغیرهای محیطی ](../../../../details/envs.md) به برنامه NodeJS اضافه کنید. به عنوان مثال، متغیرهای زیر با مقادیر فرضی زیر، حتماً باید به برنامه اضافه شوند:

```
APP_KEYS=qDSFzezRjBb9CWRgYTNKAQ==,a425msyZKCQLciHemU5XjA==,nRmH9IqqkiKahdd9wE+AXg==,szU9KUlV56pzOVDYynbdKA==
API_TOKEN_SALT=KHPr2aDzbEhFe56iBRLa6w==
ADMIN_JWT_SECRET=thazfV/lEGoPPZAqlJGsJg==
TRANSFER_TOKEN_SALT=X7FvBRJ+T4ddz3yM2ZMv8g==
# Database
DATABASE_CLIENT=sqlite
DATABASE_FILENAME=.tmp/data.db
JWT_SECRET=qvyu4YsbaS03suqri3sZVQ==
```

برای دیپلوی برنامه‌های Strapi نیازی به تغییر فایل `package.json` نیست و لیارا به صورت کامل از این CMS، پشتیبانی می‌کند. در ادامه یک مثال از اسکریپت‌های استاندارد این فریم‌ورک در فایل `package.json` آمده است:

```json
{
    "scripts": {
        "develop": "strapi develop",
        "start": "strapi start",
        "build": "strapi build",
        "strapi": "strapi"
    }
}
```

و از آنجایی که [فایل سیستم لیارا](../../../../details/file-system.md) به صورت پیش‌فرض ReadOnly است؛ پس بایستی برای اتصال موفق به دیتابیس SQLite و ذخیره media، طبق مستندات [دیسک‌ها](../../../../disks/about.md)، دو دیسک ایجاد و آن‌ها را به آدرس‌های `app/.tmp/` و `app/public/uploads/` متصل کنید.


Liara CLI

در نهایت کافیست تا دستور زیر را اجرا کنید تا برنامه شما در لیارا مستقر شود :

```
liara deploy --port=3000 --platform=node
```

Liara Console



در نهایت کافیست تا برنامه خود را با کنسول و پورت 3000، در لیارا آپلود کنید و عملیات استقرار را انجام دهید تا برنامه با موفقیت در لیارا مستقر شود. 

