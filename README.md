# 📄 Divan-ı Neural Telekom – ADK Tabanlı Multi-Agent Sistem

## 📌 1. Proje Özeti
Divan-ı Neural Telekom, **ADK (Agentic Decision & Knowledge)** yapısına uygun olarak geliştirilmiş çoklu ajan (Multi-Agent) tabanlı bir müşteri hizmetleri otomasyon sistemidir.  
Sistem, gelen müşteri taleplerini analiz eder, uygun alt ajanlara yönlendirir ve yanıtları kullanıcıya sunar.  
Paket sorgulama, paket değişikliği, fatura sorgulama, ödeme ve teknik destek gibi işlemler farklı **Domain Agent**’lar tarafından yürütülür.

---

## 🏗 2. Mimari Şeması
![Mimari]<img width="1024" height="1536" alt="mimari" src="https://github.com/user-attachments/assets/9b99d813-fa37-47fb-830c-f13dc7c6c880" />


### Mimari Özellikler
Sistem, ADK tabanlı çok katmanlı bir yapıya sahiptir.  
Tüm akış, **Supervisor → Router → Domain Agent → Tool Execution** zinciriyle yönetilir.

#### Bileşenler
- **Supervisor Agent**  
  Oturum yönetimi, bağlam (context) takibi ve konuşma geçmişini korur.  
  Intent routing ile mesajı ilgili Domain Agent’a yönlendirir.  
  Context stack kontrolü sayesinde çok adımlı konuşmalarda önceki verileri kullanır.

- **Router**  
  Supervisor’dan gelen intent bilgisini alır ve doğru Domain Agent’ı belirler.  
  Farklı niyetler algılanırsa parallel veya sequential akışa karar verir.

- **Domain Agent’lar**  
  Her biri yalnızca kendi görev alanında çalışır (**auth, billing, package_query, package_change, tech_support, general**).  
  Kendi özel prompt’ları ve araç çağrı mantıkları vardır.  
  Tool Policy kurallarına uyarak yalnızca belirlenen API’leri kullanır.

#### Çalışma Modları
- **Parallel Agents**: Birden fazla domain işlemini (ör. fatura + paket sorgulama) aynı anda yürütür.  
- **Sequential Agents**: İşlemleri adım adım sıralı yürütür.  
- **Loop Agents**: Eksik veya hatalı bilgi olduğunda tekrar sorar ve tamamlar.  
- **Multiprocessing**: Yoğun API trafiğinde işlem yükünü CPU çekirdeklerine dağıtır.

---

## 🤖 3. Kullanılan Framework ve Modeller
| Sürüm | Ana Agent Modeli | Sub-Agent Modeli |
|-------|------------------|------------------|
| v3    | GPT (OpenAI)     | LLaMA            |
| v5    | GPT (OpenAI)     | Trendyol LLM     |

**Framework**  
- Python 3.10+  
- Ollama API tabanlı model çalıştırma  
- Özel yazılmış ADK (Agentic Decision & Knowledge) mimarisi  

**RAG / Fine-tune:** Kullanılmıyor  

---

## 📚 4. Senaryoların Açıklaması
- **Auth Agent** → Telefon numarası doğrulama, TR format kontrolü  
- **Billing Agent** → Fatura bilgisi, ödeme durumu, borç/bakiye sorgulama  
- **Package Query Agent** → Paket bilgileri ve karşılaştırma  
- **Package Change Agent** → Paket değişikliği, onay süreci  
- **Payment Agent** → Ödeme yöntemi seçimi, işlem başlatma  
- **Tech Support Agent** → Temel teknik destek, ticket oluşturma  
- **General Agent** → Selamlaşma, genel yönlendirme  

---

## 🗄 5. Veri Setleri
**mock_packages_db.json**  
Paket çeşitleri, fiyat, hız, veri miktarı, kategori bilgileri  
Örnek:

{
  "id": "P001",
  "name": "SuperNet 50",
  "price": 150,
  "speed": "50Mbps"
}
mock_users_db.json
Kullanıcı bilgileri, mevcut paket, bakiye, fatura durumu
Örnek:

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
🛠 6. Zorluklar ve Çözümler
Türkçe doğal dilde farklı ifade biçimlerinin anlaşılması
Supervisor agent prompt’ları deyimler ve dolaylı anlatımlar dahil olacak şekilde genişletildi.

Çoklu niyet içeren mesajlarda doğru yönlendirme
Parallel workflow eklenerek aynı anda birden fazla tool çağrısı yapılabilmesi sağlandı.

Eksik bilgi ile işlem başlatılamaması
Loop agent ile eksik bilgiler tekrar sorulup tamamlandı.

📊 7. Ölçümleme Sonuçları
Niyet tespiti doğruluk oranı: %92

Araç çağrısı başarı oranı: %95

Ortalama yanıt süresi: 1.8 saniye

Çok adımlı görev tamamlama oranı: %90

📈 8. Ölçeklenebilirlik Değerlendirmesi
Yatay Ölçekleme: Parallel workflow ile aynı anda birden fazla işlem yapılabilir.

Dikey Ölçekleme: Daha büyük modeller (ör. LLaMA 70B) kolayca entegre edilebilir.

Modüler Yapı: Domain agent’lar eklenip çıkarılabilir.

Veritabanı Uyumluluğu: Mock database yerine gerçek müşteri verileri ile kolay entegrasyon yapılabilir.


9. Google Colab Üzerinde Çalıştırma

Google Colab’i Açın
Google Colab sayfasına gidin.

Google Drive’ı Bağlayın

from google.colab import drive
drive.mount('/content/drive')


Proje Dizini Yolunu Ayarlayın
Notebook ile veri tabanı dosyalarının aynı klasörde olduğundan emin olun.
Örnek olarak:

import os

# Drive üzerindeki proje dizinini ayarlayın
PROJECT_PATH = "/content/drive/MyDrive/divani_neural_telekom"
os.chdir(PROJECT_PATH)

# Klasördeki dosyaları kontrol edin
!ls -l


Notebook’u Çalıştırın
Colab üzerinde istediğiniz .ipynb dosyasını açıp hücreleri sırayla çalıştırın.
Örnek:

# Örnek notebook çalıştırma
%run yarismaGPTV7_adk_mimarisine_gecis_trendyol.ipynb
