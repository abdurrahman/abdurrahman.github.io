---
layout: post
title: ASP.NET MVC'de MySQL ile Code First Kullanımı
date: 2015-06-21 17:11:48 +02:00
abstract: Fluent api'yi basitçe tanımlamak gerekirse (code first yaklaşımı ile) veri tabanı sınıflarını ve ilişkilerini yapılandırabilmenin bir yoludur. Veri tabanları bağlamında bir ilişki, bir tablodaki primary key alanının diğer tablodaki foreign key alanına karşılık gelmesi durumudur...
---

EntityFramework yapısı bizlere **Code First, Model First** ve **Database First** olmak üzere 3 farklı yaklaşım sunar. Bu makalemizde Code First yaklaşımının MySQL ile kullanımını inceliyor olacağız. Öncelikle bilgisayarımızda MySQL Server ve MySQL connector'un kurulu olması gerekiyor. Gerekli linkler aşağıdaki gibidir;

- [MySQL Community Server 5.6.25](http://dev.mysql.com/downloads/mysql/)
- [MySQL Connector/Net 6.9.6](https://dev.mysql.com/downloads/connector/net/6.9.html)
- [MySQL Workbench 6.3.4](https://dev.mysql.com/downloads/workbench/)

<p class="message">Veritabanı uygulaması olarak bilgisayarınızda Wamp Server ve benzeri uygulama kurulu olabilir, indirmeniz gerekmiyor.<p>

Connector kurulumunu yaptıktan sonra Visual Studio ile yeni bir MVC projesi açıyoruz, sonrasında Code First için EntityFramwork'u, MySQL içinde data ve entity kütüphanelerini yüklüyoruz. Bunun için proje içerisinde Nuget üzerinden arama yapabilirsiniz ya da Nuget Package Manager konsolu üzerinden sırasıyla aşağıdaki şekilde yazıp da yükleyebilirsiniz.

<blockquote>
Install-Package EntityFramework<br />
Install-Package MySql.Data<br />
Install-Package MySql.Data.Entity<br />
</blockquote>

Bu yazım ile kütüphanelerin son versiyonları kullanıyor olacağız.
Gereksinimlerimizi tamamladıktan sonra Model yapımızı yani veri tabanında oluşturacağımız tabloların kod tarafında yazımını gerçekleştirelim. Ürünler ve Kategoriler şeklinde 2 tablo oluşturalım.

**Product.cs**
{% highlight csharp %}
using System;
using System.Collections.Generic;
using System.Linq;
using System.Web;

namespace CodeFirstApplication.Models
{
    public class Product
    {
        public int ProductId { get; set; }
        public int ProductCode { get; set; }
        public int CategoryId { get; set; }
        public string Name { get; set; }
        public string Color { get; set; }
        public decimal Price { get; set; }
        public int StockCount { get; set; }
        public string ImgPath { get; set; }
    }
}
{% endhighlight %}

**Category.cs**
{% highlight csharp %}
using System;
using System.Collections.Generic;
using System.Linq;
using System.Web;

namespace CodeFirstApplication.Models
{
    public class Category
    {
        public int CategoryId { get; set; }
        public string Name { get; set; }
    }
}
{% endhighlight %}
Modellerimizi oluşturduktan sonra bu tablolarımızın oluşturulabilmesi için EntityFramework'un base type sınıfı olan **DbContext** sınıfını kullandığımız Context yapımızı oluşturuyoruz;
**CodeFirstContext.cs**
{% highlight csharp %}
using System;
using System.Collections.Generic;
using System.Data.Entity;
using System.Linq;
using System.Web;

namespace CodeFirstApplication.Models
{
    public class CodeFirstContext : DbContext
    {
        public DbSet<Product> Products { get; set; }
        public DbSet<Category> Categories { get; set; }
    }
}
{% endhighlight %}
<blockquote>Context'i oluşturacağımız bu dosyada tablo isimlerini verirken EntityFramework siz yazmasanız da <strong>"s"</strong> ve ya <strong>"ies"</strong> eklemekte, burada düzenli olması açısıdan ben kendim yazdım.</blockquote>

Model ve context yapımızı oluşturduğumuza göre **Web.config** dosyamızda veri tabanı bağlantı cümleciğimizi güncelliyoruz. Güncelliyoruz dememin sebebi **EntityFramework** dll dosyamızı nuget üzerinden yüklediğimiz sırada buraya örnek connection string'i ve microsoft yine kendi ürünü olan Sql Server'in provider sınıfını otomatik olarak eklemekte, biz kendi bilgilerimiz ile güncellerken **name** kısmına context'e verdiğimiz adın aynısını veriyoruz, **providerName**'in de aşağıdaki gibi olduğuna dikkat edelim. Oluşacak veri tabanı adını da buradan belirtebiliyoruz, ben CodeFirstDB adını verdim siz farklı isim kullanabilirsiniz.
{% highlight xml %}
  <connectionStrings>
    <add name="CodeFirstContext" connectionString="Data Source=localhost;port=3306; Initial Catalog=CodeFirstDB;uid=root; pwd=1234" providerName="MySql.Data.MySqlClient" />
  </connectionStrings>
{% endhighlight %}
Bağlantı ayarlarımızıda güncelledikten sonra veri tabanımızı kod üzerinden oluşturacağımız CodeFirst yaklaşımının **migration** özelliğini etkinleştirip yeni migration ekleyeceğiz. Bunun için Package Manager Console'u kullanıyoruz.

1- İlk olarak **enable-migrations** komutu ile migration'ımızı etkinlieştiriyoruz;
<img class="alignnone size-full wp-image-734" src="{{ site.baseurl }}/assets/enable-migrations-uykusuzadam.png" alt="enable-migrations-uykusuzadam" width="457" height="44" />
Bu adımı gerçekleştirirken projemizde oluşan **Migration** adındaki klasör içerisinde **Configuration.cs** dosyasını aşağıdaki gibi düzenliyoruz. SqlGenerator methodunun mysql ile çalışacağını belirtmemiz gerekiyor.
{% highlight csharp %}
public Configuration()
{
      AutomaticMigrationsEnabled = true;
      SetSqlGenerator("MySql.Data.MySqlClient", new MySql.Data.Entity.MySqlMigrationSqlGenerator());
}
{% endhighlight %}

2- Daha sonra **add-migration** komutu ile migration ekleyip bir isim veriyoruz. Proje adı ile aynı olmamasına dikkat etmemiz gerekiyor.
<img class="alignnone size-full wp-image-738" src="{{ site.baseurl }}/assets/add-migration-uykusuzadam.png" alt="add-migration-uykusuzadam" width="698" height="137" />

3- Son adım olarak **update-database** komutu ile veri tabanımızı oluşturuyoruz. CodeFirst ile ilk defa çalışıyorsanız bundan sonraki tablo ekleme, mevcut tabloya yeni alan ekleme ya da mevcut alanın özelliğini değiştirme gibi işlemlerden sonra bu adım üzerinden devam ediyoruz. Diğer ilk 2 adım migration tanımlama işlemleri olduğu için mevcut tanımlı migration üzerinden veri tabanı değişikliklerimizi güncellemek yeterli oluyor. Herhangi bir şekilde manuel olarak veri tabanı üzerinden kolon silme yada kolon özelliği değiştirme yaptığınız halde CodeFirst bunu anlayıp çalışmayacaktır. Adı üzerinde herşeyi kod ile yapmanızı istiyor. :)

Yukarıdaki adımlar sonrasında update-database yaptıysanız aşağıdaki gibi hata almanız muhtemeldir.
<blockquote><span style="color: #ff0000;">Specified key was too long; max key length is 767 bytes</span></blockquote>

Mysql kod tarafında oluşturulduğundan default olarak InnoDB engine yapısını kullanır, primary key olarak belirttiğimiz alanlar için bu hatayı döndürür. Buna çözüm olarak Context modelimizi aşağıdaki gibi tekrar güncelliyoruz. Daha önce yukarıda SqlGenerator için yaptığımız ayarın bir benzerini burada yapıyoruz.
**CodeFirstContext.cs**
{% highlight csharp %}
using System;
using System.Collections.Generic;
using System.Data.Entity;
using System.Linq;
using System.Web;

namespace CodeFirstApplication.Models
{
    [DbConfigurationType(typeof(MySql.Data.Entity.MySqlEFConfiguration))]
    public class CodeFirstContext : DbContext
    {
        static CodeFirstContext()
        {
            DbConfiguration.SetConfiguration(new MySql.Data.Entity.MySqlEFConfiguration());
        }
        public DbSet<Product> Products { get; set; }
        public DbSet<Category> Categories { get; set; }
    }
}
{% endhighlight %}
<img class="alignnone size-full wp-image-737" src="{{ site.baseurl }}/assets/update-database-uykusuzadam.png" alt="update-database-uykusuzadam" width="648" height="76" />
Son adımdan sonra aşağıdaki gibi veri tabanımızın oluştuğunu görebilirsiniz.
<img class="alignnone size-full wp-image-739" src="{{ site.baseurl }}/assets/codefirst-db-uykusuzadam.png" alt="codefirst-db-uykusuzadam" width="542" height="305" />
CodeFirst yaklaşımı diğer yaklaşımlara göre daha çok kullandığım bir yaklaşım. Veri tabanına sadece "veri eklenmiş mi ?" diye açıp bakmak ve geri kalan tüm işlemleri bir IDE üzerinden kod tarafında gerçekleştirmek daha kullanışlı ve daha hızlı kod yazmanızı sağlıyor.

Umarım faydalı olmuştur.

*Kaynaklar;*
* [ASP.NET: Setup a MVC5 website with MySQL, Entity Framework 6 Code-First and VS2013](http://www.ryadel.com/2014/10/20/asp-net-setup-mvc5-website-mysql-entity-framework-6-code-first-vs2013/)
* [6 Steps To Get Entity Framework 5 Working with MySql 5.5](http://www.nsilverbullet.net/2012/11/07/6-steps-to-get-entity-framework-5-working-with-mysql-5-5/)
* [Getting Started with Entity Framework 6 Code First using MVC 5](https://www.asp.net/mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application)
