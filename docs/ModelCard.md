# Model Card

## Overview
**ModelName:** Gaussian Process Bayesian Optimizer (GP-BO)

**ModelType:** Gaussian Process Regressor (GPR). It utilizes Gaussian Process as a probabilistic surrogate  model to map the input **X** space to objective **y** response and uses uncertainty driven acquisition functions to select queries. 

**Version:** 2.0

## Intended use
The model is intended for Global Optimisation of expensive black box functions where the analytical form of the target function is unknown and budget for evaluation is strictly limited to 10 weekly queries.

#### Use cases
- Hyperparameter Tuning: Optimizing machine learning model parameters (e.g. learning rate, regularisation strength etc.)
- Expensive BlackBox Function Optimisation: Use the BO model where evaluating the target function is expensive in terms of cost, time or resources.
- Experimental Design: Search for optimal experimental conditions like temperature, pressure, chemical concentration etc. to yield best result.
- Sensitivity Analysis: Use the kernel length scale to find which features are more significant helping to narrow the search space for future studies. 

Avoid using the model for high-dimensional input spaces as it suffers from cures of dimensionality specially on the acquisition function to clear the uncertainty area.

## Details
Initial week1-week3, I spend time tuning the kernel length scale, noise level and 'y' values nornalised/scaled. With different GP settings, I spend the initial weeks to explore the data landscape. 
Once the model fairly understood data landscape and pick up peak values, I tune the UCB/EI acquisition to search for better high values and climb the data landscape. 
I use the sensitivity scale graph to look for which feature is contribution significantly as well its sensitivity to the result. For less sensitive features, once I found local best result, I freeze these less sensitive feature to current best value and plot the 2D map on other highly sensitive features to see their correlation.
Later in 3 or 4 weeks, I start using parallel coordinate plots for features to see the relation instead of plotting 2D plot as pair plot graph becomes less meaning for hinger dimensions.
I maintain a record of the weekly submission to keep track of weekly progress. A convergence report and query distance analysis indicates that my weekly results are converging to global optimal value. 

## Performance

| Functions                    | Initial Best(y) | Week 10 best (y) | Strategy used               |
|:-----------------------------|:----------------|:-----------------|:----------------------------|
| F1: Radiation Source         | 0.0036061       | 1.9507410        | Standard GP                 |
| F2: Log-likelihood score     | 0.611205        | 0.757247         | Standard GP                 |
| F3: Drug discovery           | -0.034835       | -0.008595        | Standard GP                 |
| F4: Warehouses storage       | -6.702089       | -2.5084003       | Standard GP                 |
| F5: Chemical process         | 1088.859618     | 263397.283       | Standard GP + Extrapolation |
| F6: Cake recipe              | -0.714265       | -0.303           | Standard GP                 |
| F7: 6D hyperparamters Tuning | 1.364968        | 2.652            | Standard GP                 |
| F8: 8D hyperparamters Tuning | 9.598482        | 9.861678         | Standard GP                 |

## Assumptions and limitations
One of main assumption is the query search space is continuous and relative smooth. I assume that points close to each other yields similar output results.
Also, the roughness of the query space is consistent across the search space and hence a stationary kernel length is used to map the entire data landscape. 

### Constraints 
- Query search space is mostly constraint to [0,1] hypercube with scope for extrapolation for some cases.
- Query budget is restricted to 10-week limit so far.

## Ethical considerations

- **Transparency:** By providing a detailed weekly query and result along with corresponding week jupyter notebook captures the weekly submission journey which can be traced and reproduced.
- **Safety and Real world Adaptation:** In real world an exponential growth can lead to equipment failure for example for risky chemical experiment. 
I do log-transform of 'y' target variables so the GP remains numerically stable and uncertainty variance remains calibrated. This prevent GP to make "explosive" suggestions in high magnitude regions.



