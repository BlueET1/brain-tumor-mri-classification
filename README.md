# Brain Tumor MRI Classification — TResNet34_CBAM

> **學習記錄用途**：此 repo 為大學機器學習課程期末專題成果，記錄學習歷程與實驗結果，供個人學習參考使用。

## 專案簡介

本專題提出 **TResNet34_CBAM** 架構，結合預訓練 ResNet34 與 CBAM 雙重注意力機制（Channel Attention + Spatial Attention），用於腦腫瘤 MRI 影像的四類分類任務。

- **課程**：機器學習（Machine Learning）期末專題
- **組別**：第 7 組（5人）
- **學校**：國立中央大學 資訊電機學院學士班

---

## 模型架構

```
Input (224×224 grayscale MRI)
  → G2B Conversion (Conv2d 1→3)
  → Pre-trained ResNet34 backbone (ImageNet weights)
  → CBAM module at Layer 4
      ├── Channel Attention (MLP squeeze-and-excitation)
      └── Spatial Attention (7×7 conv)
  → FC classifier (4 classes)
```

**四類腦腫瘤**：Glioma / Meningioma / Pituitary tumor / No tumor

---

## 實驗結果

| Model | Accuracy | Precision | Recall | F1-score |
|-------|----------|-----------|--------|----------|
| Baseline ResNet34 | 91.38% | - | - | - |
| EfficientNet-B0 | 84.81% | - | - | - |
| EfficientNet-B1 | 84.00% | - | - | - |
| **TResNet34_CBAM (ours)** | **98.06%** | **0.98** | **0.98** | **0.98** |

### Ablation Study（注意力機制）

| Attention | Accuracy |
|-----------|----------|
| None (Transfer Learning only) | 96.92% |
| Channel Attention only | 97.31% |
| Spatial Attention only | 97.88% |
| CBAM (Channel + Spatial) | **98.06%** |

---

## 資料集

- **來源**：Figshare + SARTAJ + Br35H 三資料集合併
- **總數**：7,023 張 MRI 影像，4 類別
- **前處理**：資料增強（水平/垂直翻轉、random crop）平衡各類至 1,600 訓練樣本，統一縮放至 224×224，正規化至 [-0.5, 0.5]

---

## 訓練設定

- **環境**：Google Colab
- **Optimizer**：Adam（lr = 5e-5）
- **Loss**：Cross-Entropy
- **Batch size**：8
- **Epochs**：5
- **驗證**：5-fold cross-validation，80:20 stratified split

---

## 檔案說明

| 檔案 | 說明 |
|------|------|
| `Final Project Paper.pdf` | 完整論文報告 |
| `111504006_陳至恆.pdf` | 個人貢獻說明 |
| `Final Project Group 7/Final Proposal Presentation.pdf` | 期末提案簡報 |

---

## 參考文獻

1. He et al., "Deep Residual Learning for Image Recognition," CVPR 2016
2. Yosinski et al., "How transferable are features in deep neural networks?", NeurIPS 2014
3. Woo et al., "CBAM: Convolutional Block Attention Module," ECCV 2018
4. Nickparvar, "Brain Tumor MRI Dataset," Kaggle 2021
