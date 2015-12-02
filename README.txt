Bu websitesi github pages'te yayınlanıyor.

Adres: http://ozgun.github.io

$ git checkout -b gh-pages

"gh-pages" branch'i yayınlanıyor. master'da yapılan değişiklikler sitede
yayınlanmıyor. Sitede değişiklik yapmak için aşağıdaki yol izlenmeli:

* git co master
* master branch'te değişiklik yapılır ve commit ve push edilir
* Eger bu adima kadar degisikikler yayinlanmamissa diger adimlar uygulanir.
* gh-pages branch'ine geçilir: git co gh-pages
* master branch bu branch'e merge edilir: git merge master 
* gh-pages branch'i push edilir: git push -u origin gh-pages
