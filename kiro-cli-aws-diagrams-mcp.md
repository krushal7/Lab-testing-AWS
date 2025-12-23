# Build AWS Architecture Diagrams using Kiro CLI and MCP

This guide explains how to set up Kiro CLI with MCP servers to generate AWS architecture diagrams using natural language prompts while following AWS best practices.

---

## Prerequisites

To implement this solution, you must have:

- An AWS account with appropriate permissions
- Internet access
- Python 3.10 or newer
- AWS Builder ID (recommended for experimentation)

---

## 1. Set Up Your Environment

Before creating diagrams, you need to install Kiro CLI and configure MCP servers.

---

## 2. Install Kiro CLI

Kiro CLI is available as a standalone installation.

### 2.1 Download and Install

Follow the official Kiro CLI installation instructions provided by AWS.

```
wget https://desktop-release.q.us-east-1.amazonaws.com/latest/kiro-cli.deb
```

```
sudo dpkg -i kiro-cli.deb
sudo apt-get install -f
```

```
Launch Kiro CLI:

bash

kiro-cli
```

---

### 2.2 Verify Installation

```bash
kiro-cli --version
```

Expected output:

```
kiro-cli x.x.x
```

---

### 2.3 Authenticate Kiro CLI

Configure Kiro CLI with your AWS credentials:

```bash
kiro-cli login
```

Choose the login method:

- Use AWS Builder ID (recommended for experimentation)
- Other supported authentication methods

Complete the authentication flow in your browser.

---

## 3. Set Up MCP Servers

MCP (Model Context Protocol) servers enable diagram generation and AWS documentation validation.

---

### 3.1 Install uv

```bash
pip install uv
```

---

### 3.2 Install Python 3.10+

```bash
uv python install 3.10
```

---

### 3.3 Install GraphViz

GraphViz is required to render architecture diagrams.

#### Ubuntu / Debian

```bash
sudo apt update
sudo apt install -y graphviz
```

#### macOS

```bash
brew install graphviz
```

#### Windows (WSL)

```bash
sudo apt install -y graphviz
```

Verify installation:

```bash
dot -V
```

---

### 3.4 Configure MCP Servers

Create the MCP configuration directory:

```bash
mkdir -p ~/.kiro/settings
```

Create or edit the MCP configuration file:

```bash
nano ~/.kiro/settings/mcp.json
```

Paste the following configuration:

```json
{
  "mcpServers": {
    "awslabs.aws-diagram-mcp-server": {
      "command": "uvx",
      "args": ["awslabs.aws-diagram-mcp-server"],
      "env": {
        "FASTMCP_LOG_LEVEL": "ERROR"
      },
      "autoApprove": [],
      "disabled": false
    },
    "awslabs.aws-documentation-mcp-server": {
      "command": "uvx",
      "args": ["awslabs.aws-documentation-mcp-server@latest"],
      "env": {
        "FASTMCP_LOG_LEVEL": "ERROR"
      },
      "autoApprove": [],
      "disabled": false
    }
  }
}
```

Kiro CLI automatically discovers MCP servers from this file when the chat interface starts.

---

## 4. Test Your Setup

Verify that Kiro CLI and MCP servers are working correctly.

---

### 5.1 Start Kiro CLI Chat Interface

```bash
kiro-cli
```

Verify MCP servers are loaded:

```text
/mcp
```

---

### 5.2 Generate a Test Diagram

Paste the following prompt into the chat interface:

```text
Please create a diagram showing an EC2 instance in a VPC connecting to an external S3 bucket. Include essential networking components (VPC, subnets, Internet Gateway, Route Table), security elements (Security Groups, NACLs), and clearly mark the connection between EC2 and S3. Label everything appropriately concisely and indicate that all resources are in the us-east-1 region. Check for AWS documentation to ensure it adheres to AWS best practices before you create the diagram.
```

---

### 5.3 Trust the Tool

When prompted, trust the tool by entering:

```
t
```

Kiro CLI will generate and display the architecture diagram.

---

## 6. Validation

- Diagram generation confirms correct setup
- Visual layout may vary due to generative AI
- Core AWS components and relationships should be present

If issues occur, verify:

- MCP servers are configured correctly
- Required tools are installed
- The `~/.kiro/settings/mcp.json` file exists

---

## Result

Your environment is now ready to generate AWS architecture diagrams using Kiro CLI and MCP servers, with built-in AWS documentation validation.

---
