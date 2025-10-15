BAŞLA

// 1. Öğrenci Girişi
YAZ "Öğrenci numarasını giriniz: "
OKU ogr_no
YAZ "Şifreyi giriniz: "
OKU sifre

EĞER (ogr_no ve sifre doğruysa) İSE
    YAZ "Giriş başarılı."
    DEVAM ET
DEĞİLSE
    YAZ "Hatalı giriş. Tekrar deneyiniz."
    DUR
BİTİŞ

// 2. Ders Listesini Görüntüleme
YAZ "Alınabilir dersler listeleniyor..."
HER ders İÇİN ders_listesi’NDE
    YAZ ders.kod, ders.ad, "Kredi:", ders.kredi, "Kontenjan:", ders.kontenjan
SON

// 3. Ders Ekleme/Çıkarma Döngüsü
toplam_kredi ← 0
secilen_dersler ← boş_liste

TEKRAR
    YAZ "1- Ders ekle  2- Ders çıkar  3- Kayıt özetini göster  0- Çıkış"
    OKU secim

    EĞER secim = 1 İSE
        YAZ "Eklemek istediğiniz ders kodunu giriniz:"
        OKU ders_kod
        ders ← ders_bul(ders_kod)

        // Kontenjan kontrolü
        EĞER ders.kontenjan = 0 İSE
            YAZ "Bu derste boş kontenjan yok!"
            DEVAM ET
        BİTİŞ

        // Ön koşul dersi kontrolü
        EĞER ders.onkosul VARSA VE ogrenci bu_onkosulu_almamis İSE
            YAZ "Bu dersi almak için ön koşul dersi tamamlamanız gerekiyor!"
            DEVAM ET
        BİTİŞ

        // Zaman çakışması kontrolü
        EĞER ders.zaman ÇAKIŞIYORSA secilen_dersler’den biriyle
            YAZ "Zaman çakışması var!"
            DEVAM ET
        BİTİŞ

        // Kredi limiti kontrolü
        EĞER toplam_kredi + ders.kredi > 35 İSE
            YAZ "Kredi limiti aşıldı! Maksimum 35 kredi alabilirsiniz."
            DEVAM ET
        BİTİŞ

        // Ders ekleme işlemi
        secilen_dersler’e ders ekle
        toplam_kredi ← toplam_kredi + ders.kredi
        ders.kontenjan ← ders.kontenjan - 1
        YAZ "Ders eklendi:", ders.ad

    EĞER secim = 2 İSE
        YAZ "Çıkarmak istediğiniz ders kodunu giriniz:"
        OKU ders_kod
        ders ← ders_bul(ders_kod)
        EĞER ders secilen_dersler’DE VARSA
            secilen_dersler’den ders çıkar
            toplam_kredi ← toplam_kredi - ders.kredi
            ders.kontenjan ← ders.kontenjan + 1
            YAZ "Ders çıkarıldı:", ders.ad
        DEĞİLSE
            YAZ "Bu dersi seçmemişsiniz."
        BİTİŞ
    BİTİŞ

    EĞER secim = 3 İSE
        YAZ "----- Kayıt Özeti -----"
        HER ders İÇİN secilen_dersler’DE
            YAZ ders.kod, ders.ad, "Kredi:", ders.kredi
        SON
        YAZ "Toplam kredi:", toplam_kredi
    BİTİŞ

UNTIL secim = 0

// 4. Danışman Onayı Kontrolü
EĞER ogrenci.GPA < 2.5 İSE
    YAZ "Düşük ortalama nedeniyle danışman onayı gereklidir."
    YAZ "Danışman onayı bekleniyor..."
    onay ← danışman_onayı_al()
    EĞER onay = RED İSE
        YAZ "Kayıt danışman tarafından reddedildi."
        DUR
    BİTİŞ
BİTİŞ

// 5. Kayıt Onayı
YAZ "Kayıt özetini onaylıyor musunuz? (E/H)"
OKU cevap
EĞER cevap = "E" İSE
    YAZ "Kayıt başarıyla tamamlandı!"
DEĞİLSE
    YAZ "Kayıt iptal edildi."
BİTİŞ

BİTİR
