# Model Context Protocol 

- MCP Hosts:  AI application that coordinates and manages one or multiple MCP clients ie. Claude, Cursor
- MCP Client: A component that maintains a connection to an MCP server and obtains context from an MCP server for the MCP host to use
- MCP Server: A program that provides context to MCP clients

<img width="1341" height="666" alt="image" src="https://github.com/user-attachments/assets/be18c5a6-a87b-43cb-ab93-be3b3a3c7645" />

Everything is in JSON

## Transport Layer for MCP protocol
- stdio or httpp+ sse
- JSON-RPC

- no encryption

- tool - get_weather,

## MCP Primitives

- Notifications
- Server
  1. tools
  2. resources
  3. prompts
- Client
  1. sampling
  2. roots
  3. elicitation
 
  <img width="1341" height="786" alt="image" src="https://github.com/user-attachments/assets/d8892d32-bc1c-499d-915e-4d41d25d2546" />

<img width="1309" height="447" alt="image" src="https://github.com/user-attachments/assets/4ff69f99-1f59-460b-be0c-4cc8a219afba" />

### MCP Communication
* mcp COMMUNICATES VIA JSON
* JSON can be sent over by multiple ways
  - stdio
  - streamble http
 
  - STDIO
    <img width="1290" height="655" alt="image" src="https://github.com/user-attachments/assets/d44a9433-d47e-4973-ba94-97e67080d1c0" />

  - Streamable HTTP
 
    <img width="1279" height="622" alt="image" src="https://github.com/user-attachments/assets/f92f506e-bf63-4a87-94fe-0c68082c4b46" />



MCP plus DEV workflow

Give control to LLM

if you don't have paid version of co-pilot, you can use openai codex 

Context7 - anything you want to work on there is documentation...   Airflow,  scan document and preserve context of tool,  astro...    pydanticAI, langchain...  github mcp server to integrate with vs-code etc... 

airflow to write piplines,  use library to get transcripts.  

#### mcp servers
- extensions @mcp,   context7, github, playwright

  hot to integrate with mcp server... MCP open configuration, mcp open user configuration...

  add the hashnode mcp server...  ,if you want only for the project, you can update mcp.json for the specific project...

  How to know it's installed...  configure tools,   some built in tools, and some external tools.  , remove tools, .

  <img width="1375" height="756" alt="image" src="https://github.com/user-attachments/assets/77c9ce6f-f0e0-455f-9e27-4f3c18c67042" />

<img width="1387" height="763" alt="image" src="https://github.com/user-attachments/assets/56f042be-dadc-4adc-b9a7-a454f58b75e7" />


