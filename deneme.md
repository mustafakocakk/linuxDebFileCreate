---
baslik: Linux Sistemlerde Deb Dosyası Hazırlama
kategori: Linux
yayinTarihi: 11.02.2019
etiket: dpkg,debfile
---

Debian paketleri, sisteme kurulmak istenen uygulamaların gerekli  ayarlarının bulunduğu paketlerdir.

**Debian paketleri ikiye ayrılır;**

Binary packages : Bu paket içerisinde çalıştırılabilir dosyalar, konfigürasyon dosyaları, man/info sayfaları ve diğer dokümanlar bulunur. İkili paketler genellikle .deb uzantısı ile ayırt edilir ve dpkg komutu ile kurulum yapılır.

Source packages : Kaynak paketleri, bu paketin yapısını belirten .dsc dosyası, orjinal kaynak kodunu arşiv içerisinde bulunduran .orig.tar.gz dosyası ve dosyadaki debian-spesifik değişiklikleri içeren .diff.gz dosyasından oluşan paketlerdir.


**Bağımlılık Nedir ?**
Bağımlılık o paketin çalışabilmesi/derlenebilmesi için gereken paketlerdir.

Build Dependency : Paketin derlenebilmesi için gereken paketler.
Runtime Dependency : Paketin çalışabilmesi için gereken paketler.


**Gereksinimler**

Bir deb file oluşturmak için gerekli olan paketlerin kurulumu aşağıdaki gibidir.
apt-get install build-essential binutils fakeroot lintian debhelper dh-make devscripts patch libc6-dev file checkinstall makefile 



# Manuel Debian (.deb) Paketi Oluşturma

Burada  herhangi bir kaynak kod  kullanmayarak kendimize özel ayar dosyaları  ile "deb" dosyamızı oluşturmaya çalışacaz.


DEBIAN/control : Bu dosya yazmış olduğumuz uygulamanın ismini, versiyonunu, boyutu, açıklama, bağımlılıklarının yazıldığı dosyadır. Bu dosyanın içeriği aşağıdaki gibidir.

 

Package : Uygulamanın adı.

Version : Uygulamanının versiyon numarası.

Source : Uygulamanın adı yazılabilir.

Section : Uygulamanın hangi kategoriye girdiği.

Priority : Uygulamanın sistem için gerekliliği.

Maintainer : Geliştiricini mail adresi veya ismi.

Architecture : Hangi mimariler için olduğu.

Depends : Hangi paketlere bağımlılık duyduğu. Burada paket isimlerini virgül (,) ile ayırıyoruz.

Description : Uygulamanın ne ile alakalı olduğunu veren ufak bir yazı yazılabilir.

 

DEBIAN/preinst : Bu dosya .deb dosyasının ilk çalıştırdığımız zaman çalışan komutlar seti  bulunur.

DEBIAN/postinst : Bu dosya ise preinst dosyasından sonra çalışan bir dosyadır. Uygulama sisteme kurulduktan sonra yapılacak olan konfigurasyonları içerir.

DEBIAN/postrm or DEBIAN/prerm: Bu dosyalar sisteme kurulu olan paketi gerekli ayarla ile kaldırmaya yarar.

DEBIAN/copyright : Debian için bu dosya bir paket hakkında lisans bilgisini içermektedir. 

DEBIAN/rules : Debian paketiniz için bir Makefile dosyasıdır. Bir debian paketi dpkg komutu ile kurulurken bu dosyadaki bilgilere göre kurulur.

DEBIAN/changelog : Bu dosya, programın gelişimini anlatan bir seyir defteridir. Programın kaynağından bağımsız bir şeyler yaptıysanız veya bazı hataları giderdi iseniz bu dosyanın içerisine yazabilirsiniz.

DEBIAN/readme & DEBIAN/install : Bu iki dosya kullanıcıları bilgilendirmek için kullanılmaktadır. Genellikle bu dosyanın içerisine programın nasıl kurulacağını veya ne işe yaradığını yazabilirsiniz.

İlk uygumamıza başlarsak  

mkdir /home/user/Debfile
mkdir /home/user/Debfile/DEBIAN
cd /home/user/Debfile/DEBIAN
touch control postinst prerm


**Control Dosyası Ayarı**

Package: debfile
Version: 1.1.1
Architecture: all
Essential: no
Section: web
Priority: optional
Depends: python3
Maintainer: Mustafa Akocak
Description: Örnek deb file

**Preinstall Dosyası**

sudo  apt-get update

**Postinst Dosyası**
#!/bin/sh
cd /tmp
mkdir deneme
**Prerm Dosyaları**

#!/bin/sh
if [ "$1" = "remove" ]; then
       rmdir /tmp/deneme
fi


 dpkg-deb --build /home/akocak/Debfile/


 dpkg -i Debfile.deb

dpkg -r debfile
