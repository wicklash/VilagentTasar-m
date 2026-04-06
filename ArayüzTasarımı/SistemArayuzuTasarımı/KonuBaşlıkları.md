1. Kullanıcı Arayüzü (User Interface - UI)
Bu, insan operatör ile VILAGENT arasındaki etkileşim katmanıdır.

Komut Giriş Paneli: Kullanıcının "Excel'deki verileri oku ve mail at" gibi doğal dil komutlarını girdiği arayüz.

İzleme (Monitoring) Ekranı: Ajanın o an ne düşündüğünü (Thought), neyi gözlemlediğini (Observation) ve hangi işlemi yaptığını (Action) gösteren panel.

Görsel Kanıt (Visual Evidence): Ajanın ekran üzerinde hangi butona odaklandığını kırmızı bir çerçeve ile gösteren "bounding box" katmanı.

2. Programatik Arayüz (Application Interface - API/UIA)
Ajanın Windows işletim sistemiyle "konuştuğu" dildir. VILAGENT bir insan gibi mouse hareket ettirir ama aynı zamanda Windows'un ruhunu da okur.

Windows UI Automation (UIA) Entegrasyonu: Ajanın ekranı sadece bir resim olarak değil, bir nesne ağacı (Tree Structure) olarak görmesini sağlayan arayüzdür.

McpService (Model Context Protocol): Ajanın dosya sistemi, tarayıcı veya özel Windows servislerine erişmek için kullandığı standartlaştırılmış iletişim protokolü.

3. Ajanlar Arası Arayüz (Inter-Agent Interface)
Projenin modüler yapısında (DeepAgent ve SubAgent'lar) bu birimlerin birbiriyle veri alışverişi yaptığı arayüzdür.

State Management: LangGraph üzerinde ajanlar arasında aktarılan "durum" (state) verisi.

Hiyerarşik İletişim: Ana orkestratörün (DeepAgent), görevi alt ajanlara (SubAgent) nasıl delege ettiğini belirleyen protokol.

4. Kaydedici Arayüzü (Steps Recorder Interface)
VILAGENT'ın rakiplerinden ayrılan en önemli özelliği olan "öğrenme" arayüzüdür.

Demonstrasyon Arayüzü: psr.exe (Steps Recorder) benzeri yapı sayesinde, kullanıcının yaptığı işlemleri kaydeden ve bunları VLM'in anlayacağı bir "adım dizisine" dönüştüren katmandır. Bu arayüz, maliyeti düşürmek için operasyonel rutinleri hafızaya (Vector DB) yazar.