# MCP — Model Context Protocol
### A Qualitative and Quantitative Analysis

**Author:** Nykoll Ortiz Vasquez  
**Date:** June 2026  
**Category:** Artificial Intelligence · Automation · CRM  
**Report Version:** 1.0  
**Language:** English  

---

## Executive Summary

The **Model Context Protocol (MCP)** is an open communication standard developed by Anthropic and publicly released on November 25, 2024. Its core purpose is to standardize how large language models (LLMs) connect with external tools, data sources, and third-party systems — replacing the need for one-off, point-to-point integrations.

In practical terms: MCP gives an AI model a structured "key ring" to enter your applications and act on your behalf — without you copying, pasting, or acting as the middleman between your AI assistant and your tools.

This report examines MCP from both a qualitative perspective (architecture, use cases, limitations, user experience) and a quantitative perspective (adoption metrics, ecosystem size, workflow efficiency gains).

---

## 1. Background and Origin

### 1.1 The Problem Before MCP

Prior to MCP, connecting a language model to an external tool required:

- A custom, model-specific API integration for every tool combination
- The user acting as a manual bridge: copy data from app → paste into chat → copy response → carry it back to the app
- AI companies duplicating integration development for every platform they wanted to support

The result: fragmentation, high maintenance costs, and slow, inconsistent user experiences.

### 1.2 Protocol Launch

| Data Point | Value |
|---|---|
| Developing Organization | Anthropic |
| Public Launch Date | November 25, 2024 |
| Initial Protocol Version | 2024-11-05 |
| License | Open Source — MIT |
| Official Repository | github.com/modelcontextprotocol |
| Protocol Type | Open standard (non-proprietary) |
| Underlying Transport Standard | JSON-RPC 2.0 |

### 1.3 Industry Analogies

- **"USB-C for AI"** — A universal connector that works regardless of the device (tool) or charger (AI model). The same plug, always compatible.
- **"LSP for AI models"** — Analogous to the Language Server Protocol, which unified how IDEs communicate with compilers and linters, ending the era of custom per-editor integrations.
- **"Gojo's Infinity (Jujutsu Kaisen)"** — The model doesn't manually filter each data point. MCP acts as an invisible, automatic shield that handles all context retrieval transparently. *(Personal analogy — surprisingly effective for non-technical audiences.)*

---

## 2. Qualitative Analysis

### 2.1 Protocol Architecture

MCP defines three primary roles in every interaction:

```
┌──────────────────────────────────────────────────────────────┐
│                          HOST                                │
│               (e.g., Claude Desktop, Cursor)                 │
│                                                              │
│   ┌──────────────┐          ┌──────────────────────────┐    │
│   │    CLIENT    │◄────────►│       MCP SERVER         │    │
│   │  (manages    │          │  (exposes tool resources: │    │
│   │  connection) │          │  HubSpot, Drive, GitHub, │    │
│   └──────────────┘          │  Notion, Slack, etc.)    │    │
│                             └──────────────────────────┘    │
└──────────────────────────────────────────────────────────────┘
```

| Component | Role | Real-World Examples |
|---|---|---|
| **Host** | Application that runs the AI model | Claude Desktop, Cursor, Cline, Continue, Zed |
| **Client** | Maintains connection to MCP servers | Embedded within the Host |
| **Server** | Exposes a tool's capabilities to the model | HubSpot MCP, Google Drive MCP, GitHub MCP |

### 2.2 Protocol Primitives

MCP defines three capability types that any server can expose:

| Primitive | Description | Practical Example |
|---|---|---|
| **Resources** | Readable data the model can access | HubSpot contacts, Drive files, calendar events |
| **Tools** | Actions the model can execute | Create a deal, send an email, edit a document |
| **Prompts** | Reusable instruction templates | "Meeting summary", "Sales follow-up generator" |

### 2.3 Transport Mechanisms

| Type | Description | Best For |
|---|---|---|
| **stdio** (local) | Standard input/output communication | Servers running on the same machine as the user |
| **HTTP + SSE** (remote) | Network-based with Server-Sent Events | Cloud-hosted servers, multi-user environments |

### 2.4 Qualitative Advantages

**For end users:**
- Eliminates the "middleman" role between AI and tools
- Natural language becomes the only interface ("review all deals with no response in 7+ days")
- Consistent behavior: the same model can orchestrate multiple tools in a single conversation

**For developers:**
- One MCP server works with any compatible host — build once, deploy everywhere
- Standardized protocol means less custom code per client integration
- JSON-RPC 2.0 base makes debugging straightforward and tooling accessible

**For organizations:**
- Significant reduction in integration development costs
- Faster automation cycles without custom connector engineering
- Reduced vendor lock-in: switching AI providers doesn't require rebuilding tool integrations

### 2.5 Limitations and Considerations

| Limitation | Description |
|---|---|
| **Technical learning curve** | Setting up MCP servers requires working with JSON configs and terminal commands — not plug-and-play for non-technical users |
| **Maturing ecosystem** | As a 2024 protocol, not every tool has an official MCP server yet; community servers vary in quality |
| **Remote server trust model** | Remote MCP servers have access to potentially sensitive data; trust evaluation is the user's responsibility |
| **Host dependency** | Experience quality varies by how deeply a host implements MCP — Claude Desktop vs. others differ significantly |
| **No native auth standard (v1)** | The initial specification lacked a built-in authentication framework; being addressed in subsequent versions |

---

## 3. Quantitative Analysis

### 3.1 Ecosystem Adoption (as of report date)

| Metric | Value | Source |
|---|---|---|
| Compatible hosts (official) | 10+ | MCP Official Documentation |
| Reference MCP servers (Anthropic) | 15+ at launch | github.com/modelcontextprotocol/servers |
| Community MCP servers (public repos) | 1,000+ | mcp.so directory, awesome-mcp-servers |
| Tools integrated at launch | 10 official (Brave Search, Filesystem, Git, GitHub, Google Drive, Google Maps, Memory, PostgreSQL, Slack, SQLite) | Anthropic Launch Blog |
| GitHub stars — MCP spec repo | Growing rapidly (verify current count at repository) | github.com/modelcontextprotocol |

### 3.2 Compatible Hosts and Applications

| Application | Type | MCP Support |
|---|---|---|
| Claude Desktop | AI Assistant | ✅ Native (protocol creator) |
| Cursor | Code Editor | ✅ Integrated |
| Windsurf (Codeium) | Code Editor | ✅ Integrated |
| Cline (VS Code Extension) | AI Coding Assistant | ✅ Integrated |
| Continue | AI Coding Assistant | ✅ Integrated |
| Zed | Code Editor | ✅ Integrated |
| OpenAI (ChatGPT) | AI Assistant | 🔄 Adopting (2025) |

### 3.3 Comparison with Prior Integration Approaches

| Criterion | Manual API Integration | Pure Function Calling | MCP |
|---|---|---|---|
| **Reusability** | Per client | Per model | Universal |
| **Integration effort** | High | Medium | Low |
| **Interoperability** | None | Limited | Full |
| **Capability types** | Tools only | Tools only | Resources + Tools + Prompts |
| **Open standard** | No | No | Yes |
| **Real-time data access** | Manual | Manual | Automatic |
| **Context window efficiency** | Low | Medium | High (streamed resources) |

### 3.4 Workflow Efficiency — CRM Context

Estimated step reduction for common tasks when using MCP vs. not using MCP:

| Task | Steps Without MCP | Steps With MCP | Reduction |
|---|---|---|---|
| Review deals with no response | 6 manual steps | 1 natural language prompt | ~83% |
| Generate weekly performance report | 8 steps | 1 prompt | ~87% |
| Write personalized follow-up emails | 5 steps per contact | 1 prompt (batch) | ~80% |
| Segment contacts by custom criteria | 4 steps | 1 prompt | ~75% |

> **Methodology note:** These estimates are based on observed CRM workflow analysis in real HubSpot environments, not controlled benchmark studies. They represent practical efficiency gains, not vendor-published performance data.

---

## 4. CRM and Marketing Applications

### 4.1 Priority Use Cases

**For CRM Specialists:**

```
Prompt: "Review all pipeline deals with no activity in the last 7 days and 
draft a personalized follow-up for each, based on the last note logged."

→ MCP + HubSpot: Reads deals → Filters by date → Reads notes → Generates emails
```

```
Prompt: "Which 5 contacts have the highest lead score and haven't been 
contacted this week?"

→ MCP + HubSpot: Accesses properties → Filters → Returns ranked list
```

**For Marketing Teams:**

```
Prompt: "Build this month's KPI campaign report compared to last month, 
formatted for a stakeholder presentation."

→ MCP + Google Sheets / HubSpot: Extracts data → Compares → Formats report
```

### 4.2 CRM Tools with MCP Servers Available

| Tool | MCP Server | Status |
|---|---|---|
| HubSpot | Community MCP (via Zapier / custom) | Available |
| Notion | Official Notion MCP | Available |
| Google Drive | Official Google Drive MCP | Available |
| Slack | Official Slack MCP | Available |
| GitHub | Official GitHub MCP | Available |
| Airtable | Community MCP | Available |
| Salesforce | In development | Upcoming |

---

## 5. Conclusions

### 5.1 Is MCP Worth Adopting in 2026?

**Yes — with context.**

For **technical users or teams with development resources**: MCP represents a genuine paradigm shift. The friction reduction in multi-tool workflows is immediate and measurable. The investment in setting up MCP servers pays back within the first week of real use.

For **non-technical users**: Adoption depends on the host. Claude Desktop makes the configuration curve manageable, but it's not trivial. The potential is there; the barrier is the initial setup — and that barrier is shrinking as tooling improves.

### 5.2 Expected Trajectory

MCP is at the same inflection point cloud computing was at in 2008–2010: not yet the default, but the direction is unambiguous. Organizations that build workflows on this protocol today will hold a meaningful operational advantage in 12–18 months when MCP support becomes standard across business software.

### 5.3 Practical Recommendation (User Level)

1. Install **Claude Desktop** (free with any Claude plan)
2. Configure the **Filesystem MCP server** as a first step (access your local files)
3. Add **Google Drive** or **Notion** based on your existing tool stack
4. Run real tasks before connecting critical or production systems
5. Treat MCP like any new tool: test with low-stakes tasks first, scale once comfortable

---

## 6. Sources and References

| # | Source | Type | URL |
|---|---|---|---|
| 1 | Anthropic — Introducing the Model Context Protocol | Official blog | https://www.anthropic.com/news/model-context-protocol |
| 2 | MCP Specification — Official Documentation | Technical documentation | https://modelcontextprotocol.io/docs |
| 3 | GitHub — modelcontextprotocol/modelcontextprotocol | Official specification repo | https://github.com/modelcontextprotocol/modelcontextprotocol |
| 4 | GitHub — modelcontextprotocol/servers | Reference server implementations | https://github.com/modelcontextprotocol/servers |
| 5 | MCP.so — Community Server Directory | Community resource | https://mcp.so |
| 6 | awesome-mcp-servers (punkpeye) | Curated community list | https://github.com/punkpeye/awesome-mcp-servers |
| 7 | JSON-RPC 2.0 Specification | Technical standard | https://www.jsonrpc.org/specification |
| 8 | HubSpot Developer Documentation | API reference | https://developers.hubspot.com |
| 9 | Anthropic Documentation — Claude API | Official docs | https://docs.anthropic.com |

---

## Methodological Notes

- Quantitative adoption data corresponds to the period November 2024 – June 2026.
- Step-reduction estimates in CRM workflows are observational, drawn from real workflow analysis — not controlled benchmarks or vendor-published data.
- The compatible tools table reflects the state of the ecosystem at publication; the MCP ecosystem evolves rapidly. Always verify current status at the official directory.
- This report was produced as educational and professional content, not formal academic research.
- All company and product names are trademarks of their respective owners.

---

*Written by Nykoll Ortiz Vasquez — CRM Specialist & AI Educator*  
*Repository: github.com/nykollortiz/maskai*  
*Content License: CC BY 4.0 — Free to use with attribution*
