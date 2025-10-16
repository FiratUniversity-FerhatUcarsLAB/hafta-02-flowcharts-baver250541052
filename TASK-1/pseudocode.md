BAŞLA

  // Değişken Tanımlamaları
  TANIMLA pin_deneme_hakki = 3
  TANIMLA dogru_pin = 1234 // Örnek PIN
  TANIMLA bakiye = 1500   // Örnek Bakiye
  TANIMLA gunluk_limit = 1000 // Örnek Günlük Limit
  TANIMLA pin_dogru_mu = HAYIR

  // PIN Doğrulama Döngüsü
  DÖNGÜ (pin_deneme_hakki > 0 VE pin_dogru_mu == HAYIR)
    YAZ "Lütfen 4 haneli PIN kodunuzu giriniz:"
    OKU girilen_pin

    EĞER girilen_pin == dogru_pin
      pin_dogru_mu = EVET
    DEĞİLSE
      pin_deneme_hakki = pin_deneme_hakki - 1
      EĞER pin_deneme_hakki > 0
        YAZ "Hatalı PIN. Kalan deneme hakkınız: " + pin_deneme_hakki
      DEĞİLSE
        YAZ "3 hatalı giriş yaptınız. Kartınız bloke olmuştur."
      SON-EĞER
    SON-EĞER
  SON-DÖNGÜ

  // Ana İşlem Döngüsü
  EĞER pin_dogru_mu == EVET
    TANIMLA baska_islem = EVET
    DÖNGÜ (baska_islem == EVET)
      YAZ "Mevcut Bakiyeniz: " + bakiye + " TL"
      YAZ "Çekmek istediğiniz tutarı giriniz:"
      OKU cekilecek_tutar

      // Bakiye Kontrolü
      EĞER cekilecek_tutar > bakiye
        YAZ "Yetersiz bakiye."
      DEĞİLSE
        // 20 TL Katları Kontrolü
        EĞER cekilecek_tutar % 20 != 0
          YAZ "Lütfen 20 TL ve katları bir tutar giriniz."
        DEĞİLSE
          // Günlük Limit Kontrolü
          EĞER cekilecek_tutar > gunluk_limit
            YAZ "Günlük para çekme limitini aştınız. Çekebileceğiniz maksimum tutar: " + gunluk_limit + " TL"
          DEĞİLSE
            // İşlemi Gerçekleştir
            bakiye = bakiye - cekilecek_tutar
            gunluk_limit = gunluk_limit - cekilecek_tutar
            YAZ "Lütfen paranızı alınız."
            YAZ "Fişiniz yazdırılıyor."
            YAZ "Kalan Bakiye: " + bakiye + " TL"
            YAZ "Kalan Günlük Limit: " + gunluk_limit + " TL"
          SON-EĞER
        SON-EĞER
      SON-EĞER

      YAZ "Başka bir işlem yapmak ister misiniz? (E/H)"
      OKU cevap
      EĞER cevap == "H" VEYA cevap == "h"
        baska_islem = HAYIR
      SON-EĞER
    SON-DÖNGÜ
    YAZ "İyi günler dileriz."
  SON-EĞER

BİTİR
