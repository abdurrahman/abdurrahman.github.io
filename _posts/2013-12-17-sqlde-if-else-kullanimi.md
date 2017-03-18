---
layout: post
title:  SQL'de If Else Kullanımı
date: 2013-12-17 01:51:21 +02:00
abstract: Merhabalar, bu makalemizde SQL tarafında If-Else koşul ifadelerini anlatmaya çalışacağım. If-Else koşul ifadelerimiz çoğu yazılım dilinde kullanıldığı gibi burada da aynı mantıkta çalışır...
published: true
status: publish
categories:
- MS SQL
tags:
- Sql'de if else kullanımı
meta:
  views: '21346'
  dsq_thread_id: '2999049041'
---

Merhabalar, bu makalemizde SQL tarafında If-Else koşul ifadelerini anlatmaya çalışacağım. If-Else koşul ifadelerimiz çoğu yazılım dilinde kullanıldığı gibi burada da aynı mantıkta çalışır.

### Genel Syntax

{% highlight sql %}
IF(koşul)
  BEGIN
    --Eğer koşulumuz doğru ise bu alandaki ifademiz çalışır.
  END
ELSE
  BEGIN
    --Eğer koşulumuz doğru değilse bu alandaki ifademiz çalışır.
  END
{% endhighlight %}

AdventureWorks veritabanımızı baz alarak bir örnek yapalım. Çalışanların bilgilerinin tutulduğu Person.Contact tablosuna gidelim, isminin içinden ‘B’ harfi geçen personelin toplamını alıp bunu da bir değişkenin üzerine alalım, sonrasında şöyle bir koşul belirleyelim; eğer (if) gelen değer (yani değişkenin üzerindeki değer) 1500’den büyükse ‘Fazla Kayıt Var!’, 1500’den küçük ise (else if) ‘Az Kayıt Var!’, eğer hiçbir değilse (else) ‘Hiç Kayıt Yok!’ şeklinde sorgumuzu sonuçlandıralım.


{% highlight sql %}
declare @Toplam int
--1- Bu bir kural değil ama standart olan ilk önce değişkeni tanımlamaktır.
select @Toplam = count( * ) from Person.Contact where FirstName like '%b%'
--2- Burada tablomuzdaki FirstName kolonunda içinde b harfi olan kayıtların toplamını değişkene aktarıyoruz.

if(@Toplam > 2000)
  begin
    print 'Fazla Kayıt Var!'
  end
else if(@Toplam < 2000)
  begin
    print 'Az Kayıt Var!'
  end
else
  begin
    print 'Az Kayıt Var!'
  end
{% endhighlight %}

Dipnot: Yukarıdaki TSql sorgumuzun çalışması için tamamını seçmemiz gerekmektedir. Bunun nedeni yaptırmak istediğimiz işlevi sadece bi bölümden oluşuyor, sayfalarca kod da yazabilirsiniz ama Execute(F5) ettiğinizde bütün sayfayı dikkate alıp sonuç getirmeye çalışır. Doğrusu getiremez hata alırsınız. Misal tablo oluşturdunuz, sonra bir tablo daha yazdınız, sonrasında bir store procedure eklediniz diyelim, bunları sırasıyla ayrı ayrı seçip execute etmeniz gerekmektedir ki Sql’de yaptırmanız gerekeni blok blok algılayıp sorunsuz çalıştırsın. Gerçi store procedure bilen kişiye buradaki anlatım saçma gelebilir “adam store procedure yazabiliyorsa bu bilgiyi çoktan aşmıştır..” gibi, tabi yazı yeni başlayanlara hitap ettiği için ancak store procedure’de neyin nesi sorularının ortaya çıkmasına mahal verir. Fazla uzattım, az önceki örneğimizi daha kısa nasıl yazabiliriz sorusuyla devam edelim.

{% highlight sql %}
IF((SELECT COUNT( * ) FROM Person.Contact WHERE FirstName LIKE '%b%') > 2000)
begin
  print '2000'den Fazla Kayıt Var!'
end
{% endhighlight %}

Yukarıdaki şekilde bu sefer değişken kullanmadık direk if ifadesi içinde koşulumuzu yazdık, parantezlere dikkatinizi vermenizi istiyorum çünkü kapsam alanları önemli..

Bir örnek daha yapalım; bu kez de Production.Product (Ürün) tablomuzdan rengi null (boş,sıfır) olmayan fiyatı sıfırdan büyük olan ürünlerin adetini alalım. Bunu da bir if içerisinde kontrol edelim. 100’den büyükse ve küçükse ifadeler yazdıralım.

{% highlight sql %}
IF((SELECT COUNT( * ) FROM Production.Product WHERE ListPrice > 0 AND Color IS NOT NULL) > 100)
BEGIN
  print 'Çok fazla kayıt var.'
END
ELSE
BEGIN
  print 'Çok az kayıt var.'
END
{% endhighlight %}

Burada fiyatı sıfırdan büyük olup rengi boş olmayanların 100 den fazla ise ‘Çok fazla kayıt var.’ değilse ‘Çok az kayıt var.’ şeklinde sonuçlarını ekrana yazdırdık. Bu makaleyle If-Else koşul ifadelerini sade bir dille anlatmaya çalıştım. Umarım faydalı olabilmişimdir. Sorularınız varsa bana iletişim bölümünden ulaşabilirsiniz. Bir sonraki makale için takipte kalmanız rica olunur o vakte kadar esen kalın!
