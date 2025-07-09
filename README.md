 Project Overview

This project demonstrates a dynamic pricing system for urban parking using real-time data streams. It processes occupancy and contextual data (vehicle type, traffic, queue length, etc.) to inform intelligent pricing decisions. The demo is simplified to one parking spot, with the goal of scaling it up to multiple spaces using Pathway’s real-time stream processing.

Tech Stack

| Component           | Technology Used        |
|--------------------|------------------------|
| Programming Language | Python                |
| Data Streaming      | Pathway                |
| Data Manipulation   | Pandas, NumPy          |
| Visualization       | Bokeh, Panel           |
| Plotting            | Matplotlib             |
| Stream Schema       | Pathway Schema API     |
| Format              | Jupyter Notebook (.ipynb) |

 Architecture Diagram (Mermaid)
 `mermaid
flowchart TD
    A[CSV Data - parking_stream.csv] -->|Replay as Stream| B[Pathway replay_csv]
    B --> C[Data Schema Definition]
    C --> D[Timestamp Parsing]
    D --> E[Windowing & Aggregation]
    E --> F[Pricing Model]
    F --> G[Real-time Visualization]
    G --> H[Interactive Dashboard - Bokeh/Panel]
`



 Detailed Architecture & Workflow Explanation

1. Data Preprocessing:
    - Reads raw CSV data (`dataset-2.csv`) with features like:
      - `Occupancy`, `Capacity`, `QueueLength`, `TrafficConditionNearby`, `VehicleType`, etc.
    - Preprocesses categorical values using mappings (e.g., `'car' → 2`).
    - Combines `LastUpdatedDate` and `LastUpdatedTime` into a single `Timestamp`.

2. Streaming with Pathway:
    - Uses `Pathway.replay_csv()` to simulate a real-time stream (1000 rows/sec).
    - Defines a schema via `ParkingSchema` to ensure structure and type-checking.

3. Time Handling:
    - Parses timestamps using a specific format and extracts daily buckets (`day`) for windowing.

4. Aggregation & Windowing:
    - Employs daily tumbling windows for feature aggregation.
    - Can include instantaneous metrics (occupancy at end of day, etc.).

5. Pricing Logic:
    - price = 10 + ((pw.this.occ_max) / pw.this.cap) + (pw.this.queue_length_max)*(0.32) + pw.this.traffic_condition_max + ((1.25)*pw.this.is_special_day) + (0.8)*(pw.this.vehicle_type_max)-2
    this pricing model is used to ensure smooth and bounded pricing over a window of 1 hour
   
6. Visualization:
    - Uses Bokeh + Panel to build an interactive dashboard to display dynamic prices across time.
    - Ensures real-time update of visual elements based on stream state.
