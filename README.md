# AnyLogic-base-Animated-Simulation-Validator

# SAV Trip Plan Validator: AnyLogic Simulation

This repository contains a standalone AnyLogic agent-based simulation model (`ridesharing_v3`). 

**Note:** This project is part of a two-repository system. The Deep Reinforcement Learning (DRL) model that actually *generates* the shared trip plans lives in a separate repository. **This repository's sole purpose is validation.** It takes the execution plans produced by the DRL model, animates them on a map, and gathers objective statistics to ensure the generated plans are physically viable, unbiased, and mathematically sound.

---

## 📑 Table of Contents
- [Project Overview](#project-overview)
- [Key Simulation Assumptions](#key-simulation-assumptions)
- [Prerequisites & Lombok Setup](#prerequisites--lombok-setup)
- [Configuration (`rides_db.xlsx`)](#configuration-rides_dbxlsx)
- [Running the Simulation](#running-the-simulation)
- [Outputs and Exports](#outputs-and-exports)

---

## 🔍 Project Overview
By running the DRL-generated trip plans through this AnyLogic simulator, we can visually verify the routes and independently calculate key metrics. The simulator uses a real road network to measure true travel efficiency, ensuring the DRL agent's theoretical plans translate safely and effectively to the real world without hidden biases.

---

## 📝 Key Simulation Assumptions
The simulation engine and its subsequent statistical exports operate under the following strict assumptions:

* **A. Unique Passenger IDs:** A `Passenger_id` will never be repeated with another request.
* **B. Departure Timing:** `actual_departure_time` is the exact time the vehicle leaves the origin node of a specific ride action.
* **C. Arrival Timing:** `actual_arrival_time` is the time the vehicle arrives at the target location **plus** the designated `load-unload time`.
* **D. Action Coordinates:** All rides (Actions `0`, `1`, and `2`) consist of both an origin and a destination. Therefore, the export logs consistently track `origin_lat`, `origin_lon`, `dest_lat`, and `dest_lon`, alongside their respective actual departure and arrival times.
* **E. Time Spent in Vehicle:** `time_spent_in_vehicle` (in minutes) is calculated as `actual_arrival_time - actual_departure_time`. 
  * *Note:* Rides labeled as Action `1` (Drop-off) store the total time the passenger spent inside the vehicle. This captures the full duration from when the vehicle starts its journey at the passenger's initial location to the passenger's final destination, inclusive of the load-unload time.
* **F. Distance Metric:** `vehicle_dist_travelled` is always measured in **kilometers**.

---

## ⚙️ Prerequisites & Lombok Setup
This model utilizes custom Java classes instead of standard lists/tuples for highly efficient data handling. These classes require the **Lombok** library. **You must configure AnyLogic to use Lombok before running the model.**

### 1. Configure the AnyLogic Application
1. Download the `lombok.jar` file.
2. Navigate to the folder where AnyLogic is installed on your computer.
3. Place the `lombok.jar` file directly into this root program folder.
4. Locate the AnyLogic configuration file (e.g., `AnyLogic.ini`) in that same folder. 
5. Open the `.ini` file in a text editor (you may need Administrator privileges) and add the following line:
   ```text
   -javaagent:lombok.jar
