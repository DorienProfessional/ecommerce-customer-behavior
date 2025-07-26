# 🔍 Analyze Step – Capstone Project: What Makes Customers Repeat Buyers in E-Commerce

## 📊 Overview
In this step, I explored the e-commerce dataset to identify behaviors and factors that influence repeat purchases. The goal was to clean, organize, and analyze the data to reveal patterns that may help businesses improve customer retention.

---

## 📦 1. Aggregate the Data
I grouped the dataset by `Customer_ID` to calculate:
- Total number of orders per customer
- Total amount spent per customer
- A new field called `Repeat_Buyer` to identify customers with more than one order

This helped separate repeat buyers from one-time customers.

---

## 📐 2. Organize and Format the Data
- Cleaned up column names for clarity
- Reformatted numerical values and date columns
- Sorted columns by customer, order count, repeat status, and total spent

Example structure:
| Customer_ID | Repeat_Buyer | Order_Count | Total_Spent |
|-------------|---------------|-------------|-------------|
| 1001        | Yes           | 3           | $185.75     |
| 1002        | No            | 1           | $52.00      |

---

## 🔢 3. Perform Calculations
I calculated:
- **Average order value** per customer
- **Repeat purchase rate** across all customers
- **Top product categories** among repeat buyers
- **Average delivery time** for repeat vs. non-repeat customers

---

## 📈 4. Identify Trends and Relationships
Early trends show:
- Repeat buyers typically spend more than one-time buyers.
- Certain categories, like **electronics** and **fashion**, have higher repeat rates.
- Faster delivery times may be associated with higher return rates.

These patterns will help shape business strategies focused on retention and loyalty.

---

## 🧠 Next Steps
- Create visualizations comparing spending behavior
- Investigate seasonal trends in customer return rates
- Explore the effect of delivery times or promotions on repeat buying

