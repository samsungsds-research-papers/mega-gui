# MEGA-GUI: Multi-stage Enhanced Grounding Agents for GUI Elements

**arXiv:** http://arxiv.org/abs/2511.13087

MEGA-GUI is a modular, multi-stage framework for GUI grounding. 
It decomposes grounding into coarse Region-of-Interest (ROI) selection and fine-grained element grounding, orchestrated through specialized Vision-Language Models (VLMs).
This repository provides code, dataset generators, experiment logs, and serving utilities for reproducing the results of our paper.

## Features

- Multi-stage grounding pipeline (ROI selection + fine-grained grounding)
- Bidirectional ROI zoom algorithm for robust search
- Context-aware rewrite agent for reducing semantic ambiguity
- Integration of diverse VLMs (Qwen, GPT-based, custom vLLM servers)
- Benchmark support for ScreenSpot-Pro and OSWorld-G
- Synthetic dataset generator aligned with the experimental setup
- JSON logs enabling reproducibility

---

## Experimental Results

Our MEGA-GUI framework achieves state-of-the-art performance on key GUI grounding benchmarks, demonstrating significant improvement over both single-shot and agentic baselines.

The table below summarizes results on **ScreenSpot-Pro (SSP)** and **OSWorld-G (OSG)**.  
Single-shot baselines were reproduced in our unified evaluation environment; agentic baselines are reported from their original papers.

### Performance Comparison

| Method              | SSP Accuracy (%) | OSG Accuracy (%) |
|---------------------|------------------|------------------|
| **Single-Shot Baselines** |                  |                  |
| Gemini-2.5-Pro      | 6.96             | 28.19            |
| CUA (Operator)      | 35.03            | 32.26            |
| UI-TARS-7B          | 29.53            | 44.50            |
| Qwen-VL-2.5-72B     | 41.55            | 46.80            |
| GTA1-72B            | 46.34            | 50.53            |
| Jedi-7B             | 30.10            | 51.41            |
| UI-TARS-72B         | 37.12            | 55.67            |
| GTA1-7B             | 50.03            | 56.22            |
| **Agentic Baselines** |                |                  |
| ReGUIDE-7B          | 44.40            | N/A              |
| ScreenSeekeR        | 48.10            | N/A              |
| DiMo-GUI            | 49.70            | N/A              |
| RegionFocus         | 61.60            | N/A              |
| **MEGA-GUI (Ours)** | **73.18**        | **68.63**        |

MEGA-GUI outperforms all prior baselines by a wide margin, highlighting the effectiveness of:
- Decomposing GUI grounding into multiple specialized stages  
- Leveraging complementary strengths of different VLMs  
- Reducing ambiguity through semantic rewriting  
- Resolving spatial dilution via bidirectional adaptive zoom  

For detailed ablation studies and reproducibility, refer to the paper and experiment notebooks.

---

## Project Structure

```
mega-gui/
├── data_noImage/
├── data_generator_code/
├── Bidirectional Adaptive Zoom Data/
│   ├── roi_list_result_*.json
│   └── candidate_point_list_*.json
├── main_code/
│   ├── MEGA-GUI_state_1_2.ipynb
│   └── MEGA-GUI_state_1_2_refusal.ipynb
├── vlm_serving_guide/
│   └── VLM_serving_guide_via_vllm.md
├── data_for_semantic_analysis/
├── requirements.txt
└── README.md
```

---

## Getting Started

### Prerequisites

- Python 3.11 recommended  
- Linux/WSL/macOS environment (preferred for large model serving)  
- GPU access if running open-source VLMs  
- API access for proprietary models (GPT-4o, Gemini 2.5 Pro, etc.)

### Installation

1. Clone the repository:

```
git clone https://github.com/samsungsds-research-papers/mega-gui.git
cd mega-gui
```

2. (Optional) Create a virtual environment:

```
python3 -m venv .venv
source .venv/bin/activate
```

3. Install dependencies:

```
pip install -r requirements.txt
```

---

## API & Credential Configuration

Create a file at:

```
../.credentials.json
```

Example:

```
{
  "OPENAI_API_BASE": "<YOUR_OPENAI_API_BASE>",
  "OPENAI_API_KEY": "<YOUR_OPENAI_API_KEY>",
  "OPENAI_API_TYPE": "azure",
  "OPENAI_API_VERSION": "2024-08-01-preview",
  "HTTP_PROXY": "",
  "HTTPS_PROXY": "",
  "NO_PROXY": "",
  "REQUESTS_CA_BUNDLE": "",
  "OPENAI_API_ENGINE_CUA": "",
  "VLLM_SERVER_IP": "<SERVER_IP>",
  "VLLM_SERVER_PORT": "<PORT>",
  "OPENROUTER_API_KEY": "<YOUR_OPENROUTER_API_KEY>"
}
```

---

## Datasets

MEGA-GUI uses metadata from:

- ScreenSpot-Pro  
  https://huggingface.co/datasets/likaixin/ScreenSpot-Pro  

- OSWorld-G  
  https://github.com/xlang-ai/OSWorld-G/tree/main/benchmark  

Download dataset images/metadata and place them under:

```
data_noImage/
```

Use scripts in:

```
data_generator_code/
```

to generate experiment-ready parquet files.

---

## Running Experiments

### ScreenSpot-Pro

Open:

```
main_code/MEGA-GUI_state_1_2.ipynb
```

Key parameters:

- step_ratio  
- target_roi_size  
- max_zoom_out_count  
- VLM_MODEL_NAME  

### OSWorld-G

Open:

```
main_code/MEGA-GUI_state_1_2_refusal.ipynb
```

Includes Stage 0 refusal-handling.

---

## Bidirectional Adaptive Zoom Data

Files include:

- roi_list_result_*.json  
- candidate_point_list_result_*.json  

These provide full logs for each zoom-in/out iteration.

---

## VLM Serving Guide

See:

```
vlm_serving_guide/VLM_serving_guide_via_vllm.md
```

Contains:

- vLLM installation  
- Model loading  
- API serving  
- Example requests  

---

## Deployment

MEGA-GUI can be deployed as:

- REST API service  
- Offline batch processing tool  
- Module integrated into GUI automation or RPA systems  

---

## Contributing

Guidelines:

1. Follow code style  
2. Document major changes  
3. Provide minimal tests  
4. Use GitHub issues for discussion  

---

## Versioning

```
v0.1.0
```

---

## License

MIT License (see `LICENSE`).  
Third-party license attributions are in `NOTICE`.

---
