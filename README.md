# Module 5 Challenge

## Athletic Sales Analysis

This notebook analyses the athletic sales data.

### Requirements

#### Combine and Clean the Data

* The two DataFrames have been combined on the rows using an inner join and the index has been reset.

``` python
as20df.set_index("retailer",inplace=True)
as21df.set_index("retailer",inplace=True)

joined_as = pd.concat([as20df,as21df],axis=0,join="inner")

joined_as.reset_index()
```

* The "invoice_date" column has been converted to a datetime data type.

``` python
joined_as["invoice_date"] = pd.to_datetime(joined_as["invoice_date"])
```

#### Determine which Region Sold the Most Products

* A groupby or pivot_table function has been used to create a multi-index DataFrame with the "region", "state", and "city" columns.

``` python
# Using groupby()
a = pd.DataFrame(joined_as.groupby(["region","state","city"]).agg(Total_Products_Sold = ('units_sold',"sum")))

# Using pivot_table()
b = pd.pivot_table(joined_as,values="units_sold",columns=["region","state","city"],aggfunc="sum")
```

* The aggregated column has been renamed to reflect the aggregation of the data in the column.

``` python
# group() rename in the agg()
a = pd.DataFrame(joined_as.groupby(["region","state","city"]).agg(Total_Products_Sold = ('units_sold',"sum")))

# pivot_table() rename
b.rename(index={"units_sold":"Total_Products_Sold"},inplace=True)
```

* The results are sorted in descending order to show the top five regions, including the state and city that sold the most products.

``` python
# groupby() sorting
a.sort_values(["Total_Products_Sold"], ascending=False).head(5)

# pivot_table() sorting
b.transpose().sort_values(["Total_Products_Sold"], ascending=False).head(5)
```

#### Determine which Region had the Most Sales

* A groupby or pivot_table function has been used to create a multi-index DataFrame with the "region", "state", and "city" columns.

``` python
# Using groupby()
c = pd.DataFrame(joined_as.groupby(["region","state","city"]).agg(Total_Sales = ("total_sales","sum")))

# Using pivot_table()
d = pd.pivot_table(joined_as,values="total_sales",columns=["region","state","city"],aggfunc="sum")
```

* The aggregated column has been renamed to reflect the aggregation of the data in the column.

``` python
# group() rename in the agg()
c = pd.DataFrame(joined_as.groupby(["region","state","city"]).agg(Total_Sales = ("total_sales","sum")))

# pivot_table() rename
d.rename(index={"total_sales":"Total Sales"},inplace=True)
```

* The results are sorted in descending order to show the top five regions, including the state and city that generated the most sales.

``` python
# groupby() sorting
c.sort_values(["Total_Sales"], ascending=False).head(5)

# pivot_table() sorting
d.transpose().sort_values(["Total Sales"], ascending=False).head(5)
```

#### Determine which Retailer had the Most Sales

* A groupby or pivot_table function has been used to create a multi-index DataFrame with the "retailer", "region", "state", and "city" columns.

``` python
# Using groupby()
e = pd.DataFrame(joined_as.groupby(["retailer","region","state","city"]).agg(Total_Sales = ("total_sales","sum")))

# Using pivot_table()
f = pd.pivot_table(joined_as,values="total_sales",columns=["retailer","region","state","city"],aggfunc="sum")
```

* The aggregated column has been renamed to reflect the aggregation of the data in the column.

``` python
# group() rename in the agg()
e = pd.DataFrame(joined_as.groupby(["retailer","region","state","city"]).agg(Total_Sales = ("total_sales","sum")))

# pivot_table() rename
f.rename(index={"total_sales":"Total Sales"},inplace=True)
```

* The results are sorted in descending order to show the top five retailers along with their region, state, and city that generated the most sales.

``` python
# groupby() sorting
e.sort_values(["Total_Sales"], ascending=False).head(5)

# pivot_table() sorting
f.transpose().sort_values(["Total Sales"], ascending=False).head(5)
```

#### Determine which Retailer Sold the Most Women's Athletic Footwear

* A filtered DataFrame is created that shows only women's athletic footwear sales data.

``` python
g = joined_as[joined_as["product"]=="Women's Athletic Footwear"]
```

* A groupby or pivot_table function has been used to create a multi-index DataFrame with the "retailer", "region", "state", and "city" columns.

``` python
# Using groupby()
h = pd.DataFrame(g.groupby(["retailer","region","state","city"]).agg(Womens_Footwear_Units_Sold = ("units_sold","sum")))

# Using pivot_table()
i = pd.pivot_table(g,values="units_sold",columns=["retailer","region","state","city"],aggfunc="sum")
```

* The aggregated column has been renamed to reflect the aggregation of the data in the column.

``` python
# group() rename in the agg()
h = pd.DataFrame(g.groupby(["retailer","region","state","city"]).agg(Womens_Footwear_Units_Sold = ("units_sold","sum")))

# pivot_table() rename
i.rename(index={"units_sold":"Womens_Footwear_Units_Sold"},inplace=True)
```

* The results are sorted in descending order to show the top five retailers along with their region, state, and city that had the most women's athletic footwear sales.

``` python
# groupby() sorting
h.sort_values(["Womens_Footwear_Units_Sold"], ascending=False).head(5)

# pivot_table() sorting
i.transpose().sort_values(["Womens_Footwear_Units_Sold"], ascending=False).head(5)
```

#### Determine the Day with the Most Women's Athletic Footwear Sales

* A pivot table is created that has the "invoice_date" column as the index and the "total_sales" column assigned to the values parameter.

``` python
j = pd.pivot_table(g,values="total_sales",columns=["invoice_date"],aggfunc="sum")
```

* The aggregated column has been renamed to reflect the aggregation of the data in the column.

``` python
j.rename(index={"total_sales":"Total Sales"},inplace=True)
```

* The resample function is used on the pivot table, the data is placed into daily bins, and the total sales for each day is calculated.

``` python
k = j.transpose().resample("D").sum()
```

* The results are sorted in descending order to show the days that generated the most women's athletic footwear sales.

``` python
k.sort_values(["Total Sales"],ascending=False).head(10)
```

#### Determine the Week with the Most Women's Athletic Footwear Sales

* The resample function is used on the pivot table, the data is placed into weekly bins, and the total sales for each week is calculated.

``` python
l = j.transpose().resample("W").sum()
```

* The results are sorted in descending order to show the weeks that generated the most women's athletic footwear sales.

``` python
l.sort_values(["Total Sales"], ascending=False).head(10)
```
