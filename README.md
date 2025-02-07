# Google-Sheets-in-Wikipedia-API-
 📌 Google Sheets İçin Wikipedia API Entegrasyonu

Bu proje, **Google Apps Script** kullanarak **Wikipedia API** üzerinden bilgi almayı sağlar ve Google Sheets'e kaydeder. **Tamamen ücretsizdir** ve **API Key gerektirmez**. 🚀

## ✨ **Özellikler**
- 📝 **Google Sheets'e Otomatik Bilgi Yazma**  
- 🔍 **Wikipedia'dan Konu Arama ve Özet Alma**  
- ✅ **API Key Gerekmez**  
- ⚡ **Google Apps Script ile Entegre Çalışır**  

---

## 🚀 **Kurulum ve Kullanım**

### **1️⃣ Google Sheets Hazırlama**
1. [Google Sheets](https://docs.google.com/spreadsheets/) açın ve yeni bir dosya oluşturun.
2. **"havam"** adlı bir sayfa ekleyin.
3. **A1 hücresine bir konu girin** (Örneğin: "Patoloji").

### **2️⃣ Google Apps Script Kodu Ekleme**
1. **Extensions (Eklentiler) > Apps Script** seçeneğini tıklayın.
2. Açılan sayfada aşağıdaki kodu kopyalayıp yapıştırın:

```javascript
function getWikipediaResponse() {
  var sheet = SpreadsheetApp.getActiveSpreadsheet().getSheetByName("havam");

  if (!sheet) {
    Logger.log("⚠ 'havam' sayfası bulunamadı! Lütfen sayfayı oluşturun.");
    return;
  }

  // Kullanıcının girdiği kelimeyi A1 hücresinden al
  var query = sheet.getRange("A1").getValue();
  if (!query) {
    sheet.getRange("G1").setValue("⚠ Hata: Lütfen A1 hücresine bir konu girin!");
    return;
  }

  var apiUrl = "https://tr.wikipedia.org/api/rest_v1/page/summary/" + encodeURIComponent(query);

  try {
    var response = UrlFetchApp.fetch(apiUrl, {
      method: "get",
      muteHttpExceptions: true
    });

    var json = JSON.parse(response.getContentText());
    var summary = json.extract || "⚠ Yanıt alınamadı!";
    var pageUrl = json.content_urls.desktop.page || "⚠ Sayfa bulunamadı!";

    // Yanıtı Google Sheets'e yaz
    sheet.getRange("G1").setValue(summary);
    sheet.getRange("H1").setValue(pageUrl);
    Logger.log("✅ Wikipedia yanıtı başarıyla alındı: " + summary);

  } catch (error) {
    Logger.log("⚠ Hata: " + error.message);
    sheet.getRange("G1").setValue("⚠ Hata: Yanıt alınamadı!");
  }
}
```

### **3️⃣ Kodu Çalıştırma**
1. **Kodu kaydedin.**
2. **"▶️" (Oynat) butonuna basarak fonksiyonu çalıştırın.**
3. Google izin isterse **yetkilendirmeyi kabul edin.**
4. **G1 hücresinde Wikipedia’dan gelen bilgiyi göreceksiniz.** 🔥
5. **H1 hücresinde Wikipedia sayfasına link eklenecek.** 🔗

---

## 🛠 **Örnek Çıktı**
| A Sütunu (Konu) | G Sütunu (Wikipedia Özeti) | H Sütunu (Wikipedia Linki) |
|-----------------|----------------------------|----------------------------|
| Patoloji | Patoloji, hastalıkların nedenlerini, gelişim süreçlerini ve etkilerini inceleyen tıp dalıdır. | [Wikipedia](https://tr.wikipedia.org/wiki/Patoloji) |

---

## 🎯 **Avantajlar**
- ✅ **Tamamen Ücretsizdir** (API Key gerektirmez!)
- 📚 **Wikipedia’dan Güvenilir Bilgi Çeker**
- ⚡ **Hızlı ve Kolay Kullanılır**

🔹 **Eğer daha fazla özellik eklemek isterseniz, geliştirmeler yapabiliriz!** 😊 🚀

