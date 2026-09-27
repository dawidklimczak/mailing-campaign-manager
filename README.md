# Mailing Campaign Manager

An internal tool for managing email marketing campaigns in SALESmanago, with AI-assisted copywriting and a feedback-driven prompt optimizer.

## Problem

Running email campaigns through SALESmanago's own UI is slow for repetitive tasks: building HTML templates, keeping track of sender accounts and campaign groups, and doing bulk contact tagging. On top of that, writing on-brand marketing copy for every campaign by hand doesn't scale. This tool wraps the SALESmanago API in a purpose-built internal UI and adds an AI copywriting layer so campaigns can be drafted, tagged, and sent faster and more consistently.

## Solution & Architecture

- **Streamlit** multi-page app as the internal UI (dashboard, template editor, tag "recipes", sender/group management)
- **SQLite** for local persistence of templates, sender accounts, campaign groups, and reusable tag-merge recipes
- **SALESmanago API integration**: authenticated HTTP calls (SHA-based signing) to create/update email templates, run async contact-export jobs, and batch-tag contacts in chunks
- **Google Gemini** integration for two AI-assisted workflows: generating campaign copy from source material and style guides, and a separate "optimizer" pass that improves the prompts themselves

## Key Product Decisions

- **Composable AI inputs**: copy generation is driven by a "Task" (what to write) and a "Communication Profile" (how to write it), kept as separate, reusable pieces rather than one big prompt — closer to how a real editorial brief is structured.
- **A self-optimizing prompt loop**: when generated copy misses the mark, the user can describe what's wrong; a dedicated optimizer model analyzes the Task/Profile pair against that feedback and proposes minimal, explained edits back to the source prompts — instead of just regenerating output, it improves the instructions that produced it.
- **Async job handling for bulk tagging**: SALESmanago's contact-export is an async job; the tag-merge feature polls for completion and processes results in bounded batches (1,000 contacts per call) to stay within API limits.
- **Local data ownership**: campaign templates and account/group config live in a local SQLite database rather than being re-fetched from SALESmanago each time, so the tool stays fast and usable offline for editing.

## Outcome

Turned a handful of manual, repetitive SALESmanago workflows (template creation, sender/group bookkeeping, tag consolidation) into a single internal app, and added an AI copy-drafting step with a built-in feedback loop for iterating on prompt quality over time.

## Preview

<!-- screenshot placeholder — added separately -->

## Note

Source code is kept in a private repository; happy to share on request.
