# Capstone-Project-ML-AI-Imperial-College
## BBO Capstone Project README
### Project overview
This capstone focuses on Black-Box Optimization (BBO). The main aim of this task is that I query unknown objective functions and use the returned outputs to choose better future inputs. I do not know each function’s analytical form in advance, so I treat it as a real-world optimization problem where only input-output interaction is available.

The overall goal is to learn a strategy that finds high-quality solutions with a limited number of evaluations. This is highly relevant in ML and applied optimization because many practical systems are expensive or opaque (for example, hyperparameter tuning, engineering design, and simulation-driven optimization). In these settings, every query can be costly, so sample efficiency matters.

From a career perspective, this project helps me demonstrate practical skills in iterative modelling, decision-making under uncertainty, and technical communication. It also strengthens my ability to justify model choices and adapt strategy based on evidence across multiple rounds.

### Input and Outputs

For each function, I submit an input vector (x) with a fixed dimensionality and receive a scalar output (y = f(x)).
•	Function 1: 2D input
•	Function 2: 2D input
•	Function 3: 3D input
•	Function 4: 4D input
•	Function 5: 4D input
•	Function 6: 5D input
•	Function 7: 6D input
•	Function 8: 8D input

The query format is a numeric vector (float values), and the response is a single numeric score. I store data as:
•	X: matrix of queried points, shape (n_samples, n_features)
•	Y: vector of observed outputs, shape (n_samples,)

Example:
•	Input: [0.998312, 0.002851]
•	Output: -0.08088785665400462

As I receive new weekly outputs, I append them to the dataset and remove duplicates to keep my training data consistent.

### Challenge Objectives
My objective is to optimize each unknown function (maximize or minimize according to leaderboard scoring and observed behaviour), while operating under practical limits:
•	limited number of queries
•	unknown function structure
•	no direct gradients or closed-form expression

This creates a trade-off: I must explore the space enough to avoid missing better regions but exploit promising regions quickly to improve performance within the query budget.

### Technical Approach (Week 1-3)
Across the first three submissions, I used an iterative surrogate-model workflow:
•	Collect query-response pairs for each function.
•	Train/update a predictive model on current X, Y.
•	Use predictions and uncertainty-informed logic to propose the next query.
•	Submit, observe new output, and update the dataset.

I experimented with regression-based surrogates and Bayesian-style thinking for query selection. My approach combines:
•	Exploitation: select points predicted to perform well,
•	Exploration: sample uncertain or under-covered regions.

To keep the pipeline robust, I standardized weekly data updates by:
•	loading the correct function-specific .npy files,
•	appending one new point at a time,
•	enforcing shape consistency by function dimension,
•	de-duplicating repeated points with np.unique,
•	saving clean updated X and Y back to disk.

This project is a living process, meaning I will continue refining model choice, acquisition strategy, and query efficiency as more results become available.
Biggest Lesson So-Far - in black-box settings, disciplined data management and careful exploration-exploitation balance are as important as the model itself. I plan to keep improving this README as my strategy matures in later submissions.
