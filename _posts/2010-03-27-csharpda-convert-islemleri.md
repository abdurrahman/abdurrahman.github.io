---
layout: post
title: C#'da Convert(Dönüştürme) İşlemleri
date: 2010-03-27 22:13:59 +02:00
abstract: Biz değeri olan bir değişkenimizi başka bir tipe convert (çevirmek) etmek isteyebiliriz. Örneğin int tipinde tanımladığımız sayı değişkenimizi ihtiyacımıza göre string tipine convert edebiliriz...
published: true
status: publish
categories:
- C#
tags:
- C#'da convert işlemleri
- Cast işlemleri
- Parse Metodu
meta:
  views: '824'
  dsq_thread_id: '3280958204'
  dsq_needs_sync: '1'
---

Biz değeri olan bir değişkenimizi başka bir tipe **convert** (çevirmek) etmek isteyebiliriz. Örneğin int tipinde tanımladığımız sayı değişkenimizi ihtiyacımıza göre string tipine convert edebiliriz.

{% highlight csharp %}
byte b = 25;
int a = Convert.ToInt32(b);
Console.WriteLine("Sayınız :{0}", a);
{% endhighlight %}

Yukarıda byte olarak tanımladığım b değişkenimi int tipinde a değişkeni üzerine alıp byte değeri int‘e çevirdim.

Aşağıdaki örnekte de yalnızca iki değer alabilen bool değişkenimi int‘e çevirip tabi ki bool‘un true olması durumunda ekrana 1 yazdırdım. False olsaydı 0 yazdıracaktım.

{% highlight csharp %}
bool boolDegisken = true;
int a = Convert.ToInt32(boolDegisken);
Console.WriteLine(a);
{% endhighlight %}

Güzel bir örnekle öğrendiğimizi daha iyi kavrayalım;

<ins>Amaç :</ins> Dışardan değer alalım, bu değer string üzerine alsın, sonrasında int tipine çevirelim;

{% highlight csharp %}
string kelimem;
int sayi = 0;
Console.Write("Bir sayı giriniz :"); // Kullanıcıda sayı girmesini istiyoruz.
kelimem = Console.ReadLine(); // Kullanıcı girmiş olduğu sayı değerini önceden tanımladığımız string değişkeni üzerine alıyoruz.
sayi = Convert.ToInt32(kelimem); // Daha sonra string değişkenimizi integer'a convert ediyoruz.
Console.Write("Girdiğiniz sayı :{0}", sayi);
Console.ReadLine();
{% endhighlight %}

Convert işlemiyle güzel bir örnek daha… :)

{% highlight csharp %}
Console.Write("Lütfen yaşınızı giriniz :");
Console.WriteLine("Dünyadaki gün sayınız : {0}", 365 * Convert.ToInt32(Console.ReadLine()));
{% endhighlight %}

Kullanıcıdan aldığımız yaş değerini Convert.ToInt32() metodunun içine aldık ve sonucu place holder ile ekrana yazdırdık…
