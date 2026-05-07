---
name: pr-assistant
description: Opens pull requests, DO NOT use for any other purpose.
compatibility: github
metadata: 
  repository: https://github.com/unlock-com/unlock-app/pulls
  trigger_phrases:
    - "open PR"
    - "open pull request"
    - "create pull request"
    - "create PR with my changes"
---

# Pull Request Assistant

## Instructions

### Step 1: Verify if a pull request is already open for the current branch
Before opening a new pull request, it's important to check if there is already an open pull request. This can be done using the `gh` command-line tool. Run the following command in your terminal:

```bash
gh pr list --head <current-branch-name>
```

### Step 2: Naming the pull request
If there is an open pull request for the current branch, exit with no action needed.
Let the user know that there is already an open pull request and provide a link to it.
This helps to avoid duplicate pull requests and keeps the review process organized.

If there is no open pull request for the current branch, you can proceed to open a new pull request.
When naming the pull request, follow a specific format that includes the jira ticket number.
This helps to easily identify the related jira ticket when reviewing pull requests.

Example:
Jira Ticket Number: UN-1234
PR name: UN-1234 Add Login functionality

Where UN-1234 is the jira ticket number, and Add Login functionality is a short description of the changes in the pull request. This format helps to easily identify the related jira ticket when reviewing pull requests.

### Step 3: The PR body

```md
## [UN-1234] [Replace this with PR Title]

<!-- What type of PR is this? (leave all that apply) -->
<pre>
👩🏻‍💻  Feature
🐛  Fix
🧨  Refactor
📄  Documentation Update
🔥  BREAKING CHANGE
</pre>

### Description
<!-- Summary of the proposed change -->

<!-- End of PR body template, don't add any additional information below this line -->
```

### Step 4: Create a temp file with the PR body content
Create a temporary file named `pr_body.md` and add the content of the PR body as per the template provided in Step 3.

### Step 5: Preview the pull request
Before opening the pull request, it's a good practice to preview it to ensure that the title and body are correctly formatted. You can use the `gh` command-line tool to preview the pull request. Run the following command in your terminal:
```bash
gh pr create --title "UN-1234 Add Login functionality" --body "$(cat /tmp/pr_body.md)" --preview
```

### Step 6: Open the pull request
Check with the user first if they want to base the PR against "dev" branch.

Use the `gh` command-line tool to open the pull request with the specified title and body. Run the following command in your terminal:
```bash
gh pr create --title "UN-1234 Add Login functionality" --body "$(cat /tmp/pr_body.md)" --base dev
```

### Step 7: Clean up
After the pull request has been created, you can delete the temporary file `pr_body.md` to keep your workspace clean.
Run the following command in your terminal:
```bash
rm /tmp/pr_body.md
```
