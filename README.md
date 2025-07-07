Here's a professional and detailed `README.md` file for your **"Intelligent Dynamic Parking Pricing System"** project, incorporating your code, logic, streaming pipeline, and models. This is structured to be **portfolio-ready** and demonstrates project quality and depth.

---

````markdown
# 🚗 Intelligent Dynamic Parking Pricing System

An intelligent real-time parking price optimization system that leverages demand forecasting, traffic conditions, and competitive pricing strategies to dynamically compute and adjust parking prices across multiple city locations. This solution also incorporates **real-time streaming with Pathway**, **interactive visualizations with Bokeh**, and a **Streamlit-based UI** for monitoring.

---

## 📌 Table of Contents

- [📌 Project Overview](#-project-overview)
- [🚀 Tech Stack](#-tech-stack)
- [🏗️ Architecture Diagram](#️-architecture-diagram)
- [🔁 Workflow & System Architecture](#-workflow--system-architecture)
- [📊 Pricing Models](#-pricing-models)
- [📡 Real-Time Streaming](#-real-time-streaming)
- [🧠 Feature Engineering](#-feature-engineering)
- [📎 Usage Guide](#-usage-guide)
- [📄 Future Improvements](#-future-improvements)
- [📚 Additional Notes](#-additional-notes)

---

## 📌 Project Overview

This system analyzes parking occupancy data across 14 major locations to compute **dynamic pricing in real-time**, using:

- Rolling occupancy & demand intensity
- Traffic & queue conditions
- Special events
- Competitor pricing strategies

It features:

✅ Predictive pricing  
✅ Competitive benchmarking  
✅ Real-time streaming & visualizations  
✅ Streamlit dashboard for exploration

---

## 🚀 Tech Stack

| Category            | Tools/Technologies                                  |
|---------------------|-----------------------------------------------------|
| Programming         | Python 3.10+                                        |
| Streaming Framework | [Pathway](https://pathway.com/)                     |
| Visualization       | Bokeh, Panel                                        |
| Data Processing     | Pandas, NumPy, datetime                             |
| Dashboard           | Bokeh integration                                   |
| ML Logic            | Linear & demand-driven pricing models               |

---


### 🔧 Feature Engineering

* Occupancy normalization (`Occupancy / Capacity`)
* Temporal features (`hour`, `is_weekend`)
* Demand features: `QueueLength`, `TrafficConditionNearby`, `VehicleType`, `IsSpecialDay`
* Rolling occupancy window: `occupancy_rolling_3h`
* Distance to closest lot
* Competitive metrics: `AvgCompPrice`, `NumCompetitors`


## 📊 Pricing Models

| Model Name        | Description                                        |
| ----------------- | -------------------------------------------------- |
| **Price\_Model1** | Basic linear model without daily reset             |
| **Price\_Model2** | Linear model with reset every day                  |
| **Price\_Model3** | Demand-based pricing (queue, traffic, special day) |
| **Price\_Model4** | Competitive pricing (adjust based on nearby lots)  |

✅ Each model clips pricing to `[0.5x, 2x]` of base price.

---

## 📡 Real-Time Streaming

The `Pathway` library powers the real-time computation pipeline. The final enriched dataframe is streamed and visualized using:

* `pw.Schema`: defines expected fields (e.g., price models, location)
* `DFSubject`: converts static data into a stream
* `windowby()`: daily aggregation of metrics
* `plot()`: Bokeh-based live plot per parking lot

**Streaming Rate**: 1000 rows/sec
**Total Records Processed**: \~18,000+

---

## 🧠 Feature Engineering

Your function performs:

* Contextual encoding for `VehicleType`
* Time-aware features: `hour`, `is_weekend`
* Distance-based demand score (`closest_lot_distance`)
* Rolling occupancy demand proxy
* Normalization of queue and traffic
* Filtering invalid records & normalizing where necessary

---

## 📎 Usage Guide

```bash
# 1. Install dependencies
pip install -r requirements.txt

# 2. Launch the Streamlit dashboard
streamlit run app.py
```

Navigate to:

* 📘 **Introduction Tab**: View explanation, architecture
* 📈 **Streaming Panel**: Select pricing model & parking lot → view real-time trend
* 🌍 **Map Heatmap**: Interactive heatmap of all locations

---

## 📄 Future Improvements

✅ Add anomaly detection to reroute logic
✅ Include pricing elasticity models using RL or XGBoost
✅ Integrate live occupancy feeds (IoT sensors or APIs)
✅ Export live alerts (e.g., high demand, surge pricing)

---

## 📚 Additional Notes

* **Data Source**: UrbanSmart Parking Dataset (CSV)
* **Input Rate**: Simulated using Pathway
* **Visualization**: Real-time Bokeh chart per parking lot
* **Limitations**: Performance bottleneck for larger windowed joins in competitive model

---

## 📎 Optional Attachments

* 📄 [`Report.pdf`](./report.pdf) – optional academic/portfolio report
* 📂 [`processed_data.csv`](./processed_data.csv) – generated features & prices

---

## 💡 Author

**Shivansh Tiwari**
3rd Year | B.Tech, IIT Roorkee
🔗 GitHub: [@shivansh-iitr](https://github.com/...)
📧 Email: [shivansh@email.com](mailto:shivansh@email.com)
📘 Resume: \[LinkedIn or PDF Link]

```

---

Would you like this to be exported as a `.md` file or need the **architecture diagram image** rendered as well?
```
