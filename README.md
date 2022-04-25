This repository hosts files for our python clinical dosing project.

# 1. Introduction 

Clinical trials are a critical component of pharmaceutical drug product development. An important objective of clinical trials, especially during the early phases, is to determine the dose strengths that will be progressed to the later clinical stages. These dose-range studies eventually help determine the final commercial strengths that are made available to patients. To present a (fictitious) representative example, the dose-range studies may investigate the efficacy of a drug over a wider range of, say, 20 mg to 300 mg. Eventually, tablets with dose strengths of 50 mg and 150 mg may be commercialized, based on the clinical efficacy and safety, among other considerations.

The dose-ranging clinical studies require providing patients/ trial participants with a wide range of dose strengths. In the previous representative example, the clinical study requires flexibility to dose from 20 mg to 300 mg, which is decided based on the study protocol, the patient/participant’s age, body weight, or other factors. To supply such a broad range of doses, multiple tablet strengths need to be developed to support the clinical needs. During the trial, patients/participants will receive multiple tablets dose to achieve the target dose. For example, to receive a 140 mg dose, a patient may have to ingest 1 tablet that is 100 mg and 2 tablets that are 20 mg each, for a total of 3 tablets. If the dosing protocol is based on age or body-weight calculations, then the trial would require flexibility in dosing any arbitrary dose value in the range of interest. To illustrate, based on the body weight, a patient may be required to take 132 mg of the drug, which is not achievable using 100 mg and 20 mg tablet strengths alone. An additional, third tablet strength needs to be manufactured to provide this dosing flexibility. Else, the accuracy of the dosing plan is compromised (e.g., dosing 120 mg or 140 mg against a target of 132 mg).  

Manufacturing scientists and engineers supporting dose-range clinical studies strive to manufacture multiple dose strengths that best support the clinical needs. The desire is to keep the number of tablets per dose to a minimum while ensuring highest-possible dose accuracy. (Note that a 1 mg tablet would meet dose-accuracy needs, but is highly impractical because a patient would have to ingest 132 tablets for a 132 mg dose!)   

For example, the choice to further develop, say, a 50 mg strength vs. a 60 mg strength will be determined based on the outcomes of earlier clinical studies, taking into account the safety, efficacy, and intended patient population, among other considerations. Early clinical studies will oftentimes study a wider dosage range (say, 20 mg to 100 mg, following the previous example) to finally determine that a 60 mg dose is ideal in terms of efficacy. Thus far, the decision to manufacture clinical tablet strengths is based on heuristics. For example, in order to dose between 20 mg and 300 mg, a formulation scientist may empirically choose to manufacture strengths of 5 mg, 20 mg, and 75 mg, which would allow coverage of the range of interest, albeit with some dosing errors. However a more rigorous mathematical approach to minimize the number of tablets dosed as well as the dosing-accuracy errors is preferable. Furthermore, it may emerge that a set of four tablet strengths (say, 5 mg, 20 mg, 75 mg, 150 mg) may be considerably better for patients. At present, no mathematical framework is available to address this problem, other than heuristic approaches or expert intuition.  

This works presents an elegant numerical approach to address this problem. Starting with the classic change-making problem (refs, described in Sec. 3), we propose a simple adaptation (Sec. 4) to solve the present clinical-dosing problem. A generalized form of the clinical-dosing problem is described in Sec. 2, listing the assumptions and constraints; these assumptions may be modified for more specific scenarios in the future. Finally, some representative results using the modified algorithm are presented in Sec. 5, followed by conclusions, general observations, and potential future work (Sec. 6).  

# 2. Description of the clinical-dosing problem statement

A generalized description of the clinical-dosing problem is described as follows. This generalized description and constraints can be modified, along with the solution algorithm, to adapt to the needs of a specific clinical protocol.

- The dose range of the clinical study is defined. Patients need to be dosed a drug amount, expressed in milligrams, between a lower limit and upper limit of the dose range. For example, a study may require patients to be dosed between 50 mg and 200 mg, based on age, weight, or other clinical considerations.  

- Doses need to be delivered to patients in whole-number increments of 1 mg. For large clinical studies, it is assumed that all doses between the lower and upper limits are equally likely to be needed. Continuing the previous example, the ability to dose all values between 50 and 200 mg (inclusive) is desired; i.e., be able to dose 50 mg, 51 mg, 52 mg, …, 199 mg, up to 200 mg.  

- The intent is to minimize the number of tablets that patients must swallow for each dose. For a given dose range, the average number of tablets across all dose values should be minimized. Ideally, clinicians wish to avoid situations where patients need to swallow more than 6 tablets for any given dose, if possible.  

- To supply the clinical study, multiple tablet strengths can be manufactured. Typically, no fewer than 2 tablet strengths and no more than 5 tablet strengths should be manufactured.

- There is a constraint on the lowest-dose tablet that can be manufactured. For example, it may be impractical to have a 1 mg tablet, as such a small tablet and dose may prove difficult to manufacture. In such case, a minimum manufacturable tablet strength is defined, say, 20 mg or greater.

There are marked similarities between the present clinical-dosing problem and the classic change-making problem (refs, described in Sec. 3). We will take advantage of this similarity, and adapt the classic-change making problem to the clinical-dosing problem (Sec. 4). Some representative solutions and results from the clinical-dosing problem are described in Sec. 5, along with a description of how these results can help formulators and clinicians with clinical manufacturing and trials.

# 3. Algorithm for solving the classical change-making problem

The change-making problem and its solution is a classic problem that is part of most algorithms classes. The change-making problem addresses the question of how best to return change such that the number of coins returned is minimized. For example, with the United States coinage system, comprised of coins of face value 1 cent, 5 cents, 10 cents and 25 cents2, what would be the best way to return 69 cents? One poor and cumbersome solution would be to return 69 coins that are 1 cent each. One could also return 6 × 10-cent coins along with a 5-cent coin and 4 × 1-cent coins, for a total of 15 coins. However, the best approach is to return 2 × 25-cent coins, a 10-cent coin, a 5-cent coin, and 4 × 1-cent coins, for a total of 8 coins, which is the best solution involving the least amount of coins.  

 

This process of determining the optimal solution may be repeated for all change values from 1 cent up to 99 cents (i.e., until a dollar banknote becomes available). The intent would be to ensure that cashiers and customers handle the fewest number of coins possible. That is, to minimize the average number of coins returned across all change values from 1 to 99 cents (Table 1).

## **Table 1**

| Change to be returned (cents) | Optimally returned change (coin face values in cents) | Total coins |
| ------------------------- | ------------------------- | ----------- |
| 1 | 1 | 1 |
| 2 | 1, 1 | 2 |
| 3 | 1, 1, 1 | 3 |
| 4 | 1, 1, 1, 1 | 4 |
| 5 | 5 | 1 |
| 6 | 5, 1 | 2 |
| 7 | 5, 1, 1 | 3 |
| 8 | 5, 1, 1, 1 | 4 |
| 9 | 5, 1, 1, 1, 1 | 5 |
| 10 | 10 | 1 |
| (values in between not shown) |
| 98 | 25, 25, 25, 10, 10, 1, 1, 1 | 8 |
| 99 | 25, 25, 25, 10, 10, 1, 1, 1, 1 | 9 |
| | Average | 4.75 (average from 1 to 99 cents)

Already, the similarity between the change-making problem and the clinical-dosing problem is starting the emerge. The intent in both cases is to minimize the average number of coins/tablets that is distributed. The mathematical definition of the change-making problem and its solution algorithm are described in Table 2. Note that the change-making problem is a specialized case of the 0/1 knapsack problem (refs). 

## **Table 2 (Change making problem)**

| Change to be returned (cents) | Optimally returned change (coin face values in cents) | Total coins |
| ------------------------- | ------------------------- | ----------- |


The algorithm in Table 2 is to determine the best way to return minimum coins when the coinage system is already defined. Now the change-making problem may be modified further to make it analogous to the clinical-dosing problem. Instead of being constrained by a given coinage system (e.g., the US system with 1 cent, 5 cents, 10 cents, and 25 cents), say we had the freedom to define a new coinage system. The problem could be restated as: What are the best coin face values for a country such that the average number of coins returned is minimized under this new coinage system? Shallit (2003) has addressed this problem for a coinage system where the coin denominations may be  

- It should be noted that a “greedy” algorithm may not always yield the best solution. A greedy algorithm always starts with the highest denomination, and then   

# 4. Change-making algorithm adaptations to clinical-dosing problem

# 5. Results and Discussion 

The clinical-dosing problem reduces to the classic change-making problem if the value of the penalty tablet is set to 1, and the dose-range set as 1 to 99 (irrespective of the units, milligrams or cents). Our implementation of the clinical-dosing algorithm may be verified by using these change-making parameters, and comparing the results to the solutions of the change-making problem reported in Shallit (2003).    

Table 3 reports the three best optimal solutions for coinage systems with 3, 4, and 5 coins, alongside the results from Shallit (2003). Our averages were computed over the range 1 to 99, whereas Shallit (2003) computed them over 0 to 99, which explains the slight discrepancy in the average coins returned; the results are otherwise identical. For the 4-coin system, there are two equally good top solutions: {1, 5, 18, 25} and {1, 5, 18, 19}, also reported by Shallit (2003). For the 5-coin system, there are four solutions tied in second place. These results confirm that our change-making problem algorithm is properly implemented.

| Coins in currency system | Best Solutions Generated | Average coins returned<sup>1</sup> | Solution reported in Shallit (2003) - Best Solutions | Average coins returned<sup>2</sup>
| --- | --- | --- | --- | --- | 
| 2 coins | | | (1,10)<br/>(1,11) | 9.00<br/>9.00 |
| 3 coins | (1, 12, 19)<br/>(1, 7, 23)<br/>(1, 8, 19) | 5.20<br/>5.21<br/>5.23 | (1, 12, 19) | 5.15 |
| 4 coins | (1, 5, 18, 25)<br/>(1, 5, 18, 29)<br/>(1, 4, 18, 30) | 3.93<br/>3.93<br/>3.94 | (1, 5, 18, 25)<br/>(1, 5, 18, 29) | 3.89<br/>3.89 |
| 5 coins | (1, 5, 16, 23, 33)<br/>(1, 3, 11, 27, 34)<br/>(1, 4, 9, 24, 35)<br/>(1, 4, 13, 21, 36)<br/>(1, 4, 14, 25, 33) | 3.23<br/>3.34<br/>3.34<br/>3.34<br/>3.34 | (1, 5, 16, 23, 33) | 3.29 |

<sup>1</sup>Averaged over 99 values (1 through 99 cents)

<sup>2</sup>Averaged over 100 values (0 through 99 cents), else identical to if averaged over 99 values

Having verified the implementation, we now will apply our algorithm to solving the clinical-dosing problem. For our first example, we choose a clinical trial that needs to dose patients between 50 mg to 150 mg of drug using combinations of three different strengths. The value of the fourth, fictitious “penalty” tablet is taken as 0.01 mg, i.e., a penalty of 100 tablets per milligram is imposed. Other values of the penalty tablet were also explored (0.05 mg and lower), but the results were identical as long as the penalty was sufficiently high. Furthermore, the smaller manufacturable tablet strength is set as 15 mg; it is presumed that making tablets smaller than 15 mg is not feasible for this drug.

Table 4 presents the top five solutions obtained by applying our clinical-dosing algorithm (Sec. 4). Interestingly, the solutions favor the selection of small-dose tablets, and the top five solutions are similar. The ordering of the solution considers the inclusion of the fictitious penalty tablet (0.01 mg), but in reality, this tablet would not actually be manufactured. The average tablets dosed are also reported after excluding the penalty tablets; the top-five solutions all yield similar values. Practically, any of these solutions are sufficiently good for use in a clinical setting.  

For the best solution obtained (comprised of 15, 16, and 19 mg tablets), Fig. 2 depicts the optimal dosing regime for the dosing range of interest (50 to 150 mg). For each dose, the graph shows the number of tablets of each type that should be administered. For some dose values, e.g., 52, 55, 56, 58, 59, 71, and 74 mg, it is not possible to achieve an exact dose using the tablet strengths.


Table 4:
| Dose range | Top 5 solution and average tablets dosed (DoseValues3 without penalty) |
| --- | --- |
| 50 mg to 150 mg, 3 tablets  (min tablet strength 15 mg) | 15 mg, 16 mg, 19 mg (average 5.821782178217822 tablets)
| `` | 16 mg, 18 mg, 21 mg (average  5.376237623762377 tablets)
| `` | 15 mg, 18 mg, 19 mg (average  5.693069306930693 tablets)
| `` | 15 mg, 17 mg, 20 mg (average  5.584158415841584 tablets)
| `` | 15 mg, 17 mg, 22 mg (average  5.475247524752476 tablets)

![alt text](https://github.com/dylankrish/pfizer-python-clinical-dosing/blob/main/graphs/Table4-1.png?raw=true)

![alt text](https://github.com/dylankrish/pfizer-python-clinical-dosing/blob/main/graphs/Table4-2.png?raw=true)

2.	Effect of dose range on the optimal solution
3 tab version range; range [25, 100, lowestMfg 20]
3 tablet version; range [50, 200, lowestMfg 20] (20, 23, 28 with average 38.34437086092715)
3 tablet version (+penalty); range [100,300, lowestMfg 20] – BASELINE (20 22 27 with average 10.796019900497512)
3 tab version; range [150,400, lowestMfg 20] (20 21 48 with average 9.524793388429751)

![alt text](https://github.com/dylankrish/pfizer-python-clinical-dosing/blob/main/graphs/N2-1.png?raw=true)

![alt text](https://github.com/dylankrish/pfizer-python-clinical-dosing/blob/main/graphs/N2-2.png?raw=true)

![alt text](https://github.com/dylankrish/pfizer-python-clinical-dosing/blob/main/graphs/N2-3.png?raw=true)

![alt text](https://github.com/dylankrish/pfizer-python-clinical-dosing/blob/main/graphs/N2-4.png?raw=true)



*This is a version of the paper in GitHub's .md format for readme. Some parts may be unfinished as this is a draft version. The original file in PDF format will be found in /manuscript/Manuscript - Tablet Clinical Dosing.pdf*