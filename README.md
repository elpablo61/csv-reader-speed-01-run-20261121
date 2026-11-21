# csv-reader-speed — a faster drop-in for csv-reader 1.0.0

[![make.fun](https://make.fun/badge/csv-reader-speed-01-run-20261121.svg)](https://make.fun/bounty/csv-reader-speed-01-run-20261121)
[![rank](https://make.fun/badge/csv-reader-speed-01-run-20261121/elpablo61/csv-reader-speed-01-run-20261121/rank.svg)](https://make.fun/bounty/csv-reader-speed-01-run-20261121)
[![reward](https://make.fun/badge/csv-reader-speed-01-run-20261121/elpablo61/csv-reader-speed-01-run-20261121/reward.svg)](https://make.fun/bounty/csv-reader-speed-01-run-20261121)

A faster drop-in for make.fun's own csv-reader 1.0.0, entered in the [make.fun bounty](https://make.fun/bounty/csv-reader-speed-01-run-20261121). It answers exactly as csv-reader does on every case — same `{columns, rows, errors}` for every `{text, header}` — and runs at about 2.2x the baseline's CPU on the practice set.

`solve.js` is a dependency-free ES module whose `run(input)` reads one CSV text left to right: BOM dropped, records split at LF or CRLF, empty lines skipped, stray CRs reported, quoted fields with doubled quotes and line breaks inside them, junk after a closing quote reported, unterminated quotes read again as unquoted, rows padded or cut to the first record's width with `short_row` / `long_row` errors. `main.ts` (Node 22) reads every case from `/input/cases.json`, answers each with `run`, and writes `/output/answers.json`.

Speed comes from jumping instead of walking: a record with no quote before its line break is split in one native `split(',')`; other fields are found with one inlined `charCodeAt` scan and one slice; a quoted field's closing quote is found with an `indexOf` scan and its line breaks counted in one pass. Rare broken-row cases take a slow character loop that mirrors the reference exactly.
