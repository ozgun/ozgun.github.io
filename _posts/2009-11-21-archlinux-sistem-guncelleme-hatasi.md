---
title: ArchLinux sistem güncellemesinden sonra ortaya çıkan xorg ve intel ekran kartı problemi
tags: linux
layout: post
---

Ne zamandır planladığım ArchLinux full sistem güncellemesini niyahet dün akşam yapmaya başlamıştım. Başlamıştım başlamasına ama, 1 GB’ı indirmek pek kolay olmadığından güncelleme işlemi yarım kalmıştı. Güncellemeye bugün kaldığım yerden devam ettim. Güncelleme esnasında bir sorunla karşılaşmamak için, önce masaüstünden logout olup terminale geçtim. Sonra da wireless’ı devre dışı bırakıp, ethernet kablosunu taktım ki bir de komut satırından wireless ile uğraşmak durumunda kalmayayım.

Güncelleme işleminin hızlı olması için linux.org.tr yansısını aşağıdaki dosyanın en tepesine yazarsanız iyi olur:


{% highlight bash %}
"/etc/pacman.d/mirrorlist"
{% endhighlight %}

Güncellemeye aşağıdaki komut ile başladım:


{% highlight bash %}
# pacman -Syu
{% endhighlight %}

Paketlerin netten indirilmesi bittikten sonra kurulum işlemi başladı. Kurulum işlemi “file system” arızası yüzünden birkaç kez kesintiye uğradı. Onarmak için bilgisayarı SystemRescueCd Linux Live CD ile boot edit aşağıdaki komutları çalıştırdım:


{% highlight bash %}
# fsck /dev/sda6
# fsck.reiserfs --rebuild-tree /dev/sda6
{% endhighlight %}

Ama sorunlar peşimi bıraktı mı? Bırakır mı hiç! Paketlerin kurulumu tamamlandıktan sonra sıra sistemi yeniden başlatmaya geldi. Ne yalan söyleyeyim, bugüne kadar hiç bir full sistem güncellemesini ya da sürüm güncellemesini bir çırpıda başaramamış biri olarak, bu sefer de bir değişiklik olmasını beklemenin bir anlamı yoktu. Netekim de öyle oldu. Benim “Intel 855GM” chipsetli tümleşik ekran kartının “xf86-video-intel” driver’ı ile ilgili bir sorun nedeniyle Xorg çakılıyordu…

Ama umudumu yitirmemiştim. Çünküleyin “ArchLinux forumları”http://bbs.archlinux.org vardı. Ne zaman dara düşsem hızır gibi imdadıma yetişen forumlar… Bu sefer de öyle oldu. Forumdaki [solved] xorg-server and xf86-video-intel-legacy başlıklı mesaj için gönderilmiş yorumları okuduktan ve ArchLinux Wiki’sindeki Intel Graphics dokumanını inceledikten sonra aşağıdaki işlemleri uygulayarak xorg’u yeniden başlatmayı başardım.

“/boot/grub/menu.lst” dosyasındaki şu satırı:

{% highlight bash %}
kernel /boot/vmlinuz26 root=/dev/disk/by-uuid/b66f3e7a-dae8-4867-b17c-cd8f6399c27f ro
{% endhighlight %}

aşağıdaki gibi değiştirdim:


{% highlight bash %}
kernel /boot/vmlinuz26 root=/dev/disk/by-uuid/b66f3e7a-dae8-4867-b17c-cd8f6399c27f ro i915.modeset=1
{% endhighlight %}

Sonra, “/etc/mkinitcpio.conf” dosyasındaki “modules” listesine “intel_agp” ve “i915” modullerini ekledim:


{% highlight bash %}
MODULES="pata_acpi ata_generic scsi_mod ata_piix intel_agp i915"
{% endhighlight %}

Son olarak da, grub’u tekrar sda’ya kurdum. Bu adım gerekli mi tam olarak bilmiyorum ama “menu.lst” dosyasında değişklik yaptığımız için sanki gerekli gibi:


{% highlight bash %}
# grub-install /dev/sda
{% endhighlight %}

### Kaynaklar

* [xorg-server and xf86-video-intel-legacy](http://bbs.archlinux.org/viewtopic.php?id=83741)
* [ArchLinux wiki’sindeki intel ekran kartları ile ilgili dokuman](http://wiki.archlinux.org/index.php/Intel_Graphics)
* [Her derde deva ArchLinux Forumları](http://bbs.archlinux.org/)
* [ArchLinux Wiki](http://wiki.archlinux.org/)
