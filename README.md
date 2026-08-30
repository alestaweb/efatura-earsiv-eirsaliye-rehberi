# e-Fatura, e-Arşiv ve e-İrsaliye: Yazılım Tarafı Rehberi

Türkiye'de satış yapan bir yazılımın elektronik belge süreçlerini doğru kurabilmesi için
gereken kavramların ve karar noktalarının dökümü. Amaç, bir e-ticaret veya sipariş
sisteminin "faturayı hangi anda, hangi tipte, hangi bilgiyle keseceğini" tarif edebilmek.

Piyasadaki çoğu anlatım "e-Faturaya nasıl geçilir" sorusunu cevaplıyor. Bu rehber onun
sonrasını anlatıyor: **geçtiniz, peki yazılımınızın içinde ne değişiyor.**

> **Uyarı:** Bilgilendirme amaçlıdır; mali veya hukuki tavsiye değildir. Mevzuat değişir,
> hadler her yıl güncellenir. Uygulamadan önce Kaynaklar bölümündeki resmî metinlerden
> teyit edin veya mali müşavirinize danışın.

---

## İçindekiler

1. [Terim sözlüğü](#1-terim-sözlüğü)
2. [Hangi belge ne zaman: karar ağacı](#2-hangi-belge-ne-zaman-karar-ağacı)
3. [Zorunluluk kapsamı ve mevzuat zinciri](#3-zorunluluk-kapsamı-ve-mevzuat-zinciri)
4. [Üç entegrasyon yöntemi](#4-üç-entegrasyon-yöntemi)
5. [Sipariş akışında faturanın yeri](#5-sipariş-akışında-faturanın-yeri)
6. [En sık atlanan adım: alıcı mükellefiyet kontrolü](#6-en-sık-atlanan-adım-alıcı-mükellefiyet-kontrolü)
7. [Fatura senaryoları: TEMEL ve TİCARİ farkı](#7-fatura-senaryoları-temel-ve-ticari-farkı)
8. [İptal, itiraz ve iade](#8-iptal-itiraz-ve-iade)
9. [e-İrsaliye ve sevkiyat](#9-e-i̇rsaliye-ve-sevkiyat)
10. [Saklama ve ibraz](#10-saklama-ve-i̇braz)
11. [Sık yapılan hatalar](#11-sık-yapılan-hatalar)
12. [Değerlendirme kontrol listesi](#12-değerlendirme-kontrol-listesi)
13. [Kaynaklar](#13-kaynaklar)

---

## 1. Terim sözlüğü

Bu başlıklar birbirinin yerine kullanılıyor ve karışıklığın çoğu buradan çıkıyor.

| Terim | Ne demek |
|---|---|
| **e-Fatura** | Sistemde **kayıtlı iki mükellef arasında** düzenlenen elektronik fatura. Gelir İdaresi Başkanlığı'nın belirlediği format ve standartta, kapalı devre bir sistem üzerinden iletilir. |
| **e-Arşiv Fatura** | Alıcı e-Fatura sistemine kayıtlı **değilse** düzenlenen elektronik fatura. Nihai tüketiciye kesilen fatura bu gruba girer. Alıcıya e-posta/bağlantı ile iletilir, rapor olarak İdare'ye bildirilir. |
| **e-İrsaliye** | Sevk irsaliyesinin elektronik hâli. Malın fiziksel hareketini belgeler; fatura ile karıştırılmamalı. |
| **e-SMM** | Serbest meslek makbuzunun elektronik hâli (danışman, avukat, doktor vb.). |
| **e-MM** | Müstahsil makbuzunun elektronik hâli (defter tutmayan çiftçiden alım). |
| **e-Defter** | Yevmiye ve büyük defterin elektronik olarak tutulup berat ile onaylanması. Fatura uygulamalarından ayrı bir yükümlülüktür. |
| **UBL-TR** | e-Faturanın XML tabanlı veri formatı. Uluslararası UBL standardının Türkiye uyarlaması. |
| **ETTN** | Her elektronik belgeye verilen evrensel tekil numara. Belgenin kimliğidir; fatura numarasından ayrıdır. |
| **Mali mühür** | Tüzel kişilerin elektronik belgeyi imzalamak için kullandığı, Kamu Sertifikasyon Merkezi'nden alınan donanım sertifikası. Şahıs işletmeleri nitelikli elektronik imza (e-imza) da kullanabilir. |
| **Özel entegratör** | İdare'den izin almış, mükellef adına belge düzenleme/iletme hizmeti veren kuruluş. |
| **Senaryo** | Faturanın hangi kurallara tabi olduğunu belirleyen tip bilgisi (TEMEL, TİCARİ, İHRACAT vb.). |
| **Portal** | İdare'nin sağladığı, belgelerin elle girildiği web arayüzü. |

**En kritik ayrım:** e-Fatura ile e-Arşiv arasındaki seçim sizin tercihiniz değil,
**alıcının durumu** tarafından belirlenir. Yazılım bu kararı her satışta yeniden vermek
zorundadır.

---

## 2. Hangi belge ne zaman: karar ağacı

Bir satış gerçekleştiğinde sıra şudur:

1. **Satıcı e-Fatura kapsamında mı?**
   Değilse kâğıt fatura veya kapsamına göre e-Arşiv söz konusu olur.
   Kapsamdaysa devam.

2. **Alıcının vergi kimliği var mı?**
   Kurumsal alıcı için VKN, bireysel alıcı için TCKN alınır. Bireysel satışta bu bilgi
   her zaman zorunlu değildir; tutar ve belge tipine göre değişir.

3. **Alıcı e-Fatura sistemine kayıtlı mı?**
   Bu, İdare'nin yayımladığı **kayıtlı kullanıcı listesinden** sorgulanır.
   - **Kayıtlıysa → e-Fatura.** Kâğıt veya e-Arşiv kesilemez.
   - **Kayıtlı değilse → e-Arşiv Fatura.**

4. **Mal fiziksel olarak sevk ediliyor mu?**
   Ediliyorsa sevk irsaliyesi/e-İrsaliye ayrıca gerekir. Fatura irsaliye yerine geçmez;
   yerine geçtiği özel durumlar mevzuatta tanımlıdır.

5. **Satış yurt dışına mı?**
   İhracat faturaları ayrı senaryoya tabidir ve gümrük süreciyle bağlantılıdır.

Bu ağacın 3. adımı, yazılım tarafında **en çok hataya yol açan** yerdir. Ayrıntısı
6. bölümde.

---

## 3. Zorunluluk kapsamı ve mevzuat zinciri

Elektronik belge uygulamalarının çerçevesi **509 Sıra No'lu Vergi Usul Kanunu Genel
Tebliği** ile çizilmiştir. Bu tebliğ zaman içinde 515, 526, 535, 550, 573 ve 589 sıra
numaralı tebliğlerle değiştirilmiştir. Yürürlükteki metin, İdare'nin e-Belge mevzuat
sayfasında "değişiklikleri işlenmiş hâli" olarak yayımlanır.

Kapsam üç ayrı eksende belirlenir:

- **Ciro eşiği** — belirli bir hesap dönemindeki brüt satış hasılatı eşiği aşan mükellefler.
- **Faaliyet konusu** — internet üzerinden satış, aracı hizmet sağlayıcılığı, gayrimenkul
  ve motorlu taşıt ticareti gibi belirli faaliyetler için **daha düşük** bir eşik uygulanır.
  E-ticaret yapan bir işletmenin kapsama girme ihtimali, aynı cirodaki bir perakendeciden
  yüksektir.
- **Sektörel zorunluluk** — bazı sektörler ciroya bakılmaksızın kapsama alınmıştır.

**Bu rehber rakam yazmıyor.** Hadler ve geçiş tarihleri neredeyse her yıl değişiyor;
sabitlenen bir sayı rehberi birkaç ay içinde yanlış hâle getirir. Güncel eşik için
509 numaralı tebliğin yürürlükteki hâline ve İdare duyurularına bakın.

**Yazılım açısından çıkarım:** kapsam bir kere kontrol edilip geçilecek bir şey değil.
Bir müşteri bu yıl kapsam dışıyken gelecek yıl kapsama girebilir. Sisteminizde
"bu işletme hangi belge tiplerini kesebilir" bilgisi, kod içine gömülü değil,
**ayarlanabilir** olmalıdır.

---

## 4. Üç entegrasyon yöntemi

İdare üç yol tanımlıyor:

| | **Portal** | **Özel entegrasyon** | **Doğrudan entegrasyon** |
|---|---|---|---|
| Nasıl çalışır | Belgeler İdare'nin web arayüzünden elle girilir | İzinli bir kuruluşun altyapısı üzerinden | İşletme kendi sistemini doğrudan bağlar |
| Yazılım geliştirme | Yok | Sağlayıcının arayüzüne göre orta | Yüksek |
| Hacim uygunluğu | Düşük hacim | Her ölçek | Yüksek hacim |
| Bakım yükü | Yok, ama otomasyon da yok | Format/mevzuat güncellemesi sağlayıcıda | Tamamen sizde |
| Tipik e-ticaret tercihi | Uygun değil (elle giriş ölçeklenmez) | **En yaygın** | Büyük hacim / özel gereksinim |

Sipariş hacmi düzenli olan bir e-ticaret sistemi için pratikte tercih özel entegrasyondur.
Portal, günde birkaç fatura kesen işletmeler dışında sürdürülebilir değildir: her siparişin
elle girilmesi hem hata üretir hem de sipariş sayısıyla birlikte doğrusal iş yükü yaratır.

**Doğrudan entegrasyon seçilirse ne devralınıyor:** format sürüm geçişleri, şema
doğrulama, sertifika yenileme, zarf durum takibi, İdare tarafındaki kesinti senaryoları.
Bunlar bir kere yazılıp bırakılan işler değil, süreklilik gerektiren bir bakım kalemidir.

---

## 5. Sipariş akışında faturanın yeri

Bir e-ticaret siparişinde faturayla ilgili duraklar:

1. **Sepet** — fiyat, KDV oranı ve indirim matrahı burada belirlenir. Faturadaki tutar
   bu hesabın birebir aynısı olmalıdır; sonradan yuvarlama farkı çıkarsa uyuşmazlık olur.
2. **Alıcı tipi seçimi** — bireysel mi kurumsal mı. Kurumsalsa VKN ve vergi dairesi,
   bireyselse ad-soyad ve gerekiyorsa TCKN.
3. **Fatura adresi** — teslimat adresinden ayrı tutulmalı. Aynı sanılıp tek alan
   kullanılan sistemler, kurumsal alıcıda merkez adresi ile şube teslimatını ayıramaz.
4. **Ödeme onayı** — faturanın hangi olayla tetikleneceği karar noktasıdır. Ödeme anı mı,
   sevkiyat anı mı? İkisi arasında gün farkı varsa bu fark belgede görünür.
5. **Mükellefiyet sorgusu** — e-Fatura mı e-Arşiv mi (bkz. 6. bölüm).
6. **Belge oluşturma** — sıra numarası, tarih, para birimi, KDV ayrıştırması, kalem
   bazında açıklama.
7. **İmzalama ve iletim** — mali mühür/e-imza ile imzalanır, kanala verilir.
8. **Durum takibi** — belge kabul edildi mi, reddedildi mi, hata mı döndü. Bu adımı
   izlemeyen sistemler, faturasız kalan siparişleri günler sonra fark eder.
9. **Müşteriye iletim** — e-Arşiv tarafında alıcıya erişilebilir bir kopya sunulması.
10. **Muhasebeye aktarım** — kayıtların mali müşavir tarafında görünür olması.

**Sık atlanan nokta:** 4. ve 8. adımlar arasında geçen sürede sipariş iptal edilebilir.
Fatura kesildikten sonraki iptal, faturasız iptalden tamamen farklı bir süreçtir
(bkz. 8. bölüm). Akış tasarlanırken bu ihtimal baştan hesaba katılmalıdır.

---

## 6. En sık atlanan adım: alıcı mükellefiyet kontrolü

Alıcının e-Fatura sistemine kayıtlı olup olmadığı, İdare'nin yayımladığı kayıtlı kullanıcı
listesinden belirlenir. Yanlış tipte belge kesmek düzeltilebilir bir şey değildir;
belgeyi iptal edip yeniden düzenlemeyi gerektirir.

Yazılım tarafında dikkat edilecekler:

- **Liste değişkendir.** Yeni mükellefler eklenir, kapsamdan çıkanlar olur. Bir defa
  indirip sabitlemek, zamanla yanlış belge kesmek demektir. Düzenli tazelenmeli.
- **Sorgu anı önemlidir.** Sipariş anında kayıtlı olmayan bir alıcı, fatura kesim anında
  kayıtlı hâle gelmiş olabilir. Belirleyici olan **fatura düzenleme anıdır.**
- **Kimlik doğrulaması ayrı bir iştir.** VKN/TCKN'nin biçimsel olarak geçerli olması,
  o kimliğe ait bir mükellefiyet olduğu anlamına gelmez. Biçim doğrulaması ile varlık
  doğrulaması iki ayrı kontroldür.
- **Kesinti hâlinde ne olacak?** Sorgu yapılamıyorsa sipariş bekletilecek mi, yoksa
  varsayılan bir tiple mi ilerlenecek? Bu kararın önceden verilmiş olması gerekir;
  akış anında verilirse yanlış verilir.
- **Kayıt tutun.** Hangi sorgunun hangi tarihte hangi sonucu verdiği saklanmalı. Sonradan
  çıkan bir uyuşmazlıkta "o gün liste böyleydi" demenin tek yolu budur.

---

## 7. Fatura senaryoları: TEMEL ve TİCARİ farkı

e-Faturada belge, bir senaryo tipiyle düzenlenir. En çok karşılaşılan ikisi:

**Temel fatura**
Alıcı, sistem üzerinden bir kabul/red yanıtı göndermez. Belge iletildiği anda süreç
tamamlanmış sayılır. Alıcının itirazı varsa bunu sistem dışı yollarla (noter, taahhütlü
mektup, kayıtlı elektronik posta gibi) yapması gerekir.

**Ticari fatura**
Alıcı, belgeyi sistem üzerinden **kabul veya reddedebilir.** Reddedilen fatura,
düzenleyen tarafta karşılığı olan bir işlem gerektirir. Bu yanıt için tanınmış bir süre
vardır; süre ve usul yürürlükteki mevzuattan teyit edilmelidir.

Diğer senaryolar ihracat, yolcu beraberi eşya, kamu alımları gibi özel durumlar içindir.

**Yazılım açısından farkın anlamı:** ticari fatura kesen bir sistem, **yanıt bekleyen bir
durum makinesine** ihtiyaç duyar. "Fatura kesildi" bitmiş bir durum değildir; kabul,
red veya süre aşımı sonuçlarını izlemek gerekir. Yalnızca temel fatura kesileceği
varsayımıyla yazılmış bir akış, ilk kurumsal müşteride tıkanır.

---

## 8. İptal, itiraz ve iade

E-ticaretin en çok karıştığı bölüm burasıdır, çünkü **üç ayrı kavram** birbirine karışır:

| Durum | Ne olur |
|---|---|
| **İptal** | Belge hiç düzenlenmemiş sayılacak şekilde geçersiz kılınır. Hangi belge tipinde, hangi süre içinde, hangi usulle yapılabileceği mevzuatla sınırlıdır. |
| **İtiraz/red** | Alıcı belgeye karşı çıkar. Ticari faturada sistem içinden, temel faturada sistem dışı yollarla yapılır. İdare'nin bildirim ekranları üzerinden bildirilmesi gereken durumlar vardır. |
| **İade** | Mal geri gelir. Bu bir muhasebe işlemidir; alıcı mükellefse **alıcı iade faturası keser**, mükellef değilse iade belgesi düzenlenir. Orijinal fatura ortadan kalkmaz. |

Cayma hakkı kapsamındaki bir iade, faturanın iptali değildir. Tüketici malı iade
ettiğinde satış gerçekleşmiş ve sonra geri alınmıştır; belge akışı bunu yansıtmalıdır.

**Kısmi iade**, sistemlerin çoğunun hazırlıksız yakalandığı durumdur: üç kalemli bir
siparişin tek kalemi iade edilir, kargo bedeli iade edilmez, indirim tüm sepete
dağıtılmıştı. İade edilecek matrahın nasıl hesaplanacağı baştan tanımlı değilse,
her vaka elle çözülür ve hata üretir.

**Tasarım kuralı:** faturayı sipariş kaydının bir alanı olarak değil, **kendi durumu olan
ayrı bir kayıt** olarak tutun. Bir siparişin birden fazla belgesi olabilir: asıl fatura,
iade faturası, kısmi iade, düzeltme. Tek alanlı model bunların hiçbirini taşıyamaz.

---

## 9. e-İrsaliye ve sevkiyat

e-İrsaliye, sevk irsaliyesinin elektronik karşılığıdır ve **faturadan bağımsızdır.**
Malın hareketini belgelendirir; fatura ise satışı.

Yazılım tarafında dikkat edilecekler:

- **Zaman sırası:** irsaliye sevk anında düzenlenir. Fatura sonradan kesiliyorsa iki belge
  arasındaki bağ kurulabilmelidir.
- **Kısmi sevkiyat:** bir sipariş iki kolide, iki farklı günde çıkabilir. Her sevkiyatın
  kendi belgesi olur; tek irsaliye varsayımı bu senaryoda kırılır.
- **Kargo entegrasyonu ayrı bir konudur.** Kargo firmasının barkodu ve takip numarası
  ticari bir izleme aracıdır, mali belge değildir. İkisinin karıştırılması, "kargo
  fişi irsaliye yerine geçer mi" sorusuna yol açar; geçmez.
- **İade sevkiyatı** da bir mal hareketidir ve kendi belge gereksinimi vardır.

Kapsam ve zorunluluk e-Faturadan ayrı belirlenir; e-Fatura mükellefi olmak otomatik olarak
e-İrsaliye mükellefi olmak demek değildir.

---

## 10. Saklama ve ibraz

Elektronik belgeler, mevzuatta belirlenen süre boyunca **özgün hâlleriyle** saklanmalı ve
istendiğinde ibraz edilebilmelidir. Yazılım açısından bunun anlamı:

- **Ekrandaki görüntü belge değildir.** Saklanması gereken, imzalı özgün veridir; onu
  ekranda gösteren HTML çıktısı değil. Veritabanındaki alanlardan belgeyi yeniden
  üretmek, saklama yükümlülüğünü karşılamaz.
- **Yedekleme bir saklama planı değildir.** Yedeğin geri dönebildiği, dönen belgenin
  bütünlüğünün korunduğu ve ilgili sürenin sonuna kadar erişilebilir kalacağı
  gösterilebilmelidir.
- **Erişim süresi.** Beş yıl önceki bir belgeyi makul sürede bulup sunabiliyor musunuz?
  Arşiv büyüdükçe sorgulanamaz hâle gelen sistemler yaygındır.
- **Sağlayıcı değişikliği.** Özel entegratör değiştirirseniz eski belgelere erişim ne
  olacak? Bu, sözleşme yapılırken sorulması gereken sorudur; ayrılırken değil.

---

## 11. Sık yapılan hatalar

| Hata | Sonucu |
|---|---|
| Mükellefiyet listesini bir kere indirip sabitlemek | Zamanla yanlış tipte belge kesilir |
| Fatura adresi ile teslimat adresini tek alanda tutmak | Kurumsal alıcıda merkez/şube ayrımı kaybolur |
| Faturayı siparişin bir alanı olarak modellemek | İade, kısmi iade ve düzeltme belgeleri taşınamaz |
| Belge durum takibini yapmamak | Reddedilen veya hata alan faturalar günlerce fark edilmez |
| Sepet toplamı ile fatura tutarını ayrı hesaplamak | Yuvarlama farkı uyuşmazlığa dönüşür |
| Yalnızca temel fatura senaryosuna göre yazmak | İlk ticari fatura talebinde akış tıkanır |
| Kargo takip numarasını irsaliye yerine saymak | Mal hareketi belgesiz kalır |
| Sertifika son kullanma tarihini izlememek | Belirli bir günde tüm belge kesimi durur |
| Ciro eşiğini koda gömmek | Had değiştiğinde kod değişikliği gerekir |
| İade matrahının nasıl hesaplanacağını tanımlamamak | Her kısmi iade elle çözülür |
| Test ortamı ile canlı ortamı ayırmamak | Test belgeleri gerçek ortama karışır |
| Belgeyi yalnızca ekran görüntüsü olarak saklamak | İbraz yükümlülüğü karşılanmaz |

---

## 12. Değerlendirme kontrol listesi

Bir yazılımın veya sağlayıcının bu süreçleri karşılayıp karşılamadığını ölçmek için:

**Belge tipi ve yönlendirme**
1. Alıcının e-Fatura mükellefiyeti otomatik sorgulanıyor mu?
2. Mükellef listesi hangi sıklıkla tazeleniyor?
3. Sorgu yapılamadığında akış ne yapıyor — bekletiyor mu, varsayıyor mu?
4. Ticari fatura senaryosu destekleniyor mu, yanıt durumu izleniyor mu?
5. İhracat gibi özel senaryolar kapsamda mı?

**Sipariş entegrasyonu**
6. Fatura hangi olayla tetikleniyor, bu ayarlanabilir mi?
7. Fatura adresi teslimat adresinden ayrı tutuluyor mu?
8. Sepet toplamı ile belge tutarı aynı kaynaktan mı hesaplanıyor?
9. Kalem bazında KDV oranı farklı olabiliyor mu?
10. Para birimi ve kur bilgisi belgeye doğru yansıyor mu?

**İade ve düzeltme**
11. Kısmi iade destekleniyor mu?
12. İade matrahı indirim ve kargo bedeli dağıtımıyla birlikte doğru hesaplanıyor mu?
13. Bir siparişe birden fazla belge bağlanabiliyor mu?
14. İptal ve itiraz süreçleri sistem içinden yürütülebiliyor mu?

**Operasyon**
15. Belge durumları (iletildi, kabul, red, hata) panelde görünüyor mu?
16. Hata alan belgeler için uyarı mekanizması var mı?
17. Sertifika geçerlilik tarihi izleniyor ve önceden uyarılıyor mu?
18. Test ortamı canlıdan ayrı mı?
19. Geçmiş belgeler tarih/tutar/alıcı ile aranabiliyor mu?
20. Sağlayıcı değişikliğinde geçmiş belgelere erişim nasıl sağlanıyor?

Bu listedeki maddelerin çoğuna "sonra bakarız" cevabı verilebilir. Verilemeyecek olanlar
1, 6, 11 ve 15'tir: bunlar sonradan eklendiğinde veri modelinin değişmesini gerektirir.

---

## 13. Kaynaklar

Yalnızca resmî kaynaklar. Rakamlar ve tarihler için tek geçerli referans bunlardır.

- **Gelir İdaresi Başkanlığı e-Belge portalı** — https://ebelge.gib.gov.tr
- **e-Fatura mevzuatı (tebliğler, değişiklikleri işlenmiş hâlleriyle)** — https://ebelge.gib.gov.tr/efaturamevzuat.html
- **Gelir İdaresi Başkanlığı** — https://www.gib.gov.tr
- **Mevzuat Bilgi Sistemi** — https://www.mevzuat.gov.tr
- **Ticaret Bakanlığı e-Ticaret Bilgi Sistemi (ETBİS)** — https://www.eticaret.gov.tr
- **Kamu Sertifikasyon Merkezi (mali mühür)** — https://kamusm.bkk.com.tr

---

## Katkı

Düzeltme ve ekleme önerileri açıktır. Yalnızca şu koşulla: **her bilgi resmî bir kaynağa
dayanmalı.** Sağlayıcı blog yazısı, forum gönderisi veya "bize öyle söylediler" kaynak
sayılmaz. Rakam eklerken yürürlük tarihini de belirtin.

Ticari tanıtım, sağlayıcı karşılaştırması ve fiyat listesi kabul edilmez.

---

Bu rehber, Türkiye'de haber ve e-ticaret yazılımları geliştiren
[Alesta WEB](https://alestaweb.com) tarafından, sahada tekrar tekrar karşılaşılan
sorulardan derlenmiştir.

MIT lisansı ile yayımlanmıştır.
