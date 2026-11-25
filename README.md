# 旅行天气时钟 · ESP32-C3 2.0 — 硬件资料库

根据立创开源硬件平台（OSHWHub）公开工程整理的硬件资料仓库，供学习、交流与复刻查阅。

![整机显示效果](assets/images/01-assembled-display.jpg)

## 重要声明

- **原工程作者**：eda_pnhleoqpu（立创开源硬件平台）
- **原始工程**：[旅行天气时钟-Esp32C3 2.0](https://oshwhub.com/eda_pnhleoqpu/esp32c3_-zhang-tiao-ping-you-hu-ji_copy_copy_copy_copy_copy)
- **上游版本**：[旅行天气时钟-Esp32C3](https://oshwhub.com/eda_pnhleoqpu/esp32c3_-zhang-tiao-ping-you-hu-ji_copy_copy_copy)
- **原工程许可证**：GPL 3.0（全文见 [LICENSE](LICENSE)）
- 本仓库是**第三方资料整理**，不是原作者官方仓库；原工程知识产权归创作者所有。平台声明该项目仅供学习交流及研究，**不得用于商业售卖或盈利用途**。
- 原工程页面未公开附件与 3D 模型下载，本仓库**不含**原理图、PCB 源文件、3D 结构文件与固件源码。需要这些文件请到原工程页面克隆。

资料抓取时间：2026-11-25。所有事实出处见 [docs/SOURCES.md](docs/SOURCES.md)。

## 项目简介

一块基于 **ESP32-C3** 的桌面旅行天气时钟：1.9 寸彩色 LCD 显示时间、日期、星期、天气与温度，带万年历、商店、花园和自定义铃声等玩法。2.0 版本在硬件上增加了 **3 个蜂鸣器**，取消 IIC 温湿度传感器，改用**热敏电阻**采集温度。

- 复刻成本（原站标注）：**约 ￥30**
- 开源协议：**GPL 3.0**
- 详细介绍：B 站搜索 UP 主 **小他里里**

## 硬件速览

| 项目 | 参数 | 依据 |
| --- | --- | --- |
| 主控 | ESP32-C3（工程名称 / ESP 系列标签） | 原工程页面 |
| 显示屏 | 1.9 寸 TFT，320×170，ST7789 驱动，4-SPI，10P 接口 | 原工程描述 + 屏幕截图 |
| 发声 | 3 个蜂鸣器（2.0 新增） | 原工程简介 + 实物图 |
| 温度采集 | 热敏电阻（替代原 IIC 温湿度模块） | 原工程简介 |
| 板间连接 | 两块 PCB，通过 14P / 0.5 mm FPC 排线连接 | 原工程描述 + 排线截图 |
| 结构件 | 3D 打印外壳，M2 平头自攻螺丝固定 | 原工程描述 + 实物图 |
| 设计工具 | 立创 EDA（OSHWHub 工程） | 平台属性 |
| 协议 / 成本 | GPL 3.0 / 约 ￥30 | 原工程页面 |

详见 [docs/HARDWARE.md](docs/HARDWARE.md)。

## 显示界面（原工程实拍）

屏幕顶部显示时间（如 `15:32`）、日期星期（`06月04日 周四`）、当日温度区间与天气图标；中间是天气插画；底部显示城市（如 `广州`）与温度。2.0 新增万年历模式。更多功能见 [docs/FIRMWARE.md](docs/FIRMWARE.md)。

![PCB 与结构](assets/images/02-internals.png)

## 仓库导航

| 文档 | 内容 |
| --- | --- |
| [docs/HARDWARE.md](docs/HARDWARE.md) | 硬件架构、已确认参数与实物图可推断内容 |
| [docs/BOM.md](docs/BOM.md) | 可核实物料清单与待补全项 |
| [docs/PINOUT.md](docs/PINOUT.md) | 屏幕与板间接口（待原工程原理图确认） |
| [docs/FIRMWARE.md](docs/FIRMWARE.md) | 功能与界面特性 |
| [docs/ASSEMBLY.md](docs/ASSEMBLY.md) | 复刻装配注意事项 |
| [docs/SOURCES.md](docs/SOURCES.md) | 每条信息的来源与图片索引 |
| [hardware/](hardware/) | EDA 源文件获取与目录约定 |
| [assets/images/](assets/images/) | 原工程页面配图（6 张） |

## 复刻提醒

1. 原站的“设计图 / BOM / 3D 模型 / 附件”在抓取时均无公开下载数据，需到原工程页面克隆工程或联系原作者才能得到完整 EDA 文件。
2. 接线与引脚定义请以原工程原理图为准；本仓库的引脚表是待核实模板，不可直接用于打板。
3. 自行验证电路可行性，复刻后果自负；不得用于商业售卖。
