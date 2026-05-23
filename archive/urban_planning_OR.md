---
title: "Mathematical Modelling in Urban Planning"
layout: inner
output: pdf_document
---

<script type="text/javascript" src="http://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML"></script>
<script type="text/x-mathjax-config"> MathJax.Hub.Config({ tex2jax: {inlineMath: [['$', '$']]}, messageStyle: "none" });</script>

# Mathematical Modelling in Urban Planning: Lowry's Pittsburgh Model

I once read a seminal paper by Ira. S. Lowry about planning a city using math. Lowry proposed one of the first computational city planning models in 1964, while he was working for the US research institute, RAND (an acronym from the phrase <u>R</u>esearch <u>AN</u>d <u>D</u>evelopment). In his time, planners planned the city in parts but there was no way to check how changing one part affected the whole. Lowry's model in contrast, was able to simultaneously balance the city's commercial, transportation and housing constraints. The model, formulated as a set of mathematical equations, was published in [“A Model of Metropolis"](https://www.rand.org/content/dam/rand/pubs/research_memoranda/2006/RM4035.pdf).

It took me several reads to understand what he did but with each read I remember asking myself the same question, "How is it possible to encode multiple city planning objectives using a system of equations?" Well, Lowry did it, but not in the way I thought he would. My intial confusion stemmed from the silly mistake of reading my own wants into the paper. I know. I thought Lowry managed to mathematically optimise what **I'd** want in a city - all the basic needs like housing and cheap transport but also fun things like live music venues. I live in Singapore where people often complain that it's liveable but boring. If you're fortunate enough to be born middle-class here, you'd get your basic needs met...provided you follow the script to get an average job, buy subsidised housing yada yada. In other words, I assumed city planning followed Maslow's hierarchy of needs. You plan for base needs like food and shelter before moving on to higher needs like arts and culture. This was pretty different from the debates about 'gentrification' and 'walkable cities' in my urban studies seminars. I think these topics are still important but it's for another essay. I want to focus on two needs that Lowry addresses in his city planning model - jobs and housing with transportation being the necessary glue that connects the two. 

Lowry based his model on the city of Pittsburgh, which he divided into 1-sq mile pieces that he called 'tracts'. 

<html>
<div style="text-align: center;">
  <img src="/img/blog/urban_planning_or/tracts.png" alt="Description" width="213px" >
</div>
</html>

The area within each tract was a sum of areas for 4 land-use types, as represented by the equation. 

$$
\begin{align}
  A_j &= A_j^U + A_j^B + A_j^R +A_j^H \\
\end{align}
$$

Where,

* $U$: Usable but unoccupied land
* $B$: Basic sector (business)
* $R$: Retail sector (business)
* $H$: Household sector (residential)

**Definitions**

1. **Basic sector.** This category is made up of businesses that primarily serve clients outside the city. This category is designed to be unaffected by the movement of the local population. 

2. **Retail sector.** Similar to (1) but for local facing businesses/services. The implication here is that these businesses/services *would* be affected by shifts in the local population. For instance, fewer households in the tract would mean lesser demand for schools and grocery stores.

3. **Household sector.** The number of households in the area, which the retail sector is dependent on. 

<html>
<div style="text-align: left;">
  <img src="/img/blog/urban_planning_or/tract_composition.png" alt="Description">
  <p><em>(Left: An image from the report with the tract divisions. Right: The three categories plus unoccupied land which is not shown.)</em></p>
</div>
</html>

Lowry is that the planner inputs the location of basic sector businesses, and the model outputs estimates the other two variables - the number of households and retail businesses which should be in each tract. This was done in a “part-waterfall-part-iterative” process. I'll explain what that means. 

The waterfall step is...

The iterative step is...

Then the stop condition is...
[At the end, point forward to the full model which explains the constraints in detail.]

![Flowchart of Experiment A](/img/blog/urban_planning_or/FlowchartA.png)


![Flowchart of Experiment B](/img/blog/urban_planning_or/FlowchartB.png)


![Flowchart of Experiment C](/img/blog/urban_planning_or/FlowchartC.png)

At the start, the model distributes housing across the city based on the location of basic sector workplaces.  After this, the “market potential” of each tract is calculated. Lowry defined “market potential” as the **initial** estimate of how much retail business is required to supply goods and services to households in that tract and surrounding tracts.[^1] People living at the edge of their own tract boundary may choose to visit neighbouring tracts to buy supplies. Up till here, the algorithm can be viewed as a waterfall process. The model distributes the housing based on the basic sector workplaces once. The algorithm then goes into a loop to house the retail workers’ until all constraints . This is based on the humane assumption that retail workers need homes too. 

So, how did Lowry know his model was useful in the real world? The first check was internal consistency. Since this model was a set of math equations, he made sure his system had as many equations as unknowns - a necessary condition to obtain a solution. Second, he validated the model outputs with ground truth data from an earlier dataset collected in 1958. The 1958 dataset contained locations of basic sector businesses, land-use data, distribution of households and retail businesses. Lowry designed three experiments to validate different parts of his model.

Experiment A was a full simulation to validate the entire model - Basic sector business locations were given as inputs and the model output household and retail numbers per tract. Experiment B and C were partial simulations to test the iterative logic of the model. In Experiments B and C, Lowry and his team inserted either real-world housing or retail business figures at the iterative step instead of allowing the model to propose its own solution. This was done to speed up convergence and to see if there was any difference between partially ‘aided’ solution and 'unaided' solution.


<table style="border-collapse: collapse; width: 100%;">
  <tr style="border-bottom: 1px solid black;">
    <th style="border: 1px solid black; padding: 8px;">Experiment</th>
    <th style="border: 1px solid black; padding: 8px;">Input</th>
    <th style="border: 1px solid black; padding: 8px;">Output</th>
  </tr>
  <tr style="border-bottom: 1px solid black;">
    <td style="border: 1px solid black; padding: 8px;">A</td>
    <td style="border: 1px solid black; padding: 8px;">- <b>Basic</b> employment figures<br>- Land use data</td>
    <td style="border: 1px solid black; padding: 8px;">Distribution of retail employment and household figures</td>
  </tr>
  <tr style="border-bottom: 1px solid black;">
    <td style="border: 1px solid black; padding: 8px;">B</td>
    <td style="border: 1px solid black; padding: 8px;">- <b>Basic and Retail</b> figures<br></td>
    <td style="border: 1px solid black; padding: 8px;">Distribution of household figures</td>
  </tr>
  <tr style="border-bottom: 1px solid black;">
    <td style="border: 1px solid black; padding: 8px;">C</td>
    <td style="border: 1px solid black; padding: 8px;">- <b>Retail</b> employment figures<br>- Land use data</td>
    <td style="border: 1px solid black; padding: 8px;">Retail employment & market potential</td>
  </tr>
</table>

<br>

Lowry interpreted the results from the three experiments in two ways. The first was to compare the difference in outputs at a tract-by-tract level. The second was to compare the difference in outputs as a measure of the distance from the centre of the city. 

For the second interpretation, the map below visualises Pittsburgh as a set of concentric circles radiating from the central business district. Lowry referred to them as annular rings.

<i>Pittsburgh represented as annular rings</i>

![Pittsburgh as annular rings](/img/blog/urban_planning_or/annular_rings.png)

There are way too many interesting results to discuss for each experiment so I've chosen a sample of findings for Experiment A only. My goal isn't to list out everything Lowry has found but rather show the potential of an early computation model used in urban planning.


To recap the workflow of Experiment A, the basic sector business locations were given as input and the model outputs the population and retail business estimates per tract. Let's look at how the model's household predictions compared with the 1958 ground truth data. 

  * "Tract-by-tract" interpretation

    * This model explains ~62% of the variation in the tract-level housing figures ($R^2$ is 0.621.) in the city of Pittsburgh.
    * Differences in the model output and real life could be caused by the model's ignorance of the lack of transport infrastructure in some parts of the city. For instance, Lowry observed that the model overestimated the number of households in the Western sector. However, that error turned out to be prediction of future growth in the Western sector of Pittsburgh post-1960, when roads network was significantly improved. Since Lowry wrote the report several years after the 1958 inventory was compiled, he observed that The Fort Pitt Tunnel and a road link to the Greater Pittsburgh Airport led to rapid growth in population and employment in the Western sector.[^2]
  * "Distance-from-centre" interpretation
  ![Experiment A](/img/blog/urban_planning_or/ExA-GroundTruth.png)
    * We are primarily interested in the "3rd Iteration" and "1958 Inventory" lines. The workplaces line is for additional comparison. From visual inspection, the model output of households generally follow ground truth data. However, there are two large deviations around 4,5,6 and 8,9,10 miles away from the central business district. Lowry's explanation for the deviations in Rings 8,9,10 is that those areas were formerly highlands unsuitable for housing and were only recently developed. So even though the land-use was classified as housing, the population has not reached its peak.

We see here that the two interpretations of Lowry's experiments are not always in agreement. For instance, the difference between the numbers in tract-wise varied a lot but when summed at the annular-ring level, they matched closely to the ground truth figures. 

I've included Lowry's formulation below along with some explanation of what each equation does. I've broken up the model roughly into three groups, area ($A$), employment ($E$) and population ($N$). However, there are some interaction equations between the three decision variables that cannot be neatly classified in any group.


**Notations**

| Decision Variable Notations | Meaning                             |
|-----------------------------|-------------------------------------|
| $A$                         | Area of land (thousand square feet) |
| $E$                         | Employment (number of persons)      |
| $N$                         | Population (number of households)   |
| $T$                         | Index of trip distribution          |
| $Z$                         | Constraints                         |
{:.table}


| Super/Subscript Notations | Meaning                                                                                                  |
|---------------------------|----------------------------------------------------------------------------------------------------------|
| $U$                       | Usable Land                                                                                              |
| $B$                       | Basic Sector                                                                                             |
| $R$                       | Retail Sector                                                                                            |
| $H$                       | Household Sector                                                                                         |
| $k$                       | Retail sector/Shopping trip class. Examples of a class include 'Food and Drugs', 'Department Stores' etc. |
| $m$                       | number of classes of retail establishments ($k = 1, ...,m$); Lowry used m=10.                            |
| $i, j$                    | Sub-areas of a bounded region, <u> called tracts </u>                                                    |
| $n$                       | Number of tracts ($i=1,...,n; j =1,...n$)                                                                |
{:.table}


Unspecified functions and coefficients: $a,b,c,d,e,f,g$

$$
\begin{align}
  A_j &= A_j^U + A_j^B + A_j^R +A_j^H \\ \tag{1}
\end{align}
$$

(1) Mentioned in "Model Setup"

$$
\begin{align}
  E^k &= a^k N \tag{2} \\
  E^k &= b^k \left[\sum^n_{k=1} \left(\frac{c^kN_i}{T_{ij}^k} + d^k E_j\right)\right] \tag{3}\\
  E^k  &= \sum^n_{j=1} E^k_j \tag{4} \\
  E_j &= E_j^B + \sum^m_{k=1} E^k_j \tag{5} \\
  A^R_j &= \sum^m_{k=1} e^k E_j^k \tag{6}
\end{align}
$$

(2) $E^k$ denotes the employment for the $k$-th retail sector, a function of how many people live in the area. Lowry supposed that the more people lived in the area, the more employment required in the retail sector. $a^k$ is a weight that represents the market potential of the given location. 

(3) The RHS expresses employment as a measure of distance between homes and shopping areas. $\left(\frac{c^kN_i}{T^k_{ij}}\right)$ is a ratio of the  number of residents in tract $i$ to a score of the distance between tracts $i$ and $j$. It is a confusing but reasonable assumption that people will travel to neighbouring tracts for shopping if it’s not very far away.

(4) The total employment in the $k$-th retail sector is the sum of employment in the $k$-th retail sector in all tracts

(5) Sum of all retail sectors employment and basic sector employment is the total employment for each tract.

(6) Land required for retail is a function of employment figures. More employees = more business = more land for retail needed.

**Household Sector**

$$
\begin{align}
  N &= f \sum^n_{j=1} E_j \tag{7}\\
N_j &= g \sum^n_{i=1} \frac{E_i}{T_{ij}} \tag{8} \\ 
N &= \sum^n_{j=1} N_j \tag{9}\\
\end{align}
$$

(7) The region's population of households is a function of employment. The assumption here is that people can only live if they have jobs to pay for housing.

(8) The number of households in a tract is a function of distance to employment opportunities. The $T_{ij}$ term handles the pair-wise distance between tracts.

(9) Sum of residents in all tracts must equal total population count.

**Constraints**

$$
\begin{align}
E_j^k &\geq Z^k \hspace{0.5cm} \text{, or else $E_j^k=0$} \tag{10} \\
N_j &\leq Z^H_jA^H_j \tag{11} \\
A^R_j &\leq A_j - A^U_j -A^B_j \tag{12}
\end{align} 
$$

(10) Retail employment must hit a minimum number otherwise it will be set to zero. This simulates a minimum economies of scale for retail businesses to be profitable.

(11) The population density of a tract is suppressed by this equation. The greater the accessibility of the tract, the lower the population density.

(12) Amount of land used for retail cannot exceed available land (which equals the sum of land in the $j$-th tract minus all other land use areas)


So, where does that leave us? Well, Lowry has shown that math models of cities can be validated, but only for a snapshot in time, which in his case, was 1958. Although his model did not fully capture the real-life household demand in the city, it's interesting to note that we can still gain new insights from asking questions about the reasons behind the discrepancy. One example was the Western sector housing boom after more roads were added. Towards the end of his report, Lowry suggested some possible limitations to the model:

* The model does not take into account any local peculiarities like ethnic cohesiveness, prestige of location, scenery.

* Retail establishments are shifted around without care of the communities' purchasing power.

* The model did not factor in actual transportation routes, instead it uses airline distance (distance between two points on a sphere) to optimise the distribution of households.

I think modelling a city is interesting because good planning means you're helping to make people's lives easier - shorter commutes, more affordable housing etc. And in recent times, it seems like there's still a lot of uncertainty in urban planning because infrastructure takes many years to build, sometimes decades. It also doesn't help that governments don't always know how people's living patterns will change during the planning and building process. Just take a look the pre-pandemic tax breaks from governments to US companies. It seems a bit wasted given that people prefer working from home these days. But if it's one thing Lowry's model makes clear, it's that planning a city always begins with money.

<html>
<div style="text-align: center;">
  <img src="/img/blog/urban_planning_or/IBM_7090.jpg" alt="Description" width="678" height="540">
  <p><em>A photo of an IBM 7090, the computer model used to run Lowry's simulations.</em></p>
</div>
</html>
# Footnotes

[^1]: Market potential is the linear combination of the number of workplaces and the number of households in that tract and adjoining tract. The coefficients depend on how far the tract is, this applies for adjacent tracts. For workplaces, coefficients are infinite for distances greater than one mile.

[^2]: The [Fort Pit Tunnel](https://en.wikipedia.org/wiki/Fort_Pitt_Tunnel) was opened in 1960, two years after the 1958 dataset was compiled. It was featured in the book and film, "[The Perks of Being a Wallflower.](https://www.youtube.com/watch?v=kMalrBgdRvI)"