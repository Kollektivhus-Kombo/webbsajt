# Projekt: Kollektivhuset Kombo (Webbsajt)

Detta projekt är en statisk webbplats för Kollektivhuset Kombo, byggd med HTML och Vanilla CSS.

## Systemkrav och Miljö
För att arbeta effektivt med detta projekt krävs följande verktyg:
- **Git:** Måste vara installerat på datorn.
- **GitHub CLI (gh):** Måste vara installerat och inloggat (`gh auth login`).
- **Figma MCP:** Måste vara ansluten och konfigurerad för att kunna läsa designkontext.
- **Homebrew:** Om något verktyg saknas, använd `brew` för att installera det (t.ex. `brew install gh git`).

## Designreferens (Figma)
All ny design och uppdateringar ska utgå från följande Figma-fil:
- **URL:** [https://www.figma.com/design/3xRtXbb4Ih6muH6Lba3PUK/KOMBO](https://www.figma.com/design/3xRtXbb4Ih6muH6Lba3PUK/KOMBO)
- **Huvudnod:** `0:1`

## Arbetssätt med Figma (MANDAT)

När du implementerar ändringar baserat på Figma-designen, följ dessa steg:

1.  **Läs designkontexten:** Använd `get_design_context` med relevant `nodeId` och `fileKey` (`3xRtXbb4Ih6muH6Lba3PUK`) för att få ut CSS-egenskaper, färger och struktur.
2.  **Färg- och designtokens:** Följ de CSS-variabler som definierats i Figma (t.ex. `--main-bg`, `--dark-red`). Om de saknas i koden, lägg till dem i `:root`-blocket i de berörda HTML-filerna eller i en global CSS-fil.
3.  **Responsiv design:** Designen i Figma är den primära källan. Sidorna ska alltid vara optimerade för både desktop och mobil, med fokus på att bibehålla sajtens unika karaktär på båda:
    - **Dekorativa linjer:** De vertikala linjerna (`.vertical-line`) ska tas bort på mobil för att skapa en renare layout.
    - **Kompakt layout:** Mobilvyn ska vara kompakt. Undvik onödiga tomrum genom att använda snäva marginaler, särskilt när rubriker följer på varandra eller när stycken staplas under rubriker.
    - **Separata anpassningar:** Vid behov, använd specifika media queries för att optimera layouten för respektive plattform så att de känns skräddarsydda.
4.  **Media och bilder:** Nya bilder från Figma ska exporteras och placeras i mappen `media/`. Referera till dem med relativa sökvägar (t.ex. `media/bildnamn.png`).
5.  **Kodstil:**
    - Använd **Vanilla CSS**. Undvik Tailwind eller externa ramverk om de inte redan används.
    - Håll HTML-strukturen ren och semantisk.
    - Var observant på att vissa sidor har interna `<style>`-block medan andra använder filer i `stylesheets/`. Prioritera konsekvens med den fil du redigerar.

## Kritiska kontroller: Medlemsformulär (bli-medlem.html)
Sidan `bli-medlem.html` innehåller ett formulär som är kopplat till ett Google Apps Script för att uppdatera ett Google Sheet. Det är **extremt viktigt** att denna koppling inte bryts vid designuppdateringar.

### Obligatoriska verifieringar vid ändringar på bli-medlem.html:
- **Formulär-ID:** Ändra ALDRIG id på `<form id="medlem-form">` utan att också uppdatera JavaScript-koden.
- **Input-ID:n:** Följande ID:n används av skriptet och måste bibehållas: `namn`, `mail`, `fodelsedatum`, `godkanner`.
- **Google Script URL:** URL:en `https://script.google.com/macros/s/.../exec` i `fetch`-anropet får inte röras.
- **Parametrar:** Skriptet förväntar sig parametrarna `email`, `name`, `birth` och `gdpr`. Dessa mappas från formulärets fält. Verifiera att mappningen i `fetch`-anropet är korrekt.
- **Success-feedback:** Popup-logiken (`id="popup"`) måste fungera så att användaren får bekräftelse efter inskickat formulär.
- **Validering:** Innan du committar ändringar på denna sida, verifiera att JavaScript-koden inte har några syntaxfel som hindrar `submit`-eventet.

### Obligatoriska verifieringar vid ändringar på stadgar.html:
- **Legal text:** Texterna i stadgarna är juridiskt bindande. Vid designuppdateringar får INGA ord eller formuleringar i själva stadgetexten (§ 1 till § 11) ändras, läggas till eller tas bort. Verifiera alltid mot tidigare version (t.ex. via `git diff`) att endast HTML-taggar och CSS-klasser har ändrats, inte innehållet i paragraferna.

## Projektstruktur
- `index.html`, `medlem.html`, etc. – Sidornas HTML-innehåll.
- `stylesheets/` – Externa CSS-filer (t.ex. `navigation.css`, `globals.css`).
- `media/` – Bilder, logotyper och PDF-dokument.

## Validering
Efter varje ändring:
1. Kontrollera att den visuella troheten matchar Figma-skärmbilderna (använd `get_screenshot` om nödvändigt för jämförelse).
2. Säkerställ att navigeringen mellan sidorna fortfarande fungerar korrekt.
