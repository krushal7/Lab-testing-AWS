# Creating an AWS DevOps AI Agent with the Strands Agents SDK

**Lab Code:** SPL-TF-300-MLAIST-1  
**Version:** 1.1.1  

© 2025 Amazon Web Services, Inc. or its affiliates. All rights reserved.

> ⚠️ Note  
> Do not include any personal, identifying, or confidential information in the lab environment.  
> Information entered may be visible to others.

---

## Lab Overview

AnyCompany Airlines operates a complex multi-cloud infrastructure that requires constant monitoring, maintenance, and rapid operational response. DevOps and SRE teams currently spend significant manual effort on repetitive operational tasks such as monitoring CloudWatch alarms, researching AWS best practices, provisioning resources, and coordinating actions across multiple AWS services.

In this lab, you build a multi-agent AWS DevOps AI system using the **Strands Agents SDK** and **Amazon Bedrock models**. You create specialized agents for coding, monitoring, research, and AWS resource management, then combine them into a single orchestrator agent capable of handling complex natural-language requests such as:

> “Deploy a web application that displays user data from DynamoDB.”

---

## Objectives

By the end of this lab, you will be able to:

- Implement intelligent file analysis and code generation using SDK tools  
- Build monitoring agents integrated with Amazon CloudWatch  
- Construct research-enabled agents using AWS Documentation MCP servers  
- Develop agents that manage AWS services and automate deployments  
- Design a multi-agent orchestrator that converts natural language into AWS workflows  

---

## Lab Environment

The lab environment includes:

- Pre-configured EC2 instance with development tools  
- IAM roles with permissions for Amazon Bedrock and AWS APIs  
- Access to Amazon Nova Lite, Nova Pro, and Claude 3.5 Haiku models  
- Python environment with required packages installed  

---

## Task 1: Create Specialized Agents

### Task 1.1: Connect to Code Editor

1. Open the **LabInstanceURL** in a new browser tab  
2. Open the file explorer and select `lab.ipynb`  
3. Install and trust suggested extensions  
4. Create a Python virtual environment (Venv)

Activate the virtual environment:

```bash
source .venv/bin/activate
```

Install dependencies:

```bash
pip install -r requirements.txt
```

Run each cell in `lab.ipynb`.

---

## Task 2: Integrate AWS Documentation via MCP

Edit `agents/aws_researcher.py`.

Add MCP client:

```python
documentation_mcp_server = MCPClient(
    lambda: stdio_client(
        StdioServerParameters(
            command="uvx", args=["awslabs.aws-documentation-mcp-server"]
        )
    )
)
```

Setup tools:

```python
with documentation_mcp_server:
    tools = documentation_mcp_server.list_tools_sync() + [file_write]
```

Create agent:

```python
aws_documentation_agent = Agent(
    model=nova_lite,
    system_prompt="""
    You are an AWS researcher.
    Use only official AWS documentation.
    Always provide citations.
    """,
    tools=tools,
)
```

Invoke agent:

```python
response = aws_documentation_agent(query)
return str(response)
```

Test:

```bash
python -u agents/aws_researcher.py
```

---

## Task 3: Automate AWS Resource Management

Run the AWS manager agent:

```bash
python -u agents/aws_manager.py
```

Create DynamoDB table:

```
Create a dynamodb table called flights with string key "FlightNumber"
```

Populate table:

```
Put batch.json in dynamodb table 'flights'
```

Verify in AWS Console → DynamoDB → Tables → flights.

---

## Task 3.2: Extend AWS Manager Capabilities

Update system prompt in `aws_manager.py`:

```python
system_prompt="""You are an AWS operations assistant.

- AWS operations → AWS API MCP tools
- File operations → file_read
- Sleep → sleep tool
"""
```

Test S3 listing:

```
Tell me what S3 buckets I have in us-east-1
```

---

## Task 4: Orchestrate Multi-Agent Operations

Add `@tool` decorator to all agent functions:

```python
@tool
def coder(query: str) -> str:
```

```python
@tool
def alarms(query: str) -> str:
```

```python
@tool
def aws_researcher(query: str) -> str:
```

```python
@tool
def aws_manager(query: str) -> str:
```

Import tools in `orchestrator.py`:

```python
from coder import coder
from aws_researcher import aws_researcher
from alarm_manager import alarms
from aws_manager import aws_manager
```

Orchestrator system prompt:

```python
ORCHESTRATOR_SYSTEM_PROMPT = """
Route user requests to the correct agent.
Do not ask follow-up questions until the task is complete.
"""
```

Register tools:

```python
tools=[coder, alarms, aws_researcher, aws_manager]
```

---

## Task 4.2: Deploy Flight Website via Orchestrator

Run orchestrator:

```bash
python -u agents/orchestrator.py
```

Prompt (replace values):

```
Check if an alarm indicates the client system is down.
If it is, create a backup EC2 instance in REGION using the
AnyCompanyFlightTracker launch template in subnet SUBNET_ID
and return the public IP.
```

Open the returned Public IP in a browser to view flight data.

---

## Conclusion

You successfully:

- Built intelligent AWS DevOps agents using Strands SDK  
- Integrated real-time AWS documentation via MCP  
- Automated DynamoDB and EC2 operations  
- Created a multi-agent orchestrator for AWS DevOps workflows  

---

## End Lab

1. Sign out of the AWS Console  
2. Choose **End Lab**  
3. Confirm  

