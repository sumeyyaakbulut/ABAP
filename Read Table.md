## Read Table
READ TABLE ifade dahili bir tablodaki belirli bir girişi aramak için kullanılır. 

 READ TABLE <internal_table>
   [WITH KEY <key_fields>]
   [TRANSPORTING <fields>]
   [INDEX <index>]
   [REFERENCE INTO <wa>]
   [BINARY SEARCH]
   [COMPARING <fields>].

İfadenin bileşenlerini parçalayalım:
* <internal_table>: Aramak istediğiniz dahili tablonun adıdır.
* WITH KEY <key_fields>: Aramanın gerçekleştirildiği anahtar alanları belirtir.
* TRANSPORTING <fields>: Belirtilen alanları dahili tablo girişinden başka bir çalışma alanına kopyalar.
* INDEX <index>: Aramanın başlaması gereken dizini belirtir. Bu belirtilmezse arama baştan başlar.
* REFERENCE INTO <wa>: Bulunan tablo girdisine bir referansı ile belirtilen çalışma alanına kopyalar <wa>.
* BINARY SEARCH: Sıralanmış bir dahili tabloda ikili arama gerçekleştirir. Bu seçeneğin doğru çalışması için dahili tablonun sıralanması gerekir.
* COMPARING <fields>: kullanılırken varsayılan olmayan karşılaştırma için alanları belirtir BINARY SEARCH.
Yukarıdaki ifadeleri kodda inceleyelim.

#### WITH KEY 
```cadence
DATA: lt_data TYPE TABLE OF Zemployee,
      ls_employee TYPE Zemployee,
      lv_employee_id TYPE i,
      lv_index TYPE sy-tabix.

"Zemployee'nin Çalışan Kimliği, Adı, Soyadı vb. alanları içeren bir structure olduğunu varsayalım..

"Internal tablosunu bazı verilerle doldurun
APPEND VALUE #( EmployeeID = 1 FirstName = 'Sümeyya' LastName = 'Akbulut' ) TO lt_data.
APPEND VALUE #( EmployeeID = 2 FirstName = 'Şifa' LastName = 'Keleş' ) TO lt_data.
APPEND VALUE #( EmployeeID = 3 FirstName = 'Sebiha' LastName = 'Dilek' ) TO lt_data.

"Arama için anahtar alanları belirtilmiştir.
lv_employee_id = 2.

"Girişi aramak için READ TABLE'ı with KEY ile kullanılmıştır
READ TABLE lt_data WITH KEY EmployeeID = lv_employee_id TRANSPORTING NO FIELDS.

"Girişin bulunup bulunmadığını kontrol edin.
IF sy-subrc = 0.
  lv_index = sy-tabix.
  WRITE: / 'Employee ID', lv_employee_id, 'found at index', lv_index.
  MOVE-CORRESPONDING lt_data[ lv_index ] TO ls_employee.
  " Now ls_employee contains the data of the found entry.
  WRITE: / 'Çalışan bilgileri:', ls_employee-FirstName, ls_employee-LastName.
ELSE.
  WRITE: / 'Çalışan ID', lv_employee_id, 'internal tabloda bulunamadı.'.
ENDIF.
```

#### TRANSPORTING 

ABAP'da "READ TABLE TRANSPORTING NO FIELDS" ifadesi, bir tablodan veri okurken okunan satırın herhangi bir alanını taşımamak için kullanılır. Bu ifade, okunan satırın tüm alanlarını başka bir değişkene veya alanlara taşımak istemediğiniz durumlarda kullanılır.

"READ TABLE" ifadesi, belirli bir tablodan veri okumak için kullanılır. Örneğin, bir iç tablodan veya veritabanından veri okumak için kullanılabilir. "TRANSPORTING" ise okunan değeri taşımak için kullanılan bir ek parametredir.

"TRANSPORTING" parametresi, okunan satırın hangi alanlarını taşıyacağını belirlemek için kullanılır. Eğer "NO FIELDS" kullanılırsa, okunan satırın hiçbir alanı taşınmaz. Bu durumda, sadece okunan satırın varlığını kontrol etmek veya sadece belirli bir koşulu kontrol etmek istediğinizde kullanışlıdır.



#### INDEX
```cadence
DATA: lt_data TYPE TABLE OF string,
      lv_index TYPE sy-tabix,
      ls_entry TYPE string.

* İç tabloya bazı değerler ekleyelim.
APPEND 'Sümeyya' TO lt_data.
APPEND 'Şifa' TO lt_data.
APPEND 'Sebiha' TO lt_data.

* İndeks numarasını belirleyelim.
lv_index = 2.

* İç tabloyu indeksleme ile okuyalım.
READ TABLE lt_data INDEX lv_index TRANSPORTING NO FIELDS.

* Sy-subrc kontrolü ile girişin bulunup bulunmadığını kontrol edelim.
IF sy-subrc = 0.
  ls_entry = lt_data[ lv_index ].
  WRITE: / 'İndeks bulundu:', lv_index.
  WRITE: / 'Bulunan giriş:', ls_entry.
ELSE.
  WRITE: / 'İndeks bulunamadı:', lv_index.
ENDIF.
```

#### REFERENCE INTO
Tablonun belirli bir indeksindeki girişin referansını tutan REFERENCE INTO kullanımı, aslında belirli bir indeksteki elemana referans alarak, bu elemana daha sonra referans üzerinden erişebilme yeteneğini sağlar.
```cadence
DATA: lt_data TYPE TABLE OF string,
      lr_entry TYPE REF TO string,
      ls_entry  TYPE string.

" İç tabloya bazı değerler ekleyelim.
APPEND 'Sümeyya' TO lt_data.
APPEND 'Şifa' TO lt_data.
APPEND 'Sebiha' TO lt_data.

" Belirli bir indeksi içeren girişi belirleyelim.
READ TABLE lt_data INDEX 2 REFERENCE INTO lr_entry TRANSPORTING NO FIELDS.

" Sy-subrc kontrolü ile girişin bulunup bulunmadığını kontrol edelim.
IF sy-subrc = 0 AND lr_entry IS NOT INITIAL.
  ls_entry = lr_entry->*.
  WRITE: / 'İndeks bulundu:', sy-tabix.
  WRITE: / 'Bulunan giriş:', ls_entry.
ELSE.
  WRITE: / 'İndeks bulunamadı veya lr_entry boş.', sy-tabix.
ENDIF.
```
Bu kodda,lt_data adındaki iç tablo üzerinde bir READ TABLE işlemi yapılır.
INDEX 2 ifadesi ile ikinci satırdaki eleman seçilir.
REFERENCE INTO lr_entry ifadesi ile bu elemanın referansı lr_entry değişkenine atanır.
TRANSPORTING NO FIELDS ifadesi ile herhangi bir alan taşınmaz (değiştirilmez).
Bu işlemin ardından, lr_entry değişkeni, lt_data iç tablosunun ikinci satırının referansını içerir. Yani, lr_entry üzerinden lt_data iç tablosundaki ikinci satırdaki değerlere erişebilirsiniz.
Bu kod bloğu içinde, ls_entry değişkeni, lr_entry referansının işaret ettiği değeri okur ve ekrana yazdırır. Eğer belirtilen indeks numarası geçerliyse (sy-subrc = 0) ve lr_entry boş değilse, ls_entry değişkenine atama yapılarak içeriğini okuyup ekrana yazdırılır. Aksi takdirde, belirtilen indeks bulunamamış veya lr_entry boşsa uygun bir hata mesajı yazdırılır.


####  BINARY SEARCH
Read table da doğrusal okumayla binary(ikili arama) hangi durumlarda kullanımın avantajlı olduğuna bakalım.
* Bunun nedeni, belirtildiği gibi ikili aramanın, eşleşen bir kayıt bulana kadar arama aralığını her 'arama' için ikiye bölmesi, oysa doğrusal aramanın bir eşleşme bulana kadar sırayla ilerlemesidir.
*Doğrusal bir arama için ortalama arama süresi, N/2 olacaktır; burada N, tablodaki girişlerin sayısıdır. Bir ikili arama, arama aralığını tekrar tekrar yarıya indirir. Bu nedenle arama süresi en fazla Log2(N)'dir.
*İkili aramam için önceden o internal tablonun sıralanmış olması gerekmektedir.(SORT, abap programlarında dahili tabloları sıralamak için kullanılır. order by, veri formu veritabanı tablolarını almak için seçme sorgusunda kullanılır yükselerek veya azalarak)
*Örnek: 128 girişli dahili bir tabloda, ortalama doğrusal aramanın 64 girişi kontrol etmesi ve en kötü durumda 128 girişin tamamının kontrol edilmesi gerekir. İkili arama, 128 girişli bir tablo için asla 8'den fazla girişi kontrol etmek zorunda kalmayacak.

```cadence
DATA: lt_data TYPE TABLE OF string,
      lv_search TYPE string.

" Internal tablonun sıralanması.
SORT lt_data.

" Internal tabloyu bazı verilerle doldurulması.
APPEND 'Sümeyya' TO lt_data.
APPEND 'Şifa' TO lt_data.
APPEND 'Sebiha' TO lt_data.

" Aranacak değeri belirtilmesi.
lv_search = 'Sebiha'.

" Sıralanan internal tabloda binary search yapın.
READ TABLE lt_data WITH KEY lv_search BINARY SEARCH TRANSPORTING NO FIELDS.

" Girişin bulunup bulunmadığını kontrol edin.
IF sy-subrc = 0.
  WRITE: / 'Bulundu:', lv_search, 'at index', sy-tabix.
ELSE.
  WRITE: / 'Bulunamadı:', lv_search.
ENDIF.
```
