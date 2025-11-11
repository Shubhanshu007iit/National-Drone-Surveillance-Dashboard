# National-Drone-Surveillance-Dashboard
The National Drone Surveillance Dashboard is an interactive Power BI project that tracks drone operations across regions. It shows flight paths, mission types, and airbase coverage, helping identify safe and unsafe zones through dynamic maps and charts for better national surveillance analysis.


https://github.com/Shubhanshu007iit/National-Drone-Surveillance-Dashboard/blob/main/Screenshot%202025-11-11%20174427.png


# Data Processing with Python

Before visualization, the dataset was cleaned and transformed using Python. The script detects potential drone intrusions based on flight speed and altitude.
https://github.com/Shubhanshu007iit/National-Drone-Surveillance-Dashboard/blob/main/Screenshot%202025-11-11%20225751.png

# config: edit thresholds if needed
SPEED_THRESHOLD = 200        # km/h
ALTITUDE_THRESHOLD = 50      # meters
UNKNOWN_WEIGHT = 1           # only 'Unknown' considered for intrusion rule


# load
df = pd.read_csv("/content/india_drone_alerts.csv")

# ensure columns exist
expected_cols = ["Drone_ID","Airbase","Altitude (m)","Speed (km/h)","Status","Time"]
for c in expected_cols:
    if c not in df.columns:
        raise SystemExit(f"Missing column: {c}")

# Convert Time to datetime for Power BI friendly format
df['Time'] = pd.to_datetime(df['Time'], format="%H:%M:%S").dt.time
df['Timestamp'] = pd.to_datetime(df['Time'].astype(str), format="%H:%M:%S")

# Intrusion rule function
def check_intrusion(row):
    if (row['Status'].strip().lower() == "unknown") and (
        (row['Speed (km/h)'] > SPEED_THRESHOLD) or (row['Altitude (m)'] < ALTITUDE_THRESHOLD)
    ):
        return "Intrusion Alert"
    else:
        return "Safe"

df['Alert_Status'] = df.apply(check_intrusion, axis=1)
df['Alert_Flag'] = df['Alert_Status'].apply(lambda x: 1 if x == "Intrusion Alert" else 0)

# Save output
out_csv = "india_drone_alerts.csv"
df.to_csv(out_csv, index=False)
print(f"Saved: {out_csv}")
print("Summary:")
print(df['Alert_Status'].value_counts())


# Purpose of the Script:

Cleans and validates raw drone data.

Detects intrusions using speed (>200 km/h) or low altitude (<50 m).

Adds Alert_Status and Alert_Flag for Power BI DAX measures.

Generates a clean, Power BI–ready dataset.

# Dashboard Highlights

Total Drones: 121

Safe Drones: 58

Intrusion Count: 63

Safe Percentage: 47.93%

Top Intrusion Bases: Tezpur, Ambala, Jaisalmer

Visuals include bar charts, pie charts, maps, line charts, and KPI cards.

# 🧰 Tools & Technologies

Python: Data cleaning and preprocessing (Pandas, NumPy)

Microsoft Power BI: Dashboard design and DAX calculations

Power Query Editor: Data transformation and modeling

Dataset: Drone operation and surveillance data

# 🚀 Steps to Reproduce

Run the Python preprocessing script.

Save the cleaned file as india_drone_alerts.csv.

Open Power BI Desktop and import the dataset.

Use Power Query Editor for additional transformations.

Apply DAX functions to calculate safety metrics.

Design visuals like KPI cards, maps, and charts.

# 📈 Insights

Detects unsafe airbases based on high intrusion counts.

Compares drone activity levels and mission types.

Highlights altitude and speed trends by time.

Displays real-time safety ratios using gauge visuals.

# 🧠 Future Enhancements

Integration of real-time drone tracking via APIs.

Predictive modeling using historical intrusion data.

GIS-based visual mapping for detailed area surveillance.


# Shubhanshu Kumar
Data Analyst | Power BI Developer
📍 IIT Patna 



