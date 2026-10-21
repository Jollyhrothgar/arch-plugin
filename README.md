# arch

A Claude Code plugin that investigates a software system and produces a
source-grounded, navigable HTML explanation of its architecture.

| Skill | What it does |
| --- | --- |
| `/arch:visualize` | Explain a system's architecture as a navigable HTML page |

It is about a kind of work rather than about you, so install it for the
projects that need it:

```bash
claude plugin marketplace add Jollyhrothgar/personal_ai_config
claude plugin install arch@jollyhrothgar -s project
```

To try a working copy without installing: `claude --plugin-dir .`
