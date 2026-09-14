# GitHub Code Analyzer

Counts code lines with [cloc](https://github.com/AlDanial/cloc). It skips forks, uses each repo's default branch, and puts the results in a README.

There are two workflows:

- **[Public by username](.github/workflows/analyze-public-code.yml):** counts someone's public repos. No PAT needed.
- **[With a PAT](analyze-code.yml):** counts repos your token can access, including private ones.

You can use both in the same repo. They write to separate sections of the README.

## Public repos by username

<!-- PUBLIC CODE STATS START -->
Public, non-fork repositories owned by [@arhamkhnz](https://github.com/arhamkhnz): **28**

| Language | Code lines |
| --- | ---: |
| TypeScript | 102,330 |
| Vuejs Component | 7,937 |
| JavaScript | 1,762 |
| Others | 6,220 |
| **Total** | **118,249** |
<!-- PUBLIC CODE STATS END -->

**To set it up:**

1. Copy [`.github/workflows/analyze-public-code.yml`](.github/workflows/analyze-public-code.yml) to the same path in your repo. Copy the two marker comments around the stats above into your README where you want the result. Add them only once.
2. Go to **Actions → Analyze Public Repositories by Username → Run workflow** and enter a username.
3. If you leave the username blank, it uses the `PUBLIC_STATS_USERNAME` repo variable, or your repo owner if that is not set. You can set the variable in **Settings → Secrets and variables → Actions → Variables**.

It also runs every Sunday at 00:17 UTC. It saves the result in the README and `output/public-summary.json`.

## Repos with a PAT

This workflow counts non-fork repos the token can access, including private repos. A fine-grained token can limit which private repos it sees, but public repos may still be counted. If your README is public, the totals from private code will be public too.

<!-- LANGUAGES BREAKDOWN START -->
```
[ LANGUAGES BREAKDOWN ]

JavaScript      --> 451,719 lines
TypeScript      --> 448,192 lines
JSX             --> 20,584 lines
Vuejs Component --> 7,937 lines
PHP             --> 5,248 lines
Others          --> 13,721 lines

[ TOTAL LINES OF CODE: 947,401 ]
```
<!-- LANGUAGES BREAKDOWN END -->

**To set it up:**

1. Copy [`analyze-code.yml`](analyze-code.yml) to `.github/workflows/analyze-code.yml` in your repo. Copy the two marker comments around the stats above into your README where you want the result. Add them only once.
2. Create a token in **GitHub → Settings → Developer settings → Personal access tokens**:
   - **Fine-grained (recommended):** [Generate a token](https://github.com/settings/personal-access-tokens/new), choose the account or organization, select the repos, and set **Contents: Read-only**. GitHub includes **Metadata: Read-only**. Some organizations need to approve the token.
   - **Classic:** [Generate a token](https://github.com/settings/tokens/new) with the `repo` scope if you need one token for repos under different owners or for an outside-collaborator repo. It gives broader access.
3. In the repo where you want the results, go to **Settings → Secrets and variables → Actions → New repository secret**. Save the token as `GH_PAT`.
4. Go to **Actions → Analyze Repositories with PAT → Run workflow**.

The copied workflow also runs every Sunday at 00:00 UTC. The version running in this repo also runs on pushes to `main`. The PAT reads source repos; the workflow's built-in `GITHUB_TOKEN` writes the README and `output/cloc-output.json`. Your repo must allow Actions to push those changes.

## Languages

Edit `HIGHLIGHT_LANGS` and `IGNORE_LANGS` near the top of either workflow. Use [cloc language names](https://github.com/AlDanial/cloc), like `Vuejs Component`. Highlighted languages get their own row, other counted languages go under **Others**, and ignored languages are left out.

## Contributing

Issues and pull requests are welcome.
