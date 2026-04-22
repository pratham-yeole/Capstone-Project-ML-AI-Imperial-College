# Capstone-Project-ML-AI-Imperial-College
## BBO Capstone Project README
### Project overview
This capstone focuses on Black-Box Optimization (BBO). The main aim of this task is that I query unknown objective functions and use the returned outputs to choose better future inputs. I do not know each function’s analytical form in advance, so I treat it as a real-world optimization problem where only input-output interaction is available.

The overall goal is to learn a strategy that finds high-quality solutions with a limited number of evaluations. This is highly relevant in ML and applied optimization because many practical systems are expensive or opaque (for example, hyperparameter tuning, engineering design, and simulation-driven optimization). In these settings, every query can be costly, so sample efficiency matters.

From a career perspective, this project helps me demonstrate practical skills in iterative modelling, decision-making under uncertainty, and technical communication. It also strengthens my ability to justify model choices and adapt strategy based on evidence across multiple rounds.

### Input and Outputs

For each function, I submit an input vector (x) with a fixed dimensionality and receive a scalar output (y = f(x)).
- **Function 1**: 2D input
- **Function 2**: 2D input
- **Function 3**: 3D input
- **Function 4**: 4D input
- **Function 5**: 4D input
- **Function 6**: 5D input
- **Function 7**: 6D input
- **Function 8**: 8D input

I have added more information about the specific dataset in the overview of the dataset folder. This should give you information on what the data is about and what the optimisation goal is.

The query format is a numeric vector (float values), and the response is a single numeric score. I store data as:
•	X: matrix of queried points, shape (n_samples, n_features)
•	Y: vector of observed outputs, shape (n_samples,)

Example:
- Input: [0.998312, 0.002851]
- Output: -0.08088785665400462

As I receive new weekly outputs, I append them to the dataset and remove duplicates to keep my training data consistent.

### Challenge Objectives
My objective is to optimize each unknown function (maximize or minimize according to leaderboard scoring and observed behaviour), while operating under practical limits:
- limited number of queries
- unknown function structure
- no direct gradients or closed-form expression

**This creates a trade-off**: I must explore the space enough to avoid missing better regions but exploit promising regions quickly to improve performance within the query budget.

### Technical Approach (Week 1-3)
Across the first three submissions, I used an iterative surrogate-model workflow:
- Collect query-response pairs for each function.
- Train/update a predictive model on current X, Y.
- Use predictions and uncertainty-informed logic to propose the next query.
- Submit, observe new output, and update the dataset.

I experimented with regression-based surrogates and Bayesian-style thinking for query selection. My approach combines:
- **Exploitation**: select points predicted to perform well
- **Exploration**: sample uncertain or under-covered regions.

To keep the pipeline robust, I standardized weekly data updates by:
- loading the correct function-specific .npy files,
- appending one new point at a time,
- enforcing shape consistency by function dimension,
- de-duplicating repeated points with np.unique,
- saving clean updated X and Y back to disk.

This project is a living process, meaning I will continue refining model choice, acquisition strategy, and query efficiency as more results become available.

**Biggest Lesson So-Far** - in black-box settings, disciplined data management and careful exploration-exploitation balance are as important as the model itself. 
- I plan to keep improving this README as my strategy matures in later submissions.

### Technical Approach (Weeks 4-6)

**Week 4** upgraded all functions from shared isotropic length scales to Automatic Relevance Determination (ARD), giving each input dimension its own independently optimised length scale. This turned the surrogate into a diagnostic tool, fitted length scales revealed which variables actually drive the output, essential in high-dimensional functions like F7 (6D) and F8 (8D).

**Week 5** was defined by mixed results: some functions improved (F2, F7) while others regressed (F3, F4, F5, F6, F8). The key insight was that a step backward means different things depending on function structure, for a unimodal function (F5) it means "return to the peak"; for a multi-modal cliff-ridden landscape (F4) it means "escape the region." This drove a deliberate split between exploit-first EI and explore-first UCB across the portfolio.

**Week 6** focused on kernel stabilisation. For functions approaching convergence (F3, F5, F6), models were held constant, changing a kernel mid-convergence discards accumulated learning. For functions still struggling (F1, F4), higher exploration weights were maintained.

Across all three weeks, acquisition function choice followed a consistent decision rule:
- UCB with high beta: when outputs are near-zero or the surface is rugged and local-optima-prone (F1, F4)
- EI with low xi: when a promising region has been found and exploitation is warranted (F3, F5, F7)
- EI with moderate xi: when recovering from a regression in a high-dimensional space (F6, F8)

**The biggest lesson from Weeks 4-6**: the acquisition function and kernel must be co-designed per function, not applied uniformly. 
- A single strategy applied across all eight functions would have caused premature exploitation in some and aimless exploration in others.

## Technical Approach (Weeks 7-9)

**Week 7** introduced Yeo-Johnson power transformations to stabilize output skewness. By mapping outputs (notably in F1, F2, and F4) to a Gaussian distribution, the surrogate models gained the sensitivity needed to distinguish exceptional peaks from good results, preventing the over-smoothing seen in earlier weeks.

**Week 8** pivoted to a Focused Search Grid. Sampling shifted from uniform global exploration to a Normal distribution centred on all-time best coordinates (80-85% density). This high-resolution exploitation directly enabled yield breakthroughs in F5 and near-perfect convergence in F3.

**Week 9** finalized Kernel Specialization and Hybrid Search. WhiteKernel integration for F2 prevented the model from hallucinating patterns in noise. For high-dimensional tasks (F7, F8), a 15-20% uniform sampling component was maintained alongside local grids as an insurance policy against the curse of dimensionality and undiscovered global optima.

The refined acquisition decision rules for the final optimization stretch were:
- EI with Ultra-Low xi (0.0005–0.001): Squeezed final gains from converged surfaces (F3, F5).
- UCB with High beta: Forced movement out of persistent local optima in rugged landscapes (F4).
- EI with Power Transforms: Mathematically prioritized marginal improvements during high-dimensional recovery (F6, F8).

**The biggest lesson from Weeks 7-9**: Success shifted from finding the right neighbourhood to finding the exact address. 
- The combination of output transformation and localized search proved that a Gaussian Process is most effective when transitioned from a global explorer to a high-precision local optimizer.

## **Technical Approach (Week 10-12)

**Week 10** transitioned to Precision Exploitation by deploying Focused Grids with ultra-tight search radii (std 0.008–0.010). Concentrating sampling density around historical bests yielded immediate breakthroughs in Function 2 (reaching 0.6296) and pushed Function 5 yield past the 6000-unit threshold.

**Week 11** implemented Dynamic Acquisition Tuning to stabilize high-dimensional noise. By lowering exploration parameters (xi) to 0.0003–0.0005 for converged functions (F3, F5) and maintaining Yeo-Johnson transforms for output stability, the models avoided local oscillation and established a clear trajectory for final queries in F7 and F8.

**Week 12** concluded with Full Model Commitment, shifting from probabilistic scouting to deterministic execution. For converged surfaces (F3, F6), exploration was bypassed to query the exact global maximum predicted by the GP mean. This terminal strategy successfully exhausted the search space, achieving a project-high 6605.98 yield for F5 and an all-time best of 2.614 for F7.

Final Decision Rules:
- Ultra-Low xi for EI (0.0003–0.0005): Squeezed final marginal gains from established unimodal peaks in F3 and F5.
- Matern 2.5 Kernel & WhiteKernel: Maintained as the primary architecture for F1–F8 to model complex surfaces while accounting for observation noise.
- Strict Parameter Ceilings: Locked high-dimensional queries (F7) within valid architectural limits to ensure usable final results.

**Takeaway/Lesson**: The final trimester proved that once a surface is mapped, success comes from shifting the Gaussian Process from a sceptical explorer to a high-precision executor, prioritizing localized refinement over global discovery.
