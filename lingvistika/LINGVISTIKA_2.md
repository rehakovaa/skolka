### přednáška 2 - morfologie

#### historie
- nejstarší odvětví lingvistiky
- panini
    - sepsal morfologii pro sanskrt 
    - složková gramatika  
- de Saussure 
    - course in general linguistics
- antická řečtina a latina 
    - flektivní jazyky (spoustu skloňování a časování)

#### základní info
- studium vnější suktury slov 
- lexikologie 
    - slova studována jako základní jednotky slovní zásoby
- lexikografie
    - sestavování slovníků
    - někdy se dá slovo jednoho jazyka přeložit více výrazy a když využiju jeden ten výraz, tak v tom původní jazyku bude spoustu jiných výrazů
        - ten vztah mezi jazyky není 1 ku 1

- základní jednotkou je morfém
    - nejmenší znaková jednotka, která nese nějaký význam
    - význam může být různého typu
        - lexikální (nesou význam)
        - gramatické (koncovky, určují gramatickou roli slovního tvaru)
    - uváděl příklad 'zahradou'

- skloňování - deklinace 
- časování - konjugace 

- tvaroslovné duplety - stejná gramatická funkce, ale mají jiný tvar
    - vezmu lemma a k tomu dostanu kombinaci rodu, čísel a tvaru a mechanismus má vyhodit správný tvar koncovky
- opačný problém - homonyma 
    - problém při analýze
    - žena - přechodník od hnát 
    - už - rozkazovací tvar pro užít 
- jak se zvětšuje rozsah slovníku, tak se tam dostanou divné homonymní tvary
    - jak (vztažné, tázací,... zájmeno, podstatné jméno)
        - analýza nepracuje s pravděpodobností, tak je těžké rozeznat, co tam je 
- alternace 
    - nejde slovo rozsekat na morfémy a poté je jednoduše dohledávat, protože se jeho mění
    - matka - matce, matčin, matek
    - já -> mně/ mě, je to nepravidelné, je potřeba uvést všechny
- plnovýznamové slovo 

#### morfologická topologie jazyků 
- chceme dělat analýzu a je potřeba vybrat tu nejlepší metodu
- množná čísla 
    - japonština: nulová změna - ze slovního tvaru se to nepozná, jde to z kontextu
    - tagalog: funkční slovo - přidá se tam slovo, které zahlásí, že to znamená více
    - turečtina: afixační - používá se předpona nebo přípona
        - morfém má jenom jeden úkol
    - angličtina: zvukový rozdíl (man/men)
    - malajština: reduplikace - anak (dítě) -> anak-anak
- plurál může být jeden morfém, ale někdy jich může být víc
- obrázkové písmo
    - znaky pro to, jak se to má skloňovat 

#### morfologická typologie jazyků
- jazyky analytické
    - jazyk izolační
        - vietnamština (pod vlivem francouzské kolonizace přešla na modifikovanou latinku, ale předtím měla znakového písmo), čínština
        - obrázková písma
        - je to prostě jenom morfém, není tam k tomu nic připojeného 
        - slovo == morfém 
    - trochu tam spadá angličtina
- jazyky syntetické
    - slovo je tvořeno více morfémy 
    - pokud má morfém více funkcí - flektivní jazyk
        - slovanské jazyky
    - morfém má jednu funkci - aglutinační
        - maďarština, japonština
- jazyky polysyntetické 
    - slovo == věta
    - pospojuje se to dohromady do jednoho dlouhého slova
    - v oceánii 

- těžko se dá najít jazyk, který by měl charakteristiku čistě jedné z těch kategorií 
    - ovlivňují se díky tomu, že spolu interagují 

#### přístupy ke zpracování morfologie 
- morfologie založená na jednotlivých morfémech (aglutinační)
    - je to jako navlékat korálky na niť 
- morfologie založená na lexémech
    - základní tvar a pravidla, jak se dají odvodit další tvary a získat z toho nový slovní tvar 
    - two level morfology 
    - v angličtině máme sun -> suns
- morfologie založená na slovech (vzorech)
    - ty vzory jsou speciální slova, která byla vytipována jako vzor pro skloňování a časování všech ostatních slov 
    - od 35 do 250 podle toho, jak moc jednoznačné to chceme (pro učení jazyka stačí těch 35)

#### two-level morphology 
- způsobila převrat ve zpracování morfologie 
- předtím bylo složité se ke zpracování jazyků přistoupit systematicky, protože byly velmi obšírné
- využitý nejjednodušší typ automatu 
    - založeno na konečně stavových automatech
- z hlediska lingvistického vytvořili systém, kam šli dodat pravidla pro konkrétní jazyky 
- reagovali na tradiční problém - do té doby se většina morfologických prací se zabývala generováním
    - tvořili slova, která byla správně seskládaná, ale nedávaly smysl
    - vybírala se slova ze stromečku možností
    - šli jedním směrem, který se hrozně větvil, ale kdyby se vzal ten druhý směr, tak je to o dost méně rozvětvené 
    - uplatnění pravidel bylo sekvenční
        - potom a potom a potom
        - v two-level se snažili formulovat pravidla nezávisle, aby se dali provádět paralelně a ne sekvenčně

- dvě úrovně - lexikální a povrchová 
- pravidla se aplikují paralelně a můžou se tam dodávat podmínky, které jsou vztažené k jedné nebo oběma úrovněmi 
- lexikální vyhledávání - trie (jdeme po jednotlivých písmenech)
    - jakmile narazíme na slepou větev, tak víme, že tudyma nejde pokračovat

- každý se poté snažil tuhle metodu použít pro svůj jazyk
    - u nás v češtině se o to nikdo nepokusil, protože máme ten druhý typ
- první systematicky široce použitelný nástroj pro analýzu 

#### morfologie založená na vzorech
- potřebujeme   
    - slovník (rozsáhlý celý jazyk - pokrývající všechny kmeny a info o skloňovacím vzoru)
    - seznam možných koncovek a pro každou koncovku napsat v jaké kombinaci vzoru, rodu, čísla a pádu se tahle koncovka používá

- algoritmus
    - kačena - odtrhnu všechnu koncovky a dostanu kačen, kače, kač a pouze první je kmen ve slovníku a je to rodu ženského, tak se budeme zabývat jenom tímhle tím 
        - ze seznamu u příslušné koncovky pak vybereme jenom čísla a pády uvedené pro žena 
    
    - silně zjednodušený, protože se v kmenu mohou měnit samohlásky a souhlásky 