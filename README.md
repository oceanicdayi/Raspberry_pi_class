# Raspberry Pi Class

本 repository 整理地球物理／地震學課程中的 Raspberry Pi、Raspberry Pi Pico、ESP32／NodeMCU 自製地震儀與學生專題成果。

## 學生作品整理

- [2023 年 6 月：ESP32 / NodeMCU 地球物理學生作品](docs/2023_06_geophysics_esp32_student_projects.md)
- [2023 Raspberry Pi Pico 學生作品](docs/2023_raspberry_pi_student_projects.md)
- [2024 Raspberry Pi 學生作品](docs/2024_raspberry_pi_student_projects.md)
- [2024 Raspberry Pi Pico 學生作品完整整理](docs/2024_raspberry_pi_pico_student_projects_all.md)
- [2026 Raspberry Pi 學生作品](docs/2026_raspberry_pi_student_projects.md)

## 原始資料與來源索引

- [原始附件與外部來源索引](source_files/README.md)
- [原始附件檔名、大小與 SHA-256 manifest](source_files/source_manifest.csv)

> 部分學生原始簡報包含 Wi-Fi 密碼、LINE token、Discord webhook 等敏感憑證；整理文件不重現這些憑證。大型 PDF / PPTX 原始附件是否已實體存入 repository，請以 `source_files/README.md` 與 repository 檔案列表為準。

## 年度技術演進概覽

```text
2023-06  NodeMCU / ESP8266
         I2C + threshold + SAC + SWARM + spectrum
                    ↓
2023     Raspberry Pi Pico / Pico W
         background correction + IoT + Email / LINE / IFTTT
                    ↓
2024     Raspberry Pi Pico + MPU6050
         bias correction + threshold + LINE / Discord + Socket / JSON
                    ↓
2026     Raspberry Pi
         PGA + filter + STA/LTA + FFT + Edge-to-Cloud + web visualization
```
