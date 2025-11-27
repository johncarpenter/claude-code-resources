# Create Git Branch

Create a new git branch with Jira ticket number and optional description.

## Usage

- `/branch <ticket-number>` - Create branch with just ticket number (e.g., `TCK-123`)
- `/branch <ticket-number> <description>` - Create branch with ticket and description (e.g., `TCK-123_add-user-authentication`)

## Instructions

You are tasked with creating a new git branch following the Jira ticket naming convention.

1. **Parse the Parameters**:
   - First parameter: Jira ticket number (required) - e.g., "TCK-123", "PROJ-456"
   - Second parameter: Description (optional) - e.g., "add user auth", "fix login bug"
   - If description is provided, sanitize it:
     - Convert spaces to hyphens
     - Convert to lowercase
     - Remove special characters except hyphens and underscores
     - Replace multiple hyphens with single hyphen

2. **Construct Branch Name**:
   - **If only ticket number provided**: Use format `<TICKET-NUMBER>`
     - Example: `TCK-123`

   - **If ticket number and description provided**: Use format `<TICKET-NUMBER>_<description>`
     - Example: `TCK-123_add-user-authentication`
     - Example: `PROJ-456_fix-login-bug`

3. **Validate Current State**:
   - Run `git status` to check current branch and any uncommitted changes
   - Warn the user if there are uncommitted changes (but still proceed if they want to)
   - Run `git branch` to verify the new branch name doesn't already exist

4. **Create the Branch**:
   - Run `git checkout -b <branch-name>` to create and switch to the new branch

5. **Verify**:
   - Run `git branch --show-current` to confirm the new branch was created and checked out
   - Display a success message with the branch name

## Examples

**Input**: `/branch TCK-123`
**Result**: Creates and checks out branch `TCK-123`

**Input**: `/branch TCK-123 add user authentication`
**Result**: Creates and checks out branch `TCK-123_add-user-authentication`

**Input**: `/branch PROJ-456 fix login bug`
**Result**: Creates and checks out branch `PROJ-456_fix-login-bug`

## Important Notes

- Always preserve the ticket number case as provided (typically uppercase)
- Description should be lowercase with hyphens
- Warn about uncommitted changes but don't block branch creation
- Do NOT push the branch unless explicitly requested
