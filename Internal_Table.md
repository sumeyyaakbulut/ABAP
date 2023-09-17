# Internal Tabloların Oluşturulması

```cadence
  DATA  itab1 TYPE STANDARD TABLE OF data_type.
  DATA  itab2 TYPE TABLE OF data_type.
```


TYPE TABLE OF kullanıldığında, bir iç tablo türü oluşturulur ve bu iç tablo, herhangi bir veritabanı tablosu ile ilişkilendirilmez. Bu iç tablo, herhangi bir türdeki verileri içerebilir ve programın çalışması sırasında bellekte oluşturulur ve yönetilir. Veri eklemesi (INSERT), veri çıkarması (DELETE) veya sıralama gibi işlemler program tarafından kontrol edilir.

TYPE STANDARD TABLE OF kullanıldığında, bir iç tablo türü oluşturulur ve bu iç tablo, bir veritabanı tablosunun türü ile eşleşir. Bu, veritabanı tablosundan doğrudan veri almak ve veritabanı işlemleri yapmak için kullanışlıdır. TYPE STANDARD TABLE OF ile tanımlanan iç tablolar, ilgili veritabanı tablosu ile uyumlu olacak şekilde tasarlanır ve bu nedenle veritabanı işlemlerinde kullanılır.

TYPE TABLE OF ile TYPE STANDARD TABLE OF farkını anlamak için aşağıdaki kodu inceleyelim.

```cadence
DATA: lt_table1 TYPE TABLE OF i,
      lt_table2 TYPE STANDARD TABLE OF i,
      lv_value  TYPE i.

lv_value = 2.
APPEND lv_value TO lt_table1.
APPEND lv_value TO lt_table2.

lv_value = 1.
APPEND lv_value TO lt_table1.
APPEND lv_value TO lt_table2.

WRITE: 'lt_table1 (TYPE TABLE OF):',
       / lt_table1.

WRITE: 'lt_table2 (TYPE STANDARD TABLE OF):',
       / lt_table2.
```

Yukarıdaki kodun çıktısı aşağıda gösterilmiştir. lt_table1, TYPE TABLE OF ile tanımlandığı için doğrudan sıralanmaz ve sıralamak için bir SORT komutu kullanılması gerekir. Bu nedenle, lt_table1 iç tablosu eklenen sırayla görüntülenir.lt_table2, TYPE STANDARD TABLE OF ile tanımlandığı için otomatik olarak sıralanır. Bu nedenle, lt_table2 iç tablosu eklenen değerlerin sırasına göre görüntülenir.

![image](https://github.com/sumeyyaakbulut/ABAP/assets/62395974/38fe7cc5-1e04-42a1-8a08-1140d0936b7f)


SORTED TABLE:
İç tablo otomatik olarak sıralanır ve verilere erişim hızlıdır.
Sıralama gerektiren durumlar için ekstra bir işlem yapmanıza gerek yoktur.
Aynı anahtara sahip yinelenen öğeleri içerebilir.
WITH NON-UNIQUE DEFAULT KEY ifadesi ile aynı anahtara sahip yinelenen öğeleri destekler.

HASHED TABLE:
Verilere hızlı erişim sağlar, çünkü anahtar değerleri kullanarak verilere doğrudan erişim yapabilirsiniz.
Anahtar-değer çiftlerini içerir ve her anahtar yalnızca bir kez iç tabloya eklenir.
Anahtar değerlerinin benzersiz olması gereklidir.
Sıralama gerektiren durumlar için ekstra bir işlem yapmanız gerekir.
