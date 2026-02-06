# Google SecOps Extension

This repository contains the **Google SecOps Extension**, providing specialized skills for security operations.

## Overview

This extension packages setup and key security workflows into [skills](https://agentskills.io/specification). 

These skills are **Adaptive**, designed to work seamlessly with:
 *   [Google SecOps Remote MCP Server](https://google.github.io/mcp-security/docs/remote_server.html) (Preferred)
 *   **Local Python Tools** (Fallback)

This allows the skills to function in diverse environments, automatically selecting the best available tool for the job.

## Prerequisites

1.  **Install Gemini CLI (Preview)**:
    ```bash
    npm install -g @google/gemini-cli@preview
    ```

2.  **GUI Login Requirement**: You MUST have logged into the Google SecOps GUI at least once before using the API/MCP server.

3.  **Enable Skills**: Ensure your `~/.gemini/settings.json` has `experimental.skills` enabled:
    ```json
    {
      "security": {
        "auth": {
          "selectedType": "gemini-api-key"
        }
      },
      "general": {
        "previewFeatures": true
      },
      "experimental": {
        "skills": true,
        "extensionConfig": true
      }
    }
    ```

Verify skills are enabled from the Gemini CLI prompt:
```
/skills list
```

## Installation

### Option 1: Install from GitHub (Recommended)

You can install this extension directly from the GitHub repository:

```bash
gemini extensions install github:dandye/secops-gemini-extension
```

### Option 2: Clone and Install Locally

If you want to modify the extension or install it from a local copy:

1.  **Clone the repository**:
    ```bash
    git clone https://github.com/dandye/secops-gemini-extension.git
    cd secops-gemini-extension
    ```

2.  **Install the extension**:
    ```bash
    gemini extensions install .
    ```

### Configuration

During installation, you will be prompted for environment variables for the MCP configuration:

1. `PROJECT_ID` (GCP Project ID on your SecOps tenant's /settings/profile page)
2. `CUSTOMER_ID` (Your Chronicle Customer UUID)
3. `REGION` (Your Chronicle Region, e.g., `us`, `europe-west1`)
4. `SERVER_URL` (e.g. https://chronicle.northamerica-northeast2.rep.googleapis.com/mcp, https://chronicle.us.rep.googleapis.com/mcp, etc.)

> **Note**: These values are persisted in the extension's configuration and can be referenced by skills.

## Available Skills

### 1. Setup Assistant (Antigravity) (`secops-setup-antigravity`)
*   **Trigger**: "Help me set up Antigravity", "Configure Antigravity for SecOps".
*   **Function**: Checks for Google Cloud authentication and environment variables, then merges the correct configuration into your Antigravity settings.

### 2. Alert Triage (`secops-triage`)
*   **Trigger**: "Triage alert [ID]", "Analyze case [ID]".
*   **Function**: Orchestrates a Tier 1 triage workflow. It checks for duplicates, enriches entities, and provides a classification recommendation (FP/TP).

### 3. Investigation (`secops-investigate`)
*   **Trigger**: "Investigate case [ID]", "Deep dive on [Entity]".
*   **Function**: Guides deep-dive investigations using specialized runbooks.

### 4. Threat Hunting (`secops-hunt`)
*   **Trigger**: "Hunt for [Threat]", "Search for TTP [ID]".
*   **Function**: Assists in proactive threat hunting by generating hypotheses and constructing complex UDM queries for Chronicle.

### 5. Cases (`secops-cases`)
*   **Trigger**: "List cases", "Show recent cases", "/secops:cases".
*   **Function**: Lists recent SOAR cases to verify connectivity and view case status.

## Custom Commands

You can use the following slash commands as shortcuts for common tasks:

*   `/secops:triage <ALERT_ID>`: Quickly start triaging an alert.
*   `/secops:investigate <CASE_ID>`: Start an investigation.
*   `/secops:hunt <THREAT>`: Start a threat hunt.
*   `/secops:cases`: List recent cases.

## How it Works

These skills act as **Driver Agents** that:
1.  **Read** standardized Runbooks.
2.  **Execute** the steps using the available MCP tools.
3.  **Standardize** the output according to SOC best practices.

### Tool Selection

The skills employ an **Adaptive Execution** strategy to ensure robustness:

1.  **Check Environment**: The skill first identifies which tools are available in the current workspace.
2.  **Prioritize Remote**: If the **Remote MCP Server** is connected, the skill uses remote tools (e.g., `list_cases`, `udm_search`) for maximum capability.
3.  **Fallback to Local**: If remote tools are unavailable, the skill attempts to use **Local Python Tools**.

## Cross-Compatibility

These skills are designed to be compatible with **Claude Code** and other AI agents. The `slash_command` and `personas` metadata in the skill definitions allow other tools to index and trigger these skills effectively.

## Known Issues
* If the `SERVER_URL` requires regionalization (i.e. LEP vs REP vs MREP), it can be very difficult for the user to know what value to use.

Documentation says:
> Server URL or Endpoint: Select the regional endpoint and add /mcp at the end. For example, https://chronicle.us.rep.googleapis.com/mcp

Known-good values for Regional Endpoints (REP):
* https://chronicle.us-east1.rep.googleapis.com/mcp
* https://chronicle.africa-south1.rep.googleapis.com/mcp
* https://chronicle.asia-northeast1.rep.googleapis.com/mcp
* https://chronicle.me-central1.rep.googleapis.com/mcp
* https://chronicle.europe-west1.rep.googleapis.com/mcp
* https://chronicle.northamerica-northeast2.rep.googleapis.com/mcp
* https://chronicle.southamerica-east1.rep.googleapis.com/mcp
* https://chronicle.europe-west2.rep.googleapis.com/mcp
* ...

Known-good values for Multi-Regional Endpoints (MREP):
* https://chronicle.us.rep.googleapis.com/mcp

## References
* [Agent Skills Specification](https://agentskills.io/specification)
* [Gemini CLI Documentation](https://geminicli.com)
* [Antigravity Skills](https://antigravity.google/docs/skills)
* [Use the Google SecOps MCP server](https://docs.cloud.google.com/chronicle/docs/secops/use-google-secops-mcp)
