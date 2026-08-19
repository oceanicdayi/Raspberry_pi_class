# 2026 Raspberry Pi 地震學學生作品整理

> 整理日期：2026-08-19  
> 範圍：依提供的 12 個學生作品連結，擷取其中與 Raspberry Pi、Raspberry Pi Pico、微型地震儀、震動感測、地震警報、資料處理與 IoT 通報直接相關的內容。  
> 注意：本文件以學生作品頁面所呈現的內容為準；其中部分「震度／PGA／預警」設定屬課堂原型或展示用途，不等同正式地震觀測站或中央氣象署作業系統。

---

## 一、作品快速比較

| # | 作品 | 核心硬體 | 資料／演算法 | 警報與輸出 | 特色 |
|---|---|---|---|---|---|
| 1 | Seismology Raspberry Pi Project | Raspberry Pi、三軸加速度感測器、LED、蜂鳴器 | 三軸加速度、最大合成加速度、方向分析 | LED、蜂鳴器、Discord、視覺化圖 | Edge-to-cloud 即時警報與 3D 合力方向 |
| 2 | Raspberry Pi 地震儀專案 | Raspberry Pi、LED、蜂鳴器 | PGA、Python/NumPy、多線程採集 | 網頁即時數據、聲音警報 | 從 LED → 蜂鳴器 → 數位觀測網的迭代歷程 |
| 3 | 地震學 × Raspberry Pi 微型地震儀 | Raspberry Pi、MPU6050、LED、蜂鳴器 | 三軸合成加速度、PGA、三級門檻 | Discord Webhook、LED PWM、蜂鳴器 PWM | 主程式完整，具三級震動門檻與約 25 Hz 採樣 |
| 4 | 第八組 Raspberry Pi 智慧地震儀 | Raspberry Pi 4、MPU6050、LED、主動式蜂鳴器 | 三軸加速度、1.2 g 閾值 | Discord Webhook、LED、蜂鳴器 | UART、Wi-Fi、I2C 到 IoT 警報的完整建置流程 |
| 5 | 地震學期末互動式總報告 | MPU6050 + Raspberry Pi 架構 | 三軸合成、PGA、L1/L2/L3 門檻；討論重力校正 | LED、蜂鳴器、Discord、網頁模擬 | 明確指出「靜止仍約 1 g」的假觸發問題與改進方向 |
| 6 | 曾柏凱期末回顧 | Raspberry Pi、MPU6050、蜂鳴器 | 高通濾波、STA/LTA、FFT、RMS、PGA、位移趨勢 | Discord、震度分級蜂鳴器 | 對 false trigger 做深入診斷與規則修正 |
| 7 | Seismology Mission Log | Raspberry Pi Pico、三軸加速度感測器、LED/Buzzer | PGA、觸發門檻、濾波、基線校正、有效震動時長 | 現場警報、資料展示；規劃雲端警報 | 從方向性震動、弱中強震到校正流程的整合 |
| 8 | Raspberry Pico 專題製作 | Raspberry Pi Pico、三軸加速度計、蜂鳴器、LED | 最大合成加速度、閾值觸發 | Discord Webhook、LED、蜂鳴器 | 地震警報之外延伸 PWM 音樂與互動遊戲 |
| 9 | 陳柏亘地震學期末學習回顧 | Raspberry Pi 4B、MPU6050 | 50 Hz、Butterworth 高通、STA/LTA、FFT、互補濾波 | LINE Notify / Discord、即時監控 | DSP 與姿態解算整合程度高 |
| 10 | 作業 11 Raspberry Pi 地震觀測站 | Raspberry Pi、MPU6050 | STA/LTA、PGA、FFT、重力扣除 | Discord Webhook、Matplotlib 圖 | Edge-to-Cloud 架構、實測速報、GPS 延伸構想 |
| 11 | 樹莓派地震儀完整實作報告 | Raspberry Pi 3/4/5、MPU6050、LED、蜂鳴器 | 100 Hz、合力、滾動緩衝、閾值 | CSV、Matplotlib、3D 向量、聲光警報 | 最完整的 UART/驅動/供電/Wi-Fi/I2C 實作教學之一 |
| 12 | 地震工程與地震學整合平台 | Raspberry Pi、MPU6050 | I2C 暫存器、16-bit 轉換、PGA 閾值、濾波概念 | LINE Notify 等 IoT 通報構想 | 從 MEMS 原理、暫存器到 IoT 防災的理論化整理 |

---

# 二、逐一作品整理

## 1. Seismology Raspberry Pi Project

**原始連結：**  
https://zxcvbnm010043-bot.github.io/seismology_raspberryPi/

### Raspberry Pi 專案內容

- 主題為「基於樹莓派之地震學監測與物聯網邊緣運算預警系統」。
- 使用 Raspberry Pi 作為感測資料處理與警報控制核心。
- 透過 MobaXterm SSH 遠端連線，並可利用 Raspberry Pi USB 接手機網路進行無線操作，降低外接螢幕與鍵盤需求。
- 感測板負責量測三軸加速度，再將資料即時傳給 Raspberry Pi。
- 麵包板端整合 LED 與蜂鳴器；偵測到劇烈晃動後，由 Raspberry Pi 判斷並控制聲光警報。
- Python 程式負責即時監測與警報。
- 結合 Discord，即時推送警告訊息並上傳視覺化圖。
- 成果包括：
  - 特定時間內的加速度變化歷史。
  - 最大合成加速度及其三維合力方向。
  - 強烈震動即時警報。
  - 以加速度資訊估算震動／烈震等級。
- 作品附實際操作影片，展示晃動觸發、LED／蜂鳴器連動與 Discord 即時推播。

### 可辨識的技術關鍵字

`Raspberry Pi`、`SSH`、`MobaXterm`、`三軸加速度`、`LED`、`Buzzer`、`Python`、`Discord`、`Edge Computing`、`IoT`

---

## 2. Raspberry Pi 地震儀專案

**原始連結：**  
https://d2844gfmyz-sys.github.io/seismology_-all/

### Raspberry Pi 專案內容

作品以「優化歷程」呈現開發過程，而不是只展示最後成果。

#### 第一階段：LED 直觀呈現
- 原先規劃以 LED 數量代表震度。
- 因線材不足與線路複雜而未完成預期設計。
- 從硬體限制開始思考更具彈性的呈現方式。

#### 第二階段：蜂鳴器警報
- 改用蜂鳴器頻率／音量模擬地震預警警報。
- 發現人耳對快速、細微的聲音變化不夠敏感。
- 因此認知到地震觀測更需要「可量化的資料視覺化」。

#### 第三階段：數位觀測網
- 將 Raspberry Pi 轉型為微型數位觀測站。
- 把即時資料傳到電腦／網頁。
- 使用圖表呈現震動波形。
- 終端機輸出即時 PGA 與有感地震分級。

### 技術架構

- Raspberry Pi：觀測站主機，負責訊號連續讀取、資料封裝與本地儲存。
- Python：撰寫多線程資料採集程式。
- NumPy：計算地動 PGA。
- 網頁：呈現地動訊號及觀測資訊。
- 開發過程也使用 AI 工具協助除錯。

### 教學價值

這份作品特別適合呈現「工程設計迭代」：

`硬體燈號 → 聲音警報 → 數據可視化`

---

## 3. 地震學 × Raspberry Pi 微型地震儀統合展示站

**原始連結：**  
https://yaxin06.github.io/2026_raspberrypi/

### Raspberry Pi 專案內容

作品將地震學理論與 Raspberry Pi 實作整合成同一條學習主線：

1. 理解地震波與地動資料。
2. MPU6050 讀取三軸加速度。
3. 將感測資料轉為可判讀數值。
4. 以 PGA／門檻做事件判斷。
5. 控制 LED、蜂鳴器與 Discord 推播。
6. 用互動式網頁展示程式、影片、困難與解法。

### 核心硬體與程式

- Raspberry Pi。
- MPU6050。
- LED：GPIO 18，使用 PWM。
- 蜂鳴器：GPIO 24，使用 PWM。
- I2C 位址：`0x68`。
- Python 套件／模組：`smbus`、`RPi.GPIO`、`requests`、`math`、`time`。

### 三級震動門檻

- L1：1.0 g
- L2：1.5 g
- L3：2.0 g
- Discord 警報冷卻時間：7 秒。
- 取樣迴圈約每 0.04 秒一次，約 25 Hz。

### 訊號與輸出

三軸合成值：

\[
A = \sqrt{a_x^2+a_y^2+a_z^2}
\]

系統記錄啟動後的最大合成加速度，並依不同門檻改變：

- LED 亮度。
- 閃爍速度。
- 蜂鳴器頻率。
- Discord 通知。
- 終端機即時進度條。

### 值得注意

此版以原始三軸合成加速度直接和 1.0 g、1.5 g、2.0 g 比較，因此靜止時重力也會進入合成值。其他學生作品已進一步討論此問題，可作為後續教學比較案例。

---

## 4. 第八組 Raspberry Pi 智慧地震儀系統

**原始連結：**  
https://u11310006-glitch.github.io/seismology-final/

### Raspberry Pi 專案內容

- Raspberry Pi 4 作為主控端。
- MPU6050 六軸感測器負責三軸加速度與陀螺儀資料。
- UART USB-to-TTL 連接筆電。
- LED 與主動式蜂鳴器提供現場聲光警報。
- 設定約 `1.2 g` 為觸發加速度閾值。

### 完整實作流程

1. **UART 連線**
   - 安裝 PL2303 驅動。
   - MobaXterm 設定 115200 bps。

2. **Wi-Fi**
   - 使用 `nmtui` 連上手機熱點。

3. **I2C**
   - 使用 `raspi-config` 啟用 I2C。
   - 使用 `i2cdetect` 確認 MPU6050 位址 `0x68`。

4. **Python**
   - 安裝 `mpu6050-raspberrypi`。
   - 即時讀取加速度、角速度、溫度。

5. **Discord**
   - 透過 Webhook 在超過閾值時推播警報。

6. **聲光警報**
   - 以 `gpiozero` 控制 LED 與蜂鳴器。

### 成果影片

網站包含三類實作影片：

- MPU6050 即時資料讀取。
- LED＋蜂鳴器聲光警報。
- Discord 即時警報訊息。

---

## 5. 地震學期末互動式總報告：微型地震儀

**原始連結：**  
https://icon826.github.io/raspberrypi/

### Raspberry Pi 專案內容

系統流程：

`MPU6050 → Python → 三軸合成加速度 / PGA → L1/L2/L3 → LED / Buzzer / Discord`

### 主要功能

- I2C 讀取 MPU6050 X/Y/Z 三軸資料。
- 計算合成加速度：

\[
A=\sqrt{x^2+y^2+z^2}
\]

- 警報門檻：
  - L1 ≥ 1.0 g
  - L2 ≥ 1.5 g
  - L3 ≥ 2.0 g
- 提供網頁版三軸輸入與警報模擬。
- 顯示目前等級、合成加速度、最高 PGA。
- LED、蜂鳴器、Discord 可依不同門檻反應。

### 作品中特別重要的反思

學生明確發現：

- MPU6050 靜止時仍會量到重力，因此 Z 軸通常接近 1 g。
- 若 L1 設為 1.0 g，可能未晃動就觸發。
- 單純使用三軸原始合成值並不夠嚴謹。

### 提出的改進方向

- 靜態校正。
- 零點偏移修正。
- 扣除重力。
- 高通濾波。
- 低通濾波降噪。
- STA/LTA 事件判斷。
- 分析主要震動方向。

這是很適合用於「從展示型原型走向科學量測」的案例。

---

## 6. 曾柏凱：Raspberry Pi 地震儀與 AI 除錯

**原始連結：**  
https://tsengpokai.github.io/FINAL_REPORT/

### Raspberry Pi 專案內容

課程 Week 14 進行 Raspberry Pi 地震儀：

- Raspberry Pi。
- UART。
- MPU6050。
- 麵包板。
- IoT 地震觀測。
- AI 協助除錯。

### 作品最大的特色：處理假觸發

早期系統問題：

- 蜂鳴器太敏感。
- 未明顯晃動就判定為地震。
- 感測器背景值漂移。
- `dynamic_gal` 超過門檻即觸發。
- 位移由加速度積分後產生嚴重漂移。
- STA/LTA 雖有繪圖，但沒有真正參與事件決策。

### 學生用於診斷的資訊

- 時序波形。
- STA/LTA。
- FFT／主頻。
- 滑動 RMS。
- PGA。
- 位移趨勢。

### 修正方向

- 加入高通濾波。
- 排除 0.05–0.07 Hz 等極低頻漂移。
- 將 STA/LTA 納入判斷。
- 增加事件持續時間條件。
- 不再瞬間超門檻就警報。
- 事件成立後才啟動蜂鳴器。
- 事件成立後依震度鳴叫約 5 秒。
- Discord 在事件確認後才推播。
- 位移改稱「相對位移趨勢」，避免當成真實位移。

### 新版事件邏輯

`事件門檻 + 持續時間 + STA/LTA / 頻率輔助 + 震度分級蜂鳴器`

此作品很適合做「問題解決能力、AI 輔助除錯、科學推理」案例。

---

## 7. Seismology Mission Log：Raspberry Pi Pico 微型地震儀

**原始連結：**  
https://earthearthton-prog.github.io/final_report/

> 註：公開 GitHub Pages 在擷取時無法由一般網頁爬取器完整載入，因此另檢視其公開 GitHub repository 的 `index.html` 原始內容。

### Raspberry Pi 專案內容

- 使用 Raspberry Pi Pico 與三軸加速度感測器。
- 整合：
  - 三軸加速度。
  - PGA。
  - 觸發門檻。
  - 濾波。
  - 基線校正。
  - LED／Buzzer。
  - 警報流程。

### Week 14：設計與製作

重點包括：

- 將感測資料轉為 PGA 與震動等級。
- 考慮背景雜訊。
- 考慮有效震動持續時間。
- 調整事件門檻。
- 比較垂直與水平震動在三軸上的主導方向。
- 以硬體、程式、感測與地震學概念完成微型地震儀。

### Week 15：校正與展示

進一步調整：

- 有效震動時長。
- 積分區間。
- 背景雜訊。
- 濾波。
- 基線修正。
- 弱／中／強震展示。
- 上下／左右晃動差異。

學生也明確指出：

- 門檻太低會把輕微碰觸誤判為較強震動。
- 警報系統不能只看蜂鳴器是否響，而應完整呈現資料處理、校正、濾波與判斷流程。

### 未來方向

可進一步發展成：

- 長時間資料記錄。
- 更完整的校正。
- 雲端警報系統。

---

## 8. Raspberry Pico 專題製作

**原始連結：**  
https://huggingface.co/spaces/JinJia0618/seiesmology_final_report

**直接專題頁：**  
https://jinjia0618-seiesmology-final-report.static.hf.space/pico.html

### 地震警報器專題

- 以 Raspberry Pi Pico 為主控。
- 連接三軸加速度計。
- 即時擷取震動訊號。
- 超過預設加速度閾值即觸發：
  - 蜂鳴器。
  - LED。
  - Discord Webhook。
- Discord 訊息包含：
  - 觸發時間。
  - 最大合成加速度 g 值。

### 其他 Raspberry Pi 延伸作品

同一頁也呈現了非地震類硬體延伸：

- 蜂鳴器播放〈給愛麗絲〉。
- LED 隨音符閃爍。
- 蜂鳴器播放〈倫敦鐵橋垮下來〉。
- Tkinter 圈圈叉叉遊戲。
- 簡單 AI 對手。
- 遊戲結果以蜂鳴器播放不同音效。

### 教學價值

此作品顯示學生已能將：

`GPIO + PWM + Python + 感測器 + API`

從地震警報延伸至音樂與互動應用。

---

## 9. 陳柏亘：低成本地震即時監測系統

**原始連結：**  
https://huggingface.co/spaces/Sapphirejimmy/seismology_final

**直接展示頁：**  
https://sapphirejimmy-seismology-final.static.hf.space/index.html

### Raspberry Pi 專案內容

使用：

- Raspberry Pi 4B。
- MPU6050。
- 三軸加速度。
- STA/LTA。
- Butterworth 高通濾波。
- FFT 頻譜分析。
- 互補濾波姿態解算。
- 即時警報推播。

### DSP 設定

頁面記載：

- 取樣率：50 Hz。
- 高通截止頻率：0.1 Hz。
- 姿態互補濾波係數：`alpha = 0.96`。
- 硬體：Raspberry Pi 4B + MPU6050。

### 系統輸出

- PGA。
- 震度估算。
- 優勢頻率。
- Roll / Pitch。
- 即時監控介面。
- STA/LTA 觸發。
- LINE Notify／Discord 自動速報。

### 開源程式

網站提供：

- `seismograph_main.py`
  - MPU6050 採集。
  - STA/LTA。
  - Butterworth 高通。
  - FFT。
  - 姿態解算。
  - Discord／LINE 推播。
- `buzzer_alarm.py`
  - GPIO PWM 蜂鳴器。
  - LED 閃爍。
  - 音效／旋律。

此作品的訊號處理與系統整合程度相對高。

---

## 10. 作業 11：Raspberry Pi 地震觀測站

**原始連結：**  
https://rrrnnn0518-ux.github.io/2026_sesmology_review/assignment11.html

### 專題定位

主題為：

**基於 Raspberry Pi 與 MPU6050 之簡易地震觀測站與即時速報系統**

強調完整 Edge-to-Cloud 流程：

`邊緣端感測 → 訊號處理 → 事件偵測 → 雲端警報`

### 核心技術

- Raspberry Pi。
- MPU6050。
- I2C。
- UART / PL2303。
- MobaXterm SSH。
- STA/LTA。
- PGA。
- FFT。
- Discord Webhook。
- Matplotlib。

### STA/LTA

- 以短時窗與長時窗的能量／平均比判斷突發震動。
- 範例 Threshold = 3.5。
- 網頁另提供互動式 STA/LTA 觸發模擬器。

### PGA

將 X/Y/Z 向量合成，再扣除重力影響，轉換為 Gal，並依 CWA 震度分級估算震度。

### FFT

- 將時域訊號轉成頻域。
- 分析主頻。
- 用於輔助辨識非地震型假觸發。

### Discord 實測

網站展示一筆即時速報：

- PGA：8.637 Gal。
- 估算震度：CWA 3 級。
- 主頻：17.87 Hz。

### 有趣的實作反思

學生嘗試用手機熱點的 IP 定位測站，結果得到的不是教室位置，而是 ISP／基地台位置，因此提出未來應外加 GPS 模組取得真正測站座標。

### 未來方向

- 校園多節點觀測網。
- Kalman Filter。
- ML 事件判讀。
- 降低誤報率。

---

## 11. 樹莓派地震儀完整實作報告

**原始連結：**  
https://minh891711.github.io/seismology_finalreport/report.html

### 專案定位

這份作品不只展示成果，也接近一份完整 Raspberry Pi 地震儀安裝手冊。

### 硬體／環境

- Raspberry Pi 3 / 4 / 5。
- MPU6050。
- USB-to-TTL。
- PL2303 / FTDI。
- MobaXterm。
- I2C。
- LED。
- 蜂鳴器。
- Python / matplotlib。

### UART 接線

- GND → Pin 6。
- RXD → Pi TXD / Pin 8。
- TXD → Pi RXD / Pin 10。
- 特別強調 TX/RX 必須交叉。

### 系統建置問題

作品記錄多項真實工程問題：

- Windows PL2303 驅動 Code 10。
- 驅動版本相容性。
- UART 供電錯誤。
- Raspberry Pi undervoltage。
- Type-C 線材壓降。
- Wi-Fi 無頭模式設定。
- `nmtui`、`raspi-config`、`nmcli`。

### MPU6050

- 啟用 I2C。
- `i2cdetect -y 1` 檢查 `0x68`。
- 安裝 `mpu6050-raspberrypi`。
- 讀取：
  - 三軸加速度。
  - 三軸角速度。
  - 溫度。
- ±2 g 量程下，以 `16384 LSB/g` 轉換。

### 系統流程

`MPU6050 100 Hz → I2C → Python → 合力 / 滾動緩衝 / 閾值 → LED + Buzzer → CSV → Matplotlib`

### 資料保存

- 超過 1.0 g 觸發。
- 儲存事件前後約 10 秒。
- 輸出 CSV。
- 產生三軸時序圖。
- 繪製 3D 合力向量。

### 未來方向

- 更高靈敏度 MEMS 地震計。
- 與 TSMIP 即時資料比對。
- 戶外固定站長期背景噪聲監測。

---

## 12. 地震觀測與工程知識整合平台：樹莓派微型震動系統

**原始連結：**  
https://hsiaoyunan36-seismologyterm-framework.static.hf.space/index.html

### 專案動機

以低成本 MEMS + Raspberry Pi 取代昂貴地震觀測設備的部分教學／展示功能，思考：

- 家庭。
- 智慧大樓。
- 社區防災。
- 高密度微型強震監測網。

### 硬體

#### Raspberry Pi
負責：

- Linux。
- I2C。
- 固定頻率採樣。
- 即時濾波。
- 事件判斷。
- 網路通報。

#### MPU6050
內含：

- 三軸加速度計。
- 三軸陀螺儀。
- 16-bit 數位輸出。

### I2C 接線

- VCC → Pin 1 / 3.3V。
- GND → Pin 6。
- SCL → Pin 5 / GPIO 3。
- SDA → Pin 3 / GPIO 2。

### 暫存器與數值轉換

- MPU6050 位址：`0x68`。
- Power Management register：`0x6B`。
- 設定採樣率／DLPF。
- 將 High Byte / Low Byte 組成 16-bit 值。
- 以二補數還原負值。
- ±2g 設定下：

\[
a(g)=\frac{\text{Signed Value}}{16384}
\]

### IoT 警報概念

- 以 PGA／動態加速度門檻判斷異常晃動。
- 範例門檻：`ΔA > 0.05 g`，但網站也指出實際值必須依安裝環境校正。
- 可透過網路 API／Socket／LINE 類型服務推送警報。

### 作品定位

與其他作品相比，這份內容較偏：

`MEMS 原理 → I2C 暫存器 → 數值轉換 → IoT 防災架構`

適合用作技術原理與硬體底層教學案例。

---

# 三、12 份作品共同出現的技術主題

## 1. 硬體層

最常見組合：

- Raspberry Pi / Raspberry Pi Pico。
- MPU6050。
- 麵包板。
- 杜邦線。
- LED。
- 蜂鳴器。
- UART USB-to-TTL。
- I2C。

## 2. 資料處理層

由簡至繁可分為：

### Level A：直接加速度門檻
- X/Y/Z 三軸。
- 合成加速度。
- 固定 g 值 threshold。

### Level B：PGA 與資料保存
- PGA。
- 滾動緩衝。
- CSV。
- 時序圖。
- 3D 向量。

### Level C：事件偵測與訊號處理
- 基線校正。
- 重力移除。
- 高通／低通濾波。
- STA/LTA。
- FFT。
- RMS。
- 主頻分析。
- 持續時間判斷。

### Level D：進階系統整合
- 姿態解算。
- 互補濾波。
- IoT Edge-to-Cloud。
- 多節點觀測網。
- ML 事件分類構想。

## 3. 警報層

常見輸出：

- LED。
- 蜂鳴器。
- Discord Webhook。
- LINE 類型即時通知。
- 終端機即時畫面。
- Web dashboard。
- Matplotlib 波形圖。

---

# 四、從作品發展可看出的學習進階路徑

這批作品呈現出相當清楚的進階路徑：

1. **能把感測器接起來**
2. **能讓 Raspberry Pi 讀到資料**
3. **能用 LED／蜂鳴器做出回饋**
4. **能把資料傳到 Discord／網頁**
5. **能計算三軸合成與 PGA**
6. **開始發現重力、雜訊與漂移造成假觸發**
7. **加入濾波、STA/LTA、FFT、事件持續時間**
8. **開始區分「展示型警報器」與「較嚴謹的地震觀測系統」**
9. **思考多測站、GPS、雲端、ML 與真實觀測網**

此路徑可進一步作為學生作品評量 rubric 或教學研究中的作品層級分類依據。

---

# 五、全部原始連結

1. https://zxcvbnm010043-bot.github.io/seismology_raspberryPi/
2. https://d2844gfmyz-sys.github.io/seismology_-all/
3. https://yaxin06.github.io/2026_raspberrypi/
4. https://u11310006-glitch.github.io/seismology-final/
5. https://icon826.github.io/raspberrypi/
6. https://tsengpokai.github.io/FINAL_REPORT/
7. https://earthearthton-prog.github.io/final_report/
8. https://huggingface.co/spaces/JinJia0618/seiesmology_final_report
9. https://huggingface.co/spaces/Sapphirejimmy/seismology_final
10. https://rrrnnn0518-ux.github.io/2026_sesmology_review/assignment11.html
11. https://minh891711.github.io/seismology_finalreport/report.html
12. https://hsiaoyunan36-seismologyterm-framework.static.hf.space/index.html

---

# 六、Hugging Face 直接展示頁補充

部分 Hugging Face Space 的內容實際顯示於 iframe／static Space：

- JinJia0618 專題頁：  
  https://jinjia0618-seiesmology-final-report.static.hf.space/pico.html

- Sapphirejimmy 完整展示頁：  
  https://sapphirejimmy-seismology-final.static.hf.space/index.html

---

## 整理結語

2026 年這批 Raspberry Pi 學生作品的共同核心，不只是「做一個會響的地震警報器」，而是逐漸形成一條完整的 STEM／地球物理實作鏈：

**地震學概念 → MEMS 感測 → Python → 訊號處理 → 事件判斷 → 聲光輸出 → IoT 通報 → 網頁數位敘事 → 問題診斷與改良。**

其中尤其值得後續教學研究分析的面向包括：

- 是否處理重力與基線問題。
- 是否考慮 false trigger。
- 是否從瞬間門檻進階至 STA/LTA／持續時間判斷。
- 是否能用圖表證據支持程式修改。
- 是否能將 Raspberry Pi 技術與地震學概念真正連結。
- 是否能反思教學原型與正式觀測儀器之間的差距。
