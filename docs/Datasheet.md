# DataSheet

## Motivation
The purpose of this datasheet is to track the performance and convergence of Bayesian Optimisation(BO) on 8 unknown block box functions as part my capstone project for ML/AI course at Imperial College, London.

The datasheet mainly supports the weekly sequential Black Box Optimisation. The running cumulative set of queries and result are fed to the Gaussian Process (GP) for Surrogate Model Calibration, Acquisition Function Evaluation and result Convergence analysis. 

## Composition
The data contains the input X - dimensional vector like [x1, x2, x3...] representing the queries space for the BO function and output Y - scalar output from the BO after submission.
Dataset component
 - Initial Baseline - initial_inputs.npy and initial_outputs.npy. These are baseline X and y values given to me for each unknown functions
 - Weekly Update - X and y updates are accumulated every week. The dataset grows each week as queries and result are added to the weekly dataset.
 - DataType - X is floating point number(float64), y is floating point number (float64)
 - Format - The raw data are stored in numpy binary files and weekly result are in csv format.

## Collection process
The dataset is collected iteratively week by week using Gaussian Process Bayesian Optimisation. 
- Each week the InitialBaseline dataset is updated with cumulative queries and result collected over the previous iterations.
- This cumulative dataset is feed to GP-BO surrogate model. The model estimate the mean and uncertainty across the search space.
- Acquisition functions - Maximising UCB/EI acquisition functions are used to generate the next week Query.
- Submit the new query to get the result and process loop starts again for the next week.

## Preprocessing and uses
 - The logarithmic transform(log(y)) is applied to functions that exhibit exponential range of y values. This allows GP to model relative change in data landscape rather than being overwhelmed with huge magnitude of absolute value. 
 - Input space (X) is normalized to [0,1] hypercube with room to extrapolate up to to 1.5 for one of the function.
The intended use for this datasheet
 -  Optimisation convergence tracking - track how each week BO result are converging.
 -  Capstone documentation - provide a traceable weekly record of data submission

## Distribution and maintenance
 - The datasheet is hosted in github
 - The weekly queries and result are recorded to each function jupyter noted book. This ensure a readable, transparent chronological log of the submission.
 - In addition, the queries and result are stored under /data/<function>/X_week_10.csv, /data/<function>/y_week_10.csv 