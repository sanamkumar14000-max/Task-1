import csv
import io
from collections import defaultdict

# 1. Embed the data as a string so it runs instantly without file uploads
csv_data = """country,continent,population,Cases,Recovered,Deaths
USA,North-America,334805269.0,111820082,109814428.0,1219487.0
India,Asia,1406631776.0,45035393,,533570.0
France,Europe,65584518.0,40138560,39970918.0,167642.0
Germany,Europe,83883596.0,38828995,38240600.0,183027.0
Brazil,South-America,215353593.0,38743918,36249161.0,711380.0
S-Korea,Asia,51329899.0,34571873,34535939.0,35934.0
Japan,Asia,125584838.0,33803572,,74694.0
Italy,Europe,60262770.0,26723249,26361218.0,196487.0
UK,Europe,68497907.0,24910387,24678275.0,232112.0
Russia,Europe,145805947.0,24124215,23545818.0,402756.0"""

# 2. Parse the CSV data
reader = csv.DictReader(io.StringIO(csv_data))
data = list(reader)

# Convert string numbers into actual integers/floats for math
for row in data:
    row['Cases'] = int(row['Cases']) if row['Cases'] else 0
    row['Deaths'] = float(row['Deaths']) if row['Deaths'] else 0.0

print("=" * 50)
print(" COVID-19 DATA SCIENCE CONSOLE DASHBOARD")
print("=" * 50)

# --- Analysis 1: ASCII Bar Chart of Top Countries ---
print("\n1. TOP COUNTRIES BY TOTAL CASES")
print("-" * 50)

# Sort the data highest to lowest based on cases
sorted_by_cases = sorted(data, key=lambda x: x['Cases'], reverse=True)

# Find the maximum cases to scale our text-based bar chart properly
max_cases = sorted_by_cases[0]['Cases']

for row in sorted_by_cases:
    country = row['country']
    cases = row['Cases']
    
    # Calculate how many block characters to draw (max 25 blocks)
    bar_length = int((cases / max_cases) * 25)
    bar = '█' * bar_length
    
    # Print formatted output
    print(f"{country.ljust(10)} | {bar.ljust(25)} | {cases:,}")


# --- Analysis 2: Aggregating Data by Continent ---
print("\n\n2. TOTAL CASES BY CONTINENT")
print("-" * 50)

# Create a dictionary to hold our continent totals
continent_cases = defaultdict(int)

# Loop through and add cases to their respective continent buckets
for row in data:
    continent_cases[row['continent']] += row['Cases']

# Sort continents from highest cases to lowest
sorted_continents = sorted(continent_cases.items(), key=lambda x: x[1], reverse=True)

for continent, cases in sorted_continents:
    print(f"{continent.ljust(15)} : {cases:,} cases")

print("\n" + "=" * 50)
print("Analysis Complete.")
