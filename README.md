# 🪙 Precious Metals Monitor (Pi-Based Bloomberg Terminal)

A real-time, self-hosted financial dashboard running on a **Raspberry Pi 5**. This project ingests live market data for Gold and Silver, processes currency conversions and weight metrics (Ounces to Grams), and visualizes the data in a "Bloomberg Terminal" style interface using **Grafana** and **InfluxDB**.

![Screenshot_31-1-2026_195348_192 168 31 9](https://github.com/user-attachments/assets/457f4286-220a-48a4-aff7-916c915a108d)


## 🚀 Features

* **Live Spot Pricing:** Fetches Gold (GC=F) and Silver (SI=F) prices every 60 seconds.
* **Multi-Metric Conversion:** Automatically calculates:
    * Gold: 24k (Fine) and 22k (Standard Jewelry) per gram.
    * Silver: .999 (Fine) and .925 (Sterling) per gram.
* **Gold-to-Silver Ratio:** Live calculation of the classic economic indicator with conditional formatting.
* **Dynamic Historical Trends:** Time-series graphs that scale dynamically based on the selected dashboard time window.
* **News Ticker:** Integrated RSS feed scrolling live market news from major financial sources.
* **Data Persistence:** Uses InfluxDB to store historical ticks for long-term trend analysis.

## 🛠️ Tech Stack

* **Hardware:** Raspberry Pi 5 (Compatible with Pi 4/3)
* **Language:** Python 3.11+
* **Data Source:** Yahoo Finance API (`yfinance`)
* **Database:** InfluxDB (Time-series)
* **Visualization:** Grafana
* **OS:** Raspberry Pi OS / Linux

## ⚙️ Installation

### 1. Prerequisites
Ensure you have Python 3 and `pip` installed. You also need a running instance of **InfluxDB v2** and **Grafana**.

### 2. Clone the Repository
```bash
git clone https://github.com/ahmedrashadtxt/gold-and-silver-price-monitor.git
cd gold-and-silver-price-monitor
```

### 3. Set Up Python Environment
It is recommended to use a virtual environment.
```
python3 -m venv venv
source venv/bin/activate
pip install -r requirements.txt
```

requirements.txt contents:
```
yfinance
influxdb-client
pandas
```

### 4. Configuration
Edit the gold_tracker.py script to update your InfluxDB credentials. Note: For production, consider using Environment Variables (os.getenv) instead of hardcoding credentials.
```
INFLUX_URL = "http://localhost:8086"
INFLUX_TOKEN = "YOUR_INFLUX_TOKEN"
INFLUX_ORG = "your-org"
INFLUX_BUCKET = "finance_data"
```

### 5. Running the Tracker
Test the script manually:
```
python3 gold_tracker.py
```

You should see output logs indicating: GOLD: $4713.90 | Change: -12.32% SILVER: $78.29 | Change: -33.01%



## 🤖 Automation (Systemd Service)
To make the tracker run automatically on boot, create a systemd service file.

1. Create the file:
```
sudo vi /etc/systemd/system/gold_tracker.service
```

2. Paste the following configuration (adjust paths to match your setup):
```
[Unit]
Description=Gold & Silver Price Tracker Service
After=network.target

[Service]
ExecStart=/home/youruser/gold-and-silver-price-monitor/venv/bin/python /home/youruser/gold-and-silver-price-monitor/gold_tracker.py
WorkingDirectory=/home/youruser/gold-and-silver-price-monitor
StandardOutput=inherit
StandardError=inherit
Restart=always
User=youruser

[Install]
WantedBy=multi-user.target
```

3. Enable and Start the service:
```
sudo systemctl enable gold_tracker.service
sudo systemctl start gold_tracker.service
```

## 📊 Grafana Dashboard Setup
Install Plugins:

This dashboard uses the Infinity plugin for the RSS News Feed. Install it via Grafana Plugins.

Import Dashboard:

Go to Grafana > Dashboards > Import.

Upload the `dashboard.json` file included in this repository.

Select your InfluxDB data source when prompted.


## 📝 License
This project is open-source and available under the MIT License.


## 🤝 Acknowledgements
Data provided by Yahoo Finance.

Built as a home-lab project to track the Precious Metals market.

### 🌍 Customization
The dashboard is pre-configured for **Dubai (UAE)** timezones and news. 
To change the weather location or news source:
1. Edit the **Weather** panel and update the Latitude/Longitude in the URL.
2. Edit the **News** panel and update the region code (e.g., `gl=US`) in the RSS URL.
