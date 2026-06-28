# Part 4: Tableau Executive Dashboard & Data Storytelling

## Business Problem Summary
The retail leadership team requires an executive dashboard to monitor and act on sales performance,
profitability, customer segments, category trends, shipping efficiency, discount impact, and return
patterns across two fiscal years (2024–2025) and four Indian regions.

---

## Dataset Description

**File:** `data/dashboard_sales_data.xlsx`  
**Sheet:** `dashboard_sales_data`  
**Rows:** 4,200 orders  
**Columns:** 20  
**Date Range:** 1 January 2024 – 31 December 2025 


### Field Inventory

#### Date Fields
| Field | Tableau Type | Notes |
|---|---|---|
| `order_date` | Date | Order placement date. No nulls. Range: 2024-01-01 → 2025-12-31 |
| `ship_date` | Date | Shipment date. 16 orders ship into early 2026 (year-end orders). No nulls. |

#### Geographic Fields
| Field | Tableau Type | Unique Values |
|---|---|---|
| `region` | String (Dimension) | 4 — North, South, East, West |
| `state` | String (Geographic Role: State) | 19 Indian states |
| `city` | String (Geographic Role: City) | 20 cities |

#### Categorical Fields
| Field | Tableau Type | Unique Values | Nulls |
|---|---|---|---|
| `order_id` | String (Dimension) | 4,200 | 0 |
| `customer_id` | String (Dimension) | 4,096 | 0 |
| `customer_segment` | String (Dimension) | 3 — Consumer, Corporate, Home Office | 0 |
| `category` | String (Dimension) | 3 — Furniture, Office Supplies, Technology | 0 |
| `sub_category` | String (Dimension) | 13 | 0 |
| `product_name` | String (Dimension) | 4,111 | 0 |
| `ship_mode` | String (Dimension) | 4 — Same Day, First Class, Second Class, Standard Class | 0 |
| `campaign_channel` | String (Dimension) | 5 — Email, Organic, Paid, Referral, Social | 24 |

#### Numerical Measures
| Field | Tableau Type | Min | Max | Mean | Notes |
|---|---|---|---|---|---|
| `sales` | Continuous Measure | ₹72.91 | ₹4,27,418 | ₹51,671 | Revenue per order line |
| `profit` | Continuous Measure | –₹17,758 | ₹1,33,628 | ₹7,930 | Can be negative at high discounts |
| `quantity` | Continuous Measure | 1 | 10 | 5.5 | Units sold per order |
| `discount` | Continuous Measure | 0.00 | 0.35 | 0.137 | Stored as decimal (0–0.35 = 0%–35%) |
| `delivery_days` | Continuous Measure | 0 | 9 | 3.59 | Verified to match ship_date − order_date |
| `customer_rating` | Continuous Measure | 2.1 | 5.0 | 4.06 | 32 nulls — treat as optional survey field |

#### Binary / Flag Fields
| Field | Tableau Type | Values | Notes |
|---|---|---|---|
| `return_flag` | Integer → convert to Boolean Dimension | 0 = Not Returned, 1 = Returned | 191 returns (4.55% of orders) |

---

## Tableau Workbook Description

**File:** `tableau/executive_dashboard.twbx`

The packaged workbook contains:
- 7 individual analysis sheets (one per required view)
- 1 executive summary dashboard combining 5+ charts and 3+ KPI cards
- 5 calculated fields (see below)
- Dashboard actions for cross-filtering

---

## Calculated Fields Created

| Calculated Field | Formula | Purpose | Interpretation |
|---|---|---|---|
| **Profit Margin** | `SUM([Profit]) / SUM([Sales])` | Profit Margin measures how much profit is generated for every unit of sales revenue. It helps leadership understand overall business profitability and compare performance across regions, categories, and customer segments. | A higher profit margin indicates better operational efficiency and stronger pricing performance, while a low margin may indicate excessive discounting or high operational costs. |
| **Cost** | `[Sales] - [Profit]` | Cost estimates the total cost incurred to generate sales revenue. Since profit already exists in the dataset, cost can be derived by subtracting profit from sales. | This field helps compare revenue generation against cost structure and supports profitability analysis. |
| **Average Order Value** | `SUM([Sales]) / COUNTD([Order Id])` | Average Order Value measures the average revenue generated per customer order. It helps evaluate customer purchasing behavior and transaction size. | A higher average order value indicates larger purchase sizes, while lower values may suggest opportunities for cross-selling or upselling. |
| **Return Rate** | `SUM([Return Flag]) / COUNT([Order Id])` | Return Rate measures the percentage of total orders that were returned by customers. It helps identify product quality issues, customer dissatisfaction, or operational problems. | A high return rate may indicate poor product quality, inaccurate customer expectations, or supply chain issues. |
| **Shipping Delay Bucket** | `IF [Delivery Days] <= 1 THEN "Express (0-1d)" ELSEIF [Delivery Days] <= 3 THEN "Fast (2-3d)" ELSEIF [Delivery Days] <= 5 THEN "Standard (4-5d)" ELSE "Slow (6-9d)" END` | Shipping Delay Bucket categorizes delivery performance based on delivery speed. It helps identify shipping inefficiencies and customer service risks. | This field allows leadership to compare shipping performance across shipping modes and identify operational bottlenecks. |
| **Profit Per Unit** | `SUM([profit]) / SUM([quantity])` | Measures profit generated for each product unit sold. | Helps identify highly profitable products even when overall sales volume is low. |

---

## Dashboard Components

### KPI Cards (Summary Metrics)
1. **Total Sales** — `SUM([Sales])`
2. **Profit Margin** — `SUM([Profit]) / SUM([Sales])`
3. **Average Order Value** — `SUM([Sales]) / COUNTD([Order Id])`
4. **Return Rate** — `SUM([Return Flag]) / COUNT([Order Id])`

### Dashboard Sheets / Views
| Sheet | Chart Type | Business Question |
|---|---|---|
| Sales Trend | Line chart (Continuous) | How are total sales changing over time? |
| Regional Performance | Filled map | Which specific states and regions generate the most profit? |
| Category Profitability | Bar chart (Horizontal) | Which sub-categories are profitable or loss-making? |
| Customer Segment | Bar chart | How does performance compare across customer segments? |
| Shipping Performance | Stacked bar chart | Which shipping mode causes the highest volume of delivery delays? |
| Discount vs Profit | Scatter plot | How does the discount percentage relate to overall profitability? |
| Return Analysis | Highlight table (Matrix) | Which customer segment and region has the highest return risk? |

---

## Filters and Interactions Used

### Dashboard Filters
1. Region (on-click dashboard interaction filter)
2. Category (multi-select)
3. Customer Segment (multi-select)


### Dashboard Actions
- **Filter Action**: Clicking a specific state on the Regional Performance map filters all other sheets and KPI cards to display data exclusively for that state.

---

## Key Business Insights

1.  **Seasonal Sales Volatility:** Sales experience massive late-summer spikes (e.g., August 2025), indicating that revenue is heavily driven by seasonal campaigns rather than steady baseline growth.
2.  **Regional Strengths:** The South region is the most lucrative market, driving nearly 10M in profit, while the East and West regions suffer from low volume despite maintaining healthy ~15% margins.
3.  **Technology vs. Furniture:** Technology is the company's cash cow (18% margins), while Furniture items like Tables and Bookcases operate on dangerously thin margins (~5.7%).
4.  **Home Office Value & Friction:** The Home Office segment generates the highest revenue but suffers from the highest average return rate.
5.  **Discount Thresholds:** Offering discounts greater than 25% completely erodes profitability, resulting in net losses per order.
6.  **Standard Shipping Bottleneck:** Standard Class shipping is the sole cause of severe delivery delays, accounting for 98% of all shipments delayed by 6+ days.
7.  **Localized Return Crisis:** Home Office customers in the South region are returning products at an alarming 7.3% rate, signaling a localized logistics or product quality failure.
8.  **Missed Cross-Selling:** Office Supplies represent a missed opportunity; bundling these items with premium tech purchases could easily boost the Average Order Value.

---

## Dashboard Story Summary

The retail business is currently operating from a position of top-line strength, generating over 217M in sales with a healthy blended profit margin of 15.35%. Technology category and Southern geographic markets are the primary engines of this success. However, the dashboard reveals critical operational friction constraining our bottom line. Severe standard shipping bottlenecks are likely driving a highly localized spike in product returns (specifically among Southern Home Office buyers). Furthermore, margin erosion in the Furniture category requires a pricing restructure. By capping sales discounts at 20%, investigating Southern logistics, and preparing for our predictable August sales surges, the business is well-positioned to scale its profitability.

---

## Assumptions and Limitations

| # | Assumption / Limitation |
|---|---|
| 1 | `delivery_days` = `ship_date − order_date`. This measures time-to-ship, not time-to-deliver. Actual customer delivery time is not available. The `delivery_days` measure is assumed to accurately reflect the transit lead time calculated as the difference between `ship_date` and `order_date`. |
| 2 | Each row represents one order line. Multiple rows may share the same `order_id` if not deduplicated; COUNTD([Order Id]) used for order-level metrics. |
| 3 | `discount` field is stored as a decimal (0.0–0.35). Multiply by 100 in Tableau to display as percentage. |
| 4 | 24 null `campaign_channel` values are treated as "Unknown" and excluded from channel-level analysis. |
| 5 | 32 null `customer_rating` values are excluded from average rating calculations. |
| 6 | 16 orders with `ship_date` in January 2026 are attributed to 2025 (order date basis). |
| 7 | The `return_flag` is binary (1 = returned, 0 = kept). It is assumed that a returned order constitutes a full return of the sales value and profit adjustment. |
| 8 | Financial measures (`sales`, `profit`) are recorded in the local standard currency and do not require conversion. |
| 9 | Geographic roles in Tableau must be manually assigned: set `state` → Geographic Role → State/Province (India), and `city` → City. |
| 10 | `customer_id` is not fully unique per customer — repeat purchasers exist (4,096 unique IDs for 4,200 orders). |

---

## Screenshots Included

| File | Contents |
|---|---|
| `screenshots/full_dashboard.png` | Complete executive dashboard |
| `screenshots/sales_trend_view.png` | Monthly sales  line chart |
| `screenshots/regional_performance_view.png` | The geographic map of profitability. |
| `screenshots/category_profitability_view.png` | Sub-category profit bar chart |
| `screenshots/filter_interaction_view.png` | Evidence of the map's state-level action filter applied to the dashboard.|
