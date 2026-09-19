# ECE2112-PA4
# Intended Learning Outcomes
1. filter tabular data using several categorical and numerical conditions;
2. construct focused DataFrames by selecting relevant features;
3. summarize the relationship between categorical features and a numerical variable; and
4. communicate a data comparison using clear and correctly labeled plots.


# A. VISAYAS COMMUNICATION DATAFRAME
- Create a DataFrame named VisComm containing students whose Hometown is Visayas and whose Track
is Communication. Retain only these columns, in the stated order:
| Name, Gender, Math, Electronics, Average |
- Display the resulting DataFrame and its number of rows. Both filtering conditions must be applied to
the source dataset before the columns are selected.
- ## What Happened?
  The code imports pandas and matplotlib, reads board exam data from an Excel file into a DataFrame, and calculates an Average score column across subject scores using .mean(axis=1). It then defines a combined boolean filter requiring Hometown to be "Visayas" and Track to be "Communication". Finally, it filters the board_exam dataset, selects only the specified columns (Name, Gender, Math, Electronics, Average), and prints both the filtered VisComm DataFrame and its total row count as requested
- ## Implementation
```python
board_exam["Average"] = board_exam[
    ["Math", "Electronics", "GEAS", "Communication"]
].mean(axis=1)

vis_comm_filter = (board_exam["Hometown"] == "Visayas") & (
    board_exam["Track"] == "Communication")

target_columns = ["Name", "Gender", "Math", "Electronics", "Average"]
VisComm = board_exam[vis_comm_filter][target_columns]

print("VisComm DataFrame:")
print(VisComm)
print("\nNumber of rows:", len(VisComm))
```
# B. VISAYAS FEMALE DATAFRAME
- Create a second DataFrame named VisFemale containing students whose Hometown is Visayas and
whose Gender is Female. Retain only:
| Name, Track, GEAS, Electronics, Average |
- Display VisFemale. Then display only the rows of VisFemale whose Average is at least 60. Do not
overwrite VisFemale when performing this second filter.
- ## What Happened?
In this section, the code creates a second DataFrame called VisFemale by filtering board_exam for students where Hometown is "Visayas" and Gender is "Female". It retains only the requested columns (Name, Track, GEAS, Electronics, Average) and prints the complete VisFemale DataFrame. Then, without modifying or overwriting VisFemale, it applies a secondary filter (VisFemale["Average"] >= 60) directly inside the print statement to output only those female students with an average score of at least 60
- ## Implementation
```python
vis_female_filter = (board_exam["Hometown"] == "Visayas") & (
    board_exam["Gender"] == "Female")

target_cols_b = ["Name", "Track", "GEAS", "Electronics", "Average"]
VisFemale = board_exam[vis_female_filter][target_cols_b]

print("VisFemale DataFrame: ")
print(VisFemale)

print("\n", "Female Students with and Average atleast 60: ")
print(VisFemale[VisFemale["Average"] >= 60])
```
# C. CATEGORY-AVERAGE VISUALIZATION
- Examine how the recorded Average differs across the three categorical features Track, Gender, and
Hometown
a. For each feature, compute the mean of Average for every category using Pandas.
b. Display the three summary tables.
c. Create one figure containing three bar charts: mean Average by Track, by Gender, and by
Hometown.
d. Below the figure, write three concise statements identifying the category with the highest sample
mean for each feature.
- ## What Happened?
In this section, the code calculates the mean Average score for three categorical variables—Track, Gender, and Hometown—using groupby().mean() and prints each resulting summary table. It then uses Matplotlib to generate a single figure containing three side-by-side bar charts (1x3 subplot grid) visualizing these category means, complete with titles, labels, and fixed y-axis limits (0 to 100)
- ## Implementation
```python
board_exam["Average"] = board_exam[
    ["Math", "Electronics", "GEAS", "Communication"]
].mean(axis=1)

track_mean = board_exam.groupby("Track")["Average"].mean().reset_index()
gender_mean = board_exam.groupby("Gender")["Average"].mean().reset_index()
hometown_mean = board_exam.groupby("Hometown")["Average"].mean().reset_index()

print("=== Summary Table: Mean Average by Track ===")
print(track_mean.to_string(index=False))

print("\n=== Summary Table: Mean Average by Gender ===")
print(gender_mean.to_string(index=False))

print("\n=== Summary Table: Mean Average by Hometown ===")
print(hometown_mean.to_string(index=False))

fig, axes = plt.subplots(1, 3, figsize=(15, 5))

axes[0].bar(track_mean["Track"], track_mean["Average"], color="skyblue")
axes[0].set_title("Mean Average by Track")
axes[0].set_xlabel("Track")
axes[0].set_ylabel("Mean Average")
axes[0].set_ylim(0, 100)

axes[1].bar(gender_mean["Gender"], gender_mean["Average"], color="salmon")
axes[1].set_title("Mean Average by Gender")
axes[1].set_xlabel("Gender")
axes[1].set_ylabel("Mean Average")
axes[1].set_ylim(0, 100)

axes[2].bar(
    hometown_mean["Hometown"], hometown_mean["Average"], color="lightgreen"
)
axes[2].set_title("Mean Average by Hometown")
axes[2].set_xlabel("Hometown")
axes[2].set_ylabel("Mean Average")
axes[2].set_ylim(0, 100)

plt.tight_layout()
plt.show()
