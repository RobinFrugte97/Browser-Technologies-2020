## Browser tech

### Opdracht 2

### Enquete Minor Web

Voor opdracht 2 ga ik een progressive enhanced enquete maken.

### Prototype

[Link naar het prototype](https://minorenquetebt.herokuapp.com/)

### Install

Clone de repo naar je eigen omgeving

`git clone https://github.com/RobinFrugte97/Browser-Technologies-2020`

Navigeer naar de repo

`cd Browser-Technologies-2020`

Installeer de dependencies

`npm install`

Run de applicatie

`npm run dev`

## Use case

Ik wil een enquete kunnen invullen over de minor Web Development, met verschillende antwoord mogelijkheden. Als ik de enquete niet afkrijg, wil ik later weer verder gaan met waar ik ben gebleven.

## Basis functionaliteit

Wanneer de gebruiker Javascript en/of CSS uit heeft staan, of om wat voor reden dan ook niet binnen krijgt, werkt de basis functionaliteit in HTML.

De vragen worden server-side opgesteld en afgeleverd met Express en EJS. Vervolgens worden de antwoorden server-side opgeslagen. Wanneer de  gebruiker later verder wilt, kan er server-side worden gekeken of de gebruiker al voortgang heeft in de enquete en zoja verder gaan waar de gebruiker gebleven was.

Ook kan de gebruiker terug gaan naar de vorige vraag met de `terug` knop. De antwoorden van de vorige vraag worden dan weer terug gegeven aan de gebruiker, zodat de gebruiker weet wat hij/zij heeft ingevuld.

## Eerste Feedback moment
<details><summary>Feedback vragen</summary>
1. Zou je mij feedback kunnen geven op de structuur/semantiek van mijn verschillende forms?
- [Een radio form](https://github.com/RobinFrugte97/Browser-Technologies-2020/blob/master/views/questions/vraag1.ejs)
- [Een textarea form](https://github.com/RobinFrugte97/Browser-Technologies-2020/blob/master/views/questions/vraag2.ejs)
- [Een 'number' form](https://github.com/RobinFrugte97/Browser-Technologies-2020/blob/master/views/questions/vraag3.ejs)
- [Een text form](https://github.com/RobinFrugte97/Browser-Technologies-2020/blob/master/views/gegevens.ejs)

2. Zou je mij feedback kunnen geven op de Findings/feature detections in mijn readme? Is het bijvoorbeeld nodig om voorbeelden van wanneer het niet werkt toe te voegen? Of een caniuse.com link van iets dat niet werkt op een bepaalde browser?

3. Zou je feedback kunnen geven op het prototype qua basis functies? Is er iets "obvious" voor de user experience dat ik over het hoofd zie?

</details>

## To-Do's

<details><summary>To-do lijst</summary>

- Ingevulde vragen weer ophalen als je op `Terug` klikt. [CHECK]
- Required's aangeven, textarea uit de required's halen. [CHECK]
- Extra input type. [CHECK]
- Number naar range omzetten met JS. [CHECK]
- JS form validation. [CHECK]

[CSS selectors van PPK](https://quirksmode.org/css/selectors/)

</details>

## <a id="progressive-enhancement"></a> Functional laag/Core functionaliteit

De functional laag is de kale HTML die in verbinding staat met de Node server. De functional laag geeft alle basis functionaliteiten en niet meer. Er is geen CSS en geen client-side Javascript. De gebruiker kan de enquete volledig invullen, alleen is het geen plezier voor het oog. De antwoorden worden nogsteeds opgeslagen en de gebruiker kan nogsteeds op een ander moment verder doorgaan met de enquete. De gebruiker kan nogsteeds naar een vorige vraag navigeren, waarbij de reeds ingevulde antwoorden nogsteeds worden teruggegeven aan de gebruiker.

![](https://raw.githubusercontent.com/RobinFrugte97/Browser-Technologies-2020/master/screenshots/functionallaag.png)


## Usable laag

De Usabel laag bestaat uit CSS. Het is bedoeld om de gebruiker meer te geven dan kale HTML. De text is beter leesbaar dankzij css en de enquete is mobile-first opgebouwd. Er is een kleur toegevoegd, maar om wille van contrast maak ik gebruik van zwarte borders, text en buttons.

De enquete blijft prettig leesbaar doordat het formulier een maximale breedte heeft gekregen. Waar nodig is de content onder elkaar geplaatst in plaats van naast elkaar. Dit verschilt tussen mobiel en desktop.

Mobiel:

![](https://raw.githubusercontent.com/RobinFrugte97/Browser-Technologies-2020/master/screenshots/usablelaag.png)


Desktop: 

![](https://raw.githubusercontent.com/RobinFrugte97/Browser-Technologies-2020/master/screenshots/usablelaagdesktop.png)

## Pleasurable/enhanced laag

#### Grote radio buttons

Ik maak gebruik van extra grote labels voor de radio buttons. Dit maakt mobiel gebruikt vele male prettiger dan dat je op een klein radio buttontje moet klikken, of op het label die er niet naar uitziet om op te klikken.

![](https://raw.githubusercontent.com/RobinFrugte97/Browser-Technologies-2020/master/screenshots/radiobuttons.png)

#### Range slider

Als enhancement heb ik range sliders toegevoegd aan alle nummer inputs. Op die manier hoef je niet meer te typen en kun je simpelweg op de slider selecteren welk getal je wilt kiezen. Je hebt gelijk feedback in het nummer input veld, want die verandert mee met de slider.

![](https://raw.githubusercontent.com/RobinFrugte97/Browser-Technologies-2020/master/screenshots/nummerslider.png)


## <a id="feature-detection"></a> Findings/Feature detection

#### addEventListener

Oude versies van Internet Explorer ondersteunen `addEventListener` niet. Dit kan opgelost worden door te checken of `addEventListener` bestaat en zo niet een `attachEvent` fallback te schrijven.

```javascript
if (document.addEventListener) {
    slider.addEventListener('input', function () {
        nummer.value = slider.value
    })
    // Event 'change' voor IE.
    slider.addEventListener('change', function () {
        nummer.value = slider.value
    })
    if (nummer !== null) {
        nummer.addEventListener('input', function () {
            slider.value = nummer.value
        })
    }
} else {
    slider.attachEvent("oninput", function () {
        nummer.value = slider.value
    })
    // Event 'onchange' voor IE.
    slider.attachEvent("onchange", function () {
        nummer.value = slider.value
    })
    if (nummer !== null) {
        nummer.attachEvent("oninput", function () {
            slider.value = nummer.value
        })
    }
} 
```

#### Slider

Het input veld moet worden aangepast wanneer je bezig bent met het verslepen van de slider. Om dit in zowel Chrome, Firefox en Internet Explorer te laten werken heb je verschillende soorten events nodig op de slider. 

Chrome en Firefox willen graag het event `input` hebben, terwijl Internet Explorer graag het event `change` wilt hebben.

Oplossing:

```javascript
slider.addEventListener('input', function () {
    nummer.value = slider.value
})
// Event 'change' voor IE.
slider.addEventListener('change', function () {
    nummer.value = slider.value
})
```

Ik zet meerdere eventlisteners op de slider. Zo kan Internet Explorer gebruik maken van `change`, terwijl Chrome en Firefox beide pakt. Het feit dat Chrome en Firefox beide pakt merk je niks van, omdat dezelfde functie in beide eventlisteners wordt uitgevoerd.

#### Submit

- `<input type="submit">` kan niet buiten de bijbehorende form in Internet Explorer 11, ook als ze gelinkt zijn via een `form=""`.
Het idee was om de `submit` in de navigatie te plaatsen als zijnde een "verder" knop, naast de "terug" knop.

Het idee:

```html
<form action="/vraag2" id="vraag1">...</form>
<nav>
    <ul>
        <a href="/vraag1">Terug</a>
        <input form="vraag2" type="submit" value="Verder">
    </ul>
</nav>
```

Werkt niet in IE 11, dus oplossing `<nav>` in de `<form>`:
```html
<form action="/vraag2" id="vraag1">
...
  <nav>
      <ul>
          <a href="/vraag1">Terug</a>
          <input form="vraag2" type="submit" value="Verder">
      </ul>
  </nav>
</form>
```

#### Radio button styling

- De labels van een :checked radio input kunnen alleen gestyled worden als de label na de radio input staat, in plaats van eromheen.

Normale input & label:

```html
<label>
    Cijfer van 1 tot 10
    <input type="number" name="Slack cijfer" min="1" max="10">
</label>
```

Radio input alternatief:

```html
<input id="vraag3" value="Leuk!" type="radio" name="vraag1">
<label for="vraag3">
    Leuk!
</label>
```

#### Input type number

Bij een aantal vragen in de enquete vraag ik om een nummer. Dit deed ik eerst met een input type number `<input type="number">`. 

In het volgende [Gov.uk technology blog artikel](https://technology.blog.gov.uk/2020/02/24/why-the-gov-uk-design-system-team-changed-the-input-type-for-numbers/) wordt beschreven dat dit een heleboel usability problemen met zich mee brengt, waaronder veel screenreader problemen.

De oplossing is om een input type text met een `inputmode="numeric"` en een `pattern="*regex*"` te gebruiken. In de regex zeggen dat alleen nummer worden geaccepteerd. In het label van de input kun je gebruikers feedforward geven over wat er in de input verwacht/geaccepteerd wordt.

Oud number input voor een cijfer 1 t/m 10:

```html
<label>
    Cijfer van 1 tot 10
    <input type="number" name="Minor cijfer" min="1" max="10">
</label>
```

Nieuw input voor een cijfer 1 t/m 10:

```html
<label>
    Cijfer van 1 tot 10
    <input name="Minor cijfer" type="text" inputmode="numeric" pattern="^(?:[1-9]|0[1-9]|10)$">
</label>
```

## CSS Fallback

#### Display: inline-grid

Voor de desktop versie van de enquete is wat extra styling nodig. Om de inputvelden met labels nog goed leesbaar te houden bijvoorbeeld. Daarvoor maak ik gebruik van `display: inline-grid`. Dit is niet ondersteund in Internet Explorer.
De fix in Internet Explorer is om de labels een `float: left` te geven. Ik check met `@supports (display: inline-grid)` of `display: inline-grid)` gesupport wordt. Zo ja, voer de code uit die binnen de `@supports` staat. Zo nee, negeer deze code en houd de oude float in stand.

```css
fieldset > label {
        float: left;
    }
    
@supports (display: inline-grid) {
    fieldset {
        display: inline-grid;
    }

    fieldset > label {
        float: none;
    }
}
```

#### Display: flex

In mijn fieldset maak ik gebruik van `Display: flex` om de vragen netjes onder elkaar te zetten en ze in het midden uit te lijnen. Flex gebruik ik ook om buttons te positioneren. Wederom maak ik gebruik van `@supports` om te checken of de browser `Display: flex` ondersteunt.


```css
@supports (display: flex) {
    fieldset {
        display: flex;
        flex-direction: column;
        float: none;
    }
    fieldset > label {
        float: none;
    }
}
```

## <a id="test-features"></a> Test features

### Getest in Chrome version 80 op Windows 10

### Afbeeldingen
De site maakt geen gebruik van afbeeldingen. Er verandert niks aan de enquete.

### Custom fonts
Ik maak gebruik van een custom font op de website. De kans bestaat dat om wat voor reden dan ook de font niet geladen kan worden.

Ik heb font fallbacks in mijn css: 

```css
font-family: Lato, Arial, 'Open Sans', sans-serif;
```

Met custom fonts: 

![](https://raw.githubusercontent.com/RobinFrugte97/Browser-Technologies-2020/master/screenshots/usablelaag.png)

Zonder custom fonts:

![](https://raw.githubusercontent.com/RobinFrugte97/Browser-Technologies-2020/master/screenshots/geenfonts.png)

### JavaScript
De enquete werkt helemaal zonder Javascript, omdat Javascript alleen wordt gebruikt voor pleasurable enhancements.

De slider bij het kiezen van een cijfer zal dus niet ingeladen worden. Ook wordt de form feedback bij het gegevens scherm niet geupdatet.

Met slider: 

![](https://raw.githubusercontent.com/RobinFrugte97/Browser-Technologies-2020/master/screenshots/nummerslider.png)

Zonder slider:

![](https://raw.githubusercontent.com/RobinFrugte97/Browser-Technologies-2020/master/screenshots/geenjs.png)


### Kleur
De kleuren in de enquete hebben een goed contrast, waardoor de site goed werkt in bijvoorbeeld zwart-wit.

![](https://raw.githubusercontent.com/RobinFrugte97/Browser-Technologies-2020/master/screenshots/contrast.png)


### Breedband internet
De enquete werkt precies hetzelfde, alleen wat trager.

### Cookies
Ik maak op het moment geen gebruik van cookies in de website.


### Local Storage
Ik maak geen gebruik van local storage in de website.

### Muis / Trackpad
Er zijn focus-states aangebracht. Je kunt door de enquete navigeren met je toetsenbord.


## Devices en browsers voor het testen

### Desktop Windows 10

#### Chrome version 80
Primaire development browser. Werkt zoals het bedoeld is. Geheel responsive en styling is 100% perfect. De meeste bugs en styling issues zijn tijdens developement weggewerkt. Alle screenshots in de readme komen uit Chrome 80

#### Firefox version 59.0
Paar kleine styling onenigheden, zoals `width: fit-content;` dat niet werkt naar zin. Ook is de input type range anders gestylet dan in de andere browsers, omdat er andere prefixes zijn voor elke browser. De progress bar is ook iets anders gestylet weer door de prefixes.

![](https://raw.githubusercontent.com/RobinFrugte97/Browser-Technologies-2020/master/screenshots/firefox.png)

#### Internet Explorer version 11
Internet explorer is qua styling iets lastiger, omdat veel dingen of niet ondersteund zijn, of anders werken dan dat je verwacht. De progress bar is niet te stylen en de range input is erg lastig te stylen. Ook in IE werkt `width: fit-content;` niet, dus daar zitten wat styling onenigheden in. Ik werk in IE veel met `float` om de content netjes onder elkaar te krijgen. Resultaat is dat de content niet in het midden staat, maar voor de rest werkt alles in IE.

![](https://raw.githubusercontent.com/RobinFrugte97/Browser-Technologies-2020/master/screenshots/internetexplorer.png)

### Samsung Galaxy S8 

#### Chrome
Op Chrome mobile werkt de enquete hetzelfde als Chrome desktop. De enquete is mobile-first ontworpen, dus er zijn geen verrassingen.

![](https://raw.githubusercontent.com/RobinFrugte97/Browser-Technologies-2020/master/screenshots/mobile.png)

# Conclusie

## Core functionaliteit 
Wanneer de gebruiker Javascript en/of CSS uit heeft staan, of om wat voor reden dan ook niet binnen krijgt, werkt de basis functionaliteit in HTML. De enquete kan ten aller tijden worden ingevuld.

De vragen worden server-side opgesteld en afgeleverd met Express en EJS. Vervolgens worden de antwoorden server-side opgeslagen. Wanneer de  gebruiker later verder wilt, kan er server-side worden gekeken of de gebruiker al voortgang heeft in de enquete en zoja verder gaan waar de gebruiker gebleven was.

Ook kan de gebruiker terug gaan naar de vorige vraag met de `terug` knop. De antwoorden van de vorige vraag worden dan weer terug gegeven aan de gebruiker, zodat de gebruiker weet wat hij/zij heeft ingevuld.

## Toegankelijkheid
Zie het feature onderzoek [hierboven](#test-features).

## Probleemdefinitie en oplossing
Zie de feature detection/findings documentatie [hierboven](#feature-detection).

## Progressive enhancement 
Progressive enhancement is wanneer je vanuit de basis een website 'enhancet' met extraatjes. Je gaat stapje voor stapje door een aantal lagen heen. Eerst moet het functioneel zijn. Werkt de basis van de website helemaal en altijd? Ga dan naar de bruikbare laag. Hier maak je de website bruikbaarder, door bijvoorbeeld custom css fonts toe te voegen en de website beter bruikbaar te maken door de font size te vergroten. Daarna ga je door naar pleasurable. Deze laag bestaat uit dingen die echt extra zijn en voor een soort 'wow' factor zorgen in je website. Denk aan Javascript en CSS enhancements. De site moet werken als een van de lagen wegvalt.

## Progressive Enhancement in project
Zie [Progessive Enhancement](#progressive-enhancement).

## Feature detection
Feature detection is wanneer je eerst kijkt of een feature ondersteund wordt in de browser of omgeving, en daarna pas de feature toepast. Ook kan je feature detection gebruiken om fallbacks te schrijven in Javascript en CSS

## Feature Detection  in project
Zie [Feature detection/findings](#feature-detection).



## Wireflow
<details><summary>Wireflow</summary>


Ik schets eerst een wireflow en/of breakdown-schets met hoe de demo moet gaan werken en hoe het eruit komt te zien.

Het idee is dat de student voor het invullen van de enquete hun studentnummer invult, zodat de enquete later verder ingevuld kan worden.

#### Wireflow

![](https://raw.githubusercontent.com/RobinFrugte97/Browser-Technologies-2020/master/screenshots/wireflow.png)

#### Start scherm

![](https://raw.githubusercontent.com/RobinFrugte97/Browser-Technologies-2020/master/screenshots/wireflow1.png)

#### Gegevens scherm

![](https://raw.githubusercontent.com/RobinFrugte97/Browser-Technologies-2020/master/screenshots/wireflow2.png)

#### Radio button scherm

![](https://raw.githubusercontent.com/RobinFrugte97/Browser-Technologies-2020/master/screenshots/wireflow3.png)

#### Textbox scherm

![](https://raw.githubusercontent.com/RobinFrugte97/Browser-Technologies-2020/master/screenshots/wireflow4.png)

#### Slider scherm

![](https://raw.githubusercontent.com/RobinFrugte97/Browser-Technologies-2020/master/screenshots/wireflow5.png)

#### Later verder notificatie scherm

![](https://raw.githubusercontent.com/RobinFrugte97/Browser-Technologies-2020/master/screenshots/wireflow6.png)

#### Verder gaan scherm

![](https://raw.githubusercontent.com/RobinFrugte97/Browser-Technologies-2020/master/screenshots/wireflow7.png)

</details>

## Opdracht 1

<details><summary>Opdracht 1</summary>

### WAFS site
Mijn Web App From Scratch site gaat over Marvel superheroes. Er is een overzicht van characters met elk een detail pagina. Op een detail pagina heb je een grotere afbeelding en kun je zien in welke comic het desbetreffende character in voor komt.

[Link naar prototype](RobinFrugte97.github.io/wafs/app/)

### De 8 test features

1. Afbeeldingen uit
2. Custom fonts uit
3. Geen Javascript
4. Kleur uit
5. Breedband internet
6. Cookies
7. Local Storage
8. Muis/Trackpad

### Getest in Chrome version 80 op Windows 8.1


### Afbeeldingen
De site maakt veel gebruik van afbeeldingen, zowel op de hoofdpagina als op de detailpaginas. Ik krijg alle afbeeldingen in een zelf gekozen grootte terug van de Marvel API. Elke afbeelding die een superhero afbeeldt is een JPG. Het logo is een PNG afbeelding.

Problemen:
1. De images hebben geen `alt`. Als de afbeeldingen om wat voor reden dan ook niet laden, dan weet je niet waar het om gaat.
2. De site bestaat grotendeels uit afbeeldingen. Voor blinden of slechtzienden is de site dus praktisch onbruikbaar.

Oplossingen:
1. Per image wordt nu naast een `<img>` src ook een `<img>` alt gerendert.
  Code voor:
  ```Javascript
  thumbnail: {
    src: function(){
      return this.thumbnail
    }
  }
  ```
  Code na:
  ```Javascript
  thumbnail: {
    src: function(){
      return this.thumbnail
    },
    alt: function(){
      return "Thumbnail afbeelding van ${this.character}"
    }
  }
  ```
2. Per image wordt nu ook een `<img>` title gerendert, met daarin de naam van de desbetreffende superhero.  


### Custom fonts
Ik maak gebruik van een custom font op de website. De kans bestaat dat om wat voor reden dan ook de font niet geladen kan worden.

Problemen:
1. Er is geen geteste fallback font.

Oplossingen:
1. Helvetica toegevoegd als fallback font. Als fallback daarop een sans-serif font.
`font-family: americanCaptain, helvetica, sans-serif;`


### JavaScript
De website werkt niet zonder JavaScript. Zonder JavaScript zul je niet verder komen dan de loading spinner.

Problemen:
1. De website werkt niet zonder JavaScript. De data wordt niet opgehaald en er wordt geen pagina gerendert.

Oplossingen:
1. De site zou aan de server kant gerendert kunnen worden. Bij PWA wordt daar aan gewerkt


### Kleur
De site heeft een aantal verschillende kleuren, dankzij de vele verschillende afbeeldingen en kleurrijke characters.

De site is goed te gebruiken in zwart-wit.

![Zwart-wit](https://raw.githubusercontent.com/RobinFrugte97/Browser-Technologies-2020/master/screenshots/blackandwhite2.png)


### Screenreader
Ik heb de site getest met een screenreader. De gebruikte screenreader heet [NVDA](https://www.nvaccess.org/). De reader leest per character de afbeeldingen en de naam van het character.

![reader](https://raw.githubusercontent.com/RobinFrugte97/Browser-Technologies-2020/master/screenshots/screenreader.png)

### Breedband internet
De site is bijna geheel gebaseerd rond de Marvel API.
De API call die naar alle characters wordt gemaakt duurt op langzaam 3G netwerk minimaal 10 seconden. Vervolgens moeten alle afbeeldingen geladen worden. Per afbeelding duurt het 2 tot 5 seconden voor het geladen is.

Problemen:
1. Op een langzaam 3G netwerk duurt het erg lang om de site te laden. Dit komt mede door de vele afbeeldingen die geladen moeten worden.
2. Er wordt een groot aantal afbeeldingen binnen gehaald. Dit kan problemen veroorzaken bij iemand met een trage internet snelheid, zoals een langzame 3G verbinding op hun telefoon.

Oplossingen:
1. De Marvel API biedt verschillende afbeelding formaten. Je zou binnen JavaScript kunnen checken of de afbeeldingen er te lang over doen en vervolgens doorgaan met een kleiner formaat afbeeldingen.
2. De API call naar Marvel is eenvoudig te limiten. Zo is het aantal characters dat opgehaald wordt beperkt.

### Cookies
Ik maak op het moment geen gebruik van cookies in de website.


### Local Storage
Ik maak geen gebruik van local storage in de website.

Problemen:
1. Omdat er geen gebruik wordt gemaakt van local storage, moet de gebruiker elke keer de afbeeldingen en andere content opnieuw downloaden. Dit kan met een trage verbinding problemen opleveren.

Oplossingen:
1. De images zouden in de local storage opgeslagen kunnen worden voor mensen die vaker op de site terug komen, in het geval dat de gebruiker langzaam internet heeft.


### Muis / Trackpad
Ik had tijdens het maken van de app bij Web App From Scratch geen aandacht besteed aan hoe de site te bedienen is zonder muis.

Problemen:
1. Geen focus-states om door de website te navigeren met een toetsenbord.

Oplossingen:
1. Duidelijke focus-states toegevoegd aan alle elementen in de site die een functionaliteit hebben.

![Focusstate](https://raw.githubusercontent.com/RobinFrugte97/Browser-Technologies-2020/master/screenshots/focus-state.png)


## Devices en browsers voor het testen

### ASUS Windows 8.1 laptop

#### Firefox version 59.0
![firefoxscreenshot](https://raw.githubusercontent.com/RobinFrugte97/Browser-Technologies-2020/master/screenshots/BTFF.png)
- In Firefox werkt de site goed, alleen worden missende plaatjes op een andere manier getoond.

#### Internet Explorer version 11
![internetexplorerscreenshot](https://raw.githubusercontent.com/RobinFrugte97/Browser-Technologies-2020/master/screenshots/BTIE11.png)
- De website komt niet voobij de loading state in Internet Explorer. Dit komt omdat de JavaScript kapot gaat. Internet Explorer ondersteunt geen Fetch. Dit is de manier 
waarmee ik de API calls doe. XMLHttpRequests worden wel ondersteund door Internet Explorer.

### Microsoft Surface (Windows 8.1)
#### Internet Explorer version 11

- JS fetch wordt niet ondersteund. De api call wordt nooit gedaan. Er is dus geen content.
    - De oplossing zou kunnen zijn: Een fallback schrijven in de Javascript naar XMLHttpRequest. Deze manier van api call wordt wel door IE 11 ondersteund.
- De styling gaat kapot, omdat de javascript niet voorbij de loader komt.


## NOTES

support detection & browsers

Als je iets wilt gebruiken, kun je in css en js kijken of het gesupport wordt.

CSS: @supports

```css
@supports (display: grid) {
  ...
}

@supports not (display: grid) {
  ...
}
```

JS: if (*check of er een waarde uit hetgene komt dat je wilt checken*)

In oude browsers:
```js
if (window.localStorage) {
  localStorage.setItem(etc)
}
```

Kan ook:
```js
if (localStorage) {
  localStorage.setItem(etc)
}
```

Render blocking
-link naar css of js, dan stopt html parser.
zet css maar boven in je pagina, accepteer dat het even blockt

defer vs async

do it now:
<script src>

Do it later:
<script defer src>

I dont care when, just not now:
<script async src>


Testen:

blink 
webkit safari
gecko

</details>
