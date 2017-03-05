---
layout: post
title: SQL'e Giriş ve Değişkenler
date: 2011-08-06 16:24:21 +02:00
abstract: SQL(Structured Query Language) yapılandırılmış sorgu dili olarak çevirilir. Kısaca tanımlamak gerekirse bir veritabanı oluşturmak, ve bu veritabanında bilgiler saklamak,...
---

SQL(Structured Query Language) yapılandırılmış sorgu dili olarak çevirilir. Kısaca tanımlamak gerekirse bir veritabanı oluşturmak, ve bu veritabanında bilgiler saklamak, istenildiğinde de bu bilgilere ulaşmak için kullanılan platformdur diyebiliriz.

SQL tanımlamadan sonra değişkenlere giriş yapabiliriz. Daha iyi anlayabilmek için C# daki değişkenler ile karşılaştırmalara yer vereceğim.

C#’da değişken tanımlarken mesela -string Ad- şeklinde tanımlarız. Bu durum SQL’de de mantık olarak aynı fakat syntax(söz dizimi) farkı vardır:

{% highlight sql %}
declare  @Ad nvarchar(20) -- şeklinde tanımlama yaparız..
{% endhighlight %}

C#’da (yukarıda verdiğimiz örneğe göre belirlersek) oluşturduğumuz değişkene -Ad = “Simon”- şeklinde atama yaparken, SQL’de bu işlem şöyledir;

{% highlight sql %}
set @Ad = ‘Simon’
{% endhighlight %}

Az önce Ad değişkenimize isim olarak “Simon” atanmış oldu, C#’da durumu ekranda görmek istersek Conrtol.WriteLine metodu kullanılırız. SQL’de bunun karşılık olarak ifadesi print dir.

{% highlight sql %}
print = @Ad
{% endhighlight %}

Yukarıdaki TSQL  yapısında biz bir ad değişkeni tanımladık ve çıktı olarak mesaj bölümünde tanımladığımız değişkene verdiğimiz değeri ekrana yazdırdık…

Birden fazla değişken tanımlamamız gerektiğinde C#’da – string ad,soyad, etc; – şeklinde tanımlarız. SQL de ise;

{% highlight sql %}
declare @Ad nvarchar(10), @Soyad nvarchar(15)
set @Ad = ‘Simon’
set @Soyad = ‘Phoenix’
print @Ad+‘  ‘+@Soyad
{% endhighlight %}

Yukarıdaki örnekte birden fazla değişken tanımlayıp, değerlerini atadık sonrasında ise print ile ekrana yazdırdık. Şimdi makalemizi sonlandırmadan basit bi toplama işlemi yapalım konuyu daha iyi kavrayalım, buyrun;

Kısa bi algoritma ile nelere ihtiyacımız var gözden geçirelim;

  - İki sayıyı toplayabilmemiz için iki ayrı değişkene ihtiyacımız var.
  - Ayrı olarak bir değişkende bu iki sayının toplamını üzerine alması için gerekecek
  - Sonrasında print ile toplam değerini ekrana yazdıracağız

{% highlight sql %}
declare @Sayi1 int,@Sayi2 int,@Toplam int -- Bize gerekli üç değişkeni tanımladık
set @Sayi1 = 10 -- sayi1 değişkenine değer verdik
set @Sayi2 = 20 -- sayi2 değişkenine değer verdik
set @Toplam = @Sayi1 + @Sayi2 -- Sql tarafında iki değişkeni topladık ve toplam değişkeni üzerine aldık
print @Toplam -- …ve sonucu ekrana yazdırdık
{% endhighlight %}

Umarım faydalı bi makale olmuştur, bir sonraki makaleye kadar kodlamada kalın 😉
