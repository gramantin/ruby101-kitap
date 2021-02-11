# Bölüm 3: Metodlar

## Methods (Fonksiyonlar)

Programlama parçacıklarının ve/veya ifadelerin bir araya toplandığı şeydir method. Aslında lise matematiğinden hepimiz aşinayız.

Bildiğiniz matematik fonksiyonu. Bundan böyle fonksiyon yerine **method** kullanacağım. Çünkü Ruby demek neredeyse **Obje** (_Nesne_) ve **Method** (_Method_) demek.

Önceki konularda gördüğümüz operatörlerin neredeyse hepsi bir method! Hemen **Method Definition**'a yani nasıl method tanımlandığına bir göz atalım.

```ruby
def merhaba
  "Merhaba"
end

merhaba        # => "Merhaba"
```

`def` ve `end` anahtar kelimeleri arasına method’un adı geldi. Önce method’u tanımladık, sonra çağırdık.

Ruby'de her şey mutlaka **geriye bir şey döner**. Ne demek bu? Prensip olarak method’lar zincir olarak çalıştığı için, method denen şey de aslında bir fonksiyon ve fonksiyon denen şey de bir dizi işlemin yapılıp geriye sonucun dönüldüğü bir taşıyıcı aslında.

Bir örnek vermek için hemen `irb` ye geçelim:

    irb(main):001:0> puts "Merhaba"
    Merhaba
    => nil
    irb(main):002:0>


`puts "Merhaba"` önce işini yaptı ve çıktı olarak **Merhaba** verdi. sonra `=> nil` dikkatinizi çekti mi?

Çünkü `puts` method’u işini yaptı bitirdi ve geriye `nil` döndü! Peki daha önceki programlama tecrübelerimize dayanak, **geriye döndü** işini hangi komut yapmış olabilir?

Pek çok dilde fonksiyondan bir şey geri dönmek için **return** kelimesi kullanılır. Ruby'de de kullanılır ama zorunlu değildir. Yukarıdaki `def merhaba` örneğinde `return` kullanmamamıza rağmen geriye **Merhaba** dönebildi.

Ruby'de methodlar içerisindeki çalıştırılan en son satırın değerini döndürür. Ancak siz daha öncesinde özellikle `return` anahtar kelimesi ile bir sonuç dönmezseniz. Bir örnekle konuyu pekiştirelim.

```ruby
def merhabaBir
  m = "Merhaba"
  n = "Ornek"
end

def merhabaIki
  m = "Merhaba"
  n = "Ornek"
  return "Return Ornek"
end

puts merhabaBir   # Sonuç : Ornek
puts merhabaIki   # Sonuç : Return Ornek
```

İşte bu Ruby'nin özelliği. Kodu okurken bunu bilmezsek kafamız süper karışabilir.

`def` ile tanımlanan method’u, `undef` ile yok edebilirsiniz.

```ruby
def merhaba
  "Merhaba"
end

merhaba # => "Merhaba"

undef merhaba

merhaba # =>
# ~> -:9:in `<main>': undefined local variable or method `merhaba' for main:Object (NameError)
```

Gördüğünüz gibi `undefined local variable or method .. Object (NameError)` hatasını aldık.

Method’lar argüman alabilir. Yani fonksiyona, doğal olarak, parametre geçebilirsiniz.

```ruby
def merhaba(isim)
  "Merhaba #{isim}"
end

merhaba("vigo") # => "Merhaba vigo"
```

aynı örneği aşağıdaki gibi de yazabiliriz:

```ruby
def merhaba isim
  "Merhaba #{isim}"
end

merhaba "vigo" # => "Merhaba vigo"
```

Method’u tanımlarken ve çağırırken **parantez** kullanmadık! Bu alışmanız gereken önemli konulardan birisi. Şahsen ben, daha önce hiçbir programlama dilinde böyle bir şey görmedim!

Bazı durumlara, argüman alan method çağırırken, argümanın tipine göre, eğer parantez kullanmadan çağırma yaparsanız **warning** alabilirsiniz!

### Method Yazma Kuralları (_Method Conventions_)

Ruby, pek çok konuda rahat gibi görünse bile bazı kuralları var tabi. Özellikle method’ların son karakteri ile ilgili. Eğer bir method’un son karakteri `?` ise bu o method’un `true` ya da `false` yani **Boolean** bir değer döneceğini ifade eder.

```ruby
a = "ali"
b = "ali"

a.eql? b  # => true
a.eql?(b) # => true
```

`.eql?` method’u eşitliği kontrol eder ve mutlaka sonuç **Boolean** döner.

Eğer method’un son karakteri `!` (_Ünlem_) ise; bu, o method’un tehlikeli bir iş yaptığını anlatır. Yani ilgili nesnenin kopyalanmadan direk üzerinde değişiklik yapacağı anlamına gelir.

```ruby
a = "deneme"

a.upcase   # => "DENEME"
a          # => "deneme"

a.upcase!  # => "DENEME"
a          # => "DENEME"
```

`a` değeri **deneme**. `.upcase` ile orijinal değeri değiştirmeden **uppercase** (_Büyük harf_) yaptık. Değeri kontrol ettiğimizde halen küçük harf olduğunu gördük. `.upcase!` kullandığımız anda değişkenin orijinal değerini de bozduk.

Eğer bir method `=` ile bitiyorsa, bu, o method’un bir **setter** method’u olduğu anlamına gelir ve **Class** ile ilgili bir konudur.

```ruby
class User
  def email=(email)
    @email = email
  end
end

u = User.new
u # => #<User:0x007ff7229ed880>

u.email = "vigo@xyz.com"
u # => #<User:0x007ff7229ed880 @email="vigo@xyz.com">
```

### Varsayılan Argümanlar (_Default Arguments_)

Method argümanlarına varsayılan değerler atayabilirsiniz. Bu, eğer methoda gönderilmesi beklenen argüman gelmemişse otomatik olarak değer atama yapmayı sağlar.

```ruby
def merhaba(isim="insalık!")
  "Merhaba #{isim}"
end

merhaba          # => "Merhaba insalık!"
merhaba("vigo")  # => "Merhaba vigo"
```

Parametre geçmeden çağırdığımızda, tanımladığımız varsayılan (_default_) değer atandı.

### Değişken Sayıda Argümanlar (_Variable-Length Arguments_)

Bazı durumlarda method’a değişken sayıda olarak parametre geçmek gerekebilir. Bu durumda argümanın başına `*` işareti gelir. Bu sayede o argüman artık bir dizi (_Array_) haline gelir.

```ruby
def merhaba(*isimler)
  "Merhaba #{isimler.join(" ve ")}"
end

merhaba("vigo")                        # => "Merhaba vigo"
merhaba("vigo", "yeşim", "ezel")       # => "Merhaba vigo ve yeşim ve ezel"

merhaba "dünya", "uzay", "evren", "ay" # => "Merhaba dünya ve uzay ve evren ve ay"
```

Keza, şu şekilde de kullanılabilir:

```ruby
def custom_numbers(first, second, *others)
  puts "ilk sayı: #{first}"
  puts "ikinci sayı : #{second}"
  puts "diğer sayılar : #{others.join(",")}"
end

custom_numbers 1,2,50,100 # => nil
# >> ilk sayı: 1
# >> ikinci sayı : 2
# >> diğer sayılar : 50,100
```

### Method’a Takma İsim Vermek (_Aliasing_)

Varolan bir method’u, başka bir isimle çağırmak. Bu aslında **Class** konusuyla çok ilintili ama kısaca değinmek istiyorum.

```ruby
def merhaba(isim)
  "Merhaba! #{isim}"
end

alias naber merhaba

merhaba "Uğur" # => "Merhaba! Uğur"
naber "Uğur"   # => "Merhaba! Uğur"
```

Formül şu: `alias` `TAKMA AD` `ORİJİNAL` yani `alias naber merhaba` derken, `merhaba` method’una takma ad olarak `naber`i tanımladık!


#### Unutma!

* `return` kullanmadan method’dan geri dönüş yapılabilir
* Parantez kullanmadan method tanımlanabilir
* Parantez kullanmadan method çağırılıp parametre geçilebilir.
* `?` ile biten method mutlaka `true` ya da `false` döner.
* `!` ile biten orijinal değeri mutlaka değiştirir.
* `=` ile biten **setter**'dır.

## Blocks (Bloklar)

Blok olayı, bence Ruby'nin en çılgın özelliklerinden biri. Aslında bu konu, neredeyse başlı başına bir kitap konusu olabilir. Genelde **Block, Proc, Lambda** üçlemesi olarak anlatılır.

Kitabımız 101 yani giriş seviyesinde olduğu için, kafaları minimum karıştırma adına basit şekilde değineceğim.

Blok'lar, genelde [Closure](http://en.wikipedia.org/wiki/Closure_(computer_programming) ya da [Anonim](http://en.wikipedia.org/wiki/Anonymous_function) fonksiyonlar olarak tanımlanır. Sanki method içinde başka bir method’u işaret eden ya da değişkenleri method’lar arasında paylaşan kapalı bir ortam gibidirler.

Genelde ya `{` `}` ile ya da `do/end` ile sarmalanmışlardır.

```ruby
family_members = ["Yeşim", "Ezel", "Uğur", "Ömer"]

family_members.each do |member_name|
  puts member_name
end

# Yeşim
# Ezel
# Uğur
# Ömer
```

Aynı kod:

```ruby
family_members = ["Yeşim", "Ezel", "Uğur", "Ömer"]

family_members.each { |member_name| puts member_name }

# Yeşim
# Ezel
# Uğur
# Ömer
```

şeklinde de yazılabilirdi. `do/end` ya da `{}` arasında kalan kısım **Block** kısmıdır.

`family_members` bir **Array** yani dizidir. Eğer `puts family_members.class` dersek bize `Array` oldunu söyler. Array'in `each` method’u bize block işleme şansını sağlar.

Komut satırında `ri Array#each` dersek bize Array'in **each** method’uyla ilgili tüm bilgiler gelir.

`do` komutundan hemen sonra gelen `|member_name|` bizim kafamıza göre tanımladığımız bir değişkendir ve Array'in her elemanı bu değişkene atanır.

**Enumeration** bölümünde bunlardan sıkça bahsedeceğiz. Blockların esas gücü `yield` olayından gelir. Hemen bir örnek verelim:

```ruby
def test_function
  yield
end

test_function {
  puts "Merhaba"
}

# Merhaba

test_function do
  puts "Ben block içinden geliyorum"
end

# Ben block içinden geliyorum

test_function do
  [1, 2, 3, 4].each do |n|
    puts "Sayı #{n}"
  end
end

# Sayı 1
# Sayı 2
# Sayı 3
# Sayı 4
```

`test_function` adında bir fonksiyonum var (_yani method’um var_) Hiç parametre almıyor! ama **Block** alıyor. İlkinde **curly brace** ile (_yani_ `{` _ve_ `}`), ikincisinde `do/end` ile, son örnekte `do/end` ile ve iç kısımda başka bir iterasyonla kullandım.

Kabaca, fonksiyona kod bloğu geçtim.

Peki, ya şu şekilde kullansaydım `test_function` ? yani hiçbir şey geçmeden? Alacağım hata mesajı:

    no block given (yield)

olacaktı. Demek ki block geçilip geçilmediğini anlamanın bir yolu var :)

```ruby
def test_function
  if block_given?
    yield
  else
    puts "Lütfen bana block veriniz!"
  end
end
```

`block_given?` ile bu kontrolü yapabiliyoruz. Şimdi biraz daha kafa karıştıralım :)

```ruby
def numerator
  yield 10
  yield 4
  yield 8
end

numerator do |number|
  puts "Geçilen sayı #{number}"
end
```

Örnekte, `yield` block içinden gelen kod bloğunu bir **fonksiyon** gibi çağırıyor, çağırırken de bloğa **parametre** geçiyor. Dikkat ettiyseniz kaç tane `yield` varsa o kadar kez block çağırıldı (_call edildi__)

```ruby
def print_users
  ["Uğur", "Yeşim", "Ezel"].each do |name|
    yield name
  end
end

print_users do |name|
  puts "Kullanıcı Adı: #{name}"
end

# Kullanıcı Adı: Uğur
# Kullanıcı Adı: Yeşim
# Kullanıcı Adı: Ezel
```

Fonksiyon içine fonksiyon geçtik gibi.

**Enumeration** / **Number** bölümünde de göreceğiz ama hemen hızlı bir-iki örnek vermek istiyorum blok kullanımıyla ilişkili.

```ruby
5.times { puts "Merhaba" }
5.times { |i| puts "Sayı #{i}" }
5.times do |i|
  puts "Sayı #{i}"
end
```

Not: Aslında `times` sayılara ait bir method ve yukarıdaki örneklerde gördüğünüz gibi blok geçebiliyoruz kendisine.

## Proc ve Lambda

**Block** kullanımını daha dinamik ve programatik hale getirmek için **Procedures** ya da genel adıyla **Procs**, object-oriented Ruby'nin vazgeçilmezlerindendir.

Bazen, her seferinde aynı bloğu sürekli geçmek gerektiğinde imdadımıza **Proc** yetişiyor. Nasıl mı?

Düşünün ki bir method'unuz (_fonksiyonunuz_) olsun, ve bu method’u dinamik olarak programlayabilseniz?

```ruby
def multiplier(with)
  return Proc.new {|number| number * with }
end
```

Çarpma yaptıran dinamik bir fonksiyon. Sayıyı kaç ile çarpacaksak `with` e o sayıyı geçiyoruz.

```ruby
multiply_with_5 = multiplier(5)
```

Elimizde geçtiğimiz sayıyı **5** ile çarpacak, fonksiyondan türemiş bir fonksiyon oluştu. Eğer `multiply_with_5` acaba neymiş dersek?

```ruby
puts multiply_with_5
# #<Proc:0x007fcc9c94a938@/Users/vigo/Desktop/ruby101_book_tests.rb:2>

puts multiply_with_5.class
# Proc
```

Gördüğünüz gibi elimizde bir adet `Proc` objesi var!. Haydi kullanalım!

```ruby
puts multiply_with_5.call(5)  # 25
puts multiply_with_5.call(10) # 50
```

Bu kadar kasmadan, basit bir method ile yapsaydık:

```ruby
def multiplier(number, with)
  return number * with
end

puts multiplier 5, 5 # 25
```

şeklinde olurdu. Benzer bir örnek daha yapalım:

```ruby
multiplier = Proc.new { |*number|
  number.collect { |n| n ** 2 }
}

multiplier.call(1)       # => [1]
multiplier.call(2,4,6)   # => [4, 16, 36]
multiplier[2,4,6].class  # => Array
```

Az ileride `Array` konusunda göreceğimiz `collect` method'unu kullandık. Bu method ile Array'in her elemanını okuyor ve karesini `n ** 2` alıyoruz. `*` işareti yine Array'de göreceğimiz **splat** özelliği. Yani geçilen parametre grubunu **Array** olarak değerlendir diyoruz.

## Lambda

Python'la uğraşan okurlarımız **Lambda**'ya aşina olabilirler. **Proc** ile ilk göze çarpan fark, argüman olarak geçilen parametrelerin sayısı önemlidir Lambda'da. Aynı **Proc** gibi çalışır.

```ruby
custom_print = lambda { |txt| puts txt }
custom_print.call("Hello") # Hello
```

Eğer **2** parametre geçseydik:

```ruby
custom_print = lambda { |txt| puts txt }
custom_print.call("Hello", "World")

# ArgumentError: wrong number of arguments (2 for 1)
```

Ya da;

```ruby
l = lambda { "Merhaba" }
puts l.call # Merhaba
```

Başka bir kullanım;

```ruby
l = lambda do |user_name|
  if user_name == "vigo"
    "Selam vigo! nasılsın?"
  else
    "Selam sana #{user_name}"
  end
end

puts l.call("vigo")   # Selam vigo! nasılsın?
puts l.call("uğur")   # Selam sana uğur
```

### Proc ve Lambda Farkı

```ruby
def arguments(input)
  one, two = 1, 2
  input.call(one, two)
end

puts arguments(Proc.new{ |a, b, c| puts "#{a} ve #{b} ve #{c.class}" })

# 1 ve 2 ve NilClass

puts arguments(lambda{ |a, b, c| puts "#{a} ve #{b} ve #{c.class}" })

# ArgumentError: wrong number of arguments (2 for 3)
```

Aynı kodu kullandık, **Lambda** yeterli parametre almadığı için hata verdi.

### Proc'u Block'a çevirmek

Elimizdeki **Array** elemanlarının her birini bir fonksiyona tabii tutsak? Dizinin her elemanını ekrana yazdıran bir şey?

```ruby
items = ["Bu bir", "bu iki", "bu üç"]
print_each_element = lambda { |item| puts item }
items.each(&print_each_element)
```

Bu örnekte `items.each(&print_each_element)` satırı Proc'u Block'a çevirdiğimiz yer. `&` işin sırrı. `each`'den gelen eleman, sanki method çağırır gibi `print_each_element` e pas ediliyor, karşılayan da `{` `}` içinde tanımlı kod bloğunu çalıştırıyor.

Keza aynı işi;

```ruby
items = ["Bu bir", "bu iki", "bu üç"]
def print_each_element(item)
  puts item
end
items.each(&method(:print_each_element))
```

şeklinde de yapabilirdik!


## Conditional Statements (Koşullar)

Verilen ifadenin doğru ya da yanlış olduğunun testi yapılır, akış **true** ya da **false** durumuna göre seyreder.

### `if` durumu

`if a == b then` dediğimizde, **Eğer "a, b'ye eşittir" önermesi doğruysa** demiş oluruz.

`if a != b then` dediğimizde, **Eğer "a, b'ye eşit değildir" önermesi doğruysa** demiş oluruz.

Aynı mantıkta, `a > b` ("a, b'den büyüktür"), `a < b` ("a, b'den küçüktür"), `a >= b` ("a, b'en büyük ya da eşittir"), `a <= b` ("a, b'den küçük ya da eşitse") şeklinde önermeler de kurulabilir.

### Negatiflik Operatörü

`!` işareti önermenin sol tarafında kullanılırsa, negatiflik ya da olumsuzluk kontrolü olduğu anlamındadır.

`if !a == b then puts "a, b'ye eşit değil" end` ya da `if !a > b then puts "a, b'den büyük değil" end` gibi kullanılır. İçerideki önermenin tersi doğruysa demektir. 

`if !a == b then` örneğini daha iyi inceleyecek olursak. Örnekte önerme "a, b'ye eşittir" önermesinin olumsuzu, yani "a, b'ye eşit değildir"dir. Yani bu `if` durumu şuna karşılık gelmektedir:  **"a, b'ye eşit değildir" doğru ise**

### Çoklu Kontrol

Eğer önermeler arasında `and` (_ve_), `or` (_veya_) ya da bunların kısa yollarını (_and_ için `&&`, _or_ için `||`) kullanırsak birden fazla şeyi kontrol etmiş oluruz.

```ruby
a = 5
b = 10
if a == 5 && b == 10
  puts "İşleme devam edebiliriz!"
end
```

`if a == 5 && b == 10` yerine `if a == 5 and b == 10` de yazabilirdik.

***

Ruby'nin konuşma diline yakın olması sebebiyle, çok daha anlaşılır kontrol satırları yazmak mümkün. Örneğin, **Eğer, a, 5'e eşitse Merhaba Yaz** demek için ilk akla gelen yöntem:

```ruby
if a == 5
  puts "Merhaba"
end
```

ama bunu çok daha basit hale getirebiliriz:

`puts "Merhaba" if a == 5` Tek satırda, `if`i koşul sonucunda olacak şeyin sağına yazmak yeterlidir.

### `elsif`

Programlama mantığında pek de sevmediğim (_daha düzgün yöntemleri var_) ama bazen mecbur kaldığımız bir durumdur.

```ruby
if a == b
  puts "a, b'ye eşit"
elsif a < b
  puts "a, b'den küçük"
else
  puts "a, b'ten büyük"
end
```

İlk olarak `a == b` kontrolü yapılır, eğer sonuç `false` ise, `a < b` kontrol edilir, o da `false` ise en sondaki `else` devreye giriyor ve çıktı olarak "a, b'ten büyük" yazdırılıyor.

### `unless`

Bu, aslında `if`in tersi gibi. Daha doğrusu `if not` anlamında. **Eğer a, b'ye eşit değilse** demek için;

```ruby
unless a == b
  puts "Eşit değil"
end
```

Aynı mantıkta `puts "Eşit değil" unless a == b` şeklinde de yazabiliriz. Semantik olarak olumsuzluk kontrolü yaparken `unless` kullanılması önerilir. Kodu okuma ve anlama açısından kolay olması için.

### `while`, `break`, `until` Döngüleri

Tanımlanan önerme `true` olduğu sürece **loop** yani döngü çalıştırma kontrolüdür.

```ruby
i = 0
while i < 5 do
  puts "i = #{i}"
  i += 1
end
```

Eğer `i += 1` yani `i`yi bir arttır, demezsek sonsuz döngüye gireriz. Eğer belli bir anda döngüyü kırmak istersek,

```ruby
i = 0
while i < 5 do
  puts "i = #{i}"
  break if i == 3
  i += 1
end
```

`i` **3** olduğunda loop devre dışı kalır...

Aynı `unless` mantığında, `until` kullanılır loop'larda.

```ruby
i = 0
until i == 10 do
  puts "i = #{i}"
  i += 1
end
```

Yani `i` **10**'a eşit olmadığı sürece bu loop çalışır.

### `case`, `when` Yapısı

`elsif` yerine kullanılması muhtemel, daha anlaşılır kontrol mekanizmasıdır. Hemen örneğe bakalım:

```ruby
computer = "c64"
year = case computer
  when "c64" then "1982"
  when "c16" then "1984"
  when "amiga" then "1985"
  else
    "Tarih bilinmiyor"
end

puts "#{computer} çıkış yılı #{year}"

# c64 çıkış yılı 1982
```

Yukarıdaki kodu bir ton `if`, `elsif` ile yapmak yerine, `when` ve `then` ile daha anlaşılır hale getirdiğimizi düşünüyorum.

`when` kullanırken **range** (_aralık_) belirmesi de yapma şansı var.

```ruby
student_grade = 8
case student_grade
when 0
  puts "Çok kötü"
when 1..4
  puts "Başarısız"
when 5..7
  puts "İyi"
when 8..9
  puts "Çok İyi"
when 10
  puts "Süper"
end
```

Eğer not **1** ile **4** aralığındaysa (ve dahil ise) ya da **5** ile **7** aralığındaysa gibi bir kontrol ekledik.

### `for` Döngüsü

**1**'den **10**'a kadar (1 ve 10 dahil) bir döngü yapalım:

```ruby
for i in 1..10
  puts "i = #{i}"
end

# i = 1
# i = 2
# i = 3
# i = 4
# i = 5
# i = 6
# i = 7
# i = 8
# i = 9
# i = 10
```

Aynı işi çok daha kolay yapmanın yolunu **5.Bölüm**'de **Iteration** kısmında göreceğiz!

### Ternary Operatörü

Kısaltılmış `if` yapısıdır. Hemen hemen pek çok dilde kullanılan, **Eğer şu doğru ise bu değilse bu** ifadesi için kullanılır.

```ruby
amount = 2
pluralize = amount == 1 ? "apple" : "apples"
puts "#{amount} #{pluralize}."
```

Bu örnekte **Ternary** olarak `amount == 1 ? "apple" : "apples"` kullanılmış, eğer `amount` **1** ise "apple" dönecek, değil ise "apples" dönecek. Yani `pluralize` değişkenine kontrolden dönen atanıyor.

### BEGIN ve END

Ruby'de ilginç bir özellik de, kodun çalışmasından önceye ve sonraya bir ek takabiliyoruz. 

Aşağıdaki örnekte BEGIN block'undaki kodlar program başladığında, END block'undaki kodlar program bitmeden hemen önce çalışacaktır. 

```ruby
BEGIN { puts "Kodun başlama saati #{Time.now.to_s}" }
END { puts "Kodun bitme saati #{Time.now.to_s}" }

def say_hello(username)
  "Merhaba #{username}"
end

puts say_hello "Uğur"
sleep 5  # zaman farkı için 5 saniye bekle

# Kodun başlama saati 2014-08-04 09:30:24 +0300
# Merhaba Uğur
# Kodun bitme saati 2014-08-04 09:30:29 +0300
```