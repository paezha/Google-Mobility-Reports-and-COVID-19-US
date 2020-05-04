
<!-- README.md is generated from README.Rmd. Please edit that file -->

# Using Google Community Mobility Reports to investigate the growth of COVID-19 in the United States

<!-- badges: start -->

<!-- badges: end -->

## Abstract

In 2020 Google released a set of Community Mobility Reports (GCMR).
These reports are based on the company’s location-tracking capabilities
and measure changes in mobility with respect to a baseline. This novel
source of data offers an opportunity to investigate the way mobility
potentially correlates with transmission of COVID-19. Using data from
the New York Times on COVID-19 cases and GCMR, this paper presents an
analysis of mobility levels and new daily reported cases of COVID-19 by
state in the US. The results provide insights about the utility and
interpretability of GCMR for COVID-19 research and decision-making.

## Keywords

  - COVID-19
  - Google Community Mobility Reports
  - Mobility
  - Modelling
  - Regression analysis
  - Policy

# Research Questions and Hypotheses

The main policy tool to control the spread of the COVID-19 pandemic has
been stay-at-home orders. Concurrently, numerous efforts exist to track
the progress and the impact of the pandemic, and new sources of data
include the recently-released Google Community Mobility Reports
[GCMR](https://www.google.com/covid19/mobility/), as well as The New
York Times repository of [COVID-19
data](https://github.com/nytimes/covid-19-data). These two open data
sets offer novel opportunities to investigate in quasi-real time the
relationship between mobility patterns and transmission of COVID-19.

This paper investigates the potential of Google Community Mobility
Reports to asses the impact of mobility on COVID-19. The following
questions are posed:

  - Do changes in mobility according to GCMR correlate with the
    transmission of COVID-19?
  - And if so, what do we learn about mobility and the spread of the
    disease?

The source document for this paper is an `R` markdown document available
from the following repository:

<https://github.com/paezha/Google-Mobility-Reports-and-COVID-19-US/tree/fcd055878020bf567cd4deabb16c8056189eb45c/Covid-19-Google-CMR-US>

# Methods and Data

GCMR use aggregated and anonymized data to chart changes in mobility
with respect to different classes of places, namely retail and
recreation, groceries and pharmacies, parks, transit stations,
workplaces, and residential. Mobility indicators for each of these
places are reported as percentage changes from a baseline level, which
corresponds to the median value of mobility of identical days of the
week during the period between January 3 and Feb 6, 2020. Covid-19 data
is compiled by The New York Times based on reports from state and local
health agencies.

For analysis, all mobility indicators are centered so that the value of
1 is the base mobility, and a 0.01 deviation corresponds to a 1% change.
The incubation time of the disease has been estimated to be between 2
and 12 days (95% interval) by Lauer et al. (2020). Given this, it is to
be expected that any changes in mobility will have a lagged effect on
the discovery of new cases. For this reason, lagged moving averages of
the mobility indicators are calculated. Furthermore, it is possible that
mobility and reports of new cases of COVID-19 are endogenous, if the
public adjust their mobility according to reports of the incidence.
Therefore, in addition to being consistent with an incubation period,
use of lagged indicators also helps to break this potential endogeneity.

<table class="table" style="margin-left: auto; margin-right: auto;">

<caption>

Descriptive statistics of the data set

</caption>

<thead>

<tr>

<th style="text-align:left;">

Variable

</th>

<th style="text-align:left;">

Definition

</th>

<th style="text-align:left;">

min

</th>

<th style="text-align:left;">

median

</th>

<th style="text-align:left;">

max

</th>

<th style="text-align:left;">

sd

</th>

</tr>

</thead>

<tbody>

<tr>

<td style="text-align:left;">

New Cases

</td>

<td style="text-align:left;">

New Daily Cases of COVID-19

</td>

<td style="text-align:left;">

0

</td>

<td style="text-align:left;">

77.5

</td>

<td style="text-align:left;">

12126

</td>

<td style="text-align:left;">

1059.81

</td>

</tr>

<tr>

<td style="text-align:left;">

date

</td>

<td style="text-align:left;">

Date

</td>

<td style="text-align:left;">

2020-02-27

</td>

<td style="text-align:left;">

2020-04-01

</td>

<td style="text-align:left;">

2020-04-26

</td>

<td style="text-align:left;">

</td>

</tr>

<tr>

<td style="text-align:left;">

retail

</td>

<td style="text-align:left;">

Retail and Recreation

</td>

<td style="text-align:left;">

0.34

</td>

<td style="text-align:left;">

0.66

</td>

<td style="text-align:left;">

1.16

</td>

<td style="text-align:left;">

0.22

</td>

</tr>

<tr>

<td style="text-align:left;">

groceries

</td>

<td style="text-align:left;">

Groceries and Pharmacies

</td>

<td style="text-align:left;">

0.66

</td>

<td style="text-align:left;">

0.95

</td>

<td style="text-align:left;">

1.26

</td>

<td style="text-align:left;">

0.13

</td>

</tr>

<tr>

<td style="text-align:left;">

parks

</td>

<td style="text-align:left;">

Parks

</td>

<td style="text-align:left;">

0.36

</td>

<td style="text-align:left;">

1.15

</td>

<td style="text-align:left;">

1.8

</td>

<td style="text-align:left;">

0.24

</td>

</tr>

<tr>

<td style="text-align:left;">

transit

</td>

<td style="text-align:left;">

Transit Stations

</td>

<td style="text-align:left;">

0.24

</td>

<td style="text-align:left;">

0.73

</td>

<td style="text-align:left;">

1.14

</td>

<td style="text-align:left;">

0.24

</td>

</tr>

<tr>

<td style="text-align:left;">

work

</td>

<td style="text-align:left;">

Workplaces

</td>

<td style="text-align:left;">

0.34

</td>

<td style="text-align:left;">

0.65

</td>

<td style="text-align:left;">

1.07

</td>

<td style="text-align:left;">

0.2

</td>

</tr>

<tr>

<td style="text-align:left;">

residential

</td>

<td style="text-align:left;">

Residential

</td>

<td style="text-align:left;">

0.97

</td>

<td style="text-align:left;">

1.14

</td>

<td style="text-align:left;">

1.27

</td>

<td style="text-align:left;">

0.08

</td>

</tr>

</tbody>

<tfoot>

<tr>

<td style="padding: 0; border: 0;" colspan="100%">

<span style="font-style: italic;">Note: </span>

</td>

</tr>

<tr>

<td style="padding: 0; border: 0;" colspan="100%">

<sup></sup> All mobility indicators are lagged 11-day moving averages

</td>

</tr>

</tfoot>

</table>

The lagged indicators are calculated as the mean of the mobility
indicator using the values from date-minus-12-days to date-minus-2-days.
Furthermore, using the cumulative number of reported COVID-19 cases, the
total number of new daily cases is calculated. This variable
(log-transformed after adding a small constant) is paired to the
corresponding lagged moving average of the mobility indicators. The
log-transformation is useful to avoid negative values of daily new cases
when making predictions. Analysis is based on correlation analysis,
multivariate regression, and data visualization. Table  shows the
descriptive statistics of the data set. Temporal coverage is from
February 27 to April 26, 2020. The maximum number of new daily cases
during this period is 12,126.

# Findings

Table  shows that the mobility indicators are highly correlated with
each other. This is not surprising: it is well-known that mobility
involves time-use trade-offs: the more there is of one kind of mobility,
the less time there is available for any other. That said, the strongest
correlation with the outcome of interest is for residential-based
mobility. To avoid multicollinearity, parks-related mobility is selected
as a covariate since it has the lowest correlation with
residential-based mobility.

<table>

<caption>

Simple correlation between log(New Cases) and the mobility indicators

</caption>

<thead>

<tr>

<th style="text-align:left;">

</th>

<th style="text-align:right;">

log\_new\_cases

</th>

<th style="text-align:right;">

retail

</th>

<th style="text-align:right;">

groceries

</th>

<th style="text-align:right;">

parks

</th>

<th style="text-align:right;">

transit

</th>

<th style="text-align:right;">

work

</th>

<th style="text-align:right;">

residential

</th>

</tr>

</thead>

<tbody>

<tr>

<td style="text-align:left;">

log\_new\_cases

</td>

<td style="text-align:right;">

1.00

</td>

<td style="text-align:right;">

\-0.68

</td>

<td style="text-align:right;">

\-0.47

</td>

<td style="text-align:right;">

\-0.30

</td>

<td style="text-align:right;">

\-0.68

</td>

<td style="text-align:right;">

\-0.69

</td>

<td style="text-align:right;">

0.71

</td>

</tr>

<tr>

<td style="text-align:left;">

retail

</td>

<td style="text-align:right;">

\-0.68

</td>

<td style="text-align:right;">

1.00

</td>

<td style="text-align:right;">

0.86

</td>

<td style="text-align:right;">

0.44

</td>

<td style="text-align:right;">

0.93

</td>

<td style="text-align:right;">

0.98

</td>

<td style="text-align:right;">

\-0.98

</td>

</tr>

<tr>

<td style="text-align:left;">

groceries

</td>

<td style="text-align:right;">

\-0.47

</td>

<td style="text-align:right;">

0.86

</td>

<td style="text-align:right;">

1.00

</td>

<td style="text-align:right;">

0.46

</td>

<td style="text-align:right;">

0.87

</td>

<td style="text-align:right;">

0.87

</td>

<td style="text-align:right;">

\-0.86

</td>

</tr>

<tr>

<td style="text-align:left;">

parks

</td>

<td style="text-align:right;">

\-0.30

</td>

<td style="text-align:right;">

0.44

</td>

<td style="text-align:right;">

0.46

</td>

<td style="text-align:right;">

1.00

</td>

<td style="text-align:right;">

0.52

</td>

<td style="text-align:right;">

0.44

</td>

<td style="text-align:right;">

\-0.45

</td>

</tr>

<tr>

<td style="text-align:left;">

transit

</td>

<td style="text-align:right;">

\-0.68

</td>

<td style="text-align:right;">

0.93

</td>

<td style="text-align:right;">

0.87

</td>

<td style="text-align:right;">

0.52

</td>

<td style="text-align:right;">

1.00

</td>

<td style="text-align:right;">

0.94

</td>

<td style="text-align:right;">

\-0.95

</td>

</tr>

<tr>

<td style="text-align:left;">

work

</td>

<td style="text-align:right;">

\-0.69

</td>

<td style="text-align:right;">

0.98

</td>

<td style="text-align:right;">

0.87

</td>

<td style="text-align:right;">

0.44

</td>

<td style="text-align:right;">

0.94

</td>

<td style="text-align:right;">

1.00

</td>

<td style="text-align:right;">

\-0.99

</td>

</tr>

<tr>

<td style="text-align:left;">

residential

</td>

<td style="text-align:right;">

0.71

</td>

<td style="text-align:right;">

\-0.98

</td>

<td style="text-align:right;">

\-0.86

</td>

<td style="text-align:right;">

\-0.45

</td>

<td style="text-align:right;">

\-0.95

</td>

<td style="text-align:right;">

\-0.99

</td>

<td style="text-align:right;">

1.00

</td>

</tr>

</tbody>

<tfoot>

<tr>

<td style="padding: 0; border: 0;" colspan="100%">

<span style="font-style: italic;">Note: </span>

</td>

</tr>

<tr>

<td style="padding: 0; border: 0;" colspan="100%">

<sup></sup> All mobility indicators are lagged 11-day moving averages

</td>

</tr>

</tfoot>

</table>

A regression model uses the log of new daily cases as the dependent
variable. The covariates are parks- and residential-related mobility,
which enter the regression in the form of a second order polynomial
expansion. In addition, the date is introduced to account for the
temporal trend of the pandemic. Finally, an indicator variable for the
state of New York and an interaction with the date are used to
distinguish the unusually high incidence of the disease there. The
results appear in Table . The model provides a good fit to the data and
all variables reported are significant at \(p<0.05\) or better.

There is an overall temporal trend that indicates a growing number of
cases over time, but at a decreasing rate (see negative sign of date^2).
New York has on average more new daily cases than the rest of the
states, but this has also declined over time (see negative sign of NY x
date). Visualization is the most effective way to understand the trend
according to the mobility indicators and date. Figure  shows the
prediction surfaces on three different dates: March 21, when the first
states began implementing stay-at-home orders; April 5, fifteen days
later; and April 20, fifteen more days later, and at a time when some
states are considering relaxing stay-at-home orders. On March 21 there
were still only minor departures from baseline mobility (recall that
these are temporally lagged); the prediction surface at this point is
relatively flat. This changes by April 5, when large increases in
residential-based mobility are seen, along with greater variations in
parks-based mobility. The prediction surface indicates an expectation of
a greater number of new daily cases as both classes of mobility
increase. On the last date examined, April 20, the trend becomes more
steep for park-based mobility, even as this indicator continues to
display large variations from the baseline in both directions. In
general, higher levels of mobility tend to be associates with a higher
number of new daily cases.

<table>

<caption>

Results of estimating regression model. Dependent variable is log(New
Cases + 0.0001).

</caption>

<thead>

<tr>

<th style="text-align:left;">

Variable

</th>

<th style="text-align:center;">

Coefficient Estimate

</th>

<th style="text-align:center;">

p-value

</th>

</tr>

</thead>

<tbody>

<tr>

<td style="text-align:left;">

date

</td>

<td style="text-align:center;">

7.9619

</td>

<td style="text-align:center;">

\<0.0001

</td>

</tr>

<tr>

<td style="text-align:left;">

date^2

</td>

<td style="text-align:center;">

\-0.0411

</td>

<td style="text-align:center;">

\<0.0001

</td>

</tr>

<tr>

<td style="text-align:left;">

parks^2

</td>

<td style="text-align:center;">

1.4017

</td>

<td style="text-align:center;">

0.0251

</td>

</tr>

<tr>

<td style="text-align:left;">

parks

</td>

<td style="text-align:center;">

\-37.2492

</td>

<td style="text-align:center;">

\<0.0001

</td>

</tr>

<tr>

<td style="text-align:left;">

parks x residential

</td>

<td style="text-align:center;">

39.0902

</td>

<td style="text-align:center;">

\<0.0001

</td>

</tr>

<tr>

<td style="text-align:left;">

residential

</td>

<td style="text-align:center;">

\-286.7568

</td>

<td style="text-align:center;">

\<0.0001

</td>

</tr>

<tr>

<td style="text-align:left;">

residential^2

</td>

<td style="text-align:center;">

249.9901

</td>

<td style="text-align:center;">

\<0.0001

</td>

</tr>

<tr>

<td style="text-align:left;">

parks x date

</td>

<td style="text-align:center;">

\-0.1412

</td>

<td style="text-align:center;">

\<0.0001

</td>

</tr>

<tr>

<td style="text-align:left;">

residential x date

</td>

<td style="text-align:center;">

\-6.9180

</td>

<td style="text-align:center;">

\<0.0001

</td>

</tr>

<tr>

<td style="text-align:left;">

residential x date^2

</td>

<td style="text-align:center;">

0.0365

</td>

<td style="text-align:center;">

\<0.0001

</td>

</tr>

<tr>

<td style="text-align:left;">

NY

</td>

<td style="text-align:center;">

6.9864

</td>

<td style="text-align:center;">

\<0.0001

</td>

</tr>

<tr>

<td style="text-align:left;">

NY x date

</td>

<td style="text-align:center;">

\-0.0557

</td>

<td style="text-align:center;">

0.0022

</td>

</tr>

</tbody>

<tfoot>

<tr>

<td style="padding: 0; border: 0;" colspan="100%">

<span style="font-style: italic;">Note: </span>

</td>

</tr>

<tr>

<td style="padding: 0; border: 0;" colspan="100%">

<sup></sup> Coefficient of Determination \(R^2\)= 0.821

</td>

</tr>

<tr>

<td style="padding: 0; border: 0;" colspan="100%">

<sup></sup> Adjusted Coefficient of Determination \(R^2\)= 0.82

</td>

</tr>

<tr>

<td style="padding: 0; border: 0;" colspan="100%">

<sup></sup> Standard Error \(\\sigma\)= 2.102

</td>

</tr>

</tfoot>

</table>

![Prediction surfaces at three points during the pandemic according to
the model; the dots are scatterplots of the park- and
residential-mobility indicators of the states on that
date.](README_files/figure-gfm/prediction-plots-1.png)

The results suggest the potential of GCMR to investigate the spread of
COVID-19, but also point at some limitations. The baseline level is not
defined in a metric that is amenable to policy development (Google
defines “mobility” as an aggregated score of visits and length of stay
at places.) For example, it is not clear precisely what is residential
mobility: is it visits with friends and relatives, or mobility in the
vicinity of the place of residency? Without a clearer understanding of
these variables, their use can suggest trends, but their potential for
applied policy analysis appears to be more limited.

# References

<div id="refs" class="references">

<div id="ref-Lauer2020incubation">

Lauer, Stephen A., Kyra H. Grantz, Qifang Bi, Forrest K. Jones, Qulu
Zheng, Hannah R. Meredith, Andrew S. Azman, Nicholas G. Reich, and
Justin Lessler. 2020. “The Incubation Period of Coronavirus Disease 2019
(Covid-19) from Publicly Reported Confirmed Cases: Estimation and
Application.” Journal Article. *Annals of Internal Medicine*.
<https://doi.org/10.7326/m20-0504>.

</div>

</div>
