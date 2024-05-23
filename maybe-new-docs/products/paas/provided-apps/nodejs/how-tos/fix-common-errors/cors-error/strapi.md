# رفع خطای CORS در فریم‌ورک Strapi

برای رفع خطای CORS در Strapi، باید تنظیمات مربوط به CORS را در فایل `config/middleware.js` به برنامه خود اضافه کنید:

```js
module.exports = ({ env }) => ({
  settings: {
    cors: {
      enabled: true,
      origin: '*', 
    },
  },
});
```



