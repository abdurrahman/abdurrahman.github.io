---
layout: post
title: C#'da değişken kavramı
date: 2010-03-14 23:42:51 +02:00
abstract: C# programlama dilinde başlangıç olarak ilk konumuz değişken kavramı olacaktır. Değişkenler ihtiyaca göre tanımlanır, örneğin sayısal bir veri üzerinden derleyiciye iş yaptıracağız, bunun için bizim bellekte bir sayı değişkeni tanımlamamız gerekecek.
---

C# programlama dilinde başlangıç olarak ilk konumuz değişken kavramı olacaktır. Değişkenler ihtiyaca göre tanımlanır, örneğin sayısal bir veri üzerinden derleyiciye iş yaptıracağız, bunun için bizim bellekte bir sayı değişkeni tanımlamamız gerekecek. Örnek olarak;

{% highlight csharp %}
int sayi = 25;
{% endhighlight %}

Yukarıda **int** tipinde "sayi" adında değeri 25 olan bir değişken tanımladık. Şimdi bunu ekrana yazdıralım;

{% highlight csharp %}
Console.WriteLine(degiskenSayi);
Console.ReadLine();
{% endhighlight %}

Burada C# da var olan **Console.WriteLine();** metodu ile verilerimizi ekrana yazdırıyoruz. Bu yüzden parantez içine ekrana yazdıracağımız değişkenimizin adını verdik. Yine **Console.ReadLine();** metodu ile de console ekranını beklettik.

Başka bir değişken tanımlama ve değer verme şekliyle devam edelim;

{% highlight csharp %}
int a;
int b;
int c;
a = 50;
b = 62;
c = 70;
Console.WriteLine(a);
Console.WriteLine(b);
Console.WriteLine(c);
Console.ReadLine();
{% endhighlight %}

Yukarıdaki örnekte de ilk başta a,b ve c değişkenlerimizi tanımladık ve bu değerler belleğin stack bölgesinde bizim için `null` yani boş olarak oluşturuldu. Değerlerini ise sonradan verdik, daha sonra da her birini ekrana ayrı ayrı yazdırdık. Şimdi de **string** bir değişken tanımlayalım;

{% highlight csharp %}
string kelime = "İstanbul-34";
Console.WriteLine(kelime);
Console.ReadLine();
{% endhighlight %}

Yukarıda bu kez "kelime" adında string tipinde bir değişken tanımlayıp, "İstanbul-34" değerimi verdim ve ekrana yazdırdım.

**bool** değişkeni bize true ya da false değeri döner, yani yaptıracağımız bir işlemde doğru mu ya da yanlış mı kontrollerini takip etmemize olanak sağlar.

{% highlight csharp %}
bool kayitSilindiMi = true;
Console.WriteLine(kayitSilindiMi);
Console.ReadLine();

double doubleDegisken = 10.2;
Console.WriteLine(doubleDegisken);
Console.ReadLine();

Single singleDegisken = 10.2F;
Console.WriteLine(singleDegisken);
Console.ReadLine();
{% endhighlight %}

**object** referans tipli bir değişkendir, sözgelimi bütün tipler object'den türemiştir, dolayısı ile object içine her türlü veriyi alır. Örneğimizi inceleyelim;

{% highlight csharp %}
object obj = "kelimem";
object obj1 = 12;
object obj2 = true;
object obj3 = 10.2;

Console.WriteLine(obj);
Console.WriteLine(obj1);
Console.WriteLine(obj2);
Console.WriteLine(obj3);
Console.ReadLine();

char charDegisken = ‘f’;
Console.WriteLine(charDegisken);
Console.ReadLine();
{% endhighlight %}
