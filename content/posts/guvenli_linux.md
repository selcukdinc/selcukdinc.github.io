+++
title = 'Güvenli Linux'
date = 2025-03-30T18:05:56+03:00
draft = false
ShowToc = true
+++
#### Bu yazıda kullanılan işletim sistemleri
| ___Taraf___ | İşletim Sistemi |
| :--:      | --: |
| Kullanıcı | Windows 11 Home 24H2 | 
|Sunucu     | Linux-Ubuntu 24.04 LTS (Focal Fossa) |

## Güvenli SSH Bağlantısı Nasıl Yapılır ? (Sıfırdan)
host firmasından sunucu kiraladığınız zaman size, sunucuya ait ip adresi, kullanıcı adı ve parola verir. (Genellikle root kullanıcısı verilir [Root kullanıcısı sunucunun bütün kaynaklarına erişimi olan kullanıcıdır])

Powershell'e <br>`ssh kullanici_adi@sunucu_ip` yazıp girdikten sonra sunucu sizden parola ister, doğru parola ardından sunucunuza bağlanmış olursunuz. 
___[`NOT: powershell'e sağ tıkladığınızda yapıştırma işlemi yapabilirsiniz`]___

Bu yöntem bir çok varsayılan ayarla birlikte çalışır. (Örneğin ssh bağlantısı için port numarası 22'dir)
- Kullanıcı adı, 
- parola, 
- sunucu ip

maddelerine sahip olan birisi root kullanıcısı olarak rahatlıkla sunucuya bağlanabilir ve istediğini yapabilir. 

Bu başlıktaki adımları uyguladıktan sonra art niyetli kişinin sunucunuzu ele geçirebilmesi için; 
- kullanıcı adı, 
- sunucu ip, 
- sunucunun hangi port ile iletişim kurduğu, 
- ed25519 ile şifrelenmiş anahtar dosyası
- anahtar ile bağlantılı olan parola cümlesi

maddelerini ele geçirmesi gerekmektedir.

---

### Sudo yetkisine sahip kullanıcı oluşturalım

[Sunucu Tarafı]

Bu adım için Sudo yetkisine sahip olmanız gerekiyor. (Root iseniz sorun yok)
- Kullanıcı oluştur `sudo adduser kullanici1` [isim örnek olarak seçilmiştir]
  - Kullanıcı için istenilen bilgiler parola oluşturma adımına kadar atlanabilir
- Kullanıcıyı Sudo grubuna ekleme : `sudo usermod -aG sudo kullanici1`
- Oluşturulan kullanıcının sudo yetkisinin kontrolü
  - `su - kullanici1`
  - `sudo ls /root` (bu adımı sorunsuz geçtiyseniz kullanici1 artık sudo yetkisine sahip demektir)
- Şimdi sunucuya kendi oluşturduğunuz kullanıcı adı ve parola ile girmeyi deneyin. 
 
`ssh kullanici1@sunucu_ip`

---

### Sunucunun SSH için standart belirlenmiş port numarasını değiştirelim

- `sudo vim /etc/ssh/sshd_config` belge açılır. (Burada vim yerine favori text editörünü kullanabilirsiniz)
- INS tuşu ile belge insert moduna alınır
- `Port` parametresi değiştirilir. (Örneğin `Port 1923`) ___Yeni port numarasını not etmeyi untmayın___
- ESC tuşu ile belge insert modundan çıkılır
- `:wq` ile belge kaydedilir.
- ssh 'daemon yeniden başlatılır `sudo systemctl restart ssh`
- Şimdi sunucuya kendi kullanıcınız ve yeni port numaranız ile giriş yapmayı deneyin 

`ssh -p 1923 kullanici1@sunucu_ip`


Bu adımıda sorunsuz tamamladıysanız bir sonraki adıma geçebiliriz

---

### ed25519 tipinde Anahtar Oluşturalım 
[ed25519 Anahtarı hakkında daha fazla bilgi için tıklayın](https://ed25519.cr.yp.to/)

[Kullanıcı Tarafı]

- Powershell'e `ssh-keygen -t ed25519` yazalım. 
- Bizden kaydedeceği yeri gösteren ve anahtar dosyasına isim vermemizi isteyen bir satır çıkıyor. Dilediğiniz bir ismi verebilirsiniz. (anahtarımızın adı anahtar1 olsun)
- Sonraki adımda sizden 'passphrase' opsiyonel olarak anahtarınızla ilişkili parola cümlesi girmenizi istiyor. Bu cümleniz ne kadar güçlü olursa bu adımı kırmak o kadar imkansız olur. 

 Örnek bir cümle `ItaotIaL-test-QpFXZzjl-cumlesi-LvgpAtFv` bir parola üreticisinden dilediğiniz uzunluğa ayarlayıp cümlenizin arasına parolalar serpiştirebilirsiniz. Bu sayede bu cümle tahmin edilemez oldu.
  
  ___Oluşturduğunuz cümleyi kaydetmeyi unutmayın___ 
  
  ([Kullanıcı Tarafı] Öncelikle not defterinde cümlenizi oluşturun, ardından 'passphrase' sırasında cümlenizi yapıştırın.)
- Bilgisayarınızda C://Users/Kullanıcı > içerisinde 'anahtar1' ve 'anahtar1.pub' adında iki adet dosya oluşturulmuş olması gerekiyor.

---

### Oluşturulan Anahtarı Ubuntu sunucusuna aktaralım

[Kullanıcı Tarafı]

- powershell'e `scp ./anahtar1.pub kullanici_adi@sunucu_ip:~` yazalım
- Aktarım tamamlandıktan sonra ssh ile sunucunuza bağlanın.
[Sunucu Tarafı]
- Anahtarınızı `ls` komutundan sonra `anahtar1.pub` olarak görüyor olmanız gerekiyor

---

### Yalnızca Anahtar ile Girişi Sağlayalım

[Sunucu Tarafı]

Bu adım kritiktir. Hatalı bir işlem ardından sunucunuza erişimi kaybedebilirsiniz. Lütfen bu adımı uygulamadan önce verilerinizi yedeklemeyi unutmayın.

#### SSH Bağlantısının Zaman Aşımını Kapatalım

Bu adım bize sunucuya bağlandıktan sonra hiçbir işlem yapmasak dahi ssh bağlantımızı canlı tutacaktır.

[ Sunucu Tarafı ]

- `sudo vim /etc/ssh/sshd_config` dosyasını açalım.
- INS tuşu ile belge insert moduna alınır
- `ClientAliveInterval 40` ve `ClientAliveCountMax 3` parametrelerini bulalım ve yorum durumundan çıkaralım. `ClientAliveInterval` parametresi sıfırsa değeri güncelleyin
- ESC tuşu ile belge insert modundan çıkılır
- `:wq` ile belge kaydedilir. 
- ssh daemon'u yeniden başlatın `sudo systemctl restart ssh`

Şimdi sunucunuza yeniden bağlanın. Hiçbir şey yapmasanız dahi ssh bağlantınız siz çıkana kadar canlı kalacaktır.

#### Anahtarlı Girişi Aktive Etme

Bu adımda minimum 2 adet ssh bağlantısı kurun. Bir tanesinde anahtarlı girişi test edeceğiz. Eğer bir hata ile karşılaşırsak diğer ssh bağlantısı ile yaptığımız değişiklikleri geriye alabiliriz.

Sunucumuza 'anahtar1.pub' olarak bir anahtar göndermiştik. Anahtarın bulunduğu dosya konumundan devam ediyoruz.

- `cat anahtar1.pub >> .ssh/authorized_keys`
- `rm anahtar1.pub`
- `chmod 600 .ssh/authorized_keys`
- `chmod 700 .ssh`
- `sudo systemctl restart ssh`

Bu adımlardan sonra sshd belgesini düzenlememiz gerekiyor.

- `sudo vim /etc/ssh/sshd_config` dosyasını açalım.
- INS tuşu ile belge insert moduna alınır
- Belgenin en sonuna alttaki parametreler eklenir.
 ```
 Protocol 2
MaxAuthTries 3
PermitRootLogin no
PasswordAuthentication no
PubkeyAuthentication yes
AuthenticationMethods publickey
KbdInteractiveAuthentication no
X11Forwarding no
```
- ESC tuşu ile belge insert modundan çıkılır
- `:wq` ile belge kaydedilir. 
- ssh daemon'u yeniden başlatın `sudo systemctl restart ssh`

Şimdi powershell'den tekrar bağlanmayı deneyin. Bağlantı satırınız alttaki gibi olacaktır.
```red
ssh -i .\anahtar1 -p PORTSAYISI kullanici1@sunucuip
```
Eğer bağlantı başarılı bir şekilde kurulmuş ise sunucu sizden passphrase isteyecektir, onuda girdikten sonra sunucuya bağlanmış olacaksınız.

---

### Olası Hatalar
#### 'WARNING: UNPROTECTED PRIVATE KEY FILE!' Hatası
Eğer anahtar dosyanız bir git projesi içinde ise, ssh bağlantısı esnasında “WARNING: UNPROTECTED PRIVATE KEY FILE!” hatası almanız büyük olası. Bu sorunu çözebilmeniz için dosyanın okuma ve yazma ayarlarını güncellemeniz gerekir.

___Sorunun Çözümü___
- `chmod 0400 file` ile dosya yalnızca okuma moduna alınır.

Tekrar bağlanmak istediğinizde hata almamanız gerekir.

---
## Kaynakça

- 'Güvenli SSH Bağlantısı Nasıl Yapılır' için faydalınılan kaynaklar - 
  - [SSH To Windows Using Public Key (Video)](https://www.youtube.com/watch?v=Wx7WPDnwcDg)
  - [Securing A Linux Server / SSH - Blog Post](https://kenhv.com/blog/securing-a-linux-server)
  - Chatgpt 4o Prompt : 
   
    "Merhaba! Sana aklıma takılan soruları sormak istiyorum. Sunucumda Linux Ubuntu dağıtımı var. Sunucuya kullanmakta olduğum windows 11 - powershellden bağlanıyorum. Root yerine sudo grubuna dahil ettiğim kullanıcı oluşturdum. Bağlantıyı ise "ssh kullaniciadi@sunucuip" şeklinde yazıp bilgisayarımdan bağlanabiliyorum. Parolamı girdikten sonra sunucuya bağlanmış oluyorum. Fakat benim istediğim şey yalnızca kullanıcı adı ve parola dışında birde private key dahil olsun istiyorum. Bu güvenlik adımı için bir makale inceliyordum ed25519 kriptoloji metodunu kullanarak key oluşturdu ve sunucuya yükledi bir ssh_config dosyasını güncelleyip sunucuyu yeniden başlattı. 

    Kaynak edindiğim site : https://kenhv.com/blog/securing-a-linux-server
    Rehberde tamamladığım adımlar : "Generate an Ed25519 key (passphrase is optional but recommended):" "ssh-keygen -t ed25519" adımını tamamladım, keyi görüntüleyebiliyorum. Hatta 'passphrase' oluşturdum. Fakat ne amaçla kullanıldığını anlamadım.
 
    Rehberde takıldığım adım : "Copy the key to your server:" adımında "ssh-copy-id -i <path-to-key> <user>@<ip>" işlemini yaptıktan sonra keyi sunucuda nasıl bulabilirim bilmiyorum. En son ssh'i yeniden başlattığımda bilgisayarımdaki key dosyasıyla nasıl bağlanabilirim? Takıldığım yerlerde yardımcı olursan sevinirim."
- Olası Hatalar
  - 'WARNING: UNPROTECTED PRIVATE KEY FILE!' Hatası
    - sorunun çözüm [kaynağı](https://www.cyberciti.biz/faq/warning-unprotected-private-key-file-ssh-linux-unix-error/)
    - chmod için faydalı [makale](https://www.linuxfoundation.org/blog/blog/classic-sysadmin-understanding-linux-file-permissions)