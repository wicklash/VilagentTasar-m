# 📊 Ajan Orkestrasyonu: Tool-as-Agent Mimarisi

Bu diyagram, ana ajanın (Orchestrator) alt ajanları birer **"Ajan Modülü"** (Tool-as-Agent) olarak nasıl yönettiğini ve her birinin neden izole bir odada çalıştığını gösterir.

```mermaid
sequenceDiagram
    autonumber
    participant Main as Ana Ajan (Orkestratör)
    participant Bridge as Ajan Köprüsü (Tool-as-Agent)
    participant Sub as Uzman Modül (Subagent)

    Note over Main: Kullanıcıdan kapsamlı bir talep gelir.
    
    Main->>Bridge: Ajan_Modülü_Çağır(hedef="Veriyi analiz et", uzmanlık="araştırmacı")
    
    rect rgb(220, 235, 255)
        Note over Bridge: KONTEKST İZOLASYONU (Seçici Kopyalama)
        Bridge->>Bridge: Sistem Yetkilerini Aktar
        Bridge->>Bridge: Mesaj Geçmişini Temizle (Flush)
        Bridge->>Bridge: Sadece Mevcut Hedefi Hafızaya Yaz
    end

    Bridge->>Sub: Modülü Kendi Sürecinde Başlat (Otonom Run)

    rect rgb(240, 240, 240)
        Note over Sub: BAĞLAMDAN İZOLE ÇALIŞMA
        loop Akıl Yürütme Döngüsü
            Sub->>Sub: Kendi Araçlarını Kullan
            Note right of Sub: Bu karmaşa ana state'e bulaşmaz
        end
    end
    
    Sub-->>Bridge: Sentezlenmiş Sonuç (Rafine Bilgi)
    
    rect rgb(230, 255, 230)
        Note over Bridge, Main: SONUÇ ENJEKSİYONU
        Bridge->>Main: Modülden Gelen Final Raporu
        Main->>Main: Bilgiyi Ana Hafızaya Ekle (Merge State)
    end

    Main-->>User: "İstediğiniz analizler uzman modüller tarafından tamamlandı."
```

### 🧠 Neler Değişti? (Tool-as-Agent Mantığı)
1.  **Fonksiyon İsimleri:** `task(...)` yerine `Ajan_Modülü_Çağır(...)` ifadesini kullandık.
2.  **Parametre İsimleri:** `description` yerine `hedef`, `type` yerine `uzmanlık` terimlerini kullanarak aracın ne işe yaradığını netleştirdik.
3.  **İzolasyon Vurgusu:** Ana ajanın geçmişini sildiği ("Flush") ve alt ajanın kendi başına "Akıl Yürütme Döngüsü"ne girdiği kısımları teknik terimlerden arındırıp daha anlaşılır kıldık.
