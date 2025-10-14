İsim - Soy isim : ahmed kannavi
Öğrenci No: 240541603

sistemin kısa açıklamas:
Bu Graphviz diyagramı, **ATM para çekme sisteminin mantıksal işleyişini** adım adım gösterir.
İşlem, kullanıcının kartını takmasıyla başlar; sistem kartı okur ve **PIN doğrulaması** yapar.
Kullanıcıya üç deneme hakkı verilir; yanlış girişlerde uyarı yapılır, üç başarısız denemede kart bloke edilir.
PIN doğrulandıktan sonra kullanıcıdan çekmek istediği tutar alınır.
Girilen tutar **20 TL’nin katı**, **pozitif**, **bakiye** ve **günlük limit** sınırlarına göre kontrol edilir.
Koşullar uygunsa sistem kullanıcıdan onay alır, parayı verir ve bakiyeyi günceller.
İşlem sonunda kullanıcıya “başka işlem yapmak ister misiniz?” seçeneği sunulur;
eğer hayır derse kart iade edilir ve süreç **SON** adımıyla tamamlanır.

