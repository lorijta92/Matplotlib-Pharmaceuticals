# Goal

Analyze the results of an anti-cancer drug treatment trial using Python and the Pandas and Matplotib libraries. Create visualizations showing the following:
* Changes in tumor volume over time
* Number of metastatic sites over time
* Mice survival rate over time
* Total percent change in tumor volume

# Process

The data was held on two separate csv files, so after being read into the notebook, the files were merged into one data frame using an outer join with `pd.merge`. Then, the data was grouped by drug and timepoint, and stored as the object, `grouped_data`. I knew that I wanted to make two separate scatter plots (one for tumor volume, one for metastatic sites) with error bars, so I created two data frames (`mean_grouped_data` and `sem_grouped_data`) from `grouped_data` ready to be called for each scatter plot.

**Tumor Response**

To create the tumor volume plot, I created two new data frames (`mean_tumor_vol` and  `sem_tumor_vol`) by dropping the “Metastatic Sites” columns from the `mean_grouped_data` and `sem_grouped_data` data frames. I then pivoted both data frames, using the timepoints as the index and the drug names as the columns.

Knowing that I would be using the timepoints as the x-values for three of the four plots, I saved the index values (which are the timepoints) of one of the pivoted data frames into the object, `timepoints`.  The y-values were set to the mean tumor volume (accessed through the `mean_tumor_vol` data frame) and the y-error for the error bars set to the standard error (accessed through the `sem_tumor_vol` data frame). I then plotted four lines on the same figure using `plt.errorbar()`. 

![tumor_volume](https://github.com/lorijta92/matplotlib-pharmaceuticals/blob/master/Resources/Images/tumor_response.png?raw=true)

**Metastatic Reponse**

To plot the number of metastatic sites over time, the same procedure for plotting the tumor volumes was repeated, but instead of dropping the “Metastatic Sites” column, I dropped the “Tumor Volume (mm3)” column from the `mean_grouped_data` and `sem_grouped_data` data frames before pivoting. 

![met_sites](https://github.com/lorijta92/matplotlib-pharmaceuticals/blob/master/Resources/Images/metastatic_response.png?raw=true)

**Survival Rate**

For this chart, I wanted to track the number of mice still alive at the end of the study compared to the beginning.  To do this, I counted each mouse by ID using `.count()` on the groupby object, `grouped_data` from earlier and stored that information in a data frame, which I then pivoted in a similar fashion to above. 
To account for the percentage of mice still alive at each timepoint, I divided the count of mice of each timepoint by the first timepoint (index 0) and multiplied by 100. Each “survival rate” was stored in an object to be used when plotting. 

![survival_rate](https://github.com/lorijta92/matplotlib-pharmaceuticals/blob/master/Resources/Images/survival_rates.png?raw=true)

**Total Percent Change**

Before creating a bar chart of the four selected drugs, I first wanted to see the percent change in tumor volumes for all drugs. I stored the first timepoint for all drugs by using `.iloc[0]` on the `mean_tumor_vol` data frame, and stored the last timepoint for all drugs by using `.iloc[-1]` on the same data frame. To calculate the percent change, I subtracted those two objects (`first_timepoint` and `last_timepoint`), divided by `first_timepoint`, multiplied by 100, and stored the result in the object, `pct_change`.

I then stored the four relevant  percent changes in a tuple, and separated the drugs between passing and failing (negative tumor growth vs. positive tumor growth). These values were then plot on a bar chart and labeled using a modified “[autolabel]( http://composition.al/blog/2015/11/29/a-better-way-to-add-labels-to-bar-charts-with-matplotlib/
)” function.  

![summary_bar](https://github.com/lorijta92/matplotlib-pharmaceuticals/blob/master/Resources/Images/summary_bar.png?raw=true)
