---
title: Docker Notlarim - I
tags: linux
layout: post
---

Asagidaki notlari "John Willis" tarafindan hazirlanmis 
[YouTube videolarini](https://www.youtube.com/playlist?list=PLkA60AVN3hh_6cAz8TUGtkYbJSL2bdZ4h")
izlerken aldim.

Asagidaki notlar hazirlandigi sirada bilgisayarimda kurulu Docker versiyonu `1.9.1, build a34a1d5` seklindeydi.

### `ps`

Calisan calismayan tum container'lari listelemek icin:

{% highlight bash %}
$ docker ps -a
$ docker ps -aq
{% endhighlight %}

Sadece calisan docker container'larini listelemek icin:

{% highlight bash %}
$ docker ps
$ docker ps -q
{% endhighlight %}

Calisan en son container'i gormek icin:

{% highlight bash %}
$ docker ps -l
$ docker ps -lq
{% endhighlight %}

### `run`

Bir docker image'ini kullarak bir container olusturmak icin asagidaki komut
kullanilir. Eger image henuz inidirilmemisse, yani `docker images` ila
baktigimizda listede gozukmuyorsa, oncelikle o image Docker Hub'dan
indirilir.

{% highlight bash %}
$ docker run busybox
{% endhighlight %}

#### `run -i`

`run` komutunu `-i` parametresi ile calistirdigimizda, eger container
olusturuldugunda calisacak olan default komut(Dockerfile'de bu `CMD` ile
belirlenmistir) bizden input alacak bir komut ise(ornegin `sh`), container'in
icinde shell komutlarini calistirabiliriz. `busybox` image'ini kullanarak bir
container olusturudugumuzda default olarak `sh` komutu calisir, ve bu bizim
komut girmemizi saglar.

{% highlight bash %}
$ docker run -i busybox
{% endhighlight %}

Eger `busybox` image'indeki komutu asagidaki gibi override edersek, bu
durumda container bir input icin beklemeden calisip hemen sonlanacaktir. 

{% highlight bash %}
$ docker run -i busybox echo hello
$ docker run -i busybox touch file.txt
{% endhighlight %}

Yukaridaki komutlari calistirdiktan sonra `docker ps -a` ile kontrol edersek, durmus olan container'lari gorururuz. 

{% highlight bash %}
CONTAINER ID  IMAGE    COMMAND          CREATED          STATUS                      PORTS NAMES
f63efc5ad449  busybox  "touch file.txt" 7 seconds ago    Exited (0) 7 seconds ago          prickly_poitras
902c6abd891d  busybox  "echo hello"     12 seconds ago   Exited (0) 11 seconds ago         sharp_tesla
76d675c8ca65  busybox  "sh"             25 seconds ago   Exited (0) 17 seconds ago         tender_morse
{% endhighlight %}

#### `run -t`

Container'a bir pseudo terminal araciligiyla baglanmamizi saglar. 

`t` parametresi ile ilgili detayli bilgi 
[bu linkte](http://stackoverflow.com/questions/22272401/what-does-it-mean-to-attach-a-tty-std-in-out-to-dockers-or-lxc)
bulunabilir.

{% highlight bash %}
$ docker run -t busybox
{% endhighlight %}

Yukaridaki komuttan sonra `CTRL+C` ile tty(pseudo termnial)'dan cikilir. Cikmis
olmamiza ragmen `docker ps` komutu ile calisan container'lara baktigimizda hala
`busybox` conainter'ini goruruz. `docker stop` komutu ile container'in calismasini durdurabiliriz.

#### `run -it`

Intercative shell istiyorsak `-it` parametersini kullaniriz. Asagidaki komut
ile olusturdugumuz container'in icine girip komutlarimizi calistirmaya baslayabiliriz.

{% highlight bash %}
$ docker run -it busybox
{% endhighlight %}

Container'in icindeyken `exit` komutu ile cikarsak container calismayi
durdurur, `docker ps` komutu ile calisan container'i gormeyiz. 

#### `run -itd` ve `attach`

Asagidaki komut ile "busybox" image'ini kullanarak bir container olusturulur ve
bu container arka planda(detached mode) calismaya devam eder.

{% highlight bash %}
$ docker run -itd busybox
{% endhighlight %}

Yukaridaki komut ile calistirilan container'a baglanmak icin once `docker ps`
komutu ile calisan container'lara bakip container ID'sini ogreniriz.

{% highlight bash %}
CONTAINER ID  IMAGE    COMMAND  CREATED        STATUS        PORTS  NAMES
b0599d3a63f8  busybox  "sh"     4 seconds ago  Up 4 seconds         berserk_hawking
{% endhighlight %}

Yukaridaki ciktidan gorulecegi uzere "busybox" image'inden olusturulan
container calisir calismaz biz herhangi bir parametre gecmedigimiz icin
busybox'un Dockerfile'inda tanimlanan `sh` komutunu calistirmis.

Container'a baglanmak icin ise asagidaki komutu kullaniriz. Asagidaki komuttan
sonra, komut satirina dusmek icin Enter'a basilir.

{% highlight bash %}
$ docker attach b0599d3a63f8
{% endhighlight %}

Eger container'i calistirirken "busybox" image'i icin tanimlanmas default
komut yerine kendi istedigimiz bir komutun calistirmasini istiyorsak:

{% highlight bash %}
$ docker run -itd busybox /bin/dmesg
{% endhighlight %}

Yukaridaki komutu calistirdiktan sonra `docker ps` komutu ile calisan
container'lara baktigimizda bu son container'i goremeyiz. Cunku `/bin/dmesg`
komutu kullanicidan bir input beklemedigi icin calisip sonlanir ve bu da
container'in calismasini durdurur. Duran container'a ait bilgileri gormek icin
`docker ps -a` komutunu kullandigimizda asagidaki bir cikti aliriz:

{% highlight bash %}
CONTAINER ID  IMAGE   COMMAND       CREATED         STATUS                     PORTS   NAMES
5b42f93e1da2  busybox "/bin/dmesg"  10 seconds ago  Exited (0) 9 seconds ago           evil_snyder
{% endhighlight %}

Yukaridaki ciktidaki `STATUS` sutunu bize container'in sonlandigini gosterir.

`/bin/dmesg` yerine `/bin/cat` gibi kullanicidan bir input
bekleyen bir komut calistirsaydik, o zaman container arka planda calismaya devam
edecekti ve `docker ps` ile baktigimizda calisan container'lar arasinda gorebilecektik.

{% highlight bash %}
$ docker run -itd busybox /bin/cat
$ docker ps
{% endhighlight %}

Yukaridaki komutlarin ciktisi:

{% highlight bash %}
CONTAINER ID  IMAGE    COMMAND     CREATED        STATUS        PORTS   NAMES
4c62b281274d  busybox  "/bin/cat"  2 seconds ago  Up 1 seconds          big_saha
{% endhighlight %}

#### `--name`

Bir docker image'ini calistirirken isim vermek icin kullanilir.

{% highlight bash %}
$ docker run --name john1 -itd busybox
{% endhighlight %}


### `stop`

Calisan container'lari durdurmak icin once `docker ps` ile calisan
container'lari buluruz sonra `stop` komutu ile durduruz. 

{% highlight bash %}
$ docker stop 6e41ef36fdc7
{% endhighlight %}

`stop` komutu biraz yavas calisir.

Eger docker container'ini calistirirken `--name` parametresi ile bir isim
vermissek, `stop` komutunu o verdigimiz isimle birlikte de calistirabiliriz.

{% highlight bash %}
$ docker stop john1
{% endhighlight %}

### `kill`

`kill` komutu `stop` komutuna gore daha hizli calisir.

Sadece bir tane container'i durdurumak icin:

{% highlight bash %}
$ docker stop 6e41ef36fdc7
{% endhighlight %}

`kill` komutu ile tum calisan docker container'larini durdurmak icin:

{% highlight bash %}
$ docker kill $(docker ps -q)
{% endhighlight %}

En son calistirdigimiz container'i durdurmak icin:

{% highlight bash %}
$ docker kill $(docker ps -ql)
{% endhighlight %}

### `rm`

Docker container'larinin container ID'si kullanilarak veya isimleri kullanilarak silinmesi.

{% highlight bash %}
$ docker ps -a
$ docker rm a759cdfefc9d
$ docker rm john1
{% endhighlight %}

Durmus olan tum container'larin silinmesi icin asagidaki komut kullanilir:

{% highlight bash %}
$ docker rm $(docker ps -aq)
{% endhighlight %}


### `restart`

Calisan bir docker container'ini durdurup baslatmak icin `restart` komutu kullanilir.

{% highlight bash %}
$ docker run -itd busybox
$ docker restart ebf3e3609c56
{% endhighlight %}

### `exec`

Bir docker container'inda bir process calistirmak icin `exec` komutu kullanilir.

{% highlight bash %}
$ docker run -itd busybox
$ docker exec ebf3e3609c56 ls
{% endhighlight %}

### Container'in Izlenmesi

#### `logs`

Container'in output'a bastigi log'lari gormek icin kullanilir.

{% highlight bash %}
$ docker run -itd busybox cat /etc/hosts
$ docker logs $(docker ps -alq)
{% endhighlight %}

Yukaridaki komutlarin ciktisi asagidaki gibi olur:

{% highlight bash %}
172.17.0.2      7c9d96519bdc
127.0.0.1       localhost
::1     localhost ip6-localhost ip6-loopback
fe00::0 ip6-localnet
ff00::0 ip6-mcastprefix
ff02::1 ip6-allnodes
ff02::2 ip6-allrouters
{% endhighlight %}

#### `inspect` 

Bir container hakkinda detayli bilgi almak icin `inspect` komutu kullanilir.

{% highlight bash %}
$ cid=$(docker run -itd busybox)
$ echo $cid
$ docker inspect $cid
{% endhighlight %}

#### `stats`

Calisan bir container'in ne kadar kaynak tukettiginin, disk IO, ve network trafiginin gorulmesi.

{% highlight bash %}
$ docker run -itd --name job1 busybox
$ docker stats job1
{% endhighlight %}

#### `top`

Calisan bir container'daki process'lerin ne kadar kaynak tukettiginin gorulmesi.

{% highlight bash %}
$ docker run -itd --name job2 busybox
$ watch docker top job2 -ef
{% endhighlight %}

### Docker image'leri

Sistemimizde kurulu Docker image'lerini listelemek icin:

{% highlight bash %}
$ docker images
{% endhighlight %}

Bir image hakkinda detayli bilgi almak icin:

{% highlight bash %}
$ docker inspect busybox
{% endhighlight %}

#### `history`

"busybox" image'ini kullarak bir container olusturulma asamasinda "busybox"
image'inin nasil bir surecten gectigini gormek icin asagidaki komutu
kullaniriz.

{% highlight bash %}
$ docker history busybox
{% endhighlight %}

Yukaridaki komutun ciktisi su sekilde:

{% highlight bash %}
IMAGE         CREATED      CREATED BY                                      SIZE      COMMENT
ac6a7980c6c2  4 weeks ago  /bin/sh -c #(nop) CMD ["sh"]                    0 B       
c00ef186408b  4 weeks ago  /bin/sh -c #(nop) ADD file:c295b0748bf05d4527   1.113 MB  
{% endhighlight %}

#### `search`

Docker hub'da image aramasi yapmak:

{% highlight bash %}
$ docker search ubuntu
{% endhighlight %}

En az 10 star almis docker image'leri arasindan arama yapmak:

{% highlight bash %}
$ docker search -s 10 ubuntu
{% endhighlight %}

#### `pull`

En son "ubuntu" docker image'ini Docker Hub'dan indirmek icin:

{% highlight bash %}
$ docker pull ubuntu
{% endhighlight %}

"14.04" ile tag'lenmis "ubuntu" docker image'ini indirmek icin ise:

{% highlight bash %}
$ docker pull ubuntu:14.04
{% endhighlight %}

### Data Volumes

#### Tek container islemleri

Bir docker container'inde bir data volume olusturmak icin `-v` parametresi
kullanilir. Asagidaki komut ile bir volume olusturulur ve bu volume `/john1`
seklinde container'in icinde mount edilir. Olusturulan volume host sistemde
`/var/lib/docker/volumes` klasorunde tutulur. Bu volume'e yazilan veri docker
image'i silinse dahi silinmez. Ayrica bu volume diger docker container'larinda
ortak kullanilabilir.

{% highlight bash %}
$ docker run -it -v /john1 busybox
#inside-container> cd /john1
#inside-container> touch file1
{% endhighlight %}

Host sistemdeki bir path'i, calistirdigimiz bir docker container'ina mount
etmek:

{% highlight bash %}
$ mkdir /tmp/john1
$ docker run -it -v /tmp/john1:/john1 busybox
{% endhighlight %}

Sadece klasorler degil dosyalari da ayni sekilde mount edebiliriz:

{% highlight bash %}
$ docker run -it -v ~/.bash_history:/.bash_history busybox
{% endhighlight %}

Bazi durumlarda host'taki dosya veya klasorlerin containerin icinden
degistirilmemesi icin readonly mount etmek isteyebilirz. Ornegin host'taki
`~/.vimrc` dosyasini container'a mount readonly mount edelim.

{% highlight bash %}
$ docker run -it -v ~/.vimrc:/.vimrc:ro busybox
{% endhighlight %}

#### Volume'larin Paylasilmasi

Asagidaki ornekte, once "john1" adinda bir container olusturuluyor, ve bu
container'da `/john1` volume'u olusturuluyor. Bu container hala calisir
durumdayken, "john2" adinda yeni bir container olusturuluyor bu container
olusturulurken "john1" container'indaki volume "john2" containerina mount
ediliyor:

{% highlight bash %}
$ docker run -it --name john1 -v /john1 busybox
$ docker run -it --name john2 --volumes-from john1 busybox
{% endhighlight %}

### Network Tricks

Calisan bir docker container'inin IP adresini ogrenmek. cid = container ID, nid = IP Address of Container

{% highlight bash %}
$ cid=$(docker run -itd busybox)
$ echo $cid
$ nid=$(docker inspect --format '{{.NetworkSettings.IPAddress}}' $cid)
$ echo $nid
{% endhighlight %}
