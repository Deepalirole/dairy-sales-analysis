Dairy Sales Analytics – Power BI Dashboard

An end-to-end Power BI project analyzing dairy products (Milk, Ghee, Cheese, Butter, Yogurt, etc.) across brand, region, product, shelf life, storage temperature, farm size, land area, and expiry dates. The report answers: Which products and brands drive quantity sold? How do shelf life and storage temperature influence sales? Which regions, farm sizes, and locations contribute most?
![image alt](<img width="1682" height="789" alt="image" src="https://github.com/user-attachments/assets/f3b66919-f10d-4fca-9431-acc011cb5851" />)



🔧 Tech Stack

Power BI Desktop (Power Query, DAX) • Excel/CSV data sources • Basic stats/EDA

📁 Dataset (example fields)

Date/Expiration: OrderDate, ExpirationDate

Product & Brand: ProductName, Brand, Category

Quantity & Price: QuantitySold, UnitPrice (if available)

Region & Location: Region, State, City, Latitude, Longitude

Operations: ShelfLifeDays, StorageTempCategory (warm/cold), FarmSize, TotalLandArea, Cows

Keep column names consistent; add a Date table for time intelligence.

🛠️ How I Built It — Step by Step
1) Data Ingestion & ETL (Power Query)

Connect to CSV/Excel → Transform Data.

Data types: set Date/Whole Number/Decimal/Text correctly.

Clean text: Transform → Format → Clean and Trim; fix inconsistent casing (e.g., “ghee”, “Ghee” → Ghee).

Remove duplicates on business keys (Brand, ProductName, Location, ExpirationDate as needed).

Handle missing values: replace nulls with 0 or “Unknown”; remove rows with impossible dates/shelf life.

Derive columns

ShelfLifeDays = ExpirationDate − ManufactureDate (or provided value)

SelfLifeGroup = Less / More bucket based on threshold (e.g., < 30 days = Less)

Year, MonthName, Week from date

Normalize StorageTempCategory (e.g., warm/cold)

Standardize Location names (map typos to canonical list)

Star schema shaping

FactSales (QuantitySold, dates, keys)

DimProduct (ProductName, Brand, Category, ShelfLifeDays)

DimLocation (Region/State/City, LandArea, FarmSize)

DimDate (generated via DAX or M)

2) Data Modeling

Create one-to-many relationships:

DimDate[Date] → FactSales[OrderDate]

DimProduct[ProductName] → FactSales[ProductName]

DimLocation[Location] → FactSales[Location]

Set Dim tables as Single direction; use surrogate keys where possible.

Mark DimDate as Date table.

3) EDA Highlights (inside Power BI)

Outliers: check extreme ShelfLifeDays vs QuantitySold.

Distributions: histogram of ShelfLifeDays; bar of QuantitySold by StorageTempCategory.

Correlations: scatter TotalLandArea vs Sales/Quantity (bubble by Brand).

4) Report Pages & Visuals (as in the screenshot)

Header slicers: Farm Size, Expiration Date range, Brand, Product Name.

Sales by Location (bar): Location × [Total Quantity].

Land Area vs Sales (scatter): TotalLandArea × [Total Quantity] (legend by Brand/Location).

Region-wise Farm States (table): Location, Cows, TotalLandArea.

Quantity Sold by Location (map): Location + [Total Quantity].

Sales by Product & Brand (clustered bar): ProductName × [Total Quantity] (legend Brand).

Sales by Region & Brand (stacked column): Region × [Total Quantity].

Shelf Life vs Sales (line): ShelfLifeDays × [Total Quantity].

Avg Shelf Life by Storage Temp Capacity (scatter/line): StorageTempCategory × avg ShelfLifeDays.

Storage Temp vs Shelf Life (table): ProductName, StorageTempCategory, ShelfLifeDays.

Product Popularity (donut/pie): [Total Quantity] by ProductName.

Avg Shelf Life per Product (bar): ProductName × avg ShelfLifeDays.

Top 5 Products Sold (bar): ProductName × [Total Quantity] (TOPN).

5) Interactivity

Slicers: Brand, ProductName, ExpirationDate, FarmSize, Region.

Drill-through: from overview → product or location detail.

Tooltips: show [Total Quantity], Avg ShelfLifeDays, StorageTempCategory.

Bookmarks: “Less Shelf Life (Milk)” and “More Shelf Life (Ghee)” views to compare.

