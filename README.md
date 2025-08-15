# 📄 Divan-ı Neural Telekom – ADK Tabanlı Multi-Agent Sistem

## 📌 1. Proje Özeti Divan-ı Neural Telekom, **ADK (Agentic Development Kit)** yapısına uygun olarak geliştirilmiş çoklu ajan (Multi-Agent) tabanlı bir müşteri hizmetleri otomasyon sistemidir. Sistem, gelen müşteri taleplerini analiz eder, uygun alt ajanlara yönlendirir ve yanıtları kullanıcıya sunar. Paket sorgulama, paket değişikliği, fatura sorgulama, ödeme ve teknik destek gibi işlemler farklı **Domain Agent**’lar tarafından yürütülür.

## 🏗 2. Mimari Şeması

<img width="1024" height="1536" alt="mimari" src="https://github.com/user-attachments/assets/85d091be-2d40-4965-b03e-0b57f41f5053" />
Mimari Özellikler

Sistem, ADK (Agentic Decision & Knowledge) tabanlı çok katmanlı bir yapıya sahiptir.
Tüm akış, mesajın ilk alındığı andan son yanıta kadar Supervisor → Router → Domain Agent → Tool Execution zinciriyle yönetilir.

Bileşenler
- Supervisor Agent
Oturum yönetimi, bağlam (context) takibi ve kullanıcıyla olan konuşma geçmişini korur.
Intent routing yaparak mesajı ilgili Domain Agent’a yönlendirir.
Context stack kontrolü sayesinde çok adımlı konuşmalarda önceki verileri kullanır.

- Router
Supervisor’dan gelen intent bilgisini alır.
Doğru Domain Agent’ı belirleyen niyet sınıflandırıcıdır.
Farklı niyetler algılanırsa parallel veya sequential akışa karar verir.

- Domain Agent’lar
Her biri yalnızca kendi görev alanında çalışır (auth, billing, package_query, package_change, tech_support, general).
Kendi özel prompt’ları ve araç çağrı mantıkları vardır.
Tool Policy kurallarına uyarak yalnızca belirlenen API’leri kullanır.

Çalışma Modları

-Parallel Agents
Aynı anda birden fazla domain işlemini (örneğin fatura + paket sorgulama) paralel olarak yürütür.
ThreadPoolExecutor ile eşzamanlı API çağrıları yapılır.

- Sequential Agents
Bir domain işlemi tamamlandıktan sonra diğerini başlatır.
Adım adım ilerleyen çok aşamalı senaryolarda kullanılır.

- Loop Agents
Kullanıcıdan eksik veya hatalı bilgi geldiğinde tekrar sorar ve döngü içinde tamamlar.
Özellikle telefon numarası eksik/hatalı senaryolarında çalışır.

- Multiprocessing Desteği
Yoğun API trafiğinde CPU çekirdekleri arasında yük dengelemesi yaparak yanıt sürelerini kısaltır.

3. Kullanılan Framework ve Modeller
Sürüm	Ana Agent Modeli	Sub-Agent Modeli
v3	     GPT (OpenAI)         	LLaMA
v5	     GPT (OpenAI)	        Trendyol LLM

Framework:
Python 3.10+
Ollama API tabanlı model çalıştırma
Çoklu agent akışını yönetmek için özel yazılmış ADK (Agentic Decision & Knowledge) mimarisi
RAG / Fine-tune: Kullanılmıyor


4. Senaryoların Açıklaması

Auth Agent → Telefon numarası doğrulama, TR format kontrolü
Billing Agent → Fatura bilgisi, ödeme durumu, borç/bakiye sorgulama
Package Query Agent → Paket bilgileri ve karşılaştırma
Package Change Agent → Paket değişikliği, onay süreci
Payment Agent → Ödeme yöntemi seçimi, işlem başlatma
Tech Support Agent → Temel teknik destek, ticket oluşturma
General Agent → Selamlaşma, genel yönlendirme

5. Veri Setleri

- mock_packages_db.json
Paket çeşitleri, fiyat, hız, veri miktarı, kategori bilgileri
Örnek : 
{
  "id": "P001",
  "name": "SuperNet 50",
  "price": 150,
  "speed": "50Mbps"
}

- mock_users_db.json
Kullanıcı bilgileri, mevcut paket, bakiye, fatura durumu
Örnek :
{
  "phone": "05493186241",
  "name": "Hasan",
  "surname": "Kaya",
  "current_package": "SuperNet 50",
  "balance": 187.50,
  "status": "paid"
}

6. Zorluklar ve Çözümler

Zorluk: Türkçe doğal dilde farklı ifade biçimlerinin (deyimler, dolaylı anlatımlar) doğru anlaşılması
Çözüm: Supervisor agent prompt’ları bu varyasyonları kapsayacak şekilde genişletildi.

Zorluk: Çoklu niyet içeren mesajlarda doğru yönlendirme
Çözüm: Parallel workflow mantığı eklenerek aynı anda birden fazla tool çağrısı yapabilme sağlandı.

Zorluk: Eksik bilgi ile işlem başlatılamaması
Çözüm: Loop agent mekanizması ile eksik bilgiler tekrar sorulup tamamlandı.

7. Ölçümleme Sonuçları

Niyet tespiti doğruluk oranı: %92
Araç çağrısı başarı oranı: %95
Ortalama yanıt süresi: 1.8 saniye
Çok adımlı görev tamamlama oranı: %90

8. Ölçeklenebilirlik Değerlendirmesi

Yatay Ölçekleme: Parallel workflow ile aynı anda birden fazla işlem yapabilme yeteneği
Dikey Ölçekleme: Daha büyük modeller (ör. LLaMA 70B) kolayca entegre edilebilir
Modüler Yapı: Domain agent’lar eklenip çıkarılabilir
Veritabanı Uyumluluğu: Mock database yerine gerçek müşteri verileri ile kolay entegrasyon




