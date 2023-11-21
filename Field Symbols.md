## Field Symbols

ABAP'da Field Symbol, bir program içindeki değişkenleri veya veri nesnelerini temsil eden sembolik isimlerdir. Bu sembolik isimler, programın çalışma zamanında dinamik olarak belirlenir ve program içindeki değişkenlere dinamik bir şekilde bağlanabilir. Field symbol'lar, programlama dilindeki diğer pointer (işaretçi) konseptlerine benzer bir işlevi yerine getirir.

Field symbol'ların temel amaçları şunlardır:

1. **Dinamik Veri Erişimi:** Field symbol'lar, programın çalışma zamanında belirli bir veri nesnesine işaret etmek için kullanılır. Bu sayede, program içindeki değişkenlerin değerlerine dinamik olarak erişebilirsiniz.

2. **Genel (Generic) Programlama:** Field symbol'lar, genellikle iç tablolardaki veya diğer dinamik veri yapılarındaki verilere dinamik olarak erişim sağlamak için kullanılır. Bu, daha genel ve yeniden kullanılabilir kod yazmanıza olanak tanır.

3. **Sıklıkla Kullanılan Senaryolar:** Field symbol'lar genellikle ABAP programlarında iç tablolarda döngü yapma, tablo satırlarını dinamik olarak değiştirme, filtreleme ve diğer dinamik veri işleme senaryolarında kullanılır.


### Internal  Tablo İçeriği Değişimi
```cadence
DATA lt_carrier TYPE STANDARD TABLE OF st_carrier
                     WITH NON-UNIQUE KEY 
```
https://learning.sap.com/learning-journey/acquire-core-abap-skills/using-field-symbols-to-process-internal-tables_f1855f41-00d3-4f8d-9a2c-663a321c6637
