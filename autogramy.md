konečný automat
- definice:
    - deterministický konečný automat
    - jazyk a spol.
    - rozšířená přechodová funkce
    - regulární jazyk

- věta: 
    - myhill-nerodova věta
        - kongruence 
        - nechť jazyk L je nad konečnou abecedou, pak L je rozpoznatelný konečným automatech $\iff$ existuje pravá kongruence konečného indexu, kde jazyk L je sjednocením této kongruence 
            - důkaz: má konečný automat $\implies$ je to pravá kongruence konečného indexu
                - je tranzitivní, symetrická a reflexivní 
                - je to pravá kongruence (když dojdeme do stejného stavu, tak potom z něho půjdeme stejnou cestou)
                - má konečný index
            - teď druhá strana: je to pravá kongruence konečného indexu $\implies$ má konečný automat
                - sestrojíme konečný automat - za stavy Q zvolíme třídy rozkladu 
                - $[\epsilon]$ je $q_0$ a ty konečné stavy jsou ty třídy, jejichž sjednocení dá celý ten jazyk (každé slovo je v jedné třídě) $\delta([u], x) = [ux]$
                - přechodová funkce je korektní podle def pravé kongruence 
        - jazyk není regulární - jde se přes spor (L = {$a^+b^ic^i \lor b^ic^j $})
            - máme $m$ tříd kongruence a jazyk L je sjednocení některých tříd z nich 
            - máme množinu řetězců S, která má více než $m$ čísel
                - S = {$ab, abb,..., ab^{m+1}$} 
                - dvě slova budou ve stejné třídě a přidáme tam $c^i$ a dostaneme spor, že jsou pořád ve stejné třídě

    - součin automatů: stav bude každý s každým, přechody budou ze stavu tam, kam vedly původní přechody a nový vstupní bude ten stav, kde jsou oba vstupní ty dva původní


    - iterační lemma: mějme jazyk L, pak existuje konstanta $n \in \N $ tž. každé slovo $w$ delší než n lze rozdělit na tři části $w=xyz$: 
        - $y \not = \epsilon$
        - $|xy| \leq n$
        - $\forall k \in \N_0 $, slovo $xy^kz$ je také v L

        - důkaz: máme jazyk L s DFA, který má $n$ stavů a vezmeme slovo, které je delší než n, takže dvě písmena budou muset být ve stejném stavu
            - x bude ta část slova do $a_i$, y bude od $a_{i+1}$ do $a_j$ a z bude zbytek


    - ekvivalence stavů- definice (je tam iff) 
        - postup je takový, že prostě všechny ty, co jsou přijímací a nejsou, tak je rozdělím 
            - pak jdu indukčně - p,q chci zjistit a r, s jsou rozlišitelné a $q = \delta(r,a)$ a $p=\delta(s,a)$, tak i p,q jsou rozlišitelné
                - JDE TAM O TO PÍSMENO, CO JE POUŽITO NA PŘECHOD   
            - tohle platí
                - pro spor vezměme dvojici stavů, které jsou různé, ale algoritmus tvrdí, že nejsou
                    - mají nějaké nejkratší slovo, které je rozlišuje a to slovo je ještě kratší, které rozděluje tu dvojici vstupů předtím 
                    - v dalším kroku to rozliší i tuhle dvojici
        - ekvivalence automatů a jazyků (mají vlastně stejný automat)
            - sjednotíme ty dva automaty a koukáme, jestli jsou oba vstupní stavy ekvivalentní (prostě děláme velkou ekvivalenci stavů, kde nás zajímají pouze ty vstupní)
        - redukt automatu
            - automat je redukovaný, pokud jsou všechny stavy dosažitelné a rozlištielné a přechodová funkce je totální (fail vstupy)
            - jak udělat redukt: 
                - odstraníme nedosažitelné vrcholy a poté uděláme ekvivalenci stavů a hrany z těch stavů dáme tak, jak byli původně 
                    - vstupní a výstupní jsou ty stavy, které obsahují ty původní vstupní a výstupní


        - automatový homomorfizmus
            - zobrazení h je automatovým homomorfizmem, pokud počáteční stav je zachován
                - přechody jsou zachovány (přechody v $A_1$ se přes h přenesou do přechodu v $A_2$)
                - přijímací stavy odpovídají (stav v $A_1$ je akceptační $\iff$ stav je přijímací v $A_2$)
            - když bude zobrazení prosté (žádné dva různé stavy v $A_1$ neodpovídají jednomu v $A_2$) a na (každému stavu v $A_2$ odpovídá nějaký stav v $A_1$), tak je to izomorfismus
        - ekvivalence automatů - definice
            - když existuje homomorfismus konečných automatů, tak ty dva automaty jsou ekvivalentní 
            - každé dva redukované automaty, co jsou homomorfní, jsou také izomorfní 
            - každý DFA A má redukt 

nedeterministické automat
    - definice (rozdíl je v přechodové funkci, kdy nevracíme jeden přechod, ale jeho množinu)
    - $\epsilon$ -uzávěr
        - každý prvek je ve svém uzávěru a poté ty, kam se dostanu přes $\epsilon$
        - přechodová funkce - definice
        - jazyk přijímaný $\epsilon$ NFA
    - podmnožinová konstrukce s e-přechody
        - z eNFA jsme schopni udělat DFA přes e-uzávěr, kdy každý stav je ten svůj e-uzávěr, vstupní je ten vstupní e-uzávěr a výstupní jsou všechny stavy, které obsahují původní výstupní
    - množinové operace nad jazyky
        - sjednocení, průnik, rozdíl {$w \in L \land w \notin M$} a doplněk {$w \notin L$}
        - de morganova pravidla: průnik je doplněk sjednocení doplňků; sjednocení je doplněk průniku doplňků; rozdíl je sjednocení L a doplňku M
        - věta o uzavřenosti na množinové operace: M a L jsou regulární jazyky, pak jejich děti viz. nahoře jsou také regulární 
            - pro doplněk: pokud není definovaná nějaká dvojice (q,a), tak jí uděláme nový stav $q_{fail}$ a vše, co z něho vede, taky skončí zde; pak už jenom prohodíme vstupní a výstupní stavy
            - pro zbytek: uděláme $\delta$ totální a zkonstroujeme nový součinový automat
                - průnik: přijímáme slovo, pokud je přijato v obou zároveň 
                - sjednocení: přijímáme, pokud bylo přijato alespoň v jednom z automatů
                - rozdíl: přijímáme, pokud přijal první, ale nepřijal druhý
