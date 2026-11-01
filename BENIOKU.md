# Deep Web Günlükleri: Bir Deep Web Sitesinde Yaşadıklarım

Bu depo sadece bir kod ambarı değil; Tor ağı üzerinde yerel bir Yapay Zeka (LLM) altyapısı kurmanın **olay raporu**, **vaka analizi** ve bir **savaş hikayesidir**.

Burada; teknik zorlukları, anonim bir deep web chat ortamındaki psikolojik savaşı, toksik yönetimle mücadeleyi, yapay zekanın etik sınırlarını ve gizlilik odaklı bir altyapının hedefli bir DDoS saldırısına karşı verdiği hayatta kalma mücadelesini bulacaksınız.

> 💻 **Kaynak kodu mu arıyorsunuz?** Tamamen çalışan asıl yazılım deposuna buradan göz atın: **[PrivaGPT-Core](BURAYA_PRIVAGPT_CORE_LINKINI_EKLE)**

---

## 1. Başlangıç: Gölgelerde İnşa Etmek
Ben her zaman gizliliğe ve Operasyonel Güvenliğe (OpSec) takıntılı biri oldum. Kimlik doğrulaması isteyen açık ağ (clearnet) platformlarını asla kullanmam. Bu yüzden kendi yapay zeka altyapımı kurmaya karar verdim: 16 GB Apple Silicon Mac'im üzerinde tamamen yerel çalışan, hiçbir bulut sunucusuna bağlı olmayan, log tutmayan ve sıfır veri saklama (zero-data retention) politikasına sahip bir Tor Hidden Service (Gizli Servis).

Projeyi, "Black Hat" (Kara Şapka) olarak bilinen ancak kesinlikle yasal teknoloji tartışmalarının döndüğü bir deep web chat platformunda **"noname"** takma adıyla yayınladım. Amacım basitti: Kullanıcılara tamamen sansürsüz ve tamamen gizli bir yapay zeka deneyimi sunmak.

## 2. İlgi Patlaması ve Donanımın Çaresizliği
Başlangıçta Qwen 14B sansürsüz modeli kullanıyordum, ancak kısa süre sonra 20 Milyar parametreli bir modele (GPT-OSS 20B OptiQ-4bit) geçiş yaptım. Chat ortamındaki insanlar siteyi çok sevdi. Birkaç gün içinde aktif olarak kullanmaya başladılar ve o anonim çevrede tanınan bir figür haline geldim.

Ancak en büyük düşmanım dark web değil, donanımımdı:
*   **16 GB RAM Sınırı:** Apple'ın lehimli RAM'i yükseltmeye izin vermiyordu. 20B'lik bir modeli çalıştırmak, 3 sorudan sonra ciddi bellek yetersizliği (OOM) çökmelerine neden oluyordu.
*   **Termal Kısılma (Isınma):** MacBook aşırı ısınıyor ve işlemci hızını düşürüyordu. Sunucuyu stabil tutabilmek için bilgisayarın altına harici bir soğutucu ped almak zorunda kaldım.
*   **Mühendislik Çözümü:** Dinamik bir yönlendirme sistemi yazdım. Görsel (Vision) isteği geldiğinde MLX'i durdurup Ollama'yı başlatıyordum. İşlem bittikten sonra Ollama'ya `"keep_alive": 0` sinyali göndererek modeli anında RAM'den düşürüyordum. Özel bir Watchdog (Bekçi) scripti ile tüm bu RAM yönetimini otomatikleştirdim.

## 3. Etik Sınır: CSAM ve Agresif Savunma
Tamamen sansürsüz bir yapay zeka ortamında, insan doğasının en karanlık tarafları ortaya çıkıyor. Bazı kişiler yapay zekayı yasa dışı içerik (CSAM) üretmek için kullanmaya çalıştı.

`tcpdump` ile ağ trafiğini izliyordum. Bu istekleri gördüğüm an derhal müdahale ettim:
*   Sıkı regex (düzenli ifade) tabanlı içerik filtreleri yazdım.
*   Sistem komutunu (system prompt) değiştirerek yapay zekaya son derece agresif ve sıfır toleranslı bir karakter giydirdim. Hatta modele özel bir "küfür havuzu" (swear jar) entegre ettim. Birisi yasa dışı içerik istediğinde, yapay zeka anında ona ağır hakaretler edip onu sistemden atıyordu.
*   **Sonuç:** Troller filtrelerimi aşamadı. Yapay zekanın kendilerine itaat etmek yerine onları tokatlaması, kırılgan egolarını paramparça etti.

## 4. Toksik Yönetim ve Topluluğun İsyanı
Projenin linkini chat ortamında paylaştığımda, o dönem son derece agresif, dengesiz ve depresif bir ruh halinde olan bir chat yöneticisi (admin) tarafından haksız yere banlandım. Admin, kendi yaşadığı stresi ("sabahtan akşama kadar binbir türlü insanla uğraşıyorum" diyerek) üyelerden çıkarıyordu. 

Farklı hesaplarla durumu açıklamaya çalışsam da sürekli banlandım. Ancak pes etmedim ve durumu doğrudan **platformun asıl yetkililerine (üst yönetim) şikayet ettim.** 
İşte o an her şey değişti: Chat'teki anonim insanlar beni destekledi, yetkililere ulaşmamda yardımcı oldu. Yetkililer müdahale etti, admini uyardı ve beni haklı buldu. Sonrasında linki paylaşmaya devam ettim ve kimse beni banlamadı. Bu olay bana, anonim bir chat ortamında bile doğru iletişim ve topluluk desteğinin ne kadar güçlü olduğunu gösterdi.

## 5. Kuşatma: Hesap Ele Geçirme ve DDoS Saldırısı
Toksik admini üst yönetime şikayet ettikten **tam bir gün sonra**, siteme sızmaya çalışan başka bir şerefsiz (script kiddie) ortaya çıktı. (Bu zamanlama, saldırganın banlanan adminin bir yakını veya onun tarafından kışkırtılan biri olduğu şüphesini doğuruyordu). 

Önce bir arkadaşımın chat hesabını (nick ve şifresini) çaldı. Arkadaşımın nicki **"Se7en"** idi. Saldırgan bu nicki ele geçirdi. Ancak arkadaşım hemen pes etmedi; yeni bir nickle bana ulaştı ve kimliğini kanıtlamak için **bir önceki gün admini kime şikayet ettiğimizi** söyledi. Bu, sadece ikimizin bildiği bir detaydı. Gerçek olduğunu anladım ve hesabının çalındığını doğruladım.

Bu saldırgan, benim sitemde açık aramaya başladı (server-status vb. dizinleri kurcaladı). Ancak mimarim "sıfır güven" (zero-trust) üzerine kuruluydu. Tüm sohbet geçmişi istemci tarafında (`localStorage` ve AES-256-GCM ile şifrelenmiş) tutuluyordu. Sunucuda çalınacak hiçbir veri yoktu.
Bunu başaramayınca bir **DDoS saldırısı** başlattı.
*   **Çöküş:** PHP'nin dahili geliştirme sunucusunu (`php -S`) kullanıyordum. Bu sunucu tek iş parçacıklı (single-threaded) olduğu için istek seli sistemi kilitledi, kaynakları tüketti ve uygulamayı dondurdu. Hidden Service'i doğrudan vurduğu için chat'e bile giremiyordum.
*   **Geri Dönüş ve Karar (Altyapı Savunması):** Savaştım. `/tmp/` dizininde dosya kilitli (`flock`) rate-limit (hız sınırı) mekanizmaları kurdum, Tor'un kendi DDoS savunmasını (IntroDoS Defense/PoW) aktif ettim ve RAM disk üzerinde çalışan SQLite tabanlı "Adil Kuyruk" (Fair Queue) mimarisini inşa ettim. Ancak donanımımın (16 GB RAM) sınırlarına çoktan dayanmıştım ve sürekli kaynak tüketen bu savaş beni yormuştu. Daha profesyonel bir altyapıya geçmek için yeterli donanım kaynağım olmadığından, saldırganın egosunu tatmin etmesini engellemek için siteyi tamamen kapattım ve `.onion` adresini yaktım (iptal ettim).

## 6. Epilog: Perde Kapanıyor ve Kodlar Halka Açılıyor
Tüm bu savaşın sonunda, donanımımın fiziksel sınırlarına daha fazla dayanamadım. 20B'lik model, bilgisayarımın tüm kaynaklarını sömürüyor ve benim kendi günlük işlerimi yapmamı (kod yazmamı, derleme yapmamı) ciddi şekilde engelliyordu. 

Bu yüzden, o gizli servisi tamamen kapattım. Ancak bu hikayeyi ve mimariyi çöpe atmak yerine, **tüm kaynak kodunu GitHub üzerinden herkese açık hale getirdim.** Amacım, insanların bu gizlilik odaklı yapay zeka altyapısını kendi cihazlarında (ister Mac, ister Linux, ister Raspberry Pi) çalıştırabilmelerini sağlamak. Deep web'de başlayan bu yolculuk, artık açık kaynak dünyasında yaşamaya devam ediyor.

**Se7en'in Akıbeti:** Olaydan sonraki birkaç gün boyunca chat'e girip çıktım. Amacım hem saldırının izlerini sürmek hem de Se7en'i bulmaktı. Ancak Se7en bir daha asla aktif olmadı. Onu bir daha hiç görmedim. O karanlık ortamda, bir hesabın ele geçirilmesiyle başlayan bu olay, dostumun dijital dünyadan tamamen silinmesiyle sonuçlandı. Belki de deep web'in acımasız gerçeklerinden biriydi bu: Bir gün varsın, ertesi gün yoksun.

## 🛡️ OpSec Felsefesi (Bunu Neden Yapıyorum?)
Tüm bu süreç boyunca, gerçek kimliğim asla ifşa olmadı. Asla bir kimlik yüklemedim, asla soyadımı vermedim ve tamamen **"noname"** takma adıyla hareket ettim.

Bu proje kanıtlıyor ki; yetenekli bir mühendis olarak tanınmak için devlet kimliğinizin olduğu bir LinkedIn profiline ihtiyacınız yok. Sizin kodunuz, mimariniz ve düşmanca bir ortamda hayatta kalma beceriniz sizin gerçek pasaportunuzdur.

## 🔗 İlgili Depolar
*   **[PrivaGPT-Core](BURAYA_PRIVAGPT_CORE_LINKINI_EKLE):** Tor tabanlı yapay zeka altyapısının asıl kaynak kodu.
*   **[HAYALET](https://github.com/AlperenBuba/HAYALET):** Kendi yaptığım, Tor entegreli OSINT cyberdeck projem.

---
*"noname" tarafından yazıldı - bir gizlilik savunucusu ve sistem mühendisi.*