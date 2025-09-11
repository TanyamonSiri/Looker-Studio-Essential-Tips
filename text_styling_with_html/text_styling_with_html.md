
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

From the list, select **Tell Me Light**.  

<img width="1814" height="728" alt="image" src="https://github.com/user-attachments/assets/9f2e07e9-ba32-469e-8fab-d482ce76c1aa" />  

---

### 2. Choose the Template Option  

The **Tell Me Light** visualisation offers several template styles. For this demo, select **Custom** in the **Style** tab.  

<img width="353" height="629" alt="image" src="https://github.com/user-attachments/assets/8cc60fea-2414-4f05-8fca-537a3cee20a7" />  

The **Text Template** defines how your metrics will be displayed. For example:  

- `{{label "metric.0"}}` → shows the label of the first metric  
- `{{value "metric.0" 0}}` → shows the value of the first metric  

In my case, `Record Count` is the first metric, so the visual displays:  

<img width="461" height="346" alt="image" src="https://github.com/user-attachments/assets/0e25c03a-a1ec-4723-a344-f364ac86cf91" />  

<img width="248" height="678" alt="image" src="https://github.com/user-attachments/assets/13f4d756-2f88-414f-8f04-350292876483" />  

Here, the card simply shows that my dataset contains **224 records**.  

---

### 3. Add Calculated Fields for Dynamic Text  

Now, let’s update the **Text Template** to include a calculated field with the date range:  

```html
Between {{date-range "start"}} and {{date-range "end"}},
the total sales for both Product A and Product B is {{ value "metric.0" 0 }}
```
Result in the report:

<img width="1251" height="587" alt="image" src="https://github.com/user-attachments/assets/1b802c18-7bf0-4cba-a119-3f7e3e90dca7" />

If I adjust the date range (e.g., ending on 31 March 2025), the text updates automatically:

<img width="1244" height="609" alt="image" src="https://github.com/user-attachments/assets/20f52649-81ae-46a5-8f40-6cf877ed01c8" />

### 4. Enhance with HTML Styling 
Finally, let’s improve the readability by applying bold tags and a highlight emoji:

``` html
💡Between {{date-range "start"}} and {{date-range "end"}},
the total sales for both <b> Product A </b> and <b> Product B </b> is <b> {{ value "metric.0" 0}} </b>.

```

<img width="1235" height="597" alt="image" src="https://github.com/user-attachments/assets/31acb144-40fe-466b-a12f-d4b56c814f5b" />

## ✅ Final Result

Your card will now dynamically update whenever the date range changes — giving you an instant, presentation-ready text summary that saves time and looks professional

## 🔗 Live Demo
For demonstration purposes, I built an interactive Looker Studio dashboard using a Google Sheets data source.

👉 [Open the Mockup Dashboard ↗](https://lookerstudio.google.com/reporting/363d8d76-928f-4ff4-a2fd-447eaf98b592)


