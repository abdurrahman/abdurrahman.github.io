---
layout: post
title: SQL'de Tablolar Arası Veri Taşımak
date: 2014-01-26 02:49:02 +02:00
abstract: Database içindeki bir tablomuzu yine aynı database içerisinde başka bir tabloya taşıyabildiğimiz gibi databaseler arası da bu işlemi yapabiliyoruz...
published: true
status: publish
categories:
- MS SQL
tags:
- sql veri taşıma
- tablolar arası veri taşıma
meta:
  views: '2245'
  dsq_thread_id: '3033458655'
---

Database içindeki bir tablomuzu yine aynı database içerisinde başka bir tabloya taşıyabildiğimiz gibi databaseler arası da bu işlemi yapabiliyoruz.

Her zaman yaptığım gibi örnek üzerinden gitmek istiyorum. Tabi AdventureWorks veri tabanından yararlanacağız. Örneğimizde AdventureWorks database’inde Person.Contact tablomuzdan FirstName, LastName, EmailAddress alanlarını yeni oluşturduğumuz tablomuza taşıyalım.

Öncelikle AdventureWorks database’inden çekeceğimiz alanları aktaracağımız tabloyu (ben örnek olduğu için öncesinde database’de oluşturdum) oluşturalım.

{% highlight sql %}
CREATE DATABASE MyDatabaseUrun -- Örnek database
USE MydatabaseUrun

CREATE TABLE Personel
(
  PersonelId INT IDENTITY(1,1),
  PersonelAdi nvarchar(50),
  PersonelSoyad nvarchar(50),
  PersonelEmailAdres nvarchar(100)
)
{% endhighlight %}
Veriyi insert edeceğimiz tablomuzu oluşturduk.

{% highlight sql %}
SELECT Firstname,Lastname,EmailAddress FROM AdventureWorks.Person.Contact
{% endhighlight %}

Yukarıdaki sorguyu çalıştırdığımızda AdventureWorks database’indeki Person.Contact tablosunda gelmesini istediğimiz alanları seçip görüntüledik buraya dikkatinizi çekmek istiyorum. Farklı databasede olan veriyi üzerinde çalıştığımız database kullanımdayken çağırabilmiş olduk. Çok basit, sadece yapmanız gereken çağrılmak istenen tablonun başına database adını yazmak. Şayet database adını yazmazsak çağırdığımız tabloyu bulunduğumuz database üzerinde arayacak tabi olmadığı için bizi hata ile karşılaştıracak.

Şimdi tablolar arası veri aktarımını yapalım;

{% highlight sql %}
INSERT INTO Personel SELECT Firstname,Lastname,Emailaddress FROM AdventureWorks.Person.contact
{% endhighlight %}

<img alt="tablolar_arasi_veri_tasima" src="{{ site.baseurl }}/assets/tablolar_arasi_veri_tasima-300x122.jpg" width="300" height="122" />

Yukarıdaki gibi başarılı bir şekilde seçili alanlarımızı yeni tablomuza aktarabilmiş olduk, tekrar bir select sorgusuyla teyit edebiliriz.

{% highlight sql %}
SELECT * FROM Personel
{% endhighlight %}

<img alt="sorgu_sonuc" src="{{ site.baseurl }}/assets/sorgu_sonuc-300x185.jpg" width="300" height="185" />

Bu kadar basit miydi ? Evet 😉
