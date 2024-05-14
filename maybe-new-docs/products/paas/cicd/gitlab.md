# راه‌اندازی CI/CD در برنامه با Gitlab

> [!NOTICE]
> از این پس برای کاربران جدید GitLab امکان راه‌اندازی فرایند CI/CD نیست زیرا برای استفاده از Shared runnerهای رایگان این سرویس باید حساب کاربری خود را تایید کنید. به‌همین علت توصیه می‌کنیم پروژه‌ی خود را به سرویس GitHub انتقال داده و فرایند CI/CD را با استفاده از [GitHub Actions](./github.md) راه‌اندازی کنید. 

[ویدیوی آموزشی](https://files.liara.ir/liara/CICD/cicd-gitlab.mp4)

برای راه‌اندازی CI/CD در GitLab می‌توانید از قابلیت GitLab CI استفاده کنید.
در ابتدا باید یک فایل به نام `gitlab-ci.yml.` در مسیر اصلی پروژه‌ی خود ایجاد کرده و قطعه‌کد زیر را در این فایل قرار دهید:

```yml
image: node:18-alpine

stages:
  - update-production

deploy:
  stage: update-production
  only:
    - master
  script:
    - npm i -g @liara/cli@5
    - export http_proxy=http://proxy.liara.ir:6666
    - liara deploy --app APP_NAME --api-token $TOKEN --no-app-logs
```
> [!NOTICE]
> در مثال فوق باید مقدار `APP_NAME` را با شناسه‌ی برنامه‌تان در لیارا جایگزین کنید.

> [!NOTICE]
> در صورت نیاز به تنظیم پورتی مانند `3000` که برنامه‌ی شما روی آن اجرا می‌شود لازم است پارامتر `--port=3000` را هم برای دستور `liara deploy` تنظیم کنید.

> [!NOTICE]
> توجه داشته باشید، که اگر branch پیش فرض ریپازیتوری‌تان برابر با main است، باید آن را در فایل `gitlab-ci.yml.` تغییر دهید در غیر اینصورت branch پیش فرض‌تان را برابر با master قرار دهید.

1) ایران در لیست تحریم سرویس GitLab قرار دارد بنابراین برای استقرار پروژه‌های خود در لیارا باید از پروکسی‌ای که در قطعه‌کد فوق قرار داده شده استفاده کنید.

2) اگر از سرویس Gitlab.com استفاده نمی‌کنید و GitLab را بر روی سرورهای شخصی راه‌اندازی کرده‌اید، نیازی به تنظیم پروکسی نیست.

در قطعه‌کد فوق تمام مراحل لازم برای استقرار یک پروژه در لیارا تعریف شده است. در ابتدا ابزار Liara CLI نصب شده و پس از تنظیم پروکسی برای استقرار پروژه، دستور `liara deploy` اجرا می‌شود.

پس از شخصی‌سازی مقدار `APP_NAME` در فایل `gitlab-ci.yml.` باید [کلید دسترسی به API](https://console.liara.ir/API) حساب‌تان را در بخش **Variables** تنظیمات CI/CD ریپازیتوری GitLab اضافه کنید.

برای این کار وارد تنظیمات CI/CD ریپازیتوری شوید و کمی به پایین اسکرول کنید. در بخش **Variables** بر روی گزینه‌ی *Expand* کلیک کنید تا گزینه‌ی **Add Variable** نمایش داده شود.

![gitlab-ci-cd-variables-settings](https://files.liara.ir/liara/docs/gitlab-ci-cd-variables-settings.png)

حال برای تعریف یک Variable جدید، روی گزینه‌ی **Add Variable** کلیک کنید. نام (Key) این Variable را **TOKEN** و مقدار آن را از صفحه‌ی [کلید دسترسی به API](https://console.liara.ir/API) کپی کرده و در بخش **Value** قرار دهید. پس از انتخاب گزینه‌های **Protect variable** و **Mask variable**، بر روی گزینه‌ی **Add variable** کلیک کنید.

> [!TIP]
> با انتخاب گزینه‌ی **Protect variable** متغیر TOKEN فقط در branchها و tagهای protected در دسترس خواهد بود.

![add-token-variable-in-gitlab-cicd-settings](https://files.liara.ir/liara/docs/add-token-variable-in-gitlab-cicd-settings.png)

در آخر با push کردن فایل `gitlab-ci.yml.` در ریپازیتوری GitLab متوجه خواهید شد که یک pipeline به‌صورت خودکار در منوی CI/CD ریپازیتوری شما اجرا شده است و شما نیز می‌توانید مراحل استقرار پروژه‌ی خود را در صفحه‌ی تاریخچه دنبال کنید.
