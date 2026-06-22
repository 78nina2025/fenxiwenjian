# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Repository nature

This is **not a code repository**. It is a knowledge-extraction project over a personal library of ~590 Chinese-language stock-trading books. There is no build, lint, test, or runtime — work consists of reading source books and appending structured rules to the digest files.

## Layout

- **Root (`*.pdf`, `*.doc`)** — original book files. Filenames are Chinese book/article titles (e.g. `炒股就是炒盘口 两星期炼成盘口实战高手.pdf`). Treat as read-only sources.
- **`_txt/`** — plain-text extractions of the same books (~274 files, e.g. `MACD指标精粹讲解.txt`). Use these for grepping/reading content; do not regenerate them unless asked.
- **`规则汇总.md`** — primary digest. Single Markdown table, rows numbered 1–8300.
- **`规则汇总2.md`** — continuation. Same schema, numbering resumes at 8301. New rules append here, not to `规则汇总.md`.

When the user asks to add rules from a book, find the source in `_txt/` first (faster than the PDF), extract rules, append rows to `规则汇总2.md` continuing from the last `序号`.

## Rules-table schema

Both digest files share a 10-column table. Every new row must use exactly these columns and follow the existing conventions — schema drift is the main failure mode here.

| Column | Notes |
|---|---|
| 序号 | Monotonic integer, continues across files. Check `tail` of `规则汇总2.md` before adding. |
| 大分类 | Top category, e.g. `量价关系`, `K线形态`, `波浪战法`, `心法战法`. Reuse existing values when possible — grep before inventing a new one. |
| 子类 | Sub-category under 大分类. |
| 规则名称 | Short rule label. |
| 多空方向 | One of: `多`, `空`, `多空`, `观望`, `中性`, `反向`, or qualified forms like `多·买点`, `空·卖点`, `多·选股`, `多空·待定`. |
| 触发条件 | Concrete trigger conditions, semicolons (`;`) separate clauses. |
| 操作建议 | Action to take. |
| 适用级别 | `短线` (intraday–days) / `中线` (weeks–months) / `长线` (months+); also `日内`, `中短线`, `各级`. |
| 强度等级/可靠性 | `★` weakest → `★★★★★` classic/always-present. The header note in each file defines this scale. |
| 原书和章节 | Source citation in `《书名》章节` form. If a rule appears in multiple books, join with `；同见` (see existing rows for examples). |

The header block (lines 1–7) of each digest defines `适用级别` and 强度等级 — preserve it verbatim when editing.

## Conventions to preserve

- Chinese punctuation throughout (`、`, `《》`, `：`); ASCII `;` is used as the in-cell clause separator inside `触发条件` — do not "normalize" to Chinese `；`.
- Rules are deduped by meaning, not wording. Before adding a rule, grep `规则汇总.md` and `规则汇总2.md` for the source book name and key trigger terms; if a rule already exists, add the new source to the existing row's `原书和章节` via `；同见《...》` rather than creating a duplicate row.
- Books cited multiple times reuse the same `《书名》` spelling — match existing citations exactly (grep before inventing).
- File encoding is UTF-8. Some `_txt/` files have noisy OCR; quote sparingly and prefer paraphrase in `触发条件`/`操作建议`.

## Working with the source files

- Prefer `_txt/` over PDFs for reading and grepping. PDFs at root are large (tens of MB each) and slow to load.
- Filenames contain spaces and Chinese characters — always quote paths.
- The empty file `规突破真假》` at the root is a stray fragment of a filename; ignore unless the user mentions it.
