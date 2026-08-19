# 2023 年 6 月地球物理學課程：ESP32 / NodeMCU 自製地震儀學生作品整理

> 整理日期：2026-08-19  
> 課程時段：2023 年 6 月，地球物理學課程  
> 整理範圍：依使用者提供的 1 個 Canva 連結與 3 份 PDF 附件，擷取其中與 ESP32、NodeMCU、ESP8266、Arduino 類微控制器、自製地震儀、MPU6050 / GY-521、震動量測、資料紀錄與地震訊號分析直接相關的內容。  
> 整理原則：以學生作品實際呈現為準，不自行補齊來源未提供的資訊。

---

# 一、重要名詞與硬體名稱說明

本批附件檔名使用「esp32」，但學生作品內文實際出現的控制器名稱包含：

- **NodeMCU 開發板**
- **ESP8266**
- 「Arduino 地震儀」的稱呼
- MicroPython `machine` 模組

因此，本整理不將所有裝置一律改寫成 ESP32，而採用：

> **ESP32 / NodeMCU / ESP8266 類微控制器地震儀**

作為這批作品的統稱。

若後續要進行 2023 → 2024 → 2026 的技術縱向比較，建議將此批作品獨立標記為：

```text
2023-06
NodeMCU / ESP8266 類地震儀
```

而不要與同年後續的 Raspberry Pi Pico 作業混在同一硬體類別。

---

# 二、資料來源

## 1. Canva：第三組地震學期末專題

原始連結：

https://www.canva.com/design/DAFlSyKYy8c/4jwW0EDJcdW5F2M-dYIxDw/edit

### 可讀內容

- 第三組
- NodeMCU
- 加速度儀
- LED
- 蜂鳴器
- 開關
- Wi-Fi
- NTP
- UTC 時間
- 模擬 P 波 / S 波
- 文字資料輸出
- SAC 轉檔
- SWARM
- Spectrogram
- 地震波結果圖
- 學生心得

---

## 2. 附件：`地震儀-esp32第4組.pdf`

- 共 14 頁。
- 第 4 組。
- 組員：
  - 邱沛瑄
  - 林佑儒
  - 黃天賜
- 硬體：
  - NodeMCU 開發板與擴充板
  - GY-521 加速度儀
  - 開關
  - LED
  - 蜂鳴器
- 重點：
  - 完整接線說明
  - PWM 聲光警報
  - 高低聲交替
  - 波形輸出
  - 實測影片

---

## 3. 附件：`地震儀小專題esp32.pdf`

- 共 21 頁。
- 地生系二，第五組。
- 組員：
  - 林貫益
  - 張盛翔
  - 陳泓愷
- 專題：
  - **檢測機車在路上行駛時的震動**
- 重點：
  - 自製地震儀放入機車車廂實測
  - 5.5 km 實際騎乘
  - Arduino / 自製地震儀與手機 phyphox 對照
  - 10 Hz vs 100 Hz
  - 坑洞、橋梁伸縮縫、轉彎、煞車、起步
  - 頻譜分析
  - SWARM / Google 試算表
  - 實驗失敗與備援設計

---

## 4. 附件：`地震學期末報告esp32.pdf`

- 共 24 頁。
- 成員：
  - U11010017 籃畯宏
  - U10910015 陳佑豪
- 前半部包含：
  - SWARM 地震定位
  - Python 地震規模
  - Python + PyGMT 震度分析
- 與 ESP / 自製地震儀最直接相關的部分從「自製地震儀」開始。
- 核心特色：
  - 將原本單一 `threshold` 改成多級門檻
  - `threshold_big`
  - `threshold_small`
  - 強震 / 弱震差異化警報
  - LED 與蜂鳴器分級反應

---

# 三、四份作品快速比較

| 來源 | 組別 | 控制器 / 平台 | 感測器 | 主要程式功能 | 分析 / 輸出 | 主要特色 |
|---|---|---|---|---|---|---|
| Canva | 第三組 | NodeMCU / ESP8266 | 加速度儀 | Wi-Fi、NTP、警報、時間戳、8 秒記錄 | SAC、SWARM、spectrogram | 把地震儀資料一路做到地震學軟體分析 |
| 第4組 PDF | 第4組 | NodeMCU + 擴充板 | GY-521 | PWM LED / buzzer、threshold | 三軸波形 | 高低聲警報與 LED 強弱交替 |
| 機車專題 PDF | 第五組 | 作品稱 Arduino 地震儀 | 加速度儀 | 模式切換、0.1 s 記錄 | Google Sheet、phyphox、頻譜 | 5.5 km 真實行車震動實驗 |
| 地震學期末報告 PDF | 籃畯宏、陳佑豪 | 自製地震儀平台 | 加速度儀 | 多級 threshold | LED / buzzer 分級 | 從單門檻進展到弱震 / 強震判斷 |

---

# 四、作品 A：第三組 — NodeMCU 地震學期末專題

來源：

https://www.canva.com/design/DAFlSyKYy8c/4jwW0EDJcdW5F2M-dYIxDw/edit

---

## 4.1 系統硬體

Canva 列出的硬體包括：

- NodeMCU
- 麵包板
- 杜邦線
- LED
- 加速度儀
- 蜂鳴器
- 開關

可整理成：

```text
NodeMCU
   │
   ├─ Accelerometer
   ├─ LED
   ├─ Buzzer
   └─ Button
```

---

## 4.2 腳位

學生作品中明確列出：

```text
D1 / pin 5  → 地震儀 SCL
D2 / pin 4  → 地震儀 SDA
D6 / pin 12 → 按鈕
D5 / pin 14 → LED
D7 / pin 13 → 蜂鳴器
3V          → 地震儀 VCC
GND         → 共地
```

因此核心通訊為：

```text
NodeMCU
  ↓
I2C
  ↓
Accelerometer
```

---

# 五、第三組的地震模擬概念

第三組不是只做「搖一下就響」，而是把一次地震的概念拆成：

```text
收到地震波訊號
     ↓
震動一下
     ↓
持續震動
     ↓
搖晃劇烈
     ↓
慢慢衰減
```

並將其對應成：

```text
模擬 P 波
→ 地震持續
→ 模擬 S 波
→ 地震衰減
```

這表示學生開始嘗試把：

```text
硬體震動
```

與：

```text
地震波隨時間發展
```

做概念連結。

---

# 六、第三組的聲光警報

## 6.1 蜂鳴器

學生設計：

```text
蜂鳴器一大聲、一小聲
```

並說明：

- 約持續 2 秒。
- 用來模擬真實地震警報聲。

---

## 6.2 LED

學生設計：

```text
LED 由暗到亮
```

並讓 LED 持續變化：

```text
直到地震波觀測結束
```

因此警報不是單一 ON / OFF，而有簡單的時間變化設計。

---

# 七、第三組 Wi-Fi 與 ESP8266

Canva 程式說明直接寫：

> 將 ESP8266 連網。

學生需要：

- 修改自己的 SSID。
- 修改 Wi-Fi 密碼。
- 開啟筆電熱點。
- 連線成功後顯示 IP address。

流程：

```text
ESP8266 / NodeMCU
      ↓
Wi-Fi hotspot
      ↓
connected
      ↓
IP address
```

---

# 八、第三組 NTP 與時間

學生使用 NTP server：

```text
Wi-Fi
 ↓
NTP
 ↓
取得 UTC time
```

可取得：

- 年
- 月
- 日
- 時
- 分
- 秒
- 星期
- 一年中的第幾天

學生也注意到：

```text
UTC = GMT+0
Taiwan = UTC+8
```

因此台灣時間需要再加 8 小時。

---

# 九、第三組對「即時時間」的程式理解

一個相當重要的反思是：

> 取得時間的程式必須放在與資料記錄相同的迴圈裡，才能得到持續更新的即時資料。

這表示學生已理解：

```text
初始化時讀一次 time
≠
每個 sample 的 timestamp
```

應該是：

```text
while recording:
    read time
    read acceleration
    write sample
```

而不是：

```text
read time once
while recording:
    reuse same time
```

---

# 十、第三組資料記錄時間

學生設定：

```text
超過 8 秒後記錄結束
```

因此此系統比較接近：

```text
短事件記錄器
```

而不是無限期連續地震站。

---

# 十一、第三組從文字檔轉 SAC

Canva 作品特別包含：

> 將文字檔轉成 SAC 檔。

學生的流程包括：

1. 讀取檔案名稱。
2. 三軸資料需各自處理。
3. 因時間資料放在第一欄，所以三軸資料使用後續欄位。
4. 匯出 SAC。
5. 在地震學工具中讀取。

可整理為：

```text
NodeMCU record
   ↓
TXT
   ↓
Python
   ↓
three components
   ↓
SAC
   ↓
SWARM / waveform analysis
```

這是本批 2023 作品中很重要的跨層次整合。

---

# 十二、第三組 SWARM 與 Spectrogram

學生進一步：

- 從 Stream 選擇第一條 trace。
- 讀取檔案。
- 將地震波訊號重複 10 次。
- 繪製：
  - 地震波結果圖。
  - Spectrogram。

因此其學習流程已跨越：

```text
微控制器
→ 感測
→ 資料檔
→ Python
→ 地震波格式
→ 時頻分析
```

---

# 十三、第三組除錯經驗

學生心得記錄一個非常典型的硬體問題：

```text
LED 不亮
+
蜂鳴器不響
```

起初：

- 重複檢查程式。
- 請助教協助檢查。

最後發現原因只是：

```text
GND 接錯
```

這是一個清楚的例子：

> 軟體看起來像出錯，但根因其實是硬體 wiring。

---

# 十四、第三組學生對程式修改的反思

負責程式設計的學生提到：

- 原本較不熟麵包板。
- 熟悉後較能判斷程式應改哪裡。
- 曾遇到蜂鳴器原本只想響 2 秒，但加入大小聲變化後卻持續響。
- 經提問與除錯後解決。

這表示學生已開始進行：

```text
需求
→ 改程式
→ 出現副作用
→ debug
→ 再測試
```

---

# 十五、作品 B：第四組 — NodeMCU + GY-521 自製地震儀

來源附件：

`地震儀-esp32第4組.pdf`

## 15.1 組員

- 邱沛瑄
- 林佑儒
- 黃天賜

---

# 十六、第四組系統硬體

PDF 第 2 頁的系統圖顯示：

- NodeMCU 開發板。
- 擴充板。
- GY-521 加速度儀。
- 開關按鈕。
- LED。
- 蜂鳴器。

整體：

```text
GY-521
   ↓
NodeMCU
   ↓
├─ LED
├─ buzzer
└─ button
```

---

# 十七、第四組 GY-521 接線

PDF 第 3 頁顯示：

```text
GY-521 VCC → 擴充板 3V
GY-521 GND → 擴充板 GND
GY-521 SCL → 擴充板 D1
GY-521 SDA → 擴充板 D2
```

這與 NodeMCU 的 I2C 使用一致。

---

# 十八、第四組開關接線

PDF 第 4 頁：

```text
Button OUT → D6
Button GND → GND
```

開關用於控制地震儀的操作流程。

---

# 十九、第四組 LED 接線

PDF 第 5 頁：

```text
LED → D5
LED → GND
```

---

# 二十、第四組蜂鳴器接線

PDF 第 6 頁：

```text
Buzzer → D7
Buzzer → GND
```

---

# 二十一、第四組程式中的 GPIO

程式截圖可辨識：

```python
i2c = I2C(scl=Pin(5), sda=Pin(4))
status = Pin(2, Pin.OUT)
button = Pin(12, Pin.IN, Pin.PULL_UP)
led = Pin(14, Pin.OUT)
buzzer = Pin(13, Pin.OUT)
```

也就是：

| 元件 | Pin |
|---|---:|
| I2C SCL | 5 |
| I2C SDA | 4 |
| Button | 12 |
| LED | 14 |
| Buzzer | 13 |

---

# 二十二、第四組 threshold

程式畫面可看到：

```python
threshold = 1
```

來源沒有把這個值進一步對應到正式地震震度，因此本整理只稱：

> 加速度觸發門檻。

---

# 二十三、第四組 PWM 聲光效果

第四組自行修改的重要部分是：

> 設計高低聲之警告鈴。

程式加入：

```python
led_pwm = PWM(...)
buzzer_pwm = PWM(...)
```

並把 PWM frequency 調高。

---

## 23.1 LED

學生用：

```text
duty 100
↔
duty 1000
```

每：

```text
0.1 秒
```

切換一次。

目的：

```text
製造 LED 一閃一閃的差距
```

---

## 23.2 蜂鳴器

蜂鳴器則：

```text
duty 100
↔
duty 500
```

同樣約每：

```text
0.1 秒
```

切換。

學生將其描述為：

```text
創造高低聲之變化
```

---

# 二十四、第四組警報邏輯

程式截圖顯示：

```python
if data[0] >= threshold \
or data[1] >= threshold \
or data[2] >= threshold:
```

即：

```text
X or Y or Z
任一軸超過門檻
→ trigger
```

觸發後：

```text
LED
+
buzzer
+
record file
```

並執行交替 PWM 效果。

---

# 二十五、第四組實際測試

PDF 第 9 頁：

> 實測反應影片。

可看到已組裝完成的：

- NodeMCU。
- 麵包板。
- GY-521。
- LED。
- 蜂鳴器。

因此此組完成了實體裝置測試。

---

# 二十六、第四組波形輸出

PDF 第 10–12 頁展示三個波形輸出。

檔案標示包括：

```text
Report6.x
Report6.y
```

圖中可見三分量波形。

其中部分圖的 trace label 類似：

```text
BW.RJOB.EHZ
BW.RJOB.EHN
BW.RJOB.EHE
```

這些頁面顯示學生已把「自製裝置」與「標準地震波形視覺化」放在同一份成果中。

> 來源沒有清楚說明這三張標準波形是否全部由自製 GY-521 實測而來，因此本整理不額外推定其資料來源。

---

# 二十七、第四組學習反思

學生普遍提到：

- 三週時間很趕。
- 過去學過 Python，但：
  - 資料分析 Python
  - 控制硬體的 Python / MicroPython  
  感受上不同。
- 學會：
  - 接線。
  - 把程式燒入晶片。
  - 操作微型地震儀。
  - 理解地震儀運作。

---

# 二十八、作品 C：第五組 — 用自製地震儀量測機車行駛震動

來源：

`地震儀小專題esp32.pdf`

## 28.1 組員

地生系二，第五組：

- 林貫益
- 張盛翔
- 陳泓愷

## 28.2 題目

**檢測機車在路上行駛時的震動**

這是本批資料中最接近：

```text
真實場域感測實驗
```

的一件作品。

---

# 二十九、第五組硬體

學生將作品稱為：

```text
Arduino 地震儀
```

儀器包含：

- 加速度儀。
- 電路板。
- LED。
- 蜂鳴器。
- 開關。
- 電池。
- 麵包板。
- 地震儀外盒。

學生特別使用雙面膠固定麵包板，以避免：

```text
騎乘震動
→ 線路鬆脫
→ 資料失敗
```

這已經開始考慮真實場域中的 mechanical stability。

---

# 三十、第五組操作狀態機

學生把程式分成三個主要狀態：

## 30.1 待機模式

```text
面板燈持續短閃
```

---

## 30.2 觀察模式

長按按鈕約 1 秒進入。

模式切換時：

```text
LED + buzzer 長閃 / 響 3 次
```

觀察模式就緒後：

```text
面板燈恆亮
```

---

## 30.3 記錄模式

輕觸按鈕：

```text
LED + buzzer 閃爍 3 次
→ 開始記錄
```

記錄中：

```text
LED 恆亮
buzzer 關閉
```

---

# 三十一、第五組結束記錄方式

有兩種：

## 31.1 手動結束

開始記錄 3 秒後，按下按鈕可提前結束。

---

## 31.2 自動結束

```text
約 20 分鐘
```

後自動結束。

結束時：

```text
LED + buzzer 快閃 / 響 5 次
```

之後回到待機模式。

---

# 三十二、第五組加速度換算

程式截圖顯示：

```python
data = [
    float(ori_data["AcX"]) / -16384,
    float(ori_data["AcY"]) / -16384,
    float(ori_data["AcZ"]) / -16384
]
```

也就是將 MPU6050 類 raw counts：

```text
÷ 16384
```

轉成約以 `g` 為尺度的加速度值。

這是本批資料中很清楚的：

```text
raw sensor counts
→ physical-scaled acceleration
```

處理。

---

# 三十三、第五組取樣率

程式：

```python
time.sleep(0.1)
```

因此自製地震儀約為：

```text
10 Hz
```

學生後續也明確比較：

- 自製地震儀：10 Hz。
- 手機 phyphox：100 Hz。

---

# 三十四、第五組檔案記錄

程式建立：

```text
report100.txt
report101.txt
...
```

並寫入：

- X。
- Y。
- Z。
- time。

因此：

```text
sensor
→ text file
→ later analysis
```

---

# 三十五、第五組實地實驗

## 日期與時間

```text
2023/06/08
20:12:02 ~ 20:31:03
```

## 儀器位置

```text
125 c.c. 機車車廂內
```

## 距離

```text
約 5.5 km
```

## 路線

由：

```text
臺北市立大學機車車庫
```

騎往：

```text
中和宜安路附近
```

途中經過：

- 重慶南路。
- 牯嶺街。
- 南海路。
- 中正橋。
- 永和路。
- 中和路。
- 宜安路。

---

# 三十六、第五組備援觀測設計

學生擔心：

- 自製地震儀故障。
- 資料遺失。
- 時間錯位。
- SWARM 無法讀取。

因此加入兩台手機作備援。

使用：

```text
phyphox
```

取樣：

```text
每 0.01 sec
```

即約：

```text
100 Hz
```

這是一個很好的研究設計概念：

```text
primary sensor
+
redundant sensors
```

---

# 三十七、第五組影像同步

除了地震儀與手機外，學生還使用：

- 行車記錄器。
- 手機畫面。
- 時間戳。

目的：

```text
道路事件
↔
時間
↔
波形
```

例如辨識：

- 坑洞。
- 伸縮縫。
- 轉彎。
- 煞車。
- 起步。

---

# 三十八、第五組時間同步問題

學生嘗試把：

```text
video time
+
self-made sensor time
+
phone time
```

疊合。

但仍有：

```text
1–2 秒 lag
```

此問題對後續分析造成明顯影響。

---

# 三十九、第五組 SWARM 問題

學生原本希望：

```text
自製地震儀資料
→ 轉檔
→ SWARM
```

但遇到：

```text
SWARM 無法開啟轉好的檔案
```

因此最後改用：

```text
Google 試算表
```

繪製波形。

這是典型：

```text
原計畫工具失敗
→ 改用替代工具
→ 繼續完成分析
```

---

# 四十、第五組自製地震儀 vs 手機

## 自製地震儀

```text
10 Hz
```

## 手機 phyphox

```text
100 Hz
```

學生指出：

- 手機解析度較高。
- 自製地震儀資料量較小。
- 自製地震儀資料在 Google 試算表較容易處理。
- 手機資料量很大，試算表處理負擔高。

這是很直接的：

```text
sampling rate
↔
time resolution
↔
data volume
↔
processing cost
```

權衡。

---

# 四十一、第五組軸向問題

學生發現：

- 手機 X 軸與 Y 軸方向。
- 自製地震儀 X/Y 軸方向。

並不一致。

作品說明：

```text
手機：
X = 偏移軸
Y = 前進軸

自製地震儀：
方向相反
```

原因是感測器安裝方向不同。

這是一個重要的量測觀念：

```text
sensor orientation
→ axis definition
→ physical interpretation
```

---

# 四十二、第五組引擎怠速觀察

學生在靜止但發動引擎時觀察到：

```text
約 200–400 gal
```

的垂直向震動。

來源將其解釋為：

```text
引擎怠速
```

造成的震動。

---

# 四十三、第五組坑洞

遇到道路坑洞時：

```text
垂直方向有時 > 1 g
```

學生將波形中的突然大振幅與實際路面坑洞對應。

---

# 四十四、第五組橋梁伸縮縫

在中正橋經過伸縮縫時：

```text
可能出現 > 1 g 的垂直震動
```

這顯示學生已能用加速度儀辨識非常短時間的機械衝擊事件。

---

# 四十五、第五組轉彎

學生以：

- 前進軸。
- 偏移軸。

觀察轉彎過程。

例如施工便道：

```text
左轉
→ 緩慢右轉
→ 左轉
```

學生在波形中辨識：

- 前進軸速度 / 加速度變化。
- 偏移軸方向改變。

---

# 四十六、第五組煞車

其中一次煞停：

```text
前進軸加速度多為負值
```

學生估計 1 秒煞車最大量：

```text
約 -2 m/s²
```

---

# 四十七、第五組起步

其中一次起步：

```text
前進軸加速度多為正值
```

學生報告約：

```text
+1.6 m/s²
```

---

# 四十八、第五組頻譜分析

學生不只分析時域波形，也進行：

```text
frequency spectrum
```

包括：

- 垂直軸。
- 前進軸。
- 偏移軸。

學生觀察到：

> 騎車時較強的能量大多集中在高頻訊號（短週期）。

並推測可能與：

- 坑洞。
- 引擎快速轉動。
- 轉向操作。

有關。

---

# 四十九、第五組對低頻成分的態度

學生也看到：

```text
low-frequency / long-period signal
```

但明確寫道：

> 無法解釋。

這個「不過度解釋」反而是很值得保留的科學態度。

---

# 五十、第五組緩衝材料效應

學生指出：

自製地震儀下方放了：

```text
雨衣
```

當作緩衝。

因此靜止時某些振動可能被吸收。

相較之下，固定在手機架上的手機會同時受到：

```text
機車震動
+
手機架震動
```

這已經接近「安裝耦合 / installation response」的概念。

---

# 五十一、第五組結論

學生總結：

## 實驗執行

- 實際騎乘本身算成功。
- 儀器騎完後仍正常運作。

## 資料分析

較不順利：

- 自製地震儀時間錯位。
- SWARM 無法分析。
- 最後主要使用：
  - 手機備援資料。
  - Google 試算表。

---

## 主要物理觀察

1. 怠速：
   ```text
   垂直向約 200–400 gal
   ```

2. 坑洞 / 伸縮縫：
   ```text
   vertical shock > 1 g
   ```

3. 轉彎：
   ```text
   lateral-axis change
   ```

4. 煞車：
   ```text
   forward acceleration < 0
   ```

5. 起步：
   ```text
   forward acceleration > 0
   ```

---

# 五十二、第五組對自製地震儀的評價

學生指出：

```text
手機 100 Hz
> 
自製地震儀 10 Hz
```

因此手機時間解析度較高。

但：

```text
自製地震儀資料量小
→ Google Sheet 更容易處理
```

而手機：

```text
資料量大
→ 試算表負擔大
```

---

# 五十三、第五組最重要的工程反思

自製地震儀有：

```text
時間錯位
```

導致影片與波形對不上。

學生最後判斷是：

```text
程式本身
```

造成。

因此本次分析：

```text
手機為主
+
自製地震儀為輔
```

這是非常有價值的「儀器品質評估」結果。

---

# 五十四、作品 D：籃畯宏、陳佑豪 — 多級地震警報

來源：

`地震學期末報告esp32.pdf`

## 54.1 成員

- U11010017 籃畯宏
- U10910015 陳佑豪

---

# 五十五、原始設計

學生指出自製地震儀：

```text
大部分使用課堂範例程式
```

主要自行修改：

```text
earthquake classification
```

也就是：

```text
不同晃動程度
→ 不同 LED / buzzer response
```

---

# 五十六、從單一 threshold 到多級 threshold

原本：

```python
threshold = 1
```

學生改成：

```python
threshold_big = 1
threshold_small = 0.5
```

其中：

```text
threshold_big
→ 強震

threshold_small
→ 弱震
```

---

# 五十七、多級 threshold 延伸構想

學生進一步提出：

```python
threshold_big = 2
threshold_medium = 1
threshold_small = 0.5
```

即：

```text
strong
medium
small
```

三級架構。

這是本批作品中非常重要的：

```text
binary trigger
→ multi-level classification
```

進步。

---

# 五十八、if / elif 設計

學生使用：

```python
if ...
elif ...
```

進行分級。

其理解是：

1. 先判斷較大的 threshold。
2. 若沒有達到，再判斷較小門檻。

學生也指出更完整的條件應寫成類似：

```python
threshold_big > data >= threshold_small
```

避免分類範圍重疊。

---

# 五十九、弱震反應

當：

```text
data >= threshold_small
```

但未達 `threshold_big` 時：

```text
判斷為輕微搖動
```

學生設定：

- LED 閃爍。
- 不啟動蜂鳴器。
- 不記錄震動資訊。

即：

```text
small vibration
→ visual warning only
```

---

# 六十、強震反應

較強震動則沿用：

- LED。
- 蜂鳴器。
- 記錄。

可整理：

```text
Weak
→ LED only

Strong
→ LED + buzzer + recording
```

---

# 六十一、學生對更多分級的理解

學生指出：

如果有更多門檻：

```text
if 最大
elif 次大
elif 更小
...
else
```

判斷最好：

```text
由大到小
```

依序進行。

這是對條件判斷順序相當實際的理解。

---

# 六十二、分級警報可調參數

學生提出每一級都可另外設計：

- LED 頻率。
- 蜂鳴器頻率。
- 是否記錄。
- 警報方式。

例如：

```text
不同地震級別
→ 不同燈號
→ 不同警報聲
→ 不同資料記錄策略
```

---

# 六十三、此組的限制

學生指出：

- 作業時間有限。
- 只完成較輕度的修改。
- 如果時間更多，希望加入更多變化。

因此本作品較接近：

```text
multi-threshold proof-of-concept
```

而非完整震度分類系統。

---

# 六十四、相關地震學背景能力

此份報告前半部雖不是 ESP32 硬體本身，但可以看到同一組學生同時使用：

- SWARM。
- P / S picking。
- Python。
- ObsPy 類資料處理。
- PyGMT。
- PGA / PGV 型震度計算。
- 地震規模計算。
- 測站波形比較。

因此自製地震儀分級不是完全脫離地震學課程，而是與本學期其他：

```text
waveform
location
magnitude
intensity
```

內容並列在同一份期末報告中。

---

# 六十五、四個作品共同硬體架構

```text
                 NodeMCU / ESP8266
                       │
          ┌────────────┼─────────────┐
          │            │             │
          ▼            ▼             ▼
      MPU6050        LED          Buzzer
      / GY-521
          │
          ▼
      ax / ay / az
          │
          ▼
   threshold / record
          │
          ├───────────────┐
          ▼               ▼
      local alarm       TXT data
                           │
                           ▼
                     Python / Sheet
                           │
                           ▼
                   waveform / spectrum
```

---

# 六十六、2023 年 6 月共同軟體能力

學生已實際接觸：

- MicroPython。
- `machine.Pin`
- `PWM`
- `I2C`
- MPU6050。
- GPIO input。
- GPIO output。
- file write。
- Wi-Fi。
- NTP。
- timestamp。
- Python 後處理。
- SAC。
- SWARM。
- Google 試算表。
- phyphox。
- spectrogram。
- frequency spectrum。

---

# 六十七、2023 年 6 月最明顯的學習階梯

可重建為：

```text
認識微控制器
      ↓
Breadboard / wiring
      ↓
LED
      ↓
Buzzer
      ↓
Button
      ↓
MPU6050 / GY-521
      ↓
Read X/Y/Z
      ↓
Threshold
      ↓
Event recording
      ↓
Timestamp
      ↓
Waveform
      ↓
SAC
      ↓
SWARM
      ↓
Spectrum / Spectrogram
```

---

# 六十八、與同年 Raspberry Pi Pico 作業相比的重要差異

2023 年 6 月這批作品已有相當明顯的：

```text
data analysis
```

導向。

尤其：

- SAC。
- SWARM。
- Spectrogram。
- 頻譜。
- 實際機車量測。
- 手機比較。
- 取樣率比較。

因此與後續以：

```text
Pico + LED + buzzer + IoT
```

為主的作業相比，這批更接近：

> **地球物理量測實驗 + 嵌入式裝置**

而不只是「感測器警報器」。

---

# 六十九、2023 年 6 月可觀察的科學能力

## 69.1 時域分析

學生能辨識：

- 坑洞 impulsive signal。
- 引擎震動。
- 煞車負加速度。
- 起步正加速度。
- 轉彎橫向加速度。

---

## 69.2 頻域分析

第五組已使用：

```text
Spectrum
```

第三組使用：

```text
Spectrogram
```

因此已有：

```text
time domain
+
frequency domain
+
time-frequency domain
```

三種不同表現形式。

---

## 69.3 取樣率

學生直接比較：

```text
10 Hz
vs
100 Hz
```

並連結到：

- 時間解析度。
- 波形細節。
- 資料量。
- 試算表效能。

---

## 69.4 軸向與姿態

學生注意到：

```text
sensor installation direction
→ X/Y meaning changes
```

這是比單純讀三軸數字更進一步的量測理解。

---

# 七十、2023 年 6 月可觀察的工程能力

## 70.1 接線

- I2C。
- GPIO。
- 共地。
- LED。
- buzzer。
- button。

## 70.2 微控制器

- NodeMCU。
- ESP8266。
- MicroPython。

## 70.3 網路

- Wi-Fi。
- hotspot。
- IP。
- NTP。

## 70.4 資料

- txt。
- timestamp。
- sample interval。
- SAC。

## 70.5 視覺化

- waveform。
- Google Sheet chart。
- SWARM。
- spectrum。
- spectrogram。

---

# 七十一、學生作品中最值得保留的失敗經驗

## 71.1 GND 接錯

症狀：

```text
LED 不亮
buzzer 不響
```

最後發現：

```text
ground wiring error
```

---

## 71.2 蜂鳴器兩秒後不停

加入聲音強弱變化後：

```text
alarm duration logic 出現問題
```

經修改後解決。

---

## 71.3 SWARM 開不了資料

第五組：

```text
converted self-made sensor data
→ SWARM failed
```

改用：

```text
Google Sheets
```

---

## 71.4 自製地震儀 timestamp 錯位

造成：

```text
waveform
≠
video time
```

最後分析改用手機為主。

---

## 71.5 SWARM 定位 bug

另一份地震學期末報告中：

- 重啟。
- 重匯資料。
- 重開機。
- 改計算模式。
- 增加 P/S picks。
- 換較好的波形。

仍未排除定位 bug。

雖然這不是 ESP32 本身，但反映同一課程高度重視：

```text
debugging
```

---

# 七十二、2023 年 6 月作品中的設計迭代

可以看到三種非常清楚的迭代方向。

## 方向 A：警報效果

```text
ON/OFF buzzer
→ high/low alternating buzzer
→ PWM
```

---

## 方向 B：事件分類

```text
threshold
→ threshold_big + threshold_small
→ strong / medium / small concept
```

---

## 方向 C：資料分析

```text
raw X/Y/Z
→ txt
→ chart
→ SAC
→ SWARM
→ spectrum / spectrogram
```

---

# 七十三、適合後續研究編碼的欄位

| 構面 | 2023-06 可觀察證據 |
|---|---|
| Controller | NodeMCU / ESP8266 / Arduino 類平台 |
| Sensor | MPU6050 / GY-521 |
| Hardware integration | LED、buzzer、button、breadboard |
| GPIO | Digital input / output |
| I2C | SCL/SDA |
| PWM | LED duty、buzzer duty / frequency |
| Network | Wi-Fi、hotspot、IP |
| Time | NTP、UTC、UTC+8、timestamp |
| Sampling | 10 Hz；手機對照 100 Hz |
| Recording | TXT event record |
| Trigger | single threshold |
| Multi-level trigger | big / small / medium 概念 |
| Waveform | 三分量 waveform |
| Standard format | SAC |
| Seismology software | SWARM |
| Frequency analysis | Spectrum |
| Time-frequency analysis | Spectrogram |
| Mobile comparison | phyphox |
| Field experiment | 機車 5.5 km 騎乘 |
| Redundancy | 兩台手機備援 |
| Debugging | wiring、time offset、SWARM、alarm timing |
| Scientific reflection | orientation、sampling rate、buffering、installation effect |

---

# 七十四、若做 2023–2024–2026 縱向研究，2023-06 的定位

這批作品可以定義為：

> **「嵌入式地球物理量測原型期」**

因為學生已經能：

```text
sensor
→ microcontroller
→ event
→ data file
→ waveform
→ seismology analysis
```

但事件偵測仍主要使用：

```text
simple threshold
```

---

# 七十五、2023-06 → 2023 後續 Pico → 2024 → 2026 的可能能力演進

可先概念化為：

```text
2023-06
NodeMCU / ESP8266
I2C + threshold + SAC + SWARM + spectrum
        ↓

2023 後續
Raspberry Pi Pico
背景校正 + IoT + Email / LINE / IFTTT
        ↓

2024
Pico + MPU6050
bias correction + threshold + LINE
        ↓

2026
Raspberry Pi
PGA + filter + STA/LTA + FFT
+ Edge-to-Cloud
+ Discord / web visualization
```

這條線很適合用於分析學生作品中的：

- 程式能力。
- 硬體能力。
- 地震學概念。
- 資料處理。
- 量測品質。
- 工程設計。
- 科學反思。

---

# 七十六、2023 年 6 月最值得後續量化比較的幾個指標

## 1. 地震學概念

```text
只知道 shake
→ P/S concept
→ waveform
→ spectrum
```

## 2. 訊號處理

```text
raw threshold
→ time series
→ spectrum / spectrogram
```

## 3. 資料品質意識

```text
axis orientation
sampling rate
timestamp
sensor coupling
```

## 4. 工程可靠度

```text
GND
wiring
backup sensor
file compatibility
```

## 5. 系統設計

```text
single trigger
→ multi-level trigger
```

---

# 七十七、所有原始來源

## Canva

https://www.canva.com/design/DAFlSyKYy8c/4jwW0EDJcdW5F2M-dYIxDw/edit

## 附件

1. `地震儀-esp32第4組.pdf`
2. `地震儀小專題esp32.pdf`
3. `地震學期末報告esp32.pdf`

---

# 七十八、來源限制

1. 本批文件名稱使用「ESP32」，但可辨識的實際控制器名稱以 **NodeMCU / ESP8266** 為主，因此本整理特別保留兩者差異。
2. 第五組報告稱「Arduino 地震儀」，但程式使用 MicroPython `machine` / `mpu6050` 類語法；本整理保留學生原用語，不自行更正硬體型號。
3. 第四組波形頁面出現標準地震 trace 名稱，但來源未明確說明是否全部直接由自製 GY-521 轉換，因此不將其視為已完全驗證的自製儀器地震紀錄。
4. 學生作品中的 `gal`、`g`、加速度數值與地震震度之對應，本整理以來源敘述為準，不額外用外部標準校正。
5. 本文件重點為「ESP / 自製地震儀作品」，因此 `地震學期末報告esp32.pdf` 前半段 SWARM 定位、規模計算與 PyGMT 震度分析僅保留與整體課程能力有關的摘要，沒有逐頁重製。

---

# 七十九、整體摘要

2023 年 6 月這批學生作品已經超越單純的「讓 LED 亮、蜂鳴器響」。

最完整的學習鏈可以整理為：

```text
NodeMCU / ESP8266
      ↓
MPU6050 / GY-521
      ↓
三軸加速度
      ↓
Threshold / multi-threshold
      ↓
LED + buzzer
      ↓
Event recording
      ↓
NTP timestamp
      ↓
TXT
      ↓
SAC / Google Sheets
      ↓
Waveform
      ↓
SWARM
      ↓
Spectrum / Spectrogram
```

其中第五組更進一步把裝置帶到真實機車道路環境，以手機 phyphox 作 100 Hz 備援觀測，對比自製地震儀 10 Hz 資料，並從坑洞、伸縮縫、轉彎、煞車與起步事件分析三軸加速度。這使 2023 年 6 月作品成為後續 Raspberry Pi / Raspberry Pi Pico 學生作品縱向研究中非常重要的**早期技術基準**。
