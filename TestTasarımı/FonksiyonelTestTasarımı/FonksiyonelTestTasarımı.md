# C.3.2. Fonksiyonel Test Tasarımı

## C.3.2.1. Birim (Unit) Testleri Tasarımı

- UIA ayrıştırıcı modülünün boş ağaç, eksik özellik, derin hiyerarşi ve büyük hacimli UIA verisi altında doğru çalışması sınanacaktır.
- Gözlem katmanında güven skoru üretimi, fallback tetikleme eşiği ve hedef nesne seçimi birim bazında doğrulanacaktır.
- Eylem komutları için tıklama, yazma, pencere değiştirme ve kısayol yürütme mantığı ayrı ayrı test edilecektir.
- Verify ve Recover modüllerinin karar kuralları, hata sınıflandırması ve yeniden deneme mantığı birim düzeyde sınanacaktır.
- Bellek yönetimi, özetleme, pruning, log maskeleme ve güvenlik kontrolü işlevleri için birim testleri oluşturulacaktır.
- Boş girdi, belirsiz komut, çok uzun görev açıklaması, büyük log kaydı ve yüksek hacimli gözlem verisi gibi sınır durumları ayrıca test edilecektir.

## C.3.2.2. Entegrasyon Testleri Tasarımı

- Electron ana süreç ile React arayüzü arasındaki haberleşme yapısı test edilecektir.
- Kullanıcı arayüzü ile ajan çekirdeği arasındaki görev başlatma, durum güncelleme, onay alma ve sonuç aktarma akışları doğrulanacaktır.
- DeepAgent ile planlama, gözlem, eylem, doğrulama ve toparlama bileşenleri arasındaki veri akışı sınanacaktır.
- LLM sağlayıcısı, prompt orkestrasyonu ve şema tabanlı çıktı işleme katmanlarının birlikte doğru çalışması test edilecektir.
- UIA gözlemi ile VLM fallback mekanizmasının aynı akış içinde doğru şekilde devreye girmesi doğrulanacaktır.
- Eylem icrası sonrası doğrulama ve gerekirse hata toparlama zincirinin uçtan uca entegrasyonu test edilecektir.
- Bellek, loglama, güvenlik politikası ve telemetri bileşenlerinin ajan yürütümü ile tutarlı çalışması değerlendirilecektir.

## C.3.2.3. Sistem Testleri Tasarımı

- 
- Kullanıcının görev girmesi, sistemin plan üretmesi, hedef arayüzü gözlemlemesi, doğru eylemi icra etmesi, sonucu doğrulaması ve çıktı üretmesi bir bütün olarak değerlendirilecektir.
- Standart erişilebilirlik desteği sunan uygulamalar ile daha karmaşık veya legacy arayüzler üzerinde sistem davranışı gözlemlenecektir.
- Beklenmeyen pencere, zaman aşımı, odak kaybı, yanlış ekran ve eksik UIA verisi gibi hata durumları sistem seviyesinde test edilecektir.
- Uzun görev zincirleri, tekrar eden iş akışları ve kullanıcı onayı gerektiren senaryolar da sistem testlerine dahil edilecektir.

## C.3.2.4. Kabul Testleri Tasarımı


- Kullanıcının masaüstü arayüzünden görev oluşturabilmesi, görev ilerlemesini izleyebilmesi ve sonuç özetine erişebilmesi kabul kriterleri arasında yer alacaktır.
- Ajanın doğru hedefe tıklaması, yanlış eylemleri engellemesi ve kritik işlemler öncesinde açık onay istemesi temel kabul kriteri olacaktır.
- Hata durumlarında sistemin güvenli biçimde toparlanması veya anlamlı hata mesajı ile sonlanması beklenecektir.
- Sonuçların insan tarafından okunabilir ve gerektiğinde makine tarafından işlenebilir biçimde sunulması kabul testlerinde doğrulanacaktır.

## C.3.2.5. Kapalı Kutu (Black-box) Testleri Tasarımı

- Boş görev girdisi, belirsiz görev, çok uzun görev açıklaması, çelişkili komut ve eksik kullanıcı onayı gibi durumlarda sistem tepkisi gözlemlenecektir.
- UIA ile erişilebilen, kısmen erişilebilen ve erişilemeyen arayüzler üzerinde sistemin dış davranışı değerlendirilecektir.
- Beklenen çıktı ile fiilî çıktı karşılaştırılarak görev tamamlama başarısı, hata üretme biçimi ve kullanıcıya sunulan geri bildirim incelenecektir.

## C.3.2.6. Açık Kutu (White-box) Testleri Tasarımı

- Durum makinesindeki Observe, Plan, Act, Verify, Recover, Complete ve Fail geçişleri dal kapsamı açısından test edilecektir.
- Retry, timeout, fallback, approval, reject ve safe-stop gibi dallanma noktalarının her biri doğrulanacaktır.
- Hata sınıflandırma mantığı, güvenlik kontrol akışı, log maskeleme dalları ve bellek pruning koşulları iç yapıya göre sınanacaktır.
- Kritik modüllerde satır, karar ve istisna akışı kapsamı artırılarak yazılım kalitesi desteklenecektir.
