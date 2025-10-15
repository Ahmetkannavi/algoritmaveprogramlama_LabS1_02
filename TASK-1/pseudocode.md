BAŞLA

  # 1. Kart takma işlemi
  YAZ "Lütfen kartınızı takınız..."
  OKU kart

  EĞER kart = HATALI İSE
      YAZ "Kart okunamadı! Lütfen tekrar deneyiniz."
      SON
  SON

  # 2. PIN doğrulama (3 hak)
  deneme = 0
  PIN_DOGRULANDI = HAYIR

  DÖNGÜ (deneme < 3) BAŞ
      YAZ "Lütfen 4 haneli PIN kodunuzu giriniz:"
      OKU girilenPIN

      EĞER girilenPIN = DOGRU_PIN İSE
          PIN_DOGRULANDI = EVET
          ÇIKIŞ_DÖNGÜ
      DEĞİLSE
          deneme = deneme + 1
          KALAN = 3 - deneme
          EĞER KALAN > 0 İSE
              YAZ "Hatalı PIN! Kalan deneme hakkı: " + KALAN
          DEĞİLSE
              YAZ "3 kez hatalı giriş yaptınız. Kartınız bloke edildi."
              SON
          SON
      SON
  SON DÖNGÜ

  # 3. PIN doğruysa devam et
  EĞER PIN_DOGRULANDI = EVET İSE

      bakiye = 1500
      gunlukLimit = 2000
      gunlukCekilen = 0

      islemDevam = EVET

      DÖNGÜ (islemDevam = EVET) BAŞ
          # 4. Bakiye sorgulama
          YAZ "Mevcut bakiyeniz: " + bakiye + " TL"

          # 5. Çekilmek istenen tutarı al
          YAZ "Çekmek istediğiniz tutarı giriniz (20 TL katları):"
          OKU tutar

          # 6. Yetersiz bakiye kontrolü
          EĞER tutar > bakiye İSE
              YAZ "Yetersiz bakiye! İşlem iptal edildi."
          DEĞİLSE

              # 7. 20 TL’nin katı mı?
              EĞER tutar MOD 20 <> 0 İSE
                  YAZ "Tutar 20 TL’nin katı olmalıdır!"
              DEĞİLSE

                  # 8. Günlük limit kontrolü
                  EĞER (gunlukCekilen + tutar) > gunlukLimit İSE
                      YAZ "Günlük limit aşılıyor! Limit: " + gunlukLimit + " TL"
                  DEĞİLSE
                      # 9. Para verme işlemi
                      bakiye = bakiye - tutar
                      gunlukCekilen = gunlukCekilen + tutar
                      YAZ tutar + " TL veriliyor..."
                      YAZ "Lütfen paranızı ve fişinizi alınız."
                  SON
              SON
          SON

          # 10. Başka işlem seçeneği
          YAZ "Başka bir işlem yapmak ister misiniz? (E/H)"
          OKU cevap
          EĞER cevap = "E" İSE
              islemDevam = EVET
          DEĞİLSE
              islemDevam = HAYIR
              YAZ "Kartınızı alınız. İyi günler!"
              SON
          SON
      SON DÖNGÜ

  DEĞİLSE
      YAZ "PIN doğrulanamadı. İşlem sonlandırıldı."
  SON

SON
