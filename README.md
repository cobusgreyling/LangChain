# LangChain practice log

A hands-on notebook log of [LangChain](https://www.langchain.com/) experiments by [Cobus Greyling](https://cobusgreyling.me), Chief Evangelist and AI writer. The work here was written between September 2023 and January 2024, while the library was still organised around chains, early agents and prompt recipes.

The notebooks follow one path. They start with prompts, templates, sequential chains and conversation memory. They then add tools and agents, including human-in-the-loop, web search and a vector store. From there they try prompting strategies that change how a model reasons: Tree of Thought, step-back questions, plan-and-execute and program-aided language models. They finish by asking whether the answers are any good, first with [Ragas](https://docs.ragas.io/) metrics on a RAG dataset, then with [LangSmith](https://docs.langchain.com/langsmith/) traces, datasets and evaluators.

That is still a useful way to read the repo. The model is only part of the system. The prompt, tools, memory, retrieval and evaluation around it decide what you actually get.

These notebooks target the LangChain APIs of 2023–2024 (`initialize_agent`, `LLMChain`, `langchain.llms.OpenAI`, and similar). They have not been tested against current LangChain 1.x packages. See [What's current in LangChain](#whats-current-in-langchain-as-of-november-2026) if you want to map an idea onto today's APIs.

## Flagship examples

Three notebooks show the main themes of the log most clearly.

### 1. Human-in-the-loop agent

[HITL_Working_Langchain_Agent_HITL.ipynb](HITL_Working_Langchain_Agent_HITL.ipynb) builds a zero-shot ReAct agent with `initialize_agent` and `AgentType.ZERO_SHOT_REACT_DESCRIPTION`. The first tool set loads LangChain's `human` tool plus `llm-math`, so the agent can pause and ask a person when it cannot answer from the model alone. The demo question is "In what month is Cobus Greyling's birthday?" A second run swaps in SerpAPI search and a calculator for a mixed fact-and-math query.

It matters because tool-using agents still need a way to stop and ask for confirmation or missing facts. In current LangChain, the usual equivalent is [`create_agent`](https://docs.langchain.com/oss/python/langchain/agents) with [`HumanInTheLoopMiddleware`](https://docs.langchain.com/oss/python/langchain/middleware/built-in), including the per-call interrupt mode added in LangChain 1.4.6.

### 2. Tree of Thought

[LangChain_Tree_of_Thought_(ToT)_example.ipynb](LangChain_Tree_of_Thought_%28ToT%29_example.ipynb) uses `ToTChain` from `langchain_experimental.tot` to solve a 4x4 Sudoku puzzle. A custom `ToTChecker` scores each thought as a valid intermediate step, a complete solution, or invalid, so the chain can expand promising partial grids and discard illegal ones. The run is also wired to LangSmith tracing.

It matters as a clear, inspectable example of search over reasoning steps rather than a single completion. The checker, the puzzle representation and the `ToTChain(k=30, c=5)` search limits are all in one short notebook.

### 3. RAG evaluation with Ragas and LangSmith

[LangSmith_Ragas_RAG_Assess.ipynb](LangSmith_Ragas_RAG_Assess.ipynb) evaluates a small slice of the [explodinggradients/fiqa](https://huggingface.co/datasets/explodinggradients/fiqa) `ragas_eval` split with Ragas metrics: context relevancy, faithfulness, answer relevancy, context recall and harmfulness. It turns LangSmith tracing on so the same run can be inspected in a project. [Ragas_RAG_Assess.ipynb](Ragas_RAG_Assess.ipynb) is the same evaluation without the LangSmith setup.

It matters because it treats retrieval quality as something you measure, not something you assume. The pattern (dataset, metrics, trace) is still the right one, even though both Ragas and LangSmith have moved on since this notebook was written.

## Notebook catalogue

Filenames are as they appear in the repository, including the typo in `LangChain_Compostition_pynb.ipynb`.

### Agents and tools

| Notebook | What it covers |
| --- | --- |
| [HITL_Working_Langchain_Agent_HITL.ipynb](HITL_Working_Langchain_Agent_HITL.ipynb) | ReAct agent with a human tool, then search and math |
| [LangChain_LLM_Based_Agent.ipynb](LangChain_LLM_Based_Agent.ipynb) | Plan-and-execute agent with SerpAPI search and a calculator |
| [LangChain_Plan_and_solve_prompting.ipynb](LangChain_Plan_and_solve_prompting.ipynb) | Compact version of the same plan-and-execute setup |
| [Working_Langchain_Tools_Vector_DB.ipynb](Working_Langchain_Tools_Vector_DB.ipynb) | Custom agent tools with SerpAPI and a FAISS vector store |

### Prompting and reasoning

| Notebook | What it covers |
| --- | --- |
| [LangChain_Tree_of_Thought_(ToT)_example.ipynb](LangChain_Tree_of_Thought_%28ToT%29_example.ipynb) | Tree of Thought search on a 4x4 Sudoku puzzle |
| [LangChain_Step_Back_Prompting.ipynb](LangChain_Step_Back_Prompting.ipynb) | Step-back questions with LCEL, DuckDuckGo retrieval and a Hub answer prompt |
| [LangChain_PAL.ipynb](LangChain_PAL.ipynb) | Program-aided language models (`PALChain`) that solve a counting problem with generated code |
| [LangChain_Prompt_Pipeline.ipynb](LangChain_Prompt_Pipeline.ipynb) | Composing `PromptTemplate` objects into a single prompt |
| [LangChain_Compostition_pynb.ipynb](LangChain_Compostition_pynb.ipynb) | `PipelinePromptTemplate` that stitches introduction, example and start prompts |

### Chains and memory

| Notebook | What it covers |
| --- | --- |
| [LangChain_1.ipynb](LangChain_1.ipynb) | First pass at LLMs, prompt templates, `LLMChain`, `SequentialChain` and `ConversationBufferMemory` |
| [LangChain_2.ipynb](LangChain_2.ipynb) | Hugging Face BLOOM via `HuggingFaceHub`, plus a conversation chain with memory |
| [Self_checking_chain_Langchain.ipynb](Self_checking_chain_Langchain.ipynb) | `LLMCheckerChain` for self-critique of a factual claim |

### Retrieval and RAG

| Notebook | What it covers |
| --- | --- |
| [docs/extras/use_cases/web_scraping.ipynb](docs/extras/use_cases/web_scraping.ipynb) | `WebResearchRetriever` with Google Search, Chroma and `RetrievalQAWithSourcesChain` |
| [Working_Langchain_Tools_Vector_DB.ipynb](Working_Langchain_Tools_Vector_DB.ipynb) | Also listed under agents: FAISS as a tool-backed knowledge source |

### Evaluation and LangSmith

| Notebook | What it covers |
| --- | --- |
| [LangSmith_Ragas_RAG_Assess.ipynb](LangSmith_Ragas_RAG_Assess.ipynb) | Ragas RAG metrics on FIQA, with LangSmith tracing |
| [Ragas_RAG_Assess.ipynb](Ragas_RAG_Assess.ipynb) | The same Ragas evaluation without LangSmith |
| [LangSmith_Walk_Through.ipynb](LangSmith_Walk_Through.ipynb) | LangSmith client, a Hub-prompted search agent, a labelled dataset and `run_on_dataset` evaluators |

## What's current in LangChain (as of November 2026)

The notes below are from official LangChain docs, GitHub releases and PyPI, opened on 13 November 2026. They are a snapshot, not a migration of these notebooks.

The notebooks in this repo were written against earlier LangChain APIs and have not been re-run on the current 1.x stack. Imports such as `langchain.llms.OpenAI`, `initialize_agent`, `LLMChain`, `PALChain` and `langchain_experimental.plan_and_execute` belong to that earlier surface. In LangChain 1.x, much of that legacy API lives in [`langchain-classic`](https://docs.langchain.com/oss/python/migrate/langchain-v1), and new agents are built with `create_agent`.

- **22 October 2025 — LangChain 1.0 and LangGraph 1.0.** The first stable major releases. LangChain focuses on `create_agent`, middleware and standard content blocks. Legacy chains, retrievers, indexes and hub helpers move to `langchain-classic`. `create_react_agent` in `langgraph.prebuilt` is replaced by `langchain.agents.create_agent`. Python 3.9 support is dropped (3.10+ required at 1.0). Both 1.0 lines are designated LTS. Sources: [announcement](https://www.langchain.com/blog/langchain-langgraph-1dot0), [Python changelog](https://docs.langchain.com/oss/python/releases/changelog), [v1 migration guide](https://docs.langchain.com/oss/python/migrate/langchain-v1), [release policy](https://docs.langchain.com/oss/python/release-policy).
- **12 May 2026 — LangChain 1.3.0, LangGraph 1.2.0 and Deep Agents 0.6.0.** LangChain agents gain `stream_events` / `astream_events` with `version="v3"`. LangGraph 1.2.0 adds `DeltaChannel` (beta) for incremental checkpoints, per-node timeouts, node-level error handlers, graceful shutdown, and a content-block-centric v3 streaming API. Deep Agents 0.6.0 adopts `DeltaChannel` for message history and files; rolling back those threads to an earlier Deep Agents version is not supported once the new checkpoint format is persisted. Source: [changelog](https://docs.langchain.com/oss/python/releases/changelog).
- **24 July 2026 — Deep Agents 0.7.0.** A leaner default harness (the changelog reports a 65% drop in default-agent input tokens on their eval suite). Breaking changes include making planning todos opt-in (`TodoListMiddleware` is no longer included by default), removing backend compatibility shims, and changing `ls` / `glob` / `read_file` output formats. Deep Agents remains pre-1.0, so minor versions may still break APIs. Sources: [changelog](https://docs.langchain.com/oss/python/releases/changelog), [release policy](https://docs.langchain.com/oss/python/release-policy).
- **1 September 2026 — LangChain 1.4.0.** MCP support ships in the `langchain.mcp` namespace (`pip install "langchain[mcp]"`), built on FastMCP, and is intended to replace the standalone `langchain-mcp-adapters` package. `MCPAdapter` lists tools for `create_agent`, supports interrupt-driven elicitation, and authenticates through FastMCP. The namespace is beta. Source: [changelog](https://docs.langchain.com/oss/python/releases/changelog).
- **14 October 2026 — LangChain 1.4.6.** `HumanInTheLoopMiddleware` can pause once per tool call with `interrupt_mode="per_call"`. Interrupts carry a `response_schema`, and invalid answers are rejected before they are saved. LangChain plans to make per-call mode the default in the next major release. Source: [changelog](https://docs.langchain.com/oss/python/releases/changelog).
- **22 October 2026 — langchain-core 1.7.0.** Tool-call arguments stream as incremental `tool-call-delta` events instead of cumulative `block-delta` events. Upgrade stream clients before upgrading the server. Iterating `message.tool_calls` yields only the new part of each call's arguments from `langchain-core` 1.6.13 onward. Source: [changelog](https://docs.langchain.com/oss/python/releases/changelog).
- **11 November 2026 — LangChain 1.5.0 and langchain-core 1.8.0.** As of this writing, [PyPI lists `langchain` 1.5.0](https://pypi.org/project/langchain/1.5.0/) as the current release, requiring `langchain-core>=1.8.0` and Python 3.11 or later. `langchain-core` 1.8.0 [drops Python 3.10](https://github.com/langchain-ai/langchain/releases/tag/langchain-core%3D%3D1.8.0). New work includes a Decisions API (`langchain_core.decisions.BaseDecisionModel`) and beta decision-driven middleware (`ModelRouterMiddleware`, `AutoModeMiddleware`). TypeSafe experimental middleware imports are deprecated in favour of `langchain.agents.middleware` with an explicit `classifier=`. Source: [changelog](https://docs.langchain.com/oss/python/releases/changelog).
- **LangSmith (2–9 November 2026).** Cloud updates include coding-agent GitHub pull-request outcome and spend tracking, continuous Online Insights, plain-language AI search over tracing projects, scoped tool policies with call approvals, and LLM Gateway requests grouped as a single trace. Source: [LangSmith Cloud changelog](https://docs.langchain.com/langsmith/changelog).
- **Support window.** LangChain 1.x and LangGraph 1.x are LTS and remain ACTIVE until 2.0. LangChain 0.3 and LangGraph 0.4 are in maintenance until December 2026. Source: [release policy](https://docs.langchain.com/oss/python/release-policy).

Current starting points:

- [Agents (`create_agent`)](https://docs.langchain.com/oss/python/langchain/agents)
- [LangChain v1 migration guide](https://docs.langchain.com/oss/python/migrate/langchain-v1)
- [Python package changelog](https://docs.langchain.com/oss/python/releases/changelog)
- [LangSmith docs](https://docs.langchain.com/langsmith/changelog)

## Getting started

Open any notebook in [Google Colab](https://colab.research.google.com/) or a local Jupyter environment.

The notebooks install packages with `pip` inside cells and call hosted models. You will typically need some of:

```bash
export OPENAI_API_KEY="..."
export LANGCHAIN_API_KEY="..."      # LangSmith notebooks
export SERPAPI_API_KEY="..."        # search-tool notebooks
export HUGGINGFACEHUB_API_TOKEN="..."  # LangChain_2.ipynb
```

Prefer environment variables or your notebook host's secret store. Do not commit keys. Some of the older cells assign keys in the notebook itself; replace those with env lookups before you run or share a copy.

A few practical constraints:

- Several demos use retired or legacy model names (for example `text-davinci-003` and `gpt-4-0314`). You will need a current model and the matching provider package, such as `langchain-openai`.
- Search, Hugging Face and LangSmith features need their own accounts and keys.
- Current `langchain` 1.5 / `langchain-core` 1.8 require Python 3.11+. These notebooks were written earlier and may only run if you pin older packages, or if you rewrite imports against `langchain-classic` and today's agent APIs.

## Links

- Site and writing: [cobusgreyling.me](https://cobusgreyling.me)
- This repository: [github.com/cobusgreyling/LangChain](https://github.com/cobusgreyling/LangChain)
- LangChain documentation: [docs.langchain.com](https://docs.langchain.com/)
- LangChain Python changelog: [docs.langchain.com/oss/python/releases/changelog](https://docs.langchain.com/oss/python/releases/changelog)
