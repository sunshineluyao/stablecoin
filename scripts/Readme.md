# 🛠️ Scripts & API Integration

This folder contains all reproducible scripts and data interface examples that power the empirical and visual analyses in:

> **📄 SoK: Stablecoins for Digital Transformation — Design, Metrics, and Application with Real World Asset Tokenization as a Case Study**

---

## 🔎 Overview

This repository combines **on-chain data**, **off-chain analytics**, and **real-time financial metrics** to offer a comprehensive empirical case study in stablecoin and tokenized real-world asset (RWA) infrastructure.  

The scripts and notebooks demonstrate reproducible workflows that connect the following ecosystems:

| Data Type       | Source               | API Layer       | Use Case                                |
|-----------------|----------------------|------------------|------------------------------------------|
| 🧾 On-chain TXs | Ethereum Mainnet     | `web3.py`        | Contract traces, token mints/burns       |
| 📊 Market Data  | Coin Metrics         | `REST API`       | Time-series metrics, velocity, supply    |
| 🧠 DeFi Yields  | DeFiLlama            | `JSON API`       | APY, TVL, pool metadata                  |

---

## 🔌 APIs & Tools

| Tool             | Logo             | Purpose                                 |
|------------------|------------------|------------------------------------------|
| Coin Metrics     | 🧮               | Off-chain market metrics (e.g., USDC velocity, supply age) |
| web3.py          | 🔗               | Ethereum contract + event decoding       |
| DeFiLlama        | 🐙               | DeFi APY, TVL, RWA yield pool mapping    |
| Plotly           | 📊               | Sankey & circular token flow visualizations |

---

## 🧠 Conceptual Primer

Stablecoin flows typically span both **on-chain** (e.g., token transfers, smart contracts) and **off-chain** (e.g., exchanges, oracles, analytics) infrastructures. Our scripts use APIs to bridge this gap:

- 🔗 **On-chain**: Ethereum RPC queries resolve raw contract state and transfer events.
- 🌐 **Off-chain**: Coin Metrics, DeFiLlama, and similar APIs contextualize blockchain data with macro-level or economic metadata.

Together, these enable rigorous, hybrid financial analysis in DeFi and stablecoin research.

---

## 📘 Tutorials

### ✅ 1. Query Coin Metrics

```python
import requests

metric = "AdrActCnt"
asset = "usdc"
start = "2023-01-01"
end = "2023-12-31"
api_key = "YOUR_COINMETRICS_API_KEY"

url = f"https://api.coinmetrics.io/v4/timeseries/asset-metrics?assets={asset}&metrics={metric}&start_time={start}&end_time={end}"

headers = {"Authorization": f"Bearer {api_key}"}
resp = requests.get(url, headers=headers).json()

for row in resp["data"]:
    print(f"{row['time']} → {row[metric]}")
````

📌 *Popular metrics*: `AdrActCnt`, `SplyCur`, `TxTfrCnt`, `NVTAdj`.

---

### ✅ 2. Query Ethereum Events via Web3.py

```python
from web3 import Web3

web3 = Web3(Web3.HTTPProvider("https://eth-mainnet.g.alchemy.com/v2/YOUR_KEY"))

contract = web3.eth.contract(address="0xYourContract", abi=your_abi)
from_block = 19000000

events = contract.events.Transfer.createFilter(fromBlock=from_block).get_all_entries()
for e in events:
    print(e["args"]["from"], "→", e["args"]["to"], ":", e["args"]["value"])
```

📌 *Use cases*: token transfers, LP deposits, mint/burn tracking.

---

### ✅ 3. Query DeFiLlama for RWA Yield Pools

```python
import requests

url = "https://yields.llama.fi/pools"
response = requests.get(url)
data = response.json()

for pool in data["data"]:
    if "real yield" in pool.get("project", "").lower():
        print(pool["project"], pool["symbol"], f"{pool['apy']}%")
```


