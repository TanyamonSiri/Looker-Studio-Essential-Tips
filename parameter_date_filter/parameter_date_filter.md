
# 📅 Parameter Date Filters & Slicers

In many projects, I’ve often encountered a common challenge: different stakeholders require sales data at different levels of date granularity.

For example:

- The **Finance** team typically prefers a **monthly** sales view for financial reporting.
- The **Marketing** team, on the other hand, often needs **daily** or **weekly** breakdowns to evaluate campaign performance in detail.

To meet these varied needs, some data analysts create **separate reports** for each team or split date granularities across different pages. While this approach may work in the short term, it leads to:

- Unnecessary **report duplication**
- Increased **maintenance overhead**
- Higher **resource consumption**

## ✅ Solution: Use a Date Granularity Parameter

A better approach is to implement **parameterized date filters**. By using a parameter, you can dynamically control the date granularity across your dashboards without duplicating reports.

This technique allows users to **switch between daily, weekly, and monthly views** using a single report interface, improving both usability and maintainability.

You can apply this method in:

- **Looker Studio**
- **Power BI**
- **Tableau**
- Or any modern BI tool that supports parameters and dynamic filtering.

Note: The solution described below works seamlessly with the BigQuery connector.

> 💡 **Tip**: Although this guide is focused on Looker Studio, the concept of parameter-driven filtering is applicable across most BI platforms.
 

## 📁 Sample Data

- [mockup_demand_sales.csv](https://github.com/TanyamonSiri/Looker-Studio-Essential-Tips/blob/main/parameter_date_filter/mockup_demand_sales_data.csv)

## 📌 Data Requirement

To use this technique, your dataset must include the following:

- `date`: Must be in a **date data type**, not text or string.

> ⚠️ **Important**: Make sure the `date` column is correctly recognized as a date field in your BI tool (e.g., Looker Studio). If it's stored as text, convert it to a proper date format to enable correct filtering and parameter usage.


## 🛠️ Steps

### 1. Create a Parameter in Looker Studio

In Looker Studio, start by creating a **parameter** that includes all the date granularities you need.

In the example below, I created a parameter named **`Date Level`** with the following values:

- `Daily`
- `Weekly`
- `Monthly`
- `Quarterly`
- `Yearly (CY)`


![image](https://github.com/user-attachments/assets/aeff83c5-28ca-4c8c-a9fc-c7f842e0ffb7)

### 2. Create a New Calculated Field

In this step, you’ll create a calculated field that links to the `Date Level` parameter.

In my case, the dataset includes a `order_date` field as the date dimension.  
To support dynamic formatting based on the selected date granularity, I used a simple `CASE WHEN` expression:

``` sql
CASE
  WHEN  LOWER(   Date Level) = 'daily' THEN FORMAT_DATETIME("%Y-%m-%d", order_date)
  WHEN LOWER(  Date Level) = 'weekly' THEN
           FORMAT_DATETIME("%Y-%m-%d",
                  DATETIME_TRUNC(order_date, ISOWEEK)) || ' to ' || FORMAT_DATETIME("%Y-%m-%d", DATETIME_ADD(DATETIME_TRUNC(order_date, ISOWEEK), INTERVAL 6 DAY) )
  WHEN LOWER ( Date Level) = 'monthly' THEN FORMAT_DATETIME("%Y%m", order_date)
  WHEN LOWER(  Date Level) = 'quarterly' THEN FORMAT_DATETIME("%Y", order_date) || 'Q' || QUARTER(order_date)
  WHEN LOWER(  Date Level) = 'yearly (cy)' THEN FORMAT_DATETIME("%Y", order_date)
END
 
```

![image](https://github.com/user-attachments/assets/9a2526a6-6052-4f7e-ad43-f9789ef33d9c)



> 🗓️ **Note**: This example uses **ISO week**, where **Monday** is considered the first day of the week.


### 3. Use Your New Calculated Field in Visuals

In this step, I’ve created a pivot table using the calculated field, as shown in the image below.


![image](https://github.com/user-attachments/assets/4c902016-66fa-44b9-a8d7-46bbd3139ecc)


However, I have not yet changed the `order_date` to the new `Date Level Selection` field.  
To do this, go to the **Row dimension** section and select `Date Level Selection`.

![image](https://github.com/user-attachments/assets/3ac13722-91e7-4392-98c7-a371f7a6816f)

Now the table uses the calculated field that connects to the `Date Level` parameter.  
When users want to view **monthly sales** by product, they can simply select `'Monthly'` from the `Date Level` parameter.

**Monthly View:**


![image](https://github.com/user-attachments/assets/f348718b-cf59-4cf8-a54a-76ddab311b05)

**Quarterly View:**


![image](https://github.com/user-attachments/assets/3bc9c7fb-ca1b-4f1b-9881-abdf28ce4ef8)


> ℹ️ **Note**: Even though the calculated field is stored as text, the table still displays the sales data in the correct chronological order.

**Monthly View:**


![image](https://github.com/user-attachments/assets/3c2e6ef2-397d-44bd-81d7-d99e7b2669fe)

**Quarterly View:**


![image](https://github.com/user-attachments/assets/828e6a92-bf1f-4eae-93fa-608ac45350e8)


Now anyone can freely select the `Date Level`, and both charts will update dynamically 🎉😊

## 🧾 Summary

Although not all chart types are suitable for every date granularity, combining **Parameter Date Filters** with a **Calculated Field** empowers report users to manipulate the data and choose the view that best suits their needs.


## 🔗 Live Demo
Now, try it by yourself! For demonstration purposes, I built an interactive Looker Studio dashboard using a Google Sheets data source.

👉 [Open the Mockup Dashboard ↗](https://lookerstudio.google.com/reporting/363d8d76-928f-4ff4-a2fd-447eaf98b592)

Note: The “Quarterly” option has been removed in this version, as it did not work with the Google Sheets.

<!--
 which is not only hard to maintain in a long term, but also use more resources.

-->

