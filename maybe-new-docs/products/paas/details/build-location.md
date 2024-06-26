# تعیین موقعیت build

گاهی ممکن است بخواهید پکیج‌هایی که در موقعیت ایران در دسترس نیستند را دانلود کنید یا نیاز داشته باشید که فرایند push کردن ایمیج‌ها به [رجیستری خصوصی](./private-registery.md)، با سرعت بیشتری انجام شود. برای این موارد می‌توانید موقعیت build ایمیج برنامه‌تان را با یکی از سه روش زیر مشخص کنید:

## روش اول: liara.json

یک فایل با نام `liara.json` در ریشه‌ پروژه‌تان ایجاد کرده و قطعه‌کد زیر را درون این فایل قرار بدید:

```
{
  "build": {
     "location": "germany"
  }
}
```
در نهایت، با اجرای دستور `liara deploy` برنامه را در لیارا مستقر کنید.

## روش دوم: Liara CLI
در استقرار با Liara CLI در دستور `liara deploy` موقعیت build را با پارامتر `build-location--` مشخص کنید:
```
liara deploy --build-location="germany"
```

## روش سوم: Liara Console
در استقرار با کنسول، موقعیت build را در بخش **تنظیمات build** به شکل زیر انتخاب کنید:

![set-build-location](https://files.liara.ir/liara/docs/set-build-location.png)

در حال حاضر، موقعیت‌های زیر در دسترس هستند:
- Iran
- Germany

> [!NOTE]
> با انجام عملیات build در موقعیت germany، دانلود پکیج‌ها هیچ محدودیتی ندارد ولی فرایند push کردن ایمیج‌ها کند می‌باشد. در مقابل، عملیات build در موقعیت iran محدودیت دانلود پکیج‌ها را دارد ولی سرعت push کردن ایمیج‌ها بیشتر است.




