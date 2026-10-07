# Current Buffer Design (Common-Gate & Cascode) - README

本專案為共閘極電流緩衝器（Common-Gate Current Buffer）與疊接式電流緩衝器（Cascode Current Buffer）之設計與模擬整理。內容包含理論推導、SPICE 電路圖、直流工作點與交流模擬驗證。

---

## 📋 系統設計規格 (Design Specifications)
* **電源電壓 (V<sub>DD</sub>)**：1.8 V
* **負載電阻 (R<sub>4</sub>)**：100 kΩ（共閘極）/ 20 kΩ（疊接式）
* **製程與電晶體參數**：閾值電壓 V<sub>th</sub> = 0.35 V，製程參數 k<sub>p</sub> = 200 μA/V<sup>2</sup>
* **設計目標**：電流增益 > 0.95 A/A，總功率消耗 < 0.6 mW

---

## 1. 共閘極放大器設計 (Common-Gate Amplifier)
* **偏壓與功率設計**：選定汲極電流 I<sub>D</sub> = 9 μA、偏置網路電流 I<sub>bias</sub> ≈ 1.8 μA，總電流 I<sub>total</sub> = 10.8 μA，總功率消耗 P<sub>total</sub> = 19.44 μW，遠低於 0.6 mW 上限。
* **元件尺寸與偏壓**：源極電阻 R<sub>3</sub> = 50 kΩ 使 V<sub>S</sub> = 0.45 V。電晶體 M<sub>1</sub> 尺寸設定為 W = 50 μm、L = 0.5 μm。分壓電阻 R<sub>1</sub> = 500 kΩ、R<sub>2</sub> = 500 kΩ 提供 V<sub>G</sub> = 0.9 V。
* **模擬驗證**：實際 DC 工作點模擬顯示總電流 I<sub>total</sub> = 12.16 μA，總功率 P<sub>total</sub> ≈ 21.88 μW。電流增益 A<sub>i</sub> ≈ 0.962 A/A，成功達成 > 0.95 之規範。

| 共閘極電路原理圖 | DC 工作點分析 |
| :---: | :---: |
| ![CG Schematic](figures/cg_schematic.png)<br>*圖 1：共閘極放大器 SPICE 電路圖* | ![CG Op Point](figures/cg_op.png)<br>*圖 2：DC 工作點分析 (.op)* |

| 原始負載電流波形 (含 DC 偏移) | 交流輸入訊號 | 交流輸出訊號 |
| :---: | :---: | :---: |
| ![CG Original Wave](figures/cg_wave_original.png)<br>*圖 3：原始 I(R<sub>4</sub>) 帶有直流偏移之波形* | ![CG Input Wave](figures/cg_wave_in.png)<br>*圖 4(a)：輸入 I<sub>in</sub>(AC)* | ![CG Output Wave](figures/cg_wave_out.png)<br>*圖 4(b)：輸出 I<sub>out</sub>(AC)* |

---

## 2. 疊接式電流緩衝器設計 (Cascode Current Buffer)
* **偏壓與功率設計**：選定汲極電流 I<sub>D</sub> = 20 μA、偏置電流 I<sub>bias</sub> ≈ 2 μA，總電流 I<sub>total</sub> = 22 μA，總功率消耗 P<sub>total</sub> = 39.6 μW。
* **元件尺寸與偏壓**：採用 R<sub>1</sub> = R<sub>2</sub> = 500 kΩ 設定 V<sub>G</sub> = 0.9 V。M<sub>2</sub> 採二極體連接，尺寸 W<sub>2</sub> = 1 μm、L = 0.5 μm。M<sub>1</sub> 尺寸設定為 W<sub>1</sub> = 380 μm、L = 0.5 μm。負載電阻 R<sub>4</sub> = 20 kΩ。
* **模擬驗證**：實際總電流 9.04 μA，總功率 P<sub>total</sub> ≈ 16.27 μW。測得電流增益 A<sub>i</sub> ≈ 0.952 A/A，滿足設計需求。

| 疊接式電路原理圖 | DC 工作點分析 |
| :---: | :---: |
| ![Cascode Schematic](figures/cascode_schematic.png)<br>*圖 5：Cascode 電流緩衝器電路圖* | ![Cascode Op Point](figures/cascode_op.png)<br>*圖 6：Cascode 體系之 DC 工作點分析* |

| 原始負載電流波形 (含 DC 偏移) | 交流輸入訊號 | 交流輸出訊號 |
| :---: | :---: | :---: |
| ![Cascode Original Wave](figures/cascode_wave_original.png)<br>*圖 7：原始 I(R<sub>4</sub>) 帶有直流偏移之波形* | ![Cascode Input Wave](figures/cascode_wave_in.png)<br>*圖 8(a)：輸入 I<sub>in</sub>(AC)* | ![Cascode Output Wave](figures/cascode_wave_out.png)<br>*圖 8(b)：輸出 I<sub>out</sub>(AC)* |

---

## 3. 架構效能比較與總結
* **效能對比**：
  * **共閘極 (CG)**：電流增益 0.962，輸入阻抗約為 1/g<sub>m1</sub>，設計靈活性較低。
  * **疊接式 (Cascode)**：電流增益 0.952，輸入阻抗約為 1/g<sub>m1</sub> ∥ 1/g<sub>m2</sub>，具備高設計靈活性與確定性增益控制。
* **主要優勢**：
  * **精確增益控制**：透過寬長比設定可精確調控跨導比例，使電流轉移率高度可預測。
  * **設計效率高**：偏置需求與緩衝效能解耦，使設計迭代更加簡便。
  * **高強健性**：提供優異的輸出阻抗與穩定的 M<sub>1</sub> 直流操作點。