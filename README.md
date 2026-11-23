<div align="center">

# 🧪 AI Lab

**Learning AI by building it, one layer at a time.**

From an agent that teaches itself a video game to a multi-model AI system that turns 100,000 customer reviews into a product decision.

<sub>Tomas Diez Canedo · built while taking <i>Foundations of Agentic AI</i> at UC Berkeley Haas, Fall 2026</sub>

<br>

[![01](https://img.shields.io/badge/01-Reinforcement_learning-6f42c1?style=flat-square)](https://github.com/tomas10k/pacman-rl-agent)
[![02](https://img.shields.io/badge/02-Transformers_from_scratch-0969da?style=flat-square)](https://github.com/tomas10k/tiny-gpt-from-scratch)
[![03](https://img.shields.io/badge/03-Local_LLMs_+_RAG-1a7f37?style=flat-square)](https://github.com/tomas10k/offline-notes-ai)
[![04](https://img.shields.io/badge/04-Multi--agent_LLM_systems-d97757?style=flat-square)](https://github.com/tomas10k/spotify-review-insights)

</div>

---

Each build answers the question the previous one raised. First: *how does a machine learn at all?* Then: *what's actually inside a language model?* Then: *how do you make one useful and trustworthy on your own data?* Finally: *how do you run several models together as a reliable system that makes a real business call?*

Every repo has runnable code, saved evidence, and an honest write-up of what didn't work.

## ⭐ Featured: Spotify Review Insights

<a href="https://github.com/tomas10k/spotify-review-insights"><img src="https://raw.githubusercontent.com/tomas10k/spotify-review-insights/main/docs/images/dashboard.png" alt="Spotify Review Insights dashboard" width="100%"></a>

**An AI pipeline that read 100,000 Spotify app reviews and recommended where the product team should focus next quarter, for $4.74 in API costs.**

- **Several models, each with one narrow job:** Claude Haiku labels the reviews, a second model checks a sample *blind*, and Claude Sonnet writes the memo. Code does all counting and ranking.
- **The AI never types a number:** the memo references facts by ID, and code fills in and verifies every figure.
- **Built like production:** it survived a deliberate mid-run kill and a real network outage without re-sending a review. A spend ledger can't overshoot its budget, and prompt-injection attacks were tested.
- **The answer:** billing complaints grew from 3.8% → 13.2% of reviews year over year, so the recommendation was **billing & support**.

[**Repository**](https://github.com/tomas10k/spotify-review-insights) · [**Live dashboard**](https://haas-ai-a5-spotify-insight-pipeline.vercel.app/) · [**3-min demo**](https://drive.google.com/file/d/1pMleGuY7WNcjZznoMf1LIN9h-5viLjO8/view?usp=sharing)

## The builds

| | I was learning… | So I built… | Result | Stack |
|:-:|---|---|---|---|
| **01** | how machines learn from trial and error | [**Pac-Man RL agent**](https://github.com/tomas10k/pacman-rl-agent): a Deep Q-Network that learns Ms. Pac-Man from raw pixels | **+43%** score vs. untrained; a 10× memory experiment lifted late-training average 712 → 916 | PyTorch · Gymnasium · GPU |
| **02** | what's inside a language model | [**Tiny GPT from scratch**](https://github.com/tomas10k/tiny-gpt-from-scratch): a nanoGPT trained from random weights, plus my own teaching data | Eval score **20 → 27 / 48** after a targeted corpus | PyTorch · nanoGPT |
| **03** | how to ground an LLM in my own data, privately | [**Offline notes AI**](https://github.com/tomas10k/offline-notes-ai): a RAG assistant running Gemma on my laptop with Wi-Fi off | **~1 s** cited answers; says "insufficient evidence" instead of guessing | Gemma 4 · MLX · BM25 |
| **04** | how to run many models as one reliable system | [**Spotify review insights**](https://github.com/tomas10k/spotify-review-insights): a multi-model pipeline + SQL backend + live dashboard | **100k reviews for $4.74**, 88% agreement with human labels | Claude · SQLite · Vercel |

## A closer look

### 🕹️ 01 · Reinforcement learning: an agent that teaches itself Ms. Pac-Man
<img align="right" src="https://raw.githubusercontent.com/tomas10k/pacman-rl-agent/main/results/final_best.gif" width="160" alt="Trained agent playing Ms. Pac-Man">

I trained a Deep Q-Network for 500 games on a GPU, then ran a controlled experiment: the same games and exploration, but **10× the replay memory**. Late-training scores rose from 712 to 916, and games above 2,000 points went from 3 to 15. The write-up also explains why five evaluation games can't separate the two runs, and why rising loss didn't mean worse play.

**What it shows:** experiment design · hyperparameter reasoning · reading noisy results honestly

→ [**pacman-rl-agent**](https://github.com/tomas10k/pacman-rl-agent)

<br clear="right">

### 🧠 02 · Transformers: a language model trained from scratch
I trained a ~112k-parameter GPT from random weights and traced the training mechanics: one word's embedding before and after, the very first gradient update, attention weights, and next-word probabilities going from flat to confident. I then wrote a targeted teaching corpus (with a leakage check against the test set), which took the eval from **20/48 to 27/48**. I wrote predictions down *before* each run and compared them with the results.

**What it shows:** tokenization & embeddings · training dynamics · data-centric improvement · eval hygiene

→ [**tiny-gpt-from-scratch**](https://github.com/tomas10k/tiny-gpt-from-scratch)

### 📚 03 · Local LLMs & RAG: a private AI assistant for my notes
`wiki` is a command-line tool that answers questions about my notes using **Gemma 4 running locally** on a MacBook, with Wi-Fi off. It retrieves passages with BM25 and cites them as `[S1]…[S5]`, and code verifies every citation. I wrote the tests before building and iterated the prompts through three versions. The write-up diagnoses the one failure, a paraphrased question that keyword search missed, and the hybrid-retrieval fix it points to.

**What it shows:** RAG · on-device model deployment · prompt iteration · grounding & citation checks

→ [**offline-notes-ai**](https://github.com/tomas10k/offline-notes-ai)

### 📊 04 · Multi-agent systems: an AI pipeline for product analytics
See [Featured](#-featured-spotify-review-insights) above. Six stages (prepare → classify → verify → group → rank → recommend), each reading only the previous stage's saved output. Evaluation covers a hand-labeled golden set, a blind verifier and fault-injection tests. A cost model predicted spend before the run, and a deployed dashboard traces any review end to end.

**What it shows:** multi-agent orchestration · LLM evaluation · cost & reliability engineering · data pipelines · full-stack deployment

→ [**spotify-review-insights**](https://github.com/tomas10k/spotify-review-insights)

## How I work

| | |
|---|---|
| 🧪 **Measure before claiming** | Each build has a fixed evaluation set up before the results came in: seeds, test questions, or human labels |
| 🔁 **Reproducible** | Pinned dependencies, saved run outputs, and commands to replay the results |
| 🧾 **Show the evidence** | Screenshots, transcripts, logs and per-case results are committed next to the code |
| ⚠️ **Honest about limits** | Every write-up names what failed and the next experiment I'd run |
| 🔐 **No secrets in git** | Keys stay in `.env`; only blank `.env.example` files are committed |
