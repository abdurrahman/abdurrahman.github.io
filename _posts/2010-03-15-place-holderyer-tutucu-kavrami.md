---
layout: post
title: Place holder(yer tutucu) kavramı
date: 2010-03-15 01:47:00 +02:00
abstract: Place holder nam-ı diğer yer tutucu, ayrı string değerleri sıralı bir şekilde formatlarken birleştirmemize yarayan bir yöntem.
---

Place holder nam-ı diğer yer tutucu, ayrı string değerleri sıralı bir şekilde formatlarken birleştirmemize yarayan bir yöntem. Kullanım şekli aşağıdaki gibidir.

{% highlight csharp %}
Console.WriteLine(“{index numarası} kelime {index numarası}”, değişkenadi1, değişkenadi2);
{% endhighlight %}

Not: index numarası 0’dan başlar ve bu 0, virgülden sonraki ilk değişkeni yazdırır. Yani virgulden sonra ilk gelen 0. indexte yazdırılır sonrası 1,2,3… diye gider buna göre parantez içerisinde index verirken virgül sıralamasına dikkat etmeliyiz.

Daha iyi kavrayabilmek için hemen bir örnekle pekiştirelim.

{% highlight csharp %}
string kelimeDegisken = "Hayat";
string kelimeDegisken2 = "Merhaba";
// place holder : 0. index te virgülden hemen sonraki değişken adı "Hayat", 1. index te ise "Merhaba" çağrıldı...
// index numaraları her zaman 0 dan başlar...
Console.WriteLine("{1} {0}", kelimeDegisken, kelimeDegisken2);
Console.ReadLine();
{% endhighlight %}

Ekranda ” Merhaba Hayat ” yazdırdık.. index sıralamasını değiştirip ” Hayat Merhaba ” da yazdırabilirdik.

{% highlight csharp %}
//Asla Asla Vazgeçmem Senden Asla şarkı sözünü place holder ile ekrana yazdıralım...
string kelimem = "Asla";
Console.WriteLine("{0} {0} Vazgeçemem Senden {0}", kelimem);
{% endhighlight %}

Yukarıda string değerimize üç adet index verdim, olması gerektiği gibi tek bir değişken değerini index numarasına göre istediğimiz sayıda kullanabiliyoruz.

Bir diğer kullanım yöntemi olarak string.Format methodu içerisine de yer tutucu değerimizi yazıp, kullanabiliriz.

{% highlight csharp %}
int ogrenciNo = 1503;
string ogrenciAd = "Mikael";
string ogrenciSoyad = "Akerfelt";

string ogrenciBilgileri = string.Format("Öğrenci No: {0} - Ad: {1} - Soyad: {2}", ogrenciNo, ogrenciAd,ogrenciSoyad);

// Formatlanmış ogrenciBilgileri değişkenimizi ekrana yazdırdık.
Console.WriteLine(ogrenciBilgileri);
{% endhighlight %}
