# Command Line Interface Guidelines


Geleneksel UNIX ilkelerini alıp günümüze uygun şekilde güncelleyerek daha iyi komut satırı programları yazmanıza yardımcı olacak [açık kaynaklı](https://github.com/cli-guidelines/cli-guidelines) bir rehber.

> **Çevirmen Notu**
>
> Bu döküman [Command Line Interface Guidelines](https://clig.dev/) adlı yazıdan tercüme edilmiştir.
>
> Yazım veya çeviri yanlışı gördüğünüz yerleri bildirirseniz çok sevinirim.
>
> Son güncellenme tarihi: *8/2/2024*


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
    * [Dokümantasyon](#dokümantasyon)
    * [Çıktı](#çıktı)
    * [Hatalar](#hatalar)
    * [Argümanlar ve flagler](#argümanlar-ve-flagler)
    * [Etkileşim](#etkileşim)
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

Eğer rehberi veya CLI tasarımını tartışmak istiyorsanız, [Discord'ta bize katılın](https://discord.gg/EbAW5rUCkE).


## Önsöz

1980'lerde kişisel bilgisayarınız sizin için bir şeyler yapmasını isteseydiniz, `C:\>` veya `~$` ile karşılaştığınızda ne yazmanız gerektiğini bilmeniz gerekiyordu. Yardım, kalın, spiral ciltli kılavuzlar şeklinde gelirdi. Hata mesajları şeffaf değildi. Seni kurtaracak Stack Overflow yoktu. Ancak internet erişimine sahip olacak kadar şanslıysanız [Usenet](https://en.wikipedia.org/wiki/Usenet)'ten -*en az sizin kadar hüsrana uğramış insanlarla dolu, internet öncesi topluluk*- yardım alabilirdin. Ya sorununuzu çözmenize yardımcı olabilirlerdi ya da en azından biraz manevi destek ve dostluk sağlayabilirlerdi.

Kırk yıl sonra, bilgisayarlar herkes için çok daha erişilebilir hale geldi, bu da çoğu zaman tecrübeli son kullanıcının bilgisayar üzerinde kontrolü pahasına oldu. Çoğu cihazda komut satırı erişimi yok, bunun nedeni kısmen 4 tarafı çevrili şirketlerin ve uygulama mağazalarının kurumsal çıkarlarına aykırı olmasıdır.

Günümüzde çoğu insan komut satırının ne olduğunu bilmiyor, hatta neden bunula uğraşmak istesinler. Bilgisayar bilimleri öncüsü Alan Kay'in [2017'deki bir röportajda](https://www.fastcompany.com/40435064/what-alan-kay-thinks-about-the-iphone-and-technology-now) söylediği gibi, "İnsanlar bilgisayarların neyle ilgili olduğunu anlamadıkları için iPhone'da olduğunu sanıyorlar ve bu yanılsama 'Guitar Hero'nun gerçek bir gitarla aynı olduğu yanılsaması kadar kötü."

Kay'in bahsettiği "gerçek gitar" tam olarak CLI değil. CLI'nin gücünü sunan ve metin dosyalarında yazılan yazılımların ötesine geçen bilgisayarları programlamanın yollarından bahsediyordu. Kay'in öğrencileri arasında onlarca yıldır içinde yaşadığımız metin tabanlı zirveden kurtulmamız gerektiğine dair bir inanç var.

Bilgisayarları çok farklı şekilde programladığımız bir geleceği hayal etmek heyecan verici. Bugün bile *spreadsheet*'ler açık ara en popüler programlama dili, ve *no-code* hareketi, yetenekli programcılara yönelik yoğun talebin bir kısmında yerini almaya çalışırken hızla yayılıyor.

><b>Çevirmen Notu</b>: Spreadsheet'ler, Excel gibi verilerin tablo halinde hesaplanması, düzenlenmesi, analizi ve saklanması için kullanılan bilgisayar programları.

Ancak gıcırtılı, onlarca yıllık kısıtlamaları ve açıklanamaz tuhaflıkları ile komut satırı hala bilgisayarların en *çokyönlü* köşesidir. Perdeyi geri çekmenize, gerçekte neler olup bittiğini görmenize ve bilgisayarla *GUI*'lerin karşılayamayacağı gelişmişlik ve derinlik düzeyinde yaratıcı şekilde etkileşime girmenize olanak tanır. Öğrenmek isteyen herkes için hemen hemen her dizüstü bilgisayarda mevcuttur. İnteraktif olarak kullanılabilir veya otomatikleştirilebilir. Sistemin diğer parçaları kadar hızlı değişmezler ve dengeli olmasında yaratıcı bir değer vardır.

Bu nedenle, hâlâ elimizdeyken, faydasını ve erişilebilirliğini en üst düzeye çıkarmaya çalışmalıyız.

O ilk günlerden bu yana bilgisayarları nasıl programladığımız konusunda çok şey değişti. Geçmişteki komut satırı makine öncelikliydi -scripting platformunun üstündeki bir *REPL*'den biraz daha fazlasıydı-. Ancak genel amaçlı yorumlanan diller geliştikçe *shell script*in rolü daraldı. Günümüzünde komut satırı insan öncelikli -her türlü araca, sisteme ve platforma erişim sağlayan metin tabanlı bir kullanıcı arayüzü-. Geçmişte editör terminalin içindeydi; bugün ise terminal çoğunlukla editörün bir özelliğidir. Ve `git` benzeri çok özellikli komutlarda bir çoğalma oldu. Atomic fonksiyonlar yerine, komutların içindeki komutlar ve yüksek seviyeli komutlar tüm iş akışlarını gerçekleştirdi.

><b>Çevirmen Notu</b>: REPL(*read–eval–print loop* / oku-işle-yazdır döngüsü), kullanıcı girdilerini alan, bunları işleyen ve sonucu kullanıcıya döndüren basit, etkileşimli bir bilgisayar programlama ortamıdır.

Geleneksel UNIX felsefesinden esinlenerek, daha keyifli ve erişilebilir bir CLI ortamını teşvik etme ilgisinden yola çıkarak ve programcılar olarak deneyimlerimizin rehberliğinde, komut satırı programları oluşturmaya yönelik en iyi uygulamaları ve tasarım ilkelerini yeniden gözden geçirme zamanının geldiğine karar verdik.

<b>Yaşasın komut satırı!</b>


## Giriş

Bu belge hem tasarım mimarisi felsefesini hem de somut yönergeleri kapsamaktadır. Ayrıca bu belge, yönergeler konusunda daha ağırdır çünkü yazılımcılar olarak bizim felsefemiz çok fazla felsefe yapmak değildir. Örnekler ile öğrenmeye inanıyoruz, bu yüzden örneklerden bol miktarda var.

Bu rehber, *emacs* ve *vim* gibi tam ekran terminal programlarını kapsamaz. Tam ekran terminal programları niş projelerdir, çok azımız bir tane tasarlayabilecek konumda olacaktır.

Bu kılavuz aynı zamanda genel olarak programlama dilleri ve araçlar konusunda da tarafsızdır.

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

Geleneksel olarak UNIX komutları, öncelikli olarak diğer programlar tarafından kullanılacakları varsayımıyla yazılmıştır. Grafik uygulamalarından ziyade programlama dilindeki fonksiyonlarla daha fazla ortak noktaları vardı.

Günümüzde birçok CLI programı öncelikli olarak (veya hatta yalnızca) insanlar tarafından kullanılsa da, arayüz tasarımlarının çoğu hâlâ geçmişin yükünü taşıyor. Bu yükün bir kısmını atmanın zamanı geldi: Eğer bir komut öncelikle insanlar tarafından kullanılacaksa, önce insanlar için tasarlanmalıdır.

### Birlikte çalışan basit parçalar

[Orijinal UNIX felsefesinin](https://en.wikipedia.org/wiki/Unix_philosophy) temel ilkelerinden biri olan temiz interfacelere sahip küçük, basit programların daha büyük sistemler oluşturmak için birleştirilebileceği fikridir. Bu programlara daha fazla özellik eklemek yerine, gerektiğinde yeniden birleştirilebilecek kadar modüler programlar yaparsınız.

Eski günlerde, pipelar ve shell scriptler birlikte program oluşturma süreci çok önemli bir rol oynadı. Çok amaçlı yorumlanan dillerin yükselişiyle belki rolleri azalmış olabilir, ancak kesinlikle ortadan kalkmadılar. Dahası, CI/CD, orkestrasyon ve konfigürasyon yönetimi biçimindeki büyük ölçekli otomasyonlar gelişti. Artık programları birleştirilebilir hale getirmek her zamankinden daha önemli.

Neyse ki, tam da bu amaç için tasarlanmış UNIX ortamının köklü kuralları bugün hala bize yardımcı oluyor. Standard in/out/err, sinyaller, çıkış kodları ve diğer mekanizmalar, farklı programların birbirine güzel bir şekilde oturmasını sağladı. Düz, satır tabanlı metnin komutlar arasında aktarılması kolaydır. Çok daha yeni bir buluş olan JSON, ihtiyaç duyduğumuzda bize daha fazla yapı sağlıyor ve komut satırı araçlarını web ile daha kolay entegre etmemizi sağlıyor.

Hangi yazılımı geliştiriyor olursanız olun, insanların onu tahmin etmediğiniz şekillerde kullanacağından kesinlikle emin olabilirsiniz. Yazılımınız daha büyük bir sistemin parçası haline gelecek -tek seçeneğiniz yazılımınızın uyumlu bir parça olup olmayacağıdır.

En önemlisi, uyumluluk için tasarım yapmak, önce insan odaklı tasarım yapmakla çelişmek zorunda değildir. Bu belgedeki tavsiyelerin çoğu her ikisine de nasıl ulaşılacağıyla ilgilidir.

### Programlar arasında tutarlılık

Terminalin kuralları parmaklarımıza bağlıdır. Komut satırı sözdizimi, işaretler, ortam değişkenleri vb. hakkında bilgi edinerek ön maliyet ödemek zorunda kaldık, ancak programlar tutarlı olduğu sürece uzun vadede verimlilikle karşılığını veriyorlar.

Mümkün olduğunda bir CLI, halihazırda var olan kalıpları takip etmelidir. Bu CLI'leri sezgisel ve tahmin edilebilir kılar ayrıca kullanıcıları verimli kılan şey de budur.

Bununla birlikte bazen tutarlılık ile kullanım kolaylığı çelişiyor. Örneğin, uzun süredir kullanılan birçok UNIX komutu, varsayılan olarak fazla bilgi vermez; bu da, komut satırına aşina olmayan kişiler için kafa karışıklığına veya endişeye neden olabilir.

Kuralları takip etmek programın kullanılabilirliğini tehlikeye attığında, artık kurallardan kopmanın zamanı gelmiş olabilir -ancak böyle bir karar dikkatle verilmelidir.

### (Sadece) Söylemek yeterli
 
Terminal saf bir bilgi dünyasıdır. Bilginin arayüz olduğunu ve her arayüzde olduğu gibi bilginin genellikle çok fazla veya çok az olduğunu iddia edebilirsiniz.

Bir komut birkaç dakika boyunca çalışır kaldığında çok az şey söylüyor demektir ve kullanıcı, komutun bozuk olup olmadığını merak etmeye başlar. Bir komut, sayfalarca bilgi dökerek, gerçekten önemli olanı çıktıyı gereksiz bilgiler okyanusunda boğduğunda çok fazla şey söylüyor demektir. Nihai sonuç ikisinde de aynı: <b>net olmama</b>, bu sonuç kullanıcının kafasını karıştırır ve sinirlendirir.

Bu dengeyi doğru kurmak çok zor olabilir, ancak yazılımın, kullanıcılarına güç ve hizmet vermesi isteniliyorsa kesinlikle çok önemlidir.

### Keşif kolaylığı

Söz konusu anlaşılabilirlik olduğunda, GUI'lerin üstünlüğü vardır. Yapabileceğiniz her şey önünüzdeki ekranda sergileniyor, böylece hiçbir şey öğrenmenize gerek kalmadan ihtiyacınız olanı bulabilir ve hatta belki de mümkün olduğunu bilmediğiniz şeyleri keşfedebilirsiniz.

CLI'lerin bunun tersi olduğunu, her şeyin nasıl yapılacağını hatırlamanız gerektiği varsayılmaktadır. 1987'de yayınlanan orijinal [Macintosh Human Interface Guidelines](https://archive.org/details/applehumaninterf00appl), sanki yalnızca birini seçebiliyormuşsunuz gibi, "remember-and-type" yerine "see-and-point" önerisinde bulunuyor.

><b>Çevirmen Notu</b>: "see-and-point": GUI'lerde olan, gör ve işaretle. "remember-and-type": CLI'lerde olan, hatırla ve yaz.

Bu şeylerin birbirini dışlaması gerekmez. Komut satırını kullanmanın verimliliği, komutları hatırlamaktan gelir, ancak komutların öğrenmenize ve hatırlamanıza yardımcı olmaması için hiçbir neden yok.

Anlaşılabilir CLI'ler kapsamlı dökümantasyonlara sahiptir, çok sayıda örnek sunar, bir sonraki komut için öneride bulunur, bir hata oluştuğunda ne yapılacağını gösterir. CLI'lerin öğrenilmesini ve kullanılmasını kolaylaştırmak için tecrübeli kullanıcılar için bile GUI'lerden alınabilecek pek çok fikir vardır.

Alıntı: *The Design of Everyday Things (Don Norman), Macintosh Human Interface Guidelines*

### Norm olarak konuşma

GUI tasarımı -özellikle ilk günlerinde-, metaforlardan yoğun bir şekilde yararlanıyordu: masaüstü, dosyalar, klasörler, geri dönüşüm klasörleri. Bu çok mantıklıydı çünkü bilgisayarlar hâlâ kendilerini meşrulaştırmaya çalışıyorlardı. Metaforların uygulanmasının kolaylığı, GUI'lerin CLI'lara sağladığı en büyük avantajlardan biriydi. Ancak ironik bir şekilde, CLI başından beri tesadüfi bir metaforu somutlaştırdı: *konuşma*.

Basit komutlar dışında, bir programı gerçekten çalıştırmak için genellikle birden fazla çalıştırmak gerekir. Genellikle bunun nedeni, ilk seferde doğru yapmanın zor olmasıdır. Kullanıcı bir komut yazar, bir hata alır, komutu değiştirir, farklı bir hata alır ve bu işlem çalışana kadar devam eder. Tekrarlanan başarısızlık yoluyla öğrenme, kullanıcının programla yaptığı bir konuşmaya benzer.

Ancak deneme-yanılma, tek etkileşim türü değildir. Başkaları da vardır:

- Bir işlemi ayarlamak için bir komut çalıştırmak ve ardından onu gerçekten kullanmaya başlamak için hangi komutların çalıştırılacağını öğrenmek.
- Bir işlemi ayarlamak için birkaç komutun çalıştırılması ve ardından onu ayarlamak için son bir komutun çalıştırılması (örneğin, birden fazla `git add` ve ardından bir `git commit`).
- Bir sistemi keşfetmek (örneğin, bir dizin yapısını anlamak için çok fazla `cd` ve `ls` yapmak veya bir dosyanın geçmişini keşfetmek için `git log` ve `git show` yapmak).
- Karmaşık bir işlemi gerçek anlamda çalıştırmadan önce prova yapmak.

Komut satırı etkileşiminin konuşmaya dayalı doğasını kabul etmek, ilgili teknikleri cli tasarımına uygulayabileceğiniz anlamına gelir. Parametreler geçersiz olduğunda olası düzeltmeler önerebilir, kullanıcı çok adımlı bir süreçten geçtiğinde ara durumları netleştirebilir, korkutucu bir şey yapmadan önce her şeyin iyi gözüktüğünü onlara iletebilirsin.

İsteseniz de istemeseniz de kullanıcı yazılımınızla konuşuyor. En kötüsü, kendilerini aptal ve kırgın hissetmelerine neden olan düşmanca bir konuşmadır. En iyi ihtimalle, yeni bilgiler ve başarı duygusuyla gidecekleri yolları hızlandıran hoş bir alışveriştir.

Daha fazlası için: [The Anti-Mac User Interface (Don Gentner and Jakob Nielsen)](https://www.nngroup.com/articles/anti-mac-interface/)

### Sağlamlık

Sağlamlık hem nesnel hem de öznel bir özelliktir. Yazılım elbette sağlam olmalıdır: Beklenmedik girdiler zarifçe ele alınmalı, işlemler mümkün olduğunca bağımsız olmalıdır vb. Ancak aynı zamanda sağlam da *hissettirmelidir*.

Yazılımınızın parçalanmayacakmış gibi hissetmesini istiyorsunuz. Sanki dayanıksız bir plastik "yumuşak anahtar" değil de büyük bir mekanik makineymiş gibi hızlı ve anlaşılır olmasını istiyorsunuz.

Öznel sağlamlık, ayrıntılara dikkat etmeyi ve neyin yanlış gidebileceği hakkında iyice düşünmeyi gerektirir. Pek çok küçük şey var: kullanıcıyı olup bitenler hakkında bilgilendirmek, yaygın hataların ne anlama geldiğini açıklamak, korkutucu görünen yığın izleri yazdırmamak.

Genel bir kural olarak sağlamlık, işi basit tutmaktan da kaynaklanabilir. Pek çok özel durum ve karmaşık kod, bir programı kırılgan hale getirme eğilimindedir.

### Empati

Komut satırı araçları bir programcının yaratıcı araç takımıdır, bu nedenle kullanımı keyifli olmalıdır. Bu, onları bir video oyununa dönüştürmek veya çok fazla emoji kullanmak anlamına gelmez (ama doğal olarak bunda 😉 bir sorun yok). Bu, kullanıcıya onun yanında olduğunuzu, onun başarılı olmasını istediğinizi, sorunları ve bunların nasıl çözüleceği hakkında dikkatlice düşündüğünüz hissini vermek anlamına gelir.

Onların bu şekilde hissetmelerini sağlayacak herhangi bir eylemler listesi yok, ancak tavsiyelerimize uymanın sizi o noktaya bir miktar ulaştıracağını umuyoruz. Kullanıcıyı memnun etmek, *her fırsatta beklentilerini* aşmak anlamına gelir ve bu da empatiyle başlar.

### Kaos

Terminal dünyası tam bir karmaşa. Tutarsızlıklar her yerde ve bu bizi yavaşlatıyor, kendimizi ikinci bir tahminde bulunmamıza neden oluyor.

Ancak bu kaosa sebep olan bir güç kaynağı olduğu yadsınamaz. Terminal -genel olarak UNIX tabanlı ortamlar gibi- oluşturabileceğiniz şeyler üzerinde çok az kısıtlama getirir. Bu yüzden bu alanda her türlü icat çiçek açtı.

Bu belgenin, onlarca yıllık komut satırı geleneğiyle çelişen tavsiyelerin yanı sıra mevcut kalıpları takip etmenizi istemesi de ironik. Kuralları çiğnemekten herkes kadar biz de suçluyuz.

Sizin de kuralları çiğnemeniz gereken bir zaman gelebilir. Geldiği zaman bunu niyetle ve amacınızın netliğiyle yapın.

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

### Dokümantasyon

[Yardım metninin](#yardım) amacı, programın ne olduğu, hangi seçeneklerin mevcut olduğu ve en yaygın görevlerin nasıl gerçekleştirileceği hakkında kısa ve hızlı şekilde bilgi vermektir. Öte yandan dokümantasyon, tüm ayrıntılara girdiğiniz yerdir. Burası insanların, programınızın ne işe yaradığını, ne işe yaramadığını, nasıl çalıştığını ve ihtiyaç duyabilecekleri her şeyi nasıl yapabileceklerini anlayacakları yerdir.

**Web tabanlı dokümantasyon sağlayın.** İnsanların programınınız dokümantasyonunu çevrimiçi olarak arayabilmeleri ve diğer kişilerle belirli bölümleri paylaşabilmeleri gerekir. Web mevcut olan en kapsayıcı dokümantasyon formatıdır.

**Terminal tabanlı dokümantasyon sağlayın.** Terminaldeki dokümantasyonun birkaç güzel özelliği vardır: erişimi hızlıdır, programın yüklü sürümüyle senkronize kalır ve internet bağlantısı olmadan çalışır.

**Man sayfaları sağlamayı dikkate alın.** Unix'in orijinal dokümantasyon sistemi olan [man sayfaları](https://en.wikipedia.org/wiki/Man_page) günümüzde hala kullanılmaktadır ve birçok kullanıcı programınınz hakkında bilgi edinmeye çalışırken ilk adım olarak `man mycmd`'yi refleks olarak kontrol edecektir. Dokümantasyon oluşturmayı kolaylaştırmak için [ronn](http://rtomayko.github.io/ronn/ronn.1.html) gibi bir araç kullanabilirsiniz (ayrıca web tabanlı dokümantasyon da oluşturabilir).

Ancak, herkes man sayfalarını bilmiyor ve tüm platformlarda çalışmıyor, bu nedenle terminal tabanlı dokümanlarınıza programınızın kendisiyle de erişilebildiğinden emin olmalısınız. Örneğin, `git` ve `npm`, man sayfalarını `help` alt komutu aracılığıyla erişilebilir kılar, bu nedenle `npm help ls` ve `man npm-ls` eşdeğerdir.

```
NPM-LS(1)                                                            NPM-LS(1)

NAME
       npm-ls - List installed packages

SYNOPSIS
         npm ls [[<@scope>/]<pkg> ...]

         aliases: list, la, ll

DESCRIPTION
       This command will print to stdout all the versions of packages that are
       installed, as well as their dependencies, in a tree-structure.

       ...
```

### Çıktı

**Okunur çıktılar çok önemli.** Önce insan, sonra makineler gelir. En basit ve doğrudan bir çıktının (stdout veya stderr) olup olmadığını anlamanın yolu *TTY* olup olmadığıdır. Hangi dili kullanıyor olursanız olun, bunu yapmak için bir yardımcı program veya kitaplık bulunacaktır (örn. [Python](https://stackoverflow.com/questions/858623/how-to-recognize-whether-a-script-is-running-on-a-tty), [Node](https://nodejs.org/api/process.html#process_a_note_on_process_i_o), [Go](https://github.com/mattn/go-isatty)).

Daha fazlası için: [what a TTY is](https://unix.stackexchange.com/questions/4126/what-is-the-exact-difference-between-a-terminal-a-shell-a-tty-and-a-con/4132#4132)

**Kullanılabilirliği etkilemediği yerlerde makine tarafından okunabilir bir çıktıya sahip olun.** Metin akışları UNIX'teki evrensel arayüzdür. Programlar genellikle metin satırı basar veya girdi olarak metin satırı bekler, bu nedenle birden fazla programı bir arada kullanabilirsiniz. Bu normalde script yazabilmek için yapılır, ancak aynı zamanda programları kullanan insanların kullanılabilirliğine de yardımcı olabilir. Örneğin, bir kullanıcı çıktıyı `grep`'e yönlendirebilmelidir.

"Her programın çıktısını başka bir programın girdisi olarak bekleyin" — Doug McIlroy

**İnsan tarafından okunabilir çıktı makine tarafından okunabilir çıktıyı keserse, çıktıyı `grep` veya `awk` gibi araçlarla entegrasyon için düz ve tablo biçiminde görüntülemek için `--plain` kullanın.** Bazı durumlarda, çıktıyı insan tarafından okunabilir hale getirmek için farklı bir şekilde çıktı almanız gerekebilir.

Örneğin, satır satır bir tablo görüntülüyorsanız, ekran boyutundan dolayı bilgiyi tabloya sığdırmak için birden çok satıra bölmeyi seçebilirsiniz. Bu durum satır başına bir veri beklenen davranışını bozar, bu nedenle scriptler için tüm bu işlemleri devre dışı bırakan ve satır başına bir kayıt çıkaran `--plain` flagini sağlamalısınız.

**`--Json` flagi eklenirse çıktıyı JSON olarak görüntüle.** JSON, düz metinden daha fazla yapıya izin verir, bu nedenle karmaşık veri yapılarının çıktısını almayı ve işlemeyi çok daha fazla kolaylaştırır. [jq](https://jqlang.github.io/jq/), komut satırında JSON ile çalışmak için yaygın olarak kullanıran bir araç ve artık json'u çıkaran ve manipüle eden [çok fazla araç](https://ilya-sher.org/2018/04/10/list-of-json-tools-for-command-line/) var.

Web'de de yaygın olarak kullanılır, bu nedenle `curl` kullanarak web servislerini doğrudan programınıza girdi veya çıktı olarak kullanabilirsiniz. 

**Başarılı olan durumlarda çıktıyı görüntüleyin, ancak kısa tutun.** Geleneksel olarak, yanlış bir şey olmadığında, UNIX komutları kullanıcıya herhangi bir çıktı göstermez. Bu, scriptlerde kullanıldıklarında anlamlıdır ama insanlar tarafından kullanıldığında komutların askıda kaldıklarını veya bozuk olduğunu düşünmelerine neden olabilir. Örneğin, `cp` çalışması uzun zaman alsa bile hiçbir şey yazdırmaz.

Hiçbir şey yazdırmamak nadiren en iyi varsayılan davranıştır, ama err kullanmaktan daha iyidir.

Çıktı istemediğiniz durumlarda (örneğin, script dosyalarında), `stderr`'nin `/dev/null`'a beceriksiz bir şekilde yönlendirilmesini önlemek için, gerekli olmayan tüm çıktıları `-q` seçeneği ile susturabilirsiniz.

**Eğer mevcut durumu değiştiriyorsanız, kullanıcıya söyleyin.** Bir komut sistemin durumunu değiştirdiğinde, ne yaptığını açıklamak çok değerlidir, böylece kullanıcı sistemin durumunu kafasında modelleyebilir -özellikle sonuç doğrudan kullanıcının istediğiyle eşleşmiyorsa.

Örneğin, `git push` size tam olarak ne yaptığını ve yeni durumunun ne olduğunu söyler:

```
$ git push
Enumerating objects: 18, done.
Counting objects: 100% (18/18), done.
Delta compression using up to 8 threads
Compressing objects: 100% (10/10), done.
Writing objects: 100% (10/10), 2.09 KiB | 2.09 MiB/s, done.
Total 10 (delta 8), reused 0 (delta 0), pack-reused 0
remote: Resolving deltas: 100% (8/8), completed with 8 local objects.
To github.com:replicate/replicate.git
 + 6c22c90...a2a5217 bfirsh/fix-delete -> bfirsh/fix-delete
```

**Sistemin mevcut durumunu görmeyi kolaylaştırın.** Programınız çok karmaşık durum değişiklikleri yapıyorsa ve dosya sisteminde hemen görünmüyorsa, bunu görüntülemeyi kolaylaştırdığınızdan emin olun.

Örneğin, `git status` size Git deponuzun mevcut durumu hakkında mümkün olduğunca fazla bilgi verir ve durumu nasıl değiştireceğinize dair bazı ipuçları verir:

```
$ git status
On branch bfirsh/fix-delete
Your branch is up to date with 'origin/bfirsh/fix-delete'.

Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
	modified:   cli/pkg/cli/rm.go

no changes added to commit (use "git add" and/or "git commit -a")
```

**Kullanıcının çalıştırması gereken komutları önerin.** Birkaç komut bir iş akışı oluşturduğunda, kullanıcıya bir sonraki komutlarını çalıştırabileceklerini önermek, programınızı nasıl kullanacaklarını öğrenmelerine ve yeni işlevler keşfetmelerine yardımcı olur. Örneğin, yukarıdaki `git status` çıktısında, görüntülemekte olduğunuz durumu değiştirmek için çalıştırabileceğiniz komutları öneriyor.

**Programın iç dünyasının sınırını aşan eylemler hakkında genellikle açık olun.** Bu, aşağıdaki örnekleride kapsar:

- Kullanıcının açıkça argüman olarak iletmediği dosyaları okuma veya yazma (eğer bu dosyalar önbellek gibi dahili program durumunda saklanmıyorsa).
- Uzak bir sunucuyla konuşmak, örneğin bir dosyayı indirmek.

**Bilgi yoğunluğunu artırın-ASCII sanatı ile !** Örneğin, `ls` izinleri taranabilir bir şekilde gösterir. İlk gördüğünüzde, bilgilerin çoğunu görmezden gelebilirsiniz. Sonra, nasıl çalıştığını öğrendikçe, zaman içinde daha fazla kalıp seçersiniz.

```
-rw-r--r-- 1 root root     68 Aug 22 23:20 resolv.conf
lrwxrwxrwx 1 root root     13 Mar 14 20:24 rmt -> /usr/sbin/rmt
drwxr-xr-x 4 root root   4.0K Jul 20 14:51 security
drwxr-xr-x 2 root root   4.0K Jul 20 14:53 selinux
-rw-r----- 1 root shadow  501 Jul 20 14:44 shadow
-rw-r--r-- 1 root root    116 Jul 20 14:43 shells
drwxr-xr-x 2 root root   4.0K Jul 20 14:57 skel
-rw-r--r-- 1 root root      0 Jul 20 14:43 subgid
-rw-r--r-- 1 root root      0 Jul 20 14:43 subuid
```

**Rengi bir amaçla kullanın.** Örneğin, kullanıcının fark etmesi için bir metni vurgulamak veya bir hatayı belirtmek için kırmızı rengi kullanmak isteyebilirsiniz. Aşırı kullanmayın—her şey farklı renkteyse, o zaman rengin hiçbir anlamı yoktur ve yalnızca okumayı zorlaştırır.

**Programınız bir terminalde değilse veya kullanıcı bunu talep ettiyse rengi devre dışı bırakın.** Bunlar renkleri devre dışı bırakmalıdır:

- Eğer `stdout` veya `stderr` etkileşimli bir terminal (TTY) değilse. Tek tek kontrol etmek en iyisidir; eğer `stdout`'u başka bir programa aktarıyorsanız, `stderr`'yi renkli görmek yine de faydalıdır.
- Eğer `NO_COLOR` değişkeni ayarlandıysa.
- Eğer `TERM` değişkeni `dumb` değerine sahipse.
- Eğer kullanıcı `--no-color` flagini belirttiyse.
- Kullanıcıların programınız için rengi devre dışı bırakmak istemesi durumunda, kullanıcılardan bir `MYAPP_NO_COLOR` değişkeni eklemelerini isteyebilirsiniz.

Daha fazlası için: [no-color.org](https://no-color.org/), [12 Factor CLI Apps](https://medium.com/@jdxcode/12-factor-cli-apps-dd3c227a0e46)

**Eğer `stdout` etkileşimli bir terminal değilse herhangi bir animasyon görüntülemeyin.** Bu, CI çıktısındaki ilerleme çubuklarının Noel ağaçlarına dönüşmesini engelleyecektir.

**İşleri daha net hale getirecek semboller ve emojiler kullanın.** Birkaç şeyi belirginleştirmeniz, kullanıcının dikkatini çekmeniz veya sadece biraz karakter eklemeniz gerekiyorsa resimler kelimelerden daha iyi olabilir. Dikkat olun çünkü abartabilirsiniz ve programınızın karışık görünmesine veya oyuncak gibi görünmesine neden olabilir.

Örneğin, [yubikey-agent] çıktının sadece metin duvarı olmamaması için emoji kullanarak yapılandırıyor ve önemli bir bilgiye dikkatinizi çekmek için ❌ kullanıyor:

```
$ yubikey-agent -setup
🔐 The PIN is up to 8 numbers, letters, or symbols. Not just numbers!
❌ The key will be lost if the PIN and PUK are locked after 3 incorrect tries.

Choose a new PIN/PUK: 
Repeat the PIN/PUK: 

🧪 Retriculating splines …

✅ Done! This YubiKey is secured and ready to go.
🤏 When the YubiKey blinks, touch it to authorize the login.

🔑 Here's your new shiny SSH public key:
ecdsa-sha2-nistp256 AAAAE2VjZHNhLXNoYTItbmlzdHAyNTYAAAAIbmlzdHAyNTYAAABBBCEJ/
UwlHnUFXgENO3ifPZd8zoSKMxESxxot4tMgvfXjmRp5G3BGrAnonncE7Aj11pn3SSYgEcrrn2sMyLGpVS0=

💭 Remember: everything breaks, have a backup plan for when this YubiKey does.
```

**Varsayılan olarak, yalnızca yazılımın yaratıcılarının anlayabileceği bilgileri çıkartmayın.** Eğer bir çıktı parçası yalnızca sizin, (geliştiricinin) yazılımınızın ne yaptığını anlamanıza yardımcı oluyorsa, kesinlikle normal kullanıcılara varsayılan olarak görüntülenmemelidir; yalnızca ayrıntılı modda görüntülenmelidir.

Dışarıdan ve projenizde yeni olan kişilerden kullanılabilirlik geri bildirimlerini davet edin. Farkedemeyecek kadar yakın baktığınız kodun önemli sorunlarını görmenize yardımcı olacaklardır.

**En azından varsayılan olarak `stderr`'e bir log dosyası gibi davranmayın.** Ayrıntılı modda olmadığı sürece günlük düzeyindeki etiketleri (ERR, WARN, vb.) veya konu dışı bağlamsal bilgileri yazdırmayın.

**Çok fazla metin çıktısı alıyorsanız bir sayfalayıcı kullanın (örn. `less`).** Örneğin, `git diff` bunu varsayılan olarak yapıyor. Sayfalayıcı kullanmak hataya açık olabilir; bu nedenle, kullanıcı deneyimini daha da kötüleştirmemek için uygulamanıza dikkat edin. Eğer `stdin` veya `stdout` etkileşimli bir terminal değilse sayfalayıcı kullanmamalısınız.

`less` kullanmak için mantıklı olan parametreler `less -FIRX`'tir. Bu, içerik bir ekranı dolduruyorsa ekstra sayfa açmaz, arama yaptığınızda büyük/küçük harf dikkate alınmaz, renk ve biçimlendirmeyi etkinleştirir ve `less`'ten çıkıldığında içeriği ekranda bırakır.

Dilinizde `less` yerine daha sağlam kütüphaneler olabilir. Örneğin Python'daki [pypager](https://github.com/prompt-toolkit/pypager).

### Hatalar

Dokümantasyonlara başvurmanın en yaygın nedenlerinden biri hataları düzeltmektir. Dokümantasyonda hataları işleyebilirseniz, bu kullanıcıya çok fazla zaman kazandıracaktır.

**Hataları yakalayın ve [bunları insanlar için yeniden yazın](https://www.nngroup.com/articles/error-message-guidelines/).** Bir hatanın oluşmasını bekliyorsanız, onu yakalayın ve yararlı olması için hata mesajını yeniden yazın. Bunu kullanıcının yanlış bir şey yaptığı ve programın onu doğru yöne yönlendirdiği bir konuşma gibi düşünün. Örnek: “file.txt dosyasına yazılamıyor.  'chmod +w file.txt' komutunu çalıştırarak onu yazılabilir hale getirmeniz gerekebilir."

**Sinyal-gürültü oranı çok önemlidir.** Ne kadar alakasız çıktı üretirseniz kullanıcının neyi yanlış yaptığını anlaması o kadar uzun sürer. Programınız aynı türde birden fazla hata üretiyorsa, benzer görünen birçok satır yazdırmak yerine bunları tek bir açıklayıcı başlık altında gruplandırmayı düşünün.

**Kullanıcının ilk olarak nereye bakacağını düşünün.** En önemli bilgiyi çıktının sonuna koyun. Gözler kırmızı metne çekilecektir, bu nedenle bunu bilinçli ve dikkatli kullanın.

**Beklenmeyen veya açıklanamayan bir hata varsa hata ayıklama bilgilerini, geri izleme bilgilerini ve hatanın nasıl gönderileceğine ilişkin talimatları sağlayın.** Bununla birlikte, sinyal-gürültü oranını unutmayın: Kullanıcıyı anlamadığı bilgilerle bunaltmak istemezsiniz. Log günlüğünü terminale yazdırmak yerine bir dosyaya yazmayı düşünün.

**Hata raporlarını göndermeyi zahmetsiz hale getirin.** Yapabileceğiniz güzel şeylerden biri, bir URL sağlamak ve mümkün olduğunca fazla bilgiyi önceden doldurmasını sağlamaktır.

### Argümanlar ve flagler

- Argümanlar, veya args, bir komutun konumsal parametreleridir. Örneğin, `cp`'ye sağladığınız dosya yolları args'tır. Arg'ların sırası genellikle önemlidir: `cp foo bar`, `cp bar foo`'dan farklı bir anlama gelir.
- Flagler, kısa çizgi ve tek harfli adla (`-r`) veya çift kısa çizgi ve çok harfli adla (`--recursive`) gösterilen adlandırılmış parametrelerdir. Ayrıca kullanıcı tarafından belirlenen bir değer içerebilir veya içermeyebilir (`--file foo.txt` veya `--file=foo.txt`). Genel anlamda flaglerin sırası programın akışını etkilemez.

**Flagleri argümanlara tercih edin.** Biraz daha fazla yazı yazmak gerekiyor ama neler olup bittiğini çok daha net bir şekilde ortaya koyuyor. Ayrıca ileride girdiyi kabul etme şeklinizde değişiklik yapmanızı da kolaylaştırır. Bazen argümanları kullanırken mevcut davranışı bozmadan veya belirsizlik yaratmadan yeni girdi eklemek imkansızdır.

*Alıntı: [12 Factor CLI Apps.](https://medium.com/@jdxcode/12-factor-cli-apps-dd3c227a0e46)*

**Bütün flaglerin tam uzunluktaki versiyonlarına sahip olun.** Örneğin hem `-h` hem de `--help`'e sahip olun. Tam uzunluktaki sürüme sahip olmak, ayrıntılı ve açıklayıcı olmak istediğiniz scriptlerde kullanışlıdır ve flaglerin anlamını her yerde aramanıza gerek yoktur.

*Alıntı: [GNU Coding Standards.](https://www.gnu.org/prep/standards/html_node/Command_002dLine-Interfaces.html)*

**Yaygın olarak kullanılan flagler için yalnızca tek harfli flag kullanın,** özellikle alt komutları kullanırken en üst düzeyde yalnızca tek harfli işaretler kullanın. Bu şekilde ad alanınızı kısa flaglerle "kirletmezsiniz" ve gelecekte ekleyeceğiniz flagler için sizi karmaşık harfler ve büyük/küçük harf kullanmaya zorlamazsınız.

**Birden çok dosyaya basit eylemler için çoklu argüman kullanımı uygundur.** Örneğin, `rm file1.txt file2.txt file3.txt`. Bu aynı zamanda evrensel çalışmasını sağlar: `rm *.txt`.

**Farklı şeyler için iki veya daha fazla argümanınız varsa, muhtemelen yanlış bir şey yapıyorsunuzdur.** Bir istisna olarak kısalığın ezberlenmeye değer olduğu, birincil eylemler. Örneğin, `cp <source> <destination>`.

*Alıntı: [12 Factor CLI Apps.](https://medium.com/@jdxcode/12-factor-cli-apps-dd3c227a0e46)*

**Bir standart varsa, flagler için standart adlar kullanın.** Yaygın olarak kullanılan başka bir komut bir flag adı kullanıyorsa, mevcut modeli takip etmek en iyisidir. Bu şekilde, kullanıcının iki farklı seçeneği (hangisin hangi komuta uygulandığını) hatırlaması gerekmez ve hatta kullanıcılar yardım metnine bakmak zorunda kalmadan bir seçeneği tahmin edebilir.

Yaygın olarak kullanılan seçeneklerin bir listesi:

- `-a`, `--all`: Hepsi. Örneğin `ps`, `fetchmail`.
- `-d`, `--debug`: Hata ayıklama çıktısını göster.
- `-f`, `--force`: Zorla. Örneğin `rm -f` komutu, yeterli izinlerin olmadığını düşünse bile dosyaların kaldırılmasını zorlayacaktır. Bu aynı zamanda, genellikle kullanıcı onayı gerektiren, yıkıcı bir şey yapan komutlar için de kullanışlıdır, ancak onu bir komut dosyasında bu yıkıcı eylemi yapmaya zorlamak istiyorsunuz.
- `--json`, JSON çıktısını görüntüle. [Çıktı](#çıktı) bölümüne bakın.
- `-h`, `--help`: Yardım. Bu sadece yardım anlamına gelmelidir. [Yardım](#yardım) bölümüne bakın.
- `--no-input`: [Etkileşim](#etkileşim) bölümüne bakın.
- `-o`, `--output`: Çıktı dosyası. Örneğin, `sort`, `gcc`.
- `-p`, `--port`: Port. Örneğin `psql`, `ssh`.
- `-q`, `--quiet`: Sessiz. Daha az çıktı göster. Bu, özellikle bir script dosyasını çalıştırırken gizlemek isteyebileceğiniz çıktıları insanlara yönelik görüntülerken kullanışlıdır.
- `-u`, `--user`: Kullanıcı.  Örneğin `ps`, `ssh`.
- `--version`: Version.
- `-v`: Bu genellikle ayrıntılı mod veya version anlamına gelebilir. Ayrıntılı mod için `-d`'yi ve version için `-v`'yi veya karışıklığı önlemek için hiçbirini kullanmayabilirsiniz.

**Çoğu kullanıcı için varsayılanı doğru yapın.** İşleri yapılandırılabilir hale getirmek iyidir, ancak çoğu kullanıcı doğru flagi bulamayacak ve onu her zaman kullanmayı (veya onun takma adını) hatırlamayacaktır.

><b>Çevirmen Notu</b>: Takma ad çevirisi alias'ın yerine kullanılmıştır.

Örneğin `ls`, script dosyalarından ve diğer tarihsel nedenlerle optimizasyon için varsayılan olarak kısa ve öz çıktıya sahiptir, ancak bugün tasarlanmış olsaydı muhtemelen varsayılan olarak `ls -lhF` olurdu.

**Kullanıcı girdisi isteyin.** Bir kullanıcı bir argümanı veya flagi iletmezse, bunu isteyin. (Ayrıca bakınız: [Etkileşim](#etkileşim))

**Asla bir komut istemine gerek duymayın.** Her zaman flaglerle veya argümanlarla girdi aktarmanın bir yolunu sağlayın. Eğer `stdin` etkileşimli bir terminal değilse, istemi atlayın ve yalnızca gerekli flag/arg'leri isteyin.

**Tehlikeli bir şey yapmadan önce onaylayın.** Yaygın bir kural olarak kullanıcı, etkileşimli olarak çalışıyorsa `y` veya `yes` yazmasını veya `-f` veya `--force` flagini kullanmasını istemektir.

”Tehlike" öznel bir terimdir ve farklı tehlike seviyeleri vardır:

- **Hafif**: Bir dosyayı silmek gibi küçük, yerel bir değişikliktir. Onay isteyebilirsiniz veya istemeyebilirsiniz. Örneğin, kullanıcı açıkça "delete" gibi bir komut çalıştırıyorsa, muhtemelen sormanıza gerek yoktur.
- **Orta**: Bir dizini silmek gibi daha büyük bir yerel değişiklik, bir tür kaynağı silmek gibi uzak bir değişiklik veya kolayca geri alınamayacak karmaşık bir toplu değişiklik. Genellikle burada onay istersiniz. Kullanıcı çalıştırmadan önce ne olacağını görebilmeleri için işlemi "dry run (prova)" yolu ile vermeyi düşünün.
- **Ağır**: Uzak bir uygulamanın veya sunucunun tamamı gibi karmaşık bir şeyin silinmesi. Burada sadece onay istemek istemezsiniz — kazara onaylamayı zorlaştırmak istiyorsunuz. Onlardan, sildikleri şeyin adı gibi önemsiz olmayan bir şey yazmalarını isteyebilirsiniz. Alternatif olarak `--confirm = "dosya adı"` gibi bir flag geçmelerine izin verin, böylece hala script dosyalarına yazılabilir olur.

Bir şeyleri kazara yok etmenin yollarının olup olmadığını düşünün. Örneğin, ayar dosyasındaki bir sayıyı 10'dan 1'e değiştirmenin, 9 şeyi örtülü olarak sileceği anlamına geldiği bir durumu düşünün (bu ciddi bir risk olarak görülmeli ve kazara yapılması zor olmalıdır).

**Giriş veya çıkış bir dosya ise, `-` ile `stdin`'den okumayı veya `stdout`'a yazmayı destekleyin.** Bu, geçici bir dosya kullanmadan, başka bir komutun çıktısının sizin komutunuzun girişi olmasını veya tam tersini sağlar. Örneğin tar, `stdin`'den dosya çıkarabilir:

```
$ curl https://example.com/something.tar.gz | tar xvf -
```

**Eğer bir flag isteğe bağlı bir değeri kabul edebiliyorsa "none" gibi özel bir kelimeye izin verin.** Örneğin, `ssh -F`, alternatif bir `ssh_config` dosyasını isteğe olarak bağlı alır ve `ssh -F none`, SSH'yi ayar dosyası olmadan çalıştırır. Yalnızca boş bir değer kullanmayın; bu, argümanların, bir flag değeri mi yoksa bir argüman mı olduğu konusunda belirsizliğe neden olabilir.

**Mümkünse argümanları, flagleri ve alt komutları sıradan bağımsız yapın.** Pek çok CLI'nin, özellikle de alt komutları olanların, çeşitli argümanları nereye koyabileceğiniz konusunda söylenmemiş kuralları vardır. Örneğin bir komutta, yalnızca alt komutun önüne koyarsanız çalışan `--foo` flagi bulunabilir:

```
mycmd --foo=1 subcmd
works

$ mycmd subcmd --foo=1
unknown flag: --foo
```

Bu, kullanıcı için çok kafa karıştırıcı olabilir; özellikle de kullanıcıların bir komutu çalıştırırken yaptıkları en yaygın şeylerden biri olan son komutu geri almak için yukarı oka basması, sonuna başka bir seçenek yapıştırması ve onu çalıştırmak olduğu göz önüne alındığında tekrar çalıştırması gibi. Mümkünse her iki formu da eşdeğer hale getirmeye çalışın, ancak argüman ayrıştırıcınızın sınırlamalarıyla karşılaşabilirsiniz.

**Sırları doğrudan flaglerden okumayın.** Bir komut bir sırrı kabul ettiğinde, örneğin `--password` argümanı aracılığıyla, argüman değeri `ps` çıktısında ve potansiyel olarak shell geçmişinde sızacaktır. Ve bu tür bir bayrak, sırlar için güvenli olmayan ortam değişkenlerinin kullanımını teşvik eder.

Hassas verileri yalnızca dosyalar yoluyla, örneğin `--password-file` argümanıyla veya `stdin` aracılığıyla kabul etmeyi düşünün. `--password-file` argümanı, bir sırrın gizlice çok çeşitli yollarla iletilmesine olanak tanır.

(`--password $(< password.txt)` komutunu kullanarak bir dosyanın içeriğini Bash'teki bir değişkene aktarmak mümkündür. Bu yaklaşım, dosyanın içeriğinin `ps` çıktısına sızması gibi güvenlik sorununa sahiptir. Bundan kaçınmak en iyisidir.) 

### Etkileşim

**Yalnızca `stdin` etkileşimli bir terminal (TTY) ise istemleri veya etkileşimli öğeleri kullanın.** Bu, verileri bir komuta mı aktardığınızı yoksa bir script dosyasında mı çalıştırıldığını anlamanın oldukça güvenilir bir yoludur; bu durumda istem çalışmaz ve kullanıcıya hangi flagi ileteceğini söyleyen bir hata iletmeniz gerekir.

**Eğer `--no-input` iletilirse, istemde bulunmayın veya etkileşimli herhangi bir şey yapmayın.** Bu, kullanıcılara komutlardaki tüm istemleri devre dışı bırakmanın açık bir yolunu sağlar. Eğer komut girdi istiyorsa, çalıştırmayın ve kullanıcıya girdiyi flag olarak nasıl ileteceğini söyleyin.

**Eğer bir şifre soruyorsanız, kullanıcı yazarken şifreyi yazdırmayın.** Bu, terminaldeki echo'yu kapatarak yapılır. Bunu yapmak için kullandığınız dilin yardımcı kütüphaneleri olmalıdır.

**Kullanıcının programdan çıkmasına izin ver.** Programdan nasıl çıkılacağını açıkça belirtin. (Vim'in yaptığını yapmayın :D) Eğer programınız network, I/O vb. durumlarda takılı kalırsa, her zaman `Ctrl-C`'nin çalışır kalmasını sağlayın. Eğer programınız çok katmanlıysa ve Ctrl-C ile çıkılmadığı durumlarda (SSH, tmux, telnet, vb.) bunun nasıl yapılacağını kullanıcıya açıkça belirtin. Örneğin SSH, `~` kaçış karakteriyle çıkışa izin verir.

### Alt komutlar

Yeterince karmaşık bir aracınız varsa, bir dizi alt komut oluşturarak karmaşıklığını azaltabilirsiniz. Birbiriyle yakından ilişkili birkaç aracınız varsa, bunları tek bir komutta birleştirerek bunların kullanımını ve keşfedilmesini kolaylaştırabilirsiniz (örneğin, RCS ve Git).

Tek bir komutta birleşmesi genel flagler, yardım metni, ayarlamalar, depolama mekanizmaları gibi şeyleri paylaştırmak için kullanışlıdır.

**Alt komutlarda tutarlı olun.** Aynı şeyler için aynı flag adlarını kullanın, benzer çıktı formatına sahip olun, vb.

**Birden fazla alt komut düzeyi için tutarlı adlar kullanın.** Eğer karmaşık bir yazılım parçasında çok sayıda nesne ve nesneler üzerinde gerçekleştirilebilecek işlemler varsa, bunun için birinin *isim* ve diğerinin *fiil* olduğu iki düzeyde alt komut kullanmak yaygın bir kalıptır. Örneğin, `docker container create`. Farklı nesne türlerinde kullandığınız fiiller konusunda tutarlı olun.

`isim fiil` veya `fiil isim` sıralaması işe yarar, ancak `isim fiil`'in daha yaygın olduğu görülmektedir.

Daha fazlası için: [User experience, CLIs, and breaking the world, by John Starich](https://uxdesign.cc/user-experience-clis-and-breaking-the-world-baed8709244f).

**Belirsiz veya benzer adlara sahip komutlarınız olmasın.** Örneğin “update” ve “upgrade” adında iki alt komutun olması oldukça kafa karıştırıcıdır. Farklı kelimeler kullanarak veya fazladan kelimelerle belirsizliği ortadan kaldırmak isteyebilirsiniz.