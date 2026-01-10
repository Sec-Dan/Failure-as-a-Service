# Contributing to FAaS

Cheers for contributing!

## Adding new failure messages

Edit `data/messages.json` and add strings under the right category.

Guidelines:
- Keep it **workplace-safe** (no slurs, no harassment, no explicit content)
- Avoid brand-bashing real companies/people
- Keep it short enough to fit in a log line
- Try not to duplicate existing jokes

## Running checks

```bash
npm ci
npm run lint
npm run typecheck
npm test
```
