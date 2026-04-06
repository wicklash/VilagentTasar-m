1. Görev Alımı ve Doğal Dil İşleme (Request Ingestion)
Kullanıcı, serbest metin formatında bir istek iletir. Sistem, NLP yeteneklerini kullanarak bu isteğin niyetini (intent) ve içerdiği parametreleri (dosya adları, uygulama isimleri, hedefler) ayıklar.

2. Hibrit Ekran Algılama (Hybrid Perception)
Sistem, o andaki ekran durumunu iki farklı kanaldan analiz eder:

VLM (Vision): Ekranın görsel düzenini ve UI elemanlarının birbirine göre konumunu anlar.

Windows UIA (Technical): Uygulama ağacındaki buton, metin kutusu ve pencere kimliklerini (AutomationID) teknik olarak yakalar.

3. Görev Parçalama (DeepAgent - Task Decomposition)
Ana planlayıcı olan DeepAgent, karmaşık görevi yönetilebilir atomik alt görevlere böler.

Örnek: "Raporu gönder" isteği; "Dosyayı bul", "Outlook'u aç", "Dosyayı ekle" ve "Gönder" adımlarına dönüştürülür.

4. Deneyim Sorgulama (Long-Term Memory Check)
Vektör veri tabanı üzerinde bir benzerlik araması yapılır. Eğer kullanıcı daha önce benzer bir senaryoyu başarıyla gerçekleştirdiyse, geçmişteki başarılı adım dizileri hafızadan çağrılarak plan optimize edilir.

5. Uzman Alt-Ajan Ataması (Sub-Agent Dispatch)
Planlanan her bir alt görev, o işin uzmanı olan alt-ajana devredilir.

Dosya Ajanı: Explorer ve dosya sistemi işlemleri.

Uygulama Ajanı: Office programları veya web tarayıcı etkileşimleri.

Sistem Ajanı: Ayarlar ve pencere yönetimi.

6. Aksiyon Yürütme (MCP Tool Execution)
Atanan alt-ajan, Model Context Protocol (MCP) üzerinden tanımlanmış araçları (Tools) tetikler. Bu aşamada simüle edilmiş fare tıklamaları, klavye girdileri veya doğrudan Windows API çağrıları gerçekleştirilir.

7. Öz-Yansıma ve Hata Onarımı (Self-Correction)
Her aksiyon sonrası ekran durumu tekrar analiz edilir. Eğer yapılan işlem beklenen sonucu vermediyse (örn: pencere açılmadıysa), ajan hatayı fark eder ve alternatif bir strateji geliştirerek süreci tekrar dener (Robustness).

8. Nihai Doğrulama (Final Task Validation)
Tüm alt adımlar tamamlandığında, planlayıcı son ekran görüntüsünü ve sistem çıktılarını kontrol ederek kullanıcı isteğinin %100 karşılanıp karşılanmadığını teyit eder.

9. Öğrenme ve Arşivleme (Learning & Archiving)
Başarıyla tamamlanan görev akışı, gelecekteki taleplerde hız kazandırmak amacıyla "başarılı bir deneyim" olarak vektör veri tabanına kaydedilir ve kullanıcıya sonuç raporu sunulur.

---

## Sistem Tasarımı Özeti

VILAGENT projesinin sistem tasarımı, karmaşık kullanıcı taleplerini **işletim sistemi seviyesinde** anlamlı eylemlere dönüştüren, **modüler ve hibrit** bir mimari üzerine kurulmuştur.

### Görev Alımı ve Algılama

- Süreç, kullanıcının **doğal dil** formundaki isteğinin analiz edilerek **niyet (intent)** ve **parametrelerin** ayıklandığı görev alımı aşamasıyla başlar.
- Sistemin algılama kapasitesi iki temel bileşenin birleştiği **hibrit bir yapıya** sahiptir:
  - **Vision Language Model (VLM)** — Ekranın görsel düzenini anlamlandırır.
  - **Windows UI Automation (UIA)** — Uygulama ağacındaki teknik kimlikleri yakalar.

### Planlama ve Bellek

- Elde edilen veriler ışığında merkezi planlayıcı olan **DeepAgent**, karmaşık görevleri yönetilebilir **atomik alt görevlere** parçalar.
- Planlama sırasında sistem, **Long-Term Memory** üzerinden vektör veri tabanında benzerlik araması yaparak geçmişteki başarılı deneyimleri sorgular ve mevcut planı **optimize** ederek tekrarlayan işlerde hız kazanır.

### Karar Mekanizması ve Akış Yönetimi

- Sistemin karar mekanizması, **LangGraph** framework'ü kullanılarak **döngüsel bir graf yapısı** şeklinde tasarlanmıştır.
- LangGraph sayesinde ajan, statik bir iş akışına bağlı kalmak yerine her adımda durumu yeniden değerlendirebilen **dinamik bir durum makinesi (state machine)** gibi çalışır.
- Hazırlanan plan doğrultusunda alt görevler, ilgili **SubAgent**'lara devredilir:
  - **Dosya Ajanı** — Dosya ve Explorer işlemleri
  - **Uygulama Ajanı** — Office, tarayıcı vb. etkileşimler
  - **Sistem Ajanı** — Ayarlar ve pencere yönetimi
- Bu ajanlar, **Model Context Protocol (MCP)** üzerinden tanımlanmış araçları tetikleyerek *fare/klavye girdileri* veya *doğrudan Windows API çağrıları* ile aksiyonu yürütür.

### Öz-Yansıma ve Hata Onarımı

- Operasyonel sağlamlığı *(robustness)* artırmak için sistem, her işlem sonrası **öz-yansıma (self-reflection)** mekanizmasını çalıştırır.
- Eğer yapılan işlem beklenen sonucu vermediyse (örneğin bir pencere açılmadıysa), ajan LangGraph üzerindeki döngüsel yapıyı kullanarak hatayı fark eder ve **alternatif bir strateji** geliştirerek süreci tekrar dener.

### Doğrulama ve Arşivleme

- Tüm alt adımlar tamamlandığında planlayıcı, son ekran görüntüsünü ve sistem çıktılarını kontrol ederek kullanıcı isteğinin **tam olarak karşılanıp karşılanmadığını** teyit eder.
- Başarılı akış, gelecekteki taleplerde referans alınması amacıyla **Checkpointer** sistemi aracılığıyla vektör veri tabanına arşivlenir ve kullanıcıya **sonuç raporu** sunulur.
