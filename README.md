# Dr. IELTS — 在线体验版 (Static Demo)

这是 **Dr. IELTS** 的纯前端静态体验版。无需后端、无需注册登录，打开浏览器即可浏览剑雅 21 题库并做题。

> 数据不会保存。如需完整功能（AI 教练对话、错题归因、记忆画像、训练计划、12 Agent 协作），请到 GitHub 下载完整版：[dr-ielts-open](https://github.com/Richard-LLLL/dr-ielts)

## 体验范围

| 模块 | 是否可用 | 说明 |
|:---|:---:|:---|
| 剑雅 21 题库浏览 | ✅ | 4 tests × 12 sections = 48 个完整 section，共 288 题 |
| 听力 / 阅读 / 写作 / 口语做题 | ✅ | 题目、选项、答案、解析均可正常渲染 |
| 全真模考 | ✅ | 计时器、答题卡、阶段流程均可用，提交后不保存 |
| 学习记录页 | ✅ | 日报/周报/月报卡片 + 月度打卡日历，数据为空 |
| 错题档案 | ✅ | 页面布局正常，数据为空 |
| AI 教练对话 | ❌ | 静态版不支持，需完整版后端 |
| 错题归因 / 记忆画像 | ❌ | 静态版不支持，需完整版后端 |
| 训练计划 / 计划任务 | ❌ | 静态版不支持，需完整版后端 |

## 本地体验

无需安装任何依赖，使用任意静态文件服务器，根目录设为 `product/`（这样题库 JSON 才能被前端通过相对路径访问）：

```bash
# Python 3 (推荐)
cd product
python -m http.server 8080

# Node.js (npx)
npx serve product

# 然后访问：
#   http://localhost:8080/frontend/landing/index-zh.html   # Landing 页
#   http://localhost:8080/frontend/dashboard/index.html    # 直接进入做题
```

> 也可直接在浏览器打开 `product/frontend/landing/index-zh.html`，但建议使用静态服务器以避免 file:// 协议下的跨域限制。

## 目录结构

```
dr-ielts-demo/
├── product/
│   ├── domain/
│   │   └── question_bank/
│   │       └── data/ielts/cambridge21/    # 仅剑雅 21 题库 (48 JSON 文件)
│   └── frontend/
│       ├── landing/                         # 营销首页 (Cyan 主题)
│       └── dashboard/                       # 雅思做题工作台
│           ├── index.html                    # 入口 (含 demo 横幅提示)
│           ├── css/dashboard.css
│           └── js/
│               ├── auth.js                  # 拦截 API 请求 → 静态 JSON 文件
│               ├── dashboard.js             # 主入口
│               └── modules/                 # 12+ 个业务模块
├── .gitignore
├── LICENSE                                  # MIT
└── README.md                                # 本文件
```

## 技术实现

- **静态拦截层**：`auth.js` 中的 `apiFetch()` 将 `/api/v1/question-bank/*` 请求映射到本地 JSON 文件，其他 API 返回空 200 响应
- **空状态替换**：所有 `renderEmptyState()` 在 demo 模式下显示 "在线体验版" 提示，避免页面出现"暂无数据"的空状态
- **顶部横幅**：每页顶部展示 demo 提示，引导用户到 GitHub 下载完整版
- **缓存版本**：所有静态资源统一带 `?v=20260720b` 强制浏览器刷新

## 与完整版的差异

| 维度 | 在线体验版 (本项目) | 完整版 (dr-ielts-open) |
|:---|:---|:---|
| 后端 | 无 | FastAPI + LangGraph 多 Agent |
| 题库 | 仅剑雅 21 (288 题) | 剑雅 9-21 (3,868 题) |
| 数据存储 | 不保存 | SQLite + Mem0 双层记忆 |
| AI 能力 | 不支持 | 12 Agent + 3 协作模式 |
| 训练计划 | 不支持 | 5-tier 分层训练体系 |
| 错题归因 | 不支持 | 4 维度评分 + 错因标签 |
| 用户认证 | 无 (体验访客) | JWT + 多用户 |

## License

MIT License — 详见 [LICENSE](LICENSE)
