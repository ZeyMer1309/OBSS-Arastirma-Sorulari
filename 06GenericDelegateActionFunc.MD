Generic Class ve Method'lar, Delegate, Action ve Func
======

## Sorular

- Generic Class ve Generic Method nedir açıklayın.
- Bir ve birden fazla Generic Type içeren birer Generic Class örneği kodlayın.
- Input olarak da output olarak da aynı Generic Type'ı içeren bir Generic Method örneği kodlayın.
- Input olarak ve output olarak farklı Generic Type'ları içeren bir Generic Method örneği kodlayın.
- Delegate nedir açıklayıp bir kullanım örneği kodlayın.
- Action<T> nedir açıklayıp bir kullanım örneği kodlayın.
- Func<T, TResult> nedir açıklayıp bir kullanım örneği kodlayın.
- Aşağıdaki kodu function delegateleri kullanarak daha kompakt bir hâlde nasıl refactor edersiniz?
```csharp
string operation = Console.ReadLine();
decimal result = 0;
if (operation == "+")
{
    result = Add(x, y);
}
else if (operation == "-")
{
    result = Substract(x, y);
}
else if (operation == "*")
{
    result = Multiply(x, y);
}
else if (operation == "/")
{
    result = Divide(x, y);
}
else
{
    throw new Exception("Hatalı Giriş");
}

Console.WriteLine(result);

decimal Add(decimal x, decimal y) { return x + y; }
decimal Substract(decimal x, decimal y) { return x - y; }
decimal Multiply(decimal x, decimal y) { return x * y; }
decimal Divide(decimal x, decimal y) { return x / y; }
```

## Cevaplar

