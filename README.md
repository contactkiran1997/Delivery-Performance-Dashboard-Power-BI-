# MNC Company — Delivery Performance Dashboard (Power BI)

A Power BI dashboard built for ** The Car Wash Factory** to track and analyze **On-Time Delivery (OTD)**, **Order Fill Rate**, and **Line Fill Rate** performance across Manufacturing plants, Parts plants, and Distribution Center (DC) warehouses — with YoY trend tracking and drill-through to sales order-level detail.

---

## 📌 Project Overview

Sonny's operates multiple manufacturing plants, parts plants, and DC/parts warehouses (US and Canada) that ship orders to customers against requested delivery dates. Leadership needed a single, interactive view to answer:

- Are we hitting our **>95% target** for On-Time Delivery and Fill Rate?
- Which **plants / shipping locations / warehouses** are underperforming?
- How has performance trended **month-over-month and year-over-year**?
- What is the **root cause** at the sales-order line level when a plant misses target?
- What's *achievable* (Attainable OTD) once controllable exceptions are factored out?

This dashboard consolidates ERP shipment data into a single Power BI model with slicers, KPI tiles, trend charts, conditional-formatted summary tables, and order-level drill-through.

---

<img width="1427" height="762" alt="image" src="https://github.com/user-attachments/assets/399734df-004f-42a7-9cde-4bb9e3a67696" />


## 🎯 Business Problem

| Problem | Impact |
|---|---|
| Delivery performance data was scattered across multiple ERP/export reports (Order, Line, SCWS) with no unified view | No single source of truth; manual Excel reconciliation every week |
| No visibility into **YoY trends** — only current month snapshots were available | Couldn't tell if performance was improving or degrading |
| No way to isolate **plant-level vs. DC-level** issues | Root-cause analysis of missed SLAs was slow and reactive |
| No distinction between **raw OTD** and **"Attainable" OTD** (removing customer-caused/uncontrollable delays) | Plants were being penalized for delays outside their control |
| Stakeholders needed to drill from a KPI down to the **exact sales order/line** causing a miss | No drill-through existed; findings required manual VLOOKUPs in raw exports |

---

## ✅ Solution

Built a multi-page Power BI report with:

1. **Landing/Cover Page** — navigation entry point summarizing the 3 delivery views (Order On Time, Line On Time, Line On Time SCWS).
2. **YTD Delivery Metrics (Order-level)** — YoY line chart for Order On-Time % and Order Fill Rate % by month, plus an Operation Shipment Summary and DC Delivery Summary table (conditional-formatted: 🔴 0–88 / 🟡 88–95 / 🟢 97–100).
3. **YTD Delivery Metrics (Line-level)** — same structure at the **line-item** granularity (Line On-Time % / Line Fill Rate %), broken out by DC/plant.
4. **On-Time Delivery Performance | Manufacturing & Parts** — side-by-side tables and combo charts comparing plants (Tamarac, PECO, Savage MN, Tecate CA, etc.) on Order On-Time, Order Fill, Line On-Time, and Line Fill Rate.
5. **SCWS (Ship Complete Warehouse Shipment) View** — dedicated performance breakdown for parts warehouses (Edmonton, Calgary, Indy, Orlando, etc.) with a horizontal bar comparison chart.
6. **Attainable OTD View** — adds "Attainable OOT %" and "Attainable LOT %" columns/series alongside raw OTD, so uncontrollable misses can be filtered out and true plant performance is visible.
7. **Sales-Order Drill-Through** — every summary table links to a **detail table** (Sales Order, Sales Order Line, Order Date, Requested Delivery Date, Last Delivery/PGI Date, Material, Description, Total Delivered) so any red flag can be traced to the exact line item.
8. **Global Slicers** — Delivery Bucket, Classification, Plant, Shipping Method, Month, and Year, synced across all pages, with a "Clear Slicers" button for fast reset.

---

## 🧰 Tools & Tech Stack

- **Power BI Desktop** — data modeling, DAX measures, report design
- **Power Query (M)** — ERP export cleansing/transformation (date parsing, plant/warehouse mapping, status flag logic)
- **DAX** — On-Time / Fill Rate scoring logic, YTD/YoY time intelligence, conditional KPI banding
- **Data Source** — ERP shipment/sales order exports (SAP-style fields: PGI Date, Requested Delivery Date, Sales Order/Line, Material)

---

## 🧮 Key Metric Definitions

| Metric | Definition |
|---|---|
| **Order On-Time** | Binary score (0/100) per order: ship date vs. requested delivery date |
| **Order Fill Rate** | % of ordered quantity fulfilled by the ship date, scored monthly |
| **Line On-Time** | Same scoring logic as Order On-Time, applied at the sales-order-line level |
| **Line Fill Rate** | Same as Order Fill Rate, applied at the line level |
| **Attainable OOT / LOT** | On-Time metrics recalculated after excluding delays outside the plant's control (e.g., customer-requested push-outs) |
| **Target** | >95% (color banded: 🔴 0–88 below target · 🟡 88–95 near target · 🟢 97–100 on target) |

> Current month shows scores through the current date; completed months show the final locked score.

---

## 🧩 Challenges & Solutions

| Challenge | Solution |
|---|---|
| Multiple report grains needed (Order vs. Line vs. SCWS) but had to feel like one cohesive tool | Standardized visual layout/template across pages (same slicer bar, same table conditional formatting scale) so users can navigate between grains without relearning the UI |
| Raw OTD unfairly penalized plants for customer-driven delays | Added a parallel "Attainable OOT/LOT" measure set, computed by excluding orders where the delay reason was flagged as customer/uncontrollable in the source data |
| Users needed to go from "this plant is red" to "this specific order is why" in one click | Implemented drill-through pages ("Details", "Details SCWS", "Attainable Details") passing plant/month/classification filter context down to the order line grain |
| Wide tables (12 months × 2 metrics per plant) were hard to read | Used icon-based conditional formatting (▼ red / ■ yellow / ▲ green) instead of plain numbers, and froze the DC/Plant column for horizontal scroll |
| YoY comparison needed but only one year of clean data existed at build time | Built the YoY chart to support multi-year toggle via a Year slicer, ready to populate as 2026/2027 data lands |

---

## 📊 Dashboard Pages

1. Cover / Navigation
2. YTD Order-Level Delivery Metrics (YoY)
3. YTD Line-Level Delivery Metrics (YoY)
4. On-Time Delivery Performance — Manufacturing & Parts
5. On-Time Delivery Performance — SCWS (Parts Warehouses)
6. On-Time Delivery Performance — Attainable OTD
7. Sales Order Drill-Through Detail

*(See `/screenshots` folder for page previews.)*

---

## 🚀 How to Use

1. Open `Sonnys_Delivery_Metrics.pbix` in Power BI Desktop.
2. Refresh data source connection (update credentials/path in Power Query if reusing with your own ERP export).
3. Use the top slicer bar (Delivery Bucket, Classification, Plant, Month, Year, Shipping Method) to filter any page.
4. Right-click any summary row → **Drill through** → view underlying sales order line detail.
5. Click **Clear Slicers** to reset all filters.

---

## 📁 Repository Structure

```
├── Sonnys_Delivery_Metrics.pbix     # Power BI report file
├── README.md                        # This file
├── screenshots/                     # Dashboard page previews
└── docs/
    └── data-dictionary.md           # Field/measure definitions
```

---

## 👤 Author

Built as a freelance Power BI project — data modeling, DAX, and dashboard/UX design for delivery performance reporting.

---

## 📄 License

This repository shares the dashboard design/structure for portfolio purposes. Underlying business data is illustrative/anonymized and does not represent live production figures.
