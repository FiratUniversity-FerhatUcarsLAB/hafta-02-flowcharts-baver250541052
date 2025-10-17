BAŞLA

  // Ana Sistem Döngüsü - Bu döngü sürekli çalışır
  DÖNGÜ (DOĞRU)
  
    // Sistemin Aktif Olup Olmadığının Kontrolü
    EĞER sistem_aktif_mi
      // --- SENSÖR OKUMA VE KONTROL BAŞLANGICI ---
      
      // Sensörlerden verileri anlık olarak oku
      OKU hareket_sensoru_durumu
      OKU kapi_pencere_sensoru_durumu
      
      // Tehdit Algılama
      EĞER hareket_sensoru_durumu == "HAREKET ALGILANDI" VEYA kapi_pencere_sensoru_durumu == "AÇIK"
      
        // Yanlış Alarm Kontrolü
        EĞER ev_sahibi_evde_mi == HAYIR
        
          // Gerçek Tehdit Durumu Aksiyonları
          İŞLEM: Kamera sistemini aktive et ve kayda başla.
          
          // Alarm Seviyesi Belirleme
          TANIMLA alarm_seviyesi = 0
          EĞER hareket_sensoru_durumu == "HAREKET ALGILANDI" VE kapi_pencere_sensoru_durumu == "AÇIK"
            alarm_seviyesi = 3 // Yüksek Seviye
          DEĞİLSE EĞER hareket_sensoru_durumu == "HAREKET ALGILANDI"
            alarm_seviyesi = 2 // Orta Seviye
          DEĞİLSE
            alarm_seviyesi = 1 // Düşük Seviye (Sadece kapı/pencere)
          SON-EĞER
          
          // Alarm ve Bildirim Gönderme
          İŞLEM: Sireni çal (Seviye: " + alarm_seviyesi + ")"
          İŞLEM: Bildirim Gönder (SMS, Uygulama Bildirimi, E-posta)
          
          // Alarm Sıfırlama Bekleme Döngüsü
          TANIMLA alarm_devam_ediyor = EVET
          DÖNGÜ (alarm_devam_ediyor == EVET)
            YAZ "Alarm aktif. Sıfırlama komutu bekleniyor..."
            OKU gelen_komut
            EĞER gelen_komut == "SIFIRLA"
              alarm_devam_ediyor = HAYIR
              İŞLEM: Sireni durdur.
            SON-EĞER
            BEKLE (5 saniye) // Sıfırlama komutunu beklerken kısa aralıklarla kontrol et
          SON-DÖNGÜ
          
        SON-EĞER // Yanlış alarm kontrolü sonu
        
      SON-EĞER // Tehdit algılama sonu
      
      // --- SENSÖR OKUMA VE KONTROL SONU ---
      
    SON-EĞER // Sistem aktif mi kontrolü sonu
    
    BEKLE (1 saniye) // Ana döngünün her tekrarı arasında kısa bir bekleme süresi
    
  SON-DÖNGÜ

BİTİR // Not: Sonsuz döngüden dolayı bu adıma teorik olarak ulaşılamaz.
