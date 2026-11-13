# LangChain notebooks

Notebooks from 2023–2024, updated in November 2026 to install and import on the LangChain 1.5 line.

The examples are unchanged in intent: prompt pipelines, chains, tools, PAL, tree-of-thought, plan-and-execute, vector search, LangSmith, and RAGAS. Imports now use the current packages.

| What the notebook used | Where it lives now |
| --- | --- |
| `langchain.chat_models.ChatOpenAI`, `OpenAI`, embeddings | `langchain_openai` |
| Legacy chains (`LLMChain`, `ConversationChain`, `LLMCheckerChain`) and the old agent executors | `langchain_classic` |
| Search, FAISS, document loaders | `langchain_community` 0.4.1 |
| PAL, tree-of-thought, plan-and-execute | `langchain_experimental` |
| Prompt templates, output parsers, documents | `langchain_core` |

The maintained tutorial series for current agent work is [langchain-showcase](https://github.com/cobusgreyling/langchain-showcase).

## Setup

Python 3.11 or newer.

```bash
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
```

Set `OPENAI_API_KEY` before running a notebook. Search notebooks also need `SERPAPI_API_KEY`. The web-scraping notebook needs `GOOGLE_API_KEY`, `GOOGLE_CSE_ID`, and the optional packages listed at the bottom of `requirements.txt`.

`langchain-community` stays on 0.4.1 so the RAGAS notebooks can import. RAGAS 0.4.3 does not import against `langchain-community` 0.4.2.

## Notebooks

| Notebook | Topic |
| --- | --- |
| `LangChain_1.ipynb` | Prompts, `LLMChain`, sequential and conversation chains |
| `LangChain_2.ipynb` | Hugging Face hub chain and conversation memory |
| `LangChain_Prompt_Pipeline.ipynb` | Prompt templates with a chat model |
| `LangChain_Compostition_pynb.ipynb` | Composing prompts. `PipelinePromptTemplate` was removed, so this notebook formats the pieces directly |
| `LangChain_LLM_Based_Agent.ipynb` | Plan-and-execute agent with search and math |
| `LangChain_Plan_and_solve_prompting.ipynb` | Same plan-and-execute pattern |
| `LangChain_PAL.ipynb` | Program-aided language models. Generated code still runs |
| `LangChain_Tree_of_Thought_(ToT)_example.ipynb` | Tree of thought |
| `Self_checking_chain_Langchain.ipynb` | `LLMCheckerChain` |
| `Working_Langchain_Tools_Vector_DB.ipynb` | Tools and FAISS |
| `HITL_Working_Langchain_Agent_HITL.ipynb` | Agent with tools |
| `LangChain_Step_Back_Prompting.ipynb` | Step-back prompting and search |
| `LangSmith_Walk_Through.ipynb` | LangSmith tracing and evaluation |
| `Ragas_RAG_Assess.ipynb` | RAGAS 0.4 metrics on the FIQA sample |
| `LangSmith_Ragas_RAG_Assess.ipynb` | LangSmith plus RAGAS |
| `docs/extras/use_cases/web_scraping.ipynb` | Loaders, extraction, and web research |

Retired model names (`text-davinci-003`, `gpt-4-0314`, `gpt-3.5-turbo-0613`, `gpt-3.5-turbo-16k`) now point at `gpt-4o-mini`.
