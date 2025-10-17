BAŞLA

  // Değişken ve Durum Tanımlamaları
  TANIMLA ogrenci_giris_yapti_mi = HAYIR
  TANIMLA secilen_dersler = []
  TANIMLA toplam_kredi = 0
  TANIMLA kredi_limiti = 35
  TANIMLA ogrenci_gpa = 2.4 // Örnek GPA değeri

  // Öğrenci Girişi
  YAZ "Öğrenci No ve Şifrenizi Giriniz:"
  OKU ogrenci_no, sifre
  EĞER ogrenci_no ve sifre doğru
    ogrenci_giris_yapti_mi = EVET
  DEĞİLSE
    YAZ "Hatalı giriş bilgileri."
  SON-EĞER

  // Ana Kayıt Süreci
  EĞER ogrenci_giris_yapti_mi == EVET
    TANIMLA kayit_devam_ediyor = EVET
    // Ders Ekleme/Çıkarma Döngüsü
    DÖNGÜ (kayit_devam_ediyor == EVET)
      YAZ "Ders Listesi: [Ders1, Ders2, Ders3, ...]"
      YAZ "Mevcut Sepetiniz: " + secilen_dersler.listele()
      YAZ "Toplam Kredi: " + toplam_kredi
      YAZ "İşlem Seçin (Ekle / Çıkar / Tamamla):"
      OKU islem
      
      EĞER islem == "Ekle"
        YAZ "Eklemek istediğiniz dersin kodunu girin:"
        OKU secilen_ders
        
        // --- KONTROL MEKANİZMALARI BAŞLANGICI ---
        // Kontenjan Kontrolü
        EĞER secilen_ders.kontenjan > secilen_ders.kayitli_ogrenci_sayisi
          // Ön Koşul Kontrolü
          EĞER ogrenci_on_kosulu_sagliyor_mu(secilen_ders)
            // Zaman Çakışması Kontrolü
            EĞER zaman_cakismasi_yok_mu(secilen_ders, secilen_dersler)
              // Kredi Limiti Kontrolü
              EĞER (toplam_kredi + secilen_ders.kredi) <= kredi_limiti
                secilen_dersler.EKLE(secilen_ders)
                toplam_kredi = toplam_kredi + secilen_ders.kredi
                YAZ secilen_ders.adi + " başarıyla eklendi."
              DEĞİLSE
                YAZ "Kredi limiti (35 AKTS) aşıldı."
              SON-EĞER
            DEĞİLSE
              YAZ "Zaman çakışması! Ders eklenemedi."
            SON-EĞER
          DEĞİLSE
            YAZ "Bu dersin ön koşulunu sağlamıyorsunuz."
          SON-EĞER
        DEĞİLSE
          YAZ "Ders kontenjanı dolu."
        SON-EĞER
        // --- KONTROL MEKANİZMALARI SONU ---
        
      DEĞİLSE EĞER islem == "Çıkar"
        YAZ "Çıkarmak istediğiniz dersin kodunu girin:"
        OKU cikarilacak_ders
        secilen_dersler.CIKAR(cikarilacak_ders)
        toplam_kredi = toplam_kredi - cikarilacak_ders.kredi
        YAZ cikarilacak_ders.adi + " sepetten çıkarıldı."
        
      DEĞİLSE EĞER islem == "Tamamla"
        kayit_devam_ediyor = HAYIR
      SON-EĞER
    SON-DÖNGÜ
    
    // Kayıt Onay Aşaması
    YAZ "KAYIT ÖZETİ:"
    YAZ "Seçilen Dersler: " + secilen_dersler.listele()
    YAZ "Toplam Kredi: " + toplam_kredi
    
    // Danışman Onayı Kontrolü
    EĞER ogrenci_gpa < 2.5
      YAZ "Not ortalamanız 2.5'in altında olduğu için kaydınız danışman onayına gönderilmiştir."
      İŞLEM: Kaydı onaya gönder.
    DEĞİLSE
      YAZ "Kaydınızı onaylıyor musunuz? (E/H)"
      OKU onay
      EĞER onay == "E"
        YAZ "DERS KAYDINIZ BAŞARIYLA ONAYLANMIŞTIR."
      DEĞİLSE
        YAZ "Kayıt işlemi iptal edildi."
      SON-EĞER
    SON-EĞER
  SON-EĞER

BİTİR
