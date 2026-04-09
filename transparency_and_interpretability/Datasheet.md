# **Dataset Overview** - Black-Box Optimisation Query Dataset
- **Creator**: Pratham Yeole
- **Date Created**: 09/04/2026
- **Version**: Iteratively updated (weekly)

- **Description**

  - This dataset consists of sequentially collected input–output pairs generated through interaction with multiple unknown objective functions.

  - Each dataset corresponds to a separate optimisation task with varying dimensionality (2D–8D).

## **Motivation**
- Why was this dataset created?
  - To simulate real-world optimisation problems where:
    - The function is unknown
    - Evaluations are costly

  - To study:
    - Exploration vs exploitation trade-offs
    - Sample-efficient learning strategies
    - Sequential decision-making behaviour

## **Dataset Composition**
### **Structure**
- Each dataset contains:
  - Inputs (X):
    - Continuous vectors: $x∈R^d$

  - Outputs (y):
    - Scalar values: $y=f(x)$

## **Dimensionality by Function**
| Function | Dimension |
|-----------|------------|
| F1        | 2D         |
| F2        | 2D         |
| F3        | 3D         |
| F4        | 4D         |
| F5        | 4D         |
| F6        | 5D         |
| F7        | 6D         |
| F8        | 8D         |

## **Data Collection Process**

- How was the data collected?
  - Data was collected iteratively over multiple weeks

- At each step:
  - A query point $x$ is selected by the optimisation strategy
  - The system returns $y=f(x)$
  - The pair $(x,y)$ is stored

- Important Characteristics:
  - **Sequential dependency**: Each new data point depends on previous observations
  - **Strategy-driven sampling**: Data distribution reflects the model’s behaviour

## **Preprocessing and Cleaning**
- Steps Applied:
  - Aggregation of weekly data into cumulative datasets
  - Formatting into structured numerical arrays
  - Consistent dimensional alignment across functions

## **Uses of the Dataset**
- **Intended Uses**
  - Evaluating optimisation strategies
  - Analysing exploration vs exploitation
  - Studying convergence behaviour
  - Teaching sequential decision-making

- **Not Recommended Uses**
  - Supervised learning benchmarking
  - General regression tasks
  - Fairness or bias benchmarking across populations

## **Dataset Biases and Limitations**

### **Sources of Bias**
- **Sampling bias**: Data concentrated in high-performing regions
- **Exploration bias**: Early random exploration influences later distribution
- **Coverage bias**: Some areas of the input space remain unobserved

### **Key Risk**

- The dataset does not represent the full input space, it reflects where the model chose to look.

## **Dataset Splitting and Evaluation**
- **Splitting Strategy** - Traditional splits (train/test) are not applicable.

- Instead:
  - Temporal structure (by week)
  - Performance evaluated via:
  - Best-so-far value
  - Convergence trajectory

- **Important discalimer**: Historical performance does not guarantee future generalisation.

## **Ethical Considerations**

- **Direct Risks**: None (synthetic data, no personal information)

- **Indirect Risks** - If applied in real-world contexts:
  - Biased sampling can lead to biased decisions
  - Under-explored regions will mean there is potential to miss optimal solutions

### **Mitigation**
- Multiple optimisation runs
- Monitoring search coverage
- Transparency via documentation

## **Distribution**

- Dataset is stored in a GitHub repository.

- The GitHub repository organised by:
  - Weekly queries
  - Processed input-output files

### **Accessibility**
  - Open for academic use
  - No licensing restrictions

## **Maintenance and Updates**

### **Update Process**
- Dataset grows incrementally each week
- New data appended rather than replaced

### **Maintenance Responsibilities**
- Ensure consistency of formatting
- Track changes across iterations
- Monitor dataset bias over time

## **Data Retention and Longevity**
- Data is retained for:
  - Reproducibility
  - Performance comparison across iterations

- Long-term value:
  - Enables analysis of optimisation strategy evolution
