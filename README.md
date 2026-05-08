# C-LOPAI V0.5

**C-LOPAI**, OpenClaw üzerinde çalışan, kendi VPS ortamında barındırılan ve LLM tabanlı çalışan kişisel bir yapay zekâ komuta sistemidir. Temel amacı; günlük yaşam, planlama, hatırlatmalar, alışkanlık takibi, kişisel destek ve sistem yönetimi gibi farklı alanlardan sorumlu alt ajanları tek bir merkezden koordine etmektir.

Bu proje basit bir fikirle başladı: Her şeyi tek bir genel amaçlı chatbot’a yaptırmak yerine, her biri net bir role sahip, kendi bağlamı ve hafızası olan, gerektiğinde merkezi komutanla kontrollü biçimde bilgi paylaşabilen modüler bir kişisel yapay zekâ ağı kurmak. C-LOPAI bu ağın merkezindeki komutan ajan olarak tasarlandı.

**V0.5** sürümü, sistemin temel mimarisini oluşturur. Bu sürümde odak noktası; ajan orkestrasyonu, ortak bağlam paylaşımı, arka plan kontrolleri, Telegram üzerinden etkileşim ve alt ajanlar arasında ilk köprü yapısının kurulmasıdır.

---

## C-LOPAI Ne Yapar?

C-LOPAI yalnızca mesajlara cevap veren bir sohbet asistanı değildir. Daha çok, hafif ama genişleyebilir bir kişisel yapay zekâ komuta merkezi olarak düşünülmüştür.

Bu aşamada sistem şunları yapabilir:

- birden fazla uzmanlaşmış ajanı koordine eder;
- alt ajanlardan gelen durum raporlarını okuyabilir;
- merkezi sistem durumunu takip eder;
- ajan ağı için ortak bir context bridge dosyasını günceller;
- OpenClaw cron görevleriyle zamanlanmış arka plan kontrolleri çalıştırabilir;
- Telegram üzerinden kullanıcıyla iletişim kurar;
- günlük hafıza ve operasyon notları tutar;
- ileride yeni ajanların eklenebileceği modüler bir mimari sunar.

Uzun vadeli hedef, C-LOPAI’yi yalnızca reaktif cevap veren bir asistandan çıkarıp, kullanıcının durumunu takip eden, gerektiğinde sessiz kalabilen, gerektiğinde yönlendirme yapan daha proaktif bir kişisel işletim katmanına dönüştürmektir.

---

## Ajan Ağı

C-LOPAI, çok ajanlı sistemin merkezi komutanıdır. Her alt ajan kendi uzmanlık alanından sorumludur ve kendi bağlamını ana sisteme raporlayarak genel resmi besler.

### C-LOPAI — Komutan Ajan

C-LOPAI ana ajandır. Genel durumu okur, diğer ajanlardan gelen sinyalleri yorumlar, ortak dosyaları günceller ve sistemin sessiz kalması mı, kullanıcıyı uyarması mı, yoksa belirli bir alana dikkat yönlendirmesi mi gerektiğine karar verir.

Öncelikler, sistem durumu, ajan direktifleri ve genel koordinasyon C-LOPAI’nin sorumluluğundadır.

### Fixer — Genel Yardımcı Ajan

Fixer; araştırma, teknik görevler, dosya işlemleri, sistem yönetimi, komut çalıştırma ve genel yardım gibi pratik işleri üstlenir. Sistemin teknik ve operasyonel yardımcı koludur.

### Minder — Hatırlatıcı ve Zamanlama Ajanı

Minder; hatırlatmalar, deadline’lar, takvim benzeri görevler ve tekrar eden bildirimlerden sorumludur. Sistemin zamanı takip eden ve önemli tarihleri kaçırmamasını sağlayan parçasıdır.

### Forcer — Koçluk Ajanı

Forcer; motivasyon, disiplin, odak, yeniden başlama ve düşük enerjili günlerde destek sağlamak için tasarlanmıştır. Farklı koçluk tonlarıyla çalışabilecek şekilde kurgulanmıştır.

### Shaper — Sağlık ve Alışkanlık Takip Ajanı

Shaper; sigara kullanımı, yemek kayıtları ve günlük alışkanlık takibi gibi alanlara odaklanır. İlerleyen sürümlerde kalori tahmini, detaylı raporlar, fiziksel kapasite takibi ve giyilebilir cihaz entegrasyonu gibi işlevlerle genişletilmesi hedeflenmektedir.

### Healer — Psikolojik Destek Ajanı

Healer, ayrı servis yapısına sahip destek odaklı bir ajandır. Diğer OpenClaw ajanlarıyla aynı mesaj gönderme yolu üzerinden kontrol edilmez. Bunun yerine kendi raporları ve özetleri aracılığıyla C-LOPAI’ye daha üst düzey psikolojik bağlam sağlar.

---

## Context Bridge: Ajanlar Arası Köprü

Sistemin en önemli parçalarından biri ajanlar arasındaki **context bridge** yapısıdır.

Her alt ajan, kendi alanındaki durumu özetleyen bir `CONTEXT_REPORT.md` dosyası tutabilir. Bu raporda genel olarak şunlar yer alır:

- kullanıcının o ajan alanındaki güncel durumu;
- tespit edilen riskler;
- değerlendirilebilecek fırsatlar;
- başka bir ajandan istenen destek;
- C-LOPAI’ye yükseltilmesi gereken kararsız konular;
- acil dikkat gerektiren bayraklar.

C-LOPAI, zamanlanmış kontroller sırasında bu raporları okur ve merkezi `CONTEXT_BRIDGE.md` dosyasını günceller. Bu dosya, ajanlar arasındaki komuta katmanı gibi çalışır. C-LOPAI bu köprü üzerinden sisteme şunu söylemiş olur:

> “Genel durum bu. Her ajanın şu an özellikle dikkat etmesi gereken noktalar şunlar.”

Bu yapı sayesinde sistem yalnızca birbirinden bağımsız botlardan oluşmaz. Ajanlar, merkezi bir bağlam üzerinden koordineli hareket etmeye başlar.

---

## Temel Dosyalar ve Sistem Durumu

C-LOPAI, kimliğini, kurallarını, hafızasını ve sistem durumunu okunabilir dosyalar üzerinden yönetir. Bu yaklaşım sistemi daha şeffaf ve gerektiğinde müdahale edilebilir hale getirir.

Önemli workspace dosyaları şunlardır:

- `IDENTITY.md` — C-LOPAI’nin kimliğini tanımlar;
- `SOUL.md` — tonunu, karakterini ve karar alma tarzını belirler;
- `AGENTS.md` — ajan ağını ve temel görevleri tanımlar;
- `USER.md` — kullanıcıyla ilgili üst düzey bağlamı tutar;
- `TOOLS.md` — kullanılabilir araçları ve kullanım notlarını içerir;
- `STATE.md` — güncel sistem durumunu saklar;
- `CONTEXT_BRIDGE.md` — ajan ağına yönelik güncel komutan direktiflerini tutar;
- `MEMORY.md` — uzun vadeli hafıza notlarını içerir;
- `memory/YYYY-MM-DD.md` — günlük hafıza kayıtlarıdır;
- `memory/decisions.md` — önemli kararları ve sistem düzeyi notları saklar.

Bu yapı sayesinde sistemin davranışı tamamen kapalı bir kutu içinde kalmaz. Ajanlar, gerektiğinde bu dosyaları okuyup güncelleyerek kendi durumlarını ve kararlarını izlenebilir hale getirir.

---

## Heartbeat ve Zamanlanmış Çalışma

V0.5 ile birlikte C-LOPAI’nin ilk zamanlanmış çalışma modeli kurulmuştur.

Sistem, OpenClaw cron görevleri aracılığıyla C-LOPAI’yi belirli aralıklarla uyandırabilir. Bir heartbeat döngüsünde C-LOPAI şu adımları izleyebilir:

1. kendi mevcut durumunu okur;
2. alt ajanların context raporlarını inceler;
3. risk, acil durum veya öncelik çatışması olup olmadığını değerlendirir;
4. `STATE.md` dosyasını günceller;
5. `CONTEXT_BRIDGE.md` dosyasını günceller;
6. önemli bir durum yoksa kullanıcıyı rahatsız etmeden sessiz kalır.

Bu tasarımda önemli bir ilke vardır: **Sessizlik geçerli bir davranıştır.** C-LOPAI yalnızca rutin bir kontrol yaptığı için kullanıcıya bildirim göndermemelidir. Bildirim, ancak gerçekten anlamlı bir neden varsa yapılmalıdır.

---

## Günü Bitirme Akışı

V0.5 sürümünde ayrıca erken aşama bir **günü bitirme** akışı da bulunmaktadır.

Kullanıcı `gunu bitir` gibi bir komut verdiğinde, C-LOPAI o günü kapatmak için sistem durumunu güncelleyebilir, günlük hafıza notu yazabilir ve ajan ağını daha düşük bildirimli bir gece moduna hazırlayabilir.

Hedeflenen akış şöyledir:

1. `STATE.md` güncellenir;
2. `CONTEXT_BRIDGE.md` güncellenir;
3. `memory/YYYY-MM-DD.md` dosyası yazılır veya güncellenir;
4. dosya güncellemeleri tamamlandıktan sonra kullanıcıya son mesaj gönderilir.

Bu akış hâlâ sistemin erken otomasyon katmanının bir parçasıdır ve ilerleyen sürümlerde daha güvenli, daha tutarlı ve daha düşük maliyetli hale getirilecektir.

---

## V0.5 Mevcut Durumu

C-LOPAI V0.5, sistemin temel mimari kilometre taşıdır.

Bu sürümde tamamlanan başlıca parçalar:

- OpenClaw üzerinde self-hosted ajan altyapısı;
- Telegram tabanlı etkileşim katmanı;
- merkezi C-LOPAI komutan ajanı;
- ilk alt ajan ağı tanımı;
- ortak workspace dosya yapısı;
- `STATE.md` ve `CONTEXT_BRIDGE.md` iş akışı;
- ilk heartbeat / zamanlanmış kontrol tasarımı;
- alt ajanlar için ilk context-report formatı;
- erken aşama günü bitirme akışı;
- haftalık özet ve arşivleme fikrinin temelleri;
- C-LOPAI ile uzman ajanlar arasında context bridge yaklaşımı.

Bu haliyle sistem, kişisel bir komuta katmanı prototipi olarak kullanılabilir durumdadır. Ancak hâlâ aktif geliştirme aşamasındadır.

---

## V0.5 Sonrası Yol Haritası

Bir sonraki aşamada odak, C-LOPAI’yi gerçek kullanımda daha stabil, daha düşük maliyetli ve daha güvenilir hale getirmektir.

Planlanan geliştirmeler:

- daha güvenli ve düşük token kullanımlı heartbeat davranışı;
- izole ve hafif arka plan session’ları;
- daha kaliteli günlük hafıza özetleri;
- daha güçlü alt ajan context raporları;
- geliştirilmiş otomatik günü bitirme akışı;
- ajanlar arasında daha net yükseltme ve öncelik kuralları;
- LLM gerektirmeyen hatırlatmalar için daha deterministik akışlar;
- zamanlanmış arka plan görevleri için daha iyi maliyet kontrolleri;
- daha temiz uzun vadeli hafıza ve arşivleme davranışı.

Uzun vadeli hedef, yalnızca konuşabilen değil, aynı zamanda operasyonel davranabilen bir kişisel yapay zekâ sistemi kurmaktır. C-LOPAI; bağlamı anlayan, uzman ajanları koordine eden, kullanıcının dikkatini koruyan ve günlük yaşam yönetimine gereksiz gürültü yaratmadan destek olan bir yapı olarak gelişmeye devam edecektir.

---

## Sürüm

**Mevcut public sürüm:** `V0.5`

Bu sürüm, C-LOPAI için ilk kararlı mimari kilometre taşını temsil eder: merkezi bir komutan ajan, uzmanlaşmış alt ajanlar, zamanlanmış arka plan mantığı ve ajanlar arası context bridge yapısına sahip çalışan bir çok ajanlı kişisel yapay zekâ komuta sistemi.
