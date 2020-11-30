# Trustworthy Online Controlled Experiments

- Simplest type of experiment:
- A/B Test, a controlled experiment that compares two `variants` 
	- A: `control`
	- B: `Treatment`


Key themes in online experiments:
- It is hard to assess the value of an idea
- small changes can have a big impact	
- experiments with big impact are rare
- the overhead of running an experiment must be small
- OEC, overall evaluation criteria must be clear


## Terminology
- `Control`: Existing system
- `Treatment`: Existing system with Feature X
- `Overall Evaluation Criterion`: Quantitative measure of the experiment's objective
	- Must be measurable in the short term 
	- causally drive long-term objectives 
	- Synonyms: Outcome, evaluation, Fitness Function
- `Parameter`: A controllable experimental variable that is thought to influence the OEC 
or other metrics of interest
	- Factors, variables
	- Assigned `values` or `levels`
	- A simple A/B Test has a single parameter with two values
	- Univariable design with multiple values: A,B,C,D 
	- `Multivariate Test`: MVTs, evaluate multiple parameters together
- `Variant`: A user experience being tested, in an A/B Test:
	- A = Control (special variant, this is the existing version on which to run the experiment)
	- B = Treatment
- `Randomized Unit`: Pseudo-randomization (eg: hashing) process applied to units (users, pages)
to map them to variants
	- Mapping must be persistent
	- Proper randomization is critical: each user should have the same chance to be assigned 
to each variant
	- It is recommended to use users as the unit


## Why Experiment? Correlations, Causality, and Trustworthiness
- Correlation does not imply causality
- Hierarchy of evidence:
![](https://www.researchgate.net/profile/Dale_Hattis2/publication/311504831/figure/fig1/AS:437009874460674@1481202681874/Hierarchy-of-evidence-pyramid-The-pyramidal-shape-qualitatively-integrates-the-amount-of.png)
- Controlled experiments are:
	- The best scientific way to establish causality with high probability
	- Able to detect small changes, such as changes over time (sensitivity)
	- Able to detect unexpected changes
- User other unstrustworthy methods only when online controlled experiments are not possible.


## Necessary Ingredients for Running Useful Controlled Experiments



