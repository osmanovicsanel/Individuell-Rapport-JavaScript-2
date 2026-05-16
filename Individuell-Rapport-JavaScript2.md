# Individuell rapport - Receptapplikation
**Namn: Sanel Osmanovic**
**Kurs: JavaScript2**

---


## Del 1: React-frågor (Godkänt)

### 1. Vad är JSX och hur skiljer det sig från vanlig HTML? ###

JSX står för JavaScript XML och man kan säga att det är som ett "extraspråk" för JavaScript som man brukar använda i React. Man kombinerar helt enkelt JavaScript och HTML. Det kan ofta se ut som HTML men det omvandlas till JavaScript och man skriver dem i JavaScript-filerna. Att använda JSX gör det enklare att bygga dynamiskt.

Det finns några skillnader mellan JSX och vanlig HTML men de enklaste och tydligaste skillnaderna är:
**- Parent element:** Man kan inte returnera två `<h1>`-taggar bredvid varandra utan att lägga in dem i en gemensam `<div>` eller en fragment-tagg `(<> ... </>)`
**- Stängningstaggar:** I HTML kan man använda `<img>` eller `<input>` utan att stänga men i JSX är det ett krav att de måste stängas med ett snedstreck `<img />` eller `<input />`.

### 2. Vad är en component i React och varför använder vi components? ###

**Svar**
Enkelt förklarat är en component något vi kan skapa för att återanvända kod. Istället för att skriva samma kod på flera olika ställen så skapar man en component som man återanvänder. T.ex. Om jag har en snygg knapp med hover-effekt som jag vill använda på flera ställen på min sida, då skapar jag en component som jag sedan använder på de ställen jag vill. Det är alltså en JavaScript-funktion som returnerar JSX.
Fördelen med att använda components är just att den är återanvändbar, enklare att underhålla samt tydligare ansvar. Det som är viktigt dock är att det måste börja med en stor bokstav t.ex. `UserButton`.

### 3. Förklara skillnaden mellan props och state. ###

Både props och state hanterar data i en component men det används på olika sätt.
**- Props:** Är data som skickas till en component utifrån. Det är den övre component (föräldern) som äger datan och sedan refererar till en annan component (barnet). Det viktiga att känna till med props är att de enbart är read-only. Vilket innebär att en component inte kan ändra sina egna props.

**- State:** Är data som component hanterar själv. Man kan säga att det är components egna minne. Datan i en component kan ändras och oftast efter att användaren gör något t.ex. klickar på en knapp eller skriver i ett fält. State triggar en om-rendering av sidan.

### 4. Vad gör `useState` och hur använder vi det? Ge ett exempel. ###

Eftersom props är read-only och det är statisk data så behöver vi göra det mer interaktivt. Vi kanske vill kunna klicka vidare på en hemsida, vi kanske vill ha ett fält där man kan skriva någonting. Det är här `useState` kommer in i bilden. Vanliga JavaScript-variabler glömmer bort sina värden från en component så fort de ändras och de nollställs vid en omrendering. När vi anropar `useState` returnerar den alltid en array med två saker:

1. En variabel som håller det nuvarande värdet.
2. En speciell funktion som vi använder för att uppdatera värdet. Då förstår React att sidan måste renderas om så att den nya datan visas.

**Exempel:**

```jsx
import { useState } from 'react';

function Counter () {
    const [count, setCount] = useState(0);   // count är nuvarande värdet och det startar på 0, setCount är funktionen som ändrar det.

    return (
        <div>
        <p>Du har klickat {count} gånger</p>
        <button onClick={() => setCount(count + 1)}>
        Klicka här
        </button>
        </div>
    );
}
```

### 5. Vad gör `useEffect` och varför behövs det? Ge ett exempel på när man använder det? ###

Det är en funktion som används för att köra kod vid specifika tillfällen istället för att koden körs varje gång sidan ritas om. När man använder `useEffect` är det två saker man gör.

1. Det är den kod som faktiskt ska köras, alltså funktionen.
2. En dependency array som talar om för React när funktionen ska köras igen. Om den till exempel är tom [] körs den bara en gång när sidan laddas.

Ett exempel när `useEffect` behövs är om vi ska hämta data från ett externt API. Om vi lägger in kod som hämtar data från ett API direkt i components utan `useEffect`, skulle koden köras varje gång components renderas om. Om components renderas om 50 gånger skulle vi göra 50 API-anrop. Det kan göra att applikationen kraschar eller går långsamt.

Under en lektion gjorde vi en övning som visar exakt hur detta fungerar. Vi fick en kod där en timer skulle räkna upp med 1 varje sekund. Koden såg ut så här:

```jsx
import { useState, useEffect } from 'react'

export default function Counter() {
    const [count, setCount] = useState(0)

    useEffect(() => {
        const id = setInterval(() => {
            setCount(count + 1)
        }, 1000)
        return () => clearInterval(id)
        }, []) // Denna tomma array gör att den bara körs en gång vid start.

        //....return JSX
    
}
```

Eftersom arrayen i slutet är tom så körs `useEffect` bara en gång i början. När intervallet skapades var `count` lika med noll. Den fortsatte göra samma uträkning ( 0 + 1 ) varje sekund p.g.a att timern låste in startvärdet. För att lösa detta hade man behövt lägga till count ([count]) i arrayen så att varje gång siffran ändras körs effekten om.

 ### 6. Vad är en controlled component när det gäller formuläret i React? ###

Det är det vanligaste och säkraste sättet att hantera formulär eller input-fält i React. React har kontroll över värdet via state.
I vanliga HTML-formulär håller webbläsaren själv reda på vad användaren skriver i ett fält. Men i React vill vi koppla ihop fältets värde med ett state, på så sätt blir det en controlled component. Statet blir då single source of truth.

För att den ska bli controlled behöver vi sätta ett värde på inputen (`value`) till vår state-variabel. Vi behöver också en `onChange`. Varje gång en användare skriver något körs funktionen `onChange` som uppdaterar vårt state med det nya värdet (e.target.value).


 ### 7. Vad gör React Router och varför använder vi det istället för vanliga HTML-länkar? ###

Det är ett bibliotek som man kan ladda ner som hanterar navigeringen (routing) i en React-applikation. En React-applikation är en SPA (Single Page Application) och det betyder att hela webbplatsen bara består av en enda HTML-fil. Istället för att ladda in nya HTML-filer från en server när användaren klickar på en länk, laddar React Router snabbt in och byter ut olika components på skärmen t.ex. Home eller About.

När vi navigerar i React använder vi komponenten `<Link>` istället för `<a>` (vanlig HTML-länk). Det gör vi för att en vanlig HTML-länk (`<a>`) tvingar hela webbläsaren att göra en ny förfrågan till servern, ladda om hela sidan och hämta allt på nytt medan React Router stoppar denna omladdning och bara byter ut komponenten direkt.
En till anledning till varför vi använder React Router är för att med en vanlig HTML-länk förlorar vi all data i våra states när sidan laddas om. Om vi till exempel har lagt något i en varukorgen så skulle det försvinna så fort man klickar på en länk. React Router behåller appens minne intakt under hela besöket.

 ### 8. Vad är Zustand och vilket problem löser det? ###

Zustand är också ett bibliotek i React men mer som ett globalt state bibliotek. Istället för att varje komponent har sitt egna lokala state via `useState`, skapar man med Zustand en store utanför komponenterna. Därifrån kan sedan komponenterna hämta eller uppdatera data direkt.

Det absolut vanligaste problemet som Zustand löser kallas för "Prop Drilling". När en applikation blir större och man har komponenter inuti komponenter kan det hända att en komponent högt upp i det trädet har data som en komponent längst ner behöver. Utan Zustand blir koden rörig och svår att underhålla eftersom vi måste skicka den datan som en prop genom varje lager, även genom komponenter som inte ens bryr sig om den datan. Men med Zustand slipper man detta. Komponenten längst ner kan "koppla upp sig" direkt mot den globala storen som vi skapat med Zustand och hämta det den behöver, utan någon mellanhand.

----------------------------------------------

## React-frågor (Väl Godkänt)

### 1. Förklara vad som händer när state updateras i en React component. Vad innebär en "re-render"? ###

**Svar**

### 2. Vad är skillnaden mellan att hantera state lokalt med `useState` och globalt med Zustand? När passar vilket? ###

**Svar**

### 3. Vad är "lifting state up" och varför behöver man ibland göra det? ###

**Svar**

### 4. Förklara dependency array i `useEffect`. Vad händer om den är tom, har värden eller saknas helt? ###

**Svar**


---

## Del 2: Reflektion (Godkänt)

Minst 300 ord

### - Vad byggde ni för applikation och varför valde ni det temat? ###

### - Hur delade ni upp arbetet i gruppen? ###

### - Vilka React-koncept använde ni i projektet? Ge konkreta exempel. ###

### - Vad var svårast och hur löste ni det? ###

----------------------------------------------

## Reflektion (Väl Godkänt)

Minst 500 ord

### - Om du fick börja om från scratch, vad hade du gjort annorlunda? Varför? ###

### - Hur hanterade ni state i applikationen? Var valde ni lokal state vs Zustand och varför? ###

### - Vad lärde du dig av peer review-sessionen? Var det något i en annan grupps kod som du tyckte var bra eller som du hade gjort annorlunda? ###