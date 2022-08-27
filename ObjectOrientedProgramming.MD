Nesne Yönelimli Programlama Esasları
======

## Sorular

- Encapsulation nedir, niye uygularız, bize nasıl bir fayda sağlar? C#'da Encapsulation'ı nasıl uygularız kod örneğiyle gösterin.
- Polymorphism nedir, niye uygularız, bize nasıl bir fayda sağlar? C#'da Polymorphism'i nasıl uygularız kod örneğiyle gösterin.
- Abstraction nedir, niye uygularız, bize nasıl bir fayda sağlar? C#'da Abstraction'ı nasıl uygularız kod örneğiyle gösterin.
- Interface nedir, abstract class nedir? Abstract classlar bize nasıl özellikler sağlar?

## Cevaplar

Encapsulation, nesne içerisindeki belirli alanların dışarıdan erişimini kapatarak verilerdeki veya uygulama üzerindeki bozulmaların önüne geçmeyi sağlayan prensiptir. Bunu sağlarken de temelde 4 access modifier'i kullanıyoruz. Bunlar: Public, Protected, Internal, Private.

```csharp
public class Ayakkabi{
    private string marka;

    // Dışarıdan direkt marka alanına erişemeyiz. marka alanındaki veriyi okuyabilmek için GetMarka metoduna ihtiyacımız var.

    public string GetMarka{
        return marka;
    }
}
```

Polymorphism, base class'tan türetilen derived class'ların, base class üzerinden erişilebilmesini sağlar. Böylece kodu daha genel kullanıma uygun hale getirebiliriz.

```csharp
public class Urun
{
    public string UrunAdi { get; set; }
    public double Fiyat { get; set; }
    public double KDVOrani { get; set; }

    public double KDVUygula(){
        return Fiyat + Fiyat * KDVOrani;
    }
}

public class Tekstil : Urun
{
    public string KumasTuru { get; set; }
    public string UreticiFirma { get; set; }
}

static void Main(string[] args)
{
    Tekstil tekstil = new Tekstil();
    tekstil.Fiyat = 10;
    tekstil.KDVOrani = 0.18;
    Console.WriteLine(tekstil.KDVUygula()); // Çıktı olarak 11,8 verecektir.
}
```

Abstraction soyut olarak var olan ancak fiziksel olarak karşılığı olmayan kavramları ifade etmekte kullandığımız bir kavramdır. Abstract class da aynı karşılığa denk gelmektedir. Abstract class'tan herhangi bir nesne üretemeyiz ancak diğer nesnelerin base class'ı olarak işlev görebilir. Örneğin bir markete gittiğimizda ürün diye bir şey istesek bize yanıtsız gözlerle bakabilir. Oysaki oradaki tüm her şey birer üründür ancak farklı sınıflardan türetilmişlerdir. Bazıları deterjan sınıfından, bazıları abur cubur vs. vs... Yazılımda da bu abstract class'lar derived class'larda olacak temel özellikleri içeren sınıftır.

```csharp
public abstract class MuzikAleti
{
    public string Marka { get; set; }
    public string Aciklama { get; set; }

    public abstract string Cal();
    //Abstract metodlar; gövde kodları türetilmiş sınıflar tarafından tamamlanacak olan eksik bırakılmış metodlardır. Virtual ile karıştırılmamalıdır. Çünkü virtual dediğimizde asıl fonksiyon gövdeye sahiptir ve opsiyonel olarak override edilebilir.
}

public class Gitar : MuzikAleti
{
    public override string Cal()
    {
        throw new NotImplementedException();
    }
}
```

Buna benzer bir kavram olan interface ise bir özellik olarak düşünebiliriz. Bir oyunda mesela ateş etme özelliği adında bir interface'imiz bulunsun. Biz her silah için ayrı ayrı bu ateş etme özelliğini belirtmeliyiz. Ve ateş edilebilir silahları yasakladığımız bir kural getirirsek de bu interface'i implemente eden class'lara yasak koyarak kuralımızı yürütebiliriz.

```csharp
public interface IAtesliSilah
{
    void AtesEt();
}

public class AK47 : IAtesliSilah
{
    public void AtesEt()
    {
        Console.WriteLine("Tak tak");
    }
}

//Fonksiyonla da ateşli silahları yasaklayalım.
public void AtesYasak()
{
    foreach (var silah in TumSilahlar)
    {
        if (OyuncununSilahi is IAtesliSilah)
            continue;
        OyundakiSilahlar.Add(silah);
    }
}
```
