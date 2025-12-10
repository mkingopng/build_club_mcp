# MCP Server on Amazon Bedrock AgentCore Runtime (Minimal README)

This project demonstrates how to **build, test, and deploy an MCP (Model Context Protocol) server** onto **Amazon Bedrock AgentCore Runtime** with **OAuth (Cognito) authentication**.
It also includes local and remote MCP clients for testing tool discovery and invocation.

---

## Features

* MCP Server built with **FastMCP**
* Three example tools:

  * `add_numbers`
  * `multiply_numbers`
  * `greet_user`
* Local MCP client for testing
* Cognito-secured AgentCore Runtime deployment
* Remote client with automatic JWT refresh
* End-to-end demo: build → deploy → authenticate → invoke tools

---

## Project Structure

```
mcp_server_project/
├── mcp_server.py                 # MCP server implementation
├── my_mcp_client.py             # Local testing client
├── my_mcp_client_remote.py      # Remote (Cognito-authenticated) client
├── invoke_mcp_tools.py          # Remote invocation demo (tool calls)
├── requirements.txt
└── notebooks/
    ├── hosting_mcp_server.ipynb                # Main tutorial notebook
    └── runtime_with_strands_and_bedrock.ipynb # Additional runtime examples
```

---

## Local Development

### Start the MCP server

```bash
python mcp_server.py
```

### List available tools

```bash
python my_mcp_client.py
```

---

## Deploy to AgentCore Runtime

Deployment is handled via the **AgentCore Starter Toolkit**, which:

* Generates a Dockerfile
* Builds and pushes container to ECR
* Creates an AgentCore Runtime
* Configures OAuth via Cognito

Run the deployment steps in the accompanying notebook:

* **Hosting MCP Server on Amazon Bedrock AgentCore Runtime – OAuth Inbound Authentication**


This notebook creates:

* Cognito User Pool + App Client
* JWT-based authorizer configuration
* AgentCore Runtime instance
* SSM + Secrets Manager entries for connection details

---

## Remote Invocation

Once deployed, you can test the MCP server using:

```bash
python my_mcp_client_remote.py
```

Or invoke specific tools:

```bash
python invoke_mcp_tools.py
```

The clients automatically:

* Fetch Agent ARN from SSM
* Fetch Cognito tokens (and refresh when needed)
* Connect to the MCP server over **streamable HTTP**
* Perform `initialize`, `list_tools`, and `call_tool` operations

---

## Requirements

* Python 3.10+
* AWS credentials configured
* Docker
* Amazon Bedrock AgentCore Starter Toolkit
* FastMCP & MCP libraries

Full dependencies listed in:

* **requirements.txt**

Notebooks reference:

* AWS Show & Tell session: AgentCore + Gateway deep dive

* AgentCore Runtime tutorial with Strands Agents


---

## Cleanup

To remove deployed resources, use the cleanup section in the notebook:

* Deletes AgentCore runtime
* Removes ECR repo
* Removes SSM parameter + Secrets Manager entries
* always check manually via thee AWS console to ensure that the cleanup has been effective
