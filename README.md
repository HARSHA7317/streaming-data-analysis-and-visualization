# 📊 Streaming Data Analysis and Visualization

A Data Analysis Essentials project that analyzes **household energy consumption data** by simulating a continuous data stream and extracting useful insights such as consumption trends, peak usage, hourly patterns, and potential anomalies.

## 👨‍💻 Project Team

| Name              | Roll Number |
| ----------------- | ----------- |
| N. Harsha Vardhan | 25B11CS243  |
| P. Chandrakanth   | 25B11CS715  |
| M.N. Anvesh       | 25B11CS057  |
| C.K. Kamal Basha  | 25B11CS148  |

**Institution:** Aditya University, Surampalem
**Department:** Computer Science and Engineering
**Course:** Data Analysis Essentials (2501AI06)
**Academic Year:** 2026–2027

---

## 📌 Project Overview

Streaming Data Analysis and Visualization focuses on analyzing continuously generated data to identify patterns, trends, peak usage periods, and unusual activities.

In this project, **household energy consumption data** is used as a simulated streaming dataset. Python is used to clean and process the data, perform statistical analysis, simulate streaming data, detect anomalies, identify peak consumption, and generate visualizations.

The project uses:

* 🐍 Python
* 🐼 Pandas
* 🔢 NumPy
* 📈 Matplotlib
* ☁️ Google Colab

The project documentation describes the system as a **simulation of streaming processing using a historical CSV dataset**, rather than a live IoT or real-time streaming infrastructure.

---

## 🎯 Objectives

The main objectives of this project are:

* Load and organize household energy consumption data.
* Combine `Date` and `Time` into a `DateTime` column.
* Clean missing and duplicate records.
* Convert appliance consumption values into numeric format.
* Calculate mean, median, minimum, maximum, and standard deviation.
* Simulate streaming data using the first 100 appliance readings.
* Calculate running and moving averages.
* Detect potential anomalies using a statistical threshold.
* Identify peak energy consumption.
* Analyze average consumption by hour.
* Visualize energy consumption using different charts.

---

## 📂 Dataset

The project uses a household energy consumption dataset stored as:

```text
household_energy_consumption.csv
```

The dataset contains household appliance energy readings along with date-time, temperature, and humidity information.

The main columns used in the analysis are:

| Column           | Description                               |
| ---------------- | ----------------------------------------- |
| `Date`           | Date of the energy reading                |
| `Time`           | Time of the energy reading                |
| `Appliances`     | Main energy consumption variable          |
| `Temperature`    | Environmental temperature                 |
| `Humidity`       | Environmental humidity                    |
| `DateTime`       | Derived date-time column                  |
| `Hour`           | Derived hour from DateTime                |
| `Moving_Average` | 10-record rolling average                 |
| `Anomaly`        | Boolean indicator for potential anomalies |

The dataset and derived fields are documented in the project report.

---

## 🔄 Project Workflow

```text
Household Energy Dataset
          ↓
     Load CSV Data
          ↓
     Data Cleaning
          ↓
Combine Date + Time
          ↓
   Data Validation
          ↓
 Statistical Analysis
          ↓
 Streaming Simulation
          ↓
   Moving Average
          ↓
 Anomaly Detection
          ↓
 Peak Consumption
          ↓
   Hourly Analysis
          ↓
 Data Visualization
          ↓
       Insights
```

---

## 🧹 Data Preprocessing

The following preprocessing operations are performed:

### 1. Load Dataset

```python
df = pd.read_csv("/content/household_energy_consumption.csv")
```

### 2. Create DateTime

```python
df["DateTime"] = pd.to_datetime(
    df["Date"] + " " + df["Time"],
    errors="coerce"
)
```

### 3. Convert Appliances to Numeric

```python
df["Appliances"] = pd.to_numeric(
    df["Appliances"],
    errors="coerce"
)
```

### 4. Remove Missing Values

```python
df = df.dropna(
    subset=["DateTime", "Appliances"]
)
```

### 5. Remove Duplicate Records

```python
df = df.drop_duplicates()
```

### 6. Sort Chronologically

```python
df = df.sort_values("DateTime")
```

### 7. Create Hour Column

```python
df["Hour"] = df["DateTime"].dt.hour
```

These preprocessing steps prepare the dataset for statistical and time-based analysis.

---

## 📊 Statistical Analysis

The project calculates:

* Mean
* Median
* Minimum
* Maximum
* Standard Deviation

Example:

```python
mean_value = df["Appliances"].mean()
median_value = df["Appliances"].median()
minimum = df["Appliances"].min()
maximum = df["Appliances"].max()
std_value = df["Appliances"].std()
```

### Results

| Measure            |   Value |
| ------------------ | ------: |
| Mean               | 94.2086 |
| Median             |   97.92 |
| Minimum            |   29.65 |
| Maximum            |   350.0 |
| Standard Deviation | 31.2072 |

---

## 🌊 Streaming Simulation

Since the project uses a historical CSV dataset, streaming is simulated by processing the first **100 appliance readings** sequentially.

```python
stream_data = []

for value in df["Appliances"].head(100):
    stream_data.append(value)

running_average = np.mean(stream_data)
```

### Streaming Result

```text
Records processed: 100
Running Average: 89.7992
```

This demonstrates the basic concept of processing incoming readings continuously.

---

## 📈 Moving Average

A 10-record rolling window is used to identify consumption trends.

```python
df["Moving_Average"] = (
    df["Appliances"].rolling(window=10).mean()
)
```

The moving average provides a smoother representation of the energy consumption data.

---

## 🚨 Anomaly Detection

Potential anomalies are detected using the following statistical rule:

```text
Upper Limit = Mean + 2 × Standard Deviation

Lower Limit = Mean - 2 × Standard Deviation
```

Python implementation:

```python
mean = df["Appliances"].mean()
std = df["Appliances"].std()

upper_limit = mean + (2 * std)
lower_limit = mean - (2 * std)

df["Anomaly"] = (
    (df["Appliances"] > upper_limit) |
    (df["Appliances"] < lower_limit)
)

anomalies = df[df["Anomaly"]]
```

The implementation reported **4 potential anomalies**.

---

## ⚡ Peak Consumption

The maximum appliance energy consumption is identified using:

```python
peak_value = df["Appliances"].max()

peak_record = df[
    df["Appliances"] == peak_value
]
```

### Peak Result

```text
Peak Consumption: 350.0
Date: 2026-01-12
Time: 11:00:00
```

---

## 🕐 Hourly Analysis

Average appliance consumption is calculated for every hour of the day.

```python
hourly_consumption = (
    df.groupby("Hour")["Appliances"].mean()
)
```

This helps identify periods with relatively higher or lower household energy consumption.

---

## 📊 Data Visualization

The project generates several visualizations:

### 1. Daily Average Energy Consumption

A bar chart compares average appliance consumption across different days.

### 2. Average Energy Consumption by Hour

A line chart shows how average consumption changes across the 24 hours of the day.

### 3. Potential Anomaly Visualization

A line chart highlights energy readings that cross the selected anomaly threshold.

### 4. Histogram

The histogram shows the frequency distribution of appliance energy consumption.

### 5. Box Plot

The box plot displays the median, spread, quartiles, and potential outliers.

The project documentation includes all five visualization types.

---

## 🛠️ Technologies Used

| Technology   | Purpose                                |
| ------------ | -------------------------------------- |
| Python       | Programming language                   |
| Pandas       | Data loading, cleaning and analysis    |
| NumPy        | Numerical and statistical calculations |
| Matplotlib   | Data visualization                     |
| CSV          | Dataset format                         |
| Google Colab | Development environment                |

---

## 📌 Key Results

The implementation produced the following important results:

| Analysis            | Result |
| ------------------- | -----: |
| Mean Consumption    |  94.21 |
| Median Consumption  |  97.92 |
| Minimum Consumption |  29.65 |
| Maximum Consumption |  350.0 |
| Standard Deviation  |  31.21 |
| Streaming Records   |    100 |
| Running Average     |  89.80 |
| Potential Anomalies |      4 |
| Peak Consumption    |  350.0 |

The peak consumption was recorded at:

```text
2026-01-12 11:00:00
```

---

## ▶️ How to Run the Project

### Option 1 — Google Colab

1. Open the Google Colab notebook.
2. Upload `household_energy_consumption.csv`.
3. Run the cells from top to bottom.
4. The notebook will:

   * Load the dataset
   * Clean the data
   * Perform statistical analysis
   * Simulate streaming
   * Detect anomalies
   * Find peak consumption
   * Generate visualizations

### Option 2 — Local Python Environment

Install the required libraries:

```bash
pip install pandas numpy matplotlib
```

Then place:

```text
household_energy_consumption.csv
```

in the same project directory and run the Python notebook/script.

---

## 📁 Recommended GitHub Repository Structure

```text
Streaming-Data-Analysis-and-Visualization/
│
├── README.md
├── Streaming_Data_Analysis.ipynb
├── household_energy_consumption.csv
└── outputs/
    ├── daily_average.png
    ├── hourly_consumption.png
    ├── anomaly_detection.png
    ├── histogram.png
    └── box_plot.png
```

> If your dataset is not included in the repository, keep it out of GitHub and explain where users can obtain it.

---

## ⚠️ Limitations

* The project uses a historical CSV dataset instead of a live event stream.
* Streaming is simulated rather than implemented using real-time infrastructure.
* Technologies such as Kafka, Spark Streaming, and Flink are not implemented.
* Anomaly detection uses a simple `mean ± 2 × standard deviation` rule.
* No machine-learning prediction model is implemented.
* No real-time interactive dashboard is included.

These limitations are explicitly described in the project documentation.

---

## 🚀 Future Enhancements

Possible future improvements include:

* Connect the system to a live IoT data source.
* Use APIs for real-time energy readings.
* Implement Apache Kafka for real-time streaming.
* Explore Apache Spark Streaming or Apache Flink.
* Apply machine-learning models for anomaly detection.
* Build an interactive real-time dashboard.
* Add energy-consumption forecasting.
* Improve scalability, latency, fault tolerance, and security.

---

## 👥 Team

**N. Harsha Vardhan** — 25B11CS243
**P. Chandrakanth** — 25B11CS715
**M.N. Anvesh** — 25B11CS057
**C.K. Kamal Basha** — 25B11CS148

---

## 🎓 Academic Project

This project was developed as part of the **Data Analysis Essentials (DAE)** Cornerstone Project for B.Tech Computer Science and Engineering at **Aditya University, Surampalem**.

---

## 📚 References

* Python for Data Analysis — Wes McKinney
* Python Data Analytics — Fabio Nelli
* Pandas Documentation
* NumPy Documentation
* Matplotlib Documentation
* Kaggle

---

## ⭐ Conclusion

The **Streaming Data Analysis and Visualization** project demonstrates how household energy consumption data can be cleaned, processed, analyzed, and visualized using Python.

The project combines Pandas, NumPy, and Matplotlib to perform data preprocessing, statistical analysis, streaming simulation, moving-average analysis, anomaly detection, peak identification, hourly analysis, and visualization.

The results provide useful insights into household energy-consumption patterns and demonstrate the basic concepts of streaming-oriented data analysis.

---

### 🔗 Google Colab Notebook

[Open the Google Colab Notebook](https://colab.research.google.com/drive/1bdmuFm1p1QiEEFKlebsdaB-fXW8M09Uj)

---

**Made with Python 🐍 | Pandas 🐼 | NumPy 🔢 | Matplotlib 📊**
