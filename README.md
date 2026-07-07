# ⛷️ Snow Finder — Ski Resort Intelligence Dashboard
**Power BI · Power Query · DAX · Data Cleaning · Star Schema · Geospatial Analytics**

![Power BI](https://img.shields.io/badge/Power%20BI-F2C811?style=flat&logo=powerbi&logoColor=black)
![Power Query](https://img.shields.io/badge/Power%20Query-1D4ED8?style=flat)
![DAX](https://img.shields.io/badge/DAX-7B2D8B?style=flat)
![Data Cleaning](https://img.shields.io/badge/Data%20Cleaning-15803D?style=flat)

---

## 🎯 Who Is This For?

This dashboard is built for **general audiences** — anyone planning a ski trip who wants to find the best resort based on **actual snowfall data**, budget, slope difficulty, season, and facilities. No technical background needed to use it.

---

## 📌 Project Overview

Snow Finder is an end-to-end Power BI dashboard that helps skiers and snowboarders identify the **best ski resorts worldwide based on real snowfall data**. The project integrates **499 ski resorts across 38 countries and 5 continents** with **820,000+ monthly snowfall records** — enabling users to filter resorts by snow coverage, slope difficulty, price, season, and facilities.

The project required significant **data quality work on the resorts table** — 156 out of 499 resort names had encoding corruption, manually corrected using **SkiResort.info**. Snow data was cleaned entirely inside **Power Query**, keeping the raw CSV untouched.

---

## 📊 Dataset at a Glance

| Metric | Value |
|---|---|
| Ski Resorts | 499 |
| Countries | 38 |
| Continents | 5 (Europe, N. America, Asia, S. America, Oceania) |
| Snow Records | 820,523 |
| Pass Price Range | €0 – €141 per day |
| Highest Peak | 3,914m |
| Longest Run | 16 km |
| Largest Resort | 600 km total slopes |

**Top 5 countries:** 🇦🇹 Austria (89) · 🇫🇷 France (81) · 🇺🇸 USA (78) · 🇨🇭 Switzerland (59) · 🇮🇹 Italy (44)

---

## 🧹 Data Cleaning & Quality Work

| File | Status | Details |
|---|---|---|
| [`resorts.csv`](https://github.com/abhisheknirmal02-lab/Snow-Finder-/blob/main/resorts.csv) | ✅ Manually Cleaned | 156 names fixed · duplicates removed · misspellings corrected |
| [`snow.csv`](https://github.com/abhisheknirmal02-lab/Snow-Finder-/blob/main/snow.csv) | ⚙️ Raw — cleaned in Power Query | 820K+ rows transformed inside Power BI pipeline |

1. **156 resort names fixed** — encoding corruption replaced accents with `?`
   - ❌ `Tignes - ?Val d'Ise?re` → ✅ `Tignes - Val d'Isère`
   - ❌ `Nevados de Chilla?n` → ✅ `Nevados de Chillán`
2. **Cross-referenced SkiResort.info** — every corrected name validated against the official directory
3. **Duplicates removed** — eliminated to prevent inflated dashboard metrics
4. **Snow data cleaned in Power Query** — data types, nulls, formatting — source CSV untouched
5. **GeoKey engineered** — solved coordinate precision mismatch between resort GPS and 0.25° snow grid

---

## 🗂️ Data Model — Star Schema

| Table | Type | Rows | Key Columns |
|---|---|---|---|
| [`resorts`](https://github.com/abhisheknirmal02-lab/Snow-Finder-/blob/main/resorts.csv) | Dimension | 499 | ID, Resort, Country, Slopes, Lifts, Price |
| [`snow`](https://github.com/abhisheknirmal02-lab/Snow-Finder-/blob/main/snow.csv) | Fact | 820,523 | Month, Latitude, Longitude, Snow % |
| Calendar | Dimension | — | Date, Month Name, Month Number, Year |
| Resort Lookup | Dimension | — | ID, Resort, Season, Available Months |

### ⚙️ GeoKey — Power Query M
```m
// Round resort GPS to nearest 0.25° to match snow grid
GeoKey = Number.RoundDown([Latitude] * 4) / 4
         & "_"
         & Number.RoundDown([Longitude] * 4) / 4
```

---

## 🛠 Tools & Techniques

`Power BI Desktop` `Power Query (M)` `DAX Measures` `Star Schema Modelling`
`Manual Data Cleaning` `Geospatial Join` `GeoKey Engineering`
`Calendar Table` `Map Visuals` `Slicers & Filters` `SkiResort.info Validation`

---

## 📁 Repository Files

| File | Description |
|---|---|
| [📊 Snow Finder.pbix](https://github.com/abhisheknirmal02-lab/Snow-Finder-/blob/main/Snow%20Finder.pbix) | Main dashboard — data model, Power Query, visuals & DAX |
| [⛷️ resorts.csv](https://github.com/abhisheknirmal02-lab/Snow-Finder-/blob/main/resorts.csv) | 499 resorts · 25 columns · manually cleaned |
| [❄️ snow.csv](https://github.com/abhisheknirmal02-lab/Snow-Finder-/blob/main/snow.csv) | 820,523 snow records · raw — cleaned in Power Query |
| [📖 data_dictionary.csv](https://github.com/abhisheknirmal02-lab/Snow-Finder-/blob/main/data_dictionary.csv) | Field definitions for all 29 columns |
| [🎬 Snow finder.mp4](https://github.com/abhisheknirmal02-lab/Snow-Finder-/blob/main/Snow%20finder.mp4) | Full dashboard walkthrough demo |

---

## ▶️ Dashboard Walkthrough Video

> 🎬 **[Watch Full Demo — Snow Finder Dashboard](https://github.com/abhisheknirmal02-lab/Snow-Finder-/blob/main/Snow%20finder.mp4)**
> 
> Power BI · Interactive filters · Resort exploration · Snow coverage analysis

---

## 🖼️ Dashboard Preview

![Snow Finder Dashboard](https://github.com/abhisheknirmal02-lab/Snow-Finder-/blob/main/Snow%20Finder%20SS.png?raw=true)

---

## 🗺️ Data Model Preview

![Data Model](https://github.com/abhisheknirmal02-lab/Snow-Finder-/blob/main/Snow%20finder%20Model.png?raw=true)

*Star Schema — resorts · snow · Calendar · Resort Lookup · GeoKey join on 0.25° coordinate grid*

---

📁 `.pbix` included &nbsp;|&nbsp; ⛷️ 499 resorts · 38 countries &nbsp;|&nbsp; ❄️ 820K+ snow records &nbsp;|&nbsp; 🧹 156 names manually cleaned &nbsp;|&nbsp; 🎬 Demo video included
