# Chart Selection Justification

This document explains the rationale behind the visual design choices made for the worksheets and dashboard, ensuring alignment with data visualization best practices.

### 1. Sales Trend View (Monthly & Quarterly)
*   **What question the chart answers:** How are total sales changing over time?
*   **Why the chart type is appropriate:** A continuous line chart is the optimal choice for time-series data. It visually connects data points, representing an unbroken chronological progression that makes trends and seasonal spikes instantly recognizable.
*   **What field is used for color, size, label, or filter:** 
    *   *Columns:* `MONTH(Order Date)` / `QUARTER(Order Date)` (Continuous)
    *   *Rows:* `SUM(Sales)`
*   **What design principle you applied:** Minimal clutter. Redundant axis titles were hidden, as the dates are self-explanatory.
*   **What mistake you avoided:** Avoided using a discrete bar chart, which would break the visual flow of a timeline into disconnected chunks.

### 2. Regional Performance View
*   **What question the chart answers:** Which specific states and regions generate the most sales and profit?
*   **Why the chart type is appropriate:** A filled map provides immediate geographic context, allowing leadership to intuitively grasp spatial performance without reading a list of 19 different state names.
*   **What field is used for color, size, label, or filter:**
    *   *Columns/Rows:* `Longitude (generated)` / `Latitude (generated)`
    *   *Detail:* `State`
    *   *Color:* `SUM(Profit)` (Sequential color palette)
    *   *Filter:* Used as the primary Dashboard Action filter.
*   **What design principle you applied:** Focus on business interpretation. The map's background layers (oceans, neighboring borders) were washed out to 100% to draw the eye strictly to the relevant data points.
*   **What mistake you avoided:** Avoided a misleading scale by ensuring the map view was locked to India, preventing the map from zooming out unhelpfully to show the entire globe.

### 3. Category Profitability View
*   **What question the chart answers:** Which specific sub-categories drive profit or cause financial losses?
*   **Why the chart type is appropriate:** A horizontal bar chart allows for precise comparison of magnitudes across multiple categories while keeping long sub-category names perfectly horizontal and readable.
*   **What field is used for color, size, label, or filter:**
    *   *Rows:* `Category` and `Sub-Category`
    *   *Columns:* `SUM(Profit)`
    *   *Color:* `Profit Margin` (Diverging palette: Green for positive, Red for negative).
*   **What design principle you applied:** Appropriate sorting. Bars are sorted in descending order by profit to establish a clear hierarchy, putting top performers at the top and loss-leaders at the bottom.
*   **What mistake you avoided:** Avoided using pie charts, which are mathematically ineffective for displaying negative numbers (losses) and cannot cleanly accommodate 13 distinct sub-category slices. 

### 4. Return Analysis View
*   **What question the chart answers:** Which customer segment and region combination has the highest risk of product returns?
*   **Why the chart type is appropriate:** A highlight table (matrix) efficiently cross-references two distinct dimensions, using color intensity to allow the viewer to instantly spot outliers.
*   **What field is used for color, size, label, or filter:**
    *   *Rows:* `Region`
    *   *Columns:* `Customer Segment`
    *   *Color & Label:* `Return Rate` (Formatted as a percentage)
*   **What design principle you applied:** Proper labels and clear hierarchy. Formatting the metric as a strict percentage provides immediate, actionable business context.
*   **What mistake you avoided:** Avoided using a raw count of returned items, which would unfairly penalize high-volume regions. Using the calculated rate prevents data distortion.

### 5. Shipping Performance View
*   **What question the chart answers:** Which shipping mode causes delivery delays?
*   **Why the chart type is appropriate:** A bar chart effectively compares the baseline transit performance of categorical shipping tiers.
*   **What field is used for color, size, label, or filter:**
    *   *Rows:* `Ship Mode`
    *   *Columns:* `AVG(Delivery Days)`
    *   *Color:* `Shipping Delay Bucket`
*   **What design principle you applied:** Meaningful colors. By placing the delay bucket on color, the severity of delays within each shipping tier is visually highlighted.
*   **What mistake you avoided:** Avoided using `SUM(Delivery Days)`. Summing delivery times across different orders mathematically distorts the data by artificially inflating the numbers based on order volume. Using `AVG()` shows the true expected transit time per mode.

### 6. Customer Segment View
*   **What question the chart answers:** How does total performance compare across consumer, corporate, and home office segments?
*   **Why the chart type is appropriate:** A bar chart allows for a clear, side-by-side comparison of total financial metrics across a small number of categories.
*   **What field is used for color, size, label, or filter:**
    *   *Columns:* `Customer Segment` and `Measure Names`
    *   *Rows:* `Measure Values` (Sales, Profit, etc.)
*   **What design principle you applied:** Clear hierarchy and readable titles.
*   **What mistake you avoided:** Avoided using a line chart, as customer segments are discrete categories, not chronological or continuous data points.

### 7. Discount vs Profit View
*   **What question the chart answers:** How does the discount percentage relate to overall profitability?
*   **Why the chart type is appropriate:** A scatter plot is the standard and most effective chart type for analyzing the correlation and relationship between two different numerical measures.
*   **What field is used for color, size, label, or filter:**
    *   *Columns:* `AVG(Discount)`
    *   *Rows:* `SUM(Profit)`
*   **What design principle you applied:** Focus on business interpretation. 
*   **What mistake you avoided:** Avoided using `SUM(Discount)`, which is mathematically invalid for percentages and would distort the X-axis completely. Using `AVG(Discount)` accurately groups the profit behaviors.