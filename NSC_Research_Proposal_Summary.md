# 國科會大專生研究計畫摘要表

---

## 計畫名稱

**基於 YOLOv8 姿態檢測之遊戲化智慧健身教練系統研究與實作**

---

## (一) 摘要

本研究旨在開發一套結合深度學習姿態檢測與遊戲化設計之智慧健身教練系統。系統採用 YOLOv8 姿態估計模型進行即時人體關鍵點偵測，透過關節角度計算與狀態機設計實現多種運動類型之自動計數與品質評估。本研究創新性地將遊戲化元素（怪物戰鬥、Combo 連擊、護盾機制）融入健身訓練流程，以提升使用者運動動機與持續性。系統架構採用 Flask + Socket.IO 實現低延遲即時通訊，前端以模組化事件驅動設計搭配 Three.js 3D 渲染，提供沉浸式訓練體驗。本研究涵蓋 11 種以上運動類型，包含健身運動（深蹲、二頭彎舉、肩推、伏地挺身、引體向上、啞鈴划船、平板支撐）及球類運動（籃球、排球、桌球）與武術訓練（跆拳道），展現系統之高度擴展性。研究成果預期可為智慧健身領域提供一套完整的技術解決方案，並為遊戲化健身之有效性評估提供實證基礎。

**關鍵詞**：深度學習、YOLOv8、姿態估計、遊戲化、智慧健身、即時運動分析、WebSocket

---

## (二) 研究動機與研究問題

### 2.1 研究動機

隨著現代生活型態改變，久坐與缺乏運動已成為全球性健康問題。世界衛生組織（WHO）指出，全球約有 27.5% 的成年人運動量不足，導致心血管疾病、糖尿病等慢性病風險增加（WHO, 2022）。傳統健身房雖提供運動場所，但存在以下限制：（1）專業教練資源有限，無法即時指導每位使用者；（2）運動過程缺乏即時回饋，使用者難以判斷動作是否正確；（3）訓練過程單調乏味，容易導致運動中斷。

近年來，深度學習技術在電腦視覺領域取得突破性進展，尤其是人體姿態估計（Human Pose Estimation）技術已達到即時應用之成熟度。YOLOv8 系列模型以其優異的速度與準確度，為建構即時運動分析系統提供了技術基礎。然而，現有研究多聚焦於單一運動類型之偵測，缺乏支援多元運動場景之統一架構；此外，如何有效利用遊戲化（Gamification）設計提升使用者運動動機，亦為值得深入探討之議題。

### 2.2 研究問題

基於上述動機，本研究擬探討以下核心問題：

1. **技術層面**：如何建構一套基於 YOLOv8 之即時姿態檢測系統，實現多種運動類型之自動識別、計數與品質評估？

2. **架構層面**：如何設計一套高效能、低延遲之系統架構，使姿態分析結果能即時回饋至使用者介面？

3. **應用層面**：遊戲化設計（怪物戰鬥、Combo 連擊、經驗值系統）對使用者運動動機與訓練持續性之影響為何？

4. **評估層面**：如何建立一套客觀的運動品質評估機制，為使用者提供精準的動作改善建議？

---

## (三) 文獻回顧與探討

### 3.1 人體姿態估計技術

人體姿態估計（Human Pose Estimation, HPE）旨在從圖像或影片中定位人體關節位置，為運動分析之基礎技術。現有方法主要分為二類：

**自上而下（Top-down）方法**：先偵測人體邊界框，再於各區域內進行關鍵點定位。代表性方法包含 AlphaPose（Fang et al., 2017）與 HRNet（Sun et al., 2019），此類方法準確度高但運算成本較大。

**自下而上（Bottom-up）方法**：先偵測所有關鍵點，再組合成各人體實例。OpenPose（Cao et al., 2017）為此類方法之代表，適用於多人場景但關鍵點分配易出錯。

**YOLO-Pose 系列**：YOLOv8-Pose（Ultralytics, 2023）採用單階段偵測架構，同時輸出邊界框與 17 個人體關鍵點，兼顧速度與準確度。在 COCO 資料集上，YOLOv8n-Pose 達到 50.4% mAP，推理速度可達 30+ FPS（GPU），符合即時應用需求。

### 3.2 運動動作識別與計數

運動動作識別研究可追溯至慣性感測器（IMU）時代，近年則轉向視覺化方法。Khurana et al.（2018）提出基於骨架序列的深度學習模型，用於識別重複性運動。Chen et al.（2020）結合姿態估計與時序分析，實現健身動作之自動計數。

本研究採用**狀態機（State Machine）**設計，透過關節角度閾值判斷運動狀態轉換（如深蹲之「站立→下蹲→站立」循環），此方法具有可解釋性強、參數可調整之優點。

### 3.3 遊戲化理論與健身應用

遊戲化（Gamification）係指將遊戲元素應用於非遊戲情境，以提升使用者參與度（Deterding et al., 2011）。在健身領域，Hamari & Koivisto（2015）指出，遊戲化設計可顯著提升運動持續性。常見遊戲化元素包含：

- **點數與等級系統**：量化進度，提供成就感
- **挑戰與競爭**：設定目標，激發動機
- **即時回饋**：強化行為，引導改善
- **敘事與角色扮演**：增加趣味，降低枯燥感

本研究創新性地設計**怪物戰鬥系統**，將每次運動動作轉化為對怪物的攻擊傷害，並導入 **Combo 連擊機制**與**護盾修復系統**，使健身過程如同遊戲冒險，預期可有效提升使用者黏著度。

### 3.4 即時系統架構

即時運動分析系統需兼顧處理延遲與使用者體驗。WebSocket 協定提供全雙工通訊能力，相較於傳統 HTTP 輪詢，可大幅降低延遲（Fette & Melnikov, 2011）。本研究採用 Flask-SocketIO 框架，搭配多執行緒處理與幀緩衝機制，確保影像處理與使用者互動之流暢度。

---

## (四) 研究方法及步驟

### 4.1 系統架構設計

本系統採用三層式架構設計：

```
┌─────────────────────────────────────────────────────────────┐
│                     展示層 (Presentation Layer)              │
│  - 模組化前端架構（27 個獨立模組）                           │
│  - Three.js 3D 場景渲染                                     │
│  - 事件驅動通訊機制                                         │
└─────────────────────────────────────────────────────────────┘
                              │ WebSocket / HTTP
┌─────────────────────────────────────────────────────────────┐
│                     業務層 (Business Layer)                  │
│  - Flask + Socket.IO 即時通訊伺服器                         │
│  - 運動偵測服務（11 種運動類型）                            │
│  - 遊戲邏輯服務（關卡、怪物、Combo）                        │
└─────────────────────────────────────────────────────────────┘
                              │
┌─────────────────────────────────────────────────────────────┐
│                     資料層 (Data Layer)                      │
│  - MySQL 資料庫（使用者、進度、紀錄）                       │
│  - YOLOv8 模型快取                                          │
│  - 運動狀態管理                                             │
└─────────────────────────────────────────────────────────────┘
```

### 4.2 姿態檢測與角度計算

**4.2.1 YOLOv8-Pose 模型整合**

系統整合 YOLOv8n-Pose 模型，輸出 17 個人體關鍵點座標：

| 索引 | 關鍵點 | 索引 | 關鍵點 |
|:---:|:------:|:---:|:------:|
| 0 | 鼻子 | 9 | 左手腕 |
| 1-2 | 左/右眼 | 10 | 右手腕 |
| 3-4 | 左/右耳 | 11-12 | 左/右髖部 |
| 5-6 | 左/右肩膀 | 13-14 | 左/右膝蓋 |
| 7-8 | 左/右手肘 | 15-16 | 左/右腳踝 |

**4.2.2 關節角度計算**

採用向量夾角公式計算三點構成之關節角度：

$$\theta = \arccos\left(\frac{\vec{BA} \cdot \vec{BC}}{|\vec{BA}| \times |\vec{BC}|}\right)$$

其中 $\vec{BA}$、$\vec{BC}$ 分別為由頂點 B 指向端點 A、C 之向量。系統計算八個核心關節角度：

- 左/右手肘角度（二頭彎舉、伏地挺身偵測）
- 左/右膝蓋角度（深蹲偵測）
- 左/右肩膀角度（肩推偵測）
- 左/右髖部角度（姿態評估）

### 4.3 運動狀態機設計

以深蹲為例，設計二階段狀態機：

```
             ┌──────────────────────────────────┐
             │                                  │
             ▼                                  │
    ┌─────────────┐    膝蓋角度 < 90°    ┌─────────────┐
    │  站立狀態   │ ───────────────────▶ │  下蹲狀態   │
    │  (Standing) │                      │  (Squatting)│
    └─────────────┘                      └─────────────┘
             ▲                                  │
             │      膝蓋角度 > 160°             │
             │      [計數 +1]                   │
             └──────────────────────────────────┘
```

各運動類型對應之角度閾值與狀態定義，列於系統設定檔中，可依使用者身體條件調整。

### 4.4 遊戲化系統設計

**4.4.1 怪物戰鬥系統**

- **怪物類型**：史萊姆、哥布林、獸人（血量遞增）
- **攻擊機制**：每完成一次運動動作，對怪物造成傷害
- **護盾機制**：怪物具備護盾值，需先擊破護盾才能傷害血量

**4.4.2 Combo 連擊系統**

連續完成運動動作可累積 Combo 數，提升傷害倍率：

| Combo 數 | 傷害倍率 |
|:-------:|:--------:|
| 1-3 | 1.0x |
| 4-6 | 1.5x |
| 7-9 | 2.0x |
| 10+ | 3.0x |

**4.4.3 經驗值與等級系統**

- 基礎經驗值 = 10 + (關卡數 - 1) × 5
- 升級門檻採階梯增長設計
- 達成特定條件自動解鎖成就

### 4.5 研究步驟

| 階段 | 工作項目 | 預計產出 |
|:---:|---------|---------|
| 1 | 文獻蒐集與分析、技術調研 | 文獻回顧報告 |
| 2 | 系統架構設計與核心模組開發 | 系統原型 |
| 3 | 姿態檢測演算法實作與優化 | 運動偵測模組 |
| 4 | 遊戲化系統開發與整合 | 完整系統 |
| 5 | 使用者測試與效能評估 | 測試報告 |
| 6 | 論文撰寫與成果發表 | 研究論文 |

---

## (五) 預期結果

### 5.1 技術成果

1. **即時姿態檢測系統**：建構支援 11 種以上運動類型之即時偵測系統，處理延遲低於 100 毫秒，達到流暢互動之使用體驗。

2. **多運動類型統一架構**：設計可擴展之運動偵測服務架構，透過狀態機與角度閾值配置，可快速新增支援之運動類型。

3. **運動品質評估演算法**：發展結合對稱性、角度準確度與時序穩定性之多維度品質評分機制。

4. **高效能即時通訊系統**：實現 15 FPS 視訊傳輸與 5 FPS 運動計數更新之 WebSocket 通訊架構。

### 5.2 應用成果

1. **遊戲化健身平台**：完成一套具備完整遊戲機制（關卡、怪物、Combo、成就）之互動式健身訓練平台。

2. **3D 視覺化介面**：整合 Three.js 建構沉浸式 3D 訓練場景，包含角色動畫與特效系統。

3. **訓練數據分析**：建立個人化訓練歷史紀錄與統計分析功能，協助使用者追蹤進度。

### 5.3 學術成果

1. **研究論文**：預計產出一篇學術論文，投稿至國內外人工智慧或人機互動領域研討會（如 TAAI、IUI）。

2. **技術報告**：完成完整之系統架構文件與技術規格說明。

3. **開源專案**：將核心模組開源，供學術社群參考與延伸研究。

---

## (六) 需要指導教授指導內容

### 6.1 研究方向指導

1. **深度學習模型優化**：如何進一步提升 YOLOv8 模型在運動場景之準確度？是否需進行遷移學習或微調？

2. **實驗設計與評估**：遊戲化效果之量化評估應採用何種實驗設計（如隨機對照試驗）？評估指標如何選定？

3. **論文撰寫**：如何將系統開發成果轉化為具學術價值之研究論文？適合投稿之期刊或研討會建議？

### 6.2 技術層面指導

1. **系統效能優化**：在資源受限環境（如邊緣裝置）部署時，如何兼顧即時性與準確度？

2. **多人偵測擴展**：如何將系統擴展至支援多人同時運動之場景？技術挑戰與解決策略？

3. **跨平台部署**：如何將系統移植至行動裝置或嵌入式平台？需考量之技術議題？

### 6.3 專案管理指導

1. **時程規劃**：如何合理分配開發與研究時程，確保計畫如期完成？

2. **資源調度**：GPU 運算資源之申請與配置建議？

3. **成果發表**：國科會大專生計畫成果發表之相關規範與建議？

---

## (七) 參考文獻

1. Cao, Z., Simon, T., Wei, S. E., & Sheikh, Y. (2017). Realtime multi-person 2d pose estimation using part affinity fields. In *Proceedings of the IEEE Conference on Computer Vision and Pattern Recognition* (pp. 7291-7299).

2. Chen, Y., Tian, Y., & He, M. (2020). Monocular human pose estimation: A survey of deep learning-based methods. *Computer Vision and Image Understanding*, 192, 102897.

3. Deterding, S., Dixon, D., Khaled, R., & Nacke, L. (2011). From game design elements to gamefulness: defining" gamification". In *Proceedings of the 15th International Academic MindTrek Conference: Envisioning Future Media Environments* (pp. 9-15).

4. Fang, H. S., Xie, S., Tai, Y. W., & Lu, C. (2017). RMPE: Regional multi-person pose estimation. In *Proceedings of the IEEE International Conference on Computer Vision* (pp. 2334-2343).

5. Fette, I., & Melnikov, A. (2011). The websocket protocol. *RFC 6455*, 1-71.

6. Hamari, J., & Koivisto, J. (2015). Why do people use gamification services?. *International Journal of Information Management*, 35(4), 419-431.

7. Khurana, R., Ahuja, K., Yu, Z., Mankoff, J., Harrison, C., & Goel, M. (2018). GymCam: Detecting, recognizing and tracking simultaneous exercises in unconstrained scenes. *Proceedings of the ACM on Interactive, Mobile, Wearable and Ubiquitous Technologies*, 2(4), 1-17.

8. Sun, K., Xiao, B., Liu, D., & Wang, J. (2019). Deep high-resolution representation learning for visual recognition. In *Proceedings of the IEEE/CVF Conference on Computer Vision and Pattern Recognition* (pp. 5693-5703).

9. Ultralytics. (2023). YOLOv8: A new state-of-the-art computer vision model. Retrieved from https://github.com/ultralytics/ultralytics

10. World Health Organization. (2022). Global status report on physical activity 2022. Geneva: World Health Organization.

11. Johansson, G. (1973). Visual perception of biological motion and a model for its analysis. *Perception & Psychophysics*, 14(2), 201-211.

12. Loper, M., Mahmood, N., Romero, J., Pons-Moll, G., & Black, M. J. (2015). SMPL: A skinned multi-person linear model. *ACM Transactions on Graphics*, 34(6), 1-16.

13. Newell, A., Yang, K., & Deng, J. (2016). Stacked hourglass networks for human pose estimation. In *European Conference on Computer Vision* (pp. 483-499). Springer.

14. Toshev, A., & Szegedy, C. (2014). Deeppose: Human pose estimation via deep neural networks. In *Proceedings of the IEEE Conference on Computer Vision and Pattern Recognition* (pp. 1653-1660).

15. Wei, S. E., Ramakrishna, V., Kanade, T., & Sheikh, Y. (2016). Convolutional pose machines. In *Proceedings of the IEEE Conference on Computer Vision and Pattern Recognition* (pp. 4724-4732).

---

## 附錄：系統技術規格

### A. 軟體環境

| 項目 | 規格 |
|-----|------|
| 程式語言 | Python 3.8+, JavaScript ES6 |
| Web 框架 | Flask 2.3.3, Flask-SocketIO 5.3.4 |
| 深度學習 | PyTorch 2.0.1, Ultralytics YOLOv8 |
| 影像處理 | OpenCV |
| 3D 渲染 | Three.js |
| 資料庫 | MySQL 8.0+ |

### B. 硬體需求

| 項目 | 最低需求 | 建議配置 |
|-----|---------|---------|
| CPU | Intel i5 或同級 | Intel i7/i9 或 AMD Ryzen 7/9 |
| GPU | - | NVIDIA RTX 3060 以上（CUDA 12.x） |
| 記憶體 | 8 GB | 16 GB 以上 |
| 儲存空間 | 10 GB | 20 GB（含模型檔案） |

### C. 支援運動類型

| 類別 | 運動類型 |
|-----|---------|
| 健身運動 | 深蹲、二頭彎舉、肩推、伏地挺身、引體向上、啞鈴划船、平板支撐、臂膀擺動 |
| 球類運動 | 籃球（投籃、運球）、排球（高手托球、低手接球）、桌球 |
| 武術訓練 | 跆拳道 |

---

*本研究計畫書由國立高雄科技大學提出*
