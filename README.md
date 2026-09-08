# Content Writing Pipeline

A skill for researching, drafting, editing, and refreshing articles, SEO pages, emails, newsletters, social posts, documentation, and other written content. Adapted from [SEO Page Builder](https://github.com/octolens/seo-page-builder).

## How it works

**Scope → Research → Gather evidence → Verify → Write and edit → Prepare delivery → QA**

The original seven-stage pipeline adapts to the assignment: lightweight for a short email, thorough for a researched comparison. SEO research and site integration run when relevant. A natural-voice editing pass removes generic phrasing while preserving meaning and voice.

## Optional integrations

| Tool | Function | Configuration |
|---|---|---|
| Perplexity API | Discover sources, audience experiences, and questions; synthesize research and verify claims against originals | `PERPLEXITY_API_KEY` |
| Ahrefs / DataForSEO | Keyword metrics, rankings, and SERP research | Provider API credentials |
| [watermarks-remover](references/text-cleanup.md) | Inspect Unicode artifacts, clean a separate copy, and review changes before QA | `WATERMARKS_REMOVER_DIR` pointing to a trusted checkout |

Integrations are optional and documented, not bundled or automatically installed. Text cleanup does not prove removal of statistical watermarks or guarantee AI-detector results.

## Install and use

For Claude Code:

```bash
mkdir -p .claude/skills
git clone https://github.com/kriiv/content-writing-pipeline .claude/skills/content-writing-pipeline
```

Then request a writing task or invoke `content-writing-pipeline` explicitly:

- “Turn these notes into a concise customer email.”
- “Edit this newsletter while preserving my voice.”
- “Build a researched product comparison with verified pricing.”

See [SKILL.md](SKILL.md) for the full workflow. Deliverables default to drafts; publishing or sending requires explicit authorization.

## License

MIT
