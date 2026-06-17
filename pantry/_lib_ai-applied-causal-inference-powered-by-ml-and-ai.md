

Applied Causal Inference Powered by ML and AI

Victor Chernozhukov∗		Christian Hansen†	Nathan Kallus‡ Martin Spindler§	Vasilis Syrgkanis¶
March 16, 2026




Publisher: Online Version 0.1.2





∗ MIT
† Chicago Booth
‡ Cornell University
§ Hamburg University
¶ Stanford University
 




























© Victor Chernozhukov, Christian Hansen, Nathan Kallus, Martin Spindler, Vasilis Syrgkanis
Colophon
This document was typeset with the help of KOMA-Script and LATEX using the kaobook class.
Publisher
First printed in September 2022 by the Authors for Online Distribution.
A website for this book containing all materials is maintained at
https://causalml-book.org/.
 
Contents
Contents	iii
Preface	2
0	Sneak Peek: Powering Causal Inference with ML and AI	4
Core Material	10
1	Predictive Inference with Linear Regression in Moderately High Di-mensions	11
1.1	Introduction	12
1.2	Foundation of Linear Regression	13
Regression and the Best Linear Prediction Problem	13
Best Linear Approximation Property	14
From Best Linear Predictor to Best Predictor	15
1.3	Statistical Properties of Least Squares	17
The Best Linear Prediction Problem in Finite Samples	17
Properties of Sample Linear Regression	18
Analysis of Variance	20
Overfitting: What Happens When 𝑝 𝑛 Is Not Small	21
Measuring Predictive Ability by Sample Splitting	22
1.4	Inference about Predictive Effects or Association	23
Understanding 𝛽1 via "Partialling-Out"	24
Adaptive Statistical Inference	26
1.5	Application: Wage Prediction and Gaps	28
Prediction of Wages	29
Wage Gap	32
1.6	Inference on Predictive Effect when 𝑝 𝑛 < 1 is not small★	34
1.7	Notes	36
1.8	Notebooks	36
1.9	Checklist	37
1.10	Exercises	37
1.A  Central Limit Theorem★	38
Univariate	38
 
Multivariate	39
2	Causal Inference via Randomized Experiments	43
2.1
Potential Outcomes Framework and Average Treatment Effects	. .
44
	Random Assignment/Randomized Controlled Trials . . . . . . . .
47
	Statistical Inference with Two Sample Means . . . . . . . . . . . . .
49
	Pfizer/BioNTech Covid Vaccine RCT  . . . . . . . . . . . . . . . . .
50
2.2
Pre-treatment Covariates and Heterogeneity . . . . . . . . . . . . .
52
	Regression and Statistical Inference for ATEs . . . . . . . . . . . . .
54
	Classical Additive Approach . . . . . . . . . . . . . . . . . . . . . . The Interactive Approach: Always Improves Precision and Discovers Heterogeneity . . . . . . . . . . . . . . . . . . . . . . . . . .	54
57
	Reemployment Bonus RCT . . . . . . . . . . . . . . . . . . . . . . .	58
2.3
Drawing RCTs via Causal Diagrams . . . . . . . . . . . . . . . . . .	59
2.4
Key Limitations of RCTs . . . . . . . . . . . . . . . . . . . . . . . . .	60
	Externalities, Stability, and Equilibrium Effects . . . . . . . . . . . .	60
	Ethical, Practical, and Generalizability Concerns . . . . . . . . . . .
61
2.5
Notes  . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . .	61
2.6
Notebooks  . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . .	62
2.7
Checklist  . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . .	62
2.8
Exercises  . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . .	63
2.A
Approximate Distribution of the Two Sample Means	. . . . . . . .
64
2.B
Statistical Properties of the Classical Additive Approach★ . . . . . .
65
3
Predictive Inference via Modern High-Dimensional Linear Regression
69
	3.1	Linear Regression with High-Dimensional Covariates . . . . . . . .
70
	The Framework	. . . . . . . . . . . . . . . . . . . . . . . . . . . . .	70
	Lasso  . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . .	72
	Quick Heuristics for Lasso Properties and Penalty Choice★ . . . . .
77
	OLS Post-Lasso . . . . . . . . . . . . . . . . . . . . . . . . . . . . . .	78
	3.2	Predictive Performance of Lasso and Post-Lasso . . . . . . . . . . .
3.3	A Helicopter Tour of Other Penalized Regression Methods for Prediction . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . .	79
82
	3.4	Choice of Regression Methods in Practice . . . . . . . . . . . . . . .
89
	3.5	Notes  . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . .	91
	3.6	Notebooks  . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . .	91
	3.7	Checklist  . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . .	92
	Checklist  . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . .	92
	3.8	Exercises  . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . .	92
	3.A  Additional Discussion and Results . . . . . . . . . . . . . . . . . . .	93
	Iterative Estimation of 𝜎 . . . . . . . . . . . . . . . . . . . . . . . . .	93
	Some Lasso Heuristics via Convex Geometry★ . . . . . . . . . . . .
94
	Other Variations on Lasso . . . . . . . . . . . . . . . . . . . . . . . .	95
	3.B	Cross-Validation . . . . . . . . . . . . . . . . . . . . . . . . . . . . .	96
	3.C  Laws of Large Numbers for Large Matrices★  . . . . . . . . . . . . .	98
 
3.D  A Sketch of the Lasso Guarantee Under Exact Sparsity★	99
4	Statistical Inference on Predictive Effects in High-Dimensional Linear Regression Models	105
4.1
Introduction  . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . .	106
4.2
Inference with Double Lasso . . . . . . . . . . . . . . . . . . . . . .	106
	Inference on One Coefficient  . . . . . . . . . . . . . . . . . . . . . .	106
	Application to Testing the Convergence Hypothesis . . . . . . . . .	110
4.3
Why Partialling-out Works: Neyman Orthogonality . . . . . . . . .	111
	Neyman Orthogonality . . . . . . . . . . . . . . . . . . . . . . . . .	111
	What Happens if We Don’t Have Neyman Orthogonality?  . . . . .	114
4.4
Inference on Many Coefficients . . . . . . . . . . . . . . . . . . . . .	117
	Discovering Heterogeneity in the Wage Gap Analysis . . . . . . . .
120
4.5
Other Approaches That Have the Neyman Orthogonality Property
122
	Double Selection . . . . . . . . . . . . . . . . . . . . . . . . . . . . .	122
	Debiased Lasso . . . . . . . . . . . . . . . . . . . . . . . . . . . . . .	122
4.6
Checklist  . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . .	124
4.7
Notes  . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . .	124
4.8
Notebooks  . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . .	125
4.9
Exercises  . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . .	125
4.A
High-Dimensional Central Limit Theorems★ . . . . . . . . . . . . .	126
5	Causal Inference via Conditional Ignorability	132
5.1	Introduction	133
5.2	Potential Outcomes and Ignorability	134
Identification by Conditioning	135
Conditional Ignorability via Causal Diagrams	138
Connections to Linear Regression	139
5.3	Identification Using Propensity Scores	140
Stratified RCTs	142
Covariate Balance Checks	142
Connections to Linear Regression	143
5.4	Conditioning on Propensity Scores★	143
5.5	Average Treatment Effect for Groups and on the Treated	145
5.6	Exercises	146
5.A	Rosenbaum-Rubin’s Result	147
5.B	Clever Covariate Regression	148
5.C	Details of ATET	148
6	Causal Inference via Linear Structural Equations	152
6.1	Structural Equation Modelling and Conditional Exogeneity	153
A Simple Triangular Structural Equation Model (TSEM)	153
6.2	Drawing the Model: Causal Diagrams, aka DAGs	156
6.3	When Conditioning Can Go Wrong: Collider Bias, aka Heckman Selection Bias	159
 
6.4
Wage Gap Analysis and Discrimination . . . . . . . . . . . . . . . .	162
6.5
Notes  . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . .	166
6.6
Notebooks  . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . .	166
6.7
Exercise . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . .	166
6.A
Details of the Wage Discrimination Analysis . . . . . . . . . . . . .	168
7	Causal Inference via Directed Acyclical Graphs and Nonlinear Struc-
tural Equation Models	172
7.1
Introduction  . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . .	173
7.2
General DAG and SEMs via an Example	. . . . . . . . . . . . . . .
174
	The Impact of 401(k) Eligibility on Financial Wealth . . . . . . . . .
174
	The DAG as a Markovian Model . . . . . . . . . . . . . . . . . . . .	175
	The DAG as a Structural Equations Model	. . . . . . . . . . . . . .
176
	Intervention and Counterfactual DAG and SEM  . . . . . . . . . . .
176
	Conditional Ignorability/Exogeneity  . . . . . . . . . . . . . . . . .
178
	Wrap-Up and Implications for 401(k) Analysis . . . . . . . . . . . .	180
7.3
Definitions of General DAGs and ASEMs . . . . . . . . . . . . . . .
180
	From DAGs to ASEMs . . . . . . . . . . . . . . . . . . . . . . . . . .	181
	D-Separation and Testable Restrictions  . . . . . . . . . . . . . . . .	182
7.4
Counterfactuals and Identification by Conditioning . . . . . . . . .
185
	Counterfactuals  . . . . . . . . . . . . . . . . . . . . . . . . . . . . .	185
	Ignorability by D-Separation in Counterfactual DAGs . . . . . . . .
186
	Ignorability by Backdoor Blocking in Factual DAG . . . . . . . . . .
189
7.5
Notes  . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . .	190
7.6
Additional Resources  . . . . . . . . . . . . . . . . . . . . . . . . . .	191
7.7
Notebooks  . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . .	191
7.8
Exercises  . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . .	192
7.A
Review of Conditional Independence . . . . . . . . . . . . . . . . .
193
7.B
Theoretical Details of d-Separation★ . . . . . . . . . . . . . . . . . .	193
7.C
Faithfulness and Causal Discovery . . . . . . . . . . . . . . . . . . .	195
8
Predictive Inference via Modern Nonlinear Regression
200
	8.1	Introduction  . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . .	201
	8.2	Regression Trees and Random Forests . . . . . . . . . . . . . . . . .	201
	Introduction to Regression Trees . . . . . . . . . . . . . . . . . . . .	201
	Random Forests  . . . . . . . . . . . . . . . . . . . . . . . . . . . . .	205
	Boosted Trees . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . .	207
	8.3	Neural Nets / Deep Learning	. . . . . . . . . . . . . . . . . . . . .	208
	Basic Ideas  . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . .	209
	Deep Neural Networks  . . . . . . . . . . . . . . . . . . . . . . . . .	213
	8.4	Prediction Quality of Modern Nonlinear Regression Methods  . . .
216
	Learning Guarantees of Trees and Forests . . . . . . . . . . . . . . .	217
	Learning Guarantees of DNNs . . . . . . . . . . . . . . . . . . . . .	220
	The Need for More and Better Theory . . . . . . . . . . . . . . . . .	222
	Trust but Verify	. . . . . . . . . . . . . . . . . . . . . . . . . . . . .	223
 
	A Simple Case Study using Wage Data  . . . . . . . . . . . . . . . .	224
8.5
Combining Predictions - Aggregation - Ensemble Learning . . . . .
225
	Auto ML Frameworks . . . . . . . . . . . . . . . . . . . . . . . . . .	227
8.6
When Do Neural Networks Win?	. . . . . . . . . . . . . . . . . . .	228
8.7
Notes  . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . .	229
8.8
Checklist  . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . .	230
8.9
Additional resources	. . . . . . . . . . . . . . . . . . . . . . . . . .	230
8.10
Notebooks  . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . .	231
8.11
Exercises  . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . .	231
8.A
Variable Importance via Permutations . . . . . . . . . . . . . . . . .	232
9	Statistical Inference on Predictive and Causal Effects in Modern Non-linear Regression Models	237
9.1
Introduction  . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . .	238
9.2
DML Inference in the Partially Linear Regression Model  . . . . . .
239
	Discussion of DML Construction . . . . . . . . . . . . . . . . . . . .	244
	The Effect of Gun Ownership on Gun-Homicide Rates	. . . . . . .
248
	Revisiting the Price Elasticity for Toy Cars	. . . . . . . . . . . . . .
252
9.3
DML Inference in the Interactive Regression Model . . . . . . . . .
254
	DML Inference on APEs and ATEs . . . . . . . . . . . . . . . . . . .	254
	DML Inference for GATEs and ATETs . . . . . . . . . . . . . . . . .	257
	The Effect of 401(k) Eligibility on Net Financial Assets . . . . . . . .
259
9.4
Generic Debiased (or Double) Machine Learning	. . . . . . . . . .
263
	Key Ingredients  . . . . . . . . . . . . . . . . . . . . . . . . . . . . .	263
	Neyman Orthogonal Scores for Regression Problems  . . . . . . . .
266
	The DML Inference Method	. . . . . . . . . . . . . . . . . . . . . .	267
	Properties of the General DML Estimator . . . . . . . . . . . . . . .
269
9.5
Notes  . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . .	271
9.6
Notebooks  . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . .	272
9.7
Exercises  . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . .	273
9.A
Bias Bounds with Proxy Treatments . . . . . . . . . . . . . . . . . .	275
9.B
Illustrative Neyman Orthogonality Calcuations	. . . . . . . . . . .
276
10
Feature Engineering for Causal and Predictive Inference
280
	10.1  Introduction  . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . .	281
	10.2 From Principal Components to Autoencoders	. . . . . . . . . . . .
282
	Variational Autoencoders . . . . . . . . . . . . . . . . . . . . . . . .	286
	10.3 From Autoencoders to General Embeddings  . . . . . . . . . . . . .	287
	10.4 Text Embeddings	. . . . . . . . . . . . . . . . . . . . . . . . . . . .	288
	Revisiting the Price Elasticity for Toy Cars	. . . . . . . . . . . . . .	299
	10.5 Image Embeddings  . . . . . . . . . . . . . . . . . . . . . . . . . . .	301
	10.6 Application: Hedonic Prices  . . . . . . . . . . . . . . . . . . . . . .	303
	10.7 Notes  . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . .	306
	10.8 Notebooks  . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . .	306
	10.9 Exercises  . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . .	307
 

Topics
311
11 Deeper Dive into DAGs, Good and Bad Controls
312
11.1
Valid Adjustment Sets . . . . . . . . . . . . . . . . . . . . . . . . . .	313
11.2
Other Useful Adjustment Strategies . . . . . . . . . . . . . . . . . .	313
	Conditioning on Parents	. . . . . . . . . . . . . . . . . . . . . . . .	314
	Conditioning on All Common Causes of 𝐷 and 𝑌  . . . . . . . . . .
315
11.3
Examples of Good and Bad Controls	. . . . . . . . . . . . . . . . .	316
	Pre-Treatment Variables or Proxies of Pre-Treatment Variables . . .
317
	Post-Treatment Variables  . . . . . . . . . . . . . . . . . . . . . . . .	322
11.4
Notebooks  . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . .	325
11.5
Exercises  . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . .	325
11.A
Front-Door Criterion via Example . . . . . . . . . . . . . . . . . . .	326
12
Unobserved Confounders, Instrumental Variables, and Proxy Controls
330
	12.1	The Difficulty of Causal Inference with an Unobserved Confounder
12.2	Impact of Confounders on Causal Effect Identification and Sensitivity
Analysis . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . . .
331
332
	12.3 Partially Linear IV Models  . . . . . . . . . . . . . . . . . . . . . . .	336
	A Wage Equation with Unobserved Ability . . . . . . . . . . . . . .	336
 
Aggregate Market Demand	338
Limits of Average Causal Effect Identification under Partial Linearity339
12.4	Nonlinear IV Models	342
The LATE Model	342
The IV Quantile Model★	344
12.5	Partially Linear SEMs with Griliches-Chamberlain Proxy Controls .  345
12.6	Nonlinear Models with Proxy Controls★	347
12.7	Notebooks	349
12.8	Exercises	349
12.A Proofs	351
Latent Confounder Bias Result: Theorem 12.2.1	351
Partially Linear Outcome IV Model: Theorem 12.3.2	352
Partially Linear Compliance IV Model: Theorem 12.3.3	352
Linear Proxy Model: Theorem 12.5.1	353
13	DML for IV and Proxy Controls Models and Robust DML Inference under Weak Identification	359
13.1	DML Inference in Partially Linear IV Models	360
The Effect of Institutions on Economic Growth	362
13.2	DML Inference in the Interactive IV Regression Model (IRM)	365
DML Inference on LATE	365
The Effect of 401(k) Participation on Net Financial Assets	366
13.3	DML Inference with Weak Instruments	368
Motivation	368
DML Inference Robust to Weak-IV in PLMs	370
The Effect of Institutions on Economic Growth Revisited	372
13.4	Generic DML Inference under Weak Identification	373
13.5	Notebooks	375
13.6	Exercises	376
14	Statistical Inference on Heterogeneous Treatment Effects	379
14.1	CATEs under Conditional Exogeneity	380
14.2	Inference on Best Linear Approximations	383
Least Squares Methods for Learning CATEs	384
Application to 401(k) Example	386
14.3	Non-Parametric Inference for CATEs with Causal Forests	388
Empirical Example: The "Welfare" Experiment	395
14.4	Notebooks	397
15	Estimation and Validation of Heterogeneous Treatment Effects	400
15.1	ML Methods for CATE Estimation	401
Meta-Learning Strategies for CATE Estimation	401
Qualitative Comparison and Guidelines	411
Guarding for Covariate Shift	414
15.2	Scoring for CATE Model Selection and Ensembling	420
Comparing Models with Confidence	421
Competing with the Best Model	425
15.3	CATE Model Validation	431
Heterogeneity Test Based on Doubly Robust BLP	432
Validation Based on Calibration	433
Validation Based on Uplift Curves	437
15.4	Empirical Example: The "Welfare" Experiment	447
15.5	Empirical Example: Digital Advertising A/B Test	454
15.A	Appendix: Lower Bound on Variance in Model Comparison	457
15.B	Appendix: Interpretation of Uplift curves	458
16	Difference-in-Differences	463
16.1	Introduction	464
16.2	The Basic Difference-in-Differences Framework: Parallel Worlds	464
The Mariel Boatlift	468
16.3	DML and Conditional Difference-in-Differences	469
Comparison to Adding Regression Controls	471
16.4	Example: Minimum Wage	471
16.5	Notes	476
16.6	Notebooks	476
16.7	Exercises	477
16.A Conditional Difference-in-Differences with Repeated Cross-Sections 478
17	Regression Discontinuity Designs	482
17.1	Introduction	483
17.2	The Basic RDD Framework	484
Setting	484
Estimation	485
17.3	RDD with (Many) Covariates	486
Motivation for Using Covariates	486
Low-Dimensional Covariates	487
High-Dimensional Covariates	488
Heterogeneous Treatment Effects and Adjustments for Heterogeneity492
17.4	Empirical Example	493
17.5	Notes	494
17.6	Notebooks	495
17.7	Exercises	495
 


Notation
 

Contents	1
 


:=	assignment or definition equivalence
"maps to" in the definition of a function
𝑋, 𝑌, 𝑍, ...	random variables (includes vectors); for noise vectors, we use 𝜖, 𝜖𝑋, 𝜖𝑗, ...
𝑋′	transpose of a vector;
x	value of a random variable 𝑋
P	probability measure
𝑃𝑋	probability distribution or law of 𝑋
𝑃𝑌 𝑋	conditional law of 𝑌 given 𝑋
p	density (either probability mass function or probability density function)
p𝑋	density of 𝑃𝑋
p 𝑥	density of 𝑃𝑋 evaluated at the point 𝑥
p 𝑥 𝑑𝑥	integral with respect to the base measure
(Lebesgue for probability density and counting measure for pmf)
p(𝑦|𝑥)	(conditional) density of 𝑃𝑌|𝑋=𝑥 evaluated at 𝑦
 
𝑧a
𝑛
 
the a−quantile of the standard normal distribution
 
𝑋𝑖 𝑖=1	= 𝑋1, 𝑋2, ..., 𝑋𝑛
typically an iid. sample of size 𝑛 with distribution 𝑃𝑋
𝑋𝑗1, ..., 𝑋𝑗𝑛 when referring to 𝑗𝑡ℎ component of 𝑋
E[𝑋]	expectation of 𝑋
E[𝑌|𝑋]	conditional expectation of 𝑌 given 𝑋
𝔼𝑛[ 𝑓 (𝑌, 𝑋)]	empirical expectation (e.g. 𝔼𝑛[ 𝑓 (𝑌, 𝑋)] := 1 '5:𝑛	𝑓 (𝑌𝑖 , 𝑋𝑖)
 
Var(𝑋)	variance of 𝑋
𝕍𝑛[𝑔(𝑊)]	empirical variance
 

 
𝑛	𝑖=1
 
Cov
 
(e.g. 𝕍𝑛[𝑔(𝑊)] = 𝔼𝑛[𝑔(𝑊)𝑔(𝑊)′] − 𝔼𝑛[𝑔(𝑊)]𝔼𝑛[𝑔(𝑊)]′)
 
𝑋, 𝑌	covariance of 𝑋, 𝑌
𝑋	𝑌	orthogonality of 𝑋, 𝑌, i.e. E 𝑋𝑌′ = 0
𝑋	𝑌	independence of 𝑋, 𝑌
𝑋	𝑌	𝑍	conditional independence of 𝑋, 𝑌 given 𝑍
𝑃𝑌 𝑥 = 𝑃𝑌:𝑑𝑜 𝑋=𝑥	intervention distribution
𝑃𝑌 𝑥 𝑋 = 𝑃𝑌 𝑋: 𝑓 𝑖𝑥𝑌 𝑋=𝑥	counterfactual distribution
G	directed graph
paG 𝑋 , deG 𝑋  , anG 𝑋	parents, descendants, and ancestors of node 𝑋
in graph G
ℝ𝑝	the 𝑝-dimensional Euclidean space
∥𝑥∥1 := '5:𝑝	|𝑥𝑗 |	the ℓ1-norm in ℝ𝑝
∥𝑥∥ ≡ ∥𝑥∥2 := J'5:𝑝	2	the ℓ2-norm in ℝ𝑝
∥𝑥∥∞ := max𝑗=1 |𝑥𝑗 |	the ℓ∞-norm in ℝ𝑝
∥𝐴∥ := sup𝑥∈ℝ𝑝 \0 𝑥′𝐴𝑥	the operator norm (maximum eigenvalue) of a matrix 𝐴
 


Preface



This book aims to provide a working introduction to the emerg-ing fusion of modern statistical inference – aka machine learning (ML) or artificial intelligence (AI) – and causal inference meth-ods. The book is aimed at upper level undergraduates and master’s-level students as well as doctoral students focusing on applied empirical research. A sufficient background for the core material is one semester of introductory econometrics and one semester of machine learning. We hope the book is also useful to empirical researchers looking to apply modern methods in their work.
The book provides an overview of key ideas in both predictive inference and causal inference and shows how predictive tools are key ingredients to answering many causal questions. We use the term predictive inference to refer to settings where prediction or description is the main goal such that models and estimates do not need a causal interpretation. ML/AI tools are largely designed to answer predictive inference questions, and we provide a high-level overview of popular ML/AI methods (such as Lasso, random forests, and deep neural networks, among others) to provide background for readers less familiar with these methods.
On the causal inference side, we introduce foundational ideas that provide the underpinning to attaching causal interpre-tations to statistical estimates. We discuss these ideas using the language of potential outcomes, directed acyclical graphs (DAGs), and structural causal models (SCMs). We view the language of potential outcomes, DAGs, and SCMs as com-plementary. We recognize that readers coming from different backgrounds may be more familiar or disposed to one of po-tential outcomes, DAGs, or SCMs, but we strongly believe that individuals interested in causal inference should be familiar with each of these frameworks. We find that they all offer useful insights and being able to communicate using each framework allows one to communicate with audiences interested in under-standing causality coming from many different backgrounds.
The book has two main sections: Core Material and Advanced Topics. The Core Material provides the main content of the book. After concluding the Core Material, a reader should have an idea of the key ideas underlying both predictive and causal
 
Contents	3


inference and how to wed these ideas to learn canonical objects in causal inference settings. The Core Material is made up of chapters that move between predictive inference and causal inference, typically by first introducing tools developed for predictive inference and then showing how these tools can be used as inputs to answering causal inference questions. The Advanced Topics then provide extensions of the Core Material to settings with more complicated causal structures, such as instrumental variables models, to settings where understanding heterogeneity in causal effects is the goal, and to specific popular settings in empirical work such as Difference-in-Differences.
Within sections, blocks marked with ★ require more substantial preparation in mathematical statistics. We recommend that the reader looking to apply machine learning methods in their work skim or pass them on their first reading and return to them at their leisure.
Short lists of references and study problems are included after each chapter to offer the reader opportunities to investigate further and consolidate their knowledge.
We would like to also acknowledge the tremendous and excep-tional help and expertise provided by Philipp Bach, Wenxuan Guo, Andy Haupt, Shunzhuang Huang, David Hughes, Jan-nis Kück, Malte Kurz, Sven Klassen, Oliver Schacht, Sophie Sun, Vira Semenova, Gulin Tuzcuoglu, Suhas Vĳaykumar, John Walker, Thomas Wiemann, Justin Young, and Dake Zhang with both writing and developing supporting Notebooks in R and Python. We are also grateful to Alexander Quispe and Anzony Quispe for developing a Bookdown version of the note-books and providing other complementary topics and great examples.

Chernozhukov, Hansen, Kallus, Spindler & Syrgkanis
 



Sneak Peek: Powering Causal Inference
with ML and AI



 
A primary question we will be concerned with in this book is: What is the causal effect of an action on an outcome? For example, we may want to know what the effect of setting a product’s price is on the volume of its sales.1 To consider this question we scraped data on toy cars from Amazon.com. Figure 0.1 shows a log–log scatter plot of the 30-day average price at which each car was offered and the reciprocal of its sales rank, a publicly available surrogate for sales volume.2 We let 𝐷 denote the log of the price and 𝑌 the negative log of the sales rank of a toy car randomly drawn from the population of toy cars sold on Amazon.com. We will use this running example to preview how the book’s pieces interlock, and why modern ML and AI turn causal inference from a fragile craft into a disciplined toolkit.
 




1: This effect may be referred to as the price elasticity of demand for the product.

2: Were the reader to do such an analysis using internal company data they would use actual sales volumes.
 









 




If we throw ordinary least squares at Figure 0.1—the way we do in Chapter 1—it whispers something unsettling: the fitted slope is so close to zero that one cannot even rule out a slightly positive relationship between price and sales. That is a classic false lead. If we let ourselves believe it, we would conclude that raising the price of a toy car barely changes its sales volume, or even increases it. Yet basic economic logic insists that the unobserved potential log-sales 𝑌 𝑑 of a given product should decrease when we raise its log-price 𝑑.
The tension here is the tension that animates the whole book: prediction is easy to validate, but causality is treacherous. Prices are not assigned by a benevolent randomized experimenter; they are chosen in the market, and those choices are entangled with latent forces that also shape demand. Branding, licensing, visibility, product quality, and a hundred other factors move
 
Figure 0.1: Log-prices and log-reciprocal-sales-rank of toy cars on Amazon.com along with a linear fit.
 


both the price tag and the sales rank. When those factors are left in the shadows, naive regression makes the wrong suspect look innocent.
To bring the right suspects into the light, we build the causal vocabulary first. In Chapter ?? we introduce potential outcomes and the logic of randomized variation; in Chapter 5 we formalize confounding and what it means to “control” for it; in Chapter 6 we step into structural equations so that the word intervention stops being a metaphor; and in Chapter 7 we widen the frame to systems and nonlinear structure. In a simple linear structural equation,
𝑌(𝑑) = 𝛼𝑑 + 𝑈,	(0.0.1)
the parameter 𝛼 is the causal effect: the change in 𝑌 produced by changing 𝑑 while holding everything else fixed. That effect is generally not recovered from regressing the observed outcome on the observed treatment, because the observed price is se-lected by the market and is plausibly related to the unobserved idiosyncrasy 𝑈.
The key modeling move is to articulate what would make 𝛼 learnable. If we observe a set of confounders 𝑊 rich enough to account for all confounding, then the observed data obey a partially linear regression,
𝑌 = 𝛼𝐷 + 𝑔(𝑊) + 𝜀,	𝔼[𝜀 | 𝐷, 𝑊] = 0,	(0.0.2)
for some (possibly complicated) function 𝑔. At that point, the causal problem becomes an inferential problem: we must esti-mate 𝛼 reliably even when 𝑔 is high-dimensional, nonlinear, and learned by machine learning.
This is where the first twist arrives. If 𝑊 is modest and the model is linear, classical tools work. But modern datasets rarely show mercy: 𝑊 can be thousands of features, and the best predictors are nonlinear. In Chapter 3 we introduce regularization that stabilizes prediction in high dimensions; in Chapter 4 we show how to repair the bias that regularization injects into coefficients; in Chapter 8 we unleash nonlinear learners—trees, ensembles, neural nets—that predict superbly but refuse to hand us an interpretable slope; and in Chapter 9 we resolve the impasse by developing orthogonalization and double machine learning (DML). The idea is to learn the nuisance parts well enough to remove them, and to do it in a way that preserves valid inference for the low-dimensional causal target.
Now the plot thickens again. The scatter plot in Figure 0.1 is
 


 
a single snapshot; the market, of course, is a moving picture. In the demand-analysis study that accompanies this chapter, we follow 𝑁 = 7,226 toy-car products over 𝑇 = 12 adjacent
four-week periods from March 2023 to January 2024, tracking
prices and sales ranks over time.3 Once you watch the series unfold, the data reveal a signature pattern: prices move in sticky steps, while sales ranks jump and surge like a seismograph. Figure 0.2 shows two examples.


This temporal structure changes what “confounding” looks like. A product’s past sales are not just a lagged outcome; they are a proxy for visibility and latent quality, and they can drive both future prices and future demand. The demand study therefore uses a simple dynamic causal model in which the relevant state includes lagged quantity and price along with product characteristics. Conditioning on that state turns the causal problem back into a regression problem, and DML again provides the inferential engine.
If the market is the crime scene, then the product page is the dossier. Every toy car comes with rich evidence: text descrip-tions, images, and tabular facts. Figure 0.3 shows one such page.
 





3: The empirical study uses the log inverse sales rank as a proxy for log quantity and the log price as a price signal.













Figure 0.2: Price and sales-rank series for two example products. Prices typically evolve in piecewise-constant steps, while ranks react quickly.
 




 




The catch is that none of this evidence arrives in the neat columns that regression likes. We therefore will teach the practical art
 
Figure 0.3: A typical product “dossier” consists of an image, a text description, and structured at-tributes.
 


of turning rich modalities into numerical representations that a causal pipeline can use without becoming a house of cards. We start from transformer encoders: models trained by self-supervision to read text, and vision transformers trained to see images. We then show how these encoders can be connected to downstream prediction heads, and how fine-tuning can align the representation with the nuisance regressions that DML needs. In the demand study, this means fine-tuning multimodal embeddings to predict both price and quantity signals, because those predictions are exactly what we must partial out to isolate price shocks.
When we project the resulting embeddings into low dimensions, they organize products into coherent neighborhoods: clusters that separate by visual style, branding cues, and latent “type.” Figure 0.4 gives a glimpse. The point is not the picture itself; the point is that the representation has learned enough structure that it can serve as a control, an effect modifier, or both.















 






At this stage, the big win from embeddings is often expected to be “better confounding control.” The demand study delivers a subtler and more exciting twist: embeddings matter most as effect modifiers. Average price elasticity is informative, but it is also a blunt instrument; some products are almost immune to small price changes, while others are exquisitely sensitive. Once we let elasticity vary with product characteristics, the model uncovers a wide spread of price sensitivities that is not
 
Figure 0.4: A 3D projection of multimodal product embeddings (text+image), colored by a simple clustering. Even in three dimen-sions, the representation separates latent product “types” that matter for demand.
 


 
statistical noise but systematic, predictable structure.
Figure 0.5 shows the estimated elasticity function sorted across products, together with confidence bands. The line climbs from strongly negative effects to near zero, and in some specifications even positive values for the least price-sensitive products, while the average effect sits in the middle like a red dashed “alibi.” This is the moment when the case file turns into a story about who responds to price, not merely whether price matters.4



This book is built around that kind of turn: we begin with simple estimands, then show how the world tries to fool them, and then show how to defend them without giving up flexibility. The “BERT chapter”—Chapter 10—is therefore not about mem-orizing a brand name; it is about learning the encoder–decoder logic that turns text, images, and other unstructured data into usable features, and then learning how to plug those features into orthogonal causal estimators. Along the way we connect BERT-style masked-language modeling to modern transformer encoders, connect vision transformers to image embeddings, and show how fine-tuning for prediction can be made compati-ble with valid inference for causal targets.
If you want the story to keep escalating after that, the Ad-vanced Topics do exactly that. In Chapter 12 and Chapter 13 we confront the cases where we do not believe we have mea-sured all confounders, and we develop tools that exploit extra structure—sensitivity analysis, instruments, proxy controls—without surrendering the ability to use rich AI features. In Chapter 14 and Chapter 15 we move from “the average effect” to heterogeneous effects and personalization, and in Chap-ter 16 and Chapter 17 we show how DML can be fused with difference-in-differences and regression discontinuity designs. Finally, in Chapter 7 and Chapter 11 we explain why directed acyclic graphs are the right map for all of these journeys: they tell you, before you estimate anything, which arrows you are betting your credibility on.
 









4: The full story with many more background information can be looked up in the paper “Adventures in Demand Analysis Using AI” by Bach et. al. (2025).




Figure 0.5: Sorted elasticity esti-mates as a function of effect mod-ifiers, with pointwise confidence bands, under several specifications.
 


We close the preview by returning to the guiding promise. It is impossible to definitively validate a causal claim from observa-tional data alone, because identification rests on assumptions. But we can make those assumptions explicit, we can use the richest available information to make them more plausible, and we can avoid needless functional-form commitments. DML on top of modern encoders and decoders gives us a rare combi-nation: the power of AI representations and the discipline of inferential guarantees. If you read the book, you will not only learn how to fit models that look impressive, but how to extract conclusions that can survive cross-examination.
 





Core Material
 

 
Predictive Inference with Linear Regression in Moderately High
Dimensions


"Infer: to form an opinion or guess that something is true because of the information that you have."
– Cambridge Dictionary [1].










Least squares—particularly in the form of linear regression—is among the most widely used and intuitive statistical methods, useful for both predictive inference and identifying associations. Introduced in the early 1800s by L. Legendre and C. F. Gauss, it has become a foundational tool in data analysis. Here, we review the properties of least squares estimation for linear models in moderately high-dimensional settings, focusing on its utility in prediction and association analysis. This discussion sets the stage for our subsequent review of modern statistical (machine) learning approaches, which relax dimensionality assumptions and incorporate nonlinear models.
 
1
1.1	Introduction	12
1.2	Foundation of Linear Regres-sion	13
Regression and the Best Lin-ear Prediction Problem	13
Best Linear Approximation Property	14
From Best Linear Predictor to Best Predictor	15
1.3	Statistical Properties of Least Squares	17
The Best Linear Prediction Problem in Finite Samples	17
Properties of Sample Linear Regression	18
Analysis of Variance	20
Overfitting: What Happens When 𝑝 𝑛 Is Not Small	21
Measuring Predictive Abil-ity by Sample Splitting	22
1.4	Inference about Predictive Effects or Association	23
Understanding 𝛽1  via "Partialling-Out"	24
Adaptive Statistical Infer-ence	26
1.5	Application: Wage Predic-tion and Gaps	28
Prediction of Wages	29
Wage Gap	32
1.6	Inference on Predictive Ef-fect when 𝑝 𝑛 < 1 is not small★	34
1.7	Notes	36
1.8	Notebooks	36
1.9	Checklist	37
1.10	Exercises	37
1.A Central Limit Theorem★	38
Univariate	38
Multivariate	39
 

1.1	Introduction
Linear regression enters whenever we observe an outcome 𝑌
together with covariates 𝑋 and want a simple rule for predicting
𝑌 from 𝑋. The predictive goal is straightforward to state: we would like a function of 𝑋 that produces accurate forecasts of 𝑌 for new observations drawn from the same population. A second goal is equally common and only slightly more subtle: we often want a low-dimensional summary of how the prediction changes when one component of 𝑋 changes and the others are held fixed. Writing 𝑋 = 𝐷, 𝑊′ ′, this amounts to
understanding how the predicted value of 𝑌 changes when 𝐷
increases by one unit while 𝑊 is kept the same. In this chapter we treat this as a predictive effect (or association parameter): it describes a property of the best linear prediction rule, not, by itself, a causal effect.
Both goals become delicate when the model is flexible relative to the data. With many regressors, it is easy to improve in-sample fit by adding polynomials, interactions, and indicators, yet lose predictive accuracy on new data. At the same time, when 𝐷 is embedded in a high-dimensional regression, the meaning of “holding 𝑊 fixed” can feel implicit. The partialling-out view-point resolves this: we make “holding 𝑊 fixed” literal by first removing from both 𝑌 and 𝐷 the part that is linearly predictable from 𝑊, and then relating the resulting residualized variables. This residual-on-residual representation is algebraically equiva-lent to the original multiple regression (Frisch–Waugh–Lovell), but it makes the target coefficient transparent. It also previews a theme that will recur later: the residualized moment condition that identifies the coefficient is orthogonal to nuisance features, a structure formalized as Neyman orthogonality and exploited by debiased or Double Machine Learning (DML) methods.
This chapter has three jobs. First, we develop linear regression as an object in the population—the best linear predictor (BLP)—and then show how ordinary least squares (OLS) estimates it from a finite sample. Second, we explain why the accuracy of prediction depends sharply on the ratio 𝑝 𝑛, and why in-sample fit can become misleading when the number of regressors 𝑝 is not small relative to the sample size 𝑛. Third, we study inference on a single coefficient of interest through partialling-out: by residualizing both the outcome and the regressor of interest with respect to controls, we obtain a simple “residual-on-residual” regression that is algebraically identical to the original multiple regression. This partialling-out viewpoint is more than interpretation; it is the blueprint that later becomes Neyman
 


orthogonality, the key structural property behind debiased or Double Machine Learning (DML) procedures.
To keep notation light, we will repeatedly use a small set of abbreviations: MSE for mean squared error, 𝑅2 for the explained-variance ratio, BLP for best linear predictor, and OLS for ordi-nary least squares.

 
1.2	Foundation of Linear Regression
Regression and the Best Linear Prediction Problem
We consider a scalar random variable 𝑌, an outcome of interest, and a 𝑝-vector of covariates
𝑋 = (𝑋1, . . . , 𝑋𝑝)′.
We assume that a constant of 1 is included as the first component in 𝑋; that is, 𝑋1 = 1.
For theoretical purposes, we first consider linear regression in the population. Working in the population means that we have access to unlimited amounts of data to compute population moments – such as E 𝑌 , E 𝑌𝑋 , and E 𝑋𝑋′ – and that we can define "ideal" quantities. After defining these ideal quantities, we then turn to estimation with real data, which we will take to be a sample of observations drawn from the population.
Our first goal is to construct the best linear prediction rule for
𝑌 using 𝑋. That is, the predicted value of 𝑌 given 𝑋 will be of the linear form:
𝑝
𝛽𝑗 𝑋𝑗 = 𝛽′𝑋, for 𝛽 = 𝛽1, ..., 𝛽𝑝 ′,
𝑗=1
where 𝛽’s are called the regression parameters or coefficients.
We define 𝛽 as any solution to the Best Linear Prediction (BLP) Problem,
 





Figure 1.1: The only known portrait of Legendre (a friendly caricature) by Julien Léopold Boilly. Source: Wikipedia. The hairstyle is amaz-ing.
 
min E r(𝑌 − 𝑏′𝑋 2l ,
 
where the choice variable is the coefficient vector 𝑏	ℝ𝑝, and
we minimize the Expected or Mean Squared Error (MSE) for predicting 𝑌 using the linear rule
𝑝
 
of a more realistic portrait of Leg-
endre based on Boilly’s caricature. Source: generated by authors using ChatGPT-4, who noted that “this image captures the intense and ex-
 
𝑏′𝑋 = I: 𝑏 𝑋 ,	𝑏 = (𝑏 , ..., 𝑏 )′.
 
pressive nature that was suggested
 


The solution to this optimization problem, 𝛽′𝑋, is called the Best Linear Predictor (BLP) of 𝑌 using 𝑋. This jargon refers to the fact that 𝛽′𝑋 is the best, according to MSE, linear prediction rule for 𝑌 among all possible linear prediction rules.
We can compute 𝛽 by solving the first order conditions for the BLP problem:
 
E [(𝑌 − 𝛽′𝑋)𝑋] = 0.
These equations are also referred to as the Normal Equations and are obtained by setting the derivative of the objective function 𝑏  E 𝑌 𝑏′𝑋 2 with respect to 𝑏 equal to zero. Thus, any solution to the BLP problems satisfies the Normal Equations.
Defining the regression error or residual as
𝜀 := (𝑌 − 𝛽′𝑋),
we can write the Normal Equations as1
E [𝜀𝑋] = 0,	or equivalently	𝜀 ⊥ 𝑋.
Therefore, the BLP problem provides a simple decomposition of 𝑌:
𝑌 = 𝛽′𝑋 + 𝜀,	𝜀 ⊥ 𝑋,
where 𝛽′𝑋 is the part of 𝑌 that can be linearly predicted or explained with 𝑋, and 𝜀 is whatever remains – the so-called unexplained or residual part of 𝑌. It is worth keeping in mind that 𝜀 is defined relative to linear prediction: it need not corre-spond to a structural shock or measurement error; it is simply the component of 𝑌 that is orthogonal to 𝑋.

Best Linear Approximation Property
The normal equation E 𝑌 𝛽′𝑋 𝑋 = 0 implies by the law of iterated expectations that
E [(E[𝑌 | 𝑋] − 𝛽′𝑋)𝑋] = 0.
Therefore, the BLP of 𝑌 is also the BLP for the conditional expectation of 𝑌 given 𝑋. This observation is important and motivates the use of various transformations of regressors to form 𝑋.
 













1: Note that we use to denote or-thogonality between random vari-ables, and to denote full statisti-cal independence. That is, for ran-dom variables 𝑈 and 𝑉, 𝑈 𝑉 means E 𝑈𝑉 = 0. Further, if 𝑈 is
a centered random variable, then 𝑈
𝑉 implies 𝑈 𝑉, but the reverse implication is not true in general. Uncorrelated does not mean inde-pendent; here is a classic counterex-ample. Indeed, let 𝑈 ∼ 𝑁(0, 1) and
𝑉 = 𝑈2 − 1, then 𝑈 ⊥ 𝑉 but
𝑈 ⊥ 𝑉.
 

 
From Best Linear Predictor to Best Predictor
Here we explain the use of constructed features or regressors. If
𝑊 are "raw" regressors/features, technical (constructed) regressors
are of the form
𝑋 = 𝑇(𝑊) = (𝑇1(𝑊), ..., 𝑇𝑝(𝑊))′,
where the set of transformations 𝑇 𝑊 is sometimes called the dictionary of transformations. Example transformations include polynomials, interactions between variables, and applying func-tions such as the logarithm or exponential. In the wage analysis reported below, for example, we use quadratic and cubic trans-formations of experience, as well as interactions (products) of these regressors with education and geographic indicators.
The main motivation for the use of constructed regressors is to build more flexible and potentially better prediction rules. The potential for improved prediction arises because we are using
prediction rules 𝛽′𝑋 = 𝛽′𝑇 𝑊 that are nonlinear in the original raw regressors 𝑊 and may thus capture more complex patterns that exist in the data. Conveniently, the prediction rule 𝛽′𝑋 is still linear with respect to the parameters, 𝛽, and with respect to the constructed regressors 𝑋 = 𝑇(𝑊).
In the population, the best predictor of 𝑌 given 𝑊 is
𝑔(𝑊) = E[𝑌 | 𝑊],
the conditional expectation of 𝑌 given 𝑊. The conditional expec-tation function 𝑔 𝑊 is also called the regression function of 𝑌 on 𝑊. Specifically, the conditional expectation function 𝑔 𝑊 solves the best prediction problem2
min E [(𝑌 − 𝑚(𝑊))2 ] .
 











































2: This result follows by rewriting the objective function as
min E E	2
 
Here we minimize the MSE among all prediction rules 𝑚 𝑊
(linear or nonlinear in 𝑊).
As the conditional expectation solves the same problem as the best linear prediction rule among a larger class of candidate rules, the conditional expectation generally provides better predictions than the best linear prediction rule.3
By using 𝛽′𝑇(𝑊), we are implicitly approximating the best predictor 𝑔(𝑊) = E[𝑌|𝑊]. Indeed, for any parameter 𝑏,
E [(𝑌 − 𝑏′𝑇(𝑊))2 ] = E [(𝑔(𝑊) − 𝑏′𝑇(𝑊))2 ] +E [(𝑌 − 𝑔(𝑊))2 ] ,
 

noting that it is equivalent to E[min E[(𝑌 − 𝜇)2 | 𝑊]],
and deriving the first order condi-tions for the inner minimization:
𝐸(𝑌 | 𝑊) − 𝜇 = 0.
3: Unless the conditional expecta-tion function turns out to be linear, in which case the conditional ex-pectation and best linear prediction rule coincide.
 


That is, the mean squared prediction error is equal to the mean squared approximation error of 𝑏′𝑇 𝑊 to 𝑔 𝑊 plus a constant that does not depend on 𝑏. Therefore, minimizing the mean squared prediction error is the same as minimizing the mean squared approximation error. Thus, the BLP 𝛽′𝑇 𝑊 is the Best Linear Approximation (BLA) to the best predictor, which is the regression function 𝑔 𝑊 . Finally, as the dictionary of transformations 𝑇 𝑊 becomes richer, the quality of the approximation of the BLA 𝛽′𝑇 𝑊 to the best predictor 𝑔 𝑊 improves.

 




p = 2	p = 3

50	50
40
40
30
30
20
20
10
0	10
 
10
0.0	0.2	0.4	0.6	0.8	1.0
W
 
0
0.0	0.2	0.4	0.6	0.8	1.0
W
 
p = 4

50

40
30
20
10
 
0
0.0	0.2	0.4	0.6	0.8	1.0
W
 

Figure 1.3: Refinements of Approx-imation to Regression Function
𝑔(𝑊) by using polynomials of 𝑊.
 

 
There are many ways of generating flexible approximations, which are studied by approximation theory and nonparametric statistical learning theory.4
When we have multiple variables, we may generate transforma-tions of each of the variables and employ interactions – products involving these terms. As a simple concrete example, consider a case with two raw regressors, 𝑊1 and 𝑊2. We could build polynomials of second order in each of the raw regressors –
 



4: See, e.g., Tsybakov [2]. We will also consider nonlinear approxima-tions using trees and neural net-works in Chapter 8.
 
2	2
1, 𝑊1, 𝑊1 , 1, 𝑊2, 𝑊2 . We may then collect these variables along with the interaction in the raw regressors, 𝑊1𝑊2 in a vector
2	2
(1, 𝑊1, 𝑊2, 𝑊1 , 𝑊2 , 𝑊1𝑊2)
for use in a regression model. There are, of course, many other possibilities such as considering higher order polynomial terms, e.g. 𝑊 3; higher order interactions, e.g. 𝑊 2𝑊2; and other
1	1
nonlinear transformations, e.g. log(𝑊1).

1.3	Statistical Properties of Least Squares
The Best Linear Prediction Problem in Finite Samples
In practice, the researcher does not have access to the entire population, but observes only a sample
{(𝑌𝑖 , 𝑋𝑖)}𝑖=1 = ((𝑌1, 𝑋1), ..., (𝑌𝑛 , 𝑋𝑛)).
 


 
We assume that this sample is a random sample from the distribution of 𝑌, 𝑋 . Formally, this condition means that the observations were obtained as realizations of independently and identically distributed (iid) copies of the random variable (𝑌, 𝑋).5
We construct the best in-sample linear prediction rule for 𝑌 using
𝑋 analogously to the population case by replacing theoretical expected values, E, with empirical averages (sample means),
𝔼𝑛.6 Specifically, given 𝑋, our predicted value of 𝑌 will be
𝑝
𝛽ˆ𝑗 𝑋𝑗 = 𝛽ˆ′𝑋 , for 𝛽ˆ = 𝛽ˆ1 , ..., 𝛽ˆ𝑝 ′,
𝑗=1
where 𝛽ˆ is any solution to the Best Linear Prediction Problem in the Sample, also known as Ordinary Least Squares (OLS):7
min 𝔼𝑛[(𝑌 − 𝑏′𝑋 2].
 






5: By treating the observations as iid, we are modeling the data as independent random draws with replacement from a population. Other possible models include sam-pling without replacement from a finite population, stratified sam-pling, observations of a process over time, and other schemes or sce-narios that induce dependence be-tween the data points. For the most part, we focus on the iid model throughout this book, as it typically conveys the key aspects of the in-ferential problem sufficiently well.
6: 𝔼𝑛  abbreviates the notation
 
1 '5:𝑛
 
. For example,
 
That is, 𝛽ˆ minimizes the sample MSE for predicting 𝑌 using
 
𝑛	𝑖=1
 
     I:


 
We can compute Equations,
 
𝛽ˆ
 
as any solution to the Sample Normal
𝔼𝑛[𝑋(𝑌 − 𝑋′𝛽ˆ)] = 0,
 
7: The hat notation ˆ is com-monly used to denote estimators—quantities derived from a sample. For instance, 𝛽 represents the best
 
which are obtained as the first order conditions to the Best Linear Prediction Problem in the Sample. Further, defining the residuals (or, in-sample regression errors) as
𝜀ˆ𝑖 := (𝑌𝑖 − 𝛽ˆ′𝑋𝑖),
we obtain the decomposition
𝑌𝑖 = 𝑋𝑖′𝛽ˆ + 𝜀ˆ𝑖 ,	𝔼𝑛[𝑋 𝜀ˆ] = 0,
where 𝑋𝑖′𝛽ˆ is the predicted or explained part of 𝑌𝑖, and 𝜀𝑖 is the unexplained or residual part.

Properties of Sample Linear Regression
The best linear prediction rule in the population is 𝛽′𝑋, and a key question is whether 𝛽ˆ′𝑋 estimates (that is, approximates using data) 𝛽′𝑋 well.
The best linear prediction rule is also the best linear rule for predicting future values of 𝑌 given a new draw 𝑋, when
 
linear predictor (BLP) in the popu-lation (the estimand), whereas 𝛽ˆ is the corresponding estimator com-puted from the sample.
 


new 𝑌, 𝑋 are sampled from the same population. Therefore, if we can approximate the best linear prediction rule in the population, we can also approximate the best linear prediction rule for predicting outcomes given future 𝑋’s sampled from the population.
The fundamental statistical issue is that we are trying to estimate
𝑝 parameters, 𝛽1, ..., 𝛽𝑝, without imposing any assumptions on these parameters. Intuitively, to estimate each parameter well, we need many observations per parameter. This intuition suggests that 𝑛 𝑝 should be large, or, equivalently that 𝑝 𝑛 should be small, in order for estimation error to be small. The following result captures this intuition more formally.
The following theorem bounds the root mean square approxi-mation error (RMSE) defined as:
JE𝑋[(𝛽′𝑋 − 𝛽ˆ′𝑋)2] = J(𝛽ˆ − 𝛽)′E𝑋[𝑋𝑋′](𝛽ˆ − 𝛽),
where E𝑋 is the expectation with respect to 𝑋 alone.


 
A useful way to read this bound is as a rule of thumb: the typical prediction error from estimating the best linear predictor scales
like the noise level √E𝜀2 multiplied by  𝑝/𝑛.
Theorem 1.3.1 says that, for nearly all realizations of data, the sample linear regression is close to the population linear regression if 𝑛 is large and 𝑝 is much smaller than 𝑛:8
 







8: Given indexed random variables (vectors, elements) 𝐴𝑛 and 𝐵𝑛 in a
 
JE𝑋
 
[(𝛽′𝑋 − 𝛽ˆ′𝑋)2] ≈ 0 whenever 𝑝/𝑛 ≈ 0.
 
metric space equipped with metric
𝑑, the notation 𝐴𝑛	𝐵𝑛 means that the distance between 𝐴𝑛 and 𝐵𝑛
 
In other words, under our requirement of 𝑝 𝑛 small, the sample
BLP approximates the population BLP well.
 
concentrates around 0 – formally, that lim𝑛	P 𝑑 𝐴𝑛 , 𝐵𝑛  𝜀 = 1 for each 𝜀 > 0.
 

Analysis of Variance
Analysis of variance involves the decomposition of the variation of 𝑌 into explained and unexplained parts. Explained variation is a measure of the predictive performance of a model. This decomposition can be conducted both in the population and in the sample.
The main idea is to use the previous decomposition of 𝑌,
𝑌 = 𝛽′𝑋 + 𝜀,	E[𝜀𝑋] = 0,
to decompose the variation in 𝑌 into the sum of explained variation and residual variation:
E[𝑌2] = E[(𝛽′𝑋)2] + E[𝜀2].

 
The quantity
 

MSE𝑝𝑜𝑝 = E[𝜀2]
 
is the population MSE. The ratio of the explained variation to the total variation is the population 𝑅2:
2	E[(𝛽′𝑋)2]	E[𝜀2]
𝑅𝑝𝑜𝑝 :=	E[𝑌2]	= 1 − E[𝑌2] ∈ [0, 1].
That is, 𝑅2	is the proportion of variation of 𝑌 explained by
the BLP.


 
The decomposition of the variance in the sample proceeds analogously. Using the representation
𝑌𝑖 = 𝛽ˆ′𝑋𝑖 + 𝜀ˆ𝑖
and the orthogonality condition 𝔼𝑛 𝑋 𝜀 = 0 provided by the sample Normal Equations, we obtain the decomposition
𝔼𝑛[𝑌2] = 𝔼𝑛[(𝛽ˆ′𝑋)2] + 𝔼𝑛[𝜀ˆ2].

Thus, we can define the sample MSE,
MSE𝑠𝑎𝑚𝑝𝑙𝑒 = 𝔼𝑛[𝜀ˆ ],
 













Figure 1.4: Pythagoras of Samos invented least squares and analysis of variance for the case of 𝑛 = 2
and 𝑝  2 around 570 BC. He was
therefore the first known machine learner.
 

 
and the sample 𝑅2,

2
 

𝔼𝑛[(𝛽ˆ′𝑋)2]	𝔼𝑛[𝜀ˆ2]
 
𝑅𝑠𝑎𝑚𝑝𝑙𝑒 :=
 
𝔼𝑛[𝑌2]	= 1 − 𝔼𝑛[𝑌2] ∈ [0, 1].
 

By the law of large numbers and Theorem 1.3.1, when 𝑝 𝑛 is small, we have the following approximations:
𝔼𝑛[𝑌2] ≈ E[𝑌2],  𝔼𝑛[(𝛽ˆ′𝑋)2] ≈ E[(𝛽′𝑋)2],  𝔼𝑛[𝜀ˆ2] ≈ E[𝜀2].

Thus, when 𝑝/𝑛 is small and 𝑛 is large, the sample fit measures are good approximations to population fit mea-sures:
MSE𝑠𝑎𝑚𝑝𝑙𝑒 ≈ MSE𝑝𝑜𝑝 and 𝑅2	≈ 𝑅2	.
𝑠𝑎𝑚𝑝𝑙𝑒	𝑝𝑜𝑝



Overfitting: What Happens When 𝑝/𝑛 Is Not Small
When 𝑝 𝑛 is not small, the picture about predictive perfor-mance of the in-sample BLP becomes inaccurate and possibly misleading. In this setting, the in-sample linear predictor can be substantially different from the population BLP.
Consider an extreme example where 𝑝 = 𝑛 and all variables in
𝑋 are linearly independent. In this case, we have

MSE𝑠𝑎𝑚𝑝𝑙𝑒 = 0 and 𝑅2	= 1
 
no matter what MSE𝑝𝑜𝑝 and 𝑅2
 
are. E.g. we could have
 
2
𝑠𝑎𝑚𝑝𝑙𝑒
 
= 1 even if 𝑅2
 
= 0. Therefore, here we have an ex-
 
treme example of overfitting, where the in-sample predictive performance overstates the out-of-sample predictive perfor-mance of the linear model. A perfect in-sample fit is therefore not evidence of good prediction; it can simply be a symptom of having too many degrees of freedom relative to the sample size. The following example illustrates less extreme cases.
 







The Notebooks 1.8.3 contain code for the numerical experiment.
 

 
▶ If 𝑝 = 𝑛/2, then the typical 𝑅2
▶ If 𝑝 = 𝑛/20, then the typical 𝑅2
 
is about .5 ≫ 0.
is about .05 > 0.
 
𝑠𝑎𝑚𝑝𝑙𝑒
These results can be deduced by simulation or analytically.
Provided 𝑝 < 𝑛, better measures of out-of-sample predictive ability are the "adjusted" 𝑅2 and MSE:9
 




9: The adjustment factor  𝑛
−𝑝
 





is de-
 
  𝑛		2	2
 
𝑛	𝔼𝑛[𝜀ˆ2]
 
rived in a homoskedastic model, so
that E[MSE𝑎𝑑𝑗𝑢𝑠𝑡𝑒𝑑] = MSE𝑝𝑜𝑝, see
 
MSE𝑎𝑑𝑗𝑢𝑠𝑡𝑒𝑑 = 𝑛 − 𝑝 𝔼𝑛[𝜀ˆ
 
],	𝑅𝑎𝑑𝑗𝑢𝑠𝑡𝑒𝑑 := 1 − 𝑛 − 𝑝 𝔼𝑛[𝑌2].
 
e.g., p. 8 in [3] for the derivation.
 
The adjustment by
 
𝑛
𝑛−𝑝
 
corrects for overfitting and provides
 
a more accurate assessment of predictive ability of the linear model in Example 1.3.1 and more generally under the assump-tion of homogeneous 𝜀. The intuition is that models with many parameters increase the in-sample fit and potentially cause overfitting. Hence, the number of parameters is incorporated
in the definition of MSE𝑎𝑑𝑗𝑢𝑠𝑡𝑒𝑑 and 𝑅2	in an attempt to
account for this phenomenon.

Measuring Predictive Ability by Sample Splitting
How should we measure the predictive ability of the linear model (or other nonlinear models that we will discuss) more reliably, even in cases when 𝑝/𝑛 is not small?

A general way to measure predictive performance is to perform data splitting. The idea can be summarized in two parts:
1.	Use a random part of a dataset, called the training sample, for estimating/training the prediction rule.
2.	Use the other part, called the testing sample, to eval-uate the quality of the prediction rule, recording out-of-sample mean squared error and 𝑅2.

 
Generally, a predictive model is trained on a sample and the real test of its predictive ability happens when "new, unseen" observations arrive. With new observations in hand, we learn how far off our predictions are, when compared to the realized values. By partitioning the data set into two parts, we preserve an "unseen" set of observations on which to test our model, mimicking this process of ex-post performance assessment.10
 






10: If the "test set" is used many times to evaluate models, it be-comes a "validation" set. The term "test set" is often reserved for the final evaluations of very few mod-els.
 


The data splitting procedure can be described more formally as follows:

Generic Evaluation of Prediction Rules by Sample-Splitting
1.	Randomly partition the data into training and test-ing samples. Suppose we use 𝑛train observations for training and 𝑛test for testing/validation.
2.	Use the training sample to compute a prediction rule
3.	𝑓ˆ(𝑋). For example, 𝑓ˆ(𝑋) = 𝛽ˆ′𝑋 in the linear model.t
Let Idenote the indexes of the observations in the tes
sample. Then the out-of-sample/test mean squared error is
MSE𝑡𝑒𝑠𝑡 =	1	I:(𝑌𝑘 − 𝑓ˆ(𝑋𝑘))2 ,
𝑛test 𝑘∈I
and the out-of-sample/test 𝑅2 is
  MSE𝑡𝑒𝑠𝑡	
𝑅2	= 1 −	'5:	.
𝑡𝑒𝑠𝑡	 1 	2
𝑛test	𝑘∈I 𝑌𝑘

 
In Section 3.B, we consider a more data-efficient evaluation procedure called cross-validation. In brief, we split the data into 𝐾 folds of about equal size. For each fold, we repeat the evaluation procedure by designating that fold as the "test" set and using the remaining folds for training. We then average the values of MSE𝑡𝑒𝑠𝑡 computed in each fold. Moreover, we can also average these results over different Monte Carlo seeds.
There is an important variation on the sample splitting pro-cedure, called stratified splitting that provides guarantees that the training and test samples are similar across key subgroups called "strata".11 In large samples, training and test samples will be similar across the subgroups by virtue of the laws of large numbers, but similarity is not guaranteed in moderate-sized samples. For more discussion, please see this blog on Data Splitting [4].

1.4		Inference about Predictive Effects or Association
Here we examine inference on predictive effects, which describe how our (population best linear) predictions change if the value
 













11: For example, we can make sure that the proportions of college-graduates and non-college-graduates are the same in both training and test samples. These issues are important in moderate-sized samples.
 


of a regressor changes by a unit, while the other regressors remain unchanged.
Specifically, we partition the vector of regressors 𝑋 into two components:
𝑋 = (𝐷, 𝑊′)′,
where 𝐷 represents the "target" regressor of interest, and 𝑊 represents the other regressors, sometimes called the controls. We can therefore write

 
𝑌 = 𝛽1𝐷 + 𝛽′2𝑊
predicted value

and ask the question: 12
 
𝜀
error
 
,	(1.4.1)
 




12: Note that this question is purely about the properties of the predic-
 
How does the predicted value of 𝑌 change if 𝐷
increases by a unit while 𝑊 remains unchanged?
The answer is the predicted value of 𝑌 changes by
𝛽1.
 
tion rule and generally has nothing to do with causality.
 

 

Understanding 𝛽1 via "Partialling-Out"
"Partialling-out" is an important tool that provides conceptual understanding of the regression coefficient 𝛽1.
In the population, we define the partialling-out operation as a procedure that takes a random variable 𝑉 and creates the
 

 
"residualized" variable 𝑉˜
linearly predicted by 𝑊:
 
by subtracting the part of 𝑉 that is
 
𝑉˜ = 𝑉 − 𝛾𝑉′ 𝑊 𝑊 ,	𝛾𝑉𝑊 ∈ arg min E [(𝑉 − 𝛾′𝑊)2 ] .

 
In words, 𝑉˜ is the residual from the population linear regression of 𝑉 on 𝑊: it is what remains in 𝑉 after subtracting its best linear prediction from 𝑊. We use a tilde to mark such residualized (partialled-out) variables.
When 𝑉 is a vector, we apply the operation to each component. It can be shown that the partialling-out operation is linear in the sense that13
𝑌 = 𝜈𝑉 + 𝜇𝑈 =⇒ 𝑌˜ = 𝜈𝑉˜ + 𝜇𝑈˜ .
Formally, this operation is well defined on the space of random variables with finite second moments.
We apply the partialling-out operation to both sides of our regression equation 𝑌 = 𝛽1𝐷 + 𝛽′2𝑊 + 𝜀 to get
 








13: Verify this as a reading exer-cise. Use the definition of the BLP decompositions of 𝑈 and 𝑉 with respect to regressors 𝑊, to derive a BLP decomposition of 𝑌 with re-spect to 𝑊.
 
𝑌˜ = 𝛽1𝐷˜
 
+ 𝛽′2𝑊˜
 
+ 𝜀˜,
 
which simplifies to the decomposition:
𝑌˜ = 𝛽1𝐷˜ + 𝜀,	E [𝜀𝐷˜ ] = 0.	(1.4.2)

Decomposition (1.4.2) follows because partialling-out eliminates
𝛽′2𝑊, since 𝑊˜ = 0, and leaves 𝜀 untouched, 𝜀˜ = 𝜀, since 𝜀 is
linearly unpredictable by 𝑋 and therefore by 𝑊. Equivalently, the best linear predictor of 𝜀 given 𝑊 is zero, so residualizing
𝜀 with respect to 𝑊 leaves it unchanged. Moreover, E[𝜀𝐷˜ ] = 0
since 𝐷˜ is a linear function of 𝑋 = (𝐷, 𝑊′)′ and 𝜀 is orthogonal
to 𝑋 and therefore to any linear function of 𝑋.
The decomposition (1.4.2) implies that E𝜀𝐷˜ = 0 is the Normal
Equation for the population regression of 𝑌˜ on 𝐷˜ . Therefore,
we just rediscovered the following result.

 

 
In other words, 𝛽1 can be interpreted as a (univariate) linear regression coefficient in the linear regression of residualized 𝑌 on residualized 𝐷, where the residuals14 are defined by partialling-out the linear effect of 𝑊 from 𝑌 and 𝐷.
When we work with the sample, we simply mimic the partialling-out operation in the population in the sample. In what follows, we assume 𝑝 𝑛 is small, so sample linear regression provides high-quality partialling-out. By the FWL Theorem applied to the sample instead of in the population, the sample linear
regression of 𝑌 on 𝐷 and 𝑊 gives us the estimator 𝛽ˆ1 which is
identical to the estimator obtained via sample partialling-out.
 



14: Technically, these are regres-sion errors, not residuals, as we are here working with the population, whereas residuals refer to errors to the sample regression fit. How-ever, we will not adhere strictly to this distinction as it will be conve-nient to apply analogous logic to partialling-out in the population and the sample.
 
It is useful to give the formula for partialling-out:
 
𝛽ˆ1 in terms of sample
 
𝛽ˆ1 = arg min 𝔼𝑛[(𝑌ˇ − 𝑏𝐷ˇ )2] = (𝔼𝑛[𝐷ˇ 2])−1𝔼𝑛[𝐷ˇ 𝑌ˇ ],	(1.4.3) where 𝑉ˇ𝑖 is the residual left after predicting 𝑉𝑖 with controls
𝑊𝑖 in the sample and we assume 𝔼𝑛[𝐷ˇ 2] > 0. That is,
𝑉ˇ𝑖 = 𝑉𝑖 − 𝛾ˆ𝑉′ 𝑊 𝑊𝑖 ,	𝛾ˆ𝑉𝑊 ∈ arg min 𝔼𝑛[(𝑉 − 𝛾′𝑊)2].

 
From Theorem 1.3.1, we know that using sample linear regres-sion for partialling-out will provide high-quality estimates of the residuals when 𝑝 𝑛 is small. When 𝑝 𝑛 is not small, using sample linear regression for partialling-out won’t be such a good idea and an alternative is to use penalized regression or dimension reduction. We will cover this in Chapter 3, but we can definitely try it out in the empirical example that concludes this chapter before we even attempt to understand it.15

Adaptive Statistical Inference
We next consider the large sample properties of the estimator
𝛽ˆ1.
 









15: Why not?
 
Because 𝑌ˇ and 𝐷ˇ are themselves computed using estimated
regressions on 𝑊, it is natural to worry that this first-stage estimation affects the distribution of 𝛽ˆ1. The next theorem shows that, when 𝑝/𝑛 is small, this effect is of higher order: to first order, 𝛽ˆ1 behaves as if we had used the population residuals
𝑌˜ and 𝐷˜ .

 











 
We can equivalently write
 
The notation 𝐴𝑛	a	𝑁(0, V)
 

𝛽ˆ1 a
ˆ
 

𝑁(
 

𝛽1, V/𝑛).
 
reads as 𝐴𝑛 is approxi∼mately dis-
tributed as 𝑁 0, V . Approximate distribution formally means that
sup𝑅∈R |P(𝐴𝑛 ∈ 𝑅) − P(𝑁(0, V) ∈
 
That is, 𝛽1 is approximately normally distributed with mean 𝛽1 and variance V 𝑛. Thus, 𝛽ˆ1 concentrates in a V 𝑛- neighbor-hood of 𝛽1 with deviations controlled by the normal law.
The first result in Theorem 1.4.2, equation (1.4.4), states the esti-mator minus the estimand is an approximate centered average. The remaining properties stated in the theorem then follow from the central limit theorem.
The adaptivity refers to the fact that estimation of residuals 𝐷ˇ has a negligible impact on the large sample behavior of the OLS estimator – the approximate behavior is the same as if we had
 
𝑅   0, where R is the collection of rectangular sets (intervals for the case of 𝐴𝑛 being a scalar random variable).
 
used true residuals 𝐷˜ instead. This adaptivity property will be
 
derived later as a consequence of a more general phenomenon which we shall call Neyman orthogonality.16

The estimated standard error of 𝛽ˆ1 is	Vˆ/𝑛, where Vˆ is any estimator of V based on the plug-in principle such that Vˆ	𝑉.
The standard estimator for independent data is called the Eicker-Huber-White robust variance estimator ([5], [6],[7], [8]):
Vˆ = (𝔼𝑛[𝐷ˇ 2])−1𝔼𝑛[𝐷ˇ 2 𝜀ˆ2](𝔼𝑛[𝐷ˇ 2])−1.
This standard error estimator formally works when 𝑝/𝑛 ≈ 0, but fails in settings where 𝑝/𝑛 is not small; see, e.g., [9].
Consider the set, called the (1 − a)% confidence interval, 17
[𝑙ˆ, 𝑢ˆ] := J𝛽ˆ1 − 𝑧1 a 2JVˆ/𝑛, 𝛽ˆ1 + 𝑧1 a 2JVˆ/𝑛l ,
 

16: We’ll defer the formal defini-tion of Neyman orthogonality for a bit. See Section 4.3.









17: An  alternative  is  to  use
𝑛 𝔼 𝐷2 −1𝔼 𝜀2 instead of 𝑉, w−hich is a “classical” choice. How-
ever, this choice is not robust to
 

 
where 𝑧1−a/2 denotes the (1 − a/2)−quantile of the standard normal distribution. We say that a (1 − a) × 100% confidence interval contains the true value 𝛽1 “(1 − a) × 100% of the time,”
 
variance of 𝜀 depending on 𝑋 – and is therefore not valid in general. We never use the “classical” choice in this book, even though it is still a default reporting option in some statistical software.
 

approximately. For example, the 95% confidence interval is given by
J𝛽ˆ1 − 1.96JVˆ/𝑛, 𝛽ˆ1 + 1.96JVˆ/𝑛l ,
and contains 𝛽1 approximately 95% of the time.


1.5	Application: Wage Prediction and Gaps
In labor economics, an important question is what determines the wage of workers. Interest in this question goes back at least to the work of Jacob Mincer (see [10]). While determining the factors that lead to a worker’s wage is a causal question, we can begin to investigate it from a predictive perspective. We aim to answer two main questions:
▶ The Prediction Question: How can we use job-relevant characteristics, such as education and experience, to best predict wages?
▶ The Predictive Effect or Association Question: What is the difference in predicted wages between male and female workers with the same job-relevant characteristics?
We illustrate using data from the 2015 March Supplement of the U.S. Current Population Survey (CPS 2015). As outcome,
 

 
𝑌, we use the log hourly wage, and we let 𝑋 denote various characteristics of workers, including sex.
We focus on a (sub) sample of single (never married) workers, which is of size 𝑛 = 5, 150. Table 1.1 provides mean characteris-tics of some key variables.


Log Wage	2.97
Female	0.44
Some High School	0.02
High School Graduate	0.24
Some College	0.28
College Graduate	0.32
Advanced Degree	0.14
Experience	13.76


We will estimate a linear predictive (regression) model for log hourly wage using job-relevant characteristics
𝑌 = 𝛽′𝑋 + 𝜀,	𝜀 ⊥ 𝑋,
and assess the quality of the empirical prediction rule 𝛽ˆ′𝑋 using out-of-sample prediction performance.
We will also analyze if there is a gap (difference) in pay for male and female workers.18 Any such gap may partly reflect discrimination in the labor market. We will discuss the potential to learn about discrimination as a causal mechanism in more detail in Chapter 6.

Prediction of Wages
Our goal here is to predict (log) wages using various character-istics of workers, and assess the predictive performance of two linear models using adjusted MSE and 𝑅2 and out-of-sample MSE and 𝑅2.
We start with two different specifications of covariates to use as predictors: 19
▶ In the Basic Model 𝑋 consists of a set of raw regressors (e.g. sex, experience, education indicators, occupation and industry indicators, and regional indicators), for a total of
𝑝 = 51 regressors. Our basic specification is inspired by the famous Mincer equation from labor economics; see,
 








Table 1.1: Descriptive statistics for sample of never married workers.





















18: More precisely, we will analyze the sex pay gap using data from the CPS 2015, where sex is recorded as binary and self-reported vari-able. In labor economics, such anal-yses are often referred to as "gender wage gap" studies; see, for exam-ple, the Nobel lecture by Claudia Goldin [11] for an overview of this area of research.






19: The Notebooks 1.8.1 contain the predictive exercise for wages.
 


e.g., [10] for a review.

▶ In the Flexible Model, 𝑋 consists of all raw regressors from the basic model as well as technical regressors, which are transformations of the raw regressors, namely, poly-nomials in experience (experience2, experience3, and experience4) and additional two-way interactions of the polynomials in experience with all other raw regressors except for sex. These interactions allow the relationship between experience and wages to vary across education, occupation, industry, and region. An example of a regres-sor created through a two-way interaction is experience times the indicator of having a college degree. In total, we have 𝑝 = 246 regressors.

Table 1.2: Assessment of predic-tive performance with in-sample
𝑅2 and 𝑀𝑆𝐸.




To enable both in- and out-of-sample performance evaluation. We start by randomly selecting 80% of the observations as the training sample and keep the other 20% for use as a test sample.
Table 1.2 shows measures of predictive performance in the train-ing data. That is, the table reports predictive performance on the same data that were used to estimate the model parameters. The flexible regression model performs slightly better than the
basic model (higher 𝑅2	and lower 𝑀𝑆𝐸𝑎𝑑𝑗). Note also that the
 
discrepancy between the unadjusted and adjusted measures is not large, which is expected given that
𝑝/𝑛 is small.

We report results for evaluating the prediction rules in the test data in Table 1.3. That is, the table reports predictive perfor-mance on new data that were not used to estimate the models.
 










Table 1.3: Assessment of predictive performance on a 20% validation sample.
 

 
Based on this exercise, it appears that the basic regression model works slightly better than the flexible regression at predicting log wages for new observations. That is, we see that the test (out-of-sample) 𝑀𝑆𝐸 and 𝑅2 for the basic regression model are respectively slightly lower and higher than those of the flexible regression model, indicating slightly superior out-of-sample predictive performance. This behavior is different from that obtained when looking at the within sample fit statistics reported in Table 1.2.
This is a simple instance of the bias–variance tradeoff: adding flexibility can improve in-sample fit while hurting out-of-sample prediction.
Tables 1.2 and 1.3 also provide the test 𝑀𝑆𝐸 of the flexible model estimated via Lasso regression.20 Lasso is a penalized regression method used to reduce the complexity of a regression model when the ratio 𝑝 𝑛 is not small. It achieves this by penalizing the sum of the absolute values of the regression coefficients, thereby shrinking some coefficients to exactly zero and effectively performing variable selection.
We introduce this method in more detail in Chapter 3, but it is applied here despite appearing as a "black box" at this stage. The out-of-sample 𝑀𝑆𝐸 can also be computed for any other black-box prediction method. In this example, Lasso performs similarly to the basic and flexible regression models estimated using OLS. This result is not surprising, given the modest dimensionality of the problem and the similarity in performance between the two OLS-estimated models.
Finally, to highlight the potential of estimating the linear model via OLS to overfit, we consider one more model.
▶ In the Extra Flexible Model, 𝑋 consists of sex and all two-way interactions between experience, experience2, experience3, experience4, and all other raw regressors except for sex. In total, we have 𝑝 = 979 regressors in this
specification.

We report measures of predictive performance in the training and test data from OLS and Lasso estimates of our “extra flexible” model in Table 1.4. Here, we see that the model estimated by OLS appears to be overfitting. The in-sample statistics substantially overstate predictive performance relative to the performance we see in the test data. For example, the 𝑅2 and adjusted 𝑅2 in the training data are 0.467 and 0.345, both of which substantially overstate the 𝑅2 obtained in the test data, 0.148. We also see that
 

















20: Lasso stands for "Least Abso-lute Shrinkage and Selection Oper-ator".
 

 

	OLS	Lasso
𝑀𝑆𝐸𝑠𝑎𝑚𝑝𝑙𝑒	0.178	0.210
𝑀𝑆𝐸𝑎𝑑𝑗	0.235	0.223
𝑀𝑆𝐸𝑡𝑒𝑠𝑡	0.250	0.199





the performance on the test data for the extra flexible model is substantially worse than the performance of the much simpler basic and flexible models. That is, it looks like the OLS estimates of the extra flexible model have specialized to fitting aspects of the training data that do not generalize to the test data and lead to a deterioration in predictive performance relative to the simpler models.
The performance of the Lasso contrasts sharply with this be-havior. We see that the in-sample and out-of-sample predictive performance measures for the Lasso based estimates of the extra flexible model are similar to each other. They are also similar to the performance of the simpler models. It seems that Lasso is finding a competitive predictive model without overfitting even in the extra flexible model. We will return to this behavior in Chapter 3 where we will show that Lasso and related methods are able to find good prediction rules in even extremely high-dimensional settings, where for example 𝑝  𝑛, where OLS breaks down theoretically and in practice.

Wage Gap
An important question is whether there is a gap (i.e., difference) in predicted wages between male and female workers with the same job-relevant characteristics. To answer this question, we estimate the log-linear regression model:
𝑌 = 𝛽1𝐷 + 𝛽′2𝑊 + 𝜀,	(1.5.1)
where 𝑌 is log-wage, 𝐷 is the sex indicator (1 if female and 0 otherwise) and the 𝑊’s are other determinants of wages.
𝑊 includes a constant of 1, education, polynomials in experi-ence, region, and occupation and industry indicators plus all two-way interactions of polynomial in experience with region,
occupation, and industry indicators. This gives us 𝑝 = 246 regressors.
 
Table 1.4: Assessment of predictive performance in the extra flexible model with 𝑝 = 979 regressors.



































The Notebooks 1.8.2 contain the code for this section.
 


Table 1.5: Empirical means for the groups defined by the sex variable for never-married workers.









 
As we have log-transformed wages, we are analyzing the relative difference in pay for male and female workers. Table 1.5 tabulates mean characteristics given sex. It shows that the difference in average log-wage between never married male and never married female workers is equal to 0.038 with male workers earning more. Thus, in this group, male average wage is about 3.8% higher than female average wage.21 We also observe that never married female workers are relatively more educated than never married male workers.
Table 1.6 summarizes the regression results. Overall, we see that the unconditional wage gap of size 3.8% for female workers increases to about 7% after controlling for worker characteristics. This means we would predict a female worker’s wage to be about 7% less per hour on average than the wage of a male worker who had the same experience, education, geographical region and occupation.
The partialling-out approach provides a numerically identical estimate for the coefficient 𝛽1 (𝛽1  7%), numerically confirming the FWL theorem. Using Lasso for partialling-out (p-out w/ Lasso) gives similar results to using OLS. This similarity is expected here, since
 







21: This interpretation relies on the approximation log(𝑎) − log(𝑏) ≈ (𝑎 − 𝑏)/𝑏, which is accurate when-ever (𝑎 − 𝑏)/𝑏 is small and 𝑎, 𝑏 > 0.
 
𝑝/𝑛 is small,
and partialling out by least squares should work well.
 




Table 1.6: Estimated conditional wage gaps for never married work-ers.
 


To sum up, our estimate of the conditional wage gap for never-married workers using OLS is about −7% and the 95% confidence interval is about [−10%, −4%].

 
Kitagawa-Oaxaca-Blinder Decomposition
One way to understand the estimate with controls ( 0.070) is as the part of the total gap ( 0.038) that cannot be explained by differences in group characteristics. This is a descriptive accounting identity; interpreting the “unexplained” compo-nent as discrimination requires additional causal assumptions. Namely, take Eq. (1.5.1) and average it in the male and female groups22 to obtain the decomposition:23
𝔼𝑛[𝑌 | 𝐷 = 1] − 𝔼𝑛[𝑌 | 𝐷 = 0]
−0.038
=	𝛽ˆ1	+ 𝛽ˆ′2(𝔼𝑛[𝑊 | 𝐷 = 1] − 𝔼𝑛[𝑊 | 𝐷 = 0]) .
−0.070	0.032
Here, the 0.032 difference in average log wages, predicted based on differences in observed characteristics (𝑊) and slopes (𝛽ˆ2), suggests higher average log wages for female workers
compared to male workers. However, this positive difference is counteracted by a negative difference of 0.070 that remains unexplained by the characteristics. This unexplained difference arises from the different pay predicted for female and male workers possessing the same characteristics.24

1.6	Inference on Predictive Effect when
𝑝/𝑛 < 1 is not small★
In order to wrap up and provide a stylized illustration of the impact of dimensionality 𝑝 on inference, we revisit the extra-flexible model from the prediction exercise, which used 𝑝 = 979
controls. Up to this point, our inference arguments relied on
the “𝑝 𝑛 small” regime; the goal here is to see what changes as we move toward 𝑝 𝑛 close to one. To further "stress-test" the
inference, we reduce the sample size to 𝑛 = 1000 by selecting a random subset of the original observations.25
In this reduced-sample setting, we have 𝑝 𝑛  1, meaning the usual theory for estimating linear model coefficients using
 










22: 𝔼𝑛   𝐷 = 𝑑 abbreviates 𝔼𝑛
for the subsample of the data where
𝐷 = 𝑑, for 𝑑 = 0, 1.
23: Decompositions of this sort and the one given below are called Kitagawa-Oaxaca-Blinder decomposi-tion introduced in [12], [13], and [14], in different contexts.









24: Differences of this sort poten-tially reveal discrimination in the pay structure, a question we will return to later in the book.








25: Using the full sample yields re-sults very similar to those reported in the previous section. In this case,
𝑝 𝑛 1 5, which is small enough for conventional OLS inference to perform reasonably well, demon-strating its substantial resilience. Please verify this as an exercise us-ing the Wage Gap Notebook.
 

 
OLS no longer applies. [15] provide more refined results for OLS estimates of regression coefficients in the case where
𝑝 𝑛	𝐶 < 1. They show that OLS estimates of individual
coefficients can still be consistent in this regime and also provide an estimator for the asymptotic variance that is consistent when 𝑝 𝑛 < 1 2, as long as certain regularity conditions are satisfied. Furthermore, they note that the usual Eicker-Huber-White robust variance estimator is inconsistent in this high-dimensional setting, while the jackknife variance estimator,26 although not consistent, remains conservative.
We report estimates of the conditional wage gap in this setup in Table 1.7. Specifically, we report point estimates from OLS applied to the full set of variables, providing both the Eicker-Huber-White standard error (HC0) and the jackknife standard error (HC3).27 These are provided mainly for illustration, but
 










26: The jackknife variance estima-tor is a resampling-based method.
First, an estimate 𝜃ˆ is computed us-
ing all 𝑛 observations. Then, each observation is omitted one at a time,
and the model is refitted to obtain a new estimate 𝜃ˆ 𝑖 . The variance estimate is calculated as
 
we note that HC0 is known to be inconsistent and to behave very
 
Var ˆ
 
𝑛 −  I:( ˆ
 
ˆ)2
 

 
setting. HC3 is more reliable, but one should also be skeptical given that 𝑝 𝑛 1 in this example. Finally, we report point estimates and standard errors for the Double Lasso procedure. The resulting estimator is consistent, asymptotically normal, and has estimable standard errors under the structure outlined in Chapter 4 even when 𝑝  𝑛. For now, we can think of it as a point of comparison.

 
which measures the sensitivity of
𝜃ˆ to individual data points by com-
paring leave-one-out estimates to the original estimate.
27: The Eicker-Huber-White vari-ance estimator is often referred to as “HC0” and the jackknife as “HC3.”

Table 1.7: The estimated condi-tional wage gaps for never married workers with approximately 1000 controls in a sample of 1000 obser-vations.
 

Comparing to the case with the full data set, we see that point estimates are not wildly different but that standard errors are larger. Part of the standard error difference is predicted simply by the difference in sample sizes. Specifically,	5150 1000 2.27, so we would expect standard errors to be about 2.27 times larger with 𝑛 = 1000 observations than with 𝑛 = 5150. This
inflation holds almost exactly for the Double Lasso estimates.
More interestingly, now that 𝑝 𝑛 -,t 0, we start seeing substantial differences in standard errors between unregularized partialling out (OLS) and partialling out with Lasso (also known as Double Lasso). While we do not want to take the OLS standard errors too seriously—given that the Huber-Eicker-White standard error does not work in this setting and we are also skeptical of the jackknife here—the comparison between the OLS and Double
 


Lasso standard errors, as well as the comparison to the full-sample results, is revealing. Relative to the full sample results, the jackknife standard error (HC3) is much larger than would be expected simply due to the decrease in sample size. The difference from this expectation (partially) reflects the impact of dimensionality on the OLS estimate of the regression coefficient. In contrast, the Double Lasso appears to be roughly insensitive to the dimensionality of the control variables and scales exactly as one would expect given the difference in sample size.
The punchline of this final example is that OLS is no longer adaptive in the “𝑝 𝑛 not small” regime. The lack of adaptivity means that conventional properties of OLS may fail, and that other procedures may become preferable to OLS.

1.7	Notes
Least squares were invented by Legendre ([16]) and Gauss ([17]) around 1800. Frisch, Waugh, and Lovell ([18],[19],[20]) discovered the partialling-out interpretation of the least squares coefficients in the 1930s. The asymptotic theory mentioned in Theorems 1.3.1 and 1.4.2 is more recent and has been developed since early work of Huber in the 70s on 𝑚-estimators (estimators that minimize objective functions that correspond empirical averages of losses) under moderately high dimensions; see e.g.
[21] and the textbook [22].
For a good, concise treatment of classical least squares, see for example, Chapter 1 in Amemiya’s classical graduate economet-rics text [3]; Bruce Hansen’s new textbook [23] is an excellent up-to-date reference.
Regularity conditions under which Theorem 1.3.1 and Theorem
1.4.2 hold under 𝑝	and 𝑝 𝑛 0 asymptotics can be found in [24] and [15]. The results of the latter reference allow for 𝑝 𝑛   𝑐 < 1, which introduces an additional asymptotic
variance term when 𝑐 > 0; the case with 𝑐 = 0 recovers Theorem
1.4.2. See also review [25] for some recent understanding of
properties of least squares estimators.

1.8	Notebooks
Notebook 1.8.1 (Predicting Wages) Predicting Wages R Note-book and Predicting Wages Python Notebook contain a simple predictive exercise for wages. We will return to this dataset
 

and prediction problem repeatedly in future chapters, re-estimating it using a broad range of ML estimators and providing a means of comparing their performance.

Notebook 1.8.2 (Analyzing Wage Gaps) Wage Gaps R Note-book and Wage Gaps Python Notebook contain a simple analysis of wage gaps.

Notebook 1.8.3 (Exploring Overfitting Notebook) The Linear Model Overfitting R Notebook and the Linear Model Over-fitting Python Notebook contain a set of simple simulations that show how measures of fit perform in a high 𝑝/𝑛 setting.


1.9	Checklist
After reading this chapter, you should be able to:
▶ State the best linear prediction (BLP) problem and derive the normal equations.
▶ Explain how ordinary least squares (OLS) estimates the BLP and why the typical prediction error scales like  𝑝 𝑛.
▶ Diagnose overfitting and evaluate prediction rules using sample splitting (and, in later chapters, cross-validation).
▶ Interpret a regression coefficient as a predictive effect via partialling-out (Frisch–Waugh–Lovell) and construct a confidence interval when 𝑝/𝑛 is small.

1.10	Exercises
Exercise 1.10.1 (Sample Splitting) Write a notebook (R, Python, etc.) where you briefly explain the idea of sample splitting to evaluate the performance of prediction rules to a fellow student, and show how to use it on the wage data. The expla-nation should be clear and concise (one paragraph suffices) so that a fellow student can understand. You can take our notebooks as a starting point, but provide a bit more explana-tion and modify them by exploring different specifications of the models (or looking at an interesting subset of the data or even other data – for example, the data you use for your research or thesis work).

Exercise 1.10.2 (Least Squares and Partialling-out) Write a
 

notebook (R, Python, etc), where you carry out a wage gap analysis, focusing on the subset of college-educated workers. The analysis should be analogous to what we’ve presented – explaining "partialling out," generating point estimates and standard errors – but don’t hesitate to experiment and explain more. Exploring other data-sets or similar questions, e.g. wage gaps by race, is always welcome.

Exercise 1.10.3 (Discovering Heterogeneity in Wage Gap) Write a notebook (R, Python, etc.) where you carry out a wage gap analysis and decomposition for each major education group separately. In essence, this amounts to performing linear wage gap regressions and decompositions for each group separately. Report the findings and any patterns of heterogeneity you observe. Is the heterogeneity you see eco-nomically and statistically significant? What if you perform the analysis by occupation groups instead? How do these group-wise decompositions contribute to the overall wage gap?

Exercise 1.10.4 (Machine Learning in Ancient Greece) The half-serious link to Pythagoras was serious in its half. Consider sample linear regression with 𝑛 = 2 and just one regressor,
so that 𝑌𝑖 = 𝛽ˆ𝑋𝑖  𝜀𝑖 for 𝑖 = 1, 2, where 𝛽ˆ is the ordinary
least squares estimator, a scalar quantity in this case. Let Y = (𝑌1, 𝑌2)′ , X = (𝑋1, 𝑋2)′, 𝜀ˆ = (𝜀ˆ1, 𝜀ˆ2)′, and let Yˆ = 𝛽ˆX. Find the connection between the decomposition Y′Y/𝑛 = Yˆ′Yˆ /𝑛 +
𝜀ˆ′𝜀ˆ/𝑛 and the Pythagorean theorem. Find the geometric
interpretation for 𝑌ˆ , and write the explicit formula for 𝛽ˆ in
this case. If you get stuck, google the "geometric interpretation of least squares."

1.A Central Limit Theorem★
Univariate
Consider the scaled sum 𝑊 = '5:𝑛	𝑋𝑖/√𝑛 of independent and
identically distributed variables 𝑋𝑖 such that E 𝑋 = 0 and Var 𝑋 = 1. The classical CLT states that 𝑊 is approximately Gaussian provided that none of the summands are too large,
namely
sup P(𝑊 ≤ 𝑥) − P(𝑁(0, 1) ≤ 𝑥) ≈ 0.
𝑥∈ℝ
 


This result is reassuring, but the theorem does not inform us how small the error is in a given setting.
The Berry-Esseen theorem provides a quantitative characteriza-tion of the error.


 
The result asserts that the Gaussian approximation error rate declines like 1 √𝑛. It also states that given 𝑛, the approxima-
tion quality improves as the third absolute moment E 𝑋 3
decreases. This results gives a good guide regarding when the Gaussian approximation gives accurate results.28 Of course, one can also check the approximation quality via simulation experiments that mimic the practical situation.
 





28: Consider, for instance, the case when 𝑋𝑖 are centered and standard-ized Bernoulli random variables
with success probability 𝑝, i.e., 𝑋𝑖 =
 
𝑍𝑖 −𝑝 
 
𝑝(1−𝑝)
 
and 𝑍𝑖 is Bernoulli with
 
Multivariate
Later in the book, we will use multivariate central limit theorems as well. To this end, we are going to state the following more general result due to [26], which refines earlier results by [27] and [28].
Let I be a countable set (either finite or infinite) and let 𝑋𝑖 , 𝑖 I, be independent ℝ𝑑-valued random vectors. Assume that E 𝑋	= 0 for all 𝑖 and that	Var 𝑋	= 𝐼 . It is well known
that in this case, the sum 𝑊 :=	𝑖 I 𝑋𝑖 exists almost surely
and that E𝑊 = 0 and Var(𝑊 ) = 𝐼𝑑.
 
success probability 𝑝. The error in the Berry-Esseen theorem, in this case, becomes 1 𝑝 1 𝑝 𝑛. Thus, the error in the Gaussian ap-proximation is guaranteed to be small by the Berry-Esseen theorem only if 𝑝 1  𝑝 𝑛 is large. Thus, for extreme probabilities, where ei-ther success or failure events are extremely rare for the given sample size, i.e., when 𝑝 𝑛 or 1  𝑝  𝑛 is small, the use of the Gaussian approximation is not advisable.
 

 
 


Bibliography



[1]	Cambridge Dictionary, Infer (cited on page 11).
[2]	Alexandre B. Tsybakov. Introduction to nonparametric esti-mation. Springer, 2009 (cited on page 17).
[3]	Takeshi Amemiya. Advanced Econometrics. Cambridge, MA: Harvard University Press, 1985 (cited on pages 22, 36).
[4]		Data Splitting | R-bloggers. https://www.r-bloggers. com/2016/08/data-splitting/, Accessed: 2022-25-02 (cited on page 23).
[5]	Friedhelm Eicker. ‘Limit theorems for regressions with unequal and dependent errors’. In: ed. by Lucien M. Le Cam and Jerzy Neyman. 1967 (cited on page 27).
[6]		Peter J. Huber. The behavior of maximum likelihood estimates under nonstandard conditions. English. Proc. 5th Berkeley Symp. Math. Stat. Probab., Univ. Calif. 1965/66, 1, 221-233 (1967). 1967 (cited on page 27).
[7]	Halbert White. ‘A heteroskedasticity-consistent covari-ance matrix estimator and a direct test for heteroskedastic-ity’. In: Econometrica (1980), pp. 817–838 (cited on page 27).
[8]	Friedhelm Eicker. ‘Asymptotic normality and consistency of the least squares estimators for families of linear re-gressions’. In: Annals of Mathematical Statistics 34.2 (1963),
pp. 447–456 (cited on page 27).
[9]		Matias Cattaneo, Michael Jansson, and Whitney Newey. ‘Alternative Asymptotics and the Partially Linear Model with Many Regressors’. In: Working Paper, http://econ-www.mit.edu/files/6204 (2010) (cited on page 27).
[10]	Thomas Lemieux. ‘The “Mincer equation” thirty years after schooling, experience, and earnings’. In: Jacob Min-cer a Pioneer of Modern Labor Economics. Springer, 2006,
pp. 127–145 (cited on pages 28, 30).
[11]	Claudia Goldin. ‘Nobel Lecture: An Evolving Economic Force’. In: American Economic Review 114.6 (2024), pp. 1515–
1539 (cited on page 29).
[12]	Evelyn M Kitagawa. ‘Components of a difference between two rates’. In: Journal of the American Statistical Association
50.272 (1955), pp. 1168–1194 (cited on page 34).
 


[13]		Ronald Oaxaca. ‘Male-Female Wage Differentials in Ur-ban Labor Markets’. In: International Economic Review 14.3 (1973), pp. 693–709. (Visited on 10/10/2023) (cited on page 34).
[14]	Alan S. Blinder. ‘Wage Discrimination: Reduced Form and Structural Estimates’. In: Journal of Human Resources
8.4 (1973), pp. 436–455. (Visited on 10/10/2023) (cited on
page 34).
[15]	Matias D. Cattaneo, Michael Jansson, and Whitney K. Newey. ‘Inference in linear regression models with many covariates and heteroscedasticity’. In: Journal of the Amer-ican Statistical Association 113.523 (2018), pp. 1350–1361
(cited on pages 35, 36).
[16]	Adrien Marie Legendre. Nouvelles méthodes pour la détermi-nation des orbites des comètes: avec un supplément contenant divers perfectionnemens de ces méthodes et leur application aux deux comètes de 1805. Courcier, 1806 (cited on page 36).
[17]	Carl-Friedrich Gauss. Theoria combinationis observationum erroribus minimis obnoxiae. Henricus Dieterich, 1823 (cited on page 36).
[18]		Ragnar Frisch and Frederick V Waugh. ‘Partial time regressions as compared with individual trends’. In: Econometrica (1933), pp. 387–401 (cited on page 36).
[19]		Michael C Lovell. ‘Seasonal adjustment of economic time series and multiple regression analysis’. In: Journal of the American Statistical Association 58.304 (1963), pp. 993–1010 (cited on page 36).
[20]		Michael C Lovell. ‘A simple proof of the FWL theorem’. In: Journal of Economic Education 39.1 (2008), pp. 88–91 (cited on page 36).
[21]	Peter J Huber. ‘Robust regression: asymptotics, conjec-tures and Monte Carlo’. In: Annals of Statistics (1973),
pp. 799–821 (cited on page 36).
[22]		Elvezio M Ronchetti and Peter J Huber. Robust Statistics. John Wiley & Sons Hoboken, NJ, USA, 2009 (cited on page 36).
[23]	Bruce E. Hansen. Econometrics. Princeton University Press, 2022 (cited on page 36).
[24]	Alexandre Belloni, Victor Chernozhukov, Denis Chetverikov, and Kengo Kato. ‘Some New Asymptotic Theory for Least Squares Series: Pointwise and Uniform Results’. In: Jour-nal of Econometrics 186.2 (2015), pp. 345–366 (cited on
page 36).
 


[25]		Arun K. Kuchibhotla, Lawrence D. Brown, and Andreas Buja. ‘Model-free study of ordinary least squares lin-ear regression’. In: arXiv preprint arXiv:1809.10538 (2018) (cited on page 36).
[26]	Martin Raič. ‘A multivariate Berry–Esseen theorem with explicit constants’. In: Bernoulli 25.4A (2019), pp. 2824–2853 (cited on page 39).
[27]		Vidmantas Bentkus. ‘On the dependence of the Berry–Esseen bound on dimension’. In: Journal of Statistical Planning and Inference 113.2 (2003), pp. 385–402 (cited on
page 39).
[28]	F Goetze. ‘On the rate of convergence in the multivariate CLT’. In: Annals of Probability (1991), pp. 724–739 (cited on page 39).
 

 

Causal Inference via Randomized
Experiments


“Let us divide them in halves, let us cast lots, that one half of them may fall to my share, and the other to yours; I will cure them without bloodletting and sensible evacuation; but do you do as ye know [...]. We shall see how many funerals both of us shall have.”

– Jan Baptist van Helmont (17th Century)

Van Helmont’s line about casting lots sounds like a provocation, but it is really a blueprint for credible evidence. He was insisting on a fair comparison: if the treated and untreated patients are comparable by design, then differences in outcomes can be attributed to the treatment rather than to who happened to receive it. The challenge is memorable because it isolates the object of causal inference. We do not merely want to know whether treated units look different from untreated units; we want to know whether the treatment itself changes what would have happened otherwise.
In this chapter we make that distinction precise and turn it into a practical empirical method. We introduce the potential out-comes framework and show how randomized controlled trials (RCTs) identify average treatment effects through simple com-parisons of group means. We then explain how pre-treatment covariates can sharpen estimation, reveal heterogeneity across subgroups, or both. We also introduce causal diagrams as a compact way to visualize the assumptions that make RCT logic work, and we conclude by outlining some key limitations of RCTs, including settings where interference, externalities, or equilibrium effects matter.
 
2
2.1	Potential Outcomes Frame-work and Average Treatment Effects	44
Random Assignment/Ran-domized Controlled Trials	47
Statistical Inference with Two Sample Means	49
Pfizer/BioNTech Covid Vac-cine RCT	50
2.2	Pre-treatment Covariates and Heterogeneity	52
Regression and Statistical In-ference for ATEs	54
Classical Additive Ap-proach	54
The Interactive Approach: Always Improves Precision and Discovers Heterogeneity . 57 Reemployment	Bonus
RCT	58
2.3	Drawing RCTs via Causal Diagrams	59
2.4	Key Limitations of RCTs 60 Externalities, Stability, and Equilibrium Effects	60
Ethical, Practical, and Gen-eralizability Concerns	61
2.5	Notes	61
2.6	Notebooks	62
2.7	Checklist	62
2.8	Exercises	63
2.	AApproximate Distribution of the Two Sample Means	64
2.B  Statistical Properties of the Classical Additive Approach★	65
 
2.1		Potential Outcomes Framework and Average Treatment Effects
 

To begin, we introduce a notation that forces us to say explicitly what we mean by “the effect of a treatment.” The potential outcomes (POs) framework provides this language and will remain one of the principal tools throughout the book.1 For each observational unit, we posit two latent (unobserved) random variables
𝑌(1) and 𝑌(0).
They represent the potential (counterfactual) outcomes the unit would realize under treatment (treatment state 𝑑 = 1) and under no treatment (control state 𝑑 = 0).2 In an economic context, the treatment might be a training program or a policy intervention,
and the outcome might be an individual’s wage or employment status. In what follows, it is also useful to introduce the potential response or structural function:
𝑑 ↦→ 𝑌(𝑑),
which maps the potential treatment state 𝑑 ∈ {0, 1} to the random potential outcome 𝑌(𝑑).
The key difficulty appears immediately: for any given unit we can never observe both 𝑌 1 and 𝑌 0 at the same time. This is the fundamental problem of causal inference. Each unit is either treated or untreated, so one of the two potential outcomes remains counterfactual. This is why causal questions are inherently comparative: we generally cannot learn a unit-level effect directly, but we can hope to learn averages from groups that are made comparable.
The individual treatment effect is
𝑌(1) − 𝑌(0).
This effect will typically vary across individuals. Because we observe only one of the two terms, it is generally infeasible to uncover individual treatment effects.3 What we can often esti-mate are population summaries such as the average treatment effect (ATE):
𝛿 = E[𝑌(1) − 𝑌(0)] = E[𝑌(1)] − E[𝑌(0)].

To connect potential outcomes to observed data, let 𝐷 denote the treatment assignment: 𝐷 = 1 if the unit is assigned to treatment and 𝐷 = 0 otherwise. In settings with perfect compliance,
 





1: POs were introduced by Donald Rubin [1], building upon the earlier work of R. A. Fisher and J. Neyman in the 1920s.



2: For simplicity, here we focus on binary treatments; the ideas are similar for multivalued and con-tinuous treatments.

























3: As an example, we could un-cover individual treatment effects if we had identical twins (for exam-ple, a pair of cars) that could be put in treatment and control groups.
 


assignment coincides with treatment receipt; later we discuss what changes when they differ.

In the binary case, this can also be written as 𝑌 = 𝐷𝑌 1
1 𝐷 𝑌 0 . For example, suppose the treatment group (𝐷 = 1) consists of individuals who completed a job training program, while the control group (𝐷 = 0) comprises those who did not. According to Assumption 2.1.1, an individual’s observed wage outcome equals 𝑌 1 if she completed the program (𝐷 = 1) and equals 𝑌 0 if she did not (𝐷 = 0 . Although this assumption might seem almost tautological, it plays a crucial role by ruling
out hidden variations in treatment. In other words, it requires that the treatment and control conditions are well defined and clearly correspond to the observed treatment status, 𝐷.


 
Assumption 2.1.2 is implicitly embedded in our compact nota-tion 𝑌 𝑑 , which indexes each unit’s outcome only by its own treatment status.4 This formulation rules out scenarios in which the treatment administered to one unit affects the outcome of another unit. For example, in social networks, treating an in-dividual might influence the outcomes of all of that person’s friends. Some types of spillover effects can be accommodated by expanding the treatment definition and adjusting the poten-tial outcomes accordingly,5 but addressing these extensions is beyond the scope of this book.6
The following analytical example may help gain better under-standing of the potential outcomes framework.
 


4: Assumptions 2.1.1 and 2.1.2 to-gether constitute what is commonly known as the Stable Unit-Treatment Value Assumption (SUTVA); see, e.g., Imbens and Rubin [2].


5: For example, if each individual has two friends, we could define po-tential outcomes with spillovers as
𝑌 𝑑0, 𝑑1, 𝑑2 , where 𝑑0 represents the individual’s treatment status,
𝑑1 denotes the treatment status of friend 1, and 𝑑2 denotes that of friend 2.
6: For further reading, see, among many others, [3], [4], [5], [6], and
[7].
 


 
Under Assumption 2.1.1, population data directly provide the conditional averages
E[𝑌 | 𝐷 = 𝑑] = E[𝑌(𝑑) | 𝐷 = 𝑑], for 𝑑 ∈ {0, 1}.
The difference of the two averages gives us the average predictive effect (APE) of treatment status on the outcome:
𝜋 = E[𝑌 | 𝐷 = 1] − E[𝑌 | 𝐷 = 0].
It measures the association of the treatment status with the outcome.
While the APE is identified – meaning computable from the population data – it may seem surprising that the APE in general does not agree with the ATE 𝛿:
𝛿 ≠ 𝜋.	(2.1.1)
The difference between the APE and ATE is generally said to be due to selection bias. We clarify this intuition in the following example and then explain it more formally below.

To sum up, in the smoking example, the chosen "treatment" variable 𝐷 is potentially negatively associated with the potential
 


health outcome, inducing the selection bias – the difference between the predictive effect and the causal effect.


It is useful to emphasize the main reason for having selection bias is that
E[𝑌(𝑑)|𝐷 = 1] ≠ E[𝑌(𝑑)]
whenever 𝐷 is not independent of 𝑌 𝑑 . If 𝐷 and 𝑌 𝑑 were independent,
E[𝑌(𝑑)|𝐷 = 1] = E[𝑌(𝑑)]
would hold since in this case 𝐷 is uninformative about the potential outcome and drops out from the conditional expecta-tion.
To sum up, the problem with observational studies like our con-trived example is that the "treatment" variable 𝐷 is determined by individual behaviors which may be linked to potential out-comes. This linkage generates selection bias - the disagreement between APE and ATE. There are many ways of addressing selection bias, one of which is through an experiment, where we randomly assign the treatment to the units.

Random Assignment/Randomized Controlled Trials
A way to remove selection bias is through random assignment of treatment.
 


 
This assumption states that the treatment assignment mecha-nism is purely random, and the positivity condition ensures that there are units in treatment and in control.

A key result is that selection bias is removed under random assignment, which allows us to learn summaries of causal effects.

 
 
A short argument explains why Theorem 2.1.1 holds. By Consis-tency (Assumption 2.1.1), for any 𝑑   0, 1 we have E 𝑌  𝐷 =
𝑑 = E 𝑌 𝑑  𝐷 = 𝑑 . By random assignment, 𝐷   𝑌 𝑑 , so
conditioning on 𝐷 = 𝑑 does not change the distribution of 𝑌 𝑑 and hence E 𝑌 𝑑 𝐷 = 𝑑 = E 𝑌 𝑑 . Putting these equalities together yields the statement.
Assumption 2.1.3 is often not plausible for observational data. In a randomized controlled trial (RCT)7 , the aim is to ensure the plausibility of Assumption 2.1.3 by direct random assignment of treatment 𝐷. That is, subjects are randomly assigned a treat-ment state 𝐷 by the experimenter without regard to any of their
 









7: Synonyms are experiments and A/B tests.
 


 
characteristics. Because the random assignment of the treat-ment is unrelated to all subject characteristics by construction, well-executed RCTs guarantee that Assumption 2.1.3 is satisfied. Because of this property, many consider RCTs as the gold stan-dard in causal inference, and RCTs are routinely employed in a variety of important settings.8 Examples include evaluating the efficacy of medical treatment, vaccinations, training programs, marketing campaigns, and other kinds of interventions.


Statistical Inference with Two Sample Means
Inference is based on the independent sample	 𝑌𝑖 , 𝐷𝑖	𝑛 obtained from an RCT, where index 𝑖 denotes the observational unit. We assume that each 𝑌𝑖 , 𝐷𝑖 has the same distribution as 𝑌, 𝐷 . Estimation of the two means 𝜃𝑑 = E 𝑌	𝐷 = 𝑑 for
𝑑 = 0 and 𝑑 = 1 can be done by considering two group means
𝔼𝑛[𝑌1(𝐷 = 𝑑)]
𝔼𝑛[1(𝐷 = 𝑑)]
The two means example can also be treated as a special case of linear regression,9 but we find it instructive to work out the details directly for the two group means. We provide these details in Section 2.A.
 






8: Of course, RCTs must be cor-rectly done to guarantee Assump-tion 2.1.3. For example, RCTs where experimental protocols are not fol-lowed may suffer from selection bias.



























9: Indeed, we can regress 𝑌 on 𝐷
and 1 𝐷; that is, estimate the model 𝑌 = 𝜃1𝐷 𝜃0 1 𝐷 𝑈. We can then apply the inferential ma-
chinery developed in the previous chapter.
 

 
 



so that 𝛿ˆ = 𝜃ˆ1 − 𝜃ˆ0 obeys
√𝑛(𝛿ˆ − 𝛿) a 𝑁(0, V11 + V22).
∼

To use this result in practice, variance components are usually estimated using the plug-in principle, which amounts to using the sample analogues of the expressions above.
Sometimes we are interested in relative effectiveness of treat-ment effects (for example, vaccine efficiency):
𝑓 (𝜃) = (𝜃1 − 𝜃0)/𝜃0 = 𝛿/𝜃0.
Relative effectiveness can be estimated by 𝛿ˆ/𝜃ˆ0 = 𝑓 (𝜃ˆ), where
𝜃ˆ = 𝜃ˆ𝑑 𝑑  0,1 and 𝜃 = 𝜃𝑑 𝑑 0,1 , with approximate distri-
bution obtained using the delta method:
 
√𝑛( 𝑓 (𝜃ˆ) − 𝑓 (𝜃)) ≈ 𝐺′√𝑛(𝜃ˆ − 𝜃) a
 
𝑁(0, 𝐺′V𝐺),
 
where 𝐺 = ∇ 𝑓 (𝜃), 𝜃ˆ = (𝜃ˆ0 , 𝜃ˆ1)′, 𝜃 = (𝜃0, 𝜃1).10 .

Pfizer/BioNTech Covid Vaccine RCT
Pfizer/BNTX was the first vaccine approved for emergency use in the EU and US to reduce the risk of Covid-19 disease. See the Food and Drug Administration (FDA) briefing for details about the RCT and the summary data. Volunteers were randomly assigned to receive either a treatment (2-dose vaccination) or a placebo, without knowing which they received, and the doctors making the diagnoses did not know whether a given volunteer received a vaccination or not. In other words, the trial was a double-blind randomized control trial. The results of the study are presented in the following table.
We see that the rate of Covid-19 infection was relatively low at the time. Specifically, the treatment group saw 9 Covid-19 cases per 19,965, while the control group saw 169 cases per 20,172.
The estimated average treatment effect is about
−792.7 cases per 100,000, and the 95% confidence interval is11 12
[−922, −664].
 

10: The approximation follows from application of the first order Taylor expansion and continuity of the derivative ∇ 𝑓 at 𝜃.

Figure 2.1: Tozinameran (Pfizer-BioNTech Covid-19 vaccine); Image Source: Wikipedia / Arne Müseler






11: The Notebooks 2.6.1 contain the analysis of the Pfizer-BioNTech Covid-19 Vaccine RCTs.
12: In this example, we don’t need the underlying individual data to evaluate the effectiveness of the vaccine because the poten-tial outcomes are Bernoulli random
variables with mean E[𝑌(𝑑)] and variance Var(𝑌(𝑑)) = E[𝑌(𝑑)](1 − E[𝑌(𝑑)]).
 


Table 2.1: Aggregate efficacy results from the Pfizer/BioNTech COVID-19 vaccine RCT (as reported in the FDA briefing); “Surveillance Time” is total at-risk follow-up (in 1000 person-years); (𝑛𝑑) is the number of participants
 
contributing that follow-up.
 

BNT162b2
𝑁𝑎 = 19965
 

Placebo
𝑁𝑎 = 20172
 

Vaccine Efficacy % (95% CI)𝑒
 
Efficacy Endpoint
 
Cases
1
 
Surveillance Time𝑐
2
 
Cases
1
 
Surveillance Time𝑐
2
 






 

Under Assumptions 2.1.3 and 2.2.1 the confidence interval suggests that the Covid-19 vaccine caused a reduction in the risk of contracting Covid-19.
We also compute the Vaccine Efficacy metric, which according to [8], refers to the following measure:
VE = Risk for Unvaccinated - Risk for Vaccinated.
Risk for Unvaccinated
It describes the relative reduction in risk caused by vaccination. Estimating the VE is simple as we can plug-in the estimated group means. We can compute standard errors using the delta method or by simulation. We obtain that the overall vaccine efficacy is 94.6%, replicating the results shown in Table2.1. Our 95% confidence interval for VE, based on the normal approximation, is
 
[90.9%, 98.2%],
which differs only slightly from the FDA briefing table.13
 


13: The analysis in the FDA table is based on the inversion of exact binomial tests, the Cornfield proce-dure.
 


 
2.2	Pre-treatment Covariates and Heterogeneity
Sometimes we also have additional pre-treatment or pre-determined covariates 𝑊. We might be interested in either using these co-variates to estimate average effects more precisely or to describe heterogeneity of the treatment effects. For example, we might be interested in the impact of a treatment across age or income groups.
For this purpose, we consider conditional average treatment effects (CATE):
𝛿(𝑊) = E[𝑌(1) | 𝑊] − E[𝑌(0) | 𝑊],
which compare the average potential outcomes conditional on a set of covariates 𝑊.
We can directly learn the conditional predictive effects (CAPE),
𝜋(𝑊) = E[𝑌 | 𝐷 = 1, 𝑊] − E[𝑌 | 𝐷 = 0, 𝑊],
from population data. However, these CAPE will generally not agree with the CATE. One assumption that will be sufficient for the CAPE and CATE to agree is having treatment assigned randomly and independently of covariates. As before, the use of RCTs help ensure the plausibility of this assumption.

 







 
This assumption spells out that, if we plan to use covariates in the analysis, randomization has to be made with respect to these covariates as well.
In practice, it is often tempting to use post-treatment covariates in regression analysis, but the use of such variables runs the danger of violating Assumption 2.2.1.14 A common scenario where accidentally using a post-treatment covariate may occur is when researchers encounter missing data from imperfect data collection in following-up with control and treated units to collect demographic information. For instance, if treated units are more likely to move or change contact information because the intervention affects their opportunities, then follow-up may be systematically harder in the treated group. When we drop observations with missing data, we implicitly condition on a post-treatment variable (missingness) which can cause violations of Assumption 2.2.1.
The desire to assess randomization with respect to covariates
 







14: To see why this is a problem, consider the extreme case of con-ditioning on the post-treatment ob-served outcome 𝑌. In this case we can find that 𝜋 𝑌 = 0, even when
there is a treatment effect. In a less
extreme case, conditioning on post-treatment variables related to the outcome can "control-away" part of the effect, diminishing estimates.
 
motivates the following diagnostic procedure.	For random variables 𝐴 and 𝐵, 𝐴
𝐵 denotes that 𝐴 and 𝐵 have the
same distribution.

















Under Assumption 2.2.1, Theorem 2.1.1 continues to hold, but we now have a stronger result.

 


 
The logic is the same as in Theorem 2.1.1, but now we condition on 𝑊. Consistency gives E[𝑌 | 𝐷 = 𝑑, 𝑊] = E[𝑌(𝑑) | 𝐷 =
𝑑, 𝑊]. Under Assumption 2.2.1, we have 𝐷 ⊥ (𝑌(𝑑), 𝑊), so
conditional on 𝑊 the assignment remains independent of 𝑌(𝑑)
and therefore E[𝑌(𝑑) | 𝐷 = 𝑑, 𝑊] = E[𝑌(𝑑) | 𝑊].

Regression and Statistical Inference for ATEs
Empirical researchers often base statistical inference on the ATE using the classical additive linear regression model, where covariates enter additively in the model. This approach has some good practical properties and often empirically leads to improvements in precision over the simple two-means approach, though this precision improvement is not guaranteed. Another approach that we will emphasize is the interactive regression approach, where de-meaned covariates are also interacted with the base treatment. Including interactions of de-meaned covariates with the treatment always improves precision, and it also allows us to discover treatment effect heterogeneity.
These regression adjustments also preview the logic of ML-powered causal inference. Later in the book we will replace the simple linear predictors used here with flexible machine-learning estimators of conditional expectations, but we will keep the same guiding idea: use the experiment (or an identification strategy) to create orthogonality so that richer prediction tools can improve precision without reintroducing selection bias.

 
Classical Additive Approach
We begin explaining the classical additive approach. Here, to simplify the exposition, we make the strong assumption that the conditional expectation function is exactly linear:
E[𝑌 | 𝐷, 𝑊] = 𝐷𝛼 + 𝛽′𝑋,	(2.2.1)
where 𝑋 = 1, 𝑊 contains an intercept and pre-treatment covariates 𝑊. This setup is clearly restrictive, but the statistical inference result will be valid without this assumption.15 Later
 












15: See Section 2.B for details.
 


 
in the book, we will consider fully nonlinear models. We assume that covariates are centered:16
E[𝑊] = 0.
By Assumption 2.2.1, there is covariate balance:
E[𝑊 | 𝐷 = 1] = E[𝑊 | 𝐷 = 0].
Using centered covariates implies that
E[𝑌(0)] = E[E[𝑌 | 𝐷 = 0, 𝑋]] = 𝛽1
E[𝑌(1)] = E[E[𝑌 | 𝐷 = 1, 𝑋]] = 𝛽1 + 𝛼.
That is, the average outcome in the untreated state is 𝛽1, and the average treatment effect 𝛿 = E[𝑌(1)] − E[𝑌(0)] equals 𝛼.
Equation (2.2.1) implies that
𝑌 = 𝐷𝛼 + 𝛽′𝑋 + 𝜖,	𝜖 ⊥ (𝐷, 𝑋),	(2.2.2) implying that 𝛼 coincides with the coefficient in the BLP of 𝑌
on 𝐷 and 𝑋.
In fact, even if we don’t assume the linearity (2.2.1), we still have that 𝛼 = 𝛿. That is, the projection coefficient 𝛼 recovers the ATE 𝛿 without the linearity assumption as we detail in
Section 2.B. Furthermore the statistical inference result stated below will hold without requiring linear conditional expectation functions as it is simply a statement about inference on the BLP coefficient.
We are interested in statistical inference on the ATE and Relative ATE17
𝛼	and	𝛼/𝛽1.
Under regularity conditions, application of the OLS theory from Chapter 1 gives us
 


16: Theoretically, this is imple-mented by redefining 𝑊 := 𝑊
E 𝑊 . In estimation, this is imple-
mented by redefining 𝑊𝑖 := 𝑊𝑖
𝔼𝑛 𝑊 . Recentering by empirical
means is asymptotically equivalent to recentering by the true means. This is true here but is not true more generally. This can be verified using the concept of Neyman orthogonal-ity that we develop later.
























17: Relative ATE is often called lift
in business applications.
 
( √𝑛(𝛼ˆ − 𝛼), √𝑛(𝛽ˆ1 − 𝛽1)	∼ 𝑁(0, V),
where covariance matrix V has components:

 
V	= E[𝜖2𝐷˜ 2] , V
 
= E[𝜖21˜2] , V
 
= V	=	E[𝜖2𝐷˜ 1˜] ,
 
(E[𝐷˜ 2])2
 
(E[1˜2])2
 
E[1˜2]E[𝐷˜ 2]
 
where 𝐷˜ = 𝐷 − E[𝐷] is the residual after partialling out 𝑋 from
𝐷 linearly and 1˜ := 1	𝐷  is the residual after partialling out
𝐷 and 𝑊 from 1.
 


We also obtain the approximate normality for the Relative ATE using the delta method:
 


where
 
√𝑛(𝛼ˆ/𝛽ˆ1 − 𝛼/𝛽1) a 𝑁(0, 𝐺′V𝐺),
 
𝐺 = [1/𝛽1, −𝛼/𝛽2]′.

 
Improvement in Precision under Linearity
Now we explain the role of covariates in potentially delivering improvements in precision of estimating the ATE. The under-lying idea is that of "denoising." This improvement, however, hinges on the linear model (2.2.1). In the next section, we will obtain improvement without linearity assumptions.
We consider what happens when we do not include covariates in the regression. In this case, the OLS estimator 𝛼¯ estimates the projection coefficient 𝛼 in the BLP using (1, 𝐷) alone:18
𝑌 = 𝛼𝐷 + 𝛽1 + 𝑈,  E[𝑈] = E[𝑈𝐷] = 0, where the noise
𝑈 = 𝛽′(𝑋 − E[𝑋]) + 𝜖
contains the part of 𝑌 that is linearly predicted by 𝑋, 𝛽′(𝑋 −
E[𝑋]) = 𝛽′𝑋 − 𝛽1. We then have that 𝛼¯ obeys
 











18: Here 𝑈 = 𝑌 − 𝛼𝐷 − 𝛽1 obeys
E[𝑈 | 𝐷 = 𝑑] = E[𝑌(𝑑) − 𝛼𝑑 − 𝛽1 | 𝐷 = 𝑑]
= E[𝑌(𝑑) − 𝛼𝑑 − 𝛽1] = 0,
invoking random assignment and the definition of 𝛼 and 𝛽1.
 
√ 	a	¯
 
¯	E[𝑈2𝐷˜ 2]
 
𝑛(𝛼¯ − 𝛼) ∼ 𝑁(0, V11),
 
V11 =
 
(E[𝐷˜ 2])2 .
 
Under the linear model (2.2.1), it follows that
V11 ≤ V¯ 11 ,

with the inequality being strict ("<") if Var 𝛽′𝑋 > 0.19 That is, under (2.2.1), using pre-determined covariates improves the precision of estimating the ATE 𝛼.
However, this improvement theoretically hinges on the cor-rectness of the additive linear model. In contrast, statistical inference on the ATE based on the normal approximation provided above remains valid without this assumption ( as long as robust standard errors are used). We default to Eicker–Huber–White standard errors because the regression model is a working approximation, while identification comes from random assignment.20 However, the precision can be either
 





19: Verify this as a reading exercise.






20: We always use robust vari-ance formulas throughout the book. However, the default inferential al-gorithms in R and Python often report the classical Student’s for-mulas as variances, which critically rely on the linearity assumption.
 


higher or lower than that of the classical two-sample approach without covariates. That is, without (2.2.1), V11 and V¯ 11 are not generally comparable.


 
The Interactive Approach: Always Improves Precision and Discovers Heterogeneity
We can also consider estimation of CATE through the lens of an interactive linear regression model, which interacts treatment indicator 𝐷 with regressors 𝑋 constructed from original raw regressors 𝑊. Including these interactions respects the logic of approximating the conditional expectation of 𝑌 given 𝐷 and raw regressors using linear functional forms. To simplify exposition, we first assume that the interactive model is exactly correct for the CEF:
E[𝑌 | 𝐷, 𝑊] = 𝛼′𝑋𝐷 + 𝛽′𝑋.	(2.2.3) However, this approach works without this assumption.
As before, we assume
𝑋 = (1, 𝑊′)′,	E[𝑊] = 0,
which can be achieved in practice by recentering.21 Here, we recover CATE via
𝛿(𝑊) = E[𝑌(1) | 𝑊] − E[𝑌(0) | 𝑊]
= E[𝑌 | 𝐷 = 1, 𝑊] − E[𝑌 | 𝐷 = 0, 𝑊] = 𝛼′𝑋.
Using that E𝑊 = 0, the ATE is then
𝛿 = E[𝛿(𝑊)] = E[𝛼′𝑋] = 𝛼1,
where 𝛼1 is the first component of 𝛼. The function 𝛼′2𝑊, where
𝛼2 is the vector all elements of 𝛼 excluding 𝛼1, therefore de-
scribes the deviation of CATE away from the ATE.
We can verify that 𝛼 is the coefficient of the linear projection equation:
 























21: In this interactive model, using re-centering by empirical means is not equivalent to using re-centering true means. This requires adding additional variance term to the con-ventional variance output of least squares, which is variance of CAPE
𝑋𝑖′𝛼.
 
𝑌 = 𝛼′𝐷𝑋 + 𝛽′𝑋 + 𝜖,  𝜖 ⊥ (𝑋, 𝐷𝑋).
 


 
Therefore, we can treat
 

𝐷¯
 

:= 𝐷𝑋
 
as a vector of technical treatments22 and invoke the "partialling out" approach for inference on components of 𝛼.
 
22: A technical treatment refers to any variable obtained as a trans-formation of the original treatment variable.
 







 
Reemployment Bonus RCT
Here we re-analyze the Pennsylvania re-employment bonus experiment [12], which was conducted in the 1980s by the U.S. Department of Labor to test the incentive effects of alternative compensation schemes for unemployment insurance (UI). In these experiments, UI claimants were randomly assigned either to a control group or one of five treatment groups. We focus our discussion on treatment group 4. In the control group the current rules of the UI applied. Individuals in the treatment groups were offered a cash bonus if they found a job within some pre-specified period of time (qualification period), provided that the job was retained for a specified duration; see the Penn Data Codebook for further details on the data.
We consider the
▶ classical 2-sample approach, no adjustment (CL)
▶ classical linear regression adjustment (CRA)
▶ interactive regression adjustment (IRA)
▶ interactive regression adjustment with double lasso (par-tialling out by lasso) (IRA-DL)
We use the last approach in the spirit of exploration and ex-perimentation. We describe the last approach and establish its validity in Chapter 4.
Estimates of the ATE on (log) unemployment duration and corresponding estimated standard errors are given in Table 2.2.
The different estimators deliver fairly similar point estimates suggesting that treatment group 4 experiences an average de-crease in unemployment duration of around 8%. The three regression estimators deliver estimates that are slightly more
 


The Notebooks 2.6.2 explore the use of covariates to improve preci-sion and learn about heterogeneity in a Reemployment Bonus RCT.
 


Table 2.2: Estimates of the ATE of the reemployment bonus on log unemployment duration.




 
precise (have lower standard errors) than the simple difference in means estimator.23
We also see that the regression estimators offer slightly lower estimates of the ATE than the difference in means estimator. These differences likely occur due to minor imbalances in the treatment allocation: People older than 54 tended to receive the treatment more than other groups of qualified UI claimants during the later period of the experiment. Loosely speaking, the regression estimators try to correct for this imbalance by "partialling out" the effect of this oversampling24 and averaging over differences net of these "imbalancing" effects. We will explain how regression adjustment corrects for imbalances in Chapter 5.

2.3	Drawing RCTs via Causal Diagrams
RCTs can be represented using causal diagrams, which clearly display the assumptions underlying our treatment effect model. Causal diagrams were first introduced by Sewall Wright in the 1920s ([13], [14]) and later formalized by Judea Pearl and James
M. Robins ([15], [16]).
In these diagrams, nodes represent random variables, and arrows indicate causal effects (and related statistical dependen-cies). In observational data, treatment status can be confounded: unobserved factors 𝑈 may affect both the treatment 𝐷 and the outcome 𝑌, so a simple comparison of treated and untreated mixes causation and selection. In an RCT, by contrast, 𝐷 is assigned independently of all pre-treatment factors, observed or unobserved. Figure 2.2 contrasts these two situations.
Figure 2.3 extends panel (b) of Figure 2.2 by including potential outcomes as nodes.
In Figure 2.3, the potential outcomes 𝑌 𝑑 are represented as a single node influenced by 𝑊 (indicated by the arrow from 𝑊 to
𝑌 𝑑 ). The absence of an arrow from 𝐷 to 𝑌 𝑑 indicates that the treatment assignment is independent of the potential outcomes. The arrow from the deterministic node 𝑑 to 𝑌 𝑑 captures the causal dependency inherent in the potential outcome process.
 

23: The standard errors are com-puted using the standard Eicker-Huber-White formula (HC0). Sur-prisingly, IRA performs worse than CL when we use the HC1 formula, which multiplies the HC0 standard errors by a factor of 𝑛 𝑛 𝑝 , where
𝑝 is the total number of regressors in the model. This adjustment is heuristic and is motivated to cap-ture the effect of overfitting, assum-ing a homoscedastic model.
24: See the Reemployment Bonus RCT Notebooks 2.6.2 for the results from the balance check.
 


 

𝑈

 
𝐷	𝑌

 
𝑊
(a)	Observational
 


𝐷	𝑌

 
𝑊	𝑈
(b)	RCT
 
Figure 2.2: Causal diagrams con-trasting observational confounding with an RCT. (a) In observational data, an unobserved confounder 𝑈 may affect both treatment 𝐷 and outcome 𝑌, so differences between treated and untreated mix causa-tion and selection. (b) In an RCT, 𝐷 is assigned independently of pre-treatment factors such as 𝑊 and 𝑈; the missing arrows into 𝐷 encode random assignment.
 

 
𝐷	𝑑   𝑌(𝑑)
 
𝑊


Together, the process 𝑑 ↦→ 𝑌(𝑑) and the treatment assignment
𝐷 determine the realized outcome via 𝑌 := 𝑌(𝐷).
We further develop these concepts and employ causal diagrams as a formal tool in Chapters Chapter 7 and Chapter 11.

2.4	Key Limitations of RCTs
We outline key limitations of RCTs. First, we discuss identifi-cation threats by examining settings in which the Stable Unit Treatment Value Assumption (SUTVA) may fail, and the result-ing implications for inference. We then address ethical, practical, and generalizability concerns.
Even when the assignment mechanism is random, internal validity can be threatened if assignment does not translate into receipt (noncompliance), if outcomes are missing differentially by treatment (attrition), or if measurement error depends on treatment status (for example, treated units are monitored more closely). These issues can reintroduce selection and typically require additional design choices or analytical adjustments.

Externalities, Stability, and Equilibrium Effects
Rubin’s causal model relies on SUTVA (see Section 2.1), which requires that one unit’s potential outcomes remain unaffected by
 
Figure 2.3: Extending the RCT dia-gram with potential outcomes: the deterministic node 𝑑 selects the rel-evant counterfactual 𝑌 𝑑 , and the absence of an arrow from 𝐷 to 𝑌 𝑑 encodes independence of assign-ment from potential outcomes.
 


 
others’ treatment assignments [17]. However, there are settings where this assumption may not hold.
For instance, in vaccine trials SUTVA holds when treatment and control groups are "small" (infinitesimal) subpopulations of the general population, allowing us to measure average vaccine effects. If a large fraction of the population is vaccinated—thus achieving herd immunity—the outcomes for the control group may mirror those for the treated, and SUTVA fails.25
In economics, such spillover effects are called externalities or, in some cases, general equilibrium effects. For example, if only a small subpopulation earns a college degree, general equilibrium wage effects are minimal. Conversely, if many obtain a degree, the equilibrium wage may adjust (reducing the college wage premium). Similarly, in large-scale training programs, an individual’s outcomes may depend on the number of people trained for the same job.

Ethical, Practical, and Generalizability Concerns
Many RCTs are infeasible because implementing them can be unethical. The 1978 Belmont Report ([18]) outlines ethical princi-ples—"Respect for persons," "Beneficence," and "Justice"—that govern research with human subjects and are enforced by in-stitutional review boards. For example, a hypothetical RCT assigning individuals to a smoking treatment would violate "Beneficence" by causing harm, rendering such studies unethi-cal.
RCTs may also encounter practical issues. They can be pro-hibitively expensive when treatment costs, data collection, or the necessary sample size for adequate power are high. Long-term RCTs are particularly challenging due to attrition, and randomizing access to a desirable treatment may be politically unfeasible.
Even when successfully implemented, RCT findings may be difficult to generalize. Local conditions or differences in imple-mentation and intervention scale may limit the external validity of the estimated average treatment effect.

2.5	Notes
RCTs have had a profound influence on business, economics, and science. They are standard in testing drug efficacy and
 









25: Because SUTVA does not hold in the vaccination context, it is cus-tomary to use relative measures of impact like "vaccine efficiency" because they may be a somewhat more stable measure when general-izing from "small" treated subpop-ulations to a "large" treated popu-lation.
 


 
various programs in labor and development economics. The FDA adopted RCTs as the gold standard in the 1970s–80s, and in the tech industry and marketing, RCTs are known as "A/B Tests" and are widely used. Many major tech companies have dedicated experimental platforms.26
The expansion of experimentation in economics is linked to the work of Richard Thaler (2017 Alfred Nobel Memorial Prize in Economics), Abhĳit Banerjee, Esther Duflo, and Michael Kremer (2019 Alfred Nobel Memorial Prize in Economics), John List, among others.
For real examples of how RCTs are done and designed in prac-tice, see, for example, the FDA registry of RCTs, the American Economic Association’s registry of RCTs in economics, or the The Poverty Action Lab.
We have presented basic ideas here. For a detailed analysis of experimental design, please see Art Owen’s lecture notes ([19]). For further reading on RCTs and causal analysis, refer to Imbens and Rubin [2] and Duflo et al. [20].

2.6	Notebooks
Notebook 2.6.1 (Vaccination RCT) Vaccination RCT R Note-book and Vaccination RCT Python Notebook contain the analysis of vaccination examples.

Notebook 2.6.2 (Reemployment Bonus) Reemployment Bonus RCT R Notebook and Reemployment Bonus RCT Python Note-book explore the use of covariates to improve precision and learn about heterogeneity in a Reemployment Bonus RCT.


2.7	Checklist
After reading this chapter, you should be able to:
▶ Explain’s Rubin’s framework for potential outcomes 𝑌 1 and 𝑌 0 , explain the fundamental problem of causal inference, and define the ATE 𝛿 = E 𝑌 1	𝑌 0 .
▶ Distinguish association from causation by comparing the
average predictive effect 𝜋 = E 𝑌  𝐷 = 1  E 𝑌  𝐷 = 0 to 𝛿, and explain selection bias as the reason 𝜋 ≠ 𝛿 in observational data.
 





26: See, for example, ExP platform at Microsoft and the WebLab plat-form at Amazon.
 


▶ State the core RCT assumptions (consistency, no interfer-ence, and random assignment) and explain why random-ization removes selection bias (Theorem 2.1.1).
▶ Estimate 𝛿 in an RCT using the difference in sample means
and construct a confidence interval using large-sample normal approximations.
▶ Use pre-treatment covariates 𝑊 to define CATEs 𝛿 𝑊 , ex-
plore heterogeneity, and improve precision via regression adjustment (classical and interactive approaches).
▶ Draw and interpret causal diagrams for RCTs, and explain key limitations of RCTs (spillovers/interference, equilib-rium effects, ethics/practicality, and external validity).

2.8	Exercises
Exercise 2.8.1 (Selection Bias) Set up a simulation experiment that illustrates the contrived smoking example, following the analytical example we’ve presented in the text. Illustrate the difference between estimates obtained via an RCT (smoking generated independently of potential outcomes) and an ob-servational study (smoking choice is correlated with potential outcomes).

Exercise 2.8.2 (Vaccinations RCT) Study the notebook on vaccinations RCTs. Try to replicate the results in the FDA briefing table for age group 18-64 (exact replication is not required). Explain your calculations.

Exercise 2.8.3 (Reemployment example) Study the notebook on the reemployment example. Experiment with putting even more flexible controls (e.g. use extra interactions of some controls). Experiment with using HC0 vs. HC1 standard errors. Report and explain your findings.

Exercise 2.8.4 (RCT Design) Skim over the information on the Pfizer RCT design briefing. Write down one paragraph summarizing the study design.

Exercise 2.8.5 (AEA RCT Registry) Skim over one of the RCTs registered with AEA RCT Registry. Write down one paragraph summarizing the study design.
 


Exercise 2.8.6 (Stability) Think of some RCTs where stability is likely to hold and some RCTs where it likely does not.


2.A		Approximate Distribution of the Two Sample Means
To demonstrate the result in the text, we note that
𝔼𝑛[(𝑌(𝑑) − E𝑌(𝑑))1(𝐷 = 𝑑)]
𝔼𝑛[1(𝐷 = 𝑑)]
for 𝑑	0, 1 because we can re-write the population group average as
𝜃𝑑 = E[𝑌(𝑑)] = E[𝑌(𝑑)] 𝔼𝑛[1(𝐷 = 𝑑)] .

Hence, for each 𝑑 ∈ {0, 1},
√𝑛(𝜃ˆ𝑑 − 𝜃𝑑) = √𝑛 𝔼𝑛[(𝑌(𝑑) − E𝑌(𝑑))1(𝐷 = 𝑑)].

By the law of large numbers, 𝔼𝑛 1 𝐷 = 𝑑	P 𝐷 = 𝑑 ; so we have the approximation

 
√𝑛{𝜃ˆ𝑑 − 𝜃𝑑 }𝑑
 

∈{0,1}
 
√ 𝔼𝑛[(𝑌(𝑑) − E𝑌(𝑑))1(𝐷 = 𝑑)]
P(𝐷 = 𝑑)
 
Note that the terms being averaged are
(𝑌𝑖(𝑑) − E[𝑌(𝑑)])1(𝐷𝑖 = 𝑑) .
P(𝐷 = 𝑑)
These terms have zero mean27 and variance
E[(𝑌(𝑑) − E[𝑌(𝑑)])21(𝐷 = 𝑑)2] = Var(𝑌 | 1(𝐷 = 𝑑) = 1) .
 






27: Why? Hint: Use the law of iter-ated expectations.
 
P(𝐷 = 𝑑)2
Also note the zero covariance:
 
P(𝐷 = 𝑑)
 

P(𝐷 = 1)	P(𝐷 = 0)
The application of the central limit theorem then yields the claimed result.
 

2.B		Statistical Properties of the Classical Additive Approach★
Here we analyze statistical inference on ATE using OLS and adjusting for 𝑋 = 1, 𝑊 , without making the linearity assump-tions we made in Section 2.2.
We consider the linear projection equation in the population:
𝑌 = 𝐷𝛼 + 𝑋′𝛽 + 𝜖,	𝜖 ⊥ (𝐷, 𝑋).
Here, we have that 𝐷 and 𝑋 = (1, 𝑊) with E[𝑊] = 0, so that
𝛽′𝑋 = 𝛽1 𝛽′2𝑊. Moreover, we have that 𝐷 𝑊 in the RCT setting.
First, we’d like to verify that 𝛼 = E[𝑌(1)] − E[𝑌(0)] and 𝛽1 =
E[𝑌(0)]. For 𝑈 := 𝛽′2𝑊 + 𝜖, we can write
𝑌 = 𝐷𝛼 + 𝛽1 + 𝑈,	𝑈 ⊥ (1, 𝐷).
𝑈  1, 𝐷 holds because 1, 𝐷   𝑊 , 𝜖 using that E 𝑊 = 0 and that 𝐷   𝑊 , 𝜖 . Therefore, 𝐷𝛼  𝛽1 coincides with the
population projection of 𝑌 onto 1, 𝐷 . Hence, the projection coefficients are the same as those obtained by the 2-sample approach in the population. Therefore, 𝛽1 = E[𝑌(0)] and 𝛼 =
E[𝑌(1)] − E[𝑌(0)].
Second, we’d like to explain the details of the approximate normality for the estimators of sample OLS coefficients 𝛽ˆ1. The OLS theory of the first chapter implies that the OLS estimator
𝛼ˆ obeys

 
√𝑛 𝛼	𝛼	√𝑛 𝔼𝑛[𝜖𝐷˜ ]
𝔼𝑛[𝐷˜ 2]
 
a 𝑁(0, V11),
 
where 𝐷˜ = 𝐷 − E[𝐷] is the residual after partialling out 𝑋 from
 
𝐷 linearly,28 and
 


V11
 
= E[𝜖2𝐷˜ 2] .
(E[𝐷˜ 2])2
 
28: Derive that 𝐷˜ = 𝐷 E 𝐷 from Assumption 2.2.1.
 
Applying the same theory for 𝛽1 (the intercept coefficient), yields29
 

29: To explain the derivation, note that by partialling out 𝐷 and 𝑊
 
√  ˆ	√
 
𝔼𝑛[𝜖1˜] a
 
(recall that 𝑋 = (1, 𝑊)) from 1 and
 
𝑛(𝛽1 − 𝛽1) ≈
 

 
𝑛
𝔼𝑛
 
[1˜2]
 
∼ 𝑁(0, V22),
 
𝑌, we obtain
𝑌˜ = 𝛽11˜ + 𝜖;
 

1˜ := (1 − 𝐷).
 
where 1˜ := (1 − 𝐷) is the residual after partialling out 𝐷 and 𝑋
 

The projection of 1 on 𝐷 and 𝑊 is given by 𝐷 since 𝐷 is binary and we’ve assumed E[𝑊] = 0.
 


 
from 1 and
 


V22
 
= E[𝜖21˜2] .
(E[1˜2])2
 
We can also establish that the estimators are jointly approxi-mately normal with covariance

 

V12
 
=	E[𝜖2𝐷˜ 1˜] .
E[1˜2]E[𝐷˜ 2]
 


Bibliography



[1]	Donald B. Rubin. ‘Estimating causal effects of treatments in randomized and nonrandomized studies.’ In: Journal of Educational Psychology 66.5 (1974), pp. 688–701 (cited
on page 44).
[2]	Guido W. Imbens and Donald B. Rubin. Causal Inference in Statistics, Social, and Biomedical Sciences. Cambridge University Press, 2015 (cited on pages 45, 62).
[3]	Tyler J. VanderWeele, Guanglei Hong, Stephanie M. Jones, and Joshua L. Brown. ‘Mediation and Spillover Effects in Group-Randomized Trials: A Case Study of the 4Rs Educational Intervention’. In: Journal of the American Sta-tistical Association 108.502 (2013), pp. 469–482. (Visited on 02/17/2024) (cited on page 45).
[4]		Peter M. Aronow and Cyrus Samii. ‘Estimating average causal effects under general interference, with application to a social network experiment’. In: The Annals of Applied Statistics 11.4 (2017), pp. 1912 –1947. doi: 10.1214/16-AOAS1005 (cited on page 45).
[5]		Michael P. Leung. ‘Treatment and Spillover Effects Under Network Interference’. In: The Review of Economics and Statistics 102.2 (2020), pp. 368–380 (cited on page 45).
[6]	Francis J. DiTraglia, Camilo García-Jimeno, Rossa O’Keeffe-O’Donovan, and Alejandro Sánchez-Becerra. ‘Identify-ing causal effects in experiments with spillovers and non-compliance’. In: Journal of Econometrics 235.2 (2023),
pp. 1589–1624. doi: https : / / doi . org / 10 . 1016 / j . jeconom.2023.01.008 (cited on page 45).
[7]		Gonzalo Vazquez-Bare. ‘Identification and estimation of spillover effects in randomized experiments’. In: Journal of Econometrics 237.1 (2023), p. 105237. doi: https:// doi.org/10.1016/j.jeconom.2021.10.014 (cited on page 45).
[8]		Walter A Orenstein, Roger H Bernier, Timothy J Dondero, Alan R Hinman, James S Marks, Kenneth J Bart, and Barry Sirotkin. Field evaluation of vaccine efficacy / Walter A. Orenstein ... [et al.] 1984 (cited on page 51).
 
Bibliography	68


[9]		Jerome Cornfield. ‘A statistical problem arising from retrospective studies’. In: Proceedings of the Third Berkeley Symposium on Mathematical Statistics and Probability. Vol. 4. University of California Press Berkeley, CA. 1956, pp. 135–148 (cited on page 52).
[10]	Winston Lin. ‘Agnostic notes on regression adjustments to experimental data: Reexamining Freedman’s critique’. In: Annals of Applied Statistics 7.1 (2013), pp. 295–318 (cited on page 58).
[11]		Max Cytrynbaum. ‘Covariate adjustment in stratified ex-periments’. In: Quantitative Economics 15.4 (2024), pp. 971–
998 (cited on page 58).
[12]	Yannis Bilias. ‘Sequential testing of duration data: The case of the Pennsylvania ‘reemployment bonus’ exper-iment’. In: Journal of Applied Econometrics 15.6 (2000),
pp. 575–594 (cited on page 58).
[13]	Philip G. Wright. The Tariff on Animal and Vegetable Oils. New York: The Macmillan company, 1928 (cited on page 59).
[14]		Sewall Wright. ‘Correlation and Causation’. In: Journal of Agricultural Research 20.7 (Jan. 1921), pp. 557–585 (cited
on page 59).
[15]	Judea Pearl. ‘Causal diagrams for empirical research’. In:
Biometrika 82.4 (1995), pp. 669–688 (cited on page 59).
[16]		Sander Greenland, Judea Pearl, and James M. Robins. ‘Causal diagrams for epidemiologic research’. In: Epidemi-ology 10.1 (1999), pp. 37–48 (cited on page 59).
[17]	David R. Cox. Planning of experiments. Wiley, 1958 (cited on page 61).
[18]		The Belmont report: Ethical principles and guidelines for the protection of human subjects of research. Tech. rep. Na-tional Commission for the Protection of Human Subjects of Biomedical and Behavioral Research, 1978 (cited on page 61).
[19]	Art Owen. ‘A First Course in Experimental Design: Notes from Stat 263/363’. Lecture notes. Accessed 1/17/2024. 2020 (cited on page 62).
[20]		Esther Duflo, Rachel Glennerster, and Michael Kremer. ‘Using randomization in development economics re-search: A toolkit’. In: Handbook of Development Economics 4 (2007), pp. 3895–3962 (cited on page 62).
 

 
Predictive Inference via Modern High-Dimensional Linear
Regression


"Il semble que la perfection soit atteinte non quand il n’y a plus rien à ajouter, mais quand il n’y a plus rien à retrancher."
(It seems perfection is attained not when there is no longer anything to add, but when there is no longer anything to take away.)
– Antoine de Saint-Exupéry [1].
In this chapter we learn how to build prediction rules when our feature set is so rich that 𝑝 can be comparable to, or even larger than, 𝑛. To keep the ideas concrete, we will return throughout to a empirical example: predicting log-wages in CPS 2015. We start from a transparent baseline specification and then enrich it with nonlinear terms and interactions, quickly generating high-dimensional constructed regressors. At that point ordinary least squares can fit the training sample very well and yet generalize poorly. Penalized regression methods resolve this tension by trading a small amount of in-sample fit for a large reduction in overfitting risk. We begin with Lasso and Post-Lasso, then take a short tour of Ridge, Elastic Net, and Lava, and end by comparing methods on an honest train/test split.
 
3
3.1	Linear Regression with High-Dimensional Covari-ates	70
The Framework	70
Lasso	72
Quick Heuristics for Lasso Properties and Penalty Choice★	77
OLS Post-Lasso	78
3.2	Predictive Performance of Lasso and Post-Lasso	79
3.3	A Helicopter Tour of Other Penalized Regression Methods for Prediction	82
3.4	Choice of Regression Meth-ods in Practice	89
3.5	Notes	91
3.6	Notebooks	91
3.7	Checklist	92
Checklist	92
3.8	Exercises	92
3.A	Additional Discussion and Results	93
Iterative Estimation of 𝜎 93
Some Lasso Heuristics via Convex Geometry★	94
Other Variations on Lasso95
3.B	Cross-Validation	96
3.C	Laws of Large Numbers for Large Matrices★	98
3.D	A Sketch of the Lasso Guar-antee Under Exact Sparsity★99
 

3.1	Linear Regression with
High-Dimensional Covariates
The Framework
We consider a regression model
𝑌 = 𝛽′𝑋 + 𝜖,	𝜖 ⊥ 𝑋,
Here and throughout, the notation 𝜖  𝑋 denotes orthogonal-ity, meaning E 𝑋𝜖 = 0 (componentwise);1 where 𝛽′𝑋 is the population best linear predictor of 𝑌 using 𝑋, or simply the population linear regression function. The vector 𝑋 = (𝑋𝑗)𝑝
 


















1: This is much weaker than the statistical independence, 𝜖 ⊥ 𝑋.
 
is 𝑝-dimensional. That is, there are 𝑝 regressors, and
𝑝 is large, possibly much larger than 𝑛.

 
This case where 𝑝 is large relative to the sample size is what we call a high-dimensional setting. High-dimensional settings arise when
▶ data have large dimensional features (i.e. many covariates are available for use as regressors),
▶ we construct many technical regressors2 from raw re-
gressors, or
▶ both.
Examples of datasets where many covariates are available and potential corresponding exemplary applications include
▶ country characteristics in cross-country wealth analysis,
▶ housing characteristics in house pricing/appraisal analy-sis,
▶ individual health information in electronic health records and claims data, and
▶ product characteristics at the point of purchase in demand analysis,
▶ embeddings, which are covariate characteristics gener-ated by AI models, as we discuss in Chapter 10.
 







2: Recall, a technical regressor or con-structed regressor is any variable ob-tained as a transformation of a basic regressor.
 







Figure 3.1: An example of AI-driven feature generation: In Bajari et al [2], constructed features 𝑋 are derived from neural embeddings of a product’s image and text, specifi-cally using ResNet50 for the image and BERT for the text.

 
Another source of high-dimensionality is the use of constructed features or regressors of the form
𝑋 = 𝑇(𝑊) = (𝑇1(𝑊), ..., 𝑇𝑝(𝑊))′,
where 𝑊 denotes original raw regressors. As we discussed in Chapter 1, the set of transformations 𝑇 𝑊 is sometimes called the dictionary of transformations. Example transformations in-clude polynomials, splines, interactions between variables, and applying functions such as the logarithm or exponential. In the wage analysis in Chapter 1, for example, we used quadratic and cubic transformations of experience, as well as interactions (products) of these regressors with education and geographic indicators. Recall that the main motivation for the use of con-structed regressors is to build more flexible and potentially better prediction rules.
The potential for improved prediction arises because we are using prediction rules 𝛽′𝑋 = 𝛽′𝑇 𝑊 that are nonlinear in the original raw regressors 𝑊 and may thus capture more complex
patterns that exist in the data. Conveniently, the prediction rule
𝛽′𝑋 is still linear with respect to the parameters, 𝛽, and with re-spect to the constructed regressors 𝑋 = 𝑇 𝑊 , so inherits much from the previous discussion of linear regression provided in
Chapter 1.
 

 
Figure 3.2: A schematic represen-tation of the embedding process using Nano-Banana. The resulting embeddings are depicted as dia-monds emerging from the AI brain, symbolizing their high value across various empirical applications. Re-markably, we didn’t ask for dia-monds in our prompt.
 

In summary, we have provided two motivations for using high-dimensional regressors in prediction:
▶ The first motivation is that modern datasets have high-dimensional features that can be used as regressors.
▶ The second motivation is that we can use nonlinear transformations of features or raw regressors and their interactions to form constructed regressors. Using transformations allows us to better approximate the best prediction rule – the conditional expectation of
 


the outcome given raw regressors.

Lasso
Recall that we are considering a regression model

 
𝑝
𝑌 = 𝛽′𝑋	𝜖 =	𝛽𝑗 𝑋𝑗	𝜖,	𝜖	𝑋	(3.1.1)
𝑗=1
where 𝑝 is possibly much larger than 𝑛.
We further assume that regressors are normalized,
E[𝑋2] = 1,
to discuss theoretical properties.3 However, the estimation algorithms provided are stated without assuming this normal-ization.
Classical linear regression or least squares fails in these high-dimensional settings because it overfits in finite samples. Intu-itively, overfitting refers to using patterns that are idiosyncratic to a specific dataset and do not generalize out of sample. That is, it corresponds to using a prediction rule that is overly complex in that it uses patterns that help explain a given dataset, increas-ing in-sample measures of fit, but are not present in different data even if the data are drawn from the same population, potentially harming out-of-sample prediction performance.
The potential for classical linear regression estimated with least squares to overfit is especially apparent when 𝑝  𝑛. In this case, conventional least squares will perfectly fit the data regardless of the value of 𝛽 as long as the covariate matrix is rank 𝑛.4 We therefore make some assumptions and modify the regression method to deal with cases where 𝑝 is large.
An intuitive starting point is the assumption of approximate sparsity. Under approximate sparsity, there is a small group of regressors with relatively large coefficients whose use alone suffices to approximate the BLP 𝛽′𝑋 well. The rest of the re-gressors are assumed to have relatively small coefficients; if we work with a sparse approximation that keeps only the largest coefficients, the contribution of the remaining regressors shows up as approximation errors.
An example of approximate sparsity is captured by regression coefficients of the form5
 










3: This normalization is important to discuss the concept of approxi-mate sparsity.
















4: Recall that we illustrated the problem with overfitting in Section 1.3.











5: The notation reads as "propor-tional to."
 

𝛽𝑗 ∝ 1/𝑗2,	𝑗 = 1, ..., 𝑝.
Here, the first few coefficients capture almost all the explanatory power of the full vector of coefficients as shown in Figure 3.3.

 
 
5	10	15	20

j

Next, we define approximate sparsity formally.
 
Figure 3.3: Example of regression
coefficients, 𝛽𝑗 = 1 𝑗2 that satisfy approximate sparsity.
 

 
To connect Definition 3.1.1 to the example 𝛽𝑗  1 𝑗2, note that the tail of the sorted coefficients decays quickly: if we keep only the largest 𝑠 coefficients and set the rest to zero, the remaining discrepancy is small. This is the sense in which a high-dimensional linear predictor can behave as if it had a much smaller effective dimension.
For estimation purposes, we have a random sample {(𝑌𝑖 , 𝑋𝑖)}𝑛  .
 
We seek to construct a good linear predictor 𝛽ˆ′𝑋, which works
well when 𝑝/𝑛 is not small.
Before defining the Lasso problem, it is important to note that we are treating all variables as centered and thus do not include an intercept in the model.6 In practice, this construction means that, for raw variables 𝑌∗ and 𝑋∗, we start by defining demeaned versions of these variables 𝑌 = 𝑌∗ 𝔼𝑛 𝑌∗ and 𝑋 = 𝑋∗ 𝔼𝑛 𝑋∗ for use in estimation of model parameters. We note that the
centered model (3.1.1) is equivalent to starting with the model
𝑌∗ = 𝛼 + 𝛽′𝑋∗ + 𝜖	𝜖 ⊥ 𝑋∗
 





6: A centered random variable 𝑈 has E[𝑈] = 0, and a centered vari-able 𝑈 in a sample has 𝔼𝑛[𝑈] = 0.
 


with intercept 𝛼 = E 𝑌∗  𝛽′E 𝑋∗ . For estimates 𝛽ˆ obtained by estimating (3.1.1), we can thus recover an estimate of 𝛼 as
𝛼ˆ = 𝔼𝑛[𝑌∗] − 𝛽ˆ′𝔼𝑛[𝑋∗].
As mentioned earlier, we will further assume that regressors are normalized, E 𝑋2 = 1. We do state the estimation algo-rithms without assuming this normalization. The combination of centering and normalization – standardization – is commonly employed in practice and is done by default in many software
packages.














 



The penalty loadings are typically set as
𝜓ˆ 𝑗 = J𝔼𝑛[𝑋2].
The use of this penalty ensures invariance of Lasso predictions to rescaling 𝑋𝑗. Note that many software packages implement the Lasso with simple penalty loadings 𝜓ˆ 𝑗 = 1. In such cases,
the use of standardized variables produces the same results as
using these penalty loadings.
As long as 𝜆 > 0, the introduction of the penalty term in (3.1.3) leads to a prediction rule which is less complex than the rule that would be obtained via solving the unpenalized least squares problem. Specifically, the penalty term in the Lasso problem,
 
Rather than work with centered variables, we could equivalently de-fine (3.1.3) with an intercept where the intercept does not enter the penalty function. The important thing to keep in mind is that it is rarely appropriate to penalize the intercept.
 
𝑝
𝑗=1
 
|𝑏𝑗 |𝜓ˆ 𝑗 , provides a measure of complexity of a regression
 
model in terms of the overall magnitude of the coefficients. When 𝜆 is positive, minimizing the Lasso problem requires trading off in-sample fit with this measure of complexity. As a result, the overall magnitude of the estimated coefficients, as measured by the penalty term, will be smaller than the overall magnitude of the coefficients absent this penalty. That is, the
 

 
Lasso solution will have coefficients that are "shrunk" towards 0 relative to the unpenalized least squares problem.7
One important benefit of introducing the penalty term is that it helps guard against overfitting by introducing a cost to model complexity. Intuitively, overfitting occurs as a model is made increasingly complex in an effort to make improvements to in-sample fit that are small relative to sampling error and could thus correspond to idiosyncrasies of a specific finite sample. The penalty term imposes a cost to complexity which help keep increases to complexity that have small benefit in terms of improving fit from being made. Through careful choice of
𝜆, we can theoretically guarantee that the Lasso predictor is similar to the optimal predictor, and thus generalizable, even in high-dimensional settings.
A second important feature of Lasso is that it imposes the approximate sparsity condition on the estimated coefficients 𝛽ˆ.
Approximate sparsity is produced because the penalty function in (3.1.3) has a kink at zero which results in the marginal cost of including regressor 𝑋𝑗 (𝜆𝜓ˆ 𝑗 > 0) always being positive when
𝜆 > 0 . Therefore, Lasso includes a regressor 𝑋𝑗 with non-zero coefficient only if its marginal predictive ability is higher than this marginal cost threshold. That is, Lasso does variable selection: The Lasso solution drops any variable (equivalently sets the variable’s coefficient to 0) whose marginal predictive benefit does not exceed the marginal cost of inclusion. We illustrate this variable selection property numerically in Example 3.1.1 below.
It is important to note that Lasso will not generally select the "right" set of variables. Lasso will tend to exclude variables with small, but non-zero population coefficients. Lasso will also tend to fail to select the right variables in settings where the
𝑋 variables are correlated.8  That is, one should not conclude
that Lasso has selected exactly the variables with non-zero coefficients in the population unless one can rule out variables with small, but non-zero coefficients and ensure that variables are all at most weakly correlated.9 This failure does not mean that the Lasso predictions are poor quality, but does mean that care should be taken in interpreting the selected variables. In particular, we should not interpret the selected set as a list of “the causes”; it is simply the subset that best supports prediction under the chosen penalty.

 

7: This overall shrinkage towards zero relative to the unpenalized problem is sometimes referred to as shrinkage bias or regularization bias.




























8: For example, consider a scenario where variable 𝑋1 has coefficient
𝛽1 = 0 but is highly correlated to variables 𝑋2, ..., 𝑋𝑘 that have non-
zero coefficients. It is quite plau-sible that the marginal predictive benefit of including 𝑋1 in the model is very high when 𝑋2, ..., 𝑋𝑘 are not in the model while the marginal predictive benefit of any one of
𝑋2, ..., 𝑋𝑘 is relatively low. In this case, 𝑋1 may enter the Lasso so-lution with a non-zero coefficient while all of 𝑋2, .., 𝑋𝑘 are excluded. This kind of correlation is one moti-vation for Elastic Net, which tends to keep correlated predictors to-gether; see Section 3.3.
9: This inability to select exactly the right regressors is not special to Lasso but shared by all variable selection procedures.
 


 

 
 
5	10	15	20

j

A crucial point for the two Lasso properties that we have discussed is the choice of the penalization parameter 𝜆. A theoretically valid choice is10
√
 
Figure 3.4: The true coefficients
(black) vs. coefficients estimated by Lasso (blue) in Example 3.1.1.



10: Recall that 𝑧𝑡 is such that P((𝑁(0, 1) ≤ 𝑧𝑡 ) = 𝑡.
 
𝜆 = 2 · 𝑐𝜎ˆ 𝑛𝑧1−a/(2𝑝)	(3.1.4)
where 𝜎ˆ ≈ 𝜎 = ✓E[𝜖2] is obtained via an iteration method
 
defined in Appendix 3.A, 𝑐 > 1, and 1 a is a confidence level.11 We can further simplify the choice using Feller’s tail inequality:
 

11: Practical	recommendations, based on theory and that seem to
 
𝑧1−a/(2𝑝)
 
≤ J2 log(2𝑝/a),
 
work well in practice, are to set
𝑐 = 1.1 and a = .05.
 
where the inequality becomes sharp as 𝑝 → ∞.
This penalty level ensures that the Lasso predictor 𝛽ˆ′𝑋 does not overfit the data and delivers good predictive performance under approximate sparsity ([3, 4]). Another good way to pick
 

 
the penalty level when building a model for prediction is by cross-validation ([5]).12

Quick Heuristics for Lasso Properties and Penalty Choice★
Here, we provide a sketch of the mathematics of the Lasso estimator illustrating its variable selection properties and moti-vating the choice of 𝜆 in (3.1.4).
Assume 𝜓ˆ 𝑗 = 1 for simplicity. The 𝑗-th component 𝛽ˆ𝑗 of the Lasso estimator 𝛽ˆ is set to zero if the marginal predictive benefit of changing 𝛽ˆ𝑗 away from zero is smaller than the marginal
increase in penalty:
𝛽ˆ𝑗 = 0 if 1  𝜕  I:(𝑌𝑖 − 𝛽ˆ′𝑋𝑖)21 < 𝜆.
 


12: Cross-validation is a repeated data-splitting method for choos-ing penalty parameters for Lasso and for selecting among predictive models more generally. We outline the basic idea of cross-validation in Section 3.B.
 

That is,
 
1 𝜕𝛽ˆ𝑗


𝛽ˆ𝑗 = 0 if | − 𝑆ˆ𝑗 | < 𝜆,
 
𝑖	1

𝑆ˆ𝑗 = 2 I:(𝑌𝑖 − 𝛽ˆ′𝑋𝑖)𝑋𝑗𝑖 .
 

We discuss more detailed heuristics for penalty level selection in the appendix, but the rough idea is that the penalty should dominate the noise
𝑆𝑗 = 2 I:(𝑌𝑖 − 𝛽′𝑋𝑖)𝑋𝑗𝑖

in the measurement of the marginal predictive ability. By the high-dimensional central limit theorem ([6]), we have that
 
(𝑆𝑗
 
a 2√𝑛𝜎(Nj)𝑝
 
,	N𝑗 ∼ 𝑁(0, 1).
 
Therefore, to guarantee that Lasso sets to zero any coefficient whose actual value is zero, we would like to choose 𝜆 to domi-nate
 

 
2√𝑛𝜎 max
𝑗=1,...,𝑝
 
|N𝑗 |
 
with high probability, say 1  a. Because we are controlling
the maximum over 𝑝 coordinates, the calibration inevitably depends on 𝑝 (and, once we use Gaussian tail bounds, typically through a log 𝑝 term). Then by the union bound and symmetry
 


of centered normal variables,
 
P ( 𝑗 max |N𝑗 | > 𝑧1−a/(2𝑝)) ≤
 
𝑝
2
𝑗=1
 
P (N𝑗 > 𝑧1−a/(2𝑝))
 
=	2𝑝 (1 − (1 − a/(2𝑝))) = a.
The union bound here is crude, but the bound is not very loose. In particular, when the N𝑗’s are independent, the bound becomes sharp as 𝑝 → ∞. Finally, setting
𝜆 = 2𝜎√𝑛𝑧1−a/(2𝑝)
 
we conclude that
 


P(max |𝑆𝑗 | ≤ 𝜆) ≥ 1 − a,
 

up to a vanishing error. That is, this choice of 𝜆 guarantees that variables with 𝛽𝑗 = 0 are excluded from the model (have 𝛽ˆ𝑗 = 0) with high probability.

 
OLS Post-Lasso
We can use the Lasso-selected set of regressors, those regressors whose Lasso coefficient estimates are non-zero, to refit the model by least squares. This method is called "least squares post Lasso" or simply Post-Lasso ([4]). Compared to Lasso, Post-Lasso undoes the overall shrinkage toward zero relative to unconstrained least squares from the estimated non-zero coefficients, as we illustrate in Figure 3.5 below.13 Removing this shrinkage towards zero from the non-zero coefficients sometimes delivers improvements in predictive performance.
 










13: Note that the estimates of the large coefficients are nearly perfect after OLS refitting of the model selected by Lasso in this example.
 

Post-Lasso. We define the Post-Lasso
-𝛽 ∈	I:(𝑌𝑖 − 𝑏′𝑋𝑖)2 such that
arg min
𝑏∈ℝ𝑝	𝑖	(3.1.5)
𝑏𝑗 = 0 if 𝛽ˆ𝑗 = 0 for each 𝑗,
where 𝛽ˆ is the Lasso coefficient estimator. The formal prop-erties of the Post-Lasso estimator 𝛽 are similar to those of
Lasso 𝛽ˆ; see Section 3.2.	-

 









 




5	10	15	20

j
 
Figure 3.5: The true coefficients (black) vs. coefficients estimated by Post-Lasso (blue) in the Example
3.1.1.	Post-Lasso tends to remove regularization bias from the esti-mated non-zero coefficients.
 

 
3.2	Predictive Performance of Lasso and Post-Lasso
The best linear prediction rule (out-of-sample) is 𝛽′𝑋. We want to understand the quality of the Lasso prediction rule, 𝛽ˆ′𝑋.
That is,
▶ Does 𝛽ˆ′𝑋 provide a good approximation to 𝛽′𝑋?
Recall that with Lasso, we are trying to estimate 𝑝 parameters
𝛽1, ..., 𝛽𝑝, imposing approximate sparsity via penalization. Un-der approximate sparsity, only a few, say 𝑠, parameters will be "important." We can call 𝑠 the effective dimension. Lasso ap-proximately figures out which parameters are important to keep. Further, intuitively, to estimate each of the "important"
 


𝑠 parameters well, we need many observations for each such parameter. This means that 𝑛 𝑠 must be large, or, equivalently
𝑠 𝑛 must be small. Using previous reasoning from least squares theory, we might also conjecture that the key determinant of the rate at which Lasso approximates the best linear predictor is  𝑠/𝑛. This conjecture is almost correct.
We now state a representative guarantee. It bounds the pre-diction error of Lasso in terms of the noise level, the effective dimension 𝑠, and the ambient dimension 𝑝. The extra log 𝑝 fac-tor reflects the cost of searching over many potential regressors when we do not know in advance which ones matter.

Therefore, if 𝑠 log max 𝑝, 𝑛 𝑛 is small, Lasso and Post-Lasso regression come close to the population regression function/best
linear predictor. Relative to our conjectured rate ✓𝑠/𝑛, there
factor captures the price of not knowing a priori which of the
𝑝 regressors are the 𝑠 important ones. Lasso approximately finds these important predictors, but correspondingly suffers a loss relative to a predictor estimated with knowledge of the best 𝑠-dimensional model (“oracle estimator”). A theoretical guarantee similar to Theorem 3.2.1 has been established for cross-validated Lasso [5], though the bound contains additional logarithmic factors.
 


Under approximate sparsity and with appropriate choice of penalty parameters, Lasso and Post-Lasso will approximate the best linear predictor well. Theoretically, they will not overfit the data, and we can thus use the sample and adjusted 𝑅2 and 𝑀𝑆𝐸 to assess out-of-sample predictive performance. Of course, it is always a good idea to verify the out-of-sample predictive performance by using sample splitting.

Remark 3.2.1 (Exact Sparsity) It is helpful to consider the exactly sparse case, in which there are only 𝑘 non-zero co-efficients bounded by some constant and the rest of the coefficients are exactly zero. In this case, the effective dimen-sion is (up to constants) equal to the number of non-zero coefficients, i.e.
𝑠 = const · 𝑘.
To see this, note that 𝛽 satisfies the approximate sparsity condition with 𝐴 = const · 𝑘𝑎 for 𝑎 ≥ 1, since 𝛽𝑗 ≤ const ≤ const · 𝑘𝑎/𝑗𝑎 for 𝑗 ≤ 𝑘 and 𝛽𝑗 = 0 ≤ const · 𝑘𝑎/𝑗𝑎 for 𝑗 > 𝑘.
Then 𝑠 ≤ const · 𝑘𝑛1/2𝑎, which yields the result as 𝑎 → ∞.
On regularity conditions★. A sufficient condition under which Theorem 3.2.1 can be established is the restricted isometry condition:

This condition says that "small groups" of regressors are not collinear and are well-behaved. I.e. we have that subvectors 𝑍 of
𝑋 with dimension 𝐿 = 𝑠 log 𝑛 have empirical Gram matrices
𝔼𝑛 𝑍𝑍′ that are close to their population analogues E 𝑍𝑍′
in the operator norm and have population covariance matrix E 𝑍𝑍′ with eigenvalues bounded away from zero and from above. This condition is simple and intuitive but is stronger than necessary. Results similar to Theorem 3.2.1 have been shown
 


to hold under considerably weaker conditions. The condition sup 𝑎 =1 𝑎′ 𝔼𝑛 𝑍𝑍′  E 𝑍𝑍′ 𝑎  0 has been demonstrated to be valid under various more primitive conditions; see Appendix
3.C.

3.3	A Helicopter Tour of Other Penalized Regression Methods for Prediction
Instead of the Lasso penalty, other penalty schemes can be used, leading to different regression estimators with different proper-ties. These estimators are motivated by different structures for the coefficients on the set of regressors in a high-dimensional model. We consider three important settings where coefficients are sparse, dense, or sparse+dense.
We have already seen that sparse coefficient vectors have a small number of relatively large, non-zero coefficients with the rest of the coefficients being close enough to zero to be ignorable. A dense coefficient vector has the vast majority or all coefficients non-zero and of comparable magnitude. A sparse+dense structure has the vast majority of coefficients being non-zero and of similar magnitude along with a small number of relatively large coefficients. Figure 3.6 illustrates each setting.
Throughout this section, in order to simplify the exposition, we assume that regressors have been centered and normalized to have second empirical moment equal to 1:
𝔼𝑛 𝑋2 = 1, for all 𝑗 = 1, ..., 𝑝.
This allows us to simplify the penalty choice. In the context of Lasso, this normalization induces the unitary penalty loadings:
𝜓ˆ = 1, for all 𝑗.
We have already outlined Lasso regression, which performs best in an approximately sparse setting. We next consider the Ridge method, which performs best in the dense setting.

Ridge. The Ridge method estimates coefficients by penal-ized least squares, where we minimize the sum of squared prediction error plus the penalty term given by the sum of the squared values of the coefficients times a penalty level
𝜆:
 



Approximately Sparse

5	10	15	20

j


Dense

5	10	15	20
 

j


Sparse+ Dense









5	10	15	20

j
 


Figure 3.6: The Lasso penalty is best suited for approximately sparse models, and the Ridge penalty for models with small dense co-efficients. The Elastic Net can be tuned to perform well with either sparse or dense coefficients. The Lava penalty is best suited for mod-els with coefficients generated as the sum of approximately sparse coefficients and small dense coeffi-cients.
 

𝑛
𝛽ˆ(𝜆) = arg min I:(𝑌𝑖 − 𝑏′𝑋𝑖)2 + 𝜆 I: 𝑏2.
𝑏∈ℝ𝑝 𝑖=1	𝑗	𝑗

Ridge balances the complexity of the model measured by the sum of squared coefficients with the goodness of in-sample fit. In contrast to Lasso, Ridge penalizes the large values of coefficients much more aggressively and small values much less aggressively – indeed, squaring big values makes them even bigger and squaring small numbers makes them even smaller.

Because of the latter property,
▶ Ridge does not set estimated coefficients to zero and
 


so it does not do variable selection.
▶ The Ridge predictor 𝛽ˆ′𝑋 is especially well suited
for prediction in "dense" models, where the 𝛽𝑗’s are all small without necessarily being approximately sparse.
▶ Ridge regression is also well suited when the matrix E[𝑋𝑋′] is poorly behaved, as measured by the decay of its eigenvalues to zero.
In the dense case, the Ridge predictor can easily outperform the Lasso predictor.

Like Ridge, the Lasso predictor empirically seems to have rea-sonable prediction performance in the presence of ill-behaved design matrices, although its theoretical properties are not as well understood in this case.

 


 

 


Now consider Ridge with penalty level 𝜆, and write its fitted value at 𝑋𝑖 as 𝑋𝑖′𝛽ˆ 𝜆 . A short calculation using the spectral decomposition above shows that Ridge can be written as
a regression on all PC scores, but with component-specific shrinkage:
𝑋′𝛽ˆ(𝜆) = I: 𝑃𝑘𝑖 𝑤𝑘(𝜆) 𝔼𝑛[𝑃𝑘𝑌],	𝑤𝑘(𝜆) =   𝜁𝑘	 .

In words, along the 𝑘th PC direction we would (without regularization) use the OLS coefficient 𝔼𝑛 𝑃𝑘𝑌 , but Ridge multiplies it by the attenuation factor 𝑤𝑘 𝜆  0, 1 . Compo-nents with large eigenvalues 𝜁𝑘 (directions with substantial variation in 𝑋) are barely shrunk, while components with small eigenvalues (directions close to collinearity) are strongly shrunk.
Principal components regression (PCR) uses the same PC scores but makes a different regularization choice: it keeps only the first 𝐾 components and drops the rest. Its fitted values have the form
𝑦ˆPCR(𝐾) = I: 𝑃𝑘𝑖 𝔼𝑛[𝑃𝑘𝑌].
𝑘=1
Comparing the two displays, we see that PCR applies a
hard cutoff (weights equal to 1 for 𝑘   𝐾 and 0 for 𝑘 >
𝐾), whereas Ridge applies a soft cutoff (weights 𝑤𝑘 𝜆 that decrease smoothly as 𝜁𝑘 becomes small relative to 𝜆). Thus, Ridge may be viewed as a continuous version of PCR: it uses all principal components, but downweights those supported by little variation in the design.
This connection is useful beyond interpretation. It suggests that we can explicitly rotate 𝑋 into PC scores and then apply any of the penalized methods in this chapter in that rotated basis, as well as the more advanced techniques discussed in Chapter 8. We further explore PCA as a feature-extraction device in Chapter 10. For additional discussion, see [9] (pp. 64–67) or the online discussion Ridge vs. PCA.
Ridge and Lasso have other useful modifications or hybrids that can perform well in the sparse, dense or sparse + dense settings. One popular modification is the Elastic Net [10] that can perform well in either the sparse or the dense scenario with appropriate tuning.
 


Elastic Net. The Elastic Net method estimates coefficients by penalized least squares with the penalty given by a linear combination of the Lasso and Ridge penalties:
𝛽ˆ(𝜆1, 𝜆2) = arg min I:(𝑌𝑖 − 𝑏′𝑋𝑖)2 + 𝜆1 I: 𝑏2 + 𝜆2 I: |𝑏𝑗 |.
𝑏∈ℝ𝑝  𝑖	𝑗	𝑗	𝑗

We see that the penalty function has two penalty levels 𝜆1 and
𝜆2, which are chosen by cross-validation in practice.

▶ By selecting different values of penalty levels 𝜆1 and
𝜆2, we have more flexibility with Elastic Net for build-ing a good prediction rule than with just Ridge or Lasso.
▶ The Elastic Net performs variable selection unless we completely shut down the Lasso penalty by setting
𝜆2 = 0.
▶ With proper tuning, Elastic Net works well in regres-
sion models where regression coefficients are either approximately sparse or dense.

See [11] for some theoretical results on Elastic Net.
Another way to combine the Lasso and Ridge penalties is the Lava method, which is intended to work well in sparse+dense settings.

Lava. The Lava method ([12], [13]) estimates coefficients by solving the penalized least squares problem:
𝛽ˆ(𝜆1, 𝜆2) = arg	min	I:(𝑌𝑖 − 𝑏′𝑋𝑖)2
𝑏:𝑏=𝛿+𝜉∈ℝ𝑝  𝑖
+𝜆1 I: 𝛿2 + 𝜆2 I: |𝜉𝑗 |.
𝑗
𝑗	𝑗

Here components of the parameter vector are split into a "dense part" 𝛿𝑗 and "sparse part" 𝜉𝑗, where the 𝛿𝑗’s are penalized like in Ridge, and the 𝜉𝑗’s are penalized like in Lasso. The minimization program automatically determines the best split into the dense and sparse parts. There are two corresponding penalty levels 𝜆1 and 𝜆2, which can be chosen by cross-validation in practice.
 



▶ Compared to the Elastic Net, the Lava method pe-nalizes large and small coefficients much less aggres-sively – large coefficients are penalized like Lasso and small coefficients like Ridge. Like Ridge, Lava does not do variable selection.
▶ Lava is designed to work well in
"sparse + dense"
regression models where there are several large coeffi-cients and many small coefficients that do not vanish quickly enough to satisfy approximately sparsity.
▶ With proper tuning that allows either 𝜆1 or 𝜆2 to
be set to large values, Lava can also work in either "sparse" or "dense" models.

 
Theoretical guarantees for these methods are given in [12] and [13]. Theoretically and practically, Lava can significantly outper-form Lasso, Ridge and Elastic Net in "sparse+dense" models, and, with appropriate tuning, has comparable performance to Lasso in "sparse" models and to Ridge in "dense" models.
 






The code to produce the results and specific details about simulation de-sign are in the Notebooks 3.6.1.
 

 


Lasso (Plug-in)	0.775	-0.030	0.329
Post-Lasso (Plug-in)	0.800	0.000	0.285
Ridge	0.182	0.162	0.116
Elastic Net	0.741	0.003	0.319
Lava	0.774	0.152	0.399

3.4	Choice of Regression Methods in Practice
How should we select the appropriate penalized regression method? The answer is simple if we are interested in building the best prediction. We can split the data into training and testing sets and simply choose the method that performs the best on the test set. Rigorous theoretical guarantees for this approach have been provided by [14].
We show an example of this approach in Notebook 3.6.2 which illustrates the use of penalized regression methods for pre-dicting log-wages using CPS 2015 data. We make use of three specifications introduced in Chapter 1
▶ Basic Model: 𝑋 consists of raw regressors – sex, expe-rience, education indicators, occupation and industry indicators, and regional indicators.

▶ Flexible Model: 𝑋 consists of all raw regressors from the basic model as well as transformations of the raw regressors – polynomials in experience (experience2, experience3, and experience4) and additional two-way interactions of the polynomials in experience with all other raw regressors except for sex.

▶ Extra Flexible Model: 𝑋 consists of sex and all two way in-teractions between experience, experience2, experience3, experience4, and all other raw regressors except for sex.

In the CPS 2015 wage application used in Notebook 3.6.2, these three specifications correspond to 𝑝 = 51 (Basic), 𝑝 = 246 (Flexible), and 𝑝 = 979 (Extra Flexible) constructed regressors.
We consider estimating our prediction rule with each set of regressors using OLS. For the Flexible and Extra Flexible models, we also consider the penalized approaches discussed in
 
Table 3.1: Population Out-of-Sample 𝑅2 in Simulation Experi-ment.
 

 
this chapter. For each penalized method, we use cross-validation to select tuning parameters. For Lasso and Post-Lasso, we also consider the plug-in choice provided in (3.1.4).
In this exercise, we first split the data into a training and test set (80% training, 20% test).14 We use the training data to estimate candidate models based on our three specifications. For the penalized procedures, this estimation on the training set includes cross-validation for tuning parameter choice. We then evaluate the trained models on the test data. We provide the out-of-sample 𝑅2 obtained from applying the models estimated on the training data to predict the test outcomes in Table 3.2.


OLS	Basic
0.288	Flexible
0.238	Extra Flexible
0.133
Lasso (Cross-validation)		0.279	0.272
Lasso (Plug-in)		0.265	0.264
Post-Lasso (Plug-in)		0.261	0.244
Ridge		0.263	0.253
Elastic Net		0.276	0.264
Lava		0.283	0.290


In this example, we see that there is not much difference be-tween just using simple OLS with the basic controls and the penalized methods with the more expansive set of controls. Importantly, we see that even though the simplest specifica-tion works relatively well in this example, there is not much loss from considering the broader set of controls. Considering this broader set of controls can lead to large gains complex settings with many raw variables or where nonlinearities are important. One takeaway from this example is that it is often useful to consider simple baseline models among the set of models considered, especially in settings common in the social sciences where data sets are relatively small and predictor sets are only moderately complex. In addition to sometimes working relatively well, as in this example, simple models are easy to explain and understand.
To conclude, we might choose to use the model produced by Lava in the Extra Flexible specification as our prediction rule as it produces (marginally) the best performance on the test data in this example, though there’s a strong argument for preferring basic OLS as it is simple and highly competitive. Rather than choose a single method, we could also use ensemble methods to aggregate prediction methods to get boosts in predictive performance. We describe these aggregation methods in Chapter 8.
 






14: It is also common practice to choose models purely on the ba-sis of cross-validation. The Python notebook for this example, Note-book 3.6.2, follows this approach.




Table 3.2: Test Sample 𝑅2 in the Wage Prediction Example.
 

3.5	Notes
Lasso was introduced by Frank and Friedman [15], and its geometric and computational properties were elaborated on by Tibshirani [16], who also gave it its name. The first general theoretical analysis of Lasso was done by Bickel, Ritov, and Tsybakov [3]. Hastie, Tibshirani, and Wainwright [17] provides a good textbook introduction.
There are many variations on the basic Lasso theme, only some of which we mentioned in this chapter. The properties of the Post-Lasso estimator in approximately sparse models (without assuming that Lasso perfectly selects the "right model") were first established in [4]. The properties of Lasso and Post-Lasso don’t hinge on the assumption of Gaussian or sub-Gaussian errors, as proven in [7], though such assumptions are often imposed. Fundamentally, the properties of these procedures rely on a high-dimensional central limit theorem ([6]) that allows Gaussian approximations to key average-like quantities. While cross-validation has been frequently used to select the penalty level, validity of this approach for Lasso was only proven recently – [5]. The Lasso has been extended to clustered dependence by [18] and to time series and many time series by [19], with the corresponding package available at this Link.
There is a large literature on Ridge estimation, with the reference
[8] providing what seems to be the state of the art. The Lava approach has been proposed and analyzed in [12] and [13]. [13] also discusses applications to problems with latent confounding and, for this reason, refers to Lava as the spectral deconfounder. We discuss other approaches to dealing with latent confounding in Chapter 12 and Chapter 13.

3.6	Notebooks
Notebook 3.6.1 (Penalized Regression) R Notebook on Penal-ized Regressions and Python Notebook on Penalized Regres-sions provide details of implementation of different penalized regression methods and examine their performance for ap-proximating regression functions in a simulation experiment. The simulation experiment includes one case with approxi-mate sparsity, one case with dense coefficients, and another case with both approximately sparse and dense components.
 


Notebook 3.6.2 (Predicting Wages) R Notebook on ML for Prediction of Wages and Python Notebook on ML for Predic-tion of Wages provide details of implementation of different penalized regression methods and examine their performance for predicting log-wages using CPS 2015 data.


3.7	Checklist
After working through this chapter, we should be able to:
▶ explain why least squares can overfit when 𝑝 is large (especially when 𝑝  𝑛);
▶ state the idea of approximate sparsity and interpret the notion of an effective dimension 𝑠;
▶ write down the Lasso optimization problem, interpret the roles of 𝜆 and the penalty loadings, and explain why the ℓ1 penalty induces sparse solutions;
▶ describe Post-Lasso and explain why refitting by least squares can reduce shrinkage (regularization) bias;
▶ interpret the basic prediction-error bound for Lasso and identify the roles played by 𝑠, 𝑛, and log 𝑝;
▶ recognize when Ridge, Elastic Net, and Lava may be preferable (dense structure, correlated regressors, or sparse+dense structure);
▶ implement honest model comparison with a train/test split and cross-validation of tuning parameters (including cross-validating the full Post-Lasso pipeline).

3.8	Exercises
Exercise 3.8.1 (Lasso Optimization) Solve the Lasso opti-mization problem analytically with only one regressor and interpret the solution.

Exercise 3.8.2 (Lasso Simulation) Experiment with the R Notebook on Penalized Regressions, trying out modifications of the Monte-Carlo experiments. As examples, you might change parameters that govern the speed of decay of coef-ficients to zero, change the error distribution, or alter the structure of dependence among the design variables. Try to explain the results to a fellow student, linking explanations to the theoretical properties of these methods.
 


Exercise 3.8.3 (Predicting Wages) Experiment with the R Notebook on ML Prediction of Wages. Try to explain the results to a fellow student, linking explanations to the theo-retical properties of these methods.


3.A	Additional Discussion and Results
Iterative Estimation of 𝜎
The plug-in choice of 𝜆 given in equation (3.1.4) requires an estimate of 𝜎. We can estimate 𝜎 using the following iterative method. Let 𝑋0 be a small set of regressors (a trivial choice is just the intercept, but we may include, for example, the five regressors that are most strongly correlated with the 𝑌𝑖’s).
 
Let 𝛽0
 
be the least squares estimator of the coefficients on the
0
 
𝜎ˆ0 := J𝔼𝑛[(𝑌𝑖 − 𝛽ˆ′0 𝑋0)2].
Set 𝑘 = 0, and specify a small constant 𝜈	0 as a tolerance level and a constant 𝐾 > 1 as an upper bound on the number
of iterations:	We find that 𝐾 = 1 works well in
practice.
1.	Compute the Lasso estimator 𝛽 based on the penalty level
𝜆 given in equation (3.1.4) using 𝜎ˆ𝑘.
2.	Set 𝜎ˆ𝑘+1 =	𝔼𝑛[(𝑌𝑖 − 𝛽ˆ′𝑋𝑖)2].
3.	If 𝜎𝑘 1	𝜎𝑘 ⩽ 𝜈 or 𝑘 > 𝐾, stop; otherwise set 𝑘	𝑘	1
and go to (1).
We note that the plug-in choice of 𝜆 given in equation (3.1.4) relies on assuming independence between the BLP residu-als and the regressors, i.e. 𝜖 ⊥ 𝑋. Independence implies (and is stronger than) homoskedasticity and yields E[𝜖2𝑋2] = E[𝜖2]E[𝑋2]. With independent observations where we do not want to assume 𝜖  ⊥ 𝑋, we should use penalty loadings
𝜓ˆ 𝑗 =  𝔼𝑛[𝜖ˆ2 𝑋 ], where 𝜖ˆ𝑖 ≈ 𝜖𝑖 can be estimated in a similar
iterative manner as described above. In this case, we would then take 𝜎ˆ = 1 in formula (3.1.4) for 𝜆 (see [7] for more details).
We expect the homoskedastic formula for the penalty provided in (3.1.4) will work well in many cases, especially when ran-dom variables 𝜖, 𝑋𝑗 are expected to have fast decaying tail probabilities. For example, when fourth moments of 𝜖, 𝑋𝑗 are bounded by some constant factor of their second moments,
 


an application of the Cauchy-Schwarz inequality implies that E[𝜖2𝑋2] ≤ const · E[𝜖2]E[𝑋2], which is, up to a constant, the
simplifying condition implied by homoskedasticity.

Some Lasso Heuristics via Convex Geometry★
Assume 𝜓ˆ 𝑗 = 1 for each 𝑗 for simplicity, which amounts to normalizing regressors to have the second empirical moment equal to 1. Consider

 


where
 
-𝛽 ∈
 
arg min 𝑄 𝑏
𝑏∈ℝ𝑝
 
𝜆
) + 𝑛 ∥
 
𝑏∥1,	(3.A.1)
 
𝑄(𝑏) = 𝔼𝑛[(𝑌𝑖 − 𝑏′𝑋𝑖)2].
The key quantity in the analysis of (3.A.1) is the score – the gradient of 𝑄 at the true value:
𝑆 = −∇𝑄(𝛽0) = 2𝔼𝑛[𝑋𝜖].
The score 𝑆 is the effective "noise" in the problem that should be dominated by the regularization. However, we would like to make the regularization bias as small as possible. This reasoning suggests choosing the smallest penalty level 𝜆 that is just large enough to dominate the noise with high probability, say 1 a, which yields
𝜆 > 𝑐Λ, for Λ := 𝑛∥𝑆∥∞.	(3.A.2)
Here, Λ is the maximal score scaled by 𝑛, and 𝑐 > 1 is a theo-retical constant that guarantees that the score is dominated.
It is useful to mention some simple heuristics for the principle (3.A.2) which arise from considering the simplest case where all of the regressors are irrelevant so that 𝛽 = 0. We want our
estimator to perform at a near-oracle level in all cases, including
this case, but here the oracle estimator 𝛽∗ sets 𝛽∗ = 𝛽 = 0. We thus also want 𝛽 = 𝛽 = 0 in this case, at least with a high probability, say 1  a. From the subgradient optimality
conditions for (3.A.1), we must have
−𝑆𝑗 + 𝜆/𝑛 > 0 and 𝑆𝑗 + 𝜆/𝑛 > 0 for all 1 ≤ 𝑗 ≤ 𝑝	(3.A.3)
for the Lasso estimator for each coefficient to be exactly 0. We can guarantee (3.A.3) holds by setting the penalty level 𝜆/𝑛 such that 𝜆 > 𝑛 max1≤𝑗≤𝑝 |𝑆𝑗 | = 𝑛∥𝑆∥∞ with probability at least 1 − a, which is precisely what the rule (3.A.2) does.
 


Gaussian approximations to this score motivate the following X-dependent penalty implementation.


Other Variations on Lasso
Here and below we assume that
𝜓ˆ 𝑗 = 1,	𝑗 = 1, ..., 𝑝
to simplify notation. A variant of Lasso, called the Square-root Lasso estimator ([20],[21]), is defined as a solution to the following program:
 
min ✓𝔼𝑛[(𝑌 − 𝑏′𝑋 2
 

𝑏∥1.	(3.A.6)
 


Analogously to Lasso, we may set the penalty level as
𝜆 = 𝑐 · Λ-(1 − a|{𝑋𝑖 }𝑖=1),	(3.A.7)
where 𝑐 > 1 and
Λ-(1−a|{𝑋𝑖 }𝑖=1)	J

with 𝑔𝑖	𝑁 0, 1 independent for 𝑖 = 1, . . . , 𝑛. As with Lasso, there is also a simple asymptotic option for setting the penalty
level:
𝜆 = 𝑐 · 2√𝑛𝑧1−a/(2𝑝).	(3.A.8)
The main attractive feature of (3.A.6) is that the penalty level 𝜆 specified above is independent of the value 𝜎. This estimator has statistical performance that is as good as the iterative or cross-validated Lasso. Moreover, the estimator is a solution to a highly tractable conic programming problem:
 

min	𝑡
𝑡≥0,𝑏∈ℝ𝑝
 
𝜆
+ 𝑛 ∥
 
𝑏∥1 : ✓𝔼𝑛[(𝑌 − 𝑏′𝑋)2] ≤ 𝑡,	(3.A.9)
 
where the criterion function is linear in parameters 𝑡 and positive and negative components of 𝑏, while the constraint can be formulated with a second-order cone, informally known as the "ice-cream cone."
There are several other estimators that make use of penalization by the ℓ1-norm. A final important case is the Dantzig selector estimator [22]. It also relies on ℓ1-regularization but exploits the notion that the residuals should be nearly uncorrelated with the covariates. The estimator is defined as a solution to

 
min
𝑏∈ℝ𝑝
 
∥𝑏∥1  :  ∥𝔼𝑛[𝑋(𝑌 − 𝑏′𝑋)]∥∞ ≤ 𝜆/𝑛.	(3.A.10)
 
Again, one may set 𝜆 = 𝜎Λ(1 −a|{𝑋𝑖 }𝑛 ). Here, we focused our
discussion on Lasso but virtually all theoretical results carry over to other ℓ1-regularized estimators including (3.A.6) and (3.A.10). We also refer to [23] for a feasible Dantzig estimator that combines the square-root Lasso method (3.A.9) with the Dantzig method.

3.B	Cross-Validation
Cross-validation is a common practical tool that provides a way to choose tuning parameters such as the penalty level in Lasso.
 


The idea of cross-validation is to rely on repeated splitting of the training data to estimate the out-of-sample predictive performance.

We can also consider many different methods for constructing prediction rules as well. For example, we could try Lasso with many different values of the penalty parameter and Ridge with many different values of the penalty parameter and choose the tuning parameter and method (Lasso or Ridge) that minimizes the cross-validated Mean Squared Prediction Error.

 


 

Note that the pooled procedure is different from the default CV procedure implemented in many software packages and used in many applications. In particular, the pooled rule averages the fold-specific predictors, whereas the default practice typically refits a single predictor on the full sample at the chosen tuning parameter.

3.C	Laws of Large Numbers for Large Matrices★
The following results are useful for justifying the restricted isometry condition for empirical Gram matrices 𝔼𝑛[𝑋𝑋′].
Let 𝑠𝑛 , 𝑝𝑛 , 𝑘𝑛 be sequences of positive constants, ℓ𝑛 = log(𝑛),
and 𝐶 a fixed positive constant. Let (𝑋𝑖)𝑛	be iid. vectors.
 
Denote by 𝑍 𝑛
 
𝑖=1
 
( 𝑖)𝑖=1 corresponding subvectors.
Suppose that max∥𝑎∥=1 E[(𝑍′𝑎)2] ≤ 𝐶 for all 𝑍 ⊂ 𝑋 such that dim(𝑍) ≤ 𝑠𝑛ℓ𝑛 and that one of the following holds:
(a)	𝑋𝑖 is a sub-Gaussian, namely
sup P(|𝑋′𝑢| > 𝑡) ≤ 2 exp(−𝑡2/𝑐2)
𝑖	2
∥𝑢∥≤1
for all 𝑡 ≥ 0, and 𝑠𝑛(log 𝑛) (log (max{𝑝𝑛 , 𝑛})) /𝑛 → 0;
 


(b)	𝑋𝑖 has bounded components, namely
max |𝑋𝑖𝑗 | ≤ 𝑘𝑛

and 𝑘2 𝑠𝑛 log2 𝑛 log(𝑠𝑛 log 𝑛) log (max{𝑝𝑛 , 𝑛}) /𝑛 → 0.
Then with probability 1 − 𝛿𝑛
 


𝑍⊂𝑋
 
max
 

𝑠 ℓ
 
max |𝑎′ (𝔼𝑛 [𝑍𝑍′] − E [𝑍𝑍′]) 𝑎| ≤ Δ𝑛 ,
 
:dim(𝑍)≤ 𝑛 𝑛 ∥𝑎∥=1
where (𝛿𝑛 , Δ𝑛) are decreasing sequences and (𝛿𝑛 , Δ𝑛) → 0.
Under (a) the result follows from Theorem 3.2 in [25] and under
(b) the result follows from [26]. These references also imply finite-sample characterization of error bounds (𝛿𝑛 , Δ𝑛).

3.D	A Sketch of the Lasso Guarantee Under Exact Sparsity★
Let us assume that the population BLP 𝛽0 satisfies exact sparsity,
i.e. only 𝑠 out of 𝑝 coefficients are non-zero. Denote with 𝑆 the set of non-zero coefficients and with 𝑆𝑐 the complement of that set. Consider the Lasso minimizing the objective 𝑄ˆ (𝑏) + 𝜆 ∥𝑏∥1
for 𝑄-(𝑏) = 𝔼𝑛[(𝑌 − 𝑏′𝑋) ]. From the optimality conditions, we
𝑄ˆ (𝛽ˆ) − 𝑄ˆ (𝛽0) ≤ 𝜆(∥𝛽0∥1 − ∥𝛽ˆ∥1).	(3.D.1)
Let 𝜈 := 𝛽ˆ	𝛽0. Since the objective 𝑄ˆ 𝛽 is convex in 𝛽, we have by an application of the Cauchy-Schwarz inequality that
𝑄ˆ (𝛽ˆ) − 𝑄ˆ (𝛽0) ≥ ∇𝑄ˆ (𝛽0)′𝜈 = −𝑆′𝜈 ≥ −∥𝑆∥∞∥𝜈∥1
for 𝑆 = −∇𝑄(𝛽0) = 2𝔼𝑛[𝑋𝜖].
We will assume that 𝜆 is chosen such that we have 𝜆 ≥ 2∥𝑆∥∞
 
 
with probability 1 −a.15
 
𝑛
We focus then on the good event where
 
15: The High-Dimensional CLT
 
the above inequality is satisfied. Then we can combine the above two inequalities:
 
bounds tell us that if we set 𝜆 ≈ (	{ /	})
equality holds with probability 1 −
 
𝜆	𝜆	a.
𝑛 (∥𝛽0∥1 − ∥𝛽ˆ∥1) ≥ −∥𝑆∥∞∥𝜈∥1 ≥ − 2𝑛 ∥𝜈∥1.
Hence, with high probability,
𝛽ˆ − 𝛽0 ∈ 𝑅𝐶 = {𝜈 : ∥𝛽0 + 𝜈∥1 ≤ ∥𝛽0∥1 + ∥𝜈∥1/2}.
 


 
Note also that 𝜈 ∈ 𝑅𝐶 implies16
∥𝜈𝑆𝑐 ∥1 ≤ 3∥𝜈𝑆 ∥1	(3.D.2)
where 𝜈𝑆 denotes the entries from 𝜈 in 𝑆 and 𝜈𝑆𝑐 denotes the entries of 𝜈 in 𝑆𝑐 . This inequality roughly states that the error vector 𝜈 = 𝛽ˆ − 𝛽0 is primarily supported on 𝑆.
We impose the following regularity condition, which holds with probability approaching 1:
 
16: Verify this as a reading exercise.
 

 
0 < 𝐶1 ≤ min
 
𝜈′𝐺𝜈
 
≤ max
 
𝜈′𝐺𝜈
 
≤ 𝐶2 < ∞,	(3.D.3)
 
𝜈∈𝑅𝐶\0 ∥𝜈∥2	𝜈∈𝑅𝐶\0 ∥𝜈∥2
 
for both the empirical Gram matrix 𝐺 = 𝔼𝑛 𝑋𝑋′ and population Gram matrix 𝐺 = E𝑋𝑋′. This condition is in fact implied by the Restricted Isometry Conditions stated in the main text.17
Then, using the fact that 𝑄ˆ 𝛽 is quadratic in 𝛽, we can invoke the exact second order Taylor expansion:
𝑄ˆ (𝛽ˆ) − 𝑄ˆ (𝛽0) = 𝑆′𝜈 + 𝜈′𝔼𝑛[𝑋𝑋′]𝜈 ≥ −∥𝑆∥∞∥𝜈∥1 + 𝐶1∥𝜈∥2.
When combined with the upper bound from the optimality of
𝛽ˆ for the penalized empirical loss and the fact that 𝜆 ≥ 2∥𝑆∥∞,
 



17: See, e.g. Lemma 10 in [27] for an argument based on [3].
 
this expansion yields
𝜆 ∥𝜈∥1 ≥ 𝑄ˆ (𝛽ˆ) − 𝑄ˆ (𝛽0) ≥ − 2 𝜆𝑛 ∥𝜈∥1 + 𝐶1∥𝜈∥2.
The second crucial inequality is that
2	3𝜆
 


then follows.
 
∥𝜈∥2 ≤ 2𝐶1 𝑛 ∥𝜈∥1	(3.D.4)
 
Finally, note that for any vector 𝜈 that is primarily supported on
𝑆, the ℓ2 and ℓ1 norms are within a factor ≈ √𝑠 of each other:
∥𝜈∥1 = ∥𝜈𝑆 ∥1 + ∥𝜈𝑆𝑐 ∥1 ≤ 4∥𝜈𝑆 ∥1 ≤ 4√𝑠∥𝜈𝑆 ∥2 ≤ 4√𝑠∥𝜈∥2
where we used the norm inequality, that for an 𝑠-dimensional vector 𝑣, we have ∥𝑣∥1 ≤ √𝑠∥𝑣∥2. Thus, we can conclude
𝜈	 6𝜆 √𝑠.	(3.D.5)
𝐶1𝑛
Using the assumption that 𝜈′E[𝑋𝑋′]𝜈 ≤ 𝐶2∥𝜈∥2 for 𝜈 ∈ 𝑅𝐶,
 


we get the final bound:
JE𝑋[(𝑋′𝛽ˆ − 𝑋′𝛽0)2] = ✓𝜈′E[𝑋𝑋′]𝜈 ≤ 𝐶2∥𝜈∥2 ≤ 6𝜆 𝐶2 √𝑠.
 


Bibliography



[1]		Antoine de Saint-Exupéry. Terre des hommes. Gallimard, 1939 (cited on page 69).
[2]		Patrick L. Bajari, Zhihao Cen, Victor Chernozhukov, Manoj Manukonda, Jin Wang, Ramon Huerta, Junbo Li, Ling Leng, George Monokroussos, Suhas Vĳaykunar, et al. Hedonic prices and quality adjusted price indices powered by AI. Tech. rep. cemmap working paper CWP04/21, 2021 (cited on page 71).
[3]		Peter J. Bickel, Ya’acov Ritov, and Alexandre B. Tsybakov. ‘Simultaneous analysis of Lasso and Dantzig selector’. In: Annals of Statistics 37.4 (2009), pp. 1705–1732 (cited on
pages 76, 91, 100).
[4]		Alexandre Belloni and Victor Chernozhukov. ‘Least Squares After Model Selection in High-dimensional Sparse Mod-els’. In: Bernoulli 19.2 (2013). ArXiv, 2009, pp. 521–547
(cited on pages 76, 78–80, 91).
[5]		Denis Chetverikov, Zhipeng Liao, and Victor Chernozhukov. ‘On cross-validated lasso in high dimensions’. In: Annals
of Statistics 49.3 (2021), pp. 1300–1317 (cited on pages 77,
80, 91, 98).
[6]	Victor Chernozhukov, Denis Chetverikov, and Kengo Kato. ‘Central Limit Theorems and Bootstrap in High Dimensions’. In: Annals of Probability 45.4 (2017), pp. 2309–
2352 (cited on pages 77, 91).
[7]	Alexandre Belloni, Daniel L. Chen, Victor Chernozhukov, and Christian B. Hansen. ‘Sparse Models and Methods for Optimal Instruments with an Application to Emi-nent Domain’. In: Econometrica 80.6 (2012). Arxiv, 2010,
pp. 2369–2429 (cited on pages 80, 91, 93).
[8]		Daniel Hsu, Sham M. Kakade, and Tong Zhang. ‘Random design analysis of ridge regression’. In: Conference on Learning Theory. Vol. 23. JMLR Workshop and Conference Proceedings. 2012, pp. 9.1–9.24 (cited on pages 84, 85, 91).
[9]		Jerome Friedman, Trevor Hastie, and Robert Tibshirani. The Elements of Statistical Learning. Vol. 1. Springer Series in Statistics, New York, 2001 (cited on page 86).
 


[10]		Hui Zou and Trevor Hastie. ‘Regularization and variable selection via the elastic net’. In: Journal of the Royal Sta-tistical Society: Series B 67.2 (2005), pp. 301–320 (cited on page 86).
[11]		Christine De Mol, Ernesto De Vito, and Lorenzo Rosasco. ‘Elastic-net regularization in learning theory’. In: Journal of Complexity 25.2 (2009), pp. 201–230. doi: https://doi. org/10.1016/j.jco.2009.01.002 (cited on page 87).
[12]		Victor Chernozhukov, Christian Hansen, and Yuan Liao. ‘A lava attack on the recovery of sums of dense and sparse signals’. In: Annals of Statistics 45.1 (2017), pp. 39–76 (cited on pages 87, 88, 91).
[13]		Domagoj Ćevid, Peter Bühlmann, and Nicolai Mein-shausen. ‘Spectral deconfounding via perturbed sparse linear models’. In: Journal of Machine Learning Research 21 (2020), pp. 1–41 (cited on pages 87, 88, 91).
[14]	Marten Wegkamp. ‘Model selection in nonparametric regression’. In: Annals of Statistics 31.1 (2003), pp. 252–273
(cited on pages 89, 98).
[15]		lldiko E. Frank and Jerome H. Friedman. ‘A statistical view of some chemometrics regression tools’. In: Techno-metrics 35.2 (1993), pp. 109–135 (cited on page 91).
[16]		Robert Tibshirani. ‘Regression shrinkage and selection via the Lasso’. In: Journal of the Royal Statistical Society: Series B 58.1 (1996), pp. 267–288 (cited on page 91).
[17]		Trevor Hastie, Robert Tibshirani, and Martin Wainwright. Statistical Learning with Sparsity: The Lasso and Generaliza-tions. Chapman & Hall/CRC, 2015 (cited on page 91).
[18]	Alexandre Belloni, Victor Chernozhukov, Christian Hansen, and Damian Kozbur. ‘Inference in High-Dimensional Panel Models With an Application to Gun Control’. In: Journal of Business & Economic Statistics 34.4 (2016),
pp. 590–605 (cited on page 91).
[19]		Victor Chernozhukov, Wolfgang Karl Härdle, Chen Huang, and Weining Wang. ‘Lasso-driven inference in time and space’. In: Annals of Statistics 49.3 (2021), pp. 1702–1735 (cited on page 91).
[20]		Alexandre Belloni, Victor Chernozhukov, and Lie Wang. ‘Square-root lasso: pivotal recovery of sparse signals via conic programming’. In: Biometrika 98.4 (2011). Arxiv, 2010, pp. 791–806 (cited on page 95).
 


[21]		Alexandre Belloni, Victor Chernozhukov, and Lie Wang. ‘Pivotal estimation via square-root lasso in nonparametric regression’. In: Annals of Statistics 42.2 (2014), pp. 757–788 (cited on page 95).
[22]		Emmanuel Candès and Terence Tao. ‘The Dantzig selec-tor: statistical estimation when 𝑝 is much larger than 𝑛’. In: Annals of Statistics 35.6 (2007), pp. 2313–2351 (cited on page 96).
[23]	Eric Gautier and Alexander B. Tsybakov. ‘High-Dimensional Instrumental Variables Regression and Confidence Sets’. In: ArXiv working report (2011) (cited on page 96).
[24]	Guillaume Lecué and Charles Mitchell. ‘Oracle inequali-ties for cross-validation type procedures’. In: Electronic Journal of Statistics 6 (2012), pp. 1803–1837 (cited on
page 98).
[25]	M. Rudelson and S. Zhou. ‘Reconstruction from anisotropic random measurements’. In: ArXiv:1106.1151 (2011) (cited on page 99).
[26]		Mark Rudelson and Roman Vershynin. ‘On sparse re-construction from Fourier and Gaussian measurements’. In: Communications on Pure and Applied Mathematics 61.8 (2008) (cited on page 99).
[27]	Alexandre Belloni, Victor Chernozhukov, Denis Chetverikov, Christian Hansen, and Kengo Kato. ‘High-dimensional econometrics and regularized GMM’. In: arXiv preprint arXiv:1806.01888 (2018) (cited on page 100).
 
Statistical Inference on Predictive
and Causal Effects in High-Dimensional Linear Regression Models


"The partial trend regression method can never, in-deed, achieve anything which the individual trend method cannot, because the two methods lead by definition to identically the same results."
(An in-words restatement of the FWL theorem.) – Ragnar Frisch and Frederick V. Waugh [1].
We return in this chapter to a familiar regression move—asking how the predicted outcome changes when we nudge one re-gressor 𝐷, holding the rest 𝑊 fixed—but we place it in the high-dimensional world where the control vector 𝑊 can con-tain hundreds or thousands of variables. Our protagonist is the coefficient 𝛼 on 𝐷 in the best linear predictor of 𝑌 given
𝐷, 𝑊 ; the goal is to do honest inference on 𝛼 even when 𝑝 is
comparable to, or larger than, 𝑛.
We build that inference in three steps. First we learn 𝛼 with the Double Lasso (partialling-out) recipe: we use Lasso to remove from 𝑌 and from 𝐷 the part that can be explained by 𝑊, and then we regress the remaining pieces on each other. Second we explain why this two-step estimator behaves as if we had known the population residuals all along—the key property is Neyman orthogonality, a low-bias insensitivity condition that will reappear throughout the book. Third we extend the same idea to many coefficients at once (for example, interaction terms that capture heterogeneous effects), where we must also account for multiplicity and construct simultaneous confidence bands.
We illustrate these ideas with two examples. We test a classic convergence hypothesis in growth economics, and we look for heterogeneity in the gender wage gap. Both examples make the same point: with high-dimensional controls, good prediction is not enough—we need procedures specifically designed for inference.
 
4
4.1	Introduction	106
4.2	Inference with Double Lasso	106
Inference on One Coeffi-cient	106
Application to Testing the Convergence Hypothesis	110
4.3	Why Partialling-out Works: Neyman Orthogonality	111
Neyman Orthogonality 111 What Happens if We Don’t Have Neyman Orthogonal-ity?	114
4.4	Inference on Many Coeffi-cients	117
Discovering Heterogeneity in the Wage Gap Analysis 120
4.5	Other Approaches That Have the Neyman Orthogonal-ity Property	122
Double Selection	122
Debiased Lasso	122
4.6	Checklist	124
4.7	Notes	124
4.8	Notebooks	125
4.9	Exercises	125
4.A High-Dimensional Central Limit Theorems★	126
 

4.1	Introduction

 
We want to learn—and quantify uncertainty about—the coeffi-cient 𝛼 on a regressor 𝐷 in the best linear predictor of 𝑌 given
𝑋 = 𝐷, 𝑊 . In plain terms, 𝛼 answers the predictive effect question:1
▶ How does the predicted value of 𝑌 change if a regressor
𝐷 increases by a unit, while other regressors 𝑊 remain unchanged?
Equivalently, if we increase 𝐷 by one unit while holding 𝑊 fixed, the fitted value from the best linear predictor changes by about 𝛼 units.
As before, we denote the set of regressors as 𝑋 = 𝐷, 𝑊 . In Chapter 1, we discussed how we could use the population regression coefficient corresponding to the variable 𝐷, denoted
𝛼, to answer this question. We also discussed how to estimate this effect and construct confidence intervals with regression. Now we turn to estimation and construction of confidence intervals for 𝛼 in the high-dimensional setting, using the tools we developed in Chapter 3.
Here we focus on using Lasso methods. We can use other penalized methods with the caveat that theoretical guarantees are not available unless we perform additional data splitting. We will discuss the use of data splitting and more general machine learning methods in detail when we introduce "double machine learning" or "debiased machine learning" in Chapter 9.

4.2	Inference with Double Lasso
Inference on One Coefficient
The key to inference will be the application of Frisch-Waugh-Lovell partialling-out. Consider the simple predictive model:
𝑌 = 𝛼𝐷 + 𝛽′𝑊 + 𝜖,	(4.2.1)
where 𝐷 is the target regressor and 𝑊 consists of 𝑝 controls. After partialling-out 𝑊,
 




1: We discuss assumptions and modeling frameworks under which the predictive effect question has a causal interpretation in detail in Chapter 5 thrChapter 11. Under the framework developed in those chapters, the tools in this chapter offer one approach to performing statistical inference for causal ef-fects. Here, we simply note that we may be interested in providing statistical inference for predictive effects regardless of whether they have a causal interpretation.
 

 
𝑌˜ = 𝛼𝐷˜
 
+ 𝜖,	E[𝜖𝐷˜ ] = 0.	(4.2.2)
 


In words, we first remove from 𝑌 whatever can be linearly explained by 𝑊, and we do the same for 𝐷; the predictive effect
𝛼 is then the slope from regressing the leftover outcome on
the leftover regressor. The variables with tildes are residuals retrieved from taking out the linear effect of 𝑊:
𝑌˜ = 𝑌 − 𝛾𝑌′ 𝑊 𝑊 ,	𝛾𝑌𝑊 ∈ arg min E[(𝑌 − 𝛾′𝑊)2],
𝐷˜ = 𝐷 − 𝛾𝐷′ 𝑊 𝑊 ,	𝛾𝐷𝑊 ∈ arg min E[(𝐷 − 𝛾′𝑊)2].
Then 𝛼 can then be recovered from population linear regression of 𝑌˜ on 𝐷˜ :
𝛼 = arg min E[(𝑌˜ − 𝑎𝐷˜ )2] = (E[𝐷˜ 2])−1E[𝐷˜ 𝑌˜ ].
Note also that 𝑎 = 𝛼 solves the “partialled-out" moment equa-tion:
E[(𝑌˜ − 𝑎𝐷˜ )𝐷˜ ] = 0.	(4.2.3)
We now consider estimation of 𝛼 in a high-dimensional setting. For estimation purposes, we maintain that we have a random
sample {(𝑌𝑖 , 𝑋𝑖)}𝑛	where 𝑋𝑖 = (𝐷𝑖 , 𝑊𝑖).
To estimate 𝛼, we will mimic the partialling-out procedure in the population in the sample. In Chapter 1, where 𝑝 𝑛 was small, we employed ordinary least squares as the prediction method in the partialling-out steps. We are now considering cases where
𝑝 𝑛 is not small, and we instead employ Lasso-based methods in the partialling-out steps.
The estimation procedure for a target parameter 𝛼 in a high-dimensional linear model setting can be summarized as fol-lows:

The Double Lasso procedure:
1. We run Lasso regressions of 𝑌𝑖 on 𝑊𝑖 and 𝐷𝑖 on 𝑊𝑖
𝛾ˆ𝑌𝑊 = arg min	I:(𝑌𝑖 − 𝛾′𝑊𝑖)2 + 𝜆1 I: 𝜓ˆ𝑌 |𝛾𝑗 |,
𝛾∈ℝ𝑝	𝑖	𝑗	𝑗
𝛾ˆ𝐷𝑊 = arg min	I:(𝐷𝑖 − 𝛾′𝑊𝑖)2 + 𝜆2 I: 𝜓ˆ 𝐷 |𝛾𝑗 |,
𝛾∈ℝ𝑝	𝑖	𝑗	𝑗
and obtain the resulting residuals:
𝑌ˇ𝑖 = 𝑌𝑖 − 𝛾ˆ𝑌′ 𝑊 𝑊𝑖 ,
𝐷ˇ 𝑖 = 𝐷𝑖 − 𝛾ˆ𝐷′ 𝑊 𝑊𝑖 .
 


In place of Lasso, we can also use Post-Lasso or other Lasso relatives (the Dantzig selector, square-root Lasso, and others).
2. We run the least squares regression of 𝑌ˇ𝑖 on 𝐷ˇ 𝑖 to obtain the estimator 𝛼ˆ:
𝛼ˆ = arg min 𝔼𝑛[(𝑌ˇ − 𝑎𝐷ˇ )2]
𝑎∈ℝ	(4.2.4)
= (𝔼𝑛[𝐷ˇ 2])−1𝔼𝑛[𝐷ˇ 𝑌ˇ ].
We can use standard results from this regression, treat-ing the residuals as if they were observed. Concretely, letting 𝜖ˆ𝑖 := 𝑌ˇ𝑖 − 𝛼ˆ𝐷ˇ 𝑖 , a heteroskedasticity-robust
estimate of the asymptotic variance V is
Vˆ = (𝔼𝑛[𝐷ˇ 2])−1𝔼𝑛[𝐷ˇ 2 𝜖ˆ2](𝔼𝑛[𝐷ˇ 2])−1 ,
so the standard error of 𝛼ˆ is ✓Vˆ/𝑛.

 
This procedure works because the partialling-out moment (4.2.3) is Neyman orthogonal – so the first-step estimation error affects
𝛼 only through higher-order terms. We explain this property
fully below.
A tempting shortcut is to run a single Lasso regression of 𝑌 on 𝐷, 𝑊 , keep the selected controls, and then refit by least squares. That “single selection” idea is fine for prediction, but it can deliver badly biased point estimates and misleading confidence intervals for 𝛼; we return to this failure mode after we explain Neyman orthogonality.
Good performance of the Double Lasso procedure relies on approximate sparsity of the population regression coefficients
𝛾𝑌𝑊 and 𝛾𝐷𝑊 , with a sufficiently high speed of decrease in the
sorted coefficients and on careful choice of the Lasso tuning parameters. For approximate sparsity, we will impose that the sorted coefficients satisfy
|𝛾𝑌𝑊 |(𝑗) ≤ 𝐴𝑗−𝑎 and	|𝛾𝐷𝑊 |(𝑗) ≤ 𝐴𝑗−𝑎
for 𝑎 > 1 and 𝑗 = 1, . . . , 𝑝.2 The stronger decay requirement
𝑎 > 1 reflects that we are aiming for √𝑛-rate inference on 𝛼, not
just good prediction of 𝑌 and 𝐷; the first-step errors must be small enough that their impact disappears in the second step.
Under these sparsity conditions, we can use the plug-in rule outlined in Chapter 3 for choosing 𝜆1 and 𝜆2. Importantly,
 
























2: Note that in this case the effec-tive dimension 𝑠 of the problem is
𝑠	𝐴1/𝑎 𝑛1/2𝑎	𝑛1/2. Intuitively, the effective number of non-zero
coefficients grows slower than √𝑛.
 

 
using these tuning parameters theoretically guarantees that we produce high quality prediction rules for 𝐷 and 𝑌 while si-multaneously avoiding overfitting under approximate sparsity. Absent these guarantees, we cannot theoretically ensure that
first step estimation of 𝐷ˇ and 𝑌ˇ does not have first-order impacts
on the final estimator 𝛼. Although using cross-validated Lasso in the partialling-out steps achieves similar theoretical perfor-mance, we have found that it frequently overfits in moderately sized samples, leading to poor practical performance in simula-tions. We revisit this issue in Chapter 9 by introducing double (or debiased) machine learning. There, we show how flexible learners—including Lasso and other regularized estimators with data-driven tuning—can be used without sacrificing valid inference. The crucial mechanism is cross-fitting, which guards against overfitting in the nuisance estimation steps.3
To fix ideas, imagine that we could observe the population
residuals 𝑌˜ and 𝐷˜ from (4.2.2). Then ordinary least squares of
 

















3: As a brief preview, cross-fitting is an efficient form of sample split-ting. We estimate the nuisance func-tions on 𝐾 − 1 folds of the data
 
𝑌˜ on 𝐷˜ would deliver the usual √𝑛-rate approximately normal
inference for 𝛼. The content of the Double Lasso theory is that, under approximate sparsity and suitable tuning, the feasible procedure that uses estimated residuals behaves as if these oracle residuals were known.
 
and evaluate the orthogonal scores (residuals, in this chapter) on the remaining held-out fold. Cycling through each held-out fold yields a complete set of cross-fitted scores, which we then use to compute the final estimator of the target param-eter.
 









We note that the leading term √𝑛 𝔼𝑛[𝐷˜ 𝜖]/𝔼𝑛[𝐷˜ 2] is is exactly what we would obtain if we regressed the oracle residual 𝑌˜ on
𝐷˜ .
The above statement means that 𝛼ˆ concentrates in a ✓V/𝑛-
neighborhood of 𝛼, with deviations controlled by the normal law. Observe that the approximate behavior of the Double Lasso estimator is the same as the approximate behavior of the least squares estimator in low-dimensional models; see Theorem
1.4.2 in Chapter 1.
Just like in the low-dimensional case, we can use these results to construct a confidence interval for 𝛼. The standard error of 𝛼ˆ
 

is
Vˆ/𝑛,
where Vˆ is a plug-in estimator of V. The result implies, for
example, that the interval
[𝛼ˆ ± 1.96JVˆ/𝑛] covers 𝛼 about 95% of the time.

 
Application to Testing the Convergence Hypothesis
We provide an empirical example of partialling-out with Lasso to estimate the regression coefficient 𝛼 in the high-dimensional linear regression model:
𝑌 = 𝛼𝐷 + 𝛽′𝑊 + 𝜖.

In this example, we are interested in how economic growth rates (𝑌) are related to the initial wealth levels in each country (𝐷) controlling for a country’s institutional, educational, and other similar characteristics (𝑊).
The relationship is captured by 𝛼, the "speed of convergence/-divergence," which predicts the speed at which poor countries catch up 𝛼 < 0 or fall behind 𝛼 > 0 rich countries, after controlling for 𝑊. Here, we are interested in understanding if poor countries grow faster than rich countries, controlling for educational and other characteristics. In other words, is the speed of convergence negative: Is 𝛼 < 0?4
In our data, the outcome (𝑌) is the realized annual growth rate of a country’s wealth (Gross Domestic Product per capita). The target regressor (𝐷) is the initial level of the country’s wealth. The controls (𝑊) include measures of education levels, quality of institutions, trade openness, and political stability in the country. The sample, which is based on the Barro-Lee data set [2], contains 90 countries and about 60 controls. Thus
𝑝  60, 𝑛 = 90 and 𝑝 𝑛 is not small. We expect the least squares method to provide a poor/ noisy estimate of 𝛼. We expect
the method based on partialling-out with Lasso to provide a high-quality estimate of 𝛼.
To keep notation straight, 𝑌𝑖 is the realized annual growth rate of GDP per capita, 𝐷𝑖 is the initial level of GDP per capita, and
 




R Notebook on Double Lasso for Growth Convergence and Python Notebook on Double Lasso for Growth Convergence provides code for the convergence hypothe-sis example.














4: The hypothesis 𝛼 < 0 corre-sponds to the Convergence Hypoth-esis predicted by the Solow growth model. Robert M. Solow is a world-renowned MIT economist who won the Nobel Prize in Economics in 1987.
 


𝑊𝑖 collects the additional country characteristics we control for.


OLS	Estimate
-0.009	Std. Error 0.032	95% CI
[-0.073, 0.054]	Table 4.1: Estimates for the conver-
gence coefficient. We report specifi-
cation robust standard errors with
Double Lasso	-0.045	0.018	[-0.080, -0.010]	finite sample correction, i.e., "HC1."


Least squares provides a rather noisy estimate of convergence speed, which does not allow drawing strong conclusions about the convergence hypothesis. For example, the 95% confidence interval is wide and includes both positive and negative val-ues. Given that 𝑝 𝑛 is not small in this example, we should also be highly skeptical of the OLS results and especially the standard error. For example, [3] show that conventional robust standard errors are not even consistent in linear models when
𝑝 𝑛 is not small. In sharp contrast, Double Lasso provides a precise estimate for which we can obtain theoretically jus-tified inferential statements even though 𝑝 𝑛 is not close to
0. The Lasso-based point estimate is	4.5% and the 95% con-fidence interval for the (annual) convergence rate is	8% to 1%. This empirical evidence is consistent with the conditional
convergence hypothesis.
Above, after using Lasso to estimate the residuals 𝑌ˇ and 𝐷ˇ ,
we ran an ordinary least squares regression and then used off-the-shelf standard errors as if those residuals were known. Why is it legitimate to ignore the first-step estimation when we build confidence intervals for 𝛼? The next section answers this question; the short answer is Neyman orthogonality.

4.3	Why Partialling-out Works: Neyman Orthogonality
Neyman Orthogonality
In the Double Lasso approach, 𝛼 is the target parameter and 𝜂
are nuisance projection parameters with true value
𝜂𝑜 = (𝛾𝐷′ 𝑊 , 𝛾𝑌′ 𝑊 )′.
As the learned value 𝛼ˆ of 𝛼 depends on the values of the
nuisance parameters, it is useful to explicitly consider the
 


dependence of 𝛼ˆ on the nuisance parameters:
𝛼ˆ(𝜂).
For the majority of the estimation processes we will describe in this book, we can construct a population analogue
𝛼(𝜂)
of the estimator 𝛼 𝜂 , such that the in-sample estimation proce-dure converges to it, in a formal sense.
For instance, the Double Lasso process constructs the residu-als
𝑌ˇ𝑖 (𝜂) = 𝑌𝑖 − 𝜂′1𝑊𝑖 ,	𝐷ˇ 𝑖(𝜂) = 𝐷𝑖 − 𝜂′2𝑊𝑖
and then obtains 𝛼 𝜂 as the solution to the empirical estimating equation
-	ˇ	ˇ	ˇ
This process implicitly defines the function 𝛼 𝜂 . We can think of the population analog of this process, where we construct the residuals
𝑌˜ (𝜂) = 𝑌 − 𝜂′1𝑊 ,	𝐷˜ (𝜂) = 𝐷 − 𝜂′2𝑊
and solve the population moment equation
M(𝑎, 𝜂) := E[(𝑌˜ (𝜂) − 𝑎𝐷˜ (𝜂))𝐷˜ (𝜂)] = 0,	(4.3.1) which again implicitly defines the function 𝛼(𝜂).
The main idea of the Double Lasso approach is that, in the
population limit, it corresponds to a procedure for learning the target parameter 𝛼 that is first-order insensitive to local perturbations of the nuisance parameters around their true
values, 𝜂𝑜:	Formally, we use 𝜕𝜂 to denote the di-
rectional derivative (Gateaux) with
 
𝜕𝜂𝛼(𝜂𝑜) = 0.	(4.3.2)
We will call the local insensitivity of target parameters to nui-sance parameters as in (4.3.2) Neyman orthogonality of the estimation process.
Neyman orthogonality is important for providing high-quality estimation and inference, especially in high-dimensional set-tings. In high-dimensional settings, we use regularization pro-cedures to estimate the nuisance parameters as solutions to suitable prediction problems. The use of regularization gen-
 
respect to 𝜂.
 


erally results in bias, and we may heuristically view using regularized estimates of nuisance parameters as plugging in estimates of these parameters that are close to, but not exactly equal to, the true values of the nuisance parameters 𝜂𝑜. Neyman orthogonality, which guarantees that the target parameter is locally insensitive to perturbations of the nuisance parameters around their true values, then ensures that this bias does not transmit to the estimation of the target parameter, at least to the first order.
Let us prove the claim 𝜕𝜂𝛼(𝜂𝑜) = 0 for the Double Lasso process. Since the function 𝛼(𝜂) is implicitly defined as the solution to the equation M(𝑎, 𝜂) = 0, by the implicit function theorem and letting 𝛼 = 𝛼(𝜂𝑜):
𝜕𝜂𝛼(𝜂𝑜) = −𝜕𝑎M(𝛼, 𝜂𝑜)−1 𝜕𝜂M(𝛼, 𝜂𝑜).
 
Here
 

𝜕𝜂M(𝛼, 𝜂𝑜)
 
consists of two components
𝜕𝜂1 M(𝛼, 𝜂𝑜) = E[𝑊 𝐷˜ (𝜂𝑜)] = E[𝑊(𝐷 − 𝛾𝐷′ 𝑊 𝑊)] = 0
and
𝜕𝜂2 M(𝑎, 𝜂) = E 𝑎𝑊 𝐷˜ (𝜂) − 𝑊(𝑌˜ (𝜂) − 𝑎𝐷˜ (𝜂))
= −E[𝑊𝑌˜ (𝜂)] + 2𝑎 E[𝑊 𝐷˜ (𝜂)].
Evaluating at 𝑎 = 𝛼 and 𝜂 = 𝜂𝑜 yields
𝜕𝜂2 M(𝛼, 𝜂𝑜) = −E[𝑊(𝑌 − 𝛾𝑌′ 𝑊 𝑊)] + 2E[𝛼𝑊(𝐷 − 𝛾𝐷′ 𝑊 𝑊)] = 0. We summarize the discussion as follows:
Neyman Orthogonality. The parameter of interest 𝛼 that depends on nuisance parameters 𝜂 with true value 𝜂𝑜 is Neyman orthogonal with respect to these parameters if
𝑜
𝜕𝜂𝛼(𝜂 ) = 0.
If the parameter 𝛼 is defined as a root in 𝑎 of the equation M(𝑎, 𝜂) = 0, which depends on the nuisance parameters 𝜂 with true value 𝜂𝑜, then the equation is Neyman orthogonal
if
𝜕𝜂M(𝛼, 𝜂𝑜) = 0.
 


The principle is applicable to problems outside the high-dimensional linear model problem considered in this chap-ter.

What Happens if We Don’t Have Neyman Orthogonality?
If we don’t have Neyman orthogonality, we should not expect to get high-quality estimates of the target parameters. For example, a seemingly sensible approach that one might consider for statistical inference in the high-dimensional linear model context is as follows:

(Invalid) Single Selection/Naive Method.
In this invalid method, one applies Lasso regression of 𝑌 on
𝐷 and 𝑊 to select relevant covariates 𝑊𝑌, in addition to the covariate of interest, then refits the model by least squares of 𝑌 on 𝐷 and 𝑊𝑌. Inference for the target parameter is then carried out using conventional inference based on the latter regression.

Despite its simplicity and seeming intuitive appeal, the ap-proach outlined above is not a valid approach if the goal is to perform inference on 𝛼. It is a fine approach if the goal is solely the prediction of the outcome, but it can result in very misleading conclusions about the parameter of interest 𝛼, as we demonstrate in Example 4.3.1 below.
The naive approach outlined above relies on the moment con-dition
 
M(𝑎, 𝑏) = E[(𝑌 − 𝑎𝐷 − 𝑏′𝑊)𝐷] = 0.
When 𝑏 = 𝛽, this moment condition is satisfied by the true value, 𝑎 = 𝛼. In this case, it coincides with the classical moment condition for 𝛼 underlying low-dimensional ordinary least
squares which sets prediction errors to be orthogonal to each predictor variable.
However, this moment condition does not exhibit Neyman orthogonality since
𝜕𝑏M(𝛼, 𝛽) = E[𝐷𝑊] ≠ 0
unless 𝐷 is orthogonal to 𝑊.5 Because M(𝑎, 𝑏) is not Neyman
 










5: In "pure" RCTs where treatment is assigned independently of ev-erything, 𝐷’s are orthogonal to 𝑊, after de-meaning 𝐷, so Neyman or-thogonality automatically holds in this setting.
 


orthogonal, the bias and the slower than parametric rate of convergence,
𝑠 log(𝑝 ∨ 𝑛)/𝑛,
of our estimate of 𝛽′𝑊 will transmit to bias and slower than √𝑛 convergence in estimates of 𝛼 provided by solving the empirical analog of M 𝑎, 𝑏 . The "Single Selection" procedure outlined above exactly provides the solution to this moment condition.
Consequently, while this naive procedure provides an estimator of 𝛼 that will approach the true value in large samples (at
a slower than √𝑛-rate), the bias of the estimator converges
too slowly for standard inference methods to provide reliable inference.
We can set up a simulation experiment to verify that this naive approach provides low-quality estimates for 𝛼.

The reason that the naive estimator does not perform well is that it only selects controls that are strong predictors of the outcome, thereby omitting weak predictors of the outcome. However, weak predictors of the outcome could still be strong predictors of 𝐷, in which case dropping these controls results in a strong omitted variable bias. In contrast, the orthogonal approach solves two prediction problems – one to predict 𝑌 and another to predict 𝐷 – and finds controls that are relevant for either. The resulting residuals are therefore approximately "de-confounded."
 
















































Figure 4.1: Top Panel: Simulated distribution of the orthogonal es-timator centered around the true value. Bottom Panel: Simulated distribution of the naive (single-selection) non-orthogonal estima-tor centered around the true value.
 

4.4	Inference on Many Coefficients
In many empirical questions, we care about a whole family of related coefficients at once—a main effect together with a collection of interaction effects, for example—and we would like uncertainty statements that remain honest even after we look across many estimates.
If we are interested in more than one coefficient, we can repeat the one-by-one Double Lasso procedure for each of the coeffi-cients of interest and obtain valid estimation and inference on each component under regularity conditions.
We consider the model
 

𝑌	=
Outcome
 
𝑝1
𝛼ℓ 𝐷ℓ
ℓ =1
Target Predictors
 
𝑝2
𝛽𝑗𝑊¯ 𝑗
𝑗=1
 
Controls
 

+ 𝜖,
 
where we use 𝐷ℓ for ℓ = 1, ..., 𝑝1 to denote the predictors of interest and 𝑊¯ 𝑗 for 𝑗 = 1, ..., 𝑝2 to denote other predictors in the model. Here, the number of predictors of interest, 𝑝1, and the
number of additional variables, 𝑝2, can both be very large.
There are at least three motivations for considering many coeffi-cients of interest:
▶ there can be multiple policies whose predictive effect we would like to infer;
▶ we can be interested in heterogeneous predictive effects across pre-specified groups;
▶ we can be interested in nonlinear effects of policies.
This setting encompasses examples where we are interested in
heterogeneous effects, where 𝐷ℓ′ 𝑠 are generated as
𝐷ℓ = 𝐷0𝑋¯ ℓ ,	ℓ = 1, ..., 𝑝1,
where 𝐷0 is a base variable of interest – for example, a treatment
indicator, a price, or a group indicator – and (𝑋¯ ℓ )𝑝1  are known
 
transformations of controls 𝑊¯
indicators.
 
– for example, various subgroup
 
The setting also encompasses cases where nonlinear effects are of interest. For example, we could consider 𝐷ℓ ’s generated as polynomial transformations of a multi-valued base variable, such as a price:
𝐷ℓ = 𝐷ℓ ,	ℓ = 1, ..., 𝑝1.
 


We could further interact these transformations with other variables to study nonlinear heterogeneous effects.

One by One Double Lasso for Many Target Parameters. For each ℓ = 1, ..., 𝑝1, we apply the one-by-one Double Lasso procedure for estimation and inference on the coefficient
𝛼ℓ in the model
𝑌 = 𝛼𝑙 𝐷ℓ + 𝛾ℓ′𝑊ℓ + 𝜖,	𝑊ℓ = ((𝐷𝑘)′𝑘≠ℓ , 𝑊¯ ′)′.

Under approximate sparsity conditions, the Double Lasso
𝑝1
 
method provides a high-quality estimate 𝛼
𝑝1
 
= (𝛼ˆℓ )ℓ =1 of 𝛼 =
 
𝛼ℓ ℓ =1 that is approximately Gaussian. We can thus easily construct individual confidence intervals or even joint confi-
dence bands. Under regularity conditions, these results allow for simultaneous inference on 𝑝1 > 𝑛 coefficients.

Recall that the above distributional approximation formally means that
sup P √𝑛(𝛼ˆ − 𝛼) ∈ 𝑅 − P 𝑁(0, V) ∈ 𝑅	→ 0,
𝑅∈R
where R is a collection of all (hyper) rectangles. The latter result allows the construction of simultaneous confidence bands on all target parameters 𝛼ℓ ’s of the form:
𝐶-𝑅 = ×ℓ =1 J𝛼ˆℓ ± 𝑐JVˆℓℓ /𝑛l ,
The critical value 𝑐 in the simultaneous confidence band is
 


chosen so that
P(𝛼 ∈ 𝐶𝑅) = P (√𝑛(𝛼 − 𝛼ˆ) ∈ √𝑛(𝐶ˆ𝑅 − 𝛼ˆ))
= P (√𝑛(𝛼ℓ − 𝛼ˆℓ ) ∈ [±𝑐Vˆ1/2] ∀ ℓ ∈ {1, ..., 𝑝1})
≈ 1 − a
where 1 − a denotes the confidence level.


 


 

Discovering Heterogeneity in the Wage Gap Analysis
We now return to the CPS wage data from Chapter 1, but we change the question. Instead of asking for a single “average” wage gap, we ask whether the gap appears different across education groups, regions, and levels of experience. In the lan-guage of the previous section, this means that we are interested in a family of target coefficients: the main effect of the female indicator together with a collection of interaction effects.
Let sex denote the female indicator (equal to 1 for women and 0 for men), and keep the log hourly wage as the outcome. We build a set of pre-specified effect modifiers: four education indicators—Some High School (shs), High School Graduate (hsg), Some College (scl), and College Graduate (clg)—with Advanced De-gree as the omitted reference group; three region indicators—Midwest (mw), South (so), and West (we)—with Northeast as the omitted region; and a fourth-degree polynomial in potential experience, where exp1= Experience, exp2= Experience2/100,
exp3= Experience3/1000, and exp4= Experience4/10000.
Our target predictors are sex itself together with the interactions sex:shs, sex:hsg, sex:scl, sex:clg, sex:mw, sex:so, sex:we, and sex:exp1–sex:exp4, for a total of 12 coefficients. Before forming interactions, we center each modifier by subtracting its sample mean, while leaving sex uncentered. This centering makes interpretation clean: the coefficient on sex is the predicted wage gap at the average values of the modifiers, while each interaction coefficient tells us how the gap changes as we move the corresponding modifier away from its mean.
To absorb a rich set of confounders for wages, we add a high-dimensional control set: all pairwise interactions among the modifiers (excluding sex) together with one-hot encodings for
 



sex	Estimate
-0.07	Std. Error 0.01	p-value 0.00	Table 4.2: Estimates of Heteroge-neous Predictive Effects in the CPS 2015 data
sex:shs	-0.32	0.19	0.08	
sex:hsg	0.05	0.05	0.29	
sex:scl	0.03	0.05	0.49	
sex:clg	0.06	0.05	0.19	
sex:mw	-0.12	0.04	0.01	
sex:so	-0.08	0.04	0.06	
sex:we	-0.03	0.04	0.50	
sex:exp1	0.02	0.01	0.12	
sex:exp2	-0.04	0.07	0.59	
sex:exp3	-0.05	0.03	0.18	
sex:exp4	-0.00	0.00	0.98	


occupation and industry, for a total of 990 control variables. We then apply the one-by-one Double Lasso procedure from the previous section to each target coefficient and construct both pointwise 𝑝-values and a 95% simultaneous confidence band across the 12 targets.
Table 4.2 reports point estimates, standard errors, and pointwise
𝑝-values. Table 4.3 reports the corresponding 95% simultaneous confidence band. Rows give variable names, with “:” indicating an interaction; for example, the row sex:shs corresponds to the interaction between sex and shs.
The main effect on sex is around 0.07 and is precisely esti-mated, suggesting a sizeable wage gap at the average values of the modifiers. The interaction estimates suggest patterns—for example, a larger gap for workers with less schooling and in the Midwest—but once we account for multiplicity using the simultaneous confidence band, all interaction coefficients in-clude zero. In this example, we therefore do not find statistically strong evidence of heterogeneity in the wage gap along these dimensions.
 



sex	Estimate
-0.07	CI lower
-0.11	CI upper
-0.03	Table 4.3: Simultaneous 95% Confi-
dence Intervals for the Estimates of
Heterogeneous Predictive Effects
sex:shs	-0.32	-0.85	0.20	in the CPS 2015 data.
sex:hsg	0.05	-0.09	0.20	
sex:scl	0.03	-0.11	0.17	
sex:clg	0.06	-0.07	0.19	
sex:mw	-0.12	-0.25	0.00	
sex:so	-0.08	-0.20	0.04	
sex:we	-0.03	-0.16	0.10	
sex:exp1	0.02	-0.01	0.04	
sex:exp2	-0.04	-0.23	0.16	
sex:exp3	-0.05	-0.14	0.05	
sex:exp4	-0.00	-0.01	0.01	

4.5	Other Approaches That Have the Neyman Orthogonality Property
Double Selection
One way to fix the naive "single selection" approach outlined in Section 4.3 would be to have "double selection":

Double Selection
▶ find controls 𝑊𝑌 that predict 𝑌 as judged by lasso;
▶ find controls 𝑊𝐷 that predict 𝐷 as judged by lasso;
▶ regress 𝑌 on 𝐷 and the union of controls 𝑊𝑌 ∪ 𝑊𝐷; proceed with standard inference.

This procedure is approximately equivalent to the partialling out approach, and therefore inherits the orthogonality property. This approach is more conservative compared to single selection, as it makes sure that we have not omitted controls that are strong confounders for 𝐷. It therefore guards against large omitted variable biases.

Debiased Lasso
Yet another procedure that has the orthogonality property and is approximately equivalent to the partialling out approach under suitable conditions is the debiased (also called desparsified) Lasso.
 


This approach uses the fact that 𝑎 = 𝛼 solves the equation,
M(𝑎, 𝜂) = E[(𝑌 − 𝑎𝐷 − 𝑏′𝑊)𝐷˜ (𝛾)] = 0,
when 𝜂 = 𝑏′, 𝛾′ ′ = 𝜂𝑜 := 𝛽′, 𝛾𝐷′ 𝑊 ′ for 𝛾𝐷𝑊 the best linear predictor coefficient from regressing 𝐷 onto 𝑊 and
𝐷˜ (𝛾) = 𝐷 − 𝛾′𝑊.
One can verify that
𝛼(𝜂) = (E[𝐷𝐷˜ (𝛾)])−1 E [(𝑌 − 𝑏′𝑊)𝐷˜ (𝛾)] ,
 
and that
 

𝛼 = 𝛼(𝜂𝑜).
 
Further, the moment condition is Neyman orthogonal – verifi-cation of which is left to the reader – which implies that
𝜕𝜂𝛼(𝜂𝑜) = 0, similarly to the argument for Double Lasso.

Debiased Lasso
▶ Run a Lasso estimator with suitable choice of 𝜆 as discussed in Chapter 3 of 𝑌 on 𝐷 and 𝑊, and save the coefficient estimate 𝛽ˆ.
▶ Run a Lasso estimator with suitable choice of 𝜆 as
discussed in Chapter 3 of 𝐷 on 𝑊 and save the coefficient estimate 𝛾ˆ.
▶ The estimator 𝛼ˆ is then the solution of the empirical
analog of the moment condition above:
𝔼𝑛[(𝑌 − 𝛼ˆ𝐷 − 𝛽ˆ′𝑊 )𝐷˜ (𝛾ˆ)] = 0, which has the explicit form
𝛼ˆ = (𝔼𝑛[𝐷𝐷˜ (𝛾ˆ)])−1 𝔼𝑛 [(𝑌 − 𝛽ˆ′𝑊 )𝐷˜ (𝛾ˆ)] ,
where 𝛽ˆ and 𝛾ˆ are Lasso estimators.

Estimators of this form are referred to in econometrics as "instrumental variable estimators." In purely technical terms,
we are using residualized 𝐷˜ to "instrument" for 𝐷.
 

4.6	Checklist
After reading this chapter, you should be able to:
▶ Define the predictive effect parameter 𝛼 in a high di-mensional linear regression model, and explain when (under additional identifying assumptions) it can also be interpreted as a causal effect. Use Frisch–Waugh–Lovell partialling-out to express 𝛼 as the slope from regressing the residualized outcome on the residualized regressor.
▶ Implement the Double Lasso procedure (two Lasso pre-diction steps followed by a low-dimensional regression), including the construction of heteroskedasticity-robust standard errors and confidence intervals.
▶ Explain Neyman orthogonality in words and in equations, and verify why the Double Lasso score is orthogonal. Understand why “single selection” (naive post-Lasso OLS) can fail for inference, even when it looks sensible for prediction.
▶ Extend Double Lasso to many target coefficients and construct simultaneous confidence bands; distinguish simultaneous bands from marginal confidence intervals.
▶ Recognize closely related orthogonal approaches, such as double selection and the debiased/desparsified Lasso.

4.7	Notes
We mainly follow the Double Lasso approach developed in [7] and [8], because it is nicely connected to the classical partialling out. Desparsified Lasso was developed by [9] and [10]; a closely related approach is the debiased Lasso proposed by [11]. All of these approaches could be called "debiased" Lasso and will generalize later to the approach called Debiased Machine Learning. The Double Selection method was developed by
[12] and [13]. Inference on many coefficients was developed by [14], [15], and [16]. [17] provide results for Double Lasso with clustered dependence. The Double Lasso and desparsified Lasso approaches have also been extended to time series and many time series by [18]. Both [17] and [18] take into account the temporal dependencies in the data when fitting Lasso and performing inference on the coefficients of interest.
Failure of single selection even when 𝑝 is small is discussed in simple terms in [13], but the problem was first systematically
 


examined by [19]. A recent paper [20] develops debiasing meth-ods for shape constrained high-dimensional linear regression models.
[4] provide a recent survey on methods for simultaneous infer-ence in high-dimensional settings.
For an in-depth analysis of heterogeneity in the wage gap based on Lasso, we refer to [21].

4.8	Notebooks
Notebook 4.8.1 (Orthogonal vs Non-Orthogonal Learning) R Notebook with Experiment on Orthogonal vs Non-Orthogonal Learning and Python Notebook with Experiment on Orthog-onal vs Non-Orthogonal Learning presents the simulation experiment comparing orthogonal (partialling-out) with non-orthogonal learning (naive method).


Notebook 4.8.2 (Hard Sparsity on Orthogonal vs Non-Orthog-onal) R Notebook with Hard Sparsity on Orthogonal vs Non-Orthogonal Learning and Python Notebook with Hard Spar-sity on Orthogonal vs Non-Orthogonal Learning presents an alternative simulation to that shown in the main text comparing orthogonal (partialling-out) with non-orthogonal learning. In this simulation, we consider orthogonal and non-orthogonal learning in a stylized treatment effects simulation.

Notebook 4.8.3 (Double Lasso for Growth Convergence) R Notebook on Double Lasso for Growth Convergence and Python Notebook on Double Lasso for Growth Convergence presents a Double Lasso analysis of the conditional conver-gence hypothesis in growth economics.

Notebook 4.8.4 (Double Lasso for the Heterogeneous Wage Gap) R Notebook on Double Lasso for the Heterogeneous Wage Gap and Python Notebook on Double Lasso for the Heterogeneous Wage Gap presents a Double Lasso analysis of the heterogeneous wage gap.


4.9	Exercises
 


Exercise 4.9.1 Experiment with the first Notebooks 4.8.1. Try different models. For example, try different coefficient struc-tures for 𝛽 and 𝛾𝐷𝑊 and/or different covariance structures for 𝑊. Provide an explanation to a friend for what each step in the Double Lasso procedure is doing.

Exercise 4.9.2 (Double Lasso for Growth Convergence) Ex-plore the Notebooks 4.8.3. Provide an explanation to a friend for what each step in the Double Lasso procedure is doing. Explain the empirical results to a friend. Experiment with mak-ing the set of controls more flexible and higher-dimensional by adding nonlinear and/or interaction terms that seem po-tentially interesting. Comment on how the results differ from the baseline results.

Exercise 4.9.3 (Double Lasso for the Heterogeneous Wage Gap) Explore the Notebooks 4.8.4. Provide an explanation to a friend for what each step in the inference procedure is doing. Explain the empirical results to a friend.

Exercise 4.9.4 (Neyman Orthogonality) Verify that Neyman orthogonality holds for the "de-sparsified" Lasso strategy.


4.A High-Dimensional Central Limit Theorems★
Let 𝑋1, . . . , 𝑋𝑛 be independent (but not necessarily identically distributed) random vectors with dimension 𝑝. Assume that
𝑋𝑖’s have mean zero (otherwise, work with 𝑋𝑖  E 𝑋𝑖 instead of 𝑋𝑖 ). Consider the scaled sample mean
1	𝑛
𝑆𝑛 = √𝑛 I: 𝑋𝑖 .
Let 𝜎¯ , 𝜎 be given positive constants such that 𝜎 ⩽ 𝜎¯ , and let
𝐵𝑛 ⩾ 1 be a sequence of constants that may diverge as 𝑛 → ∞.
Let Σ𝑛 = E [𝑆𝑛𝑆𝑇 ] = 𝑛−1 '5:𝑛	E [𝑋𝑖 𝑋𝑇 ] . Also, let R denote
the collection of closed rectangles in ℝ𝑝.
We first present a high-dimensional CLT over the rectangles under a sub-exponential condition on the coordinates. Suppose that the coordinates of 𝑋𝑖 are sub-exponential with scale 𝐵𝑛,
 

 
then
 


sup |P (𝑆𝑛 ∈ 𝑅) − P(𝑁(0, Σ𝑛) ∈ 𝑅)| ≈ 0,	(4.A.1)
∈
 
provided that 𝐵2 log5 𝑝𝑛 𝑛 0. Note that this allows 𝑝 to be much larger than 𝑛. It turns out that a similar result applies without sub-exponential conditions, as stated formally below.
To state the results in a finite-sample form, let

 

𝛿1,𝑛 := for 𝑞 > 2.
 
2	5	1 4
𝑛
 
𝑛
 

2,𝑛 :=
 

 
2	3 2 𝑞
𝑛

 
𝑛1−2/𝑞
 
 
Notably, the above theorem does not impose any restrictions on the correlation structure between the coordinates of the random vectors, so Σ𝑛 is permitted to be singular.
As discussed in [23], the assumption of Part (A) is satisfied if, for example, 𝑋𝑗𝑖 ⩽ 𝐵𝑛 for all 𝑖, 𝑗 , but also allows for unbounded coordinates. Part (B) covers the following scenario relevant to regression applications: 𝑋𝑖 = 𝜖𝑖 𝑧𝑖 where 𝜖𝑖 is a univariate
"error" term while 𝑧𝑖 ∈ ℝ𝑝 is a vector of fixed "covariates." In
this case, E [∥𝑋𝑖 ∥𝑞 ] ⩽ ∥𝑧𝑖 ∥𝑞 E [|𝜖𝑖 |𝑞 ] , so if the covariates are
uniformly bound∞ed and the∞ -th moments of the error terms
are bounded, then 𝐵𝑛 = 𝑂(1). Notably this only requires 𝜖𝑖 to have 𝑞 = 2 + 𝛿 bounded moments.
 

 
Often, statistics of interest are not exactly sample means, but can be well approximated by sample means. For example, the Dou-ble Lasso estimator, 𝛼 = 𝔼𝑛 𝐷ˇ 2 −1𝔼𝑛 𝐷ˇ 𝑌ˇ   E 𝐷˜ 2 −1𝔼𝑛 𝐷˜ 𝑌˜ ,
takes this form. In order to claim a High-Dimensional CLT for
such statistics, we need the approximation error to vanish at

 










for Gaussian vectors over rectangles; see [23] for the proof.
 






6: The requirement that approxi-mation error, denoted R𝑛, vanishes faster than 1 log 𝑝 arises from the fact that the maximum of a Gaus-sian random vector 𝑁 0, Σ concen-
trates in (i.e., places a probability
 
mass of near 1 to) a 1 log 𝑝- neigh-borhood of its expected value, but not in smaller neighborhoods (anti-concentration). The approximation error R𝑛 needs to be much smaller than the size of the neighborhood. Otherwise, the probabilistic errors
incurred by Gaussian approxima-tion to the distribution of 𝑆ˆ can
be as large as 1, meaning that the Gaussian approximation fails.
 


Bibliography



[1]		Ragnar Frisch and Frederick V Waugh. ‘Partial time regressions as compared with individual trends’. In: Econometrica (1933), pp. 387–401 (cited on page 105).
[2]	Robert Barro and Jong-Wha Lee. ‘A new data set of edu-cational attainment in the world, 1950–2010’. In: Journal of Development Economics 104.C (2013), pp. 184–198 (cited on page 110).
[3]		Matias D. Cattaneo, Michael Jansson, and Whitney K. Newey. ‘Inference in linear regression models with many covariates and heteroscedasticity’. In: Journal of the Amer-ican Statistical Association 113.523 (2018), pp. 1350–1361 (cited on page 111).
[4]		Philipp Bach, Victor Chernozhukov, and Martin Spindler. Valid Simultaneous Inference in High-Dimensional Settings (with the hdm package for R). 2018. doi: 10.48550/ARXIV. 1809.04951. url: https://arxiv.org/abs/1809.04951 (cited on pages 119, 125).
[5]	Yoav Benjamini and Daniel Yekutieli. ‘False Discovery Rate–Adjusted Multiple Confidence Intervals for Se-lected Parameters’. In: Journal of the American Statisti-cal Association 100.469 (2005), pp. 71–81. doi: 10.1198/
016214504000001907 (cited on page 120).
[6]	Alexandre Belloni, Victor Chernozhukov, Denis Chetverikov, Christian Hansen, and Kengo Kato. ‘High-dimensional econometrics and regularized GMM’. In: arXiv preprint arXiv:1806.01888 (2018) (cited on page 120).
[7]		Victor Chernozhukov, Christian Hansen, and Martin Spindler. ‘Valid post-selection and post-regularization inference: An elementary, general approach’. In: Annual Review of Economics 7.1 (2015), pp. 649–688 (cited on
page 124).
[8]		Alexandre Belloni, Victor Chernozhukov, and Lie Wang. ‘Pivotal estimation via square-root lasso in nonparametric regression’. In: Annals of Statistics 42.2 (2014), pp. 757–788 (cited on page 124).
[9]		Cun-Hui Zhang and Stephanie S. Zhang. ‘Confidence intervals for low dimensional parameters in high dimen-sional linear models’. In: Journal of the Royal Statistical Society: Series B 76.1 (2014), pp. 217–242 (cited on page 124).
 


[10]	Sara Van de Geer, Peter Bühlmann, Ya’acov Ritov, and Ruben Dezeure. ‘On asymptotically optimal confidence regions and tests for high-dimensional models’. In: Annals of Statistics 42.3 (2014), pp. 1166–1202 (cited on page 124).
[11]		Adel Javanmard and Andrea Montanari. ‘Confidence intervals and hypothesis testing for high-dimensional regression’. In: Journal of Machine Learning Research 15.1 (2014), pp. 2869–2909 (cited on page 124).
[12]		Alexandre Belloni, Victor Chernozhukov, and Christian B. Hansen. ‘Inference for High-Dimensional Sparse Econo-metric Models’. In: Advances in Economics and Econometrics: Tenth World Congress. Ed. by Daron Acemoglu, Manuel Arellano, and Eddie Dekel. Vol. 3. Econometric Society Monographs. Cambridge University Press, 2013, pp. 245–
295. doi: 10 . 1017 / CBO9781139060035 . 008 (cited on
page 124).
[13]		Alexandre Belloni, Victor Chernozhukov, and Christian Hansen. ‘Inference on Treatment Effects After Selection Amongst High-Dimensional Controls’. In: Review of Eco-nomic Studies 81.2 (2014), pp. 608–650 (cited on page 124).
[14]	Alexandre Belloni, Victor Chernozhukov, and Kengo Kato. ‘Uniform post-selection inference for least absolute deviation regression and other Z-estimation problems’. In: Biometrika 102.1 (2015), pp. 77–94 (cited on page 124).
[15]	Alexandre Belloni, Victor Chernozhukov, Denis Chetverikov, and Ying Wei. ‘Uniformly valid post-regularization con-fidence regions for many functional parameters in z-estimation framework’. In: Annals of statistics 46.6B (2018),
p. 3643 (cited on page 124).
[16]		Ruben Dezeure, Peter Bühlmann, and Cun-Hui Zhang. ‘High-dimensional simultaneous inference with the boot-strap’. In: Test 26.4 (2017), pp. 685–719 (cited on page 124).
[17]	Alexandre Belloni, Victor Chernozhukov, Christian Hansen, and Damian Kozbur. ‘Inference in High-Dimensional Panel Models With an Application to Gun Control’. In: Journal of Business & Economic Statistics 34.4 (2016),
pp. 590–605 (cited on page 124).
[18]		Victor Chernozhukov, Wolfgang Karl Härdle, Chen Huang, and Weining Wang. ‘Lasso-driven inference in time and space’. In: Annals of Statistics 49.3 (2021), pp. 1702–1735 (cited on page 124).
[19]	Hannes Leeb and Benedikt M. Pötscher. ‘Model selection and inference: Facts and fiction’. In: Econometric Theory
21.1 (2005), pp. 21–59 (cited on page 125).
 


[20]		Yufei Yi and Matey Neykov. ‘A New Perspective on Debi-asing Linear Regressions’. In: arXiv preprint arXiv:2104.03464 (2021) (cited on page 125).
[21]		Philipp Bach, Victor Chernozhukov, and Martin Spindler. ‘Heterogeneity in the US gender wage gap’. In: Journal of the Royal Statistical Society Series A: Statistics in Society
187.1 (2024), pp. 209–230 (cited on page 125).
[22]	Victor Chernozhukov, Denis Chetverikov, Kengo Kato, and Yuta Koike. ‘Improved central limit theorem and bootstrap approximations in high dimensions’. In: Annals of Statistics 50.5 (2022), pp. 2562–2586 (cited on page 127).
[23]		Victor Chernozhukov, Denis Chetverikov, Kengo Kato, and Yuta Koike. ‘High-dimensional Data Bootstrap’. In: Annual Review of Statistics and Applications; arXiv preprint arXiv:2205.09691 (2023) (cited on pages 127, 128).
 

 
Causal Inference via Conditional Ignorability

"compare apples to apples: to compare things that are very similar."
– Merriam Webster Dictionary [1].










Here we discuss how average causal effects may be identified using regression when treatment is not randomly assigned but instead depends on observed covariates. We discuss the conditional or adjustment method, which relies on comparing the average difference between expected outcomes for treated and untreated units that are comparable (formally, identical) in terms of their characteristics 𝑋. If treatment is as good as randomly assigned conditional on 𝑋, then this approach recovers average causal or treatment effects. This key condition is commonly referred to as conditional ignorability, conditional exogeneity, or unconfoundedness.
 
5
5.1	Introduction	133
5.2	Potential Outcomes and Ig-norability	134
Identification by Condition-ing	135
Conditional Ignorability via Causal Diagrams	138
Connections to Linear Re-gression	139
5.3	Identification Using Propen-sity Scores	140
Stratified RCTs	142
Covariate   Balance Checks	142
Connections to Linear Re-gression	143
5.4	Conditioning on Propensity Scores★	143
5.5	Average Treatment Effect for Groups and on the Treated145
5.6	Exercises	146
5.A	Rosenbaum-Rubin’s Re-sult		147
5.B	Clever Covariate Regres-sion	148
5.C	Details of ATET	148
 
5.1	Introduction
In a cross-country analysis, higher chocolate consumption pre-dicts a higher number of Nobel laureates per capita.


 
























Figure 5.1: Source: Franz H. Messerli, "Chocolate Consumption, Cognitive Function, and Nobel Lau-reates," New England Journal of Medicine. 2012
 

 
Is this a reflection of a true causal effect and therefore an actionable insight? If it were, countries could generate more Nobel laureates per capita by making chocolate abundant to everyone. (This wouldn’t be a bad thing.) Is this perhaps what Switzerland did? Switzerland has the highest number of Nobel laureates per capita.
Or is there a common cause1 that creates non-causal association? Perhaps wealthy countries invest more in science and higher wealth causes people to consume luxury goods like chocolate, as displayed in Figure 5.2. See for instance plots (D) and (E) in Figure 5.3. Comparative analysis, where we compare nations with identical or similar wealth, would probably reveal that the correlation is not causal.2 Probably we should be comparing Switzerland to similar countries in terms of wealth – the "apples-to-apples" comparison, so to speak. This type of analysis is very
 







1: We often refer to these common causes as "omitted variables" that give rise to "omitted variable bias."
2: It remains a fundamental empir-ical problem to confirm this con-jecture or disprove this conjecture. The causal channel through which chocolate (and other flavonoids) may affect Nobel production is by documented improvement in the cognitive function.
 

 
Chocolate	?
 
Country’s Wealth
 
Nobel
 



Figure 5.2: A Contrived Causal Path Diagram for the Effect of Coun-try’s Wealth on Chocolate Con-sumption and Nobel Prize Produc-tion per capita.
 


common in causal inference and is implemented via a set of tools introduced in this chapter.






 







In what follows, we work within Rubin’s [2] potential outcomes framework, as introduced in Chapter ??. The idea is that if we can think of observed treatment 𝐷 as generated randomly – independently of potential outcomes – conditional on some pre-treatment variables 𝑋, then we can learn the average causal (treatment) effects by regression
of 𝑌 on 𝐷 and 𝑋,
or, as is often said, by "adjusting" or "controlling" for 𝑋.

Notation
Recall that we denote the independence of two random variables (these can include random vectors) 𝑈 and 𝑉 as
𝑈 ⊥ 𝑉.

Independence, conditional on a third variable 𝑋, is denoted by
 
Figure 5.3: Source: J Nutr, Vol-ume 143, Issue 6, June 2013, Pages 931–933, "Does Chocolate Consumption Really Boost Nobel Award Chances? The Peril of Over-Interpreting Correlations in Health Studies," ©2013 American Society for Nutrition
 
𝑈 ⊥ 𝑉 | 𝑋.

5.2	Potential Outcomes and Ignorability
Recall that we use 𝑌 𝑑 to denote potential outcome in the treatment state 𝑑, where we consider only the case 𝑑	0, 1 for simplicity. We also recall our example of smoking from the previous chapter. Suppose we want to study the impact of
 


smoking marĳuana on life longevity. Suppose that smoking marĳuana has no causal/treatment effect on life longevity:
𝑌 = 𝑌(0) = 𝑌(1), so that 𝛿 = E[𝑌(1)] − E[𝑌(0)] = 0.
However, the observed smoking behavior, 𝐷, results not from an experimental study, but from observational data in which an individual’s smoking decisions are driven by other behavioral choices 𝑋 (drinking alcohol for example) which cause shorter life longevity. In this case, the predictive effect recovered by regression without adjusting for 𝑋 does not match the average causal effect

 
E[𝑌 | 𝐷 = 1] − E[𝑌 | 𝐷 = 0] < 0 = 𝛿,
because higher 𝐷 predicts higher 𝑋, which predicts lower 𝑌. This difference between the predictive effect and average causal effect is the result of confounding or selection bias.
In this example, conditioning on 𝑋 can remove the selection bias (see Figure 5.4)
E[E[𝑌 | 𝐷 = 1, 𝑋] − E[𝑌 | 𝐷 = 0, 𝑋]] = 𝛿,
provided that conditional on 𝑋 variation in 𝐷 is independent of the potential health outcomes.
The following provides a formal assumption under which we can eliminate the confounding bias by controlling for 𝑋.3


Identification by Conditioning
The ignorability assumption4 says that variation in treatment assignment 𝐷 is as good as random conditional on 𝑋. This assumption means that if we look at units with the same value
of the covariates, e.g. units with 𝑋 = 𝑥, then treatment variation among these observationally identical units, 𝐷 𝑋 = 𝑥, is indeed produced as if by a formal randomized control trial.
 
















3: The assumption is fundamen-tally untestable and is an assump-tion in the purest sense. Given assumed domain knowledge en-coded in causal DAGs, we study a systematic way of finding 𝑋 that satisfy this assumption in subse-quent chapters.







4: You may wonder why the term "ignorability" is used. The distri-bution of 𝑌 𝑑 depends only on 𝑋 and not on 𝐷, so the latter is "ignor-able." Note that the conventional name used in econometrics for the ignorability assumption is the con-ditional exogeneity or conditional in-dependence assumption.
 


 


 

 

Therefore, we can learn about the causal effect of 𝐷 by com-paring outcomes across treated and control units who have identical characteristics 𝑋 = 𝑥 under the conditional ignora-
bility assumption. The idea of comparing observations who
have identical characteristics is the essence of the so-called conditioning or adjustment strategy to learning causal effects. As conditioning approaches produce a different contrast for every potential value of 𝑋, we may also wish to average the contrasts at different values of 𝑋 over the distribution of characteristics to produce a summary measure of the causal effects.
The conditional probability of receiving treatment, the propensity score, plays an important role in this approach.
 
Figure 5.4: Pictorial representation of how selection on 𝑋 can lead to biased observed outcomes be-tween treated and control popula-tions, while conditioning on 𝑋 re-moves the selection bias. In this ex-ample, the potential outcomes 𝑌 0 and 𝑌 1 have identical distribu-tions shown in the far left and right of the figure. We also have a binary covariate 𝑋 that is related to treat-ment probability in the sense that P 𝐷 = 1 𝑋 = 1 > P 𝐷 = 1 𝑋 = 0
and P 𝐷 = 0 𝑋 = 1  < P 𝐷 =
0 𝑋 = 0 which leads to selection bias when we do not condition on
𝑋. This bias is illustrated by the difference in the distribution of (ob-served) 𝑌 given 𝐷 = 0 and 𝐷 = 1
shown in the black curves in the
middle of the figure. The bottom panel then shows that selection bias is removed by conditioning on 𝑋 as the distribution of potential out-comes given 𝑋 (blue and orange curves under 𝑌 0 𝑋 and 𝑌 1 𝑋) equals the distribution of observed outcomes given 𝐷 and 𝑋 (blue and orange curves under 𝑌|𝐷 = 0, 𝑋
and 𝑌|𝐷 = 1, 𝑋).
 

 
 


The overlap assumption requires that there is proper random-ization or variation in 𝐷 at each value 𝑥 in the support of 𝑋. Without this condition, there are values 𝑥 in the support of 𝑋 where we cannot construct a contrast between treatment and control units. We cannot learn the conditional average treatment effect at these values of 𝑋 and thus are also unable to learn the unconditional average effect of the treatment.

The following is the most important theoretical result that states that we can recover expectations of potential outcomes from regressions.

To prove Theorem 5.2.1, note that the overlap assumption makes it possible to condition on the events 𝐷 = 0, 𝑋 and 𝐷 = 1, 𝑋 at any value in the support of 𝑋 and that the second equality
holds by ignorability.

Hence, the Conditional Average Predictive Effect (CAPE),
𝜋(𝑋) = E[𝑌 | 𝐷 = 1, 𝑋] − E[𝑌 | 𝐷 = 0, 𝑋],
is equal to the Conditional Average Treatment Effect (CATE),
𝛿(𝑋) = E[𝑌(1) | 𝑋] − E[𝑌(0) | 𝑋].
Thus, the APE and ATE also agree:
𝛿 = E[𝛿(𝑋)] = E[𝜋(𝑋)] = 𝜋.
 


 
Conditional Ignorability via Causal Diagrams
It is possible to illustrate the key ignorability assumption, As-sumption 5.2.1, graphically as follows:5
𝐷	𝑑   𝑌(𝑑)
 



5: Note that what we present is just one of many causal diagrams that are compatible with the conditional ignorability condition. There are others, as will become apparent in subsequent chapters.
 
Figure 5.5: A Causal Diagram for the Conditional Ignorability Re-search Design

In this graph, we show the potential outcome 𝑌 𝑑 as a node and the potential treatment status 𝑑 as another node. The latter node is deterministic. There is an arrow from 𝑑 to 𝑌 𝑑 indicating the dependency. The pre-treatment covariates 𝑋 affect both the realized treatment variable 𝐷 and the potential outcomes 𝑌 𝑑 , as shown by the arrow from 𝑋 to 𝐷 and from 𝑋 to 𝑌 𝑑 . The assigned treatment variable 𝐷 is independent of the node 𝑌 𝑑 , conditional on 𝑋. Independence can be derived from the graph by observing the absence of any path between the 𝐷 and 𝑌 𝑑 nodes other than the path through the variable 𝑋 upon which we’ve conditioned. Note that Assumption 5.2.2, the overlap condition, is not illustrated in the graph.
The potential outcome process 𝑑  𝑌 𝑑 and treatment assign-ment jointly determine the realized outcome variable 𝑌 via the assignment 𝑌 := 𝑌 𝐷 . This generates the following causal
diagram. This graph says that 𝑋 is generated first. 𝐷 is then


𝐷



𝑋	Figure 5.6: A Causal Diagram with
Conditional Ignorability
generated, with the distribution of 𝐷 depending on 𝑋. Finally,
𝑌 is generated, with its distribution depending on both 𝐷 and
𝑋. Here, after conditioning on 𝑋, the statistical dependence (association) between 𝐷 and 𝑌 only reflects the causal channel,
𝐷 → 𝑌 allowing us to uncover the ATE, for example.
 


Connections to Linear Regression
The tools from Chapter 1 and Chapter 4 can be used to perform statistical inference on ATEs. We briefly discuss how (high-dimensional) regression can be used to retrieve causal estimates when conditional ignorability holds in this section.
The simplest instance of the problem is when the conditional expectation function of 𝑌 given 𝐷 and 𝑋 is linear,
E[𝑌 | 𝐷, 𝑋] = 𝛼𝐷 + 𝛽′𝑊 ,
which gives a model
𝑌 = 𝛼𝐷 + 𝛽′𝑊 + 𝜖,	E[𝜖 | 𝐷, 𝑋] = 0.
Here it is understood that 𝑊 may include 𝑋 as well as pre-specified nonlinear transformations of 𝑋.
In this model, 𝛼 identifies 𝛿

 
𝛿 = 𝛼
under the linearity assumption and ignorability, and our in-ference tools for 𝛼 automatically carry over to 𝛿. Note that the linearity assumption and ignorability assumptions imply that
treatment effects are homogeneous; that is, 𝛿 𝑥 = 𝛿 for all 𝑥 in the support of 𝑋.
Of course, the assumption of linearity and homogeneous treat-ment effects is restrictive. A simple way to relax this is to consider interactions. One version of this approach takes all interactions between 𝑊 and 𝐷 and assumes
E[𝑌 | 𝐷, 𝑋] = 𝛼1𝐷 + 𝛼′2𝑊𝐷 + 𝛽1 + 𝛽′2𝑊 ,
where we also maintain that we are working with centered covariates: E𝑊 = 0.6
We then recover the ATE as
𝛿 = 𝛼1
 


















6: This model is still linear and re-sults for linear models carry over to this case as well.
 
and CATE as
𝛿(𝑋) = 𝛼1 + 𝛼′2𝑊.

 



 

Note, we used this approach in the heterogeneous wage gap example in Chapter 1. The discussion of whether the wage gap analysis has a causal interpretation is given in the next causal inference chapter, Chapter 6.
As demonstrated in Theorem 5.2.1, the ultimate targets are the conditional expectation functions E 𝑌 𝑑 𝑋 if our goal is to learn average causal effects under ignorability. This being our target makes the relevance of considering transformations
𝑊 = 𝑇 𝑋 of 𝑋 important as we would like to have the linear model provide a good approximation to these conditional expectation functions. See the discussion in "From Best Linear
Predictor to Best Predictor" in Chapter 1. If the linear model is misspecified in the sense that it does not approximate the conditional expectation functions well, the estimated causal effects - e.g. 𝛼1 in the interactive model - do not necessarily have any causal interpretation. This potential failure is a major reason we consider more flexible, modern machine learning methods.


5.3	Identification Using Propensity Scores
The identification by conditioning approach requires being able to accurately model the "outcome process," i.e. the conditional expectation function E 𝑌 𝐷, 𝑋 . This conditional expectation function might correspond to a complicated real world process that is hard to model or approximate.
When the outcome process is hard to model, we might have a much better handle on the "treatment selection process," i.e. the propensity score:
𝑝(𝑋) = P(𝐷 = 1 | 𝑋).

An alternative approach, known as the Horvitz-Thompson method [3], uses propensity score reweighting to recover aver-
 


 
ages of potential outcomes. Using the propensity score rather than identification by conditioning on 𝑋 is a useful empirical strategy when 𝑋 is high-dimensional and 𝑝 𝑋 is available or can be approximated accurately.7 An example of a setting where the propensity score is known is a stratified RCT, which is an experiment where treatment is assigned at random with probability 𝑝 𝑋 to individuals with different observed covari-ates 𝑋. In this case, the treatment assignment probability 𝑝 𝑋 is exactly the propensity score.
 




7: An interesting example where the propensity score is not known but can be well-approximated is the examination in [4] of the causal effect of attendance at a particular school or group of schools relative to one or more alternative schools (e.g., "elite" vs. "non-elite" schools) in settings where matching algo-rithms are used to assign students to schools. In this example, we can think of these student assignment mechanisms as 𝑝(𝑋).
 










To prove this result, note

 
P(𝐷 = 𝑑|𝑋)
 
P(𝐷 = 𝑑|𝑋)
= E[𝑌(𝑑) | 𝑋] E[1(𝐷 = 𝑑) | 𝑋]
 
= E[𝑌(𝑑) | 𝑋],
where we used conditional ignorability in the second equality.

As a consequence, we can identify average treatment effects by simple averaging of transformed outcomes:
 1(𝐷 = 1) 	 1(𝐷 = 0) 
𝛿 = E[𝑌𝐻],	𝐻 = P(𝐷 = 1|𝑋) − P(𝐷 = 0|𝑋) ,
where 𝐻 is called the Horvitz-Thompson transform. Simi-larly, we can identify conditional average treatment effects as a conditional average of transformed outcomes:
𝛿(𝑋) = E[𝑌𝐻 | 𝑋].
 


Note that propensity score reweighting reduces to the differ-ence of means in the control and treatment groups when the propensity score is constant.

Stratified RCTs
In the case where the propensity score 𝑝 𝑋 is known, we are essentially back to a classical RCT.



Covariate Balance Checks
Given a propensity score 𝑝 𝑋 , we can check if the RCT is valid (randomization is successful) by performing a covariate balance check. Specifically, conditional ignorability implies that
E[𝐻 | 𝑋] = 0.
Thus, if covariates predict 𝐻, we can conclude that conditional ignorability does not hold. Heuristically, covariates predicting
𝐻 means that covariates are imbalanced in the sense that, af-ter reweighting by 𝑋 dependent treatment probability, there are systematic differences in 𝑋 across treatment and control observations which can be exploited to predict treatment as-signment.
 


In a low-dimensional linear model framework, a covariate balance check can be done by regressing 𝐻 on 𝑊, a dictionary of transformations of 𝑋, and testing if 𝑊 predicts 𝐻. 𝑊 predicting
𝐻 suggests that the RCTs randomization protocol did not go as planned.

Connections to Linear Regression
Note that by the Horvitz-Thompson transform characterization of the CATE, 𝛿 𝑋 = E 𝑌𝐻 𝑋 , we can view the conditional average treatment effect as the solution to a prediction problem
of predicting the transformed outcome 𝑌𝐻 from the regressors
𝑋.
A useful strategy is to consider (potentially high-dimensional) linear regression models where 𝐻𝑌 is the dependent variable; see, e.g., [5]. Note that if we assume that E[𝑌 | 𝐷, 𝑋] = 𝛼1𝐷 +
𝛼′2𝑊𝐷  𝛽1  𝛽′2𝑊, where 𝑊 is a dictionary of transformations
of 𝑋, then we have
E[𝑌𝐻 | 𝑋] = 𝛼1 + 𝛼′2𝑊.
Thus, we can simply run a regression of 𝑌𝐻 on 1, 𝑊′ ′. In this regression model, we recover the ATE as
𝛿 = 𝛼1
and CATE as
𝛿(𝑋) = 𝛼1 + 𝛼′2𝑊.
We can use partialling out methods, such as Double Lasso, to perform inference on 𝛼1 and components of 𝛼2. We also discuss estimating CATE using more general machine learning methods in Chapter 14 and Chapter 15.

5.4	Conditioning on Propensity Scores★
The fact that conditioning on the right set of controls removes se-lection bias has long been recognized by researchers employing regression methods. Rosenbaum and Rubin [6] made the much more subtle point that conditioning on only the propensity score
𝑝(𝑋) = P(𝐷 = 1 | 𝑋)
also suffices to remove the selection bias.
 


 
In other words, conditional on 𝑝 𝑋 = 𝑝, variation in 𝐷 is as good as randomly assigned. Hence, whenever it suffices to use
𝑋 for identification by conditioning, it also suffices to use 𝑝 𝑋 . This fact makes 𝑝 𝑋 a "minimal sufficient" statistic, condition-ing on which removes selection bias under ignorability.
In scenarios with a known propensity score, we can simply use 𝑝 𝑋 as a control in place of the high-dimensional set of characteristics, 𝑋, and thus bypass a potentially complicated high-dimensional estimation problem. In other words, we can identify the conditional average potential outcome as
E[𝑌(𝑑) | 𝑝(𝑋)] = E[𝑌 | 𝐷 = 𝑑, 𝑝(𝑋)].
Thus, it suffices to learn the CEF E 𝑌 𝐷, 𝑝 𝑋 . We learn good approximations of these CEFs by incorporating polynomials or other transformations of 𝑝 𝑋 to make things more flexible and running linear regression methods. Finally, we can also employ nonlinear machine learning methods introduced in Chapter 8 to overcome the limitations of linear models.
After controlling for 𝑝 𝑋 , we can also consider the use of high-dimensional methods to include other transformations 𝑊 of the raw variables 𝑋 in order to improve precision, estimating the more flexible CEF E 𝑌 𝐷, 𝑝 𝑋 , 𝑊 . It is especially advisable to include transformations 𝑊 that fail the covariate balance checks discussed in Section 5.3. Including 𝑊 can reduce the selection bias (and, hopefully, set it equal to zero). In the reemployment experiment, for example, we observed that balance did not seem satisfied across age groups. Hence, further controlling for age makes sense and results in modest changes to estimates of the treatment effect. Of course, there is no guarantee that controlling for observed covariates can overcome selection bias in compromised RCTs in general because unobserved covariates may be driving the bias.

 


 

5.5	Average Treatment Effect for Groups and on the Treated
In addition to unconditional average treatment effects (ATE) or average treatment effects at specific values of the covariates
𝑋 = 𝑥, we may be interested in average effects within specific subpopulations.
A leading example of an interesting subpopulation treatment effect is a group ATE (GATE):
𝛿𝐺 = E[𝑌(1) − 𝑌(0)|𝐺 = 1]
where 𝐺 is a group indicator defined in terms of 𝑋’s. For example, we might be interested in the effects of a training program among younger people, say between 18 and 30 years old (𝐺 = 1 18  age  30 ); among people older than 30 years
old (so 𝐺 = 1 30 < age ); and differences between these two
groups.
We can immediately obtain the GATE using the identification results above and the law of iterated expectations:
E[𝑌(1) − 𝑌(0)|𝐺 = 1]
= E[E[𝑌|𝐷 = 1, 𝑋] − E[𝑌|𝐷 = 0, 𝑋]|𝐺 = 1]
= E[𝐻𝑌|𝐺 = 1].
That is, we can identify GATEs either by taking the difference in regression functions or applying propensity score reweighting of outcomes and then averaging over group 𝐺.
 


 
We next consider treatment effects for the subpopulation of treated units, the average treatment effect on the treated (ATET):8
𝛿1 = E[𝑌(1) − 𝑌(0) | 𝐷 = 1].
For example, consider training completion as a treatment, 𝐷, and 𝑋 a vector of pre-treatment variables such that unconfound-edness holds. Consider the question:
▶ On average, how much more do trainees earn after going through the training program than they would have earned had they not gone through the program?
Note that this question is a counterfactual question as it requires us to compare outcomes for trainees in the treated state, where they receive training, and the unobserved control state, where they did not receive training. The ATET, 𝛿1, is the parameter that answers such questions about counterfactuals. The ATET is identified by
E[E[𝑌|𝐷 = 1, 𝑋] − E[𝑌|𝐷 = 0, 𝑋] | 𝐷 = 1]
similarly to what we had above. It is also possible to bypass the use of E 𝑌 𝐷 = 1, 𝑋 in this case; see Appendix 5.C for more details.

5.6	Exercises
Exercise 5.6.1 (Conditioning) Use one or two paragraphs to explain conditioning and its use in learning treatment effects/causal effects in observational data and randomized trials where treatment probability depends on pre-treatment variables. This discussion should be non-technical as if you were writing an explanation for a smart friend with relatively little exposure to causal modeling.

Exercise 5.6.2 (Propensity Score Reweighting) Use one or two paragraphs to explain the propensity score reweighting approach for identification of average treatment effects. This discussion should be non-technical as if you were writing an explanation for a smart friend with relatively little exposure to causal modeling.

Exercise 5.6.3 (GATE and ATET) Use one or two paragraphs to explain why group ATE and the ATE on the treated may be
 


8: Rather than ATET, some use the abbreviation AToT or ATT.
 


of interest in empirical work. This discussion should be non-technical as if you were writing an explanation for a smart friend with relatively little exposure to causal modeling.

5.A	Rosenbaum-Rubin’s Result
Recall the propensity score is

𝑝(𝑋) := P(𝐷 = 1|𝑋),
which is the probability of receiving treatment given 𝑋. A simple useful intermediate property is the balancing property of the propensity score which states that treatment is independent of
𝑋 conditional on the propensity score:
𝐷 ⊥ 𝑋 | 𝑝(𝑋)  ⇔  P(𝐷 = 1|𝑋, 𝑝(𝑋)) = P(𝐷 = 1|𝑝(𝑋)). This result follows simply from (i) P 𝐷 = 1 𝑋, 𝑝 𝑋  = P 𝐷 =
1 𝑋  = 𝑝 𝑋  and (ii) P 𝐷 = 1 𝑝 𝑋  = E 𝐷 = 1 𝑝 𝑋  =
E E 𝐷 𝑋, 𝑝 𝑋 𝑝 𝑋 = E 𝑝 𝑋 𝑝 𝑋 = 𝑝 𝑋 . This property underlies covariate balance checks.
We now turn to the theorem of Rosenbaum and Rubin. By Theorem 5.3.1 and the law of iterated expectations, we have that for any function of the form 𝑔(𝑦) = 1(𝑦 ≤ 𝑡), 𝑡 ∈ ℝ:
E [𝑔(𝑌(1)) | 𝑝(𝑋)] = E[E[𝑔(𝑌(1))|𝑋, 𝑝(𝑋)]|𝑝(𝑋)]
= E[E[𝑔(𝑌(1))|𝑋]|𝑝(𝑋)]
= E J 𝑔(𝑌) 1(𝐷 = 1) | 𝑝(𝑋)l
= E J 𝑔(𝑌) 1(𝐷 = 1) | 𝐷 = 1, 𝑝(𝑋)l 𝑃(𝐷 = 1|𝑝(𝑋))
+ E J 𝑔(𝑌) 1(𝐷 = 1) | 𝐷 = 0, 𝑝(𝑋)l 𝑃(𝐷 = 0|𝑝(𝑋))
1 
= E[𝑔(𝑌) | 𝐷 = 1, 𝑝(𝑋)]
= E[𝑔(𝑌) | 𝐷 = 1, 𝑝(𝑋)]
= E[𝑔(𝑌(1)) | 𝐷 = 1, 𝑝(𝑋)]
where we use 𝑃 𝐷 = 1	𝑝 𝑋	= 𝑝 𝑋 . We can similarly argue for the case of 𝑑 = 0. Thus, the conditional distribution of
𝑌 1 does not depend on 𝐷, once we condition on 𝑝 𝑋 , which
verifies Theorem 5.4.1.
 

5.B	Clever Covariate Regression
Here we show that if we care only about estimating the ATE, then it suffices to learn the BLP of the outcome 𝑌 using the single covariate
𝜙(𝐷, 𝑋) := 𝐻 = 1(𝐷 = 1) − 1(𝐷 = 0) .
𝑝(𝑋)	1 − 𝑝(𝑋)
 
We can then use this BLP model as a proxy for the CEF E 𝑌  𝐷, 𝑝 𝑋  . Specifically, we learn a decomposition 𝑌 =
𝛽𝜙 𝐷, 𝑋   𝜖, 𝜖  𝜙 𝐷, 𝑋  by running OLS of 𝑌 on 𝜙 𝐷, 𝑋
and then use E 𝛽 𝜙 1, 𝑋 𝜙 0, 𝑋 as the ATE. This approach, referred to in the literature as the "clever covariate" approach, was first proposed in [7] and further developed in [8].
Note that the random variable 𝐻 satisfies
E[ 𝑓 (𝐷, 𝑋)𝐻 | 𝑋] = 𝑓 (1, 𝑋) − 𝑓 (0, 𝑋)
for any function 𝑓 𝐷, 𝑋 .9 Then, by Theorem 5.3.1 and orthog-onality of 𝜖 in the BLP decomposition:
E[𝑌(1) − 𝑌(0)] = E[𝑌𝐻] = E[𝛽𝜙(𝐷, 𝑋)𝐻]
= E 𝛽(𝜙(1, 𝑋) − 𝜙(0, 𝑋)) .
Note that even though this approach allows us to identify the ATE, it does uncover the CATE E 𝑌 1  𝑌 0  𝑋 . The reason for the failure in learning the CATE is that the residual 𝜖 does not necessarily satisfy conditional orthogonality; i.e. we do not have E[(𝑌 − 𝛽𝜙(𝐷, 𝑋))𝐻 | 𝑋] = 0.

5.C	Details of ATET
In observational studies, the ATET is identified under weaker conditions than the ATE because
E[𝑌(1) | 𝐷 = 1] = E[𝑌 | 𝐷 = 1],
so we only need to identify E 𝑌 0 𝐷 = 1 . We can state the weaker version of the ignorability and overlap conditions as follows:
 













9: Verify this as a reading exercise.
 

 
 


 

Theorem 5.C.1 follows because, by iterated expectations and ignorability,
E[𝑌(0) | 𝐷 = 1] = E[E[𝑌(0) | 𝐷 = 1, 𝑋] | 𝐷 = 1]
= E[E[𝑌(0) | 𝐷 = 0, 𝑋] | 𝐷 = 1]
= E[E[𝑌 | 𝐷 = 0, 𝑋] | 𝐷 = 1],
where the outer expectation is well-defined because the support of 𝑋 conditional on 𝐷 = 1 is a subset of the support of 𝑋 conditional on 𝐷 = 0 by the overlap condition.
The Horvitz-Thompson method can be also used to recover averages of potential outcomes for the treated. Indeed,
E[𝐷𝑌] = E[𝐷𝑌(1)] = E[𝑌(1) | 𝐷 = 1]
E[𝐷]	E[𝐷]
 


and
E r 1(1−𝐷) 𝑝(𝑋)𝑌l	E r 1 𝑝(𝑋) E[(1 − 𝐷)𝑌 | 𝑋]l
 
E[𝐷]
 
E r 1 𝑝(𝑋)
 
E[𝐷]
E[(1 − 𝐷)𝑌(0) | 𝑋]
 
 
= 	−𝑝(𝑋)	
E[𝐷]
E r 1 𝑝(𝑋) E[1 − 𝐷|𝑋]E[𝑌(0) | 𝑋]l
E	E[𝐷]
=  [𝑝(𝑋)E[𝑌(0) | 𝑋]]
E	E[𝐷]
=  [E[𝐷 | 𝑋]E[𝑌(0) | 𝑋]]
E	E[𝐷]
=  [E[𝐷𝑌(0) | 𝑋]]
E	E[𝐷]
=  [𝐷𝑌(0)] = E[𝑌(0) | 𝐷 = 1]

where in the second to last step we used that 𝐷 𝑌 0 𝑋, implies E 𝐷𝑌 0  𝑋 = E 𝐷 𝑋 E 𝑌 0  𝑋 . Hence, we obtain the following result:

 


Bibliography



[1]	Merriam Webster Dictionary. Compare apples and/to/with apples. url: https : / / www . merriam - webster . com / dictionary/compare\%20apples\%20and\%2Fto\%2Fwith\
%20apples (cited on page 132).
[2]	Donald B. Rubin. ‘Estimating causal effects of treatments in randomized and nonrandomized studies.’ In: Journal of Educational Psychology 66.5 (1974), pp. 688–701 (cited
on page 134).
[3]	Daniel G. Horvitz and Donovan J. Thompson. ‘A gener-alization of sampling without replacement from a finite universe’. In: Journal of the American Statistical Association
47.260 (1952), pp. 663–685 (cited on page 140).
[4]		Atila Abdulkadiroğlu, Joshua D. Angrist, Yusuke Narita, and Parag A. Pathak. ‘Research Design Meets Market Design: Using Centralized Assignment for Impact Eval-uation’. In: Econometrica 85.5 (2017), pp. 1373–1432. doi: 10.3982/ECTA13925 (cited on page 141).
[5]		Victor Chernozhukov, Mert Demirer, Esther Duflo, and Iván Fernández-Val. Generic machine learning inference on heterogeneous treatment effects in randomized experiments, with an application to immunization in India. Tech. rep. National Bureau of Economic Research, 2018 (cited on page 143).
[6]	Paul R. Rosenbaum and Donald B. Rubin. ‘The Central Role of the Propensity Score in Observational Studies for Causal Effects’. In: Biometrika 70.1 (1983), pp. 41–55 (cited
on page 143).
[7]		Daniel O. Scharfstein, Andrea Rotnitzky, and James M. Robins. ‘Adjusting for Nonignorable Drop-Out Using Semiparametric Nonresponse Models’. In: Journal of the American Statistical Association 94.448 (1999), pp. 1096–
1120. (Visited on 01/25/2023) (cited on pages 145, 148).
[8]		Heejung Bang and James M. Robins. ‘Doubly Robust Estimation in Missing Data and Causal Inference Models’. In: Biometrics 61.4 (2005), pp. 962–973. doi: https://doi. org/10.1111/j.1541- 0420.2005.00377.x (cited on pages 145, 148).
 

 

Causal Inference via Linear Structural
Equations


"the scientific [. . . ] problem of causality is essen-tially a problem regarding our way of thinking, not a problem regarding the nature of the exterior world."
– Ragnar Frisch [1].










Here we present the linear structural equation model framework and causal diagrams. The advantage of these models is they are closely related to underlying structural models commonly used in economics and other fields. They allow for transparent derivation of the conditional ignorability assumption from the structure of the model. While linearity is imposed in this chapter, it will be dispensed with in later chapters.
 
6
6.1	Structural Equation Mod-elling and Conditional Exo-geneity	153
A Simple Triangular Structural Equation Model (TSEM)	153
6.2	Drawing the Model: Causal Diagrams, aka DAGs    156
6.3	When Conditioning Can Go Wrong: Collider Bias, aka Heck-man Selection Bias	159
6.4	Wage Gap Analysis and Dis-crimination	162
6.5	Notes	166
6.6	Notebooks	166
6.7	Exercise	166
6.A Details of the Wage Discrim-ination Analysis	168
 
6.1	Structural Equation Modelling and Conditional Exogeneity
Basic ideas that appeared in econometrics between the 20s and 40s (P. Wright [2], S. Wright [3], J. Tinbergen [4], T. Haavelmo [5]) provide another take on and language for causality that is closely related to the potential outcomes framework.
 

A Simple Triangular Structural Equation Model (TSEM)
We illustrate the basic ideas using a simple model of a house-hold’s (say weekly) demand for gasoline, motivated by Haus-man and Newey [6].
We start with a log-linear (Cobb-Douglas [7]) model for log-demand 𝑦 given the log-price 𝑝
𝑦(𝑝) := 𝛿𝑝,
where 𝛿 is the elasticity of demand. Demand is random across households, and we may model this randomness as
𝑌(𝑝) := 𝛿𝑝 + 𝑈,	E[𝑈] = 0,	(6.1.1)
where 𝑈 is a stochastic shock that describes variation of demand across households (or across time, but assume that we are just looking at a particular time point). We immediately recognize that 𝑌 𝑝 plays the same role as a potential outcome in Rubin’s potential outcome model.1
The stochastic function
𝑝 ↦→ 𝑌(𝑝)
describes a household’s log-demand at a given log-price 𝑝. The expected log-demand at log-price 𝑝 is given by E 𝑌 𝑝 = 𝛿𝑝. The function encodes various structural causal effects: If we
change 𝑝 from 𝑝0 to 𝑝1, the expected demand change would be
 


























1: The subtle difference here is that
𝑈 does not depend on the index 𝑝, though we could make 𝑈 be in-dexed by 𝑝 at the cost of more com-plicated exposition. The distinction drawn is not superficial. Later on, when we discuss models with in-struments, the dependence of 𝑈 on
𝑝 can create non-trivial problems which are not present in this sec-tion.
 
E[𝑌(𝑝1)] − E[𝑌(𝑝0)] = 𝛿(𝑝1 − 𝑝0).
Model (6.1.1) is very simple, and we may want to introduce covariates to capture other observable factors that may be asso-ciated with demand. That is, we may think there are observable parts of the stochastic shock, characterized by 𝑋, which help us predict household demand. Leading examples are household
 


characteristics. For example, we may think demand is associated with features such as family size, income, number of cars, or geographical location. We can incorporate these features by modelling 𝑈 = 𝑋′𝛽  𝜖𝑌, where 𝜖𝑌 is independent of 𝑋 and
has mean zero. Employing this model structure, we can write
our augmented model as
𝑌(𝑝) := 𝛿𝑝 + 𝑋′𝛽 + 𝜖𝑌 ,	𝜖𝑌 ⊥ 𝑋.	(6.1.2)

Equation (6.1.2) is a structural stochastic model of economic outcomes. This model has nothing to do with regression or a statistical predictive model. Rather, it is a model that provides counterfactual predictions: If log-price is set to 𝑝, then a household with characteristics 𝑋 can be predicted to purchase
𝛿𝑝 + 𝑋′𝛽
log-units. Here 𝑝 is not a random variable – it is an index describing potential values of the price.

Then we ask the question:
▶ What data 𝑌, 𝑃, 𝑋 on quantities, prices, and characteris-tics should we collect to allow us to estimate the structural parameter 𝛿?











 

Assumption 6.1.1 is the econometric analog of ignorability.2 In the context of household demand, this condition requires that
𝑃 is determined independently of a household’s demand shock
𝜖𝑌, conditional on characteristics 𝑋. This assumption seems plausible for household level decisions, especially if we include geography in the set of covariates 𝑋.
 
2: At a general level, gasoline prices are determined by aggregate supply and demand conditions, with small local geographic adjust-ments (e.g., gasoline prices in areas with higher prices of land may be higher than in other areas to reflect the higher land costs for gasoline stations). Conditional on being in a given small geographic region, we may think of price fluctuations as independent of household-specific demand shocks.
 



If the conditional exogeneity condition holds, then
𝑌 = 𝑌(𝑃) = 𝛿𝑃 + 𝑋′𝛽 + 𝜖𝑌 ,	𝜖𝑌 ⊥ (𝑃, 𝑋).
This means that the projection parameters of 𝑌 on 𝑃 and 𝑋
coincide with the structural parameters 𝛿 and 𝛽.

 
We stress that our parameters 𝛿 and 𝛽 are not defined by regression; they are defined by the model. Under the condi-tional exogeneity condition, these parameters coincide with the projection parameters.3
We might further postulate a structural equation for log-prices:
𝑃(𝑥) := 𝑥′𝜈 + 𝜖𝑃 ,
where 𝑃 𝑥 is the stochastic price process indexed by a house-hold characteristics and 𝜖𝑃 describes the centered stochastic price shock. We assume that observed 𝑋 is independent of price shock 𝜖𝑃,
 




3: A weaker starting condition than the conditional exogeneity condition for the above result is simply
(𝑃, 𝑋) ⊥ 𝜖𝑌.
That is, the observed 𝑃 and 𝑋 are orthogonal to the structural error
𝜖𝑌.
 
𝑋 ⊥ 𝜖𝑃.
Independence between 𝜖𝑃 and observed 𝑋 implies that 𝜈 coin-cides with the projection coefficient of 𝑃 on 𝑋.
The price process 𝑃 𝑥 captures the belief that prices faced by households may differ depending on household characteristics. Note that this notation allows for only a subset of household characteristics to be systematically related to price; that is, we can have 𝑃 𝑥 = 𝑃 𝑥1 for some subvector 𝑥1 of 𝑥. For example,
it seems reasonable that households located in different regions
would experience different prices, in which case 𝑥1 could repre-sent a household’s geographic characteristics. Independence of the price shock 𝜖𝑃 from observed 𝑋 may be plausible if house-hold characteristics are determined well before gasoline prices faced by individual households in any specific time period are set.

Putting the equations together, we have a triangular struc-tural equation model (TSEM):
𝑌 := 𝛿𝑃 + 𝑋′𝛽 + 𝜖𝑌 ,
𝑃 := 𝑋′𝜈 + 𝜖𝑃 ,	(6.1.3)
𝑋,
where 𝜖𝑌, 𝜖𝑃, and 𝑋 are mutually independent (or at least uncorrelated) and determined outside of the model. They
 



are called exogenous variables. 𝑌 and 𝑃 are determined within the model and called the endogenous variables. The structural parameter 𝛿 can be identified by linear regression provided Var(𝜖𝑃) > 0, and the structural parameter 𝜈 can be identified by linear regression provided Var(𝑋) > 0.

 
Under the conditions stated above the parameters of these structural equations coincide with the projection parameters.


What do we mean by the model being structural? The term structural means that each of the equations is assumed to provide comparative statics and answers to counterfactual questions. Setting the right-hand-side variables to their potential values, we have
𝑌(𝑝, 𝑥) := 𝛿𝑝 + 𝑥′𝛽 + 𝜖𝑌 ,
𝑃(𝑥) := 𝑥′𝜈 + 𝜖𝑃.
The conceptual operation of "setting" or "fixing" the vari-ables is supposed to leave the structure invariant. More generally, the structural parameters are supposed to be invariant to changes in the distribution of exogenous vari-ables – 𝑋, 𝜖𝑌, 𝜖𝑃 – that have been generated outside of the model. Therefore, we can use these structural parameters to generate counterfactual predictions.

6.2	Drawing the Model: Causal Diagrams, aka DAGs
Sewall and Philip Wright [2], [3] would have depicted system of equations (6.1.3) graphically as a causal (path) diagram as in Figure 6.1. Observed variables are shown as nodes, causal paths are shown by directed arrows, and the structural (causal) pa-rameters are given by the symbols placed next to the arrows.
The graph represents a structural economic model that can answer causal (comparative statics) questions. For example, the elasticity parameter 𝛿 tells us how household demand will respond to a firm setting a new price. Note that a firm setting a new price will not alter household characteristics or the other exogenous features of the model, and thus only the parameter
𝛿 is relevant for answering this question within the model.
 




The jargon comparative statics refers to the determination of how en-dogenous variables change in re-sponse to changes in exogenous variables. Similarly, counterfactual questions coincide with asking how outcomes or endogenous variables change when variables are set to new values with other features of the model remaining fixed; e.g. ask-ing how demand changes when price is set to some new value by a firm with household character-istics, price shocks, and demand shocks unaffected.
 


𝑃   𝑌

𝜈
Figure 6.1: A simple causal diagram representation of the TSEM for the household gasoline demand exam-ple.

We could have expanded the previous graph to include unob-served shocks 𝜖𝑃 and 𝜖𝑌 as follows:
𝑃   𝑌

𝜈
Figure 6.2: An expanded causal di-agram representation of the TSEM that shows the unobserved shocks
𝜖𝑃 and 𝜖𝑌 as root nodes.

The graph initiates with the root nodes 𝜖𝑃, 𝑋, and 𝜖𝑌. The absence of links between the root nodes signifies the orthogo-nality between the nodes: namely, the absence of correlation. Understanding the orthogonality structure between nodes is an important input into identification of structural parameters via projection. The nodes 𝑋 and 𝜖𝑃 are parents of 𝑃; the nodes
𝑃, 𝑋, and 𝜖𝑌 are parents of 𝑌. The node 𝑌 is a collider on all
paths, because it contains only incoming arrows.
The main effect of interest is 𝛿, which we call the structural causal effect of 𝑃 on 𝑌. This effect is identified after adjusting for
𝑋. In terms of the graph above, there are two paths connecting
𝑃 and 𝑌:
𝑃 → 𝑌 and 𝑃 ← 𝑋 → 𝑌.
The second path is called a backdoor path because there is an arrow pointing back to 𝑃 from 𝑋. This connection indicates that there is a common cause for 𝑃 and 𝑌. Figuratively speaking, controlling or adjusting for 𝑋 is said to be like "closing the backdoor path," shutting down the non-causal sources of statistical dependence between 𝑌 and 𝑃.
This visual characterization of the adjustment for 𝑋 is due to
J. Pearl [8] and generalizes to much more complicated graphs. We revisit these ideas throughout subsequent chapters.
 



How do household characteristics impact our model? 𝑋
affects 𝑌 through two paths:
▶ the direct effect 𝛽 via 𝑋 → 𝑌,
▶ and the indirect effect 𝑣𝛿 via 𝑋 → 𝑃 → 𝑌.

 
The indirect effect is said to be "mediated" by 𝑃. We saw in Section 6.1 that we can identify 𝛿 and 𝛽 from projection of 𝑌 on 𝑃 and 𝑋, and we can identify 𝜈 by projection of 𝑃 on 𝑋. Therefore both the direct and indirect effects are identified.
The total effect of 𝑋 on 𝑌 is
 
Mediation structures appeared right at the outset in the Wrights’ work [2], [3].
 
𝜈𝛿 + 𝛽,
which can be identified in this case by projection of 𝑌 on 𝑋. To verify this, we plug the first equation from the TSEM in (6.1.3) into the second equation producing
𝑌 = (𝜈𝛿 + 𝛽)′𝑋 + 𝑉;	𝑉 = 𝜖𝑌 + 𝛿𝜖𝑃.
We see that the composite disturbance 𝑉 is orthogonal to 𝑋,
𝑉 ⊥ 𝑋,
and, therefore, 𝜈𝛿 𝛽 coincides with the projection coefficient in the projection of 𝑌 on 𝑋. The latter point can be seen graphi-cally: There are no "backdoor" paths from 𝑋 to 𝑌, so it is not necessary to adjust or control for anything to identify the total effect of 𝑋 on 𝑌.
In fact, while conditioning on 𝑃 would allow us to identify the direct effect of 𝑋, 𝛽, it would prevent us from retrieving the total effect 𝜈𝛿 𝛽. In empirical practice, we may think of conditioning on 𝑃 as "conditioning on the outcome," as 𝑃 is determined by its parents, including 𝑋, so may be thought of as an outcome relative to 𝑋.

Remark 6.2.1 (Statistical Identification) Statistical identifi-
cation typically relies on a combination of orthogonality or
conditional independence restrictions and additional condi-
tions – referred to as "rank conditions" in some settings – that
ensure there is variation available for learning parameters of
interest. For example, we need that Var(𝜖𝑃) > 0 if we wish
to learn 𝛿 in the TSEM in (6.1.3), and we need overlap for
learning ATE as discussed in Chapter 5. Graphical methods
provide a tool for representing orthogonality and conditional
 


independence relationships. They typically do not immedi-
ately reveal the additional rank-type conditions one would
use in establishing statistical point identification. Examining
the graphical structure does reveal what causal effects are
potentially learnable within the structure, and additional
restrictions, such as Var(𝜖𝑃) > 0 in the TSEM, can then be
deduced. Throughout the remainder of this book, we abstract
away from rank-type conditions when discussing graphi-
cal models and talk about identifying parameters from the
implied orthogonality or conditional independence structure.
To summarize, to learn a causal parameter, we must first define the causal parameter of interest and then carefully consider the choice of what to condition on to learn this effect. These choices
are particularly important given the existence of collider bias.	The Notebooks 6.6.1 provide a sim-
ple simulated example of collider
bias based on the SEM (6.3.1).
6.3	When Conditioning Can Go Wrong: Collider Bias, aka Heckman Selection Bias

 
Consider the following SEM:
𝑇 := 𝜖𝑇
𝐵 := 𝜖𝐵
𝐶 := 𝑇 + 𝐵 + 𝜖𝐶
 



(6.3.1)
 

𝐵	𝑇
   

 
where 𝜖𝑇 , 𝜖𝐵, and 𝜖𝐶 are independent 𝑁 0, 1 shocks. Here the average structural function for 𝑇, which does not depend on what values 𝐵 might take, is zero,
E[𝑇] = 0.
Regression without conditioning on 𝐶 correctly identifies that
𝑇 is not causally impacted by 𝐵:
E[𝑇 | 𝐵 = 𝑏] = 0.
However, further conditioning on 𝐶 removes the causal inter-pretation of the projection coefficient:4
E[𝑇 | 𝐵, 𝐶] = (𝐶 − 𝐵)/2; =⇒ E[E[𝑇 | 𝐵 = 𝑏, 𝐶]] = −𝑏/2 < 0.
This regression suggests that, controlling for 𝐶, the predictive effect of 𝐵 on 𝑇 is 1 2. This predictive effect is not a causal effect.
 
𝐶

Figure 6.3: DAG with a collider rep-resenting SEM (6.3.1).










4: Dividing by 2 may seem coun-terintuitive, but it is correct. See the Collider Bias Notebooks 6.6.1 for detail.
 


 
Collider bias illustrates that conditioning on outcomes may produce the wrong conclusions about causality, so conditioning on outcomes should be always approached with care. In econo-metrics, collider bias is known as a form of sample selection bias5 ("conditioning on endogenous variables" or Heckman selection bias [9]).

A Serious Digression on Colliders. Within our toy SEM frame-work, regression on a collider is clearly the wrong thing to do if one wants to identify the causal effect of 𝐵 on 𝑇. However, we do note that regression on a collider can be very useful for other predictive tasks.
The following example draws on the discussion given in the "Book of Why" [10] to illustrate collider bias.
 





5: J. Heckman was awarded the Nobel Memorial prize "for his de-velopment of theory and methods for analyzing selective samples." Source: Nobelprize.org




Figure 6.4: Our SEM predicts that this actor, A. Terminator, is (essen-tially) the most talented actor in Hollywood.
 



















The example illustrates how simple theoretical models are often used in economics. Causal reasoning is made within a simple model, such as the SEM (6.3.1). This reasoning then leads to some testable restrictions, such as negative correlation between
𝑇 and 𝐵 conditional on 𝐶 > 0. Even though we may not believe
that the stylized model provides a complete model of reality, the
 


 
implications of the simple model provide some insight into how observed phenomena, such as a negative correlation between 𝑇 and 𝐵 conditional on 𝐶 > 0, may arise. Such reversion of the correlation between two variables has been observed empirically in several cases, a prominent one being the birth-weight paradox
[11] described below.

Example 6.3.2 (Birth-weight "paradox" [11]) In a study con-ducted in 1991 in the US, it was found that infants born to smokers had higher risk of low birth-weight (LBW) and higher risk of infant mortality than infants born to non-smokers. However, when looking at the sub-group of infants with LBW, the comparison is reversed and the risk of infant mortality is lower for infants born to smokers, than for infants born to non-smokers. How is that possible? Does smoking have a positive causal effect on infant mortality conditional on LBW?
A more plausible alternative explanation can be uncovered through the lens of SEMs and Causal Diagrams if one starts to think of competing risks and collider bias. Let’s denote with 𝑆 the smoking indicator, 𝑌 the infant death outcome, and 𝐵 the low birth-weight indicator. We will also denote with 𝑈 an abstract variable corresponding to the multitude of competing risks that can cause LBW. It is highly plausible that smoking is a risk factor for LBW and also has a direct effect on mortality. Moreover, LBW and the competing risk factors can also have a direct effect on mortality. Putting these factors together leads to the Causal Diagram depicted in Figure 6.5. In this setting, an infant with a smoking parent may be highly likely to have LBW caused by smoking. At the same time, LBW can be much less frequent for non-smoking parents. When we further focus in on the group of infants of non-smoking parents with LBW, it is highly probable that LBW was caused by some other competing risk which can adversely affect mortality. Thus, conditioning on LBW, we could essentially be comparing infants of smoking parents without competing risks to infants of non-smoking parents with competing risks.
To illustrate how the unconditional association between 𝑌
and 𝑆 uncovers the true causal effect, while conditioning on
𝐵 introduces bias and can even reverse the sign of the true effect, let’s look at a simple linear SEM that corresponds to
 





𝑆


𝑌


𝑈

Figure 6.5: DAG with a collider rep-resenting low birth-weight "para-dox" Example 6.3.2.
 


 
6.4	Wage Gap Analysis and Discrimination
“The central question in any employment-discrimination case is whether the employer would have taken the same action had the employee been of a different
race (age, sex, religion, national origin etc.) and ev-erything else had remained the same.” (In Carson versus Bethlehem Steel Corp., 70 FEP Cases 921, 7th Cir. (1996) [12]).
Wage regressions are widely used by labor economists to char-acterize the wage gap between men and women and to link the wage gap to discrimination; see, e.g., [13] and [14]. Some economists have asserted that it is wrong to study discrimi-nation by doing wage gap regressions, e.g. [15], and that we should instead look at the unconditional difference in outcomes across groups. Their reasoning is based on the argument that key job characteristics – e.g., education and occupation – are determined in response to both a group identity and discrimi-nation and are therefore (intermediate) outcomes. Controlling for these characteristics may then introduce a form of selection bias. Which of these two sets of economists is right?
In what follows, we present a simple SEM in (6.4.1), which postulates that different groups receive equal wages if there
 







 
𝐺
 
Figure 6.6: A Simple Model of
𝑌	Discrimination. Here 𝐺 denotes a group (e.g., sex), 𝐻 is human capi-tal, and 𝑌 is the wage. 𝐷𝑤 denotes
unobserved wage discrimination occuring in the work place, and 𝐷ℎ denotes unobserved discrimination that occurs in the accumulation of human capital.
 

are no conditional productivity differences between the groups. We will see that, in this SEM, wage gap regressions do uncover well-defined discrimination effects that occur in wage-setting mechanisms. In contrast, the unconditonal average wage gap uncovers a more complicated causal object, which absorbs discrimination in wage setting, discrimination in human cap-ital and occupational acquisitions, as well as group specific preferences for occupations.

Here we begin with the linear SEM and the equivalent DAG shown in Figure 6.6:
𝑌 := 𝜅𝐷𝑤 + 𝜃𝐻 + 𝜖𝑌 ,
𝐷𝑤 := 𝛼𝐺 + 𝛿𝐻 + 𝜖𝐷𝑤 ,
𝐻 := 𝛾𝐺 + 𝜆𝐷ℎ + 𝜖𝐻 ,	(6.4.1)
𝐷ℎ := 𝛽𝐺 + 𝜖𝐷ℎ ,
𝐺,
where the shocks 𝜖𝑌, 𝜖𝐷𝑤 , 𝜖𝐻, 𝜖𝐷ℎ , and 𝐺 are all mean zero and uncorrelated.

 
The outcome 𝑌 is wage, 𝐺 is group (e.g., sex), 𝐻 is human capital (a scalar index that includes labor-relevant character-istics such as education, occupation, etc.),6 𝐷𝑤 is latent wage discrimination arising in the work-place, and 𝐷ℎ is latent dis-crimination arising in acquisition of human capital. There could be other observed confounders that we don’t show for the sake of simplicity.
The discrimination variables 𝐷𝑤 and 𝐷ℎ are latent variables that are important for our model but cannot be directly observed. We maintain throughout that these variables are non-degenerate and related to group identity 𝐺. Under these assumptions,
 



6: 𝐻 can be easily made a vector with a slightly more complicated notation.
 


the scale of these latent variables is non-zero but arbitrary, so we normalize the effect 𝐺  𝐷𝑤 to unity, 𝛼 = 1, and the effect 𝐺   𝐷ℎ to unity as well, 𝛽 = 1. There is no edge from
𝐺 to 𝑌, reflecting our assumption that there is no systematic
group difference in productivity conditional on 𝐻 and 𝐷𝑤. In the absence of productivity differences between workers, economic reasoning suggests that they would be assigned the same wage in a discrimination-free economy [16]. Thus, we would expect 𝜅 = 0 in a discrimination-free economy in the case
that 𝐻 captures all sources of productivity differences between
workers.

Within this model, the parameter of interest is then the causal or structural effect of discrimination on wages given by
𝜅.
If 𝜅 ≠ 0, we can conclude that wages are assigned unfairly within the framework of this SEM.

 
If we observed 𝐷𝑤 directly, we could learn the effect of dis-crimination on wages, 𝜅, by regression of 𝑌 on 𝐷𝑤 and 𝐻. Identification of 𝜅 from this regression follows from the back-door criterion discussed in Section 6.2. We don’t observe 𝐷𝑤 directly, but we postulate that this variable is determined only by 𝐺, 𝐻, and a stochastic shock. Dependence on 𝐻 captures the idea that discrimination may be larger or smaller depending on education level, profession, etc. We return to using this additional structure to learn about 𝜅 below.
Discrimination may operate through channels other than simple wage differences. For example, in the 1960s, there were rela-tively few women or African American lawyers, a highly paid occupation. Discrimination that operates through occupational choice or human capital formation is captured by latent variable
𝐷ℎ. In our model, 𝐻, which captures productivity differences between individuals, can be determined as a result of both discrimination and group preferences.7 The parameter 𝛾 then captures the effect of group preferences on the formation of 𝐻, while the effect of discrimination on 𝐻 is captured by 𝜆. Since
𝐷ℎ is not observed, there is no way to separately identify these two effects.
 




















7: For example, 90% of firefight-ers in the US are men, which may reflect a genuine preference for this occupation among men. At the same time, even preference for oc-cupation may be a result of cultural institutions that could themselves be interpreted as discriminatory in broader, cross-cultural, contexts.
 



crimination effect,
𝜅,
and that the linear regression of 𝐻 on 𝐺 recovers
𝛾 + 𝜆,
the sum of the group preference effect and the human capital discrimination effect; see Appendix 6.A for details. If a further strong assumption is made that there is no group preference effect, 𝛾 = 0, the linear regression of 𝑌 on 𝐺
recovers the total discrimination effect:
𝜅 + 𝜆(𝜅𝛿 + 𝜃).

Endogenous Sample Selection. There is an important issue with our empirical example. We are only able to look at earn-ings of people who are employed. Thus, we are conditioning on
𝑌 > 𝑅,
where 𝑅 is the reservation wage. In other words, we are conditioning on the outcome which may cause major selec-tivity issues: People get employed, and end up in our data, only if the offered wage is higher than some reservation wage. This sample selection on the basis of the outcome can cause major biases in the analysis. The potential for large biases was recognized by James J. Heckman [9] in the 70s and led to the development of the celebrated Heckman selection correction and related methods.
An alternate approach to applying a selection correction in our example is to select a subset 𝑆 of people who are employed with probability one (or very close to one). For example, one could look at highly educated, unmarried people. Within this subset, we would then have
𝑃(𝑌 > 𝑅|𝑆) ≈ 1.
That is, the value of the wage offer, 𝑌, is approximately unrelated to whether we observe individual wages for this subset of people. This type of strategy has been employed by Casey Mulligan and Yona Rubinstein [17]. Mulligan and Rubinstein continue to find evidence in favor of the existence of wage gaps in their analysis of a subsample where selection effects are likely small. This finding then
 



suggests that the broad conclusion of the existence of wage gaps is not driven entirely by sample selection issues.

 
In summary, we have the following observations:
▶ In general, wage gap regressions just estimate predictive effects or associations.
▶ When we assume a SEM like the one above holds and there are no endogenous sample selection effects, wage gap regressions estimate wage discrimination effects.
▶ Unconditional wage gaps generally reflect a combination of different types of discrimination and group preferences and thus do not isolate solely the effects of discrimination.

6.5	Notes
This chapter presented an approach to causal inference that goes back to the works of Sewall and Philip Wright [2], [3], Tinbergen [4], Haavelmo [5], and others. This tradition lives in modern structural causal models used in econometrics (espe-cially, industrial organization) and in the artificial intelligence community. The latter community, inspired by the foundational work of J. Pearl [8], strongly adopted the use of causal diagrams, known as directed acyclical graphs (DAGs). We continue explor-ing this approach throughout the remainder of our treatment on causal inference.

6.6	Notebooks
Notebook 6.6.1 (Collider Bias) Collider Bias R Notebook and Collider Bias Python Notebook provide a simple simulated example of collider bias, informing our discussion of condi-tioning on Celebrity in our Structural Model of Hollywood.


6.7	Exercise
Exercise 6.7.1 (Collider Bias) Explain collider bias to a friend in simple terms. Use no more than two paragraphs. Illustrate
 










Figure 6.7: Early 20th century: The work of Sewall and Philip Wright made it possible for humans to be-gin to "fly" in the space of causal models. Another family of Wrights made it possible for humans to be-gin to fly in the air.

Figure 6.8: An early drawing for an airplane appears very much like an early drawing of a DAG.

Figure 6.9: DAG for Supply-Demand Systems in P. Wright’s work in 1928 [2].
 


your explanation using a simulation experiment.

Exercise 6.7.2 (Wage Gap Revisited) Empirical: Revisit the group wage gap analysis from Chapter 4, focusing on college-educated workers. Is there a structural/causal interpretation for the estimated wage gap? Is there a group gap in educa-tion achievement? Does this group gap in education have a structural/causal interpretation? Some of these questions are open ended and have no simple answers, but it is useful to think about them. (If you have other data sets that might illuminate discrimination in other settings, please use them in place of the wage data set).

Exercise 6.7.3 (Mechanisms for Wage Gap) Free-style exercise: The model for wage discrimination presented in our notes is very stylized and subject to multiple criticisms. For example, it does not deal with promotion and hiring decisions. There are several interesting models of discrimination in hiring, college admissions, and pay. For example, see "The Book of Why"[10] and the Bickel et al. 1975 paper [18] for an analysis of Berkeley undergraduate admissions decisions. Nina Roussile’s (2020)
[19] paper isolates the ask gap as the central mechanism for the subsequent wage gap. Referring to one such analysis, draw or write down a linear structural causal model that captures the structural idea of the analysis and discuss identification in the model.
 

6.A Details of the Wage Discrimination Analysis

 
We write out some of the structural equations corresponding to our stylized DAG for discrimination (Figure 6.6):
𝑌 := 𝜅𝐷𝑤 + 𝜃𝐻 + 𝜖𝑌 ,	𝜖𝑌 ⊥ 𝐷𝑤 , 𝐻, 𝐺
𝐷𝑤 := 𝐺 + 𝛿𝐻 + 𝜖𝐷𝑤 ,	𝜖𝐷𝑤 ⊥ 𝐺, 𝐻
where the orthogonality relations are implied by the model.
Linear regression analysis would use observable variables only, so we substitute the model for the unobserved 𝐷𝑤 in terms of
𝐺 and 𝐻 into the equation for 𝑌 to obtain
𝑌 = 𝜅𝐺 + (𝜅𝛿 + 𝜃)𝐻 + 𝑈,	𝑈 := 𝜅𝜖𝐷𝑤 + 𝜖𝑌 ⊥ (𝐺, 𝐻).
The composite error term 𝑈 is orthogonal to 𝐺 and 𝐻. Therefore, regression of 𝑌 on 𝐺 and 𝐻 learns 𝜅 and 𝜅𝛿  𝜃 , with our main target being 𝜅. We can also see that by partialling out 𝐻,
𝑌˜ = 𝜅𝐺˜ + 𝑈, 𝑈 ⊥ 𝐺˜ .
Thus, 𝜅 is retrievable only if there is non-zero variation in 𝐺˜
after taking out the linear effect of 𝐻.
Now suppose we want to study discrimination effects in occu-pational choices, captured by 𝐻 in our model. We write out the relevant structural equations:
𝐻 := 𝛾𝐺 + 𝜆𝐷ℎ + 𝜖𝐻 ,	𝜖𝐻 ⊥ (𝐺, 𝐷ℎ),
𝐷ℎ := 𝐺 + 𝜖𝐷ℎ ,	𝜖𝐷ℎ ⊥ 𝐺.
Recall that 𝛾 is the group preference effect and 𝜆 is the discrim-ination effect. Since 𝐷ℎ is not directly observed, we substitute it out to arrive at
𝐻 = (𝛾 + 𝜆)𝐺 + 𝑉;	𝑉 := 𝛾𝜖𝐷ℎ + 𝜖𝐻 ⊥ 𝐺.
Therefore, 𝛾	𝜆 is the projection coefficient in the projection of
𝐻 on 𝐺. Hence, we can identify 𝛾	𝜆, but we can’t identify 𝛾
and 𝜆 separately.
Going further, suppose that the group preference effect is zero, so 𝛾 = 0. Then, the previous argument would identify 𝜆 and we could identify the total discrimination effect arising from
two different channels:
 


















“This is elementary, my dear Wat-son,” said Sherlock Holmes after seeing this.
 
𝜅 + 𝜆(𝜅𝛿 + 𝜃).
 


from the regression of 𝑌 on 𝐺.
We can assert that the unconditional difference in wages mea-sures discrimination only if the group preference effect in determining 𝐻 is zero (𝛾 = 0). Of course, most economists
would probably not agree with the assumption that 𝛾 = 0.
Empirically, there are large differences in group composition
among different professions. These differences likely reflect both discrimination and genuine preferences.
 


Bibliography



[1]		R. Frisch. ‘A Dynamic Approach to Economic Theory: Lectures by Ragnar Frisch at Yale University’. Frisch Archives, Department of Economics, University of Oslo. 1930 (cited on page 152).
[2]		Philip G. Wright. The Tariff on Animal and Vegetable Oils. New York: The Macmillan company, 1928 (cited on pages 153, 156, 158, 166).
[3]		Sewall Wright. ‘Correlation and Causation’. In: Journal of Agricultural Research 20.7 (Jan. 1921), pp. 557–585 (cited
on pages 153, 156, 158, 166).
[4]	Jan Tinbergen. ‘Bestimmung und Deutung von Angebot-skurven Ein Beispiel’. In: Zeitschrift für Nationalökonomie
1.5 (1930), pp. 669–679 (cited on pages 153, 166).
[5]	Trygve Haavelmo. ‘The probability approach in econo-metrics’. In: Econometrica 12 (1944), pp. iii–vi+1–115 (cited on pages 153, 166).
[6]		Jerry A. Hausman and Whitney K. Newey. ‘Nonpara-metric estimation of exact consumers surplus and dead-weight loss’. In: Econometrica 63.6 (1995), pp. 1445–1476 (cited on page 153).
[7]	Charles W. Cobb and Paul H. Douglas. ‘A Theory of Production’. In: The American Economic Review 18.1 (1928),
pp. 139–165 (cited on page 153).
[8]		Judea Pearl. Causality. Cambridge University Press, 2009 (cited on pages 157, 166).
[9]	James J. Heckman. ‘Sample selection bias as a specifica-tion error’. In: Econometrica 47.1 (1979), pp. 153–161 (cited
on pages 160, 165).
[10]	Judea Pearl and Dana Mackenzie. The Book of Why. Pen-guin Books, 2019 (cited on pages 160, 167).
[11]		Sonia Hernández-Díaz, Enrique F Schisterman, and Miguel A Hernán. ‘The birth weight “paradox” uncovered?’ In: American Journal of Epidemiology 164.11 (2006), pp. 1115–
1120 (cited on page 161).
[12]	‘Carson v. Bethlehem Steel Corp.’ In: 82 F.3d 157, 158, 7th
Cir. (1996) (cited on page 162).
 

 
Bibliography
 
171
 


[13]		Francine D. Blau and Lawrence M. Kahn. ‘The gender wage gap: Extent, trends, and explanations’. In: Journal of Economic Literature 55.3 (2017), pp. 789–865 (cited on
page 162).
[14]	Sonja C. Kassenboehmer and Mathias G. Sinning. ‘Distri-butional changes in the gender wage gap’. In: ILR Review
67.2 (2014), pp. 335–361 (cited on page 162).
[15]	Elise Gould, Jessica Schieder, and Kathleen Geier. ‘What is the gender pay gap and is it real’. In: Economic Policy Institute (2016) (cited on page 162).
[16]	Gary S. Becker. The Economics of Discrimination. University of Chicago Press, 2010 (cited on page 164).
[17]		Casey B. Mulligan and Yona Rubinstein. ‘Selection, Invest-ment, and Women’s Relative Wages Over Time’. In: Quar-terly Journal of Economics 123.3 (2008), pp. 1061–1110. doi: 10.1162/qjec.2008.123.3.1061 (cited on page 165).
[18]		Peter J. Bickel, Eugene A. Hammel, and J. William O’Connell. ‘Sex Bias in Graduate Admissions: Data from Berkeley: Measuring bias is harder than is usually assumed, and the evidence is sometimes contrary to expectation.’ In: Science 187.4175 (1975), pp. 398–404 (cited on page 167).
[19]	Nina Roussille. ‘The central role of the ask gap in gen-der pay inequality’. In: URL: https://ninaroussille. github. io/files/Roussille_ askgap. pdf 34 (2020), p. 35 (cited on
page 167).
 

 
Causal Inference via Directed Acyclical Graphs and Nonlinear Structural
Equation Models


"you are smarter than your data. Data do not un-derstand causes and effects; humans do."
– Judea Pearl [1].










Here, we explore a fully nonlinear, nonparametric formulation of causal diagrams and their associated structural equation models (SEMs). These models offer a powerful and flexible tool for understanding the structures that underpin causal identifica-tion, allowing us to move beyond restrictive linear assumptions. Using these structures, we define potential outcomes—also known as counterfactuals—following what Judea Pearl terms the "First Law of Causal Inference," which establishes that SEMs naturally induce these outcomes. This foundation enables a systematic approach to causal analysis. Moreover, we can algo-rithmically verify, using the directed acyclic graphs (DAGs) that encode these structures, whether the conditional ignorability conditions necessary to transform predictive regressions into causal inferences are satisfied. In fact, given a DAG, we can derive sufficient adjustment sets—sets of variables to condition on in regressions—that enable us to uncover average causal effects. This process leverages the graphical representation of contextual knowledge to ensure that the statistical relation-ships we observe reflect true causal impacts, rather than mere correlations.
 
7
7.1	Introduction	173
7.2	General DAG and SEMs via an Example	174
The Impact of 401(k) Eligi-bility on Financial Wealth 174 The DAG as a Markovian Model	175
The DAG as a Structural Equations Model	176
Intervention and Counterfac-tual DAG and SEM	176
Conditional Ignorability/Ex-ogeneity	178
Wrap-Up and Implications for 401(k) Analysis	180
7.3	Definitions of General DAGs and ASEMs	180
From DAGs to ASEMs . 181 D-Separation and Testable Restrictions	182
7.4	Counterfactuals and Identi-fication by Conditioning  185
Counterfactuals	185
Ignorability by D-Separation in Counterfactual DAGs	186
Ignorability by Backdoor Blocking in Factual DAG	189
7.5	Notes	190
7.6	Additional Resources	191
7.7	Notebooks	191
7.8	Exercises	192
7.A	Review of Conditional Inde-pendence	193
7.B	Theoretical Details of d-Separation★	193
7.C	Faithfulness and Causal Dis-covery	195
 
7.1	Introduction
 

The purpose of this module is to present a formal, fully nonlinear (nonparametric) formulation of structural equation models (SEMs) and their corresponding causal directed acyclic graphs (DAGs). We explore the concepts and identification results pioneered by Judea Pearl and his collaborators, as well as those developed by James M. Robins and his collaborators.1
SEMs define a recursive system of equations that generate vari-ables. From these models, we can derive counterfactuals, also known as potential outcomes (POs).2 We represent both fac-tual and counterfactual variables using DAGs, leveraging their structure to deduce the conditional independence conditions (e.g., ignorability, exogeneity) required to transform predictive regressions into causal inferences.
We then examine two approaches for identifying variables to adjust for (condition on) when estimating the causal effect of a treatment on an outcome using DAGs: the backdoor adjustment approach and the counterfactual DAG approach. The backdoor criterion, developed by Judea Pearl, analyzes the factual DAG to identify a set of variables that blocks all backdoor paths – paths from the treatment to the outcome beginning with an arrow into the treatment– while ensuring these variables are not descendants of the treatment, thereby eliminating confounding influences. In contrast, the counterfactual DAG approach uses a modified DAG where the treatment is hypothetically fixed to a specific value, identifying a set of variables that “d-separates” the natural treatment value from the counterfactual outcome, ensuring conditional independence between the treatment and the counterfactual outcome given these variables.
This chapter is divided into two parts. The first introduces key concepts through a specific empirical example, while the second provides general, albeit more technical, mathematical definitions. Additional technical material is included in the appendix.
Notation. Consider a pair of random variables (or, equivalently, random vectors) 𝑈 and 𝑉 with joint probability (or mass) function p𝑈𝑉(𝑢, 𝑣) evaluated at (𝑢, 𝑣). When no ambiguity arises, we denote p𝑈𝑉(𝑢, 𝑣) simply as p(𝑢, 𝑣). Their marginal probability (or mass) functions are denoted by p𝑈(𝑢) and p𝑉(𝑣), or simply p(𝑢) and p(𝑣). We say that 𝑈 and 𝑉 are independent (denoted 𝑈 ⊥ 𝑉) if and only if the joint function factorizes as
p(𝑢, 𝑣) = p(𝑢) p(𝑣),
 








1: In 2011, J. Pearl received the A.M. Turing Award, the highest honor in Computer Science and Artificial Intelligence, “for fundamental con-tributions to artificial intelligence through the development of a cal-culus for probabilistic and causal reasoning.” In his 1995 Biometrika article [2], Pearl frames his work as a generalization of the SEMs pro-posed by T. Haavelmo [3] in 1944 and others.
2: Pearl refers to the implication of POs from SEMs as the “First Law of Causal Inference.”
 


or equivalently, if E 𝑔 𝑈 ℓ 𝑉	= E 𝑔 𝑈 E ℓ 𝑉	for any bounded functions 𝑔 and ℓ . This definition of independence implies the ignorability or exclusion results,
p(𝑢 | 𝑣) = p(𝑢),	p(𝑣 | 𝑢) = p(𝑣),
which follow from Bayes’ law. Conditional independence is defined similarly by replacing distributions and expectations with their conditional counterparts. Appendix 7.A reviews some useful results on conditional independence.

7.2	General DAG and SEMs via an Example
The best way to learn the main ideas behind modern SEMs and causal directed acyclic graphs DAGs is to work through a concrete example.

The Impact of 401(k) Eligibility on Financial Wealth
A 401(k) is a U.S. employer-sponsored retirement plan that allows workers to contribute a portion of their wages—often pre-tax—to investment accounts, sometimes with matching contributions from their employer. Figure 7.1 shows one possible causal diagram for this problem:

 
𝑌










This diagram represents how 401(k) eligibility (𝐷) might affect an individual’s net financial assets (𝑌) both directly and indi-rectly through the employer’s matching contribution (𝑀). It includes observed worker-level characteristics (𝑋), unobserved firm-level characteristics (𝐹), and latent factors (𝑈) that may influence the pathway from eligibility to financial outcomes.
 


Figure 7.1: Causal Diagram for 401(k): 𝑌 represents net financial assets; 𝐷 denotes eligibility for a 401(k) program; 𝑋 includes ob-served worker-level covariates (e.g., income); 𝐹 represents unobserved firm-level covariates; 𝑀 denotes the employer’s matching contribu-tion; and 𝑈 captures general latent factors. Circled nodes denote latent variables.
 

 
This representation reasonably captures the context of the un-derlying problem and what variables were collected in the original study [4] of the 401(k) plans.3

The DAG as a Markovian Model
The DAG above formally represents the conditional dependen-cies among the variables, allowing us to express their joint dis-tribution in terms of conditional distributions. In these graphs, each node represents a random variable (or vector), and an arrow from one node (a “parent”) to another (a “child”) indi-cates that the parent directly influences the child, establishing statistical dependency.
The Markov property states that each variable is conditionally independent of all non-parents (and non-descendants) given its parents. Consequently, the joint probability distribution can be written as the product of each variable’s conditional distribution given its parents.
In our example, the variables are:
𝑈,	𝐹,	𝑋,	𝐷,	𝑀,	𝑌,
with the following parent-child relationships based on the DAG:
▶ 𝑈 has no parents (a “root” node),
▶ 𝐹 has parent 𝑈,
▶ 𝑋 has parent 𝑈,
▶ 𝐷 has parents 𝐹 and 𝑋,
▶ 𝑀 has parents 𝐷, 𝐹, and 𝑋,
▶ 𝑌 has parents 𝐷, 𝑀, and 𝑋.
According to the chain rule for Markovian networks,4 the joint distribution p(𝑢, 𝑓 , 𝑥, 𝑑, 𝑚, 𝑦) factorizes as:
p(𝑢, 𝑓 , 𝑥, 𝑑, 𝑚, 𝑦) = p(𝑢)
× p( 𝑓 | 𝑢) p(𝑥 | 𝑢)
× p(𝑑 | 𝑓 , 𝑥)
× p(𝑚 | 𝑑, 𝑓 , 𝑥)
× p(𝑦 | 𝑑, 𝑚, 𝑥).

Because 𝑈 is a root node, its distribution is simply p 𝑢 . Each subsequent variable is represented by its conditional distri-bution given its parents. This approach is a cornerstone of
 



3: Please check this assertion by querying AI.


































4: Markovian networks, also known as Bayesian Markovian Networks or simply Bayesian Networks, are a type of probabilis-tic graphical model. We use the term ’Markovian networks’ as it is arguably more precise.
 


probabilistic graphical models, clarifying the conditional in-dependencies and potential pathways of influence among the variables.

The DAG as a Structural Equations Model
Following Pearl, we interpret the DAG as implying that (or being implied by) the following system of structural equations holds:
 








where
 
𝑌 := 𝑓𝑌 (𝐷, 𝑀, 𝑋, 𝜖𝑌) ,
𝐷 := 𝑓𝐷 (𝐹, 𝑋, 𝜖𝐷) ,

𝐹 := 𝑓𝐹 𝑈, 𝜖𝐹 ,
𝑈 := 𝜖𝑈 ,

𝜖𝑌 , 𝜖𝑀 , 𝜖𝐷 , 𝜖𝑋 , 𝜖𝐹 , 𝜖𝑈
 
are mutually independent stochastic shocks (which may be vector-valued), and 𝑓𝑌 , 𝑓𝑀 , 𝑓𝐷 , 𝑓𝑋 , 𝑓𝐹 are structural functions.
Here, each variable is defined as a function of its parent variables (as determined by the DAG) and its own exogenous noise 𝜖. For instance, the equation
𝑌 := 𝑓𝑌 𝐷, 𝑀, 𝑋, 𝜖𝑌
indicates that net financial assets 𝑌 are determined by eligibility
𝐷, the matching contribution 𝑀, observed covariates 𝑋, and an unobserved shock 𝜖𝑌. The assignment operator (:=) signi-fies that variables are generated recursively, starting from the
root and proceeding through subsequent layers based on their parents and noise terms.

Intervention and Counterfactual DAG and SEM
Thus far, the DAG and SEM we have formulated lack inherent causal meaning. Causality emerges when we introduce the concept of an intervention. In particular, consider intervening by replacing 𝐷 with a fixed value 𝑑 in the equations for all descendants of 𝐷. By assumption, the structural equations remain invariant under such an intervention—this invariance
 


is the essence of their "structural" nature. Consequently, we obtain the counterfactual outcomes:
𝑌(𝑑) := 𝑓𝑌 𝑑, 𝑀(𝑑), 𝑋, 𝜖𝑌 ,
𝑀(𝑑) := 𝑓𝑀 𝑑, 𝐹, 𝑋, 𝜖𝑀 ,
where 𝑌 𝑑 denotes the potential net financial wealth under treatment 𝑑 and 𝑀 𝑑 represents the matching contribution under 𝑑. The remaining equations remain unchanged. Thus, the complete counterfactual system is:
𝑌(𝑑) := 𝑓𝑌 𝑑, 𝑀(𝑑), 𝑋, 𝜖𝑌 ,
𝑀(𝑑) := 𝑓𝑀 𝑑, 𝐹, 𝑋, 𝜖𝑀 ,
𝐷 := 𝑓𝐷 (𝐹, 𝑋, 𝜖𝐷) ,

𝐹 := 𝑓𝐹 𝑈, 𝜖𝐹 ,
𝑈 := 𝜖𝑈 .
(Note: An alternative formulation, called the do-counterfactual, omits the equation for the naturally generated 𝐷; the version here is known as the fix-counterfactual.)
The invariance assumption ensures that the functional forms
𝑓𝑌, 𝑓𝑀, etc., remain unchanged even when we set 𝐷 = 𝑑 in the equations for 𝑌 and 𝑀. Although the original equation for
𝐷 remains in the model, the intervention fixes 𝐷 at 𝑑 for the purpose of determining 𝑌(𝑑) and 𝑀(𝑑).
We can now construct a counterfactual DAG—also known as a SWIG (Single World Intervention Graph)—that corresponds to this counterfactual system.

 




𝐹	𝑋

 
𝑈
 
Figure 7.2: Counterfactual DAG for the intervention 𝐷 = 𝑑. Here,
𝑌 𝑑  =  𝑓𝑌 𝑑, 𝑀 𝑑 , 𝑋, 𝜖𝑌  and
𝑀 𝑑 = 𝑓𝑀 𝑑, 𝐹, 𝑋, 𝜖𝑀 reflect the intervention (with 𝑑 shown as a de-terministic node). The natural node
𝐷 is still generated by 𝑓𝐷 𝐹, 𝑋, 𝜖𝐷 , but its outgoing arrows are re-moved. Other nodes: 𝑋 (worker-level covariates), 𝐹 (firm-level co-variates), and 𝑈 (latent factors).
 

Conditional Ignorability/Exogeneity
The fact that the SEM implies potential outcomes is known as the First Law of Causal Inference. This equivalence means that nothing is lost by working with SEMs/DAGs instead of potential outcomes directly. Moreover, because SEMs/DAGs encapsulate the contextual knowledge of a problem, we can derive the conditional ignorability/exogeneity condition from the model rather than merely postulating it. For example, in our case we deduce that
𝑌(𝑑) ⊥ 𝐷 | 𝐹, 𝑋,
which implies that
E 𝑌(𝑑) | 𝐹, 𝑋  = E 𝑌 | 𝐷 = 𝑑, 𝐹, 𝑋 ,
allowing us to identify average causal (or treatment) effects by adjusting (or conditioning on 𝐹, 𝑋.)
There are at least mulitple ways to verify that 𝐹, 𝑋 satisfy this condition:

Functional (Structural) Argument.

In the counterfactual setting where we fix 𝐷 = 𝑑, the relevant structural equations are
𝑌(𝑑) = 𝑓𝑌 𝑑, 𝑀(𝑑), 𝑋, 𝜖𝑌	and	𝑀(𝑑) = 𝑓𝑀 𝑑, 𝐹, 𝑋, 𝜖𝑀 .
The random variable 𝐷 is still generated by
𝐷 = 𝑓𝐷 𝐹, 𝑋, 𝑈, 𝜖𝐷 .
Once we condition on 𝐹 and 𝑋, the distribution of 𝑌 𝑑 is deter-mined solely by the noise terms 𝜖𝑌 and 𝜖𝐷; the realized value of
𝐷, on the other hand, is determined by the independent terms
𝑈 and 𝜖𝐷. The two sets of noise terms are independent. Thus, formally, 𝑌 𝑑 is statistically independent from 𝐷, conditional on 𝐹 and 𝑋:
𝑌(𝑑) ⊥ 𝐷 | 𝐹, 𝑋.
In other words, once 𝐹 and 𝑋 are given, knowing 𝐷 adds no additional information about 𝑌(𝑑).
A similar argument shows that 𝐷 is not ignorable when condi-tioning on worker characteristics 𝑋 alone:
𝑌(𝑑) ⊥ 𝐷 | 𝑋.
 


 
Omitted firm characteristics 𝐹 induce a dependency between the potential outcomes and the treatment 𝐷, posing a problem for studies that control for 𝑋 but not 𝐹.5

D-Separation (Graphical) Argument
In the counterfactual DAG, 𝑌 𝑑 receives inputs from 𝑀 𝑑 ,
𝑋, and the fixed node 𝑑. Although 𝐷 remains in the graph (generated by its usual parents 𝐹, 𝑋, and 𝑈), there is no arrow from 𝐷 to 𝑌 𝑑 . Any path from 𝐷 to 𝑌 𝑑 must traverse 𝐹 or 𝑋. For example, the paths are:
1.	𝐷 ← 𝑋 → 𝑌(𝑑),
2.	𝐷 ← 𝐹 → 𝑀(𝑑) → 𝑌(𝑑),
3.	𝐷 ← 𝐹 ← 𝑈 → 𝑋 → 𝑌(𝑑).
Conditioning on 𝐹 and 𝑋 is said to block these paths (condi-tioning on a node severs information flow), which then makes
𝐷 to be d-separated from 𝑌 𝑑 given 𝐹, 𝑋 .6 By the equiva-lence between d-separation and conditional independence,7 we conclude that
𝑌(𝑑) ⊥ 𝐷 | 𝐹, 𝑋.

Backdoor Blocking (Graphical) Argument
Another approach for identifying the average causal effect of 𝐷 on 𝑌 uses the original DAG instead of the counterfactual DAG. We note though that this principle was in fact derived by J. Pearl
[2] from the counterfactual DAG of the form stated above.
The goal is to identify a set 𝑍 that blocks all backdoor paths
between 𝐷 and 𝑌. A set 𝑍 satisfies the backdoor criterion if:
1.	No variable in 𝑍 is a descendant of 𝐷, and
2.	𝑍 blocks8 every backdoor path from 𝐷 to 𝑌 (a backdoor path starts with an arrow into 𝐷).
The first rule prevents blocking causal paths from 𝐷 to 𝑌, such as 𝐷   𝑀   𝑌.9 The second rule ensures that conditioning on 𝑍 eliminates all non-causal paths that could confound the relationship between 𝐷 and 𝑌. Thus conditional on 𝑍, the statistical association between 𝑌 and 𝐷 only reflects the causal channels.
In the 401(k) diagram, the backdoor paths from 𝐷 to 𝑌 run through 𝐹 and 𝑋:
1.	𝐷 ← 𝑋 → 𝑌,
2.	𝐷 ← 𝐹 → 𝑀 → 𝑌,
 


5: In such cases, omitted vari-able bias can still be studied and bounded; see later chapters and [5].
















6: See the formal definition of d-separation and blocking later in the chapter.
7: Equivalence of d-separtion and conditional independence is called Global Markov property and is a fundamental result in the DAG the-ory.










8: See the formal definition of blocking later in the chapter.


9: It also prevents conditioning on colliders, examples of which we have seen the previous chapter.
 

3.	𝐷 ← 𝐹 ← 𝑈 → 𝑋 → 𝑌.
By conditioning on both 𝐹 and 𝑋, we are said to block all such paths, allowing us to identify the average causal effect of 𝐷 on
𝑌, assuming no additional unobserved confounding.

Wrap-Up and Implications for 401(k) Analysis
Both functional and graphical perspectives yield the same con-clusion. Functionally, once 𝐹 and 𝑋 are fixed, 𝑌 𝑑 is governed solely by noise terms independent of those influencing the natural value of 𝐷. Graphically, conditioning on 𝐹 and 𝑋 blocks all paths from 𝐷 to 𝑌 𝑑 in the counterfactual DAG, and in the original DAG, all backdoor paths are blocked by 𝐹, 𝑋 . In either case, we have
𝑌(𝑑) ⊥ 𝐷 | 𝐹, 𝑋,
which formalizes the idea that, once 𝐹 and 𝑋 are taken into account, the naturally generated 𝐷 is irrelevant for the counter-factual outcome 𝑌 𝑑 . This conditional ignorability allows us to identify the average causal effect of 𝐷 on 𝑌.

7.3		Definitions of General DAGs and ASEMs

 
The purpose of this section is to generalize the previous example to encompass general ASEMs and DAGs. Here, we provide con-cise general definitions and present key mathematical results.10

A graph G is an ordered pair 𝑉, 𝐸 , where 𝑉 = 1, . . . , 𝐽 is a set of vertices (nodes) and 𝐸 is a collection of edges, represented by entries 𝑒𝑖𝑗 ∈ {0, 1} for (𝑖, 𝑗) ∈ 𝑉 × 𝑉.
Given a collection of random variables 𝑋 = 𝑋𝑗 𝑗 𝑉, we asso-ciate each index 𝑗 with 𝑋𝑗 and use them interchangeably for convenience. If an edge 𝑖, 𝑗 exists (i.e., 𝑒𝑖𝑗 = 1), we interpret it as
 



10: Although we believe the previ-ous examples effectively illustrate the core concepts, presenting gen-eral definitions and results remains essential, given the foundational role of ASEMs and DAGs in causal inference.
 
“𝑋𝑖 → 𝑋𝑗”	or	“𝑋𝑖 is an immediate cause of 𝑋𝑗”.

Consider a strict partial order < on 𝑉 induced by 𝐸, where
𝑋𝑗 < 𝑋𝑘 (read as “𝑋𝑗 is determined before 𝑋𝑘”) means either
 

 
𝑋𝑗 → 𝑋𝑘 or there exists a directed path
𝑋𝑗 → 𝑋𝑣1 → · · · → 𝑋𝑣𝑚 → 𝑋𝑘 .
A partial ordering exists if no node precedes itself (i.e., the graph contains no cycles).11
 






11: The absence of cycles ensures that 𝑋𝑗 < 𝑋𝑗 never holds.
 
 


From DAGs to ASEMs
Every causal DAG implicitly defines a nonparametric acyclic structural equation model (ASEM); the two are equivalent rep-resentations of the same assumptions about the data-generating process. In this perspective, DAGs serve as visual depictions of ASEMs, while ASEMs provide the structural equation formula-tions of DAGs.

 


 
In linear ASEMs, the requirement of independent errors may be relaxed to uncorrelated errors.


The stochastic shocks 𝜖𝑗 𝑗 𝑉 are called exogenous variables, while the variables 𝑋𝑗 𝑗 𝑉 are endogenous; the latter are determined by the model equations, whereas the former are not.
The joint distribution of variables in an ASEM is characterized by the following theorem.


D-Separation and Testable Restrictions
Next, we examine the constraints on the data-generating process implied by a given DAG. We introduce the concept of d-separation
 


and demonstrate that it implies conditional independence, known as the global Markov condition associated with the DAG. To proceed, we first require several definitions.


 
 
In Figure 7.3, the backdoor path 𝑌	𝑋	𝐷 is blocked by setting 𝑆 = 𝑋.

In Figure 7.4, the path 𝑌	𝐶	𝐷 is blocked (by the empty set) but becomes open when conditioned on the collider 𝐶.
 
𝑍   𝐷   𝑌

 
Figure 7.3: The path 𝑌   𝑋   𝐷
is blocked by conditioning on 𝑋.

𝑍   𝐷   𝑌
 
Figure 7.4: The path 𝑌  𝐶  𝐷 is blocked but opens when condi-tioning on 𝐶.
 



The following theorem establishes a fundamental link between d-separation and conditional independence.

 


 
Intuitively, conditioning on 𝑆 interrupts the information flow between 𝑋 and 𝑌, rendering them unable to predict each other given 𝑆. While this result is intuitive and verifiable in simple cases, its formal proof is nontrivial. The converse does not generally hold but is argued to hold “generically” (see Section 7.C).

𝑌

Figure	7.5:	Example	of	d-separation.



𝑌
 
 




These testable restrictions—known as exclusion restrictions in econometrics—can be expressed as
𝑌 ⊥ 𝑋 | 𝑍	⇐⇒	p(𝑦 | 𝑥, 𝑧) = p(𝑦 | 𝑧),	(7.3.1) which is equivalent to
E[𝑔(𝑌) | 𝑋, 𝑍] = E[𝑔(𝑌) | 𝑍]	(7.3.2)
for any bounded function 𝑔. In other words, 𝑋 does not improve the prediction of 𝑔 𝑌 when 𝑍 is known. Numerous tests for such restrictions exist in the literature; see, e.g., [7].12
In linear ASEMs, these tests reduce to hypotheses about regres-sion coefficients. For example, to test whether 𝑌  𝑋 𝑍, one can examine whether the coefficient 𝛼 = 0 in the regression
𝑌 = 𝛼′𝑋 + 𝛽′𝑍 + 𝜖,	𝜖 ⊥ 𝑍.
Standard statistical tools can implement such tests; see the R Dagitty Notebook and the Python Pgmpy Notebook 7.7.1 for
 
𝑈

Figure 7.6: Example of d-separation.













12: Examples include conditional independence tests, exclusion re-striction tests, or conditional mo-ment tests.
 


examples.


 
7.4	Counterfactuals and Identification by Conditioning
Counterfactuals
In this subsection, we focus on counterfactuals induced by fix interventions, a concept that builds on the foundation of causal reasoning.13 This approach allows us to explore hypothetical scenarios while preserving the underlying structure of the original model, making it particularly useful for understanding causal effects in complex systems.
 










13: A fix-intervention extends the do-intervention by retaining the natural version of the variable while creating an intervention ver-sion. In contrast, the original do-intervention, introduced by Pearl, erases the natural version by replac-ing it entirely with the intervention version.
 


 
Interventions like fix 𝑋𝑗 = 𝑥𝑗 induce new counterfactual distributions for the endogenous variables, offering a win-dow into what would happen under specific conditions. Non-descendants of 𝑋𝑗 remain unchanged, so 𝑋𝑘∗ = 𝑋𝑘 for all 𝑋𝑘 that
are not downstream of 𝑋𝑗. For simplicity, we drop the "stars"
from counterfactual variables that exactly replicate their factual counterparts, streamlining notation where the intervention has no effect.

Ignorability by D-Separation in Counterfactual DAGs
Consider any variable 𝐷 in an ASEM as a treatment of interest and one of its descendants 𝑌 as an outcome we wish to study. Our goal is to identify the causal effect of 𝐷 on 𝑌, which requires finding an adjustment set 𝑆 that ensures conditional exogeneity, or ignorability. This condition is formally expressed as:
𝑌(𝑑) ⊥ 𝐷 | 𝑆,
meaning that, given 𝑆, the counterfactual outcome 𝑌 𝑑 is independent of the natural value of 𝐷. This independence allows us to isolate the causal impact of setting 𝐷 = 𝑑.

To achieve this, we construct the counterfactual DAG in-duced by the fix(𝐷 = 𝑑) intervention, which replaces 𝐷 with the fixed value 𝑑 in all structural equations defining
its descendants. This modified graph, or SWIG, reflects the system under the intervention. If, in this SWIG, 𝑌(𝑑) is d-separated from 𝐷 by a set 𝑆, then the conditional exogeneity condition holds:
𝑌(𝑑) ⊥ 𝐷 | 𝑆.
D-separation here means that all paths between 𝐷 and
𝑌(𝑑) are blocked by 𝑆, ensuring no confounding influences remain.

Given conditional exogeneity, we can identify counterfactual expectations, such as E[𝑔(𝑌(𝑑)) | 𝑆 = 𝑠], from observed data,
 


specifically by the regression E 𝑔 𝑌  𝑆 = 𝑠, 𝐷 = 𝑑 , provided the positivity condition p 𝑠, 𝑑 > 0 holds. This positivity ensures that every combination of 𝑆 = 𝑠 and 𝐷 = 𝑑 we condition on is observable in the data. By exogeneity and consistency, we
have:
E[𝑔(𝑌(𝑑)) | 𝑆 = 𝑠] = E[𝑔(𝑌) | 𝑆 = 𝑠, 𝐷 = 𝑑],
and integrating over 𝑆 gives the average potential outcome: E[𝑔(𝑌(𝑑))] = E rE[𝑔(𝑌) | 𝑆, 𝐷 = 𝑑]l ,

assuming p 𝑠, 𝑑 > 0 for all 𝑠 in the support of 𝑆	𝐷 = 𝑑. This process links hypothetical outcomes to measurable quantities.
The following theorem, essentially due to [9], formalizes this approach.









 
𝑍2
 
  𝑋3	𝑌
 
 	 
 


𝑍1
 
𝑋2	𝑀
 	   
  𝑋1   𝐷
 

 


This criterion is "complete" in that it captures all valid adjust-
 
Figure 7.7: A DAG in Pearl’s Exam-ple.
 
ment sets excluding descendants of 𝐷. As discussed in [9], it encompasses all adjustment sets verifiable through an im-plementable intervention, providing a robust tool for causal inference in practice.
 
𝑍2




𝑍1
 
𝑋3

𝑋2

𝑋1
 
 
 
𝐷	𝑑
 

Figure 7.8: The DAG induced by the fix 𝐷 = 𝑑 intervention in Pearl’s Example.
 


 

 

 
Ignorability by Backdoor Blocking in Factual DAG
Pearl [8] developed a powerful and practical criterion for es-tablishing conditional exogeneity/ignorability by analyzing the structure of the factual DAG, without needing to construct a counterfactual DAG. This method, known as the backdoor criterion, simplifies the process of identifying valid adjustment sets and is widely applied in causal inference studies, espe-cially when working directly with observed data. We note that the proof provided by Pearl [2] itself relies on the concept of d-separation within a counterfactual DAG.14 This approach un-derscores the deep connection between the backdoor criterion and counterfactual reasoning.
 












14: Specifically, Pearl constructs a modified DAG by removing all out-going edges from 𝐷 and then exam-ines whether the remaining paths from 𝐷 to 𝑌 are blocked, meaning they are d-separated. This modi-fied DAG is essentially equivalent to the counterfactual DAG used in the fix-intervention framework.
 






A backdoor path, recall, is one that starts at 𝐷 and ends with an arrow pointing into 𝐷, representing confounding influences. The key insight is that blocking these backdoor paths elimi-nates confounding, leaving only the direct and indirect causal channels from 𝐷 to 𝑌. This approach is intuitive and efficient because it focuses on the factual DAG structure.

 


 
Applying the backdoor criterion systematically can yield all minimal adjustment sets needed for identification; see [8]. How-ever, it may not capture every possible valid set. Let’s revisit the DAG: 𝑍  𝐷  𝑌. Here, conditioning on 𝑍 does not satisfy the backdoor criterion because 𝑍 is descendant of 𝐷, but it is a valid control variable. Here 𝐷 directly causes 𝑌, and there is no need to condition on 𝑍. If we do condition 𝑍, it does not confound the direct effect. However, conditioning on
𝑍 may lower the precision with which we estimate the effect of 𝐷 on 𝑌, thereby making 𝑍 potentially unhelpful, but not invalid control. This highlights the fact that this "limitation" of the backdoor approach is useful in disregarding controls that are not useful. As we had seen, the same comment applies to the counterfactual approach.

7.5	Notes
We adopt the framework pioneered by J. Pearl in his seminal Biometrika paper [2], with a few minor adaptations to enhance its applicability. First, we emphasize fix interventions over do interventions because fix interventions preserve the original “natural” variable alongside the intervened version, providing a seamless transition to counterfactual DAGs. This preservation enables us to systematically deduce conditional ignorability conditions, such as 𝑌 𝑑  𝐷 𝑆, directly from the graph structure. Fix interventions, as an extension of Pearl’s earlier do-interventions, were formalized by Heckman and Pinto [10] and by Robins and Richardson [9]. Notably, elements of fix inter-ventions also appear in Pearl’s original work [2], particularly in his theoretical analysis and proofs, where he employs graph ma-nipulations known as the do-calculus to explore counterfactual scenarios.
The connection between structural equation models (SEMs) and potential outcomes is encapsulated in what J. Pearl [8] calls the First Law of Causal Inference. This principle asserts that SEMs fully induce potential outcomes, highlighting that no information is lost by beginning causal analysis with SEMs rather than potential outcomes directly. In fact, starting with an SEM offers a distinct advantage: it allows us to mathematically
 


articulate contextual assumptions and derive conditional ignor-ability, rather than simply assuming it upfront as is common in potential outcomes frameworks. For instance, in empirical 401(k) analyses, researchers often claim that potential outcomes are independent of 401(k) eligibility given worker characteristics. However, constructing a comprehensive DAG that captures the full context reveals additional factors—such as firm characteris-tics—that must also be conditioned on to ensure ignorability. Thus, DAGs serve as a powerful tool to uncover critical details that might be overlooked in a less structured potential outcomes approach, grounding causal inference in a clearer and more explicit model of the data-generating process.
The uses of DAGs in empirical work are very common in epi-demiology, see e.g. [11] for a review, common in theoretical work in computer science, and is much less common in em-pirical economics, despite the first use dating back to 1928 in the foundational work of Philip Wright [12]; there are recent attempts to revive the interest in economics, see [13] and [14].

7.6	Additional Resources
Dagitty.Net is an excellent online resource for plotting and analyzing causal DAG models. It contains many interesting examples used in empirical analyses across various fields.
Causalfusion.Net is another valuable online resource for explor-ing causal DAG models, covering several deviations from the standard framework.

7.7	Notebooks
Notebook 7.7.1 (DAGs I) R: Dagitty Notebook employs the R package "dagitty" to analyze Pearl’s Example (Figure 11.1) as well as simpler ones. Python: Pgmpy Notebook employs the analogue with Python package "pgmpy" and conducts the same analysis. Both packages automatically list all conditional independence in a DAG; these are obtained by using the graphical d-separation criterion. We then go ahead and test those restrictions assuming a linear ASEM structure. The notebook also illustrates the analysis from the next chapter.

Notebook 7.7.2 (DAGs II) R: Dosearch Notebook employs the R-package "dosearch" to analyze Pearl’s Example (Figure
 


11.1). This package automatically finds identification answers to causal queries, allowing us to also answer these types of queries under different data sources, sample selection, and other deviations from the standard framework. Python: Dosearch Notebook does the same thing by loading the R "dosearch" package into Python.

7.8	Exercises
Exercise 7.8.1 (401(k) Example) Consider the DAG for the 401(k) example, but suppose now that there is an additional arrow from 𝑈 to 𝐷.
1.	Write down the ASEM corresponding to the DAG. State the joint distribution of all variables in the model, exploiting the Markovian structure. What conditional independence restrictions are implied by the model? Are they testable?
2.	Provide the counterfactual DAG (SWIG) for this DAG corresponding to the Fix intervention and state the corresponding ASEM. Using d-separation, determine sufficient adjustment sets for identifying the average causal effect of 𝐷 on 𝑌.
3.	In the factual DAG, list all backdoor paths from 𝐷 to
𝑌. Identify sufficient adjustment sets for the average causal effect of 𝐷 on 𝑌 using the "blocking backdoor paths" approach.
4.	Now suppose there is no arrow from 𝐹 to 𝑀, meaning the match amount does not statistically depend on firm characteristics. Determine the sufficient adjustment sets using either the counterfactual approach or the "blocking backdoor paths" approach. Which approach do you find easier to use?

Exercise 7.8.2 (Pearl’s Example) Consider the DAG in Figure
11.1. Answer the following questions. The best way to answer these question is to use computational packages (but please explain the principles the package is using).
1.	What are the testable implications of the assumptions embedded in the model? Hint: The testable implications are derived from the d-separation criterion.
2.	Assume that only variables 𝐷, 𝑌, 𝑋2 and 𝑀 are mea-sured, are there any testable implications?
3.	Now assume only 𝐷, 𝑌, and 𝑋2 are measured. Are
 


there any testable implications?
4.	Now assume that all of the variables but 𝑋2 (7 in total) are measured. Are there any testable restrictions?
5.	Assume that an alternative model, competing with Model 1, has the same structure, but with the 𝑋2  𝐷 arrow reversed. What statistical test would distinguish between the two models?

Exercise 7.8.3 Work through the proof that d-separation implies conditional independence in Section 7.B. Supply the steps of the proof that were left as a homework or reading exercise.

7.A	Review of Conditional Independence
The following lemma reviews various ways in which conditional independence can be established.

As a reading exercise prove the equivalence of (1) and (2), of (1) and (7), and of any other pair.

7.B	Theoretical Details of d-Separation★

 
Here we explain why d-separation implies conditional indepen-dence.15
 

15: We follow the proof sketch pre-sented in Nevin L. Zhang’s lecture notes, but rely on ASEMs to sim-plify some arguments and supply a proof for a key claim.
 





 
Proof. Let Z1 be the set of nodes in Z that have parents in X. And let Z2 = Z\Z1.
Because Z d-separates X and Y, we have that (see Figure 7.9):
▶ For any 𝑊 ∈ X ∪ Z1, 𝑃𝑎𝑊 ⊆ X ∪ Z;16
▶ For any 𝑊 ∈ Y ∪ Z2, 𝑃𝑎𝑊 ⊆ Y ∪ Z.17
Let U denote the set of variables not included in X, Y, or Z. We then obtain a factorization
 





16: Suppose that any such node has a parent in Y. If it were a node in X, then we get a violation of d-separation. If it were a node in Z1, then we have that Z1 has one par-ent in X and one parent in Y and
 
p(x, z, y) = ∫	n
∫ n
 
p(𝑤 | 𝑃𝑎𝑊 = 𝑝𝑎𝑊 )𝑑u
 
therefore it is a collider that was in-cluded in Z, violating d-separation.
17: Suppose that any such node has
 
× 𝑊n∈X∪Z1
× 𝑊n∈Z2∪Y
 

p(𝑤 | 𝑃𝑎𝑊 = 𝑝𝑎𝑊 )
p(𝑤 | 𝑃𝑎𝑊 = 𝑝𝑎𝑊 ),
 

have that a node in Y has a parent in X, violating d-separation.
 
where in the last equality we used the fact that u does not appear at all in the second and third factors, since X Y Z is ancestral. Moreover, the second factor is a function of x and z alone and the third factor is a function of y and z alone. The integral is 1 by total probability.18 It follows that X ⊥ Y | Z.19
Now we restate the main claim we’d like to demonstrate, which is that d-separation implies conditional independence.

Global Markov. Let 𝑋 and 𝑌 be two variables and Z be a set of variables that does not contain 𝑋 or 𝑌. If Z d-separates
𝑋 and 𝑌, then
𝑋 ⊥ 𝑌 | Z

Proof of Theorem 7.3.2.
Let X be the set of all ancestors of	𝑋, 𝑌	Z that are not
d-separated from 𝑋 by Z. Let Y be the set of all ancestors of
{𝑋, 𝑌} ∪ Z that are neither in X nor in Z.
Key Claim: The set Z d-separates the sets X and Y.
The claim follows from the careful use of the definition of d-separation, and is proven below.
 





Figure 7.9: Pictorial representation of key argument in Lemma 7.B.1.
18: Prove this as a reading exercise by integrating over the variables in U in reverse order with respect to the DAG ordering.
19: Prove this as a reading exercise, i.e., prove bullet (7) of Lemma 7.A.1.
 

 
Given the key claim, Lemma 7.B.1 implies that X ⊥ Y | Z, since X ∪ Y ∪ Z is ancestral by its exhaustive construction. This implies that there must exist functions 𝑓 (x, z) and 𝑔(z, y) such that
p(x, z, y) = 𝑓 (x, z)𝑔(z, y).
Since 𝑋 is in X and 𝑌 in Y, the conclusion is reached.20    
Proof of the Key Claim. Suppose that Z does not d-separate the sets X and Y and that there exists a node 𝑋′  X which is not d-separated from some node 𝑌′  Y. Thus, there is an open path 𝑋 - - 𝑋′,21 and an open path 𝑋′ - - 𝑌′. Consider the concatenation of these two paths. If 𝑋′ is not a collider on this concatenated path, then the path 𝑋 - - 𝑋′ - - 𝑌′ is also open, and therefore 𝑋 is not d-separated from 𝑌′, which is in contradiction with the definition of X and Y. Thus 𝑋′ has to be a collider on this concatenated path. Moreover, note that since we are only restricting our analysis to the ancestral set 𝐴𝑛{𝑋,𝑌}∪Z, we have that 𝑋′ must be an ancestor of either Z or 𝑌 or 𝑋:
If 𝑋′ is an ancestor of some node in Z then the path 𝑋 - - 𝑋′ - - 𝑌′ is again open, leading to a contradiction with the definition of X and Y.
If 𝑋′ is an ancestor of 𝑌, then there is a directed path 𝑋′ ---t 𝑌. If that path is open, then there is an open path 𝑋 - - 𝑋′ ---t 𝑌, violating the fact that Z was d-separating 𝑋 from 𝑌. For the path to be closed, it must be that some node 𝑍  Z is on the path. However, in this case 𝑋′ is an ancestor of a node in Z, which has already been excluded.
Finally, if 𝑋′ is an ancestor of 𝑋, then there exists a directed path 𝑋′ ---t 𝑋. This path also has to be open, as if a node in Z existed on that path, then 𝑋′ would be an ancestor of a node in Z, which has been excluded. However, in this case, we have an open path 𝑌′ - - 𝑋′ ---t 𝑋, from 𝑌′ to 𝑋, which violates the definition of X and Y.
 







20: Prove this explicitly, as a read-ing exercise, by integrating over all variables in X 𝑋 and Y 𝑌 and invoking Lemma 7.A.1.

21: In this proof, we denote with
𝑈 - - 𝑉 a path from a node 𝑈 to a node 𝑉 and with 𝑈 ---t 𝑉 a di-rected path from 𝑈 to 𝑉.
 

7.C	Faithfulness and Causal Discovery
Given that DAGs effectively encode conditional independence relations, it is tempting to try to infer conditional independence directly from the data. Causal discovery refers to methods that indeed attempt to learn conditional independence relationships from data with one application being attempting to recover causal structures. The possibility of recovering causal structures
 


perfectly from the population data critically relies on the concept of faithfulness.
Recall that d-separation implies conditional independence, but the reverse implication
𝑌 ⊥ 𝑋 | 𝑆 =⇒ (𝑌 ⊥𝑑 𝑋 | 𝑆)G	(7.C.1)
is not true in general. If we restrict attention to the set of distributions 𝑝 of random variables associated with graph G such that implication (7.C.1) holds, we are said to impose the faithfulness assumption on p.

The observation about the simple example above generalizes: If probabilities p themselves are viewed as generated by Nature as a draw from a continuum P, where each p P factorizes according to G, then the set of models where the reverse impli-cation (7.C.1) does not hold has measure zero. This observation motivates the argument that the faithfulness assumption is a weak requirement; that is, a given p is "very unlikely" to be unfaithful.

Remark 7.C.1 (Causal Discovery) The use of the faithfulness
assumption should allow us to discover the equivalence class
of the true DAG from the population distribution p: We
can compute all valid conditional independence relations
and then discover the equivalence class of DAGs. See, for
example, the PC algorithm [15] for an explicit causal discovery
algorithm and the review provided in [16]. We can then apply
contextual knowledge to further orient the edges of the graph.
Even though the set of unfaithful distributions has measure zero, the neighborhood of this set may not be small in high-
 


 
Figure 7.10: Uhler et. al [17]: A set of "unfaithful" distributions p in the simple triangular Gaussian SEM/DAG:
𝑋1 → 𝑋2, (𝑋1, 𝑋2) → 𝑋3.

dimensional graphs, which creates difficulty in inferring the DAG structure from an estimated version pˆ.













 




Thus, it is hard to distinguish exact independence from ap-proximate independence with finite data. In high-dimensional graphs, the possibility that 𝑝 lands in the "near-unfaithful" re-gions can be substantial, as Uhler et. al.[17]’s analysis shows.22

The observations above motivate a form of sensitivity analysis – e.g., Conley et al. [18] – where one replaces exact exclusion restrictions by approximate exclusion restrictions that can’t be distinguished from exact exclusion restrictions and examines the sensitivity of causal effect estimates.
 
22: See Uhler et al’s [17] figure; re-produced in Figure 7.10. The set is parameterized in terms of the co-variance of 𝑋1, 𝑋2, 𝑋3 . The right panel shows the set of unfaithful distributions, and the three other panels show 3 of 6 components of the set. Each of the cases cor-responds to the non-generic case which would make faithfulness fail, leading to discovery of the wrong DAG structure. While the exact set-ting where faithfulness would fail is non-generic, there are many dis-tributions that are "close" to these unfaithful distributions. This obser-vation means that, in finite samples, we are not able distinguish models that are close to the set of unfaithful distributions from unfaithful distri-butions and may thus also discover the wrong DAG structure and cor-respondingly draw incorrect causal conclusions.
 


Bibliography



[1]	Judea Pearl and Dana Mackenzie. The Book of Why. Pen-guin Books, 2019 (cited on page 172).
[2]	Judea Pearl. ‘Causal diagrams for empirical research’. In:
Biometrika 82.4 (1995), pp. 669–688 (cited on pages 173,
179, 189, 190).
[3]	Trygve Haavelmo. ‘The probability approach in econo-metrics’. In: Econometrica 12 (1944), pp. iii–vi+1–115 (cited on page 173).
[4]	James M. Poterba, Steven F. Venti, and David A. Wise. ‘Do 401(k) Contributions Crowd Out Other Personal Saving?’ In: Journal of Public Economics 58.1 (1995), pp. 1–32 (cited on page 175).
[5]		Victor Chernozhukov, Carlos Cinelli, Whitney Newey, Amit Sharma, and Vasilis Syrgkanis. ‘Long Story Short: Omitted Variable Bias in Causal Machine Learning’. In: arXiv preprint arXiv:2112.13398 (2023) (cited on page 179).
[6]		Thomas Verma and Judea Pearl. Influence diagrams and d-separation. Tech. rep. Cognitive Systems Laboratory, Computer Science Department, UCLA, 1988 (cited on page 183).
[7]		Rajen D. Shah and Jonas Peters. ‘The hardness of condi-tional independence testing and the generalised covari-ance measure’. In: Annals of Statistics 48.3 (2020), pp. 1514–1538 (cited on page 184).
[8]		Judea Pearl. Causality. Cambridge University Press, 2009 (cited on pages 185, 189, 190).
[9]	Thomas S. Richardson and James M. Robins. Single world intervention graphs (SWIGs): A unification of the counterfac-tual and graphical approaches to causality. Working Paper No. 128, Center for the Statistics and the Social Sciences, University of Washington. 2013. url: https://csss.uw. edu/files/working- papers/2013/wp128.pdf (cited on pages 187, 190).
[10]	James Heckman and Rodrigo Pinto. ‘Causal analysis after Haavelmo’. In: Econometric Theory 31.1 (2015 (NBER 2013)), pp. 115–151 (cited on page 190).
 

 
Bibliography
 
199
 


[11]	Peter WG Tennant, Eleanor J Murray, Kellyn F Arnold, Laurie Berrie, Matthew P Fox, Sarah C Gadd, Wendy J Harrison, Claire Keeble, Lynsie R Ranker, Johannes Tex-tor, et al. ‘Use of directed acyclic graphs (DAGs) to identify confounders in applied health research: review and rec-ommendations’. In: International journal of epidemiology
50.2 (2021), pp. 620–632 (cited on page 191).
[12]	Philip G. Wright. The Tariff on Animal and Vegetable Oils. New York: The Macmillan company, 1928 (cited on page 191).
[13]	Paul Hünermund and Elias Bareinboim. ‘Causal inference and data fusion in econometrics’. In: The Econometrics Journal (2023), utad008 (cited on page 191).
[14]		Jaap H Abbring, Victor Chernozhukov, and Iván Fernández-Val. ‘Philip G. Wright, directed acyclic graphs, and instru-mental variables’. In: Econometrics Journal (2025) (cited on page 191).
[15]	Peter Spirtes, Clark N. Glymour, Richard Scheines, and David Heckerman. Causation, Prediction, and Search. MIT Press, 2000 (cited on page 196).
[16]		Clark Glymour, Kun Zhang, and Peter Spirtes. ‘Review of causal discovery methods based on graphical models’. In: Frontiers in Genetics 10 (2019), p. 524 (cited on page 196).
[17]	Caroline Uhler, Garvesh Raskutti, Peter Bühlmann, and Bin Yu. ‘Geometry of the faithfulness assumption in causal inference’. In: Annals of Statistics 41.2 (2013), pp. 436–463 (cited on page 197).
[18]		Timothy G. Conley, Christian B. Hansen, and Peter E. Rossi. ‘Plausibly exogenous’. In: Review of Economics and Statistics 94.1 (2012), pp. 260–272 (cited on page 197).
 

 

Predictive Inference via Modern
Nonlinear Regression


"Nowhere is it written on a stone tablet what kind of model should be used to solve problems involving data."
– Leo Breiman [1].
Here we discuss nonlinear regression methods based on tree models and (deep) neural network models. Tree-based methods include regression trees, random forests, and boosted trees. Regression trees are great for exploration and explainable analytics, while random forests and boosted trees are great predictive tools for structured data and data sets of intermediate size (say, up to several million observations). Neural networks are extremely flexible nonlinear regression methods and are particularly successful for data sets of larger size.
 
8
8.1	Introduction	201
8.2	Regression Trees and Ran-dom Forests	201
Introduction to Regression Trees	201
Random Forests	205
Boosted Trees	207
8.3	Neural Nets / Deep Learn-ing	208
Basic Ideas	209
Deep Neural Networks 213
8.4	Prediction Quality of Modern Nonlinear Regression Methods	216
Learning Guarantees of Trees and Forests	217
Learning Guarantees of DNNs	220
The Need for More and Bet-ter Theory	222
Trust but Verify	223
A Simple Case Study using Wage Data	224
8.5	Combining Predictions - Ag-gregation - Ensemble Learn-ing	225
Auto ML Frameworks	227
8.6	When Do Neural Networks Win?	228
8.7	Notes	229
8.8	Checklist	230
8.9	Additional resources . 230 8.10Notebooks	231
8.11 Exercises	231
8.A Variable Importance via Per-mutations	232
 
8.1	Introduction
We are interested in predicting an outcome 𝑌 using raw regres-sors 𝑍, which are 𝑑-dimensional. The best prediction rule 𝑔 𝑍 under square loss is the conditional expectation function (CEF) of 𝑌 given 𝑍:
𝑔(𝑍) = E(𝑌 | 𝑍).
In previous chapters, we used best linear prediction rules to approximate 𝑔 𝑍 and linear regression or Lasso regression for estimation. Now we consider nonlinear prediction rules to approximate 𝑔 𝑍 , focusing on tree-based methods and neural networks.
The use of best prediction rules (CEFs) is not just important for generating good predictions but is crucial for causal inference. Identification of causal parameters such as ATEs via condi-tioning strategies requires us to work with CEFs rather than with best linear prediction rules. Previously we tried to make best linear prediction rules flexible to try to approximate best prediction rules. Here we explore fully nonlinear strategies.

8.2	Regression Trees and Random Forests
Introduction to Regression Trees
Regression trees are based on partitioning the regressor space (the space where 𝑍 takes on values) into a set of rectangles. A simple model is then fit within each rectangle.
The most common approach fits a simple constant model within each rectangle, which corresponds to approximating the un-known function by a "step function." Given a partition into 𝑀 regions, denoted 𝑅1, . . . , 𝑅𝑀 the approximating function when a constant is fit within each rectangle is given by

𝑀
𝑓 𝑧 =	𝛽𝑚1 𝑧	𝑅𝑚 ,
𝑚=1
where 𝛽𝑚 , 𝑚 = 1, . . . , 𝑀 denotes a constant for each region and 1(·) denotes the indicator function.
Suppose we have 𝑛 observations 𝑍𝑖 , 𝑌𝑖 for 𝑖 = 1, . . . , 𝑛. The estimated coefficients for a given partition are obtained by
 


minimizing the in-sample MSE:
J	I:	\2

which results in
𝛽ˆ𝑚 = average of 𝑌𝑖 where 𝑍𝑖 ∈ 𝑅𝑚.
The regions 𝑅1, . . . , 𝑅𝑀 are called nodes, and each node 𝑅𝑚 has a predicted value 𝛽ˆ𝑚 associated with it. Nodes that are split
further are called internal nodes; nodes with no further splits are called terminal nodes or leaves. A regression tree predicts
𝑔 𝑧 by sending 𝑧 down the tree to a leaf and returning the corresponding leaf mean.
A nice feature of regression trees is that you get to draw cool pictures, so let’s explore their usage graphically in the context of our wage example. In this example, the outcome variable 𝑌 is (log) hourly wage; and 𝑍 includes experience, geographic, and educational characteristics.
Figure 8.1 illustrates a simple regression tree for the wage data. This tree has a depth of two, meaning that predictions are produced as a sequence of two binary decisions (or partitions of the data). Starting at the top of the tree and working down provides a simple prediction rule for any observation. For example, the predicted wage for a worker without a college degree (college = 0) and with less than 14 years of experience (exper < 14) is 12 dollars an hour. We obtain this prediction by starting at the top of the tree and taking the left branch because college = 0 < .5. We then go left again at the second step because exper < 14 and arrive at the predicted value of 12.


 








The key feature of trees is that the cut points for the partitions are adaptively chosen based on the data. That is, the splits are
 
Figure 8.1: Regression tree based on wage data. The bottom nodes on the tree provide prediction rules for different subsets of observations. For example, the predicted hourly wage for a college educated worker with 9.5 or more years of experience (a worker with college = 1 and exper ≥ 9.5) is 24 dollars.
 


 
not pre-specified but are purely data dependent. So, how did we use the data to grow the tree in Figure 8.1?
To make computation tractable, we use recursive binary parti-tioning or splitting of the regressor space:
▶ Growing the Tree: Level 1. First, we cut the regressor space into two regions by choosing the regressor and splitting point such that using the prediction rule fit within each region produces the best improvement in the in-sample MSE. Concretely, suppose the current node contains the observations indexed by 𝐼. For each candidate split consisting of a coordinate 𝑗  1, . . . , 𝑑 and a split point 𝑠, define the left and right child nodes:
𝐿(𝑗, 𝑠) = {𝑖 ∈ 𝐼 : 𝑍𝑖𝑗 ≤ 𝑠},    𝑅(𝑗, 𝑠) = {𝑖 ∈ 𝐼 : 𝑍𝑖𝑗 > 𝑠},
and let 𝑌¯𝐿 and 𝑌¯𝑅 be the sample means of 𝑌𝑖 in the two child nodes. We choose 𝑗, 𝑠 to minimize the within-node sum of squared errors1
SSE(𝑗, 𝑠) = I: (𝑌𝑖 − 𝑌¯𝐿)2 + I: (𝑌𝑖 − 𝑌¯𝑅)2.
 

























1: To be clear, note that, in prin-ciple, finding this split point re-quires trying the partition pro-
 
𝑖∈𝐿(𝑗,𝑠)
 
𝑖∈𝑅(𝑗,𝑠)
 
duced by splitting the data along every possible value of every ob-served variable. That is, we are nei-
 
Applying this procedure in the wage data gives us the
depth 1 tree shown Figure 8.2. In this case, the best regres-sor to split on is the indicator of college degree, that takes values 0 or 1. Here splitting at any point between 0 and 1 provides the same rule, and an often used convention for binary variables is to use the "natural" split point of .5. Applying this split point yields the initial prediction rule: an hourly wage of $20 for college graduates and $13 for others.
▶ Growing the Tree: Level 2. To grow the tree to depth 2, we then repeat the procedure for choosing the first partition rule within the two regions resulting from the first step. This step will result in a partition of the covari-ate space into four new regions. It is important to note that the two splits produced at this point may use differ-ent variables/splitting points than before. This feature means that the tree algorithm can create "interactions" and "nonlinearities" without requiring input from the user.
In our example, the regions resulting from applying the first splitting rule correspond to college graduates and non-college graduates). For college graduates, the parti-tioning rule that minimizes in-sample MSE is to split this
 
ther pre-specifying which variables nor which split points are impor-tant in providing a good prediction rule.

 
Figure 8.2: Depth 1 tree in the wage example










Figure 8.3: Depth 2 tree in the wage example
 


 
group into those with less than 9.5 years of experience and those with 9.5 years or more of experience. We have thus refined the prediction rule for graduates to be $24 an hour if experience is greater than or equal to 9.5 years, and $17 an hour otherwise. For non-graduates the pro-cedure works similarly, though here the in-sample MSE minimizing split is produced by dividing non-graduates into those with less than 14 years of experience and those with 14 years of experience or more.
▶ Growing the Tree: Higher Levels and Stopping Rule. To grow deeper trees corresponding to more complex prediction rules, we simply keep repeating. We stop when the desired depth of the tree is reached,2 or when a prespecified minimal number of observations per region, called minimal node size, is reached.
In the wage example, we can grow a depth 3 tree by repeating the basic procedure within each of the four nodes of the depth 2 tree. The resulting tree is illustrated in Figure 8.4. Here, we see that the indicator for self-reported sex (female), high-school graduate indicator (hsg), and Southern region indicator (so) are the splitting variables chosen in the third level.
 















2: One practical choice of the depth of a tree is to stop just before we get a headache from looking at a com-plicated tree. This rule is indeed useful if we want to present the tree as a communication device.
 







Figure 8.4: Depth 3 tree in the wage example. A depth of three is shown for readability; deeper trees are pos-sible but less interpretable.

Pruning Regression Trees. We now make several observa-tions.
First, the deeper we grow the tree, the better is our approxi-mation to the regression function 𝑔 𝑍 . However, the deeper the tree, the noisier our estimate 𝑔 𝑍 becomes, since there are fewer observations per terminal node to estimate the predicted value for this node. From a prediction point of view, we can try to find the right depth or the structure of the tree by a validation exercise such as using a single train/test split or cross-validation. For example, in the wage example, the tree of depth 2 performs better in terms of cross-validated MSE than the tree of depth 3 or 1. The process of cutting down the branches of the tree to improve predictive performance is called
 


 
"Pruning the Tree."
Often for business analytics and explainability, simple trees like the ones shown are used. If we only care about prediction, a common workflow is to first grow a fairly large tree and then prune it back to the size that performs best out-of-sample. A standard pruning criterion is the cost-complexity criterion, which trades off training fit against the number of leaves. If 𝑇 denotes a tree and L(𝑇) its set of leaves, define the training fit
 

 
Figure 8.5: "To prune a tree." Source: Wikipedia
 
𝑅(𝑇) := I:	I: (𝑌𝑖 − 𝑌¯ℓ )2 ,
ℓ ∈L(𝑇) 𝑖:𝑍𝑖 ∈𝑅ℓ
where 𝑌¯ℓ is the average outcome among observations that fall in leaf ℓ . Cost-complexity pruning chooses the subtree 𝑇′ that minimizes
 
𝑅𝛼(𝑇′) := 𝑅(𝑇′) + 𝛼 |L(𝑇′)|.
Here 𝛼 0 is a tuning parameter: larger 𝛼 favors smaller (and more interpretable) trees. In practice one computes a nested sequence of pruned subtrees and chooses 𝛼 by cross-validation, often using a “one-standard-error” rule to lean toward simpler trees; see, e.g., [2]. Unlike Lasso, there is no widely used plug-in choice of 𝛼 that works uniformly well; validation is the workhorse.

Random Forests
In practice, regression trees often do not provide the best pre-dictive performance, because a single regression tree provides a relatively crude approximation to a smooth regression func-tion g(Z). We illustrate the potential poor approximation of regression trees in Figures 8.6 and 8.7. These figures simply illustrate that step functions, which are the outputs of typical regression tree implementations, struggle in approximating smooth functions.
A powerful and widely used approach that aims to improve upon simple regression trees is to build a random forest, as proposed by Leo Breiman [3]. The idea of a random forest is to grow many different deep trees that have low approximation error and then average the prediction rules across trees.
To produce different trees using only the observed data, the trees going into a random forest are grown from artificial data generated by sampling randomly with replacement from the original data; that is, each tree in a random forest is fit to a bootstrap sample.3 Within the bootstrap samples, trees are grown
 

 
Figure  8.6:  Approximation  of
𝑔 𝑍 = exp 4𝑍 by a shallow re-gression tree in the noiseless case.


Figure  8.7:  Approximation  of
𝑔 𝑍 = exp 4𝑍 by a deep regres-sion tree in the noiseless case.
3: bootstrap sample: typically a sam-ple of the same or similar size to the size of the original dataset pro-duced by sampling uniformly from the original data with replacement. Other sampling schemes may also be used, e.g. to accommodate de-pendence.
 


 
deep to keep approximation error low. Averaging across the trees produced in the bootstrap samples is then meant to reduce the noisiness of the individual trees. The procedure of averaging noisy prediction rules over bootstrap samples is called Bootstrap Aggregation or Bagging. When the data set is large, we can also rely on fitting trees within subsamples4 instead of using the bootstrap. Using subsamples offers some computational advantages and also simplifies theoretical analysis.
The idea seems very unusual, so let us explain again.

Each bootstrap sample is created by sampling from our data on pairs (𝑌𝑖 , 𝑍𝑖) randomly, with replacement. Hence, some observations are drawn multiple times and some aren’t redrawn at all. Given a bootstrap sample, indexed by 𝑏, we build a tree-based prediction rule 𝑔ˆ𝑏(𝑍). We repeat the procedure 𝐵 times in total, and then average the prediction rules that result from each of the bootstrap samples:
1  𝐵
𝑔ˆrandom forest(𝑍) = 𝐵 I: 𝑔ˆ𝑏 (𝑍).
𝑏=1

The use of the bootstrap here is unusual, yet corresponds to an intuitive idea: If we could have many independent copies of the data, we could obtain low-bias but potentially very noisy prediction rules in each copy of the data and then average the prediction rules obtained over these copies to reduce the noise. Since we don’t have many copies in reality, we rely on the bootstrap to create many quasi-copies of the data. Another feature of this idea is that the cut-points defining partitions for the tree obtained within each bootstrap sample will be different, producing a different step function approximation. Averaging over many step functions with steps at different locations will potentially produce a much smoother approximation to the underlying function. The improved approximation relative to simple trees is illustrated in Figure 8.8.
There are many refinements of the simple bootstrap aggre-gation idea we described above. Bagging refers to averaging trees grown on bootstrap (or subsample) data. A random forest adds a second source of randomness designed to “decorrelate” the trees: at each node, rather than searching over all 𝑑 raw regressors for the best split, we first draw a random subset of regressors of some prespecified size (often called mtry) and search only within this subset. Bagging reduces variance by
 






4: subsample: typically a sample of size much smaller than the origi-nal dataset produced by sampling uniformly from the original data without replacement. Other sam-pling schemes may also be used,
e.g. to accommodate dependence.














Figure  8.8:  Approximation  of
𝑔 𝑍 = exp 4𝑍 by a random forest in the noiseless case.
 


averaging; random feature subsampling further reduces vari-ance by lowering the correlation across trees, because different trees are encouraged to use different variables near the top and throughout the tree.
In summary, a random forest is an average of tree based pre-diction rules (a forest) produced from bootstrap or subsample data (generated randomly).

 
Boosted Trees
The idea of boosting is that of recursive fitting: We estimate a simple prediction rule, then take the residuals5 and estimate another simple prediction rule for these residuals. We then take the residuals produced from this new prediction rule and build yet another simple model to predict them. We keep repeating this process until we reach some stopping criterion. The sum of these prediction rules fitted at each step then gives us the overall prediction rule for the outcome.
Boosting can be applied with any type of base prediction rule. A common use of boosting is with regression trees which leads to boosted trees. Boosted trees are built up using shallow trees as the simple prediction rule. Shallow trees are trees with very few levels of depth. By keeping depth low, shallow trees produce low noise prediction rules. However, shallow trees also tend to have high approximation error because they rely on step functions with very few steps to approximate the target regression function. That is, a single shallow regression tree tends to produce a high bias, low variance prediction rule. Boosting then helps alleviate the bias of shallow regression trees. At each step, fitting a model to the residuals from the previous step reduces the approximation error from the previous step. The improved approximation of boosted trees relative to simple trees is illustrated in Figure 8.9.
 




5: residuals: the unexplained part of an outcome we want to predict, af-ter subtracting the prediction from the observed outcome.




















Figure  8.9:  Approximation  of
𝑔 𝑍 = exp 4𝑍 by boosted trees in the noiseless case with a suffi-cient number of steps 𝐽.
 



3. Output 𝑔ˆ𝐽 (equivalently, 𝑔ˆ𝐽 (𝑧) = 𝑌¯ + '5:𝐽	𝜆ℎˆ𝑗 (𝑧)).
𝑗=1

 
In practice, using boosted trees requires making several choices. One needs to define the tree-based prediction rule used at each step and also choose the number of learning steps, 𝐽, and the learning rate, 𝜆. These tuning parameters are typically chosen by cross-validation.6
Note that the boosting algorithm is quite general and can be ap-plied to non-tree uses. Note that the number of learning steps for boosting is important across any implementation. Because each step is building a model to predict the unexplained part of the outcome from the previous step, the in-sample prediction errors – the fit to the outcomes used to train the model – must weakly improve with each additional step (equivalently, the in-sample prediction errors must weakly decrease). If too many iterations are taken, it is thus likely that overfitting will occur, but too few iterations may leave significant bias in the final prediction rule. In practice, the number of iterations is typically chosen by stopping the procedure once there is no marginal improvement to cross-validated MSE. A very popular implementation widely used in industry is xgboost, which has the capability to impose qualitative shape constraints like monotonicity in one or several variables. Other frequently used implementations are lightgbm and catboost. Across these packages, the main tuning knobs are the depth (or number of leaves) of the base trees, the number of boosting iterations 𝐽, the learning rate 𝜆, and regularization devices such as row/column subsampling and early stopping based on validation performance.

8.3	Neural Nets / Deep Learning
Neural networks are a very powerful tool for modelling nonlin-ear relationships. They rely on many constructed regressors to approximate 𝑔 𝑍 , the conditional expectation given the raw regressors. The method and the name "neural networks" were loosely inspired by the mode of operation of the human brain, and developed by scientists working in Artificial Intelligence. They can be represented by cool graphs and diagrams.
 





6: We need 0 < 𝜆  1, and a com-mon default value for 𝜆 is 0.1. The idea of boosting is to fit simple pre-diction rules, so one will typically specify the prediction rule by set-ting the depth of the trees to a small number. For example, at each step, the prediction rule may be a regres-sion tree of depth one (so-called stumps) or depth two. Typically, one will try several small values for depth and again choose among them by cross-validation.
 


Basic Ideas
First, we focus on a single layer neural network to introduce the more formal definition of neural nets. In the neural network literature, the raw regressors 𝑍 are often called the inputs. The estimated prediction rule will take the form:

𝑀
𝑔 𝑍 := 𝛽′𝑋 𝛼 :=	𝛽𝑚 𝑋𝑚 𝛼𝑚 ,
𝑚=1
where the 𝑋𝑚(𝛼ˆ𝑚)’s are constructed regressors called neurons,
𝛼 = (𝛼𝑚)𝑀  ,	𝛽 = (𝛽𝑚)𝑀  ,	𝑋(𝛼) = (𝑋𝑚(𝛼𝑚))𝑀   .
We always take 𝑍 to include a constant of 1 as a component and set 𝑋1(𝛼) = 1. The remaining neurons are generated as
𝑋𝑚(𝛼𝑚) = 𝜎(𝛼′𝑚 𝑍),  𝑚 = 2, . . . , 𝑀,
where 𝛼𝑚’s are neuron-specific vectors of parameters called weights, and 𝜎 is an activation function chosen by the practi-tioner. Popular activation functions are
▶ the sigmoid function,
1
𝜎(𝑣) = 1 + 𝑒−𝑣 ,
▶ the rectified linear unit function (ReLU),
𝜎(𝑣) = max(0, 𝑣),
▶ the softplus function (a smooth approximation to ReLU),
𝜎(𝑣) = log(1 + exp(𝑣)),
▶ the leaky rectified linear unit function (Leaky-ReLU),
𝜎(𝑣) = 𝑐 𝑣 1(𝑣 < 0) + 𝑣 1(𝑣 ≥ 0)
▶ or the linear function,
𝜎(𝑣) = 𝑣.
The use of nonlinear activation functions is critical for generating high-quality approximations.
The estimators 𝛼𝑚 and 𝛽ˆ𝑚 , for 𝑚 = 1, ..., 𝑀, are obtained as the solution to a penalized nonlinear least squares problem.
 




















Figure 8.10: The sigmoid (logit) and ReLU activation functions

For example, we could obtain parameter estimates by solving
 

min
{𝛼𝑚 },{𝛽𝑚 }
 
I:𝑖  J
 

𝑌𝑖 −
 
𝑚I:=1
 
2
𝛽𝑚 𝑋𝑖𝑚(𝛼𝑚)
 

+ pen(𝛼, 𝛽; 𝜆),	(8.3.1)
 
where pen 𝛼, 𝛽; 𝜆 is a penalty function with penalty parameter
𝜆. Common penalty functions are Lasso-type ℓ1 penalties,
𝜆 JI: I: |𝛼𝑚𝑗 | + I: |𝛽𝑚 |\ ,
 	 	 

 
and Ridge-type ℓ2 penalties,7
JI: I:	2	I:	2\
 
7: In many implementations of neural network training the ℓ2 penalty is referred to as the "weight
 
𝜆
𝑚	𝑗
 
(𝛼𝑚𝑗) +
 
𝛽𝑚	.
𝑚
 
decay" parameter; inspired by the fact that the ℓ2 penalty adds an extra
−2𝜆𝑤 term in the gradient calcu-
 
Neural network estimates are typically computed using stochas-tic gradient descent (SGD) algorithms. In its simplest version, SGD proceeds as follows: At each step, parameters are updated based on the update formula
(𝛼, 𝛽) ← (𝛼, 𝛽) − 𝜂∇𝛼,𝛽 Loss(𝐵; 𝛼, 𝛽)
where 𝐵	1, . . . , 𝑛 is a subset of the observations and the loss is the penalized non-linear least squares objective in
 
lated at each gradient step of SGD – see below – for each parameter 𝑤, with 𝑤 being the parameter’s cur-rent value. Thus it always "decays" the parameter towards zero.
 


Equation (8.3.1) calculated on the subset 𝐵:
 

Loss(𝐵; 𝛼, 𝛽) :=
 
I:𝑖∈𝐵 J
 

𝑌𝑖 −
 
𝑚I:=1
 
2
𝛽𝑚 𝑋𝑖𝑚(𝛼𝑚)
 

+ pen(𝛼, 𝛽; 𝜆).
 
In other words, every time we take a small step in the direc-tion opposite to an approximate (or stochastic) version of the gradient of the loss that we want to minimize. The gradient designates the direction of the parameters in which the loss increases the most, and the opposite is the direction in which the loss decreases the most. In neural networks, the gradients needed for SGD are computed efficiently by the backpropaga-tion algorithm.8 The magnitude of the step is controlled by the parameter 𝜂, which is many times referred to as the step-size.
In SGD, gradients are computed on subsamples of data (often consisting of a single observation) called batches, and a single cycle through all subsamples is termed an "epoch." By only mak-ing use of batches of observations, SGD algorithms are able to scale to massive data sets. Using subsamples of data introduces "stochasticity" relative to using the "full" gradient computed on the entire data. In addition to the computational advantages SGD enjoys in large data sets, the presence of this noise in the computation of gradients also seems to have advantages in helping SGD algorithms avoid local saddle points. There are many fine practical details in terms of efficient computation of gradients for deep neural nets, how updating is done in SGD algorithms in general, and in the application of SGD to learning parameters of deep neural nets.9
The optimization methods employed for learning neural net-work parameters provide avenues for regularization beyond simply penalizing the size of the coefficients. A popular regu-larization method is dropout regularization where each neuron in a given layer can be set to zero with a given probability – for example, .1 – during parameter update steps. Dropout is used only during training; when we evaluate or deploy the network, dropout is turned off (equivalently, we use the full network, often with a simple rescaling of weights). Dropout encourages more robust networks: If a particular neuron is important, the dropout regularization encourages creation of very similar neurons that can replicate the properties of the given neuron. Therefore, dropout regularization can be viewed as a penalty that forces similar weights for groups of neurons.
Another commonly used regularization device used with neural networks is early stopping. With early stopping, a measure of out-of-sample prediction accuracy is monitored along with the
 









8: This direction is typically re-ferred to as the direction of steepest descent















9: These details are outside of the scope of this text. Interested read-ers might refer to Deep Learning by Goodfellow, Bengio, and Courville
[4] for a textbook treatment of these issues. A popular method for train-ing neural networks is called Adam; see this Towards Data Science blog for a detailed explanation [5].
 


 
value of the in-sample objective function (8.3.1). Rather than optimizing until the in-sample objective function is minimized, optimization proceeds until out-of-sample performance appears to start to degrade. By updating parameters based on in-sample fit but stopping based on out-of-sample performance, early stopping helps guard against overfitting.
As can be seen from the preceding paragraphs, using neural networks in practice relies on the choice of many tuning param-eters. As there is relatively little theoretical guidance on these choices, tuning parameters are typically chosen using validation exercises such as cross-validation or simple train/test splits. An important choice that clearly relates to model flexibility is the number of neurons – the width of the network. We must also choose the number of layers of neurons – the depth of the network – in the deeper networks discussed below. Having more neurons or layers gives us additional flexibility, just like having more constructed regressors provides more flexibility in high-dimensional linear models. Other choices about regu-larization then interact with the choice of how many neurons and layers to use in preventing overfitting.10
To visualize the working of a neural network, we rely on a resource called playground.tensorflow.org [7], with which we produced a prediction regression model using a simple single layer neural network model based on two input variables. A screenshot taken after training the model is shown below.
 
























10: There has been a flurry of re-cent research considering the use of very large neural networks with many more parameters than the number of observations that may easily overfit the data. These papers find that such highly overparam-eterized neural networks tend to find solutions that generalize well,
 

















The network depicts the process of taking raw regressors and transforming them into predicted values. In the second column
 


 
(labeled "FEATURES"), we see the inputs – our two raw regres-sors. The third column depicts a hidden layer made up of eight neurons.11 Each neuron is constructed as a (weighted) linear combination of the raw regressors transformed by an activation function. Here we use the ReLU activation function. The neu-rons are connected to the inputs, and the connections represent the 𝛼𝑚 coefficients. The coloring represents the sign of the coef-ficients – orange is negative and blue positive – and the width of the connections represents the size of the coefficients.
Finally, the neurons are combined linearly to produce the output – the prediction rule. The connections going outwards from the neurons to the output represent the coefficients 𝛽ˆ𝑚 of the linear
combination of the neurons that produce the final output. The coloring and the width again represent the sign and the size of these coefficients.
The output (prediction) is shown here by the "heat" map in the box on the right. On the horizontal and vertical axes we see the values of the two inputs. The color and its intensity in the "heat" map represent the predicted value.
At the top of the screenshot, we also see that we used "L1" for the type of regularization, which corresponds to using the Lasso-type penalty. Here, the penalty level is called the regularization rate and is provided as the last entry in the top line of the screenshot.
In this example, we used a single layer neural network. If we add one or two additional layers of neurons constructed from the previous layer of neurons we get a "deep" network. We illustrate a two-layer network in the following figure.
Prediction methods based on neural networks with several or many layers of neurons are called "deep learning" methods.

Deep Neural Networks
Deep neural networks extend the single-layer construction from the previous subsection by stacking several hidden layers of neurons. Depth refers to the number of hidden layers, while width refers to the number of neurons per layer. When 𝑚 = 1
and 𝑇 = 1, the definition below reduces to our earlier single-
layer network: the hidden layer plays the role of the constructed
regressors 𝑋 𝛼 , and the output is a (possibly regularized) linear combination of these neurons.
We begin with the common single-task case in which we predict a single outcome 𝑌 from raw regressors 𝑍 ∈ ℝ𝑑 (called the inputs
 



11: "Hidden" refers to the fact that these layers are typically not re-ported. However, these layers can be extracted and used as technical regressors for other tasks. We dis-cuss using hidden layers as features in Chapter 10 which deals with fea-ture engineering.
 



 

 
in the neural network literature). We use 𝑖 for observations, ℓ for layers, and 𝑘 for neurons within a layer. The multitask case—predicting 𝑌𝑡 for tasks 𝑡 = 1, . . . , 𝑇—shares the same
hidden layers and attaches a separate output “head” for each
task.12
Let 𝐾0 := 𝑑 and set 𝐻(0) := 𝑍. For each hidden layer ℓ = 1, . . . , 𝑚, let 𝐾ℓ denote the width of the layer and write 𝐻(ℓ) ℝ𝐾ℓ for the vector of neuron outputs. Given a weight matrix
𝑤(ℓ)	ℝ𝐾ℓ ×𝐾ℓ−1 and bias vector 𝑏(ℓ)	ℝ𝐾ℓ , the forward pass is defined recursively by
𝐻(ℓ) := 𝜎ℓ (𝑤(ℓ)𝐻(ℓ−1) + 𝑏(ℓ)) ,	ℓ = 1, . . . , 𝑚,	(8.3.2)
 





12: For example, we might be inter-ested in predicting the price of a product using product characteris-tics across multiple markets or time periods, 𝑡. In treatment effect anal-ysis, we may build a single neural network to predict both the out-come, 𝑌, and the treatment, 𝐷, us-ing other covariates. We could view this as a multitask learning prob-lem where we are interested in two
 
where each activation function 𝜎ℓ acts coordinatewise. In most
 
outputs, 𝑌1 = 𝑌 and 𝑌2 = 𝐷.
 
applications, the same activation function is used for all neurons within a layer; we adopt this convention to keep notation light. Bias terms 𝑏(ℓ) provide the intercepts, so we do not need to include an explicit constant among the inputs. (Including both a constant in 𝑍 and bias terms is harmless but redundant.)
The last hidden layer 𝐻(𝑚) 𝑍 is often best thought of as a learned feature vector (an embedding) extracted from the raw inputs. For a single regression outcome, we typically take the output to be linear in this representation,
𝑔ˆ(𝑍) := (𝑤out)′𝐻(𝑚) + 𝑏out,
although one can optionally apply an output activation 𝜎out
 



 

Figure 8.11: Standard Architecture of a Deep Neural Network. The input is mapped nonlinearly into the first hidden layer of the neurons. The output of this first mapping is then mapped nonlinearly into the second layer. This process is then repeated 𝑚 times. The output of the penultimate layer is finally mapped (linearly or nonlinearly) into the output layer, which can have multiple outputs corresponding to different tasks.

 
(for example, a logistic link in classification problems). In a mul-titask network we instead use task-specific output parameters (𝑤(𝑡), 𝑏(𝑡)) and (possibly task-specific) output activations:
𝑔ˆ𝑡 (𝑍) := 𝜎out,𝑡 ((𝑤(𝑡))′𝐻(𝑚) + 𝑏(𝑡)) ,	𝑡 = 1, . . . , 𝑇.
When the output layer is simple (often linear in regression), most of the model’s flexibility comes from learning the rep-resentation 𝐻(𝑚) 𝑍 ; this representation-learning viewpoint also helps connect deep networks to the embedding-based workflows discussed later in the chapter.
Although we will not use it formally, the recursion in (8.3.2) can also be read as a repeated composition of nonlinear maps. This layered structure has been shown to be an extremely powerful way to generate flexible functional forms and is backed by approximation theory. Good approximations can be achieved by both considering sufficiently many neurons and sufficiently many layers (Yarotsky, 2017 [8]; Schmidt-Hieber, 2020 [9]; Farrell
et al., 2021 [10]; Kidger and Lyons, 2020 [11]). In empirical economic examples, it is common to just use a few hidden layers, while much deeper networks are typically used in image
 












Figure 8.12: Approximation of
𝑔 𝑍 = exp 4𝑍 by a Neural Net-work
 


processing and text applications.
Collect all weights and biases into a parameter vector 𝜂. The
practitioner chooses the architecture—depth 𝑚, widths (𝐾ℓ )𝑚 ,
and activation functions—which determines a class of networks N; training then chooses 𝜂 within this class by minimizing a regularized empirical loss:

𝑇	𝑛
min I: 𝜔𝑡 I:(𝑌𝑡 − 𝑔ˆ𝑡 (𝑍𝑖; 𝜂 )2	pen(𝜂; 𝜆),	(8.3.3)
where 𝜔𝑡 are user-chosen task weights (for a single task we take
𝑇 = 1 and 𝜔1 = 1), and pen 𝜂; 𝜆 is a penalty term such as a Ridge (ℓ2) penalty or aLasso (ℓ1) penalty.
It is also useful to keep in mind how quickly the parameter count can grow. In a fully connected network with linear output heads, the number of free parameters is roughly

𝑚	𝑇
I:(𝐾ℓ 𝐾ℓ−1 + 𝐾ℓ ) + I:(𝐾𝑚 + 1),
so even modest widths can produce models with many more parameters than observations, which is why validation, regu-larization, and early stopping play such a prominent role in practice.

8.4	Prediction Quality of Modern Nonlinear Regression Methods
As we have already mentioned, the best prediction rule for an outcome 𝑌 using raw regressors 𝑍 is the function 𝑔 𝑍 , equal to the conditional expectation of 𝑌 using 𝑍:
𝑔(𝑍) = E[𝑌 | 𝑍].

Modern nonlinear regression methods, when appropriately tuned and under some regularity conditions, provide estimated prediction rules 𝑔ˆ(𝑍) that approximate the best prediction rule
𝑔(𝑍) well.
Theoretical work demonstrates that under appropriate regular-ity conditions and with appropriate choices of tuning parame-ters, the mean squared approximation error of prediction rules produced by modern nonlinear regression methods is small
 


 
once the sample size 𝑛 is sufficiently large, namely, the root mean square approximation error is small13
∥ 𝑔ˆ − 𝑔∥𝐿2(𝑍) := JE𝑍[(𝑔ˆ(𝑍) − 𝑔(𝑍))2] ≈ 0,	as 𝑛 → ∞,
where E𝑍 denotes the expectation taken over 𝑍, holding every-thing else fixed. To deliver these guarantees in high-dimensional settings where the number of raw regressors (features) is large, we rely on structured parsimony assumptions, analogously to the approximate sparsity we used in the case of Lasso.

Learning Guarantees of Trees and Forests
One important property of adaptively built trees is that they are able to identify the relevant dimensions along which the regression function varies. To isolate this type of behavior of trees and forests, we consider a setting where all the regressors are binary, i.e. 𝑍  0, 1 𝑑. Considering all binary regressors is without loss of generality for categorical (discrete-valued) regressors, since each level of the regressor can be coded as a binary indicator.14
Without further assumptions on the regression function 𝑔 :
{0, 1}𝑑 →✓ ℝ, the best convergence rates that one could hope for
 

13: The notation 𝑓 𝐿2 𝑍 denotes the so-called 𝐿2 norm (or the “root mean-square" norm) of a function
𝑓 , that is, ∥ 𝑓 ∥𝐿2 (𝑍) := E𝑍 𝑓 2(𝑍).


















14: Continuous regressors can also be discretized. However, discretiza-tion entails some loss of generality, and approximation properties fol-
 
this rate of convergence can be prohibitively slow.
This “2𝑑” term has a simple interpretation in the binary-regressor case. The function 𝑔 assigns a mean outcome to each of the 2𝑑 possible regressor profiles in 0, 1 𝑑, so without structure we are effectively estimating a table with 2𝑑 cells. If we have 𝑛 observations spread across these cells, the average cell contains only about 𝑛 2𝑑 observations, so the noise in estimating cell means scales like 2𝑑 𝑛 1/2. Trees become useful precisely be-cause they do not try to estimate the full table: they adaptively coarsen it by grouping together regressor profiles that appear to have similar conditional means.
Adaptively built trees are particularly successful when there is only a small subset 𝑆, of size 𝑆 = 𝑟, among the 𝑑 variables that is relevant. Using this principle, we can formulate a nonparametric
analogue of the sparsity assumption that we analyzed in the case of high-dimensional linear regression with Lasso that allows us to improve on the convergence rate obtained without restrictions.
 
formally investigated.
 


 

 
The assumption can probably be relaxed to "approximate" sparsity.15
Observe that, unlike the sparsity assumption we made in the case of high-dimensional penalized linear regression, Assump-tion 8.4.1 imposes no restrictions on the form of the function
𝑓 that takes as input the relevant variables.16 Here, under the
nonparametric sparsity assumption together with several other regularity conditions, we can prove that the mean squared approximation error of shallow regression trees or "honest" and arbitrarily deep regression forests17 scales at a

2𝑟 log(𝑑) log(𝑛)/𝑛
rate. Thus, the convergence rate depends strongly on the spar-sity level 𝑟, while the overall number of regressors 𝑑 enter only logarithmically. Moreover, even if we knew the relevant variables 𝑆, we could not hope for a rate faster than 2𝑟 𝑛 since we make no further assumptions on the function 𝑓 . Thus, not knowing the relevant set of regressors 𝑆 adds an extra multiplicative cost on the achievable rate that only grows loga-rithmically with the number of regressors and the sample size. See [12] for results of similar flavor for variants of regression trees in settings beyond the binary regressor case.
 

15: This relaxation has not been for-mally investigated.



16: The existence of subset 𝑆 of size
𝑆 = 𝑟 where 𝑟  𝑑 does depend on the particular choice of binary
representation in settings where binary variables are constructed from a set of discrete variables. For example, consider a discrete vari-able that encodes 𝑑 different occu-pations. Forming 𝑑 binary indica-tors for each type of occupation is without loss of generality. However, sparsity in this representation with
𝑟  𝑑 requires that most occupa-tions are the same and that there are only 𝑟 occupations that differ from this otherwise common baseline. There are, of course, other ways to represent exactly the same informa-tion. For example, rather than an indicator for each observation, one could construct overlapping indi-cators for groups of observations that one a prior believes are similar. One should remember that deal-ing with general discrete variables is often challenging and requires careful thought.
17: An "honest" training approach makes use of subsampling. See The-orem 8.4.2 and the discussion im-mediately preceding its statement.
 


 
Capping the depth of the regression tree as in Theorem 8.4.1 helps avoid overfitting, since otherwise we could potentially construct binary trees that achieve zero error on the training data and have large error out-of-sample.
An alternative to avoiding overfitting is to use an ensemble approach based on sub-sampled data. To implement an en-semble approach, we train multiple regression trees, each on a random subsample (without replacement) of the original data of size sub < 𝑛 and average the predictions of each of these trees. To formally argue about the approximation error of such sub-sampled forests, we will require the forests to be trained in an "honest" manner.
In our setting, an honest training approach is as follows: When we train a tree on a subsample, we randomly partition the data in half and we use half of the data to find the best splits in a greedy manner, and we use the other half of the data to construct the estimates at each node of the tree. Such subsampled honest forests have been recently popularized by the work of [14]. Subsequent work of [13] showed that honest forests provably adapt to non-parametric sparsity of the regression function.

 


 

 
Learning Guarantees of DNNs
To orient ourselves, it is helpful to begin with a classical bench-mark from nonparametric regression. We say that a function
𝑔 : ℝ𝑑  ℝ is 𝛽-smooth (with integer 𝛽 1) if all partial derivatives of 𝑔 up to order 𝛽 exist, are continuous, and are uniformly bounded.19 If the only information we have about the regression function 𝑔 is that it is 𝛽-smooth in this sense, then there is an unavoidable worst-case tradeoff between sample size, smoothness, and dimension: the best worst-case estimation error cannot converge faster than
𝑛−𝛽/(2𝛽+𝑑),

and in fact the best estimators achieve this rate; see Stone [15]. When 𝑑 is not small, this rate becomes painfully slow. This is one concrete way of seeing the curse of dimensionality: if we treat 𝑔 as an arbitrary smooth function of 𝑑 variables, the data requirement for a given accuracy can grow astronomically with
𝑑.20
The way out is to strengthen what we assume about 𝑔. We already saw, for high-dimensional linear models in Chapter 3, that rates can be dramatically improved when the target is parsimonious in a suitable sense. Deep neural networks can exploit a different kind of parsimony that is inherently nonlinear: the regression function may be complicated as a map ℝ𝑑  ℝ, but it may be built by composing simpler maps, each of which depends only on a few coordinates. Following Schmidt-Hieber [9], we formalize this idea through a compositional structured sparsity assumption.21
 








18: A greedy algorithm is any al-gorithm that follows the problem-solving heuristic of making the lo-cally optimal choice at each stage. In our case, a greedily grown tree optimizes over the name of regres-sor and splitting point that achieve the best one-step improvement in the in-sample MSE at each node.






19: A more general and very com-mon definition allows non-integer
𝛽 (via Hölder classes). We restrict attention to integer 𝛽 to keep nota-
tion light.







20: For example, take 𝛽 = 1 (only a uniformly bounded first deriva-tive) and 𝑑 = 10. The rate suggests we need 𝑛 such that 𝑛−1/12   0.1
in order to reach error 𝜖 = 0.1, i.e.
𝑛 0.1−12 = 1012 observations. If instead 𝛽 = 2, the exponent be-comes 2 2 2 10 = 1 7, so reach-ing 𝜖 = 0.1 requires about 𝑛  107
observations, which is still large but
far more plausible.
 


 
Under this assumption, the learning difficulty is governed not by the ambient input dimension 𝑑, but by the list of parsimony–smoothness pairs
(𝑡𝑗 , 𝛽𝑗),	𝑗 = 0, . . . , 𝑞,
which record, layer by layer, how many variables matter (𝑡𝑗) and how smooth the corresponding components are (𝛽𝑗).
As a simple illustration, consider 𝑔 : ℝ100 → ℝ of the form
𝑔(𝑥1, 𝑥2, . . . , 𝑥100) = 𝑓1( 𝑓01(𝑥3), 𝑓02(𝑥2)) .
Here we can take 𝑔 = 𝑓1  𝑓0 with 𝑑0 = 100 and 𝑑1 = 2, where 𝑓0 maps ℝ100 into ℝ2 by extracting and transforming two coordinates. The first stage is maximally sparse (𝑡0 = 1 for each component of 𝑓0), while the second stage uses both intermediate features (𝑡1 = 2).
We now state the main payoff: if 𝑔 has this compositional sparse structure, then there exist sparse deep networks that estimate
𝑔 at a rate controlled by an intrinsic (or effective) dimension rather than by 𝑑. The result below is a simplified version of Schmidt-Hieber’s theorem, stated in the form that is most useful for rate comparisons.

 


 
This fundamental result is due to Schmidt-Hieber [9]; the com-plete set of regularity conditions and technical details can be found there. See also [10] for related developments.
Returning to the preceding 𝑑 = 100 example, assume that 𝑓01
and 𝑓02 are 𝛽-smooth and that 𝑓1 is also 𝛽-smooth (with 𝛽  2,
say). Then the two relevant parsimony–smoothness pairs are
(𝑡0, 𝛽0) = (1, 𝛽),	(𝑡1, 𝛽1) = (2, 𝛽).
The effective dimension becomes
𝑠 = max (𝑛 1  , 𝑛 2  ) = 𝑛 2  ,
so (ignoring logarithmic factors) the theorem yields the rate

𝑠 = 𝑛−𝛽/(2𝛽+2).
𝑛
When 𝛽 = 2, this is 𝑛−1/3. The bottleneck is the second stage
𝑡1, 𝛽1 , since it produces the larger value of 𝑠; intuitively, it
is the least favorable layer in the composition that governs the overall difficulty. The main message is that, despite the ambient dimension 𝑑 = 100, the learning rate is driven by low-
dimensional pieces of the function, and can therefore be quite
fast when the composition is sparse and sufficiently smooth.

The Need for More and Better Theory
Providing theory for tree-based methods and deep neural networks is currently an active area of research. Available results, such as those provided in Theorems 8.4.1-8.4.3 show that these methods can produce prediction rules that approximate best prediction rules well. However, there are still substantial gaps in our theoretical understanding of these methods.
For example, the rate guarantee for Honest Forests in Theorem
8.4.2	is the same as the rate for shallow trees in Theorem 8.4.1. This theory thus does not shed light on why random forests seem to achieve superior predictive performance over simple trees in many applications. Moreover, practical random forest algorithms tend to work well with default tuning choices. How-ever, the theoretical results require careful alignment of tuning parameters to get good rate guarantees, and this alignment
 


 
often seems inconsistent the latter evidence. The regularity conditions also require the explanatory power of the subset of the covariates that are relevant, 𝑆, to dominate the explanatory power of the irrelevant covariates.22 This condition on signal strength is a sufficient condition, but it may not be necessary for good performance.
Theorem 8.4.3 is an interesting result and helps shed light on the power of neural networks to perform well in nonpara-metric settings with many variables under sensible dimension reducing structure as in Assumption 8.4.2. However, the result applies only to particular network architectures with particular activation functions and requires uniformly bounded weights (parameters). Imposing bounded weights is not typically done in practice as it is computationally challenging, and it is not clear that the other structure imposed in the theoretical construc-tion maps well to the very deep and relatively unconstrained networks that are often employed in practice.
That is, there seem to remain substantial gaps in our theoretical understanding of the performance of tree-based algorithms and neural networks. Further exploring these properties is an ongoing, interesting area of study.

Trust but Verify
Both tree-based methods and neural networks provide powerful, flexible models that can deliver high-quality approximations of regression functions. However, the high degree of flexibility can lead to overfitting. Therefore, it is always important to verify the performance on test data to make sure that the predictive model being used is actually a good one.
A simple verification procedure is data splitting, which can be performed in the following way:

1.	We use a random subset of data for estimating/training the prediction rule.
2.	We use the other part of the data to evaluate the quality of the prediction rule, recording out-of-sample mean squared error, 𝑅2, or some other desired measure of prediction quality.

Recall that the part of the data used for estimation is called the training sample. The part of the data used to tune choices and compare learners is often called the validation sample, and, if we can afford it, we keep a separate test sample for a final
 




21: See also [16] for more recent the-oretical developments on provable guarantees for neural networks un-der sparsity conditions.
 


once-and-for-all evaluation. We have a data sample containing observations on outcomes 𝑌𝑖 and raw regressors 𝑍𝑖. Suppose we use 𝑛 observations for training and 𝑚 for testing/ validation. We use the training sample to compute prediction rule 𝑔 𝑍 . Let 𝑉 denote the indices of the observations in the test sample. Then the out-of-sample/test mean squared error is
1
MSE𝑡𝑒𝑠𝑡 = 𝑚	(𝑌𝑘 − 𝑔ˆ(𝑍𝑘)) .
𝑘∈𝑉
The out-of-sample/test 𝑅2 is defined as
 

2
𝑡𝑒𝑠𝑡
 
 MSE𝑡𝑒𝑠𝑡 
1 '5:𝑘∈𝑉 𝑌2
 
Note that it is conventional to use demeaned outcomes in the definition of 𝑅2.

 
A Simple Case Study using Wage Data
We illustrate23 ideas using a data set of 5150 observations from the March Current Population Survey Supplement 2015. 𝑌𝑖’s are log wages of never-married workers living in the U.S. 𝑍𝑖’s include experience, education, 23 industry and 22 occupation indicators, and some other characteristics. We consider a variety of linear and nonlinear rules for predicting 𝑌 with 𝑍.24
For the linear models, we estimate prediction rules of the form
𝑔ˆ(𝑍) = 𝛽ˆ′𝑋 using 𝑋 generated in two ways:
▶ (basic model) 𝑋 consists of the 51 raw regressors in 𝑍.
▶ (flexible model) 𝑋 consists of 246 variables composed of the 51 raw regressor in 𝑍, a fourth order polynomial in experience, and two-way interactions between the polynomial terms in experience and the non-experience variables in 𝑍.
We estimate 𝛽ˆ by linear regression/least squares and by the following penalized regression methods: Lasso and Post-Lasso with plug-in choice of 𝜆, cross-validated Lasso, Ridge, and Elastic Net.
For the nonlinear models, we estimate prediction rules of the form 𝑔 𝑍 without imposing that 𝑔 𝑍 = 𝛽ˆ′𝑋. That is, we do not assume prediction rules to be linear. We estimate the prediction
models by random forests, regression trees, boosted trees, and a neural network. We use an implementation of the random forest where, at the step of growing a regression tree, we choose
 



22: Irrelevance here only means that, given the set 𝑆 of relevant co-variates, the other variables do not contribute to the best prediction rule. It does not mean that the irrel-evant covariates have no predictive power on their own.
23: The notebooks for the wage prediction example are Notebooks 8.10.1.
 

 
the best variable to split upon among √51 randomly selected variables.
Table 8.1 displays results based upon a single split of data into training and testing sets. It shows the test MSE in column 1, the standard error of the test MSE in column 2, and the test 𝑅2 in column 3. The standard error is computed in the usual way as the standard deviation of the squared prediction errors on the
evaluation sample divided by √𝑚, i.e.
 


24: Recall that we include a more detailed comparison of penalized linear methods in these data in Chapter 3.
 
S.E
 
.(M-SE
 

𝑡𝑒𝑠𝑡 ) =
 
sd ((𝑌𝑘 − 𝑔ˆ(𝑍𝑘))2 : 𝑘 ∈ 𝑉 )
 
We see that the best performing prediction rule is provided by OLS using the raw 51 regressors. The performance of the remaining procedures outside of the Tree, Neural Net, and Least Squares using a large set of regressors is also very similar to that of OLS with the original regressors and each other. Looking at standard errors, we see that the vast majority of methods have test MSE’s that are within one standard error of the best test MSE, suggesting relatively little difference in performance across methods. The performance here is in line with what we saw in Chapter 3 where the simple OLS procedure performed very well. As discussed in Chapter 3, this finding is not surprising given the relatively small amount of data and available variables and is one reason we recommend that simple prediction rules be considered along with more elaborate learners in many social science applications.
The outliers, in terms of performing relatively poorly, are OLS using the flexible set of covariates as well as the regression tree (Pruned Tree) and the neural net. OLS with the flexible set of predictors uses a relatively large number of variables relative to the sample size and seems likely to be overfit. On the other hand, the regression tree is not fully tuned, and the neural net is based on a simple ad hoc architecture choice and is also not tuned. Thus, there may be room to improve the performance of these methods. The exact tuning choices used in this comparison (and code for exploring alternatives) are reported in Notebook 8.10.1.

8.5	Combining Predictions - Aggregation
- Ensemble Learning
Given different prediction rules, we can choose either a single method or an aggregation of several methods as our prediction
 


 

Least Squares (basic)	0.228	0.016	0.288
Least Squares (flexible)	0.243	0.016	0.238
Lasso	0.234	0.015	0.267
Post-Lasso	0.233	0.015	0.271
Lasso (flexible)	0.235	0.015	0.265
Post-Lasso (flexible)	0.236	0.016	0.261
Cross-Validated lasso	0.230	0.015	0.279
Cross-Validated ridge	0.236	0.015	0.262
Cross-Validated elnet	0.231	0.015	0.276
Cross-Validated lasso (flexible)	0.232	0.015	0.275
Cross-Validated ridge (flexible)	0.233	0.015	0.271
Cross-Validated elnet (flexible)	0.231	0.015	0.276
Random Forest	0.232	0.015	0.275
Boosted Trees	0.231	0.015	0.278
Pruned Tree	0.249	0.016	0.220
Neural Net (Early)	0.249	0.016	0.221


approach. An aggregated prediction is a linear combination of the basic prediction rules.25
Specifically, we consider an aggregated prediction rule of the form:
 
Table 8.1: Prediction Performance for the Test/Validation Sample.
 
𝐾
𝑔 𝑍 =	𝛼𝑘 𝑔𝑘 𝑍 ,
𝑘=1
where 𝑔𝑘’s denote the individual candidate prediction rules, potentially including a constant. The basic prediction rules are computed on the training data.
If the number of candidate prediction rules, 𝐾, is small, we can learn the aggregation weights on a validation sample 𝑉 by regressing the outcomes in the validation sample on the pre-dicted values produced by the learners trained on the training sample:
 

min
 
I: J
 
𝑌𝑖 − I:
 
2
𝛼𝑘 𝑔ˆ𝑘(𝑍𝑖)	.
 
(𝛼𝑘 )𝐾  𝑖∈𝑉
 
𝑘=1
 
This is the simplest form of stacking. Ideally, after choosing the weights on 𝑉, we would evaluate the resulting ensemble on a separate test sample that has not been used either for fitting the base learners or for fitting the weights.
If 𝐾 is large relative to 𝑚 = 𝑉 , we can regularize the aggrega-tion step; for example, we can use Lasso for aggregation:
 

min
 
I: J
 
𝑌𝑖 − I:
 
2
𝛼𝑘 𝑔ˆ𝑘(𝑍𝑖)
 
+ 𝜆 I:
 

|𝛼𝑘 |.
 
(𝛼𝑘 )𝐾  𝑖∈𝑉
 
𝑘=1
 
𝑘=1
 


Another popular choice is to minimize the sum of squared prediction errors in the validation sample subject to the con-straint that the coefficients on each candidate prediction rule are
proper weights – i.e. subject to min𝑘 𝛼𝑘 ≥ 0 and '5:𝐾	𝛼𝑘 = 1.
When we report performance on the same validation sample
used to estimate the weights, the reported 𝑅2 can be slightly optimistic. In the case study below we also report the adjusted
𝑅2 from the weight regression as a quick diagnostic; the cleanest approach remains evaluation on an untouched test sample.

 
Aggregation Results for the Case Study

We consider building an ensemble of the prediction rules fit in the wage prediction example.

The estimated weights are shown in Table 8.2. The least squares weight estimates are relatively large in magnitude with off-setting large negative and positive values. This behavior is likely driven by the fact that the underlying prediction rules are relatively similar and thus highly correlated with each other. The weights estimated by Lasso are easier to interpret. Here we see that the majority of the weight is placed on OLS with the basic set of controls, the random forest, and boosted trees. These learners seem to perform relatively well individually – see Table 8.1.
The ensemble methods also seem to mildly improve perfor-mance relative to any of the individual learners. Both the ensemble using OLS-estimated weights and the ensemble us-ing Lasso-estimated weights have an adjusted 𝑅2 of .30 in the validation sample (computed from the weight regression).

Auto ML Frameworks
There are a variety of new frameworks emerging that do auto-mated search and aggregation of different prediction methods. These automatic aggregation procedures use approaches like the one we outlined above or other heuristics. Example imple-mentations of automatic aggregation methods include H2O, AutoML [17], Auto Gluon [18] (which relies on Neural Nets), Auto-Sklearn, Hyperopt-Sklearn and FLAML.
We’ve tried H2O on the wage data. It produced a model that performs similarly to OLS with the basic predictor set, yielding a test 𝑅2 of 0.287. The performance is impressive because we
 






25: In econometrics and statistics, the procedures for combining sev-eral methods are called "model av-eraging" and "aggregation." In ma-chine learning, these terms are re-labeled as "ensembles" and "stack-ing."
 


gave H2O a time budget of just 100 seconds and did not specify anything other than the outcome and basic set of controls!

8.6	When Do Neural Networks Win?

 
The wage example may give a pessimistic impression of the power of deep learning (and machine learning more generally). But this comparison is taking place in a fairly small data set with fairly standard, tabular regressors. In this regime, many modern learners tend to look similar once they are tuned, and it is not unusual for a well-specified linear model to perform very competitively.
Deep neural networks tend to deliver their biggest gains when
(i) the sample size is large, (ii) the inputs are high-dimensional and unstructured (images, text, audio, clicks, or other raw signals), and (iii) the key challenge is to learn intermediate representations rather than to choose among a handful of hand-crafted predictors. In these settings we can also often reuse representations learned on other tasks (transfer learning), which effectively increases the amount of information available for the prediction problem at hand.

W
Constant	eight OLS	W
-0.357	eight Lasso
-0.097
Least Squares (basic)	1.031	0.274
Least Squares (flexible)	0.076	0.081
Lasso (basic)	1.012	0.000
Lasso (flexible)	-0.413	-0.085
Post-Lasso (basic)	0.453	0.153
Post-Lasso (flexible)	-0.237	0.000
LassoCV (basic)	-2.354	0.000
Lasso CV (flexible)	6.002	0.000
Ridge CV (basic)	0.545	0.000
Ridge CV (flexible)	0.388	0.000
ElNet CV (basic)	-0.143	0.000
ElNet CV (flexible)	-5.843	0.000
Pruned Tree	-0.091	0.000
Random Forest	0.415	0.319
Boosted Trees	0.235	0.292
Neural Net	0.048	0.000


A concrete example is Bajari et al. (2021) [19]. Here we are in-terested in predicting product prices given their characteristics,
 





















Table 8.2: Weights of the ensemble method.
 


which include both text and images. The resulting predic-tions are called hedonic prices. The core idea is a two-stage representation-and-prediction pipeline. First, neural networks such as BERT [20] and ResNet50 [21] convert raw text and images into several-thousand-dimensional numerical represen-tations (embeddings). Second, these embeddings are used as inputs to a multi-task deep neural network that predicts prices across time periods.
In this data set, which contains more than 10 million observa-tions, the test 𝑅2 of the deep neural network described above is about 90%. In contrast, random forests trained on the same embeddings deliver a test 𝑅2 in the ballpark of 80%, and a least-squares linear model using the embeddings as predictors achieves only around 70%. If we ignore the neural network embeddings and use only simple catalog features, the test 𝑅2 is below 40%. The main takeaway is not that forests or linear models are “bad”—they can be very strong baselines—but that deep learning can add large incremental value when the bottle-neck is learning representations from rich, high-dimensional inputs.
We will discuss further details of generating embeddings in Chapter 10.

8.7	Notes
Many of the formative developments in modern nonlinear regression were led by the statistics and artificial intelligence communities. The methods were rebranded as machine learning in the 90s, and learning with neural networks was rebranded as deep learning when it was realized that deep network architec-tures produced phenomenal results in image classification (and later in natural language processing tasks). The success of deep neural networks was a breakthrough associated with advances in both computing power and the ability to collect very large data sets. See [22], [4], and [23] for in-depth treatments of deep learning.
In Chapter 9, we will study the use of the machine learning and deep learning for statistical inference on causal and predictive effects in high-dimensional nonlinear regression settings. In Chapter 10, we’ll be using deep learning for engineering features from text and data (e.g. using images and product descriptions as "regressors").
 

8.8	Checklist
After working through this chapter, a student should be able to:
▶ Describe how a regression tree turns 𝑑-dimensional raw regressors into a step-function predictor by recursive splitting, and interpret a tree as a sequence of decision rules ending in leaf means.
▶ Explain overfitting in trees through the bias–variance tradeoff, and describe how cost-complexity pruning (pe-nalizing the number of leaves) and cross-validation help choose a tree size.
▶ Describe how random forests and boosted trees improve prediction relative to a single tree (averaging many deep trees vs. sequentially correcting residual structure), and identify the main tuning knobs at a conceptual level (depth, number of trees/iterations, learning rate).
▶ Translate between the “constructed regressors” view of neural networks and the standard layer notation, treating raw regressors 𝑍 as inputs, and explain the role of depth, width, activation functions, and shared representations (embeddings) in deep learning.
▶ Explain how neural networks are trained and regularized in practice, including the role of SGD, regularization (“weight decay"), dropout, and early stopping guided by validation performance.
▶ Evaluate predictive performance honestly using train/-validation/test splits (or cross-validation), compute and interpret out-of-sample MSE and 𝑅2, and understand how aggregation/stacking are used.

8.9	Additional resources
▶ Andrej Karpathy’s [24]Recipe for Training Neural Net-works provides a good workflow and practical tips for training good neural network models.
▶ For practical details of tree-based methods, please see Hastie et al. [25] ’s book "Introduction to Statistical Learn-ing".
▶ For an in-depth treatment of deep learning, see Zhang et al.’s [22] "Dive Into Deep Learning", Goodfellow et al.
[4] "Deep Learning", and Nielsen [23] "Neural Networks and Deep Learning".
 

8.10	Notebooks
Notebook 8.10.1 (ML-based Prediction of Wages) Python Notebook on ML-based Prediction of Wages and R Notebook on ML-based Prediction of Wages provide details of imple-mentation of penalized regression, regression trees, random forest, boosted tree and neural network methods, a compari-son of various methods and a way to choose the best method or create an ensemble of methods. Moreover, they provide an application of the FLAML (Python) and H2O (R) AutoML framework to the wage prediction problem. With a small time budget, both FLAML and H2O found the model that worked well for predicting wages.

Notebook 8.10.2 (Approximation of a Function by Random Forest and Neural Network) Python Notebook on Approxi-mation of a Function by Random Forest and Neural Network and R Notebook on Approximation of a Function by Random Forest and Neural Network illustrate the flexibility of these methods in approximating the function exp(4𝑥).


8.11	Exercises
Exercise 8.11.1 (Tree-based Strategies) Use two paragraphs to explain to a friend how one of the tree-based strategies works.

Exercise 8.11.2 (Neural Network) Use two paragraphs to explain to a friend how a basic neural network works.

Exercise 8.11.3 (Hands-on Example I) Experiment with one of the empirical notebooks provided and summarize your findings. For example, try to see if you can build a better per-forming neural network in the wage example. One possibility is to use custom models in Keras, where we can construct a partially linear model that borrows the strength of the basic linear model and corrects it slightly with a nonlinear deviation function.

Exercise 8.11.4 (Hands-on Example II) Experiment with the last (non-empirical) notebook. See, for example, if you can find a (much) simpler neural network that provides the same quality of fit as the current example in the notebook.
 

8.A Variable Importance via Permutations
There are many ways to assess “which variables matter” in a nonlinear model. A particularly convenient approach is the permutation importance method, because it is model-agnostic: it can be applied to linear models, trees, forests, boosted trees, or neural networks in exactly the same way.
Fix an evaluation sample 𝑉 (validation or test). Let 𝑔 be the fitted prediction rule, and let the baseline loss be the mean squared error on 𝑉,
1
𝐿0 :=   I:(𝑌𝑖 − 𝑔ˆ(𝑍𝑖))2 ,	𝑚 := |𝑉|.
To measure the importance of regressor 𝑗, we “break” its connec-tion to the outcome by randomly permuting its values within the evaluation sample. Concretely, let 𝜋 be a random permuta-tion of {1, . . . , 𝑚}. For each 𝑖 ∈ 𝑉, define a perturbed regressor vector 𝑍˜ (𝑗,𝜋) that is identical to 𝑍𝑖 except that its 𝑗-th coordinate
is replaced by the permuted value. We then recompute the loss using the perturbed inputs:
1	(𝑗,𝜋)  2
𝐿𝑗(𝜋) := 𝑚	𝑌𝑖 − 𝑔ˆ(𝑍˜ 𝑖	)  .
𝑖∈𝑉
The permutation importance score for regressor 𝑗 is the average increase in loss,
VI𝑗 := E𝜋 𝐿𝑗(𝜋) − 𝐿0 ,
where the expectation is taken over repeated random permu-tations.26 Variables can then be ranked by VI𝑗, from largest to smallest.
This measure has an appealing interpretation: if permuting vari-able 𝑗 barely changes predictive accuracy, then the fitted model is essentially not using 𝑗 once all other regressors are available. At the same time, we should be careful with interpretation when regressors are strongly correlated. If two variables carry very similar information, permuting one of them may have little effect because the other can act as a substitute. In such cases it can be informative to permute groups of related variables together, or to use conditional permutation schemes. The basic permutation idea appeared in Breiman’s original random forest paper [3].
 
















 

Figure 8.13: The structure of the predictive model in Bajari et al. (2021) [19]. The input consists of images and unstructured text data. The first step of the process creates numerical embeddings 𝐼 and 𝑊 for images and text data via deep learning methods, such as ResNet50 and BERT. The second step of the process takes as input 𝑋 = 𝐼 , 𝑊 and
creates predictions for hedonic prices 𝐻𝑡 𝑋 using deep learning methods with a multi-task structure. The models of
the first step are trained on tasks unrelated to predicting prices (e.g., image classification or word prediction), where embeddings are extracted as hidden layers of the neural networks. The models of the second step are trained by price prediction tasks. The multitask price prediction network creates an intermediate lower dimensional embedding
𝑉 = 𝑉 𝑋 , called a value embedding, and then predicts the final prices in all time periods 𝐻𝑡 𝑉 , 𝑡 = 1, ..., 𝑇 . Some variations of the method include fine-tuning the embeddings produced by the first step to perform well for price prediction tasks (i.e. optimizing the embedding parameters so as to minimize price prediction loss).
 


Bibliography



[1]	Leo Breiman. ‘Statistical modeling: The two cultures’. In: Statistical Science 16.3 (2001), pp. 199–231 (cited on
page 200).
[2]		Jerome Friedman, Trevor Hastie, and Robert Tibshirani. The Elements of Statistical Learning. Vol. 1. Springer Series in Statistics, New York, 2001 (cited on page 205).
[3]		Leo Breiman. ‘Random forests’. In: Machine learning 45.1 (2001), pp. 5–32 (cited on pages 205, 232).
[4]		Ian Goodfellow, Yoshua Bengio, and Aaron Courville. Deep Learning. http : / / www . deeplearningbook . org. MIT Press, 2016 (cited on pages 211, 229, 230).
[5]		Lili Jiang. A Visual Explanation of Gradient Descent Methods (Momentum, AdaGrad, RMSProp, Adam). 2020. url: https:
//towardsdatascience.com/a-visual-explanation-
of- gradient- descent- methods- momentum- adagrad-rmsprop-adam-f898b102325c (visited on 04/03/2022) (cited on page 211).
[6]		Peter L Bartlett, Andrea Montanari, and Alexander Rakhlin. ‘Deep learning: a statistical viewpoint’. In: Acta Numerica 30 (2021), pp. 87–201 (cited on page 212).
[7]	playground.tensorflow.org. https://playground.tensorflow. org/. Accessed: 2022-04-03 (cited on page 212).
[8]	Dmitry Yarotsky. ‘Error bounds for approximations with deep ReLU networks’. In: Neural Networks 94 (2017),
pp. 103–114 (cited on page 215).
[9]		Johannes Schmidt-Hieber. ‘Nonparametric regression us-ing deep neural networks with ReLU activation function’. In: Annals of Statistics 48.4 (2020), pp. 1875–1897 (cited on pages 215, 220–222).
[10]	Max H. Farrell, Tengyuan Liang, and Sanjog Misra. ‘Deep Neural Networks for Estimation and Inference’. In: Econo-metrica 89.1 (2021), pp. 181–213 (cited on pages 215, 222).
[11]		Patrick Kidger and Terry Lyons. ‘Universal Approxima-tion with Deep Narrow Networks’. In: Proceedings of Thirty Third Conference on Learning Theory. Ed. by Jacob Abernethy and Shivani Agarwal. Vol. 125. Proceedings of Machine Learning Research. PMLR, 2020, pp. 2306–2327 (cited on page 215).
 


[12]	Stefan Wager and Guenther Walther. ‘Adaptive concen-tration of regression trees, with application to random forests’. In: arXiv preprint arXiv:1503.06388 (2015) (cited on page 218).
[13]	Vasilis Syrgkanis and Manolis Zampetakis. ‘Estimation and Inference with Trees and Forests in High Dimensions’. In: Proceedings of Thirty Third Conference on Learning Theory. Ed. by Jacob Abernethy and Shivani Agarwal. Vol. 125. Proceedings of Machine Learning Research. PMLR, 2020,
pp. 3453–3454 (cited on pages 218, 219).
[14]	Stefan Wager and Susan Athey. ‘Estimation and Infer-ence of Heterogeneous Treatment Effects using Random Forests’. In: Journal of the American Statistical Association
113.523 (2018), pp. 1228–1242 (cited on page 219).
[15]		Charles J. Stone. ‘Optimal global rates of convergence for nonparametric regression’. In: Annals of Statistics 10.4 (1982), pp. 1040–1053 (cited on page 220).
[16]	Rahul Parhi and Robert D Nowak. ‘Deep Learning Meets Sparse Regularization: A Signal Processing Perspective’. In: arXiv preprint arXiv:2301.09554 (2023) (cited on page 223).
[17]		Erin LeDell and Sebastien Poirier. ‘H2O AutoML: Scalable automatic machine learning’. In: Proceedings of the AutoML Workshop at ICML. Vol. 2020. 2020 (cited on page 227).
[18]		Nick Erickson, Jonas Mueller, Alexander Shirkov, Hang Zhang, Pedro Larroy, Mu Li, and Alexander Smola. ‘Autogluon-tabular: Robust and accurate AutoML for structured data’. In: arXiv preprint arXiv:2003.06505 (2020) (cited on page 227).
[19]		Patrick L. Bajari, Zhihao Cen, Victor Chernozhukov, Manoj Manukonda, Jin Wang, Ramon Huerta, Junbo Li, Ling Leng, George Monokroussos, Suhas Vĳaykunar, et al. Hedonic prices and quality adjusted price indices powered by AI. Tech. rep. cemmap working paper CWP04/21, 2021 (cited on pages 228, 233).
[20]		Jacob Devlin, Ming-Wei Chang, Kenton Lee, and Kristina Toutanova. ‘BERT: Pre-training of deep bidirectional trans-formers for language understanding’. In: arXiv preprint arXiv:1810.04805 (2018) (cited on page 229).
[21]		Kaiming He, Xiangyu Zhang, Shaoqing Ren, and Jian Sun. ‘Deep residual learning for image recognition’. In: Proceedings of the IEEE conference on computer vision and pattern recognition. 2016, pp. 770–778 (cited on page 229).
 


[22]		Aston Zhang, Zachary C. Lipton, Mu Li, and Alexander J. Smola. ‘Dive into deep learning’. In: URL: https://d2l. ai (2020) (cited on pages 229, 230).
[23]	Michael A. Nielsen. Neural Networks and Deep Learning. Determination Press, 2015 (cited on pages 229, 230).
[24]	Andrej Karpathy. A Recipe for Training Neural Networks. 2019. url: http://karpathy.github.io/2019/04/25/ recipe/ (visited on 04/06/2022) (cited on page 230).
[25]		Gareth James, Daniela Witten, Trevor Hastie, and Robert Tibshirani. An introduction to statistical learning. Springer, 2013 (cited on page 230).
 

 
Statistical Inference on Predictive and Causal Effects in Modern Nonlinear
Regression Models


"Whoever has participated in non-trivial research in any domain of science involving statistical problems must have encountered the difficulty that none of the statistical procedures found in the books fits exactly the practical situation."
– Jerzy Neyman [1].










Here we discuss double/debiased machine learning (DML) methods for performing inference on average predictive or causal effects in two important classes of models: partially linear regression models and interactive regression models. We also present a general DML method for performing inference on a low-dimensional target parameter in the presence of high-dimensional nuisance parameters. Two case studies illustrate the approach.
 
9
9.1	Introduction	238
9.2	DML Inference in the Partially Linear Regression Model	239
Discussion of DML Con-struction	244
The Effect of Gun Own-ership on Gun-Homicide Rates	248
Revisiting the Price Elastic-ity for Toy Cars	252
9.3	DML Inference in the Inter-active Regression Model  254
DML Inference on APEs and ATEs	254
DML Inference for GATEs and ATETs	257
The Effect of 401(k) Eligibil-ity on Net Financial Assets259
9.4	Generic Debiased (or Dou-ble) Machine Learning . 263 Key Ingredients	263
Neyman Orthogonal Scores for Regression Problems  266
The  DML  Inference Method	267
Properties of the General DML Estimator	269
9.5	Notes	271
9.6	Notebooks	272
9.7	Exercises	273
9.A	Bias Bounds with Proxy Treatments	275
9.B	Illustrative Neyman Orthog-onality Calcuations 		276
 
9.1	Introduction
 

We recall the predictive effect question:
▶ How does the predicted value of the outcome,
E[𝑌 | 𝐷, 𝑋],
change if a regressor value 𝐷 increases by a unit, while regressor values 𝑋 remain unchanged?
This question may have a causal interpretation within any SEM, where conditioning on 𝑋 is sufficient for identification of the causal effect of 𝐷 on 𝑌 – that is, in any situation where unconfoundedness holds.1 When this condition holds, the question becomes the causal effect question:
▶ How does the predicted value of the potential outcome,
E[𝑌(𝑑) | 𝑋],
change if we intervene and change the treatment value 𝑑
by a unit, conditional on the observed 𝑋?
Both questions are interesting and useful to ask, depending on the application. In what follows, we set up double/debiased machine learning (DML) methods for answering these questions with data.2 These statistical inference methods do not distinguish between the two types of questions, so the methods are equally applicable to answering both types.
Here we discuss DML methods for performing inference on average predictive or causal effects in two important classes of nonlinear regression models. After presenting these two special cases, we also present a general DML method for performing inference on a low-dimensional target parameter in the presence of high-dimensional nuisance parameters.
Current arguments used to formally establish √𝑛 asymptotic
normality of DML estimators of target parameters while allow-ing for the use of general machine learning methods for learning nuisance parameters make use of two key ingredients. First, the DML method is based on a Neyman orthogonal representation of the target parameters. Intuitively, Neyman orthogonality is the requirement that estimates of the parameters of interest are locally insensitive to the value of nuisance parameters. This local insensitivity reduces the spillover of regularization biases that are inherent in using regularized methods, such as the machine learning methods discussed in Chapter 8, to learn nuisance parameters onto the estimation of target parameters.
 














1: Recall that unconfoundedness or conditional exogeneity is covered in Chapter 5-Chapter 11.











2: In the book we will use the terms double/debiased machine learn-ing, double machine learning and debiased machine learning inter-changeably. It generalizes the dou-ble/debiased Lasso approach to generic machine learning methods.
 


Second, DML makes use of cross-fitting: an efficient form of sample splitting that guards against "own-observation biases" that may arise from overfitting.
To illustrate the general principles, we provide two case studies. In the first, we perform inference on the effect of gun ownership on homicide rates. In the second, we perform inference on the effect of 401(k) eligibility on financial assets.

9.2	DML Inference in the Partially Linear Regression Model
We first answer the predictive/causal effect question within the context of the partially linear regression model (PLM):
𝑌 = 𝛽𝐷 + 𝑔(𝑋) + 𝜖,	E[𝜖 | 𝐷, 𝑋] = 0,	(9.2.1)
where 𝑌 is the outcome variable, 𝐷 is the regressor of inter-est, and 𝑋 is a high-dimensional vector of other regressors or features, called "controls." The coefficient 𝛽 answers the predic-tive effect question. In this section, we discuss estimation and construction of confidence intervals for 𝛽. We also provide a case study in which we examine the effect of gun ownership on homicide rates.
The PLM allows a part of the regression function, 𝑔 𝑋 , to be fully nonlinear, which generalizes the approach from Chapter 4. However, the model is still not fully general, because it imposes additivity in 𝑔 𝑋 and 𝐷. We shall consider a fully unrestricted model in the case of a binary treatment 𝐷 in Section 9.3. It is worth pointing out before turning to that setting that the PLM is not as restrictive as it appears since we can consider explicit interactions within the partially linear framework.

 





 






In what follows, we employ an operation that "partials-out" 𝑋 from a random variable 𝑉 by taking 𝑉 as an input and returning the "residualized" form:
 
In practice and depending on the learner, it may be convenient to
treat 𝑔𝑙 𝑋𝑙 = ℎ 𝐷𝑘 𝑘≠𝑙 , 𝑍 as a flexible function during estimation rather than impose the structure
𝑔𝑙(𝑋𝑙) := '5:𝑘≠𝑙 𝛽𝑘 𝐷𝑘 + 𝑔(𝑍).
 

 
𝑉˜
 
:= 𝑉 − E[𝑉 | 𝑋].
 
Applying this operation to (9.2.1), we obtain

 
𝑌˜ = 𝛽𝐷˜
 
+ 𝜖,	E[𝜖𝐷˜ ] = 0,	(9.2.2)
 
where 𝑌˜ and 𝐷˜ are the residuals left after predicting 𝑌 and 𝐷
 
using 𝑋. Specifically, we have that
𝑌˜ := 𝑌 − ℓ(𝑋) and 𝐷˜
 

:= 𝐷 − 𝑚(𝑋),
 
where ℓ 𝑋 and 𝑚 𝑋 are defined as conditional expectations of 𝑌 and 𝐷 given 𝑋:
ℓ(𝑋) := E[𝑌 | 𝑋] and 𝑚(𝑋) := E[𝐷 | 𝑋].
Here we recall that the conditional expectations of 𝑌 and 𝐷 given 𝑋 are the best predictors of 𝑌 and 𝐷 using 𝑋 under squared error loss.
The equation E[𝜖𝐷˜ ] = 0 above is the Normal Equation for the
 
population regression of 𝑌˜
following result:
 
on 𝐷˜ . This equation implies the
 

 
 

 
Thus, 𝛽 can be interpreted as a regression coefficient of residu-alized 𝑌 on residualized 𝐷, where the residuals are defined by respectively subtracting the conditional expectation of 𝑌 given
𝑋 and 𝐷 given 𝑋 from 𝑌 and 𝐷. This result generalizes the FWL from linear models to partially linear models.
Our estimation procedure for 𝛽 in the sample will mimic the partialling out procedure in the population. We also rely on cross-fitting (outlined below) to make sure any overfitting in learning the conditional expectation functions used in construct-ing the residualized quantity does not spillover to contaminate the final estimator of the quantity of interest. 3

Double/Orthogonal ML for the Partially Linear Model
1.	Partition data indices into random folds of approxi-mately equal size: {1, ..., 𝑛} = ∪𝐾 𝐼𝑘. For each fold
𝑘=1
𝑘 = 1, ..., 𝐾, compute ML estimators ℓˆ[𝑘] and 𝑚ˆ [𝑘] of
the conditional expectation functions ℓ and 𝑚, leav-
ing out the 𝑘-th block of data. Obtain the cross-fitted residuals for each 𝑖 ∈ 𝐼𝑘:
𝑌ˇ𝑖 = 𝑌𝑖 − ℓˆ[𝑘](𝑋𝑖),	𝐷ˇ 𝑖 = 𝐷𝑖 − 𝑚ˆ [𝑘](𝑋𝑖).
2.	Apply ordinary least squares of 𝑌ˇ𝑖 on 𝐷ˇ 𝑖 . That is, obtain 𝛽ˆ as the root in 𝑏 of the normal equations:
𝔼𝑛[(𝑌ˇ − 𝑏𝐷ˇ )𝐷ˇ ] = 0.
3.	Construct standard errors and confidence intervals as in standard least squares theory.

In what follows it will be convenient to use the notation

∥ℎ∥𝐿2 :=	E𝑋[ℎ2(𝑋)],
where, as before, E𝑋 computes the expectation over values of
𝑋.
 













3: Step 1 of the Double/Orthog-onal ML for the Partially Linear Model algorithm is the cross-fitting step. Here, the nuisance parame-ters - the conditional expectation functions in this example - are learned from one set of observa-tions and then applied to a differ-ent set of observations to construct the residualized quantities. Heuris-tically, this step keeps any overfit-ting that occurred in estimating the conditional expectation from feed-ing through and contaminating the final estimate of the parameter of interest. See Section 9.4 for more discussion.
 

 
 


 


Confidence Interval The standard error of 𝛽ˆ is	Vˆ/𝑛, where
Vˆ is an estimator of 𝑉. The result implies that the confidence
interval
J𝛽ˆ − 2JVˆ/𝑛, 𝛽ˆ + 2JVˆ/𝑛l
covers 𝛽 in approximately 95% of possible realizations of the sample. In other words, if our sample is not atypical, the interval covers the truth.
Selecting the Best ML Learners of ℓ and 𝑚. There may be sev-eral methods that satisfy the quality requirements of Theorem 9.2.2, and we may therefore ask what ML methods we should use in practice. Consider a collection of ML methods indexed by 𝑗  1, ..., 𝐽 . Our goal would be to select the methods that minimize an upper bound on the bias of the DML estimator.
The bias of the DML estimator is controlled by the mean square approximation errors (MSAE):

1  𝐾	1  𝐾
𝐾 I: ∥ℓˆ[𝑘] − ℓ ∥22 and 𝐾 I: ∥𝑚ˆ [𝑘] − 𝑚∥22 .	(9.2.4)
Therefore, we can select the best ML method for estimating 𝑚
and the best method for estimating ℓ to minimize the upper
 


bound on the bias. We will be using mean square prediction
errors as proxies for MSAEs.	MSPEs approximate MSAEs up to terms that do not depend on 𝑗. Hence, by doing MSPE minimiza-tion, we in fact approximately min-imize MSAEs.





















Note that it may well be that different methods provide the best prediction rules for 𝑌 and 𝐷. By allowing ourselves to consider multiple methods, we allow finding methods that perform best for the different tasks which should improve performance in practice relative to insisting on one fixed, pre-specified method.
Rather than selecting the single best predictors of 𝑌 and 𝐷, we can also use residuals to form linear ensembles of ML methods that minimize MSPEs.

In practical terms, the result of Corollary 9.2.3 means that we should only choose among or aggregate over relatively few ML methods. Otherwise, we may end up overfitting (since we are "cheating" here by using validation data to form the aggregator).
 


 

 
Discussion of DML Construction
The partialling out operation causes the moment equations defining 𝛽 to be Neyman orthogonal. That is, the moment con-ditions are locally insensitive to perturbations of the nuisance parameters ℓ and 𝑚.4 We discussed Neyman orthogonality in the context of high-dimensional linear regression models in Chapter 4. We return to and generalize this discussion formally in Section 9.4. This property alleviates the impact of the bias in estimation of 𝑚 and ℓ that arises when ML estimators are applied in high-dimensional settings.
Naive application of machine learning methods directly to outcome equations may lead to highly biased estimators because the resulting strategy is not Neyman orthogonal. The lack of Neyman orthogonality means that estimates of the parameter of interest are heavily impacted by estimation of the nuisance parameters. This sensitivity means that any biases in estimation of 𝑔, which are essentially unavoidable in high-dimensional estimation, create a non-trivial bias in the estimate of the main effect. This bias is large enough to cause failure of conventional inference.
The left panel of Figure 9.1 illustrates the bias arising due to the use of a non-orthogonal, naive approach for learning 𝛽; see Remark 9.2.4 for details. Specifically, the figure shows the
behavior of a conventional (non-orthogonal) ML estimator, 𝛽˜,
in the partially linear model in a simple simulation experi-ment where we learn 𝑔 using a random forest. The 𝑔 in this experiment is a very smooth function of a small number of variables, so the experiment is seemingly favorable to the use of random forests a priori. The histogram shows the simulated
distribution of the centered estimator, 𝛽˜  𝛽. The estimator is
badly biased, shifted much to the right relative to the true value
𝛽. Furthermore, the distribution of the estimator (approximated by the blue histogram) is substantively different from a normal approximation (shown by the red curve) derived under the assumption that the bias is negligible.
 






4: Recall that we use the term nui-sance parameter to name parame-ters that are not the target parame-ters. In the PLM, the target param-eter is 𝛽, and ℓ and 𝑚 are nuisance parameters.
 


 
Non-Orthogonal, n = 500, p = 20
10

9
 
Orthogonal, n = 500, p = 20
10

9
 

8	8

7	7

6	6

5	5

4	4

3	3

2	2

1	1
 

0
-0.2  -0.15  -0.1  -0.05	0	0.05  0.1  0.15  0.2
 

0
-0.2  -0.15  -0.1  -0.05	0	0.05  0.1  0.15  0.2
 

Figure 9.1: Histograms of centered estimates of 𝛽 from a simulation experiment where random forests are used to learn nuisance functions. The left panel provides the histogram of 𝛽 − 𝛽 where 𝛽 is obtained from a procedure that does not satisfy Neyman orthgonality; see Remark 9.2.4. The right panel provides the histogram of 𝛽 𝛽 where 𝛽 is the DML estimator. In both cases, the same tuning settings are used for random forest estimation of nuisance parameters.

The right panel of Figure 9.1 illustrates the behavior of the (Neyman) orthogonal DML estimator, 𝛽ˆ, in the partially linear model in a simple experiment where we learn nuisance func-
tions 𝑚 and ℓ using random forests. Note that the simulated data are exactly the same as those underlying the left panel. The simulated distribution of the centered estimator, 𝛽ˆ  𝛽, (given
by the blue histogram) illustrates that the estimator is approxi-mately unbiased, concentrates around 𝛽, and is approximately normally distributed. The low bias arises because DML uses
 


Neyman orthogonal moment equations.
The DML algorithm also uses a form of sample splitting, called cross-fitting, to guard against a less obvious source of bias that may arise when estimation of nuisance parameters results in overfitting. Heuristically, overfitting simply means that an estimator has captured not just generalizable signal but also noise that is idiosyncratic to each observation. The presence of this idiosyncratic noise in the estimates of the nuisance functions may then lead to a type endogeneity bias as the observed estimates of the nuisance functions which are used in place of the unobserved, true nuisance functions are associated with the noise in the observations used to learn the nuisance functions. Cross-fitting guards against this source of bias as overfitting resulting from learning nuisance functions in one subsample will not carry over when the nuisance function estimates are applied on a different, separate subsample. As it is very hard to ensure that highly complex fitting methods such as boosting, deep neural networks, and random forests do not overfit, it is hard to know that their use would not lead to substantial biases without making use of sample splitting. That is, if we don’t do sample splitting and the ML estimates overfit, we may end up with very large biases.
Figure 9.2 illustrates how the bias resulting from overfitting in the estimation of nuisance functions can cause DML without sample splitting (i.e. estimation on the full sample using an estimator that satisfied Neyman orthogonality) to be biased and how sample splitting eliminates this problem. In the left panel the histogram shows the finite-sample distribution of the DML estimator in the partially linear model in a simple simulation experiment where nuisance parameters are estimated with overfitting using the full sample, i.e. without sample splitting. The finite-sample distribution is clearly shifted to the left of the true parameter value, demonstrating the substantial bias. In the right panel, the histogram shows the finite-sample distribution of the DML estimator in the same simulation experiment in the partially linear model where nuisance parameters are estimated with sample-splitting using the cross-fitting estimator. Here, we see that the use of sample-splitting has completely eliminated the bias induced by overfitting.
 


 
Figure 9.2: Left: DML distribution without sample-splitting. Right: DML distribution with cross-fitting.






Figure 9.3: Witchcraft tables used by some ML practitioners to tune parameters. There are no known theoretical guarantees attached to this tuning method.
 

 
The Effect of Gun Ownership on Gun-Homicide Rates
We consider the problem of estimating the effect of gun owner-ship on the homicide rate.5 For this purpose, we estimate the partially linear model:
𝑌𝑖,𝑡 = 𝛽𝐷𝑖,(𝑡−1) + 𝑔(𝑋𝑖,𝑡 , 𝑋¯ 𝑖 , 𝑋¯ 𝑡 , 𝑋𝑖,0 , 𝑌𝑖,0 , 𝑡) + 𝜖𝑖,𝑡 .
𝑌𝑖,𝑡 is the log homicide rate in county 𝑖 at time 𝑡. 𝐷𝑖,𝑡−1 is the log fraction of suicides committed with a firearm in county
𝑖 at time 𝑡  1, which we use as a proxy for gun ownership,
𝐺𝑖,𝑡, which is not observed. 𝑋𝑖,𝑡 is a set of demographic and economic characteristics of county 𝑖 at time 𝑡. We use 𝑋¯ 𝑖 to denote the within county average of 𝑋𝑖,𝑡 and 𝑋¯ 𝑡 to denote the
within time period average of 𝑋𝑖,𝑡. 𝑋𝑖,0 and 𝑌𝑖,0 denote initial conditions in county 𝑖. We use 𝑍𝑖,𝑡 to denote the set of observed control variables  𝑋𝑖,𝑡 , 𝑋¯ 𝑖 , 𝑋¯ 𝑡 , 𝑋𝑖,0, 𝑌𝑖,0, 𝑡 . The sample covers
195 large United States counties between the years 1980 through 1999, giving us 3900 observations.
Raw control variables 𝑋𝑖,𝑡 are from the U.S. Census Bureau and contain demographic and economic characteristics of the counties such as features of the age distribution, the income distribution, crime rates, federal spending, home ownership rates, house prices, educational attainment, voting patterns, employment statistics, and migration rates.
The intent here is that parameter 𝛽 is an approximation of the causal effect of gun ownership, 𝐺𝑖,𝑡, on homicide rates
𝑌𝑖,𝑡, controlling for county-level demographic and economic characteristics; see Figure 9.4 for a potential DAG representation. To attempt to flexibly account for fixed heterogeneity across counties, common time factors, and deterministic time trends, we include county-level averages, time period averages, initial conditions, and the time index as additional control variables. This strategy is related to strategies for addressing latent sources of heterogeneity via conditioning as in [4]. Finally, for simplicity in this illustration, we assume that all sources of dependence are accounted for by observed variables such that we may take
𝜖𝑖,𝑡 as independent across counties, 𝑖, and over time, 𝑡.
As a summary statistic we first look at a simple regression of 𝑌𝑖,𝑡 on 𝐷𝑖,𝑡−1 without controls. The point estimate is 0.302 with 95% confidence interval, based on the assumption that
𝜖𝑖,𝑡 is independent over time and space, ranging from 0.277 to
0.327. These results suggest that increases in gun ownership rates are associated with (predict) gun homicide rates – if gun
 





5: We adapt the basic strategy from Cook and Ludwig [3] who consider using suicide rates as a proxy for gun ownership.
The Notebooks 9.6.1 provide code for the example.
 

 







(a)










(b)	𝐷𝑖,𝑡 = 𝐺𝑖,𝑡
 




































𝑍𝑖,𝑡
 











𝑌𝑖,𝑡
















𝑌𝑖,𝑡

 
 
Figure 9.4: Possible DAG Structure for the Gun Ownership Example. Figure (a) provides a relatively gen-eral DAG structure that could rep-resent the gun ownership exam-ple. We include nodes for latent county specific and time period spe-cific shocks (𝐴𝑖 and 𝐹𝑡 ). Often such shocks are accounted for with so-called "fixed effects." In practice, estimating models with fixed ef-fects are typically leverages strong functional form assumptions. Here, we instead leverage the different, though still strong, assumption that flexibly conditioning on observ-ables, including time- and county-specific variables, is sufficient to account for latent county specific and time period specific shocks. Unfortunately, neither the causal effect of the unobserved 𝐺𝑖,𝑡 or observed 𝐷𝑖,𝑡 is identified assum-ing only structure (a). Figure (b) allows identification of the average causal effect 𝐺𝑖,𝑡  𝑌𝑖,𝑡 by impos-ing 𝐺𝑖,𝑡 = 𝐷𝑖,𝑡 . To the extent that
we believe 𝐺𝑖,𝑡   𝐷𝑖,𝑡 , the structure
in (b) allows us to approximate the effect of interest. We discuss a fur-ther generalization in Section 9.A where we rely on the the assump-tion that 𝐷𝑖,𝑡 is equal to 𝐺𝑖,𝑡 plus an additive, independent measure-ment error. In this case, the target parameter 𝛽 will be attenuated rel-ative to the true causal effect.
 

ownership increases by 1% the predicted gun homicide rate goes up by around 0.3% – without controlling for any time factors or county characteristics.
Since our goal is to estimate the effect of gun ownership after controlling for a rich set characteristics, we next include the controls and estimate the model by an array of the modern regression methods that we’ve learned. Specifically, we con-sider ten candidate learners for predicting the outcome and for predicting the target variable. We consider linear models esti-mated with OLS using no control variables (OLS - No Controls), using only the raw control variables (OLS - Basic), and using the raw control variables plus the constructed cross-sectional and time series averages and initial conditions (OLS - All). The remaining methods always take as inputs the complete set of candidate control variables including cross-sectional averages, time-specific averages, and initial conditions. We use cross-
 

 
validation to choose tuning parameters for Lasso, Ridge, and Elastic Net. We consider a random forest with the software default tuning choices and boosted trees constrained to have depth four. Finally, we consider neural nets with four hidden layers of 50 nodes each and either dropout or early stopping. For further training details, refer to the example’s notebooks.



OLS - Basic	0.4889	0.1269
OLS - All	0.4259	0.1259
Lasso (CV)	0.4625	0.1353
Ridge (CV)	0.5303	0.1448
Elastic Net (CV)	0.4651	0.1339
Random Forest	0.4027	0.1246
Boosted trees - depth 4	0.4018	0.1223
DNN dropout	0.6142	0.7594


.
Before turning to estimation results for 𝛽, we look at estimated out-of-sample predictive performance in Table 9.1 which reports cross-fitted root mean square error (RMSE) for the different procedures we consider. The column RMSE Y gives the RMSE for predicting the outcome (log gun homicide rate), and the column RMSE D gives the RMSE for predicting our gun prevalence variable (log of the lagged firearm suicide rate). Here we see evidence of the potential relevance of trying several learners rather than just relying on a single, pre-specified choice. There are noticeable differences between performance of most of the learners, with Boosted Trees and Random Forests providing the best performance for predicting both the outcome and policy variable.
Table 9.2 presents the estimated effects of the lagged gun owner-ship rate on the gun homicide rate as well as the corresponding standard errors. Looking across the results, we see relatively large differences in estimates. These differences suggest that the choice of learner has a material impact in this example. Looking at the measures of predictive performance in Table 9.1, we see that Random Forest and Boosted trees performed best among the considered learners, and we also see that their performance is relatively similar in terms of point estimates of the effect of the lagged gun ownership rate on the gun homicide rate and standard errors. Focusing on the Boosted trees row, the point estimate suggests a 1% increase in the gun proxy is associated
 








Table 9.1: Cross-fitted RMSE for predicting outcome (Y) and vari-able of interest (D) in the gun illus-tration.
 



OLS - No Controls	                Estimate	
0.3018	Standard Error  0.0126	Table 9.2: Cross-fit estimates for the coefficient on our gun control proxy and standard errors in the
OLS - Basic	0.3539	0.0738	gun illustration.
OLS - All	0.2016	0.0570	
Lasso (CV)	0.2758	0.0565	
Ridge (CV)	0.4607	0.0600	
Elastic Net (CV)	0.2944	0.0583	
Random Forest	0.0631	0.0524	
Boosted trees - depth 4	0.1025	0.0537	
DNN dropout	0.2444	0.0125	
DNN early stopping	0.4850	0.0492	
Best	0.1025	0.0537	
 Ensemble	0.1079	0.0548		

.

with around .1% increase in the gun homicide rate, though the 95% confidence interval is relatively wide: (-0.003,0.208).
The final two rows of Table 9.2 provide estimates based on using ensembles of the individual methods to estimate the nuisance
functions. The row "Best" uses the method with the lowest MSE as the estimator for ℓˆ 𝑋 and 𝑚 𝑋 . In this example, Boosted trees give the best performances in predicting both 𝑌𝑖,𝑡 and
𝐷𝑖,𝑡−1, so the results for "Best" and Boosted trees are identical. The row "Ensemble" uses the linear combination of all ten of the predictors that produces the lowest MSE for predicting 𝑌𝑖,𝑡 or 𝐷𝑖,𝑡 1 as the estimator ℓˆ 𝑋 or 𝑚 𝑋 respectively. Here the
results are similar to the results using only Boosted trees, but differ somewhat due to non-zero linear combination coefficients on the other learners. In either case, we see mild evidence of positive effect of our gun ownership proxy on the gun homicide rate, though the 95% confidence interval includes 0 and small negative values in both cases.
We also wish to emphasize that this example helps illustrate the practical importance of considering several learners as it is generally ex ante unknown which learner will work best and there are substantial differences between the better performing learners (random forests, boosted trees, and the two ensemble methods) and the others both in terms of predictive accuracy and the results DML estimates and standard errors for the effect of interest.
 

 
Revisiting the Price Elasticity for Toy Cars
We now revisit again the example from Chapter 0. We are interested in the coefficient 𝛼 in the PLM:
𝑌 = 𝛼𝐷 + 𝑔(𝑊) + 𝜖,
where 𝑌 is log-reciprocal-sales-rank, 𝐷 is log-price, and 𝑊 are product features. In Chapter 4, we let 𝑔 𝑊  = 𝛽′𝑇 𝑊 be a high-dimensional regression using a transformation that
included powers and interactions. We now employ flexible nonlinear regression models using DML. We take 𝑊 to consist of indicators for brand and subcategory along with physical dimensions interacted with missingness indicators, using no further transformation, leading to a 2083-dimensional feature vector. We consider inference on 𝛼 using DML with different choices of learners applied to both 𝑚 𝑊 and 𝑔 𝑊 : decision trees, gradient boosted trees (with 1000 trees), random forests (with 2000 trees), or a neural network (with two hidden layers of 200 and 20 neurons, respectively, and ReLU activations).
In Table 9.3, we report the cross-validated 𝑅2 for predicting 𝐷 and 𝑌 with each of the learners along with the resulting DML point estimate, standard error estimate, and 95% confidence interval. The first thing we note is that all confidence intervals indicate a substantial negative effect, with a clear indication not only of the direction of the effect but also of its overall magnitude.
Let us first compare these results to the previous ones from when we last revisited this example in Chapter 4. There we saw that OLS with varying number of features failed to exclude 0 from the confidence interval and that Double Lasso led to an interval [-0.099, -0.029]. We can attribute the latter more negative interval to controlling more for confounding, as we expect confounding effects to push the apparent price-sales relationship upward compared to the theorized downward causal relationship.
Here, we see that with more flexible nonlinear methods we obtain an even more negative estimate and confidence interval. This result appears to be consistent with the degree to which we are able to control for confounders. Lasso has a cross-validated
𝑅2 of 0.09 and 0.32 for predicting 𝑌 and 𝐷, respectively. The
𝑅2’s in Table 9.3 are substantially larger. That the corresponding estimates and intervals are also more negative seems to coincide with our theoretical prediction.
 










This example makes use of propri-etary data, so no notebooks are pro-vided.
 








0.2



0.1


0.0

 
 0.1


 0.2


 0.3
1.5 2.0 2.5	3.0	3.5	4.0	4.5
Log-price
 
Figure 9.5: DML estimates of the price-sales relationship using PLM with higher-order transformations of price. Note the exponential scal-ing in the axes, which transforms the overall scale back to (non-log) price and sales (reciprocal sales rank).
 
Comparing between the nonlinear methods, this theory appears to remain consistent. Forest and neural net methods have higher
𝑅2’s than tree and gradient boosting methods, and, at the same time, have more negative point estimates and confidence intervals.

Estimate
-0.109	Std. Err.
0.018	95% CI
[-0.143, -0.074]	Table 9.3: DML estimates of price
elasticity based on different learn-
ers, along with their 𝑅2 for predict-
-0.102	0.019	[-0.139, -0.064]	ing 𝐷 and 𝑌.
-0.134	0.019	[-0.171, -0.096]	
-0.132	0.020	[-0.171, -0.093]	


Note that just as we can play with transformations in linear models, we can do the same in the PLM. That is, we can modify from partial linearity in the univariate 𝐷 to partial linearity in a multivariate set of transformations, 𝑇 𝐷 . We can use this to investigate potentially non-linear price-sales relationships in this data. Let us transform 𝐷 using the first 𝑟 (probabilist’s) Hermite polynomials (applied to a location-scale-standardized
𝐷). We then use DML with neural network learners to learn the coefficients on these polynomial terms. That is, we estimate a model of the form
𝑟
𝑌 =	𝛼𝑗𝑇𝑗 𝐷	𝑔 𝑊	𝜖
𝑗=1
where 𝑇𝑗 𝐷 represents the 𝑗th term in the Hermite polynomial of order 𝑟.
We plot the resulting estimated functions for 𝑟 = 1, . . . , 4 in
 

 
Figure 9.5. As can be seen, the price-sales relationship seems to not be exactly linear, as it stabilizes around a flat-then-decreasing shape for degrees 2, 3, and 4. This shape either suggests that indeed there is less elasticity at lower price points (the mean log-price is 3.06) or that we simply failed to account well for confounding effects at lower price points, which may be idiosyncratic compared to higher-priced toy trucks.

9.3	DML Inference in the Interactive Regression Model
DML Inference on APEs and ATEs
We consider estimation of average treatment effects when treat-ment effects are fully heterogeneous and the treatment variable is binary. We consider observable variables 𝑊 = 𝑌, 𝐷, 𝑋 and
the pair of regression equations:
𝑌 = 𝑔0(𝐷, 𝑋) + 𝜖,	E[𝜖 | 𝑋, 𝐷] = 0,	(9.3.1)
 

The relationship being not exactly linear does not invalidate using a PLM (in the untransformed univari-ate 𝐷). It still corresponds to an av-erage derivative – see Remark 9.3.3 – which can be more interpertable than nonlinear estimates of a causal effect.
 
𝐷 = 𝑚0(𝑋) + 𝐷˜ ,	E[𝐷˜ | 𝑋] = 0,	(9.3.2)
where the second regression equation captures that 𝐷 and 𝑋 are confounded. Here 𝑌 is an outcome of interest, 𝐷  0, 1 is a binary policy or treatment variable, and 𝑋 are controls/con-founding factors. Since 𝐷 is not additively separable in the first equation, this model is more general than the partially linear model for the case of binary 𝐷.
A common target parameter of interest in this model is the average predictive effect (APE),
𝜃0 = E[𝑔0(1, 𝑋) − 𝑔0(0, 𝑋)].
This quantity is the average predictive effect of switching 𝐷 = 0 to 𝐷 = 1. Under ignorability/conditional exogeneity, the APE coincides with the average treatment effect (ATE) of the intervention that moves 𝐷 = 0 to 𝐷 = 1.
The confounding factors 𝑋 affect the policy variable via the propensity score 𝑚0 𝑋 and the outcome variable via the func-tion 𝑔0 𝐷, 𝑋 . Both of these functions are unknown (except for the case of RCTs, where 𝑚0 𝑋 is known) and potentially complicated. Fortunately, we can employ ML methods to learn them.
 

 
Our construction of the efficient estimator for the APE/ATE will be based upon the relation6
𝜃0 = E[𝜑0(𝑊)],	(9.3.3)
where
𝜑0(𝑊) = 𝑔0(1, 𝑋) − 𝑔0(0, 𝑋) + (𝑌 − 𝑔0(𝐷, 𝑋))𝐻0
 

6: This representation is known as "doubly robust" parameterization, which refers to the fact that 𝜃0 is recovered whenever the 𝑔 or 𝐻 is specified correctly. We don’t dwell on this property here – for us, only the Neyman orthogonality prop-erty is important.
 
and
 

1(𝐷 = 1)	 1(𝐷 = 0)
 
𝐻0 =
 
𝑚0(𝑋) − 1 − 𝑚0(𝑋)
 
is the Horvitz-Thompson transformation.















Recall we introduced Neyman or-thogonality in Chapter 4. We con-tinue this discussion formally in Section 9.4.


The construction provided in (9.3.1) is equally applicable in cases where the propensity score P 𝐷 = 1 𝑋 is known, as in stratified randomized experiments, and in cases where the
propensity score is unknown. When the propensity score is known, the role of regression adjustment in (9.3.1) is to reduce estimation noise.
We will employ the Neyman orthogonal parameterization and cross-fitting to construct a high-quality estimator and perform statistical inference on the target parameter.

DML for APEs/ATEs in IRM
1. Partition sample indices into random folds of ap-proximately equal size: {1, ..., 𝑛} = ∪𝐾  𝐼𝑘. For each
𝑘=1
𝑘 = 1, ..., 𝐾, compute estimators 𝑔ˆ[𝑘] and 𝑚ˆ [𝑘] of the
 


conditional expectation functions 𝑔0 and 𝑚0, leaving out the 𝑘-th block of data, such that 𝜖 ≤ 𝑚ˆ [𝑘] ≤ 1 − 𝜖, and for each 𝑖 ∈ 𝐼𝑘 compute
𝜑ˆ (𝑊𝑖) = 𝑔ˆ[𝑘](1, 𝑋𝑖) − 𝑔ˆ[𝑘](0, 𝑋𝑖) +(𝑌𝑖 − 𝑔ˆ[𝑘](𝐷𝑖 , 𝑋𝑖))𝐻ˆ 𝑖
with	1(𝐷 = 1)  1(𝐷 = 0) 
𝐻ˆ 𝑖 =	𝑖	−	𝑖	.
𝑚ˆ [𝑘](𝑋𝑖)	1 − 𝑚ˆ [𝑘](𝑋𝑖)
2.	Compute the estimator
𝜃ˆ = 𝔼𝑛[𝜑ˆ (𝑊)].
3.	Construct standard errors via
JVˆ/𝑛,	Vˆ = 𝔼𝑛[𝜑ˆ (𝑊) − 𝜃ˆ]2 ,
and use standard normal critical values for inference.


 


 
The condition on the quality of estimators of 𝑔0 and 𝑚0 provides a possibility of "trading off" the quality of each estimator while retaining the adaptive inference property. The better we estimate the propensity score 𝑚0, the worse our estimate of the regression function 𝑔0 can be; and vice versa.

DML Inference for GATEs and ATETs
As discussed in Chapter 5, we may also be interested in average effects for interesting subpopulations such as group ATEs (GATEs) or average treatment effect on the treated (ATET). Recall that a GATE is defined as the average treatment effect within a group:
𝜃0 = E[𝑔0(1, 𝑋) − 𝑔0(0, 𝑋) | 𝐺 = 1],
where 𝐺 is a group indicator. For example, we might be inter-ested in the impact of a vaccine on teenagers, in which case we could set 𝐺 = 1(13 ≤ Age ≤ 19), or on older individuals, in
which case we might set 𝐺 = 1(65 ≤ Age).
GATEs are of interest for describing heterogeneity of the average treatment effects across groups. This parameter also has a predictive interpretation in a non-causal sense: It measures the average change in prediction as 𝐷 switches from 0 to 1, averaging over characteristics of the group 𝐺 = 1.
Another common target parameter ATET:
𝜃0 = E[𝑔0(1, 𝑋) − 𝑔0(0, 𝑋) | 𝐷 = 1].
In business applications, the ATET is often of the interest for attribution calculations. For example, if the treatment of interest is having experience with a new product, the ATET captures the effect of the new product on those that actually received it.
 


DML estimation and inference for GATEs can be carried out similarly to estimation and inference for the ATE by exploiting the relation
𝜃0 = E[𝜑0(𝑋) | 𝐺 = 1] = E[𝜑0(𝑋)𝐺]/P(𝐺 = 1).
We provide further detail for DML estimators of GATEs and ATETs in Section 9.4.

Remark 9.3.3 (Misspecification of PLM as inference on an overlap-weighted APE) In the case of binary treatment 𝐷 0, 1 , the IRM (Eqs. 9.3.1 and 9.3.2) generalizes the PLM of Section 9.2 (Eq. 9.2.1) by permitting interaction between the treatment and controls. The PLM, nonetheless, admits a very simple estimator for the treatment coefficient via partialling
out: simply regress cross-fitted outcome residuals on cross-fitted treatment residuals, never dividing by propensity scores. What does this get at, however, when the PLM fails to hold?
Per Remark 9.2.2, we need only consider the BLP of 𝑌˜ in
terms of 𝐷˜ in the more general IRM. Writing
𝑔0(𝐷, 𝑋) = 𝑔0(0, 𝑋) + 𝐷(𝑔0(0, 𝑋) − 𝑔0(1, 𝑋)),
we see that
𝑌˜ = 𝐷˜ (𝑔0(1, 𝑋) − 𝑔0(0, 𝑋)) + 𝜖.
Since E[𝐷˜ 2 | 𝑋] = 𝑚0(𝑋)(1 − 𝑚0(𝑋)), we find that the esti-

𝛽 = E[𝑚0(𝑋)(1 − 𝑚0(𝑋))(𝑔0(1, 𝑋) − 𝑔0(0, 𝑋))] .
E[𝑚0(𝑋)(1 − 𝑚0(𝑋))]
That is, the APE on the population reweighted by 𝑚0(𝑋)(1 −
𝑚0(𝑋))/E[𝑚0(𝑋)(1 − 𝑚0(𝑋))]. These weights are known as overlap weights as they upweight when 𝑚0(𝑋) is close to 1/2 and downweight when 𝑚0(𝑋) is close to 0 or 1.
In the case of a continuous univariate treatment on 0, 1 , we can leverage the same idea of writing 𝑔0 𝐷, 𝑋 as a baseline plus the effect of 𝐷 using the fundamental theorem of calculus:

derivative in the first argument. We can then find that 𝛽
identifies the weighted average derivative
𝛽 = E[𝑤(𝐷, 𝑋)𝑔0′ (𝐷, 𝑋)]/E[𝑤(𝐷, 𝑋)]
 

 

 
The Effect of 401(k) Eligibility on Net Financial Assets
Here we re-analyze the impact of 401(k) eligibility on financial assets (Poterba et al., [6] and [7]). The data covers a short period a few years after the introduction of 401(k)’s when they were rapidly increasing in popularity.
The key problem in determining the effect of 401(k) eligibility is that working for a firm that offers access to a 401(k) plan is not randomly assigned. To overcome the lack of random assignment, we follow the strategy developed in [6] and [7]. In these papers, the authors use data from the 1991 Survey of Income and Program Participation and argue that eligibility for enrolling in a 401(k) plan in this data can be taken as exogenous after conditioning on a few observables of which the most important for their argument is income.
The basic idea of their argument is that, at least around the time 401(k)’s initially became available, people were unlikely to be basing their employment decisions on whether an employer offered a 401(k) but would instead focus on income and other aspects of the job. Following this argument, whether one is eligible for a 401(k) may then be taken as exogenous after ap-propriately conditioning on income and other control variables related to job choice.
A key component of the argument underlying the exogeneity of 401(k) eligibility is that eligibility may only be taken as exogenous after conditioning on income and other variables related to job choice that may correlate with whether a firm offers a 401(k). [6] and [7] and many subsequent papers adopt this argument but control for parsimonious, pre-specified functions of what they deem to be relevant characteristics. One might wonder whether such specifications are able to adequately control for income and other related confounders. At the same time, the power to learn about treatment effects decreases as
 




The Notebooks 9.6.3 provide appli-cation of DML inference to learn predictive/causal effects of 401(K) eligibility on net financial wealth.


















Compare this argument to the one given below using DAGs.
 

𝐷   𝑌	𝐷   𝑌
𝑋	𝑋
𝐷   𝑌




Figure 9.6: Three Causal DAGs for analysis of the 401(K) example in which adjusting for 𝑋 is a valid identification strategy. The bottom figure encompasses the other two as special cases.

 
one allows more flexible models. The principled use of flexible ML tools offers one resolution to this tension.
In what follows, we use net financial assets7 as the outcome variable, 𝑌, in the analysis. The treatment variable, 𝐷, is an indicator for being eligible to enroll in a 401(k) plan. The vector of raw covariates, 𝑋, consists of age, a self-reported male indicator, income, family size, years of education, a marital status indicator, a two-earner status indicator, a defined benefit pension status indicator, an IRA participation indicator, and a home ownership indicator.
It is useful to think about a causal diagram that represents our thinking about identification in this example. In Figure 9.6, we provide three example DAGs for 𝑌, the outcome; 𝐷, the 401(K) eligibility offer which depends on firm characteristics,
𝐹, which are not observed; and 𝑋, the worker characteristics. In one structure, 𝐹 determines the workers characteristics (via the hiring decision), so we have 𝐹  𝑋. In another structure, workers determine the characteristics of the company they choose to work at, 𝑋  𝐹. Finally, in the last structure 𝐹, 𝑋, and 𝐷 are jointly determined by a set of latent factors 𝑈. In any of these cases, 𝑋 is a valid adjustment set because it is the only parent of 𝑌 (other than 𝐷).
It is also useful to consider structures that would break down the identification strategy. We illustrate two such structures in Figures 9.7 and 9.8. In these figures, we introduce a node for the employer match amount, 𝑀,8 which could mediate the effect of 401(k) eligibility and have an important effect on financial wealth.
 



7: Defined as the sum of IRA balances, 401(k) balances, check-ing accounts, U.S. saving bonds, other interest-earning accounts in banks and other financial institu-tions, other interest-earning assets (such as bonds held personally), stocks, and mutual funds less non-mortgage debt.


You can explore these DAG struc-tures in the Notebooks 9.6.2.














8: Employers often offer a benefit where they will match a proportion of an employee’s contribution to their 401k, up to a limit. The limit is referred to as the employer match amount.
 

𝑌



Figure 9.7: A DAG Structure where adjusting for 𝑋 is not sufficient for identifying the causal effect from
𝐷 to 𝑌. If there were no arrow from
𝐹 to 𝑀, adjusting for 𝑋 would suf-ficient.
Figure 9.8: Another DAG Structure where adjusting for 𝑋 is not suffi-cient for identifying the causal ef-fect from 𝐷 to 𝑌. Here the latent confounder 𝑈 affects all variables, so even in the absence of an arrow connecting 𝐹 to 𝑀, causal effects cannot be determined after adjust-ing for 𝑋. The presence of such la-tent confounders is always a threat to causal interpretability of any ob-servational study.

In Figure 9.7, we suppose that 𝑀 is determined by unobserved firm characteristics, 𝐹, and worker characteristics, 𝑋. In this case, adjustment for 𝑋 is not sufficient for identifying the causal effect from 𝐷 to 𝑌 as there is a path from latent firm characteristics, which are related to the treatment, to the outcome that is not closed by 𝑋. However, if 𝑀 is determined solely by 𝐷 and 𝑋, so the red arrow is erased, adjustment for 𝑋 is sufficient. Therefore, interpreting the target parameter of our estimation strategy as a causal effect is only valid if the match amount is independent of 𝐹 given 𝐷 and 𝑋, that is, if there is no arrow from 𝐹 to 𝑀 in the graph. Otherwise, the default interpretation is that we are estimating predictive effects of 401(k) eligibility.
In the second example, Figure 9.8, we maintain the assumption that 𝑀 is independent of 𝐹 given 𝐷 and 𝑋 by eliminating the arrow between nodes 𝐹 and 𝑀. However, we now allow for the possibility that latent variables 𝑈 have a direct effect on 𝑌. That is, we have an unobserved confounder or omitted variable. In this example, such a counfounder may be unobserved risk preferences that relate to an individual’s preference over jobs, an individual’s characteristics, and also have direct effects on savings decisions not channeled purely through observed indi-vidual or job characteristics. In general, the possibility of latent confounders always poses a challenge to obtaining estimates of causal effects in non-experimental data. The presence or absence of latent confounders cannot be determined solely from the data in general, and thus their presence must be argued against
 


Table 9.4: Estimated Effect of 401(k) Eligibility on Net Financial Assets





















based on scientific and institutional knowledge in different contexts. See, e.g., discussion in the original papers, [6] and [7], underlying this example. As in the previous example, we must interpret our estimates as predictive effects of 401(k) eligibility if we believe the connection from 𝑈 to 𝑌 exists.
In Table 9.4, we report DML estimates of ATE of 401(k) eligibility on net financial assets both in the partially linear model and the interactive regression model allowing for heterogeneous treatment effects. To reduce the disproportionate impact of extreme propensity score weights in the interactive model, we trim the propensity scores at 0.01 and 0.99.
Turning to the results, it is first worth noting that when no controls are used, the estimated ATE of 401(k) eligibility on net financial assets is $19,559 with an estimated standard error of 1413. Of course, this number is not a valid estimate of the causal effect of 401(k) eligibility on financial assets if there are neglected confounding variables as suggested by [6] and [7]. When we turn to the estimates that flexibly account for confounding reported in Table 9.4, we see that they are substantially attenuated relative to this baseline that does not account for confounding, suggesting much smaller causal effects of 401(k) eligibility on financial asset holdings.
 

 
It is interesting and reassuring that the results obtained from the different flexible methods are broadly consistent with each other. Given that the predictive performance of the methods is relatively similar, this similarity is consistent with the the-ory that suggests that results obtained through the use of orthogonal estimating equations and any method that provides sufficiently high-quality estimates of the necessary nuisance functions should be similar. Finally, it is interesting that these results are also broadly consistent with those reported in the original work of [6] and [7] which used a simple, intuitively-motivated functional form, suggesting that this intuitive choice was sufficiently flexible to capture much of the confounding variation in this example.
Finally, we can conclude the discussion with a more sobering note that there are credible deviations in the graph structure (e.g. unobserved firm characteristics may affect the match amount) that challenge causal interpretation of the estimates. One ap-proach to dealing with such deviations would be to conduct thorough sensitivity analysis.9 We discuss an approach to sensitivity analysis in the DML framework in Chapter 12.

9.4	Generic Debiased (or Double) Machine Learning
Key Ingredients
As a general framework, we consider DML estimation and inference based upon a method-of-moments estimator for some low-dimensional target parameter 𝜃0 based upon the empirical analog of the moment condition
E[𝜓(𝑊; 𝜃0, 𝜂0) = 0].	(9.4.1)
In (??), 𝜓 is the score function, 𝑊 denotes a data vector, 𝜃0 denotes the true value of a low-dimensional parameter of interest, and
𝜂 denotes nuisance parameters with true value 𝜂0.
 























9: We have done some informal simulations to assess the impact of this threat using the that firms match up to 5% of employees’ salary. In this scenario, we estimate the size of the bias to be in the ball park of 10%. Given this, we believe the results reported here are reason-able approximations to the causal effects.
 

The first key input of the generic DML procedure is using a score function 𝜓(𝑊; 𝜃, 𝜂) such that (i)
M(𝜃, 𝜂) = E[𝜓(𝑊; 𝜃, 𝜂)]
 


identifies 𝜃0 when 𝜂 = 𝜂0 – that is,
M(𝜃, 𝜂0) = 0 if and only if 𝜃 = 𝜃0−
and (ii) the Neyman orthogonality condition –
0 − .	(9.4.2)
𝜕𝜂M(𝜃0, 𝜂)1𝜂=𝜂0 =
is satisfied.

Here, (9.4.2) ensures that the moment condition (9.4.1) used to identify and estimate 𝜃0 is insensitive to small perturbations of the nuisance function 𝜂 around 𝜂0.

Using a Neyman orthogonal score eliminates the first order biases arising from the replacement of 𝜂0 with a ML estimator
𝜂0. Eliminating this bias is important because estimators 𝜂0 must
be heavily regularized in high-dimensional settings, so these estimators will be biased in general. The Neyman orthogonality property is responsible for the adaptivity of these estimators – namely, their approximate distribution will not depend on the fact that the estimate 𝜂0 contains error as long as the error is sufficiently mild.

 


The second key input is the use of high-quality machine learning estimators of the nuisance parameters. A sufficient condition in the examples given includes the requirement
𝑛1/4∥𝜂ˆ − 𝜂0∥𝐿2 ≈ 0.

Different structured assumptions on 𝜂0 allow us to use different machine-learning tools for estimating 𝜂0. For instance,
1)	approximate sparsity for 𝜂0 with respect to some dic-tionary calls for the use of Lasso, post-Lasso, or other sparsity-based techniques;
2)	well-approximability of 𝜂0 by trees calls for the use of
regression trees and random forests;
3)	well-approximability of 𝜂0 by sparse deep neural nets calls for the use of ℓ1-penalized deep neural networks;
4)	well-approximability of 𝜂0 by at least one model men-
tioned in 1)-3) above calls for the use of an ensemble/best choice method over the estimation methods mentioned in 1)-3).
There are performance guarantees for most of these ML methods that make it possible to satisfy the sufficient rate condition
𝑛1/4 𝜂  𝜂0 𝐿2	0. We note that many of these convergence guarantees rely on constructions and tuning choices that do not necessarily align with the way these methods are often applied in practice. There thus remains more work to be done in understanding the behavior of ML estimators. Finally, the
use of Ensemble and best choice methods ensures that the performance guarantee is no worse than the performance of the best method.

The third key input is to use sample splitting were nuisance functions are estimated on different data than are used in their evaluation when producing the estimator of the main parameter 𝜃0. The use of sample splitting allows us to avoid biases arising from overfitting.

Overfitting can easily occur when using highly complex fitting methods such as boosting, random forests, deep nets, ensem-bles, and other hybrid machine learning methods. We may heuristically think of overfitting as capturing noise that is par-ticular to the observations used to fit a model in addition to signal. Using overfit estimates of nuisance parameters obtained
 


using the same data as used to estimate the target parameter then heuristically leads to estimation error in these parameters being correlated to outcomes which introduces a type of bias. This bias can be very large, as illustrated in Figure 9.2. We specifically use cross-fitted forms of the empirical moments, as detailed in the Generic DML Algorithm below, in estimation of 𝜃0 to avoid this problem.

Neyman Orthogonal Scores for Regression Problems

Scores for the Partially Linear Regression Model. In the PLM, we employ the score function

 
𝜓(𝑊; 𝜃,𝜂) :=
{𝑌 − ℓ(𝑋) − 𝜃(𝐷 − 𝑚(𝑋))}(𝐷 − 𝑚(𝑋)),
 

(9.4.3)
 
where 𝑊 = 𝑌, 𝐷, 𝑋 are observable variables, and 𝜂 is the nuisance parameter 𝜂 = ℓ , 𝑚 with true value 𝜂0 = ℓ0, 𝑚0 . Here, ℓ and 𝑚 are square-integrable functions mapping the
support of 𝑋 to ℝ whose true values are given by
ℓ0(𝑋) = E[𝑌 | 𝑋],	𝑚0(𝑋) = E[𝐷 | 𝑋].
The score above is Neyman orthogonal by elementary calcula-tions delegated to Section 9.B. The objects 𝑌 ℓ 𝑋 and 𝐷 𝑚 𝑋 in the PLM score function (9.4.3) are also clearly the flexible analogs of taking residuals from linear models discussed in Chapter 1.
Scores for Interactive Regression Model. For estimation of the ATE parameter in the IRM model, we employ the score

 
𝜓1(𝑊; 𝜃, 𝜂) := (𝑔(1, 𝑋) − 𝑔(0, 𝑋))
+ 𝐻(𝐷, 𝑋)(𝑌 − 𝑔(𝐷, 𝑋)) − 𝜃,
 

(9.4.4)
 
where
 𝐷 	 (1 − 𝐷) 
𝐻(𝐷, 𝑋) :=	−	,	(9.4.5)
𝑚(𝑋)	1 − 𝑚(𝑋)
𝑊 = (𝑌, 𝐷, 𝑋) are observable variables, and 𝜂 := (𝑔, 𝑚) is the nuisance parameter with true value 𝜂0 = (𝑔0, 𝑚0). Here, 𝑔 is a square-integrable function mapping the support of (𝐷, 𝑋) to ℝ, and 𝑚 is a function mapping the support of 𝑋 to (𝜀, 1 − 𝜀) for
 


some 𝜀 ∈ (0, 1/2). The true values of 𝑔 and 𝑚 are given by
𝑔0(𝐷, 𝑋) = E[𝑌 | 𝐷, 𝑋],	𝑚0(𝑋) = P[𝐷 = 1 | 𝑋].	(9.4.6)
The score above is Neyman orthogonal by elementary calcula-tions delegated to Section 9.B.
For estimation of GATEs, we use the score
𝜓(𝑊; 𝜃, 𝜂) := 𝐺 𝜓1(𝑊; 𝜃, 𝜂);	(9.4.7)
where 𝐺 denotes the group membership indicator, the nuisance parameter 𝜂 is 𝑔, 𝑚, 𝑝 with true value 𝜂0 = 𝑔0, 𝑚0, 𝑝0 for 𝑔0 and 𝑚0 defined in (9.4.6) and 𝑝0 = P 𝐺 = 1 , and 𝜓1 is the score for the ATE parameter defined in (9.4.4).
For estimation of the ATET parameter, we use the score
𝜓(𝑊; 𝜃, 𝜂) := 𝐻(𝐷, 𝑋) 𝑚(𝑋)(𝑌 − 𝑔(0, 𝑋)) − 𝐷𝜃 ,	(9.4.8)

where 𝐻(𝐷, 𝑋) is given in (9.4.5), and 𝜂 = (𝑔, 𝑚, 𝑝) is the nuisance parameter with the true value 𝜂0 = (𝑔0, 𝑚0, 𝑝0) for
𝑔0 and 𝑚0 defined in (9.4.6) and 𝑝0 = P(𝐷 = 1). Note that this score does not require estimating 𝑔0(1, 𝑋).
The scores for GATEs and ATET can be shown to be Neyman orthogonal by calculations similar to those in Section 9.B.

The DML Inference Method
We assume that we have a sample {𝑊𝑖 }𝑛  , modeled as i.i.d.
copies of random variable 𝑊, whose law is determined by the probability measure 𝑃.

Generic DML Algorithm
1.	Inputs: Provide the data frame {𝑊𝑖 }𝑛  , the Neyman
𝑖=1
orthogonal score/moment function 𝜓(𝑊 , 𝜃, 𝜂) that
identifies the statistical parameter of interest, and estimation method(s) for 𝜂.

2.	Train ML Predictors on Folds: Take a K-fold random partition (𝐼𝑘)𝐾	of observation indices {1, ..., 𝑛} such
𝑘=1
that the size of each fold is about the same. For each
𝑘 ∈ {1, . . . , 𝐾}, construct a high-quality machine learning estimator 𝜂ˆ[𝑘] that depends only on the
 


subset of data (𝑋𝑖)𝑖∉𝐼𝑘 that excludes the 𝑘-th fold.
3.	Estimate Moments: Letting 𝑘(𝑖) = {𝑘 : 𝑖 ∈ 𝐼𝑘 }, con-struct the moment equation estimate
Mˆ	1 I:𝑛
(𝜃, 𝜂ˆ) = 𝑛	𝜓(𝑊𝑖; 𝜃, 𝜂ˆ[𝑘(𝑖)])
𝑖=1
4.	Compute the Estimator: Set the estimator 𝜃ˆ as the solution to the equation.
Mˆ (𝜃ˆ, 𝜂ˆ) = 0.	(9.4.9)
5.	Estimate Its Variance: Estimate the asymptotic vari-ance of 𝜃ˆ by
Vˆ	1 I:𝑛
= 𝑛	[𝜑ˆ (𝑊𝑖)𝜑ˆ (𝑊𝑖)′]
𝑖=1
1  𝑛	1  𝑛
− 𝑛 I:[𝜑ˆ (𝑊𝑖)] 𝑛 I:[𝜑ˆ (𝑊𝑖)]′,
𝑖=1	𝑖=1
where
𝜑ˆ (𝑊𝑖) = −𝐽ˆ0−1𝜓(𝑊𝑖; 𝜃ˆ, 𝜂ˆ[𝑘(𝑖)])
and
1  𝑛
𝐽ˆ0 := 𝜕𝜃 𝑛 I: 𝜓(𝑊𝑖; 𝜃ˆ, 𝜂ˆ[𝑘(𝑖)]).
𝑖=1
6.	Confidence Intervals: Form an approximate (1 − 𝛼)% confidence interval for any functional 𝑐′𝜃0, where 𝑐 is a vector of constants, as
[𝑐′𝜃ˆ ± 𝑧1−𝛼/2J𝑐′Vˆ 𝑐/𝑛],
where 𝑧1−𝛼/2 is the (1 − 𝛼/2) quantile of the 𝑁(0, 1) distribution.

 


 


Properties of the General DML Estimator
We turn now to the properties of the DML estimator under the assumption of strong identification.

In the context of the PLM, the latter condition is satisfied if
E[𝐷˜ 2] is bounded away from 0, that is, if 𝐷˜ has non-trivial
variation left after partialing-out controls. In the context of the IRM, the latter condition is satisfied if the overlap condition holds.

 


 

Selection of the Best ML Methods for DML to Minimize Upper Bounds on Bias. In many problems the nuisance parameters are regression functions
𝜂𝑚 = E[𝑉𝑚 | 𝑋𝑚],	𝑚 ∈ {1, ..., 𝑀},
where 𝑉𝑚 are some response variables and 𝑋𝑚 are covariate vectors. Consider a set of ML methods enumerated by 𝑗
1, ..., 𝐽  that produce estimates 𝜂𝑚𝑗 𝑘 when applied to data
excluding the 𝑘-th fold. We have that
𝑉ˇ𝑖,𝑚 𝑗 = 𝑉𝑖 − 𝜂ˆ𝑚 𝑗[𝑘(𝑖)](𝑋𝑖),	𝑖 ∈ 𝐼𝑘 .

Selection of the Best ML Methods for DML to Minimize Bias.
▶ For each method 𝑗, compute the cross-fitted MSPEs
𝔼𝑛[𝑉ˇ 2 ].
𝑚 𝑗

▶ Select the best ML method for predicting 𝑉𝑚 via
𝑗ˆ𝑚 = arg min 𝔼𝑛[𝑉ˇ 2 ].
𝑗	𝑚 𝑗
 



▶ Use the method 𝑗ˆ𝑚 as a learner of 𝜂𝑚 in the Generic DML Algorithm.

The precise conditions may depend on the problem at hand. See Remark 9.2.3 for discussion in the context of the partially linear model.

9.5	Notes
For a detailed literature review and technical regularity condi-tions needed for each of theorems, see [2], which also gives an overview of various analytical methods for generating Neyman orthogonal scores in a wide variety of problems.
The paper [9] goes further and describes methods for generating higher-order orthogonal scores:
𝜕𝜂𝜕𝜂E[𝜓(𝜃0, 𝜂0)] = 0.
The use of higher-order orthogonal scores allows even weaker requirements for the quality of machine learning estimators of the form,
𝑛1/6∥𝜂ˆ − 𝜂0∥𝐿2 ≈ 0,
with the caveat that such higher-order orthogonal scores may not always exist for certain subsets of distributions.
The DML method, developed in Chernozhukov, Chetverikov, Demirer, Duflo, Hansen, Newey, and Robins [2], is simply a practical meta-recipe that explicitly incorporates many classical ideas from the parametric and semi-parametric econometrics and statistics literature; see, e.g., Neyman [8]; Bickel, Klassen, Ritov, Wellner [10]; Newey [11]; Robinson [12]; and Robins and Rotnitzky [13]. The intent was to combine ideas from the classi-cal semi-parametric learning literature and prediction methods from the modern machine learning literature to provide imme-diately practical methods that are ready for rigorous statistical inference on predictive and causal effects. In essence, the ap-proach can be viewed as a modernized version of the "one"-step debiasing correction proposed by Neyman; see, e.g. [14] for a review.
 


The partialling-out approach has long been employed in clas-sical econometrics. Robinson [12] was the first to employ it in the context of kernel regressions. [2] extended this approach to more modern settings where ML estimators are used for partialling out, with cross-fitting enabling the extension.
For ATE, GATEs and ATET parameters, DML (or "doubly robust" ML) reduces to the use of machine learned "doubly robust scores" with cross-fitting. The idea of using doubly robust scores (also called augmented inverse propensity score weighted scores) is due to Robins and Rotnitzky [13], but also arises as a special case of Newey’s [11] fundamental analysis.
Targeted maximum likelihood estimation (TMLE) is another general approach for building orthogonal estimators [15]. This approach relies on doing maximum likelihood estimation for a target parameter, using a least favorable parametric submodel for the parameter of interest as the likelihood function. As with DML, TMLE needs to be combined with cross-fitting in order to deal with general ML estimators to avoid overfitting. The DML and cross-fitted TMLE should generally produce first order equivalent answers under correct specification. However, using TMLE can refine the finite-sample properties.
In the context of ATE, TMLE can be seen as applying a calibrated correction to a nonlinear regression function. We regress 𝑌ˇ𝑖 =
𝑌𝑖 − 𝑔ˆ(𝐷𝑖 , 𝑋𝑖) on 𝐻ˆ 𝑖 , obtaining
𝑏ˆ = 𝔼𝑛[𝑌ˇ 𝐻ˆ ]/𝔼𝑛[𝐻ˆ 2].
Then we correct the regression function estimate by 𝑔¯ (𝐷𝑖 , 𝑋𝑖) =
𝑔 𝐷𝑖 , 𝑋𝑖	𝑏ˆ𝐻ˆ 𝑖 . This correction was first proposed by Sharfstein,
Rotnitzky and Robins [16]. The basic idea is that we know that
𝑌𝑖  𝑔 𝐷𝑖 , 𝑋𝑖 should be orthogonal to 𝐻𝑖. Thus, if our estimate of the regression function does not have this property, we can recalibrate the regression function so the property holds.
For guidance on using DML in empirical studies and on hyper-parameter tuning related to DML we refer to [17].

9.6	Notebooks
Notebook 9.6.1 (DML for Impact of Gun Ownership on Homi-cide Rates) R Notebook on DML for Impact of Gun Ownership on Homicide Rates and Python Notebook on DML for Impact of Gun Ownership on Homicide Rates provide an application of DML inference to learn predictive/causal effects of gun
 


ownership on homicide rates across U.S. counties.

Notebook 9.6.2 (Notebook on Dagitty-Based Identification in 401(K) Example) R Notebook on Dagitty-Based Identification in 401(K) Example and Python Notebook on Pgmpy-Based Identification in 401(K) Example analyze graph structures that enable identification of the causal effect of 401(K) eligibility on net financial wealth.

Notebook 9.6.3 (DML for Impact of 401(K) Eligibility on Financial Wealth) R Notebook on DML for Impact of 401(K) Eligibility on Financial Wealth and Python Notebook on DML for Impact of 401(K) Eligibility on Financial Wealth provide ap-plication of DML inference to learn predictive/causal effects of 401(K) eligibility on net financial wealth.

Notebook 9.6.4 (DML for Growth Regression Analysis) R Notebook on DML for Growth Regression Analysis and Python Notebook on DML for Growth Regression Analysis build upon the application discussed in Chapter 4 by pro-viding an application of DML inference based on ML on predictive/causal effects of countries’ initial wealth on the rate of economic growth.


9.7	Exercises
Exercise 9.7.1

Exercise 9.7.2 (Hands-on Exercise) Experiment with one of the notebooks for the partially linear models (Guns example, Guns with DNNs, or Growth example). For example,
(a)	Apply the methods to a different empirical example (e.g., Penn reemployment experiment from CI-1),
(b)	or, using the same empirical example, try to use the H20 Auto ML framework as the machine learning tool to estimate 𝑚 and ℓ functions. (See Chapter 8 H20 Auto ML to get started).
Explain what you are doing to a fellow student.

Exercise 9.7.3 (Identification (Empirical)) Study the 401(K) identification notebook that uses Dagitty. Extend it to another
 


empirical example of your choice. Explain the principles you are using to a fellow student.

Exercise 9.7.4 (Empirical Application) Study the 401(K) em-pirical analysis notebook. Extend it to another empirical example of your choice (the Penn reemployment experiment from Chapter 1, for example) or estimate ATE for 401(K) eli-gibility for a subset of low income (or high-income) workers (Group ATEs).

Exercise 9.7.5 ((Theoretical) Neyman Orthogonality) Explain to a friend the concept of Neyman orthogonality, illustrating it with one of the examples in Appendix B. Extend the cal-culations in Appendix B to verify Neyman orthogonality for the ATET score specified in (9.4.8).

Exercise 9.7.6 ((Theoretical) Neyman Orthogonality) Explain to a friend the concept of Neyman orthogonality, and explain why the formulations given in Remark 9.3.1 are not Neyman orthogonal.
 

9.A	Bias Bounds with Proxy Treatments
Here we explain the measurement error bias in the partially linear structural equation model where treatment is measured with error:
𝑌	:=	𝛼𝐺 + 𝑔𝑌(𝑋) + 𝜖𝑌;
𝐷	:=	𝐺 + 𝑔𝐷(𝑋) + 𝜖𝐷;
𝐺	:=	𝑔𝐺(𝑋) + 𝜖𝐺;
𝑋	:=	𝜖𝑋;
where 𝜖’s are independent and centered. The second equation states that 𝐷 is generated as a proxy for the actual treatment 𝐺 using a partially linear structure. In partialled-out form

 
𝑌˜
𝐷˜
𝐺˜
The projection of 𝑌˜ on 𝐷˜
 
:=	𝛼𝜖𝐺 + 𝜖𝑌;
:=	𝜖𝐺 + 𝜖𝐷;
:=	𝜖𝐺.
recovers the projection coefficient:
 
𝛽 = E[𝑌˜ 𝐷˜ ]/E[𝐷˜ 2] = 𝛼E[𝜖2 ]/(E[𝜖2 ] + E[𝜖2 ]).
It follows that there is attenuation bias in the estimable quantity
𝛽 relative to the target parameter 𝛼:
|𝛽| < |𝛼|.
As the proxy error E[𝜖2 ] becomes small, the difference between
 
𝐷
𝛽 and 𝛼
 
[𝜖 ] → 0, then 𝛽 →
 
𝛼.
 
becomes small. Specifically, if E	𝐷
 
If we somehow knew that
𝑅2	:= E[𝜖2 ]/(E[𝜖2 ] + E[𝜖2 ]) ≥ 𝑟 −
 
𝐷˜ ∼𝐺˜
 
𝐺	𝐺	𝐷
 
that is, if we knew that the true treatment 𝐺 explains at least
𝑟 of the variance of the proxy treatment 𝐷 – then we could construct the upper and lower bound on 𝛼 from 𝛽. E.g. when
𝛽 > 0, we would have
𝛽 ≤ 𝛼 ≤ 𝛽/𝑅2	= (1/𝑟)𝛽.
 

9.B	Illustrative Neyman Orthogonality Calcuations
The Score in the Partially Linear Model. Consider the score for the PLM given in (9.4.3). We have that
E[𝜓(𝑊; 𝛽0, 𝜂0)] = 0
by definition of 𝛽0 of 𝜂0. Let 𝑈 = 𝑌  ℓ0 𝑋   𝐷  𝑚0 𝑋 𝛽0 . Then, for any 𝜂 = 𝑚, ℓ that are square integrable, the Gateaux derivative in the direction
Δ = 𝜂 − 𝜂0 = (𝑚 − 𝑚0, ℓ − ℓ0)
is given by
𝜕𝜂E[𝜓(𝑊; 𝛽0, 𝜂0)][Δ]
= −E r𝑈(𝑚(𝑋) − 𝑚0(𝑋))l
− E r((𝑚(𝑋) − 𝑚0(𝑋))𝛽0 + (ℓ (𝑋) − ℓ0(𝑋)))(𝐷 − 𝑚0(𝑋))l

by the law of iterated expectations since E[𝐷 − 𝑚0(𝑋) | 𝑋] = 0 and E[𝑈 | 𝐷, 𝑋] = 0.

The Score for IRM. Consider the score for the ATE in the IRM given in (9.4.4). We have that
E[𝜓(𝑊; 𝜃0, 𝜂0)] = 0
by definition of 𝜃0 and 𝜂0. Also, for any 𝜂 = 𝑔, 𝑚 that are square integrable with 1 𝑚  1 1  𝑚 uniformly bounded, the Gateaux derivative in the direction
Δ = 𝜂 − 𝜂0 = (𝑔 − 𝑔0, 𝑚 − 𝑚0)
 

is given by
𝜕𝜂E[𝜓(𝑊; 𝜃0, 𝜂0)][Δ]
= E r 𝑔(1, 𝑋) − 𝑔0(1, 𝑋)l
− E 𝑔(0, 𝑋) − 𝑔0(0, 𝑋)
E  𝐷(𝑔(1, 𝑋) − 𝑔0(1, 𝑋))
𝑚0(𝑋)
E (1 − 𝐷)(𝑔(0, 𝑋) − 𝑔0(0, 𝑋))
1 − 𝑚0(𝑋)
E  𝐷(𝑌 − 𝑔0(1, 𝑋))(𝑚(𝑋) − 𝑚0(𝑋))
𝑚2(𝑋)
E (1 − 𝐷)(𝑌 − 𝑔0(0, 𝑋))(𝑚(𝑋) − 𝑚0(𝑋))
(1 − 𝑚0(𝑋))2
which is 0 by the law of iterated expectations since E[𝐷 | 𝑋] =
𝑚0(𝑋), E[1 − 𝐷 | 𝑋] = 1 − 𝑚0(𝑋), E[𝐷(𝑌 − 𝑔0(1, 𝑋)) | 𝑋] = 0, and E[(1 − 𝐷)(𝑌 − 𝑔0(0, 𝑋)) | 𝑋] = 0.
 


Bibliography



[1]	Jerzy Neyman. ‘𝐶 𝛼 tests and their use’. In: Sankhya¯: The Indian Journal of Statistics, Series A (1979), pp. 1–21 (cited on page 237).
[2]		Victor Chernozhukov, Denis Chetverikov, Mert Demirer, Esther Duflo, Christian Hansen, Whitney Newey, and James Robins. ‘Double/debiased machine learning for treatment and structural parameters’. In: Econometrics Journal 21.1 (2018), pp. C1–C68 (cited on pages 241, 242,
256, 269, 271, 272).
[3]	Philip J. Cook and Jens Ludwig. ‘The social costs of gun ownership’. In: Journal of Public Economics 90 (2006),
pp. 379–391 (cited on page 248).
[4]		Jeffrey M. Wooldridge. ‘Two-Way Fixed Effects, the Two-Way Mundlak Regression, and Difference-in-Differences
Estimators’. In: Available at SSRN: https://ssrn.com/abstract=3906345 or http://dx.doi.org/10.2139/ssrn.3906345 (2021) (cited on
page 248).
[5]		Joshua D. Angrist and Alan B. Krueger. ‘Empirical Strate-gies in Labor Economics’. In: Handbook of Labor Economics. Volume 3. Ed. by O. Ashenfelter and D. Card. Elsevier: North-Holland, 1999 (cited on page 259).
[6]		James M. Poterba, Steven F. Venti, and David A. Wise. ‘401(k) Plans and Tax-Deferred savings’. In: Studies in the Economics of Aging. Ed. by D. A. Wise. Chicago, IL: University of Chicago Press, 1994, pp. 105–142 (cited on pages 259, 262, 263).
[7]	James M. Poterba, Steven F. Venti, and David A. Wise. ‘Do 401(k) Contributions Crowd Out Other Personal Saving?’ In: Journal of Public Economics 58.1 (1995), pp. 1–32 (cited on pages 259, 262, 263).
[8]	Jerzy Neyman. ‘Optimal asymptotic tests of composite hypotheses’. In: Probability and Statistics (1959), pp. 213–234 (cited on pages 264, 271).
[9]	Lester Mackey, Vasilis Syrgkanis, and Ilias Zadik. ‘Or-thogonal machine learning: Power and limitations’. In: International Conference on Machine Learning. PMLR. 2018,
pp. 3375–3383 (cited on page 271).
 

 
Bibliography
 
279
 


[10]	Peter J. Bickel, Chris A.J. Klaassen, Ya’acov Ritov, and Jon
A. Wellner. Efficient and Adaptive Estimation for Semipara-metric Models. Johns Hopkins University Press Baltimore, 1993 (cited on page 271).
[11]	Whitney K. Newey. ‘The asymptotic variance of semipara-metric estimators’. In: Econometrica 62.6 (1994), pp. 1349–
1382 (cited on pages 271, 272).
[12]	Peter M. Robinson. ‘Root-𝑁-consistent semiparametric regression’. In: Econometrica 56.4 (1988), pp. 931–954. doi:
10.2307/1912705 (cited on pages 271, 272).
[13]	James M. Robins and Andrea Rotnitzky. ‘Semiparametric efficiency in multivariate regression models with missing data’. In: Journal of the American Statistical Association
90.429 (1995), pp. 122–129 (cited on pages 271, 272).
[14]		Victor Chernozhukov, Christian Hansen, and Martin Spindler. ‘Valid post-selection and post-regularization inference: An elementary, general approach’. In: Annual Review of Economics 7.1 (2015), pp. 649–688 (cited on
page 271).
[15]		Mark J. van der Laan and Sherri Rose. Targeted Learn-ing: Causal Inference for Observational and Experimental Data. Springer Science & Business Media, 2011 (cited on page 272).
[16]		Daniel O. Scharfstein, Andrea Rotnitzky, and James M. Robins. ‘Adjusting for Nonignorable Drop-Out Using Semiparametric Nonresponse Models’. In: Journal of the American Statistical Association 94.448 (1999), pp. 1096–1120. (Visited on 01/25/2023) (cited on page 272).
[17]		Philipp Bach, Oliver Schacht, Victor Chernozhukov, Sven Klaassen, and Martin Spindler. Hyperparameter Tuning for Causal Inference with Double Machine Learning: A Simulation Study. 2024 (cited on page 272).
 

 
Feature Engineering for Causal and Predictive
Inference

"It’s all about paying attention. [...] Attention is vitality. It connects you with others."
– Susan Sontag [1].










Here we discuss feature engineering as an approach to trans-form complex objects such as text and images into a collection of relatively low-dimensional numerical features (embeddings) that can be used for standard predictive or causal applications, for example as regressors in a prediction problem. We consider principal components, autoencoders and neural networks as general approaches to generate embeddings. We then consider text embeddings in detail, introducing two popular neural network-based Natural Language Processing (NLP) algorithms: ELMo and BERT. We finally consider image embeddings, ap-plying a hedonic price model to apparel data using a neural network algorithm (ResNet50) to generate embeddings.
 
10
10.1	Introduction	281
10.2	From Principal Components to Autoencoders	282
Variational  Autoen-coders	286
10.3	From Autoencoders to Gen-eral Embeddings	287
10.4	Text Embeddings	288
Revisiting the Price Elastic-ity for Toy Cars	299
10.5	Image Embeddings	301
10.6	Application:  Hedonic Prices	303
10.7	Notes	306
10.8	Notebooks	306
10.9	Exercises	307
 
10.1	Introduction
 

Thus far, we have imposed a significant restriction on the kinds of data on which we can perform inference. While empiricists often consider simple datasets that include variables that have a numeric representation (binary, factor and continuous vari-ables), researchers are increasingly confronted with complex forms of data, such as images and text, that encode a vast amount of information. In this section, we generalize our approach to allow using these types of data.
As a motivating example, we consider the problem of predicting prices of products using the types of characteristics that one might find on a webpage, namely the text in the product description and the product’s image. The resulting predicted prices are called hedonic prices, and predictive modeling of this form is motivated by the hedonic price models of economics.
In order to predict prices, we have to convert text and images into relatively low-dimensional numerical features, called em-beddings or encodings. The minimal requirement on embeddings is that similar products should have similar embeddings. This requirement guarantees that price predictions for similar prod-ucts are also similar. The maximal requirement on embeddings is that they should parsimoniously approximate as much in-formation as possible from text and images that is relevant for price predictions.
The main methods for generating successful embeddings in-clude the following, in order of increasing generality:
▶ classical principal component analysis,
▶ autoencoders, and
▶ neural networks solving auxiliary prediction tasks.
The auxiliary tasks in the final method may include solving image processing problems, such as object classification and image compression, or natural language processing problems, such as summarization and machine translation.
These auxiliary tasks are not the same as the "main" task. In our price prediction example, the main task is predicting product prices. Before turning to the primary price prediction task, we might consider running our image and text data through neural networks designed to perform well on image classification or natural language processing. We can then extract features of these networks – embeddings – to use as summaries of the image and text data that are useful for predicting the image type and semantic context of the text. For example, we could
 

















































For example, one might use ResNet 50, a pretrained residual network of depth 50, which performs well on image classification tasks and/or a language processing neural net-work such as BERT. We will discuss such neural networks later in this chapter.
 


 
take the final hidden layer of neurons in the simple deep neural networks discussed in Chapter 8 as our embeddings as these neurons are the constructed features that are used in forming the final prediction for the outcome of interest – image type or a language processing task. These embeddings then provide useful inputs for solving the auxiliary object classification or text description task. Since product type and product description are likely relevant determinants of price, these embeddings produced by the auxiliary tasks can serve as useful inputs to the main task – price prediction.
Embeddings are useful in a variety of predictive and causal inference problems. For example, we can imagine using
▶ embeddings of product images and descriptions for mod-eling variety and demand for products,
▶ embeddings of text resumes for studying the wage offer structure,
▶ or embeddings of countries’ characteristics for studying the effect of institutions.
There is an emerging literature on the use of embeddings for causal inference; see this repository of papers about using text data in causal inference. See also [2] for a recent review article on the importance and subtleties of using text as data in the social sciences.

10.2		From Principal Components to Autoencoders
Principal components are an early classical example of embed-dings. One way to frame principal components is that principal components find unit length orthogonal linear combinations, directions, of a collection of variables that are "best" at reproduc-ing the underlying data. The idea is then that a small number of principal components should capture most of the variabil-ity in the original variables and thus may provide a useful low-dimensional summary of the original data.
Specifically, let 𝑊1, ..., 𝑊𝑛 be a sample of 𝑛 observations of a high-dimensional centered1 random vector 𝑊 in ℝ𝑑, and let Σ𝑛 = 𝔼𝑛 𝑊𝑊′ ℝ𝑑×𝑑 denote the empirical covariance matrix. In order to reduce the dimension of 𝑊, suppose we wish to find 𝐾 ≪ 𝑑 mutually orthogonal rotations
𝑋𝑘𝑖 := 𝑐′𝑘𝑊𝑖 ,	𝑘 = 1, ..., 𝐾,
 































https://github.com/causaltext/causal-text-papers



















1: Thus, 𝔼𝑛[𝑊𝑗 ] = 0 for 𝑗 = 1, ..., 𝑑.
 


of the original 𝑊𝑖’s where
𝑐ℓ′ 𝑐𝑘 = 0 for ℓ ≠ 𝑘 and 𝑐′𝑘 𝑐𝑘 = 1 for each 𝑘
such that linear combinations of these variables approximate the original data. These rotations are called principal components of
𝑊𝑖. In applications, 𝑊𝑖 represent high-dimensional raw features (images, for example), and the principal components
𝑋𝐾 = (𝑋𝑖1, ...𝑋𝑖𝐾)′
represent a lower-dimensional encoding or embedding of 𝑊𝑖. More formally, we wish to solve
𝑑	𝑛
 
min
 
I: I:(𝑊𝑗𝑖 − 𝑊ˆ 𝑗𝑖)2
 

subject to
 
{𝑎𝑗 }𝑑  ,{𝑐𝑘 }𝐾
 
𝑗=1 𝑖=1
 
𝑊ˆ 𝑗𝑖 := 𝑎′𝑗 𝑋𝐾 for 𝑋𝐾 = (𝑋1𝑖 , ...𝑋𝐾𝑖)′, 𝑗 = 1, ..., 𝑑, and 𝑖 = 1, ..., 𝑛;
𝑋𝑘𝑖 = 𝑐′𝑘𝑊𝑖 for 𝑖 = 1, ..., 𝑛 and 𝑘 = 1, ..., 𝐾;
𝑐′𝑘 𝑐𝑘 = 1 for 𝑘 = 1, . . . , 𝐾;
𝑐′𝑘 𝑐ℓ = 0 for ℓ ≠ 𝑘.
The constructed variables resulting from solving this prob-lem,
𝑋𝐾 = (𝑋1𝑖 , ...𝑋𝐾𝑖)′
are the first 𝐾 principal components.


 
Another interesting feature of principal components is that they satisfy
𝔼𝑛[𝑋2] = 𝜆𝑘
 


Figure 10.1: Featurizing a tal-ented man: The original 3072-dimensional image 𝑊 and im-
 
for 𝑘 = 1, ..., 𝐾 and
 
age
 
𝑊ˆ
 
produced from a 256-
 
𝔼𝑛[𝑋𝑘 𝑋ℓ ] = 0
for ℓ ≠ 𝑘. These properties result from the fact that the 𝑐𝑘 are eigenvectors of Σ𝑛.
Finding principal components offers one way to produce em-
 
dimensional principal component embedding. As a by-product, we’ve just made an important causal dis-covery that, surprisingly, doing em-bedding causes one to be younger
;).
 


Input	Encoding	Decoding	Input	Encoding	Encoding	Decoding	Decoding
layer	layer	layer	layer	layer 1	layer 2	layer 1	layer 2


Figure 10.2: The left panel shows a linear single layer autoencoder, such as linear principal components. The right panel shows a three layer nonlinear autoencoder; the middle layers can be used as embeddings.

beddings of raw inputs. Once we have embeddings from any method, we can look at how similar the raw inputs 𝑊𝑘 and 𝑊𝑙 are via the cosine similarity of the embeddings:
sim(𝑊𝑘 , 𝑊𝑙) = 𝑋𝑘′ 𝑋𝑙/(∥𝑋𝑘 ∥∥𝑋𝑙 ∥).
In the context of product embeddings, this approach can be used, for example, to find products that are similar to a given product.

The predictive exercise underlying principal components can be seen as a linear neural network:
𝑊𝑖 −→ 𝐶𝐾′ 𝑊𝑖 =: 𝐸 −→ 𝐴′𝐸 =: 𝑊ˆ 𝑖 ,
𝑑×1	𝑘×1	𝑑×1
for 𝐴 = [𝑎1, ...𝑎𝑑]. The first step is said to be "encoding" the information in the input, and the second step is said to be "de-coding" in the sense of returning the encoded information
to the original space. Therefore, principal components are embeddings generated by a linear "encoder-decoder" net-work (an autoencoder, for short). For principal components, the relationship between the encoder and decoder happens to be rather simple, in that 𝐴 = 𝐶𝐾′ (see Remark 10.2.1).

This framing suggests that we can immediately generalize this approach to nonlinearly generated encoders and decoders that
 


have multiple layers:

 
𝑔1
𝑊
 
𝐸1...  𝑔𝑘
 
𝐸𝑘
 
𝑔𝑘+1
 
𝐷𝑘+1...  𝑔𝑚
 
𝐷𝑚 =: 𝑊ˆ ,
 
𝑖 −→	𝑖
 
−→	𝑖 −→	𝑖
 
−→	𝑖	𝑖
 
where maps 𝑔ℓ ’s are neuron-generating maps. The middle layer or layers of low dimension, represented by the 𝐸𝑘 , are taken to be encoders. The layers of neurons are mnemonically labelled as either "E" or "D," depending on whether they are doing "encoding" or "decoding," though note that there is no strict formal distinction between these types of layers.

Autoencoders are a way of discovering latent, low-dimensional structures in a dataset. In particular, a random data vector 𝑊 ∈ ℝ𝑑 can be said to have low-dimensional structure if we can find some "well-behaved" functions e : ℝ𝑑 → ℝ𝑘 and d : ℝ𝑘 → ℝ𝑑, with 𝑘 ≪ 𝑑, such that
(d(e(𝑊)) ≈ 𝑊.
In other words, 𝑋 = e(𝑊) is a parsimonious, 𝑘-dimensional representation of 𝑊 that contains all of the information necessary to approximately reconstruct the full vector 𝑊.
Traditionally, the map e(·) is called an encoder, and the map d(·) is called a decoder function. Given this, a general formulation of autoencoders is to minimize the average reconstruction loss,
𝔼𝑛[loss(𝑊 , d(e(𝑊))],
over "well-behaved" functions d ∈ D and e ∈ E. These classes are often linear, as in principal components, or generated via neural networks.

 
The qualification of "well-behaved" is important since it is always possible to write down some (completely wild) one-to-one function e : ℝ𝑑 → ℝ1 such that e−1e(𝑊) = 𝑊.2
 


2: Google "Borel Isomorphism."
 

Remark 10.2.2 (Independent Component Analysis) Princi-
pal component analysis defines "well-behaved" functions as
linear functions whose output (𝑋1, . . . , 𝑋𝑘) = 𝑒(𝑊) has un-
correlated entries, i.e. E[𝑋𝑖 𝑋𝑗] = 0. In other words, PCA tries
to find latent embedding vectors (𝑋1, . . . , 𝑋𝑘) that have the
ability to reconstruct the original covariate vectors and are
uncorrelated with each other. One could take a step further
and require that these latent embeddings are independent of
 


 

Variational Autoencoders
The encoding and decoding functions so far in our discussion have been restricted to be deterministic. Implicitly, this assumes that given the observed high-dimensional variables 𝑊, we can uniquely identify the low-dimensional variables that contain all the information in 𝑊. Such unique mapping from observed factors to latent factors is not always possible.
An important extension of the autoencoder framework is al-lowing for these mappings to be stochastic. This extension is inspired by Bayesian probabilistic modelling that views the embeddings as latent factors and imagines that the data are drawn by first drawing the latent factors and then drawing the data samples from some distribution that is dependent on the latent factors. The variational autoencoder [5] attempts to reverse engineer this problem and learn the latent factor space and the posterior distribution of the latent factors conditional on the observed data using computationally tractable approximate versions of the maximum likelihood method.
Although arrived at via different reasoning, variational autoen-coders ultimately look similar to autoencoders, albeit intro-ducing randomness in the encoding phase. Roughly speaking, variational autoencoders optimize a loss of the form:
𝔼𝑛[loss(𝑊 , d(e(𝑊 , 𝑍))] + 𝑝𝑒𝑛𝑎𝑙𝑡𝑦(e),
where 𝑍 is an exogenous jointly independent Gaussian random noise vector and the penalty term forces the encoding function to be non-deterministic and stems from derivations related to the objective of learning the posterior distribution of latent factors. Conditional on an observed 𝑊, the random variable e 𝑊 , 𝑍  𝑊 can be interpreted as a random sample from the posterior distribution of the latent factors that could have generated the observed sample 𝑊. Moreover, the function
 


𝑒 𝑊 , 𝑍 , is typically of the form 𝑒 𝑊 , 𝑍 = 𝜇 𝑊	Σ 𝑊 𝑍, where the deterministic functions 𝜇 𝑊	and Σ 𝑊		encode the mean and the covariance of the posterior distribution of the latent factors. These deterministic functions 𝜇 𝑊 , Σ 𝑊 can be viewed as deterministic embeddings of 𝑊 and can
be used as engineered features in downstream tasks; see e.g. [6]. Alternatively, one can use only 𝜇 𝑊 , which approximates the posterior mode, as the embeddings. For a more in-depth introduction to variational autoencoders, see [7].

10.3	From Autoencoders to General Embeddings
We can generalize from autoencoders by considering other loss functions where the target outcome in the loss, 𝐴, need not be
𝑊. Within this context, we can still search for embeddings that minimize average prediction loss for target 𝐴,
𝔼𝑛[loss(𝐴, f(e(𝑊))],
where the role of f is no longer just to decode (i.e. predict 𝑊) but rather to predict 𝐴.
For example, in feature engineering from images, 𝐴 could be a product type or subtype, and 𝑊 could be the image. In feature engineering from text, 𝐴 could be a masked word in a sentence and 𝑊 the sentence containing this word. These alternative approaches could be more useful in relation to the final learning task. For example, to build good hedonic price models, we may be much more interested in image or text embeddings that best help to accurately describe the type or subtype of a product than we are in embeddings that are useful for reconstructing the entire image or text itself.
Learning general embeddings with a generic target 𝐴 is gener-ally implemented via neural networks with structure

 
𝑔1
𝑊
 
𝐸1...  𝑔𝑘
 
𝐸𝑘
 
𝑔𝑘+1
 
𝐹𝑘+1...  𝑔𝑚
 
𝐹𝑚 =: 𝐴ˆ .
 
𝑖 −→	𝑖
 
−→	𝑖 −→	𝑖
 
−→	𝑖	𝑖
 
In this structure, 𝑔ℓ ’s are neuron-generating maps. The middle layers 𝐸𝑘 are taken to be embedding layers. The 𝐹𝑘’s are pre-
𝑖	𝑖
dictive layers which are meant to create good predictions of auxiliary targets.
 

10.4	Text Embeddings
First generation: Word2Vec Embeddings
We first review some basic ideas underlying the Word2Vec algorithm [8]. One way we could encode words that appear in a corpus of documents (e.g. product descriptions) into a vector is to consider a very high-dimensional vector of dimension
𝑑, where 𝑑 is the total number of words in the corpus. Then the 𝑗-th word in the corpus (e.g. in alphabetical order) can be represented as:
𝑒𝑗 = (0, .....0, 1, 0,	0)′,
with 1 in the j-th position. This encoding has a very high dimen-sion limiting its usefulness. Furthermore, this representation does not capture word similarity – e.g., cosine similarity between two different words 𝑗 and 𝑘 is always zero since 𝑒 𝑗′𝑒𝑘 = 0.
Instead we aim to represent words by vectors of much lower dimension, 𝑟, that are able to capture word similarity. We denote the representation of the 𝑗-th word by 𝑢𝑗, so the dictionary is an 𝑟 × 𝑑 matrix
𝜔 = {𝑢1, ..., 𝑢𝑑 },
where 𝑟 is the reduced dimensionality of the dictionary. This dictionary is a linear rotation of the original dictionary 𝐸 =
{𝑒1, ..., 𝑒𝑑 }, where
𝜔 = 𝜔𝐸.
Therefore, the problem of finding the rotation 𝜔 is analogous to the problem of finding principal components, except that our goal is now to find representations 𝜔 that are able to capture word similarity. Once we are done, each word 𝑡𝑗 in a human-readable dictionary can be represented by a new "word" 𝑢𝑗. The goal of Word2Vec is to find an effective representation with the dimension 𝑟 of the embedding being much smaller than the total number of words in the corpus, 𝑑. We achieve this goal by treating 𝜔 as parameters and estimating them so that the model performs well in some basic natural language processing tasks. These tasks are typically not related to downstream tasks, such as predicting hedonic prices or performing causal infer-ence using text as control features, but are related to language prediction tasks.
Figure 10.3 shows components of embeddings for several words produced by a trained Word2Vec map. The numbers presented in the table are not particularly interpretable in isolation. Each
 


 
Example of Word2Vec Features

womens	0.388	0.031	-0.197	0.180	-0.223	-0.607	0.306	-0.597
mens	0.759	0.372	0.370	0.707	-0.125	0.509	0.106	0.209
clothing	0.149	0.516	-0.028	0.218	-0.851	-0.410	0.386	0.171
shoes	1.324	-0.359	-0.008	-0.552	0.011	0.365	0.228	-0.566
women	0.601	-0.046	-0.099	0.011	-0.097	-0.605	0.256	-0.551
girls	0.417	-0.005	-0.409	-0.531	-1.319	-0.035	-0.941	-0.361
men	0.778	0.407	0.426	0.534	-0.056	0.518	0.108	0.245
boys	0.897	-0.017	-0.002	-0.182	-1.313	0.449	-0.828	0.521
accessories	0.868	-0.378	-1.248	1.541	0.324	0.283	-0.491	0.081
socks	0.276	0.354	0.186	0.301	-0.643	-0.022	0.321	0.241
luggage	0.797	1.750	-2.307	-0.560	0.031	0.921	0.417	0.313
dress	0.282	0.233	0.043	0.175	-0.501	-0.381	0.298	-0.026
baby	0.346	-0.550	-1.136	-0.044	-2.005	0.690	-1.092	0.010
jewelry	-0.316	0.348	-0.309	0.879	-0.766	1.124	-0.080	-2.039
black	0.427	0.030	-0.019	0.224	-0.162	-0.325	0.170	-0.173
boots	1.009	-0.304	0.032	-0.334	-0.096	0.111	0.118	-0.519
shirts	0.444	0.453	0.394	0.518	-0.531	0.100	0.146	0.204
shirt	0.329	0.422	0.227	0.456	-0.700	0.067	0.106	0.234
underwear	0.231	0.491	0.226	0.202	-0.774	0.005	0.229	0.310
 








Figure 10.3: Examples of words con-verted to numerical features via Word2Vec. Compare embeddings for words "shirt" and "shirts" (high-lighted in red) and for "luggage" and "dress" (highlighted in blue). The embeddings for shirt and shirts are much more similar than the em-beddings for luggage and dress.
 

column represents a "trait" and the cell entry represents the loading of the word in the row in that trait. The numbers are more useful in comparison with each other across different rows which allows us to understand word similarity. For example, we can see that the very similar words "shirt" and "shirts" have very similar embeddings while the embeddings for the seemingly relatively different words "luggage" and "dress" are quite dissimilar.
In our context, we can think of each word appearing in a datum (e.g. a product description) as a random variable 𝑇 and denote its corresponding embedding representation by 𝑈.

One of the ways to train the word embeddings is to predict the middle word from the words that surround it in word sentences.

 
Given a subsentence 𝑠 of 𝐾  1 words, we have a central word
𝑇𝑐,𝑠 whose identity we would like to predict. As predictors, we have the context words 𝑇𝑜,𝑠 that surround the central word 𝑇𝑐,𝑠. One approach for forming the prediction starts by collapsing the embeddings for context words by a sum,3
1
𝑈¯ 𝑠 =  	𝑈𝑜,𝑠 ,
𝑜
where 𝑈𝑜,𝑠 is the element of 𝜔 corresponding to the word 𝑇𝑜,𝑠. This step imposes a drastically simplifying assumption that the context words are exchangeable – i.e. the position of each word is not important.
The probability of the middle word 𝑇𝑐,𝑠 being equal to 𝑡 is
 





3: Why not? We can try it and see if it works.
 


modeled via the multinomial logit function:
𝑝 (𝑡; 𝜋, 𝜔) := 𝑃 (𝑇	= 𝑡 | {𝑇	}, 𝜋, 𝜔) =	exp(𝜋′𝑡 𝑈¯ 𝑠 (𝜔)) ,
 
𝑠	𝑐,𝑠
 
𝑜,𝑠
 
'5:¯ exp(𝜋′ 𝑈¯ 𝑠 (𝜔))
 
where 𝜋 = 𝜋1, ..., 𝜋𝑑 is an 𝑚 𝑑 matrix of parameter vectors defining the choice probabilities. The model constrains the choice probabilities 𝜋 to be 𝜔, and estimates 𝜔 using the quasi-
maximum likelihood method:
max	log 𝑝𝑠 𝑇𝑖,𝑠; 𝜋, 𝜔 ,
𝜔=𝜋 𝑠∈S
where we sum the log-probabilities over many examples S of subsentences 𝑠. Once we are done training, we can generate the embedding for the title or description of product 𝑖, containing
the embedded words {𝑈𝑗,𝑖 }𝐽	by simply averaging them:
1  𝐽
 
𝑊𝑖 =
 
𝑈𝑗,𝑖 .	(10.4.1)
𝑗=1
 

Remark 10.4.1 In summary, the Word2Vec algorithm trans-forms text into a vector of numbers that can be used to com-pactly represent words. The algorithm trains a neural network in a supervised manner such that contextual information is used to predict another part of the text.
For example, let’s say that the title description of the item is: "Hiigoo Fashion Women’s Multi-pocket Cotton Canvas Handbags Shoulder Bags Totes Purses." The model will be trained using many 𝑛-word subsentence examples, such that the center word is predicted from the rest. If we just
use 𝑛 = 3 subsentence examples, then we train the model using the following examples: (Hiigoo,Women’s) → Fash-ion, (Fashion,Multi-pocket)→ Women’s, (Women’s,Cotton)
→ Multi-pocket, and so on.
How do we judge whether the text embedding is successful or not? In the hedonic price context, we can check whether Word2Vec features improve the quality of prediction of the price by the hedonic model. We can also check if similar words 𝑇𝑘 and 𝑇𝑙 have similar embeddings. We can measure the similarity through cosine similarity:
sim(𝑇𝑘 , 𝑇𝑙) = 𝑈𝑘′ 𝑈𝑙/(∥𝑈𝑘 ∥∥𝑈𝑙 ∥) ∈ [−1, 1].
 


 
The more similar the words are, according to our human no-tion of similarity, the higher the value our formal measure of similarity should take. For example, the following are the two words that are most similar to "tie" under the similarity measure: "necktie" and "bowtie." The embeddings also induces an interesting vector space on the set of words, which seems to encode analogues well. For example, the word "briefcase" is very cosine-similar to the artificial latent word4
Artificial word = Word2Vec(men′s)
+ Word2Vec(handbag) − Word2Vec(women′s).
This similarity between a real word and our constructed latent word gives some justification for the "averaging" of embeddings to summarize whole sentences or descriptions.
Word2vec embeddings were among the first generation of early successful embedding algorithms. These algorithms have been improved by the next generation of NLP algorithms, such as ELMo and BERT, which are discussed next.

Second Generation: Sequence Models
A major advance in language modeling has been to represent text as a sequence using recurrent (autoregressive) models. Among various benefits, representing text with an autogres-sive structure allows for better capturing the context around words.
Of note is the Embeddings from Language Models (ELMo) algorithm [11], which uses the idea of the Shannon game where we aim to guess a word in a sentence, 𝑚, consisting of 𝑛 total words. Specifically, we consider the problem of predicting word
𝑘 + 1 using the preceding 𝑘 words via
 









4: This example also shows us how word embeddings very easily en-code and propagate biases that ex-ist in document corpora that are typically used in machine learning; a realization that has been high-lighted by several recent works [9]. One should always be cognizant of such inherent biases in trained embeddings. Recent works in ma-chine learning (e.g. [9]) provide au-tomated approaches that partially correct for these biases, though not completely removing the problem [10].
 
𝑓
𝑘,𝑚
 
(𝑡) = 𝑃(𝑇𝑘+1,𝑚 = 𝑡 | 𝑇1,𝑚 , ..., 𝑇𝑘,𝑚; 𝜃)
 
and similarly consider the reverse prediction via

 
𝑏
𝑘,𝑚
 
(𝑡) = 𝑃(𝑇𝑘−1,𝑚 = 𝑡 | 𝑇𝑘,𝑚 , ...𝑇𝑛,𝑚; 𝜃),
 
where 𝜃 is a parameter vector. ELMo then uses recurrent neural networks (RNNs) to model these probabilities.

On Recurrent Neural Networks A RNN is a particular architecture for sequence input and output where we use
 



neurons from the previous prediction to make the current prediction.RNNs are essentially the neural network version of linear autoregressive models, such as ARIMA models, which go back to the early work of statisticians George Box and Gwilym Jenkins [12, 13] and have also been used in economics to model volatility of financial assets in the GARCH model of economist Tim Bollerslev [14].
In its simplest form, a RNN parses inputs in a serial manner
𝑇1, . . . , 𝑇𝑡 , . . . , 𝑇𝑘 where each step 𝑡 produces a state vector
𝑆𝑡 = 𝜎(𝐴𝑇𝑡 + 𝐵𝑆𝑡−1 + 𝑐) that is a non-linear function (a set of neurons) of the current input and the previous state vector. That is, 𝜎 is an activation function as presented in Section 8.3
applied elementwise to each coordinate. Moreover, a RNN produces an output prediction vector 𝑦𝑡 = 𝜎(𝐷𝑆𝑡 + 𝑒) that is a non-linear function (a set of neurons) of the current
state. The parameters 𝐴, 𝐵, 𝑐, 𝐷, 𝑒 of all these neurons are the same (shared) across steps.
Parameters are estimated by maximizing parameterized approximate versions of the log-likelihoods of the observed data (aka quasi-likelihoods), typically referred to as quasi-maximum log-likelihood methods, where the forward and backward log quasi-likelihoods are added together.

Specifically, ELMo uses a particular form of recursive neural net-work called Long Short-Term Memory (LSTM) network. LSTMs improve upon the numerical stability of RNNs by allowing for the "state" to pass through the current step as-is, without any non-linearity applied. Allowing the state to pass through steps without alteration helps in propagating information across dis-tant steps and thus better accommodates long-term memory.
To give a simple example, suppose we wanted model word choice with a multinomial logit function, as in the previous subsection, but wanted to better grasp the positional context of the individual words. Rather than start by collapsing the embeddings for context words surrounding a target central word via a sum, we could instead keep track of word order and assign individual parameters to each context. For example, we could model the forward predicted probability of word 𝑘 in sentence 𝑚 as
 
𝑒'5:𝑘−1 𝜋′
 
𝑈𝑗,𝑚(𝜔)
 
𝑃(𝑇𝑘,𝑚 = 𝑡 | {𝑇𝑗,𝑚 }𝑘−1 , 𝜋) =
 
𝑗=1
'5:
 
𝑡,𝑗
,

 
 



Outputs
Softmax (Logit)

 
Hidden Layers


Context-free Embedding
 







𝑡1
 







𝑡2
 







𝑡3
 







𝑡4
 



Figure 10.4: ELMo Architecture. ELMo network for a string of 4
words, with 𝐿 = 2 hidden layers. Here, the softmax layer (multino-mial logit) is a single function map-
ping each input in ℝ𝑑 to a probabil-ity distribution.
 

 
and similarly model the reverse prediction problem where we recall that 𝑈𝑗,𝑚 𝜔 is the embedding corresponding to word
𝑗 in sentence 𝑚. ELMo uses a more sophisticated (and more parsimonious) recursive nonlinear regression model (specifi-cally a recurrent neural network) to build these probabilities. We illustrate a simple ELMo structure in Figure 10.4.

The basic structure of ELMo.
Given a sentence 𝑚 of 𝑛 words,
1.	Words are mapped to context-free embeddings in
ℝ𝑑.

2.	A network is trained to predict each word 𝑇𝑘,𝑚 of a string given (a) words (𝑇1,𝑚 , . . . , 𝑇𝑘−1,𝑚) (forward prediction) and (b) words (𝑇𝑘+1,𝑚 , . . . , 𝑇𝑛,𝑚) (back-ward prediction). The objective is to maximize the average over the sum of the log-likelihoods of the 2𝑛 − 2 words being predicted, where the average is taken over all sentences.

3.	The embedding of word 𝑇𝑘,𝑚 is given by a weighted average of outputs of certain hidden neurons corre-sponding to this word’s entire context. Importantly, a subset of the parameters is coupled across the forward and backward prediction problems (2a) and (2b). In particular, the first layer that goes out of the context-free embedding and the final ("softmax") layer that produces the probabilistic predictions is the same for the two prediction objectives (2a) and (2b). Thus the inputs to this layer, which represent the forward and
 




































A softmax layer assigns probabil-ities to each class in a multi-class problem. It is a multi-class general-ization of logistic regression that as-sumes mutually exclusive classes.
 



backward context, are constrained to lie in "the same space."

Training

In Figure 10.4, the output probability distribution 𝑝 𝑓 is taken as a prediction of 𝑇𝑘 1,𝑚 using words 𝑇1,𝑚 , . . . , 𝑇𝑘,𝑚 . Similarly, 𝑝𝑏 is taken as a prediction of 𝑇𝑘 1,𝑚 using words 𝑇𝑘,𝑚 , . . . , 𝑇𝑛,𝑚 . The parameters of the network, 𝜃, are obtained by maximizing the quasi-log-likelihood:
 

max
𝜃	𝑚∈M
 

𝑛−1 log
𝑘=1
 

𝑝 𝑓	(𝑇𝑘+1,𝑚;
 

𝜃) +
 
I:𝑘=2
 

log
 

𝑝𝑏	(𝑇𝑘−1,𝑚;
 
𝜃)\ ,
 
where M is a collection of sentences. In our running pricing example, M would be the collection of titles and product de-scriptions taken from product web pages.

Producing embeddings
To produce embeddings from the trained network, each word
𝑡𝑘 in a sentence 𝑚 = 𝑡1, ..., 𝑡𝑛 is mapped to a weighted average of the outputs of the hidden neurons indexed by 𝑘:

 
𝐿
𝑓
𝑘𝑖
 
+ 𝛾¯𝑖𝑤𝑘𝑖).
 
𝑖=1
The embedding for the sentence (or an entire product descrip-tion in our example) is produced by summing the embeddings for each individual word. The weights 𝛾 and 𝛾 can be tuned by the neural network performing the final task. In principle, the whole network could be plugged in to the network performing the final task and allowed to update. However, the ELMo archi-tecture and methodology is more inline with being used as a feature extractor, with only the final linear layer being trained towards the target task (in sharp contrast to the BERT model that we outline next; see e.g. [15]).

Third generation: Transformers
A subsequent major advance in language modeling has been the development and use of the transformer architecture. Going beyond backwards and forwards sequences, transformers use a
 


mechanism termed "self-attention" [16] in order to model the importance of different parts of the text in understanding any one other part. Like RNNs, this self-attention mechanism allows for understanding context; but unlike RNNs, it allows the model to better focus on potentially far away parts of the text that may be more relevant than nearby words for understanding context. For example, looking beyond the local neighborhood of say the word "it" allows understanding that "it" in one sentence refers to a particular word from a previous sentence.
An early and prominent example of transformer-based lan-guage models is Bidirectional Encoder Representations from Transformers (BERT) [17].
Unlike the language model in ELMo which predicts the next word from previous and subsequent words, the BERT model is trained on two self-supervised tasks simultaneously:
▶ Mask Language Model: Randomly mask a certain per-centage of the words in a sentence and predict the masked words.
▶ Next Sentence Prediction: Given a pair of sentences, pre-dict whether one sentence precedes another.

The basic structure of BERT.

1.	Each word in the input sentence is broken into subwords (tokenized) and each piece is called a "token." Each token is encoded using a context-free embedding called WordPiece. A special token [cls] is added to the beginning of the sequence. x% of the tokens representing individual words are replaced by [mask].
2.	For each token, its input representation consists of i) its token embedding from (1), ii) its position embedding indicating the position of the token in the sentence, and iii) its segment embedding indicating whether it belongs to sentence A or B.
3.	The input representation of tokens in the sequence is fed into the main model architecture: L layers of Transformer-Encoder blocks. Each block consists of a "multi-head attention layer" (described below), followed by a feed forward layer.
 



4. The output representation of the mask token [mask] is used to predict the masked word via a softmax layer, and the output representation of the special [cls] token is used for Next Sentence Prediction. The loss function is a combination of the two losses.

We next focus in detail on the main structure used to construct the network in (3), especially the "multi-head attention" layer.

Computing the Attention
We begin with the context-free embeddings 𝑥1, 𝑥2, . . . , 𝑥𝑛 , for
𝑛 words, with each 𝑥𝑘  ℝ𝑑. Let 𝑋 denote the matrix whose
𝑘-th row is the embedding 𝑥𝑘. An attention module transforms this matrix of 𝑛 embeddings, 𝑋, into another matrix of 𝑛 em-beddings, where each row 𝑘 of the new matrix contains an embedding of the "information" in a "neighborhood" around token 𝑘. The notion of "neighborhood" and the notion of "infor-mation" are all parameterized by neural network parameters of the attention module and learnable in a data-driven manner as we describe below.
The goal of an attention module is to create weighted neigh-borhoods ( attention regions) of seemingly distant tokens in a data-driven manner and then create embeddings that corre-spond to linear combinations of the embeddings of the tokens in these attention regions. One way to achieve this goal is to decouple the "neighborhood" representation of a token with the representation of each "meaning" or "value." Thus we will trans-form each token embedding 𝑥𝑘 into a key embedding 𝜅𝑘 := 𝑥′𝑘 𝜔𝐾,
where 𝜔𝐾 ∈ ℝ𝑑×𝑑𝑘 is a learnable matrix parameter, and a value
embedding 𝑣𝑘 := 𝑥′𝑘 𝜔𝑉, where 𝜔𝑉 ℝ𝑑×𝑑𝑣 is a learnable matrix parameter. Then a neighborhood can be encoded by a query vector 𝑞 that lies in the same space as the space of keys and
such that the weighted neighborhood is defined via a similarity metric between the vector 𝑞 and the key vectors. Attention
mechanisms used in Transformers use a scaled inner product as the similarity, i.e. 𝑠𝑘 := 𝑞′𝜅𝑘 √𝑑𝑘. Then this similarity is
passed through a soft-max function 𝜎	to map it to a selection
probability in 0, 1 . Finally, as we alluded to in the beginning the embedding of the neighborhood that corresponds to this query
𝑞 is simply the weighted average of the value embeddings of
the tokens, i.e. 𝑎 := '5:𝑛	𝜎(𝑠𝑘)𝑣𝑘.
 


Suppose now that we had 𝑛 neighborhood queries 𝑞1, . . . , 𝑞𝑛, then we could create 𝑛 such neighborhood embeddings 𝑎1, . . . , 𝑎𝑛. Transformers consider "self-attention" queries, where each of the 𝑛 queries 𝑞𝑘 corresponds to a query embedding associated with a particular token and is yet another linear embedding of the form 𝑞𝑘 = 𝑥′𝑘 𝜔𝑄, where 𝜔𝑄	ℝ𝑑×𝑑𝑘 is a learnable ma-
trix parameter. Then for each such query we can calculate the
corresponding neighborhood embedding 𝑎𝑘.
Overall this transformation takes as input a matrix 𝑋 ℝ𝑛×𝑑, where each row corresponds to an original token embedding and transforms it into a matrix 𝐴, where each row 𝑘 corresponds to the neighborhood embedding associated with query 𝑞𝑘, which in turn is associated with token 𝑥𝑘. We can write this
calculation in matrix form: Let 𝑄 = 𝑋𝜔𝑄 denote the matrix with rows corresponding to query embeddings, let 𝐾 = 𝑋𝜔𝐾 denote the matrix with rows corresponding to key embeddings, and let
𝑉 = 𝑋𝜔𝑉 denote the matrix with rows corresponding to value embeddings. Then the attention embeddings (or neighborhood embeddings) can be written in matrix form as
𝐴 = Attention(𝑄, 𝐾, 𝑉) := 𝜎 (𝑄𝐾⊤/✓𝑑𝑘 ) 𝑉.

A Multi-Head Attention mapping, which is the building block of the BERT model, builds many such attention transformations,
for different matrix parameters {𝜔𝑄 , 𝜔𝐾 , 𝜔𝑉 }ℎ  , calculates
the corresponding attention embedding matrices 𝐴𝑗 ℝ𝑛×𝑑𝑣 , then concatenates the results in a big embedding matrix 𝐴 = Concatenate 𝐴1, . . . , 𝐴ℎ  ℝ𝑛×ℎ·𝑑𝑣 and applies a linear projec-tion transformation 𝐴𝜔𝑂, where 𝜔𝑂 ℝℎ·𝑑𝑣 ×𝑑𝑜 , to produce the final output encoding. Thus, we can define the basic Multi-Head
Attention transformation:
𝑋 −→ MultiHead(𝑋) := Concatenate(Head1, . . . , Headℎ)𝜔𝑂 , Head𝑗 = Attention(𝑋𝜔𝑄 , 𝑋𝜔𝐾 , 𝑋𝜔𝑉),
Each Transformer building block in BERT consists of a series of several repetitions of multi-head attention encodings, followed by a fully connected neural network (applied to each of the 𝑛 output encodings separately). The input 𝑛 encodings of each repetition is the output of the previous repetition.
 


Output Softmax
Transformer Blocks

 
Context-free
+ Positional Embedding
Tokens
 



𝑡1
 



𝑡2
 



𝑡3
 



𝑡4
 



𝑡5
 





Figure 10.5: BERT Architecture
 

Generating product embeddings
Depending on specific tasks and resources, Devlin et al. [17]
suggested to construct BERT embeddings in various ways:	Tuning only a final linear layer on
top of a pre-trained embedding net-
 
▶ Use the last layer, second-to-last layer, or concatenate the last 4 layers of the encoder outputs from the pre-trained BERT model.
▶ Fine tune the whole BERT model using the downstream task.
▶ Train the BERT language model from scratch on new data.
In the hedonic price example discussed in Section 10.6, the feature-based approach was chosen. Specifically, the second-to-last layer from a pre-trained BERT model was extracted as embeddings to be used as covariates in the final price prediction task. Each product’s text embedding is the average of the embeddings of each word/token from the input text field.

Beyond ELMo and BERT
ELMo and BERT are both important breakthroughs in NLP. The former marked the first contextual word embedding trained from a deep language model, and the latter was the first con-textual word embedding using Transformer architecture. The biggest difference lies in the choice of fundamental architec-tures: ELMo is based on a Recurrent Neural Network (RNN), while BERT is based on the Transformer architecture. RNNs can struggle to capture long-term dependencies, whereas the
 
work and freezing all other parame-ters of the embedding is referred to in the machine learning literature as linear probing. If one allows for the parameters of the embedding it-self to be updated when optimizing for a particular downstream pre-diction task, then this practice is re-ferred to as fine-tuning. [18] presents a way of blending the two modes by first training the final linear layer and then un-freezing the remain-ing parameters of the embedding and continuing to train. This blend-ing seems to produce substantial gains in generalization ability and accuracy of the resulting predictive model.
 


Transformer architecture is more efficient at capturing long-range dependencies in the text. Furthermore, ELMo creates context by using the left-to-right and right-to-left language model representations, while BERT models the entire context simultaneously.
Large language models are continuously evolving and becoming ever more powerful and sophisticated in their understanding of language and meaning. The latest generation of large lan-guage models lie in the Generative Pre-trained Transformer (GPT) family [19–21]. While BERT can be understood to use the transformer architecture as an encoder, GPT models use the transformer architecture in a decoder for a generative model of the probabilities in an autoregressive model reminiscent of the one used in ELMo. GPT models combine these modeling ideas from their successful precursors with pre-training on a large corpus of text. With the rapid development in the space of large language and multi-modal models, the latest and greatest models will certainly advance beyond the descriptions in this book, but the principles of using these models to understand complex data and use it to support robust causal inference will likely remain the same.

Revisiting the Price Elasticity for Toy Cars
In Chapter 0, Chapter ??, and Chapter 9, we saw how using increasingly flexible learning methods (OLS, LASSO, nonlin-ear regression) to control for confounding in the price-sales data for toy cars lead to increasingly more negative estimates and confidence intervals for elasticity. However, the models discussed in those chapters only used the categorical (brand, subcategory) and numeric (physical dimensions) features of the products, missing important information captured in product text descriptions and images. We revisits this example using a richer, multimodal approach and a more carefully specified causal model.

 
Multimodal embeddings and causal fine-tuning. First, we convert all available product information into numerical embed-dings: text descriptions are encoded using transformer-based language models (RoBERTa or LLaMA), product images are en-coded using the BEiT model, and tabular features are encoded using the SAINT model. Crucially, the embeddings are causally fine-tuned: after pre-training on self-supervised tasks, the model parameters are updated to optimize prediction of the price and quantity signals that serve as inputs to the DML estimator. This
 






These models all rely on the transformer architecture and self-supervised pre-training described earlier in this chapter. The key advance over first-generation ap-proaches such as BERT is that the embeddings are derived from more capable foundation models and are subsequently fine-tuned for the causal inference task at hand.
 


fine-tuning step is directly motivated by Neyman orthogonality – producing better nuisance predictions translates into more reliable causal estimates.
Using these multimodal embeddings, the deep learning models achieve test 𝑅2 values of approximately 60–67% for predicting quantity levels 𝑄𝑖𝑡 and 65–67% for predicting price levels 𝑃𝑖𝑡, substantially outperforming boosted tree models that rely on tabular data alone (approximately 48% and 19%, respectively). Notably, however, predicting the changes Δ𝑄𝑖𝑡 and Δ𝑃𝑖𝑡 – the
variation that is relevant for identifying the causal price effect –
is far harder: even the best models achieve only about 15% 𝑅2
for Δ𝑄𝑖𝑡 and 1.5% for Δ𝑃𝑖𝑡.

The key confounder: lagged quantity, not embeddings. This finding has a crucial implication for causal inference. Because product embeddings are strong predictors of price and quantity levels but weak predictors of their changes, they do not serve as major confounders of the causal price–quantity relationship. Instead, it is lagged quantity 𝑄𝑖,𝑡−1 (reflecting accumulated prod-uct visibility and quality) that is the key omitted confounder. Simple cross-sectional regressions – even those that include rich embedding controls – yield implausibly small elasticity
estimates (𝛿ˆ   0.3,  0.1 ) because they fail to account for this
dynamic confounding.	This reconciles with the intuition from prior chapters: the estimates
 
To address this, we formulate a dynamic panel model in which
the state variable 𝑆𝑖𝑡 = 𝑄𝑖,𝑡 1, 𝑃𝑖,𝑡 1, 𝑋𝑖 – comprising lagged quantity, lagged price, and time-invariant product characteris-tics (captured via embeddings) – serves as the set of controls.
Including the lagged quantity in this way shifts the estimated elasticities into an economically plausible range.

Updated elasticity estimates. Applying DML with cross-fitted boosted trees to control for 𝑆𝑖𝑡, we get rank elasticity estimates (i.e., the effect of log price on the log inverse sales rank) of approximately 0.69, with 90% confidence intervals ranging from about 0.78, 0.54 across specifications. These estimates are substantially more negative than those reported in earlier chapters and in the previous version of this example, reflecting the importance of properly accounting for dynamic confounding through lagged quantity.

Heterogeneous price elasticity. Perhaps the most striking new finding is that product embeddings are important effect modifiers: price elasticity varies substantially across products. Estimating a
 
were becoming more negative as we added better controls, but the static cross-sectional framework was never the right setting.









To convert rank elasticities to de-mand elasticities under the power-law assumption for sales ranks (He and Hollenbeck, 2020), one multi-
plies by 1 𝜗ˆ  2. The implied de-
mand elasticities therefore fall in the range [−1.58, −1.08].
 


partially linear model that allows the price coefficient to depend on lagged price, lagged quantity, and cluster-based embedding similarities, we find that sorted product-level rank elasticities range from about 2.0 to nearly 0.4 (statistically significant differences, as confirmed by pointwise confidence bands). Con-verting to demand elasticities, these estimates span approxi-mately 4.0 to 0.8, with an average near 1.5. Higher-priced and better-ranked products tend to be more price-sensitive, and products’ similarities to embedding cluster centroids are jointly significant predictors of their price sensitivity (𝑝 < 0.001 for the boosted tree specification). This pronounced heterogeneity underscores the value of AI-generated product representations:
while they do not substantially change the average elasticity es-timate, they reveal important variation in price responsiveness that a homogeneous model would miss.
At the end of the chapter we provide a notebook wherein we repeat the exercise of constructing neural nets using BERT for predicting 𝑌 and 𝐷 and plug them into DML. The results in that notebook are different than those reported here (and previously) as we use a publicly available dataset in that notebook as opposed to the proprietary data underlying the numbers we
report here.	The biggest difference between the
public and private data is that the
public data does not have the same
 
10.5	Image Embeddings
One of the most successful deep learning models for image classification was the ResNet50 model developed by He et al. [22]. At the time of the release, the paper achieved the best results in image classification, in particular for the ImageNet and COCO datasets.
The central idea of the paper is to exploit "partial linearity": tra-ditional nonlinearly-generated neurons are combined (or added together) with the previous layer of neurons. More specifically, ResNet50 takes a standard feed-forward convolutional neural network and adds skip connections that bypass two (or one or several) convolutional layers at a time. Each skipping step gen-erates a residual block in which the convolution layers predict a residual.
Formally, each 𝑘-th residual block is a neural network map-ping
 
range of numeric features as in the private data.
 
𝑣 −→ (𝑣, 𝜎0(𝜔0𝑣)) −→ (𝑣, 𝜎1 ◦ 𝜔1𝜎0(𝜔𝑘 𝑣))
 
𝑘	𝑘
 
𝑘
1	1  0
 
𝑘  𝑘
0
 
−→ 𝑣 + 𝜎𝑘 ◦ 𝜔𝑘 𝜎𝑘(𝜔𝑘 𝑣),
 




 


Figure 10.6: The ResNet50 operates on numerical 3-dimensional arrays representing images. It first does some pre-processing by applying convolutional and pooling filters, then it applies many L-residual block mappings, producing the arrays shown in green. The penultimate layer produces a high-dimensional vector 𝐼, the image embedding, which is then used to predict the image type.

where 𝜔’s are matrix-valued parameters or "weights." This structure can be seen as a special case of general neural network architecture, designed so that it is easy to learn the identity sub-maps (entering the composition of the entire network). Putting together many blocks like these sequentially results in the overall architecture depicted in Figure 10.6.
The deep feed-forward convolutional networks developed in prior work suffered from major optimization problems – once the depth was sufficiently high, additional layers often resulted in much higher validation and training error. It was argued that this phenomenon was a result of "vanishing gradients," where in a network of 𝑛 layers, computation by backpropogation us-ing the chain rule involves multiplying 𝑛 small numbers (if using traditional activation functions, recent popular activation functions such as RELU do not induce such a small derivative), causing the gradient to "vanish" for early layers and posing a computational challenge. The residual network architecture ad-dresses this by using the residual block architecture: including the residual directly via skip connections reduces the mini-mizing impact of the activation function. The creation of this architecture has allowed for high quality training even for very deep networks.
In many applications, we will not be interested in the final predictions from the image classification task. Rather, we will
 


be interested in using lower levels of the network, such as the last hidden layer, as our image embeddings to be used as inputs into the modeling task of interest. For example, in the pricing example discussed next in Section 10.6, we feed image data from product webpages into a publicly trained ResNet50 model and extract the final layer to generate the image embeddings.

10.6	Application: Hedonic Prices

 
Here we apply our new knowledge of embeddings to review an empirical application considered in Bajari et al. [23]. The application is a prediction problem which deals with hedonic price models. An empirical hedonic model is a predictive model for price given a traded object’s characteristics.5 Here, the goal is to predict the price of apparel bought and sold on Amazon.com using the product’s image and description:
𝑃𝑖𝑡 = 𝐻𝑖𝑡 + 𝜖𝑖𝑡 = ℎ𝑡(𝑋𝑖𝑡) + 𝜖𝑖𝑡 , E[𝜖𝑖𝑡 | 𝑋𝑖𝑡] = 0,	(10.6.1) where 𝑃𝑖𝑡 is the price of product 𝑖 at time 𝑡 (in months), 𝑋𝑖𝑡
are the product features, and the price function 𝑥	ℎ𝑡 𝑥 can change from period to period, reflecting the fact that prod-uct attributes/features may be valued differently in different periods. [23] use the data from time period 𝑡 to estimate the function ℎ𝑡 using deep neural network methods. The results are contrasted with classical linear regression methods as well as other modern regression methods, such as the random forest.
One of the main uses of hedonic prices is construction of cost of living indices. The use of hedonic prices allows us to "price" the product attributes as well as entire "baskets of attributes" that consumers buy. Then, given a reference "basket of attributes," one can look at the hedonic cost of a basket today compared to its cost in an earlier reference period to determine whether the cost increased or decreased. These types of calculations underlie the construction of commonly used consumer price indices (measuring inflation rates), at least for categories such as apparel products.
A key component of the approach taken in [23] is the use of product features 𝑋𝑖𝑡 generated as neural network embeddings of text and image information about the product. Specifically,
𝑋𝑖𝑡 consists of text embedding features 𝑊𝑖𝑡, constructed by converting the title and product description available on a product’s web page into numeric vectors, and image embedding
 





5: It can be given structural or causal interpretation using the so-called hedonic price models from economics.
 


features 𝐼𝑖𝑡 constructed by converting the product image into numeric vectors:
𝑋𝑖𝑡 = (𝑊𝑖′𝑡 , 𝐼𝑖′𝑡 )′.	(10.6.2)
These text and image embedding features are generated respec-tively by applying the BERT and ResNet50 mappings.
The model takes high-dimensional text and image features as inputs, converts them into a lower dimensional vector of value embeddings using deep learning methods, and outputs simultaneous predictions of price in all time periods.


Figure 10.7: The structure of the predictive model in Bajari et al. [23]. The input consists of images and unstructured text data. The first step of the process creates numerical embeddings 𝐼 and 𝑊 for images and text data via deep learning methods, specifically ResNet50 and BERT. The second step of the process takes as its input 𝑋 = 𝐼 , 𝑊  and creates predictions for hedonic prices 𝐻𝑡 𝑋  using deep learning methods
with a multi-task structure. The models of the first step are trained on tasks unrelated to predicting prices
(e.g., image classification or word prediction), where embeddings are extracted as hidden layers of the neural networks. The models of the second step are trained by price prediction tasks. The multitask price
prediction network creates an intermediate lower dimensional embedding 𝑉 = 𝑉 𝑋 , called a value embedding, and then predicts the final prices in all time periods 𝐻𝑡 𝑉 , 𝑡 = 1, ..., 𝑇 . Some variations of the method include fine-tuning the embeddings produced by the first step to perform well for price
prediction tasks (i.e. optimizing the embedding parameters so as to minimize price prediction loss).

The general structure of the model takes the form

 
Text𝑖𝑡 Image𝑖𝑡
 
𝑒
−→ 𝑋𝑖𝑡
 
𝑔1
 
(1)
 
𝑔𝑚
 
(𝑚)
 
𝜃′	𝑇
 
′	𝑇
 
−→ 𝐸𝑖𝑡 ... −→ 𝐸𝑖𝑡	=: 𝑉𝑖𝑡 −→ {𝐻𝑖𝑡 }𝑡=1 := {𝛽𝑡𝑉𝑖𝑡 }𝑡=1.
Here 𝑍𝑖𝑡,6 the original input which lies in a very high-dimensional space, is nonlinearly mapped into an embedding vector 𝑋𝑖𝑡 which is of moderately high dimension (up to 5120 dimensions
 
6: As a practical matter, most of the product attributes in [23] are time-invariant - that is, 𝑍𝑖𝑡 = 𝑍𝑖 has no
time variation. We state the model
in more generality here.
 


in this example). 𝑋𝑖𝑡 is then further nonlinearly mapped into a lower dimension vector 𝐸(1). This process is repeated to produce
the final hidden layer, 𝑉𝑖𝑡 = 𝐸(𝑚), which is then linearly mapped to the final output that consists of hedonic price 𝐻𝑖𝑡 for product
𝑖 in all time periods 𝑡 = 1, ..., 𝑇.
The last hidden layer 𝑉 = 𝐸(𝑚) is called the value embedding in this context – the value embedding represents latent attributes to which dollar values are attached. The embeddings produced
in this example are moderately high-dimensional (up to 512 dimensions) summaries of the product, derived from the most common attributes that directly determine the price of the pre-dicted hedonic price of the product. Note that the embeddings
𝑉 in this example do not depend on time and so may be thought of as representing intrinsic, potentially valuable attributes of the product. However, the predicted price does depend on time 𝑡 via the coefficient 𝛽𝑡, reflecting the fact that the different intrinsic attributes are valued differently across time.
The network mapping above comprises a deep neural network with neurons 𝐸𝑘,ℓ of the form
𝑔ℓ : 𝑣 −→ {𝐸𝑘,ℓ (𝑣)}𝐾ℓ  := {𝜎𝑘,ℓ (𝑣′𝛼𝑘,ℓ )}𝐾ℓ  .	(10.6.3)
Here 𝜎𝑘,ℓ is the activation function that can vary with the layer
ℓ and can vary with 𝑘, from one neuron to another.
The model is trained by minimizing the loss function
min	I: I:(𝑃𝑐 − 𝛽′𝑡𝑉𝑖𝑡(𝜂))2𝑄𝑖𝑡 ,	(10.6.4)
   	 
where 𝜂 denotes all of the parameters of the mapping
𝑋𝑖𝑡 ↦→ 𝑉𝑖𝑡(𝜂)
and N represents the parameter space. Here, we are using a weighted loss where we weight by the quantity of product 𝑖 sold at time 𝑡, 𝑄𝑖𝑡.
Next we review how the initial embedding is generated. A multilingual BERT model is used to convert text information and the ResNet50 model is used to convert images into a subvector of
𝐸(1). These models are trained on auxiliary prediction tasks with auxiliary outputs 𝐴𝑇𝑖𝑡 for text and 𝐴𝐼𝑖𝑡 for images. Introducing
 


these auxiliary tasks can be illustrated diagrammatically as
𝐴𝑇𝑖𝑡
𝑋𝑖𝑡 = J Text𝑖𝑡	l	𝑒	𝑊x𝑖	=: 𝐸(1)... −→ 𝐸(𝑚) := 𝑉𝑖𝑡 −→ {𝑃ˆ∗ }𝑇 ,
 
𝐴_𝐼𝑖𝑡

The embeddings 𝑊𝑖𝑡 and 𝑋𝑖𝑡 forming 𝐸(1)
 

(10.6.5)
are obtained by
 
mapping them into auxiliary outputs 𝐴𝑇𝑗 and 𝐴𝐼 𝑗 that are scored on natural language processing tasks and image classification tasks respectively. This step uses data that are not related to
prices. The parameters of the mapping generating 𝐸(1) are
considered as fixed in our analysis.
The price prediction network we employ in this example con-tains three hidden layers, with the last hidden layer containing 400 neurons. The network is trained on a large data set with more than 10 million observations. A large enough data set is crucial for training successful neural networks.
The accuracy of prediction as measured by the 𝑅2 on the test sample is about 90%. In contrast, random forests using embeddings deliver an 𝑅2 in the ballpark of 80%; the linear model using least squares applied to embeddings delivers an 𝑅2 in the ballpark of 70% and the linear model using only simple catalog features (without embeddings) delivers an 𝑅2 lower than 40%.
Thus, embeddings offer a means of making use of complex data for predictions and, at least for large data sets, neural nets can offer predictive improvements relative to competing machine learning approaches.

10.7	Notes
[24]	develop "DoubleMLDeep" which explicitly explores deep neural network architectures for incorporating text and image data as confounding variables in the DML framework.

10.8	Notebooks
Notebook 10.8.1 (Autoencoders) Python Autoencoders Note-book and R Autoencoders Notebook provide an introduction
 


to autoencoders, starting from classical principal components.

Notebook 10.8.2 (Embeddings via BERT) Python Toys and Prices Notebook provides an introduction to text embeddings via BERT and provides an application to predicting demand for toys.


10.9	Exercises
Exercise 10.9.1 (Autoencoders) Work through the Autoen-coders notebook. Try to improve the performance of the autoencoders. Report your findings (even if you don’t man-age to improve them! :-)).

Exercise 10.9.2 (BERT) Work through the BERT notebook. Try to experiment with the structure of the neural nets and demand estimation procedure. Report your findings.
 


Bibliography



[1]	Susan Sontag. ‘Turning Points’. In: Irish Pages 2.1 (2003),
pp. 186–191 (cited on page 280).
[2]	Matthew Gentzkow, Bryan Kelly, and Matt Taddy. ‘Text as Data’. In: Journal of Economic Literature 57.3 (2019),
pp. 535–74. doi: 10 . 1257 / jel . 20181020 (cited on page 282).
[3]	Pierre Comon. ‘Independent component analysis, A new concept?’ In: Signal Processing 36.3 (1994). Higher Order Statistics, pp. 287–314. doi: https://doi.org/10.1016/ 0165-1684(94)90029-9 (cited on page 286).
[4]	Francesco Locatello, Stefan Bauer, Mario Lucic, Gunnar Raetsch, Sylvain Gelly, Bernhard Schölkopf, and Olivier Bachem. ‘Challenging Common Assumptions in the Un-supervised Learning of Disentangled Representations’. In: Proceedings of the 36th International Conference on Ma-chine Learning. Ed. by Kamalika Chaudhuri and Ruslan Salakhutdinov. Vol. 97. Proceedings of Machine Learning Research. PMLR, 2019, pp. 4114–4124 (cited on page 286).
[5]		Diederik P. Kingma and Max Welling. ‘Auto-Encoding Variational Bayes’. In: 2nd International Conference on Learn-ing Representations, ICLR 2014, Banff, AB, Canada, April 14-16, 2014, Conference Track Proceedings. Ed. by Yoshua Bengio and Yann LeCun. 2014 (cited on page 286).
[6]		Yoshua Bengio, Aaron Courville, and Pascal Vincent. ‘Representation learning: A review and new perspectives’. In: IEEE Transactions on Pattern Analysis and Machine Intelligence 35.8 (2013), pp. 1798–1828 (cited on page 287).
[7]	Diederik P Kingma, Max Welling, et al. ‘An introduction to variational autoencoders’. In: Foundations and Trends® in Machine Learning 12.4 (2019), pp. 307–392 (cited on
page 287).
[8]		Tomas Mikolov, Kai Chen, Gregory S. Corrado, and Jef-frey Dean. ‘Efficient Estimation of Word Representations in Vector Space’. In: International Conference on Learning Representations. 2013 (cited on page 288).
 


[9]		Tolga Bolukbasi, Kai-Wei Chang, James Zou, Venkatesh Saligrama, and Adam Kalai. ‘Man is to Computer Pro-grammer as Woman is to Homemaker? Debiasing Word Embeddings’. In: Proceedings of the 30th International Con-ference on Neural Information Processing Systems. NIPS’16. Barcelona, Spain: Curran Associates Inc., 2016, 4356–4364 (cited on page 291).
[10]		Hila Gonen and Yoav Goldberg. ‘Lipstick on a pig: De-biasing methods cover up systematic gender biases in word embeddings but do not remove them’. In: arXiv preprint arXiv:1903.03862 (2019) (cited on page 291).
[11]		Matthew E. Peters, Mark Neumann, Mohit Iyyer, Matt Gardner, Christopher Clark, Kenton Lee, and Luke Zettle-moyer. ‘Deep contextualized word representations’. In: CoRR abs/1802.05365 (2018) (cited on page 291).
[12]		George EP Box and Gwilym M Jenkins. Time Series Analysis Forecasting and Control. Tech. rep. WISCONSIN UNIV MADISON DEPT OF STATISTICS, 1970 (cited on
page 292).
[13]	George EP Box, Gwilym M Jenkins, Gregory C Reinsel, and Greta M Ljung. Time series analysis: forecasting and control. John Wiley & Sons, 2015 (cited on page 292).
[14]		Tim Bollerslev. ‘A conditionally heteroskedastic time series model for speculative prices and rates of return’. In: Review of Economics and Statistics (1987), pp. 542–547 (cited on page 292).
[15]		Matthew E. Peters, Sebastian Ruder, and Noah A. Smith. ‘To Tune or Not to Tune? Adapting Pretrained Representa-tions to Diverse Tasks’. In: Proceedings of the 4th Workshop on Representation Learning for NLP (RepL4NLP-2019). Flo-rence, Italy: Association for Computational Linguistics, Aug. 2019, pp. 7–14. doi: 10.18653/v1/W19-4302 (cited on page 294).
[16]		Ashish Vaswani, Noam Shazeer, Niki Parmar, Jakob Uszkoreit, Llion Jones, Aidan N. Gomez, Łukasz Kaiser, and Illia Polosukhin. ‘Attention is All you Need’. In: Advances in Neural Information Processing Systems. Ed. by I. Guyon, U. V. Luxburg, S. Bengio, H. Wallach, R. Fergus, S. Vishwanathan, and R. Garnett. Vol. 30. Curran Associates, Inc., 2017 (cited on page 295).
[17]		Jacob Devlin, Ming-Wei Chang, Kenton Lee, and Kristina Toutanova. ‘BERT: Pre-training of Deep Bidirectional Transformers for Language Understanding’. In: CoRR abs/1810.04805 (2018) (cited on pages 295, 298).
 


[18]		Ananya Kumar, Aditi Raghunathan, Robbie Jones, Tengyu Ma, and Percy Liang. ‘Fine-Tuning can Distort Pretrained Features and Underperform Out-of-Distribution’. In: In-ternational Conference on Learning Representations. 2022 (cited on page 298).
[19]	Alec Radford, Karthik Narasimhan, Tim Salimans, Ilya Sutskever, et al. ‘Improving language understanding by generative pre-training’. In: (2018) (cited on page 299).
[20]	Alec Radford, Jeffrey Wu, Rewon Child, David Luan, Dario Amodei, Ilya Sutskever, et al. ‘Language models are unsupervised multitask learners’. In: OpenAI blog 1.8 (2019), p. 9 (cited on page 299).
[21]	Tom Brown, Benjamin Mann, Nick Ryder, Melanie Sub-biah, Jared D Kaplan, Prafulla Dhariwal, Arvind Nee-lakantan, Pranav Shyam, Girish Sastry, Amanda Askell, et al. ‘Language models are few-shot learners’. In: Ad-vances in Neural Information Processing Systems 33 (2020),
pp. 1877–1901 (cited on page 299).
[22]		Kaiming He, Xiangyu Zhang, Shaoqing Ren, and Jian Sun. ‘Deep residual learning for image recognition’. In: Proceedings of the IEEE conference on computer vision and pattern recognition. 2016, pp. 770–778 (cited on page 301).
[23]		Patrick L. Bajari, Zhihao Cen, Victor Chernozhukov, Manoj Manukonda, Jin Wang, Ramon Huerta, Junbo Li, Ling Leng, George Monokroussos, Suhas Vĳaykunar, et al. Hedonic prices and quality adjusted price indices powered by AI. Tech. rep. cemmap working paper CWP04/21, 2021 (cited on pages 303, 304).
[24]		Sven Klaassen, Jan Teichert-Kluge, Philipp Bach, Victor Chernozhukov, Martin Spindler, and Suhas Vĳaykumar. DoubleMLDeep: Estimation of Causal Effects with Multimodal Data. 2024 (cited on page 306).
 





Topics
 

 

Deeper Dive into DAGs, Good and Bad
Controls


"if ’good’ is taken to mean ’best’ fit, it is tempting to include anything in 𝑥 that helps predict [treatment]"
– Jeffrey Wooldridge [1].










DAGs give us an intuitive approach to take domain knowledge and turn it into an identification strategy. In this section, we focus on identification by conditioning and discuss graphical criteria that lead to the construction of valid adjustment sets for the identification of average causal effects via regression adjustment. We also discuss how graphical criteria can help us differentiate between "good" and "bad" controls.
 
11
11.1	Valid Adjustment Sets  313
11.2	Other Useful Adjustment Strategies	313
Conditioning on Parents314 Conditioning on All Com-mon Causes of 𝐷 and 𝑌 . 315
11.3	Examples of Good and Bad Controls	316
Pre-Treatment Variables or Proxies of Pre-Treatment Vari-ables	317
Post-Treatment Variables322
11.4	Notebooks	325
11.5	Exercises	325
11.	AFront-Door Criterion via Ex-ample	326
 

11.1	Valid Adjustment Sets
We discussed formally the general principles for finding valid adjustment sets in the previous chapter dedicated to the DAGs. Here we quickly review these principles before discussing others, and going into many examples of how these strategies help identify good and bad controls.
Consider any variable 𝐷 of an ASEM as a treatment of interest and any of its descendants 𝑌 as an outcome of interest. An adjustment set 𝑆 is said to be valid for identification of the causal effect of 𝐷 on 𝑌 if the conditional exogeneity/ignorability condition holds
𝑌(𝑑) ⊥ 𝐷 | 𝑆.
We first recall the counterfactual DAG approach.

We write down the counterfactual DAG induced by the fix(𝐷 = 𝑑) intervention, which operates on all structural equations defining the descendants of 𝐷 by setting 𝐷 = 𝑑 in these equations. Then, if we have that the potential outcome
𝑌(𝑑) is 𝑑-separated from the (policy) variable 𝐷 by a set of variables 𝑆, then 𝑆 is a valid adjustment set.

We also recall the "blocking backdoor paths" approach that operates in the factual DAG.

The adjustment set 𝑆 is valid if the backdoor criterion is satisfied in the factual DAG: No element of 𝑆 is a descendant of 𝐷, and all backdoor paths from 𝑌 to 𝐷 are blocked by
𝑆.

Recall that the basic idea is that if we block the backdoor path, we remove all channels of non-causal association between 𝐷 and 𝑌.

11.2	Other Useful Adjustment Strategies

 
The strategies above are good general strategies for finding valid adjustment sets or finding "good controls". We next present Some of these strategies are quite helpful because they are either very simple to apply or can also be used under partial knowledge of the DAG.1  The strategies are derived from the
 


1: See [2] for a more detailed dis-cussion of identification by condi-tioning under limited knowledge of DAGs.
 


general principles described above.
We consider the following approaches that allow us to identify the causal effect of D on Y:
▶ Conditioning on all parents of 𝐷, or the design approach (includes conditioning on the propensity score).
▶ Conditioning on all parents of 𝑌 that are not descendants of 𝐷,
▶ Conditioning on all parents of both 𝐷 and 𝑌 is sufficient.
▶ Conditioning on all common causes of 𝐷 and 𝑌 is also sufficient.
▶ Conditioning on the union of causes of 𝐷 and 𝑌 is also sufficient.
All of these methods have different advantages and robustness properties as we discuss below.

 
Conditioning on Parents
A very simple strategy is conditioning on one of the parents of
𝐷, the parents of 𝑌, or the parents of both 𝐷 and 𝑌.
 




𝑍2
 




𝑋3	𝑌
 
 	 
𝑋2	𝑀

 
𝑍1
 
  𝑋1   𝐷
 

Figure 11.1: A DAG in Pearl’s Ex-ample.











Note that 𝐴 is allowed to be an empty set. Also note that, in the second case, the additional adjustment set 𝐴 is redundant, since p(𝑦 | 𝑎, 𝑧, 𝑑) = p(𝑦 | 𝑧, 𝑑) in this case.
 


Adjusting for parents is a very useful strategy, because it only requires knowledge of parents in a DAG without precise knowl-edge of the remaining graph structure. Conditioning on parents is also behind the propensity score strategies used in many experimental or quasi-experimental empirical analyses. If the propensity score is known, it can be used as a parent of 𝐷 itself. Finally, conditioning on parents of 𝑌 is most useful for attaining maximal statistical efficiency, but may be less robust than conditioning on both sets of parents under unforeseen deviations from the given graph structure. See [2] for further detailed discussion of robustness of adjusting for both sets of parents.

Conditioning on All Common Causes of 𝐷 and 𝑌
Another simple and widely used adjustment strategy is con-ditioning on all common causes of the outcome variable of interest and the treatment variable.




𝑌
𝑋 = (𝑋1, 𝑋2, 𝑋3)	𝑀
𝐷

Figure 11.2: Reduced DAG for Pearl’s Example


Let 𝐴𝑛𝑋 denote the set of strict ancestors of node 𝑋, where strict means that 𝑋 is excluded. That is,
𝐴𝑛𝑋 = 𝐴𝑛𝑋 \ 𝑋.

 



 

The strategy above is commonly used in empirical work. How-ever, [2] recommend adjusting for the union 𝑆 of causes of 𝑌 or 𝐷 (excluding descendants of 𝐷) in practice as they formally quantify this strategy as the maximally robust strategy under perturbations of a specified DAG structure that preserves 𝑆. This strategy is useful when we don’t know the parents of 𝑌 or
𝐷, but only know that 𝑆 are their ancestors.



11.3	Examples of Good and Bad Controls

 
We now present a series of simple example DAGs that might arise in empirical research. Within these examples, we discuss what would be good and bad variables to adjust for in each case (aka good and bad controls), when one is interested in estimating the average treatment effect of a treatment 𝐷 on an outcome 𝑌.2 Similar to the collider bias examples we presented in Chapter 6.3, we will see how adjusting for some of the observed variables can introduce bias and lead to estimating a parameter that is far from the causal effect of interest. In each case, we will denote the candidate control of interest with
𝑍 and will denote unobserved variables with 𝑈. We depict unobserved variables with a dashed circle in the figures.
 






2: The content in this section draws heavily from the excellent research paper of Cinelli, Forney and Pearl [3].
 


We start by analyzing a group of potential control variables that in most empirical applications would correspond to pre-treatment variables, i.e. variables whose value was determined prior to the treatment assignment. It is common empirical prac-tice to adjust for as many pre-treatment variables as available in an attempt to ensure that conditional ignorability holds. How-ever, we will see that bias can be introduced by controlling even for pre-treatment variables if one is not careful. Rather than always control for all pre-treatment variables, a better approach is to adjust only for pre-treatment variables that are ancestors of either the treatment, the outcome, or both. If one is willing to believe that identification by conditioning is feasible, then following this approach is a safe strategy.
We then consider the use of post-treatment variables, i.e. variables that correspond to quantities whose value is determined after the treatment assignment. We will see that in this case there are relatively few good control cases. In some cases, controlling for post-treatment variables might not hurt and may even improve precision (reduce variance). However, such settings seem unlikely to be common in empirical practice. Hence, as a high-level rule, controlling for post-treatment variables should be avoided when one is interested in estimating causal effects.
Finally, we provide a separate discussion of post-treatment but pre-outcome variables, i.e. variables whose value is determined prior to the determination of the value of the outcome of interest. Pre-outcome variables should be included if one is interested in estimating direct effects of the treatment on the outcome while excluding indirect effects. This type of direct effect is referred to as a controlled direct effect to distinguish it from other forms of direct effects appearing in mediation analysis. We will see again that one should be careful that the mediation variables that one conditions on are not themselves confounded through unobserved factors even in this case.

Pre-Treatment Variables or Proxies of Pre-Treatment Variables
Observed common causes or proxies of common causes. A common example of a good control that we have discussed so far is an observed common cause, 𝑍, of 𝐷 and 𝑌 (Figure 11.3a). Even if the common cause is unobserved, it suffices that we have a proxy control variable that controls all the information flow to either the treatment (complete treatment proxy; Figure 11.3b) or to the outcome (complete outcome proxy; Figure 11.3c).
 


Controlling for such a proxy also blocks the backdoor path
𝐷  𝑈  𝑌. Of course, the proxy blocking the backdoor path only holds if the proxy variable captures all the information flow from the unobserved confounder. If, for instance, there are also direct paths from the unobserved variable to the treatment (in the case of a treatment proxy), then controlling for a proxy does not remove confounding bias. In this case, we will see that one can follow more advanced approaches related to proxy controls under additional structure in Chapter 12.

𝑌	𝑌	𝑌


 

𝐷

(a)
 

𝐷

(b)
 



(c)
 
Figure 11.3: Good controls: (a) ob-
𝐷		served common cause, (b) com-plete treatment proxy control of un-observed common cause, (c) com-plete outcome proxy control of un-observed common cause.
 

 

Confounded mediators with observed common cause or prox-ies of unobserved common cause. It is important to note that confounding occurs even when there exists a common cause
𝑍 of the treatment 𝐷 and some mediator 𝑀 in a path from
𝐷 to 𝑌 (Figure 11.4a). In such cases, if we don’t condition on the common cause of 𝐷 and 𝑀, there is an open backdoor path 𝐷   𝑍   𝑀   𝑌. In such cases, 𝑍 is a good control as it blocks this backdoor path. Similarly, if a common cause
𝑈 of 𝐷 and 𝑀 is unobserved, but some complete treatment proxy control 𝑍 (Figure 11.4b) or some complete outcome proxy control 𝑍 (Figure 11.4c) is observed, then it suffices to adjust for this proxy 𝑍.
 


𝑌	𝑌	𝑌
 	 	 
 
𝑀

 
𝐷

(a)
 
𝑀

 
𝐷

(b)
 






(c)
 
𝑀
Figure 11.4: Good controls: (a) con-founded mediator with observed common cause, (b) confounded mediator, with observed complete
𝐷	treatment proxy control of unob-
served common cause, (c) con-founded mediator with observed complete outcome proxy control of unobserved common cause.
 

Causes of only treatment or only outcome. As stated in Corol-lary 11.2.3, a conservative empirical practice is to include the union of parents of 𝐷 and 𝑌 in the adjustment set. Including variables that are parents of the outcome (Figure 11.5a) can lead to reduced variance during estimation as explained in Chapter ?? where we discuss including pre-treatment covari-ates in RCTs. Including variables 𝑍 that affect the treatment
𝐷 but have no causal path to the outcome (Figure 11.5b) is potentially more controversial. Including these variables does not introduce bias. However, their inclusion can be detrimental for precision, as such variables can potentially explain away all of the useful variation in the treatment, leaving little variation for the identification of causal effects.
𝑌	𝑌


 

𝐷

(a)
 

𝐷

(b)
 
Figure 11.5: Neutral controls: (a) Outcome-only cause. Can improve precision; decrease variance. (b) Treatment-only cause. Can de-crease precision; introduce vari-ance.
 

Even more importantly, when there are unobserved common causes of 𝐷 and 𝑌 as illustrated in Figure 11.6, adjusting for a treatment-only cause, 𝑍, can exacerbate the bias stemming from unobserved confounding. Essentially, controlling for 𝑍 removes exogenous variation in the treatment 𝐷 that is useful for identifying the causal effect but leaves the confounded variation
- as 𝑍 is not related directly to the unobserved confounder 𝑈. As such, the resulting estimated effect may be essentially driven by the unobserved confounder and thus be heavily biased. For this reason, one should avoid controlling for variables that are
 


known to have no causal path to the outcome that does not pass through the treatment. As we study in Chapter 12, such variables are known as instrumental variables. These variables can be thought as inducing natural experiments that can be leveraged for causal identification even in the presence of unobserved confounding. However, we need to use alternative identification – instrumental variable – arguments and estimation strategies to make use of instruments. We introduce these instrumental variable approaches in Chapter 12 and Chapter 13. Importantly, instruments should not be used in an identification by adjustment strategy.

 


  𝐷   𝑌
 
Figure 11.6: Bad control. Bias ampli-fication by adjusting for an instru-ment. Treatment-only cause (instru-ment) that can amplify unobserved confounding bias.
 


 
M-bias. The DAG in Figure 11.7, typically referred to in the literature as the M structure, is the source of much debate; see e.g. [7, 8]. If such cases were impossible, the high-level strategy of controlling for all pre-treatment variables when attempting to identify causal effects by conditioning would be an unambiguously safe empirical route resulting in no harm other than potentially increasing variance by including an instrument. However, this structure shows that there exist settings where adjusting for a pre-treatment covariate 𝑍 can lead to a wrong causal effect, while not adjusting for 𝑍 would have yielded the correct causal effect. A better high-level strategy is the one highlighted in the prior sections: If we are willing to assume that identification by conditioning is possible, then we should adjust only for pre-treatment variables that are either an ancestor of the treatment, of the outcome, or of both treatment and outcome.
More concretely, in the M structure graph (Figure 11.7), 𝐷 and
𝑌 are driven by two independent unobserved causal factors
𝑈1, 𝑈2. The variable 𝑍 is a common outcome of these two un-observed causal factors. When conditioning on 𝑍, we introduce collider bias between 𝑈1, 𝑈2, making them correlated factors. Conditioning on 𝑍 can thus lead to a causal effect estimate that is solely driven by this spurious correlation between 𝑈1 and 𝑈2, introduced by the collider bias. In graphical terms, adjusting for 𝑍 closes the path 𝐷 ← 𝑈1 → 𝑍 ← 𝑈2 → 𝑌(𝑑) in the SWIG
DAG G˜ (𝑑) produced by the fix(𝐷 = 𝑑) operation. However,
 




𝑌

 
𝐷

Figure 11.7: Bad control. M-Bias. Pre-treatment variable that intro-duces Heckman selection bias between two uncorrelated unob-served causes.
 


there is no open path connecting 𝐷 to 𝑌 𝑑 when we do not condition on 𝑍. Hence, the effect identified by not adjusting for any variable is the correct causal effect within this example structure.














Homophily refers to the tendency to associate with similar individ-uals - i.e. similar people tend to become friends.






Finally, note that the M-bias argument is very sensitive to the exact independence of the unobserved factors 𝑈1, 𝑈2. In most empirical applications, we expect these unobserved factors that drive the treatment and outcome of interest to be correlated with each other as in Figure 11.8a. In this case, note that even if we don’t adjust for 𝑍, the calculated effect is biased due to the backdoor path 𝐷   𝑈1   𝑈2   𝑌. Thus, neither adjusting nor not adjusting for 𝑍 gives the correct answer.
Moreover, it is not clear whether adjusting for 𝑍 increases or decreases the correlation between 𝑈1 and 𝑈2 and hence exacerbates or ameliorates the confounding bias. Similarly, if
𝑍 itself has a direct effect on the outcome (as in Figure 11.8b), on the treatment, or on both (as in Figure 11.8c), then not adjusting for 𝑍 opens the backdoor paths 𝐷  𝑈1  𝑍  𝑌 and 𝐷 𝑍 𝑌, correspondingly. Hence, it is not clear that removing the bias induced by these open backdoor paths, by adjusting for 𝑍, is more beneficial than the extra M-bias incurred by closing the path 𝐷  𝑈1  𝑍  𝑈2  𝑌. Work of [7, 11] argues that M-bias in many realistic data generating processes is of lower order than confounding bias and therefore argues
 


that one should err on the side of adjusting for pre-treatment covariates even in the potential presence of M-bias. [8] provides a counterpoint, arguing that the strength of the different biases will differ in general and thus careful consideration of the strength of each of the causal paths at play should be done on a case-by-case basis.

𝑌


 



𝐷

(a)
 





(b)
 





(c)
 
Figure 11.8: No control solutions:
(a) M-bias with correlated unob-served factors. (b) M-Bias with con-founding. Pre-treatment variable that introduces Heckman selection between two uncorrelated unob-served causes and is a confounder itself. (c) Butterfly Bias. M-bias with direct confounding.
 

Post-Treatment Variables
Now we turn to adjustment for post-treatment variables. The general message of this section is that explicitly adjusting for post-treatment variables is almost always a bad idea. Impor-tantly, the general message implies that researchers should be careful to avoid implicitly adjusting for post-treatment variables through the way they have structured their observational anal-ysis, data collection, and variable definitions – see e.g. [4] for examples from epidemiology. For instance, when estimating the effect of education on wages using data on employed individuals, we are implicitly conditioning on "employment" which is a post-treatment variable and can lead to selection bias.

Mediation. A common way a post-treatment variable can lead to bias in identifying the full causal effect of 𝐷 on 𝑌 is if it lies on a causal path from the treatment to the outcome (Figure 11.9a). In this case, the causal influence that flows through that path is blocked and we are only measuring a partial effect. It is important to note, that the causal influence of such a path can be partially blocked even if one conditions on a descendant of the mediator (Figure 11.9b).
 



 
𝐷	𝑌
 
𝐷	𝑌
 


 



(a)
 



(b)
 
Figure 11.9: Bad controls for learn-ing the full effect of 𝐷 on 𝑌: (a) over-control bias, by controlling for a mediator. (b) over-control bias, by controlling for an outcome caused by a mediator.
 

 
Interestingly, controlling for an ancestor of a mediator (Fig-ure 11.10) does not impede us from learning the full direct effect of 𝐷 on 𝑌. In this case, the flow through the causal path
𝐷 → 𝑀 → 𝑌 is not blocked by 𝑍. For example, d-separation can be easily checked in the SWIG G˜ (𝑑) produced by fix(𝐷 = 𝑑).
When we are controlling for a post-treatment variable that mediates the effect of the treatment as in Figure 11.9a, we are only capturing direct effects from the treatment to the outcome that do not work through this mediator. This type of direct effect after controlling for mediators is typically referred to as a controlled direct effect. Identifying the controlled direct effect is many times a relevant empirical question, in which case con-trolling for 𝑍 is not problematic. However, even when we are interested in the controlled direct effect, we should pay atten-tion to cases where the mediators are themselves confounded through unobserved factors as illustrated in Figure 11.11. In such settings, by controlling for the mediator, we are opening a collider path 𝐷  𝑍  𝑈  𝑌 which can lead to severe bias, such as calculating non-zero direct effects even when they are zero.

Heckman selection bias. Another common way that post-treatment variables can lead to bias is due to collider bias or Heckman selection, as described in Chapter 6.3. In this case, conditioning on the post-treatment variable introduces spurious correlations between the treatment variable and some other variable which opens new paths of non-causal influence from the treatment to the outcome. For instance, Figure 11.12a corresponds to the low birthweight paradox we presented in Example 6.3.2. Similarly, Figure 11.12b corresponds to the (Hollywood) Example 6.3.1. Finally, Figure 11.12c arises when we are controlling for an outcome of the outcome, perhaps even without realizing this.
 









𝐷	𝑌

Figure 11.10: Neutral control. Cause of a mediator. Can poten-tially improve precision.








Figure 11.11: Bad control even for the controlled direct effect. Con-founded mediator bias.
 



 


 



(a)
 
𝑌

𝑌

𝐷

(b)
 


𝐷   𝑌




(c)
 


 
Figure 11.12: Bad controls: (a) col-lider stratification bias (e.g. low birth-weight "paradox" example),
(b) collider stratification bias, (c) controlling for an outcome of the outcome of interest.
 

 
There are of course some edge cases where controlling for a post-treatment variable 𝑍 does not lead to selection bias – e.g. Figure 11.13a and Figure 11.13b. In each of these two cases, the post-treatment variable is not a collider on a path from 𝐷 to
𝑌. However, it is not clear that adjusting for 𝑍 improves the analysis in any respect even in these cases, and adjusting for 𝑍 could potentially hurt precision.



𝐷   𝑌
𝑌

 



(a)
 



(b)
 
Figure 11.13: Neutral controls: (a) outcome of the treatment that is unrelated to the outcome of inter-est, (b) outcome of the treatment that does not introduce Heckman selection.
 

11.4	Notebooks
Notebook 11.4.1 (DAGs) R: Dagitty Notebook employs the R package "dagitty" to analyze some simple DAGs as well as Pearl’s Example. This package automatically finds adjustment sets and also lists testable restrictions in a DAG. Python: Pgmpy Notebook employs the analogue with Python package "pgmpy" and conducts the same analysis.


11.5	Exercises
Exercise 11.5.1 (Pearl’s Example continued) The study prob-lems ask learners to continue the analysis of Pearl’s Example DAG that we started in the Study Problems to Chapter 7. The provided notebooks are a useful starting point. Recall that Pearl’s Example is structured as follows:

 
𝑍2
 
  𝑋3   𝑌
 
 	 
𝑋2	𝑀

 
𝑍1
 
  𝑋1   𝐷
 

Figure 11.14: Pearl’s Example
 

1.	For Pearl’s Example, write out the parents, non-parents, descendants, and non-descendants of nodes 𝑋2 and 𝑀. List all the backdoor paths between 𝑌 and 𝑋2. Can you identify the effect of 𝑋2 on 𝑌 by conditioning?
2.		(Front-Door-Criterion) For Pearl’s Example, show that we can identify the effect 𝐷   𝑀 by conditioning on an empty set and the effect 𝑀   𝑌 by conditioning on 𝐷. Combining the two results, we can identify the total effect of 𝐷 on 𝑌. Solving this exercise analytically is a nice exercise; you can compare your results against causal identification packages. (Identification via this strategy is known as the Front-Door criterion; see Ap-pendix 11.A.
3.	Add an arrow 𝑍2  𝑍1 in Pearl’s Example and figure out how to identify the effect of 𝐷  𝑌 by condition-ing, of 𝐷 𝑀 by conditioning, and of 𝑀 𝑌 by conditioning. (Note that valid conditioning sets may be
 


empty.) Can you identify the effect of 𝑋2  𝑌? If so, how? You may solve this analytically or using a causal identification package.
4.	Add an arrow 𝑋1  𝑀 in Pearl’s Example and figure out how to identify the effect of 𝐷  𝑌 by condition-ing, of 𝐷 𝑀 by conditioning, and of 𝑀 𝑌 by conditioning. Can you identify the effect of 𝑋2  𝑌? If so, how? You may solve this analytically or using a causal identification package.
5.	Try to ask an instruction-following LLM (such as Chat-GPT) about identification and valid adjustment sets, both for the original Pearl’s Example as well as the variations in the latter two problems. Can you verify or find mistakes in the response? If you find mistakes, how might they be corrected? When mistakes are pointed out to the LLM, is it able to correct them? For exam-ple, you can try starting with the following prompt and make variations on it: “I have a causal graph with nodes Z1, Z2, X1, X2, X3, D, M, Y and edges Z1->X1, Z1->X2, Z2->X2, Z2->X3, X1->D, X2->D, X2->Y, X3->Y, D->M,
M->Y. Is the effect of D on Y identified? What are the valid adjustment sets?"

11.A Front-Door Criterion via Example
We examine identification in Pearl’s Example (Figure 11.1), via the front-door criterion. First note that we can write the potential outcome of interest 𝑌(𝑑) as 𝑌(𝑀(𝑑)), since in the SWIG 𝐺˜ (𝑑)
there is no other path from 𝑑 to 𝑌(𝑑) other than through 𝑀(𝑑).
E[𝑌(𝑑)] = E[𝑌(𝑀(𝑑))]
=	E[𝑌(𝑀(𝑑)) | 𝑀(𝑑) = 𝑚]P(𝑀(𝑑) = 𝑚)𝑑𝑚
= ∫ E[𝑌(𝑚) | 𝑀(𝑑) = 𝑚]P(𝑀(𝑑) = 𝑚)𝑑𝑚
Suppose that we make a further surgery to the SWIG graph in Figure 7.8 by adding an intervention on the variable 𝑀 𝑑 ,
i.e. take the modified SWIG graph induced by intervention fix(𝐷 = 𝑑) and on that graph make a further intervention fix(𝑀(𝑑) = 𝑚). This leads to the new SWIG:
 


 
𝑍2



𝑍1
 
  𝑋3
𝑋2    𝑋1
 
  𝑌(𝑚)
 
𝑚
𝑀(𝑑)
 
  𝐷	𝑑
 





Figure 11.15: The DAG induced by a recursive Fix/SWIG intervention fix 𝑀 𝑑 = 𝑚 on the SWIG in Fig-
ure 7.8.
 

 
Note that in this SWIG, we have 𝑌(𝑚) ⊥ 𝑀(𝑑). Thus we have: E[𝑌(𝑚) | 𝑀(𝑑) = 𝑚] = E[𝑌(𝑚)],
leading to the front-door formula:
E[𝑌(𝑑)] = ∫  E[𝑌(𝑚)]P(𝑀(𝑑) = 𝑚)𝑑𝑚

The term E 𝑌 𝑚 is the mean counterfactual response of 𝑌 when we intervene on 𝑀 and P 𝑀 𝑑 = 𝑚 is the probability law of the counterfactual response of 𝑀 when we intervene
on 𝐷. Both of these interventional quantities can be separately identified via backdoor adjustment. More concretely, E[𝑌(𝑚)] = E[E[𝑌 | 𝑀 = 𝑚, 𝐷]], and P(𝑀(𝑑) = 𝑚) = P(𝑀 = 𝑚 | 𝐷 = 𝑑).3
Note that under linearity assumptions on the CEFs – i.e. E[𝑌 |
𝑀 = 𝑚, 𝐷] = 𝛼𝑚 + 𝛽𝐷 +4 𝑐 and E[𝑀 | 𝐷 = 𝑑] = 𝛾𝑑 + 𝛿 – we get E[𝑌(1) − 𝑌(0)] = 𝛼𝛾.  Thus, the average treatment effect
 

















3: See Exercise 2.

4: Prove this as a reading exercise.
 
𝛼𝛾, can be estimated by estimating 𝛼 via OLS of 𝑌 on 𝑀, 𝐷
and 𝛾 via OLS of 𝑀 on 𝐷.
 


Bibliography



[1]	Jeffrey M Wooldridge. ‘Violating ignorability of treatment by controlling for too many factors’. In: Econometric Theory
21.5 (2005), pp. 1026–1028 (cited on page 312).
[2]	Tyler J. VanderWeele and Ilya Shpitser. ‘A new crite-rion for confounder selection’. In: Biometrics 67.4 (2011),
pp. 1406–1413 (cited on pages 313, 315, 316).
[3]		Carlos Cinelli, Andrew Forney, and Judea Pearl. ‘A Crash Course in Good and Bad Controls’. In: Sociological Methods & Research (2022) (cited on page 316).
[4]		Miguel A Hernán, Sonia Hernández-Díaz, Martha M Werler, and Allen A Mitchell. ‘Causal knowledge as a prerequisite for confounding evaluation: an application to birth defects epidemiology’. In: American Journal of Epidemiology 155.2 (2002), pp. 176–184 (cited on pages 318,
322).
[5]		A Pastuszak, D Bhatia, B Okotore, and G Koren. ‘Precon-ception counseling and women’s compliance with folic acid supplementation.’ In: Canadian Family Physician 45 (1999), p. 2053 (cited on page 318).
[6]		Rolv Terje Lie, Allen J Wilcox, and Rolv Skjærven. ‘A population-based study of the risk of recurrence of birth defects’. In: New England Journal of Medicine 331.1 (1994),
pp. 1–4 (cited on page 318).
[7]		Peng Ding and Luke W. Miratrix. ‘To Adjust or Not to Adjust? Sensitivity Analysis of M-Bias and Butterfly-Bias’. In: Journal of Causal Inference 3.1 (2015), pp. 41–57 (cited on pages 320, 321).
[8]	Judea Pearl. ‘Comment on Ding and Miratrix: “To Adjust or Not to Adjust?”’ In: Journal of Causal Inference 3.1 (2015),
pp. 59–60. doi: doi:10.1515/jci-2015-0004 (cited on pages 320, 322).
[9]	Cosma Rohilla Shalizi and Andrew C Thomas. ‘Ho-mophily and contagion are generically confounded in observational social network studies’. In: Sociological Meth-ods & Research 40.2 (2011), pp. 211–239 (cited on page 321).
[10]		Felix Elwert and Christopher Winship. ‘Endogenous se-lection bias: The problem of conditioning on a collider variable’. In: Annual Review of Sociology 40 (2014), pp. 31–53 (cited on page 321).
 

 
Bibliography
 
329
 


[11]	Wei Liu, M Alan Brookhart, Sebastian Schneeweiss, Xiao-juan Mi, and Soko Setoguchi. ‘Implications of M bias in epidemiologic studies: a simulation study’. In: American Journal of Epidemiology 176.10 (2012), pp. 938–948 (cited
on page 321).
[12]	Eric B Schneider. ‘Collider bias in economic history re-search’. In: Explorations in Economic History 78 (2020),
p. 101356 (cited on page 324).
 

 
Unobserved Confounders, Instrumental Variables, and Proxy
Controls


"Without Philip Wright
would there have been causal DAGs? Who can really say?"
– Kei Hirano∗
https://keihirano.github.io/haiku.html











In this chapter, we discuss various models with unobserved confounders where the adjustment strategies based on con-ditioning that we have discussed no longer work. We start with sensitivity analysis of causal inference to the presence of unobserved confounders. Then we discuss identification of causal effects when instrumental variables or proxy controls are available.
 
12
12.1	The Difficulty of Causal In-ference with an Unobserved Confounder	331
12.2	Impact of Confounders on Causal Effect Identification and Sensitivity Analysis	332
12.3	Partially Linear IV Mod-els		336
A Wage Equation with Un-observed Ability	336
Aggregate Market De-mand	338
Limits of Average Causal Ef-fect Identification under Partial Linearity	339
12.4	Nonlinear IV Models . 342 The LATE Model	342
The IV Quantile Model★344
12.5	Partially Linear SEMs with Griliches-Chamberlain Proxy Controls	345
12.6	Nonlinear Models with Proxy Controls★	347
12.7	Notebooks	349
12.8	Exercises	349
12.	AProofs	351
Latent Confounder Bias Re-sult: Theorem 12.2.1	351
Partially Linear Outcome IV Model: Theorem 12.3.2 . 352 Partially Linear Compliance IV Model: Theorem 12.3.3 352 Linear Proxy Model: Theo-rem 12.5.1	353
 
∗ Sewall Wright, son, and Philip Wright, father, were responsible for some of the greatest ideas in causal inference. Sewall Wright invented causal path diagrams (linear DAGs), and Philip Wright wrote down DAGs for supply-demand equations, proposed IV methods for their identification, and even proposed weather conditions as instruments. Just one of these contributions would probably have been enough to get a QJE publication in the 1970s and later, but it was not good enough in 1926 or so. Philip Wright is a (causal) parent of Sewall Wright, so he is one of the causes of DAGs (hence the haiku).
 
12.1		The Difficulty of Causal Inference with an Unobserved Confounder
 

"All happy statisticians are happy in their own way; but all the unhappy ones are all alike — they all do causal inference with observational data”. L. Tolstoy in Anna Karenina (Source: Twitter)
Here we consider models with unobserved confounding vari-ables. The presence of unobserved confounding variables com-plicates identification of causal effects. Without further assump-tions, it is impossible to identify causal effects in a setting with unobserved confounding variables.
For example, consider the following two basic models shown in Figures 12.1 and 12.2, where we can think of 𝑌 as wages, 𝐷 as education, and 𝐴 as latent ability.
In the first model, 𝐷 has a causal effect on 𝑌; and in the second, it does not. However, the two models in Figures 12.1 and 12.2 are statistically indistinguishable from each other if 𝐴 is not observed. Even with strong restrictions, as in Gaussian linear SEMs, the observed correlation between 𝐷 and 𝑌 can always be rationalized either as a causal effect of 𝐷 on 𝑌 or the result of a common cause 𝐴.
The observation that Figures 12.1 and 12.2 are statistically indis-tinguishable applies more generally. While we cannot precisely pin down causal effects in such cases, we can still learn about causal effects by performing sensitivity analysis if we are willing to assume a bound on the strength of unobserved confounders. We discuss a practical and intuitive approach to sensitivity analysis in Section 12.2.
We may also make progress in learning causal effects in the presence of unobserved confounders by considering the use of instrumental variables (IVs) – additional random vectors 𝑍 that create exogenous variation in 𝐷 – as illustrated in Figure 12.3. This approach was introduced by Philip Wright in 1928 [1]. The use of instruments renders many linear ASEMs identifiable, allowing us to perform inference on structural effects 𝐷
𝑌. Some nonlinear ASEMs also become identifiable, though identification still fails for completely unrestricted nonlinear models. We discuss the use of instruments in Sections 12.3-12.4.
A related set of problems is when we observe multiple proxy measurements of the latent confounder 𝐴. For example, we may observe 𝑆, the SAT score, and 𝑄, the ACT score, which may
 









𝐷   𝑌
Figure 12.1: 𝐷 causes 𝑌

𝐷	𝑌

 
Figure 12.2: 𝐷 and 𝑌 are caused by a latent factor 𝐴






𝑍   𝐷   𝑌

 
Figure 12.3: A DAG with Latent Confounder 𝐴 and Instrument 𝑍.
 

 
both be proxies for latent confounder, 𝐴, ability as illustrated in Figure 12.4. Note that conditioning on 𝑄 and 𝑆 does not block the backdoor path 𝑌   𝐴   𝐷. Hence we cannot use the regression adjustment method for identification of
𝐷  𝑌. However, this problem is related to IVs, because we can effectively use one measurement in place of 𝐴 and instrument it with another measurement to deal with the measurement error. This process can provide identification of the main effect 𝐷  𝑌. In other words, we can use instrumental variable regression of 𝑌 on 𝐷 and 𝑆, using 𝐷 and 𝑄 as technical instrumental variables. This approach was introduced by Zvi Griliches in 1977 [2]. This model has also been extensively studied for nonlinear models as well, e.g., Miao et al. [3] and Deaner [4], especially in the recent literature. We discuss proxy approaches in Section 12.6.

12.2		Impact of Confounders on Causal Effect Identification and Sensitivity Analysis
 



𝑄	𝑆

𝐷	𝑌
Figure 12.4: A DAG with two prox-ies for latent confounders.
 



𝑋

Figure 12.5: 𝑋 are observed con-
founders, and 𝐴 are unobserved confounders.










To give context to our example, we can interpret 𝑌 as earnings, 𝐷 as education, 𝐴 as ability, and 𝑋 as a set of observed background variables. In this example, we can interpret 𝛼 as the returns to schooling.
We start by applying the partialling out operator to get rid of the
 


𝑋’s in all of the equations. Define the partialling out operation of any random vector 𝑉 with respect to another random vector
𝑋 as the residual that is left after subtracting the best predictor of 𝑉 given 𝑋:
𝑉˜ = 𝑉 − E[𝑉 | 𝑋].
If 𝑓 ’s are linear, we can replace E 𝑉	𝑋 by linear projection. After partialling out, we have a simplified system:

 
𝑌˜
 
:=	𝛼𝐷˜ + 𝛿𝐴˜ + 𝜖𝑌 ,
 
𝐷˜
𝐴˜
 
:=	𝛾𝐴˜ + 𝜖𝐷 ,
:=	𝜖𝐴,
 
where 𝜖𝑌, 𝜖𝐷, and 𝜖𝐴 are uncorrelated.








 



The formula follows from inserting the expression for 𝐷˜ into
 
Omitted confounder bias is also of-ten referred to as omitted variables bias.
 
the definition of 𝛽 and then simplifying the resulting expression using the assumptions on the 𝜖’s.
We can use this formula to bound 𝜙 directly by making assump-tions on the size of 𝛿 and 𝛾. An alternative approach can be based on the following characterization, based on partial 𝑅2’s. This characterization generalized the analogous result from Cinelli and Hazlett [5], with the slight difference that we have adapted the result to the partially linear model.1
 









1: [6] provides an equivalent result for PLM and a general form of such result for fully nonlinear models.
 








Therefore, if we place bounds on how much of the variation in
𝑌˜ and in 𝐷˜ the unobserved confounder 𝐴˜ is able to explain, we
 


can bound the omitted confounder bias by
J𝜙2.


 
Example 12.2.2 Here, we consider the empirical example from Cinelli and Hazlett [5], which is based on the original analysis from Hazlett [7]. In this example, we are interested in estimating the effect of having experienced direct war violence (violence) on attitudes towards peace (peace). To obtain our estimates, we use data from a survey on attitudes of Darfurian refugees in eastern Chad. For further details regarding the data and historical context, see the original paper [7].
As we suspect that experiencing direct war violence is not as good as randomly assigned, we control for observed de-mographics to hopefully captured sources of confounding. The 𝑅2 of the regression of peace on the controls after par-tialling out violence is 0.13, and the 𝑅2 of the regression of violence on the controls is 0.01. Based on these observed values, suppose we are willing to accept that
 
The Notebooks 12.7.1 analyse the sensitivity of the DML estimate in the Darfur wars.














Benchmarking the association of unobserved confounders to that of observed confounders can be ar-
 
𝑅2
𝑌˜ ∼𝐴˜|𝐷˜
 
≤ 0.13,	𝑅𝐷˜ ∼𝐴˜
 
≤ 0.01.
 
gued for on the basis that the orig-inal controls were chosen in an ef-fort to find good variables to cap-
 
That is, we are willing to assume that any latent confounder
is no stronger than the observed controls for predicting 𝑌 and for predicting 𝐷.
We can now apply Theorem 12.2.1 to obtain a point estimate of the bias using these benchmark values for 𝑅2	and
 
ture confounding, so any remain-ing source of confounding is likely less predictive of the outcome and variable of interest than the ob-served controls. While a nice story, one should be cautious of such ar-
 
𝑅2
 
𝑌˜ ∼𝐴˜|𝐷˜
 
guments as controls are often based
 
. Filling these values in, we obtain a point estimate of the bias as
𝜙ˆ = (0.13 ∗ 0.01 0.0597 ≈ 0.0236
 
on convenience and what is read-ily available and measurable rather than careful consideration. Here, we adopt this story as a benchmark
 
 
where we estimate E 𝑌˜  𝛽𝐷˜ 2	0.0597 using the sam-ple average of the squared residuals from the regression of the residuals from partialling the controls out from 𝑌 onto the residuals from partialling out the controls from 𝐷
and E  𝐷˜ 2   0.1402 as the sample average of the squared
residuals from partialling out the controls from 𝐷. Finally, the point estimate of 𝛽 from the data is 0.1003, so we can obtain point estimates of the upper and lower bounds on 𝛼 as 𝛽ˆ ± 𝑝ˆ ℎ𝑖 = 0.1003 ± 0.0236. That is, our point estimate for
 





Combination of R2 such that |Bias| < 0.0236











Figure 12.6: Sensitivity contour plot: The graph shows values
 
of 𝑅2
𝑌˜ ∼𝐴˜|𝐷˜
 
and 𝑅2
𝐷˜ ∼𝐴˜
 
that give
 
a given value of the bias 𝜙ˆ =
0.0236. in the Darfur example.
The value 0.0236 was chosen as this corresponds to the es-timated bias in our benchmark
 
scenario for 𝑅2
𝑌˜ ∼𝐴˜|𝐷˜
 
and 𝑅2	.
𝐷˜ ∼𝐴˜
 
0.00	0.05	0.10	0.15	0.20	0.25	0.30
Partial R2 of Treatment with Confounder
 
All points below the plotted con-tour would correspond to esti-mated bias smaller than 0.0236.
 

 
 

12.3	Partially Linear IV Models
When instrumental variables are available, it becomes possible to point identify causal effects in partially linear models and certain types of causal effects in nonlinear models. Here we begin with partially linear models.

A Wage Equation with Unobserved Ability


Figure 12.7: An IV model with observed and unobserved con-founders.













Examples of instruments for schooling, 𝑍, that have appeared in the literature include
▶ distance to college (Card [8]),
▶ compulsory schooling laws (Angrist [9]),
▶ offer to participate/offer to treat in a training program (Bloom et al. [10]), and
▶ local earnings and unemployment at age 17 (Cameron and Heckman [11]).
We apply the partialling-out operator to get rid of the 𝑋’s in all of the equations. As before, we define the partialling out operation of any random vector 𝑉 with respect to another
 


random vector 𝑋 as the residual that is left after subtracting the best predictor of 𝑉 given 𝑋:

 
𝑉˜
 
= 𝑉 − E[𝑉 | 𝑋].
 
If 𝑓 ’s are linear, we replace E[𝑉 | 𝑋] with linear projection. After partialling-out, we have a simplified system.
 
𝑌˜
 
:=	𝛼𝐷˜ + 𝛿𝐴˜ + 𝜖𝑌 ,
 
𝐷˜
𝑍˜
𝐴˜
 
:=	𝛽𝑍˜ + 𝛾𝐴˜ + 𝜖𝐷 ,
:=	𝜖𝑍,
:=	𝜖𝐴,
 
where 𝜖𝑌, 𝜖𝐷, 𝜖𝑍, and 𝜖𝐴 are uncorrelated. We immediately obtain the following result:






 


Wright’s Causal Path Derivation
Starting from the DAG given in Figure 12.7, we obtain Figure
12.8 after partialling out.
 
𝑍˜
 
𝐷˜
 
𝑌˜
 
Figure 12.8: DAG corresponding to Figure 12.7 after partialling out observed confounder 𝑋.
 


Philip Wright (1928) [1] observed that the structural param-eter 𝛽𝛼, the effect 𝑍˜ → 𝑌˜ , is identified from the projection of 𝑌˜ ∼ 𝑍˜ :	2
𝛽𝛼 = E[𝑌˜ 𝑍˜ ]/E[𝑍˜ ].
The structural parameter 𝛽, the effect of 𝑍 → 𝐷, is identified from the projection of 𝐷˜ ∼ 𝑍˜ :
𝛽 = E[𝐷˜ 𝑍˜ ]/E[𝑍˜ 2].
𝛼, the effect of 𝐷 → 𝑌, is then identified by the ratio of the two provided 𝛽 ≠ 0:
𝛽𝛼
𝛼 = 𝛽 = E[𝑌˜ 𝑍˜ ]/E[𝐷˜ 𝑍˜ ].

We provide a thorough discussion of using DML to estimate parameters within the instrumental variables framework along with example applications in Chapter 13.

 
Aggregate Market Demand
Let’s apply our approach to a canonical example in economics: the identification of the price elasticity of demand using a supply shifter as an instrument.
 



𝑍˜
 



𝐷˜
 



𝑌˜
 

 

Figure 12.9: A DAG for aggregate demand, with the latent node 𝜖𝑑 representing the demand shock










In econometrics, the set-up here is sometimes referred to as a limited information model or formulation because we are focusing on iden-tifying only a single equation in a more complicated underlying sys-tem.
 


Example 12.3.2 is equivalent to the previous Example 12.3.1 – set 𝐴 = 𝜖𝑑, 𝜖𝑌 = 0, 𝜖𝑠 = 𝜖𝐷, and so on. Hence, the identification method is the same as before.

Limits of Average Causal Effect Identification under Partial Linearity
The result in Theorem 12.3.1 extends beyond the partially linear setting presented in Example 12.3.1 to the following non-linear structural equation model:

Theorem 12.3.1 extends almost as is to this more general non-linear structural equation model.

 


Note that the non-linear structural equation model in Exam-ple 12.3.3 imposes extra assumptions on the structural response function of the outcome 𝑌. Thus our identification argument imposes more conditions on the structural equations than the ones that can be encoded via a DAG. Such auxiliary assumptions are required for identification of average treatment effects with instruments.
In particular, the identification argument relies on the fact that the unobserved confounder 𝐴 enters in an additively separable manner in the outcome equation. If for instance, 𝐴 was an input to the function 𝑔, i.e. 𝑌 := 𝑔𝑌 𝐴, 𝜖𝑌 𝐷   𝑓𝑌 𝐴, 𝑋, 𝜖𝑌 , then the
quantity identified by the moment restriction in Theorem 12.3.2
would not correspond to an average treatment effect. In this case, the unobserved confounder creates heterogeneity in the treat-ment effect and also heterogeneity in the effect of the instrument on the treatment, typically referred to as the "compliance" (i.e., the correlation between 𝑍 and 𝐷 varies with 𝐴). This property
is what renders the ratio quantity 𝛼 = E 𝑌˜ 𝑍˜  E 𝐷˜ 𝑍˜ invalid
for the causal estimand of interest.
In fact, it is the joint heterogeneity in both the outcome relation-ship and the compliance relationship that causes the problem. We show next that we could allow for a much more complex outcome model as long as the effect of the instrument on the treatment (compliance) is not heterogeneous in 𝐴 or 𝑋.


 


 
Thus, we see that we need that either the effect of education on wages is not heterogeneous in the unobserved ability variable
𝐴 or that the effect of the observed education shifter 𝑍 (e.g. distance to college) on education 𝐷 is not heterogeneous in the unobserved ability variable to use the identification strategies presented in this section in the context of our education exam-ple. In Section 12.4, we will investigate what causal quantities are identifiable even in non-linear structural equation models, where the unobserved confounder creates heterogeneity in both the treatment effect and in the compliance behavior.

 


 
12.4	Nonlinear IV Models
Once we consider nonlinear models, identification becomes a much more delicate matter. We first consider the local average treatment effect (LATE) model, and then we turn to quantile models.

The LATE Model
An important nonlinear IV model in the case of a binary treat-ment variable and a binary instrumental variable is the local average treatment effect model (LATE) proposed by Imbens and Angrist [17].

Figure 12.10: LATE models. Green arrow denotes a monotone func-tional relation.












Define
𝑌(𝑑) := 𝑓𝑌(𝑑, 𝑋, 𝐴, 𝜖𝑌) and 𝐷(𝑧) := 𝑓𝐷(𝑧, 𝑋, 𝐴, 𝜖𝐷)
as the potential outcomes that result from applying fix-interventions in the corresponding equations from Example 12.4.1.
 


The model allows us to identify the local average treatment effect (LATE), defined as
𝜃 = E[𝑌(1) − 𝑌(0) | 𝐷(1) > 𝐷(0)].
Here, 𝐷 1 > 𝐷 0  is the compliance event which corresponds to the case where exogenously switching the instrument value from 𝑍 = 0 to 𝑍 = 1 induces a switch from the control state to
the treatment state. Therefore, the LATE measures the average
treatment effect conditional on compliance.


 
The result has an intuitive interpretation.2 In the event of compliance, the instrument moves the treatment as if experi-mentally, which induces quasi-experimental variation in the outcome. We measure the probability of compliance with 𝜃2 and the average induced changes in outcome by 𝜃1. Taking the ratio is then like conditioning on the compliance event. See the proof in Section 12.A for details.
The ratio can be recognized as the ratio of average treatment effects of 𝑍 on 𝑌 and 𝐷,
𝜃1 = 𝐴𝑇𝐸(𝑍 → 𝑌),
𝜃2 = 𝐴𝑇𝐸(𝑍 → 𝐷).
This assertion follows from the application of the backdoor cri-terion. Therefore, we can simply re-use the tools for performing inference on the two ATEs to perform inference on the LATE.
 
2: In the model with no 𝑋 the ratio
𝜃1 𝜃2 is equivalent to Wright’s [1] IV estimand.
 


 

The IV Quantile Model★
Another nonlinear IV model is the following model that ex-ploits monotonicity in the unobservable shock in the outcome equation to obtain identification.



Figure 12.11: IV Quantile Model. The green arrow represents a strictly monotonic effect.





















The testable implication of the IV Quantile Model is the follow-ing.

 


 
In practice, linear forms 𝑓𝑌 𝐷, 𝑋, 𝑢 = 𝛼 𝑢 ′𝐷  𝛽 𝑢 ′𝑋 are often used. Adopting a linear functional form leads to method of moments approaches such as the IV quantile regression for
performing inference on structural quantile functions.	Code for IV Quantile Models can
be found here.

12.5	Partially Linear SEMs with Griliches-Chamberlain Proxy Controls

Suppose we are interested in the causal effect of college educa-tion on earnings in the presence of an unobserved confounder – individual ability. Here we show that we can recover the effect of college education on earnings in the presence of latent ability using proxies for ability, but not the effect of ability itself.
𝑄	𝑆

𝐷	𝑌

𝑋


Figure 12.12: A DAG with Controls and Proxy Controls
 













 
After partialling out we are left with the DAG in Figure 12.13:
 
𝑄˜
 
𝑆˜
 
𝑌˜	:=	𝛼𝐷˜ + 𝛿𝐴˜ + 𝜄𝑆˜ + 𝜖𝑌 ,
 
𝐷˜
𝑄˜
𝑆˜
 
:=	𝛾𝐴˜ + 𝛽𝑄˜ + 𝜖𝐷 ,
:= 𝜂𝐴˜ + 𝜖𝑄 ,
:=	𝜙𝐴˜ + 𝜖𝑆,
 
𝐷˜
 
𝑌˜
 
𝐴˜
 
:=	𝜖𝐴,
 
Figure 12.13: A DAG with Proxy
 
where 𝜖𝑌 , 𝜖𝐷 , 𝜖𝑄 , 𝜖𝑆, 𝜖𝐴 are uncorrelated. The idea now is to
 
Controls After Partialling Out
 
replace 𝐴˜ in the equation for 𝑌˜ with 𝑆˜. Note that because 𝑆
enters the 𝑌 equation directly, we cannot consider using 𝑄˜
to proxy for 𝐴˜. We still cannot learn 𝛼 from the regression of
𝑌˜ on 𝐷˜ and 𝑆˜ though as 𝑆 is an imperfect proxy for 𝐴. The
 
following result, which provides an IV approach to identify 𝛼, is immediate via substitution.3
 

3: Prove the result as a reading ex-ercise. Substitute 𝐴˜ = 𝑆˜ 𝜖𝑆 𝜙 in the first equation and use the
assumptions on the disturbances.
 











Note that 𝑄˜ here plays the role of a technical instrument for
𝑆˜. This approach recovers 𝛼, but not 𝛿. For inference, we can
employ the DML method for IV models; see Chapter 13.

 


 

The Notebooks 12.7.2 provide code for the birth weight proxy controls example using data downloaded from Stat Labs.



























The posited model is very stylized, but illustrates the main ideas and thought process.

12.6	Nonlinear Models with Proxy Controls★
A relatively recent literature considers proximal causal inference, which generalizes early work by Griliches and Chamberlain [19].
 


See, among others, [20], [21], [22], [23], [3]. Here we describe some results specialized to the discrete case.
𝑄	𝑆

𝐷	𝑌
Figure 12.14: A SEM with Proxy Controls 𝑄 and 𝑆. Note that condi-tioning on 𝑄 and 𝑆 does not block the backdoor path 𝑌  𝐴  𝐷, hence we cannot use the regression adjustment method for identifica-tion of 𝐷 → 𝑌.



Here we can introduce background exogenous controls 𝑋 in each of the equations, but we don’t do so to save notation. Notice that the model in Example 12.6.1 generalizes the Example 12.5.1 to the nonparametric case.

Condition (b) is analogous to the usual relevance condition in IV and basically says that the two proxies 𝑆 and 𝑄 have sufficient joint variation at any value of 𝑑 to allow 𝑄 to serve as an "instrument" for 𝑆. The discreteness assumption can be generalized to a more general completeness condition; see, e.g., Miao et al. [3] and Deaner [4]. As with the usual IV relevance condition, Condition (b) is testable from the data. In contrast, the DAG itself and the other conditions involve an unobserved variable 𝐴 and are therefore generally untestable. The validity of these untestable conditions must be assessed using contextual knowledge about the empirical problem.

 


 
The mnemonic way to think about the formula above is that we are doing a kind of instrumental variable regression of 𝑌 on 𝑆, while instrumenting 𝑆 with 𝑄, which is exactly how we dealt with the linear version of this problem in Section 12.6.


12.7	Notebooks
Notebook 12.7.1 (DML Sensitivity) DML Sensitivity R Note-book analyses the sensitivity of the DML estimate in the Darfur wars example to unobserved confounders using the Sensemakr package in R. DML Sensitivity Python Notebook does the same analysis in Python.

Notebook 12.7.2 (DML for Linear Proxy Controls) DML for Linear Proxy Controls R Notebook and DML for Linear Proxy Controls Python Notebook provide an application of using linear instrumental variables estimation withing the proxy controls framework to estimate the effect of smoking on birth weight.


12.8	Exercises
Exercise 12.8.1 (Omitted Confounder Bias) Explain omitted confounder bias to a fellow student (one paragraph). Explore using sensitivity analysis to aid in understanding robust-ness of economic conclusions to the presence of unobserved confounders in an empirical example of your choice. The Notebooks 12.7.1 can be a helpful starting point, but be sure to apply the ideas to a different empirical example. (You could use any of the previous examples we have analyzed).
 


Exercise 12.8.2 (Instrumental Variables) Write a brief expla-nation of the idea of the instrumental variables regression model that would be appropriate for educating a fellow stu-dent. Discuss the idea of identifying the causal effect in this setting via path analysis in the spirit of what Philip Wright did. Illustrate your discussion within a concrete empirical setting.

Exercise 12.8.3 (Simulation IV or Proxy Controls) Create a notebook to simulate one of the linear IV or proxy controls models that we’ve described. Assume there are no 𝑋’s for simplicity. Demonstrate numerically why using least squares may not be appropriate due to unobserved confounding. Demonstrate numerically how using instrumental variable regression overcomes the issue.

Exercise 12.8.4 (LATE etc.) Write a verbal explanation of one of the nonlinear models (e.g. LATE, IV quantile model, or the nonlinear model with proxy controls) that would be understandable by a fellow student. Be sure that the explanation includes an intuitive discussion of how causal parameters in these models are identified.
 

12.A Proofs
Latent Confounder Bias Result: Theorem 12.2.1
The proof heavily relies on the Frisch-Waugh-Lovel partialling out theorem (FWL) and the normalization on the variance of the latent confounder:
E[𝐴˜2] = 1.	(12.A.1)
 
The proof also relies on the properties of 𝑅2
𝑈∼𝑉
 
which measures
 
the proportion of variance of centered random variable 𝑈 that is linearly explained by another centered random variable 𝑉:
 
2	𝐸[𝛽2𝑉2]
 
E[𝜖2]
 
(E[𝑈𝑉])2	2
 
𝑅𝑈∼𝑉 =  E[𝑈2]  = 1 − E[𝑈2] = E[𝑈2]E[𝑉2] = [Cor(𝑈, 𝑉)] ,
where 𝛽 = E[𝑉𝑈]/E[𝑉2] is the coefficient of the best linear projection of 𝑈 onto 𝑉, 𝜖 = 𝑈 − 𝛽𝑉 is the projection residual, and Cor(𝑈, 𝑉) denotes the correlation between 𝑈 and 𝑉. Note
 
that 𝑅2 is symmetric in 𝑈 and 𝑉: 𝑅2
𝑈∼𝑉
 
= 𝑅2	.
𝑉∼𝑈
 
By FWL and the normalization (12.A.1), we have
𝛾 = E[𝐴˜𝐷˜ ],	𝛿 = E[𝐴¯𝑌¯ ]/E[𝐴¯2],
 
where
 
𝑌¯ = 𝑌˜ − 𝛽𝐷˜ ;	𝛽 = E[𝑌˜ 𝐷˜ ]/E[𝐷˜ 2];
 
𝐴¯ = 𝐴˜ − 𝛽˜𝐷˜ ;	𝛽˜ = E[𝐴˜𝐷˜ ]/E[𝐷˜ 2].
It follows that
 
2	𝛾2 𝛿2
 
(E[𝐴˜𝐷˜ ])2 (E[𝑌¯ 𝐴¯])2
 
𝜙 = (E[𝐷˜ 2])2 = (E[𝐷˜ 2])2 (E[𝐴¯2])2 .
Then the result follows from the normalization (12.A.1);the relations
(E[𝐷˜ 𝐴˜])2 = [Cor(𝐷˜ , 𝐴˜)]2E[𝐷˜ 2] = 𝑅2	E[𝐷˜ 2],
(E[𝑌¯ 𝐴¯])2 = [Cor(𝑌¯ , 𝐴¯)]2E[𝑌¯ 2]E[𝐴¯2] = 𝑅2	E[𝑌¯ 2]E[𝐴¯2],
 
E 𝐴2 = 1	𝑅2
𝐴˜∼𝐷˜
 
= 1	𝑅2	;
𝐷˜ ∼𝐴˜
 
and by noting that by definition 𝑅2
𝑌¯ ∼𝐴¯
 
= 𝑅2	.
𝑌˜ ∼𝐴˜|𝐷˜
 

Partially Linear Outcome IV Model: Theorem 12.3.2

 
First note that since E 𝑍˜
condition as
 
| 𝑋] = 0, we can re-write the moment
 
E[(𝑌 − 𝛼𝐷)𝑍˜ ] = 0.
We can use the structural equation for 𝑌 to replace 𝑌 in the moment equation:
E[(𝑔𝑌(𝜖𝑌)𝐷 + 𝑓𝑌(𝐴, 𝑋, 𝜖𝑌) − 𝛼𝐷)𝑍˜ ] = 0.
Furthermore, since 𝑍˜ ⊥ 𝐴, 𝜖𝑌 | 𝑋, we have that
E[ 𝑓𝑌(𝐴, 𝑋, 𝜖𝑌)𝑍˜ ] = E[ 𝑓𝑌(𝐴, 𝑋, 𝜖𝑌)E[𝑍˜ | 𝑋, 𝐴, 𝜖𝑌]]
= E[ 𝑓𝑌(𝐴, 𝑋, 𝜖𝑌)E[𝑍˜ | 𝑋]] = 0.
Thus, we can re-write the moment equation as
E[(𝑔𝑌(𝜖𝑌)𝐷 − 𝛼𝐷)𝑍˜ ] = 0.
Solving for 𝛼 and using the fact that 𝜖𝑌 ⊥ 𝑍˜ , we get
𝛼 = E[𝑔𝑌(𝜖𝑌)𝐷𝑍˜ ] = E[𝑔𝑌(𝜖𝑌)]E[𝐷𝑍˜ ] = E[𝑔𝑌(𝜖𝑌)].
 	 
E[𝐷𝑍˜ ]	E[𝐷𝑍˜ ]
Partially Linear Compliance IV Model: Theorem 12.3.3
Using the exact same arguments as in the proof of Theo-rem 12.3.2, we can deduce that the solution to the moment restriction takes the form
𝛼 = E[𝑔𝑌(𝑋, 𝐴, 𝜖𝑌)𝐷𝑍˜ ] = E[𝑔𝑌(𝑋, 𝐴, 𝜖𝑌)E[𝐷𝑍˜ | 𝑋, 𝐴, 𝜖𝑌]] .
 	 
E[𝐷𝑍˜ ]	E[𝐷𝑍˜ ]
We now use the assumptions on the structural response func-
tions of 𝐷 and 𝑍 to argue that E[𝐷𝑍˜ | 𝑋, 𝐴, 𝜖𝑌] = E[𝐷𝑍˜ ] –
that is, to argue the covariance of 𝐷 and 𝑍 (aka compliance) is independent of 𝑋, 𝐴, 𝜖𝑌. This independence then implies the theorem, since
𝛼 = E[𝑔𝑌(𝑋, 𝐴, 𝜖𝑌)].

First, we use the assumption on the structural response function
 


of 𝐷:
E[𝐷𝑍˜ | 𝑋, 𝐴, 𝜖𝑌] = E[(𝑔𝐷(𝜖𝐷)𝑍 + 𝑓𝐷(𝑋, 𝐴, 𝜖𝐷))𝑍˜ | 𝑋, 𝐴, 𝜖𝑌].
Using the fact that 𝑍 ⊥ 𝐴, 𝜖𝐷 , 𝜖𝑌 | 𝑋 and that E[𝑍˜ | 𝑋] = 0, we
can remove the term 𝑓𝐷(𝑋, 𝐴, 𝜖𝐷) from the above equation:
E[𝐷𝑍˜ | 𝑋, 𝐴, 𝜖𝑌] = E[𝑔𝐷(𝜖𝐷)𝑍𝑍˜ | 𝑋, 𝐴, 𝜖𝑌].
Using the additively separable assumption on the structural response of 𝑍 and the fact that 𝜖𝑍 is an exogenous independent variable, we have
E[𝐷𝑍˜ | 𝑋, 𝐴, 𝜖𝑌] = E[𝑔𝐷(𝜖𝐷)𝑍𝜖𝑍 | 𝑋, 𝐴, 𝜖𝑌]
= E[𝑔𝐷(𝜖𝐷)𝜖2  | 𝑋, 𝐴, 𝜖𝑌] = E[𝑔𝐷(𝜖𝐷)𝜖𝑍]
where we used the fact that all noise variables 𝜖𝐷 , 𝜖𝑌 , 𝜖𝑍 are exogenous and mutually independent.

 
Linear Proxy Model: Theorem 12.5.1.
We substitute 𝐴˜ = (𝑆˜ − 𝜖𝑆)/𝜙 in the equation 𝑌˜ = 𝛼𝐷˜
𝜄𝑆˜ + 𝜖𝑌 to obtain
 


+ 𝛿𝐴˜ +
 

where
 
𝑌˜ = 𝛼𝐷˜
 
+ 𝛿¯𝑆˜ + 𝑈
 
𝑈 = −𝛿𝜖𝑆/𝜙 + 𝜖𝑌 and 𝛿¯ = 𝜄 + 𝛿/𝜙.
To verify
E [𝑈] = 0,
we observe using repeated substitutions that
▶ 𝐷˜ is a linear combination of 𝜖𝐴, 𝜖𝑄 , 𝜖𝐷 ,
▶ 𝑄˜ is a linear combination of 𝜖𝐴 and 𝜖𝑄.
▶ 𝑈 is a linear combination of (𝜖𝑆, 𝜖𝑌).
The conclusion follows from the assumption that
(𝜖𝐴, 𝜖𝑄 , 𝜖𝐷 , 𝜖𝑆, 𝜖𝑌)
are all uncorrelated. The conclusion that 𝛼 is identified provided
that 𝐷˜ and the best linear predictor of 𝑆˜ using 𝑄˜ and 𝐷˜ have
non-degenerate covariance matrices is left as an exercise.
 

The LATE Result: Theorem 12.4.1
We can use, for example, the backdoor criterion to conclude that
E[E[𝐷 | 𝑍 = 𝑧, 𝑋]] = E[E[𝐷(𝑧) | 𝑋]] = E[𝐷(𝑧)].
Similarly,
E[E[𝑌 | 𝑍 = 𝑧, 𝑋]] = E[E[𝑌(𝐷(𝑧)) | 𝑋]] = E[𝑌(𝐷(𝑧))].
Furthermore, by monotonicity, we have both
𝜃2 = E[𝐷(1) − 𝐷(0)] = P(𝐷(1) > 𝐷(0))
 
and



Therefore
 


𝜃1 = E[𝑌(𝐷(1)) − 𝑌(𝐷(0))]
= E[(𝑌(1) − 𝑌(0))1{𝐷(1) > 𝐷(0)}].


𝜃1/𝜃2 = E[𝑌(1) − 𝑌(0) | 𝐷(1) > 𝐷(0)].
 

Testable Restriction for the IV Quantile Model: Theorem 12.4.2
The result is immediate from (i) the equivalence of the event
𝑌	𝑓𝑌 𝐷, 𝑋, 𝑢 and the event 𝜖𝑌	𝑢, which holds under the strict monotoniticity assumption, and (ii) the independence of
𝜖𝑌 from 𝑍 and 𝑋 which follows from the stated independence
conditions. Using (i) and (ii), we have
P[𝑌 ≤ 𝑓𝑌(𝐷, 𝑋, 𝑢) | 𝑍, 𝑋] = P[𝜖𝑌 ≤ 𝑢 | 𝑍, 𝑋]
= P[𝜖𝑌 ≤ 𝑢] = P[𝑈(0, 1) ≤ 𝑢] = 𝑢.

Identification in the Nonlinear Proxy Variables Model: Theorem 12.6.1
To sketch a proof, the DAG implies that the observed vari-ables 𝐷, 𝑌, 𝑄, 𝑆 and the unobserved variable 𝐴 obey the two conditional independence relations:
(𝑖)	𝑆 ⊥ (𝑄, 𝐷) | 𝐴	(𝑖𝑖)	𝑄 ⊥ 𝑌 | (𝐴, 𝐷).	(12.A.2)
 


These in turn imply
Π(𝑆 | 𝑄, 𝑑) = Π(𝑆 | 𝑄, 𝐴, 𝑑)Π(𝐴 | 𝑄, 𝑑)
= Π(𝑆 | 𝐴)Π(𝐴 | 𝑄, 𝑑)
and
Π(𝑦 | 𝑄, 𝑑) = Π(𝑦 | 𝑄, 𝐴, 𝑑)Π(𝐴 | 𝑄, 𝑑)
= Π(𝑦 | 𝐴, 𝑑)Π(𝐴 | 𝑄, 𝑑).
We now want to solve these equations for Π 𝑦 𝐴, 𝑑 in terms of quantities that could be learned in the data.
We will need invertibility of Π 𝑆 𝑄, 𝑑 which requires in-vertibility of both Π 𝑆 𝐴 and Π 𝐴 𝑄, 𝑑 . Under these invertibility conditions, we have
Π(𝐴 | 𝑄, 𝑑) = Π(𝑆 | 𝐴)−1Π(𝑆 | 𝑄, 𝑑)
and
Π(𝑦 | 𝑄, 𝑑) = Π(𝑦 | 𝐴, 𝑑)Π(𝑆 | 𝐴)−1Π(𝑆 | 𝑄, 𝑑),
which yield
Π(𝑦 | 𝐴, 𝑑) = Π(𝑦 | 𝑄, 𝑑)Π(𝑆 | 𝑄, 𝑑)−1Π(𝑆 | 𝐴).

Next, because 𝐴 blocks backdoor paths between 𝐷 and 𝑌, we have that
𝑝(𝑦 | 𝑎 : 𝑑𝑜(𝑑)) = 𝑝(𝑦 | 𝑎, 𝑑)	(12.A.3) or, after integrating out 𝑎,
𝑝(𝑦 : 𝑑𝑜(𝑑)) = Π(𝑦 | 𝐴, 𝑑)Π(𝐴),
which can be further expressed as
Π(𝑦 | 𝑑, 𝑄) Π(𝑆 | 𝑄, 𝑑)−1 Π(𝑆) ,	(12.A.4) using the derivations above.
 


Bibliography



[1]	Philip G. Wright. The Tariff on Animal and Vegetable Oils. New York: The Macmillan company, 1928 (cited on pages 331, 338, 343).
[2]	Zvi Griliches. ‘Estimating the returns to schooling: Some econometric problems’. In: Econometrica 45.1 (1977), pp. 1–
22 (cited on pages 332, 336, 345).
[3]		Wang Miao, Zhi Geng, and Eric J. Tchetgen Tchetgen. ‘Identifying causal effects with proxy variables of an unmeasured confounder’. In: Biometrika 105.4 (2018),
pp. 987–993 (cited on pages 332, 348).
[4]	Ben Deaner. ‘Proxy controls and panel data’. In: arXiv preprint arXiv:1810.00283 (2018) (cited on pages 332, 348).
[5]		Carlos Cinelli and Chad Hazlett. ‘Making sense of sen-sitivity: Extending omitted variable bias’. In: Journal of the Royal Statistical Society: Series B 82.1 (2020), pp. 39–67 (cited on pages 333–335).
[6]		Victor Chernozhukov, Carlos Cinelli, Whitney Newey, Amit Sharma, and Vasilis Syrgkanis. ‘Long Story Short: Omitted Variable Bias in Causal Machine Learning’. In: arXiv preprint arXiv:2112.13398 (2023) (cited on page 333).
[7]		Chad Hazlett. ‘Angry or Weary? How Violence Impacts Attitudes toward Peace among Darfurian Refugees’. In: Journal of Conflict Resolution 64.5 (2020), pp. 844–870. doi: 10.1177/0022002719879217 (cited on page 334).
[8]		David Card. ‘Using Geographic Variation in College Proximity to Estimate the Return to Schooling’. In: Aspects of Labor Market Behavior: Essays in Honor of John Vanderkamp. Ed. by L. N. Christofides and R. Swidinsky. Toronto: University of Toronto Press, 1995, pp. 201–222 (cited on page 336).
[9]		Joshua D. Angrist and Alan B. Krueger. ‘Does Compul-sory School Attendance Affect Schooling and Earnings?’ In: Quarterly Journal of Economics 106.4 (1991), pp. 979–1014 (cited on page 336).
 


[10]		Howard S. Bloom, Larry L. Orr, Stephen H. Bell, George Cave, Fred Doolittle, Winston Lin, and Johannes M. Bos. ‘The Benefits and Costs of JTPA Title II-A Programs: Key Findings from the National Job Training Partnership Act Study’. In: The Journal of Human Resources 32.3 (1997),
pp. 549–576. (Visited on 04/01/2024) (cited on page 336).
[11]	Stephen V. Cameron and James J. Heckman. ‘Life Cycle Schooling and Dynamic Selection Bias: Models and Ev-idence for Five Cohorts of American Males’. In: Journal of Political Economy 106.2 (1998), pp. 262–333. (Visited on 04/01/2024) (cited on page 336).
[12]		Alberto Abadie. ‘Semiparametric instrumental variable estimation of treatment response models’. In: Journal of Econometrics 113.2 (2003), pp. 231–263 (cited on page 341).
[13]	Peter M. Aronow and Allison Carnegie. ‘Beyond LATE: Estimation of the Average Treatment Effect with an Instru-mental Variable’. In: Political Analysis 21.4 (2013), pp. 492–
506. (Visited on 02/27/2023) (cited on page 341).
[14]	Ryo Okui, Dylan S. Small, Zhiqiang Tan, and James M. Robins. ‘Doubly Robust Instrumental Variable Regres-sion’. In: Statistica Sinica 22.1 (2012), pp. 173–205. (Visited on 02/27/2023) (cited on page 341).
[15]	Susan Athey and Stefan Wager. ‘Policy learning with observational data’. In: Econometrica 89.1 (2021), pp. 133–
161 (cited on page 341).
[16]		Vasilis Syrgkanis, Victor Lei, Miruna Oprescu, Maggie Hei, Keith Battocchi, and Greg Lewis. ‘Machine learn-ing estimation of heterogeneous treatment effects with instruments’. In: Advances in Neural Information Processing Systems 32 (2019) (cited on page 341).
[17]		Guido W. Imbens and Joshua D. Angrist. ‘Identification and Estimation of Local Average Treatment Effects’. In: Econometrica 62.2 (1994), pp. 467–475 (cited on page 342).
[18]		Victor Chernozhukov, Christian Hansen, and Kaspar Wuthrich. ‘Instrumental variable quantile regression’. In: arXiv preprint arXiv:2009.00436 (2020) (cited on page 345).
[19]		Gary Chamberlain and Zvi Griliches. More on brothers. In “Kinometrics: Determinants of Socioeconomic Success Within and Between Families “(P. Taubman, Ed.) 1977 (cited on pages 345, 347).
[20]		Marc Lipsitch, Eric Tchetgen Tchetgen, and Ted Cohen. ‘Negative controls: a tool for detecting confounding and bias in observational studies’. In: Epidemiology 21.3 (2010),
pp. 383–388 (cited on page 348).
 


[21]	Ben Deaner. ‘Proxy controls and panel data’. In: arXiv preprint arXiv:1810.00283 (2018) (cited on page 348).
[22]		Nathan Kallus, Xiaojie Mao, and Masatoshi Uehara. ‘Causal inference under unmeasured confounding with negative controls: A minimax learning approach’. In: arXiv preprint arXiv:2103.14029 (2021) (cited on pages 348, 349).
[23]		Yifan Cui, Hongming Pu, Xu Shi, Wang Miao, and Eric Tchetgen Tchetgen. ‘Semiparametric Proximal Causal In-ference’. In: Journal of the American Statistical Association 0.0 (2023), pp. 1–12. doi: 10.1080/01621459.2023.2191817
(cited on pages 348, 349).
 

 
DML for IV and Proxy Controls Models and Robust DML Inference under Weak
Identification


"Better LATE than nothing." – Guido Imbens [1].









Here, we specialize DML methods to partially linear models with instruments, arising either through endogeneity of the policy variable or through the use of proxy controls as outlined in Chapter 12. We also present DML methods for LATE param-eters in the fully nonlinear model with a binary endogenous treatment and binary instrument. We further examine how DML inference method can be modified to cope with weak in-struments and weak identification in generic moment problems through the use of Neyman orthogonal scores and Neyman’s
𝐶(𝛼) statistic.
 
13
13.1	DML Inference in Partially Linear IV Models	360
The Effect of Institutions on Economic Growth	362
13.2	DML Inference in the In-teractive IV Regression Model (IRM)	365
DML Inference on LATE365 The Effect of 401(k) Participa-tion on Net Financial Assets366
13.3	DML Inference with Weak Instruments	368
Motivation	368
DML Inference Robust to Weak-IV in PLMs	370
The Effect of Institutions on Economic Growth Revis-ited	372
13.4	Generic DML Inference un-der Weak Identification . 373 13.5Notebooks	375
13.6Exercises	376
 

13.1	DML Inference in Partially Linear IV Models
Here we consider estimation of parameters that obey the fol-lowing instrumental variable exclusion restriction:
E[𝜖𝑍˜ ] = 0,
 
where and
 

𝜖 := 𝑌˜ − 𝜃0′ 𝐷˜ ,

𝑌˜ = 𝑌 − ℓ0(𝑋),	ℓ0(𝑋) = E[𝑌 | 𝑋],
 
𝐷˜ = 𝐷 − 𝑟0(𝑋),	𝑟0(𝑋) = E[𝐷 | 𝑋],
𝑍˜ = 𝑍 − 𝑚0(𝑋),	𝑚0(𝑋) = E[𝑍 | 𝑋].
Here we take the dimension of 𝑍˜ to be the same as that of 𝐷˜
for simplicity.
Two key examples leading to this statistical structure are
▶ the partially linear instrumental variable model and
▶ the partially linear model with proxy controls.
We discussed these examples and showed they fit into this structure in Chapter 12.
To estimate and perform inference on 𝜃0, we can apply the general DML algorithm with the score
𝜓(𝑊; 𝜃, 𝜂) := (𝑌 − ℓ(𝑋) − 𝜃′(𝐷 − 𝑟(𝑋)))(𝑍 − 𝑚(𝑋)), (13.1.1) where 𝑊 = 𝑌, 𝐷, 𝑋, 𝑍  and 𝜂 = ℓ , 𝑚, 𝑟  with ℓ , 𝑚, and 𝑟
being 𝑃-square-integrable functions mapping the support of 𝑋
to ℝ. Under the exclusion restriction and using the definition of the nuisance functions, we have that
E[𝜓(𝑊; 𝜃0, 𝜂0)] = 0.
It is not difficult to check that the Neyman orthogonality condi-tion,
𝜕𝜂E[𝜓(𝑊; 𝜃0, 𝜂0)] = 0,
holds at the true value 𝜂0 = ℓ0, 𝑚0, 𝑟0 of the nuisance parameters.	Verify Neyman-orthogonality for
yourself as an exercise.
We now explicitly restate the DML algorithm specialized to this case of partially linear IV models.
 


DML for Partially Linear IV and Proxy Models
1.	Partition data indices into 𝑘 folds of approximately equal size: {1, ..., 𝑛} = ∪𝐾  𝐼𝑘. For each fold 𝑘 =
𝑘=1
1, ..., 𝐾, compute ML estimators ℓˆ[𝑘](𝑋), 𝑚ˆ [𝑘](𝑋),
𝑟ˆ[𝑘](𝑋) of the best predictors ℓ0(𝑋), 𝑚0(𝑋), 𝑟0(𝑋), leav-ing out the 𝑘-th block of data, and obtain the cross-fitted residuals for each 𝑖 ∈ 𝐼𝑘:
𝑌ˇ𝑖 = 𝑌𝑖 − ℓˆ[𝑘](𝑋𝑖),
𝐷ˇ 𝑖 = 𝐷𝑖 − 𝑟ˆ[𝑘](𝑋𝑖),
𝑍ˇ 𝑖 = 𝑍𝑖 − 𝑚ˆ [𝑘](𝑋𝑖).
2.	Compute the standard IV regression of 𝑌ˇ𝑖 on 𝐷ˇ 𝑖 using
𝑍ˇ 𝑖 as the instrument. That is, obtain 𝜃ˆ as the root in
𝜃 of the following equation:
𝔼𝑛[(𝑌ˇ − 𝜃′𝐷ˇ )𝑍ˇ ] = 0.
3.	Construct standard errors and confidence intervals as in the standard linear instrumental variables re-gression theory.

In what follows it will be convenient to use the following notation		
∥ℎ∥𝐿2 :=	E𝑋[ℎ2(𝑋)],
where, as before, E𝑋 computes the expectation over values of
𝑋.

 


 
 
The standard error of 𝜃ˆ is estimated as Vˆ 𝑛, where Vˆ is an estimator of V based on the plug-in principle. The result implies that the confidence interval
[𝜃ˆ − 2JVˆ/𝑛, 𝜃ˆ + 2JVˆ/𝑛]
covers 𝜃 for approximately 95% of the realizations of the sample. In other words, if our sample is not atypical, the interval covers the truth.

 
The Effect of Institutions on Economic Growth
To demonstrate DML estimation of partially linear structural equation models with instrumental variables, we consider es-timating the effect of institutions on aggregate output follow-ing the work of Acemoglu, Johnson, and Robinson (2001) [3] (AJR).
We use the same set of 64 country-level observations as AJR. The outcome variable, 𝑌, is the logarithm of GDP per capita and the endogenous explanatory variable, 𝐷, is an index which measures protection against expropriation risk that is used as a proxy for the strength of institutions. To deal with endogeneity, we use an instrumental variable 𝑍, which is mortality rates for early European settlers. Our raw set of control variables, 𝑋, include distance from the equator and dummy variables for Africa, Asia, North America, and South America.
Estimating the effect of institutions on output is complicated by the clear potential for simultaneity between institutions and output: Better institutions may generate higher incomes, but higher incomes may also lead to the development of better institutions. To help overcome this simultaneity, AJR use mor-tality rates for early European settlers as an instrument for institution quality. The validity of this instrument hinges on the argument that settlers set up better institutions in places
 


The Notebooks 13.5.2 implement the AJR example.
 


where they were more likely to establish long-term settlements, that where they were likely to settle for the long term is related to settler mortality at the time of initial colonization, and that institutions are highly persistent. The exclusion restriction for the instrumental variable is then motivated by the argument that GDP, while persistent, is unlikely to be strongly influenced by mortality in the previous century, or earlier, except through institutions.
In their paper, AJR note that their instrumental variable strategy will be invalidated if other factors are also highly persistent and related to the development of institutions within a country and to the country’s GDP. A leading candidate for such a factor, as they discuss, is geography. AJR address this by assuming that the confounding effect of geography is adequately captured by a linear term in distance from the equator and a set of continent dummy variables. Using DML allows us to relax this assumption and replace it by a weaker assumption that geography can be sufficiently controlled by an unknown function of distance from the equator and continent dummies which can be learned by ML methods.
We present the verbal identification argument above in the form of a DAG in Figure 13.1. In the DAG, 𝑌 is wealth, 𝑂 the quality of early institutions, 𝐷 the quality of modern institutions,
𝑋 observed measures of geography, 𝑍 early settler mortality,
𝐴 the present day latent factors jointly determining modern institutions and wealth, and 𝐿 early latent factors affecting early settler mortality. Applying the IV method here requires the identification of the causal effect of 𝑍  𝐷 and 𝑍  𝑌. From the DAG, we see that 𝑋 blocks the backdoor paths from 𝑌 to 𝑍 and from 𝐷  𝑍. This means that the instrument satisfies the required exogeneity condition conditional on 𝑋.






 


We think the story sounds plausible, but it is always impor-tant to consider threats to identification. The direct threat to identification would be if 𝐿 directly affected 𝑍 and either 𝑂, 𝐷, or 𝑌, or, in words, if early latent factors directly affected early settler mortality and either present-day quality of institutions or present day wealth. In such cases we would need to include
 
Figure 13.1: DAG for the Effect of Quality of Institutions on Wealth.
 


Table 13.1: DML Estimates of the Effect of Institutions on Output









 
𝐿 as additional controls. 𝐿 could represent many different latent factors. For example, one might conjecture that the religion of early European settlers (e.g., Catholic vs Protestant) is related to the type of institutions they would establish and to their mortality rates upon colonization. In their original study, AJR did examine this threat by checking robustness of their result with respect to the inclusion of religion variables. They also ex-amined the use of other additional controls to assess robustness to other potential sources of confounding.1
We report results from applying DML following the procedure outlined in Section 9.4 in Table 13.1. For cross-fitting, we use 5 folds. Here we just tried two methods, Lasso with plug-in tuning and Random Forests with package defaults, for learning the nuisance functions 𝜂. As predictors in the Lasso estimates, we used a dictionary formed by taking latitude and latitude2 interacted with continent dummies as technical controls. For the Random Forest estimates, we simply include latitude and continent dummies as raw controls. The Random Forest predicts outcomes 𝑌, 𝐷, and 𝑍 better than Lasso. The resulting best DML estimate is therefore based on DML with Random Forest used in all ML steps.
In this example, we see uniformly large and positive point esti-mates across all procedures considered, and estimated effects are statistically significant at the 5% level in all cases. We note the estimates are somewhat smaller than the baseline estimates reported in AJR – an estimated coefficient of 1.10 with estimated standard error of 0.46 ([3], Table 4, Panel A, column 7) – but are qualitatively similar, indicating a strong and positive effect of institutions on output.
 










1: It is good to revisit their analy-sis using ML tools. See their Data archive to get started.
 

13.2	DML Inference in the Interactive IV Regression Model (IRM)
DML Inference on LATE
In this section, we consider estimation of local average treatment effects (LATE) with a binary treatment variable, 𝐷   0, 1 , and a binary instrument, 𝑍  0, 1 . As before, 𝑌 denotes the outcome variable, and 𝑋 is the vector of covariates. Consider the following statistical parameter:
𝜃0 =  E[E[𝑌 | 𝑍 = 1, 𝑋] − E[𝑌 | 𝑍 = 1, 𝑋]] .
E[E[𝐷 | 𝑍 = 1, 𝑋] − E[𝐷 | 𝑍 = 0, 𝑋]]
This parameter is the ratio of the average predictive effects of 𝑍 on 𝑌 and of 𝐷 on 𝑌. Under the assumptions laid out in Chapter 12, this statistical parameter is a causal parameter – the average treatment effect for compliers (LATE).
To set up estimation, define the regression functions:
𝜇0(𝑍, 𝑋) = E[𝑌 | 𝑍, 𝑋]
𝑚0(𝑍, 𝑋) = E[𝐷 | 𝑍, 𝑋]
𝑝0(𝑋) = E[𝑍 | 𝑋].
Define the nuisance parameter 𝜂 = 𝜇, 𝑚, 𝑝 to denote square-integrable functions 𝜇, 𝑚, and 𝑝, with 𝜇 mapping the support
of 𝑍, 𝑋 to ℝ and 𝑚 and 𝑝 respectively mapping the support of
𝑍, 𝑋 and 𝑋 to 𝜀, 1  𝜀 for some 𝜀  0, 1 2 . The true value of the nuisance parameter is 𝜂0 = 𝜇0, 𝑚0, 𝑝0 , the regression functions defined above.
The DML estimator of 𝜃0 employs the orthogonal score
𝜓(𝑊; 𝜃, 𝜂) := 𝜇(1, 𝑋) − 𝜇(0, 𝑋) + 𝐻(𝑝)(𝑌 − 𝜇(𝑍, 𝑋))
− (𝑚(1, 𝑋) − 𝑚(0, 𝑋) + 𝐻(𝑝)(𝐷 − 𝑚(𝑍, 𝑋)) 𝜃,
for 𝑊 = (𝑌, 𝐷, 𝑋, 𝑍) and
𝐻(𝑝) :=   𝑍   −  (1 − 𝑍) .
𝑝(𝑋)	1 − 𝑝(𝑋)
It is easy to verify that this score satisfies the moment condi-tion
E[𝜓(𝑊; 𝜃0, 𝜂0)] = 0
 


and also the Neyman orthogonality condition
𝜕𝜂E[𝜓(𝑊; 𝜃0, 𝜂0)] = 0
at the true value 𝜂0 = 𝜇0, 𝑚0, 𝑝0 of the nuisance parameter.	Verifying both claims is a good ex-
ercise.
Therefore we can apply the generic ML algorithm to this prob-lem, including the selection of the best ML methods to estimate the nuisance parameters.


Variance estimation and confidence intervals are constructed as in the generic DML algorithm.

 
The Effect of 401(k) Participation on Net Financial Assets
Here we continue to re-analyze the effects of 401(k)’s on house-hold financial assets, picking up from Section 9.3. In this section, we report the LATE where we take the endogenous treatment variable to be participating in a 401(k) plan using 401(k) eligibility
 




The Notebooks 13.5.3 implement DML estimation of the LATE of 401(k) participation.
 


Table 13.2: Estimated Effect of 401(k) Participation on Net Finan-cial Assets













as instrument. Even after controlling for features related to job choice, it seems likely that the actual choice of whether to participate in an offered plan would be endogenous. Of course, we can use eligibility for a 401(k) plan as an instrument for participation in a 401(k) plan under the conditions that were used to justify the exogeneity of eligibility for a 401(k) plan outlined in Section 9.3.
We report DML results of estimating the LATE of 401(k) partic-ipation using 401(k) eligibility as an instrument in Table 13.2. We employ the procedure outlined in Section 13.2 using the same ML estimators to estimate the quantities used to form the orthogonal estimating equation as we employed to estimate the ATE of 401(k) eligibility in Section 9.3. Looking at the results, we see that the Lasso does a very poor job predicting the outcome relative to the other considered learners and returns a negative and very imprecise coefficient estimate. The remaining learners all have similar predictive performance for each of 𝑌, 𝐷, and 𝑍 and return similar estimates and standard errors. It is reassuring that the results obtained from the different flexible methods with similar predictive performance are broadly consistent with each other.
It is also interesting that the results based on flexible ML meth-ods are broadly consistent with, though somewhat attenuated relative to, those obtained by applying the same specification for controls as used in [4] and [5] and using a linear IV model which returns an estimated effect of participation of $13,102 with estimated standard error of (1922). The attenuation may suggest that the simple intuitive control specification used in the original baseline specification is not sufficiently flexible.
 

13.3	DML Inference with Weak Instruments
Motivation
As a simple motivating example, consider an instrumental variables model with a one-dimensional endogenous variable
𝐷 when there are either no controls or we are able to partial them out perfectly. In this case, the IV estimator takes the form
𝜃ˆ = 𝔼𝑛[𝑍˜ 𝑌˜ ]/𝔼𝑛[𝑍˜ 𝐷˜ ],
and we have that
√𝑛(𝜃ˆ − 𝜃) = √𝑛𝔼𝑛[𝑍˜ 𝜖]/𝔼𝑛[𝑍˜ 𝐷˜ ].
When 𝔼𝑛 𝑍˜ 𝐷˜ is well-separated away from zero, we invoke the approximation
 
√𝑛𝔼𝑛[𝑍˜ 𝜖]/𝔼𝑛[𝑍˜ 𝐷˜ ] a
 
𝑁(0, E[𝑍˜ 2 𝜖2])/E[𝑍˜ 𝐷˜ ].	(13.3.1)
 
However, this approximation is not reliable when instruments are "weak" – when 𝔼𝑛 𝑍˜ 𝐷˜ appears close to zero. Intuitively, we may worry that small changes in a sample that result in
relatively small changes in 𝔼𝑛[𝑍˜ 𝐷˜ ] may still have large impacts on the estimator 𝜃ˆ when 𝔼𝑛 𝑍˜ 𝐷˜ is near zero because 𝔼𝑛 𝑍˜ 𝐷˜
shows up in the denominator. That is, (13.3.1), which essentially ignores sampling variation in 𝔼𝑛 𝑍˜ 𝐷˜ , may provide a very poor approximation to the actual finite sample sampling behavior of
the IV estimator in this case.
We illustrate the potential poor performance of the usual asymp-totic approximation (13.3.1) in Figure 13.2 which reports results from a simulation experiment in which E 𝑍˜ 𝐷˜ is close to zero.
Here we see the sampling distribution (given by the blue curve) of the IV estimator deviates strongly from the normal approx-
imation (given by the red curve). Note that by varying how close E 𝑍˜ 𝐷˜ is to zero, one can make the differences more or less pronounced.
In principle, we can detect the weak instrument problem by testing whether 𝛽 = 0 in the projection equation
 


"Weak identification" (or "weak in-struments" in IV models) refers to settings in which we cannot confi-dently conclude a testable identify-ing assumption holds in our data. In our simple IV model, the pa-rameter 𝜃 is not identified when
E 𝑍˜ 𝐷˜ = 0 as solving the popu-
lation moment condition requires solving E[𝑍˜ 𝐷˜ ]𝜃 = E[𝑍˜ 𝑌˜ ].
 
𝐷˜
 
= 𝛽𝑍˜ + 𝑈,	E[𝑍˜ 𝐷˜ ].
 
The usual asymptotic approximation relies on E 𝑍˜ 𝐷˜ being so far from zero that we can ignore finite sample variability in the
 






















Figure 13.2: Actual sampling distri-bution of the IV estimator (blue) vs the normal approximation of the IV Estimator (red) in a simulation ex-periment using a weak instrument.

 
empirical analog of this expectation. Note that this statement essentially says that we need to be sure that 𝛽 ≠ 0 before we use the usual asymptotic approximation.
There are a variety of "rules of thumb" for when to conclude instruments are strong in the literature. Staiger and Stock (1997)
[6] suggested the most common rule of thumb used in practice. In the one endogenous variable, one instrument case, this rule of thumb corresponds to using the usual asymptotic approxima-tion when the first stage t-statistic for testing the null hypothesis
that 𝛽 = 0, 𝛽ˆ  𝛽 se 𝛽ˆ , is bigger than √10  3.16. This rule of
thumb can unfortunately be very optimistic in that confidence
intervals based on the usual asymptotic approximation may have coverage far from the stated coverage level – e.g. a 95% confidence interval may cover the true parameter value in far fewer than 95% of repeated samples – even when this condition is satisfied. Hansen, Hausman, and Newey [7] suggest a differ-ent rule of thumb which reduces to using the usual asymptotic approximation when the first stage t-statistic for testing the null
hypothesis that 𝛽 = 0 is greater than 5.6 in the one endogenous variable one instrument case. More recent work, e.g. Andrews (2018) [8], suggests that one should be cautious in applying any
 




These are rules of thumb as they are based on simulations rather than formal justification.
 
such rule of thumb.	A related caution is that basing in-
ference on the usual asymptotic ap-
 
All of these results suggest that the usual asymptotic approxi-
 
proximation after seeing the result of a test for the strength of associ-
ation between 𝐷˜ and 𝑍˜ can intro-
duce substantial pre-test bias that further distorts inference. See An-drews, Stock, and Sun (2019) [9].
 


mation may not be safe if we are worried that our instruments are not strongly related to the endogenous variables. If we have such worries, is there anything else we can do?
Of course there is. There are a variety of alternative inferen-tial procedures whose behavior does not hinge on the well-separation of 𝔼𝑛 𝑍˜ 𝐷˜ from zero. Here, we consider one specific
approach based upon the statistic	The statistic 𝐶 𝜃 is Neyman’s 𝐶 𝛼
statistic [10]. In the case we con-
 
|𝔼𝑛[(𝑌˜ − 𝜃𝐷˜ )𝑍˜ ]|2
 
sider here, the statistic is essentially
 
𝐶(𝜃) = 𝕍
 
.
[(𝑌˜ − 𝜃𝐷˜ )𝑍˜ ]/𝑛
 
the same as considered in Ander-son and Rubin (1949) [11] without
 
If 𝜃0 = 𝜃, then 𝐶(𝜃	a 𝑁(0, 1)2 = 𝜒2(1). Therefore, we can
 
imposing homoskedasticity as in
Stock and Wright (2000) [12].
 
reject the hypothesis 𝜃0 = 𝜃 at level a (for example a = .05 for a 5% level test) if 𝐶 𝜃 > 𝑐 1 a where 𝑐 1 a is the 1 a - quantile of a 𝜒2 1 variable. The probability of falsely rejecting
the true hypothesis is approximately a 100%. To construct a 1 a 100% confidence region for 𝜃, we can then simply invert the test by collecting all parameter values that are not rejected at the a level:
𝐶𝑅(𝜃) = {𝜃 ∈ Θ : 𝐶(𝜃) ≤ 𝑐(1 − a)}.

In more complex settings with many controls or controls that enter with unknown functional form, we can simply replace the residuals 𝑌˜ , 𝐷˜ , and 𝑍˜ by machine learned cross-fitted residuals
𝑌ˇ , 𝐷ˇ , and 𝑍ˇ . Thanks to the orthogonality of the IV moment
condition underlying the formulation outlined above, we can formally assert that the properties of 𝐶 𝜃 and the subsequent testing procedure and confidence region for 𝜃 continue to hold when using cross-fitted residuals. We will further be able to apply the general procedure to cases where 𝐷 is a vector, with a suitable adjustment of the statistic 𝐶(𝜃).

DML Inference Robust to Weak-IV in PLMs
Here, we present a more general version of weak identification robust inference, including implementation and theoretical details, in settings where we want to use machine learning to aid in controlling for confounding variables 𝑋.

DML Weak-IV-Robust Inference for PLIV Model
1. Initialize: Let Θ be a known parameter space that contains the true value 𝜃0. Using the DML-PLIV
 


algorithm, produce the cross-fitted residuals: 𝑌ˇ𝑖 , 𝐷ˇ 𝑖 , and 𝑍ˇ 𝑖 . Using the cross-fitted residuals and for 𝜃 ∈ Θ, compute the moment function
Mˇ (𝜃) := 𝔼𝑛[(𝑌ˇ𝑖 − 𝜃′𝐷ˇ 𝑖)𝑍ˇ 𝑖],
the empirical covariance function
Ωˇ (𝜃) := 𝕍𝑛[(𝑌ˇ − 𝜃′𝐷ˇ )𝑍ˇ ],
and the score statistic
𝐶(𝜃) := 𝑛Mˇ (𝜃)′Ωˇ −1(𝜃)Mˇ (𝜃).
2. Robust Confidence Region: Construct the approxi-mate (1 − a) × 100% confidence region as
𝐶𝑅(𝜃0) = {𝜃 ∈ Θ : 𝐶(𝜃) ≤ 𝑐(1 − a)},
where 𝑐(1 − a) := (1 − a)-quantile of a 𝜒2(𝑚) variable, where 𝑚 = dim(𝑍𝑖).

In order to state the next result, define the oracle version of the moment and covariance functions given in Step 1 of the DML Weak-IV-Robust Inference algorithm,
Mˆ (𝜃) = 𝔼𝑛[(𝑌˜ − 𝜃′𝐷˜ )𝑍˜ ]
and
Ωˆ (𝜃) = 𝕍𝑛[(𝑌˜ − 𝜃′𝐷˜ )𝑍˜ ],
which are defined in terms of the true residuals 𝑌˜𝑖 , 𝐷˜ 𝑖 , and
𝑍˜ 𝑖 .

 


 
The Effect of Institutions on Economic Growth Revisited
We illustrate the use of DML weak identification robust inference by revisiting the AJR example from Section 13.1. Recall that Random Forests performed best in all auxiliary predictive steps in our original exercise in this example, so we only consider the use of Random Forests to form residuals in this section.
After partialling out controls using Random Forests, we run the
regression of 𝐷ˇ on 𝑍ˇ to assess the strength of the instruments.
The resulting t-statistic is approximately 2, much lower than any rule-of-thumb "safety" threshold that appears in the literature. As such, we conclude that we have a weak instrument and proceed with weak identification robust inference.
















 







We implement the robust inferential approach from the previous subsection considering Θ = 2, 2 as our parameter space for the causal effect of institutions on wealth. We note that, because
the outcome we consider is the logarithm of GDP per capita, the range [-2,2] includes extremely (likely implausibly) large
 
Figure 13.3: Construction of weak IV robust confidence regions for the effect of institutions on output us-ing DML. Values of the 𝐶 𝜃 statis-tic are shown on the vertical axis; values of 𝜃 tested on the horizontal axis. The 90% confidence region is given by the red vertical bars.
 


negative and positive effects, so restricting attention to this range a priori seems reasonable. We illustrate the procedure in Figure 13.3 which plots the value of the test statistic 𝐶(𝜃) for
𝜃 ∈ [−2, 2].
The resulting 95% confidence region is
[.28, 2].
We can compare this region to the confidence region produced by the usual Gaussian asymptotic approximation which is not robust to weak identification:
[.86 ± 2 · 0.33] = [.20, 1.52].
Both the usual and robust confidence regions are consistent with relatively large positive effects of institutions on wealth. However, it is interesting that the lower end of the robust confidence region is larger than the lower end of the usual region and that this difference is economically meaningful. That is, we could not rule out that a one unit increase in quality of institutions causes an approximately a 20% increase in GDP per capita looking at the usual interval, while we could rule out all effect sizes smaller than 28% with the robust interval. The difference between a 20% and 28% increase in GDP per capita is small but certainly economically relevant. Given that the instruments are weak, we should, of course, rely on the robust confidence interval.

13.4	Generic DML Inference under Weak Identification
We now present a generally applicable formulation of weak identification robust inference. This formulation covers the problem of weak instruments in the context of LATE estimation as well as other problems where Neyman orthogonal scores are available.
The initialization and first two steps to our approach to weak identification robust inference are the same as in the Generic DML Algorithm. We then use these estimates of the nuisance parameters in conjunction with the score function at a fixed value of 𝜃 to construct a score test statistic analogous to 𝐶 𝜃 from the previous section which can be used to test the hypoth-esis that 𝜃0 = 𝜃 and to form confidence regions. We collect this
procedure in the following algorithm:
 


Generic DML Robust to Weak Identification
1.	Initialize: Provide the data frame (𝑊𝑖)𝑛  , the Ney-
man orthogonal score/moment function 𝜓 𝑊 , 𝜃, 𝜂 and the name and model for ML estimation method(s) for learning nuisance parameters 𝜂. Specify Θ to be a
known parameter space that contains the true value
𝜃0. We then take a K-fold random partition I𝑘 𝐾 of observation indices 1, ..., 𝑛 such that the size of each fold is about the same. For each 𝑘	1, . . . , 𝐾 , we construct a machine learning estimator 𝜂 𝑘 using data 𝑊𝑖 𝑖∉I𝑘 , that is, all the data except the data from
the 𝑘th fold.
2.	Estimate Moments and Their Variance: Letting
𝑘(𝑖) = {𝑘 : 𝑖 ∈ 𝐼𝑘 }, construct the moment function
1  𝑛
Mˇ (𝜃) = 𝑛 I: 𝜓(𝑊𝑖; 𝜃, 𝜂ˆ[𝑘(𝑖)])
covariance function,


1  𝑛
Ωˇ (𝜃) = 𝑛 I:[𝜓(𝑊𝑖; 𝜃, 𝜂ˆ[𝑘(𝑖)])𝜓(𝑊𝑖; 𝜃, 𝜂ˆ[𝑘(𝑖)])′]
1  𝑛	1  𝑛


 
𝑖=1
and score statistic
 
𝑖=1
 
𝐶(𝜃) = 𝑛Mˇ (𝜃)′Ωˇ −1(𝜃)Mˇ (𝜃).
3.	Confidence Region: Construct the approximate (1 −
a)	× 100% confidence region as
𝐶𝑅(𝜃0) = {𝜃 ∈ Θ : 𝐶(𝜃) ≤ 𝑐(1 − a)}
where 𝑐(1 − a) is the (1 − a)−quantile of a 𝜒2(𝑚)
variable where 𝑚 = dim(Mˇ (𝜃)).

Note that this confidence region simply collects all values 𝜃 Θ that are not rejected by testing 𝜃0 = 𝜃 using test statistic 𝐶 𝜃 at the a-level.
As in the previous section, we define oracle versions of the moment and covariance functions from the preceding algorithm
 


for use in stating formal results:
Mˆ (𝜃) = 𝔼𝑛[𝜓(𝑊; 𝜃, 𝜂0)],
Ωˆ (𝜃) = 𝕍𝑛[𝜓(𝑊; 𝜃, 𝜂0)].


13.5	Notebooks
Notebook 13.5.1 (Weak IV) R Notebook on Weak IV and Python Notebook on Weak IV provide a simulation exper-iment illustrating the weak instrument problem with IV estimators.

Notebook 13.5.2 (DML for Partially Linear IV) DML for Partially Linear IV R Notebook and DML for Partially Linear IV Python Notebook carry out the DML IV analysis of the Acemoglu-Johnson-Robinson example, which considers the impact of the quality of institutions on economic growth, instrumenting quality of institutions with settler mortality. The notebook explores the partially linear IV model and tests for the presence of weak instruments.

Notebook 13.5.3 (DML for LATE Models) DML for LATE Models R Notebook and DML for LATE Models Python Notebook estimate the Local Average Treatment Effects of 401(K) participation on net financial wealth.
 

13.6	Exercises
Exercise 13.6.1 (Weak IV) Experiment with Notebook 13.5.1, varying the strength of the instrument. How strong should the instrument be in order for the conventional normal approx-imation based on strong identification to provide accurate inference? Based on your experiments, provide a brief expla-nation of the weak IV problem to a friend.

Exercise 13.6.2 (DML for Partially Linear IV) Experiment with Notebook 13.5.2. Try to extend the analysis by includ-ing other control variables (e.g. religion, other measures of geography, or measures of natural resources) or consider another empirical application to another IV example. (See some potential applications at the the Angrist data archive). In the case of a new application, don’t forget to draw your DAGs!

Exercise 13.6.3 (DML for LATE Models) Experiment with the Notebook 13.5.3. Apply the analysis to another dataset. For example, try the JTPA data from Joshua Angrist’s data archive. Don’t forget to draw your DAGs!

Exercise 13.6.4 ((Theoretical) Neyman Orthogonality of the Partially Linear IV Methods) Verify that the scores for the partially linear IV methods are Neyman orthogonal.
 


Bibliography



[1]		Guido W. Imbens. ‘Better LATE Than Nothing: Some Comments on Deaton (2009) and Heckman and Urzua (2009)’. In: Journal of Economic Literature 48.2 (2010),
pp. 399–423. doi: 10 . 1257 / jel . 48 . 2 . 399 (cited on page 359).
[2]		Victor Chernozhukov, Denis Chetverikov, Mert Demirer, Esther Duflo, Christian Hansen, Whitney Newey, and James Robins. ‘Double/debiased machine learning for treatment and structural parameters’. In: Econometrics Journal 21.1 (2018), pp. C1–C68 (cited on pages 361, 366).
[3]		Daron Acemoglu, Simon Johnson, and James A. Robinson. ‘The colonial origins of comparative development: An empirical investigation’. In: American Economic Review
91.5 (2001), pp. 1369–1401 (cited on pages 362, 364).
[4]		James M. Poterba, Steven F. Venti, and David A. Wise. ‘401(k) Plans and Tax-Deferred savings’. In: Studies in the Economics of Aging. Ed. by D. A. Wise. Chicago, IL: University of Chicago Press, 1994, pp. 105–142 (cited on page 367).
[5]	James M. Poterba, Steven F. Venti, and David A. Wise. ‘Do 401(k) Contributions Crowd Out Other Personal Saving?’ In: Journal of Public Economics 58.1 (1995), pp. 1–32 (cited on page 367).
[6]	D. Staiger and J. H. Stock. ‘Instrumental Variables Regres-sion with Weak Instruments’. In: Econometrica 65 (1997),
pp. 557–586 (cited on page 369).
[7]		Christian Hansen, Jerry Hausman, and Whitney K. Newey. ‘Estimation with Many Instrumental Variables’. In: Journal of Business and Economic Statistics 26 (4 2008), pp. 398–422 (cited on page 369).
[8]	Isaiah Andrews. ‘Valid Two-Step Identification-Robust Confidence Sets for GMM’. In: The Review of Economics and Statistics 100.2 (May 2018), pp. 337–348. doi: 10.1162/ REST_a_00682 (cited on page 369).
[9]		Isaiah Andrews, James Stock, and Liyang Sun. In: An-nual Review of Economics 11 (2019), pp. 727–753 (cited on page 369).
 

 
Bibliography
 
378
 


[10]	Jerzy Neyman. ‘Optimal asymptotic tests of composite hypotheses’. In: Probability and Statistics (1959), pp. 213–234 (cited on page 370).
[11]	T. W. Anderson and H. Rubin. ‘Estimation of the Parame-ters of Single Equation in a Complete System of Stochastic Equations’. In: Annals of Mathematical Statistics 20 (1949),
pp. 46–63 (cited on page 370).
[12]		James H. Stock and Jonathan H. Wright. ‘GMM with Weak Identification’. In: Econometrica 68 (2000), pp. 1055–
1096 (cited on page 370).
 

 
Statistical Inference on Heterogeneous Treatment
Effects

"Never cross a river that is on average four feet deep."
– Nassim Nicholas Taleb [1].










In this chapter, we begin our treatment of estimation and infer-ence targeting heterogeneous treatment effects. We provide a motivation for why heterogeneous effects, captured by condi-tional average treatment effects, are of interest in many settings and highlight why inference for heterogeneous effects in high-dimensional settings is challenging. We then consider simple summaries of heterogeneous effects characterized by best linear approximations based on pre-specified variables. Estimation and inference for these summaries can readily be performed by simple adaptation of DML methods. We then consider flexible inference on conditional average effects based on adaptations of Random Forest methods known as Causal Forests. These methods can deliver flexible methods and valid confidence statements for conditional average treatment effects based on relatively low-dimensional conditioning variables.
 
14
14.1	CATEs under Conditional Exogeneity	380
14.2	Inference on Best Linear Ap-proximations	383
Least Squares Methods for Learning CATEs	384
Application to 401(k) Exam-ple	386
14.3	Non-Parametric Inference for CATEs with Causal Forests	388
Empirical Example: The "Welfare" Experiment   395
14.4	Notebooks	397
 
14.1 CATEs under Conditional Exogeneity
 

We consider the standard setup for analyzing the effect of a binary treatment in the presence of a high-dimensional set of controls 𝑍. Specifically, we have potential outcomes 𝑌 0 and 𝑌 1 and assigned treatment 𝐷 that obey the conditional exogeneity condition:
𝐷 ⊥ 𝑌(𝑑) | 𝑍.
We observe the outcome 𝑌 := 𝑌 𝐷 , the treatment assignment
𝐷, and the high-dimensional set of controls 𝑍.
Our main interest in this section is a Conditional Average Treatment Effect (CATE) defined as
𝜏0(𝑋) = E[𝑌(1) − 𝑌(0) | 𝑋],
where 𝑋 is (typically) a low-dimensional subset of covariates 𝑍. We have already seen that the conditional average treatment effect is identified by the conditional average predictive effect under conditional exogeneity; see Theorem 5.2.1. Using this result, we obtain the simple identifying equation
𝜏0(𝑋) = E[E[𝑌 | 𝐷 = 1, 𝑍] − E[𝑌 | 𝐷 = 0, 𝑍] | 𝑋].	(14.1.1)

The value of CATE estimation We have thus far largely fo-cused on simple unconditional average causal effects. However, unconditional average treatment effects are not informative about whom to treat. At best they can inform uniform policies, where we decide whether or not to roll out a treatment on the whole population. Such uniform policies can have two major drawbacks. Even if the average effect is significantly positive, there could potentially exist sub-groups in the population for which the treatment has severe adverse effects. Analogously, if the average effect is significantly negative or a statistical null, then we might choose not to deploy a new policy or treatment. However, there could exist responder sub-groups in the popu-lation for which the new treatment has a significant positive impact. In both cases, by focusing on overall average causal ef-fects, we are potentially harming sub-groups of the population, either by depriving them of a beneficial treatment or forcing them into a treatment that makes them worse off.
Conditional average treatment effects allow us to identify het-erogeneities of the effect and discover in a data-driven manner sub-groups of the population for which the treatment can be
 



We focus on the binary treatment case, but note that the approach readily extends to more general set-tings.
 


harmful or beneficial. Good estimates of the CATE allow us to deploy personalized policies, where personalization refers to offering treatment based on each unit’s observable character-istics. The potential of improving policy targeting by offering treatment to those for whom the treatment is effective has led to widespread interest in CATE estimation. Estimation of CATE and policy targeting is especially prevalent in settings with rich datasets and many informative covariates. Perhaps the frontrun-ning application domain of CATE estimation occurs in digital experimentation, a setting in which rich data is readily available and personalization of treatment (e.g. targeted marketing) is easily implementable.

The difficulty of CATE estimation From a statistical view-point, estimation and inference on the CATE is inherently harder than estimation and inference on simple average treatment ef-fects and related low-dimensional summaries. So far, most of the policy relevant target parameters that we have been inter-ested in, take the form of some low-dimensional vector valued parameter. This is the first time, where our target causal param-eter of interest is actually a function or the value of a function at a particular point. The closest estimation problem to the CATE is that of estimating a Best Prediction rule or a Conditional Expectation Function. Note that even if we had access to both counterfactuals 𝑌 1 , 𝑌 0 , then estimation of the CATE is as hard as estimating a regression function corresponding to the outcome 𝑌 1  𝑌 0 . For such problems, thus far we were con-tent at estimating them with respect to the mean-squared-error metric, and at a reasonable rate that decays to zero, potentially slower than the parametric rate of 𝑛−1/2. On the contrary for most causal effects of interest, we were not really content with simply a mean-squared error rate; we typically sought the abil-ity to construct confidence intervals and were striving for very accurate estimation, most of the times at parametric rates.
For this reason, when it comes to CATE estimation, we will need to re-calibrate our expectations and potentially relax our goals. In this and the next chapter, we will consider four such avenues:
▶ Target the estimation of the best linear approximation (BLA) of the CATE function, with a set of predefined low-dimensional engineered features. In this case, we can essentially recover all the desiderata of target causal quantities: estimation at parametric rates, confidence intervals for the BLA at a particular point and even
 


simultaneous confidence bands for the BLA at a set of target evaluation points.
▶ Target inference on other summarizations of CATE such as its tail expectations, the value of a covariate-based treatment policy, and the value of the optimal such pol-icy. Again, we recover the desiderata of target causal quantities.
▶ Construct non-parametric confidence intervals for CATE predictions at a particular point, using novel methods (such as Causal Forests), which are practically powerful and marry machine learning techniques with uncertainty quantification, but which are theoretically valid only when 𝑋 is low-dimensional, and which in practice can be more brittle and are not as assumption-lean as inference based on OLS.
▶ Drop our desire to produce confidence intervals on the CATE function and only require good accuracy of the learned CATE function as captured by the mean-squared-error metric. In this case, we will be essentially treating the CATE problem as a best prediction problem and we will need to develop analogous methods for model selection, ensembling and out-of-sample evaluation. To compen-sate for the lack of confidence intervals for the CATE predictions, we will develop hypothesis tests that can be performed out-of-sample, that act as validation metrics that measure the quality of the CATE model as whole, as summarized in particular dimensions. For instance, we can test out of sample, whether the model picked up any statistically significant signal of heterogeneity, or if we use the model to prioritize treatment among the population, then will it lead to statistically significant policy gains.
▶ Drop the emphasis on learning the effect heterogeneity and focus only on the value of personalized policies that come out of our estimation process. In this case, we view CATE only as a means to our goal of designing personalized policies and in that respect we might want to measure the quality of our process, solely based on the personalized policy gains over some baseline, and not on the accuracy of the magnitude of the effect. Note that to learn a good policy, we are primarily interested in learning the sign of the effect and not necessarily its magnitude and appropriately partitioning the population such that the sign of the effect is relatively homogeneous within each sub-group. From this perspective, learning a good policy is more akin to a classification problem (classifying for which parts of the population the effect is
 


positive/negative) as opposed to a regression problem and we will investigate such a formal equivalence.

14.2		Inference on Best Linear Approximations
Our main goal is summarizing the potentially complex and high-dimensional treatment effect function, which may depend on the entire vector 𝑍, in terms of a lower-dimensional object 𝑋. We may be interested in such summaries for aiding interpretation or for policy reasons where we are interested in effects among particular recipients defined by observable characteristics.
For example, in the context of the 401(k) analysis from previous chapters, we have that 𝑌 is a household’s total net financial assets, 𝐷 is 401(k) eligibility status, and 𝑍 is the entire set of household characteristics. We might then take 𝑋 to be income in which case the CATE 𝜏0 𝑋 shows the expected effect of 401(k) eligibility on total financial assets for a subject whose income level is 𝑋.
The key to adaptively estimating and potentially performing in-ference for the CATE is expressing it as a conditional expectation of an unbiased signal:
𝜏0(𝑋) = E[𝑌(𝜂0) | 𝑋],
where the signal takes the form
𝑌(𝜂) = 𝐻(𝜇) (𝑌 − 𝑔(𝐷, 𝑍)) + 𝑔(1, 𝑍) − 𝑔(0, 𝑍),
with nuisance parameters 𝜂 := (𝜇, 𝑔) and
𝐻(𝜇) :=  𝐷  −  1 − 𝐷 .
𝜇(𝑍)	1 − 𝜇(𝑍)
Here, 𝑔 𝐷, 𝑍 and 𝜇 𝑍 are square integrable functions with
𝜇 𝑍 taking on values in 𝜖, 1  𝜖 for some 𝜖 > 0. The true values of these nuisance parameters are 𝜂0 := 𝜇0, 𝑔0 defined as
𝜇0(𝑍) := P(𝐷 = 1 | 𝑍),	𝑔0(𝐷, 𝑍) := E[𝑌 | 𝑍, 𝐷].
Importantly, the signal has the Neyman orthogonality prop-erty:
𝜕𝜂E[𝑌(𝜂0) | 𝑋] = 0.
 


Making use of the representation of the CATE as the conditional expectation of 𝑌 𝜂0 , we then estimate the CATE using the following steps:

Generic DML for CATE
1.	Partition data indices into 𝑘 folds of approxi-mately equal size: {1, ..., 𝑛} = ∪𝐾 𝐼𝑘. For each fold
𝑘=1
𝑘 = 1, ..., 𝐾, compute ML estimators 𝑔ˆ[𝑘](𝐷, 𝑍) and
𝜇ˆ[𝑘](𝑍) of the best predictors 𝑔0(𝐷, 𝑍) and 𝜇0(𝑍) leav-
ing out the 𝑘-th block of data. For any observation
𝑖 ∈ 𝐼𝑘, define
𝑌𝑖(-𝜂) = 𝑌𝑖(-𝜂𝑘)
= 𝐻𝑖(𝑌𝑖 − 𝑔ˆ[𝑘](𝐷𝑖 , 𝑍𝑖)) + 𝑔ˆ[𝑘](1, 𝑍𝑖) − 𝑔ˆ[𝑘](0, 𝑍𝑖)
where 𝐻𝑖 =   𝐷𝑖	 −   1 − 𝐷𝑖	 .
𝜇ˆ[𝑘](𝑍𝑖)	1 − 𝜇ˆ[𝑘](𝑍𝑖)
2.	Use low-dimensional or high-dimensional regression methods to regress 𝑌𝑖(𝜂ˆ) on covariate features 𝑋𝑖. If low-dimensional methods are used, inference on CATE can proceed using standard results for low-dimensional methods.

Under regularity conditions, the second step is adaptive, mean-ing all the learning guarantees and confidence intervals are approximately the same as if we knew the nuisance parame-ters 𝜂0. This adaptation holds true because of the conditional Neyman orthogonality of 𝑌 𝜂 . We note that this adaptivity does not imply that inferential objects, e.g. confidence intervals, can readily be obtained if high-dimensional methods are used in Step 2. We discuss implementation and inferential issues in more detail in the following sections.

Least Squares Methods for Learning CATEs
Here we focus on using least squares in the second step of the general approach given above.
Consider approximating or summarizing the function 𝑡 𝑥 by a linear combination of basis functions:
𝑝(𝑥)′𝛽0,
 


where 𝑝(𝑥) is 𝑑-dimensional dictionary with
𝑑 ≪ 𝑛.
For example, 𝑝 𝑥 could be a vector of group indicators or a vector of orthogonal polynomials or splines.
The parameter 𝛽0 is chosen to minimize the approximation error to the CATE:
min E(𝜏0(𝑋) − 𝑝(𝑋)′𝛽)2.

𝑝(𝑥)′𝛽0 is thus the best linear predictor for the CATE; that is,
𝛽0 = (E [𝑝(𝑋)𝑝(𝑋)])−1 E [𝑝(𝑋)𝑌(𝜂0)] .

An important, easily interpretable special case is when we choose to use group indicators in forming the basis functions
𝑝(𝑥). Specifically, we define group indicators as
𝐺𝑘(𝑋) = 1(𝑋 ∈ 𝑅𝑘),
where 𝑅′𝑘 𝑠 are mutually exclusive regions in the covariate space. For example, in the 401(k) example, we may be interested in average treatment effects for observations with household in-
come less than $10,000, observations with income between
$10,000 and $20,000, etc. which we could capture by defin-ing 𝐺1 𝑋	= 1 Income < $10, 000 , 𝐺2 𝑋	= 1 $10, 000 Income < $20, 000 , etc. With the group indicators defined,
we then set
𝑝(𝑋) = (𝐺1(𝑋), . . . , 𝐺𝐾(𝑋))′.
In this case, the Best Linear Predictor 𝛽0 recovers the GATEs (group average treatment effects).
More generally, 𝑝 𝑥  ℝ𝑑 represents a 𝑑-dimensional dic-tionary of series/sieve basis functions – e.g., polynomials or splines – and 𝑝 𝑥 ′𝛽0 corresponds to the best linear approxima-tion to the target function 𝜏0 𝑥 in the given dictionary. Under some smoothness conditions, 𝜋 𝑥 = 𝑝 𝑥 ′𝛽0 will approximate
𝜏0 𝑋 as the dimension of the dictionary becomes large, and
our inference will target this function.
Taking the approach motivated above to a sample of data, we have that the natural estimator of the best linear predictor of the CATE is
𝑝(𝑥)′-𝛽,
 


where 𝛽 is the ordinary least squares estimate of 𝛽 defined on
the ran-dom sample (𝑋𝑖 , 𝐷𝑖 , 𝑌𝑖)𝑁 :	0
 
-𝛽
 
𝑁 I:𝑖=1
 

𝑝(𝑋𝑖)𝑝(𝑋𝑖)′
 
−1  1
𝑁
 
I:𝑖=1
 

𝑝(𝑋𝑖)𝑌𝑖(𝜂).
 

Semenova et al. [2] derive a complete set of results for the
′
curve 𝑥 ↦→ 𝑝(𝑥)′𝛽0. Importantly, these results establish an
asymptotic approximation that allows simultaneous inference on all parameters of the best linear predictor curve. The key result verifies that the large sample properties of 𝛽 are the same
 
as those of

𝛽¯ :=
 

𝑁 I:𝑖=1
 


𝑝(𝑋𝑖)𝑝(𝑋𝑖)′
 

−1  1
𝑁
 


I:𝑖=1
 
-

𝑝(𝑋𝑖)𝑌𝑖(𝜂0),
 
when ML tools are used to estimate the nuisance parameter 𝜂0 so long as the ML tools perform sufficiently well. Thus, we can employ standard methods for inference about 𝛽0 and the best linear predictor curve functional 𝑥 ↦→ 𝑝(𝑥)′𝛽0.
Specifically, leveraging that 𝛽 and 𝛽 have the same large sample
properties, we have	-	¯

 

where
 
𝛽ˆ − 𝛽0 ∼𝑎 𝑁(0, Ωˆ /𝑁),
 
Ω := 𝑄−1 r𝔼𝑛 𝑝(𝑋𝑖)𝑝(𝑋𝑖)′(𝑌𝑖(𝜂) − 𝑝(𝑋𝑖)′𝛽)2 l 𝑄−1	(14.2.1)
for 𝑄 = 𝔼𝑛 𝑝(𝑋𝑖)𝑝(𝑋𝑖)′.
This result can be used to construct uniform confidence bands for
 
𝑥 ↦→ 𝑝(𝑥)′𝛽0,
which can be interpreted as confidence intervals for CATE
𝑥 ↦→ 𝜏0(𝑥) if the approximation error is small.

Application to 401(k) Example
We illustrate estimation of CATEs and GATEs by revisiting the 401(k) example. Here, we consider the effect of 401(k) eligibility on net total financial assets controlling for household character-istics. We consider heterogeneity of this effect as a function of
 








R Notebook for DML on CATE [3]: Analyzes summaries of the CATE of 401(K) conditional on income.
 





















Figure 14.1: Inference on ATE of 401(k) Eligibility by Income Group

income. We consider two different ways to summarize these het-erogeneous effects: GATEs based on coarse income categories and a summary of the CATE given income based on a collection of polynomial terms in log(Income).
We show estimates and confidence bands on GATEs by income groups in Figure 14.1. Here, groups correspond to income quintiles; e.g the first group has households with income smaller than the 20th percentile, the second group has households with income between the 20th and 40th percentile, and so on. Point estimates are provided by the central solid black bands. We represent pointwise confidence bands with the red lines in the interior of the box for each GATE. These bands would be appropriate for inference if one were interested ex ante in a single, pre-specified GATE. For example, one might be specifically interested in the eligibility effect among low income individuals and thus focus on the pointwise intervals over the first GATE. Finally, uniform confidence bands are given by the upper and lower bounds of the box for each GATE. These uniform bands provide a coverage guarantee for all five reported GATEs and would be appropriate for inference in settings where one was interested in all five effects and did not ex ante have a single specific GATE of interest.
We illustrate using a polynomial in log income to approximate the CATE in Figure 14.2. Point estimates are given by the central black line while the blue lines provide confidence bands. The narrower – dashed – confidence bands are pointwise and would be appropriate for a scenario in which one had a single,
 
















Figure 14.2: Inference on CATE of 401(k) Eligibility Conditional on Log-Income

pre-specified value of income of interest. The wider confidence bands are uniform, providing a coverage guarantee for the entire best linear predictor curve 𝑥  𝑝 𝑥 𝛽0. That is, any path for the entire curve that would not be rejected will lie entirely within the uniform confidence band. Finally, note that the coverage guarantee extends to the true CATE function 𝑥  𝜏0 𝑥 if the approximation error of the polynomial to the true CATE is small.

14.3		Non-Parametric Inference for CATEs with Causal Forests
An inherently harder task is performing inference on the value
𝜏0 𝑥 at a given point 𝑥. Statistically this problem can be even harder than performing inference on the value of a regression function at a particular point 𝑥. In fact, one solution casts the problem as such. Note that we already argued that:
𝜏0(𝑥) = E [𝑌(𝜂0) | 𝑋 = 𝑥]	(14.3.1) Thus one approach is to estimate the nuisance parameters 𝜂0
in a cross-fitting manner and then use any flexible regression method that supports prediction intervals and apply it to the re-gression problem 𝑌 𝜂 𝑋. In low dimensions, many classical approaches, such as kernel regression are applicable and can be invoked. In high-dimensions these methods will struggle to provide any meaningful insight.
An alternative is to use Random Forest based methods that will perform much better in practice. Standard Random Forest
 


approaches typically equally balance bias and variance and hence do not allow for confidence interval construction. Recent work of [4, 5] proposes adaptations of Random Forests that, in low-dimensions, provably produce asymptotically normal and un-biased predictions and provide theoretically justified construction of confidence intervals. The key ingredients in these adaptations are:
i	honesty: a separate sample is used to construct the struc-ture of the tree and a separate sample is used to calculate the estimates at the leaf nodes of the tree,
ii	balancedness: every split should leave at least a 𝜌  0.2
fraction of the samples on each side,
iii	random feature split: every feature should have a probability of at least 𝜋 𝑑 to be chosen on each split, where 𝑑 is the number of features (e.g. this can be achieved by choosing a random feature to split on with probability 𝜋),
iv	fully grown: the tree should be grown fully, such that the number of samples that fall in every leaf should be at most some small constant,
ii sub-sampling: unlike typical random forest methods that use bootstrap sub-samples to build each tree (i.e. of the same size as the original samples and drawn with replace-ment), these adapted forests use sub-samples without replacement and of a smaller size 𝑠  𝑛 than the original sample on each tree (the size of the sub-sample needs to be chosen carefully for the validity of the confidence
intervals and should be of the order of 𝑛 𝛼𝑑+1 , where
𝛼 =   log(1/𝜌)   ).
𝜋 log(1/(1−𝜌))
We will refer to any forest construction process that satisfies these properties as an Honest Random Forest.
We will refer to Honest Random Regression Forests that are trained on the doubly robust proxy labels 𝑌 𝜂 , with cross-fitted estimates of the nuisance functions, as Doubly Robust Forests. Based on the results in [5, 6], one can show that the validity of the confidence intervals of Honest Random Regression Forests is maintained even when the labels are biased due to the estimation error of the nuisance parameters 𝜂ˆ, so long as:

𝑛/𝑠E[(𝐻(𝜇ˆ − 𝐻(𝜇0)) (𝑔ˆ(𝐷, 𝑍) − 𝑔0(𝐷, 𝑍)) | 𝑋 = 𝑥] ≈ 0
Note that this requires accuracy of the nuisance estimates with respect to a confidional mean squared error. [6] shows how this can be achieved even in settings where 𝑍 is high-dimensional, albeit 𝑋 remains low-dimensional. In particular, one should expect the size of the confidence interval or the error of the
 


estimate to decay to zero at a rate of  𝑛− 2(𝑎𝑑+1) , where 𝑑 is the dimension of the covariates in 𝑋. This result is based on a conditional variant of the Neyman orthogonality property that is satisfied by the doubly robust proxy labels, in conjunction with the asymptotic normality of predictions that stem from Honest Random Forests.
Finally, under stronger assumptions and for binary covariates, the recent work of [7] also shows that confidence intervals of Honest Regression Random Forests trained with the squared loss criterion and without the balancedness or random feature split property, are asympotitcally valid even in high-dimensions, as long as the true regression function E 𝑌  𝑋 = 𝑥  is sparse
(i.e. only a small constant number of variables are relevant
for predicting 𝑌). However, an upper bound on the degree of sparsity (i.e. the number of relevant features) needs to be known. Extending this inference result to high-dimensional continuous features remains an active area of investigation. Similarly, extending this result to simultaneous confidence bands and not just pointwise confidence intervals is another active area of investigation.
While adaptive estimation of the CATE can be obtained fairly generally, it is important to note that 𝑋 should be low dimen-sional if we want to obtain confidence intervals or perform hypothesis tests. Genovese and Wasserman (Annals of Stats, 2008) [8] show that there do not exist adaptive confidence bands for estimation of the curve 𝜏0 𝑋 except under very restrictive assumptions more generally. They suggest instead to construct adaptive bands that cover a surrogate function 𝜋 which is close to, but simpler than, 𝜏0.
In the previous section, where we discuss the use of OLS with low-dimensional 𝑋, the surrogate 𝜋 represents either GATEs or the best linear approximation of the CATE. Inferential guarantees are also available for the case where 𝑋 is low-dimensional and Random Forests are used. Inferential results for low-dimensional surrogates 𝜋 based on other methods should also be possible, though we note that GATEs and best linear predictors more generally are readily interpretable and will likely be useful in many settings.
Despite these theoretical limitations, forest based approaches are empirically powerful as they tend to identify the most relevant factors that drive treatment effect heterogeneity, while at the same time providing some signal of uncertainty of the prediction. Even though this uncertainty quantification is more brittle than for instance the confidence intervals of an OLS
 


regression, as it depends on many more assumptions and holds only under particular choices of the hyperparameters of the method (which are typically violated in practice; especially when data-driven hyperparameter tuning is invoked via cross-validation, which is the typical case), honest random forest based approaches still provide a meaningful signal of how uncertain the model is about its CATE predictions, at different regions of the covariate space.

Generalized Random Forests (GRF) An alternative approach is to formulate the CATE problem as solving a local or condi-tional version of a moment restriction [5, 6]:
E[𝑚(𝑊; 𝜏0(𝑥), 𝜂0) | 𝑋 = 𝑥] = 0	(14.3.2)
where 𝑚 𝑊; 𝜏, 𝜂 is a vector of moment restrictions of the same dimension as 𝜏.
Such Generalized Random Forests are trained so as to maximize the induced heterogeneity in 𝜏 𝑥 with every split. For every node 𝑃 in some tree of the forest, let 𝜏𝑃 denote our estimate of E 𝑚 𝑊; 𝜏𝑃 , 𝜂0  𝑋  𝑃  = 0. Such an estimate can be con-
structed by solving the moment restriction with respect to 𝜏𝑃
using only the samples that fall in node 𝑃 and using an estimate
𝜂 of 𝜂0 based on auxiliary data or in a cross-fitting manner. Let
𝐶1, 𝐶2 denote the child nodes that will be created from some candidate split, with sample sizes 𝑛1 and 𝑛2 correspondingly. Then a proxy criterion that targets maximizing heterogeneity
is maximizing 𝑛1 𝜏2 + 𝑛2 𝜏𝐶2 . This is one of the criteria typi-
cally used in Generalized Random Forests. Moreover, to avoid the computational burden of resolving the moment equation for every candidate split, typically an approximation of the quantities 𝜏𝐶1 and 𝜏𝐶2 is used. In particular, a local linear ap-proximation around the estimate of the parent node is being used and locally updated, i.e. 𝜏𝐶  𝜏𝑃  1  𝑖 𝐶 𝐽𝑃𝑚 𝑊𝑖; 𝜏𝑃 , 𝜂 , with 𝐽𝑃 =  1   𝑖 𝑃 𝜕𝜏𝑚 𝑊; 𝜏𝑃 , 𝜂 , with 𝑛𝑃 being the number of
samples in the parent node.
Moreover, the final estimate 𝜏 𝑥 is derived in a manner slightly different than regression forests (albeit it coincides for the case of a regression moment, i.e. 𝑚 𝑊; 𝜏 𝑥  = 𝑦  𝜏 𝑥 ). For more
general moments, for every target point 𝑥 for which we want
to predict the CATE, the Random Forest structure is used to construct weights for every other sample 𝑖  1, . . . , 𝑛 , that capture the degree of "similarity" of 𝑥 to 𝑋𝑖. These weights roughly correspond to the fraction of trees in the forest, for which 𝑋𝑖 falls in the same leaf node as 𝑥, downweighting leafs
 


of larger size. Thus if we have trained a forest with 𝐵 trees and we let 𝐿𝑏 𝑥 denote the leaf node that a sample with covariates
𝑥 falls in at tree 𝑏 and let 𝐿𝑏 𝑥	the number of samples in that leaf, then we have:
1  𝐵  1{𝐿𝑏(𝑥) = 𝐿𝑏(𝑋𝑖)}
𝐾(𝑥, 𝑋𝑖) = 𝐵 I:	|𝐿𝑏(𝑥)|
Then to calculate 𝜏 𝑥 , we solve with respect to 𝜏 𝑥 , a weighted empirical average version of the moment condition:
𝑛
𝐾 𝑥, 𝑋𝑖 𝑚 𝑊𝑖; 𝜏 𝑥 , 𝜂 = 0	(14.3.3)
𝑖=1
using the weights that are derived based on the similarity metric induced by the forest structure.
When the forest construction process satisfies the criteria we defined earlier in the section of honesty, balancedness, random feature splitting, fully grown trees and sub-sampling without replace-ment, then under similar conditions as in the case of Regression Forests, the prediction of a Generalized Random Forest (and its extension, the Orthogonal Random Forest) can be shown to be asymptotically normal and an asymptotically valid confidence interval construction can be employed. Albeit the same limita-tions as we described in the regression case, carry over to the confidence intervals produced by these methods.

Causal Forests: a GRF for CATE We describe here an empiri-cally popular variant of causal forests that uses the Generalized Random Forest formulation. Albeit, unlike the Doubly Robust
Forest approach, this approach is valid only if 𝑋 = 𝑍 or if we make the stronger further assumption that the high-dimensional CATE function 𝛿0(𝑍) = E[𝑌(1) − 𝑌(0) | 𝑍], is only a function of the variables 𝑋, i.e. 𝛿0(𝑍) = 𝜏0(𝑋) and 𝜏0.
In this case, for a binary treatment, we can write without loss of generality
E[𝑌 | 𝐷, 𝑍] = 𝜋0(𝑍) 𝐷 + 𝑔0(𝑍)
where 𝜋0 𝑍 = E 𝑌  𝐷 = 1, 𝑍  E 𝑌  𝐷 = 0, 𝑍 is the conditional average predictive effect (CAPE). Moreover, by conditional exogeneity, the CAPE function 𝜋0 is equal to the high-dimensional CATE function 𝛿0. Thus, for a binary treat-
ment we can always write the regression equation:
𝑌 = 𝛿0(𝑍) 𝐷 + 𝑔0(𝑍) + 𝜖,	E[𝜖 | 𝐷, 𝑍] = 0
 


From this, we can derive that E 𝑌	𝑍 = 𝛿0 𝑍 E 𝐷	𝑍	𝑔0 𝑍 . Subsequently, we can write:
𝑌 − E[𝑌 | 𝑍] = 𝛿0(𝑋) (𝐷 − E[𝐷 | 𝑍]) + 𝜖
Letting 𝑌ˆ = 𝑌 − E[𝑌 | 𝑍] and 𝐷ˆ = 𝐷 − E[𝐷 | 𝑍] and since
𝑍, 𝐷ˆ is uniquely determined by 𝐷, 𝑍, we can conclude that the
following regression equation holds:
𝑌ˆ = 𝛿0(𝑍) 𝐷ˆ + 𝜖,	E[𝜖 | 𝐷ˆ , 𝑋] = 0
If we now further assume that 𝛿0(𝑍) = 𝜏0(𝑋), and since 𝑋, 𝐷ˆ
is a subset of 𝑍, 𝐷ˆ , we can write the regression equation:
𝑌ˆ = 𝜏0(𝑋) 𝐷ˆ + 𝜖,	E[𝜖 | 𝐷ˆ , 𝑋] = 0	(14.3.4)
From this regression equation we can derive the moment con-straint:
E [(𝑌ˆ − 𝜏0(𝑥) 𝐷ˆ ) 𝐷ˆ | 𝑋 = 𝑥] = E [𝜖 𝐷ˆ | 𝑋 = 𝑥] = 0

Note that this moment equation is a conditional analogue of the Normal Equation that we used in the PLR model, where we used the equation
E(𝑌ˆ − 𝜃0 𝐷ˆ ) 𝐷ˆ = 0
to estimate the constant treatment effect under a partially linear model E 𝑌 𝐷, 𝑋 = 𝜃0𝐷 𝑔 𝑍 . Now that the coefficient associated with 𝐷 is allowed to vary with 𝑋, we can estimate
the heterogeneous coefficient by solving the same moment but conditional on 𝑋, i.e.
E [(𝑌ˆ − 𝜏0(𝑥) 𝐷ˆ ) 𝐷ˆ | 𝑋 = 𝑥] = 0	(14.3.5)

Note that the above method falls in the general framework that can be handled by Generalized Random Forests and their extension, the Orthogonal Random Forests. We can estimate
𝜏ˆ(𝑥) by estimating the nuisance function 𝜂0 = (𝑝0, 𝑞0), where
𝑝0(𝑍) = E(𝐷 | 𝑍) and 𝑞0(𝑍) = E(𝑌 | 𝑍) in a cross-fitting man-
ner, letting 𝑌ˇ = 𝑌 − 𝑝ˆ(𝑍), 𝐷ˇ = 𝐷 − 𝑞ˆ(𝑍) and then applying the
Generalized Random Forest method with moment equation:
𝑚(𝑊; 𝜏(𝑥), 𝜂ˆ) = (𝑌ˇ − 𝜏(𝑥) 𝐷ˇ ) 𝐷ˇ
The formal analysis of the validity of the confidence intervals of this approach can be found in [5] for the case when 𝑋 = 𝑍 and is low-dimensional, in which case, one does not need to account
 


for the errors in 𝜂, as long as a constant offset is also added to the moment equation, solving the vector of moments:
 

𝑚(𝑊; 𝜏(𝑥), 𝛽(𝑥), 𝜂ˆ) = (𝑌ˇ − 𝜏(𝑥) 𝐷ˇ
 
− 𝛽(𝑥)) (
 
𝐷ˇ
1
 
A formal analysis of the case when 𝑋  𝑍 and 𝑍 can be high-dimensional (or when the constant term is not added to the moment equation) can be found in [6], which also accounts for the impact of the nuisance estimation errors.

 



















Figure 14.3: Doubly Robust Forest in the 401k example.


















Figure 14.4: Causal Forest in the 401k example.

Empirical Example: The "Welfare" Experiment
We illustrate the estimation of CATEs with forests, with an em-pirical application on studying the effects of the word “welfare” on the support of people for government programs. Starting in the 1980s, the General Social Survey (GSS) started including a question around satisfaction with public spending. What is more important, the GSS conducted a randomized controlled trial where the respondents were assigned one of two variations of the same question at random. Both variations had the same meaning and introduction, albeit one was asking about satisfac-tion of the respondent with respect to government spending for “welfare programs”, while the other variation was phrased as
 


government spending for “assistance to the poor”. This small variation has been found to have substantial average effect on the response and several studies have attempted to parse out treatment effect heterogeneity.
In this section we applied Causal Forests and Doubly Robust Forests on a dataset collected from such GSS surveys from 1986 to 2010, as described in [9]. The dataset consists of 12907 samples containing i) the variant of the question that was assigned to the participant (with 𝐷 = 1 corresponding to “welfare programs”
and 𝐷 = 0 corresponding to “assistance to the poor), ii) their
numerical level of satisfaction response to the question (𝑌)
and 42 features (𝑋) that contain many characteristics of the respondent related to gender, income, education, family size and marital status, race, political views and occupation sector. The average treatment effect based on a simple two-means estimate is −0.3681 as reported in Figure 14.5.
			coef	std err	P> |z|	[0.025	0.975]  const	0.4798		0.006		0.000		0.467		0.492
𝐷	-0.3681	0.007	0.000	-0.383	-0.354	Figure 14.5: Average treatment ef-
 
fect in welfare experiment.

We constructed a Causal Forest and a Doubly Robust Forest using all the 42 variables for treatment effect heterogeneity and as controls. We used gradient boosting regression with cross-fitting to calculate the nuisance functions required for each of the forests. The hyperparameters of the nuisance estimators were selected based on cross validation. Subsequently, we looked at the most important feature in the forest, as measured by a feature importance criterion that roughly corresponds to the average reduction in the splitting criterion, every time that the feature was used for splitting. The most important feature came out to be political views, both in the Causal Forest and in the Doubly Robust Forest. Subsequently, in Figure 14.6 and Figure 14.7 we report the heterogeneous effect for each value of the polviews covariate and imputing all other covariates at their median value. The point estimate and the corresponding 5%-95% confidence intervals that are provided by the forest methods are depicted.
 



















Figure 14.6: R-Learner based causal forest in the welfare example.


















Figure 14.7: Doubly Robust honest regression forest in the welfare ex-ample.

14.4	Notebooks
Notebook 14.4.1 R Notebook for DML on CATE [3]: Analyzes summaries of the CATE of 401(K) conditional on income.

Notebook 14.4.2 Python Notebook for CATE Inference [10]: Analyzes CATE for welfare experiment and for 401k experi-ment with Best Linear Predictors of CATE and with Random Forest and Causal Forest based methods.
 


Bibliography



[1]		Nassim Nicholas Taleb. The Black Swan: The Impact of the Highly Improbable. Random House Group, 2007 (cited on page 379).
[2]	Vira Semenova and Victor Chernozhukov. ‘Debiased machine learning of conditional average treatment effects and other causal functions’. In: The Econometrics Journal
24.2 (2021), pp. 264–289 (cited on page 386).
[3]	R Notebook for DML on CATE. 2024. url: https : / / colab.research.google.com/github/CausalAIBook/ MetricsMLNotebooks/blob/main/T/dml-for-conditional-average-treatment-effect.irnb (cited on pages 386, 397).
[4]	Stefan Wager and Susan Athey. ‘Estimation and Infer-ence of Heterogeneous Treatment Effects using Random Forests’. In: Journal of the American Statistical Association
113.523 (2018), pp. 1228–1242 (cited on page 389).
[5]		Susan Athey, Julie Tibshirani, and Stefan Wager. ‘Gener-alized Random Forests’. In: The Annals of Statistics 47.2 (2019), pp. 1148–1178 (cited on pages 389, 391, 393).
[6]		Miruna Oprescu, Vasilis Syrgkanis, and Zhiwei Steven Wu. ‘Orthogonal random forest for causal inference’. In: International Conference on Machine Learning. PMLR. 2019,
pp. 4932–4941 (cited on pages 389, 391, 394).
[7]	Vasilis Syrgkanis and Manolis Zampetakis. ‘Estimation and Inference with Trees and Forests in High Dimensions’. In: Proceedings of Thirty Third Conference on Learning Theory. Ed. by Jacob Abernethy and Shivani Agarwal. Vol. 125. Proceedings of Machine Learning Research. PMLR, 2020,
pp. 3453–3454 (cited on page 390).
[8]	Christopher Genovese and Larry Wasserman. ‘Adaptive confidence bands’. In: Annals of Statistics 36.2 (2008),
pp. 875–905 (cited on page 390).
[9]	Donald P Green and Holger L Kern. ‘Modeling hetero-geneous treatment effects in survey experiments with Bayesian additive regression trees’. In: Public opinion quarterly 76.3 (2012), pp. 491–511 (cited on page 396).
 

 
Bibliography
 
399
 


[10]	Python Notebook for CATE Inference. 2024. url: https:// colab.research.google.com/github/CausalAIBook/ MetricsMLNotebooks/blob/main/T/CATE-inference. ipynb (cited on page 397).
 

 
Estimation and Validation of Heterogeneous Treatment
Effects

"You can, for example, never foretell what any one man will do, but you can say with precision what an average number will be up to."
– Sherlock Holmes [1].










We study flexible estimation of heterogeneous treatment effects. We target the construction of an estimate of the true CATE function and not its projection on a simpler function space, with as small root-mean-squared-error as possible. We consider flexible estimation using generic ML techniques and discuss how one can perform model selection and out-of-sample val-idation of the quality of the learned model of heterogeneity. We conclude with the topic of policy learning, i.e. constructing optimal personalized policies.
 
15
15.1	ML Methods for CATE Esti-mation	401
Meta-Learning Strategies for CATE Estimation	401
Qualitative Comparison and Guidelines	411
Guarding for Covariate Shift	414
15.2	Scoring for CATE Model Se-lection and Ensembling . 420 Comparing Models with Confidence	421
Competing with the Best Model	425
15.3	CATE Model Validation 431 Heterogeneity Test Based on Doubly Robust BLP	432
Validation Based on Calibra-tion	433
Validation Based on Uplift Curves	437
15.4	Empirical Example: The "Welfare" Experiment    447
15.5	Empirical Example: Digital Advertising A/B Test    454
15.AAppendix: Lower Bound on Variance in Model Compari-son	457
15.	BAppendix: Interpretation of Uplift curves	458
 

15.1	ML Methods for CATE Estimation
We consider the same setting as in Chapter 14 of analyzing the heterogeneous effect of a binary treatment in the presence of a high-dimensional set of observed controls 𝑍, under conditional exogeneity. In this section, we target the construction of an estimate 𝜏 𝑋 of the true CATE function 𝜏0 𝑋 and not its best linear approximation, using generic ML techniques, in a manner such that the mean squared error E𝑋 𝜏0 𝑋  𝜏 𝑋 2, which is also what we used in Chapter 9 to measure the quality of non-linear predictive ML models, is minimized. We will also be interested in the mean squared error of the estimate with respect to the best approximation of the CATE over some flexible, potentially non-linear function space 𝑇, i.e. the function
𝜏∗ defined as
𝜏∗ = arg min E𝑋(𝜏0(𝑋) − 𝜏(𝑋))2.	(15.1.1)

As in the previous section, the key is to decompose the esti-mation of the CATE into a sequence of regression problems. Then generic ML techniques can be used to address each of these regression problems. This approach has been coined meta-learning in the literature on CATE estimation, since we are trying to treat ML techniques as a black-box oracle that solves any regression problem and we are trying to build on top of that oracle to learn the CATE. Motivated by the ability to construct confidence intervals, in the previous section, we provided one such choice of a reduction, as we will explain later. However, when one is primarily interested in mean squared error, other decompositions could potentially have better finite sample per-formance. We present here the multitude of such meta-learning approaches that have been proposed in the literature and we will conclude with a comparative analysis of each of them.

Meta-Learning Strategies for CATE Estimation
To simplify the exposition, and emphasizing the meta-learning aspect of these methods, we will introduce a notation for a
regression estimate oracle. We denote with 𝑂𝐻({𝑋𝑖 , 𝑌𝑖 , 𝑊𝑖 }𝑛  )
an oracle algorithm that takes as input a dataset of 𝑛 i.i.d. sam-ples, consisting of covariatex 𝑋𝑖, regression labels 𝑌𝑖 and sample weights 𝑊𝑖 (where weights are assumed to be independent of
𝑌𝑖 given 𝑋𝑖) and produces an estimate ℎˆ of the function that
 


 
minimizes the sample-weighted square loss:
ℎ0 = arg min E𝑊(𝑌 − ℎ(𝑋))2
= arg min E𝑊(E(𝑌 | 𝑋) − ℎ(𝑋))2
 



(15.1.2)
 
over some function space 𝐻. When sample weights 𝑊𝑖 are omitted, they will be assumed to be equal to 1. This oracle will typically correspond to some ML approach to solving this weighted regression problem and we will be assuming that
such an oracle provides an estimate ℎˆ that converges to ℎ0 at
some rate, with respect to the mean-squared-error metric, i.e.
∥ ℎˆ − ℎ0∥𝐿2(𝑋) = 𝑟𝑛 → 0.

Single (S)-Learner Starting from the very simple identifica-tion formula for the CATE in Equation (14.1.1), we can learn the CATE by first invoking an ML regression method to con-
struct an estimate 𝑔ˆ of the conditional expectation function
𝑔0 𝐷, 𝑍 := E 𝑌 𝐷, 𝑍 , assuming that 𝑔0 lies in some function space 𝐺. Then we can construct a model 𝜏 of the CATE by
invoking an ML regression method to construct an estimate of the conditional expecation function E 𝑔 1, 𝑍  𝑔 0, 𝑍  𝑋 , over some function space 𝑇. Overall we arrive at the following meta-learning algorithm:

Single Learner (S-Learner)

𝑔ˆ := 𝑂𝐺({(𝐷𝑖 , 𝑍𝑖), 𝑌𝑖 }𝑛  )	(15.1.3)
𝑖=1
𝜏ˆ := 𝑂𝑇 ({𝑋𝑖 , 𝑔ˆ(1, 𝑍𝑖) − 𝑔ˆ(0, 𝑍𝑖)}𝑛 )
𝑖=1

Even if 𝑇 does not contain 𝜏0, as long as 𝑔0   𝐺 and  𝑔
𝑔0 𝐿2 𝐷,𝑍	0, the S-Learner estimate will be converging to the best approximation of the CATE within the space 𝑇, i.e.
∥𝜏ˆ − 𝜏∗ ∥𝐿2(𝑋) → 0.

Two (T)-Learner Estimating a single regression model that predicts the outcome 𝑌 from the treatment 𝐷 and the controls
𝑍, can overly regularize the treatment variable. Especially in settings where the treatment has a small effect, many ML algorithms will most probably shrink the treatment effect to zero and prioritize the inclusion of other informative covariates in the selected model. For this reason, it seems natural to weaken this regularization bias on the treatment. This can be achieved
 


by fitting two separate models, one model 𝑔𝑇 that estimates the relationship between the outcome 𝑌 and the covariates 𝑍 within
the treated group, i.e. 𝑔𝑇 := E[𝑌 | 𝑍, 𝐷 = 1] and one model
𝑔𝐶 that learns the same relationship within the control group,
i.e. 𝑔𝐶 := E 𝑌 𝑍, 𝐷 = 0 . Then the CATE can be estimated by invoking an ML regression method to construct an estimate of the CEF E 𝑔𝑇 𝑍   𝑔𝐶 𝑍   𝑋 . Overall, we arrive at the
following meta-learning algorithm:

Two Learner (T-Learner)
𝑔ˆ𝐶 := 𝑂𝐺𝐶 ({𝑍𝑖 , 𝑌𝑖 }𝑖∈{1,...,𝑛}:𝐷 =0)
𝑖
𝑔ˆ𝑇 := 𝑂𝐺𝑇 ({𝑍𝑖 , 𝑌𝑖 }𝑖∈{1,...,𝑛}:𝐷 =1)	(15.1.4)
𝑖
𝜏ˆ := 𝑂𝑇({𝑋𝑖 , 𝑔ˆ𝑇 (𝑍𝑖) − 𝑔ˆ𝐶 (𝑍𝑖)}𝑛 )
𝑖=1

Similar to the S-Learner, as long as 𝑔𝐶 ∈ 𝐺𝐶 and 𝑔𝑇 ∈ 𝐺𝑇, then
the result of the T-Learner will always be converging to 𝜏∗, i.e. the best approximation of the CATE within 𝑇.

 
Doubly Robust (DR)-Learner The above approaches rely fully on accurate outcome modelling. If we face settings where the conditional counterfactual outcomes E 𝑌  𝐷 = 1, 𝑍 are
complicated functions that are hard to model and estimate, but
the CATE function 𝜏 𝑋 is relatively simple, the aforementioned two meta-learners will suffer from large estimation errors in
𝑔, 𝑔𝑇 , 𝑔𝐶. If for instance, we are in a randomized controlled trial and we know the propensity 𝜇0, then we also know that the random variable 𝑌𝐻(𝜇0) satisfies
E[𝑌𝐻(𝜇0) | 𝑋] = E[E[𝑌𝐻(𝜇0) | 𝑍] | 𝑋]
= E[E[𝑌 | 𝐷 = 1, 𝑍] − E[𝑌 | 𝐷 = 0, 𝑍] | 𝑋]
= 𝜏(𝑋).
Thus when solving this regression problem we only need to be accurately approximating the potentially simpler CATE function, as opposed to the response functions under treatment or control.1
Beyond randomized control trials, the above approach is too heavily dependent on constructing a good estimate 𝜇 of the propensity score. Moreover, even for randomized control trials, the latter method can have very large variance, due to divid-ing the outcome 𝑌 by the inverse propensity. For this reasons, it might be beneficial even when we care solely about mean
 





















1: An estimation strategy based on running a regression of 𝑌𝐻 𝜇 on
𝑋, is referred to in the literature as the Inverse Propensity Score (IPS)-Learner, but we will omit more de-tails on it.
 


squared error, to use the doubly robust approach, which com-bines propensity and regression modelling and can reduce both bias due to errors in estimating the propensity and variance by dividing only the un-explained variation in the outcome by the propensity. This leads to the doubly robust meta-learner (we will describe its two-learner variant, which is advisable in practice):

Doubly Robust Learner (DR-Learner)
𝑔ˆ𝐶 := 𝑂𝐺𝐶 ({𝑍𝑖 , 𝑌𝑖 }𝑖∈{1,...,𝑛}:𝐷 =0)
𝑖
𝑔ˆ𝑇 := 𝑂𝐺𝑇 ({𝑍𝑖 , 𝑌𝑖 }𝑖∈{1,...,𝑛}:𝐷 =1)
𝑖
𝜇ˆ := 𝑂𝑀({𝑍𝑖 , 𝐷𝑖 }𝑖∈{1,...,𝑛})	(15.1.5)
𝑔ˆ(𝐷, 𝑍) := 𝑔ˆ𝑇 (𝑍) 𝐷 + 𝑔ˆ𝐶 (𝑍) (1 − 𝐷)
𝑌𝑖(𝜂ˆ) := 𝐻𝑖(𝜇ˆ) (𝑌𝑖 − 𝑔ˆ(𝐷𝑖 , 𝑍𝑖)) + 𝑔ˆ𝑇 (𝑍𝑖) − 𝑔ˆ𝐶 (𝑍𝑖)
𝜏ˆ := 𝑂𝑇 ({𝑋𝑖 , 𝑌𝑖(𝜂ˆ)}𝑛 )
𝑖=1

 
The previous section can be seen as a special of this meta-learner, where we use OLS as our regression oracle in the final step and were we use cross-fitting when we estimate the first three steps
and calculate the proxy labels 𝑌ˆ𝑖 .
The DR-Learner inherits certain type of double robustness
 

2: For any estimate 𝑓ˆ of a function
𝑓0, that takes as input some random variable 𝑊, the fourth moment of
the prediction error  𝑓ˆ	𝑓0 𝐿4 𝑋 is
defined as
 
properties that we have, even when analyzing the mean squared error. In particular, suppose that we knew the true regression
 
(E𝑊
 
( 𝑓ˆ(𝑊 ) − 𝑓 (𝑊))4 )1/4
 
functions 𝑔𝐶 , 𝑔𝑇 , 𝜇0 and based on some appropriate argument,
 
and is a slightly stronger measure
 
0	0
 
we could show that the regression oracle in the last step, when ran with the ideal labels 𝑌(𝜂0), achieves a mean squared error
 
of performance that the root-mean-squared-error, i.e.
 
2	𝐶
 
𝐶	𝑇
 
𝑇		
 
rate of the order of 𝑟𝑛. Then, assuming 𝑔0 ∈ 𝐺
 
, 𝑔0 ∈ 𝐺
 
and	JE ( 𝑓ˆ(𝑊 ) − 𝑓 (𝑊))2
 
𝜇0	𝑀, one can argue, under benign regularity conditions,	𝑊	0
that the mean squared error of the DR-Learner, can be upper	.
 
bounded as:
2	2	2
 
3: In fact, one can always use the slightly better error metric:
2
 
E(𝜏ˆ(𝑋) − 𝜏∗(𝑋)) ≲ 𝑟𝑛 + Error(𝑔ˆ) · Error(𝐻(𝜇ˆ))	(	r
 
2	2l)1/4
 

 
be the fourth moment of the prediction error2 and under further regularity conditions on the function space 𝑇 used in the estimation for 𝜏, it can be taken to be the root-mean-squared-error.3 Thus as long as the product of the errors in modelling the regression and the propensity function are small, then the mean squared error for 𝜏 will not be significantly impacted by these first stage estimation errors. For formal versions of variants of such results, we refer the reader to the following papers [2–6].
 
where 𝑋 is the set of variables that enter the CATE function 𝜏. When
𝑋 is the empty set, as in the case of average causal effects this boils down to the mean squared error and otherwise this requires better control, on average, of the condi-tional mean squared error of the nuisance functions, conditional on the variables 𝑋 that enter the CATE function.
 


Residual (R)-Learner If we know that the true CATE model lies in a simple function space and even if we knew the true nuisance parameters 𝜂0, the labels that are used in the final stage of the DR-Learner can still have a large magnitude, due to the division by the propensity. In settings were the overlap assumption is almost violated at particular regions of the covari-ate space, the regression labels 𝑌𝑖 𝜂0 will be taking very large values in absolute magnitude. This can lead to a high-variance estimate. For instance, if we knew that the treatment effect is constant, then we are essentially assuming the partially linear regression model and we shouldn’t be using the doubly robust method, but rather the residual-on-residual method, which
minimizes the loss E(𝑌ˆ𝑖 − 𝜏𝐷ˆ 𝑖)2, where 𝑌ˆ = 𝑌 − E[𝑌 | 𝑍] and
𝐷ˆ = 𝐷 − E[𝐷 | 𝑍]. Similarly, if we are willing to assume that
the CATE function is linear in some engineered features of only the variables 𝑋, i.e. 𝛿 𝑍 = 𝜏 𝑋 = 𝛽′𝑝 𝑋 , then we should instead be estimating a linear interactive model, where we
interact the treatment with the engineered features and apply the residualization apporach to arrive at the loss function
E(𝑌ˆ𝑖 − 𝛽′𝑝(𝑋) 𝐷ˆ 𝑖)2
since 𝑝(𝑋)𝐷−E[𝑝(𝑋)𝐷 | 𝑍] = 𝑝(𝑋) (𝐷−E[𝐷 | 𝑍]) = 𝑝(𝑋) 𝐷ˆ .
Analogously, if we know that the high-dimensional CATE function 𝛿0 𝑍 = E 𝑌 1 𝑌 0 𝑍 , is only a function of the variables 𝑋, i.e. 𝛿0 𝑍 = 𝜏0 𝑋 and 𝜏0 lies in some simple space
𝑇, a lower variance loss function, than the doubly robust loss
function would be:
min E(𝑌ˆ𝑖 − 𝜏(𝑋)𝐷ˆ 𝑖)2
As we already showed in Equation (14.3.4) in Section 14.3, under the aforementioned assumptions we can write the regression equation:
𝑌 = 𝜏0(𝑋) 𝐷 + 𝑔0(𝑍) + 𝜖,	E[𝜖 | 𝐷, 𝑍] = 0
Thus, we are faced with a non-linear regression equation, re-gressing 𝑌ˆ on 𝐷ˆ , 𝑋, where we know that the CEF is of the form
E[𝑌ˆ | 𝐷ˆ , 𝑋] = 𝜏(𝑋) 𝐷ˆ , for some function 𝜏 in some simple func-
tion space 𝑇. To estimate this regression problem, we should thus minimize the square loss, over the space of such CEFs, i.e.
min E(𝑌ˆ − 𝜏(𝑋)𝐷ˆ )2 ,	(15.1.6) which is exactly the R-Learner loss.
 


When taken to estimation, the residuals 𝑌ˆ , 𝐷ˆ will be replaced
by the estimated residuals 𝑌ˇ , 𝐷ˇ , where 𝑌ˇ = 𝑌 − ℎˆ(𝑍) and
𝐷ˇ = 𝐷 − 𝜇ˆ(𝑍), with ℎˆ being an estimate of the CEF E[𝑌 | 𝑍]
(e.g. one could use the two-learner based estimate)
ℎˆ(𝑍) := 𝑔ˆ𝑇 (𝑍) 𝜇ˆ(𝑍) + 𝑔ˆ𝐶 (𝑍) (1 − 𝜇ˆ(𝑍)).
or a direct regression, regressing 𝑌 on 𝑍. Moreover, note that minimizing the R-Learner loss, is equivalent to minimizing a sample-weighted square loss, where the covariates are 𝑋, the
labels are 𝑌ˆ /𝐷ˆ and the weights are 𝐷ˆ 2:
E(𝑌ˆ − 𝜏(𝑋)𝐷ˆ )2 = E𝐷ˆ 2 (𝑌ˆ /𝐷ˆ − 𝜏(𝑋)) ,
Thus the final step in the R-Learner also corresponds to a sample-weighted regression oracle problem. This leads to the following meta-learner algorithm:

Residual Learner (R-Learner)
ℎˆ := 𝑂𝐻({𝑍𝑖 , 𝑌𝑖 }𝑖∈{1,...,𝑛})
𝜇ˆ := 𝑂𝑀({𝑍𝑖 , 𝐷𝑖 }𝑖∈{1,...,𝑛})
𝑌ˇ𝑖 := 𝑌𝑖 − ℎˆ(𝑍𝑖)	(15.1.7)
𝐷ˇ 𝑖 := 𝐷𝑖 − 𝜇ˆ(𝑍𝑖)
𝜏ˆ := 𝑂𝑇 ({𝑋𝑖 , 𝑌ˇ𝑖 /𝐷ˇ 𝑖 , 𝐷ˇ 2)}𝑛  )
𝑖	𝑖=1

Under the assumption that 𝛿0 = 𝜏0   𝑇, and that ℎ0   𝐻,
𝜇0  𝑀, then the R-Learner converges to the true CATE 𝜏0.
Moreover, this approach inherits similar robustness properties as the partialling out approach for the case of estimating average causal effects. In particular, if we let 𝑟2 denote the mean squared error that the final regression oracle would have achieved had we known the true nuisance parameters ℎ0, 𝜇0, then under regularity conditions, one can show that:
E𝑋(𝜏ˆ(𝑋) − 𝜏0(𝑋))2 ≲ 𝑟2 + Error(𝜇ˆ)4 + Error(𝜇ˆ)2 Error(ℎˆ)2
Unlike the DR-Learner, we see here that accurate estimation of the propensity is more important and cannot be compensated by more accurate estimation of the outcome regression problem. Similar to the DR-Learner, the error function in the above claim can always be taken to be the fourth moment of the prediction error and under further restrictions on the function space 𝑇, it can be taken to be the root-mean-squared-error. For formal versions of this claim see [4, 6, 7].
 


One may also wonder what does the R-Learner estimate when the assumption that 𝛿0 = 𝜏0 or that 𝜏0 𝑇 is violated. Unlike all prior meta-learners, the R-Learner does not converge necessarily to the best approximation 𝜏∗ of the CATE within 𝑇. For instance, consider the extreme case where 𝑇 contains only constant
functions. Then we are estimating an average treatment effect based on a partialling out approach, while the partial linear response function does not hold and there exists treatment effect heterogeneity. In this case, the partialling out approach will not be converging to the average causal effect and similarly for any 𝑇, the R-Learner will not be converging to 𝜏∗.
To understand the limit point of the R-Learner, let us examine the R-Learner loss as defined in Equation (15.1.6). By construction, 𝜏 will be converging to the solution to that minimization problem. As we have already argued, under conditional exogeneity, we
can always write 𝑌ˆ = 𝛿0(𝑍)𝐷ˆ + 𝜖, with E[𝜖 | 𝐷ˆ , 𝑍] = 0. Thus
we can re-write the R-Learner loss as:
E(𝑌ˆ − 𝜏(𝑋)𝐷ˆ )2 = E(𝛿0(𝑍)𝐷ˆ − 𝜏(𝑋)𝐷ˆ )2 + E𝜖2
= E [(𝛿0(𝑍) − 𝜏(𝑋))2 Var(𝐷 | 𝑍)] + E𝜖2
where we used the fact that E 𝐷ˆ 2 𝑍 = Var 𝐷 𝑍 . Thus minimizing the R-Learner loss is equivalent to minimizing a treatment-variance-weighted square loss and the estimate
will be converging to the best treatment-variance-weighted approximation of the high-dimensional CATE function, i.e.
𝜏 = arg min E	𝛿0 𝑍	𝜏 𝑋  2 Var 𝐷	𝑍	(15.1.8)
𝜏∈𝑇
This solution is essentially putting more weight on regions of the covariate space 𝑍, where the treatment was more randomly assigned. If for instance parts of the population were almost always treated or almost always not treated, then these parts of the population will not be considered when constructing the best approximation. We will refer to this solution as the best overlap-weighted approximation, since it assigns weights to parts of the population, dependent on the degree of "overlap" (i.e. whether both treatments were observed for this part of the population). For instance, suppose that 𝑇 is the space of constant functions and that the treatment is randomly assigned for some parts of the population and is essentially deterministic
for other parts. Then 𝜏˜ will recover the average treatment
effect of the subset of the population for which treatment was randomly assigned. On the contrary, in this case the doubly robust estimate will try to recover the average causal effect of the overall population, but because of that it will inadvertently
 


be very high variance and unstable, since for some parts of the population it barely ever sees one of the two treatments.

Cross (X)-Learner The Cross Learner tries to combine propen-sity to improve on outcome modelling in a manner qualitatively very different from the DR- or R-learner and not with the target of reducing the sensitivity to errors in the nuisance models. Rather it does so primarily motivated from an accuracy and covariate-shift consideration. Moreover, it begins with a very dif-ferent starting point and idea. As a first one realizes that the high-dimensional CATE 𝛿0 𝑍 is the same, whether we mea-sure it on the treated or on the control! In other words, the Conditional Average Treatment Effect on the Treated (CATT) is equal to the Conditional Average Treatment Effect on the Control (CATC), unlike the average treatment effect, which can be different due to different distributions of 𝑍 in treatment and control. This can be easily seen as, by conditional exogeneity:
𝛿𝑇(𝑍) := E[𝑌(1) − 𝑌(0) | 𝑍, 𝐷 = 1]
 
= E[𝑌(1) − 𝑌(0) | 𝑍] = 𝛿0(𝑍)
and similarly for 𝜏0.4
Moreover, when we try to measure the CATT, then we actually observe the counterfactual under treatment and therefore we do not need to impute this counterfactual outcome (e.g. by learning 𝑔1). Similarly for the CATC. Thus we can identify the CATT and CATC as:
𝛿𝑇(𝑍) = E[𝑌 − E[𝑌 | 𝑍, 𝐷 = 0] | 𝑍, 𝐷 = 1]
𝛿𝐶(𝑍) = E[E[𝑌 | 𝑍, 𝐷 = 1] − 𝑌 | 𝑍, 𝐷 = 0]
This yields two ways of identifying the CATE 𝛿0 𝑍 and any convex combination of these two solutions, would also be a valid identification strategy for the CATE. This approach, allows us to avoid having to model both response models well for all regions of the covariate space (which would be the case for the S-, T-, or DR-Learners). This can be powerfull if we know that the CATE is a much simpler function to learn than a mean counterfactual response model.
If we believe that the hard part is modelling the mean counter-factual response under some treatment but not the treatment effect, then we can use the following strategy: for parts of the covariate space 𝑍, where we have more control data (i.e. 𝜇0 𝑍 is small), we can use the CATT strategy, which only requires estimating the mean counterfacutal response under control, i.e.
 


4: Note that the same wouldn’t be true if we condition a subset 𝑋 of
𝑍:
𝜏𝑇(𝑋) := E[𝑌(1) − 𝑌(0) | 𝑋, 𝐷 = 1]
= E[E[𝑌(1) − 𝑌(0) | 𝑍] | 𝑋, 𝐷 = 1]
= E[𝛿(𝑍) | 𝑋, 𝐷 = 1]
≠ E[𝛿(𝑍) | 𝑋] =: 𝜏0(𝑋)
 


E 𝑌 𝑍, 𝐷 = 0 , but not under treatment. Of course, we still have to learn the effect function using only the treated data, which we don’t have that many in this part of 𝑍, but since we
believe that the effect function is simple, this is a more benign problem. Similarly, if for parts of the covariate space 𝑍, we have more treated data (i.e. 𝜇0 𝑍 is large), we can use the CATC strat-egy, which only requires estimating the mean counterfactual response under treatment, i.e. E 𝑌  𝑍, 𝐷 = 1 , but not under
control. This motivates using the following convex combination
as our final identification formula for the CATE:
𝛿0(𝑍) = 𝛿𝑇(𝑍) (1 − 𝜇0(𝑍)) + 𝛿𝐶(𝑍) 𝜇0(𝑍)
Subsequently, for any subset 𝑋 of 𝑍, we can use the fact that
𝜏0 𝑋 = E 𝛿0 𝑍	𝑋 . This identification strategy leads to the following meta-learning estimation strategy:

Cross Learner (X-Learner)
𝑔ˆ𝐶 := 𝑂𝐺𝐶 ({𝑍𝑖 , 𝑌𝑖 }𝑖∈{1,...,𝑛}:𝐷 =0)
𝑖
𝑔ˆ𝑇 := 𝑂𝐺𝑇 ({𝑍𝑖 , 𝑌𝑖 }𝑖∈{1,...,𝑛}:𝐷 =1)
𝑖
𝜇ˆ := 𝑂𝑀({𝑍𝑖 , 𝐷𝑖 }𝑖∈{1,...,𝑛})
𝛿ˆ𝐶 := 𝑂Δ({𝑍𝑖 , 𝑔ˆ𝑇 (𝑍𝑖) − 𝑌𝑖 }𝑖∈{1,...,𝑛}:𝐷 =0)
𝑖	(15.1.9)
𝛿ˆ𝑇 := 𝑂Δ({𝑍𝑖 , 𝑌𝑖 − 𝑔ˆ𝐶 (𝑍𝑖)}𝑖∈{1,...,𝑛}:𝐷 =1)
𝑖
𝛿ˆ𝑋 (𝑍) := 𝛿ˆ𝑇 (𝑍) (1 − 𝜇ˆ(𝑍)) + 𝛿ˆ𝐶 (𝑍) 𝜇ˆ(𝑍)
𝜏ˆ := 𝑂𝑇 ({𝑋𝑖 , 𝛿ˆ𝑋 (𝑍𝑖)}𝑛 )
𝑖=1

 
Assuming that the function spaces used in the nuisance oracles contain the true functions, the final step of the X-learner will converge to the best approximation of the CATE 𝜏∗, within the space 𝑇. Moreover, this estimation strategy can have substan-tial benefits when the CATE function 𝛿0 is much simpler than the response functions 𝑔𝐶 , 𝑔𝑇 and when there are substantial
 



Figure 15.1: DGP 1. Imbalanced dataset were baseline response is more complex than heterogeneous effect.
 
0	0
 
imbalances in the treatment across the population (i.e. the propensities substantially deviate from 1 2). The latter many times arises in digital experimentation, where only a small fraction of the population receives the treatment. In this case, the response under control can be much more accurately esti-mated. In fact, in many such settings we have a lot of historical data, prior to running an experiment, where the treatment was un-available and which can be used as auxiliary datasets for learning the baseline response; with the small treated data from the experiment only being used to estimate the heterogeneous effect function 𝛿𝑇.
 

 
Figure 15.2: DGP 1. Different CATE estimates in the X-Learner. The leg-end displays the mean squared er-ror of each estimate.
 



Example 15.1.1 (Imbalanced Dataset with Hard Baseline Re-sponse and Simple CATE) As a stark example, consider the case when 𝑍 = 𝑋   𝑈 0, 1 , treatment is very rare, i.e.
𝜇 𝑍  = .05, the treatment effect is constant, i.e. 𝛿0 𝑍  = .5 and
the baseline response is complex and contains a discontinuity:

 
𝑌 = .5 𝐷 + .3 𝟙{𝑍 ∈ [.6, .8]} + 𝑁(0, 𝜎 = .05),
𝐷 = Bernoulli(𝜇(𝑍) = .05)
 
(DGP 1)
 
In this case, the data that we collect, for 𝑛 = 500, are depicted in Figure 15.7. If we use gradient boosted forest regression
to estimate the two response functions under treatment and under control, we find that the 𝑔𝑇 response function is substan-tially more regularized and the discontinuity is not learned, due to the small sample size. On the other hand 𝑔𝐶 is much more accurate and the discontinuity is learned due to the large sample size. Subsequently, we see in Figure 15.2 that the estimate based on the CATC identification strategy is much less accurate than the one based on the CATT identifi-cation strategy. Moreover, the X-Learner is putting almost all
the weight on the CATT estimate 𝛿ˆ𝑇 and is highly accurate compared to 𝛿ˆ𝐶 . However, in this setting, we also find that
other strategies that also use propensity modelling (e.g the R- or DR-Learners) also manage to correct the error in the T-Learner regression models and achieve similar accuracy to the X-Learner.
On the other hand, if the inductive bias that the CATE is simpler than the response functions under either treatment or control does not hold, then the superiority of the X-Learner strategy as compared for instance to the T-learner strategy for outcome modelling vanishes. For instance, if we instead have an outcome model of:
 
𝑌 = .5 𝟙{𝑍 ∈ [.6, .8]} 𝐷 + .1 + 𝑁(0, 𝜎 = .05),
𝐷 = Bernoulli(𝜇(𝑍) = .05)
 

(DGP 2)
 
then all methods that only rely on outcome modelling fail and methods that also combine propensity based identification start to outperform (see Figure 15.4). Even more vivid is the flip in performance if we further make the treatment more prevalent than the baseline:

 
𝑌 = .5 𝟙{𝑍 ∈ [.6, .8]} 𝐷 + .1 + 𝑁(0, 𝜎 = .05),
𝐷 = Bernoulli(𝜇(𝑍) = .95)
 
(DGP 3)
 


 
 
We conclude by noting that the reasoning in the cross learner strategy can actually be used as a sub-process to improve out-come modelling in all other learners. In particular, note that the key advance of the cross learner is to observe than when the treatment is very rare, then we should be estimating the response 𝑔ˆ𝐶 under control and then estimating only the ef-
fect 𝛿ˆ𝑇 using the treatment data. In this case, we can also use
𝑔𝑇 = 𝑔𝐶 𝛿ˆ𝑇 as our estimate of the response under treatment. Similarly, if the control group is very rare, then we should be estimating the response 𝑔ˆ𝑇 under treatment and then esti-mating only the effect 𝛿ˆ𝐶 using the control data. In this case,
 

 
Figure 15.3: DGP 1. CATE esti-mates (𝑛 = 500) from other meta-learners. The legend displays the
mean squared error of each learner.
 
again we can also use 𝑔ˆ𝐶 = 𝑔ˆ𝑇 − 𝛿ˆ𝐶 as our estimate of the
 
response under control. Moreover, we can locally blend these two estimation strategies by weighting both estimates of the two response functions using the propensity, i.e. putting a weight of
1  𝜇 𝑍  to the first estimation strategy and a weight of 𝜇 𝑍 to
the second estimation strategy. This approach is an alterantive outcome modelling process that can be used instead of the 𝑆 or 𝑇 learner approaches for learning the response functions under the different treatments. In that respect, the X-Learner outcome modelling strategy can be used in conjunction with the DR- or the R-Learner approaches, if one wants to introduce some robustness with respect to outcome modelling by incor-prorating identification by propensity approaches. For instance, in Figure 15.3, we also depict the CATE learned if we combine the X-learner approach to outcome modelling with the doubly robust correction (coined the DRX-Learner).

Qualitative Comparison and Guidelines
We present here a set of bullet points that can guide the reader through the choosing among the different meta-learner strate-gies, dependent on inductive biases about their setting:
▶ S/T-Learner: they heavily rely on correct outcome mod-elling, trying to learn how the outcome relates to the control covariates 𝑍. If this estimation problem is hard to learn, then they will have poor performance, especially when the effect is a simple function and 𝑋 is much lower dimensional than 𝑍. However, they can have very low
 
 
Figure 15.4: DGP 2. CATE estimates (𝑛 = 500) from meta-learners, when CATE is complex and base-
line simple. The legend displays the mean squared error of each learner.

Figure 15.5: DGP 3. CATE estimates (𝑛 = 500) from meta-learners, when CATE is complex and base-
line simple and treatment very prevalent. The legend displays the mean squared error of each learner.
 


variance and be more stable as they only depend on sim-ple regression strategies. If one has to choose among the two, then the T-Learner should be preferable, if both treat-ments are sufficiently represented, since it avoids overly regularizing the treatment, which reduces the bias of the treatment effect estimate. If one of the two treatments is rare, then the X-Learner should be more preferable than the T-Learner.
▶ X-Learner: even if the X learner estimates a propensity, the propensity is primarily used to select locally, which outcome model is better and is not used to identify the effect. Thus the X learner is essentially also only per-forming outcome modelling. If we believe that the CATE function is simpler than the response functions under treatment or control, this outcome modelling estimation strategy should be preferred. Otherwise, if we believe that the CATE function is equally or more complex than the response functions, then a T-Learner approach can outperform an X-Learner approach. If we further believe that learning any of these outcome processes could po-tentially be a substantially harder task than learning the propensity, then this method can be heavily biased. In that case the DR-Learner or the R-Learner should be prefered. However, the X learner reasoning can still be useful in improving the outcome modelling part of the DR or R learners.
▶ DR-Learner: possesses doubly robust properties, in that the mean squared error of the CATE is small if either the outcome model is learned accurately or the propensity model. It is particularly useful in learning projections of the CATE on simpler function spaces or on small subsets
𝑋 of the control variables 𝑍. If 𝑋 is very small compared to 𝑍, then even the X-Learner needs to accurately learn the complex effect function 𝛿0 𝑍 accurately. However, the DR-Learner can learn the simpler CATE 𝜏0 𝑋 , if the propensity model is accurately learned. For instance, we saw in DGP 3 in Example 15.1.1, that when the CATE func-tion was complex, then methods such as the DR-Learner that incorporate propensity modelling are more accurate. However, contrary to the S/T/X-learners, when the true data generating process has extreme propensities in parts of the covariate space (i.e. parts of the population are almost deterministically either treated or not treated), then the DR-Learner can have high variance and become unstable. On the other hand the R-Learner will be ex-trapolating the CATE from nearby regions where there
 


is more overlap and assuming the CATE model space is smooth enough, such model-based extrapolation can be quite accurate.
▶ R-Learner: posseses insensitivity properties related to Neyman orthogonality, in that the impact of errors in the propensity model or the outcome model impact the CATE model only in a second order manner. In particular, if the outcome model is wrong, but the propensity model is very accurate, the CATE will be highly accurate. However, it is more heavily relying on moderately accurate propen-sity modelling, unlike the DR-Learner. If for instance the outcome model is perfect, but the propensity model is very wrong, then the DR-Learner will be highly accurate but the R-Learner will not be. On the positive side, the R-Learner is much more stable than the DR-Learner in the presence of extreme propensities, as it does not divide by the propensity score when constructing the regression labels. The reason that it can bypass that is that it inher-ently estimates only an overlap-weighted projection of the CATE and not the true projection of the high-dimensional CATE 𝛿0, when the CATE model is either mis-specified or does not solely depend on 𝑋 and not on the larger set of covariates 𝑍. In both cases the DR-Learner converges to the true CATE, while the R-Learner can potentially ignore large parts of the population to reduce its vari-ance; introducing bias and extrapolating the CATE from nearby highly overlapping regions. For instance, we show that in the case of DGP 1 in Example 15.1.1 the R-Learner was out-performing the DR-Learner, since the treatment effect was constant and the propensity very small. The R-Learner should be prefered to the DR-Learner when such overlap weighted projections are acceptable within the application context and when we believe we have a relatively accurate propensity model. In principle, a similar variance reduction can also be performed for the DR-Learner, by multiplying the DR-Learner loss with sample weights 𝑊 = Var 𝐷  𝑍 2, which would then
avoid dividing by the propensity and would converge to
an overlap weighted projection of the CATE 𝛿0, with the aforementioned sample weights, while preserving the double robust nature of the estimation (i.e. that errors in propensity can be compensated by more accurate es-timation in the outcome model and vice versa) (see e.g. [8]).
All-in-all, one should note that there is no clear winner among the X-, R- and DR-Learner methods and each can potentially be
 


the best performer in different contexts. The above discussion gives a high-level strategy of which method to use dependent on which types of phenomena one should be expecting to arise in their data. In the next section we give a more data-driven selection among these methods using out-of-sample scoring and ensembling.



Guarding for Covariate Shift
When machine learning models are evaluated on a different population of covariates than the one that they were trained on, then an important finite sample consideration is deterioration of their performance due to the covariate shift. Such population mis-match between training and evaluation typically arises when we employ ML algorithms within a CATE estimation. For instance, in the T-Learner we train an ML model on the treated datapoints and then we evaluate it on all the datapoints.
 


Similarly, in the X-Learner we train a CATE model on the treated points and then we evaluate it on all the datapoints.
In such settings, we should only expect the oracle ML model to have small mean squared error with respect to the distribution of its training data and with respect to the best approximation of the CEF, where the approximation error is calculated with respect to the training data. For instance, suppose that we
estimate a regression model ℎˆ that takes as input random
variables 𝑋 and predicts a variable 𝑌, with sample weights 𝑊, by invoking an ML regression oracle as defined in Equation (15.1.2). Assuming that the CEF ℎ := E 𝑌  𝑋 does not change between
train and evaluation data and letting 𝐷𝑡 denote the distribution
of 𝑋 in the training data and 𝐷𝑒 in the evaluation data, then our regression estimate satisfies that:
E𝑋∼𝐷𝑡 𝑊(ℎˆ(𝑋) − ℎ0(𝑋))2 ≤ 𝑟𝑛
where ℎ0 = arg minℎ 𝐻 E𝑋 𝐷𝑡 𝑊 ℎ 𝑋  ℎ 𝑋 2. Since we eval-uate this regression model on a different population, we would typically care about the following mean squared error:
E𝑋∼𝐷𝑒 𝑊𝑒 (ℎˆ(𝑋) − ℎ∗(𝑋))2
with some set of weights 𝑊𝑒 that depend on some downstream use of the model.

There are two sources of discrepancy: first the approximation error can be substantially different if we use the best approxi-mation with respect to a different distribution and second the mean squared error is measured with respect to the wrong distribution. If the true CEF ℎ0 lies in the function space 𝐻, then
 


the first problem vanishes (though in finite samples and with some growing sieve space, we should always expect some finite sample approximation bias). Simiarly, if we denote with 𝑝𝑡 the density of 𝑋 under 𝐷𝑡 and 𝑝𝑒 under 𝐷𝑒 , then if the density ratio
𝑝𝑒 (𝑋)/𝑝𝑡(𝑋) is upper and lower bounded by some constants
[𝑐, 𝐶], then we always have that:
 

E𝑋
 

∼𝐷𝑒 𝑊𝑒 (ℎ(𝑋) − ℎ0(𝑋))2 = E𝑋
 


∼𝐷
 
𝑝𝑒 (𝑋)𝑊  ℎ 𝑋	ℎ  𝑋  2
𝑡 𝑝𝑡(𝑋)
 
∈ [𝑐, 𝐶] · E𝑋∼𝐷𝑡 𝑊𝑒 (ℎ(𝑋) − ℎ0(𝑋))
Thus even if we don’t take any measures to address the covari-ate shift, by minimizing the squared error under the training distribution, we are approximately minimizing the error under the evaluation distribution. However, these constants can be quite large in practice and the magnitude of the discrepancy can be comparable to the sample size.
For these reasons a large literature in machine learning has focused on addressing such covariate shift problems by chang-ing how we train the model, when we know what the target evaluation distribution or metric will be. In its simplest form, one can instead optimize for the density ratio weighted error, i.e.:
 

E𝑋
 


∼𝐷
 
𝑝𝑒 (𝑋)𝑊  ℎ 𝑋	ℎ  𝑋  2
𝑡 𝑝𝑡(𝑋)
 
Noting also that 𝑝𝑒 (𝑋) = 𝑝(𝑋|𝑒) = 𝑝(𝑒|𝑋)𝑝(𝑡), the above is equiva-
 
𝑝 𝑋
lent to minimizing:
 
𝑝(𝑋|𝑡)
 
𝑝(𝑡|𝑋)𝑝(𝑡)
 

 

E𝑋
 

∼𝐷
 
𝑝(𝑒 | 𝑋)𝑊 ℎ 𝑋	ℎ  𝑋  2
𝑡 𝑝(𝑡 | 𝑋)
 
which requires solving two classification problems (i.e. predict-ing the probability that a sample is in population 𝑒 given 𝑋 and predicting whether the sample is in population 𝑡 given 𝑋, using the union of the populations).

Example 15.1.3 (Covariate Shift in X-Learner (continued))
Going back to our X-Learner example, we have 𝑝(𝑒 | 𝑍) = 1
(since we evaluate on all the population) and 𝑝(𝑡 | 𝑍) = 𝜇0(𝑍)
(since we train only on the training population). Moreover,
we care about evaluation weights 𝑊𝑒 = (1 − 𝜇ˆ(𝑍))2. Thus it
 


 
 
Analogous finite sample corrections can be taken throughout the meta-learner algorithms by first working out what is the target evaluation population and metric we care about and changing the training of the ML model appropriately.

Covariate shift techniques when overlap fails.	Beyond this simple approach of density weighting, many other ML methods have been developed in the literature to guard against covariate shift. One advantage of many of these alternative methods, is that they are applicable even when there is lack of overlap (i.e. when the density ratio can be unbounded or zero). For instance, one large class of covariate shift approaches within the context of neural network training, makes the assumption that overlap holds on some latent representation space 𝜙 𝑋 and not on the observed covariate space 𝑋 and that the conditional expectation function can be written as a function of these latent variables, i.e. E 𝑌	𝑋	E 𝑌	𝜙 𝑋 . In this case, one can train a neural network architecture where the first few layers of the neural network are used to construct the mapping 𝜙 𝑋 and the subsequent layers are used to construct E 𝑌	𝜙 𝑋 . Subsequently a distribution distance measure is introduced as a regularizer, that measures the distribution distance of 𝜙 𝑋 between samples that stem from the training and evaluation population. A popular metric is a variant of the Wasserstein distance. In this manner, we are trying to construct a latent representation that has approximately the same distribution under the two populations and which predicts well the target
𝑌.

Shared representation learning with neural networks. In the context of CATE estimation, the latter approach was utilized by [11, 12] within the T-Learner framework for outcome modelling. In particular, the first few layers of the network are used to represent 𝜙(𝑍), which then is used to represent both 𝑔𝑇 and
 

 
Figure 15.6: DGP 3. CATE estimates (𝑛 = 500) from meta-learners in adapted X-Learner, with covariate
shift corrections.
 



 

Z



D	dist
 
2




2



Figure 15.7: Counterfactual regret network of [11, 12], to guard against covariate shift in the T-Learner.
 

𝑔𝐶. Moreover, a Wasserstein penalty is introduced so that the distribution of 𝜙 𝑍 is similar between the treated and control population. The resulting method is typically referred to as the CFR-Net. If one believes that their setting satisfies this inductive bias, i.e. that there exists a latent representation that is sufficient for the CEF of the outcome and in which overlap holds, then this approach can be used for better outcome modelling within the context of any meta-learner. For instance, one can use the CFR-Net together with the DR- or R- learners for estimating
𝑔𝑇 , 𝑔𝐶 and hence also 𝑔0 and ℎˆ (potentially using the same
shared representation, when estimating the propensity; to avoid extreme propensities). See also [13] for the empirical evaluation of variants of such neural network approaches, combined with doubly robust learning.

 












Figure 15.8: CATE predictions of different meta-learners in the 401k example. Gradient boosted forests (via the xgboost library) were used as ML oracles for regression and classification. The CATE is pre-dicted on a grid of income points, corresponding to equally spaced income quantiles. All other covari-ates were imputed at their median values. For comparison, each plot also displays the doubly robust best linear predictor of the CATE with 5-95% confidence intervals on a sim-ple linear form of engineered fea-tures of the income.
 


 
15.2	Scoring for CATE Model Selection and Ensembling
The previous section gave an overview of how to qualitatively select among the different meta-learning strategies. Here we discuss how one can automate the process of selection using out-of-sample scoring and moreover how to potentially ensemble the models that come out of different estimation strategies into a single CATE model. In this section, we envision that the user has split their data into a training and scoring set and based on the training set they have fitted a set of candidate CATE
models 𝑇 := 𝜏1, . . . , 𝜏𝑀 . These models could correspond to the result of the different meta-learning strategies and with different regression style oracles. For instance, 𝜏1 could be the
result of an X-Learner with random forest regression oracles,
 


𝜏2 the result of an X-Learner with gradient boosted regression oracles, 𝜏3 the result of the DR-Learner with linear/logistic model oracles.
Let 𝑛 denote the size of the scoring set. Our goal is to be able to use the scoring set in order to evaluate which of these 𝑀 models is more accurate (with confidence) and to device approaches to select a single model 𝜏∗ that could either correspond to one of the models 𝑇 or to an ensemble of these models with weights
(𝑤1, . . . , 𝑤𝑀), such that 𝜏∗(𝑋) = '5:𝑀  𝑤𝑖𝜏𝑖(𝑋). Such a model 𝜏∗
should ideally be competing with the best model in 𝑇, i.e., with high probability:

E(𝜏 (𝑋) − 𝜏 (𝑋))2 ≤  𝑀	(𝜏 (𝑋) − 𝜏 (𝑋)) + 𝜖(𝑛, 𝑀)
 
0	min E	𝑗	0
𝑗=1
 

(15.2.1)
 
for some error function 𝜖 𝑛, 𝑀 that should decay fast to zero as a function of 𝑛 and should grow slowly with the number of candidate models 𝑀. We can use again the doubly robust outcome approach, viewing the problem as a regression prob-lem with the doubly robust proxy outcomes 𝑌𝑖 𝜂 as the labels and utilize techniques from model scoring, ensembling, model selection and stacking for regression problems.

Comparing Models with Confidence
We can use the doubly robust loss:
𝐿ˆ𝐷𝑅(𝜏; 𝜂ˆ) := 𝔼𝑛(𝑌(𝜂ˆ) − 𝜏(𝑋))2	(15.2.2)
as a quality score for each of the candidate models. Since we care about selecting among the models in 𝑇, we primarily care about choosing a score function that orders the models accurately. Hence, we primarily care that differences in the score between two models, i.e.:
𝛿ˆ𝑖,𝑗 (𝜂ˆ) = 𝐿ˆ𝐷𝑅(𝜏𝑖; 𝜂ˆ) − 𝐿ˆ𝐷𝑅(𝜏𝑗; 𝜂ˆ),	(15.2.3) approximate well differences in mean squared error, i.e.:
𝛿∗𝑖,𝑗 = E(𝜏𝑖(𝑋) − 𝜏0(𝑋))2 − E(𝜏𝑗(𝑋) − 𝜏0(𝑋))2	(15.2.4)
 


 
Consider the population analogues of the score and differences in the score, i.e.:
𝐿𝐷𝑅(𝜏; 𝜂) := E(𝑌(𝜂) − 𝜏(𝑋))2	(15.2.5)
𝛿𝑖,𝑗(𝜂) := 𝐿𝐷𝑅(𝜏𝑖; 𝜂) − 𝐿𝐷𝑅(𝜏𝑗; 𝜂)	(15.2.6)
Since E[𝑌(𝜂0) | 𝑋] = 𝜏0(𝑋), we have that:5
𝛿𝑖,𝑗(𝜂0) = 𝛿∗𝑖,𝑗	(15.2.7) Moreover, note that the population difference at the estimate 𝜂
satisfies (by simply expanding the squares):
 







5: Prove this as an exercise.
 
𝛿𝑖,𝑗(𝜂ˆ) = E[𝜏𝑖(𝑋)2	𝑗	2 − 2 𝑌(𝜂ˆ) (𝜏𝑖(𝑋) − 𝜏𝑗(𝑋))]
= E[𝜏𝑖(𝑋) − 𝜏𝑗(𝑋) − 2 E[𝑌(𝜂ˆ) | 𝑋] (𝜏𝑖(𝑋) − 𝜏𝑗(𝑋))]
 
Thus the error in the model comparison due to the error in the estimate 𝜂ˆ is:
𝛿𝑖,𝑗(𝜂ˆ) − 𝛿𝑖,𝑗(𝜂0) = 2 E[E[𝑌(𝜂0) − 𝑌(𝜂ˆ) | 𝑋] (𝜏𝑖(𝑋) − 𝜏𝑗(𝑋))]
We see that a good scoring rule, should be using proxy labels that have small bias, i.e.:
bias(𝑋; 𝜂ˆ) := E[𝑌(𝜂0) − 𝑌(𝜂ˆ) | 𝑋]	(15.2.8)
The doubly robust proxy labels exactly achieve this property. In particular, we can show based on results in prior sections:6
bias(𝑋; 𝜂ˆ) = (𝐻(𝜇0) − 𝐻(𝜇ˆ)) (𝑔0(𝐷, 𝑍) − 𝑔ˆ(𝐷, 𝑍))	(15.2.9)
Thus we derived that the error in the comparison between model 𝜏𝑖 and model 𝜏𝑗, due to the estimation error in 𝜂ˆ is:
2 E[(𝐻(𝜇0) − 𝐻(𝜇ˆ)) (𝑔0(𝐷, 𝑍) − 𝑔ˆ(𝐷, 𝑍)) (𝜏𝑖(𝑋) − 𝜏𝑗(𝑋))]
This has doubly robust properties, i.e. if either 𝐻 𝜇 is accurate or 𝑔 is accurate, then the comparison between the two models will be accurate. Moreover, the difference in scores also satisfies the Neyman orthogonality property:7
𝜕𝜂𝛿𝑖,𝑗(𝜂0) = 0	(15.2.10)
and since 𝛿ˆ𝑖,𝑗 𝜂 is the empirical analogue of 𝛿𝑖,𝑗 𝜂 , we can apply the general framework of Neyman orthogonality to deduce that the score difference estimate 𝛿ˆ𝑖,𝑗 𝜂 is root-𝑛 asymptotically
normal:8
 













6: Prove this as an exercise.












7: Prove this as an exercise.


8: A similar theorem also holds for the case of cross-fitted estimates. In practice, one can either use the nuisance estimates that were con-structed on the training set, which was also used to construct the functions 𝜏1, . . . , 𝜏𝑀 or perform cross-fitting within the scoring set.
 


Theorem 15.2.1 Let 𝜈𝑖𝑗 𝑋  = 𝜏𝑖 𝑋	𝜏𝑗 𝑋  and suppose that
𝔼𝑛 𝜈𝑖𝑗 𝑋 2	𝑐 for some constant 𝑐 > 0 and let 𝑛 grow to infinity.
As long as 𝜇 and 𝑔 are estimated on a separate sample and satisfy that:
√𝑛E[(𝐻(𝜇0) − 𝐻(𝜇ˆ)) (𝑔0(𝐷, 𝑍) − 𝑔ˆ(𝐷, 𝑍)) 𝜈𝑖𝑗(𝑋)] ≈ 0
and both nuisance functions are consistent, i.e.:
∥𝜇ˆ − 𝜇0∥𝐿2 + ∥ 𝑔ˆ − 𝑔0∥𝐿2 ≈ 0	(15.2.11)
Then the estimation error in the nuisance functions 𝜇, 𝜂, does not have a first order effect in the estimation error of the score difference between two models:
√𝑛(𝛿ˆ𝑖,𝑗 (𝜂ˆ) − 𝛿∗𝑖,𝑗) ≈ √𝑛𝔼𝑛(𝑌(𝜂0) − 𝜏𝑖(𝑋))2 − (𝑌(𝜂0) − 𝜏𝑗(𝑋))2
Consequently, the estimate 𝛿ˆ𝑖,𝑗 concentrates in a 1/√𝑛 neigborhood of 𝛿∗𝑖,𝑗 with deviations controlled by the Gaussian law:
√𝑛(𝛿ˆ𝑖,𝑗 (𝜂ˆ) − 𝛿∗	𝑎 𝑁(0, V)	(15.2.12)
𝑖,𝑗) ∼
where:
V := E ((𝑌(𝜂 ) − 𝜏 (𝑋))2 − (𝑌(𝜂 ) − 𝜏 (𝑋))2 − 𝛿∗ )2
Moreover, confidence intervals on the performance difference between two models can be constructed as:
P (𝛿∗𝑖,𝑗 ∈ J𝛿ˆ𝑖,𝑗 (𝜂ˆ) ± 𝑐JVˆ/𝑛l ≈ 1 − 𝛼	(15.2.13)
where 𝑐 is the (1−𝛼/2)-quantile of the standard normal distribution
Vˆ := 𝔼 ((𝑌(𝜂ˆ) − 𝜏 (𝑋))2 − (𝑌(𝜂ˆ) − 𝜏 (𝑋))2 − 𝛿ˆ	(𝜂ˆ))2
The above theorem can also directly be used to construct

Remark 15.2.1 (Sample-dependent base models) The assump-
tion that 𝔼𝑛 𝜈𝑖𝑗(𝑋)2 is some non-zero constant 𝑐 independent
of 𝑛 is required so that the variance 𝑉 is non-zero. In practice,
the candidate models will also be changing with 𝑛 as we will
be growing the sample size of the training set together with
the scoring set. As the size of the training set converges to
infinity it is highly probable that 𝑐 will be converging to zero,
 


in which case 𝑉 will also be converging to zero. The above theorem allows one to compare models that are distinct in their average predictions by at least some constant. If we want to be comparing models whose distinctness (i.e. E𝜈𝑖𝑗 𝑋 2) shrinks with the sample size, then we need to be more careful in the asymptotic normal approximation. In this case, it is more appropriate to consider the asymptotic properties of the self-normalized quanity:
( 𝑛 (𝛿ˆ𝑖,𝑗 (𝜂ˆ) − 𝛿∗ ),

where 𝑉𝑛 is now allowed to depend on 𝑛, since 𝜏𝑖 , 𝜏𝑗 are allowed to depend on 𝑛. In this case, to ignore the error due to 𝜂ˆ we would need that:
( 𝑛 E[(𝐻(𝜇0) − 𝐻(𝜇ˆ)) (𝑔0(𝐷, 𝑍) − 𝑔ˆ(𝐷, 𝑍)) 𝜈𝑖𝑗(𝑋)] ≈ 0 As we show in Appendix 15.A:
𝑉𝑛 ≥ 4E𝜈𝑖𝑗(𝑋)2 Var(𝑌(𝜂0) | 𝑋)
Thus if we assume that Var(𝑌(𝜂0) | 𝑋) ≥ 𝑐 > 0, then 𝑉𝑛 ≥
4𝑐∥𝜈𝑖𝑗 ∥22 and it suffices that:
√𝑛E J(𝐻(𝜇0) − 𝐻(𝜇ˆ)) (𝑔0(𝐷, 𝑍) − 𝑔ˆ(𝐷, 𝑍))  𝜈𝑖𝑗(𝑋) l ≈ 0

If 𝜈𝑖𝑗 𝑋   𝐶 𝜈𝑖𝑗 𝐿2 almost surely, then the above would hold under the standard condition that:
√𝑛∥𝐻(𝜇0) − 𝐻(𝜇ˆ)∥𝐿2 ∥𝑔0 − 𝑔ˆ ∥𝐿2 ≈ 0
Even when this condition does not hold, by an application of the Cauchy-Schwarz inequality it suffices that:
√𝑛JE [(𝐻(𝜇0) − 𝐻(𝜇ˆ))2 (𝑔0(𝐷, 𝑍) − 𝑔ˆ(𝐷, 𝑍))2] ≈ 0 which would hold if:
√𝑛∥𝐻(𝜇0) − 𝐻(𝜇ˆ)∥𝐿4 ∥𝑔0 − 𝑔ˆ ∥𝐿4 ≈ 0	(15.2.14)
Moreover, for the confidence interval to be valid, we would also need that:
|V − Vˆ |	0	(15.2.15)
Vˆ
 


 


Competing with the Best Model
The doubly robust loss can also be used for constructing an ensemble 𝜏∗ that competes with the best model in 𝑇. The simplest approach would be to choose the model with the best score, i.e.:
𝜏 = arg min 𝐿𝐷𝑅 𝜏; 𝜂	(15.2.17)
𝜏∈𝑇
 


Such a model satisfies the oracle performance guarantee in Equation (15.2.1) with (see e.g. [4])
 
𝜖(𝑛, 𝑀) ≲ (log(𝑀) + ∥𝐻(𝜇0) − 𝐻(𝜇ˆ)∥
 

𝐿2 ∥𝑔0 − 𝑔ˆ ∥
 


𝐿2
 
The leading term in this result is unfortunate, since it does not decay fast with the sample size 𝑛, i.e. as 1 𝑛. For instance, for parametric base models, we would expect the base models to have RMSE performance of ≲ 1 𝑛, in which case the above
1/√𝑛 rate becomes a dominant term.
One problem with this approach is the non-convexity of the space of models over which we are optimizing (i.e. optimizing over singleton models). This non-convexity can be alleviate by stacking approaches that convexify the optimization space over which we optimize and minimize the doubly robust loss over linear combinations of the base CATE models, i.e.:
 

𝜏∗ :=
 
I:𝑖=1
 

𝑤∗𝑖 𝜏𝑖 ,	𝑤∗
 

:= arg min 𝐿𝐷𝑅
𝑤∈𝑊
 

𝑀

𝑖=1
 
𝑤𝑖𝜏𝑖; 𝜂ˆ\
 

(15.2.18)
 
where 𝑊 could either be ℝ𝑀, in which case this is simply OLS regression with covariates 𝜏1 𝑋 , . . . , 𝜏𝑀 𝑋 and target outcome 𝑌 𝜂 ,9 or 𝑊 could be the 𝑀-dimensional simplex, i.e.
 



9: This is mathematically equiva-lent to the BLP approach we de-
 
𝑊 :=
 
f𝑤 ∈ ℝ𝑀
 
: 𝑤𝑖 ≥ 0,
 
I:𝑖=1
 
𝑤𝑖 = 1) ,
 
scribed in the first section of this
chapter, albeit using the predictions of the base CATE models as the en-
 
in which case this corresponds to a convex regression with the same covariates and outcome as in the OLS case. In the absence of further assumptions on the quality of the base models 𝜏𝑖, the above yield a model 𝜏∗ that satisfies the oracle performance guarantee in Equation (15.2.1) with (see e.g. [4, 14])
𝜖(𝑛, 𝑀) ≲ min { 𝑀 + ∥𝐻(𝜇0) − 𝐻(𝜇ˆ)∥𝐿4 ∥𝑔0 − 𝑔ˆ ∥𝐿4 ,
 
gineered features.
 
(log(𝑀) + ∥𝐻(𝜇0) − 𝐻(𝜇ˆ)∥
 
𝐿2 ∥𝑔0 − 𝑔ˆ ∥
 
𝐿2 o
 
The above approach yields a fast rate guarantee with respect to the sample size, but suffers from a large set of base models 𝑀. The reason being that the convexification of the optimization space introduced 𝑀 parameters that correspond to the weights for each model and no penalty to encourage sparsity of the solution.
One can achieve the ideal leading rate of log 𝑀 𝑛, that is both fast with respect to the sample size 𝑛 and grows only logarith-
 


mically with the number of base models 𝑀, by a penalized stacking approach called Q-aggregation [15], which penalizes different models based on their individual performance:
 

𝑤 = arg min 𝐿𝐷𝑅
𝑤∈𝑊
 

𝑀

𝑖=1
 
𝑤𝑖𝜏𝑖; 𝜂ˆ\
 
+ I:𝑖=1
 

𝑤𝑖 𝐿ˆ𝐷𝑅(𝜏𝑖; 𝜂ˆ)	(15.2.19)
 
where 𝑊 is the 𝑀-dimensional simplex. This is an 𝑀-dimensional convex optimization program that can be solved very fast with modern convex optimization solvers. The resulting ensemble model competes with the best model at the statistically optimal leading rate of (see [16]):
𝜖(𝑛, 𝑀) ≲ log(𝑀) + ∥𝐻(𝜇0) − 𝐻(𝜇ˆ)∥𝐿4 ∥𝑔0 − 𝑔ˆ ∥𝐿4


 


 


 








(a) DGP 1	(b) DGP 2	(c) DGP 3


Moreover, in Table 15.10, we depict the average performance of each meta-learning method and of three variants of the ensemble methods (based on Q-aggregation, convex regres-sion and simply choosing the single best score model) in terms of CATE RMSE over 100 experiments. We find that even though different learners are optimal in each of the DGPs, the ensemble learners are consistently close to the best performer across the board, while each of the other learners fails by a large margin in at least one DGP.

DGP 1	DGP 2	DGP 3
 
[0.016	0.012]
0.015 (0.036)
[0.012	0.009]
0.010 (0.030)
[0.002	0.007]
0.000 (0.020)
[0.038	0.005]
0.037 (0.047)
[0.010	0.008]
0.008 (0.028)
[0.013	0.008]
0.010 (0.030)
[0.017	0.014]
0.014 (0.038)
[0.019	0.013]
0.018 (0.037)
[0.017	0.015]
0.011 (0.042)
 
[0.193	0.036]
0.195 (0.243)
[0.193	0.037]
0.195 (0.243)
[0.374	0.080]
0.418 (0.421)
[0.235	0.007]
0.232 (0.245)
[0.235	0.007]
0.232 (0.245)
[0.156	0.053]
0.151 (0.243)
[0.165	0.049]
0.161 (0.243)
[0.163	0.042]
0.164 (0.236)
[0.171	0.055]
0.164 (0.269)
 
[0.049	0.011]
0.047 (0.070)
[0.178	0.034]
0.181 (0.230)
[0.374	0.079]
0.418 (0.421)
[0.036	0.011]
0.035 (0.056)
[0.223	0.006]
0.221 (0.235)
[0.155	0.045]
0.149 (0.232)
[0.037	0.012]
0.035 (0.056)
[0.038	0.011]
0.037 (0.056)
[0.037	0.013]
0.036 (0.061)
 













Figure 15.10: CATE RMSE perfor-mance of each of the meta-learning and ensemble methods in the three simple DGPs across 100 experi-ments. Each cell displays [mean standard deviation], median (95%) of the RMSE across the 100 experi-ments.
 

 
 











Figure 15.11: Score difference and confidence interval for each of the meta-learner models as compared to a constant treatment effect base-line. We find that among all models, only for the s-learner we can barely find that it has better CATE eval-uation accuracy as compared to a constant effect model with statisti-cal significance.



















Figure 15.12: CATE predictions of different stacked ensemble mod-els in the 401k example. Gradient boosted forests (via the xgboost li-brary) were used as ML oracles for regression and classification. The CATE is predicted on a grid of income points, corresponding to equally spaced income quantiles. All other covariates were imputed at their median values. For com-parison, each plot also displays the doubly robust best linear predictor of the CATE with 5-95% confidence intervals on a simple linear form of engineered features of the income.
 




















 



15.3	CATE Model Validation
Now that we have selected a winning CATE model or ensemble (e.g., the ensemble that comes out of Q-aggregation on the scor-ing data), we want to run formal statistical tests that validate whether the model that we chose contains any signal of treat-ment effect heterogeneity, or whether it is a confident model on average, or whether it is a useful model to derive personalized policy decisions as compared to simple benchmarks. We will refer to all these methodologies as CATE model validation and all the techniques can be thought as diagnostics that one should run on their CATE model before deployment or before using it to drive personalized decisions. As a side benefit, many of these diagnostics can also be used as a formal statistical test of the presence of treatment effect heterogeneity.
Throughout this section we assume that one has held out yet another dataset, called the test set (e.g., by splitting their data into train, validation and test) and that one has selected a CATE model 𝜏∗ without using the test set (e.g., by running some ensemble pipeline on the train and validation set).
 
Figure 15.13: Single binary regres-sion tree distillation of the Q-aggregation based stacked ensem-ble.
 


Heterogeneity Test Based on Doubly Robust BLP
If we calculate the doubly robust pseudo outcomes 𝑌𝐷𝑅 𝜂 on the test set (using cross-fitting within the test set to estimate the models 𝜂 or using the union of the training and scoring data to estimate 𝜂). Then we know that if the model of the CATE 𝜏 is good, the best linear predictor of the true CATE using 1, 𝜏 𝑋 as features should yield a statistically significant coefficient on the feature associated with the CATE model. In fact, in an ideal world this coefficient should be 1.
Thus we can run such a significance test to measure whether the CATE model 𝜏∗ has picked up any signal that is correlated with the true CATE. Note that if 𝜏0 𝑋 is the true CATE E 𝑌 1  𝑌 0
𝑋 , then the coefficient associated with 𝜏 in the OLS regression
𝑌 𝜂 1, 𝜏 𝑋 is converging in the population limit to the quantity:
𝛽1 := Cov(𝜏0(𝑋), 𝜏∗(𝑋)) = Cov(𝑌(1) − 𝑌(0), 𝜏∗(𝑋))	(15.3.1)
Var(𝜏∗(𝑋))	Var(𝜏∗(𝑋))
Thus, the statistical test of whether 𝛽1 is non-zero is a statistical test on the correlation of the individual treatment effect 𝑌 1
𝑌 0 and the learned model 𝜏 𝑋 . Note that if this test comes up
as significant, then this also implies that there exists treatment effect heterogeneity, as a function of the observed features 𝑋. Moreover, the theory from the first section of this chapter applies here to show that the statistical test based on OLS regression is a valid test, as long as the product of the regression and propensity estimation errors, converges faster than 𝑛−1/2.

 













Figure 15.14: OLS statistical test regression 𝑌𝐷𝑅 𝜂 on the features 1, 𝜏 𝑋 𝔼𝑛 𝜏 𝑋 in the 401(k) example. Standard Errors are het-eroscedasticity robust (HC1). 𝜏 cor-responds to the stacked ensemble based on Q-aggregation.

Validation Based on Calibration
A good CATE model should also be well calibrated. In the context of regression, a regression model 𝑔 𝑋 that predicts some outcome 𝑌, is calibrated if the expected value of the outcome, conditional on the model returning a value of 𝛾, should be equal to 𝛾, i.e.
E[𝑌 | 𝑔(𝑋) = 𝛾] = 𝛾.	(15.3.2)
Similarly, we can say that a CATE model 𝜏∗ is calibrated if the expected value of the treatment effect, conditional on the model returning a value of 𝑡, should be equal to 𝑡, i.e.
𝛾(𝜏∗, 𝑡) := E[𝑌(1) − 𝑌(0) | 𝜏∗(𝑋) = 𝑡] = 𝑡	(15.3.3)
Moreover, if we let P𝜏∗ denote the distribution of treatment effects returned by model 𝜏∗, then we can define average cali-bration scores across different values of 𝑡. Some popular mea-sures defined in the literature [17–19] are either the ℓ2 or the ℓ1-expected calibration error:
CAL1(𝜏∗) := ∫ |𝛾(𝜏∗, 𝑡) − 𝑡 | 𝑑P𝜏∗ (𝑡)	 (15.3.4) CAL2(𝜏∗) := ∫ (𝛾(𝜏∗, 𝑡) − 𝑡)2 𝑑P𝜏 (𝑡)	(15.3.5)
In fact, an interesting property of the ℓ2-calibration error, is that the MSE of a CATE model 𝜏∗ satisfies a calibration-distortion de-composition (analogous to the bias-variance decomposition):
∥𝜏∗ − 𝜏0∥𝐿2 = CAL2(𝜏∗) + DIS(𝜏∗)	(15.3.6)
 


where DIS 𝜏 = E Var 𝜏0 𝑋 𝜏 𝑋 . Thus any consistent 𝜏 model will eventually also be calibrated. However, calibration is a self-consistency guarantee that should be desireable for
many models and should not account for the majority of the MSE. Moreover, even if a model is far from 𝜏0, it is still desirable from a "steakholder experience" perspective that it should be calibrated.
The aforementioned desiderata can be taken to data by invoking again the proxy outcome regression approach. In particular, note that by the properties of the doubly robust proxy labels:
𝛾(𝜏∗, 𝑡) = E[𝑌(𝜂0) | 𝜏∗(𝑋) = 𝑡]	(15.3.7)
we can use observed data and out-of-sample estimates 𝜂 of the nuisance functions 𝜂0, to measure the calibration properties of a candidate CATE model.
To avoid having to run a non-parametric regression of 𝑌 𝜂0 on
𝜏 𝑋 , in order to estimate the function 𝛾 𝜏 , 𝑡 , a typical way that calibration is evaluated is by looking at quantile bins of the distribution of CATE. For instance, if we let 𝑞1 . . . 𝑞𝐾 denote a set of 𝐾 equally spaced quantiles of the distribution P𝜏∗ , then a well-calibrated model should satisfy that:
E[𝑌(𝜂0) | 𝜏∗(𝑋) ∈ [𝑞𝑡 , 𝑞𝑡+1]] = E[𝜏∗(𝑋) | 𝜏∗(𝑋) ∈ [𝑞𝑡 , 𝑞𝑡+1]]
In other words, consider any group 𝐺𝑡, defined by some quantile interval 𝑞𝑡 , 𝑞𝑡 1 of the predictions of the model 𝜏 . Then the group average treatment effect (GATE) for the group 𝐺𝑡, should be the same, whether we calculate it by using the doubly robust GATE, i.e., E[𝑌(𝜂0) | 𝑋 ∈ 𝐺𝑡] or whether we calculate it by using the average CATE value of the model 𝜏∗ within that group, i.e., E[𝜏∗(𝑋) | 𝑋 ∈ 𝐺𝑡].
We can now easily take the latter approach to data. For some small 𝐾 (e.g. 𝐾 = 4), we can consider a set of thresholds
𝑞1 ≤ . . . ≤ 𝑞𝐾+1 that roughly approximate equally spaced
quantiles of the CATE distribution P𝜏∗ and which are calculate without looking at the test sample (e.g. this can be calculated as the empirical quantiles of the empirical distribution of values of the model 𝜏∗ on the union of the training and scoring samples). These now define a set of 𝐾 groups, 𝐺1, . . . , 𝐺𝐾 as described in the previous paragraph. Subsequently, we can estimate the GATE for each group, using the doubly robust approach on the
 


 
test data, i.e.
𝜃ˆ𝐷𝑅 =	1
 

I:	𝑌𝑖(𝜂ˆ)	(15.3.8)

 
where 𝜂 is either estimated in a cross-fitting manner on the test set or using the union of training and scoring samples. Equiv-alently, we can simultaneously estimate all these parameters by running OLS of 𝑌 𝜂 on the one-hot-encodings of the group membership indicator functions, as in the first section of the chapter. Moreover, confidence intervals can be directly obtained for these values (e.g. based on the OLS heteroskedasticity robust confidence intervals or based on the simple formula for the standard error of an average of i.i.d. observations; in this case we have the average of the  𝑖   𝑛  : 𝑋𝑖   𝐺𝑘  observations
𝑌𝑖 𝜂 : 𝑖  𝑛 , 𝑋𝑖  𝐺𝑘 ). These confidence intervals can also be used to test whether these different groups have statistically signficant different average treatment effects, i.e. whether the groups are separated statistically.
Moreover, these estimates can then also be used to construct approximate analogues of the ℓ2 and ℓ2-average calibration scores. For each group 𝐺𝑘, we can also calculate the average value of the model 𝜏∗, i.e.,
1
𝜃ˆ𝑘∗ = |{𝑖 ∈ [𝑛] : 𝑋𝑖 ∈ 𝐺𝑘 }|	I:	𝜏∗(𝑋𝑖)	(15.3.9)
Ideally, if the model was reasonable, 𝜃ˆ𝑘∗ should be very close to 𝜃ˆ𝐷𝑅. The average difference can be considered as a quality metric of 𝜏∗, i.e.,
C]AL1(𝜏∗) := I: 1𝜃ˆ𝐷𝑅 − 𝜃ˆ𝑘∗ 1 · |{𝑖 ∈ [𝑛] : 𝑋𝑖 ∈ 𝐺𝑘 }| (15.3.10)
 

C]AL
 
:	I:
 
𝑘=1
( ˆ𝐷𝑅
 
ˆ∗ )2	:
 

(15.3.11)
 

These can be viewed as binning approximations to the ℓ1- and ℓ2-average calibration scores (the first one was recommended as a calibration score in the context of randomized trials by [20]).

 


 
defined by quartiles of the CATE distribution of the ensemble
𝜏∗ constructed based on Q-aggregation stacking. The bottom group corresponds to the bottom 25% of predicted CATEs, the next group to the 25%-50% of predicted CATEs, etc. In Figure 15.15, we depict on the x-axis the average CATEs, as calculated based on 𝜏∗, within each group and on the y-axis and the doubly robust estimate and 5-95% confidence interval for the GATE as calculated based on the doubly robust proxy labels 𝑌(𝜂ˆ) on the test set.










includegraphics[]401k-calibration-score.pdf
 












Figure 15.15: Calibration check for chosen ensemble model 𝜏 in the 401(k) example. Test samples are splitted in four groups based on CATE predictions and CATE quan-tiles (e.g. bottom group contains samples whose CATE predictions lie in the bottoms 25% of predic-tions). The x-axis depicts the aver-age predicted CATE within each group based on 𝜏 , while the y-axis depicts the GATE as calculated based on the doubly robust pseudo-outcomes calculated on the test set.
 

Interpretation via Distillation and Group Differences. We can also try to interpret what are the differences of characteris-tics between the top and bottom CATE groups; if we find that they have statistically significantly different GATEs. We can do that by either reporting the mean values of the covariates in the two groups or building some interpretable classification model that distinguishes between the two groups.

 
group1
 
group2
 
group1 - group2
 
	mean ± s.e.	mean ± s.e.	mean ± s.e.	
age	40.01 ± 0.27	42.56 ± 0.45	-2.56 ± 0.72
inc	26898 ± 346	65771 ± 760	-38873 ± 1106
fsize	2.82 ± 0.04	3.12 ± 0.07	-0.30 ± 0.11
educ	12.77 ± 0.07	14.74 ± 0.11	-1.97 ± 0.18
db	0.24 ± 0.01	0.37 ± 0.02	-0.14 ± 0.03
marr	0.52 ± 0.01	0.84 ± 0.02	-0.32 ± 0.03
male	0.22 ± 0.01	0.20 ± 0.02	0.02 ± 0.03
twoearn	0.29 ± 0.01	0.66 ± 0.02	-0.37 ± 0.03
pira	0.16 ± 0.01	0.42 ± 0.02	-0.26 ± 0.03
nohs	0.16 ± 0.01	0.02 ± 0.01	0.13 ± 0.02
hs	0.41 ± 0.01	0.26 ± 0.02	0.15 ± 0.03
smcol	0.24 ± 0.01	0.26 ± 0.02	-0.02 ± 0.03
col	0.19 ± 0.01	0.46 ± 0.02	-0.27 ± 0.03
 hown	0.57 ± 0.01	0.84 ± 0.02	-0.27 ± 0.03	
 












Figure 15.16: Group differences be-tween the top 25% predicted CATE group (group2) and the bottom 75% predicted CATE group (group1) in the 401k example.
 






















Figure 15.17: Decision tree that dis-tills the main differences between group1 and group2 as defined in Figure 15.16.

Validation Based on Uplift Curves
Another way that we can judge the quality of a CATE model is by testing its ability to help us prioritize or stratify which part of the population we should be treating. In Section ??, we studied the value of the optimal policy subject to treating exactly a 𝑞-fraction of the overall population. In a sense, how much this value varies or how different it is from the ATE is a measure of the amount of uplift offered by personalizing optimally based on 𝑋 using the true CATE 𝜏0. We can study the same question from the lens of 𝜏∗, which can give a lesser or equal level of uplift, the higher the uplift the better the model. Unlike Section ??, where 𝜏0 was a nuisance to be estimated and plugged into the optimal constrained or unconstrained policy, here 𝜏∗ is fixed and given (as it is based on a separate data set). In particular, our evaluation procedures would work even if
𝜏0 were hard to learn or had discontinuities in its distribution, since we focus on a fixed 𝜏∗ instead.
Let 𝜇 𝜏 , 𝑞 denote an estimate based on non-test data of the
1  𝑞 quantile of the distribution P𝜏 of CATEs produced by the model 𝜏 . Then the group 𝑋 : 𝜏 𝑋  𝜇 𝜏 , 𝑞  is fixed in terms of the test data. The corresponding GATE is
GATE(𝑞) := E[𝑌(1) − 𝑌(0) | 𝜏∗(𝑋) ≥ 𝜇(𝜏∗, 𝑞)]	(15.3.12)
 


 
The improvement in the average effect of the treated, induced by the prioritization rule based on 𝜏∗, as compared to treating a random 𝑞 fraction of the population, would be:
TOC(𝑞) := GATE(𝑞) − ATE	(15.3.13)
and the improvement in the total effect would be:
QINI(𝑞) := TOC(𝑞) P(𝜏∗(𝑋) ≥ 𝜇(𝜏∗, 𝑞)).	(15.3.14)
For any fixed CATE model 𝜏 , the function TOC 𝜏 , is referred to in the literature as the Treatment Operating Characteristic curve, while the function QINI 𝜏 , is referred to as the QINI curve (analogous to the Gini curve for classification models).10
These curves also have interesting interpretations as covariances of the individual treatment effect 𝑌(1) − 𝑌(0) with non-linear functions of the CATE model 𝜏∗ (see proofs in Appendix 15.B):
TOC	Cov	1	0	1{𝜏∗(𝑋) ≥ 𝜇(𝜏∗, 𝑞)}
P(𝜏∗(𝑋) ≥ 𝜇(𝜏∗, 𝑞))
QINI(𝑞) = Cov (𝑌(1) − 𝑌(0), 1{𝜏∗(𝑋) ≥ 𝜇(𝜏∗, 𝑞)})
Since the second term in each covariance is a function of 𝑋 alone and E 𝑌 1  𝑌 0  𝑋 = E 𝑌 𝜂0  𝑋 , these quantities are identified by replacing the individual effects with the doubly
robust pseudo-outcomes:
TOC	Cov	1{𝜏∗(𝑋) ≥ 𝜇(𝜏∗, 𝑞)}
P(𝜏∗(𝑋) ≥ 𝜇(𝜏∗, 𝑞))
QINI(𝑞) = Cov (𝑌(𝜂0), 1{𝜏∗(𝑋) ≥ 𝜇(𝜏∗, 𝑞)})

Area Under the Curve (AUC) Viewing the above two quanti-ties as functions of the target fraction 𝑞, we can calculate the areas under these two curves as scalar measures of quality of the model 𝜏∗ in its ability to correctly target sub-parts of the population at different levels of treatment population size targets, i.e.
 















10: These terminologies primarily stem from the uplift modelling lit-erature in Computer Science [21–23].
 

𝐴𝑈𝑇𝑂𝐶 :=
𝐴𝑈𝑄𝐶 :=
 
∫ 1 TOC	(15.3.15)
∫ 1 QINI	(15.3.16)
 
The larger the Area Under the Curve, the better the CATE model is at treatment prioritization or stratification.
 


Moreover, these measures are signals of treatment effect hetero-geneity. If any of the two measures are statistically non-zero, then treatment effect heterogeneity was detected with statistical significance. In fact, if we detect that any of these curves lies above zero at any point 𝑞, with statistical significance, then that also serves as a test for treatment effect heterogeneity. For this reason, we will now develop confidence intervals and simul-taneous confidence bands for these two curves, when they are estimated from samples.


Estimation and inference.	To estimate the TOC and the QINI curves, we will use the doubly robust proxy outcome approach.
 


We will train nuisance models 𝜂 without using the test sample and then construct estimates of the TOC and QINI curves as:
T-OC(𝑞) = Cov𝑛 (𝑌(𝜂ˆ), 1{𝜏∗(𝑋) ≥ 𝜇(𝜏∗, 𝑞)}	(15.3.17)
Q-INI(𝑞) = Cov𝑛 (𝑌(𝜂ˆ), 1{𝜏∗(𝑋) ≥ 𝜇(𝜏∗, 𝑞)})	(15.3.18)
where 𝜋 𝑞 = 𝔼𝑛1 𝜏 𝑋   𝜇 𝜏 , 𝑞  and we used the short-hand notation:
Cov𝑛(𝐴, 𝐵) = 𝔼𝑛 [(𝐴 − 𝔼𝑛(𝐴)) (𝐵 − 𝔼𝑛(𝐵))]
Both of these estimates are of the general estimation form that can be handled by the Neyman orthogonality framework. For each 𝑞, we can view each of the estimates as an estimate of the form:
𝜃ˆ(𝑞; 𝜈) = 𝔼𝑛[𝜓(𝑊; 𝜈)]
for some appropriate defined function 𝜓 and with 𝜈 being a vector of nuisance quantities, which contain 𝜂, 𝜋 and 𝜃0 = E 𝑌 𝜂0  and which satisfies Neyman orthogonality with respect
to all of these nuisance quantities. Thus these estimates will be asymptotically Gaussian with the effect of the nuisances being negligible. Moreover, even if we evaluate these curves at many points, as long as the number of points 𝑞 that we use does not grow exponentially with the sample size, then these estimates will be jointly Gaussian and we can construct simultaneous confidence bands as in Section 4.4.

 


 
Analogous theorem also applies to the QINI curve estimates.
This result can be used to construct simultaneous confidence bands for the value of the TOC curve at many quantiles 𝑞 as described in Remark 4.4.1. We can consider the estimate of the variance:
 
𝑉ˆℓ 𝑘 = 𝔼𝑛𝜓ˆℓ (𝑊)𝜓ˆ 𝑘(𝑊)
 
𝜓ˆ (𝑊) = (𝑌(𝜂ˆ) − 𝜃ˆ) (  𝕀(𝑞ℓ )
 
− 1 − 𝛼ˆℓ
 
and construct a confidence band at confidence level 𝛼:
 
𝐶𝑅 =
 
𝑝
×ℓ =1[
 
𝛼ˆℓ ± 𝑐J𝑉ˆℓℓ /𝑛]
 
where 𝑐 is the 1 − 𝛼 quantile of the distribution of ∥𝑍∥∞ for a
random variable 𝑍 ∼ 𝑁(0, 𝐷ˆ −1/2𝑉ˆ 𝐷ˆ −1/2), where 𝐷ˆ = diag(𝑉ˆ )
is the matrix with diagonal entries 𝑉ˆℓℓ and zero off-diagonal
entries.















Figure 15.18: Point estimates and uniform confidence band of the TOC curve for the Q-aggregation ensemble 𝜏∗ in the 401(k) example.

Note that if there is any point that is above the zero line, with
 


confidence, in this curve, then the CATE model 𝜏 has identified heterogeneity in the effect in a statistically significant manner. For such a test we can calculate a one-sided confidence interval, as we only care that the quantities are larger than some value with high confidence. Using the Gaussian approximation, a one-sided confidence band, at confidence level 𝛼, can be calculated as:
 
𝐶𝑅 = ×𝑝
 
J𝛼ˆℓ − 𝑐J𝑉ˆℓℓ /𝑛, ∞ 
 
where 𝑐 is the 1	𝛼 2 quantile of the distribution of 𝑍	for a random variable 𝑍 as defined in the previous paragraph.









Figure 15.19: Point estimates and one-sided uniform confidence band of the TOC curve for the Q-aggregation ensemble 𝜏 in the 401(k) example. The heterogeneity statistic depicted in the title corre-sponds to the largest lower bound of the confidence band across all quantile points and is a statistical signal for the presence treatment effect heterogeneity.

We can also calculate the area under the curve using the discrete difference approximation:
𝑝
𝐴𝑈\𝑇𝑂𝐶 =	T-OC 𝑞ℓ	𝑞ℓ 1	𝑞ℓ	(15.3.19)
ℓ =1
Note that since this is a linear combination of the estimates at each 𝑞ℓ , under the assumptions of Theorem 15.3.1, the estimate of the area under the curve will be asympotically normal and centered around the quantity:
𝑝
𝐴𝑈𝑇𝑂𝐶 =	TOC 𝑞ℓ	𝑞ℓ 1	𝑞ℓ
ℓ =1
 


and we can calculate a one-sided confidence interval as:
𝐴𝑈𝑇𝑂𝐶 ∈ J𝐴𝑈\𝑇𝑂𝐶 − J𝑉ˆ /𝑛, ∞ 
where the estimate of the variance is:
𝑝
𝑉ˆ = 𝔼𝑛𝜓ˆ(𝑊 )2	𝜓ˆ(𝑊 ) = I: 𝜓ˆℓ (𝑊) (𝑞ℓ+1 − 𝑞ℓ )

 
If the confidence interval does not contain zero, then we have again detected heterogeneity.

AUTOC		s.e.	One-Sided 95% CI 5228.9137	1471.8731		[2807.8980, Infty]
 



Figure 15.20: AUTOC point esti-mate and one-sided confidence in-terval for the Q-aggregation ensem-ble 𝜏∗ in the 401(k) example.
 

The exact same analysis can be conducted for the QINI curve, constructing doubly robust point estimates and a simultaneous one-sided confidence band, as well as a one-sided confidence interval for the discretized quantile approximation of the AUQC, i.e.
𝑝
𝐴𝑈𝑄𝐶 =	QINI 𝑞ℓ	𝑞ℓ 1	𝑞ℓ
ℓ =1










Figure 15.21: Point estimates and one-sided uniform confidence band of the QINI curve for the Q-aggregation ensemble 𝜏 in the 401(k) example. The heterogeneity statistic depicted in the title corre-sponds to the largest lower bound of the confidence band across all quantile points and is a statistical signal for the presence treatment effect heterogeneity.

 

 
AUQC		s.e.	One-Sided 95% CI 1542.4581	385.2292		[908.8125, Infty]
 
Figure 15.22: AUQC point estimate and one-sided confidence interval for the Q-aggregation ensemble 𝜏 in the 401(k) example.
 


 
In Section ?? we studied evaluation of personalized policies, in particular optimal ones. However, we did not delve into the learning of optimal policies, just as we discussed inference on CATE in Chapter 14 but did not delve into learning it using flexible non-parametric methods. While any CATE model learned as in the present chapter can be used to prioritize treatment, a CATE model would only be a means to an end and not the object of interest itself, which may be learned more directly. The primary object of interest is a good personalized treatment policy 𝜋 that given any instance of the variable 𝑋 returns a treatment assignment 𝜋(𝑋) ∈ {0, 1}.
Note that learning a good policy is an inherently different statistical task than learning a good CATE model. For a good unconstrained policy, it suffices that we learn whether the CATE
𝜏 𝑋 = E 𝑌 1  𝑌 0  𝑋 is positive or negative. As seen in Section ??, the optimal policy is given by looking at the sign of CATE: 𝜋∗ 𝑋 = 𝟙 𝜏0 𝑋  0 . Thus policy learning is more akin to a classification problem that tries to predict the sign of
the CATE as opposed to a regression problem that tries to learn the magnitude of CATE too. Of course, mistakes in predicting the sign are more detrimental when the magnitude of the CATE is larger and therefore should be weighed differently. Thus policy learning is more accurately described as a classification
 


problem with sample dependent mis-classification costs, known in the machine learning literature as cost-sensitive classification.
Recall from Section ?? that we define the gains of policy over no treatment as 𝑉 𝜋 = E 𝜋 𝑋 𝑌 1   1  𝜋 𝑋  𝑌 0   E 𝑌 0  = E 𝜋 𝑋 𝑌 𝜂0  (Eq. (??)). Optimizing 𝑉 𝜋 over 𝜋 is equivalent
to a sample-weighted classification problem, where the goal of 𝜋 is to match the sign of 𝑌 𝜂0 , with sample weights 𝑌 𝜂0 . More formally, note that:
argmax𝜋𝑉(𝜋) = argmax𝜋E [(2𝜋(𝑋) − 1) 𝑌(𝜂0)] and we can simplify the latter as:
E [(2𝜋(𝑋) − 1) 𝑌(𝜂0)] = E [(2𝜋(𝑋) − 1) sign (𝑌(𝜂0)) |𝑌(𝜂0)|]
= E [𝟙 {2𝜋(𝑋) − 1 = sign (𝑌(𝜂0))} |𝑌(𝜂0)|]
Thus we can treat the sign of 𝑌 𝜂0 as the "label" of the sample in a classification problem and 𝑌 𝜂0 as the weight of the sample, and our centered treatment policy 2𝜋 𝑍  1 is trying to predict the label. We can therefore invoke any machine learning classification approach in a meta-learning manner, so as to solve this weighted classification problem. One popular approach is to use a decision tree classifier, since it will lead to an interpretable policy that is easy to visualize.
In finite samples, we would also need to construct estimates 𝜂 of the nuisance parameters 𝜂0 in a cross-fitting manner using arbitrary ML regression methods, as discussed in prior sections and then solve a sample weighted classification problem with samples	𝑋𝑖 , sign 𝑌𝑖 𝜂  , 𝑊𝑖 = 𝑌𝑖 𝜂	𝑖=1. The results in [25]
show that the regret of the returned policy 𝜋, as compared to
the optimal policy within some policy space Π, i.e.:
𝑅(𝜋ˆ) = max 𝑉(𝜋∗) − 𝑉(𝜋ˆ)	(15.3.20)
inherit the double robustness property and decay at the order of
≈ (𝑉∗ 𝑉𝐶(Π) + ∥𝐻(𝜇ˆ) − 𝐻(𝜇0))∥𝐿2 ∥ 𝑔ˆ − 𝑔0∥𝐿2

where 𝑉𝐶 Π is a measure of statistical complexity of the policy space Π (e.g. a small constant for shallow binary decision trees) and 𝑉∗ is a constant that in many practical scenarios can be thought as some constant multiple of the variance of the value
 


 






 



of the optimal policy in the class 𝜋∗ = argmax𝜋∈Π𝑉(𝜋), i.e.
𝑉∗ ≈ Var(𝜋∗(𝑋)𝑌(𝜂0))
See also [4, 26] for generalizations and variations of this result.
 
Figure 15.23: The details that are displayed on each node are also useful in understanding the group average treatment effect for each node. In particular, the information ‘samples=N‘, gives us the size of each node 𝑁, and the information ‘value=[A, B]‘, then ‘A‘ is the sum of the 𝑌 𝜂  for the samples where
𝑌 𝜂 < 0 and similarly, ‘B‘ is the
sum of 𝑌 𝜂  for the samples where
𝑌 𝜂 > 0. Thus to get the GATE for each node, we simply do ‘(B-A)/N‘, which would correspond to
1
𝑁
bly robust estimate of the GATE for
the node.
 

 

 


 
15.4	Empirical Example: The "Welfare" Experiment

 
We revisit the welfare experiment dataset that we analyzed in Chapter 14 and deploy all the methods described in this section. We remind that this dataset corresponds to an experiment that was run as part of the General Social Survey (GSS)11 , where some respondents received a questionnaire about their willingness to support a “Welfare Program” (which will be viewed as the treatment, i.e. 𝐷 = 1, in our analysis), while
others received the same questionnaire but the program was
referred to as “Assistance to the Poor”(which will be viewed as the control, i.e. 𝐷 = 0, in our analysis).
After some preprocessing, the dataset contains 12907 individu-
 




11: See	e.g.	https:
//gssdataexplorer.norc. org/variables/vfilter for a full description of the variables in the survey.
 


 
als and 42 covariates. Instead of simply estimating the projection of the CATE onto a simple model that is linear in the political views variable or its one-hot-encoding, we instead train generic ML models based on all the methods outlined in this chapter. We then score each of the models and construct an ensemble CATE model using Q-aggregation.
In Figure 15.24 we depict the predictions of the Q-aggregation ensemble, as a function of the political views variable, fixing all other covariates to their median values. We find that the fully data-driven model did pick up political views as a relevant variable, but the degree of variation is much smaller than the one that is identified using the doubly robust method for the projection of the CATE on political views. Potentially, this demonstrates that other variables are also relevant and some of the variation picked up by the CATE projection models should have been attributed to other covariates that covary with political views.





 






















Figure 15.24: CATE predictions of the Q-aggregation stacked ensem-ble. Gradient boosted forests (via the xgboost library) were used as ML oracles for regression and clas-sification. The CATE is predicted on a grid of income points, cor-responding to equally spaced in-come quantiles. All other covari-ates were imputed at their median values. For comparison, each plot also displays the doubly robust best linear predictor of the CATE with 5-95% confidence intervals as a linear function of the covariate ‘polviews‘ and as a linear function of the one-hot-encoding of the co-variate ‘polviews‘.
 

To understand the heterogeneity patterns that were identified by the ensemble CATE model, we fit a single shallow binary decision tree to the predictions of the CATE ensemble model. We depict the learned tree in Figure 15.25. We see that political views is indeed the single most important factor that discriminates the predictions of the learned model, however we also see that the ensemble model also learned that education and race also creates heterogeneity in the reaction to programs labeled as “welfare” (as opposed to “assistance to the poor”). In particular, the model identified that people with more left-wing political views and more than 15 years of education (i.e., 4-year college educated individuals) have the least adverse reaction to the word “welfare”, while more right wing individuals who did not
 


identify as black (i.e., race2=0) have the most adverse reaction to the term “welfare”. Moreover, we see that political views alone does not create a large variation, but it is the combination of political views and college education that creates the largest degree of heterogeneity in the effect.












Figure 15.25: Single binary regres-sion tree distillation of the Q-aggregation based stacked ensem-ble.

An alternative way to visualize the importance of the different variables in changing the output of the CATE ensemble is by using the SHAP values. In Figure 15.26. These values identify how each individual variable contributes to changes in the output of the ensemble model. We again identify that political views and education create the largest variation in the output, though here we see that other variables can also be attributed changes in the prediction, such as the number of hours worked last week (hrs1). We see here that having worked less hours last week increases the output of the model, i.e., leads to less adverse reaction to the word “welfare”. So people that worked more hours were less eager to contribute to a program termed “welfare”.
 





























Figure 15.26: SHAP values for the Q-aggreagation based stacked en-semble in the welfare experiment dataset.

 
We can also validate the learned model by running several statistical tests on a held-out sample. For instance, in Figure 15.27 we run an OLS regression of the doubly robust outcome 𝑌 𝜂 on 1, 𝜏 𝑋 . We find that the coefficient associated with the stacked ensemble was statistically significant and the confidence interval included the value 1. Hence, this validates that the model carries significant information on the heterogeneity of the effect.
			coef	std err	P> |z|	[0.025	 0.975]  const	-0.3839		0.016		0.000	 -0.416	-0.352
 𝜏∗(𝑋)	1.4655	0.267	0.000	0.943	1.988 
 










Figure 15.27: OLS statistical test re-gression 𝑌 𝜂 on 1, 𝜏 𝑋 in the Criteo example. Standard Errors are heteroscedasticity robust (HC1).
𝜏 corresponds to the stacked en-semble based on Q-aggregation.
 

We also evaluate how calibrated the model is by depicting the group average treatment effects for each quartile of the predicted CATE distribution. The GATE was estimated using the doubly robust approach on the held-out sample. We see that the bottom and top quartiles are separated in a statistically
 


significant manner, while also the calibration score of the model is quite high (0.4461).






Figure 15.28: Calibration check for chosen ensemble model 𝜏 in the welfare example. Test samples are splitted in four groups based on CATE predictions and CATE quan-tiles (e.g. bottom group contains samples whose CATE predictions lie in the bottoms 25% of predic-tions). The x-axis depicts the aver-age predicted CATE within each group based on 𝜏 , while the y-axis depicts the GATE as calculated based on the doubly robust pseudo-outcomes calculated on the test set.

Given that we identified that the bottom and top quartile are different in a statistically significant manner, we can also visu-alize the differences of these two groups, by simply depicting the difference in means of each of the covariates in the two groups. We see for instance, that hours worked last week was significantly different in the two groups, as well as income, age, political views and education, reinforcing our prior findings.

 
 
group1
 
group2
 
group1 - group2
 
	mean ± s.e.	mean ± s.e.	mean ± s.e.	
hrs1	44.16 ± 0.30	36.74 ± 0.58	7.42 ± 0.88
income	11.52 ± 0.03	10.61 ± 0.10	0.92 ± 0.12
rincome	10.58 ± 0.05	9.23 ± 0.14	1.35 ± 0.19
age	41.75 ± 0.27	37.52 ± 0.49	4.23 ± 0.76
polviews	4.39 ± 0.03	3.07 ± 0.05	1.32 ± 0.08
educ	13.58 ± 0.06	15.41 ± 0.11	-1.82 ± 0.17
earnrs	1.81 ± 0.02	1.60 ± 0.03	0.21 ± 0.05
sibs	3.40 ± 0.06	3.42 ± 0.14	-0.02 ± 0.20
childs	1.68 ± 0.03	1.24 ± 0.06	0.44 ± 0.09
 occ80	351.52 ± 5.66	280.24 ± 8.53	71.28 ± 14.19	
 








Figure 15.29: Group differences between the top 25% predicted CATE group (group2) and the bot-tom 25% predicted CATE group (group1) in the welfare example.
 

We can also visualize the differences between the two groups by fitting a shallow binary classification tree to predict membership in the top quartile vs bottom quartile groups. We see again that political views, education and race are the most important distinguishing factors for membership in the two groups. For
 


instance, as we see in Figure 15.30, in the held-out dataset, among the 309 college-educated and left-wing individuals, only 5 were in the bottom quartile group (which had a statistically significant more adverse reaction to welfare), while 304 were in the top quartile group. Similarly, among the 1622 right-wing and not black individuals, 1541 were in the bottom quartile group vs. 81 in the top quartile group.














Figure 15.30: Decision tree that dis-tills the main differences between group1 and group2 as defined in Figure 15.29.

Finally, we can verify that we detected a statistically significant heterogeneity of effect by looking at the uplift curves, i.e. the TOC (c.f. Figure 15.31) and QINI (c.f. Figure 15.32) curves. We find that both curves lie above the zero line, even when we incorporate one-sided confidence bands. The largest lower point of this confidence band is depicted as a heterogeneity statistic in the title. For instance, we see that the largest lower point is 0.1039 in the TOC curve, which occurs at roughly 5%. This means that, with 95% confidence level, if we look at the group that corresponds to the top 5% of the CATE predictions, then we expect to see an average effect within that group that is at least 0.1039 larger than the average effect in the overall population.
 



















Figure 15.31: TOC curve with a 95% one-sided confidence band for the welfare experiment dataset.

Similarly, in the QINI curve we find that this heterogeneity statistic is 0.0164 and occurs at roughly 30%, which means that if we were to treat the group of people that corresponds to the top 30% of CATE predictions, then we would expect to get a total effect in the population that is 0.0164 larger than if we were to treat a random 30% fraction of the population.

















Figure 15.32: QINI curve with a 95% one-sided confidence band for the welfare experiment dataset.

We can also calculate the area under these curves and the confidence interval for that area. If the confidence interval does not contain zero, then we have again detected heterogeneity with statistical significance.
 


AUTOC	s.e.	One-Sided 95% CI	
0.0667	0.0128	[0.0457, Infty]	


AUT Qini	s.e.	One-Sided 95% CI
0.0232	0.0046	[0.0156, Infty]

15.5		Empirical Example: Digital Advertising A/B Test
We now revisit the example introduced in Section ?? and ap-ply the CATE estimation pipeline outlined in this section. In Figure 15.33 we depict the performance of each of the meta-learners, compared to the performance of a baseline model that fits a constant treatment effect, as measured by the doubly robust score (see Theorem 15.2.1). Note that since here we are in a randomized trial, the propensity is known and hence the rate requirements in that theorem are satisfied. We see that all meta-learners perform better than a constant effect, indicating statistically significant heterogeneity. Moreover, we see that all learners except the S-learner have comparable performance.














Figure 15.33: Performance (and 95% confidence intervals) of meta-learner models in the Criteo exam-ple compared to a constant effect model, as measured by the Doubly Robust score.

We also compared the meta-learning approach to the Best-Linear-Predictor approach presented in the prior chapter on heterogeneous treatment effects. Instead of learning a CATE model using all the features, we fitted the best linear CATE when
 


using only feature ‘f3‘ or a second degree polynomial of that feature. We find that this BLP approach in this setting is quite un-stable due to poor extrapolation behavior. In particular, the feature ‘f3‘ has very heavy negative tails (see Figure 15.35). The different parametric models overfit the parametric curve to the region of high density and extrapolate very poorly in the heavy negative tail. On the contrary the Q-aggregation ensemble of the meta-learning models is more stable and regularizes appropriately in this regime.








Figure 15.34: Predictions of the Q-aggregation stacked ensemble and of the Doubly Robust BLP of CATE as a linear or quadratic function of feature ‘f3‘ in Criteo example.





1e7
5



4



3



2



1


 
0
8	6	4	2	0	2	4
f3
 

Figure 15.35: Histogram of distri-bution of feature ‘f3‘
 

We then validate the Q-aggregation ensemble using all the validation methods presented in this chapter. In Table 15.36 we run an OLS regression of the doubly robust pseudo-outcome on the CATE predictions and an intercept. We find that the coefficient of the CATE predictor is very accurately estimated to be 1.
 


 
			coef	std err	P> |z|	[0.025	0.975]  const	0.0074		0.000		0.000		0.007		0.008
 𝜏∗(𝑋)	1.0096	0.036	0.000	0.940	1.079 
 
Figure 15.36: OLS statistical test re-gression 𝑌 𝜂 on 1, 𝜏 𝑋 in the Criteo example. Standard Errors are heteroscedasticity robust (HC1).
𝜏 corresponds to the stacked en-semble based on Q-aggregation.
 

We also see that the predictions of the CATE model are very well calibrated and the doubly robust GATE estimates for each quartile of CATE prediction groups lies almost on the 45 degree line, with a calibration score that is very close to 1.






Figure 15.37: Calibration check for chosen ensemble model 𝜏 in the welfare example. Test samples are splitted in four groups based on CATE predictions and CATE quan-tiles (e.g. bottom group contains samples whose CATE predictions lie in the bottoms 25% of predic-tions). The x-axis depicts the aver-age predicted CATE within each group based on 𝜏 , while the y-axis depicts the GATE as calculated based on the doubly robust pseudo-outcomes calculated on the test set.

The TOC and Qini Curves are depicted in Figure 15.38 and Figure 15.39, with one-sided 95% uniform confidence bands. We see that there is statistically significant heterogeneity as these curves lie well above the zero line. For instance, the TOC curve tells us that if we treat roughly the group that corresponds to roughly the top 5% of CATE predictions then we should expect that the average treatment effect of that group to be approximately 0.07 larger than the average treatment effect. Moreover, the Qini curve tells us that if we treat approximately the group that corresponds to the top 15% of CATE predictions, then we should expect the total effect of such a treatment policy to be 0.005 larger than the total effect if we were to treat a random 15% subset of the population. Thus our CATE model carries significant information that is valuable for better ad targeting.
 



















Figure 15.38: TOC curve with a 95% one-sided confidence band for the digital advertising dataset.


















Figure 15.39: QINI curve with a 95% one-sided confidence band for the digital advertising dataset.

15.A		Appendix: Lower Bound on Variance in Model Comparison
First we observe that:
Δ𝑖,𝑗 := (𝑌(𝜂0) − 𝜏𝑖(𝑋))2 − (𝑌(𝜂0) − 𝜏𝑗(𝑋))2
= 𝜏𝑖(𝑋)2 − 𝜏𝑗(𝑋)2 − 2𝑌(𝜂0)(𝜏𝑖(𝑋) − 𝜏𝑗(𝑋))
= (𝜏𝑖(𝑋) − 𝜏𝑗(𝑋)) (𝜏𝑖(𝑋) + 𝜏𝑗(𝑋) − 2𝑌(𝜂0))
 


 
and note that:


Moreover,
 


V𝑛 = EΔ2
 

− (EΔ𝑖,𝑗 )2
 
2
𝑖,𝑗
 
= E [(𝜏𝑖(𝑋) − 𝜏𝑗(𝑋))2 E [(𝜏𝑖(𝑋) + 𝜏𝑗(𝑋) − 2𝑌(𝜂0))2 | 𝑋]]
 
By a variance decomposition argument and since 𝜏0(𝑋) =
E(𝑌(𝜂0) | 𝑋):
E	𝜏𝑖 𝑋	𝜏𝑗 𝑋	2𝑌 𝜂0 2	𝑋
= 4 Var(𝑌(𝜂0) | 𝑋) + E  (𝜏𝑖(𝑋) + 𝜏𝑗(𝑋) − 2𝜏0(𝑋))2 | 𝑋
Thus we have derived that:

 
2
𝑖,𝑗
 
= 4E  (𝜏𝑖(𝑋) − 𝜏𝑗(𝑋))2 Var(𝑌(𝜂0) | 𝑋)
+ E [(𝜏𝑖(𝑋) − 𝜏𝑗(𝑋))2 (𝜏𝑖(𝑋) + 𝜏𝑗(𝑋) − 2𝜏0(𝑋))2 ]
 
Moreover, note that by Jensen’s inequality:
(EΔ𝑖,𝑗 )2 = (E(𝜏𝑖(𝑋) − 𝜏𝑗(𝑋)) (𝜏𝑖(𝑋) + 𝜏𝑗(𝑋) − 2𝜏0(𝑋)))2
≤ E(𝜏𝑖(𝑋) − 𝜏𝑗(𝑋))2 (𝜏𝑖(𝑋) + 𝜏𝑗(𝑋) − 2𝜏0(𝑋))2
Thus we can conclude that:
V𝑛 ≥ 4E [(𝜏𝑖(𝑋) − 𝜏𝑗(𝑋))2 Var(𝑌(𝜂0) | 𝑋)]	(15.A.1)

15.B	Appendix: Interpretation of Uplift curves
We derive first the covariance interpretation of the TOC uplift curve.
TOC(𝑞) = E[𝑌(1) − 𝑌(0) | 𝜏ˆ(𝑋) ≥ 𝜇(𝑞)] − E[𝑌(1) − 𝑌(0)]
= E J(𝑌(1) − 𝑌(0)) 1{𝜏ˆ(𝑋) ≥ 𝜇(𝑞)} l − E[𝑌(1) − 𝑌(0)]
Let 𝐴 = 𝑌(1) − 𝑌(0) and 𝐵 = 1{𝜏ˆ(𝑋)≥𝜇(𝑞)} and note that E[𝐵] = 1.
 
Thus we have:
 
P(𝜏ˆ(𝑋)≥𝜇(𝑞))
 
TOC(𝑞) = E [𝐴 𝐵] − E[𝐴] = E [𝐴 𝐵] − E[𝐴] E[𝐵] = Cov(𝐴, 𝐵)
Next, we derive the covariance interpretation of the QINI uplift curve. Let 𝐴 = 𝑌(1) − 𝑌(0) and 𝐵 = 𝜏ˆ(𝑍) ≥ 𝜇(𝑞). Then by the
 


definition of the QINI curve:
𝜏QINI(𝑞) := 𝜏(𝑞) P(𝜏ˆ(𝑍) ≥ 𝜇ˆ(𝑞))
= (E[𝐴 | 𝐵] − E[𝐴]) P(𝐵)
= (E[𝐴𝟙{𝐵}] − E[𝐴]  P(𝐵)
= E[𝐴𝟙{𝐵}] − E[𝐴]E[𝟙{𝐵}] = Cov (𝐴, 𝟙{𝐵})
 


Bibliography



[1]		Arthur Conan Doyle. The sign of four. Spencer Blackett, 1890 (cited on page 400).
[2]		Denis Nekipelov, Vira Semenova, and Vasilis Syrgkanis. ‘Regularised orthogonal machine learning for nonlinear semiparametric models’. In: The Econometrics Journal 25.1 (2022), pp. 233–255 (cited on page 404).
[3]	Miruna Oprescu, Vasilis Syrgkanis, and Zhiwei Steven Wu. ‘Orthogonal random forest for causal inference’. In: International Conference on Machine Learning. PMLR. 2019,
pp. 4932–4941 (cited on page 404).
[4]	Dylan J Foster and Vasilis Syrgkanis. ‘Orthogonal sta-tistical learning’. In: The Annals of Statistics 51.3 (2023),
pp. 879–908 (cited on pages 404, 406, 426, 446).
[5]	Edward H Kennedy. ‘Towards optimal doubly robust estimation of heterogeneous causal effects’. In: arXiv preprint arXiv:2004.14497 (2020) (cited on page 404).
[6]		Greg Lewis and Vasilis Syrgkanis. ‘Double/debiased machine learning for dynamic treatment effects via g-estimation’. In: arXiv preprint arXiv:2002.07285 (2020) (cited on pages 404, 406).
[7]		Xinkun Nie and Stefan Wager. ‘Quasi-oracle estimation of heterogeneous treatment effects’. In: Biometrika 108.2 (2021), pp. 299–319 (cited on page 406).
[8]		Vasilis Syrgkanis, Victor Lei, Miruna Oprescu, Maggie Hei, Keith Battocchi, and Greg Lewis. ‘Machine learn-ing estimation of heterogeneous treatment effects with instruments’. In: Advances in Neural Information Processing Systems 32 (2019) (cited on page 413).
[9]	Scott M Lundberg and Su-In Lee. ‘A Unified Approach to Interpreting Model Predictions’. In: Advances in Neu-ral Information Processing Systems 30. Ed. by I. Guyon,
U. V. Luxburg, S. Bengio, H. Wallach, R. Fergus, S. Vish-wanathan, and R. Garnett. Curran Associates, Inc., 2017,
pp. 4765–4774 (cited on page 414).
[10]	Christoph Molnar. Interpretable machine learning. Lulu. com, 2020 (cited on page 414).
 


[11]	Uri Shalit, Fredrik D Johansson, and David Sontag. ‘Esti-mating individual treatment effect: generalization bounds and algorithms’. In: International conference on machine learning. PMLR. 2017, pp. 3076–3085 (cited on pages 417,
418).
[12]	Fredrik Johansson, Uri Shalit, and David Sontag. ‘Learn-ing representations for counterfactual inference’. In: In-ternational conference on machine learning. PMLR. 2016,
pp. 3020–3029 (cited on pages 417, 418).
[13]		Alicia Curth and Mihaela van der Schaar. ‘On inductive biases for heterogeneous treatment effect estimation’. In: Advances in Neural Information Processing Systems 34 (2021),
pp. 15883–15894 (cited on page 418).
[14]	Kevin Wu Han and Han Wu. ‘Ensemble Method for Estimating Individualized Treatment Effects’. In: arXiv preprint arXiv:2202.12445 (2022) (cited on page 426).
[15]		Guillaume Lecué and Philippe Rigollet. ‘Optimal learning with Q-aggregation’. In: The Annals of Statistics 42.1 (2014),
pp. 211–224 (cited on page 427).
[16]		Hui Lan and Vasilis Syrgkanis. ‘Causal Q-Aggregation for CATE Model Selection’. In: arXiv preprint arXiv:2310.16945 (2023) (cited on page 427).
[17]		Lars van der Laan, Ernesto Ulloa-Pérez, Marco Carone, and Alex Luedtke. ‘Causal isotonic calibration for hetero-geneous treatment effects’. In: arXiv preprint arXiv:2302.14011 (2023) (cited on page 433).
[18]	Chirag Gupta, Aleksandr Podkopaev, and Aaditya Ram-das. ‘Distribution-free binary classification: prediction sets, confidence intervals and calibration’. In: Advances in Neural Information Processing Systems 33 (2020), pp. 3711–
3723 (cited on page 433).
[19]		Chirag Gupta and Aaditya Ramdas. ‘Distribution-free calibration guarantees for histogram binning without sample splitting’. In: International Conference on Machine Learning. PMLR. 2021, pp. 3942–3952 (cited on page 433).
[20]	Raaz Dwivedi, Yan Shuo Tan, Briton Park, Mian Wei, Kevin Horgan, David Madigan, and Bin Yu. ‘Stable discov-ery of interpretable subgroups via calibration in causal studies’. In: International Statistical Review 88 (2020), S135–S178 (cited on page 435).
[21]		Nicholas Radcliffe. ‘Using control groups to target on predicted lift: Building and assessing uplift model’. In: Direct Marketing Analytics Journal (2007), pp. 14–21 (cited on page 438).
 


[22]	Nicholas J Radcliffe and Patrick D Surry. ‘Real-world uplift modelling with significance-based uplift trees’. In: White Paper TR-2011-1, Stochastic Solutions (2011), pp. 1–33 (cited on page 438).
[23]		Patrick D Surry and Nicholas J Radcliffe. ‘Quality mea-sures for uplift models’. In: submitted to KDD2011 (2011) (cited on page 438).
[24]	Steve Yadlowsky, Scott Fleming, Nigam Shah, Emma Brunskill, and Stefan Wager. ‘Evaluating treatment pri-oritization rules via rank-weighted average treatment effects’. In: arXiv preprint arXiv:2111.07966 (2021) (cited on page 444).
[25]	Susan Athey and Stefan Wager. ‘Policy learning with observational data’. In: Econometrica 89.1 (2021), pp. 133–
161 (cited on page 445).
[26]		Victor Chernozhukov, Mert Demirer, Greg Lewis, and Vasilis Syrgkanis. ‘Semi-parametric efficient policy learn-ing with continuous actions’. In: Advances in Neural Infor-mation Processing Systems 32 (2019) (cited on pages 446,
447).
[27]		John C Duchi, Peter W Glynn, and Hongseok Namkoong. ‘Statistics of robust optimization: A generalized empir-ical likelihood approach’. In: Mathematics of Operations Research 46.3 (2021), pp. 946–969 (cited on page 447).
[28]		Ying Jin, Zhimei Ren, Zhuoran Yang, and Zhaoran Wang. ‘Policy learning" without”overlap: Pessimism and gener-alized empirical Bernstein’s inequality’. In: arXiv preprint arXiv:2212.09900 (2022) (cited on page 447).
[29]		Dylan J Foster and Alexander Rakhlin. ‘Foundations of Reinforcement Learning and Interactive Decision Mak-ing’. In: arXiv preprint arXiv:2312.16730 (2023) (cited on page 447).
 

Difference-in-Differences	16



 
"Corpus omne perseverare in statu suo quiescendi vel movendi uniformiter in directum, nisi quatenus a viribus impressis cogitur statum illum mutare." ("An object remains in its state of rest or of moving uniformly in a straight direction, unless forced to change that state by impressed forces.")
– Isaac Newton [1].










Here we discuss debiased machine learning (DML) methods for performing inference on average causal effects in panel (or longitudinal) or repeated cross-section data in the difference-in-differences (DiD) framework. We present and discuss the key identifying assumption for the average treatment effect on the treated based on DiD – the so-called "parallel trends" assump-tion – allowing for high-dimensional observed confounding variables. This assumption suggests a natural estimation strat-egy that directly applies DML to estimate average treatment effects on the treated using differenced outcomes.
 
16.1	Introduction	464
16.2	The   Basic   Difference-in-Differences Framework: Paral-lel Worlds 		464
The Mariel Boatlift    468
16.3	DML	and Conditional Difference-in-Differences 469 Comparison to Adding Re-gression Controls	471
16.4	Example:  Minimum Wage		471
16.5	Notes	476
16.6	Notebooks	476
16.7	Exercises	477
16.A Conditional Difference-in-Differences with Repeated Cross-Sections	478
 
16.1	Introduction
 

We now consider estimation of causal effects in panel (longitu-dinal) data where we observe individual units in multiple time periods or repeated cross-section data. While there are many po-tential approaches for analyzing data with both a cross-sectional and temporal component, we specifically look at difference-in-differences (DiD) and closely related approaches.
DiD and related methods are widely used in empirical work in the social sciences and in policy analysis. The basic DiD structure relies on having two groups of observations – a treatment group and a control group – for two time periods – a pre-treatment and a post-treatment period. Canonical DiD analysis then proceeds by comparing changes in the average pre- and post-treatment outcomes in the treatment group to changes in the average pre- and post-treatment outcomes in the control group. Attaching a causal interpretation to this comparison relies on an assumption that imposes that changes in the treatment group in the absence of treatment would have been the same as changes in the control group. This assumption captures the intuition that the treatment group would have evolved along the same path as the control group in the absence of treatment – i.e., the two groups share "parallel trends." Under the parallel trends assumption, the difference between the treatment and control differences between the pre- and post-treatment averages identifies the average treatment effect on the treated (ATET).
In this chapter, we review the basic DiD framework. We then focus on DiD in a setting where a researcher wishes to impose conditional parallel trends. That is, we consider settings where there are observed variables that are thought to be related to the evolution of the outcome of interest such that parallel trends holds only after conditioning on these variables. After suitably defining the conditional parallel trends assumption, we illustrate that the DML approach to estimating ATET from Chapter 9 can be readily applied within the DiD context.

16.2	The Basic Difference-in-Differences Framework: Parallel Worlds
The basic DiD structure has many appealing features. It is intuitive. It allows for essentially unrestricted differences in baseline outcomes for the treatment and control groups and
 

 
Figure 16.1: DiD is perhaps the oldest quasi-experimental research design. John Snow was a London doctor and is often considered the father of modern epidemiology.
[2]	is essentially an effort to provide convincing evidence that water is the causal agent for cholera transmission. It presents and discusses multiple pieces of evidence – including a DiD.
Source: https://www.micropia. nl/en/discover/microbiology/
john-snow/, accessed 6/7/23.
 











 

𝑌𝑡(𝑑)
where 𝑑  0, 1 denotes the treatment state in period 𝑡 = 2. For example, 𝑌1 1 denotes the period one outcome under
treatment – that is, the outcome in the period before treatment is received – and 𝑌2 1 denotes the period two outcome under treatment. Let 𝐷  0, 1 be the treatment group indicator with 𝐷 = 1 indicating that treatment is received at 𝑡 = 2 and
𝐷 = 0 indicating no treatment in either time period. Observed
outcomes in period 𝑡 may then be represented as 𝑌𝑡 = 𝐷𝑌𝑡 1
1  𝐷 𝑌𝑡 0 . As in other causal inference contexts, we are left
with missing data as we are unable to observe observations simultaneously in the treatment and control state.
DiD proceeds under the following key assumption:
 
four	potential	outcomes
𝑌𝑡 0, 0 , 𝑌𝑡 0, 1 , 𝑌𝑡 1, 0 , 𝑌𝑡 1, 1 for each time period. The DiD struc-ture imposes that 𝑌𝑡 1, 0 , 𝑌𝑡 1, 1 can never be observed so it is impossible to learn about the effects of treatment paths that have treatment occur at t = 1. We choose the simpler representation with a single argument in the potential outcomes for notational clarity. Keeping explicit track of potential outcomes for different treatment paths is important in more complicated settings with more potential treatment paths as may arise with many time periods or more complex treatment variables.
 
 

 
Condition (16.2.1) is the parallel trends assumption. It requires that, in expectation, the change in control potential outcomes among the treatment group is the same as the change in the control potential outcomes among the control group. Con-dition (16.2.2) imposes that receipt of treatment at 𝑡  = 2
does not impact average period 1 potential outcomes. Here,
we are effectively ruling out anticipation effects. Importantly, (16.2.2) allows for systematic differences between average po-tential outcomes among treated and control observations in the pre-treatment period. That is, it does not impose that E 𝑌1 0   𝐷 = 1  = E 𝑌1 0   𝐷 = 0 . Thus, we can accom-
modate, for example, scenarios where we believe that period
two treatment assignment is related to period one outcomes.
 







Often, the no anticipation assump-tion is left implicit or ignored. We state it for clarity and because it al-lows clean definition of the causal effect of interest.
 


 
It is worth explicitly noting that the parallel trends assumption is typically functional form dependent. That is, if E 𝑌2 0  𝑌1 0
𝐷 = 1 = E 𝑌2 0  𝑌1 0  𝐷 = 0 , it will generally not be the case that E 𝑔 𝑌2 0  𝑔 𝑌1 0  𝐷 = 1 = E 𝑔 𝑌2 0  𝑔 𝑌1 0
𝐷 = 0 . For example, suppose the outcome of interest is wages. Parallel trends holding for wage does not imply that parallel
trends holds for log(wage), and the DiD estimator based on log(wage) need not recover a causal effect. Intuitively, this functional form dependence arises because parallel trends relies on latent sources of confounding being additively separable so that they are eliminated by the differencing operation.
It is straightforward to verify that ATET is identified under Assumption 16.2.1. Note that the right-hand-side of Eq. (16.2.1) is an observable quantity while the left-hand-side corresponds to the unobservable change in the control potential outcomes of treated units. Parallel trends allows us to impute this latent change from the observed change in the control units. Effec-tively, we are assuming that the treated observations would have changed in the same way as the control observations in the absence of treatment. Similarly, the right-hand-side of Eq. (16.2.2) is an observable quantity while the left-hand-side is the unobserved average of control potential outcomes in period one for the treated group. Eq. (16.2.2) allows us to impute this baseline average from the observed baseline average in the treat-ment group. We can then reconstruct the counterfactual average of the control potential outcome in the post-treatment period by adjusting this baseline average by the observed change in average outcomes between the two periods in the control group. Figure 16.2 presents a graphical illustration of the identification argument.
More formally, we can put this together to write the ATET as
𝛼 = E[𝑌2(1) − 𝑌2(0) | 𝐷 = 1]
= E[𝑌2(1) | 𝐷 = 1] − E[𝑌2(0) | 𝐷 = 1]
= E[𝑌2(1) | 𝐷 = 1]	(16.2.3)
− (E[𝑌1(1) | 𝐷 = 1] + E[𝑌2(0) − 𝑌1(0) | 𝐷 = 0])
= E[𝑌2(1) − 𝑌1(1) | 𝐷 = 1] − E[𝑌2(0) − 𝑌1(0) | 𝐷 = 0]
where the third equality follows from a direct application of Assumption 16.2.1. The expression in the last line is exactly the difference between the difference between post- and pre-treatment period average outcomes in the treatment group and the difference between post- and pre-treatment period average outcomes in the control group – hence, difference-in-differences.
 





One situation where parallel trends hold regardless of the outcome transformation is when 𝐷 is randomly assigned (i.e., when
𝑌1 0 , 𝑌1 1 , 𝑌2 0 , 𝑌2 1   𝐷). See
[3]	for further discussion.
A related framework to DiD that is independent to monotone transfor-mations (at the cost of other restric-tions, of course) is the changes-in-changes model of [4].





















One could identify the ATE by aug-menting Assumption 16.2.1 with E 𝑌2 1  𝑌1 1  𝐷 = 0 = E 𝑌2 1
𝑌1 1  𝐷 = 1 and E 𝑌1 0  𝐷 =
0 = E 𝑌1 1  𝐷 = 0 . Recall that we are notation subsumes treat-
ment paths so that 𝑌2 1 denotes the potential outcome in the second time period of a unit under control in period 1 and under treatment in period 2 while 𝑌1 1 denotes the first time period of unit under con-trol in period 1 and under treatment in period 2. The first condition is thus not just a restriction on evo-lution of potential outcomes in a specific treatment state but also a re-striction on treatment effects them-selves which seems hard to moti-vate in realistic settings. As such, we follow the majority of the DiD literature in focusing on estimation of ATET.
 







 














Estimation of the ATET in canonical DiD in a finite sample is straightforward by considering four group means:
𝔼𝑛[𝑌1(𝐷 = 𝑑, 𝑡 = 𝑠)]
𝔼𝑛[1(𝐷 = 𝑑, 𝑡 = 𝑠)]
Defining the estimator of the ATET as 𝛼, we have
𝛼 = (𝜃ˆ2(1) − 𝜃ˆ1(1)) − (𝜃ˆ2(0) − 𝜃ˆ1(0)).	(16.2.4)
Asymptotic properties under independence follow in a fashion similar to difference-in-mean estimators for the ATE outlined in Chapter ??.
We can also obtain a numerically equivalent estimator of the ATET via regression. Specifically, the ordinary least squares estimator of the parameter 𝛼 in the linear model
𝑌 = 𝛽0 + 𝛽1𝐷 + 𝛽2𝑃 + 𝛼𝐷𝑃 + 𝑈,	(16.2.5)
where 𝑃 is a binary variable with 𝑃 = 1 indicating the post-treatment time period (t = 2), is numerically equivalent to
in (16.2.4). The regression formulation is especially conve-
 
Figure 16.2: DiD Identification. This figure illustrates identification of the ATET in the canonical DiD framework. Objects represented in black are observable. Objects in blue are unobserved and identi-fied via Assumption 16.2.1. Visu-ally we impute the unobserved E 𝑌2 0  𝐷 = 1 by extrapolating
from the observed E 𝑌1 1  𝐷 = 1
using the observed "trend" between
E 𝑌1 0   𝐷 = 0 and E 𝑌2 0
𝐷 = 0 . The ATET is then the difference between the observed E[𝑌2(1) | 𝐷 = 1] and the imputed E[𝑌2(0) | 𝐷 = 1].
 
𝛼
nient for obtaining standard errors under different dependence assumptions.
 


Table 16.1: DiD Estimation of the Effect of the Mariel Boatlift on Un-employment









The Mariel Boatlift
Card’s analysis of the impact of the Mariel Boatlift on the Miami labor market, [5], provides a prototypical application of DiD. For example, it is the example of DiD in Angrist and Krueger’s Handbook of Labor Economics chapter on empirical methods [6]. The basic idea of the study was to use the Mariel Boatlift – a sudden and arguably unexpected inflow of immigrants that increased the Miami labor force by about 7% between May and September of 1980 – to understand the impact of immigration on low-skilled labor market outcomes.
A key component of the analysis was arguing that Atlanta, Los Angeles, Houston, and Tampa-St. Petersburg provide valid control cities in the sense that we might plausibly believe that the change in labor market outcomes in these cities from the late 1970’s to the early 1980’s is useful for inferring how the Miami labor market would have changed in the absence of the Mariel immigration. Part of the argument in [5] relies on evidence that the cities had similar characteristics in the pre-treatment period. Effectively, this argument relies on parallel trends holding conditional on these pre-treatment characteristics. We consider using DML to flexibly control for rich covariates in Section 16.3.
We illustrate canonical DiD in the Mariel Boatlift example in Table 16.1 which uses numbers taken from Table 4 in [5]. Here, we see the DiD estimate of the ATET on unemployment is -1.1 with standard error 1.5, which does not provide strong evidence of a large impact of the Mariel immigration on unemployment.
 

16.3	DML and Conditional Difference-in-Differences
In many empirical applications, researchers deviate from the canonical DiD framework by including additional control vari-ables. The fundamental motivation is similar to that for includ-ing control variables in other causal contexts, e.g., as motivated in Chapter 5: it is easier to believe that parallel trends holds among units that are identical in terms of observed charac-teristics. In this section, we explore flexibly including control variables in a DiD framework leveraging DML methods.
We restate the canonical DiD assumptions so that they hold after conditioning on pre-treatment/strictly exogenous charac-teristics 𝑋.

The intuition for (16.3.1) and (16.3.2) is essentially identical to the intuition for Assumption 16.2.1 discussed in the previous section. The only difference is that these conditions are now imposed within observationally identical groups as defined by 𝑋. Condition (16.3.3) is a standard overlap condition for identifying ATET which essentially imposes that there are control observations available for every value of 𝑋. Under Assumption 16.3.1, it is straightforward to verify that the ATET is identified by repeating the argument in (16.2.3) conditional on 𝑋 and averaging over the distribution of 𝑋 in the 𝐷 = 1
group.	We leave verification of identifica-
tion of the ATET in the conditional
 
Similar to estimating parameters in the partially linear model or average treatment effects under confounding as discussed in Chapter 9, obtaining estimates of the ATET in the conditional
 
DiD framework as an exercise.
 


 
DiD setting will require estimating high-dimensional nuisance objects. We thus exploit DML methods to accommodate the use of flexible methods in estimating these objects.
A key input into DML estimation is a Neyman orthogonal score. In the conditional DiD framework with panel data,1
𝜓(𝑊; 𝛼, 𝜂) =  𝐷 − 𝑚(𝑋) (Δ𝑌 − 𝑔(0, 𝑋)) − 𝐷 𝛼	(16.3.4)
 





1: We provide the Neyman orthog-onal score and discuss DML estima-tion with repeated cross-sections in
 
𝑝(1 − 𝑚(𝑋))
 
𝑝	Section 16.A.
 
provides an orthogonal score for the ATET, 𝛼, where 𝑊 =
𝑌1, 𝑌2, 𝐷, 𝑋 denotes the observable variables; Δ𝑌 = 𝑌2	𝑌1;
𝜂 = 𝑝, 𝑚, 𝑔 denotes nuisance parameters with true values
𝑝0 = E 𝐷 , 𝑚0 𝑋  = E 𝐷	𝑋 , and 𝑔0 0, 𝑋  = E Δ𝑌	𝐷 =
0, 𝑋 . See also [7], [8], [9]. Comparing to the score for the ATET
provided in Chapter 9, we see that the score function in (16.3.4) is identical to that for learning the ATET under conditional ignorability where the outcome variable is simply defined as Δ𝑌.
Given the Neyman orthogonal score (16.3.4), it is then straight-forward to implement DML to estimate the ATET. √𝑛-asymptotic
normality of 𝛼, the DML estimator of the ATET, follows from
Theorem 9.4.1 in Chapter 9.


















-
 


𝔼𝑛[𝜓ˆ(𝑊𝑖; 𝛼)] = 0 which yields
1 '5:𝑛	  𝐷𝑖 −𝑚ˆ [𝑘(𝑖)](𝑋𝑖 )	 (Δ𝑌𝑖 − 𝑔ˆ 𝑘 𝑖 (0, 𝑋𝑖))
 𝑛	𝑖=1 𝑝ˆ[𝑘(𝑖)](1−𝑚ˆ [𝑘(𝑖)](𝑋𝑖))	[ ( )]	
𝛼 =	.
-	 1 '5:𝑛	 𝐷𝑖 
𝑛	𝑖=1 𝑝ˆ[𝑘(𝑖)]
3. Let
 𝜓(𝑊𝑖; 𝛼ˆ) 
𝜑ˆ (𝑊𝑖) =  1 ˆ 𝑛	 𝐷	.
𝑛 '5:𝑖=1 𝑝ˆ 𝑖 
[𝑘(𝑖)]
Construct standard errors via
JVˆ/𝑛,	Vˆ = 𝔼𝑛[𝜑ˆ (𝑊𝑖)2]
and use standard normal critical values for inference.

 
Comparison to Adding Regression Controls
The equivalence between the ATET estimator obtained by di-rectly looking to the difference between the treatment and control differences in means and the ordinary least squares estimator of the coefficient 𝛼 in the linear model (16.2.5) in the canonical DiD setting suggests a simple approach to incorpo-rating control variables by augmenting the regression model to include controls linearly. That is, add 𝛽′𝑋 to the model in (16.2.5). However, the coefficient on the 𝐷𝑃-interaction term is not equivalent to the ATET and need not uncover any sensible causal effect without very strong functional form restrictions and restrictions on treatment effect heterogeneity. See, e.g., [10] for further discussion. In contrast, the DML estimator always targets the ATET under Assumption 16.3.1 and is relatively simple to implement.


16.4	Example: Minimum Wage
In this section, we use DML for DiD to estimate the effect of minimum wage increases on teen employment. We use data from and roughly follow the approach of [11]. The data are annual county level data from the United States covering 2001 to 2007. The outcome variable is log county-level teen employment, and the treatment variable is an indicator for whether the county has a minimum wage above the federal minimum wage.2 Note
 




















The Notebooks 16.6.1 contain the code for the minimum wage exam-ple.










2: The federal minimum wage over 2001-2007 was constant at $5.15.
 


 
that this definition of the treatment variable makes the analysis straightforward but ignores the nuances of the exact value of the minimum wage in each county and how far those values are from the federal minimum.3 The data also includes county population and county average annual pay. We follow [11] by removing observations with missing entries which produces a balanced panel with data from counties in 42 states. See [11], [12], and [13] for further details regarding the data.
We focus our analysis exclusively on the set of counties that had wage increases away from the federal minimum wage in 2004. That is, we treat 2003 and earlier as the pre-treatment period and the period 2004-2007 as the post-treatment period. We assume that parallel trends holds after conditioning on three pre-treatment variables – 2001 population, 2001 average pay, and 2001 teen employment – and the region to which each county belongs.4
We estimate dynamic effects by estimating the ATET in 2004-2007 which provide estimates of the effect in the year of treat-ment and one, two, and three years after the treatment. For control observations, we use the set of observations that still have minimum wage equal to the federal minimum in each year – the "as-yet not treated" – so the control group changes from period to period. For example, we use all observations that had minimum wage equal to the federal minimum in 2004 as control observations when estimating the ATET in 2004, but we use all observations that had minimum wage equal to the federal minimum in 2005 as control observations to estimate the 2005 ATET. These definitions yield 102 treatment observations for estimating each ATET and 2389, 2327, 2080, and 1417 control
observations for 2004, 2005, 2006, and 2007 respectively.
Since our goal is to estimate the ATET of the county level mini-mum wage being larger than the federal minimum imposing that parallel trends holds after flexibly controlling for region and our pre-treatment variables, we employ DML using the algo-rithm from Section 16.3. We consider using an array of methods for learning the nuisance functions including several of the mod-ern regression methods that we discussed in previous chapters. Specifically, we consider ten candidate learners for the high-dimensional nuisance functions 𝑔0 0, 𝑋 = E Δ𝑌  𝐷 = 0, 𝑋
and 𝑚0 𝑋  = E 𝐷  𝑋 . We consider using no control variables
(No Controls) which corresponds to maintaining unconditional
parallel trends. We consider linear index models using only the raw control variables (Basic) – the four region dummies and log of 2001 population, log of 2001 average pay, and log of 2001 em-ployment – and using a full cubic expansion of the raw control
 




3: Under these definitions, this ex-ample is an example of staggered adoption. Staggered adoption refers to a setting with a binary, absorb-ing treatment variable. That is, once an observation becomes treated it remains treated thereafter. This setting is straightforward to ana-lyze as treatment paths are com-pletely characterized by the treat-ment date and controls can be con-structed from observations that are not treated during the sample pe-riod (the never treated) or observa-tions that are not treated prior to the treatment date and remain un-treated in the period in which one wants to estimate the ATET (the as-yet not treated).
4: We follow [11] and categorize each observation as belonging to one of four U.S. census regions.
 


Table 16.2: RMSE for Learners in Minimum Wage example


Expansion	0.1887	0.2122	0.2445	0.2710
Lasso (CV)	0.1631	0.1851	0.2193	0.2214
Ridge (CV)	0.1631	0.1851	0.2191	0.2213
Random Forest	0.1716	0.1982	0.2330	0.2388
Deep Tree	0.1922	0.2250	0.2599	0.2708
Shallow Tree	0.1678	0.1924	0.2279	0.2290
Tree (CV)	0.1633	0.1889	0.2178	0.2227

No Controls
Basic	0.1983
0.1986	0.2006
0.2009	0.2111
0.2113	0.2503
0.2217
Expansion	0.1988	0.2007	0.2113	0.2217
Lasso (CV)	0.1968	0.1986	0.2083	0.2197
Ridge (CV)	0.1971	0.1989	0.2086	0.2198
Random Forest	0.2005	0.2051	0.2128	0.2355
Deep Tree	0.2207	0.2364	0.2303	0.2744
Shallow Tree	0.1921	0.1944	0.2029	0.2301
Tree (CV)	0.1937	0.1955	0.2039	0.2311








 
variables including all third order interactions (Expansion).5 We consider Lasso and Ridge with the cubic expansion of the raw variables and penalty parameter chosen by cross-validation (Lasso (CV) and Ridge (CV)). We consider a random forest with no randomization over input variables and 1000 trees (Random Forest). Additionally, we consider three different tree models: a tree with depth 15 (Deep Tree), a tree with depth 3 (Shallow Tree), and a tree tuned using cross-validation (Tree (CV)). For random forest and the tree models, we use region, log of 2001 population, log of 2001 average pay, and log of 2001 employment as input variables. Finally, we consider estimation using the learner for E Δ𝑌  𝐷 = 0, 𝑋 and for E 𝐷  𝑋 that produce the
lowest RMSE during cross-fitting (Best) allowing for a different
learner to be selected for each task.6
We start by reporting the RMSE obtained during cross-fitting for each learner in each period in Table 16.2. Here we see that the Deep Tree systematically performs substantially worse in terms of cross-fit predictions than the other learners for both tasks and that Expansion performs similarly poorly for the outcome prediction. It also appears there is some signal in the regressors,
 
5: We use a linear model estimated by OLS for 𝑔0 0, 𝑋 and a logis-tic model with linear index in the stated variables for 𝑚0(𝑋).











6: For any observation with esti-mated propensity score larger than 0.95, we replace the propensity score with 0.95. Applying this trim-ming, we replace 12, 10, 13, and 21 observations for the deep tree in 2004-2007 respectively and replace 2, 2, and 1 observation for Basic, Expansion, and Lasso (CV) in 2007.
 


 


(0.019)	(0.021)	(0.023)	(0.026)
-0.037	-0.066	-0.088	-0.041
(0.018)	(0.020)	(0.021)	(0.033)
-0.022	-0.046	-0.061	0.303
(0.025)	(0.030)	(0.033)	(0.227)
-0.035	-0.062	-0.082	-0.049
(0.018)	(0.020)	(0.021)	(0.031)
-0.035	-0.062	-0.083	-0.061
(0.018)	(0.020)	(0.021)	(0.025)
0.013	-0.056	-0.039	-0.071
(0.029)	(0.024)	(0.028)	(0.038)
0.077	0.007	0.100	-0.470
(0.079)	(0.172)	(0.080)	(0.178)
-0.028	-0.040	-0.058	-0.065
(0.019)	(0.021)	(0.021)	(0.026)
-0.027	-0.045	-0.060	-0.069
(0.019)	(0.021)	(0.021)	(0.025)
-0.028	-0.051	-0.055	-0.055









especially for the propensity score, as all methods outside of Deep Tree and Expansion produce notably smaller RMSEs than the No Controls baseline. The other methods all produce similar RMSEs, with a small edge going to Ridge and Lasso. While it would be hard to reliably conclude which of the relatively good performing methods is statistically best here, one could exclude Expansion and Deep Tree from further consideration on the basis of out-of-sample performance suggesting they are doing a poor job approximating the nuisance functions. Best (or a different ensemble) provides a good baseline that is principled in the sense that one could pre-commit to using the best learners without having first looked at the subsequent estimation results.
We report estimates of the ATET in each period in Table 16.3. Here, we see that the majority of methods provide point es-timates that suggest the minimum wage increase leads to de-creases in youth employment with small effects in the initial period that become larger in the years following the treatment. This pattern seems economically plausible as it may take time for firms to adjust employment and other input choices in
 
Table 16.3: Estimated ATET in Min-imum Wage example
 


response to the minimum wage change. The methods that pro-duce estimates that are not consistent with this pattern are Deep Tree and Expansion which are both suspect as they systemati-cally underperform in terms of having poor cross-fit prediction performance. In terms of point estimates, the other pattern that emerges is that all estimates that use the covariates produce ATET estimates that are systematically smaller in magnitude than the No Controls baseline, suggesting that failing to include the controls may lead to overstatement of treatment effects in this example.
Turning to inference, we would reject the hypothesis of no minimum wage effect in 2005 and 2006 at the 5% level, even after multiple testing correction, if we were to focus on the row "Best" (or many of the other individual rows). Focusing on "Best" is a reasonable ex ante strategy that could be committed to prior to conducting any analysis. It is, of course, reassuring that this broad conclusion is also obtained using many of the individual learners suggesting some robustness to the exact choice of learner made.
Because we have data for the period 2001-2007, we can perform a so-called placebo or pre-trends test to provide some evidence about the plausibility of the conditional DiD assumptions, Assumption 16.3.1. Specifically, we can continue to use 2003 as the reference period but now consider 2002 to be the treatment period. Sensible economic mechanisms underlying Assumption
16.3.1 would typically suggest that the ATET in 2002 – before the 2004 minimum wage change we are considering – should be zero. Finding evidence that the ATET in 2002 is non-zero then calls into question the validity of Assumption 16.3.1.
We repeat the exercise for obtaining our ATET estimates and standard error for 2004-2007 and report the results in Table
16.4. Here we see broad agreement across all methods in the sense of returning point estimates that are small in magnitude and small relative to standard errors. In no case would we reject the hypothesis that the pre-event effect in 2002 is different from zero at usual levels of significance. We note that failing to reject the hypothesis of no pre-event effects certainly does not imply that Assumption 16.3.1 is in fact satisfied. For example, confidence intervals include values that would be consistent with relatively large pre-event effects. Conditioning inference on the results of such an assessment is also generally a bad idea; see, e.g. [14] and [15] for a discussion specifically in the context of DiD. However, it is reassuring to see that there is not strong evidence of a violation of the underlying identifying assumption.
 


Table 16.4: Pre-trends Assessment

Basic	0.1541	0.1949	-0.0044	(0.0130)
Expansion	0.1577	0.1949	0.0046	(0.0140)
Lasso (CV)	0.1544	0.1932	-0.0039	(0.0131)
Ridge (CV)	0.1544	0.1935	-0.0053	(0.0131)
Random Forest	0.1635	0.2265	0.0230	(0.0265)
Deep Tree	0.1822	0.2234	0.0080	(0.0276)
Shallow Tree	0.1620	0.1884	-0.0037	(0.0134)
Tree (CV)	0.1550	0.1905	-0.0056	(0.0133)











16.5	Notes
There is a relatively large literature focusing on flexibly esti-mating ATET in DiD contexts. Much of this work has focused on potential failure of the usual practice of estimating homo-geneous coefficient linear models with additive fixed effects for groups and time periods under heterogeneous treatment effects. Specifically, much of the work has noted that coefficients on a treatment variable in a homogeneous linear model with fixed effects need not be proper weighted averages of heteroge-neous treatment effects but may place negative weights on some effects. The possibility of negative weights then leaves open the possibility of, for example, having uniformly positive treat-ment effects but obtaining negative and significant estimates of the coefficient on a treatment variable in a linear model. The DML approach we present in this chapter offers one solution to this problem that allows for flexibly accommodating control variables that can account for heterogeneity. See the excellent review papers [11], [16], [17] for more discussion.

16.6	Notebooks
Notebook 16.6.1 (Minimum Wage) Minimum Wage R Note-book and Minimum Wage Python Notebook contain the analysis of minimum wage example
 

16.7	Exercises
Exercise 16.7.1 (ATET) Verify that the ATET is identified under Assumption 16.3.1. Provide a short explanation of the intuition for the identification result. Give an intuitive exam-ple where the ATET would be identified after conditioning on covariates but where identification of the ATET would fail in the canonical DiD framework (i.e. without conditioning on additional covariates).

Exercise 16.7.2 (Minimum Wage I) Study the minimum wage empirical analysis notebook. Estimate the ATET for observa-tions treated in a year different than 2004 – e.g. repeat the analysis doing the exercise for observations treated in 2005.

Exercise 16.7.3 (Minimum Wage II) Study the minimum wage empirical analysis notebook. Estimate the ATET using the never treated as opposed to the not-yet treated as the control group.
 

16.A Conditional
Difference-in-Differences with Repeated Cross-Sections
Here we provide the Neyman orthogonal score for the ATET in the conditional DiD context with repeated cross-section data. For additional development including formal statement of additional assumptions for DiD with repeated cross sections, see [7], [8], [9].
The chief difference in this setting relative to when one has panel data is that we cannot directly construct the difference between outcomes in the first and second period as we do not see the same individuals across time periods. Rather, we revert to the analog of the canonical DiD estimator by directly working with the four conditional means defined by grouping the treated and control observations pre- and post-treatment. Specifically, we make use of the score function
𝜓(𝑊 , 𝛼, 𝜂) = ( 𝐷𝑇 (𝑌 − 𝑔(1, 2, 𝑋))
−	(𝑌 − 𝑔(1, 1, 𝑋)) 

 
𝑝𝜆(1 − 𝑚(𝑋))
𝑚(𝑋)(1 − 𝐷)(1 − 𝑇)	0 1
𝑝(1 − 𝜆)(1 − 𝑚(𝑋))
𝐷
 
(16.A.1)
 
+ 𝑝 (𝑔(1, 2, 𝑋) − 𝑔(1, 1, 𝑋))
− 𝐷 (𝑔(0, 2, 𝑋) − 𝑔(0, 1, 𝑋)) −
 

𝐷 𝛼
𝑝
 
where 𝑊 = 𝑌, 𝑇, 𝐷, 𝑋 denotes the observable variables for each observation with 𝑇 an indicator which equals one if the observation is in the post-treatment period (period 2) and
𝜂 = (𝑝, 𝜆, 𝑚, 𝑔) denotes nuisance parameters with true values
𝑝0 = E[𝐷], 𝜆0 = E[𝑇], 𝑚0(𝑋) = E[𝐷 | 𝑋], and 𝑔0(𝑑, 𝑡, 𝑋) =
E[𝑌 | 𝐷 = 𝑑, 𝑇 = 𝑡, 𝑋].
Under iid sampling, we can directly apply the generic cross-fitting approach to DML as in Section 9.4. In many DiD settings, researchers wish to allow for unmodeled dependence between observations corresponding to different groups such as cities or counties. As long as there are many such groups, it is straight-forward to modify the DML algorithm to accommodate this dependence. The algorithm simply needs to be adjusted by
 


forming the cross-fitting folds such that all observations within groups are included together in the same fold. Similarly, it is straightforward to adjust inference to account for this depen-dence by applying clustered standard errors with clustering done at the group level.
 


Bibliography



[1]		Isaac Newton. PhilosophiæNaturalis Principia Mathematica. 1687 (cited on page 463).
[2]		John Snow. On the Mode of Communication of Cholera. Edited by John Churchill, Second edition, London. 1855 (cited on page 464).
[3]	Jonathan Roth and Pedro H. C. Sant’Anna. ‘When is par-allel trends sensitive to functional form?’ In: Econometrica 91 (2 2023), pp. 737–747 (cited on page 466).
[4]		Susan Athey and Guido W Imbens. ‘Identification and in-ference in nonlinear difference-in-differences models’. In: Econometrica 74.2 (2006), pp. 431–497 (cited on page 466).
[5]		David Card. ‘The impact of the Mariel boatlift on the Miami labor market’. In: Industrial and Labor Relations Review 43 (1990), pp. 245–257 (cited on page 468).
[6]		Joshua D. Angrist and Alan B. Krueger. ‘Empirical Strate-gies in Labor Economics’. In: Handbook of Labor Economics. Volume 3. Ed. by O. Ashenfelter and D. Card. Elsevier: North-Holland, 1999 (cited on page 468).
[7]	Michael Zimmert. ‘Efficient Difference-in-Differences Es-timation with High-Dimensional Common Trend Con-founding’. In: arXiv preprint arXiv:1809.01643 (2018) (cited on pages 470, 478).
[8]	Neng-Chieh Chang. ‘Double/Debiased Machine Learn-ing for Difference-in-Differences Models’. In: Econometrics Journal 23 (2 2020), pp. 177–191 (cited on pages 470, 478).
[9]	Pedro H. C. Sant’Anna and Jun Zhao. ‘Doubly Robust Difference-in-Differences Estimators’. In: Journal of Econo-metrics 219 (1 2020), pp. 101–122 (cited on pages 470,
478).
[10]		Carolina Caetano and Brantly Callaway. ‘Difference-in-Differences with Time-Varying Covariates in the Parallel Trends Assumption’. In: arXiv preprint arXiv:2202.02903 (2023) (cited on page 471).
[11]		Brantly Callaway. ‘Difference-in-Differences for Policy Evaluation’. In: Handbook of Labor, Human Resources and Population Economics. Ed. by K. F. Zimmermann. Springer Cham, 2023 (cited on pages 471, 472, 476).
 

 
Bibliography
 
481
 


[12]		Brantly Callaway and Pedro H. C. Sant’Anna. ‘Difference-in-differences with multiple time periods’. In: Journal of Econometrics 225 (2 2021), pp. 200–230 (cited on page 472).
[13]		Arindrajit Dube, T. William Lester, and Michael Reich. ‘Minimum wage shocks, employment flows, and labor market frictions’. In: Journal of Labor Economics 34 (3 2016),
pp. 663–704 (cited on page 472).
[14]		Jonathan Roth. ‘Pre-test with Caution: Event-study Es-timates After Testing for Parallel Trends’. In: American Economic Review: Insights 4 (3 2022), pp. 305–322 (cited
on page 475).
[15]		Ashesh Rambachan and Jonathan Roth. ‘A More Credible Approach to Parallel Trends’. In: (2023). forthcoming Review of Economic Studies (cited on page 475).
[16]		Jonathan Roth, Pedro Sant’Anna, Alyssa Bilinski, and John Poe. ‘What’s trending in difference-in-differences? A synthesis of the recent econometrics literature’. In: Journal of Econometrics 235 (2 2023), pp. 2218–2244 (cited
on page 476).
[17]		Clément de Chaisemartin and Xavier D’Haultfœuille. ‘Two-way fixed effects and differences-in-differences with heterogeneous treatment effects: a survey’. In: Economet-rics Journal (June 2022), utac017. doi: 10.1093/ectj/ utac017 (cited on page 476).
 

Regression Discontinuity Designs	17



 
"la nature ne fait jamais des sauts." ("nature never makes jumps.")
– Gottfried Leibniz [1].




















In this chapter we discuss the Regression Discontinuity Design (RDD). First, we introduce the basic idea of Regression Discon-tinuity (RD). RDDs, when they exist, offer a highly credible way to identify causal effects. However, leveraging RDDs without covariates can fall short in practice. We show how modern machine learning methods can be utilized for estimation in RDDs with many covariates.
 
17.1	Introduction	483
17.2	The Basic RDD Frame-work	484
Setting	484
Estimation	485
17.3	RDD with (Many) Covari-ates	486
Motivation for Using Covari-ates	486
Low-Dimensional Covari-ates	487
High-Dimensional Covari-ates	488
Heterogeneous Treatment Ef-fects and Adjustments for Het-erogeneity	492
17.4	Empirical Example	493
17.5	Notes	494
17.6	Notebooks	495
17.7	Exercises	495
 
17.1	Introduction
 

Like many other methods presented in the Advanced Top-ics – instrumental variables, proxy controls, and difference-in-differences – Regression Discontinuity Designs (RDDs) are widely used in empirical work for measuring causal effects in non-experimental settings where we cannot reliably measure all confounders.
The basic RDD structure relies on a so-called running variable or score which determines treatment: units whose score is above a cutoff value are assigned to the treatment, while units with score below the cutoff are assigned to control. Examples are reward of a scholarship if a student’s grade average exceeds a certain threshold, bestowing of license to practice (say, medicine or law) if one’s exam score exceeds a threshold, assignment of a particular medical treatment if a biomarker is above a cutoff, or getting social benefits if income is below some income threshold.
The intuition for identification is that units marginally above and below the threshold are comparable in terms of potential outcomes, since they are the same in all ways except the assign-ment to treatment, assuming of course that there are no other discontinuities at the cutoff that would also render them differ-ent in other ways. The latter continuity in potential outcomes is the identifying assumption in RDDs. For example, suppose we are interested in the causal effect of a student receiving a scholarship on their future academic success. While the future academic success of students with low grade averages is very different from those with high averages, with or without a scholarship, the students right at the cutoff essentially have the same grade averages and are comparable. However, those just above the cutoff have a scholarship and those just below do not. We can thus compare those just above the cutoff to those just below to learn the effect of having a scholarship on people at the grade average cutoff.
We can also conceive of being above or below as random "luck," i.e., exogenous variation. E.g., getting just one more question right on the exam might be viewed as a random event that has nothing to do with the academic preparedness of the student – anyone can happen to accidentally guess right on one question. Viewing falling just to one side or the other of a cutoff as a purely is an alternative approach to identification in RDDs based on local randomization [2].
 













We can always negate the running variable or rename the treatment if the relationship is the other way.
 

17.2	The Basic RDD Framework
Setting
In the sharp RDD the binary treatment variable 𝐷𝑖  0, 1 for individual 𝑖 is assigned on the basis of a running variable 𝑋𝑖 in a deterministic ("sharp") way: 𝐷𝑖 = 1 𝑋𝑖   𝑐 , where 1 denotes
the indicator function and 𝑐 the cutoff value. That is, a unit is
treated (𝐷𝑖 = 1) if the value of the running variable is above the threshold and in the control group (𝐷𝑖 = 0) otherwise. For each individual, we observe additionally the outcome 𝑌𝑖 and potentially some pre-treatment variables 𝑍𝑖 ∈ ℝ𝑝. The
observed data {𝑊𝑖 }𝑛	= {(𝑌𝑖 , 𝑋𝑖 , 𝑍𝑖)}𝑛	are an i.i.d. sample of
 
size 𝑛 from the distribution of 𝑊 = 𝑌, 𝑋, 𝑍 . Note that we also observe 𝐷𝑖 = 1 𝑋𝑖  𝑐 for each individual because we know the cutoff value 𝑐 and observe 𝑋𝑖.
The parameter of interest in RDD is the ATE at the cutoff value
𝑐:
𝜏RD = E [𝑌𝑖(1) − 𝑌𝑖(0) | 𝑋𝑖 = 𝑐] .
This parameter can be identified under mild conditions in the RDD context. Learning treatment effects away from the RDD cutoff 𝑐 generally requires stronger conditions that allow extrapolation.
We now state a simple sufficient condition under which 𝜏RD is identified.

Assumption 17.2.1 (RDD Assumptions [3]) Suppose that i)
the conditional mean of the potential outcomes E [𝑌(𝑡) | 𝑋 = 𝑥]
for 𝑡 ∈ {0, 1} are continuous at the cutoff level 𝑐, ii) that the
density of the running variable, 𝑓𝑋 near the cutoff is positive –
𝑓𝑋(𝑐) > 0, and iii) there is no selection on gains local to the
cutoff: E[𝑌(1) − 𝑌(0)|𝐷, 𝑋 = 𝑥] = E[𝑌(1) − 𝑌(0)|𝑋 = 𝑥] for
𝑥 ∈ (𝑐 − 𝜖, 𝑐 + 𝜖) for some small 𝜖 > 0.
Under Assumption 17.2.1, we have
𝜏RD = lim E (𝑌𝑖 | 𝑋𝑖 = 𝑥) − lim E (𝑌𝑖 | 𝑋𝑖 = 𝑥)
 



Figure 17.1: In the sharp RDD, the assignment of treatment depends in a deterministic way on the un-derlying running variable (or score variable). Units with values of the running variable below a cutoff are not treated, while units above the threshold are treated.





Figure 17.2: Identification and esti-mation in the sharp RDD.
 
𝑥↓𝑐	𝑥↑𝑐
where lim𝑥↓𝑐 and lim𝑥↑𝑐 denote the right-sided and left-sided limit. Hence, the jump in E 𝑌𝑖 𝑋𝑖 = 𝑥 , the conditional ex-pectation function of the observed outcome, at the threshold
determines the causal effect of interest.
 


Estimation
In sharp RDD, we are faced with the problem of estimating the jump in the conditional mean functions at the cutoff value. As we do not see treated and control observations exactly at the cutoff, this problem boils down to estimation of the conditional mean functions at points to the left and right of, but local to, the cutoff value. In practice, we can estimate the conditional mean at points local to the cutoff using conventional local nonparametric methods. Local polynomial estimation has become the default method for this local estimation task in RDD, and we therefore focus on this method following the notation and exposition in
[4]	closely.
Standard RDD Estimator: Without covariates, a weighted linear regression of 𝑌𝑖 on 𝑋𝑖 is estimated locally around the cutoff to estimate the parameter of interest:
𝜏ˆbase(ℎ) = 𝑒2⊤argmin I: 𝐾ℎ (𝑋𝑖 − 𝑐) (𝑌𝑖 − 𝜃′𝑉𝑖)2 .

 
Here, 𝐾 denotes a kernel function, ℎ > 0 a bandwidth, 𝐾ℎ 𝑥 =
𝐾 𝑥 ℎ ℎ, 𝑉𝑖 = 1, 𝐷𝑖 , 𝑋𝑖  𝑐 , 𝐷𝑖 𝑋𝑖  𝑐 ⊤ a vector of ap-propriate transformations of the running variable, and 𝑒2 = 0, 1, 0, 0 ⊤ the unit vector to select the coefficient of 𝐷𝑖, which
is the target parameter.
Under standard conditions for local linear regression such as continuity of the running variable and having bandwidth ℎ approach zero at a suitable rate, the estimator 𝜏base(ℎ) follows
 
By kernel function, we mean a function that integrates to one and is symmetric around 0. Common examples are the uniform kernel,
1
2
the triangular or Bartlett kernel,
𝐾(𝑥) = (1 − |𝑥|)1(−1 < 𝑥 < 1).
 
𝜏base (ℎ) ∼ 𝑁 (𝜏 + ℎ2𝐵base , (𝑛ℎ)−1𝑉base ) .
In the asymptotic distribution, the term ℎ2𝐵base with
𝐵base = 𝜈¯ ( 𝜕2𝔼 [𝑌𝑖 | 𝑋𝑖 = 𝑥]1	− 𝜕 𝔼 [𝑌𝑖 | 𝑋𝑖 = 𝑥]1	)
represents bias, which is of the order ℎ2. The variance
1	1	𝜅
 
𝑛ℎ 𝑉base = 𝑛ℎ 𝑓
 
¯	(𝕍 [𝑌𝑖 | 𝑋𝑖 = 𝑐+] + 𝕍 [𝑌𝑖 | 𝑋𝑖 = 𝑐−])
 
is of the order of (𝑛ℎ)−1. The terms 𝜈¯ and 𝜅¯ in the bias and
variance expressions are constants related to the kernel defined as
𝜈¯ = (𝜈¯2 − 𝜈¯1𝜈¯3) /(𝜈¯2𝜈¯0 − 𝜈¯2 )
 

 
for 𝜈¯𝑗 = ∫ ∞ 𝑣𝑗 𝐾(𝑣)𝑑𝑣 and
𝜅¯ = ∫ ∞ (𝐾(𝑣) (𝜈¯1𝑣 − 𝜈¯2
 


2 𝑑𝑣/(𝜈¯2𝜈¯0 − 𝜈¯2 )2 .
 

The choice of the bandwidth ℎ plays an important role in feasible implementation of RDD estimation. MSE optimal choice of ℎ involves equating squared bias and variance which renders conventional confidence intervals invalid. Calonico et al. (2014)
[5]	considers the use of MSE optimal bandwidths along with bias correction and adjustment to standard errors to account for estimating the bias. Their proposed methods are widely used in practice and available in, e.g., R and python.

17.3	RDD with (Many) Covariates
Motivation for Using Covariates
For identification and estimation of the average treatment effect at the cut-off value, no covariate information is required except the running variable. Of course, additional covariates are avail-able in many applications. As outlined in Cattaneo et al. (2023)
[6]	provide an extensive discussion of the use of covariates in RDDs. They note there are several reasons that using covariates in RDD settings may be useful:
1.	Efficiency and power improvements: As in randomized con-trolled trials, using covariates can increase efficiency and improve power; see, e.g., [7] and [8].
2.	Auxiliary information: In RDD, the running variable deter-mines the assignment of the treatment, and measurement error in the running variable can distort the results. Addi-tional covariates can help overcome measurement issues and help deal with missing data problems.
3.	Treatment effect heterogeneity: Covariates can be used to explore heterogeneity in treatment effects. For example, we can adapt methods from Chapter 14 and Chapter 15.
4.	Other parameters of interest and extrapolation: Conventional RDD without covariates only identifies treatment effects at the cutoff value of the running variable. Making use of additional covariates may allow extrapolation of the treatment effects to other values of the runnign variable away from the cutoff point and may also be useful for identifying other causal parameters.
 


Low-Dimensional Covariates
There are several ways to adjust the RDD estimator to allow for the presence of covariates. Calonico et al. (2019) [7] provide a detailed analysis of the use of additional regressors in RDD in a setting with relatively few covariates. A transparent approach is to simply include the controls linearly within the objective function defining the baseline local linear RDD estimator. That is, we solve
𝜏ˆlin(ℎ) =
 
𝑒2⊤ argmin
 
I: 𝐾ℎ (𝑋𝑖 − 𝑐) (𝑌𝑖 − 𝜃′𝑉𝑖 − 𝛾′𝑍𝑖 2
 
(17.3.1)
 

 
where 𝛾 are the coefficients on the 𝑝 dimensional vector of pretreatment variables, 𝑍𝑖.
It is clear that the estimator for 𝜃 can be equivalently written as a RDD estimator with a covariate-adjusted outcome, 𝑌˜𝑖 = projectio-n coefficients associated with -𝑍𝑖 obtained from solving
 







ℎ	arg min I:
 






𝐾ℎ (𝑋𝑖 − 𝑐)
 






(𝑌𝑖 − 𝛾 𝑍𝑖 )
 
𝛾 =
𝑛
 
𝛾  𝑖=1
 
ˇ	′ ˇ
 
𝜏ˆlin(ℎ) = 𝑒2⊤argmin I: 𝐾ℎ (𝑋𝑖 − 𝑐) (𝑌˜𝑖 − 𝜃′𝑉𝑖 )2 .
 
where 𝑊ˇ 𝑖 is the residual from lo-
 
(𝜃)∈ℝ4  𝑖=1
 
cally partialling 𝑉 out from 𝑊 (with partialling out interpreted el-ementwise if 𝑊 is a vector). That is,
 
𝜏lin (ℎ) is consistent for the conditional average treatment effect
 
𝑊𝑖 = 𝑊𝑖 − 𝜁 𝑉𝑖 with
 
-at 𝑋 = 𝑐,
 
𝜏𝑅𝐷
 
if the conditional distribution of the regressors	ˇ
 
-′ℎ
 
given the running variable varies smoothly around the cutoff. As the estimator is just a local linear regression involving the variables 𝑉 and 𝑍, this results essentially follows immediately from properties of local regression which will hold under mild smoothness conditions without requiring parametric functional
form restrictions.Specifically, if 𝔼 𝑍𝑖 𝑋𝑖 = 𝑥 is twice continu-ously differentiable around the cutoff, we will have
 
𝜁𝑖 = arg min
𝑏
 
I:𝑖=1
 
𝐾ℎ (𝑋𝑖 − 𝑐)
 
(𝑊 − 𝑏′𝑉ˇ )2 .
 
𝜏lin (ℎ) ∼ 𝑁 (𝜏 + ℎ2𝐵base , (𝑛ℎ)−1𝑉lin )
under regularity conditions similar to those for the estimator without covariates. Interestingly, the bias term 𝐵base is the same as in the case without including 𝑍𝑖, but the variance term differs with
𝑉lin =   𝜅¯	 (𝕍 [𝑌𝑖 − 𝑍⊤𝛾0 | 𝑋𝑖 = 𝑐+]
+𝕍 [𝑌𝑖 − 𝑍𝑖⊤ 𝛾0 | 𝑋𝑖 = 𝑐−] )
 


where 𝛾0 is a non-random vector corresponding to the probabil-
ity limit of 𝛾ℎ (see also [8]). Unsurprisingly, the linear adjustment
estimator generally has smaller asymptotic variance than the estimator without covariates; i.e. 𝑉lin  𝑉base . See [7] and [8] for formal statement of the properties of 𝑉lin and discussion of additional potential avenues for inclusion of covariates.

High-Dimensional Covariates
RDD with Lasso
In the case where many covariates are available, one straightfor-ward option is to adopt the procedure from the low-dimensional setting by including all variables linearly inside the local linear regression and then using (local) Lasso regression to estimate the parameters. This procedure has been analyzed by [9] and [8]. Here, we follow [8] closely. The idea is that in a first step the relevant variables are selected with a localized / weighted Lasso regression. In the second step, we then run local linear RDD estimation with the selected covariates from the first step.
Specifically, estimation proceeds as follows:

(Post)-Lasso Estimation in RDD
1. Using a preliminary bandwidth 𝑏 and a penalty parameter
𝜆, one solves a Lasso version of the local linear regression defining the RDD estimator by adding a penalty term to obtain preliminary estimates
(𝜃-,-𝛾)
𝑛
= argmin I: 𝐾𝑏 (𝑋𝑖 − 𝑐) (𝑌𝑖 − 𝑉⊤𝜃 − (𝑍𝑖 − 𝜇ˆ𝑍)⊤ 𝛾)2
𝑖
(𝜃,𝛾)∈ℝ4+𝑝 𝑖=1
𝑝
+ 𝜆 I: 𝑤ˆ 𝑘 |𝛾𝑘 | ,
𝑘=1
where
1  𝑛
𝜇ˆ𝑍 = 𝑛 I: 𝑍𝑖 𝐾𝑏 (𝑋𝑖 − 𝑐)
𝑖=1
and
𝑛  (	(𝑘)	(𝑘))2
𝑤ˆ 2 = 𝑏 I: 𝐾𝑏 (𝑋𝑖 − 𝑐) 𝑍	− 𝜇
𝑘	𝑛 𝑖=1	𝑖	𝑍
are the local sample mean and variance, respectively, of the covariates.
 



2. Let 𝐽ˆ = {𝑘 ∈ {1, . . . , 𝑝} : -𝛾(𝑘) ≠ 0} denote the set of in-
dices of those covariates whose first step Lasso estimates are non-zero. Using the variables in 𝐽ˆ and a final bandwidth ℎ, we compute our estimate of the treatment effect, 𝜏RD, as 𝜏ˆ𝐽ˆ(ℎ) exactly as in (17.3.1) where the set of covariates is restricted to 𝐽ˆ.

[8] provide formal asymptotic properties post-Lasso estimator
𝜏𝐽 ℎ . As the estimator fundamentally relies on properties of the Lasso, a key assumption is approximate sparsity – Definition
3.1.1 – of the coefficients on the controls 𝑍𝑖.
To adapt approximate sparsity to the RDD setting, we de-fine population regression coefficients, 𝜃0 𝐽 , ℎ , 𝛾0 𝐽 , ℎ , and corresponding residuals, 𝑟𝑖 𝐽 , ℎ , for any 𝐽  1, . . . , 𝑝 and bandwidth ℎ:
(𝜃0(𝐽, ℎ), 𝛾0(𝐽, ℎ))
= argmin E 𝐾ℎ (𝑋𝑖 − 𝑐) (𝑌𝑖 − 𝑉⊤𝜃 − 𝑍𝑖(𝐽)⊤𝛾)2  ,
(𝜃,𝛾)
𝑟𝑖(𝐽, ℎ) = 𝑌𝑖 − 𝑉𝑖⊤𝜃0(𝐽, ℎ) − 𝑍𝑖(𝐽)⊤𝛾0(𝐽, ℎ).

Approximate sparsity then means that there exists a covariate set 𝐽∗  1, . . . , 𝑝 that contains a "small" number 𝑠  𝐽∗  𝑝 of regressors that captures the majority of the local predictive power of the control variables 𝑍𝑖. We formalize the requirement that variables in 𝐽∗ capture the majority of the explanatory power by requiring that the local correlation between the regression errors 𝑟𝑖 𝐽∗, ℎ and each component of 𝑍𝑖 is small relative to the estimation error:

max  E r𝐾ℎ (𝑋𝑖 − 𝑐) 𝑍(𝑗)𝑟𝑖 (𝐽∗, ℎ)l1 = 𝑂 J(log 𝑝 \ . (17.3.2)
1

To ensure that properties of the estimator are not heavily in-fluenced by the exact bandwidth choice, we further require that (17.3.2) holds uniformly across an appropriate range of bandwidths.
Under this local approximate sparsity condition and other regularity conditions, [8] show that the post-Lasso estimator
𝜏ˆ𝐽ˆ(ℎ) has the same first-order asymptotic properties as an infeasible estimator 𝜏ˆ𝐽∗ (ℎ) that uses only the subset of variables
 


indexed by 𝐽∗. They then prove an asymptotic normality result for 𝜏𝐽∗ ℎ . Taken together, we then obtain that 𝜏𝐽 ℎ of 𝜏RD satisfies
 
√𝑛ℎ 𝜏ˆ𝐽ˆ (ℎ) − 𝜏RD − ℎ2B𝑛
S𝑛
 

→ N(0, 1),
 
with asymptotic bias and variance, respectively, given by	For generic random vectors  𝐴
and 𝐵, 𝜇𝐴(𝑥)  = E(𝐴  |  𝑋  =
 
B ≈ 𝐶B (𝜇′′
 
− 𝜇′′
 
)	and	S2 ≈  𝐶S  (𝜎2
 
+ 𝜎	) .
 
𝑥),  𝜇𝐴𝐵(𝑥)  =  E (𝐴𝐵′ | 𝑋 = 𝑥) ,
 
𝑛	𝑛
 
𝑓 (𝑐)	-	-
 
𝜎2
 
(𝑥) = 𝜇𝐴𝐵(𝑥) − 𝜇𝐴(𝑥)𝜇𝐵(𝑥)′.
 


Here 𝐶B and 𝐶S are constants that depend on the kernel function 𝐾 only, and 𝑌-𝑖 = 𝑌𝑖 − 𝛾𝑛′ 𝑍𝑖 (𝐽𝑛) with
 
Denote 𝜎𝐴 𝑥 = 𝜎𝐴𝐴 𝑥 for sim-plicity. For a generic function 𝑔, we also write 𝑔  = lim𝑥 𝑐 𝑔 𝑥 and
𝑔  = lim𝑥 𝑐 𝑔 𝑥 for its right and
left limit at the RDD cutoff 𝑐, re-
 
𝛾𝑛
 

=	2
𝑍(𝐽𝑛)−
 
𝜎2
𝑍(𝐽𝑛)+
 
−1	2
𝑌𝑍(𝐽𝑛)−
 
𝑌𝑍(𝐽𝑛)+ ) ,
 
spectively. Thus, 𝜏RD = 𝜇𝑌+ − 𝜇𝑌−.
 
is a covariate-adjusted version of the outcome variable that uses a vector 𝛾𝑛 that can be thought of as an approximation of
𝛾0 𝐽∗, ℎ that is independent of the bandwidth. The estimator is thus first-order asymptotically equivalent to a basic sharp RDD estimator with the covariate-adjusted outcome 𝑌˜𝑖 replacing the original outcome 𝑌𝑖

RDD with generic ML Methods

As discussed above in the section on using low-dimensional control variables, adjusting for a small number of controls by including them linearly within the local linear RDD estimator is asymptotically equivalent to running a local linear RDD regres-sion with a modified outcome variable 𝑌𝑖  𝑍′𝑖 𝛾 for appropriately
defined 𝛾. Noack et al. (2023) [4] consider flexible inclusion of
control variables, allowing for high-dimensional settings, by considering more general outcome variable adjustments of the form 𝑌𝑖  𝜂0 𝑍𝑖 for potentially nonlinear function 𝜂0. That is, they consider employing the conventional, covariate free RDD estimator using the adjusted outcome 𝑌𝑖   𝜂0 𝑍𝑖  in place of
𝑌𝑖.
Under the assumption that E 𝜂 𝑍 𝑋 = 𝑥 is twice continuously differentiable at points local to 𝑋 = 𝑐 for a large class of functions
𝜂, different choices of 𝜂0 result in the same estimand for the
 


 
adjusted RDD estimator because
𝜏𝑅𝐷 = lim E[𝑌 − 𝜂0(𝑍)|𝑋 = 𝑥]
− lim E[𝑌 − 𝜂0(𝑍)|𝑋 = 𝑥]
 


(17.3.3)
 
for any choice of 𝜂0 withing this class of functions under this assumption. This assumption seems reasonable in settings where treatment can be safely assumed to have no effect on
𝑍, such as scenarios where 𝑍 are pretreatment characteristics. However, the choice of 𝜂0 impacts the performance of the RDD estimator by altering the estimator’s asymptotic variance. [4] show that the optimal choice of 𝜂0 with regard to the asymptotic variance of the RDD estimator of the treatment effect is the average of the conditional expectation functions of the outcome given the running variables and covariates just to the right and left of the cutoff value.
Given these observations,[4] provide an approach to allow for flexible covariate adjustment in high-dimensional settings using modern machine learning methods to estimate the optimal 𝜂0. They consider a DML style estimator that employs cross-fitting and consists of two steps.

General ML Estimation in RDD [4]
1.	Randomly split the data {𝑊𝑖 }𝑖∈1,...,𝑛 into 𝑆 folds of (approx-imately) equal size, collecting the corresponding indices
in the sets 𝐼𝑠, for 𝑠 ∈ 1, ..., 𝑆. Let -𝜂𝑠(𝑧) = -𝜂 (𝑧; {𝑊𝑖 }𝑖∈𝐼𝑐 ),
𝑠
for 𝑠 ∈ 1, ..., 𝑆 and 𝐼𝑐 the set of indices of observations not
𝑠
included in fold 𝑠, be the researcher’s preferred estimator of 𝜂0 calculated using only data from outside the 𝑠th fold.
2.	Estimate 𝜏𝑅𝐷 by computing a local linear "no covariates" RDD estimator that uses the adjusted outcome 𝑌˜𝑖 (-𝜂𝑠(𝑖)) =
𝑌𝑖 − -𝜂𝑠(𝑖) (𝑍𝑖) as the dependent variable, where 𝑠(𝑖) denotes
the fold that contains observation 𝑖. I.e. estimate 𝜏𝑅𝐷 as
𝑛
𝜏ˆ𝜂(ℎ) = 𝑒2⊤argmin I: 𝐾ℎ (𝑋𝑖 − 𝑐) (𝑌˜𝑖 (-𝜂𝑠 𝑖 ) − 𝜃′𝑉𝑖 )2 .
ˆ	𝜃∈ℝ4	𝑖=1	( )

[4]	establish that the estimator 𝜏𝜂 ℎ is asymptotically equivalent to the infeasible estimator
𝑛
-𝜏 (ℎ) = 𝑒2⊤argmin I: 𝐾 (𝑋 − 𝑐) (𝑌˜ (𝜂¯	) − 𝜃′𝑉 )2 ,
 


where 𝜂 is a deterministic approximation of 𝜂 whose error vanishes in large samples in some appropriate sense. It then holds that
𝜏ˆ𝜂ˆ(ℎ) ∼ 𝑁 (𝜏 + ℎ2𝐵base , (𝑛ℎ)−1𝑉(𝜂¯))
The asymptotic variance in the above expression is minimized
if 𝜂 is consistent for 𝜂 , in the sense that 𝜂 = 𝜂 . However, the
-	0	¯	0
distributional approximation is valid even if 𝜂 ≠ 𝜂0 because (17.3.3) holds for (essentially) all adjustment functions, not just the optimal one. In that sense, the procedure allows for
misspecification in the choice of model for 𝜂. Moreover, 𝑉 𝜂 is typically smaller than 𝑉base even under misspecification. Valid confidence intervals can easily be constructed for 𝜏 by applying standard methods developed for settings without covariates
to a data set with running variable 𝑋𝑖 and outcome 𝑌˜𝑖  𝜂𝑠 𝑖
ignoring sampling uncertainty about the estimated adjustment function.

 
Heterogeneous Treatment Effects and Adjustments for Heterogeneity
Our treatment of covariates thus far has been focused on using covariates to increase efficiency of the average treatment effect at the cutoff value 𝜏RD. Covariates can also help us understand heterogeneity of treatment effects.
At a conceptual level, it is clear that we can repeat the setup in Section 17.2 conditional on 𝑍 = 𝑧, leading to the CATE at the cutoff :
𝜏C−RD(𝑍) = E[𝑌(1) − 𝑌(0) | 𝑍, 𝑋 = 𝑐]
= lim 𝑔(𝑥, 𝑍) − lim 𝑔0(𝑥, 𝑍),
 












We can also define GATEs at the cut-off exactly as in Chapter ??. More generally, one may adapt the ma-terial on heterogeneous treatment effects from Chapter 14 and Chap-ter 15 to the RDD setting.
 
𝑥↓𝑐
where 𝑔0(𝑋, 𝑍) = E[𝑌 | 𝑋, 𝑍].
 
𝑥↑𝑐
 
A potentially policy-relevant summary of 𝜏C RD 𝑍 is its aver-age:
𝜏A−C−RD = E[𝜏C−RD(𝑍)] = E[E[𝑌(1) − 𝑌(0) | 𝑍, 𝑋 = 𝑐]].
For example, if we were to assume that 𝑍 accounts for all treatment effect heterogeneity across values of the running variable, that is, 𝑌 1  𝑌 0   𝑋  𝑍,1 then we would conclude
that 𝜏A C RD = E 𝑌 1 𝑌 0 is the marginal ATE in the population, not just at the cutoff. More generally, we can say
 








1: The weaker conditional mean-independence of 𝑌 1	𝑌 0 and
1 𝑋 = 𝑐 , given 𝑍, suffices, but is perhaps harder to reason about.
 


 
that 𝜏A−C−RD controls for the heterogeneity modulated by 𝑍, without requiring that 𝑍 accounts for all effect heterogeneity.
We can leverage DML to estimate 𝜏A−C−RD. For ℎ > 0, consider a smoothed version of the same parameter:
𝜏˜ℎ =	∞(41[𝑥 > 𝑐] − 2)𝐾ℎ(𝑥 − 𝑐)E[𝑔0(𝑥, 𝑍)]d𝑥.
−∞
Note that limℎ 0 𝜏ℎ = 𝜏A C RD under appropriate continuity of 𝑔0 𝑥, 𝑊 near 𝑥 = 𝑐 for almost every 𝑊. The quantity 𝜃0 = 𝜏ℎ is a simple linear summary of 𝑔0, similar to those we studied in Chapter 14. We can apply DML to estimate 𝜃0 = 𝜏ℎ using the Neyman orthogonal score
𝜓(𝑊; 𝜃, 𝜂) = ∫ ∞(41[𝑥 > 𝑐] − 2)𝐾ℎ(𝑥 − 𝑐)𝑔(𝑥, 𝑍)d𝑥
(41[𝑥 > 𝑐] − 2)𝐾ℎ(𝑋 − 𝑐) 𝑌	𝑔 𝑋, 𝑍	𝜃,
𝑓 (𝑋 | 𝑍)
where 𝜂 = 𝑔, 𝑓 are nuisance functions where the population value of 𝑓 , 𝑓0, is the conditional density of 𝑋 given 𝑍. Imple-
menting DML in this setting then requires estimation of the conditional expectation function 𝑔0 and the conditional density function 𝑓0.2

17.4	Empirical Example
 




























2: There are a variety of ap-proaches for estimating conditional density functions using machine learning methods. See, for exam-ple, [10], [11], and [12].
 

In this section, we examine the effect of the antipoverty program PROGRESA/Oportunidades on the consumption behavior of families in Mexico in the early 2000s using RDD. Data for this application are provided by [5]. We follow [4] in the presentation
of the results.	The Notebooks 17.6.1 implement
the empirical exercise.
The program was intended for families in extreme poverty and included financial incentives for participation in measures tar-geted at improving the family’s health, nutrition, and children’s education. The effect of this program has been widely studied in economics; see, e.g. Parker and Todd (2017) [13].
Eligibility for the program was determined based on a pre-intervention household poverty-index. Individuals above a cer-tain threshold were eligible to receive a cash transfer through the program(and are coded as the treatment group), while individuals below the threshold were excluded and recorded
 


Table 17.1: RDD Estimates of Ef-fects of Progresa




















 
as a control group. All observations above the threshold partic-ipated in the program, which makes the analysis fall into the standard (sharp) RDD framework.
We look at the outcome variables food and non-food con-sumption, both one and two years after the implementation of the program. The data set contains 1,944 observations and 27 measured variables including outcomes and pre-treatment de-mographic and socioeconomic variables. We summarize RDD estimates of the treatment effect on the four outcome variables when we do not include controls, adjust for controls linearly, and adjust for controls using a handful of machine learning methods in Table 17.1.
As the theory predicts, the point estimates obtained across the different methods are quite similar relative to standard errors. We do see that the methods that control for pretreatment variables are somewhat more precise than the baseline estimates that do not use controls, though the method used to adjust for the controls does not seem to have a major impact.

17.5	Notes
The ideas behind RDDs and IVs come together in fuzzy RDDs. Whereas in sharp RDDs the treatment assignment is determin-
 
Because our treatment variable is eligiblity for the program, we are technically estimating the average intention to treat effect at the eligibil-ity cutoff.
 


istic depending on being above or below the cutoff, in fuzzy RDDs the assignment mechanism is assigned at random with a assignment probability that need not be 0 or 1. Nonetheless, as in the sharp case, there is a discontinuity at the cutoff level. Then, for the units in an infinitesimal neighborhood of the cutoff, being just above or just below can be understood as an instrument for the treatment, with the assignment probability re-flecting the compliance and the size of the discontinuity therein being the strength of the instrument. Almost the same tools for IV can be used once we localize to the cutoff.
Excellent introductions and surveys for RDD are the "classics"
[14] and [15]. Updates including recent results are [16], [17], [18]
and the monographs [19] and [2].

17.6	Notebooks
Notebook 17.6.1 (RDD) R Notebook for RDD and Python Notebook for RDD provide an analysis of the effect of the antipoverty program Progresa/Opportunidades on the con-sumption behavior of families in Mexico in the early 2000s.


17.7	Exercises
Exercise 17.7.1 ((Theoretical). RDD) Derive the moment con-ditions which identify the target parameter in RDD and show that it is orthogonal with regard to covariates.

Exercise 17.7.2 (RDD in Practice) In Israel, there is a strict restriction on the maximum size of public-school classrooms. For several decades in the previous century, the maximum was 40, such that, say, having 81 enrolled in a single grade meant a school has to open three parallel classrooms for that grade so that no one classroom has more than 40 students. Discuss why this structure induces an RDD for the study of the impact of class size on academic performance? Assuming we have the school id, class id, and test scores of each individual student in, say the 5th grade in 1991, how would you construct an RDD: what would be the unit of analysis, the running variable, and the cutoff? How should we interpret the ATE and to what kind of student population might it not be relevant for and why? (Once you have thought about this study question, you can read about the study that famously leveraged this RDD
 


in [20].)
 


Bibliography



[1]	Gottfried Wilhelm Leibniz. Nouveaux Essais sur l’entendement humain. 1765 (cited on page 482).
[2]		Matias D. Cattaneo, Nicolas Idrobo, and Rocio Titiunik. ‘A Practical Introduction to Regression Discontinuity Designs: Extensions’. In: 2023 (cited on pages 483, 495).
[3]		Jinyong Hahn, Petra Todd, and Wilbert Van der Klaauw. ‘Identification and Estimation of Treatment Effects with a Regression-Discontinuity Design’. In: Econometrica 69.1 (2001), pp. 201–209. (Visited on 04/19/2024) (cited on page 484).
[4]		Claudia Noack, Tomasz Olma, and Christoph Rothe. ‘Flexible Covariate Adjustments in Regression Disconti-nuity Designs’. In: (2023) (cited on pages 485, 490, 491,
493).
[5]		Sebastian Calonico, Matias D. Cattaneo, and Rocio Titiu-nik. ‘Robust Nonparametric Confidence Intervals for Regression-Discontinuity Designs’. In: Econometrica 82.6 (2014), pp. 2295–2326. (Visited on 02/08/2024) (cited on pages 486, 493).
[6]		Matias D. Cattaneo, Luke Keele, and Rocio Titiunik. ‘Co-variate Adjustment in Regression Discontinuity Designs’. In: 2023 (cited on page 486).
[7]		Sebastian Calonico, Matias D. Cattaneo, Max Farrell, and Rocío Titiunik. ‘Regression Discontinuity Designs Using Covariates’. In: The Review of Economics and Statistics 101.3 (2019), pp. 442–451. doi: 10.1162/rest_a_00760 (cited
on pages 486–488).
[8]	Alexander Kreiss and Christoph Rothe. ‘Inference in re-gression discontinuity designs with high-dimensional covariates’. In: The Econometrics Journal 26.2 (Dec. 2022),
pp. 105–123. doi: 10 . 1093 / ectj / utac029 (cited on pages 486, 488, 489).
[9]	Yoichi Arai, Taisuke Otsu, and Myung Hwan Seo. ‘Re-gression Discontinuity Design with Potentially Many Covariates’. In: 2022 (cited on page 488).
 


[10]	Rafael Izbicki and Ann B. Lee. ‘Converting high-dimensional regression to high-dimensional conditional density es-timation’. In: Electronic Journal of Statistics 11.2 (2017),
pp. 2800 –2831. doi: 10 . 1214 / 17 - EJS1302 (cited on page 493).
[11]	Zĳun Gao and Trevor Hastie. ‘LinCDE: Conditional Den-sity Estimation via Lindsey’s Method’. In: Journal of Ma-chine Learning Research 23 (2022), pp. 1 –55 (cited on
page 493).
[12]		Jonas Rothfuss, Fabio Ferreira, Simon Walther, and Maxim Ulrich. Conditional Density Estimation with Neural Networks: Best Practices and Benchmarks. 2019 (cited on page 493).
[13]		Susan W. Parker and Petra E. Todd. ‘Conditional Cash Transfers: The Case of Progresa/Oportunidades’. In: Jour-nal of Economic Literature 55.3 (2017), pp. 866–915. doi: 10.1257/jel.20151233 (cited on page 493).
[14]		Guido W. Imbens and Thomas Lemieux. ‘Regression discontinuity designs: A guide to practice’. In: Journal of Econometrics 142.2 (2008). The regression discontinuity design: Theory and applications, pp. 615–635. doi: https:
//doi.org/10.1016/j.jeconom.2007.05.001 (cited on page 495).
[15]	David S. Lee and Thomas Lemieux. ‘Regression Disconti-nuity Designs in Economics’. In: Journal of Economic Litera-ture 48.2 (2010), pp. 281–355. doi: 10.1257/jel.48.2.281
(cited on page 495).
[16]	Blaise Melly and Rafael Lalive. Estimation, inference, and interpretation in the regression discontinuity design. eng. Discussion Papers. Bern, 2020. url: http://hdl.handle. net/10419/228904 (cited on page 495).
[17]		Matias D. Cattaneo and Rocío Titiunik. ‘Regression Dis-continuity Designs’. In: Annual Review of Economics 14.1 (2022), pp. 821–851. doi: 10.1146/annurev-economics-
051520-021409 (cited on page 495).
[18]	Matias D. Cattaneo, Luke Keele, and Rocío Titiunik. ‘A guide to regression discontinuity designs in medical applications’. In: Statistics in Medicine n/a.n/a (2023),
pp. 1–31. doi: https:// doi. org/ 10 . 1002 / sim. 9861
(cited on page 495).
[19]		Matias D. Cattaneo, Nicolás Idrobo, and Rocío Titiunik. A Practical Introduction to Regression Discontinuity Designs: Foundations. Elements in Quantitative and Computational Methods for the Social Sciences. Cambridge University Press, 2020 (cited on page 495).
 


[20]		Joshua D Angrist and Victor Lavy. ‘Using Maimonides’ rule to estimate the effect of class size on scholastic achievement’. In: The Quarterly Journal of Economics 114.2 (1999), pp. 533–575 (cited on page 496).
 
Index


 
𝑅2, 20, 21
A/B test, 48
adaptivity/adaptive inference, 26
Adjusted 𝑅2, 22
approximate distribution, 27, 38
autoencoder, 284
average predictive effect - APE, 46, 48, 137,
254
average treatment effect - ATE, 44, 48, 54,
137, 201, 254
average treatment effect on the treated -
ATET, 146, 148, 464
bad controls, 316
Bagging - Bootstrap Aggregation, 206 Berry-Esseen theorem, 39
Best Linear Approximation, 16 Best Linear Prediction, 13, 18 best linear prediction rule, 13
Best Linear Predictor, 14, 55, 70, 72, 79
Best Predictor, 15, 201
Bidirectional Encoder Representations from Transformers - BERT, 295
boosted trees, 207
boosting, 207
bootstrap sample, 205
causal discovery, 195
centered variable, 14, 20, 55, 73 Central Limit Theorem, 38 collider bias, 159, 320
comparative statics, 156
conditional average predictive effect -
CAPE, 52, 137
conditional average treatment effect -
CATE, 52, 137, 380
conditional average treatment effect: CATE, 492
conditional exogeneity, 135, 154, 238
conditional expectation function, 15, 201
conditional independence, 135
confidence band, 105, 118
 
confidence interval, 27
Consistency assumption, 45, 135
constructed regressor, 15
counterfactuals, 44, 156
covariate balance, 53, 142
cross-fitting, 239, 241
cross-validation, 77, 96
DAG – cross-world, 188 Dantzig selector, 96
deep neural networks, 213 delta method, 50, 56
dictionary, 15, 71
difference-in-differences - DiD, 464 directed acyclic graph - ancestors, 181 directed acyclic graph - backdoor path, 157,
183, 189
directed acyclic graph - blocked path, 183 directed acyclic graph - children, 181 directed acyclic graph - colliders, 157 directed acyclic graph - d-separation, 183 directed acyclic graph - DAG, 138, 156, 173,
181
directed acyclic graph - descendants, 181 directed acyclic graph - directed path, 183 directed acyclic graph - M-bias, 320 directed acyclic graph - nodes, 157 directed acyclic graph - parents, 157, 181 DML - strong identification, 269
DML Algorithm, 267, 360
Double Lasso, 107, 139, 143 double/debiased machine learning - DML,
238
dropout, 211
early stopping, 211
effective dimension, 79, 108
Elastic Net, 87
embeddings, 281
Embeddings from Languae Models: ELMo, 291
empirical average, 18
 
endogenous variables, 156
error, 14
exact binomial ratio test, 52 exogenous variables, 156
Frisch-Waugh-Lovell Theorem, 25
fuzzy RDD, 494
Generative Pre-Trained Transformer - GPT, 299
group average treatment effect - GATE, 145
heterogeneous effects, 117
high-dimensional setting, 70
Horvitz-Thompson method, 140
identification, 158
ignorability, 135, 154
independence, 14, 134
independent and identically distributed - iid, 18
instrumental variables, 331
interactive regression model - IRM, 254
Lasso, 31, 74
Lasso penalty parameter, 76 Lava, 87
lift, 55
linear regression, 13, 54, 139
local average treatment effect - LATE, 342, 365
Markov factorization, 182
Mean Squared Error - MSE, 13, 15, 18, 20
mediation, 158
multitask learning, 214
neural network - activation function, 209 neural network - hidden layer, 213 neurons, 209
Neyman orthogonality, 27, 105, 112, 238,
264
No interference assumption, 45 Normal Equations, 14, 18, 25, 240
nuisance parameters, 244
omitted variable bias, 133, 333 Ordinary Least Squares - OLS, 18
 
orthogonality, 14
out-of-sample 𝑅2, 23
overfitting, 21, 72
overlap assumption, 137, 269
panel data, 464
parallel trends, 464, 465
parallel trends, conditional, 464, 469
parents, 157
partialling-out, 24, 106, 240
partially linear instrumental variable model, 360
partially linear instrumental variables models, 336
partially linear regression model - PLM, 239
penalized regression, 74, 139
personalized policies, 381
placebo test, 475
Post-Lasso, 78
potential outcomes, 44, 134
predictive effect, 23, 28, 106, 135
principal components, 282
propensity score, 136, 143 propensity score weighting, 141 proximal causal inference, 347 proxy controls, 331, 345
random assignment, 48
random forest, 205
randomized controlled trial - RCT, 48, 114 rank conditions, 158
RDD, 482
recurrent neural network, 291 regression coefficients, 18
Regression Discontinuity Design, 482 regression function, 15
regression trees, 201
regularization bias, 75, 78
residual, 14, 18
residuals, 207, 240
Ridge, 82
running variable, 483
sample linear regression, 18, 19
sample splitting, 23
score function, 263
selection bias, 46, 135, 137
 
sharp RDD, 484
single layer neural network, 209 sparsity, 72, 79, 108
Square-root Lasso, 95
staggered adoption, 472 stochastic gradient descent, 210
stochastic gradient descent - batch, 211 stratified RCT, 142
structural equation models, 153, 156, 173,
181
structural function, 44
technical regressor, 15, 30, 70, 140, 209
test/validation data, 22
 
text embeddings, 288
training data, 22
transformations, 15
transformers, 294
treatment effect heterogeneity, 52 triangular structural equation model, 155
unconfoundedness, 135, 238
variational autoencoder, 286
wage gap example, 28
weak identification/instruments, 368
Word2Vec, 288
