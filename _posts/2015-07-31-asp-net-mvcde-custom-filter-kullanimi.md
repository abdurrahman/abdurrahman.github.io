---
layout: post
title: ASP.NET MVC'de Custom Filter Kullanımı
date: 2015-07-31 01:32:53 +02:00
abstract: Diyelim ki kurumsal bir web sitesi yapıyorsunuz ve müşterinizin içerik girebileceği, düzenleyebileceği ya da modül ekleyip çıkarabileceği vs. bir yönetim paneline ihtiyacı var...
---
Diyelim ki kurumsal bir web sitesi yapıyorsunuz ve müşterinizin içerik girebileceği, düzenleyebileceği ya da modül ekleyip çıkarabileceği vs. bir yönetim paneline ihtiyacı var. Bu saydığımız işlemlerin bir yönetici tarafından yapılacağı gibi sadece yöneticinin erişebileceği sayfalara yetki kontrolü gerekecek. Böyle bir senaryoda çözümünüz ne olurdu ?

Asp.Net'in [Identity ](http://www.asp.net/identity/overview/getting-started/introduction-to-aspnet-identity) yapısını kullanabilirsiniz, mvc yapısına uygun ve code first bilgisi gerektiriyor. Ama burada söz ettiğimiz yapı kurumsal bir sitenin yönetim paneli olduğu için **Identity** 'nin fazla kompleks kalacağı gibi büyük projeler(CRM, ERP vb.) ile kullanılması daha yerinde olacaktır. Tabi bu tamamen size kalmış birşey, şahsi görüşüm olduğunu belirtmek istiyorum.

Asp.Net Identity bilmeyenler için konumuza devam edersek **aradığımız çözüm** kullanıcı giriş sayfasında(Login) kullandığımız **Cookie** ya da **Session** adımızı, yetki kontrolünü yapacağımız sayfaların hepsinde tek tek sorgulatarak **if-else** durumuna göre sayfaya girişini kontrol etmek mi olurdu ? - *İtiraf etmeliyim ki Webform zamanlarımda bunu yapmıştım, 4-5 sayfalık yönetim paneli olan bir site için tek tek session kontrolü yapıyordu. -* Evet bu bir çözüm olabilir ama profesyonelce değil !

### FilterAttribute, IActionFilter

Çözüm : Kendi filtre özelliğimizi yazarak ve yazdığımız bu özelliği hangi action için ya da tamamen bir controller için geçerli olmasını istiyorsak onun hemen tepesine ekleyerek o action/controller'in bir filtre özelliğine bağlı olarak kontrol edildiğini belirleyebiliriz.

**O kadar anlattın, hiç birşey anlamadık ?!** Tabi ki hemen açıklık getireyim; örnek olarak MVC'de bir sayfada post işlemi yapılacaksa o sayfanın action methodunun üzerine **[HttpPost]** ekleriz, bu bir attribute'dür yani action post olduğunda bu özelliği kullanacağını tepesine ekleyerek belirtiriz ya da ckeditor kullandigimiz bir sayfanin html etiketlerini kaydedebilmesi ve [potansiyel saldırı hatası](http://stackoverflow.com/a/30522282/4387930) alamamak için **[ValidateInput(false)]** attribute'ünü yani özelliğini kullanırız. Biz de kendi filtremizi yazarak hangi sayfanın kullanıcı yetkisine ihtiyacı varsa o sayfanın action methodu üzerine sadece **[YekiKontrolluSayfa]** vb. şeklinde filtre kontrolümüzü ekleyerek o sayfanın yetki kontrolüne tabi tutulduğunu belirtebiliriz. Aşağıdaki örneğimizde daha iyi pekiştireceğinizi düşünüyorum.

Buradaki örnekte login sayfasında kullanıcı bilgileri doğrulandığında **username** adında 1 gün geçerli cookie'miz oluşuyor;
{% highlight csharp %}
public ActionResult Login()
{
    return View();
}
[HttpPost]
public ActionResult Login(UserModel model)
{
    if (!ModelState.IsValid)
    {
        ViewBag.Message = "Lütfen tüm alanları doldurunuz.";
    }
    string encryptedPass = Encryption.Base64Encode(model.Password);
    string publicIP = Request.ServerVariables["REMOTE_ADDR"];
    bool result = UserRepo.Instance.GetUserLoginInfo(model.Username, encryptedPass, publicIP);
    if (result)
    {
        HttpCookie CookieUsername = new HttpCookie("username", model.Username);
        CookieUsername.Expires = DateTime.Now.AddDays(1);
        Response.Cookies.Add(CookieUsername);
        return RedirectToAction("index", "home");
    }
    ViewBag.Message = "Kullanıcı adı/parola geçersizdir";
    return View();
}
{% endhighlight %}
Bu adımdan sonra kullanıcı giriş yaptığında yetki kontrolü isteyecek sayfalar için bize gerekecek olan özel filtremizi yazabiliriz. Proje içerisinde dilerseniz Helper adında klasör oluşturup içerisine CustomFilter.cs adında dosya ekleyebilirsiniz.
{% highlight csharp %}
using System;
using System.Web;
using System.Web.Mvc;
using System.Web.Routing;
namespace CustomFilterApp.Helper
{
    public class AuthFilterAttribute : FilterAttribute, IActionFilter
    {
        public void OnActionExecuting(ActionExecutingContext context)
        {
            HttpContextWrapper wrapper = new HttpContextWrapper(HttpContext.Current);
            if (context.HttpContext.Request.Cookies["username"] == null || String.IsNullOrEmpty(context.HttpContext.Request.Cookies["username"].Value))
            {
                context.Result = new RedirectToRouteResult(
                new RouteValueDictionary { { "controller", "account" }, { "action", "login" } });
            }
        }
        public void OnActionExecuted(ActionExecutedContext context)
        {
        }
    }
}
{% endhighlight %}
Filtre özelliğine ben isim olarak **AuthFilter** adını verdim, siz farkli sekilde isimlendirebilirsiniz. Dilerseniz **OnActionExecuting**, **OnActionExecuted** methodlarından başlayalım. Kabaca anlatırsak; kullanıcı login olur olmaz yönetim panelinin giriş sayfasına yönlendirilmesi gerekiyor. Dolayısıyla bir kere filtremizi o action'un üzerine bir ekleyelim;
{% highlight csharp %}
using System;
using System.Collections.Generic;
using System.Linq;
using System.Web;
using System.Web.Mvc;
using CustomFilterApp.Helper; // Filtremize erisebilmek icin namespace'e ekliyoruz.
namespace CustomFilterApp.Areas.Manage.Controllers
{
    public class HomeController : Controller
    {
        [AuthFilter] // Yetkili kullanici cookie kontrolü
        public ActionResult Index()
        {
            return View();
        }
    }
}
{% endhighlight %}
Peki filtre neye göre çalışıyor, nasıl anlıyor ? Sorumuzu cevaplandıracak olursak; kullanıcı login sayfasından giriş sayfasına yönlendiğinde giriş sayfasının action methodu tetikleniyor. Tam bu sırada söz konusu action filtremizin *OnActionExecuting* methodunda yer alan cookie kontrolümüzden geçiyor. Koşul sağlanıyorsa sayfaya yönleniyor, sağlanmıyorsa login sayfasına geri döndürülüyor. Hatta her action'a tek tek yazmak yerine HomeController'da yer alan action'lar zaten yetkili kişi ilgilendiren sayfalar olacağı için sadece controller'in uzerine eklemeniz yeterli olacaktır.
<blockquote>OnActionExecuting: Action metod tetiklendiği anda devreye girer.<br>
OnActionExecuted: Action metod çalıştırıldıktan sonra devreye girer.</blockquote>

MVC'de yer alan filtre tiplerini detaylı olarak [buradan](https://msdn.microsoft.com/en-us/library/gg416513(VS.98).aspx) inceleyebilirsiniz.

Böylece Action Filter Attribute kullanımını görmüş,  projemize esneklik ve profesyonellik kazandırmış olduk. Bir benzer uygulamayı da [buradan okuyabilirsiniz.](https://visualstudiomagazine.com/articles/2013/08/28/asp_net-authentication-filters.aspx) Tabi örnekler çoğaltılabilir; mesela bazı methodlarınızın tetiklendiğinde değilde çalıştırıldığında log tutmasını ya da hata meydana geldiğinde mail olarak bildirmesini yine filter kullanarak yapabilirsiniz. Umarım katkısı olmuştur. 

Keyifli kodlamalar..
