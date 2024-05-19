# استقرار برنامه‌های NuxtJS در لیارا

NuxtJS یک فریم‌ورک مبتنی بر Vue است که بسیاری از قابلیت‌ها مانند SSR را برای شما به ارمغان می‌آورد. شما می‌توانید برنامه‌های NuxtJS خود را با [ایجاد برنامه‌های NodeJS ](../../how-tos/create-app.md)بر روی لیارا دیپلوی کنید.

توجه داشته باشید که برای دیپلوی برنامه‌های **Nuxt 2** نیازی به ایجاد تغییر در فایل `package.json `نیست و لیارا به‌طور کامل از این فریم‌ورک پشتیبانی می‌کند بنابراین تغییری در بخش `scripts` ایجاد نکنید؛ به عنوان مثال:

```json
"scripts": {
    "dev": "nuxt",
    "build": "nuxt build",
    "start": "nuxt start"
},
```

ولی برای دیپلوی برنامه‌های **Nuxt 3** باید در بخش `scripts` فایل `package.json` مقدار `start` را به شکل زیر تغییر دهید:

```json
"scripts": {
    "start": "node .output/server/index.mjs"
},
```

Liara CLI

[ویدیوی آموزشی](https://files.liara.ir/liara/nuxtjs/nuxtjs-cli.mp4)

در نهایت کافیست تا دستور زیر را اجرا کنید تا برنامه شما در لیارا مستقر شود:

```
liara deploy --port=3000 --platform=node
```

Liara Console

[ویدیوی آموزشی](https://files.liara.ir/liara/nuxtjs/nuxtjs-desktop.mp4)

در نهایت کافیست تا برنامه خود را با کنسول، در لیارا آپلود کنید و عملیات استقرار را انجام دهید تا برنامه با موفقیت در لیارا مستقر شود.

> [!TIP]
> همچنین بخوانید: [استفاده از قابلیت Static Site Generation در برنامه‌های NuxtJS]()

> [!TIP]
> همچنین بخوانید [رفع خطای CORS در برنامه‌های NuxtJS](../../how-tos/fix-common-errors/cors-error/nuxtjs.md)