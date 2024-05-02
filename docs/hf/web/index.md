# Webes házi feladat információk - 2024 tavasz

A házi feladat során egy olyan, Angular alapú webalkalmazást kell készíteni, ami tetszőleges elemeket (pl. könyvek, videojátékok, filmek, autók, focista-kártyák, kitűzők, kutyafajok, HTTP státuszkódok stb.) tud nyilvántartani.

## Kötelező elemek részletes leírása
Az alkalmazásban az alábbi funkcióknak kell elérhetőnek lenniük:

- **Listázás:** az elemek listázása egy áttekintő nézetben. A nézetben lehet váltani listás és kártyás megjelenítés között. Kártyás nézetben az elem képe is jelenjen meg, illetve rácsos (sorok és oszlopok) elrendezésben jelenjenek meg az elemek. Listás nézetben függőlegesen, egymás után jelenjenek meg az elemek, csak fejléc adatokkal (pl. kép, leírás, stb. nélkül). Egy elem részleteit az elemre kattintva lehet megtekinteni.
- **Részletezés:** a listanézetben egy elemet kiválasztva megtekinthetők annak részletes tulajdonságai (legalább 3 különböző, egyedi tulajdonság, pl.: szerző, kiadás éve, műfaj vagy HTTP státuszkód száma, leírása, link egy tetszőleges forrásra). A részletező oldalon látható az elemről annak képe.
- **Létrehozás/szerkesztés:** egy elem létrehozása és szerkesztése ugyanazon a felületen történik. A különbség annyi, hogy létrehozáskor a lehetséges mezők üresek, szerkesztéskor pedig a mezők az elem adataival vannak feltöltve.
- **Törlés:** az elem részletes oldalán van lehetőség az elem törlésére. Törlés előtt felugró üzenet (modális ablak) kér megerősítést a törlés előtt. Törlés után a listázás oldalra kerülünk.

Az oldalnak megfelelően strukturált URL-eket kell definiálnia (routing segítségével):

- a kezdőoldal (/) tipikusan a listázás felületet hozza be, vagy oda irányít át (pl. /dogs)
- a részletes oldal az URL-ben tartalmazza az elem azonosítóját (pl. /dogs/5)
- a szerkesztés oldal a részletes oldal alatt van hierarchiáját tekintve (pl. /dogs/5/edit)
- a létrehozás oldal testvére a részletezésnek (pl. /dogs/new)
- a törlésnek nincs külön oldala (felugró ablak jelenik meg, így lehet törölni)

??? warning "FONTOS: Adatok tárolása és routing"
    A két Angular labor során előkerültek az Angular alapvetői funkciói és készítünk egy egyszerűbb alkalmazást. Nem foglalkoztunk viszont azzal, hogy hogyan kommunikálunk egy szerverrel (minden logika a kliensben van jelenleg), illetve [`routing`](https://angular.io/guide/router)-ot sem valósítottunk meg. A szerverrel való kommunikáció a tárgy szempontjából nem olyan fontos (házi feladatnál kiváltható adatok memóriában tárolásával, vagy akár nyers mock adatok használatával is), viszont a routing-ot érdemes részletesebben megnézni! Ezek a fogalmak egy harmadik Angular labor keretein belül pár éve szóba kerültek, viszont az elmaradó laborok miatt erre idén nincs lehetőség. A régi laboranyag még [elérhető](https://github.com/bmeaut/VIAUBB03/tree/master/Web/07%20-%20Angular%203), itt is találhatunk információkat a fenti kimaradt problémákról, amik a házi feladat készítése során jól jöhetnek - de ez nem kötelező, bőven elegendő a hivatalos Angular dokumentáció használata is.

    Az alkalmazásnak tehát nem kötelező szervert használnia, helyette például [ezen megoldást](https://angular.io/tutorial/toh-pt6#simulate-a-data-server) lehet használni az adatok memóriában (vagy localStorage-ban) történő tárolására. Használható tetszőleges publikus API is. Ezeken kívül legegyszerűbb módszerként akár nyers mock adatok is használhatók. A követelmények minden esetben ugyanazok maradnak, pontlevonás nem jár egyik megoldás használatáért sem.

Stílusozásra a [Bootstrap](https://getbootstrap.com) és [NgBootstrap](https://ng-bootstrap.github.io/) könyvtárakat kell használni.

A DOM manipuláció közvetlenül kódból (a document objektumon keresztül vagy pl. jQuery-ből) **tilos**, kizárólag Angular adatkötéseket szabad használni a felületi adatok megjelenítésére.

## Pontozás

- Funkciók (0-15 pont):
    - létrehozás: 1 pont
    - kártyás nézet: 2 pont
    - listás nézet: 2 pont
    - részletezés: 1 pont
    - szerkesztés: 2 pont
    - törlés: 2 pont
    - reszponzivitás: 2 pont
    - megfelelő útvonalválasztás (routing): 2 pont
    - karbantartható, strukturált kód: 1 pont
- Ergonómia (0-5 pont):
    - 0 pont: az oldal dizájnnal gyakorlatilag nem rendelkezik
    - 1-2 pont: az oldal nehezen áttekinthető, vagy pedig sok szembetűnő ergonómiai hiba látható
    - 3-4 pont: az oldal áttekinthető, de kisebb felületi hibák, elcsúszások akadnak, vagy natív alert/prompt/confirm ablakok vannak stílusozott modális ablakok helyett törlésnél
    - 5 pont: az oldal áttekinthető, a felületi elemek nincsenek szétcsúszva
- Funkcionális hiba, infrastrukturális hiányosság esetén 1 – 10 pont levonás járhat (darabonként)

## Beadási határidő és bemutatás

A házi feladatot **május 1. 23:59-ig** kell beadni, pull request formájában, a kiadott [**github classroom repository**](https://classroom.github.com/a/0I2F4uAi) alatt. A repository-ban a "feladat" mappa alá dolgozz! Commit-nál a felesleges fájlok (különösen az .angular mappa és a node_modules mappa) ne kerüljenek fel, csak a forráskód! A kiadott .gitignore ezeket kiszűri, viszont push előtt ellenőrizd, hogy valóban így van-e! A repository tartalma alapján elő kell tudni állítani a működő alkalmazást, tehát fontos fájlok ne maradjanak ki! Ha mindent jól csináltál, akkor a repository mérete (leklónozva) nem nagyobb, mint 5MB (az esetlegesen mellékelt képek méretétől függően), valamint `npm install`-t és `ng serve`-t futtatva a HF hibátlanul fordul és az alkalmazás működik. Ha az alkalmazás ennek megfelelően nem indul el, úgy pontlevonás vagy 0 pont adható.

A házi feladatot **május 2-án kell bemutatni** a laborvezetőnek, online, Teams-en, kb. 10 percben. Beosztás (vagyis hogy ezen a napon belül pontosan mely időpontokban lehet majd bemutatni) később várható. A laborvezető tetszőleges kérdést feltehet a kóddal, az alkalmazással kapcsolatban, akár megkérhet, hogy módosíts vagy magyarázz el valamit. A webes házi feladatra maximum 20 pont kapható, ami beleszámít a félévvégi jegybe. A házi feladatot kötelező sikeresen teljesíteni, ez 10 ponttól (50%) van meg.

A bemutatás menete a következő:

- Az alkalmazás funkcióinak bemutatása
    - létrehozás
    - listázás
    - kártyás/listás nézet
    - részletezés
    - szerkesztés
    - törlés
    - reszponzivitás
- Saját bevallás szerint a legszebb és legkevésbé szép megoldások megmutatása kódban
- Laborvezetői kérdések