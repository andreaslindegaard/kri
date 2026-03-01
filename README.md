# Opgivelsesskema til Kristendomskundskab

Et interaktivt, browser-baseret værktøj til lærere i folkeskolen, der skal udarbejde og indlevere opgivelser til den mundtlige (udtræks)prøve i faget Kristendomskundskab.

Værktøjet er bygget som en **Single Page Application (SPA)** eksklusivt med standard web-teknologier, hvilket betyder at det kan afvikles direkte i en hvilken som helst moderne browser, offline eller som statisk fil hosting. Ingen backend eller datasamling pågår; alt data holdes 100% lokalt hos brugeren af hensyn til GDPR og privatliv på elev-niveau.

## Nøglefunktioner

- **Krav-Validering:** Systemet holder automatisk øje med, om ministeriets minimumskrav til opgivelser (4 emner, 5 udtryksformer, 4 kulturteknikker, 4 kompetenceområder) er opfyldt, og præsenterer læreren for en progress bar, der skifter til grøn, når alt er godkendt.
- **Lokal Autosave:** Skemaet gemmer automatisk løbende ("kladde") i browserens `localStorage`, så læreren ikke mister data ved uheldig fane-lukning. Derudover eksisterer der en "beforeunload"-advarsel, hvis browseren alligevel lukkes midt i at der skrives.
- **Fil-baseret kladde system:** En lærer kan gemme hele sit system-state lokalt som en downloadbar `.json`-fil, og genåbne projektet på et senere tidspunkt, hvilket gør at ét værktøj kan bruges til flere klasser (og de næste årgange på sigt).
- **Flere Eksportmuligheder:**
  - **Native Browser Print (PDF):** Ren kodet CSS Layout til print, ekskluderer knapper/værktøjs-ikoner. Modaler validerer opgivelses-mangler, inden print trigger. Browserens titel på fanen muteres kortvarigt for at servere en læsbar print-filnavn på PDF-visningen.
  - **Eksport til Microsoft Word (.docx):** Klonskaber et snapshot af den printbare tabel og pakker den som en Word-kompatibel HTML base-fil der åbnes igennem Office-pakken.
  - **Kopier til Google Docs:** Systemet kopierer automatisk den korrekte tabel-opsætning usynligt i baggrunden ned i styresystemets udklipsholder og åbner derefter automatisk et tomt Google Docs-dokument hvori læreren kun behøves trykke Sæt Ind (Ctrl+V).
- **In-place Tabelredigering:** Tidligere tilføjede rækker/emner, kan live redigeres inde i selve skemaets struktur (`contenteditable=true`).
- **One-click Eksempelvisning:** Nyt layout til nemmere at lade fremtidige eller nye censorer/undervisere se, hvordan et gyldigt forløb skal se ud uden permanent at skade deres data. Skemaet sikrer den midlertidige data lokalt gennem skjult serialize-storage og genfremkalder det ved endt eksempelvisning.
- **Kulturteknikker Logik:** Tags registreres som faglige ord ned i en intelligent array-sorterings motor. Programmet har et indbygget ordbogs-tjek på Kulturteknikker, som samler synonymer (så "Podcast", "Lyd", og "Voxpop" anerkendes under éns paraply) så indtastninger har rum til fejltagelser når der opregnes minimums-teknikker opfyldt.

## Teknisk Stack

- **HTML5:** Strukturering af dokument og input-felter. Web Storage API til offline hukommelse.
- **JavaScript (Vanilla/ES6):** Sørger for DOM manipulering, data håndtering (JSON serialize/deserialize), begreb-tag parsing og validerings-scripts. 
- **Tailwind CSS (via CDN):** Alt design generes igennem Utility first Tailwind CSS som muliggør et ekstremt modult og pænt layout uafhængig af bundlet CSS. Print mediatyper til at rydde UI (`print:hidden`).
- **Ikoner:** Bruger SVG-ikoner (oftest Heroicons/SVGrepo typografien) inline til knapper m.m. for nul load-delay.

## Hvordan det køres (Local/Production)

Da applikationen udelukkende er et `index.html`-dokument, kræves der minimal opsætning:

### Metode 1: Dobbeltklik (Lettest)
1. Pak mappen ud (hvis zipped).
2. Dobbeltklik på filen: `index.html`. 
3. Din standardbrowser åbner den øjeblikkeligt (virker i Edge, Chrome, Safari, Firefox OSV).

### Metode 2: Lokal statisk HTTP-Server (Til dev)
Brug hvilken som helst statisk server. F.eks, hvis NodeJS (npm) er installeret, kan du skrive:
```bash
npx serve -l 8000
```
Navigere til `http://localhost:8000` i din browser. Dette tillader live-reload med visse plugins.

### Metode 3: Github Pages / Webhoteller (Live)
For at udstille værktøjet til et lærerkollegium, uploades `index.html` direkte til roden af jeres domæne-udbyder via FTP. Filen benytter relative CDN'er (Tailwind), hvorfor ingen undermapper eller afhængigheder medfølger / kræves. 

## Struktur over dataformater
Hver "Række" / Hold opsummeres bag kulisserne i et `state` format af datatypen JSON, indholdende undergrupperne "stamdata", "rows" (holdets emner/tekster) samt arrayet "kulturteknikker". Disse hentes live hver gang der tastes ("oninput" eller blur evt.) igennem browser-events. 
```json
{
  "version": 1,
  "stamdata": { ... },
  "rows": [ { ... } ],
  "kulturteknikker": [ ... ]
}
```

## Vedligehold / Kontakt
Tilføjelser til faget som eksempelvis "nye faglige kompetenceområder" bedes udelukkende tilrettes statisk i de udleverede dropdown/hardkodede arrays omkring linje 400 (Input Form dropdowns/Suggestions list). Resten tegnes dynamisk.
