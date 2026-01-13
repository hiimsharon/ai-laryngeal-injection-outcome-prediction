# ai-laryngeal-injection-outcome-prediction

## 應用人工智慧方法預測注射式喉成型術後狀況

## Project Scope and Disclaimer｜專案範圍與聲明


**中文**

本專案為課程期末專案，目的在於展示人工智慧模型於語音分析之方法設計與實作流程，  
**並非正式之臨床研究或經同儕審查之學術發表成果**。

本專案僅作為學術展示與技術驗證用途，  
**不作為任何臨床決策或醫療建議之依據**。


**English**

This repository is developed as a **course final project** for academic demonstration purposes.
It is **not an official clinical study**, nor a peer-reviewed research publication.

The implementation focuses on methodological exploration and model design,
and is **not intended for clinical deployment or medical decision-making**.

---

## 1. Project Summary (English)

This project presents a **dual-input shared-backbone convolutional neural network architecture**,
employing **EfficientNetB0** as the primary feature extraction backbone.
The model jointly integrates **Mel spectrograms** and
**Short-Time Fourier Transform (STFT) spectrograms**
to perform **segment-level voice classification**.

For model training and evaluation, we adopt **stratified five-fold cross-validation**,
with **AUROC serving as the primary model selection criterion**,
combined with **best-threshold selection** for classification decision making.
This evaluation framework is designed to reduce bias caused by data imbalance
and threshold dependency, thereby enhancing the **generalization performance
and reliability of the reported results**.

Additional methodological details and implementation considerations are available at:  
[Supplementary link]((https://www.notion.so/114-1-2e436e7f278c801995cfd6439d14df89#2e436e7f278c8035ae0ed62f7f2be55c))


## 2. 專案摘要（中文）
### Project Summary (Chinese)

本專案提出一套**雙輸入共享骨幹（Dual-input Shared-backbone）之卷積神經網路架構**，
以 **EfficientNetB0** 作為主要特徵擷取模型，
同時整合 **Mel 頻譜圖（Mel Spectrogram）**
與 **短時傅立葉轉換頻譜圖（Short-Time Fourier Transform, STFT）**，
進行**語音片段層級（segment-level）之分類任務**。

在模型訓練與評估設計上，
本研究採用**分層五折交叉驗證（Stratified Five-Fold Cross-Validation）**，
並以 **AUROC 作為模型選擇之主要指標**，
搭配**最佳分類閾值搜尋（Best-threshold Selection）**，
以降低資料分佈不均與閾值偏移對模型評估之影響，
進一步提升模型之**泛化能力與評估可信度**。

相關方法推導、實驗設定與設計考量之補充說明請參考：  
[文字補充連結](https://www.notion.so/114-1-2e436e7f278c801995cfd6439d14df89#2e436e7f278c8035ae0ed62f7f2be55c)

## 3. 專案結構  
## Repository Structure
├─ src/ # 模型實作與訓練程式碼
├─ data/ # 示範資料與資料使用說明
├─ requirements.txt
├─ README.md
└─ .gitignore

**說明（中文）：**  
`data/` 資料夾中不包含任何原始臨床資料。

**Note (English):**  
The `data/` directory does **not** contain original clinical data.


## 重現性與使用方式  
## Reproducibility and Usage

### 中文
欲重現本專案之程式流程，請先安裝所需套件：

```bash
pip install -r requirements.txt
模型訓練與評估流程請參考 src/ 資料夾內之程式碼。
本專案未提供原始臨床資料。





## 4. 資料可得性聲明
Data Availability Statement (Chinese)

基於病人隱私保護、臨床研究倫理與個人資料保護規範之考量，本研究所使用之**原始臨床資料未對外公開**。
本 GitHub 專案僅提供**去識別化之示範資料**或**特徵層級（feature-level）之資料表示**，以支援研究流程展示與方法重現。

<p align="center">
  <img src="https://raw.githubusercontent.com/andreasbm/readme/master/assets/lines/colored.png" width="70%">
</p>


<p align="center">
  <img src="https://raw.githubusercontent.com/andreasbm/readme/master/assets/lines/wave.svg" width="80%">
</p>

## 作者與課程資訊  
## Author and Course Information

Author / 作者：hiimsharon
Project Type / 專案性質：Course Final Project

<p align="center">
  <img src="https://capsule-render.vercel.app/api?type=rect&color=0:2b5876,100:4e4376&height=2" />
</p>
