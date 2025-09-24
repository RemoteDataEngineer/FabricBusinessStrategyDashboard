# FabricBusinessStrategyDashboard
A Fabric Business Strategy Dashboard

![Architecture Diagram for Fabric Business Strategy Dashboard](https://github.com/RemoteDataEngineer/FabricBusinessStrategyDashboard/blob/c7971f2ed9681e1e2a717190b667ce791cb7e399/Remote%20Data%20Engineer%20Fabric%20Architecture%20Diagram.jpg)


📊 Customer Retention & Lifecycle Analytics – Medallion Architecture
This project showcases a modular, interview-ready medallion architecture built on Microsoft Fabric and Power BI. It leverages the Worldwide Importers sample dataset to demonstrate scalable customer retention, churn analysis, and lifecycle storytelling using Lakehouse, Dataflow Gen2, and semantic modeling.

📋  Source - Wide World Importers sample database

🪙 Bronze Layer – Raw Data Ingestion
Lakehouse Name: lkh_WorldwideImportersData Source: Microsoft Fabric sample data – Worldwide Importers

Key Tables & Columns
1. Sale (Fact) - SaleKey, CustomerKey, StockItemKey, DeliveryDateKey, SalespersonKey- Quantity, UnitPrice, TaxRate, TotalIncludingTax, TotalExcludingTax, Profit, OrderDateKey, LineageKey
2. Customer (Dimension) - CustomerKey, WWICustomerID, Customer, BillToCustomer, Category, BuyingGroup, PrimaryContact, PostalCode, ValidFrom, ValidTo, LineageKey
3. Date (Dimension) - Date, DayNumber, Month, CalendarYear, FiscalMonthLabel, ISOWeekNumber
4. City (Dimension) - CityKey, City, StateProvince, Country, Continent, SalesTerritory
5. Stock Item (Dimension) - StockItemKey, StockItem, BuyingPackage, SellingPackage, Brand, Size, LeadTimeDays, Barcode, RecommendedRetailPrice

🥈 Silver Layer – Transformed Snowflake Schema
Dataflow Gen2 Name: dfg2_Fact_Tables Purpose: Transform bronze data into curated, retention-aware fact tables

Tables
tbl_CustFacts - Customer-centric metrics for churn, CLV, loyalty, and lifecycle analysis.

Columns: col_ChurnStatus, col_CLV_Discounted, col_CLV_Engaged, col_CLV_Estimate, col_CustomerAvgSpend, col_CustomerProfit, col_DaysSinceLastInvoice, col_IsChurned, col_LifecycleStatus, col_LoyaltyTier, col_OrderFrequency, col_ProfitMargin, col_RetentionContribution, col_TotalCustomerSpend, CustomerKey

tbl_InvoiceFacts: Invoice-level metrics for retention and churn modeling.

Columns: BillToCustomerKey, CityKey, CustomerKey, DeliveryDateKey, InvoiceDateKey, col_IntervalDaysBetweenFirstInvoiceCurrentInvoice, col_IntervalDaysBetweenInvoiceDateToDeliveryDate, col_MinInvoiceDateKey, ms_CustomerChurnRate, ms_DailyRetentionRate, ms_IsRetained

🥇 Gold Layer – Semantic Measures & Business Logic
This layer contains curated DAX measures used across Power BI dashboards to drive customer retention, lifecycle analysis, and strategic storytelling. Each measure is prefixed with ms_ to denote its modular, governed nature and alignment with enterprise standards.

📐 Measure Definitions & Strategic Purpose

ms_AverageCLV	Average  - Customer Lifetime Value across all customers. Useful for benchmarking and segmentation.
ms_CustomerChurnRate	- Percentage of customers flagged as churned. Calculated as Churned ÷ Total Customers.
ms_CustomerChurnStatus	- Categorical label (e.g., Active, Churned) based on churn logic. Used for filtering and segmentation.
ms_CustomerFirstPurchaseDate	- Earliest invoice date per customer. Supports lifecycle modeling and retention curves.
ms_CustomerLifetimeValue	- Total projected value of a customer based on historical spend and engagement.
ms_CustomersWithMultiplePurchases	- Count of customers with more than one purchase. Indicates engagement depth.
ms_DailyRetentionRate	- Probability of retention on a daily basis. Used in cohort and lifecycle visuals.
ms_IsRetained	- Boolean flag indicating if a customer is retained. Drives retention matrix logic.
ms_RepeatPurchaseCustomers	- Count of customers with repeat purchases. Supports loyalty and behavioral segmentation.
ms_RepeatTarget	- Target benchmark for repeat purchase rate. Used for goal tracking and variance analysis.
ms_TargetRetentionRate	- Strategic retention goal (e.g., 85%). Used for simulation and performance comparison.
ms_TotalCustomers	- Total distinct customers in the filtered context. Used in cards and pie charts.
ms_TotalSales	- Sum of sales revenue across all transactions. Used in trend and product performance visuals.

📈 Presentation - Business Strategy Dashboard: The following infromation is also found in the Glossary Tab of the Power Bi.

🧭 01. Executive Summary – Strategic Overview
This dashboard provides a high-level snapshot of customer behavior, profitability, and retention performance across key dimensions. It’s designed to help business leaders quickly assess health metrics and identify areas for deeper exploration.

🔍 Key Visuals & Metric Explanations

📅 Date Filter
Purpose: Enables dynamic time-based analysis.
Usage: Filters all visuals to the selected date range (e.g., Nov 2–30, 2020).

👤 Employee & City Filters
Purpose: Drill down into performance by sales rep or geography.
Usage: Refines all metrics to selected employee or city context.

🗺️ Map – Sum of Profit by City
Metric: Aggregated profit across cities.
Insight: Highlights geographic hotspots of profitability.
Use Case: Identify high-performing regions or cities needing attention.

🧮 Card – Unique Customers
Metric: Total distinct customers in the selected period.
Insight: Measures reach and engagement breadth.
Use Case: Track customer acquisition trends over time.

🍩 Donut Chart – Repeat Customers
Metric: Count and percentage of repeat customers.
Insight: Indicates customer loyalty and retention.
Use Case: Evaluate effectiveness of retention strategies and lifecycle programs.

📈 Line Chart – Sales and Profit Monthly Trend
Metrics: Sales and profit over time.
Insight: Visualizes revenue and margin fluctuations.
Use Case: Spot seasonal patterns, campaign impact, or anomalies.

🥧 Pie Chart – Sum of Profit by Stock Item
Metric: Profit contribution by product.
Insight: Reveals top-performing items driving margin.
Use Case: Inform product strategy, bundling, and inventory prioritization.

🧠 Strategic Highlights
Combines customer, product, and geographic views for holistic performance monitoring.
Repeat customer ratio is a key retention KPI, directly tied to lifecycle value.
Profit by city and stock item enables targeted sales and marketing interventions.


📦 02. Product Performance – Sales, Profit & Lead Time Analysis
This dashboard provides a focused view of product-level performance across sales, profit, and operational lead times. It’s designed to help stakeholders identify high-margin items, optimize inventory decisions, and understand the relationship between delivery speed and revenue.

🔍 Key Visuals & Metric Explanations

📅 Date Selector
Purpose: Filters all visuals to the selected time window (e.g., Nov 1–30, 2000).
Use Case: Enables temporal analysis of product trends and performance.

📦 Buying Package Selector
Purpose: Filters visuals by product packaging type.
Use Case: Compare performance across packaging tiers (e.g., bulk vs retail).

📊 Bar Chart – Total Sales by Stock Items
Metric: Sum of total sales per stock item.
Insight: Highlights top-selling products.
Use Case: Identify revenue drivers and prioritize inventory.

📈 Scatter Plot – Sales vs. Profit by Stock Item
Metrics: X-axis = Sales, Y-axis = Profit.
Insight: Reveals margin efficiency per product.
Use Case: Spot high-volume, low-margin items or hidden profit gems.

📋 Table – Profit by Stock Item & Lead Days

🧬 03. Customer Segmentation – Loyalty, Behavior & Purchase Patterns
This dashboard provides a multidimensional view of customer segmentation, combining loyalty tiers, behavioral flags, and purchase activity. It’s designed to help analysts and business leaders identify high-value customers, assess churn risk, and tailor engagement strategies.

🔍 Key Visuals & Metric Explanations

🎯 Churn Status Selector
Purpose: Filters all visuals by churn status (Active, Churned).
Use Case: Compare behaviors and value across retained vs lost customers.

🧱 Bar Chart – Loyalty Tier by Avg Spend
Metric: Average spend per loyalty tier.
Insight: Platinum and Gold tiers drive the most revenue.
Use Case: Prioritize retention efforts for high-spend segments.

🧠 Behavioral Segmentation Filter
Toggle: "At Risk & Engaged"
Purpose: Highlights customers flagged by behavioral models.
Use Case: Target campaigns toward engaged but at-risk cohorts.

📋 Table – Top Customers by Purchase Count
Metrics:
Purchase Count: Total transactions per customer.
Days Active: Lifecycle duration.
Days Since Last Purchase: Recency indicator.
Total Spend: Cumulative revenue per customer.
Insight: These churned customers were highly active and valuable.
Use Case: Analyze missed retention opportunities and refine churn triggers.

📈 Line Chart – Last Invoice Date × Customer Spend
X-axis: Invoice dates (Nov 23–Dec 21)
Y-axis: Customer spend
Highlight: Cumulative spend labeled “36bn”
Insight: Shows spend velocity and drop-off patterns.
Use Case: Identify peak engagement windows and lifecycle decay.

🧠 Strategic Highlights
Loyalty tiers correlate strongly with spend—Platinum customers are retention-critical.
Behavioral segmentation adds predictive depth beyond static churn flags.
Purchase frequency and recency are key indicators of churn risk and engagement.
Combining lifecycle metrics with spend enables targeted retention and upsell strategies.


💰 04. Customer Lifetime Value – CLV Modeling & Lifecycle Prioritization
This dashboard provides a deep dive into customer lifetime value (CLV) across lifecycle stages and loyalty tiers. It’s designed to help analysts and decision-makers identify high-value customers, assess retention risk, and quantify the financial impact of engagement strategies.

🔍 Key Visuals & Metric Explanations
🧭 Lifecycle Status Filter
Options: All, At Risk, Engaged
Purpose: Segment customers by behavioral lifecycle status.
Use Case: Compare CLV and spend across retention risk categories.

📊 Bar Chart – Value Distribution by Loyalty Tier
Metric: CLV distribution across tiers (Bronze, Silver, Gold, Platinum)
Insight: Engaged Platinum and Gold customers contribute the highest value.
Use Case: Prioritize retention and upsell efforts for top-tier segments.

📋 Table – Top Customers by CLV
CustomerKeyCLV EstimateCLV DiscountedTotal SpendLoyalty Segment
CLV Estimate: Projected lifetime value based on historical behavior.
CLV Discounted: Time-adjusted value using discounting logic.
Total Spend: Actual revenue generated to date.
Insight: Engaged customers show higher future value despite lower historical spend.
Use Case: Justify investment in engagement programs and retention campaigns.

📈 Bar Chart – CLV Contribution Across Customer Lifecycle
Metrics:
Engaged: 1.09K
At Risk: 0.31K
Insight: Engaged customers contribute 3.5× more CLV than at-risk ones.
Use Case: Quantify ROI of lifecycle interventions and behavioral segmentation.

🧠 Strategic Highlights
CLV modeling enables proactive retention and personalized marketing.
Discounted CLV reflects financial realism and supports budgeting decisions.
Lifecycle segmentation adds predictive depth to static loyalty tiers.
Combining CLV with behavioral flags helps prioritize high-impact customer actions.

🧠 05. Customer Behavior – Churn Dynamics & Behavioral Segmentation
This dashboard provides a behavioral lens on customer retention, combining loyalty tiers, purchase activity, and spend velocity. It’s designed to help analysts identify churn-prone segments, quantify engagement, and surface high-value behavioral patterns.

🔍 Key Visuals & Metric Explanations

🧭 Churn Status Panel
Options: Select All, Churned, Active
Purpose: Filters all visuals by churn status.
Use Case: Compare behavioral traits across retained vs lost customers.

🧱 Bar Chart – Loyalty Tier by Avg Spend
Metric: Count of customers per loyalty tier.
Insight: Gold tier dominates in volume, but Platinum likely drives higher per-customer value.
Use Case: Prioritize retention efforts for high-spend, low-volume segments.

📋 Table – Top Customers by Purchase Count
Metrics:
Purchase Count: Frequency of transactions.
Days Active: Lifecycle duration.
Days Since Last Invoice: Recency indicator.
Total Spend: Cumulative revenue per customer.
Insight: These churned customers were highly engaged and valuable.
Use Case: Refine churn prediction models and reactivation strategies.

📈 Area Chart – Last Invoice Date × Customer Spend
X-axis: Invoice dates (Nov 23–Nov 28)
Y-axis: Cumulative spend
Highlight: Spend peak labeled “36bn”
Insight: Visualizes spend velocity and drop-off patterns.
Use Case: Identify peak engagement windows and lifecycle decay.

🧠 Strategic Highlights
Behavioral segmentation adds predictive depth to static churn flags.
Loyalty tier distribution reveals concentration of value and risk.
Purchase frequency and recency are key indicators of churn likelihood.
Combining lifecycle metrics with spend enables targeted retention and upsell strategies.

🔮 06. What-If Simulation – Engagement Impact & CLV Forecasting
This dashboard models the potential uplift in Customer Lifetime Value (CLV) based on engagement scenarios. It enables analysts and decision-makers to simulate behavioral interventions and quantify their financial impact across lifecycle stages and loyalty tiers.

🔍 Key Visuals & Metric Explanations

🎛️ Dropdown – Select Loyalty Tier
Purpose: Filters all visuals by loyalty tier (e.g., Bronze, Silver, Gold, Platinum).
Use Case: Assess how engagement affects CLV across different customer segments.

📊 Bar Chart – CLV Uplift from Engagement Program
Metric: Average CLV per engagement status.
Insight: Engaged customers show a 52% uplift in CLV.
Use Case: Justify investment in engagement programs and retention campaigns.

📈 Bar Chart – CLV Stages
Metric: Average CLV by lifecycle stage.
Insight: Maturity stage yields the highest value; decline stage signals urgency.
Use Case: Target lifecycle-specific interventions to maximize CLV.

📋 Table – Top CLV and CLV Discount
CLV: Projected lifetime value.
CLV Discount: Time-adjusted value using discounting logic.
Insight: Discounted CLV reflects financial realism for forecasting.
Use Case: Support budgeting and ROI modeling.

📊 Area Chart – Last Invoice Date × CLV & Discounted CLV
X-axis: Time (monthly intervals)
Y-axis: CLV values
Final Value: Cumulative CLV reaches 8,334
Insight: Visualizes CLV growth trajectory and engagement impact over time.
Use Case: Forecast future value and simulate retention outcomes.

🧠 Strategic Highlights
What-if modeling enables proactive strategy design and ROI forecasting.
Engagement programs show measurable uplift in CLV across tiers and lifecycle stages.
Discounted CLV supports financial planning and prioritization.
Lifecycle segmentation reveals timing windows for maximum impact.

