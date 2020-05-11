
<!-- README.md is generated from README.Rmd. Please edit that file -->

# Using Google Community Mobility Reports to investigate the growth of COVID-19 in the United States

<!-- badges: start -->

<!-- badges: end -->

Antonio Paez (<paezha@mcmaster.ca>)  
School of Geography and Earth Science  
McMaster University  
1280 Main Street West, Hamilton, ON L8S 4K1  
Canada

(revised version of paper after anonymous reviews; resubmitted to
Transport Findings on May 11, 2020)

## Abstract

In 2020 Google released a set of Community Mobility Reports (GCMR).
These reports are based on the company’s location-tracking capabilities
and measure changes in mobility with respect to a baseline. This novel
source of data offers an opportunity to investigate potential
correlations between mobility and transmission of COVID-19. Using data
from the New York Times on COVID-19 cases and GCMR, this paper presents
an analysis of mobility levels and new daily reported cases of COVID-19
by state in the US. The results provide insights about the utility and
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
been stay-at-home orders, which in the United States have been
implemented on a state-by-state basis, with considerable variations in
compliance. Concurrently, numerous initiatives have been developed to
track the progress and the impact of the pandemic. As a result, there
are new sources of data such as the recently-released Google Community
Mobility Reports (GCMR), as well as The New York Times repository of
COVID-19 data. These two open data sets offer novel opportunities to
investigate in quasi-real time the relationship between mobility
patterns and transmission of COVID-19.

This paper investigates the potential of Google Community Mobility
Reports to asses the impact of mobility on COVID-19. The following
questions are posed:

  - Do changes in mobility according to GCMR correlate with the
    transmission of COVID-19?
  - And if so, what do we learn about mobility and the spread of the
    disease?

This paper is a reproducible research document. The source is an `R`
markdown file available in a public repository.

# Methods and Data

GCMR use aggregated and anonymized data to chart changes in mobility
with respect to different classes of places (see Table ). Mobility
indicators are calculated based on the frequency and length of visits.
The reports give percentage change from a baseline level, which
corresponds to the median value of mobility of identical days of the
week during the period between January 3 and Feb 6, 2020. Covid-19 data
is compiled by The New York Times based on reports from state and local
health agencies.

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

<td style="text-align:left;width: 12em; ">

New Daily Cases of COVID-19

</td>

<td style="text-align:left;">

0

</td>

<td style="text-align:left;">

90

</td>

<td style="text-align:left;">

12312

</td>

<td style="text-align:left;">

1051.67

</td>

</tr>

<tr>

<td style="text-align:left;">

date

</td>

<td style="text-align:left;width: 12em; ">

Date

</td>

<td style="text-align:left;">

2020-02-27

</td>

<td style="text-align:left;">

2020-04-04

</td>

<td style="text-align:left;">

2020-05-02

</td>

<td style="text-align:left;">

</td>

</tr>

<tr>

<td style="text-align:left;">

retail

</td>

<td style="text-align:left;width: 12em; ">

Mobility trends for places like restaurants, cafes, shopping centers,
theme parks, museums, libraries, and movie theaters

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

0.21

</td>

</tr>

<tr>

<td style="text-align:left;">

groceries

</td>

<td style="text-align:left;width: 12em; ">

Mobility trends for places like grocery markets, food warehouses,
farmers markets, specialty food shops, drug stores, and pharmacies

</td>

<td style="text-align:left;">

0.66

</td>

<td style="text-align:left;">

0.94

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

<td style="text-align:left;width: 12em; ">

Mobility trends for places like local parks, national parks, public
beaches, marinas, dog parks, plazas, and public gardens

</td>

<td style="text-align:left;">

0.36

</td>

<td style="text-align:left;">

1.15

</td>

<td style="text-align:left;">

2.1

</td>

<td style="text-align:left;">

0.26

</td>

</tr>

<tr>

<td style="text-align:left;">

transit

</td>

<td style="text-align:left;width: 12em; ">

Mobility trends for places like public transport hubs such as subway,
bus, and train stations

</td>

<td style="text-align:left;">

0.24

</td>

<td style="text-align:left;">

0.72

</td>

<td style="text-align:left;">

1.14

</td>

<td style="text-align:left;">

0.23

</td>

</tr>

<tr>

<td style="text-align:left;">

work

</td>

<td style="text-align:left;width: 12em; ">

Mobility trends for places of work

</td>

<td style="text-align:left;">

0.34

</td>

<td style="text-align:left;">

0.63

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

<td style="text-align:left;width: 12em; ">

Mobility trends for places of residence

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

For analysis, all mobility indicators are centered so that the value of
1 is the base mobility, and a 0.01 deviation corresponds to a 1% change.
The incubation time of the disease is between 2 and 12 days (95%
interval; see Lauer et al. 2020). Given this, it is to be expected that
any changes in mobility will have a lagged effect (if any) on the
discovery of new cases. For this reason, lagged moving averages of the
mobility indicators are calculated. Furthermore, it is possible that
mobility and reports of new cases of COVID-19 are endogenous, if the
public adjust their mobility according to reports of the incidence.
Therefore, in addition to being consistent with an incubation period,
use of lagged indicators also helps to break this potential endogeneity.

The lagged indicators are calculated as the mean of the mobility
indicator using the values from date-minus-12-days to date-minus-2-days.
Furthermore, using the cumulative number of reported COVID-19 cases, the
total number of new daily cases is calculated. This variable
(log-transformed after adding a small constant) is paired with the
corresponding lagged moving average of the mobility indicators. The
log-transformation is useful to avoid negative values of daily new cases
when making predictions. Table  shows the descriptive statistics of the
data set. Analysis is based on correlation analysis, multivariate
regression, and data visualization.

# Findings

Table  shows that the mobility indicators are highly correlated with
each other. Two variables are selected for multivariate analysis: parks-
and work-related mobility. Work has a high correlation with the outcome
variable, and its correlation with parks is relatively weak, which
increases the information content of the two variables. Furthermore,
parks- and work-related mobility represent two dimensions of out-of-home
activities: mandatory and discretionary travel.

<table class="table" style="margin-left: auto; margin-right: auto;">

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

\-0.48

</td>

<td style="text-align:right;">

\-0.27

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

0.87

</td>

<td style="text-align:right;">

0.41

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

\-0.48

</td>

<td style="text-align:right;">

0.87

</td>

<td style="text-align:right;">

1.00

</td>

<td style="text-align:right;">

0.44

</td>

<td style="text-align:right;">

0.87

</td>

<td style="text-align:right;">

0.87

</td>

<td style="text-align:right;">

\-0.87

</td>

</tr>

<tr>

<td style="text-align:left;">

parks

</td>

<td style="text-align:right;">

\-0.27

</td>

<td style="text-align:right;">

0.41

</td>

<td style="text-align:right;">

0.44

</td>

<td style="text-align:right;">

1.00

</td>

<td style="text-align:right;">

0.49

</td>

<td style="text-align:right;">

0.39

</td>

<td style="text-align:right;">

\-0.41

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

0.49

</td>

<td style="text-align:right;">

1.00

</td>

<td style="text-align:right;">

0.93

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

0.39

</td>

<td style="text-align:right;">

0.93

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

\-0.87

</td>

<td style="text-align:right;">

\-0.41

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

A regression model is estimated with the log of new daily cases as the
dependent variable. The covariates enter the regression in the form of a
second order polynomial expansion. In addition, the date (centered on
April 5) is introduced to account for the temporal trend of the
pandemic. Finally, an indicator variable for the state of New York and
an interaction with the date are used to distinguish the unusually high
incidence of the disease there. The results appear in Table . The model
provides a good fit to the data and all variables reported are
significant at \(p<0.05\) or better.

There is an overall temporal trend that indicates a growing number of
cases over time, at an accelerating rate (see positive sign of date^2).
Mobility related to parks and to work both tend to increase the number
of new cases; the negative sign for their interaction is indicative of
the trade-offs between these two forms of mobility and their impact on
new cases. The influence of parks-related mobility was relatively weaker
early in the pandemic (negative sign of the parks x date term) but has
become more important over time (positive sign of the parks x date^2
term). The opposite happens with work-related mobility, which started
with a greater effect (positive sign of work x date term), but whose
impact has declined over time (negative sign of work x date^2 term) New
York has on average more new daily cases than the rest of the states,
but this has declined over time (see negative sign of NY x date).

<table class="table" style="margin-left: auto; margin-right: auto;">

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

0.1085

</td>

<td style="text-align:center;">

\<0.001

</td>

</tr>

<tr>

<td style="text-align:left;">

date^2

</td>

<td style="text-align:center;">

0.0070

</td>

<td style="text-align:center;">

\<0.001

</td>

</tr>

<tr>

<td style="text-align:left;">

parks^2

</td>

<td style="text-align:center;">

2.1756

</td>

<td style="text-align:center;">

\<0.001

</td>

</tr>

<tr>

<td style="text-align:left;">

parks

</td>

<td style="text-align:center;">

8.9203

</td>

<td style="text-align:center;">

\<0.001

</td>

</tr>

<tr>

<td style="text-align:left;">

parks x work

</td>

<td style="text-align:center;">

\-23.6010

</td>

<td style="text-align:center;">

\<0.001

</td>

</tr>

<tr>

<td style="text-align:left;">

work

</td>

<td style="text-align:center;">

6.3156

</td>

<td style="text-align:center;">

0.0035

</td>

</tr>

<tr>

<td style="text-align:left;">

work^2

</td>

<td style="text-align:center;">

12.0539

</td>

<td style="text-align:center;">

\<0.001

</td>

</tr>

<tr>

<td style="text-align:left;">

parks x date

</td>

<td style="text-align:center;">

\-0.2133

</td>

<td style="text-align:center;">

\<0.001

</td>

</tr>

<tr>

<td style="text-align:left;">

parks x date^2

</td>

<td style="text-align:center;">

0.0079

</td>

<td style="text-align:center;">

\<0.001

</td>

</tr>

<tr>

<td style="text-align:left;">

work x date

</td>

<td style="text-align:center;">

0.1376

</td>

<td style="text-align:center;">

0.0272

</td>

</tr>

<tr>

<td style="text-align:left;">

work x date^2

</td>

<td style="text-align:center;">

\-0.0238

</td>

<td style="text-align:center;">

\<0.001

</td>

</tr>

<tr>

<td style="text-align:left;">

NY

</td>

<td style="text-align:center;">

3.5466

</td>

<td style="text-align:center;">

\<0.001

</td>

</tr>

<tr>

<td style="text-align:left;">

NY x date

</td>

<td style="text-align:center;">

\-0.0527

</td>

<td style="text-align:center;">

\<0.001

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

<sup></sup> Coefficient of Determination \(R^2\)= 0.831

</td>

</tr>

<tr>

<td style="padding: 0; border: 0;" colspan="100%">

<sup></sup> Adjusted Coefficient of Determination \(R^2\)= 0.83

</td>

</tr>

<tr>

<td style="padding: 0; border: 0;" colspan="100%">

<sup></sup> Standard Error \(\\sigma\)= 2.081

</td>

</tr>

</tfoot>

</table>

Visualization is the most effective way to understand the trend
according to the mobility indicators and date. Figure  shows the
prediction surfaces on three different dates: March 21, when the first
states began implementing stay-at-home orders; April 5, fifteen days
later; and April 20, fifteen more days later, and at a time when some
states started to consider relaxing stay-at-home orders.

On March 21 there were still only minor departures from baseline
mobility (recall that these are temporally lagged); the prediction
surface at this point is relatively flat. This changes by April 5, when
work-based mobility has declined substantially. Although every state
registers lower work-based mobility, there are large variations in
parks-based mobility, with some states seeing increases of up to 60% for
this class of mobility.

The prediction surface indicates an expectation of a greater number of
new daily cases as either class of mobility increases, but the effect is
not linear. On the last date examined, April 20, the trend becomes more
steep for park-based mobility, even as this indicator continues to
display large variations from the baseline in both directions. The white
dashed lines in the plots are the folds of the saddles, and represent,
for each date, the combination of parks- and work-related mobility
levels that tended to minimize the emergence of new cases.

![Prediction surfaces at three points during the pandemic according to
the model; the dots are a scatterplot of the parks- and work-related
mobility indicators of the states on that date; the white dashed line is
the fold of the saddle.](README_files/figure-gfm/prediction-plots-1.png)

The results suggest that over time the benefits of reduced work-related
mobility can be offset by parks-related mobility. For example, on March
21, New Jersey and Idaho had similar levels of park-related mobility. By
April 20, New Jersey had substantially reduced park-related mobility,
whereas Idaho’s was even higher than in March. The estimated and actual
number of new cases grew in the intervening period; however, New
Jersey’s growth in new cases (from March 21 to April 20) was only
894%, whereas Idaho’s was 1030%.

These results suggest the potential of GCMR to investigate the spread of
COVID-19, but also point at some limitations. The baseline level is not
defined in a metric that is amenable to policy development (e.g.,
person-km travelled). Without a clearer understanding of the absolute
levels of these variables, their use can suggest trends, but their
potential for applied policy analysis appears to be more limited.

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
