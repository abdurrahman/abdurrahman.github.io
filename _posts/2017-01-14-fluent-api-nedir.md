---
layout: post
title: Fluent Api Nedir ?
date: 2017-01-14 00:16:53 +02:00
abstract: Fluent api'yi basitçe tanımlamak gerekirse (code first yaklaşımı ile) veri tabanı sınıflarını ve ilişkilerini yapılandırabilmenin bir yoludur. Veri tabanları bağlamında bir ilişki, bir tablodaki primary key alanının diğer tablodaki foreign key alanına karşılık gelmesi durumudur...
---

Fluent api'yi basitçe tanımlamak gerekirse (code first yaklaşımı ile) veri tabanı sınıflarını ve ilişkilerini yapılandırabilmenin bir yoludur. Veri tabanları bağlamında bir ilişki, bir tablodaki **primary key** alanının diğer tablodaki **foreign key** alanına karşılık gelmesi durumudur. İlişkiler (relations), ilişkisel veri tabanların farklı tablolara bölünmesini ve depolanmasını sağlarken, birbirinden farklı veri öğelerini de birbirine bağlar. Böylece, RDBMS yani ilişkisel veri tabanı yönetimi sistemlerinin de tanımını yapmış olduk. Örneğin bir müşteri ve onun siparişleri hakkında bilgi depolamak istersek, biri müşteri (Customer) diğeri sipariş (Order) için iki tablo oluşturmamız gerekir. Bir müşterinin birden fazla siparişi olabileceği için **one-to-many** yani bire çok ilişki kurduğumuzda kolayca siparişlerine ulaşabiliyor olacağız.

Code First yaklaşımında domain sınıflarına yani database modellerimize yapılandırma eklemenin iki yolu vardır. Biri DataAnnotations diye tabir ettiğimiz property'lere attribute olarak eklemek, diğeri ise (yazımızın da konusu olan) kodda kural olarak tanımlayabilmemizi sağlayan Code First yaklaşımının Fluent API'sini kullanarak.. Farkındayım fazla teknik bir cümle kurdum ama bazı noktalarda gerçekten terimlerin tam olarak Türkçe karşılığı olmayabiliyor. :(

Bu makalede örnek olması için önceden yazmış olduğum bir projem üzerinden Fluent API'yi anlatmaya çalışacağım. Github üzerinden inceleyebilmeniz için yazının sonunda linkini paylaştım. Walltage.Domain adında bir class library projesi oluşturuyoruz. (Siz projenize başka isim verebilirsiniz, buradan kodları kopyalamanız durumunda namespace'lere dikkat etmeniz gerekiyor) Oluşturduğumuz bu projeye içinde entity'lerimizin yer alacağı Entities adında bir klasör ekliyoruz.

İlk olarak **BaseEntity** adında her tablonun ihtiyacı olacağı (kalıtılabileceği) bir sınıf ekliyoruz.
{% highlight csharp %}
namespace Walltage.Domain.Entities
{
    public abstract class BaseEntity
    {
        public int Id { get; set; }
    }
}
{% endhighlight %}
Bazı tablolarımıza sadece id alanı yeterli olmayabilir, oluşturma ya da düzenleme tarihi gibi ortak kullanılabilecek alanlara da ihtiyacımız olabilir. Bunun için **AuditableEntity** adında base bir sınıf daha oluşturuyoruz. Fazla detaylı oldu sanki ama olsun fazla bilginin zararı olmaz. ;)
{% highlight csharp %}
namespace Walltage.Domain.Entities
{
    public class AuditableEntity : BaseEntity
    {
        protected AuditableEntity()
        {
            ModifiedDate = DateTime.Now;
        }
        [MaxLength(256)]
        public string AddedBy { get; set; }
        public DateTime AddedDate { get; set; }
        public DateTime ModifiedDate { get; set; }
    }
}
{% endhighlight %}
Şimdi Entities klasörü altına **User** ve **UserRole** entity'lerimizi ekliyoruz.
{% highlight csharp %}
namespace Walltage.Domain.Entities
{
    public class User : AuditableEntity
    {
        public string Email { get; set; }
        public string Username { get; set; }
        public string Password { get; set; }
        public DateTime LastActivity { get; set; }
        public string IpAddress { get; set; }
        public int UserRoleId { get; set; }
        // ForeignKey attribute olarak UserRoleId alanını
        // UserRole tablosuna bağlamak adına tanımladık
        [ForeignKey("UserRoleId")]
        public virtual UserRole UserRole { get; set; }
}
{% endhighlight %}
Yukarıdaki örnekte Foreign Key ilişkisini DataAnnotations attribute'u (niteliği diye çevirebiliriz) olarak tanımladık ve key olarak **UserRoleId** alanının kullanılacağını parantez içinde belirttik. Buraya şimdilik çok takılmayalım, daha sonra User entity sınıfımızı aşağıda DataAnnotation yöntemiyle tekrar ele alacağız.
**UserRole.cs**
{% highlight csharp %}
namespace Walltage.Domain.Entities
{
    public class UserRole : BaseEntity
    {
        public string Name { get; set; }
        // Bir yetki birden fazla kullanıcıda olabilir.
        // One-to-Many Relation
        public virtual ICollection<User> UserList { get; set; }
    }
}
{% endhighlight %}
Görmüş olduğunuz gibi her iki tablonunda birbirleri arasında kolayca gezinebilmesi için ilişkileri mevcut. Şimdi Fluent Api ile bu tablolarımızın ilk defa oluşturulurken dikkate alınması gereken yapılandırmasını tanımlayabiliriz. Domain katmanımıza **Mappings** adında klasörümüzü ekliyoruz. İlk olarak yine ortak kullanılacak ve içinde Id alanımızın da olduğu bir BaseEntityMap sınıfı oluşturuyoruz. Bu yapılandırmaları context'imizin oluştuğu sırada OnModelCreate methodunu override edebileceğimiz **EntityTypeConfiguration** sınıfından yararlanıyoruz.
**BaseEntityMap.cs**
{% highlight csharp %}
namespace Walltage.Domain.Mappings
{
    public abstract class BaseEntityMap<TEntity> : EntityTypeConfiguration<TEntity> where TEntity : BaseEntity
    {
        protected BaseEntityMap()
        {
            HasKey(t => t.Id);
            Property(t => t.Id).HasDatabaseGeneratedOption(DatabaseGeneratedOption.Identity);
        }
    }
}
{% endhighlight %}
HasKey, veri tabanımızda primary key olarak tanımladığımız alan ve TEntity'ye hangi entity'i verirsek o tabloya Id alanını primary key olarak atama yapacak. Diğer özelliğimiz **HasDatabaseGeneratedOption** ise veri tabanımızda id alanının nasıl oluşturulacağını belirtmemizi sağlar. Burada auto-increment şeklide id'lerimizi artmasını istediğimiz için **DatabaseGeneratedOption.Identity** seçeneğini kullanıyoruz.
**UserMap.cs**
{% highlight csharp %}
namespace Walltage.Domain.Mappings
{
    public class UserMap : BaseEntityMap<User>
    {
        public UserMap()
        {
            // fields
            Property(t => t.Email).IsRequired().HasMaxLength(245);
            Property(t => t.Username).IsRequired().HasMaxLength(50);
            Property(t => t.Password).IsRequired().HasMaxLength(50);
            Property(t => t.LastActivity).IsOptional();
            Property(t => t.IpAddress).IsOptional().HasMaxLength(45);
            // relation
            //HasRequired(t => t.UserRole).WithMany().HasForeignKey(t => t.UserRoleId).WillCascadeOnDelete(false);
            // table
            ToTable("User");
        }
    }
}
{% endhighlight %}
Yukarıdaki UserMap constructor'ı ile tabloda özellikleri eşleştirmek ve yapılandırmak için Fluent Api'yi kullandık, bu yapılandırmadaki ayarları birer birer görelim.
**HasKey() :** Tabloda Primary key ataması yapılandırmanızı sağlar.
**Property() :** Belirli bir alana tablodaki durumu için özellik atamak için kullanılır. Buradaki örnekte <span style="text-decoration: underline;">*IsRequired();*</span> ilgili alanın zorunlu oluğunu, <span style="text-decoration: underline;">*IsOptional();*</span> ilgili alanın zorunlu olmadığını, opsiyonel olduğunu, <span style="text-decoration: underline;">*HasMaxLength();*</span> ilgili alanın maksimum karakter sayısı parantez içinde belirtilmiştir.
**ToTable() :** İlgili entity'nin tablo adını yapılandırmanızı sağlar. Genelde entity adı ile aynı kullanılır.
**HasRequired() :** İlgili entity'nin gerekli olan ilişkisini yapılandırmanızı sağlar. Örnekte User tablosunda UserRole foreign key ilişkisinin **UserRoleId** alanı ile zorunlu olduğunu diğer bir deyişle bu alan olmadan kaydedilemeyeceğini belirttik. WillCascadeOnDelete() ise ilgili UserRole'un silinmesi durumunda kullanıcının silinmemesi yapılandırmasıdır(Sql'deki constraint).
**UserRoleMap.cs**
{% highlight csharp %}
namespace Walltage.Domain.Mappings
{
    public class UserRoleMap : BaseEntityMap<UserRole>
    {
        public UserRoleMap()
        {
            Property(t => t.Name).IsRequired().HasMaxLength(20);
            ToTable("UserRole");
        }
    }
}
{% endhighlight %}
Buraya kadar entity'lerimizi ve Fluent Api ile yapılandırmalarımızı tamamladık, artık context sınıfımızı oluşturabiliriz. Oluşturacağımız sınıf EntityFramework ile gelen DbContext sınıfından türetilmeli, böylelikle yukarıda da bahsettiğimiz gibi yapılandırmalarımızın da geçerli olabilmesi için OnModelCreating() methodunu override edebileceğiz.
**WalltageDbContext.cs**
{% highlight csharp %}
namespace Walltage.Domain
{
    public class WalltageDbContext : DbContext
    {
        public virtual DbSet<User> Users { get; set; }
        public virtual DbSet<UserRole> UserRoles { get; set; }
        protected override void OnModelCreating(DbModelBuilder modelBuilder)
        {
            modelBuilder.Conventions.Remove<PluralizingTableNameConvention>();
            modelBuilder.Configurations.Add(new UserMap());
            modelBuilder.Configurations.Add(new UserRoleMap());
            base.OnModelCreating(modelBuilder);
        }
    }
{% endhighlight %}
OnModelCreating() methodunda DbModelBuilder nesnesi ile yapılandırmalarımızı ekleyerek model oluştuğunda dikkate alınmasını sağlamış olduk. Structure görünümün ü merak ediyorsanız şu şekilde;
<img class="alignnone size-full wp-image-759" src="{{ site.baseurl }}/assets/domain_structure.png" alt="domain_structure" width="267" height="274" />
Yazıya son vermeden önce User entity'mizin DataAnnotation yöntemi ile yapılandırılmış haline bakalım.
{% highlight csharp %}
namespace Walltage.Domain.Entities
{
    public abstract class BaseEntity
    {
        [Key]
        [DatabaseGenerated(DatabaseGeneratedOption.Identity)]
        public int Id { get; set; }
    }
    public class User : BaseEntity
    {
        [Required]
        [MaxLength(245)]
        public string Email { get; set; }
        [Required]
        [MaxLength(50)]
        public string Username { get; set; }
        [Required]
        [MaxLength(50)]
        public string Password { get; set; }
        // ? ile nullable yani opsiyonel olduğunu belirttik
        // Nullable<DateTime> bu şekilde de yazılabilir.
        public DateTime? LastActivity { get; set; }
        [MaxLength(45)]
        public string IpAddress { get; set; }
        public int UserRoleId { get; set; }
        [ForeignKey("UserRoleId")]
        public virtual UserRole UserRole { get; set; }
    }
}
{% endhighlight %}
Bu şekilde kullanım diğer yönteme göre daha kolay gelebilir. Tamamen tercihinize bağlı fakat şu detayı belirtmeliyim ki yukarıda alan özelliklerinin yanı sıra sadece foreign key ilişkisini belirtebildik. Fluent Api ile foreign key ilişkisinin ne şekilde olacağını ve hatta constraint tanımını bile ekleyebiliyoruz. Her iki yönteminde kendine göre avantaj ve dezavantajları var. Bu konuyu yorumlarda tartışabiliriz. Yeni bir yazı da tekrar görüşmek üzere, kendinize iyi bakın ! :)
Örnek Proje Repo: https://github.com/abdurrahman/walltage

*Kaynaklar;*

* https://msdn.microsoft.com/tr-tr/data/jj591617
* https://msdn.microsoft.com/tr-tr/data/jj591620
* https://www.tutorialspoint.com/entity_framework/entity_framework_fluent_api.htm
* http://www.c-sharpcorner.com/UploadFile/3d39b4/relationship-in-entity-framework-using-code-first-approach-w/
