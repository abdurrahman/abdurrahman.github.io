---
layout: post
title: ASP.NET MVC'de ViewBag, ViewData ve TempData Ne Zaman Kullanılır?
date: 2017-01-25 23:26:56 +02:00
abstract: Bu nesnelerin arasında, MVC uygulamaları geliştirirken bu nesnelerin her birini tam olarak nasıl kullanabileceğimizi görmek için yeterince benzerlikler ve farklılıklar vardır...
published: true
status: publish
categories:
- ASP.NET
tags:
- ASP.NET MVC ViewBag
- TempData nedir
- ViewBag kullanımı
- ViewBag nedir
- ViewData nedir
- ViewData nesnesi
meta:
  views: '75'
  dsq_thread_id: '5494724068'
---

**"Ne zaman bir ViewBag/ViewData/TempData nesnesi kullanmalıyım ?"**-forumlarda, gruplarda sürekli sorulan ya da tartışılan soru. Bu nesnelerin arasında, MVC uygulamaları geliştirirken bu nesnelerin her birini tam olarak nasıl kullanabileceğimizi görmek için yeterince benzerlikler ve farklılıklar vardır.

Her üç nesnede view ve controller'da özellik(properties) olarak mevcuttur. Bir kural olarak küçük miktarda bir veriyi belirli bir yere taşımak amacıyla ViewBag, ViewData ya da TempData kullanırız(Ör: controller'dan bir veriyi view'da görüntülemek). ViewBag ve ViewData nesneleri aşağıdaki senaryolarda etkili çalışır:

* Bir tablodan verileri açılır listeye(dropdown,combobox gibi) doldurmak
* Components/Bileşenler (alışveriş sepeti gibi)
* Widget'lar (kullanıcı profil widget'ı gibi)
* Az miktarda hesaplanmış veri

TempData nesnesi bir temel senaryoda iyi çalışır:

* Güncel ve bir sonraki HTTP istekleri arasında veri taşımak

Daha büyük miktarda veri ile çalışmak, verileri raporlamak, gösterge panoları(dashboard) oluşturmak veya çok sayıda farklı veri kaynağı ile çalışmak isterseniz bir ViewModel nesnesi kullanabilirsiniz.

## ViewData & ViewBag nesneleri

### ViewData

ViewData, view'da görüntülemek üzere içerisine veri yerleştirebileceğiniz bir dictionary nesnesidir. ViewData, "key/value" söz dizimini kullanan ViewDataDictionary sınıfının bir türevidir.

### ViewBag

ViewBag nesnesi, dinamik olarak view için özellikler oluşturmanıza olanak sağlar.

ViewData ve ViewBag nesneleri controller ve view arasında ekstra veriye(data model'in dışında) erişmek için idealdir. View'lar belirli bir nesneyi kendi modelleri olarak zaten beklediğinden bu veri türü ek verilere erişir, MVC bu nesneyi hem view'ın hem de controller'ın bir özelliği olarak uygular ve bu nesnelerler erişimi ve kullanımı kolaylaştırır.

ViewBag, ViewData, ve TempData nesnelerin kullanımı ve söz dizimi aşağıdaki kod örneğinde özetlenmiştir.

**HomeController.cs**
{% highlight csharp %}
public class HomeController : Controller
{
    // ViewBag & ViewData sample
    public ActionResult Index()
    {
        var mainStore = new Store
        {
            Name = "The Cheesecake Factory Inc.",
            Address = "14 Tottenham Court Road - London, England",
            PhoneNumber = "555-555-55-55",
            StoreCode = "1001",
        };
        ViewData["MainStore"] = mainStore;
        ViewBag.Store = mainStore;
        TempData["MainStore"] = mainStore;
        return View();
    }
}
{% endhighlight %}

Index view, controller'da olduğu gibi aynı söz dizimi ile koda erişerek Product nesnesini işler. ViewBag için değil fakat ViewData ve TempData nesnelerini view'da kullanırken cast işlemine tabi tutmayı unutmayınız.

**Index.cshtml**
{% highlight csharp %}
@using FourthCoffee.Models;
@{
    ViewBag.Title = "Home Page";
    var viewDataStore = ViewData["MainStore"] as Store;
    var tempDataStore = TempData["MainStore"] as Store;
}
<h2>Welcome to Fourth Coffee Bakery</h2>
<div>
  <a href="/Stores">
      <img src='@Url.Content("\\Content\\Images\\cake.jpg")' alt="Fourth Coffee Bakery" />
  </a>
  <div>
  Today's Featured Store is!
  <h4>@ViewBag.Store.Name</h4>
  <h3>@viewDataStore.Name</h3>
  <h2>@tempDataStore.Name</h2>
  </div>
  @Html.ActionLink("Test Tempdata", "Featured")
</div>
{% endhighlight %}

ViewBag nesnesi, onu çok yönlü bir araç haline getiren dinamik özellikler eklemenize izin verir.
Örnekteki view render edildiğinde her üç nesnede birşeyler gösterir fakat TempData bu şekilde kullanıldığında zahmetli olabilir, sebebine gelince...

### TempData
TempData'nın çok kısa süren bir instance'ı olması amaçlanmıştır ve bunu **yalnızca mevcut ve sonraki isteklerde kullanabilirsiniz!** TempData bu şekilde çalıştığından dolayı bir sonraki isteğin ne olacağını kesin olarak bilmeniz gerekir ve bunu garanti edebilmenin tek zamanı başka bir view'a yönlendirmenizdir. Bu nedenle, TempData'nın güvenilir şekilde çalışacağı tek senaryo yeniden yönlendirme yaptığınız zamandır. Çünkü bir yönlendirme(redirect) mevcut isteği(request) öldürür (ve 302 HTTP durum kodunu gönderir yani nesnenin istemciye(client) taşınması), sonra yeniden yönlendirilen view'a hizmet etmek için sunucuda yeni bir istek oluşturur. Yukarıdaki HomeController örneğine baktığımızda TempData nesnesi beklenenden farklı sonuçlar verebilir çünkü bir sonraki isteğin kaynağı garanti edilememiştir. Örneğin, bir sonraki istek tamamen farklı bir makineden ya da tarayıcıdan kaynaklanabilir.
Aşağıda açıklandığı gibi, TempData'yı kullanmak için kullanılan söz dizimi(syntax) ViewData ile aynıdır.

{% highlight csharp %}
// TempData sample
public ActionResult Store()
{
    var mainStore = new Store
    {
        Name = "The Cheesecake Factory Inc.",
        Address = "14 Tottenham Court Road - London, England",
        PhoneNumber = "555-555-55-55",
        StoreCode = "1001",
    };
    ViewData["MainStore"] = mainStore;
    ViewBag.Store = mainStore;
    TempData["MainStore"] = mainStore;
    // Redirect yapıldıktan sonra ViewBag & ViewData nesneleri artık geçerli değildir.
    // Bir yönlendirmede, yalnızca TempData geçerli/canlı kalabilir.
    return new RedirectResult(@"~\Store\");
}
{% endhighlight %}
Ancak, denetleyici yeniden yönlendirme yaptığında, ViewBag ve ViewData boş(null) değerler içerecektir. Yönlendirme sonrasında TempData nesnesini hata ayıklama(debug) moduyla incelerseniz, özellikli bir mağazanın tam olarak doldurulduğunu görürsünüz. Bunun nedeni, yönlendirme yalnızca sonraki istektir, bu nedenle yalnızca TempData nesnesine endişe duymadan erişebilir.

**Store.cshtml**
{% highlight csharp %}
@using FourthCoffee.Models;
@model FourthCoffee.Models.Store
@{
    ViewBag.Title = "Details";
    var store = TempData["MainStore"] as Store;
}
{% endhighlight %}

Klasik Session nesnesi, TempData nesnesinin yedekleme deposudur ve normal bir oturumdan daha hızlı bir şekilde, diğer bir deyişle hemen ardından yok edilir. Kısa süren kapsamı nedeniyle hata mesajlarını bir hata sayfasına iletmek harika bir yöntem.
ViewData, ViewBag ve TempData nesnelerini nasıl ve ne zaman kullanacağınızı şimdi gördünüz, daha büyük veri kümelerini ve ya daha karmaşık senaryoları kullanırken ne yapmanız gerektiğini merak ediyor olabilirsiniz. Neyse ki, MVC'de yaygın olarak ihtiyaç duyulan bu senaryoları ele almanın yolları vardır.
### ViewBag'in dışındaki durumlar
Gereksinimleriniz doğrultusunda, ViewBag, ViewData ve ya TempData nesneleri için pek de kullanışlı olmayan aşağıdaki veri tiplerini göstermeniz gerekebilir. MVC yapısı içerisinde ViewData'dan daha fazlasına ihtiyacınız olduğunda ViewModel nesneleri bulunur. ViewModel'e uyan veri türleri aşağıdaki gibidir:

* Master-detail veri
* Daha geniş veri setleri
* Karmaşık ilişkisel veri
* Raporlama ve kümesel(aggregate) veri
* Gösterge panoları(Dashboards)
* Farklı kaynaklardan gelen veriler

Büyük olasılıkla bu ve benzeri senaryolara uygulama geliştirme sürecinde gireceksiniz.

### Özet
ViewData ve ViewBag nesneleri, modelinizin yanında view'a giden bazı ekstra verilere erişmenin yollarını sunar. Ancak daha karmaşık veriler için ViewModel'e geçebilirsiniz. TempData diğer yandan **HTTP** yönlendirmelerinde veriler ile çalışmak için özel olarak tasarlanmıştır, bu nedenle TempData kullanırken temkinli olmayı unutmayın.

*Bu makale Rachel Appel'ın 2 Ocak 2014 tarihli [When to use ViewBag, ViewData, or TempData in ASP.NET MVC 3 applications](http://rachelappel.com/when-to-use-viewbag-viewdata-or-tempdata-in-asp.net-mvc-3-applications/) adlı makalesinden izin alınarak Türkçe'ye çevrilmiştir.*
