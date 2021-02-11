# Bölüm 5: Dosya yönetimi

## Enumeration ve Iteration

Sayılabilen nesneler `Enumerator` sınıfından türemişlerdir ve içinde döngü / tekrar yapılabilen nesneler haline geldikleri için de **Iterable** hale gelmişlerdir. Yani ne demek istiyorum?

```ruby
["a", "b", "c"].class      # => Array
["a", "b", "c"].each.class # => Enumerator
```

`["a", "b", "c"]` aslında bir `Array` iken, `#each` method'unu çağırdığımız anda elimizdeki `Array` birden `Enumerator` haline geldi ve içinde **iterasyon** yapılabilecek yani teker teker dizi içindeki elemanlara erişip istediğimizi gibi kullanabileceğimiz bir hale geldi.

**each_with_object**, **with_object**

İki method'da aynı işi yapar. Elimizde **Enumerator** varsa, yani bu içinde dolaşılabilen bir nesne ise, bu iterasyona ara elementler takabiliriz.

Bu iki method, Enumerator'deki her elemana verilen şeyi takar. Aşağıdaki örnekte `each_with_object("foo")`, `["a", "b", "c"]` dizisindeki her eleman içindir. Dolayısıyla, bu işlem sonrasında ne olduğunu anlamak için `Enumerator`ü `to_a` method'u ile `Array`e çevirdik.

```ruby
enumerator = ["a", "b", "c"].each
enumerator_with_foo = enumerator.each_with_object("foo")
enumerator_with_foo.to_a # => [["a", "foo"], ["b", "foo"], ["c", "foo"]]
```

Keza, bu durumda `enumerator_with_foo` da `each` method'unu kullanarak, `each_with_object` ile pas edilen nesneye de iterasyon esnasına ulaşabiliyoruz;

```ruby
enumerator = ["a", "b", "c"].each
enumerator_with_foo = enumerator.each_with_object("foo")
enumerator_with_foo.each do |element, obj|
  puts "eleman: #{element}, obj: #{obj}"
end

# eleman: a, obj: foo
# eleman: b, obj: foo
# eleman: c, obj: foo
```

Elimizde `Enumerator` varsa, bir şekilde sıra / index bilgisi de var demektir;

```ruby
sayilar = [1, 2, 3, 4].each
sayilar.next # => 1
sayilar.next # => 2
sayilar.next # => 3
sayilar.next # => 4
sayilar.next # => StopIteration hatası!
```

`next` method'unu kullanarak sonraki elemana ulaşabiliyoruz. Hatırlarsanız, **Array**'ler 0 index'lidir. Elimizdeki Array içinde dolaşırken index kullanmak istersek `each_with_index` method'unu kullanırız;

```ruby
["Uğur", "Yeşim", "Ezel", "Ömer"].each_with_index do |name, index|
  puts "İsim: #{name}, index: #{index}"
end
# İsim: Uğur, index: 0
# İsim: Yeşim, index: 1
# İsim: Ezel, index: 2
# İsim: Ömer, index: 3
```

Keza `for` kullanarak da aşağıdaki gibi bir işlem yapabiliriz;

```ruby
isimler = ["Uğur", "Yeşim", "Ezel", "Ömer"]
for isim in isimler
  puts "isim: #{isim}"
end
# isim: Uğur
# isim: Yeşim
# isim: Ezel
# isim: Ömer
```

**next_values**, **peek**, **peek_values**, **rewind**

Aynı `next` gibi çalışır fakat geriye `Array` döner ve ilgili index pozisyonunu ileri taşır. Sona geldiğinde de `StopIteration` hatası verir (*Exception*)

```ruby
a = [1, 2, 3, 4].each
a.next        # => 1
a.next_values # => [2]
a.next_values # => [3]
```

`peek` ile `next` den sonraki değeri görürüz. Eğer sona gelinmişse yine StopIteration raise edilir.

```ruby
a = [1, 2, 3, 4].each
a.next        # => 1
a.peek        # => 2

a.next        # => 2
a.peek        # => 3

a.next        # => 3
a.peek        # => 4

a.next        # => 4
a.peek        # => StopIteration: iteration reached an end
```

aynı `next_values` gibi `peek_values` de bize Array olarak bilgi verir `next` sonrasında kalan elemanları.

`rewind` ile pozisyonu başa alırız, yani kaydı geri sararız :)

```ruby
a = [1, 2, 3, 4].each
a.next        # => 1
a.next        # => 2
a.next        # => 3
a.rewind      # => #<Enumerator: [1, 2, 3, 4]:each>
a.peek_values # => [1]
a.next        # => 1
```

### Diğer Nesnelerdeki Enumeration Durumları

#### Fixnum

**upto**, **downto**, **times**

Bir sayıdan yukarı doğru sayarken `upto`, aşağı doğru sayarken `downto` ve kaç defa aynı işlemi yaparken de `times` kullanırız.

```ruby
# 10'dan 5'e sayıyoruz, 10'da 5'de dahil..
10.downto(5){ |i| puts "Sayı: #{i}" }
# >> Sayı: 10
# >> Sayı: 9
# >> Sayı: 8
# >> Sayı: 7
# >> Sayı: 6
# >> Sayı: 5

# 5'den 10'a sayıyoruz, 10'da 5'de dahil..
5.upto(10){ |i| puts "Sayı: #{i}" }
# >> Sayı: 5
# >> Sayı: 6
# >> Sayı: 7
# >> Sayı: 8
# >> Sayı: 9
# >> Sayı: 10

# 3 defa block içindeki kod çalışsın
# 0'dan 3'e kadar 3 hariç :)
3.times{ |i| puts "#{i}" }
# >> 0
# >> 1
# >> 2
```

#### String

Aynı mantıkta `upto` ve `downto` ilginç bir şekilde `String` için de kullanılır. Örneğin A'dan itibaren M'ye kadar demek için:

```ruby
"A".upto("M"){ |s| puts s }
# >> A
# >> B
# >> C
# >> D
# >> E
# >> F
# >> G
# >> H
# >> I
# >> J
# >> K
# >> L
# >> M
```

ya da, "AB", "AC", "AD" gibi sekans olarak gitmek gerektiğinde de;

```ruby
"AB".upto("AE"){ |s| puts s }
# >> AB
# >> AC
# >> AD
# >> AE
```

tam tersi için `downto`kullanılır.

## Ranges

Başı, sonu olan, tanımlanan belli bir aralıktaki değerleri gösteren nesneler `Range` sınıfındadır. Örneğin 0'la 5 arasındaki sayılar **Range** olarak ifade edilebilir;

```ruby
(0..5)           # => 0..5
(0..5).class     # => Range
(0..5).to_a      # => [0, 1, 2, 3, 4, 5]
(0..5).to_a.join # => "012345"
(0..5).each      # => #<Enumerator: 0..5:each>
```

`(0..5)` ifadesinde 0 ve 5 dahil olmak üzere bir aralık tanımladık. İstediğimiz gibi işleyebiliriz.

```ruby
(-10..0).to_a      # => [-10, -9, -8, -7, -6, -5, -4, -3, -2, -1, 0]
(0..-10).to_a      # => []
("a".."e").to_a    # => ["a", "b", "c", "d", "e"]
```

Eğer `..` yerine `...` kullanırsak, yani `(0..5)` dersek, 5 hariç demiş oluruz;

```ruby
(0...5).to_a         # => [0, 1, 2, 3, 4]
("a"..."e").to_a     # => ["a", "b", "c", "d"]
```

Son değer hariç mi dahil mi anlamak için `exclude_end?` method'unu kullanırız;

```ruby
(5..10).exclude_end?          # => false
(5...10).exclude_end?         # => true
```

`Range` aslında bir `Class`'dır ve her Class gibi;

```ruby
r = Range.new(0,2)     # => 0..2
r.to_a                 # => [0, 1, 2]
```

kullanılabilir. `==` ya da `eql?` method'u ile karşılaştırılabilir;

```ruby
(0..2) == (0..2)            # => true
(0..2).eql?(0..2)           # => true
(0..2) == (0...2)           # => false
(0..2) == Range.new(0,2)    # => true
(0..2).eql?(Range.new(0,2)) # => true
```

**begin**, **first**, **cover?**, **include?**, **member?**, **end**, **last**

Aralığın başlama değerini almak için `begin` ya da `first` kullanabildiğimiz gibi, `first`'e parametre geçerek, ilk N değeri de okuyabiliriz;

```ruby
(5..10).begin               # => 5
(5..10).first               # => 5
(5..10).first(2)            # => [5, 6]  # ilk 2 değeri ver
```

Belirlediğimiz aralık içinde sorgu yapmak için `cover?` ya da `include?` ya da `member?` method'unu kullanırız. `cover?` kullanırken eğer verdiğimiz değer aralık içindeyse sonuç `true` döner;

```ruby
(5..10).cover?(6)               # => true
(5..10).cover?(4)               # => false
(5..10).cover?(11)              # => false
(5..10).cover?(9)               # => true
("a".."z").cover?("b")          # => true
("a".."z").cover?("1")          # => false
("a".."z").cover?(1)            # => false
("a".."z").cover?("abc")        # => true
```

`include?` ile `cover?` arasındaki fark ise şudur; `cover?`a verilen parametredeki değer, örnekteki `abc` teker teker range içinde var mı? yani `a` var mı? `b` var mı? `c` var mı? şeklinde olurken, `include?` da `abc` var mı? şeklindedir;

```ruby
("a".."z").cover?("abc")        # => true
("a".."z").include?("abc")      # => false

("a".."z").cover?("a")        # => true
("a".."z").include?("a")      # => true
```

Tahmin edeceğiniz gibi, `end` ile son değeri alırız.

```ruby
(5..10).end                 # => 10
(5...10).end                # => 10
```

`last` ile de aynı `first` deki gibi son ya da son N değeri okuruz;

```ruby
(5..10).last                  # => 10
(5..10).last(3)               # => [8, 9, 10]
```

**min**, **max**, **size**

`min` ve `max` ile tanımlı aralıktaki en büyük/küçük değeri alırız, `size` bize boyu verir;

```ruby
(5..10).min        # => 5
(5..10).max        # => 10
(5..10).size       # => 6
```

**step** ile kaçar kaçar artacağını veririz;

```ruby
r = Range.new(0, 5)
r.step(2) # => #<Enumerator: 0..5:step(2)>
r.step(2).to_a # => [0, 2, 4]
```

## File System ve IO (Dosya Sistemi)

### File

`IO` sınıfından özellikler içeren `File` sınıfı, fiziki dosyalarla işlem yapmamızı sağlayan özellikleri sunar bize. Ruby'nin üzerinde çalıştığı işletim sistemine göre de **file permission** yani dosya üzerindeki yetki sistemi de devrededir.

Default olarak gelen Constant'ları:

```ruby
File.constants # => [:Separator, :SEPARATOR, :ALT_SEPARATOR, :PATH_SEPARATOR, :Constants, :Stat, :WaitReadable, :WaitWritable, :EAGAINWaitReadable, :EAGAINWaitWritable, :EWOULDBLOCKWaitReadable, :EWOULDBLOCKWaitWritable, :EINPROGRESSWaitReadable, :EINPROGRESSWaitWritable, :SEEK_SET, :SEEK_CUR, :SEEK_END, :RDONLY, :WRONLY, :RDWR, :APPEND, :CREAT, :EXCL, :NONBLOCK, :TRUNC, :NOCTTY, :BINARY, :SYNC, :DSYNC, :NOFOLLOW, :LOCK_SH, :LOCK_EX, :LOCK_UN, :LOCK_NB, :NULL, :FNM_NOESCAPE, :FNM_PATHNAME, :FNM_DOTMATCH, :FNM_CASEFOLD, :FNM_EXTGLOB, :FNM_SYSCASE]

File::ALT_SEPARATOR  # => nil # Bu Ruby’nin çalıştığı platforma özeldir
File::PATH_SEPARATOR # => ":"
File::SEPARATOR      # => "/"
File::Separator      # => "/"
```

Örneğin Windows'da çalışan Ruby'de `SEPARATOR` ters slash `\` şeklinde gelecektir.

### Public Class Method'ları

**absolute_path**, **expand_path**, **join**, **split**

String olarak verilen path bilgisini **absolute path**'e çevirir. Eğer ikinci parametre verilmezse **CWD** (*current working directory*) yani o an için içinde çalıştığınız directory bilgisi kullanılır.

```ruby
File.absolute_path("~")               # => "/~"
File.absolute_path(".gitignore", "~") # => "/~/.gitignore"
```

`expand_path` de bir nevi absolute path'e çevirir:

```ruby
File.expand_path("~/.gitignore") # => "/Users/vigo/.gitignore"
```

Keza daha kompleks path bulma işlerinde de kullanılır. Bu durumda `__FILE__` sabiti hayatımızı kolaylaştırır. O an Ruby script'inin çalıştığı dosyanın path'i `__FILE__` sabitindedir. Örneğin aşağıdaki gibi bir directory yapısı olsa:

    proje/
    ├── lib
    │   └── users.rb
    └── main.rb

ve `lib/users.rb` içinden, dışarıda bulunan `main.rb` dosyasının path'ine ulaşmak istesek;

```ruby
File.expand_path("../../main.rb", __FILE__)
```

şeklinde kullanırız.

`join` kullanarak, Ruby'nin çalıştığı işletim sistemine bağlı olarak, `File::SEPARATOR` kullanarak geçilen string'leri birleştiririz:

```ruby
File.join("usr", "local", "bin") # => "usr/local/bin"
```

Dizin ve dosya ayrıştırmasını da `split` ile yaparız:

```ruby
File.split("usr/local/bin/foo")   # => ["usr/local/bin", "foo"]
```

**atime**, **ctime**, **mtime**

Dosyaya son erişilen tarihi `atime` ile, dosyada yapılmış olan son değişiklik tarihini de `ctime` ile, son değişiklik zamanını da `mtime` ile alırız.

```ruby
File.atime("/Users/vigo/.gitignore") # => 2014-11-05 11:45:10 +0200
File.ctime("/Users/vigo/.gitignore") # => 2014-08-04 11:33:14 +0300
File.mtime("/Users/vigo/.gitignore") # => 2014-10-29 15:05:15 +0200
```

**basename**, **dirname**, **extname**

Path içinden dosya adını almak için `basename` kullanırız. Eğer parametre olarak atacağımız şeyi (*örneğin extension olarak .gif, .rb gibi*) geçersek bize sadece dosyanın adını verir.

```ruby
File.basename("/Users/vigo/test.rb")        # => "test.rb"
File.basename("/Users/vigo/test.rb", ".rb") # => "test"
```

Bu işin tersini de `dirname` ile yaparız, yani directory adı gerekince:

```ruby
File.dirname("/Users/vigo/test.rb") # => "/Users/vigo"
```

şekinde kullanırız. Dosyanın extension'ını öğrenmek için `extname` kullanırız.

```ruby
File.extname("test_file.rb")          # => ".rb"
File.extname("/foo/bar/test_file.rb") # => ".rb"
File.extname("test_file")             # => ""
```

**chmod**, **chown**, **lchmod**, **lchown**

Her iki komut da Unix'den gelir. **Change mod** ve **Change owner** işlerini yapmamızı sağlar. `chmod` ile Unix izinlerini ayarlarız:

```bash
-rw-r--r-- 1 vigo wheel 0 Aug 30 19:19 file-01.txt
||||||||||
|||||||||+--- Others, Execute    (x) 1 = 2^0
||||||||+---- Others, Write      (w) 2 = 2^1
|||||||+----- Others, Read       (r) 4 = 2^2
||||||+------ Group, Execute     (x) 1 = 2^0
|||||+------- Group, Write       (w) 2 = 2^1
||||+-------- Group, Read        (r) 4 = 2^2
|||+--------- Owner/User Execute (e) 1 = 2^0
||+---------- Owner/User Write   (w) 2 = 2^1
|+----------- Owner/User Read    (r) 4 = 2^2
+------------ Is Directory?      (d)
```

`file-01.txt` dosyasında, User (yani dosyanın sahibi) **R**ead ve **W**rite hakkına sahiptir. Group ve Others ise sadece **R**ead hakkına sahiptir. Bu durumda varolan bu dosyanin **chmod** değeri:

```bash
Owner/User  : Read, Write  => 4 + 2 = 6
Group       : Read         => 4     = 4
Others      : Read         => 4     = 4
----------------------------------------
644 unix file permission
```

şeklindedir. Hatta Terminal'den; `stat -f '%A' file-01.txt` yaparsak **644** olduğunu da görebiliriz. Şimdi bu dosyayı Ruby ile sadece sahibi tarafından okunur ve yazılır yapıp, başka hiçbir kimse tarafından okunamaz ve yazılamaz hale getirelim:

```ruby
File.chmod(0600, "file-01.txt")
```

Keza dosyanın sahibini de düzenlemek için `chown` kullanırız. Aynı terminaldeki gibi **KULLANICI:GRUP** şeklinde, toplamda üç parametre geçeriz. İlki kullanıcıyı belirler. `nil` ya da `-1` geçtiğimiz taktirde ilgili şeyi set etmemiş oluruz. Yani sadece grubu değiştireceksek kullanıcı için `nil` ya da `-1` geçebiliriz.

```ruby
File.chown(nil, 20, "/tmp/file-01.txt") # => 1
```

Grup ID olarak 20 geçtik, OSX'deki id 20 karşılık olarak **staff** grubuna denk gelir.

`lchmod` ve `lchown` ile normal `chmod`,`chown` farkı, `l` ile başlayanlar sembolik linkleri takip etmezler.

**ftype**, **stat**, **lstat**, **size**

Dosyanın ne tür bir dosya olduğunu `ftype` ile anlarız:

```ruby
File.ftype("/tmp/file-01.txt") # => "file"
File.ftype("/usr/")            # => "directory"
File.ftype("/dev/null")        # => "characterSpecial"
```

`stat` ile aynen biraz önce shell'den yaptığımız (*stat -f '%A' file-01.txt*) gibi aynı işi Ruby'den de yapabiliriz:

```ruby
File.stat("/tmp/file-01.txt")       # => #<File::Stat dev=0x1000004, ino=1540444, mode=0100600, nlink=1, uid=501, gid=20, rdev=0x0, size=4, blksize=4096, blocks=8, atime=2014-11-12 14:41:49 +0200, mtime=2014-11-12 14:40:13 +0200, ctime=2014-11-12 14:45:28 +0200>
File.stat("/tmp/file-01.txt").uid   # => 501
File.stat("/tmp/file-01.txt").gid   # => 20
File.stat("/tmp/file-01.txt").mtime # => 2014-11-12 14:40:13 +0200
```

`lstat` da aynı işi yapar fakat aynı `lchmod` ve `lchown` daki gibi sembolik linkleri takip etmez!

Dosyanın byte cinsinden büyüklüğünü almak için `size`kullanırız:

```ruby
File.size("/Users/vigo/.gitignore") # => 323
```

**delete**, **unlink**, **link**, **rename**, **readlink**, **symlink**

Her ikisi de dosya silmeye yarar. Eğer dosya başarıyla silinirse `1` döner, aksi halde hata alırız!

```ruby
File.delete("/tmp/foo.txt") # => 1 yani silindi
File.delete("/tmp/foo1.txt") # => No such file or directory
```

`link` ile **HARD LINK** oluşturuyoruz. Bunu dosyanın bir kopyası / yansıması gibi düşünebilirsiz. Orijinal dosya değiştikçe linklenmiş dosya da güncel içeriğe sahip olur. Link'in hangi dosyaya bağlı olduğunu da `readlink` ile okuruz:

```ruby
File.link("orijinal_dosya", "linklenecek_dosya")
File.readlink("linklenecek_dosya") # => "orijinal_dosya"
```

Sembolik link yani `symlink` için;

```ruby
File.symlink("foo.txt", "bar.txt") # => 0
File.readlink("bar.txt")           # => "foo.txt"
```

Komut satırından bakınca;

    -rw-r--r--   1 vigo wheel    0 Dec 13 16:08 foo.txt
    lrwxr-xr-x   1 vigo wheel    7 Dec 13 16:08 bar.txt -> foo.txt

şeklinde `bar.txt` dosyasının `foo.txt` dosyasına linklendiğini görürüz.

Dosya ismini değiştirmek için `rename` kullanırız.

```ruby
File.rename("/tmp/file-01.txt", "/tmp/file-01.txt.bak") # => 0
```

**file?**, **directory?**, **executable?**, **exist?**, **identical?**, **readable?**, **size?**,

Dosya gerçekten fiziki bir dosya mı? ya da directory mi? ya da bu dosya var mı?

```ruby
# file?
File.file?("/tmp/file-01.txt")      # => true (evet)
File.file?("/tmp/file-02.txt")      # => false

# directory?
File.directory?("/tmp/file-02.txt") # => false
File.directory?("/tmp/test_folder") # => true (evet)

# dosya var mı?
File.exist?("/tmp/file-01.txt") # => true (var)
File.exist?("/tmp/file-02.txt") # => false
```

Daha önce dosya izinlerinden bahsetmiş, bazı dosyaların **executable** olduğunu söylemiştik. Acaba dosya çalıştırılabilir yani executable mı?

```ruby
File.executable?("/tmp/file-01.txt")      # => false
-rw-r--r-- 1 vigo wheel   0 Nov 15 10:39 file-01.txt


File.executable?("/tmp/execuatable_file") # => true
-rwxr-xr-x 1 vigo wheel   0 Nov 15 10:43 execuatable_file
```

Executable olan dosyada `x` flag aktif gördüğünüz gibi :)

**new**, **open**

`open` method'u ile `new` aynı işi yapar. Dosya açmaya yarar. Dosyayı açarken hangi duruma göre açacağımızı yani okumak için mi? yazmak için mi? yoksa varolan dosyaya ek yapmak için mi? belirtmemiz gerekir.

```ruby
f = File.new("/tmp/test.txt", "w")
f.puts "Merhaba"
f.close
```

`/tmp/test.txt` adlı bir dosya oluşturup içine `puts` ile **Merhaba** yazdık. Eğer `cat /tmp/test.txt` yaparsanız kontrol edebilirsiniz. Dikkat ettiyseniz mode olarak "w" kullandık. Bu mode'lar neler?

| Mode | Açıklama |
| -- | -- |
| r | Read-only, sadece okumak için. Bu default mode'dur. |
| r+ | Read+write, hatta read + prepend, pointer'ı başa alır, yani bu method'la bişi yazarsanız, yazdığınız şey dosyanın başına eklenir. |
| w | Write-only, sadece yazmak içindir. Eğer dosya yoksa hata verir! |
| w+ | Read+Write, hatta read + append, pointer'ı dosyanın sonuna alır ve yazdıklarınızı sona ekler. Eğer dosya yoksa hata verir! |
| a | Write-only, Eğer dosya varsa pointer'ı sona alır ve sona ek yapar, dosya yoksa sıfırdan yeni dosya üretir. |
| a+ | Read+write, Aynı a gibi çalışır, sona ek yapar, hem okumaya hem de yazmaya izin verir. |
| b | Binary mode |
| t | Text mode |


**fnmatch**, **fnmatch?**

**File Name Match** yani dosya adı eşleştirmek. RegEx pattern'ine göre dosya adı yakalamak / kontrol etmek için kullanılır. 2 zorunlu ve 1 opsiyonel olmak üzere 3 parametre alabilir. Pattern, dosya adı ve opsiyonal olarak Flag'ler...

```ruby
File.fnmatch('foo', 'foobar.rb')                       # => false
File.fnmatch('foo*', 'foobar.rb')                      # => true
File.fnmatch('*foo*', 'test_foobar.rb')                # => true
```

Şimdi;

```ruby
File.fnmatch('**.rb', './main.rb')                     # => false
```

Bu işlemin `true` dönemsi için `FNM_DOTMATCH` flag'ini kullanacağız:

```ruby
File.fnmatch('**.rb', './main.rb', File::FNM_DOTMATCH) # => true
```


| 0:0 | 1:0 |
| -- | -- |
| FNM_DOTMATCH | Nokta ile başlayan dosyalarda * kullanımına izin ver |
| FNM_EXTGLOB | {a,b,c} gibi paternlerde global aramaya izin ver |
| FNM_PATHNAME | Path ayraçlarında * kullanımını engelle |
| FNM_CASEFOLD | Case in-sensitive yani büyük/küçük harf ayırt etme! |
| File::FNM_NOESCAPE | ESCAPE kodu kullan |

```ruby
File.fnmatch('*', '/', File::FNM_PATHNAME)             # => false
File.fnmatch('FOO*', 'foo.rb', File::FNM_CASEFOLD)     # => true
File.fnmatch('f{o,a}o*', 'foo.rb', File::FNM_EXTGLOB)  # => true
File.fnmatch('f{o,a}o*', 'fao.rb', File::FNM_EXTGLOB)  # => true
File.fnmatch('\foo*', '\foo.rb')                       # => false
File.fnmatch('\foo*', '\foo.rb', File::FNM_NOESCAPE)   # => true
```

### IO

Tüm giriş/çıkış (Input/Output) işlerinin kalbi burada atar. `File` sınıfı da `IO`'nun alt sınıfıdır. Binary okuma/yazma işlemleri, multi tasking işlemler (*Process spawning, async işler*) hep bu sınıf sayesinde çalışır.

Ruby 101 seviyesi için biraz karmaşık olsa dahi, sadece fikriniz olması açısından, en bilinen ve kullanılan birkaç method'a değinmek istiyorum.

**binread**

Binary read, yani byte-byte okuma işlemi için kullanılır. Opsiyonel olarak geçilen 2.parametre, kaç byte okumak istediğimizi, 3.parametre de offset yani kaç byte öteden okumaya başlamak gerek bunu bildirir. Yani elinizde bir dosya olsun, dosyanın ilk 100 byte'ını 20.byte'tan itibaren okumanız gerekirse kullanacağınız method budur :)

```ruby
# 1 byte atlayarak 3 byte okuduk ve
IO.binread("test.png", 3, 1) # => "PNG"
```

**binwrite**

Tahmin edeceğiniz gibi `binread` in tersi, yani Binary olarak yazma işini yapan method. Aynı şekilde opsiyonel 2 ve 3.parametreleri kullanabilirsiniz.

**copystream**

Birebir kopya yapmaya yarar. İlk parametre SOURCE yani neyi kopyalacaksınız, ikinci parametre DESTINATION yani nereye kopyalacaksınız, eğer kullanırsanız 3.parametre kopyalanacak byte adedi, eğer 4.parametre kullanırsanız aynı read/write daki gibi offset değeri olarak kullanabilirsiniz.

**foreach**

Elimizde `test-file.txt` olsun ve içinde;

    satır 1
    satır 2

yazsın... Satır-satır içinde dolaşmak için;

```ruby
IO.foreach("test-file.txt"){|x| print "bu satır: ",x}
```

Dediğimizde çıktı;

    bu satır: satır 1
    bu satır: satır 2

şeklinde içeriye block pas edip kullanabiliriz.

**popen**

Subprocess yani alt işlemler açmak için kullanılır. Özellikle Ruby üzerinden SHELL komutları çağırmak için çok kullanılan bir yöntemdir. Asenkron işler.

```ruby
# Bu işlem asenkron/alt işlem olarak çalışır...
IO.popen("date") do |response|
  system_date = response.gets
  puts "system_date: #{system_date}"
end
```

`/tmp/` dizinini listeleyelim:

```ruby
p = IO.popen("ls /tmp/")
p.pid       # => 52389
p.readlines # => ["D8D75028-234B-4F49-9358-C4C4775B4A08_IN\n", "D8D75028-234B-4F49-9358-C4C4775B4A08_OUT\n", "F7C71944B49B446081C0603DE90E4855_IN\n", "F7C71944B49B446081C0603DE90E4855_OUT\n", "KSOutOfProcessFetcher.501.OlaJUhhgKAnFsX7fZ0FyXTFxIgg=\n", "com.apple.launchd.4H4RVax25p\n", "com.apple.launchd.Kn8Wcx4NQX\n", "fo\n", "lilo.12159\n", "swtag.log\n", "test-file.txt\n"]
```

Gördüğünüz gibi **pid** yani Process ID : 52389, eğer shell'den;

    ps ax | grep 52389

derseniz;

    52389   ??  Z      0:00.00 (ls)

gibi ilgili işlemi görürsünüz.

## Exception Handling

## Kernel Modülü
