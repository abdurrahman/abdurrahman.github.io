---
layout: post
title: Asp.Net ile MySQL Kullanımı
date: 2014-08-10 21:37:02 +02:00
abstract: C# programlama dilinde başlangıç olarak ilk konumuz değişken kavramı olacaktır. Değişkenler ihtiyaca göre tanımlanır, örneğin sayısal bir veri üzerinden derleyiciye iş yaptıracağız, bunun için bizim bellekte bir sayı değişkeni tanımlamamız gerekecek.
published: true
status: publish
categories:
- ASP.NET
- C#
tags:
- asp.net de mysql
- asp.net entity mysql
- ASP.NET ile mysql
- c# ve mysql
- mysql connector
- mysql entity framework
- mysql framework
- MySQL Kullanımı
- phpmyadmin
- veri tabanı
meta:
  dsq_thread_id: '3001755600'
  views: '5913'
---
Bu zamana kadar veri tabanı işlemlerimi hep MS SQL üzerinden yapmıştım ama bu hep lokalde çalıştığım içindi ve hiç değişmemişti. Daha sonra üzerinde çalıştığım bir projeyi hayata geçirmek üzere php deneyimlerimde de hala kullandığım MySQL'i Asp.Net ile de kullanmak zorunda hissetim. Bu zorunluluk biraz da "hep SQL Server kullandım, Asp.Net ile **MySQL** kullanıldığını da biliyordum ve neden onu da öğrenmeyeyim" iç sesimin dışa vurumuydu.

Kısaca gerekli programların adreslerini ve Asp.Net projemizle nasıl MySQL bağlantısı yapılıra değinip yazımı sonuçlandırmayı hedefliyorum.

Öncelikle **SQL Server**'daki gibi MySQL'in de arka planda çalışması gereken servis programı için [buradan](http://dev.mysql.com/downloads/mysql/5.0.html#downloads) MySQL'i indirebilirsiniz. Ben PHP çalışmalarım içinde yıllardır kullandığım <em>Wamp Server üzerinden MySQL ve phpMyAdmin de veri tabanı işlemlerimi halledebiliyorum. *(Wamp Server uygulamasını çalıştırdığımda Apache, PHP ve MySQL server'lari da çalışmış oluyor kullanışlı olduğu için ve arada php ile de bir şeyler yapıyorsanız sizde kullanabilirsiniz.)*

Servis kurulumundan sonra veri tabanı işlemlerimizi yapabileceğimiz **MySQL-Front** uygulamamızı [bu linkten](http://www.mysqlfront.de/) indirelim. *(Bu uygulamayı deneyimlemek için bende indirdim ama normalde yukarıda dediğim gibi phpmyadmin ile de işlemlerimi yapabiliyorum kısaca Wamp Server bunu da indirmenizi gerektirmiyor.)*

İndirilmesi gerekenlerde son olarak bizim MySQL'i Asp.Net projemizde kullanabilmemiz için MySQL framework'lerini indirmemiz gerekiyor. Daha sonra bu framework'leri yani dll kütüphanelerimizi projemize referans olarak ekleyeceğiz. **Connector** için de gerekli uygulamayı [buradan](http://dev.mysql.com/downloads/connector/net/) indirebilirsiniz. Şunu belirtmek isterim ki ben son versiyonu indirdiğimde ve dll'leri projeme eklediğimde sonuç alamadım. Buna çözüm olarak daha eski versiyonlardan **6.5.7** versiyonunu indirmiştim. Sizde sonuç almazsanız yani kullandığınız son versiyon dll'leri işe yaramaz ise eski versiyonlardan faydalanabilirsiniz.

Kurulumlarla ilgili çok basit olduğundan caps paylaşmak istemedim, yinede bununla ilgili youtube'tan videolarla, google'dan resimli anlatımlarla kendinize kolaylık sağlayabilirsiniz.
<img src="{{ site.baseurl }}/assets/mysql-front-uykusuzadam.png" alt="mysql-front-uykusuzadam" width="937" height="639" /> MySQL-Front uygulamasında tablo ekleme görünümü

Kurulumlardan sonra veri tabanımızda oluşturduğumuzu da varsayarak projemizde uygulamaya geçiyoruz. Ben MySQL-Front'da *UserName, Password, EmailAddress* kolonlarıyla users adında bir tablo oluşturdum. Uygulamamızı **login panel** mantığında çalışan kullanıcı var mı/yok mu sorgusuyla var ise giriş yapsın, yok ise *"kullanıcı adı ve ya şifre hatalı"* diye uyarsın şeklinde planladım.

Visual Studio'da projemizi oluşturduktan sonra yazının girişinde de bahsettiğimiz gibi MySQL kütüphanesini projemize referans etmemiz gerekiyor. References > Add References > Browse bölümüne gelip bu dizine geliyoruz *C:\Program Files\MySQL\MySQL Connector Net 6.5.x\Assemblies\v2.0*
<img src="{{ site.baseurl }}/assets/mysql-dll-uykusuzadam.png" alt="mysql-dll-uykusuzadam" width="482" height="408" />
Ben projelerimde EF (Entity Framework) kullandığım için **MySQL.Data.Entity.dll**'de seçiyorum, siz seçmeyebilirsiniz. Geçelim uygulamamıza; projemize Login.aspx adında yeni bir webform ekleyelim. HTML tarafi şu şekilde;
{% highlight html %}
<%@ Page Language="C#" AutoEventWireup="true" CodeBehind="Login.aspx.cs" Inherits="ForumSanat.fspan.Login" %>
<!DOCTYPE html>
<html lang="tr">
<head runat="server">
  <title>Forum Sanat Merkezi - Panel Giriş</title>
  <link rel="stylesheet" href="~/css/normalize.css" type="text/css" />
  <style type="text/css">
    body {
      height:100%;
      background:url(images/login-bg.jpg) no-repeat;
      background-position:fixed;
      background-size:cover;
      -webkit-background-size:cover;
      -o-background-size:cover;
      -moz-background-size:cover;
      font:18px Calibri,sans-serif;
    }
    #container{
      margin:220px auto;
      width:200px;
    }
    .text-user, .text-pass, .login-button {
      width:200px;
      height:36px;
      box-shadow:0px 1px 13px #000
    }
    .text-user{
      background:url(images/user-input.png) no-repeat;
      margin:0 auto;
    }
    .text-pass{
      background:url(images/pass-input.png) no-repeat;
      margin:10px auto;
    }
    .text-field {
      float:right;
      width:138px;
      height:26px;
      border:none;
      padding:5px 10px;
    }
    .login-button {
      background:#7ab752;
      margin:1px auto 0px;
      text-align:center;
      color:#FFF;
      border:none;
      border-top-left-radius:4px;
      border-bottom-left-radius:4px;
      -webkit-transition:background 0.5s;
    }
    .login-button:hover {
      background:#DC3F42 /*#81c356*/;
    }
    #container a {
      color:#7ab752;
      text-decoration:none;
      text-shadow:0px 1px 5px #000;
      -webkit-transition:color 0.5s
    }
    #container a:hover { color:#FFF }
  </style>
</head>
<body>
<form id="form1" runat="server">
  <div id="container">
    <asp:Label ID="lblDurum" runat="server"></asp:Label>
    <div class="text-user">
      <asp:TextBox ID="txtUser" runat="server" PlaceHolder="Username" TabIndex="1" class="text-field"></asp:TextBox>
    </div>
    <div class="text-pass">
      <asp:TextBox ID="txtPass" runat="server" PlaceHolder="Password" TextMode="Password" TabIndex="2" class="text-field"></asp:TextBox>
    </div>
    <asp:Button ID="btnLogin" runat="server" Text="GİRİŞ" TabIndex="3" class="login-button" onclick="btnLogin_Click" />
    <p><a href="/Default.aspx">← ana sayfaya geri dön!</a></p>
  </div>
</form>
</body>
</html>
{% endhighlight %}
<img src="{{ site.baseurl }}/assets/login-screen-uykusuzadam.png" alt="login-screen-uykusuzadam" width="526" height="294" />
Bu şekilde bir görünüm elde ettim, gayet hoş oldu :)

Daha sonra butonumuzun işlevselliği için design tarafına geçip butona çift tıklıyoruz. **Login.aspx.cs** sayfasına geçtiğimize göre ilk önce sayfamızın üst kısmındaki **namespace** grubuna <span style="color: #0000ff;">**using MySql.Data.MySqlClient;**</span> kütüphanemizi kullanmak için ekliyoruz. Daha sonra buradaki kod bölümüzde şu şekilde;
{% highlight csharp %}
using System;
using System.Collections.Generic;
using System.Linq;
using System.Web;
using System.Web.UI;
using System.Web.UI.WebControls;
using MySql.Data.MySqlClient;

namespace ForumSanat.fspan
{
  public partial class Login : System.Web.UI.Page
  {
    protected void Page_Load(object sender, EventArgs e)
    {
    }

    public static MySqlConnection msc = new MySqlConnection("server=localhost; user id=root; database=fsm_db; pooling=false");
    // M.Schumacher'e de selam olsun! (:
    public static MySqlCommand sorgu;
    public static MySqlDataReader dr;
    protected void btnLogin_Click(object sender, EventArgs e)
    {
      sorgu = new MySqlCommand("select UserName,Password from users where UserName=@User and Password=@Pass", msc);
      sorgu.Parameters.Add("@User", MySqlDbType.VarChar).Value = txtUser.Text;
      sorgu.Parameters.Add("@Pass", MySqlDbType.VarChar).Value = txtPass.Text;
      msc.Open();
      dr = sorgu.ExecuteReader();
      if (dr.Read())
      {
        Response.Redirect("~/Default.aspx");
      }
      else
      {
        Response.Redirect("Login.aspx");
        lblDurum.Text = "Kullanıcı adı ve şifre hatalı !";
      }
      dr.Close(); msc.Close();
    }
  }
}
{% endhighlight %}
Gördüğünüz gibi SQL Server ile yaptığımız veri tabanı uygulamaları ile arasında pek bir fark yok, **MySqlConnection** ile bağlantı cümlemizi yazıyoruz, burada ben wampserver kurulumunda root'a şifre tanımlamamıştım onun için eklemedim. Şifre belirlediyseniz ya da varsa **user id=root; password=mysqlkullanicisifresi;** seklinde örnekteki bağlantı cümlesini düzenlemeniz gerekmekte.. **MySqlCommand** ile sorgumuzu tanımladık, **MySqlDataReader** ile de veri okuyucumuzu tanımladık, aynı SqlCommand ve SqlDataReader gibi.. Uygulamamızı çalıştırdığımızda kullanıcı bilgilerini girdikten sonra butonu tıkladığında girdiği bilgiler veri tabanımızdakilerle karşılaştırılıp varsa yani okuyucu gelen veriyi doğruluyorsa Default.aspx sayfamıza yönlendirecek okumuyorsa yani else durumunda ise Login.aspx sayfasına tekrar yönlendirilecek.

Kısaca MySQL kullanımına değinmeye çalıştım, umarım faydalı olabilmişimdir. Sorularınız varsa yorumda belirtirseniz yardımcı olurum.

Kolaylıklar dilerim.
