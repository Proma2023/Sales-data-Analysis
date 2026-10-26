# 📦 Orders & Shipments Analytics Dashboard

An end-to-end analysis of ~31,000 retail order and shipment records, built in Google Colab with Python. The project cleans the data, engineers delivery-performance and profitability metrics, and presents the findings through interactive Plotly Dash dashboards and a Folium world map.

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/<username>/<repo-name>/blob/main/<notebook-name>.ipynb)
![Python](https://img.shields.io/badge/Python-3.10+-blue.svg)
![Dash](https://img.shields.io/badge/Dash-3.2.0-informational.svg)
![License](https://img.shields.io/badge/License-MIT-green.svg)

## 📑 Table of Contents
- [Overview](#-overview)
- [Features](#-features)
- [Tech Stack](#️-tech-stack)
- [Dataset](#-dataset)
- [Methodology](#-methodology)
- [Key Findings](#-key-findings)
- [Getting Started](#️-getting-started)
- [Usage](#-usage)
- [Repository Structure](#-repository-structure)
- [Future Work](#-future-work)
- [Contributing](#-contributing)
- [License](#-license)
- [Contact](#-contact)

## 🔎 Overview
Retail and supply chain teams need to know how well orders are being fulfilled, where revenue comes from, and which markets are most profitable. This project analyzes an orders-and-shipments dataset (2015–2017) to answer questions such as:

- Are shipments arriving on time, early, or late compared with the scheduled shipping days?
- How do gross sales trend by year and month?
- Which customers, product categories, and regions drive sales and profit?
- How are sales distributed across countries?

The results are delivered as interactive dashboards so stakeholders can filter by year and explore the data themselves.

## 📦 Features
- Data cleaning and preprocessing (column standardization, date construction, discount parsing)
- Feature engineering: **ShippingTime**, **ShippingDelay**, **DeliveryStatus** (Early / OnTime / Delayed), and **ProfitMargin%**
- Exploratory analysis of top customers, product categories, and yearly sales
- Interactive **Dash** dashboards:
  - Delivery status pie chart
  - Monthly sales trend with a year dropdown
  - Profit by customer market with a year slider
  - Combined yearly dashboard with profit-by-region bar chart and sales-by-country choropleth map
- **Folium** choropleth world map of gross sales by country
- Runs entirely in Google Colab, with no local setup required

## 🛠️ Tech Stack
- **Language:** Python 3
- **Environment:** Google Colab
- **Data processing:** pandas
- **Visualization:** Plotly Express, Plotly Graph Objects, Matplotlib, Seaborn, Folium
- **Dashboarding:** Dash 3.2.0 (`dash`, `dcc`, `html`)
- **Storage:** Google Drive (mounted in Colab)

## 📊 Dataset
- **File:** `orders_and_shipments.csv`
- **Size:** 30,871 rows × 24 columns (no missing values)
- **Coverage:** Orders from 2015 to 2017

| Group | Columns |
|-------|---------|
| Order | OrderID, OrderItemID, order date parts, OrderTime, OrderQuantity |
| Product | ProductDepartment, ProductCategory, ProductName |
| Customer | CustomerID, CustomerMarket, CustomerRegion, CustomerCountry |
| Shipment | WarehouseCountry, shipment date parts, ShipmentMode, ShipmentDaysScheduled |
| Financial | GrossSales, Discount%, Profit |

> **Note:** The dataset is shared through Google Drive and is not included in this repository. See [Getting Started](#️-getting-started) for how to access it.

## 🔬 Methodology
1. **Data loading:** mount Google Drive and read the CSV into pandas.
2. **Cleaning:**
   - Standardize column names (remove stray spaces and symbols).
   - Combine year, month, and day columns into `OrderDate` and `ShipmentDate`.
   - Drop the now-redundant date part columns.
   - Strip whitespace from `Discount%`, replace `-` with 0, and convert to float.
3. **Feature engineering:**
   - `ShippingTime` = ShipmentDate − OrderDate (in days)
   - `ShippingDelay` = ShipmentDaysScheduled − ShippingTime
   - `DeliveryStatus` = Delayed (< 0), OnTime (= 0), or Early (> 0)
   - `ProfitMargin%`
4. **Exploratory analysis:** top customers by gross sales, most frequent product categories and products per top customer, yearly and monthly sales trends.
5. **Visualization:** Plotly pie chart, Seaborn line plot, and a Folium choropleth map.
6. **Dashboards:** interactive Dash apps with callbacks driven by dropdowns and sliders.

## 📈 Key Findings

**Yearly gross sales**

| Year | Gross Sales |
|------|-------------|
| 2015 | 2,112,579 |
| 2016 | 2,213,350 |
| 2017 | 1,855,547 |

**Delivery status** (30,871 order items)

| Status | Count | Share |
|--------|-------|-------|
| Delayed | 13,442 | ~43.5% |
| Early | 10,718 | ~34.7% |
| On time | 6,711 | ~21.7% |

**Other highlights**
- Top product categories by order lines: Cleats (4,298), Men's Footwear (3,855), and Women's Apparel (3,622).
- The highest-spending customer (ID 9897) placed 18 orders totaling 4,570 in gross sales.
- Average order quantity is about 2.1 items, and the average discount is about 10%.

<!-- Add screenshots of your dashboards here, e.g.:
![Delivery Status](images/delivery_status.png)
![Sales Trend](images/sales_trend.png)
![World Map](images/world_map.png)
-->

## ⚙️ Getting Started

### Option 1: Run in Google Colab (recommended)
1. Click the **Open in Colab** badge at the top of this README.
2. Make the dataset available in your Google Drive:
   - Open Drive and go to **Shared with me**.
   - Right-click the `NPowerJDA_Group8` folder → **Add shortcut** (under **Organize** in newer versions) → **All locations** → **My Drive** → **Add**.
   - If you can't see the folder, request access from the project owner.
3. Run the notebook cells in order from the top, and approve the Google Drive mount prompt when it appears.

### Option 2: Run locally
**Prerequisites:** Python 3.10+ and Jupyter.

1. Clone the repository:
   ```bash
   git clone https://github.com/<username>/<repo-name>.git
   cd <repo-name>
   ```
2. Install dependencies:
   ```bash
   pip install pandas plotly dash==3.2.0 folium matplotlib seaborn jupyter
   ```
3. Download `orders_and_shipments.csv` and place it in the project folder.
4. In the notebook, remove the `from google.colab import drive` and `drive.mount(...)` lines, and change the data path to your local file:
   ```python
   df_orig = pd.read_csv('orders_and_shipments.csv')
   ```
5. Launch Jupyter:
   ```bash
   jupyter notebook
   ```

## 💡 Usage
1. Run the setup cell to install and import the libraries and mount Google Drive (Colab only).
2. Confirm the data path matches your Drive:
   ```python
   df_orig = pd.read_csv('/content/drive/MyDrive/NPowerJDA_Group8/orders_and_shipments.csv')
   ```
3. Run the cleaning and feature engineering cells in order.
4. Run a dashboard cell (any cell that calls `app.run()`) to launch an interactive Dash app inline. Use the dropdown or slider to filter by year.
5. Run the Folium cell to render the sales-by-country world map.

> Each dashboard cell defines a new `app`, so run one dashboard cell at a time. If you hit a port error, restart the runtime and rerun the cells you need.

## 📁 Repository Structure
```
├── <notebook-name>.ipynb
├── images/                # dashboard screenshots (optional)
├── LICENSE
└── README.md
```

## 🔮 Future Work
- Add filters for product category, shipment mode, and warehouse country
- Analyze root causes of delayed deliveries by shipment mode and region
- Predict delivery delays with a machine learning model
- Combine all views into a single multi-page dashboard and deploy it (e.g., Render or Hugging Face Spaces)

## 🤝 Contributing
Contributions are welcome! Please open an issue first to discuss the changes you would like to make.

## 📄 License
Distributed under the MIT License. See `LICENSE` for more information.

## 📬 Contact
Proma Anwar · [LinkedIn](https://www.linkedin.com/in/promaanwara3b2a5283) · [proma.anwarkhan@gmail.com](mailto:proma.anwarkhan@gmail.com)

*Developed as part of the NPower × JDA Group 8 project.*
