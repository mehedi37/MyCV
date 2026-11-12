# CV modules

Shared building blocks for the modular CVs. Edit a module once and every CV that
`\input`s it picks up the change the next time it is compiled.

```
modules/
├── facts.tex             GENERATED from facts/facts.toml: every number, date and the
│                         LAK status as macros (\factLakMerge, \factExpYears, \factLakIs ...).
│                         Never type those values into a CV; see facts/README.md
├── preamble.tex          packages, colors, section style, page-break rules,
│                         contact details (\cvname, \cvphone, ...) and the two headers
├── experience/           aamira, code_studio, outlier, brainstation, rapl
├── projects/             omnious, studentai, pharmac, thesis_site
├── research/trace.tex    B.Sc. thesis entry
├── education/ruet.tex    B.Sc. entry (options below)
├── certifications.tex    certificate and contest list (options below)
└── skills/software.tex   default software skills (AI/ML row first)
```

## How a CV uses them

Every modular CV starts with the same four lines, so it finds `modules/` from the
repo root, `lang_based/<x>/`, `master/` or `SOP/<x>/`:

```latex
\documentclass[10pt, a4paper]{article}
\makeatletter\def\input@path{{modules/}{../modules/}{../../modules/}}\makeatother
\input{preamble}
\hypersetup{pdfsubject={...}, pdfkeywords={...}}   % optional, per CV
```

Then the body picks a header and the modules it needs:

```latex
\begin{document}
\color{primary}
\cvheader                     % or \cvheaderacademic (no phone, links on one line)
\section{Summary}
...summary written for this CV...
\section{Experience}
\input{experience/aamira}
\input{experience/code_studio}
% \input{experience/outlier}  % only in academic/lecturer CVs
\input{experience/brainstation}
\section{Selected Projects}
\input{projects/omnious}      % Omnious first when included
\input{projects/studentai}
\section{Skills}
\input{skills/software}       % or write tailored rows inline
\section{Education}
\def\eduthesisline{}          % adds the thesis line (job CVs without a Research section)
% \def\educoursework{}        % adds key coursework (academic CVs)
\input{education/ruet}
\section{Certifications \& Achievements}
\def\certindustry{}           % adds the industry certificates (job CVs)
\input{certifications}
\end{document}
```

Per-CV tweaks go right after `\input{preamble}`, for example
`\geometry{margin=0.5in}` and `\setlength{\parskip}{0.25em}` in the academic CV.

## Which CVs are modular

- Modular: `Mehedi_CV.tex`, `lang_based/{ts_js,brainstation,optimizely,ai,python,dot_net,research}`,
  `master/mehedi_master.tex` (reference that loads every module) and `SOP/academic_cv/` (local only, SOP/ is gitignored).
- Standalone on purpose: `lang_based/{google,navvis,lecturer,banks}`. They use
  their own formats (one-page Google layout, NavVis job tailoring, Bangladeshi
  lecturer and bank formats with tables, personal info and references), so they
  keep their own text.

## Building

There is no CI any more; compile locally and upload the PDF yourself:

```
cd lang_based/ai && latexmk -xelatex mehedi_ai.tex
```

After changing a module, rebuild every CV that uses it and check page breaks
(an entry or heading should never be split across pages).
