# 2023 Raspberry Pi Pico 自製地震儀學生作品整理

> 整理日期：2026-08-19  
> 資料來源：3 個 Canva 設計、1 個 Google Slides、3 份附件。  
> 整理原則：僅整理來源中實際呈現的 Raspberry Pi / Raspberry Pi Pico 專案內容；不自行補齊來源未提供的資訊。  
> 安全處理：部分原始程式含 Wi-Fi 密碼、IFTTT key、LINE Notify token 等憑證，本文件僅保留其功能與架構，**不重現實際密碼或 token**。

---

# 一、資料來源

## 外部連結

1. 第一組 Canva  
   https://www.canva.com/design/DAF41bVK0Rc/wJgiAaQWeAQNXCkD2aXCBw/edit

2. 第三組 Canva  
   https://www.canva.com/design/DAF4h9POMV0/JM0SAeYOo_-oxmGGMAjAmA/edit?ui=eyJHIjp7fX0

3. Google Slides：Raspberry pico 自製地震儀成果報告  
   https://docs.google.com/presentation/d/1C6svKWVyMLiwx_Pr7qvKOJp2BOOFhNHwe74O5SmbXlU/mobilepresent?slide=id.p

4. 第六組 Canva  
   https://www.canva.com/design/DAF4d2nLCmg/FgUXFdl-tLwxt9s3GD6MGQ/edit

## 附件

5. `第二組Raspberry pico 自製地震儀成果報告.pdf`
6. `Raspberry pico  自製地震儀成果報告.pptx`（第四組）
7. `Raspberry pico 自製地震儀成果報告-1.pptx`（第七組）

---

# 二、2023 年作品整體快速比較

| 作品 | 核心硬體 | 感測/資料處理 | 警報輸出 | 網路/IoT | 主要特色或問題 |
|---|---|---|---|---|---|
| 第一組 | Raspberry Pi Pico W、加速度儀、LED、蜂鳴器、麵包板 | 加速度背景值、地震資料記錄 | 提示音、地震警報音 | Email、試算表；提出 LINE 延伸 | 開機/網路成功失敗都有聲音；重視背景值與誤報 |
| 第二組附件 | 來源頁面未完整呈現硬體 | NTP 網路校時 | 未在附件頁面呈現 | Wi-Fi + NTP | 已連網但無法取得 NTP 時間；以增加重試/等待成功解決 |
| 第三組 | Pico、MPU6050、多 LED、蜂鳴器 | 三軸背景值校正、100 筆循環緩衝、第一代門檻 2、第二代門檻 1.5 | LED、蜂鳴器；第二代播放旋律 | IFTTT、LINE 程式架構 | 兩代原型；嘗試震度分級、低通、三向量、PGA，但因單位理解不足未完成 |
| 第四組 | Pico、地震儀/加速度感測器、LED、蜂鳴器、開關 | 三方向加速度與其他三軸資料 | LED、蜂鳴器、開關 | IFTTT 未執行 | IFTTT 因付費註冊限制未完成 |
| Google Slides 作品 | Pico、LED、蜂鳴器、加速度儀 | 震動大小與波幅、距離影響 | LED、蜂鳴器 | 未見完整 IoT 實作 | 強調低成本、接線與加速度儀觀測；首頁未標示組別 |
| 第六組 | Pico、加速度儀、蜂鳴器、LED | X/Y/Z 加速度超門檻 | LED、蜂鳴器、資料回傳電腦 | 後續功能受付費軟體限制 | 從「亂閃/延遲」除錯到成功找到反應規律 |
| 第七組 | Raspberry Pi Pico W、GY-521、LED、開關、蜂鳴器 | bias 校正、加速度+時間循環列表 | Pico LED 等狀態輸出 | Wi-Fi、IFTTT 原規劃 | 已能記錄與校正，但因 IFTTT 無法使用而無法上傳 |

---

# 三、作品 1：第一組 — Raspberry Pico 自製地震儀

**來源：Canva**  
https://www.canva.com/design/DAF41bVK0Rc/wJgiAaQWeAQNXCkD2aXCBw/edit

## 3.1 組員

- U11110033 周家甫
- U11110021 何寬祐
- U11204035 張鈞瑜

---

## 3.2 專案結構

作品內容包含：

- 自製地震儀
- 匯入程式與定義基本參數
- 定義旋律與節奏
- 樂譜
- 播放音樂程式
- 連接網路與校正時間
- 發送警報
- 運作主程式

這顯示此組不只做震動感測，也把**聲音設計、網路與資料紀錄**整合進 Pico 地震儀。

---

## 3.3 工作流程

學生整理的地震儀工作流程為：

```text
開機
 ↓
播放提示音
 ↓
連接網路
 ├─ 成功：有提示音
 └─ 失敗：也有不同提示
 ↓
偵測加速度
 ↓
偵測到地震
 ↓
播放警報音
 ↓
發送警報 Email
 ↓
將地震數據記錄到試算表
```

這是 2023 作品中較完整的「感測 → 警報 → 雲端紀錄」流程之一。

---

## 3.4 Raspberry Pi Pico 與程式學習

學生心得指出：

- 使用 Raspberry Pi Pico W。
- 使用麵包板與杜邦線。
- 使用 Thonny 執行 Python / MicroPython 程式。
- 透過修改程式可調整：
  - 蜂鳴器頻率。
  - 聲音。
  - LED 亮度/行為。
- 有學生將先前 Python 課程所學實際用在 Pico 硬體控制。

學生亦提到結合 ChatGPT 協助撰寫程式，使地震儀可以「鳴唱音樂」，代表生成式 AI 已開始進入程式設計支援流程。

---

## 3.5 地震警報與資料處理思考

學生特別提到最困難的問題之一是：

> 如何避免誤發警報。

他們意識到此問題與：

- 地震判定。
- 加速度背景值。
- 儀器放置環境。

有關。

這表示 2023 年第一組已開始從「只要感測到震動就響」進一步思考**背景值、門檻與 false alarm**。

---

## 3.6 可優化方向

學生列出三項：

### 1. 依放置地點調整背景值

```text
不同環境
→ 不同背景振動
→ 重新設定 background / bias
→ 降低誤發警報
```

### 2. 與 LINE 連動

除了 Email 外，希望增加 LINE 作為地震訊息通道。

### 3. 繪製地震波形

希望把偵測到的地震資料繪成波形，使結果比單純數值更直觀。

---

## 3.7 教學特徵

此組已出現以下能力：

```text
硬體接線
+ Python
+ 聲音/LED
+ 網路
+ Email
+ 試算表
+ 背景值
+ 誤報反思
+ 波形視覺化構想
```

---

# 四、作品 2：第二組 — NTP 網路時間除錯紀錄

**來源附件：**  
`第二組Raspberry pico 自製地震儀成果報告.pdf`

> 本附件目前僅有一頁可讀內容，主題集中在 NTP 時間同步問題；來源並未呈現完整硬體、組員與警報程式，因此本節只整理該頁實際支持的內容。

---

## 4.1 問題

學生已成功連上網路，但：

```text
無法從 NTP server 取得時間
```

這表示 Pico 的 Wi-Fi 本身不是唯一問題，時間同步層仍可能失敗。

---

## 4.2 學生查找的可能原因

### 原因 1：NTP Server 無法訪問

學生提出可嘗試：

- `time.google.com`
- `pool.ntp.org`

並注意到某些：

- 公司網路
- 學校網路

可能阻擋：

```text
UDP port 123
```

造成 NTP 無法連線。

---

### 原因 2：防火牆或網路安全設定

可能是：

```text
Router / Firewall
→ 阻止 NTP request
```

---

### 原因 3：程式碼問題

學生提出應確認：

- 連線流程是否正確。
- 時間同步階段是否正確。
- 是否加入 exception / error handling。
- 是否能診斷失敗原因。

---

### 原因 4：同步 timeout

網路：

- 延遲。
- 不穩定。

可能導致 NTP timeout。

學生提出：

- 增加等待時間。
- 增加 timeout。
- 多次嘗試同步。

---

## 4.3 實際解決方式

學生最後選擇先從最容易改善的第四項開始：

```text
增加/反覆嘗試同步
```

經多次測試後成功解決。

之後實驗中也持續確認網路狀態，沒有再發生相同問題。

---

## 4.4 教學價值

此附件很重要，因為它不是只有「成功作品」，而是留下完整的：

```text
問題
→ 假設原因
→ 查找資料
→ 選擇最容易測試的原因
→ 反覆驗證
→ 問題解決
```

這可視為工程除錯與 problem-solving 的直接證據。

---

# 五、作品 3：第三組 — 第一代與第二代地震儀

**來源：Canva**  
https://www.canva.com/design/DAF4h9POMV0/JM0SAeYOo_-oxmGGMAjAmA/edit?ui=eyJHIjp7fX0

## 5.1 組員

- 黃翊瑄
- 周子堯
- 徐楷茹
- 林宸佑

---

# 六、第三組第一代地震儀

## 6.1 硬體與 GPIO

程式中使用：

- Pico 內建 LED。
- 多顆外接 LED：
  - Pin 28
  - Pin 27
  - Pin 26
  - Pin 22
  - Pin 21
- 蜂鳴器：
  - Pin 2
- MPU6050。

I2C：

```python
I2C(
    0,
    sda=Pin(0),
    scl=Pin(1),
    freq=400000
)
```

---

## 6.2 加速度背景值

程式明確設定：

```python
axbias = 0.06689453
aybias = 0.01220703
azbias = 0.9699707
```

實際資料讀取時：

```python
ax = imu.accel.x - axbias
ay = imu.accel.y - aybias
az = imu.accel.z - azbias
```

因此已經有：

```text
Raw acceleration
→ subtract bias
→ dynamic acceleration
```

的校正概念。

---

## 6.3 資料緩衝

程式設定：

```python
max_sample = 100
sample_range = 30
data_list = [[] for i in range(max_sample)]
```

來源註解寫：

```text
最大保存 100 筆（1 秒）的資料
```

資料結構會保存：

- |X|
- |Y|
- |Z|
- hour
- minute
- second

並以循環方式寫入。

這是 2023 作品中較明確的**事件前後資料暫存 / circular buffer**概念。

---

## 6.4 時間

使用：

- `RTC`
- `ntptime`

程式將：

```text
UTC + 8 hours
```

轉為台北時間。

---

## 6.5 Wi-Fi

程式：

- 啟動 WLAN。
- 最多嘗試 10 次。
- 每次連線間隔數秒。
- 利用 Pico LED 表示網路狀態。

這已經比單次 `connect()` 多了一層簡單的連線可靠度設計。

---

## 6.6 第一代觸發門檻

第一代主程式判斷：

```python
if abs(x) >= 2 \
or abs(y) >= 2 \
or abs(z) >= 2:
    alert()
```

也就是任一軸背景校正後的絕對值達到約 `2` 即觸發。

---

## 6.7 第一代警報

`alert()`：

- 開 LED。
- 開蜂鳴器。
- 約 2 秒後關閉蜂鳴器。
- 關 LED。

程式中另設計 IFTTT 上傳函式：

```text
send_data_to_ifttt()
```

但大量資料上傳區塊在來源程式裡被註解，因此不能視為已完成運作。

---

# 七、第三組第二代地震儀

第二代最重要的改變是把蜂鳴器從單純「響」變成：

```text
PWM 蜂鳴器
→ 音高 frequency
→ 音符
→ 旋律
```

---

## 7.1 蜂鳴器 PWM

程式建立大量 tone frequency，例如：

```text
C4
D4
E4
...
C8
```

並定義：

- `playtone()`
- `bequiet()`
- `playsong()`

觸發地震後會播放一段旋律。

---

## 7.2 第二代門檻

第二代改為：

```python
if abs(x) >= 1.5 \
or abs(y) >= 1.5 \
or abs(z) >= 1.5:
```

相較第一代的 `2`：

```text
2.0
↓
1.5
```

系統變得更敏感。

---

## 7.3 LINE Notify

第二代程式中出現：

```text
send_line_notify()
```

以 HTTP request 搭配 Bearer token 傳送 LINE Notify。

但在主程式裡：

```python
# send_line_notify(...)
```

是被註解的，因此來源支持的是：

> 已設計 LINE 通知程式，但主程式中實際呼叫被關閉。

---

# 八、第三組對「震度」與 PGA 的反思

這是 2023 年資料中非常值得保留的部分。

學生原先希望：

```text
加速度大小
→ 類似中央氣象署震度分級
→ 不同 LED / 蜂鳴器反應
```

但實際進行時發現：

- 自製加速度儀輸出值與氣象署使用的地動加速度 PGA `(cm/s²)` 似乎不相同。
- 不確定程式輸出數值的物理單位。
- 因此無法可靠轉成震度。

學生進一步提到原本想進行：

- 10 Hz 低通濾波。
- 三向量合成。
- 最大地動加速度 PGA 計算。

但最後因：

```text
單位 / 資料定義不清
```

沒有完成。

這個反思非常重要，因為學生已開始意識到：

> 「感測器有數字」不代表「數字已可直接解釋成地震學上的 PGA 或震度」。

---

# 九、第三組的實作問題

## 9.1 Wi-Fi

學生曾遇到：

```text
Pico 無法正常連網
```

且一度無法找到問題原因。

---

## 9.2 USB

有組員的電腦無法讀到 Pico USB，最後改用其他組員電腦。

---

## 9.3 電源

實驗初期電池接線損壞，後來使用行動電源解決。

---

## 9.4 組內操作分工

有學生指出：

- 主要操作者集中在 Pico 所連接的電腦附近。
- 有人因此難以參與。
- 提出若「每人一盒」可能提高每位學生的實際操作機會，但也理解成本與教學支援的限制。

這屬於很有價值的教學實施反思。

---

# 十、作品 4：第四組 — Raspberry Pico 自製地震儀

**來源附件：**  
`Raspberry pico  自製地震儀成果報告.pptx`

## 10.1 組員

- U11110006 林憶蓁
- U11110015 莊博文
- U11110020 葉宥陞

---

## 10.2 報告內容架構

作品包含：

- 組合裝置。
- 地震儀程式碼。
- LED 程式碼。
- 蜂鳴器。
- 開關接線。
- 開關及 LED 程式碼。
- IFTTT。
- 裝置展示。

---

## 10.3 組合裝置

學生將：

- Pico。
- 地震感測裝置。

安裝在麵包板。

來源文字記錄的接線為：

```text
地震儀 UCC → 麵包板 j5
GND → j3
SCL → a2
SDA → a1
```

> 原來源寫作 `UCC`，本整理保留原文，不自行改寫成其他腳位名稱。

---

## 10.4 感測資料

來源將：

```text
ax, ay, az
```

描述為三方向加速度。

來源將：

```text
gx, gy, gz
```

描述為三方向的「重力加速度」。

本文件保留學生原始用語，不另外修正其感測器物理量定義。

---

## 10.5 控制元件

作品另實作：

- LED。
- 蜂鳴器。
- 開關。

因此學習並不只限於 sensor input，也有：

```text
digital output
+ digital input
```

---

## 10.6 IFTTT

報告寫道：

```text
因 IFTTT 需付費註冊帳號
因此無法執行此項程式
```

因此第四組的 IoT 上傳設計並未完成。

---

# 十一、作品 5：Google Slides — Raspberry Pico 自製地震儀

**來源：Google Slides**  
https://docs.google.com/presentation/d/1C6svKWVyMLiwx_Pr7qvKOJp2BOOFhNHwe74O5SmbXlU/mobilepresent?slide=id.p

> 首頁列出四位學生，但未在可讀文字中明確標示「第幾組」，因此本整理不自行推定組別。

## 11.1 成員

- 地生四 陳芯柔
- 地生四 崔友維
- 地生四 何函育
- 地生三 葉景翔

---

# 十二、Google Slides 作品的 LED

接線概念：

1. 將 LED 長短腳接到麵包板。
2. 再用接線將 Pico 與 LED 形成電路。
3. 使用程式控制：
   - 亮一下。
   - 閃爍。
   - 長時間發光。

這代表學生先從單純 digital output 入門。

---

# 十三、Google Slides 作品的蜂鳴器

學生指出：

> 接線方式類似 LED，但程式中的 `value` 要記得設為 0，否則蜂鳴器會一直響。

這是一個簡單但實際的 GPIO state 控制經驗。

---

# 十四、Google Slides 作品的加速度儀接線

來源記錄：

```text
Pico GP0 → SDA
Pico GP1 → SCL
Pico GND → 加速度儀 GND
Pico 3V3(OUT) → 加速度儀 VCC
```

這與 I2C 感測器接法一致，是完整可辨識的硬體接線資訊。

---

# 十五、Google Slides 作品的地震學觀察

學生指出加速度儀可測量附近震源造成的震動。

並提出：

```text
震動大小
→ 波幅大小改變
```

以及：

```text
測站與震動源距離
→ 也會影響實驗結果
```

因此不能只看單一振幅，必須同時考慮多個因素。

這是這份作品中最明確的「硬體量測連結地球物理概念」內容。

---

# 十六、Google Slides 學習反思

學生共同提到：

- 一塊小型 Pico / 麵包板可以完成從亮燈到震動監測。
- Pico 成本低、一般學生或民眾較能負擔。
- 基本程式指令能帶來很直接的硬體回饋。
- 把零件、線路和程式組合起來的感覺像「樂高」。
- 此實驗提供進一步學習程式與硬體的動機。

---

# 十七、作品 6：第六組 — 自製簡易地震儀

**來源：Canva**  
https://www.canva.com/design/DAF4d2nLCmg/FgUXFdl-tLwxt9s3GD6MGQ/edit

## 17.1 組員

- 地生二 林幼鎂
- 王云柔
- 謝語姍

---

## 17.2 專案目的

學生描述的目標是：

```text
偵測到地震
→ 發出警報
→ 將當時資料回傳電腦
```

實體硬體包括：

- Pico。
- 加速度儀。
- 蜂鳴器。
- LED。
- 麵包板。

---

# 十八、第六組的程式開發方式

學生先：

1. 使用老師提供的範例測試。
2. 確認功能。
3. 以範本進行修改。
4. 完成最後的地震儀程式。

這是典型的：

```text
scaffold code
→ modify
→ test
→ debug
→ final prototype
```

---

# 十九、第六組的地震觸發構想

學生希望：

```text
X / Y / Z 三方向加速度
→ 超過設定值
→ 蜂鳴器發出警告
→ LED 亮起
→ 資料回傳電腦
```

來源並未提供可確認的具體門檻數值，因此本整理不補入 threshold。

---

# 二十、第六組的除錯歷程

這組作品的價值很大一部分在除錯。

## 20.1 初始問題

剛匯入程式時：

- LED 閃爍。
- 蜂鳴器反應。

與學生實際製造的震動沒有明顯規律。

學生先確認：

```text
接線沒有明顯錯誤
```

因此轉向檢查程式。

---

## 20.2 調整項目

學生嘗試修改：

- 閃爍時間。
- 反應時間。
- 程式邏輯。

經多次測試一度仍得到相同結果。

---

## 20.3 找到原因

課堂接近結束時，學生調整反應時間後：

- 找到感測器接收與警報反應的時間規律。
- 成功完成簡單地震儀。

心得進一步指出：

- 部分問題是接觸不良。
- 部分問題是程式造成反應延遲。
- 原先敲擊後以為程式沒運作，過一段時間蜂鳴器才突然響。
- 修改後反應變得流暢。

這是一個很清楚的：

```text
hardware problem
+
software timing problem
```

混合除錯案例。

---

# 二十一、第六組的限制

有學生提到：

> 後面因為程式 / 軟體需要付費，因此無法做實際的上路測試。

來源沒有在這段文字中明確指出是哪一個服務，因此本整理不自行將其指定為 IFTTT。

---

# 二十二、作品 7：第七組 — Raspberry Pi Pico W 地震儀

**來源附件：**  
`Raspberry pico 自製地震儀成果報告-1.pptx`

## 22.1 硬體與工具

材料清單：

- Raspberry Pi Pico W × 1。
- GY-521 加速度儀模組 × 1。
- LED × 1。
- 開關 × 1。
- 蜂鳴器 × 1。
- 杜邦線若干。
- 麵包板 × 1。
- 電腦 × 1。
- Thonny IDE × 1。

GY-521 一般搭載 MPU6050，但來源使用的名稱是 **GY-521 加速度儀模組**，本整理以來源名稱為主。

---

# 二十三、第七組的學習流程

作品將課程分成：

```text
0. 硬體介紹
1. 安裝環境與測試
2. 數位輸出
3. 數位輸入
4. 加速度儀
5. 寫出檔案與自動啟動程式
6. 進階篇 — 物聯網
```

這份流程很適合用來重建 2023 課程原始教學設計。

---

# 二十四、第七組的數位 I/O

學生分別完成：

- LED。
- 蜂鳴器。
- 開關。

因此有：

```text
Digital output
→ LED / Buzzer

Digital input
→ Switch
```

---

# 二十五、第七組的加速度校正

作品結果描述：

```text
連接 Wi-Fi
↓
程式執行
↓
Pico LED 亮起
↓
開始記錄數據
↓
讀取加速度
↓
從偏差值中減去
↓
校正數據
```

也就是：

```text
corrected acceleration
=
raw acceleration - bias
```

雖未提供實際 bias 數值，但校正概念明確存在。

---

# 二十六、第七組的循環列表

校正後系統會把：

- 加速度。
- 時間。

存到：

```text
循環列表
```

因此該組已具備：

```text
continuous acquisition
+ time stamp
+ circular data buffer
```

的概念。

---

# 二十七、第七組的 IoT 上傳限制

正常規劃的下一步是：

```text
記錄資料
↓
上傳資料
```

但學生報告指出：

```text
無法使用 IFTTT
→ 無法上傳紀錄資料
```

並造成：

```text
程式沒有完整跑完
→ 麵包板上的 LED 一直亮著
```

學生已能把硬體狀態與軟體流程未完成連結起來。

---

# 二十八、2023 年共同硬體架構

從各組可以整理出共同的硬體核心：

```text
Raspberry Pi Pico / Pico W
        │
        ├─ Accelerometer / MPU6050 / GY-521
        ├─ LED
        ├─ Buzzer
        ├─ Switch（部分組）
        └─ Breadboard + jumper wires
```

---

# 二十九、2023 年共同軟體能力

## 29.1 MicroPython / Python

主要透過：

- Thonny。
- `machine.Pin`
- `machine.I2C`
- `RTC`
- `network`
- `ntptime`
- HTTP request。

---

## 29.2 Digital output

學生學習：

```text
LED ON/OFF
Buzzer ON/OFF
PWM tone
```

---

## 29.3 Digital input

部分組別加入：

```text
Switch
```

---

## 29.4 Sensor acquisition

使用三軸加速度：

```text
ax
ay
az
```

部分程式同時讀取：

```text
gx
gy
gz
```

---

# 三十、2023 年資料校正層次

2023 已可看到不同成熟度。

## Level 1：直接讀 raw data

```text
MPU6050 / accelerometer
→ ax / ay / az
```

## Level 2：背景值

學生開始注意：

```text
background vibration
bias
```

## Level 3：bias subtraction

第三組、第七組明確出現：

```text
corrected = raw - bias
```

## Level 4：循環緩衝

第三組與第七組已出現：

```text
circular list / data buffer
```

---

# 三十一、2023 年事件判斷方式

主要仍是：

```text
單一加速度 threshold
```

例如第三組：

### 第一代

```text
任一軸 |a| >= 2
```

### 第二代

```text
任一軸 |a| >= 1.5
```

尚未在這批來源中看到正式實作：

- STA/LTA。
- FFT。
- Butterworth filter。
- 多條件事件確認。

---

# 三十二、2023 年 IoT 與資料傳輸

學生已經嘗試多種方法：

- Email。
- Google 試算表 / spreadsheet 紀錄。
- IFTTT。
- LINE Notify。
- Wi-Fi。
- NTP。

因此 2023 並非只有「硬體地震儀」，而是已開始形成：

```text
Edge sensor
→ network
→ notification / cloud data
```

---

# 三十三、2023 年 IoT 實作的主要障礙

## 33.1 NTP

第二組：

```text
Wi-Fi connected
but NTP failed
```

最後用增加重試成功處理。

## 33.2 IFTTT

第四組與第七組都指出：

```text
IFTTT 付費/帳號限制
→ 無法完成資料上傳
```

## 33.3 Wi-Fi

第三組也出現不明原因連線問題。

因此 2023 的 IoT 教學中，網路與第三方雲端服務本身已成為很重要的工程變因。

---

# 三十四、2023 年地震學概念的深度

不同作品呈現不同層次。

## 基礎層

```text
晃動
→ 加速度
→ 超門檻
→ 警報
```

## 中階層

學生開始思考：

- 背景值。
- 儀器位置。
- 與震源距離。
- 波幅。
- false alarm。

## 進一步但未完成的層次

第三組已提出：

- 類氣象署震度分級。
- 10 Hz 低通。
- 三向量合成。
- PGA。

但因不清楚感測器數值單位，沒有完成。

這是很有價值的「認識到科學量測限制」證據。

---

# 三十五、2023 年作品的問題解決類型

## 35.1 Hardware debugging

- 接觸不良。
- USB 無法讀到 Pico。
- 電池接線損壞。
- LED / buzzer wiring。

## 35.2 Software debugging

- 程式延遲。
- 反應時間。
- 門檻設定。
- PWM 音樂。

## 35.3 Network debugging

- Wi-Fi。
- NTP。
- Timeout。
- NTP server。
- UDP 123。
- Firewall。

## 35.4 External service constraints

- IFTTT 付費。
- 第三方服務無法使用。

---

# 三十六、2023 年作品中已出現的創意功能

除了地震警報外：

- Pico 開機提示音。
- 網路成功/失敗提示音。
- 地震警報旋律。
- PWM 音樂。
- 多顆 LED。
- Email 通知。
- 試算表記錄。
- LINE 通知程式。
- 依環境改背景值。
- 地震波形繪圖構想。

這顯示創意主要集中在：

```text
人機互動
+
通知方式
+
IoT
```

而不是複雜地震訊號處理。

---

# 三十七、可用於後續年度比較的 2023 編碼欄位

| 構面 | 2023 可觀察證據 |
|---|---|
| Hardware integration | Pico、感測器、LED、buzzer、switch |
| Coding | Thonny / MicroPython / GPIO / I2C |
| Sensor acquisition | ax/ay/az；部分有 gx/gy/gz |
| Calibration | background、bias subtraction |
| Trigger logic | fixed threshold |
| Buffering | 100 samples / circular list |
| Local feedback | LED、buzzer、melody |
| Network | Wi-Fi |
| Time | NTP、RTC、UTC+8 |
| Cloud / IoT | Email、Spreadsheet、IFTTT、LINE |
| Debugging | USB、接線、延遲、Wi-Fi、NTP |
| Scientific interpretation | 波幅、距離、背景值、誤報 |
| Advanced seismology concept | PGA、低通、三向量（提出但未完整實作） |
| Reflection | 學生心得與具體改進方向 |
| Generative AI | 第一組學生提到以 ChatGPT 協助程式創作 |

---

# 三十八、2023 年可重建的教學進階路徑

根據所有作品，可推回大致的教學序列：

```text
Pico 硬體介紹
     ↓
Thonny / MicroPython
     ↓
LED digital output
     ↓
Buzzer digital output
     ↓
Switch digital input
     ↓
I2C accelerometer
     ↓
讀取 X/Y/Z
     ↓
背景值 / bias
     ↓
固定門檻 earthquake trigger
     ↓
LED / buzzer alarm
     ↓
Wi-Fi
     ↓
NTP / RTC
     ↓
IFTTT / Email / LINE / spreadsheet
     ↓
自動啟動 / 持續觀測
```

---

# 三十九、2023 年整體技術定位

綜合資料，2023 年學生作品可以定義為：

> **以 Raspberry Pi Pico / Pico W 為低成本邊緣裝置，結合加速度感測器、LED、蜂鳴器、MicroPython、Wi-Fi 與第三方網路服務，建立固定加速度門檻型的微型地震警報原型。**

這一年已經有：

- 背景校正。
- 簡單資料緩衝。
- 網路通知。
- NTP 時間。
- 雲端資料構想。

但科學訊號處理仍主要停留在：

```text
raw / bias-corrected acceleration
→ fixed threshold
```

而不是後續可能出現的：

```text
PGA
→ filtering
→ STA/LTA
→ FFT
→ event duration
→ multi-condition detection
```

---

# 四十、特別值得作為縱向研究基準的 2023 特徵

## 40.1 「能動」是第一層成果

2023 很多學生的核心成就：

```text
Pico 可以讀到震動
LED 可以亮
蜂鳴器可以響
```

---

## 40.2 開始理解「儀器不是只要有數字」

第三組已發現：

```text
sensor output unit 不清楚
→ 不能直接當 PGA
→ 不能直接套震度分級
```

這是重要的科學認識轉折。

---

## 40.3 背景校正已開始出現

2023 並非完全只用 raw threshold。

至少已有學生：

```text
量測背景值
→ 設定 bias
→ current - bias
```

---

## 40.4 IoT 是主要創新方向

這一年作品大量出現：

- Email。
- LINE。
- IFTTT。
- Spreadsheet。

所以 2023 的「進階」主要是：

```text
sensor
→ internet
→ notification / cloud
```

---

## 40.5 工程問題本身成為學習內容

學生必須處理：

- NTP。
- Wi-Fi。
- USB。
- 供電。
- 接觸不良。
- 反應延遲。
- 第三方 API / 付費服務。

這些資料很適合分析學生的：

```text
debugging
problem-solving
reflection
engineering thinking
```

---

# 四十一、2023 年作品共同架構圖

```text
                    Raspberry Pi Pico / Pico W
                              │
             ┌────────────────┼────────────────┐
             │                │                │
             ▼                ▼                ▼
       Accelerometer         LED            Buzzer
       MPU6050/GY-521         │                │
             │                └──── local ─────┘
             ▼                       alert
         ax / ay / az
             │
             ▼
     Background / Bias
             │
             ▼
      Corrected acceleration
             │
             ▼
       Fixed threshold
             │
             ▼
        Earthquake event
             │
       ┌─────┴───────────────┐
       │                     │
       ▼                     ▼
 LED / Buzzer           Wi-Fi / NTP
                             │
          ┌──────────────────┼───────────────────┐
          ▼                  ▼                   ▼
        Email              IFTTT               LINE
          │                  │                   │
          ▼                  ▼                   ▼
     Spreadsheet         Data upload         Notification
```

---

# 四十二、全部原始連結

## Canva

1. https://www.canva.com/design/DAF41bVK0Rc/wJgiAaQWeAQNXCkD2aXCBw/edit
2. https://www.canva.com/design/DAF4h9POMV0/JM0SAeYOo_-oxmGGMAjAmA/edit?ui=eyJHIjp7fX0
3. https://www.canva.com/design/DAF4d2nLCmg/FgUXFdl-tLwxt9s3GD6MGQ/edit

## Google Slides

4. https://docs.google.com/presentation/d/1C6svKWVyMLiwx_Pr7qvKOJp2BOOFhNHwe74O5SmbXlU/mobilepresent?slide=id.p

## 附件

5. `第二組Raspberry pico 自製地震儀成果報告.pdf`
6. `Raspberry pico  自製地震儀成果報告.pptx`
7. `Raspberry pico 自製地震儀成果報告-1.pptx`

---

# 四十三、來源限制與整理注意事項

1. **第二組附件只有一頁可讀內容**，因此只能確定 NTP 除錯歷程，不能推定完整系統功能。
2. Google Slides 首頁沒有在可讀文字中標示組別，因此本文件不自行命名為第五組。
3. 部分原始程式包含真實：
   - Wi-Fi SSID / password。
   - IFTTT key。
   - LINE Notify token。  
   本文件基於安全理由全部省略。
4. 學生原始文字中的物理量名稱、震度/PGA理解若有不精確之處，本文件以「來源如何描述」為主，不自行修改成外部標準答案。
5. IFTTT、LINE Notify 等第三方服務在不同年份的可用性可能改變；本文件描述的是**2023 學生作品當時的實作經驗**。

---

# 四十四、可供後續 2023–2024–2026 縱向比較的基準摘要

2023 的代表性能力可濃縮為：

```text
Pico / Pico W
+
MPU6050 / GY-521
+
GPIO / I2C
+
固定加速度門檻
+
初步 background correction
+
LED / Buzzer
+
Wi-Fi / NTP
+
Email / IFTTT / LINE / Spreadsheet
+
大量工程除錯
```

其中最重要的科學與工程轉折包括：

```text
從「裝置會響」
        ↓
理解 background / bias
        ↓
發現 sensor value 與 PGA / 震度不是同一件事
        ↓
開始思考濾波、三向量、PGA
```

因此 2023 很適合作為後續年份的**原型建構與基礎 IoT 階段**基準。
