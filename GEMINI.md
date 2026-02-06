# Google SecOps Extension

This repository contains the **Google SecOps Extension**, providing specialized skills and tools for security operations.

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

2.  **Google Cloud Authentication**: Ensure you are authenticated with Google Cloud and have set a quota project:
    ```bash
    gcloud auth application-default login
    gcloud auth application-default set-quota-project <YOUR_PROJECT_ID>
    ```

3.  **GUI Login Requirement**: You MUST have logged into the Google SecOps GUI at least once before using the API/MCP server.

4.  **Enable Skills**: Ensure your `~/.gemini/settings.json` has `experimental.skills` enabled:
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

## Installation

### Option 1: Install directly from GitHub (Fastest)

You can install this extension directly without cloning:

```bash
gemini extensions install github:dandye/secops-gemini-extension
```

### Option 2: Clone and Install Locally (Best for Development)

1.  **Clone the repository**:
    ```bash
    git clone https://github.com/dandye/secops-gemini-extension.git
    cd secops-gemini-extension
    ```

2.  **Install the extension**:
    ```bash
    gemini extensions install .
    ```

## Post-Installation

### 1. Configuration

During installation, you will be prompted for several parameters:

*   `PROJECT_ID`: Your Google Cloud Project ID.
*   `CUSTOMER_ID`: Your Chronicle Customer UUID.
*   `REGION`: Your Chronicle Region (e.g., `us`, `europe-west1`).
*   `SERVER_URL`: The regional MCP endpoint (e.g., `https://chronicle.us.rep.googleapis.com/mcp`).

> **Note**: These values are persisted in `~/.gemini/extensions/google-secops/.env`. You can edit this file at any time to update your configuration.

### 2. Verify Skills

Run the following command to ensure the skills are loaded:

```bash
/skills list
```

You should see `secops-setup-antigravity`, `secops-triage`, etc., in the list.

## Usage

### Available Skills

*   **Setup Assistant** (`secops-setup-antigravity`): "Help me set up Antigravity"
*   **Alert Triage** (`secops-triage`): "Triage alert [ID]"
*   **Investigation** (`secops-investigate`): "Investigate case [ID]"
*   **Threat Hunting** (`secops-hunt`): "Hunt for [Threat]"
*   **Cases** (`secops-cases`): "List recent cases"

### Custom Commands

Use these shortcuts for common tasks:

*   `/secops:triage <ALERT_ID>`
*   `/secops:investigate <CASE_ID>`
*   `/secops:hunt <THREAT>`
*   `/secops:cases`

## Known Issues

* **Regional Endpoints**: If the `SERVER_URL` requires regionalization, ensure you use the correct endpoint from the [official documentation](https://docs.cloud.google.com/chronicle/docs/secops/use-google-secops-mcp).

Known-good values for Regional Endpoints (REP):
* `https://chronicle.us-east1.rep.googleapis.com/mcp`
* `https://chronicle.europe-west1.rep.googleapis.com/mcp`
* `https://chronicle.us.rep.googleapis.com/mcp` (Multi-Regional)

## References
* [Agent Skills Specification](https://agentskills.io/specification)
* [Gemini CLI Documentation](https://geminicli.com)
* [Use the Google SecOps MCP server](https://docs.cloud.google.com/chronicle/docs/secops/use-google-secops-mcp)
