flowchart TB
    %% Kullanıcı ve Giriş Noktası
    User((Kullanıcı)) -->|Windows Görevi Verir| TaskQue[(Etkileşim Girişi/Görev)]
    TaskQue --> DeepAgent

    %% Bulut API Katmanı (Beyin)
    subgraph Cloud["Bulut Hizmetleri Katmanı (Cloud LLM/VLM APIs)"]
        direction LR
        LLM_API[("Bulut LLM API<br/>(Planlama, Karar, Kod Üretme)")]
        VLM_API[("Bulut VLM API<br/>(Görüntü İşleme, JSON Çıktı)")]
    end

    %% 1. Orkestra Şefi (Orchestrator)
    subgraph orchestrator["1. DeepAgent (Orchestrator)"]
        direction TB
        DeepAgent["DeepAgent<br/>Görev Ayrıştırma & Orkestrasyon"]
        TaskGraph{"Task Graph<br/>(Ardışık & Paralel Alt Görev Modeli)"}
        DeepAgent <-->|Muhakeme ve Strateji Talepleri| LLM_API
        DeepAgent -->|Görevleri Dağıtır| TaskGraph
    end

    %% Uzun Dönem Hafıza
    subgraph memory_sys["7. Uzun Dönem Bellek (Long-Term Memory)"]
        direction TB
        File_Storage[("Fiziksel .md / .json Dosyaları<br/>Uygulama ve Task bazlı yapı")]
        DeepAgent -.->|Başarılı iş akışlarını Oku / Kaydet| File_Storage
    end

    %% 2. Çalışan Alt Ajanlar (Workers)
    subgraph subagents["2 & 6. Uzman Alt Ajanlar (SubAgents)"]
        direction TB
        UIAgent["UIAgent (Pencere Hiyerarşisi / Kontroller)"]
        VisionAgent["VisionAgent (Görsel Algı)"]
        AppAgent["AppAgent (Uygulama / Process Yönetimi)"]
        FileAgent["FileAgent (Dosya Sistemi / Yönetim)"]
        WebAgent["WebAgent (Playwright Tarayıcı Gezintisi)"]
        
        %% Güvenlik Modülleri
        ValidationAgent["ValidationAgent (İşlem Sonucu Doğrulama)"]
        RecoveryAgent["RecoveryAgent (Hata Yönetimi ve Rollback)"]
    end

    %% 3. Araçlar ve Yazılım Kitleri (Tools / Middleware)
    subgraph tools["3. Tools & Middleware"]
        direction TB
        UIA_Tool["Windows UIA Tool<br/>(Birincil Görüş, Çok Hızlı)"]
        Screen_Tool["Screenshot Tool<br/>(İkincil Görüş, Yavaş)"]
        App_Tool["OS Command Executer<br/>(Process Start/Kill)"]
        File_Tool["Python File OS Middleware"]
        Playwright_Tool["Playwright Engine"]
    end

    %% Task Flow
    TaskGraph -->|Rol ve Görev Ataması| subagents

    %% Agent -> Tool Bağlantıları
    UIAgent --> UIA_Tool
    VisionAgent --> Screen_Tool
    AppAgent --> App_Tool
    FileAgent --> File_Tool
    WebAgent --> Playwright_Tool

    %% VLM Bağlantısı
    Screen_Tool -->|Görüntü Gönder| VLM_API
    VLM_API -->|Parse Edilebilir JSON Döndür| VisionAgent

    %% Doğrulama ve Güvenlik (Safety Loop)
    subagents -->|Her aksiyonda| ValidationAgent
    ValidationAgent -.->|Durumu Check Et| UIA_Tool
    ValidationAgent -->|Başarısız/Hatalı Aksiyon| RecoveryAgent
    RecoveryAgent -->|Geri Alma / Onarım İsteği| TaskGraph

    classDef cls_cloud bg:#fff3e0,stroke:#e65100,stroke-width:2px;
    classDef cls_agent bg:#e3f2fd,stroke:#1565c0,stroke-width:2px;
    classDef cls_tool bg:#f3e5f5,stroke:#6a1b9a,stroke-width:2px;
    classDef cls_mem bg:#e8f5e9,stroke:#2e7d32,stroke-width:2px;

    class Cloud,LLM_API,VLM_API cls_cloud;
    class DeepAgent,TaskGraph,subagents cls_agent;
    class tools cls_tool;
    class memory_sys,File_Storage cls_mem;
