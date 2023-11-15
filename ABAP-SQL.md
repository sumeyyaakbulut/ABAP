# Select Kullanarak Verileri Okuma

'SELECT' Bir veya daha fazla veritabanı tablosundaki veri okumak için ABAP SQL ifadelerini kullanılır. Bu, sonuç kümesini uygun bir veri nesnesine atayarak çok satırlı veya tek satırlı bir sonuç kümesi oluşturmak için yapılır.

FIELDS İfadesi Kullanımı
```cadence
 SELECT SINGLE FROM dbtab
  FIELDS field1, field2
  WHERE ...
  INTO @lv_field1
```
Artıları:Daha esnek ve genel kullanım sağlar. Özellikle çok sayıda alan varsa, belirli alanları seçerek sorgunun daha okunabilir olmasına yardımcı olabilir.
Eksileri:ABAP yorumlayıcısının altında bu ifade, tüm alanları seçmek yerine belirli alanları seçerek bir avantaja dönüşmeyebilir. Performans farkı, sorgunun karmaşıklığına ve kullanılan veritabanına bağlı olacaktır.

Doğrudan Alanları Belirleme:
```cadence
 SELECT SINGLE field1, field2 FROM dbtab
  WHERE ...
  INTO @lv_field1.

```
Artıları,Daha basit ve açık bir sorgu yapısına sahiptir. Belirli alanlar doğrudan belirtilir, bu nedenle sorgunun amacı daha açıktır


İki yaklaşımın da sonuçları aynı olacak, ancak performans açısından bazı farklılıklar olabilir. Eğer sorgu basitse veya tüm alanlara ihtiyaç varsa, doğrudan alanları belirtmek daha anlamlı ve açık olabilir. Eğer sorgu karmaşıksa veya sadece belirli alanlara ihtiyaç varsa, FIELDS ifadesi kullanılabilir.
