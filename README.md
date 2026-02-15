# MusicGen Fine-Tuning: Taylor's Version (1989 Era)

This repository contains the configuration and scripts to fine-tune **Meta AI's MusicGen (Small)** model to recreate the iconic 1989-era synth-pop aesthetic. This is still **work in progress**, so it's not yet finished.

---

## Getting Started

### 1. Execution Environment
Due to the heavy GPU requirements and specific system-level dependencies (FFmpeg, audio drivers), this project is strictly optimized for **Kaggle**. 

> [!IMPORTANT]  
> **Run this project exclusively as a Kaggle Notebook** using the **GPU T4 x2** accelerator. Attempting to run this locally or on other platforms without massive reconfiguration will likely result in environment conflicts.

### 2. The Dataset
The model is trained on a curated selection of synth-pop tracks, meticulously labeled with metadata including BPM, key, and instrumentation.

* **Dataset Link:** [Kaggle Dataset - 1989 Style Fine-Tuning](https://kaggle.com/datasets/1f39972159d83f315d74495ccaefc68211432102ce5137fc9efc83cd5c15b523)

Before starting the training, ensure you have added this dataset to your Kaggle session via the `Add Data` sidebar.

---

## Technical Specifications

| Component | Requirement / Version |
| :--- | :--- |
| **Platform** | Kaggle Notebook |
| **GPU** | 2x Tesla T4 (15GB+ VRAM total) |
| **Environment** | Micromamba (Python 3.9 isolated) |
| **AI Framework** | AudioCraft (Meta AI) |
| **Key Dependencies** | PyAV (with FFmpeg), PyTorch, Flashy |

---

## Engineering & Troubleshooting

Fine-tuning MusicGen on a restricted platform like Kaggle required several custom workarounds implemented in the provided code:

* **PyAV & `av._core` Fix:** We bypass Kaggle's default Python 3.12 environment by isolating the project in a Python 3.9 Micromamba environment and using `LD_PRELOAD` to bridge library conflicts.
* **Storage Management:** To prevent the `Input/output error` (disk full) typical of Kaggle's `/tmp` directory, we implement a **sparse checkpointing system** (saving every 5 epochs instead of every 1).
* **Warmup Override:** For small datasets, the standard 8000-step warmup prevents learning. We use a **Constant LR Schedule** (0.001) to force immediate stylistic adaptation.

---

## Usage & Inference

Once training reaches an optimal **Cross-Entropy (ce)** score (target: < 4.2), you can generate audio using the provided inference scripts.

**Recommended Prompt:**
> `"Synth-pop instrumental, 96 BPM, F major, high energy, very danceable, Blank Space style"`

---

## License
This project is for educational and research purposes. All rights to the original training audio belong to the respective copyright holders.
