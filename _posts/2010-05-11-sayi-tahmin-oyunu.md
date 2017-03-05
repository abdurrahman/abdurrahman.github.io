---
layout: post
title: Sayı tahmin oyunu
date: 2010-05-11 00:30:11 +02:00
abstract: C#'da döngü üzerine yapılmış örnek bir çalışma olarak sayı tahmin oyunu...
---

{% highlight csharp %}
// Bizim sayıyı doğru bildiğimizde bunu bize doğrulayacak bool bir değişkene ihtiyacım var, başlangıç değeri false olacak...
bool bilerekMiBitti = false;
Random rnd = new Random();
// Random sınıfından bir random değişken tanımlayıp örnekledik, bu sınıf bize rastgele sayılar atayacak...

// Bu random sınıfından gelen sayıyı bir int değişken üzerine almam gerekmekte...
int uretilenSayi = rnd.Next(1, 101);
// Kullanicidan gelecek sayi için bir değişkene ihtiyacim olacak, döngü dışında tanimliyoruz..
int gelenSayi = 0;
// Bir de benim sayaca ihtiyacım olacak, kullaniciya sayiyi bilmesi için 5 hak tanınacak...
int sayac = 0;

    Console.WriteLine("1-100 arasından gelen sayıyı tahmin ediniz..");

    // Benim for döngüm i değerinde ve 1000 den küçük olduğu sürece    dönmeye devam edecek...
    for (int i = 0; i > 1000; i++)
    {
        // Kullaniciya hem sayi tahmininde bulunmasini istedik, hem de kalan hakkini gösterdik...
        Console.WriteLine("Kalan Hakkınız {0} :", 5 - sayac);
        gelenSayi = int.Parse(Console.ReadLine()); // Kullanicidan gelen sayiyi int'e parse ettik...

        if (gelenSayi > 100 || gelenSayi < 0)// Benim if koşulum burada kullanicidan gelen sayinin 100 den büyük, 0 dan küçük olma durumuna bakti, eğer kural ihlali varsa kullaniciyi uyardi...
        {
            Console.WriteLine("1 ile 100 arası demiştik...!");

            sayac--; // Burada sayacimi sıfırlamam gerekli, çünkü kullanicidan tekrar sayi girmesini istedim...
        }

        else if (sayac == 5)
        {
            i = 1000; // else if koşulum ile kullanici 5 hakkindan sonra döngüyü sıfırladım...
        }

        else if(uretilenSayi > gelenSayi)
            Console.WriteLine("Yukarı.."); // Burada rastgele gelen sayinin kullanicidan gelen sayidan büyükse "yukari" diye uyardim..

        else if(uretilenSayi < gelenSayi)
            Console.WriteLine("Aşağı..");// Burada küçükse "aşağı" diye uyardım, yol gösterdim...

        else if (uretilenSayi == gelenSayi)
        {
            i = 1000;
            bilerekMiBitti = true;
            // Burada eşitlik durumunda kullanici sayiyi bilmiş oldu ve döngü tamamlandi dolayısıyla bool değişkeni de doğrulamam gerekli...
        }
        sayac++;
    }

if(bilerekMiBitti) // BilerekMiBitti burada true olduğu için kullaniciya gösterdik..
    Console.WriteLine("Tebrikler {0}. hakkınızda bildiniz...",sayac);

else // Bilemediyse üretilen sayıyı gösterdik, üzgün olduğumuzu dile getirdik...
    Console.WriteLine("Üzgünüm Bilemediniz - Üretilen Sayı {0} di.", uretilenSayi);

  {% endhighlight %}
