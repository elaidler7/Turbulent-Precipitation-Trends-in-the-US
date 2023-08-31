R Notebook
================

\#DATA IMPORT climate \<- read.csv(“\~/Desktop/Frequently Accessed /All
Folders/Transfer to backup for Umass Google docs/School/Graduate
School/Y1S2/DACSS 601/Final Project/climate.csv”) print(head(climate))

\#DATA TIDYING

\#Change the column names to represent the months of the year: climate2
\<- climate colnames(climate2) = c(“a”, “January”, “February”, “March”,
“April”, “May”, “June”, “July”, “August”, “September”, “October”,
“November”, “December”)

climate3 \<- climate2 library(tidyverse) climate3 \<- climate3 %\>%
add_row(“a” = 01001011895, “January” = 7.03, “February” = 2.96, “March”
= 8.36, “April” = 3.53, “May” = 3.96, “June” = 5.40, “July” = 3.92,
“August” = 3.36, “September” = 0.73, “October” = 2.03, “November” =
1.44, “December” = 3.66, .before = 1)

\#Change the row names: \#(1) Establish the row names. climate4 \<-
climate3 rownames(climate4) = climate4$a

\#(2) Eliminate the now unnecessary column a. climate5 \<-
climate4\[,-(1),drop=FALSE\]

\#Create function that counts the number of digits in a number. nDigits
\<- function(x) nchar(trunc(abs(x)))

\#In the code below, I discovered that row names have either 10 or 11
digits.This is because the state ID’s at the beginning have either 1 or
2 digits, depending on whether it is single- or double-digit. sum_10 = 0
sum_11 = 0 for(i in 1:nrow(climate5)){ if
(nDigits(as.numeric(rownames(climate5)\[i\])) != 10){ sum_11 = sum_11 +
1 } if (nDigits(as.numeric(rownames(climate5)\[i\])) != 11){ sum_10 =
sum_10 + 1 } }

\#Confirm that all of the state ID’s are either 1 or 2 digits. sum_10
represents number of single-digit state IDs; sum_11 is number of
double-digit state ID’s. (sum_11 + sum_10) == nrow(climate5)

\#Now we have confirmed that the state ID’s either have 1 or 2 digits,
and hence that the row names have either 10 or 11 digits total. We need
to deal with both of these cases.

\#Create 4 columns: state, division, metric, or year based on the
codings in the row names, and taking into account the cases when the row
names are either 10 or 11 digits.

\#Contains the number of digits for each row name. name_num =
nDigits(as.numeric(rownames(climate5)))

climate_try \<- climate5 %\>% mutate(state = ifelse(name_num == 11,
as.numeric(substr(rownames(climate5), 1,2)),
as.numeric(substr(rownames(climate5), 1,1))), division = ifelse(name_num
== 11, as.numeric(substr(rownames(climate5), 3,5)),
as.numeric(substr(rownames(climate5), 2,4))), metric = ifelse(name_num
== 11, as.numeric(substr(rownames(climate5), 6,7)),
as.numeric(substr(rownames(climate5), 5,6))), year = ifelse(name_num ==
11, as.numeric(substr(rownames(climate5), 8,11)),
as.numeric(substr(rownames(climate5), 7,10))))

climate6 \<- climate_try

\#Add another column to explicitly describe what metric number codings
1, 2, 27, and \#28 refer to. climate7 \<- climate6 %\>%
mutate(metric_name = case_when( metric == 1 \~ “Precipitation”, metric
== 2 \~ “Average Temperature”, metric == 27 \~ “Maximum Temperature”,
metric == 28 \~ “Minimum Temperature” ))

\#Rename row names to simply be ID numbers from 1 to 400,636. climate8
\<- climate7 rownames(climate8) = 1:400636 head(climate8)

\#Check to see how many non-precipitation measurements there are. sum6
\<- sum(((climate8\[,15\] != 1)))

\#Remove the metric and metric_name columns. climate9 \<-
subset(climate8, select = -c(metric, metric_name))

\#Remove all rows which are associated with 2022. climate_new
\<-subset(climate9, year !=“2022”) climate \<- climate_new

\#Check to see if there are any more rows with negative values.
sum(climate9\[1:12,\] \<0)

\#UNDERSTANDING THE DATA

\#Finding Maximum Values (The Highest Monthly Precipitation
Measurement):

\#Initialize the vector ‘highest’.  
highest \<- rep(0, 12)

\#In loop below, we will establish the vector ‘highest’. This vector
contains 12 lists, which represent the 12 months in 2021. In each list,
there are 49 values for the 49 states. Each of the 49 values represent
the highest precipitation measurement recorded in the state that month,
out of all the divisions in the state.

\#For instance, the first list in ‘highest’ is ‘January’. The first
value in January is associated with state \#1 (Alabama) and has a
measurement of 3.19. This means that the maximum precipitation value
recorded in Alabama (state \#1) during January 2021 was 3.19.

climate_t \<- climate

library(tidyverse) climate10 \<- c()

for(i in 1:12){ climate10 \<- climate_t %\>% group_by(state) %\>%
arrange(desc(climate_t\[,i\])) %\>% filter(year == 2021) %\>% slice(1)
%\>% ungroup() %\>% select(state, i) highest\[i\] \<- climate10\[,2\] }

\#Now that we have found the highest precipitation measurement for each
month for every state, I want to find the maximum value of the whole
year for every state.

\#The vector max_val will contain 49 values, each of which contains the
highest precipitation measurement recorded in one of the 49 states over
the course of 2021. max_val \<- c(0, 49) for (i in 1:49){ max_val\[i\] =
max(highest\[\[1\]\]\[i\], highest\[\[2\]\]\[i\], highest\[\[3\]\]\[i\],
highest\[\[4\]\]\[i\], highest\[\[5\]\]\[i\], highest\[\[6\]\]\[i\],
highest\[\[7\]\]\[i\], highest\[\[8\]\]\[i\], highest\[\[9\]\]\[i\],
highest\[\[10\]\]\[i\], highest\[\[11\]\]\[i\], highest\[\[12\]\]\[i\])
}

\#Finding Minimum Values

\#Initialize the vector ‘lowest’. lowest \<- rep(0, 12)

\#Below I complete the vector ‘lowest’, which has the same structure as
the vector ‘highest’ in the chunk of code above, except that it
represents the minimum values rather than the maximum values.

climate_t \<- climate

library(tidyverse) climate10 \<- c()

for(i in 1:12){ climate10 \<- climate_t %\>% group_by(state) %\>%
arrange(climate_t\[,i\]) %\>% filter(year == 2021) %\>% slice(1) %\>%
ungroup() %\>% select(state, i) lowest\[i\] \<- climate10\[,2\] }

\#Now I create the vector min_val, which is similar in structure to
max_val in the code chunk above, except now it contains the minimum
values. min_val \<- c(0, 49) for (i in 1:49){ min_val\[i\] =
min(lowest\[\[1\]\]\[i\], lowest\[\[2\]\]\[i\], lowest\[\[3\]\]\[i\],
lowest\[\[4\]\]\[i\], lowest\[\[5\]\]\[i\], lowest\[\[6\]\]\[i\],
lowest\[\[7\]\]\[i\], lowest\[\[8\]\]\[i\], lowest\[\[9\]\]\[i\],
lowest\[\[10\]\]\[i\], lowest\[\[11\]\]\[i\], lowest\[\[12\]\]\[i\]) }

lowest

\#Highest precipitation measurement in 2021: max(max_val)

\#State ID associated with highest precipitation measurement:
which.max(max_val)

\#Second highest precipitation measurement - Washington:  
sort(max_val,partial=48)\[48\]

\#Third highest precipitation measurement - Oregon:
sort(max_val,partial=47)\[47\]

\#Fourth highest precipitation measurement - Louisiana:
sort(max_val,partial=46)\[46\]

\#Lowest precipitation measurement in 2021: min(min_val)

\#Number of states which have a precipitation measurement of 0:
sum(min_val == 0)

\#State ID’s for states that had precipitation measurements of 0 in 2021
zero \<- c() for (i in 1:49){ if (min_val\[i\] == 0){ zero \<-
append(zero, i) } } print(“State ID’s associated with precipitation
measurements of 0 in 2021:”) print(zero)

\#Largest range in precipitation: max(max_val-min_val)

\#State associated with largest range in precipitation: ”
which.max(max_val-min_val) range \<- max_val - min_val

\#Second highest range in precipitation : sort(range, partial=48)\[48\]
\#Third highest range in precipitation : sort(range, partial=47)\[47\]
\#Fourth highest range in precipitation : sort(range, partial=46)\[46\]

climate_s \<- climate

climate_s %\>% filter(state == 16) %\>% summarize(mean = mean(June))

\#VISUALIZATIONS

\#Plot A \#Create a data frame which is the result of subsetting on data
from November 2021. nov_louis \<- climate9 %\>% group_by(year,
state)%\>% filter(state == ‘16’, (year == ‘2021’ \|\| year == ‘2020’
\|\| year == ‘2019’ \|\| year == ‘2018’ \|\| year == ‘2017’ \|\| year ==
‘2016’ \|\| year == ‘2015’ \|\| year == ‘2014’ \|\| year == ‘2013’ \|\|
year == ‘2012’ \|\| year == ‘2011’ \|\| year == ‘2010’\|\| year ==
‘2009’ \|\| year == ‘2008’ \|\| year == ‘2007’ \|\| year == ‘2006’ \|\|
year == ‘2005’ \|\| year == ‘2004’\|\| year == ‘2003’\|\| year ==
‘2002’\|\| year == ‘2001’\|\| year == ‘2000’)) %\>% select(‘November’)

nov_louis$month = "November" nov_louis$precipitation =
nov_louis$November nov_louis \<- subset(nov_louis, select = c(‘month’,
‘precipitation’))

\#Create a data frame which is the result of subsetting on data from
June 2021. june_louis \<- climate9 %\>% group_by(year, state)%\>%
filter(state == ‘16’ & (year == ‘2021’ \|\| year == ‘2020’ \|\| year ==
‘2019’ \|\| year == ‘2018’ \|\| year == ‘2017’ \|\| year == ‘2016’ \|\|
year == ‘2015’ \|\| year == ‘2014’ \|\| year == ‘2013’ \|\| year ==
‘2012’ \|\| year == ‘2011’ \|\| year == ‘2010’\|\| year == ‘2009’ \|\|
year == ‘2008’ \|\| year == ‘2007’ \|\| year == ‘2006’ \|\| year ==
‘2005’ \|\| year == ‘2004’\|\| year == ‘2003’\|\| year == ‘2002’\|\|
year == ‘2001’\|\| year == ‘2000’)) %\>% select(‘June’)

june_louis$month = "June" june_louis$precipitation = june_louis$June
june_louis \<- subset(june_louis, select = c(‘month’, ‘precipitation’))

\#Bind the November and June data frames together. june_nov_df \<-
rbind(june_louis, nov_louis)

\#Assign colors. colors = c(June=“pink”, November=“lightblue”)

\#Plot the data as 2 histograms laid over each other, with a legend.
plotA = ggplot(june_nov_df, aes(precipitation, fill=month)) +
theme_minimal() + geom_bar(stat = “count”) +
scale_fill_manual(values=colors) + ylim(0, 10) + labs(title = “Louisiana
Precipitation in June and November (2000-2021)”, x = “Precipitation
Measurements”) plotA

\#Plot B june_louis \<- climate9 %\>% group_by(year, state)%\>%
filter(state == 16) %\>% select(‘year’, ‘June’) %\>% summarize(mean =
mean(June)) %\>% select(mean)

june_louis_v \<- june_louis$mean

year \<- as.factor(2000:2021)

june_louis_df \<- data.frame(year, june_louis_v\[1:22\])

plotB \<- ggplot(june_louis_df, aes(x= year, y =
june_louis_v\[1:22\])) + geom_bar(stat = “identity”) + labs(x= “Year”, y
= “Mean Precipitation value in June”, title = “June Precipitation
Measurements in Louisiana from 2000 to 2021”) + theme(axis.text.x =
element_text(angle = 90, vjust = 0.5, hjust = -1)) plotB

\#Plot C library(tidyverse) june_louis \<- climate9 %\>% group_by(year,
state)%\>% filter(state == 16) %\>% select(‘year’, ‘June’) %\>%
summarize(mean = mean(June)) %\>% select(mean)

june_louis_v \<- june_louis$mean

year_list1 \<- c(1895:1916) year_list2 \<- c(2000:2021) year_f1 \<-
as.factor(year_list1) year_f2 \<- as.factor(year_list2)

june_louis_v1 \<- june_louis_v\[1:22\] june_louis_v2 \<-
june_louis_v\[105:126\]

june_louis_df1 \<- data.frame(year = year_f1, precipitation =
june_louis_v1, category = “Era 1”) june_louis_df2 \<- data.frame(year =
year_f2, precipitation = june_louis_v2, category = “Era 2”)

june_louis_df \<- rbind(june_louis_df1, june_louis_df2)

ggplot(data = june_louis_df, aes(year, precipitation)) + geom_bar(stat =
“identity”) + labs(x= “Year”, y = “Mean Precipitation Value in June”,
title = “June Precipitation Measurements in Louisiana Across Eras”) +
theme(axis.text.x = element_text(angle = 90, vjust = 0.5, hjust = -1)) +
facet_wrap(\~ category, scales = “free_x”)
