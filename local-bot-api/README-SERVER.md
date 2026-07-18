# 🚀 راه‌اندازی سرور محلی Bot API (عبور از محدودیت ۵۰ مگابایت)

## تنظیمات در Railway

### 1️⃣ سرویس سرور محلی
| بخش | مقدار |
|-----|-------|
| **Root Directory** | `local-bot-api` |
| **Volume** | `/data` (باید با سرویس ربات مشترک باشد) |

#### متغیرهای محیطی (Variables):
| Key | Value |
|-----|-------|
| `TELEGRAM_API_ID` | عددی که از my.telegram.org گرفتی |
| `TELEGRAM_API_HASH` | هشی که از my.telegram.org گرفتی |
| `TELEGRAM_LOCAL` | `1` |

### 2️⃣ سرویس ربات
| بخش | مقدار |
|-----|-------|
| **Root Directory** | `railway-bot` |
| **Volume** | `/data` (همان ولوم مشترک با سرور محلی) |

#### متغیرهای محیطی (Variables):
| Key | Value |
|-----|-------|
| `BOT_TOKEN` | توکن رباتت |
| `OWNER_ID` | آیدی عددی خودت |
| `DB_PATH` | `/data/bot_data.db` |
| `TG_API_URL` | `http://NAMA-SERVICE.railway.internal:8081` |

> **نکته:** حتماً `TG_API_URL` را با نام دقیق سرویس سرور محلی خودت جایگزین کن
> (مثلاً `http://usabolfazl-rail-dock.railway.internal:8081`)

## تست
بعد از دیپلوی، لاگ ربات را چک کن:
```
INFO:root:استفاده از Local Bot API Server: http://...:8081 (سقف 2GB)
✅ دیتابیس با موفقیت ریستور شد
```
