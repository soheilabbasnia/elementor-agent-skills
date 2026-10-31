# Elementor Agent Skills Suite

<div dir="rtl">

مجموعه مهارت‌های استاندارد و جامع برای طراحی، ساخت و مدیریت وب‌سایت‌های وردپرسی مبتنی بر **المنتور (Elementor 4.0+ & 3.x)** توسط ایجنت‌ها و دستیاران هوش مصنوعی (Antigravity, Claude Code, Cursor, Codex).

</div>

---

## 🚀 Overview / نمای کلی

This repository provides an enterprise-grade modular suite of **AI Agent Skills** specifically tailored for WordPress & Elementor. Whether you want an AI agent to **generate clean, importable JSON templates from scratch** with modern Flexbox Containers, **build pages live via the Model Context Protocol (MCP)**, or **manage sites at scale using WP-CLI and Browser Automation**, this repository has a dedicated skill for each workflow.

این مجموعه به چهار مهارت تخصصی و مجزا تفکیک شده است تا ایجنت‌های هوش مصنوعی بتوانند بر اساس دسترسی‌ها و سناریوی پروژه، از مناسب‌ترین ابزار استفاده کنند:

| Skill / نام مهارت | Focus / تمرکز اصلی | Connection Mode / نحوه ارتباط | RTL & Persian / پشتیبانی فارسی |
| :--- | :--- | :--- | :---: |
| [`elementor-template-architect`](skills/elementor-template-architect) | معمار تولید قالب‌های JSON سازگار با کانتینرهای فلکس‌باکس المنتور ۴+ | Offline / بدون نیاز به سایت (خروجی مستقیم JSON) | 🟢 بومی |
| [`elementor-mcp`](skills/elementor-mcp) | طراحی و شخصی‌سازی زنده روی وردپرس از طریق پروتکل MCP (پلاگین EMCP Tools) | Live via MCP Protocol | 🟢 بومی |
| [`elementor-premium-addons`](skills/elementor-premium-addons) | استفاده از افزونه پرمیوم ادانز، کتابخانه تمپلیت‌های آماده و ویجت‌های پیشرفته | Live via Premium Addons MCP | 🟢 بومی |
| [`elementor-ops`](skills/elementor-ops) | تعمیرات، تغییر سریع متون دیتابیس با WP-CLI، خالی کردن کش و اتوماسیون مرورگر | WP-CLI (SSH) + Browser Automation | 🟢 بومی |

---

## 📂 Skills Breakdown / معرفی مهارت‌ها

### 1. `elementor-template-architect`
* **هدف:** تولید مستقیم قالب‌های تک‌صفحه یا بلوک‌های المنتور نسخه ۴ و ۳ جدید به صورت فایل ساختاریافته و استاندارد `.json`.
* **ویژگی‌های کلیدی:**
  * بهره‌گیری ۱۰۰٪ از معماری جدید **Flexbox Container** (حذف کامل ساختار منسوخ Section / Column).
  * ایجاد شناسه‌های یکتا و معتبر هگزادسیمال ۸ کاراکتری برای تمامی المان‌ها.
  * جلوگیری اکید از قرار دادن کدهای شکننده در ویجت `html` و حفظ ۱۰۰٪ قابلیت ویرایش بصری با ویجت‌های بومی (`heading`, `text-editor`, `button`, `image`, `icon-box`, `counter`).
  * پیاده‌سازی استانداردهای چیدمان راست‌به‌چپ (RTL) و فونت‌های استاندارد فارسی (وزیرمتن، ایران‌یکان، دانا).
  * واکنش‌گرایی تضمین‌شده برای تبلت و موبایل با متغیرهای اختصاصی `_tablet` و `_mobile`.

### 2. `elementor-mcp`
* **هدف:** ساخت و ویرایش صفحات روی وردپرس زنده از طریق پلاگین **EMCP Tools (v3+)**.
* **ویژگی‌های کلیدی:**
  * جریان استاندارد کار با کاتالوگ ویجت‌ها (`list-widgets` → `get-widget-schema` → `add-free-widget`).
  * اعمال پالت رنگ و تایپوگرافی با متغیرهای Global Kit (`__globals__`) به جای هاردکد کردن مقادیر.
  * حل باگ‌های رایج المنتور مانند تله جمع‌شدن کانتینرهای فرزند (`content_width: "full"` در فرزندان فلکس).
  * استفاده از کلیدهای صحیح المنتور (مانند `flex_gap` به جای `gap` و `padding` به جای `_padding`).

### 3. `elementor-premium-addons`
* **هدف:** پشتیبانی از اکوسیستم پرطرفدار **Premium Addons for Elementor**.
* **ویژگی‌های کلیدی:**
  * دسترسی به صدها سکشن و الگوی آماده (Hero, Features, Pricing, Testimonials).
  * اعمال تدابیر امنیتی عدم انتشار یا حذف تصادفی المان‌ها روی سایت زنده (Drafts by Default).
  * هماهنگی استایل تمپلیت‌ها با استایل‌های کلی سایت.

### 4. `elementor-ops`
* **هدف:** مدیریت عملیاتی، نگهداری و خطایابی سایت‌های المنتوری بدون نیاز به MCP.
* **ویژگی‌های کلیدی:**
  * جایگزینی امن متون و آدرس تصاویر در دیتابیس با `wp search-replace` روی فیلد `_elementor_data`.
  * خالی‌کردن فوری کش CSS المنتور (`wp elementor flush-css`).
  * کپی و انتقال قالب‌ها بین صفحات به کمک WP-CLI.
  * بازبینی ظاهری و تست واکنش‌گرایی صفحه از طریق ابزارهای مرورگر بدون هدلس (مانند Playwright).

---

## 🛠️ Installation & Usage / راهنمای نصب و استفاده

### در Antigravity:
کافی است این مخزن یا پوشه هر مهارت را در مسیر مهارت‌های کاربر یا پروژه قرار دهید:
```bash
# کپی کل مهارت‌ها در پوشه مهارت‌های کاربری آنتی‌گرویتی
cp -r skills/* ~/.gemini/antigravity/skills/
```

### در Claude Code یا دستیارهای دیگر:
می‌توانید هر مهارت را به مسیر `.claude/skills/` پروژه یا دایرکتوری تنظیمات اضافه کنید:
```bash
mkdir -p .claude/skills
cp -r skills/elementor-template-architect .claude/skills/
```

---

## 📜 License
Distributed under the [MIT License](LICENSE).
Open source and free for the WordPress & AI developer community.
