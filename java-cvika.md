# Java (úkoly ze cvičení)

Násedují úkoly ze cvičení (2019/2020), kde to bylo možné tak se správnou odpovědí a vysvětlením. Důležité příklady, které bývají na zkouškách, jsou označeny jako *zkouškový příklad*.

## Cvičení 1

**Co se vypíše?**

```java
Integer i1 = new Integer(1);
Integer i2 = new Integer(1);
if (i1 == i2)
    System.out.println("ANO"); 
else 
  	System.out.println("NE");
```

*Řešení:* NE

`==` porovnává pro referenční typy (což jsou všechny objekty, i `Integer`) pouze ukazatele do paměti, ne hodnoty. `i1` a `i2` jsou v tomto případě různé objekty s různými pozicemi v paměti.

**Co se vypíše?**

```java
public class Overflow {
    public static void main(String[] argv) {
        int b = 2147483647; 
        System.out.println(b); 
        b = b + 1; 
        System.out.println(b);
    } 
}
```

*Řešení:* 2147483647 -2147483648

2147483647 je Integer.MAX_VALUE, když k ní přičteme jedničku, dojde k *overflow* (jede se opět "od začátku" intu).

**Co se vypíše?**

```java
class URL {
    public static void main(String[] argv) {
        System.out.println("url:");
        http://google.com/
        System.out.println(":url");
    }
}
```

*Řešení:* url: :url

V `http://google.com/` se vše po `//` bere jako komentář a `http:` se bere jako label. Labely fungují podobně jako v Pascalu, lze na ně skočit pomocí `continue`.

**Co se vypíše?**

```java
public class Swap {
    public static void main(String[] argv) {
        int x = 10;
        int y = 20;
        x ^= y ^= x ^= y;
        System.out.println(x);
        System.out.println(y);
    }
}
```

*Řešení:* 0 10

`a ^= b`  odpovídá `a = a ^ b`. `a ^ b` je operace bitwise `XOR`, při níž se `a` a `b` berou jako bity (čili pokud to jsou integery, berou se převedené do binární soustavy). Kratší z nich se zepředu doplní nulami, aby byla obě čísla stejně dlouhá. Poté se porovná i-tý bit z `a` s i-tým bitem z `b` pomocí operace XOR.  

`x ^= y ^= x ^= y` lze pak přepsat jako `x = x ^ (y = y ^ (x = x ^ y))`, což je výraz, který se vyhodnocuje zleva. Po dosazení vyjde `x = 10 ^ (y = 20 ^ (x = 10 ^ y))` atd.

**Co se vypíše?**

```java
class ForCycle {
    public static void main(String[] argv) {
        int j = 0;
        for (int i = Integer.MAX_VALUE - 10; i <= Integer.MAX_VALUE; i++) {
            j++;
        }
      	System.out.println(j);
    }
}
```

*Řešení:* Nekonečný cyklus (nevypíše se nic)

`int i` nikdy nebude větší než `Integer.MAX_VALUE`, protože je to jeho horní hranice.

## Cvičení 2

**Doplňte deklaraci `i`, aby se vypsalo `ANO`**

```java
if (i == -i && i != 0) { 
    System.out.println(“ANO“);
} else { 
    System.out.println(“NE“);
}
```

*Řešení:* `i = Integer.MIN_VALUE` 

Když ho znegujeme, dostaneme `Integer.MAX_VALUE + 1`, což je zase `Integer.MIN_VALUE`.

**Co se vypíše?**

```java
public class LoopTest {
	public static void main(String[] argv) {
		int START = 2000000000;
		int count = 0;
		for (float f = START; f < START + 50; f++) {
			count++;
		}
		System.out.println(count); 
    }
}
```

*Řešení:* 0

Má to co dočinění s *floating-point* čísly. `START` je sice na začátku `int`, kvůli porovnání s `f` je však převeden na `float`. `START` jen o málo menší než $2^{31}$, proto v jeho binárním zápisu bude určitě 31. bit jednička. `float` čísla jsou sice 32 bitová, ale 1 bit z toho je na znaménko, dalších 8 je na exponent a tak na samotné číslo zbude pouze 23 bitů. 

Protože k uložení start potřebujeme mít 31. bit nastavený na 1, při převodu na `float` si ze `START` necháme následujících 23 bitů: [31., 30., 29., ..., 8.] — jinými slovy prvních sedm bitů ze `START` odstřihneme, neboť se nám do floatu nevlezou. Protože přičtená 50 se vleze do prvních sedmi bitů, a my těchto sedm bitů poté odstřihneme, přičtení 50 se vůbec neprojeví a platí `START == f == START + 50` (pokud je `START` převeden na `float`).

## Cvičení 3

**Co se vypíše?**

```java
public class Test01 {
	public static void main(String[] argv) {
		System.out.println(test()); 
    }
   	public static boolean test() {
		try {
       		return true;
     	} finally {
       		return false;
     	}
	} 
}
```

*Řešení:* `false`

`finally` přebíjí většinu věcí, i `return`.

**Co se vypíše?**

```java
public class Test01 {
	public static void main(String[] argv) {
		try {
       		System.out.println("Hello world!");
			System.exit(0);
     	} finally {
       		System.out.println("Goodbye");
     	}
    }
}
```

*Řešení:* "Hello world!"

`finally` přebíjí většinu věcí, ale `System.exit()` je jedna z výjimek — prostě rovnou ukončí průběh programu. Kromě něj takto funguje i `halt` a pak už většinou jen pády JVM nebo samotného operačního systému.

**Co se vypíše?**

```java
class ParamsTest {
    public ParamsTest(Object o) {
        System.out.println("ParamsTest(Object o)");
    }
    public ParamsTest(long[] a) {
        System.out.println("ParamsTest(long[] a)");
    }
    public static void main(String[] argv) {
        new ParamsTest(null);
    }
}
```

*Řešení:* ParamsTest(long[] a)

Podle skutečných parametrů se vybere ta nejspecifičtější vyhovující implementace (zde `long[] a`).

**Co se vypíše?** (zkouškový příklad)

```java
class A { 
    int x = 1; 
} 

class B extends A { 
    int x = 2; 
    public void foo() { 
        System.out.println(this.x); 
        System.out.println(super.x); 
        System.out.println(((A)this).x); 
    } 
    public static void main(String[] args) { 
        B b = new B(); 
        b.foo(); 
   } 
}
```

*Řešení:* 2 1 1

Statické metody se koukají pouze na typ objektu, ne na jeho hodnotu; podle toho se pak zavolá konkrétní implementace. Viz také [tato otázka na Stack Overflow](https://stackoverflow.com/questions/60065506/inherited-attribute-method-static-method-behavior?noredirect=1).

## Cvičení **4**

**Co program udělá?** (zkouškový příklad)

```java
public class Null {
	public static void main(String[] argv) {
        ((Null) null).hello(); 
    }
	public static void hello() {
    	System.out.println("Hello world!");
	} 
}
```

*Řešení:* vypíše se "Hello world!"

Že se třída jmenuje `Null` je v pořádku. Poté sice voláme `null.hello()`, ale `hello()` je statická metoda, čili vůbec nekouká na reálnou hodnotu objektu — pouze na jeho typ (a to je `Null`). Vše tedy funguje, jak má.

**Co program udělá?**

```java
class Test01 {
    public static void main(String[] argv) {
        run();
        System.out.println("Konec");
    }

    public static void run() {
        try {
            run();
        } finally {
            run();
        }
    }
}
```

*Řešení:* Myslím, že vyhodí `StackOverflowError`.

**Co program vypíše?**

```java
class A {
	public String className = "A"; 
}

class B extends A {
	private String className = "B";
}

public class Test02 {
	public static void main(String[] argv) {
		System.out.println(new B().className); 
    }
}
```

*Řešení:* Nepřeloží se, ve třídě Test02 je chyba.

Deklarace `className` v `B` schová atribut `className` z `A ` (doslova se to jmenuje *field hiding*). To, že je sama `private` na věci nic nemění. Protože původní `className` z `A` je schovaná a `className` z `B`  je zase `private`, je chyba vůbec se snažit tohoto atributu dosáhnout.

## Cvičení 5

**Co se vypíše?**

```java
public interface Test {
	public static void main(String[] argv) {
		System.out.println("Hello"); 
    }
}
```

*Řešení:* Je to úplně v pořádku, vypíše se Hello.

V interfacu mohou totiž být i statické metody.

**Co se vypíše?** (zkouškový příklad)

```java
public enum Test {  
	RED, GREEN, BLUE;

	public static void main(String[] argv) { 
    	System.out.println("Hello");
    }
}
```

*Řešení:* Je to úplně v pořádku, vypíše se Hello.

`enum` je v podstatě jen vylepšená třída, proto může obsahovat statické metody. Nemůže však dědit, neboť implicitně již dědí od `java.lang.Enum`. Může dokonce obsahovat i abstraktní metody, pak je ale musí implementovat každý z prvků enumu.

## Cvičení 6

**Co se vypíše?**

```java
public class Greeter {
	public static void main (String[] args) {
		String greeting = "Hello world";
		for (int i = 0; i < greeting.length(); i++) {
			System.out.write(greeting.charAt(i)); 
        }
	} 
}
```

*Řešení:* Nevypíše se nic.

`PrintStream.write()` sice na stream přidá dané znaky, ale stream pak automaticky nevyprázdní do konzole — proto nejde nic vidět. Naproti tomu `println()` se o toto stará, čili kdybychom za loop přidali `System.out.println("");`, vypsalo by se už `Hello world`.

**Co se vypíše?**

```java
class Slasher {
    public static void main(String[] argv) {
        String fullClassName = "cz.cuni.mff.java.io.Slasher";
        String fileName = fullClassName.replaceAll(".", "/") + ".java";
        System.out.println("Trida " + fullClassName + " musi byt v souboru " + fileName);
    }
}
```

*Řešení*: Trida cz.cuni.mff.java.io.Slasher musi byt v souboru ///////////////////////////.java

`String.replaceAll()` bere jako první argument `regex`, ne obyčejný string. Proto `.` je interpretováno regex enginem jako "jakýkoli znak" a všechny znaky jsou proto nahrazeny lomítkem.

## Cvičení 7

**Co program udělá?**

```java
public class TestString {
    public static void main(String[] args) {
        String s = new String("Hello world");
        System.out.println(s); 
    }
}

class String {
    private final java.lang.String s; 
    public String(java.lang.String s) {
        this.s = s; 
    }
    public java.lang.String toString() {
        return s; 
    }
}
```

*Řešení:* Přeloží se, ale při runtimu vyhodí chybu, že mu chybí metoda `main`.

`main` má totiž v této chvíli mít hlavičku `public static void main(java.lang.String[] args)`, protože `String` je teď ta naše nová třída a ne původní `String`.

**Lze třídu `B` nadeklarovat tak, aby program vypsal `false`? (bez přepsání `equals`)** 

```java
public class A {
	public static void main(String[] args) {
        B b = new B();
		System.out.println(b.equals(b)); 
    }
}
```

*Řešení:* Ano, ale je to dost podvod.

```java
class B {
   public B() {
       System.out.println(false);
		System.exit(0); 
   }
}
```

## Cvičení 8

**Co se vypíše?**

```java
public class Test01 {
    public static synchronized void main(String[] a) {
        Thread t = new Thread() {
            public void run() {
                pong();
            }
        };
        t.start();
        System.out.println("Ping");
    }

    static synchronized void pong() {
        System.out.println("Pong");
    }
}
```

*Řešení:* Ping Pong

Obě metody jsou `static synchronized`, čili berou zámek přímo od třídy `Test01`. Když už jsme v `main`, má současné vlákno zámek (protože vstoupilo do `synchronized` bloku) a tak se vlákno `t` (potažmo metoda `pong()`) spustí až poté, co skončí to naše vlákno.

**Co se vypíše?**

```java
class SelfInterruption {
    public static void main(String[] args) {
        Thread.currentThread().interrupt();
        if (Thread.interrupted()) {
            System.out.println("Interrupted: " + Thread.interrupted());
        } else {
            System.out.println("Not interrupted: " + Thread.interrupted());
        }
    }
}
```

*Řešení:* Interrupted: false

`Thread.interrupted()` totiž nejen vrátí současnou hodnotu `.interrupted()`, ale také ji resetuje na `false`. Kdyby se místo toho použilo `this.isInterrupted()`, který po vrácení žádný reset neprovádí, vypsalo by se *Interrupted: true*.

## Cvičení 9

**Co se vypíše?**

```java
public class Test01 {
    private static java.util.Random rnd = new java.util.Random();

    public static void main(String[] args) {
        StringBuffer word = null;
        switch (rnd.nextInt(2)) {
            case 1:
                word = new StringBuffer('P');
            case 2:
                word = new StringBuffer('G');
            default:
                word = new StringBuffer('M');
        }
        word.append('a');
        word.append('i');
        word.append('n');
        System.out.println(word);
    }
}
```

*Řešení:* ain

V `case` nejsou breaky, takže word je vždy nakonec nastaven na `new StringBuffer('M')`. Protože `StringBuffer` dostal `char` (potažmo `int`) bere to jako hranici kapacity (ne jako první písmeno — to by musel dostat `"M"`).

**Co se vypíše?**

```java
public class Test02 {
	public static void main(String args[]) {
		System.out.println("H" + "a");
		System.out.println('H' + 'a'); 
    }
}
```

*Řešení:* Ha  169

`char` + `char` je v tomto případě interpretováno jako součet intů. Pro zajímavost, `println("" + 'H' + 'a')` by vypsalo skutečně `Ha` [[*laughs in javascript*](https://www.destroyallsoftware.com/talks/wat)].

## Cvičení 10

**Lze definovat `i` tak, aby byl cyklus nekonečný?**

```java
while (i != i) { }
```

*Řešení:* Ano, `Double.NaN` se nerovná ničemu, ani sám sobě.

**Co se vypíše?**

```java
public class Increment {
	public static void main(String[] args) {
		int j = 0;
		for (int i = 0; i < 100; i++) {
			j = j++; 
        }
		System.out.println(j); 
    }
}
```

*Řešení:* 0

(Myslím, že) `j++` vrátí 0 a nastaví `j` na 1; `j = j++` pak vezme 0 z `j++` a uloží jí do `j`.

## Cvičení 11

**Lze od této třídy (bez použití reflection API) vytvořit další instance (kromě instance v atributu `INSTANCE`)?**

```java
public class Dog implements Serializable {
	public static final Dog INSTANCE = new Dog();
	private Dog() { }
	public String toString() {
		return "Woof"; 
    }
}
```

*Řešení:* Leda bychom přidali něco do třídy samotné (jinak máme příklad tzv. **singleton pattern**, což je třída pouze s jednou instancí, zde konkrétně `INSTANCE`).

```java
public class Dog implements Serializable {
	public static final Dog INSTANCE = new Dog();
	private Dog() { }
	public String toString() {
		return "Woof"; 
    }
    
    public Dog getDog() {
        return new Dog();
    }
}

Dog d = Dog.getDog();
```

V tomto případě naopak používám tzv. **factory pattern** (máme metody, které nejsou konstruktory, ale přesto *vyrábějí* a vrací objekt své třídy).

## Cvičení 12

**Lze napsat deklaraci proměnné `i` tak, aby následující cyklus byl nekonečný?**

```java
while (i != i + 0) { }
```

*Řešení:* Ano, např. `String i = "a"`, potom `i + 0 == "a0"`.

**Lze napsat deklaraci proměnných `i` a `j` tak, aby následující cyklus byl nekonečný?**

```java
while (i <= j && j <= i && i != j) { }
```

*Řešení:* Ano, například `i = new Integer(1)` a `j = new Integer(1)`.

Protože se jedná o referenční typy (objekty), výraz `i != j` je pravda, protože porovnává umístění v paměti a ne samotné hodnoty `i` a `j`.

