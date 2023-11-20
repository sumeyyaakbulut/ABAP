TYPES: BEGIN OF kullanımı genellikle farklı tablolardan veya veri yapılarından alanları bir arada kullanmak için kullanılır. Bu yapının temel amacı, belirli bir veri yapısını veya tabloyu temsil eden bir tür (TYPE) oluşturmaktır. Bu tür, içinde farklı alanları (alanlar veya bileşenler) barındırabilir.

TYPES: BEGIN OF struc_type,
         comp1 TYPE i,                           
         comp2 TYPE c LENGTH 5,       
         comp4 TYPE itab_type,        
         comp5 TYPE ddic_type,        
       END OF struc_type.
