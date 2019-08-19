---
title: R ile Github üzerinden internet sitesi oluşturmak
author: İsmail Bekar
date: '2018-09-08'
slug: internetsitesi
tags:
  - R programlama
comments: no
showcomments: yes
showpagemeta: yes
disqus: false
---


Uzun zamandır kişisel bir internet sitesi oluşturmak istiyordum fakat var olan seçenekler arasında kararsız kalmamdan ötürü bunu bir türlü gerçekleştiremedim. Mesela Wordpress'i bir türlü sevemedim, Wix gibi alternatifleri ise pahalı buldum, sıfırdan bir web sitesi yaratacak programlama bilgisine ve zamana da sahip değildim. Sonra birden karşıma R için yazılmış olan **Blogdown** paketi çıktı. Eğer R programlama diline hakimseniz, çok kısa bir süre içinde internet sitesi oluşturmanız mümkün. Üstelik hiç bir masraf yapmadan, yayınlamak da buna dahil.

Umarım bu rehber size de yardımcı olur. Fakat eğer siz de benim gibi daha önce hiç Github kullanmamışsanız, başta ufak tefek sorunlar yaşamanız mümkün. Eğer yazıda bahsetmeyi akıl edemediğim bir noktada sorun yaşıyorsanız, Twitter üzerinden bana  -[@fire_ecologist](https://twitter.com/fire_ecologist)- yazabilirsiniz, belki birlikte bir çözüm bulabiliriz.
\
\
**1.  Github Kısmı**

Siteyi GitHub üzerinden yayınlayacağımız için burada bir hesap oluşturmamız gerekiyor. Ben GitHub sevmiyorum diyorsanız GitLab da kullanabilirsiniz fakat ben işlemleri GitHub üzerinden anlatacağım.

Github hesabını oluşturduktan sonra iki tane repository (yazının geri kalanında depo olarak geçecek) oluşturmanız gerekiyor. Bir numara sitenin kaynak halini, iki numara ise sitenin yayınlanmasını sağlayan ve aslında bir numarada da var olan `public` klasörünü barındıracak. Bunu yapmamızın sebebi [şurada (İngilizce)](https://tclavelle.github.io/blog/blogdown_github/) yazıyor.

**Github depoları**

`DEPO 1`= blog_kaynak

`DEPO 2`= kullanıcıadı.github.io

Önce `GitHubURL`'yi github repo sayfanızdan kopyalayın ve daha sonra bilgisayarınızın terminalini açarak sırasıyla şu komutları yazın. 


```
cd /Users/kullanıcı/GitHub2 
git clone GitHubURL1
git clone GitHubURL2
```

Böylece Github’da kurduğunuz iki depo bilgisayarınıza klonlanmış oldu.
\
\
**2.  Blogdown ve Hugo'yu yüklemek**

R konsoluna aşağıdaki kodları yazarak Blogdown paketini ve hugoyu yükleyin.

```
install.packages("blogdown")
library(blogdown)
install_hugo()
```
\
**3. Tema seçimi**

Ben yazıda [nishanths/cocoa-hugo-theme](http://bit.ly/2wVMhBF) teması üzerinden ilerleyeceğim ama siz https://themes.gohugo.io adresine giderek bir başka tema seçebilirsiniz. Fakat bu durumda temanın rehberini okumanızı tavsiye ederim, bazı ayarlar değişiklik gösterebilir.

Eğer farklı bir tema kullanırsanız temanın Github sayfasında yazan kullanıcı adı (nishanths) ve tema ismi (cocoa-hugo-theme) kısmını kopyalamanız ve 4. adımdaki ilgili yerleri ona göre değiştirmeniz gerekiyor.
\
\
**4.  Temanın yüklenmesi**

R konsolunda aşağıdaki kodu yazarak temayı yükleyelim. Burada bir önemli nokta format seçeneği. Seçeceğiniz temanın Github sayfasında tema türünü kontrol edin, eğer `.toml` ise sıkıntı yok fakat `.yaml` ise format satırını ona göre düzenleyin. 
```
setwd("/Users/kullanıcı/GitHub2")
new_site(dir = 'blog_kaynak', 
         theme = 'nishanths/cocoa-hugo-theme',
         format = 'toml')
```
\
\
**5.  GitHub bağlantısının oluşturulması**

`blog_kaynak` klasöründe yer alan `config.toml` sitenizle ilgili bir çok temel ayarın yer aldığı bir dosya. Burada sitenizin adını, çeşitli özelliklerini vs. değiştirebilirsiniz. Fakat öncelikle şu iki satırı bu şekilde değiştirmeniz lazım:

baseurl = `"https://kullanıcıadı.github.io/"`

publishdir = `"../kullanıcı.github.io"`

Bu sayede blog_kaynak klasöründeki *public* dosyası, GitHub2 altında yer alan *kullanıcıadı.github.io* adlı depoya kopyalanıyor (sitenin yayınlanmasından sorumlu).
\
\
**6.  Sitenin yayınlanması**

R çalışma dizinini blog_kaynak klasörüne getirip, siteyi oluşturuyoruz.
```
setwd("~/GitHub2/blog_kaynak/")
build_site()
```

Şimdi *kullanıcıadı.github.io* adlı klasördeki dosyaları Github'a yollayıp siteyi yayınlayalım.

Bilgisayarınızda terminali açarak aşağıdaki ilgili yerleri değiştirerek kodları yazın. 
```
cd /Users/kullanıcı/GitHub2/kullanıcıadı.github.io/

git status

git add .

git commit -m  “site olusturuluyor”

git push
```
Tebrikler! Artık siteniz yayınlandı ve kullanıma hazır. 

*kullanıcıadı.github.io* adresine gidip kontrol edebilirsiniz.
\
\
**Birkaç not:**
\
\

- Content kısmında isminden de anlaşılacağı üzere içerikleriniz var.

- Verilen uyarılardan kurtulmak için bu satırı da ekleyebilirsiniz. Ne yaptığı hakkında peki bir fikrim yok ama işe yarıyor...

```
ignoreFiles = ["\\.Rmd$", "\\.Rmarkdown$", "_files$", "_cache$"]
```
- Static/img kısmında ise fotoğraflar var. Fotoğraf eklemenin en kolay yolu `Addins` seçeneğinden `Insert Image` seçeneğini kullanmak.
- Yeni bir gönderi yayınlayacağınızda da yine `Addins` seçeneğinden `New post` özelliğini kullanabilirsiniz. Burada subdirectory kısmını değiştirmeyi unutmayın. Eğer post olarak bırakırsanız ve kullandığınız temada post sayfası yoksa ve bir içerik yayınlamaya çalışırsanız sitede görünmeyecektir. 
- Yeni bir değişiklik yaptığınızda kullanıcıadı.github.io klasöründeki değişiklikleri Github'a yollamanız gerekiyor.

Şimdilik aklıma gelenler bunlar, başta da söylediğim gibi eğer sorularınızı Twitter üzerinden -[@fire_ecologist](https://twitter.com/fire_ecologist)- iletirseniz hem cevaplamaya çalışır hem de yazıyı güncelleyip, daha kapsamlı hale getirebilirim.
____
**Yararlandığım ve daha fazla bilgi almak için okuyabileceğiniz kaynaklar:**

- [blogdown: Creating Websites with R Markdown](http://bit.ly/2wPaQ3d)
- [Building a Blog with Blogdown and GitHub](http://bit.ly/2CwnhX1)
- [Bilim insanları için akademik tema kurulumu](http://bit.ly/2wUSo9n)
