## Field Symbols

ABAP'da Field Symbol, bir program içindeki değişkenleri veya veri nesnelerini temsil eden sembolik isimlerdir. Bu sembolik isimler, programın çalışma zamanında dinamik olarak belirlenir ve program içindeki değişkenlere dinamik bir şekilde bağlanabilir. Field symbol'lar, programlama dilindeki diğer pointer (işaretçi) konseptlerine benzer bir işlevi yerine getirir.

Field symbol'ların temel amaçları şunlardır:

1. **Dinamik Veri Erişimi:** Field symbol'lar, programın çalışma zamanında belirli bir veri nesnesine işaret etmek için kullanılır. Bu sayede, program içindeki değişkenlerin değerlerine dinamik olarak erişebilirsiniz.

2. **Genel (Generic) Programlama:** Field symbol'lar, genellikle iç tablolardaki veya diğer dinamik veri yapılarındaki verilere dinamik olarak erişim sağlamak için kullanılır. Bu, daha genel ve yeniden kullanılabilir kod yazmanıza olanak tanır.

3. **Sıklıkla Kullanılan Senaryolar:** Field symbol'lar genellikle ABAP programlarında iç tablolarda döngü yapma, tablo satırlarını dinamik olarak değiştirme, filtreleme ve diğer dinamik veri işleme senaryolarında kullanılır.


### Internal  Tablo İçeriği Değişimi
```cadence
DATA lt_carrier TYPE STANDARD TABLE OF st_carrier
                     WITH NON-UNIQUE KEY carrier_id.
DATA ls_carrier LIKE LINE OF lt_carrier.
  LOOP AT lt_carrier INTO ls_carrier
                     WHERE currency_code IS INITIAL.
    ls_carrier-currency_code = 'USD'.
    MODIFY lt_carrier FROM ls_carrier
ENDLOOP.
```


```cadence
DATA lt_carrier TYPE STANDARD TABLE OF st_carrier
                     WITH NON-UNIQUE KEY carrier_id.
FIELD-SYMBOLS <fs_carrier> LIKE LINE OF lt_carrier.
  LOOP AT lt_carrier ASSIGNING <fs_carrier>
                     WHERE currency_code IS INITIAL.
    <fs_carrier>-currency_code = 'USD'.
    
ENDLOOP.
```
