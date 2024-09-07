---
layout: inner
title: Urban Planning and Operations Research
---

# Math and Urban Planning

While I was in Yale-NUS, I took a class called "Smart Cities", a class taught by A/P Chaewon Ahn at Yale-NUS College. When I hear the words, "Smart Cities" I was thinking of 5G network towers or sensors placed around the city to track the movement of traffice or people. So color me surprised to see a linear program formulation of the city in one of my readings earlier on in the class. 

This post is about my takeaways from Lowry's model of the city. Lowry's Pittsburgh model is a linear programming model created to optimise land-use in Pittsburgh Pennsevalnia. Who was Lowry?  I couldn't find much information abut Lowry except that when he developed the Pittsburgh model, he was funded by the Ford Foundation 

**Notations**

| Notation | Meaning                                                     |
|----------|-------------------------------------------------------------|
| $U$      | Usuable Land                                                |
| $B$      | Basic Sector                                                |
| $R$      | Retail Sector                                               |
| $H$      | Household Sector                                            |
| $k$      | retail sector/shopping trip class                           |
| $m$      | number of classes of retail establishments ($k = l, ...,m$) |
| $n$      | number of tracts ($i=l,...,n; j =1,...n$)                   |

Unspecified functions and coefficients:
$a,b,c,d,e,f,g$

Remarks

* A <u>tract</u> is a large area of land. Some of it is unusable for the activities considered in the model. The usuble part is broken down into several land-use types.

**The model**

$$A_j = A_j^U + A_j^B + A_j^R +A_j^H$$

What other people have said about the model...

What I think of the model...