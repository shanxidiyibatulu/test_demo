# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## 项目概述

知识库文件导入前端 — 单页面 HTML 应用，提供 PDF/MD 文件上传到后端知识库系统的 UI 界面。

## 架构

```
import.html          # 唯一的源文件，包含 HTML/CSS/JS
```

前端通过两个 API 与后端通信：

| 接口 | 方法 | 请求 | 响应 |
|------|------|------|------|
| `/upload` | POST | FormData (file) | `{task_id: string}` |
| `/status/{task_id}` | GET | — | `{status, done_list, running_list, durations}` |

后端地址硬编码在 `API_BASE = 'http://127.0.0.1:8000'`（第 402 行）。

## 关键逻辑

- **节点计数**：PDF 文件 8 步（含 `pdf_to_md`），MD 文件 7 步。`totalNodes` 决定进度百分比计算（第 451-453 行）。
- **进度计算**：`done.length / totalNodes * 100`，运行中的节点额外贡献 `+0.5/totalNodes`（第 553 行）。
- **轮询**：每 1.5 秒轮询 `/status/{task_id}`，直到返回 `completed` 或 `failed`（第 607 行）。
- **状态字段**：后端返回的 `status` 取值：`processing` | `completed` | `failed`。
- **`upload_file` 步骤**在日志渲染时被跳过显示（第 576 行）。

## 开发方式

无构建工具、无包管理、无测试框架。直接编辑 `import.html`，在浏览器中打开即可预览。需要后端运行在 `127.0.0.1:8000` 才能完整体验上传流程。
