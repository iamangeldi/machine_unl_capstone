# Machine Unlearning – LoRA Reconstruction Attack

This project explores whether a LoRA adapter can be reconstructed after being fused into a base diffusion model.

## Main Idea

If a LoRA is fused into a model, can an attacker recover it by computing weight differences and applying Singular Value Decomposition (SVD)?

---

# What We Did

## 1. Train a LoRA Adapter

Notebook: `Copy_of_SDXL_DreamBooth_LoRA_.ipynb`

- Used SDXL as the base model  
- Fine-tuned a LoRA adapter on a custom dataset  
- Uploaded the trained LoRA to HuggingFace  

This notebook handles:

- LoRA training  
- Saving adapter weights  
- Uploading to HuggingFace  

---

## 2. Fuse and Reconstruct (Attack)

Notebook: `train.ipynb`

This notebook performs the core experiment:

- Load SDXL base model  
- Load trained LoRA adapter  
- Fuse LoRA into the base model  
- Save fused model weights  

Then compute the weight difference:

ΔW = W_fused − W_base

Apply Singular Value Decomposition:

ΔW = U Σ Vᵀ

- Approximate low-rank matrices  
- Reconstruct a recovered LoRA adapter  

The goal is to demonstrate that LoRA fusion does not necessarily prevent recovery.

---

# Repository Files

train.ipynb  
Copy_of_SDXL_DreamBooth_LoRA_.ipynb  
README.md  

- Copy_of_SDXL_DreamBooth_LoRA_.ipynb → Training + upload  
- train.ipynb → Fusion + SVD reconstruction attack  

---

# Important Note About Notebook Preview

These notebooks:

- Generate images dynamically  
- Require GPU runtime  
- Load large diffusion models  
- Save intermediate weights  

Because of this:

GitHub preview may not render them correctly.  
Notebook outputs may appear missing in browser view.  

However, everything works when the repository is cloned and run locally or in Google Colab.

---

# How to Run

git clone <your-repo-url>  
cd <repo-name>  

Then open the notebooks locally or upload them to Google Colab.
