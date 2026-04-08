---
layout: post
title: Beyond the Blanket Shutdown - Analyzing Commercial Energy Consumption During COVID-19
image: "/posts/covid-1.jpg"
tags: [Covid-19, Electricity] 
---

## Main Takeaways
In much of the research regarding electricity usage, utility customers are placed into the broad categories of residential, commercial, and industrial. These groups are traditionally viewed as homogeneous. However, working with real-world municipal data right here in Fort Collins, Colorado, quickly reveals that reality is far more complex. 

When the COVID-19 pandemic triggered widespread shutdowns, it resulted in what has been called "the biggest psychological experiment in history." Treating all commercial properties as a single monolith masks the true impact of public policy. Our research demonstrated that varying business types responded to government-mandated changes in operations in vastly different ways. By applying high-resolution data analysis to our mid-sized US city, we proved that it is an oversimplification to view commercial properties as a single, homogeneous group. Understanding the nuanced responses of specific property types is essential for accurately monitoring financial impacts, managing municipal utility assets, and designing future energy conservation policies.

---

## The Analytics Project Flow

### 1. Problem Statement
The primary objective of this project was to analyze the deviation from expected electricity usage patterns in commercial properties due to the COVID-19 pandemic. We aimed to link these usage patterns specifically to the timing of public health and executive orders, requiring a fine-grained, detailed understanding of usage across different property classifications, such as restaurants, hotels, schools, and outpatient medical facilities.

### 2. Data Collection & Preparation
The foundation of this analysis relied on aggregating granular, high-frequency data. In 2015, Fort Collins transitioned all of its electricity customers to advanced metering infrastructure (AMI). Our data pipeline integrated multiple distinct sources:

* **Smart Meter Data**: We utilized detailed 15-minute interval data for approximately 77,000 electrical accounts. This interval data from 2019 to 2020 was aggregated into 24-hour windows for the analysis.
* **Property Assessor Data**: To achieve our required granularity, we moved past standard utility rate classes. We integrated the utility meter data with publicly available building data maintained by the Larimer County Assessor. We meticulously reviewed 321 different county classifications and condensed them into 28 standardized categories that align with ENERGYSTAR Portfolio Manager building types. 
* **Meteorological Data**: Temperature is a primary indicator of electricity usage. Because our study area is a fairly compact 50 square miles, we used a single temperature source to supply daily average temperatures for our regression models.

![Data Integration Pipeline Placeholder](assets/data_pipeline_diagram.png)
*(Image placeholder: A flowchart showing the integration of 15-minute AMI data, Larimer County Assessor classifications, and localized meteorological data into a unified analytics dataset.)*

### 3. Methodology
Establishing a robust baseline is critical in any economic or asset management analysis. To determine what electricity consumption would have been in the absence of a pandemic, we used a modified version of the International Performance Measurement and Verification Protocol (IPMVP) Option C. 

Because local heating is largely driven by gas-burning forced-air systems and cooling by electric air-conditioning, the relationship between temperature and electricity consumption changes dramatically around the equilibrium point of $55^{\circ}F$ ($13^{\circ}C$). We parameterized electricity consumption using two distinct temperature splines, with day-type classifications (weekday vs. weekend) as a secondary parameter.

**Heating Days Linear Fit ($T \le 55^{\circ}F$):**
For colder days, electricity usage scales linearly as a secondary heating/operational requirement.
$$E_{expected} = p_0 + p_1 \cdot T$$

**Cooling Days Parabolic Fit ($T > 55^{\circ}F$):**
For warmer days, electric air-conditioning drives an exponential increase in power demand.
$$E_{expected} = p_0 + p_1 \cdot T + p_2 \cdot T^2$$

*(Where $E_{expected}$ is the daily expected electricity consumption and $T$ is the average daily temperature.)*

Average daily temperatures for each day in 2020 were plugged into our 2019 regression equations to create the daily expected consumption baseline. The specific impact of the COVID-19 pandemic was then estimated as the ratio of the actual consumption from the metering data to this expected consumption baseline.

![Regression Curve Placeholder](assets/temperature_spline_regression.png)
*(Image placeholder: A dual-axis scatter plot showing the linear regression for temperatures below $55^{\circ}F$ and the parabolic regression curve for temperatures above $55^{\circ}F$.)*

### 4. Results & Insights
Our granular approach yielded critical insights that a macro-level view would have missed entirely. While electricity usage was lower than expected for most commercial property classes, the timing and magnitude of these effects varied widely. 

* **Hospitality Disparities**: Within the hospitality industry, hotels evidenced a larger and more sustained decrease in usage (-17.3%) as compared with food service restaurants (-11.1%). 
* **Education**: Poudre School District (K-12) buildings saw an 18.4% decrease in consumption, underscoring the rapid shift to remote learning.
* **Healthcare Variance**: Outpatient medical facilities experienced a 2.6% drop, with usage patterns that directly tracked with specific executive orders. Meanwhile, inpatient healthcare remained largely unchanged, showing only a negligible 0.8% decrease as hospitals remained fully operational.

### 5. Conclusion
This project reinforced a core principle of municipal asset management and data analytics: granularity matters. By systematically integrating discrete datasets—smart meter intervals, tax assessor building classifications, and meteorological data—we built a robust model that accurately tracked the nuanced, sector-specific impacts of an unprecedented global event. The findings highlight the malleability of electricity usage and provide valuable insights into the potential impact of policy instruments on future energy conservation strategies.
