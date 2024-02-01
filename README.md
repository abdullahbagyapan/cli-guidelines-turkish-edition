# Command Line Interface Guidelines


Geleneksel UNIX ilkelerini alıp günümüze uygun şekilde güncelleyerek daha iyi komut satırı programları yazmanıza yardımcı olacak açık kaynaklı bir rehber.

> **Çevirmen Notu**
>
> Bu döküman [Command Line Interface Guidelines](https://clig.dev/) adlı yazıdan tercüme edilmiştir.
>
> Yazım veya çeviri yanlışı gördüğünüz yerleri bildirirseniz çok sevinirim.
>
> Son güncellenme tarihi: *1/2/2024*


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