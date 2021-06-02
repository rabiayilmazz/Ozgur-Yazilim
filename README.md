# Özgür Yazılım A.Ş. 2021 Staj Programı - Rabia YILMAZ

## Sanal Makine Nasıl Kurulur?
***NOT***: Ben daha önce indirdim kullanıyordum. Anlatımları hiç sanal makine kurmamış biri okuyacakmışcasına yapacağım.
* Bu yazıda VMWare Workstation 16 pro kurulumu yapacağız.
* [VMWare sitesinden](https://www.vmware.com/products/workstation-pro/workstation-pro-evaluation.html) indirme linkine gidiyoruz.
* Bir tane key gerekiyor. [Buradan](https://gist.github.com/gopalindians/ec3f3076f185b98353f514b26ed76507) bir key seçip kurulumu tamamlıyoruz.
* Bir sonraki aşama Ubuntu kurmak.

## Ubuntu LTS Server 20.04
* [Buradan](https://ubuntu.com/download/server/step2) iso dosyamızı indiriyoruz.
* İndirme tamamlandığında VMWare'i açıp yeni bir sanal makine oluşturarak işlemlere devam ediyoruz.
* Karşımıza çıkan pencereden ***I will install the operating system lather.*** seçeneği ile iso dosyasını daha sonra ekleyeceğimizi belirtiyoruz.
* Next diyerek Linux/Ubuntu-64 bit seçeneğini seçiyoruz. Bilgisayarım 64 bit olduğu için 64 biti seçiyorum. 32 bit olanlar 32 bit ya da x86 seçeneğini seçmeli.
* Bir sonraki adımda sanal makinenin adını ve pathini ayarlıyoruz. Ben F disk bölümü oluşturup işleme devam edeceğim.
* Sanal makineye kaç GB ayıracağımızı, işlemci ve ram ayarlarını da düzenleyip CD kısmından iso file seçeneğini seçiyoruz.
* İndirdiğimiz iso dosyasını seçip **OK** diyerek devam ediyoruz.
* İlgili ayarlamaları ana ekranda **edit machine** kısmından da yapabiliriz.
* Artık sanal makine hazır çalıştırabiliriz. Dil klavye seneçlerini ayarlayıp kurulumu başlatıyoruz. Sanal makine artık hazır.

### Root ayarlarını yapmak ve güncellemeler
* root şifresi ayarlama
```
sudo passwd root
```
* Güncellemeleri kontrol etmek için.
```
sudo apt update
```
* Güncellemeleri almak için
```
sudo apt upgrade
```

## SSH ile Hosta bağlanma
* Hostları görüntülemek için aşağıdaki komut kullanılır.
```
cat /etc/hosts
```
* Ssh ile locale bağlanmak içinde kullanıcı ve ip adresi gerekli.
```bash
ssh rabia@127.0.0.1
```
* Ssh key oluşturma ve keyi ssh key kopyalama için bu komutları kullandım. Asimetlik şifreleme ile şifreledim. Dsa da kullanılabilir.
```bash
sudo ssh-keygen -t rsa # id_rsa şeklinde bir key dosyası oluştu.
ssh-copy-id -i key kullanici_adi@sunucuId # sunucuya kopyaladım
ssh kullanici_adi@sunucuId -i key # key ile ssh a bağlandım
```
* Parolasız ssh girişi için:
```bash
ssh -o PubkeyAuthentication=no kullanici@uzak_ip  # rabia@127.0.0.1
```
* Güvenlik duvarı ayarlarını
```bash
sudo ufw status # güvenlik duvarının durumu
sudo ufw enable # güvenlik duvarını etkinleştirme
sudo ufw status verbose # güvenlik duvarının detaylı durumu
sudo less /etc/services #servislerin sayfalanmış listesi
```
* Güvenlik duvarının ssh ve web isteklerine açma
```bash
sudo ufw allow SSH # ssh isteklerine izin verilmesi
sudo ufw allow http # web sunucusuna izin verilmesi
sudo ufw status verbose # detaylı olarak tekrar kontrol ettim
```
* Apache kurulumu için
```bash
sudo apt-get install apache2
```
systemctl unmask sshd
systemctl enable sshd


* Web servisini 3 alan adına (domain) birden hizmet verecek biçimde
ayarlayın: bugday.org, ozgurstaj2021.com, özgürstaj2021.com.
* ozgurstaj2021.com için,
** Wordpress'in son sürümünü kurun.
** Wordpress'te yeni bir yazı (post) yazın ve o yazıya bir dosya yükleyin.
** Yeni yazınıza SEO-uyumlu bir URL'den ulaşabilmelisiniz
(ör:https://ozgurstaj2021.com/2021/02/03/benim-yeni-yazim/)
** Herhangi bir Wordpress eklentisi, teması kurmanız ya da PHP kodu
düzenlemeniz beklenmiyor. Oralara girmeyin.
** Aynı Wordpress'in hem ozgurstaj2021.com hem de özgürstaj2021.com ile
erişilebilir olması gerekiyor.
* bugday.org için bir web sitesi oluşturun.
** Site için bir web sayfası oluşturun, içinde 100 kere
"Kullanıcılarımın kişisel verilerini toplamayacağım." yazsın (her biri
yeni bir satırda).
** zubizu.com/yonetim adresine giriş parola korumasına sahip olmalı,
sadece "ad.soyad" kullanıcı adı ve "parola" parolası ile girilebilmeli.
** Web sitesi hemwww.zubizu.com  hem de zubizu.com adresinden
erişilebilir olmalıdır.
* Bu alan adları için DNS sunucu kurmanız ve ayarlamanız beklenmiyor.
Oralara girmeyin. DNS'in doğru biçimde ayarlandığını farzedin.

Dikkat edilmesi gereken noktalar:
* Sistem tekrar başlatıldığında sistemin ilgili servislerin otomatik
başlaması ve elle müdahale gerekmeden sorunsuz çalışması gerekiyor.
* Yetkileri bol keseden dağıtmayın. Örneğin bir dosya için okuma yetkisi
vermek yeterli ise, fazladan yazma ve çalıştırma yetkisi vermeyin.
* Paket yönetimi kullanın (Wordpress hariç).
* İhtiyaç olmayan ek paketler kurmayın.
* Ek özgür ve açık kaynak kodlu olmayan yazılımlar kullanmayın.

Gönderim:
* Yaptığınız işlemlerin adım adım yazıldığı bir markdown belgesi
yazmanızı istiyoruz. Ekran görüntüleri almakla uğraşmayın (yazılı olarak
anlatabilirsiniz). Belgeyi size yazılan e-postaya yanıt olarak gönderin.
* Belgede olabildiğince açıklayıcı olmanızı, düşündüklerinizi, neyi
neden yaptığınızı aktarmanızı bekliyoruz.
* Sanal makinenizi saklayın. Bir sonraki aşamada sizinle görüşme
yapılırken, sanal makinenizi açıp yaptıklarınızı göstermeniz istenebilir.
