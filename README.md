# Command Line Interface Guidelines


Geleneksel UNIX ilkelerini alıp günümüze uygun şekilde güncelleyerek daha iyi komut satırı programları yazmanıza yardımcı olacak açık kaynaklı bir rehber.

> **Çevirmen Notu**
>
> Bu döküman [Command Line Interface Guidelines](https://clig.dev/) adlı yazıdan tercüme edilmiştir.
>
> Yazım veya çeviri yanlışı gördüğünüz yerleri bildirirseniz çok sevinirim.
>
> Son güncellenme tarihi: *5/2/2024*


## İçindekiler

* [Önsöz](#önsöz)
* [Giriş](#giriş)
* [Felsefe](#felsefe)
    * [İnsan odaklı tasarım](#i̇nsan-odaklı-tasarım)
    * [Birlikte çalışan basit parçalar](#birlikte-çalışan-basit-parçalar)
    * [Programlar arasında tutarlılık](#programlar-arasında-tutarlılık)
    * [(Sadece) Söylemek yeterli](#sadece-söylemek-yeterli)
    * [Keşif kolaylığı](#keşif-kolaylığı)
    * [Norm olarak *konuşma*](#norm-olarak-konuşma)
    * [Sağlamlık](#sağlamlık)
    * [Empati](#empati)
    * [Kaos](#kaos)

* [Rehber](#rehber)
    * [Temeller](#temeller)
    * [Yardım](#yardım)
    * Documentation
    * Output
    * Errors
    * Arguments and flags
    * Interactivity
    * Subcommands
    * Robustness
    * Future-proofing
    * Signals and control characters
    * Configuration
    * Environment variables
    * Naming
    * Distribution
    * Analytics

* Further reading


## Yazarlar

**Aanand Prasad** \
Squarespace'de mühendis, Docker Compose'un ortak yaratıcısı. \
[@aanandprasad](https://twitter.com/aanandprasad)

**Ben Firshman** \
[Replicate](https://replicate.ai/)'in ortak yaratıcısı, Docker Compose'un ortak yaratıcısı. \
[@bfirsh](https://twitter.com/bfirsh)

**Carl Tashian** \
[Smallstep](https://smallstep.com/)'te yazılımcı avukatı, Zipcar'ın ilk mühendisi, Trove'un ortak yaratıcısı. \
[tashian.com](https://tashian.com/) [@tashian](https://twitter.com/tashian)

**Eva Parish** \
Squarespace'te Teknik Yazar, O'Reilly'ye katkıda bulunan.\
[evaparish.com](https://evaparish.com/) [@evpari](https://twitter.com/evpari)

[Mark Hurrell](https://mhurrell.co.uk/) tarafından tasarlandı. Erken katkılarından dolayı Andreas Jansson'a ve Andrew Reitz, Ashley Williams, Brendan Falk, Chester Ramey, Dj Walker-Morgan, Jacob Maine, James Coglan, Michael Dwan, ve Steve Klabnik'e taslakları inceledikleri için teşekkür ederiz.

<br>

Eğer rehberi veya CLI tasarımını tartışmak istiyorsanız, [Discord'ta bize katılın](https://discord.gg/EbAW5rUCkE).


## Önsöz

1980'lerde kişisel bilgisayarınız sizin için bir şeyler yapmasını isteseydiniz, `C:\>` veya `~$` ile karşılaştığınızda ne yazmanız gerektiğini bilmeniz gerekiyordu.
Yardım, kalın, spiral ciltli kılavuzlar şeklinde gelirdi. Hata mesajları şeffaf değildi. Seni kurtaracak Stack Overflow yoktu.
Ancak internet erişimine sahip olacak kadar şanslıysanız [Usenet](https://en.wikipedia.org/wiki/Usenet)'ten -*en az sizin kadar hüsrana uğramış insanlarla dolu, internet öncesi topluluk*- yardım alabilirdin.
Ya sorununuzu çözmenize yardımcı olabilirlerdi ya da en azından biraz manevi destek ve dostluk sağlayabilirlerdi.

<br>

Kırk yıl sonra, bilgisayarlar herkes için çok daha erişilebilir hale geldi, bu da çoğu zaman tecrübeli son kullanıcının bilgisayar üzerinde kontrolü pahasına oldu.
Çoğu cihazda komut satırı erişimi yok, bunun nedeni kısmen 4 tarafı çevrili şirketlerin ve uygulama mağazalarının kurumsal çıkarlarına aykırı olmasıdır.

<br>

Günümüzde çoğu insan komut satırının ne olduğunu bilmiyor, hatta neden bunula uğraşmak istesinler.
Bilgisayar bilimleri öncüsü Alan Kay'in [2017'deki bir röportajda](https://www.fastcompany.com/40435064/what-alan-kay-thinks-about-the-iphone-and-technology-now) söylediği gibi, "İnsanlar bilgisayarların neyle ilgili olduğunu anlamadıkları için iPhone'da olduğunu sanıyorlar ve bu yanılsama 'Guitar Hero'nun gerçek bir gitarla aynı olduğu yanılsaması kadar kötü."

<br>

Kay'in bahsettiği "gerçek gitar" tam olarak CLI değil.
CLI'nin gücünü sunan ve metin dosyalarında yazılan yazılımların ötesine geçen bilgisayarları programlamanın yollarından bahsediyordu.
Kay'in öğrencileri arasında onlarca yıldır içinde yaşadığımız metin tabanlı zirveden kurtulmamız gerektiğine dair bir inanç var.

<br>

Bilgisayarları çok farklı şekilde programladığımız bir geleceği hayal etmek heyecan verici.
Bugün bile *spreadsheet*'ler açık ara en popüler programlama dili, ve  *no-code* hareketi, yetenekli programcılara yönelik yoğun talebin bir kısmında yerini almaya çalışırken hızla yayılıyor.

><b>Çevirmen Notu</b>: Spreadsheet'ler, Excel gibi verilerin tablo halinde hesaplanması, düzenlenmesi, analizi ve saklanması için kullanılan bilgisayar programları.

<br>

Ancak gıcırtılı, onlarca yıllık kısıtlamaları ve açıklanamaz tuhaflıkları ile komut satırı hala bilgisayarların en *çokyönlü* köşesidir.
Perdeyi geri çekmenize, gerçekte neler olup bittiğini görmenize ve bilgisayarla *GUI*'lerin karşılayamayacağı gelişmişlik ve derinlik düzeyinde yaratıcı şekilde etkileşime girmenize olanak tanır.
Öğrenmek isteyen herkes için hemen hemen her dizüstü bilgisayarda mevcuttur.
İnteraktif olarak kullanılabilir veya otomatikleştirilebilir. Sistemin diğer parçaları kadar hızlı değişmezler ve dengeli olmasında yaratıcı bir değer vardır.

<br>

Bu nedenle, hâlâ elimizdeyken, faydasını ve erişilebilirliğini en üst düzeye çıkarmaya çalışmalıyız.

<br>

O ilk günlerden bu yana bilgisayarları nasıl programladığımız konusunda çok şey değişti.
Geçmişteki komut satırı makine öncelikliydi -scripting platformunun üstündeki bir *REPL*'den biraz daha fazlasıydı-.
Ancak genel amaçlı yorumlanan diller geliştikçe *shell script*in rolü daraldı.
Günümüzünde komut satırı insan öncelikli -her türlü araca, sisteme ve platforma erişim sağlayan metin tabanlı bir kullanıcı arayüzü-.
Geçmişte editör terminalin içindeydi; bugün ise terminal çoğunlukla editörün bir özelliğidir.
Ve `git` benzeri çok özellikli komutlarda bir çoğalma oldu.
Atomic fonksiyonlar yerine, komutların içindeki komutlar ve yüksek seviyeli komutlar tüm iş akışlarını gerçekleştirdi.

><b>Çevirmen Notu</b>: REPL(*read–eval–print loop* / oku-işle-yazdır döngüsü), kullanıcı girdilerini alan, bunları işleyen ve sonucu kullanıcıya döndüren basit, etkileşimli bir bilgisayar programlama ortamıdır.

<br>

Geleneksel UNIX felsefesinden esinlenerek, daha keyifli ve erişilebilir bir CLI ortamını teşvik etme ilgisinden yola çıkarak ve programcılar olarak deneyimlerimizin rehberliğinde, komut satırı programları oluşturmaya yönelik en iyi uygulamaları ve tasarım ilkelerini yeniden gözden geçirme zamanının geldiğine karar verdik.

<b>Yaşasın komut satırı!</b>


## Giriş

Bu belge hem tasarım mimarisi felsefesini hem de somut yönergeleri kapsamaktadır.
Ayrıca bu belge, yönergeler konusunda daha ağırdır çünkü yazılımcılar olarak bizim felsefemiz çok fazla felsefe yapmak değildir.
Örnekler ile öğrenmeye inanıyoruz, bu yüzden örneklerden bol miktarda var.
inanıyoruz, bu yüzden örneklerden bol miktarda var.

<br>

Bu rehber, *emacs* ve *vim* gibi tam ekran terminal programlarını kapsamaz.
Tam ekran terminal programları niş projelerdir, çok azımız bir tane tasarlayabilecek konumda olacaktır.

<br>

Bu kılavuz aynı zamanda genel olarak programlama dilleri ve araçlar konusunda da tarafsızdır.

<br>

Bu rehber kimin için?

- Bir CLI programı oluşturuyorsanız ve kullanıcı arayüzü tasarımı için ilkeler ve somut prensipler arıyorsanız, bu kılavuz tam size göre.
- Profesyonel bir "CLI kullanıcı arayüzü tasarımcısı" iseniz bu harika, sizden bir şeyler öğrenmek isteriz.
- 40 yıldır süre gelen CLI tasarım kurallarına aykırı olan çeşitli bariz yanlışlardan kaçınmak istiyorsanız bu kılavuz tam size göre.
- Programınızın iyi tasarımı ve faydalı yardımlarıyla insanları memnun etmek istiyorsanız bu kılavuz kesinlikle size göre.
- Bir GUI programı oluşturuyorsanız bu kılavuz size göre değildir -ama yine de okumaya karar verirseniz doğru olmayan GUI tasarım kalıplarını öğrenebilirsiniz.
- Minecraft'ın sürükleyici, tam ekran CLI portunu tasarlıyorsanız bu kılavuz size göre değildir (görmek için sabırsızlanıyoruz!).


## Felsefe

Bunlar, iyi bir CLI tasarımının temel ilkeleri olarak gördüğümüz şeylerdir.

### İnsan odaklı tasarım

Geleneksel olarak UNIX komutları, öncelikli olarak diğer programlar tarafından kullanılacakları varsayımıyla yazılmıştır.
Grafik uygulamalarından ziyade programlama dilindeki fonksiyonlarla daha fazla ortak noktaları vardı.

<br>

Günümüzde birçok CLI programı öncelikli olarak (veya hatta yalnızca) insanlar tarafından kullanılsa da, arayüz tasarımlarının çoğu hâlâ geçmişin yükünü taşıyor.
Bu yükün bir kısmını atmanın zamanı geldi: Eğer bir komut öncelikle insanlar tarafından kullanılacaksa, önce insanlar için tasarlanmalıdır.

### Birlikte çalışan basit parçalar

[Orijinal UNIX felsefesinin](https://en.wikipedia.org/wiki/Unix_philosophy) temel ilkelerinden biri olan temiz interfacelere sahip küçük, basit programların daha büyük sistemler oluşturmak için birleştirilebileceği fikridir.
Bu programlara daha fazla özellik eklemek yerine, gerektiğinde yeniden birleştirilebilecek kadar modüler programlar yaparsınız.

<br>

Eski günlerde, pipelar ve shell scriptler birlikte program oluşturma süreci çok önemli bir rol oynadı.
Çok amaçlı yorumlanan dillerin yükselişiyle belki rolleri azalmış olabilir, ancak kesinlikle ortadan kalkmadılar.
Dahası, CI/CD, orkestrasyon ve konfigürasyon yönetimi biçimindeki büyük ölçekli otomasyonlar gelişti.
Artık programları birleştirilebilir hale getirmek her zamankinden daha önemli.

<br>

Neyse ki, tam da bu amaç için tasarlanmış UNIX ortamının köklü kuralları bugün hala bize yardımcı oluyor.
Standard in/out/err, sinyaller, çıkış kodları ve diğer mekanizmalar, farklı programların birbirine güzel bir şekilde oturmasını sağladı.
Düz, satır tabanlı metnin komutlar arasında aktarılması kolaydır.
Çok daha yeni bir buluş olan JSON, ihtiyaç duyduğumuzda bize daha fazla yapı sağlıyor ve komut satırı araçlarını web ile daha kolay entegre etmemizi sağlıyor.

<br>

Hangi yazılımı geliştiriyor olursanız olun, insanların onu tahmin etmediğiniz şekillerde kullanacağından kesinlikle emin olabilirsiniz.
Yazılımınız daha büyük bir sistemin parçası haline gelecek -tek seçeneğiniz yazılımınızın uyumlu bir parça olup olmayacağıdır.

<br>

En önemlisi, uyumluluk için tasarım yapmak, önce insan odaklı tasarım yapmakla çelişmek zorunda değildir.
Bu belgedeki tavsiyelerin çoğu her ikisine de nasıl ulaşılacağıyla ilgilidir.

### Programlar arasında tutarlılık

Terminalin kuralları parmaklarımıza bağlıdır.
Komut satırı sözdizimi, işaretler, ortam değişkenleri vb. hakkında bilgi edinerek ön maliyet ödemek zorunda kaldık, ancak programlar tutarlı olduğu sürece uzun vadede verimlilikle karşılığını veriyorlar.

<br>

Mümkün olduğunda bir CLI, halihazırda var olan kalıpları takip etmelidir.
Bu CLI'leri sezgisel ve tahmin edilebilir kılar ayrıca kullanıcıları verimli kılan şey de budur.

<br>

Bununla birlikte bazen tutarlılık ile kullanım kolaylığı çelişiyor.
Örneğin, uzun süredir kullanılan birçok UNIX komutu, varsayılan olarak fazla bilgi vermez; bu da, komut satırına aşina olmayan kişiler için kafa karışıklığına veya endişeye neden olabilir.

<br>

Kuralları takip etmek programın kullanılabilirliğini tehlikeye attığında, artık kurallardan kopmanın zamanı gelmiş olabilir -ancak böyle bir karar dikkatle verilmelidir.

### (Sadece) Söylemek yeterli
 
Terminal saf bir bilgi dünyasıdır.
Bilginin arayüz olduğunu ve her arayüzde olduğu gibi bilginin genellikle çok fazla veya çok az olduğunu iddia edebilirsiniz.

<br>

Bir komut birkaç dakika boyunca çalışır kaldığında çok az şey söylüyor demektir ve kullanıcı, komutun bozuk olup olmadığını merak etmeye başlar.
Bir komut, sayfalarca bilgi dökerek, gerçekten önemli olanı çıktıyı gereksiz bilgiler okyanusunda boğduğunda çok fazla şey söylüyor demektir.
Nihai sonuç ikisinde de aynı: <b>net olmama</b>, bu sonuç kullanıcının kafasını karıştırır ve sinirlendirir.

<br>

Bu dengeyi doğru kurmak çok zor olabilir, ancak yazılımın, kullanıcılarına güç ve hizmet vermesi isteniliyorsa kesinlikle çok önemlidir.

### Keşif kolaylığı

Söz konusu anlaşılabilirlik olduğunda, GUI'lerin üstünlüğü vardır.
Yapabileceğiniz her şey önünüzdeki ekranda sergileniyor, böylece hiçbir şey öğrenmenize gerek kalmadan ihtiyacınız olanı bulabilir ve hatta belki de mümkün olduğunu bilmediğiniz şeyleri keşfedebilirsiniz.

<br>

CLI'lerin bunun tersi olduğunu, her şeyin nasıl yapılacağını hatırlamanız gerektiği varsayılmaktadır.
1987'de yayınlanan orijinal [Macintosh Human Interface Guidelines](https://archive.org/details/applehumaninterf00appl), sanki yalnızca birini seçebiliyormuşsunuz gibi, "remember-and-type" yerine "see-and-point" önerisinde bulunuyor.

><b>Çevirmen Notu</b>: "see-and-point": GUI'lerde olan, gör ve işaretle. "remember-and-type": CLI'lerde olan, hatırla ve yaz.

<br>

Bu şeylerin birbirini dışlaması gerekmez.
Komut satırını kullanmanın verimliliği, komutları hatırlamaktan gelir, ancak komutların öğrenmenize ve hatırlamanıza yardımcı olmaması için hiçbir neden yok.

<br>

Anlaşılabilir CLI'ler kapsamlı dökümantasyonlara sahiptir, çok sayıda örnek sunar, bir sonraki komut için öneride bulunur, bir hata oluştuğunda ne yapılacağını gösterir.
CLI'lerin öğrenilmesini ve kullanılmasını kolaylaştırmak için tecrübeli kullanıcılar için bile GUI'lerden alınabilecek pek çok fikir vardır.

<br>

Alıntı: *The Design of Everyday Things (Don Norman), Macintosh Human Interface Guidelines*

### Norm olarak konuşma

GUI tasarımı -özellikle ilk günlerinde-, metaforlardan yoğun bir şekilde yararlanıyordu: masaüstü, dosyalar, klasörler, geri dönüşüm klasörleri.
Bu çok mantıklıydı çünkü bilgisayarlar hâlâ kendilerini meşrulaştırmaya çalışıyorlardı.
Metaforların uygulanmasının kolaylığı, GUI'lerin CLI'lara sağladığı en büyük avantajlardan biriydi.
Ancak ironik bir şekilde, CLI başından beri tesadüfi bir metaforu somutlaştırdı: *konuşma*.

<br>

Basit komutlar dışında, bir programı gerçekten çalıştırmak için genellikle birden fazla çalıştırmak gerekir.
Genellikle bunun nedeni, ilk seferde doğru yapmanın zor olmasıdır.
Kullanıcı bir komut yazar, bir hata alır, komutu değiştirir, farklı bir hata alır ve bu işlem çalışana kadar devam eder.
Tekrarlanan başarısızlık yoluyla öğrenme, kullanıcının programla yaptığı bir konuşmaya benzer.

<br>

Ancak deneme-yanılma, tek etkileşim türü değildir. Başkaları da vardır:

- Bir işlemi ayarlamak için bir komut çalıştırmak ve ardından onu gerçekten kullanmaya başlamak için hangi komutların çalıştırılacağını öğrenmek.
- Bir işlemi ayarlamak için birkaç komutun çalıştırılması ve ardından onu ayarlamak için son bir komutun çalıştırılması (örneğin, birden fazla `git add` ve ardından bir `git commit`).
- Bir sistemi keşfetmek (örneğin, bir dizin yapısını anlamak için çok fazla `cd` ve `ls` yapmak veya bir dosyanın geçmişini keşfetmek için `git log` ve `git show` yapmak).
- Karmaşık bir işlemi gerçek anlamda çalıştırmadan önce prova yapmak.

Komut satırı etkileşiminin konuşmaya dayalı doğasını kabul etmek, ilgili teknikleri cli tasarımına uygulayabileceğiniz anlamına gelir.
Parametreler geçersiz olduğunda olası düzeltmeler önerebilir, kullanıcı çok adımlı bir süreçten geçtiğinde ara durumları netleştirebilir, korkutucu bir şey yapmadan önce her şeyin iyi gözüktüğünü onlara iletebilirsin.

<br>

İsteseniz de istemeseniz de kullanıcı yazılımınızla konuşuyor.
En kötüsü, kendilerini aptal ve kırgın hissetmelerine neden olan düşmanca bir konuşmadır.
En iyi ihtimalle, yeni bilgiler ve başarı duygusuyla gidecekleri yolları hızlandıran hoş bir alışveriştir.

<br>

Daha fazlası için: [The Anti-Mac User Interface (Don Gentner and Jakob Nielsen)](https://www.nngroup.com/articles/anti-mac-interface/)

### Sağlamlık

Sağlamlık hem nesnel hem de öznel bir özelliktir.
Yazılım elbette sağlam olmalıdır: Beklenmedik girdiler zarifçe ele alınmalı, işlemler mümkün olduğunca bağımsız olmalıdır vb. Ancak aynı zamanda sağlam da *hissettirmelidir*.

<br>

Yazılımınızın parçalanmayacakmış gibi hissetmesini istiyorsunuz.
Sanki dayanıksız bir plastik "yumuşak anahtar" değil de büyük bir mekanik makineymiş gibi hızlı ve anlaşılır olmasını istiyorsunuz.

<br>

Öznel sağlamlık, ayrıntılara dikkat etmeyi ve neyin yanlış gidebileceği hakkında iyice düşünmeyi gerektirir.
Pek çok küçük şey var: kullanıcıyı olup bitenler hakkında bilgilendirmek, yaygın hataların ne anlama geldiğini açıklamak, korkutucu görünen yığın izleri yazdırmamak.

<br>

Genel bir kural olarak sağlamlık, işi basit tutmaktan da kaynaklanabilir.
Pek çok özel durum ve karmaşık kod, bir programı kırılgan hale getirme eğilimindedir.

### Empati

Komut satırı araçları bir programcının yaratıcı araç takımıdır, bu nedenle kullanımı keyifli olmalıdır.
Bu, onları bir video oyununa dönüştürmek veya çok fazla emoji kullanmak anlamına gelmez (ama doğal olarak bunda 😉 bir sorun yok).
Bu, kullanıcıya onun yanında olduğunuzu, onun başarılı olmasını istediğinizi, sorunları ve bunların nasıl çözüleceği hakkında dikkatlice düşündüğünüz hissini vermek anlamına gelir.

<br>

Onların bu şekilde hissetmelerini sağlayacak herhangi bir eylemler listesi yok, ancak tavsiyelerimize uymanın sizi o noktaya bir miktar ulaştıracağını umuyoruz.
Kullanıcıyı memnun etmek, *her fırsatta beklentilerini* aşmak anlamına gelir ve bu da empatiyle başlar.

### Kaos

Terminal dünyası tam bir karmaşa.
Tutarsızlıklar her yerde ve bu bizi yavaşlatıyor, kendimizi ikinci bir tahminde bulunmamıza neden oluyor.

<br>

Ancak bu kaosa sebep olan bir güç kaynağı olduğu yadsınamaz.
Terminal -genel olarak UNIX tabanlı ortamlar gibi- oluşturabileceğiniz şeyler üzerinde çok az kısıtlama getirir.
Bu yüzden bu alanda her türlü icat çiçek açtı.

<br>

Bu belgenin, onlarca yıllık komut satırı geleneğiyle çelişen tavsiyelerin yanı sıra mevcut kalıpları takip etmenizi istemesi de ironik.
Kuralları çiğnemekten herkes kadar biz de suçluyuz.

<br>

Sizin de kuralları çiğnemeniz gereken bir zaman gelebilir.
Geldiği zaman bunu niyetle ve amacınızın netliğiyle yapın.

<br>

"Üretkenliğe veya kullanıcı memnuniyetine açıkça zararlı olduğunda bir standardı terk edin." - Jef Raskin, [The Humane Interface](https://en.wikipedia.org/wiki/The_Humane_Interface)

## Rehber

Bu rehber, komut satırı programınızı daha iyi hale getirmek için yapabileceğiniz belirli şeylerin bir derlemesidir.

İlk bölüm, izlemeniz gereken temel şeyleri içerir. Bunları yanlış anlamak programınızın kullanımı zorlaştıracak veya programınızın kötü bir CLI örneği olmasına sebep olacaktır.

Geri kalan bölümler ise *olursa daha güzel olur* tarzındadır. Bunları eklemek için yeterince zamanınız ve enerjiniz varsa, programınız ortalama programlardan çok daha güzel olacaktır.

Buradaki ana fikir, aslında programınızın tasarımı hakkında çok fazla zaman harcamaya gerek olmadığıdır: sadece bu kurallara uyarak programınız muhtemelen daha iyi olacaktır. Öte yandan, tasarladıysanız ve programınız için bir kuralın yanlış olduğunu belirlediyseniz bu bir sorun değildir (Keyfi kurallara uymadığınız için programınızı reddedecek herhangi bir otorite yoktur).

Ayrıca bu kurallar kesinleşmiş değildir. Eğer bir kurala iyi bir nedenden ötürü katılmıyorsanız, [bir değişiklik önereceğinizi](https://github.com/cli-guidelines/cli-guidelines) umuyoruz.

### Temeller

Uymanız gereken birkaç temel kural var. Bunları yanlış anladığınızda programınızın kullanımı ya çok zor olacak ya da tamamen bozulacaktır.

<b>Mümkün olduğunca "<i>command-line argument parsing</i>" kütüphanelerini kullanın.</b> Ya *built-in* olmalı ya da 3.parti bir yazılım olmalı. Argümanları ve bayrakları ayırmayı, yardım metnini ve hatta yazım önerilerini mantıklı bir şekilde ele alacaklardır.

İşte beğendiklerimizden bazıları:

* Multi-platform: [docopt](http://docopt.org)
* Bash: [argbash](https://argbash.io)
* Go: [Cobra](https://github.com/spf13/cobra), [cli](https://github.com/urfave/cli)
* Haskell: [optparse-applicative](https://hackage.haskell.org/package/optparse-applicative)
* Java: [picocli](https://picocli.info/)
* Node: [oclif](https://oclif.io/)
* Deno: [flags](https://deno.land/std/flags)
* Perl: [Getopt::Long](https://metacpan.org/pod/Getopt::Long)
* PHP: [console](https://github.com/symfony/console), [CLImate](https://climate.thephpleague.com)
* Python: [Argparse](https://docs.python.org/3/library/argparse.html), [Click](https://click.palletsprojects.com/), [Typer](https://github.com/tiangolo/typer)
* Ruby: [TTY](https://ttytoolkit.org/)
* Rust: [clap](https://clap.rs/), [structopt](https://github.com/TeXitoi/structopt)
* Swift: [swift-argument-parser](https://github.com/apple/swift-argument-parser)

**Başarı durumunda 0 çıkış kodunu döndür, başarısızlık durumunda sıfırdan farklı bir kod döndür.** Çıkış kodları, bir programın başarılı çalışıp çalışmadığını gösterir; bu nedenle bunu doğru şekilde bildirmelisiniz. Sıfır olmayan çıkış kodlarını en önemli hata kodlarıyla eşleştirin.

**Çıktıları `stdout`'a bastır.** Programınızın birincil çıktıları stdout'a gitmelidir. Ayrıca makine tarafından okunabilen her şeyde stdout(varsayılan olarak herşeyin basıldığı bir kanal)'a gitmelidir.

**Bilgi mesajlarını `stderr`'a bastır.** Log mesajları, hatalar vb. tümü `stderr`'e gönderilmelidir. Bu, birden fazla komut bir araya getirildiğinde mesajların kullanıcıya görüntülendiği ve bir sonraki komuta aktarılmadığı anlamına gelir.

### Yardım

**Hiçbir seçenek girilmediğinde yardım metnini, `-h` bayrağını veya `--help` bayrağını görüntüleyin.**

**Varsayılan olarak kısa bir yardım metni görüntüle.** Yapabiliyorsanız, `myapp` veya `myapp altkomut` çalıştırıldığında varsayılan olarak yardım metnini görüntüleyin. Tabi eğer programınız çok basit olmadığı ve varsayılan olarak bariz bir şey yapmadığı sürece(örnek olarak `ls`) veya girilenleri okuyan bir programınız varsa (örnek olarak `cat`).

Kısa yardım metni yalnızca şunları içermelidir:

- Programınızın ne yaptığının açıklaması.
- Bir veya iki örnek çağrı.
- Çok fazla olmadığı sürece bayrakların açıklamaları.
- Daha fazla bilgi için `--help` bayrağını iletme talimatı.

`jq` bunu iyi yapıyor. `jq` yazdığınızda, giriş niteliğinde bir açıklama ve bir örnek görüntülenir, ardından bayrakların tam listesi için `jq --help` komutunu geçmeniz istenir:

```
$ jq
jq - commandline JSON processor [version 1.6]

Usage:    jq [options] <jq filter> [file...]
    jq [options] --args <jq filter> [strings...]
    jq [options] --jsonargs <jq filter> [JSON_TEXTS...]

jq is a tool for processing JSON inputs, applying the given filter to
its JSON text inputs and producing the filter's results as JSON on
standard output.

The simplest filter is ., which copies jq's input to its output
unmodified (except for formatting, but note that IEEE754 is used
for number representation internally, with all that that implies).

For more advanced filters see the jq(1) manpage ("man jq")
and/or https://stedolan.github.io/jq

Example:

    $ echo '{"foo": 0}' | jq .
    {
        "foo": 0
    }

For a listing of options, use jq --help.
```

**`-h` ve `--help` bayrakları geçildiğinde tam yardım metnini göster.** Bunların hepsi yardım göstermeli:

```
$ myapp
$ myapp --help
$ myapp -h
```

Eğer `-h` bayrağı eklenmişse diğer işaretleri ve argümanları göz ardı edin ve yardımı gösterin. `-h` bayrağını herhangi bayrağın ezmesine izin vermeyin.

Eğer programınız `git` benzeriyse, aşağıdakiler de yardım sunmalıdır:

```
$ myapp help
$ myapp help subcommand
$ myapp subcommand --help
$ myapp subcommand -h
```

**Geri bildirim ve sorunlar için bir destek yolu gösterin.** Üst yardım metninde bir web sitesi veya GitHub bağlantısı kullanımı yaygındır.

**Yardım metninde dokümantasyonların web sürümüne bağlantı verin.** Bir alt komut için belirli bir sayfanız veya bağlantınız varsa doğrudan ona bağlantı verin. Bu, özellikle web'de daha ayrıntılı belgeler veya bir şeyin davranışını açıklayabilecek daha fazla okuma varsa faydalıdır.

**Örneklerle yol gösterin.** Kullanıcılar diğer dokümantasyon biçimleri yerine örnekleri kullanma eğilimindedir; bu nedenle, bunları, özellikle yaygın karmaşık kullanımlar olmak üzere, yardım sayfasında ilk önce gösterin. Ne yaptığını açıklamaya yardımcı oluyorsa ve çok uzun değilse gerçek çıktıyı da gösterin.

Bir hikayeyi bir dizi örnekle anlatarak karmaşık kullanımlara doğru yol alabilirsiniz.

**Bir sürü örneğiniz varsa, bunları başka bir yere koyun.** Örneğin bir web veya bir komut cheat sheet sayfası Kapsamlı, gelişmiş örneklere sahip olmak faydalıdır ancak yardım metninizi çok uzun yapmak istemezsiniz.

Daha karmaşık kullanım durumları için, örneğin başka bir araçla entegrasyon yaparken, kapsayıcı bir eğitim yazmak uygun olabilir.

**Yardım metninin başında en sık kullanılan bayrakları ve komutları görüntüleyin.** Çok sayıda bayrağa sahip olmak sorun değil, ancak gerçekten yaygın olanlarınız varsa, önce onları gösterin. Örneğin, `git` başlangıç komutlarını ve en sık kullanılan alt komutları önce görüntüler:

```
$ git
usage: git [--version] [--help] [-C <path>] [-c <name>=<value>]
           [--exec-path[=<path>]] [--html-path] [--man-path] [--info-path]
           [-p | --paginate | -P | --no-pager] [--no-replace-objects] [--bare]
           [--git-dir=<path>] [--work-tree=<path>] [--namespace=<name>]
           <command> [<args>]

These are common Git commands used in various situations:

start a working area (see also: git help tutorial)
   clone      Clone a repository into a new directory
   init       Create an empty Git repository or reinitialize an existing one

work on the current change (see also: git help everyday)
   add        Add file contents to the index
   mv         Move or rename a file, a directory, or a symlink
   reset      Reset current HEAD to the specified state
   rm         Remove files from the working tree and from the index

examine the history and state (see also: git help revisions)
   bisect     Use binary search to find the commit that introduced a bug
   grep       Print lines matching a pattern
   log        Show commit logs
   show       Show various types of objects
   status     Show the working tree status
…
```

**Yardım metninizde biçimlendirme kullanın.** Kalın başlıklar taramayı çok daha kolaylaştırır. Ancak, bunu terminalden bağımsız bir şekilde yapmaya çalışın, böylece kullanıcılarınız *kaçış karakterlerinden* oluşan bir duvara bakmazlar.

```
$ heroku apps --help
list your apps

USAGE
  $ heroku apps

OPTIONS
  -A, --all          include apps in all teams
  -p, --personal     list apps in personal account when a default team is set
  -s, --space=space  filter by space
  -t, --team=team    team to use
  --json             output in json format

EXAMPLES
  $ heroku apps
  === My Apps
  example
  example2

  === Collaborated Apps
  theirapp   other@owner.name

COMMANDS
  apps:create     creates a new app
  apps:destroy    permanently destroy an app
  apps:errors     view app errors
  apps:favorites  list favorited apps
  apps:info       show detailed app information
  apps:join       add yourself to a team app
  apps:leave      remove yourself from a team app
  apps:lock       prevent team members from joining an app
  apps:open       open the app in a web browser
  apps:rename     rename an app
  apps:stacks     show the list of available stacks
  apps:transfer   transfer applications to another user or team
  apps:unlock     unlock an app so any team member can join
```

Not: `heroku apps --help` komutu herhangi bir cihazdan çalışıtırıldığında, komut hiçbir kaçış karakteri yaymaz.

**Kullanıcı yanlış bir şey yaptıysa ve ne anlama geldiğini tahmin edebiliyorsanız, önerin.** Örneğin, `brew update jq`, `brew upgrade jq` komutunu çalıştırmanız gerektiğini söyler.

Önerilen komutu çalıştırmak isteyip istemediklerini sorabilirsiniz, ancak bunu zorlamadan yapın. Örneğin:

```
$ heroku pss
 ›   Warning: pss is not a heroku command.
Did you mean ps? [y/n]:
```

Düzeltilmiş sözdizimini önermek yerine, sanki ilk etapta doğru yazmışlarmış gibi, onlar için çalıştırmak cazip gelebilir. Bazen bu yapılacak doğru şeydir, ama her zaman değil.

İlk olarak, geçersiz girdi mutlaka basit bir yazım hatası anlamına gelmez -bu genellikle kullanıcının mantıksal bir hata yaptığı veya bir kabuk değişkenini kötüye kullandığı anlamına gelebilir. Özellikle eylem bir durumu değiştiriyorsa, ne anlama geldiklerini varsaymak tehlikeli olabilir.

İkincisi, kullanıcının yazdıklarını değiştirirseniz, doğru sözdizimini öğrenemeyeceklerini unutmayın. Bunu yaparak yazdıkları şeklin geçerli ve doğru olduğuna karar veriyorsunuz. Ve bunu süresiz olarak desteklemeyi taahhüt ediyorsunuz. Bu kararı verirken kasıtlı olun ve her iki sözdizimini de belgeleyin.

Daha fazlası için: ["Do What I Mean"](http://www.catb.org/~esr/jargon/html/D/DWIM.html)

**Komutunuz ona bir şey iletilmesini bekliyorsa ve stdin etkileşimli bir terminalse, hemen yardımı görüntüleyin ve çıkın.** Bu, `cat` komutunda olduğu gibi herhangi bir ileti beklemek zorunda kalmaması anlamına gelir. Alternatif olarak, stderr'a bir log mesajı yazdırabilirsiniz.