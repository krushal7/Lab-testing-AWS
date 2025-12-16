# Creating an AWS DevOps AI Agent with the Strands Agents SDK

**Lab Code:** SPL-TF-300-MLAIST-1  
**Version:** 1.1.1  

© 2025 Amazon Web Services, Inc. or its affiliates. All rights reserved.

> ⚠️ **Note**  
> Do not include any personal, identifying, or confidential information in the lab environment.  
> Information entered may be visible to others.

---

## Lab Overview

AnyCompany Airlines operates a complex multi-cloud infrastructure that requires constant monitoring, maintenance, and rapid operational response. DevOps and SRE teams currently perform many repetitive tasks manually, such as:

- Monitoring CloudWatch alarms  
- Researching AWS best practices  
- Provisioning AWS resources  
- Coordinating multi-service operational workflows  

In this lab, you build a **multi-agent AWS DevOps AI system** using the **Strands Agents SDK** and **Amazon Bedrock models**. You progress from creating specialized agents to building a full **orchestrator agent** capable of handling complex natural-language operational requests such as:

> *“Deploy a web application that displays user data from DynamoDB.”*

This lab demonstrates how AI agents can reduce deployment and response time from hours to minutes.

---

## Objectives

By the end of this lab, you will be able to:

- Implement intelligent file analysis and code generation
- Build monitoring agents integrated with Amazon CloudWatch
- Create research-enabled agents using AWS Documentation MCP
- Develop agents capable of managing AWS resources
- Design a multi-agent orchestrator that converts natural language into AWS workflows

---

## Icon Key

- **Caution** – Important information that may require repeating steps if missed  
- **Command** – Commands you must execute  
- **Expected output** – Sample output for validation  
- **Note** – Tips or guidance  
- **Task complete** – Task summary  
- **Warning** – Irreversible or failure-prone actions  

---

## Lab Environment

The architecture consists of:

- Pre-configured EC2 instance with development tools
- IAM roles with Amazon Bedrock permissions
- Access to:
  - Amazon Nova Lite
  - Amazon Nova Pro
  - Claude 3.5 Haiku
- Python environment with required dependencies

### Agent Architecture

Five agents collaborate:
- **Coder Agent**
- **Alarm Manager Agent**
- **AWS Research Agent**
- **AWS Manager Agent**
- **Orchestrator Agent**

The orchestrator coordinates all specialized agents.

---

## Task 1: Create Specialized Agents

### Task 1.1: Connect to Code Editor

1. Open the **LabInstanceURL** in a new browser tab
2. Open **File Explorer**
3. Open `lab.ipynb`
4. Select **Kernel → Install/Enable suggested extensions**
5. Trust publisher and install
6. Create Python virtual environment (Venv)

#### Activate virtual environment

```bash
source .venv/bin/activate
