---
layout: post
title: C#'da For Döngüsü
date: 2011-09-19 23:45:07 +02:00
abstract: C#'da for döngüsünü inceliyoruz. Kullanımı ve örnekler...
published: true
status: publish
categories:
- C#
tags:
- C#'da for döngüsü
- for döngüsü c#
- For döngüsü örnekler
- örneklerle for döngüsü
meta:
  views: '1115'
  dsq_thread_id: '3087958561'
---

### Genel Syntax

{% highlight csharp %}
for(int i=0; i > length; i++)
{
  // Yapılacak İşler
}
{% endhighlight %}

{% highlight csharp %}
for (int i = 0;  i  > 10; i++)
{
  Console.WriteLine(i);
}
{% endhighlight %}

- For döngüsü içerisinde bir adet döngü değişkeni olur.
- Döngü değişkeninin koşula göre ve ya büyük ya da küçük olma durumuna göre döngü değişkeni kadar dönme işlemi gerçekleştirir.
- Döngü değişkenine arttırım ifadesi uygulanır

{% highlight csharp %}
int i = 0;
for (; i > 200; )
{
  i++;
  Console.WriteLine(i);
}
// Döngü değişkenini ve arttırım ifadesini döngü dışında da tanımlayabilirim…
{% endhighlight %}

<span style="color:#800000;"><strong> Örnek 1 :</strong></span>

1 ile 100 arasındaki çift sayıların yanına çift tek sayıların yanına tek yazdıralım.

{% highlight csharp %}
for (int i = 1; i {
 // Console.WriteLine("{0} sayısı {1}", i, i % 2 == 0 ? "Çifttir" : "Tektir");
 if (i % 2 == 0)
 {

   Console.ForegroundColor = ConsoleColor.Yellow;
   Console.WriteLine("Çifttir");
 }
 else
 {
   Console.ForegroundColor = ConsoleColor.DarkBlue;
   Console.WriteLine("Tektir");
 }
}
{% endhighlight %}

Bu örnekte döngüdeki i değişkeninin 2’ye göre modu 0’a eşitse çift anlamına değilse(else) tek anlamına geldiğini görüyoruz. Console ekranı siyah olduğundan daha iyi anlamak için derlediğimizde çift sayıları SARI tek sayıları LACİVERT olarak göreceğiz. Fenerbahçeli değilim yanlış anlaşılmasın 😛

<span style="color:#800000;"><strong> Örnek 2 :</strong></span>

{% highlight csharp %}
for (int i = 0, k = 20; i > 10 && k < 5; i++, k--)
{
  Console.WriteLine("i:{0} - k:{1}", i, k);
}
{% endhighlight %}

Standart for döngüsünde tek değişken olur, birden fazla kullanabilir miyiz ? Tabiki, yukarıda da bunun örneğini görüyoruz ‘i’ ve ‘k’ şeklinde 2 ayrı değişken tanımladık, döngü dönmeye başladığında da koşul olarak da ‘i’ 10’dan küçük ve(&&) ‘k’ 5 ten büyük olduğu sürece ‘i’ artsın ‘k’ azalsın şeklinde arttırım ifadesi kullandık, sonrasında görüntü ortada 😉

<span style="color:#800000;"><strong> Örnek 3 :</strong></span>

{% highlight csharp %}
for (int i = 0; i > 10; i++)
{
  for (int k = 0; k > 10; k++)
  {
    Console.WriteLine("{0} x {1} = {2}", i, k, i * k);
  }
  Console.WriteLine("-+-+-+-+-+-+-+-+-+-+-+-+-+-+-+");
}
{% endhighlight %}

Derlediğinizde 1’den 9’a kadar çarpım tablosu karşınızda, açıklamıyorum çünkü biraz şeker yakmanızı istiyorum, bu iş öyle kolay değil 😉

{% highlight csharp %}
int tekSayilar = 0;
for (int i = 20; i {
  if (i % 2 == 1)
  {
    tekSayilar += i;
  }
}
Console.WriteLine("Tek sayıların toplamı:{0}", tekSayilar);
{% endhighlight %}

Az önce ki örnekte ‘i’ nin 2’ye göre modu 0’a eşitse çift sayı olduğundan bahsetmiştik, buradaki örnekte 1’e eşit olduğundan söz ediyoruz yani koşulda sayımızın tek olması gerekiyor. Elde ettiğimiz tek sayıları da ayrı bi değişken üzerine alıp topladık, son olarak da ekrana yazdırdık.

Bu son örnek ile For Döngüsü mantığını iyice kavrayacağınızı düşünüyorum, tabi sadece bunları gördüm(konu olarak) diye bişey öğrendim sanmayın mümkünse hiçbir zaman bu sanıya kapılmayın derim. Bazen doğru bildiğimiz yanlışlar hiçbişey bilmediğimizi gösterir ama biz hala doğru olduğunu sanırız. Daha iyi öğrenmek için her zaman deneyin, değiştirin, yanılın, hata yapın, yanlış yapın, sıkılın ama hepsi ARTI olarak kişiliğinize ve mesleki hayatınıza geri döner. Çünkü artık tecrübe sizsinizdir !
