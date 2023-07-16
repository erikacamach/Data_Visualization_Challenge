# Data_Visualization_Challenge
Pymaceuticals Inc.
Analysis
Add your analysis here.
# Dependencies and Setup
import matplotlib.pyplot as plt
import pandas as pd
import scipy.stats as st
import numpy as np

# Study data files
mouse_metadata_path = "data/Mouse_metadata.csv"
study_results_path = "data/Study_results.csv"

# Read the mouse data and the study results
mouse_metadata = pd.read_csv(mouse_metadata_path)
study_results = pd.read_csv(study_results_path)

# Combine the data into a single DataFrame
combined_data = pd.merge(study_results, mouse_metadata, how="left")

# Display the data table for preview
combined_data.head()
Mouse ID	Timepoint	Tumor Volume (mm3)	Metastatic Sites	Drug Regimen	Sex	Age_months	Weight (g)
0	b128	0	45.0	0	Capomulin	Female	9	22
1	f932	0	45.0	0	Ketapril	Male	15	29
2	g107	0	45.0	0	Ketapril	Female	2	29
3	a457	0	45.0	0	Ketapril	Female	11	30
4	c819	0	45.0	0	Ketapril	Male	21	25
# Checking the number of mice.
mice=combined_data["Mouse ID"].value_counts()
number_of_mice=len(mice)
number_of_mice
249
# Our data should be uniquely identified by Mouse ID and Timepoint
# Get the duplicate mice by ID number that shows up for Mouse ID and Timepoint. 
all_duplicate_mouse_id = pd.DataFrame(duplicate_mice)
print(all_duplicate_mouse_id)
      0
0  g989
# Optional: Get all the data for the duplicate mouse ID. 
duplicate_df = combined_data.loc[combined_data["Mouse ID"]=="g989"]
duplicate_df
Mouse ID	Timepoint	Tumor Volume (mm3)	Metastatic Sites	Drug Regimen	Sex	Age_months	Weight (g)
107	g989	0	45.000000	0	Propriva	Female	21	26
137	g989	0	45.000000	0	Propriva	Female	21	26
329	g989	5	48.786801	0	Propriva	Female	21	26
360	g989	5	47.570392	0	Propriva	Female	21	26
620	g989	10	51.745156	0	Propriva	Female	21	26
681	g989	10	49.880528	0	Propriva	Female	21	26
815	g989	15	51.325852	1	Propriva	Female	21	26
869	g989	15	53.442020	0	Propriva	Female	21	26
950	g989	20	55.326122	1	Propriva	Female	21	26
1111	g989	20	54.657650	1	Propriva	Female	21	26
1195	g989	25	56.045564	1	Propriva	Female	21	26
1380	g989	30	59.082294	1	Propriva	Female	21	26
1592	g989	35	62.570880	2	Propriva	Female	21	26
# Create a clean DataFrame by dropping the duplicate mouse by its ID.
clean_df = combined_data.drop_duplicates(subset='Mouse ID')
clean_df
Mouse ID	Timepoint	Tumor Volume (mm3)	Metastatic Sites	Drug Regimen	Sex	Age_months	Weight (g)
0	b128	0	45.0	0	Capomulin	Female	9	22
1	f932	0	45.0	0	Ketapril	Male	15	29
2	g107	0	45.0	0	Ketapril	Female	2	29
3	a457	0	45.0	0	Ketapril	Female	11	30
4	c819	0	45.0	0	Ketapril	Male	21	25
...	...	...	...	...	...	...	...	...
245	t565	0	45.0	0	Capomulin	Female	20	17
246	i557	0	45.0	0	Capomulin	Female	1	24
247	m957	0	45.0	0	Capomulin	Female	3	19
248	f966	0	45.0	0	Capomulin	Male	16	17
249	m601	0	45.0	0	Capomulin	Male	22	17
249 rows × 8 columns

# Checking the number of mice in the clean DataFrame.
clean_mice = clean_df["Mouse ID"].value_counts()
clean_number_of_mice = len(clean_mice)
clean_number_of_mice
248
Summary Statistics
# Generate a summary statistics table of mean, median, variance, standard deviation, and SEM of the tumor volume for each regimen
regimen_df = combined_data.groupby("Drug Regimen")
tumor_mean = regimen_df["Tumor Volume (mm3)"].mean()
tumor_median = regimen_df["Tumor Volume (mm3)"].median()
tumor_var = regimen_df["Tumor Volume (mm3)"].var()
tumor_stdev = regimen_df["Tumor Volume (mm3)"].std()
tumor_SEM = regimen_df["Tumor Volume (mm3)"].sem()

# Use groupby and summary statistical methods to calculate the following properties of each drug regimen: 
# mean, median, variance, standard deviation, and SEM of the tumor volume. 
# Assemble the resulting series into a single summary DataFrame.
summary_df = pd.DataFrame([tumor_mean,tumor_median,tumor_var,tumor_stdev,tumor_SEM]).T
summary_df.columns = ["Tumor Volume Mean",
                      "Tumor Volume Median",
                      "Tumor Volume Variance",
                      "Tumor Volume Std. Dev.",
                      "Tumor Volume Std. Err."]
summary_df
Tumor Volume Mean	Tumor Volume Median	Tumor Volume Variance	Tumor Volume Std. Dev.	Tumor Volume Std. Err.
Drug Regimen					
Capomulin	40.675741	41.557809	24.947764	4.994774	0.329346
Ceftamin	52.591172	51.776157	39.290177	6.268188	0.469821
Infubinol	52.884795	51.820584	43.128684	6.567243	0.492236
Ketapril	55.235638	53.698743	68.553577	8.279709	0.603860
Naftisol	54.331565	52.509285	66.173479	8.134708	0.596466
Placebo	54.033581	52.288934	61.168083	7.821003	0.581331
Propriva	52.320930	50.446266	43.852013	6.622085	0.544332
Ramicane	40.216745	40.673236	23.486704	4.846308	0.320955
Stelasyn	54.233149	52.431737	59.450562	7.710419	0.573111
Zoniferol	53.236507	51.818479	48.533355	6.966589	0.516398
# A more advanced method to generate a summary statistics table of mean, median, variance, standard deviation,
# and SEM of the tumor volume for each regimen (only one method is required in the solution)
aggregations = {
    "Tumor Volume (mm3)": ["mean", "median", "var", "std", "sem"]
}
# Using the aggregation method, produce the same summary statistics in a single line
summary_stats = combined_data.groupby("Drug Regimen").agg(aggregations)
summary_stats
Tumor Volume (mm3)
mean	median	var	std	sem
Drug Regimen					
Capomulin	40.675741	41.557809	24.947764	4.994774	0.329346
Ceftamin	52.591172	51.776157	39.290177	6.268188	0.469821
Infubinol	52.884795	51.820584	43.128684	6.567243	0.492236
Ketapril	55.235638	53.698743	68.553577	8.279709	0.603860
Naftisol	54.331565	52.509285	66.173479	8.134708	0.596466
Placebo	54.033581	52.288934	61.168083	7.821003	0.581331
Propriva	52.320930	50.446266	43.852013	6.622085	0.544332
Ramicane	40.216745	40.673236	23.486704	4.846308	0.320955
Stelasyn	54.233149	52.431737	59.450562	7.710419	0.573111
Zoniferol	53.236507	51.818479	48.533355	6.966589	0.516398
Bar and Pie Charts
# Generate a bar plot showing the total number of rows (Mouse ID/Timepoints) for each drug regimen using Pandas.

total_datapoints = combined_data['Drug Regimen'].value_counts()
total_datapoints_plot = total_datapoints.plot(kind='bar')

total_datapoints_plot.set_xlabel('Drug Regimen')
total_datapoints_plot.set_ylabel('# of Observed Mouse Timepoints')
total_datapoints_plot.set_title('Total Data Points for Each Treatment')

plt.xticks(rotation=45)
plt.tight_layout()
plt.show()

# Generate a bar plot showing the total number of rows (Mouse ID/Timepoints) for each drug regimen using pyplot.
total_datapoints = combined_data['Drug Regimen'].value_counts()

x_axis = total_datapoints.index
y_axis = total_datapoints.values

plt.bar(x_axis, y_axis, color='b', alpha=0.75)
plt.xticks(rotation='vertical')
plt.xlabel('Drug Regimen')
plt.ylabel('# of Observed Mouse Timepoints')
plt.title('Total Data Points for Each Treatment')

plt.tight_layout()
plt.show()

# Generate a pie plot showing the distribution of female versus male mice using Pandas
gender_df = combined_data['Sex'].value_counts()

gender_plot = gender_df.plot(kind='pie', autopct='%1.1f%%', fontsize=12, figsize=(5, 5))
gender_plot.set_ylabel('')
gender_plot.set_title('Gender Distribution')

plt.axis('equal')  # Ensure the pie chart is circular

plt.show()

# Generate a pie plot showing the distribution of female versus male mice using pyplot
labels = ["Male", "Female"]
count = [gender_df["Male"], gender_df["Female"]]

plt.pie(count, labels=labels, autopct="%1.1f%%")
plt.ylabel("")

plt.title("Distribution of Female versus Male Mice")
plt.axis("equal")

plt.show()

Quartiles, Outliers and Boxplots
# Calculate the final tumor volume of each mouse across four of the treatment regimens:  
# Capomulin, Ramicane, Infubinol, and Ceftamin

# Start by getting the last (greatest) timepoint for each mouse
treatment_regimens = ["Capomulin", "Ramicane", "Infubinol", "Ceftamin"]
filtered_df = combined_data[combined_data["Drug Regimen"].isin(treatment_regimens)]
last_timepoint = filtered_df.groupby("Mouse ID")["Timepoint"].max()
last_timepoint_df = pd.DataFrame({"Timepoint": last_timepoint})

# Merge this group df with the original DataFrame to get the tumor volume at the last timepoint
tumor_volume_df = pd.merge(last_timepoint_df, filtered_df, on=["Mouse ID", "Timepoint"])
tumor_volume_df.head()
Mouse ID	Timepoint	Tumor Volume (mm3)	Metastatic Sites	Drug Regimen	Sex	Age_months	Weight (g)
0	a203	45	67.973419	2	Infubinol	Female	20	23
1	a251	45	65.525743	1	Infubinol	Female	21	25
2	a275	45	62.999356	3	Ceftamin	Female	20	28
3	a411	45	38.407618	1	Ramicane	Male	3	22
4	a444	45	43.047543	0	Ramicane	Female	10	25
# Put treatments into a list for for loop (and later for plot labels)
# Create empty list to fill with tumor vol data (for plotting)
treatments = ["Capomulin", "Ramicane", "Infubinol", "Ceftamin"]
tumor_vol_cap = []
tumor_vol_ram = []
tumor_vol_inf = []
tumor_vol_cef = []

# Calculate the IQR and quantitatively determine if there are any potential outliers. 
  # Locate the rows which contain mice on each drug and get the tumor volumes
  # add subset
  # Determine outliers using upper and lower bounds
outlier_list = []
for drugs in (treatments):
    tumor_regiment = tumor_volume_df.loc[tumor_volume_df["Drug Regimen"]==drugs,"Tumor Volume (mm3)"]
    outlier_list.append(tumor_regiment)
    quartiles = tumor_regiment.quantile([.25,.5,.75])
    lowerq = quartiles[0.25]
    upperq = quartiles[0.75]
    iqr = upperq-lowerq
    lower_bound = lowerq - (1.5*iqr)
    upper_bound = upperq + (1.5*iqr)
    outlier = tumor_regiment.loc[(tumor_regiment< lower_bound) | (tumor_regiment > upper_bound)]
    print(f"{drugs}'s potential outliers:{outlier}")
  
    
Capomulin's potential outliers:Series([], Name: Tumor Volume (mm3), dtype: float64)
Ramicane's potential outliers:Series([], Name: Tumor Volume (mm3), dtype: float64)
Infubinol's potential outliers:15    36.321346
Name: Tumor Volume (mm3), dtype: float64
Ceftamin's potential outliers:Series([], Name: Tumor Volume (mm3), dtype: float64)
# Generate a box plot that shows the distrubution of the tumor volume for each treatment group.
red_diamond = dict(markerfacecolor="r", marker="D")

fig, ax = plt.subplots()
ax.set_ylabel("Final Tumor Volume (mm3)")
ax.boxplot(outlier_list, labels=treatments, flierprops=red_diamond)
ax.set_title("Final Tumor Volume across Four Regimens", fontsize=14)
ax.set_xticklabels(treatments)

plt.show()

Line and Scatter Plots
# Generate a line plot of tumor volume vs. time point for a single mouse treated with Capomulin
l509_tumor = combined_data.loc[(combined_data["Drug Regimen"] == "Capomulin") & (combined_data["Mouse ID"] == "l509")]

# Create the line plot
plt.plot(l509_tumor["Timepoint"], l509_tumor["Tumor Volume (mm3)"], color="blue")
plt.title("Capomulin Treatment of Mouse I509", fontsize=14)
plt.xlabel("Timepoint (days)")
plt.ylabel("Tumor Volume (mm3)")

# Display the plot
plt.show()

# Generate a scatter plot of mouse weight vs. the average observed tumor volume for the entire Capomulin regimen
capomulin_df = combined_data.loc[combined_data["Drug Regimen"]=="Capomulin"].groupby("Mouse ID")
avg_tumor_capomulin = capomulin_df["Tumor Volume (mm3)"].mean()
mouse_weight = capomulin_df["Weight (g)"].unique()
plt.scatter(mouse_weight, avg_tumor_capomulin)
plt.xlabel("Weight (g)")
plt.ylabel("Average Tumor Volume (mm3)")
plt.show()

Correlation and Regression
# Calculate the correlation coefficient and a linear regression model 
# for mouse weight and average observed tumor volume for the entire Capomulin regimen
mouse_weight = mouse_weight.astype(float)
correlation = st.pearsonr(mouse_weight, avg_tumor_capomulin)
(slope, intercept, rvalue, pvalue, stderr) = st.linregress(mouse_weight, avg_tumor_capomulin)
regress_values = mouse_weight * slope + intercept
line_eq = "y = " + str(round(slope,2)) + "x + " + str(round(intercept,2))
plt.scatter(mouse_weight, avg_tumor_capomulin)
plt.plot(mouse_weight,regress_values,"r-")
plt.xlabel("Weight (g)")
plt.ylabel("Average Tumor Volume (mm3)")
print(f"The correlation between mouse weight and the average tumor volume is {round(correlation[0], 2)}.")
plt.show()
The correlation between mouse weight and the average tumor volume is 0.84.
