# Bölüm 1

## Önsöz

> [Buradan](https://github.com/vigo/ruby101-kitap/blob/master/README.md) alıntılanmıştır.

Kitap yazmak hep hayalini kurduğum bir şeydi. Hem kendi işime yarayacak hem de başkalarının işini görecek bir kitap olmalıydı. Aslında bir sene önce bu işe soyundum ama bir türlü fırsat bulamadım.

Kafamda kabaca planlar yaptım hep ama son noktayı bir türlü koyamadım. [Gitbook.io](http://gitbook.io) bu konuda çok işime yaradı. Hem beni fişekledi hem de [GitHub](http://github.com) ile kolay entegre olması kendimi organize etmem açısından çok rahat oldu.

Hep [O'Reilly](http://www.oreilly.com/)'nin **Pocket** yani cep kitaplarına bayılmışımdır. Hem boyut itibariyle hem de içerik anlamında. Sürekli yanınızda taşıyabileceğiniz, içinde konusuyla ilgili her şeyin kompak bir şekilde bulunduğu kaynak.

Amacım, bu kitaplar tadında, her zaman yanınızda bulunabilecek, tabiri caizse **başucu** kitabı hazırlamak.

Kitabı hazırlarken en çok zorlandığım kısım İngilizce'den anlamlı Türkçe metinler çıkartmak oldu. Bazı şeyleri İngilizce olarak ifade etmek çok kolay, fakat bazı durumlarda tam Türkçe anlamlı karşılık bulmak gerçekten zor oluyor.

Prensip olarak **Developer** (_Yazılım Geliştiren Kişi_) denen insanın **default** olarak İngilizce bilmesi gerektiğine inanıyorum. Neden? Örneğin milyonlarca açık-kaynak projenin bulunduğu GitHub'da herkes İngilizce konuşuyor.

Takıldığınız bir konuda, GitHub'da yorumları okumanız gerekecek. Hatta bazen siz bir şey soracaksınız. **Issue**'lara bakacaksınız, **Pull Request** yapacaksınız. Gördüğünüz gibi bir cümlede iki tane İngilizce terim. Bunlar evrensel. Bilmemiz gerekiyor yoksa çuvallarız :)

Özellikle pek çok şeyi olduğu gibi İngilizce olarak kullanmak istedim. Tabii ki Türkçe anlamını da yazdım fakat, genel olarak kullandığım terminoloji Ruby ve yazılım geliştirme terminolojisi.

Örneğin **Constant** dediğimde bunun ne anlama geldiğini anlamış olmanız gerekiyor. Ya da **Instance** dediğimde, bunun sınıftan oluşturulmuş bir nesne olduğunu anlamanız gerekiyor.

Yazılım dünyası ne yazık ki İngilizce ve tüm kaynaklar da İngilizce. Bu bakımdan orijinal kelimeleri ve terminolojiyi öğrenmemiz, bilmemiz şart :)

## Yazar Hakkında

> [Buradan](https://github.com/vigo/ruby101-kitap/blob/master/yazar_hakkinda/README.md) alıntılanmıştır.

1972’de İstanbul’da doğdum. Rahmetli anne-annem’in satın aldığı 
[Commodore 16][link-C16] ile bilgisayarı hayatımı değiştirdi. Uzun yıllar 
[Commodore 64][link-C64] ve [Amiga][link-AMIGA] kullandım. İnternetin, 
Google'ın ve Stackoverflow'un olmadığı bir dünyada [Assembly][link-assembly] 
dili ile **code** yazdım.

1990'ların başında, televizyonlarda fırtına gibi esen [Dinozorus][link-dinozorus], 
Küp Küp, Sokak Dövüşcüsü gibi oyunları Amiga platformunda kodlamıştım. 1997'nin 
ortalarına kadar Amiga ile devam ettim hayatıma. 1998-2007 arasında Windows/PC 
kullanmak zorunda kaldım. [Asp 3.0][link-asp] ile başlayan web programlama 
maceram, sırasıyla **JavaScript**, **Php**, **Perl**, **Python** ve son olarak 
**Ruby** ile devam etmekte.

2003 yılında evlendim, 2011’de dünyanın en güzel hediyesini verdi eşim. 
Kızım **Ezel** dünyaya geldi! Bazen beraber Amiga'da oyun oynuyoruz :)

Uzun yıllar [İstanbul Bilgi Üniversitesi][link-bilgi]’nde çalıştıktan sonra 
2013’te [webBox.io][link-webbox] şirketini kurdum. [Kod.io 2013](http://kod.io), 
[Kod.io 2014](http://linz.kod.io), [Codefront.io](http://codefront.io), 
[Jsist 2014](http://jsist.org) gibi konferanslar düzenledik. 

[Failconf](http://failconf.io), [Hack-ing](http://hack-ing.io) gibi 
organizasyonlar ve çeşitli meetup’lar yaptık. Ocak 2015’de webBox.io’nun
faaliyetlerini bitirdik. Mayıs 2015’ten beri tekrar eski görevime, 
İstanbul Bilgi Üniversitesi’ne geri döndüm.

Halen, büyük bir heyecanla, **Amiga** ve **Commodore 64** makine dili 
programlama ile uğraşıyorum. Sizlere de tavsiye ederim :)

[link-C16]:       http://en.wikipedia.org/wiki/Commodore_16
[link-C64]:       http://en.wikipedia.org/wiki/Commodore_64
[link-AMIGA]:     http://en.wikipedia.org/wiki/Amiga
[link-assembly]:  http://en.wikipedia.org/wiki/Assembly_language
[link-dinozorus]: https://github.com/vigo/dinozorus
[link-asp]:       http://en.wikipedia.org/wiki/Active_Server_Pages
[link-bilgi]:     http://bilgi.edu.tr
[link-webbox]:    http://webbox.io

## Ruby Hakkında

1990'lı yılların ortalarında \(1995\) **Yukuhiro "Matz" Matsumoto** tarafından geliştirilen Ruby, günümüzde en çok kullanılan [açık-kaynak](https://github.com/ruby/ruby) yazılımların başında geliyor.

> Üretkenlik \(az kod, çok iş\) ve basitliğe odaklı, dinamik, açık-kaynak programlama dili. Okuması ve yazması kolay, anlaşılabilir nitelikte!

Dilin en büyük esin kaynakları tabii ki yine varolan diller. Bunlar; Perl, Smalltalk, Eiffel, Ada ve Lisp dilleri.

İlk kararlı \(stable\) sürümü 1995'de yayınlanan Ruby'nin geliştiricilerin tam anlamıyla dikkatini çekmesi **2006** yılına kadar sürdü. Keza ilk versiyonları gerçekten çok yavaş ve sıkıntılıydı.

Ruby en büyük patlamasını [Ruby on Rails](http://rubyonrails.org/) framework'ü ile yaptı. Danimarkalı yazılımcı [@dhh](http://twitter.com/dhh)'in \(_David Heinemeier Hansson_\) yayınladığı bu framework ne yazık ki Ruby dilinin önüne bile geçti.

Kitabı yazdığım an itibariyle \(_13 Temmuz 2014, Pazar_\) Ruby'nin en son sürümü `2.1.2` \(Stabil sürüm\)

* Güncelleme: Ruby versiyon `2.1.3` oldu. \(_26 Ekim 2014, Pazar_\)
* Güncelleme: Ruby versiyon `2.1.5` oldu. \(_6 Aralık 2014, Pazar_\)
* Güncelleme: Ruby versiyon `2.2.0` oldu. \(_25 Aralık 2014_\)
* Güncelleme: Ruby versiyon `2.2.1` oldu. \(_3 Mart 2015_\)
* Güncelleme: Ruby versiyon `2.2.2` oldu. \(_1 Mayıs 2015_\)
* Güncelleme: Ruby versiyon `2.3.0` oldu. \(_24 Aralık 2015_\)
* Güncelleme: Ruby versiyon `2.3.1` oldu.


Ruby'nin en önemli özelliği her şeyin bir nesne yani **Object** olmasıdır. Nesneyi bir tür paket \/ kutu gibi düşünebilirsiniz. Doğal olarak, **Object** yani nesne olan bir şeyin, action'ları \/ method'ları da olur.

Ruby'nin yaratıcısı **Matz** şöyle demiş:

> **Perl** dilinden daha güçlü, **Python** dilinden daha object-oriented bir script dili olmasını istedim.

Pek çok programlama dilinde sayılar primitive \(ilkel\/basit\) tiplerdir, nesne değildirler. Halbuki Ruby'de sayılar dahil her şey nesnedir. Yani sayının method'ları vardır :\)

[Örnek](https://www.ruby-lang.org/en/about/)

```ruby
class Numeric
  def topla(x)
    self.+(x)
  end
end

5.topla(6)  # => 11
5.topla(16) # => 21
```

Sayılara \(_yani Numeric tipine_\) `topla` diye bir method ekledik...

Block ve Mixin ise Ruby'nin yine öne çıkan özelliklerindendir. Block denen şey aslında [closure](http://en.wikipedia.org/wiki/Closure_%28computer_programming%29)'dur. Herhangi bir method'a block takılabilir:

```ruby
search_engines = %w[Google Yahoo MSN].map do |engine|
  "http://www." + engine.downcase + ".com"
end

search_engines # => ["http://www.google.com", "http://www.yahoo.com", "http://www.msn.com"]
```

Bu örnekte `%w` string'i boşluklarından ayırarak bir array \(dizi\) formatına çeviri. Yani sonuçta `%w[Google Yahoo MSN]` dediğimizde elimize `["Google", "Yahoo", "MSN"]` dizisi gelir. `map` metodu bize bu diziden bir `Enumarator` dönecektir ve `do/end` kısmı ise bizim `block` kısmımızdır. Bu konuda daha açıklayıcı bilgiyi 3. bölümdeki Bloklar başlığı altında bulacaksınız.

Ruby'de bir **Class** \(sınıf\) sadece tek bir sınıftan türeyebilir. Yani A class'ı B'den türer ama aynı anda hem B'den hem C'den türeyemez. Bu Python'da mümkün olan bir şeydir. Ruby'de ise bunun üstesinden gelmek için class'lar **Module**'leri kullanır. Bir Class N tane Module içerebilir, işte bu tür nesnelere **Mixin** denir:

```ruby
class MyArray
  include Enumerable
end
```

Ortak kullanılacak metodları ya da değişkenleri ayrı bir Module olarak tasarlayıp, gerektiği yerde `include` ederek Class + Module karışımından oluşan Mixin'ler ortaya çıkar.

Diğer dillerdeki gibi Exception Handling, Garbage Collector özelliklerinin yanı sıra, C-Extension'ı yazmak diğer dillere göre daha kolaydır. İşletim sisteminden bağımsız **threading** imkanı sunmaktadır. Pek çok işletim sisteminde Ruby kullanmak mümkündür: Linux \/ Unix \/ Mac OS X \/ Windows \/ DOS \/ BeOS \/ OS\/2 gibi...

Ruby'den türemiş farklı Ruby uygulamaları da var:

[JRuby](http://jruby.org/), [Rubinius](http://rubini.us/), [MacRuby](http://www.macruby.org/), [mruby](http://www.mruby.org/), [IronRuby](http://www.ironruby.net/), [MagLev](http://ruby.gemstone.com/), [Cardinal](https://github.com/parrot/cardinal)

Son olarak, **Test Driven Development** yani test'le yürüyen geliştirme mentalitesinin en iyi oturduğunu düşündüğüm dillerden biri Ruby'dir. Çok güzel test kütüphaneleri var ve nasıl kullanabileceğimize dair tonlarca blog\/video sitesi de mevcut!

> Ruby ile programlamak eğlencelidir!

diyor Matz, haydi o halde biz de bu eğlenceye katılalım :\)

## Kurulum

### MacOS / OSX

MacOS Catalina sürümü ve önceki sürümler için Ruby geliştirme ortamı kurulu geliyor iken; ilerleyen sürümlerde Ruby'nin kurulu olarak gelmeyeceği [belirtilmiş.](https://developer.apple.com/documentation/macos-release-notes/macos-catalina-10_15-release-notes) Bu işletim sistemini kullananlar için Homebrew ile kurulum yapmaları yahut [rvm](#rvm), [rbenv](#rbenv) gibi Ruby sürüm yöneticilerini kullanarak Ruby kurmalarını öneririm.

### Linux

[Debian](http://debian.org) ve [Ubuntu](http://ubuntu.com) kullanan okuyucularımız

```bash
sudo apt-get install ruby # ya da
sudo aptitude install ruby
```
[CentOS](https://www.centos.org/), [Fedora](http://fedoraproject.org/), ya da [RedHat](http://www.redhat.com/) kullananlar:

```bash
sudo yum install ruby
sudo dnf install ruby # fedora
```

[Gentoo](http://www.gentoo.org/) kullananlar;
```bash
sudo emerge dev-lang/ruby
```

### Kaynak Kodunu Derleme

Ruby'nin sitesinden [tar dosyasını](https://www.ruby-lang.org/en/downloads/) indirip;
```bash
./configure
make
sudo make install
```
şeklinde de kurulum yapabilirsiniz.

### Windows

[Bu siteden](http://rubyinstaller.org/) özel Windows için hazırlanmış Ruby kurulum paketini indirip klasik "next" > "next" diyerek kurulum yapabilirsiniz.


### Ruby Versiyon Yöneticileri

Bazen, kullandığınız hazır kütüphanelerin destekledikleri Ruby versiyonlarındaki kısıtlamalar ya da kişisel tercihiniz gibi, farklı nedenlerle birden fazla Ruby sürümü ile çalışmak isteyebilirsiniz. Projelerinizden biri, örneğin `ruby 1.9.3` kullanırken, diğer bir projeniz `ruby 2.1.0` kullanıyor olabilir. Bu anlarda kullandığınız Ruby versiyonunu kolayca değiştirmek, aslında aktive etmek de diyebiliriz, için 2 adet popüler versiyon yöneticisi bulunmaktadır.

#### Rbenv

[Rbenv](https://github.com/sstephenson/rbenv) meşhur [37 Signals](http://37signals.com/)'ın. Aslında orada çalışan [Sam Stephenson](https://github.com/sstephenson) tarafından geliştirilmiş bir araç.

Eğer OSX ve [Homebrew](http://brew.sh) kullanıyorsanız kurulum çok kolay:

```bash
brew install rbenv ruby-build
```

Eğer farklı bir işletim sistemi kullanıyorsanız (Linux/Unix tabanlı)

```bash
git clone https://github.com/sstephenson/rbenv.git ~/.rbenv

# sonra PATH'e ekleyin
export PATH="$HOME/.rbenv/bin:$PATH"

# açılışa bunuda ekleyin
# hangisini kullanıyorsanız (.bashrc, .profile ya da .bash_profile)
eval "$(rbenv init -)"
```

Kurulumdan sonra istediğini Ruby versiyonu için;

```bash
# kurulabilecek versiyonları göster
rbenv install -l

# ruby 2.3.0'ı kuralım
rbenv install 2.3.0
```

Kurulan Ruby'yi

* Sistem genelinde `rbenv global`
* Sadece bulunduğumuz dizin içinde (Uygulamaya Özel) `rbenv local`
* Anlık, sadece Shell'de `rbenv shell`

aktive etme opsiyonlarımız var. Örneğin proje dizinin içine `.ruby-version` dosyası koyar ve içine de hangi versiyonu kullandığımızı yazarsak o dizine geçtiğimiz an Ruby versiyonu değişir.

Yani **A** projesinde versiyon `2.1.1`, **B** projesinde version `1.9.3` kullanmak için;

```bash
cd ~/projelerim/A/
echo "1.9.3" > .ruby-version

cd ~/projelerim/B/
echo "2.1.1" > .ruby-version

# bakalım hangi versiyonu aktive etmişiz?
rbenv version
```

#### RVM

Adından da anlaşılacağı gibi **R**uby **V**ersion **M**anager yani [RVM](https://rvm.io/) de aynı Rbenv gibi Ruby versiyonlarını kolay yönetmeyi sağlıyor. Ruby dünyasından Rbenv'ciler ve RVM'ciler olarak iki kanat olduğunu söyleyebilirim.

Kurulumu da zor değil:

```bash
gpg --keyserver hkp://keys.gnupg.net --recv-keys D39DC0E3
\curl -sSL https://get.rvm.io | bash -s stable
```

Rbenv'den en büyük farklığı **Gem Set** yani proje bazlı Ruby paketi yönetimi özelliği.

Ben Rbenv'ci olduğum için RVM kullanmıyorum. Özellikle yeni başlayanlar için RVM'i öneriyorum, Rbenv'e göre daha kolay kurulumu ve kullanımı var.

## İnteraktif Kullanım

Ruby'nin (*ve benzer dillerin*) en çok hoşuma giden özelliği İnteraktif Shell özelliği olmasıdır. Aynı eskiden Commodore 64 günlerindeki gibi, shell'i açıp Ruby yazmaya başlayabiliriz.

Genel olarak bu interaktif kullanım **REPL** olarak geçer. REPL aslında **R**ead **E**valuate **P**rint **L**oop'un baş harfleridir. Yani, kullanıcıdan bir input (_Read_) gelir, bu girdi çalıştırılır (_Evaluate_), sonuç ekrana yazdırılır (_Print_) ve son olarak başa döner ve yine input bekler (_Loop_).

REPL olayı, Python ve PHP'de de var.

### IRB

Ruby kurulumu ile beraber gelir. Yapmanız gereken Terminal'i açıp `irb` yazıp `enter`a basmak.

```bash
irb
irb(main):001:0> print "Merhaba Dünya"
Merhaba Dünya=> nil
irb(main):002:0>
```

Örnekleri yaparken çok sık kullanacağız bu komutu. Keza, daha da geliştirilmiş bir versiyon olan `pry` gem'ini de göreceğiz ilerleyen bölümlerde.


### Shell

**Shebang** dediğimiz yöntemli Linux/Unix tabanlı işletim sistemlerinde Ruby dosyalarını aynı bir uygulama çalıştırır gibi kullanabilirsiniz.

`test.rb` dosyası olduğunu düşünün; bu dosya

```bash
#!/usr/bin/env ruby
puts "Merhaba dünya"
```

şeklinde olsun. Bu dosyayı çalıştırmak için ya

```bash
ruby test.rb
```

ya da, dosyanın Execute flag'ini aktif hale getirerek

```bash
chmod +x test.rb
./test.rb
```

çalıştırabilirsiniz. Eğer Execute flag'ini aktif hale getirmez iseniz işletim sistemi size aşağıdaki gibi bir hata dönecektir.

```bash
permission denied: ./test.rb
```

Bunun sebebi dosyanızın Execute edilebilmesi için izninin bulunmamasıdır. Konuya daha yakından bakmak için `test.rb` dosyasının bulunduğu dizinde `ls -l` komutunu çalıştıralım. 

```bash
$ ls -l
total 8
-rw-r--r--  1 kullanici  staff  42 May 20 23:48 test.rb
```

Burada görüleceği gibi `-rw-r--r--` dosyanızın sadece okuma ve aktif kullanıcı için yazma izni bulunmakta. Eğer yukarıdaki gibi Execute flag'ini aktif hale getirirseniz dosyanızın son hali aşağıdaki gibi olacaktır. 

```bash
$ ls -l
total 8
-rwxr-xr-x  1 kullanici  staff  42 May 20 23:48 test.rb
```

Şu anda dosyanız tüm kullanıcılarda okunabilir ve çalıştırılabilir durumda. 

## Ruby Komutu ve Parametreleri

Kurulum işlemleri bittikten sonra ya da kullandığınız **OS** (_İşletim Sistemi_) ön tanımlı Ruby ile geliyorsa hemen aşağıdaki testi yapabilirsiniz:

```bash
ruby --help

Usage: ruby [switches] [--] [programfile] [arguments]
```

`ruby`'yi çağırırken, her shell aracı gibi (_binary mi diyim, executable mı diyim karar veremedim!_) Ruby de çeşitli parametreler alabiliyor. Bu kısıma dikkat edelim çünkü burada bahsi geçecek **Switch**'ler 2.Bölümde göreceğimiz **Ön Tanımlı Değişkenler** ile çok alakalı.

| Switch | Açıklaması |
| -- | -- |
| -0[octal] | `$/` değeridir. (_Ön tanımlı değişkenlerde göreceğiz_) **octal** yani 8'lik sistemde değer atanır. Örneğin `ruby -0777` şeklinde çalıştırılsa, Ruby, dosya okuma işlemleri sırasında tek seferde dosyayı okur ve tek **String** haline getirir. |
| -a | `-n` ve `-p` ile birlikte kullanılınca **autosplit mode** olarak çalışır. Yani `$F` => `$_.split` şeklinde işler. |
| -c | **Syntax Check** yani dosya içindeki kodu çalıştırmadan, sadece söz dizimi kontrolü yapar ve çıkar. |
| -C**directory** | Ruby önce belirtilen **directory**'ye `cd` (_Shell'de bir dizine geçiş yapmak_) yapar ve daha sonra kodu çalıştırır. `ruby -C/tmp/foo` gibi.. |
| -d, --debug | Debug modda çalıştırır. Bu esnada `$DEBUG` değişkeni de `true` değerini alır. Yani eğer kodunuzun içinde `if $DEBUG` gibi bir ifade kullanabilir ve sonucunu görebilirsiniz. |
| -e **'command'** | Komut satırından tek satırda Ruby kodu çalıştırmak için. `ruby -e 'puts "hello"'` gibi. |
| -Eex[:in], --encoding=ex[:in] | Varsayılan karakter encoding'i (_Internal ve External için..._) |
| --external-encoding=encoding | `-E` gibi |
| --internal-encoding=encoding | `-E` gibi |
| -F**pattern** | **auto split** için ve `split()` için varsayılan regex pattern'i. `$;`değeri. |
| -i | **in-place-edit** mod. Lütfen örneğe bakın! |
| -I | `$LOAD_PATH`'e ilave path ekleme. |
| -K | Japonca (_KANJI_) encoding belirtilir. `UTF-8` için `-K u` kullanılabilir. |
| -l | Otomatik satır sonu (_Line Ending_) işlemi. `-n`ve `-p` ile çalışır. Önce `$\` değişkenine `$/` değeri atanır, `chop!` method’u her satıra uygulanır. |
| -n | Komut satırındaki `sed -n` ya da `awk` gibi çalışır. Sanki kodun etrafında **loop** varmış gibi davranarak süzgeçten geçirir. |
| -p | `-n` gibi çalışır, farkı `$_` den gelen değeri döngünün sonunda **print** eder. |
| -r | `require` komutunun yaptığı gibi verilen değeri **require** eder. (**require** komutunu ileride göreceğiz) |
| -s | Komut satırı argümanlarını **parse** etme (_işleme_) özelliğini aktive eder. |
| -S | `$PATH` çevre değişkenini bulmayı forse eder. Örneğe bakınız! |
| -T | Güvenlik seviyesi, **tainted** kontrolünü devreye sokmak. `$SAFE` değişkenine `-T` ile geçilen değer atanır. |
| -v, --verbose | Önce versiyon numarasını yazar sonra da **verbose** (_Ayrıntılı çalıştırma_) modu aktive eder. Yani `$VERBOSE` **true** olur. |
| -w | `-v` ile aynı işi yapar sadece versiyon numarasını yazmaz. |
| -W | Uyarı seviyesini belirler (_Warning Level_). **0** Sessiz, **1** Orta şekerli, **2** Verbose! |
| -x | **Shebang**'den önceki yazıyı siler atar ve alternatif olarak ilgili dizine `cd` yapar.  |
| --copyright | Ruby'e ait telif bilgisini yazar. `ruby - Copyright (C) 1993-2013 Yukihiro Matsumoto` |
| --enable=feature[,...], --disable=feature[,...] | Örneğin kodun **RubyGem**'lerini kullanmasını istemiyorsanız `--disable-gems` şeklinde, ya da `$RUBYOPT` çevre değişkenini devre dışı bırakmak için `--disable-rubyopt` gibi. `--disable-all` her ek özelliği devre dışı bırakır. `--enable-all` ya da devreye sokar. |
| --version | Versiyon numarasını yazar. |
| --help | Yardım sayfasını gösterir. |

`ruby --help` dışında daha detaylı bilgi `man ruby` yani **man pages**'da bulmak mümkündür, ben de pek çok şeye oradan baktım.

### `-i` örneği:

Önce içinde düz metin olan bir dosya oluşturalım:

```bash
echo vigo > /tmp/test.txt
cat /tmp/test.txt
# vigo
```

sonra;

```bash
ruby -p -i.backup -e '$_.upcase!' /tmp/test.txt
cat /tmp/test.txt.backup
# VIGO
```

Ne oldu? Amacımız, `/tmp/test.txt` dosyasında, satır satır okuyup her satırda yazan metni **uppercase** yani büyük harfe çevirmek. Normalde bu işlemi;

```bash
ruby -p -e '$_.upcase!' /tmp/test.txt
```

şeklinde komut satırından yapabiliyoruz. Ama **in-place-edit** mod ve **extension** özelliği ile, çalıştırılmış kod çıktısını başka bir dosyada görüntüleyebiliriz. Bu noktada `-i` devreye giriyor. `-i.backup` sonucun görüntülendiği dosya oluyor.

### `-n` örneği:

Loop'tan kastım, sanki;

```ruby
while gets
    # kod...
end
```
çevreler.

### `-s` örneği:

`example_s.rb` adında bir dosyamız olsun ve içinde;
```ruby
print "xyz argümanı kullanıldı\n" if $xyz
```

yazsın. Bu dosyayı çalıştırın;

```bash
ruby example_s.rb
```

Hiçbir çıktı görmezsiniz. Eğer şu şekilde çalıştırırsanız;

```bash
ruby -s example_s.rb -xyz
```

şu çıktıyı alırsınız:

    xyz argümanı kullanıldı

### `-S` örneği:

Bazı işletim sistemlerin **Shebang** sorunu olabilir. `#!/usr/bin/env ruby` Bu gibi durumlarda;

```bash
#!/bin/sh
exec ruby -S -x $0 "$@"
```
şekinde, `bash` üzeriden Ruby scripti'ni çalıştırabiliriz.

wip...
