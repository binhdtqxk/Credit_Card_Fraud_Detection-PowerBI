# **💳 Credit Card Fraud Detection Analysis: BI Dashboard**

### **📌 Project Overview**

Credit card fraud is a major concern for financial institutions. The challenge in fraud detection is the extreme data imbalance: in this dataset of European cardholders, out of 284,807 transactions, only 492 were fraudulent (accounting for a mere 0.17%).

The goal of this project is to build an interactive Power BI dashboard that uncovers hidden patterns of fraudulent activities, answering three core questions:

- When do fraudsters strike the most?
- How much do they usually steal per transaction?
- What is the actual financial impact on the bank?

Dataset: [Credit Card Fraud Detection](https://www.kaggle.com/datasets/mlg-ulb/creditcardfraud)

### **🛠 Tech Stack**

- **BI Tool:** Power BI - Data Visualization, DAX Modeling, Active Cross-Filtering.
- **Domain:** Risk Management, Financial Analytics.
- **Environment:** Power BI Desktop.

### **📑 Workflow & Technical Implementation**

**1. Data Processing & DAX Modeling (Power BI)**
To extract meaningful insights from raw data, several transformations and DAX measures were applied directly within the BI environment:

- **Time Series Transformation:** Converted raw elapsed seconds into a 24-hour cyclical format (MOD 24) to identify daily seasonality and peak hours of fraudulent activities.
    
    ```sql
    Hour = MOD(INT(creditcard[Time] / 3600), 24)
    ```
    
- **Amount Binning Strategy:** Instead of using raw transaction amounts, categorizations were created for specific bins (e.g., Micro <$50, Small $50-$200, Extreme >$1000) using a DAX SWITCH statement. This was crucial to isolate "card-testing" behaviors from "high-impact" thefts.
    
    ```sql
    Amount Category = SWITCH(
        TRUE(),
        creditcard[Amount]=0,"Zero",
        creditcard[Amount]<=50,"Micro(<50)",
        creditcard[Amount]<=200,"Small(50-200)",
        creditcard[Amount]<=500,"Medium(200-500)",
        creditcard[Amount]<=1000,"High(500-1000)",
        creditcard[Amount]>1000,"Extreme(>1000)",
        "Other"
    )
    ```
    
- **Cross-Filtering Optimization:** Designed a "Corporate Dark" UI with active cross-filtering, allowing stakeholders to slice data seamlessly by Day and view isolated fraud behaviors without being overwhelmed by normal transaction noise.

**💡 Key Business Insights**
Based on the dashboard visualizations, several critical fraud patterns were identified:

- **The "Micro-Transaction" Tactic (Test Cards):** Over 56% of fraud cases involved extremely small amounts (Micro <$50). This indicates that criminals often run small test charges to check if a stolen card is active before committing larger thefts.
- **The "Golden Hours" of Fraud:** Fraudulent activities do not happen randomly. The density of fraud spikes significantly during specific off-hours, peaking explicitly around 2:00 AM and 11:00 AM.
- **High Frequency vs. High Impact:** While Micro transactions are the most frequent, the Extreme (>$1000) and Medium ($200-$500) categories cause the most severe financial damage (Total Fraud Loss) to the bank.

**🚀 Strategic Recommendations**
To mitigate financial losses, the bank's fraud prevention system should implement the following rules:

- **Targeted Authentication:** Trigger secondary authentication (OTP/Call) for consecutive Micro (<$50) transactions occurring between 1:00 AM and 3:00 AM.
- **Automated Flagging:** Automatically flag and temporarily block accounts showing a sudden jump from a Micro transaction to an Extreme (>$1000) transaction within a short timeframe.
