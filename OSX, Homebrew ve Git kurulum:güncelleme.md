Merhaba,

OSX kurulu iMac'imde mevcut Git'i güncellemeyi düşünürken ```git --version``` komutu ile sürümüne baktım. Ardından brew ile güncellemeye kalktığımda Git'i brew ile kurmadığımı farkettim. Bu aşamadan sonra ```brew install git``` komutuyla kurulumu gerçekleştirdim. İşte tantana tam da bundan sonra koptu :) Böylelikle sistemimde iki adet git kurulumu yapmış oldum. Birisi Apple tarafından varsayılan olarak gelen (Büyük ihtimalle XCode ile kurulan git ve benim az önce brew ile kurdugum yeni git) Ancak bu sorunun çözümünü bulmam çok zaman almadı.

Homebrew kullanarak Git kurmak istiyorsanız normalde sisteminizde bulunan Git'i yeni kurdugumuz homebrew tarafından yüklenen Git ile değiştirmek için bu yazımda anlatacağım küçük değişikliği yağmamız gerekecektir.

[more]

Normal şartlarda bilgisayarınızda consolu açıp aşağıdaki komutu yazdıgınızda ```/usr/bin/git``` dizininde bulunan git çalışacak ve aşağıdaki gibi bir çıktı üretecektir.

```bash
$>git --version
git version 1.7.9.6 (Apple Git-31.1)
````

Bunun yerine biz homebrew kullanarak git kurmak istediğimizde terminalde aşağıdakı kurulum komutunu kullandıktan sonra;

```bash
$> brew install git
````

kurulum bittiğinde git brew tarafından ```/usr/local/bin/``` dizinine konumlandırılacak ve burada bir PATH cakışması oalcaktır. Bu hataları ise aşağıdaki brew komutu ile kontrol edebiliriz;

```bash
$>brew doctor
Error: /usr/bin occurs before /usr/local/bin
This means that system-provided programs will be used instead of those
provided by Homebrew. The following tools exist at both paths:
git
...
...
````

Yukarıdakı çıktıda çakışan git ve ilişkili uygulamalar listelencektir.

Gelelim çözüme:

Sorunu çözmek için ```/etc/paths``` dosyanızda küçük bir değişiklik yapmanız yetecektir. Bu dosyada bulunan en alt satırdaki ```/usr/local/bin``` yolunu dosyanın en üstüne ```/usr/bin``` yolunun hemen üstüne taşıyıp bilgisayarınızı yeniden başlattığınızda artık sorun ortandan kalmış olacak ve terar ```brew doctor``` komutu ile paketlerinizi kontrol ettiğinizde PATH çakışması hatasından kurtulacaksınız. ve herşey yolunda gittiyse aşağıdaki terminal komutlarını yazdıgınızda hemen altında bulunan çıktıları almış olmanız gerekiyor.

```bash
$>which git
/usr/local/bin/git
$>git --version
git version 1.8.0
````

Lafı biraz dolandırarak anlattıgımın farkındayım (yorgunluğuma verin) ama umarım bu tarz sorun yaşayan birisine faydası dokunur :)

İyi çalışmalar.

### Etiketler
git, homebrew, osx, version, güncelleme, path, bin


