# Load necessary libraries
library(dplyr)
library(ggplot2)
library(lubridate)
library(tidyr)

# Step 1: Preview the dataset structure
glimpse(ecommerce_data)

# Step 2: Convert event_time to a proper datetime format
ecommerce_data <- ecommerce_data %>%
  mutate(event_time = as_datetime(event_time))

# -----------------------------------------------------
# Step 3: Identify how many customers are repeat buyers
# -----------------------------------------------------
# Filter for purchases only
purchase_data <- ecommerce_data %>%
  filter(event_type == "purchase")

# Count purchases per customer
repeat_buyers <- purchase_data %>%
  group_by(customer_id) %>%
  summarise(purchase_count = n()) %>%
  mutate(repeat_buyer = purchase_count > 1)

# Summarize how many are repeat vs one-time buyers
repeat_summary <- repeat_buyers %>%
  count(repeat_buyer) %>%
  mutate(percent = n / sum(n) * 100)

print(repeat_summary)

# --------------------------------------------
# Step 4: Calculate Average Order Value (AOV)
# --------------------------------------------
# Group by customer and calculate AOV
aov <- purchase_data %>%
  group_by(customer_id) %>%
  summarise(
    total_spent = sum(price),
    total_orders = n(),
    avg_order_value = total_spent / total_orders
  )

# Summary of AOV across all customers
summary(aov$avg_order_value)

# ---------------------------------------------------
# Step 5: Build a customer journey funnel visualization
# ---------------------------------------------------
# Count total events by type (view → add_to_cart → purchase)
event_funnel <- ecommerce_data %>%
  filter(event_type %in% c("view", "add_to_cart", "purchase")) %>%
  count(event_type) %>%
  arrange(desc(n))

# Visualize the funnel
ggplot(event_funnel, aes(x = reorder(event_type, -n), y = n, fill = event_type)) +
  geom_col() +
  labs(title = "Customer Journey Funnel",
       x = "Event Type",
       y = "Count") +
  theme_minimal()

# -----------------------------------------------------
# Step 6: Analyze purchases by day of the week
# -----------------------------------------------------
# Extract day of week from timestamp and count purchases
purchase_data %>%
  mutate(weekday = wday(event_time, label = TRUE)) %>%
  count(weekday) %>%
  ggplot(aes(x = weekday, y = n)) +
  geom_line(group = 1, color = "steelblue") +
  geom_point() +
  labs(title = "Purchases by Day of Week",
       x = "Day",
       y = "Number of Purchases") +
  theme_minimal()

# -----------------------------------------------------
# Step 7: Perform RFM Analysis (Recency, Frequency, Monetary)
# -----------------------------------------------------
# Calculate RFM metrics for each customer
rfm_data <- purchase_data %>%
  group_by(customer_id) %>%
  summarise(
    recency = as.numeric(difftime(Sys.Date(), max(event_time), units = "days")),
    frequency = n(),
    monetary = sum(price)
  )

# Step 8: Create RFM score by segmenting each metric into quintiles (1-5)
rfm_data <- rfm_data %>%
  mutate(
    r_score = ntile(-recency, 5), # lower recency = better score
    f_score = ntile(frequency, 5),
    m_score = ntile(monetary, 5),
    rfm_score = r_score + f_score + m_score
  )

# View top of RFM table
head(rfm_data)
