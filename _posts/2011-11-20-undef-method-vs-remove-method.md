---
title: Ruby - undef_method vs remove_method
tags: ruby metaprogramming
layout: post
---

### undef_method:

{% highlight ruby %}
class Computer
  # Eğer method __ ile başlıyorsa, veya method_missing ise veya respond_to ise
  # methodu çıkarma.  
  instance_methods.each do |m|
    # undef_method kullandığımız zaman üst sınıflardan inherit edilen 
    # tüm methodlar çıkarılır.
    undef_method m unless m.to_s =~ /^__|^method_missing$|^respond_to\?$/
  end
end
{% endhighlight %}

{% highlight ruby %}
c = Computer.new
begin
  c.inspect
rescue Exception => e
  puts "inspect methodu undef_method ile cikarilmis: #{e.message}"
end
{% endhighlight %}

### remove_method:

{% highlight ruby %}
class Monitor 
  def hello 
    "hello"; 
  end
end
m = Monitor.new
puts m.hello
{% endhighlight %}

{% highlight ruby %}
class Monitor
  # inspect methodu Monitor classında tanımlanmadığı için remove_method
  # ile çıkarılamaz.
  begin
    remove_method :inspect 
  rescue Exception => e
    puts e.message
  end

  # remove_method sadece o sınıfa ait methodları çıkarır. Inherit edilen
  # methodlara dokunmaz. O yüzden burda inherit edilen bir methodu
  # remove_method ile çıkarmaya çalıştığımız zaman hata verir.
  puts "hello methodunu remove_method ile cikariyoruz"
  remove_method :hello 
end
{% endhighlight %}

{% highlight ruby %}
begin
  puts m.hello
rescue Exception => e
  puts e.message
end
{% endhighlight %}

### Kaynaklar:

* [Metaprogramming Ruby: Program Like the Ruby Pros](http://pragprog.com/book/ppmetr/metaprogramming-ruby)
