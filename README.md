# ai-laryngeal-injection-outcome-prediction
應用人工智慧方法預測注射式喉成型術後狀況
本專案實作一套雙輸入共享骨幹 EfficientNetB0 卷積神經網路，同時整合 Mel 與 STFT 頻譜圖進行語音片段層級分類，並採用分層五折交叉驗證、AUROC 導向模型選擇與最佳閾值搜尋，提升模型泛化能力與評估可信度
