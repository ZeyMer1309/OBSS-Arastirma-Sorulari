Pass by Value ve Pass by Reference
======

## Sorular

- C# için "Pass by Value" ve "Pass by Reference" kavramları neler, bunların farkı ne, C#'da hangi tipler varsayılanda Reference Type hangileri Value Type araştırın.
- Bir tipin Value Type olmasının ne etkisi var, Reference Type olmasının ne etkisi var bunları gösteren minik bir kod bloğu yazın. (Fonksiyonların vs ne yaptığı önemli değil.)
- Varsayılanda Value Type olan bir tipi Reference Type gibi davrandırtabilir miyiz, yapabiliyorsak bunu örnekleyen minik bir kod bloğu yazın.
- Varsayılanda Reference Type olan bir tipi Value Type gibi davrandırtabilir miyiz, yapabiliyorsak bunu örnekleyen minik bir kod bloğu yazın.

## Cevaplar

Pass by Value ile veriyi bir fonksiyona gönderdiğimizde yalnızca değeri gider ve biz fonksiyon üzerinde herhangi bir değişiklik yaptığımızda değerin asıl tutulduğu değişkende veri değişmez. Buna karşılık Pass by Reference ile veriyi gönderip değişiklik yaparsak asıl değer de değişecektir.

Varsayılan olarak Value Type'lar başlıca sayısal değer tutan tipler, bool, char, tuple, ... idir. Reference Type'lar ise record, class, interface ve delegate'lerdir.


```csharp
static void Main(string[] args)
{
    int test = 5;
    AddByValue(test);
    Console.WriteLine(test);
    AddByReference(ref test);
    Console.WriteLine(test);


}

public static void AddByValue(int num)
{
    num += 5;
}

public static void AddByReference(ref int num)
// Varsayılanda Value Type olan bir tipi Reference Type olarak kullanmak
{
    num += 5;
}

public static void
```

```
5
10
```


```csharp
class RefType
{
    public int Number { get; set; }
}
```

```csharp
static void Main(string[] args)
{
    RefType refType = new RefType();
    refType.Number = 20;
    Console.WriteLine(refType.Number);
    AddOnClass(refType);
    Console.WriteLine(refType.Number);
    AddByValue(refType.Number);
    Console.WriteLine(refType.Number);
}

public static void AddOnClass(RefType refType)
{
    refType.Number += 5;
}

public static void AddByValue(int num)
// Dışardan gelecek verinin ilgili değerini göndererek ya da fonksiyon içinde gelen veriyi yeni bir değişkende işleyerek asıl değeri koruyarak referans tipi değer tipi gibi çalıştırabiliriz.
{
    num += 5;
}
```

```
20
25
25
```


