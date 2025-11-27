# Create Git Worktree

Create a git worktree for the current branch, allowing you to work on the same branch in a separate directory.

## Usage

- `/worktree` - Create a worktree for the current branch in a directory named after the branch
- `/worktree <directory-name>` - Create a worktree for the current branch in a custom directory name

## Instructions

You are tasked with creating a git worktree for the current branch. A worktree allows you to check out the same branch in a separate directory, enabling parallel work on the same branch.

1. **Verify Prerequisites**:
   - Run `git status` to check the current branch and any uncommitted changes
   - Run `git branch --show-current` to get the current branch name
   - Run `git worktree list` to see existing worktrees and avoid conflicts
   - Verify we're not in a bare repository (worktrees require a non-bare repo)

2. **Determine Worktree Directory**:
   - **If no directory name provided**: 
     - Use the current branch name as the directory name
     - Sanitize the branch name:
       - Replace slashes with hyphens (e.g., `feature/auth` → `feature-auth`)
       - Remove special characters that aren't valid in directory names
       - Use format: `../<branch-name>-worktree` (one level up from current repo)
   
   - **If directory name provided**: 
     - Use the provided name as-is
     - If it's a relative path, resolve it relative to the repository root
     - If it's an absolute path, use it directly

3. **Validate Directory**:
   - Check if the target directory already exists
   - If it exists and is not empty, warn the user and ask for confirmation
   - If it exists and is empty, proceed with worktree creation
   - If it doesn't exist, proceed with creation

4. **Check Branch Status**:
   - Run `git log -1 --oneline` to see the latest commit on current branch
   - Verify the branch exists and has commits
   - If the branch is new and has no commits, warn the user

5. **Create the Worktree**:
   - Run `git worktree add <directory-path> <branch-name>` to create the worktree
   - Example: `git worktree add ../feature-auth-worktree feature/auth`
   - The command will:
     - Create the directory if it doesn't exist
     - Check out the specified branch in that directory
     - Link it to the main repository

6. **Verify Creation**:
   - Run `git worktree list` to show all worktrees and confirm the new one appears
   - Run `cd <directory-path> && git branch --show-current` to verify the branch is checked out
   - Display the absolute path to the new worktree directory

7. **Provide Usage Instructions**:
   - Explain how to navigate to the worktree: `cd <directory-path>`
   - Note that changes in the worktree are immediately visible in the main repo
   - Remind that you can commit from either location
   - Mention how to remove the worktree later: `git worktree remove <directory-path>`

## Examples

**Input**: `/worktree`
**Current Branch**: `feature/user-authentication`
**Result**: Creates worktree at `../feature-user-authentication-worktree` with `feature/user-authentication` checked out

**Input**: `/worktree ../my-feature`
**Current Branch**: `TCK-123_fix-bug`
**Result**: Creates worktree at `../my-feature` with `TCK-123_fix-bug` checked out

**Input**: `/worktree /tmp/test-worktree`
**Current Branch**: `main`
**Result**: Creates worktree at `/tmp/test-worktree` with `main` checked out

## Important Notes

- Worktrees share the same `.git` directory, so changes are immediately visible across all worktrees
- You cannot check out the same branch in multiple worktrees simultaneously
- If the branch is already checked out in another worktree, you'll need to either:
  - Use a different branch
  - Remove the existing worktree first
  - Use `git worktree add --force` to force creation (not recommended)
- The worktree directory should be outside the main repository directory (typically one level up)
- Uncommitted changes in the main repo won't affect worktree creation, but be aware of potential conflicts
- To remove a worktree later: `git worktree remove <directory-path>` or `git worktree remove -f <directory-path>` if it has uncommitted changes
- Worktrees are useful for:
  - Testing changes while keeping the main repo clean
  - Working on the same branch from different locations
  - Running builds or tests in parallel
  - Comparing different states of the same branch

