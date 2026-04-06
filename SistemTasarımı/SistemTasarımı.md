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

VILAGENT projesinin sistem tasarımı, karmaşık kullanıcı taleplerini işletim sistemi seviyesinde anlamlı eylemlere dönüştürebilen, modüler ve hibrit bir mimari üzerine inşa edilmiştir. Süreç, kullanıcının doğal dil formundaki isteğinin derinlemesine analiz edilerek niyet (*intent*) ve parametrelerin ayıklandığı **görev alımı** aşamasıyla başlar. Sistemin algılama katmanı, ekranın piksel bazlı görsel düzenini semantik olarak yorumlayan **Vision Language Model (VLM)** ile uygulamaların erişilebilirlik ağacındaki teknik kimlikleri (buton etiketleri, AutomationID'ler, pencere hiyerarşileri) yakalayan **Windows UI Automation (UIA)** bileşenlerinin füzyonuna dayanır. Bu iki kanalın ürettiği zengin bağlamsal veri, merkezi planlayıcı olan **DeepAgent**'a iletilir; DeepAgent, gelen karmaşık talebi yönetilebilir atomik alt görevlere parçalar ve her bir alt görev için en uygun stratejiyi belirler. Planlama aşamasında sistem, **uzun süreli bellek (Long-Term Memory)** modülü aracılığıyla vektör veri tabanında semantik benzerlik araması gerçekleştirerek geçmişteki başarılı görev akışlarını sorgular; eşleşen deneyimler bulunduğunda mevcut planı bu örüntülerle optimize eder ve tekrarlayan senaryolarda önemli ölçüde hız ile doğruluk kazanır.

Sistemin tüm karar mekanizması ve akış yönetimi, **LangGraph** framework'ü kullanılarak döngüsel (*cyclic*) bir çizge yapısı şeklinde modellenmiştir. Geleneksel sıralı (*sequential*) pipeline'lardan farklı olarak LangGraph, ajanın her adımda mevcut durumu yeniden değerlendirmesine olanak tanıyan **dinamik bir durum makinesi (state machine)** gibi çalışır: her düğüm (*node*) bir işlemi (algılama, planlama, yürütme veya doğrulama) temsil ederken, düğümler arasındaki kenarlar (*edges*) koşullu geçiş mantıklarıyla yönetilir. Böylece ajan, bir adımda başarısızlık algıladığında çizge üzerinde geriye dönerek alternatif bir rotayı izleyebilir ya da yeni bir planlama döngüsüne girebilir. LangGraph'ın sağladığı bu esneklik sayesinde statik iş akışlarına hapsolmak yerine, ortam değişikliklerine anlık tepki verebilen reaktif ve uyarlanabilir bir orkestrasyon elde edilir. Hazırlanan plan doğrultusunda alt görevler (dosya işlemleri, uygulama etkileşimleri, sistem yapılandırmaları) ilgili alanda uzmanlaşmış **SubAgent**'lara devredilir ve bu ajanlar, **Model Context Protocol (MCP)** üzerinden tanımlanmış araç setini tetikleyerek simüle edilmiş fare/klavye girdileri veya doğrudan Windows API çağrıları aracılığıyla aksiyonu fiziksel düzlemde yürütür.

Operasyonel sağlamlığı (*robustness*) en üst düzeyde tutmak için sistem, her işlem sonrasında **öz-yansıma (self-reflection)** mekanizmasını devreye sokar: yürütülen aksiyonun ardından güncel ekran durumu yeniden analiz edilir ve beklenen çıktıyla karşılaştırılır. Eğer bir sapma tespit edilirse (örneğin hedef pencere açılmadıysa veya yanlış bir UI elemanı tıklandıysa) ajan, LangGraph çizgesindeki döngüsel yapıyı kullanarak hatanın kök nedenini teşhis eder ve alternatif bir strateji geliştirerek süreci yeniden dener. Bu iteratif düzeltme döngüsü, tek seferlik başarıya bağımlı olmayan, kendini onaran (*self-healing*) bir yürütme modeli sunar. Tüm alt adımlar başarıyla tamamlandığında planlayıcı, son ekran görüntüsünü ve sistem çıktılarını bütünsel olarak denetleyerek kullanıcı isteğinin eksiksiz karşılandığını teyit eder. Son olarak, başarıyla sonuçlanan görev akışı **Checkpointer** mekanizması aracılığıyla vektör veri tabanına kalıcı bir deneyim kaydı olarak arşivlenir; böylece sistem, her tamamlanan görevle birlikte bilgi birikimini genişleterek gelecekteki benzer taleplerde daha hızlı, daha isabetli ve daha az hata ile yanıt verebilir hale gelir.
