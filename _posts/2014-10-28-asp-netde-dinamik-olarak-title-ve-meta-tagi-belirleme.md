---
layout: post
title: Asp.Net'de Dinamik Olarak Title ve Meta Tagi Belirleme
date: 2014-10-28 23:40:02 +02:00
abstract: Asp.Net ile proje geliştirmeye başlayanlar için bir süre sonra kullanıma ihtiyaç duyacağınız "dinamik olarak sayfamıza ya da sayfalarımıza meta tagi nasil ekleriz?" sorusunu cevaplıyor olacağız. Proje geliştirirken senaryomuz şöyle olabilir; kurumsal bir web sitesi ...
---
Asp.Net ile proje geliştirmeye başlayanlar için bir süre sonra kullanıma ihtiyaç duyacağınız "dinamik olarak sayfamıza ya da sayfalarımıza meta tagi nasil ekleriz?" sorusunu cevaplıyor olacağız. Proje geliştirirken senaryomuz şöyle olabilir; kurumsal bir web sitesi tasarlıyoruz ve anasayfa ve alt sayfaların meta taglari birbirlerinde bağımsız olsun diyebiliriz. Ya da bir admin panelimiz var ve kullanıcının admin panelinden istediği zaman meta taglarini guncelleyebilmesini isteyebiliriz. Bu işlemin .Net 3.5 ve .Net 4.0 ile kullanımı için buyrun örneklerimizi inceleyelim.

.Net Framework 3.5 için **CodeBehind** tarafta **HtmlMeta** sınıfından yararlanıyoruz. Örnek kullanım aşağıdaki gibidir.
{% highlight csharp %}
  Page.Header.Title = "Sayfanin Title Bilgisi Buraya Girilecek";

  HtmlMeta description = new HtmlMeta();
  description.Name = "description";
  description.Content = "Sayfanın Description Bilgisi Buraya Girilecek";
  Page.Header.Controls.Add(description);

  HtmlMeta keyword = new HtmlMeta();
  keyword.Name = "keyword";
  keyword.Content = "Sayfanın Keyword Bilgileri Buraya Girilecek";
  Page.Header.Controls.Add(keyword);
{% endhighlight %}
Yukarıda 3.5 versiyonuna göre tanımlamayı gördük. HtmlMeta sınıfı ile html tarafta nasıl **<head>** tagi arasına manuel yazıyorsak burada da sınıfı tanıtıp örnegin **author** belirlemek için her meta tagi için bu sınıftan yararlanabiliyoruz. Page.Header.Controls.Add(**meta_name**) metodu ile ilgili sayfanın head bölümüne meta tagimizi eklemiş oluyoruz.

.Net 4.0'da kullanımı daha da basitleştirildi,
{% highlight csharp %}
  Page.Title = "Sayfanin Title Bilgisi";
  Page.MetaDescription = "Sayfanin Tanim Bilgisi";
  Page.MetaKeywords = "Sayfanin Anahtar Kelimeler Bilgisi";
{% endhighlight %}
3.5 versiyonundaki gibi her seferinde HtmlMeta sınıfı tanımlama zahmetinden kurtulmuş oluyoruz. Oldukça basit kullanımı ile dinamik olarak Title, Description ve Keyword tanımlamayı inceledik. Umarım yardımcı olabilmişimdir.
