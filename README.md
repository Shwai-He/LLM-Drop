<div align="center">

# Uncovering the Redundancy in Transformers via a Unified Study of Layer Dropping

[![TMLR](https://img.shields.io/badge/TMLR-2026-0B7285?style=for-the-badge&logo=openaccess&logoColor=white)](https://openreview.net/forum?id=1I7PCbOPfe)
[![OpenReview](https://img.shields.io/badge/Paper-OpenReview-8A2BE2?style=for-the-badge&logo=openreview&logoColor=white)](https://openreview.net/forum?id=1I7PCbOPfe)
[![arXiv](https://img.shields.io/badge/arXiv-2406.15786-b31b1b?style=for-the-badge&logo=arxiv&logoColor=white)](https://arxiv.org/abs/2406.15786)
[![Hugging Face](https://img.shields.io/badge/🤗%20Models-Hugging%20Face-FFD21E?style=for-the-badge)](https://huggingface.co/collections/LLM-Drop/llm-drop-66dde616140f04eb18424a0a)
[![Project Page](https://img.shields.io/badge/🌐%20Website-Project%20Page-0d6b5d?style=for-the-badge)](https://case-lab-umd.github.io/LLM-Drop/)
[![License](https://img.shields.io/badge/License-Apache%202.0-blue?style=for-the-badge)](LICENSE)
[![Python](https://img.shields.io/badge/Python-3.10+-3776AB?style=for-the-badge&logo=python&logoColor=white)](https://www.python.org/)

<p align="center">
  <b><a href="https://shwai-he.github.io/">Shwai He*</a></b>,
  <b><a href="https://s1ghhh.github.io/">Guoheng Sun*</a></b>,
  <b><a href="https://shenzheyu.github.io/">Zheyu Shen</a></b>,
  <b><a href="https://www.ang-li.com/">Ang Li</a></b>
  <br>
  <i>CASE Lab, University of Maryland, College Park</i>
  <br>
  <sub>* Equal contribution</sub>
</p>

<p align="center">
  <a href="https://case-lab-umd.github.io/LLM-Drop/">🌐 <b>Project Page</b></a> •
  <a href="#-news--recognition">🏆 <b>News & Awards</b></a> •
  <a href="#-key-highlights">🌟 <b>Highlights</b></a> •
  <a href="#-methodology--taxonomy">📐 <b>Taxonomy</b></a> •
  <a href="#-model-zoo--checkpoints">🧰 <b>Model Zoo</b></a> •
  <a href="#%EF%B8%8F-installation">⚙️ <b>Installation</b></a> •
  <a href="#-quickstart--usage">🚀 <b>Quickstart</b></a> •
  <a href="#-benchmark-results">📊 <b>Benchmarks</b></a> •
  <a href="#-citation">📄 <b>Citation</b></a>
</p>

</div>

---

> [!NOTE]
> This is the official repository for the paper **[Uncovering the Redundancy in Transformers via a Unified Study of Layer Dropping](https://openreview.net/forum?id=1I7PCbOPfe)**, published in **Transactions on Machine Learning Research (TMLR 2026)** *(Early version: [What Matters in Transformers? Not All Attention Is Needed](https://arxiv.org/abs/2406.15786))*.

---

## 🏆 News & Recognition

- **[Feb 2026]** 📄 Published in **Transactions on Machine Learning Research (TMLR 2026)**!
- **[May 2025]** 🏆 **Won the Qualcomm Innovation Fellowship (QIF) North America 2025** for the proposal *"Less Attention, Much Faster: Toward a Future of Efficiency-Optimized Transformer Architectures."*
- **[Nov 2024]** 🚀 Added support for more foundation model families (**Gemma-2**, **DeepSeek**, **Yi**, **Baichuan**, **Solar**).
- **[Sep 2024]** 🤗 Released dropped-model checkpoints on [Hugging Face](https://huggingface.co/collections/LLM-Drop/llm-drop-66dde616140f04eb18424a0a).
- **[Jun 2024]** 💡 Released initial arXiv preprint and complete codebase.

---

## 🌟 Key Highlights

- ⚡ **Significant Speedup & Memory Savings**: Achieves up to **2.1× inference speedup** and over **40% KV cache memory reduction** without requiring specialized hardware kernels.
- 🧩 **Unified Dropping Taxonomy**: Systematically dissects and compares **Block Drop**, **Attention-Layer Drop**, **MLP-Layer Drop**, and **Joint Layer Drop** under a standardized framework.
- 🎯 **High Performance Retention**: Retains **>95–98%** of core reasoning and general language capabilities (MMLU, GSM8K, ARC-c, HellaSwag) through importance-aware layer selection.
- 🗜️ **Orthogonal Quantization Synergy**: Easily pairs with post-training 4-bit quantization (**AWQ** / **GPTQ**) for compounding latency and memory benefits.
- 🔌 **Plug-and-Play Hugging Face Integration**: Output models use standard `auto_map` configurations for seamless loading via `AutoModelForCausalLM`.

---

## 📖 Overview

Standard Transformer architectures treat every layer and sublayer identically throughout the network depth. However, deep representations exhibit profound **asymmetric redundancy**:
1. **Attention Redundancy vs. MLP Redundancy**: In deeper layers, attention mechanisms often collapse into static routing patterns, whereas MLPs continue to perform knowledge retrieval and feature transformation.
2. **Sublayer Granularity**: Dropping full blocks can cause catastrophic representational collapse; in contrast, selectively dropping attention or MLP sublayers provides fine-grained Pareto-optimal compression frontiers.

<p align="center">
  <img src="Layer_Drop.svg" alt="LLM-Drop Unified Framework" width="95%">
  <br>
  <em>Figure: Overview of LLM-Drop framework showing Block Drop, Sublayer Drop (Attention / MLP), Joint Dropping, and Quantization.</em>
</p>

---

## 📐 Methodology & Taxonomy

| Strategy | Dropped Components | Target Redundancy | Memory / KV Cache Saving | Latency Speedup | Recommended Use Case |
| :--- | :--- | :--- | :---: | :---: | :--- |
| **Block Drop** | Full Transformer Block (MHA + MLP) | Inter-block similarity | 🟢 High (Weights + KV) | 🚀 High | High-throughput batch serving |
| **Attention Drop** | Self-Attention / MHA Layers | Redundant query-key routing | ⚡ **40%+ KV Cache** | ⚡ High (Prefill & Decode) | Long-context & memory-bound generation |
| **MLP Drop** | Feed-Forward (FFN/MLP) Layers | Parameter/computation bloat | 🟢 High (Weight footprint) | 🚀 High (Compute-heavy) | Compute-bound environments |
| **Joint Layer Drop**| Hybrid Attention + MLP schedule | Compound depth redundancy | 🔥 Maximum flexibility | ⚡ Best Pareto curve | Custom hardware budget constraints |
| **Drop + Quant** | Dropped model + 4-bit AWQ/GPTQ | Intra- & Inter-layer redundancy | 💎 Ultra-compact | 🔥 Maximum efficiency | Edge & on-device deployment |

---

## 🧰 Model Zoo & Checkpoints

Pre-dropped model checkpoints are available in our [Hugging Face Collection](https://huggingface.co/collections/LLM-Drop/llm-drop-66dde616140f04eb18424a0a):

| Model Base | Dropping Configuration | Hugging Face Checkpoint | Base Size | Dropped Size |
| :--- | :--- | :--- | :---: | :---: |
| **Mistral-7B-v0.1** | Attention-Drop (4 Attn dropped) | [LLM-Drop/Mistral-7B-drop-attn4](https://huggingface.co/collections/LLM-Drop/llm-drop-66dde616140f04eb18424a0a) | 7.2B | ~6.5B |
| **Mistral-7B-v0.1** | MLP-Drop (4 MLP dropped) | [LLM-Drop/Mistral-7B-drop-mlp4](https://huggingface.co/collections/LLM-Drop/llm-drop-66dde616140f04eb18424a0a) | 7.2B | ~5.8B |
| **Mistral-7B-v0.1** | Block-Drop (4 Blocks dropped) | [LLM-Drop/Mistral-7B-drop-block4](https://huggingface.co/collections/LLM-Drop/llm-drop-66dde616140f04eb18424a0a) | 7.2B | ~5.1B |
| **Llama-2-7B** | Joint-Drop (6 Attn + 2 MLP) | [LLM-Drop/Llama-2-7B-joint-drop](https://huggingface.co/collections/LLM-Drop/llm-drop-66dde616140f04eb18424a0a) | 6.7B | ~5.3B |
| **Llama-3-8B** | Attention-Drop (4 Attn dropped) | [LLM-Drop/Llama-3-8B-drop-attn4](https://huggingface.co/collections/LLM-Drop/llm-drop-66dde616140f04eb18424a0a) | 8.0B | ~7.2B |
| **Gemma-2-9B** | Attention-Drop (6 Attn dropped) | [LLM-Drop/Gemma-2-9B-drop-attn6](https://huggingface.co/collections/LLM-Drop/llm-drop-66dde616140f04eb18424a0a) | 9.2B | ~8.1B |

```python
from transformers import AutoModelForCausalLM, AutoTokenizer

# Load directly from Hugging Face with trust_remote_code
model_id = "LLM-Drop/Mistral-7B-drop-attn4"
tokenizer = AutoTokenizer.from_pretrained(model_id, trust_remote_code=True)
model = AutoModelForCausalLM.from_pretrained(model_id, trust_remote_code=True, device_map="auto")
```

---

## ⚙️ Installation

```bash
# 1. Create and activate a clean conda environment
conda create -n llm-drop python=3.10 -y
conda activate llm-drop

# 2. Clone the repository
git clone https://github.com/CASE-Lab-UMD/LLM-Drop.git
cd LLM-Drop

# 3. Install core dependencies and LLM-Drop package
pip install -e .
pip install flash-attn --no-build-isolation

# 4. Optional: Install Quantization dependencies (AutoAWQ & AutoGPTQ)
cd src/llmtuner/compression/quantization/AutoAWQ
pip install -e .
cd AutoAWQ_kernels && pip install -e . && cd ..

cd ../AutoGPTQ
pip install -vvv --no-build-isolation -e .
cd ../../../../..
```

---

## 🚀 Quickstart & Usage

### 1️⃣ Model Configuration Setup
To load dropped models with standard Hugging Face `AutoModelForCausalLM`, add the `auto_map` and drop lists to `config.json`:

```json
{
  "drop_mlp_list": [],
  "drop_attn_list": [25, 26, 24, 22],
  "auto_map": {
    "AutoConfig": "configuration_dropped_mistral.MistralConfig",
    "AutoModelForCausalLM": "modeling_dropped_mistral.MistralForCausalLM"
  }
}
```

*Drop list formats:*
- **Drop Attention Layers**: `"drop_mlp_list": [], "drop_attn_list": [25, 26, 24, 22]`
- **Drop MLP Layers**: `"drop_mlp_list": [26, 27, 25, 24], "drop_attn_list": []`
- **Drop Full Blocks**: `"drop_mlp_list": [26, 25, 24, 27], "drop_attn_list": [26, 25, 24, 27]`

### 2️⃣ Run Dropping Pipelines

```bash
# Block Dropping
bash scripts/dropping/block_drop.sh

# Sublayer Dropping (Attention or MLP)
bash scripts/dropping/layer_drop.sh

# Joint Layer Dropping
bash scripts/dropping/layer_drop_joint.sh

# Iterative Dropping
bash scripts/dropping/layer_drop_iterative.sh
```

### 3️⃣ Benchmark Task Performance

Evaluate dropped checkpoints on standard NLP and reasoning benchmarks with [EleutherAI/lm-evaluation-harness](https://github.com/EleutherAI/lm-evaluation-harness):

```bash
bash scripts/benchmark/benchmark_lm_eval.sh
```

### 4️⃣ Measure Speed & KV Cache Savings

```bash
bash scripts/benchmark/benchmark_speed.sh
```

### 5️⃣ Post-Training Quantization (AWQ / GPTQ)

```bash
# 4-bit AWQ Quantization on Dropped Model
bash scripts/quantization/awq.sh

# 4-bit GPTQ Quantization on Dropped Model
bash scripts/quantization/gptq.sh
```

---

## 📊 Benchmark Results

### Mistral-7B-v0.1 Dropping Performance

| Model Variant | Strategy | # Dropped | MMLU (5-shot) | GSM8K (8-shot) | ARC-c (25-shot) | HellaSwag (10-shot) | Relative Speedup | KV Cache Saving |
| :--- | :---: | :---: | :---: | :---: | :---: | :---: | :---: | :---: |
| **Dense Base** | — | 0 | **64.2%** | **37.8%** | **60.1%** | **83.3%** | 1.00× | 0% |
| **LLM-Drop (Attn)** | Attention Drop | 4 | **63.8%** | **37.1%** | **59.6%** | **82.9%** | **1.22×** | **-12.5%** |
| **LLM-Drop (Attn)** | Attention Drop | 8 | **62.5%** | **35.4%** | **58.2%** | **81.7%** | **1.45×** | **-25.0%** |
| **LLM-Drop (MLP)** | MLP Drop | 4 | **63.1%** | **36.2%** | **58.9%** | **82.4%** | **1.28×** | 0% |
| **LLM-Drop (Block)** | Block Drop | 4 | **62.7%** | **35.0%** | **58.4%** | **81.9%** | **1.32×** | **-12.5%** |
| **LLM-Drop + AWQ-4b**| Attn Drop + AWQ | 4 Attn | **63.2%** | **36.5%** | **59.0%** | **82.1%** | **2.14×** | **-12.5%** |

---

## 📦 Repository Layout

```
LLM-Drop/
├── docs/                   # GitHub Pages project website
│   ├── index.html          # Interactive project homepage
│   └── static/images/      # Figures and SVG assets
├── scripts/
│   ├── dropping/           # Block, layer, joint & iterative dropping scripts
│   ├── benchmark/          # LM-Eval & inference speed benchmarks
│   └── quantization/       # AWQ and GPTQ quantization scripts
├── src/
│   ├── compress.py         # Main entry point for importance estimation & dropping
│   ├── benchmark_speed.py  # Inference latency & throughput measurement
│   └── llmtuner/           # Core model definitions, dropping modules & pruning
├── Layer_Drop.svg          # Architectural overview diagram
├── setup.py                # Package setup script
└── requirements.txt        # Base dependencies
```

---

## 📄 Citation

If you find this work, repository, or released checkpoints helpful in your research, please cite our papers:

```bibtex
@article{he2026uncovering,
  title={Uncovering the Redundancy in Transformers via a Unified Study of Layer Dropping},
  author={He, Shwai and Sun, Guoheng and Shen, Zheyu and Li, Ang},
  journal={Transactions on Machine Learning Research},
  issn={2835-8856},
  year={2026},
  url={https://openreview.net/forum?id=1I7PCbOPfe}
}

@article{he2024what,
  title={What Matters in Transformers? Not All Attention Is Needed},
  author={He, Shwai and Sun, Guoheng and Shen, Zheyu and Li, Ang},
  journal={arXiv preprint arXiv:2406.15786},
  year={2024}
}
```

---

## 📬 Contact & Support

For questions, collaborations, or issues:
- **Shwai He**: [`shwaihe@umd.edu`](mailto:shwaihe@umd.edu) • [Homepage](https://shwai-he.github.io/)
- **Guoheng Sun**: [`ghsun@umd.edu`](mailto:ghsun@umd.edu) • [Homepage](https://s1ghhh.github.io/)
- **CASE Lab @ UMD**: [https://github.com/CASE-Lab-UMD](https://github.com/CASE-Lab-UMD)
