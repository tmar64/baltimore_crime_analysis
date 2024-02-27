# baltimore_crime_analysis

Using a public Kaggle dataset containing century worth of Crime Data in Baltimore City!

As a Baltimore resident, crime is hard to ignore and impacts life for me and many others, this inspired me to do analysis on the data :)


### IMPORT
```ruby
import os
import pandas as pd
import numpy as np
import matplotlib.pyplot as plt
import seaborn as sns
import folium
from folium.plugins import HeatMap
sns.set_theme()
```

### DATA CLEANING PROCESS
```ruby
missing_values_weapon = df["Weapon"].isnull()
missing_value_counter = 0

for i in range(516635):
    if missing_values_weapon[i]:
        missing_value_counter += 1

print(missing_value_counter)

# splitting DATE column to check years, months, and days for anomaly
data_parts = df['Date'].str.split("/", expand=True)

anomaly_year = 0
anomaly_month = 0
anomaly_day = 0

years = data_parts[0]
months = data_parts[1]
days = data_parts[2]

numbers_month = [str(i).zfill(2) for i in range(1, 13)]
numbers_day = [str(i).zfill(2) for i in range(1, 32)]

for year in years:
    if not (year.startswith("19") or year.startswith("20")):
        anomaly_year += 1

for month in months:
    if not (month in numbers_month):
        anomaly_month += 1

for day in days:
    if not (day in numbers_day):
        anomaly_day += 1

print("Number of anomalies in year: ", anomaly_year)
print("Number of anomalies in month: ", anomaly_month)
print("Number of anomalies in day: ", anomaly_day)
print()
```
Anomaly Report:

409,301 of the 516,635 WEAPON fields were blank, that is 79.26%!
Anomalies in year:  126 / Many of the years were formatted incorrectly (1024)
Anomalies in month:  0
Anomalies in day:  0


### CREATING THE DATAFRAMES VISULIZATIONS
```ruby
# converting the strings from date/time columns to usable objects
df['Date'] = pd.to_datetime(df['Date'])
df['Time'] = pd.to_datetime(df['Time'])

# Create the DataFrame with the top 10 most frequent dates and their counts
df['Month'] = df['Date'].dt.month
top_dates = df['Month'].value_counts().head(12)
top_dates_df = top_dates.reset_index()
top_dates_df.columns = ['Month', 'Count']

# Create a bar plot using Matplotlib
sns.barplot(x='Month', y='Count', hue=False, data=top_dates_df, palette='Blues', legend=False)
plt.title('Total Crime in Baltimore by Month: Aug 1921- April 2022')
plt.xlabel('Month', labelpad=10)
plt.ylabel('Crime Count', labelpad=10)

months_order = ['Jan', 'Feb', 'Mar', 'Apr', 'May', 'Jun', 'Jul', 'Aug', 'Sep', 'Oct', 'Nov', 'Dec']
plt.xticks(ticks=range(12), labels=months_order)

# Rotate the x-axis labels for better readability
plt.xticks(rotation=0)

# Show the plot
plt.tight_layout()
plt.show()
```


