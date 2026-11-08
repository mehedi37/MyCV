# Master CV

`mehedi_master.tex` loads every module in `../modules/` (all experience entries,
all four projects, research, education with both options, the full certificate
list) plus the alternative summaries as comments. Use it to see all content in
one PDF. It runs to 3 pages on purpose; do not submit it.

The shared text lives in `../modules/`, not here. See `../modules/README.md`.

## Make a new tailored CV

1. Copy the closest existing CV, e.g. `cp lang_based/ai/mehedi_ai.tex lang_based/<domain>/mehedi_<name>.tex`.
2. Write the Summary for the role.
3. Choose modules: comment out or add `\input{...}` lines in Experience and Projects.
4. Use `\input{skills/software}` or write tailored skill rows inline.
5. Compile in that folder: `latexmk -xelatex mehedi_<name>.tex`, then check
   page count and that no entry is split across pages.
