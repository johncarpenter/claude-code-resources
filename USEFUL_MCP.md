See https://registry.modelcontextprotocol.io/ for a list of useful MCP servers.

## Core Productivity


```bash

### Context7
Fetches real-time, version-specific library documentation directly from source repositories to ensure current API usage patterns.

```bash
# Remote server (recommended)
claude mcp add --transport http context7 https://mcp.context7.com/mcp

# Local installation
claude mcp add context7 -- npx -y @upstash/context7-mcp
```

### GitHub
Direct interaction with repositories, issues, pull requests, and CI/CD workflows without leaving your terminal.

```bash
# Remote server (recommended)
claude mcp add --transport http github https://api.githubcopilot.com/mcp/

# Docker installation
claude mcp add github -- docker run -i --rm -e GITHUB_PERSONAL_ACCESS_TOKEN ghcr.io/github/github-mcp-server
claude mcp update github -e GITHUB_PERSONAL_ACCESS_TOKEN=your_github_pat
```

## Web & API Tools

### Playwright
Browser automation using structured accessibility trees for reliable web testing, scraping, and workflow automation.

```bash
claude mcp add playwright npx -- @playwright/mcp@latest
# Windows:
claude mcp add playwright cmd -- /c npx @playwright/mcp@latest
```

### Apidog
Bridges API documentation and code generation by connecting to OpenAPI/Swagger specifications.

```bash
# With remote OpenAPI URL
claude mcp add apidog -- npx -y apidog-mcp-server@latest --oas=https://petstore.swagger.io/v2/swagger.json

# With local file
claude mcp add apidog -- npx -y apidog-mcp-server@latest --oas=~/data/api/swagger.json
```

## Cloud Infrastructure

### AWS
Comprehensive access to Amazon Web Services including Lambda, DynamoDB, S3, and documentation.

```bash
# AWS API server
claude mcp add aws-api -e AWS_REGION=us-east-1 -e AWS_API_MCP_PROFILE_NAME=default -- uvx awslabs.aws-api-mcp-server@latest

# AWS Knowledge server (no account required)
claude mcp add --transport http aws-knowledge https://knowledge-mcp.global.api.aws

# Read-only mode (recommended for security)
claude mcp add aws-api -e AWS_REGION=us-east-1 -e READ_OPERATIONS_ONLY=true -- uvx awslabs.aws-api-mcp-server@latest
```

### Cloudflare
Access to Cloudflare's edge computing platform including Workers, R2, D1, and observability tools.

```bash
# Workers bindings
claude mcp add --transport sse cloudflare-workers https://bindings.mcp.cloudflare.com/sse

# Documentation (no auth required)
claude mcp add --transport sse cloudflare-docs https://docs.mcp.cloudflare.com/sse

# Radar analytics
claude mcp add --transport sse cloudflare-radar https://radar.mcp.cloudflare.com/sse

# Observability
claude mcp add --transport sse cloudflare-observability https://observability.mcp.cloudflare.com/sse

# Browser rendering
claude mcp add --transport sse cloudflare-browser https://browser.mcp.cloudflare.com/sse
```

### Google Cloud Platform (Community)
Access to GCP services including Compute Engine, Cloud Storage, BigQuery, and Cloud Functions.

```bash
claude mcp add gcp -e GOOGLE_APPLICATION_CREDENTIALS=/path/to/service-account-key.json -e GCP_PROJECT_ID=your-project-id -e GCP_REGION=us-central1 -- npx @eniayomi/gcp-mcp-server
```

*Note: Community-maintained, not official Google server.*

## Monitoring & Analytics

### Sentry
Error tracking and performance monitoring with intelligent pattern analysis and debugging insights.

```bash
claude mcp add --transport sse sentry https://mcp.sentry.dev/mcp
# Configure Authorization header with your Sentry auth token when prompted
```

### PostHog
Product analytics including feature flags, A/B tests, funnels, and user behavior analysis.

```bash
claude mcp add --transport sse posthog https://mcp.posthog.com/sse
# Configure Authorization header with Bearer token when prompted
```

## Project Management & Docs

### Linear
Create, find, and update Linear issues, projects, and comments directly from your development environment.

```bash
claude mcp add --transport sse linear https://mcp.linear.app/sse
# OAuth authentication - browser window opens for login
```

### Notion
Read, create, and manipulate Notion pages, databases, and comments with AI-optimized token efficiency.

```bash
claude mcp add notion -e NOTION_API_TOKEN=your-integration-token -- npx -y @makenotion/notion-mcp-server
```

*Prerequisite: Create integration at https://www.notion.so/my-integrations and share pages with it.*

### Atlassian (Beta)
Real-time interaction with Jira and Confluence for searching, creating, and updating issues and pages.

```bash
claude mcp add --transport sse atlassian https://mcp.atlassian.com/v1/sse
# OAuth 2.1 authorization via browser
```

## Design Tools

### Figma Dev Mode
Structured access to Figma designs for generating framework-specific code from design selections.

```bash
# Enable MCP Server in Figma desktop: Preferences → Developer settings → Enable "MCP Server"
claude mcp add --transport sse figma http://127.0.0.1:3845/sse
```

*Prerequisite: Dev or Full seat on Professional/Organization/Enterprise plan.*


## Verification

After installing any server, verify with:

```bash
claude mcp list
```

## Resources

- [AWS MCP Servers](https://github.com/awslabs/mcp)
- [Cloudflare MCP Servers](https://github.com/cloudflare/mcp-server-cloudflare)
- [GCP MCP Server (Community)](https://github.com/eniayomi/gcp-mcp)
- [Airtable MCP Server (Community)](https://github.com/domdomegg/airtable-mcp-server)
- [Atlassian MCP Docs](https://support.atlassian.com/rovo/docs/getting-started-with-the-atlassian-remote-mcp-server/)