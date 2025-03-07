# air_quality-analysis
import pandas as pd
import matplotlib.pyplot as plt
import seaborn as sns

# Dynamic User Input for Cities and Air Quality Data
num_cities = int(input("Enter number of cities: "))
data = {'city': [], 'date': [], 'pm2_5': [], 'no2': [], 'so2': []}

for i in range(num_cities):
    city = input(f"Enter city {i+1} name: ")
    
    while True:
        date = input(f"Enter date for {city} (YYYY-MM-DD): ")
        try:
            pd.to_datetime(date)  # Validate date format
            break
        except ValueError:
            print("Invalid date format! Please enter in YYYY-MM-DD format.")
    
    while True:
        try:
            pm2_5 = float(input(f"Enter PM2.5 level for {city}: "))
            no2 = float(input(f"Enter NO2 level for {city}: "))
            so2 = float(input(f"Enter SO2 level for {city}: "))
            break
        except ValueError:
            print("Invalid input! Please enter numeric values.")

    data['city'].append(city)
    data['date'].append(date)
    data['pm2_5'].append(pm2_5)
    data['no2'].append(no2)
    data['so2'].append(so2)

df = pd.DataFrame(data)

# Convert date column to datetime format
df['date'] = pd.to_datetime(df['date'])

# Sort by date and city for better structure
df = df.sort_values(by=['date', 'city'])

# Basic Data Exploration
print(df.head())
print(df.info())

# PM2.5 Analysis
average_pm2_5 = df['pm2_5'].mean()
print("Average PM2.5 across cities:", average_pm2_5)

# City with Highest NO2 Levels
max_no2_city = df.loc[df['no2'].idxmax(), ['city', 'no2']]
print("City with Highest NO2 Levels:", max_no2_city.to_dict())

# Correlation Analysis
correlation_matrix = df[['pm2_5', 'no2', 'so2']].corr()
print("\nCorrelation Matrix:\n", correlation_matrix)

# PM2.5 Levels by City (Bar Chart)
plt.figure(figsize=(10,5))
sns.barplot(x='city', y='pm2_5', data=df,hue='city' ,palette='Blues',legend=False)
plt.xticks(rotation=45)
plt.xlabel("City")
plt.ylabel("PM2.5")
plt.title("PM2.5 Levels by City")
plt.show()

# Air Quality Trends Over Time (Line Plot)
plt.figure(figsize=(10,5))
sns.lineplot(x='date', y='pm2_5', hue='city', data=df, marker='o')
plt.xticks(rotation=45)
plt.xlabel("Date")
plt.ylabel("PM2.5 Levels")
plt.title("PM2.5 Levels Over Time")
plt.legend(title="City")
plt.show()

