# *** Régi tematika *** - Labor 05 – Angular bevezető

## Bevezetés

A laboron egy klasszikus táblajáték első részét fogjuk elkészíteni az Angular keretrendszer segítségével. Az előző webes laborok ismereteinek megszerzése jelen labor elvégzéséhez erősen ajánlott.

Felhasznált technológiák és eszközök:

- webböngészők beépített hibakereső eszközei,
    - javasolt az [új Microsoft Edge](https://www.microsoft.com/en-us/edge) vagy a [Google Chrome](https://www.google.com/chrome/) böngésző használata,

- npm, a [NodeJS](https://nodejs.org/en/download/) csomagkezelője,
    - a laborhoz használható mind a Current, mind az LTS verzió, de ha még nem telepítetted a NodeJS-t, akkor érdemes a Currentet telepíteni (ha korábbi verzió van telepítve, akkor pedig a Currentre frissíteni),

- [Visual Studio Code](https://code.visualstudio.com/download) kódszerkesztő alkalmazás,
    - otthoni vagy egyéni munkavégzéshez használható bármilyen más kódszerkesztő vagy fejlesztőkörnyezet, de a környezet kapcsán felmerülő eltérésekről önállóan kell gondoskodni,
- javasolt Visual Studio Code bővítmények:
    - [Angular Language Service](https://marketplace.visualstudio.com/items?itemName=Angular.ng-template) segítségével még a HTML kódban is kapunk fordítási elemzést és kódkiegészítést,
    - érdemes lehet feltelepíteni a böngészőnknek megfelelő bővítményt VS Code-hoz, amivel debugolni tudunk:
        - [Debugger for Microsoft Edge](https://marketplace.visualstudio.com/items?itemName=msjsdiag.debugger-for-edge)
        - [Debugger for Chrome](https://marketplace.visualstudio.com/items?itemName=msjsdiag.debugger-for-chrome)
        - [Debugger for Firefox](https://marketplace.visualstudio.com/items?itemName=firefox-devtools.vscode-firefox-debug)

Ahol nincs külön kifejezetten jelölve, a szoftverek legfrissebb stabil verzióit érdemes használni. Ha már van telepítve korábbról globális csomag, pl. az Angular CLI, akkor azokat érdemes frissíteni a legfrissebb verziókra.

??? warning "Nagyméretű függőségek"
    Az Angular laborok során számos, viszonylag nagyméretű (többszáz megabájt) függőségi csomag letöltésére lesz szükség (az `npm install` parancs hatására), de ha valaki már sikeresen telepítette egy korábbi labor során (ha volt már) az NPM csomagokat, akkor azokat várhatóan nem kell újra letölteni.

??? note "Az Angular-ről dióhéjban"
    Érdemes bevezetőként megnézni a hivatalos dokumentáció áttekintő oldalát: [https://angular.io/guide/architecture](https://angular.io/guide/architecture).

    <figure markdown>
      ![Angular architektúra áttekintése](https://angular.io/generated/images/guide/architecture/overview2.png)
      <figcaption>Angular architektúra áttekintése</figcaption>
    </figure>

    Az [Angular](https://angular.io/) egy komplett alkalmazásfejlesztési keretrendszer, aminek segítségével böngészőben futtatható, JavaScript alapú kliensalkalmazásokat tudunk írni. Általában bonyolultabb *single-page application*-ök készítésére szoktuk használni, egyszerűbb esetekben nem biztos, hogy a legjobb megoldás. Az Angular kulcsgondolatai:

    - [Komponensek](https://angular.io/guide/architecture-components): az alkalmazás egymásba ágyazható elemei, amik egy adott felületi elem (oldal, menü, gomb stb.) kirajzolásáért és adatkötéséért felelősek.
    - [Adatkötés](https://angular.io/guide/displaying-data): az adatkötés az adat felületre történő kirajzolását, a felületről beérkező (felhasználói) események kezelését, illetve ezek kombinációját (kétirányú adatkötés) jelenti.
    - [Szolgáltatások](https://angular.io/guide/architecture-services): az alkalmazásunkban olyan konkrét részfunkcionalitásért felelős osztályok, amelyeket akár több komponens is szeretne használni. [Dependency Injection](https://angular.io/guide/dependency-injection) támogatja a használatukat.
    - [Modulok](https://angular.io/guide/architecture-modules): az alkalmazásunk összefüggő komponensei, szolgáltatásai (direktívái, csővezetékei, routerei stb.), amelyek egységesen importálhatók más modulokba.
        - Maga az Angular is moduláris, a keretrendszer különböző funkcióit a megfelelő modulok, azokból komponensek, szolgáltatások (stb.) importálásával tudjuk elérni, ehhez a TypeScript (JavaScript) `import` kulcsszavát fogjuk használni, pl. `import { AppModule } from './app/app.module'; ` vagy `import { NgModule } from '@angular/core';`.
    - [Sablonozónyelv (template syntax)](https://angular.io/guide/template-syntax): az Angular saját "nyelve", ami HTML és TypeScript elemeket, valamint néhány egyedi szintaktikai elemet tartalmaz.
    - Sablonok: az Angular komponensek egy mögöttes, logikát és struktúrát leíró TypeScript fájlból és általában egy ehhez tartozó HTML fájlból tevődnek össze. A HTML fájl a mögöttes TypeScript fájlban definiált komponenshez fog adatot és eseményt kötni.
    - [Angular CLI](https://angular.io/cli): az Angular Command Line Interface (CLI) egy [npm](https://www.npmjs.com/get-npm)-ből telepíthető parancssori eszköz, aminek segítségével összeállíthatjuk a kiinduló projektünket, új fájlokat, komponenseket hozhatunk létre előre összeállított formában.
    - Dekorátorok: a TypeScript [dekorátorai](https://www.typescriptlang.org/docs/handbook/decorators.html#class-decorators) segítségével az Angular osztályainkhoz, tulajdonságokhoz metaadatokat rendelhetünk, ld. az alábbi példán a `@Component` dekorátort:

    ```TS
    import { Component } from '@angular/core';

    @Component({
      selector: 'app-root',
      templateUrl: './app.component.html',
      styleUrls: ['./app.component.scss']
    })
    export class AppComponent {
      /* ... */
    }

    ```

### Git repository létrehozása és letöltése

A feladatok megoldása során ne felejtsd el követni a feladat beadás folyamatát ([GitHub](../../tudnivalok/github/GitHub.md)).

1. A Moodle-ben keresd meg a laborhoz tartozó meghívó URL-jét és annak segítségével hozd létre a saját repository-dat.
2. Várd meg, míg elkészül a repository, majd checkout-old ki.
    * Egyetemi laborokban, ha a checkout során nem kér a rendszer felhasználónevet és jelszót, és nem sikerül a checkout, akkor valószínűleg a gépen korábban megjegyzett felhasználónévvel próbálkozott a rendszer. Először töröld ki a mentett belépési adatokat (lásd [itt](../../tudnivalok/github/GitHub-credentials.md)), és próbáld újra.
3. Hozz létre egy új ágat `megoldas` néven, és ezen az ágon dolgozz.
4. A `neptun.txt` fájlba írd bele a Neptun-kódodat. A fájlban semmi más ne szerepeljen, csak egyetlen sorban a Neptun-kód 6 karaktere.

### A feladat

A feladat a klasszikus [Mastermind](https://en.wikipedia.org/wiki/Mastermind_(board_game)) táblajáték kliensalkalmazás elkészítése lesz. Ennek sok variánsa létezik, a játék szabályai nálunk az alábbiak lesznek:

- A "gép" (a kódmester) sorsol egy véletlenszerű, 4 hosszúságú sorozatot az alábbi 6 színű golyóból: piros, lila, kék, zöld, sárga, narancs.
    - Ugyanaz a szín többször is szerepelhet a sorrendben, pl.: kék, sárga, sárga, kék, zöld, lila.
- A játékos (a kódfejtő) megpróbálja megtippelni a kódmester által kisorsolt színsorozatot. Minden tippre a játékos visszajelzést kap: 
    - annyi **fekete** színű jelzést kap a játékos, ahány helyen **eltalálta** a kisorsolt elemet,
    - annyi **fehér** színű jelzést kap *ezeken felül*, ahány szín **helyes, de rossz helyen van** a sorrendben.
- A játékosnak 10 körön belül ki kell találnia az eredeti sorrendet.

Ezen labor során a játék kezdőképernyőjét készítjük el, a következő alkalommal innen folytatjuk!

???+ info "Specifikáció"
    A kezdőoldalon maga a játék jelenjen meg. A játéktéren rácselrendezésben jelenik meg 10 sor, minden sorban 4 üres kör, ezek jelzik majd a tippelt színeket, ezek mellett pedig 4 kisebb üres kör, ahol a fekete/fehér jelzők fognak szerepelni.

    A sorok fölött az aktuális tippünket fogjuk összeállítani, így megjelenik ott is 4 üres kör. Ez alatt megjelenik a 6 különböző színű golyó: piros, lila, kék, zöld, sárga, narancs. Az egyes golyókra kattintva az bekerül a balról következő üres helyre (ha van még). Ha a tippünkben egy golyóra kattintunk, akkor az kikerül a sorból, az utána következő elemek pedig balra csúsznak eggyel. Ha minden hely megtelt, aktiválódik egy gomb, amivel el tudjuk küldeni a tippünket.

    Miután elküldjük a tippet, a tippünk bekerül a 10 sorból az első üres sorba, és megjelenik mellette, hogy hány golyó van megfelelő helyen (fekete jelző) és hány van rossz helyen (fehér jelző).

    Ha a tippünk talált (4 fekete jelző), a játék jelzi, hogy nyertünk, és új játékot kezdhetünk. Ha a 10. próbálkozás sem talál, akkor veszítettünk, és új játékot kezdhetünk. Amikor a játéknak vége van, felfedésre kerül, hogy mi volt az eredetileg sorsolt sorrend.


## 1. feladat – Kiindulóprojekt

A gépre telepítve kell lennie az Angular CLI eszköznek. Az Angular CLI egy npm parancssori parancs, a NodeJS telepítésekor a globálisan telepített eszközök így bekerülnek a PATH változóba, így parancssorból egyszerűen az `ng` parancs futtatásával érhető el, ezért javasolt globálisan telepíteni az eszközt. Nyissunk meg a kiinduló repository `feladat` mappáját VS Code-ban, majd a Terminalban (`Ctrl+Ö`) adjuk ki az alábbi parancsot:

> `npm install -g @angular/cli`

Az esetleges warningokat (`WARN`) figyelmen kívül hagyhatjuk, csak az error jelzésű sorok jeleznek hibát.

Ezután adjuk ki az alábbi parancsot, ami egy új, üres projektet hoz nekünk létre a jelenlegi útvonalon:

> `ng new mastermind --prefix=mm --style=scss --skip-tests=true --routing=false --no-standalone --directory=.`


Az Angular felteheti az alábbi kérdést, amire válaszoljunk a preferenciánknak megfelelően:
> Would you like to enable autocompletion? This will set up your terminal so pressing TAB while typing Angular CLI commands will show possible options and autocomplete arguments. (Enabling autocompletion will modify configuration files in your home directory.)

Az alábbi kérdést is felteheti, amire válaszoljunk `n`-nel:
> Would you like to share anonymous usage data with the Angular Team at Google under Google’s Privacy Policy at https://policies.google.com/privacy? For more details and how to change this setting, see http://angular.io/analytics.

Az alábbi kérdést is felteheti, amire válaszoljunk `y`-nal:
> Do you want to enable Server-Side Rendering (SSR) and Static Site Generation (SSG/Prerendering)?

???+ tip "ng parancs különböző környezetekben"
    Ha valaki járatos a PowerShell világában, akkor érdemes tisztában lenni vele, hogy a globális `ng` parancs futtatása PowerShellből a PATH-ban található `ng.ps1` PowerShell scriptet preferálja az `ng.cmd` helyett. Mivel a PS1 szkript nem digitálisan aláírt, ezért nem futtatható anélkül, hogy a felhasználó ehhez kifejezetten hozzá ne járulna. Az aláíratlan PowerShell szkriptek futtatása veszélyes lehet, ezért csak akkor engedélyezzük az aláíratlan szkript futtatását, ha tudjuk, mit csinálunk (vagy kontrollált környezetben, pl. konténerben vagy virtuális gépen futunk, amit nem zavar, ha tönkretesztünk). Az aláíratlan szkriptek engedélyezéséhez lásd a [hivatalos leírást](https://go.microsoft.com/fwlink/?LinkID=135170). Más operációs rendszereken vagy más shell használatával (pl. bash) az ng parancs meghívásának szintaxisa változhat (pl. `ng`, `ng.cmd` vagy `ng.ps1`)

    ***FONTOS:*** a fentiek tükrében a későbbiekben futtatandó parancsokban értelemszerűen használjuk az `ng` parancs helyett az `ng.cmd`-t, ha szükséges.


A parancs futtatásakor az eszköz megválaszolandó kérdéseket tehet fel, pl. azzal kapcsolatban, hogy engedélyezzük-e a [strict](https://angular.io/guide/strict-mode) módot. Ezt érdemes engedélyezni, mivel manapság a TypeScript esetén a strict mode best practice-nek számít.

A parancshoz kapcsolódó kapcsolók értelmezéséhez a [dokumentáció](https://angular.io/cli/new) ad segítséget. Kiemelendő a `prefix` kapcsoló, amivel az automatikusan generált komponensek CSS selectorainak prefixét adjuk meg. Ez alapértelmezetten az `app` értéket veszi fel, tehát a KutyaComponent pl. az `app-kutya` selectorral lenne elérhető, ehelyett ezt a paraméter megadásával `mm-kutya`-ként fogjuk tudni elérni.

???+ tip "Ha az ng parancs nem futna le"
    Ha a parancs nem fut le, mert nem találja a `ng.cmd`/`ng` parancsot, ellenőzizzük, hogy a rendszer PATH környezeti változójában megtalálható-e az `%APPDATA%\npm` bejegyzés, és ha nem, **vegyük fel, majd indítsuk újra a VS Code-ot**, hogy beolvassa az új környezeti változókat!

    <figure markdown>
      ![Környezeti változók Windows-on](./assets/env_vars.png)
      <figcaption>Környezeti változók Windows-on</figcaption>
    </figure>

    <figure markdown>
      ![A PATH környezeti változó Windows-on](./assets/env_vars_path.png)
      <figcaption>A PATH környezeti változó Windows-on</figcaption>
    </figure>

    Ha ezután sem sikerül futtatni az `ng` parancsot, az útmutatóban szereplő `ng` parancsokat mindenütt cserélni szükséges az alábbiak valamelyikére: `npm run ng` vagy `.\node_modules\.bin\ng.cmd` (ez utóbbi nem a gépen telepített globális, hanem az aktuális mappában, lokálisan telepített Angular CLI-t futtatja).

Ha létrehoztuk a kiinduló projektet, el tudjuk indítani azt az alábbi paranccsal:

> `ng serve`

Ezzel a paranccsal egy **fejlesztésre használható** (éles bevetésre nem alkalmas) szerver szolgálja ki az Angular alkalmazásunkat. Figyeli a háttérben a változásokat, szükség esetén a megfelelő részeket újrafordítja, és frissíti a böngésző megfelelő részeit (nem a teljes oldalt).

Kis idő után az alábbihoz hasonlót látunk:
```
Initial Chunk Files | Names         |  Raw Size
polyfills.js        | polyfills     |  83.46 kB |
main.js             | main          |  21.80 kB |
styles.css          | styles        |  96 bytes |

                    | Initial Total | 105.35 kB

Application bundle generation complete. [2.646 seconds]
Watch mode enabled. Watching for file changes...
  ➜  Local:   http://localhost:4200/
  ➜  press h + enter to show help
```

Nyissuk meg tehát a böngészőt a <a href="http://localhost:4200" target="_blank">`http://localhost:4200`</a>-on (ha magától nem nyílna meg)!

Az `ng serve` parancsot hagyjuk a háttérben futni. Ha új parancsokat kell végrehajtanunk, nyissunk egy új terminált a `Ctrl+Shift+Ö` billentyűkombinációval! **FONTOS!** Ha a fordítás hibát jelez, és úgy gondoljuk, hogy mégsincs hiba, akkor állítsuk le az `ng serve` parancsot (pl. `Ctrl+C`), és indítsuk újra. Ez akkor fordulhat elő esetenként, ha például fájlt törlünk vagy új függőségi csomagot hivatkozunk be.

???+ tip "Debugolás"
    Debugoláshoz (a megfelelő bővítmények telepítése után) nyomjuk meg az `F5` billentyűt VS Code-ban, válasszuk ki a lenyíló menüben a megfelelő debuggert (Edge/Chrome, amit az imént telepítettünk), és a létrejövő `launch.json` fájlban változtassuk meg az URL-t, hogy az a 4200-as portra mutasson. Ezután az `F5` billenyűvel indíthatjuk bármikor a debugolást (ehhez természetesen az `ng serve`-nek futnia kell). Ekkor a VS Code-ban elhelyezett töréspontokat (`F9`) meg fogjuk ütni, és megvizsgálhatjuk pl. a változók értékét a Debug fülön vagy az egérrel a változó nevére mutatva a kódban, vagy használhatjuk a Watch lehetőségeket, átléphetünk parancsokon stb.

    **FONTOS!** Ha problémába ütközöl/nem várt működést tapasztalsz, __mindig__ használd a debuggert, vizsgáld meg a környező változók állapotát!

<figure markdown>
  ![A kiinduló Angular projekt böngészőben](./assets/start.png)
  <figcaption>A kiinduló Angular projekt böngészőben</figcaption>
</figure>

Vizsgáljuk meg a létrejött projekt tartalmát (a számunkra jelenleg relevánsak vannak itt csak kiemelve):

- **node_modules**: a számos függőség, ami a kiinduló projekthez kell (maga az Angular és függőségei)
- **angular.json**: az alkalmazásunk Angular konfigurációja
- **package.json**: az alkalmazásunk függőségeinek listája, ide tudjuk felvenni az npm csomagjainkat függőségként (vagy az `npm i [függőség_neve]` paranccsal ebbe a fájlba kerülnek be)
- **tsconfig.json**: az alkalmazásunk TypeScript konfigurációja
- **src**: az alkalmazásunk teljes forráskódja, elsősorban itt fogunk dolgozni
    - **index.html**:
        - a kiinduló fájlunk, gyakorlatilag semmi érdemi nem található benne, az Angular build fogja kitölteni a megfelelő `<script>` és egyéb hivatkozásokkal
        - a törzsben található egy `<mm-root>` nevű elem, ami az alkalmazásunk gyökéreleme, erről még lesz szó később
    - **main.ts**: az alkalmazás belépési pontja, ez állítja össze magát az alkalmazást és indítja el
    - **styles.scss**: a globális stíluslapunk
        - jelenleg ez a fájl üres, ide írhatjuk a globális CSS(/SCSS) szabályainkat
        - Angular-ben a komponenseknek lehet saját stíluslapjuk is, ami csak az adott komponensen fog érvényesülni, ezt is fogjuk látni később
    - **assets**: ebben a mappában tárolhatjuk a statikus tartalmainkat (pl. képek)
    - **app**: az alkalmazásunk lényegi forráskódja
        - **app-routing.module.ts**: az alkalmazás útvonalválasztási logikáját írja le (milyen URL-re milyen komponens töltődjön be), jelenleg még nem létezik, ugyanis a `--routing=false` kapcsolót használtuk a projekt létrehozásakor
        - **app.module.ts**: az alkalmazásunk modulja, ami összefogja a teljes alkalmazásban definiált elemeinket (komponensek, direktívák, szolgáltatások)
        - **app.component**
            - **.ts**: a komponensünk TypeScript forrása, egy egyszerű TypeScript osztály, ami dekorálva van az Angular `@Component()` dekorátorával, így tudatjuk az Angular-rel, hogy az osztályunk egy komponens
            - **.html**: a komponenshez tartozó HTML kód, itt tudunk adatkötni a TypeScript osztályban definiált tulajdonságokhoz
            - **.scss**: a komponenshez tartozó, csak a komponens *scope*-jára vonatkozó stíluslap

### Beadandó (0.2 pont)
!!! example "1. feladat beadandó"
    Illessz be egy képernyőképet, ahol bal oldalon a böngészőben futó Angular kezdőprojekt, jobb oldalon a VS Code-ban futó terminál látható! (`f1.png`)

## 2. feladat – Peg komponens

### Bootstrap

Elsőként adjuk hozzá a Bootstrap-et az alkalmazásunkhoz, hogy könnyen stílusozható legyen, és tudjuk hasznosítani a Bootstrap adta komponenseket. A Bootstrapet nem a szokásos módon adjuk a projektünkhöz most, ugyanis az Angular alapú működéshez specifikus integrációs csomag készült [`ng-bootstrap`](https://ng-bootstrap.github.io/#/getting-started) néven. Ez egyedileg lett implementálva a Bootstrap komponenseihez az Angular lehetőségeinek megfelelően, nincs is JQuery-től, Popper.js-től való függőségünk. Futtassuk le a `feladat` projekt mappájában (ahol a `package.json` található) az alábbi parancsot:
> `ng add @ng-bootstrap/ng-bootstrap`

Ez a parancs néhány dolgot megcsinál helyettünk, pl. letölti és behivatkozza a `bootstrap` és `ng-bootstrap` csomagokat függőségként, importálja a modult az alkalmazásunkba (az `src\app\app.module.ts`-t érdemes megvizsgálni), alkalmazza a lokalizációs polyfillt stb. Ezzel meg is volnánk, az alkalmazásunkban használhatjuk a szokásos Bootstrap elemeket, de fontos megjegyezni, hogy ez nem feltétlenül (/kizárólag) a szokásos Bootstrap osztályok elemekre aggatásával történik. A [hivatalos dokumentáció](https://ng-bootstrap.github.io/#/getting-started) az irányadó.

Abban a ritka esetben, amennyiben nem sikerülne hozzáadni a Bootstrapet, próbáljuk meg a `package.json` fájlban az összes `~X.Y.Z` stílusú verziószámot `^X.Y.Z` stílusúra lecserélni. Ezután futtassuk az `npm update` parancsot, majd próbáljuk megint hozzáadni a Bootstrapet.

### Peg komponens

Az oldalon tehát meg kell jelennie 10 sornak, benne 4 nagyobb és 4 kisebb "golyóval". Készítsük el a nagyobb golyókat és a kisebb golyókat reprezentáló `mm-peg` komponenst!

Hozzunk létre egy új komponenst az Angular CLI segítségével:
> `ng generate component peg`

A fentinek a rövidebb formája: [`ng g c peg`](https://angular.io/cli).

A parancs 3 fájlt hoz nekünk létre a `mastermind\src\app\peg` mappában: 

- a komponens stíluslapját (`.scss`) - ez jelenleg üres,
- a komponens template-jét, vagyis a DOM-szerkezetét (`.html`):

```HTML
<p>peg works!</p>
```

- a komponens mögöttes kódját, vagyis a logikáját (`.ts`):

```TS
import { Component } from '@angular/core';

@Component({
  selector: 'mm-peg',
  templateUrl: './peg.component.html',
  styleUrl: './peg.component.scss'
})
export class PegComponent {
  
}

```

Az `app.component.html` kódját cseréljük le úgy, hogy az példányosítson 4 db `PegComponent`et!

```HTML
<mm-peg></mm-peg>
<mm-peg></mm-peg>
<mm-peg></mm-peg>
<mm-peg></mm-peg>
```

Vegyük észre, hogy a kód írása közben kapunk IntelliSense-t a komponens nevéhez. Ha mégsem kapnánk IntelliSense-t, ellenőrizzük, hogy telepítettük-e az [Angular Language Service](https://marketplace.visualstudio.com/items?itemName=Angular.ng-template) bővítményt, indítsuk újra a VS Code-ot, és indítsuk újra az `ng serve`-öt!

<figure markdown>
  ![peg works!](./assets/peg-works.png)
  <figcaption>peg works!</figcaption>
</figure>

A komponensünket tehát úgy példányosítottuk, hogy a komponenshez tartozó CSS selector-nak megfelelő elemet elhelyeztük a HTML-ben. Jellemzően elemszintű selectorokat alkalmazunk, mint most a példában is, de van lehetőség más szabályszerűség (pl. class) alapján is példányosítani.

A komponensünk akár megvalósíthatja az ún. `OnInit` interfészt is, ez később az [Angular komponens/direktíva életciklus](https://angular.io/guide/lifecycle-hooks) során lehet még hasznos, de alapesetben nem generálódott bele a komponens TypeScript-kódjába.

Hozzuk létre a színeket reprezentáló típust az `src\app\models\peg-color.ts` fájlba (a mappát és fájlt is hozzuk létre). A típus egy TypeScript-uniótípus legyen a `red`, `purple`, `blue`, `green`, `yellow`, `orange`, `black`, `white` és `unset` stringértékekkel!</summary>

```TS
export type PegColor = 'red' | 'purple' | 'blue' | 'green' | 'yellow' | 'orange' | 'black' | 'white' | 'unset';
```

A Peg kétféle lehet: `code` vagy `key`, ennek is hozzunk létre egy típust az `src\app\models\peg-type.ts` fájlba `PegType` néven!

```TS
export type PegType = 'code' | 'key';
```

A `PegComponent`be vegyünk fel egy adatkötött `color` és `type` tulajdonságot (komponens paraméter)! Vegyünk fel két számított, csak lekérdezhető értéket (getter): `colorChar` (a szín első karakterét adja vissza nagybetűsítve, vagy az "X" értéket, ha nincs szín) és `colorLower` (a szín nevét adja vissza, vagy az `unset` értéket, ha nincs szín).

??? tip "Megvalósítás: `PegComponent`"
    ```TS
    export class PegComponent {
      @Input() // Az Input dekorátort importálnunk kell a jelenlegi scope-ba. Ehhez használhatjuk a VS Code segítségét (Ctrl+. a kurzort a hibára helyezve), vagy fentre beírhatjuk: import { Input } from '@angular/core';
      color?: PegColor; // Hasonlóképp a PegColor-ra is, csak itt a lokális '../models/peg-color'-ból importálunk.

      @Input()
      type?: PegType;

      get colorChar() {
        return (this.color ?? "X")[0].toUpperCase();
      }

      get colorLower() {
        return this.color ?? "unset"; 
      }
    }
    ```

A ?? operátor C#-ból ismerős lehet, TypeScriptben is használatos. A ?? a bal oldalt adja vissza, ha az nem `undefined` vagy `null`, a jobb oldalt, ha igen. Ehelyett használatos a normál `||` operátor is, viszont az minden `falsey` értékre vizsgálna.

A ? a változó neve után jelzi, hogy a az adott változó lehet `undefined`. Máskülönben a fordító hibát dobna, mert nincs inicializálva a változó. Alternatívaként használható a ! (ez kizárja az `undefined`-ot), vagy a konstruktorban is inicializálhatunk értékeket. Ezekre az intézkedésekre a [strict-mode] miatt van szükség.

Szóval a komponensünknek, ami egy golyót fog reprezentálni, lesz egy színe és egy típusa, amit ő kívülről, a szülőtől fog várni paraméterként. A szín első betűjét nagybetűsítve le tudjuk kérdezni, vagy ha nincs megadva szín, egy X-et adunk vissza helyette.

Módosítsuk a `peg.component.html` tartalmát az alábbira:

```HTML
<div class="{{colorLower}}">{{colorChar}}</div>
```

A fenti szintaxis az egyirányú adatkötést jelenti. A `colorLower` és `colorChar` a komponensük scope-jában (`this`-én) elérhető változók (tulajdonságok), amiket a felület irányába továbbítunk. Amikor ezek az értékek változnak, az Angular megfelelően újrarendereli nekünk az elemeket! Szintén kapunk IntelliSense-t itt is, ha telepítettük az Angular Language Service-t, és kapunk fordítási hibát, ha hibás TypeScript kódot írunk az adatkötések helyére.

A fentivel teljes mértékben ekvivalens az alábbi szintaxis is, amit később fogunk használni:

```HTML
<div [class]="colorLower">{{colorChar}}</div>
```

Ezzel a szintaxissal szokás attribútum értékeket kötni, ill. ugyanezzel a módszerrel adhatunk át `@Input()` paramétert egy komponensnek vagy direktívának is.

Cseréljük le a `peg.component.html` tartalmát az alábbira:

```HTML
<div class="peg peg-{{colorLower}} peg-{{type}}">{{colorChar}}</div>
```

Stílusozzuk meg az elemet! A komponenshez tartozó stíluslap csak a komponens hatáskörében fog érvényre jutni, tehát azon kívül a megfelelő selectorral rendelkező elemekre sem. Az `src\app\peg\peg.component.scss` tartalma legyen az alábbi:

??? tip "Megvalósítás: Peg SCSS"
    ```SCSS
    .peg {
        border: 1px solid grey;
        margin: 8px;
        box-shadow: 2px 2px;
    }

    .peg.peg-code {
        height: 50px;
        width: 50px;
        border-radius: 25px;
    }

    .peg.peg-key {
        height: 20px;
        width: 20px;
        border-radius: 10px;
    }

    @mixin peg-bg-color($color){
        .peg.peg-#{$color} {
            background: #{$color};
        }
    }

    @each $color in 'red', 'purple', 'blue', 'green', 'yellow', 'orange', 'black', 'white' {
        @include peg-bg-color($color);
    }

    .peg.peg-unset {
        background: #b8b8b8;
    }
    ```

Cseréljük le az `app.component.html` tartalmát (ez az oldalunk fő komponense): adjuk át adatkötéssel rendre a piros, zöld, kék és narancs színeket a `PegComponent` példányoknak!

```HTML
<mm-peg [color]="'red'"></mm-peg>
<mm-peg [color]="'green'"></mm-peg>
<mm-peg [color]="'blue'"></mm-peg>
<mm-peg [color]="'orange'"></mm-peg>
```

Ha most megnézzük az oldalt, a következőt láthatjuk:

<figure markdown>
  ![Színek az oldalon #1](./assets/colors.png)
  <figcaption>Színek az oldalon #1</figcaption>
</figure>

A DOM Explorerben (`F12` a böngészőben) láthatjuk, hogy nem jutott érvényre sem a `.peg-key`, sem a `.peg-code`, ugyanis ennek nem adtunk értéket, így egy `.peg-` osztály kerül az elemre.

Észrevehetjük még, hogy az elem egy fura attribútumot kapott (pl. `_ngcontent-dca-c97`). Ennek oka, hogy a CSS szabályunk valójában módosításra került úgy, hogy magába foglalja ezt a generált attribútumot. Ezért nem fut le tehát ez a selector más elemekre. Az `F12` CSS-eszközei között (pl. az Elements/Styles fülön) ezt láthatjuk is.

Módosítsuk az `app.component.html`-t, hogy legyen egy piros és egy zöld kódjelző, majd két fekete és két fehér kulcsjelző!

```HTML
<mm-peg [color]="'red'" [type]="'code'"></mm-peg>
<mm-peg [color]="'green'" [type]="'code'"></mm-peg>
<mm-peg [color]="'black'" [type]="'key'"></mm-peg>
<mm-peg [color]="'black'" [type]="'key'"></mm-peg>
<mm-peg [color]="'white'" [type]="'key'"></mm-peg>
<mm-peg [color]="'white'" [type]="'key'"></mm-peg>
```

<figure markdown>
  ![Színek az oldalon #2](./assets/colors-2.png)
  <figcaption>Színek az oldalon #2</figcaption>
</figure>

Alakul, most már látjuk, mit szeretnénk elérni. Néhány apróságot tegyünk rendbe a `peg.component.html` és `peg.component.scss` fájlokban:

```HTML
<div class="peg peg-{{colorLower}} peg-{{type}}"></div>
```

```SCSS
.peg {
    // ...
    box-shadow: 2px 2px;
    display: inline-block; // <<< Ezt adjuk hozzá
}

// ...
```

A `peg.component.ts`-ből törölhetjük a `get ColorChar()` függvényt, nem lesz rá szükség.


<figure markdown>
  ![Színek az oldalon #3](./assets/colors-3.png)
  <figcaption>Színek az oldalon #3</figcaption>
</figure>

### Beadandó
!!! example "2. feladat beadandó (0.2 pont)"
    Illessz be egy képernyőképet, ahol bal oldalon a színes golyók, jobb oldalon a VS Code-ban futó terminál látható! (`f2.png`)

## 3. feladat – Sor definiálása

Egy sorban meg kell jelennie 4 színes golyónak (vagy helyőrzőnek), mellette 4 jelzőnek. Tíz sornak kell összesen megjelennie.

Vegyünk fel egy osztályt, ami az egyes tippeket fogja reprezentálni az `src\app\models\guess.ts` fájlban! A modell osztály konstruktor tulajdonságokat tartalmaz: két 'PegColor' tömböt 'colors' és 'keys' néven.

??? tip "Megvalósítás: Guess"
    ```TS
    import { PegColor } from './peg-color';

    export class Guess {
        constructor(
            public colors: PegColor[],  // A tippelt színeket jelző tömb, benne pontosan 4 elemmel.
            public keys: PegColor[]) {  // A tippek helyességét jelző tömb, benne pontosan 4 elemmel.
        }
    }
    ```

Az osztályunk 4-4 színt fog tehát tárolni: ami tipp érkezett, illetve ami a visszajelzéseket mutatja majd.

Az `src\app\app.component.ts` funkcionalitását egészítsük ki! Legyen egy `guesses` tömb, ami tárolja a leadott tippeket! Legyen egy `initGame()` függvény, ami feltölti a tippeket 10 db üres tipp (`Guess`) példánnyal! A konstruktor hívja meg az `initGame()` függvényt!

??? tip "Megvalósítás: AppComponent-logika"
    ```TS
    import { Component } from '@angular/core';
    import { Guess } from './models/guess';
    import { PegColor } from './models/peg-color';

    @Component({
      selector: 'mm-root',
      templateUrl: './app.component.html',
      styleUrls: ['./app.component.scss']
    })
    export class AppComponent {
      guesses: Guess[] = [];	// ! helyett most inicializálunk

      constructor() {
        this.initGame();
      }

      initGame() {
      this.guesses = [];
        for (let i = 1; i <= 10; i++)
          this.guesses.push(new Guess(['unset', 'unset','unset','unset'], ['unset', 'unset','unset','unset']));

        console.log(this.guesses);
      }
    }
    ```

Jelenítsük meg az AppComponent template-jében (`src\app\app.component.html`) a "leadott" (jelenleg üres) tippeket! Járjuk be az összes tippet (`*ngFor`), hozzuk létre a tipp színeinek megfelelő kód és kulcs típusú `PegComponent` példányokat!

??? tip "Megvalósítás: AppComponent-sablon"
    ```HTML
    <main class="container-fluid">
        <section class="guesses-container">
            <div *ngFor="let guess of guesses" class="guess-row text-center">
                <mm-peg *ngFor="let color of guess.colors" [type]="'code'" [color]="color"></mm-peg>
                <div class="keys-container">
                    <mm-peg *ngFor="let key of guess.keys" [type]="'key'" [color]="key"></mm-peg>
                </div>
            </div>
        </section>
    </main>
    ```

A fenti megoldásban az oldal template-jében az `*ngFor` attribútum által jelzett `NgFor` direktívával bejárást végzünk a `guesses`, azon belül pedig a `colors` és `keys` tömbökön, és mindegyiknek létrehozzuk a szükséges HTML elemeket.

Stílusozzuk a komponenst (`src\app\app.component.scss`) az alábbiak szerint.

??? tip "Megvalósítás: AppComponent SCSS"
    ```SCSS
    main {
        min-width: 450px;
    }

    .guesses-container {
        background: #aaa;
    }

    .keys-container {
        width: 72px;
        display: inline-block;
    }

    .guess-row {
        border-bottom: 1px solid rgba(100, 100, 100, 0.6)
    }
    ```

<figure markdown>
  ![Leadott tippek megjelenítése](./assets/guesses.png)
  <figcaption>Leadott tippek megjelenítése</figcaption>
</figure>

### Beadandó
!!! example "3. feladat beadandó (0.2 pont)"
    Illessz be egy képernyőképet, ahol bal oldalon a tippek megjelenítése, jobb oldalon a VS Code-ban futó terminál látható! (`f3.png`)

## 4. feladat – Tipp összeállítása

A sorok fölött a jelenlegi tippünk összeállítása fog látszani. Ez alatt a 6 lehetséges szín fog megjelenni, amire kattintva össze tudjuk állítani a tippet, valamint egy gomb, amivel el tudjuk küldeni a tippünket. Ezt a funkcionalitást a következő laboron fogjuk megvalósítani, most csak a megjelenítést csináljuk meg.

Egészítsük ki az `src\app\app.component.ts` funkcionalitását és megjelenítését az elvárt funkcionalitásnak megfelelően!

??? tip "Megvalósítás: AppComponent-logika"
    ```TS
    export class AppComponent {
      guesses: Guess[] = [];
      currentGuess: PegColor[] = [];
      possibleValues: PegColor[] = ['red', 'purple', 'blue', 'green', 'yellow', 'orange'];

      constructor() {
        this.initGame();
      }

      initGame() {
        this.guesses = [];
        this.currentGuess = [];
        for (let i = 1; i <= 4; i++)
          this.currentGuess.push('unset');
        for (let i = 1; i <= 10; i++)
          this.guesses.push(new Guess(['unset', 'unset','unset','unset'], ['unset', 'unset','unset','unset']));
        console.log(this.guesses);
      }
    }
    ```

Az inicializáláskor kitöltjük a jelenlegi tippünket reprezentáló 4 színből álló tömböt (`currentGuess`), ezen felül a lehetséges értékek listáját eltároljuk a `possibleValues` tömbben.

Egészítsük ki az `src\app\app.component.html` fájlt, hogy megjelenítse az elemeket:

??? tip "Megvalósítás: AppComponent-sablon"
    ```HTML
    <main class="container-fluid">
        <section class="current-guess-container text-center mb-3">
            <div class="current-guess-row">
                <mm-peg *ngFor="let color of currentGuess" [type]="'code'" [color]="color"></mm-peg>
            </div>
            <div class="colorpicker">
                <mm-peg *ngFor="let color of possibleValues" [type]="'code'" [color]="color"></mm-peg>
            </div>
            <button class="btn btn-primary">Guess!</button>
        </section>
        <section class="guesses-container">
          <!-- ... -->
        </section>
    </main>
    ```

Igazítsuk hozzá a stílust (`src\app\app.component.scss`):

??? tip "Megvalósítás: AppComponent SCSS"
    ```SCSS
    // ...

    .current-guess-container {
        background: #f0f0f0
    }
    .current-guess-container {
        background: #f0f0f0
    }
    ```

<figure markdown>
  ![A kész kezdőoldal](./assets/main-page.png)
  <figcaption>A kész kezdőoldal</figcaption>
</figure>

 Az adatkötést egy irányban gyakoroltuk, a modell (komponens) felől a nézet (template) irányába. A következő alkalommal bekötjük az eseménykezelőket, és megírjuk a szükséges logikákat, hogy a játék játszható legyen.

### Beadandó
!!! example "4. feladat beadandó (0.2 pont)"
    Illessz be egy képernyőképet, ahol bal oldalon a kész kezdőoldal, jobb oldalon a VS Code-ban futó terminál látható! (`f4.png`)

## 5. feladat (önálló) – Neptun-kód

A `currentGuess` mezőben tároljuk a felhasználó által aktuálisan szerkesztett tipp-példányt, aminek 4 elemet kell tartalmaznia, de semmi nem akadályozza meg, hogy több vagy kevesebb elem kerüljön bele.

Alkalmazd az alábbi logikát, hogy megjelenítsd **a saját Neptun-kódodnak megfelelő**, 6-jegyű színkombinációt az első sorban (`AppComponent.currentGuess`), a színpaletta (`AppComponent.possibleValues`) felett!

A Neptun-kódod minden karakterének vedd a karakterkódjának számértékét a `string.charCodeAt()` függvénnyel! Pl: 

```JS
let x = `XYZ012`.charCodeAt(0); // == 88
```

Vedd az így keletkező karakter 6-os modulusát!

```JS
let m = x % 6; // == 4
```

Vedd a `possibleValues` ennek az indexnek megfelelő elemeit minden karakterre!

Ennek eredményeképp a példa `XYZ012` Neptun-kód esetén a `[88, 89, 90, 48, 49, 50]` értékek, ebből a `[4, 5, 0, 0, 1, 2]` maradékok tömbje, ebből pedig a `['yellow', 'orange', 'red', 'red', 'purple', 'blue']` tömb áll elő, amely a felületen az alábbi módon jelenik meg:

<figure markdown>
  ![Az 'XYZ012' Neptun kódhoz tartozó színkód vizualizációja](./assets/colorful-code.png)
  <figcaption>Az 'XYZ012' Neptun kódhoz tartozó színkód vizualizációja</figcaption>
</figure>

### Beadandó
!!! example "5. feladat beadandó (0.2 pont)"
    Illessz be egy képernyőképet, ahol bal oldalon a Neptun-kódod vizualizációja, jobb oldalon a VS Code-ban futó terminál látható! (`f5.png`)