---
layout: post
title:  Entity Framework ile Insert Kullanımı
date: 2014-06-18 23:35:36 +02:00
abstract: En iyi öğrenme yolu herkesin de tavsiye ettiği gibi uygulamalı(proje) olarak öğrenmedir düşüncesiyle hareket ederek yeni başladığım web projemde Entity Framework kullanmaya karar verdim..
---

Bu yazımda **entity framework** kullanarak nasıl veri tabanına kayıt ekleriz onu anlatmaya çalışacağım. Visual Studio'larımız açıksa hemen başlayabiliriz.

**Default.aspx dosyamız;**
{% highlight csharp %}
<div id="product-add">
  <h2>Yeni Ürün Ekle</h2>
  <asp:Label runat="server" class="label">Ürün Adı :</asp:Label>
  <asp:TextBox ID="txtUrunAdi" runat="server" class="text" placeholder="Örn: iPhone 5S"></asp:TextBox>
  <asp:RequiredFieldValidator ID="rfvProductName" runat="server" ControlToValidate="txtUrunAdi" Display="Dynamic" ErrorMessage="Ürün Adı Girmediniz !"></asp:RequiredFieldValidator><br />
  <asp:Label runat="server" class="label">Renk :</asp:Label>
  <asp:TextBox ID="txtRenk" runat="server" class="text" placeholder="Örn: Kırmızı"></asp:TextBox><br />
  <asp:Label runat="server" class="label">Fiyatı :</asp:Label>
  <asp:TextBox ID="txtFiyat" runat="server" class="text" placeholder="Örn: 1500"></asp:TextBox>
  <asp:RegularExpressionValidator ID="revFiyat" runat="server" ControlToValidate="txtFiyat" ErrorMessage="Geçerli bir fiyat girin!" ValidationExpression="^[0-9]+$"></asp:RegularExpressionValidator>
  <asp:RequiredFieldValidator ID="rfvFiyat" runat="server" ControlToValidate="txtFiyat" ErrorMessage="Fiyat Girmediniz!"></asp:RequiredFieldValidator><br />
  <asp:Label runat="server" class="label">Stok Adedi :</asp:Label>
  <asp:TextBox ID="txtStok" runat="server" class="text"></asp:TextBox><br />
  <asp:Label runat="server" class="label">Markası :</asp:Label>
  <asp:DropDownList ID="ddlMarka" runat="server" class="text" ></asp:DropDownList>
  <asp:RequiredFieldValidator ID="rfMarka" runat="server" ControlToValidate="ddlMarka" ErrorMessage="Ürünün markasını seçmediniz!" InitialValue="Birini seç!"></asp:RequiredFieldValidator><br />
  <asp:Label runat="server" class="label">Resim Ekle :</asp:Label>
  <asp:FileUpload ID="fuResimYukle" runat="server" /><br /><br />
  <asp:Label runat="server" class="label">Teknik Özellikleri :</asp:Label><br /><br />
  <asp:TextBox ID="txtTeknik" runat="server" TextMode="MultiLine" class="textarea"></asp:TextBox>
  <asp:Label ID="lblDurum" runat="server" Text=""></asp:Label>
  <asp:Button ID="btnUrunEkle" Text="Ürün Ekle" runat="server" class="btn" onclick="btnUrunEkle_Click" /><br /><br />
</div>
{% endhighlight %}

Bu aspx dosyamızda ben div ve form verileri için bazı css özellikleri kullandım onu da bu açıklamanın hemen altına ekleyeceğim. Kodu incelediğimizde textbox, label ve hataları gösterebilmek için zorunlu alanlar kullanıldığını görüyorsunuz.
**style.css**
{% highlight css %}
#product-add { width:600px; margin:0; padding:20px;position:relative; color:white }
#product-add .label { float:left; width:150px; margin:4px 10px; color:#fff }
#product-add .text { width:200px; margin-bottom:10px }
#product-add .textarea { width:300px; display:block }
#product-add .img { width:150px; height:150px; display:block; }
#product-add .btn { float:right; margin:10px 0 0 0; width:150px; height:35px; background:#DBA499; color:#222222; font:18px Ubuntu, Sans-Serif; border-radius:4px; -webkit-border-radius:4px; -moz-border-radius:4px; border:0}
#product-add .btn:hover { border:1px dashed #DBA499; background:#000; color:#DBA499; }
{% endhighlight %}

Şimdi kod tarafına geçip entity framework ile veri tabanımıza **insert** işlemi için kodlarımızı yazalım;
**Default.aspx.cs**
{% highlight csharp %}
protected void Page_Load(object sender, EventArgs e)
{
  if (!Page.IsPostBack)
  {
    MarkaDoldur();
  }
}

protected void btnUrunEkle_Click(object sender, EventArgs e)
{
  var context = new MyDbEntities();
  Products urun = new Products();
  urun.ProductName = txtUrunAdi.Text;
  urun.Color = txtRenk.Text;
  urun.Price = decimal.Parse(txtFiyat.Text);
  urun.Stock = short.Parse(txtStok.Text);
  urun.BrandsId = ddlMarka.SelectedIndex;
  if (fuResimYukle.HasFile)
  {
    fuResimYukle.SaveAs(MapPath("/images/products/" + fuResimYukle.FileName));
    lblDurum.Text = "<p style=" + "color:green" + ">Yeni ürün başarıyla eklendi.<p>";
  }
  else
  {
    lblDurum.Text = "<p style=" + "color:red" + ">Ouups! Bir hata meydana geldi, tekrar denermisin.<p>";
  }
  urun.ProductImgFilePath = "/images/products/" + fuResimYukle.FileName;
  urun.TechnicalDetails = txtTeknik.Text;
  context.AddToProducts(urun);
  context.SaveChanges();

  txtFiyat.Text = "";
  txtRenk.Text = "";
  txtStok.Text = "";
  txtTeknik.Text = "";
  txtUrunAdi.Text = "";
}
public void MarkaDoldur()
{
  var context = new MyDbEntities();
  List<Brands> marka = context.Brands.ToList();
  ddlMarka.DataSource = marka;
  ddlMarka.DataValueField = "BrandsId";
  ddlMarka.DataTextField = "BrandsName";
  ddlMarka.DataBind();
}
{% endhighlight %}

Satır numaralarına göre anlatırsak sanırım daha anlaşılır olacak, bu yüzden en alttan üste doğru gitmek daha etkili olacaktır.
37-40. satırlar arasında markalar isminde oluşturduğum kategorinin verilerini veri tabanından çekerek entity framework ile **dropdown** listeme dolduruyorum.
9-36. satırlar arasındaki kod bloğum ise sayfada girilen verilerin butona tıklandığındaki işlev sürecini anlatmakta, kısaca bahsedelim;
11. satırda entity modelimizi tanımlayıp,
12. satırda ürün tablomuza verilerimizi insert yapacağımız için bir tanımlama yapmamız gerekiyor, burada onu tanımlıyoruz,
13-17. satırlar arasında sırasıyla ürün tablomuzdaki kolonlarımıza defaul.aspx sayfamızda önceden eklemiş olduğumuz textbox verilerini eşitliyoruz. **SQL** tarafında fiyat kolonu money ve stok kolonu int olarak tanımladığımız için bu verilere insert yapabilmemiz için textbox verilerini **parse** etmemiz gerekiyor. Marka seçimini dropdown ile yaptığımız için ürün tablomuzda bunun id'si tutulmakta, bu yüzden seçilen markanın id'sini de buraya eşitliyoruz.
18-26. satırlar arası yüklenmek üzere ürüne seçilen resmin kaydedileceği dizini belirtiyoruz.
27. satırda ise bu resmin veri tabanında daha sonra farklı işlemler için(ör. ürün gösterimi vs.) dizin yolunu tutmamız gerekecek, burada bunu belirtiyoruz.
29. satırda girilen bilgilerin veri tabanında ürün tablosuna eklenmesini,
30. satırda ise entity modelimiz ile bunun kayıt işlemini gerçekleştiriyoruz.
31-35. satırlar arasında da form verilerimizi resetliyoruz.
1-7. satırlar arasında markaların dropdown listesini sayfa açılışında doldurması için sayfamızın yükleme esnasında bu işlemi yapmasını sağlıyoruz.

Bir sonraki entity framework ile update kullanımı yazımızda görüşmek üzere, kalın sağlıcakla..
