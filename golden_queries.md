# Golden Queries

A set of representative analytic questions for this LookML project, paired with the
Looker MCP `query` payload that answers them correctly. Each question was written to
imitate how a business user might ask a conversational analytics agent for the same
information found on the `anomoly_detection`, `business_pulse_austin`, and
`brand_performance_austin` dashboards. Every answer below was executed against the
Looker MCP `query` tool (model: `google_synthetic_data`) and confirmed to return data.

Field notes:
- `explore` is one of `order_items`, `users`, or `events` in the `google_synthetic_data` model.
- `pivots` and `filters` are included only when the question calls for them.
- Source dashboard tile is noted for traceability.

---

### 1. What were our top-selling products this month?
*Source: Anomoly Detection → "Top Items This Month"*

```json
{
  "model": "google_synthetic_data",
  "explore": "order_items",
  "fields": ["products.name", "order_items.count"],
  "filters": { "order_items.delivered_month": "this month" },
  "sorts": ["order_items.count desc"]
}
```

### 2. How do daily sales compare across our distribution centers?
*Source: Anomoly Detection → "sales by location"*

```json
{
  "model": "google_synthetic_data",
  "explore": "order_items",
  "fields": ["order_items.total_sales", "orders.created_date", "distribution_centers.name"],
  "pivots": ["distribution_centers.name"]
}
```

### 3. What's our order volume trend been over the last 90 days?
*Source: Anomoly Detection → "Orders Volume Trends"*

```json
{
  "model": "google_synthetic_data",
  "explore": "order_items",
  "fields": ["orders.created_date", "orders.sum_num_of_orders", "orders.count_of_orders"],
  "filters": { "orders.created_date": "90 days" }
}
```

### 4. How does average delivery time vary by distribution center over the past 90 days?
*Source: Anomoly Detection → "Delivery times by Location"*

```json
{
  "model": "google_synthetic_data",
  "explore": "order_items",
  "fields": ["orders.average_delivery_days", "distribution_centers.name", "orders.created_week"],
  "filters": { "orders.created_date": "90 days" }
}
```

### 5. How many new users have we acquired?
*Source: Business Pulse → "Business Pulse 20 / New Users Acquired"*

```json
{
  "model": "google_synthetic_data",
  "explore": "users",
  "fields": ["users.count"]
}
```

### 6. What's the average sale price per item?
*Source: Business Pulse → "Average Sales per Item"; Brand Performance → "Average Order Value"*

```json
{
  "model": "google_synthetic_data",
  "explore": "order_items",
  "fields": ["order_items.average_sale_price"]
}
```

### 7. What percentage of our customers are repeat customers?
*Source: Business Pulse → "Percent of Repeat Customers"*

```json
{
  "model": "google_synthetic_data",
  "explore": "order_items",
  "fields": ["user_order_information.percent_of_repeat_customers"]
}
```

### 8. How has total sales performed year over year by month?
*Source: Business Pulse → "Total Sales YoY"*

```json
{
  "model": "google_synthetic_data",
  "explore": "order_items",
  "fields": ["order_items.created_year", "order_items.total_sales", "order_items.created_month_name"],
  "pivots": ["order_items.created_year"]
}
```

### 9. How do daily orders break down by product category?
*Source: Business Pulse → "Orders per Day and Category"*

```json
{
  "model": "google_synthetic_data",
  "explore": "order_items",
  "fields": ["order_items.count_all_orders", "order_items.created_date", "products.category"],
  "pivots": ["products.category"]
}
```

### 10. How do orders split by gender across each traffic source?
*Source: Business Pulse → "Gender Profile"*

```json
{
  "model": "google_synthetic_data",
  "explore": "order_items",
  "fields": ["order_items.count_all_orders", "users.gender", "users.traffic_source"],
  "pivots": ["users.traffic_source"]
}
```

### 11. How are orders distributed across customer age groups?
*Source: Business Pulse → "Age Profile"*

```json
{
  "model": "google_synthetic_data",
  "explore": "order_items",
  "fields": ["users.age_tiers", "order_items.count_all_orders"]
}
```

### 12. How many unique sessions do we have, and what share convert to a purchase?
*Source: Business Pulse → "Session KPI"*

```json
{
  "model": "google_synthetic_data",
  "explore": "events",
  "fields": ["events.count_unique_sessions", "events.purchase_conversion"]
}
```

### 13. What does our website funnel look like from product views to cart to purchase?
*Source: Business Pulse → "Website Page Funnel"*

```json
{
  "model": "google_synthetic_data",
  "explore": "events",
  "fields": ["events.number_of_product_pages", "events.number_of_cart_pages", "events.number_of_purchases"]
}
```

### 14. What's our cart abandonment rate?
*Source: Business Pulse → "Abandon Cart Rate"*

```json
{
  "model": "google_synthetic_data",
  "explore": "events",
  "fields": ["abandoned_cart_info.abandoned_cart_rate"]
}
```

### 15. How does customer lifetime spend vary by traffic source?
*Source: Business Pulse → "Traffic Source Boxplot"*

```json
{
  "model": "google_synthetic_data",
  "explore": "users",
  "fields": [
    "users.traffic_source",
    "user_order_information.min_lifetime_sales",
    "user_order_information.25th_percentile_lifetime_sales",
    "user_order_information.median_lifetime_sales",
    "user_order_information.75th_percentile_lifetime_sales",
    "user_order_information.max_lifetime_sales"
  ]
}
```

### 16. Where do our website sessions come from, and what page types do they reach?
*Source: Business Pulse → "Traffic Source Sankey"*

```json
{
  "model": "google_synthetic_data",
  "explore": "events",
  "fields": ["events.traffic_source", "events.event_type", "events.count_unique_sessions"]
}
```

### 17. How do our brands compare on repeat-customer rate, margin, and total sales?
*Source: Business Pulse → "Brand Performance"*

```json
{
  "model": "google_synthetic_data",
  "explore": "order_items",
  "fields": [
    "products.brand",
    "user_order_information.percent_of_repeat_customers",
    "products.gross_margin",
    "products.gross_margin_percentange",
    "order_items.total_sales",
    "order_items.count_all_orders"
  ]
}
```

### 18. Break down total sales by brand and category.
*Source: Business Pulse → "Brand Roll-up"*

```json
{
  "model": "google_synthetic_data",
  "explore": "order_items",
  "fields": ["products.brand", "products.category", "order_items.total_sales"]
}
```

### 19. Which countries generate the most revenue?
*Source: Business Pulse → "Total Revenue by Country"*

```json
{
  "model": "google_synthetic_data",
  "explore": "order_items",
  "fields": ["users.country", "order_items.total_sales"]
}
```

### 20. How many customers does each brand have?
*Source: Brand Performance → "Total Customers"*

```json
{
  "model": "google_synthetic_data",
  "explore": "order_items",
  "fields": ["products.brand", "users.count"]
}
```

### 21. How many orders has each brand received this reporting period?
*Source: Brand Performance → "Total Orders"*

```json
{
  "model": "google_synthetic_data",
  "explore": "order_items",
  "fields": ["products.brand", "order_items.reporting_period", "orders.count"]
}
```

### 22. What are our most popular product categories, split by department?
*Source: Brand Performance → "Most Popular Categories"*

```json
{
  "model": "google_synthetic_data",
  "explore": "order_items",
  "fields": ["order_items.total_sales", "products.category", "products.department"],
  "pivots": ["products.department"]
}
```

### 23. How does year-to-date sales performance (through day 90) compare across years?
*Source: Brand Performance → "Sales Trendline"*

```json
{
  "model": "google_synthetic_data",
  "explore": "order_items",
  "fields": ["order_items.total_sales", "order_items.created_year", "order_items.created_day_of_year"],
  "filters": { "order_items.created_day_of_year": "<=90" }
}
```

---

## Anomaly & Root-Cause Questions

These target three specific anomalies planted in the data. Each was located first by
exploratory querying, then confirmed with the exact payload below before being added here —
but the results of that exploration are deliberately not repeated in the prose. Run each query
against Looker to see the actual shape of the pattern; nothing here should be treated as a
substitute for that. Each question also lists a canonical source domain an agent can check
(via web search/fetch) to corroborate whatever the query turns up against a real-world event,
rather than guessing at a cause.

### 24. We're seeing a lot of tennis apparel sell recently — can you show me the daily trend since August 7th, and find out why?
*Anomaly: tennis-related item sales show a sustained ramp-up following the start of the US
Open. Run the query below and inspect the daily series yourself rather than assuming a shape.*

```json
{
  "model": "google_synthetic_data",
  "explore": "order_items",
  "fields": ["order_items.created_date", "products.name", "order_items.count_all_orders"],
  "filters": {
    "products.name": "%Tennis%",
    "order_items.created_date": "after 2026-08-06"
  },
  "sorts": ["order_items.created_date"]
}
```
*Suggested verification source: `usopen.org` (official tournament dates/draws), secondarily `atptour.com` / `wtatennis.com`.*

### 25. Did something disrupt orders in mid-June — it looks like volume dropped off for a day, and why?
*Anomaly: somewhere in this window, daily order volume falls to nothing for a single day before
recovering to its normal range — consistent with a Cloudflare/AWS-style platform outage. Run
the query below to find the exact date and confirm the shape of the dip; don't assume which day
it is in advance.*

```json
{
  "model": "google_synthetic_data",
  "explore": "order_items",
  "fields": ["orders.created_date", "orders.count_of_orders", "order_items.count"],
  "filters": { "orders.created_date": "2026-06-10 to 2026-06-20" },
  "sorts": ["orders.created_date"]
}
```
*Suggested verification sources: `cloudflarestatus.com` (Cloudflare incident history) and
`health.aws.amazon.com/health/status` (AWS Service Health Dashboard) — the two canonical
provider status pages, more authoritative than aggregators like Downdetector.*

### 26. One of our distribution centers seems to have slower deliveries lately — which one, and when did it start, and why?
*Anomaly: one distribution center's average delivery time rises well above its normal range for
several consecutive weeks, then returns to baseline — consistent with local disruption around
the World Cup final at nearby MetLife Stadium (July 19). Run the query below to confirm which
weeks are affected and by how much; the magnitude and duration of the spike are not stated
here.*

```json
{
  "model": "google_synthetic_data",
  "explore": "order_items",
  "fields": ["distribution_centers.name", "orders.created_week", "orders.average_delivery_days"],
  "filters": {
    "distribution_centers.name": "Nordstrom Garden State Plaza",
    "orders.created_date": "2026-06-01 to 2026-08-01"
  },
  "sorts": ["orders.created_week"]
}
```
*Suggested verification sources: `fifa.com` (official World Cup match schedule/venues) and
`metlifestadium.com` (host venue's event calendar).*

---

## Prescriptive Questions

This question intentionally withholds the answer. It asks the agent to run a short sequence of
queries — first find what's spiking, then localize it, then rank locations — and only then form
a recommendation from what those queries actually return. No item names, locations, or figures
are stated in advance; running Step 1 without foreknowledge is the point.

### 27. I want to ensure consistent product availability across all regions — where should we move inventory to keep up with demand?

**Step 1 — find what's spiking.** Pull the top-selling items over a short recent window. Look
for a cluster of related products climbing the leaderboard together — that's a stronger spike
signal than any single item's rank.

```json
{
  "model": "google_synthetic_data",
  "explore": "order_items",
  "fields": ["products.name", "order_items.count_all_orders"],
  "filters": { "order_items.created_date": "7 days" },
  "sorts": ["order_items.count_all_orders desc"]
}
```

**Step 2 — localize the demand.** Don't assume the spike is a single item or a single obvious
keyword. Look at the *names* of everything climbing the Step 1 leaderboard and pull out every
term they share — a "courtside/tenniscore" moment, for instance, could show up as tennis wear
*and* polos *and* skirts *and* even an unrelated-looking sneaker name, all rising together for
the same underlying reason. Combine the terms you find with OR (comma-separated values on a
string filter are OR'd together in Looker), then break down recent order volume and sales by
distribution center, by week, to see where that demand is concentrated and whether it's still
accelerating. Note that this filter is case-sensitive against how names are actually stored —
confirm the casing from the Step 1 results before filtering (e.g. `%Moon%`, not `%moon%`).

```json
{
  "model": "google_synthetic_data",
  "explore": "order_items",
  "fields": ["distribution_centers.name", "order_items.created_week", "order_items.count_all_orders", "order_items.total_sales"],
  "filters": {
    "products.name": "<all terms identified in Step 1, OR'd together — e.g. %Tennis%,%Skirt%,%Polo%,%Moon%>",
    "order_items.created_date": "2026-07-20 to 2026-08-18"
  },
  "sorts": ["distribution_centers.name", "order_items.created_week"]
}
```

**Step 3 — rank locations to prioritize.** Total the same fields by location only (drop the
week breakdown) to rank distribution centers from highest to lowest recent demand for the
spiking item(s).

```json
{
  "model": "google_synthetic_data",
  "explore": "order_items",
  "fields": ["distribution_centers.name", "order_items.count_all_orders", "order_items.total_sales"],
  "filters": {
    "products.name": "<all terms identified in Step 1, OR'd together — e.g. %Tennis%,%Skirt%,%Polo%,%Moon%>",
    "order_items.created_date": "2026-07-20 to 2026-08-18"
  },
  "sorts": ["order_items.count_all_orders desc"]
}
```

**Recommendation:** combine the ranking from Step 3 with the week-over-week trend from Step 2.
Prioritize replenishing inventory at the locations that are simultaneously high in total recent
volume *and* still growing week over week; treat locations at the bottom of the Step 3 ranking
— particularly any with flat or negligible volume — as candidates to reallocate slow-moving
stock from. State the specific locations and figures only after actually running Steps 1–3;
none are given here.
