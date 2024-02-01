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