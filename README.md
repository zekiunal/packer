--------------------------
**Disclaimer:** non-English version of the guide contain unofficial translations contributed by our users. They are not binding in any way, are not guaranteed to be accurate, and have no legal effect. The official text is the [English](https://www.packer.io/docs/index.html) version of the Packer website.

--------------------------

# Packer v1.0.3

Packer dökümantasyonuna hoş geldiniz! Bu dökümantasyon Packer'daki tüm mevcut özellikler ve yetenekler için referans kaynağıdır. Packer'ı kullanmaya yeni başlıyorsanız, lütfen [başlangıç kılavuzuna](https://github.com/zekiunal/packer/blob/master/INTRODUCTION.md) göz atın.

### Kurulum

Packer'ı kurulumu oldukça basittir. Packer'ı yüklemek için iki yaklaşım vardır:

* Derlenmiş bir [dosya](https://github.com/zekiunal/packer/blob/master/README.md#derlenmiş-bir-dosya-kullanma) kullanma

* Kaynaktan [derleme](https://github.com/zekiunal/packer/blob/master/README.md#kaynaktan-derleme)

Hazır derlenmiş bir dosyayı indirmek en kolay yoldur. We provide downloads over TLS along with SHA256 sums to verify the binary. We also distribute a PGP signature with the SHA256 sums that can be verified.

#### Derlenmiş Bir Dosya Kullanma

Önceden derlenmiş dosyayı yüklemek için, işletim sisteminize uygun paketi indirin. Packer bir `zip` dosyası olarak paketlenmiştir. Başka sistem paketlerini desteklemek için kısa vadede bir planımız yok.

Zip dosyasını indirildikten sonra, herhangi bir dizine unzip ile açın. Packer'ı çalıştırmak için gereken tek şey `packer` dosyasıdır (Windows için `packer.exe`). Eğer varsa, klasürün içindeki diğer dosyalar Packer'ın çalışması için gerekli değildir.

Dosyayı sisteminizdeki herhangi bir yere kopyalayın. Komut satırından erişmeyi düşünüyorsanız, `PATH` ortam değişkeninize `packer` dosyasının konumu eklediğinizden emin olun.

#### Kaynaktan Derleme

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

#### Yüklemeyi Doğrulama

Packer'ın doğru şekilde kurulduğunu doğrulamak için, sisteminizde `packer -v` komutunu çalıştırın. Yardım çıktısını görmelisiniz. Komut satırından çalıştırıyorsanız, PATH ortam değişkeninin tanımlı olduğundan emin olun, aksi taktirde Packer'ın bulunamadığına dair bir hata alabilirsiniz. (`Packer not being found`)

```
$ packer -v
```

### Packer Terminolojisi

Packer'ı daha önce kullanmadıysanız, Packer dokümantasyonunda kullanılan bir kaç terim vardır. Neyse ki, bu terimlerin sayısı oldukça azdır. Bu sayfa Packer'ı anlamak ve kullanmak için gereken tüm terminolojileri belgelemektedir. Hızlı başvuru için terminoloji alfabetik sıradadır.

**Çıktılar (Artifacts)**, tek bir kurulumun sonucudur ve genellikle bir makine görüntüsünü temsil eden için bir dizi kimlik veya dosya grubudur. Her kurucu tek bir çıktı üretir. Örnek olarak, Amazon EC2 kurucusunda çıktı, bir AMI kimliği kümesidir (bölge başına bir tanetir). VMware oluşturucu için çıktı, oluşturulan sanal makineyi içeren bir dosya dizinidir.

**Kurulumlar (Builds)**, sonunda tek bir platform için bir görüntü üreten tek bir görevdir. Birden fazla kurulum paralel olarak çalışabilir. Örnek olarak cümle içinde kullanırsak: "Packer kurulumu, web uygulamamız için bir AMI üretti." Veya: "Packer şu anda VMware, AWS ve VirtualBox kurulumlarını çalıştırıyor."

**Kurucular (Builders)** Packer'ın tek bir platform için bir makine görüntüsü oluşturabilen bileşenleridir. Kurucular bazı yapılandırmaları okur ve bunu bir makine görüntüsü oluşturmak ve çalıştırmak için kullanır. Kurucular, elde edilen gerçek görüntüleri oluşturmak için bir kurulumun parçası olarak çağrılır. Örnek kurucular arasında VirtualBox, VMware ve Amazon EC2 bulunur. Daha fazla kurucu Packer'a eklentiler şeklinde eklenebilir.

**Komutlar (Commands)**, işi yapan paketleyicinin alt komutlarıdır. Örnek bir komut, paketleyici derlemesi olarak çağrılan `build`'dir. Packer, komut satırı arayüzünü tanımlamak için bir dizi komutla birlikte gönderilir.

**Ön tanımlı işlemler (Post-processors)**, bir kurucu veya başka bir ön tanımlı işlemin sonucunu alan ve yeni bir çıktı oluşturmak için kullanılan Packer bileşenleridir. Ön tanımlı işlemlere örnek olarak, çıktıyı yüklemek için sıkıştırmak verilebilir.

**Hazırlayıcılar (Provisioners)**, paketleyicinin statik bir görüntüye dönüştürülmeden önce çalışan bir makinede yazılım kurulumları ve yapılandırlamaları üstlenen Packer bileşenleridir. İşletim sistemi için faydalı olan yazılımlarla ilgili temel operasyonlar bu bileşen ile gerçekleştirilir. Hazırlayacılara örnek vermek gerekirse, kabuk komut dosyaları, Chef, Puppet vb. sayılarbilir.

**Şablonlar (Templates)**, Packer'ın çeşitli bileşenlerini yapılandırarak bir veya daha fazla kurulumu tanımlayan JSON dosyalarıdır. Packer, bir şablonu okuyabilir ve bu bilgileri paralel olarak birden çok makine görüntüsü oluşturmak için kullanabilir.

### Packer Komutları (CLI)
Packer, komut satırı arabirimi kullanılarak kontrol edilir. Packer ile olan tüm etkileşimler `packer` komut satırı aracı ile yapılır. Diğer birçok komut satırı aracı gibi, `packer` aracı da çalıştırmak için bir alt komut alır ve bu alt komutun da ek seçenekleri olabilir. Alt komutlar, `packer alt-komut` ile yürütülür, burada `alt-komut` gerçek komuttur.

`packer`'ı tek başına çalıştırırsanız, tüm kullanılabilir alt komutları ve yaptıklarının kısa bir özetini gösteren yardım görüntülenir. Buna ek olarak, belirli bir alt komut için daha ayrıntılı bir yardım çıktısı almak için herhangi bir `packer` komutunu -h parametresi ile çalıştırabilirsiniz.

Komut satırında bulunan belgelere ek olarak, her komut bu web sitesinde belgelenmiştir. Soldaki gezinmeyi kullanarak belirli bir alt komut belgelerini bulabilirsiniz.

#### Makine Tarafından Okunabilir Çıktı

Varsayılan olarak, Packer'ın çıktısı insan tarafından okunabilir niteliktedir. Packer'ı kullanmaktan zevk duymak için güzel biçimlendirme, boşluk ve renkler kullanıyor. Bununla birlikte, Packer otomasyon düşünülerek oluşturulmuştur. Bu amaçla, Packer, Packer'ı otomatik ortamlarda kullanmanıza izin veren, tam olarak makine tarafından okunabilen bir çıktı ayarını destekler.

Makine tarafından okunabilen çıktı biçimi  awk/sed/grep/etc'dir. Bu özellik kolay ve yeni bir format öğrenmenizi gerektirmeden tanıdık bir kullanım sağlar.

##### Makine Tarafından Okunabilir Çıktıyı Etkinleştirme

Makine tarafından okunabilen çıktı biçimi, `-machine-readable` parametresini herhangi bir Packer komutuna geçirerek etkinleştirilebilir. Bu, tüm çıktıların stdout'da makineden okunabilir olmasını sağlar. Günlüğe kaydetme etkinleştirilirse stderr'da görünmeye devam eder. Çıktının bir örneği aşağıda gösterilmiştir:

```
$ packer -machine-readable version
1498365963,,version,1.0.2
1498365963,,version-prelease,
1498365963,,version-commit,3ead2750b+CHANGES
1498365963,,ui,say,Packer v1.0.2
```

Bu konu daha sonra ayrıntılı olarak ele alınacaktır. Fakat gördüğünüz gibi, makina dostu bir çıktı elde etmek çok kolaydır. Deneyimlemek için `-machine-readable` parametresi ile  diğer bazı komutları deneyin!

> `-machine-readable` parametresi, otomatikleştirmeye yönelik tasarlanmıştır ve etkileşimli ortamlar için tasarlanmış `-debug` parametresi nin yetenekleri ile özelleştirilmiştir.

##### Makine Tarafından Okunabilir Çıktı Biçimi

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

##### Makine Tarafından Okunabilir Mesaj Türleri

Makine tarafından okunabilen mesaj türleri, [`machine-readable format`](https://www.packer.io/docs/commands/index.html#machine-readable-output) bölümünde bulunabilir. Bu bölüm, varsayılan olarak Packer çekirdeğiyle birlikte gönderilen tüm bileşenlerin yanı sıra Packer tarafından sunulan tüm ileti türleriyle ilgili belgeler içerir.

#### `build` Komutu

Packer `build` komutu bir şablon ile birlikte çalışır ve bir çıktı kümesi üretmek için içindeki tüm kurulumları çalıştırır. Bir şablonda belirtilen çeşitli kurulumlar aksi belirtilmedikçe paralel olarak yürütülür. Çıktılar, kurulum sonucunda elde edilir.

##### Seçenekler

* **-color=false** - Renkli çıktıyı devre dışı bırakır. Varsayılan olarak etkindir.

* **-debug**  - Paralelleştirmeyi devre dışı bırakır ve hata ayıklama modunu etkinleştirir. Hata ayıklama modu, kuruculara hata ayıklama bilgilerini çıkarmaları gerektiğini belirtir. Hata ayıklama modunun tam davranışı kurucuya bırakılmıştır. Genel olarak, kurucular genellikle her adım boyunca durur ve devam etmeden önce klavye girişi için bekler. Bu, kullanıcının ilerlemeyi gözlemlemesine izin verecektir.

* **-except=foo,bar,baz**  - Belirtilen virgül ile ayrılmış adlar haricindeki tüm kurulumları çalıştırır. Yapılandırma içinde belirli bir ad özniteliği belirtilmediği sürece, kurulum adları varsayılan olarak kurucularının adlarıdır.

* **-force**  - Önceki bir kurulumdaki çıktılar bir kurulumun çalışmasını engellediğinde bir kurucuyu çalıştırmaya zorlar. Zorla yaptırılan bir kurulumum kesin davranışı kurucuya bırakılmıştır. Genel olarak, mecburi kurulumları destekleyen bir kurucu önceki kulumdaki çıktıyı kaldıracaktır. Bu, kullanıcıya bu çıktıyı önceden elle temizlemenize gerek kalmadan bir kurulum oluşturmasını sağlayacaktır.


* **-on-error=cleanup (default), -on-error=abort, -on-error=ask** - Kurulum başarısız olduğunda ne yapılacağını tanımlar. `cleanup`, geçici dosyaları ve sanal makineleri silmek için önceki adımlardan sonra çalışır. `abort`, sonraki kurulumların `-force` kullanılmasını gerektirecek herhangi bir temizleme işlemi gerçekleşmeden çıkar. `ask` ne yapmak istediniğinizi sorar ve başarısız adımı temizlemeye, iptal etmeye veya yeniden denemeye karar vermenizi bekler.

* **-only=foo,bar,baz**  - Sadece virgülle ayrılmış ada sahip olan kurulumları çalıştırır. Yapılandırma içinde belirli bir ad özniteliği belirtilmediği sürece, kurulum adları varsayılan olarak kurucuların adlarıdır.

* **-parallel=false**  - Birden fazla kurucunun paralel çalışmasını devre dışı bırakır (varsayılan olarak aktiftir).

#### `fix` Komutu

Packer `fix` komutu bir şablon ile çalışır ve geriye dönük olarak uyumlu olmayan kısımlarını bulur ve Packer'ın en son sürümü ile kullanılabilmesi için güncelemeleri aktifleştirir. Yeni bir Packer sürümüne güncelledikten sonra, şablonlarınızın yeni sürümle birlikte çalışıp çalışmadığından emin olmak için fix komutunu çalıştırmalısınız.

`fix` komutu değiştirilen şablonu standart çıktıya gönderir, bu nedenle bir dosyaya kaydetmek isterseniz çıktıyı işletim sisteminize özgü teknikleri kullanarak yeniden yönlendirmeniz gerekir. Örneğin, Linux sistemlerinde, bunu yapmak isteyebilirsiniz:

```
$ packer fix old.json > new.json
```

Herhangi bir nedenle `fix` başarısız olursa, `fix` komutu "0" olmayan bir çıkış durumu ile çıkar. Hata mesajları standart hata çıktısında gözlemlenebilir; bu nedenle çıktıyı yeniden yönlendirseniz bile hata mesajlarını görürsünüz.

> `packer fix` şablonada bir değişiklik yapmasa bile, şablon çıktı olarak gönderilecektir. Konfigürasyon sıralaması ve metin girintileri gibi şeyler değişmiş olabilir. Bununla birlikte, çıktı biçimi insan tarafından okunabilir düzeyde olacaktır.

`fix` komutunun yeteneklerinin tam listesine, `packer fix -h` kullanılarak yardım menüsünden erişilebilir.

#### `inspect` Komutu

Packer `inspect` komutu bir şablon ile çalışır ve bir şablonun tanımladığı çeşitli bileşenleri listeler. Bu, JSON'a üzerinden okuma yapmak zorunda kalmadan hızlı bir şekilde bir şablon hakkında bilgi almanıza yardımcı olabilir. Komut, şeylerin bir şablonun kabul ettiği değişkenleri, kullandığı kurucuları, kurulumları ve çalışacakları sıra gibi ayrıntıları söyler.

Bu komut, `machine-readable` etkin olduğunda kullanılırsa çok faydalıdır. Komut, bileşenleri makineler tarafından çözümlenebilecek şekilde listeler.

Komut, çeşitli bileşenlerdeki gerçek kurulumları doğrulamaz (`validate` komutu bunun içindir), ancak şablonunuzun sözdizimini doğrulayacaktır.

##### Kullanım Örneği

Basit bir şablon, şöyle bir çıktı üretebilir:

```
$ packer inspect template.json
Variables and their defaults:

  aws_access_key =
  aws_secret_key =

Builders:

  amazon-ebs
  amazon-instance
  virtualbox-iso

Provisioners:

  shell
```

#### `push` Komutu

Packer `push` komutu, sizin için paketleyici yapınızı çalıştıran bir şablon ve gerekli diğer dosyaları Atlas hizmetine yükler.  [Atlas ile Packer hakkında daha fazla bilgi edinin.](https://atlas.hashicorp.com/help/packer/features)

Kurulumları uzaktan çalıştırmak, işletim sisteminizde desteklenmeyen paketleyici kurulumlarında (örneğin, Mac veya Windows üzerinde gelişmekte olan `docker` veya QEMU) çalışmayı kolaylaştırır. Ayrıca, VM'leri oluşturmanın zor tarafı olan kaynak ihtiyacına karşı, daha fazla CPU, bellek ve ağ kaynaklarına sahip sunucularda çalışır.

Atlas'da bir kurulum çalıştırmak için `push` komutunu kullandığınızda, kurulum çıktılarının Atlas'da saklamanmasını isteyebilirsiniz. Bunu yapmak için ayrıca [Atlas post-processor](https://www.packer.io/docs/post-processors/atlas.html) yapılandırmasını yapmanız gerekecektir. Bu isteğe bağlıdır. Sonuç olarak `post-processor` (Ön tanımlı işlemler) ve `push` komutları bağımsız olarak kullanılabilir.

#### `validate` Komutu

Packer, bir [şablonun](https://www.packer.io/docs/templates/index.html) sözdizimini ve yapılandırmasını doğrulamak için Packer `validate` komutunu kullanır. Komut başarıysa 0 çıkış durumu, başarısızsa 0 olmayan bir çıkış durumu döndürür. Buna ek olarak, başarısızlık durumunda, hata mesajı da verecektir.

##### Kullanım Örneği

```
$ packer validate my-template.json
Template validation failed. Errors are shown below.

Errors validating build 'vmware'. 1 error(s) occurred:

* Either a path or inline script must be specified.
```

##### Seçenekler

* **-syntax-only** - Şablonun yalnızca sözdizimi kontrol edilir. Yapılandırma doğrulanmaz.

### Şablonlar

Şablonlar, bir veya daha fazla makine imajı oluşturmak için Packer'ın çeşitli bileşenlerini yapılandıran JSON dosyalarıdır. Şablonlar taşınabilir, statik, okunabilir ve hem insanlar hem de bilgisayarlar tarafından yazılabilir. Bu özellikle, yalnızca şablonları elle yaratmak ve değiştirmekle kalmaz, aynı zamanda şablonları dinamik olarak yaratmak veya değiştirmek için program yazmanın avantajına da sahip olmanızı sağlar.

Şablonlar, şablonu alan ve içindeki kurulumları çalıştırarak makine imajlarını üreten `packer build` gibi komutlara verilir.

#### Şablon Yapısı

Bir şablon, Packer'ın çeşitli bileşenlerini yapılandıran bir dizi bölüm içeren bir JSON nesnesidir. Bir şablon içindeki mevcut bölümler aşağıda listelenmiştir. Her bölümün açıklaması ile birlikte, zorunlu olarak kullanılıp, kullanılmayacağı belirtilmiştir.

* **builders** (gerekli), makine imajları oluşturmak için kullanılacak kurucuları tanımlayan bir veya daha fazla nesne dizisidir ve bu bölüm kurucuların her birini yapılandırır. Bir kurucuyu tanımlama ve yapılandırma hakkında daha fazla bilgi için [şablonda kurucuların yapılandırılmasına](https://www.packer.io/docs/templates/builders.html) ilişkin bölümü okuyun.

* **description** (isteğe bağlı), şablonun ne yaptığının açıklanması sağlayan bölümdür. Bu çıktı yalnızca [`inspect`](https://github.com/zekiunal/packer/blob/master/README.md#inspect-komutu) komutunda kullanılır.

* **min_packer_version** (isteğe bağlı), şablonun kullanılması için gereken minimum hangi `packer` sürümüne sahip olması gerektiğini tanımlayan bölümdür. Bu, şablonla birlikte Packer'ın doğru sürümlerinin kullanılmasını sağlamak için kullanılabilir. Packer, `packer fix` ile geriye dönük uyumluluğu koruduğu için, maksimum bir sürüm belirtilemez.

* **post-processors** (isteğe bağlı), oluşturulan imajlarla ilgili çeşitli ön tanımlı işleri tanımlayan bir veya daha fazla nesneden oluşan bir dizidir. Belirtilmemişse, hiçbir ön tanımlı iş yapılmayacaktır. Ön tanımlı işlemlerin yaptıkları ve nasıl tanımlandıkları hakkında daha fazla bilgi için [şablonlarda post-işlemcileri yapılandırma](https://www.packer.io/docs/templates/post-processors.html) bölümünü okuyun.

* **provisioners** (isteğe bağlı), kurucular tarfından oluşturulan makineler için yazılım yüklemek ve yapılandırmak için kullanılacak olan hazırlayıcıları tanımlayan bir veya daha fazla nesne dizisidir. Belirtilmezse, hiçbir hazırlayıcı çalıştırılmaz. Bir hazırlayıcı tanımlama ve yapılandırma hakkında daha fazla bilgi için, [şablonlarda hazırlayıcı yapılandırma](https://www.packer.io/docs/templates/provisioners.html) bölümünü okuyun.

* **variables** (isteğe bağlı), şablonda bulunan kullanıcı değişkenlerini tanımlayan bir veya daha fazla anahtar/değer dizesinin bir nesnesidir. Belirtilmemişse, hiçbir değişken tanımlanmaz. Kullanıcı değişkenlerini tanımlama ve kullanma hakkında daha fazla bilgi için, [şablonlardaki kullanıcı değişkenleri](https://www.packer.io/docs/templates/user-variables.html) bölümünü okuyun.

#### Yorumlar

JSON yorumları desteklemiyor ve Packer tanımlayamadığı bölümleri doğrulama hataları olarak bildiriyor. Şablona yorum eklemek isterseniz, bir ağacın en üst düğümüne (root) alt çizgi önekini ekleyebilirsiniz. Örnek:

```json
{
  "_comment": "This is a comment",
  "builders": [
    {}
  ]
}
```

**Önemli:** Yalnızca ağacın en üst düğümüne (root) sahip bölümlerde altçizgi öneki olabilir. Kurucular (builders), hazırlayıcılar (provisioners) vb. bölümler doğrulama hatalarına neden olacaktır.

#### Örnek Şablon

Aşağıda, `packer build` ile çağrılabilecek temel bir şablon örneği verilmiştir. AWS'de bir makina oluşturur ve bir kere çalıştırıldıktan sonra onun üzerine bir komut dosyası kopyalar ve bu komut dosyasını SSH'yi kullanarak çalıştırır.

Not: Bu örnek, bir Amazon Web Hizmetleri hesabı gerektirir. Kurulumun gerçekleşmesi için sağlanması gereken bir takım parametreler alır. Daha fazla bilgi için [Amazon builder](https://www.packer.io/docs/builders/amazon.html) belgelerine bakın.

```json
{
  "builders": [
    {
      "type": "amazon-ebs",
      "access_key": "...",
      "secret_key": "...",
      "region": "us-east-1",
      "source_ami": "ami-fce3c696",
      "instance_type": "t2.micro",
      "ssh_username": "ubuntu",
      "ami_name": "packer {{timestamp}}"
    }
  ],

  "provisioners": [
    {
      "type": "shell",
      "script": "setup_things.sh"
    }
  ]
}
```

#### Şablonda Kurucular (builders)

Şablon içinde, kurucular (builders) bölümü, Packer'ın şablon için bir makine imajı oluşturmak için kullanması gereken tüm kurulumlardan oluşan bir dizi içerir.

Kurucular, çeşitli platformlar için makineler oluşturup bunlardan imaj üretmekle yükümlüdürler. Örneğin, EC2, VMware, VirtualBox için ayrı kurucular vardır. Packer varsayılan olarak birçok kurucu ile birlikte çalışır ve  yeni kurucular eklemek mümkündür.

Bu dokümantasyon, bir kurucunun şablonda nasıl yapılandırılacağını açıklar. Bununla birlikte, her kurucunun kullanabileceği belirli yapılandırma seçenekleri için, söz konusu kurucunun yardım sayfalarından istifade edilmelidir.

Bir şablonda, kurucu tanımlarının bir bölümü şuna benzer:

```json
{
  "builders": [
    // ... one or more builder definitions here
  ]
}
```

##### Kurucu Tanımlama

Tek bir kurucu (builder) tanımı, tam olarak bir kuruluma (build) eşlenir. Kurucu (builder) tanımı, en az bir tip girdi gerektiren bir JSON nesnesidir. Tip, kurulum için bir makine imajı oluşturmak için kullanılacak kurucunun (builder) adıdır.

Tipe ek olarak, diğer girdiler kurucunun (builder) kendisini yapılandırır. Örneğin, AWS kurucusu (AWS builder) için, `access_key`, `secret_key` ve diğer bazı ayarlar gerektirir. Bunlar doğrudan kurucu (builder) tanımına yerleştirilir.

Aşağıda, AWS kurucu (builder) yapılandırılmasına ilişkin örnek kurucu (builder) tanımı gösterilmektedir:

```json
{
  "type": "amazon-ebs",
  "access_key": "...",
  "secret_key": "..."
}
```

##### Kurucuların Adlandırılması

Packerda her kurulum bir ada sahiptir. Varsayılan olarak, ad kullanılan kurucunun adıdır. Bu genelde yeterlidir. Adlandırmalar neler olduğunu gösteren bir gösterge görevi görür. Bununla birlikte, isterseniz kurucu tanımında `name` anahtarını kullanarak özel bir ad belirtebilirsiniz.

Aynı kurucuyu kullanan birden çok kurulum tanımladıysanız, adlandırma yararlı olcaktır. Böyle bir durumda, adların benzersiz olması gerektiğinden, kuruculardan en az birinde farklı bir ad belirtmelisiniz.

##### İletişim Yolları (communicator)

Her kurulum tek bir [iletişim yolu](https://www.packer.io/docs/templates/communicator.html) ile ilişkilendirilir. İletişim yolları, uzaktaki bir makine (AWS örneği veya yerel sanal makine gibi) ile  bağlantı kurmak için kullanılır.

Çeşitli kurucuların tüm örnekleri bazı iletişim yolu (genellikle SSH) kullanır, ancak iletişim yolları özelleştirilebilir, dolayısıyla [iletişim yoları](https://www.packer.io/docs/templates/communicator.html) dokümanlarını okumanızı öneririz.

#### Şablonda İletişim Yolları (communicators)

İletişim Yolları, Packer'ın kurduğu makineye dosyaları yüklemek, komut dosyalarını çalıştırmak vb. operasyonlar için kullandığı mekanizmadır.

İletişim Yolları kurucu (builders) bölümünde yapılandırılır. Packer şu anda üç çeşit iletişim yollunu destekliyor:

* **none** - İletişim yollu kullanılamaz. Bu seçenekte, hazırlayıcıların (provisioners) çoğunda kullanılamaz.

* **ssh** - Makineye bir SSH bağlantısı kurulacaktır. Bu genellikle varsayılan değerdir.

* **winrm** - Bir WinRM bağlantısı kurulacaktır.

Yukarıdakilere ek olarak, bazı kurucular kullanabilecekleri özel iletişimcilere sahiptirler. Örneğin, Docker kurucusu, komut dosyalarını çalıştırmak için ` docker exec`, dosyaları kopyalamak için `docker cp`'yi kullanan bir "docker" iletişim yoluna sahiptir.

##### İletişim Yollarını Kullanma

Varsayılan olarak, genellikle SSH iletişim yolu kullanılır. Amazon gibi bazı kurucular otomatik olarak her şeyi yapılandırdıklarından, ek yapılandırma gerekli olmayabilir.

Bununla birlikte, bir iletişim yolu belirtmek için, bir iletişim yolu tanımı bir kurulum içinde ayarlayın. Birden fazla kurulum, farklı iletişim yollarına sahip olabilir. Örnek:

```json
{
  "builders": [
    {
      "type": "amazon-ebs",
      "communicator": "ssh"
    }
  ]
}
```

İletişim yolu belirledikten sonra, o İletişim yolu için diğer yapılandırma parametrelerini belirleyebilirsiniz. Bunlar aşağıda belgelenmiştir.

##### SSH İletişim Yolu

SSH İletişim Yolu, ana bilgisayara SSH ile bağlanır. Packer çalıştıran makinede bir SSH aracısı etkinleştirildiyse, otomatik olarak SSH aracısını uzaktaki ana makineye iletir.

SSH İletişim Yolunun seçenekleri:

* **ssh_bastion_agent_auth** (boolean) - Doğruysa, SSH aracısı, tabya (bastion host) ile kimlik doğrulaması yapmak için kullanılacaktır. Varsayılan değer `false`'dur.

* **ssh_bastion_host** (string) - Gerçek SSH bağlantısı için kullanılacak bir tabya (bastion host).

* **ssh_bastion_password** (string) - tabya (bastion host) kimliğini doğrulamak için kullanılacak şifre.

* **ssh_bastion_port** (integer) - tabya (bastion host) portu. Varsayılan değer:

* **ssh_bastion_private_key_file** (string) - tabya (bastion host) ile kimlik doğrulamasında kullanılacak özel anahtar dosyası.

* **ssh_bastion_username** (string) - tabyaya (bastion host) bağlanacak kullanıcı adı.

* **ssh_disable_agent** (boolean) - Doğruysa SSH aracı yönlendirme devre dışı kalır. Varsayılan değer `false`'dur.

* **ssh_file_transfer_method** (scp veya sftp) - Dosyaları, Güvenli kopya (varsayılan) veya SSH Dosya Aktarım Protokolü nasıl aktarılır.

* **ssh_handshake_attempts** (integer) - SSH'nin bağlanabilmesi için girişimde bulunulacak el sıkışmaları sayısı. Bu varsayılan olarak 10'dur.

* **ssh_host** (string) - SSH adresidir. Bu genellikle kurucu tarafından otomatik olarak yapılandırılır.

* **ssh_password** (string) - SSH ile kimlik doğrulamasında kullanılacak düz metin parolası.

* **ssh_port** (integer) - SSH'ye bağlanmak için port. Bu varsayılan değer 22'dir.

* **ssh_private_key_file** (string) - SSH ile kimlik doğrulama yapmak için kullanılacak bir PEM tarafından kodlanmış özel anahtar dosyasının yolu.

* **ssh_pty** (boolean) - Doğruysa SSH bağlantısı için bir PTY istenecektir. Bu varsayılan `false`'dur.

* **ssh_timeout** (string) - SSH'nin kullanılabilir olması için beklenecek süre. Packer, makinenin ne zaman önyüklendiğini belirlemek için bunu kullanır, bu nedenle bu oldukça uzun sürer. Örnek değer: "10 m"

* **ssh_username** (string) - ile SSH'ye bağlanmak için kullanıcı adı. SSH kullanılıyorsa gereklidir.

##### WinRM İletişim Yolu

WinRM İletişim Yolu seçenekleri:

* **winrm_host** (string) - WinRM'nin bağlanacağı adres.

* **winrm_port** (integer) - Bağlanılacak WinRM portu. winrm_use_ssl true olarak ayarlandığında, bu, düz şifrelenmemiş bağlantı için 5985 ve SSL için 5986 olarak varsayılanıdır.

* **winrm_username** (string) - WinRM'ye bağlanmak için kullanılacak kullanıcı adı.

* **winrm_password** (string) - WinRM'ye bağlanmak için kullanılacak parola.

* **winrm_timeout** (string) - WinRM'nin kullanılabilmesi için beklenecek süre. Bir Windows makinesinin kurulumunun genelde uzun sürdüğü için varsayılan değer "30m" dir.

* **winrm_use_ssl** (boolean) - Doğruysa, WinRM için HTTPS kullanın

* **winrm_insecure** (boolean) - Doğruysa, sunucu sertifikası zinciri ve ana makine adını kontrol etmeyin

* **winrm_use_ntlm** (boolean) - Doğruysa, hedef konukta temel kimlik doğrulama gereksinimini ortadan kaldıran varsayılan (temel kimlik doğrulama) değil, WinRM için NTLM kimlik doğrulaması kullanılacaktır. Uzaktan bağlantı kimlik doğrulaması için daha fazla bilgi [burada](https://msdn.microsoft.com/en-us/library/aa384295(v=vs.85).aspx) bulunabilir.

#### Şablonun Altyapısı

Şablonlardaki tüm dizeler, değişkenler ve işlevlerin çalışma zamanında bir yapılandırma parametresinin değerini değiştirmek için kullanılabilecek yaygın bir Packer şablonlama altyapısı tarafından işlenir.

Şablonların sözdizimi aşağıdaki sözleşmeleri kullanır:

* Herhangi bir değişken çift parantez içinde olur: {{ }}.
* Fonksiyonlar, {{timestamp}} gibi parantez içinde doğrudan belirtilir.
* Değişkenler, bir nokta ile öneklenir ve {{.Variable}} gibi büyük harf kullanılır.

##### Fonksiyonlar

Fonksiyonlar işlemleri dizeler üzerinde gerçekleştirir ve örneğin {{timestamp}} fonksiyonu geçerli zaman damgasını oluşturmak için herhangi bir dizede kullanılabilir. Bu, AMI adları gibi benzersiz anahtarlar gerektiren yapılandırmalar için kullanışlıdır. Örneğin, aynı şablona birden fazla kurucunuz olması durumunda, AMI adını `My Packer AMI {{timestamp}}` gibi bir tanım ile AMI adı saniye bazında benzersiz olacaktır. Bir saniyeden daha fazla ayrıntıya ihtiyacınız olursa  `{{uuid}}` 'i kullanmalısınız.

Kullanılabilen fonksiyonların tam listesi:

* Build_name - Çalıştırılan yapının adı.
* Build_type - Şu anda kullanılan yapıcı türü.
* Isotime [FORMAT] - UTC zaman biçimlendirilebilir. Aşağıdaki izotime biçimi referansında daha fazla örnek görebilirsiniz.
* Lower - Dizgiyi indirir.
* Pwd - Paketleyiciyi çalıştırırken çalışma dizini.
* Template_dir - Yapı için şablonun dizini.
* Timestamp - UTC'deki şu anki Unix zaman damgası.
* Uuid - Rastgele bir UUID döndürür.
* Upper - Dizeyi üst sıralar.
* User - Bir kullanıcı değişkenini belirtir.

###### Amazon Kurucusu İçin Özel:

* clean_ami_name - AMI adları yalnızca belirli karakterleri içerebilir. Bu işlev, yasadışı karakterleri '-' karakteriyle değiştirir. ":" Olduğundan örnek kullanımı yasal bir AMI adı değildir: {{isotime | clean_ami_name}}.

##### Şablonda Değişkenler

Şablonda değişkenler, kurulum sırasında Packer tarafından otomatik olarak belirlenen özel değişkenlerdir. Bazı kurucular, hazırlayıcılar ve diğer bileşenler yalnızca o bileşen için kullanılabilen şablon değişkenlerine sahiptir. Şablon değişkenleri `{{.Name}}` gibi bir nokta ile ön ekli oldukları için tanınabilir. Örneğin, [kabuk (shell)](https://www.packer.io/docs/builders/vmware-iso.html) hazırlayıcı kullanıldığında, şablon değişkenleri, Packer'ın kabuk komutunu nasıl çalıştıracağını belirlemek için kullanılan [`execute_command`](https://www.packer.io/docs/provisioners/shell.html#execute_command) parametresini özelleştirmek için kullanılabilir.

```json
{
    "provisioners": [
        {
            "type": "shell",
            "execute_command": "{{.Vars}} sudo -E -S bash '{{.Path}}'",
            "scripts": [
                "scripts/bootstrap.sh"
            ]
        }
    ]
}
```

`{{.Vars}}` ve `{{.Path}}` şablon değişkenleri sırasıyla yürütülecek olan çevre değişkenleri ve komut dosyasının yolu ile değiştirilir.

> **Not:** Şablon değişkenlerine ek olarak, kendi kullanıcı değişkenlerinizi de belirleyebilirsiniz. Kullanıcı değişkenleri hakkında daha fazla bilgi için [kullanıcı değişkenleri belgelerine](https://www.packer.io/docs/templates/user-variables.html) bakın.

###### isotime Fonksiyonu ve Biçimlendirme

`isotime` fonksiyonunda biçimlendirme,  Mon Jan 2 15:04:05 -0700 MST 2006 formatını kullanır:

|         | Day of Week  | Month         | Date | Hour    | Minute | Second | Year | Timezone |
|---------|--------------|---------------|------|---------|--------|--------|------|----------|
| Numeric | -            | 01            | 02   | 03 (15) | 04     | 05     | 06   | -0700    |
| Textual | Monday (Mon) | January (Jan) | -    | -       | -      | -      | -    | MST      |

*Parantez içindeki değerler kısaltılmış veya 24 saatlik değerleridir.*

`isotime` her zaman UTC zamanı olduğu için "-0700" daima "+0000" biçiminde biçimlendirildiğini unutmayın.

Yukarıdaki biçim seçeneklerini kullanarak bazı biçimlendirilmiş zaman örnekleri şunlardır:

```
isotime = June 7, 7:22:43pm 2014

{{isotime "2006-01-02"}} = 2014-06-07
{{isotime "Mon 1504"}} = Sat 1922
{{isotime "02-Jan-06 03\_04\_05"}} = 07-Jun-2014 07\_22\_43
{{isotime "Hour15Year200603"}} = Hour19Year201407
```

Lütfen çift tırnaklı karakterlerin şablonların içinde (bu örnekte, ami_name değerinde) kaçış karakteri ile kullanılması gerektiğini unutmayın:

```json
{
  "builders": [
    {
      "type": "amazon-ebs",
      "access_key": "...",
      "secret_key": "...",
      "region": "us-east-1",
      "source_ami": "ami-fce3c696",
      "instance_type": "t2.micro",
      "ssh_username": "ubuntu",
      "ami_name": "packer {{isotime \"2006-01-02\"}}"
    }
  ]
}
```

> **Not:** Bu örnekteki Amazon kurucunun doğru şekilde yapılandırılmasına ilişkin daha fazla bilgi için [Amazon kurucu belgelerine](https://www.packer.io/docs/builders/amazon.html) bakın.

#### Şablonda Ön Tanımlı İşlemler (post-processor)

Bir şablon içindeki `post-processor` bölümü, kurucular tarafından oluşturulmuş imajlara yapılacak olan herhangi bir ön tanımlı işlemi yapılandırır. `post-processor`'lere örnek olarak, dosyaları sıkıştırmak, çıktıları (artifacts)  yüklemek vb. işlemler olabilir.

`post-processor`'ler isteğe bağlıdır. Bir şablon içerisinde hiçbir `post-processor` tanımlanmazsa, imaj için herhangi bir işlem gerçekleştirilmeyecektir. Bir kurulumun çıktısı (artifact), kurucu tarafından oluşturulmuş imajdır.

Bu dokümantasyon sayfası, bir ön tanımlı işlemin şablonda nasıl yapılandırılacağını açıklar. Bununla birlikte, her bir ön tanımlı işlem için mevcut olan belirli konfigürasyon seçenekleri için söz konusu ön tanımlı işlemin yardım sayfalarından istifade edilmelidir.

Bir şablon içinde, `post-processor` tanımı şuna benzer:

```json
{
  "post-processors": [
    // ... one or more post-processor definitions here
  ]
}
```

Ön Tanımlı İşlem Tanımlamak

Bir şablondaki `post-processor` dizisinde, bir işlemi tanımlamak için üç yol vardır. Basit tanımlar, ayrıntılı tanımlar ve dizi tanımları. 

**Basit tanım**, sadece bir dizedir; `post-processor` işleminin adı olarak yazılır. Aşağıda bir örnek gösterilmiştir. Ön Tanımlı İşlem için ek yapılandırmaya ihtiyaç duyulmadığında basit tanımlar kullanılır.

```json
{
  "post-processors": ["compress"]
}
```

**Ayrıntılı tanımlama** bir JSON nesnesidir. Kurucu (builder) veya hazırlayıcı (provisioner) tanımına çok benzer. Ön Tanımlı İşlemin tipini belirten bir `type` alanı içerir. Ayrıca `post-processor` için ek yapılandırma içerebilir. Ön Tanımlı İşlemin tipinin ötesinde ek bir yapılandırmaya ihtiyaç duyulduğunda da ayrıntılı bir tanım kullanılır. Aşağıda bir örnek gösterilmiştir.

```json
{
  "post-processors": [
    {
      "type": "compress",
      "format": "tar.gz"
    }
  ]
}
```

Bir **dizi tanımı**, basit veya ayrıntılı tanımlardan oluşan bir JSON dizisidir. Dizide tanımlanan Ön Tanımlı İşlemler, her bir çıktının (artifact ) sonucunu diğerine aktarır ve bir ara çıktılar (artifact) oluşturarak sırayla çalıştırılır. Bir dizi tanımı başka bir dizi tanımı içermeyebilir. Sıra tanımları, birden çok ön tanımlı işlemi zincirlemek için kullanılır. Aşağıda, bir kurucunun çıktısını sıkıştırıp, daha sonra yükleyen, ancak sıkıştırılmış çıktıyı korunmayan bir örnek gösterilmektedir.

Bu ön tanımlı işlemlerin sırayla çalıştırılması çok önemlidir!

```json
{
  "post-processors": [
    [
      "compress",
      { "type": "upload", "endpoint": "http://example.com" }
    ]
  ]
}
```

Tahmin edebileceğiniz gibi, **basit** ve **ayrıntılı** tanımlar, bir **dizi tanımı** için kısa yoldur.


#### Atlas'da Vagrant Nesneleri (box) Oluşturma

Packer yoluyla vagrant nesneleri oluştururken Atlas'a ön tanımlı işlemleri sıralamak önemlidir. Sıralama, ön tanımlı işlemlerin sırayla çalışmasını ve Atlas'a yüklemeden önce vagrant nesnesini oluşturmasını sağlar.

```json
{
  "post-processors": [
    [
      {
        "type": "vagrant",
        "keep_input_artifact": false
      },
      {
        "type": "atlas",
        "only": ["virtualbox-iso"],
        "artifact": "dundlermifflin/dwight-schrute",
        "artifact_type": "vagrant.box",
        "metadata": {
          "provider": "virtualbox",
          "version": "0.0.1"
        }
      }
    ]
  ]
}
```

Atlas ön tanımlı işlemleri hakkında daha fazla bilgiyi [burada](https://www.packer.io/docs/post-processors/atlas.html) bulabilirsiniz.

#### Girdi Olarak Kullanılan Çıktılar (Input Artifacts)

Ön tanımlı işlemleri (post-processors) kullanırken, çıktıları (artifact) (kurucudan veya başka bir ön tanımlı işlemden gelen), ön tanımlı işlem (post-processors) çalıştırıldıktan sonra varsayılan olarak silinir. Bunun nedeni, genelde oluşturulan son çıktılara (artifact) giden ara çıktıları (artifact) daha sonra kullanmak istemezsiniz.

Bununla birlikte, bazı durumlarda, ara çıktıları saklamak isteyebilirsiniz. Packer'a `keep_input_artifact` yapılandırmasını `true` olarak ayarlayarak bu çıktıların saklamasını sağlayabilirsiniz. Aşağıda bir örnek gösterilmiştir:

```json
{
  "post-processors": [
    {
      "type": "compress",
      "keep_input_artifact": true
    }
  ]
}
```

Bu ayar, yalnızca belirli bir ön tanımlı işleme girdi olarak kullanılan çıktıyı kaldıracaktır. Bir dizi tipinde ön tanımlı işlem tanımladıysanız, ön tanımlı işlemler için girdi olarak kullanılan çıktı hariç olmak üzere, tüm ara çıktılar varsayılan olarak silinir; girdi olarak kullanılan çıktıları saklamak için açıkça belirtmelisiniz.

> Not: Sezgisel bir okuyucu, birden fazla `post-processors` belirtilmişse (sırayla değil) ne olacağını merak ediyor olabilir. Packer'da, tüm `post-processors` girdi olarak kullanılan çıktıları saklamak için yapılandırma yapmımız gerektiriyor mu? Cevap hayır, elbette hayır. Packer en azından bir ön tanımlı işlemmin çıktısının tutulmasını istediğini anlamaya yetecek kadar akıllıdır, bu nedenle de onu koruyacaktır.
