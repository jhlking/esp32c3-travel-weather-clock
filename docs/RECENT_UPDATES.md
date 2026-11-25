# 近期更新清单（最新版潮汐时钟）

本文件记录本地最新版 KairosTide 相对完整项目仓库的后续更新。对比基准：

- 完整项目仓库：https://github.com/yishengsss/KairosTide-NotimerDay.git （默认分支 `main`，对比时 354 个文件）
- 本地最新版：`KairosTide-NotimerDay-main`
- 对比结果：42 个本地新增文件（已排除密钥文件 `services/api/.env` 与临时/缓存文件）；另有多处对既有文件的修改。

**说明**：这些更新都是在借鉴天气时钟思路之后，由本项目自主设计、开发与验证的，不属于被借鉴项目的原有内容，也未收录在上述完整项目仓库中。

## 1. 助手流式回答与安全排版

- 后端新增 SSE 流式协议：`adapters/ai/mimo.py` 增加 `stream_complete` 与纯函数 `decode_stream`，按协议里的分片序号重组，不依赖列表位置；`application/assistant/streaming.py` 承担 reserve、replay 与事件桥接，流式与非流式共用同一回合逻辑。
- 前端 `api/assistant.ts` 的 `sendMessageStream` 用 fetch + ReadableStream 读取流，`assistant/markdown.ts` 用 marked + DOMPurify 安全渲染回答（禁用图片与链接、脚本），回答气泡支持加粗与列表；流式过程中以短句 chip 展示工具调用过程。
- 对应测试：`tests/test_mimo_stream.py`、`tests/integration/test_assistant_stream.py`、`tests/assistant-stream.test.ts`。
- **演示效果**：回答逐字出现，等待感明显降低；查询日程时先看到工具过程，再看到完整回答。

## 2. 时间解析兜底（JioNLP）

- 新增 `application/assistant/time_check.py`：用 jionlp 解析用户原句，只有解析为准确时间点且区间宽度不超过 2 小时才采信；与模型给出的时间相差超过 30 分钟时，在草稿提示里写“原句解析出约 X，请核对”。
- 只提醒、不拒绝，草稿确认制保持不变。
- 对应测试：`tests/unit/test_time_check.py`。
- **踩坑记录**：jionlp 会把“今天天气不错”解析成全天区间，因此必须按区间宽度过滤。

## 3. 语音朗读、唤醒与服务端女声

- 新增服务端语音端口 `application/speech.py` 与适配器 `adapters/speech/edgespeak.py`（edge-tts，zh-CN-XiaoxiaoNeural，rate +4%）；路由 `POST /api/v1/assistant/speech` 返回 audio/mpeg。
- 前端 `api/speech.ts` 与 `assistant/voice.ts`：服务端女声优先，失败回退浏览器 speechSynthesis；用 generation 计数避免竞态。
- `assistant/aloud.ts` 管理朗读开关、进行中状态与“打开即读最后一条”；`createWakeListener` 支持“助手 / 小凯 / 凯洛斯 / kairos”唤醒词，结束 400ms 后自动续听。
- 对应测试：`tests/integration/test_speech_api.py`、`tests/voice.test.ts`、`tests/aloud.test.ts`。

## 4. 真实星空、流星雨与农历

- `scene/pastoral/stars.ts`：26 颗亮星（耶鲁 BSC 第五版，J2000 坐标、星等与 B−V），含昴宿星团成员；复用月亮的坐标变换，按地平高度与方位裁剪，绘制猎户座与冬季大三角连线。
- `scene/pastoral/showers.ts`：IMO Working List 的 8 场流星雨，按高斯剖面计算活跃度，跨年窗口取相邻年份；`render/meteors.ts` 负责粒子生成与渲染。
- `clock/lunar.ts`：动态加载 lunar-javascript，输出“农历十月十七 · 酉时 · 节气”，用于长按查时的副行，也进入后端助手上下文。
- 对应测试：`stars.test.ts`、`showers.test.ts`、`meteors.test.ts`、`lunar.test.ts`。
- **出处台账**：`docs/engineering/PROVENANCE.md` 登记了耶鲁 BSC、IMO 年历与相关开源库许可证。

## 5. 锦鲤重绘与树木生动化

- `scene/pastoral/render/koi.ts`：11 点脊椎采样 + 体宽剖面 + 体轴行波（carangiform），叉形尾鳍、背鳍与胸鳍随节律摆动，裁剪体形内绘制红白/墨斑，背深腹浅渐变与鳃盖、眼珠高光；游动速度驱动摆频。
- 着色器：树摆动改为悬臂梁一阶模态（越靠近树冠摆幅越大），加冠层光斑闪烁；`creatures.ts` 在风大时从树冠抖落碎屑或雪。
- 对应测试：`koi.test.ts`（包络单调、行波传播）、`shader.test.ts`（GLSL 静态护栏：ASCII、括号配平、uniform 与 RES 常量一致）。
- **出处台账**：行波模型、countershading 与树摆公式均在 `PROVENANCE.md` 登记文献来源。

## 6. 池塘互动与萤火虫同步

- `scene/pastoral/pond-life.ts`：4 条锦鲤用 Reynolds boids 分离 + seek/wander 转向；点击池塘产生涟漪与食粒，锦鲤追食并在吃到后短暂停顿；软墙与硬兜底防止游出池塘；封冻时整群隐去。
- `scene/pastoral/pulse.ts`：Mirollo–Strogatz 脉冲耦合振子，替换原先随机相位闪烁；离散模型需足够耦合强度才能完全同步，取 0.12，并用数值模拟验证收敛。
- 对应测试：`pond-life.test.ts`、`pulse.test.ts`。

## 7. 通知、摘要与状态反馈

- `schedule/notify.ts`：按 MDN Notifications API 实现系统通知，权限须在用户手势内请求，按 occurrence 去重，点击聚焦窗口。
- 后端 48 小时日程摘要预注入对话上下文，减少“接下来两天有什么安排”的一轮工具往返。
- `application/assistant/decisions.py` 与 `tests/integration/test_draft_status_note.py`：把草稿终态（已确认/已丢弃/已替代）注入对话，修复确认后再问被误答为“还没确认”的问题。
- 开始时间即提醒：只说开始时间时默认 1 小时时长，不再追问结束时间。

## 8. 文档与工程约束

- 新增 `docs/engineering/PROVENANCE.md`：所有文献、开源库与数据出处台账。
- 新增 `docs/memory/PRODUCT_MEMORY.md`：产品规则备忘录；新增 `docs/exec-plans/active/2026-10-02-m1.md`：M1 存档。
- 门禁覆盖：单文件不超过 400 行、分层依赖方向、契约一致性、mypy strict、ruff、pytest、vue-tsc、vitest 与构建；所有新增实现均有对应测试。

## 文件清单（本地新增，节选）

```text
apps/web/src/api/speech.ts
apps/web/src/assistant/{aloud,markdown,voice}.ts
apps/web/src/clock/lunar.ts
apps/web/src/schedule/notify.ts
apps/web/src/scene/pastoral/{pond-life,pulse,showers,stars,sky-bridge}.ts
apps/web/src/scene/pastoral/render/{koi,meteors}.ts
services/api/src/kairos/adapters/speech/{__init__,edgespeak}.py
services/api/src/kairos/application/speech.py
services/api/src/kairos/application/assistant/{decisions,streaming,time_check}.py
docs/engineering/PROVENANCE.md
docs/memory/PRODUCT_MEMORY.md
docs/exec-plans/active/2026-10-02-m1.md
```

完整清单包含对应的 17 个测试文件（前端 11、后端 6），未列出密钥与临时文件。
