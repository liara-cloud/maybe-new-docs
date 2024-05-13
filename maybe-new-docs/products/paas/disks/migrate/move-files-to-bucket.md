# انتقال فایل از دیسک به فضای ذخیره‌سازی ابری
[ویدیوی آموزشی](https://files.liara.ir/liara/rclone/rclone-transfer-files-from-disk-to-bucket.mp4)

برای انتقال فایل‌های درون یک دیسک به یک باکت در لیارا، در ابتدا کافیست تا برنامه [Rclone](https://rclone.org/) را از صفحه [Downloads](https://rclone.org/downloads/) متناسب با سیستم عامل خود، دانلود کنید. سپس، باید یک [دسترسی FTP](../connect/ftp.md) برای دیسک خود ایجاد کنید.

در قدم بعد باید با اجرای دستور `rclone config` یک `remote` جدید را برای دیسک پیکربندی کنید:
<div>asciinema:id->rclone-disk-ftp-remote</div>

بعد از انجام کار فوق، اکنون باید مجدداً با اجرای دستور `rclone config` یک `remote` جدید دیگر را برای باکت پیکربندی کنید:
<div>asciinema:id->rclone-backup-bucket</div>

درنهایت شما می‌توانید با اجرای دستور زیر یک نسخه از فایل‌های موجود در دیسک موردنظرتان را در باکت نیز، ذخیره کنید:

```
rclone copy -PM [disk-remote]:/ [bucket-remote]:/[bucket-name]
```
البته اگر که قصد انتقال فایل‌ها را از دیسک به باکت دارید، می‌توانید دستور زیر را اجرا کنید:

```
rclone move -PM [disk-remote]:/ [bucket-remote]:/[bucket-name]
```
برای مثال اگر یک باکت با نام `app-bucket` و یک دیسک در لیارا داشته باشید، می‌توانید با اجرای دستور زیر، تمامی فایل‌های موجود در دیسک را به باکت `app-bucket` انتقال دهید:

```
rclone move -PM r2:/ r1:/app-bucket
```











