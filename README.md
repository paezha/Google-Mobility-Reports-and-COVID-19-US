
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
