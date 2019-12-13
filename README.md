### I. Introduction

In our analysis, we focus on the data from the 1999 war between NATO and Yugoslavia.  This war took place as a result of alleged human rights violations perpetrated by the Yugoslavian state against the predominantly ethnically Albanian population of Kosovo in response to its declaration of independence from Yugoslavia in 1992.  The conflict came to a head as NATO forces decided to begin attacks on Yugoslavia in defense of Kosovo’s Albanian ethnic minority.  This period of conflict lasted from March 1999 to June 1999, and it is over this period that our dataset is focused.  This conflict resulted in the Yugoslav Army’s withdrawal from the region and the United Nations establishing of an Interim Administration Mission in Kosovo.  

Many residents of Kosovo emigrated from the region and many others were killed by violence.  Moreover, the International Criminal of Tribunal of Yugoslavia (ICTY) was established to examine human rights violations perpetrated by the Yugoslav government and by other agents in the region.  Today, Kosovo is a partially recognized state, with Serbia (another state which formerly constituted part of Yugoslavia) claiming Kosovo as an autonomous province.  

We chose this topic because we are interested in better understanding the events of the war and the events surrounding it as a period fraught with human rights violations.  An in-depth understanding of past human rights violations is important to better understand these violations, identify patterns might otherwise be unrecognized, and offer a framework for approaching analysis of conflict and human rights violations in different contexts going forward.  We hope that our analysis and visualizations can serve to highlight important aspects of the Kosovo conflict that might otherwise be unrecognized or underappreciated.  We are especially excited about our interactive visualization tool to help people easily see and understand the distribution of killings and migrations at the time of the conflict.  

### II. Data sources

The data we use in our analysis comes from a variety of sources.  Links to these sources are conveniently compiled by the Human Rights Data Analysis Group to promote further exploration of these datasets in the form its “Kosovo Memory Book”
The data set breaks down into 3 main categories:  1) data on migrations, 2) data on killings, 3) other data (including NATO airstrikes and KLA activity).  Geography is represented by the pcode variable throughout the dataset.  These codes correspond to the 29 municipalities of Kosovo.  

Data for migrations is available from 6 different sources:  border records from the Morina border, border counts from the UN High Commission for Refugees (UNHCR) and Albania’s Emergency Management Group (EMG), and interviews conducted by Physicians for Human Rights (PHR), Human Rights Watch (HRW), Institute for Policy and Legal Studies (IPLS), and the Science and Human Rights Program of the American Association for the Advancement of Science (AAAS).  

The border records for Morina are the most comprehensive data set, with 19126 records ranging from March 28, 1999 to May 28, 1999 and columns corresponding to pcode of origin for the group, date of crossing and size of party.  The UNHCR and EMG records consist only of a single record for each date (March 30, 1999 to May 29, 1999) and a total count of people crossing.  PHR data was collected for 671 households crossing into Albania and another 509 crossing into Macedonia.  These records include numbers of people, date of leaving homes and of crossing the border, municipality codes and location of the interview (Albania or Macedonia).  HRW data reflects the same information for another 123 families interviewed in Albania.  IPLS and AAAS data is collected from lists for 18 differing refugee camps in Albania, accounting for 1837 records of families with number of people, date of crossing, and pcode of origin, and further interviews with 265 families that also indicate the date on which they left their homes and an indication of whether the interview occurred in Bosnia or Albania.  

Data for killings is available from the Science and Human Rights Program of the American Association for the Advancement of Science (AAAS) and covers 4725 records detailing age, sex, pcode of the death, date of the death, and flags for whether the death was reported to each of the ABA, exhumation, HRW, or OSCE.  Some dates are estimated for missing values and indicated with a numerical flag.  NATO airstrike data is obtained from the Human Rights Watch and KLA activity data is obtained from records maintained by the International Criminal Tribunal of Yugoslavia (ICTY) Office of the Prosecutor (OTP).  The former records reflect the date, municipality, and reporting source for 364 different airstrikes.  The latter records reflect either the number of Yugoslav casualties sustained in battle with KLA forces (type=k) or fire exchange between Yugoslav government and KLA forces (type=b), with details on municipality, date, and count, with 410 records total.  

### III. Data transformation

Fortunately, the data we use was available in csv format and links to different sources are conveniently compiled on the HRDAG website.  As such, loading in the data for analysis into R was largely straightforward.  However, we did apply certain transformations to the data for convenience and analysis. 

Some transformation of the data was required for creating our interactive visualization.  Our interactive visualization lets users see aggregated data for each pcode by hovering their mouse over the corresponding location on a map of Kosovo.  To make this possible, we joined data across fields on by pcode and aggregated by summing across dates in the range.  With this transformation, we are easily able to tie these data points to coordinates for display in D3.  


### IV. Missing values



### V. Results

We begin by examining the patterns of migrations from the region of the 3 months period.  This following chart shows the pattern distribution of people migrating via the Morina border over the time period in question.  

It clear that migration occurs in spikes over the course of the NATO airstrikes, rather than occurring smoothly or evenly.  The first such spike occurs in the days immediately after the start of the NATO airstrikes on March 23.  After slowing for two days, migration spikes again for several days after April 1.

Interestingly, large spikes in migration tend to follow significant NATO bombing events.  A large spike on migrations occurs in early May after realtively low migration in the last third of April.  This corresponds time when a NATO aircraft bombed an Albanian refugee convoy, killing 50 people, after mistaking it for a Yugoslav military convoy.  

Another spike occurs just after May 7, when NATO forces completed another high profile attack on the Chinese embassy in Belgrade.  Though no Kosovo Albanians were injured in the attack, the attack drew heavy press coverage an international scrutiny.  

May saw yet another small spike in migration after the NATO airstrike on the Dubrava Prison in Kosovo from May 19 -21.  This resulted in the Dubrava Prison massacre over the following days, where Yugoslav and Serb forces summarily executed at least 99 Kosovo Albanian prisoners in response to the bombings.  

Next, we plot these migratinos against the the count of NATO bombings occurring across days. 

### VII.  Conclusion

To being, we examined the relationship between deaths other variables.  Overall there is high correlation between nsum (total estimated deaths for the two-day period) and lvcnt (estimated total number of people leaving home during this two-day period), and negative corelation between nsum and bomb (number of NATO airstrikes). This indicates that people did not die or leave due to NATO airstrikes.  

nsum and lvcnt seem to be postively correlated to KlaB (number of reported KLA exchanges of fire with Serb authorities).  This confirms one of the theory that killings lead to migration of people. However we need to look more into the reasons that deaths during this period as estimated deaths are negatively correlated to NATO bombings and positively correlated to KLA exchanges of fire.



### VI. Interactive component

[Interactive D3 Grpah](/files/d3.html)







