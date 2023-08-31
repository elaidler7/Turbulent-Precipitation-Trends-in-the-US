United States Trends in Turbulent Weather
================
Erica Laidler
2023-08-31

**Introduction**

Erratic weather may be the new normal for the children who grow up in
the world today. In the wake of a worsening climate crisis, likely the
most alarming and crucial issue of our generation, we can expect to
experience different conditions than we are used to. Climate change has
already contributed to an array of cascading effects, including more
frequent and extreme storms, heat waves, dry spells, higher sea levels,
and warming oceans (WWF). As time goes on, unless we collectively take
serious systemic measures to slow the effects, it will continue to wreak
havoc on animal habitats and our communities. In this project, I focus
on extreme weather patterns, specifically erratic precipitation in the
United States.

Climate change tends to produce extremes in the temperature, creating
longer, more intense heat spells and more frigid winters. However,
changes in precipitation are also common, including blizzards,
destructive hurricanes, and droughts.

My research goal is to detect notable extremes and pattern changes in US
precipitation over the the last century. I determine which states in the
US have experienced the greatest extremes in precipitation; notably, the
highest precipitation months, lowest precipitation months, and most
dramatic seasonal differences. My second research goal is to determine
if the weather patterns for any of these erratic states have changed
significantly in the last century.

**Findings**

In 2021, Alaska had the highest monthly precipitation measurement, at
28.78 inches. Ten states, including Arizona, California, Colorado, and
Kansas, experienced the lowest precipitation, at 0 inches. Overall, in
2021, Alaska, Oregon, Washington, and Louisiana faced the most dramatic
seasonal changes in precipitation, or the greatest difference between
their highest and lowest recorded monthly precipitation measurements.
The quantitative evidence suggests that Louisiana may have faced a
particularly dramatic increase in extreme weather patterns during the
years since 1895. More research is necessary to determine if this is a
statistically significant trend.

**Data**

This research uses a dataset from the National Centers for Environmental
Information, which provides detailed climate data in the US. It can be
found at: <https://www.ncei.noaa.gov/data/climdiv/> (data set ‘pcpncy’).
This particular data set contains records of the total precipitation
which fell in each state region over the course of every month from 1895
to 2022. It is updated monthly, and published online for public use.

**Tidying the Data**

Structuring the data set into a more useful, easily comprehensible form
required significant tidying. In the original data set, the row names
are coded. Certain digits of the row names represent the state, metric
(temperature or precipitation), and year which are associated with that
particular row.

The data set is not very immediately interpretable. According to the
code book, the first 1-2 digits of the row names represent a state ID
associated with a particular state. The next three digits represent the
state division. The next digit represents the metric (“temperature” or
“precipitation”), and the last four represent the year. In turn, each of
the 12 columns represents a month, and contain the relevant weather
measurements for that row. The elements in the data frame are all of
type double, because they refers to a precipitation measurement (in
inches).

Ultimately, I wanted the final form of the data set to contain 16
columns. The first 12 would remain the 12 months of the year, but the
next columns would be state, division, metric, and year. This format
would make the information about each row much more clear. Further
analysis of the data set will not require extraction of subsets of the
digits of the row names. For instance, to analyze Maryland precipitation
in April, one can refer to the categorical column ‘state’ and subset
based on the Maryland state ID, as opposed to referring to the
cumbersome coded row names.

I made some basic tidying adjustments, and then added four new recoded
columns to the data set. Now the data frame is tidied. The row names are
now simply ID numbers from 1 to 400,636. I added columns for state,
division, metric, year, and metric name. The first 12 columns of each
row contains the precipitation or temperature value from January to
December for a specific state, division, and year. The metric column
tells us the coded number for the metric being reported in that row.
However, it is coded as a number so I included an additional column,
metric_name, which tells us whether the metric represented in that row
is precipitation, average temperature, maximum temperature, or minimum
temperature. I choose to keep the columns state and division coded,
which means that it will be useful to have the legend available during
further analysis of the data set.

I made some final edits upon further exploration of the data. First, I
observed that all the measurements in this particular data set were
precipitation measurements, by finding that every categorical value in
column 15 (‘metric’) was ‘precipitation’. For this reason, I eliminated
the metric and metric_name columns, as they provided no extra
information.

I also removed the rows for 2022. This is because upon exploration of
the dataset, I noticed that these rows contained the value -9.99 for a
lot of their entries. 2022 was the only year for which this happened.
There is no explicit mention of this on the website where the data set
originated, but my guess is that the data was collected mid-way through
2022, meaning that they left a lot of entries with negative default
values.

**Understanding the Data**

The first research goal was to determine which state had the highest
precipitation measurement, lowest measurement, and greatest range in
2021, such that the difference between the measurement for its
highest-precipitation month and its lowest-precipitation measurement in
2021 was the greatest out of all the states.

To find this value, I took three main steps. First, I found the highest
precipitation measurement in all of 2021 for each state. Then, I found
the lowest measurement for each state. Finally, I was able to find the
range of precipitation by calculating the difference between the highest
and lowest precipitation measurements for each state. The state with the
largest of these values had the largest range.

In this paper, a single precipitation measurement refers to the
cumulative precipitation in a particular division over the course of a
month. Thus, the highest measurement of the month for a state is the
highest measurement out of all its divisions for that month.

For the first step, I iterated over the twelve months of data. The list
‘highest’ represents the highest precipitation measurements of the year.
More specifically, it contains 12 lists, which represent the 12 months
of 2021. Each list contains the highest precipitation measurements for
all 49 states for the associated month. In turn, ‘max_val’ is a
49-element list which contains each of the states’ highest recorded
precipitation measurements for the entire year.

For the second step, I found the lowest precipitation measurements. The
list ‘lowest’ represents the lowest precipitation measurements of 2021,
specifically the lowest values for each state in every month. The
min_val vector contains the lowest recorded precipitation measurement
for each of the 49 states in the year 2021.

A few observations are evident. According to the data set, Alaska (state
\#49) has the highest precipitation measurement out of all the states in
the US in 2021, at 28.78 inches. This is followed by Washington (27.09
in), Oregon (20.89), and Louisiana (20.16).

Meanwhile, there are 10 states with the lowest precipitation
measurements, at 0 inches. It is possible for there to be no rain at all
during some months. The 10 states are Arizona, California, Colorado,
Kansas, Nevada, New Mexico, Oklahoma, Oregon, Texas, and Washington.

The third step was to find the state with the highest range in monthly
precipitation, suggesting that their rainy season is the most
drastically different from their dry season.

In the code above, I found that Alaska had the largest range in
precipitation, meaning that the difference between its highest and
lowest recorded precipitation rate was the greatest out of all the
states during 2021, at 28.63 inches.

This was followed by Washington (27.09 in.), Oregon (20.89 in.), and
Louisiana (19.79 in.).

All three of the states with the largest range in precipitation,
especially Alaska and Oregon, have very high annual rates of snowfall.
This could definitely be contributing to the annual precipitation
measurements. Thus, to isolate rainfall as a variable of interest, I
choose to focus on Louisiana, which has a warm climate and tends to
receive less than an inch of snow every year (Snow Climatology).

As mentioned, Louisiana has the fourth largest range in precipitation.
As it turns out, Louisiana has a significant rainy season. In fact,
WorldAtlas.com explains that Louisiana tends to be the second rainiest
state in the entire United States. Hawaii is the first rainiest, but
upon checking the code book Hawaii is the only state which is not
represented in the data set. Overall, the findings of this 2021 study
are in alignment with existing research.

It is also interesting to obtain the mean of the measurements in all the
divisions in Louisiana during 2021. The mean precipitation value for
Louisiana in June 2021 (over all the divisions) was 4.8295 inches. This
suggests that there is a wide range in the amount of precipitation over
the state, as we earlier learned that the highest precipitation
measurement was around 20.16 inches. It seems that there are high-lying
outliers in Louisiana in June.

**Visualizations**

Based on the fact that Louisiana has the fourth highest range in
precipitation over the course of 2021, I wanted to see if there are
particular months during which the rain tends to be much more extreme. I
plot the data for June and November in Louisiana, from 2000 to 2021,
below.

Plot A. Comparison of Rainfall in Louisiana in June and November from
2000 to 2021

    ## Adding missing grouping variables: `year`, `state`
    ## Adding missing grouping variables: `year`, `state`

    ## Warning: Removed 1 rows containing missing values (geom_bar).

![](Turbulent-Prediction-Trends-in-the-Us-Readme_files/figure-gfm/unnamed-chunk-10-1.png)<!-- -->

It does appear that overall there tended to be more Louisiana
precipitation (including more extremely high precipitation measurements)
in June than in November.

Plot A is the product of two simple histograms layered over one another.
One histogram (in light blue) represents November precipitation
measurements in Louisiana from 2000 to 2021. The other histogram (in
pink) represents June precipitation measurements in Louisiana from 2000
to 2021.

I was curious to know whether there would be a visible difference in the
amount of precipitation recorded in June versus in November. June is
historically considered the rainiest month in Louisiana, especially in
certain parts of the state (Weather & Climate). Overall, though there is
not an extreme difference, the June values appear to have a wider range
and a greater right skew, with far more values which lie above the mean
and more large outliers. Previous exploration suggested that Louisiana
was the state for which there was the greatest variation in the amount
of precipitation which fell during different months of the year. Thus,
it makes sense to see that there was a visible difference in the trends
of precipitation recorded during June versus that recorded in November
in Louisiana.

The other research question addresses whether Louisiana has changed its
precipitation patterns in the years since 2000.

Plot B. Rainfall in Louisiana in June from 2000 to 2021

    ## Adding missing grouping variables: `state`

    ## `summarise()` has grouped output by 'year'. You can override using the `.groups` argument.

    ## Adding missing grouping variables: `year`

![](Turbulent-Prediction-Trends-in-the-Us-Readme_files/figure-gfm/unnamed-chunk-11-1.png)<!-- -->

In this plot, I incorporate two variables: precipitation in Louisiana,
and time. More specifically, in the plot above, the x-axis represents
the year, while the y-axis represents June precipitation measurements
collected in that year. The question I was curious to answer was whether
or not there were significant changes in the June precipitation trends
from 2000 to 2021. Based on a general analysis of the plot, it is
possible that Louisiana has had less rain in recent years. However, more
analysis would have to be conducted to verify if this was a significant
difference.

I will also display June data in Louisiana from 1895 to 1910, and then
2000 to 2021, in the form of a facet wrap plot.

    ## Adding missing grouping variables: `state`

    ## `summarise()` has grouped output by 'year'. You can override using the `.groups` argument.

    ## Adding missing grouping variables: `year`

![](Turbulent-Prediction-Trends-in-the-Us-Readme_files/figure-gfm/unnamed-chunk-12-1.png)<!-- -->

Overall, the rainfall in the second period, from 2000 to 2021, seems to
fluctuate at a higher range than that in the period from 1895 to 1916.
In particular, the precipitation in Louisiana patterns seem to be
abnormally high from around 2002 to 2005.

**Reflection**

Weather is not an easy subject of analysis. Though it is relatively
quantifiable, especially precipitation, it is highly variable and
subject to many factors, many of which we are unlikely to assess, like
the temperature of the upper layers of the atmosphere. However, data
analytics makes it possible to synthesize very large amounts of
information and come to useful conclusions.

One of the most challenging parts of this project was the data tidying
process. At the beginning, it is very difficult to know what research
questions will prove to be most relevant. As a result, deciding what
structure the data frame should take is not always obvious. This data
set in particular was far more complex and difficult to interpret than
any other one I have had experience with. In most classes, for instance,
the data set is already tidy, with columns that represent variables and
rows which represent observations. In this data set, not only was this
not the case, but it was also not very clear what should be considered
variables. For instance, it would have been possible to consider ‘month’
as a single categorical variable, with 12 levels, or to consider the
precipitation value for each month as its own observation.

Even after recoding the variables based on the ID’s in the row names, I
ran into challenges. While analyzing the data, for instance, I noticed
that certain outputs did not make sense contextually. Upon further
investigation, I found that this was because the data associated with
2022 contained negative values, and had to be eliminated. Overall, this
and other difficulties taught me that it is very important to thoroughly
familiarize yourself with the data. It is helpful to plot variables as
part of a general exploratory analysis, to get a general sense of their
distributions as well as any outliers.

Debugging is always crucial. It is important never to assume, without
checking, that the code is working properly, even if it is producing an
output which looks reasonable. It is important to debug the code
carefully, verifying at multiple points within each code chunk to make
sure that it is performing the task that you want it to, as well as
ascertaining that none of the outputs look unusual. Especially in
datasets such as this one, in which we are dealing with divisions within
states, and months within years, it is important to verify that the
functions are performing the intended task.

If I were to continue with my work, the natural trajectory would be to
continue to uncover possible trends in erratic weather in certain states
in the US. My project performed preliminary analysis on the trends over
time in Louisiana, which I identified to be one of the states with the
most dramatic highs and lows in precipitation in the country. If I were
to continue with this project, I would isolate other states with large
ranges in precipitation and perform more extensive analysis on the
trends since 1895. It would be very informative to incorporate
statistical analysis and comparisons between the amount of precipitation
in past years and now. Further research, and the implementation of
statistical modeling and regression techniques like support vector
machines, could also yield predictions of future climate and
precipitation changes.

**Conclusion**

My project was based on two main research questions. The first goal was
to identify the states in the US with the most erratic weather in 2021.
I identified Alaska as the state with the highest-precipitation
measurement, at 28.78 inches. Ten states, including Arizona, California,
Colorado, and Kansas, experienced the lowest precipitation, at 0 inches.
I also identified Alaska, Oregon, Washington, and Louisiana as the
states with the greatest range in precipitation, with measurements of
28.78, 27.09, 20.89, and 20.16 inches, respectively.

The second question was to address whether states with erratic weather
patterns have changed over the past years. Through visual depictions of
these changes, I noticed evidence which helps support the possibility
that the precipitation has increased in the years since 1895,
particularly in the very rainy state of Louisiana.

Further research should focus on states with turbulent weather patterns,
to uncover the presence of possible changes which may be associated with
climate change and predictions for future evolution.

**Bibliography**

Data set: “NOAA Monthly U.S. Climate Divisional Precipitation Database.”
National Centers for Environmental Information., 31 Mar. 2022,
<https://www.ncei.noaa.gov/data/climdiv/>.

“Climate and Average Weather Year Round in Alabama.” Weatherspark.com,
<https://weatherspark.com/y/20416/Average-Weather-in-Alabama-New-York-United-States-Year-Round#>:\~:text=The%20chance%20of%20wet%20days,least%200.04%20inches%20of%20precipitation.

Nag, Oishimaya Sen. “The 10 Wettest States in the United States of
America.” WorldAtlas, WorldAtlas, 24 Apr. 2019,
<https://www.worldatlas.com/articles/the-10-wettest-states-in-the-united-states-of-america.html>.

US Department of Commerce, NOAA. “NWS Lix - Snow Climatology.” National
Weather Service, NOAA’s National Weather Service, 17 Nov. 2021,
<https://www.weather.gov/lix/snowcli>.

Wickham, H., & Grolemund, G. (2016). R for data science: Visualize,
model, transform, tidy, and import data. OReilly Media.
