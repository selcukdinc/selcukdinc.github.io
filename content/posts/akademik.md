+++
title = 'Akademik 101'
date = 2025-10-12T18:51:01+03:00
draft = false
ShowToc = true
+++

Yazının amacı akademik alanda kullanılacak bilgileri bir noktada toplamaktır. 

## Araştırma

Bu başlık altında literatür taraması nasıl verimli yapılır, incelenecektir.

### Akademik Arama Motorları 

Popüler bir çok arma motorları mevcuttur. 

- Scholar: Google'ın akademik araştırma motoru scholar bir çok veritabanında arama yapar. [Scholar](scholar.google.com) `scholar.google.com`

- IEEE Xplore : Elektrik Elektronik alanında makaleler yoğunlukla burada yayınlanır [IEEXplore](https://ieeexplore.ieee.org)

### Arama Nasıl Yapılır?

Her iki motorda da geçerli operatörler mevcuttur.

- `"Tırnak İşareti"`: Tırnak işareti içindeki ifadeyi birebir arar. En önemli ve sık kullanılan operatördür.
    - Örnek: `"Analysis Pomputer Power"` araması yapıldığında kelime sırasına göre arama yapar.

- `AND`: İki anahtar kelimeninde dokümanda yer almasını zorunlu kılar. Bu operatör aramamızı daraltır ve spesifik makaleyi bulmamızı kolaylaştırır. 
    - Örnek: `"ESP32" AND "Wi-Fi" AND "security"` 

- `OR`: Eş anlamlı anahtar kelimeler için ideal bir operatördür. Arama sonuçlarını genişletir. 
    - Örnek: `"wearable device" OR "giyilebilir cihaz"` bu örnekte birden fazla dilde benzer makalelerin araması hedeflenmektedir.

- `NOT`veya `-`: Bu operatör ise belirlenen anahtar kelimeyi makale dışında tutmaya yarar.
    - Örnek: `"Bluetooth" NOT "classic"` aramasında yalnızca güncel teknoloji olan BLE teknolojisi ile ilgili makaleler taranır. (yalnızca `"Bluetooth"` araması yapılırsa 1.210.00 adet makale çıkarken `"Bluetooth" NOT "classic"` aramasında yalnızca 40.400 adet makale çıkmaktadır.)

- `*`: Yıldız operatörü ise kelimenin kökünden sonra eklenir ve kelimenin alabileceği kökleri arama içine katar.
    - Örnek: `"communicat*"` kelimesi `communication`, `communicate`, `communicating` gibi sonuçları dahil ederek aramayı genişletmeye yarar.

#### Araştırma Stratejisi

Scholar ve IEEEXplore için araştırma stratejileri eklenmiştir.

##### IEEE Xplore için arama stratejisi

IEEE Xplore içinde gelişmiş arama bölümü bulunmaktadır. burada şöyle bir yol izlenebilir

- `Abstract` alanında `("ESP32" OR "ESP-WROOM-32")`
- `AND`
- `Abstract` alanında `("Wi-Fi" AND ("BLE" OR "Bluetooth Low Energy"))`
- `AND`
- `Document Title` alanında `("gateway" OR "sensor" OR "IoT")`

##### Scholar için arama stratejisi

- __Önce geniş başlayın, sonra daraltın__: Genel karşılaştırma makaleleri veya derleme makaleleri aramak size o konunun genel bir özetini sunar. 
    - Örnek: `"A survey" of low power wireless technologies for IoT`
- __Önce Makalenin Özetini Okuyun__: Makalenin tam halini indirmek yerine özetinde işinize yarayacak bir makalemi karar verin.
- __Atıf Zincirini Takip Edin__: Diyelim ki konunuz ile alakalı çok iyi bir makale buldunuz o zaman önce
    - __Köklere doğru arama yapın__: Bulduğunuz makalenin referansına gidin ve yazarların hangi makalelerden bilgi aldığını öğrenin
    - __Geleceğe doğru arama yapın__: Bulduğunuz makaleyi ileri tarihlerde kimler atıf yapış bakın. Üzerinde çalıştığınız konuya benzer bir konu üstünde araştırma yapılan makaleleri inceleyin.
        - _İleri  takip_ için `Cited by` özelliğini kullanabilirsiniz.

##### Not Tutun

Her makale için kısa notlar tutmak, çok fazla makale taraması yapıldığında hayat kurtarır. Makalenizi otomatik kaydedebileceğiniz, notlarını ekleyebileceğiz programlarda mevcuttur. Açık kaynak ve gelir amacı gütmeyen geliştiriciler tarafından zotero adında uygulama oluşturulmuştur.
    - [Zotero](https://www.zotero.org) ile sitelerini ziyaret edebilirsiniz. Word eklentisinin bulunması, tarayıcılarda bulunan eklentileri ile makaleleri direkt uygulamaya ekleme fırsatı vermesi uygulamaya bir şans vermeniz için güzel özelliklerdir. 