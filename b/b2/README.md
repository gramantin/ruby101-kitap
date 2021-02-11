# Bölüm 2: Söz dizimi

## Syntax (Söz Dizimi) ve Rezerve Kelimeler

### Syntax (Söz Dizimi)
Genel olarak çok kolay ve anlaşılır bir **syntax**'a sahiptir. Sanki İngilizce okur/konuşur gibi söz dizimi bulunur. Örneğin,

> Eğer a'nın değeri 5'ten büyükse ekrana "Merhaba" yaz

demek istiyorsak;

```ruby
puts "Merhaba" if a > 5
```
şeklinde yazabiliriz.

Diğer dillerden farklı olarak, Ruby'de fonksiyon (method) çağırırken **parantez** kullanmak zorunluğu yoktur. Bu ilk etapta kafa karıştırıcı gibi dursa da, alışınca ne kadar kolay okunabilir olduğunu görüyorsunuz. Mecburi değil, yani parantez kullanmanızda sorun yok. Parantezli kullanım;

```ruby
def greet_user(user_name)
  puts "Merhaba #{user_name}"
end

greet_user("Uğur") # Merhaba Uğur
```

Eğer parantez kullanmazsak;

```ruby
def greet_user user_name
  puts "Merhaba #{user_name}"
end

greet_user "Uğur" # Merhaba Uğur
```

şeklinde olur. Keza, pek çok dilde, fonksiyon eğer bir şey dönerse geriye, mutlaka `return` komutu kullanılır. Ruby'de buna da gerek yok. Çünkü her method (_yani Fonksiyon_) mutlaka default olarak bir şey döner. Hiçbir şey dönmese bile `nil` döner. Bu bakımdan da;

```ruby
def greet_user user_name
  "Merhaba #{user_name}"
end

puts greet_user "Uğur" # Merhaba Uğur
```
şeklinde kullanabiliriz. Korkmayın, kafalar karışmasın. Detaylara ileride gireceğiz.


### Comments (Yorum Satırları)
Her dilde olduğu gibi **Comment out** yani "işaretli kısmı çalıştırma" demek için kullandığımız şey Ruby'de de var.

**Comment** için `#` işareti kullanılıyor. Genelde `line-comment` yani satır bazlı, ve `block-comment` yani kod bloğu bazlı yorum yapma şekilleri var.

```ruby
# Bu satır line-comment, yani tek satırlı yorum

# Bu kısım block-comment
#
# def test_user
#   true
# end

# ya da

=begin
Bu yorum...
Bu da yorum...
Hatta bu da...
=end
```

Gördüğünüz gibi `block-comment` için ilave olarak `=begin` ve `=end` kelimeleri kullanılabiliyor.


### Rezerve Edilmiş Kelimeler
Her dilde olduğu gibi Ruby'de de daha önceden rezerve edilmiş bazı kelimeler bulunmaktadır. Bu kelimeleri değişken ya da method adı olarak kullanamıyoruz. Bu kelimeler Ruby komutları ve özel durumlar için rezerve edilmiş.

| Kelime | Kelime |
| -- | -- |
| BEGIN | next |
| END | nil |
| alias  | not |
| and  | or |
| begin  | redo |
| break  | rescue |
| case  | retry |
| class  | return |
| def  | self |
| defined?  | super |
| do  | then |
| else  | true |
| elsif  | undef |
| end  | unless |
| ensure  | until |
| false  | when |
| for  | while |
| if  | yield |
| in  | \_\_FILE\_\_ |
| module  | \_\_LINE\_\_ |

## Değişkenler

Değişken denen şey, yani **Variable**, nesneye atanmış bir işaretçidir aslında. Ne demiştik? Ruby'de her şey bir nesne yani **Object**. Bu nesnelere erişmek için aracıdır değişkenler.

Farklı farklı türleri vardır. Birazdan bunlara değineceğiz. En basit anlamda değişken tanımlamak;

```ruby
a = 5
user_email = "example@foo.com"
```

şeklinde olur. Yukarıdaki örnekte `a` ve `user_email` değişkenin adıdır. Değeri ise `eşittir` işaretinden sonra gelendir.

Yani yukarıda; `a` ya sayı olarak `5` ve `user_email` e metin olarak `example@foo.com` değerleri atanmıştır.

Ruby **Duck Typing** şeklinde çalışır. Yani atama yapmadan önce ne tür bir değer ataması yapacağımızı belirtmemize gerek yok. Ruby zaten `a = 5` dediğimizde, "Hmmm, bu değer Fixnum türünde" diye değerlendirir.

[Duck Typing](http://en.wikipedia.org/wiki/Duck_typing) demek şudur; Eğer ördek gibi yürüyorsa, ördek gibi ses çıkartıyorsa e o zaman bu bir Ördektir! İngilizcesi;

> When I see a bird that walks like a duck and swims like a duck and quacks like a duck, I call that bird a duck

Yani, bir kuş, eğer ördek gibi yürüyorsa, ördek gibi yüzüyorsa ve ördek gibi ses çıkarıyorsa ben buna Ördek derim!

### Metinsel Atamalar ve Tırnak Kullanımı
Yeri gelmişken hızlıca bir konuya değinmek istiyorum. Metinsel değişkenler tanımlarken (**String**) eşitlik esnasında tek ya da çift tırnak işareti kullanabiliriz. Fakat aradaki farkı bilerek kullanmamız gerekir.

String içinde değişken kullanımı yaptığımız zaman yani;

```ruby
a = 41
puts "Siz tam #{a} yaşındasınız"
```

gibi bir durumda, gördüğünüz gibi `#{a}` şeklinde yazı içinde değişken kullandık. Bu kodun çıktısı aşağıdaki gibi olacak. 

    Siz tam 41 yaşındasınız

Format olarak Ruby'de, `#{BU KISIMDA KOD ÇALIŞIR}` şeklinde istediğimiz kodu çalıştırma yetkimiz var. Bu işlem sadece **çift tırnak** kullanımında geçerlidir. Başka bir örnek vermek gerekirse; 

```ruby
a = 41
puts "Siz tam #{a+2} yaşındasınız"
```

Yukarıdaki örnekte Ruby `a+2` komutunu çalıştıracaktır ve sonuç olarak `43` değerini bulacaktır ve bunu ekrana yazdıracak. Yani sonuç:

    Siz tam 43 yaşındasınız

şeklinde olacaktır. Ancak aynı kodu tek tırnak kullanarak yapmış olsaydık;

```ruby
a = 41
puts 'Siz tam #{a} yaşındasınız'
```

çıktısı:

    Siz tam #{a} yaşındasınız

olacaktı. **Tek tırnak** içinde bu işlem çalışmaz!


### Local (Bölgesel)
Bölgesel ya da **Yerel** değişkenler, bir **scope** içindeki değişkenlerdir. Scope nedir? Kodun çalıştığı bölge olarak tanımlayabiliriz. Bu tür değişkenler mutlaka küçük harfle ya da `_` (_underscore_) işareti ile başlamalıdır. Kesinlike `@`, `@@` ya da `$` işareti gibi ön ekler alamazlar.

```ruby
out_text = "vigo"
def greet_user(user_name)
  out_text = "Merhaba #{user_name}"
end

puts greet_user("vigo")  # Merhaba vigo
puts out_text            # vigo
```

Program çalıştığında `out_text` değişkeninin değeri `vigo` olarak atanmaktadır. Daha sonra 6. satırda `greet_user` method'u (_fonksiyonu_) çalıştığında, o methodun içerisinde `out_text` değişkeninin değeri değiştiriliyor gibi görünüyor. Daha sonra 7. satırda `out_text` değişkeninin değeri `puts` methodu ile ekrana yazdırılmaktadır. Ancak burada çıktılara baktığınızda methodun içerisindeki `out_text` değişkenindeki değişim, programın en başında tanımladığımız `out_text` değişkenin değerini etkilememiştir. Method içerisinde kalmıştır. Burada method içerisindeki `out_text` değişkeni aslında `local variable` yani yerel değişken şeklinde çalışmaktadır.


### Global (Genel)
`$` işaretiyle başlayan tüm değişkenler **Global** değişkenlerdir. Kodun herhangi bir yerinde kullanılabilir ve erişilebilir.

```ruby
$today = "Pazartesi"
def greet_user(user_name)
  out_text = "Merhaba #{user_name}, bugün #{$today}"
  puts out_text
end

puts "Bugün günlerden ne? #{$today}"
greet_user("vigo")  # Merhaba vigo, bugün Pazartesi
```

Bu örnekteki **Global** değişken `$today` değişkenidir.


### Constants (Sabitler)
Sabit nedir? Değiştirelemeyendir. Yani bu tür değişkenler, ki bu değişken değildir :), **sabit** olarak adlandırılır. Kural olarak mutlaka **BÜYÜK HARF**'le başlar! Bazen de tamamen büyük harflerden oluşur.

```ruby
My_Age = 18
your_age = 22

puts defined?(My_Age)    # constant
puts defined?(your_age)  # local-variable
```

`My_Age` sabit, `your_age` de yerel değişken...

Ruby'de ilginç bir durum daha var. Constant'lar **mutable** yani değiştirilebilir. Nasıl yani?

```ruby
My_Age = 18

puts defined?(My_Age)    # constant
puts "My_Age: #{My_Age}" # My_Age: 18

My_Age = 22

puts defined?(My_Age)    # constant
puts "My_Age: #{My_Age}" # My_Age: 22
```

ama `warning` yani **uyarı mesajı** aldık!

    untitled:6: warning: already initialized constant My_Age
    untitled:1: warning: previous definition of My_Age was here

`My_Age` sabiti 6.satırda zaten tanımlıydı. Önceki değeri de 1.satırda diye bize ikaz etti Ruby yorumlayıcısı.


### Paralel Atama
Hemen ne demek istediğimi bir örnekle açayım:

```ruby
x, y, z = 5, 11, 88
puts x # 5
puts y # 11
puts z # 88

a, b, c = "Uğur", 5.81, 1972
puts a # Uğur
puts b # 5.81
puts c # 1972
```

`x, y, z = 5, 11, 88` derken tek harekette `x = 5`, `y = 11` ve `z = 88` yaptık. İşte bu paralel atama.


### Instance Variable
**Instance** dediğimiz şey **Class**'dan türemiş olan nesnedir. Bu konuyu detaylı olarak ileride inceleyeceğiz. Sadece ön bilgi olması adına değiniyorum. `@` işareti ile başlarlar.

```ruby
class User
  attr_accessor :name
  def initialize(name)
    @name = name
  end

  def greet
    "Merhaba #{@name}"
  end
end

u = User.new("Uğur")
puts u.greet         # Merhaba Uğur
puts u.name          # Uğur
```

`u.name` diye çağırdığımız şey `User` class'ından türemiş olan `u` nesnesinin **Instance Variable**'ı yani türemiş nesneye ait değişkenidir. Fazla takılmayın, `Class` konusunda bol bol değineceğiz...


### Class Variable
**Class**'a ait değişkendir. Dikkat edin burada türeyen bir şey yok. `@@` ile başlar. Kullanmadan önce bu değişkeni mutlaka `init` etmelisiniz (_Yani ön tanımlama yapmalısınız_)

```ruby
class User
  attr_accessor :name
  @@instance_count = 0   # Kullanmadan önce init ettim
  def initialize(name)
    @name = name
    @@instance_count += 1 # Class'dan her instance oluşmasında sayacı 1 arttırıyorum
  end

  def greet
    "Merhaba #{@name}"
  end

  def self.instance_count # burası öneli
    @@instance_count
  end
end

user1 = User.new("Uğur")
user2 = User.new("Ezel")
user3 = User.new("Yeşim")

puts "Kaç defa User instance'ı oldu? #{User.instance_count}" # Kaç defa User instance'ı oldu? 3
```

Eğer kafanız karıştıysa sorun değil, **Class** konusunda hepsinin üzerinden tekrar geçeceğiz.

## Ön Tanımlı Değişkenler
Ruby, bir kısım ön tanımlı yani **Predefined** değişkenlerle geliyor. Değişkenlerin ne olduğunu;

```ruby
puts global_variables
```
şeklinde görebiliriz. Önden bu bilgileri vermek zorundayım, daha geniş kullanımı ve tam olarak ne işe yaradıklarını ileriki bölümlerde daha iyi anlayacaksınız.

| Değişken | Açıklama |
| -- | -- |
| $! | `raise` ile atanan **exception** bilgisidir. `rescue`ile erişilir. |
| $@ | Son **exception**'a ait **backtrace** (_bir tür log_) bilgilerinin tutulduğu dizi (_Array_) |
| $& | Son yakalanan **match**'in tutulduğu **String** (_Regex konusunda göreceğiz_) |
| $\` | Son yakalanan **match**'in solunda kalan kısım |
| $' | Son yakalanan **match**'in sağında kalan kısım |
| $+ | En yüksek **group match**'in tutulduğu yer. (_Regex yaparken group yakalama konusunda göreceğiz_) |
| $1, $2, ..., $9 | Yine, Regex ile patern yakalama (_pattern matching_) yaptığımızda, yakaladığımız şeylerin sıra numarası. |
| $~ | O anki kapsama alanında (_scope_) son yakalananla ilgili bilgilerin tutulduğu değişken |
| $= | Regex ile uğraşırken, karakterlerin büyük/küçük harfe duyarlılığı ile ilgi ayarlar vardır. Örneğin büyük/küçük harf farkı olmadan aramak yaparken (_case insensitive_) bu değişkene atama yaparız. Varsayılan değer (_default_) `nil`'dir |
| $/ | Dosyadan okuma yapılırken satırların nasıl ayrıldığının tespit edildiği değişkendir. Eğer `nil` olarak atarnırsa, dosya okuması esnasında satır-satır okuma yerine tüm dosya bir anda okunur. |
| $\ | Bu da çıktı için ayraçtır. `print`ve `puts` gibi komutlarda `IO#write` gibi işlemlerde kullanılır. Varsayılan değer `nil`'dir |
| $, | `print` ve `Array#join` de kullanılan ayraçtır. |
| $; | `String#split` de kullanılan ayraçtır. |
| $. | Dosya işlemlerinde son okunan dosyanın aktif satır numarasını verir. |
| $< | Aynı `shell` deki ekleme (_concatenation_) işlemi gibi sanal ekleme yapar. |
| $> | `print` ve `printf` işlemi için varsayılan çıktıdır. Varsayılan değeri de `$stdout` |
| $_ | `gets` veya `readline` ile alınan son satırdır, cinsi **String**'dir. |
| $0 | Çalıştırılan script'in dosya adıdır. |
| $* | Komut satırı işlemlerinde, dosyaya geçilen argümanların saklandığı değişkendir. |
| $$ | Çalıştırılan script'in işlem numarası (_Process Number_) |
| $? | Çalıştırılan son alt işlemin (_Child Process_) durumu. |
| $: | Modüller ve ek dosyalar için **Path** (_Load Path__) bilgisi. `require` komutunda göreceğiz. |
| $" | `require` ile yüklenen dosyaların adlarının tutulduğu dizi (_Array_) |
| $DEBUG | Adından da anlaşıldığı gibi, eğer **DEBUG** modda çalıştırma yapıyorsak (_ki bunu -d ile yaparız_) oluşan her **exception**'ın `$stderr` değişkenine atanmasını sağlar. |
| $KCODE | Kod yazdığımız script dosyasının **encoding** tipini seçmemize yarar. Son sürümlerde ihtiyaç kalmadı, her şey **default** olarak `UTF-8` çalışıyor. |
| $FILENAME | Komut satırından argüman olarak dosya geçtiğimizde geçilen dosyanın adını almak için kullanılır. Aslında `ARGF.filename` ile aynı işi yapar. |
| $LOAD_PATH | `$:` ile aynı işi yapan **alias**'dır (_alias = takma ad_) |
| $SAFE | Güvenlik seviyesidir. Varsayılan değer `0` dır. Bu dereceler 0'dan 4'e kadardır. Kod güvenliği ve kilitleme yapmak için kullanılır. Biraz karmaşık bir konudur :) Örneğin, emin olmadığınız bir kütüphane kullanırken kodunuzu güvenli hale getirmek için, kod bloğunun önüne `$SAFE=4` ekerseniz, takip eden kod `array` `hash` ve `string` lerde hiç bir modifikasyon yapamaz! Hatta pek [çok şeyi](http://phrogz.net/programmingruby/taint.html) yapamaz! |
| $stdin | Standart giriş. |
| $stdout | Standart çıkış. |
| $stderr | Giriş/Çıkış hata bildirimi. |
| $VERBOSE | Kernel tarafından üretilen tüm uyarı mesajlarının (_warning gibi..._) görüntülenmesi için kullanılır. |
| $-0 | `$/` ile aynı işi yapar. |
| $-a | Komut satırından çalıştırma yaparkan `-a` ataması yapılmışsa `$-a` **true** döner. |
| $-d | `$DEBUG` ile aynı işi yapar. |
| $-F | `$;` ile aynı işi yapar. |
| $-i | Bu değişken **in-place-edit** modda **extension**'ı saklar. |
| $-I | `$:` ile aynı işi yapar. (_Büyük i_) |
| $-l | Eğer `-lis` set edilmişse `true` döner. **Read Only** yani sadece okunur, değeri değiştirilemez! (_Küçük l_) |
| $-p | Eğer `-pis` set edilmişse `true` döner. **Read Only** yani sadece okunur, değeri değiştirilemez! |
| $-K | `$KCODE` ile aynı işi yapar. |
| $-v | `$VERBOSE` ile aynı işi yapar. |
| $-w | Eğer `-w` set edilmişse `true` döner. |
| $-W | **Warning Level** yani oluşabilecek hatalar vs ile ilgili 0, 1 ya da 2.seviyede uyarı mesajları göstermek için. |
| $LOADED_FEATURES | `$"` ile aynı işi yapar. |
| $PROGRAM_NAME | `$0` ile aynı işi yapar. |


## Pseudo (Gerçek Olmayan) Değişkenler
**Değişken** (_Variable_) gibi görünen ama **Sabit** (_Constant_) gibi davranan ve kesinlikle **değer ataması yapılamayan** şeylerdir.

| Değişken | Açıklama |
| -- | -- |
| self | Alıcı nesnenin o anki aktif method’u. Yani bu bir **Class** ise kendisi...  |
| nil | Tanımı olmayan (_Undefined_) şeylerin değeri. |
| true | Mantıksal (_Boolean_) işlem, anlayacağınız gibi **true** yani **1** |
| false | **true**'nun tersi (_Boolean_) yani **0** |
| \_\_FILE\_\_ | Çalışan kaynak kod dosyasının adı |
| \_\_LINE\_\_ | Çalışan kaynak kod dosyasındaki, o anki aktif satırın numarası |

## Operatörler

Operatörler çeşitli kontrolleri yapmak için kullanılır. Hatta bazıları aynı zamanda **method** olarak çalışır. Bazı operatörlerin birden farklı işlemi vardır. Örneğin `+` hem matematik işlemi olan **toplama** için hem de **pozitif** değer kontrolü için kullanılabilir.

| Operatör | Açıklama | Method mu? |
| -- | -- | -- |
| :: | İki tane iki nokta. **Scope resolution** anlamındadır. Class ve Modül konusunda detayları göreceğiz. | |
| [] | Referans, set | Evet |
| []= | Referans, set | Evet |
| ** | Üssü, kuvveti | Evet |
| + | Pozitif | Evet |
| - | Negatif | Evet |
| ! | Mantıksal uzlaşma |  |
| ~ | Tamamlayıcı | Evet |
| * | Çarpma | Evet |
| / | Bölme | Evet |
| % | Modülo (_Kalan_) | Evet |
| + | Ekleme | Evet |
| - | Çıkartma | Evet |
| << | Sola kaydır | Evet |
| >> | Sağa kaydır | Evet |
| & | **Bit** seviyesinde **and** (_Ve_) işlemi | Evet |
| &#124; | **Bit** seviyesinde **or** (_Veya_) işlemi | Evet |
| ^ | **Bit** seviyesinde **exclusive or** (_Veya'nın tersi gibi_) işlemi | Evet |
| > | Büyüktür | Evet |
| >= | Büyük ve eşit | Evet |
| < | Küçüktür | Evet |
| <= | Küçük ve eşit | Evet |
| <=> | Eşitlik karşılaştırma operatörü (_Spaceship_ yani uzay gemisi ) | Evet |
| == | Eşitlik | Evet |
| === | Denklik | Evet |
| != | Eşit değil |  |
| !~ | Yakalanmayan (_not match_) |   |
| =~ | Yakalanan (_match_) | Evet |
| && | Mantıksal ve (_and_) |  |
| &#124;&#124; | Mantıksal veya (_or_) |  |
| .. | Range'i kapsayan | Evet |
| ... | Range'i kapsamayan |  |
| ? : | **Ternary** |  |
| = | Atama |  |
| += | Arttırma ve atama |  |
| -= | Eksiltme ve tama |  |
| *= | Çarpma ve atama |  |
| /= | Bölme ve atama |  |
| %= | Modülo ve atama |  |
| **= | Üssü ve atama |  |
| <<= | Bit seviyesinde sola kaydırma ve atama |  |
| >>= | Bit seviyesinde sağa kaydırma ve atama |  |
| &= | Bit seviyesinde **and** ve atma |  |
| &#124;= | Bit seviyesinde **or** ve atama |  |
| ^= | Bit seviyesinde **eor** ve atama |  |
| &&= | Mantıksal **and** ve atama |  |
| &#124;&#124;= | Varlık ataması (_Existential Operator_) | &#20; |

İlk bakışta insanın aklını durduran bir sürü garip işaretler bunlar değil mi? Hemen örneklerle pekiştirelim.

```ruby
a = []
a.class  # => Array
a.length # => 0
```

### `[]=` Kullanım Örneği

```ruby
a = []             # Bu bir dizi / Array
a.[]=5,"Merhaba"   # 0 indekli, 5.eleman Merhaba olsun
p a                # [nil, nil, nil, nil, nil, "Merhaba"]
```

### Unary Operatörleri

**Unary** demek, `+=`, `-=`, `*=` gibi işlemleri yaptığımız operatörler. Yani `x+= 5` dediğimizde (_x'in değerine 5 ekle ve sonucu tekrar x'e ata_) aslında **Unary** operatörü kullanmış oluruz.

Keza, aşağıdaki örnekteki gibi kullanımlarda ek fayda sağlamış oluruz:

```ruby
str = "Merhaba Dünya"

class String
  def -@
    reverse
  end
end

p str     # "Merhaba Dünya"
p -str    # "aynüD abahreM"
```

## Global Constants (Genel Sabitler)

Ruby, ön tanımlı olarak çeşitli sabitlerle birlikte geliyor. Ben an itibariyle Mac OSX üzerinde, `rbenv` ile `ruby 2.1.0` kullanıyorum. Bu bağlamda sizin kullandığınız Ruby versiyonuna göre değişkenlikler olabilir.

| Sabit | Değeri |
| -- | -- |
| TRUE | Anlaşılacağı gibi bu **true** değeri |
| FALSE | Bu da **false** değeri |
| NIL | **nil** |
| STDIN | Standart giriş. `$stdin` için varsayılan değer. |
| STDOUT | Standart çıkış. `$stdout` için varsayılan değer. |
| STDERR | Standart hata. `$stderr` için varsayılan değer. |
| ENV | Aktif çevre değişkenlerinin (_Environment Variables_) bulunduğu **Hash** |
| ARGF | `$<` ile aynı işi yapıyor. |
| ARGV | `$*` ile aynı işi yapıyor. |
| DATA | Herhangi bir Ruby script dosyasında, `__END__` sonrasına yazılan şeylerin saklandığı değişken. |
| RUBY_VERSION | "2.1.0" |
| RUBY_RELEASE_DATE | "2013-12-25" |
| RUBY_PLATFORM | "x86_64-darwin13.0" |
| RUBY_COPYRIGHT | "ruby - Copyright (C) 1993-2013 Yukihiro Matsumoto" |
| RUBY_DESCRIPTION | "ruby 2.1.0p0 (2013-12-25 revision 44422) [x86_64-darwin13.0]" |
| RUBY_ENGINE | "ruby" |
| RUBY_PATCHLEVEL | "0" |
| RUBY_REVISION | 44422 |
