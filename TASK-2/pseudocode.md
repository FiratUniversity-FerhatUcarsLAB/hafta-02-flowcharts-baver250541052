BAŞLA

  // Değişken ve Durum Tanımlamaları
  TANIMLA kullanici_giris_yapti_mi = HAYIR
  TANIMLA sepet = [] // Ürünleri tutacak liste
  TANIMLA sepet_toplami = 0
  TANIMLA genel_toplam = 0

  // Kullanıcı Giriş Kontrolü
  YAZ "Kullanıcı adı ve şifrenizi giriniz:"
  OKU kullanici_adi, sifre
  EĞER kullanici_adi == "dogru_kullanici" VE sifre == "dogru_sifre"
    kullanici_giris_yapti_mi = EVET
  DEĞİLSE
    YAZ "Hatalı kullanıcı adı veya şifre."
  SON-EĞER

  // Ana Alışveriş Süreci
  EĞER kullanici_giris_yapti_mi == EVET
    TANIMLA alisverise_devam = EVET
    // Ürün Ekleme Döngüsü
    DÖNGÜ (alisverise_devam == EVET)
      YAZ "Kategoriler arasında gezinin ve bir ürün seçin:"
      OKU secilen_urun
      
      // Stok Kontrolü
      EĞER secilen_urun.stok_adeti > 0
        sepet.EKLE(secilen_urun)
        YAZ secilen_urun.adi + " sepete eklendi."
      DEĞİLSE
        YAZ "Üzgünüz, bu ürün stokta kalmamıştır."
      SON-EĞER
      
      YAZ "Başka ürün eklemek istiyor musunuz? (E/H)"
      OKU cevap
      EĞER cevap == "H" VEYA cevap == "h"
        alisverise_devam = HAYIR
      SON-EĞER
    SON-DÖNGÜ
    
    // Sepet Görüntüleme ve Düzenleme Döngüsü
    TANIMLA sepeti_duzenle = EVET
    DÖNGÜ (sepeti_duzenle == EVET)
      YAZ "Sepetinizdeki ürünler: " + sepet.listele()
      HESAPLA sepet_toplami = sepet.toplam_fiyat()
      YAZ "Sepet Toplamı: " + sepet_toplami + " TL"
      YAZ "Sepeti düzenlemek veya ödemeye geçmek için seçim yapın (Düzenle/Ödeme):"
      OKU sepet_secimi
      EĞER sepet_secimi == "Düzenle"
        YAZ "Çıkarmak istediğiniz ürünü girin veya miktarı güncelleyin."
        // Düzenleme işlemleri burada yapılır
      DEĞİLSE
        sepeti_duzenle = HAYIR
      SON-EĞER
    SON-DÖNGÜ

    // İndirim Kodu Kontrolü
    YAZ "İndirim kodunuz var mı? (E/H)"
    OKU indirim_cevabi
    EĞER indirim_cevabi == "E" VEYA indirim_cevabi == "e"
      YAZ "İndirim kodunu giriniz:"
      OKU girilen_kod
      EĞER girilen_kod == "INDIRIM25" // Örnek geçerli kod
        sepet_toplami = sepet_toplami * 0.75 // %25 indirim uygula
        YAZ "İndirim uygulandı. Yeni sepet toplamı: " + sepet_toplami + " TL"
      DEĞİLSE
        YAZ "Geçersiz indirim kodu."
      SON-EĞER
    SON-EĞER
    
    // Minimum Tutar Kontrolü
    EĞER sepet_toplami < 50
      YAZ "Minimum sipariş tutarı 50 TL'dir. Lütfen sepetinize ürün ekleyin."
    DEĞİLSE
      // Kargo Ücreti Hesaplama
      TANIMLA kargo_ucreti = 15 // Varsayılan kargo ücreti
      EĞER sepet_toplami >= 200
        kargo_ucreti = 0
        YAZ "200 TL ve üzeri alışverişlerde kargo ücretsiz!"
      DEĞİLSE
        YAZ "Kargo ücreti: " + kargo_ucreti + " TL"
      SON-EĞER
      genel_toplam = sepet_toplami + kargo_ucreti
      YAZ "Ödenecek Toplam Tutar: " + genel_toplam + " TL"
      
      // Ödeme Yöntemi Seçimi
      YAZ "Lütfen bir ödeme yöntemi seçiniz (Kredi Kartı / Havale):"
      OKU odeme_yontemi
      EĞER odeme_yontemi == "Kredi Kartı" VEYA odeme_yontemi == "Havale"
        YAZ "Ödeme işlemi gerçekleştiriliyor..."
        YAZ "SİPARİŞİNİZ ONAYLANDI!"
      DEĞİLSE
        YAZ "Geçersiz ödeme yöntemi seçtiniz."
      SON-EĞER
    SON-EĞER
  SON-EĞER

BİTİR
