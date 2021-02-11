# Bölüm 4: Veri Tipleri

## Object (Nesne)

Ruby, **Object Oriented Programming**'in dibidir :) Her şey nesnelerden oluşur. Nasıl mı? hemen basit bir değişken oluşturup içine bir tekst yazalım.

    mesaj = "Merhaba"

`mesaj` değişkeninin türü ne?

    mesaj.class # => String

Bu değişken **String** nesnesinden türemiş. Peki, **String** nereden geliyor?

    mesaj.class.superclass # => Object

Dikkat ettiyseniz burada `superclass` kullandık. Yani bu hiyerarşideki bir üst sınıfı arıyoruz. Karşımıza ne çıktı? **Object**. Peki acaba **Object** nereden türemiş?

    mesaj.class.superclass.superclass # => BasicObject

Hmmm.. Peki **BasicObject** nereden geliyor?

    mesaj.class.superclass.superclass.superclass # => nil

İşte şu anda dibi bulduk :) Demek ki hiyerarşi;

    BasicObject > Object > String

şeklinde bir hiyerarşi söz konusu.

Peki, sayılarda durum ne?

```ruby
numara = 1
numara.class # => Fixnum
numara.class.superclass # => Integer
numara.class.superclass.superclass # => Numeric
numara.class.superclass.superclass.superclass # => Object
numara.class.superclass.superclass.superclass.superclass # => BasicObject
numara.class.superclass.superclass.superclass.superclass.superclass # => nil
```

Ufff... bir an için bitmeyecek sandım :) Çok basit bir sayı tanımlası yaptığımızda bile, arka plandaki işleyiş yukarıdaki gibi oluyor. Yani

    BasicObject > Object > Numeric > Integer > Fixnum

şeklinde yine ana nesne **BasicObject** olmak koşuluyla uzun bir hiyerarşi söz konusu.

Her şey **BasicObject** den türüyor, bu yüzden de aslında her şey bir **Class** dolayısıyla bu durum dile çok ciddi esneklik kazandırıyor.

### Nesne Metodları (Object Instance Methods)

Şimdi boş bir nesne oluşturalım. **Class** bölümünde daha detaylı göreceğimiz **instantiate** işlemiyle `new` methodunu kullanarak;

```ruby
o = Object.new # => #<Object:0x007fe552099a68>
o.__id__       # => 70311450299700
```

yaptığımızda, oluşan nesnenin hafızada **unique** (_yani bundan sadece bir tane_) bir **identifier**'ı (_kabaca buna kimlik diyelim_) yani **ID**'si olduğunu görürüz. `__id__` yerine `object_id` yani `o.object_id` şeklinde de kullanabiliriz.

Eğer `hash` method’unu çağırırsak, Ruby bize ilgili objenin `Fixnum` türünde sayısal değerini üretir ve verir.

```ruby
o = Object.new # => #<Object:0x007f8c3b0a3420>
o.__id__       # => 70120131336720
o.object_id    # => 70120131336720
o.hash         # => -229260864779029724
```

Neticede **String** de bir nesne ve;

```ruby
t = String.new("Hello")  # => "Hello"
t.__id__                 # => 70170408456140
t.methods                # => [:<=>, :==, :===, :eql?, :hash, :casecmp, :+, :*, :%, :[], :[]=, :insert, :length, :size, :bytesize, :empty?, :=~, :match, :succ, :succ!, :next, :next!, :upto, :index, :rindex, :replace, :clear, :chr, :getbyte, :setbyte, :byteslice, :scrub, :scrub!, :freeze, :to_i, :to_f, :to_s, :to_str, :inspect, :dump, :upcase, :downcase, :capitalize, :swapcase, :upcase!, :downcase!, :capitalize!, :swapcase!, :hex, :oct, :split, :lines, :bytes, :chars, :codepoints, :reverse, :reverse!, :concat, :<<, :prepend, :crypt, :intern, :to_sym, :ord, :include?, :start_with?, :end_with?, :scan, :ljust, :rjust, :center, :sub, :gsub, :chop, :chomp, :strip, :lstrip, :rstrip, :sub!, :gsub!, :chop!, :chomp!, :strip!, :lstrip!, :rstrip!, :tr, :tr_s, :delete, :squeeze, :count, :tr!, :tr_s!, :delete!, :squeeze!, :each_line, :each_byte, :each_char, :each_codepoint, :sum, :slice, :slice!, :partition, :rpartition, :encoding, :force_encoding, :b, :valid_encoding?, :ascii_only?, :unpack, :encode, :encode!, :to_r, :to_c, :>, :>=, :<, :<=, :between?, :nil?, :!~, :class, :singleton_class, :clone, :dup, :taint, :tainted?, :untaint, :untrust, :untrusted?, :trust, :frozen?, :methods, :singleton_methods, :protected_methods, :private_methods, :public_methods, :instance_variables, :instance_variable_get, :instance_variable_set, :instance_variable_defined?, :remove_instance_variable, :instance_of?, :kind_of?, :is_a?, :tap, :send, :public_send, :respond_to?, :extend, :display, :method, :public_method, :singleton_method, :define_singleton_method, :object_id, :to_enum, :enum_for, :equal?, :!, :!=, :instance_eval, :instance_exec, :__send__, :__id__]

t.method(:upcase).call   # => "HELLO"
```

`t.methods` ise **String**'den türeyen `t` ye ait tüm method’ları listeledik. Sonuç **Array** (_Dizi_) olarak geldi ve bu dizinin tüm elemanları `:` işaretiyle başlıyor. Çünkü bu elemanlar birer **Symbol**.

`t.method(:upcase).call` da ise, `t`'nin `:upcase` method’unu `call` ile çağırdır. Aslında yaptığımız iş: `"hello".upcase` ile birebir aynı.

Acaba bu nesne ne?

`t.is_a?(String) # => true` `is_a?` method’u ile nesnenin türünü kontrol edebiliriz.

Diğer dillerin pek çoğunda (_özellikle Python_) bir işi yapmanın bir ya da en fazla iki yolu varken, **Ruby** bu konuda çok rahattır. Bir işi yapmanın her zaman birden fazla yolu olur ve bunların neredeyse hepsi doğrudur. (_Kullanıldığı yere ve amaca bağlı olarak_)

`is_a?` yerine `kind_of?` da kullanabiliriz!

Bir nesneye ait hangi method'ların olduğunu;

```ruby
o = Object.new
o.methods # => [:nil?, :===, :=~, :!~, :eql?, :hash, :<=>, :class, :singleton_class, :clone, :dup, :taint, :tainted?, :untaint, :untrust, :untrusted?, :trust, :freeze, :frozen?, :to_s, :inspect, :methods, :singleton_methods, :protected_methods, :private_methods, :public_methods, :instance_variables, :instance_variable_get, :instance_variable_set, :instance_variable_defined?, :remove_instance_variable, :instance_of?, :kind_of?, :is_a?, :tap, :send, :public_send, :respond_to?, :extend, :display, :method, :public_method, :singleton_method, :define_singleton_method, :object_id, :to_enum, :enum_for, :==, :equal?, :!, :!=, :instance_eval, :instance_exec, :__send__, :__id__]

o.public_methods # => [:nil?, :===, :=~, :!~, :eql?, :hash, :<=>, :class, :singleton_class, :clone, :dup, :taint, :tainted?, :untaint, :untrust, :untrusted?, :trust, :freeze, :frozen?, :to_s, :inspect, :methods, :singleton_methods, :protected_methods, :private_methods, :public_methods, :instance_variables, :instance_variable_get, :instance_variable_set, :instance_variable_defined?, :remove_instance_variable, :instance_of?, :kind_of?, :is_a?, :tap, :send, :public_send, :respond_to?, :extend, :display, :method, :public_method, :singleton_method, :define_singleton_method, :object_id, :to_enum, :enum_for, :==, :equal?, :!, :!=, :instance_eval, :instance_exec, :__send__, :__id__]

o.private_methods # => [:initialize_copy, :initialize_dup, :initialize_clone, :sprintf, :format, :Integer, :Float, :String, :Array, :Hash, :warn, :raise, :fail, :global_variables, :__method__, :__callee__, :__dir__, :eval, :local_variables, :iterator?, :block_given?, :catch, :throw, :loop, :respond_to_missing?, :trace_var, :untrace_var, :at_exit, :syscall, :open, :printf, :print, :putc, :puts, :gets, :readline, :select, :readlines, :`, :p, :test, :srand, :rand, :trap, :exec, :fork, :exit!, :system, :spawn, :sleep, :exit, :abort, :load, :require, :require_relative, :autoload, :autoload?, :proc, :lambda, :binding, :caller, :caller_locations, :Rational, :Complex, :set_trace_func, :gem, :gem_original_require, :initialize, :singleton_method_added, :singleton_method_removed, :singleton_method_undefined, :method_missing]

o.protected_methods # => []

o.public_methods(false) # => []
```

`public_methods` default olarak `public_methods(all=true)` şeklinde çalışır. Eğer parametre olarak `false` geçersek ve bu bizim oluşturduğumuz bir nesne ise, sadece ilgili nesnenin **public_method**'ları geri döner.

Başta belirttiğimiz gibi, basit bir nesne bile **Object**'den türediği için ve default olarak bu türeme esnasında tüm özellikler diğerine geçtiği için, sadece sizin method'larınızı görüntülemek açısından `false` olayı çok işe yarar.

### Method Missing

Bence Ruby'nin en süper özelliklerinden biridir. Olmayan bir method'u çağırdığınız zaman tetiklenen method `method_missing` method'udur. Ruby on Rails framework'ü neredeyse bu mekanizma üzerine kurulmuştur. 3 parametre alır; çağırılan method, eğer parametre geçilmişse parametreler, eğer block geçilmişse block.

```ruby
class User
  def method_missing(method_name, *args, &block)
    if method_name == :show_user_info
      "This user has no information"
    else
      "You've called #{method_name}, You've passed: #{args}"
    end
  end
end

u = User.new
u.show_user_info # => "This user has no information"
u.show_user_age  # => "You've called show_user_age, You've passed: []"
```

`User` adında bir Class'ımız var. İçinde hiçbir method tanımlı değil. `u.show_user_info` satırında, olmayan bir method'u çağırıyoruz. Tanımladığımız `method_missing` method'u ile olmayan method çağırılmasını yakalıyoruz. Eğer `show_user_info` diye bir method çağrılırsa yakalıyoruz, bunun dışında bir şey olursa da method adını ve geçilen parametreleri gösteriyoruz.

Bu sayede `NoMethodError` hatası almadan işimize devam edebiliyoruz.

Anlamak açısından, Roman rakamları için bir sınıf yaptığınızı düşünün. Sadece örnek olması için gösteriyorum, C,X ve M için;

```ruby
class Roman
  def roman_to_str(str)
    case str
    when "x", "X"
      10
    when "c", "C"
      100
    when "m", "M"
      1000
    end
  end
  def method_missing(method)
    roman_to_str method.id2name
  end
end

r = Roman.new
r.x # => 10
r.X # => 10
r.C # => 100
r.M # => 1000
```

Bunu geliştirip "MMCX" ya da "III" gibi gerçek dönüştürme işini yapabilirsiniz.

**respond_to_missing?**

Yukarıdaki örnekte, olmayan method'ları ürettik. Peki, acaba bu olmayan method'ları nasıl çağırabilir ya da kontrol edebiliriz? Normalde, bir Class'ın hangi method'u olduğunu `respond_to?` ile öğreniyorduk. Örneğe uygulayalım;

`r.C` derken aslında `:C` method'unu çağırıyoruz. Peki böyle bir method var mı?

```ruby
r.method(:C)      # => `method': undefined method `C' for class `Roman' (NameError)
```

Nasıl yani? peki kontrol edelim?

```ruby
r.respond_to?(:C) # => false
```

Çünkü biz `:C` yi dinamik olarak ürettik ama öylece ortada bıraktık. Yapmamız gereken `respond_to_missing?` ile gereken cevabı vermekti:

```ruby
class Roman
  def roman_to_str(str)
    case str
    when "x", "X"
      10
    when "c", "C"
      100
    when "m", "M"
      1000
    end
  end
  def method_missing(method)
    roman_to_str method.id2name
  end
  def respond_to_missing?(method_name, include_private = false)
    [:x, :X, :c, :C, :m, :M].include?(method_name) || super
  end
end

r.method(:C)      # => #<Method: Roman#C>
r.respond_to?(:C) # => true
r.respond_to?(:Q) # => false # olmayan method
```

## Number (Sayılar)

Sayılar, temel öğe olmayıp, direk nesneden türemişlerdir. Türedikleri nesne de Ruby'nin sayılar için olan **base class**'ıdır.

Örneğin **3** sayısına bakalım:

```ruby
3.class # => Fixnum
3.class.superclass # => Integer
3.class.superclass.superclass # => Numeric
3.class.superclass.superclass.superclass # => Object
```

`Numeric` sınıfıdır asıl olan. İlk türediği sınıfı `Fixnum`dır. Örnekte gördüğünüz gibi;

`Fixnum > Integer > Numeric` şeklinde bir hiyeraşi söz konusudur.

```ruby
2014.class                  # => Fixnum
2_014.class                 # => Fixnum
201.4.class                 # => Float
1.2e3.class                 # => Float
7e4.class                   # => Float
7E-4.class                  # => Float
0664.class                  # => Fixnum
0xfff.class                 # => Fixnum
0b1111.class                # => Fixnum
45678327041234321312.class  # => Bignum
```

Ruby'de sayı işlerinde `_` hiçbir etki yapmaz. Bir şekilde okumayı kolaylaştırmak için kullanılır. Örnekteki **2014** ile **2_014** aynı şeydir. Büyük sayıları yazarken; `1_345_201` gibi bir ifade `1345201`'dir aslında.

Ondalık sayılarda `.` kullanılır. **Octal** yani 8'lik sayı sistemi için direk `0` ile `0664` gibi kullanılır. 16'lık yani **Hexadecimal** sayı sistemi için, css dünyasından tanıdığınız **beyaz** rengini ifade etmek için `$fff` yerine `0xfff` şeklinde bir kullanım mevcut. 2'lik yani **Binary** sayı sistemi için `0b` ile başlamak yeterlidir. **Scientific Notation** ifadeleri için de `1.2e3`ya da `7e4`gibi kullanımlar mümkündür.

### Number Method'ları

**5** sayısı `Fixnum` sınıfındandır ve neticede üst sınıfları;

    Numeric -> Integer -> Fixnum

şeklinde olduğu için (_en üsttte Numeric_) ilgili üst sınıfların method'ları da `Fixnum` tarafından kullanılabilir durumdadır.

Her zamanki gibi, **acaba bu sınıfa ait methodlar neymiş?** dediğimiz an bir ton method gelir karşımıza;

    5.methods # => [:to_s, :inspect, :-@, :+, :-, :*, :/, :div, :%, :modulo, :divmod, :fdiv, :**, :abs, :magnitude, :==, :===, :<=>, :>, :>=, :<, :<=, :~, :&, :|, :^, :[], :<<, :>>, :to_f, :size, :bit_length, :zero?, :odd?, :even?, :succ, :integer?, :upto, :downto, :times, :next, :pred, :chr, :ord, :to_i, :to_int, :floor, :ceil, :truncate, :round, :gcd, :lcm, :gcdlcm, :numerator, :denominator, :to_r, :rationalize, :singleton_method_added, :coerce, :i, :+@, :eql?, :remainder, :real?, :nonzero?, :step, :quo, :to_c, :real, :imaginary, :imag, :abs2, :arg, :angle, :phase, :rectangular, :rect, :polar, :conjugate, :conj, :between?, :nil?, :=~, :!~, :hash, :class, :singleton_class, :clone, :dup, :taint, :tainted?, :untaint, :untrust, :untrusted?, :trust, :freeze, :frozen?, :methods, :singleton_methods, :protected_methods, :private_methods, :public_methods, :instance_variables, :instance_variable_get, :instance_variable_set, :instance_variable_defined?, :remove_instance_variable, :instance_of?, :kind_of?, :is_a?, :tap, :send, :public_send, :respond_to?, :extend, :display, :method, :public_method, :singleton_method, :define_singleton_method, :object_id, :to_enum, :enum_for, :equal?, :!, :!=, :instance_eval, :instance_exec, :__send__, :__id__]

Bunların içinden en sık kullanılanlara ve kullanım şekillerine değineceğim.

```ruby
5.to_s # => "5"
# Sayısal değeri String'e çevirdik

-5.abs # => 5
# Mutlak değer

5.zero? # => false
# Sıfır mı?

5.even? # => false
# Çift sayı mı?

5.odd?  # => true
# Tek sayı mı?

5.next # => 6
# Sonraki sayı?

5.pred # => 4
# Önceki sayı?

3.14.floor # => 3
# Taban değeri

3.14.ceil # => 4
# Tavan değeri

1.49.round # => 1
1.51.round # => 2
# Yuvarlama

1.bit_length # => 1
15.bit_length # => 4
255.bit_length # => 8
# Bit cinsinden uzunluğu/boyu

1.size        # => 8
10.size       # => 8
10242048.size # => 8
1024204810242048102420481024.size # => 12
# Byte cinsinden kapladığı yer
```

`upto`, `downto` gibi iterasyonla ilgili olanları **Enumeration ve Iteration** bölümünde göreceğiz!

## String

Kabaca, insanların anlayacağı şekilde tekst / metin bilgisi içeren nesnelerdir. Örneğin;

```ruby
m = "Merhaba"
m.class # => String
```

şeklindedir. Tanımlama yaparken **tek** ya da **çift** tırnak kullanılabilir ama aralarında fark vardır. **Expression Substitution** ya da **String Interpolation** olarak geçen, **String** içinde değişken kullanımı esnasında **çift tırnak** kullanmanız gerekir.

```ruby
m = "Merhaba"
puts "#{m} Uğur" # Merhaba Uğur
puts '#{m} Uğur' # #{m} Uğur
```

Tek tırnak kullandığımız örnekte `#{m}` değişkeni işlenmeden olduğu gibi çıktı vermiştir. Aynı şekilde **escape codes** yani görünmeyen özel karakterleri de sadece çift tırnak içinde kullanmak mümkündür.

Çift tırnak içinde kullanılan `#{BURAYA RUBY KODU GELİR}` çok işe yarar. `{` ve `}` arasında kod çalıştırmamızı sağlar.

```ruby
puts "Saat: #{Time.now}" # Saat: 2014-08-12 10:37:22 +0300
```

dediğimizde; Ruby Kernel'dan `Time` nesnesinin `now` method'unu çalıştırmış oluruz.

```ruby
puts "Merhaba\nDünya"

# Merhaba
# Dünya
```

`\n` **New Line** ya da **Line Feed** ya da yeni satıra geçme karakteri çift tırnakta çalışır.

| Escape Kod | Anlamı |
| -- | -- |
| \n | Yeni satır (0x0a) |
| \s | Boşluk (0x20) |
| \r | Satır Başı (0x0d) |
| \t | Tab Karakteri (0x09) |
| \v | Dikey Tab (0x0b) |
| \f | Formfeed (0x0c) |
| \b | Backspace (Bir geri) (0x08) |
| \a | Bell/Alert (Uyarı) (0x07) |
| \e | Escape (0x1b) |
| \nnn | Octal, 8'lik değer |
| \xnn | Hexadecimal, 16'lık değer |
| \unnnn | Unicode: U+nnnn (Ruby 1.9+) |
| \cx | Control-x |
| \C-x | Control-x |
| \M-x | Meta-x |
| \M-\C-x | Meta-Control-x |
| \x | x'in kendisi (\" çift tırnak demektir.) |


String'ler **byte array**'lerden oluşur yani elemanları **byte** cinsinden dizidir aslında.

```ruby
m = ""
m << 65
puts m # A
```

`m` boş bir String, diziye eleman ekler gibi (`<<` _diziye eleman ekler, az sonra göreceğiz_) içine **65** ekledik. Bu `A` harfinin 10'luk sayı sistemindeki *ASCII* değeridir. Aslında `m = "A"` yaptık :)

Eğer **65** in karakter setindeki değeri neydi? dersek `put 65.chr` yaptığımızda bize `A` döner. **0** ile **255** arası değerlerdir bunlar.

Daha ilginç bir olay;

```ruby
puts "öz" "yıl" "maz" "el" # özyılmazel
```

yani `"tekst" "tekst" "tekst"` şekinde bir kullanım mevcuttur.

### String Literals (String Kalıpları)

Ruby'de, yine diğer dillerde olmayan ilginç bir özellik. `%` işareti ve sonrasında gelen bazı karakterler yardımıyla enteresan şeyler yapmak mümkün:

#### `%`

Süslü parantezler arasında kalan her şey **concat** (_yani toplanarak_) edilir ve **String** olarak çıktı verir ve tırnakları **escape** eder.

```ruby
%{Merhaba Dünya Ben vigo nasılsınız?} # => "Merhaba Dünya Ben vigo nasılsınız?"

%{Bu işlemlerin %80'i "uydurma"} # => "Bu işlemlerin %80'i \"uydurma\""
```

aynı işi; `%|Merhaba Dünya Ben vigo nasılsınız?|` süslü parantez yerine **pipe** `|` kullanarak da yapabilirsiniz!

#### `%w`

Hızlıca **Array** üretmeyi sağlar:

```ruby
%w{foo bar baz}       # => ["foo", "bar", "baz"]
%w{foo bar baz}.class # => Array
```

#### `%i`

İçinde **Symbol** olan **Array** üretir:

```ruby
%i{foo bar baz} # => [:foo, :bar, :baz]
```

#### `%q` ve `%Q`

`q` tek tırnak, `Q` çift tırnakla sarmalamış gibi yapar:

```ruby
person = "Uğur"
%q{Merhaba #{person}} # => "Merhaba \#{person}"
%Q{Merhaba #{person}} # => "Merhaba Uğur"
```

#### `%s`

**Symbol**'e çevirir:

```ruby
%s{my_variable} # => :my_variable
%s{email} # => :email
```

#### `%r`

**Regular Expression**'a çevirir:

```ruby
%r{(.*)hello}i # => /(.*)hello/i
```

#### `%x`

Ruby'de **back tick** kullanarak **shell** komutu çalıştırabilirsiniz. Yani Linux ve Mac kullanan okuyucular **Terminal** ile haşır-neşir olmuşlardır. Örneğin, bulunduğunuz dizindeki dosya listesini almak için `ls` komutunu kullanırsınız. Örneğin kendi **home** dizinimde `ls` yaptığımda (_OSX kullanıyorum_)

    ls -1 # alt alta listemelek için

    Applications
    Desktop
    Development
    Documents
    Dotfiles
    Downloads
    Library
    VirtualBox VMs
    Works
    bin

gibi bir liste alıyorum. Yapacağım bir Ruby uygulamasında;

```ruby
%x{ls -1 $HOME} # => "Applications\nDesktop\nDevelopment\nDocuments\nDotfiles\nDownloads\nLibrary\nVirtualBox VMs\nWorks\nbin\n"
```

Tüm liste tek bir String olarak geldi ve `\n` karakteri ile birleşti çünki sonuç alt alta satır satır geldi. Eğer;

```ruby
%x{ls -1 $HOME}.split("\n")
```

deseydim sonuç bana dizi olarak gelecekti!

    # ["Applications", "Desktop", "Development", "Documents", "Dotfiles", "Downloads", "Library", "VirtualBox VMs", "Works", "bin"]

    %x{ruby --copyright} # => "ruby - Copyright (C) 1993-2014 Yukihiro Matsumoto\n"

### Here Document Kullanımı

Uzun metin kullanımlarında çok işe yarar:

```ruby
mesaj = <<END
Merhaba nasılsınız?
Biz de çok iyiyiz
Görüşürüz!
END
puts mesaj

# Merhaba nasılsınız?
# Biz de çok iyiyiz
# Görüşürüz!
```

Bu örnekte `mesaj = <<END` ile `END` kelimesini görene kadar içinde ne varsa kullan diyoruz! Daha da çılgın bir kullanım şekli;

```ruby
mesaj = [<<BİR, <<İKİ, <<ÜÇ]
  Bu Bir
BİR
  Bu iki....
İKİ
  Bu da üüüüüüüüç
ÜÇ

puts mesaj

# - Bu Bir
# - Bu iki....
# - Bu da üüüüüüüüç
```

### String Method'ları

#### String % argüman -> yeni String

```ruby
"Merhaba %s" % "Uğur" # => "Merhaba Uğur"
"Sayı: %010d" % 2014  # => "Sayı: 0000002014"
"Kullanıcı Adı: %s, E-Posta: %s" % [ "vigo", "vigo@foo.com" ] # => "Kullanıcı Adı: vigo, E-Posta: vigo@foo.com"
"Merhaba %{username}!" % { :username => 'vigo' } # => "Merhaba vigo!"
```

Keza bu method, `printf`, `sprintf` gibi, **String Format** mantığında çalışır. `"Sayı: %010d" % 2014` örneğinde `%010d` aslında **10** basamaklı, **0** ile pad edilmiş şekilde göster anlamındadır. **2014** 4 basamaklıdır, solunda **6** tane sıfır gelmiştir.

#### String * sayı -> yeni String

String ile sayıyı çarpmak mümkün!

```ruby
"Merhaba!" * 3 # => "Merhaba!Merhaba!Merhaba!"
"Merhaba!" * 0 # => ""
```

#### String + String -> yeni String

```ruby
"Merhaba" + " " + "Dünya" # => "Merhaba Dünya"
```

#### String << sayı -> String
#### String << nesne -> String

```ruby
a = "Merhaba"
a << " dünya" # => "Merhaba dünya"
a # => "Merhaba dünya"
a.concat(33) # => "Merhaba dünya!"
a << 33 # => "Merhaba dünya!!"
```

#### String <=> başka string → -1, 0, +1 ya da nil

`<=>` Ruby dünyasında **Spaceship** operatörü olarak geçer. Aynı cins nesneleri karşılaştırmak için kullanılır.

```ruby
"vigo" <=> "vigo"    # => 0   # eşit
"vigo" <=> "vig"     # => 1   # vigo büyük
"vigo" <=> "vigoo"   # => -1  # vigo küçük
"vigo" <=> 3         # => nil # alakasız iki şey
```

`casecmp` ile aynı işi yapar

#### String =~ Nesne -> Fixnum ya da nil

```ruby
"Saat tam 4'de buluşalım" =~ /\d/ # => 9

# \d sayı yakaladı ve indeksi döndü

"Saat tam 4'de buluşalım"[9] # => "4"
```

#### String içinde hareket

String aslında karakterlerden oluşan bir dizi olduğu için aşağıdaki gibi atraksiyonlar yapmak mümkün.

    String[indeks] -> yeni string ya da nil
    String[başlangıç, uzunluk] -> yeni string ya da nil
    String[range] -> yeni string ya da nil
    String[regexp] -> yeni string ya da nil
    String[regexp, yakalan] -> yeni string ya da nil
    String[metinni_bul] -> yeni string ya da nil

örnek olarak;

```ruby
m = "Merhaba Dünya"
m[0]         # => "M"  # 0.karakter
m[0, 2]      # => "Me" # 0'dan itibaren 2 karakter
m[0..4]      # => "Merha" # range, 0'dan 4 dahil
m[-1,]       # => "a" # son karakter
m[-13..-1]   # => "Merhaba Dünya" # sondan başa
m[15, 1]     # => nil # olmayan indeks
m[/(?<sesli>[aeiou])/, "sesli"] # => "e" # regexp
m["Merhaba"] # => "Merhaba" # metni bul
m["vigo"] # => nil # olmayan metin
```

aynı işi `slice` ile de yapabilirsiniz.

```ruby
m = "merhaba"
m.slice(2,5) # => "rhaba"
```

gibi...

#### Yardımcı Methodlar

Sıkça kullanılanlar arasında; `capitalize`, `center`, `chars`, `chomp`, `chop`, `clear`, `count`, `size`, `length`, `delete`, `ljust`, `rjust`, `reverse`, `upcase`, `downcase`, `swapcase`, `reverse`, `index`, `hex`, `rindex`, `insert` gibi methodlar'dan örnekler ekledim. Her zaman olduğu gibi, hangi method'ların olduğunu görmek için;

```ruby
String.new.methods # => [:<=>, :==, :===, :eql?, :hash, :casecmp, :+, :*, :%, :[], :[]=, :insert, :length, :size, :bytesize, :empty?, :=~, :match, :succ, :succ!, :next, :next!, :upto, :index, :rindex, :replace, :clear, :chr, :getbyte, :setbyte, :byteslice, :scrub, :scrub!, :freeze, :to_i, :to_f, :to_s, :to_str, :inspect, :dump, :upcase, :downcase, :capitalize, :swapcase, :upcase!, :downcase!, :capitalize!, :swapcase!, :hex, :oct, :split, :lines, :bytes, :chars, :codepoints, :reverse, :reverse!, :concat, :<<, :prepend, :crypt, :intern, :to_sym, :ord, :include?, :start_with?, :end_with?, :scan, :ljust, :rjust, :center, :sub, :gsub, :chop, :chomp, :strip, :lstrip, :rstrip, :sub!, :gsub!, :chop!, :chomp!, :strip!, :lstrip!, :rstrip!, :tr, :tr_s, :delete, :squeeze, :count, :tr!, :tr_s!, :delete!, :squeeze!, :each_line, :each_byte, :each_char, :each_codepoint, :sum, :slice, :slice!, :partition, :rpartition, :encoding, :force_encoding, :b, :valid_encoding?, :ascii_only?, :unpack, :encode, :encode!, :to_r, :to_c, :>, :>=, :<, :<=, :between?, :nil?, :!~, :class, :singleton_class, :clone, :dup, :taint, :tainted?, :untaint, :untrust, :untrusted?, :trust, :frozen?, :methods, :singleton_methods, :protected_methods, :private_methods, :public_methods, :instance_variables, :instance_variable_get, :instance_variable_set, :instance_variable_defined?, :remove_instance_variable, :instance_of?, :kind_of?, :is_a?, :tap, :send, :public_send, :respond_to?, :extend, :display, :method, :public_method, :singleton_method, :define_singleton_method, :object_id, :to_enum, :enum_for, :equal?, :!, :!=, :instance_eval, :instance_exec, :__send__, :__id__]
```

kullanabiliriz!

```ruby
m = "merhaba"

m.capitalize # => "Merhaba"
m # => "merhaba"

m.capitalize! # => "Merhaba" # m'in değeri artık değişti!
m # => "Merhaba"

"vigo".center(12) # => "    vigo    "
"vigo".center(12, "*") # => "****vigo****"

"merhaba".chars # => ["m", "e", "r", "h", "a", "b", "a"]

"merhaba\n".chomp # => "merhaba"
"merhaba dünya".chomp(" dünya") # => "merhaba"

"merhaba vigo".chop # => "merhaba vig"
"merhaba vigo\n".chomp # => "merhaba vigo"

"a".chr # => "a"

x = "Merhaba"
x.clear # => ""

"Merhaba Dünya".count("a") # => 3  # 3 adet a
"Merhaba Dünya".count("ab") # => 4 # a ve b toplam 4 tane
"Merhaba Dünya".count("e") # => 1  # e'den 1 tane

"Merhaba Dünya".delete("e") # => "Mrhaba Dünya"
"Merhaba Dünya".delete("a", "ba") # => "Merhb Düny"

"MERHABA".downcase # => "merhaba"
"merhaba".upcase # => "MERHABA"
"Merhaba".swapcase # => "mERHABA"

"merhaba".size # => 7
"merhaba".length # => 7

"merhaba".ljust(20)      # => "merhaba             "
"merhaba".ljust(20, "*") # => "merhaba*************"
"merhaba".rjust(20)      # => "             merhaba"
"merhaba".rjust(20, "*") # => "*************merhaba"

"   merhaba  ".strip  # => "merhaba"
"  merhaba".lstrip    # => "merhaba"
"merhaba  ".rstrip    # => "merhaba"

"merhaba".index("m") # => 0
"merhaba".index("ba") # => 5

"merhaba".rindex("m") # => 0
"merhaba".rindex("h") # => 3

"a".next     # => "b"
"abcd".next  # => "abce" # d'den sonra e geldi...
"b".succ     # => "c"

# baş, ayraç, son
"merhaba".partition("r") # => ["me", "r", "haba"]
"merhaba".partition("a") # => ["merh", "a", "ba"]
"merhaba".partition("x") # => ["", "", "merhaba"]

"merhaba dünya".reverse # => "aynüd abahrem"

"hey seeeeeeeeeeeeeeen! aloooooooo".squeeze # => "hey sen! alo"

"Merhaba".dump # => "\"Merhaba\""
"merhaba".getbyte(0) # => 109 # m'in ascii değeri

"0x0f".hex # => 15
"0x0fff".hex # => 4095

"merhaba".insert(0, "X") # => "Xmerhaba"
"merhaba".insert(3, "A") # => "merAhaba"
"merhaba".insert(-1, ".") # => "merhaba."

"123".oct # => 83 # Octal'e çevirdi (8'lik)

"A".ord # => 65  # Ascii değeri
"a".ord # => 97 # Ascii değeri

"dünya".prepend("Merhaba ") # => "Merhaba dünya" # öne ekledi

# transform
"merhaba hello".tr("el", "ip") # => "mirhaba hippo" # e=>i, l => p oldu
"ArkAdAşlar nasılsınız?".tr("A", "a") # => "arkadaşlar nasılsınız?"

# a'dan e'y kadar X ile transform yap
"merhaba dünya".tr("a-e", "X") # => "mXrhXXX XünyX"
```

#### Convert Method'ları

Tip değiştirmek için kullanılır. `to_i`, `to_f`, `to_s`, `to_str`, `to_sym`, `to_r`, `to_c`, `to_enum` method'larına bakalım:

```ruby
"merhaba".to_i   # => 0 # integer'a çevirdi
"merhaba".to_f   # => 0.0 # float'a çevirdi
"5".to_i          # => 5
"1.5".to_f        # => 1.5
"merhaba".to_s    # => "merhaba" # string
"merhaba".to_str  # => "merhaba" # string
"merhaba".to_sym  # => :merhaba # symbol
"merhaba".to_r    # => (0/1) # Rasyonel sayı
"0.2".to_r        # => (1/5) # Rasyonel sayı
"merhaba".to_c    # => (0+0i) # Kompleks sayı
"1234".to_c       # => (1234+0i)
"merhaba".to_enum # => #<Enumerator: "merhaba":each> # Enumeratör'e çevirdi
```

#### Kontrol Method'ları

Method adı `?` ilen bitiyor demek, bir kontrol olduğu ve sonucun **Boolean** yanı `true` ya da `false` döndüğü anlamında olduğunu söylemiştik.

```ruby
"merhaba".start_with?("m") # => true
"merhaba".start_with?("mer") # => true
"merhaba".start_with?("f") # => false

"merhaba".end_with?("a") # => true
"merhaba".end_with?("haba") # => true
"merhaba".end_with?("zoo") # => false

"merhaba".eql?("Merhaba") # => false
"merhaba".eql?("merhaba") # => true

"merhaba dünya".include?("dünya") # => true

"merhaba".empty? # => false
"".empty? # => true

"kedi".between?("at", "balık") # => false # başlangıç harfi a ve b arasındamı? gibi düşünün
"kedi".between?("fare", "sinek") # => true
```

#### Array ve Block ile İlişkili Methodlar

**split**
Metni parçalara böler, varsayılan **delimiter** (_ayırıcı_) boşuk karakteridir.

```ruby
"Selam millet nasıl sınız?".split # => ["Selam", "millet", "nasıl", "sınız?"]
"Selam millet-nasıl sınız?".split("-") # => ["Selam millet", "nasıl sınız?"]
"A takımı: 65 B takımı: 120".split(/ +\d+ ?/) # => ["A takımı:", "B takımı:"]
"1,2,3,4,5".split(",") # => ["1", "2", "3", "4", "5"]
```

**each_byte**

```ruby
"merhaba".each_byte {|c| puts c }

# 109 (m)
# 101 (e)
# 114 (r)
# 104 (h)
# 97  (a)
# 98  (b)
# 97  (a)
```

**each_char**

```ruby
"merhaba".each_char {|c| puts c }

# m
# e
# r
# h
# a
# b
# a
```

**each_line**

```ruby
"Merhaba\nDünya\nNasıl sın?".each_line {|l| puts l }

# Merhaba
# Dünya
# Nasıl sın?

"Merhaba@@Dünya@@Nasıl sın?".each_line("@@") {|l| puts l }
# Merhaba@@
# Dünya@@
# Nasıl sın?
```

**upto**

```ruby
"a1".upto("b1"){ |t| puts t }

# a1
# a2
# a3
# a4
# a5
# a6
# a7
# a8
# a9
# b0
# b1
```


#### Pattern Yakalama (Regexp)

Daha kapsamlı olarak **6.Bölüm**'de de değineceğimiz **Regular Expression** konusu, String'lerle çok ilişkili. Hemen ilgili method'lara bakalım

**gsub** ve **sub**

`sub` ile `gsub` arasındaki fark, `sub` ilk bulduğunu işler, `gsub` **Global** anlamındadır.

```ruby
"merhaba dünya, merhaba uzay".sub("merhaba", "olaa") # => "olaa dünya, merhaba uzay"
"merhaba dünya, merhaba uzay".gsub("merhaba", "olaa") # => "olaa dünya, olaa uzay"


"Merhaba Dünya".gsub(/[aeiou]/, "x") # => "Mxrhxbx Dünyx"
"Merhaba Dünya".gsub(/([aeiou])/, '(\1)') # => "M(e)rh(a)b(a) Düny(a)"
"Merhaba dünya, merhaba uzay, merhaba evren".gsub(/((m|M)erhaba)/){|c| c.upcase } # => "MERHABA dünya, MERHABA uzay, MERHABA evren"
"Merhaba Dünya".gsub(/(?<sesli_harf>[aeiou])/, '{\k<sesli_harf>}') # => "M{e}rh{a}b{a} Düny{a}"
"Merhaba Dünya".gsub(/[ea]/, 'e' => 1, 'a' => 'X') # => "M1rhXbX DünyX"
```

**match**

```ruby
"merhaba".match("a") # => #<MatchData "a">

"merhaba".match("(a)") # => #<MatchData "a" 1:"a"> # 1 tane yakaladı, (a) ve Array geldi
"merhaba".match("(a)")[0] # => "a" # yakalanan

"merhaba 2014".match(/\d/) # => #<MatchData "2">

"merhaba 2014".match(/(\d)/) # => #<MatchData "2" 1:"2">
"merhaba 2014".match(/(\d+)/) # => #<MatchData "2014" 1:"2014">
"merhaba 2014".match(/(\d+)/)[0] # => "2014"
"merhaba 2014".match(/(\d+)/)[0].to_i # => 2014
```

**scan**

**Match** gibi, metin üzerinde bir nevi arama yapıyoruz:

```ruby
"Merhaba millet!".scan(/\w+/) # => ["Merhaba", "millet"]
"Merhaba millet!".scan(/./) # => ["M", "e", "r", "h", "a", "b", "a", " ", "m", "i", "l", "l", "e", "t", "!"]
"Merhaba millet! Saat 10'da buluşalım".scan(/Saat \d+/) # => ["Saat 10"]
```

## Array

Kompakt, sıralı objeler içeren bir türü taşıcıyı nesnedir Array'ler. Neredeyse içinde her tür Ruby objesini taşıyabilir. (_String, Integer, Fixnum, Hash, Symbol hatta başka Array'ler vs..._)

Arka arkaya sıralanmış kutucuklar düşünün, her kutucuğun bir **index** numarası olduğunu hayal edin. Bu indeksleme işi otomatik olarak oluyor. Diziye her eleman eklediğinizde (_Eğer kendiniz indeks numarası atamadıysanız_) yeni indeks bir artarak devam ediyor. **Zero Indexed** yani ilk eleman **0**.eleman oluyor.

Aynı **String**'deki gibi negatif indeks değerleri ile tersten erişim sağlıyorsunuz. Yani ilk eleman `array[0]` ve son eleman `array[-1]` gibi...

Array oluşturmak için;

```ruby
a = []  # Ya bu şekilde
a.class # => Array

b = Array.new # => Ya da bu şekilde
b.class       # => Array
```

kullanabilirsiniz. Keza `a = [1, 2, 3]` dediğinizde de Array'i hem tanımlamış hem de elemanlarını belirlemiş olursunuz.

Array'i **initialize** ederken yani ilk kez oluştururken büyüklüğünü de verebilirsiniz.

```ruby
a = Array.new(5)  # içinde 5 eleman taşıyacak Array oluştur
a # => [nil, nil, nil, nil, nil]
```

Hatta, **default** değer bile atarsınız:

```ruby
aylar = Array.new(12, "ay") # 12 eleman olsun ve hepsi "ay" olsun
aylar # => ["ay", "ay", "ay", "ay", "ay", "ay", "ay", "ay", "ay", "ay", "ay", "ay"]
```

Ruby'de her nesnenin bir **ID**'si ve **HASH** değeri vardır.

```ruby
[1, 2, 3].hash   # => 3384159031637530117
[1, 2, 3].__id__ # => 70147646473880
```

**Block** kabul ettiği için;

```ruby
dizi = Array.new(10) { |eleman| eleman = eleman * 4 }
dizi # => [0, 4, 8, 12, 16, 20, 24, 28, 32, 36]

# ya da aynı kodu

dizi = Array.new(10) do |eleman|
  eleman = eleman * 4
end
dizi # => [0, 4, 8, 12, 16, 20, 24, 28, 32, 36]
```

şeklinde de kullanabilirsiniz. Neticede Array'in **initialize** method'una parametre geçmiş oluyoruz:

```ruby
aylar = Array.[]("oca", "şub", "mar", "nis", "may", "haz", "tem", "ağu", "eyl", "eki", "kas", "ara")
aylar # => ["oca", "şub", "mar", "nis", "may", "haz", "tem", "ağu", "eyl", "eki", "kas", "ara"]
```

Yani: `aylar = Array.[](param, param, param)` gibi.

Başka nasıl üretiriz? Sayılardan oluşan bir dizi lazım olsa; Örneğin **1984** ile **2000** arasındaki yıllar lazım olsa;

```ruby
years = Array(1984..2000)
years # => [1984, 1985, 1986, 1987, 1988, 1989, 1990, 1991, 1992, 1993, 1994, 1995, 1996, 1997, 1998, 1999, 2000]
```

Bir Array içinde farklı türden elemanlar olabilir;

```ruby
a = ["Hello", :word, 2014, 3.14] # içinde String, Symbol, Fixnum ve Float var!
a # => ["Hello", :word, 2014, 3.14]
```

Array içindeki elemanlar sıralı bir şekilde dururlar. Bu sıraya **Index** denir. **0**'dan başlar. Yani ilk eleman demek Array'in 0.elemanı demektir. İsteğimiz elemanı almak için ya `Array[index]` ya da `Array.fetch(index)` yöntemlerini kullanabiliriz.

```ruby
a = ["Uğur", "Yeşim", "Ezel", "Ömer"]
a[0]                       # => "Uğur"
a.fetch(0)                 # => "Uğur"

a[4]                       # => nil
a.fetch(4, "Hatalı Index") # => "Hatalı Index"

a = [1, 2, 3, 4]           # İlk N elemanı al
a.take(2)                  # => [1, 2]

a.drop(2) # => [3, 4]      # take'in tersi... İlk 2 haricini al
```

Örnekte `a[4]` dediğimiz zaman, olmayan index'li elemanı almaya çalışıyor ve eleman olmadığı için `nil` geri alıyoruz. `fetch` kullanarak hata kontrolü de yapmış oluyoruz. `nil` yerine belirlediğimiz hata mesajını dönmüş oluyoruz.

`values_at` method'u ile ilgili index ya da index'lerdeki elemanları alabiliriz, keza `at` de benzer işe yarar.

```ruby
isimler = ["Uğur", "Ömer", "Yeşim", "Ezel", "Eren"]
isimler.values_at(0)         # => ["Uğur"]
isimler.values_at(1, 2)      # => ["Ömer", "Yeşim"]

["a", "b", "c", "d", "e"].at(1)  # => "b"
["a", "b", "c", "d", "e"].at(-1) # => "e"
```

`rindex` ile sağdan hizalı index'e göre elemana ulaşıyoruz:

```ruby
a = [ "a", "b", "b", "b", "c" ]

a.rindex("b") # => 3 # 3 tane b var, en sağdaki son b'yi verdi!
a.rindex("z") # => nil
```

Keza "acaba Array'in ne gibi method'ları var?" dersek; hemen `methods` özelliği ile bakabiliriz. Karıştırmamamız gereken önemli bir konu var. Detayını **Class** konusunda göreceğiz ama yeri gelmişken, Array'in **Class Method**'ları ve **Instance Method**'ları var.

`Array.methods` dediğimizde Kernel'dan gelen Array objesinin yani **Class**'ının method'larını görürüz. Eğer `Array.new.methods` dersek, Array'den türettiğimiz **instance**'a ait method'ları görürüz.

Yani `a = []` dediğimizde, aslında **Array**'den bir **instance** çıkartmış oluyoruz. Az önce `Array.[](...)` olarak yaptığımız şey de aslında Class method'u çağırmak.

### Class Method'ları

```ruby
Array.methods # => [:[], :try_convert, :allocate, :new, :superclass, :freeze, :===, :==, :<=>, :<, :<=, :>, :>=, :to_s, :inspect, :included_modules, :include?, :name, :ancestors, :instance_methods, :public_instance_methods, :protected_instance_methods, :private_instance_methods, :constants, :const_get, :const_set, :const_defined?, :const_missing, :class_variables, :remove_class_variable, :class_variable_get, :class_variable_set, :class_variable_defined?, :public_constant, :private_constant, :singleton_class?, :include, :prepend, :module_exec, :class_exec, :module_eval, :class_eval, :method_defined?, :public_method_defined?, :private_method_defined?, :protected_method_defined?, :public_class_method, :private_class_method, :autoload, :autoload?, :instance_method, :public_instance_method, :nil?, :=~, :!~, :eql?, :hash, :class, :singleton_class, :clone, :dup, :taint, :tainted?, :untaint, :untrust, :untrusted?, :trust, :frozen?, :methods, :singleton_methods, :protected_methods, :private_methods, :public_methods, :instance_variables, :instance_variable_get, :instance_variable_set, :instance_variable_defined?, :remove_instance_variable, :instance_of?, :kind_of?, :is_a?, :tap, :send, :public_send, :respond_to?, :extend, :display, :method, :public_method, :singleton_method, :define_singleton_method, :object_id, :to_enum, :enum_for, :equal?, :!, :!=, :instance_eval, :instance_exec, :__send__, :__id__]
```

Bu method'ların bir kısmı **Enumerable** sınıfından gelen method'lardır. Ruby, **Module** yapısı kullandığı için ortak kullanılan method'lar modül eklemelerinden gelmektedir. **Class** konusunda detayları göreceğiz.

Bu kısımdan en fazla kullanacağımız `[]` ve `new` method'ları olacaktır.

### Instance Method'ları

En çok kullanacağımız method'larsa;

```ruby
Array.new.methods # => [:inspect, :to_s, :to_a, :to_h, :to_ary, :frozen?, :==, :eql?, :hash, :[], :[]=, :at, :fetch, :first, :last, :concat, :<<, :push, :pop, :shift, :unshift, :insert, :each, :each_index, :reverse_each, :length, :size, :empty?, :find_index, :index, :rindex, :join, :reverse, :reverse!, :rotate, :rotate!, :sort, :sort!, :sort_by!, :collect, :collect!, :map, :map!, :select, :select!, :keep_if, :values_at, :delete, :delete_at, :delete_if, :reject, :reject!, :zip, :transpose, :replace, :clear, :fill, :include?, :<=>, :slice, :slice!, :assoc, :rassoc, :+, :*, :-, :&, :|, :uniq, :uniq!, :compact, :compact!, :flatten, :flatten!, :count, :shuffle!, :shuffle, :sample, :cycle, :permutation, :combination, :repeated_permutation, :repeated_combination, :product, :take, :take_while, :drop, :drop_while, :bsearch, :pack, :entries, :sort_by, :grep, :find, :detect, :find_all, :flat_map, :collect_concat, :inject, :reduce, :partition, :group_by, :all?, :any?, :one?, :none?, :min, :max, :minmax, :min_by, :max_by, :minmax_by, :member?, :each_with_index, :each_entry, :each_slice, :each_cons, :each_with_object, :chunk, :slice_before, :lazy, :nil?, :===, :=~, :!~, :class, :singleton_class, :clone, :dup, :taint, :tainted?, :untaint, :untrust, :untrusted?, :trust, :freeze, :methods, :singleton_methods, :protected_methods, :private_methods, :public_methods, :instance_variables, :instance_variable_get, :instance_variable_set, :instance_variable_defined?, :remove_instance_variable, :instance_of?, :kind_of?, :is_a?, :tap, :send, :public_send, :respond_to?, :extend, :display, :method, :public_method, :singleton_method, :define_singleton_method, :object_id, :to_enum, :enum_for, :equal?, :!, :!=, :instance_eval, :instance_exec, :__send__, :__id__]
```

Aynı **String**'deki gibi, şu Array'in bir röntgenini çekelim:

```ruby
Array.class # => Class
Array.class.superclass # => Module
Array.class.superclass.superclass # => Object
Array.class.superclass.superclass.superclass # => BasicObject
Array.class.superclass.superclass.superclass.superclass # => nil
```

Array'in bir üst objesi ne? **Module** Yine **Class** konusunda göreceğiz diyeceğim ve siz bana kızacaksınız :) Ruby'de bir Class en fazla başka bir Class'dan türeyebilir. Örneğin Python'da bir Class N TANE Class'tan **inherit** olabilir (_Miras alabilir, türeyebilir_)

Ruby, bu sorunu **Module** yapısıyla çözüyor. Bu mantıkla aslında ortaklaşa kullanılan Kernel modülleri yardımıyla, ortak kullanılacak method'lar bu modüllerin **Include** edilmesiyle ilgili yerlere dağıtılıyor.

Acaba Array'de hangi modüller var?

```ruby
Array.included_modules # => [Enumerable, Kernel]
```

Bu bakımdan Array, Hash gibi nesnelerde benzer ortak method'lar görmek mümkün.

**length** veya **count**

Array'in boyu / içinde kaç eleman olduğu ile ilgili bilgiyi almak için kullanılır.

```ruby
[1, 2, 3, 4].length # => 4
[1, 2, 3, 4].count  # => 4
```

**empty?**

Array acaba boşmu? İçinde hiç eleman var mı?

```ruby
[1, 2, 3, 4].empty? # => false
[].empty?           # => true
```

**eql?**, **==**, **===**

Eşitlik kontrolü içindir. Eğer karşılığı aynı cinsse ve birebir aynı elemanlara sahipse `true` döner.

```ruby
a = ["Uğur", "Yeşim", "Ezel", "Ömer"]

a.eql?(["Yeşim", "Ezel", "Ömer", "Uğur"]) # => false
a.eql?([])                                # => false
a.eql?(["Uğur", "Yeşim", "Ezel", "Ömer"]) # => true
```

`==` **Generic Equality** yani genel eşitlik kontrolü yani hepimizin bildiği kontrol, `===` ise **Case Equality** yani `a === b` ifadesinde **a**, **b**'nin **subseti** mi? demek olur. Örnek verelim:

```ruby
5.class.superclass # => Integer
Integer === 5      # => true
# 5, Integer subsetinde...

Integer.class # => Class
Integer.class.superclass # => Module
Integer.class.superclass.superclass # => Object
Integer.class.superclass.superclass.superclass # => BasicObject
Integer.class.superclass.superclass.superclass.superclass # => nil

# Integer, 5'in subsetinde değil.
5 === Integer      # => false
```

**include?** ve **member?**

Acaba verdiğim eleman Array'in içinde mi? Verdiğim eleman bu dizinin üyesi mi?

```ruby
[1, 2, 3, 4].include?(3)                   # => true

[1, 2, 3, 4].member?(1) # => true
[1, 2, 3, 4].member?(5) # => false

["Uğur", "Ezel", "Yeşim"].include?("Uğur") # => true
["Uğur", "Ezel", "Yeşim"].include?("Ömer") # => false
```

**array & başka_bir_array**

İki dizide de kullanın ortak elemanları alır yeni Array döner:

```ruby
a = [1, 2, 3, 4]
b = [3, 1, 10, 22]
a & b # => [1, 3]
```

**array \* int [ya da] array \* str**

```ruby
a = ["a", "b", "c"]
a * 5 # => ["a", "b", "c", "a", "b", "c", "a", "b", "c", "a", "b", "c", "a", "b", "c"]

a * "-vigo-" # => "a-vigo-b-vigo-c"
```

**\*** çaparak **3** elemanlı `a` Array'inden sanki birleştirilmiş **15** elemanlı yeni bir Array oluşturduk. **String** ile çarpınca da aslında `join` methodu ile Array'den String yaptık ve birleştirici olarak **-vigo-** metni kullandık!

**array + başka_array**

İki Array'i toplar ve yeni Array döner:

```ruby
a = ["Uğur", "Yeşim", "Ezel"]
b = ["Ömer"]

a + b # => ["Uğur", "Yeşim", "Ezel", "Ömer"]
```

**array - başka_array**

Array'ler arasındaki farkı Array olarak bulmak:

```ruby
a = ["Uğur", "Yeşim", "Ezel"]
b = ["Uğur", "Yeşim"]

a - b # => ["Ezel"] # a'da olab b elemanları kayboldu
```

**array | başka_array**

İki Array'i **unique** (_tekil_) elemanlar olarak birleştirdi. Aynı eleman varsa bunlardan birini aldı:

```ruby
a = ["Uğur", "Yeşim", "Ezel"]
b = ["Uğur", "Ömer"]

a | b # => ["Uğur", "Yeşim", "Ezel", "Ömer"]
```

**array << nesne** ya da **push**

Array'in sonuna eleman eklemek için kullanılır.

```ruby
a = ["Uğur", "Yeşim", "Ezel"]
a << "Ömer"     # => ["Uğur", "Yeşim", "Ezel", "Ömer"]
a.push("Eren")  # => ["Uğur", "Yeşim", "Ezel", "Ömer", "Eren"]
```

Keza zincirleme çağrı da yapabilirsiniz:

```ruby
a.push("Tunç").push("Suat")  # => ["Uğur", "Yeşim", "Ezel", "Ömer", "Eren", "Tunç", "Suat"]
```

**concat**

Array sonuna Array eklemek için kullanılır.

```ruby
a = [1, 2, 3]
a.concat([4, 5, 6])
a # => [1, 2, 3, 4, 5, 6]
```

**join**

Array elemanlarını birleştirip **String**'e çevirmeye yarar. Eğer parametre verirsek aradaki birleştiriciyi de belirlemiş oluruz.

```ruby
["Commodore 64", "Amiga", "Sinclair", "Amstrad"].join        # => "Commodore 64AmigaSinclairAmstrad"
["Commodore 64", "Amiga", "Sinclair", "Amstrad"].join(" , ") # => "Commodore 64 , Amiga , Sinclair , Amstrad"
```

**unshift**

Array'in başına eleman eklemek için kullanılır.

```ruby
a = ["Uğur", "Yeşim", "Ezel"]
a.unshift("Ömer") # => ["Ömer", "Uğur", "Yeşim", "Ezel"]
```

**insert**

Array'de istediğiniz bir noktaya eleman eklemek için kullanılır. İlk parametre **index** diğer parametre/ler de eklenecek eleman/lar.

```ruby
a = ["Uğur", "Yeşim", "Ezel"]
a.insert(1, "Ömer") # => ["Uğur", "Ömer", "Yeşim", "Ezel"]
a.insert(1, "Ahmet", "Ece", "Eren") # => ["Uğur", "Ahmet", "Ece", "Eren", "Ömer", "Yeşim", "Ezel"]
```

**replace**

Array'in içini, diğer Array'le değiştirir. Aslında Array'i başka bir Array'e eşitlemek gibidir. Eleman sayısının eşit olup olmaması hiç önemli değildir.

```ruby
a = ["Uğur", "Yeşim", "Ezel", "Ömer"]
a.replace(["Foo", "Bar"]) # => ["Foo", "Bar"]
a                         # => ["Foo", "Bar"]
```

**array <=> başka_array**

**Spaceship** operatöründen bahsetmiştik. Array'ler arasında karşılaştırma yapmayı sağlar.

```ruby
[1, 2, 3, 4] <=> [1, 2, 3, 4] # => 0 # Eşit
[1, 2, 3, 4] <=> [1, 2, 3]    # => 1 # İlk değer büyük
[1, 2, 3] <=> [1, 2, 3, 4]    # => -1 # İlk değer küçük
```

**pop**, **shift**, **delete**, **delete_at**, **delete_if**

Son elemanı çıkartmank için **pop** ilk elemanı çıkartmak için **shift** kullanılır. Herhangi bir elemanı çıkartmak için **delete**, belirli bir index'deki elemanı çıkartmak için **delete_at** kullanılır.

```ruby
a = ["Uğur", "Ömer", "Yeşim", "Ezel", "Eren"]
a.pop              # => "Eren"
a                  # => ["Uğur", "Ömer", "Yeşim", "Ezel"]
a.shift            # => "Uğur"
a                  # => ["Ömer", "Yeşim", "Ezel"]
a.delete("Ömer")   # => "Ömer"
a                  # => ["Yeşim", "Ezel"]
a.delete_at(1)     # => "Ezel"
a                  # => ["Yeşim"]

# not 50'den küçükse sil :)
notlar = [40, 45, 53, 70, 99, 65]
notlar.delete_if { |notu| notu < 50 } # => [53, 70, 99, 65]
```

**pop**'a parametre geçersek **son n** taneyi uçurmuş oluruz:

```ruby
a = ["Uğur", "Ömer", "Yeşim", "Ezel", "Eren"]
a.pop(2) # => ["Ezel", "Eren"]
a        # => ["Uğur", "Ömer", "Yeşim"]
```


**compact** ve **uniq**

`nil` elemanları uçurmak için **compact**, duplike elemanları tekil hale getirmek için **uniq** kullanılır.

```ruby
["a", 1, nil, 2, nil, "b", 1, "a"].compact         # => ["a", 1, 2, "b", 1, "a"]
["a", 1, nil, 2, nil, "b", 1, "a"].uniq            # => ["a", 1, nil, 2, "b"]
["a", 1, nil, 2, nil, "b", 1, "a"].compact.uniq    # => ["a", 1, 2, "b"]
```

**array == başka_array**

İki Array nitelik ve nicelik olarak birbirine eşit mi?

```ruby
[1, 2, 3, 4] == [1, 2, 3, 4]   # => true
[1, 2, 3, 4] == ["1", 2, 3, 4] # => false
[1, 2, 3, 4] == [1, 2, 3]      # => false
```

**assoc** ve **rassoc**

Elemanları Array olan bir Array içinde, ilk değere göre yakalama yapmaya yarar.

```ruby
a = ["renkler", "kırmızı", "sarı", "mavi"]
b = ["harfler", "a", "b", "c"]
c = "foo"

t  = [a, b, c]
t                 # => [["renkler", "kırmızı", "sarı", "mavi"], ["harfler", "a", "b", "c"], "foo"]
t.assoc("renkler") # => ["renkler", "kırmızı", "sarı", "mavi"]
t.assoc("foo")     # => nil

t.rassoc("kırmızı")   # => ["renkler", "kırmızı", "sarı", "mavi"]
```

`rassoc` ise ikinci elemanına bakar, yani "renkler" yerine "kırımızı" kullanabiliriz:

**slice(başlangıç, boy) ya da slice(aralık)**

Array içinden kesip başka bir Array oluşturmak için kullanılır. **başlangiç** indeks'indeki eleman dahil olmak üzere, boy ya da aralık kadarını kes.

```ruby
[1, 2, 3, 4].slice(0, 2) # => [1, 2] # 0.dan itibaren 2 tane
[1, 2, 3, 4].slice(2..4) # => [3, 4] # 2.den itibaren 2 tane
```

**first** ve **last**

Adından da anlaşılacağı gibi, Array'in ilk ve son elemanları için kullanılır:

```ruby
a = [1, 2, 3, 4, 5]
a.first # => 1
a.last # => 5
```

Eğer parametre geçersek, **ilk n** ya da **son n** elemanları alabiliriz:

```ruby
a = [1, 2, 3, 4, 5]
a.first(2) # => [1, 2]
a.last(2)  # => [4, 5]
```

**find** (**detect**), **find_all**, **index**, **find_index**

`find` ile blok içinde koşula uyan ilk Array elemanını, `find_all` ile tümünü alırız:

```ruby
["Uğur", "Yeşim", "Ezel", "Ömer"].find { |n| n.length > 3 } # => "Uğur"
["Uğur", "Yeşim", "Ezel", "Ömer"].find_all { |n| n.length > 3 } # => ["Uğur", "Yeşim", "Ezel", "Ömer"]
```

`detect` ile `find` aynı işi yapar.

`index`, `find_index` ile elemanın index'ini buluruz:

```ruby
["a", "b", "c", "d", "e"].index("e")                 # => 4
["Uğur", "Yeşim", "Ezel", "Ömer"].index("Ezel")      # => 2
["Uğur", "Yeşim", "Ezel", "Ömer"].find_index("Ezel") # => 2
```

**clear**

Array'i temizlemek için kullanılır :)

```ruby
a = [1, 2, 3]
a.clear # => []
a       # => []
```

**reverse**

Array'i terse çevir.

```ruby
a = [1, 2, 3, 4, 5]
a.reverse # => [5, 4, 3, 2, 1]
```

**sample**

Array'den **random** olarak eleman almaya yarar. Eğer parametre geçilirse geçilen adet kadar random eleman döner.

```ruby
a = [1, 2, 3, 4, 5]
a.sample    # => 3
a.sample(3) # => [5, 1, 3]
```

**shuffle**

Array'in içindeki elemanların index'lerini karıştırı :)

```ruby
a = [1, 2, 3, 4, 5]
a.shuffle # => [5, 4, 1, 3, 2]
a.shuffle # => [1, 2, 3, 5, 4]
```

**sort**

Array içindeki elemanları `<=>` mantığıyla sıralar.

```ruby
a = [1, 4, 2, 3, 11, 5]
a.sort # => [1, 2, 3, 4, 5, 11]

b = ["a", "c", "b", "z", "d"]
b.sort # => ["a", "b", "c", "d", "z"]
```

**fill**

Array'in içini ilgili değerle doldurmak için kullanılır. İşlem sonucunda orijinal Array'in değeri değişir. Yani ne ile **fill** ettiyseniz Array artık o değerlerdedir.

```ruby
a = ["Uğur", "Yeşim", "Ezel", "Ömer"]
a.fill("x")       # => ["x", "x", "x", "x"] # tüm elemanları x yaptı
a                 # => ["x", "x", "x", "x"] # artık a'nın yeni değeri bu

a = ["Uğur", "Yeşim", "Ezel", "Ömer"]
a.fill("y", 2)    # => ["Uğur", "Yeşim", "y", "y"] # 2.den itibaren y ile doldur

a = ["Uğur", "Yeşim", "Ezel", "Ömer"] # 2.den itibaren 1 tane doldur
a.fill("z", 2, 1) # => ["Uğur", "Yeşim", "z", "Ömer"]
```

Keza;

```ruby
a = [1, 2, 3, 4, 5]
a.fill { |i| i * 5 }     # => [0, 5, 10, 15, 20]
a                        # => [0, 5, 10, 15, 20]
```

şeklinde de kullanılır.

**flatten**

Array içinde Array elemanları varsa, tek harekette bunları düz tek bir Array haline getirebiliriz.

```ruby
[1, 2, ["a", "b", :c], [66, [5.5, 3.1]]].flatten # => [1, 2, "a", "b", :c, 66, 5.5, 3.1]
```

**rotate**

Array elemanları kendi içinde kaydırır.

```ruby
a = [1, 2, 3, 4, 5]

a.rotate    # => [2, 3, 4, 5, 1] # 1 kaydırdı
a.rotate(2) # => [3, 4, 5, 1, 2] # 2 kaydırdı, ilk 2 elemanı sona koydu!
```

Varsayılan değer **1**'dir.

**zip**

```ruby
a = [ 4, 5, 6 ]
b = [ 7, 8, 9 ]

[1, 2, 3].zip(a, b)   # => [[1, 4, 7], [2, 5, 8], [3, 6, 9]]
[1, 2].zip(a, b)      # => [[1, 4, 7], [2, 5, 8]]
a.zip([1, 2], [8])    # => [[4, 1, 8], [5, 2, nil], [6, nil, nil]]
```

`[1, 2, 3].zip(a, b)` işlemini yaparken, önce 0.elemanı yani **1**'i aldı, sonra **a**'nun 0.elemanını aldı, sonra da **b**'nin 0.elemanını aldı ve paketledi : `[1, 4, 7]` aynı işi 1. ve 2. elemanlar için yaptı.

`[1, 2].zip(a, b)` yaparken, Array boyları eşit olmadığı için `[1, 2]` sadece 2 elemanlı olduğu için bu işlemi 0. ve 1. elemanlar için yaptı.

Son örnekte index'e karşılık gelmediği için elemanlar `nil` geldi!

**transpose**

Array içindeki Array'leri satır gibi düşünüp bunları sütuna çeviriyor gibi algılayabilirsiniz.

```ruby
a = [[1, 2], [3, 4], [5, 6]]
a.transpose   # => [[1, 3, 5], [2, 4, 6]]

# [
#   [1, 2],
#   [3, 4],
#   [5, 6]
# ]
# -> [1, 3, 5], [2, 4, 5]
#
```

### Tip Çeviricileri

`to_a` ve `to_ary` kendisini döner, asıl görevi eğer alt sınıftan çağrılmışsa, yani Array'den türeyen başka bir Class'da kullanıldığında direk Array'e dönüştürür.

```ruby
["a", 1, "b", 2].to_a       # => ["a", 1, "b", 2]
["a", 1, "b", 2].to_ary     # => ["a", 1, "b", 2]
[["a", 1], ["b", 2]].to_h   # => {"a"=>1, "b"=>2}
["a", 1, "b", 2].to_s       # => "[\"a\", 1, \"b\", 2]"
["a", 1, "b", 2].inspect    # => "[\"a\", 1, \"b\", 2]"
```

`entries` de aynen `to_a` gibi çalışır:

```ruby
(1..3)         # => 1..3
(1..3).entries # => [1, 2, 3]
(1..3).to_a    # => [1, 2, 3]
```

**grep**

Aslında bu konuları **Regular Expressions**'da göreceğiz ama yeri gelmişken hızla değinelim. Array içinde elemanları **Regex** koşullarına göre filtreleyebiliyoruz:

```ruby
(1..10).to_a        # => [1, 2, 3, 4, 5, 6, 7, 8, 9, 10]

# 2'den 5'e kadar (5 dahil)
(1..10).grep 2..5   # => [2, 3, 4, 5]

# sadece .com olan elemanları al
["a", "http://example.com", "b", "foo", "http://webbox.io"].grep(/^http.+\.com/) # => ["http://example.com"]
```

**pack**

Array'in içeriğini verilen direktife göre **Binary String** haline getirir. Uzunca bir [direktif listesi](http://www.ruby-doc.org/core-2.1.2/Array.html#method-i-pack) var.

```ruby
# A: String olarak işle, space karakteri kullan
# 5: Uzunluğu 5 karakter olsun
["a", "b", "c"].pack("A5A5A5")            # => "a    b    c    "

# Uzunluğu 5'ten büyük olan kesintiye uğradı
["ali", "burak", "cengiz"].pack("A5A5A5") # => "ali  burakcengi"

# a: String olarak işle, null yani \x00 karakteri kullan
["a", "b", "c"].pack("a3a3a3")            # => "a\x00\x00b\x00\x00c\x00\x00"
```

### İterasyon ve Block Kullanımı

**collect / map { |eleman| blok } → yeni_array**

Blok içinde gelen kodu her elemana uygular, yeni Array döner:

```ruby
a = [1, 2, 3, 4, 5]
a.collect { |i| i * 2 }       # => [2, 4, 6, 8, 10]
a.collect { |i| "sayı #{i}" } # => ["sayı 1", "sayı 2", "sayı 3", "sayı 4", "sayı 5"]
```

`map` de aynı işi yapar:

```ruby
["Uğur", "Yeşim", "Ezel", "Ömer"].map { |isim| "İsim: #{isim}" } # => ["İsim: Uğur", "İsim: Yeşim", "İsim: Ezel", "İsim: Ömer"]
["Uğur", "Yeşim", "Ezel", "Ömer"].collect { |isim| "İsim: #{isim}" } # => ["İsim: Uğur", "İsim: Yeşim", "İsim: Ezel", "İsim: Ömer"]
```

**select**

Blok içinden gelen ifadenin **true** / **false** olmasına göre filtre yapar ve yeni Array döner:

```ruby
[1, 2, 3, 10, 15, 20].select { |n| n % 2 == 0 } # => [2, 10, 20] # 2'ye tam bölünenler
[1, 2, "3", "ali", 15, 20].select { |n| n.is_a?(Fixnum) } # => [1, 2, 15, 20] # sadece sayılar
```

**reject**

`select`in tersidir.

```ruby
[1, 2, 3, 10, 15, 20].reject { |n| n % 2 == 0 } # => [1, 3, 15] # 2'ye tam bölülenleri at
[1, 2, "3", "ali", 15, 20].reject { |n| n.is_a?(Fixnum) } # => ["3", "ali"] # Sayı olanları at
```

**keep_if**

Blok içindeki ifade'den sadece `false` dönenleri atar ve Array'in orjinal değerini bozar, değiştirir.

```ruby
a = [1, 2, 3, 10, 15, 20]
a.keep_if { |n| n % 2 == 0 } # => [2, 10, 20] # 2'ye bölünemeyenler false geldiği için düştüler.
a # => [2, 10, 20] # a artık bu!
```

**combination(n) { |c| blok } → array**

Matematikteki kombinasyon işlemidir. 1, 2 ve 3 sayılarının 2'li kombinasyonu:

```ruby
a = [1, 2, 3]
a.combination(1).to_a # => [[1], [2], [3]]
a.combination(2).to_a # => [[1, 2], [1, 3], [2, 3]]

a.combination(2) { |c| puts "Olasıklar: #{c.join(" ve ")}" }

# Olasıklar: 1 ve 2
# Olasıklar: 1 ve 3
# Olasıklar: 2 ve 3
```

**permutation**

Aynı kombinasyon gibi, matematikteki permutasyon işlemidir.

```ruby
[1, 2, 3].permutation.to_a # => [[1, 2, 3], [1, 3, 2], [2, 1, 3], [2, 3, 1], [3, 1, 2], [3, 2, 1]]
```

Eğer parametre geçersek kaçlı permutasyon olduğunu belirtiriz:

```ruby
[1, 2, 3].permutation(1).to_a # => [[1], [2], [3]]
[1, 2, 3].permutation(2).to_a # => [[1, 2], [1, 3], [2, 1], [2, 3], [3, 1], [3, 2]]
```

**repeated_combination**, **repeated_permutation**

`combination` ile `repeated_combination` arasındaki farkı örnekle görelim:

```ruby
[1, 2, 3].combination(1).to_a # => [[1], [2], [3]]
[1, 2, 3].repeated_combination(1).to_a # => [[1], [2], [3]]

[1, 2, 3].combination(2).to_a # => [[1, 2], [1, 3], [2, 3]]
[1, 2, 3].repeated_combination(2).to_a # => [[1, 1], [1, 2], [1, 3], [2, 2], [2, 3], [3, 3]]
```

`combination` olası tekil sonucu, `repeated_combination` pas edilen sayıya göre tekrar da edebilen sonucu döner. Aynısı `repeated_permutation` için de geçerlidir:

```ruby
[1, 2, 3].permutation(1).to_a # => [[1], [2], [3]]
[1, 2, 3].repeated_permutation(1).to_a # => [[1], [2], [3]]

[1, 2, 3].permutation(2).to_a # => [[1, 2], [1, 3], [2, 1], [2, 3], [3, 1], [3, 2]]
[1, 2, 3].repeated_permutation(2).to_a # => [[1, 1], [1, 2], [1, 3], [2, 1], [2, 2], [2, 3], [3, 1], [3, 2], [3, 3]]
```

**product**

Array ve argüman olarak geçilecek diğer Array/lerin elemanlarıyla oluşabilecek tüm alternatifleri üretmenizi sağlar.

```ruby
[1, 2, 3].product            # => [[1], [2], [3]]
[1, 2, 3].product([4, 5])    # => [[1, 4], [1, 5], [2, 4], [2, 5], [3, 4], [3, 5]]
[1, 2, 3].product([7, 8, 9]) # => [[1, 7], [1, 8], [1, 9], [2, 7], [2, 8], [2, 9], [3, 7], [3, 8], [3, 9]]
[1, 2, 3].product(["a", "b"], ["x", "y"]) # => [[1, "a", "x"], [1, "a", "y"], [1, "b", "x"], [1, "b", "y"], [2, "a", "x"], [2, "a", "y"], [2, "b", "x"], [2, "b", "y"], [3, "a", "x"], [3, "a", "y"], [3, "b", "x"], [3, "b", "y"]]
```

**count**

Az önce method olarak işlediğimiz `count` ile başka ilginç işler de yapabiliyoruz:

```ruby
a = [1, 2, 3, 4, 2]

a.count                    # => 5 # eleman sayısı
a.count(2)                 # => 2 # kaç tane 2 var?
a.count { |n| n % 2 == 0 } # => 3 # kaç tane 2'ye tam bölünen var?
```

**cycle(n=nil) { |obje| blok } → nil**

Pas edilen blok'u **n** defa tekrar eder.

```ruby
a = [1, 2, 3]

a.cycle(2).to_a # => [1, 2, 3, 1, 2, 3] # 2 defa
a.cycle(4).to_a # => [1, 2, 3, 1, 2, 3, 1, 2, 3, 1, 2, 3] # 3defa
a.cycle(2) { |o| puts "Sayı #{o}" }

# Sayı 1
# Sayı 2
# Sayı 3
# Sayı 1
# Sayı 2
# Sayı 3
```

Eğer `[1, 2, 3].cycle { |i| puts i }` gibi bir işlem yaparsanız, default olarak `nil` geçmiş olursun ve bu sonsuz döngüle girer, sonsuza kadar 1, 2, 3, 1, 2, 3 .... şeklinde devam eder!

**drop_while { |array| blok } → yeni array**

`delete_if` ile aynı işi yapar.

```ruby
notlar = [40, 45, 53, 70, 99, 65]
notlar.drop_while {|notu| notu < 50 }   # => [53, 70, 99, 65]
```

Koşula göre Array'den atar gibi düşünebilirsiniz. Not 50'den küçükse bırak.

**take_while**

Aynı `drop_while` gibi çalışır ama tersini yapar:

```ruby
notlar = [40, 45, 53, 70, 99, 65]
notlar.take_while { |notu| notu < 50 } # => [40, 45]
```

Koşula göre Array'e ekler gibi düşünebilirsiniz. Not 50'den küçükse sepete ekle! :)

**each**, **each_index**, **each_with_index**, **each_slice**, **each_cons**, **each_with_object**, **reverse_each**

Array ve hatta Enumator'lerin can damarıdır. Ruby yazarken siz de göreceksiniz `each` en sık kullandığınız iterasyon (_yineleme / tekrarlama_) yöntemi olacak.

```ruby
a = ["Uğur", "Yeşim", "Ezel", "Ömer"]

a.each # => #<Enumerator: ["Uğur", "Yeşim", "Ezel", "Ömer"]:each>
a.each { |isim| puts "İsim: #{isim}" }

# İsim: Uğur
# İsim: Yeşim
# İsim: Ezel
# İsim: Ömer
```

Array ve içinde dolaşılabilir her nesnede işe yarar. Birde bunun **index**'li hali var;

```ruby
a = ["Uğur", "Yeşim", "Ezel", "Ömer"]

a.each_index # => #<Enumerator: ["Uğur", "Yeşim", "Ezel", "Ömer"]:each_index>
a.each_index.to_a # => [0, 1, 2, 3]
a.each_index { |i| puts "Index: #{i}, Değeri: #{a[i]}" }

# Index: 0, Değeri: Uğur
# Index: 1, Değeri: Yeşim
# Index: 2, Değeri: Ezel
# Index: 3, Değeri: Ömer
```

ya da bu işi;

```ruby
a = ["Uğur", "Yeşim", "Ezel", "Ömer"]
a.each_with_index { |eleman, index| puts "index: #{index}, eleman: #{eleman}" }

# index: 0, eleman: Uğur
# index: 1, eleman: Yeşim
# index: 2, eleman: Ezel
# index: 3, eleman: Ömer
```

`each_slice` da Array'i gruplamak, parçalara ayırmak içindir. Geçilen parametre bu işe yarar:

```ruby
a = ["Uğur", "Yeşim", "Ezel", "Ömer"]
a.each_slice(2) # => #<Enumerator: ["Uğur", "Yeşim", "Ezel", "Ömer"]:each_slice(2)>
a.each_slice(2).to_a # => [["Uğur", "Yeşim"], ["Ezel", "Ömer"]]
a.each_slice(2) { |ikili_grup| puts "#{ikili_grup}" }

# ["Uğur", "Yeşim"]
# ["Ezel", "Ömer"]
```

`each_cons` ise slice gibi ama mutlaka belirtilen miktarda parça üretir.

```ruby
# 3'lü üret
[1, 2, 3, 4, 5,6].each_cons(3).to_a # => [[1, 2, 3], [2, 3, 4], [3, 4, 5], [4, 5, 6]]

# 4'lü üret
[1, 2, 3, 4, 5,6].each_cons(4).to_a # => [[1, 2, 3, 4], [2, 3, 4, 5], [3, 4, 5, 6]]
```

`each_with_object` de ise, iterasyona girerken bir nesne pas edip, o nesneyi doldurabilirsiniz.

```ruby
[1, 2, 3, 4].each_with_object([]) { |number, given_object|
  given_object << number * 2
} # => [2, 4, 6, 8]
```

`number` Array'den gelen eleman (_1, 2, 3, 4 gibi_), `given_object` ise `each_with_object([])` method'da geçtiğimiz boş Array `[]`.

`reverse_each` aslında Array'i otomatik olarak ters çevirir yani **reverse** eder ve içinde dolaşmanızı sağlar:

```ruby
computers = ["Commodore 64", "Amiga", "Sinclair", "Amstrad"]
computers.reverse_each # => #<Enumerator: ["Commodore 64", "Amiga", "Sinclair", "Amstrad"]:reverse_each>
computers.reverse_each.to_a # => ["Amstrad", "Sinclair", "Amiga", "Commodore 64"]

computers.reverse_each { |c| puts "Bilgisayar: #{c}" }

# Bilgisayar: Amstrad
# Bilgisayar: Sinclair
# Bilgisayar: Amiga
# Bilgisayar: Commodore 64
```

**find_index**

İndeks'i ararken blok işleyebiliriz:

```ruby
computers = ["Commodore 64", "Amiga", "Sinclair", "Amstrad"]
computers.find_index { |c| c == "Amstrad" } # => 3
```

**freeze** ve **frozen?**

Array'i kitlemek için kullanılır. Yani **freeze** (_dondurulmuş_) bir Array'e yeni eleman eklenemez. Keza `Array#sort` esnasında da otomatik olarak **freeze** olur sort bitince buz çözülür!

```ruby
a = ["Uğur", "Yeşim", "Ezel", "Ömer"]
a.freeze
a << "Fazilet"  # Yeni isim eklemek mümkün değildir!
```

    RuntimeError: can't modify frozen Array

Array'de buzlanma var mı yok mu anlamak için **frozen?** kullanırız:

```ruby
a = ["Uğur", "Yeşim", "Ezel", "Ömer"]
a.freeze  # => ["Uğur", "Yeşim", "Ezel", "Ömer"]
a.frozen? # => true
```

**min**, **max**, **minmax**, **min_by**, **max_by** ve **minmax_by**

`min` ve `max` ile Array elemanlarından en küçük/büyük değeri alırız:

```ruby
a = [6, 1, 8, 4, 11]
a.min # => 1
a.max # => 11
```

Peki sayı yerine metinler olsa ne olacaktı?

```ruby
m = ["a", "ab", "abc", "abcd"]
m.min # => "a"
m.max # => "abcd"
```

peki, `m` Array'i şöyle olsaydı : `m = ["a", "ab", "abc", "abcd", "111111111"]` sonuç ne olurdu?

```ruby
m = ["a", "ab", "abc", "abcd", "111111111"]

m.min # => "111111111" # ?
m.max # => "abcd"
```

Önce **Comparable** mı diyer bakılır, sayılar için çalışan bu yöntem, **String** de `a <=> b` karşılaştırmasına girer ve **Lexicological** karşılaştırma yapar. `"111111111"` karakter sayısı olarak diğerlerine göre çok olmasına rağmen, `min` değer olarak gelir. Eğer karakter sayına göre karşılaştırma yapmak gerekiyorsa;

```ruby
m = ["a", "ab", "abc", "abcd", "111111111"]
m.min { |a, b| a.length <=> b.length }  # => "a"
m.max                                   # => "abcd"
```

Şeklinde yapmak gerekir. Blok kullanabildiğimiz için aynı iş `max` için de geçerlidir. Ya da bu işleri yapabilmek için `min_by` ve `max_by` kullanabiliriz:

```ruby
m = ["a", "ab", "abc", "abcd", "111111111"]
m.min_by { |x| x.length }   # => "a"
m.max_by { |x| x.length }   # => "111111111"
```

`minmax` da Array'in minimum ve maximum'unu döner:

```ruby
m = ["a", "ab", "abc", "abcd"]
m.min    # => "a"
m.max    # => "abcd"
m.minmax # => ["a", "abcd"]
```

Aynı mantıkta `minmax_by` da gerekli şarta göre min, max döner:

```ruby
m = ["a", "ab", "abc", "abcd"]
m.minmax_by { |x| x.length } # => ["a", "abcd"]
```

**all?**, **any?**, **one?**, **none?**

Array içindeki elemanları belli bir koşula göre kontrol etmek için kullanılır. Sonuç **Boolean** yani `true` ya da `false` döner. Tüm elemanların kontrolü koşula uyuyorsa `true` uymuyorsa `false` döner.

```ruby
# acaba hayvanlar dizisindeki isimlerin hepsinin uzunluğu
# en az 2 karakter mi?

hayvanlar = ["Kedi", "Köpek", "Kuş", "Kurbağa", "Kaplumbağa"]
hayvanlar.all? { |hayvan_ismi| hayvan_ismi.length >= 2 } # => true

# Acaba ilk karfleri K harfimi?

hayvanlar.all? { |hayvan_ismi| hayvan_ismi.start_with?("K") } # => true

# Elemanların her biri true mu?
[true, false, nil].all? # => false
```

`any?` de yanlızca bir tanesi `true` olsa yeterlidir:

```ruby
# En azından bir hayvan ismi A ile başlıyor mu?

hayvanlar = ["Kedi", "Köpek", "At", "Yılan", "Balık"]
hayvanlar.any?{ |hayvan_ismi| hayvan_ismi.start_with?("A") } # => true
```

`one?` da ise sadece bir eleman koşula uymalıdır. Yani bir tanesi `true`dönmelidir. Eğer birden fazla eleman koşula `true` dönerse sonuç `false` olur:

```ruby
hayvanlar = ["Kedi", "Köpek", "At", "Yılan", "Balık", "Kaplumbağa"]

# Sadece bir ismin uzunluğu 6 karaterten büyük olmalı!
hayvanlar.one?{ |hayvan_ismi| hayvan_ismi.length > 6 } # => true

# Uzunluğu 3'ten büyük 5 isim olduğu için false döndü!
hayvanlar.one?{ |hayvan_ismi| hayvan_ismi.length > 3 } # => false
```

`none?` da ise hepsi `false` olmalıdır ki sonuç `true` dönsün:

```ruby
hayvanlar = ["Kedi", "Köpek", "At", "Yılan", "Balık", "Kaplumbağa"]

# Hiçbir ismin uzunluğu 2 karakter olmamalı ? false. At'ın uzunluğu 2
hayvanlar.none?{ |hayvan_ismi| hayvan_ismi.length == 2 } # => false

# C ile başlayan hayvan ismi olmasın! true. Hiçbir isim C ile başlamıyor
hayvanlar.none?{ |hayvan_ismi| hayvan_ismi.start_with?("C") } # => true
```

**inject**, **reduce***

`inject` ve `reduce` aynı işi yaparlar ve bir tür akümülator işlemi yapmaya yararlar. Blok içinde 2 paramtre kullanılır. Başlama parametresi de alabilir. Örneğin Array `[1, 2, 3, 4, 5]` ve tüm elemanları birbiriyle toplamak isitiyoruz.

```ruby
[1, 2, 3, 4, 5].inject{ |toplam, eleman| toplam + eleman } # => 15

# işlem şu şekilde ilerliyor
# toplam: 0, eleman: 1
# toplam: 1, eleman: 2
# toplam: 3, eleman: 3
# toplam: 6, eleman: 4
# toplam: 10, eleman: 5
# sona geldiğinde toplam 10, eleman 5 -> 10 + 5 = 15
```

Eğer başlangıç değeri için parametre geçseydik, örneğin **10**:

```ruby
[1, 2, 3, 4, 5].inject(10){ |toplam, eleman| toplam + eleman } # => 25

# toplam: 10, eleman: 1
# toplam: 11, eleman: 2
# toplam: 13, eleman: 3
# toplam: 16, eleman: 4
# toplam: 20, eleman: 5
# sona geldiğinde toplam 20, eleman 5 -> 20 + 5 = 25
```

Aynı işi `reduce` ile de yapabilirdik.

```ruby
[1, 2, 3, 4, 5].reduce(:+) # => 15
```

Örnekte her elemanın `+` methodu'nu çağırıyoruz ve sanki `x = x + 1` mantığında, kendisini ekleye ekleye sonuca varıyoruz.

```ruby
en_uzun_hayvan_ismi = ["kedi", "köpek", "kamplumbağa"].inject do |buffer, hayvan|
   buffer.length > hayvan.length ? buffer : hayvan
end

en_uzun_hayvan_ismi # => "kamplumbağa"
```

**partition** ve **group_by**

`partition` Array'i 2 parçaya ayırmaya yarar. Sonuç, blok'ta işlenen ifadeye bağlı olarak `[true_array, false_array]` olarak döner. Yani koşula `true` cevap verenlerle `false` cevap verenler ayrı parçalar halinde döner :)

```ruby
[1, 2, 3, 4, 5, 6].partition{ |n| n.even? } # => [[2, 4, 6], [1, 3, 5]]

# Çift sayılar, true_array yani ilk parça:    [2, 4, 6]
# Tek sayılar, false_array yani ikinci parça: [1, 3, 5]

# Sadece çift sayılar gelsin:
[1, 2, 3, 4, 5, 6].partition{ |n| n.even? }[0] # => [2, 4, 6]
```

`group_by` gruplama yapmak için kullanılır. Sonuç **Hash** döner, ilk değer (_key_) blok içindeki ifadenin sonucu, ikinci değer (_value_) ise sonucu verenlerin oluşturdu gruptur. 1'den 6'ya kadar sayıları 3'e bölünce kaç kaldığını gruplayarak bulalım:

```ruby
[1, 2, 3, 4, 5, 6].group_by{ |n| n % 3 } # => {1=>[1, 4], 2=>[2, 5], 0=>[3, 6]}

# 3'e bölünce kalanı;
# 1 olanlar: [1, 4]
# 2 olanlar: [2, 5]
# 0 olanlar (tam bölünenler) : [3, 6]
```

Notu 50'den büyük olanlar:

```ruby
notlar = [50, 20, 44, 60, 80, 100, 99, 81, 5]
notlar.group_by{ |notu| notu > 40 }[true] # => [50, 44, 60, 80, 100, 99, 81]
```

**chunk**

Array elemanları koşula göre gruplar.

```ruby
[3, 1, 4, 1, 5, 9, 2, 6, 5, 3, 5].chunk { |n| n.even? }.to_a

# => [
#  [false, [3, 1]],
#  [true, [4]],
#  [false, [1, 5, 9]],
#  [true, [2, 6]],
#  [false, [5, 3, 5]]
# ]
```

**slice_before**

Array içinde belli bir elemana ya da kurala göre parçalara ayırmak için kullanılır.

```ruby
[1, 2, 3, 'a' , 4, 5, 6, 'a' , 7 , 8 , 9 , 'a' , 1 , 3 , 5].slice_before {|i| i == 'a'}.to_a
# => [[1, 2, 3], ["a", 4, 5, 6], ["a", 7, 8, 9], ["a", 1, 3, 5]]
```

**flat_map**, **collect_concat**

İkisi de aynı işi yapar.

Önce `map` eder sonra `flatten` yapar.

```ruby
pos_neg = [1, 2, 3, 4, 5, 6].map { |n| [n, -n] }
pos_neg         # => [[1, -1], [2, -2], [3, -3], [4, -4], [5, -5], [6, -6]]
pos_neg.flatten # => [1, -1, 2, -2, 3, -3, 4, -4, 5, -5, 6, -6]

# yerine:
[1, 2, 3, 4, 5, 6].flat_map { |n| [n, -n] } # => [1, -1, 2, -2, 3, -3, 4, -4, 5, -5, 6, -6]
```

**sort_by**

Aynı `sort` gibi çalışır, Blok kullanır. İfadenin `true` olmasına göre çalışır:

```ruby
hayvanlar = ["kamplumbağa", "at", "eşşek", "kurbağa", "ayı"]

# isimleri uzunluklarına göre küçükten büyüğe doğru sıralayalım
hayvanlar.sort_by{ |isim| isim.length } # => ["at", "ayı", "eşşek", "kurbağa", "kamplumbağa"]

# isimleri uzunluklarına göre büyükten küçüğe doğru sıralayalım
hayvanlar.sort_by{ |isim| -isim.length } # => ["at", "ayı", "eşşek", "kurbağa", "kamplumbağa"]
```

**bsearch**

**Binary** arama yapar, `O(log n)` formülünü uygular, buradaki `n` Array'in boyudur. **Find minimum** gibidir, yani koşula ilk uyanı bul gibi...

```ruby
# 2'den büyük 3, 4 ve 5 olmasına rağmen tek sonuç
[1, 2, 3, 4, 5].bsearch{ |n| n > 2 }  # => 3

[1, 2, 3, 4, 5].bsearch{ |n| n >= 4 } # => 4
```

### Tehlikeli İşlemler

Başlarda da bahsettiğimiz gibi method ismi `!` ile bitiyorsa bu ilgili nesnede değişiklik yapıyor olduğumuz anlamına gelir. Array'lerde de bu tür method'lar var:

    [:reverse!, :rotate!, :sort!, :sort_by!, :collect!, :map!, :select!, :reject!, :slice!, :uniq!, :compact!, :flatten!, :shuffle!]

Bu method'lar orijinal Array'i bozar. Yani;

```ruby
a = [1, 2, 3, 4, 5]
a.reverse! # => [5, 4, 3, 2, 1]

# a artık reverse edilmiş halde!
a          # => [5, 4, 3, 2, 1]
```

## Hash

Pek çok dilde **Dictionary** olarak geçen, Array'imsi, hatta **Associative Array** de denir, **Key**-**Value** çifti barındıran yine Array'e benzeyen başka bir taşıyıcıdır. **Key**-**Value** dediğimiz şey ise;

```ruby
{key1: "value1", key2: "value2", ....}
```

şeklindedir. Yukarıdaki örnek, yeni syntax'ı kullanır. Ruby programcılarının alışık olduğu örnek:

```ruby
{"key1" => "value1", "key2" => "value2", ...}
```

ya da

```ruby
{:key1 => "value1", :key2 => "value2", ...}
```

şeklindedir. Hepsi aynı kapıya çıkar... Array'deki sıra (_index_) mantığı burada **key**'ler ile oluyor gibi düşünebilirsiniz. Key'ler **unique**'dir yani bir Hash içinde **2 tane aynı key**'den olamaz.

Hash'de sonuçta bir class olduğu için `new` method'u ile Hash'i oluşturabiliriz;

```ruby
Hash.new # => {}
```

Aynı Array'deki gibi Hash'in de hızlı oluşurma yolu var : `h = {}`. Hemen Hash'in nereden geldiğine bakalım:

```ruby
Hash.class                                  # => Class
Hash.class.superclass                       # => Module
Hash.class.superclass.superclass            # => Object
Hash.class.superclass.superclass.superclass # => BasicObject
Hash.class.superclass.superclass.superclass.superclass # => nil
```

Dikkat ettiyseniz Hash'in bir üst sınıfı **Module**. Aynı Array'deki gibi. Peki bu modüller nelermiş?

```ruby
Hash.included_modules # => [Enumerable, Kernel]
```

Eğer Hash'i oluştururken **default** değer geçersek, tanımsız olan **key** için değer atamış oluruz:

```ruby
h = Hash.new("Tanımsız") # => {}
h[:isim] = "Uğur"        # => "Uğur"
h                        # => {:isim=>"Uğur"}
h[:soyad]                # => "Tanımsız"
h.default                # => "Tanımsız"
```

Olmayan bir key'e ulaşmak istediğimizde `"Tanımsız"` değeri geldi. Eğer bu default değeri atamasaydı ne olacaktı?

```ruby
h = Hash.new               # => {}
h[:isim] = "Uğur"          # => "Uğur"
h[:soyad]                  # => nil
```

`nil` gelecekti. Biraz sonra göreceğimiz `fetch` method'unu lütfen aklınızda tutun!

**Default** değer tanımlama mantığında;

```ruby
h = Hash.new { |hash, key| hash[key] = "User: #{key}" }
h["vigo"]             # => "User: vigo"
h["foobar"]           # => "User: foobar"
h["animal"] = "horse" # => "horse"
h                     # => {"vigo"=>"User: vigo", "foobar"=>"User: foobar", "animal"=>"horse"}
```

bu tarz ilginç bir yöntem de kullanılabilir. Normalde `vigo` key'ine karşılık **value** yok ama `Hash`in `new` method'unda yaptığımız bir blok işlemi ile olmayan `key` için değer ataması yaptığımız gibi key-value ataması da yapabiliyoruz.

### Hash Class Method'ları

Hash'den bir instance oluşturmadan kullandığımız methodlardır.

**Hash[ key, value, ... ] -> yeni_hash**
**Hash[ [ [key, value], ... ] ] -> yeni_hash**
**Hash[ object ] -> yeni_hash**

```ruby
Hash["user_count", 5]                            # => {"user_count"=>5}
Hash[ [["user_count", 5], ["active_users", 2]] ] # => {"user_count"=>5, "active_users"=>2}
Hash["user_count" => 5, "active_users" => 2]     # => {"user_count"=>5, "active_users"=>2}
```

**Hash.new**

Zaten ilgili örnekleri başta vermiştik, tekrar edelim:

```ruby
h = Hash.new
h # => {}
h["user_count"] = 5
h # => {"user_count"=>5}

h = Hash.new { |hash, key| hash[key] = "User ID: #{key}" }
h["1"] # => "User ID: 1"
h["2"] # => "User ID: 2"
h # => {"1"=>"User ID: 1", "2"=>"User ID: 2"}
```

**try_convert(obj) → hash ya da nil**

Hash'e dönüşebilme ihtimali olan nesneyi Hash haline çevirir.

```ruby
Hash.try_convert({"user_count"=>5})   # => {"user_count"=>5}
Hash.try_convert("user_count=>5")     # => nil
```

### Hash Instance Method'ları

Hash instance'ı oluşturduktan sonra kullanacağımız method'lardır.

Önce klasik değer okuma ve değer atama işlerine bakalım. Zaten bu noktaya kadar kabaca biliyoruz nasıl değer atarız geri okuruz. Ama biraz kafaları karıştırmak istiyorum:

```ruby
h = {username: "vigo", password: "1234"} # => {:username=>"vigo", :password=>"1234"}
```

Yukarıdaki gibi bir **Hash**'imiz var. Dikkat ettiyseniz, key,value olarak baktığımızda `:username` ve `:password` diye başlayan `key`ler var... Hatta:

```ruby
h.keys # => [:username, :password]
```

diye de sağlamasını yaparız. Peki, yeni bir `key` tanımlasak? `h["useremail"] = "vigo@example.com"`. Tekrar bakalım `key`lere:

```ruby
h.keys # => [:username, :password, "useremail"]
```

Bir sonraki bölümde karşımıza çıkacak olan **Symbol** tipi ile karşı karşıyayız. Sadece **symbol** mu? hayır, karışık keyler var elimizde. Hemen sağlamasını yapalım:

```ruby
h.keys.map(&:class) # => [Symbol, Symbol, String]
```

İlk iki key **Symbol** iken son key **String** oldu. Demek ki Hash içine key cinsi olarak karşık atama yapabiliniyor. Biraz sıkıntılı bir durum ama genel kültür mahiyetinde aklınızda tutun bunu!

Şimdi genel olarak Hash hangi method'lara sahip hemen bakalım:

```ruby
h = Hash.new
h.methods # => [:rehash, :to_hash, :to_h, :to_a, :inspect, :to_s, :==, :[], :hash, :eql?, :fetch, :[]=, :store, :default, :default=, :default_proc, :default_proc=, :key, :index, :size, :length, :empty?, :each_value, :each_key, :each_pair, :each, :keys, :values, :values_at, :shift, :delete, :delete_if, :keep_if, :select, :select!, :reject, :reject!, :clear, :invert, :update, :replace, :merge!, :merge, :assoc, :rassoc, :flatten, :include?, :member?, :has_key?, :has_value?, :key?, :value?, :compare_by_identity, :compare_by_identity?, :entries, :sort, :sort_by, :grep, :count, :find, :detect, :find_index, :find_all, :collect, :map, :flat_map, :collect_concat, :inject, :reduce, :partition, :group_by, :first, :all?, :any?, :one?, :none?, :min, :max, :minmax, :min_by, :max_by, :minmax_by, :each_with_index, :reverse_each, :each_entry, :each_slice, :each_cons, :each_with_object, :zip, :take, :take_while, :drop, :drop_while, :cycle, :chunk, :slice_before, :lazy, :nil?, :===, :=~, :!~, :<=>, :class, :singleton_class, :clone, :dup, :taint, :tainted?, :untaint, :untrust, :untrusted?, :trust, :freeze, :frozen?, :methods, :singleton_methods, :protected_methods, :private_methods, :public_methods, :instance_variables, :instance_variable_get, :instance_variable_set, :instance_variable_defined?, :remove_instance_variable, :instance_of?, :kind_of?, :is_a?, :tap, :send, :public_send, :respond_to?, :extend, :display, :method, :public_method, :singleton_method, :define_singleton_method, :object_id, :to_enum, :enum_for, :equal?, :!, :!=, :instance_eval, :instance_exec, :__send__, :__id__]
```

Dikkat ettiyseniz method'ların bir kısmı **Array** ile aynı çünki ikisi de **Enumerable** modülünü kullanıyor.

Şimdi sıradan başlayalım! Önceki **Array** bölümünde anlattığım ortak method'ları pas geçeceğim!

**rehash**

Hash'e key olarak Array verebiliriz. Yani `h[key] = value` mantığında `key` olarak bildiğiniz Array geçebiliriz.

```ruby
a = [ "a", "b" ]
c = [ "c", "d" ]
h = { a => 100, c => 300 } # => {["a", "b"]=>100, ["c", "d"]=>300}
```

`h` Hash'inin keyleri nedir?

```ruby
h.keys                     # => [["a", "b"], ["c", "d"]]
```

2 key'i var biri `["a", "b"]` ve diğeri `["c", "d"]` nasıl yani?

```ruby
h[a]                       # => 100
h[["a", "b"]]              # => 100
h[c]                       # => 300
h[["c", "d"]]              # => 300
```

Şimdi işleri karıştıralım. `a` Array'inin ilk değerini değiştirelim. Bakalım `h` ne olacak?

```ruby
a[0] = "v"                 # => "v"
a                          # => ["v", "b"]
h[a]                       # => nil ????????
```

`h[a]` patladı? `nil` döndü. İşte şimdi imdadımıza ne yetişecek?

```ruby
h.rehash                   # => {["v", "b"]=>100, ["c", "d"]=>300}
h[a]                       # => 100
```

**to_hash**, **to_h**, **to_a**, **to_s**

Tip dönüştürmeleri için kullanılırlar. `to_h` ve `to_hash` eğer kendisi Hash ise sonuç yine kendisi olur. `to_a` ise Hash'den Array yapmak için kullanılır. Tahmin edeceğiniz gibi `to_s` de String'e çevirmek için kullanılır.

```ruby
h = {:foo => "bar"}
h                                 # => {:foo=>"bar"}
h.to_hash                         # => {:foo=>"bar"}
h.to_h                            # => {:foo=>"bar"}
["foo", "bar"].respond_to?(:to_h) # => true
[[:foo, "bar"]].to_h              # => {:foo=>"bar"}

[["a", 1], ["b", 2]].to_h   # => {"a"=>1, "b"=>2}

h.to_a                            # => [[:foo, "bar"]]
h.to_s                            # => "{:foo=>\"bar\"}"
```

**==** ve **eql?** Eşitlik

Hash içinde key'lerin sırası eşitlik kontrolünde önemli değildir. İçerik önemlidir. Eşitlik kontrolü için kullanılırlar.

```ruby
h1 = { "a" => 100, "c" => 200 }
h2 = { 70 => 350, "x" => 22, "y" => 11 }
h3 = { "y" => 11, "x" => 22, 70 => 350 }

h1 == h2 # => false
h2 == h3 # => true

h1.eql?(h2)  # => false
h2.eql?(h3)  # => true
```

`h2` ile `h3` key sıraları farklı olmasına rağmen içerik bazında eşittirler.

**fetch**

Hash içinden sorgu yaparken kullanılır. Eğer olmayan key çağırırsanız **exception** oluşur. Bu method güvenli bir yöntemdir. Aksi takdirde `nil` döner ve kompleks işlerde **Silent Fail** yani dipsiz kuyuya düşer bir türlü hatanın yerini bulamazsınız!

```ruby
h = {:user => "vigo", :password => "secret"}
puts h.fetch(:user) # "vigo"
puts h.fetch(:email)

KeyError: key not found: :email
```

Keza eğer key'e karşılık yoksa **default** değer ataması yapabilirsiniz:

```ruby
h = {:user => "vigo", :password => "secret"}
h.fetch(:user)               # => "vigo"
h.fetch(:email, "Not found") # => "Not found"
```

**Block** kabul ettiği için artistlik hareketler yapmak da mümkün :)

```ruby
h = {:user => "vigo", :password => "secret"}
h.fetch(:email) { |element| "key: #{element} is not defined!" } # => "key: email is not defined!"
```

**store**

Atama yapmanın farklı bir yöntemidir.

```ruby
h = {:user => "vigo", :password => "secret"}
h.store(:email, "vigo@example.com") # => "vigo@example.com"
h                                   # => {:user=>"vigo", :password=>"secret", :email=>"vigo@example.com"}

# ya da

h[:url] = "http://webbox.io"        # => "http://webbox.io"
h                                   # => {:user=>"vigo", :password=>"secret", :email=>"vigo@example.com", :url=>"http://webbox.io"}
```

**default**, **default=**

Karşılığı olmayan `key`ler için varsayılan değer ataması yapılmışsa bunu bulmak için ya da varsayılan değeri atamak için kullanılır. En başta benzer işler yaptık:

```ruby
h = Hash.new(10)
h[:user_age]            # => 10
h                       # => {}
h.default               # => 10
h.default(:user_weight) # => 10
```

ya da

```ruby
h = Hash.new
h                         # => {}
h.default = 100           # => 100
h[:user_weight]           # => 100
h[:foo]                   # => 100
```

**key**

Value'den key'i bulmak için kullanılır. Eğer key'i olmayan bir value kullanırsanız sonuç `nil` döner!

```ruby
h = {:user => "vigo", :password => "secret"}
h.key("vigo")   # => :user
h.key("foobar") # => nil
```

**size**, **length**, **count**

Aynı işi yaparlar, Arrar gibi Hash'in boyunu / uzunluğunu verir.

```ruby
h = {:user => "vigo", :password => "secret"}
h.length # => 2
h.size   # => 2
h.count  # => 2
```

### Key, Value Kontrolleri

**keys**, **values**, **values_at**

Tahmin edeceğiniz gibi `keys` ile Hash'e ait key'leri, `values` ile sadece key'lere karşılık gelen değerleri, `values_at` ile verdiğimiz key'lere ait değerleri alırız.

```ruby
h = {:user => "vigo", :password => "secret", :email => "vigo@foo.com"}
h.keys                        # => [:user, :password, :email]
h.values                      # => ["vigo", "secret", "vigo@foo.com"]
h.values_at(:user, :password) # => ["vigo", "secret"]
```

**key?**, **value?**, **has_key?**, **has_value?**

Soru işareti ile biten method'lar bize her zaman **Boolean** yani `true` ya da `false` döner demiştik. Acaba Hash'in içinde ilgili key var mı? ya da value var mı?

```ruby
h = {:user => "vigo", :password => "secret", :email => "vigo@foo.com"}
h.key?(:user)          # => true
h.has_key?(:user)      # => true

h.key?(:full_name)     # => false
h.has_key?(:full_name) # => false

h.value?("vigo")       # => true
h.has_value?("vigo")   # => true

h.value?("lego")       # => false
h.has_value?("lego")   # => false
```

**include?**, **member?**

`key?` ya da `has_key?` ile aynı işi yapar.

```ruby
h = {:user => "vigo", :password => "secret", :email => "vigo@foo.com"}
h.include?(:user)        # => true
h.member?(:user)         # => true
```

**empty?**

Hash'in içinde eleman var mı yok mu?

```ruby
{:user => "vigo", :password => "secret", :email => "vigo@foo.com"}.empty? # => false
{}.empty? # => true
```

**all?**, **any?**, **one?**, **none?**

**Array** bölümünde görmüştük, **Enumerable** modülünden gelen bu özellik aynen Hash'de de kullanılıyor. `all?` da tüm elemanlar, verilen koşuldan `nil` ya da `false` dışında bir şey dönmek zorunda, aksi halde sonuç `false` oluyor:

```ruby
# value'su boş olan var mı?
{:user => "vigo", :password => "secret", :email => "vigo@foo.com"}.all?{ |k,v| v.empty? } # => false
{:user => "", :password => "", :email => ""}.all?{ |k,v| v.empty? }                       # => true
{:user => "vigo", :password => "", :email => ""}.all?{ |k,v| v.empty? }                   # => false
```

`any?` de içlerinden biri `false` ya da `nil` dönmezse sonuç `true`olur. `one?` da sadece bir tanesi `true` dönmelidir. `none` da ise block'daki işlem sonucu her eleman için `false` olmalıdır.

```ruby
{:is_admin => true, :notifications_enabled => true}.all?{ |option, value| value } # => true
{:is_admin => true, :notifications_enabled => false}.any?{ |option, value| value } # => true
{:is_admin => true, :notifications_enabled => false}.one?{ |option, value| value } # => true
{:is_admin => false, :notifications_enabled => false}.one?{ |option, value| value } # => false
{:is_admin => false, :notifications_enabled => false}.all?{ |option, value| value } # => false
{:is_admin => false, :notifications_enabled => false}.none?{ |option, value| value } # => true
{:is_admin => false, :notifications_enabled => false}.any?{ |option, value| value } # => false
```

**shift**

Hash'den key-value çiftini silmek için kullanılır. Her seferinde ilk key-value çiftini siler.

```ruby
h = {:user => "vigo", :password => "secret", :email => "vigo@foo.com"}
h.shift # => [:user, "vigo"]
h       # => {:password=>"secret", :email=>"vigo@foo.com"}
h.shift # => [:password, "secret"]
h       # => {:email=>"vigo@foo.com"}
h.shift # => [:email, "vigo@foo.com"]
h       # => {}
```

**delete**, **delete_if**, **keep_if**

Hash'den key kullanarak eleman silmek için `delete` method'u kullanılır.

```ruby
h = {:user => "vigo", :password => "secret", :email => "vigo@foo.com"}
h.delete(:user) # => "vigo"
h               # => {:password=>"secret", :email=>"vigo@foo.com"}
```

Block kullanıldığında, eğer olmayan bir key kullanılmışsa, bununla ilgili işlem yapmamızı sağlar:

```ruby
h.delete(:phone){ |key| "-#{key}- bulunamadı?" } # => "-phone- bulunamadı?"
```

`delete_if` de ise direk block kullanarak koşullu silme işlemi yapabiliyoruz.

```ruby
# 40'dan büyükleri silelim
h = { point_a: 10, point_b: 20, point_c: 50 } # => {:point_a=>10, :point_b=>20, :point_c=>50}
h.delete_if{ |k,v| v > 40 }                   # => {:point_a=>10, :point_b=>20}
h                                             # => {:point_a=>10, :point_b=>20}
```

`keep_if` ise `delete_if` in tam tersi gibidir. Eğer block'daki koşul `true` ise key-value çiftini tutar, aksi halde siler:

```ruby
# 20'dan küçükleri tutalım sadece
h = { point_a: 10, point_b: 20, point_c: 50 }
                         # => {:point_a=>10, :point_b=>20, :point_c=>50}
h.keep_if{ |k,v| v < 20} # => {:point_a=>10}
h                        # => {:point_a=>10}
```

**invert**

Hash'in key'leri ile value'lerini yer değiştirmek için kullanılır.

```ruby
h = { "a" => 100, "b" => 200 } # => {"a"=>100, "b"=>200}
h.keys                         # => ["a", "b"]
h.invert                       # => {100=>"a", 200=>"b"}
```

**merge**, **update**, **merge!**

İki Hash'i birbiryle birleştirmek için `merge` kullanılır.

```ruby
h1 = { "a" => 100, "b" => 200 }
h2 = { "x" => 1, "y" => 2, "z" => 3 }
h1.merge(h2) # => {"a"=>100, "b"=>200, "x"=>1, "y"=>2, "z"=>3}
h1           # => {"a"=>100, "b"=>200}
```

Dikkat ettiyseniz `h1` ile `h2` yi birleştirdik ama `h1`in orijinal değerini bozmadık. Eğer bu birleşmenin kalıcı olmasını isteseydik ya `update` ya da `merge!` kullanmamız gerekecekti!

```ruby
h1 = { "a" => 100, "b" => 200 }
h2 = { "x" => 1, "y" => 2, "z" => 3 }
h1.update(h2) # => {"a"=>100, "b"=>200, "x"=>1, "y"=>2, "z"=>3}
h1            # => {"a"=>100, "b"=>200, "x"=>1, "y"=>2, "z"=>3}

h1 = { "a" => 100, "b" => 200 }
h2 = { "x" => 1, "y" => 2, "z" => 3 }
h1.merge!(h2) # => {"a"=>100, "b"=>200, "x"=>1, "y"=>2, "z"=>3}
h1            # => {"a"=>100, "b"=>200, "x"=>1, "y"=>2, "z"=>3}
```

**replace**

Hash'in içeriğini başka bir Hash ile değiştirmek için kullanılır. Aslında varolan Hash'i başka bir Hash'e çevirmek gibidir. Neden `replace` kullanılıyor? Tamamen hafızadaki adresleme ile ilgili. `replace` kullanıldığı zaman, aynı Hash kullanılıyor, yeni bir Hash instance'ı yaratılmıyor.

```ruby
h1 = { "a" => 100, "b" => 200, "c" => 0 }
h1.__id__ # => 70320602334320
```

Hafızadaki `h1` Hash'nin nesne referansı : 70320602334320. Şimdir `replace` ile değerlerini değiştirelim:

```ruby
h1.replace({ "foo" => 1, "bar" => 2 })
h1        # => {"foo"=>1, "bar"=>2}
h1.__id__ # => 70320602334320
```

Referansları aynı : **70320602334320**. Eğer direkt olarak atama yapsakdık `h1` gibi görünen ama bambaşka yepyeni bir Hash'imiz olacaktı.

```ruby
h1 = { "foo" => 1, "bar" => 2 }
h1.__id__ # => 70216424232360
```

### İterasyon ve Block Kullanımı

Aynı Array'lerdeki gibi Hash'lerde de iterasyon ve block kullanmak mümkün.

**each**, **each_pair**, **each_value**, **each_key**

`each` ve `each_pair` kardeş gibidirler:

```ruby
h = { "a" => 100, "b" => 200, "c" => 0 }
h.each { |key, value| puts "key: #{key}, value: #{value}" }
h.each_pair { |key, value| puts "key: #{key}, value: #{value}" }

# key: a, value: 100
# key: b, value: 200
# key: c, value: 0
```

`each_value` sadece **value**, `each_key` de sadece **key** döner.

```ruby
h = { "a" => 100, "b" => 200, "c" => 0 }

h.each_value { |value| puts "value: #{value}" }

# value: 100
# value: 200
# value: 0

h.each_key { |key| puts "key: #{key}" }

# key: a
# key: b
# key: c
```

**each_entry**, **each_slice**, **each_cons**

Hash'deki key-value çifti Array şeklinde bir **entry** olur:

```ruby
h = { "a" => 100, "b" => 200, "c" => 0 }
h.each_entry{ |o| puts "o: #{o}" }

# o: ["a", 100]
# o: ["b", 200]
# o: ["c", 0]
```

`each_slice` ile **entry**'leri parçacıklara ayırırız:

```ruby
h = { "a" => 100, "b" => 200, "c" => 0 }

# 2'li dilimlere ayırdık
h.each_slice(2){ |s| puts "slice: #{s}" }

# slice: [["a", 100], ["b", 200]]
# slice: [["c", 0]]
```

`each_cons` ise `each_slice` gibi çalışır ama farkı örnekteki gibidir:

```ruby
h = { "a" => 100, "b" => 200, "c" => 0 }
h.each_cons(2){ |s| puts "grup: #{s}" }

# grup: [["a", 100], ["b", 200]]
# grup: [["b", 200], ["c", 0]]
```

Neticede, 3 key-value çifti vardı. **2**'li grupladık ama sonuç `each_slice` daki gibi dönmedi. `["b", 200]` tekrar etti, çıktı gruplaması mutlaka 2 eleman içerdi.

**default_proc**, **default_proc=**

Konunun başında varsayılan değer ataması yaparken şöyle bir örnek vermiştik :

```ruby
h = Hash.new { |hash, key| hash[key] = "User: #{key}" }
```

eğer;

```ruby
h.default_proc # => #<Proc:0x007f85f2250fd8@-:7>
```

deseydik, bu Hash'e ait **Proc** u görmüş olurduk. Yani bu Hash için varsayılan işlem prosedürünü tanımlamış olduk aslında. Örneği biraz genişletelim:

```ruby
h = Hash.new {|obj, key| obj[key] = key * 4 } # => {}
h[1] # => 4
h[2] # => 8
h    # => {1=>4, 2=>8}
```

Key olarak sayı veriyoruz, gelen sayıdan da value üretiyoruz otomatik olarak. İşlemin çalışması için bir adet obje ve sayı geçmemiz gerekiyor parametre olarak. Aslında;

```ruby
h.default_proc.call(Array.new, 9) # => 36
h.default_proc.call([], 9) # => 36
h.default_proc.call({}, 9) # => 36
```

şeklinde de, Hash'i sanki bir fonksiyon gibi kullanıp işleyebiliyoruz.

Daha sonra, önceden tanımladığımız bu prosedürü değiştirmek istersek `default_proc=` methodunu kullanıyoruz:

```ruby
h = Hash.new { |hash, key| hash[key] = "User: #{key}" }
h.default_proc # => #<Proc:0x007feea39bbd80@-:7>
h[1] # => "User: 1"

# Yeni prosedür veriyoruz
h.default_proc = proc do |hash, key|
  hash[key] = "hello #{key}"
end
h # => {1=>"User: 1"}
h[2] # => "hello 2"
h # => {1=>"User: 1", 2=>"hello 2"}
```

**compare_by_identity**, **compare_by_identity?**

Hash'in key ve value'leri birbirine benziyor mu?

```ruby
h = { "a" => 1, "b" => 2, :c => "c" }
h["a"] # => 1
h.compare_by_identity? # => false
h.compare_by_identity  # => {"a"=>1, "b"=>2, :c=>"c"}
h.compare_by_identity? # => true

# acaba key ile value benziyormu?
h["a"]                 # => nil

# burada benzer :)
h[:c]                  # => "c"
```

## Symbol

Symbol (*sembol*) Ruby'e ait özel bir nesnedir. Bir tür **placeholder** (*yer tutucu*) görevindedir. `:` işareti ile başlayan her şey semboldür.

Sembolün, değişkenden en önemli farkı tekil olmasıdır. Yani sembole atanan değişkenden hafızada 1 adet bulunur.

```ruby
user = "vigo"
user.object_id # => 70122132113780

# şimdi başka değer atayalım
user = "bronx"
user.object_id # => 70122132113340
# object_id değişti!
```

Eğer Symbol kullansaydık:

```ruby
user = :vigo
user.object_id # => 420488

user = :bronx
user.object_id # => 420648
```

`user` değişkeninin değeri **Symbol** cinsinden olduğu için, artık hafızada sabit bir yer ayrılmış oldu bu iş için. Değer değişse bile hafızadaki adreslendiği alan değişmemiş oluyor :)

String olarak atanmış değişkeni de Symbol'e çevirmek mümkün:

```ruby
full_name = "Uğur"         # => "Uğur"
full_name.to_sym           # => :Uğur
full_name == :Uğur.id2name # => true
user_full_name = :Uğur     # => :Uğur
user_full_name.object_id   # => 420428

:is_user_admin.id2name     # => "is_user_admin"
:is_user_admin.to_s        # => "is_user_admin"
```

Symbol'ler, değişkenler gibi direkt atama yöntemiyle yani `:a = 1` gibi bir şekilde çalışmazlar. Eğer bir String'den Symbol üretmek isterseniz `to_sym` methodunu kullanmanız gerekiyor.

Hafızayı idareli kullanmak, boşu boşuna değişken kirliği yaratmamak gibi konularda tercih edilir. Keza Hash'lerde de **KEY** ataması Symbol olarak yapılıyor bu tür hız / tasarruf işleri için.

```ruby
{:user=>"vigo"}
```

## Class (Sınıf)

Ruby, **Object-Oriented** (*OO*) bir dil olduğu için, methodları değişkenleri ve benzeri şeyleri içinde barından bir tür taşıyıcıya ihtiyaç duyar. İşte bu taşıyıcıya **Class** ya da sınıf diyoruz. Aslında ben **tür** demeyi tercih ediyorum sınıf yerine.

Zaten önceki konularda **Class Methods**, **Instance Methods** gibi kavramlara girmiştik.

Class'lar birbirinden türeyebilir (*Hani* `class.superclass` *şeklinde analizler yapmıştık*)

Teknik olarak bir dosyada birden fazla Class tanımlaması yapılabilir. Örneğin, `my_class.rb` adlı bir dosya içinde farklı farklı Class tanımlamaları olabilir;

```ruby
class MyClass
end

class OtherClass
end

a = MyClass.new    # => #<MyClass:0x007ffa2b09b758>
b = OtherClass.new # => #<OtherClass:0x007ffa2b09b3c0>
```

Class tek başına bir nesne (*Obje*) bu bakımdan **instantiate** etmeseniz bile (*Yani* `a = Array.new` *gibi*) kullanabileceğiniz bir şeydir.

Class'ların diğer bir özelliği de açık olmasıdır. Ruby'nin bence en harika özelliği, **built-in** yani dilin çekirdeğinden gelen Class'lara bile method / property eklemeniz mümkündür. Bu konuları **Monkey Patching** kısmında detaylı göreceğiz.

En basit tanımıyla Class aşağıdaki gibidir:

```ruby
class Merhaba
  def initialize(isim)
    @isim = isim
  end

  def selam_sana
    "Selam sana, #{@isim}"
  end
end

hey = Merhaba.new "Uğur"
hey.selam_sana # => "Selam sana, Uğur"
```

`initialize` methodu, Class'dan ilk örnek türedildiğinde (*yani bir instance oluşturulduğunda*) tetiklenir ve bir tür Class ile ilgili ön tanımlamaların yapıldığı alan konumundadır. Benzer dillerdeki **class constructor**'ı gibi düşünülebilir.

`@isim` ise **Instance Variable** yani Class'tan türeyen nesneye ait değişkendir.

`selam_sana` ise bu Class'ın bir method'udur. `hey` değişkeni, `Merhaba` Class'ından türemiş bir **Instance**'dır. `Merhaba` sınıfındaki tüm method'lar **inherit** yani miras olarak `hey` nesnesine geçmiştir.

Class'ı tanımlarken ister klasik ister block yöntemini kullanabilirsiniz. Klasik yöntem:

```ruby
class Person
end

jack = Person.new # => #<Person:0x007fb0521a4820>
```

Block ise;

```ruby
Person = Class.new do
end

jack = Person.new # => #<Person:0x007fc42c0c8648>
```

şeklindedir. Class isimleri **Büyük** harfle başlar.

### Public Instance Method'ları

Bir Class'tan türeyen şeye **Instance** diyoruz. Ruby'de Class'lar **first-class objects** olarak geçer yani birinci sınıf nesnelerdir. Bu da şu anlama gelir, aslında her Class, Kernel'dan gelen `Class` nesnesinden türemiş alt sınıftır :)

**allocate**, **new** ve **superclass**

`superclass` ilgili Class'ın kimden geldiğini / türediğini gösterir. Benzer örnekleri kitabın başında yapmıştık:

```ruby
String.superclass      # => Object
Object.superclass      # => BasicObject
BasicObject.superclass # => nil
```

`new` ise önce `allocate` method'unu çağırıp hafızada gereken yeri ayırır, yani **instantiate** edeceğimiz Class'ın sınıfını organize eder, sonra oluşacak Class'ın `initialize` method'unu çağırıp varsa ilgili argümanları pas eder. Her `.new` çağırıldığında bu iş olur.

### Private Instance Method'ları

**inherited(subclass)**

İlgili sınıfın alt sınıfı oluşturulduğunda tetiklenir.

```ruby
class Animal
  def self.inherited(subclass)
    puts "Yeni subclass: #{subclass}"
  end
end

class Cat < Animal
end

class Tiger < Cat
end

# Yeni subclass: Cat
# Yeni subclass: Tiger
```

Animal (*hayvan*) sınıfından, Cat (*kedi*) ürettik, Tiger (*kaplan*)'ı da yine Cat (*kedi*)'den ürettik...

### Accessors (getter + setter)

Özel method'lar kullanarak **Meta Programming** mantığıyla Ruby, Instance Variable'ları yönetmeyi kolaylaştırır. Yukarıdaki `Merhaba` Class'ındaki `@isim` için aslında `get`ve `set` yani oku ve yaz method'ları tanımlamamız lazım ki ilgili değişken üzerinde işlem yapabilelim. Önce uzun yolu, sonra doğru ve kısa yolu görelim:

```ruby
class Person
  def name
    @name
  end
  
  def name=(name)
    @name = name
  end
end

vigo = Person.new  # => #<Person:0x007f903b8d0590>
vigo.name          # => nil
vigo.name = "Uğur" # => "Uğur"
vigo.name          # => "Uğur"
```

`name` method'unu çağırınca bize instance variable olan `@name` dönüyor. İlk anda **set** etmediğimiz yani değer atamadığımız için `nil` geliyor. Daha sonra `vigo.name = "Uğur"` diyerek atama yapıyoruz ve artık değerini belirlemiş oluyoruz. Bu iş için 2 tane method yazdık. `name` ve `name=` method'ları.

İşte bu noktada **accessors** imdadımıza yetişiyor:

```ruby
class Person
  attr_accessor :name
end

vigo = Person.new  # => #<Person:0x007fb7c9a4c620>
vigo.name          # => nil
vigo.name = "Uğur" # => "Uğur"
vigo.name          # => "Uğur"
```

`attr_accessor :name` dediğimizde, Ruby, bizim için `name` ve `name=` method'ları oluşturuyor. Keza sadece bununla kalmayıp, pek çok farklı kullanım imkanları sunuyor.

`attr` modülüyle:

* attr
* attr_accessor
* attr_reader
* attr_writer

gibi özel getter/setter'lar geliyor. Yukarıdaki örneği `attr` ile yapalım;

```ruby
class Person
  attr :name, true
end

Person.instance_methods - Object.instance_methods # => [:name, :name=]
```

Otomatik olarak 2 method ekledi : `[:name, :name=]`. Aynı şeyi `attr_accessor :name` ile de yapabilirdik:

```ruby
class Person
  attr_accessor :name
end

Person.instance_methods - Object.instance_methods # => [:name, :name=]
```

Eğer sadece `attr_reader` kullansaydık, sadece ilgili instance variable'ını okuyabilir ama değerini set edemezdik!

```ruby
class Person
  attr_reader :name
end

Person.instance_methods - Object.instance_methods # => [:name]

vigo = Person.new
vigo.name # => nil
vigo.name = "Uğur" # => NoMethodError: undefined method `name=' for #<Person:0x007ffe4d0e8528>
```

Gördüğünüz gibi `NoMethodError` hatası aldık çünki setter yani `name=` method'u oluşmadı! Peki sadece `attr_writer` olsaydı?

```ruby
class Person
  attr_writer :name
end

Person.instance_methods - Object.instance_methods # => [:name=]

vigo = Person.new
vigo.name = "Uğur" # => "Uğur"
vigo.name # => NoMethodError: undefined method ‘name’ for #<Person:0x007fb92b9d45b8 @name="Uğur">
```

Set edebiliyoruz ama get edemiyoruz! Peki `attr_writer` nerede işimize yarar? Örneğin sadece Class'ı initialize ederken değer pas edip sınıf içinde bir değişkene atama yapmak gerektiğinde kullanabilirsiniz:

```ruby
class Person
  attr_writer :name
  
  def initialize(name)
    @name = name
  end
  
  def greet
    "Hello #{@name}"
  end
end

vigo = Person.new "Uğur"
vigo.greet # => "Hello Uğur"
```

`@name` değişkenini sadece ilk tetiklenmede set ediceksem ve dışarıdan okuma ihtiyacım yoksa bu şekilde kullanabilirim!

### Class Variables

`@@` ile başlayan değişkenler Class Variable (*Sınıf Değişkeni*) olarak tanımlanır. Yani Ana Class'a ait bir değişkendir. Her yeni instance oluştuğunda bu değer ait olduğu üst sınıftan erişilebilir:

```ruby
class Person
  attr_accessor :name
  
  @@amount = 0
  def initialize(name)
    @@amount += 1
    @name = name
  end
  
  def greet
    "Hello #{name}"
  end
  
  def how_many_people_created
    "Number of people: #{@@amount}"
  end
end

user1 = Person.new "Uğur"
user2 = Person.new "Yeşim"
user3 = Person.new "Ezel"

Person.class_variable_get(:@@amount) # => 3
user3.how_many_people_created        # => "Number of people: 3"
```

### Class Methods

İlgili Class'dan türetme yapmadan, direk Class'dan çağırılan özel method'dur. Bu method'u çağırmak için sınıftan herhangi bir türetme yapmaya gerek olmaz, direkt olarak sınıf'tan çağırılır:

```ruby
class Person
  attr_accessor :name
  
  @@amount = 0
  def initialize(name)
    @@amount += 1
    @name = name
  end
  
  def greet
    "Hello #{name}"
  end
  
  def how_many_people_created
    "Number of people: #{@@amount}"
  end

  def self.how_many_people_created
    "We have #{@@amount} copie(s)"
  end
end

user1 = Person.new "Uğur"
user2 = Person.new "Yeşim"
user3 = Person.new "Ezel"

Person.how_many_people_created # => "We have 3 copie(s)"
```

`Person.how_many_people_created` direkt olarak çağırılır!

### Singletons

Sınıf içinde `class` komutunu kullanarak method oluşturmak içindir. Buna **Singleton** denir. Sadece bir kere **instantiate** (*tetiklenme diyelim*) olur. Örneğin alan hesabı yapacak bir sınıf düşünüyoruz ve bunun `calculate` method'u olsun. En x Boy bize metrekare'yi versin:

```ruby
class Area
 class << self
   def calculate(width, height)
     width * height
   end
 end
end

Area.calculate(5, 5) # => 25
```

Gördüğünüz gibi hiçbir şekilde `new` ya da benzer bir şey türetme kullanmadık direkt olarak `Area.calculate(5, 5)` şeklinde kullandık. Keza aynı işi;

```ruby
class Area
end

x = Area.new
def x.calculate(width, height)
  width * height
end
x.calculate 5,5 # => 25
```

şeklinde de yapabilirdik.

### Inheritance (Miras)

Aslında bu da bildiğimiz bir şey. Sınıftan türeme yaparkan, türettiğimiz sınıfın özellikleri türeyene miras geçer.

```ruby
class Animal
  attr_accessor :name, :kind
  
  def initialize(name)
    @name = name
  end
  
  def say_hi
    "Hello! I'm a #{@kind}, my name is #{@name}"
  end
end

class Cat < Animal
end

class Horse < Animal
end

bidik = Cat.new "Bıdık"
bidik.kind = "cat"

zuzu = Horse.new "Zuzu"
zuzu.kind = "horse"

bidik.say_hi # => "Hello! I'm a cat, my name is Bıdık"
zuzu.say_hi  # => "Hello! I'm a horse, my name is Zuzu"
```

`Cat` ve `Horse` **Animal** sınıfından `<` yöntemiyle türedi ve `Animal` deki tüm method'lar `Cat` ve `Horse`'a geçti.

### Access Level (Erişim): Public, Private, ve Protected Method'lar

Class içindeki method'lar duruma göre erişilebilirlik açısından kısıtlanabilir. `public` olanlar her yerden erişilebilirken (*bu default bir durumdur*), `private` olana sadece içeriden erişilebilir, `protected` olana ise ancak alt sınıftan türeyenden erişilebilir.

```ruby
class User
  def bu_sayede_private_cagirabilirim
    bu_sadece_iceriden
  end

  private
  def bu_sadece_iceriden
    puts "Bu private method. Bu method instance'dan çağırılamaz!"
  end

  protected
  def bu_sadece_subclass_veya_instance_dan
    puts "Bu proteced method."
  end
end

u = User.new
u.bu_sadece_iceriden # => NoMethodError: private method ‘bu_sadece_iceriden’ called for #<User:0x007feb9d0d2560>
```

Gördüğünüz gibi `bu_sadece_iceriden` method'unu `User` dan instantiate ettiğimiz `u` üzeriden çağıramıyoruz. `private` olduğu için ancak içeriden çağırılabilir:

```ruby
u.bu_sayede_private_cagirabilirim # => "Bu private method. Bu method instance'dan çağırılamaz!"
```

`public` olan `bu_sayede_private_cagirabilirim` method'u içeriden `private` method olan `bu_sadece_iceriden` 'e erişebildi. Peki ya `protected` ? Eğer direkt olarak çağırmaya kalksaydık:

```ruby
u.bu_sadece_subclass_veya_instance_dan # => NoMethodError: protected method ‘bu_sadece_subclass_veya_instance_dan’ called for #<User:0x007fff131c60a0>
```

Hemen gerekeni yapalım; `User` Class'ından başka bir Class üretelim:

```ruby
class SuperUser < User
  def initialize
    bu_sadece_subclass_veya_instance_dan
  end
end

y = SuperUser.new # => "Bu proteced method."
```

### Method Aliasing

Bazı durumlarda, üst sınıftaki method'u ezmek gerekir. Bu işlemi yaparken aslında üst sınıftaki orijinal method'a da erişmeniz gerekebilir.

```ruby
class User
  attr_accessor :name
  
  def initialize(name)
    @name = name
  end

  def give_random_age
    (20..45).to_a.sample
  end
end

class SuperUser < User
  alias :yedek :give_random_age # üst sınıftaki give_random_age’i sakladık, yedek adını verdik
  
  def give_random_age
    rnd = self.yedek
    "Kendi yaşım: 43, rnd= #{rnd}"
  end
end

u = User.new "vigo"
u.name            # => "vigo"
u.give_random_age # => 29

v = SuperUser.new "Uğur"
v.give_random_age # => "Kendi yaşım: 43, rnd= 44"
```

Örnekte, `SuperUser` Class'ında kafamıza göre `give_random_age` method'unu ezip kendi işlemimizi yaparken, üst sınıftan miras gelen orijinal method'u da yedekliyoruz, `yedek` adı altında.

### Sınıflar Açıktır, Modifiye Edilebilir!

İster Kernel'dan ister başka bir yerden gelsin, her şekilde Class'lar modifiye edilebilir. Detayları **Monkey Patching**'de göreceğiz. Kısa bir örnek yapalım. `String` Class'ına neşemize göre bir method ekleyelim:

```ruby
class String
  def hello
    "Hello: #{self}"
  end
end


"Deneme".hello # => "Hello: Deneme"
```

Tipi `String` olan her şeyin artık `hello` diye bir method'u oldu :)

### Nested Class

Aynı Module'lerde olduğu gibi, iç içe Class tanımlamak da mümkündür. Kimi zaman düzenli olmak için (*Namespace*) kimi zaman da belli bir kuralı uygulamak için kullanılır;

```ruby
class Animal
  attr_reader :name
  
  def initialize(name)
    @name = name
  end

  class Cat < Animal
  end
  
  class Horse < Animal
  end
  
  class Uber
  end
end

horse = Animal::Horse.new "Furry"
horse.name             # => "Furry"
horse.class            # => Animal::Horse
horse.class.superclass # => Animal

cat = Animal::Cat.new "Bıdık"
cat.name               # => "Bıdık"
cat.class              # => Animal::Cat
cat.class.superclass   # => Animal

alien = Animal::Uber.new
alien.respond_to?(:name) # => false
alien.class            # => Animal::Uber
alien.class.superclass # => Object
```

`Cat` ve `Horse`, `Animal` sınıfından türemiş, `Uber` ise sadece `Animal` namespace'i içinde olup kendi başına bir Class'ı temsil etmektedir.


## Module

Class'a benzeyen ama Class gibi **instantiate** edilemeyen şeydir modül. Modül denen şeye Class eklenebilir (*include edilir*) Modülden gelen methodlar artık ilgili Class'ın methodu haline gelir.

Yani düşünün ki bir Class var, bu Class'ın farklı 2-3 Class'tan özellik almasını istiyorsunuz. Bunu başarmak için ilgili Class'a o 2-3 Class'ı Modül olarak ekliyorsunuz!

Module'ler sayesinde **Namespace** ve **Mix-in** fonksiyonalitesi de gelmiş olur.

Tahmin edebileceğiniz gibi `module` kelimesiyle başlarlar ve aynı Class'larda olduğu gibi büyük harfle başlayan module adı ya da Namespace tanımlaması yapılır:

```ruby
module RandomNumbers
  def generate
    rand(10)
  end
end

class DiceGame
  include RandomNumbers
end

class RaceGame
  include RandomNumbers
end

g = DiceGame.new
g.generate # => 3

x = RaceGame.new
x.generate # => 7
```

`RandomNumbers` adında bir Module yaptık, iki farklı Class'ımız var, `DiceGame` ve `RaceGame` diye, `include` ile bu Module'ü 2 farklı Class'a ekledik. Şimdi her iki Class'ın da `generate` adında method'u oldu...

### Namespacing

Module içinde Module tanımlayabilirsiniz. Bu sayede belirlediğiniz Module tanımı altında başka alt Module'ler ve method'lar ekleyebilir, bu sayede tüm fonksiyonaliteyi ortak bir isim altından yürütebilirsiniz:

```ruby
module Framework
  module HttpFunctions
    def self.fetch_url
      "This is url fetcher"
    end
  end
end

Framework::HttpFunctions.fetch_url # => "This is url fetcher"
```

Alt Module'e ulaşmak için `::` kullandık. Aynı kodu şu şekilde de tanımlayabilirdik:

```ruby
module Framework
end

module Framework::HttpFunctions
  def self.fetch_url
    "This is url fetcher"
  end
end

Framework::HttpFunctions.fetch_url # => "This is url fetcher"
```

Bu sayede, başka bir kütüphaneden gelen Module'e ek Module'ler takma şansınız olur. Örneğin [Sinatra](http://sinatrarb.com) için ek bir özellik yapıyorsunuz. Bu durumda;

```ruby
module Sinatra::MyFeature
end
```

Şeklinde kullanabilirsiniz.

### Scope (Kapsama Alanı)

Dikkat ettiyseniz Module'ü kullanırken Class gibi instanciate etmedik. Keza örnekte `self.fetch_url` diye method tanımlaması yaptık. Aslında burada **Singleton** gibi kullandık. Örnekte `fetch_url` methodu için scope olarak `HttpFunctions` vermiş olduk. Yani `fetch_url` sadece `Framework::HttpFunctions.fetch_url` şeklinde erişilebilir oldu.

### Constants (Sabitler)

Module içinde sabit değer tanımlaması da yapmak mümkündür.

```ruby
module A
  SABIT = 5
end

A::SABIT # => 5
```

Eğer **nested** (*iç içe*) yani Module içinde Module yaparsak, sabitlere aşağıdaki gibi erişebiliriz:

```ruby
module A
  SABIT = 5
  module B
    def self.sabit_degeri_ver
      SABIT
    end
  end
end

A::SABIT              # => 5
A::B.sabit_degeri_ver # => 5
```

Peki, dışarıda tanımlanmış bir sabit varsa?

```ruby
SABIT = 5 # en dıştaki global
module A
  SABIT = 10 # içerideki
  module B
    def self.sabit_degeri_ver
      "#{::SABIT}, #{SABIT}"
    end
  end
end

A::B.sabit_degeri_ver # => "5, 10"
```

En dıştakini `::SABIT` ile aldık.

### Visibility, Access Level (Erişim):

Aynı Class'lardaki gibi `public`, `private` ve `protected` olayı Module'ler için de geçerlidir.

```ruby
module A
  def sadece_iceriden
    "Bu private method"
  end

  def bu_sayede_private_erisim_olur
    sadece_iceriden
  end

  private :sadece_iceriden
end

class Deneme
  include A
end

c = Deneme.new
c.sadece_iceriden # => NoMethodError: private method ‘sadece_iceriden’ called for #<Deneme:0x007f8f7c9188c8>
c.bu_sayede_private_erisim_olur # => "Bu private method"
```

### Extend ve Include Durumları

Ruby'de bir Class sadece tek bir Class'tan türeyebildiği için `module` ve `include` çözümlerinden bahsetmiştik:

```ruby
module Person
  attr_accessor :name
  def say_hi
    "Hello #{@name}"
  end
end

Person # => Person

class User
  include Person
  def initialize(name)
    @name = name
  end
end

User # => User

u = User.new("Uğur") # => #<User:0x007fcd42976de8 @name="Uğur">
u.say_hi # => "Hello Uğur"

u.name = "vigo"
u.say_hi # => "Hello vigo"
```

`Person` modülünden gelen `say_hi` method'una;

```ruby
User.new("Ezel").say_hi # => "Hello Ezel"
```

erişebiliyorsunuz ama ;

```ruby
User.say_hi # => undefined method `say_hi' for User:Class (NoMethodError)
```

yaptığımızda olmayan bir method çağrımı yapmış oluruz. Eğer `include` yerine `extend` kullansaydık;

```ruby
module Person
  attr_accessor :name
  
  def say_hi
    @name ||= "Undefined name"
    "Hello #{@name}"
  end
end

class User
  extend Person
  
  def initialize(name)
    @name = name
  end
end
```

```ruby
user = User.new("Yeşim") # => #<User:0x007f87c39702a0 @name="Yeşim">
user.name # => undefined method `name' for #<User:0x007f87c39702a0 @name="Yeşim"> (NoMethodError)
```

Çünki `Person`a ait özellikleri eklemek (*include*) yerine **extend** (*genişletme*) ettik ve;

```ruby
User.say_hi # => "Hello Undefined name"
User.instance_methods - Object.instance_methods # => [] # boş array
```

`say_hi` artık bir **singleton** haline geldi yani **Instance Method**'u olmak yerine **Class Method**'u oldu. Zaten ilgili sınıfın varolan method'larına baktığımızda boş `Array` döndüğünü görürüz.

Özetle, `include` ile sanki başka bir sınıftan türer gibi tüm özellikleri **instance method** olarak alırken, `extend` kullandığımızda direk **Class** kopyası gibi davranıyor.

Modülden gelen method'ları **Class** içinde **instance method** gibi kullanmak yerine **singleton method** olarak kullanacağınız zaman extend kullanabilirsiniz!