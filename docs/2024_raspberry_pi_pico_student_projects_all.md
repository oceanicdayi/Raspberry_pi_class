# 2024 Raspberry Pi Pico 學生作品整理

> 整理日期：2026-08-19  
> 主題：2024 年學生 Raspberry Pi Pico／自製地震監測儀作品  
> 主要來源：使用者提供的 3 個 Canva 連結；另以本次附加的 2024 Pico 專題 PDF 與既有 2024 整理檔作補充交叉整理。  
> 整理原則：只記錄來源中可確認的內容；無法從投影片文字獨立確認的硬體、程式或結果不自行補寫。來源中出現的 Wi-Fi 密碼、Discord webhook、LINE token 等憑證不重現。

---

# 一、來源總覽

## Canva 1：地球物理學通論期末報告

原始連結：

https://www.canva.com/design/DAGTsYX402c/1IzCG9VPb9Bn8fTWD-ZmiQ/edit

可確認資訊：

- 設計標題：**地球物理學通論期末報告**
- 2024 年建立／修改的 15 頁簡報。
- 組員：
  - U11210013 莊稚晴
  - U11210025 林宥辰
  - U11111020 方妤
- 前段主題：地球物理常用繪圖工具。
- 後段明確有：
  - `Pico成果`
  - `Raspberry Pi Pico 成果`
  - 測站分佈圖
  - 規模 4.0 地震個數
  - GitHub 資料連結

GitHub notebook：

https://github.com/u11210013/Geophysics/blob/dfaa0e492bd639d33582f7d06cad89ff2927d373/finalterm/T1.ipynb

> 注意：此 Canva 的 Pico 成果頁有相當多資訊以圖片／截圖形式呈現；Canva 文字內容擷取無法完整讀出圖中的程式與硬體細節，因此本文件不推測其未被文字層揭露的內容。

---

## Canva 2：Pico 地震儀

原始連結：

https://www.canva.com/design/DAGaI6KtHH8/EjUKDBWKgf5097-2whGx3w/edit

可確認組員：

- U11110021 蔡有哲
- U11110022 林宥辰

這份作品可讀到完整的 Raspberry Pi Pico 地震儀技術內容，包括：

- Raspberry Pi Pico
- MPU6050
- MicroPython
- I2C
- 三軸加速度
- 三軸角速度
- 溫度
- 合成加速度
- 0.8 g 觸發門檻
- LED
- 蜂鳴器
- Wi-Fi
- Socket
- JSON
- Discord webhook
- Port 1333
- 本機電腦與 Pico 的發送／接收架構
- 防止重複回報的 `detected` 狀態
- I2C 錯誤處理

---

## Canva 3：地震學期末專題報告

原始連結：

https://www.canva.com/design/DAFlSyKYy8c/4jwW0EDJcdW5F2M-dYIxDw/edit

### 年份／平台核對結果

此連結**不是 2024 Raspberry Pi Pico 作品**。

作品內容明確使用：

- NodeMCU
- ESP8266
- 加速度儀
- LED
- 蜂鳴器
- 按鈕
- NTP
- SAC
- SWARM
- Spectrogram

此作品先前已屬於 **2023 年地震學課程 NodeMCU／ESP8266 自製地震儀**。

因此本文件：

- 保留此連結。
- 在附錄說明其技術內容。
- **不把它納入 2024 Raspberry Pi Pico 核心樣本。**

---

## 補充附件：2024 Pico 地球物理通論專題報告

檔名：

`2024pico地球物理通論 專題報告 最終版.pdf`

組員：

- U11210028 黃昱褘
- U11210033 吳羽捷

其 Part 2 為：

> **自製 Pico 地震監測儀**

內容與 Canva 2 的系統架構非常接近，並有完整硬體照片、網路設定、Discord 警報與實測畫面。

---

## 補充附件：既有 2024 Raspberry Pi 學生作品整理

檔名：

`2024_raspberry_pi_student_projects.md`

此檔為先前整理的 2024 Pico／MPU6050 學生作品資料，包含：

- Pico + MPU6050。
- LED / buzzer。
- Wi-Fi。
- NTP。
- LINE Notify。
- 1000 組背景值平均與 bias correction。
- `0.082 g` 門檻。
- 固定 X/Y/Z 加速度門檻。
- 網路連線與 MPU6050 偵測失敗案例。
- 成功的 LINE 即時通知案例。

此檔在本文件中作為「2024 年其他作品背景」使用，不與此次兩份可確認的 Canva Pico 專案混為同一組。

---

# 二、2024 Pico 作品快速比較

| 來源 | 組員 | 核心平台 | 感測器 | 觸發方式 | 本地警報 | 網路／雲端 | 其他特色 |
|---|---|---|---|---|---|---|---|
| Canva 1 | 莊稚晴、林宥辰、方妤 | Raspberry Pi Pico（Pico 成果頁） | 文字層未完整揭露 | 未能從文字層確認 | 未能從文字層確認 | GitHub 資料連結 | 測站分佈、M4.0 地震個數、地球物理繪圖 |
| Canva 2 | 蔡有哲、林宥辰 | Raspberry Pi Pico | MPU6050 | 合成加速度 > 0.8 g | LED + buzzer | Wi-Fi + Socket + Discord webhook | JSON、Port 1333、錯誤處理、避免重複回報 |
| 2024 Pico PDF | 黃昱褘、吳羽捷 | Raspberry Pi Pico | MPU6050 | 實測顯示總加速度與地震判定 | LED + buzzer | Wi-Fi + Discord | Pico IP／PC IP、Port 1333、實測 Discord 訊息 |
| 既有 2024 整理 | 多組 | Pico / Pico 類型 | MPU6050 | 0.082 g bias-corrected 或 X/Y/Z 固定門檻 | LED + buzzer | Wi-Fi + NTP + LINE Notify | 背景校正、離線運作構想、錯誤案例 |
| Canva 3 | 2023 第三組 | NodeMCU / ESP8266 | 加速度儀 | threshold | LED + buzzer | Wi-Fi + NTP | SAC、SWARM、Spectrogram；非 2024 Pico |

---

# 三、Canva 1：地球物理學通論期末報告

來源：

https://www.canva.com/design/DAGTsYX402c/1IzCG9VPb9Bn8fTWD-ZmiQ/edit

## 3.1 組員

- U11210013 莊稚晴
- U11210025 林宥辰
- U11111020 方妤

---

## 3.2 專題前半部：地球物理繪圖工具

來源文字指出：

> 本次專題報告將會介紹 3 個地球物理常用的繪圖工具。

可辨識到的工具／內容包括：

- PyGMT
- GMT
- ObsPy
- 繪圖結果展示

這表示本組作品不只聚焦 Pico，也把：

```text
地球物理資料
→ Python / 地球物理工具
→ 圖形化結果
```

納入同一份期末專題。

---

## 3.3 Raspberry Pi Pico 成果

後半部明確出現標題：

```text
Pico成果
Raspberry Pi Pico 成果
```

但 Canva 的可擷取文字未能讀出這些頁面中圖片所包含的：

- 接線細節。
- 程式碼。
- 感測器型號。
- 觸發門檻。
- 警報方法。

因此此組可以確認「有 Raspberry Pi Pico 成果」，但不能僅由目前可讀文字安全地填入更多硬體／演算法細節。

---

## 3.4 測站分佈

簡報列出 10 個測站：

1. SLM 關山
2. VAN 新化
3. JTS 嘉義
4. CHN 新港
5. ALS 阿里山
6. TWU 大肚山
7. SSG 杉林
8. TAO 桃園
9. WFS 五分山
10. ENTH 花蓮

並包含：

- 測站分佈圖。
- 規模 4.0 地震個數。

這顯示本組的期末專題至少把 Pico 成果與地震測站／地震資料視覺化放在同一份成果中。

> 來源文字沒有清楚說明這 10 個測站的資料是否直接由 Pico 取得，因此不把兩者強行建立因果關係。

---

## 3.5 GitHub

簡報提供 GitHub notebook：

https://github.com/u11210013/Geophysics/blob/dfaa0e492bd639d33582f7d06cad89ff2927d373/finalterm/T1.ipynb

這顯示作品使用 GitHub 保存／分享程式或 notebook。

---

# 四、Canva 2：Pico 地震儀

來源：

https://www.canva.com/design/DAGaI6KtHH8/EjUKDBWKgf5097-2whGx3w/edit

組員：

- U11110021 蔡有哲
- U11110022 林宥辰

---

# 五、硬體架構

來源明確寫有：

```text
Pico 與 MPU6050 連接
```

主要元件：

- Raspberry Pi Pico
- MPU6050
- LED
- 蜂鳴器
- 麵包板／接線
- 電腦
- Wi-Fi 網路

可整理為：

```text
MPU6050
   │ I2C
   ▼
Raspberry Pi Pico
   │
   ├── LED
   ├── Buzzer
   ├── Wi-Fi / network
   └── Socket / Discord
```

---

# 六、I2C 與 MPU6050

來源程式：

```python
i2c = I2C(
    0,
    scl=Pin(5),
    sda=Pin(4),
    freq=400000
)
imu = MPU6050(i2c)
```

因此：

| 功能 | 設定 |
|---|---|
| I2C bus | 0 |
| SCL | Pin 5 |
| SDA | Pin 4 |
| Frequency | 400000 Hz |
| IMU | MPU6050 |

---

# 七、GPIO 輸出

來源可讀程式包括：

```python
led = Pin(0, Pin.OUT)
buzzer = Pin(15, Pin.OUT)
```

因此：

- LED：GPIO 0。
- 蜂鳴器：GPIO 15。

---

# 八、感測資料

程式讀取：

```python
ax = imu.accel.x
ay = imu.accel.y
az = imu.accel.z

gx = imu.gyro.x
gy = imu.gyro.y
gz = imu.gyro.z

temperature = imu.temperature
```

也就是同時取得：

- X/Y/Z acceleration。
- X/Y/Z angular velocity。
- Temperature。

---

# 九、合成加速度

此組沒有只使用單軸門檻，而計算：

```python
total_accel = math.sqrt(
    ax ** 2 +
    ay ** 2 +
    az ** 2
)
```

也就是三軸加速度向量大小：

```text
ax, ay, az
   ↓
sqrt(ax² + ay² + az²)
   ↓
total acceleration
```

這比單純：

```text
ax > threshold
```

更接近「三軸合成量」的判斷方式。

---

# 十、地震觸發門檻

程式使用：

```python
if total_accel > 0.8:
```

因此此組的地震偵測規則可整理成：

```text
MPU6050
   ↓
ax, ay, az
   ↓
total_accel
   ↓
total_accel > 0.8 g ?
   ↓ yes
earthquake alert
```

來源把它作為自製地震監測儀的觸發條件；本文件不將 `0.8 g` 額外換算成官方震度。

---

# 十一、觸發後的本地警報

當偵測到事件：

```text
確認地震發生
```

並：

```text
啟動蜂鳴器
開啟 LED
```

因此具有兩種本地提示：

- 聲音。
- 光。

---

# 十二、防止重複回報

來源提到：

```python
detected = True
```

用途是：

> 防止同一次持續搖晃造成重複發送警報。

這是一個比單純門檻式警報更進一步的事件狀態概念：

```text
threshold crossing
→ detected = True
→ avoid repeated alerts
```

---

# 十三、監測時間間隔

來源說明：

```python
detect_earthquake(0.2)
```

代表：

```text
每 0.2 秒重複進行一次地震偵測
```

即約：

```text
5 次 / 秒
```

的偵測呼叫頻率。

> 這是程式呼叫間隔，不能直接等同 MPU6050 真正內部取樣率。

---

# 十四、Wi-Fi

程式使用：

```python
import network

wlan = network.WLAN(network.STA_IF)
wlan.active(True)
wlan.connect(...)
```

連線後：

```python
print(
    'Connected! IP address:',
    wlan.ifconfig()[0]
)
```

因此學生理解並使用：

```text
Pico
↓
Wi-Fi hotspot
↓
取得 IP
```

原 Canva 中包含實際熱點名稱／密碼，本整理刻意移除憑證。

---

# 十五、Pico 與電腦的網路角色

來源有：

```text
Pico IP（發送者）
本機 IP（訂閱者）
```

以及「轉送規則設定」。

也就是學生把系統拆為：

```text
Pico / sender
      ↓
network
      ↓
PC / receiver
```

這比單一裝置直接呼叫 webhook 更接近簡單的 edge-to-host 架構。

---

# 十六、Socket

接收端程式可讀到：

```python
HOST = '0.0.0.0'
PORT = 1333
```

並具有：

```text
bind
listen
accept
recv
```

流程：

```text
Pico
   ↓
TCP / socket
   ↓
PC server
   ↓
receive sensor / event data
```

---

# 十七、Port 1333

作品在網路設定中使用：

```text
1333
```

作為通訊 port。

同時設定：

- 本機 IPv4。
- Pico IP。
- forwarding rule。

這是 2024 作品中相對明顯的網路工程元素。

---

# 十八、JSON

接收端使用：

```python
ujson.loads(data)
```

表示 Pico 與接收端不是只傳一個純文字警報，而有使用結構化資料。

可概念化：

```json
{
  "ax": "...",
  "ay": "...",
  "az": "...",
  "gx": "...",
  "gy": "...",
  "gz": "...",
  "temperature": "..."
}
```

來源文字可確認的是使用 JSON 解碼；欄位示意依其後續輸出的量測值整理。

---

# 十九、Discord

作品使用 Discord webhook。

警報 payload 中包含：

```python
payload = {
    'content': message,
    'username': "Spidey Bot",
}
```

Discord 接收到的警報訊息可看到：

```text
Earthquake!Earthquake!
```

---

# 二十、Discord 的警報流程

完整流程可整理為：

```text
MPU6050
   ↓
total_accel
   ↓
> 0.8 g
   ↓
Pico confirms earthquake
   ↓
LED + buzzer
   ↓
network / socket
   ↓
PC / forwarding
   ↓
Discord webhook
   ↓
Earthquake!Earthquake!
```

---

# 二十一、錯誤處理

來源程式包含：

```python
except OSError:
```

並可看到：

```text
I2C failure when communicating with IMU
```

這表示學生不只展示成功結果，也處理：

```text
Pico ↔ MPU6050 communication failure
```

的情境。

---

# 二十二、實時輸出

來源展示的即時資訊包括：

```text
ax
ay
az
gx
gy
gz
Temperature
total acceleration
```

因此開發階段可直接從 console 觀察：

```text
感測值
→ 合成加速度
→ 是否觸發
→ 警報是否傳送成功
```

---

# 二十三、Canva 2 的系統能力摘要

此組完成的整體鏈條：

```text
MPU6050
→ I2C
→ Raspberry Pi Pico
→ three-axis acceleration
→ total acceleration
→ 0.8 g threshold
→ state control
→ LED / buzzer
→ Wi-Fi
→ Socket
→ JSON
→ PC
→ Discord
```

相較 2024 年較早期的：

```text
Pico + fixed threshold + LINE Notify
```

此作品多出了：

- 三軸合成量。
- sender / receiver。
- Socket。
- Port。
- JSON。
- Discord。
- 重複警報控制。
- I2C exception handling。

---

# 二十四、補充附件：黃昱褘、吳羽捷的 Pico 地震監測儀

來源：

`2024pico地球物理通論 專題報告 最終版.pdf`

組員：

- U11210028 黃昱褘
- U11210033 吳羽捷

---

# 二十五、Part 1：程式與協作工具

報告前半部不是 Pico 硬體本身，但呈現同一課程的資訊能力培養：

- VS Code。
- GitHub。
- Jupyter Notebook。
- Markdown。
- Django。
- PyGMT。
- ObsPy。

並實作：

```text
VS Code
↔ GitHub
↔ Jupyter
```

以及使用 Django 網頁呈現：

- VS Code 介紹。
- GitHub 介紹。
- PyGMT 台灣地圖。
- ObsPy 地震資料分析。

---

# 二十六、Part 2：自製 Pico 地震監測儀

報告第 16 頁開始：

> **自製 Pico 地震監測儀**

---

# 二十七、網路與轉送規則

第 17 頁明確區分：

```text
本機 IP（訂閱者）
Pico IP（發送者）
```

並設定網路轉送。

可視為：

```text
Pico sender
    ↓
local network
    ↓
PC subscriber / receiver
```

---

# 二十八、Pico Wi-Fi

第 18 頁的 MicroPython 程式：

```python
import network

wlan = network.WLAN(network.STA_IF)
wlan.active(True)
wlan.connect(...)
```

成功連線後回傳：

```text
Connected! IP address: ...
```

原投影片中可見實際熱點密碼，本整理不重現。

---

# 二十九、硬體接線

第 19 頁：

- Raspberry Pi Pico。
- MPU6050。
- 蜂鳴器。
- 麵包板。

第 20 頁：

- Pico。
- MPU6050。
- LED。

投影片同時提供：

- 實際裝置照片。
- 麵包板接線示意。

---

# 三十、製造震動與警報測試

第 21 頁：

> **製造地震及蜂鳴器開啟**

畫面顯示：

- 操作者搖動／測試裝置。
- 電腦執行程式。
- Discord 畫面同步觀察。

---

# 三十一、實測感測資料

第 22 頁 console 顯示例如：

```text
ax: -0.18 g
ay: -0.41 g
az: 0.49 g

gx: -1.15 °/s
gy: -1.21 °/s
gz: 1.79 °/s

Temperature: 24.88 °C
總加速度: 0.67 g
```

另一次可看到：

```text
ax: -0.14 g
ay: -0.38 g
az: 0.52 g

Temperature: 24.84 °C
總加速度: 0.66 g
```

---

# 三十二、地震觸發與通知

觸發畫面中可見：

```text
⚠️ 確認地震發生！
成功發送通知到 Discord。
啟動蜂鳴器！
```

這是直接的成功操作證據。

---

# 三十三、I2C 失敗也被保留

同一頁亦出現：

```text
讀取數據時出錯：
I2C failure when communicating with IMU
```

之後仍有量測值，例如：

```text
ax: 0.78 g
ay: 0.41 g
az: 0.46 g
...
Temperature: 25.45 °C
總加速度: 1.00 g
```

因此報告同時保留：

- 成功地震偵測。
- Discord 成功。
- 蜂鳴器成功。
- I2C 通訊錯誤。

這對後續做「學生 debugging 能力」分析很有價值。

---

# 三十四、Discord 成果

第 23 頁：

> **discord 接收到地震資訊**

Discord 中可看到 bot：

```text
Spidey Bot
```

以及多筆：

```text
Earthquake!Earthquake!
```

證明事件可從 Pico 系統送到 Discord。

---

# 三十五、LED 成果

第 24 頁：

> **接收到地震警報後開燈**

console 顯示：

```text
確認地震發生
成功發送通知到 Discord
啟動蜂鳴器
開燈
```

並附 LED 實際亮起照片。

因此此組至少完成：

```text
sensor
→ earthquake trigger
→ buzzer
→ LED
→ Discord
```

的完整操作鏈。

---

# 三十六、2024 既有作品補充：LINE Notify 型 Pico 地震儀

依附加的既有整理檔，2024 年其他學生作品另有一條常見架構：

```text
Raspberry Pi Pico
→ MPU6050
→ threshold
→ LED / buzzer
→ Wi-Fi
→ NTP
→ LINE Notify
```

---

# 三十七、背景值與 bias correction

其中一份作品先蒐集：

```text
1000 組背景資料
```

計算三軸加速度平均：

```text
axbias
aybias
azbias
```

再比較：

```text
current acceleration - bias
```

以：

```text
±0.082 g
```

作觸發門檻。

這代表學生已開始處理：

```text
sensor baseline
→ calibration
→ relative change
```

而非只讀原始值。

---

# 三十八、固定三軸門檻型作品

另一些 2024 作品使用：

```python
ax > 2
or ax < -2
or ay > 1
or ay < -1
or az > 1
or az < -1
```

即：

| 軸 | 門檻 |
|---|---|
| X | ±2 |
| Y | ±1 |
| Z | ±1 |

觸發後：

- LED。
- buzzer。
- NTP 時間。
- LINE Notify。
- 約 5 秒後關閉本地警報。

---

# 三十九、離線運作反思

既有整理中的學生提出：

> 即使 Wi-Fi 失敗，本地地震偵測與蜂鳴器仍應繼續工作。

可整理為：

```text
Edge detection
≠
Cloud notification
```

也就是：

```text
sensor + local alert
```

不應完全依賴：

```text
network + LINE
```

這是很重要的系統可靠度思考。

---

# 四十、2024 年可辨識的三條技術路徑

## 路徑 A：固定門檻 + LINE

```text
MPU6050
→ X/Y/Z threshold
→ LED / buzzer
→ LINE Notify
```

---

## 路徑 B：背景校正 + LINE

```text
1000 background samples
→ bias
→ current - bias
→ 0.082 g threshold
→ LINE Notify
```

---

## 路徑 C：合成加速度 + Discord

```text
MPU6050
→ ax/ay/az
→ sqrt(ax²+ay²+az²)
→ 0.8 g threshold
→ detected state
→ LED / buzzer
→ Socket / JSON
→ Discord
```

---

# 四十一、2024 年作品的能力進展

若把 2024 年不同作品放在同一年內比較，可看到事件判斷逐漸從：

```text
single-axis / per-axis fixed threshold
```

發展到：

```text
bias-corrected threshold
```

以及：

```text
vector magnitude threshold
```

同時網路輸出也從：

```text
LINE Notify
```

延伸到：

```text
Pico sender
→ PC receiver
→ Socket
→ JSON
→ Discord webhook
```

---

# 四十二、2024 年 Raspberry Pi Pico 技術能力矩陣

| 構面 | 可觀察內容 |
|---|---|
| 微控制器 | Raspberry Pi Pico |
| 感測器 | MPU6050 |
| 通訊 | I2C |
| Acceleration | X/Y/Z |
| Gyroscope | X/Y/Z |
| Temperature | 有 |
| Calibration | 部分作品有 1000 組背景 bias |
| Trigger | 固定軸門檻、bias-corrected、total acceleration |
| Total acceleration | `sqrt(ax²+ay²+az²)` |
| Local alert | LED、buzzer |
| Wi-Fi | 有 |
| Time | 部分作品使用 NTP / RTC |
| Cloud messaging | LINE Notify、Discord |
| Socket | 有 |
| JSON | 有 |
| Port / networking | Port 1333 |
| Sender/receiver | Pico 發送端、PC 接收端 |
| Error handling | Wi-Fi、MPU detection、I2C OSError |
| State control | `detected` 防止重複警報 |
| Development tools | Thonny / VS Code / Jupyter |
| Version control | GitHub |
| Web | Django |
| Geophysics tools | PyGMT / GMT / ObsPy |

---

# 四十三、2024 年最值得保留的 debugging 證據

## 43.1 MPU6050 無法偵測

既有 2024 作品中曾出現：

```text
No MPU's detected
```

---

## 43.2 網路連線失敗

有學生因：

```text
電腦無法連手機網路
```

導致系統未成功執行。

---

## 43.3 I2C failure

本次 Pico / Discord 報告中可見：

```text
I2C failure when communicating with IMU
```

---

## 43.4 網路與偵測解耦

學生開始提出：

```text
No Wi-Fi
→ still detect
→ still buzzer
→ only cloud message fails
```

這表示作品已從「做得出來」逐漸朝：

```text
system robustness
```

思考。

---

# 四十四、與 2023 年的技術差異

2023 NodeMCU／ESP8266 作品已有：

- MPU6050 / 加速度感測。
- threshold。
- Wi-Fi。
- NTP。
- SAC。
- SWARM。
- Spectrogram。

2024 Pico 作品則更集中在：

- Raspberry Pi Pico。
- Edge trigger。
- 即時 IoT 警報。
- LINE / Discord。
- sender / receiver。
- JSON。
- Socket。
- 網路錯誤與裝置可靠度。

因此可以初步把演進整理為：

```text
2023
embedded seismometer
+ waveform / SAC / SWARM
        ↓

2024
Raspberry Pi Pico
+ IoT alert
+ calibration
+ networking
+ edge-to-host communication
```

---

# 四十五、第三個 Canva 連結的排除說明

來源：

https://www.canva.com/design/DAFlSyKYy8c/4jwW0EDJcdW5F2M-dYIxDw/edit

此作品使用：

```text
NodeMCU
ESP8266
D1 / D2
LED
Buzzer
NTP
```

並做：

```text
TXT
→ SAC
→ SWARM
→ Spectrogram
```

它非常有價值，但應歸在：

> **2023 年 NodeMCU／ESP8266 自製地震儀**

而不是：

> **2024 年 Raspberry Pi Pico**

因此若之後要進行跨年度縱向分析，建議標記：

```yaml
year: 2023
controller: NodeMCU / ESP8266
include_in_2024_pico_dataset: false
```

---

# 四十六、適合後續縱向研究的編碼欄位

```yaml
year:
course:
team:
controller:
sensor:
i2c:
accel_xyz:
gyro_xyz:
temperature:
calibration:
bias_correction:
trigger_type:
trigger_threshold:
vector_magnitude:
sampling_or_loop_interval:
local_led:
local_buzzer:
wifi:
ntp:
line_notify:
discord:
socket:
json:
port:
sender_receiver:
data_logging:
github:
web_output:
geophysics_analysis:
error_handling:
debugging_evidence:
reflection:
```

---

# 四十七、2024 年整體定位

綜合此次 Canva 與附件，可將 2024 Raspberry Pi Pico 學生作品描述為：

> **「Pico 感測與 IoT 地震警報整合期」**

核心學習鏈為：

```text
Raspberry Pi Pico
        ↓
MPU6050
        ↓
I2C
        ↓
ax / ay / az
        ↓
fixed threshold
or bias correction
or total acceleration
        ↓
earthquake detection
        ↓
LED + buzzer
        ↓
Wi-Fi
        ↓
LINE / Socket / Discord
        ↓
remote notification
```

這批作品已開始出現：

- 感測器校正。
- 合成加速度。
- 本地與網路警報。
- Edge / host 分工。
- Socket。
- JSON。
- Discord webhook。
- 防重複事件狀態。
- I2C exception handling。
- GitHub 與網頁呈現。

因此很適合作為後續 2026 Raspberry Pi 作品中：

```text
filter
PGA
STA/LTA
FFT
web dashboard
edge-to-cloud
```

等進階功能的前期基準。

---

# 四十八、所有原始連結

## 2024 Canva：地球物理學通論期末報告

https://www.canva.com/design/DAGTsYX402c/1IzCG9VPb9Bn8fTWD-ZmiQ/edit

## 2024 Canva：Pico 地震儀

https://www.canva.com/design/DAGaI6KtHH8/EjUKDBWKgf5097-2whGx3w/edit

## 跨年度來源：2023 NodeMCU / ESP8266

https://www.canva.com/design/DAFlSyKYy8c/4jwW0EDJcdW5F2M-dYIxDw/edit

## Canva 1 提供的 GitHub Notebook

https://github.com/u11210013/Geophysics/blob/dfaa0e492bd639d33582f7d06cad89ff2927d373/finalterm/T1.ipynb

---

# 四十九、資料限制與安全處理

1. Canva 1 的 Raspberry Pi Pico 成果有部分內容存在圖片／截圖中，文字擷取無法還原全部程式與線路資訊，因此未自行推測。
2. Canva 2 與 PDF 的原始頁面含 Wi-Fi 熱點密碼、網路設定及 webhook 相關資訊；本整理只保留技術用途，不重現實際憑證。
3. 第三個 Canva 連結經內容核對為 2023 NodeMCU / ESP8266 作品，故不納入 2024 Pico 核心樣本。
4. `0.8 g`、`0.082 g` 與固定 X/Y/Z 門檻均依學生原始作品記錄，本整理不替其換算成官方地震震度。
5. 不同作品可能使用相似課堂範例程式；本文件只描述各來源呈現的功能，不判定其程式來源或獨創程度。
