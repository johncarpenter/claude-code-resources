# Atlassian MCP Integration

## Overview

When the user requests Atlassian integration or wants to create issues directly in Jira, use the Atlassian MCP server if available. This allows creating Epics, Stories, and Tasks directly from the work breakdown.

## Checking for MCP Availability

Before attempting to use MCP tools, check if the Atlassian MCP server is available in your environment.

## Creating Issues in Jira

### Epic Creation

Epics are created at the top level of the hierarchy:

```
Title: Epic title from work breakdown
Description: Summary from PRD executive summary
Issue Type: Epic
```

### Feature Creation (as Stories under Epic)

Features become Stories linked to the Epic:

```
Title: Feature title from work breakdown
Description: Detailed feature description
Issue Type: Story
Epic Link: Parent epic key
```

### User Story Creation (as Sub-tasks)

User stories become sub-tasks under their parent feature:

```
Title: User story from work breakdown
Description: Full user story with acceptance criteria
Issue Type: Sub-task
Parent: Feature story key
Story Points: Estimate based on sizing (S=2, M=5, L=8)
```

## Workflow for MCP Integration

1. **Generate markdown files first**: Always create the PRD and work breakdown markdown files as primary deliverables
2. **Offer MCP integration**: Ask the user if they want to create these items in Jira
3. **Create hierarchically**: Start with Epic, then Features, then User Stories
4. **Capture issue keys**: Store the returned issue keys to establish proper parent-child relationships
5. **Provide summary**: After creation, summarize what was created with links

## Story Point Mapping

- S (Small): 2 story points
- M (Medium): 5 story points
- L (Large): 8 story points
- Unknown: Leave empty or use 0

## Example Interaction Flow

```
1. Generate PRD markdown → Save to /outputs/prd.md
2. Generate Work Breakdown markdown → Save to /outputs/work_breakdown.md
3. Ask user: "Would you like me to create these items in Jira using the Atlassian MCP?"
4. If yes:
   a. Check MCP availability
   b. Create Epic
   c. For each Feature: Create Story and link to Epic
   d. For each User Story: Create Sub-task and link to parent Story
   e. Provide summary with Jira issue links
```

## Handling MCP Unavailability

If Atlassian MCP is not available:
- Inform the user that manual creation will be needed
- Ensure markdown files are formatted for easy copy-paste
- Include instructions in the work breakdown for importing to Jira
- Consider providing a CSV format option for bulk import

## Best Practices

- Always include acceptance criteria in user story descriptions
- Use consistent formatting for better readability in Jira
- Tag stories appropriately (Frontend, Backend, DevOps, Testing)
- Include technical notes that will help developers
- Link related documentation from the PRD
