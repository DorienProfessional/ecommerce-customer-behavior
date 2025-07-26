

```r
library(tidyverse)

data <- read_csv("data/ecommerce_data.csv")
glimpse(data)
summary(data)
data <- data %>% 
  janitor::clean_names()
data <- data %>% 
  filter(!is.na(customer_id), !is.na(order_id))
data <- data %>% 
  mutate(order_date = as.Date(order_date, format = "%Y-%m-%d"))
data <- data %>% 
  mutate(category = stringr::str_to_lower(category))
repeat_buyers <- data %>% 
  group_by(customer_id) %>% 
  filter(n() > 1)
write_csv(data, "data/cleaned_ecommerce_data.csv")

