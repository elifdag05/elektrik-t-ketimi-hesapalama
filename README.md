import matplotlib.pyplot as plt

# Kullanıcıdan genel bilgiler
birim_fiyat = float(input("Elektrik birim fiyatı (TL/kWh): "))
cihazlar = []

# Cihaz verilerini al
while True:
    ad = input("\nCihaz adı (çıkmak için boş bırak): ")
    if ad == "":
        break
    guc = float(input(f"{ad} gücü (W): "))
    sure = float(input(f"{ad} günlük kullanım süresi (saat): "))

    gunluk_tuketim = (guc * sure) / 1000  # kWh
    aylik_tuketim = gunluk_tuketim * 30
    aylik_maliyet = aylik_tuketim * birim_fiyat

    cihazlar.append({
        "ad": ad,
        "gunluk_tuketim": gunluk_tuketim,
        "aylik_tuketim": aylik_tuketim,
        "aylik_maliyet": aylik_maliyet
    })

# Hesaplama özeti
print("\n🔋 Aylık Elektrik Tüketim Özeti:")
toplam_tuketim = 0
toplam_maliyet = 0

for c in cihazlar:
    print(f"- {c['ad']}: {c['aylik_tuketim']:.2f} kWh, {c['aylik_maliyet']:.2f} TL")
    toplam_tuketim += c['aylik_tuketim']
    toplam_maliyet += c['aylik_maliyet']

print(f"\n💡 Toplam Aylık Tüketim: {toplam_tuketim:.2f} kWh")
print(f"💰 Toplam Aylık Maliyet: {toplam_maliyet:.2f} TL")

# 🔸 Grafik oluştur
cihaz_adlari = [c["ad"] for c in cihazlar]
maliyetler = [c["aylik_maliyet"] for c in cihazlar]

plt.figure(figsize=(10, 6))
bars = plt.bar(cihaz_adlari, maliyetler, color="royalblue")
plt.title("Cihazlara Göre Aylık Elektrik Maliyeti")
plt.xlabel("Cihaz")
plt.ylabel("Maliyet (TL)")
plt.grid(axis='y', linestyle='--', alpha=0.7)

# Barların üzerine değer yaz
for bar in bars:
    plt.text(bar.get_x() + bar.get_width()/2, bar.get_height(),
             f"{bar.get_height():.1f}", ha='center', va='bottom', fontsize=9)

plt.tight_layout()
plt.show()

