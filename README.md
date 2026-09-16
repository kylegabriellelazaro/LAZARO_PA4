# Programming Assignment 4
#### Made by: Lazaro, Kyle Gabrielle A. | 2ECE-C
#### Date Submitted: September 14, 2026  
## Objectives:
1. filter tabular data using several categorical and numerical conditions;
2. construct focused DataFrames by selecting relevant features;
3. summarize the relationship between categorical features and a numerical variable; and
4. communicate a data comparison using clear and correctly labeled plots.

#### A. VISAYAS COMMUNICATION DATAFRAME
- Filters for examinees whose Hometown is `Visayas` and Track is `Communication`, displaying their `Name`, `Gender`, `Math`, `Electronics`, and calculated `Average` (mean of Math and Electronics).

```python
VisComm = ECE_Board_Exam_2.loc[
    (ECE_Board_Exam_2['Hometown'] == 'Visayas') & (ECE_Board_Exam_2['Track'] == 'Communication'),
    ['Name', 'Gender', 'Math', 'Electronics']
]
VisComm['Average'] = VisComm[['Math', 'Electronics']].mean(axis=1)

print("Number of rows in VisComm:", len(VisComm))
```

#### B. VISAYAS FEMALE DATAFRAME 
- Filters for female examinees from Visayas, calculating their Average score between GEAS and Electronics. Further filtering isolates students achieving an Average >= 60.
  
```python
VisFemale = ECE_Board_Exam_2.loc[
    (ECE_Board_Exam_2['Hometown'] == 'Visayas') & (ECE_Board_Exam_2['Gender'] == 'Female'),
    ['Name', 'Track', 'GEAS', 'Electronics']
]

VisFemale['Average'] = VisFemale[['GEAS', 'Electronics']].mean(axis=1)

# Filter for Average >= 60
VisFemale_passed = VisFemale[VisFemale['Average'] >= 60]
```

#### C. CATEGORY-AVERAGE VISUALIZATION
- Calculates overall student averages across all four subjects (Math, Electronics, GEAS, Communication) and groups them by Track, Gender, and Hometown.

```python
ECE_Board_Exam_2['Average'] = ECE_Board_Exam_2[
    ['Math', 'Electronics', 'GEAS', 'Communication']
].mean(axis=1)

mean_track = ECE_Board_Exam_2.groupby('Track')['Average'].mean().reset_index()
mean_gender = ECE_Board_Exam_2.groupby('Gender')['Average'].mean().reset_index()
mean_hometown = ECE_Board_Exam_2.groupby('Hometown')['Average'].mean().reset_index()

fig, axes = plt.subplots(1, 3, figsize=(15, 5), sharey=True)

axes[0].bar(
    mean_track["Track"],
    mean_track["Average"],
  
)
axes[0].set_title("Mean Average by Track")
axes[0].set_xlabel("Track")
axes[0].set_ylabel("Mean Board Exam Average")

axes[1].bar(
    mean_gender["Gender"],
    mean_gender["Average"],
   
)
axes[1].set_title("Mean Average by Gender")
axes[1].set_xlabel("Gender")

axes[2].bar(
    mean_hometown["Hometown"],
    mean_hometown["Average"],
   
)

axes[2].set_title("Mean Average by Hometown")
axes[2].set_xlabel("Hometown")

plt.tight_layout()
plt.show()

top_track = mean_track.loc[mean_track["Average"].idxmax()]
top_gender = mean_gender.loc[mean_gender["Average"].idxmax()]
top_hometown = mean_hometown.loc[mean_hometown["Average"].idxmax()]

print(
    f"1. Track: The {top_track['Track']} track achieved the highest sample mean Average of {top_track['Average']:.2f}%."
)
print(
    f"2. Gender: {top_gender['Gender']} students achieved the highest sample mean Average of {top_gender['Average']:.2f}%."
)
print(
    f"3. Hometown: Students from {top_hometown['Hometown']} achieved the highest sample mean Average of {top_hometown['Average']:.2f}%."
)
```

## READMe File Version History
September 17, 2026 - Update READMe output uploaded
