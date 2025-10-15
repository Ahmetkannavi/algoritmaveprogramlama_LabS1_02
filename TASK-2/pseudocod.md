BAŞLA
    YAZ "Online Alışveriş Sistemine Hoş Geldiniz."
    YAZ "Lütfen kullanıcı adınızı ve şifrenizi giriniz."
    OKU kullanıcı_adı, şifre

    EĞER kullanıcı_adı ve şifre doğru İSE
        YAZ "Giriş başarılı! Hoş geldiniz."
        
        DÖNGÜ ürün_kategorisi seçilene kadar
            YAZ "Lütfen bir ürün kategorisi seçiniz (Elektronik, Giyim, Ev vb.)"
            OKU kategori
            YAZ kategori içindeki ürünleri listele
            
            YAZ "Sepete eklemek istediğiniz ürünün adını yazınız veya 'çıkış' yazarak menüye dönünüz."
            OKU ürün
            
            EĞER ürün ≠ "çıkış" İSE
                EĞER stokta ürün mevcut İSE
                    YAZ "Ürün sepete eklendi."
                    sepete_ekle(ürün)
                DEĞİLSE
                    YAZ "Stokta yeterli ürün yok!"
                BİTİR EĞER
            DEĞİLSE
                YAZ "Kategori menüsüne dönülüyor..."
            BİTİR EĞER
        DÖNGÜ SONU

        DÖNGÜ kullanıcı sepeti düzenlemek istedikçe
            YAZ "Sepetinizdeki ürünler:"
            sepeti_görüntüle()
            YAZ "Ürün silmek istiyor musunuz? (E/H)"
            OKU cevap
            EĞER cevap = "E" İSE
                YAZ "Silmek istediğiniz ürünün adını yazınız:"
                OKU silinecek
                sepetten_sil(silinecek)
            DEĞİLSE
                ÇIK
            BİTİR EĞER
        DÖNGÜ SONU

        YAZ "İndirim kodunuz var mı? (E/H)"
        OKU indirim_var
        EĞER indirim_var = "E" İSE
            OKU indirim_kodu
            EĞER indirim_kodu geçerli İSE
                indirimi_uygula()
            DEĞİLSE
                YAZ "Geçersiz indirim kodu!"
            BİTİR EĞER
        BİTİR EĞER

        toplam_tutar = sepet_toplamı()
        EĞER toplam_tutar < 50 TL İSE
            YAZ "Minimum alışveriş tutarı 50 TL olmalıdır!"
            YAZ "Lütfen sepetinize daha fazla ürün ekleyiniz."
            GİT ürün_ekleme_adımına
        BİTİR EĞER

        EĞER toplam_tutar > 200 TL İSE
            kargo_ücreti = 0
            YAZ "Kargo ücretsiz!"
        DEĞİLSE
            kargo_ücreti = 30
            YAZ "Kargo ücreti 30 TL olarak eklendi."
        BİTİR EĞER

        YAZ "Ödeme yöntemini seçiniz: (1) Kredi Kartı (2) Havale (3) Kapıda Ödeme"
        OKU ödeme_yöntemi

        EĞER ödeme_yöntemi = 1 İSE
            YAZ "Kart bilgilerinizi giriniz."
        DEĞİLSE EĞER ödeme_yöntemi = 2 İSE
            YAZ "IBAN bilgisi ekranda gösteriliyor."
        DEĞİLSE
            YAZ "Kapıda ödeme seçildi."
        BİTİR EĞER

        YAZ "Siparişi onaylıyor musunuz? (E/H)"
        OKU onay
        EĞER onay = "E" İSE
            YAZ "Siparişiniz başarıyla oluşturuldu. Teşekkür ederiz!"
        DEĞİLSE
            YAZ "Sipariş iptal edildi."
        BİTİR EĞER

    DEĞİLSE
        YAZ "Giriş başarısız! Kullanıcı bilgilerinizi kontrol ediniz."
    BİTİR EĞER
BİTİR
