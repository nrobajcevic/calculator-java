LOC

Calculator.java:
- ukupno ima 188 linija koda

Start.java
- ukupno ima 26 linije koda

Zajedno imaju 214 linije koda


PREGLED I STATICKA ANALIZA

- Pregled i staticka analiza calculator.java

linija 7 – Upotreba statičkog polja finalResult može dovesti do problema u konkurentnom okruženju i otežava testiranje, jer rezultat ostaje u globalnom stanju.

linija 13–20 – Klasa Operations je statička i sadrži samo simbole. 
Metoda ToString() vraća string svih operatora, ali redosled je neuobičajen (+*-/), što može zbuniti pri parsiranju.

linija 27 – Metoda Run samo prosleđuje poziv na evaluateExpression. Može se smatrati redundantnom.

linija 33–37 – Dodavanje nule na početak izraza ako počinje sa + ili - rešava problem, ali može dovesti do neočekivanih rezultata kod izraza poput -3*2.

linija 40 – expression.split("[" + Operations.ToString() + "]") koristi regex. 
Pošto * i + imaju posebno značenje u regex-u, trebalo bi ih escape-ovati (\\*, \\+) da bi se izbegle greške.

linija 45–52 – Parsiranje operatora prolazi kroz ceo string, ali koristi expression.length() - 1. 
To znači da poslednji karakter nikada neće biti analiziran, pa se operator na kraju izraza može ignorisati.

linija 61–69 – Parsiranje brojeva u Float. Ako se pojavi nevalidan broj, vraća se "ERROR", ali bez detalja o grešci. 
Takođe, korišćenje Float može dovesti do gubitka preciznosti; Double bi bio bolji izbor.

linija 74 – Metoda Calculate koristi rekurziju. Za duže izraze može doći do StackOverflowError. Iterativni pristup bi bio sigurniji.

linija 80–100 – Logika za prioritizaciju * i / je ispravna, ali koristi result += ... umesto result = .... 
Pošto se result inicijalizuje na 0, ovo može dovesti do pogrešnih vrednosti ako se metoda pozove više puta u istom prolazu.

linija 122–142 – Slična logika za + i -. Ponovo se koristi result += ..., što može akumulirati pogrešne vrednosti.

linija 150 – Nedostaje eksplicitna obrada slučaja kada se deli sa nulom. Trenutno se dobija Infinity ili NaN, ali korisniku se ne prijavljuje greška.

- Pregled i staticka analiza Start.java

linija 9 – Deklaracija promenljive Expression sa velikim početnim slovom nije u skladu sa Java konvencijama
(promenljive se pišu malim slovom).

linija 11 – Promenljiva active se koristi kao kontrola petlje, 
ali mogla bi se zameniti direktnim while(true) i break za izlaz, što bi pojednostavilo kod.

linija 14 – Deklaracija Scanner scanIn; van petlje je nepotrebna, jer se instanca kreira unutar petlje.

linija 16 – U svakoj iteraciji petlje kreira se nova instanca Scanner. To je neefikasno i može dovesti do curenja resursa. Bolje je kreirati jedan Scanner pre petlje i koristiti ga tokom celog rada programa.

linija 19 – Poređenje Expression.equals("exit") je ispravno, 
ali bi bilo bolje koristiti equalsIgnoreCase da bi se prihvatila i varijanta "Exit".

linija 20 – Poziv scanIn.close() zatvara System.in. Nakon toga, svaka nova instanca Scanner nad System.in neće raditi.
To može izazvati IllegalStateException ako se program proširi.

linija 24 – Poziv Calculator.Run(Expression) koristi statičku metodu iz druge klase. To je u redu,
ali zbog globalnog stanja u Calculator (polje finalResult), rezultati mogu biti nepredvidivi u složenijim scenarijima.
