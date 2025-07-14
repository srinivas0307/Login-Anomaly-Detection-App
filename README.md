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
