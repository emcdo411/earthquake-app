# 📊 Earthquake Anomaly Detection System

> Built with R + Shiny | JSON-Based Sensor Simulation | AI-Powered Anomaly Detection

---

## 🔍 [Overview](#overview)

This Shiny application simulates a real-time earthquake monitoring system that:

* Ingests JSON-based sensor data
* Uses Isolation Forests to detect anomalies
* Visualizes anomalies using 2D/3D plots and maps
* Features a modern, interactive dashboard (now using **Leaflet** instead of `echarts4r`)

This project was rebuilt and debugged with classmate **Myles Merriweather** in mind to show the evolution from early bugs to a stable, production-ready app.

---

## 🛠️ [What Was Improved](#what-was-improved)

### ✅ Switched to JSON instead of .CSV

* JSON allowed structured, hierarchical data simulation via PowerShell.
* CSV was causing parsing errors and lacked the flexibility needed for nested simulation.

### ✅ Map Tab Refactored

* **echarts4r** map was unstable and produced `must pass serie` errors.
* Switched to **Leaflet** for precise latitude/longitude plotting with color-coded risk levels and tooltips.

### ✅ UTF-8 Fix

* JSON files created in Windows often include a Byte Order Mark (BOM) which crashes RStudio's `fromJSON()`.
* Added a line to strip the BOM for consistent UTF-8 parsing.

### ✅ Core Logic Debugged

* Missing tildes, bad symbol bindings, missing columns — all repaired.
* Errors handled gracefully with `tryCatch()` and `validate(need(...))`.

---

## ⚙️ [Tech Stack](#tech-stack)

### 📦 Packages

<div align="center">
  <img src="https://img.shields.io/badge/RStudio-IDE-blue" />
  <img src="https://img.shields.io/badge/Shiny-Dashboard-orange" />
  <img src="https://img.shields.io/badge/jsonlite-JSON-green" />
  <img src="https://img.shields.io/badge/leaflet-Maps-lightblue" />
  <img src="https://img.shields.io/badge/ggplot2-Graphs-blueviolet" />
  <img src="https://img.shields.io/badge/isotree-AI-red" />
  <img src="https://img.shields.io/badge/plotly-3D-lightgreen" />
  <img src="https://img.shields.io/badge/DT-Tables-yellow" />
  <img src="https://img.shields.io/badge/shinyjs-JavaScript-lightgrey" />
  <img src="https://img.shields.io/badge/reshape2-Correlation-cyan" />
</div>

---

## 💾 [Simulated JSON via PowerShell](#simulated-json-via-powershell)

```powershell
# PowerShell script to simulate 200 rows of earthquake data and save as JSON
$data = @()
1..200 | ForEach-Object {
  $base = Get-Random -Minimum 0.3 -Maximum 0.7
  $row = [PSCustomObject]@{
    Timestamp = (Get-Date).AddMinutes(-($_ * 10)).ToString("yyyy-MM-dd HH:mm:ss")
    Sensor1   = [math]::Round($base + (Get-Random -Minimum -0.05 -Maximum 0.05), 2)
    Sensor2   = [math]::Round($base + (Get-Random -Minimum -0.04 -Maximum 0.04), 2)
    Sensor3   = if ($_ % 40 -eq 0) { Get-Random -Minimum 2.5 -Maximum 4.0 } else { [math]::Round($base + (Get-Random -Minimum -0.06 -Maximum 0.06), 2) }
    Sensor4   = if ($_ % 40 -eq 0) { Get-Random -Minimum 2.5 -Maximum 4.0 } else { [math]::Round($base + (Get-Random -Minimum -0.06 -Maximum 0.06), 2) }
    Sensor5   = [math]::Round($base + (Get-Random -Minimum -0.05 -Maximum 0.05), 2)
    Magnitude = if ($_ % 40 -eq 0) { Get-Random -Minimum 4.0 -Maximum 7.0 } else { Get-Random -Minimum 0.5 -Maximum 2.0 }
    Latitude  = Get-Random -Minimum 55.0 -Maximum 65.0
    Longitude = Get-Random -Minimum 10.0 -Maximum 20.0
  }
  $data += $row
}
$data | ConvertTo-Json -Depth 3 | Out-File -Encoding UTF8 earthquake_data.json
```

---

## 🧠 [Why I Chose R Over Python](#why-i-chose-r-over-python)

* R has native support for statistical models like Isolation Forest via `{isotree}`.
* `{shiny}` provides a GUI framework that feels more like PowerBI or Tableau.
* Easier integration with reactive plots, dynamic filtering, and tabbed dashboards.

---

## 📋 [Final `app.R`](#final-app-r)

<details>
<summary>Click to expand</summary>

```r
# Your app.R code here
```

</details>

---

## 🗺️ [Mermaid Workflow](#mermaid-workflow)

```mermaid

flowchart TD
  Start["Start App"] --> Upload{Upload JSON or Simulate?}
  Upload -- Simulated --> Sim[Generate 200 rows]
  Upload -- Uploaded --> Valid[Validate JSON + UTF-8 fix]
  Sim --> Forest[Anomaly Detection (Isolation Forest)]
  Valid --> Forest
  Forest --> Plot1[Anomaly Score Stream (ggplot2)]
  Forest --> Plot2[Heatmap (reshape2)]
  Forest --> Plot3[3D Plot (plotly)]
  Forest --> Plot4[Map (leaflet)]
  Forest --> Table[Historical Table (DT)]

---

## 💡 [Suggestions to Improve the App Further](#suggestions-to-improve-the-app-further)

* Add real-time polling using `invalidateLater()`
* Connect to USGS live API for real earthquake feed
* Replace leaflet popups with modal windows
* Add download button for filtered data
* Add dark/light theme toggle using `bslib`
* Create an R package out of this for reuse

---

## 🧾 [Conclusion](#conclusion)

If you’re new to software or data development:

* This project showcases how R can build fully interactive dashboards
* Even without front-end experience, Shiny helps data analysts prototype apps fast
* The switch from CSV to JSON, using AI models, debugging R errors — these are common but solvable problems

> The biggest takeaway? **Debugging is learning.** Every crash helped harden this app into something shareable.

Let me know what you think, Myles!

---
