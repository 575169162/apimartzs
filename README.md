# APIMARTZS · 图像 Playground

纯前端 AI 生图工作台，**浏览器直连接口，不需要任何服务器**。

> 在线地址：**https://575169162.github.io/apimartzs/**

## ✨ 功能

| 能力 | 说明 |
| --- | --- |
| 密钥验证才能使用 | 打开先验证 API Key；用拉取模型列表的方式校验（免费、不消耗额度），401 即判定无效 |
| 全部图像系列 | 集成了 **19 个模型家族 / 52 个图像模型**：GPT-Image-2 / 2.5 / 1.X、Seedream 4.0/4.5/5.0、Flux 2.0 / Kontext、Nano Banana 2 / Pro / 基础、Imagen 4.0、Qwen Image 2.0/3.0、Z-Image Turbo、Grok Imagine 1.5/2.0、Wan 2.7 Image、DALL·E、Midjourney |
| 参数按模型联动 | 分辨率、宽高比、数量、质量、参考图都会跟着所选模型切换；不合法的档位不会出现 |
| 尺寸看得见 | 宽高比下拉直接显示该档位的实际像素（如 `16:9 · 1536×864`，切到 2K 变 `2048×1152`） |
| 价格透明 | 每个模型显示各档位 credits 单价（credits = 美元 × 8，与官网 playground 口径一致） |
| 参考图 | 拖拽上传（自动压缩）或 URL，按模型上限限制张数；不支持图生图的模型会自动隐藏该区域 |
| 异步任务 | 提交后拿 task_id 轮询进度，卡片显示进度条、耗时与实际扣费 credits |
| 本地历史 | 生成记录（提示词/参数/扣费/耗时）存在本机 localStorage，随时可查、可复用参数 |

## 🔌 接口

| 用途 | 方法 | 地址 |
| --- | --- | --- |
| 校验密钥 / 模型列表 | GET | `https://api.aishuch.com/v1/models?expand=category` |
| 提交生成（异步） | POST | `https://api.aishuch.com/v1/images/generations` |
| 查询结果 | GET | `https://api.aishuch.com/v1/tasks/{task_id}` |
| 模型价格 | GET | `https://aishuch.com/api/pricing/model?model=<id>` |

取图路径：`data.result.images[].url[]`（`n>1` 时遍历）。

> ⚠️ 官方文档里的域名是 `api.apimart.ai`，但该域名在中国大陆无法访问；本站改用同路径的 **`api.aishuch.com`**（腾讯云节点，实测可用，且已开放 CORS：`access-control-allow-origin: *`，所以浏览器可以直连）。

## 🚀 使用

1. 打开 <https://575169162.github.io/apimartzs/>
2. 填入 API Key（[APIMart 控制台](https://apimart.ai)获取），验证后进入
3. 选模型 → 写提示词 → 选分辨率/宽高比/数量 → RUN

默认参数为 **gpt-image-2 / 1k / 16:9**。

## ⚠️ 注意事项

- **密钥不写进代码**：填在页面里的 Key 只保存在你自己的浏览器（localStorage），不会上传到任何服务器，也不会出现在仓库里。共用电脑用完请点浏览器「清除站点数据」。
- **历史记录不跨设备**：数据存在本机浏览器，换设备/换浏览器看不到，清理浏览器数据会清空。
- **图片链接会过期**：不同模型 24–72 小时不等（页面会显示到期时间）。**图床（CloudFront）不返回 CORS 头，前端无法直接把图片抓成二进制**，所以「下载」会在新标签页打开原图，右键另存为即可；生成后请及时保存。
- **计费以官网为准**：页面价格取自官网价格接口，credits = 美元 × 8；实际以账单为准。
- **部分模型限制**：`imagen-4.0`、`grok-imagine-2.0-ext` 不支持参考图；`seedream-4.5` 不支持 1K；`seedream-5-0-pro`、gemini 3 系列、imagen 单次只能出 1 张——页面已按官方文档自动限制选项。

## 🎨 主题（右上角可切换）

页面右上角有三个模式按钮，选择会记在本机（localStorage），刷新后保持；未登录时按钮浮动在登录页右上角。

| 模式 | 风格 |
| --- | --- |
| **像素**（默认） | 大面积灰（页面 `#d4d4d4`、侧栏 `#c2c2c2`、面板 `#dedede`）+ 小面积白；**零圆角、2–3px 粗描边、无模糊硬阴影、点阵底纹**、阶梯式进度条、方块开关与复选框、logo 像素化渲染 |
| **极简** | 大面积淡灰（`#f4f5f7`）+ 白色面板与输入框；1px 细描边、10px 圆角、柔和阴影；主按钮为深灰底白字；状态徽章改为浅色底彩字 |
| **夜间** | 黑灰配色（页面 `#17191c`、侧栏 `#101214`、面板 `#1e2126`）；1px 描边、8px 圆角；主按钮为浅底深字；进度条与徽章使用低饱和蓝/绿/红 |

实现方式：CSS 变量驱动——`--bg / --panel / --white / --ink / --bw / --radius / --sh-* / --bg-pattern / --img-render` 等结构参数在各主题块里整体覆盖，切换只改 `<html data-theme>` 一个属性。

- **字体**：黑体（`Heiti SC` → `PingFang SC` → `Microsoft YaHei` → `Noto Sans CJK SC` 回退链），全部使用系统字体，**不加载任何外部字体或 CDN**

## 📄 License

MIT
