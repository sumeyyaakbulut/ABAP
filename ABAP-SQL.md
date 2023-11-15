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

## SELECT DISTINCT
SELECT sorgusu kullanılırken belirli bir alanın benzersiz değerlerini getirmek için kullanılır. Bu ifade, belirtilen alanın farklı değerlerini içeren satırları döndürür, yani tekrar eden değerleri filtreler.

Bir örnek üzerinden inceleyelim.
"dbtab" tablosunda "comp1" alanı şu değerlere sahipse:
comp1
A
B
A
C
B

```cadence
SELECT DISTINCT comp1
  FROM dbtab
  WHERE ...
  INTO TABLE @itab.
```
Bu sorgu, "dbtab" adlı tablodan belirli bir koşulu sağlayan satırları seçer ve "comp1" adlı alanın benzersiz (distinct) değerlerini getirir.Yani, her değeri yalnızca bir kez içeren benzersiz (distinct) "comp1" değerlerini içerecektir.


# Veri Kaynağı Olarak Internal Tablodan Veri Okuma

FROM @itab1 AS tab ifadesinde itab1 adlı bir içsel tablo (internal table) referans alınmaktadır, yani burada bir veritabanı tablosu değil, program içinde tanımlanan bir içsel tablo kullanılmaktadır. Bu şekilde, veritabanı tablolarının yanı sıra program içinde tanımlanan içsel tablolar da kullanılabilir.

Bu tür kullanımlar, genellikle program içinde geçici olarak tutulan verilerle çalışırken veya veri manipülasyonu yaparken kullanılır. Veritabanı tabloları genellikle FROM ifadesinde doğrudan belirtilirken, program içinde tanımlanan içsel tabloların referansları @ sembolü ile başlatılır.

Örnek
```cadence
DATA: lt_source TYPE TABLE OF sflight,
      lt_filtered TYPE TABLE OF sflight,
      ls_flight   TYPE sflight.

" lt_source adlı içsel tabloyu doldurma
APPEND VALUE #( carrid = 'LH' connid = '123' " Diğer alanlar da eklenmeli"
                fldate = '20230101' seatsocc = 100 )
              TO lt_source.

APPEND VALUE #( carrid = 'LH' connid = '456' " Diğer alanlar da eklenmeli"
                fldate = '20230201' seatsocc = 150 )
              TO lt_source.

" lt_source adlı içsel tabloyu kullanarak veri filtreleme
SELECT * FROM @lt_source AS tab
  WHERE carrid = 'LH'
  INTO TABLE @lt_filtered.

LOOP AT lt_filtered INTO ls_flight.
  " Filtrelenmiş verilerle bir şeyler yapma
ENDLOOP.


```

