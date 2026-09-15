# Samsung Supply Chain & Logistics Analytics Dashboard

An end-to-end Power BI dashboard analyzing Samsung's simulated global supply chain — from supplier procurement through inventory, shipment logistics, and final sales across commercial channels.

![Power BI](https://img.shields.io/badge/Power%20BI-F2C811?style=flat&logo=powerbi&logoColor=black)
![DAX](https://img.shields.io/badge/DAX-217346?style=flat)
![Status](https://img.shields.io/badge/Status-Complete-brightgreen)

---

## 📌 Project Note

This project was built by following a guided tutorial to strengthen my Power BI and DAX skills. The dataset is **AI-generated (synthetic)**, not scraped or sourced from Samsung's real business data — it exists purely to simulate realistic supply chain relationships (suppliers, inventory, shipments, sales) for analytics practice.

What's genuinely mine in this project: the data modeling decisions, the DAX measures, the dashboard structure and page layout, and the analysis of what the data shows. I'm noting the tutorial/synthetic-data origin upfront so this is presented accurately.

---

## 🎯 Project Overview

The dashboard tracks the full operational pipeline for a simulated Samsung electronics supply chain:

**Raw material sourcing → Factory assembly → Warehouse inventory → Shipping logistics → Retail/e-commerce sales**

Goal: give supply chain stakeholders a single source of truth to spot bottlenecks (supplier delays, stock shortages, shipment failures) and connect them to downstream revenue and profit impact.

---

## 🗂️ Data Model

**Star schema** with 3 fact tables and 4 dimension tables:

| Fact Tables | Dimension Tables |
|---|---|
| Fact Sales | Dim Products |
| Fact Inventory | Dim Suppliers |
| Fact Shipment | Dim Customers |
| | Dim Date |

- All relationships are **1-to-many, single-direction** — no bi-directional filters or unindexed many-to-many joins, to keep DAX evaluation performant.
- Data ingested via a single **folder-load Power Query connection** across 7 relational CSVs (Sales, Inventory, Shipment, Suppliers, Customers, Products, Date).
- A parameterized `folder_path` variable in Power Query lets the source location be reconfigured per environment without breaking query dependencies.
- Custom `MMM` month-abbreviation column sorted by a numeric `Month Number` column, to fix chronological ordering on line charts (Power BI sorts text months alphabetically by default).

---

## 📊 Dashboard Pages

### 1. Home / Navigation
Landing page with brand visuals and button-based navigation to each analysis page.

### 2. Executive Overview
Top-level KPIs — Total Revenue, Gross Revenue, Profit, Profit Margin %, Perfect Order %, Total Shipments — organized into 4 sections mirroring the supply chain: Supplier Procurement, Inventory/Manufacturing, Shipment Logistics, Customer Sales.

### 3. Supplier Performance Analytics
Evaluates 7 suppliers on Total Unit Cost, Order Quantity, Average Quality Score, and Average Lead Time. Geographic breakdown by supplier country/city (China shows the highest lead times in this dataset). Field Parameters let the user toggle the line chart between Total Cost and Order Quantity.

### 4. Inventory & Production Analytics
Combines defect tracking with warehouse stock control: Stock Levels, Safety Stock, Reorder Point, Inventory Turnover Rate, Days of Inventory, Defective Unit Counts, Average Defect Rate. Monthly defect trend shows December as the peak and September as the lowest.

### 5. Shipment & Logistics Analytics
Total Shipments, Quantity Shipped, Delivery Success Rate (75% in this dataset), Shipment Costs, Courier Performance. Breaks down shipment status (In Progress / Delayed / Delivered) and delay root causes (Courier Capacity, Documentation Issues).

### 6. Customer & Commercial Analytics
Revenue performance by platform (Amazon, Flipkart) and channel (Online, Retailer, Direct). Net Revenue, Gross Revenue, Profit, Profit Margin %, Discount Amount/%, YoY Revenue Growth. Bubble chart comparing sales volume, revenue, and discount % across product categories (Smartphones, TVs, Audio/Buds).

---

## 🧮 Key DAX Measures

```dax
Total Revenue = SUM(Sales[Net Revenue])
Total Profit = SUM(Sales[Profit])
Profit Margin % = DIVIDE([Total Profit], [Total Revenue])
Discount % = DIVIDE([Discount Amount], [Total Product Amount])
YoY Revenue Growth % = 
    VAR CurrentRevenue = [Total Revenue]
    VAR PriorRevenue = CALCULATE([Total Revenue], SAMEPERIODLASTYEAR('Date'[Date]))
    RETURN DIVIDE(CurrentRevenue - PriorRevenue, PriorRevenue)

Average Lead Time = AVERAGE(Supplier[Lead Time])
Perfect Order % = DIVIDE([Delivered Non-Defective Orders], [Total Shipments])
Delivery Rate % = DIVIDE([Delivered Shipments], [Total Shipments])
Inventory Turnover Rate = DIVIDE([Sales Quantity], [Current Stock Level])
Days of Inventory = DIVIDE(365, [Inventory Turnover Rate])
Reorder Point = SUM(Inventory[Reorder Point])
```

---

## 🎨 Technical & UI/UX Details

- Custom JSON theme applied for consistent brand colors across all pages
- Custom image slicers using product thumbnails (Galaxy S24 Ultra, S23, Galaxy Buds) alongside standard button slicers
- Dynamic Field Parameters to swap chart measures without adding extra visuals
- Collapsible side filter panel to maximize report canvas space

---

## 🛠️ Tools Used

- **Power BI Desktop** — data modeling, DAX, report design
- **Power Query (M)** — data ingestion and transformation
- **DAX** — calculated measures and KPIs

---

## 🔍 What I'd Improve Next

- Replace synthetic data with a real open dataset to validate the model against messier, less clean data
- Add row-level security (RLS) for a multi-region stakeholder view
- Build out a incremental refresh setup to simulate a production data pipeline

---

## 📬 Contact

Feel free to connect or reach out with feedback.
