# استقرار برنامه‌های SvelteKit در لیارا

نرم‌افزار [SvelteKit](https://svelte.dev/) یک متافریم‌ورک front-end و متن باز جاوااسکریپت است که بر پایه [فریم‌ورک Svelte](../svelte/starter.md) توسعه داده شده است و شامل ویژگی‌ها و قابلیت‌های بیشتری است. شما می‌توانید برنامه‌های SvelteKit خود را با [ایجاد برنامه‌های NodeJS](../../how-tos/create-app.md) در لیارا، مستقر کنید.


برای استقرار این برنامه در لیارا، فقط کافیست تا پکیج `adapter-node` را با اجرای دستور زیر در مسیر اصلی پروژه خود، نصب کنید:

```
npm install @sveltejs/adapter-node
```

در ادامه، باید در ابتدای فایل `svelte.config.js` قطعه کد زیر را، قرار دهید:

```js
import adapter from '@sveltejs/adapter-node';
```

همچنین، بایستی اسکریپت `start` را به شکل زیر، به فایل `package.json` خود، اضافه کنید:

```json
"scripts": {
		"dev": "vite dev",
		"build": "vite build",
		"preview": "vite preview",
		"start": "node build/index.js"
	},
```


