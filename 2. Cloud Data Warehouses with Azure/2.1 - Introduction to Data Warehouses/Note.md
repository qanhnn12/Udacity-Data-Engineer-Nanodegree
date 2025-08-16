# Introduction to Data Warehouse

# 1. OLTP vs. OLAP

![https://video.udacity-data.com/topher/2021/August/6111bc21_l1-introduction-to-datawarehousing/l1-introduction-to-datawarehousing.png](https://video.udacity-data.com/topher/2021/August/6111bc21_l1-introduction-to-datawarehousing/l1-introduction-to-datawarehousing.png)

**Operational Processes**: Make it Work

- Find goods & make orders (for customers)
- Stock and find goods (for inventory staff)
- Pick up & deliver goods (for delivery staff)

**Analytical Processes**: What is Going On?

- Assess the performance of sales staff (for HR)
- See the effect of different sales channels (for marketing)
- Monitor sales growth (for management)

# 2. Technical perspective

![https://video.udacity-data.com/topher/2021/August/6111d21c_l1-introduction-to-datawarehousing-1/l1-introduction-to-datawarehousing-1.png](https://video.udacity-data.com/topher/2021/August/6111d21c_l1-introduction-to-datawarehousing-1/l1-introduction-to-datawarehousing-1.png)

# 3. **DWH Architecture**

## **3.1. Kimball's Bus Architecture**

![https://video.udacity-data.com/topher/2021/August/6112ddd2_l1-introduction-to-datawarehousing-3/l1-introduction-to-datawarehousing-3.png](https://video.udacity-data.com/topher/2021/August/6112ddd2_l1-introduction-to-datawarehousing-3/l1-introduction-to-datawarehousing-3.png)

## 3.2. Independent Data Marts

- Departments have separate ETL processes & dimensional models
- These separate dimensional models are called “Data Marts”
- Different fact tables for the same events, no conformed dimensions
- Uncoordinated efforts can lead to inconsistent views
- Despite awareness of the emergence of this architecture from departmental autonomy, it is generally discouraged

![https://video.udacity-data.com/topher/2021/August/6112e7c3_l1-introduction-to-datawarehousing-4/l1-introduction-to-datawarehousing-4.png](https://video.udacity-data.com/topher/2021/August/6112e7c3_l1-introduction-to-datawarehousing-4/l1-introduction-to-datawarehousing-4.png)

## 3.3. Inmon's Corporate Information Factory

- 2 ETL Process
    - Source systems → 3NF database
    - 3NF database → Departmental Data Marts
- The 3NF database acts as an enterprise-wide data store.
    - Single integrated source of truth for data-marts
    - Could be accessed by end-users if needed
- Data marts are dimensionally modeled & unlike Kimball’s dimensional models, they are mostly aggregated

## 3.4. DWH Architecture: Hybrid Bus & CIF

![https://video.udacity-data.com/topher/2021/August/6112eac6_l1-introduction-to-datawarehousing-5/l1-introduction-to-datawarehousing-5.png](https://video.udacity-data.com/topher/2021/August/6112eac6_l1-introduction-to-datawarehousing-5/l1-introduction-to-datawarehousing-5.png)

[Code 360 by Coding Ninjas](https://www.naukri.com/code360/library/inmon-vs-kimball-approach-in-data-warehousing)

[Inmon vs Kimball: Data Warehousing Approaches](https://www.ssp.sh/brain/inmon-vs-kimball/)

# 4. **OLAP Cubes**

Once we have a star schema, we can create OLAP cubes.

- An OLAP cube is an aggregation of a fact metric on a number of dimensions
    - Movie, Branch, Month
- Easy to communicate to business users

## OLAP Cubes: Roll-Up and Drill Down

**Roll-up:** Sum up the sales of each city by Country: e.g. US, France (less columns in branch dimension)

**Drill-Down:** Decompose the sales of each city into smaller districts (more columns in branch dimension)

![https://video.udacity-data.com/topher/2021/August/6112fa12_screen-shot-2021-08-10-at-5.08.08-pm/screen-shot-2021-08-10-at-5.08.08-pm.png](https://video.udacity-data.com/topher/2021/August/6112fa12_screen-shot-2021-08-10-at-5.08.08-pm/screen-shot-2021-08-10-at-5.08.08-pm.png)

## OLAP Cubes: Slice and Dice

| Slice | Dice |
| --- | --- |
| Reducing N dimensions to N-1 dimensions by restricting one dimension to a single value | Same dimensions but computing a sub-cube by restricting, some of the values of the dimensions |
| e.g. month = ‘FEB’ | e.g. month IN (’FEB’, ‘MAR’) AND movie in (’Avatar’, ‘Batman’) AND branch = ‘NY’ |

![https://video.udacity-data.com/topher/2021/August/61130009_screen-shot-2021-08-10-at-5.10.57-pm/screen-shot-2021-08-10-at-5.10.57-pm.png](https://video.udacity-data.com/topher/2021/August/61130009_screen-shot-2021-08-10-at-5.10.57-pm/screen-shot-2021-08-10-at-5.10.57-pm.png)

![https://video.udacity-data.com/topher/2021/August/611321f3_screen-shot-2021-08-10-at-5.11.43-pm/screen-shot-2021-08-10-at-5.11.43-pm.png](https://video.udacity-data.com/topher/2021/August/611321f3_screen-shot-2021-08-10-at-5.11.43-pm/screen-shot-2021-08-10-at-5.11.43-pm.png)

# 5. Roll Up and Drill Down Demo

**Roll-up**

- Stepping up the level of aggregation to a large grouping
- e.g. city is summed as country

**Drill Down**

- Breaking up one of the dimensions to a lower level.

# 6. Grouping Sets

- Group by CUBE (dim1, dim2, ..), produces all combinations of different lengths in one go.
- This view could be materialized in a view and queried, which would save lots of repetitive aggregations

# 7. CUBE

- Often for 3 dimensions, you want to aggregate a fact by multiple dimensions
- Demonstration of doing this with the SQL grouping statement `grouping sets ()`