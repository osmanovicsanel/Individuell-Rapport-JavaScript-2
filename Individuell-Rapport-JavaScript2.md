# Individuell rapport - Receptapplikation
**Namn: Sanel Osmanovic**  
**Kurs: JavaScript2**

---


## Del 1: React-frågor (Godkänt)

### 1. Vad är JSX och hur skiljer det sig från vanlig HTML? ###

JSX står för JavaScript XML och man kan säga att det är som ett "extraspråk" för JavaScript som man brukar använda i React. Man kombinerar helt enkelt JavaScript och HTML. Det kan ofta se ut som HTML men det omvandlas till JavaScript och man skriver dem i JavaScript-filerna. Att använda JSX gör det enklare att bygga dynamiskt.

Det finns några skillnader mellan JSX och vanlig HTML men de enklaste och tydligaste skillnaderna är:  
**- Parent element:** Man kan inte returnera två `<h1>`-taggar bredvid varandra utan att lägga in dem i en gemensam `<div>` eller en fragment-tagg `(<> ... </>)`.  
**- Stängningstaggar:** I HTML kan man använda `<img>` eller `<input>` utan att stänga men i JSX är det ett krav att de måste stängas med ett snedstreck `<img />` eller `<input />`.

### 2. Vad är en component i React och varför använder vi components? ###

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

När vi uppdaterar värdet på en state triggar React en uppdatering av det visuella innehållet, alltså en re-rendering. Så till exempel om vi skulle ha en komponent som visar en produkt. Produkten har en knapp som heter "visa detaljer". State kommer då heta `showDetails` och det är inställt på `false` från början. När användaren trycker på "visa detaljer"-knappen körs funktionen `setShowDetails(true)`. På så sätt vet React att datan i komponenten har ändrats. React kör sedan om hela komponentfunktionen från start till slut och den läser av `true` och returnerar JSX som visar till exempel produktens information eller råd som inte fanns vid förra renderingen.

React skapar sedan en **Virtual DOM** för att visa detta på skärmen. Man kan säga att det är som en kopia på sidan för att jämföra med hur den såg ut precis innan användaren klickade på "visa mer"-knappen. Man kan också kalla det för **diffing**. När den jämfört nya kopian mot den gamla så upptäcker den att det är likadant som innan förutom att nu har det tillkommit en ny t.ex. `<p>`-tagg under knappen med information. Och istället för att ladda om hela sidan så skjuter React bara in den nya `<p>`-taggen direkt i webbläsaren.

### 2. Vad är skillnaden mellan att hantera state lokalt med `useState` och globalt med Zustand? När passar vilket? ###

Om vi bara pratar om lokalt state med `useState` kan man säga att det är ett state som finns inuti en specifik komponent. Det skapas när komponenten visas på skärmen och raderas helt från minnet när komponenten försvinner. Eftersom datan är isolerad inuti just den komponenten kan det leda till så kallad "props drilling", om en komponent långt ner i applikationen också behöver tillgång till datan då måste man skicka props genom flera komponenter som inte ens är i behov av den datan, som jag skrev i frågan om Zustand.

Däremot om vi tittar på global state med Zustand då finns statet centralt i en "store". Och den finns hela tiden i applikationen oavsett vilka komponenter som visas eller döljs på skärmen. Det gör att vi löser "props drilling" situationen med att hämta ut eller uppdatera datan via detta store utan att blanda in props.  
**Lokalt (`useState`):** Till exempel när vi använder en "visa mer/visa mindre"-knapp. Då används data bara specifikt för den komponenten och det är egentligen bara den komponenten och dess barn som bryr sig om datan.  
**Globalt (Zustand):** Till exempel när en funktion används eller behövs på flera olika ställen eller data som måste finnas kvar även om man rör sig runt till olika sidor. Till exempel en kundvagn, om användaren har lagt in 2 produkter i kundvagnen och rör sig till andra sidor måste informationen finnas kvar.

### 3. Vad är "lifting state up" och varför behöver man ibland göra det? ###

I React kan data bara skickas nedåt. Alltså från förälder till barn, och det görs via props. Att "lyfta upp state" innebär att man flyttar ett state från en eller flera barn upp till den närmsta gemensamma förälder-komponenten.

Och det gör man för att man vill uppnå en källa till sanning alltså "Single Source of Truth".

Ett exempel skulle kunna vara om vi har en app där man kan boka biljetter. Om komponenten är att användaren ska välja antal biljetter med en plus- och minusknapp (`TicketCounter`). Och den andra komponenten är att den totala summan för de antal biljetter man valt (`PriceTotal`). Dessa två komponenter är syskon och de fåste få reda på hur många biljetter användaren har valt för att kunna räkna ut priset.  
**Lösningen:** `PriceTotal`kan inte se hur många biljetter som valts om vi har statet `count` inuti `TicketCounter` utan vi måste lyfta upp statet till deras gemensamma förälder t.ex. `BookingPage`. Föräldern skickar sedan ner värdet `count` till `PriceTotal` (så att den kan räkna ut priset) och `count` samt funktionen `setCount` till `TicketCounter` (så att knapparna kan uppdatera statet).

### 4. Förklara dependency array i `useEffect`. Vad händer om den är tom, har värden eller saknas helt? ###

Komponenter får in data och får ut JSX, det är så de fungerar. Men ibland behöver man göra saker utanför det till exempel hämta data från ett API. En dependency array i slutet på en `useEffect` kan man säga fungerar som en dörrvakt. Den talar om för React när och om koden inuti ska köras igen efter komponenten har renderats.  
**Om arrayen saknas helt:** Då körs koden efter den första renderingen och sedan efter varje re-render som komponenten gör. Det kan göra att hela sidan kraschar eftersom det blir en oändlig loop. Om man ska göra en felsökning kan det vara bra att inte ta med arrayen.  
```jsx
useEffect (() => {
    console.log("Jag körs hela tiden");
});
```  
**Om arrayen är helt tom:** Då körs koden bara en gång precis när komponenten visas för första gången. Oavsett hur många gånger komponenten renderas om. Här skulle man kunna ha den tom om man vill hämta data från ett API.  
```jsx
useEffect(() => {
    console.log("Jag körs bara en gång");
}, []);
```  
**Om arrayen har värden:** Koden körs vid första renderingen och sedan håller React koll på variablen inne i arrayen. Kommer bara köras ifall något av värdena i arrayen har förändrats sedan förra renderingen. Kan användas när man har en sökruta och vill göra ett nytt API-anrop varje gång användaren ändrar sitt sökord.  
```jsx
useEffect(() => {
    console.log("Jag körs när searchTerm ändras");
}, [searchTerm]);
```

---

## Del 2: Reflektion (Godkänt)

Minst 300 ord

### - Vad byggde ni för applikation och varför valde ni det temat? ###

Vi byggde en modern recept-applikation med fokus på användarvänlighet, struktur och personlig datahantering. Receptdatan hämtas från TheMealDB API. Applikationen innehåller funktioner som kategoribaserad sökning, ett interaktivt betygssystem för recept samt en personlig profilsida där användaren har full kontroll över sina egna skapelser.

Vi valde att bygga en recept-app eftersom vi såg det som en bra teknisk utmaning men samtidigt gav det oss rätt förutsättningar för att uppfylla samtliga kriterier för "Väl Godkänt". Vi ville också skapa ett verktyg som inte bara används för att hitta inspiration men också kunna skapa sina egna recept och lagra dem på samma ställe man letar efter inspiration.

### - Hur delade ni upp arbetet i gruppen? ###

Ivana ordnade snabbt en prototyp i Figma som gav oss en bild om vad vi ville ha och inte ha. Sedan skapade vi en kanban-tavla med User Stories och vi försökte skapa dem så tydliga och konkreta som möjligt för att ha en gemensam och tydlig målbild.

Eftersom vissa sidor och funktioner var större än andra, valde vi att hålla ihop ansvar per funktion. Det innebar att den som ansvarade för logiken på en viss sida till exempel "About" eller profilsidan, även tog fullt ansvar för att skriva CSS:en för den sidan.

För vissa komplexa delar som autentiseringen, våra ProtectedRoutes och hantering av global state i store samarbetade vi eller stämde av i gruppen om någon specifik person skulle jobba med dem. Vi använde Pull Requests på GitHub för att granska varandras kod innan den slogs ihop med huvudbranchen, vilket gjorde att alla fick en bra insyn i projektets helhet.

### - Vilka React-koncept använde ni i projektet? Ge konkreta exempel. ###

I projektet har vi använt flera av Reacts koncept för att bygga en dynamisk, säker och modulär applikation.  
**Exempel 1 (Komponentbaserad arkitektur):** Vår `RecipeCard`. Denna komponent används på flera ställen i appen (både på söksidan och på profilsidan). Genom att skicka olika data till samma kort slipper vi duplicera kod, vilket gör applikationen lättskött.  
**Exempel 2 (Lokal tillståndshantering):** Vi använder Hooks för att hantera lokal data som förändras baserat på användarens val. Till exempel i formulären på `LoginPage` använder vi `useState` för att fånga upp vad användaren skriver i textfälten innan data skickas vidare.  
**Exempel 3 (Global State Management):** Vi skapade en global Store (`useAuthStore.js`) som håller reda på om en användare är inloggad samt lagrar användarens egna recept. Detta gör att både `ProtectedRoute` och profilsidan kan läsa av samma information utan att vi behöver skicka data manuellt genom flera komponenter.

### - Vad var svårast och hur löste ni det? ###

Under projektets gång stötte vi på främst två saker som handlade om hur vi hanterade data från vårt API (TheMealDB). Det var lite klurigt.  
**API-anrop:** Från början gjordes ett separat API-anrop för varje enskilt receptkort (`RecipeCard`) som skulle visas på skärmen. Detta resulterade i väldigt stort antal requests till API:et, vilket gjorde att applikationens prestanda sjönk.

Vi löste det genom att ändra hur vi hämtade datan. Istället för att göra anrop per kort, gjorde vi ett större anrop och hämtade 15 recept åt gången till vår store, vilket minskade antalet requests och gjorde appen snabbare.  
**Begränsningar i API-datan:** Vi märkte att "TheMealDB" saknade en del information som vi tyckte var viktig att visa för användaren som till exempel portioner eller svårighetsgrad.

För att visa svårighetsgrad byggde vi en funktion som automatiskt beräknade om receptet var lätt eller svårt baserat på hur många ingredienser det innehöll. Och när det gällde antal portioner, där API:et helt saknade data, valde vi att hårdkoda ett standardvärde för att hålla designen och strukturen likadan på alla sidor.

----------------------------------------------

## Reflektion (Väl Godkänt)

Minst 500 ord

### - Om du fick börja om från scratch, vad hade du gjort annorlunda? Varför? ###

Om vi hade börjat om projektet från grunden idag så hade jag velat göra en sak annorlunda. Och det är att lägga mer tid på att analysera och testa det externa API:et (TheMealDB) ännu mer och kanske gärna tillsammans. Jag kan inte säga att vi upptäckte för sent att data som portioner och svårighetsgrad saknades men man tvingades att lägga tid på att bygga lösningar och även hårdkoda värden. Om vi hade gjort en ordentlig analys och testat det externa API:et direkt i startfasen hade vi kunnat välja antingen ett annat API, eller planerat vår struktur bättre för att slippa "workarounds". Så ur ett utbildningsperspektiv var det bra att det hände för då fick man lära sig mer så klart. Men jag som person gillar att ha koll på läget och veta vad jag har att jobba med.

### - Hur hanterade ni state i applikationen? Var valde ni lokal state vs Zustand och varför? ###

Vi valde att använda `useState` för allt som var isolerat till en specifik komponent och som inte påverkade resten av appen. Ett konkret exempel är våra formulär, så som `LoginPage` eller när en användare skapar ett eget recept på sin profil. Användarens inmatning i textfälten sparas lokalt i komponenten under tiden som de skriver.

Vi valde Zustand för data som behövde delas mellan helt olika delar av appen, och där vi ville undvika så kallad "prop-drilling". Ett exempel på detta är vår autentisering och hantering av inloggade användare. Den informationen behövs i navigeringsfältet, i våra `ProtectedRoutes` och på profilsidan (för att visa vems recept som ska visas). Vi använde även global state för att lagra de 15 recept vi hämtade från API:et, så att vi slipper göra stora anrop varje gång användaren backar eller bläddrar i applikationen.

### - Vad lärde du dig av peer review-sessionen? Var det något i en annan grupps kod som du tyckte var bra eller som du hade gjort annorlunda? ###

Att delta i peer review-sessionen var en väldigt lärorik del av denna kurs samt av detta projekt. Det största värdet med att granska en annan grupps kod är att man tvingas läsa och förstå kod som man inte har skrivit själv. Det gav en bra insikt i att det nästan alltid finns flera olika tekniska lösningar på exakt samma problem i React, och att det inte alltid finns ett enskilt "rätt" svar.

En viktig sak jag tog med mig från sessionen var vikten av ren och väldokumenterad kod. När man kliver in i ett helt främmande projekt blir det väldigt tydligt hur avgörande det är med tydliga komponentnamn, en logisk mappstruktur och korta kommentarer som förklarar logiken. Även att den som gör själva reviewen är tydlig och konkret i sin granskning. 

När vi granskade den andra gruppens applikation fanns det särskilt en sak som jag tyckte att de löst på ett väldigt bra sätt. I filen `ContactForm.jsx` hade de placerat objektet `emptyForm` högst upp, helt utanför själva funktionen. Det tyckte jag var en smart och bra lösning eftersom det gör koden mycket mer lättläst för den som granskar den. Dessutom är det väldigt bra för applikationens prestanda, då objekt inte behöver skapas på nytt i minnet varje gång komponenten renderas om när användaren skriver i ett fält.

Om det var något jag däremot kände att jag hade velat göra annorlunda i deras kod, så var det just den filen `ContactForm.jsx`. Med 156 rader kod är den ju inte så stor men det var mycket som hände på en gång. Om det varit mitt projekt hade jag kanske velat bryta ut `validate()`-funktionen till en helt extern fil för att separera valideringslogiken från själva UI. Jag hade nog även velat skapa en egen underkomponent för de input-fält som repeterades, bara för att rensa upp strukturen och göra koden mer modulär.
