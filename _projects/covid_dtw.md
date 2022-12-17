---
title: COVID Policy Comparison.
layout: inner
lead_text: A clustering project to compare government responses to Covid.
project_link: 'nicholas-ko-zy.github.io/projects/covid_dtw.html'
---
# Covid-19 Policy Comparison 

_This is a clustering project where I compare the government responses to Covid between countries using [dynamic time warping](https://en.wikipedia.org/wiki/Dynamic_time_warping) - an algorithm which measures the similarity between two signals irrespective of time. The dataset contains information on 229 countries._

## Background
During the course of the pandemic, I had many conversations with people who argued for or against the efficacy of certain COVID-19 policies. I myself also questioned if lockdowns were necessary much later into the pandemic, so I looked to see if the data could give answers to **which policies are useful for reducing COVID fatalities.**

My first task was to cluster countries which experienced similar COVID fatalities. I found a way to cluster countries which experienced similar COVID peaks irrespective of when the peaks came using the dynamic time warping algorithm. The algorithm I implemented is found in many packages but the one I found most useful is the `ts-learn` package because of its clustering function.

My second task was to peer into the clusters and understand what policies were imposed at the time of the COVID peaks.

## Goal

Compare policies between countries that experience similar COVID fatalities (by month).

## Data
The data was obtained from Google COVID-19 data hosted on github. The github page consists of multiple datasets that contain information related to the pandemic like masked movement data, economic data, demographic data. You may find the full data set [here](https://github.com/GoogleCloudPlatform/covid-19-open-data).

For this project I have used the following datasets

<html>
<head>
<style>
table, th, td {
  border: 1px solid black;
  border-collapse: collapse;
}
th, td {
  padding: 7px;
}
th {
  text-align: left;
}
</style>
</head>

<font size = "3">
 <table>
  <tr>
    <th>Dataset</th>
    <th>Content</th>
  </tr>
  <tr>
    <td><code>Index</code></td>
    <td>Various names and codes, useful for joining with other datasets.</td>
  </tr>
  <tr>
    <td><code>Government Response</code></td>
    <td>Government interventions and their relative stringency.</td>
  </tr>
    <tr>
    <td><code>Epidemiology</code></td>
    <td>COVID-19 cases, deaths, recoveries and tests.</td>
  </tr>
    <tr>
    <td><code>Demographic</code></td>
    <td>Various (current) population statistics.</td>
  </tr>
	</table> 
	</font>
</html>

## Implementation
_The full notebook can be found on my [github](https://github.com/nicholas-ko-zy) page._ 

#### Cleaning

I wrote a simple for-loop to iterative calculate the inertia for each n-clusters. 

{% highlight python %}
# Step 1: Initialise empty array to store inertia values
intertia = []

# Step 2: For-loop to get inertia value for n-cluster
# n-values of 1-100
for i in range(101):
 	if i ==0:
     	pass
 	else:
 	# km is the name of the model which we use to cluster the Covid-19 fatality signals.
 	# We specify the parameters of the model in the function TimeSeriesMean.
     	km = TimeSeriesKMeans(n_clusters= i , metric="dtw",
                    	random_state=0, dtw_inertia = True)
     	inertia.append(km.fit(X).inertia_)

# Step 3: Store the log of the inertia values 
# in a dataframe "intertia_df"
inertia_df = pd.DataFrame({
	'inertia' : inertia,
	'inertia_log': np.log(inertia)
}).reset_index()

inertia_df['index'] = inertia_df['index'] + 1
inertia_df = inertia_df.rename(columns = {'index':'n_clusters'})
inertia_df['n_clusters'] = inertia_df['n_clusters'].astype(int)
{% endhighlight %}  

&nbsp;
![Output of inertia dataframe.](/img/covid/inertia_df.png)
_Output of inertia dataframe._

Inertia is a measure of how well a dataset is clustered. A good model aims for a lower inertia score and low number of clusters. For example, a streaming service wants to cluster clients into categories based on what they watch. Too many clusters might make interpreting the preferences of each cluster. Too much inertia and you might still risk over generalising someone as preferring action films when they actually prefer _**80s-**_ action films

#### Clustering
I chose 8 clusters using the elbow method. 

![Elbow method.](/img/covid/elbow.png)
## Results
We observe that there are a few countries which are in a class of their own in terms of fatalities suffered. There are many countries in both cluster 2 and 7 which could warrant further clustering. 

![Elbow method.](/img/covid/clusters.png)

We can observe that countries in cluster 4 are characterised by two Covid peaks. Upon closer inspection, we see that the countries in that cluster really do experience 2-3 Covid peaks during the 33 months (Jan 2020 - Aug 2022). For a full list of all the countries within each cluster, scroll to the bottom.

#### Cluster 8
_Indonesia and the United Kingdom_

Within cluster 8, we have that the peak COVID fatality takes place in the United Kingdom (UK) 7 months earlier than in Indonesia. We see that the peak number of COVID fatalities in both countries is very close after we algined the two signals together (right figure).

<html>
<head>
<style>
* {
  box-sizing: border-box;
}

.column {
  float: left;
  width: 50%;
  padding: 5px;
}

/* Clearfix (clear floats) */
.row::after {
  content: "";
  clear: both;
  display: table;
}
</style>
</head>
<body>
<div class="row">
  <div class="column">
    <img src="/img/covid/cluster_8.png" alt="Cluster 8" style="width:100%">
  </div>
  <div class="column">
    <img src="/img/covid/c8_align.png" alt="Aligned" style="width:100%">
  </div>
</div>

</body>
</html>

With the alignment of the two signals, we can look into the government policies during the months when Indonesia and the UK experienced a surge in Covid fatalities. 

<html>
<head>
<style>
* {
  box-sizing: border-box;
}

.column {
  float: left;
  width: 50%;
  padding: 5px;
}

/* Clearfix (clear floats) */
.row::after {
  content: "";
  clear: both;
  display: table;
}
</style>
</head>
<body>
<div class="row">
  <div class="column">
    <img src="/img/covid/indo.png" alt="Cluster 8" style="width:100%">
  </div>
  <div class="column">
    <img src="/img/covid/uk.png" alt="Aligned" style="width:100%">
  </div>
</div>

</body>
</html>

![](/img/covid/indo_uk.png)

We find that Indonesia and the UK’s COVID policies during the highest peak in COVID fatalities were largely similar. Although Indonesia has historically enact stronger “School Closing” and “Stay at Home” policies while the UK has imposed slightly stricter “Workplace Closing” policies. 

The same analysis can be repeated for countries in other clusters. 

List of countries per cluster:
![](/img/covid/all_countries.png)
