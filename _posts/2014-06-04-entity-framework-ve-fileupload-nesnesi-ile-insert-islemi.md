---
layout: post
title:  Entity Framework ve FileUpload Nesnesi ile Insert İşlemi
date: 2014-06-04 00:31:00 +02:00
abstract: En iyi öğrenme yolu herkesin de tavsiye ettiği gibi uygulamalı(proje) olarak öğrenmedir düşüncesiyle hareket ederek yeni başladığım web projemde Entity Framework kullanmaya karar verdim...
published: true
status: publish
categories:
- ASP.NET
- C#
- MS SQL
tags:
- asp.net ile file upload
- c# dosya yükleme örneği
- entity framework
- entity framwork giriş
- entity model
- entity model kullanımı
- file upload
- file upload with entity framework
- fileupload nesnesi
- insert işlemi
meta:
  views: '1963'
  dsq_thread_id: '3018307521'
---

En iyi öğrenme yolu herkesin de tavsiye ettiği gibi uygulamalı(proje) olarak öğrenmedir düşüncesiyle hareket ederek yeni başladığım web projemde Entity Framework kullanmaya karar verdim. Kolaylığı görünce şaşırdığımı söylemeliyim, kesinlikle tavsiye ederim. Entity Framework’e geçipte eski yöntemi kullanan yoktur sanırım. Bir kere her seferinde connection bağlantısı yazmıyorsunuz gerçi bunun Entity Framework olmadan da kolaylığı var web.config dosyasına yazmak gibi aslında Entity Framework bunu sizin yerinize zaten yapıyor. O zaman şöyle diyelim her seferinde connection açıp kapatmakla uğraşmıyorsunuz.

Giriş paragraflarında pek başarılı olamıyorum, bunu kabullenmeliyim. Direk anlatıma geçmek istiyorum ne demek istediğimi daha iyi anlayacaksınız.

Ben SQL’de kendi tablomu oluşturdum, Entity Framwork’de boş model oluşturarak hiç SQL açmadan da veri tabanı oluşturup tablolar ekleyebiliyorsunuz daha sonra onunla ilgili de bir yazı eklerim.

{% highlight sql %}
create table Brands
(
  BrandsId int identity,
  BrandsName nvarchar(50),
  BrandsImgFilePath nvarchar(max)
)
{% endhighlight %}

Markalar adında bir tablomuz olsun, markanın adını ve resim yolunu kaydedelim. Bu yazıda daha önce amatörce kullandığım dosya yükleme işlemini de pekiştirmeyi amaçladım. Resimleri binary olarak veritabanına kaydedip çekmek ile klasörde tutup dosya yolunu kaydetmek arasında kaldım. Binary tutmanın büyük çaplı projeler için sağlıklı olmadığı kanaatine varınca seçimimi yaptım.

Şimdi Visual Studio tarafına geçebiliriz, bir web projesi başlatalım, ben projemde Markalar.aspx şeklinde sayfa oluşturmuştum, siz default.aspx üzerinde çalışabilirsiniz.

{% highlight html %}
<label>Marka Adı:</label>
<asp:TextBox ID="txtBrandName" runat="server"></asp:TextBox><br /><br />
<label>Marka Logosu:</label>
<asp:FileUpload ID="fuBrandImg" runat="server" /><br /><br />

<asp:Button ID="btnMarkaEkle" runat="server" Text="Ekle!"
    ToolTip="Yeni marka ekleme.." onclick="btnMarkaEkle_Click"/><br />
    <asp:Label ID="lblMarkaDurum" runat="server"></asp:Label>
{% endhighlight %}

Yukarıdaki gibi form alanlarını ekleyelim ve Solution Explorer bölümünde projemiz üzerinde sağ tuş > add > new item diyelim. Gelen diyalog ekranında sol bölümde Data’yı seçerek projemize ADO.NET Entity Data Model ekleyelim.

<img alt="entity model nedir" src="{{ site.baseurl }}/assets/entitymodel.jpg" width="632" height="558" />

Burada model içeriğimizi seçiyoruz, bizim veri tabanımız olduğundan Generate from database’i seçiyoruz.

<img alt="entity model giriş" src="{{ site.baseurl }}/assets/entitymodel2.jpg" width="632" height="558" />

Bu ekranda New Connection butonuna tıklıyoruz, veri tabanı bağlantı ayarlarımızı yapmamız gerekiyor;

<img alt="entity model uykusuzadam" src="{{ site.baseurl }}/assets/entitymodel3.jpg" width="451" height="663" />

Gelen ekranda hangi veri tabanımız ile çalışacağımızı seçiyoruz, Test Connection ile bağlantınızı test edebilirsiniz. Bir önceki ekrana geri geldik, burada Entity Model’in web.config dosyasına kaydetmek istediği ismi belirleyebilirsiniz daha sonra projemizde bu ismi çağırarak nesneler oluşturacağız.

<img alt="entitymodel4" src="{{ site.baseurl }}/assets/entitymodel4.jpg" width="627" height="558" />

Bu ekranda çalışacağımız tablolarımızı, varsa view ve store procedure’lerimizi seçebiliyoruz, ben sadece markalar tablosu ile çalışacağım için onu seçtim. Tablolarımızı da belirledikten sonra Finish diyerek projemize entity modelimizi ekleyelim. Karşımıza daha öncede aşina olduğumuz diagram görünümü gelecektir.

Proje başlangıcında default.aspx’e eklemiş olduğumuz alanların back-end bölümüne geçmek için default.aspx’de sayfadayken gönder butonumuza double-click yaparak butonumuzun kliğine gidelim.

{% highlight csharp %}
mertdbEntities entity = new mertdbEntities();
Brands marka = new Brands();
marka.BrandsName = txtBrandName.Text;
if (fuBrandImg.HasFile)
{
  fuBrandImg.SaveAs(Server.MapPath("/images/brands/" + fuBrandImg.FileName));
  lblMarkaDurum.Text = "Dosya başarıyla yüklendi!";
}
else
{
  lblMarkaDurum.Text = "Üzgünüz, yükleme başarısız";
}
marka.BrandsImgFilePath = "/images/brands/" + fuBrandImg.FileName;
entity.AddToBrands(marka);
entity.SaveChanges();
{% endhighlight %}

Veri tabanına insert yapacak kodumuz yukarıdaki şekilde, manuel yazdığımızdan farkı burada sınıf mantığında çalışması, ne kadar rahat ve keyifli olduğunu umarım anlatabilmişimdir. Klasör yolu sizde farklı olabilir ben projemde bu şekilde oluşturmuştum.

entity-model-uykusuzadam
<img alt="entity-model-uykusuzadam" src="{{ site.baseurl }}/assets/entity-model-uykusuzadam-1024x415.jpg" width="635" height="257" />

Yazımı bitirmeden önce benim de yaşadığım yukarıdaki ekran görüntüsündeki gibi bir hata alacaksınız, bunun çok basit bir çözümü var; veri tabanından tabloya primary key eklemeniz yeterli olacaktır. Yalnız ekledikten sonra projenizde entity modelinize çift tıklayıp diagram görünümüne geldiğinizde model1.edmx üzerinde sağ tuş > Update Model from Database demeli ve gelen ekranda refresh bölümünde tablonuzu seçip Finish demelisiniz aksi hâlde tahmin ettiğiniz gibi güncellemediğiniz için primary key algılanmayacaktır.

<img alt="entity-update-uykusuzadam" src="{{ site.baseurl }}/assets/entity-update-uykusuzadam.jpg" width="623" height="554" />

Keyifli kodlamalar
Abdurrahman
