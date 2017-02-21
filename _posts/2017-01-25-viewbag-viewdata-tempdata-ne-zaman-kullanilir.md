---
layout: post
title: ASP.NET MVC uygulamalarında ViewBag, ViewData ve TempData ne zaman kullanılır ?
---

<p><strong>"Ne zaman bir ViewBag/ViewData/TempData nesnesi kullanmalıyım ?"</strong>-forumlarda, gruplarda sürekli sorulan ya da tartışılan soru. Bu nesnelerin arasında, MVC uygulamaları geliştirirken bu nesnelerin her birini tam olarak nasıl kullanabileceğimizi görmek için yeterince benzerlikler ve farklılıklar vardır.</p>
<p>Her üç nesnede view ve controller'da özellik(properties) olarak mevcuttur. Bir kural olarak küçük miktarda bir veriyi belirli bir yere taşımak amacıyla ViewBag, ViewData ya da TempData kullanırız(Ör: controller'dan bir veriyi view'da görüntülemek). ViewBag ve ViewData nesneleri aşağıdaki senaryolarda etkili çalışır:</p>
<ul>
<li>Bir tablodan verileri açılır listeye(dropdown,combobox gibi) doldurmak</li>
<li>Components/Bileşenler (alışveriş sepeti gibi)</li>
<li>Widget'lar (kullanıcı profil widget'ı gibi)</li>
<li>Az miktarda hesaplanmış veri</li>
</ul>
<p>TempData nesnesi bir temel senaryoda iyi çalışır:</p>
<ul>
<li>Güncel ve bir sonraki HTTP istekleri arasında veri taşımak</li>
</ul>
<p>Daha büyük miktarda veri ile çalışmak, verileri raporlamak, gösterge panoları(dashboard) oluşturmak veya çok sayıda farklı veri kaynağı ile çalışmak isterseniz bir ViewModel nesnesi kullanabilirsiniz.</p>
<h2>ViewData &amp; ViewBag nesneleri</h2>
<ul>
<li>ViewData
<ul>
<li>ViewData, view'da görüntülemek üzere içerisine veri yerleştirebileceğiniz bir dictionary nesnesidir. ViewData, "key/value" söz dizimini kullanan ViewDataDictionary sınıfının bir türevidir.</li>
</ul>
</li>
<li>ViewBag
<ul>
<li>ViewBag nesnesi, dinamik olarak view için özellikler oluşturmanıza olanak sağlar.</li>
</ul>
</li>
</ul>
<p>ViewData ve ViewBag nesneleri controller ve view arasında ekstra veriye(data model'in dışında) erişmek için idealdir. View'lar belirli bir nesneyi kendi modelleri olarak zaten beklediğinden bu veri türü ek verilere erişir, MVC bu nesneyi hem view'ın hem de controller'ın bir özelliği olarak uygular ve bu nesnelerler erişimi ve kullanımı kolaylaştırır.</p>
<p>ViewBag, ViewData, ve TempData nesnelerin kullanımı ve söz dizimi aşağıdaki kod örneğinde özetlenmiştir.</p>
<p><strong>HomeController.cs</strong></p>

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
