---
layout: post
title:  Entity Framework ile Veri Kontrolü ve Session Yönetimi
date: 2014-06-11 22:48:48 +02:00
abstract: Entity Framework’ün getirdiği kolaylığı yeni yeni keşfeden biri olarak, hızlıca ilerlemeye devam ediyorum. Daha önceki yazımda da proje bazlı öğrenme yolunda gittiğimi...
published: true
status: publish
categories:
- ASP.NET
- C#
- MS SQL
tags:
- asp.net session
- data control
- entity framework
- kullanıcı kontrolü
- login panel asp.net
- session yönetimi
- veri kontrolü
- veri tabanı
meta:
  dsq_thread_id: '3001018139'
  views: '3288'
---

Entity Framework’ün getirdiği kolaylığı yeni yeni keşfeden biri olarak, hızlıca ilerlemeye devam ediyorum. Daha önceki yazımda da proje bazlı öğrenme yolunda gittiğimi ve bunu da her zaman tavsiye ettiğimi belirtmiştim tekrar dile getirmek istiyorum. Bir önceki projemde entity framework kullanmadığım için oradaki kullanıcı kontrolünü yapıp, kayıt varsa session atayıp giriş yapmasını eski yöntem ile yapmıştım. EF’de (artık kısaltalım 🙂 ) bunu nasıl yapabilirim derken daha doğrusu projeye de dahil etmek istediğim bir şeydi ve bu makale ortaya çıkmış oldu, umarım beğenirsiniz. Yanlış kavram ve ya yöntem kullanmış olabilirim, yorumlarda bunu belirtirseniz sevinirim, hep beraber öğrenelim.

İlk adım veri kontrolü olacağı için veri tabanımızda kullanıcı tablomuzu oluşturalım ve ardından bir kayıt ekleyelim.

{% highlight sql %}
create database Users
(
  UserId int identity(1,1),
  UserName nvarchar(50),
  Password nvarchar(50)
)
insert into Users values ('admin','123456')
{% endhighlight %}

Şimdi sırada giriş sayfamızın form verilerini oluşturmak için **Login.aspx** adında yeni bir web form ekleyelim. Dipnot olarak ben tasarım tarafına vakit ayıramadığım için genelde hoşuma giden tasarımları koda dökmeyi tercih ediyorum. Burada hazır dökülmüşü var :D Burada tasarım olarak ben psdup.com'dan bir yönetim paneli tasarımı seçtim, sizde kullanmak isterseniz indirmek için resme tıklayabilirsiniz. Diğer login panel tasarımları için www.psdup.com'u ziyaret edebilirsiniz.

<img class="alignnone" src="{{ site.baseurl }}/assets/yonetici-girisi-klasik.jpg" alt="" width="580" height="190" />

{% highlight html %}
<html lang="tr">
<head runat="server">
 <title>Uykusuz Adam</title>
 <link rel="stylesheet" href="~/css/main.css" type="text/css"><br />
 <script src="https://ajax.googleapis.com/ajax/libs/jquery/1.7.1/jquery.min.js" language="javascript"></script>
 <script type="text/javascript">
  $(document).ready(function () {
    $("#remember").change(function () {
      if ($(this).is(":checked")) {
        $(".check label").addClass("true");
      }
      else {
        $(".check label").removeClass("true");
      }
    });
 });
 </script>
</head>
<body>
 <form id="form1" runat="server">
  <div id="container">
    <div class="panel">
      <h2>Hoşgeldiniz</h2>
        <p>Yönetim paneline erişmek için kullanıcı bilgilerinizi giriniz.</p>
        <hr />
        <ul class="login">
          <li class="text">
            <label for="username">Kullanıcı :</label><asp:TextBox ID="username" runat="server" tabindex="0" type="text" />
          </li>
          <li class="text">
            <label for="password">Şifre :</label><asp:TextBox ID="password" runat="server" tabindex="1" type="password" />
          </li>
          <li class="send">
            <div class="check">
              <input runat="server" id="remember" type="checkbox" />
              <label for="remember">Beni Hatırla</label>
            </div>
            <asp:Button ID="send" runat="server" onclick="send_Click"></asp:Button>
          </li>
        </ul>
      </div>
    </div>
 </form>
</body>
</html>
{% endhighlight %}

Burada entity framework ile veri kontrolümüzü yapmak için Login.aspx dosyamızın kod tarafına geçiyoruz. Burada veri tabanımızda kullanıcı varsa kullanıcımıza oturum açıp Index.aspx sayfamıza yönlendiriyoruz.

{% highlight csharp %}
protected void send_Click(object sender, EventArgs e)
{
    using(var context = new MyDbEntities())
    {
        var username = username.Text;
        var password = password.Text;
        var isUserValid = context.Users.Any(x => x.UserName == username && x.Password == password);
        if(isUserValid)
        {      
            Session["adminSession"] = username.Text;
            Session.Timeout = 30;
            Response.Redirect("Index.aspx"); 
        }
        else
        {
            username.Text = "";
            password.Text = "";
        }
    }
}
{% endhighlight %}

Hem kullanıcımızın veri tabanında kaydı var mı yok mu bunu sorgulayıp panelden başarılı ise giriş yaptık hem de 30 dakikalık oturum süresi belirleyerek kullanıcının session üzerinde diğer bir sayfaya(Index.aspx) sayfamıza yönlendirdik.

{% highlight html %}
<html lang="tr">
<head runat="server">
 <title>Login Panel</title>
</head>
<body>
  <form id="form1" runat="server">
    <div id="container">
      <p>Hoşgeldin <asp:Label ID="lblSession" runat="server"></asp:Label> | <asp:LinkButton ID="lbSessionOut" runat="server" OnClick="lbSessionOut_Click">Çıkış</asp:LinkButton></p>
    </div>
  </form>
</body>
</html>
{% endhighlight %}

Burada ise kullanıcı giriş yaptıktan sonra yönlendirilen sayfada giriş yapan kullanıcının session adını label nesnemize yazdırdık, bu bahsettiğim "admin(session) olarak giriş yaptınız." gibisinden bir yazı olabilir size kalmış..

{% highlight csharp %}
protected void Page_Load(object sender, EventArgs e)
{
  if (Session["adminSession"] != null)
    lblSession.Text = Session["adminSession"].ToString();
}
protected void lbSessionOut_Click(object sender, EventArgs e)
{
  Session.Abandon();
  Response.Redirect("Login.aspx");
}
{% endhighlight %}

Bir yazının daha sonuna geldik, bu yazımda entity framework oluşturmaktan bahsetmedim, bunun için bir önceki yazıma göz atabilirsiniz. Takıldığınız ve ya sormak istediğiniz bir şey olursa yorum yazmaktan çekinmeyin.
