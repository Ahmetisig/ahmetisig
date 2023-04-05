16,21,11] & [8,12,22] -> Önce tüm diziyi eşit parçalara böldüm.
[16] & [21,11] & [8] & [12,22] -> Dizinin atomik birimlerine ulaşılana ve daha fazla bölme mümkün olmayana kadar bu iki diziyi daha fazla yarıya böldüm.
[16] & [21] , [11] & [8] & [12] , [22]
[16] & [11,21] & [8] & [12,22] -> Elemanların boyut karşılaştırmasına dayalı olarak tekrar elemanları birleştirmeye başladım.
[11,16,21] & [8,12,22] -> Her liste için öğeyi karşılaştırdım ve sonra bunları sıralı bir şekilde başka bir listede birleştirdim.
[8,11,12,16,21,22] -> Son birleştirme
Büyük O Gösterimi
Her birleştirme (n-1) karşılaştırmaya ihtiyaç duyar(n=dizi boyutu) EXP: [16] & [11,21] = [11,16,21] -> Yani 3 değer ve 2 karşılaştırma Seviyem ne kadar derin
? 2^x=n -> x=logn
n.logn=O(nlogn) --> Büyük O Gösterimi
