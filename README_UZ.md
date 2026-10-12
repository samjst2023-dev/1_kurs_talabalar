# 1-bosqich talabalar ma’lumotlarini yig‘ish tizimi

## Imkoniyatlar
- Talaba umumiy havola orqali ma’lumot va rasm yuboradi.
- Maydonlar: familiya, ism, sharif, telefon, guruh, rasm.
- Admin parol bilan kiradi; ro‘yxatni qidiradi va guruh bo‘yicha filtrlaydi.
- Excel (`.xlsx`) va Excel + guruhlar bo‘yicha rasmlar ZIP arxivini yuklab oladi.
- Rasm formatini tekshiradi, maksimal 7 MB, JPEG qilib saqlaydi.
- Ma’lumotlar SQLite bazasida saqlanadi.

## A. GitHub’ga joylash
1. GitHub.com saytiga kiring va **New repository** bosing.
2. Repository nomini `talabalar-malumotlari` deb yozing.
3. Public yoki Private tanlang (kodni yashirish uchun Private mumkin), **Create repository** bosing.
4. **Add file → Upload files** orqali loyiha ichidagi barcha fayl va `templates`, `static` papkalarini yuklang. Yoki kompyuterda Git orqali push qiling.
5. `app.py`, `requirements.txt`, `render.yaml`, `templates/`, `static/` repository ildizida tursin.

## B. Render’da saytni ishga tushirish
1. https://render.com saytiga kiring va GitHub bilan ro‘yxatdan o‘ting.
2. **New + → Blueprint** ni tanlang va repository’ni ulang.
3. Render `render.yaml` ni o‘qiydi. `ADMIN_PASSWORD` uchun kuchli, boshqa joyda ishlatilmaydigan parol kiriting.
4. Diskli xizmat pullik tarif talab qilishi mumkin. Doimiy disk yoqilganini tekshiring: `/var/data`.
5. Deploy tugagach Render bergan `https://....onrender.com` havolani oching.
6. Talabalarga shu bosh sahifa havolasini yuboring.
7. Admin paneli uchun `https://SAYT-HAVOLASI/admin/login` manziliga kiring.

**Muhim:** Render’dagi doimiy disk ma’lumotlar yo‘qolib ketmasligi uchun zarur. Disk bo‘lmasa, bepul xizmat qayta ishga tushganda SQLite bazasi va rasmlar yo‘qolishi mumkin. `render.yaml` diskni biriktirishni so‘raydi; tarif va narxlar Render hisobingizda ko‘rsatiladi.

## C. Guruhlar ro‘yxatini o‘zgartirish
Render → xizmat → **Environment** bo‘limi → `GROUPS` qiymatini vergul bilan ajratib kiriting:
`101,102,103,104,105,201,202`
So‘ng deploy/restart qiling. Guruh nomlarini aynan qanday yozilsa, eksportda ham shunday papkalar yaratiladi.

## D. Admin parolini o‘zgartirish
Render → xizmat → **Environment** → `ADMIN_PASSWORD` qiymatini yangi kuchli parolga almashtiring → Save, so‘ng deploy/restart.
`SECRET_KEY` ni ochiq joyda e’lon qilmang va o‘zgartirgandan keyin admin sessiyalari qayta kirishni talab qilishi mumkin.

## E. Kompyuterda sinash
Python 3.11 yoki 3.12 o‘rnating. Terminal/PowerShell’da loyiha papkasiga o‘ting:
```powershell
py -m venv .venv
.\.venv\Scripts\Activate.ps1
pip install -r requirements.txt
$env:ADMIN_PASSWORD="Kuchli-Vaqtinchalik-Parol-2026!"
$env:SECRET_KEY="Mahalliy-test-uchun-uzun-tasodifiy-kalit"
python app.py
```
Brauzerda `http://127.0.0.1:5000` ni oching. Admin: `http://127.0.0.1:5000/admin/login`.

## F. Eksport
Admin paneliga kirib:
- **Excel faylini yuklab olish** — barcha yozuvlar jadvali.
- **Excel + rasmlar ZIP** — Excel fayli va `Rasmlar/101/`, `Rasmlar/102/` kabi guruh papkalari.

## Xavfsizlik va maxfiylik
- Administrator parolini talabalarga tarqatmang.
- Ma’lumot yig‘ishdan oldin talabalarni nima uchun ma’lumot va rasm yig‘ilayotgani haqida xabardor qiling.
- HTTPS havoladan foydalaning; Render domeni HTTPS bilan ishlaydi.
- Ma’lumotlar zaxira nusxasini muntazam saqlang. Ushbu loyiha avtomatik bulutli backup xizmati emas.
- Rasm fayllari ommaga ochiq URL bilan berilmaydi; ular adminning ZIP eksportiga kiritiladi.
- Bu boshlang‘ich amaliy loyiha. Tashkilot miqyosida ishlatishdan oldin maxfiylik siyosati, zaxiralash, kirish auditi va ma’lumotlarni saqlash muddatini belgilang.
