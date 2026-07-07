# papers/ — input

Drop the manuscript you want refereed here (a `.pdf`, or a `.tex` / `.qmd` source), then run:

```
/referee <filename>
```

`/referee` resolves the argument against a direct path first, then `papers/<filename>`, then a glob match. With no argument it lists the PDFs here and asks which one.

The generated report is written to `referee_reports/`.

> PDFs placed here are **not** committed by default (see `.gitignore`) — manuscripts under review are usually confidential. Remove that ignore rule if you want to track example papers in the repo.
