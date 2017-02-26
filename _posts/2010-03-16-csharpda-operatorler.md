---
layout: post
title: C#'da Operatörler
date: 2010-03-16 01:32:12 +02:00
abstract: Her dilde olduğu gibi matematiksel işlemlerimizi operatörler aracılığı ile yaptırırız. Bir de C#’da bu konuya bakalım. Örnek olarak toplama operatör ile toplama işlemini inceleyelim;
---

### Aritmetik Operatörler ( + , – , * , / , % )

Her dilde olduğu gibi matematiksel işlemlerimizi operatörler aracılığı ile yaptırırız. Bir de C#’da bu konuya bakalım. Örnek olarak toplama operatör ile toplama işlemini inceleyelim;
{% highlight csharp %}
int toplam, sayi1, sayi2;
sayi1 = 20;
sayi2 = 5;
toplam = sayi1 + sayi2;

Console.WriteLine("{0}+{1} toplama işleminin sonucu = {2}", sayi1, sayi2, toplam);
{% endhighlight %}

<ins>Açıklama :</ins> İlk satırda 3 adet değeri olmayan değişken tanımladım, sonrasında sayi1 ve sayi2 değişkenlerine değerlerimi verdim, 4. satırda irdım. Çok basit değil mi! Sizlerde pekiştirebilmek için diğer işlemleri bu şekilde deneyebilirsiniz.

### Arttırma Ve Azaltma Operatörleri (--, ++)

Genelde döngülerde kullanılan arttırma ve azaltma işlemleri için anlaşılır bir şekilde eski ve yeni kullanım yöntemlerinden bahsedeceğiz;

{% highlight csharp %}
int a = 6;
a = a + 1; //eski yöntem
Console.WriteLine(a);
a++; //yeni yöntem
Console.WriteLine(a);

a = a - 1; //eksi yöntem
Console.WriteLine(a);
a--;//yeni yöntem
Console.WriteLine(a);

//a = a + 2; //eski yöntem
//Console.WriteLine(a);

a += 2; //yeni yöntem
Console.WriteLine(a);

a = a - 2; //eski yöntem
a -= 2; //yeni yöntem

a = a * 2; //eski yöntem
a * = 2; //yeni yöntem

a = a / 2; //eski yöntem
a /= 2; //yeni yöntem

{% endhighlight %}

### Eşit ( == ) & Eşit Değil ( != ) Operatörleri

İki değerin bir birine eşit olup olmadığı durumları incelemek için sonucu bool değişkeni olarak dönen operatörlerdir.

{% highlight csharp %}
int sayi1, sayi2, sayi3 = 0;
sayi1 = 25;
sayi2 = 50;
sayi3 = 25;

bool esitMi = sayi1 == sayi2;
Console.WriteLine("sayi1 değişkeni sayi2'ye eşit mi? {0}", esitMi);
// Yukarıdaki değer eşit olmadığından false döner...

bool esitDegilMi = sayi1 != sayi2;
Console.WriteLine("sayi1 değişkeni sayi2'ye eşit DEĞİL mi? {0}", esitDegilMi);
// Yukarıdaki değer ise true döner çünkü eşit olmadığını soruyoruz...
{% endhighlight %}

### Ve ( && ) &  Veya ( || ) Operatörleri

Yine kontrolü bool tarafından yapılan ve, veya operatörlerini inceleyelim;

{% highlight csharp %}
string user = "Admin";
string pass = "123456";

bool kullaniciVarMi = user == &quot;Admin&quot; &amp;amp;&amp;amp; pass == &quot;123456&quot;; // Bu iki değişkeni bool bir değişkende, ikisininde (&amp;amp;&amp;amp;) operatörü ile eşit olma durumuna bakıyorum.
Console.WriteLine(&quot;Kullanıcı adı ve parola eşleşme durumu: {0}&quot;, kullaniciVarMi); // Ekrana true değeri yazdırıyorum
{% endhighlight %}

### Büyük-Küçük Operatörleri (<,<=,>,>=)

Mantıksal ifadelerde ve koşul (if-else) ifadelerinde çok kullanacağımız operatörleri giriş olarak inceleyelim;

{% highlight csharp %}
int a = 25, b = 30;
bool sayiBuyukMu = a > b; // Burada a değişkenin b değişkeninden küçük mü olduğuna baktık..
Console.WriteLine(&quot;{0} sayısı {1} sayısından büyük mü? Sonuc:{2}&quot;, a, b, sayiBuyukMu); // Değer küçük olduğundan ekrana true yazdırdık.
{% endhighlight %}

Örnekler daha da genişletilebilir. Burada operatör kullanımlarına sadece giriş yaptık, ileriki konularda daha iyi anlayacağız.
