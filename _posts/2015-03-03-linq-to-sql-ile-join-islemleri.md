---
layout: post
title: Linq-to-SQL ile Join İşlemleri
date: 2015-03-03 02:07:03 +02:00
abstract: Linq to Sql işlemlerinde Join kullanımına değineceğiz, örnek olarak 3 tabloyu birleştirip DataGridView nesnemiz üzerinde nasıl göründüğüne bakacağız. Linq to Sql'de iki farklı şekilde sorgu yazabiliyoruz...
published: true
status: publish
categories:
- C#
tags:
- Join methodu
- Left join kullanımı
- Linq ile Join işlemleri
- Linq ile Join kullanımı
- Linq to Sql
- Linq to sql join kullanımı
- linq to sql left join kullanımı
meta:
  views: '2071'
  dsq_thread_id: '3562091533'
---

Linq to Sql işlemlerinde Join kullanımına değineceğiz, örnek olarak 3 tabloyu birleştirip DataGridView nesnemiz üzerinde nasıl göründüğüne bakacağız. Linq to Sql'de iki farklı şekilde sorgu yazabiliyoruz.

### 1. Yöntem
{% highlight csharp %}
int gelirId = 1;
var query = from c in db.Gelir
            where c.Id == gelirId
            select c;
{% endhighlight %}

### 2. Yöntem
{% highlight csharp %}
int gelirId = 1;
var query = db.Gelir.Where(x => x.Id == gelirId).Select(x => x.Id);
// Bir diger yazim sekli
var query = db.Gelir
            .Where(x => x.Id == gelirId)
            .Select(x => x.Id);
{% endhighlight %}
Şimdi gerçek değerler ile join kullanımında nasıl sonuçlar alacağımıza bakalım.
<!--more-->
{% highlight csharp %}
using (TestEntities db = new TestEntities())
{
     var datasource = from g in db.Gelir
                      join gk in db.GelirKategorisi
                      on g.GelirKategoriId equals gk.KategoriId
                      join gak in db.GelirAltKategorisi
                      on g.GelirAltKategoriId equals gak.AltKategoriId
                      orderby c.Tarih ascending
                      select new
                      {
                           Id = c.Id,
                           Tarih = c.Tarih.Value,
                           Aciklama = c.Aciklama,
                           KategoriAdi = k.KategoriAdi,
                           AltKategoriAdi = j.AltKategoriAdi,
                           Fiyat = c.Fiyat.Value
                      };
     dataGridView1.DataSource = datasource;
}
{% endhighlight %}
Burada kullandığımız örnekte **Gelir(g), GelirKategorisi(gk)** ve **GelirAltKategorisi(gak)** tablolarını birleştirdik. Sql de yazdığımız sorgudan biraz farklı olarak tabloları birbirine bağladığımız Id kolonlarını eşittir (=) yerine kelime anlamıda eşittir olan **equals** deyimini kullanıyoruz. Tablomuzu birleştirdikten sonra orderby deyimi ile tarihe göre artan sıralama yaptık azalan için bildiğiniz gibi **orderby c.Tarih descending** yazılabilir. Daha sonrasında **select new** diyerek hangi alanları grid üzerinde göstereceğimizi seçtirerek datagrid nesnemiz üzerine alıyoruz.

Tabi her kaydın bir alt kategorisi olmayabilir, bu durum ele alındığında yukarıdaki sorguya göre alt kategorisi null olan kayıt datagrid nesnemizde listelenmeyecektir. Alt kategorisi **Null** olan kayıtlarımızında listelenmesini istersek SQL'den bildiğimiz **Left Join** bize bu konuda yardımcı olacaktır. Dilerseniz hemen nasıl kullanıldığına bakalım.
{% highlight csharp %}
using (TestEntities db = new TestEntities())
{
     var datasource = from g in db.Gelir
                      join gk in db.GelirKategorisi
                      on g.GelirKategoriId equals gk.KategoriId
                      join gak in db.GelirAltKategorisi
                      on g.GelirAltKategoriId equals gak.AltKategoriId into sub
                      from o in sub.DefaultIfEmpty()
                      orderby c.Tarih ascending
                      select new
                      {
                           Id = c.Id,
                           Tarih = c.Tarih.Value,
                           Aciklama = c.Aciklama,
                           KategoriAdi = k.KategoriAdi
                           // Gelir tablosundaki AltKategoriId null geliyorsa hücrede Yok yazmasını sağlıyorum.
                           AltKategoriAdi = o.AltKategoriId == null ? "Yok" : o.AltKategoriAdi,
                           Fiyat = c.Fiyat.Value
                      };
     dataGridView1.DataSource = datasource;
}
{% endhighlight %}
Yine aynı şekilde join işlemimizi yaptık fakat bir öncekinden farklı olarak alt kategori işlemini bağladığımız bölümden sonra **into** deyimi ile yeni bir isim tanımlayarak **DefaultIfEmpty()** methodu ile null olan kayıtları da getirmesini istedik. Normalde SQL'de Left Join, Right Join gibi veri çekme çeşitlilikleri var fakat linq to sql'de bu çeşitlilik söz konusu olmadığı gibi DefaulIfEmpty() methodu ile **select new** bölümünde gördüğünüz üzere **Ternary If** kullanarak null gelen kayıtlara alt kategorisi olmadığı için **Yok** yazdırdık.

Linq to Sql'in sorgu şeklinde yazımını kullandık. Bir de method halinde yazımını inceleyelim. Biraz karmaşık gelebilir ama buradaki örneğe göre adım adım Visual Studio üzerinde yazarsanız **intelisense**'in de yardımıyla gayet iyi anlayabileceğinizi düşünüyorum.
{% highlight csharp %}
using (TestEntities db = new TestEntities())
{
     var newQuery = db.FaturaAlis
                    .Join(db.FaturaAlisKategorisi,
                        fa => fa.FaturaAlisKategoriId,
                        fak => fak.KategoriId, (fa, fak) => new { fa, fak })
                    .GroupJoin(db.FaturaAlisAltKategorisi,
                        fa => fa.fa.FaturaAlisAltKategoriId,
                        faak => faak.AltKategoriId, (fa, faak) => new { fa, faak })
                    .SelectMany(sub => sub.faak.DefaultIfEmpty(),
                        (x, y) => new
                        {
                            Id = x.fa.fa.Id,
                            Tarih = x.fa.fa.Tarih.Value,
                            Aciklama = x.fa.fa.Aciklama,
                            KategoriAdi = x.fa.fak.KategoriAdi,
                            AltKategoriAdi = y.AltKategoriId == null ? "Yok" : y.AltKategoriAdi,
                            Fiyat = x.fa.fa.Fiyat.Value
                        })
                    .OrderBy(x => x.Tarih);
     dataGridView1.DataSource = newQuery;
}
{% endhighlight %}
<img class="alignnone size-full wp-image-719" src="{{ site.baseurl }}/assets/JoinMethodu-uykusuzadam.jpg" alt="JoinMethodu-uykusuzadam" width="870" height="225" />
Join methodumuzu tanımlarken parantezi açtığımızda tooltip'teki açıklamada bizden istenen ilk değer **inner** olarak bağlayacağımız tablonun adı, daha sonra **outerkey** ve **innerkey** olarak id parametrelerimizi ister. new ile yeniden tanımlama yapılır. Group Join ile 3. tablomuzu da belirttikten sonra SelectMany methodu ile yaptığımız tanımlamaları aynı **select new** belirtirken yaptığımız gibi göstereceğimiz alanları belirtiyoruz. Alanlarımıza yaptığımız tanımlamaya göre merdiven gibi düşünürsek her seferinde  bir üstteki bağlama giderek tablomuzdaki alanlara ulaşabiliyoruz. Açıklama yetersiz gelebilir bu yüzden uygulama halinde örnek yapmanızı şiddetle tavsiye ediyorum.
<img class="alignnone size-full wp-image-717" src="{{ site.baseurl }}/assets/JoinMethodu2-uykusuzadam.jpg" alt="JoinMethodu2-uykusuzadam" width="572" height="186" />
<blockquote>3. tablomuzu **Group Join** ile bağlamamızın sebebi **SelectMany** sırasında **DefaultIfEmpty()** methodunun Join methodunda çalışmaması, bunu kendi bilgi eksikliğimden yazıyorum. Eminim vardır bir yöntemi henüz araştırma fırsatım olmadı.</blockquote>

**Lambda** operatörü ile yaptığımız tanımlamalar sonrasında select işlemimizi de tamamladık ve OrderBy methodumuz ile belirlediğimiz alana göre ascending sıralamımızı gerçekleştirdik. Tam tersi için OrderByDescending methodunu kullanabilirsiniz. Sadece OrderBy methodu kullanılırsa ascending(artan) sıralama yapacaktır.
<img class="alignnone size-full wp-image-718" src="{{ site.baseurl }}/assets/JoinMethodu3-uykusuzadam.jpg" alt="JoinMethodu3-uykusuzadam" width="492" height="202" />
Umarım faydalı bir yazı olmuştur. Yanlış ya da eksik bilgiye mahal vermiş olabilirim ve varsa sorularınız yorumda belirtirseniz sevinirim.
