--------------------------
**Disclaimer:** non-English version of the guide contain unofficial translations contributed by our users. They are not binding in any way, are not guaranteed to be accurate, and have no legal effect. The official text is the [English](https://www.packer.io/docs/index.html) version of the Packer website.

--------------------------

# Packer v1.0.3

Packer dünyasına hoşgeldiniz! Bu rehber Packer'ın ne olduğunu, sunduğu avantajları ve nasıl kullanmyaya başlayacağınızı açıklayacaktır. Packer'ı zaten biliyorsanız, belgeler bölümü mevcut tüm özellikler için daha fazla referans sağlar.

## Kurulum

Packer'ı kurulumu oldukça basittir. Packer'ı yüklemek için iki yaklaşım vardır:

* Derlenmiş bir dosya kullanma

* Kaynaktan derleme

Hazır derlenmiş bir dosyayı indirmek en kolay yoldur.

### Derlenmiş bir dosya kullanma

Önceden derlenmiş dosyayı yüklemek için, işletim sisteminize uygun paketi indirin. Packer bir `zip` dosyası olarak paketlenmiştir. Başka sistem paketlerini desteklemek için kısa vadede bir planımız yok.

Zip dosyasını indirildikten sonra, herhangi bir dizine unzip ile açın. Packer'ı çalıştırmak için gereken tek şey `packer` dosyasıdır (Windows için `packer.exe`). Eğer varsa, klasürün içindeki diğer dosyalar Packer'ın çalışması için gerekli değildir.

Dosyayı sisteminizdeki herhangi bir yere kopyalayın. Komut satırından erişmeyi düşünüyorsanız, `PATH` ortam değişkeninize `packer` dosyasının konumu eklediğinizden emin olun.

### Kaynaktan Derleme

Kaynaklardan derlemek için [Go'nun](https://golang.org/) yüklü olması ve düzgün yapılandırılması (GOPATH ortam değişkeni seti de dahil olmak üzere) ve PATH ortam değişkeninde `git` dosya yolunun tanımlı olması gerekir.

1. Packer'ı GitHub üzerinden GOPATH konumuna kopyalayın:

```
$ mkdir -p $GOPATH/src/github.com/hashicorp && cd $_
$ git clone https://github.com/hashicorp/packer.git
$ cd packer
```

2. Mevcut işletim sisteminiz için Packer'ı oluşturun ve dosyayı `./bin/` dizinine yerleştirin. `make dev`, sadece yerel ortamınız için `packer` derleyen bir kısayoldur (çapraz derleme yapmaz).

```
$ make dev
```

### Yüklemeyi Doğrulama

Packer'ın doğru şekilde kurulduğunu doğrulamak için, sisteminizde `packer -v` komutunu çalıştırın. Yardım çıktısını görmelisiniz. Komut satırından çalıştırıyorsanız, PATH ortam değişkeninin tanımlı olduğundan emin olun, aksi taktirde Packer'ın bulunamadığına dair bir hata alabilirsiniz. (`Packer not being found`)

```
$ packer -v
```

## Packer Terminolojisi

Packer'ı daha önce kullanmadıysanız, Packer dokümantasyonunda kullanılan bir kaç terim vardır. Neyse ki, bu terimlerin sayısı oldukça azdır. Bu sayfa Packer'ı anlamak ve kullanmak için gereken tüm terminolojileri belgelemektedir. Hızlı başvuru için terminoloji alfabetik sıradadır.

**Çıktılar (Artifacts)**, tek bir kurulumun sonucudur ve genellikle bir makine görüntüsünü temsil eden için bir dizi kimlik veya dosya grubudur. Her kurucu tek bir çıktı üretir. Örnek olarak, Amazon EC2 kurucusunda çıktı, bir AMI kimliği kümesidir (bölge başına bir tanetir). VMware oluşturucu için çıktı, oluşturulan sanal makineyi içeren bir dosya dizinidir.

**Kurulumlar (Builds)**, sonunda tek bir platform için bir görüntü üreten tek bir görevdir. Birden fazla kurulum paralel olarak çalışabilir. Örnek olarak cümle içinde kullanırsak: "Packer kurulumu, web uygulamamız için bir AMI üretti." Veya: "Packer şu anda VMware, AWS ve VirtualBox kurulumlarını çalıştırıyor."

**Kurucular (Builders)** Packer'ın tek bir platform için bir makine görüntüsü oluşturabilen bileşenleridir. Kurucular bazı yapılandırmaları okur ve bunu bir makine görüntüsü oluşturmak ve çalıştırmak için kullanır. Kurucular, elde edilen gerçek görüntüleri oluşturmak için bir kurulumun parçası olarak çağrılır. Örnek kurucular arasında VirtualBox, VMware ve Amazon EC2 bulunur. Daha fazla kurucu Packer'a eklentiler şeklinde eklenebilir.

**Komutlar (Commands)**, işi yapan paketleyicinin alt komutlarıdır. Örnek bir komut, paketleyici derlemesi olarak çağrılan `build`'dir. Packer, komut satırı arayüzünü tanımlamak için bir dizi komutla birlikte gönderilir.

**Ön tanımlı işlemler (Post-processors)**, bir kurucu veya başka bir ön tanımlı işlemin sonucunu alan ve yeni bir çıktı oluşturmak için kullanılan Packer bileşenleridir. Ön tanımlı işlemlere örnek olarak, çıktıyı yüklemek için sıkıştırmak verilebilir.

**Hazırlayıcılar (Provisioners)**, paketleyicinin statik bir görüntüye dönüştürülmeden önce çalışan bir makinede yazılım kurulumları ve yapılandırlamaları üstlenen Packer bileşenleridir. İşletim sistemi için faydalı olan yazılımlarla ilgili temel operasyonlar bu bileşen ile gerçekleştirilir. Hazırlayacılara örnek vermek gerekirse, kabuk komut dosyaları, Chef, Puppet vb. sayılarbilir.

**Şablonlar (Templates)**, Packer'ın çeşitli bileşenlerini yapılandırarak bir veya daha fazla kurulumu tanımlayan JSON dosyalarıdır. Packer, bir şablonu okuyabilir ve bu bilgileri paralel olarak birden çok makine görüntüsü oluşturmak için kullanabilir.

## Packer Komutları (CLI)
Packer, komut satırı arabirimi kullanılarak kontrol edilir. Packer ile olan tüm etkileşimler `packer` komut satırı aracı ile yapılır. Diğer birçok komut satırı aracı gibi, `packer` aracı da çalıştırmak için bir alt komut alır ve bu alt komutun da ek seçenekleri olabilir. Alt komutlar, `packer alt-komut` ile yürütülür, burada `alt-komut` gerçek komuttur.

`packer`'ı tek başına çalıştırırsanız, tüm kullanılabilir alt komutları ve yaptıklarının kısa bir özetini gösteren yardım görüntülenir. Buna ek olarak, belirli bir alt komut için daha ayrıntılı bir yardım çıktısı almak için herhangi bir `packer` komutunu -h parametresi ile çalıştırabilirsiniz.

Komut satırında bulunan belgelere ek olarak, her komut bu web sitesinde belgelenmiştir. Soldaki gezinmeyi kullanarak belirli bir alt komut belgelerini bulabilirsiniz.

### Makine tarafından okunabilir çıktı

Varsayılan olarak, Packer'ın çıktısı insan tarafından okunabilir niteliktedir. Packer'ı kullanmaktan zevk duymak için güzel biçimlendirme, boşluk ve renkler kullanıyor. Bununla birlikte, Packer otomasyon düşünülerek oluşturulmuştur. Bu amaçla, Packer, Packer'ı otomatik ortamlarda kullanmanıza izin veren, tam olarak makine tarafından okunabilen bir çıktı ayarını destekler.

Makine tarafından okunabilen çıktı biçimi  awk/sed/grep/etc'dir. Bu özellik kolay ve yeni bir format öğrenmenizi gerektirmeden tanıdık bir kullanım sağlar.

### Makine tarafından okunabilir çıktıyı etkinleştirme

Makine tarafından okunabilen çıktı biçimi, `-machine-readable` parametresini herhangi bir Packer komutuna geçirerek etkinleştirilebilir. Bu, tüm çıktıların stdout'da makineden okunabilir olmasını sağlar. Günlüğe kaydetme etkinleştirilirse stderr'da görünmeye devam eder. Çıktının bir örneği aşağıda gösterilmiştir:

```
$ packer -machine-readable version
1498365963,,version,1.0.2
1498365963,,version-prelease,
1498365963,,version-commit,3ead2750b+CHANGES
1498365963,,ui,say,Packer v1.0.2
```

Bu konu daha sonra ayrıntılı olarak ele alınacaktır. Fakat gördüğünüz gibi, makina dostu bir çıktı elde etmek çok kolaydır. Deneyimlemek için `-machine-readable` parametresi ile  diğer bazı komutları deneyin!

> `-machine-readable` parametresi, otomatikleştirmeye yönelik için tasarlanmıştır ve etkileşimli ortamlar için tasarlanmış `-debug` parametresi nin yetenekleri ile özelleşmiştirilmiştir.

### Makine tarafından okunabilir çıktı biçimi

Makine tarafından okunabilir format, satır yönelimli, virgülle sınırlandırılmış bir metin biçimidir. Bu, `Ruby` veya `Python` gibi  programlama dillerine ek olarak `awk` veya `grep` gibi standart Unix araçlarını kullanarak ayrıştırmanın daha kolay olmasını sağlar.

Format:

```
timestamp,target,type,data...
```

Her bileşen aşağıda açıklanmıştır:

**timestamp**, UTC'de Unix zaman damgasıdır.

**target**, çıktıların hedefidir. Packer küresel olarak tanımlıysa, buradaki çıktı boş bir değer alacaktır. Aksi takdirde, genellikle bir kurulumun adıdır, böylece paralel kurulumlar çalışırken çıktıyı belirli bir kurulum ile ilişkilendirebilirsiniz.

**type**, çıktısı makine tarafından okunabilen mesaj türüdür. Daha sonra kapsanan bir dizi standart tür vardır, ancak Packer'ın her bir bileşeni (kurucular, hazırlayıcılar, vb.) kendi özel türlerinde çıktı verebilir, bu da makine tarafından okunabilir çıktıların sonsuz esneklikte olmasını sağlar.

**data** Önceki türle ilişkili olan virgülle ayrılmış değerlerdir. Bu verilerin tam miktarı ve anlamı türüne bağlıdır, bu nedenle tam olarak anlamak için türe ait belgeleri okumanız gerekir.

Within the format, if data contains a comma, it is replaced with %!(PACKER_COMMA). This was preferred over an escape character such as \' because it is more friendly to tools like awk.

Newlines within the format are replaced with their respective standard escape sequence. Newlines become a literal \n within the output. Carriage returns become a literal \r.

### Makine Tarafından Okunabilir Mesaj Türleri

Makine tarafından okunabilen mesaj türleri, `machine-readable format` dokümantasyon bölümünde bulunabilir. Bu bölüm, varsayılan olarak Packer çekirdeğiyle birlikte gönderilen tüm bileşenlerin yanı sıra Packer tarafından sunulan tüm ileti türleriyle ilgili belgeler içerir.
