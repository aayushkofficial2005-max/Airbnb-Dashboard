# Airbnb-Dashboard
# 🏠 Global Airbnb Performance Dashboard

An interactive 3-page **Power BI** dashboard built to analyze the performance, ratings, and review behavior of ~250,000 Airbnb listings across 10 major global cities.

Overview 
Ratings 
Reviews


---

## 📌 Project Overview

This dashboard turns raw Airbnb listings and review data into a business-ready report that answers three core questions:

1. **Growth** — How has the volume and mix of Airbnb listings evolved, and what macro events (regulation, COVID-19) shaped that trend?
2. **Quality** — How do listings compare across cities on price, ratings, and Superhost status?
3. **Trust & Engagement** — How verified are hosts, how often do guests leave reviews, and when does demand peak seasonally?

> **Note:** This repo ships a Power BI **Template (`.pbit`)**, not a `.pbix`. A `.pbit` stores the data model, DAX measures, and report layout — but **no data**. You'll need to point it at your own copy of the source CSVs the first time you open it (see [Setup](#-setup--how-to-use) below).

---

## 📊 Dashboard Pages

### 1️⃣ Overview
- **KPI cards:** Total Listings, Cities, Hosts, Property Types, Total Reviews
- **Trend line chart** of new listings over time, broken out by room type (Entire Place, Private Room, Shared Room, Hotel Room)
- Annotated with Airbnb's market lifecycle — Introduction → Growth → Maturity → Decline — plus callouts on the 2015 listings peak, the 2016–17 regulatory slowdown, and the COVID-19 disruption from 2019 onward

### 2️⃣ Ratings
- **Combo chart:** Superhost vs. non-Superhost listings by city, with a cumulative % (Pareto) line
- **Bar chart:** average price by room type
- **Column chart:** average overall rating by city
- **Pivot table:** sub-category ratings (accuracy, cleanliness, communication, location, value) by city
- Key callouts: Paris, NYC, and Sydney together account for ~48% of listings and reviews; Mexico City and Rio score highest overall; Hong Kong and Istanbul score lowest; cleanliness and value-for-money are the weakest-scoring categories globally

### 3️⃣ Reviews
- **Cumulative review-frequency chart:** what share of reviewers leave 1, 2, 3+ reviews (long-tail distribution)
- **Host trust KPI cards:** % of hosts that are Verified + have a profile photo, Verified + no photo, Not Verified + photo, Not Verified + no photo
- **Ribbon chart:** % of monthly reviews by city, revealing seasonality (Paris & Rome peak in European summer; New York peaks around the November–December holidays)
- Key callouts: 98.8% of reviewers left 3 or fewer reviews; over two-thirds of hosts are fully verified

---

## 🗂️ Data Model

| Table | Description | Columns |
|---|---|---|
| `Listings` | One row per listing — host info, location, property/room type, pricing, and review sub-scores | 33 |
| `Reviews` | One row per review — `review_id`, `listing_id`, `reviewer_id`, `date` | 4 |

**Relationship:** `Reviews[listing_id]` → `Listings[listing_id]` (many-to-one, bidirectional cross-filtering), plus auto date tables built off `Reviews[date]` and `Listings[host_since]` for time intelligence.

### Selected DAX measures
```DAX
Cumulative % review frequency =
DIVIDE ( [Cumulative Reviewers], [Total Reviewers] )

City Rank =
RANKX (
    ALL ( Listings[city] ),
    [Total Listings],
    ,
    DESC
)

Verified_Profile % =
DIVIDE ( [Verified_Profile], [Hosts Total] )

Superhost Listings =
CALCULATE (
    COUNT ( Listings[listing_id] ),
    Listings[host_is_superhost] = "t"
)
```
The model also includes measures for cumulative listings share by city, average price/rating breakdowns, monthly review share (`% of Monthly Reviews`), and a four-way host verification/profile-photo segmentation.

---

## 📁 Dataset

- **Source:** [Airbnb Listings & Reviews — Kaggle](https://www.kaggle.com/datasets/mysarahmadbhat/airbnb-listings-reviews)
- **Scope:** 250,000+ listings and ~5 million reviews across 10 cities — Bangkok, Cape Town, Hong Kong, Istanbul, Mexico City, New York City, Paris, Rio de Janeiro, Rome, and Sydney
- **Files used:** `Listings.csv`, `Reviews.csv`

---

## 🛠️ Tools & Skills

- **Power BI Desktop** — data modeling, relationships, and report design
- **Power Query (M)** — CSV import, type transformation, header promotion
- **DAX** — `CALCULATE`, `RANKX`, `DIVIDE`, `DISTINCTCOUNT`, `FILTER`, `ALLSELECTED` for KPIs, cumulative/Pareto logic, and segmentation
- **Data storytelling** — annotated callouts tying each visual back to a business insight

---

## 🚀 Setup / How to Use

1. Download `Listings.csv` and `Reviews.csv` from the [Kaggle dataset](https://www.kaggle.com/datasets/mysarahmadbhat/airbnb-listings-reviews).
2. Open `AIRBNB_Dashboard.pbit` in Power BI Desktop.
3. When prompted, enter the local folder path containing the two CSV files.
4. Power BI will load the data into the existing model — all measures and visuals will populate automatically.
5. (Optional) Use **File → Save As → Power BI file (.pbix)** to save a version with your data baked in.

---

## 📂 Repository Structure

```
├── AIRBNB_Dashboard.pbit   # Power BI template (data model + DAX + report layout)
├── screenshots/            # Dashboard page screenshots (add your own)
└── README.md
```

---

## 🔮 Future Enhancements

- Add a geospatial map visual using `latitude`/`longitude` for listing density by neighbourhood
- Build a price/revenue-potential view by room type and city
- Add interactive slicers (city, room type, date range) for self-serve exploration
- Refresh against live [Inside Airbnb](http://insideairbnb.com/get-the-data/) data instead of a static snapshot

---

## 👤 Author

**Ayush**
B.Com (Honours), Jamia Millia Islamia — building skills in financial modelling, Power BI, and SQL for corporate finance internships.

---

## 📜 License

This project is shared for educational and portfolio purposes. The underlying dataset is provided by Maven (https://mavenanalytics.io/data-playground/airbnb-listings-reviews) under its respective license — please review the dataset's terms before redistributing the raw data.
