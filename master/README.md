# Master CV

`mehedi_master.tex` is the single source of truth: every Experience/Project/Research
bullet from all `lang_based/` variants (ts_js, python, dot_net, optimizely, ai,
research) lives here in one file, tagged by which target it fits. It will overflow
one page — that's expected, it's a bank to cut down from, not a resume to submit
as-is.

## Tailor a new CV in ~5 minutes

1. Copy it: `cp master/mehedi_master.tex lang_based/<domain>/mehedi_<name>.tex`
2. **Summary** — uncomment the one paragraph matching the role, delete the rest.
3. **Skills** — trim rows so the job description's top keywords sit in the first
   line (ATS parsers and human skimmers both weight position).
4. **Experience / Projects / Research** — every subsection ends with a
   `% TAG: [...]` comment saying which stack(s) it belongs to (`ALL`, `TS/JS`,
   `PYTHON`, `DOTNET ONLY`, `AI/ML`, `OPTIONAL`, etc). Delete or comment out
   anything whose tag doesn't match the target role.
5. Compile: `latexmk -pdf -xelatex mehedi_<name>.tex`
6. Confirm it's exactly 1 page. If it's not, cut another bullet — don't shrink
   fonts or margins to force a fit.

CI auto-compiles any `*.tex` pushed to `main` (see `.github/workflows/`), so once
committed the PDF is generated automatically.

## Section-tag legend

- `[ALL]` — keep in every variant unless space-constrained
- `[TS/JS]`, `[PYTHON]`, `[DOTNET ONLY]`, `[AI/ML, RESEARCH]` — stack-specific,
  include only when the target role matches
- `[OPTIONAL]` — nice-to-have, first thing to cut under space pressure
