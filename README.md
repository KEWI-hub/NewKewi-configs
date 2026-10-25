# NewKewi-configs

Per-site config modules for [New Kewi Video Downloader](https://github.com/KEWI-hub/NewKewiDownloader).

The app syncs this repository on startup, so a new site or a fix to an existing one reaches
everyone without shipping a new release.

```
modules/
├── lua/            one module per site, loaded by the app
│   └── _Example.lua   documented template (files starting with _ are not loaded as sites)
└── naming.json     filename rules for sites that have no module
```

## How the app uses it

1. On startup it asks GitHub for this branch's commit sha
2. Same sha as last time → nothing to do
3. Different → download the tarball and extract **only** `.lua` files under `modules/lua/`
4. Reload the site registry immediately, no restart

Each destination path is checked by the app itself, so an entry with `../` in it cannot write
outside the module folder.

`modules/local/` in the user's own install is never touched by a sync — anything they write
themselves keeps winning.

## Writing a module

Copy `modules/lua/_Example.lua`, rename it, and fill in `Register()` and `GetStreams()`.
The template documents every function and helper available inside the sandbox.

Modules run with `io`, `os.execute` and `debug` removed, and `require` cannot leave the module
folder. They reach the network only through the `http` and `browser` objects the app provides.

---

# NewKewi-configs (ไทย)

config ของเว็บแต่ละเว็บสำหรับ [New Kewi Video Downloader](https://github.com/KEWI-hub/NewKewiDownloader)

แอปซิงก์ repo นี้ตอนเปิดทุกครั้ง เพิ่มเว็บใหม่หรือแก้เว็บเดิมจึงถึงมือผู้ใช้ได้โดย**ไม่ต้องออก release**

## แอปใช้ repo นี้ยังไง

1. เปิดแอป → ถาม GitHub ว่า sha ล่าสุดของ branch คืออะไร
2. เหมือนเดิม → ไม่ต้องทำอะไร
3. ต่างออกไป → โหลด tarball มาแตกเฉพาะไฟล์ `.lua` ใต้ `modules/lua/`
4. โหลด config ใหม่ทันที ไม่ต้องเปิดแอปรอบหน้า

แอปตรวจ path ของทุกไฟล์เอง ไฟล์ที่พยายามเขียนออกนอกโฟลเดอร์ (`../`) จึงทำไม่ได้

`modules/local/` ในเครื่องผู้ใช้ไม่ถูกแตะตอนซิงก์ ของที่เขาเขียนเองชนะเสมอ

## เขียน module ใหม่

ก๊อป `modules/lua/_Example.lua` ไปตั้งชื่อใหม่ แล้วเขียน `Register()` กับ `GetStreams()`
ในเทมเพลตมีคู่มือของทุกฟังก์ชันและตัวช่วยที่เรียกได้

module รันใน sandbox ที่ไม่มี `io`, `os.execute`, `debug` และ `require` ออกนอกโฟลเดอร์ module ไม่ได้
ออกเน็ตได้ทาง `http` กับ `browser` ที่แอปเตรียมให้เท่านั้น
