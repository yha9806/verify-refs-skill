**Purpose**: Academic reference authenticity verification and validation

---

@include shared/universal-constants.yml#Universal_Legend

## Command Execution
Execute: immediate. --plan→show plan first
Legend: Generated based on symbols used in command
Purpose: "[Verify][References] in $ARGUMENTS"

Perform comprehensive authenticity verification on academic references with multi-source cross-validation.

@include shared/flag-inheritance.yml#Universal_Always

Examples:
- `/verify-refs references/key_papers.md` - 验证文献清单
- `/verify-refs --quick --bib paper.bib` - 快速扫描 BibTeX
- `/verify-refs --deep --arXiv --report` - 深度验证 + arXiv 重点
- `/verify-refs --fix --recent 2024` - 自动修复 + 过滤近期文献

## Command-Specific Flags
--deep: "多源交叉验证 (CrossRef + Semantic Scholar + arXiv)"
--quick: "快速模式: 仅 DOI/arXiv 存在性检查"
--bib: "解析独立 .bib 文件"
--report: "生成详细报告到 .claudedocs/refs/"
--fix: "自动修复可修复问题 (URL更新、格式修正)"
--arXiv: "扩展 arXiv 验证 (格式+存在性+版本)"
--recent: "按年份过滤 (如 --recent 2024)"
--self-cite: "验证自引一致性"
--url-check: "验证 URL 可访问性"
--strict: "零容忍模式: 任何未验证条目即失败"

## Verification Pipeline

**Phase 1 - Parse & Extract:**
BibTeX解析→字段提取→元数据规范化→条目分类 (★★★/★★)

**Phase 2 - Structure Validation:**
必需字段检查→格式验证→编码检查→重复检测

**Phase 3 - API Cross-Verification:**
CrossRef DOI查询 (主)→Semantic Scholar (备)→arXiv API (预印本)→URL可达性

**Phase 4 - Anomaly Detection:**
作者名漂移→年份不一致→标题不匹配→自引验证→缺失DOI警告

**Phase 5 - Report Generation:**
统计概览→详细发现→严重程度分类→修复建议

## API Sources & Rate Limits

```yaml
CrossRef_API:
  Endpoint: "https://api.crossref.org/works/{doi}"
  Rate: "50 req/sec (polite pool)"
  Priority: 1
  Headers: "User-Agent: RefVerifier/1.0 (mailto:user@example.com)"

Semantic_Scholar:
  Endpoint: "https://api.semanticscholar.org/v1/paper/{id}"
  Rate: "100 req/5min"
  Priority: 2

arXiv_API:
  Endpoint: "http://export.arxiv.org/api/query"
  Interval: "3 sec between requests"
  Priority: 3
```

## Anomaly Detection Rules

**🚨 Red Flags (Critical):**
- DOI not found in CrossRef
- Author count mismatch > 50%
- Year difference > 2
- Title similarity < 60%
- arXiv ID format invalid
- Self-cite author mismatch

**⚠ Yellow Flags (Warning):**
- No DOI provided (non-arXiv)
- URL returns non-200
- Minor title variation (60-90% similarity)
- Venue abbreviation mismatch
- Missing optional fields

**✅ Green Flags (Verified):**
- DOI verified via CrossRef
- All required fields present
- Author/Title/Year match source

## Verification Logic

### BibTeX Parsing
```
1. 检测输入格式 (embedded markdown vs standalone .bib)
2. 使用正则提取 BibTeX 块: ```bibtex ... ```
3. 提取优先级标记: ★★★ / ★★
4. 解析字段: title, author, year, doi, arxiv_id, url, venue
5. 规范化作者名: "Last, First" 格式
6. 提取 arXiv ID: 从 journal 或 url 字段
```

### CrossRef Verification
```
请求: GET https://api.crossref.org/works/{doi}
验证点:
- title 相似度 (Levenshtein / Jaccard)
- author 数量匹配
- year 一致性
- venue 名称
返回: {status, title, authors[], year, venue}
```

### arXiv ID Validation
```
New Format: ^\d{4}\.\d{4,5}(v\d+)?$  (e.g., 2301.12345v2)
Old Format: ^[a-z-]+/\d{7}(v\d+)?$  (e.g., cs/0012001)
```

## EMNLP 2026 Project Rules

```yaml
Citation_Format: "Markdown 嵌入 BibTeX 块"
Priority_Markers:
  Must_Cite: "★★★ 必引"
  Optional: "★★ 选引"
Self_Citation_Keys:
  - "VULCA-Bench (vulcabench2025)"
  - "VULCA Framework (vulca2025)"
  - "火意象 (fire_imagery2025)"
  - "Art Critique (art_critique2026)"
Author_Variants:
  - "Yu, Haorui"
  - "Haorui Yu"
  - "H. Yu"
L1_L5_Attribution: "VULCA-Bench 框架"
```

## Self-Citation Consistency Check

```
检查自引条目:
- vulcabench2025: VULCA-Bench
- vulca2025: VULCA Framework
- fire_imagery2025: 火意象研究
- art_critique2026: Art Critique

验证:
- 作者名变体一致性
- L1-L5 框架正确归属于 VULCA-Bench
- 年份与实际发表匹配
```

## Output Report Template

```markdown
# 文献验证报告
生成时间: {timestamp}
目标文件: {file_path}

## 统计概览
| 指标 | 数值 |
|------|------|
| 总条目 | {total} |
| ✅ 已验证 | {verified} |
| ⚠ 警告 | {warnings} |
| 🚨 错误 | {errors} |

## 🚨 关键问题
{critical_issues}

## ⚠ 警告
{warnings_list}

## ✅ 已验证条目
{verified_list}

## 修复建议
{recommendations}
```

## Deliverables

**Reports:** `.claudedocs/refs/verification-{timestamp}.md`
**Metrics:** `.claudedocs/refs/metrics-{date}.json`
**Fix Log:** `.claudedocs/refs/fixes-{timestamp}.md` (when --fix used)

@include shared/academic-verification-patterns.yml#Verification_Workflow
@include shared/academic-verification-patterns.yml#API_Rate_Limits

@include shared/research-patterns.yml#Mandatory_Research_Flows
@include shared/quality-patterns.yml#Validation_Sequence
@include shared/universal-constants.yml#Standard_Messages_Templates
