# C.5.1. Veri Tabanı İsterler Dokümanı

Tasarlanan veritabanı yapısının temel amacı, tamamlanmış görevlerin tekrar kullanılabilir bilgi birimlerine dönüştürülmesini sağlamaktır.

Bu kapsamda sistemin, her görev kaydı için benzersiz bir kimlik, görev açıklaması, görev özeti, ilgili uygulama adı, iş akışı adımları, sonuç durumu, oluşturulma zamanı, etiket bilgileri ve embedding vektörünü saklayabilmesi gerekmektedir.

Veritabanı, yeni bir görev geldiğinde semantik benzerlik araması yaparak en ilgili geçmiş görevleri geri döndürebilmeli; ayrıca uygulama adı, görev tipi, başarı durumu veya etiket gibi metadata alanları üzerinden filtreleme desteklemelidir.

Sisteme yalnızca tamamlanmış ve doğrulanmış görevler eklenmeli, hassas veri içeren alanlar veritabanına yazılmadan önce maskelenmeli veya özetlenmelidir.

Görev belleğine eklenen kayıtlar daha sonra güncellenebilir, arşivlenebilir veya gerektiğinde silinebilir olmalıdır.

Ayrıca kullanılan embedding modeline bağlı olarak tüm vektör kayıtlarının aynı boyutta tutulması, eksik veya bozuk metadata ile kayıt oluşturulmaması ve aynı görevin gereksiz tekrarlarla belleğe eklenmesinin engellenmesi temel isterler arasında yer almaktadır.

Böylece veritabanı yalnızca bir depolama alanı değil, ajan davranışını iyileştiren kontrollü bir deneyim belleği olarak işlev görecektir.
