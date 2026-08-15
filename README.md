# FrostMart UK — Inventory Optimization & Wastage Analytics
A comprehensive inventory optimization and revenue protection analytics solution built for FrostMart UK, transforming retail stock, sales, and weather data into an interactive, multi-view Power BI dashboard. This project establishes a single source of truth for executive leadership and procurement teams to mitigate perishable inventory losses across 800+ UK stores. 

## Table of Contents
- [Overview](#overview)
- [Project Brief and Problem Statement](#Project-Brief-and-Problem-Statement)
- [Data Pipeline and Architecture](#Data-Pipeline-&-Architecture)
- [Data Model & Relationships](#Data-Model-&-Relationships)
- [Core DAX Measures & Formulas](#Core-DAX-Measures-&-Formulas)
- [Dashboards & Visualizations](#Dashboards-and-Visualizations)
- [Key Business Insights](#Key-Business-Insights)
- [Strategic Recommendations](#Strategic-Recommendations)
- [Tech Stack](#Tech-Stack)
- Author
  
##Overview 
FrostMart UK is a national retail chain established in 1992, operating over 800 stores across the United Kingdom and specializing in affordable fresh produce, dairy, bakery, and seasonal goods. With seasonal perishable items driving nearly 35% of annual revenue, effective stock management is critical to protecting margins. 
This project establishes an analytics framework by integrating transaction, supplier, store, and weather data directly into a Power BI star schema model without needing data cleaning, enabling leadership to track perishable waste drivers, evaluate cold storage limits, and optimize stock levels against weather patterns. 

## Project Brief and Problem Statement
Problem Statement FrostMart UK faced £12.2 million in annual inventory losses due to gut-feeling planning. Overstocking led to £5.2M in spoilage and markdowns, while understocking caused £7.0M in missed sales during peak demand periods. 
Project Objectives
	Centralize Financial Visibility: Track expected revenue, total revenue, wastage cost, and total units sold across UK regions. 
	Environmental & Seasonal Tracking: Measure the impact of rainfall, average weekly temperatures, and holiday weeks on product demand. 
	Supply Chain & Cold Storage Profiling: Evaluate supplier lead times, capacity limits, and cold storage capacity against regional product wastage. 
	Targeted Loss Mitigation: Identify high-spoilage products and vendors to guide stock procurement and inventory policies. 
⚙️ Data Pipeline & Architecture
[ Source CSV Tables ] ➔ [ Direct Import & Model Structuring ] ➔ [ Relational Star Schema ] ➔ [ Interactive Power BI Report ]
Data Integration & Preparation Steps The project integrated five pre-structured CSV tables directly into Power BI, focusing straight on schema connections, measure creation, and visual reporting: 
	Direct Data Import: Imported weekly_sales.csv, product_details.csv, store_info.csv, weather_data.csv, and supplier_info.csv into Power BI. 
	Zero Cleaning Workflow: Datasets arrived fully pre-cleaned and standardized, enabling immediate star schema relationship mapping without transformation steps. 
	Custom Binning & Slicers: Applied calculated parameters and dynamic UI metric selectors (Selected Metric) for dynamic visual switching. 
📐 Data Model & Relationships
The data model uses a Star Schema connecting central weekly sales transaction records to descriptive dimension tables: 
	fact_sales: Central fact table storing weekly sales, price, marketing spend, discount percentages, and wastage metrics. 
	dim_store: Store dimension table provided in the source files, containing store locations (Store_ID), regions (Region), cold storage capacity (Cold_Storage_Capacity), and storage group tiers (Cold_Store_Group). 
	dim_product: Product catalog lookup containing Product_ID, Product_Name, Product_Category, Shelf_Life_Days, and foreign key Supplier_ID. 
	dim_supplier: Vendor lookup containing Supplier_ID, Supplier_Name, Lead_Time_Days, and Supply_Capacity. 
	dim_weather: Environmental condition lookup mapping temperature, rainfall, and holiday indicators by region and week. 
	Selected Metric: Disconnected dynamic parameter table supporting user-driven field switching across visual charts.
📊 Dashboards & Visualizations
🩺 Dashboard 1 — Executive Summary
Delivers dynamic financial health overview, total revenue tracking, revenue lost to wastage, and high-level regional performance.
	KPI Cards: Expected Revenue (£227.7M), Total Revenue (£205.7M / 90.3% realization), Total Wastage (£15.7M / 6.9% loss), and Total Units Sold (61.5M). 
	Financial Category & Regional Distribution: Expected Revenue broken down by Category (led by Dairy at £72M and Meat at £55M) and Region (led by London at £76M and South West at £46M).
	Temporal Patterns: Revenue distribution across Week Type (Normal Weeks £186M vs. Holiday Weeks £47M) and monthly trends (peaking in January at £23.4M and August at £21.4M).
🌧️ Dashboard 2 — Environmental Impact
Maps external weather patterns and holiday occurrences against regional revenue and product category demand.
	Environmental KPIs: Average Rainfall (20.85 mm), Average Temperature (10.02°C), and Total Holiday Weeks (10 weeks).
	Weather & Temperature Drivers: Revenue mapped across Average Temperature Groups (peaking at £147M in 11–19°C) and Rainfall Groups (highest at £201M in 10–19 mm range).
	Product-Level Weather Sensitivity: Monthly temperature trend tracking alongside product-level expected revenue under varying weather conditions.
🚚 Dashboard 3 — Supply Chain Operations (2 Pages)
Evaluates supplier performance, lead times, cold storage constraints, and top spoilage-driving products via multi-page navigation.
	Supply Chain Overview (Page 1): Highlights 10 Total Suppliers, 48 Total Products, and an Average Lead Time of 2 Days. Compares Expected Revenue by Supplier (led by TrustedSource Provisions at £35M) against weekly Supply Capacity (led by FarmDirect Suppliers Ltd. at 72K units). Tracks wastage across Cold Storage Capacity groups (highest wastage at 2.2M units in 1000–1999 capacity stores).
	High Spoilage & Wastage Breakdown (Page 2): Isolates the top 15 products driving inventory waste, led by White Sandwich Loaf (£531.0K waste / 229.9K units), Donuts 8-pack (£551.0K waste / 229.9K units), and Croissant 4-pack (£548.6K waste / 228.8K units), totaling £7.41M across top waste items.
🧮 Core DAX Measures & Formulas
"Total Units Sold"="SUM" ("fact_sales" ["Units_Sold" ])
"Total Revenue"="SUMX" ("fact_sales" ,"fact_sales" ["Units_Sold" ]×"fact_sales" ["Price" ])
"Total Wastage Cost"="SUMX" ("fact_sales" ,"fact_sales" ["Wastage_Units" ]×"fact_sales" ["Price" ])
"Expected Revenue"="Total Revenue"+"Total Wastage Cost" 
"Wastage Rate (%)"=("Total Wastage Cost" /"Expected Revenue" )×100
📈 Key Business Insights
Financial & Regional Insights
	Revenue Realization & Loss: FrostMart generated £205.7M in total revenue out of £227.7M expected income, losing £15.7M (6.9% of total expected income) directly to product wastage. 
	Dominant Categories & Regions: Dairy (£72M expected revenue) and Meat (£55M expected revenue) form the core of sales. London is the top revenue region at £76M, followed by the South West (£46M) and Midlands (£42M). 
	Holiday Spike Demand: Holiday weeks generate £47M in expected revenue across just 10 holiday weeks, demonstrating intense demand concentration during festive periods. 
Supply Chain & Weather Impact
	Cold Storage Bottlenecks: Stores with mid-tier cold storage capacities (1000–1999) suffered the highest waste volume at 2.2M units, indicating that capacity limitations directly drive spoilage during peak shipments.
	Short Shelf-Life Vulnerability: Bakery products with 2 to 4-day shelf lives (e.g., White Sandwich Loaf, Donuts, Croissants) dominate the top waste lists, generating over £7.4M in combined wastage cost.
	Weather Sensitivity: Sales peak significantly when weekly temperatures range between 1–19°C and moderate rainfall (10–19 mm) occurs, whereas extreme cold (-5 to 0°C) reduces expected revenue to £9M.
❓ Business Questions Answered
Brief Question	Data-Backed Finding
What is overall financial health and waste loss?	£205.7M revenue realized; £15.7M lost to wastage (6.9% total loss rate). 
Which categories drive the highest revenue and waste?	Dairy (£72M) and Meat (£55M) lead revenue; Bakery items dominate total waste volume. 
How do temperature and rainfall impact sales volume?	Peak sales occur at 11–19°C (£147M) and 10–19 mm rainfall (£201M); extreme cold lowers sales to £9M.
Do store cold storage limits correlate with higher waste?	Yes, stores with 1000–1999 cold storage capacity experienced the highest wastage (2.2M units).
Which suppliers/products carry the highest spoilage rates?	Bakery suppliers (TrustedSource Provisions, LocalHarvest Distributors) driving top waste items like Loaves, Donuts, and Croissants (£7.41M combined top 15 waste). 
💡 Strategic Recommendations
	Dynamic Weather-Based Replenishment: Integrate weekly weather forecasts into store ordering algorithms, reducing perishable orders by 15–20% during predicted extreme cold spells (-5 to 0°C).
	Cold Storage Infrastructure Expansion: Upgrade refrigeration capacity in stores falling within the 1000–1999 capacity group to alleviate bottleneck-driven spoilage.
	Bakery Vendor Lead-Time Optimization: Shift high-spoilage short shelf-life bakery items (2–4 day shelf life) to daily micro-deliveries rather than large batch shipments from suppliers like TrustedSource Provisions and LocalHarvest Distributors. 
	Holiday Stock Allocation Safeguards: Establish buffer stock thresholds leading into key holiday weeks to capture maximum demand without over-allocating short shelf-life items.
⚙️ Tech Stack
	Data Transformation & Modeling: Microsoft Power BI Desktop, DAX, Star Schema Data Modeling 
	Reporting & UI: Dynamic Slicers, Visual Field Parameters, Custom UI Navigation Buttons, Microsoft PowerPoint 
👨‍💻 Author
Emmanuel Sey Williams
📧 williamsemmanuel7382@gmail.com
📅 2026 | 📊 Project Status: Complete

