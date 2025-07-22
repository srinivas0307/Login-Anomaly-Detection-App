# **Login Anomaly Detection – Splunk App**

## **Overview**

This Splunk app detects **anomalous login behavior** by analyzing CIM-compliant authentication logs using **KMeans clustering** in Splunk's **Machine Learning Toolkit (MLTK)**. The app identifies:

- High login failure accounts (possible brute-force)
- Impossible travel logins (based on geolocation/time)
- Risky users for investigation

It includes real-time **alerts**, detailed **dashboards** for **visualizations**.

---

## **Features**

- KMeans model trained on `failure_ratio`, `login_hour`, `total_logins`, `unique_ip_count`
- Real-time scoring and risk tagging
- Alerts for:   High-risk login clusters (Cluster 0) and Impossible travel logins
- Dashboard with 7 panels for visualization
- Fully packaged Splunk app compatible with CIM logs

## **Dashboard Panels**

| Panel Title | Type |
| --- | --- |
| Login Activity Monitor | Area Chart |
| Top Users by Login Volume | Bar Chart |
| Login Heatmap by Hour | Heatmap |
| Anomalous vs Normal | Bar Chart |
| Failure Ratio (Anomalous Only) | Scatter |
| Anomaly Trend Over Time | TimeChart |
| Top Risky Users | Table |



## Setup and Installation

### 1. Prerequisites

- **Splunk Enterprise** (v8.0 or later recommended)
- **Splunk Machine Learning Toolkit (MLTK)** installed
- Admin access to Splunk Web
- Optional: Access to login-related logs (with fields like `user`, `ip`, `action`, `_time`)

### 2. Installation

#### Method A: From Splunk Web UI

1. Go to **Apps → Manage Apps → Install app from file**
2. Upload the file `login_anomaly_detection_app.spl`
3. Click **Upload**
4. Restart Splunk if prompted

#### Method B: Manual Installation (Linux)

```bash
# Move the SPL package into the apps directory
sudo cp login_anomaly_detection_app.spl /opt/splunk/etc/apps/

# Extract the app
cd /opt/splunk/etc/apps/
tar -xvzf login_anomaly_detection_app.spl

# Restart Splunk
sudo /opt/splunk/bin/splunk restart
```

---

## Configuration

### 1. Configure Data Inputs

Ensure your Splunk index contains login logs with the following fields:
- `user`
- `ip`
- `action` (e.g., success/failure)
- `_time`

Example SPL search used for feature extraction:
```spl
index=main sourcetype="your_login_data"
| eval login_hour = tonumber(strftime(_time, "%H"))
| stats count as total_logins, 
        dc(ip) as unique_ips, 
        sum(eval(action="failure")) as failures 
  by user, login_hour, _time
| eval failure_ratio = failures / total_logins
```

### 2. Model Configuration

The app includes a pre-trained KMeans model:
- `__mlspl_login_anomaly_kmeans_model.mlmodel` (stored in `lookups/`)
- Automatically applied via `apply` command in saved searches and dashboards

---


##  Dependencies

- Splunk MLTK (Machine Learning Toolkit)
- Base Splunk installation (Enterprise or Free)
- Your login logs indexed in Splunk

## File Structure

```
login_anomaly_detection_app/
├── appserver/
│   └── static/
│       └── my_dashboard.xml
├── metadata/
│   ├── default.meta
│   └── local.meta
├── default/
│   ├── app.conf
│   ├── savedsearches.conf
│   └── data/
│       └── ui/
│           ├── nav/
│           │   └── default.xml
│           └── views/
│               ├── login_anomaly_detection_dashboard.xml
│               └── README
├── bin/
│   └── README
└── lookups/
    └── __mlspl_login_anomaly_kmeans_model.mlmodel
```


---





