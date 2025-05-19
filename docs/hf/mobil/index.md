# Mobilos házi feladat információk - 2025 tavasz

A házi feladat célja, hogy az előadáson és laborokon bemutatott technológiák segítségével egy komplex alkalmazást készítsen a hallgató, önálló funkcionalitással.

## Követelmények

- **Kötelező technológiák**: ((UI, RecyclerView, ViewBinding) vagy (Jetpack Compose, LazyList)) és (Room vagy Retrofit) 
- **Opcionális technológiák** (legalább 2): egyedi nézetek, fragmentek, egyéb perzisztens adattárolás, animációk, stílusok/témák, *Service*, *BroadcastReceiver*, *Content Provider*, chartok, egyéb külső könyvtárak, MVI/MVVM stb.
-	Kotlin nyelven kell készülnie
-	Önálló alkalmazás legalább 3-4 képernyővel/nézettel
-	Bármilyen külső könyvtár használható a fejlesztéshez, hogy még látványosabb alkalmazások készüljenek:
	-	[https://github.com/wasabeef/awesome-android-ui](https://github.com/wasabeef/awesome-android-ui)
	-	[https://github.com/nisrulz/android-tips-tricks](https://github.com/nisrulz/android-tips-tricks)
	-	[https://foso.github.io/Jetpack-Compose-Playground](https://foso.github.io/Jetpack-Compose-Playground)
	-	[https://www.jetpackcompose.app/compose-catalog](https://www.jetpackcompose.app/compose-catalog)

## Kötelező elemek részletes leírása

Az alkalmazásnak az alábbi funkciókat kell biztosítani:

- Adatkezelés
	- Room segítségével egy legalább 4 attribútumot tartalmazó entitást kell definiálni, és arra egy teljes CRUD-ot megvalósítani felülettel. Tehát kell felület elemek listájának megjelenítésére, egy elem megjelenítésére (amennyiben többletinformációt hordoz, mint a listanézet), egy elem hozzáadására, egy elem szerkesztésére, valamint egy elem törlésére.
	**VAGY**
	- Retrofit segítségével kell egy webes API-t feldolgozni. A követelmény, hogy legalább 3, egymástól különböző endpoint-ot kell az alkalmazásnak kezelnie.
- Felhasználói felület:
	- Legalább 4, egymástól érdemben különböző felületet kell kialakítani Activity-k, Fragmentek vagy Jetpack Compose segítségével.
	- Legalább egy Activity-ből, Fragmentből vagy Screenből meg kell tudni nyitni egy másik Activity-t, Fragmentet, vagy Screent és ennek az újonnan megnyílónak adatot kell átadni, ami ettől függően jelenik meg. (Például lista elemére kattintásnál, megnyílik egy új Activity, ami az elem részleteit tartalmazza.)
	- Legalább egy felületen listát kell megvalósítani RecyclerView vagy LazyList segítségével.
	- Két nagyon hasonló felület (pl új elem felvétele és szerkesztés) nem számít külön felületnek.
	- Egy olyan Activity vagy Fragment, amin csak és kizárólag egy Fragment, vagy egy statikus elemszámú ViewPager van, nem számít bele a fenti 4 felületbe!

## Példák

Az alábbiakban bemutatunk két specifikációt, ami a laborfeladatokat gondolja tovább, a fenti követelmények szemléltetése végett. Házi feladatnak olyan feladatot kell választani, ami ezektől (és a laborfeladatoktól) több lényegi elemében eltér:

- **Bevásárlólista**
	- Room segítségével, adatbázisban vannak tárolva a lista elemeinek adatai (név, leírás, ár, kategória).
	- Van egy listanézet (Activity), amiben a lista elemei fel vannak sorolva. (RecyclerView-val megvalósítva)
	- Van egy szerkesztő felület (DialogFragment), aminek a segítségével lehet elemeket hozzáadni, illetve szerkeszteni. Mivel ez a kettő nagyban hasonlít, nem számít külön elemnek.
	- Elemeket lehet törölni egy listanézetben lévő ikon segítségével. 
	- Amennyiben egy listaelemre kattintunk, megnyílik egy részletes nézet (Activity), ami tartalmazza a lista adatait, valamint két kördiagramot. Az egyiken az szerepel, hogy az összes elem ára hogyan viszonyul ennek az elemnek az árához, a másikon pedig az, hogy a kategóriáján belüli elemek árához hogyan viszonyul ennek az elemnek az ára.

- **Időjárás alkalmazás**
	- Van egy listanézet (Activity) a követett városokról. (RecyclerView-val megvalósítva). A listában levő elemek SharedPreferences segítségével vannak eltárolva.
	- Van lehetőség elemet hozzáadni, törölni a listából. Mivel ezekre vannak egyszerű beépített dialógusok, nem számítanak külön elemnek.
	- Egy városra kattintás után megnyílik egy új Activity, amin egy ViewPager van, három Fragmenttel. Ez nem számít önálló felületnek, de az adatátadás követelményét teljesíti. 
	- Az első Fragment az OpenWeatherMap API-jával kéri le a város aktuális időjárását és jeleníti meg az adatokat.
	- A második Fragment az OpenWeatherMap API-jával kéri le a város egy hetes előrejelzését. Az alapadatok megjelenítése mellett, egy vonaldiagramon ábrázolja a hőmérséklet várható alakulását.
	- A harmadik Fragment pedig szintén az OpenWeatherMap API-jával kéri le a város egy hónapos előrejelzését és jeleníti meg, mint az előző Frament. Ez nagyban hasonlít az előző Fragmenthez, így nem számít külön felületnek, de az API eltér, így a 3 endpoint követelményébe beleszámít.

!!!tip "Néhány egyéb példa alkalmazás"
	-	Kiadás/bevétel nyomkövető figyelmeztető funkcióval és grafikonokkal
	-	Turisztikai látványosságokat gyűjtő alkalmazás
	-	Raktár kezelő alkalmazás
	-	Számla kezelő megoldás
	-	Recept kezelő alkalmazás
	-	Napló készítő alkalmazás fényképekkel
	-	Sport tracker alkalmazás
	-	Készülék esemény naplózó alkalmazás
	-	Apróhirdetés alkalmazás
	-	Találkozó szervező alkalmazás
	-	Sportfogadó megoldás
	-	Szaki kereső alkalmazás
	-	Játék alkalmazás, pl. aknakereső, shooter, stb.
	-	Valamilyen REST API-t használó alkalmazás, például valuta váltás, tőzsdei infók, stb:
		-	[https://github.com/toddmotto/public-apis](https://github.com/toddmotto/public-apis)
		-	[https://github.com/Kikobeats/awesome-api](https://github.com/Kikobeats/awesome-api)
		-	[https://github.com/abhishekbanthia/Public-APIs](https://github.com/abhishekbanthia/Public-APIs)
	-	A házi feladat használhat felhő megoldást is, pl. Firebase, Amazon, stb.

Ha valaki a leírások alapján nem biztos benne, hogy az általa választott feladat megfelel a követelményeknek, egyeztessen a laborvezetőjével!

## Pontozás
A feladatra **12 pontot** lehet kapni, amiből legalább **6-ot** kell elérni ahhoz, hogy meglegyen a házi feladat.

- Amennyiben az alapkövetelmények, tehát a legalább négy nézet, adatbázis CRUD vagy három webes endpoint feldolgozása nem teljesül, a megoldás automatikusan, mérlegelés nélkül 0 pontot ér!
- Felhasználói felület (4pont)
	- Ergonómia (2 pont)
		- 0 pont: az oldal nehezen áttekinthető vagy sok, szembetűnő ergonómiai hiba látható,
		- 1 pont: az oldal áttekinthető, de kisebb felületi hibák, elcsúszások akadnak,
		- 2 pont: az oldal áttekinthető, a felületi elemek nincsenek szétcsúszva.
	- Erőforrások használata, szövegek erőforrásba szervezése (1 pont)
	- sűrűségfüggetlen mértékegységek használata (1 pont)
- Funkciók (5 pont)
	- Hatékony működés, nincsenek felesleges ciklusok, újrarajzolások (3 pont)
		- Különös tekintettel a RecyclerView-ra, diagramokra
		- Hosszú hívások aszinkron módon
	- Megfelelő hibakezelés (2pont)
		- Mi történik, ha a felhasználó érvénytelen adatot üt be (validáció)
		- Mi történik, ha az internet nem elérhető (hibaüzenetek)
		- Mi történik vissza gomb esetén
		- Mi történik elforgatás esetén
- Laborvezetői pontok (-9 – 3 pont)
	- Ezeket a laborvezető saját belátása szerint oszthatja az alábbiak alapján
		- Mennyire logikusan elvárt a működés
		- Mennyire történt törekvés a jó funkcionalitás megvalósítására, és nem csak a funkció látszat kivitelezése volt-e a cél
		- Mennyire érthető / jól strukturált a kód
	- A laborvezető itt negatív pontszámot is oszthat, amennyiben
		- Az alkalmazás összeomlik, lefagy
		- Igénytelen a kódminőség
		- Felkészületlen a bemutatás során a hallgató


## Beadás módja

A házi feladat beadása is, mint ahogyan a laboroké, GitHub Classroomon keresztül fog történni:

1. Moodle-ben keresd meg a házi feladathoz tartozó meghívó URL-jét és annak segítségével hozd létre a saját repository-dat.
1. Várd meg, míg elkészül a repository, majd checkout-old ki.
1. Hozz létre egy új ágat hf néven, és ezen az ágon dolgozz.


!!!danger "neptun.txt"
	A legfontosabb, hogy az eddigiekhez hasonlóan töltsd ki a neptun.txt fájlt, hogy a rendszer azonosítani tudjon.


A házi feladat beadás határideje a **14. heti labor**.
A házi feladat elkészítése közben a "hf" branchen dolgozz. Erre az ágra akárhány kommitot tehetsz. 
A projektet mindenképpen ebbe a repository-ba hozd létre, a fejlesztést végig itt végezd.
A beadás akkor teljes, ha a "hf" branch-en megtalálható a projekted teljes forráskódja. A beadást egy pull request jelzi, amely pull request-et a laborvezetődhöz kell rendelned.
A házi feladathoz mindenképpen tartozik **házi feladat védés** is. Ennek ideje a **14. heti laboron** van.

A házi feladatot nem szükséges külön dokumentálni, csak a Repository-ban lévő README.md fájl értelemszerű kitöltése szükséges.

### Házi feladat pótlás - fizetésköteles!

A pótbeadás határideje a **pótlási hét szerda**.
A házi feladat pótlása közben a "pothf" branchen dolgozz. Erre az ágra akárhány kommitot tehetsz. 
A beadás akkor teljes, ha a "pothf" branch-en megtalálható a projekted teljes forráskódja. A beadást egy pull request jelzi, amely pull request-et a laborvezetődhöz kell rendelned.
A házi feladat pótlásához mindenképpen tartozik **pót házi feladat védés** is. Ennek ideje a beadást követően van. Módjáról és idejéről egyeztess a laborvezetőddel.


## Bemutatás
A feladatot a **14. heti laboron** be kell mutatni a laborvezetőnek, kb. 5-10 percben. Ha az időpont nem megfelelő, a pótlásról a laborvezetővel kell egyeztetni. A laborvezető tetszőleges kérdést feltehet a kóddal, az alkalmazással kapcsolatban, akár megkérhet, hogy módosíts vagy magyarázz el valamit.

A bemutatás menete:

- Az alkalmazás funkcióinak bemutatása demóval:
	- Minden képernyő, funkcionális bemutatása
	- Hol van RecyclerView
	- Hol van adatátadás
	- Hol vannak fragmentek, diagrammok
	- Milyen adattárolást használ/Milyen hálózati hívást használ, aszinkronitás bemutatása
- Saját bevallás szerint a legszebb és legkevésbé szép megoldások megmutatása kódban.
- Laborvezetői kérdések
