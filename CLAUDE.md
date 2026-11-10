# Kuzey'in haftalık ödev sayfası

GitHub Pages ile yayınlanan tek sayfalık site (`index.html`). Kuzey (4E, Bahçeşehir Kurtköy) her günün kartında yapacaklarını görür, işaretler ve ödev fotoğrafı yükler.

## Yapı
- `weeks/index.json` — hafta listesi, `guncel` gösterilecek hafta (Pazartesi tarihi, `YYYY-MM-DD`).
- `weeks/<pazartesi>.json` — o haftanın işleri. **Kural:** ödevler ait oldukları günün kartına yazılır (Pazartesi ödevi → Pazartesi kartı, hafta sonu ödevleri → "Hafta sonu" kartı). Okula götürülecek şeyler (`tur: "getir"`, `icin: "<gün>"`) gerektiği günden bir ÖNCEKİ günün kartına yazılır (Pazartesi tableti → Pazar, Cuma malzemesi → Perşembe). Belirli günü olmayanlar `haftaBoyu`'na.
  - İş alanları: `id` (hafta içinde benzersiz), `tur` (`odev` | `getir` | `okuma` | `hatirlat`), `ders`, `baslik`, `detay?`, `foto?` (fotoğraf istenecekse `true`).
- `data/<pazartesi>/durum.json` — sayfanın yazdığı işaretler `{ isler: { <id>: { tamam, zaman } } }`.
- `data/<pazartesi>/foto/<id>__<YYYYMMDD-HHMMSS>.jpg` — Kuzey'in yüklediği fotoğraflar.
- `data/<pazartesi>/kontrol.json` — Claude'un kontrol sonucu, sayfada ilgili işin altında görünür:
  `{ "isler": { "<id>": { "durum": "tamam" | "duzelt", "hatalar": ["syf. 45, 3. soru: ..."], "not": "", "tarih": "..." } } }`

## Haftalık akış
1. Cumartesi: okul e-postalarını takvime (Aile) ekle, aynı içerikle yeni `weeks/<pazartesi>.json` oluştur, `weeks/index.json`'da `guncel`'i güncelle ve listeye ekle.
2. Kontrol: `data/<hafta>/foto/` altındaki yeni fotoğraflara bak, hataları `kontrol.json`'a yaz ve ebeveyne (Taylan) özellikle yanlışları bildir.

Sayfa GitHub API'ye yazmak için cihazda saklanan, yalnızca bu depoya `Contents: Read and write` izni olan fine-grained anahtar kullanır; anahtar depoya asla yazılmaz.
