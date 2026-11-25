# 硬件工程文件

本目录预留给原理图、PCB、3D 结构与固件等工程文件。目前为空：原工程页面的设计图、BOM、3D 模型与附件栏均无公开下载数据，本仓库不转载未公开的源文件。

KairosTide 的完整软件项目在 https://github.com/yishengsss/KairosTide-NotimerDay.git ；本仓库以硬件资料为主，两者不要混淆。

## 如何取得原始工程

1. 打开原工程：[旅行天气时钟-Esp32C3 2.0](https://oshwhub.com/eda_pnhleoqpu/esp32c3_-zhang-tiao-ping-you-hu-ji_copy_copy_copy_copy_copy)
2. 在立创 EDA 中“克隆工程”，获取原理图和 PCB 源文件（需按平台规则操作）。
3. 需要 3D 结构 STL 与固件源码时，请联系原作者；B 站搜索 UP 主“小他里里”可看功能演示。

## 建议目录约定

取得源文件后可按下面结构放入本目录：

```
hardware/
├── schematic/    # 原理图导出 PDF / EDA 源文件
├── pcb/          # PCB 源文件与 Gerber
├── 3d/           # 外壳 STL / 步进文件
└── firmware/     # 固件源码或烧录固件
```

放入文件前请再次确认原工程许可证（GPL 3.0）与平台“仅限学习交流研究、不得商用”的声明。
