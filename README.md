<div align="center">

# From Zero to AI Agents

**One semester, four builds: from training a game-playing agent with reinforcement learning to deploying a multi-model AI pipeline in production.**

Tomas Diez Canedo · *Foundations of Agentic AI* (MBA 290T) · UC Berkeley Haas · Fall 2026

</div>

---

Each assignment builds on the last: first how models *learn*, then what's *inside* a language model, then how to make one *useful* with retrieval, and finally how to run several models together as a *reliable system*. Every project lives in its own repository with runnable code, saved evidence and an honest write-up of what didn't work.

## ⭐ Featured: Spotify Insight Pipeline

<a href="https://github.com/tomas10k/haas-ai-a5-spotify-insight-pipeline"><img src="https://raw.githubusercontent.com/tomas10k/haas-ai-a5-spotify-insight-pipeline/main/docs/images/dashboard.png" alt="Spotify Insight Pipeline dashboard" width="100%"></a>

**An AI pipeline that read 100,000 Spotify app reviews and recommended where the product team should focus next quarter, for $4.74 in API costs.**

- **Several models, each with one narrow job:** Claude Haiku labels the reviews, a second model checks a sample *blind*, and Claude Sonnet writes the memo. Code does all counting and ranking.
- **The AI never types a number:** the memo references facts by ID, and code fills in and verifies every figure.
- **Production habits:** it survived a deliberate mid-run kill and a real network outage without re-sending a review. A spend ledger can't overshoot its budget, and prompt-injection reviews were tested.
- **Result:** billing complaints grew from 3.8% → 13.2% of reviews year over year, so the recommendation was **billing & support**.

[**Repository**](https://github.com/tomas10k/haas-ai-a5-spotify-insight-pipeline) · [**Live dashboard**](https://haas-ai-a5-spotify-insight-pipeline.vercel.app/) · [**3-min demo**](https://drive.google.com/file/d/1pMleGuY7WNcjZznoMf1LIN9h-5viLjO8/view?usp=sharing)

## All projects

| # | Project | What I built | Headline result | Stack |
|:-:|---|---|---|---|
| 2 | [**Ms. Pac-Man DQN**](https://github.com/tomas10k/haas-ai-a2-pacman-dqn) | A Deep Q-Network that learns to play Ms. Pac-Man from raw pixels | **+43%** mean score vs. untrained. A 10× replay memory lifted late-training average from 712 → 916 | PyTorch · Gymnasium · Colab GPU |
| 3 | [**Custom LLM from scratch**](https://github.com/tomas10k/haas-ai-a3-custom-llm) | A nanoGPT language model trained from random weights, plus my own teaching corpus | Eval score **20 → 27 / 48** after adding a targeted negation & grammar corpus | PyTorch · nanoGPT |
| 4 | [**Personal Wiki CLI**](https://github.com/tomas10k/haas-ai-a4-knowledge-hub) | A fully offline RAG assistant over my own course notes, running Gemma locally on a Mac | **~1 s** answers with checked citations; declines when the evidence isn't there | Gemma 4 · MLX · BM25 · Python CLI |
| 5 | [**Spotify Insight Pipeline**](https://github.com/tomas10k/haas-ai-a5-spotify-insight-pipeline) | A staged multi-model pipeline + SQL backend + live dashboard over 660k reviews | **100k reviews for $4.74**, 88% agreement with human labels | Claude · TypeSafe Jev · SQLite · Vercel |

<sub>Assignment 1 (a networking tracker web app) was not published.</sub>

## A closer look

### 🕹️ A2: Teaching an agent to play Ms. Pac-Man
<img align="right" src="https://raw.githubusercontent.com/tomas10k/haas-ai-a2-pacman-dqn/main/results/final_best.gif" width="160" alt="Trained agent playing Ms. Pac-Man">

**Reinforcement learning.** I trained a DQN for 500 games on a T4 GPU and ran a controlled second experiment: same games and exploration, but **10× the replay memory**. Late-training scores rose from 712 to 916, and games above 2,000 points went from 3 to 15. The write-up also explains why a 5-game evaluation can't separate the two runs, and why rising loss didn't mean worse play.

**Skills:** experiment design · hyperparameter reasoning · reading noisy results honestly

→ [Repository](https://github.com/tomas10k/haas-ai-a2-pacman-dqn)

<br clear="right">

### 🧠 A3: Training a language model from scratch
**Deep learning and transformers.** I trained a ~112k-parameter nanoGPT from random weights and traced the training mechanics: one word's embedding before and after, the very first gradient update, attention weights, and next-word probabilities going from flat to confident. I then wrote a targeted teaching corpus (with a leakage check against the test set), which took the eval from **20/48 to 27/48**. Predictions were written down *before* each run and compared against the results.

**Skills:** tokenization & embeddings · training dynamics · data-centric improvement · eval hygiene

→ [Repository](https://github.com/tomas10k/haas-ai-a3-custom-llm)

### 📚 A4: A private, offline AI assistant for my notes
**LLM behavior, prompting and retrieval.** `wiki` is a command-line tool that answers questions about my course notes using **Gemma 4 running locally** on an M5 MacBook, with Wi-Fi off. It retrieves passages with BM25 and cites them as `[S1]…[S5]`, and code verifies every citation. It says "insufficient evidence" instead of guessing. I wrote the tests before building and iterated the prompts through three versions. The write-up includes a failure diagnosis: a paraphrased question that keyword search missed, and the hybrid-retrieval fix it points to.

**Skills:** RAG · local model deployment · prompt iteration · grounding & citation checks

→ [Repository](https://github.com/tomas10k/haas-ai-a4-knowledge-hub)

### 📊 A5: Production LLM pipeline for product analytics
**Production LLMs and multi-agent systems.** See [Featured](#-featured-spotify-insight-pipeline) above. Six stages (prepare → classify → verify → group → rank → recommend), each reading only the previous stage's saved output. Evaluation covers a hand-labeled golden set, a blind verifier and fault-injection system tests. There's a cost model that predicted spend before the run, and a deployed dashboard where you can trace any review end to end.

**Skills:** multi-agent orchestration · LLM evaluation · cost & reliability engineering · data pipelines · full-stack deployment

→ [Repository](https://github.com/tomas10k/haas-ai-a5-spotify-insight-pipeline)

## What ties them together

| | |
|---|---|
| 🧪 **Measure before claiming** | Every project has a fixed evaluation set up before the results came in: seeds, test questions, or human labels |
| 🔁 **Reproducible** | Pinned dependencies, saved run outputs, and commands that replay results without paid API calls |
| 🧾 **Show the evidence** | Screenshots, transcripts, logs and per-case results are committed next to the code |
| ⚠️ **Honest about limits** | Each write-up names what failed and the next experiment I'd run |
| 🔐 **No secrets in git** | Keys stay in `.env`; only blank `.env.example` files are committed |

---

<div align="center">
<sub>Course site: <a href="https://haas-ai-classes-fall-26.vercel.app/">haas-ai-classes-fall-26.vercel.app</a></sub>
</div>
