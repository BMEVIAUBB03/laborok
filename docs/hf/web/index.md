# Webes házi feladat információk - 2025 tavasz

A házi feladat során egy olyan, Preact alapú webalkalmazást kell készíteni, ami tetszőleges elemeket (pl. könyvek, videojátékok, filmek, autók, focista-kártyák, kitűzők, kutyafajok, HTTP státuszkódok stb.) tud nyilvántartani.

## Kötelező elemek részletes leírása
Az alkalmazásban az alábbi funkcióknak kell elérhetőnek lenniük:

- **Listázás:** az elemek listázása egy áttekintő nézetben. A nézetben lehet váltani listás és kártyás megjelenítés között. Kártyás nézetben az elem képe is jelenjen meg - tehát fontos, hogy az elemekhez valamilyen módon kép is tartozzon -, illetve rácsos (sorok és oszlopok) elrendezésben jelenjenek meg az elemek. Listás nézetben függőlegesen, egymás után jelenjenek meg az elemek, csak fejléc adatokkal (pl. kép, leírás, stb. nélkül). Egy elem részleteit az elemre kattintva lehet megtekinteni.
- **Részletezés:** a listanézetben egy elemet kiválasztva megtekinthetők annak részletes tulajdonságai (legalább 3 különböző, egyedi tulajdonság, pl.: szerző, kiadás éve, műfaj vagy HTTP státuszkód száma, leírása, link egy tetszőleges forrásra). A részletező oldalon látható az elemről annak képe is.
- **Létrehozás:** új elem létrehozásánál meg kell adnunk annak minden adatát.
- **Szerkesztés:** egy meglévő elem minden adatát lehessen szerkeszteni (az ID kivételével). A szerkesztés felület lehet a létrehozással egy komponensben, vagy külön is megvalósítva.
- **Törlés:** a részletezés oldalon legyen lehetőség az elem törlésére. Törlés előtt egy felugró üzenet (modális ablak) kér megerősítést a törlés előtt. Törlés után a listázás oldalra kerülünk.

Az oldalnak megfelelően strukturált URL-eket kell definiálnia (routing segítségével):

- a kezdőoldal (/) tipikusan a listázás felületet hozza be, vagy oda irányít át (pl. /dogs)
- a részletes oldal az URL-ben tartalmazza az elem azonosítóját (pl. /dogs/5)
- a szerkesztés oldal a részletes oldal alatt van hierarchiáját tekintve (pl. /dogs/5/edit)
- a létrehozás oldal testvére a részletezésnek (pl. /dogs/new)
- a törlésnek nincs külön oldala (felugró ablak jelenik meg, így lehet törölni)

??? warning "FONTOS: Adatok tárolása"
    A két React labor során előkerültek a Preact alapvetői funkciói és készítettünk egy egyszerűbb alkalmazást. Nem foglalkoztunk viszont azzal, hogy hogyan kommunikálunk egy szerverrel. A szerverrel való kommunikáció a tárgy szempontjából nem fontos (házi feladatnál kiváltható adatok memóriában tárolásával, vagy akár nyers mock adatok használatával is).

    Az alkalmazásnak tehát nem kötelező szervert használnia, helyette beégetett adatokkal (memóriában, local storage-ben) vagy - a TypeScript laborhoz hasonló módon - publikus API-val is lehet dolgozni. Pontlevonás nem jár egyik megoldás használatáért sem.

Stílusozásra a gyakorlatokhoz hasonlóan a [Bootstrap](https://getbootstrap.com) könyvtárat kell használni.

A DOM manipuláció közvetlenül kódból (a document objektumon keresztül vagy pl. jQuery-ből) **tilos**, kizárólag Preact adatkötéseket szabad használni a felületi adatok megjelenítésére, a laborokhoz hasonló módon.

## Pontozás

A webes házi feladatra maximum 18 pont kapható, ami beleszámít a félévvégi jegybe. A házi feladatot kötelező sikeresen teljesíteni, ez 9 ponttól (50%) van meg. A pontok a következőképpen oszlanak el:

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
- Ergonómia (0-3 pont):
    - 0 pont: az oldal dizájnnal gyakorlatilag nem rendelkezik
    - 1 pont: az oldal nehezen áttekinthető, vagy pedig sok szembetűnő ergonómiai hiba látható
    - 2 pont: az oldal áttekinthető, de kisebb felületi hibák, elcsúszások akadnak, vagy natív alert/prompt/confirm ablakok vannak stílusozott modális ablakok helyett törlésnél
    - 3 pont: az oldal áttekinthető, a felületi elemek nincsenek szétcsúszva
- Funkcionális hiba, infrastrukturális hiányosság esetén 1 – 10 pont levonás járhat (darabonként)

## Beadás és bemutatás

A házi feladat megoldása során - a laboroknál megszokott módon - ne felejtsd el követni a [feladatbeadás folyamatát](../../tudnivalok/github/GitHub.md).

### Git repository létrehozása és letöltése

1. A Moodle-ben keresd meg a laborhoz tartozó meghívó URL-jét, és annak segítségével hozd létre a saját repositorydat.
2. Várd meg, míg elkészül a repository, majd checkoutold ki.
3. Hozz létre egy új ágat `megoldas` néven, és ezen az ágon dolgozz.
4. A `neptun.txt` fájlba írd bele a Neptun-kódodat. A fájlban semmi más ne szerepeljen, csak egyetlen sorban a Neptun-kód 6 karaktere.

### Beadás

A házi feladatot **április 9. 23:59-ig** kell beadni, pull request formájában, a kiadott **github classroom repository** alatt. A classroom meghívót moodle-n találod, a laborokhoz hasonlóan. A repository-ban a "feladat" mappa alá dolgozz! Commit-nál a felesleges fájlok (különösen a node_modules mappa) ne kerüljenek fel, csak a forráskód! A kiadott .gitignore ezeket kiszűri, viszont push előtt ellenőrizd, hogy valóban így van-e! A repository tartalma alapján elő kell tudni állítani a működő alkalmazást, tehát fontos fájlok ne maradjanak ki! Ha mindent jól csináltál, akkor a repository mérete (leklónozva) nem nagyobb, mint 5MB (az esetlegesen mellékelt képek méretétől függően), valamint `npm install` és `npm run dev` (vagy hasonló) parancsot futtatva a HF hibátlanul fordul és az alkalmazás működik. Ha az alkalmazás ennek megfelelően nem indul el, úgy pontlevonás vagy 0 pont adható.

### Bemutatás

A házi feladatot **április 10-én kell bemutatni** a laborvezetőnek, online, Teams-en, kb. 10 percben. Beosztás (vagyis hogy ezen a napon belül pontosan mely időpontokban lehet majd bemutatni) később várható. Ha hamarabb elkészülnél a házival, igény szerint korábban is megbeszélhető bemutatási időpont a laborvezetővel. A bemutatás során a laborvezető tetszőleges kérdést feltehet a kóddal vagy alkalmazással kapcsolatban, akár megkérhet, hogy módosíts vagy magyarázz el valamit. A bemutatás menete a következő:

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