# Machine Unlearning – LoRA Reconstruction Attack

This repository explores the vulnerabilities of machine unlearning in diffusion models. Specifically, we investigate whether a Low-Rank Adaptation (LoRA) adapter can be reconstructed after being fused into a base model (Stable Diffusion XL).

## Main Idea

When a LoRA is fused into a model, the weights are combined. We demonstrate a recovery attack to extract those original LoRA weights by computing the weight differences and applying Singular Value Decomposition (SVD):

1. **Compute Weight Difference:**
   $$\Delta W = W_{\text{fused}} - W_{\text{base}}$$

2. **Apply Singular Value Decomposition (SVD):**
   $$\Delta W = U \Sigma V^T$$

By approximating low-rank matrices from this decomposition, we reconstruct the recovered LoRA adapter to show that standard LoRA fusion does not prevent extraction attacks.

---

## Prerequisites & Setup

To reproduce this project, you will need to prepare a few things before running the notebooks:

### 1. Hugging Face Account & Access Tokens
You must have a [Hugging Face](https://huggingface.co/) account and generate an Access Token with **Read and Write** permissions.
* **UI Login:** For `Copy_of_SDXL_DreamBooth_LoRA_.ipynb` and `train.ipynb`, you will be prompted to log in via a Hugging Face UI widget within the notebook.
* **Hardcoded Token:** For `metrics.ipynb`, you will need to directly hardcode your Hugging Face access token into the designated cell to load the models.

### 2. Custom Training Data
To train the initial LoRA, you will need to compile and upload your own dataset of images. You will be prompted to upload these images when running the first notebook (`Copy_of_SDXL_DreamBooth_LoRA_.ipynb`). *(Note: This repository includes a `tattoo_refs` folder as an example of the reference data used for my specific scenario).*

---

## Repository Files & Workflow

Run the notebooks in the following order to reproduce the experiment:

### 1. `Copy_of_SDXL_DreamBooth_LoRA_.ipynb` (Training)
* **What it does:** Uses SDXL as the base model to fine-tune a LoRA adapter on your uploaded custom image dataset. It also outputs initial sample generations to verify the training.
* **Action Required:** Upload your custom images and log in via the Hugging Face UI widget.

### 2. `train.ipynb` (Fusion & Extraction Attack)
* **What it does:** Loads the trained LoRA, fuses it into the main SDXL body, extracts the weight difference ($\Delta W$), and recreates a recovered LoRA.
* **Action Required:** Log in via the Hugging Face UI widget to access the necessary models.

### 3. `metrics.ipynb` (Evaluation)
* **What it does:** Opens the resulting files and conducts evaluation metrics, including CLIP embedding analysis and PCA (Principal Component Analysis) deconstruction. It also generates comparison images based on defined prompts.
* **Action Required:** You must manually input your Hugging Face token into the code. You will also need to adjust the hardcoded prompts (see below).

---

## A Note on Image Prompts

In `metrics.ipynb`, the image generation relies on positive and negative prompts. These are currently **hardcoded for my specific project scenario** and must be changed if you are training on a different subject.

* **Positive Prompts:** Text describing exactly what you *want* the model to generate (e.g., specific subjects, styles, lighting, or details).
* **Negative Prompts:** Text describing what you *do not want* the model to generate (e.g., blurry, distorted, bad anatomy, wrong colors). 

Be sure to update these variables in the `metrics.ipynb` notebook to match the custom training data you uploaded in Step 1.

---

## Important Note About Notebook Previews

Because these notebooks generate images dynamically, require GPU runtimes, load large diffusion models, and save intermediate weights:
* GitHub preview may not render them correctly.  
* Notebook outputs may appear missing in the browser view.  

Everything works as intended when the repository is cloned and run locally or in Google Colab.
