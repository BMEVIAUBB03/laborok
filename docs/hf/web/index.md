# Webes házi feladat információk - 2023 tavasz

A házi feladat során egy olyan, Angular alapú webalkalmazást kell készíteni, ami tetszőleges elemeket (pl. könyvek, videojátékok, filmek, autók, focista-kártyák, kitűzők, kutyafajok, HTTP státuszkódok stb.) tud nyilvántartani.

## Kötelező elemek részletes leírása
Az alkalmazásban az alábbi funkcióknak kell elérhetőnek lenniük:

- **Listázás:** az elemek listázása egy áttekintő nézetben. A listanézetben lehet váltani listás és kártyás megjelenítés között. Egy elem részleteit az elemre kattintva lehet megtekinteni.
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
    A két Angular labor során előkerülnek az Angular alapvetői funkciói és készítünk egy egyszerűbb alkalmazást. Nem foglalkozunk viszont azzal, hogy hogyan kommunikálunk egy szerverrel (minden logika a kliensben van jelenleg), illetve [`routing`](https://angular.io/guide/router)-ot sem valósítottunk meg. A szerverrel való kommunikáció a tárgy szempontjából nem olyan fontos (házi feladatnál kiváltható adatok memóriában tárolásával, vagy akár nyers mock adatok használatával is), viszont utóbbit érdemes részletesebben megnézni! Ezek a fogalmak egy harmadik Angular labor keretein belül pár éve szóba kerültek, viszont az elmaradó laborok miatt erre idén nincs lehetőség. A régi laboranyag még [elérhető](https://github.com/bmeaut/VIAUBB03/tree/master/Web/07%20-%20Angular%203), itt is találhatunk információkat a fenti kimaradt problémákról, amik a házi feladat készítése során jól jöhetnek - de ez nem kötelező, bőven elegendő a hivatalos Angular dokumentáció használata is.

    Az alkalmazásnak tehát nem kötelező szervert használnia, helyette például a következő megoldást lehet használni az adatok memóriában (vagy localStorage-ban) történő tárolására: https://angular.io/tutorial/toh-pt6#simulate-a-data-server. Használható tetszőleges publikus API is. Ezeken kívül legegyszerűbb módszerként akár nyers mock adatok is használhatók. A követelmények minden esetben ugyanazok maradnak, pontlevonás sem fog járni egyik megoldás használatáért sem.

Stílusozásra a Bootstrap (https://getbootstrap.com) és NgBootstrap (https://ng-bootstrap.github.io/) könyvtárakat kell használni.

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

??? warning "FONTOS: Időpontok változása"
    Ha ezt látod, az azt jelenti, hogy az időpontok még változhatnak és nemsokára (várhatóan április elejére) véglegesek lesznek!

A házi feladatot **május 3. 23:59-ig** kell feltölteni moodle-re, a feltöltési útmutatót (vagyis hogy milyen fájlokat kell pontosan feltölteni) is ott találod. A házi feladatot **május 4-én kell bemutatni** a laborvezetőnek, online, Teams-en, kb. 10 percben. Beosztás (vagyis hogy május 4-én belül pontosan mely időpontokban lehet majd bemutatni) később várható. A laborvezető tetszőleges kérdést feltehet a kóddal, az alkalmazással kapcsolatban, akár megkérhet, hogy módosíts vagy magyarázz el valamit.

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