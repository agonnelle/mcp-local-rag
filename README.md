<a href='https://github.com/nkapila6/mcp-local-rag/'><img src='images/rag.jpeg' width='200' height='200'></a>

[![Open in GitHub Codespaces](https://github.com/codespaces/badge.svg)](https://codespaces.new/nkapila6/mcp-local-rag)

<!-- omit from toc -->
# mcp-local-rag
"primitive" RAG-like web search model context protocol (MCP) server that runs locally. ✨ no APIs ✨

- [Installation](#installation)
    - [Run Directly via `uvx`](#run-directly-via-uvx)
    - [Using Docker (recommended)](#using-docker-recommended)
- [Agent Skills](#agent-skills)
- [Security audits](#security-audits)
- [MCP Clients](#mcp-clients)
- [Examples on Claude Desktop](#examples-on-claude-desktop)
  - [Result](#result)
- [Contributing](#contributing)
- [License](#license)


```mermaid
%%{init: {'theme': 'base'}}%%
flowchart TD
    A[User] -->|1.Submits LLM Query| B[Language Model]
    B -->|2.Sends Query| C[mcp-local-rag Tool]
    
    subgraph mcp-local-rag Processing
    C -->|Search DuckDuckGo| D[Fetch 10 search results]
    D -->|Fetch Embeddings| E[Embeddings from Google's MediaPipe Text Embedder]
    E -->|Compute Similarity| F[Rank Entries Against Query]
    F -->|Select top k results| G[Context Extraction from URL]
    end
    
    G -->|Returns Markdown from HTML content| B
    B -->|3.Generated response with context| H[Final LLM Output]
    H -->|5.Present result to user| A

    classDef default stroke:#333,stroke-width:2px;
    classDef process stroke:#333,stroke-width:2px;
    classDef input stroke:#333,stroke-width:2px;
    classDef output stroke:#333,stroke-width:2px;

    class A input;
    class B,C process;
    class G output;
```

# Installation
Locate your MCP config path [here](https://modelcontextprotocol.io/quickstart/user) or check your MCP client settings. 

### Run Directly via `uvx`
This is the easiest and quickest method. You need to install [uv](https://docs.astral.sh/uv/) for this to work. <br>
Add this to your MCP server configuration:
```json
{
  "mcpServers": {
    "mcp-local-rag":{
      "command": "uvx",
        "args": [
          "--python=3.10",
          "--from",
          "git+https://github.com/nkapila6/mcp-local-rag",
          "mcp-local-rag"
        ]
      }
  }
}
```

### Using Docker (recommended)
Ensure you have [Docker](https://www.docker.com) installed.<br>
Add this to your MCP server configuration:
```json
{
  "mcpServers": {
    "mcp-local-rag": {
      "command": "docker",
      "args": [
        "run",
        "--rm",
        "-i",
        "--init",
        "-e",
        "DOCKER_CONTAINER=true",
        "ghcr.io/nkapila6/mcp-local-rag:v1.0.2"
      ]
    }
  }
}
```

# Agent Skills

This repository includes **Agent Skills** that teach Claude how to effectively use the mcp-local-rag tools. Skills are folders of instructions that Claude loads dynamically to improve performance on specialized tasks.

### Available Skills

**`local-rag-search`** - Teaches Claude best practices for:
- Choosing the right search tool (DuckDuckGo, Google, or deep research)
- Formulating effective search queries
- Tuning parameters for different use cases
- Performing comprehensive multi-engine research

### Using the Skills

**In Claude Desktop:**
1. Go to **Settings** → **Skills**
2. Click **Add Skill** → **Add from folder**
3. Select `skills/local-rag-search/`

**In conversations:**
Once loaded, simply ask Claude to search for information and it will automatically apply the skill's best practices.

Learn more about Agent Skills at the [Anthropic Skills Repository](https://github.com/anthropics/skills).

See the [skills/README.md](skills/README.md) for detailed usage instructions and skill development guidelines.

# Security audits
MseeP does security audits on every MCP server, you can see the security audit of this MCP server by clicking [here](https://mseep.ai/app/nkapila6-mcp-local-rag).

<a href='https://mseep.ai/app/nkapila6-mcp-local-rag'><img src='https://mseep.net/pr/nkapila6-mcp-local-rag-badge.png' width='auto' height='200'></a>

# MCP Clients
The MCP server should work with any MCP client that supports tool calling. Has been tested on the below clients.

- Claude Desktop
- Cursor
- Goose
- Others? You try!

# Examples on Claude Desktop
When an LLM (like Claude) is asked a question requiring recent web information, it will trigger `mcp-local-rag`.

When asked to fetch/lookup/search the web, the model prompts you to use MCP server for the chat.

In the example, have asked it about Google's latest Gemma models released yesterday. This is new info that Claude is not aware about.
<img src='images/mcp_prompted.png'>

## Result
`mcp-local-rag` performs a live web search, extracts context, and sends it back to the model—giving it fresh knowledge:

<img src='images/mcp_result.png'>

## Buy Me A Coffee
If the software I've built has been helpful to you. Please do buy me a coffee, would really appreciate it! 😄

[![ko-fi](https://ko-fi.com/img/githubbutton_sm.svg)](https://ko-fi.com/X8X51MK4A1)

# Contributing
Have ideas or want to improve this project? Issues and pull requests are welcome!

# License
This project is licensed under the MIT License.
