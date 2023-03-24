# Labor 04 - TypeScript, AJAX

## Bevezetés

A labor folyamán a hallgatók a laborvezető segítségével és önállóan végeznek feladatokat a webes technológiák gyakorlati megismerése érdekében.

Felhasznált technológiák és eszközök:

- webböngészők beépített hibakereső eszközei,

- npm, a [NodeJS](https://nodejs.org/en/download/) csomagkezelője,

- [Visual Studio Code](https://code.visualstudio.com/download) kódszerkesztő alkalmazás,
  - otthoni vagy egyéni munkavégzéshez használható bármilyen más kódszerkesztő vagy fejlesztőkörnyezet, de a környezet kapcsán felmerülő eltérésekről önállóan kell gondoskodni.

??? note "A TypeScriptről dióhéjban"
    A TypeScript opcionális, statikus típusrendszerrel felturbózott JavaScript, tehát elsősorban szintaktikai édesítőszereket ad a JavaScripten felül, amik könnyebbé teszik a JavaScript alkalmazások fejlesztését.

    A TypeScript kulcsfontosságú eleme a `tsc`, azaz a TypeScript compiler, aminek segítségével a TypeScript forráskódunkat fordíthatjuk le JavaScript forráskóddá (tehát ez egy *source-to-source compiler*, vagy más néven *transpiler*).

    **A TypeScript a JavaScriptre épít, így minden JavaScript fájl egyben értelmezhető TypeScript kód is.** A VS Code alapértelmezetten IntelliSense segítséget ad nekünk a JavaScript forrásokban is, ezt a TypeScript compiler segítségével teszi a háttérben.

### Git repository létrehozása és letöltése

A feladatok megoldása során ne felejtsd el követni a feladat beadás folyamatát [Github](../../tudnivalok/github/GitHub.md).

1. Moodle-ben keresd meg a laborhoz tartozó meghívó URL-jét és annak segítségével hozd létre a saját repository-dat.
2. Várd meg, míg elkészül a repository, majd checkout-old ki.
    * Egyetemi laborokban, ha a checkout során nem kér a rendszer felhasználónevet és jelszót, és nem sikerül a checkout, akkor valószínűleg a gépen korábban megjegyzett felhasználónévvel próbálkozott a rendszer. Először töröld ki a mentett belépési adatokat (lásd [itt](../../tudnivalok/github/GitHub-credentials.md)), és próbáld újra.
3. Hozz létre egy új ágat `megoldas` néven, és ezen az ágon dolgozz.
4. A neptun.txt fájlba írd bele a Neptun kódodat. A fájlban semmi más ne szerepeljen, csak egyetlen sorban a Neptun kód 6 karaktere.

## 1. feladat - BlackJack kiinduló projekt megismerése

Vizsgáljuk meg a kiinduló projektet! A kiinduló index.html a `<head>` részben hivatkozza a függőségeket, és később a HTML állomány végén a saját `blackjack.js` fájlunkat. Vegyük észre, hogy ilyen fájl nem található, mi magunk kézzel nem is fogjuk létrehozni; ezt a TypeScript compiler fogja nekünk generálni a `blackjack.ts` fájlból indulva. Vizsgáljuk meg a `tsconfig.json` nevű fájlt, ami a TypeScript compiler konfigurációját tartalmazza!

Terminálból (Ctrl+ö) adjuk ki az alábbi parancsot a munkamappában:
> `npm install`

Ezzel a paranccsal a package.json fájlban található függőségeket telepítjük a Node.js Package Manager (`npm`) segítségével. Itt a Bootstrap, a JQuery és Popper osztálykönyvtárak találhatók (utóbbi kettő a Bootstrap függősége).

Indítsuk el Terminálból (célszerű Ctrl+Shift+ö billentyűkombinációval egy új terminált indítani a szokásos `live-server` parancsnak) a TypeScript compilert `watch` módban, ami figyelni fogja az összes hivatkozott TypeScript fájl módosítását, és újragenerálja szükség esetén a kimenetet:
> `node_modules\.bin\tsc -w`

??? warning "MAC"
    Ha MAC-en lennénk és a fenti nem működik, a következő paranccsal próbálkozhatunk:
    > `tsc --watch`

Indítást követően a következő képernyő fogad minket:

<figure markdown>
  ![Kiinduló állapot](./assets/blackjack-start.png)
  <figcaption>BlackJack kezdőképernyő</figcaption>
</figure>

A Start gombra kattintva nem történik egyelőre semmi, később fogunk funkcionalitást rendelni hozzá.

Vizsgáljuk meg a `game.ts` fájl tartalmát!

``` TS
class Game {
    private constructor(public readonly deckId: string) {
    }

    static readonly apiBaseUrl = "https://deckofcardsapi.com/api/";

    static async NewGame(deckCount: number = 6) {
        var response = await this.NewDeck(deckCount);
        return new Game(response.deck_id);
    }

    static async NewDeck(deckCount: number) {
        return await this.Call<NewDeckResponse>(`deck/new/shuffle/?deck_count=${deckCount}`);
    }

    async Draw(count: number = 1) {
        return await Game.Call<DrawCardResponse>(`deck/${this.deckId}/draw/?count=${count}`);
    }

    private static async Call<ResponseType extends DeckOfCardsResponse>(relativeUrl: string) {
        const response = await fetch(`${this.apiBaseUrl}${relativeUrl}`);
        const responseJson: ResponseType = await response.json();
        if (!responseJson.success) {
            console.error("Error from Deck Of Cards API:", responseJson);
        }
        return responseJson;
    }

    dealersHand: Hand = new Hand([], false);
    playersHand: Hand = new Hand([], true);
}
```

- Ez az osztály a <a href="https://deckofcardsapi.com/" target="_blank">Deck of Cards API</a> klienseként szolgál.
- Konstruktora privát, példányosítani egy `Game` objektumot a statikus, aszinkron `NewGame` függvény meghívásával lehetséges.
- A konstruktorban jelzett `public` módosítószóval az objektumon automatikusan létrejön a konstruktorparaméter nevével és értékével egy egyszer beállítható, csak olvasható `deck_id` mező.
- Minden távoli HTTP hívás a `Call` függvényen megy keresztül, ahol egy relatív útvonalat kell megadnunk, ami a `https://deckofcardsapi.com/api/`-hoz képesti relatív útvonalra indít egy aszinkron AJAX kérést a <a href="https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API/Using_Fetch" target="_blank">fetch</a> API használatával.
- Érdemes észrevenni a C#-ból már ismerős `async/await` minta használatát. A Call hívás ekvivalens az alábbi JavaScript kóddal:

``` JS
function Call(relativeUrl) {
  return new Promise((resolve, reject) => {
    fetch(`${this.apiBaseUrl}${relativeUrl}`).then(response => {
      response.json().then(responseJson => resolve(responseJson), reject)
    }, reject);
  });
}
```

Látható, hogy a kód ebben a formájában jóval kevésbé volna olvasható. A háttérben egy (esetenként komplex) állapotgépet készít a fordító, aminek segítségével ekvivalens működést tudunk elérni, csak nem egymásba ágyazott függvények, hanem `async/await` szintaxis segítségével.

Próbáljuk ki az alábbi hívást a böngésző URL-jébe írva:
> `https://deckofcardsapi.com/api/deck/new/shuffle/?deck_count=6`

Az így kapott válaszból helyettesítsük be a `deck_id` értékét az alábbi hívásba, és azt is próbáljuk ki:
> `https://deckofcardsapi.com/api/deck/<<deck_id>>/draw/?count=2`

Láthatjuk, hogy az így kapott válasz az alábbihoz hasonló lesz:
``` JSON

{
	"success": true,
	"deck_id": "g7ldwoe33ss4", 
    "cards": 
    [
        {
			"code": "JC",
			"image": "https://deckofcardsapi.com/static/img/JC.png", 
            "images": 
            {
                "svg": "https://deckofcardsapi.com/static/img/JC.svg",
                "png": "https://deckofcardsapi.com/static/img/JC.png"
            }, 
            "value": "JACK", 
            "suit": "CLUBS"
        }, 
        {
			"code": "AD",
			"image": "https://deckofcardsapi.com/static/img/aceDiamonds.png", 
            "images": 
            {
                "svg": "https://deckofcardsapi.com/static/img/aceDiamonds.svg",
                "png": "https://deckofcardsapi.com/static/img/aceDiamonds.png"
            }, 
            "value": "ACE", 
            "suit": "DIAMONDS"
        }
    ], 
    "remaining": 310
}

```


A fenti válasz típus leírója interfész formájában szerepel a `draw-card-response.ts` fájlban:

``` TS

interface DrawCardResponse extends DeckOfCardsResponse {
    cards: Card[];
}

```

???+ tip "TypeScript interfészek és típusosság"
    A TypeScript - a szokásos objektum-orientált nyelvekkel ellentében - úgynevezett `structural subtyping`-ot vagy `duck typing`-ot használ. Ez azt jelenti, hogy TypeScript-ben az interfészek nem csak metódusok megvalósítását írhatják elő, hanem egy objektum struktúrájára is tehetnek megkötéseket. Egy objektumnak nem kell explicit implementálnia az adott interfészt, elég, hogyha megnézzük, hogy úgy "viselkedik-e", mint az adott interfész. Innen ered a `duck typing` kifejezés, vagyis ha valami úgy hápog és néz ki, mint egy kacsa, akkor az valószínűleg egy kacsa.

A fenti interfész egy kártya tömböt tartalmaz, továbbá származik a DeckOfCardsResponse-ból, melynek kódja az alábbi:

``` TS

interface DeckOfCardsResponse {
    success: boolean;
    deck_id: string;
    remaining: number;
}

```

A kártyák pedig így néznek ki:

``` TS

interface Card {
    image: string;
    value: Rank;
    suit: Suit;
    code: string;
    images: { [key: string]: string }
}

```

Az `images` objektum egy stringekkel indexelhető objektum, ami stringeket tartalmaz. Ez képzi le a válasz "format" : "file" formátumát, ami a fenti példában látható ("svg" : "..."). A value Rank típusú, a suit Suit típusú, amik rendre az alábbi típusoknak felelnek meg:

``` TS

type Rank = "ACE" | "2" | "3" | "4" | "5" | "6" | "7" | "8" | "9" | "10" | "JACK" | "QUEEN" | "KING";
type Suit = "SPADES" | "HEARTS" | "DIAMONDS" | "CLUBS";

```

???+ tip "Unió típusok"
    TypeScript-ben az `unió típusok` (`discriminated union type`) csak megadott értékeket vehetnek fel. Minden érték egyúttal saját maga típusa is, pl. az `"ACE"` érték egy olyan típus, ami kizárólag ezt az egyetlen értéket veheti fel. A fenti kódban a `Rank` és `Suit` unió típusok.

A játékunkat a statikus, aszinkron `NewGame` függvény meghívásával példányosíthatjuk. A játékban az osztó és a játékos kezét a `Hand` osztály reprezentálja:

``` TS
class Hand {
    constructor(public cards: Card[], public showFirst: boolean) {
    }

    get value(): number {
        let value = 0;
        let aces = 0;
        for (let card of this.showFirst ? this.cards : this.cards.slice(1)) {
            switch (card.value) {
                case "KING":
                case "QUEEN":
                case "JACK":
                case "10":
                    value += 10;
                    break;
                case "ACE":
                    aces++;
                    value += 11;
                    break;
                default:
                    const cardValue = parseInt(card.value);
                    if (cardValue > 1)
                        value += cardValue;
                    else
                        console.error(`Unknown card value: ${card.value} (${card.code})`);
                    break;
            }
        }
        // TODO: az ász érhessen 1-et is, ha túllépnénk vele a 21-et!
        return value;
    }
}
```

A `value` nevű mező csak lekérdezhető, számított érték: kiszámítja a kézben szereplő kártyák értékét.

### Beadandó
!!! example "1. feladat beadandó (1 pont)"
    Illessz be egy képernyőképet a böngészőről, ahol látható a `deckofcardsapi.com` oldalról a két kártya húzására kapott, JSON formátumú válasz! (`f1.png`)

## 2. feladat - Játék indítása

A játék indításához vegyük fel a `start.ts` fájlba az alábbi kódrészletet:

``` TS
let game : Game;

$(document).on("click", "#start-game-button", async e => {
    $("#start-game-button").hide(); 
    game = await Game.NewGame();
    game.dealersHand.cards.push(...(await game.Draw(2)).cards);
    game.playersHand.cards.push(...(await game.Draw(2)).cards);
    $("#hit-button, #stand-button").show();
    console.log(game);
    // TODO: kirajzolás
});
```

Ez a kód a dokumentum betöltődését követően figyel a `click` eseményekre. Ha a `start-game-button` ID-jú elemet nyomtuk meg, lefut a beregisztrált aszinkron callback eseménykezelő függvény, ami:

- elrejti a Start Game gombot,
- aszinkron példányosítja a játék objektumot a `Game` osztály statikus, aszinkron `NewGame` függvényének meghívásával, amit értékül ad a globális `game` változónak,
- húz 2 lapot az osztónak és a játékosnak, amit a játékosok kezeiben elhelyez, a <a href="https://www.typescriptlang.org/docs/handbook/variable-declarations.html#spread" target="_blank">spread</a> operátorral kibontva a tömbből az egyes elemeket,
- láthatóvá teszi a Hit és Stand gombokat,
- a konzolra logolja a játék állapotát.

<figure markdown>
  ![Console.log()](./assets/blackjack-start-console-log.png)
  <figcaption>Console.log() hívása indítás után</figcaption>
</figure>

Láthatjuk, hogy a játék elindult, de még nem jelenítjük meg a szükséges adatokat. A render.ts fájlba vegyük fel az alábbi kódot:

``` TS
function renderHands(game: Game) {
    $(".hand-title").show();
    $(".hand-container").text("");
    // TODO: csak akkor mutatjuk meg az osztó első lapját, ha a játékos végzett ("stand").
    // Ha nem látszik az első lap, helyette mutassunk hátlapot.
    for (let card of game.dealersHand.cards) {
        addCardToContainer("dealer", card);
    }
    for (let card of game.playersHand.cards){
        addCardToContainer("player", card);
    }

    renderCardValues(game);
}

function addCardToContainer(name: "dealer" | "player", card: Card) {
    $(`#${name}-container .hand-container`).append(getCardHtml(`./img/cards/${card.code}.png`, `${card.value} of ${card.suit}`));
}

function getCardHtml(cardUrl: string, cardText: string) {
    return `<img class="card-image" src="${cardUrl}" title="${cardText}" />`;
}


function renderCardValues(game: Game) {
    $("#dealer-container .cards-value").text(game.dealersHand.value);
    $("#player-container .cards-value").text(game.playersHand.value);
}
```

A függvények az alábbi funkciókkal rendelkeznek:

- `renderHands`: a paraméterül kapott játék állapotának megfelelően:
    - megjeleníti a címsor mezőket,
    - kitörli az esetleg a DOM-ban levő kártyákat (felülírja őket egy üres stringgel),
    - az osztó és játékos lapjait egyenként kirendereli `addCardToContainer` függvény segítségével,
    - kirajzolja a kártyák aktuális értékét,
- `addCardToContainer`: a megfelelő játékos kezébe kirajzolja a kártyát a `getCardHtml` függvény segítségével,
- `getCardHtml`: a paramétereknek alapján összeállítja a kártya képét reprezentáló HTML-t.

Ezután a játék állapotát ki is tudjuk rajzolni, ha a játék indítását kezelő eseménykezelőnk végén meghívjuk a `renderHands` függvényt:

``` TS

$(document).on("click", "#start-game-button", async e => {
    // ...
    renderHands(game);
});
```

<figure markdown>
  ![Első állapot megjelenítése](./assets/blackjack-initial-render.png)
  <figcaption>Első állapot megjelenítése</figcaption>
</figure>

### Új kártya húzása

A következő feladatunk a játék első lényegi eleme, az új kártya húzása. Ehhez a `start.ts` függvénybe vegyünk fel egy új eseménykezelőt:

``` TS
$(document).on("click", "#hit-button", async e => {
    $(".actions button").attr("disabled", "disabled");
    game.playersHand.cards.push(...(await game.Draw(1)).cards);
    renderHands(game);
    $(".actions button").removeAttr("disabled");
    // TODO: kezelni, ha a játékos túllépte a 21-et (bust)!
});
```

A fenti kód a korrábiakhoz képest értelemszerű, az egyetlen érdekesség, hogy a hívás előtt az akciógombokat letiltjuk (`attr`), a válasz megérkezése után pedig újra engedélyezzük őket (`removeAttr`).

### Beadandó
!!! example "2. feladat beadandó (1 pont)"
    Illessz be egy képernyőképet a játék állásásáról egy vagy több kártya húzása után! (`f2.png`)

## 3. feladat - Önálló feladatok

A következő feladatok közül a maximális 5 pont megszerzéséhez legalább 3 feladatot kell teljesítened! Tehát minden feladat 1 pontot ér, bármennyit megcsinálhatsz, de maximum +3 pontot szerezhetsz a vezetett rész 2 pontján túl. Bármely fájlban módosíthatsz, létrehozhatsz és törölhetsz is fájlokat.

- Az osztó első lapja ne legyen látható! Helyette használd a `back.png` fájlt! Ne legyen látható a `title` szöveg sem, ha az egeret a kártya fölé visszük!
- Kezeld az ászok értékét: az ász 11-et ér, ha nem lépnénk túl vele a 21-et, egyébként 1-et.
- Ha a játékos túllépi a 21-et: ebben az esetben jeleníts meg egy megfelelő üzenetet, és legyen lehetőség új játék indítására!
- Ha a játékos megnyomja a Stand gombot: a játékos nem kér több lapot, ekkor viszont az osztó nem látható lapja felfedésre kerül, majd az osztó addig húz automatikusan, amíg el nem éri a játékos pontszámát vagy túllépi a 21-et (előbbi esetben az osztó nyer, utóbbiban veszít). A feszültség fokozása érdekében használhatod a `setTimeout(() => { /* ... */}, 2000)` hívást, amivel várakoztathatod a futást, mintha az osztó gondolkodna 2 másodpercig.
- Kezeld, ha a játékos győz!
- Kezeld a játékos pénzét! A játékos 1000$-ról indít, minden játék 100$-ba kerül, amit győzelem esetén a játékos duplán elnyer.
- Kezeld a *split* szabályt: a játékos, ha a játék elején két ugyanolyan értékű lapja van, választhat egy új lehetőséget: *split*. Ekkor az egy-egy ugyanolyan értékű lap két külön kezébe kerül, a tét duplázódik, és mindkét új kezébe 1-1 új lapot kap. Mindkét kezéhez külön kéthet új lapot, vagy megállhat. A két keze külön-külön értékelődik ki az osztó lapjaival, tehát 0, 1 vagy 2 kezével nyerhet, ennek megfelelően részesül jutalomban.

### Beadandó
!!! example "3. feladat beadandó (3 pont)"
    Illessz be minden elkészített feladatról 1-1 képernyőképet! A pull request szövegébe írd bele azt is, hogy melyik feladatokat oldottad meg (bemásolhatod a feladat szövegét)! (`f3.png` - `f9.png`)