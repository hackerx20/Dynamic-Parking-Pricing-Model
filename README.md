# Intelligent Dynamic Parking Pricing System

In densely populated cities, parking is a dynamic and limited resource. Fluctuating demand, unpredictable traffic conditions, and inflexible static pricing models often result in poor utilization of parking spaces, long queues, and increased driver frustration. Traditional pricing approaches fail to adapt to real-time scenarios, leading to either underused or overcrowded lots.

This project introduces a Smart Dynamic Parking Pricing System—an intelligent, real-time solution that leverages demand forecasting, live traffic data, and competitive pricing strategies to optimize parking prices across multiple city locations. By integrating real-time data streaming with Pathway and interactive visualizations using Bokeh, the system dynamically adjusts prices based on occupancy levels, traffic congestion, queue lengths, and nearby competitor pricing. It also simulates user behavior and reroutes drivers when lots are full, fostering a more efficient, fair, and responsive urban parking environment.

[Google Colab Link](https://colab.research.google.com/drive/1yAkyLRopPcFwsEINEiMdpsxryHJIm5tW?authuser=1)

## Table of Contents

- [ Dataset Overview](#Dataset-Overview)
- [ Project Overview](#Project-Overview)
- [ Tech Stack](#Tech-Stack)
- [ Architecture Diagram](#️-architecture-diagram)
- [ Pricing Models](#-pricing-models)
- [ Real-Time Streaming](#-real-time-streaming)
- [ Feature Engineering](#-feature-engineering)
- [ Future Improvements](#-future-improvements)

## Dataset Overview

The dataset simulates parking sensor data and includes the following features:

* Lot ID: Unique identifier for each parking location.
* Capacity: Maximum number of vehicles that can be parked.
* Occupancy: Real-time count of currently parked vehicles.
* Queue Length: Vehicles waiting for a spot.
* Vehicle Type: Categorical feature (car, bike, van, etc.)
* Traffic Conditions Nearby: Encoded value representing congestion near the lot.
* Special Day Flag: Boolean for holidays/weekends.
* Timestamp: Combined date and time for streaming simulation.
* Latitude & Longitude: Coordinates used for geospatial distance calculations (Model 3).

This dataset serves as input for real-time dynamic pricing models and rerouting logic.

## Project Overview

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


## Tech Stack

| Category            | Tools/Technologies                                  |
|---------------------|-----------------------------------------------------|
| Programming         | Python 3.10+                                        |
| Streaming Framework | [Pathway](https://pathway.com/)                     |
| Visualization       | Bokeh, Panel                                        |
| Data Processing     | Pandas, NumPy, datetime                             |
| Dashboard           | Bokeh integration                                   |
| ML Logic            | Linear & demand-driven pricing models               |



### Feature Engineering

* Occupancy normalization (`Occupancy / Capacity`)
* Temporal features (`hour`, `is_weekend`)
* Demand features: `QueueLength`, `TrafficConditionNearby`, `VehicleType`, `IsSpecialDay`
* Rolling occupancy window: `occupancy_rolling_3h`
* Distance to closest lot
* Competitive metrics: `AvgCompPrice`, `NumCompetitors`

##  Pricing Models

The project implements **four progressively intelligent pricing models**, each increasing in sophistication and realism to simulate a smart parking system.
---
###  Model 1: Linear Pricing Based on Occupancy
**Principle:**  
Price is adjusted linearly based on how full the lot is.
**Mechanism:**
- As the occupancy rate increases, so does the price.
- Designed to discourage usage of nearly full lots.
**Formula:**
price = base_price + α × (occupancy / capacity)
**Strengths:**
- Simple and interpretable.
- Useful as a **baseline model** for comparison.
**Limitations:**
- Ignores contextual factors like traffic congestion or vehicle type.
- Doesn't differentiate between weekdays, weekends, or special days.

###  Model 2: Multi-Factor Demand-Based Pricing

**Principle:**  
Prices reflect a **composite demand score** calculated from multiple **real-world indicators**.

**Mechanism:**
- Demand score is a **weighted sum** of key demand-driving variables.
- Each factor reflects its influence on real-world parking demand.

**Demand Score Components:**
-  **Occupancy Rate** – More filled lots signal higher demand.
-  **Queue Length (normalized)** – Longer waiting queues indicate urgency.
-  **Traffic Conditions** – Higher congestion increases desirability of nearby parking.
-  **Special Day Flag** – Events or holidays spike demand.
-  **Vehicle Type Weight** – Larger vehicles incur higher usage costs.

**Formula:**


price = base_price × (1 + λ × normalized_demand_score)

**Strengths:**
- Adapts to both spatial and temporal context.
- More **realistic and robust** than Model 1.
- Reflects **urban behavior patterns** more accurately.

**Limitations:**
- Still ignores competition from nearby lots.
- Doesn’t optimize network-wide parking distribution.

---

###  Model 3: Competitive Pricing with Geo-Spatial Context

**Principle:**  
Lots adjust their pricing based on nearby **competitor prices and utilization levels**.

**Mechanism:**
- If nearby lots (within 0.5–1 km) offer cheaper rates and better availability, the current lot lowers its price to stay competitive.
- When a lot is **over 90% full**, rerouting is triggered to divert users to less crowded lots.

**Key Features:**
- **Geo-aware**: Uses Haversine distance to evaluate spatial proximity.
- **Rerouting Flag**: Binary marker that indicates redirection was needed.
- **Dynamic Undercutting**: Adjusts price down to prevent user loss.

**Strengths:**
- Encourages balanced distribution across all lots.
- Simulates **real-world competition** in dense urban parking ecosystems.

**Limitations:**
- Does not account for user preferences or walking distance.
- Rerouting logic assumes real-time availability updates.

### 📊 Model Reference Table

| Model Name        | Description                                        |
| ----------------- | -------------------------------------------------- |
| **Price_Model1**  | Basic linear model without daily reset             |
| **Price_Model2**  | Linear model with multi-factor demand awareness    |
| **Price_Model3**  | Pure demand-based pricing with weighted inputs     |
| **Price_Model4**  | Competitive pricing (geo-aware + rerouting logic)  |

## Real-Time Streaming

The `Pathway` library powers the real-time computation pipeline. The final enriched dataframe is streamed and visualized using:

* `pw.Schema`: defines expected fields (e.g., price models, location)
* `DFSubject`: converts static data into a stream
* `windowby()`: daily aggregation of metrics
* `plot()`: Bokeh-based live plot per parking lot

**Streaming Rate**: 1000 rows/sec
**Total Records Processed**: \~18,000+

---

## Feature Engineering

Your function performs:

* Contextual encoding for `VehicleType`
* Time-aware features: `hour`, `is_weekend`
* Distance-based demand score (`closest_lot_distance`)
* Rolling occupancy demand proxy
* Normalization of queue and traffic
* Filtering invalid records & normalizing where necessary


Navigate to:

* **Introduction Tab**: View explanation, architecture
* **Streaming Panel**: Select pricing model & parking lot → view real-time trend
* **Map Heatmap**: Interactive heatmap of all locations

---

## Future Improvements

* Add anomaly detection to reroute logic
* Include pricing elasticity models using RL or XGBoost
* Integrate live occupancy feeds (IoT sensors or APIs)
* Export live alerts (e.g., high demand, surge pricing)
* Integrate live traffic APIs (e.g., Google Maps) for real-time congestion updates.
* Build a driver-facing mobile app that displays optimal nearby parking with live prices.

---

## Additional Notes

* **Data Source**: UrbanSmart Parking Dataset (CSV)
* **Input Rate**: Simulated using Pathway
* **Visualization**: Real-time Bokeh chart per parking lot
* **Limitations**: Performance bottleneck for larger windowed joins in competitive model

---

## Optional Attachments

* [`Report.pdf`](./report.pdf) – optional academic/portfolio report
* [`processed_data.csv`](./processed_data.csv) – generated features & prices

---

## Author

**Shivansh Tiwari**
3rd Year | B.Tech, IIT Roorkee

