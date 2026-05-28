# AI Assistant Context — Overleaf/LaTeX Formatting and Workflow

> **Purpose:** Use this file as context in ChatGPT, Claude, or another AI assistant when asking for help writing, editing, or formatting sections of the thesis.  
> **Scope:** This file only describes the Overleaf/LaTeX setup, formatting rules, file organization, citation workflow, figure/table conventions, appendix workflow, and project hygiene.  
> **Do not use this file as a model specification or empirical-methods guide.**  
> **Platform:** Overleaf / LaTeX  
> **Writing language:** English  
> **Last updated:** 2026-05-17

---

## 1. Core instruction for AI assistants

When helping with this thesis, follow the existing Overleaf setup. Do not propose a new LaTeX project structure, template, preamble, bibliography system, or figure/table workflow unless explicitly asked.

When the user asks for thesis text, return **paste-ready LaTeX** for the relevant file. Keep the formatting consistent with this document.

The active thesis is organized as a modular Overleaf project with:

```text
main.tex
preamble.tex
commands.tex
frontmatter/
chapters/
appendices/
bibliography/
outputs/
```

The active thesis is controlled by:

```text
main.tex
```

Do not write normal thesis content directly in `main.tex`.

---

## 2. Root-level `old/` folder

The Overleaf root contains a folder named:

```text
old/
```

This folder contains material from the pre-overhaul Overleaf setup. It is an archive only.

Rules:

```text
Do not compile files from old/.
Do not store active figures in old/.
Do not store active tables in old/.
Do not store active bibliography files in old/.
Do not copy old preamble/package code into the new setup.
Only reuse body text after adapting it to the new chapter/appendix structure.
```

If old text is reused, paste only the relevant body text into the correct file under `chapters/` or `appendices/`.

If old figures are reused, rename them and place them in:

```text
outputs/figures/main/
outputs/figures/appendix/
```

If old citations are reused, convert long source footnotes into BibTeX entries in:

```text
bibliography/references.bib
```

There should **not** be an active standalone root-level file named:

```text
appendix.tex
```

Old `appendix.tex` content should be moved into:

```text
appendices/appendix_a_data.tex
appendices/appendix_b_additional_results.tex
```

---

## 3. Active Overleaf folder structure

The project should follow this structure:

```text
/
├── main.tex
├── preamble.tex
├── commands.tex
├── README.md
├── AI_ASSISTANT_OVERLEAF_CONTEXT.md
├── ku-frontpage.sty
├── logos/
├── old/
│   └── archived pre-overhaul files only
├── frontmatter/
│   ├── abstract.tex
│   └── acknowledgements.tex
├── chapters/
│   ├── 01_introduction.tex
│   ├── 02_literature.tex
│   ├── 03_data.tex
│   ├── 04_empirical_strategy.tex
│   ├── 05_results.tex
│   ├── 06_robustness.tex
│   └── 07_conclusion.tex
├── appendices/
│   ├── appendix_a_data.tex
│   └── appendix_b_additional_results.tex
├── bibliography/
│   └── references.bib
└── outputs/
    ├── figures/
    │   ├── main/
    │   └── appendix/
    └── tables/
        ├── main/
        └── appendix/
```

Do not create new top-level folders unless there is a clear reason.

---

## 4. Root files and their roles

### `main.tex`

This is the controller file. It should contain:

```text
document class
input of preamble.tex
input of commands.tex
KU front-page metadata
front matter
chapter includes
appendix includes
bibliography call
```

Do not place ordinary thesis paragraphs in `main.tex`.

### `preamble.tex`

This contains package loading and document-wide formatting.

Use it for:

```text
encoding and language
KU front page package
geometry and spacing
math packages
figure packages
table packages
citation packages
hyperlinks
cross-referencing
chapter heading formatting
```

Do not load packages inside chapter files.

### `commands.tex`

This contains reusable commands and notation.

Examples:

```latex
\newcommand{\MainFigureWidth}{0.82\textwidth}
\newcommand{\WideFigureWidth}{0.95\textwidth}
\newcommand{\E}{\mathbb{E}}
\newcommand{\Ind}{\mathbb{1}}
\DeclareMathOperator{\Var}{Var}
\DeclareMathOperator{\Cov}{Cov}
```

### `bibliography/references.bib`

This is the single active bibliography file.

All sources cited with `\citet{}` or `\citep{}` should be listed here.

---

## 5. Chapter structure

The active thesis chapter order is:

```text
Chapter 1: Introduction
Chapter 2: Literature and Theoretical Background
Chapter 3: Data and AI Exposure Classification
Chapter 4: Empirical Strategy
Chapter 5: Results
Chapter 6: Robustness
Chapter 7: Conclusion
```

The corresponding files are:

```text
chapters/01_introduction.tex
chapters/02_literature.tex
chapters/03_data.tex
chapters/04_empirical_strategy.tex
chapters/05_results.tex
chapters/06_robustness.tex
chapters/07_conclusion.tex
```

When writing new material, paste it into the relevant chapter file, not into `main.tex`.

---

## 6. Appendix structure

The active appendix files are:

```text
appendices/appendix_a_data.tex
appendices/appendix_b_additional_results.tex
```

Use:

```text
appendices/appendix_a_data.tex
```

for supplementary data and classification material, including:

```text
descriptive AI exposure tables
top firms by AI exposure score
keyword-frequency tables
additional data-construction details
variable definitions
sample-selection details
data-source details
```

Current appendix data content includes:

```text
Top 20 firms by AI exposure score
Keyword frequency across all pre-2023 filings
```

Use:

```text
appendices/appendix_b_additional_results.tex
```

for supplementary empirical results, including:

```text
robustness tables
additional figures
alternative specifications
appendix regression output
sensitivity checks
extra result diagnostics
```

Do not keep an active standalone `appendix.tex` file from the old setup. If old appendix content is reused, move it into the relevant modular appendix file.

Appendix files should begin with a chapter command:

```latex
\chapter{Data Appendix}
\label{app:data}
```

or:

```latex
\chapter{Additional Results}
\label{app:additional-results}
```

Use appendix labels with the prefix:

```text
app:
```

Examples:

```latex
\label{app:data}
\label{app:additional-results}
```

Use regular section labels inside appendices:

```latex
\section{Descriptive results for AI exposure classification}
\label{app:ai-descriptive-results}
```

Appendix tables and figures should still use the same label conventions:

```latex
\label{tab:top20-ai-exposure}
\label{tab:keyword-frequency}
\label{fig:appendix-sample-coverage}
```

---

## 7. Front matter format

The front matter order is:

```text
KU front page
Abstract
Acknowledgements
Table of contents
List of figures
List of tables
Chapter 1
```

Use this structure for `frontmatter/abstract.tex`:

```latex
\chapter*{Abstract}
\addcontentsline{toc}{chapter}{Abstract}

Write the abstract here.
```

Use this structure for `frontmatter/acknowledgements.tex`:

```latex
\chapter*{Acknowledgements}
\addcontentsline{toc}{chapter}{Acknowledgements}

Write acknowledgements here.
```

Do not use numbered chapters for abstract or acknowledgements.

---

## 8. Sectioning rules

Use numbered sections for main thesis content.

Preferred hierarchy:

```latex
\chapter{Chapter Title}
\label{ch:chapter-label}

\section{Section Title}
\label{sec:section-label}

\subsection{Subsection Title}
\label{subsec:subsection-label}
```

Avoid `\section*{...}` for main thesis content. Unnumbered sections should only be used for front matter or special cases.

Do not manually format headings using bold text, large fonts, or spacing commands. Use LaTeX sectioning commands.

---

## 9. Citation and bibliography rules

The project uses `natbib`:

```latex
\usepackage[authoryear,round]{natbib}
```

The bibliography call at the end of `main.tex` should be:

```latex
\bibliographystyle{plainnat}
\bibliography{bibliography/references}
```

The bibliography should be alphabetized.

Use `\citet{}` when the author is part of the sentence:

```latex
\citet{jorda2005} introduces local projections.
```

Use `\citep{}` for parenthetical citations:

```latex
Local projections estimate impulse responses horizon by horizon \citep{jorda2005}.
```

Citation key convention:

```text
surnameYear
surnameSurnameYear
surnameSurnameSurnameYear
institutionYear
```

Examples:

```text
jorda2005
fama1970
shiller2000
pastorVeronesi2009
phillipsShiYu2015a
anteSaggu2025
mckinsey2025
```

Protect acronyms and proper nouns in `.bib` titles:

```bibtex
title = {Speculative Bubbles in the Recent {AI} Boom: {NASDAQ} and the Magnificent Seven}
```

Do not use long bibliographic footnotes for academic sources. Put the source in `references.bib` and cite it with `\citet{}` or `\citep{}`.

Short explanatory footnotes are acceptable only when they contain comments that do not belong naturally in the main text or bibliography.

---

## 10. Cross-reference rules

The project uses `cleveref`.

Use:

```latex
\cref{fig:ai-capex}
\Cref{fig:ai-capex}
\cref{tab:ai-exposure-summary}
\cref{eq:ai-score}
\cref{ch:data}
\cref{sec:ai-exposure-classification}
```

Use `\Cref{}` at the start of a sentence:

```latex
\Cref{fig:ai-capex} shows ...
```

Use `\cref{}` inside a sentence:

```latex
As shown in \cref{fig:ai-capex}, ...
```

Do not write manual references such as:

```latex
Figure 1 shows ...
Table 2 reports ...
Equation (3) implies ...
```

Use labels instead.

Label conventions:

```text
ch:chapter-name
sec:section-name
subsec:subsection-name
fig:figure-name
tab:table-name
eq:equation-name
app:appendix-name
```

Examples:

```latex
\label{ch:data}
\label{sec:ai-exposure-classification}
\label{subsec:data-keyword-selection}
\label{fig:ai-score-distribution}
\label{tab:ai-exposure-summary}
\label{eq:ai-score}
\label{app:data}
```

Labels should be lowercase and hyphenated. Avoid spaces, capital letters, and punctuation.

---

## 11. Figure workflow

All active thesis figures go in:

```text
outputs/figures/main/
outputs/figures/appendix/
```

Do not store active figures in:

```text
Pics/
old/
root folder
```

Recommended figure filenames:

```text
fig_descriptive_name.png
fig_descriptive_name.pdf
```

Examples:

```text
fig_mag7_sp500.png
fig_mag7_share_sp500.png
fig_pe_dotcom.png
fig_pe_ai.png
fig_nasdaq_price_return.png
fig_ai_capex.png
fig_ai_score_pre_post.png
fig_preferred_path_irf.pdf
```

Avoid filenames with:

```text
spaces
ampersands
parentheses
Danish characters
uppercase-heavy names
vague names such as figure1.png
```

Because `preamble.tex` includes:

```latex
\graphicspath{
  {outputs/figures/main/}
  {outputs/figures/appendix/}
}
```

figures can be included without writing the full path:

```latex
\includegraphics[width=\MainFigureWidth]{fig_mag7_sp500}
```

Do not include the file extension unless necessary.

Preferred figure format:

```latex
\begin{figure}[htbp]
    \centering
    \includegraphics[width=\MainFigureWidth]{fig_name}
    \caption{Short figure caption}
    \label{fig:short-label}
    \begin{minipage}{0.90\textwidth}
    \vspace{0.5em}
    \footnotesize
    \textit{Notes:} Add source, sample, and construction details here.
    \end{minipage}
\end{figure}
```

Use short captions. Put details about sources, samples, calculations, and construction in the notes below the figure.

For subfigures:

```latex
\begin{figure}[htbp]
    \centering
    \begin{subfigure}[t]{0.49\textwidth}
        \centering
        \includegraphics[width=\linewidth]{fig_first}
        \caption{First panel}
        \label{fig:first-panel}
    \end{subfigure}
    \hfill
    \begin{subfigure}[t]{0.49\textwidth}
        \centering
        \includegraphics[width=\linewidth]{fig_second}
        \caption{Second panel}
        \label{fig:second-panel}
    \end{subfigure}
    \caption{Short combined caption}
    \label{fig:combined-figure}
\end{figure}
```

---

## 12. Table workflow

All active generated tables go in:

```text
outputs/tables/main/
outputs/tables/appendix/
```

Small hand-written tables may be written directly in chapter files or appendix files.

Preferred hand-written table format:

```latex
\begin{table}[htbp]
\centering
\caption{Short table caption}
\label{tab:short-label}
\small
\begin{threeparttable}
\begin{tabular}{lcc}
\toprule
Variable & Mean & Std. dev. \\
\midrule
... \\
\bottomrule
\end{tabular}
\begin{tablenotes}[flushleft]
\footnotesize
\item \textit{Notes:} Add sample, variable definitions, and source details here.
\end{tablenotes}
\end{threeparttable}
\end{table}
```

Use `booktabs` rules:

```latex
\toprule
\midrule
\bottomrule
```

Do not use vertical table lines.

For generated `.tex` tables:

```latex
\begin{table}[htbp]
\centering
\caption{Short table caption}
\label{tab:short-label}
\input{outputs/tables/main/table_file_name}
\end{table}
```

or for appendix tables:

```latex
\begin{table}[htbp]
\centering
\caption{Short appendix table caption}
\label{tab:appendix-table-label}
\input{outputs/tables/appendix/table_file_name}
\end{table}
```

Do not paste large generated table code into the middle of a chapter if it can be included with `\input{}`.

---

## 13. Equation formatting

Use `equation` for numbered equations:

```latex
\begin{equation}
    y_t = \alpha + \beta x_t + \varepsilon_t.
    \label{eq:example}
\end{equation}
```

Reference equations using:

```latex
\cref{eq:example}
```

Do not write manual equation numbers in the text.

Use `align` for multi-line equations:

```latex
\begin{align}
    y_t &= \alpha + \beta x_t + \varepsilon_t, \\
    x_t &= \gamma z_t + u_t.
    \label{eq:example-system}
\end{align}
```

Do not over-format simple equations. Keep notation readable.

---

## 14. Writing style

Use formal academic English appropriate for an economics master’s thesis.

Preferred style:

```text
clear topic sentences
short-to-medium paragraphs
precise terminology
explicit links to research question
economic interpretation before technical detail when useful
careful distinction between evidence, interpretation, and speculation
```

Avoid informal phrases such as:

```text
we want to look at
this is exactly what we see
this could ease our minds
there are many routes we can take
none of them perfect
```

Use instead:

```text
the analysis examines
the figure suggests
this comparison implies
several measurement strategies are possible
each approach involves trade-offs
```

The thesis may use “we” if the project is jointly written, but use it sparingly. Prefer impersonal academic phrasing where it reads naturally.

---

## 15. How to handle old material

If old material is pasted into a new chapter or appendix, clean it before final use.

Checklist:

```text
Remove old \documentclass lines.
Remove old \usepackage lines.
Remove old \begin{document} and \end{document}.
Remove old \maketitle.
Replace old figure paths.
Rename old figure labels.
Convert Figure~\ref{} to \cref{} or \Cref{} where appropriate.
Convert long academic footnotes to \citep{} or \citet{}.
Move figure sources into notes under figures.
Remove manual line breaks such as \\ between paragraphs.
Replace \section*{} with numbered sections unless it is front matter.
Move old appendix.tex content into appendices/appendix_a_data.tex or appendices/appendix_b_additional_results.tex.
```

---

## 16. Overleaf performance and project hygiene

Keep Overleaf lightweight.

Do not upload:

```text
raw data
large .dta files
large .parquet files
Stata logs
Python notebooks
intermediate data
scratch outputs
uncompressed huge images
```

Only upload final thesis-ready figures and tables.

If figures make compilation slow:

```text
compress PNGs
use PDF for vector graphs
avoid unnecessarily high-resolution raster images
remove unused files
```

---

## 17. Compile and quality-control workflow

After major edits:

```text
1. Recompile normally.
2. If citations or references show question marks, use Recompile from scratch.
3. Search the PDF for "??".
4. Search for unresolved citation question marks.
5. Check List of Figures.
6. Check List of Tables.
7. Check appendix numbering and appendix table numbering.
8. Check that figures are readable.
9. Check that tables fit the page.
10. Check that the bibliography is alphabetical.
11. Check that no active content is pulled from old/.
12. Check that no active standalone appendix.tex is being compiled.
```

Do not continue adding large sections until the current version compiles cleanly.

---

## 18. Before sending a supervisor draft

Before sending a draft:

```text
1. Recompile from scratch.
2. Check all citations.
3. Check all cross-references.
4. Check all figures.
5. Check all tables.
6. Check appendix tables and appendix references.
7. Check figure and table notes.
8. Check that bibliography is alphabetized.
9. Export a dated PDF.
10. Save or download a project backup.
```

Recommended PDF filename:

```text
thesis_draft_YYYY-MM-DD.pdf
```

---

## 19. Before final submission

Before final submission:

```text
1. Confirm university formatting rules.
2. Confirm title page requirements.
3. Confirm abstract requirements.
4. Confirm page numbering rules.
5. Confirm all figures and tables are cited in the text.
6. Confirm all appendix figures and appendix tables are cited where relevant.
7. Confirm all citations appear in the bibliography.
8. Confirm no placeholder text remains.
9. Confirm no active files are stored only in old/.
10. Confirm no active standalone appendix.tex is being compiled.
11. Confirm appendix numbering works.
12. Confirm final PDF opens outside Overleaf.
13. Download Overleaf source as a zip.
14. Archive final PDF and source zip.
```

---

## 20. Expected response format from AI assistants

When asked to write or edit thesis content, the assistant should return:

```text
1. Where to paste the text.
2. Paste-ready LaTeX.
3. Required BibTeX entries, if any.
4. Required figure/table filename assumptions, if any.
5. A short compile/check note.
```

Example:

```text
Paste this into chapters/04_empirical_strategy.tex under \section{Outcome variable}:

[LaTeX block]

Add this to bibliography/references.bib:

[BibTeX block]

This assumes the figure file is stored as outputs/figures/main/fig_example.pdf.
```

The assistant should not return a new full LaTeX document unless explicitly asked.

---

## 21. Main principle

Prioritize a clean, stable, reproducible thesis setup over fancy formatting.

Use:

```text
one controller file
one preamble
one commands file
one bibliography file
modular chapter files
modular appendix files
clean figure and table folders
consistent labels
consistent citations
short captions with detailed notes
```

Do not overengineer the thesis layout.
