# Praktisk Overleaf- og LaTeX-opsætning til et økonomispeciale

## Standardopsætningen kort fortalt

Min klare standardanbefaling til dit speciale er denne: brug **ét Overleaf-projekt pr. output-PDF**, hold **`main.tex` i roden af projektet**, vælg **`report`** som standardklasse, del teksten op i separate kapitel-filer, hold **`preamble.tex`** og **`commands.tex`** adskilt, styr litteraturen fra **én `.bib`-fil**, og lad **Python/Stata skrive figurer, tabeller og logs direkte til output-mapper**, som LaTeX derefter inkluderer uden manuel copy-paste. Det følger Overleafs anbefalinger for store projekter, for placeringen af hoveddokumentet og for at undgå flere konkurrerende main-dokumenter i samme projekt, og det ligger tæt på den reproducerbare workflow, som Stata selv fremhæver. citeturn12view6turn8view2turn18view3turn18view1

Til en **økonomi-/empirical finance-/macro-finance-thesis**, hvor du sandsynligvis vil genbruge noget af materialet i et paper senere, er den mest robuste standard normalt: **`report` + `natbib`/BibTeX + PDF-figurer + genererede `.tex`-tabeller + `hyperref`/`cleveref` + Git-backup oven på Overleaf**, med en separat online appendix-løsning kun hvis universitetet eller supervisor faktisk ønsker et separat appendix-PDF. Overleaf anbefaler som udgangspunkt ét output-dokument pr. projekt, og deres Git-integration gør det let at kombinere online samarbejde med lokal backup/versionering. citeturn33view0turn8view3turn27view0turn12view6turn12view0

Før du vælger template eller formattering, bør du afklare universitetets krav til **titelblad, abstract/resumé, sideopsætning, margener, linjeafstand, side-nummerering, eventuelt separat online appendix og afleveringsformat**. Det er ikke kosmetik: officielle universitetsguides varierer faktisk på de punkter. KU stiller officielle specialeskabeloner og titelbladsmateriale til rådighed; andre universiteter kræver eksplicit små romertal i front matter og arabiske tal i hovedteksten, og nogle kræver særlige PDF-formater ved aflevering. citeturn8view6turn29search3turn29search5turn29search6turn3search19

### Gør dette først

Denne startcheckliste er den mest værdifulde del at få på plads **før** du skriver mere end et par sider:

- Bekræft de formelle krav: titelblad, eventuelt resumé på dansk/engelsk, margener, linjeafstand, maks. sidetal, enkelt- eller dobbeltsidet opsætning, og om bilag/online appendix skal afleveres samlet eller separat.
- Beslut, om du bruger et **officielt universitetstemplate** eller en **ren `report`-opsætning**.
- Opret projektstrukturen og navngivningskonventionerne fra dag ét.
- Vælg **pdfLaTeX** som standard, medmindre universitetet specifikt kræver en systemfont som Times New Roman; i så fald skift til **XeLaTeX** eller **LuaLaTeX**.
- Sæt referencesystemet op med **Zotero + Better BibTeX** og auto-eksport til én projekt-specifik `.bib`-fil.
- Beslut, at **ingen tabeller eller figurer må copy-pastes manuelt** fra Stata/Python til LaTeX.
- Skriv en kort `README.md` med: projektets struktur, entry-point scripts, data-version, og hvad der genererer hvilke outputfiler.
- Opret den første mærkede version i Overleaf/Git, inden du begynder at skrive rigtigt.

## Projektstruktur og dokumentarkitektur

### Anbefalet mappe- og filstruktur

Overleaf anbefaler multi-file-projekter til større dokumenter, og anbefaler samtidig, at **main-dokumentet ligger i roden**, fordi features og pakker ellers kan opføre sig forkert. For et speciale som dit vil jeg holde selve afhandlingen i ét rent projekt og lade alle genererede forskningsoutputs ligge i faste undermapper, så teksten aldrig blandes sammen med analyseresultater. citeturn8view2turn8view1turn12view6

```text
thesis/
├── main.tex
├── preamble.tex
├── commands.tex
├── README.md
├── frontmatter/
│   ├── titlepage.tex
│   ├── abstract.tex
│   ├── acknowledgements.tex
│   └── abbreviations.tex
├── chapters/
│   ├── 01_introduction.tex
│   ├── 02_literature.tex
│   ├── 03_institutional_background.tex
│   ├── 04_data.tex
│   ├── 05_empirical_strategy.tex
│   ├── 06_results.tex
│   ├── 07_robustness.tex
│   └── 08_conclusion.tex
├── appendices/
│   ├── appendix_a_data_sources.tex
│   ├── appendix_b_variable_definitions.tex
│   ├── appendix_c_additional_tables.tex
│   └── appendix_d_additional_figures.tex
├── bibliography/
│   └── references.bib
├── outputs/
│   ├── figures/
│   │   ├── main/
│   │   ├── appendix/
│   │   └── oa/
│   ├── tables/
│   │   ├── main/
│   │   ├── appendix/
│   │   └── oa/
│   ├── logs/
│   │   ├── python/
│   │   └── stata/
│   └── manifests/
│       └── output_manifest.csv
└── docs/
    ├── build_notes.md
    ├── supervisor_questions.md
    └── style_decisions.md
```

Denne struktur gør tre ting rigtigt på én gang: den følger Overleafs anbefaling om et rodbaseret main-dokument; den gør store dokumenter lettere at debugge og vedligeholde; og den skaber et naturligt sted, hvor Stata kan eksportere tabeller til LaTeX og figurer til filer, som du derefter kan inkludere med `\input` og `\includegraphics`. Stata’s nyere `collect export`/`etable`-workflow og den klassiske `esttab`-workflow understøtter begge denne fragment-baserede metode. citeturn12view6turn8view2turn18view0turn17search3

### Navngivning og generated outputs

Overleaf anbefaler, at grafikfiler **ikke** har mellemrum eller flere punktummer i filnavnet, og at man normalt **udelader filendelsen** i `\includegraphics`, så LaTeX selv kan finde passende formater. Det er en god tommelfingerregel at udvide til hele projektet: brug små bogstaver, ingen mellemrum, og forudsigelige præfikser. citeturn21view0turn21view2

Min anbefalede standard er:

- **Kapitelfiler:** `01_introduction.tex`, `06_results.tex`
- **Figurfiler:** `fig_irf_baseline_ai.pdf`, `fig_sample_restrictions.png`
- **Tabel-filer:** `tab_lp_baseline_main.tex`, `tab_summary_stats.tex`
- **Appendix-output:** `fig_app_parallel_trends.pdf`, `tab_app_alt_exposure.tex`
- **Labels i LaTeX:** `ch:introduction`, `sec:identification`, `fig:irf-baseline-ai`, `tab:lp-baseline`, `eq:lp-baseline`, `app:data-sources`

Brug **filer til orden** og **labels til referencer**. Filnavnet kan godt afspejle arbejdsflowet; labelnavnet skal afspejle læserens logik. Hvis et kapitel flytter plads, skal `sec:identification` stadig give mening. `\label`/`\ref`-mekanismen er netop robust over for omorganisering af afsnit. citeturn28search4turn28search19

Generated Stata-tabeller bør som hovedregel eksporteres som **LaTeX-fragments uden ydre `table`-miljø**, så caption, label og noter styres centralt i LaTeX. I Statas officielle `collect style tex` kan du slå `table`-miljøet fra med `nobegintable`; `esttab` bruges i økonomi netop ofte til at generere tabeller, der derefter inkluderes i LaTeX-dokumentet. Det er langt mere stabilt end at lade Stata bestemme caption- og label-logik. citeturn18view0turn17search3

### Anbefalet `main.tex`-skelet

Nedenstående skelet er den opsætning, jeg ville starte med til netop dit speciale:

```latex
\documentclass[11pt,a4paper,oneside,openany]{report}

\input{preamble}
\input{commands}

\begin{document}

% ---------- Front matter ----------
\pagenumbering{roman}

\input{frontmatter/titlepage}
\clearpage

\input{frontmatter/abstract}
\clearpage

\input{frontmatter/acknowledgements}
\clearpage

\tableofcontents
\clearpage

\listoffigures
\clearpage

\listoftables
\clearpage

% ---------- Main text ----------
\pagenumbering{arabic}

\include{chapters/01_introduction}
\include{chapters/02_literature}
\include{chapters/03_institutional_background}
\include{chapters/04_data}
\include{chapters/05_empirical_strategy}
\include{chapters/06_results}
\include{chapters/07_robustness}
\include{chapters/08_conclusion}

% ---------- Appendices ----------
\appendix
\include{appendices/appendix_a_data_sources}
\include{appendices/appendix_b_variable_definitions}
\include{appendices/appendix_c_additional_tables}
\include{appendices/appendix_d_additional_figures}

% ---------- References ----------
\bibliography{bibliography/references}

\end{document}
```

Brug `\include{...}` til hele kapitler, fordi det passer godt til større dokumentdele og naturlige sideskift; brug `\input{...}` til små komponenter som front matter, tabel-fragments og korte tekststykker. Overleafs dokumentation skelner netop mellem `\include` og `\input` på den måde. citeturn8view2

### Dokumentklasse og templatevalg

Hvis dit universitet eller institut stiller en **officiel LaTeX-thesis-skabelon** til rådighed, er udgangspunktet: brug den **kun i det omfang**, den konkret løser formelle krav. KU stiller eksempelvis officielle thesis-skabeloner og titelbladsmateriale til rådighed; det peger på, at du altid bør starte med at kontrollere, om der findes et formelt layoutkrav, før du bygger din egen struktur. citeturn8view6

Hvis du **ikke** har et officielt template, er **`report`** det bedste default-valg til en økonomi-masterafhandling. CTAN beskriver `report` som en multi-chapter-klasse, der ligner `book`, men **udelader bogproduktionsfunktioner**, der primært er relevante for professionel bogsætning. Standardklasserne viser også, at `report` som default er **`oneside` + `openany`**, mens `book` som default er **`twoside` + `openright`**, hvilket i praksis giver højresider/blanke sider og mere “bog”-adfærd, end de fleste specialer har brug for. citeturn33view0turn35view0turn35view1

`book` er kun bedre, hvis du **bevidst** ønsker `frontmatter`/`mainmatter`/`backmatter`, dobbeltsidet tryk, eller et mere bogagtigt layout. Til et supervisor-venligt, reproducerbart, Overleaf-baseret speciale er det normalt en fordel at undgå den ekstra kompleksitet. `memoir` er teknisk stærk, men den tilbyder også funktionalitet svarende til “over thirty” populære pakker og har særlige interaktioner med fx `hyperref`; det gør den fremragende til avanceret bogdesign, men ikke til et speciale, hvor robusthed og enkelhed er vigtigere end typografiske eksperimenter. citeturn35view0turn34view0

De mest almindelige template-fejl i Overleaf er: at starte fra et flot men irrelevant template, at lægge `main.tex` i en undermappe, at have flere “main”-dokumenter i samme projekt, og at lade gamle versioner ligge som ekstra mapper i stedet for at bruge Overleaf History. Overleaf fraråder eksplicit flere hoveddokumenter i samme projekt som normal praksis og anbefaler History frem for versionsmapper inde i projektet. citeturn12view6turn12view2

## Preamble, referencer og krydshenvisninger

### Preamble-best-practice og pakkeliste

En god thesis-preamble skal være **kort, stabil og lagdelt**: først sprog/tegnsæt og sideopsætning, så matematik, derefter figurer/tabeller, så bibliografi, og til sidst hyperlinks og smarte referencer. `geometry` er standardværktøjet til margener og sideopsætning; `microtype` forbedrer mikrotypografien; `setspace` styrer linjeafstand; `amsmath` udvider matematikopsætningen; `booktabs` er standard til pæne tabeller; `siunitx` giver konsekvent behandling af tal og tabelflugt; `hyperref` håndterer hyperlinks; og `cleveref` gør typebestemte referencer robuste. `caption`, `subcaption`, `threeparttable`, `longtable` og `pdflscape` er typisk nyttige, men bør kun aktiveres, hvis du faktisk har brug for deres funktionalitet. citeturn23search0turn23search1turn23search2turn30search12turn13view1turn13view2turn13view3turn28search2turn13view4turn13view5turn24search0turn24search1turn24search2

Pakkekonflikter kommer sjældent af én “forkert” pakke og oftere af for meget. Den vigtigste praksisregel er: **load `hyperref` sent** og **`cleveref` efter `hyperref`**. `memoir` kræver ekstra omtanke, netop fordi klassen allerede erstatter store dele af pakkeøkosystemet. Og du bør vælge **enten** `natbib`/BibTeX **eller** `biblatex`/Biber som bibliografisystem — ikke begge. citeturn11search18turn11search14turn34view0turn8view3turn8view4

### Anbefalet `preamble.tex`-skelet

Dette er en **praktisk minimalversion** til et economics-thesis-projekt i Overleaf:

```latex
% ---------- Encoding and language ----------
\usepackage[T1]{fontenc}
\usepackage[utf8]{inputenc} % remove if you switch to XeLaTeX/LuaLaTeX
\usepackage[english]{babel} % change if thesis language differs

% ---------- Typography and layout ----------
\usepackage[a4paper,margin=2.5cm]{geometry}
\usepackage{microtype}
\usepackage{setspace}
\onehalfspacing

% ---------- Math ----------
\usepackage{amsmath,amssymb,mathtools,bm}
% \numberwithin{equation}{chapter} % optional: uncomment for (3.1), (3.2), ...

% ---------- Tables and figures ----------
\usepackage{graphicx}
\graphicspath{{outputs/figures/main/}{outputs/figures/appendix/}{outputs/figures/oa/}}

\usepackage{booktabs}
\usepackage{siunitx}
\sisetup{
  detect-all,
  group-minimum-digits = 4,
  input-symbols = {()},
  table-align-text-post = false
}

\usepackage{threeparttable} % recommended for table notes
\usepackage{longtable}      % optional for multi-page tables
\usepackage{pdflscape}      % optional for wide appendix material
\usepackage{caption}        % useful for caption*
\usepackage{subcaption}     % only if you actually need panel figures

% ---------- Appendices ----------
\usepackage[toc,page]{appendix}

% ---------- Citations ----------
\usepackage[authoryear,round]{natbib}
\bibliographystyle{plainnat} % or university-required style / aea if you use AEA template files

% ---------- Hyperlinks and cross-references ----------
\usepackage[hidelinks]{hyperref}
\usepackage[capitalize,nameinlink,noabbrev]{cleveref}

% ---------- Optional headers/footers ----------
% \usepackage{fancyhdr}
% \pagestyle{fancy}
% \fancyhf{}
% \fancyfoot[C]{\thepage}
```

Hvis universitetet kræver en font som **Times New Roman**, eller hvis du vil bruge uploaded/OpenType-fonts, skal du skifte compiler til **XeLaTeX** eller **LuaLaTeX** og bruge `fontspec`; Overleaf dokumenterer eksplicit, at `fontspec` kræver netop de compilere. Hvis du ikke har et sådant krav, er det normalt mere robust at blive på standardopsætningen og undgå et compiler-skifte midt i projektet. citeturn32search2turn32search8turn32search17

### Anbefalet `commands.tex`-skelet

Brug `commands.tex` til **stabile, genbrugte byggesten** — ikke til at gemme hele sætninger eller lave et personligt programmeringssprog oven på LaTeX. Makroer er bedst til metadata, faste figurstørrelser, matematiske operatorer og måske et par centrale notationselementer. citeturn30search12turn28search2

```latex
% ---------- Metadata ----------
\newcommand{\ThesisTitle}{AI Exposure, Forward Guidance, and Firm Responses}
\newcommand{\ThesisAuthor}{Your Name}
\newcommand{\ThesisDate}{May 2026}
\newcommand{\ThesisSupervisor}{Supervisor Name}

% ---------- Reusable sizes ----------
\newcommand{\MainFigureWidth}{0.82\textwidth}
\newcommand{\WideFigureWidth}{0.95\textwidth}

% ---------- Math operators ----------
\newcommand{\E}{\mathbb{E}}
\newcommand{\Ind}{\mathbb{1}}
\DeclareMathOperator{\Var}{Var}
\DeclareMathOperator{\Cov}{Cov}
\DeclareMathOperator{\Corr}{Corr}
\DeclareMathOperator{\SE}{SE}
\DeclareMathOperator*{\plim}{plim}

% ---------- Common notation ----------
\newcommand{\FirmFE}{\alpha_i}
\newcommand{\SectorEventFE}{\delta_{s\times e}}

% ---------- Significance stars ----------
\newcommand{\sym}[1]{\ifmmode^{#1}\else\(^{#1}\)\fi}
```

God tommelfingerregel: hvis en kommando bruges færre end cirka tre steder, så lad den normalt blive som almindelig LaTeX-kode. Hvis en notation eller formulering bruges igen og igen — fx `\E`, `\Var`, faste figurbredder eller en gennemgående FE-term — så er en kommando en gevinst.

### Bibliografi og citationer

I økonomi er den vigtigste trade-off ikke “hvad er mest moderne?”, men “hvad er mest kompatibelt med økonomi-workflows?”. Overleafs egen dokumentation siger, at `biblatex` er moderne, aktivt udviklet og mere fleksibelt, men understreger også, at **de fleste tidsskrifter stadig bruger `bibtex` og `natbib`**. Samtidig bruger AEA’s stileksempler author-date og stiller `aea.bst` til rådighed til LaTeX-brugere. Derfor er den bedste **default** til en economics-thesis typisk stadig **BibTeX + `natbib`**, især hvis kapitler senere skal kunne omskrives til papirformater tæt på AEA/AEJ. citeturn8view3turn8view4turn27view0turn27view1

Min anbefaling er derfor:

- **Vælg `natbib`/BibTeX som standard**, medmindre dit universitet eksplicit kræver `biblatex` eller du har et tungt flersproget bibliografi-behov.
- Brug én fil: `bibliography/references.bib`.
- Brug author-year citations i teksten (`\citet`, `\citep`).
- Hold stilfilen simpel: `plainnat` eller universitetets stil; brug `aea.bst`, hvis du specifikt vil tæt på AEA.

Økonomi bruger næsten altid **author-date** i teksten frem for reference-fodnoter. AEA’s stilguide gør også klart, at tekstcitationer og referenceliste skal matche, og at datasæt brugt i forskningen skal stå i referencelisten. citeturn27view0turn27view1turn27view2

### Zotero-, Google Scholar- og Better BibTeX-workflow

Det mest robuste referenceflow for LaTeX er: **Zotero som masterbibliotek**, **Zotero Connector** til at hente metadata fra journalpages/databaser, og **Better BibTeX** til stabile citekeys og automatisk eksport af én projektspecifik `.bib`-fil. Zotero Connector gemmer poster med rig metadata direkte fra browseren og kan også hente bibliografiske filer som RIS/Refer; Better BibTeX giver stabile citekeys og kan auto-eksportere en collection med “Keep updated”, så ændringer i Zotero automatisk slår igennem i din `.bib`-fil. citeturn40view2turn26search8turn40view0turn40view1

Et godt praktisk setup er:

- Opret en Zotero-collection kun til specialet.
- Auto-eksporter netop den collection til `bibliography/references.bib`.
- Brug en citekeyregel som noget i stil med `auth.lower + year`.
- Lås citekeys, hvis du først har brugt dem i teksten og senere retter metadata.

Google Scholar er nyttig som fallback, men ikke som sandheds-kilde for metadata. Scholar understøtter BibTeX direkte via **“Cite”**-knappen, men Scholar dækker kilder bredt og kan have ujævn metadata-kvalitet; derfor bør du altid kontrollere mod publisher-side, DOI-metadata eller RePEc/postingens officielle side, før en reference får lov at blive stående i din `.bib`. citeturn39view2turn39view3turn37search7

Til økonomi-working papers er **RePEc/EconPapers** særligt nyttigt: EconPapers-sider viser ofte direkte **“Export reference: BibTeX”**. Det gør dem mere pålidelige end tilfældig håndkopiering, især for diskussionspapirer, policy papers og ældre working papers med serieinformation. citeturn7search2turn7search11

### Rensning af `.bib`-filer

En ren `.bib`-fil er ikke “perfekt metadata”; det er **konsekvent metadata, som kompilerer stabilt**. Det vigtigste er:

- beskyt kapitalisering i titler som `{AI}`, `{FOMC}`, `{S\&P} 500`, `{U.S.}`;
- sørg for DOI eller URL, helst DOI når den findes;
- brug `@techreport` til working papers og institutionsserier, ikke `@article` hvis der ikke er et journalpapir;
- brug komplette felter for datasæt, software og websider;
- fjern dubletter mellem working paper og publiceret artikel, medmindre du bevidst citerer begge versioner. citeturn27view2turn25search1turn25search2turn25search0turn40view1

Hvis du holder dig til `natbib`/BibTeX, er de mest portable entry types typisk `@article`, `@book`, `@incollection`, `@techreport` og `@misc`. Hvis du i stedet vælger `biblatex`, bliver `@online`, `@dataset` og `@software` renere. Det er netop en af de reelle fordele ved `biblatex`, men økonomi-kompatibilitet trækker stadig ofte i retning af `natbib`. citeturn8view3turn8view4turn25search1turn25search20

Til datasæt bør referencen mindst indeholde skaber, år, titel/version, database eller distributør, og DOI/URL; AEA’s vejledning er meget klar på dette punkt. Til software er den bedste praksis at citere **en specifik version** af software eller kode, ikke bare et generisk repo-navn, og gerne via en persistent identifier, når det findes. citeturn27view2turn25search0turn27view3

### Krydshenvisninger og labels

Brug **`cleveref` som standard** i et speciale. CTAN beskriver pakken som intelligent cross-referencing, hvor referencetypen bestemmes automatisk, og pakken er lavet til netop robuste referencer på tværs af kapitler, tabeller, figurer og ligninger. Overleafs dokumentation viser også den grundlæggende fordel ved labels: referencer bliver rigtige, selv når sektioner flyttes rundt. citeturn28search2turn28search3turn28search4

Min anbefaling er:

- brug `\cref{fig:...}` frem for at skrive “Figure \ref{...}” manuelt;
- sæt `\label` lige efter `\caption` i figurer/tabeller;
- brug faste præfikser: `ch:`, `sec:`, `subsec:`, `eq:`, `fig:`, `tab:`, `app:`.

Overleaf bemærker, at cross-references lokalt ofte kræver to kompileringer; i Overleaf sker det normalt automatisk. Det gør dog kun systemet robust, hvis labels er konsistente og ikke genbruges. citeturn28search0

## Figurer, tabeller og notation

### Figurer

Overleafs billeddokumentation er meget tydelig: hvis grafikken er **vektorbaseret**, bør du normalt bruge **PDF eller EPS**; hvis den er **bitmap/raster**, bør du bruge **PNG eller JPG**. Stata kan eksportere til PDF, EPS, SVG, PNG og flere andre formater. Til en thesis i Overleaf er den mest robuste standard derfor: **PDF som default for Stata- og Python-figurer**, **PNG kun når figuren reelt er raster** — fx heatmaps, screenshots, komplekse density plots eller billedbaserede illustrationer. citeturn36search1turn36search4turn18view2

Det giver følgende praktiske regel:

- **PDF**: coefficient plots, IRFs, event-study-figurer, line charts, scatter plots, barplots.
- **PNG**: heatmaps, screenshots, satellit-/kortbilleder, figurer der allerede er raster.
- **EPS**: kun hvis du har et ældre workflow eller printing-krav, der kræver det.
- **SVG**: fint som redigerbar masterfil, men konvertér normalt til PDF før inclusion i specialet, selv om Stata kan eksportere SVG. Overleafs egen anbefaling til vektorfigurer peger mod PDF/EPS som den sikre standard i dokumentet. citeturn36search2turn36search1

Store eller mange billeder kan gøre Overleaf langsom. Overleaf anbefaler både Fast Draft mode og billedoptimering som konkrete løsninger på timeout-problemer, og dokumentationen fremhæver store image-filer som en central årsag til langsom kompilering. Derfor bør du ikke lægge 20 MB PNG-filer ind “for en sikkerheds skyld”. Eksportér den rigtige størrelse første gang. citeturn31view2turn12view5turn12view4

Brug en ensartet figurbredde på tværs af specialet. Det bedste trick er ikke manuel resizing i hver figur, men én eller to faste kommandoer i `commands.tex`, fx `\MainFigureWidth` og `\WideFigureWidth`. Det gør specialet visuelt roligt og gør det let at ændre hele figurlayoutet sent i processen.

### Anbefalet figurmiljø med note

Dette er et godt standardmiljø til empiriske økonomifigurer:

```latex
\begin{figure}[!htbp]
  \centering
  \includegraphics[width=\MainFigureWidth]{fig_irf_baseline_ai}
  \caption{Firm responses to forward-guidance shocks by AI exposure}
  \label{fig:irf-baseline-ai}
  
  \vspace{0.5em}
  \begin{minipage}{0.92\textwidth}
    \footnotesize
    \textit{Notes:} The figure plots coefficient estimates and 95\% confidence
    intervals from panel local projections for horizons $h=0,\ldots,12$.
    The shock is the high-frequency forward-guidance surprise measured on
    FOMC announcement windows. Firms are sorted by pre-sample AI exposure.
    Regressions include firm fixed effects and sector $\times$ event fixed effects.
    Standard errors are two-way clustered by firm and event.
    \textit{Source:} Author's calculations.
  \end{minipage}
\end{figure}
```

Fagligt er det god praksis, at figurnoter forklarer **hvad der er plottet**, **hvordan usikkerheden vises**, **hvad samplet er**, og **hvilke FE-/SE-valg der er centrale**. AEA’s stilguide kræver også klare filnavne for figurpaneler og foretrækker vektorbaserede figurer. citeturn27view0

Subfigurer bør bruges sparsomt. `subcaption` er den moderne løsning, og dokumentationen har særskilt vejledning om cross-referencing og labelplacering. Brug panels kun, når læseren faktisk vinder noget ved synkron sammenligning — fx samme outcome under flere shock-definitioner. Hvis panels bare gør figuren mindre og sværere at læse, så lav to figurer i stedet. citeturn13view5

### Tabeller

Til økonomitabeller er **`booktabs`** næsten obligatorisk: pakken er lavet til publication-quality-tabeller og understøtter den typografi, som også AEA’s stileksempler peger mod — ingen vertikale linjer, ingen skygger, få og tydelige horisontale regler, og paneler/notes i stedet for dekorativ formatering. `threeparttable` er derefter den mest praktiske løsning til table notes i samme bredde som selve tabellen. `longtable` er til multipage-tabeller; `pdflscape` til brede appendixtabeller; `rotating`/`sidewaystable` kun hvis du vil rotere en enkelt float på én side. citeturn13view1turn24search0turn24search1turn24search2turn24search3turn27view0

AEA’s style guide er nyttig som benchmark, selv hvis et speciale ikke skal ligne et AEA-paper 1:1. Guiden anbefaler bl.a. kun horisontale linjer, ingen shading, panelmærkning som “Panel A/B”, nul foran decimaler, og standard errors i parentes. AEA fraråder stjerner som signifikansmarkering i journalmanuskripter; i et speciale kan du bruge stjerner, hvis det er supervisor-normen, men så skal du stadig have rigtige standard errors og en fuld note. citeturn27view0turn27view1

Mit praktiske råd er derfor:

- **Regressionstabeller:** generér dem fra Stata.
- **Summary statistics:** generér dem fra Stata, men rens layoutet i wrapperen.
- **Variabeldefinitioner og datakilder:** skriv dem ofte manuelt i LaTeX, fordi de er teksttunge.
- **Robusthedstabeller:** appendix, med konsekvent noteformat.
- **Alt for brede tabeller:** del i paneler, flyt til appendix, eller brug `pdflscape`.

Til en Stata-baseret workflow er der to gode spor. Hvis du arbejder på Stata 17+ og vil holde dig tæt på officielt understøttede kommandoer, er `etable`/`dtable`/`collect export` et godt valg. Hvis du allerede er vant til økonomi-standardtabeller, er `esttab` fortsat et meget effektivt værktøj i praksis og designet netop med LaTeX-integration i tankerne. citeturn18view1turn18view0turn17search3

### Anbefalet regressionstabel

Nedenfor er et godt standardformat for en empirisk økonomitabel i specialet:

```latex
\begin{table}[!htbp]
\centering
\caption{Baseline panel local projections}
\label{tab:lp-baseline}
\begin{threeparttable}
\begin{tabular}{lcccc}
\toprule
& \multicolumn{4}{c}{Cumulative excess return at horizon $h$} \\
\cmidrule(lr){2-5}
& (1) & (2) & (3) & (4) \\
& $h=0$ & $h=1$ & $h=3$ & $h=6$ \\
\midrule
FG shock $\times$ AI exposure & 0.012*** & 0.018*** & 0.021** & 0.015* \\
& (0.004) & (0.006) & (0.009) & (0.008) \\
FG shock & -0.003 & -0.006 & -0.010 & -0.008 \\
& (0.003) & (0.005) & (0.007) & (0.007) \\
\midrule
Firm FE & Yes & Yes & Yes & Yes \\
Sector $\times$ event FE & Yes & Yes & Yes & Yes \\
Controls & Yes & Yes & Yes & Yes \\
Observations & 24,860 & 24,860 & 24,860 & 24,860 \\
FOMC events & 42 & 42 & 42 & 42 \\
\bottomrule
\end{tabular}
\begin{tablenotes}[flushleft]
\footnotesize
\item \textit{Notes:} Each column reports a separate panel local-projection
regression at horizon $h$. The coefficient of interest is the interaction between
the forward-guidance shock and firm-level AI exposure. All specifications include
firm fixed effects and sector $\times$ event fixed effects. Standard errors, shown
in parentheses, are two-way clustered by firm and event. AI exposure is standardized.
Significance levels: * $p<0.10$, ** $p<0.05$, *** $p<0.01$.
\end{tablenotes}
\end{threeparttable}
\end{table}
```

Hvis tabellen genereres fra Stata, bør du i praksis oftest kun lade Stata generere **selve `tabular`-delen** og lade caption, label og notes blive i LaTeX-wrapperen. Det giver dig ensartet caption-stil og reducerer risikoen for, at layoutet går i stykker, hvis du skifter template eller table package senere. Stata’s officielle LaTeX-eksport understøtter netop valg om hvorvidt `table`-miljøet skal genereres eller ej. citeturn18view0

### Ligninger og notation

Til økonometriske modeller bør du holde dig til `amsmath`/`mathtools` og skrive modeller, der er **læselige først og smukke bagefter**. AEA’s equation-råd er en nyttig baseline: skalarer i kursiv, vektorer/matricer i fed, tydelige subscripts/superscripts, og ikke for dybe indeksstrukturer. citeturn27view0turn30search12

En god lokal-projektionsligning til dit setup kunne se sådan ud:

```latex
\begin{equation}
Y_{i,e+h}
=
\beta_h \,\mathrm{FGShock}_e
+
\gamma_h \left(\mathrm{FGShock}_e \times \mathrm{AIExposure}_i\right)
+
X_{i,e-1}'\theta_h
+
\alpha_i
+
\delta_{s \times e}
+
\varepsilon_{i,e+h},
\qquad h = 0,\ldots,H.
\label{eq:lp-baseline}
\end{equation}
```

Det vigtigste er ikke at pakke al økonometrien ind i fancy notation, men at være konsekvent. Brug samme navn for forward-guidance-shocket overalt; hold faste effekter i samme symbolsprog gennem kapitler og appendikser; og erklær en notation én gang, helst i metodekapitlet og eventuelt i en kort notationstabel i appendix. Hvis du har mange symboler, kan `glossaries` eller `nomencl` bruges; hvis du kun har en håndfuld centrale symboler, er en simpel appendix-tabel næsten altid lettere at vedligeholde i praksis. citeturn30search1turn30search2

## Front matter, appendikser og formalia

### Front matter og grundformatering

En klassisk thesis-orden er: **titelblad, abstract/resumé, acknowledgements, indholdsfortegnelse, eventuelle lister over figurer/tabeller, derefter hovedtekst**. Overleaf dokumenterer de centrale kommandoer til lister over figurer og tabeller, og universitetsguides følger typisk samme overordnede rækkefølge, om end detaljerne varierer. citeturn22view0turn3search8turn3search12

Side-nummerering følger ofte mønsteret: titelblad tæller, men vises uden nummer; front matter i små romertal; hovedtekst, appendikser og litteraturliste i arabiske tal. Overleaf viser, hvordan man skifter mellem romertal og arabiske tal, og flere universiteters formateringsguides bruger netop dette mønster. Det er derfor en god default, men du skal stadig kontrollere de lokale regler. citeturn29search0turn29search3turn29search5turn29search6

Hvis universitetet ikke har specifikke krav, så hold opsætningen enkel: **A4**, læsbart 11 pt eller 12 pt, rimelige margener sat med `geometry`, og `setspace` til cirka 1.5 linjeafstand. Mange universitetsguides ender i praksis omkring 1 inch / 2.5 cm-agtige margener og 1.5 eller dobbeltafstand i hovedtekst, men variationen er stor nok til, at du ikke bør overoptimere, før kravene er bekræftet. `fancyhdr` er kun nødvendigt, hvis universitetet kræver særlige headers/footers; ellers er en simpel pagestyle typisk bedst. citeturn23search0turn23search2turn29search2turn29search3turn3search9

### Hvad du bør spørge supervisor eller studieadministration om

Afklar disse punkter tidligt, og helst skriftligt:

- Skal referenceskemaet følge universitetets formalia eller blot økonomifaglig standard?
- Skal litteraturlisten stå før eller efter appendikser?
- Skal der være både abstract og dansk resumé?
- Er der krav til font, marginer, line spacing eller sidetal?
- Skal online appendix afleveres som separat PDF?
- Er der krav om PDF/A eller anden særlig afleverings-PDF?
- Er brede tabeller i landscape acceptable?
- Er AEA-lignende tabeller uden vertikale linjer acceptable?

### Appendikser og online appendix

I `report`/`book`-klasserne nulstiller `\appendix` kapitel- og sektionscounters, og figurer/tabeller nummereres per kapitel. Derfor får du naturligt **Appendix A**, **Figure A.1**, **Table B.2** osv., hvis du strukturerer appendikser som kapitler. Hvis du vil styre appendix-layoutet mere eksplicit, giver `appendix`-pakken ekstra muligheder for overskrifter og indholdsfortegnelseopsætning. citeturn15view2turn16search1turn16search3

For et økonomispeciale vil jeg anbefale denne struktur:

- **Appendix A:** data sources og sample construction  
- **Appendix B:** variable definitions og målekonstruktion  
- **Appendix C:** ekstra robusthedstabeller  
- **Appendix D:** ekstra figurer eller alternative shock/AI-exposure-definitioner

Det er næsten altid bedre end at blande robusthed, variable, datadetaljer og screenshots ind i ét langt “miscellaneous appendix”.

Hvis du skal have et **separat online appendix**, er den mest robuste løsning typisk **et separat Overleaf-projekt** med eget hoveddokument, fx `oa_main.tex`, frem for at forsøge at få to ligeværdige output-PDF’er ud af ét projekt. Overleaf anbefaler normalt ét output-dokument pr. projekt, og advarer også om problemer, hvis flere kompilerbare filer har samme navn eller hvis hoveddokumentet ligger i en undermappe. Hvis du virkelig vil referere mellem de to dokumenter, er `xr`-pakken en avanceret mulighed, men det er kun værd at gøre, hvis du faktisk har et eksternt appendix-PDF som fast krav. citeturn12view6turn28search11

Min anbefaling til kode/data-dokumentation er: hold **kort metode- og variabledokumentation i selve thesis-appendikset**, men læg **fulde kode- og logdetaljer** i replication-materiale eller et separat arkiv. Thesis-appendikset skal hjælpe læseren; det behøver ikke være en rå dump af alle logs.

## Samarbejde, versionsstyring og reproducerbarhed

### Samarbejde med supervisor i Overleaf

Overleafs styrke er realtidssamarbejde, kommentarer og track changes. Track Changes aktiveres i reviewing mode, og Overleaf understøtter både simultan redigering, kommentarer og projekthistorik. Hvis supervisor primært skal kommentere på tekst og argumentation, er review-adgang ofte bedre end fuld edit-adgang; hvis I begge aktivt skriver, så brug edit-adgang men aftal tydeligt, hvem der “ejer” strukturændringer i `preamble`, `commands`, labels og output-stier. citeturn12view1turn29search1

Den bedste review-rutine er enkel:

- én delt master-version i Overleaf,
- kommentarer i Overleaf frem for e-mailtråde,
- labeled versions før store møder eller større omskrivninger,
- en kort changelog i begyndelsen af hvert supervisorudkast.

Overleafs History-funktion lader dig netop label og sammenligne versioner; på free-plan ser man seneste 24 timer plus mærkede versioner, mens fuld historik kræver premium. Det betyder i praksis, at labeled versions er værdifulde selv på gratis konto. citeturn12view2

### Overleaf-only eller Git-backed

**Overleaf-only** er fint, hvis du vil minimere opsætning og skrive simpelt med supervisor. **Git-backed Overleaf** er bedre, hvis du vil have seriøs backup, lokal redigering, branches/tags og en ren forbindelse mellem analysekode og tekstprojekt. Overleafs Git-integration lader dig klone projektet lokalt og bruge det som en remote repo med token-baseret auth. citeturn12view0

Mit råd til et økonomispeciale er: brug **Overleaf som daglig skriveflade** og **Git som sikkerhedsnet og milepælsarkiv**. Det er den mindst smertefulde kombination. Du behøver ikke udvikle et stort branching-regime; en simpel `main` plus tags som `draft-2026-05-25` og `submission-rc1` er nok.

### Reproducerbar workflow mellem Python, Stata og LaTeX

Dit workflow er allerede godt designet: **Python konstruerer analysis panels; Stata producerer endelige regressioner, tabeller, figurer og logs; Overleaf formaterer teksten**. Det rigtige næste skridt er at gøre filgrænserne hårde:

- Python må kun skrive til `data/final/` eller tilsvarende lokalt og evt. metadata/manifestfiler.
- Stata må kun læse færdige paneler og skrive til `outputs/tables/`, `outputs/figures/` og `outputs/logs/`.
- LaTeX må aldrig være stedet, hvor tal “rettes manuelt”.

Stata understreger selv reproducerbarhed via genkørbare scripts, `version`-kommandoen og `datasignature`, så resultater kan reproduceres og datændringer verificeres. Det er præcis det princip, du vil have ind i specialets build-logik. citeturn18view3turn19search17turn19search1

En meget god praksis er at oprette en simpel `output_manifest.csv` med kolonner som:

```text
output_file,generating_script,data_signature,created_at,used_in_draft
tab_lp_baseline_main.tex,master_tables.do,162:11(...),2026-05-17,draft_2026-05-17
fig_irf_baseline_ai.pdf,master_figures.do,162:11(...),2026-05-17,draft_2026-05-17
```

Det er ikke et LaTeX-krav; det er et specialeværktøj. Når du senere spørger dig selv “hvilken specifikation genererede Tabel 4?”, skal svaret kunne findes på få sekunder.

Brug også **Stata logs** systematisk, men opbevar dem som outputfiler — ikke som appendixtekst. En god struktur er én master-log for hver større build: `master_tables.log`, `master_figures.log`, `master_regressions.log`. Hvis du kan, så lås Stata-versionen i toppen af dine do-files med `version ...`, og gem relevante `datasignature`-checks i log eller manifest. citeturn19search17turn19search1turn18view3

### Tjekliste før hvert supervisorudkast

Denne checkliste bør du køre hver gang, før du sender et nyt draft:

- Genkør Python-delen, hvis panelerne er ændret.
- Genkør Stata master do-file(s) for tabeller, figurer og logs.
- Kontroller at modification dates på centrale outputfiler er nyere end sidste kodeændring.
- Spot-check mindst tre nøgleobjekter: én hovedtabel, én hovedfigur og én appendix-tabel mod Stata-loggen.
- Søg i projektet efter `TODO`, `TK`, `FIXME` og fjern eller skjul det, der ikke skal ses af supervisor.
- Label versionen i Overleaf History før du deler PDF’en. citeturn12view2
- Gem PDF’en lokalt med en entydig filnavnestandard, fx `thesis_draft_2026-05-17.pdf`.
- Send gerne en kort changelog med “nyt siden sidst”, så supervisor ikke skal opdage ændringerne via detail-læsning alene.

## Stabilitet, skriveflow og aflevering

### Compile-stabilitet og Overleaf-performance

Overleafs vigtigste performance-råd er meget håndgribelige: brug **Fast [draft] mode**, optimer store billeder, ret compile errors med det samme, og undgå tunge elementer, der ikke er nødvendige. Overleaf fremhæver store billeder, komplekse `TikZ`/`pgfplots`-objekter og ophobede fejl som klassiske årsager til timeouts. I dit tilfælde er det let at udnytte: lav figurerne i Stata/Python og inkluder dem som færdige filer; undgå at teikne analytiske figurer i TeX, medmindre det er helt nødvendigt. citeturn12view3turn12view4turn31view3

Store projekter bør deles op i kapitel-filer, både af organisatoriske og tekniske grunde. Overleaf nævner også, at `.tex`-filer over 2 MB bliver ikke-editable som main-dokumenter, hvilket er endnu en grund til at dele projektet op tidligt. Hvis hele dokumentet pludselig ikke kan kompilere, så brug først **Stop on first error**, derefter **Recompile from Scratch**, og derefter projekthistorikken til at finde den seneste ændring. citeturn12view6turn31view1

### Skriveworkflow under specialet

Det bedste skriveworkflow i en empirisk afhandling er ikke lineært, men modulært:

- skriv introduktion og data/metode tidligt,
- skriv resultatafsnit omkring tabeller/figurer, ikke omvendt,
- hold robusthedskapitlet som en tydelig forlængelse af baseline-resultaterne,
- brug placeholders kun for indhold, ikke for formattering.

Overleaf gør det let at kommentere og revidere tekst undervejs; pointen er derfor at holde formatting så automatiseret, at du ikke bruger uge 10 på at flytte captions og retabellere appendices i hånden. citeturn12view1turn8view2

En god vane i resultatkapitler er at lade hvert større estimatsæt have samme mikrologik:

1. kort påmindelse om specifikation  
2. henvisning til tabel/figur  
3. hovedmønster  
4. økonomisk størrelse  
5. usikkerhed/CI/SE  
6. relation til hypotesen  
7. overgang til robusthed eller heterogenitet.

Det lyder banalt, men det er præcis den type konsistens, der gør et speciale “supervisor-friendly”.

### Almindelige fejl og hvordan du undgår dem

De fejl, der koster mest tid sent i processen, er næsten altid workflow-fejl, ikke LaTeX-fejl:

- **At starte med et fancy template i stedet for at starte med universitetets faktiske krav.** Tjek officielle skabeloner og formalia først. citeturn8view6turn29search3
- **At lægge `main.tex` i en undermappe.** Overleaf fraråder det direkte. citeturn12view6
- **At have flere konkurrerende main-dokumenter i samme projekt.** Brug ét projekt pr. output-dokument. citeturn12view6
- **At copy-paste regressionstal ind i LaTeX.** Brug Stata-eksporterede fragments i stedet. citeturn18view1turn17search3
- **At redigere genererede `.tex`-tabeller manuelt.** Ret i Stata-do-filen eller wrapperen, ikke i outputfilen.
- **At blande `natbib` og `biblatex`.** Vælg ét bibliografisystem og hold dig til det. citeturn8view3turn8view4
- **At bruge enorme PNG-filer til almindelige linjediagrammer.** Brug PDF for vektorfigurer og optimer rasterfiler. citeturn36search1turn12view4
- **At loade `hyperref` for tidligt eller i forkert rækkefølge.** Hold `hyperref` sent og `cleveref` efter `hyperref`. citeturn11search18turn11search14
- **At gemme “final_v2_final_reallyfinal” som mapper i projektet.** Brug History/Git-tags i stedet for pseudo-versioner. citeturn12view2turn12view6
- **At udskyde appendix-strukturen til sidst.** Beslut tidligt, hvad der er hovedtekst, appendix og eventuelt online appendix.

### Tjekliste før endelig aflevering

Kør denne liste systematisk før submission:

- Kompilér fra scratch og verificér, at der ikke er uopklarede citationer eller referencer. citeturn31view1
- Kontroller at alle `\cref{...}` peger korrekt, og at der ikke findes `??` i PDF’en.
- Tjek, at figur- og tabelnummerering er konsekvent i hovedtekst og appendikser.
- Tjek, at alle tabeller og figurer har caption, label og note/source hvor nødvendigt.
- Tjek, at alle citerede kilder faktisk findes i litteraturlisten, og at dubletter er fjernet. citeturn27view0
- Tjek datasæt, software og onlinekilder særskilt for fulde metadata og DOI/URL. citeturn27view2turn25search0turn25search1
- Bekræft side-nummerering, front matter-rækkefølge, margener og eventuelle universitetsspecifikke krav. citeturn29search0turn29search3turn29search6
- Eksportér og gem en lokal backup af hele Overleaf-projektet, og tag den endelige version i Git/History. citeturn12view0turn12view2
- Gem den endelige PDF med entydigt navn, fx `thesis_submission_2026-05-17.pdf`.
- Arkivér efter aflevering: PDF, hele LaTeX-kilden, `references.bib`, de endelige Stata-logs, figure/table-outputs, manifestfilen og de do-files/scripts der genererede dem.
- Hvis universitetet kræver PDF/A eller særligt afleveringsformat, verificér det eksplicit før upload. citeturn3search19

### Kort slutanbefaling

Hvis du vil have den bedste **default** til netop dit speciale, så vælg denne pakke af beslutninger:

- **Dokumentklasse:** `report`  
- **Compiler:** `pdfLaTeX` som default; kun `XeLaTeX/LuaLaTeX` hvis universitetet kræver specifik font  
- **Struktur:** `main.tex` i roden, kapitler som separate filer, `preamble.tex` og `commands.tex` separat  
- **Bibliografi:** `natbib` + BibTeX + én projektspecifik `references.bib` fra Zotero/Better BibTeX  
- **Figurer:** PDF som standard; PNG kun til raster  
- **Tabeller:** Stata-eksporterede `.tex`-fragments med caption/label/notes styret i LaTeX  
- **Appendikser:** i samme thesis-PDF; separat online appendix kun som separat projekt, hvis det faktisk kræves  
- **Samarbejde:** skriv i Overleaf, label versioner før møder, og brug Git som backup hvis muligt  
- **Reproducerbarhed:** ingen manuel copy-paste af tal, én output-mappe-struktur, logs og manifest for alle centrale resultater  

Det er den opsætning, der bedst balancerer **renhed, robusthed, reproducerbarhed, økonomifaglig standard og lav risiko for sene formatteringsproblemer**. citeturn12view6turn33view0turn8view3turn12view0turn18view3turn36search1turn18view0