# 🖼️ BLIP from Scratch (Vision–Language Pretraining)

This project implements **BLIP (Bootstrapping Language–Image Pretraining)** **from scratch** using **PyTorch**, including a custom **Vision Transformer (ViT)** image encoder, a **Transformer-based text encoder–decoder**, and **vision–language contrastive training** on the COCO Captions dataset.

Unlike most BLIP implementations that rely on pretrained checkpoints, **this project is trained end-to-end from random initialization**, with a focus on architectural understanding, training dynamics, and evaluation behavior.

---

## 🔍 Project Objectives

- Implement the **BLIP architecture from first principles**
- Train a **multimodal vision–language model** without pretrained weights
- Study **image–text contrastive alignment + language modeling**
- Analyze **generation failure modes** in low-resource training
- Perform **qualitative and quantitative evaluation**

---

## 🧠 Model Architecture

### Vision Encoder
- Custom **Vision Transformer (ViT)**
- Patch embedding using Conv2D
- Multi-Head Self-Attention blocks
- CLS token pooling
- Output dimension: **512**

### Text Encoder–Decoder
- Transformer encoder for text embeddings
- Transformer decoder with cross-attention
- Tokenization via **bert-base-uncased**
- Vocabulary size: **30,522**

### Multimodal Projection
- Linear projection layers for image & text features
- L2 normalization
- Temperature-scaled similarity

---

## 🧮 Training Objectives

The model is trained using a joint loss:

### 1. Image–Text Contrastive Loss (ITC)
Aligns image and text representations in a shared embedding space.

### 2. Language Modeling Loss (LM)
Trains the decoder to predict caption tokens autoregressively.

\[
\mathcal{L} = \mathcal{L}_{ITC} + \mathcal{L}_{LM}
\]

---

## 📊 Dataset

- **COCO 2017 Captions**
- Subset: **First 10,000 images**
- Each image paired with natural-language captions
- Images resized to **224 × 224**

> A reduced dataset is intentionally used to analyze training dynamics under limited data.

---

## 🏋️ Training Details

| Parameter | Value |
|---------|------|
| Optimizer | AdamW |
| Learning Rate | 5e-5 |
| Batch Size | 8 |
| Epochs | 3 |
| Image Size | 224 × 224 |
| Max Caption Length | 40 |
| Device | GPU (T4) |

Training loss decreases consistently across epochs, indicating stable optimization.

---

## 🔎 Evaluation

### Qualitative Evaluation
Random samples from the dataset are visualized along with:
- Generated captions
- Ground-truth captions

### Quantitative Metrics
- **BLEU-1 to BLEU-4**
- **CIDEr**

---

## ⚠️ Important Observation: Decoder Mode Collapse

When trained **from scratch on a limited dataset**, the model exhibits **token repetition** during greedy decoding (e.g., repeating a frequent noun such as *"carriages"*).

This is a **known failure mode** in low-resource autoregressive sequence generation.

### Why this happens:
- No large-scale language pretraining
- No Image–Text Matching (ITM) loss
- Limited caption diversity
- Greedy decoding without beam search

### Result:
- BLEU and CIDEr scores approach **zero**
- Generated captions lack linguistic diversity

> This behavior is **expected** and highlights why large-scale pretraining is critical in BLIP-style models.

---

## 📌 Research Insight

This implementation demonstrates that:
- Vision–Language alignment can be learned from scratch
- Contrastive objectives alone are insufficient for fluent caption generation
- Decoder pretraining and ITM loss are crucial for preventing mode collapse

These observations align with findings from the original BLIP paper.

---

## 🧪 Limitations

- Trained on a small subset of COCO
- No pretrained vision or language weights
- Greedy decoding only
- No beam search or repetition penalty
- CIDEr requires corpus-level evaluation

---

## 🚀 Future Improvements

- Large-scale pretraining on image–text pairs
- Image–Text Matching (ITM) loss
- Beam search decoding
- Repetition penalty during generation
- Full COCO evaluation split

---

## 📚 References

- **BLIP: Bootstrapping Language–Image Pre-training**  
  Li et al., 2022  
- **Vision Transformer (ViT)**  
  Dosovitskiy et al., 2020  
- **COCO Captions Dataset**

---

## 🧑‍💻 Author

**Agniva Ghosh**  
Focus: Multimodal Learning · Vision–Language Models · Research-Oriented Implementations

---

### ⭐ Why this project matters

This project emphasizes **understanding over shortcuts** by implementing, training, and diagnosing a modern multimodal architecture without relying on pretrained models.
