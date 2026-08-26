# TASK-ORACLE-01: ORACLE LAB ÇEKİRDEK MOTORU (Needle + Formül Motoru)

**Durum:** 🔄 HAZIR — Picard kurulumu planladı, Jules kodlayacak.
**Proje:** ORACLE-SWARM-AMY-01 (ülke sürü-amigdala simülasyonu — Oracle Lab Aşama 1)
**Mühür:** VERITAS PER SE · Oracle TEK GÖREV: modelleri TEST + KALİBRE et (öngörü/karar/al-sat YOK)

---

## GÖREV ÖZETİ
Oracle Lab'ın çekirdeğini inşa et: **Needle (küçük DZV) + formül motoru** — "formülü ver, sistem hesaplasın." Her kriz formülü için ayrı kod yazma; ortak hesaplama motoru kullan.

## KURULUM (yapıldı — doğrula)
- Needle 2.0.10 kurulu: `C:\needle_lab\nv` (jax 0.10.2, flax 0.12.8, optax 0.2.8)
- UCI_TR formülü Needle ile HESAPLANDI: 0.348 (ispatlandı)
- LongPathsEnabled=1 (Windows uzun yol açık)

## FORMÜL MOTORU — İNŞA EDİLECEK (Jules)

### Modül ağacı
```
oracle_lab/
├── motor/
│   ├── formül_çekirdek.py   # LaTeX formül parse + hesaplama (SymPy)
│   ├── araç_kayıt.py        # Needle'a araç olarak formül kaydet
│   ├── cebirsel_motor.py    # UCI_TR/EMPI/LDR/Zombie_Ratio (tek satır)
│   ├── simülasyon_motoru.py # Ising/Kuramoto/ABM (numpy, 1024 ajan)
│   └── istatistik_motor.py  # Hurst/Benford/VPIN/Kelly (numpy/scipy)
├── needle_dzv/
│   └── oracle_needle.py     # Needle + araçlar → "formülü ver, hesaplar"
├── testler/
│   ├── test_uci_tr.py       # UCI_TR 0.348 doğrula
│   └── test_formul.py
└── README.md
```

### Temel gereksinimler
1. **formül_çekirdek.py:** LaTeX `$$...$$` formülünü parse edip hesapla (SymPy). Formül + değişken değerleri → sonuç.
2. **araç_kayıt.py:** Kriz formüllerini `@needle.tool` olarak kaydet (UCI_TR, EMPI, LDR, Zombie_Ratio, C_takas, GSCI...).
3. **simülasyon_motoru.py:** Ising (1024 ajan), Kuramoto (senkron), ABM — numpy ile, RAM < 12GB.
4. **oracle_needle.py:** Needle'a formül motorunu bağla → "formülü ver, sistem hesaplar", kalibre güven skoru döner.
5. **Epistemik hijyen:** Her sonuç kalibre güven skoru + tekrarlanabilirlik doğrulaması (aynı girdi → aynı çıktı).

### Veri kaynakları (formüller)
- UCI_TR + kriz formülleri: `E:\T2SAIM_NEXUS_MIRROR\000_SPARK\T2SAIM _OS\Prediction_Project\crises\Kaptan\Bütün kriz öngörü formülleriBütünleşik Kriz Endeksi (UCITRUCI_{TR}).md`
- Birleşik mimari (6 katman + formül grupları): `...\T2SAIM_BIRLESIK_KRİZ_MIMARISI_v6_MASTER.md`

## KURALLAR
- Ayrı kod YOK — ortak motor. Formülü tanımla, sistem hesaplasın.
- Needle formülü "düşünüp" hesaplamaz; Python motoru (numpy/sympy) hesaplar, Needle aracı seçer + güven skoru verir.
- Uydurma YOK — her formül gerçek veriyle doğrulanır (UCI_TR 0.348 testi).
- Oracle öngörü/karar/al-sat YAPMAZ — sadece test+kalibre.
- Kod yazımı Jules'a ait (Kaptan: "kodu yazdır").

## TESLİM KABUL KRİTERLERİ
1. `python testler/test_uci_tr.py` → UCI_TR = 0.348 ✅
2. Needle + motor entegrasyonu: "formülü ver, hesaplar" çalışıyor
3. Ising/ABM simülasyonu 1024 ajan, RAM < 12GB
4. Her formül sonucu kalibre güven skoruyla
5. README (nasıl çalıştırılır)

## NOT
Bu, Oracle Lab Aşama 1'in çekirdeği. Aşama 2 = ülke sürü-amigdala simülasyonu, Aşama 3 = rüya, Aşama 4 = Spark Shield bekçi. Sırayla.
