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
