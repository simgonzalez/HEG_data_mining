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
