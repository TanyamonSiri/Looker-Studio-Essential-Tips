
# Parameter Date Filters & Slicers

There were many times that I got a requirement of sales with different date granularity. <br>
In this scenario, Finance team wanted to view sales report in monthly, Marketing team wanted to see the sales in daily and weekly to track their promotions performance, etc.
In order to achieve this, I found that some data analysts build different reports to support each department's need, or sometimes they separated the granularity into each pages instead.

Therefore, even though they are the same sales reports, a lot of report duplication was made. 
The countermeasure for this problem is to use a parameter.
Parameter can be used to manipulate data in Looker Studio or any other Business Intelligence tools like Power BI and Tableau.

💡 Note that you can apply this technique to other BI tools as well :-) 

## 📌Data Requirement 
- `date` as date type

## Steps
<b> 1. Create Parameter in Looker Studio </b> <br>
In this step, you have to firstly create a parameter of all granularities you need.<br>
In the picture below, you can see that I created a list of values including `Daily`, `Weekly`, `Monthly`, `Quarterly`, and `Yearly (CY)`. <br>
I named this parameter as 'Date Level'

![image](https://github.com/user-attachments/assets/aeff83c5-28ca-4c8c-a9fc-c7f842e0ffb7)

<b> 2. Create a new Calculation field </b> <br>
In this step, you will need to create a calculated field to link with the 'Date Level' parameter. <br>
My data has `payment_date` as a date dimension so I used simple `CASE WHEN` in this case:

``` sql
 CASE
    WHEN  LOWER(  Date Level) = 'daily' THEN FORMAT_DATETIME("%Y-%m-%d", payment_date)
    WHEN LOWER( Date Level) = 'weekly' THEN FORMAT_DATETIME("%Y-%m-%d", DATETIME_TRUNC(payment_date, ISOWEEK)) || ' to ' || FORMAT_DATETIME("%Y-%m-%d", DATETIME_ADD(DATETIME_TRUNC(payment_date, ISOWEEK), INTERVAL 6 DAY) )
    WHEN LOWER (Date Level) = 'monthly' THEN FORMAT_DATETIME("%Y%m", payment_date)
    WHEN LOWER( Date Level) = 'quarterly' THEN FORMAT_DATETIME("%Y", payment_date) || 'Q' || QUARTER(payment_date)
    WHEN LOWER( Date Level) = 'yearly' THEN FORMAT_DATETIME("%Y", payment_date)
END
 
```


![image](https://github.com/user-attachments/assets/bfcd94f0-6b32-4d30-ae9e-8f4d24bc3839)


In this case, I used an ISO week which has monday as a week start day 😊

<b> 3. Use your new Calculation field in visuals </b> <br>


<!--
 which is not only hard to maintain in a long term, but also use more resources.

-->
