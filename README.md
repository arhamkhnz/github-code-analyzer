# GitHub Code Analyzer
 
A fully automated GitHub repository analyzer that counts lines of code across all your repositories and updates stats dynamically. Runs on the default branch of each repo and skips forks.

<!-- LANGUAGES BREAKDOWN START -->
```
[ LANGUAGES BREAKDOWN ]

JavaScript   --> 372,969 lines
TypeScript   --> 108,652 lines
JSX          --> 20,261 lines
Vue.js       --> 0 lines
PHP          --> 5,248 lines
C#           --> 0 lines
Other        --> 9,961 lines

[ TOTAL LINES OF CODE: 517,091 ]
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

4. **Trigger the Workflow**
   - The workflow runs **by default every Sunday at midnight UTC (customizable)**.
   - To **run manually**, go to **GitHub Actions → Select Workflow → Run Workflow**.
   - To **run on every push**, modify the workflow's `on:` section to:
     ```yaml
     on:
       push:
         branches:
           - main
     ```
   
5. **Wait for Processing**  
   The time taken depends on the number of repositories and their sizes. Once completed, your `README.md` will be updated with the latest **lines of code breakdown**.

6. **Ensure Placeholders Are Present**  
   To allow automatic updates, your `README.md` must include the following placeholders:
   ```
   <!-- LANGUAGES BREAKDOWN START -->
```
[ LANGUAGES BREAKDOWN ]

JavaScript   --> 372,969 lines
TypeScript   --> 108,652 lines
JSX          --> 20,261 lines
Vue.js       --> 0 lines
PHP          --> 5,248 lines
C#           --> 0 lines
Other        --> 9,961 lines

[ TOTAL LINES OF CODE: 517,091 ]
```
   <!-- LANGUAGES BREAKDOWN END -->
   ```
   The workflow will update the stats between these markers.

### **Configure Language Detection**
By default, the workflow excludes some file types from counting:
```bash
cloc . --exclude-ext=json,html,css,svg,md,py,ps1,scss --json > ../output/cloc-output.json
```
You can modify this list in the workflow file to include or exclude specific languages based on your needs.

After execution, you can check the **`cloc-output.json`** file inside the `output` folder to see the full language breakdown.  
For a complete list of supported languages, refer to [`cloc` documentation](https://github.com/AlDanial/cloc).


## Upcoming Features

Soon, I'll be pushing changes to configure workflow execution based on a JSON configuration.  
This will allow you to:
- Specify **which repositories** the script should run on by default.
- Define **whether to run on all branches** or only specific branches for certain repositories.

This feature will be updated shortly.

 
## Contributing
 
Contributions are welcome! Feel free to fork the repository, submit a PR, or open an issue.
 
---