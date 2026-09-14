# GitHub Code Analyzer

Counts code lines with [cloc](https://github.com/AlDanial/cloc). It skips forks, uses each repo's default branch, and puts the results in a README.

There are two workflows to use:

- **[Public by username](.github/workflows/analyze-public-code.yml):** counts someone's public repos. No PAT needed.
- **[With a fine-grained PAT](.github/workflows/analyze-code.yml):** counts repos your token can access, including private ones.

Both workflow files are in `.github/workflows` so you can copy either one. The PAT job is skipped in this demo repo; it runs when copied to another repo with a `GH_PAT` secret. You can use both in the same repo, where they write to separate sections of the README.

Both workflows commit and push the updated README using the repo's built-in `GITHUB_TOKEN`. They already request `contents: write`. If a push is denied, go to **Settings → Actions → General → Workflow permissions** in the repo where you want the results and select **Read and write permissions** if available. Organization settings or branch rules can also block the push.

## Public repos by username

<!-- PUBLIC CODE STATS START -->
Public, non-fork repositories owned by [@arhamkhnz](https://github.com/arhamkhnz): **28**

| Language | Code lines |
| --- | ---: |
| TypeScript | 151,948 |
| Vuejs Component | 7,937 |
| JavaScript | 2,071 |
| Others | 6,267 |
| **Total** | **168,223** |
<!-- PUBLIC CODE STATS END -->

**To set it up:**

1. Copy [`.github/workflows/analyze-public-code.yml`](.github/workflows/analyze-public-code.yml) to the same path in your repo. Copy the two marker comments around the stats above into your README where you want the result. Add them only once.
2. Go to **Actions → Analyze Public Repositories by Username → Run workflow** and enter a username.
3. If you leave the username blank, it uses the `PUBLIC_STATS_USERNAME` repo variable, or your repo owner if that is not set. You can set the variable in **Settings → Secrets and variables → Actions → Variables**.

It runs after every branch push and every Sunday at 00:17 UTC. It saves the result in the README and `output/public-summary.json`.
The numbers above are from this repo's latest public run. Future runs also count identical files in different repos. These are lines in the repos' default branches, not a measure of who wrote them.

## Repos with a fine-grained PAT

This workflow counts non-fork repos the token can access, including private repos. Public repos may be counted too, so its total is not a private-only count. If your README is public, the totals from private code will be public too.
The PAT workflow does not print source repo names in normal run logs or save them in the report. Its job is skipped in this repo; the example below is copied from my public profile.

Example from my GitHub profile (shown here as a static example):

```
[ LANGUAGES BREAKDOWN ]

JavaScript   --> 74,901 lines
TypeScript   --> 322,313 lines
JSX          --> 20,562 lines
Vue.js       --> 21,091 lines
PHP          --> 5,248 lines
C#           --> 15,066 lines
Other        --> 15,873 lines

[ TOTAL LINES OF CODE: 475,054 ]
```

See the current numbers on [my GitHub profile](https://github.com/arhamkhnz). That profile is updated separately from this repo.

**To set it up:**

1. Copy [`.github/workflows/analyze-code.yml`](.github/workflows/analyze-code.yml) to the same path in your repo. Add these two lines to your README where you want the result; the workflow writes between them:

   ```md
   <!-- LANGUAGES BREAKDOWN START -->
   <!-- LANGUAGES BREAKDOWN END -->
   ```

2. [Create a fine-grained token](https://github.com/settings/personal-access-tokens/new). Choose the account or organization that owns the repos, choose **All repositories** or **Only select repositories**, and set **Contents: Read-only**. GitHub includes **Metadata: Read-only**. A fine-grained token covers one resource owner, and some organizations need to approve it.
3. In the repo where you want the results, go to **Settings → Secrets and variables → Actions → New repository secret**. Save the token as `GH_PAT`.
4. Go to **Actions → Analyze Repositories with PAT → Run workflow**.

The PAT workflow is scheduled for every Sunday at 00:00 UTC (midnight UTC) and can also be run manually. In this repo its job is skipped, even if you delete the `GH_PAT` secret. In a copied repo, the PAT reads source repos; the workflow's built-in `GITHUB_TOKEN` writes the README and `output/cloc-output.json`. If the token finds no private repos, the run warns you to check its access.

## Languages

Edit `HIGHLIGHT_LANGS` and `IGNORE_LANGS` near the top of either workflow. Use [cloc language names](https://github.com/AlDanial/cloc), like `Vuejs Component`. Highlighted languages get their own row, other counted languages go under **Others**, and ignored languages are left out.

## Contributing

Issues and pull requests are welcome.
