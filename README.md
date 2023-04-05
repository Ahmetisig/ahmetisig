[ 2 ,27,16,22,18,6] -> Tüm sayılar karşılaştırıldı ve en küçük sayı bulundu. Böylece 22'yi 2 ile değiştirin.= n karşılaştırma
[2, 6 ,16,22,18,27] -> 27'nin olduğu ikinci konum için dizinin geri kalanını sıralı bir şekilde tekrar çaprazlayın. 6'nın dizideki ikinci en düşük değer olduğunu buldum, bu nedenle bu değerleri değiştirin. = (n-1) karşılaştırmalar
[2,6, 16 ,22,18,27] -> Benzer şekilde 3. konum için dizinin geri kalanını kat ettim ve dizideki en küçük 3. elemanı buldum.= (n-2) karşılaştırma
16 3. en düşük değer olduğu için dolayısıyla üçüncü sırada yer alacaktır.= (n-3) karşılaştırma
[2,6,16, 18 ,22,27] -> Dördüncü konum için 18 en düşük değer olduğu için 18'i 22 ile değiştirdim. = (n-4) karşılaştırmalar
[2,6,16,18, 22 ,27] -> Yine dizinin geri kalanını geçtim ve 22 en düşük değer, dolayısıyla aynı konuma gelecek. = (n-5) karşılaştırmalar
[2,6,16,18,22, 27 ] -> Şimdi sadece 27'ye sahibim ve bu da yineleme ama 27'nin son sırada yer alacağı açık. = 1 karşılaştırma
Tüm karşılaştırmalar = 1+(n-5)+(n-4)+(n-3)+(n-2)+(n-1)+n=[n(n+1)]/2 =(n^2+n)/2 --> Büyük O Gösterimi= O(n^2)
Sıralamadan sonra '18'in  ortada olduğunu görebiliriz. Yani 18 sayısını aramak ortalama bir durumdur.

[7,3,5,8,2,9,4,15,6] -> Seçim Sıralaması(ilk dört adım)
1 [2,3,5,8,7,9,4,15,6]
2 [2,3,5,8,7,9,4,15,6]
3 [2,3,4,8, 7,9,5,15,6]
4 [2,3,4,5,7,9,8,15,6]
