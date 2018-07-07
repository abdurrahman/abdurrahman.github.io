---
layout: post
title: RESTful API Tasarım Kuralları - Best Practices
date: 2018-07-07 22:30
abstract: REST kavramı, API yapısını mantıksal kavramlara ayırmaktır. Kaynaklarla çalışmak için GET, DELETE, POST, PUT gibi HTTP yöntemleri kullanılır.
published: true
status: publish
tags:
- Restful
- Software Development
- Api Development
---

API, geliştiricilerin verilerlerle etkileşime girdiği bir arayüzdür. Bu yüzden iyi bir API, geliştiriciler açısından anlaşılır, kullanımı kolay ve rahatça entegre olabilmeleri için iyi tasarlanmalıdır.

REST kavramı, API yapısını mantıksal kaynaklara ayırmaktır. Kaynaklarla çalışmak için GET, DELETE, POST, PUT gibi HTTP yöntemleri kullanır. Daha iyi bir REST uygulama yazmak istiyorsak [pragmatik](https://www.bitnative.com/2012/08/26/how-restful-is-your-api/) ve mantıklı olan kullanımları tercih etmeli, eski alışkanlıklarımızı bırakmalıyız.

Resmi olmasada topluluk tarafından kabul görmüş ve yaygın olarak kullanılan bazı semantik ve teknik kuralları inceleyelim.

## Çoğul isimlendirmeler
Endpoint isimlendirmeleri tercihe dayalı olsa da çoğul kullanım yaygın olarak kullanılmaktadır. Özellikle bir koleksiyon için GET methodunu kullandığınızda bu daha anlamlıdır. Bu sebeple kaynak isimlerini çoğul olarak kullanın ve basit tutun.

    GET /products
    GET /authors

Anlamsız kullanıma örnek

    GET /product (birden fazla ürün dönmesi gibi)
    GET /author (birden fazla yazar dönmesi gibi)

## İsimlendirme yeterli, fiiller gereksiz
Endpoint tanımlarında, kolay anlaşılabilmesi için isim kullanın, fiil belirten ifadelerden kaçının. Aşağıdaki gibi bir kullanıma giderseniz her bir durum için method tanımlamak zorundasınız. Method isimlendirme illetinden bahsetmiyorum bile :)

    /addNewProduct
    /deleteProduct
    /getAllProduct
    /findAuthorById
    /updateAuthor

Bunun yerine, kaynakla ne yapacağınızı belirtmek için (Rest’i, Rest yapan) HTTP yöntemlerini kullanın.

    POST /Products
    DELETE /Products/1
    GET /Products
    GET /Authors/12
    PUT /Authors/12


## İlişkiler için alt kaynak
Eğer veriniz başka bir veri ile ilişkiliyse alt kaynak (sub-resources) kullanın.

    GET /Categories/2/Products  -- 2 nolu kategorinin tüm ürünlerini döndürür
    GET /Books/32/Authors/2   -- 32 nolu kitabın 2 nolu yazarını döndürür

## Arama, sıralama, filtreleme ve sayfalama
Temel kaynak URL’lerini olabildiğince az tutmak en iyisidir. Koleksiyonlar için başlıkta ifade edilen bu eylemlerin kullanılacağı yeni URL’ler yapmanız gerekmiyor. GET methodu ile koleksiyona sorgu parametreleri eklenerek talep edilen eylem ile veriyi sınırlandırabilirsiniz.

Nasıl kullanıldığına dair örneklerle açıklamaya çalışalım;

 - **Sıralama**: Müşteri, ürünlerin listesini fiyat sırasına göre almak istiyorsa, GET /products endpoint’i, sıralama parametresini kabul etmelidir. Örnek olarak ürünleri artan fiyata göre sıralamak istersek;
**GET /Products?Sort=price_asc**
 - **Filtreleme**: Veri kümesini filtrelemek için, sorgu parametreleri ile çeşitli seçenekleri iletebiliriz. Örnek olarak kitap listesini politika ve Türkiye’de bulunanlar olarak filtreleyelim;
**GET /Books?Category=Politic&Location=Turkey** 
 - **Arama**: Temel filtrelerin yeterli olmadığı durumlarda, tam metin arama seçeneği sunabilirsiniz. Örnek olarak ürün listesinde Digital Marketing ifadesi olan ürünleri getirelim;
 **GET /Products?Search=Digital Marketing**
 - **Sayfalama**: Veri kümesi çok büyük olduğunda, yapılan sorgunun yavaş çalışması olasıdır. Sayfalama ile veri kümesini daha küçük parçalara bölerek performanslı hâle getirebilir ve yanıtı kolay yönetmiş olursunuz. Örnek olarak kitap listesinin tamamını(burada binlerce ve ya daha fazla kayıttan söz edebiliriz) çekmek yerine ihtiyacımız olan 3. sayfayı getirmek istersek;
 **GET /Books?page=3** 
 
## HTTP durum kodları
İstemci, bir endpoint aracılığıyla sunucuya bir istekte bulunduğunda, isteğin başarısız ve ya yanlış olup olmadığını bilmesi gerekir. Yapılan isteğe göre sunucu, her zaman doğru durum kodunu döndürmelidir.

> 2xx (Başarılı durumlar)
> 3xx (Yönlendirme durumları)
> 4xx (İstemci kaynaklı hatalar)
> 5xx (Sunucu kaynaklı hatalar)

[HTTP durum kodlarının](http://en.wikipedia.org/wiki/List_of_HTTP_status_codes) tamamını kullanmanız, API kullanıcınız için kafa karıştırıcı olabilir. Bunu olabildiğince basit tutup yaygın olanları(200, 201, 302, 404, 400, 500 gibi) kullanın.

## Dokümantasyon
Daha az soru soran geliştirici kitlesi hedefliyorsanız(ki aksini talep eden olmaz sanırım) API dokümantasyonu mutlaka olmalı ve iyi belgelenmelidir. Zamanınızı, geliştiriciler için bir şeyler inşa etmek, bunu nasıl yapacaklarını ve ya API’ı nasıl kullanacaklarını onlara anlatmak için ziyan etmeyin.

Dokümantasyon oluşturmak için swagger vb. kaynaklardan yardım alabilirsiniz. Kısa bir aramayla çok rahat open source kaynaklara da erişebilirsiniz.

## Versiyonlama
İşiniz, süreçleriniz ve ya kullanıcıların istekleri değişebilir. Değişim kaçınılmaz ! Bu yüzden mevcut ürün veya hizmetlerin bozulmasına engel olmak için API sürümünü zorunlu hale getirin ve versiyonlanmamış bir API’yi yayınlamayın. Böylelikle kullanıcılar eski sürümü tüketmeye(consume) devam ederken siz yeni versiyonu geliştirebilirsiniz.

API versiyonlama birkaç farklı yol ile yapılabilmektedir (farklı **Headers** kabul etmek, farklı bir **URL** kullanmak gibi). API kullanıcıları, versiyon değişikliğinde URL güncellemesi yapmak zorunda kalsa da basitliği nedeniyle URL versiyonlama çok popülerdir.

Versiyonlama için basit bir sıra numarası kullanın ve 2.5 gibi noktalama işaretlerinden kaçının.

> /v1/categories

## Özet
Yaygın olarak RESTful API geliştirmede kullanılan kalıplaşmış yöntemleri ifade etmeye çalıştım. Daha fazla detay için [Philipp Hauer'in makalesine](https://blog.philipphauer.de/restful-api-design-best-practices/) göz atabilirsiniz.

Yukarıda belirtilen başlıklar hakkındaki görüşlerinizi bilmek isterim. Lütfen bir yorum bırakarak bana bildirin.
