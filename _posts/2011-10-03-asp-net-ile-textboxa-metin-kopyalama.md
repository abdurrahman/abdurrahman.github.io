---
layout: post
title: ASP.NET ile textbox'a metin kopyalama
date: 2011-10-03 20:52:52 +02:00
abstract: Daha başka nasıl yazabilirdim başlığı bilmiyorum ama en anlaşılır şekli bu olsa gerek.  Birazdan örneği yazmaya başlayınca gerçi ben run-time yazıyorum...
---
Daha başka nasıl yazabilirdim başlığı bilmiyorum ama en anlaşılır şekli bu olsa gerek.  Birazdan örneği yazmaya başlayınca gerçi ben run-time yazıyorum siz okuyunca daha iyi anlayacaksınız. Asp.Net’e giriş tadında bu makalemi umarım beğenirsiniz.

Asp.Net’de ilk makalem olduğu için adım adım temelden gidiyorum, pro-developer’lar ziyaret etmişte okuyorsa hoş görsünler. Temel derken Asp.Net mimarisinden bahsetmeyeceğim tabiki File>New> klasör yönünü vb. belirteceğim. Başlayalım..

Yeni bir web projesi açıyoruz; File > New > Project diyoruz, karşımıza gelen dialog penceresinde solda C# dili içerisinde Web seçili olması gerek, sonrasında ASP.NET Web Application temasını seçip proje adımızı da belirterekten tamam diyoruz ve Default.aspx sayfası kod yazar halde bizi karşılıyor.

<img title="asp.net_gorunum" src="{{ site.baseurl }}/assets/asp-net_gorunum.jpg" alt="" width="614" height="345" />

Sol altta göründüğü gibi üç farklı çalışma görünümü seçeneği var, design bölümünde kullanılacak olan nesneleri sürükle-bırak yöntemi ile dilediğiniz konuma diyemiyeceğim bunun için CSS ile şablon yapmanız gerekecek aksi takdirde son nesnenin hemen arkasına gelir. Split’de hem design hem kod bölümü görüntülenir. Source bölümü işte biz coder’ların asıl ekmeğini kazandığı bölüm diyebiliriz. Kod bölümünde de aynı şekilde sürükle-bırak yöntemini kullanabilirsiniz, kod hazır halde ekleniyor ama en iyi şekilde öğrenmek için elle yazmak her zaman tavsiyemiz. Design tarafında 2 textbox ve bir buton ile görüntümüz şu şekilde olabilir.

<img title="textbox_copy_view" src="{{ site.baseurl }}/assets/textbox_copy_view.jpg" alt="" width="602" height="124" />

**Ek Bilgi:** Dikkat ederseniz Default.aspx yazan yerde ‘ * ’ işareti bulunmakta bu yaptığınız değişikliği kaydetmediğiniz anlamına gelir diğer deyişle derlemediğiniz…

{% highlight html %}
<%@ Page Language="C#" AutoEventWireup="true" CodeBehind="Default.aspx.cs" Inherits="GirilenTextiDigerTexteKopyalama._Default" %>

<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">

<html xmlns="http://www.w3.org/1999/xhtml" >
<head runat="server">
    <title>Untitled Page</title>
</head>
<body>
    <form id="form1" runat="server">
    <div>
    <strong>1. Textbox'a değer girip, butona tıkladığınızda girdiğiniz değer 2. Textbox'a kopyalanır.</strong>
        <br />
        <br />
        <asp:TextBox ID="txt_kaynak" runat="server" Width="218px"></asp:TextBox>
        <asp:Button ID="btn_kopyala" runat="server" Text="Kopyala !"
            Width="129px" />
        <asp:TextBox ID="txt_hedef" runat="server" Width="218px"></asp:TextBox>

    </div>
    </form>
</body>
</html>
{% endhighlight %}

Design’a göre kod göründüğü gibi otomatik gelmiş, yani tam anlamıyla birşey yazmadık ama mimari oluştu, .net’in amacı da bu zaten “az kod, çok iş…”

Eklediğimiz nesnelere özelliklerinden kendimiz isimlendirirsek kod tarafında işimiz daha çabuk ve kolay hallolur, bu amaçla bende 1. textbox’ı “txt_kaynak” diğerini de “txt_hedef” şeklinde isimlendirdim. Aynı şekilde butona da “btn_kopyala” adını verdim. Siz de değişik, daha kolay hatırlayabileceğiniz isimler verebilirsiniz. Bu şekilde kod yazmanız her zaman yararınıza olacaktır, alışkanlık haline getirin. Buraya kadar herşey normal ama biz butona tıkladığımızda şimdilik bi etki gerçekleşmeyecek, çünkü butona yapması gereken işlevi belirlemedik. Bunun için design tarafına geçip butona çift tıklıyoruz.

{% highlight csharp %}
namespace GirilenTextiDigerTexteKopyalama
{
    public partial class _Default : System.Web.UI.Page
    {
        protected void Page_Load(object sender, EventArgs e)
        {

        }

        protected void btn_kopyala_Click(object sender, EventArgs e)
        {
            // 1. textboxa girilen değeri diğerindede butona tıkladığımızda görmek istiyorsak iki textbox'ında text'lerini biribirine eşitlememiz gerekiyor, şöyle ki;
             txt_hedef.Text = txt_kaynak.Text;
        }
    }
}
{% endhighlight %}

Görüldüğü üzere buton kendi metodunu oluşturdu, bize de içini doldurmak kaldı..

Ünlü bir Csharp yazılımcı der ki; “yazdığın her satırdan sonra F6 ile uygulamayı derle..!” Yani bunun anlamı bir parmağın devamlı F6 tuşunda olması..

Bizde ustaya uyup uygulamamızı derliyoruz ve herhangi bir exception(hata) almamamız gerekiyor. Bu durumda butona tıklandığında tetikleme ile 1. textbox’daki değeri diğerine yazdırması gerekiyor.

Uygulamayı çalıştırmak için Debug > Start Without Debugging… ya da kısayolu Ctrl+F5’basıp projeyi çalıştırıyoruz..

<img title="text_result" src="{{ site.baseurl }}/assets/text_result.jpg" alt="" width="606" height="165" />

Böylece ilk capsli makalemi de yazmış oldum. İnşaallah daha iyilerini de yazarız, anlatırız, resimleriz… Hürmetler :)
