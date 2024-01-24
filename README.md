# dg2psg

A `dg2psg` python könyvtár a (címkézetlen) függőségi gráf alapján határozza meg egy
adott mondatszakasz maximális megszakítatlan összetevőit (maxXP)

**Megszakítatlan összetevő**: olyan megszakítatlan szólánc, amelybe pontosan egy
függőségi él mutat a szóláncon kívülről.  
**Fej**: a megszakítatlan szólánc azon eleme, amelybe kívülről mutat él.  
**Maximális**: a megszakítatlan szóláncot balról vagy jobbról megnövelve már nem
megszakítatlan

## Fájlok

- `dg2psg.py` - a python library
- `dg2psg_demo.ipynb` - demo Jupyter notebook a `dg2psg` használatáról
  - Tagmondatok azonosítása CONLL fájlból
  - Tagmondatok azonosítása a Szeged Treebank 2.0-ban
  - Zárványt tartalmazó mondatok listázása
- `dg2psg_PRF.xlsx` - a `dg2psg` pontosságát és fedését bemutató excel fájl  
Az összehasonlítás a Szeged Dependency Treebank 8. osztályos elbeszélő 
fogalmazásaival történt. Az első száz mondatnál kézi hibaelemzést végeztem.
- `nohead.conll-2009` - zárványt tartalmazó mondatok a Szeged Dependency Treebankból


## A könyvtárban definiált függvények

A függőségi szerkezetet egész számok listájaként kell megadni: `d`, 
ahol a lista *n*-edik eleme (egytől számozva) a mondat *n*-edik szavának 
a függőségi viszonyát adja meg (melyik szónak a dependense).

`closedXP(start: int, end: int, d: list[int]) --> int`

- ellenőrzi, hogy a `d` függőségi listában a `start`-tal kezdődő és `end`-del végződő
mondatszakasz megszakítatlan e
- visszaadott érték: `NOHEAD` (-1), `MULTIHEAD` (-2) vagy a fej pozíciója

`firstMaxXP(start: int, end: int, d: list[int], checkNohead = False) --> (int, int, int)`  

- a `start...end` tartományba eső első maxXP t adja vissza: (XPstart, XPhead, XPend)
- a tartományt a vége felől csökkentve megkeresi az első olyan megszakítatlan
összetevőt, amelyik egy fejjel rendelkezik
- `checkNoHead=True` érték esetén ha a keresés esetén fejjel nem rendelkező 
mondatszakaszt talál, *Dependency graph is cyclic* exception-t dob

`allMaxXP(start: int, end: int, d: list[int], checkNohead = False) --> list[(int, int, int)]`  

- a `start...end` tartományba eső összes maxXP t adja vissza: bővítmények listája
- először az első maxXP t, majd az első végétől a tartomány végéig tartó elsőt stb.

`allMaxXP_recursive (start: int, end: int, d: list[int], checkNohead = False) --> list[(int, int, int)]`  

- a `start...end` tartományba eső összes maxXP t adja vissza, rekurzívan
- az `allMaxXP` által visszaadott maxXP k fej előtti szakaszában és fej utáni szakaszában
is megkeresi a maxXP ket (top down)
- a mondat felszíni összetevős szerkezetét adja meg

`allSpecXP(sentence, start: int, end: int, d: list[int], XPtest) --> list[(int, int, int)]`  

- az `allMaxXP_recursive` által megtalált összes maxXP közül csak azokat adja vissza,
amelyeket az `XPtest` külső függvény jóváhagy
- pl. ellenőrzi, hogy a maxXP feje ige-e -> tagmondatok listája
- a `sentence` argumentum a mondat eredeti reprezentációja, amely alapján 
az `XPtest` döntést hoz

## Idézés

Ha használnád a `dg2psg`, vagy csak megemltenéd, így hivatkozhatsz rá:

> Szécsényi Tibor 2024. Tagmondatok és megszakítatlan összetevők kinyerése
függőségi elemzésből. In Berend Gábor - Gosztolya Gábor - Vincze Veronika (szerk.) *XX. Magyar Számítógépes Nyelvészeti Konferencia*. Szeged: Szegedi Tudományegyetem, Informatikai Intézet. 217-228.
