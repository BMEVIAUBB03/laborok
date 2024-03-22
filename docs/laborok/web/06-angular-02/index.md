# Labor 06 - Angular haladó

## Bevezetés

A labor folyamán a Mastermind klasszikus táblajátékot folytatjuk ott, ahol abbahagytuk, kiegészítve a megjelenítést a logikával. Tehát ez a labor az előző folytatása, a szükséges fejlesztői eszközök megegyeznek, a korábbi végállapotot folytatjuk. Az előző laborok ismereteinek megszerzése jelen labor elvégzéséhez erősen ajánlott.

??? warning "Nagyméretű függőségek"
    Az Angular laborok során számos, viszonylag nagyméretű (többszáz megabájt) függőségi csomag letöltésére lesz szükség (az `npm install` parancs hatására), de ha valaki már sikeresen telepítette egy korábbi labor során (ha volt már) az NPM csomagokat, akkor azokat várhatóan nem kell újra letölteni.

??? warning "Házi feladat információk"
    A két Angular labor során előkerülnek az Angular alapvetői funkciói és készítünk egy egyszerűbb alkalmazást. Nem foglalkozunk viszont azzal, hogy hogyan kommunikálunk egy szerverrel (minden logika a kliensben van jelenleg), illetve [`routing`](https://angular.io/guide/router)-ot sem valósítottunk meg. A szerverrel való kommunikáció a tárgy szempontjából nem olyan fontos (házi feladatnál kiváltható adatok memóriában tárolásával, vagy akár nyers mock adatok használatával is), viszont utóbbit érdemes részletesebben megnézni! Ezek a fogalmak egy harmadik Angular labor keretein belül pár éve szóba kerültek, viszont az elmaradó laborok miatt erre idén nincs lehetőség. A régi laboranyag még [elérhető](https://github.com/bmeaut/VIAUBB03/tree/master/Web/07%20-%20Angular%203), itt is találhatunk információkat a fenti kimaradt problémákról, amik a házi feladat készítése során jól jöhetnek - de ez nem kötelező, bőven elegendő a hivatalos Angular dokumentáció használata is.

### Git repository létrehozása és letöltése

A feladatok megoldása során ne felejtsd el követni a feladat beadás folyamatát [Github](../../tudnivalok/github/GitHub.md).

1. Moodle-ben keresd meg a laborhoz tartozó meghívó URL-jét és annak segítségével hozd létre a saját repository-dat.
2. Várd meg, míg elkészül a repository, majd checkout-old ki.
    * Egyetemi laborokban, ha a checkout során nem kér a rendszer felhasználónevet és jelszót, és nem sikerül a checkout, akkor valószínűleg a gépen korábban megjegyzett felhasználónévvel próbálkozott a rendszer. Először töröld ki a mentett belépési adatokat (lásd [itt](../../tudnivalok/github/GitHub-credentials.md)), és próbáld újra.
3. Hozz létre egy új ágat `megoldas` néven, és ezen az ágon dolgozz.
4. A neptun.txt fájlba írd bele a Neptun kódodat. A fájlban semmi más ne szerepeljen, csak egyetlen sorban a Neptun kód 6 karaktere.

### Előkészítés

A kiinduló repository `feladat` mappájában adjuk ki az alábbi parancsokat a VS Code beépített termináljának segítségével (`Ctrl+ö`):

> `npm install`

> `ng serve`

*Fontos!* Ha a parancsok nem futnak le, akkor az előző Angular labor *Kiindulás* fejezetében leírtakat</a> vizsgáljuk meg!

Ha sikeresen lefutnak a parancsok, nyissuk meg a <http://localhost:4200>-as porton:

<figure markdown>
  ![Kezdőoldal](./assets/main-page.png)
  <figcaption>Kezdőoldal</figcaption>
</figure>

## 1. feladat - Tipp összeállítása

A játékos tippjének összeállításáért a fenti 4 üres kör, az alatta levő színpaletta és a tipp elküldéséhez szükséges gomb felelnek. Ezeknek az adatkötését és interakcióit fogjuk most kezelni.

Az első lépés, hogy az egyes színekre kattintva a palettában az aktuális tippünk első üres helyére kerüljön be a kattintott szín.

Ehhez az Angular beépített eseménykezelési lehetőségét fogjuk használni. Az [`@Output`](https://angular.io/guide/component-interaction#parent-listens-for-child-event) dekorátorral ellátott `EventEmitter` példányunk képes eseményeket elsütni, amire a szülő komponens feliratkozhat.

A PegComponent (`src\app\peg\peg.component.ts`) forráskódját egészítsük ki egy eseménykezelő függvénnyel (onPegClicked) és egy pegClick nevű Output tulajdonsággal!

??? tip "Megvalósítás: PegComponent"
    ```TS
    @Output() // import { Output } from '@angular/core';
    pegClick: EventEmitter<void> = new EventEmitter(); // import { EventEmitter } from '@angular/core';
    // Fontos, hogy a jó csomagból importáljuk ezt az osztályt, mert több helyen is megtalálható!

    onPegClicked() {
      this.pegClick.emit();
    }
    ```

A komponensünk tehát tud értesítést küldeni arról, ha valaki őt megnyomta. Az esemény tetszőleges paramétereket átadhat elsütéskor, nekünk erre nincs szükségünk, ezért egy `<void>` típusparaméterű `EventEmitter`t hozunk létre (ez tehát nem küld semmilyen objektumot paraméterül, amikor elsütjük). Az `onPegClicked()` függvény fogja magát a `pegClick` eseményt elsütni, ezt viszont még nem hívjuk meg sehonnan.

Módosítsuk a PegComponent kódját (`src\app\peg\peg.component.html`), hogy a div-en történő click esemény hatására lefusson az eseménykezelő!

```HTML
<div class="peg peg-{{colorLower}} peg-{{type}}" 
    (click)="onPegClicked()">
</div>
```

A fenti `(click)="eventHandler"` szintaxis ekvivelens az alábbival.

```HTML 
on-click="onPegClicked()"
```

Ezt ritkábban használják, de megfelel a HTML szabványnak, ami szerint az attribútumok csak alfanumerikus értékeket tartalmazhatnak (szemben a `()[]` zárójel karakterekkel). A `[property]`-nek a megfelelője ugyanígy a `bind-property`.

A normál DOM elemeken elérhető nagyon sok beépített DOM esemény, mint pl. a `click`, ezeket az Angular alapételmezetten rendelkezésünkre bocsátja. A komponenseken (pl. az `mm-peg` elemen) ilyen nem érhető el, ugyanis ezek nem DOM elemekként, hanem komponensekként kezelendők. A komponensek viszont `@Output` dekorátorral ellátott eseménykezelő változókkal rendelkezhetnek, amelyek ezzel megegyező funkciót biztosítanak, ahogyan az alábbiakban is látható lesz.

Módosítsuk az AppComponent (`src\app\app.component.ts` és `src\app\app.component.html`) kódját, hogy kezelje a PegComponenten elérhető új eseményünket a saját addColorToCurrentGuess függvényével!

??? tip "Megvalósítás: AppComponent (TS és HTML)"
    ```TS
    addColorToCurrentGuess(color: PegColor) {
      console.log(color);
    }
    ```

    ```HTML
    <div class="colorpicker">
        <mm-peg *ngFor="let color of possibleValues" [type]="'code'" [color]="color"
                (pegClick)="addColorToCurrentGuess(color)"></mm-peg>
    </div>
    ```

<figure markdown>
  ![Peg click esemény](./assets/peg-click.png)
  <figcaption>Peg click esemény</figcaption>
</figure>

Foglaljuk össze, hogy mi történt eddig!

- A `PegComponent` osztályunk feliratkozott a `<div>` elem `click` eseményére, aminek hatására meghívja a saját `onPegClicked()` függvényét.
- A `PegComponent` deklarált egy eseményt az `@Output` dekorátorral `pegClick` néven, ezáltal az `AppComponent` fel tudott iratkozni az eseményre a `(pegClick)="eventHandler"` szintaxissal.
- A `PegComponent` az `onPegClicked()` függvényben elsüti a `pegClick` eseményt, ami tehát a `<div>`-re történő `click` eseményre van beregisztrálva.
- Az `AppComponent` minden egyes `<mm-peg>` elemre fel tudott iratkozni az `NgFor` direktívában, és így mindig a kattintott elem színét adja át az `addColorToCurrentGuess(color: PegColor)` függvénynek, ami jelenleg csak kiírja a kapott paramétert.

Nem minden `<mm-peg>` elemre fogunk feliratkoztatni eseménykezelőt, most is csak a színpalettán levőkre tettük. 

Az `EventEmitter` ad lehetőséget nekünk arra, hogy lekérdezzük, hány feliratkozó van az elemre. A DOM-ban elérhető `style` tulajdonság bármely értékére tudunk kötni a speciális [`NgStyle`](https://angular.io/api/common/NgStyle) direktívával.

Oldjuk meg, hogy azok az &lt;mm-peg&gt; elemek (`src\app\peg\peg.component.html`), amikre van beregisztrálva legalább egy eseménykezelő, pointer típusú egérkurzort kapjanak!

```HTML
<div class="peg peg-{{colorLower}} peg-{{type}}" 
    (click)="onPegClicked()"
    [style.cursor]="pegClick.observers.length > 0 ? 'pointer' : 'initial'">
</div>
```

Ezután csak azoknak az `<mm-peg>` komponenseknek a `<div>`-jeire fog felkerülni a "mutatós" egérkurzor, amikre van feliratkoztatva eseménykezelő, esetünkben tehát az egyes tippelhető színekre. A dinamikus adatkötés legfontosabb elemét használjuk tehát ki: a dinamizmust.

Már csak az "üzleti logika" megírása van hátra, tehát a megfelelő színt be kell tenni az első üres helyre.

Valósítsuk meg helyesen az addColorToCurrentGuess() függvényt (`src\app\app.component`)!

??? tip "Megvalósítás: addColorToCurrentGuess"
    ```TS
    addColorToCurrentGuess(color: PegColor) {
      for (let i = 0; i < 4; i++) {
        if (this.currentGuess[i] == 'unset'){
          this.currentGuess[i] = color;
          return;
        }
      }
    }
    ```

Fontos a `return`, ugyanis ha nem lépünk ki, miután találtunk egy üres helyet, az összes üres helyre be fog kerülni a tippünk.

A fentitől kicsit "egyszerűbben" (rövidebben, de funkcionális megközelítést alkalmazva) is megírhatjuk a keresést és beszúrást a `splice` univerzális tömbmanipuláló függvénnyel:

```TS
addColorToCurrentGuess(color: PegColor) {
  this.currentGuess.splice(this.currentGuess.indexOf('unset'), 1, color);
}
```

Ez annyiban működik másképpen, hogy ha az `indexOf('unset')` `-1`-gyel tér vissza, akkor a tömb végéről vesszük ki az utolsó elemet és cseréljük le. Ez nekünk így pont meg is felel.

<figure markdown>
  ![Tippelés folyamata](./assets/guessing.gif)
  <figcaption>Tippelés folyamata</figcaption>
</figure>

### Tipp javítása

Ezek után az aktuális tippünk javítására azáltal van lehetőségünk, hogy valamelyik nem üres színű golyóra kattintunk. Ekkor, ha ezután a klikkelt golyótól jobbra még találhatók nemüres színű golyók, akkor azokat balra csúsztatjuk eggyel.

A legegyszerűbb megoldás az `NgFor` direktívában elkérni az aktuális elemünk indexét. Erre azért van szükség, mert több ugyanolyan színű golyónk is lehet, és ha csak a szín alapján próbálnánk meg kivenni az aktuális tippből az elemet, akkor rossz elemet is kivehetnénk (vagyis nem eldönthető, melyiket akartuk kivenni a 2 piros közül).

Implementáljuk értelemszerűen a removeColorFromCurrentGuess függvényt (`src\app\app.component.ts`)!

```TS
removeColorFromCurrentGuess(index: number) {
  this.currentGuess.splice(index, 1);
  this.currentGuess.push('unset');
}
```

Ez a függvény kiveszi az aktuális tippünkből az adott indexű elemet, a tömb végére pedig beszúr egy `unset` elemet. Így minden elem balra csúszik eggyel, de mivel 4 helyett csak 3 elem marad, ezért egy új üres elemet kell beszúrni a tömb végére.

A függvényt a megfelelő index-szel kell meghívnunk, ehhez használjuk az Angular [NgFor](https://angular.io/api/common/NgForOf) index szintaxisát!

Adjuk át az aktuális elem indexét az eseménykezelőnek az AppComponent (`src\app\app.component.html`) .current-guess-row elemében!

```HTML
<div class="current-guess-row">
    <mm-peg *ngFor="let color of currentGuess; index as i" [type]="'code'" [color]="color" 
        (pegClick)="removeColorFromCurrentGuess(i)"></mm-peg>
</div>
```

Láthatjuk, hogy az `NgFor` segítségével kivettük az aktuális elem indexét az `i` változóba, amit átadunk a `pegClick` eseménykezelőben a függvényünknek.

<figure markdown>
  ![Tipp eltávolítása](./assets/guess-remove.gif)
  <figcaption>Tipp eltávolítása</figcaption>
</figure>

### Beadandó (0.25 pont)
!!! example "1. feladat beadandó"
    Illessz be egy képernyőképet, ahol bal oldalon egy kész tippelés, jobb oldalon a VS Code terminálja látható! (`f1.png`)

## 2. feladat - Tipp elküldése

A `Guess!` feliratú gombot csak akkor engedélyezzük, ha már 4 színes golyó adja az aktuális tippünket!

A problémát többféleképpen megközelíthetjük, a legcélravezetőbb szintén adatkötést használni. Az `[attribútum]="érték"` szintaxissal adott DOM elem tulajdonságát adatköthetjük. Mi a gombunk `disabled` attribútumát szeretnék akkor és csak akkor engedélyezni, ha nincsen az aktuális tippben `unset` érték. Megtehetnénk, hogy minden alkalommal, amikor módosítjuk a tömböt (elemet veszünk fel vagy törlünk), újra kiszámoljuk, hogy van-e ilyen érték. Ez viszont törékeny, ugyanis minden helyen, ahol a tömböt manipuláljuk, meg kell hívnunk ezt a logikát is. Célszerűbb ahhoz a logikai értékhez kötni, ami el tudja dönteni, hogy van-e üres elem a tömbben.

Módosítsuk a gombhoz tartozó HTML részletet az `app\src\app.component.html`-ben, egyúttal vegyük fel a majdani tippelési logikát tartalmazó eseménykezelőt guess néven az `app\src\app.component.ts`-be!

??? tip "Megvalósítás: AppComponent (TS és HTML)"
    ```TS
    guess() {
      console.log("Guess works!");
    }
    ```

    `src\app\app.component.html`:

    ```HTML
    <button class="btn btn-primary" 
        [disabled]="currentGuess.indexOf('unset') !== -1"
        (click)="guess()">
        Guess!
    </button>
    ```

Láthatjuk, hogy a `disabled` attribútum értéke azzal egyezik meg (*minden adott időpillanatban!*), hogy a `currentGuess` tömb tartalmaz-e `unset` értéket. Nem kellett tehát felvennünk egy külön erre a célra szolgáló tulajdonságot/mezőt/változót, amit nekünk kell kézzel karbantartani, egyszerűen csak megadtuk az adatkötött kritériumot, hogy mikor legyen letiltva a gomb.

<figure markdown>
  ![Guess gomb](./assets/guess-button.gif)
  <figcaption>Guess gomb</figcaption>
</figure>

Láthatjuk, hogy a gomb `hover`-re továbbra is a `pointer`-t mutatja. Használhatnánk ismét a `[style]` adatkötést, de fontos, hogy csak azért, mert van egy kalapács a kezünkben, nem szabad mindent szögnek nézni. Ezért egyszerűsítsük le a dolgunkat.

Az AppComponent stíluslapjához (`src\app\app.component.scss`) vegyünk fel egy újabb CSS szabályt, ami a letiltott gombokat megfelelő kurzorral látja el!

```SCSS
button:disabled, .btn:disabled {
  cursor: not-allowed !important;
  pointer-events: all !important;
}
```

Láthatjuk, hogy itt használjuk az `!important` kulcsszót. Ez azért van, mert szeretnénk, ha minden esetben, ha az elemen definiált `style` esetleg másképp definiálná is, az összes HTML5 `<button>` és Bootstrap `.btn` gomb letiltott állapotban letiltott kurzorral rendelkezzen. A `pointer-events` felüldefiniálására azért van szükség, mert a Bootstrap (`_buttons.scss`) `none`-ra állítja az értékét, amitől a `cursor` beállításunknak nem lesz hatása.

A tippünk összeállítását követően el is küldhetjük azt.

Ehhez kezelnünk kell a játék indulásakor, hogy sorsoljon ki nekünk a gép 4 véletlenszerű színt.

Egészítsük ki az AppComponent kódját (`src\app\app.component.ts`) egy új tulajdonsággal, ami a kisorsolt színeket fogja tartalmazni, valamint az initGame() függvényét, ami véletlenszerűen fog sorsolni a lehetséges értékek közül 4-et!

??? tip "Megvalósítás: AppComponent"
    ```TS
    private secretColors: PegColor[] = [];

    initGame() {
      // ...
      this.secretColors = Array.from(Array(4)).map(_ => this.possibleValues[Math.floor(Math.random() * this.possibleValues.length)]);
    }
    ```

A fenti feltöltési logika a `secretColors` tömböt tölti fel:

- veszünk egy 4 üres elemet tartalmazó tömböt,
- minden elemet transzformálunk egy véletlen értékre az alábbi módon:
    - az értéktől függetlenül (ez a `_` nevű paraméter, így szokás jelölni a nem használt paramétereket) a `possibleValues` tömbből kiválasztunk egy véletlen elemet,
    - a `Math.random()` egy 0 és 1 közötti értéket ad nekünk vissza, ezt extrapoláljuk 0 és 5 közé, majd a `Math.floor()` segítségével vesszük a véletlen szám egész részét,
    - a kapott véletlen számmal kiindexeljük az egyik színt a `possibleValues` tömbből.

Fontos még, hogy a kliensen tárolt bárminemű állapot (esetünkben a `secretColors` változó) nem tekinthető a végfelhasználó elől valóban *titkosnak*, ugyanis a teljes memóriaterületért az ő számítógépe felel. Ez ellen sem a TypeScript fordítás, sem a `private` kulcsszó nem véd, nagyon egyszerűen ki lehet deríteni a változó értékét.

A tipp beküldéséhez kezeljük, amikor a felhasználó megnyomja a tippelésre szánt gombot. Ennek az eseménykezelőjében ki kell számítanunk, hogy melyik tippek voltak jó helyen, és melyikek rossz helyen. Itt figyelnünk kell, hogy azokat az elemeket, amiket már egyszer bármelyik módon lekezeltük, ne kezeljük a másik módon is. Ha megvannak az értékek, akkor:

- fel kell vennünk az aktuális tippet a leadott tippek közé,
- meg kell jelenítenünk a tipp helyességét a `key` típusú `peg`-ek által,
- üríteni kell az aktuális tippet.

A tipp beküldésekor vizsgáljuk, hogy eltalálta-e mind a 4 színt a játékos, mert ekkor nyer, illetve hogy ez volt-e az utolsó tippje, amivel még mindig nem találta el a színeket, mert ekkor veszít. Egyelőre egy `alert()` ablakot feldobhatunk erre az esetre. Ezt követően egyszerűen meghívjuk az `initGame()` függvényt, amivel új játékot indítunk.

Valósítsd meg a teljes tippelési logikát a guess() függvényben (`src\app\app.component.ts`) a leírtaknak megfelelően (nehéz)!

??? tip "Megvalósítás: AppComponent"
    ```TS
    guess() {
      // Készítünk egy másolatot a tippből és a sorsolt titkos sorrendből
      const current = this.currentGuess.slice();
      const secret = this.secretColors.slice();

      let matches = 0; // Itt gyűjtjük, hány talált.
      for (let i = 0; i < current.length; i++) {
        if (current[i] == secret[i]) { // Ha a két elem index szerint is megegyezik...
          matches++; // ... találtunk egyet ...
          current.splice(i, 1); // ... kivesszük mindkét tömbből ...
          secret.splice(i, 1);
          i--; // .. és visszaléptetjük az iterátorváltozót, mert változott a tömb mérete.
        }
      }
      let wrongPlace = 0; // Itt gyűjtjük, hány helyes, de rossz helyen van.
      for (let i = 0; i < current.length; i++) {
        const secretIndex = secret.indexOf(current[i]);
        if (secretIndex !== -1) { // Ha megtaláltuk az elemet a maradékban bárhol ...
          wrongPlace++; // ... találtunk egyet ...
          current.splice(i, 1); // ... kivesszük mindkettőből az első találatot...
          secret.splice(secretIndex, 1);
          i--; // .. és visszaléptetjük az iterátorváltozót, mert változott a tömb mérete.
        }
      }

      const currentInList = this.guesses.find(g => g.colors.indexOf('unset') !== -1); // Megkeressük az első elemet a listában, ahol van üres elem.
      currentInList!.colors = this.currentGuess; // A tipp színeit átadjuk a listaelemnek.
      currentInList!.keys = Array.from(Array(4).keys())
        .map<PegColor>(i => i < matches ? 'black' : i < matches + wrongPlace ? 'white' : 'unset') // Létrehozunk annyi fekete key-t, ahány talált, annyi fehéret, amennyi nem, a többi pedig üres.
      this.currentGuess = Array.from(Array(4)).map(_ => 'unset'); // Az új tippünk pedig legyen simán csak üres.  

      if (matches === 4) { // Ha mind talált, nyertünk.
        alert("You won!");
        this.initGame();
      }
      else if (!this.guesses.some(g => g.colors.some(c => c === 'unset'))) { // Ha minden tippet leadtunk már, vesztettünk.
        alert("You lost...");
        this.initGame();
      }
    }
    ```

Az adatkötés segítségével a felületre automatikusan bekerül a megfelelő tipp, ugyanis az adatkötés automatikusan újrarajzolja az elemeket a felületen.

### Beadandó (0.25 pont)
!!! example "2. feladat beadandó"
    Illessz be egy képernyőképet, ahol bal oldalon a játék vége (alert-ben "You won!" vagy "You lost..."), jobb oldalon a VS Code terminálja látható! (`f2.png`)

## 3. feladat - Játék végét jelző modális ablak

A játéknak csak akkor lehet vége, amikor a játékos tippelt, és vagy eltalálta a helyes sorrendet, vagy elértük a 10 tippet és az utolsó tipp sem volt helyes. Ekkor jelenítsünk meg egy új modális ablakot, amibe tegyük ki, hogy mi volt az eredetileg sorsolt sorrend, mi volt a játékos utolsó tippje, hány tippet adott le, valamint egy gombot, amivel új játék indítása lehetséges. Jelenleg csak egy csúnya `alert` ablakot dobunk fel, viszont sokkal szebb megoldás volna, ha az [NgBootstrap modális ablakát](https://ng-bootstrap.github.io/#/components/modal/examples) jelenítenénk meg az alábbiakkal:

- jelezzük, hogy nyert vagy veszített a játékos,
- a leadott tippek száma,
- az utoljára leadott tipp,
- az eredetileg sorsolt sorrend,
- egy gomb, amivel új játék indítható.

A modális ablak kezeléséhez a komponensünknek (`src\app\app-component.ts`) konstruktorban kell várnia az `NgbModal` objektumot az `@ng-bootstrap\ng-bootstrap` modulból:

```TS
constructor(private modalService: NgbModal) { // import { NgbModal } from '@ng-bootstrap\ng-bootstrap';
```

Ez az ún. [Dependency Injection](https://angular.io/guide/dependency-injection) (vagy *függőséginjektálás*). Amikor az alkalmazásunk moduljába (`app.module.ts`) beregisztráltuk függőségként az `NgbModule`-t, felruháztuk az Angulart annak lehetőségével, hogy az abban a modulban definiált elemeket ismerhessük, el tudjuk kérni pl. komponenseink (szolgáltatásaink stb.) konstruktorában. A komponenseket ugyanis az Angular példányosítja nekünk (sosem hívtunk `new AppComponent()`-et a saját kódunkban), és figyelembe veszi, hogy milyen függőségeket kell átadnia az egyes objektumok létrehozásához.

Ezután a komponens kódjában a `modalService` tulajdonság segítségével megjeleníthetünk egy modális ablakot. Erre több lehetőségünk is van:

- átadható az Angular komponens típusa (egyszerű függvényparaméterül) az `open` függvénynek, de ekkor magunknak kell gondoskodni a komponens paraméterezéséről,
- átadható egy [TemplateRef](https://angular.io/api/core/TemplateRef), azaz egy komponens template-jének része (beágyazott nézet).

Készítsük el és jelenítsük meg a modális ablakot (nehéz)!

A fent felsorolt két lehetőség közül az elsőt valósítjuk meg. Ehhez létrehozunk egy önálló komponenst, ami ki fogja rajzolni a végeredményt.

> `ng generate component game-over`

??? tip "Megvalósítás: GameOverComponent"
    ```TS
    export class GameOverComponent {

      @Input()
      won!: boolean;

      @Input()
      numberOfGuesses!: number;

      @Input()
      lastGuess!: PegColor[];

      @Input()
      secretColors!: PegColor[];

      @Output()
      restart = new EventEmitter<void>();

      constructor() { }

      initParameters(inputs: { won: boolean, numberOfGuesses: number, lastGuess: PegColor[], secretColors: PegColor[] }, outputs: { restart: (...args: any[]) => any }) {
        this.won = inputs["won"];
      this.numberOfGuesses = inputs["numberOfGuesses"];
      this.lastGuess = inputs["lastGuess"];
      this.secretColors = inputs["secretColors"];

      this.restart.subscribe(outputs["restart"]);

      //for (let prop in inputs)
        //this[prop] = inputs[prop];

      //for (let prop in outputs)
        //(this[prop] as EventEmitter<any>).subscribe(outputs[prop]);
      }
    }
    ```

Értelemszerűen importáljuk a fenti kódrészlethez a szükséges szimbólumokat az `@angular/core` és `../models/peg-color` modulokból!

A komponensünkben implementáltunk egy függvényt, amivel beállítjuk az egyébként komponensparaméterként érkező változókat. Mivel most kódból példányosodik a komponensünk, ezért kézzel kell ezeket beállítanunk (a template-ezett `[adatkötés]` helyett). Az `initParameters` függvény két paramétert vár:

- az első (`inputs`) paraméterben ugyanolyan névvel és típussal szerepelnek az elemünk `@Input` paraméterei,
- a második (`outputs`) paraméterben ugyanígy a kimenő paraméterek feliratkoztatandó függvényei, de mivel ezeknek változó típusai lehetnek, ezért akárhány bemenő paramétert váró és akármivel visszatérő függvényt elfogadunk itt.

Első lépésben bejárjuk az inputs tömb kulcsait (tehát `won`, `numberOfGuesses`, `lastGuess`, `secretColors`), és a `this`-re ráindexeljük az objektum adott nevű tulajdonságait. Emlékezzünk, ezt azért tehetjük meg, mert JavaScriptben minden objektum egyben egy asszociatív tömb is.

A második lépés ezzel majdnem megegyezik, itt viszont nem adhatjuk a paraméterül kapott függvényt az `EventEmitter` értékének. Itt kiindexeljük a megfelelő nevű tulajdonságokat (most csak a `restart` nevűt), és ezt `EventEmitter<any>`-vé típusasszertálva meghívjuk a feliratkoztatás függvényét és átadjuk a feliratkoztatandó függvényt (a `restart()`-ot).

???+ note "Megjegyzések a fenti megoldáshoz"
    A fenti konstrukció a konkrét típusok ismeretének hiányában alkalmazható például (*reflection*-höz hasonló elvet követve). Önállóan ilyen kódot ritkán szükséges írni, a legtöbb fejlesztőnek nincsen szüksége a keretrendszer-jellegű funkciók megírására, csak felhasználására.

    Az Angular 12-ben bevezetett [strict mode] miatt a kikommentezett for ciklusok már nem működnek (így a fenti magyarázat sem aktuális), ezért egy butább megoldást alkalmazunk. Ugyanezért kellett az inicializálatlan változóinkat ?-el vagy !-el deklarálni, vagy pedig kezdőértéket adni nekik. Megjegyzés: ha muszáj, a strict mode kikapcsolható a `tsconfig.json` fájlban néhány beállítás módosításával, de ez nem ajánlott.

Írjuk meg a GameOverComponent sablon kódját is (`src\app\game-over\game-over.component.html`)!

??? tip "Megvalósítás: GameOverComponent"
    ```HTML
    <div class="text-center" [class.won]="won">
        <h1 *ngIf="won">Congratulations! You won!</h1>
        <h2 *ngIf="!won">You lost. Better luck next time!</h2>

        <h3>You took <span class="guesses">{{numberOfGuesses}}</span> guesses.</h3>

        <h3>Your last guess was:</h3>
        <mm-peg *ngFor="let color of lastGuess" [type]="'code'" [color]="color"></mm-peg>

        <h3>The secret was:</h3>
        <mm-peg *ngFor="let color of secretColors" [type]="'code'" [color]="color"></mm-peg>

        <button class="btn btn-success btn-block" style="width: 100%" (click)="restart.emit()">Start new game</button>
    </div>
    ```

Itt két újdonságot láthatunk:

- a `[class.won]` ekvivalensen működik, mint a [style.cursor], csak itt nem stílus értékét adjuk meg, hanem azt, hogy az adott nevű osztály rákerüljön-e az elemre vagy sem,
- az [`NgIf`](https://angular.io/api/common/NgIf) strukturális direktíva csak akkor helyezi a DOM-ba az adott elemet, ha az átadott feltétel igaz.

Adjunk egy alap stíluzosát is a komponenesnek (`src\app\game-over\game-over.component.scss`)!

```SCSS
.guesses {
    color: red;
}
.won .guesses {
    color: green;
}
```

Ebben a CSS-ben kihasználjuk a CSS szabályok specificitását. A `.guesses` szövege piros, de a `.won .guesses` értéke zöld, és mivel az utóbbi specifikusabb, ha a játékos nyer, ez jut érvényre, egyébként a piros.

A modális ablak feldobásakor igazából egyetlen paramétert kell átadnunk egy közösen kezelhető függvénynek, tegyük is ezt meg (`src\app\app.component.ts`):

??? tip "Megvalósítás: AppComponent"
    ```TS
    openGameOverModal(won: boolean) {
      let modal = this.modalService.open(GameOverComponent, { backdrop: 'static', centered: true });
      (modal.componentInstance as GameOverComponent) // import { GameOverComponent } from './game-over/game-over.component';
        .initParameters({ // így adjuk át az @Input paramétereket
          won,
          numberOfGuesses: this.guesses.filter(g => !g.colors.includes('unset')).length,
          lastGuess: this.guesses.filter(g => !g.colors.includes('unset')).reverse()[0].colors,
          secretColors: this.secretColors.slice()
        }, {
          restart: () => {
            modal.close();
            this.initGame();
          }
        });
    }
    ```

A korábbiak ismeretében a fenti kódrészlet már viszonylag egyértelmű. Az `NgbModal` `open` függvényének első paraméterül egy komponenstípust adunk át (konkrétan a komponens konstruktorfüggvényét, ebben az esetben, ami megegyezik a típus nevével), második paraméternek pedig átadjuk, hogy legyen nem "kiklikkelhető" a modális ablak, valamint legyen függőlegesen is középre igazítva. A függvényhívás eredménye egy `NgbModalRef`, amin keresztül bezárhatjuk a modális ablakot (ezt a `restart`-ban meg is tesszük), illetve ezen keresztül elérjük a példányosított komponensünket. A komponensen meghívjuk a szándékosan erre a célra írt inicializáló függvényünket, amiben értelemszerűen átadjuk a megfelelő input értékeket, valamint a `restart` függvényt, amit akkor hívunk meg, amikor a játékos a *Start new game* feliratú gombra kattint.

Még az `AppComponent` `guess()` függvényében (`src\app\app.component.ts`) fel kell dobnunk a modális ablakot (az `alert()`ek helyett).

??? tip "Megvalósítás: AppComponent"
    ```TS
    guess() {
      // ...
      if (matches === 4) // Ha mind talált, nyertünk.
        this.openGameOverModal(true);
      else if (!this.guesses.some(g => g.colors.some(c => c === 'unset'))) // Ha minden tippet leadtunk már, vesztettünk.
        this.openGameOverModal(false);
    }
    ```

Ezzel elkészültünk az alkalmazás önállóan játszható verziójával.

<figure markdown>
  ![A játék végállapota](./assets/end-result.gif)
  <figcaption>A játék végállapota</figcaption>
</figure>

### Beadandó (0.5 pont)
!!! example "3. feladat beadandó"
    Illessz be egy képernyőképet, ahol bal oldalon a felugró modális ablak, jobb oldalon a VS Code-ban futó terminál látható! (`f3.png`)