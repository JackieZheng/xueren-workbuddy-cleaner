---
id: xueren-workbuddy-cleaner
name: 雪人老师·WorkBuddy 清理专家
title: 雪人老师·WorkBuddy 清理专家
description: WorkBuddy 专用磁盘空间分析工具：自动找出所有与 WorkBuddy 相关的目录（数据目录、工作区、程序安装目录、用户缓存等），按 安全/谨慎/不可删 三级分类，生成直观的报告，提醒用户按报告自行手动清理。当用户说"清理 WorkBuddy""WorkBuddy 占用太大""清理缓存""磁盘空间不够，清理一下 WorkBuddy 相关目录"等意图时触发。
slug: xueren-workbuddy-cleaner
displayName: 雪人老师·WorkBuddy 清理专家
summary: 自动找出所有 WorkBuddy 相关目录，三级分类出报告并呈现结果。
version: 1.3.9
author: 雪人
license: MIT
display_name: xueren-workbuddy-cleaner
trigger: ["清理 WorkBuddy", "WorkBuddy 占用太大", "清理缓存", "WorkBuddy 清理工具", "清理 workbuddy 相关目录", "磁盘不够了清理一下", "workbuddy 空间分析"]
examples: "用户：WorkBuddy 占用太大，帮我清理 → 扫描后 show_widget 内联出回复卡片（指标卡+占比条）+ card 生成完整结果页用 present_files 打开（报告），正文按模板给分档汇总，提醒用户按报告自行手动清理。"
platforms: [ima, WorkBuddy, QClaw]
github: https://github.com/JackieZheng/xueren-workbuddy-cleaner
---

# 雪人老师·WorkBuddy 清理专家

WorkBuddy 专用磁盘空间分析工具：自动找出所有与 WorkBuddy 相关的目录，
按「安全清理 / 谨慎清理 / 不可删除」三级分类出报告。
只读取文件、生成报告，不会改动任何东西；报告标注哪些可清理，
具体删除由用户按报告自行手动进行。

## 标准交互流程（安全模式）

1. `doctor` 自检 → `scan --out workbuddy-cleanup-report.md` 扫描（只读），报告 present_files 打开
2. **呈现扫描结果**：
   - **回复卡片**（对话内嵌）：用 `show_widget` 渲染彩色指标卡（4 指标卡 + 三色占比条），
     与正文同一条回复直接可见
   - **报告**（文件）：`card` 生成 HTML 完整结果页（`assets/result-template.html` 基准）
     → `present_files` 打开
   - **回复正文严格按 `assets/RESULT_TEMPLATE.md` 模板输出**分档汇总
     （数据取 `summary` / `list`），文末提醒用户可自行手动删除 🟢/🟡 档目录

## 命令

```bash
python scripts/wb_cleaner.py doctor                    # 自检
python scripts/wb_cleaner.py scan --out workbuddy-cleanup-report.md  # 扫描报告（只读）
python scripts/wb_cleaner.py card                      # 完整结果页 wb-cleaner-card.html
python scripts/wb_cleaner.py summary                   # 摘要 JSON（指标卡/正文取数）
python scripts/wb_cleaner.py list --tier safe --min-mb 10  # 按分级/体积看明细
```

## 关键限制

- 🔴 运行环境、技能、数据库、凭据、配置、用户档案、程序本体是**硬白名单**，
  报告中一律标为「不可删除」，提醒用户不要手动删除
- 🟢/🟡 档路径是清理建议，供用户按报告自行手动删除参考
  （提醒用户先备份，删除可能无法恢复）
- 🟡 谨慎档包含旧会话与历史记录，删除后会丢失历史回溯能力，提醒用户确认后再删
- `doctor` 会标注每个目录的定位来源；`list --tier keep` 可查看全部保留项，`list` 按分级/体积看明细
