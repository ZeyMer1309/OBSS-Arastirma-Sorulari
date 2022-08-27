Design Pattern
======

## Sorular

- Design Pattern kavramı nedir, neden design patternleri kullanıyoruz?
- Bir A design pattern'i bize o patterni implemente etmek için "X, Y, Z yapmalısın" diyor olsun. Ama yazdıktan sonra fark ettik ki Y aslında gereksiz ve çok küçük bir adım, sadece X ve Z adımlarını yapıp Y'yi atlayarak da kodun birebir aynı çalışmasını sağlayabiliyoruz. Asıl önemli meseleler X ve Z adımlarında yapılıyor. Y'yi yazmayıp sadece X ve Y'yi yazarsak "Kodumda A design patternini kullanıyorum" diyebilir miyim? Diyemezsem neden?
- Uçak bileti sattığım bir konsol uygulaması yazıyorum. Konsoldan kullanıcıdan sadece "müşteri tipi" diye bir değer aldığımızı düşünelim. Şu an için sadece A, B, C ve D tipleri var fakat sürekli yeni tipler gelecek bunu biliyoruz. Uçak bileti üretme algoritmamsa tamamen müşteri tipine dayanıyor. Mesela A tipi için hep cam kenarı bilet arayıp bulana kadar uçak değiştirirken B tipi için varsa cam kenarı yoksa koridor kenarı buluyorum, C ve D aynı kurallarla çalışıyor gibi gibi. Buradaki detaylar önemli değil, yazdığınız atıyorum bir ATipiBiletÜret() fonksiyonu sadece Console.WriteLine("A tipi bilet üretildi"); içerebilir, bir logic kurmanıza gerek yok. Sizden istediğim şey bu akışı önce Strategy Design Pattern kullanmadan yazmanız. Hatta yazdığınız kod hiçbir pattern kullanmasın, hali hazırda zaten biliyorsanız Factory ile filan da yazmayın. Çalışan kodu ürettiğinizde aynı kodu Strategy Design Pattern kullanacak şekilde refactor edin ve ikisini de "Neden Strategy kullandık?" sorusunun cevabıyla birlikte benimle paylaşın.
Strategy'de birden fazla class, interface vs vs işin içine girecek. Normalde best practice bunların hepsini kendi dosyaları içerisinde saklamak tabii ki ama paylaşım kolaylığı olması açısından alt alta yapıştırıp tek bir Gist ile paylaşabilirsiniz.

## Cevaplar

Design pattern kavramı ilk olarak bir mimar olan Christopher Alexander tarafından 1977 yılında mimarlıkta _sık karşılaşılan sorunların genel çözümü_ amacıyla kaleme aldığı kitap _A Pattern Language_ ile hayatımıza girmiş. Yazılım alanında da yine aynı amaçla, yani sık karşılaşılan sorunların **best practice**'i olarak kullanılıyor. Aynı zamanda sonraki yazılımcı projeye baktığında kullanılan design pattern ile konuya az biraz hakim olabiliyor ve yapması gerekenleri yapabiliyor. Bu sebepten ki bizden implemente etmemizi istediği kavramları gereksiz dahi olsa eklememiz gerekiyor. Nitekim bir parçası eksik olduğunda o design pattern'a tam manasıyla uymuş sayılmayız.

```csharp
// Startegy Pattern kullanılmadan yazdığımızda bilet üretme işlemini her bir derived class'a
// yaptırarak kendi yerine yerleştirmiş olduk.

using System;

namespace OBSS
{
    internal class Program
    {
        static void Main(string[] args)
        {
            Console.Write("Bilet tipi girin: ");
            string ticketType = Console.ReadLine();

            Ticket ticket = new WrongTicket();
            switch (ticketType)
            {
                case "A":
                    ticket = new TicketA();
                    break;
                case "B":
                    ticket = new TicketB();
                    break;
                case "C":
                    ticket = new TicketC();
                    break;
                case "D":
                    ticket = new TicketD();
                    break;
            }
            ticket.CreateTicket();
        }
    }

    abstract class Ticket
    {
        public abstract void CreateTicket();
    }

    class TicketA : Ticket
    {
        public override void CreateTicket()
        {
            Console.WriteLine("A tipi bilet üretildi");
        }
    }

    class TicketB : Ticket
    {
        public override void CreateTicket()
        {
            Console.WriteLine("B tipi bilet üretildi");
        }
    }

    class TicketC : Ticket
    {
        public override void CreateTicket()
        {
            Console.WriteLine("C tipi bilet üretildi");
        }
    }

    class TicketD : Ticket
    {
        public override void CreateTicket()
        {
            Console.WriteLine("D tipi bilet üretildi");
        }
    }

    class WrongTicket : Ticket
    {
        public override void CreateTicket()
        {
            Console.WriteLine("Hatalı giriş yaptınız. Bilet üretilemedi.");
        }
    }
}
```

Kodu yazdıktan sonra strategy pattern'i araştırmaya başladım. Sanırım bilmeden strategy pattern'a benzeyen bir şey yazmışım. :) Şimdi strategy pattern'a uygun olarak revize edeyim kodu. Aradaki farklar sadece isimlendirme ve Context sınıfında.

```csharp
using System;

namespace OBSS
{
    internal class Program
    {
        static void Main(string[] args)
        {
            Console.Write("Bilet tipi girin: ");
            string ticketType = Console.ReadLine();

            Context context;

            switch (ticketType)
            {
                case "A":
                    context = new Context(new TicketStrategyA());
                    break;
                case "B":
                    context = new Context(new TicketStrategyB());
                    break;
                case "C":
                    context = new Context(new TicketStrategyC());
                    break;
                case "D":
                    context = new Context(new TicketStrategyD());
                    break;
                default:
                    context = new Context(new WrongTicketStrategy());
                    break;
            }

            context.ContextAction();
        }
    }

    abstract class TicketStrategy
    {
        public abstract void CreateTicket();
    }

    class TicketStrategyA : TicketStrategy
    {
        public override void CreateTicket()
        {
            Console.WriteLine("A tipi bilet üretildi");
        }
    }

    class TicketStrategyB : TicketStrategy
    {
        public override void CreateTicket()
        {
            Console.WriteLine("B tipi bilet üretildi");
        }
    }

    class TicketStrategyC : TicketStrategy
    {
        public override void CreateTicket()
        {
            Console.WriteLine("C tipi bilet üretildi");
        }
    }

    class TicketStrategyD : TicketStrategy
    {
        public override void CreateTicket()
        {
            Console.WriteLine("D tipi bilet üretildi");
        }
    }

    class WrongTicketStrategy : TicketStrategy
    {
        public override void CreateTicket()
        {
            Console.WriteLine("Hatalı giriş yaptınız. Bilet üretilemedi.");
        }
    }

    class Context
    {
        TicketStrategy ticketStrategy;

        public Context(TicketStrategy ticketStrategy)
        {
            this.ticketStrategy = ticketStrategy;
        }

        public void ContextAction()
        {
            ticketStrategy.CreateTicket();
        }
    }
}
```