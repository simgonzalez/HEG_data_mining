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
ID - does not help to answer the question of whether or not an employee has left the company.
Satisfaction_level - does not help to answer the question either, even though it seems like it would, this variable is a quantitative one, not a qualitative one, meaning that this variable is not serving of any help.
Last_evaluation - does not help to answer the question, because it is a quantitative variable.
Number_project - does not help to answer the question, because it is a quantitative variable.
Average_monthly_hours - does not help to answer the question, because it is a quantitative variable.
Time_spent_company - does not help to answer the question, because it is a quantitative variable.

Work_accident - could help to answer the question, since it is also a qualitative answer, that has 2 values: 0 meaning that an employee did not have a work accident, and 1 meaning that an employee did.
Promoted_last_5years - could help to answer the question, since it is also a qualitative answer, that has 2 values: 0 meaning that an employee did not receive a promotion in the last 5 years, and 1 meaning that an employee did
Departement - could help answer the question, since it is also a qualitative answer, that gives out an extra information about where did an employee that has left the company had worked.
Qualitative - could help answer the question, since it is also a qualitative answer, that has 3 possible values: low, medium, high. It also represents an information that might have influenced the choice of an employee's departure.

*Y a-t-il des données manquantes ?*

No, there is no missing data.

*Fournir une description de la variable cible et de sa distribution.*

the distribution is 7619 employees that have not left the company, and 2381 employees that have left the company. This means that 76.19% of the employees have stayed, and 23.81% of the employees have left.


### Exploratory Analysis of Qualitative Attributes
Once the conditional probability of the column *left* with the other columns containing qualitative attributes (we are talking here about the column nr.7 work_accident, the column nr.9 promotion_last_5years, the column nr 10 department and the column nr 11 salary), upon analysing all of the plots created by all of these conditional probabilities, we found 2 columns very relevant that, in our opinion, indicate the raison for a given employee's departure. When analysing the conditional probability P(left | promotion_last_5years) we did notice that amongst all of the employees that have left, the majority did because they did not receive a promotion in the last 5 years, making the column *promotion_last_5years* very important in the matter. The probability of employees that have left is 0.24200633 that did not receive a promotion, compared to the probability of employees that left while received a promotion which is 0.05687204.
Then, by analysing the conditional probability P(left | salary), we noticed that the 2 major groups of employees that left the company were in the low and medium salary categories, making the column *salary* very important for the analysis as well. The probability of employees that have left knowing that they are in a  low salary category is 0.29969294, the probability of employees that have left knowing that they are in a medium salary category is 0.20092486, while the probability of employees that have left knowing that they are in a high salary category is 0.06075949.

An attribute that doesn't seem to affect the target value *left* is the *work_accident*, because when analysing the probability of an employee leaving knowing whether or not they had a work accident, we can see that the probability of an employee who has left knowing that the person had a work accident is 0.08019191, while the probability of an employee leaving knowing that the person did not have a work accident is 0.26507435.


### Exploratory Analysis, Theoretical Questions
For this part of the analysis, we chose to analyse the attribute *promotion_last_5years*.

With the function QualitativeAttrsTheory, we were able to:
1. calculate the joint distribution P(f,y) between the values of the target attribute, (the column *left* ) and the attribute *promotion_last_5years* via the lines | jointProbability <- prop.table(table(data[, c(indexTargetVariable, indexFvariable)])) | that in included in the function and takes the values that are not hardcoded inside it. The line | print(jointProbability) | will print the calculated value, so that it will be shown in the result.
2. calculate the marginal distribution  of each of the attributes, based on the joint probability P(f,y). For this, we calculated the variable promo which is the result of the operation colSums(jointProbability) and the variable left which is the result of the operation rowSums(jointProbability). The results are shown via the lines print(promo) and print(left) and the calculations follow the examples seen in class of passing from a joint distribution P(X,Y) to marginal distributions P(X), P(Y).
3. calculate the conditional distributions P(left | promo) and P(promo | left) using the values calculated prreviously be the function in the joint and marginal distributions. So this operation is being done in the variables P_left_promo has the corresponding line |     P_left_promo <- sweep(jointProbability, 2, promo, "/") | and the variable P_promo_left that has the corresponding line |     P_promo_left <- sweep(jointProbability, 1, left, "/") |. These variables are calculated using the sweep function because in the initial attempt to calculate P_left_promo <- jointProbability/colSums(jointProbability), it lead up to getting unexpected results when then doing colSums(P_left_promo) (meaning that the result would not give 1, but other numbers). 
All of these calculations are being done inside the body of the function QualitativeAttrsTheory, with every calculation don being preceded by a print that will show the results. Lastly, the instantiation is being done by the line outside the function that reads: PROBA <- QualitativeAttrsTheory("HR_prediction-all.csv", ",", TRUE, 8, 9).

*Utilisez les distributions que vous venez de calculer pour donner un exemple simple du théorème de Bayes.*
In order to do that, we will choose f = promotion_last_5years and y = left. The goal is to use the Bayes theorem in order to calculate whether or not an employee would leave the company, given the fact that they have received a promotion in the last 5 years or not.
In this case, we would calculate the value P_bayes with the division between P_promo_left * P(left) and P(promo). 
