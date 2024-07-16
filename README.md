# heating-system-optimisation

## Overview

This project explores an optimization problem to minimize the energy cost of heating a house. The goal is to maintain the indoor temperature within a specified range while considering variable ambient temperatures and electricity costs.

## Problem Statement

The temperature dynamics inside a house can be modeled using a differential equation incorporating Newton's Law of Cooling. Additionally, electricity costs vary with time, affecting the optimization strategy. The objective is to minimize the total energy cost over a given time horizon while maintaining the indoor temperature within specified bounds.

## Mathematical Formulation

$$ \frac{dT}{dt} = k \cdot (T_{\text{ambient}}(t) - T(t)) + \alpha \cdot P_{\text{heater}}(t) $$
