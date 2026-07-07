# ⛷️ Snow Finder — Ski Resort Intelligence Dashboard
**Power BI · Power Query · DAX · Data Cleaning · Star Schema · Geospatial Analytics**

![Power BI](https://img.shields.io/badge/Power%20BI-F2C811?style=flat&logo=powerbi&logoColor=black)
![Power Query](https://img.shields.io/badge/Power%20Query-1D4ED8?style=flat)
![DAX](https://img.shields.io/badge/DAX-7B2D8B?style=flat)
![Data Cleaning](https://img.shields.io/badge/Data%20Cleaning-15803D?style=flat)
![Geospatial](https://img.shields.io/badge/Geospatial-0E7490?style=flat)

---

## 📌 Project Overview

Snow Finder is an end-to-end Power BI dashboard that helps skiers and snowboarders identify the **best ski resorts worldwide based on real snowfall data**. The project integrates **499 ski resorts across 38 countries and 5 continents** with **820,000+ monthly snowfall records** — enabling users to filter resorts by snow coverage, slope difficulty, price, season, and facilities.

The project required significant **data quality work on the resorts table** — 156 out of 499 resort names contained encoding corruption (question marks replacing special characters), which were manually corrected using **SkiResort.info**. Snow data was cleaned entirely inside **Power Query**, keeping the raw CSV untouched.

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
| Avg Day Pass | €48.72 per adult |
| Child Friendly Resorts | 99.2% |
| Resorts with Nightskiing | 40.9% |

**Top 5 countries by resort count:**
🇦🇹 Austria (89) · 🇫🇷 France (81) · 🇺🇸 USA (78) · 🇨🇭 Switzerland (59) · 🇮🇹 Italy (44)

---

## 🧹 Data Cleaning & Quality Work

> This project involved substantial manual data quality effort — not just connecting tables and building visuals.

### resorts.csv — Manually Cleaned ✅
**156 out of 499** resort names had encoding corruption where special characters (accents, umlauts, diacritics) were replaced with `?` characters during export.

**Example:**
```
Before:  Nevados de Chilla?n
After:   Nevados de Chillán

Before:  Tignes - ?Val d'Ise?re
After:   Tignes - Val d'Isère
```

**Steps taken:**
1. **Encoding errors fixed** — Every `?` character in resort names identified and corrected manually
2. **Cross-referenced SkiResort.info** — Official ski resort directory used to validate every corrected name
3. **Duplicate records removed** — Duplicate entries eliminated to prevent inflated dashboard metrics
4. **Misspellings corrected** — Resort names standardised for consistent filtering across the data model

### snow.csv — Raw file, cleaned in Power Query ⚙️
The 820,523-row fact table was **not manually edited**. All transformations — data type corrections, null handling, column formatting — were applied entirely inside **Power Query (M)** within Power BI, keeping the source CSV untouched.

---

## 🗂️ Data Model — Star Schema

4-table star schema with **snow as the fact table**.

```
resorts (Dimension) ──────────────────┐
                                       ├──── snow (Fact) ←── Calendar (Dimension)
Resort Lookup (Dimension) ────────────┘
```

| Table | Type | Rows | Description |
|---|---|---|---|
| resorts | Dimension | 499 | Ski resort details — slopes, lifts, price, facilities |
| snow | Fact | 820,523 | Monthly snow % coverage per 0.25° grid cell |
| Calendar | Dimension | — | Date, Month Name, Month Number, Year |
| Resort Lookup | Dimension | — | ID, Resort, Season, Available Months |

### ⚙️ GeoKey — Solving the Coordinate Precision Mismatch

The `resorts` table stores full GPS precision coordinates while `snow` uses a coarser **0.25° × 0.25° grid**. A direct join was impossible without engineering a bridge key.

**Solution — Power Query M:**
```m
// Round resort GPS coordinates to nearest 0.25° to match snow grid
GeoKey = Number.RoundDown([Latitude] * 4) / 4
         & "_"
         & Number.RoundDown([Longitude] * 4) / 4
```
The same `GeoKey` was added to both tables, enabling a clean **many-to-one join** between snow coverage records and resort locations.

---

## 🏔️ Resort Dataset — Key Fields (25 columns)

`ID` `Resort` `Latitude` `Longitude` `Country` `Continent` `Price` `Season`
`Highest point` `Lowest point` `Beginner slopes` `Intermediate slopes` `Difficult slopes`
`Total slopes` `Longest run` `Snow cannons` `Surface lifts` `Chair lifts` `Gondola lifts`
`Total lifts` `Lift capacity` `Child friendly` `Snowparks` `Nightskiing` `Summer skiing`

---

## 🛠 Tools & Techniques

`Power BI Desktop` `Power Query (M)` `DAX Measures` `Star Schema Modelling`
`Manual Data Cleaning` `Geospatial Join` `GeoKey Engineering` `Coordinate Rounding`
`Calendar Table` `Resort Lookup Bridge Table` `Map Visuals` `Slicers & Filters`
`SkiResort.info Validation` `Encoding Error Correction`

---

## 📁 Repository Files

| File | Status | Description |
|---|---|---|
| `Snow_Finder.pbix` | ✅ Main file | Power BI dashboard — data model, Power Query, visuals & DAX |
| `resorts.csv` | ✅ Manually cleaned | 499 resorts · 156 names fixed · duplicates removed |
| `snow.csv` | ⚙️ Raw — cleaned in Power Query | 820,523 snow records · 0.25° geospatial grid |
| `data_dictionary.csv` | 📖 Reference | Field definitions for all 29 columns |
| `Snow_finder.mp4` | 🎬 Demo | Full dashboard walkthrough video |

---

## 🖼️ Data Model Preview

![Data Model](Screenshot_2026-07-07_103450.png?raw=true)

*Star Schema — resorts · snow · Calendar · Resort Lookup · GeoKey join on 0.25° coordinate grid*

---

📁 `.pbix` included &nbsp;|&nbsp; ⛷️ 499 resorts · 38 countries &nbsp;|&nbsp; ❄️ 820K+ snow records &nbsp;|&nbsp; 🧹 156 names manually cleaned &nbsp;|&nbsp; ⚙️ Power Query transforms
