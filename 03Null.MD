Null
======

## Sorular

- C# için "null" ne demek? Bir tipin "nullable" olması ne demek? Nullable olmayan bir veri tipinden değişken tanımlarken nullable hâle getirebilir miyim, yapabilirsem nasıl? (Birden fazla yöntemi varsa hepsini söylemeniz gerekmekte.)
- Null-Coalescing operatörü (??) ne işe yarar açıklayıp çok minik bir senaryoda Null-Coalescing operatörünü kullanmadan bir işlem yapan bir kod yazın ardından aynı kodun Null-Coalescing operatörünü kullanarak aynı işlemi yapan hâlini yazın.
- Null-Coalescing Assignment operatörü (??=) ne işe yarar açıklayıp çok minik bir senaryoda Null-Coalescing Assignment operatörünü kullanmadan bir işlem yapan bir kod yazın ardından aynı kodun Null-Coalescing Assignment operatörünü kullanarak aynı işlemi yapan hâlini yazın.
- Null-Conditional operatörü (?. ve ?[]) ne işe yarar açıklayıp çok minik bir senaryoda Null-Conditional operatörünü kullanmadan bir işlem yapan bir kod yazın ardından aynı kodun Null-Conditional operatörünü kullanarak aynı işlemi yapan hâlini yazın.

## Cevaplar

Öncelikle değer olarak null'a değinecek olursak değer tutmama halini belirtir. İlk olarak C# 2.0 ile birlikte hayatımıza giren null değerler Nullable sınıfından türetilerek kullanılıyordu.

```csharp
Nullable<int> i = null;
```

Ancak ilerleyen süreçte uzun uzun Nullable<> yazmak bize zor gelmiş olacak ki ? kullanımı ortaya çıktı. Burada da yine benzer bir şekilde kullanım karşılıyor bizi.

```csharp
int? i = null;
```

Günümüzde iki türü de aktif olarak kullanabiliyoruz.

Null-Coalescing'e baktığımızda ise null olmayan bir değere nullable bir değişkenin ataması yapılırken olası hataları ortadan kaldırmak için default bir değer vermemizi sağlayan operatördür. Kullanmazsak zaten bizi uyarıyor. Yanlış anlamadıysam C# 7.3 ile kavuşmuşuz bu özelliğe.

```csharp
int? c = null;
int d = c;
Console.WriteLine($"d'nin degeri {d}"); 
```

```
error CS0266: Cannot implicitly convert type 'int?' to 'int'. An explicit conversion exists (are you missing a cast?)
```

```csharp
int? c = null;
int d = c ?? -1;
Console.WriteLine($"d'nin degeri {d}"); 
```

```
d'nin degeri -1
```

C# 8.0 ile de Null-Coalescing Assignment operatörü hayatımıza girmiş ki bu işimizi daha da kolaylaştırıyor. Tıpkı += operatöründeki gibi çalışıyor.

```csharp
int? c = null;
c ??= -1;
Console.WriteLine($"c'nin degeri {c}"); 
```

```
c'nin degeri -1
```

Bu arada bir not eklemek istiyorum. Null-Coalescing ve Null-Coalescing Assignment operatörlerini zincirleme bir şekilde kullanabiliyoruz.

```csharp
a ?? b ?? c
d ??= e ??= f
```

Şimdi de son olarak C# 6.0 ile hayatımıza giren Null-Conditional operatörüne değinelim. Bu operatör ? öncesindeki objenin null olup olmadığını kontrol eder. Null ise varsayılan değeri döndürür, null değilse bir sonraki işleme geçer. Bu operatör öncesinde öğrenci adını öğrenmeye çalışalım. Bir öğrenci class'ı olduğunu varsayarsak, bir fonksiyon bize öğrenci adını döndürsün.

```csharp
public static string? GetStudentName(Student student){
    if (student != null)
        return student.Name;
    return null;
}
```

Operatörü de kullanırsak;

```csharp
public static string? GetStudentName(Student student){
    return student?.Name;
}
```

Büyük çapta bakarsak mesela öğrencinin aldığı dersi veren hocanın soyadını öğrenmek istersek bunun için de şöyle bir kod yazabiliriz.

```csharp
public static string? GetStudentName(Student student){
    return student?.Lesson?.Teacher?.Surname;
}
```

Uzun uzun yazmaya kalkarsak uzadıkça uzar bu yazı.