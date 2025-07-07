# SUMMER_ANALYTICS-PROJECT

### CAPSTONE_PROJECT-SUMMER ANALYTICS ###

This project aims to build a dynamic pricing model using real-time data streams, basic economic theory,
and ML models built from scratch, using only numpy, pandas libraries for parking space such that:
• The price is realistically updated in real-time based on:
– Historical occupancy patterns
– Queue length
– Nearby traffic
– Special events
– Vehicle type
– Competitor parking prices

## TECHSTACKS USED ##

1) PYTHON
2) PANDAS
3) PATHWAY
4) BOKEH
5) NUMPY
6) MATPLOTLIB
7) DATETIME

   #DETAILED OVERVIEW#

   CAPSTONE PROJECT - ARCHITECTURE AND WORKFLOW

=============================================================
1. Overview
=============================================================
This project implements a dynamic pricing system for urban parking lots. It uses a combination of:
- Batch (pandas) prototype models
- Streaming (Pathway) real-time pipeline
- Interactive dashboards (Panel/Bokeh)

The goal is to price parking in real-time based on:
- Occupancy patterns
- Queue length
- Traffic congestion
- Special events
- Vehicle type

=============================================================
2. Data Ingestion
=============================================================
Data comes from 14 parking lots over 73 days, with 18 time points/day.
Main fields:
- timestamp
- lot_id
- occupancy
- capacity
- queue_length
- traffic_condition_nearby
- special_day
- vehicle_type

Streaming connectors:
- CSV folder watcher (Pathway's built-in)


=============================================================
3. Schema Definition
=============================================================
A typical schema in Pathway:
Event(
    lot_id: string,
    timestamp: datetime,
    occupancy: float,
    capacity: float,
    queue_length: float,
    traffic_condition_nearby: string,
    special_day: int,
    vehicle_type: string
)

=============================================================
4. Feature Engineering
=============================================================
Derived columns:
- occupancy_ratio = occupancy / capacity

All done via Pathway's assign() or pandas' vectorized operations.

=============================================================
5. Pricing Model
=============================================================

-------------------------------------------------------------
5.1 Baseline Linear Model
-------------------------------------------------------------
Implements:

    Price_{t+1} = Price_t + α * (Occupancy / Capacity)

- Starts from a base price of $10
- Updated per-lot in order of timestamp
- Stored in Pathway via group_by().fold() maintaining last_price as state

-------------------------------------------------------------
5.3 Smoothing
-------------------------------------------------------------
Price variations are smoothed:
- Rolling window average (pandas: rolling())
- Pathway: windowed() reducer

-------------------------------------------------------------
5.4 Clipping
-------------------------------------------------------------
Prices are clipped to reasonable bounds (e.g. $5 to $50) to avoid erratic spikes.

=============================================================
6. Stateful Computation
=============================================================
- Pathway's group_by().fold() allows stateful per-lot price updates.
- No need for external ML libraries.
- Streaming-friendly: can handle real-time updates as events arrive.

=============================================================
7. Windowing and Aggregates
=============================================================
Daily/tumbling windows aggregate:
- Max/min occupancy
- Average queue length
- Average traffic level

Used to inform pricing decisions.

=============================================================
8. pandas Integration
=============================================================
- Used in notebooks for batch prototyping
- Dataframes allow easy groupby-apply modeling before migrating to streaming
- Rolling and clipping are prototyped in pandas

=============================================================
9. Visualization and Dashboard
=============================================================
- Bokeh plots of lot-wise pricing over time.
- Panel-based dashboard with:
    - Sliders for α, β, γ, etc.
    - Live plots
    - Data tables

Deployable as a Flask/Tornado app.

=============================================================
10. Export and Deployment
=============================================================
- Output pricing streams written to CSV/JSONL or pushed to Kafka topics.
- Pathway runs continuously as a service.
- Dockerized for deployment.

=============================================================
11. Future Extensions
=============================================================
- Geo-competitive pricing: use lat/long for competitor price adjustments.
- Predictive occupancy: time-series forecasting feeds into pricing.
- RL-based coefficient tuning: adaptive pricing strategy learning.

=============================================================
12. Project Structure (example)
=============================================================
- BaselineLinearModel.ipynb -> batch prototype
- DemandBasedPricing.ipynb -> extended scoring model
- streaming_pipeline.py -> Pathway implementation
- dashboard_app.py -> Panel/Bokeh dashboard
- README.md -> project guide
- requirements.txt -> dependencies
- architecture_diagram.png -> pipeline diagram

=============================================================

#LINK OF THE CODE FILE: https://github.com/MonyaB117/SUMMER_ANALYTICS-PROJECT/blob/bfd92dd13f85cec56b65bdc194208e3b046ff4c1/CAPSTONE_PROJECT.ipynb

#LINK OF OUTPUT GRAPH:
