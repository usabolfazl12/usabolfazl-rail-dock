# 🚂 اجرای Local Telegram Bot API روی Railway

Railway از Dockerfile پشتیبانی می‌کند، پس می‌توانی سرور Local Bot API را
مستقیم روی Railway بالا بیاوری و ربات را به آن وصل کنی تا سقف آپلود از
۵۰MB به ۲GB برسد.

---

## 🏗️ معماری: دو سرویس جدا در یک پروژه

Railway هر سرویس = یک کانتینر. پس دو سرویس می‌سازی:

```
پروژه‌ی Railway
├── سرویس A: telegram-bot-api   ← این Dockerfile
└── سرویس B: meme-bot           ← کد پایتون ربات (my bot)
```

ربات (B) از طریق شبکه‌ی داخلی Railway به سرور (A) وصل می‌شود.

---

## مرحله ۱ — ساخت سرویس سرور (A)

1. در پروژه‌ی Railway: **New → GitHub Repo** (یا Empty Service)
2. این پوشه‌ی `local-bot-api` را به‌عنوان روت سرویس بده
   (Railway خودش `Dockerfile` را تشخیص می‌دهد و build می‌کند)
3. در تب **Variables** این‌ها را بگذار:

| Key | Value |
|-----|-------|
| `API_ID` | (از my.telegram.org) |
| `API_HASH` | (از my.telegram.org) |

> ⚠️ اولین build چند دقیقه طول می‌کشد چون سرور از سورس کامپایل می‌شود.

4. **Volume برای سرور:** Settings → Volumes → Add → mount path: `/data`
   (فایل‌های دانلودی/آپلودی موقت اینجا ذخیره می‌شوند)

5. **Networking:** Railway به این سرویس یک آدرس داخلی می‌دهد، مثل:
   ```
   telegram-bot-api.railway.internal
   ```
   این را برای مرحله بعد یادداشت کن (از تب Settings → Networking → Private Networking).

---

## مرحله ۲ — تنظیم سرویس ربات (B)

در تب **Variables** سرویس ربات:

| Key | Value |
|-----|-------|
| `BOT_TOKEN` | توکن ربات |
| `OWNER_ID` | آیدی عددی خودت |
| `DB_PATH` | `/data/bot_data.db` |
| `TG_API_URL` | `http://telegram-bot-api.railway.internal:8081` |

> آدرس داخلی + پورتی که سرور روی آن گوش می‌دهد (`8081`).
> اگر Railway پورت دیگری تزریق کرد، همان را در `TG_API_URL` بگذار.

سرویس ربات هم به یک **Volume روی `/data`** نیاز دارد (برای دیتابیس ماندگار).

---

## مرحله ۳ — تست

بعد از اینکه هر دو سرویس up شدند، در لاگ ربات باید ببینی:
```
استفاده از Local Bot API Server: http://telegram-bot-api.railway.internal:8081 (سقف 2GB)
```

حالا یک ویدیوی بزرگ‌تر از ۵۰MB دانلود کن؛ باید بدون خطای حجم ارسال شود. 🎉

---

## ⚠️ نکات مهم

| نکته | توضیح |
|------|-------|
| 🔀 **logOut از سرور رسمی** | بار اول که به سرور محلی وصل می‌شوی، تلگرام ممکن است بگوید ربات روی سرور دیگری لاگین است. کافیست چند دقیقه صبر کنی یا یک‌بار متد `logOut` را روی سرور رسمی صدا بزنی، بعد سراغ سرور محلی بروی. |
| 📁 **دسترسی به فایل‌ها** | در حالت `--local`، وقتی ربات فایلی می‌فرستد باید سرور به مسیر فایل دسترسی داشته باشد. چون کد ما فایل‌ها را با `FSInputFile` آپلود می‌کند (نه ارجاع مسیر)، aiogram خودش فایل را به سرور می‌فرستد و مشکلی نیست. |
| 💰 **هزینه** | دو سرویس + build سنگین tdlib مصرف منابع دارد؛ حواست به پلن رایگان Railway باشد. |
| 🔒 **پورت عمومی** | سرویس سرور را Public نکن؛ فقط از طریق Private Networking داخلی استفاده کن. |

## منابع
- سرور رسمی: https://github.com/tdlib/telegram-bot-api
- Railway Dockerfile: https://docs.railway.app/guides/dockerfiles
- Railway Private Networking: https://docs.railway.app/guides/private-networking
