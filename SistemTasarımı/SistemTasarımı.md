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
