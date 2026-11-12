# Facts: one place for every number, date, status and style rule

`facts/facts.toml` is the only place to type a number, a date or the LAK paper's status.
Everything else either reads it (the CVs) or is checked against it (statements, letters,
form answers, email drafts).

## Daily use

```
python3 facts/factsync.py all --rebuild
```

That one command does four things:

1. **build**: regenerates `modules/facts.tex` (48 LaTeX macros) and `facts/FACTS.md` (a cheat sheet to copy numbers from).
2. **fix**: rewrites the LAK status in prose files that cannot use macros, for example "is under review at LAK 2027" becomes "has been accepted at LAK 2027".
3. **check**: lists anything out of date and exits with status 1 if it finds one:
   - a retired number, such as F-measure 0.75, NDCG@10 0.81, 0.775 / 0.853 or "8 of top 10";
   - a stale LAK status phrase;
   - a number typed by hand into a CV instead of its macro.

   Add `--style` to also list prose style slips (em dashes, stock AI words, "not X but Y").
4. **rebuild**: recompiles every CV and statement listed in `[scan].rebuild` and prints page counts. Compare them with the expected counts in the TOML, then make sure no entry is split across a page break.

## When something changes

| Change | What to do |
|---|---|
| LAK decision (24 Nov 2026) | Set `[lak].status` to `"accepted"` (or `"rejected"`), then run `all --rebuild`. CVs switch through macros and prose through `fix`. For a rejection, `check` lists each mention to rewrite by hand. |
| A new number (new result, new metric at work) | Add or edit it in the TOML. In a CV, write the macro (`\factLakMerge`), never the number. |
| A number we stop using | Add a `[[retired]]` entry with the reason, so `check` catches it everywhere. |
| A new CV or statement | Add it to `[scan].rebuild`; if it is a CV, add its path to `[scan].cv_files`. A standalone CV needs this line after `\documentclass`: `\makeatletter\def\input@path{{modules/}{../modules/}{../../modules/}}\makeatother\input{facts}` |
| An email has been sent | Add its file to `[scan].exclude`. Sent text is a record, so `fix` must not rewrite it. |

## Macro names

Every key in `[edu]`, `[exp]`, `[trace]` and `[lak]` becomes `\fact` + table + key in CamelCase:
`[lak] merge` is `\factLakMerge`, and `[exp] codestudio_uptime` is `\factExpCodestudioUptime`.
The status phrases are `\factLakAt`, `\factLakIs`, `\factLakNow` and `\factLakParen`. Write `{}` after a
macro that comes before a space (`\factLakIs{} and`). The full list is in `FACTS.md`.

## Style rules (all CVs, statements and emails)

Writing:
- Run the humanizer skill on summaries, research prose, statements and emails.
- No em dashes or en dashes in prose. Avoid "not X but Y", "Outcome:" labels and stock AI words (delve, pivotal, showcase, testament, underscore).
- Bullets lead with the result or number, then how: "Raised client response rates by 30% by leading...".
- Experience is "3+ years" in CVs and "more than three years" in prose. Never "almost 4" or "nearly four".
- TRACE results come only from the submitted LAK 2027 paper (`[lak]`). Never use the ICDM or thesis-table numbers.
- Spell out Rajshahi University of Engineering & Technology (RUET) at its first mention in each document.
- In CVs, summaries carry no numbers unless they matter; job CVs keep "3+ years".

CV layout:
- No title or location under the name. The academic CV also has no phone, and its links sit on one line.
- No References section, except in the DBBL bank CV and the BD lecturer CV.
- Section order:
  - job CVs: Summary, Experience, Skills, Projects, Education, Certifications;
  - academic and lecturer CVs: Summary, Education, Research, Experience;
  - the research job CV: Research right after Summary.
- Experience order: Aamira, Code Studio, Outlier, Brain Station. Outlier is hidden in software and job CVs.
- "Selected Projects", with Omnious first. Project titles use a bar (`Omnious \textbar\ Visual Debugging Platform`). Certificate lines keep `---`.
- Bold only for headings and titles, never inside a bullet. Italic only when needed.
- Research bullets have no supervisor line and follow the story order: question, what was built, test, causal check, outcome.
- Academic education shows the CGPA and key courses only.
- After any edit, rebuild and check page counts. The lecturer CV fills exactly 2 pages, and the Brock research form must stay on 1.
