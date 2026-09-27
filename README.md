# کتاب‌کده — بک‌اند (Google Sheets + Express)

## ۱. ساخت Google Sheet

۱. یه Google Sheet جدید بساز.
۲. یه تب (Tab) به اسم دقیقاً `Books` بساز.
۳. ردیف اول این تب رو دقیقاً همین ستون‌ها کن (به همین ترتیب):

```
ID | Title | Author | Category | Price | Rating | Stock | Description | Cover1 | Cover2
```

> نکته: نیازی نیست خودت ID رو پر کنی؛ سرور خودش موقع افزودن کتاب یه ID یکتا می‌سازه.

## ۲. ساخت Service Account (برای اتصال سرور به شیت)

۱. برو به [Google Cloud Console](https://console.cloud.google.com/).
۲. یه پروژه‌ی جدید بساز (یا از قبلی استفاده کن).
۳. از منو، `APIs & Services` → `Library` برو و **Google Sheets API** رو فعال کن.
۴. برو به `APIs & Services` → `Credentials` → `Create Credentials` → `Service Account`.
۵. یه اسم بذار (مثلاً `kitabkade-sheets`) و بسازش.
۶. وارد همون Service Account شو → تب `Keys` → `Add Key` → `Create new key` → نوع `JSON` رو انتخاب کن.
   یه فایل JSON دانلود میشه — نگهش دار، توش دو چیز مهم هست:
   - `client_email`
   - `private_key`
۷. برو توی همون Google Sheet که ساختی، دکمه‌ی **Share** رو بزن و همون `client_email` (شبیه یه ایمیل gserviceaccount.com) رو با دسترسی **Editor** اضافه کن. این مرحله حیاتیه — بدونش سرور نمی‌تونه به شیت بنویسه.

## ۳. تنظیم فایل `.env`

فایل `.env.example` رو کپی کن و اسمش رو بذار `.env`، بعد مقادیر رو پر کن:

```
GOOGLE_SHEET_ID=...        → از آدرس شیت: docs.google.com/spreadsheets/d/{این قسمت}/edit
GOOGLE_SERVICE_ACCOUNT_EMAIL=...  → از فایل JSON: client_email
GOOGLE_PRIVATE_KEY=...     → از فایل JSON: private_key (با همون \n ها)
ADMIN_PASSWORD=...         → یه رمز قوی برای ورود پنل ادمین
JWT_SECRET=...             → یه رشته‌ی تصادفی و طولانی (مثلاً از یه رمزساز آنلاین بگیر)
PORT=4000
```

## ۴. اجرای پروژه (روی سیستم خودت)

```bash
npm install
npm run dev
```

سرور بالا میاد روی `http://localhost:4000`
پنل ادمین: `http://localhost:4000/admin`
API کتاب‌ها: `http://localhost:4000/api/books`

## ۵. آپلود روی گیت‌هاب

```bash
git init
git add .
git commit -m "Initial backend setup"
git branch -M main
git remote add origin https://github.com/USERNAME/REPO_NAME.git
git push -u origin main
```

⚠️ **خیلی مهم:** فایل `.env` رو هیچ‌وقت پوش نکن روی گیت‌هاب (رمز و کلید خصوصیت توشه).
یه فایل `.gitignore` توی پروژه گذاشتم که این کارو خودکار انجام می‌ده.

## ۶. دیپلوی (میزبانی آنلاین)

برای اینکه سایت واقعاً آنلاین بشه، سرور رو باید روی یه هاست اجرا کنی. گزینه‌های رایگان/ساده:
- [Render](https://render.com)
- [Railway](https://railway.app)
- [Cyclic](https://www.cyclic.sh)

توی همه‌شون، مقادیر `.env` رو به‌صورت Environment Variables توی پنل هاست وارد می‌کنی (نه توی فایل).

## نکته‌ی مهم درباره Google Sheets به‌عنوان دیتابیس

Google Sheets برای شروع و تعداد کم کتاب (چند صد تا) خوب کار می‌کنه، اما محدودیت داره:
- هر دقیقه محدودیت تعداد درخواست داره (Rate Limit)
- برای سایت با ترافیک بالا یا هم‌زمانی زیاد کند می‌شه
وقتی سایت بزرگ‌تر شد، پیشنهاد می‌کنم بریم سمت یه دیتابیس واقعی (مثل PostgreSQL) — ساختار کد جوری نوشته شده که این جابه‌جایی بعداً راحت باشه (فقط `services/bookService.js` عوض می‌شه).
