### 3. přednáška 
#### morfologické značky 
- pro izolační jazyky jsou pos značky jako identifikátory všech morfologických vlastností slovního druhu 
    - nikoli jako kombinace všech jednotlivých kategorií jako slovní druh, rod, číslo, apod.
    - pos - part of speech
- pos značky dělíme na dva typy - otevřená a uzavřená třída 
    - uzavřené slovní druhy se dají popsat výčtem (předložky, pomocná slova a spojky)
    - otevřené slovní druhy tak popsat nejde, protože jejich seznam nikdy není kompletní, protože jazyk se vyvíjí
- tagset - sada používaných značek
    - liší se nejn pro různé jazyky, ale také pro různé lingvistické teorie nebo aplikace v rámci jednoho jazyka 
        - pro angličtinu jsou dva korpusy a každý z nich používá jiný počet značek 
- universal depdendencies - snaha sjednostnit značkování pro morfologické a syntaktická lingvistická data
    - nemůže se jít příliš do hloubky 

#### česká morfologie 
- prní analyzátory na konci 70. let, protože už existovaly textové editory a pro češtinu ještě neexistoval spell checking (a bylo by fajn to vyvinout)
- morfologický analyzátor 
    - potřebujeme slovník základních stavů slov (lemmat), který musí být dost rozsáhlý
    - seznam příslušných koncovek a co ty koncovky říkají 
- slovník, který by měl pokrývat většinu slovní zásoby, by měl mít 200 až 300 tisíc hesel 
    - ne všechny jsou používaná, dost z nich jsou specifická 
    - ten slovník, co postupně vznikal, tak v podstatě pokrývá 800 tisíc slov
        - spoustu slov je odvozených od stejného základu, a toho hajič využíval (ale úplně nevím jak)
        - je složité pojmout všecha slova v češtině, protože se vždycky najde nějaké slovo, co tam zrovna není uloženo 
    - značky byly nejdříve kompaktnější (dost podobné part of speech značkám, obashovali jenom to info, co bylo relevantní, je to ale horší na automatické zpracování, protože není úplně jisté, co tam najdeme při analýze)
        - hajič změnil ten způsob značkování na poziční, který obsahoval systém patnácti pozic, kde každá informace měla přesně dané místo 
            - je tam patnáct pozic, ale jenom třináct je využívaných, dvě jsou rezervní 
        - je to podobné tomu, co bylo v derinetu (ty šílené značky u každého slova)
        - zadám tam lemma (v jakém kolik tvaru - rezignoval a vyhodí mi to rezignovat) a ono mi to vyhodí všechny relevantní informace 
            - když to bude mít více významů, tak to vyhodí všechna lemmata 
            - třeba 'na' může být před čvrtým a šestým pádem, a tak při analýze vyhodí dvě značky
            - obsahuje i tečku, protože je to důležitá část českého jazyka 
            - dobré je na to MorphoDiTa

#### činnosti využívající morfologii
- samotná analýza, která je předpokladem pro další zpracování textu
    - výsledkem je seznam lemmat a k nim přiřazené morfologické značky
    - vrátí výsledek, který je sto pro správný 
- morfologické značkování 
    - značkovací systém, který vybírá značku v daném kontextu
    - hodí se pro to statistické hodnoty a ještě lepší je to se strojovým učením 
    - pro češtinu je 98% úspěšnost 
        - v angličtině je to přes 99 %, protože mají méně značek a jednodušší jazyk
        - jak se to používá? vezme se text na vstupu a pro kolik slov z toho bylo určeno správně, tak to je ta úspěšnost
            - započítávají tam i ty slovní druhy, kde žádná jednoznačnost není, takže to zlepšuje tu procentuální úspěšnost 
        - v češtině (a jiných flektivních jazycích) je to složitější než v angličtině 
- lemmatizace
    - proces výběru správného základního tvaru, ze kterého byl odvozen daný vstupní tvar
        - klíčová operace pro vehledávání v textech 
- stemming  
    - jednoduchý způsob, jak se dostat ke kmeni, a to je odříznutí koncovky (je to pro jazyky, kde se nemění písmena ve kmeni (pro češtinu to úplně nejde, pro angličtinu celkem ano))
- doteď to bylo mám text a chci ho analyzovat
- generování 
    - mám základní tvar a info, co z toho chci dostat (co chci dosadit do nějaké věty) a vygeneruju ten správný tvar (pro generaci byl využit ten základní tvar a ta značka)

#### částečná morfologická desambiguace 
- vytvoří se velká sada pravidel, které odstraní všechny nesmyslné značky, které v tu chvíli nejsou potřebné, a potom to hodit do té statistické části
    - morfologie samotná se nedá dělat pomocí pravidel, ale jsou dobrou pomocí jako heuristika 
- pravidla byla hodně specifická - týkala se velmi specifických příkladů, které nemohly nastat
    - pravidla popsala kontext - v tomhle kontextu nesmí/musí něco nastat
        - dvě části
            - popis kontextu
            - akce (odstranění nevhodných značek anebo vybrat jenom ty, co splňují ty zaedané podmínky)
- ty pravidla zlepšily ten výsledek jenom o dvě desetiny ve značkování pomocí statistických metod 
- role jazykového experta se dá nahradit daty 
    - to, co je složité pro experta, je složité i pro data a vice versa
- příkladem je to, že v 'se hraje' to 'se' musí být zvratné zájmeno a ne předložka, takže se vyškrtají ty značky, které mi tvrdí, že to je předložka 
- využití to našlo díky tomu, že když se u nějakého slova odstranily všechny značky
    - často to znamenalo, že ta věta byla gramaticky špatně 

#### značkování 
- výběr nejpravděpdobonější morgologické značky pro daný slovní tvar
- neřešíme pro jednotlivé slovní tvary izolovaně, ale hledané nejpravděpodobnější posloupnost značek pro celou větu 
- využívá se tam pravděpodobnost a markovovy řetězce 

#### skryté markovovy modely 
- standartní metoda anaylýzy řad na základě kontextu, je to posloupnost rozhodnutí, která jsou na sobě závislá
- markovova hypotéza - kontext je možnost zkrátit na délku, která je spočitatelná
- slovo "skryté" reprezentuje fakt, že některé vlastnosti nejsou pozorovatelné (ve větě vidíme slova, nikoliv ty morfologické značky, které jim přiřazujeme)
- jedná se v podstatě o stochastický konečný automat

- formálně:
    - potřebujeme znát stavy (to jsou ty značky)
    - matice přechodových pravděpodobností 
        - jaká je pravděpedobnost, že tady je přídavné slovo v nějakém tvaru a za ním bude podstatné jméno v nějakém tvaru 
    - pravděpodobnsoti pozorování 
        - referují k tomu, jak je pravděpodobné, že se to slovo v té větě objeví (jak často ho používáme)
    - počáteční distribuce pravděpodobnosti
        - jaká je pravděpodobnost, že ta věta bude začínat nějakým slovem (některé mohou být nulové)

#### tři základní úlohy HMM
- dekódování
    - nalezení sekvenci značek
    - hledáme nejpravděpodobnější sekvenci skrytých stavů 
- rozpoznávání
    - použití: registrační značky aut
        - vzít obrázek spz a přiřadit k tomu nějakou danou kombinaci
- učení statistického modelu
    - údaje o těch statistických údajech se musí vyčíst ze statistických dat 
- na to všechno jsou algoritmy 
- dekódování (viterbiho algoritmus) - v podstatě hledání nejkratší cesty v grafu

#### kontrola překlepů 
- základní požadavky:
    - musejí být nalezeny všechny překlepy a nalezené musejí být opraveny (nabídnout správné řešení)
        - musíme zkontrolovat kontextové podmínky korigované verze (jestli je to fakt ta správná možnost)
    - rozeznat slova, která systém nezná od slov, která jsou špatně (ty, co nezná, by neměl hlásit)
    - systém by neměl dávat falešná chybová hlášení
    - korektura musí být co nejvíce automatická 
    - čas zpracování musí být velmi krátký 
- podmínky jsou dost složité splnit všechny najednou 
- když je to komerční aplikace, tak nás nezajímá úplně, jestli je to špatně nebo dobře, ale co si o tom myslí ten uživatel
