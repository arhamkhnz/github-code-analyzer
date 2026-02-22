# GitHub Code Analyzer
 
A fully automated GitHub repository analyzer that counts lines of code across all your repositories and updates stats dynamically. Runs on the default branch of each repo and skips forks.

<!-- LANGUAGES BREAKDOWN START -->
```
[ LANGUAGES BREAKDOWN ]

TypeScript   --> 291,104 lines
JavaScript   --> 134,340 lines
JSX          --> 20,576 lines
PHP          --> 5,248 lines
Others       --> 16,074 lines

[ TOTAL LINES OF CODE: 467,342 ]
```
<!-- LANGUAGES BREAKDOWN END -->
*Stats update automatically via GitHub Actions.*
 
## How It Works  
This GitHub Action automatically fetches all your public repositories (excluding forks), clones the **default branch**, and analyzes lines of code using [`cloc`](https://github.com/AlDanial/cloc). It then updates the repository’s `README.md` with the latest code statistics. The workflow runs **by default every Sunday at midnight UTC (customizable)**, keeping your stats up to date.

## Usage

### **Setting Up the GitHub Action**
1. **Add the Workflow File**  
   Copy the `analyze-code.yml` file into your repository at:
   ```
   .github/workflows/analyze-code.yml
   ```
   Then commit and push the changes.

2. **Generate a GitHub Personal Access Token (PAT)**  
   You need a **Personal Access Token (PAT)** with **`repo`** permissions.  
   Refer to [GitHub Docs](https://github.com/settings/tokens) on how to generate one.

3. **Add the Token to Repository Secrets**  
   - Go to **Settings → Secrets and variables → Actions → New repository secret**  
   - Name the secret **`GH_PAT`**  
   - Paste the generated token and save.

4. **Update Workflow Permissions**  
   In the repository where you're running the action, make sure to update workflow permissions:
   - Go to **Settings → Actions → General**.
   - Under **Workflow permissions**, select **"Read and write permissions"**.
   - This allows the workflow to update files like `README.md` automatically.

5. **Trigger the Workflow**
   - The workflow runs **by default every Sunday at midnight UTC (customizable)**.
   - To **run manually**, go to **GitHub Actions → Select Workflow → Run Workflow**.
   - To **run on every push**, modify the workflow's `on:` section to:
     ```yaml
     on:
       push:
         branches:
           - main
     ```
   
6. **Wait for Processing**  
   The time taken depends on the number of repositories and their sizes. Once completed, your `README.md` will be updated with the latest **lines of code breakdown**.

7. **Ensure Placeholders Are Present**  
   To allow automatic updates, your `README.md` must include the following placeholders:
   ```
    <!-- LANGUAGES BREAKDOWN START -->
```
[ LANGUAGES BREAKDOWN ]

TypeScript   --> 291,104 lines
JavaScript   --> 134,340 lines
JSX          --> 20,576 lines
PHP          --> 5,248 lines
Others       --> 16,074 lines

[ TOTAL LINES OF CODE: 467,342 ]
```
    <!-- LANGUAGES BREAKDOWN END -->
   ```
   The workflow will update the stats between these markers.  
   *Remove `(STATIC EXAMPLE)` when adding it in your README, as it's just a placeholder. It's included here only to prevent automatic updates in this README.*

### Configure languages

The workflow uses cloc **language names** (not file extensions).

- **HIGHLIGHT_LANGS** → languages shown individually in the README; everything else is grouped under **Others**  
- **IGNORE_LANGS** → languages dropped completely (not shown, not counted)

Edit these in `.github/workflows/analyze-code.yml` under the job `env:` block:

```yaml
env:
  # Use cloc LANGUAGE NAMES (not extensions). Example: "Vuejs Component" for .vue, "C#" for C#
  # HIGHLIGHT_LANGS: show these languages individually; everything else goes to "Others"
  HIGHLIGHT_LANGS: "JavaScript,TypeScript,JSX,Vuejs Component,PHP,C#"

  # IGNORE_LANGS: drop these languages entirely (not shown and not counted)
  IGNORE_LANGS: "JSON,HTML,CSS,SCSS,Sass,Markdown,SVG,XML,YAML,TOML,CSV,Text,Properties"
```

### **Configure Language Detection**
The workflow asks cloc to exclude languages using cloc’s own filter, so totals match cloc’s `SUM`:

```bash
cloc . --json --report-file=../output/cloc-output.json --exclude-lang="${IGNORE_LANGS}"
```

For a complete list of language names supported by cloc, see the [cloc documentation](https://github.com/AlDanial/cloc).


## Upcoming Features

Soon, I'll be pushing changes to configure workflow execution based on a JSON configuration.  
This will allow you to:
- Specify **which repositories** the script should run on by default.
- Define **whether to run on all branches** or only specific branches for certain repositories.

This feature will be updated shortly.

 
## Contributing
 
Contributions are welcome! Feel free to fork the repository, submit a PR, or open an issue.
