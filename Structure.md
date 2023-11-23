## Structure
TYPES: BEGIN OF kullanımı genellikle farklı tablolardan veya veri yapılarından alanları bir arada kullanmak için kullanılır. Bu yapının temel amacı, belirli bir veri yapısını veya tabloyu temsil eden bir tür (TYPE) oluşturmaktır. Bu tür, içinde farklı alanları (alanlar veya bileşenler) barındırabilir.

```cadence
TYPES: BEGIN OF struc_type,
         comp1 TYPE i,                           
         comp2 TYPE c LENGTH 5,       
         comp4 TYPE itab_type,        
         comp5 TYPE ddic_type,        
       END OF struc_type.
```
SAP ABAP'da TYPES: BEGIN OF ile tanımlanan türler, genellikle program içinde kullanılmak üzere özel veri yapıları oluşturmak için kullanılır. Bu tür veri yapıları, veri tabanı seviyesindeki tablolardan türetilecek bir tür değildir. İki temel nedenle bu tür bir tercih yapılabilir:

### Program İhtiyaçları:

İşlevsellik açısından, programın ihtiyaçlarına özgü veri yapıları oluşturmak gerekebilir. Veri tabanı tablolarının yapısı genellikle iş mantığı, kullanıcı arayüzü veya program içindeki özel gereksinimlere uymayabilir. Bu durumda, özel bir tür oluşturarak programın ihtiyaçlarına uygun bir veri yapısı tasarlamak daha esnek ve uygun olabilir.

### Performans ve Bakım Kolaylığı:

Veri tabanı tablolarının her zaman programın işleyişine uygun olması gerekmez. Program içinde ihtiyaç duyulan özel bir veri yapısı, belirli bir görevi daha etkili bir şekilde gerçekleştirebilir. Ayrıca, program içindeki veri yapısını değiştirmek daha hızlı ve kolay olabilir. Veri tabanındaki bir tabloyu değiştirmek, bazen karmaşık olabilir ve daha fazla bakım süresi gerektirebilir.


DATA: BEGIN OF ifadesi, genellikle program içinde kullanılacak veri değişkenlerini tanımlamak için kullanılır. Bu ifade ile belirli bir veri alanı başlatılır ve bu alan içine farklı türde değişkenler eklenerek bir veri yapısı oluşturulabilir.

```cadence
DATA: BEGIN OF struc,
        comp1 TYPE ...,
        comp2 TYPE ... VALUE ...,
        comp3 TYPE i VALUE 9,
      END OF struc.
```
Mevcut yapılandırılmış türleri kullanarak yapılar oluşturma

```cadence
TYPES: BEGIN OF struc_type,
         comp1 TYPE i,                            
         comp2 TYPE c LENGTH 5,   
       END OF struc_type.

DATA struc_1 TYPE struc_type.
. 
DATA: struc_2 TYPE scarr.
     
DATA struc_3 TYPE cl_some_class=>struc_type.

DATA: struc_4 LIKE struc_1.
      
      
```
