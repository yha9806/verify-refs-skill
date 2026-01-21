# verify-refs

A Claude Code skill for academic reference authenticity verification.

## Features

- **Multi-source Cross-validation**: CrossRef + Semantic Scholar + arXiv API
- **BibTeX Parsing**: Embedded markdown and standalone `.bib` files
- **Anomaly Detection**: Red/Yellow/Green flag system
- **Self-citation Consistency**: Author name variant checking
- **Automated Reports**: Markdown reports with JSON metrics

## Installation

Copy the skill files to your superclaude directory:

```bash
# Main skill file
cp commands/verify-refs.md ~/.claude/superclaude/commands/

# Pattern library
cp shared/academic-verification-patterns.yml ~/.claude/superclaude/shared/
```

## Usage

```bash
# Basic verification
/verify-refs references/papers.md

# Quick scan (DOI/arXiv existence only)
/verify-refs --quick --bib paper.bib

# Deep verification with report
/verify-refs --deep --arXiv --report

# Auto-fix and filter recent papers
/verify-refs --fix --recent 2024
```

## Flags

| Flag | Description |
|------|-------------|
| `--deep` | Multi-source cross-validation |
| `--quick` | Fast mode: DOI/arXiv existence only |
| `--bib` | Parse standalone .bib file |
| `--report` | Generate detailed report to `.claudedocs/refs/` |
| `--fix` | Auto-fix correctable issues |
| `--arXiv` | Extended arXiv validation |
| `--recent` | Filter by year (e.g., `--recent 2024`) |
| `--self-cite` | Verify self-citation consistency |
| `--url-check` | Verify URL accessibility |
| `--strict` | Zero-tolerance mode |

## Verification Pipeline

1. **Parse & Extract**: BibTeX parsing → field extraction → metadata normalization
2. **Structure Validation**: Required fields → format check → duplicate detection
3. **API Cross-Verification**: CrossRef → Semantic Scholar → arXiv → URL check
4. **Anomaly Detection**: Author drift → year mismatch → title similarity → self-cite check
5. **Report Generation**: Statistics → findings → severity classification → recommendations

## Anomaly Flags

### Red Flags (Critical)
- DOI not found in CrossRef
- Author count mismatch > 50%
- Year difference > 2
- Title similarity < 60%
- arXiv ID format invalid

### Yellow Flags (Warning)
- No DOI provided (non-arXiv)
- URL returns non-200
- Minor title variation (60-90%)
- Venue abbreviation mismatch

### Green Flags (Verified)
- DOI verified via CrossRef
- All required fields present
- Author/Title/Year match source

## API Rate Limits

| API | Rate Limit | Priority |
|-----|------------|----------|
| CrossRef | 50 req/sec (polite pool) | 1 |
| Semantic Scholar | 100 req/5min | 2 |
| arXiv | 3 sec between requests | 3 |

## Output

Reports are saved to `.claudedocs/refs/`:
- `verification-{timestamp}.md` - Detailed report
- `metrics-{date}.json` - JSON metrics
- `fixes-{timestamp}.md` - Fix log (when `--fix` used)

## License

MIT License

## Author

Created for the superclaude ecosystem.
