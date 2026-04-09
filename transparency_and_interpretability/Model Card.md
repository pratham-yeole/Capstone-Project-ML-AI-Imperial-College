# **Model Card**

## **System**
### **Model Details**
- **Model Name**: Black-Box Optimisation Strategy 
- **Model Type**: Sequential optimisation / decision-making system 
- **Developer**: Pratham Yeole 
- **Version**: Iterative (updated weekly) 
- **Date**: 09/04/2026

- This model is designed to optimise unknown objective functions through iterative querying and adaptive learning, without access to the underlying functional form.

## **Intended Use**
### **Primary Use Cases**
- Optimisation under uncertainty 
- Expensive function evaluation settings 
- Simulation-based decision systems 
- Experimental design problems

### **Intended Users**
- **ML/AI Practitioners**: Evaluate optimisation performance 
- **Model Developers**: Improve training/query strategy 
- **Policy Makers / Evaluators**: Assess responsible use and risks 
- **Software Developers**: Integrate into decision pipelines 
- **Affected Individuals** (indirect): Impacted by downstream decisions 

### **Out-of-Scope Use**
- Safety-critical real-time systems 
- Fully autonomous decision-making without human oversight 
- High-dimensional optimisation without adaptation mechanisms

## **Model Inputs and Outputs**
### **Inputs**
- Continuous numerical vectors $x∈R^d$
- Dimensionality varies (2D–8D)

### **Outputs**
- Scalar objective value: 
$y=f(x)$

### **Internal Data**
- Accumulated query dataset: 
	- Inputs X
	- Outputs Y

## **Modelling Approach**
- The model follows an iterative optimisation loop:
	1. Initial exploration (sampling) 
	2. Evaluation of objective function 
	3. Strategy update based on observed results 
	4. Selection of next query points 
	5. Repeat

- This reflects a trade-off between:
	- **Exploration**: covering unknown regions 
	- **Exploitation**: refining high-performing areas

## **Performance Characteristics**

- **Performance is evaluated using**:
  - Best observed objective value
  - Convergence rate over iterations
  - Sample efficiency (performance vs query budget)
  - Robustness across varying dimensionality 

## **Bias, Risks, and Limitations**
- **Sources of Bias**
  - **Sampling bias**: Model only explores limited regions of input space
  - **Strategy bias**: Early decisions influence future search trajectory
  - **Feature neglect**: Some regions/dimensions may be underexplored
    
- **Risks**
  - Convergence to local optima 
	- Over-exploitation of early high-performing regions 
	- Poor coverage of the full solution space 

## **Ethical Considerations**
- Although this is a technical optimisation system:
  - No personal or sensitive data is used
  - However, downstream applications could inherit risks, including:
    - Inefficient resource allocation
    - Suboptimal or misleading decisions

- **Responsible Use Recommendations**
	- Maintain human oversight 
	- Monitor optimisation trajectory 
	- Validate results across multiple runs

## **Transparency and Accountability**
- This model card supports:
	- **Transparency**: clear description of behaviour and limitations 
	- **Accountability**: documentation of assumptions and risks 
	- **Communication**: alignment between developers and stakeholders 

## **Model Lifecycle Considerations**
- Model evolves incrementally over time
  - Performance depends on:
    - Query history 
	  - Strategy updates 
	- Requires continuous monitoring and evaluation

- **Limitations**
	- No guarantee of global optimum 
	- Performance degrades with dimensionality
