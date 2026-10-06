# Current Buffer Design (Common-Gate & Cascode) - README

本專案為共閘極電流緩衝器（Common-Gate Current Buffer）與疊接式電流緩衝器（Cascode Current Buffer）之設計與模擬整理，由 Chao-Yang Zhan 撰寫於 2026 年 6 月 18 日[cite: 70]。內容包含理論推導、SPICE 電路圖、直流工作點與交流模擬驗證。

---

## 📋 系統設計規格 (Design Specifications)
* **電源電壓 ($V_{DD}$)**：$1.8\text{ V}$[cite: 70]
* **負載電阻 ($R_4$)**：$100\text{ k}\Omega$[cite: 70]
* **製程與電晶體參數**：閾值電壓 $V_{th} = 0.35\text{ V}$，製程參數 $k_p = 200\ \mu\text{A/V}^2$[cite: 70]
* **設計目標**：電流增益 $> 0.95\text{ A/A}$，總功率消耗 $< 0.6\text{ mW}$[cite: 70]

---

## 1. 共閘極放大器設計 (Common-Gate Amplifier)
* **偏壓與功率設計**：選定汲極電流 $I_D = 9\ \mu\text{A}$、偏置網路電流 $I_{bias} \approx 1.8\ \mu\text{A}$，總電流 $I_{total} = 10.8\ \mu\text{A}$，總功率消耗 $P_{total} = 19.44\ \mu\text{W}$，遠低於 $0.6\text{ mW}$ 上限[cite: 70]。
* **元件尺寸與偏壓**：源極電阻 $R_3 = 50\text{ k}\Omega$ 使 $V_S = 0.45\text{ V}$[cite: 70]。電晶體 $M_1$ 尺寸設定為 $W = 50\ \mu\text{m}$、$L = 0.5\ \mu\text{m}$[cite: 71]。分壓電阻 $R_1 = 500\text{ k}\Omega$、$R_2 = 500\text{ k}\Omega$ 提供 $V_G = 0.9\text{ V}$[cite: 71]。
* **模擬驗證**：實際 DC 工作點模擬顯示總電流 $I_{total} = 12.16\ \mu\text{A}$，總功率 $P_{total} \approx 21.88\ \mu\text{W}$[cite: 72, 73]。電流增益 $A_i \approx 0.962\text{ A/A}$，成功達成 $> 0.95$ 之規範[cite: 73]。

| 電路原理圖與配置 | DC 工作點與暫態響應 |
| :---: | :---: |
| ![CG Schematic](figures/cg_schematic.png)<br>*圖 1：共閘極放大器 SPICE 電路圖*[cite: 71] | ![CG Op Point](figures/cg_op.png)<br>*圖 2：DC 工作點與電流增益輸出波形*[cite: 72, 73] |

---

## 2. 疊接式電流緩衝器設計 (Cascode Current Buffer)
* **偏壓與功率設計**：選定汲極電流 $I_D = 20\ \mu\text{A}$、偏置電流 $I_{bias} \approx 2\ \mu\text{A}$，總電流 $I_{total} = 22\ \mu\text{A}$，總功率消耗 $P_{total} = 39.6\ \mu\text{W}$[cite: 74]。
* **元件尺寸與偏壓**：採用 $R_1 = R_2 = 500\text{ k}\Omega$ 設定 $V_G = 0.9\text{ V}$[cite: 74]。$M_2$ 採二極體連接，尺寸 $W_2 = 1\ \mu\text{m}$、$L = 0.5\ \mu\text{m}$[cite: 74, 75]。$M_1$ 尺寸設定為 $W_1 = 380\ \mu\text{m}$、$L = 0.5\ \mu\text{m}$[cite: 75]。負載電阻 $R_4 = 20\text{ k}\Omega$[cite: 75]。
* **模擬驗證**：實際總電流 $9.04\ \mu\text{A}$，總功率 $P_{total} \approx 16.27\ \mu\text{W}$[cite: 75, 76]。測得電流增益 $A_i \approx 0.952\text{ A/A}$，滿足設計需求[cite: 77]。

| 疊接式電路原理圖 | 工作點與電流增益波形 |
| :---: | :---: |
| ![Cascode Schematic](figures/cascode_schematic.png)<br>*圖 3：Cascode 電流緩衝器電路圖*[cite: 75] | ![Cascode Op Point](figures/cascode_op.png)<br>*圖 4：Cascode 體系之 DC 工作點與波形*[cite: 75, 76] |

---

## 3. 架構效能比較與總結
* **效能對比**：
  * **共閘極 (CG)**：電流增益 $0.962$，輸入阻抗約為 $1/g_{m1}$，設計靈活性較低[cite: 73, 77]。
  * **疊接式 (Cascode)**：電流增益 $0.952$，輸入阻抗約為 $1/g_{m1} \parallel 1/g_{m2}$，具備高設計靈活性與確定性增益控制[cite: 77]。
* **主要優勢**：
  * **精確增益控制**：透過寬長比設定可精確調控跨導比例，使電流轉移率高度可預測[cite: 77]。
  * **設計效率高**：偏置需求與緩衝效能解耦，使設計迭代更加簡便。
  * **高強健性**：提供優異的輸出阻抗與穩定的 $M_1$ 直流操作點。