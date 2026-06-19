# 📚 Literature Review AI Agent

An autonomous AI agent that automates academic literature reviews fetching papers, summarizing them with an LLM, identifying research gaps, and writing a structured literature review, all for free.

Built as a Kaggle notebook using free APIs and a free LLM, with zero cost and no paid infrastructure.

---

### 📈 Why I built it

The goal was to create a practical AI tool that reduces the repetitive work involved in early-stage research and helps researchers focus more on analysis and idea generation rather than manual paper organization. It may also help me my upcoming thesis project.

This project also gave me hands-on experience with:

  - LLM-powered information extraction
  - Research automation workflows
  - Prompt engineering
  - Data processing pipelines
  - Report generation systems

## Version 1.0  Core Agent (Current)

This version builds the foundational pipeline: fetch papers, summarize with an LLM, analyze gaps, and generate a literature review.

### Features

- **Multi-source paper fetching**  pulls papers from arXiv and CrossRef based on topic, year range, and paper count
- **LLM-powered summarization**  uses Groq's free API (Llama 3.1) to extract structured insights from each paper:
  - Core finding
  - Methodology
  - Key contribution
  - Limitation
  - Relevance to the research topic
- **Automated gap analysis**  identifies research gaps, conflicting findings, and methodological weaknesses across all fetched papers
- **Gap scoring**  rates each identified gap by novelty and feasibility, with a verdict on whether it's worth pursuing
- **Literature matrix export**  generates a formatted Excel spreadsheet with one row per paper and wrapped text cells for readability
- **Auto-generated literature review**  produces a full written review (~500 words) structured into introduction, thematic analysis, field evolution, gaps, and conclusion
- **Interactive UI**  input topic, year range, and paper count directly in the notebook via widgets, no code editing required
- **Timestamped outputs**  every run saves uniquely named files so previous reviews are never overwritten

### Tech Stack

| Component | Tool | Cost |
|---|---|---|
| LLM | Groq API (Llama 3.1 8B) | Free (14,400 req/day) |
| Paper source | arXiv API | CrossRef API |
| Spreadsheet export | openpyxl 
| Environment | Kaggle Notebooks 
| UI | ipywidgets 

### How It Works

```
User input (topic, years, paper count)
        |
Fetch papers from arXiv + CrossRef
        |
Deduplicate by title
        |
Summarize each paper with Groq LLM (5 structured fields)
        |
Analyze gaps across all papers
        |
Score gaps by novelty + feasibility
        |
Write full literature review
        |
Export: literature_matrix_<timestamp>.xlsx
         literature_review_<timestamp>.txt
```

### Setup

1. Get a free Groq API key at [console.groq.com](https://console.groq.com) (no credit card required)
2. Open the notebook on Kaggle
3. Add your key as a Kaggle Secret:
   - **Add-ons - Secrets - Add New Secret**
   - Name: `GROQ_API_KEY`
   - Value: your key (starts with `gsk_...`)
4. Run all cells, or use the UI form at the bottom to set your topic, year range, and paper count

### Output Files

| File | Contents |
|---|---|
| `literature_matrix_<timestamp>.xlsx` | One row per paper: title, authors, year, core finding, methodology, contribution, limitation, relevance |
| `literature_review_<timestamp>.txt` | Full written review + scored research gaps + individual paper summaries |

### Known Limitations (v1.0)

- All fetched papers are passed directly into the LLM prompt  this works well up to ~15/20 papers but will hit Groq's token limit on larger batches
- No persistent storage  papers and summaries exist only for the current notebook session
- No semantic search across previously fetched papers
- Gap analysis only considers papers fetched in the current run, not the full body of literature on a topic

These limitations are addressed in **v2.0** (see below).

---

## Version 2.0  RAG + Vector Database (Planned)

The next version will introduce semantic search and retrieval-augmented generation to solve v1's scaling limitations.

### Planned Features

- **Vector store integration**  embed all fetched papers using a free sentence-transformer model and store them in ChromaDB
- **Retrieval-augmented gap analysis**  instead of dumping all papers into one prompt, retrieve only the most semantically relevant papers per question
- **Scales to 100+ papers per run**  no more token-limit failures as paper count grows
- **Persistent paper memory**  previously fetched papers remain queryable across multiple runs in the same session
- **Interactive Q&A mode**  ask follow-up questions about your fetched papers and get answers grounded in retrieved sources (e.g. *"Which papers used deep learning methods?"*)
- **More targeted literature review writing**  each section of the review (thematic analysis, field evolution, gaps) retrieves its own relevant context instead of relying on the same flat list

### Why This Matters

Version 1 works well for quick reviews of 10/15 papers but breaks down at scale due to LLM context limits. Version 2 turns the agent from a one-shot generator into a persistent, queryable research assistant  closer to how real literature review tools (like Elicit or Semantic Scholar's research assistant) operate.

---

## Roadmap

- [x] v1.0  Core fetch + summarize + gap analysis + review generation
- [ ] v2.0  RAG + vector DB for scalable, queryable literature review
- [ ] v3.0  Multi-agent setup (separate agents for fetching, critiquing, and writing)
- [ ] v4.0  Citation graph analysis (which papers cite each other, influence mapping)

---

## Disclaimer

This tool is meant to **assist** the literature review process for academic research, not replace human judgment. Always verify LLM-generated summaries, gap analyses, and claims against the original papers before citing them in your thesis. LLMs can occasionally misrepresent technical details or hallucinate connections that don't exist in the source material.

---

## License

MIT  free to use, modify, and build on for personal or academic projects.
