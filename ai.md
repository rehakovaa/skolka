### bayesovká síť
-  je to dag, kde každá proměnná je vrchol a je napojená na svého rodiče, pokud je na něm závislá (hrana: chřipka -> horečka)
    - každý uzel má podmíněnou pravděpodobnost ke svým rodičům
- vyjadřuje full joint probability distribution tak, aby to bylo víc kompaktní a přehlednější -> ne velká tabulka, ale dag

- využíváme zde řetězové pravidlo (viz. sešit) 
    - příklad: P(déšť, kluzká silnice, nehoda) = P(déšť).P(kluzká silnice|déšť).P(nehoda|kluzká silnice)

- konstrukce:
    - definujeme množinu náhodných proměnných a uspořádáme je jako $X_1$, ..., $X_n$, aby příčiny byly před důsledky
    - $X_1$ nebude mít předka a $X_i$ vyrobíme tak, že z množiny {$X_1$,...,$X_{i-1}$} vybereme nejmenší podmnožinu takovou, aby platilo $P(X_i|Parents(X_i)) = (P(X_i|množina))$
    - proč to funguje? 

- chain rule je pravidlo, které vyjadřuje společnou pravděpodobnost více náhodných proměnných pomocá podmíněné pravděpodobnosti 
$P(X_1,...,X_n) = P(X_1)*P(X_2|X_1)*...*P(X_n|X_1,...,X_{n-1})$
    - celkové společné pravděpodobnostní rozdělení můžeme získat, pokud známe pravděpodobnosti jednotlivých uzlů v síti a jejich rodiče
    - to je ta výhoda: nemsuíme znát pravděpodobnost všech kombinací, stačí znát podmíněné závislosti

příklad: 
- C: zda prší
- S: je mokrá zem
- T: vezmu si deštník

$C→S→T$ pak $P(C,S,T) = P(C)*P(S|C)*P(T|S)$

- interference mi zjistí proměnné na základě známých hodnot ostatních proměnných
    - enumerace: provede výpočet podle úplné kombinace všech možných hodnot 
        - je to ale dost náročné, využít dynamické programování, abych zabránila opakovanému počítání stejného čísla
    - eliminace proměnných: eliminuje skryté proměnné pomocí marginalizace (vysčítáme P ze všech z)
    ![alt text](image.png)
    - odmítací vzorkování (reject sampling): generuje náhodné vzorky z celé sítě a vyhodí ty vzorky, které vypadají fujky
        - sample by měl být generován z known probability distribution
        - vybereme pouze ty, co sedí evidenci, ale těch může být málo
        - funguje i bez přesného modelu
    - vážené vzorkování: prostě vylepšené vyhazování, evidence se nezahazuje, ale váží se podle toho, jak odpovídá evidenci
    - vzorek odpovídá evidenci = vzorek má stejné hodnoty, jako to, co je zadané
        - víme, že $alarm = True$, tak vyhodíme ty vzorky, co mají $alarm=False$
    - využívá se na to monte carlo technika
