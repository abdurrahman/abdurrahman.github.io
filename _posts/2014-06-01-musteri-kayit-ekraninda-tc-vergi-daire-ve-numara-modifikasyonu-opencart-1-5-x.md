---
layout: post
title:  Ne tesadüf ama...
date: 2014-04-13 +02:00
abstract: Ben testlerimi yaptım, çalışır halde olduğu için paylaşıyorum ama bu demek değildir ki yedek almanıza gerek yok, her ihtimale karşı yedeğinizi(veri tabanı dahil) mutlaka alın, her zaman sağlamcı olun...
---

Ben testlerimi yaptım, çalışır halde olduğu için paylaşıyorum ama bu demek değildir ki yedek almanıza gerek yok, her ihtimale karşı yedeğinizi(veri tabanı dahil) mutlaka alın, her zaman sağlamcı olun.

İlk adım olarak phpmyadmin üzerinden customer tablosuna eklememiz gereken alanları ekleyelim,
{% highlight sql %}
ALTER TABLE `customer` ADD `tck` VARCHAR( 50 ) NOT NULL AFTER `telephone` ;
ALTER TABLE `customer` ADD `taxoffice` VARCHAR( 50 ) NOT NULL AFTER `tck` ;
ALTER TABLE `customer` ADD `taxno` VARCHAR( 50 ) NOT NULL AFTER `taxoffice` ;
{% endhighlight %}
2- catalog/controller/account/register.php dosyasını açıp, aşağıdaki satırı bulalım,
{% highlight php %}
$this->data['entry_email'] = $this->language->get('entry_email');
{% endhighlight %}
Altına ekleyelim;
{% highlight php %}
$this->data['entry_tck'] = $this->language->get('entry_tck');
$this->data['entry_taxoffice'] = $this->language->get('entry_taxoffice');
$this->data['entry_taxno'] = $this->language->get('entry_taxno');
{% endhighlight %}

Yine aynı dosyada aşağıdaki satırı bulalım,
{% highlight php %}
if (isset($this->error['email'])) {
    $this->data['error_email'] = $this->error['email'];
} else {
    $this->data['error_email'] = '';
}
{% endhighlight %}

Altına ekleyelim,
{% highlight php %}
if (isset($this->error['tck'])) {
    $this->data['error_tck'] = $this->error['tck'];
} else {
    $this->data['error_tck'] = '';
}
if (isset($this->error['taxoffice'])) {
    $this->data['error_taxoffice'] = $this->error['taxoffice'];
} else {
    $this->data['error_taxoffice'] = '';
}
if (isset($this->error['tck'])) {
    $this->data['error_taxno'] = $this->error['taxno'];
} else {
    $this->data['error_taxno'] = '';
}
{% endhighlight %}

Aynı dosyada çalışmaya devam ediyoruz, aşağıdaki satırı bulalım,
{% highlight php %}
if (isset($this->request->post['email'])) {
    $this->data['email'] = $this->request->post['email'];
} else {
    $this->data['email'] = '';
}
{% endhighlight %}

Hemen altına ekliyoruz,
{% highlight php %}
if (isset($this->request->post['tck'])) {
    $this->data['tck'] = $this->request->post['tck'];
} else {
    $this->data['tck'] = '';
}
if (isset($this->request->post['taxoffice'])) {
    $this->data['taxoffice'] = $this->request->post['taxoffice'];
} else {
    $this->data['taxoffice'] = '';
}
if (isset($this->request->post['taxno'])) {
    $this->data['taxno'] = $this->request->post['taxno'];
} else {
    $this->data['taxno'] = '';
}
{% endhighlight %}

Bu dosyadaki yapacağımız son güncellemeye geldik, aşağıdaki satırı bulalım,
{% highlight php %}
if ($this->model_account_customer->getTotalCustomersByEmail($this->request->post['email'])) {
     $this->error['warning'] = $this->language->get('error_exists');
 }
{% endhighlight %}

Hemen altına ekleyelim,
{% highlight php %}
if ((strlen(utf8_decode($this->request->post['tck'])) < 3) || (strlen(utf8_decode($this->request->post['tck'])) > 32)) {
    $this->error['tck'] = $this->language->get('error_tck');
}
if ((strlen(utf8_decode($this->request->post['taxoffice'])) < 3) || (strlen(utf8_decode($this->request->post['taxoffice'])) > 32)) {
    $this->error['taxoffice'] = $this->language->get('error_taxoffice');
}
if ((strlen(utf8_decode($this->request->post['taxno'])) < 3) || (strlen(utf8_decode($this->request->post['taxno'])) > 32)) {
    $this->error['taxno'] = $this->language->get('error_taxno');
}
{% endhighlight %}

3- catalog/view/theme/default/template/account/register.tpl dosyasını açalım ve alttaki kod bloğunu bulalım,
{% highlight html %}
<span class="required"> * </span> <!--?php echo $entry_email; ?--><input type="text" name="email" value="<?php echo $email; ?>" />
<!--?php if ($error_email) { ?-->

<!--?php } ?-->
{% endhighlight %}

Hemen altına aşağıdaki kod bloğunu ekleyelim.
{% highlight html %}
<span class="required"> * </span> <!--?php echo $entry_tck; ?--><input type="text" name="tck" value="<?php echo $tck; ?>" />
<!--?php if ($error_tck) { ?-->

<!--?php } ?-->
<span class="required"> * </span> <!--?php echo $entry_taxoffice; ?--><input type="text" name="taxoffice" value="<?php echo $taxoffice; ?>" />
<!--?php if ($error_taxoffice) { ?-->

<!--?php } ?-->
<span class="required"> * </span> <!--?php echo $entry_taxno; ?--><input type="text" name="taxno" value="<?php echo $taxno; ?>" />
<!--?php if ($error_taxno) { ?-->

<!--?php } ?-->
{% endhighlight %}

4- catalog/language/turkish/account/register.php dosyasını açalım ve aşağıdaki satırı bulalım,
{% highlight php %}
$_['entry_email']          = 'E-Posta:';
{% endhighlight %}

Hemen altına ekleyelim,
{% highlight php %}
$_['entry_tck']            = 'TC Kimlik No:';
$_['entry_taxoffice']          = 'Vergi Dairesi:';
$_['entry_taxno']          = 'Vergi No:';
{% endhighlight %}

5- catalog/model/account/costumer.php dosyasını açalım ve hemen üstte olan kodu bulalım(yine de emin olun)
{% highlight php %}
$this->db->query("INSERT INTO " . DB_PREFIX . "customer SET store_id = '" . (int)$this->config->get('config_store_id') . "', firstname = '" . $this->db->escape($data['firstname']) . "', lastname = '" . $this->db->escape($data['lastname']) . "', email = '" . $this->db->escape($data['email']) . "', telephone = '" . $this->db->escape($data['telephone']) . "', fax = '" . $this->db->escape($data['fax']) . "', password = '" . $this->db->escape(md5($data['password'])) . "', newsletter = '" . (isset($data['newsletter']) ? (int)$data['newsletter'] : 0) . "', customer_group_id = '" . (int)$this->config->get('config_customer_group_id') . "', status = '1', date_added = NOW()");
{% endhighlight %}

Aşağıda yazan kod ile değiştirelim,
{% highlight php %}
$this->db->query("INSERT INTO " . DB_PREFIX . "customer SET store_id = '" . (int)$this->config->get('config_store_id') . "', firstname = '" . $this->db->escape($data['firstname']) . "', lastname = '" . $this->db->escape($data['lastname']) . "', email = '" . $this->db->escape($data['email']) . "', tck = '" . $this->db->escape($data['tck']) . "',taxoffice = '" . $this->db->escape($data['taxoffice']) . "',taxno = '" . $this->db->escape($data['taxno']) . "', telephone = '" . $this->db->escape($data['telephone']) . "', fax = '" . $this->db->escape($data['fax']) . "', password = '" . $this->db->escape(md5($data['password'])) . "', newsletter = '" . (int)$data['newsletter'] . "', customer_group_id = '" . (int)$this->config->get('config_customer_group_id') . "', status = '1', date_added = NOW()");
{% endhighlight %}

Yine aynı dosyada aşağıdaki kod bloğunu bulalım,
{% highlight php %}
$this->db->query("UPDATE " . DB_PREFIX . "customer SET firstname = '" . $this->db->escape($data['firstname']) . "', lastname = '" . $this->db->escape($data['lastname']) . "', email = '" . $this->db->escape($data['email']) . "', telephone = '" . $this->db->escape($data['telephone']) . "', fax = '" . $this->db->escape($data['fax']) . "' WHERE customer_id = '" . (int)$this->customer->getId() . "'");
{% endhighlight %}

Aşağıda yazan kod bloğu ile değiştirelim,
{% highlight php %}
$this->db->query("UPDATE " . DB_PREFIX . "customer SET firstname = '" . $this->db->escape($data['firstname']) . "', lastname = '" . $this->db->escape($data['lastname']) . "', email = '" . $this->db->escape($data['email']) . "', tck = '" . $this->db->escape($data['tck']) . "',  taxoffice = '" . $this->db->escape($data['taxoffice']) . "', taxno = '" . $this->db->escape($data['taxno']) . "', telephone = '" . $this->db->escape($data['telephone']) . "', fax = '" . $this->db->escape($data['fax']) . "' WHERE customer_id = '" . (int)$this->customer->getId() . "'");
{% endhighlight %}

Buraya kadar yapılan işlemler sitedeki kayıt formundaki alanlar içindi, aşağıdaki işlemler admin paneli için, devam edelim;

6- admin/view/template/sale/costumer_form.tpl dosyasını açıp aşağıdaki kod bloğunu bulalım,
{% highlight html %}
<span class="required"> * </span> <!--?php echo $entry_email; ?--><input type="text" name="email" value="<?php echo $email; ?>" />
<!--?php if ($error_email) { ?-->

<!--?php  } ?-->
{% endhighlight %}

Hemen altına ekleyelim,
{% highlight html %}
<span class="required"> * </span> <!--?php echo $entry_tck; ?--><input type="text" name="tck" value="<?php echo $tck; ?>" />
<!--?php if ($error_tck) { ?-->

<!--?php  } ?-->
<span class="required"> * </span> <!--?php echo $entry_taxoffice; ?--><input type="text" name="taxoffice" value="<?php echo $taxoffice; ?>" />
<!--?php if ($error_taxoffice) { ?-->

<!--?php  } ?-->
<span class="required"> * </span> <!--?php echo $entry_taxno; ?--><input type="text" name="taxno" value="<?php echo $taxno; ?>" />
<!--?php if ($error_taxno) { ?-->

<!--?php  } ?-->
{% endhighlight %}

7- admin/controller/sale/costumer.php dosyasını açalım, aşağıdaki satırı bulalım,
{% highlight php %}
$this->data['entry_email'] = $this->language->get('entry_email');
{% endhighlight %}

Hemen altına ekleyelim,
{% highlight php %}
$this->data['entry_tck'] = $this->language->get('entry_tck');
$this->data['entry_taxoffice'] = $this->language->get('entry_taxoffice');
$this->data['entry_taxno'] = $this->language->get('entry_taxno');
{% endhighlight %}

Yine aynı dosyada aşağıdaki kod bloğunu bulalım,

{% highlight php %}
if (isset($this->error['email'])) {
    $this->data['error_email'] = $this->error['email'];
} else {
    $this->data['error_email'] = '';
}
{% endhighlight %}

Hemen altına ekleyelim,
{% highlight php %}

if (isset($this->error['tck'])) {
    $this->data['error_tck'] = $this->error['tck'];
} else {
    $this->data['error_tck'] = '';
}
if (isset($this->error['taxoffice'])) {
    $this->data['error_taxoffice'] = $this->error['taxoffice'];
} else {
    $this->data['error_taxoffice'] = '';
}
if (isset($this->error['taxno'])) {
    $this->data['error_taxno'] = $this->error['taxno'];
} else {
    $this->data['error_taxno'] = '';
}
{% endhighlight %}

Yine aynı dosyada devam ediyoruz, aşağıdaki kod bloğunu bulalım,
{% highlight php %}
if (isset($this->request->post['email'])) {
    $this->data['email'] = $this->request->post['email'];
} elseif (isset($customer_info)) {
    $this->data['email'] = $customer_info['email'];
} else {
    $this->data['email'] = '';
}
{% endhighlight %}

Hemen altına ekleyelim,

{% highlight php %}
if (isset($this->request->post['tck'])) {
    $this->data['tck'] = $this->request->post['tck'];
} elseif (isset($customer_info)) {
    $this->data['tck'] = $customer_info['tck'];
} else {
    $this->data['tck'] = '';
}
if (isset($this->request->post['taxoffice'])) {
    $this->data['taxoffice'] = $this->request->post['taxoffice'];
} elseif (isset($customer_info)) {
    $this->data['taxoffice'] = $customer_info['taxoffice'];
} else {
    $this->data['taxoffice'] = '';
}
if (isset($this->request->post['taxno'])) {
    $this->data['taxno'] = $this->request->post['taxno'];
} elseif (isset($customer_info)) {
    $this->data['taxno'] = $customer_info['taxno'];
} else {
    $this->data['taxno'] = '';
}
{% endhighlight %}

Aynı dosyada yapacağımız son güncelleme için aşağıdaki kod bloğunu bulalım;
{% highlight php %}
if ((strlen(utf8_decode($this->request->post['email'])) > 96) || !preg_match('/^[^\@]+@.*\.[a-z]{2,6}$/i', $this->request->post['email'])) {
    $this->error['email'] = $this->language->get('error_email');
}
{% endhighlight %}

Hemen altına ekleyelim,

{% highlight php %}
if ((strlen(utf8_decode($this->request->post['tck'])) < 3) || (strlen(utf8_decode($this->request->post['tck'])) > 32)) {
    $this->error['tck'] = $this->language->get('error_tck');
}
if ((strlen(utf8_decode($this->request->post['taxoffice'])) < 3) || (strlen(utf8_decode($this->request->post['taxoffice'])) > 32)) {
    $this->error['taxoffice'] = $this->language->get('error_taxoffice');
}
if ((strlen(utf8_decode($this->request->post['taxno'])) < 3) || (strlen(utf8_decode($this->request->post['taxno'])) > 32)) {
    $this->error['taxno'] = $this->language->get('error_taxno');
}

8- admin/language/turkish/sale/costumer.php dosyasını açalım ve alttaki satırı bulalım,
{% highlight php %}
$_['entry_email']               = 'E-Posta:';
{% endhighlight %}
Hemen altına ekleyelim,
{% highlight php %}
$_['entry_tck']       = 'TC Kimlik No:';
$_['entry_taxoffice']       = 'Vergi Dairesi:';
$_['entry_taxno']       = 'Vergi No:';
{% endhighlight %}

9- admin/model/sale/costumer.php dosyasını açalım ve aşağıdaki kod bloğunu bulalım,
{% highlight php %}
$this->db->query("INSERT INTO " . DB_PREFIX . "customer SET firstname = '" . $this->db->escape($data['firstname']) . "', lastname = '" . $this->db->escape($data['lastname']) . "', email = '" . $this->db->escape($data['email']) . "', telephone = '" . $this->db->escape($data['telephone']) . "', fax = '" . $this->db->escape($data['fax']) . "', newsletter = '" . (int)$data['newsletter'] . "', customer_group_id = '" . (int)$data['customer_group_id'] . "', password = '" . $this->db->escape(md5($data['password'])) . "', status = '" . (int)$data['status'] . "', date_added = NOW()");
{% endhighlight %}
Aşağıdaki kod ile değiştirelim,
{% highlight php %}

$this->db->query("INSERT INTO " . DB_PREFIX . "customer SET firstname = '" . $this->db->escape($data['firstname']) . "', lastname = '" . $this->db->escape($data['lastname']) . "', email = '" . $this->db->escape($data['email']) . "', tck = '" . $this->db->escape($data['tck']) . "',taxoffice = '" . $this->db->escape($data['taxoffice']) . "',taxno = '" . $this->db->escape($data['taxno']) . "', telephone = '" . $this->db->escape($data['telephone']) . "', fax = '" . $this->db->escape($data['fax']) . "', newsletter = '" . (int)$data['newsletter'] . "', customer_group_id = '" . (int)$data['customer_group_id'] . "', password = '" . $this->db->escape(md5($data['password'])) . "', status = '" . (int)$data['status'] . "', date_added = NOW()");
{% endhighlight %}
Yine aynı dosyada aşağıdaki kod bloğunu bulalım,
{% highlight php %}

$this->db->query("UPDATE " . DB_PREFIX . "customer SET firstname = '" . $this->db->escape($data['firstname']) . "', lastname = '" . $this->db->escape($data['lastname']) . "', email = '" . $this->db->escape($data['email']) . "', telephone = '" . $this->db->escape($data['telephone']) . "', fax = '" . $this->db->escape($data['fax']) . "', newsletter = '" . (int)$data['newsletter'] . "', customer_group_id = '" . (int)$data['customer_group_id'] . "', status = '" . (int)$data['status'] . "' WHERE customer_id = '" . (int)$customer_id . "'");
{% endhighlight %}
Aşağıdaki kod ile değiştirelim,
{% highlight php %}

$this->db->query("UPDATE " . DB_PREFIX . "customer SET firstname = '" . $this->db->escape($data['firstname']) . "', lastname = '" . $this->db->escape($data['lastname']) . "', email = '" . $this->db->escape($data['email']) . "', tck = '" . $this->db->escape($data['tck']) . "',taxoffice = '" . $this->db->escape($data['taxoffice']) . "',taxno = '" . $this->db->escape($data['taxno']) . "', telephone = '" . $this->db->escape($data['telephone']) . "', fax = '" . $this->db->escape($data['fax']) . "', newsletter = '" . (int)$data['newsletter'] . "', customer_group_id = '" . (int)$data['customer_group_id'] . "', status = '" . (int)$data['status'] . "' WHERE customer_id = '" . (int)$customer_id . "'");
{% endhighlight %}

Yapmamız gereken adımlar bu kadar, yazının başında da dediğim gibi sorunsuz olarak test ettim, yine de problem yaşarsanız yorumlarda bunu dile getirebilirsiniz, yardımcı olurum.
