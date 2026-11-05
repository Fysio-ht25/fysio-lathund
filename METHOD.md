# Metod — så byggdes och underhålls Fysio lathund

Arbetsanteckning till mig själv och till framtida sessioner. Syftet är att inte behöva
uppfinna arbetsgången på nytt varje gång lathunden ska byggas ut till en ny kurs eller termin.
Läs den här filen **innan** arbete påbörjas i det här arkivet.

---

## 1. Grundhållningen

Lathunden ska fungera för någon som **inte** redan kan materialet. Det är hela skillnaden mot
förlagan (Stefan Eks översiktsblad från T5), som listar testnamn utan utförande och därför
bara fungerar som minnesstöd för den som redan kan testet.

**Ribban för ett testkort:** utgångsläge, utförande, positivt fynd, tolkning och nästa steg.
Saknas något av dessa är posten en påminnelselapp, inte en lathundspost. Ta bort den eller
skriv klart den.

Innehållet är på svenska, konversationen om arbetet på engelska (enligt `CLAUDE.md` i
`D:\Fysio\Fysioterapi\`).

---

## 2. Arkitektur — och varför

```
source/*.md  →  build.py       →  index.html          (en fristående fil, base64-bilder)
             →  build_epub.py  →  fysio_lathund.epub  (XHTML-kapitel, bilder som filer)
```

**Varför en generator i stället för handskriven HTML:** dokumentet är ett levande material som
växer med varje kurs. En handredigerad HTML-fil på 3,7 MB går inte att underhålla. Källan är
markdown-lik och läsbar; HTML-filen är en artefakt och redigeras **aldrig** för hand.

**Varför en egen parser i stället för ett markdown-bibliotek:** innehållet behöver
domänspecifika block som inget bibliotek har — testkort, flödesscheman, färgkodade rutor,
videochips. Parsern är ~250 rader och har noll beroenden, vilket gör att bygget fungerar på
vilken dator som helst med Python 3.

Formatet dokumenteras i [`source/_README.md`](source/_README.md). Håll den filen uppdaterad
när syntaxen ändras — den är kontraktet mellan källa och bygge.

---

## 3. Arbetsgång för nytt innehåll

1. **Inventera källmaterialet först.** Kursmaterialet ligger som `.md` efter OCR i
   `D:\Fysio\Medicin\` respektive som Google Docs-export i `D:\Fysio\Fysioterapi\`.
   Den stora exporten innehåller base64-bilder — strippa dem och dela upp filen per rubrik
   innan den läses, annars blir den ohanterlig (89 MB → 795 kB).
2. **Leta efter luckor mot kursens innehåll**, inte mot en egen föreställning om vad som
   borde ingå. Neuromodulen tillkom först när en sökning visade att vibration, proprioception,
   temperatur och smärtsinne hade noll träffar i källan trots att somatosensorik är ett helt
   kompendium i Fysioterapi 2.
3. **Skriv modulen** enligt formatet. Ett flöde per verklig gaffel i resonemanget — inte som
   dekoration av en lista som redan finns i texten.
4. **Bygg båda formaten** och kontrollera siffrorna i utskriften (moduler, sektioner, flöden,
   bilder, videolänkar). En siffra som sjunker betyder att något slutat matcha.
5. **Granska visuellt i webbläsare** på både bred och smal skärm. Strukturell validering
   fångar inte layoutfel.

---

## 4. Faktaregler

- **Diagnostisk träffsäkerhet anges bara med namngiven källa.** Aldrig ur minnet. Systematiska
  översikter och metaanalyser går före enskilda studier. Saknas underlag ska det stå i texten.
- **Motsäg kursmaterialet när det har fel** — men skriv ut att det avviker och varför.
  Hittills rättade fel: fotledens plantar-/dorsalflexion var förväxlade i Google Docs-tabellen,
  höftflexion mäts med böjt knä (Magee s. 779), och Patientjournallagen (1985:562) är ersatt av
  patientdatalagen (2008:355) med tio års bevarandetid, inte tre.
- **Källorna är kursmaterialet, Petty, Magee och Neumann**, plus namngivna metaanalyser,
  FYSS, ACSM och patientdatalagen. Hela lathunden källgranskades 2026-10-14–15: varje
  påstående har en sidhänvisning eller är struket. Kursmaterialet går först vid utförande;
  litteraturen kompletterar. Lathunden citerar aldrig sig själv.
- **Sidhänvisningarna till Petty gäller 5:e upplagan (2018).** Kursen använder 6:e (2024).
  Byt sidnummer om 6:e upplagan blir tillgänglig.
- **Döp inte om ett testkort utan att uppdatera `_video.tsv`** — länken matchas på exakt rubrik.
  Byggutskriftens antal videolänkar visar om något tappats.
- **Tabellen över träffsäkerhet i `90_referens.md` ska hållas synkad** med testkorten. För in
  värdet på båda ställena samtidigt.

---

## 5. Bilder — licensarbetsgången

Alla figurer måste vara fritt licensierade, eftersom materialet publiceras offentligt.

**Gemensam bildbank:** `D:\Fysio\Fysioterapi\images\` med `catalog.json` (verktyg: `~/.claude/skills/revision-pipeline/scripts/imagebank.py`). Sök där först (`find`); webbsökningen nedan görs bara vid miss, och nya bilder läggs in via `imagebank.py add`, som hämtar och sparar licensen. Lathundens 20 befintliga bilder är registrerade i banken (licens enligt tabellen Bildkällor; exakt Commons-fil ej dokumenterad).

1. **Sök på Wikimedia Commons via API**, inte via fritextsökning i webbgränssnittet.
   Fritextsökning ger brus (sökningen på "Gray shoulder joint" returnerade uniformsinsignier
   från andra världskriget). Använd i stället:
   - kategorilistning: `list=categorymembers` — OpenStax-figurerna ligger i kategorier som
     `Human muscular system from CNX Anatomy & Physiology Textbook`
   - titelsökning: `srsearch=intitle:"Bones of the Foot"`
   - direkt titeltest på gissade filnamn: `titles=File:802 Pectoral Girdle.jpg`
2. **Hämta licensen programmatiskt** med `prop=imageinfo&iiprop=extmetadata` och läs
   `LicenseShortName` och `Artist`. Gissa aldrig licensen.
3. **Titta på varje bild innan den läggs in.** Två figurer förkastades efter granskning:
   en fotskelettbild som bara hade bokstavsnycklar A–G utan teckenförklaring, och en
   dermatomkarta från kursmaterialet som visade sig vara upphovsrättsskyddad.
4. **Normalisera**: max 880 px bredd, JPEG kvalitet ~84, vit bakgrund bakom transparens.
   SVG-filer på Commons kan hämtas färdigrenderade som PNG via `iiurlwidth` — bästa kvaliteten
   för diagram.
5. **Attribuera i bildtexten och i tabellen `Bildkällor`** i `90_referens.md`. Anges ändring
   (till exempel beskärning) ska det stå — CC kräver det.

**Bästa källorna hittills:** OpenStax *Anatomy and Physiology* (CC BY, konsekvent stil, hela
boken på openstax.org, figurnumren står i filnamnen på Commons) och Mikael Häggströms
public domain-figurer.

---

## 6. Videolänkar — verifieringsregeln

`source/_video.tsv` mappar testnamn till URL. Namnet måste stämma **exakt** med rubriken efter
`####`.

**Verifiera alltid varje länk mot YouTubes oEmbed-endpoint** och läs tillbaka titel och kanal:

```
https://www.youtube.com/oembed?url=https://www.youtube.com/watch?v=<id>&format=json
```

Detta är inte formalia. Verifieringen har fångat:
- fem länkar i Google Docs-källan som alla pekade på samma ULTT-video (klipp- och klistrafel),
- tre förslag på "collateral ligament test" som i själva verket visade tummens UCL-test,
- ett förslag på infraspinatustest som visade ett SLAP-test.

**Fuzzy-matchning av testnamn mot en URL-lista fungerar inte** — den gav Spurling → "grind-test"
och Obers test → "neer-test". Använd nyckelordsmatchning och kontrollera titeln.

Hellre inget chip än fel chip.

---

## 7. Layoutbeslut som redan är avgjorda

Ändra inte de här utan skäl — de är resultatet av iterationer:

| Beslut | Varför |
| ----- | ----- |
| Ett navigeringssystem, inte två | Piller i menyraden och regionchips gjorde samma sak och konkurrerade |
| Hamburgermeny med grupperade flikar (2026-11-05) | 14 flikar fick inte plats som textflikar. Knappen visar aktuell flik; panelen grupperar (Undersökning, Regioner, Klinik, Referens). Nya moduler läggs i `MENU_GROUPS` i `build.py`, annars hamnar de under Övrigt |
| Flödesscheman som SVG, ryggrad med utgångar | CSS-boxar med nedåtpil antydde att alla grenar fortsatte nedåt, vilket var fel |
| Utgångar grenar åt höger och slutar där | En gren är ett utfall, inte ett steg i kedjan |
| Tabeller med 2 kolumner radbryter, 3+ staplas | ~60 px per kolumn går inte att läsa; staplade rader med `data-label` gör det |
| Kolumnbredder räknas fram ur innehållet | 50/50 är logiskt men fel när vänsterkolumnen har ettordsetiketter och högerkolumnen hela meningar |
| Avstavning i stället för godtycklig ordbrytning | `overflow-wrap:anywhere` klippte *Propri-ocepti-on*; `break-word` + `hyphens:auto` med `lang="sv"` ger korrekta avstavningar |
| Flöden panoreras i sidled på mobil | Ett diagram nedskalat till 390 px får 5 px text |
| Modulikoner som en gemensam figur | En streckgubbe med markerad region blir ett system, inte elva orelaterade piktogram |
| Samma stil som Fysioterapi 3-sajten (2026-09-26) | Redaktionellt: raka hörn, inga skuggor eller piller, struktur genom typografi och linjer. KI-plommon + DM Sans, KI:s webbaccent `#FF876F` för länkar och markeringar |
| Mörkt tema som standard, ljust som val | Valet sparas i `localStorage` (`lathund.tema`), sätts i `<head>` före rendering så att sidan inte blinkar. Utskrift är alltid ljus |
| Fliken Tester genereras ur testkorten (2026-10-23) | Korten står kvar i sina regioner; fliken samlar dem per `typ=` (nerv, led, muskel, specifik) med När-raden och en länk tillbaka. Ingen egen källfil, så fliken kan inte glida isär från korten. Den byggs bara i `build.py` (inte i EPUB) och döljs vid utskrift, där korten redan finns i regionerna |
| När: och Alias: på testkorten (2026-10-23) | När-raden säger när testet väljs — det som lathunden saknade jämfört med testöversikten i Fysioterapi 3. Alias ger andra namn, främst engelska, så att testet går att söka upp i litteraturen |
| Flödespilarna har klassen `fs-arr` | CSS styr färgen i webbversionen (följer temat); EPUB saknar den CSS:en och använder `stroke`-attributet. EPUB:ens egen stil är oförändrad |

**Kolumnbredder:** `Builder.col_widths()` viktar varje kolumn efter typisk och längsta cellängd, med golv från rubrikens längd och från kolumnens längsta ord (annars måste ordet brytas mitt itu på smal skärm). Procenten summerar till 100 och skrivs som `<colgroup>`, och tabellen är `table-layout:fixed` så bredderna följs exakt.

**Textmått i SVG:** det finns inga fontmetrics vid byggtid, så bredden uppskattas per tecken.
Uppskattningen ska **överskatta** — en för trång ruta får text att spilla ut, vilket är värre
än en något rymlig ruta. Marginalerna ligger i `_tw()` och `_wrap()` i `build.py`.

---

## 8. Kontroller före leverans

```bash
python build.py && python build_epub.py
```

- Siffrorna i byggutskriften stämmer med förväntan.
- Inga `@flow`, `@fig`, `@meta` eller `!!!` kvar i utdata (skulle betyda att ett block inte tolkats).
- **EPUB:** varje XHTML-fil är välformad XML, `mimetype` är första posten och okomprimerad,
  manifest och spine kompletta, alla bildreferenser upplösta.
- **Mobil:** `document.documentElement.scrollWidth === innerWidth` vid 390 px med alla
  sektioner utfällda. Inga element bredare än viewporten utanför `.fwrap`.
- Inline SVG måste ha `xmlns="http://www.w3.org/2000/svg"` — frivilligt i HTML5, obligatoriskt
  i EPUB. Utan det renderas flödesschemat som en textklump.

---

## 9. Kända fallgropar

- `innerText` hittar inte text i hopfällda sektioner. Sökfunktionen måste använda `textContent`.
- `html.escape()` körs före de inline-reglerna, så pilen matchas som `-&gt;`, inte `->`.
- Windows-konsolen är cp1252 och kraschar på emoji och vissa tecken vid `print()`. Filen skrivs
  ändå — kontrollera resultatet i stället för att anta att skrivningen misslyckades.
- Filer i `source/` som börjar med `_` byggs inte. Exempelkoden i `_README.md` ska därför inte
  räknas när testkort räknas (det gav en felaktig siffra på 49 i stället för 48).
- Regex-ersättningar med `\1` i patchskript blir lätt `chr(1)` om strängen inte är rå.

---

## 10. Nästa gång kursmaterialet växer

Fysioterapi 3 och 4 samt Medicinska ämnen har ännu inte arbetats in. Arbetsgången är densamma:
inventera källan, sök luckor, skriv modul, bygg, granska. Lägg modulen med ett sifferprefix som
placerar den rätt i flikordningen och uppdatera `README.md` med modulnamnet.
