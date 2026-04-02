# AWS 中国区 What's New 查询工具

[English](README.md) | **中文**

一个 [OpenClaw](https://github.com/openclaw/openclaw) Agent Skill，用于查询 AWS 中国区（北京/宁夏）服务落地公告。

## 功能

- 🔍 **查询** — 某个 AWS 服务是否已在中国区落地
- 📅 **时间线** — 查看服务在北京区或宁夏区的落地时间
- 📊 **汇总** — 列出某一年中国区所有新落地的服务
- 🔔 **监控** — 配合 cron 定期检查关注的服务是否落地

## 数据来源

- 英文：`https://www.amazonaws.cn/en/new/{year}/`（2016 至今）
- 中文：`https://www.amazonaws.cn/cn/new/{year}/`（2016 至今）

数据通过解析网页内嵌的结构化 JSON 获取，无需 API Key。

## 使用方法

### 抓取数据

```bash
# 全量抓取（所有年份，首次运行约 30 秒）
python3 scripts/fetch_data.py

# 增量更新（仅当前年份，约 3 秒）
python3 scripts/fetch_data.py --incremental

# 指定年份
python3 scripts/fetch_data.py --year 2025 2026
```

> 💡 查询时会自动检查数据新鲜度：数据不存在会自动全量抓取，超过 24 小时会自动增量更新。

### 查询

```bash
# 按服务名搜索
python3 scripts/query.py --service "Lambda"
python3 scripts/query.py --service "Elastic Kubernetes"

# 按区域筛选
python3 scripts/query.py --service "S3" --region beijing
python3 scripts/query.py --region ningxia --year 2025

# 按时间筛选
python3 scripts/query.py --after 2025-01-01 --before 2025-12-31

# 查看最新公告
python3 scripts/query.py --latest
python3 scripts/query.py --limit 10

# 组合查询
python3 scripts/query.py --service "SageMaker" --region beijing --year 2025
```

### 输出格式

```json
{
  "count": 2,
  "items": [
    {
      "title": "Amazon Lambda adds support for ...",
      "date": "2026-01-09",
      "link": "/en/new/2026/amazon-lambda-adds-support-for-.../",
      "regions": ["beijing", "ningxia"],
      "year": 2026,
      "body_preview": "简短预览..."
    }
  ]
}
```

## 作为 OpenClaw Skill 安装

**方式一：从 ClawHub 安装**
```bash
clawhub install aws-china-whats-new
```

**方式二：手动安装**
```bash
cp -r . ~/.openclaw/skills/aws-china-whats-new
python3 scripts/fetch_data.py  # 首次需要抓取数据
```

安装后，Agent 会自动识别 AWS 中国区服务落地相关的问题，并调用此 Skill 查询。

## 使用场景示例

> 👤 "Lambda 在中国区落地了吗？"
> 🤖 "是的，最近一条相关更新是 2026-01-09：Lambda 支持 .NET 10，北京区和宁夏区都已落地。"

> 👤 "Security Hub 什么时候在中国区上线的？"
> 🤖 "Amazon Security Hub 于 2026-03-31 在北京区和宁夏区正式 GA。"

> 👤 "2024 年有哪些新服务落地中国区？"
> 🤖 "2024 年共有 27 项服务/功能在中国区落地，包括..."

## 技术说明

- Python 3.6+，仅使用标准库，无需 pip install
- 数据存储在 `data/whats_new.json`（约 80KB，865 条记录）
- 区域检测关键词：Beijing/Sinnet/cn-north-1（北京区）、Ningxia/NWCD/cn-northwest-1（宁夏区）

## License

MIT
