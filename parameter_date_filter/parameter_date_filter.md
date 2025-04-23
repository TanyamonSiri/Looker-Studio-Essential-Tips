
# Parameter Date Filters & Slicers

There were many times that I got a requirement of sales with different date granularity. <br>
In this scenario, Finance team wanted to view sales report in monthly, Marketing team wanted to see the sales in daily and weekly to track their promotions performance, etc.
In order to achieve this, I found that some data analysts build different reports to support each department's need, or sometimes they separated the granularity into each pages instead.

Therefore, eventhough they are the same sales reports, a lot of report duplication was made, which is not only hard to maintain in a long term, but also use more resources.
The countermeasure for this problem is to use a parameter.
Parameter can be used to manipulate data in Looker Studio or any other Business Intelligence tools like Power BI and Tableau.

💡 Note that you can apply this technique to other BI tools as well :-) 

## 📁 Sample Data
- [mockup_demand_sales]()
## 📌 Data Requirement 
- `date` as date type

## Steps
### <b> 1. Create Parameter in Looker Studio </b> <br>
In Looker Studio, you have to firstly create a parameter of all granularities you need.<br>
In the picture below, you can see that I created a list of values including `Daily`, `Weekly`, `Monthly`, `Quarterly`, and `Yearly (CY)`. <br>
I named this parameter as 'Date Level'

![image](https://github.com/user-attachments/assets/aeff83c5-28ca-4c8c-a9fc-c7f842e0ffb7)

### <b> 2. Create a new Calculation field </b> <br>
In this step, you will need to create a calculated field to link with the 'Date Level' parameter. <br>
My data has `order_date` as a date dimension so I used simple `CASE WHEN` in this case:

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



In this case, I used an `ISO week` which has monday as a week start day 😊

### <b> 3. Use your new Calculation field in visuals </b> <br>
In this step, I have already created a pivot table as shown in the picture below. <br>

![image](https://github.com/user-attachments/assets/4c902016-66fa-44b9-a8d7-46bbd3139ecc)


But I have not changed my `order_date` to `Date Level Selection` yet, so let's change it.
At <b> Row dimension</b> section, select `Date Level Selection`.  
![image](https://github.com/user-attachments/assets/3ac13722-91e7-4392-98c7-a371f7a6816f)

Now my table has calculated field that connecting to `Date Level` parameter.
When users want to view monthly sales of each product, they can simply select 'Monthly' in `Date Level` parameter.

<ins> Monthly View: </ins>

![image](https://github.com/user-attachments/assets/f348718b-cf59-4cf8-a54a-76ddab311b05)

<ins> Quarterly View: </ins>

![image](https://github.com/user-attachments/assets/3bc9c7fb-ca1b-4f1b-9881-abdf28ce4ef8)

Notice that even though the calculated field's type is text, the table shows sales data in a correct order.

### <b> 4. Enhance your report </b> <br>
Since, parameter date filter and calculated field are not limited to only table, in this case I created a stacked bar chart.  <br>

<ins> Monthly View: </ins>

![image](https://github.com/user-attachments/assets/3c2e6ef2-397d-44bd-81d7-d99e7b2669fe)

<ins> Quarterly View: </ins>

![image](https://github.com/user-attachments/assets/828e6a92-bf1f-4eae-93fa-608ac45350e8)


Now anyone can freely select the `date level` and both charts would change spontanously 🍾😊 

## Summary
Eventhough not all charts type are suitable for every date dimension. <br>
By combining Parameter Date Filters and Calculated Field, report users can freely manipulate their data and select the view that suiting with their needs.


<!--
 which is not only hard to maintain in a long term, but also use more resources.

-->

