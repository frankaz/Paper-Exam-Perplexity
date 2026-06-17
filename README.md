# Exam Perplexity

**A framework for detecting if exams questions harm students who don't share the examiner's cognitive framing**

---

## Scores

| Score                     | Description                                                                                                              |
| ------------------------- | ------------------------------------------------------------------------------------------------------------------------ |
| **Baseline Perplexity**   | How unusual the question appears to a general-purpose model.                                                             |
| **Contextual Perplexity** | The same question scored using the course materials as context.                                                          |
| **Idiosyncrasy Gap**      | Difference between baseline and contextual perplexity. Larger values indicate less established course-specific language. |

---

## Quickstart

```bash
git clone https://github.com/frankaz/Paper-Exam-Perplexity.git
cd Paper-Exam-Perplexity
pip install -r requirements.txt
cp env.example .env  # add API keys if using cloud backends
```

**Option A — Jupyter notebook:**
```bash
jupyter notebook notebooks/analysis.ipynb
```

**Option B — CLI:**
```bash
python main.py                # interactive mode
python main.py --reindex      # force re-ingest of /data
```

---

## Adding Course Materials

Drop any supported files into the `/data` folder:

```
data/
  lecture_01_transcript.vtt
  lecture_02_transcript.srt
  reading_week3.pdf
  syllabus.docx
  past_quiz_01.txt
```

Supported formats: `.pdf` `.txt` `.md` `.docx` `.vtt` `.srt`

> **Privacy:** `/data` is gitignored. Your materials never leave your machine (unless you choose a cloud API backend). The vector store is written to `/chroma_db/`, also gitignored.



---

## Configuration

Edit `config.yaml` to select your backend and model:

```yaml
backend: huggingface    # huggingface | local_server | openai | anthropic

huggingface:
  model: gpt2

local_server:
  endpoint: http://localhost:11434/v1   # Ollama default
  model: llama3
  api_key: dummy_key
```

---


## Privacy Design

- `.env` is gitignored - API keys are never committed
- `/data/` is gitignored - you cannot accidentally commit course materials
- All ingested materials stay on your disk in `chroma_db/` (gitignored)


---

## Report Analysis

Scoring and metrics generated from the report.

```text
-------------------------------------
EXAM QUESTION ANALYSIS
-------------------------------------
Question: "What is earths equatorial circumference in mm?"

BASELINE PERPLEXITY:
  General Perplexity (no context):     25.6
  Lexical Rarity Score:                0.31  (31% rare)
  Syntactic Complexity Index:          4.2   (moderate-high)

COURSE ALIGNMENT:  (requires ./data folder)
  Course-Context Perplexity:           32.2
  Idiosyncratic Gap (delta):           99.9  ⚠ HIGH
  Vocabulary Coverage Gap:             3 words not in materials

BIAS FLAGS
  Assumed Knowledge Phrases:           "equatorial circumference"
  ESL Disadvantage Signal:             moderate

ANSWER OPTIONS:  (if provided)
  Perplexity across answers:    A:low  B:low  C:HIGH  D:low
  Option C is ...
-------------------------------------
```

---

## License

MIT
