[README.md](https://github.com/user-attachments/files/31896359/README.md)
# 🛒 E-Commerce Analysis — Hackathon Report (Power BI)

An end-to-end business intelligence report built on the Olist Brazilian E-Commerce Public Dataset — analyzing revenue trends, delivery performance, seller/geographic distribution, and product category health to turn nine relational data tables into a single executive-ready story.

---

## 2. Short Description / Purpose

This project is a Power BI report developed for a data analytics hackathon, designed to simulate a real-world executive dashboard for an e-commerce marketplace. It consolidates order, payment, review, seller, product, and customer geography data into a star-schema data model and surfaces five focused report pages that answer the questions a COO, logistics lead, or category manager would actually ask: *Is revenue growing month-over-month? Are we delivering on time? Which states and sellers are driving volume? Which product categories are underperforming on customer satisfaction?*

---

## 3. Tech Stack

* **📊 Power BI Desktop** – Report authoring, page layout, and interactive canvas design.
* **📂 Power Query (M)** – Data ingestion and transformation layer used to clean and merge the raw Olist CSV tables.
* **🧠 DAX (Data Analysis Expressions)** – Custom measures for revenue, delivery performance, review sentiment, and month-over-month growth.
* **📝 Data Modeling** – Relational star-schema model built from 9+ source tables (orders, customers, products, sellers, payments, reviews, geolocation, category translation) linked through a shared Date Table.
* **📁 File Format** – `.pbit` (Power BI Template) so the report can be opened and re-pointed at a fresh copy of the dataset without carrying the full data cache in the repo.

---

## 4. Data Source

* **Source:** [Olist Brazilian E-Commerce Public Dataset](https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce) (Kaggle).
* **Structure:** A relational set of ~9 CSV tables covering the full order lifecycle:
  * `orders` – order status and lifecycle timestamps (purchase, approval, carrier handoff, delivery, estimated delivery)
  * `order_items` / `order_payments` / `order_reviews` – line-item pricing/freight, payment values, and customer review scores
  * `customers` / `geolocation` – customer location and state-level geo coordinates
  * `sellers` – seller location
  * `products` / `category_translation` – product attributes and English category names

> ⚠️ The raw CSVs are **not included** in this repo (Kaggle license/size). Download them from the link above and point Power BI's Power Query source folder at the extracted files, then click **Refresh** — see [Setup](#7-setup--how-to-use) below.

---

## 5. Features / Highlights

### • Business Problem
Marketplace operators sit on rich but siloed transactional data — orders, payments, reviews, and logistics all live in separate tables. Without a unified model, it's hard to answer operational questions like: *Which months drove the strongest revenue, and is growth accelerating or stalling? Are shipping delays actually hurting review scores? Which states have the most sellers and orders, and where is fulfillment weakest?*

### • Goal of the Report
To deliver a single interactive Power BI report that:
* Tracks core revenue, order volume, and satisfaction KPIs over time in one view.
* Connects delivery performance directly to customer review outcomes.
* Breaks down seller and order concentration by geography (Brazilian state).
* Ranks product categories by both order volume and customer satisfaction, exposing quality issues hidden inside high-volume categories.

### • Walkthrough of Report Pages

* **Page 1 — Executive Home**
  Landing page with the report title and a button-based navigation menu to jump directly to any analysis page — built for a stakeholder demo rather than manual tab-clicking.
* **Page 2 — Marketplace Performance Over Time**
  A combo chart (line + stacked columns) plotting **Total Revenue**, **Order Count**, and **Avg Review Score** across `Month-Year`. Answers "are we growing, and is satisfaction keeping pace with volume?" in a single glance, backed by a `Revenue MoM %` measure for trend framing.
* **Page 3 — Delivery Performance & Customer Satisfaction**
  Two clustered bar charts cross **Delay Bucket** (on-time vs. late-delivery tiers) against **Avg Review Score** — once by product category (`English_name`) and once by `customer_state`. Makes the link between shipping delays and star ratings explicit and localizable.
* **Page 4 — Seller & Geographic Patterns**
  A column chart of **Order Count by customer_state** paired with a pivot table (`Delay Bucket` × `customer_state` × `Avg Review Score`) for a state-by-state drill-down of both volume and delivery health.
* **Page 5 — Product Category Performance**
  A 100%-stacked bar chart of **Avg Review Score by category** alongside a clustered bar chart of **Order Count by category**, so high-volume and high-satisfaction categories can be compared side by side to spot "popular but poorly rated" outliers.

### • Business Impact & Insights
* **Growth Monitoring:** Leadership can track whether revenue growth is organic (rising order count) or price-driven, month over month.
* **Logistics Prioritization:** Ops teams can target the states/categories where delivery delays are most strongly correlated with low review scores.
* **Seller & Regional Strategy:** Regional managers can identify high-order-volume states that may need more seller onboarding or warehouse capacity.
* **Category Quality Control:** Category managers can flag high-volume product lines with disproportionately low satisfaction for supplier or QA review.

---

## 6. Data Model & Key Measures

The model uses a central `Date Table` and `orders` fact table joined to `customers`, `sellers`, `products` (+ `category_translation`), `order_items`, `order_payments`, and `order_reviews`.

| Measure | Definition |
|---|---|
| `Total Revenue` | `SUM(order_items[price])` |
| `Total Freight` | `SUM(order_items[freight_value])` |
| `Order Count` | `DISTINCTCOUNT(orders[order_id])` |
| `Avg Review Score` | `AVERAGE(order_reviews[review_score])` |
| `% Low Reviews (1-2 stars)` | Share of reviews with a score ≤ 2 |
| `Avg Delivery Delay` | `AVERAGE(orders[Delivery Delay (days)])` |
| `Late Delivery Rate` | Share of orders flagged `Delivery Status = "Late"` |
| `Avg Freight Value` / `Avg Price` | Per-order-item averages |
| `Seller Count` | `DISTINCTCOUNT(sellers[seller_id])` |
| `Avg Payment Value` | `AVERAGE(order_payments[Order Payment Value])` |
| `Revenue MoM %` | Month-over-month % change in `Total Revenue` using `DATEADD` |

Calculated columns like `Delivery Delay (days)`, `Delivery Status`, `Order-to-Delivery Days`, and `Delay Bucket` (on the `orders` table) do the heavy lifting of turning raw timestamps into the delivery-performance tiers used throughout the report.

---

## 7. Setup / How to Use

1. **Download the dataset** from Kaggle ([link above](#4-data-source)) and extract the CSVs to a local folder.
2. **Open** `E-commerce Analysis_Hackathon_Report.pbit` in Power BI Desktop.
3. When prompted for parameters/source folder, **point it at your extracted CSV folder** (Power BI templates don't embed data — only the model and report layout).
4. Click **Refresh** to load the data, then explore via the navigation buttons on the Home page.

---
[DATA_DICTIONARY.md](https://github.com/user-attachments/files/31896412/DATA_DICTIONARY.md)

## 8. Screenshots / Demos

### 🎥 Demo Walkthrough

A full screen-recorded walkthrough of the report is included at (https://drive.google.com/file/d/1TyirXBJFPxveqEzvTMweDiYryT6Dw_Er/view?usp=sharing).

> GitHub's file viewer plays `.mp4` files natively when you open the file directly (`assets/demo.mp4`) — it just won't auto-embed inline on the README page itself. Click the link above, or clone the repo and open the file locally.

If you'd like an inline-playable preview on the README page itself, drag-and-drop `assets/demo.mp4` into a new GitHub Issue or PR comment on this repo — GitHub will host it on its CDN and generate an embeddable `<video>` snippet you can paste back in here.

---

## 9. Author

Built by [Yadhusurya](https://github.com/Yadhusurya) — feel free to open an issue or PR with suggestions.

# Data Dictionary

Source: [Olist Brazilian E-Commerce Public Dataset](https://www.kaggle.com/datasets/olistbr/brazilian-ecommerce) (Kaggle). Not bundled in this repo — download separately and refresh the `.pbit` against it (see `README.md` § Setup).

| Table | Key Columns | Description |
|---|---|---|
| `orders` | `order_id`, `customer_id`, `order_status`, `order_purchase_timestamp`, `order_delivered_customer_date`, `order_estimated_delivery_date` | One row per order; lifecycle timestamps used to derive delivery delay/status. |
| `order_items` | `order_id`, `product_id`, `seller_id`, `price`, `freight_value` | Line items per order — product, seller, price, and shipping cost. |
| `order_payments` | `order_id`, `payment_type`, `payment_installments`, `payment_value` | Payment method and value per order. |
| `order_reviews` | `review_id`, `order_id`, `review_score`, `review_comment_message` | Customer review score (1–5) and comments per order. |
| `customers` | `customer_id`, `customer_unique_id`, `customer_city`, `customer_state` | Customer location at time of order. |
| `sellers` | `seller_id`, `seller_city`, `seller_state` | Seller location. |
| `products` | `product_id`, `product_category_name`, dimensions/weight | Product attributes. |
| `category_translation` | `product_category_name`, `product_category_name_english` | Maps Portuguese category names to English (used as `English_name` in the report). |
| `geolocation` | `geolocation_zip_code_prefix`, lat/lng | Zip-code-level coordinates for mapping. |

## Derived Columns / Measures

See `README.md` § 6 for the full list of DAX measures (`Total Revenue`, `Order Count`, `Avg Review Score`, `Late Delivery Rate`, `Revenue MoM %`, etc.) and calculated columns (`Delivery Delay (days)`, `Delivery Status`, `Delay Bucket`).

