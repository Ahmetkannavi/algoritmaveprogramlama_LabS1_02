digraph HastaneSistemi {
    rankdir=TB;
    node [shape=rectangle, style=rounded, fontname="Arial"];

    // --- Modül 1: Randevu Alma ---
    StartR [label="BAŞLA: Randevu Alma"];
    KimlikR [label="Kimlik doğrulama"];
    KontrolKimlikR [shape=diamond,label="Kimlik geçerli mi?"];
    Poliklinik [label="Poliklinik seçimi"];
    Doktor [label="Doktor listesi görüntüle"];
    Saat [label="Uygun saatleri göster"];
    Onay [label="Randevuyu onayla"];
    SMS [label="SMS gönder"];
    MesajOK_R [label="Randevu başarıyla oluşturuldu"];
    MesajNO_R [label="Kimlik doğrulama başarısız"];

    // Akış Modül 1
    StartR -> KimlikR -> KontrolKimlikR;
    KontrolKimlikR -> Poliklinik [label="EVET"];
    KontrolKimlikR -> MesajNO_R [label="HAYIR"];
    Poliklinik -> Doktor -> Saat -> Onay -> SMS -> MesajOK_R;

    // --- Modül 2: Tahlil Sonuçları ---
    StartT [label="BAŞLA: Tahlil Sonuçları"];
    KimlikT [label="Kimlik doğrulama"];
    KontrolKimlikT [shape=diamond,label="Kimlik geçerli mi?"];
    TahlilVar [shape=diamond,label="Tahlil mevcut mu?"];
    SonucHazir [shape=diamond,label="Sonuç hazır mı?"];
    Goruntule [label="Sonuç görüntüle"];
    PDF [label="PDF indir"];
    MesajHazirDegil [label="Sonuçlar henüz hazır değil"];
    MesajYok [label="Tahlil bulunamadı"];
    MesajNO_T [label="Kimlik doğrulama başarısız"];

    // Akış Modül 2
    StartT -> KimlikT -> KontrolKimlikT;
    KontrolKimlikT -> TahlilVar [label="EVET"];
    KontrolKimlikT -> MesajNO_T [label="HAYIR"];
    TahlilVar -> SonucHazir [label="EVET"];
    TahlilVar -> MesajYok [label="HAYIR"];
    SonucHazir -> Goruntule [label="EVET"];
    SonucHazir -> MesajHazirDegil [label="HAYIR"];
    Goruntule -> PDF;

    // --- Bitiş Noktaları ---
    MesajOK_R -> End [shape=oval,label="BİTİR"];
    MesajNO_R -> End;
    PDF -> End;
    MesajHazirDegil -> End;
    MesajYok -> End;
    MesajNO_T -> End;
}
