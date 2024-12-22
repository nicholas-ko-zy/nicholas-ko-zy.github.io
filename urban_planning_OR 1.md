---
layout: inner
title: Urban Planning and Operations Research
use_mathjax: True
---

# Math and Urban Planning: Lowry's Pittsburgh Model

Structure:
Word Count 1200 Words

- Introduction (150 words)
- Model Setup (300 words)
- Comparison with 1958 Inventory (300 words)
- Model Formulation (300 words)
- Takeaways (150 words)

## Introduction
I think urban planning is a unique optimisation problem because it's not immediately clear what you're trying to optimise for. Is it the inhabitants' happiness, the city's GDP or something else? The pragmatist says, "You need money to afford food and shelter." So, you plan for industrial parks and office buildings since no jobs means no money and no money, no food. The artist says, "But we need art and culture too!" So you build galleries and music venues. 

I know what you're thinking. We probably need a bit of everything if we can afford to and if it comes to 'needs' versus 'wants', we'll pick 'needs'. Done right? But what if the trade-offs aren't as clear? How does building a residential area in one part of the city eases or worsens road congestion? How do you know there's enough supermarkets or schools to support a residential area? I think these questions lend themselves well to the use of mathematical optimisation. That's just what economist Ira S. Lowry did in 1964.

Lowry wanted to create a decision making tool for urban planners who were struggling to plan at the city at scale. Up till then, planners planned the city in parts but there was no way to check how much a change in one part affected the whole. Lowry proposed one of the first city planning models that simultaneously considered commerce, transportation and housing. The model, formulated as a set of equations, was published in “A Model of Metropolis", which I'll go through next.


![IBM 7090](/img/blog/urban_planning_or/IBM_7090.jpg)
## Model Setup
Lowry divided the city of Pittsburgh into 1-sq mile pieces he called 'tracts'. There were three land-use types in each tract:

1. **Basic sector.** Businesess that primarily serve clients outside the city. This sector is designed to be unaffected by the movement of the local population. 

2. **Retail sector.** Similar to (1) but local facing. The implication here is that these businesses/services *would* be affected by shifts in the local population. For instance, fewer households in the tract would mean lesser demand for schools and grocery stores.

3. **Household sector.** The number of households in the area, which the retail sector is dependent on. 

![Tract Composition](/img/blog/urban_planning_or/tract_composition.png)

The general idea is that the model takes the location of basic sector businesses as input, and then produces estimates of households and retail businesses numbers for each tract. This was done in a “part-waterfall-part-iterative” process. I'll explain. It’s a waterfall process because the model distribute housing across the city based on the location of existing workplaces. After this, the “market potential” of each tract is calculated. Lowry defined “market potential” as the **initial** estimate of how much retail business is required to supply goods and services for a tract and its neighbouring tracts. The algorithm then iteratively settles the retail workers’ housing in the tracts until the system reaches a predefined equilibrium state. This is based on the humane assumption that retail workers need homes too. 

## Comparison with 1958 inventory
How did Lowry know his model made any sense? Well, the first check was internal consistency. Since this model was a set of math equations, he also checked that the system had as many equations as unknowns - a necessary but insufficient condition for a solution. The next was to check ground truth data, which he had from an earlier study in 1958. In fact he designed three experiments so that the model would output data that could be compared to the 1958 ground truth values. The 1958 data contained locations of basic sector businesses, land-use data, distribution of households and retail businesses. The three experiments validate different assumptions for different parts of the model.

[insert flowchart with graph on how goes in and out]

| Experiment | Input                                                | Output                                                  |
|------------|------------------------------------------------------|---------------------------------------------------------|
| A          | - **Basic** employment figures  <br> - Land use data | Distribution of retail employment and household figures |
| B          | - **Basic and Retail** figures  <br>                 | Distribution of household figures                       |
| C          | - **Basic** employment figures  <br> - Land use data | Retail employment & market potential*                    |

Experiment A tested for the entire waterfall plus iterative process of the model, while Experiment B and C tested for different but related parts of the iterative process.

In Experiment A, the model was given real-world location of basic sector businesses as input and the model would generate household and retail figures and geographic distribution on its own. Experiment B tested for the convergence of household distribution if real-world retail business data from the 1958 inventory was used instead of the algorithm's solution. Experiment C was similar to Experiment B but instead of feeding the model retail data, real-world household data was provided to observe the convergence of the retail business distribution.



Lowry gave an example of how deficiencies in the model can lead to observation of inefficient distribution of households. And of p.104 to p.105 - the model overestimated the number of households in the Western sector. The source of residents - CBD workers. At the present time that Lowry was writing, he observed the rapid growth of the Western sector’s population once road networks connected the sector to the CBD : the fort Pitt tunnel (look for a news article?)

But the building of such infrastructure to reduce the asymmetry of the city requires many years, even decades. Even more b reason to use planning tools? Or is it high risk and planners should adopt a wait and see approach?

Check out fit of the graph that compares the iterative solution with 1958 inventory, p.110 linear regression shows model’s distribution of households accounts for 62% of variance in the inventory of households in 1958. The remaining discrepancies, Lowry thinks, are due to lag phenomena, people don’t move as quickly to catch up with markets, and transportation accessibility biases.

Market potential def: p.112 a linear combination of the number of workplaces and the number of households in that tract and adjoining tract. The coefficients depend on how far the tract is, this applies for adjacent tracts. For workplaces, coefficients are infinite for distances greater than one mile.

Idea for result summary:
• show the 1958 vs 3rd iteration graph for each output:
    • experiment A,B,C
• have one interesting takeaway for the comparisons between 1958 and 3rd iteration values, I.e rapid growth of western sectors population once transportation was improved because the model used airlines distance

## Model Formulation

**Notations**

| Decision Variable Notations | Meaning                             |
|-----------------------------|-------------------------------------|
| $A$                         | Area of land (thousand square feet) |
| $E$                         | Employment (number of persons)      |
| $N$                         | Population (number of households)   |
| $T$                         | Index of trip distribution          |
| $Z$                         | Constraints                         |

| Super/Subscript Notations | Meaning                                                     |
|---------------------------|-------------------------------------------------------------|
| $U$                       | Usuable Land                                                |
| $B$                       | Basic Sector                                                |
| $R$                       | Retail Sector                                               |
| $H$                       | Household Sector                                            |
| $k$                       | retail sector/shopping trip class                           |
| $m$                       | number of classes of retail establishments ($k = l, ...,m$) |
| $i, j$                    | sub-areas of a bounded region, <u> called tracts </u>       |
| $n$                       | number of tracts ($i=l,...,n; j =1,...n$)                   |

Unspecified functions and coefficients:
$a,b,c,d,e,f,g$

$$
\begin{align}
  A_j &= A_j^U + A_j^B + A_j^R +A_j^H \\
\end{align}
$$

Explanation:

(1) The area of the $j$-th tract is a sum of the following land-use types:

* Usable sector (unoccupied)
* Basic sector (business)
* Retail sectors (business)
* Househould sectors (residential)

$$
\begin{align}
  E^k &= a^k N \\
  E^k &= b^k \left[\sum^n_{k=1} \left(\frac{c^kN_i}{T_{ij}^k} + d^k E_j\right)\right] \\
  E^k  &= \sum^n_{j=1} E^k_j \\
  E_j &= E_j^B + \sum^m_{k=1} E^k_j \\
  A^R_j &= \sum^m_{k=1} e^k E_j^k
\end{align}
$$
Explanation:

(2) $E^k$ denotes the employment for the $k$-th retail sector, a function of how many people live in the area. Lowry supposed that the more people lived in the area, the more employment required in the retail sector. $a^k$ is a weight that represents the market potential of the given location. 

(3) The RHS expresses employment as a measure of distance between homes and shopping areas. $\left(\frac{c^kN_i}{T^k_{ij}}\right)$ is a ratio of the  number of residents in tract $i$ to a score of the distance between tracts $i$ and $j$. It is a confusing but reasonable assumption that people will travel to neighbouring tracts for shopping if it’s not very far away.

(4) The total employment in the $k$-th retail sector is the sum of employment in the $k$-th retail sector in all tracts

(5) Sum of all retail sectors employment and basic sector employment is the total employment for each tract.

(6) Land required for retail is a function of employment figures. More employees = more business = more land for retail needed.

**Household Sector**

$$
\begin{align}
  N &= f \sum^n_{j=1} E_j \\
N_j &= g \sum^n_{i=1} \frac{E_i}{T_{ij}} \\ 
N &= \sum^n_{j=1} N_j \\
\end{align}
$$

(7) The region's population of households is a function of employment. The assumption here is that people can only live if they have jobs to pay for housing.

(8) The number of households in a tract is a function of distance to employment opportunities. The $T_{ij}$ term handles the pair-wise distance between tracts.

(9) Sum of residents in all tracts must equal total population count.

**Constraints**
$$
\begin{align}
E_j^k &\geq Z^k \hspace{0.5cm} \text{, or else $E_j^k=0$} \\
N_j &\leq Z^H_jA^H_j \\
A^R_j &\leq A_j - A^U_j -A^B_j
\end{align} 
$$

(10) Retail employment must hit a minimum number otherwise it will be set to zero. This simulates a minimum economies of scale for retail businesses to be profitable.

(11) The population density of a tract is suppressed by this equation. The greater the accessibility of the tract, the lower the population density.

(12) Amount of land used for retail cannot exceed available land (which equals the sum of land in the $j$-th tract minus all other land use areas)

## Takeaways 

Lowry has shown that it is logical and promising to built a large urban model
by first considering the bottom-line: Money from basic sector cascades down to drive demand for housing and retail businesses.
[conclusion is weak, try to give it some punch consider adding so what and next steps sentences that b draw links to the results he already found]


- p.101 gives two paras on why divergence might be expected from the model formulation and real world data 
    - Model does not take into account any local peculiarities like ethnic cohesiveness, prestige of location, scenery
    - Retail establishments are shifted around without care for the purchasing power of purchasing power
    - Model did not factor in actual transportation routes, instead it uses airline distance (distance between two points on a sphere) to optimise the distribution of households.
    - Lowry said his aim is to explore the source of source of their discrepancy not to explain away the differences. His long term goal is to covert this “snapshot” model into a dynamic model that is updated with new variables. To do so, he needs to know what new variables to add.


# Surplus paragraphs
Afterword: I first came across Lowry's Pittsburg Model in an urban studies class at Yale-NUS College. The class was "Smart Cities", taught by Assistant Professor Chaewon Ahn. The model stuck with me till now because I’d never thought I’d see math equations in an urban studies class. I decided to revisit the model out of curiosity.


Fig 89, 90 - annular rings to show how households change the further you go from the CBD. We can see from fig 90 that the differences are small. Lowry himself said that convergence was fast. Households were added in the 2nd and 3rd iteration to house retail workers. And retail establishments in the core, once added, push people out from the CBD. Not everyone in the city works in the basic sector.

CBD is ring 1

P.92 table 1 - last two rows show comparison with 1958 inventory. 
P.93 Lowry’s team had to move the CBD by hand to overcome the job constraints. The wanted to make it denser.

Experiment B is compared with experiment A. They wanted to test the logic of the iterative process. So they fixed retail employment in experiment B. But A had the loop with changing retail and household distributions.

Result. P.97 close match, R^2 of .972

Partial equilibrium - results of B and C

P.101 for output of experiment A and the 1958 inventory. Second para has some useful takeaways for a “what next?” ending of this post. 

Introduction 150


Takeaways 150


I first came across Lowry's Pittsburg Model in an urban studies class at Yale-NUS College. The class was called "Smart Cities" and was taught by Assistant Professor Chaewon Ahn. The model stuck with me till now because I’d never thought I’d see math equations in an urban studies class. I decided to revisit the model out of curiosity. 

How does Lowry validate his model? Comparisons with 1958 inventory, see distribution of households from CBD figure on p.110. Start from page 87.

First experiment - test for the robustness of the iterative method. The output of the final iteration compared against ground truth values in the 1958 dataset.

Second and third - note that the output is the input for experiment A. But how does this add value? I see, so that he can get the output for each variable without fear of one of the processed data breaking the iterative process. For instance, Lowry wanted to check convergence for household data. Then he leapfrogged the process by feeding actual retail data so that the algorithm will converge on some household distribution. The same is done for the prediction of retail employment and market potential. The iterative process is sped up and in some cases guaranteed convergence since one of your processed data (household & retail) is fixed.
