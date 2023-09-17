# Internal Tabloların Oluşturulması

```cadence
  DATA  itab1 TYPE STANDARD TABLE OF data_type.
  DATA  itab2 TYPE TABLE OF data_type.
```


*TYPE TABLE OF kullanıldığında, bir iç tablo türü oluşturulur ve bu iç tablo, herhangi bir veritabanı tablosu ile ilişkilendirilmez. Bu iç tablo, herhangi bir türdeki verileri içerebilir ve programın çalışması sırasında bellekte oluşturulur ve yönetilir. Veri eklemesi (INSERT), veri çıkarması (DELETE) veya sıralama gibi işlemler program tarafından kontrol edilir.

TYPE STANDARD TABLE OF kullanıldığında, bir iç tablo türü oluşturulur ve bu iç tablo, bir veritabanı tablosunun türü ile eşleşir. Bu, veritabanı tablosundan doğrudan veri almak ve veritabanı işlemleri yapmak için kullanışlıdır. TYPE STANDARD TABLE OF ile tanımlanan iç tablolar, ilgili veritabanı tablosu ile uyumlu olacak şekilde tasarlanır ve bu nedenle veritabanı işlemlerinde kullanılır.*

```cadence
DATA: lt_table1 TYPE TABLE OF i,
      lt_table2 TYPE STANDARD TABLE OF i,
      lv_value   TYPE i.


" Verileri ekleyelim
lv_value = 2.
APPEND lv_value TO lt_table1.
APPEND lv_value TO lt_table2.

lv_value = 2.
APPEND lv_value TO lt_table1.
APPEND lv_value TO lt_table2.

" lt_table1'i yazdıralım (TYPE TABLE OF)
WRITE: 'lt_table1 (TYPE TABLE OF):',
       / lt_table1.

" lt_table2'yi yazdıralım (TYPE STANDARD TABLE OF)
WRITE: 'lt_table2 (TYPE STANDARD TABLE OF):',
       / lt_table2.
```
