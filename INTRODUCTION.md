--------------------------
**Disclaimer:** non-English version of the guide contain unofficial translations contributed by our users. They are not binding in any way, are not guaranteed to be accurate, and have no legal effect. The official text is the [English](https://www.packer.io/intro/index.html) version of the Packer website.

--------------------------

# Packer Başlangıç Kılavuzu

Packer dünyasına hoşgeldiniz! Bu rehber Packer'ın ne olduğunu, sunduğu avantajları ve nasıl kullanmyaya başlayacağınızı açıklayacaktır. Packer'ı zaten biliyorsanız, [dokümantasyon](https://github.com/zekiunal/packer/blob/master/README.md) mevcut tüm özellikler için daha fazla referans sağlar.

## Packer Nedir?

Packer, tek bir kaynak yapılandırmasından çoklu platformlar için özdeş makine imajları oluşturmak için kullanılan açık kaynaklı bir araçtır. Packer hafiftir, bilinen her işletim sisteminde çalışır ve çoklu platformlar için paralel olarak makine imajları yaratarak oldukça performans gösterir. Packer, Chef veya Puppet gibi yapılandırma yönetiminin yerini tutmaz. Aslında, imaj oluştururken Packer, imaja yazılım yüklemek için Chef veya Puppet gibi araçları kullanabilir.

Bir makine imajı, önceden yapılandırılmış bir işletim sistemi ve yeni çalışan makinelerin hızlı bir şekilde oluşturulması için kullanılan yüklenmiş bir yazılım içeren tek bir statik birimdir. Makine imaj formatları her platform için değişir. Örnek vermek gerekirse, EC2 için [AMI'ler](https://en.wikipedia.org/wiki/Amazon_Machine_Image), VMware için VMDK/VMX dosyaları, VirtualBox için OVF vb.

## Neden Packer Kullanmalıyım?

Önceden hazırlanmış makine imajları bir çok avantaja sahiptir, ancak çoğu zaman makina imajlarından yararlanmaktan kaçınırız, çünkü imaj yaratmak ve yönetmek çok sıkıcıdır. Daha önce makine imajlarının oluşturulmasını otomatikleştirmek için varolan herhangi bir araç yoktu ya da öğrenme eğrisi çok yüksekti. Sonuç olarak, Packer'dan önce, makine imajlarının oluşturulması, operasyon ekiplerinin çevikliğini tehdit etti ve bu nedenle büyük faydalara rağmen makina imajlarının oluşturulması tercih edilmiyordu.

Packer tüm bunları değiştirdi. Packer'ın kullanımı kolaydır ve herhangi bir makine imajının yaratılmasını otomatikleştirir. Packer tarafından oluşturulan makina imajlarına yazılım yüklemek ve yapılandırmak için Chef veya Puppet gibi bir çözümü kullanmaya teşvik ederek modern yapılandırma yönetimini benimser.

Başka bir deyişle: Packer, modern çağa önceden hazırlanmış makina imajlarını getirerek, kullanılmayan potansiyelin ve yeni fırsatların değerlendirmesini sağlar.

### Packer Kullanmanın Avantajları
   
**Süper hızlı altyapı dağıtımı.** Packer tamamen hazır ve yapılandırılmış makina imajlarını, birkaç dakika veya saat yerine saniyeler içinde başlatmanızı sağlar. Geliştirme amaçlı sanal makineleri saniyeler içinde piyasaya sürülebileceğinden, sadece yayına alma süreçleri için değil, geliştirme süreçleriniz için de faydalıdır.

**Birden çok sağlayıcı için taşınabilirlik.** Packer, birden fazla platform için aynı imajları oluşturduğundan, yayına alma süreciniz için AWS, test/kalite kontrolü için OpenStack gibi özel bir bulut ve geliştirme ortamınız için VMware veya VirtualBox gibi masaüstü sanallaştırma çözümlerini kullanabilirsiniz. Her ortam için, aynı taşınabilir imajlar üretildiğinden taşınabilirlik sağlanmış olur.

**Geliştirilmiş kararlılık.** Packer, bir imajı kurulurken, makineye tüm yazılımları yükler ve yapılandırır. Eğer komut dosyalarında hata varsa, makine açıldıktan birkaç dakika yerine daha erken aşamada hata yakalanabilir.

**Daha fazla test edilebilirlik.** Bir makine imajı oluşturulduktan sonra, bu makine imajı, herşeyin yolunda olduğunu doğrulamak için hızlı bir şekilde başlatılabilir ve "Smoke Test" uygulayabilir. Böylece, bu imaja sahip diğer makinelerin düzgün şekilde çalıştığından emin olabilirsiniz.

Packer, tüm bu avantajlardan faydalanmanın son derece kolay olmasını sağlar.

Ne için bekliyorsun? Başlayalım!

## Kullanım Örnekleri 

Artık Packer'ın ne yaptığını ve imaj yaratmanın faydalarının ne olduğunu biliyorsun. Bu bölümde, Packer için bazı kullanım durumlarını listeleyeceğiz. Bunun kapsamlı bir liste olmadığını unutmayın. Burada listelenmeyen birçok kullanım örneği var. Bu liste, Packer'ın işlemlerinizi nasıl geliştirebileceği hakkında size bir fikir vermek amacıyla verilmiştir.

### Sürekli Dağıtım (Continuous Delivery)

Packer hafif, taşınabilir ve komut satırı tabanlıdır. Chef/Puppet'e yapılan her değişiklikte birden fazla platform için yeni makine imajları oluşturmak için kullanılabilir. Bu sebeple, sürekli dağıtım hattınızın merkezine koymanız için mükemmel bir araçtır. 

Bu hattın bir parçası olarak, yeni oluşturulan imajlar daha sonra başlatılacak, test edilecek ve altyapı değişikliklerinin çalışıp çalışmadığı doğrulanacaktır. Testler geçerliyse, imajın dağıtıldığında çalışacağından emin olabilirsiniz. Bu, altyapı değişiklikleri için yeni bir kararlılık ve test edilebilirlik seviyesine geçminizi sağlar.

### Dev/Prod Denkliği

Packer, geliştirme, test etme ve yayına alma ortamlarınızı olabildiğince benzer tutmaya yardımcı olur. Packer, aynı anda birden fazla platform için imaj üretmek için kullanılabilir. Packer'ı kullanarak tek bir şablondan aynı anda bir AMI ve VMware makinesi üretebilirsiniz. Böylece yayına almak için AWS'yi ve gelişme için VMware'i (belki de Vagrant ile birlikte) için kullanabilirsiniz, 

Bu yeteneği yukarıdaki sürekli dağıtımlı durumuyla eşleştirin ve geliştirme aşamasından, yayına almaya kadar tutarlı çalışma ortamları için oldukça esnek bir sisteme sahip olun.

### Ortam/Demo Oluşturma

Packer, paralel olarak birden fazla platform için tutarlı imajlar oluşturduğundan, ortamlar ve tek kullanımlık ürün demoları oluşturmak için mükemmeldir. Yazılımınız değiştikçe, otomatik olarak yazılımı önceden yüklenmiş ortamlar oluşturabilirsiniz. Potansiyel kullanıcılar daha sonra istedikleri ortamda yazılımınızı kullanmaya başlayabilir.

Yazılımı karmaşık gereksinimlerle birlikte paketlemek hiç bu kadar kolay olmamıştı.

## Packer ve HashiCorp Ekosistemi

HashiCorp, açık kaynak kodlu Vagrant, Packer, Terraform, Serf, Consul, Nomad ve ticari bir ürün olan Atlas'ın yaratıcısıdır. Packer, HashiCorp'un uygulama dağıtımını sürüme geçirilmiş, denetlenebilir, tekrarlanabilir ve işbirliğine dayalı bir süreç haline getirmek için inşa ettiği ekosistemin sadece bir parçasıdır. Modern veri merkezinin nitelikleri ve sorumlu uygulama sunumu ile ilgili inançlarımız hakkında daha fazla bilgi edinmek için Atlas Mindset: Altyapı Sürümü Kontrolü başlıklı makaleyi okuyun.

Makine imajları ve dağıtılabilir eserler oluşturmak için Packer kullanıyorsanız, muhtemelen bu eserler dağıtmak için bir çözüm bulmanız gerekir. Terraform, altyapıyı oluşturmak, birleştirmek ve değiştirmek için kullanılan bir araçtır.

Aşağıda, HashiCorp'un açık kaynak projelerinin özetleri ve Atlas'ın tam bir uygulama dağıtım iş akışı oluşturmak için bunları nasıl bağladığını gösteren bir grafik bulunmaktadır.

### HashiCorp Ekosistemi

![Atlas](https://raw.githubusercontent.com/zekiunal/packer/master/atlas.png)

[Atlas](https://atlas.hashicorp.com/) HashiCorp'un tek ticari ürünüdür. Uygulama sürüm yönetimini, denetlenebilir, tekrarlanabilir ve işbirliğine dayalı yapmak için Packer, Terraform ve Consul'ü birleştirir.

[Packer](https://www.packer.io) makine imajları ve AMI'ler, OpenStack imajları, Docker konteynerleri gibi konuşlandırılabilir çıktılar oluşturmak için kullanılan bir HashiCorp aracıdır.

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

```bash
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

```bash
$ brew install packer
```

#### Chocolatey

Windows ve [Chocolatey](http://chocolatey.org/) kullanıyorsanız, Packer'ı aşağıdaki talimatlarla kurabilirsiniz:

```bash
choco install packer
```

### Sorun Giderme

Bazı RedHat tabanlı Linux dağıtımlarında varsayılan olarak `packer` adlı başka bir araç daha yüklüdür. Bunu `which -a packer` komutu ile kontrol edebilirsiniz. Böyle bir hata alırsanız, bir isim çakışması olduğunu gösterir.

```bash
packer
/usr/share/cracklib/pw_dict.pwd: Permission denied
/usr/share/cracklib/pw_dict: Permission denied
```

Bunu düzeltmek için, `packer`'a , `packer.io` gibi farklı ad kullanan bir kısayol oluşturabilir veya istediğiniz `packer` dosyasının mutlak yolunu kullanabilirsiniz. (ör. `/usr/local/packer`)

## İmaj Oluşturma

Packer'ı kurulduktan sonra derinlere dalıp ve ilk imajımızı oluşturalım. İlk imajımız Redis kurulu bir [Amazon EC2 AMI](https://aws.amazon.com/ec2/) olacak. Bu sadece bir örnek. Packer, önceden yüklenmiş herhangi bir şeyle birçok platform için imaj oluşturabilir.

AWS hesabınız yoksa şimdi [oluşturun.](https://aws.amazon.com/free/) İmajımızı oluşturmak için "t2.micro" örneğini kullanacağız; "t2.micro" örneği, AWS [ücretsiz kullanım sınırlarında](https://aws.amazon.com/free/) yer alıyor, bu da işlemlerimizin ücretsiz olacağı anlamına geliyor. Zaten bir AWS hesabınız varsa, AWS sizden bir miktar para tahsil edilebilir, ancak bu örnek için birkaç sentten daha fazla olmamalıdır.

> **Not:** AWS ücretsiz kullanım sınırları kapsamında yer alan bir hesap kullanmıyorsanız, bu örnekleri çalıştırmak için sizden ücret alınabilir. Ücret sadece birkaç sentten olmalıdır, ancak sonuçta daha fazla ücret ödemek durumunda kalırsanız, biz sorumluluk kabul etmiyoruz.

Packer, AWS dışındaki [pek çok platform](https://www.packer.io/docs/builders/index.html) için imajlar oluşturabilir ancak AWS, bilgisayarınıza ek bir yazılım yüklemeye gerek duymaz ve [ücretsiz kullanım sınırları](https://aws.amazon.com/free/) çoğu insan için uygundur. Bu nedenle bu örnek için AWS'yi kullanmayı seçtik. Bir AWS hesabı kurmaktan rahatsızsanız, diğer platformlar için de geçerli olan temel ilkelerle birlikte denemekten çekinmeyin.

### Şablon

Yapılandırmayı tanımlamak için kullanılan yapılandırma dosyasına Packer terminolojisinde şablon (`template`) denir. Şablonun biçimi basit JSON'dur. JSON, insan ve makine tarafından düzenlemede en iyi dengeye sahiptir; hem elle yapılmış şablonların hem de makinenin ürettiği şablonların kolayca oluşturulmasına izin verir.

Şablonun tamamını oluşturarak başlayacağız, ardından her bir bölümün üzerinde kısaca duracağız. `example.json` dosyası oluşturun ve aşağıdaki içeriği doldurun:

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

Gizli anahtarlarınızı şablondan tanımlamamak için, [kullanıcı değişkenleri](https://www.packer.io/docs/templates/user-variables.html) olarak `aws_access_key` ve `aws_secret_key`'yi parametre olarak geçireceksiniz. [Bu sayfada](https://console.aws.amazon.com/iam/home?#security_credential) kimlik bilgileri oluşturma hakkında bilgi alabilirsiniz. Örnek IAM ilke belgesi [Amazon EC2 dokümanlarında](https://www.packer.io/docs/builders/amazon.html) bulunabilir.

Bu, kullanıma hazır olan temel bir şablondur. Temel bir JSON nesnesi olarak hemen tanınabilir. Nesnede, kurucular (`builders`) bölümü, belirli bir kurucuyu yapılandıran bir dizi JSON nesnesi içerir. Kurucu (`builder`), bir makine yaratmak ve makineyi bir imaja dönüştürmekle yükümlü olan Packer'ın bir bileşenidir.

Bu örnekte, yalnızca `amazon-ebs` türünde tek bir kurucu (`builder`) yapılandırıyoruz. Amazon EC2 AMI kurucusu Packer ile birlikte gelir. Bu kurucu, kaynak bir AMI başlatarak EBS destekli bir AMI oluşturur, bunun üzerine gerekli hazırlıkları yapar ve yeni bir AMI oluşturur.

Nesne içindeki ek tanımlar, erişim sağlayıcı bilgileri, kullanılacak kaynak AMI ve benzeri şeyleri belirten bu kurucu için gerekli yapılandırmadır. Bir kurucu için mevcut yapılandırma değişkenlerinin tam listesi her üreticiye özeldir ve bu [belgede](https://www.packer.io/docs/index.html) bulunabilir.

Bu şablonu kullanmadan ve bir imaj oluşturmadan önce, `packer validate example.json` komutunu çalıştırarak şablonu doğrulalım. Bu komut, doğrulama yapmak için sözdizimini ve yapılandırma değerlerini kontrol eder. Şablonun geçerli olması gerektiği için çıktı aşağıdaki gibi görünmelidir. Herhangi bir hata varsa, bu komut size hatayı bildirecektir.

```bash
$ packer validate example.json
Template validated successfully.
```

Şimdi, bu şablondan imajını oluşturalım.

Zeki bir okuyucu, önceden Redis kurulmuş bir imaj oluşturacağımızı ve buna rağmen yaptığımız şablonun herhangi bir yerinde Redis'e referans verilmediğini söyleyebilir. Aslında dokümantasyonun bu kısmı yalnızca temel hazırlıkları yapılmamış bir imaj oluşturulmasını kapsamaktadır. Hazırlık konusunu ele alan bir sonraki bölümde Redis'in kurulumuna değinilecektir.

### İlk İmaj

Doğrulanmış bir şablonla, ilk imajınızı oluşturmanın zamanı geldi. Bu işlem, `packer build` ile bir şablon dosyasını çağırarak yapılır. Çıktı aşağıda göründüğü gibi olmalıdır. Bu işlemin genellikle birkaç dakika sürdüğünü unutmayın.

> **Not:** Deneme yanılma için, komut satırından kimlik bilgilerini kullanmak daha uygundur. Ancak, potansiyel olarak güvensizdir. Amazon kimlik bilgilerini tanımlamanın diğer yolları için [belgelerimize](https://www.packer.io/docs/builders/amazon.html#specifying-amazon-credentials) bakın.

> **Not:** Windows'ta `packer`'ı kullanırken, aşağıdaki tek tırnakları çift tırnak işaretiyle değiştirin.

```bash
$ packer build \
    -var 'aws_access_key=YOUR ACCESS KEY' \
    -var 'aws_secret_key=YOUR SECRET KEY' \
    example.json
==> amazon-ebs: amazon-ebs output will be in this color.

==> amazon-ebs: Creating temporary keypair for this instance...
==> amazon-ebs: Creating temporary security group for this instance...
==> amazon-ebs: Authorizing SSH access on the temporary security group...
==> amazon-ebs: Launching a source AWS instance...
==> amazon-ebs: Waiting for instance to become ready...
==> amazon-ebs: Connecting to the instance via SSH...
==> amazon-ebs: Stopping the source instance...
==> amazon-ebs: Waiting for the instance to stop...
==> amazon-ebs: Creating the AMI: packer-example 1371856345
==> amazon-ebs: AMI: ami-19601070
==> amazon-ebs: Waiting for AMI to become ready...
==> amazon-ebs: Terminating the source AWS instance...
==> amazon-ebs: Deleting temporary security group...
==> amazon-ebs: Deleting temporary keypair...
==> amazon-ebs: Build finished.

==> Builds finished. The artifacts of successful builds are:
--> amazon-ebs: AMIs were created:

us-east-1: ami-19601070
```

`packer build` işleminin sonunda, Packer, kurulumun bir parçası olarak imajı üretir. Çıktılar (Artifacts), bir kurulumun sonucudur ve genellikle bir kimliği (bir AMI durumunda olduğu gibi) veya bir dizi dosyayı (bir VMware sanal makinesi için olduğu gibi) temsil eder. Bu örnekte yalnızca tek bir çıktıya sahibiz: `us-east-1` te oluşturulan bir AMI.

Bu AMI kullanıma hazırdır. İsterseniz gidip bu AMI'yi hemen başlatabilirsiniz.

> **Not:** AMI kimliğiniz mutlaka yukarıda belirtilenlerden farklı olacaktır. Yukarıdaki örnek çıktıdaki gibi bir başlatmayı denerseniz, bir hata mesajı alırsınız. AMI'nızı başlatmayı denemek isterseniz, AMI kimliğinizi Packer çıktısından alın.

> **Not:** If you see a VPCResourceNotSpecified error, Packer might not be able to determine the default VPC, which the t2 instance types require. This can happen if you created your AWS account before 2013-12-04. You can either change the instance_type to m3.medium, or specify a VPC. Please see http://docs.aws.amazon.com/AmazonVPC/latest/UserGuide/default-vpc.html for more information. If you specify a vpc_id, you will also need to set subnet_id. Unless you modify your subnet's IPv4 public addressing attribute, you will also need to set associate_public_ip_address to true, or set up a VPN.

### İmajı Yönetme

Packer yalnızca imajlar oluşturur. Onları herhangi bir şekilde yönetmeye çalışmaz. Kurulduktan sonra, bunları uygun gördüğünüz gibi başlatmak veya yok etmek size kalmıştır. Kolaylık için imaj adlarını saklamak isterseniz, [HashiCorp Atlas'ı](https://atlas.hashicorp.com/session) kullanabilirsiniz. Bu başlangıç kılavuzunun sonuna imajları uzaktan oluşturmayı ve saklamayı ele alacağız.

Yukarıdaki örneği çalıştırdıktan sonra, AWS hesabınızda bu kurulumla ilişkili bir AMI olacaktır. AMI'ler S3 tarafından Amazon tarafından saklanır, bu nedenle aylık 0.01 Dolar tutarında ücretlendirilmek istemiyorsanız, muhtemelen kaldırmak isteyeceksinizdir. AMI'yi önce [AWS AMI yönetim](https://console.aws.amazon.com/ec2/home?region=us-east-1#s=Images) sayfasından kaydını silerek kaldırın. Sonra, [AWS anlık imaj yönetimi](https://console.aws.amazon.com/ec2/home?region=us-east-1#s=Snapshots) sayfasındaki ilişkili anlık imajını (snapshot) silin.

Tebrik ederiz! İlk imajınızı Packer ile oluşturdunuz. Bu imaj tamamen yararsız da olsa, bu sayfada Packer'ın nasıl çalıştığını, şablonları, şablonların nasıl doğrulanacağı ve oluşturulacağı hakkında genel bir fikir edindiniz.

### Hazılık (Provision)
Bu kılavuzun önceki sayfasında, ilk imajımızı Packer ile oluşturdunuz. Ancak, henüz oluşturduğunuz imaj, temel olarak daha önce var olan bir temel AMI'yi yeniden oluşturuyordu. Packer'ın gerçek amacı, yazılımları imajlara da yükleyip yapılandırmaktır. Bu aşama, hazırlık (provision) adımı olarak da bilinir. Packer, makineleri otomatik olarak imajlara dönüştürmeden önce otomatik hazırlamayı (provision) destekler.

Bu bölümde imajımızı Redis'i kurarak tamamlayacağız. Böylelikle, yarattığımız imaj aslında Redis'i önceden yüklenmiş olacak. Redis küçük, basit bir örnek olsa da, bu, imaja daha fazla paket yüklemenin nasıl bir şey olacağı hakkında fikir verecektir.

Tarihsel olarak, önceden oluşturulmuş imajlar zorluydu, çünkü onları değiştirmek çok sıkıcı ve yavaştı. Hazırlama (Provision) da dahil olmak üzere Packer tamamen otomatik olduğundan, imajlar hızlı bir şekilde değiştirilebilir ve Chef, Puppet gibi modern yapılandırma yönetimi araçlarıyla entegre edilebilir.

### Hazılıkları Yapılandırma

Hazırlayıcılar (Provisions), şablonun bir parçasıdır. Redis'i yüklemek için Packer ile birlikte gelen yerleşik kabuk (shell) sağlayıcıyı kullanacağız. Daha önce yapmış olduğumuz `example.json` şablonunu değiştirin ve aşağıdakileri ekleyin. Yeni yapılandırmanın çeşitli bölümlerini aşağıdaki kod bloğundan sonra açıklayacağız.

```json
{
  "variables": ["..."],
  "builders": ["..."],

  "provisioners": [{
    "type": "shell",
    "inline": [
      "sleep 30",
      "sudo apt-get update",
      "sudo apt-get install -y redis-server"
    ]
  }]
}
```

> **Not:** Yukarıdaki örnekte `sleep 30` çok önemlidir. Packer, SSH kullanılabilir olduğu anda makinayı algılayabilir ve makinaya SSH'ye bağlayabilir. Aslında Ubuntu'nun hazırlanması yeteri miktar zaman almaz. Bu bekletme  işlemi, işletim sisteminin doğru şekilde başlatıldığından emin olur.

Umarım herşey net, ancak kurucular bölümünde aslında "..." bulunmamalıdır, başlangıç ​​kılavuzunun önceki sayfasındaki içerik ayarları olmalıdır. Ayrıca, önceki derste mevcut olmayan `builders` dan sonraki virgülü `[...]` bölümüne de dikkat edin.

Hazırlayıcıları (Provisions) yapılandırmak için, kurucular (builders) yapılandırmasıyla birlikte şablona yeni bir bölüm olan `provisioners`'ı ekledik. `provisioners` bölümünde, çalıştırılacak hazırlıkların dizisi vardır. Birden fazla hazırlık belirlenirse, verilen sıraya göre çalıştırılırlar.

Varsayılan olarak, hazırlıklar tanımlanan her kurucu (builder) için çalıştırılır. Şablonumuzda hem Amazon hem de DigitalOcean gibi iki kurucu tanımlanmış olsaydık, kabuk betiği (shell script) her iki kurulumun bir parçası olarak çalışırdı. Hazırları belirli kurucularla kısıtlamanın yolları da vardır, ancak başlangıç ​​kılavuzunun kapsamı dışındadır. [Dokümantasyonun](https://www.packer.io/docs/index.html) tamamında daha ayrıntılı bir şekilde ele alınmıştır.

Tanımladığımız hazırlayıcı `shell` tipindedir. Bu hazırlayıcı Packer ile birlikte gelir ve çalışan makinede kabuk komut (shell scripts) dosyalarını çalıştırır. Bizim örneğimizde, Redis'i kurmak için çalıştırılacak iki satır  komut yazdık.

### Kurulum (Build)

Hazırlayıcı yapılandırıldıktan sonra, her şeyin yolunda olduğunu doğrulamak için `packer validate` ile bir kez daha kontrol edin, sonra `packer build example.json` ile kurulumu gerçekleştirin. Ekran çıktısı, ilk imajınızı oluşturduğunuz gibi görünmelidir; ek olarak, hazırlık işleminin gerçekleştirileceği yeni bir adım daha olacaktır.

Çıktı, kabuk komut dosyalarının tüm çıktılarını içerdiğinden, bu kılavuzda yer alması çok detaya kaçacaktır. Sonuç olarak Redis'in başarıyla yüklendiğini görmeniz gerekir. Bundan sonra, Packer makineyi tekrar bir AMI'ye çevirir.

Bu AMI'yi başlatacak olursanız, Redis'in önceden yüklenmiş olduğunu göreceksiniz. Etkileyici!

Bu sadece temel bir örnektir. Gerçek bir dünyada kullanım örneğinde, uygulamanızı çalıştırmak için gerekli olan tüm bileşenleri içeren bir imaj hazırlıyor olabilirsiniz. Belki yalnızca bir web ortamı, böylece önceden oluşturulmuş web sunucuları için bir imaj oluşturabilirsiniz. Her şey önceden kurulduğundan, bu imajları başlattığınızda tonlarca tasarruf sağlamış olacaksınız. Ek olarak, her şey önceden yüklendiğinden, imajları oldukları gibi test edebilir ve yayına alındığında herşeyin çalışır durumda olacağını bilebilirsiniz.

### Paralel Kurulumlar

Şu ana kadar, Packer'ın bir imajı nasıl otomatik olarak oluşturacağını gösterdik. Bu özellik kendi başına zaten oldukça güçlüdür. Fakat Packer bundan daha iyisini yapabilir. Packer, tek bir şablondan yapılandırılmış, paralel olarak birden çok platform için birden fazla imaj oluşturabilir.

Bu, Packer'ın çok kullanışlı ve önemli bir özelliğidir. Örnek olarak, Packer, aynı komutlarla sağlanan bir AMI ve bir VMware sanal makinesini paralel olarak hazırlayabilir; sonuç olarak neredeyse aynı imajlar elde edebilir. AMI yayına alma süreçleri için kullanılabilir, VMware makineside geliştirme ortamı için kullanılabilir. Veya başka bir örnek, eğer [yazılım çalışma ortamları](https://en.wikipedia.org/wiki/Software_appliance) (cihazlar) oluşturmak için Packer kullanıyorsanız, her desteklenen platform için tek bir şablondan paralel olarak oluşturabilirsiniz.

Bu özellikten yararlanmaya başladıktan sonra, olasılıklar önünüzde açılmaya başlar.

Bu başlangıç ​​kılavuzundaki örneğe devam edersek, AMI gibi bir DigitalOcean imajı de oluşturacağız. Her ikisi de neredeyse aynı olacak: İskeleti önceden Redis  kurulmuş olan Ubuntu OS. Bununla birlikte, her iki platform için de kurulum yapacağımızdan, AMI'yi veya [DigitalOcean'un](https://www.digitalocean.com/) imajını kullanmak isteyip istemediğinize karar vereceğiz. Ya da her ikisini de kullanabiliriz.

#### DigitalOcean'u Kurma
     
[DigitalOcean](https://www.digitalocean.com/) nispeten yeni ama çok popüler olan bir VPS sağlayıcısıdır. Yüksek performanslı, düşük maliyetli VPS sunucularının kaliteli bir sunumuna sahiptir. Bu örnek için bir DigitalOcean anlık imajını (snapshot) oluşturacağız.

Bunu yapmak için, DigitalOcean ile bir hesaba ihtiyacınız olacak. [Şimdi DigitalOcean'da bir hesap açın.](https://www.digitalocean.com/) Kaydolmak ücretsizdir. "**Droplets**" (sunucular) saatlik ücretlendirildiğinden, Packer ile oluşturduğunuz her resim için sizden 0,01 $ ücret alınır. Bu hoşuna gitmediyse, sadece rehberi okuman da yeterlidir.

> **Uyarı!** "Droplets" çalıştığı için Packer ile yaratılan resim başına DigitalOcean tarafından 0.01 Dolar ücretlendirilirsiniz.

Bir hesaba kaydolduğunuzda, [DigitalOcean API erişim sayfasından](https://cloud.digitalocean.com/settings/applications) API erişim anahtarınızı alın. Bu anahtarı bir yere kaydedin; Birazdan onlara ihtiyacınız olacak.

#### Şablonun Değiştirilmesi

Şimdi, DigitalOcean'ı eklemek için şablonu değiştirmeliyiz. Kullandığımız şablonu değiştirin ve `builders` dizisine aşağıdaki JSON nesnesini ekleyin.

```json
{
  "type": "digitalocean",
  "api_token": "{{user `do_api_token`}}",
  "image": "ubuntu-14-04-x64",
  "region": "nyc3",
  "size": "512mb",
  "ssh_username": "root"
}
```

Ayrıca, DigitalOcean için erişim anahtarlarını eklemek için şablonun değişkenler bölümünü de değiştirmeniz gerekir.

```json
"variables": {
  "do_api_token": "",
  // ...
}
```

Şablonun tamamı şu şekilde görünmelidir:

```json
{
  "variables": {
    "aws_access_key": "",
    "aws_secret_key": "",
    "do_api_token": ""
  },
  "builders": [{
    "type": "amazon-ebs",
    "access_key": "{{user `aws_access_key`}}",
    "secret_key": "{{user `aws_secret_key`}}",
    "region": "us-east-1",
    "source_ami": "ami-fce3c696",
    "instance_type": "t2.micro",
    "ssh_username": "ubuntu",
    "ami_name": "packer-example {{timestamp}}"
  },{
    "type": "digitalocean",
    "api_token": "{{user `do_api_token`}}",
    "image": "ubuntu-14-04-x64",
    "region": "nyc3",
    "size": "512mb",
    "ssh_username": "root"
  }],
  "provisioners": [{
    "type": "shell",
    "inline": [
      "sleep 30",
      "sudo apt-get update",
      "sudo apt-get install -y redis-server"
    ]
  }]
}
```

Ek kurucular (builders) basitçe şablondaki `builders` dizisine eklenir. Bu, Packer'a birden fazla imaj oluşturmasını söyler. Kurucuların tiplerinin farklı olması gerekmez! Aslında, birden fazla AMI oluşturmak istiyorsanız, her kurulum için benzersiz bir ad berlirtmeniz yeterlidir.

`packer validate` ile şablonu doğrulayın. Bu her zaman faydalı bir yöntemdir.

> **Not:** Daha fazla DigitalOcean yapılandırma seçeneği arıyorsanız, [DigitalOcean Builder](https://www.packer.io/docs/builders/digitalocean.html) sayfasında bulabilirsiniz. Belgeler, mevcut yapılandırma seçeneklerinin  listesini içeren bir kılavuzudur.

#### Kurulum

Şimdi `packer build` komutunu kullanıcı değişkenlerinizle çalıştırın. Bir kısmı aşağıda listenilenen çıktı çok ayrıntılı olacaktır. Satırların sıralaması ve bazı ifadelerin farklı olabileceğini, ancak sonucun aynı olduğunu unutmayın.

```bash
$ packer build \
    -var 'aws_access_key=YOUR ACCESS KEY' \
    -var 'aws_secret_key=YOUR SECRET KEY' \
    -var 'do_api_token=YOUR API TOKEN' \
    example.json
==> amazon-ebs: amazon-ebs output will be in this color.
==> digitalocean: digitalocean output will be in this color.

==> digitalocean: Creating temporary ssh key for droplet...
==> amazon-ebs: Creating temporary keypair for this instance...
==> amazon-ebs: Creating temporary security group for this instance...
==> digitalocean: Creating droplet...
==> amazon-ebs: Authorizing SSH access on the temporary security group...
==> amazon-ebs: Launching a source AWS instance...
==> digitalocean: Waiting for droplet to become active...
==> amazon-ebs: Waiting for instance to become ready...
==> digitalocean: Connecting to the droplet via SSH...
==> amazon-ebs: Connecting to the instance via SSH...
...
==> Builds finished. The artifacts of successful builds are:
--> amazon-ebs: AMIs were created:

us-east-1: ami-376d1d5e
--> digitalocean: A snapshot was created: packer-1371870364
```

Gördüğünüz gibi Packer hem Amazon hem de DigitalOcean imajlarını paralel olarak oluşturuyor. Her biri farklı renklerle bazı bilgiler verir (yukarıdaki blokta göremediğiniz halde), böylece komutu çalıştırdığınızda gerçekleştirilen eylemlerin tanımlanması daha kolaydır.

Kurulumların sonunda, Packer yaratılmış imajları (bir AMI ve bir DigitalOcean anlık imajı) listeler. Yaratılan her iki imajda da önceden Redis kurulmuş temel Ubuntu kurulumlarıdır.

### Vagrant Nesneleri (Box)
Packer, bir kurucunun (AMI veya düz VMware imajı gibi) çıktılarını alıp bir Vagrant Nesnesine (Vagrant Box) dönüştürme yeteneğine de sahiptir.

Bu operasyon, [Ön tanımlı işlemler(post-processors)](https://www.packer.io/docs/templates/post-processors.html) kullanılarak yapılır. Bunlar, daha önce çalışan kurucunun (builder) veya Ön tanımlı işlemlerin (post-processors) oluşturduğu bir çıktıyı (artifact) alır ve yeni bir çıktıya (artifact) dönüştürür. Vagrant `post-processor`, bir kurucudan bir çıktı alır ve bir Vagrant nesnesine dönüştürür.

Ön tanımlı işlemler(post-processors) genellikle çok yararlı bir kavramdır. Ön tanımlı işlemlerin (post-processors) çok ilginç kullanım örnekleri vardır. Örneğin, imajı sıkıştırmak, yüklemek, test etmek vb. için bir ön tanımlı işlem (post-processor) yazabilirsiniz.

AWS AMI'yi, [`vagrant-aws eklentisi`](https://github.com/mitchellh/vagrant-aws) ile kullanılabilen bir Vagrant nesnesine dönüştürmek amacıyla Vagrant post-processors eklemek için şablonumuzu değiştirelim. Bir önceki sayfada ve DigitalOcean'u takip ettiyseniz, DigitalOcean içinde benzer bir ihtiyaç duyabilirsiniz, ancak şuan da Packer DigitalOcean için Vagrant nesneleri oluşturamaz. Yakında bu da mümkün olacak.

#### Ön Tanımlı İşlemlerin Etkinleştirilmesi

Ön Tanımlı İşlemler, bir şablonun `post-processor` bölümüne eklenir. `example.json` şablonunuzu değiştirin ve aşağıdaki bölümü ekleyin. Şablonunuz şöyle görünmelidir:

```json
{
  "builders": ["..."],
  "provisioners": ["..."],
  "post-processors": ["vagrant"]
}
```


Bu örnekte, "vagrant" olarak adlandırılan tek bir Ön Tanımlı İşlemi (post-processor) etkinleştiriyoruz. Bu `post-processor` Packer da var olan bir özelik olup Vagrant nesneleri yaratacaktır. Bununla birlikte, her zaman [yeni `post-processor`ler](https://www.packer.io/docs/extending/custom-post-processors.html) de tanımlayabilirsiniz. `post-processor`  yapılandırmasıyla ilgili ayrıntılar, [Ön tanımlı işlemler belgelerinde](https://www.packer.io/docs/templates/post-processors.html) bulunmaktadır.

`packer validate` ile şablonu doğrulayın.

#### Ön Tanımlı İşlemleri Kullanma

`packer build` komutunu çalıştırmanız yeterlidir ve artık ön tanımlı işlem `post-processors` devreye girecektir. Packer DigitalOcean için bir Vagrant nesnesi yapamadığından, sadece AMI'yı oluşturması için ` -only=amazon-ebs` parametresini `packer build` komutuna geçirmenizi öneririm. Komut aşağıdaki gibi görünmelidir:

```
$ packer build -only=amazon-ebs example.json
```

Çıktı, imajı listedikten sonra, bir Vagrant nesnesinin (varsayılan olarak, bulunduğu dizindeki packer_aws.box dosyası) oluşuturulduğunu fark edeceksiniz. Başardık!

Ancak, Amazon EBS kurucusunun (builder) çıktısı nereye gitti? `post-processors` kullanırken, ara üretimler (artifacts) genellikle istenmedikleri için Vagrant tarafından kaldırılır. Sadece son çıktı (artifact) korunur. Elbette bu davranış değiştirilebilir. Bu davranışın nasıl kontrol edildiği bu [dokümantasyonda](https://www.packer.io/docs/templates/post-processors.html) açıklanmıştır.

Ara çıktıyı (artifacts) kaldırılırken, çıktı (builder) alttaki dosyaları veya kaynakları da kaldırılır. Örneğin, bir VMware imajı oluştururken, bir Vagrant nesnesine çevirirseniz, VMware imajının dosyaları Vagrant nesnesine sıkıştırıldıklarından silinir. Bununla birlikte, AWS imajları oluşturmakla birlikte, AMI, Vagrant'ın çalışması için gerekli olduğundan saklanır.

### Uzaktan Kurulumlar ve Depolama
Rehberde buraya kadar, AWS ve DigitalOcean'da imajları kurmak ve hazırlamak için yerel makinenizde Packer çalıştırdık. Bunun yanında, Packer'ın uzaktan çalıştırılmasını sağlamak için Atlas'ı HashiCorp kullanabilir ve kurulumların çıktısını saklayabilirsiniz.

#### Neden Uzaktan Kurulum?

Uzaktan kurulum yaparsanız, erişim kimlik bilgilerini geliştirici ortamından çıkarabilir, yerel makineleri uzun süren Packer süreçlerinden kurtarabilir ve Packer kurulumlarını, `vagrant push`, sürüm kontrol sistemi veya CI aracı gibi tetikleyici araçlarla otomatik olarak başlatabilirsiniz.

#### Packer'ı Uzaktan Çalıştırma

Packer'ı uzaktan çalıştırmak için Packer şablonunda yapılması gereken iki değişiklik vardır. Birincisi Packer şablonunu Atlas'a gönderen pusher konfigürasyonunun eklenmesidir, böylece Packer'ı uzaktan çalıştırabilir. İkinci değişiklik de, yerel ortam yerine Atlas ortamından değişkenleri okumak için değişkenler bölümünü güncellemektir. Eğer şablonunuzda tanımlıysa, `post-processors` bölümünü şimdilik kaldırın.

```json
{
  "variables": {
    "aws_access_key": "{{env `aws_access_key`}}",
    "aws_secret_key": "{{env `aws_secret_key`}}"
  },
  "builders": [{
    "type": "amazon-ebs",
    "access_key": "{{user `aws_access_key`}}",
    "secret_key": "{{user `aws_secret_key`}}",
    "region": "us-east-1",
    "source_ami": "ami-9eaa1cf6",
    "instance_type": "t2.micro",
    "ssh_username": "ubuntu",
    "ami_name": "packer-example {{timestamp}}"
  }],
  "provisioners": [{
    "type": "shell",
    "inline": [
      "sleep 30",
      "sudo apt-get update",
      "sudo apt-get install -y redis-server"
    ]
  }],
  "push": {
    "name": "ATLAS_USERNAME/packer-tutorial"
  }
}
```

Atlas kullanıcı adı almak için [bir hesap oluşturun.](https://atlas.hashicorp.com/account/new?utm_source=oss&utm_medium=getting-started&utm_campaign=packer&_ga=2.220098398.658837797.1501750338-1968525260.1501750338) Bir hesabınızı oluşturduktan sonra, deneme hesabı alabilmek için, sales@hashicorp.com ile iletişime geçmeniz gerekecektir.

Yukarıdaki yapılandırmada "ATLAS_USERNAME" yerine kullanıcı adınızı yazın. [https://atlas.hashicorp.com/settings/tokens](https://atlas.hashicorp.com/settings/tokens) adresine gidin ve bir Atlas erişim anahtarı oluşturun. Bu anahtarı bir ortam değişkeni olarak atayın : `ATLAS_TOKEN=YOURTOKENHERE`.

Ardından, Atlas'a kurulumu göndermek için kurulumu otomatik olarak başlatan `packer push example.json` komutunu çalıştırın.

Atlas ortamında ne `aws_access_key` ne de `aws_secret_key` tanımlanmadığından bu kurulum başarısız olacaktır. Atlas'da ortam değişkenlerini ayarlamak için [**Builds**](https://atlas.hashicorp.com/builds) sekmesine gidin, yeni oluşturulan "packer-tutorial" kurulum yapılandırmasını tıklatıp sol gezinme bölmesinde 'variables' bağlantısına tıklayın. `aws_access_key` ve `aws_secret_key`'yi kendi değerlerinizle ayarlayın. Şimdi, Packer kurulumunu Atlas ara yüzündeki 'rebuild' u tıklatarak veya `packer push example.json` komutunu yeniden çalıştırarak başlatın. Aktif kuruluma (active build) tıkladığınızda, işlem kayıtlarını gerçek zamanlı olarak görüntüleyebilirsiniz.

> **Not:** Packer şablonunda bir değişiklik yapıldığında Atlas'da yapılandırmayı güncellemek için `packer push` komutunu çalıştırmalısınız.

#### Packer Kurulumlarını Depolama

Artık Atlas, önceden Redis yapılandırılmış bir AMI oluşturabiliyor. Bu harika, ancak AMI çıktısını saklamak ve sürümlemek daha da harikadır, böylece [Terraform](https://www.terraform.io/) gibi bir araç tarafından da kolayca konuşlandırılabilir. [Atlas `post-processor`](https://www.packer.io/docs/post-processors/atlas.html) bu işlemi daha kolay hale getirir:

```
{
  "variables": ["..."],
  "builders": ["..."],
  "provisioners": ["..."],
  "push": ["..."],
  "post-processors": [{
    "type": "atlas",
    "artifact": "ATLAS_USERNAME/packer-tutorial",
    "artifact_type": "amazon.image"
  }]
}
```

`post-processors` bloğunu Atlas kullanıcı adınızla güncelleyin, daha sonra `packer push example.json` komutunu çalıştırın ve kurulumun Atlas'da başladığını gözlemleyin! Kurulum tamamlandığında, ortaya çıkan çıktı (artifact ) Atlas'a kaydedilir ve saklanır.

### Sonraki Adımlar

Bu Packer için başlangıç kılavuzuydu. Şimdi, temel Packer kullanımıyla ilgili daha rahat hissediyor olmalısınız. Şablonları anlıyor, kurucuları, sağlayıcıları vb. bileşenleri tanımlayabiliyorsunuz. Bu noktada Packer'ı gerçek senaryolarda düşünmeye ve kullanmaya hazırsınız.

Bu noktadan sonra sizin için en önemli referans [dokümantasyon](https://www.packer.io/docs/index.html) olacaktır. Dokümantasyon, Packer'ın tüm genel özelliklerine ve yeteneklerine ilişkin güçlü bir referans kaynağıdır.

Packer'ın HashiCorp ekosistemi araçlarıyla nasıl çalıştığına ilişkin daha fazla bilgi edinmek istiyorsanız, [Atlas'a genel bir bakış](https://atlas.hashicorp.com/help/intro/getting-started) rehberini okuyun.

Packer'ı kullanırken, lütfen yorumlarınızı ve kaygılarınızı [elektronik posta veya IRC](https://www.packer.io/community.html) kanalı ile paylaşın. Ayrıca Packer [açık kaynaktır](https://github.com/hashicorp/packer), isterseniz lütfen katkıda bulunun. Katkılara çok açığız.
