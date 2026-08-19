# 2024 Raspberry Pi 地震儀學生作品整理

> 整理日期：2026-08-19  
> 整理範圍：依使用者提供之 Canva 連結，以及 2 份 PDF 與 1 份 PPTX 附件，擷取其中與 Raspberry Pi / Raspberry Pi Pico、MPU6050、震動偵測、警報、Wi-Fi、LINE Notify、資料校正與實作成果直接相關的內容。  
> 原則：以學生原始作品呈現為準，不自行補寫未出現在來源中的結果。原始投影片中若含 Wi-Fi 密碼、LINE Notify token 等敏感資訊，本整理僅說明其用途，不重現實際憑證。

---

# 一、來源一覽

## 1. Canva 連結

原始連結：

https://www.canva.com/design/DAGF3Lha98o/R1LTgOy1nWFA9UoKr-A-Ww/edit

### 存取狀態

本次整理過程中，Canva 編輯連結無法由公開網頁擷取器直接讀取設計內容，因此**目前無法獨立驗證其頁面文字、圖片與程式碼內容**。

因此本文件：

- 保留 Canva 原始連結。
- 不猜測 Canva 內容是否與附件中的 PDF / PPTX 重複。
- 不把附件內容誤標為 Canva 內容。
- 若之後取得 Canva 設計的可讀內容，可再補入本文件。

---

## 2. 附件：`地震儀模擬實作練習題.pdf`

- 共 9 頁。
- 主題：地震儀模擬實作練習題。
- 核心：Raspberry Pi Pico 類型開發板 + MPU6050 + Wi-Fi + LINE Notify + LED / 蜂鳴器。
- 重點：先量測背景值，再以背景值校正後的加速度差異判斷是否超過門檻。

---

## 3. 附件：`地震儀實作.pdf`

- 共 10 頁。
- 第 4 組。
- 成員：
  - 林憶蓁
  - 莊博文
  - 葉宥陞
  - 黃加榕
  - 姚宥安
- 核心：Raspberry Pi Pico + MPU6050 + LED + 蜂鳴器 + Wi-Fi + LINE Notify。
- 結果：作品報告指出因網路連線問題，系統未能正常運作；錯誤截圖另顯示 MPU6050 未被偵測的訊息。

---

## 4. 附件：`地震儀.pptx`

- 共 11 張投影片。
- 第六組。
- 成員：
  - 王云柔
  - 謝語姍
- 核心：Raspberry Pi Pico + MPU6050 + LED + 蜂鳴器 + Wi-Fi + LINE Notify。
- 結果：成功完成搖晃測試，LED、蜂鳴器與 LINE Notify 可在達門檻後觸發。

---

# 二、2024 作品快速比較

| 來源 | 組別 / 作者 | 控制板 | 感測器 | 偵測方法 | 警報 | 網路功能 | 實作結果 |
|---|---|---|---|---|---|---|---|
| 地震儀模擬實作練習題 PDF | 未在檔案首頁標示 | Pico 系列 | MPU6050 | 1000 組背景值校正後，以任一軸 ±0.082 g 為門檻 | LED / 提示音 | LINE Notify | 有 LINE 警報截圖，並提出離線模式與分級警報改進 |
| 地震儀實作 PDF | 第 4 組 | Raspberry Pi Pico | MPU6050 | 三軸加速度固定門檻：X ±2、Y ±1、Z ±1 | LED + 蜂鳴器 5 秒 | LINE Notify | 報告稱因手機網路問題未成功；錯誤截圖另見 MPU 未偵測 |
| 地震儀 PPTX | 第六組 王云柔、謝語姍 | Raspberry Pi Pico | MPU6050 | 三軸加速度固定門檻：X ±2、Y ±1、Z ±1 | LED + 蜂鳴器 5 秒 | LINE Notify | 成功搖晃觸發，LINE 即時通知，Thonny 顯示感測資料 |
| Canva | 尚無法獨立讀取 | — | — | — | — | — | 待補 |

---

# 三、作品 A：地震儀模擬實作練習題

來源：`地震儀模擬實作練習題.pdf`

## 3.1 專案核心概念

作品以 Raspberry Pi Pico 類型的 MicroPython 環境建立簡易地震偵測與警報系統。

整體概念可整理為：

```text
MPU6050
   ↓
讀取三軸加速度 / 陀螺儀
   ↓
先量測背景值
   ↓
目前值 - 背景值
   ↓
與 0.082 g 門檻比較
   ↓
超過門檻
   ↓
LED / 提示音 + LINE Notify
```

作品第 4 頁以流程圖表示：

```text
連接 Wi-Fi
    ↓
監測加速度
    ↓
當加速度超過 0.082 g（作品標示為四級）
    ↓
發送警報並發出提示音
```

---

## 3.2 硬體與軟體環境

### 控制與感測

程式使用 MicroPython：

```python
from machine import I2C, Pin
from imu import MPU6050
```

I2C 設定：

```python
i2c = I2C(
    0,
    scl=Pin(5),
    sda=Pin(4),
    freq=400000
)
imu = MPU6050(i2c)
```

可確認作品使用：

- Pico 類型開發板。
- MPU6050 IMU。
- I2C。
- GPIO。
- LED。
- 蜂鳴器 / 提示音裝置。
- Wi-Fi。
- LINE Notify。

由於來源只寫「pico」，未明確標示 Pico 或 Pico W 型號；但程式使用 Wi-Fi，因此實作環境具無線網路能力。

---

# 四、背景值量測與校正

## 4.1 蒐集 1000 組資料

作品先讓 MPU6050 在背景環境下持續量測：

- X、Y、Z 三軸加速度。
- X、Y、Z 三軸角速度。
- 溫度。

資料量：

```python
num_samples = 1000
```

每次資料之間：

```python
time.sleep(0.5)
```

之後計算各欄位平均值。

這表示學生並非直接將感測器原始值拿來判斷，而是先建立「背景值 / bias」。

---

## 4.2 實際背景平均值

PDF 第 3 頁列出的平均結果為：

| 量測項目 | 平均值 |
|---|---:|
| Average ax | 1.060225 |
| Average ay | 0.008022461 |
| Average az | 0.1139915 |
| Average gx | 2.642458 |
| Average gy | 0.364084 |
| Average gz | -0.2899008 |
| Average Temperature | 31.35 |

後續程式將前三個加速度背景平均值寫入：

```python
axbias = 1.060225
aybias = 0.008022461
azbias = 0.1139915
```

用途是比較：

```text
目前量測值 - 背景值
```

而不是單純比較原始加速度。

這是 2024 附件中相對明確的「背景校正」設計。

---

# 五、Wi-Fi、網路時間與 LINE Notify

## 5.1 Wi-Fi

程式使用：

```python
import network
```

並建立：

```python
network.WLAN(network.STA_IF)
```

啟動後連接 Wi-Fi，連線成功時印出 IP 位址。

原作品中包含實際 Wi-Fi SSID / 密碼；本整理不重現。

---

## 5.2 NTP 網路校時

程式使用：

```python
import ntptime
from machine import RTC
```

流程包括：

1. 連上 Wi-Fi。
2. 使用 NTP 同步時間。
3. 讀取 RTC。
4. 將 UTC 時間加 8 小時。
5. 輸出格式：

```text
YYYY/MM/DD HH:MM:ss
```

此設計的目的是讓每筆地震警報都有時間標記。

---

## 5.3 LINE Notify

程式使用：

```python
import urequests
```

並建立 `lineNotifyMessage(token, msg)` 函式。

訊息透過 HTTP POST 發送到 LINE Notify。

原始程式中含實際 token；本整理不重現。

---

# 六、地震觸發邏輯

作品的核心門檻為：

```text
0.082 g
```

判斷不是直接使用原始值，而是比較每一軸與背景值的差：

```python
ax - axbias
ay - aybias
az - azbias
```

只要任一軸滿足：

```text
> +0.082
```

或：

```text
< -0.082
```

就判定達到觸發條件。

可概念化為：

```python
if abs(ax - axbias) > 0.082 \
or abs(ay - aybias) > 0.082 \
or abs(az - azbias) > 0.082:
    trigger_alert()
```

學生作品將 `0.082 g` 標示為「四級」門檻。

---

# 七、警報輸出

當超過門檻時，作品描述會：

- 開啟 LED。
- 發出提示音 / 蜂鳴器。
- 取得時間。
- 傳送 LINE Notify 訊息。
- 約 5 秒後關閉 LED。

警報訊息範例文字：

```text
Earthquake occurred!!
Intensity is more then 4
```

PDF 第 8 頁展示 LINE Notify 成功傳送截圖，日期包含：

- 2024/05/22 00:13:10
- 2024/05/22 19:25:33

顯示系統確實曾完成訊息傳送測試。

---

# 八、作品 A 的自我改進構想

PDF 第 9 頁列出兩項明確改進。

## 8.1 離線偵測

原系統：

```text
必須先連接網路
    ↓
才能進入完整運作流程
```

學生提出：

即使沒有網路，也應：

- 繼續讀取 MPU6050。
- 繼續偵測震動。
- 超過門檻時仍啟動蜂鳴器。

只有：

- LINE Notify 無法發送。

這可理解為把：

```text
本地地震偵測
```

與：

```text
雲端 / 網路通知
```

拆成兩個相對獨立的功能。

---

## 8.2 分級訊息

學生也提出：

- 震度 > 3：發送第一種警告。
- 震度 > 4：再發送另一則較高等級警告。

也就是從單一門檻進一步發展：

```text
多級門檻 → 不同警報訊息
```

---

# 九、作品 B：第 4 組地震儀實作

來源：`地震儀實作.pdf`

## 9.1 組員

第 4 組：

- 林憶蓁
- 莊博文
- 葉宥陞
- 黃加榕
- 姚宥安

---

# 十、作品 B 的系統架構

從投影片與裝置照片可辨識：

- Raspberry Pi Pico。
- MPU6050。
- 麵包板。
- LED。
- 蜂鳴器。
- 杜邦線。
- MicroPython / Thonny。
- Wi-Fi。
- LINE Notify。

裝置照片顯示 Pico 與 MPU6050、LED、蜂鳴器皆安裝於麵包板。

---

# 十一、作品 B 的程式結構

## 11.1 使用模組

作品逐項說明：

### `network`
處理網路連接。

### `I2C` 與 `Pin`
控制：

- I2C 通訊。
- GPIO 針腳。

### `RTC`
實時時鐘。

### `time`
時間操作。

### `ntptime`
從網路取得時間。

### `urequests`
發送 HTTP request。

### `MPU6050`
IMU 感測器。

---

## 11.2 GPIO

作品程式設定：

```python
led = Pin(1, Pin.OUT)
buzzer = Pin(15, Pin.OUT)
```

因此：

- LED：GPIO 1。
- 蜂鳴器：GPIO 15。

---

## 11.3 I2C / MPU6050

```python
i2c = I2C(
    0,
    scl=Pin(5),
    sda=Pin(4),
    freq=400000
)
imu = MPU6050(i2c)
```

同樣使用：

- I2C 0。
- SCL：Pin 5。
- SDA：Pin 4。
- 400 kHz。
- MPU6050。

---

# 十二、作品 B 的背景資料程式

第 2 頁出現與作品 A 類似的背景平均程式：

```python
num_samples = 1000
```

持續累加：

- ax
- ay
- az
- gx
- gy
- gz
- temperature

最後計算平均。

但本份 PDF **沒有展示最後算出的背景平均數值，也沒有在後續觸發條件中明確使用這些 bias**。

因此可以確認學生有進行背景資料程式設計，但不能從來源判定其最終警報邏輯實際採用了背景校正。

---

# 十三、作品 B 的 LINE Notify

建立：

```python
lineNotifyMessage(token, msg)
```

HTTP request 包含：

- Bearer token。
- `application/x-www-form-urlencoded`。
- message payload。

用途：

```text
震動觸發
    ↓
產生警報文字
    ↓
LINE Notify
```

原作品包含 token，本整理省略。

---

# 十四、作品 B 的 Wi-Fi 與時間

## Wi-Fi

```python
network.WLAN(network.STA_IF)
```

連線成功後：

```text
顯示 IP address
```

## 時間

使用：

```python
ntptime.settime()
RTC()
```

再把日期與時間格式化後回傳。

---

# 十五、作品 B 的即時感測

主迴圈不斷讀取：

```python
ax = imu.accel.x
ay = imu.accel.y
az = imu.accel.z

gx = imu.gyro.x
gy = imu.gyro.y
gz = imu.gyro.z

temperature = imu.temperature
```

並把資料印到 Thonny / terminal。

更新頻率由：

```python
time.sleep(1)
```

控制。

---

# 十六、作品 B 的觸發門檻

主程式門檻為：

```python
if ax > 2 \
or ay > 1 \
or az > 1 \
or ax < -2 \
or ay < -1 \
or az < -1:
```

因此：

| 軸 | 正門檻 | 負門檻 |
|---|---:|---:|
| X | +2 | -2 |
| Y | +1 | -1 |
| Z | +1 | -1 |

來源沒有將這些數值再轉換為正式地震震度等級，因此本整理只稱其為「加速度觸發門檻」。

---

# 十七、作品 B 的警報流程

達門檻後：

```text
LED ON
  +
Buzzer ON
  +
取得時間
  +
建立警報訊息
  +
LINE Notify
  ↓
等待 5 秒
  ↓
LED OFF
Buzzer OFF
```

LINE 訊息包含：

- 事件時間。
- X、Y、Z 加速度。

---

# 十八、作品 B 的實作結果

報告第 9 頁寫道：

> 因為不明原因，電腦無法連接手機網路，導致程式碼及地震儀都無法運作。

因此依學生文字敘述，本組最後未完成成功運作。

### 來源中另一項值得保留的現象

同頁 Thonny 錯誤截圖實際顯示：

```text
MPUException: No MPU's detected
```

也就是說：

- 報告文字將失敗原因歸於手機網路連線。
- 錯誤畫面同時顯示 MPU6050 感測器未被偵測。

本整理不替學生判定哪一個才是真正根因，只保留兩個來源中都存在的資訊。

---

# 十九、作品 B 的組員貢獻度

原報告列出：

| 組員 | 工作 | 貢獻度 |
|---|---|---:|
| 林憶蓁 | 裝置地震儀 | 20% |
| 莊博文 | 裝置地震儀、操作程式碼 | 65% |
| 葉宥陞 | 參與討論 | 5% |
| 黃加榕 | 參與討論 | 5% |
| 姚宥安 | 參與討論 | 5% |

總計：100%。

---

# 二十、作品 C：第六組地震儀模擬實作

來源：`地震儀.pptx`

## 20.1 組員

- 王云柔
- 謝語姍

---

# 二十一、作品 C 的系統架構

這份作品與第 4 組使用非常接近的技術架構：

```text
Raspberry Pi Pico
   ↓
MPU6050
   ↓
I2C
   ↓
持續讀取加速度與陀螺儀
   ↓
固定加速度門檻
   ↓
LED + 蜂鳴器
   ↓
LINE Notify
```

---

# 二十二、作品 C 使用的軟體模組

投影片第 2 張列出：

- `network`
- `I2C`
- `Pin`
- `RTC`
- `time`
- `ntptime`
- `urequests`
- `MPU6050`

並說明用途：

### network
網路連線。

### I2C / Pin
感測器通訊與 GPIO。

### RTC
實時時鐘。

### ntptime
網路同步時間。

### urequests
HTTP request。

### MPU6050
IMU 感測器。

---

# 二十三、作品 C 的輸出裝置

程式設置：

- LED。
- 蜂鳴器。

並將兩者設為 GPIO output。

裝置照片顯示：

- Raspberry Pi Pico。
- 麵包板。
- MPU6050。
- LED。
- 蜂鳴器。
- 杜邦線。

---

# 二十四、作品 C 的 Wi-Fi 與 LINE Notify

## Wi-Fi

程式：

```text
啟動 WLAN
→ 連線到 Wi-Fi
→ 等待連線
→ 顯示 IP
```

## LINE Notify

以 Bearer token 認證，透過 HTTP POST 傳送訊息。

原始 PPTX 中包含憑證設定，本整理不重現 token 或密碼。

---

# 二十五、作品 C 的時間同步

流程：

```text
Wi-Fi
  ↓
NTP
  ↓
RTC
  ↓
格式化日期與時間
  ↓
加入警報訊息
```

目的在於讓 LINE 警報具有事件時間。

---

# 二十六、作品 C 的感測器資料

程式持續讀取：

- ax
- ay
- az
- gx
- gy
- gz
- temperature

Thonny 實際畫面中可看到例如：

```text
Temperature: 29.12
ax: 1.10083
ay: -0.1347656
az: 0.2592773
gx: -10.81679
gy: 21.87023
```

這代表學生除了使用加速度觸發，也有把：

- 陀螺儀。
- 溫度。

輸出到開發環境。

---

# 二十七、作品 C 的觸發門檻

投影片中的程式為：

```python
if ax > 2 \
or ay > 1 \
or az > 1 \
or ax < -2 \
or ay < -1 \
or az < -1:
```

因此與第 4 組相同：

| 軸 | 正門檻 | 負門檻 |
|---|---:|---:|
| X | +2 | -2 |
| Y | +1 | -1 |
| Z | +1 | -1 |

---

# 二十八、作品 C 的警報流程

觸發後：

```python
led.value(1)
buzzer.value(1)
```

接著：

- 取得即時時間。
- 建立 earthquake alert。
- 加入 X、Y、Z acceleration。
- 發送 LINE Notify。
- 等待 5 秒。
- 關閉 LED。
- 關閉蜂鳴器。

---

# 二十九、作品 C 的成功測試

第 7 張投影片明確寫道：

> 成功測試出當搖晃地震儀時，LED、蜂鳴器在達到一定搖晃程度時發光與響起聲音，而 LINE Notify 訊息也經過搖晃後即時傳出訊息。

因此此組至少完成：

- 實體搖晃。
- 感測器量測。
- 門檻判斷。
- LED。
- 蜂鳴器。
- LINE Notify。

投影片另有實際搖晃測試影片截圖。

---

# 三十、作品 C 的 LINE Notify 實測

LINE 截圖中可見多筆 2024 年測試紀錄，例如：

```text
[Pico] 2024/05/20 06:17:09
Earthquake occurred!
Acceleration: (x: 1.87, y: -0.66, z: -1.34)
```

以及：

```text
[Pico] 2024/05/20 06:18:16
Earthquake occurred!
Acceleration: (x: 0.85, y: 0.63, z: 1.43)
```

另有 2024/05/16 的測試紀錄。

### 投影片文字與截圖的差異

投影片說：

> LINE Notify 所輸出的訊息包含了時間與位置。

但截圖實際可清楚看到的是：

- 時間。
- Earthquake occurred。
- X / Y / Z 加速度。

未在畫面中清楚看到地理位置座標或位置名稱。

因此本整理將其記為：

```text
已確認：時間 + 三軸加速度
投影片聲稱：時間 + 位置
位置資訊：截圖中未能獨立確認
```

---

# 三十一、2024 作品共同技術架構

三份附件的核心技術非常一致。

## 31.1 控制器

- Raspberry Pi Pico / Pico 類型開發板。
- MicroPython。

## 31.2 感測器

- MPU6050。
- 三軸加速度。
- 三軸陀螺儀。
- 溫度。

## 31.3 通訊

- I2C。
- SCL：Pin 5。
- SDA：Pin 4。
- 400 kHz。

## 31.4 本地警報

- LED。
- 蜂鳴器。

## 31.5 網路

- Wi-Fi。
- NTP。
- LINE Notify。

## 31.6 開發環境

- Thonny。
- MicroPython。

---

# 三十二、2024 學生作品可辨識的學習能力

## 32.1 感測器與嵌入式系統

學生需要完成：

- GPIO。
- I2C。
- Pin 指定。
- MPU6050 初始化。
- 讀取感測資料。

---

## 32.2 資料處理

至少出現兩種設計：

### 方法 A：背景校正

```text
1000 組背景資料
→ 平均
→ bias
→ current - bias
→ threshold
```

### 方法 B：原始三軸固定門檻

```text
ax / ay / az
→ ± threshold
→ trigger
```

---

## 32.3 網路與 IoT

學生已實作：

- Wi-Fi 連線。
- IP address。
- HTTP POST。
- Token authentication。
- LINE 即時通知。
- NTP 校時。

---

## 32.4 人機輸出

同一事件同時可：

- LED 發光。
- 蜂鳴器響起。
- Thonny 顯示數據。
- 手機收到 LINE 警報。

這已形成基本的：

```text
Sensor → Edge device → Local alert → Cloud notification
```

架構。

---

# 三十三、2024 作品中的問題與反思

## 33.1 網路依賴

作品 A 已注意到：

```text
若網路失敗
不應連地震偵測本身都停止
```

因此提出「離線仍可蜂鳴」的改進。

---

## 33.2 感測器連線

第 4 組錯誤截圖出現：

```text
No MPU's detected
```

說明硬體實作除了網路問題，也涉及：

- I2C。
- 線路。
- MPU6050 偵測。

等問題。

---

## 33.3 警報門檻

2024 作品主要以固定單點 threshold 觸發：

- `0.082 g`。
- 或 X ±2、Y/Z ±1。

尚未在附件中看到後續年份作品常見的：

- STA/LTA。
- FFT。
- 高通濾波。
- 持續時間條件。
- 多條件事件確認。

因此 2024 作品更接近：

```text
感測器門檻型地震警報原型
```

---

# 三十四、2024 作品的發展層級

可依作品內容整理為以下學習鏈：

## Level 1：硬體接線

```text
Pico + MPU6050 + LED + Buzzer
```

## Level 2：讀取資料

```text
acceleration + gyro + temperature
```

## Level 3：門檻判斷

```text
acceleration > threshold
```

## Level 4：背景校正

部分作品進一步：

```text
background mean → bias correction
```

## Level 5：本地警報

```text
LED + buzzer
```

## Level 6：網路警報

```text
Wi-Fi + LINE Notify
```

## Level 7：事件時間

```text
NTP + RTC
```

## Level 8：工程反思

學生開始思考：

- 無網路時怎麼辦？
- 感測器無法偵測怎麼辦？
- 不同震度是否應用不同訊息？
- 警報與偵測是否應解耦？

---

# 三十五、適合後續研究編碼的欄位

若要把 2024 與 2026 作品進行縱向比較，可將每份作品依下列欄位編碼：

| 構面 | 2024 可觀察指標 |
|---|---|
| Hardware integration | Pico、MPU6050、LED、buzzer 是否完成 |
| Sensor acquisition | 是否讀取三軸 accel / gyro |
| Calibration | 是否蒐集背景值並扣除 bias |
| Trigger logic | 固定門檻 / 多級門檻 |
| Local feedback | LED / buzzer |
| Connectivity | Wi-Fi |
| Cloud notification | LINE Notify |
| Time handling | NTP / RTC |
| Data presentation | Thonny 即時輸出 |
| Debugging | 是否呈現錯誤與問題 |
| System robustness | 是否考慮離線運作 |
| Reflection | 是否提出具體改進 |
| Scientific signal processing | 2024 附件中尚未見 STA/LTA、FFT 等進階方法 |

---

# 三十六、2024 三份附件的共同流程圖

```text
                         ┌─────────────────┐
                         │ Raspberry Pi Pico│
                         └────────┬────────┘
                                  │ I2C
                                  ▼
                         ┌─────────────────┐
                         │     MPU6050     │
                         └────────┬────────┘
                                  │
              ┌───────────────────┴───────────────────┐
              │                                       │
              ▼                                       ▼
      acceleration x/y/z                       gyro x/y/z
              │
              ▼
      背景校正或固定門檻
              │
              ▼
        是否超過 threshold?
          │             │
         No            Yes
          │             │
          │             ├───────────────┐
          │             │               │
          │             ▼               ▼
          │          LED / Buzzer     NTP / RTC
          │                             │
          │                             ▼
          │                       建立警報訊息
          │                             │
          │                             ▼
          │                         Wi-Fi
          │                             │
          │                             ▼
          │                        LINE Notify
          │
          └────→ 持續監測
```

---

# 三十七、2024 作品的整體特徵

綜合三份附件，可以把 2024 Raspberry Pi 地震儀作業描述為：

> **以 Raspberry Pi Pico 與 MPU6050 為基礎，讓學生完成從感測器資料讀取、加速度門檻判斷、LED / 蜂鳴器聲光警報，到 Wi-Fi、NTP 與 LINE Notify 即時通知的完整 IoT 原型。**

其中作品 A 已進一步加入：

- 1000 組背景值。
- bias correction。
- ±0.082 g threshold。
- 離線運作構想。
- 分級警報構想。

第 4 組則呈現：

- 完整裝置。
- 程式架構。
- 但實作遭遇網路與 MPU6050 偵測問題。

第六組則呈現：

- 實際完成搖晃測試。
- LED / 蜂鳴器成功。
- LINE Notify 成功。
- Thonny 即時資料。
- 多筆實際測試記錄。

---

# 三十八、所有來源連結與檔案

## Canva

https://www.canva.com/design/DAGF3Lha98o/R1LTgOy1nWFA9UoKr-A-Ww/edit

## 附件

1. `地震儀模擬實作練習題.pdf`
2. `地震儀實作.pdf`
3. `地震儀.pptx`

---

# 三十九、與後續年份比較時特別值得保留的 2024 基準

2024 的作品已具備：

```text
硬體感測
+ MicroPython
+ IoT
+ LINE 通知
+ 初步背景校正
```

但主要事件判斷仍是：

```text
固定 acceleration threshold
```

因此如果後續要和 2026 作品做縱向分析，2024 可以視為很好的「基準年」：

```text
2024
固定門檻 + IoT 警報
        ↓
後續年份
濾波 / PGA / STA-LTA / FFT / 事件持續時間 / 多條件確認
```

這種差異非常適合用於分析學生在：

- 地震學概念。
- 感測器知識。
- 程式能力。
- 訊號處理。
- 系統設計。
- 問題診斷。
- 科學反思。

等面向的縱向發展。

---

## 備註

1. 本整理保留學生作品中的原始術語，例如「0.082 g（四級）」；未另外以外部資料校正其震度對應關係。
2. 原始檔案內出現的 Wi-Fi 密碼與 LINE Notify token 已刻意省略。
3. Canva 內容目前未能獨立讀取，因此未把任何附件內容推測為 Canva 的內容。
