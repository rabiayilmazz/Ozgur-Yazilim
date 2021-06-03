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
* İnternet, kullanıcı adı, ve kurulu gelecek ssh ayarlarını yaparak serverı açtım.

### Root ayarlarını yapmak ve güncellemeler
* Daha önce root şifresi ayarlamadıpım için root şifresi ayarlamasını yaptım. Exit yaparak geçerli kullanıcıdan çıkış yapıp root ile giriş yapmayı denedim.
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
* Ifconfig ile ip adresimi öğrendim.
* İlk ssh bağlantımı kendi wind bilgisayarımdan putty üzerinden yaptım. Ip adresini yazıp bağlantıyı gerçekleştirdim. Giriş yaparken kullanıcı adım ve şifremi kullandım.
* Girişleri kontrol etmek için
```bash
who #ile girişleri kontrol ettim. Girişim başarılıydı.
```
* Güvenlik duvarı ayarlarını düzenledim. Aşağıdaki komutları kullanarak.
```bash
sudo ufw status # güvenlik duvarının durumu
sudo ufw enable # güvenlik duvarını etkinleştirme
sudo ufw status verbose # güvenlik duvarının detaylı durumu
sudo ufw allow SSH # ssh isteklerine izin verilmesi
sudo ufw allow http # web sunucusuna izin verilmesi
sudo ufw status verbose # detaylı olarak tekrar kontrol ettim
```
* Putty üzerinden bağlantı kuracağım. Anahtar oluşturmak  için asimetlik şifreleme ile şifreledim. Dsa da kullanılanabilirdim. Öncelikle putty den keys bölümünden rsa key oluşturdum. Oluşturduğum public ve private keyleri kaydettim. Oluşan keyi kopyalayıp, sunucum üzerinde .ssh klasörü oluşturdum. Bu klasörün içine vim editörü ile keyi kopyalayıp kaydettim.
```bash
sudo mkdir /home/rabia/.ssh #.ssh oluşturma
sudo vim /home/rabia/.ssh/authorized_keys #kopyaladığım keyi yapıştırdım. :wq ile kaydedip çıktım.
```
* Key ile giriş yapmak için putty üzerinden keyimin yolunu gösterdim ve tekrar key ile giriş yaptım.
* Parolasız ssh girişi için:
```bash
ssh -o PubkeyAuthentication=no kullanici@uzak_ip
```
* Apache kurulumu için
```bash
sudo apt-get install apache2
```
* Şu ana kadar oluşturduğum dosyaların izinlerini baştan düzenledim.
```bash
chmod 640 * # verdiğim bütün izinleri geri aldım ya da 600 bundan sonra sudo su ile devam etmek zorunda kaldım.
```
* 3 domain ekledim. özgürstaj2021.com türkçe karakter içerdiği için sorun yaptı.
* Domainleri eklerken kullandığım komutlar sırasıyla bu şekilde.
```bash
sudo mkdir -p /var/www/bugday.org/public_html # bugdayın dosyasını oluşturdum.
sudo mkdir -p /var/www/ozgurstaj2021/public_html #ozgur stajın dosyasını oluşturdum.
sudo mkdir -p /var/www/özgürstaj2021/public_html #özgür sdtajın dosyasını oluşturdum.
```
  * İzinleri vermek için ayarlamalarımı yaptım.
  ```bash
    sudo chown -R $USER:$USER /var/www//bugday.org/public_html
    sudo chown -R $USER:$USER /var/www/ozgurstaj2021/public_html
    sudo chown -R $USER:$USER /var/www/özgürstaj2021/public_html
    # şu komutu da kullanabilirdim.
    sudo chown -R $USER:$USER /var/www/*/*
  ```
  * Sayfaların doğru bir şekilde çalışabilmesi için bu izni de vermem gerekiyor.
  ```bash
  sudo chmod -R 755 /var/www
  ```
  * Her bir klasör için index.html dosyası düzenledim. Html kodlarını yazdım siteye girdiğimde karşıma çıkacak yazıya ihtiyacım vardı.
  ```Html
  <html>
  <head>
    <title>Bu siteye hoş geldin</title>
  </head>
  <body> <h1>Hello world!</h1>
  </body>
</html>
```
  * Bu işlemleri tamamladıktan sonra domainlere özel .conf dosyaları oluşturdum. Düzenlemeleri yani her domain için özel kısımları ayarladım.
  ```xml
  <VirtualHost *:80>
    ServerAdmin admin@domain_adi
    ServerName domain_adi
    ServerAlias www.domain_adi
    DocumentRoot /var/www/domain_adi/public_html
    ErrorLog ${APACHE_LOG_DIR}/error.log
    CustomLog ${APACHE_LOG_DIR}/access.log combined
</VirtualHost>
  ```
  * Etkinleştirme işlemi için **a2ensite** kullandım. En sonda default conf dosyasını etkinleştirdim.
  ```bash
  sudo a2ensite bugday.org.conf
  sudo a2ensite ozgurstaj2021.com.conf
  sudo a2ensite özgürstaj2021.com.conf

  sudo a2dissite 000-default.conf
  ```
  * Sıradaki işlemim apacheyi tekrardan başlatmak ve windos bilgisayarımdan hosts dosyasına bu domainleri eklemek.
  ```bash
  sudo systemctl restart apache2 #restast işlemini bu komut ile yaptım. bunun yerine
  sudo service restart apache2 # komutu da iş görüyor.
  ```
  * Artık bugday.org yazdığımda ilgili index.html dosyasındaki veriyi görmeyi umut ederek browserda bugday.org yazdım ve aradım. Ama bundan önce windowsun altında hosts'a gidip domainleri server ip adresi ile ekledim. Arama sonucum başarısız oldu. Çünkü aynı ağ üzerinde değilmiş.
  * Karşılaştığım bu sorunu sanal makinenin network ayarlarını düzenleyerek hallettim. Tekrar denediğimde index.html içerisine yazdığım mesajı gördüm.
  * Wordpressin son sürümünü kurmak için
  ```bash
   wget https://wordpress.org/latest.zip # bunu /var/www/html dosyasında yapıyorum. Serverın ip adresini yazınca direkt açılması için.
   unzip latest.zip # sıkıştırılmış dosyayı çıkardım.
   mv wordpress/* . # sıkışmış dosyadaki her şeyi bir üst dizine taşıdım. Böylelikle domaini yazınca direkt wp indirebileceğim.
  ```
* Kurulumu yaptım. Mysql, php de ekledim. Bir database, kullanıcı oluşturup wordpressi ona bağladım.
* Bugday.org dan index.php ye 100 kere **Kullanıcılarımın kişisel verilerini toplamayacağım."** yazdım.
* Veritabanı bağlantı hatası aldım. Bu maddeyi yapamayınca son bir kaç görevi de yerine getiremiyorum. Bunu tam pes ederken wp dosyalarını kaldırıp tekrar kurunca düzletmiş bulundum.
* "Merhaba Özgür Yazılım" adında bir post hazırlayıp yayımladım.
* Diğer domainler içinde aynı veritabanı kaydı yapıp postlara erişim sağladım. Böylelikle tüm domainlerden aynı psta erişim sağlandım.
* bugday.org içinde aynı işlemleri yaptım. Aynı wordpresse bağladım. başka bir web sitesi nasıl eklenir, son maddenin ne olduğunu anlayamadım.
* Bütün adımları tamamladım.
