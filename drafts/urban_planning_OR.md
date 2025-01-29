---
layout: inner
title: Urban Planning and Operations Research
---

# Math and Urban Planning: Lowry's Pittsburgh Model

## Introduction
In the 1960s, computational tools for urban planning were beginning to be used for transportation simulations. Lowry's Pittsburgh model was an attempt to extend transportation models to include people and businesses in the city. His model of the mathematical city of Pittsburgh, Pennsylvania was first published in the 1964 report, "A Model of Metropolis". It is formulated as a linear programming problem, solved on an IBM 7090.

[Insert picture of an IBM 7090]

I first came across Lowry's Pittsburg Model in an urban studies class at Yale-NUS College. The class was called "Smart Cities", taught by Assistant Professor Chaewon Ahn. I remember my surprise when I saw a linear programming (LP) problem outside of my math classes. In the past few weeks, I decided to revisit the model out of curiosity.

Lowry's conclusion was that

## Background
The model accounts for 420 square miles of land, 1.5 million people and 550,000 jobs in the city of Pittsburgh. The images below show a comparison between a hand-drawn map of Pittsburgh, from  "A Model of Metropolis" and a 2024 OpenStreetMap screenshot. From right to left, you can see the Allegheny River and the Monongahela River combine into the Ohio River on the left.

[Insert HTML graphic]

<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta http-equiv="X-UA-Compatible" content="ie=edge">
  <title>Before/After Slider</title>
  <style>
    html, body {
      margin: 0;
      padding: 0;
    }
  </style>
  <script src="../dist/slider.bundle.js"></script>
</head>
<body>
  <h1 style="text-align: center;">Before/After Slider @Vincent Fong</h1>
  <p style="text-align: center;">Slider1: [ width: 90%, height: 400px, line: false ]</p>
  <div id="slider1"></div>
  <p style="text-align: center;">Slider2: [ width: 70% ]</p>
  <div id="slider2"></div>


  <script>
    window.onload = function() {
      new SliderBar({
        el: "#slider1",
        beforeImg: 'https://cdn.jsdelivr.net/gh/VincentTV/VincentTV-Code@master/Before%26After-Slider/before.jpg',
        afterImg: 'https://cdn.jsdelivr.net/gh/VincentTV/VincentTV-Code@master/Before%26After-Slider/after.jpg',
        width: "90%",
        height: "400px",
        line: false
      });
      new SliderBar({
        el: "#slider2",
        beforeImg: 'https://cdn.jsdelivr.net/gh/VincentTV/VincentTV-Code@master/Before%26After-Slider/before.jpg',
        afterImg: 'https://cdn.jsdelivr.net/gh/VincentTV/VincentTV-Code@master/Before%26After-Slider/after.jpg',
        width: "70%",
      })
    }
  </script>
</body>
</html>


Lowry had two outcomes for his model. First, to validate the social physical theories about how businesses interacted with and residential areas. Second, to demonstrate how a computational model could forecast the impact of city planning policies on land-use. Social physics is the field which seeks to explain social behaviour the same way a physicists explains natural phenomea using mathematics and experiments. For example, [gravity model]...

[Screenshot of Lowry's social physical math equations]


## Inputs and Outputs

The city of Pittsburgh was cut up into smaller 1 square mile chunks which Lowry named 'tracts'. Each tract is approximately 1 square mile or about half the size of the Downtown Core in Singapore. 

[Insert photo of comparison of one tract and half of downtown core of Singapore]

Lowry then divided the land into three categories:

1. **Basic sector.** This inclues industrial, business and administrative establishment of non-locals. This sector is designed in the model to be unaffected by the local markets. 

2. **Retail sector.** Similar to (1) but more local facing. The implication is that these businesses would be more affected by shifts in the local population. Schools and department stores count as 'retail'.

3. **Household sector.** The number of households in the area, in which the retail sector is dependent on. 

[Insert image of the three sectors]

The inputs of the model are

1. Geographic distribution of basic sector emplyoment
2. Amount of space occupied by basic sector establishments
3. Constraints imposed on land because of physical characteristics (i.e. terrain)
4. Constraints imposed on land because of legal restrictions  

The model's outputs are estimates of the following metrics 

1. Retail employment
2. Residential population
3. Ideal land use for currently unoccupied tracts

[Insert image of a flow chart]

Lowry used 1958 data collected from a previous study as input for this model. Since the data was painstakingly collected through interviews and surveys, he acknowledged that it was not possible for him to validate the model over time. The only thing he did was to be able to test the model's output against the 1958 distribution of residences and workplaces in Pittsburgh. As such, Lowry's model was only a proof-of-concept. 

[Any attempts on validation?] Comparisons with 1958
 
asdfasdfasdfha;sedjfh



## The Decision Making Algorithm
_"What is the optimal distribution of people and businesses in the city given some population and travel patterns?"_

**Process of formulating model**

Lowry proposed the following iterative algorithm to arrive at the optimal value of the 3 outputs above.

1. Start with any tract
2. Distribute a residential population around workplaces to supply labour. This assumes workplaces get their workers from the residences within a certain distance from it. This is supported by 1958 work travel data.
3. Tracts adjacent to major employment centers will be unable to accomodate all the households (i.e. the central business distract area cannot house every worker) so overflow is reassigned to other tracts.
4. Add "population serving activities" like department stores and elementary schools to serve residents.
5. The number of workers required to meet the demand from residents in each tract is calculated.
6. Since retail workers also need housing, the algorithm loops to recalculate the distribution of the residential population and demand for retail. In other words, retail workers need homes and places to buy things also.
7. Steps 1-6 are repeated for all tracts until an equilibrium is achieved. The equilibrium will be a balanced amount of residences and work places in each tract such that there are sufficient workers and each residential area is not overcrowded.

> Summary: Start with distributing places of employment in the city then move on to add residential areas around for the workers to live in. After this, add in the retail sector which serves the nearby residential population. Loop this process until an equilibrium is achieved.

## The Objective Function

Every linear program has an objective function that is either minimised or maximised. In Lowry's model of Pittsburgh...

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

* Usable sector
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

(2) $E^k$ denotes the employment for the $k$-th retail sector, which is a function of how many people live in the area. Lowry supposed that the more people living in the area, the more employment required in the retail sector. $a^k$ is a weight that represents the market potential of the given location. 

(3) This equation looks slightly more complex. It is a measure of the employment in the $k$-th retail sector, in the $j$-th tract of Pittsburgh. The RHS expresses employment as a measure of distance between homes and shopping areas. A key takeaway is the summation of the term $\left(\frac{c^kN_i}{T^k_{ij}}\right)$ which is a ratio of number of residents in tract $i$ to a score of the distance betweeen tracts $i$ and $j$. It is a bit confusing, but makes sense given the assumptions that people will travel to neighbouring tracts if the distance between tracts is sufficiently small. If the distance between tracts $i$ and $j$ is small enough, the model will assign higher employment in tract $j$. This is because more workers are required in tract $j$ since more people from tract $i$ might travel to tract $j$ to do their shopping. Similarly $E_j$ will increase if there are many people living in tract $i$, it assumes there will be 'spillover shopping' in other tracts.

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

(10) Retail employment must hit a minimum number otherwise it will be set to zero. 

(11) The population density of a tract is suppressed by this equation. The greater the accessibility of the tract, the lower the population density.

(12) Amount of land used for retail cannot exceed available land (which equals the sum of land in $j$-th tract minus all other land use areas)

Assuming the inequalities are ignored, Lowry has done the math to show that this formualation is a square system (meaning the number of equations equals the number of unknowns). I am going to take his word for it. So there exists a solution to this system.

## Results of Experiments
Add in next week

## Takeaways 

Lowry has shown that it is logical and promising to built a large urban model
by considering the bottom-line: Money which comes from businesses in the basic sector - aka businesses that are not affected by local population, i.e. banks, consulting, shipping businesses, product businesses etc. As long as your business's P/L will not be affected by shifts in local population, you're in the basic sector.
Then only from there, do you then calculate the distribution of residences and retail sector land in the area (on a high level). Validate using comparisons with the 
 
Then any discrepancy in the model and ground-truth figures will give you insight into the following areas

1. Transportation patterns of people
2. Retail consumption patterns of people

haha - course of history moves according to chagnes in modes of production and exchange. 

Things to explore again
* Systemic way to build a city model
