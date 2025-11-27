# Create Pull Request

Create a pull request with all necessary commits and proper formatting.

## Usage

- `/pr` - Create a PR for the current branch
- `/pr <custom-title>` - Create a PR with a custom title

## Instructions

You are tasked with preparing the code and creating a comprehensive pull request.

1. **Verify Prerequisites**:
   - Run `git status` to check for uncommitted changes
   - Run `git branch --show-current` to get the current branch name
   - Run `git remote -v` to verify remote repository is configured
   - Confirm we're not on `main`, `master`, or `develop` branch

2. **Handle Uncommitted Changes** (if any exist):
   - Run `git diff` to show unstaged changes
   - Run `git diff --staged` to show staged changes
   - Ask the user if they want to commit these changes before creating the PR
   - If yes, stage all changes with `git add .`
   - Create a commit with a descriptive message using heredoc format
   - If no, warn that uncommitted changes won't be included in the PR

3. **Analyze the Branch**:
   - Run `git log origin/main..HEAD --oneline` (or `origin/master..HEAD`) to see all commits
   - Run `git diff origin/main...HEAD` (or `origin/master...HEAD`) to see all changes
   - Review the full scope of changes to understand what the PR contains

4. **Determine Base Branch**:
   - Check if `main` or `master` exists as the default branch
   - Use the appropriate base branch for the PR

5. **Push to Remote**:
   - Run `git push -u origin <current-branch>` to push the branch
   - If the branch already exists remotely, run `git push` to update it

6. **Generate PR Title**:
   - **If custom title provided**: Use it as-is
   - **If no custom title**: Generate from:
     - Branch name (extract Jira ticket and description if present)
     - First commit message
     - Overall change summary
   - Format: `[TICKET-123] Brief description of changes` or `Brief description of changes`

7. **Generate PR Description**:
   Create a comprehensive description with:

   ```markdown
   ## Summary
   - Bullet point summary of key changes
   - What problem does this solve?
   - What approach was taken?

   ## Changes
   - Detailed list of modifications
   - New features or functionality
   - Bug fixes
   - Refactoring or improvements

   ## Testing
   - [ ] Unit tests added/updated
   - [ ] Integration tests added/updated
   - [ ] Manual testing completed
   - [ ] All tests passing

   ## Jira Ticket
   [TICKET-123](link-to-jira) (if applicable)

   ## Additional Notes
   - Any breaking changes
   - Migration steps required
   - Dependencies added/updated
   - Screenshots (if UI changes)
   ```

8. **Create the Pull Request**:
   - Use `gh pr create` command with the title and body
   - Use heredoc for the body to preserve formatting:
     ```bash
     gh pr create --title "PR Title" --body "$(cat <<'EOF'
     PR description content here
     EOF
     )"
     ```
   - Target the appropriate base branch (main/master)

9. **Verify and Display**:
   - Capture the PR URL from the `gh pr create` output
   - Run `gh pr view` to show the created PR details
   - Display the PR URL prominently to the user

## Example Output

```
✓ Committed changes: "Add user authentication feature"
✓ Pushed to origin/TCK-123_add-user-auth
✓ Created pull request: https://github.com/org/repo/pull/123

Pull Request: [TCK-123] Add user authentication feature
Base: main ← Head: TCK-123_add-user-auth
```

## Important Notes

- Ensure `gh` CLI is installed and authenticated
- If `gh` is not available, provide instructions for manual PR creation
- Always verify all tests pass before creating the PR
- Include relevant Jira ticket links if the branch name contains a ticket number
- Ask the user if they want to add reviewers or labels to the PR
- DO NOT use `--no-verify` or skip any git hooks
