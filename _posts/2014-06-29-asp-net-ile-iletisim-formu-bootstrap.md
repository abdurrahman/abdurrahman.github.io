---
layout: post
title: ASP.NET ile İletişim Formu (Bootstrap)
date: 2014-06-29 23:51:20 +02:00
abstract: Bu yazımızda çoğu web sitesinde bulunan iletişim formlarının yapımı ve çalışma mantığını kavramaya çalışacağız. İletişim sayfası bir web sitesinin olmazsa olmazlarından olduğu aşikar. İlk önce form verilerimizi alacağımız sayfanın görünümünü oluşturup...
published: true
status: publish
categories:
- ASP.NET
- C#
tags:
- asp.net ile mail gönderimi
- asp.net iletişim formu
- bootstrap form
- contact form with asp.net
- csharp mail gönderimi
- iletişim formu
meta:
  dsq_thread_id: '2999043206'
  views: '9216'
---
Bu yazımızda çoğu web sitesinde bulunan iletişim formlarının yapımı ve çalışma mantığını kavramaya çalışacağız. İletişim sayfası bir web sitesinin olmazsa olmazlarından olduğu aşikar. İlk önce form verilerimizi alacağımız sayfanın görünümünü oluşturup, textbox ve label nesnelerimizi konumlandıralım. Dipnot olarak ben bu uygulamada **bootstrap**'tan yararlandım siz kendinize göre css ile şekil verebilirsiniz ya da bootstrap'in nimetlerinden yararlanabilirsiniz. **uykusuzadam** bootstrap tavsiye eder :P

Formumuzun görünümü şu şekilde, yine burada bootstrap'ın pop-up modal güzelliğinden yararlandım.
<img  src="{{ site.baseurl }}/assets/aspnet-iletisim-uykusuzadam.png" alt="aspnet-iletisim-uykusuzadam" width="628" height="466" />
**Index.aspx** dosyamızın form içeriği;
{% highlight html %}
<body>
  <form id="form1" runat="server">
    <div class="modal fade" id="contact" tabindex="-1" role="dialog" aria-labelledby="myModalLabel" aria-hidden="true">
      <div class="modal-dialog">
        <div class="modal-content">
          <div class="modal-header">
            <button type="button" class="close" data-dismiss="modal"  aria-hidden="true">&amp;times;</button>
            <h4 class="modal-title" id="H1">Bize Ulaşın !</h4>
          </div>
          <div class="modal-body">
            <label for="txtFormName" class="col-xs-3">Ad ve Soyad:</label>
            <asp:TextBox ID="txtFormName" runat="server" class="form-control input-sm" placeholder="Adınız ve soyadınız..."></asp:TextBox><br />
            <label for="txtFormMail" class="col-xs-3">Mail Adresi:</label>
            <asp:TextBox ID="txtFormMail" runat="server" class="form-control input-sm" placeholder="Mail adresiniz..."></asp:TextBox><br />
            <label for="txtFormSubject" class="col-xs-3">Konu:</label>
            <asp:TextBox ID="txtFormSubject" runat="server" class="form-control input-sm" placeholder="Ne hakkında..."></asp:TextBox><br />
            <label for="txtFormMsg" class="col-xs-3">Mesaj:</label>
            <asp:TextBox ID="txtFormMsg" runat="server" class="form-control input-sm" TextMode="MultiLine" Rows="5"></asp:TextBox><br />
            <asp:Label runat="server" ID="lblFormResult" style="float:right;width:380px;"></asp:Label>
            <asp:Button Text="Gönder" runat="server" class="btn btn-primary" OnClick="btnForm_Click" />
          </div>
        </div>
      </div>
    </div>
  </from>
</body>
{% endhighlight %}
Index.aspx.cs ile gönder butonumuza tıklandığında form verilerinin gönderileceği mail adresi ve diğer detayları belirliyoruz, ben gmail hesabimi kullaniyorum siz baska hesap kullaniyorsaniz onun smtp bilgilerini belirtmeniz gerekmekte..;
{% highlight csharp %}
protected void btnForm_Click(object sender, EventArgs e)
{
  MailMessage mm = new MailMessage();
  mm.To.Add("uykusuzadam@yandex.com");
  mm.From = new MailAddress(txtFormMail.Text,txtFormName.Text);
  mm.Subject = txtFormSubject.Text;
  mm.IsBodyHtml = true;
  mm.BodyEncoding = Encoding.GetEncoding(1254);
  mm.Body = txtFormMsg.Text + "<br /><hr /><h2>Bu mail size iletişim formundan gönderilmiştir!</h2>";

  NetworkCredential nc = new NetworkCredential("mailadresiniz@gmail.com","sifreniz");
  SmtpClient sc = new SmtpClient();
  sc.Credentials = nc;
  sc.Host = "smtp.gmail.com";
  sc.Port = 587;
  sc.EnableSsl = true;

  try
  {
    sc.Send(mm);
    lblFormResult.Text = "<p style=" + "color:#7ede95" + ">İlginiz için teşekkür ederiz!</p>";
    txtFormMail.Text = "";
    txtFormMsg.Text = "";
    txtFormName.Text = "";
    txtFormSubject.Text = "";
  }
  catch (Exception)
  {
    lblFormResult.Text = "<p style=" + "color:red" + ">Mesajınız iletilmedi, yeniden deneyin!</p>";
  }
}
{% endhighlight %}
Kısaca iletişim formunun kullanımını anlatmaya çalıştım, siz projenize göre textfield'larınızı detaylandırabilirsiniz. Umarım faydalı olabilimişimdir.
