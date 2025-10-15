BAŞLA
    YAZ "Hastane Randevu ve Tahlil Sistemine Hoş Geldiniz."
    YAZ "Lütfen TC Kimlik Numaranızı giriniz:"
    OKU tc_no

    EĞER tc_no geçerli formatta İSE
        YAZ "Kimlik doğrulandı."
    DEĞİLSE
        YAZ "Hatalı TC Kimlik No! Lütfen tekrar deneyiniz."
        GİT BAŞA
    BİTİR EĞER

    DÖNGÜ kullanıcı çıkış yapmak isteyene kadar
        YAZ "Lütfen yapmak istediğiniz işlemi seçiniz:"
        YAZ "1 - Randevu Al"
        YAZ "2 - Tahlil Sonucu Gör"
        YAZ "3 - Çıkış"
        OKU secim

        EĞER secim = 1 İSE
            // RANDEVU MODÜLÜ
            YAZ "Poliklinik listesini görüntülüyorsunuz."
            YAZ "Lütfen bir poliklinik seçiniz:"
            OKU poliklinik

            YAZ "Seçilen polikliniğe ait doktor listesi:"
            doktor_listesini_göster(poliklinik)

            YAZ "Lütfen doktor seçiniz:"
            OKU doktor

            YAZ "Uygun saatler listeleniyor..."
            DÖNGÜ uygun saatler bitene kadar
                uygun_saatleri_göster(doktor)
                YAZ "Bir saat seçiniz:"
                OKU saat
                EĞER saat uygun İSE
                    YAZ "Randevunuz oluşturuluyor..."
                    randevu_kaydet(tc_no, doktor, saat)
                    YAZ "Randevunuz başarıyla oluşturuldu!"
                    YAZ "SMS ile bilgilendirme gönderildi."
                    ÇIK
                DEĞİLSE
                    YAZ "Bu saat dolu, lütfen başka bir saat seçiniz."
                BİTİR EĞER
            DÖNGÜ SONU

        DEĞİLSE EĞER secim = 2 İSE
            // TAHLİL MODÜLÜ
            YAZ "Tahlil sonuçları kontrol ediliyor..."
            EĞER hastanın_tahlili_var_mı(tc_no) İSE
                YAZ "Tahlil bulundu. Hazır mı kontrol ediliyor..."
                EĞER tahlil_hazır_mı(tc_no) İSE
                    YAZ "Tahlil sonuçlarınız hazır!"
                    tahlil_sonuçlarını_göster(tc_no)
                    YAZ "Sonuçları PDF olarak indirmek ister misiniz? (E/H)"
                    OKU indirme
                    EĞER indirme = "E" İSE
                        pdf_indir(tc_no)
                        YAZ "PDF başarıyla indirildi."
                    DEĞİLSE
                        YAZ "İndirme işlemi iptal edildi."
                    BİTİR EĞER
                DEĞİLSE
                    YAZ "Tahlil sonuçlarınız henüz hazır değil. Lütfen daha sonra tekrar deneyiniz."
                BİTİR EĞER
            DEĞİLSE
                YAZ "Sistemde tahlil kaydınız bulunmamaktadır."
            BİTİR EĞER

        DEĞİLSE EĞER secim = 3 İSE
            YAZ "Sistemden çıkılıyor..."
            ÇIK
        DEĞİLSE
            YAZ "Geçersiz seçim! Lütfen 1, 2 veya 3 giriniz."
        BİTİR EĞER

        YAZ "Başka bir işlem yapmak ister misiniz? (E/H)"
        OKU devam
        EĞER devam = "H" İSE
            ÇIK
        BİTİR EĞER
    DÖNGÜ SONU

    YAZ "İyi günler dileriz. Sağlıklı kalın!"
BİTİR
