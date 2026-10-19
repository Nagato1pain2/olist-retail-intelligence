# Team Contribution Guide

**Team:** Mahmoud and Ahmed. Both authors contribute under individual GitHub accounts.

## Branching
- Protected production branch: `main`
- Mahmoud's first task: `feature/business-mahmoud`
- Ahmed's first task: `feature/business-ahmed`
- Later tasks: `feature/<short-topic>-<person>`

Do not commit directly to `main` once protection is enabled.

## Pull request routine
1. Open an issue or describe the objective.
2. Create a new feature branch from the latest `main`.
3. Make small, meaningful commits.
4. Open a Pull Request with what changed, the reason, evidence, and any open questions.
5. Assign the other teammate as reviewer. Discuss changes and revise.
6. Merge after review; remove the merged branch when appropriate.

### Commit message examples
- `docs: add Mahmoud business questions`
- `docs: clarify delivery KPIs after review`
- `chore: configure Colab environment notebook`
- `feat: add initial orders data audit`
- `fix: correct order-grain aggregation`

## Google Colab
Avoid editing the same notebook simultaneously. Work on separate task files or separate branches.
A saved notebook is a Git commit only when the changes are actually committed/pushed to GitHub.
Do not assume changes in Google Drive automatically appear in the repository.

## Review checklist
- Is the objective understandable and testable?
- Are metric definitions explicit (formula, grain, filtering)?
- Are assumptions and dataset limitations stated?
- Does the document differentiate facts, hypotheses, and future analyses?
- Are files and code readable? Are there secrets, private data, or huge outputs?
