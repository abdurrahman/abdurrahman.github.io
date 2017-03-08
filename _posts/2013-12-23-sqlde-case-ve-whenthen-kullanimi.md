---
layout: post
title: SQL'de Case ve When/Then Kullanımı
date: 2013-12-23 03:31:47 +02:00
abstract: Sql Server üzerinde yaptığımız işlerde, yine kod tarafında çok büyük bir kolaylık sağlayan case ve when/then kullanımına değineceğiz. Nasıl çalıştığına bir baka...
---

Sql Server üzerinde yaptığımız işlerde, yine kod tarafında çok büyük bir kolaylık sağlayan case ve when/then kullanımına değineceğiz. Nasıl çalıştığına bir bakalım;

### Genel Syntax

{% highlight sql %}
SELECT (CASE KolonAdi WHEN 'Kolondan Gelen Değer' THEN 'Result'da gosterilecek değer' end) from TabloAdi
{% endhighlight %}

Sorgu yaparken yukarıda bahsettiğimiz gibi case uygulanacak kolonda when kısmına kolondaki değeri then kısmına da sonuç olarak yazdırmak istediğimiz değeri veririz. Örneğimizle birlikte daha kolay anlayacağınızı düşünüyorum, buyrun;

{% highlight sql %}
select * from HumanResources.Employee -- İlk önce kullanabileceğimiz kolonlara bi bakalım.

select (case Gender
  when 'M' then 'Bay' -- Male(Male) ise Bay yazdıralım.
  when 'F' then 'Bayan' -- F(Female) ise Bayan yazdıralım.
end) as [Cinsiyet],
  (case MarialStatus
  when 'M' then 'Evli' -- M(Married) ise Evli yazdıralım.
  when 'S' then 'Bekar' -- S(Single) ise Bekar yazdıralım.
end) as [Evlilik Durumu] from HumanResources.Employee
{% endhighlight %}

Kısaca örnekte ne yaptık; AdventureWorks veritabanımızdan insan kaynakları tablomuzu case’imize atayıp iki kolonu(Cinsiyet ve Evlilik Durumu) da case için kullandık. İngilizce olan değerleri when’e denk gelen kolon değerini then ifademizle Türkçeye çevirdik ve aşağıdaki gibi sorgumuzu sonuçlandırdık.

<img alt="case-then-in-sql" src="{{ site.baseurl }}/assets/case-then-in-sql-300x273.jpg" width="300" height="273" />

Yine AdventureWorks üzerinden gidelim. Bu sefer ürün tablomuzdan bi kaç renk alıp case ile when/then durumlarına bakalım.

{% highlight sql %}
select (case Color
  when 'Black' then 'Siyah'
  when 'Blue' then 'Mavi'
  when 'Grey' then 'Gri'
  when 'Multi' then 'Cok Renk'
  when 'Red' then 'Kırmızı'
  when 'Silver' then 'Gumus'
  when 'Silver/Black' then 'Gumus / Siyah'
  else 'Renksiz'
end) as [Renkler] from Production.Product
{% endhighlight %}

Burada ürün tablomuzdaki renk kolonumuzu case’e aldık, neticesinde renklerin ingilizce halini türkçeleştirmiş olduk.

Case ile When/Then durumlarını bir önceki dersimizdeki gibi if-else durumu ile de yapabilirdik fakat daha çok kod yazmamız gerekecekti. Herşeyin fazlası zarar diye atalarımızı da anmış olalım.Bence böyle Sql daha güzel, kısaca az kod, çok iş 😉
