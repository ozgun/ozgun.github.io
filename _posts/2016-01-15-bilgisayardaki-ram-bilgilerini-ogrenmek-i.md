---
title: Bilgisayarinizdaki RAM'lerin Ozelliklerini Ogrenmek
tags: linux
layout: post
---

Bilgisayara takili belleklerin(RAM) donanim bilgisini ogrenmek icin Linux'ta
`dmidecode` komutu kullanilabilir.

{% highlight bash %}
$ sudo dmidecode
{% endhighlight %}

Yukaridaki komutun ciktisinin bir kismi su sekilde:

{% highlight text %}
Physical Memory Array
  Location: System Board Or Motherboard
  Use: System Memory
  Error Correction Type: None
  Maximum Capacity: 16 GB
  Error Information Handle: No Error
  Number Of Devices: 2

Memory Device (1. slot'taki RAM)
	Array Handle: 0x0018
	Error Information Handle: 0x001B
	Total Width: 64 bits
	Data Width: 64 bits
	Size: 2048 MB
	Form Factor: SODIMM
	Set: None
	Locator: DIMM0
	Bank Locator: BANK 0
	Type: DDR3
	Type Detail: Synchronous
	Speed: 1067 MHz
	Manufacturer: Not Specified
	Serial Number: 8210CC7A
	Asset Tag: Unknown
	Part Number: M471B5673FH0-CF8  
	Rank: Unknown

Memory Device (2. slot'taki RAM)
	Array Handle: 0x0018
	Error Information Handle: 0x001F
	Total Width: 64 bits
	Data Width: 64 bits
	Size: 4096 MB
	Form Factor: SODIMM
	Set: None
	Locator: DIMM1
	Bank Locator: BANK 2
	Type: DDR3
	Type Detail: Synchronous
	Speed: 1067 MHz
	Manufacturer: Not Specified
	Serial Number: BD1FE94A
	Asset Tag: Unknown
	Part Number: 99U5428-050.A00LF 
	Rank: Unknown

{% endhighlight %}

**Physical Memory Array** bolumundeki **Maximum Capacity** degerine gore
bilgisayara takabilecegim maximum RAM miktari 16 GB'mis.

**Memory Device** ile baslayan bolumler slotlardaki RAM'ler gosteriyor.
Yukaridaki ciktiya gore benim bilgisayarimda 2 adet RAM varmis. Bunlarin
boyulari sirasiyla 2 GB(2048 MB) ve 4 GB(4096 MB) seklindeymis. Ikisinin de
frekansi 1067 Mhz ve tipleri DDR3'mus.

`dmidecode` komutu sadece RAM ile ilgili bilgi vermez, bilgisayarinizin
donanimi ile ilgili oldukca detayli bilgi verir. Ornegin BIOS, islemci hizi,
modeli, cache'i, bilgisayarin ureticisi, modeli, seri numarasi, guc
kaynagi, fan'in done hizi, vs..

Eger sadece RAM ile ilgili bilgileri almak isterseniz `dmidecode`'u asagidaki gibi farkli parameterlerle calistirabilirsiniz:

{% highlight bash %}
$ sudo dmidecode -t 16 
$ sudo dmidecode -t 17 
$ sudo dmidecode -t 6 
{% endhighlight %}

### Kaynaklar:

* `man dmidecode`
* http://www.tecmint.com/how-to-get-hardware-information-with-dmidecode-command-on-linux/
