# Formula 1 Performance Analytics

Power BI project analyzing the **2024 Formula 1 season** using data retrieved from the **Jolpica / Ergast API**.

The project covers the full BI workflow: data extraction, transformation in Power Query, dimensional modeling, DAX, interactive reporting and performance validation.

![Season Overview](screenshots/SeasonOverview.png)

## Project Overview

The goal of this project was to build a complete analytical Power BI solution rather than a simple single-table dashboard.

The dataset contains:

- **24 races**
- **24 drivers**
- **479 driver-race result records**
- Formula 1 **2024 season**

The main fact table uses the following grain:

> **One row = one driver in one race**

The report focuses on:

- season standings and championship progression
- driver performance
- constructor performance
- race-by-race results
- grid vs finish analysis
- reliability and race completion
- driver contribution to constructor results

---

## Data Source

Data was retrieved from the **Jolpica / Ergast Formula 1 API**.

Main endpoint:

`https://api.jolpi.ca/ergast/f1/2024/results/`

Power Query was used to:

- retrieve API data
- handle pagination
- expand nested JSON structures
- clean and transform columns
- assign appropriate data types
- prepare the fact and dimension tables

A staging query was used as the common source for the analytical model.

`StgRaceResults` has **Enable Load disabled**, so it supports data preparation without being loaded into the semantic model.

---

## Data Model

The project uses a **star schema**.

### Fact table

`FactRaceResults`

Grain:

> **One driver in one race**

It contains race-level facts such as:

- finishing position
- grid position
- points
- laps
- race status
- fastest lap information
- constructor and driver keys

### Dimension tables

- `DimDriver`
- `DimConstructor`
- `DimRace`
- `DimCircuit`
- `DimDate`

Relationships follow a standard:

**Dimension → Fact**

with **1-to-many, single-direction filtering**.

A dedicated `RaceKey` based on season and round is used to uniquely identify each race and keep the model scalable for additional seasons.

---

## DAX

The report uses a dedicated `_Measures` table to organize DAX measures.

Examples include:

- Total Points
- Wins
- Podiums
- Driver Rank
- Constructor Rank
- Race Starts
- DNF Count
- DNF Rate
- Finish Rate
- Avg Grid Position
- Avg Finish Position
- Positions Gained
- Avg Positions Gained
- Points per Start
- Driver Share of Constructor Points
- Constructor Share of Total Points
- Previous Race Points
- Race-over-Race Change
- Rolling 5 Race Average
- Cumulative Points

The project also uses DAX concepts such as:

- `CALCULATE`
- filter context
- context transition
- `FILTER`
- `SUMX`
- `AVERAGEX`
- `RANKX`
- `ALL`
- `ALLSELECTED`
- `REMOVEFILTERS`
- `KEEPFILTERS`
- `ADDCOLUMNS`
- `SUMMARIZE`
- `SUMMARIZECOLUMNS`

---

## Dashboard Pages

### Season Overview

Provides a high-level view of the 2024 season, including:

- driver standings
- constructor standings
- championship progression
- average grid vs finish position
- race result status distribution

![Season Overview](screenshots/SeasonOverview.png)

---

### Driver Performance

Interactive page focused on a single selected driver.

Includes:

- Driver Rank
- Total Points
- Wins
- Podiums
- DNF Rate
- Avg Grid Position
- Avg Finish Position
- Avg Positions Gained
- Finish Rate
- Points by Race
- Grid vs Finish by Race
- Race-by-Race Results

The driver selector uses single-selection filtering so all visuals remain in the context of one driver.

![Driver Performance](screenshots/DriverPerformance.png)

---

### Constructor Performance

Interactive analysis of a selected Formula 1 constructor.

Includes:

- Constructor Rank
- Total Points
- Wins
- Podiums
- DNF Rate
- Avg Grid Position
- Avg Finish Position
- Avg Positions Gained
- Finish Rate
- Points by Race
- race result status distribution
- driver contribution to constructor points

The **Driver Contribution & Performance** section compares drivers who actually raced for the selected constructor and shows their contribution to the team's total points.

![Constructor Performance](screenshots/ConstructorPerformance.png)

---

## Performance Validation

The report was tested using the built-in **Power BI Performance Analyzer**.

Visual rendering and DAX query execution times were reviewed to identify potential bottlenecks.

The report also follows several modeling practices that support performance and maintainability:

- star schema architecture
- single-direction relationships
- dedicated dimension tables
- staging query with disabled load
- limited number of visuals per page
- reusable DAX measures
- removal of unnecessary descriptive columns from the fact table

---

## Tools & Technologies

- **Power BI Desktop**
- **Power Query**
- **DAX**
- **REST API / JSON**
- **Jolpica / Ergast Formula 1 API**
- **Git**
- **GitHub**

---

## Repository Structure

```text
formula1-powerbi-performance-analysis/
│
├── F1-PowerBI.pbix
├── README.md
│
└── screenshots/
    ├── SeasonOverview.png
    ├── DriverPerformance.png
    └── ConstructorPerformance.png
```

---

## Power BI File

The complete interactive Power BI report is available here:

[Download F1-PowerBI.pbix](./F1-PowerBI.pbix)

---

## Author

**Rafał Zachraj**

Data Analyst portfolio project focused on Power BI, dimensional modeling, DAX and analytical dashboard development.
