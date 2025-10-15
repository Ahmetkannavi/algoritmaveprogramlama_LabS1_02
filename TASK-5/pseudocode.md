BAŞLA

// 1. Sistem Aktif mi?
EĞER sistem_durumu = "aktif" DEĞİLSE
    YAZ "Sistem pasif durumda. Lütfen sistemi etkinleştirin."
    DUR
BİTİŞ

YAZ "Sistem aktif. Sensörler izleniyor..."

// 2. Sürekli Sensör Okuma Döngüsü
TEKRAR
    // 3. Hareket Sensörü Kontrolü
    hareket ← hareket_sensörü_oku()
    
    // 4. Kapı/Pencere Sensörü Kontrolü
    kapi_pencere ← kapi_pencere_sensörü_oku()
    
    // 5. Kamera Aktivasyonu
    EĞER hareket = TRUE VEYA kapi_pencere = TRUE İSE
        kamera_aktif_et()
        YAZ "Kamera kayda başladı."
    BİTİŞ

    // 6. Yanlış Alarm Kontrolü
    EĞER ev_sahibi_evde = TRUE İSE
        YAZ "Ev sahibi evde, yanlış alarm olabilir."
        alarm_durumu ← "yanlış"
    DEĞİLSE
        alarm_durumu ← "doğru"
    BİTİŞ

    // 7. Alarm Seviyesi Belirleme
    EĞER hareket = TRUE VE kapi_pencere = FALSE İSE
        alarm_seviyesi ← 1      // Düşük seviye
    EĞER kapi_pencere = TRUE VE hareket = FALSE İSE
        alarm_seviyesi ← 2      // Orta seviye
    EĞER hareket = TRUE VE kapi_pencere = TRUE İSE
        alarm_seviyesi ← 3      // Yüksek seviye
    BİTİŞ

    // 8. Bildirim Gönderme
    EĞER alarm_durumu = "doğru" İSE
        YAZ "Uyarı! Alarm seviyesi:", alarm_seviyesi
        SMS_gonder("Evde şüpheli hareket algılandı!")
        App_bildirim_gonder("Akıllı Ev Uyarısı!", alarm_seviyesi)
        Email_gonder("Ev Güvenlik Sistemi Alarmı", "Alarm seviyesi: " + alarm_seviyesi)
    DEĞİLSE
        YAZ "Yanlış alarm tespit edildi. Bildirim gönderilmedi."
    BİTİŞ

    // 9. Alarm Sıfırlama veya Devam Ettirme
    YAZ "Alarm sıfırlansın mı? (E/H)"
    OKU cevap
    EĞER cevap = "E" İSE
        alarm_sıfırla()
        YAZ "Alarm sıfırlandı."
    DEĞİLSE
        YAZ "Sistem izlemeye devam ediyor."
    BİTİŞ

    // 10. Bekle ve Yeniden Kontrol Et
    BEKLE(5 saniye)
    YAZ "Sensörler yeniden kontrol ediliyor..."
UNTIL sistem_durumu = "kapalı"

YAZ "Sistem kapatıldı."
BİTİR
