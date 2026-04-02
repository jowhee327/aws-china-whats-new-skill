# AWS China What's New Skill

An [OpenClaw](https://github.com/openclaw/openclaw) Agent Skill for querying AWS China Region (Beijing / Ningxia) service launch announcements.

## Features

- 🔍 **Query** — Search if a specific AWS service is available in China regions
- 📅 **Timeline** — See when a service launched in Beijing or Ningxia
- 📊 **Summary** — List all services launched in a specific year
- 🔔 **Monitor** — Set up cron jobs to watch for new service launches

## Data Source

- `https://www.amazonaws.cn/en/new/{year}/` (English, 2016-present)
- `https://www.amazonaws.cn/cn/new/{year}/` (Chinese, 2016-present)

## Usage

### Fetch data
```bash
# Full fetch (all years)
python3 scripts/fetch_data.py

# Incremental update (current year only)
python3 scripts/fetch_data.py --incremental
```

### Query
```bash
# Search by service name
python3 scripts/query.py --service "Lambda"

# Filter by region
python3 scripts/query.py --service "EKS" --region beijing

# Filter by year
python3 scripts/query.py --year 2024 --limit 10

# Latest announcements
python3 scripts/query.py --latest
```

## Install as OpenClaw Skill

Copy the skill directory to your OpenClaw skills folder:
```bash
cp -r . ~/.openclaw/skills/aws-china-whats-new
```

Then run `python3 scripts/fetch_data.py` to populate the data.

## Requirements

- Python 3.6+ (standard library only, no pip install needed)

## License

MIT
