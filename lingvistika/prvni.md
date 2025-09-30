#### přednáška 1

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

##### oblasti počítačové lingvistiky
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
