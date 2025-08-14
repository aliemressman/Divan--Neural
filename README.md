# 📄 Divan-ı Neural Telekom – ADK Tabanlı Multi-Agent Sistem
## 📌 1. Proje Özeti Divan-ı Neural Telekom, **ADK (Agentic Development Kit)** yapısına uygun olarak geliştirilmiş çoklu ajan (Multi-Agent) tabanlı bir müşteri hizmetleri otomasyon sistemidir. Sistem, gelen müşteri taleplerini analiz eder, uygun alt ajanlara yönlendirir ve yanıtları kullanıcıya sunar. Paket sorgulama, paket değişikliği, fatura sorgulama, ödeme ve teknik destek gibi işlemler farklı **Domain Agent**’lar tarafından yürütülür. --- ## 🏗 2. Mimari Şeması
mermaid
flowchart TD
    A[📩 Kullanıcı Promptu] --> B[🤖 Ana Agent (Supervisor)]
    B --> C[🧭 Router (Intent Tespiti)]
    C -->|intent: auth_verification| D1[🔑 Auth Agent]
    C -->|intent: billing_query| D2[💰 Billing Agent]
    C -->|intent: package_query| D3[📦 Package Query Agent]
    C -->|intent: package_change| D4[🔄 Package Change Agent]
    C -->|intent: payment_process| D5[💳 Payment Agent]
    C -->|intent: tech_support| D6[🛠 Tech Support Agent]
    C -->|intent: general_conversation| D7[💬 General Agent]
    D1 & D2 & D3 & D4 & D5 & D6 & D7 --> B
    B --> E[📤 Kullanıcıya Yanıt]

    subgraph Multi-Agent Yapısı
    D1
    D2
    D3
    D4
    D5
    D6
    D7
    end

Mimari Özellikler
Supervisor Agent: Oturum yönetimi, intent routing, context stack kontrolü
Router: Doğru Domain Agent’ı belirleyen niyet sınıflandırıcı
Domain Agent’lar: Sadece kendi görev alanında çalışır

Çalışma Modları:
  Parallel Agents
Sequential Agents

Loop Agents

Multiprocessing desteği

🤖 3. Kullanılan Framework ve Modeller
Sürüm	Ana Agent Modeli	Sub-Agent Modeli
v3	GPT (OpenAI)	LLaMA
v5	GPT (OpenAI)	Trendyol LLM

Framework:

Streamlit (Canlı kullanıcı arayüzü)

RAG / Fine-tune: Kullanılmıyor

⚙️ 4. Kurulum ve Çalıştırma
Burayı sen dolduracaksın. Örnek olarak:

bash
Kopyala
Düzenle
# Gerekli paketlerin yüklenmesi
pip install -r requirements.txt

# Streamlit uygulamasını çalıştırma
streamlit run app.py
📚 5. Senaryoların Açıklaması
Auth Agent → Telefon numarası doğrulama, TR format kontrolü

Billing Agent → Fatura bilgisi, ödeme durumu, borç/bakiye sorgulama

Package Query Agent → Paket bilgileri ve karşılaştırma

Package Change Agent → Paket değişikliği, onay süreci

Payment Agent → Ödeme yöntemi seçimi, işlem başlatma

Tech Support Agent → Temel teknik destek, ticket oluşturma

General Agent → Selamlaşma, genel yönlendirme

🗄 6. Veri Setleri
mock_packages_db.json
Paket çeşitleri, fiyat, hız, veri miktarı, kategori bilgileri
(örnek veri):

json
Kopyala
Düzenle
{
  "id": "P001",
  "name": "SuperNet 50",
  "price": 150,
  "speed": "50Mbps"
}
mock_users_db.json
Kullanıcı bilgileri, mevcut paket, bakiye, fatura durumu
(örnek veri):

json
Kopyala
Düzenle
{
  "phone": "05493186241",
  "name": "Hasan",
  "surname": "Kaya",
  "current_package": "SuperNet 50",
  "balance": 187.50,
  "status": "paid"
}
🛠 7. Zorluklar ve Çözümler
Burayı sen dolduracaksın.

📊 8. Ölçümleme Sonuçları
Burayı sen dolduracaksın.

📈 9. Ölçeklenebilirlik Değerlendirmesi
Burayı sen dolduracaksın.
