---
layout: post
title: Switch-Case Mekanizması
date: 2011-07-25 22:55:50 +02:00
abstract: Switch-case mekanizması if else döngüsünün aynısıdır, buradaki tek fark şudur; if döngüsünde kod bloğu çalıştığında program her if bloğuna uğrayacaktır, Switch-Case mekanizmasında...
published: true
status: publish
categories:
- C#
tags:
- Switch-Case Mekanizması
meta:
  views: '736'
  dsq_thread_id: '3459589804'
---

Switch-case mekanizması yapı olarak if-else koşul ifadelerinin aynısıdır, buradaki tek fark şudur; if döngüsünde kod bloğu çalıştığında program her if bloğuna uğrayacaktır, Switch-Case mekanizmasında ise koşul parantezindeki şarta bağlı olarak ilgili case: bloğuna girip işlemi bitirecektir.

Switch-Case mekanizmasının ELSE bloğu default bloğudur. Ayrıca her case:’den sonra mutlaka break olmalıdır.

{% highlight csharp %}
switch(koşul)
{
  case:
    break;

  default:
    break;
}
{% endhighlight %}

Az önce switch mekanizmasının çalışma mantığını öğrendik. Basit bir hesap makinası ile bunu daha iyi anlayalım;

<span style="color:#800000;"><strong> Örnek 1 :</strong></span>
{% highlight csharp %}
Console.Write("1.sayıyı giriniz : ");
int sayi1 = int.Parse(Console.ReadLine());
Console.Write("2.sayıyı giriniz : ");
int sayi2 = int.Parse(Console.ReadLine());
Console.Write("Lütfen işlem seçiniz(+,-,/,* ) :"); // Toplama, Çıkarma, Bölme ve Çarpmayı seçenekte sembol olarak gösterdik.
char secim = char.Parse(Console.ReadLine()); // Tabi bu semboller karakter sınıfında olduğu için char değişkenin üzerine aldık.

switch (secim)
        Console.WriteLine("{0} + {1} = {2}", sayi1, sayi2, sayi1 + sayi2);
    break;
    case '-':
        Console.WriteLine("{0} - {1} = {2}", sayi1, sayi2, sayi1 - sayi2);
    break;
    case '\*':
        Console.WriteLine("{0} * {1} = {2}", sayi1, sayi2, sayi1 * sayi2);
    break;
    case '/':
        switch (sayi2)
        {
            case 0:
                Console.WriteLine("0 bölen olamaz");
            break;
            default:
                Console.WriteLine("{0} / {1} = {2}", sayi1, sayi2, sayi1 / sayi2);
            break;
        }
    break;
    default:
        Console.WriteLine("Hatalı seçim yaptınız.
Bi kaç dakikalığına yazılımcı gömleğinizi çıkarın, cafede çalışan ve sipariş alan bir eleman olarak görün kendinizi;..!");
    break;
}

{% endhighlight %}

<span style="color:#800000;"><strong> Örnek 2 :</strong></span>
{% highlight csharp %}

Console.WriteLine("[1]-Çay");
Console.WriteLine("[2]-Kahve");
Console.WriteLine("[3]-Porakal Suyu");
Console.Write("Lütfen seçiminizi yapınız..:");

char secim = char.Parse(Console.ReadLine());
switch (secim)
{
  case '1':
    Console.WriteLine("Çayınız hazırlanıyor, 2 TL alayım.");
    break;
  case '2':
    Console.WriteLine("Kahveniz Brezilyadan geliyor, bu arada ben 4 TL alayım.");
    break;
  case '3':
    Console.WriteLine("Portakallarınız sıkılıyor 7 TL alayım.");
    break;
  default:
    Console.WriteLine("Makina soğuk...!");
    break;
}

{% endhighlight %}

Yukarıda ki örnekte kullanıcı konsolda 1,2 ve 3’ü seçtiğinde switch seçimden gelen nesneye göre case deki seçenekler listelenecek, kullanıcı alakası olmayan bir istekte bulunduğunda “Makina Soğuk” yazısıyla karşılacak 😀

Anlaşıldığı gibi switch-case'in çok basit yapısı var. Daha iyi bir makalede buluşmak üzere 😉
