# Examples

This directory contains example files for testing the `/verify-refs` skill.

## Files

| File | Description |
|------|-------------|
| `sample_papers.md` | Markdown file with embedded BibTeX blocks |
| `sample.bib` | Standalone BibTeX file |
| `sample_report.md` | Example verification report output |

## Usage Examples

### Basic Verification

```bash
# Verify the sample markdown file
/verify-refs examples/sample_papers.md
```

### Quick Scan

```bash
# Fast check - only DOI/arXiv existence
/verify-refs --quick examples/sample_papers.md
```

### Standalone BibTeX

```bash
# Parse and verify a .bib file
/verify-refs --bib examples/sample.bib
```

### Deep Verification with Report

```bash
# Full verification with CrossRef + arXiv APIs, generate report
/verify-refs --deep --arXiv --report examples/sample_papers.md
```

### Auto-fix Mode

```bash
# Attempt to fix correctable issues
/verify-refs --fix examples/sample_papers.md
```

### Filter by Year

```bash
# Only verify papers from 2023 onwards
/verify-refs --recent 2023 examples/sample_papers.md
```

### Strict Mode

```bash
# Fail on any unverified entry
/verify-refs --strict examples/sample_papers.md
```

## Expected Results

When running `/verify-refs examples/sample_papers.md`, you should see:

- **5 verified entries** (gpt4_2023, transformer2017, bert2019, my_work2025, and 1 more)
- **2 red flags** (missing_year_example, bad_arxiv)
- **1 yellow flag** (no_url_example)

See `sample_report.md` for an example of the generated report.

## Testing Anomaly Detection

The sample files include intentional issues to test detection:

| Issue Type | Entry | Expected Flag |
|------------|-------|---------------|
| Missing year | `missing_year_example` | 🚨 Red |
| No identifier | `no_url_example` | ⚠ Yellow |
| Invalid arXiv | `bad_arxiv` | 🚨 Red |
| Incomplete | `incomplete_entry` | 🚨 Red |
