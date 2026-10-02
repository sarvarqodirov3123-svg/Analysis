# 🛒 Elektronika savdosi tahlili

Elektronika mahsulotlari savdosi ma'lumotlarini **Jupyter Notebook** yordamida tahlil qiluvchi loyiha. Notebook ma'lumotni tozalaydi, ustunlar orasidagi bog'liqlikni ko'rsatadi va kategoriyalar hamda regionlar bo'yicha savdo ulushlarini vizuallashtiradi.

---

## 📁 Loyiha tarkibi

```
.
├── Tahlil.ipynb    # Asosiy tahlil notebooki
├── dataset.csv     # Ma'lumotlar to'plami (2 400 qator, 15 ustun)
└── README.md
```

---

## 📦 Ma'lumotlar to'plami (`dataset.csv`)

**2 400 qator × 15 ustun.** Davr: **2023-yil yanvar – 2024-yil dekabr** (24 oy).

| Ko'rsatkich | Qiymatlari |
|---|---|
| **Kategoriyalar (10)** | `smartphones`, `laptops`, `tablets`, `wearables`, `wireless_audio`, `gaming`, `smart_home`, `cameras`, `cables_chargers`, `storage_devices` |
| **Regionlar (5)** | Tashkent, Samarkand, Bukhara, Andijan, Namangan |
| **Do'kon turi** | `offline`, `online`, `both` |

Har bir kategoriya–region juftligi uchun aynan 48 qator bor, ya'ni ma'lumot muvozanatli.

### Ustunlar tavsifi

| Ustun | Turi | Izoh |
|---|---|---|
| `product_id` | matn | Yozuv identifikatori (`P1001`, `P1002`, ...) |
| `category` | matn | Mahsulot kategoriyasi |
| `region` | matn | Savdo regioni |
| `store_type` | matn | Sotuv kanali: oflayn, onlayn yoki ikkalasi |
| `year` | butun son | Yil (2023 yoki 2024) |
| `month` | butun son | Oy (1–12) |
| `avg_unit_price` | haqiqiy son | Bir dona mahsulotning o'rtacha narxi |
| `units_sold` | butun son | Sotilgan dona soni |
| `returns` | butun son | Qaytarilgan mahsulotlar soni |
| `discount_pct` | haqiqiy son | Chegirma foizi (0–35) |
| `promo_active` | mantiqiy | Aksiya faol bo'lganmi (`True` / `False`) |
| `customer_rating` | haqiqiy son | Mijozlar bahosi (3.0–5.0) |
| `competitor_price_index` | haqiqiy son | Raqobatchilar narx indeksi (0.85–1.20) |
| `marketing_spend` | haqiqiy son | Marketing xarajati |
| `monthly_revenue` | haqiqiy son | Oylik tushum |

### Yetishmayotgan qiymatlar

| Ustun | Bo'sh qiymatlar | Ulushi |
|---|---|---|
| `promo_active` | 96 | 4.0% |
| `customer_rating` | 72 | 3.0% |
| `competitor_price_index` | 48 | 2.0% |

Boshqa ustunlarda bo'sh qiymat yo'q.

---

## 🔬 Notebook nima qiladi

| № | Bosqich | Tavsif |
|---|---|---|
| 1 | **Yuklash** | Kutubxonalarni import qilish va `dataset.csv` ni o'qish |
| 2 | **Dastlabki ko'rik** | `head()`, `info()`, `describe()`, ustun turlari |
| 3 | **Tozalash** | Bo'sh qiymatlarni tekshirish; `customer_rating` va `competitor_price_index` o'rtacha bilan, `promo_active` moda (eng ko'p uchraydigan qiymat) bilan to'ldiriladi |
| 4 | **Korrelyatsiya** | Raqamli ustunlar uchun `seaborn` issiqlik xaritasi |
| 5 | **Reyting grafigi** | Kategoriyalar bo'yicha mijozlar reytingi (ustunli grafik) |
| 6 | **Tushum ulushi** | Har bir kategoriyaning umumiy tushumdagi ulushi (foizda) |
| 7 | **Regionlar ulushi** | Regionlar bo'yicha doiraviy diagramma |

### Ma'lumotdagi asosiy kuzatuvlar

- **Tushum bo'yicha yetakchi** kategoriya `smartphones`: umumiy tushumning taxminan 42% ini beradi. Keyingilari: `laptops`, `gaming`, `tablets`.
- **Eng kuchli bog'liqlik**: `units_sold` va `returns` (r ≈ 0.90), ya'ni ko'proq sotilgan sari ko'proq qaytariladi.
- **Tushumga eng kuchli ta'sir qiluvchi**: `marketing_spend` (r ≈ 0.79) va `avg_unit_price` (r ≈ 0.49).
- `discount_pct` va `monthly_revenue` o'rtasida deyarli bog'liqlik yo'q (r ≈ 0.00).
- `avg_unit_price` va `units_sold` o'rtasida teskari bog'liqlik bor (r ≈ −0.34): arzon mahsulotlar ko'proq sotiladi.

---

## 🚀 Ishga tushirish

**Talablar:** Python 3.9+

```bash
# 1. (ixtiyoriy) virtual muhit
python -m venv venv
source venv/bin/activate        # Windows: venv\Scripts\activate

# 2. kutubxonalarni o'rnatish
pip install numpy pandas matplotlib seaborn jupyter

# 3. notebookni ochish
jupyter notebook Tahlil.ipynb
```

> `dataset.csv` fayli `Tahlil.ipynb` bilan **bir papkada** turishi kerak, chunki notebook uni nisbiy yo'l bilan o'qiydi.

Notebook ichidagi katakchalarni yuqoridan pastga ketma-ket ishga tushiring (**Run All**).

---

## ⚠️ Ma'lumot sifati bo'yicha eslatmalar

- `returns` ustunida **3 ta manfiy qiymat** bor (minimum −5). Qaytarishlar soni manfiy bo'lolmaydi, shuning uchun bu xato yozuvlar.
- `product_id` ustunida **2 ta takrorlanuvchi** qiymat bor. Bu ustun mahsulotni emas, har bir qatorni belgilaydi.
- `units_sold`, `returns`, `marketing_spend` va `monthly_revenue` ustunlarida juda katta qiymatlar (chetlashmalar) bor. Masalan, `monthly_revenue` o'rtachasi ≈ 39 mingda, maksimumi esa ≈ 1.28 mln.

---

## 🛠️ Notebookda tuzatilishi kerak bo'lgan joylar

Notebookni o'rganish davomida quyidagi kamchiliklar topildi:

1. **Promo to'ldirish ishlamaydi.** `fillna(df["promo_active"].mode())` mode'ni Series sifatida beradi va indeks bo'yicha moslashadi, shuning uchun bo'sh qiymatlarning deyarli hammasi to'ldirilmay qoladi. To'g'risi: `.fillna(df["promo_active"].mode()[0])`.
2. **Grafiklarda yorliqlar chalkashishi mumkin.** `x = df["category"].unique()` ma'lumotdagi tartibda, `groupby(...)` esa alifbo tartibida qaytaradi. Natijada ustunlar noto'g'ri nom bilan chiqadi. To'g'risi: `y = df.groupby("category")[...]...` va `x = y.index`.
3. **Reyting grafigi** `sum()` bilan hisoblangan va `%` belgisi bilan yozilgan. Reyting yig'indisi ma'noga ega emas, `mean()` ishlatish kerak.
4. **Regionlar diagrammasi** `count()` ishlatadi, shuning uchun hamma bo'lak teng (20%) chiqadi. Tushum ulushi uchun `sum()` kerak.
5. Ba'zi grafiklar sarlavhasida **«foyda»** deyilgan, lekin hisoblanayotgan ustun `monthly_revenue`, ya'ni **tushum** (foyda emas).

---

## 💡 Keyingi qadamlar

- Vaqt bo'yicha tahlil: oylik va yillik tushum dinamikasi, mavsumiylik
- Do'kon turi (`online` / `offline` / `both`) bo'yicha solishtirish
- Aksiya (`promo_active`) tushumga ta'sirini baholash
- Regionlar kesimida tushum va reytingni solishtirish
- Chetlashmalarni aniqlash va qayta ishlash
- Tushumni bashorat qilish uchun regressiya modeli

---

## 🧰 Texnologiyalar

`Python` · `pandas` · `NumPy` · `Matplotlib` · `seaborn` · `Jupyter Notebook`
