# 📈 Track Report Usage with GA4

Unlike Power BI or Tableau, which include built-in usage metrics, **Looker Studio** does not provide native tracking inside the workspace.  
To monitor report usage, you need to integrate with **GA4 (Google Analytics 4)**.  

---

## Why Usage Tracking Matters  

Building dashboards is always an **iterative process**.  
User needs can shift as business rules and priorities change. Without usage data, it’s impossible to know whether the reports we publish are:  
- Being accessed by the intended audience  
- Delivering value in line with current business requirements  

Tracking usage helps you validate whether your dashboard is actually serving its purpose.  

---

### Example 1: Refresh Requirements  

Imagine stakeholders request a dashboard that refreshes **every 4 hours** to monitor a newly launched campaign.  

A project manager will eventually ask:  
- *“Is this refresh interval still required?”*  
- *“Are users actively checking the report every 4 hours?”*  

If we cannot measure usage, we won’t know whether the effort invested in maintaining such a refresh schedule is justified.  

---

### Example 2: Changing Business Context  

Suppose you build a dashboard to track:  
- The number of emails sent to applicants  
- Compared against the number of applications received  

The email campaigns are scheduled to send weekly. But due to a technical issue, the emails **fail to send**. The responsible team notices a week later.  

In this scenario:  
- The dashboard no longer reflects reality  
- The business continues to incur maintenance cost for a dashboard that doesn’t meet its intended purpose  

---

## 🛠️ Steps to Enable GA4 Tracking in Looker Studio  

You can also review the [help](https://support.google.com/analytics/answer/9304153?visit_id=638932535016792303-3141444706&p=measure-report&rd=1#other) here.

1. **Create a GA4 Property**  
   - Go to Google Analytics (https://analytics.google.com/analytics/web/).
   - In **Google Analytics**, go to the **Admin** page and create a **new Account** if you do not already have one. This will give you a fresh space to create properties and data streams for tracking Looker Studio report usage.  

     <img width="1764" height="880" alt="image" src="https://github.com/user-attachments/assets/722d990f-ef18-4488-8cff-09f7d348e8f7" />
     
   - Follow the step to the Data Collection step:
     
     <img width="1814" height="886" alt="image" src="https://github.com/user-attachments/assets/c211816c-48cd-4434-aeff-a335cf273a27" />

     Select `Web` as a platform, enter website link, then click `Create & continue`.

     <img width="1531" height="583" alt="image" src="https://github.com/user-attachments/assets/c131a85e-ea17-4d08-8390-3c3fe1fdcf9c" />
  
     Click the new stream we created:
     <img width="1596" height="220" alt="image" src="https://github.com/user-attachments/assets/c1d43714-e2f8-4b66-b511-e4142d251edf" />

           
2. **Get the Measurement ID**  
   - Navigate to the Web Stream detail and copy the **Measurement ID** (e.g., `G-XXXXXXX`).
     
   <img width="1436" height="821" alt="image" src="https://github.com/user-attachments/assets/c04a4de6-1bba-4b72-b073-2e7994fed410" />

3. **Open Your Looker Studio Report**  
   - Go to your report and select **File → Report settings**.  

4. **Enable GA4 Tracking**  
   - In the settings panel, paste your **Measurement ID** into the Google Analytics tracking field.  
    <img width="1608" height="892" alt="image" src="https://github.com/user-attachments/assets/7a482dc1-1bc6-4351-b083-44007ded3fcd" />

5. **Publish the Report**  
   - Once enabled, GA4 will start recording report usage events (e.g., page views, sessions).  

6. **Validate in GA4**  
   - Go to GA4’s **Realtime Report** to confirm events are being logged as users view the report.  

---

## Key Takeaway  

Every dashboard comes with a cost.  
If a report is **not used** or **no longer aligned** with business needs, it should be adjusted — or even retired.  

By integrating GA4 with Looker Studio, you gain visibility into report usage and can make informed decisions about:  
- Which reports deserve continued investment  
- Which ones require updates  
- Which ones should be decommissioned  
