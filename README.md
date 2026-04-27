# IEEECH-2026-Submission-Relevant-Code-and-Documents-

Repository for IEEE CyberHumanities Conference Submission 



# What This Repository Contains

This repository provides everything needed to replicate the results reported in the paper.

```
├── SE_Master_Calculator_w3wayshuffle.py   # Primary SE calculator (Fig. 1, Fig. 2, Fig. 3)
├── NLP_Method_Shuffle_Tester.py           # 7-method comparison tester (Table 1)
├── motifs/
│   ├── genesis_motifs.py                  # 19-category motif dict, Genesis 1–3 (Table 2)
│   └── lotr_motifs.py                     # 15-category motif dict, Fellowship of the Ring
├── corpora/
│   ├── genesis_original.txt               # Genesis 1–3 KJV (public domain)
│   └── DATA.md                            # Instructions for obtaining the LOTR corpus
├── figures/
│   ├── heatmap.png                        # Fig. 1 — Genesis 3-way trifold heatmap
│   ├── lotr_trifold.png                   # Fig. 2 — LOTR 3-way trifold heatmap
│   └── lotr_single.png                    # Fig. 3 — LOTR single heatmap with H/Σ overlays
├── requirements.txt
└── README.md
```
# Requirements: 

1. Core SE Calculator dependencies:
numpy
pandas
matplotlib
scipy
python-docx
 
2. 7-Method Shuffle Tester dependencies:
scikit-learn
gensim
bertopic
umap-learn
hdbscan
transformers
torch
bert-score
spacy

3. General Requirements: 
spaCy language model (install separately after pip install):
python -m spacy download en_core_web_sm
 
---

## Quick Start

### 1. Install dependencies

```bash
pip install -r requirements.txt
```

### 2. Run SE analysis (Genesis — reproduces Figure 1)

```bash
python SE_Master_Calculator_w3wayshuffle.py \
  corpora/genesis_original.txt \
  corpora/genesis_word_shuffled.txt \
  corpora/genesis_sentence_shuffled.txt
```

To generate the shuffled variants from the original:

```python
import random, re

with open("corpora/genesis_original.txt", "r") as f:
    text = f.read()

# Word shuffle
words = text.split()
random.seed(42)
random.shuffle(words)
with open("corpora/genesis_word_shuffled.txt", "w") as f:
    f.write(" ".join(words))

# Sentence shuffle (paragraph-level)
paragraphs = [p.strip() for p in text.split("\n\n") if p.strip()]
random.seed(42)
random.shuffle(paragraphs)
with open("corpora/genesis_sentence_shuffled.txt", "w") as f:
    f.write("\n\n".join(paragraphs))
```

### 3. Run SE analysis (LOTR — reproduces Figures 2 and 3)

See `corpora/DATA.md` for instructions on obtaining and preparing the LOTR corpus, then:

```bash
python SE_Master_Calculator_w3wayshuffle.py \
  corpora/lotr_original.txt \
  corpora/lotr_word_shuffled.txt \
  corpora/lotr_sentence_shuffled.txt
```

### 4. Run 7-method comparison (reproduces Table 1)

```bash
python NLP_Method_Shuffle_Tester.py corpora/genesis_original.txt
```

---

## Motif Dictionaries

Motif dictionaries are stored as Python dicts in `motifs/` and imported directly by the calculator. Each category groups semantically related words and multi-word phrases that index the same underlying narrative motif, following the method described in Section 3.2 of the paper.

**Genesis** (`genesis_motifs.py`): 19 categories, 253 words, 93 phrases  
**Fellowship of the Ring** (`lotr_motifs.py`): 15 categories

To use a different motif dictionary, import it in the calculator's configuration block:

```python
from motifs.genesis_motifs import motif_dict   # Genesis
from motifs.lotr_motifs import motif_dict      # LOTR
```

---

## Parameters Used in the Paper

| Parameter | Value |
|-----------|-------|
| Window size | Adaptive (~500 tokens for Genesis, ~1550 for LOTR) |
| Overlap | 50% |
| Target windows | 120 |
| Random seed | 42 |
| Sentence shuffling | Paragraph-level (`\n\n` splitting) |

---

## Expected Outputs

Running the calculator produces the following files (named by input text):

| File | Description |
|------|-------------|
| `<name>_se_heatmap.png` | KL divergence heatmap with H and Σ overlays |
| `<name>_3way_comparison.png` | Side-by-side trifold: original / word-shuffled / sentence-shuffled |
| `<name>_se_timeseries.png` | H and Σ line plots across windows |
| `<name>_peaks_valleys.png` | Top peaks and valleys with text excerpts |
| `<name>_se_results.csv` | Full numerical results per window |
| `<name>_validation_summary.csv` | Cohen's *d* statistics for shuffle comparison |

---


## Repository Links

- SE Calculator: [github.com/Mkurian99/Symbolic-Semantic-Entropy-Calculator-Basic](https://github.com/Mkurian99/Symbolic-Semantic-Entropy-Calculator-Basic)
- NLP Method Shuffle Tester: [github.com/Mkurian99/NLP-Method-Shuffle-Tester-](https://github.com/Mkurian99/NLP-Method-Shuffle-Tester-)

---

## Citation

```bibtex
@inproceedings{monroy2025symbolic,
  title={Dancing the Semantic Shuffle: Symbolic Entropy and Order-Sensitive Computational Semantics},
  author={Monroy, Carlos and Kurian, Michael Joseph},
  booktitle={Proceedings of the IEEE Cyberhumanities Conference},
  year={2025}
}
```

---

## License

Code: MIT License  
Genesis 1–3 (KJV): Public domain  
LOTR corpus: Not included — see `corpora/DATA.md`
