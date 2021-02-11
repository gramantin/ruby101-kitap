# Bölüm 6: İleri Mevzular

## Monkey Patching

Ruby'nin en şaibeli özelliklerinden biridir. Kimileri için müthiş bir şey kimileri için de çok tehlikeli bir özelliktir. 7.7'de bahsettiğim **Meta Programming** konusu ile de çok yakından alakalıdır.

Ruby'deki tüm sınıflar açıktır. Yani Kernel'dan gelen herhangi bir sınıfı modifiye etmek mümkündür. Bu durum yanlış ellerde çok tehlikeli olabilir.

Yani, `String` sınıfındaki herhangi bir method'u bozmak mümkündür. Örneğin, `String#length` methodunu değiştirelim:

```ruby
"Hello".length # => 5 # Bu normali

# Monkey Patching yapıyoruz ve length method'unu değiştiriyoruz.
class String
  def length
    "Uzunluk: #{self.size} karakterdir."
  end
end

"Hello".length # => "Uzunluk: 5 karakterdir."
```

Normal şartlar altında `length` methodu `Fixnum` dönmesi gerekirken, bozduğumuz method bize `String` döndü. Anlatabilmek için bu denli abartı bir örnek vermek istedim. Düşünsenize, kullandığınız herhangi bir kütüphane, kafasına göre, standart olan herhangi bir method'u bu şekilde bozsa?

Tüm kodunuz çorbaya döner ve içinden çıkamaz bir hale gelir. Peki asıl kullanım amacı bu mudur? Tabiiki değil. Bize kolaylık sağlayan işlerde kullanmamız gerekiyor.

Örneğin, basit bir matematik işlemi için, **5 kere 5** önermesini kullanmak istiyoruz:

```ruby
class Fixnum
  def kere(n)
    self * n
  end
end
```

`Fixnum` içine `kere` diye bir method taktık. Haydi kullanalım:

```ruby
5.kere(5)         # => 25
5.kere(5).kere(2) # => 50
```

İşte bu tür bir **Monkey Patching** işe yarar ve kullanılabilitesi yüksek olan bir yöntemdir. Keza Ruby on Rails webframework'ü neredeyse bu mantık üzerine kurulmuştur.

Örneğin **5 gün önce** şeklinde bir önerme yapmak istiyoruz.

```ruby
class Fixnum
  def gün
    self * 24 * 60 * 60
  end

  def önce
    Time.now - self
  end

  def sonra
    Time.now + self
  end
end
```

Şimdi şöyle bir şey yapalım:

```ruby
Time.now    # => 2015-02-09 12:55:33 +0200
5.gün.önce  # => 2015-02-04 12:55:33 +0200
1.gün.sonra # => 2015-02-10 12:55:33 +0200
```

`Fixnum` yani basit sayılara `.gün.önce` ve `.gün.sonra` gibi iki tane method ekledik :)

## Regular Expressions

wip...

## Time ve Date Nesneleri

Zaman, tarih ve saat gibi işlemleri yapmak için Ruby ile birlikte gelen `Time` ve `Date` sınıflarından bahsedeceğim. En basit tanımıyla saatin kaç olduğunu ya da `now` yani **şimdi** / **şu anda** yı bulmak için:

### Time

```ruby
Time.now # => 2015-04-28 08:49:20 +0300
```

yapmamız yeterlidir. Saniye bazında ekleme ya da çıkartma yaparak başka zamanları da bulabiliriz. **1 saat önceyi** bulmak için **60 saniye * 60 dakika** yapmamız yeterli.

```ruby
Time.now - (60 * 60) # => 2015-04-28 07:51:01 +0300
```

Hemen Ruby'sel bir hareketle ufak bir şey yapalım:

```ruby
class Fixnum
  def seconds
    self
  end
  def minutes
    self * 60
  end
  def hours
    self * 60 * 60
  end
  def days
    self * 60 * 60 * 24
  end
end

# 10 gün sonrayı bulalım
Time.now + 10.days # => 2015-05-08 08:54:02 +0300
```

Saat farkları yüzünden çeşitli **Time Zone**'lar mevcut. An itibariyle (_28 Nisan 2015_) yerel zamana baktığımda:

```ruby
Time.local(2015, 4, 28, 8, 54) # => 2015-04-28 08:54:00 +0300
```

Peki [GMT](http://wwp.greenwichmeantime.com/)'ye göre durum ne?

```ruby
Time.gm(2015, 4, 28, 8, 54)    # => 2015-04-28 08:54:00 UTC
```

Şimdi kendi istediğimiz bir zamanı oluşturalım. Doğduğum yıl ve ayı kullanarak bir zaman oluşturup kabaca hangi güne denk geldiğini bulalım. Sadece **YIL** ve **AY** kullanıyoruz!

```ruby
t = Time.new(1972, 8) # => 1972-08-01 00:00:00 +0300
t.monday?  # => false
t.tuesday? # => true
```

Ruby bize otomatik olarak ayın birini işaret etti ve 1 Ağustos 1972'nin salı gününe denk geldiğini `tuesday?` method'u ile anladık.

Tahmin edebileceğiniz gibi, İngilizce olarak, günleri kontrol edebiliyoruz. Yani `sunday?`, `monday?` ... gibi

Eğer UNIX'in epoch zaman formatında istersek, yapmamız gereken `to_i` method'u ile integer'a çevirmek:

```ruby
Time.new(1972, 8).to_i # => 81464400
```

Eğer elimizde epoch cinsinden bir zaman varsa:

```ruby
Time.at(81464400)      # => 1972-08-01 00:00:00 +0300
Time.at(81464400).year # => 1972
```

şeklinde de kullanabiliriz. Bunlara ek olarak;

```ruby
Time.now.zone # => "EEST"
Time.now.day  # => 29
Time.now.wday # => 3      # çarşamba
Time.now.utc? # => false
Time.now.gmt? # => false
```

Zamanlar arasındaki farkı bulmak için de aynı matematik işlemi gibi yaparmış gibi davranabilirsiniz.

```ruby
birth_day = Time.new(1972, 8)         # Ağustos 1972
Time.now.to_i  # => 1430284388
birth_day.to_i # => 81464400

Time.now.to_i - birth_day.to_i        # => 1348819988 saniye
1348819870 / (60 * 60 * 24)           # => 15611 gün
1348819870 / (60 * 60 * 24 * 30)      # => 520 ay
1348819870 / (60 * 60 * 24 * 30 * 12) # => 43 yıl
```

Ayni şekilde karşılaştırma işlemleri de matematik işlemleri gibi. Doğduğum yıl ile bugünü karşılaştıralım:

```ruby
birth_day = Time.new(1972, 8)
now = Time.now
tomorrow = now + (60 * 60 * 24)

now.to_i                       # => 1430284645
tomorrow.to_i                  # => 1430371045
birth_day.to_i                 # => 81464400

now.to_i > birth_day.to_i      # => true      bugün > doğum tarihi
now.to_i > tomorrow.to_i       # => false     bugün < yarın
tomorrow.to_i > now.to_i       # => true      yarın > bugün
```

### Zamanı Formatlı Şekilde Göstermek

Tarih bilgisini istediğimiz şekilde format ederek çıktı alabiliriz.

```ruby
t = Time.now
t.strftime("Bugün %d %B %Y, %A, saat: %H:%M") # => "Bugün 01 May 2015, Friday, saat: 12:35"
```

Dikkat ettiyseniz İngilizce olarak çıktıyı aldık. Ne yazık ki Ruby'de **locale** kavramı yok. Bu yüzden Türkçe çıktı almak için `I18n` gem'ini kullanmamız gerekiyor.

```bash
gem install i18n
```

Daha sonra herhangi bir dizin altında;

```bash
cd ~
mkdir i18n-works
cd i18n-works/
mkdir locales
cd locales/
curl -O https://raw.githubusercontent.com/svenfuchs/rails-i18n/master/rails/locale/tr.yml
cd ../..
touch run.rb
```

şimdi `run.rb` dosyası içine;

```ruby
require 'i18n'

I18n.load_path = Dir['./locales/*.yml']
I18n.locale = :tr
puts I18n.locale
t = Time.now
puts I18n.localize t, :format => "Bugün %d %B %Y, %A, saat: %H:%M"
```

yaptığımızda çıktı:

```
tr
Bugün 01 Mayıs 2015, Cuma, saat: 12:55
```

şeklinde olacaktır.

Kullanımda: `%<flags><width><modifier><conversion>` şeklinde bir yöntem bulunmakta.

**Flag'ler**

```
-  don't pad a numerical output
_  use spaces for padding
0  use zeros for padding
^  upcase the result string
#  change case
:  use colons for %z
```

Hemen örneklerle görelim, ilk olarak `-`, `_` ve `0` kullanımına bakalım:

```ruby
t = Time.now       # => 2015-05-02 11:35:26 +0300
t.strftime("%d")   # => "02"

# şimdi - ile yapıyoruz
t.strftime("%-d")  # => "2"   # 0'la doldurmadı...

# şimdi _ ile yapıyoruz
t.strftime("%_d")  # => " 2"  # SPACE karakteri ile doldurdu...

# şimdi 0 ile yapıyoruz
t.strftime("%0d")  # => " 02" # 0 ile doldurdu...
```

Şimdi `^`, `#` ve `:` flag'lerine bakalım:

```ruby
t.strftime("%A")        # => "Saturday"
t.strftime("%^A")       # => "SATURDAY" # upcase yaptı
t.strftime("%#A")       # => "SATURDAY" # changecase demek, upcase ise down, downcase ise up yapmak demek.
                        # saturday downcase geldi, upcase oldu
t.strftime("%z")        # => "+0300"
 t.strftime("%:z")      # => "+03:00" # : ile ayırdı
```

**width**

```ruby
t.strftime("%d")   # => "02"
t.strftime("%10d")   # => "0000000002" # 10 basamak yaptı.
```

**Formatlama**

| İşaret | Açıklama |
| -- | -- |
| %Y | 4 dijitli yıl |
| %C | yıl/100, yüzyıl için |
| %y | yıl mod 100, 2015 için **15** gelir. |
| %m | Yılın ayı. (01..12) |
| %B | Ayın tam adı (January) |
| %b ya da %h | Ayın kısa adı (Jan) |
| %d | Ayın günü, 0 eklemeli (01..31) |
| %e | Ayın günü, SPACE karakteri eklemeli ( 1..31) |
| %j | Yılın günü (001..366) |
| %H | Saat, 24-saat formatında 0 eklemeli (00..23) |
| %k | Saat, 24-saat formatında SPACE karakteri eklemeli ( 0..23) |
| %I | Saat, 12-saat formatında 0 eklemeli (01..12) |
| %l | Saat, 12-saat formatında SPACE karakteri eklemeli ( 1..12) |
| %P | Meridyen göstergeci, küçük harf (`am` ya da `pm`) |
| %p | Meridyen göstergeci, büyük harf (`AM` ya da `PM`) |
| %M | Dakika (00..59) |
| %S | Saniye (00..60) |
| %L | Milisaniye (000..999) |
| %N | Kesirli saniye, varsayılan 9 dijitli |
| %z | saat ve dakika ofsetli UTC zaman kuşağı (time zone) |
| %Z | Zaman kuşağının harfsel karşılığı |
| %A | Haftanın günü, tam yazım |
| %a | Haftanın günü, kısa yazım |
| %u | Haftanın kaçıncı günü, Pazartesi 1 |
| %w | Haftanın kaçıncı günü, Pazar 0 |
| %c | "%a %b %e %T %Y" şeklinde |
| %D | "%m/%d/%y" şeklinde |
| %F | ISO 8601 - "%Y-%m-%d" |
| %v | VMS - "%e-%^b-%4Y" |
| %x | %D ile aynı |
| %X | %T ile aynı |
| %r | 12 saat cinsinden saat "%I:%M:%S %p" |
| %R | 24 saat cinsinden saat "%H:%M" |
| %T | 23 saat cinsinden saat "%H:%M:%S" |


**Örnek**

```ruby
t = Time.now        # => 2015-05-16 14:29:43 +0300
t.strftime("%N")    # => "691659000"
t.strftime("%3N")   # => "691" # milisaniye, 3 dijit
t.strftime("%6N")   # => "691659" # mikrosaniye, 6 dijit

t.strftime("%z")    # => "+0300"
t.strftime("%Z")    # => "EEST"
                    #    Eastern European Summer Time yani
                    #    yaz saati :)

t.strftime("%A")    # Sunday
t.strftime("%a")    # Sun
t.strftime("%u")    # "7"
 t.strftime("%w")   # "0"
```

@wip

## Ruby paketleri: RubyGems

Ruby, benzeri diğer dillerdeki gibi kendine ait bir paket yöneticisine sahiptir. Ruby paketlerine **Gem** denir. Gem'ler aslında tekrar tekrar kullanılabilecek Ruby kodlarının paketlenmiş halleridir ve herhangi bir Ruby uygulamasına çok kolay entegre edilebilir.

Genelde tüm paketler http://rubygems.org sitesinde sunulur. İster local (*yerel*) ister özel repository isterseniz de RubyGems sitesinden bu paketleri kurabilirsiniz.

Ruby kurulumunda `gem` adında bir komut eklenir sisteme. Eğer `gem --help` derseniz, ilgili kullanımları ve komutları listeleyebilirsiniz.

RubyGems, efsane isim [Jim Weirich](http://en.wikipedia.org/wiki/Jim_Weirich) tarafından yazılmıştı. Kendisi 2014 Şubat'ta aramızdan ayrıldı.

Herhangi bir paketi kurmak için `gem install PAKET_ADI`şeklinde yazmak yeterli fakat genelde Ruby'nin kurulu olduğu yer sistem dosyalarının kurulu olduğu yerde olduğu için eğer Ruby versiyon yöneticisi (rvm ya da rbenv) kullanmıyorsanız `sudo` ile ile işlem yapmanız gerekir: `sudo gem install PAKET_ADI`.

Tavsiyem [Rbenv](https://github.com/sstephenson/rbenv) ya da [RVM](https://rvm.io/) gibi bir paket yöneticisi kullanmanız. Bir sonraki bölümde göreceğimiz [Bundler](http://bundler.io/) aracıyla hem sisteminizi gereksiz gem'lerden korumuş olacağız hem de istediğimiz projede istediğimiz gem versiyonunu kullanmış olacağız.

## Paket yöneticisi: Bundler

wip...

## Komut Satırı (Command-Line) Kullanımı

wip...

## Meta Programming

Ruby'deki **Meta Programming**, yazılan kodun **run-time**'da pek çok şeyi değiştirmesi, Kernel'dan gelen sistem fonksiyonlarını manipule etmesi (*Class, Module, Instance ile ilgili şeyler*) şeklindedir.

Hatta bazen yazdığınız programı restart etmeden bile kodu değişikliği yapmak mümkün olur.

Bazı komutlar, kullanım şekilleri gerçekten de çok tehlikeli olabilir! Özellikle dış dünyadan gelecek input'ların run-time'da yorumlanması pek de önerilen bir yöntem değildir. Yani burada göreceğimiz bazı yöntemleri bilelim ama gerçek dünyada pek fazla **uygulamayalım**!

### Class'lar Değiştirilebilir!

İster Kernel ister dışarıdan eklenen, her tür Class modifiye edilebilir:

```ruby
class String
  def foo
    "foo: #{self}"
  end
end


a = "hello"
a.foo # => "foo: hello"
```

`String` Class'ına kafamıza göre `foo` method'u ekledik.

### Class'ların Birden Fazla `initialize` Yöntemi Olabilir!

Bu `Class Overloading` yani Class ya da method'u ezmek olarak düşünülebilir. Class'ın bir tane `initialize` method'u olduğu için, koşullu olarak Class'a başlangıç seviyesinde müdahale edebiliriz:

```ruby
# Dortgen.new([sol_ust_x, sol_ust_y], boy, en)
# Dortgen.new([sol_ust_x, sol_ust_y], [sag_alt_x, sag_alt_y])
class Dortgen
  def initialize(*args)
    if args.size < 2  || args.size > 3
      "Bu sınıf en az 2 en fazla 3 parametre alır"
    else
      "Doğru parametre kullanımı"
    end
  end
end
Dortgen.new([0, 0], 10, 10)     # => #<Dortgen:0x007fe3ea1330f0>
Dortgen.new([0, 0], [10, 10])   # => #<Dortgen:0x007fe3ea132d58>
```

İster 2, ister 3 parametre ile `initialize` ettiğimiz `Dortgen` sınıfı, parametre kullanımına göre farklı çıktılar üretebilir.

### Anonim Class

Anonim Class'lar, **Singleton**, **Ghost** ya da **Metaclass** diye de adlandırılır. Aslında her Ruby Class'ı kendine ait anonim bir sınıfa ve method'lara sahiptir. Tek farkı kendisine ait olmasıdır.

```ruby
class Developer
  class << self
    def personality
      "Awesome"
    end
  end
end

Developer.new         # => #<Developer:0x007fc9048a2738>
Developer.personality # => "Awesome"

a = Developer.new
a                     # => #<Developer:0x007fc9048a2120>
a.class               # => Developer
a.class.personality   # => "Awesome"

a.personality         # => undefined method `personality' for #<Developer:0x007fd3ca0a0e10> (NoMethodError)
```

`Developer` sınıfının kendi `class`'ına anonim bir method taktık. Class'dan instance üretmeden `Developer.personality` şeklinde erişebilirken, `a` instance'ından gitmek istediğimizde yani `a.personality` dediğimizde hata mesajı aldık. Oysa o methods sadece `a.class` a ait :)

Yaptığımız iş aslında bir **Singleton** oluşturma oldu.

**define_method**

Class içinde **run-time** yani dinamik olarak method oluşturabilirsiniz:

```ruby
class Developer
  define_method :personality do |arg|
    "You are #{arg} developer!"
  end
end

Developer.new.personality("an awesome") # => "You are an awesome developer!"
a = Developer.new
a.personality("an awesome") # => "You are an awesome developer!"
a.class.instance_methods(false) # => [:personality]
```

**send**

`send` method'u `Object` sınıfından gelen bir method'dur. Sınıfa göndereceğimiz mesaj ilk parametre olup bu da aslında çağıracağımız method adıdır.

```ruby
class Developer
  def hello(*args)
    "Hello #{args.join(" ")}"
  end
end
d = Developer.new
d.send(:hello, "vigo", "how are you?") # => "Hello vigo how are you?"
```

Unutmayın, sadece `public` method'lara erişebilirsiniz!

**remove_method** ve **undef_method**

Adından da anlaşılacağı gibi method'u yoketmek için kullanılır ama eğer `remove_method` ile iptal edilmek istenilen method, türediği üst sınıfında var ise ne yazık ki yok edilemez. Bu durumda da `undef_method` devreye girer:

```ruby
class Developer
  def method_missing(m, *args, &block)
    "#{m} is not available!"
  end

  def hello
    "Hello from class Developer"
  end
end

class TurkishDeveloper < Developer
  def hello
    "Hello from class TurkishDeveloper"
  end
end

d = TurkishDeveloper.new
d.hello # => "Hello from class TurkishDeveloper"

class TurkishDeveloper
  remove_method :hello
end
d.hello # => "Hello from class Developer" # üst sınıfta varolduğu için çalıştı!
```

Eğer;

```ruby
class TurkishDeveloper
  undef_method :hello
end
d.hello # => "hello is not available!"
```

yaparsak, method komple uçar ve `method_missing` ile yakaladığımız kod bloğu çalışır.

**eval**

Pek çok programlama dilinde **evaluate** etmekten gelen, yani `String` formundaki metnin çalışabilir kod parçası haline gelmesi olayıdır `eval`:

```ruby
eval("5 + 5")            # => 10
eval('"Hello".downcase') # => "hello"
```

Aslında çok tehlikelidir. Yani programatik hiçbir kontrol olmadan dümdüz metnin **executable** hale getirilmesidir ve hiçbir zaman önerilmez. Güvenlik zafiyeti doğrurabilir.

**instance_eval**

Yazılan kod bloğunu sanki Class'ın bir method'uymuş gibi çalıştırır:

```ruby
class Developer
  def initialize
    @star = 10
  end
end

d = Developer.new
d.instance_eval do
  puts self
  puts @star
end

# #<Developer:0x007fe549a91e70>
# 10
```

ya da;

```ruby
class Developer
end
Developer.instance_eval do
  def who
    "vigo"
  end
end
Developer.who # => "vigo"
```

şeklinde kullanılır. Aynı şekilde sadece `public` olan method'lar için geçerlidir.

**module_eval** ve **class_eval**

İkisi de aynı işi yapar. Dışarıdan Class değişkenlerine erişmek için kullanılır:

```ruby
class Developer
  @@geek_rate = 10
end
Developer.class_eval("@@geek_rate") # => 10
```

Aynı şekilde method tanımlamak için;

```ruby
class Developer
end

Developer.class_eval do
  def who
    "vigo"
  end
end
Developer.new.who # => "vigo"
```

**class_variable_get** ve **class_variable_set**

Class konusunda Class ve Instance Variables arasındaki farkı görmüştük. Bu iki method yardımıyla sınıf değişkenine erişmek ve değerini değiştirmek mümkün:

```ruby
class Developer
  @@geek_rate = 10
end
Developer.class_variable_set(:@@geek_rate, "100") # => "100"
Developer.class_variable_get(:@@geek_rate)        # => "100"
```

**instance_variable_get** ve **instance_variable_set**

Aynı önceki gibi, bu method'lar da sadece Instance Variable için çalışır:

```ruby
class Developer
  def initialize(name, star)
    @name = name
    @star = star
  end

  def show
    "Name: #{@name}, Star: #{@star}"
  end
end

d = Developer.new("vigo", 10)
d.instance_variable_get(:@name) # => "vigo"
d.instance_variable_get(:@star) # => 10
d.show # => "Name: vigo, Star: 10"

d.instance_variable_set(:@name, "lego") # => "lego"
d.show # => "Name: lego, Star: 10"
```

**const_get** ve **const_set**

Constant yani sabitleri Class ve Module konusunda görmüştük. `const_set` ile Class'a sabit değer atıyoruz, `const_get` ile de ilgili değeri okuyoruz:

```ruby
class Box
end

Box.const_set("NAME", "web") # => "web"
Box.const_get("NAME")        # => "web"
Box::NAME                    # => "web"

a = Box.new
a.class.constants            # => [:NAME]
a.class::NAME                # => "web"
```
