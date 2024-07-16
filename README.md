# heating-system-optimisation

## Overview

This project explores an optimization problem to minimize the energy cost of heating a house. The goal is to maintain the indoor temperature within a specified range while considering variable ambient temperatures and electricity costs.

## Problem Statement

The temperature dynamics inside a house can be modeled using a differential equation incorporating Newton's Law of Cooling. Additionally, electricity costs vary with time, affecting the optimization strategy. The objective is to minimize the total energy cost over a given time horizon while maintaining the indoor temperature within specified bounds.

## Mathematical Formulation

$$ Equation (1): \frac{dT}{dt} = k \cdot (T_{\text{ambient}}(t) - T(t)) + \alpha \cdot P_{\text{heater}}(t) $$

where:
- $T(t)$: Temperature inside the house (°C)
- $T_{\text{ambient}}(t)$: Ambient temperature outside the house (°C)
- $P_{\text{heater}}(t)$: Power supplied to the heater (kW)
- $k$: Cooling coefficient (hours $^{-1}$ )
- $\alpha$: Heating coefficient (°C $\cdot$ kWh $^{-1}$ )

The ambient temperature and electricity cost evolve as follows:

$$ Equation (2): T_{\text{ambient}}(t) = 15 - 10 \sin\left(\frac{\pi}{12} (t + 4)\right) $$

$$ Equation (3): c(t) = 40 + 25 \sin^2\left(\frac{\pi}{12} t\right) $$

### Why This Formulation?

- **Newton's Law of Cooling**: This law provides a simple yet effective way to model the rate of change of temperature in a system. It assumes that the rate of heat loss of a body is proportional to the difference in temperatures between the body and its surroundings. This is suitable for our scenario where the house loses heat to the environment.
- **Incorporating Heating Power**: By adding a term for heating power, we can control the temperature inside the house. The coefficient $\alpha$ relates the power supplied to the heater to the resulting increase in temperature, making the model more realistic and actionable.
- **Periodic Ambient Temperature**: The ambient temperature is modeled as a sinusoidal function to reflect daily temperature variations. This periodic function captures the natural fluctuations in outdoor temperature.
- **Variable Electricity Cost**: Electricity cost is also modeled as a periodic function, which reflects typical daily changes in electricity pricing, such as peak and off-peak rates. This variability adds another layer of realism to the optimization problem.

## Discretization

To transform the continuous ODE into a discrete-time problem, we use the Euler method. The Euler method approximates the temperature at the next time step by adding the product of the time step size ($\Delta t$) and the rate of change of temperature to the current temperature. This gives us the following discrete-time equations:

$$ T(t + \Delta t) = T(t) + \Delta t \cdot \left( k \cdot (T_{\text{ambient}}(t) - T(t)) + \alpha \cdot P_{\text{heater}}(t) \right) $$

## Discrete-time Optimization Problem

The objective is to minimize the total energy cost while maintaining the indoor temperature within specified bounds. The discrete-time mathematical optimization problem can be formulated as follows:

$$ 
\begin{align*}
& \text{Minimize} \quad \sum_{t=0}^{n-1} c[t] \cdot P_{\text{heater}}[t] \cdot \Delta t \\
& \text{s.t.} \\
& T[0] = T_{\text{initial}} \\
& T[t] = T[t-1] + \Delta t \cdot \left( k \cdot (T_{\text{ambient}}[t] - T[t-1]) + \alpha \cdot P_{\text{heater}}[t-1] \right), \quad \forall t \in \{1, \ldots, n-1\} \\
& P_{\text{min}} \cdot x[t] \leq P_{\text{heater}}[t] \leq P_{\text{max}} \cdot x[t], \quad \forall t \in \{0, \ldots, n-1\} \\
& T_{\text{min}} \leq T[t] \leq T_{\text{max}}, \quad \forall t \in \{0, \ldots, n-1\} \\
& x[t] \in \{0, 1\}, \quad \forall t \in \{0, \ldots, n-1\}
\end{align*}
$$

where:
- $n$: Total number of steps ($\frac{H}{\Delta t}$)
- $c[t]$: Electricity cost at time $t$, converted to \$ per kWh for consistency with $P_{\text{heater}}[t]$
- $P_{\text{heater}}[t]$: Power supplied to the heater at time $t$, continuous variable with bounds $[0, P_{\text{max}}]$
- $x[t]$: Binary variable indicating if the heater is on (1) or off (0) at time $t$
- $T[t]$: Temperature inside the house at time $t$, continuous variable with bounds $[T_{\text{min}}, T_{\text{max}}]$

## Baseline Strategy

In addition to developing an optimized heating strategy, it is essential to establish a baseline for comparison. The baseline strategy serves as a reference point to evaluate the effectiveness and efficiency of the optimization model. Here’s why the baseline strategy is chosen:

### Description of the Baseline Strategy

- **Heating Control:**
  - The baseline strategy aims to maintain the indoor temperature close to a predefined setpoint (e.g., 24°C) without considering fluctuations in electricity costs.
  - The heater operates at maximum capacity (12 kW) when the indoor temperature drops below the setpoint, and turns off when the setpoint is reached or exceeded.

- **Electricity Cost Ignorance:**
  - Unlike the optimized strategy, the baseline does not adjust power usage based on the varying electricity costs. It uses a fixed rule-based approach that ignores the dynamic pricing, leading to potentially higher operational costs.
    
### Purpose of the Baseline Strategy

1. **Benchmarking Performance:**
   - The baseline strategy provides a standard against which the optimized strategy's performance can be measured. By comparing the results, we can quantify the improvements in terms of cost savings and temperature stability.

2. **Simplicity and Realism:**
   - The baseline strategy represents a straightforward, rule-based approach that is easy to understand and implement. It mirrors a common real-world scenario where heating systems operate based on a simple setpoint control without considering dynamic electricity pricing.

3. **Identifying Improvement Areas:**
   - By analyzing the baseline strategy, we can identify specific areas where optimization can yield significant benefits. For instance, recognizing periods of high power usage during expensive electricity times highlights the potential for cost savings through better scheduling.

## Implementation

The project is implemented using Python and the `mip` optimization library. The notebook contains the following sections:
1. Problem Formulation
2. Parameter Setup
3. Model Implementation
4. Results Visualization

## Running the Project

1. Clone the repository
2. Install the required libraries: `pip install mip numpy pandas plotly`
3. Run the Jupyter notebook to see the results

## Results

The optimized heating strategy minimizes the total energy cost while maintaining the indoor temperature within the specified range. The notebook includes plots comparing the optimized strategy to a baseline approach, highlighting the cost savings.
- The insights gained from the optimization model can be scaled and applied to larger systems, such as commercial buildings or smart grids. By leveraging similar optimization techniques, broader applications can achieve significant energy and cost savings.

## Future Work

1. Incorporate randomness in ambient temperature and electricity costs
2. Extend the model to handle longer time horizons
3. Explore alternative optimization techniques for better scalability

