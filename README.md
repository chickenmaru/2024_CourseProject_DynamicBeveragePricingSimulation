# Overview

This project simulates a dynamic pricing model for beverages sold at a baseball stadium on game days. The simulation is broken down into three main tasks: modeling demand, implementing different pricing strategies (static vs. dynamic), and analyzing the results, including the impact of price interactions between different beverage types.

The core of the simulation revolves around three types of game days:
- **Home Games (中信主場):** Higher audience numbers, averaging 27,705.
- **Away Games (中信客場):** Moderate audience numbers, averaging 17,636.
- **No Games (無中信):** A baseline audience, averaging 19,945.

Demand for three beverage categories—**non-alcoholic drinks**, **beer**, and **cocktails**—is modeled using different functions (exponential, logarithmic, and linear) to reflect varying price sensitivities.

# Task 1: Demand Simulation and Supply Analysis

This task focuses on modeling and analyzing the daily demand for beer to establish a reasonable supply limit.

1.  **Audience and Beer Demand Simulation**:
    * The simulation begins by randomly selecting a game type based on predefined probabilities (`[0.25, 0.5, 0.25]`).
    * The total number of visitors for the day is simulated using a normal distribution based on the chosen game type's average and standard deviation.
    * A purchase probability of 10% is applied to the total visitors to determine the number of buyers using a Poisson distribution.
    * The demand of each buyer is then simulated with a normal distribution, and the total daily demand for beer is calculated in milliliters and cups.

2.  **Multiple Simulations and Supply Recommendation**:
    * The entire process is run **1000 times** to create a distribution of potential daily beer demand.
    * Key metrics like the **mean** and **standard deviation** of the demand are calculated.
    * The **95th percentile** of the demand distribution is recommended as the daily supply limit. This ensures that the stadium can meet demand on 95% of its operating days. The recommended limit is visualized in a histogram.

## Task 2: Intra-Day Demand and Audience Flow

This task refines the demand model by simulating how audience numbers and demand fluctuate throughout a single day.

1.  **Match Schedule Generation**:
    * A function, `generate_match_schedule`, is created to simulate game schedules for multiple days.
    * It ensures that the same type of match does not occur more than four consecutive times, adding a layer of realism to the schedule.

2.  **Audience Distribution**:
    * The `generate_audience` function distributes the total simulated audience across 26 half-hour time slots in a day.
    * A **peak period** (time slots 3 to 10) is designated, attracting 80% of the audience, while the **non-peak periods** (before and after) receive the remaining 20%. This reflects the typical flow of people at an event.

# Task 3: Beverage Demand Models and Profit Calculation

This task sets up the fundamental economic models for different beverage categories.

1.  **Beverage Categories**:
    * Three main categories are defined: **Non-alcoholic drinks**, **beer**, and **cocktails**.
    * Each category is assigned a specific number of unique drink types and a corresponding demand model.
    * The project uses logarithmic (`log`), exponential (`exp`), and linear (`linear`) demand functions, each with unique parameters to simulate different consumer behaviors.

2.  **Pricing and Cost**:
    * Each demand model has a defined **cost per unit** and an **initial price**.
    * A function `calculate_profit` determines the total profit based on `(price - cost) * demand`.
    * Additional pricing constraints are introduced, such as price limits (1.2x and 0.3x of initial price) and price adjustment rates (1% increase upon reaching a sales target, 2% decrease with zero demand).

# Task 4: Static Pricing Strategy

This task simulates a baseline scenario where prices remain constant throughout the day.

* **Supply Limits**: Daily supply caps are set for each major beverage category based on the results from Task 1.
* **Simulation**: The `simulate_static_pricing` function runs the simulation for a specified number of days, calculating demand, sales, and profit for each time slot.
* **Results**: The total profit from this static pricing model is calculated and printed, serving as a benchmark for comparison with the dynamic strategy.

# Task 5: Dynamic Pricing Strategy

This task introduces a dynamic pricing model where prices adjust in response to real-time sales and demand.

* **Price Adjustment**:
    * The `simulate_dynamic_pricing` function implements a rule: if a drink's cumulative sales reach 1000ml, its price increases by 1% in the next time slot.
    * If a drink has zero demand in a time slot, its price decreases by 2%.
    * Prices are capped by upper and lower limits to prevent extreme fluctuations.

* **Results**: The simulation calculates the total profit under this dynamic strategy and visualizes the price changes over time in a plot, showcasing how the algorithm adapts to demand.

# Task 6: Dynamic Pricing with Interactions

This final task enhances the dynamic model by incorporating the **interplay between different beverage prices**.

* **Interaction Coefficients**: An interaction matrix is introduced, defining how a price change in one beverage category affects the demand for another.
    * Positive coefficients indicate complementary goods (e.g., higher beer prices might boost non-alcoholic drink demand).
    * Negative coefficients indicate competitive goods (e.g., higher beer prices might decrease cocktail demand).
* **Optimization**: The simulation explores a range of potential discount upper limits to find the one that yields the highest total profit. It simulates with various discount levels and plots the relationship between the discount limit and total profit.
* **Conclusion**: The script outputs the optimal discount limit and the corresponding profit, providing a more refined and realistic pricing strategy for the stadium. It also visualizes the price and sales trends under this optimized model.


    * **負值**表示競爭品（例如，啤酒價格上漲可能會降低調酒的需求）。
* **優化**：模擬會探索一系列可能的折扣上限，以找出能帶來最高總利潤的最佳值。專案會模擬不同的折扣水準，並繪製折扣上限與總利潤之間的關係圖。
* **結論**：腳本將輸出最佳的折扣上限和相應的利潤，為球場提供一個更精確且貼近現實的定價策略。同時，也會視覺化此優化模型下的價格和銷售趨勢。
