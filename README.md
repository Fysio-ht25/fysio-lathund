[README.md](https://github.com/user-attachments/files/32240398/README.md)
# Fysio lathund

En snabbreferens för den fysioterapeutiska undersökningen: vad som testas, hur det utförs,
vad fyndet betyder och vad som följer av det. Byggd för fysioterapeutprogrammet vid
Karolinska Institutet, användbar på telefon i undersökningsrummet och utskriven som täta ark.

**👉 [Öppna lathunden](https://fysio-ht25.github.io/fysio-lathund/)**

---

## Vad den innehåller

Elva moduler: fysioterapiprocessen, röda flaggor, sex kroppsregioner, journalföring och
referensdata.

- **49 testkort** med utgångsläge, utförande, positivt fynd, tolkning, nästa steg och —
  där det finns underlag — sensitivitet och specificitet med källa.
- **48 videolänkar** till demonstration av testerna, mestadels Physiotutors.
- **9 flödesscheman** vid de ställen där undersökningen verkligen förgrenar sig:
  akut knätrauma, Ottawa-reglerna vid fotledstrauma, trippelsortering vid ländryggssmärta,
  nervrot kontra perifer nerv, triagering av röda flaggor med flera.
- **18 anatomifigurer**, skelett och muskulatur för varje region samt dermatom, plexus
  brachialis och plexus lumbalis.
- **Referensdata**: rörlighetsnormalvärden, myotom/dermatom/reflex C5–S1, MRC-skalan,
  end-feel, loose-packed positioner, Beighton, balanstestens riktvärden och dosering.

Sök med fältet högst upp — sökningen träffar även innehåll i hopfällda avsnitt. `/` hoppar
till sökrutan. Ctrl+P skriver ut i två spalter.

---

## Viktigt att veta

Lathunden är **studentproducerat studiematerial**, inte ett granskat kliniskt dokument och
inte kurslitteratur. Den är sammanställd från kursmaterial vid KI kompletterat med
systematiska översikter och metaanalyser, men den har inte granskats av lärare eller
legitimerad personal.

Använd den som minnesstöd och för att repetera — inte som beslutsunderlag i patientarbete.
Vid konflikt mellan lathunden och kurslitteraturen gäller kurslitteraturen.

Diagnostiska träffsäkerhetsvärden anges bara när de går att belägga med en namngiven källa.
Där underlaget saknas eller är svagt står det uttryckligen i texten.

**Hittat ett fel?** Öppna en issue här på GitHub, eller skicka en pull request.

---

## Bygga om lathunden

`index.html` genereras från källfilerna. Redigera aldrig HTML-filen direkt — den skrivs över
vid nästa bygge.

```bash
python build.py
```

Inga beroenden utöver Python 3. Skriptet läser `source/*.md`, bäddar in bilderna från
`images/` som base64 och skriver en enda fristående HTML-fil.

### Struktur

```
index.html              publicerad, byggd fil
build.py                bygger HTML från källan
source/
  00_process.md         fysioterapiprocessen och beslutslogiken
  05_flaggor.md         röda, gula, orange och blå flaggor
  20_nacke.md … 70_fot.md   regionmoduler
  80_journal.md         journalföring
  90_referens.md        referenstabeller och bildkällor
  _video.tsv            testnamn → videolänk
  _README.md            källformatet: testkort, flöden, rutor, figurer
images/                 anatomifigurer
LICENSE                 CC BY-SA 4.0
```

### Lägga till innehåll

Källformatet beskrivs i [`source/_README.md`](source/_README.md): testkort, flödesscheman,
färgade rutor, tabeller och figurer. Kortversionen:

```
#### Testets namn | struktur=ACL
- UL: utgångsläge
- Gör: utförande
- Pos: positivt fynd
- Tolk: vad fyndet betyder
- Nästa: vad som följer
- Evidens: sens/spec med källa
```

Ett test utan utgångsläge, utförande, positivt fynd och tolkning är en påminnelselapp, inte
en lathundspost — det är ribban för nytt innehåll.

---

## Licens och bilder

Innehållet i lathunden licensieras under
**[CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/deed.sv)** — använd, ändra
och sprid fritt, men ange upphov och behåll samma licens på det du delar vidare.

Samtliga figurer är fritt licensierade och hämtade via Wikimedia Commons:

| Figurer | Upphov | Licens |
| ----- | ----- | ----- |
| Skelett: kotpelaren, halsryggen, skuldergördeln, handleden, femur, foten, knäleden | OpenStax, *Anatomy and Physiology* | CC BY 3.0 |
| Muskulatur: nacke, skuldra, underarm, hand, rygg, höft/lår, underben | OpenStax | CC BY 3.0 / CC BY 4.0 |
| Plexus brachialis | Selket m.fl., derivativt verk | CC BY-SA 3.0 |
| Plexus lumbalis | Derivativt verk efter Gray (1918) | CC BY 3.0 |
| Dermatom och perifera hudnerver | Mikael Häggström, efter Gray (1918) | Public domain |

Fullständig förteckning finns under **Referensdata → Bildkällor** i lathunden. Attributionen
följer med i HTML-filen, så den kan spridas vidare som den är.

Videolänkarna pekar på material hos respektive upphovsperson, huvudsakligen Physiotutors.
Inget videoinnehåll lagras i det här arkivet.

---

## Källor

Kursmaterial från fysioterapeutprogrammet vid KI (Fysioterapi 1 och 2), samt bland annat:

- Downie A, et al. Red flags to screen for malignancy and fracture in patients with low back pain. *BMJ* 2013;347:f7095.
- Hegedus EJ, et al. Which physical examination tests provide clinicians with the most value when examining the shoulder? *Br J Sports Med* 2012;46:964–978.
- Smith BE, et al. Special tests for assessing meniscal tears within the knee: a systematic review and meta-analysis. 2015.
- Sokal PA, et al. The diagnostic accuracy of clinical tests for anterior cruciate ligament tears. *KSSTA* 2022.
- Majlesi J, et al. The sensitivity and specificity of the Slump and the Straight Leg Raising tests. *J Clin Rheumatol* 2008;14:87–91.
- Wainner RS, et al. Reliability and diagnostic accuracy of the clinical examination for cervical radiculopathy. *Spine* 2003.
- Stiell IG, et al. Ottawa ankle rules och Ottawa knee rules, med efterföljande metaanalyser.
- Patientdatalagen (2008:355) samt HSLF-FS 2016:40.
