digraph EticaretAkis {
  rankdir=TB;
  node [fontname="Arial"];

  start [shape=oval,label="BAŞLA"];
  login [shape=box,label="Kullanıcı Girişi (3 hak)"];
  login_check [shape=diamond,label="Giriş doğru mu?"];
  start_cart [shape=box,label="Sepet Başlat"];
  add_loop [shape=box,label="Ürün Ekleme Döngüsü"];
  stok_check [shape=diamond,label="Stok yeterli mi?"];
  add_cart [shape=box,label="Sepete Ekle / Rezerve Et"];
  stok_insuff [shape=box,label="Yetersiz stok - Uyarı"];
  continue_check [shape=diamond,label="Devam edilsin mi?"];
  discount_q [shape=diamond,label="İndirim kodu var mı?"];
  discount_check [shape=diamond,label="Kod geçerli mi?"];
  calc_shipping [shape=box,label="Kargo Hesapla (ağırlık, adres)"];
  show_total [shape=box,label="Toplamı göster"];
  payment_choice [shape=diamond,label="Ödeme yöntemi seçimi"];
  pay_cc [shape=box,label="Kredi Kartı bilgileri al"];
  card_valid [shape=diamond,label="Kart bilgileri geçerli mi?"];
  gateway [shape=diamond,label="Ödeme ağ geçidi onayladı mı?"];
  pay_bank [shape=box,label="Havale: Onay bekle"];
  pay_cod [shape=box,label="Kapıda ödeme seçildi"];
  payment_success [shape=box,label="Ödeme başarılı"];
  payment_failed [shape=box,label="Ödeme/Doğrulama hatası"];
  order_create [shape=box,label="Sipariş oluştur ve stok kesinleştir"];
  order_cod [shape=box,label="Sipariş oluştur (Kapıda)"];
  rollback [shape=box,label="Stok iade et, sipariş iptal"];
  end [shape=oval,label="BİTİR"];

  // Akış
  start -> login -> login_check;
  login_check -> start_cart [label="EVET"];
  login_check -> end [label="HAYIR (3 hak bitti)"];

  start_cart -> add_loop;
  add_loop -> stok_check;
  stok_check -> add_cart [label="EVET"];
  stok_check -> stok_insuff [label="HAYIR"];
  stok_insuff -> continue_check;
  add_cart -> continue_check;
  continue_check -> add_loop [label="EVET"];
  continue_check -> discount_q [label="HAYIR"];

  discount_q -> discount_check [label="EVET"];
  discount_q -> calc_shipping [label="HAYIR"];
  discount_check -> calc_shipping [label="GEÇERLİ / EVET"];
  discount_check -> calc_shipping [label="GEÇERSİZ / HAYIR"];

  calc_shipping -> show_total -> payment_choice;

  // Ödeme dallanması
  payment_choice -> pay_cc [label="KrediKartı"];
  payment_choice -> pay_bank [label="Havale"];
  payment_choice -> pay_cod [label="Kapıda"];

  // Kredi kartı akışı
  pay_cc -> card_valid;
  card_valid -> gateway [label="EVET"];
  card_valid -> payment_failed [label="HAYIR"];
  gateway -> payment_success [label="EVET"];
  gateway -> payment_failed [label="HAYIR"];

  // Havale akışı
  pay_bank -> payment_success [label="Havale onaylandı / EVET"];
  pay_bank -> payment_failed [label="Onay yok / HAYIR"];

  // Kapıda
  pay_cod -> order_cod;

  // Sonuç işlemleri
  payment_success -> order_create;
  order_create -> end;
  order_cod -> end;
  payment_failed -> rollback -> end;
}
