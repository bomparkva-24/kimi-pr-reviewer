# Kimi PR Reviewer

A small command-line tool that sends a Git diff and selected repository context to Kimi K3.1 Pro, then prints prioritized review findings as JSON.

It is an assistant for review, not a replacement for tests or human approval. Repository content is sent to CometAPI for processing, so review the files included before running it.

## Features

- Compares a base branch with the current working tree, including staged and unstaged changes
- Adds selected tracked source files as context, subject to a character budget
- Returns structured findings with file, line hint, severity, problem, and suggested fix
- Does not apply changes or approve merges

## Requirements

- Python 3.10+
- Git
- An available CometAPI key and access to `kimi-k3.1-pro`

## Setup

```bash
python -m pip install openai pydantic
export COMETAPI_KEY="your_api_key_here"
```

Do not commit API keys or sensitive repository content.

## Run

From the repository root:

```bash
python review.py main
```

This compares `main` with the current working tree. By default, it includes tracked files with selected source-code extensions, excluding common secret/config files.

Set `REVIEW_MAX_CHARS` to change the combined context limit. The script exits with a warning if the diff is empty or too large.

## Pricing

CometAPI currently lists Kimi K3.1 Pro at $1.60 per million input tokens, $8.00 per million output tokens, and $0.16 per million cached input tokens. Its sample estimate for a large code review is about $0.26 per run. Actual cost depends on repository context, diff size, output length, and cache use. Check the [model page and current pricing](https://www.cometapi.com/models/moonshotai/kimi-k3-1-pro/?utm_source=github&utm_medium=distribution&utm_campaign=bom&utm_content=kimi-k3-1-pro) before use.

## Limitations

- The repository context is a bounded selection of tracked source files, not the entire repository.
- The script does not inspect untracked files.
- A model can miss real issues or report false positives. Verify findings and run tests.
- The benchmark figures are Moonshot-published. Kimi K3.1 Pro reports 65.7% on Terminal-Bench 4.0; Moonshot’s comparison lists Claude Opus 5.5 at 66.4%.

## License

MIT

## Disclosure

This project links to CometAPI. If the link is affiliated or tracked and you may benefit from its use, disclose that clearly.
