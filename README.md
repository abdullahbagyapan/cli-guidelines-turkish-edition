# Command Line Interface Guidelines


Geleneksel UNIX ilkelerini alıp günümüze uygun şekilde güncelleyerek daha iyi komut satırı programları yazmanıza yardımcı olacak açık kaynaklı bir rehber.

> **Çevirmen Notu**
>
> Bu döküman [Command Line Interface Guidelines](https://clig.dev/) adlı yazıdan tercüme edilmiştir.
>
> Yazım veya çeviri yanlışı gördüğünüz yerleri bildirirseniz çok sevinirim.
>
> Son güncellenme tarihi: *4/2/2024*


## İçindekiler

* [Önsöz](#önsöz)
* [Giriş](#giriş)
* [Felsefe](#felsefe)
    * [İnsan odaklı tasarım](#i̇nsan-odaklı-tasarım)
    * [Birlikte çalışan basit parçalar](#birlikte-çalışan-basit-parçalar)
    * [Programlar arasında tutarlılık](#programlar-arasında-tutarlılık)
    * [(Sadece) Söylemek yeterli](#sadece-söylemek-yeterli)
    * [Keşif kolaylığı](#keşif-kolaylığı)
    * Conversation as the norm
    * Robustness
    * Empathy
    * Chaos

* Guidelines
    * The Basics
    * Help
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