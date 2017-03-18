---
layout: post
title: Google Maps JavaScript API v3 Kullanımı (Bootstrap)
date: 2014-08-26 22:28:33 +02:00
abstract: Web projelerimizde, özellikle de kurumsal web siteleri yaparken vazgeçilmez olan iletişim sayfasının olmazsa olmazı haline gelen Google haritalarının kullanımının daha da önem kazandığı günümüzde bu haritaları istediğimiz gibi şekillendirebilmemizin...
published: true
status: publish
categories:
- ASP.NET
tags:
- asp.net maps kullanımı
- bootstrap ile maps kullanımı
- bootstrap kullanımı
- bootstrap maps
- bootstrap modal sorunu
- google maps
- javascript maps
- maps api kullanımı
- popup harita
- popup maps
- popup maps kullanımı
- v3 api
meta:
  dsq_thread_id: '2998855120'
  views: '6915'
---

Web projelerimizde, özellikle de kurumsal web siteleri yaparken vazgeçilmez olan iletişim sayfasının olmazsa olmazı haline gelen Google haritalarının kullanımının daha da önem kazandığı günümüzde bu haritaları istediğimiz gibi şekillendirebilmemizin yanı sıra örneğin yer bildirim ikonunu kendi istediğimiz bir ikon ile de kullanabiliyoruz. Sonunda nokta koyabildim :)

Bende **Google Maps API V3**'ü bir projemde bootstrap'in modal pop-up güzelliğinden faydalanarak kullandım, bir Türkçe kaynak olarak benim de katkım olsun istedim.

Fazla uzatmadan önce uygulamada kullandığım API kodumuzu ve detaylarını daha sonra pop-up modal kod bölümünü ve görselini paylaşarak anlaşılır hâlde daha kolay incelemeye çalışacağım.

**Bootstrap**'in modal pop-up özelliği için [buradan detaylı bilgiye ulaşabilir](http://getbootstrap.com/javascript/#modals), yapısını ve örnekleri inceleyip sizde projelerinizde kullanabilirsiniz. Geçelim uygulamamıza; ilk önce tahmin ettiğiniz gibi Maps API V3 kütüphanemizi <head></head> taglari arasına eklememiz gerekiyor.

<!-- Google Maps API -->
<script type="text/javascript" src="http://maps.googleapis.com/maps/api/js?sensor=false"></script>
Daha sonra maps için kullanacağımız javascript kodumuzu yazıyoruz;
{% highlight html %}
<script type="text/javascript">
  function initialize() {
    var konum = new google.maps.LatLng(41.0621052,28.984069);
    var mapOptions = {
      zoom: 14,
      center: konum,
      mapTypeId: google.maps.MapTypeId.ROADMAP,
    };
    var map = new google.maps.Map(document.getElementById('map-canvas'), mapOptions);
    var contentString = '<div id="content">'+
        '<div id="siteNotice">'+
        '</div>'+
        '<h1 style="font-size:16px; font-family:Arial, Tahoma; color:#777;">Mert İletişim</h1>'+
        '<div id="bodyContent">'+
          '<p style="font-size:12px; font-family:Arial, Tahoma; color:#777;">Pınar Mahallesi 1261. Sokak No:1 Şişli / İstanbul</p>'+
        '</div>'+
      '</div>';
    var infowindow = new google.maps.InfoWindow({
      content: contentString
    });
    var marker = new google.maps.Marker({
      position: konum,
      map: map,
      icon:'images/marker.png',
      title: 'Uykusuz Adam'
    });
    $("#kroki").on("shown.bs.modal", function (e) {
      google.maps.event.trigger(map, "resize");
      map.setCenter(konum); // Set here center map coordinates
    });
    google.maps.event.addListener(marker, 'click', function(){
      infowindow.open(map,marker);
    });
  }
  google.maps.event.addDomListener(window, 'load', initialize);
</script>
{% endhighlight %}
Kodumuzu açıklarsak konum adında bir değişken oluşturduk ve konumumuza enlem ve boylam tanımladık. google.maps.LatLng(**enlem**,**boylam**); degerlerimizi burada belirtiyoruz. Peki bu değerleri nasıl alacağım derseniz, yeni bir sekmede maps.google.com açarak haritanızın sitenizde görüntülenecek konumunu ayarladığınızda adres satırındaki değeri, yani; https://www.google.com/maps/@**41.0621052**,**28.984069**,14z örnekteki gibi @ işaretinden sonra gelen enlem ve boylam değerlerini alabilirsiniz. Burada enlem ve boylamdan sonra gelen 14z değeri haritanın zoom oranıdır.
<pre>
var konum = new google.maps.LatLng(**41.0621052,28.984069**);
</pre>
Daha sonra harita ayarlarımızda zoom değerini, harita görüntülendiğinde ortalanacak değeri ve harita tipimizi belirliyoruz. Harita tiplerini de 4 şekilde belirleyebiliyoruz;

- **MapTypeId.ROADMAP** (Default yol haritası görünümü)
- **MapTypeId.SATELLITE** (Google Earth uydu görünümü)
- **MapTypeId.HYBRID** (Normal ve uydu birleşimi görünüm)
- **MapTypeId.TERRAIN** (Arazi bilgilere dayalı fiziksel görünüm)</span>

<pre>
var mapOptions = {
  zoom: 14,
  center: konum,
  mapTypeId: google.maps.MapTypeId.ROADMAP,
};
</pre>
Sayfamızda kullanacağımız haritamızı da aşağıdaki şekilde yeni google.maps.Map sınıfı oluşturarak html tarafta göstereceğimiz div'in id'sini seçtiriyoruz ve ayarlarımız da bu şekilde diyoruz.
<pre>
var map = new google.maps.Map(document.getElementById('map-canvas'), mapOptions);
</pre>

**contentString** ile info penceremizde kullanacağımız yazı veya içerik ile alakalı bilgilerimizi yine html düzenleme içerisine alarak belirleyip sonrasında bu string değerini infoWindow sınıfı tanımlayarak **content** olarak bildiriyoruz.

<pre>
var infowindow = new google.maps.InfoWindow({
  content: contentString
});
</pre>
Bu işlemlerden sonra konumu bildiren ikonumuzu default marker kullanarak dilerseniz de kendi hazırlamış olduğunuz ikonu kullanarak **marker** sınıfımızın ayarlarını belirliyoruz.

* **position :** İkonun konumunu
* **map :** Bildirim kullanılacak harita
* **icon :** ikonumuzun dosya yolu (bu degeri belirtmezseniz default ikon gelecektir)
* **title :** ikon üzerine gelindiğinde çıkacak pencerenin başlığı
değeri ile ikonun görseli için buradan dosya yolunu belirterek kullanabiliyoruz
<pre>
var marker = new google.maps.Marker({
  position: konum,
  map: map,
  icon:'images/marker.png',
  title: 'Uykusuz Adam'
});
</pre>
İkonumuza tıklandığında bilgi penceremizin açılmasını tetikleyecek olay tetikleyicimizi;
<pre>
google.maps.event.addListener(marker, 'click', function(){
  infowindow.open(map,marker);
});
</pre>
Sayfamız yüklenediğinde hazır olması için haritamızı oluşturan tetikleyicimiz;
<pre>
google.maps.event.addDomListener(window, 'load', initialize);
</pre>
Buradan sonra bootstrap'in pop-up modal kod bölümünün içeriğini yazıyoruz.
{% highlight html %}
<div class="modal fade" id="kroki" tabindex="-1" role="dialog" aria-labelledby="myModalLabel" aria-hidden="true">
 <div class="modal-dialog">
  <div class="modal-content">
    <div class="modal-header">
      <button type="button" class="close" data-dismiss="modal" aria-hidden="true">&amp;times;</button>
      <h4 class="modal-title" id="myModalLabel">Krokimiz !</h4>
    </div>
    <div class="modal-body">
      <div id="map-canvas" style="width:560px; height:250px"></div>
        <script>
        </script>
      </div>
    </div>
  </div>
</div>

<a href="#kroki" data-toggle="modal" title="Krokimiz">KROKİ</a>
{% endhighlight %}
<img src="{{ site.baseurl }}/assets/hatali-maps-uykusuzadam.png" alt="" width="619" height="364" />
Oups! Haritamızda problem mi var, neden böyle gözüktü ? Yaşadığımız bu problem haritamız normalde sayfamız için ilk seferde oluşturuluyor fakat biz pop-up modal sayfamızı çağırdığımızda konumunu güncelleyemediği için resize özelliğini kaybediyor, çözüme şu şekilde ulaşabilirsiniz;
<pre>
$("#kroki").on("shown.bs.modal", function (e) {
  google.maps.event.trigger(map, "resize");
  map.setCenter(konum); // Set here center map coordinates
});
</pre>
<img src="{{ site.baseurl }}/assets/maps-api-uykusuzadam.png" alt="maps-api-uykusuzadam" width="627" height="376" />
Bu şekilde hatamızı da gidererek sonuç alabildik, [stackoverflow.com](http://stackoverflow.com/questions/16532411/google-map-not-filling-modal-popup) iyi ki var :)

https://developers.google.com/maps/documentation/javascript/tutorial sayfasını inceleyerek daha fazla özellik katarak sizde haritalarınızı kolayca şekillendirebilir, rahatlıkla etkili sonuçlar alabilirsiniz. Umarım faydalı olmuştur.
