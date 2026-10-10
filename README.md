<p align="center">
  <img src="https://capsule-render.vercel.app/api?type=waving&color=0:2b5876,100:4e4376&height=200&section=header&text=應用人工智慧方法預測注射式喉成型術後狀況&fontSize=32&fontColor=ffffff&animation=fadeIn" />
</p>

<p align="center">
  <b>Course Final Project · Medical AI · Voice Analysis</b>
</p>

---

## 導覽列與章節快速跳轉

<p align="center">
  <a href="#1-專案範圍與免責聲明">專案範圍與免責聲明</a> | 
  <a href="#2-研究動機與臨床痛點">研究動機與臨床痛點</a> | 
  <a href="#3-資料收集與病人群體切分機制">資料收集與切分</a> | 
  <a href="#4-音檔切割與資料前處理工程">音檔切割與前處理</a> | 
  <a href="#5-機器學習階段聲學特徵與模型演進">機器學習與特徵工程</a> | 
  <a href="#6-深度學習階段雙輸入時頻圖建模">深度學習雙輸入模型</a> | 
  <a href="#7-模型評估與效能指標分析">模型評估與效能</a> | 
  <a href="#8-模型解釋與可解釋性分析-xai">模型解釋與 XAI</a> | 
  <a href="#9-專案資料夾結構">專案資料夾結構</a> | 
  <a href="#10-資料可得性聲明與作者資訊">資料宣告與作者</a>
</p>

---

## 1. 專案範圍與免責聲明

本專案為臨床合作與課程期末專案，目的在於展示人工智慧模型於語音分析之方法設計與實作流程[cite: 1, 45]，**並非正式之臨床研究或經同儕審查之學術發表成果**。本專案僅作為學術展示與技術驗證用途，**不作為任何臨床決策或醫療建議之依據**。

---

## 2. 研究動機與臨床痛點

* **聲帶閉合功能障礙**：閉合功能健全的聲帶才能使人類發出穩定的聲音；閉鎖功能不全的病人（如聲帶麻痺及聲帶萎縮）會造成嚴重發聲困擾，甚至導致聲音吃力與沙啞[cite: 3]。
* **臨床治療方式**：玻尿酸注射性喉成形手術（Injection Laryngoplasty）是臨床上大幅改善閉合功能不全的重要微創治療方式[cite: 3]。
* **臨床溝通痛點**：在實際醫療情境中，內視鏡檢查僅能告知病人「需要進行手術幫忙」，但患者與家屬最常詢問：「手術效果怎麼樣？我聲音會進步多少？我需要使用多少玻尿酸劑量？」過去臨床上僅能給出籠統的質性回應[cite: 4]。
* **人工智慧介入價值**：本專案旨在透過人工智慧建模，依據患者手術前的聲音檢測資料精準預測術後發聲預後狀況，藉此提升臨床資源利用率與病患溝通品質[cite: 1, 3, 5, 40]。

---

## 3. 資料收集與病人群體切分機制

* **資料來源與規模**：資料集來自高雄醫學大學附設中和紀念醫院耳鼻喉部曾馨儀主治醫師，回顧性收集 2021 年至 2024 年之臨床與語音紀錄，臨床回溯性資料庫總共有 43 位病人[cite: 6, 7]。
* **病人層級切分機制**：本專案遵循機器學習黃金比例（約 80/20），以病人層級（Patient-level）進行隨機抽樣，避免同一病人之多筆切片同時出現在訓練集與測試集而導致資料洩漏：
  * **訓練集（Train Set）**：約佔 80%，共 34 位病人[cite: 15]。
  * **測試集（Test Set）**：約佔 20%，共 9 位病人[cite: 15]。
  * *(註：在 113-2 的報告架構中，34 人 $\times$ 10 份切片 = 344 筆訓練樣本；9 人 $\times$ 10 份切片 = 86 筆測試樣本，總計 430 筆切片樣本[cite: 12, 14]。)*
* **原始資料敘述視覺化**：
  <p align="center">
    <img src="ai-laryngeal-injection-outcome-prediction/assets/原始資料敘述.png" alt="原始資料敘述" width="48%">
    <img src="ai-laryngeal-injection-outcome-prediction/assets/原始資料敘述2.png" alt="原始資料敘述2" width="48%">
  </p>

---

## 4. 音檔切割與資料前處理工程

* **音檔前處理與滑動視窗策略**：
  * 為了解決小樣本資料量不足之限制，本專案選用母音 A 之術前音檔進行深度時頻圖建模[cite: 11]。
  * 設定能量門檻過濾前後無聲雜訊，僅保留前 0.3 秒有效語音區段[cite: 11]。
  * 採用滑動視窗（Sliding Window）策略，每段視窗長度為 0.05 秒，總保留音訊長度 0.3 秒，重疊百分比為 44.4%，將每份音檔平均切成 10 等份，擴增樣本數至 $43 \times 1 \times 10 = 430$ 筆[cite: 12]。
* **時頻圖與影像前處理流程**：
  * 針對每個切片樣本分別產生 Mel 頻譜圖與短時傅立葉轉換（STFT）頻譜圖[cite: 13]。
  * 針對音檔過短容易導致整張圖變成單色圖之問題，將 Mel 與 STFT 圖像強制轉為灰階[cite: 16]。
  * 統一縮放尺寸（Resize）至 $224 \times 224$ 解析度，並將灰階影像擴展轉換為 3 通道格式（$224 \times 224 \times 3$），對齊 EfficientNet 預訓練模型之數值域[cite: 16]。
* **音檔切割與前處理視覺化**：
  <p align="center">
    <img src="ai-laryngeal-injection-outcome-prediction/assets/音檔切割.png" alt="音檔切割" width="48%">
    <img src="ai-laryngeal-injection-outcome-prediction/assets/音檔切割2.png" alt="音檔切割2" width="48%">
  </p>

---

## 5. 機器學習階段聲學特徵與模型演進

當面試官問起為什麼從機器學習跨到深度學習、模型怎麼選擇時，本專案展現出依據資料特性與複雜度進行 Trade-off 的工程考量：

### 113-2 機器學習階段：為什麼選 Logistic Regression 與樹模型？
* **Baseline（羅吉斯迴歸, LR）**：
  * **原因**：醫療與聲學特徵（如 RMS, ZCR, 27 個聲學特徵）具備高度可解釋性。LR 是最經典的 Linear Baseline，能快速看出哪些特徵具備正負向線性相關，並作為複雜模型的對照基準[cite: 61, 70]。
* **樹模型與整合學習（Random Forest, XGBoost, AdaBoost）**：
  * **原因**：聲學特徵與臨床特徵（如年齡、玻尿酸劑量）屬於表格型特徵（Tabular/Structured Data）。樹模型（尤其是整合學習 Boosting/Bagging）在處理表格資料時天生具有優勢，能捕捉特徵間的非線性互動（Non-linear interactions），且訓練速度快、配合 Bayesian Optimization 調參效率高[cite: 57, 70, 71, 72]。

---

## 6. 深度學習階段雙輸入時頻圖建模

### 114-1 深度學習階段：為什麼挑戰 EfficientNetB0 加上雙輸入（Mel + STFT）？
* **放棄手動萃取特徵，改用 CNN 看「時頻圖（Spectrogram）」**：
  * 傳統機器學習依賴手動計算 27 個聲學特徵，容易漏掉音檔中細微的頻譜動態變化。深度學習可以直接將音檔轉成 Mel 頻譜圖與 STFT 頻譜圖兩張影像，讓 Convolutional Neural Network (CNN) 自動學習底層的影像與音訊紋理特徵[cite: 13, 18]。
* **為什麼選 EfficientNetB0？**：
  * **原因**：考量到醫療數據量較小（僅 43 位病人），若從頭訓練（Train from scratch）大型 CNN 極易發生嚴重的 Overfitting（過擬合）。EfficientNetB0 具備輕量化、參數少、透過複合縮放（Compound Scaling）在準確度與運算效率（FLOPS）達到極佳平衡的特點，非常適合用來做 Transfer Learning（遷移學習）[cite: 7, 18]。
* **為什麼要做「雙輸入共享骨幹（Dual-input Shared Backbone）」？**：
  * **原因**：Mel 頻譜圖模擬人耳感知（著重低中頻與共振峰），而 STFT 頻譜圖保留了線性的時頻物理細節。我們讓兩者共用同一個 EfficientNetB0 特徵擷取器（Shared Backbone），強迫模型同時截取兩種不同時頻域的互補資訊，最後在特徵層進行 Concatenate 融合。我們後續透過 Grad-CAM 與遮罩分析也證實：模型主要依賴 Mel 頻譜作為決策主體，而 STFT 則扮演類似正規化（Regularization）的輔助角色[cite: 18, 38, 40]。
* **整體系統架構與模型架構圖**：
  <p align="center">
    <img src="ai-laryngeal-injection-outcome-prediction/assets/模型架構圖.png" alt="模型架構圖" width="90%">
  </p>
  <p align="center">
    <img src="ai-laryngeal-injection-outcome-prediction/assets/模型架構圖_醜.png" alt="模型架構圖舊版" width="60%">
  </p>

---

## 7. 模型評估與效能指標分析

* **評估指標**：採用混淆矩陣、Accuracy、Precision、Recall、F1 Score 以及 ROC 曲線與 AUROC 作為核心量化指標[cite: 22]。
* **五折交叉驗證測試集表現**：
  * **Accuracy**：$77.67\% \pm 6.47\%$[cite: 24]
  * **Precision**：$80.43\% \pm 7.44\%$[cite: 24]
  * **Recall**：$85.77\% \pm 10.51\%$[cite: 24]
  * **F1 Score**：$82.23 \pm 5.00\%$[cite: 24]
  * **AUROC**：$85.16\% \pm 5.45\%$[cite: 24]
* **模型評估結果視覺化**：
  <p align="center">
    <img src="ai-laryngeal-injection-outcome-prediction/assets/評估結果.png" alt="評估結果" width="85%">
  </p>

---

## 8. 模型解釋與可解釋性分析 (XAI)

* **Grad-CAM 與模態差異分析**：
  * 運用梯度資訊找出單一樣本在 Mel 與 STFT 影像中最敏感、最依賴的時頻區域[cite: 23]。
  * 計算 $\Delta\text{CAM} = \text{Mel} - \text{STFT}$ 診斷模態差異，發現在低到中頻區段與局部時間窗，模型對 Mel 頻譜具備較強主導性[cite: 23, 36]。
* **遮罩式特徵重要性分析（Occlusion Testing）**：
  * 透過遮蔽 Mel 與 STFT 分支以及進行頻帶與時間遮罩，證實模型決策高度依賴特定時間區段內的聲學型態[cite: 38, 39]。
* **XAI 分析架構視覺化**：
  <p align="center">
    <img src="ai-laryngeal-injection-outcome-prediction/assets/XAI分析架構.png" alt="XAI分析架構" width="48%">
    <img src="ai-laryngeal-injection-outcome-prediction/assets/XAI分析架構_EN.png" alt="XAI分析架構英文版" width="48%">
  </p>

---

## 9. 專案資料夾結構

本專案採用模組化軟體工程設計，目錄結構規劃如下：

```text
ai-laryngeal-injection-outcome-prediction/
│
├── assets/                  # 存放所有系統架構圖、音檔切割與 XAI 視覺化圖檔
├── data/                    # 示範資料與欄位說明文件（不包含任何臨床個資）
├── notebooks/               # 實驗探索、特徵工程與模型調校 Jupyter Notebooks
├── src/
│   ├── __init__.py
│   ├── preprocessing/       # 音檔能量切割、Mel/STFT 轉換與聲學特徵萃取模組
│   ├── models/              # EfficientNet 雙輸入架構與機器學習分類器訓練模組
│   ├── evaluation/          # 交叉驗證、混淆矩陣計算與閾值最佳化腳本
│   └── explainability/      # Grad-CAM 視覺化與 SHAP 特徵重要性分析模組
│
├── requirements.txt         # 專案相依套件清單
├── Dockerfile               # 容器化部署設定檔
└── README.md                # 專案首頁說明文件

## 10. 資料可得性聲明與作者資訊

### 資料可得性聲明 (Data Availability Statement)
基於病人隱私保護、臨床研究倫理與個人資料保護法規之嚴格規範，本專案所使用之**原始臨床音檔與電子病歷資料均受到嚴格保護，恕無法對外公開**。本 GitHub 專案僅提供去識別化之方法學架構、特徵工程邏輯與模型實作流程展示，藉此確保符合醫療資訊安全與去識別化規範。

### 作者與研究團隊 (Authors and Collaborators)
本專案由醫學資訊與臨床醫療團隊跨域合作完成，研發貢獻與署名資訊如下：

* **核心研究者與系統開發 (Author)**：
  * **Sha Huang (黃筱雯)** — 高雄醫學大學 醫務管理暨醫療資訊學系碩士[cite: 1]
  * **貢獻**：負責整體人工智慧模型架構設計、雙輸入深度學習與機器學習管道實作、聲學特徵工程、XAI 可解釋性分析，以及專案開源與技術文件編撰。

* **臨床指導與資料提供 (Clinical Collaborator)**：
  * **曾馨儀 主治醫師** — 高雄醫學大學附設中和紀念醫院 耳鼻喉部[cite: 1]
  * **協助**：提供回顧性臨床音訊與病歷資料、定義臨床核心痛點，並針對模型預測結果進行醫療合理性與臨床應用價值驗證。

* **學術指導與專案監督 (Advisor)**：
  * **何文獻 教授** — 高雄醫學大學 醫務管理暨醫療資訊學系[cite: 1]
  * **協助**：提供研究方向指導、語音資料標註原則方法設計建議及整體學術架構之把關。

<p align="center">
  <img src="https://raw.githubusercontent.com/andreasbm/readme/master/assets/lines/colored.png" width="70%">
</p>

