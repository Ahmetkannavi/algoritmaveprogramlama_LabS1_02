digraph ATM {
  rankdir=TB;
  node [fontname="Helvetica", fontsize=10];
  edge [fontname="Helvetica", fontsize=9];

  Start [shape=oval, label="BAŞLA\n(Kart tak)"];
  ReadCard [shape=box, label="Kart oku\n(karte bilgisi al)"];
  CardErr [shape=box, color=red, label="Kart okunamadı\nİşlem sonlandır"];
  GetAccount [shape=box, label="Hesap bilgilerini al\n(bakiye,günlük limit,çekilen)"];

  PIN_LOOP [shape=diamond, label="PIN doğru mu?\n(3 deneme)"];
  PIN_Input [shape=box, label="PIN gir"];
  PIN_Incorrect [shape=box, label="Yanlış PIN\n(kalan hak göster)"];
  CardBlock [shape=box, color=red, label="3 başarısız -> Kart bloke\nİşlem sonlandır"];

  MainLoop [shape=diamond, label="Yeni işlem ister misiniz?"];
  GetAmount [shape=box, label="Tutar gir (20 TL katları)"];
  CancelCheck [shape=diamond, label="İptal mi?"];
  AmountPositive [shape=diamond, label="Tutar > 0?"];
  MultipleOf20 [shape=diamond, label="20 TL'nin katı mı?"];
  DailyLimitCheck [shape=diamond, label="Günlük limit aşılıyor mu?"];
  BalanceCheck [shape=diamond, label="Bakiye yeterli mi?"];
  Confirm [shape=diamond, label="Onaylıyor musunuz?"];

  Dispense [shape=box, style=filled, fillcolor="#e0ffe0", label="Para ver\nHesabı güncelle\nFiş yazdır"];
  Cancelled [shape=box, color=orange, label="İşlem iptal edildi"];
  End [shape=oval, label="SON\nKartını al"];

  // Akış
  Start -> ReadCard;
  ReadCard -> CardErr [label="okunamadı", color=red, style=dashed];
  ReadCard -> GetAccount;

  GetAccount -> PIN_Input;
  PIN_Input -> PIN_LOOP;
  PIN_LOOP -> PIN_Incorrect [label="Hayır", color=red];
  PIN_LOOP -> MainLoop [label="Evet", color=green];

  PIN_Incorrect -> PIN_Input [label="Hak kaldıysa"];
  PIN_Incorrect -> CardBlock [label="Hak bitti", color=red];
  CardBlock -> End;

  MainLoop -> GetAmount [label="Evet"];
  MainLoop -> End [label="Hayır"];

  GetAmount -> CancelCheck;
  CancelCheck -> Cancelled [label="Evet"];
  CancelCheck -> AmountPositive [label="Hayır"];

  Cancelled -> MainLoop [label="Yeni işlem? (E/H)"];

  AmountPositive -> MultipleOf20 [label="Evet"];
  AmountPositive -> Cancelled [label="Hayır"];

  MultipleOf20 -> DailyLimitCheck [label="Evet"];
  MultipleOf20 -> Cancelled [label="Hayır"];

  DailyLimitCheck -> BalanceCheck [label="Limit aşılmıyor"];
  DailyLimitCheck -> Cancelled [label="Limit aşılıyor"];

  BalanceCheck -> Confirm [label="Evet"];
  BalanceCheck -> Cancelled [label="Hayır (yetersiz bakiye)"];

  Confirm -> Dispense [label="Evet"];
  Confirm -> Cancelled [label="Hayır"];

  Dispense -> MainLoop [label="Başka işlem?"];
  Cancelled -> End [label="Kartı al"];
}
