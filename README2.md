* Benden ilk istenen gönderilen sanal makine imajını indirmekti. İndirdim. VMWare player ile açtım.
* Kullanıcı adı ve parola gerekliydi, asıl beklenenin şifresini bilmediğim sisteme girmek olduğunu öğrenip, sisteme giriş yaptım. Bu adımı sistem başlarken "e" harfine daha sonra ```rd.break enforcing=0``` yazarak giriş yaptım.
* Root şifresi değiştirmek için
```bash
Chroot /sysroot # sh 4 e geçtim
mount -o remount,ro /” # okunabilir yazılabilir yetkisi aldım
passwd root # root şifresini değiştimek için
```
* "**“Authentication token manipulation error”**" hatası aldım. Hatanın çözümlerine baktım. Yanı şeyleri uyguladım. Olmadı. Tekrar .ova yı başlatmaya karar verdim.
* Komutları tekrar denedim. Root şifresini değiştirdim.
```bash
mount -o remount,rw /sysroot
Chroot /sysroot # sh 4 e geçtim
mount -o remount,ro / # okunabilir yazılabilir yetkisi aldım
passwd root # root şifresini değiştimek için
```
* Sisteme tekrar root olarak giriş yaptım. Apacheyi başlatırken hata aldım. /var/log/ içerisinde httpd dosyası yokmuş. Bunu ```apachectl configtest``` komutunu çalıştırmaya çalışırken anladım.
* Apache için komutları ile kontrol ettim.
```bash
systemctl status httpd
systemctl start httpd #artık aktifti
systemctl enable httpd
```
* Apacheyi güncelemek için ```yum install httpd``` komutu ile apacheyi güncelledim.
* Kendi bilgisayarımdan ip adresini kontrol ettim veeeeee ***14-06-2021 19:48 Apache çalışıyor hem de son sürümde :)*** yazısıyla görevi tamamladım.
* Dosyaları da usb ye daha sonra da maile ekledim. :)
