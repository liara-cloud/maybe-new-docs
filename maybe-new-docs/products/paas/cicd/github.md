# راه‌اندازی CI/CD در برنامه با Github

[ویدیوی آموزشی](https://files.liara.ir/liara/CICD/cicd-github.mp4)

برای راه‌اندازی CI/CD در [GitHub](https://github.com) می‌توانید از قابلیت [GitHub Actions](https://github.com/features/actions) استفاده کنید؛ فقط کافیست تا مراحل زیر را دنبال کنید:

در ابتدا یک پوشه به نام `github.` در مسیر اصلی پروژه‌‌تان ایجاد کرده و سپس پوشه‌ی دیگری به نام `workflows` داخل آن ایجاد کنید تا مسیر `github/workflows.` در پروژه‌ی شما قرار بگیرد. در این مسیر می‌توان Actionهای مورد نظر را تعریف کرد بنابراین یک فایل با نام `liara.yaml` را در این مسیر ایجاد کرده و قطعه‌کد زیر را در این فایل قرار دهید:

```yaml
name: CD-Liara
on:
  push:
    branches:
      - main
jobs:
  deploy:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v3
      - uses: actions/setup-node@v3
        with:
          node-version: "18"
      - name: update-liara
        env:
          LIARA_TOKEN: ${{ secrets.LIARA_API_TOKEN }}
        run: |
          npm i -g @liara/cli@5
          liara deploy --app="APP_NAME" --api-token="$LIARA_TOKEN" --no-app-logs
```
> [!NOTICE]
> در مثال فوق باید مقدار `APP_NAME` را با شناسه‌ی برنامه‌تان در لیارا جایگزین کنید.


> [!NOTICE]
> در صورت نیاز به تنظیم پورتی مانند `3000` که برنامه‌ی شما روی آن اجرا می‌شود لازم است پارامتر `--port=3000` را هم برای دستور `liara deploy` تنظیم کنید.

> [!NOTICE]
> توجه داشته باشید، که اگر branch پیش فرض ریپازیتوری‌تان برابر با main است، باید آن را در فایل `liara.yaml` تغییر دهید در غیر اینصورت branch پیش فرض‌تان را برابر با master قرار دهید.

> [!NOTICE]
> ریپازیتوری [liara-cloud/github-cd-example](https://github.com/liara-cloud/github-cd-example) شامل یک نمونه برنامه‌ی کامل است که می‌تواند از طریق CI/CD در لیارا مستقر شود.

در قطعه‌کد فوق تمام مراحل لازم برای استقرار یک پروژه در لیارا تعریف شده است. در ابتدا ابزار Liara CLI نصب شده و سپس دستور `liara deploy` اجرا می‌شود. 

پس از شخصی‌سازی مقدار `APP_NAME` در فایل `liara.yaml` باید کلید دسترسی به API حساب‌تان را در بخش Secret تنظیمات ریپازیتوری GitHub اضافه کنید.

برای این کار وارد تنظیمات ریپازیتوری شوید و از منوی Secrets، روی گزینه‌ی Actions کلیک کنید.
![github actions secret settings](https://files.liara.ir/liara/docs/github-action-secrets-setting.png)

سپس برای تعریف یک Secret جدید، روی گزینه‌ی **New repository secret** کلیک کنید. نام این Secret را **LIARA_API_TOKEN** و مقدار آن را از صفحه‌ی کلید [دسترسی به API](https://console.liara.ir/API) کپی کرده و در بخش Value قرار داده و بر روی گزینه‌ی **Add secret** کلیک کنید.

![add-new-secret-in-github-action](https://files.liara.ir/liara/docs/add-new-secret-in-github-action.png)

در آخر با push کردن فایل `github/workflows/liara.yaml.` در ریپازیتوری GitHub متوجه خواهید شد که یک pipeline به‌صورت خودکار در تب Action ریپازیتوری شما اجرا شده است و شما نیز می‌توانید مراحل استقرار پروژه‌ی خود را در صفحه‌ی تاریخچه دنبال کنید.

## راه‌اندازی CI/CD به ازای هر branch

در برخی موارد، لازم است چند محیط جداگانه برای برنامه‌تان ایجاد کنید. به عنوان مثال ممکن است برنامه‌تان در سه محیط test, staging و production قرارداشته باشد و بخواهید برای هر محیط یک CI/CD جدا راه‌اندازی کنید. در این صورت، ابتدا باید سه برنامه و سه branch جدا ایجاد کنید. در این حالت، هر branch به یک برنامه اشاره دارد. سپس می‌بایست فایل `github/workflows/liara.yaml.` را به شکل زیر بنویسید:

```yaml
name: CD-Liara

on:
  push:
    branches:
      - main
      - test
      - staging

jobs:
  deploy:
    runs-on: ubuntu-latest

    steps:
      - uses: actions/checkout@v3
      - uses: actions/setup-node@v3
        with:
          node-version: "18"

      - name: Install Liara CLI
        run: npm i -g @liara/cli@5

      - name: Deploy
        env:
          LIARA_TOKEN: ${{ secrets.LIARA_API_TOKEN }}
        run: |
          if [ ${{ github.ref }} == 'refs/heads/master' ]; then
            liara deploy --app="my-production-app" --api-token="$LIARA_TOKEN" --no-app-logs
          elif [ ${{ github.ref }} == 'refs/heads/test' ]; then
            liara deploy --app="my-test-app" --api-token="$LIARA_TOKEN" --no-app-logs
          elif [ ${{ github.ref }} == 'refs/heads/staging' ]; then
            liara deploy --app="my-staging-app" --api-token="$LIARA_TOKEN" --no-app-logs
          fi
```

توجه داشته باشید که در این مثال، branch های master, test و staging به ترتیب برای محیط‌های production, test و staging انتخاب‌شده اند. شما می‌توانید با توجه به برنامه خود، نام branch ها را تغییر دهید.

در سه دستور `liara deploy` نام برنامه‌ها برای محیط‌های production, test و staging به ترتیب my-production-app, my-test-app و my-staging-app انتخاب‌‌شده است. شما باید با توجه برنامه‌‌های خود، نام برنامه‌ها را تغییر دهید.


