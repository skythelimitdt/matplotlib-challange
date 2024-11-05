# Background
You've just joined Pymaceuticals, Inc., a new pharmaceutical company that specializes in anti-cancer medications. Recently, it began screening for potential treatments for squamous cell carcinoma (SCC), a commonly occurring form of skin cancer.

As a senior data analyst at the company, you've been given access to the complete data from their most recent animal study. In this study, 249 mice who were identified with SCC tumors received treatment with a range of drug regimens. Over the course of 45 days, tumor development was observed and measured. The purpose of this study was to compare the performance of Pymaceuticals’ drug of interest, Capomulin, against the other treatment regimens.

The executive team has tasked you with generating all of the tables and figures needed for the technical report of the clinical study. They have also asked you for a top-level summary of the study results.

- Generate summary statistics.
- Create bar charts and pie charts.
- Calculate quartiles, find outliers, and create a box plot.
- Create a line plot and a scatter plot.
- Calculate correlation and regression.
- Submit your final analysis.

# References:
- chatgpt for producing summary statistics in a single line with aggregation model.
  tumor_volume_per_regimen2= clean_combined_data.groupby("Drug Regimen")["Tumor Volume (mm3)"].agg(
    mean=np.mean,
    median=np.median,
    var=np.var,
    std=np.std,
    sem=lambda x: np.std(x) / np.sqrt(len(x))
).reset_index()
- chatgpt for creating loops for finding the IQRs and outliers
for treatment in treatments:
    treatment_data = merged_data[merged_data["Drug Regimen"] == treatment]
    treatment_labels.extend([treatment] * len(treatment_data))
    tumor_vol_data.extend(treatment_data["Tumor Volume (mm3)"].tolist())
    mouse_ids.extend(treatment_data["Mouse ID"].tolist())

Create a DataFrame with the collected data
tumor_df = pd.DataFrame({
    'Drug Regimen': treatment_labels,
    'Final Tumor Volume (mm3)': tumor_vol_data,
    'Mouse ID': mouse_ids
})

Identify outliers for each treatment
outliers_dict = {}  # Dictionary to store outliers for each treatment

for treatment in treatments:
    # Filter data for the current treatment
    treatment_data = tumor_df[tumor_df["Drug Regimen"] == treatment]
    
    # Calculate IQR
    Q1 = treatment_data["Final Tumor Volume (mm3)"].quantile(0.25)
    Q3 = treatment_data["Final Tumor Volume (mm3)"].quantile(0.75)
    IQR = Q3 - Q1
    # Determine bounds for outliers
    lower_bound = Q1 - (1.5 * IQR)
    upper_bound = Q3 + (1.5 * IQR)
    
    Identify outliers
    outliers = treatment_data[(treatment_data["Final Tumor Volume (mm3)"] < lower_bound) | 
                              (treatment_data["Final Tumor Volume (mm3)"] > upper_bound)]
    
    Store outliers in the dictionary
    outliers_dict[treatment] = outliers
