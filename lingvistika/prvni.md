### přednáška 1

#### úvod
- obor zabývající se popisem vlastnostní přirozených jazyků a jejich automatickým zpracováním (automatické systémy, které modelují přirozeného jazyka)
    - automatický překlad byl na nic, dokud se do toho nezapojili matematické metody 
    - překládaly se samostatné věty, a to je problém
- mezní obor, který se skládá z mnoha věcí 
    - teoretická lingvistika a informatika (mainly automaty a gramatiky)
    - umělá inteligence, psychologie (??), logika a statistika
        - psychologie - snaha porozumět napsanému textu je spjata s ní 
        - zeman plísní politiky (??)
            - podstatné jméno, podstatné jméno, podstatné jméno, a ten automat to nerozpozná, jestli je plísní tady myšleno jako podstatné jméno nebo sloveso
            - studuje se, jak dlouho člověk ulpí pohledem na nějaké části věty a z toho se usuzuje, které části věty jsou složité na pochopení 
        - statistika
            - nejdříve frekvence slov v daném textu (třeba v knize čapka)
            - potom statistické modelování (využití neuronek, které měly konečně na čem běžet)
- pravá lingvistika se dělá tu, ne na fildě 
- počítačová lingvistika x počítačové zpracování přirozeného jazyka
    - zásadní rozdíl je v tom, jak se k tomu jazyku přistupuje
    - to první se snaží pochopit ten jazyk a popsat ho (pravidla toho jazyka, proč je syntaktická analýza složitější v češtině než v angličtině)
    - to druhé je o úspěšných aplikacích 
        - ty černé skříňky, co se na to používají, nám nic neřeknou

###### zajímavost
- *firma datlové má nástroj heide (??), který čte chorobopisy místo těch doktorů, aby odhalil sekundární infekce*
    - *ale nebylo tam napsáno, jestli tenhle chorobopis obsahuje sekundární infekci anebo neobsahuje sekundární infekci*
    - *jsou tam hozená syntaktická pravidla, která vyberou ty podezdřelé chorobopisy, co se pošlou dál*
    - *slepě na to nasadit velký jazykový model nejde, a proto se na to hodily ty tradiční způsoby (viz bod předtím)*

#### oblasti počítačové lingvistiky
- morfologie (tvarosloví) - morfologická analýza
    - zabývá se strukturou a formováním slov (skloňování, časování, připojování -pon) a jaké mají gramatikcé kategorie
    - rozpoznávání mluvené řeči je složitá věc - spoustu potíží
        - každý mluví jinak (potřeba natrénovat způsob mluvy)
        - když se něco analyzuje v tichém prostředí x hluk na pozadí
    - proto si to zjednodušíme a začneme psaným textem (bude to o dost jednodušší)
    - analýza nám neřekne, jestli to slovo je podstatné jméno nebo sloveso v tomto daném případě, hodí nám obě
- syntaxe - vzájemné vztahy mezi znaky (třeba slova ve větě - větné členy)
    - čeština je jazyk s volným slovosledem (podmět není na začátku věty, a tak při překladu do angličtiny se musí věta přeházet, a proto je potřeba tu větu zanalyzovat, zjistit, co je podmět a přísudek, a poté tohle přeskládat do správného pořadí)
- sémantika - nauka o významu výrazů v různých strukturních úrovní jazyka (morfém, slovo, věta, slovní spojení)
    - každé slovo dostane jednodznačnou značku na základě analýzy (plísní je v tomhle případě sloveso, bla bla bla), a je to jasné, co to má být 
    - využívá se tu hodně statistika a neuronky (strojové učení) a pro angličtinu je to značkování dobré v 99 % (značky jsou slovní druhy)
    - nikdo se o to v nlp nezajímá, protože se to z těch dat vykouká
- formalismy - formální popsání jazyka 
    - hail všemocný chomský, který přinesl řád do chaosu (formálně správný popis)
- korpusová lingvistika 
    - metody strojového učení 
    - potřeba dat pro učení 
    - tvorba korpusů probíhá desítky let (miliardy slov v korpusech - český národní korpus se tam blíží)
    - není to derinet, tam je to v základním tvaru, v korpusu je to označkované typu čtvrtý pád, rodu jednotného
- statistická lingvistika 
    - přineslo to sice zlepšení, ale meh, neuronky vládnou všemu, takže tohle ustupuje do pozadí
- strojový překlad
- **tímhle se nebudeme zabývat**
    - rozpoznávání a generování mluvené řeči
    - vyhledávání infa v textu
        - shrnutí třiceti stránek do jedné stránky 
        - z databáze vybrat nejdůležitější dokumenty
    - dialogové systémy 

#### problémy významové ekvivalence
- *čvut má budovy nedaleko vítězného náměstí* x *čvut má budovy nedaleko kulaťáku*
    - kulaťák je slangový název pro vítězné náměstí, takže to je ekvivalentní 
- *karel prodává auta* x *od karla se kupují auta*
    - karel může prodávat auta, ale nikdo neříká, že si je lidé kupují 
    - takže tohle nemusí být ekvivalentní 
    - to samé *nakrájel salám na pět kusů* x *nakrájel ze salámu pět kusů*
        - u druhého je tam ještě šestý kus, ten zbytek po nakrájení
- na moravě se mluví česky x česky se mluví na moravě 
    - u druhé věty to vypadá jako, že se česky mluví *pouze* na moravě
    - tohle jde díky té volné slovoskladbě ve větě 

#### víceznačnost a vágnost jazyka 
- *bramborové knedíly a švestkové knedlíky*
    - v reálném světě to popisuje jinou skutečnost, s čímž se musí sémantika vypořádat
- *kritika brazilského delegáta byla ostrá*
    - kdo kritizoval koho
- *v místnosti stojí zelené skříně a židle*
    - pro analýzu je to těžké - je tam jedna židle nebo víc? jsou ty židle zelené nebo ne? 
- *na recepci se dostavil i ředitel banky roku*
    - je tam banka roku nebo ředitel banky roku??
    - následující rozvíjí to předchozí u neshodných přívlastků
- *vysoká škola lesnická v trutnově otevřela novou fakultu*
    - co je ofiko název té školy? vysoká škola lesnická nebo vysoká škola lesnická v trutnově
- *často loví tlouště na višni*
    - docent oliva založil svoji diplomku na tom, kolik významů tahle věta může mít
        - nevíme, kolik je tloušťů a loví je taky jednotné a množné 
- *když slepice málo snáší, tak se vejce špatné shání*
    - nevíme, jestli snáší a shání jsou jednotný i množný tvar v novém podání češtiny 
- *páry vycházejí z lesa*
    - syntaxe nám neřekne, co jsou páry, musíme analyzovat větu (a asi i její kontext)
- *dědeček se rozložil na gauči*
- *závodnice se před závodem se soupeřkami oddávala sexu*
- *dálnice postavená ruskem stála deset miliard*
    - vede ta dálnice ruskem nebo ho postavilo rusko (postavil ho rusk, slovenský ministr dopravy)

*spolu se sekretariátem, teda spíš v boji se sekretariátem děláme nové studijní předpisy*

- *pešán se nestačil divit, na hotelu čekali jeho milenky*
    - někdo čeká na milenky nebo je tam chyba a má tam být *y*? 
- *na trase C spadl člověk do kolejiště metra, nahradí ho autobusy*
- *otec Emmons bude trénovat australany*
    - tohle není páter emmons, ale je to otec emmonsové (vyhrála olympiádu ve střelbě v 2008)
        - zrušit 'ová', není to přivlastňovací význam, jinak by tam bylo 'a' a ne 'á' (kdyby to bylo přivlastňovací, tak je to hajného a ne hajná)
            - tatinkova x tatínková
