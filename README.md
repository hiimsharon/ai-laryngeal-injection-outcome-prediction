<p align="center">
  <img src="https://capsule-render.vercel.app/api?type=waving&color=0:2b5876,100:4e4376&height=200&section=header&text=應用人工智慧方法預測注射式喉成型術後狀況&fontSize=32&fontColor=ffffff&animation=fadeIn" />
</p>

<p align="center">
  <b>Course Final Project · Medical AI · Voice Analysis</b>
</p>

---

## 專案範圍與免責聲明

本專案為臨床合作與課程期末專案，目的在於展示人工智慧模型於語音分析之方法設計與實作流程[cite: 1, 45]，**並非正式之臨床研究或經同儕審查之學術發表成果**。本專案僅作為學術展示與技術驗證用途，**不作為任何臨床決策或醫療建議之依據**。

---

## 1. 專案名稱與一句話核心價值 (Title & Abstract)

* **專案名稱**：應用人工智慧方法預測注射式喉成型術後狀況：聲學特徵工程與深度學習時頻圖建模分析 (AI-Driven Outcome Prediction for Injection Laryngoplasty: Acoustic Feature Engineering and Deep Spectrogram Modeling)
* **一句話核心價值**：整合臨床音訊資料，運用雙輸入 EfficientNet 深度時頻模型與機器學習貝氏最佳化分類器，突破傳統肉眼內視鏡限制，以非侵入式語音聲學指標精準預測聲帶手術預後與決策[cite: 3, 5, 50]。

---

## 2. 系統架構與資料流向 (System Architecture & Workflow)

本專案之資料前處理、雙輸入深度學習建模與可解釋性分析流程如下：

```mermaid
graph TD
    A[高雄醫學大學附設中和紀念醫院<br>回顧性臨床與語音資料集 (2021-2024)] --> B[音檔能量門檻與滑動視窗切割<br>保留0.3秒片段 / 重疊率44.4%]
    B --> C1[Mel 頻譜圖轉灰階 3 通道]
    B --> C2[STFT 頻譜圖轉灰階 3 通道]
    B --> C3[萃取 27 維聲學特徵向量<br>時域/頻域/MFCC/Hjorth/功率比]
    
    C1 --> D1[雙輸入共用骨幹 EfficientNetB0]
    C2 --> D1
    D1 --> E1[特徵融合與全連接分類頭]
    E1 --> F1[五折交叉驗證與最佳閾值搜尋]
    F1 --> G1[Grad-CAM 與模態差異 ΔCAM 分析]

    C3 --> D2[貝氏最佳化超參數搜尋]
    D2 --> E2[機器學習分類器<br>RF / XGBoost / AdaBoost / LR]
    E2 --> F2[SHAP Values 特徵重要性與交互作用解析]
