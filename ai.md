#### intelligent agents
- ty rozdílné typy
#### a*
- když je a* přípustný, tak je optimální v tree search, když je a* monotónní, tak je to optimální v graph search

#### csp 
- problem solving techniques je backtracking
- fail first a suceed first (to je u variable ordering)
- forward checking je kontrola pouze dvojic vrcholu a look ahead je kontrola pro všechny vrcholy

### logika
- knowledge based agent 
# dobré ráno
### plánování 
- transition function - prostě co se změní po akci
- akce je proveditelná - když kladný efekt je podmnožinu stavu 
- relevance - kdydž se nějak přiblížím k cíli a zároveň nekazím už existující efekty
- regression set - od cíle a akce odečteme akci a přidáme předpoklady 
    - když děláme backward planning, tak bereme jenom ty vhodné stavy, ale hůř se tam dělá heuristika 
- v kalkulu hledáme stav, ve kterém jsou spolněné podmínky cíle

#### bayesovká síť
-  je to dag, kde každá proměnná je vrchol a je napojená na svého rodiče, pokud je na něm závislá (hrana: chřipka -> horečka)
    - každý uzel má podmíněnou pravděpodobnost ke svým rodičům
- vyjadřuje full joint probability distribution tak, aby to bylo víc kompaktní a přehlednější -> ne velká tabulka, ale dag

- každý vrchol má vlastní tabulku, kde jsou pouze ty vtahy, co má s rodiči

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
        - fixnu hodnoty těch evidencí a každý vzorek dostane váhu podle toho, jak pravděpodobná je evidence při dané konfiguraci
    - vzorek odpovídá evidenci (vzorek je konzistentí s e) = vzorek má stejné hodnoty, jako to, co je zadané
        - víme, že $alarm = True$, tak vyhodíme ty vzorky, co mají $alarm=False$
    - využívá se na to monte carlo technika

    - udělám podle pravděpodobností monte carlo metodou náhodné případy a vyhodíme ty, co nás nezajímají
    - při váženém vzorkování si řekneme, že earthquake je true, ale musíme to poton provážit

### probabilistic reasoning over time
- tohle se týká modelování systémů, které se vyvíjejí v čase, přičemž nemusíme znát jejich vnitřní stav tak dobře, ale máme pozorování

#### define transition and observation models and explain their assumptions

- přechodový model popisuje, jak se stav mění v čase $P(X_t|X_{0:t-1})$, tedy pravděpodobnost stavu na základě předchozího 
    - pokud využíváme Markov asumption, stačí nám pouze $P(X_t|X_{t-1})$, na ostatních to není závislé
    - stav závisí pouze na předchozím stavu
    - pokud jsou všechny hodnoty stejné, dostáváme statistický model
- observation model popisuje, jaká pozorování očekáváme, pokud známe skutečný stav (když prší, bvude pravděpodobnější, že uvidím mokré silnice)
    - pozorování závisí pouze na aktuálním stavu

- můžeme udělat bayesovskou síť
    - $P(E_t|X_{0:t}, E_{0:t-1})$, ale zas využijeme Markova a dostaneme, že nás zajímá jenom $(E_t|X_t)$
    - lze to modelovat pomocí bayesovské sítě, kde chain rule bude $P(X_{0:t} = P(x_0)\prod P(X_t|X_{t-1}))$ a $P(X_{0:t},E_{1:t} = P(x_{0})\prod P(X_t|X_{t-1}*P(E_t|X_t)))$

    ![alt text](image-1.png)

#### basic inference tasks
- filtering - výpočet aktuálního stavu na základě všech předchozích pozorování aneb 'kde teď jsem??'
    - je potřeba jít od začátku, používá se rekurzivně pomocí předchozího výsledku 
    - $f_{1:0} = P(X_0)$ a poté se jde dál jako $f_{1:t} = P(X_t|e_{1:t}) $ a $f_{1:t+1} = \alpha *forward(f_{1:t},e_{t+1})$
        - kde to první je pravděpodobnost $X_0$
        - to druhé je filtrační distribuce v čase $t$
        - a to třetí je filtrační distribuce v $t+1$
    - $P(X_{t+1}∣e_{1:t})=∑P(X_{t+1}∣x_t)P(x_t∣e_{1:t})$ nám říká, jaký je pravděpodobnost, že nastal náš stav, pokud uvážíme to, co víme z minulosti 
    - a pak to dáme dohromady s těmi pozorováními, co dostaneme v $t+1$
    - $P(X_{t+1}∣e_{1:t+1})=αP(e_{t+1}∣X{t+1})⋅P(X{t+1|}e_{1:t})$  nám říká, jak upravíme našei předikci, pokud teď vidíme nová pozorování 

- prediction - spočítat pravděpodobnost budoucího stavu proměnné $X_{t+k}$, aniž bychom měli nová pozorování po čase $t$
    - máme pozorování do času t a chceme zjistit, jaký bude náš pravděpodobný stav v čase dál
    - je to jako filtering bez nových důkazů
    $P(X_{t+k+1}∣e_{1:t})=x_{t+k}*∑P(X_{t+k+1}∣x_{t+k})*P(x_{t+k}∣e_{1:t})$
    - vezmi všechny stavy možné v čase $t+k$ a pro každý z nich spočítej, jaká je pravděpodobnost, že přejdu do stavu $x_{t+k+1}$ krát pravděpodobnost, že jsem v tom daném stavu $x_{t+k}$
    - když budeme produkovat dlouho tyhle výpočty, přestane záležet na tom, kde jsme začali
        - konvergence ke stacionární distribuci

- smoothing řeší, kde jsem byl v minulosti
    - chceme zjistit, jak pravděpodobně vypadala situace $k<t$ (např. sledujeme robota posledních deset sekund a zaznamenáváme, co dělal, tak kde byl v třetí sekundě)
    - abychom spočítali $P(X_k∣e_{1:t})$, rozdělíme ten výpočet na dvě části   
        - pravděpodobnost, že systém byl ve stavu $x_k$, když známe evidenci do času $k$ - je to prostě filtering
        - pravděpodobnost budoucích pozorování, když jsme ve stavu $x_k$
        - kde $b_k=x_{k+1}*∑P(e_{k+1}∣x_{k+1})⋅P(x_{k+1}∣X_k)⋅b_{k+1}$
        - $f_{1:k}$ je klasický forward filtering
        - $b_{k+1:t}$ je backward massage passing (zprava doleva)

    $P(X_k∣e_{1:t})=α⋅P(X_k∣e_{1:k})⋅P(e_{k+1:t}∣X_k) = α⋅f_{1:k}*b_{k+1:t}*x_{k+1}*∑P(e_{k+1:t}∣x_{k+1})⋅P(x_{k+1}∣X_k)$

- most likely explanation - nejpravděpodobnější sekvence stavů, která nejlépe vysvětluje daná pozorování
    - mohlo by se to podobat smoothingu, ale tam jde jenom o jeden okamžik, tady jde o celou sekvenci 
    - využijeme viterbiho algoritmus
        - $𝛿_t(x_t)$ je pravděpodobnost nejlepší cesty do stavu $x_t$ v čase $t$
        - $P(x_{t+1}|x_t)$ je přechodová pravděpodobnost
        - $P(e_{t+1} | x_{t+1})$ je pravděpodobnost pozorování 
        - je to rekurzivní výpočet, kdy si v každém kroku uložíme nejlepší předchozí stav a pokračuju do dalšího stavu
        - jakmile se dostanu na konec, najdu stav s nejvyšším $𝛿_t$ a zrekontruuju z něho tu nejlepší cestu
        $δ_{t+1}(x_{t+1})=max_{x_i}[δ_t(x_t)⋅P(x_{t+1}∣x_t)⋅P(e_{t+1}∣x_{t+1})]$

#### hidden markov model vs dynamic bayesian network
- pokud jsou některé stavy skryté (nejsou přímo pozorovatelné) a máme pozorování $E_t$, které závisí na stavech, a ještě přechod mezi stavy závisí jenom na předchozím stavu a pozorování závisí jen na nynějším stavu, tak máme HMM
    - $P(X_0)$ je počáteční distribuce stavů 
    - $P(X_t|X_{t-1})$ je přechodový model (transition model) 
        - je to matice, kde $T_{(i,j)} = P(X_t=j|X_{t-1}=i)$ 
    -$P(E_t|X_t)$ je pozorovací model 
        - taky matice
        - $O_{t(i,j)} = P(E_t=e_t|X_t=i)$
    - při těch maticích využíváme dopředného a zpětného šíření zpráv pomocí maticových operacích
    - nejdřív dopředné šíření - pravděpodobnost, že se systém nachází ve stavu $X_t$ na základě našich pozorování do času $t$
        $f_{1:t+1}=α⋅P(e_{t+1}∣X_{t+1})⋅∑P(X_{t+1}∣x_t)⋅P(x_t∣e_{1:t})$
        - v matici pro přechody je to $T(i,j)=P(X_{t=j}∣X_{t−1=i})$
        - a tu matice pro pozorování: $O_{t+1}(i,i)=P(E_{t+1}=e_{t+1}∣X_{t+1=i})$
        - dohromady to dává $f_{1:t+1}=α⋅O_{t+1}⋅T^T⋅f_{1:t}$
        - vysvětlení: ten poslední násobek promítne aktuální odhad stavů přes přechodovu matici; ten druhý upraví výsledek podle aktuálního pozorování a $\alpha$ to znormalizuje
    - teď zpětné šíření, co se používá při smoothingu
        - chceme spočítat $P(e_{k+1:t}∣X_k)=b_{k+1:t}$ a využijeme pro to vzorec maticový $b_{k+1:t}=T⋅O_{k+1}⋅b_{k+2:t}$
        - kde druhá část promítně budoucí zprávu podle pozorování a $T$ promítne tuhle zprávu zpět do předchozího času pomocí přechodů 
- dynamická bayesovská síť modelu časově vyvíjející jev, tedy pravděpodobnostní model v čase
    - každý časový krok má kopii stejných proměnných a vtahů a mezi jednotlivými časovými kroky jsou přechodové hrany, které modelují vývoj v čase
        - v tom popisu je zase $P(X_0)$, přechodový model a senzorový model
    - každá proměnná závisí jen na minulém stavu, ne na celé historii a pozorování závisí jen na aktuálním skrytém stavu

- když je chceme porovnat, tak HMM (kde máme jenom jednu skrytou proměnnou a jednu senzorovou proměnnou v každém čase, jejíž chování ovlivňuje ta jedna proměnná) je spešl případ DBN (umožňuje více stavových proměnných v jednom čase a každá mít vlastní rodiče) a DBN může být encoded jako HMM 
    - když by měl DBN 20 boolean proměnných, tak v HMM bude jedna velikosti $2^{20}$ možných hodnot a přechodový model musí uchovávat pravděpodobnostní přechod mezi každou dvojicí těchto složených stavů
    - v DBN ale nemusíme modelovat přechod mezi všemi kombinacemi 
        - rozdělujeme totiž ten obrovský problém na malé tabulky, kde ty proměnné modeluju samostatně a každá má svůj vlastní lokální přechodový model

#### hidden model markov
- ten sensor matice je pro každý čas
- pro každý čas si rozkopíruju znovu a slepím to dohromady
- tam je matice SxS, která je všechno se vším
### decision making
#### formalize rationality via maximum expected utility principle
- racionální agent je takový, který se rozhoduje tak, aby maximalizoval svou očekávánou užitečnost - zvolí tu akci, která má největší očekávaný přínost
- pro každý výsledek $s$ spočítáme s jakou pravděpodobností může nastat vzhledem k akci a důkazům $P(s|a,e)$ a poté to vynásobíme užitečností výsledku 
    - po sečtení toho všeho odstaneme průměrnou očekávánou hodnotu, kterou by nám akce přinesla
    $EU(a∣e)=∑P(Result(a)=s∣a,e)⋅U(s)$
- agent by měl vybrat tu akci, která nám přinese tu největší očekávánou užitečnost
- často se ale stane, že neznáme přesné číslo užitku a můžeme porovnávat dvě akce dohromady; prostě je lepší je srovnat, než jim dávat přesné číslo
    - nejhoršímu výsledku nastavím užitečnost na nulu a nejlepšímu na jedničku
    - potom se agenta ptám, jestli chce nějakou možnost S anebo si náhodně vybrat z dvojice (max_možnost a min_možnost) a měním pravděpodobnost, s jakou si náhodně zvolím tu lepší možnost, dokud agent neřekne 'mah, je mi to jedno, jestli budu mít S nebo něco z té dvojice'
        - jakmile to řekne, tak ta pravděpodobnost $p$ pro maximální možnost je hodnota užitečnosti pro ten výsledek S $U(S)=p$

#### define decision network
- je to rozšíření bayesovských sítí, které kromě náhodných proměnných obsahují také rozhodovgací uzly (volby, které agent může dělat) a užitkové uzly (kvantifikace, jak moc daný výsledek agent preferuje)
![alt text](image-2.png)
- při racionálním rozhodování využíváme Maximum expected utility (MEU), tedy vybereme akci, která maximalizuje očekávánou užitečnost
    - nastavím aktuální stav (když máme nějaká pozorování)
    - a pro každou možnou hodnotu rozhodovacího uzlu spočítám posteriózní pravděpodobnost všech proměnných, co ovlivňují užitečnost a nakonec spočítám tu očekávánou užitečnost
    - vyberu tu akci s nejvyšší užitečností

#### define a sequential decision problem
- máme plně pozorovatelné nedeterministické prostředí a hledáme plán, jak získat co největší užitek

- markov desicion process (MDP) je matematický rámen pro prostřední, které je plně pozorovatelné, stochastické a markovskovské (budoucnost závisí pouze na přítomnosti, ne na minulosti)
    - obsahuje stavy - všechny možné situace, do kterých se agent může dostat
    - akce, které agent může dělat
    - přechodový model $P(s'|s,a)$, tedy pravděpodobnost, že se akce $a$ ve stavu $s$ vede do stavu $s'$
    - odměnová funkce $R(s)$, což je krátkodobá odměna, kterou agent dostane, když se dostane do stavu $s$
    - $γ$, což je hodnota, která nám říká, jak moc si ceníme budoucích odměn (pokud je to blíž nule, tak jedeme na blízké odměny)
        - použito v rovnici $U[(s_0,s_1,...)]=R(s_0) + γ*R(s_1)*γ^2 +R(s_2) +...$, kde $U()$ je dlouhodobá odměňovací funkce

- cíl agenta je maximalizovat očekávánou užitečnost (užitečnost je konečná, protože ta řada nahoře konverguje k nule)
    - politika $\pi(s)$ mi říká, jakou akci mám provést ve stavu $s$ a my hledáme tu optimální politiku $\pi*(s)$, která nám maximalizuje dlouhodobou odměnu
    - spočíst danou politiku $\pi(s)$ se dá jako  $U_π(s)=E[∑γ^i⋅R(S_i)]$
    - a my z nich poté vybíráme tu nejlepší pro začátek $\pi^*(s)=maxU_\pi(s)$
        - ale pokud je prostředí markovské, tak mi nejde o počáteční stav, existuje jedna univerzální optimální politika
    - optimální politika se podívá na stav $s$ a zjistí, jaká akce bude mít největší užitek $π^∗(s)=max_a∑P(s′∣s,a)⋅U(s′)$ 
        - projdu prostě všechny možnosti, co mohou nastat, a vyberu tu možnost, která mi dá největší výsledek

- tak jsme se dobrali k bellmanově rovnici, která mi říká okamžitou odměnu a jakou odměnu dostanu v budoucnosti $U(s)=R(s)+γ⋅max′∑P(s′∣s,a)⋅U(s′)$, kde $U(s)$ je dlouhodobá užitečnost, $R(s)$ je okamžitá a ten zbytek je nejlepší budoucí stav
- value iteration je algoritmus, kdy za $U(s)$ dosadím nějakou hodnotu a opakuju bellmanovu rovnici, dokud se mi ta hodnota neustálí okolo nějaké hodnoty
    - ta hodnota je moje optimální politika
    - $U_{i+1}(s)← R(s)+γ⋅max′∑P(s′∣s,a)⋅U_i(s′)$

- policy iteration hledá optimální politiku, aby byl dlouhodobý zisk co největší
    - nejdříve si spočítám, jak dobrá je aktuální politika
    - a pak se podívám, jestli neexistuje lepší akce
    - tohle provádím, dokud nenajdu tu nejlepší (už se to nemění)

#### formulate Partially Observable MDP and show how to solve it.
- agent zde přesně nevidí stav, nemůžeme přímo využít $\pi(s)$, protože neví, co $s$ je
- pořád budeme mít transition model, akce, odměny a sensor model, ale nevím, co přesně je náš nynější stav, a na to použijeme odhady (belief states)
    - vyřešíme MDP přes belief states

- nejdřív uděláme ten filtering, abych zjistila, kde jsem, a pak teprve spočítám ty utility

### game theory
#### explain core properties of environment and information needed to apply adversarial search
- adversarial search se používe v prostředích, kde existuje protivník, který působí proti nám
    - prostě hry pro dva
    - snaží se najít nejlepší tah pro jednoho hráče
- abychom tohle mohli použít, musí to prostředí splňovat nějaké vlastnosti
    - každá akce má předvídatelný výsledek
    - je to plně pozorovatelné - oba hráči vidí celou herní situaci
    - diskrétní stavy - herní pozice je jednoznačně definovaná
    - dva hráči
    - to, co chce jeden, je pro druhého ta nejhorší možnost
#### minimax a alfa beta pruning
- minimax je strom herních stavů, kdy v jednom chci minimalizovat (tah spoluhráče) a v druhém maximalizovat (můj tah)
- je to ale hrozně nepraktické, stromy jsou velké (rostou exponenciálně), a tak to chceme ořezat
- alfa-beta pruning odřezává ty větve, u kterých mi je jasné, že tam nebude lepší výsledek
    - alfa je nejlepší hodnota, kterou může MAX garantovat
    - beta je nejlepší hodnota, kterou může MIN garantovat
    - pokud $\beta ≤ \alpha$, tak tu větev zařízneme
![alt text](image-3.png)

    - hodí se to, když nechceme projet celý strom - za stejný čas se dostaneme do dvojnásobné hloubky

- když nemůžeme prohledat celý strom, tak použijeme alfa-beta pruning, ale to samo o sobě nestačí - přidáme hodnotící funkci
    - v nějaké hloubce ten strom zaříznu a aproximuju užitečnost pomocí evaluation function (heuristika)
        - my chceme co největší hodnotu, nepřítel co nejmenší
        - příklad: šachy - #pešáků, celkový počet, je živá královna??
            - každé té možnosti přiřadím váhu a spočítám celkovou váhu jejich součtem

#### stochastické hry
- stochastické hry jsou takové, kde je i prvek náhody a minimax s tím musí pracovat (říká se tomu expected minimax)
    - využívá pravděpdobnost, kdy do stromu přidáme chance uzly
    - pokud jsme v terminálu node, tak vrátíme výsledek (utility)
    - pokud jsme v max, tak pro všechny děti spočítáme hodnotu a vybereme nejlepší 
    - pokud jsme v min, tak vezmeme tu nejmenší
    - pokud jsme v chance node, tak sečteme vážené průměry všech nástupců a získáme $\sum P(tah)*užitečnost$ přes všechny tahy
        - do chance node se jedou tahy a z nich se udělá vážený průměr a tahle hodnota se poté využívá v určování minimaxu
        ![alt text](image-4.png)

#### single move hry
- pri single-moves games si každý hráč vybere jednu strategii a najednou se všichni odhalí (ty svoje strategie)
    - payoff funkce uděluje hráči utility podle toho, jak hráli ostatní hráči(pro každou kombinaci můžeme vytvořit výplatní matici)
    - strategie může být deterministická (pure; hráč vybere jednu akci) anebo pravděpodobnostní (mixed; hráč přiřadí pravděpodobnost ke všem tahům)
    - chceme přiřadit každému hráči racionální strategii
- dominantní strategie - nejlepší volba bez ohledu na ostatní hráče
- nash equilibrium je ve chvíli, kdy si nikdo nemůže zlepšit svou výsledek změnou strategie 
- pareto dominance je taková strategie, pokud pro všechny hráče je stejně dobrá a pro někoho lepší než jiná strategie
- hráčovo dilema 
![](image-5.png)
    - racionální strategie je testify, ale pareto optimal je refuse refuse (myslím)

- maximin strategie je to, když chceme být opatrní, a tak si pro každou strategii najdu její nejhorší možnost a z těch strategií si vyberu, jejíž nejhorší možnost není tak hrozná 
    - používá se, když o protivníkovi nic nevím
    - příklad na two finger morra, kde evžen ukazuje svůj tah jako první a vybírá strategii, aby minimalizoval ztrátu
        - vybere si, že zahraje 1 (s pravděpodobností $p$) a hráč reaguje, aby mu co nejvíce uškodil 
![alt text](image-6.png)
            - pokud zahraje 1, tak $U_1(p)=p⋅(−2)+(1−p)⋅3=−2p+3−3p=3−5p$
            - pokud zahraje 2, tak $U_2(p)=p⋅1+(1−p)⋅(−4)=p−4+4p=5p−4$
        - chceme minimalizovat maximum těchto hodnot, takže to dáme do rovnosti a dostaneme $p=7/12$
            - hledali jsme takovou pravděpodobnost pro Evžena, abychom zajistili, že jeho výplata bude co nejlepší v tom nejhorším případě (Olga zvolí pro Evžena tu nejhorší možnost)
            - po dosazení dostáváme, že to bude $-1/12$, protože to je z pohledu Olgy, pro kterou je to $1/12$

- repeated games je to ve chvíli, kdy se ta kola opakují 
    - 100x vězňovo dilema, tak stejně bude nejracionálnější vždy prásknout
    - pokud bude ale další kolo na 99 %, tak to bude jiné
        - perpetual punishment - odmítám se přiznat tak dlouho, dokud nepráskne ten druhý, pak praskám
        - tit-for-tat - začnu s odmítám a poté opakuju tah hráče z minulého kole (tohle je velmi efektivní proti spoustě strategiím)

#### mechanism design: explain classical auctions and tragedy of commons
- mechanism design nám říká, jak navrhovat pravidla, abychom motivovali hráče k žádoucímu chování (vláda chce snížit znečištění, a tak přijde s emisními povolenkami)
- anglická aukce - jdeme odzdola nahoru
    - má to dominantní strategii (přihazovat, dokud to není nad moji cenu), ale když je tam někdo bohatý, odradí to ostatní a taky je to náročné na spojení
- dutch aukce - jdeme odshora dolů
    - rychlé
- sealed-bid auction - člověk udělá nabídku nezávisle na ostatních a ta vyhrává
    - není tu žádná simple dominantní strategie (pokud věřím, že max nabídka je $b$, kde $b$ je nižší než, co já chci max utratit, tak dám $b + \varepsilon$)
- vickrey aukce - dát nabídku nezávisle na ostatníc, vyhrává nejvyšší hodnota, ale platí druhou nejvyšší hodnotu
    - primární strategie je prostě vsadit tu svoji hodnotu

- the tragedy of commons - víc lidí sdílí omezený zdroj, ale nikdo za jeho využívání nemusí platit, rychle se využije celý, a to vede k menšímu obnosu všech účastníků
    - řešení: regulace, privatizace, zpoplatnění 
    - vickrey-clarks-groves algoritmus: lidé nahlásí, kolik jsou za tu službu ochotni platit, vybere se ta skupina lidí, která by platila nejvíce
        - ti budou platit tu nejvyšší cenu, která se nedostala do podmnožiny
        - dominantí strategie? hodit tam pravdu

### machine learning
#### define and compare types of learning
- klasifikace (přiřadí třídu) a regrese (odhadne číselnou hodnotu)
- supervised learning (učení s učitelem), kde model se učí ze značených dat, každý vstup má u sebe i správný výstup (naučit se rozpoznávat emaily jako spam)
- unsupervised learning - data nemají štítky, učí se poznávat skryté struktury nebo vzory v datech 
- reinforcement learning - cilem je maximalizovat dlouhodobou odměnu (cukr a bič)
- oklamova břitva říká, že pokud máme víc hypotéz, vybrat tu nejjednodušší 
#### decision trees
- dostane vektor vstupu a vrátí jeden výstup
- je to prostě strom, kde každý uzel je nějaký test, podle kterého se rozhoduje, a větve jsou možné odpovědi
    - listy jsou ty výsledky 
- strom se staví rekurzivně, kdy vybereme nejdůležitější test a values rozdělíme podle testů, pokud jsou ty hodnoty ve stejné kategorii, tak hotovo, ale jinak dělíme dál 
    - vybereme ten test s největším informačním ziskem, tedy hledáme ten test, s nímž se zmenší entropie co nejvíce (reálně to nějak rozdělíme a k něčemu dospějeme)
    - entropie:  $Entropy(S)=−∑p_i*log_2p_i$
    - informační zisk: $Gain(S,A)=Entropy(S)−∑\frac{∣S∣}{∣S_v|}⋅Entropy(S_v)$

#### logical models
- učení logických modelů mi říká, jak správně získat hypotézu, která mi popisuje rozdělení mezi pozitivními a negativními příklady
- chceme jednu ultimátní logickou formuli, a raději jednu hypotézu než všechny disjunkce
- false positive: model říká ano, ale skutečnost je to ne
- fase negative: je to naopak
- current best hypothesis 
    - pamatuju si jenom jednu hypotézu, a tu opravuju
        - pokud je example konzistentní, nic nedělám
        - pokud není, tak pokud je to false negativ (je moc přísná), tak odeberu konjukce (podmínky) anebo přidám disjunkce (možnosti)
        - false positive je na tom obráceně
- version space learning
    - udržuje si celou množinu hypotéz, které jsou konzistentní a tvoří se nejobecnější hypotéza (true všemu) a nejužší (false všemu)
        - pokud false negative pro G-set, tak zahodíme tu část, s kterou se kryje
        - pokud je to false positive, tak $G_i$ nahradíme přesnější hypotézou, která už bude dávat false (je to až moc benevolentní, tak to zpřesníme)

        - false negative pro S-set, tak ji zobecníme (moc přísné, zjemníme)
        - false positive, tak ji zahodíme (říká ano, i když nemá, má to být přísné a není)

#### linear regression
- metoda pro předpověď číselné hodnoty a chceme najít přímku, která sedí na trénovací data
    - hledáme lineární funkci, která má nejmenší chybu (je to kvadrát)
    - $y = w_1*x + w_0$
- lineární klasifikace je přímka, která nám dokáže rozdělit body na dvě části tak, jak chceme

- neuronové sítě je zobecnění lineárních modelů, které umožňuje nelineární rozhodování 
- architektura je vstupní vrstva, skryté vrstvy (neuronové uzly) a výstupní vrstva
    - každý neuron počítá $\sum w_i*x_i + b$, kde $x_i$ jsou vstupy a $w_i$ jsou váhy, které se síť učí a b je posun, aby síť nebyla závislá jen na vstupech
- potom aplikujeme aktivační funkci, která dodá nelinearitu a neuronky přestanou být jen vrstvy obyčejné lineární regrese

- ještě to backtrack něco
- dostanu nějaký výstup na konci a spočítám chybu s tím, co jsme měli dostat a tuhle chybu propaguju zpět pomocí řetězového pravidla a upravuju váhy tak, aby se mi ta chyba snížila
    - $Loss = 1/2*(y_{pred}-y_{true})^2$
    - kde tu váhu $w$ zjistím tak, že od ní odečtu derivaci $Loss$ podle $w$ vynásobenou rychlostí toho učení
        - gradient mi říká, jak moc změnit tu váhu

#### parametrický a neparametrický model
- parametrický model je takový, kde máme natrénovanou síť a už nepotřebujeme data, protože ta máme reprezentována vahami
    - model s pevnou strukturou a málo parametrami
    - lineární regrese
    - rychle a dobře funguje na menších datehc, ale hůř modeluje složitější vztahy
- v neparametrickém pořád potřebujeme data
    - přizpůsobuje se datum, flexibilní a nepotřebuje trénink
    - pomalejší a potřebuje víc paměti
- k examples
    - máme hodnotu x a my chceme odhadnout, jakou výstupní hodnotu bude mít
        - najdu k nejbližích sousedů, podívám se na jejich výstupní hodnoty (pokud budou většinově pes, tak i x je pes, když to bude hodnota, tak zprůměruju ceny)
        - na určení vzdálenosti použijeme buď manhattanskou nebo euklidovskou vzdálenost (minkovwskou)
    - je potřeba ještě ty hodnoty normalizovat, aby velká čísla nedominovala výpočtu vzdálenosti

- support vector machine je technika v supervised learningu hlavně pro klasifikaci (dobré výsledky hned)
    - chci najít separátor mezi dvěma typy dat a chci, aby ta čára byla co nejdéle od obou skupin
    - ty nejbližší bodíky budou support vectors (ti jsou nejdůležitější)
    - když nepůjde namapovat, tak použiju kernel funkci a dostanu to do vyšší dimenze, kde už to půjde rozdělit

#### bayesovské učení
- máme několik balíčků a my chceme přijít na to, který balíčka to je 
- pro každý balíček máme přiřazenou nějakou pravděpodobnost, a tu měníme podle toho, jaké bonbóny jsme snědli - využíváme bayesův vzorec
    - $P(h_i|d) = \alpha * P(d|h_i)*P(h_i)$
- a abychom určili, jaký to bude další bonbon, tak uděláme vážený průměr přes pravděpodobnosti pytlů
- maximum likelyhood mi říká, jaký je tam poměr v tom pytly podle toho, jaké si vytahujeme bonbóny
    - tohle je bayesovská síť s jedním vrcholem
    - vytáhneme n bonbónu, z toho $c$ jsou cherries a $l$ jsou limes a chceme zjistit poměr v tom pytli: $P(d|h_p) = p^c(1-p)^l$, kde $p$ je pravděpodobnost, že vytáhneme bonbon cherries
    - chceme to maximalizovat, tak to hodíme do logaritmu a zderivuju podle $p$, a hledám, kde to rovno nule
- učení se parametrů je to, že chceme odhadnout ty pravděpodobnosti mezi rodiči a syny 
    - nevidíme ale všechny proměnné, a tak využijeme EM algoritmus
    - začneme s tím, že prostě určíme nějaké parametry 
    - z toho spočteme hodnoty těch skrytých proměnných a aktualizujeme tyto parametry podle těchto odhadů 
    - tohle děláme, dokud se to neustálí 

#### reinforcement learning
- agent se učí chováním ve světě prostřednictvím odměn
    - máme agenta a prostředí, potom možné stavy, které mohou nastat a akce, které může udělat
        - agent dělá přechodový model podle odměn a trestů
        - jak dobrý je který stav

##### pasivní učení
- pasivní učení je to, že má danou policy (ví, co v tom stavu má udělat za akci), ale nemá transition model ani reward funkci
    - chce se naučit utility funkci a jak ta policy je dobrá
    - agent udělá sekvenci stavů a akcí a získá nějaké odměny a my po sekvenci těch akcí sečteme ty odměny a uděláme pro každý stav průměr přes ty hodnoty
        - je to ale na nic, protože ty hodnoty jsou na sobě závislé, a to DUE neřeší 
    - potom máme ADP, které využívá bellmanovu rovnici k výpočtu užitečnosti
        - využiju bellmanovu rovnici, kdy si pamatuju, že nějaký stav má odměnu, kterou si zapamuju a pomocí value iteration si dopočítám užitečnost stavů 
        - když řekneme, že jdeme desetkrát doleva a doleva jdeme sedmkrát, tak tu znalost právě využijeme v té bellmanově rovnici
        - zpřesňuje se to tím, jak tu akci opakujeme
    - potom máme temporal difference learning
        - učí se užitečnost stavů z přímého sledování přechodů mezi stavy
    -
- model base se učí z modelu a používá plánování 
- model free se učí přímo z reálných zkušeností

##### aktivní učení
- aktivní agent využívá znovu bellmanovu rovnici 
- on ale ty cesty objevuje sám
    - greedy agent - nikdy nezkusí jinou, takže nenalezne lepší (takový agent exploatuje, ale neexploruje)
- když agent jenom exploruje, nevyužije to, co se naučil, a pokud jenom exploatuje, nenaučí se nic nového (lepšího)
    - buď můžeme třeba s pravděpodobností $1/t$ vyzkoušet novou akci a potom exploatovat
    - anebo těm neprozkoumaným akcím dám vyšším váhu, takže si je vyberu spíš
