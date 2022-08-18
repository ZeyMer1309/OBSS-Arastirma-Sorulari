Pass by Value ve Pass by Reference
======

## C# için "Pass by Value" ve "Pass by Reference" kavramları neler, bunların farkı ne, C#'da hangi tipler varsayılanda Reference Type hangileri Value Type araştırın.
Pass by Value ile veriyi bir fonksiyona gönderdiğimizde yalnızca değeri gider ve biz fonksiyon üzerinde herhangi bir değişiklik yaptığımızda değerin asıl tutulduğu değişkende veri değişmez. Buna karşılık Pass by Reference ile veriyi gönderip değişiklik yaparsak asıl değer de değişecektir.

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
{
    num += 5;
}
```

```
5
10
```