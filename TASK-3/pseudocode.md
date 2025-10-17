BAŞLA

  // Kimlik Doğrulama
  YAZ "Lütfen TC Kimlik Numaranızı giriniz:"
  OKU girilen_tc
  EĞER girilen_tc geçerli_mi
    YAZ "Kimlik doğrulama başarılı."
    
    // Ana İşlem Döngüsü
    TANIMLA baska_islem = EVET
    DÖNGÜ (baska_islem == EVET)
      YAZ "Lütfen yapmak istediğiniz işlemi seçiniz:"
      YAZ "1 - Randevu Al"
      YAZ "2 - Tahlil Sonucu Görüntüle"
      OKU islem_secimi
      
      // İşlem Seçimi Kontrolü
      EĞER islem_secimi == 1
        // --- Randevu Modülü Başlangıcı ---
        // (Modül 1'deki pseudocode buraya entegre edilir)
        YAZ "Poliklinik seçiniz..."
        // ... (ilgili tüm adımlar) ...
        YAZ "Randevu onayı ve SMS gönderimi tamamlandı."
        // --- Randevu Modülü Sonu ---
        
      DEĞİLSE EĞER islem_secimi == 2
        // --- Tahlil Modülü Başlangıcı ---
        // (Modül 2'deki pseudocode buraya entegre edilir)
        EĞER hastanin_kayitli_tahlili_var_mi
          // ... (ilgili tüm adımlar) ...
          YAZ "Sonuçlar görüntülendi."
        DEĞİLSE
          YAZ "Kayıtlı tahlil bulunamadı."
        SON-EĞER
        // --- Tahlil Modülü Sonu ---
        
      DEĞİLSE
        YAZ "Geçersiz bir seçim yaptınız."
      SON-EĞER
      
      // Başka İşlem Sorgusu
      YAZ "Başka bir işlem yapmak ister misiniz? (E/H)"
      OKU devam_cevabi
      EĞER devam_cevabi == "H" VEYA devam_cevabi == "h"
        baska_islem = HAYIR
      SON-EĞER
    SON-DÖNGÜ
    
  DEĞİLSE
    YAZ "Geçersiz TC Kimlik Numarası. Sistemden çıkılıyor."
  SON-EĞER
  
  YAZ "İyi günler dileriz."
BİTİR
