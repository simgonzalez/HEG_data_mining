<!-- The report must contain : 
Introduction : Expliquer brièvement le problème (TP1-A)
Analyse préliminaire (TP1-A)
Analyse exploratoire. Dans cette section, vous rendrez compte de
votre analyse exploratoire. Elle doit être structurée en deux
parties :
Analyse exploratoire des attributs qualitatifs (TP1-A)
Analyse exploratoire des attributs quantitatifs (TP1-B).
Résumé des principaux résultats et conclusions - factuel, clair et
concis (TP1-A, TP1-B) -->
# HR Report

## Profil that are the most likely to leave

### Introduction

Since the director charged the department with the task of analysing the past and current employee database in order to figure out the raisons that would constitute the departure of a given employee. This report thus presents the results of that analysis.

### Preliminary Analysis

*Combien d'instances et combien de variables (attributs) y a-t-il ?*

There are 10'000 instances and 11 attributes/variables for each instance.

*Quelle est la variable cible et est-elle qualitative ou quantitative ?*

For this analysis the target attribute/variable that we are looking at is the **left** variable. This is a qualitative attribute, that can either take the values 0 or 1, with 0 indicating that the employee has not left the company, and 1 meaning that the employee did.

*Les autres attributs sont-ils quantitatifs ou qualitatifs ?*

| Variable | Description | Type | Why |
| :--- | :--- | :--- | :--- |
| `id` | Employee unique identifier | Quantitative | Tracks records |
| `satisfaction_level` | Employee satisfaction score | Quantitative | Measures morale |
| `last_evaluation` | Employee last evaluation score | Quantitative | Assesses performance |
| `number_project` | Employee number of project is involved in | Quantitative | Gauges workload |
| `average_monthly_hours` | Employee average monthly hours | Quantitative | Measures time input |
| `time_spend_company` | Employee time spent in the company | Quantitative | Tracks tenure |
| `work_accident` | Tells if the employee had a work accident | Qualitative | Records safety |
| `left` | Tells if the employee left the company | Qualitative | Target churn metric |
| `promotion_last_5years` | Tells if the employee was promoted in the past 5 years | Qualitative | Tracks advancement |
| `department` | Employee department | Qualitative | Groups organization |
| `salary` | Employee salary category | Qualitative | Categorizes pay |

*Certaines variables doivent-elles être supprimés de l'analyse ? Pourquoi ?* 

The variables to be eliminated should be:
ID - does not help to answer the question of whether or not an employee has left the company. With the rest of the dataset available, this variables does not add any value to the analysis of dataset.

*Y a-t-il des données manquantes ?*

No, there is no missing data.

*Fournir une description de la variable cible et de sa distribution.*

the distribution is 7619 employees that have not left the company, and 2381 employees that have left the company. This means that 76.19% of the employees have stayed, and 23.81% of the employees have left.


### Exploratory Analysis of Qualitative Attributes
When analysing the distributional probability P(f) of each of the qualitative attributes, after creating a function named allAttributesProbability which calculates probabilities in a general case, without hardcoding the values, we get:
<img width="452" height="381" alt="Screenshot 2026-10-31 at 15 47 38" src="https://github.com/user-attachments/assets/202f404f-2694-4619-a17c-253aed39c69c" />
<img width="401" height="377" alt="Screenshot 2026-10-31 at 15 48 58" src="https://github.com/user-attachments/assets/1a5a3f0e-9b4b-4597-9a8d-3dee835f37df" />
<img width="405" height="383" alt="Screenshot 2026-10-31 at 15 49 27" src="https://github.com/user-attachments/assets/e7020a61-e044-4615-ba28-bc9d87fe9449" />
<img width="396" height="387" alt="Screenshot 2026-10-31 at 15 49 55" src="https://github.com/user-attachments/assets/6419973d-c5d3-484a-8e79-b81af83cf853" />

When calculating the conditional probability of the target variable (which here is the variable *left* ) based on the other qualitative variables, we get the following results:
<img width="401" height="374" alt="Screenshot 2026-10-31 at 16 03 42" src="https://github.com/user-attachments/assets/46b5f79c-4abb-45ad-8957-fa33de5b8594" />
<img width="420" height="384" alt="Screenshot 2026-10-31 at 16 04 27" src="https://github.com/user-attachments/assets/6538c369-d181-4c64-bb3f-926a4dcaad43" />
<img width="377" height="386" alt="Screenshot 2026-10-31 at 16 04 49" src="https://github.com/user-attachments/assets/7e2396a9-56dd-4051-8f19-c030fe5d3853" />
<img width="392" height="381" alt="Screenshot 2026-10-31 at 16 05 08" src="https://github.com/user-attachments/assets/0cfdeea2-474a-45a6-8555-52e5212704a0" />

Once analysing all of the plots created by all of these conditional probabilities, we found 2 columns very relevant that, in our opinion, indicate the raison for a given employee's departure. When analysing the conditional probability P(left | promotion_last_5years) we did notice that amongst all of the employees that have left, the majority did because they did not receive a promotion in the last 5 years, making the column *promotion_last_5years* very important in the matter. The probability of employees that have left is 0.24200633 that did not receive a promotion, compared to the probability of employees that left while received a promotion which is 0.05687204.

Then, by analysing the conditional probability P(left | salary), we noticed that the 2 major groups of employees that left the company were in the low and medium salary categories, making the column *salary* very important for the analysis as well. The probability of employees that have left knowing that they are in a  low salary category is 0.29969294, the probability of employees that have left knowing that they are in a medium salary category is 0.20092486, while the probability of employees that have left knowing that they are in a high salary category is 0.06075949.

An attribute that doesn't seem to affect the target value *left* is the *work_accident*, because when analysing the probability of an employee leaving knowing whether or not they had a work accident, we can see that the probability of an employee who has left knowing that the person had a work accident is 0.08019191, while the probability of an employee leaving knowing that the person did not have a work accident is 0.26507435.



### Exploratory Analysis, Theoretical Questions
For this part of the analysis, we chose to analyse the attribute *promotion_last_5years*.

With the function QualitativeAttrsTheory, we were able to:
1. calculate the joint distribution P(f,y) between the values of the target attribute, (the column *left* ) and the attribute *promotion_last_5years* via the line: jointProbability <- prop.table(table(data[, c(indexTargetVariable, indexFvariable)])) 
2. calculate the marginal distribution  of each of the attributes, based on the joint probability P(f,y). In order to obtain the marginal distribution of promotion in the last 5 years by colSums(jointProbability). What this does is that is calculates all of the values of the column where the probability of promotion is equal to 0, so the value (0.7420+0.2369) as the first column and the column where the probability of promotion is equal to 1, so the value (0.0199+0.0012) and this 2 sums are being added finally.
The same calculation is being dine to obtain the marginal distribution of left via the line rowSums(jointProbability). Here we deal with the sum of all of the lines, so the line where the probability of left is equal to 0, which is (0.7420+0.0199) and the line where the probability of left is equal to 1, which is (0.2369+0.0012), and this 2 sums are being added finally.
3. calculate the conditional distributions P_target_f being P(y|f) and P_f_target being P(f|y). In order to calculate P(y|f) we did the probability where y = 1 (wanted condition) multiplied to f = 1 over the probability of P(f=1).
In order to calculate P(y|f) we did the probability where f = 1 (wanted condition) multiplied to y = 1 over the probability of P(y=1).
All of these calculations are being done inside the body of the function QualitativeAttrsTheory, with every calculation don being preceded by a print that will show the results. Lastly, the instantiation is being done by the line outside the function that reads: QualitativeAttrsTheory("HR_prediction-all.csv", ",", TRUE, 8, 9).

*Utilisez les distributions que vous venez de calculer pour donner un exemple simple du théorème de Bayes.*
In order to do that, we will choose f = promotion_last_5years and y = left. The goal is to use the Bayes theorem in order to calculate whether or not an employee would leave the company, given the fact that they have received a promotion in the last 5 years or not.
In this case, we would calculate this value via ( P(f|y) * P(y) ) / P(F).  
