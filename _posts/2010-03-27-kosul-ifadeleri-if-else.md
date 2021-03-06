---
layout: post
title: C#'da Koşul İfadeleri (If-Else)
date: 2010-03-27 23:00:53 +02:00
abstract: Biz değeri olan bir değişkenimizi başka bir tipe convert (çevirmek) etmek isteyebiliriz. Örneğin int tipinde tanımladığımız sayı değişkenimizi ihtiyacımıza göre string tipine convert edebiliriz.
published: true
status: publish
categories:
- C#
tags:
- If Terrior Operatörü Kullanımı
- Koşul İfadeleri If-Else
meta:
  views: '665'
  dsq_needs_sync: '1'
  dsq_thread_id: '3322618363'
---

Programcılar yeni bir dil öğrenirken ya da ilk defa programcı olacaklar için konuşalım, konuyu öğrenmeye başladıklarında anlatılanlar sıkıcı gelebilir. Ta ki **if-else** konusu anlatılana kadar, çünkü işin eğlenceli tarafı daha doğrusu sevdiren tarafı bu başlangıçtır bence.

Gelelim If-Else ifadelerine, ingilizceden de anlamına göre *eğer* ve *değilse* durumlarına göre iş yaparlar. Hemen hemen her programlama dilinde vardır ve yaptıkları iş aynıdır. C# taki syntax’ını inceleyelim;

#### Genel Syntax
{% highlight csharp %}
if(true) // -- eğer doğruysa
{
  // Yapılacak işler..
}
else // -- değilse
{
  // Yapılacak işler..
}
{% endhighlight %}

- Parantezler arasında verilen şarttan dönen değer true ise **if bloğu**, dönen şart false ise **else bloğu** çalışacaktır.
- Aynı zamanda her if döngüsünde else bloğu olacak diye bir kaide yoktur.

<span style="color:#800000;"><strong> Örnek 1 :</strong></span>

{% highlight csharp %}
Console.Write("1 sayı giriniz :"); // Kullanıcıdan bir değer aldım...
int a = int.Parse(Console.ReadLine()); // Kullanıcıdan gelen değeri a değişken üzerine aldım.
Console.Write("1 sayı daha giriniz :"); // Kullanıcıdan başka bir değer daha aldım...
int b = int.Parse(Console.ReadLine()); // Kullanıcıdan gelen diğer değeri b değişken üzerine aldım.

if (a > b) // a’nın b’den büyük olma koşuluna bakıyoruz.
{
  Console.WriteLine(“a sayısı b sayısından büyüktür”);
}
else // a’nın b’den büyük olma koşulu sağlanmadıysa else bloğu çalışacak...
{
  Console.WriteLine(“a sayısı b yi geçemedi..!”);
}
{% endhighlight %}

<span style="color:#800000;"><strong> Örnek 2 :</strong></span>

{% highlight csharp %}
Console.Write(“Lütfen bir sayı giriniz :”); // Kullanıcıdan bir sayı istedik…
int sayi = int.Parse(Console.ReadLine()); // burada gelen değeri sayi değişkeni üzerine aldık

if (sayi % 2 == 0) // sayı değerinin 2’ye bölümünden kalan değerin 0’a eşit olma yani çift sayı mı olup olmadığına baktık…
  Console.WriteLine(“Girmiş olduğunuz {0} sayısı çifttir”, sayi); // if teki değeri true ise ekrana çift ifadesini yazdırdık..
else
  Console.WriteLine(“Girmiş olduğunuz {0} sayısı tektir”, sayi); // if teki koşul false ise yani sayi tek ise burasi çalışacak..
{% endhighlight %}

<span style="color:#800000;"><strong> Örnek 3 :</strong></span>

{% highlight csharp %}
Console.Write(“Lütfen bir sayı giriniz :”);
int sayi = int.Parse(Console.ReadLine());
if (sayi > 100) // gelen sayı değerinin 100’den büyük olma durumuna baktık…
  Console.WriteLine(“{0} sayısı 100 den büyüktür”, sayi);
else
  Console.WriteLine(“{0} sayısı 100 den küçüktür”, sayi);
{% endhighlight %}

<span style="color:#800000;"><strong> Örnek 4 :</strong></span>

{% highlight csharp %}
Console.Write(“Lütfen adınız ve soyadınız ?:”); // kullanıcıdan string olarak ad ve soyadını istedik…
string adsoyad = Console.ReadLine();
// Lenght girilen string karakterin adedini verir…
if (adsoyad.Length > 15) // length ifadesi ile gelen değerin karakter uzunluğunun 15’ten büyük olma durumuna baktık…
  Console.WriteLine(“Girdiğiniz değer 15 karakterden uzun”);
else
  Console.WriteLine(“Girdiğiniz değer 15 karakterden kısa”);
{% endhighlight %}

<span style="color:#800000;"><strong> Örnek 5 :</strong></span>

{% highlight csharp %}
Console.Write(“1.Notu giriniz :”);
int vize = int.Parse(Console.ReadLine());
Console.Write(“2.Notu giriniz :”);
int final = int.Parse(Console.ReadLine());
int snc = (vize + final) / 2; // vize ve final değerlerini topladık, böldük ve snc değişkeni üzerine aldık…
if (snc > 50) // snc yani ortalamanın 50’den büyük olma durumuna baktık…
  Console.WriteLine(“{0} ortalama yaparak Geçtin…”, snc);
else
  Console.WriteLine(“{0} ortalama yaparak Kaldın…”, snc);
{% endhighlight %}

## If Terrior Operatörü Kullanımı

#### Genel Syntax
(Koşul) ? ŞartDoğruiseYapılacakİşlem : ŞartYanlışiseYapılacak

<span style="color:#800000;"><strong> Örnek 6 :</strong></span>
{% highlight csharp %}
Console.Write(“Bir sayı giriniz:”);
int gelen = int.Parse(Console.ReadLine());

Console.WriteLine(“{0} sayısı {1}”, gelen, gelen % 2 == 0 ? “Çifttir” : “Tektir”); // Tek satırda sayının çift mi tek mi olduğuna baktık, bu daha güzel değil mi? :)
{% endhighlight %}

Son örneğimizi de yapalım konuyu iyi anladık sanırım, burada kavramak sorun değil mantığını bilmek yetiyor, çünkü işi yaptıran biziz, böyleyse bu çalışsın değilse çalışmasın ya da başka bir şey çalışsın, diyebiliriz.

Dışardan (kullanıcıdan) kullanıcı adı ve şifre alalım ve bu gelecek değeri aşağıda verdiğim default değerle üzerinden eşitliğine bakalım, eğer doğru ise “giriş başarılı”, değilse “hatalı giriş yaptınız…” desin, buyrun;

<span style="color:#800000;"><strong> Örnek 6 :</strong></span>

{% highlight csharp %}
// Username:"admin"
// Password:"1234"

Console.Write(“Username :”);
string username = Console.ReadLine();
Console.Write(“Password :”);
string password = Console.ReadLine();

if (username == “admin” && password == “1234”)
{
  // && ( ve ) operatörüyle gelen username‘in “admin” ve password’ın “1234” ile eşit olma durumuna baktık…
  Console.WriteLine(“Girişiniz Başarılı…”);
}
else
{
  // iki değerden ya da her ikisi de yanlış olsa bu bölüm çalışacak…
  Console.WriteLine(“Hatalı giriş yaptınız…”);
}
{% endhighlight %}
