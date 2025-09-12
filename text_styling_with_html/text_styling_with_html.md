
# 📊 Dynamic Text Styling with HTML in Looker Studio

Do you ever find yourself preparing multiple decks with the *same* information, just presented differently? Often, scorecards and plain text in Looker Studio aren’t enough to create a polished, presentation-ready report.  

Wouldn’t it be better to have a **dynamic summary** that automatically adapts to your date range or metrics? This will saving you time and making your dashboards look more professional.  

The good news is: you can achieve this with just a little bit of **HTML** combined with Looker Studio’s **calculated fields**. And don’t worry — you only need a handful of basic HTML tags to get started.  

👉 If you want to learn more about HTML in depth, I recommend this [HTML Tutorial](https://www.w3schools.com/Html/).  

---

## 🛠️ Step-by-Step Guide

### 1. Enable Community Visualisations  
In Looker Studio, go to **Community visualisations and components**, then click **Explore More**.  

![image](https://github.com/user-attachments/assets/9c5ab436-9ec5-4585-bc76-76bb5f937f7b)  

From the list, select [**Tell Me Light**](https://lookerstudio.google.com/u/0/reporting/6a211d82-26ef-4a4d-920c-7d5a2db2c279/page/MLdGB).  

<img width="1814" height="728" alt="image" src="https://github.com/user-attachments/assets/9f2e07e9-ba32-469e-8fab-d482ce76c1aa" />  

We can use this chart for free! (with limited features ofcourse), but free version is enough for our demo.

---

### 2. Choose the Template Option  

The **Tell Me Light** visualisation offers several template styles. For this demo, **Custom** is selected in the **Style** tab.  

<img width="353" height="629" alt="image" src="https://github.com/user-attachments/assets/8cc60fea-2414-4f05-8fca-537a3cee20a7" />  

The **Text Template** defines how your metrics will be displayed. For example:  

- `{{label "metric.0"}}` → shows the label of the first metric  
- `{{value "metric.0" 0}}` → shows the value of the first metric  

In my case, `Record Count` is the first metric, so the visual displays:  

<img width="461" height="346" alt="image" src="https://github.com/user-attachments/assets/0e25c03a-a1ec-4723-a344-f364ac86cf91" />  

<img width="248" height="678" alt="image" src="https://github.com/user-attachments/assets/13f4d756-2f88-414f-8f04-350292876483" />  

Here, the card simply shows that my dataset contains **224 records**.  

This language is called `m code` and for more information, feel free to visit [Supermetrics Chart](https://www.youtube.com/watch?v=csN5Wph5xcE) 

---

### 3. Add Calculated Fields for Dynamic Text  

Now, let’s update the **Text Template** including `demand_sales` as a matrics with the date range:  

<img width="483" height="576" alt="image" src="https://github.com/user-attachments/assets/91233c71-eef6-4950-b674-4853efe17a67" />

New Text Template:
```html
Between {{date-range "start"}} and {{date-range "end"}},
the total sales for both Product A and Product B is {{ value "metric.0" 0 }}
```
Result in the report:

<img width="1251" height="587" alt="image" src="https://github.com/user-attachments/assets/1b802c18-7bf0-4cba-a119-3f7e3e90dca7" />

If I adjust the date range (e.g., ending on 31 March 2025), the text updates automatically:

<img width="1244" height="609" alt="image" src="https://github.com/user-attachments/assets/20f52649-81ae-46a5-8f40-6cf877ed01c8" />

### 4. Enhance with HTML Styling 
Finally, let’s improve the readability by applying card styling background, bold tags and a highlight emoji:

New Text Template:
``` html
<div style="background:#F9F9F9; border:1px solid #E0E0E0; padding:10px; border-radius:6px;">
💡Between  <b><span style="color:#5C6BC0">{{date-range "start"}} </span></b> and 
<b><span style="color:#5C6BC0">{{date-range "end"}} </span></b>,
the total sales for both Product A  and  Product B  is <b> {{ value "metric.0" 0}} </b>.
</div>

```

<img width="1220" height="614" alt="image" src="https://github.com/user-attachments/assets/0df07ebd-e5e1-46bf-bafc-e3ef3d7b15ac" />


> 📝 **Note**: While the *Tell Me Light* chart includes easy-to-use UI options for styling,  
> this demo highlights how you can **integrate HTML with M code** to achieve more flexible, custom formatting.


Now, let's move the visuals a little, so it is easier to tell story.

<img width="1219" height="629" alt="image" src="https://github.com/user-attachments/assets/19ba6a3f-82b2-4661-8588-c9c964a796d5" />



Just total sales is not enough to show the insights underlying in the dataset. Let's create two calculated fields to display sales by each product.

`product_A_sales`:

<img width="1092" height="403" alt="image" src="https://github.com/user-attachments/assets/9fdecf70-5ffc-4f69-84bf-b9bfe6d44932" />

`product_B_sales`:

<img width="1090" height="405" alt="image" src="https://github.com/user-attachments/assets/0ec556aa-54f6-4980-9765-d341807c64e3" />

*Tell Me Light* chart also allow comparison value for trend as well, the macro text below is adapted from [Macro language Advanced Example](https://lookerstudio.google.com/u/0/reporting/6a211d82-26ef-4a4d-920c-7d5a2db2c279/page/p_bjcrgagp5c)

In this deme, we will compare the sales with the previous period which can be selected in the Setup.

<img width="248" height="557" alt="image" src="https://github.com/user-attachments/assets/e62356c2-cde0-439d-b6c9-10aa35767af5" />


New Text Template:
``` html
<div style="background:#F9F9F9; border:1px solid #E0E0E0; padding:10px; border-radius:6px;">
Between  <b><span style="color:#5C6BC0">{{date-range "start"}} </span></b> and 
<b><span style="color:#5C6BC0">{{date-range "end"}} </span></b>,
the total sales for both products  is <b> {{ value "metric.0" 0}} </b>.
({{ delta "metric.0" "sum" }} vs Previous Period)
<br>
▪️ Product A sales has been
{{#if (deltaIsBigger "metric.1" "sum")}}up by {{ delta "metric.1" "sum" }} ({{ deltaP "metric.1" "sum" }}) {{/if}}
{{#if (deltaIsSmaller "metric.1" "sum")}}down by {{ delta "metric.1" "sum" }} ({{ deltaP "metric.1" "sum" }}) {{/if}}
{{#if (deltaIsEqual "metric.1" 1)}}unchanged{{/if}}
<br>
▪️ Product B sales has been
{{#if (deltaIsBigger "metric.2" "sum")}}up by {{ delta "metric.2" "sum" }} ({{ deltaP "metric.2" "sum" }}) {{/if}}
{{#if (deltaIsSmaller "metric.2" "sum")}}down by {{ delta "metric.2" "sum" }} ({{ deltaP "metric.2" "sum" }}) {{/if}}
{{#if (deltaIsEqual "metric.2" 1)}}unchanged{{/if}}
</div>

```

If we change the date granularity to "Weekly" and date range to March 3, 2025 and March 23, 2025

Result in the report:

<img width="1220" height="688" alt="image" src="https://github.com/user-attachments/assets/02c9b195-64c8-40cf-81b5-dad74616b15b" />

Notice that I also open the grand total values for the table, so that we can cross check the calculation.

Cross check with the previous period value:

<img width="1223" height="688" alt="image" src="https://github.com/user-attachments/assets/d5f1acf0-fcb9-4cf7-bb66-fb6425904cf5" />

> 📝 **Summary**: This card compares the current period’s total sales with the previous period.  

## ✅ Final Result

After publish the result, your card will now dynamically update whenever the date range changes — giving you an instant, presentation-ready text summary that saves time and looks professional

<img width="1794" height="913" alt="image" src="https://github.com/user-attachments/assets/d323fcd8-0425-456f-a9e7-9dfa5484c465" />


## 🔗 Live Demo
For demonstration purposes, I built an interactive Looker Studio dashboard using a Google Sheets data source.

👉 [Open the Mockup Dashboard ↗](https://lookerstudio.google.com/reporting/363d8d76-928f-4ff4-a2fd-447eaf98b592)


