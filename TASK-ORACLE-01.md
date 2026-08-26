# TASK-ORACLE-01: ORACLE LAB ÇEKİRDEK MOTORU (Needle + Formül Motoru)

**Durum:** 🔄 İNŞA — Picard kurulumu planladı, Jules kodluyor.
**Proje:** ORACLE-SWARM-AMY-01 (ülke sürü-amigdala simülasyonu — Oracle Lab Aşama 1)
**Mühür:** VERITAS PER SE · Oracle TEK GÖREV: modelleri TEST + KALİBRE et (öngörü/karar/al-sat YOK)

---

## GÖREV ÖZETİ
Oracle Lab'ın çekirdeğini inşa et: **Needle (küçük DZV) + formül motoru** — "formülü ver, sistem hesaplasın." Her kriz formülü için ayrı kod yazma; ortak hesaplama motoru kullan.

## KURULUM (yapıldı — doğrula)
- Needle 2.0.10 kurulu: `C:\needle_lab\nv` (jax 0.10.2, flax 0.12.8, optax 0.2.8)
- UCI_TR formülü Needle ile HESAPLANDI: 0.348 (ispatlandı)
- LongPathsEnabled=1 (Windows uzun yol açık)

## VERİ KAYNAKLARI (Oracle'ı besleyecek — HAZIR)
Oracle motorunu şu GERÇEK verilerle test et ve kalibre et:
1. **Amigdala 29 yıl (HAZIR):** `E:\T2SAIM_NEXUS_MIRROR\0000_A_Karargah\001_Veri_Bankasi\AMIGDALA_MASTER_29YIL_SENTEZ.csv`
   - Kolonlar: piyasa, yil, gini, vol20, vol_yil, sigma_ratio, a_load, pfc_control, theta_panic, durum, ece_calibrated
   - 29 yıl, BTC + piyasalar — **a_load/pfc_control/theta_panic/ECE kalibrasyonu hazır**
2. **6 piyasa gerçek:** `...\AMIGDALA_6_PIYASA_GERCEK.csv` (TR KRIZ a_load 0.707, US 0.639, JP 0.552)
3. **29 yıl piyasa DB:** `...\piyasa_29yil.duckdb` (8.1MB)
4. **CSV katalog:** `...\csv_katalog.json` (18.892 benzersiz CSV — önce bak, indirme)
5. **Kriz formülleri:** `E:\T2SAIM_NEXUS_MIRROR\000_SPARK\T2SAIM _OS\Prediction_Project\crises\Kaptan\Bütün kriz öngörü formülleriBütünleşik Kriz Endeksi (UCITRUCI_{TR}).md`
6. **Birleşik mimari:** `...\T2SAIM_BIRLESIK_KRİZ_MIMARISI_v6_MASTER.md`

**Not:** 10Y yield + CDS 5Y (29 ülke 30 yıl) bu ortamda yok (FRED/Yahoo bloklu). VM'nden IMF IFS / World Bank / Yahoo API'lerini DENE — erişebilirsen 10Y yield çek; erişemezsen amigdala verisiyle devam et (Oracle için yeterli başlangıç).

## FORMÜL MOTORU — İNŞA EDİLECEK (Jules)

### Modül ağacı
```
oracle_lab/
├── motor/
│   ├── formul_cekirdek.py   # LaTeX formül parse + hesaplama (SymPy)
│   ├── arac_kayit.py        # Needle'a araç olarak formül kaydet
│   ├── cebirsel_motor.py    # UCI_TR/EMPI/LDR/Zombie_Ratio (tek satır)
│   ├── simulasyon_motoru.py # Ising/Kuramoto/ABM (numpy, 1024 ajan)
│   └── istatistik_motor.py  # Hurst/Benford/VPIN/Kelly (numpy/scipy)
├── needle_dzv/
│   └── oracle_needle.py     # Needle + araçlar → "formülü ver, hesaplar"
├── veri/
│   └── amigdala_loader.py   # 001_Veri_Bankasi CSV/DuckDB okuyucu
├── testler/
│   ├── test_uci_tr.py       # UCI_TR 0.348 doğrula
│   └── test_amigdala.py     # AMIGDALA_MASTER 29 yıl ile kalibrasyon testi
└── README.md
```

### Temel gereksinimler
1. **formul_cekirdek.py:** LaTeX `$$...$$` formülünü parse edip hesapla (SymPy). Formül + değişken değerleri → sonuç.
2. **arac_kayit.py:** Kriz formüllerini `@needle.tool` olarak kaydet (UCI_TR, EMPI, LDR, Zombie_Ratio, C_takas, GSCI...).
3. **simulasyon_motoru.py:** Ising (1024 ajan), Kuramoto (senkron), ABM — numpy ile, RAM < 12GB.
4. **oracle_needle.py:** Needle'a formül motorunu bağla → "formülü ver, sistem hesaplar", kalibre güven skoru döner.
5. **amigdala_loader.py:** AMIGDALA_MASTER_29YIL_SENTEZ.csv + piyasa_29yil.duckdb oku, Oracle formüllerini bu gerçek veriyle test et.
6. **Epistemik hijyen:** Her sonuç kalibre güven skoru + tekrarlanabilirlik (aynı girdi → aynı çıktı).

## KURALLAR
- Ayrı kod YOK — ortak motor. Formülü tanımla, sistem hesaplasın.
- Needle formülü "düşünüp" hesaplamaz; Python motoru (numpy/sympy) hesaplar, Needle aracı seçer + güven skoru verir.
- Uydurma YOK — her formül gerçek veriyle doğrulanır (UCI_TR 0.348 testi + AMIGDALA 29 yıl).
- Oracle öngörü/karar/al-sat YAPMAZ — sadece test+kalibre.
- Kod yazımı Jules'a ait (Kaptan: "kodu yazdır").

## TESLİM KABUL KRİTERLERİ
1. `python testler/test_uci_tr.py` → UCI_TR = 0.348 ✅
2. Needle + motor entegrasyonu: "formülü ver, hesaplar" çalışıyor
3. Ising/ABM simülasyonu 1024 ajan, RAM < 12GB
4. `test_amigdala.py` → AMIGDALA_MASTER 29 yıl verisiyle kalibrasyon çalışıyor
5. Her formül sonucu kalibre güven skoruyla
6. README (nasıl çalıştırılır)

## NOT
Bu, Oracle Lab Aşama 1'in çekirdeği. Aşama 2 = ülke sürü-amigdala simülasyonu, Aşama 3 = rüya, Aşama 4 = Spark Shield bekçi. Sırayla.
