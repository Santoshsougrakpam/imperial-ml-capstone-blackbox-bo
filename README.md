# Imperial College London - ML/AI Capstone: Blackbox Bayesian Optimization (BBO)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

## Overview
This repository contains my work for the final Capstone Project of the Imperial College London ML/AI Professional Certificate. 
The Capstone Project is structured as competition among a cohorts of 100+ students, modeled after NeurIPS(Neural Information Processing System) 2020 BBO challenge.
I was tasked to find global maximum of 8 unknown Black Box functions of dimensions ranging from 2D to 8D. 
Each function simulates real world tasks where evaluation is expensive like radiation source detection, drug discovery, chemical process etc.

**Competition:** Over a 13 week period, the competition runs in a blind format. Each week I generate queries from my Bayesian Optimization(BO) model and submit the queries to the competition portal and wait for a result. 
I had no knowledge of my peer's strategy nor I see the leaderboard to see my progress in the competition.

**Result:** When the final leaderboard was revealed after 13 weeks, I emerged as the **top-performing participant**, being the only student in the cohort to secure two 1st-place rankings (F1 and F5). 
This demonstrated that my optimization strategy was robust and consistent across the diverse set of functions.

### About the Project
In fields like neural networks hyperparameter tuning, engineering design or drug discovery, we often need to find perfect settings without knowing the exact shape of the "landscape" we are searching. 
By building a statistical surrogate model via Gaussian Processes (GP) using scikit-learn, my approach predicts the most promising areas to sample next, reducing the computational overhead compared to brute-force searching. 

#### My Implementation Strategy
- **Surrogate Modeling with Matern Kernels**: I use Gaussian Process (GP) with Matern 5/2 kernel and WhiteKernel for noise estimation. I implemented Automatic Relevance Determination(ARD) so that each dimension length scale is set independently which is essential for feature sensitivity analysis.
- **Numerical Stability via Log-standardisation**: For Functions with target 'y' values ranges extreme magnitude(very small to very large value), I apply log10 transformation. 
This is followed with 'StandardScaler' standardisation so that kernel can accurately map the data landscape and maintain numerical stability across the entire search space.  
- **'Zoom in' and Pivot mechanism**: I balance search and local refinement by tuning acquisition functions, Upper Confidence Bound(UCB) and Expected improvement (EI). 
Once I caught a hint of a peak, I 'zoom-in' by shifting the acquisition parameters($\kappa$ and $\xi$) towards exploitation to refine the result. When GP uncertainty map indicates that the local refinement is stagnated, I pivot to exploration to search for global optimum elsewhere.
- **Dimension Reduction and sensitivity**: To manage high dimensional complexity on search space, I use sensitivity analysis to identify and freeze non-influential features at the current best values thereby reducing the search space dimensionality.
- **Heuristics Boundary extension**: For unimodal Function-5, I apply extrapolation logic to extend my search space boundary when the model high-performance results trend towards the edges of the search space [1,1,1,1]. 
By extending the initial hypercube boundaries, I uncover region of exponential growth that was hidden by artificial caps.
- **Visualisation and Monitoring**: I use **parallel coordinate plots** to visualize the feature interactions and identify high-performance cluster within the target regions. I track progress using **convergence plots** and **query distance plots**.


## Project Structure
- `data/`: Contains datasets used for testing and validation.
- `notebooks/`: Jupyter notebooks demonstrating the implementation of the optimization algorithms. 
 To provide a clear view of the development process, each function includes a dedicated notebook for each week, tracking and demonstrating the evolution of the project over a 13-week period.
- `docs/`: Detailed project documentation.
    - `Datasheet.md`: Information regarding the data sources and processing.
    - `ModelCard.md`: Technical specifications and performance metrics of the models used.

## Summary of Results
After 13 weeks of iterative optimization, the following peak values (y) were achieved for each blackbox function:

> **Performance Note:** Highest number of 1st-place rankings in the cohort (2/8 functions).

| Functions                     | Rank    | Initial Best(y) | Week 13 best (y) |  Improvement (%) |
|:------------------------------|:--------|----------------:|-----------------:|-----------------:|
| 🥇**F1: Radiation Source**    | **1st** |       0.0036061 |        1.9507410 |    **53,995.6%** |
| 🎖️F2: Log-likelihood score    | 10th    |        0.611205 |         0.757247 |            23.9% |
| F3: Drug discovery            | 24th    |       -0.034835 |        -0.008595 |            75.3% |
| F4: Warehouses storage        | 35th    |       -6.702089 |         0.316342 |           104.7% |
| 🥇**F5: Chemical process**    | **1st** |     1088.859618 |     2.145994e+07 | **1,970,764.1%** |
| F6: Cake recipe               | 30th    |       -0.714265 |        -0.303087 |            57.6% |
| F7: 6D hyperparameters Tuning | 18th    |        1.364968 |         2.739184 |           100.7% |
| F8: 8D hyperparameters Tuning | 43rd    |        9.598482 |         9.861678 |             2.7% |


**Note on F1 Initial Best(y)**: The initial best value of 0.003606 represents the absolute magnitude of a negative background noise fluctuation. 
Transforming the data via log(abs(y)) was necessary to model the steep gradient of the radiation field, which set this noise artifact as the initial mathematical maximum. 
The 53,995% improvement highlights the algorithm's ability to explore away from local noise boundaries and successfully converge on the radiation peak.


## Getting Started
To explore the implementation:
1. Clone this repository to your local machine.
2. Install the necessary dependencies (ensure you have `jupyter` installed).
3. Navigate to the `notebooks/` directory to run the optimization experiments. To see the evolution of the model, run each week jupyter notebook.

## Documentation
For a deep dive into the methodology and data handling, please refer to the files in the `docs/` directory:
- [Data Sheet](docs/Datasheet.md)
- [Model Card](docs/ModelCard.md)

---
**Author**:  
**Santosh Sougrakpam** 
<br/>Imperial College London ML/AI Capstone 2026

**Contact:** santosh.sougrakpam [at] gmail.com | [LinkedIn Profile](https://www.linkedin.com/in/santosh-sougrakpam-8491071/)
<br/>[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

