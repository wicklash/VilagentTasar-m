# C.5. Veri Tabanı Tasarımı

VILAGENT projesinde klasik ilişkisel veritabanı yaklaşımı yerine, sistemin deneyim belleği ve bilgi geri çağırma gereksinimlerini karşılamaya yönelik vektör veritabanı temelli bir yapı benimsenmiştir. Bu tercih, ajanın daha önce tamamlanmış görevleri, benzer iş akışlarını ve görev özetlerini anlamsal benzerliğe göre yeniden kullanabilmesini sağlamak amacıyla yapılmıştır.

Sistem içerisinde tüm görevler otomatik olarak değil, yalnızca doğrulanmış ve manuel olarak uygun görülen tamamlanmış görevler belleğe eklenecektir. Böylece bilgi tabanının kalitesi korunacak, hatalı veya kararsız yürütümler RAG katmanına taşınmayacaktır.

Fiziksel depolama katmanında Qdrant veya Chroma kullanılması planlanmakta olup, her kayıt vektör gömme verisi ile birlikte görev özeti, uygulama bağlamı, iş akışı adımları, etiketler ve zaman damgası gibi açıklayıcı metadata alanlarını içerecektir.

Bu yapı, DeepAgent'in yeni bir görev alındığında önce geçmiş deneyimleri semantik olarak taramasına ve gerekiyorsa planlama sürecini bu bağlam üzerinden başlatmasına olanak sağlayacaktır.
