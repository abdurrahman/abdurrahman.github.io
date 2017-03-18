---
layout: post
title: SQL'de While Döngüsü
date: 2013-12-31 02:21:32 +02:00
abstract: Daha önceki yazılarımızda sırasıyla SQL de değişken, if-else koşul ifadeleri ve case when/then kullanımından söz etmiştik. Bugün ki makalemizde while ...
published: true
status: publish
categories:
- MS SQL
tags:
- Değişkenler
- döngüler
- iterasyon nedir
- kapsamlı örnek
- sql de kod kavramı
- sql de while döngüsü
- sql kurulum problemi
- sql problemi
- sql server kurulumu
- while döngüsü
- while döngüsü kullanımı
- while iterasyonu
meta:
  dsq_thread_id: '3001306330'
  views: '7740'
---

Daha önceki yazılarımızda sırasıyla SQL de değişken, if-else koşul ifadeleri ve case when/then kullanımından söz etmiştik. Bugün ki makalemizde while iterasyonu neler yapar, nasıl işler bunlardan bahsedeceğiz.

While bir SQL deyimi ve ya deyim bloğunun tekrarlanarak döngüde dönmesini sağlayan bir koşul belirler. Break ve Continiue sözcükleriyle döngü kontrol edilebilir.

### Genel Syntax

{% highlight sql %}
while(koşul)
begin
   -- Çalıştırılacak ifade
end
while(0=0)
begin
    print 'uykusuzadam.com'
end
{% endhighlight %}

Yukarıdaki döngü sonsuz bir döngü olduğundan sorgumuzu çalıştırdığımızda durmadan dönerek devam edecek. Sonucu görebilmek adına sorgunuzu **Studio Management** üzerinden sonlandırmanız gerekmekte.

{% highlight sql %}
declare @NeKadarDonecek int, @Sayac int -- 2 adet değişken tanımladım.
set @NeKadarDonecek = 20
set @Sayac = 0
while (sayac <= @NeKadarDonecek) -- Sayac değişkenimiz 20'den küçük olduğu sürece döngümüz çalışacak.
begin
    print 'Sayac'
    set @Sayac = @Sayac + 1 -- Sayac her seferinde artarak ekrana yazdırdık.
end
{% endhighlight %}

İlk önce declare ile int değerinde değişkenlerimizi( @NeKadarDonecek, @Sayac)tanımladık. Sonra set ile değişkenlere değerler atadık. While (sayac <= @NeKadarDonecek) ile koşulumuzu belirledik, döngümüz true olduğu sürece devam etti. set @Sayac = @Sayac + 1 ifadesi ile yukarıda belirtmiş olduğumuz değer üzerine her seferinde 1 arttırdık. Sonuç →

<img src="{{ site.baseurl }}/assets/while_dongusu-252x300.jpg" alt="while_dongusu" width="252" height="300" />

Az çok while döngüsünün mantığından bahsettik, elimde çok daha kapsamlı bir örnek daha var ama başlangıç seviyesi için fazla kafa karıştıracağından buraya eklemekten vazgeçtim. Bu kadarı kavramak için yeterli bence, sorularınız olursa iletişim formu bir tık uzağınızda… ↑

Kod yazmak biraz da zevk işidir. Kolay gelsin.

Kaynak : http://technet.microsoft.com/tr-tr/library/ms178642.aspx
