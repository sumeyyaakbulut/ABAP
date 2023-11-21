## Field Symbols

ABAP'da Field Symbol, bir program içindeki değişkenleri veya veri nesnelerini temsil eden sembolik isimlerdir. Bu sembolik isimler, programın çalışma zamanında dinamik olarak belirlenir ve program içindeki değişkenlere dinamik bir şekilde bağlanabilir. Field symbol'lar, programlama dilindeki diğer pointer (işaretçi) konseptlerine benzer bir işlevi yerine getirir.

Field symbol'ların temel amaçları şunlardır:

1. **Dinamik Veri Erişimi:** Field symbol'lar, programın çalışma zamanında belirli bir veri nesnesine işaret etmek için kullanılır. Bu sayede, program içindeki değişkenlerin değerlerine dinamik olarak erişebilirsiniz.

2. **Genel (Generic) Programlama:** Field symbol'lar, genellikle iç tablolardaki veya diğer dinamik veri yapılarındaki verilere dinamik olarak erişim sağlamak için kullanılır. Bu, daha genel ve yeniden kullanılabilir kod yazmanıza olanak tanır.

3. **Sıklıkla Kullanılan Senaryolar:** Field symbol'lar genellikle ABAP programlarında iç tablolarda döngü yapma, tablo satırlarını dinamik olarak değiştirme, filtreleme ve diğer dinamik veri işleme senaryolarında kullanılır.


### Internal  Tablo İçeriği Değişimi

Bu tekniği kullandığınızda sistem, verileri dahili tablodan çalışma alanına kopyalar. Daha sonra, ifadeyi kullanarak bunları dahili tabloya geri kopyalamadan önce çalışma alanının içeriğini değiştirirsiniz MODIFY.
Bu tekniği büyük bir dahili tabloyu işlemek için kullanırsanız, verilerin ileri geri kopyalanmasının maliyeti nedeniyle performans sorunlarına neden olma riskiyle karşı karşıya kalırsınız.

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
![image](https://github.com/sumeyyaakbulut/ABAP/assets/62395974/09cba751-de31-4002-a05a-ed0a4ea22ddc)

### Internal Tablo İçeriği Field Symbol Değişimi
Alan sembolü bir işaretçidir. İşaretçi, farklı bir nesnenin bellek adresini bilen ve o nesneyi değiştirmenize olanak tanıyan bir veri nesnesidir. Dahili tablolar söz konusu olduğunda işaretçi, dahili tablonun bir satırını önce onu bir çalışma alanına kopyalamadan adreslemenize olanak tanır. Bir çalışma alanıyla değil, doğrudan tablo satırıyla çalıştığınız için değişikliklerinizi tekrar dahili tabloya kopyalamanız gerekmez.
```cadence
DATA lt_carrier TYPE STANDARD TABLE OF st_carrier
                     WITH NON-UNIQUE KEY carrier_id.
FIELD-SYMBOLS <fs_carrier> LIKE LINE OF lt_carrier.
  LOOP AT lt_carrier ASSIGNING <fs_carrier>
                     WHERE currency_code IS INITIAL.
    <fs_carrier>-currency_code = 'USD'.
    
ENDLOOP.
```
![image](https://github.com/sumeyyaakbulut/ABAP/assets/62395974/7a0c9be6-cd83-4482-a39a-bfddefd42008)
