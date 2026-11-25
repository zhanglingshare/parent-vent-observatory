# 家长废话观测站 · Parent Vent Observatory

把家长辅导作业时的「经典语录」量化成一张**会动、会讲、接近真人 3D** 的可视化大屏。

单文件 HTML，无需构建、无需后端即可运行；录音与音频解析全部在本机完成。

![screenshot](docs/screenshot.png)

## 它做什么

- **录入**：实时录音转写 / 上传已录音频（mp3 · m4a · wav · webm · ogg）/ 粘贴对话文本，支持家长↔孩子说话人切换
- **真录音**：MediaRecorder 采集、可试听、可下载，Web Audio 实时波形
- **真解析**：上传音频本地解码，得到真实时长、有效发声占比、说话段数，并绘制静态波形
- **废话分析**：八类废话词库（横向比较 / 重复唠叨 / 情绪施压 / 翻旧账 / 过度担忧 / 冷嘲热讽 / 道德绑架 / 无效催促），句级命中 + 拉普拉斯平滑得出废话浓度
- **大屏**：今日浓度大数字、四张 KPI、类型环图、高频词云、浓度弧仪表、经典语录 Top3、7 天趋势、时段横条、小测建议
- **会讲**：内置语音导览，边讲边高亮对应卡片；浏览器不支持语音时自动切换为字幕节奏
- **会动**：卡片飞入、数字滚动、扫光、条形生长、图表入场动画

## 快速开始

```bash
# 直接打开
open index.html        # macOS
start index.html       # Windows
xdg-open index.html    # Linux
```

或把仓库推到 GitHub 后，在 **Settings → Pages** 选择 `main` / root，即可得到在线地址。

> 图表库走 jsDelivr CDN（ECharts 5.5.1 + echarts-wordcloud 2.1.0），首次打开需联网。

## 数据口径（重要）

- 首屏的 76%、128 句、97 句、18 分 42 秒等为**示例数据**，用于原型演示，页脚已标注。
- 只有在你点击「生成战报」并输入文本后，浓度、类型分布、词云、Top3、时长才会替换为**本次真实计算/实测**的结果。
- 近 7 天趋势、时段分布属于聚合模块，目前仍为示例值。

## 接入火山引擎语音识别（Roadmap）

目前「上传音频」已能真实解码出时长与发声占比，但**音频转文字**需要服务端代理调用 ASR（密钥不可放前端）。

计划接入：**录音文件识别极速版**（一次请求同步返回，≤100MB / ≤2h）。

```
POST https://openspeech.bytedance.com/api/v3/auc/bigmodel/recognize/flash
X-Api-Key: <API Key>                 # 或旧版 X-Api-App-Key + X-Api-Access-Key
X-Api-Resource-Id: volc.bigasr.auc_turbo
X-Api-Request-Id: <uuid>
X-Api-Sequence: -1
Content-Type: application/json

{
  "user": { "uid": "<app id>" },
  "audio": { "data": "<base64 音频>" },
  "request": { "model_name": "bigmodel", "enable_itn": true, "enable_punc": true, "show_utterances": true }
}
```

返回 `result.text` 与 `result.utterances[]`，可直接写入转录框，再接现有的 `analyzeText()` 出报告。

## 目录

```
index.html          单文件大屏（唯一产物）
assets/kid-desk.png 3D 测算员场景图
docs/screenshot.png README 截图
```

## 说明

- 「废话」判定基于关键词匹配与启发式规则，属于娱乐化的表达分析，不构成对任何家庭的教育评价。
- 录音、音频均在本机处理，不上传服务器（接入 ASR 后仅在你主动点击时才上传）。

## License

MIT © 2026 zhanglingshare
