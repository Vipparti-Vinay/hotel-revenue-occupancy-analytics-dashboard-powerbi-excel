# Hotel Revenue & Occupancy Analytics (Power BI, Excel)

## Summary / Objective

Turned raw hotel booking data into an interactive Power BI dashboard (15+ DAX measures) that shows where this chain of hotels is losing revenue, and recommended pricing, occupancy and channel fixes.

## Problem Statement

AtliQ Grands has operated for 20 years but is losing market share and revenue in the luxury and business hotel categories, partly because of competitor moves and partly because of ineffective decision-making. The managing director wants "Business and Data Intelligence" but the company has no in-house analytics team, so its revenue management team hired an outside analyst.

The tasks:

1. Create the metrics from the metric list.
2. Build a dashboard that follows the stakeholders' mock-up.
3. Find relevant insights that are not in the metric list or the mock-up.

The three business questions this project focuses on:

1. Is pricing matched to demand (dynamic pricing)?
2. Which hotels have persistently low occupancy, and why?
3. Are booking channels (own website, travel portals, others) priced and used well?

## Data Set

Five CSV files plus a metadata file and a metric list. Period: **1 May to 31 July 2022**. Currency is assumed to be Indian rupees.

| File | Type | Rows | Description |
|---|---|---|---|
| `dim_date.csv` | Dimension | 92 | Dates, month label, week number, day type |
| `dim_hotels.csv` | Dimension | 25 | Property ID, name, category (Luxury or Business), city (Bangalore, Delhi, Hyderabad, Mumbai) |
| `dim_rooms.csv` | Dimension | 4 | Room type (RT1 to RT4) and class (Standard, Elite, Premium, Presidential) |
| `fact_aggregated_bookings.csv` | Fact | 9,200 | Capacity and successful bookings per hotel, room type and date |
| `fact_bookings.csv` | Fact | 134,590 | One row per booking: booking, check-in and check-out dates, guests, platform, status, rating, revenue generated, revenue realised |

How revenue works: a **Checked Out** or **No Show** booking keeps 100% of its revenue, and a **Cancelled** booking keeps 40% (the other 60% is refunded). The `revenue_realized` column already reflects this.

## Tools and Technologies

| Tool | Used for |
|---|---|
| Power BI Desktop | Data modelling, DAX measures, report building |
| Power Query (M) | Loading the CSV folder, cleaning, shaping |
| DAX | KPIs, week-on-week (WoW) change, calculated columns |
| CSV and Excel | Source data and the stakeholder metric list |

## Project Structure

```
hotel-revenue-occupancy-analytics/
├── README.md                          # This file
├── data/
│   ├── dim_date.csv                   # Date dimension
│   ├── dim_hotels.csv                 # Hotel dimension
│   ├── dim_rooms.csv                  # Room dimension
│   ├── fact_aggregated_bookings.csv   # Capacity and successful bookings per day
│   ├── fact_bookings.csv              # Booking-level transactions
│   ├── meta_data_hospitality.txt      # Column descriptions
│   └── metrics-list.xlsx              # Stakeholder metric list
├── powerbi/
│   └── hotel_revenue_occupancy_dashboard.pbix   # Final Power BI report
└── images/
    ├── dashboard_overview.png         # Page 1 screenshot
    ├── dashboard_main_metrics.png     # Page 2 screenshot
    └── dashboard_hotel_detail.png     # Page 3 screenshot
```

## Data Cleaning & Preparation

1. **Load:** imported all CSV files from one folder into Power BI, then split them into five tables (`dim_date`, `dim_hotels`, `dim_rooms`, `fact_aggregated_bookings`, `fact_bookings`).
2. **Clean:** promoted the first row to headers in `dim_rooms`, set correct data types, and removed the shipped `day_type` column from `dim_date`.
3. **Model:** built a star schema linking both fact tables to the date, hotel and room dimensions.
4. **Rebuild day type and week number:** recreated `day_type` with Friday and Saturday as the weekend (the industry standard, as advised by the domain expert) and added a week number for week-on-week comparisons.

## EDA (Exploratory Data Analysis)

### Headline metrics

| Metric | Value |
|---|---|
| Revenue | 1.71bn |
| Occupancy | 57.87% |
| ADR | 12,696 |
| RevPAR | 7,347 |
| Realisation | 70.15% |
| Cancellation / No-show | about 24.8% / about 5.0% |
| Average rating | 3.62 |
| Luxury / Business share of revenue | 61.6% / 38.4% |

### By city

| City | Revenue | Occupancy | Rating |
|---|---|---|---|
| Mumbai | 669M (about 39%) | 57.9% | 3.65 |
| Bangalore | 420M | 55.8% (lowest) | 3.41 (lowest) |
| Hyderabad | 325M | 58.1% | 3.66 |
| Delhi | 295M | 60.6% (highest) | 3.78 (highest) |

### Weekday and weekend (stakeholder definition: Friday and Saturday)

| Day type | Occupancy | ADR | RevPAR |
|---|---|---|---|
| Weekday | 55.99% | 12,683 | 7,101 |
| Weekend | 62.64% | 12,725 | 7,972 |

### Other observations

- **Brands:** Atliq Exotica earns the most revenue (320M) and has the lowest cancellation rate (24.4%), but its occupancy (57.3%) and rating (3.62) are average. Atliq Blu has the highest occupancy of any brand (62.0%).
- **Room classes:** Elite rooms have the most bookings (36.8%), followed by Standard (28.6%), Premium (22.7%) and Presidential (11.9%). Cancellation is about 24% to 25% in every class.
- **Booking channels:** "others" brings 41% of bookings, then makeyourtrip (20%), logtrip (11%), direct online (10%), tripster (7%), journey (6%) and direct offline (5%). ADR varies by only about 1% across channels, and direct online (the hotel's own website) has the lowest ADR.
- **Weekly pattern:** revenue is steady at about 139M in most weeks but about 115M in W21, W23, W26, W30 and W31. The data does not say why.

## Dashboard

### Page 1: Overview

![Overview](https://github.com/Vipparti-Vinay/hotel-revenue-occupancy-analytics-dashboard-powerbi-excel/blob/main/Overview%20page.png)

- Headline cards: Revenue, Occupancy %, Average Rating.
- Revenue, occupancy and rating by city.
- Occupancy by day type, and a weekly trend of rating and occupancy.
- Booking share by platform.
- Filters: Booking Status, Platform, Category, Property, Month, City.

### Page 2: Main metrics

![Main metrics](https://github.com/Vipparti-Vinay/hotel-revenue-occupancy-analytics-dashboard-powerbi-excel/blob/main/Main%20page.png)

- Six KPI cards (Revenue, RevPAR, DSRN, Occupancy %, ADR, Realisation %) with week-on-week change.
- Weekday and weekend comparison table.
- Revenue share by category (Luxury and Business).
- Weekly trend for ADR, RevPAR and occupancy.
- Realisation % and ADR by booking platform.
- Hotel-level table with data bars: revenue, RevPAR, occupancy, ADR, DSRN, DBRN, DURN, realisation, cancellation and rating.
- Filters: City, Room class, Month, Week.

### Page 3: Hotel detail

![Hotel detail](https://github.com/Vipparti-Vinay/hotel-revenue-occupancy-analytics-dashboard-powerbi-excel/blob/main/Additional%20page.png)

- Full hotel table for ranking and finding problem properties.
- Average rating
- Booking share by room class.
- Revenue by property name.

## Results and Conclusion

### Findings

1. **There is no dynamic pricing.** Weekends are busier than weekdays but ADR rises only 0.3%. Across the weeks, ADR stays flat while occupancy moves widely.
2. **Four hotels keep underperforming.** The bottom group has about 44% occupancy and ratings near 2.3. These hotels earn about 13% of revenue but hold about 22% of all unsold room nights. Higher-rated hotels are also fuller, which is a pattern but not proof of cause.
3. **Channels are priced almost identically.** The gap between the cheapest and the dearest channel is only about 1%, direct bookings are just 15% of the total, and the biggest source ("others", 41%) cannot be traced.
4. **Cancellations are costly.** About 298.8M was lost, and the cancellation rate is about 25% in every channel and room class.

### Recommendations

1. **Introduce demand-based pricing:** higher rates on peak days (Saturday and Sunday in the data), promotions on low-demand days. As an illustration only, a 5% price increase with a 2% loss in demand on Saturday and Sunday would add about 18.5M. This is a scenario, not a forecast, and should be tested at a few hotels first.
2. **Fix the four weakest hotels:** start with a service-quality review (guest reviews, staff, facilities), set occupancy and rating targets, and track them with the weekly metrics.
3. **Improve channel strategy:** give direct bookers a benefit that is not a lower price (for example complimentary services), raise portal rates on peak days, and identify what is inside the "others" channel.
4. **Reduce cancellation losses:** use deposits or non-refundable rates, since the problem is the same in every channel.

## Author and Contact

**Vipparti Vinay**

- Email: [vinayvipparti.in@gmail.com](mailto:vinayvipparti.in@gmail.com)
- LinkedIn: [https://www.linkedin.com/in/vipparti-vinay/]
- Github profile: [https://github.com/Vipparti-Vinay]
