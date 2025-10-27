### 4. přednáška

#### kontrola překlepů
- 2 základní metody     
    - 1. porovnání řetězců se slovy ve slovníku 
        - seznam všechn možných slovních tvarů daného jazyka (worldlist)
        - slovník lemmat a morfologická analýza
        - výhoda: spolehlicé a jednoduché
        - nevýhoda: pomalé, nerozezná chybná slova od neznámých, náročné na kvalitu slovníku a místo
    - 2. srovnávání skupin znaků (dvojice, trojice,...)
        - hledání nedovolených komibnací znaků 
        - výhoda: rychlé a nezávislé na slovníku
        - nevýhoda: velmi neúplné (často jsou to chybná spojení, která jsou dohledatelná v jiných slovech)

#### kontrola překlepů
- při měření vzdálenosti nabízených řešení potřebujeme vhodnou metriku
    - nejjednodušší - levenhsteinova metrika 
- komerční řešení 
    - běhalo to rychle (levenhsteinova je na to dobrá)
    - přesnost (nenalezené chyby stejně uživatel nevidí) a rychlost
    - kontrola na pozadí je důvod, proč je to teď tak populární 
    - uživatelsky vložené slovníky nemají zabudouvanou morfologii, je třeba do nich vkládat všechny tvary 
    - omezená možnost rozvoje tradičních metod, je potřeba větší zaměřenost na uplatnění kontextu

#### kontextové metody kontroly překlepů

#### příklad kontextového spellcheckeru 
- wilnSpell využívá metody strojového učení 
    -výsledky závejí na trénovacím korpusu, podstatně defradují mimo doménu trénovacích textů
        - trénování funguje v úzce vymezené oblasti (třeba na květnatých větách, takže by to krachlo na krátkých větách)
    - zdrojem trénovacích dat je Brown korpus a Wall Street Journal 

### příklady využití pravidelnosti české morfologie
- systém měl vyhledávat výskyty zadných klíčových výrazů v rozsáhlé množině textů, aniž by k tomu potřeboval kompletní slovník českých lemmat 
- klíčová část je jazykový modul
    - neobsahuje žádný rozsáhlý slovník
    - založen na retrográdním slovníku - mnoho slov, které mají v základním tvaru stejný koncový segmet, se stejně skloňuje
    - výjimky jdou do vlastního slovníku, jsou jich řádově pouze stovky (max tisíce)
- systém ASIMUT

#### algoritmus systému ASIMUT
- bylo jenom ascii bez českých písmen (potřebovalo se vyřešit, jak je tam narvat)
    - udělalo se to pomocí čísel -> 7 bylo ů, 3 byl háček, 2 byla čárka
- porovnání jednotlivých znaků základního tvaru slova odzadum dokud není možné jednotznačně určit, jak se slovo sloňuje
    - vytvářejí se v průběhu alternativní kmeny
- když to projde celým slovem, u kterého nejsme schopni určit, jestli je to rod mužský životný nebo neživotný 
    - trávník nebo právník 
- když je na konci 'ý', tak je to podle mladý (krom úterý, čehý a prý)

#### problémy ASIMUTu
- není vždy možné jednoznačně určit vzor, ani když se slova liší třeba jen první hláskou
- přílis hrubá klrasifikace (málo vzorů - 31 pro podstatná a přídavná jména)
- malý retrográdní slovník 

#### automatická indexace českých dokumentů - systém mozaika
- potřebuje slovníky klíčových slov, dokumenty jsou pak indexovány těmito slovy, v úvahu se bere četnost výskytu těchto klíčových slov
- některé koncovky a přípony nesou význam (mají semantickoý výhnam)
    - -er, -or v angličtině jsou konatelé děje; -tion činnost; -ity -ness vlastnost
    - -ič, ač, -čka, -ér, -or, -dlo a bla bla bla, to jsou nástroje a přístroje; -ace, -kce, -áž, -ní, -za jsou procesy nebo činnosti
- budeme chytat klíčová slova podle jejich koncovek
- pro texty elektrotechniky je potřeba 800 přípon, pro technickou je potřeba 2 tisíce
- je potřeba provést lematizace (které tvary patří ke kterému lemmatu)
- lemmata se filtrovala (odtrhávaly se koncovky, a pokud ten zbytek byly dvě písmena, tak to není fungl kmen nebo je to divná změť hlásek, co nedává validní slovo)
- syntaktická analýza jmenných skupin pomocní jednoduché gramatiky v jazyce systémů Q pomůže odhlait několikaslovné termíny `:-)`
    - zesilovač obsah textu charakterizuje méně než operační zesilovač TESLA
- přiřazení vah na základě výskytu termínů v různě důležitých místech textu (nadpis, první a poslední odstavec, první a poslední věty v odstatnícvh mají vyšší váhu než výskyty urpostřed odstavců)
    - některé výrazy mají velmi vysoké váhy, pokud je ten text dost dlouhý 
        - docházelo by k disproporcím podle toho, jak dlouhé ty texty byly -> muselo se to normalizovat (výraz s největší hodnotou měl 100 a podle toho se srazily ty ostatní výrazy)
- bralo se deset výrazů s nejvyšší vahou