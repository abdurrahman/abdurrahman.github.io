---
layout: post
title: Değer tip vs. referans tip
date: 2010-03-14 23:58:52 +02:00
abstract: C# dilinde değişkenler Değer Tip(Value Type) ve Referans Tip(Reference Type) ikiye ayrılır, bunlardan özellikleriyle bahsedelim...
published: true
status: publish
categories:
- C#
tags:
- C# tipleri
- Value Type vs. Reference Type
meta:
  views: '781'
  dsq_needs_sync: '1'
  dsq_thread_id: '3133397806'
---

C# dilinde değişkenler **Değer Tip(Value Type)** ve **Referans Tip(Reference Type)** ikiye ayrılır, bunlardan özellikleriyle bahsedelim;

### Değer Tipleri:

- Verilerini doğrudan kendi üzerinde saklarlar.
- Her bir değişken, verisinin bir kopyasını taşır.
- Bir değişken üzerinde yapılan değişiklik diğerini etkilemez.
- Veri, belleğin stack bölgesinde tutulur.
- Tanımlamak veri tipi ve değişken adı yeterlidir.
- Null (boş) değer alamazlar.

### Referans Tipleri:

- Verilerinin adreslerini (referanslarını) saklarlar.
- İki referans değişkeni aynı veriyi işaret edebilir.
- İki değişkenin birbirine eşitlenmesi sonrası, bir değişken üzerinde yapılan değişiklik diğerini etkileyebilir.
- Veri belleğin Heap bölgesinde tutulurken; verinin heap bölgesindeki adresini tutan değişken stack bölgesinde tutulur.
- Değer tiplerine ek olarak “new” anahtar kelimesiyle oluştururlurlar.
- Null değer alabilirler. Referans değişkeninin ‘null’ değer alması, işaret edeceği bir nesnenin olmaması anlamına gelir.

Aşağıdaki şekilde C# tipleri ve alacağı maximum ve minimum değerleri verilmiştir. Toplam olarak 15 veri tipi vardır bunlardan 13’ü değer 2’si referans tiplidir.

*Değer Tipleri*

<table border="0" cellspacing="0" cellpadding="0" width="625">
<tbody>
<tr>
<td width="67" valign="bottom"><strong>C# Adı</strong></td>
<td width="118" valign="bottom"><strong>CTS Karşılığı</strong></td>
<td width="168" valign="bottom"><strong>Açıklama</strong></td>
<td width="272" valign="bottom"><strong>Max ve Min  aralık yada değeri</strong></td>
</tr>
<tr>
<td width="67" valign="bottom">sbyte</td>
<td width="118" valign="bottom">System.Byte</td>
<td width="168" valign="bottom">8 bit işaretli  tamsayı</td>
<td width="272" valign="bottom">-128 : 127</td>
</tr>
<tr>
<td width="67" valign="bottom">short</td>
<td width="118" valign="bottom">System.Int16</td>
<td width="168" valign="bottom">16 bit  işaretli tamsayı</td>
<td width="272" valign="bottom">-32.768 :  32.767</td>
</tr>
<tr>
<td width="67" valign="bottom">int</td>
<td width="118" valign="bottom">System.Int32</td>
<td width="168" valign="bottom">32 bit  işaretli tamsayı</td>
<td width="272" valign="bottom">-2.147.483.648  : 2.147.483.647</td>
</tr>
<tr>
<td width="67" valign="bottom">long</td>
<td width="118" valign="bottom">System.Int64</td>
<td width="168" valign="bottom">64 bit  işaretli tamsayı</td>
<td width="272" valign="bottom">-9.223.372.036.854.775.808  : -9.223.372.036.854.775.807</td>
</tr>
<tr>
<td width="67" valign="bottom">byte</td>
<td width="118" valign="bottom">System.Byte</td>
<td width="168" valign="bottom">8 bit  işaretsiz tamsayı</td>
<td width="272" valign="bottom">0,177083333</td>
</tr>
<tr>
<td width="67" valign="bottom">ushort</td>
<td width="118" valign="bottom">System.UInt16</td>
<td width="168" valign="bottom">16 bit  işaretsiz tamsayı</td>
<td width="272" valign="bottom">0 : 65.535</td>
</tr>
<tr>
<td width="67" valign="bottom">uint</td>
<td width="118" valign="bottom">System.UInt32</td>
<td width="168" valign="bottom">32 bit  işaretsiz tamsayı</td>
<td width="272" valign="bottom">0 :  4.294.967.295</td>
</tr>
<tr>
<td width="67" valign="bottom">ulong</td>
<td width="118" valign="bottom">System.UInt64</td>
<td width="168" valign="bottom">64 bit  işaretsiz tamsayı</td>
<td width="272" valign="bottom">0 :  18.446.744.073.709.551.615</td>
</tr>
<tr>
<td width="67" valign="bottom">float</td>
<td width="118" valign="bottom">System.Single</td>
<td width="168" valign="bottom">32 bit tek  kayan sayı</td>
<td width="272" valign="bottom">+yada - 1,5*10<sup>-45</sup> : + ya da - 3,4*10<sup>38</sup></td>
</tr>
<tr>
<td width="67" valign="bottom">double</td>
<td width="118" valign="bottom">Sytem.Double</td>
<td width="168" valign="bottom">64 bit çift  kayan sayı</td>
<td width="272" valign="bottom">+yada - 5*10<sup>-324</sup> : + ya da - 1,7*10<sup>308</sup></td>
</tr>
<tr>
<td width="67" valign="bottom">decimal</td>
<td width="118" valign="bottom">System.Decimal</td>
<td width="168" valign="bottom">128 bit  ondalıklı sayı</td>
<td width="272" valign="bottom">+yada - 1,5*10<sup>-28</sup> : + ya da - 7,9*10<sup>28</sup></td>
</tr>
<tr>
<td width="67" valign="bottom">bool</td>
<td width="118" valign="bottom">System.Boolean</td>
<td width="168" valign="bottom"></td>
<td width="272" valign="bottom">true ya da  false</td>
</tr>
<tr>
<td width="67" valign="bottom">char</td>
<td width="118" valign="bottom">System.Char</td>
<td width="168" valign="bottom">Karakterleri  temsil eder</td>
<td width="272" valign="bottom">16 Unicode  karakterleri</td>
</tr>
</tbody>
</table>

*Referans Tipleri*

<table border="0" cellspacing="0" cellpadding="0" width="625">
<tbody>
<tr>
<td width="81" valign="bottom"><strong>C# Adı</strong></td>
<td width="138" valign="bottom"><strong>CTS Karşılığı</strong></td>
<td width="405" valign="bottom"><strong>Açıklama</strong></td>
</tr>
<tr>
<td width="81" valign="bottom">object</td>
<td width="138" valign="bottom">System.Object</td>
<td width="405" valign="bottom">Bütün veri  türlerinin türediği kök eleman</td>
</tr>
<tr>
<td width="81" valign="bottom">string</td>
<td width="138" valign="bottom">System.String</td>
<td width="405" valign="bottom">Unicode  karakterlerinden oluşan string</td>
</tr>
</tbody>
</table>

Aşağıda örnek olarak **Int** ve **Double** değişkenlerinin min. ve max değerlerini inceledik..

{% highlight csharp %}
Console.WriteLine("Int degiskenin Min Degeri :" + int.MinValue.ToString());
Console.WriteLine("Int degiskenin Max Degeri :" + int.MaxValue.ToString());
Console.WriteLine("Double degiskenin Min Degeri :" + double.MinValue.ToString());
Console.WriteLine("Double degiskenin Max Degeri :" + double.MaxValue.ToString());
{% endhighlight %}
