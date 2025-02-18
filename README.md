# whale-transactions-analysis
# Whale Transactions & Bitcoin Price Analysis  
**Analyzing how large Bitcoin transactions impact market prices using SQL and Tableau.**  

## 📌 Overview  
This project examines **whale transactions** (large BTC movements) and their effect on **Bitcoin price trends.**  
We use **SQL for data analysis** and **Tableau for visualizations.**  

## 📊 Data Sources  
- **Whale Transactions Dataset** (`whale_transactions.csv`)  
- **Bitcoin Price Data** (`bitcoin_price.csv`)  
- **SQL Query Results** (`whale_vs_btc_change.csv`)  

## 🔍 Key Insights  
### 1️⃣ BTC Price Change Before & After Whale Transactions  
- **Green Bars:** BTC price increased after a whale transaction 📈  
- **Red Bars:** BTC price dropped after a whale transaction 📉  

### 2️⃣ Whale Transactions vs. BTC Price Over Time  
- **Bars:** Show whale transaction volume (USD)  
- **Line:** BTC price trend  

### 3️⃣ Whale Transactions vs. BTC Price Change (Scatter Plot)  
- **Each dot represents a whale transaction**  
- **Red = BTC price dropped after transaction**  
- **Green = BTC price increased after transaction**  

## ⚙️ SQL Queries Used  
```sql
-- Example Query: Find the Largest Whale Transactions
SELECT * FROM whale_transactions ORDER BY USD_Volume DESC LIMIT 5;
# Whale Transactions & Bitcoin Price Analysis  
**Analyzing how large Bitcoin transactions impact market prices using SQL and Tableau.**  

## 📌 Overview  
This project examines **whale transactions** (large BTC movements) and their effect on **Bitcoin price trends.**  
We use **SQL for data analysis** and **Tableau for visualizations.**  

## 📊 Data Sources  
- **Whale Transactions Dataset** (`whale_transactions.csv`)  
- **Bitcoin Price Data** (`bitcoin_price.csv`)  
- **SQL Query Results** (`whale_vs_btc_change.csv`)  

---

## 🔍 Key Insights  
### 1️⃣ BTC Price Change Before & After Whale Transactions  
- **Green Bars:** BTC price increased after a whale transaction 📈  
- **Red Bars:** BTC price dropped after a whale transaction 📉  

### 2️⃣ Whale Transactions vs. BTC Price Over Time  
- **Bars:** Show whale transaction volume (USD)  
- **Line:** BTC price trend  

### 3️⃣ Whale Transactions vs. BTC Price Change (Scatter Plot)  
- **Each dot represents a whale transaction**  
- **Red = BTC price dropped after transaction**  
- **Green = BTC price increased after transaction**  

---

## ⚙️ SQL Queries Used  
Below are the SQL queries used to analyze the data.  

### **1️⃣ Finding the Largest Whale Transactions**
```sql
SELECT * 
FROM whale_transactions
ORDER BY USD_Volume DESC
LIMIT 5;
SELECT w.Date, w.BTC_Amount, w.USD_Volume, 
       (SELECT Close_Price FROM bitcoin_price b1 WHERE b1.Date <= w.Date ORDER BY b1.Date DESC LIMIT 1) AS Price_Before,
       (SELECT Close_Price FROM bitcoin_price b2 WHERE b2.Date >= DATE_ADD(w.Date, INTERVAL 3 DAY) ORDER BY b2.Date ASC LIMIT 1) AS Price_After,
       ((SELECT Close_Price FROM bitcoin_price b2 WHERE b2.Date >= DATE_ADD(w.Date, INTERVAL 3 DAY) ORDER BY b2.Date ASC LIMIT 1) - 
        (SELECT Close_Price FROM bitcoin_price b1 WHERE b1.Date <= w.Date ORDER BY b1.Date DESC LIMIT 1)) AS Price_Change,
       (((SELECT Close_Price FROM bitcoin_price b2 WHERE b2.Date >= DATE_ADD(w.Date, INTERVAL 3 DAY) ORDER BY b2.Date ASC LIMIT 1) - 
        (SELECT Close_Price FROM bitcoin_price b1 WHERE b1.Date <= w.Date ORDER BY b1.Date DESC LIMIT 1)) /
        (SELECT Close_Price FROM bitcoin_price b1 WHERE b1.Date <= w.Date ORDER BY b1.Date DESC LIMIT 1)) * 100 AS Price_Change_Percentage
FROM whale_transactions w;
SELECT 
    SUM((w.BTC_Amount - (SELECT AVG(BTC_Amount) FROM whale_transactions)) * 
        (b.Close_Price - (SELECT AVG(Close_Price) FROM bitcoin_price))) / 
    (SQRT(SUM(POW(w.BTC_Amount - (SELECT AVG(BTC_Amount) FROM whale_transactions), 2))) * 
     SQRT(SUM(POW(b.Close_Price - (SELECT AVG(Close_Price) FROM bitcoin_price), 2)))) 
     AS BTC_Whale_Correlation
FROM whale_transactions w
JOIN bitcoin_price b ON w.Date = b.Date;
💡 Conclusion

This analysis helps understand how major Bitcoin transactions impact price movements.
📌 Final findings:
✅ Whale transactions have some impact on BTC price, but not always.
✅ The correlation is weak (~0.23), meaning other factors also influence BTC price.
✅ Larger transactions don’t always mean bigger price changes.
## 📸 Visualizations  
| Chart | Description |
|-------|------------|
| ![BTC Price Change](https://github.com/ishatubarry/whale-transactions-analysis/blob/main/YOUR_SCREENSHOT_FILE_1.png) | BTC price before & after whale transactions |
| ![Whale Transactions vs BTC](https://github.com/ishatubarry/whale-transactions-analysis/blob/main/YOUR_SCREENSHOT_FILE_2.png) | Whale transactions vs. BTC price |
| ![Scatter Plot](https://github.com/ishatubarry/whale-transactions-analysis/blob/main/YOUR_SCREENSHOT_FILE_3.png) | Whale transactions vs. BTC price change |
