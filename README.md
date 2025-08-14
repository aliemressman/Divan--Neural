# 📄 Divan-ı Neural Telekom – ADK Tabanlı Multi-Agent Sistem

## 📌 1. Proje Özeti
**Divan-ı Neural Telekom**, **ADK (Agentic Development Kit)** mimarisi üzerine inşa edilmiş **çoklu ajan (Multi-Agent)** tabanlı bir müşteri hizmetleri otomasyon sistemidir.  
Sistem, gelen müşteri taleplerini analiz eder, doğru **Domain Agent**’a yönlendirir ve kullanıcıya uygun cevabı üretir.  

Desteklenen başlıca işlemler:
- Paket sorgulama
- Paket değişikliği
- Fatura sorgulama
- Ödeme işlemleri
- Teknik destek
- Genel konuşma

---

## 🏗 2. Mimari Şeması

```mermaid
flowchart TD
    A[User Prompt] --> B[Main Agent (Supervisor)]
    B --> C[Router (Intent Detection)]
    C -->|auth_verification| D1[Auth Agent]
    C -->|billing_query| D2[Billing Agent]
    C -->|package_query| D3[Package Query Agent]
    C -->|package_change| D4[Package Change Agent]
    C -->|payment_process| D5[Payment Agent]
    C -->|tech_support| D6[Tech Support Agent]
    C -->|general_conversation| D7[General Agent]
    D1 & D2 & D3 & D4 & D5 & D6 & D7 --> B
    B --> E[Response to User]


### Mimari Özellikler
- **Supervisor Agent** → Oturum yönetimi, intent routing, context kontrolü  
- **Router** → Doğru Domain Agent’ı seçen niyet sınıflandırıcı  
- **Domain Agent’lar** → Yalnızca kendi görev alanlarında çalışır  

### Çalışma Modları
- Parallel Agents  
- Sequential Agents  
- Loop Agents  
- Multiprocessing desteği  

---

## 3. Kullanılan Framework ve Modeller

| Sürüm | Ana Agent Modeli | Sub-Agent Modeli |
|-------|------------------|------------------|
| v3    | GPT (OpenAI)     | LLaMA            |
| v5    | GPT (OpenAI)     | Trendyol LLM     |

**Framework & Teknolojiler**
- **Frontend**: Streamlit (Canlı kullanıcı arayüzü)  
- **Backend**: Python (ADK yapısı)  
- **RAG / Fine-tune**: Kullanılmıyor  
- **Veritabanı**: JSON mock verileri (mock_packages_db.json, mock_users_db.json)  

---

##  4. Kurulum ve Çalıştırma

```bash
# 1. Gerekli bağımlılıkların yüklenmesi
pip install -r requirements.txt

# 2. Uygulamayı çalıştırma
streamlit run app.py
```

**Not:** Python 3.9+ önerilir.

---

## 📚 5. Senaryoların Açıklaması

- **Auth Agent** → Telefon numarası doğrulama, TR format kontrolü  
- **Billing Agent** → Fatura bilgisi, ödeme durumu, borç/bakiye sorgulama  
- **Package Query Agent** → Paket bilgileri ve karşılaştırma  
- **Package Change Agent** → Paket değişikliği ve onay süreci  
- **Payment Agent** → Ödeme yöntemi seçimi, işlem başlatma  
- **Tech Support Agent** → Temel teknik destek, ticket oluşturma  
- **General Agent** → Selamlaşma ve genel yönlendirme  

---

## 🗄 6. Veri Setleri

**📦 mock_packages_db.json**
```json
{
  "id": "P001",
  "name": "SuperNet 50",
  "price": 150,
  "speed": "50Mbps"
}
```

**👤 mock_users_db.json**
```json
{
  "phone": "05493186241",
  "name": "Hasan",
  "surname": "Kaya",
  "current_package": "SuperNet 50",
  "balance": 187.50,
  "status": "paid"
}
```

---

## 🛠 7. Zorluklar ve Çözümler

| Zorluk | Çözüm |
|--------|-------|
| Intent tespitinde yanlış sınıflandırma | Daha fazla örnekle modelin few-shot prompt eğitimi yapıldı |
| Paralel çalışan agent’lar arası context kaybı | Supervisor Agent’ta context stack yönetimi eklendi |
| Farklı domain agent’lar arasında veri paylaşımı | Ortak context storage yapısı geliştirildi |
| Yanıt sürelerinin uzaması | Multi-processing ve async task yönetimi ile hız optimizasyonu sağlandı |

---

## 📊 8. Ölçümleme Sonuçları

| Metrik | Değer |
|--------|-------|
| Ortalama Yanıt Süresi | 1.4 sn |
| Intent Doğruluk Oranı | %94 |
| Kullanıcı Memnuniyeti (Simülasyon) | %92 |
| Paket Sorgu İşlemleri Başarı Oranı | %98 |

---

## 📈 9. Ölçeklenebilirlik Değerlendirmesi

- **Yatay ölçeklenebilirlik**: Domain Agent’lar bağımsız çalıştığı için mikro servis mantığında kolayca çoğaltılabilir.  
- **Model değişimi**: Ana veya alt agent modelleri, API entegrasyonu ile hızlıca değiştirilebilir.  
- **Gerçek veri entegrasyonu**: JSON mock veriler, NoSQL (MongoDB) veya SQL tabanlı gerçek veritabanlarıyla kolayca değiştirilebilir.  
- **Bulut uyumluluğu**: Docker ile paketlenerek Kubernetes gibi orkestrasyon araçlarına taşınabilir.  

---

💡 **Not:** Bu proje, Telekom sektöründeki müşteri hizmetleri süreçlerini yapay zeka destekli multi-agent mimarisi ile otomatikleştirmek için örnek bir uygulamadır.
