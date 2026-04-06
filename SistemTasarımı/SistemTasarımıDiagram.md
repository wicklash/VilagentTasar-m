# VILAGENT Sistem Akış Diyagramı

VILAGENT projesinin temel çalışma prensibini ve bileşenler arasındaki etkileşimi gösteren akış diyagramı aşağıdadır.

```mermaid
graph TD
    %% Başlangıç
    Start((Kullanıcı İsteği)) --> NLP[1. Görev Alımı & NLP<br/><i>Niyet ve Parametre Çıkarımı</i>]

    %% Algılama Katmanı
    NLP --> Perception{2. Hibrit Algılama<br/><i>Hybrid Perception</i>}
    subgraph Algılama_Kanalları [Algılama Kanalları]
        direction LR
        VLM[Vision - VLM<br/>Görsel Düzen]
        UIA[Technical - Win UIA<br/>AutomationID & Ağaç]
    end
    Perception --> VLM
    Perception --> UIA

    %% Planlama Katmanı
    VLM & UIA --> Planning[3. Görev Parçalama<br/><i>DeepAgent Planner</i>]
    
    Planning --> MemoryCheck{4. Deneyim Sorgulama<br/><i>LTM Check</i>}
    MemoryCheck -- "Geçmiş Deneyim Var" --> Optimize[Planı Optimize Et]
    MemoryCheck -- "Yeni Senaryo" --> SubTasks[Atomik Alt Görevler]
    Optimize --> SubTasks

    %% Yürütme Katmanı (LangGraph Döngüsü)
    subgraph Execution_Loop [LangGraph Döngüsel Yürütme]
        direction TB
        Dispatch[5. Alt-Ajan Ataması<br/><i>File/App/System Agent</i>]
        Execute[6. Aksiyon Yürütme<br/><i>MCP Tool Execution</i>]
        Reflection{7. Öz-Yansıma<br/><i>Self-Correction</i>}
        
        Dispatch --> Execute
        Execute --> Reflection
        Reflection -- "Başarısız / Hatalı" --> Dispatch
    end
    
    SubTasks --> Execution_Loop

    %% Doğrulama ve Öğrenme
    Reflection -- "Adım Başarılı" --> NextCheck{Tüm Adımlar Bitti mi?}
    NextCheck -- "Hayır" --> Dispatch
    NextCheck -- "Evet" --> Validation[8. Nihai Doğrulama<br/><i>Final Validation</i>]

    Validation --> Archive[9. Öğrenme & Arşivleme<br/><i>Vector DB & Checkpointer</i>]
    Archive --> End((Sonuç Raporu))

    %% Stil Tanımlamaları
    classDef highlight fill:#f9f,stroke:#333,stroke-width:2px;
    classDef loop stroke:#2ecc71,stroke-width:3px,stroke-dasharray: 5 5;
    class Execution_Loop loop;
```

## Süreç Özeti

1.  **Giriş:** Kullanıcı doğal dil ile komut verir.
2.  **Algılama:** Ekran hem görsel (VLM) hem teknik (UIA) olarak taranır.
3.  **Planlama (DeepAgent):** Karmaşık görev, bellekten gelen eski verilerle harmanlanarak küçük parçalara bölünür.
4.  **Döngüsel Yürütme (LangGraph):** Uzman ajanlar (Dosya, Uygulama vb.) MCP üzerinden araçları kullanır. Her adımda hata kontrolü (Self-Reflection) yapılarak gerekirse süreç başa döner.
5.  **Bitiş:** Görev doğrulandıktan sonra başarı hikayesi olarak kaydedilir ve kullanıcıya rapor sunulur.
