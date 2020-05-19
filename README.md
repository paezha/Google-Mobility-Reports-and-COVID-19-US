
<!-- README.md is generated from README.Rmd. Please edit that file -->

# Using Google Community Mobility Reports to investigate the incidence of COVID-19 in the United States

<!-- badges: start -->

<!-- badges: end -->

Antonio Paez (<paezha@mcmaster.ca>)  
School of Geography and Earth Science  
McMaster University  
1280 Main Street West, Hamilton, ON L8S 4K1  
Canada

(third version of paper after second round of anonymous reviews;
resubmitted to Transport Findings on May 19, 2020)

## Abstract

In 2020 Google released a set of Community Mobility Reports (GCMR).
These reports are based on the company’s location-tracking capabilities
and measure changes in mobility with respect to a baseline. This novel
source of data offers an opportunity to investigate potential
correlations between mobility and incidence of COVID-19. Using data from
the New York Times on COVID-19 cases and GCMR, this paper presents an
analysis of mobility levels and incidence of COVID-19 by state in the
US. The results provide insights about the utility and interpretability
of GCMR for COVID-19 research and decision-making.

## Keywords

  - COVID-19
  - Google Community Mobility Reports
  - Mobility
  - Modelling
  - Regression analysis
  - Policy

# Research Questions and Hypotheses

The main policy tool to control the spread of the COVID-19 pandemic has
been restrictions to non-essential travel in the form of stay-at-home
orders. In the United States, such orders have been implemented on a
state-by-state basis with considerable variations in compliance.
Concurrently, numerous initiatives have been developed to track the
progress and the impact of the pandemic. As a result, there are new
sources of data such as the recently-released Google Community Mobility
Reports (GCMR), as well as The New York Times repository of COVID-19
data. These two open data sets offer novel opportunities to investigate
in quasi-real time the relationship between mobility patterns and
transmission of COVID-19.

This paper investigates the potential of Google Community Mobility
Reports to asses the impact of mobility on the incidence of COVID-19.
The following questions are posed:

  - Do changes in mobility according to GCMR correlate with the
    incidence of COVID-19?
  - And if so, what do we learn about mobility and the spread of the
    disease?

This paper is a reproducible research document. The source is an `R`
markdown file available in a public repository.

# Methods and Data

GCMR use aggregated and anonymized data to chart changes in mobility
with respect to different classes of places (see Table ). Mobility
indicators are calculated based on the frequency and length of visits to
places. The reports give percentage change from a baseline level, which
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

Incidence

</td>

<td style="text-align:left;width: 12em; ">

Total cases of COVID-19 divided by population (in 100,000s)

</td>

<td style="text-align:left;">

0.01

</td>

<td style="text-align:left;">

43.09

</td>

<td style="text-align:left;">

1607.08

</td>

<td style="text-align:left;">

195.46

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

2020-04-05

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

0.65

</td>

<td style="text-align:left;">

1.16

</td>

<td style="text-align:left;">

0.2

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

0.93

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

0.7

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

1.05

</td>

<td style="text-align:left;">

0.19

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

0.98

</td>

<td style="text-align:left;">

1.14

</td>

<td style="text-align:left;">

1.27

</td>

<td style="text-align:left;">

0.07

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
1 is the baseline mobility, and a 0.01 deviation corresponds to a 1%
change. The incubation time of the disease is between 2 and 12 days (95%
interval; see Lauer et al. 2020). Given this, it is to be expected that
any changes in mobility will have a lagged effect on the discovery of
new cases. For this reason, lagged moving averages of the mobility
indicators are calculated. Furthermore, it is possible that mobility and
reports of new cases of COVID-19 are endogenous, if the public adjust
their mobility according to reports of the incidence. Therefore, in
addition to being consistent with an incubation period, use of lagged
indicators also helps to break this potential endogeneity.

The lagged indicators are calculated as the mean of the mobility
indicator using the values from date-minus-12-days to date-minus-2-days.
Furthermore, using the cumulative number of reported COVID-19 cases, the
incidence is calculated after dividing by the population of the state
(in 100,000s). This variable (log-transformed) is paired with the
corresponding lagged moving average of the mobility indicators. The
log-transformation is useful to avoid negative values of incidence when
making predictions. Table  shows the descriptive statistics of the data
set. Analysis is based on correlation analysis, multivariate regression,
and data visualization.

# Findings

Table  shows that the mobility indicators are highly correlated with
each other. Two variables are selected for multivariate analysis: parks-
and work-related mobility. Work has a high correlation with the outcome
variable, and its correlation with parks is relatively weak, which
increases the information content of the two variables in multivariate
analysis. Furthermore, parks- and work-related mobility represent two
dimensions of out-of-home activities: mandatory and discretionary
travel.

<table class="table" style="margin-left: auto; margin-right: auto;">

<caption>

Simple correlation between log(incidence) and the mobility indicators

</caption>

<thead>

<tr>

<th style="text-align:left;">

</th>

<th style="text-align:right;">

log\_incidence

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

log\_incidence

</td>

<td style="text-align:right;">

1.00

</td>

<td style="text-align:right;">

\-0.90

</td>

<td style="text-align:right;">

\-0.76

</td>

<td style="text-align:right;">

\-0.32

</td>

<td style="text-align:right;">

\-0.85

</td>

<td style="text-align:right;">

\-0.92

</td>

<td style="text-align:right;">

0.92

</td>

</tr>

<tr>

<td style="text-align:left;">

retail

</td>

<td style="text-align:right;">

\-0.90

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

0.92

</td>

<td style="text-align:right;">

0.97

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

\-0.76

</td>

<td style="text-align:right;">

0.87

</td>

<td style="text-align:right;">

1.00

</td>

<td style="text-align:right;">

0.43

</td>

<td style="text-align:right;">

0.88

</td>

<td style="text-align:right;">

0.88

</td>

<td style="text-align:right;">

\-0.88

</td>

</tr>

<tr>

<td style="text-align:left;">

parks

</td>

<td style="text-align:right;">

\-0.32

</td>

<td style="text-align:right;">

0.41

</td>

<td style="text-align:right;">

0.43

</td>

<td style="text-align:right;">

1.00

</td>

<td style="text-align:right;">

0.50

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

\-0.85

</td>

<td style="text-align:right;">

0.92

</td>

<td style="text-align:right;">

0.88

</td>

<td style="text-align:right;">

0.50

</td>

<td style="text-align:right;">

1.00

</td>

<td style="text-align:right;">

0.93

</td>

<td style="text-align:right;">

\-0.94

</td>

</tr>

<tr>

<td style="text-align:left;">

work

</td>

<td style="text-align:right;">

\-0.92

</td>

<td style="text-align:right;">

0.97

</td>

<td style="text-align:right;">

0.88

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

0.92

</td>

<td style="text-align:right;">

\-0.98

</td>

<td style="text-align:right;">

\-0.88

</td>

<td style="text-align:right;">

\-0.41

</td>

<td style="text-align:right;">

\-0.94

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

A regression model is estimated with the log of incidence as the
dependent variable. The covariates enter the regression in the form of a
second order polynomial expansion. In addition, the date (centered on
April 5) is introduced to account for the temporal trend of the
pandemic. Finally, an indicator variable for the state of New York is
used to distinguish the unusually high incidence of the disease there.
The results of the model are shown in Table . The model provides a good
fit to the data and all variables reported are significant at \(p<0.10\)
or better.

There is an overall temporal trend that indicates a growing incidence
over time, but at a decelerating rate (see negative sign of date^2).
Mobility related to parks and to work are both associated with higher
incidence of COVID-19, however, the effect of parks-related mobility
grows non-linearly (see positive sign of quadratic term), whereas the
effect of work-related mobility grows at a decreasing rate (see negative
sign of quadratic term). Furthermore, the negative sign for the
interaction of these two mobility indicators captures the trade-offs
between these two forms of mobility and their impact on incidence. The
influence of parks-related mobility was relatively weak early in the
pandemic (negative sign of the parks x date term) but has become more
important over time (positive sign of the parks x date^2 term). The
opposite happens with work-related mobility, the importance of which has
declined over time (negative sign of work x date^2 term), but whose
impact has declined over time (negative sign of work x date^2 term). As
seen in the table, incidence of COVID-19 in New York is consistently
higher.

<table class="table" style="margin-left: auto; margin-right: auto;">

<caption>

Results of estimating regression model. Dependent variable is
log(Incidence).

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

0.1720

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

\-0.0004

</td>

<td style="text-align:center;">

0.4146

</td>

</tr>

<tr>

<td style="text-align:left;">

parks^2

</td>

<td style="text-align:center;">

0.3303

</td>

<td style="text-align:center;">

0.0482

</td>

</tr>

<tr>

<td style="text-align:left;">

parks

</td>

<td style="text-align:center;">

5.6365

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

\-11.0572

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

7.7071

</td>

<td style="text-align:center;">

\<0.001

</td>

</tr>

<tr>

<td style="text-align:left;">

work^2

</td>

<td style="text-align:center;">

\-0.1530

</td>

<td style="text-align:center;">

0.8333

</td>

</tr>

<tr>

<td style="text-align:left;">

parks x date

</td>

<td style="text-align:center;">

\-0.1147

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

0.0051

</td>

<td style="text-align:center;">

\<0.001

</td>

</tr>

<tr>

<td style="text-align:left;">

work x date^2

</td>

<td style="text-align:center;">

\-0.0085

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

1.8810

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

<sup></sup> Coefficient of Determination \(R^2\)= 0.969

</td>

</tr>

<tr>

<td style="padding: 0; border: 0;" colspan="100%">

<sup></sup> Adjusted Coefficient of Determination \(R^2\)= 0.969

</td>

</tr>

<tr>

<td style="padding: 0; border: 0;" colspan="100%">

<sup></sup> Standard Error \(\\sigma\)= 0.68

</td>

</tr>

</tfoot>

</table>

Visualization is the most effective way to understand the trend
according to the mobility indicators and date. Figure  shows the
prediction surfaces on four different dates at intervals of 15 days:
March 21, when the first states began implementing stay-at-home orders;
then April 5, two weeks into the lockdown; this is followed by April 20,
at a time when some states started to consider relaxing stay-at-home
orders; and finally May 5, when some states were reopening and/or
letting stay-at-home orders lapse.

On March 21 there were still only minor departures from the baseline
level of mobility (recall that these are temporally lagged); the
prediction surface at this point is relatively flat. This changes by
April 5, when work-based mobility has declined substantially. Although
every state registers lower work-based mobility, there are large
variations in parks-based mobility, with some states seeing increases of
up to 60% for this class of mobility. By May 5, park-related mobility in
some states had increased to 200% of the baseline.

The prediction surfaces are hyperbolic paraboloids on any given date,
and in general indicate an expectation of higher incidences as either
class of mobility increases, but with a progressively steeper trend for
park-based mobility over time. On the last date examined, May 5, the
trend becomes more steep for park-based mobility, even as this indicator
continues to display large variations from the baseline in both
directions. The white dashed lines in the plots are the folds of the
saddles, and represent, for each date, the combination of parks- and
work-related mobility levels that tended to minimize the incidence.

![Prediction surfaces at three points during the pandemic according to
the model; the dots are a scatterplot of the parks- and work-related
mobility indicators of the states on that date; the white dashed line is
the fold of the saddle.](README_files/figure-gfm/prediction-plots-1.png)

The results suggest that over time the benefits of reduced work-related
mobility can be easily offset by parks-related mobility. For example,
California has consistently registered lower levels of parks-related
mobility whereas Idaho has had high levels of this kind of mobility
throughout the pandemic. The incidence of COVID-19 grew in the
intervening period; however, between March 21 and May 5 growth in
incidence in California was 876.42% whereas Idaho’s growth in incidence
over the same period was 2519.05%.

These results suggest the potential of GCMR to investigate the potential
effects of mobility on the incidence of COVID-19. In particular, growth
appears to be more strongly driven by parks-related mobility. In terms
of the use of these mobility indicators, there are some limitations that
must be acknowledged. The baseline level is not defined in a metric that
is amenable to policy development (e.g., person-km travelled). Without a
clearer understanding of the absolute levels of these variables, these
indicators are useful for inference and perhaps short-term forecasting,
but their potential for applied policy analysis appears to be more
limited.

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
