# Project- Choose Your own Hypothesis: Crash Reporting-Drivers Data in Montgomery County
- This dataset provides information on motor vehicle operators (drivers) involved in traffic collisions occurring on county and local roadways. The dataset reports details of all traffic collisions occurring on county and local roadways within Montgomery County, as collected via the Automated Crash Reporting System (ACRS) of the Maryland State Police, and reported by the Montgomery County Police, Gaithersburg Police, Rockville Police, or the Maryland-National Capital Park Police. This dataset shows each collision data recorded and the drivers involved.

- You can get get the dataset using this link: https://catalog.data.gov/dataset/crash-reporting-drivers-data

# Impact of Distracted Driving on Fault Determination

## 1. Research Question

Does distracted driving significantly increase the likelihood of a driver being determined "At Fault," and does it lead to more severe vehicle damage compared to non-distracted drivers? This question matters because understanding the specific risks of distraction can inform public policy, insurance models, and driver education.
## 2. Hypothesis

**Null Hypothesis**: There is no difference in the proportion of "At Fault" drivers or the median vehicle damage severity between distracted and non-distracted drivers.

**Alternative Hypothesis**: Distracted drivers are significantly more likely to be found "At Fault" and will have a higher median vehicle damage score.

## 3. Data Description

* Dercription of data source: The dataset is the "Crash Reporting - Drivers Data" provided by Montgomery County, Maryland (via Data.gov).
* Where it comes from (URL, API, dataset name): link  to the dataset:  https://catalog.data.gov/dataset/crash-reporting-drivers-data
* data/crash_reporting_drivers_data.csv - Each row in crash_data; includes ['Report Number', 'Local Case Number', 'Agency Name', 'ACRS Report Type', 'Crash Date/Time', 'Route Type', 'Road Name', 'Cross-Street Name', 'Off-Road Description', 'Municipality', 'Related Non-Motorist', 'Collision Type', 'Weather', 'Surface Condition', 'Light', 'Traffic Control', 'Driver Substance Abuse', 'Non-Motorist Substance Abuse', 'Person ID', 'Driver At Fault', 'Injury Severity', 'Circumstance', 'Driver Distracted By', 'Drivers License State', 'Vehicle ID', 'Vehicle Damage Extent', 'Vehicle First Impact Location', 'Vehicle Body Type', 'Vehicle Movement', 'Vehicle Going Dir', 'Speed Limit', 'Driverless Vehicle', 'Parked Vehicle', 'Vehicle Year', 'Vehicle Make', 'Vehicle Model', 'Latitude', 'Longitude', 'Location', 'Distraction_Status']

* Unit of analysis : Each observation represents a single driver involved in a crash reported by the Automated Crash Reporting System(ACRS)

**Number of observations:**
* The cleaned dataset contains approximately 158969 observations.

**key variables**
* **Driver Distracted** By: Independent variable (categorized into 'Distracted' vs. 'Not Distracted').

* **Driver At Fault**: Dependent variable 1 (Binary: Yes/No converted to 1/0).

* **Vehicle Damage Extent**: Dependent variable 2 (Ordinal scale mapped to 0-4: No Damage to Destroyed).

**Cleaning Steps**
* Filtered out rows where distraction status was "Unknown" or "No Driver Present."

* Standardized text columns to uppercase to merge categories like "Not Distracted" and "NOT DISTRACTED."

* Created a binary is_at_fault column.

* Mapped damage descriptions to a numeric damage_score (0-4).

## 4. Methods

Summarize how you analyzed the data:

I analyzed the data using resampling techniques to avoid assumptions about normality.

- **Permutation Test**: I used a permutation test for the Fault Rate (Difference in Proportions).

    - Test Statistic: The difference in the proportion of at-fault drivers between the distracted and non-distracted groups.

    - Simulation: I shuffled the is_at_fault labels 5,000 times to generate a null distribution where distraction has no effect on fault.

- **Bootstrapping**: I used bootstrapping to estimate uncertainty for two metrics:

    1. Difference in Proportions (Fault Rate): A standard metric where the CLT applies. 

    2. Difference in Medians (Damage Score): A robust metric for ordinal data where the CLT does not apply.

- Why CLT does not apply: The damage_score is discrete and ordinal (integers 0-4). The sampling distribution of a median from such a distribution is generally not normal, making standard parametric confidence intervals invalid.

## 5. Results

- **Fault Rate**:
    - **Observed Difference**: Distracted drivers were ~66% more likely to be found at fault (Difference in Proportions $\approx$ 0.6595).
    - **P-Value**: 0.0000 (Based on 5,000 permutations). The observed difference was far outside the null distribution.
    - **Bootstrap CI (95%)**: [0.656, 0.663]. This tight interval confirms a massive, statistically significant effect.
    
- Damage Severity:
    - **Observed Difference**: 0.0 (Median score was 2.0 for both groups).
    - **Bootstrap CI (95%)**: [0.0, 1.0]. While the observed difference was zero, the upper bound of the interval suggests distracted driving accidents could skew toward higher severity, but the median is not sensitive enough to capture it fully.

## 6. Uncertainty Estimation

I used 5,000 resamples for both the permutation test and bootstrapping.

- **Permutation Distribution**: The null distribution for the fault rate difference was centered at 0.0, with no overlap with the observed value of ~0.66. This indicates the result is extremely unlikely to be due to chance.

- **Bootstrap Distribution**:

    - The fault rate distribution was normally distributed and narrow, indicating high precision.

    - The median damage distribution was discrete and skewed, confirming that non-parametric methods were necessary.

## 7. Limitations

- **Self-Reporting Bias**: Distraction data is often self-reported or assessed by police at the scene, which may undercount actual distraction incidents.

- **Ordinal Precision**: The vehicle damage scale is coarse (0-4), which may mask subtle differences in crash severity that a continuous metric (like dollar cost) would reveal.

- **Causality**: While distraction is a strong predictor of fault, observational data cannot strictly prove causality without controlling for all confounding variables (e.g., weather, road type).

## 8. References

Dataset: Montgomery County, MD. "Crash Reporting - Drivers Data." Data.gov. [Link](https://catalog.data.gov/dataset/crash-reporting-drivers-data)

Tools: Python, Pandas, NumPy, Matplotlib, Seaborn, os.


---

**Reminder:** Your README should be clear enough that someone unfamiliar with your work could understand what you studied, how you analyzed it, and what you found.
