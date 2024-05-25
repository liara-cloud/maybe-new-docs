# استقرار برنامه‌های NestJS در لیارا

نرم‌افزار NestJS فریم‌ورکی برای ایجاد برنامه‌های کارآمد و مقیاس‌پذیر NodeJS  است. این برنامه  با TypeScript ساخته شده و از آن، کاملاً پشتیبانی می کند. از ویژگی‌های متمایز این فریم‌ورک می‌توان به پشتیبانی آن از OOP، برنامه‌نویسی تابعی (FP) و FRP اشاره کرد.

شما می‌توانید برنامه‌های NestJS خود را با [ایجاد برنامه‌های NodeJS](../../how-tos/create-app.md) در لیارا، مستقر کنید.




برای دیپلوی برنامه‌های NestJS باید اسکریپت `start` را در فایل `package.json` به شکل زیر، تعریف کنید:

```json
{
  "scripts": {
    "build": "nest build",
    "format": "prettier --write \"src/**/*.ts\" \"test/**/*.ts\"",
    "start": "node dist/main",
    "start:dev": "nest start --watch",
    "start:debug": "nest start --debug --watch",
    "start:prod": "node dist/main",
    "lint": "eslint \"{src,apps,libs,test}/**/*.ts\" --fix",
    "test": "jest",
    "test:watch": "jest --watch",
    "test:cov": "jest --coverage",
    "test:debug": "node --inspect-brk -r tsconfig-paths/register -r ts-node/register node_modules/.bin/jest --runInBand",
    "test:e2e": "jest --config ./test/jest-e2e.json"
  }
}
```

در ادامه، در فایل `src/main.ts` برنامه خود، باید حتماً فیلد `host` را با مقدار `'0.0.0.0'` مشخص کنید؛ به عنوان مثال:

```js
async function bootstrap() {
  const app = await NestFactory.create<NestExpressApplication>(AppModule);

  app.useStaticAssets(join(__dirname, '..', 'public'));

  await app.listen(3000, '0.0.0.0');
}
bootstrap();
```



Liara CLI

[ویدیوی آموزشی](https://files.liara.ir/liara/nestjs/nestjs.mp4)

در نهایت کافیست تا دستور زیر را اجرا کنید تا برنامه شما در لیارا مستقر شود (البته اگر که برنامه‌تان در پورت دیگری به جز 3000 به درخواست کاربران listen می‌کند، باید در دستور زیر، مقدار آن را وارد کنید):

```
liara deploy --port=3000 --platform=node
```

Liara Console



در نهایت کافیست تا برنامه خود را با کنسول و پورت 3000، در لیارا آپلود کنید و عملیات استقرار را انجام دهید تا برنامه با موفقیت در لیارا مستقر شود. البته اگر که برنامه‌تان در پورت دیگری به جز 3000 به درخواست کاربران listen می‌کند، باید مقدار آن را در فیلد پورت، وارد کنید.

> [!TIP]
> یک پروژه NestJS آماده به استقرار در [اینجا](https://github.com/liara-cloud/nestjs-getting-started) وجود دارد که می‌توانید از آن، استفاده کنید.

> [!TIP]
> همچنین بخوانید: [رفع خطای CORS در NestJS](../../how-tos/fix-common-errors/cors-error/nestjs.md)



