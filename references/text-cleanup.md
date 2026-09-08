# Optional local text cleanup

Use [watermarks-remover](https://github.com/guillaumemeyer/watermarks-remover) as an optional step between drafting and final QA. This integration uses its local Python text scripts; no HTTP service, model backend, global hook, or metadata-stripping workflow is needed.

## Setup

Use an existing trusted checkout or obtain a reviewed release of the upstream repository. Set `WATERMARKS_REMOVER_DIR` to the checkout's absolute path. Record its release or commit when reproducibility matters. Do not automatically install its plugin, run its installer, or track upstream updates during writing tasks.

Resolve scripts from `service/scripts` in that checkout. Verify the installed scripts' `--help` before use because upstream flags can change. Python 3.10+ is required by upstream. This skill does not bundle upstream code or install the dependency.

## Run after the natural-voice pass

1. Inspect the requested text file:

   ```bash
   python3 "$WATERMARKS_REMOVER_DIR/service/scripts/inspect_text.py" draft.md
   ```

2. Review the reported characters in context. An unusual or invisible character is not proof of AI origin. Preserve intentional typography, script joiners, directionality, emoji sequences, and layout spaces. Leave exact quotations, code, URLs, identifiers, formulas, and HTML attributes unchanged. If findings touch protected spans, clean only separately extracted prose or leave the findings unchanged; the text cleaner is not a Markdown/HTML parser.

3. When whole-file cleanup is appropriate, write to a new, unused output path:

   ```bash
   python3 "$WATERMARKS_REMOVER_DIR/service/scripts/clean_text.py" draft.md -o draft.cleaned.md --stats --no-normalize-spaces
   ```

   Do not enable aggressive homoglyph mapping, NFKC normalization, bidi stripping, or emoji-glue stripping as defaults. Never pass PDF, DOCX, or other binary files to these text scripts, and never override their binary-input guard.

4. Inspect the result and compare it with the original:

   ```bash
   python3 "$WATERMARKS_REMOVER_DIR/service/scripts/inspect_text.py" draft.cleaned.md
   git diff --no-index -- draft.md draft.cleaned.md
   ```

   For `git diff --no-index`, exit code 1 means differences were found. Review every change, including escaped code points when invisible changes are unclear. Keep the original and accept the cleaned copy only if meaning, protected spans, formatting, and legitimate language characters are preserved. Otherwise retain the original or apply only individually justified corrections.

5. Continue Phase 6 QA on the accepted artifact. If another writing pass changes it, inspect the final saved version again. Do not claim a deterministic filter was applied to chat-only output.

## Rewriting and reporting

The pipeline's existing natural-voice pass is the prose-editing step. Do not optimize drafts against stylometry scores or repeatedly paraphrase them by default. Those scores measure selected style features, not a vendor's secret-key watermark. Additional rewriting requires preserving all factual claims, quotes, citations, and the requested voice, followed by another verification pass.

Report concrete results when relevant: artifacts found, characters removed or replaced, protected spans left unchanged, and any skipped checks. Do not claim that clean Unicode, a lower style score, or a rewritten passage proves watermark removal, human authorship, or acceptance by an AI detector.

Upstream interfaces reviewed: [text cleaner](https://github.com/guillaumemeyer/watermarks-remover/blob/main/service/scripts/clean_text.py) and [text-only workflow](https://github.com/guillaumemeyer/watermarks-remover/tree/main/skills/clean-user-facing-text). This is a small integration guide, not an adoption of every upstream writing rule.
