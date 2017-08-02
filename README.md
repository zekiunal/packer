--------------------------
**Disclaimer:** non-English version of the guide contain unofficial translations contributed by our users. They are not binding in any way, are not guaranteed to be accurate, and have no legal effect. The official text is the [English](https://www.packer.io/docs/index.html) version of the Packer website.

--------------------------

# Packer v1.0.3

## Packer'a Giriş
Packer dünyasına hoşgeldiniz! Bu rehber Packer'ın ne olduğunu, sunduğu avantajları ve nasıl kullanmyaya başlayacağınızı açıklayacaktır. Packer'ı zaten biliyorsanız, belgeler bölümü mevcut tüm özellikler için daha fazla referans sağlar.

### Packer Nedir?

Packer, tek bir kaynak yapılandırmasından çoklu platformlar için özdeş makine görüntüleri oluşturmak için kullanılan açık kaynaklı bir araçtır. Packer hafiftir, bilinen her işletim sisteminde çalışır ve çoklu platformlar için paralel olarak makine görüntüleri yaratarak oldukça performans gösterir. Packer, Chef veya Puppet gibi yapılandırma yönetiminin yerini tutmaz. Aslında, görüntüler oluştururken Packer, imaja yazılım yüklemek için Chef veya Puppet gibi araçları kullanabilir.

Bir makine görüntüsü, önceden yapılandırılmış bir işletim sistemi ve yeni çalışan makinelerin hızlı bir şekilde oluşturulması için kullanılan yüklenmiş bir yazılım içeren tek bir statik birimdir. Makine görüntü formatları her platform için değişir. Örnek vermek gerekirse, EC2 için [AMI'ler](https://en.wikipedia.org/wiki/Amazon_Machine_Image), VMware için VMDK/VMX dosyaları, VirtualBox için OVF vb.

## Neden Packer Kullanalım?

Önceden hazırlanmış makine görüntüleri bir çok avantaja sahiptir, ancak çoğu zaman makina görüntülerinden yararlanmaktan kaçınırız, çünkü görüntüler yaratmak ve yönetmek çok sıkıcıdır. Daha önce Makine görüntülerinin oluşturulmasını otomatikleştirmek için varolan herhangi bir araç yoktu ya da öğrenme eğrisi çok yüksekti. Sonuç olarak, Packer'dan önce, makine imajlarının oluşturulması, operasyon ekiplerinin çevikliğini tehdit etti ve bu nedenle büyük faydalara rağmen makina görüntülerinin oluşturulması tercih edilmiyordu.

Packer tüm bunları değiştirdi. Packer kullanımı kolaydır ve herhangi bir makine imajının yaratılmasını otomatikleştirir. Packer tarafından oluşturulan makina görüntülerine yazılım yüklemek ve yapılandırmak için Chef veya Puppet gibi bir çözümü kullanmaya teşvik ederek modern yapılandırma yönetimini benimser.

Başka bir deyişle: Packer, modern çağa önceden hazırlanmış makina görüntülerini getirerek, kullanılmayan potansiyelin ve yeni fırsatların değerlendirmesini sağladı.

### Paketleyiciyi Kullanmanın Avantajları
   
**Süper hızlı altyapı dağıtımı.** Packer tamamen hazır ve yapılandırılmış makina görüntülerini, birkaç dakika veya saat yerine saniyeler içinde başlatmanızı sağlar. Geliştirme amaçlı sanal makineleri saniyeler içinde piyasaya sürülebileceğinden, sadece yayına alma süreçleri için değil, geliştirme süreçleriniz için de faydalıdır.

**Birden çok sağlayıcı için taşınabilirlik.** Packer, birden fazla platform için aynı görüntüleri oluşturduğundan, yayına alma süreciniz için AWS, test/kalite kontrolü için OpenStack gibi özel bir bulutt  ve geliştirme ortamınız için VMware veya VirtualBox gibi masaüstü sanallaştırma çözümlerini kullanabilirsiniz. Her ortam için, aynı taşınabilir görüntü üretildiğinden taşınabilirlik sağlanmış olur.

**Geliştirilmiş kararlılık.** Packer, bir görüntüyü kurulurken, makineye tüm yazılımları yükler ve yapılandırır. Eğer komut dosyalarında hata varsa, makine açıldıktan birkaç dakika yerine daha erken aşamada hata yakalanabilir.

**Daha fazla test edilebilirlik.** Bir makine görüntüsü oluşturulduktan sonra, bu makine görüntüsü, herşeyin yolunda olduğunu doğrulamak için hızlı bir şekilde başlatılabilir ve "Somoke Test" uygulayabilir. Böylece, bu görüntüye sahip diğer makinelerin düzgün şekilde çalıştığından emin olabilirsiniz.

Packer, tüm bu avantajlardan faydalanmanın son derece kolay olmasını sağlar.

Ne için bekliyorsun? Başlayalım!


## Kullanım Örnekleri 

Artık Packer'ın ne yaptığını ve görüntü yaratmanın faydalarının ne olduğunu biliyorsun. Bu bölümde, Packer için bazı kullanım durumlarını listeleyeceğiz. Bunun kapsamlı bir liste olmadığını unutmayın. Burada listelenmeyen birçok kullanım örneği var. Bu liste, Packer'ın işlemlerinizi nasıl geliştirebileceği hakkında size bir fikir vermek amacıyla verilmiştir.

### Sürekli Dağıtım

Packer hafif, taşınabilir ve komut satırı tabanlıdır. Chef/Puppet'e yapılan her değişiklikte birden fazla platform için yeni makine görüntüleri oluşturmak için kullanılabilir. Bu sebeple, sürekli dağıtım hattınızın merkezine koymanız için mükemmel bir araçtır. 

Bu hattın bir parçası olarak, yeni oluşturulan görüntüler daha sonra başlatılacak, test edilecek ve altyapı değişikliklerinin çalışıp çalışmadığı doğrulanacaktır. Testler geçerliyse, görüntünün dağıtıldığında çalışacağından emin olabilirsiniz. Bu, altyapı değişiklikleri için yeni bir kararlılık ve test edilebilirlik seviyesine geçminizi sağlar.

### Dev/Prod Denkliği

Packer, geliştirme, test etme ve yayına alma ortamlarınızı olabildiğince benzer tutmaya yardımcı olur. Packer, aynı anda birden fazla platform için görüntü üretmek için kullanılabilir. Packer'ı kullanarak tek bir şablondan aynı anda bir AMI ve VMware makinesi üretebilirsiniz. Böylece yayına almak için AWS'yi ve gelişme için VMware'i (belki de Vagrant ile birlikte) için kullanabilirsiniz, 

Bu yeteneği yukarıdaki sürekli dağıtımlı durumuyla eşleştirin ve geliştirme aşamasından, yayına almaya kadar tutarlı çalışma ortamları için oldukça esnek bir sisteme sahip olun.

### Cihaz/Demo Oluşturma

Packer, paralel olarak birden fazla platform için tutarlı görüntüler oluşturduğundan, cihazlar ve tek kullanımlık ürün demoları oluşturmak için mükemmeldir. Yazılımınız değiştikçe, otomatik olarak yazılımı önceden yüklenmiş  cihazlar oluşturabilirsiniz. Potansiyel kullanıcılar daha sonra istedikleri ortamda yazılımınızı kullanmaya başlayabilir.

Yazılımı karmaşık gereksinimlerle birlikte paketlemek hiç bu kadar kolay olmamıştı.

## Packer ve HashiCorp Ekosistemi

HashiCorp, açık kaynak kodlu Vagrant, Packer, Terraform, Serf, Consul, Nomad ve ticari bir ürün olan Atlas'ın yaratıcısıdır. Packer, HashiCorp'un uygulama dağıtımını sürüme geçirilmiş, denetlenebilir, tekrarlanabilir ve işbirliğine dayalı bir süreç haline getirmek için inşa ettiği ekosistemin sadece bir parçasıdır. Modern veri merkezinin nitelikleri ve sorumlu uygulama sunumu ile ilgili inançlarımız hakkında daha fazla bilgi edinmek için Atlas Mindset: Altyapı Sürümü Kontrolü başlıklı makaleyi okuyun.

Makine görüntüleri ve dağıtılabilir eserler oluşturmak için Packer kullanıyorsanız, muhtemelen bu eserler dağıtmak için bir çözüm bulmanız gerekir. Terraform, altyapıyı oluşturmak, birleştirmek ve değiştirmek için kullanılan bir araçtır.

Aşağıda, HashiCorp'un açık kaynak projelerinin özetleri ve Atlas'ın tam bir uygulama dağıtım iş akışı oluşturmak için bunları nasıl bağladığını gösteren bir grafik bulunmaktadır.

### HashiCorp Ekosistemi

![Atlas](https://raw.githubusercontent.com/zekiunal/packer/master/atlas.png)

[Atlas](https://atlas.hashicorp.com/) HashiCorp'un tek ticari ürünüdür. Uygulama sürüm yönetimini, denetlenebilir, tekrarlanabilir ve işbirliğine dayalı yapmak için Packer, Terraform ve Consul'ü birleştirir.

[Packer](https://www.packer.io) makine görüntüleri ve AMI'ler, OpenStack görüntüleri, Docker konteynerleri gibi konuşlandırılabilir çıktılar oluşturmak için kullanılan bir HashiCorp aracıdır.

[Terraform](https://www.terraform.io) altyapıyı oluşturmak, birleştirmek ve değiştirmek için kullanılan bir HashiCorp aracıdır. Atlas iş akışında Terraform, çıktı kayıt defterini okur ve altyapıyı yönetir.

[Consul](https://www.consul.io) servisleri bulma/tanımlama, servis kayıt defteri tutma ve sağlık kontrollerini yapmak için kullanılan bir HashiCorp aracıdır. Atlas iş akışında Consul, Packer yapım aşamasında yapılandırılır ve çıktıda bulunan hizmetleri tanımlar. Consul, Packer ile yapım aşamasında yapılandırıldığından, çıktı Terraform ile dağıtıldığında, tamamıyla bağımlılıklar ve önceden tanımlanmış servis tanımları ile yapılandırılmıştır. Bu, çalışma zamanında yapılandırma hatası nedeniyle üretimde sağlıksız bir düğüm olma riskini büyük ölçüde azaltır.

[Serf](https://www.serf.io) kümeleme yönetimi ve hata algılama için bir HashiCorp aracıdır. Temel olarak Consul, hizmet tanımlama için Serf'in gossip protokolünü kullanır.

[Vagrant](https://www.vagrantup.com) yayına alınacak geliştirme ortamlarını yönetmek için kullanılan bir HashiCorp aracıdır. Vagrant, farklı ortamlar ile bir proje geliştirmenin zorluğunu azaltır ve yayın ortamında  beklenmedik davranış riskini azaltır. Geliştirme ve yayın ortamı arasındaki denkliğin korunması için Vagrant Packer'ın desteği ile yayın ortamı çıktılarını  paralel olarak kurabilir.

[Nomad](https://www.nomadproject.io) bir grup makineyi yönetmek ve bu makineler ile ilgili uygulamaları çalıştırmak için kullanılan bir HashiCorp aracıdır. Nomad, makinelerin ve uygulamaların yerini soyutlar ve bunun yerine kullanıcıların sadece neyi çalıştırmak istediklerini bildirmelerini sağlar ve Nomad nerede çalıştırılacağını ve nasıl çalıştırılacağını yönetir.

## Packer Kurulumu

Packer'ı çalıştırmak istediğiniz makineye önce yüklemelisiniz. Kurulumu kolaylaştırmak için Packer, desteklenen tüm platformlar ve mimariler için bir [dosya](https://www.packer.io/downloads.html) olarak dağıtılır. Bu sayfada, Packer'ın kaynağından nasıl derleneceği açıklanmayacaktır; derleme hakkında, [README'den](https://github.com/hashicorp/packer/blob/master/README.md) faydalanabilirsiniz ancak yalnızca ileri düzey kullanıcılar için önerilir.

### Packer'ı Yükleme

Packer'ı kurmak için önce sisteminiz için uygun paketi bulun ve indirin. Packer bir `zip` dosyası olarak paketlenmiştir.

Daha sonra indirilen paketi, Packer'ın kurulacağı bir dizinde açın. Unix sistemlerinde, yüklemeyi sadece kullanıcıyla sınırlamak isteyip istemediğinize veya sistem genelinde kurulum yapmak istemenize bağlı olarak `~/packer` veya `/usr/local/packer` hedefleri uygun olacaktır. Windows sistemlerinde, istediğiniz yere yerleştirebilirsiniz.

Paketi açtıktan sonra, dizinde `packer` adlı tek bir dosya bulunmalıdır. Yüklemenin son adımı, Packer'ı kurduğunuz dizininin `PATH` ortam değişkeninde tanımlı olduğundan emin olmaktır. Linux ve Mac'te PATH ayarlama ile ilgili talimatlar için [bu sayfaya](https://stackoverflow.com/questions/14637979/how-to-permanently-set-path-on-linux) bakın. [Bu sayfada](https://stackoverflow.com/questions/1618280/where-can-i-set-path-to-make-exe-on-windows) da PATH'ı Windows'ta ayarlama yönergeleri bulunmaktadır.

### Kurulumu Doğrulama

Packer'ı yükledikten sonra, yeni bir terminal veya konsolu açarak yüklemenin çalıştığını doğrulayın ve `packer`'ı kontrol edin:

```
$ packer
usage: packer [--version] [--help] <command> [<args>]

Available commands are:
    build       build image(s) from template
    fix         fixes templates from old versions of packer
    inspect     see components of a template
    push        push template files to a Packer build service
    validate    check that a template is valid
    version     Prints the Packer version
```

`packer`'ın bulunamadığına yönelik bir hata mesajı alırsanız, PATH ortam değişkeniniz düzgün kurulmamış demektir. Lütfen geri dönün ve PATH değişkeninizin Packer'ın yüklü olduğu dizini içerdiğinden emin olun.

Aksi takdirde, Packer kuruldu ve kullanmaya hazırsınız!

### Alternatif Kurulum Yöntemleri

`packer` dosyası kurulumun tek resmi yöntemi olmasına rağmen, alternatifler mevcuttur.

#### Homebrew

OS X ve [Homebrew](https://brew.sh/) kullanıyorsanız, Packer'ı aşağıdaki talimatlarla kurabilirsiniz:

```
$ brew install packer
```

#### Chocolatey

Windows ve [Chocolatey](http://chocolatey.org/) kullanıyorsanız, Packer'ı aşağıdaki talimatlarla kurabilirsiniz:

```
choco install packer
```

### Sorun Giderme

Bazı RedHat tabanlı Linux dağıtımlarında varsayılan olarak `packer` adlı başka bir araç daha yüklüdür. Bunu `which -a packer` komutu ile kontrol edebilirsiniz. Böyle bir hata alırsanız, bir isim çakışması olduğunu gösterir.

```
packer
/usr/share/cracklib/pw_dict.pwd: Permission denied
/usr/share/cracklib/pw_dict: Permission denied
```

Bunu düzeltmek için, `packer`'a , `packer.io` gibi farklı ad kullanan bir kısayol oluşturabilir veya istediğiniz `packer` dosyasının mutlak yolunu kullanabilirsiniz. (ör. `/usr/local/packer`)

## Imaj Oluşturma

Packer'ı kurulduktan sonra derinlere dalıp ve ilk imajımızı oluşturalım. İlk imajımız Redis kurulu bir [Amazon EC2 AMI](https://aws.amazon.com/ec2/) olacak. Bu sadece bir örnek. Packer, önceden yüklenmiş herhangi bir şeyle birçok platform için imaj oluşturabilir.

AWS hesabınız yoksa şimdi [oluşturun.](https://aws.amazon.com/free/) İmajımızı oluşturmak için "t2.micro" örneğini kullanacağız; "t2.micro" örneği, AWS [ücretsiz kullanım sınırlarında](https://aws.amazon.com/free/) yer alıyor, bu da işlemlerimizin ücretsiz olacağı anlamına geliyor. Daha Zaten bir AWS hesabınız varsa, AWS sizden bir miktar para tahsil edilebilir, ancak bu örnek için birkaç sentten daha fazla olmamalıdır.

> Not: AWS ücretsiz kullanım sınırları kapsamında yer alan bir hesap kullanmıyorsanız, bu örnekleri çalıştırmak için sizden ücret alınabilir. Ücret sadece birkaç sentten olmalıdır, ancak sonuçta daha fazla ücret ödemek durumunda kalırsanız, biz sorumluluk kabul etmiyoruz.

Packer, AWS dışındaki [pek çok platform](https://www.packer.io/docs/builders/index.html) için imajlar oluşturabilir ancak AWS, bilgisayarınıza ek bir yazılım yüklemeye gerek duymaz ve [ücretsiz kullanım sınırları](https://aws.amazon.com/free/) çoğu insan için uygundur. Bu nedenle bu örnek için AWS'yi kullanmayı seçtik. Bir AWS hesabı kurmaktan rahatsızsanız, diğer platformlar için de geçerli olan temel ilkelerle birlikte denemekten çekinmeyin.

### Şablon

Yapılandırmayı tanımlamak için kullanılan yapılandırma dosyasına Packer terminolojisinde şablon (template) denir. Şablonun biçimi basit JSON'dur. JSON, insan ve makine tarafından düzenlemede en iyi dengeye sahiptir; hem elle yapılmış şablonların hem de makinenin ürettiği şablonların kolayca oluşturulmasına izin verir.

Şablonun tamamını oluşturarak başlayacağız, ardından her bir bölümün üzerinde kısaca duracağız. example.json dosyası oluşturun ve aşağıdaki içeriği doldurun:

```json
{
  "variables": {
    "aws_access_key": "",
    "aws_secret_key": ""
  },
  "builders": [{
    "type": "amazon-ebs",
    "access_key": "{{user `aws_access_key`}}",
    "secret_key": "{{user `aws_secret_key`}}",
    "region": "us-east-1",
    "source_ami_filter": {
      "filters": {
      "virtualization-type": "hvm",
      "name": "*ubuntu-xenial-16.04-amd64-server-*",
      "root-device-type": "ebs"
      },
      "owners": ["099720109477"],
      "most_recent": true
    },
    "instance_type": "t2.micro",
    "ssh_username": "ubuntu",
    "ami_name": "packer-example {{timestamp}}"
  }]
}
```

Gizli anahtarlarınızı şablondan tanımlamamak için, [kullanıcı değişkenleri](https://www.packer.io/docs/templates/user-variables.html) olarak aws_access_key ve aws_secret_key'yi geçireceksiniz. [Bu sayfada](https://console.aws.amazon.com/iam/home?#security_credential) kimlik bilgileri oluşturma hakkında bilgi alabilirsiniz. Örnek IAM ilke belgesi [Amazon EC2 dokümanlarında](https://www.packer.io/docs/builders/amazon.html) bulunabilir.

Bu, kullanıma hazır olan temel bir şablondur. Temel bir JSON nesnesi olarak hemen tanınabilir. Nesnede, kurucular (`builders`) bölümü, belirli bir kurucuyu yapılandıran bir dizi JSON nesnesi içerir. Kurucu (`builder`), bir makine yaratmak ve makineyi bir imaja dönüştürmekle yükümlü olan Packer'ın bir bileşenidir.

Bu örnekte, yalnızca `amazon-ebs` türünde tek bir kurucu (`builder`) yapılandırıyoruz. Amazon EC2 AMI kurucusu Packer ile birlikte gelir. Bu kurucu, kaynak bir AMI başlatarak EBS destekli bir AMI oluşturur, bunun üzerine gerekli hazırlıkları yapar ve yeni bir AMI oluşturur.

Nesne içindeki ek tanımlar, erişim sağlayıcı bilgileri, kullanılacak kaynak AMI ve benzeri şeyleri belirten bu kurucu için gerekli yapılandırmadır. Bir kurucu için mevcut yapılandırma değişkenlerinin tam seti her üreticiye özeldir ve bu [belgede](https://www.packer.io/docs/index.html) bulunabilir.

Bu şablonu kullanmadan ve bir imaj oluşturmadan önce, `packer validate example.json` komutunu çalıştırarak şablonu doğrulalım. Bu komut, doğrulama yapmak için sözdizimini ve yapılandırma değerlerini kontrol eder. Şablonun geçerli olması gerektiği için çıkış aşağıdaki gibi görünmelidir. Herhangi bir hata varsa, bu komut size hatayı bildirecektir.

```
$ packer validate example.json
Template validated successfully.
```

Şimdi, bu şablondan imajını oluşturalım.

Zeki bir okuyucu, önceden Redis kurulmuş bir imaj oluşturacağımızı ve buna rağmen yaptığımız şablonun herhangi bir yerinde Redis'e referans verilmediğini söyleyebilir. Aslında dokümantasyonun bu kısmı yalnızca ilk temel, hazırlıkları yapılmamış bir görüntü oluşturulmasını kapsar. Hazırlama konusunu ele alan bir sonraki bölümde Redis'in kurulumu ele alınacaktır.

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

> `-machine-readable` parametresi, otomatikleştirmeye yönelik tasarlanmıştır ve etkileşimli ortamlar için tasarlanmış `-debug` parametresi nin yetenekleri ile özelleştirilmiştir.

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

### `build` Komutu

Packer `build` komutu bir şablon ile birlikte çalışır ve bir çıktı kümesi üretmek için içindeki tüm kurulumları çalıştırır. Bir şablonda belirtilen çeşitli kurulumlar aksi belirtilmedikçe paralel olarak yürütülür. Çıktılar, kurulum sonucunda elde edilir.

#### Seçenekler

* **-color=false** - Renkli çıktıyı devre dışı bırakır. Varsayılan olarak etkindir.

* **-debug**  - Paralelleştirmeyi devre dışı bırakır ve hata ayıklama modunu etkinleştirir. Hata ayıklama modu, kuruculara hata ayıklama bilgilerini çıkarmaları gerektiğini belirtir. Hata ayıklama modunun tam davranışı kurucuya bırakılmıştır. Genel olarak, kurucular genellikle her adım boyunca durur ve devam etmeden önce klavye girişi için bekler. Bu, kullanıcının ilerlemeyi gözlemlemesine izin verecektir.

* **-except=foo,bar,baz**  - Belirtilen virgül ile ayrılmış adlar haricindeki tüm kurulumları çalıştırır. Yapılandırma içinde belirli bir ad özniteliği belirtilmediği sürece, kurulum adları varsayılan olarak kurucularının adlarıdır.

* **-force**  - Önceki bir kurulumdaki çıktılar bir kurulumun çalışmasını engellediğinde bir kurucuyu çalıştırmaya zorlar. Zorla yaptırılan bir kurulumum kesin davranışı kurucuya bırakılmıştır. Genel olarak, mecburi kurulumları destekleyen bir kurucu önceki kulumdaki çıktıyı kaldıracaktır. Bu, kullanıcıya bu çıktıyı önceden elle temizlemenize gerek kalmadan bir kurulum oluşturmasını sağlayacaktır.


* **-on-error=cleanup (default), -on-error=abort, -on-error=ask** - Kurulum başarısız olduğunda ne yapılacağını tanımlar. `cleanup`, geçici dosyaları ve sanal makineleri silmek için önceki adımlardan sonra çalışır. `abort`, sonraki kurulumların `-force` kullanılmasını gerektirecek herhangi bir temizleme işlemi gerçekleşmeden çıkar. `ask` ne yapmak istediniğinizi sorar ve başarısız adımı temizlemeye, iptal etmeye veya yeniden denemeye karar vermenizi bekler.

* **-only=foo,bar,baz**  - Sadece virgülle ayrılmış ada sahip olan kurulumları çalıştırır. Yapılandırma içinde belirli bir ad özniteliği belirtilmediği sürece, kurulum adları varsayılan olarak kurucuların adlarıdır.

* **-parallel=false**  - Birden fazla kurucunun paralel çalışmasını devre dışı bırakır (varsayılan olarak aktiftir).

### `fix` Komutu

Packer `fix` komutu bir şablon ile çalışır ve geriye dönük olarak uyumlu olmayan kısımlarını bulur ve Packer'ın en son sürümü ile kullanılabilmesi için güncelemeleri aktifleştirir. Yeni bir Packer sürümüne güncelledikten sonra, şablonlarınızın yeni sürümle birlikte çalışıp çalışmadığından emin olmak için fix komutunu çalıştırmalısınız.

`fix` komutu değiştirilen şablonu standart çıktıya gönderir, bu nedenle bir dosyaya kaydetmek isterseniz çıktıyı işletim sisteminize özgü teknikleri kullanarak yeniden yönlendirmeniz gerekir. Örneğin, Linux sistemlerinde, bunu yapmak isteyebilirsiniz:

```
$ packer fix old.json > new.json
```

Herhangi bir nedenle `fix` başarısız olursa, `fix` komutu "0" olmayan bir çıkış durumu ile çıkar. Hata mesajları standart hata çıktısında gözlemlenebilir; bu nedenle çıktıyı yeniden yönlendirseniz bile hata mesajlarını görürsünüz.

> `packer fix` şablonada bir değişiklik yapmasa bile, şablon çıktı olarak gönderilecektir. Konfigürasyon sıralaması ve metin girintileri gibi şeyler değişmiş olabilir. Bununla birlikte, çıktı biçimi insan tarafından okunabilir düzeyde olacaktır.

`fix` komutunun yeteneklerinin tam listesine, `packer fix -h` kullanılarak yardım menüsünden erişilebilir.

### `inspect` Komutu

Packer `inspect` komutu bir şablon ile çalışır ve bir şablonun tanımladığı çeşitli bileşenleri listeler. Bu, JSON'a üzerinden okuma yapmak zorunda kalmadan hızlı bir şekilde bir şablon hakkında bilgi almanıza yardımcı olabilir. Komut, şeylerin bir şablonun kabul ettiği değişkenleri, kullandığı kurucuları, kurulumları ve çalışacakları sıra gibi ayrıntıları söyler.

Bu komut, `machine-readable` etkin olduğunda kullanılırsa çok faydalıdır. Komut, bileşenleri makineler tarafından çözümlenebilecek şekilde listeler.

Komut, çeşitli bileşenlerdeki gerçek kurulumları doğrulamaz (`validate` komutu bunun içindir), ancak şablonunuzun sözdizimini doğrulayacaktır.

#### Kullanım Örneği

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

### `push` Komutu

Packer `push` komutu, sizin için paketleyici yapınızı çalıştıran bir şablon ve gerekli diğer dosyaları Atlas hizmetine yükler.  [Atlas ile Packer hakkında daha fazla bilgi edinin.](https://atlas.hashicorp.com/help/packer/features)

Kurulumları uzaktan çalıştırmak, işletim sisteminizde desteklenmeyen paketleyici kurulumlarında (örneğin, Mac veya Windows üzerinde gelişmekte olan `docker` veya QEMU) çalışmayı kolaylaştırır. Ayrıca, VM'leri oluşturmanın zor tarafı olan kaynak ihtiyacına karşı, daha fazla CPU, bellek ve ağ kaynaklarına sahip sunucularda çalışır.

Atlas'da bir kurulum çalıştırmak için `push` komutunu kullandığınızda, kurulum çıktılarının Atlas'da saklamanmasını isteyebilirsiniz. Bunu yapmak için ayrıca [Atlas post-processor](https://www.packer.io/docs/post-processors/atlas.html) yapılandırmasını yapmanız gerekecektir. Bu isteğe bağlıdır. Sonuç olarak `post-processor` (Ön tanımlı işlemler) ve `push` komutları bağımsız olarak kullanılabilir.

### `validate` Komutu

Packer, bir [şablonun](https://www.packer.io/docs/templates/index.html) sözdizimini ve yapılandırmasını doğrulamak için Packer `validate` komutunu kullanır. Komut başarıysa 0 çıkış durumu, başarısızsa 0 olmayan bir çıkış durumu döndürür. Buna ek olarak, başarısızlık durumunda, hata mesajı da verecektir.

#### Kullanım Örneği

```
$ packer validate my-template.json
Template validation failed. Errors are shown below.

Errors validating build 'vmware'. 1 error(s) occurred:

* Either a path or inline script must be specified.
```

#### Seçenekler

* **-syntax-only** - Şablonun yalnızca sözdizimi kontrol edilir. Yapılandırma doğrulanmaz.
