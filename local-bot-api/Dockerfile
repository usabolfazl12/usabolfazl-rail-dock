# ایمیج رسمی از پیش کامپایل‌شده — بدون build سنگین (چند ثانیه به‌جای ۲۰+ دقیقه).
# این ایمیج یک entrypoint هوشمند دارد که تنظیمات را از متغیرهای محیطی TELEGRAM_* می‌خواند.
# پس نیازی به نوشتن CMD نیست؛ فقط در تب Variables این‌ها را ست کن:
#   TELEGRAM_API_ID   = عدد api_id
#   TELEGRAM_API_HASH = هش api_hash
#   TELEGRAM_LOCAL    = 1        (برای عبور از محدودیت ۵۰ مگابایت)
FROM aiogram/telegram-bot-api:latest
