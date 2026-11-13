# History (append-only)

This folder is the permanent record of the climb. **Never edit or delete past entries**, only append. One file per month: `YYYY-MM.md`. A new file starts each month, so the log can run indefinitely.

## Entry format

Every question Claude asks gets one entry, in chronological order under a dated session heading:

```
## YYYY-MM-DD (Day), Session N: <short title>
Level at start: <ladder level>

### Q<n> [skill tag] <topic>
**Asked:** the question, with code if any
**Your answer:** verbatim, as given
**Result:** Correct | Partially correct | Incorrect | Not sure
**Correct answer / why:** the explanation
**Follow-up:** retry scheduled? new weak spot? (optional)
```

Skill tags (for tracking accuracy over time): `big-o`, `recursion`, `java-lang`, `java-collections`, `data-structures`, `pattern:<name>`, `dp`, `graphs`, etc.

Coding problems get an entry too: problem, attempts, hints used, cold or not, complexity stated, and what the user got wrong.

## Session footer

Each session ends with:
```
**Session summary:** duration, what was covered, wins, mistakes
**Ladder:** <level before> -> <level after> (or "no change")
```
