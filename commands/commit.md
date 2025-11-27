# Commit Changes

Create a detailed git commit with all staged and unstaged changes.

## Instructions

You are tasked with creating a comprehensive git commit. Follow these steps:

1. **Review Current State**:
   - Run `git status` to see all changes
   - Run `git diff` to review unstaged changes
   - Run `git diff --staged` to review staged changes
   - Run `git log --oneline -5` to see recent commit history for context

2. **Stage All Changes**:
   - Run `git add .` to stage all changes

3. **Analyze Changes**:
   - Carefully review what files were modified, added, or deleted
   - Identify the primary purpose of these changes (feature, fix, refactor, docs, etc.)
   - Note any important technical details or breaking changes

4. **Create Commit Message**:
   - **If the user provided a custom message**: Use their message as the foundation, but enhance it with proper formatting:
     - First line: Concise summary (50 chars or less)
     - Blank line
     - Detailed explanation of what changed and why
     - Include technical details if relevant
     - Note any breaking changes or important considerations

   - **If no custom message was provided**: Create a detailed commit message following this structure:
     - First line: Concise summary in imperative mood (e.g., "Add feature" not "Added feature")
     - Blank line
     - Bullet points explaining:
       - What changed
       - Why it changed
       - Any important technical details
       - Impact on the codebase

5. **Create the Commit**:
   - Use a heredoc to properly format the commit message
   - Example format:
     ```bash
     git commit -m "$(cat <<'EOF'
     Brief summary of changes

     Detailed explanation:
     - Key change 1
     - Key change 2
     - Technical detail or rationale

     Any additional context or notes.
     EOF
     )"
     ```

6. **Verify**:
   - Run `git log -1` to show the created commit
   - Run `git status` to confirm the commit was successful

## Important Notes

- DO NOT push the commit unless explicitly requested
- The commit message should focus on "why" not just "what"
- Be specific and descriptive in the commit message
- Respect any custom message provided by the user, but enhance its formatting
