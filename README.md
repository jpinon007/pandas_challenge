# pandas_challenge
# Background : As a chief data scientist of my city's school district, I will be helping the school board and mayor prioritzing and managing the school budgets. I will interpret district-wide standardized test results, specifically math and reading scores, student and school information, and finally visualize trends found in the data. 
# To help solve this challenge, I referenced the homework solution posted on GitLab by my professor to make sure my code was correct. https://git.bootcampcontent.com/Northwestern-University/NU-VIRT-DATA-PT-06-2024-U-LOLC.git
# Solution : I began by importing necessary modules to support my code such as csv, pandas, and pathlib. Next, I created a path for the school and student data. I then stored the school and student data files into pandas dataframes. Then, I merged the data into a single dataset. 
# Dependencies and Setup
import csv
import pandas as pd
from pathlib import Path

# File to Load (Remember to Change These)
school_data_path = Path(r"C:\Users\jasmi\OneDrive\Documents\pandas-challenge\Starter_Code\PyCitySchools\Resources\schools_complete.csv")
student_data_path = Path(r"C:\Users\jasmi\OneDrive\Documents\pandas-challenge\Starter_Code\PyCitySchools\Resources\students_complete.csv")

# Read School and Student Data File and store into Pandas DataFrames
school_data_csv = pd.read_csv(school_data_path)
student_data_csv = pd.read_csv(student_data_path)

# Combine the data into a single dataset.
school_data_merged = pd.merge(student_data_csv, school_data_csv, how="left", on=["school_name", "school_name"])
school_data_merged.head()
# I moved on to create a disctrict summary and respective dataframe. To do that, I calculated the total number of unique schools, total students, total budget, average math and reading scores, percentage of students passing math and reading, and the overall percentage of students passing both subjects. 
# Next, I created, formatted, and displayed the data frame. 
# Create a high-level snapshot of the district's key metrics in a DataFrame
district_summary = pd.DataFrame({
    "Total Schools": [total_schools],
    "Total Students": [total_students],
    "Total Budget": [total_budget],
    "Average Math Score": [avg_math_score],
    "Average Reading Score": [avg_reading_score],
    "% Passing Math": [passing_math_pct],
    "% Passing Reading": [passing_reading_pct],
    "% Predominantly Passing": [comprehensive_passing_rate]
})
# Formatting
district_summary["Total Students"] = district_summary["Total Students"].map("{:,}".format)
district_summary["Total Budget"] = district_summary["Total Budget"].map("${:,.2f}".format)
# Display the DataFrame
district_summary
# Next, I moved on to create a school summary and its dataframe where it included the school name, school type, total students, total school budget, and per student budget. As well as calculating the average math and reading scores, percentage of students who passed math and reading, and the percentage of the overall students who passed reading and math.   
# Create a DataFrame called `per_school_summary` with columns for the calculations above.
per_school_summary = pd.DataFrame({
        "School Type": [school_types],
        "Total Students": [students_per_school],
        "Total School Budget": [total_budget_per_school],
        "Per Student Budget": [capita_per_school],
        "Average Math Score": [avg_math_per_school],
        "Average Reading Score": [avg_reading_per_school],
        "% Passing Math": [per_school_passing_math],
        "% Passing Reading": [per_school_passing_reading],
        "% Overall Passing": [overall_passing_rate]
    }
    )

# Formatting
per_school_summary["Total School Budget"] = per_school_summary["Total School Budget"].map("${:,.2f}".format)
per_school_summary["Per Student Budget"] = per_school_summary["Per Student Budget"].map("${:,.2f}".format)

# Display the DataFrame
per_school_summary
# I moved on to sorting highest performing schools bsed on the overall passing percentage and saving the findings in a dataframe called "top schools".
# Sort the schools by `% Overall Passing` in descending order and display the top 5 rows.
highest_performing_schools = per_school_summary.sort_values(["% Overall Passing"], ascending=False)
highest_performing_schools.head(5)
# I did the same for lowest performing schools. 
# Sort the schools by `% Overall Passing` in ascending order and display the top 5 rows.
lowest_performing_schools =  per_school_summary.sort_values(["% Overall Passing"], ascending=True)
lowest_performing_schools.head(5)
# I then calculated the average math score for students in grades 9-12 at each school and created a dataframe. 
# Combine each of the scores above into single DataFrame called `math_scores_by_grade`
math_scores_by_grade = pd.DataFrame(
    {
        "9th": ninth_grade_math_scores,
        "10th": tenth_grade_math_scores,
        "11th": eleventh_grade_math_scores,
        "12th": twelfth_grade_math_scores
    }
)

# Minor data wrangling
math_scores_by_grade.index.name = None

# Display the DataFrame
math_scores_by_grade
# I did the same for average reading scores. 
# Use the code provided to separate the data by grade
ninth_grade_students = school_data_merged[(school_data_merged["grade"] == "9th")]
tenth_grade_students = school_data_merged[(school_data_merged["grade"] == "10th")]
eleventh_grade_students = school_data_merged[(school_data_merged["grade"] == "11th")]
twelfth_grade_students = school_data_merged[(school_data_merged["grade"] == "12th")]

# Group by `school_name` and take the mean of the the `reading_score` column for each.
ninth_grade_reading_scores = ninth_grade_students.groupby(["school_name"])["reading_score"].mean()
tenth_grade_reading_scores = tenth_grade_students.groupby(["school_name"])["reading_score"].mean()
eleventh_grade_reading_scores = eleventh_grade_students.groupby(["school_name"])["reading_score"].mean()
twelfth_grade_reading_scores = twelfth_grade_students.groupby(["school_name"])["reading_score"].mean()

# Combine each of the scores above into single DataFrame called `reading_scores_by_grade`
reading_scores_by_grade = pd.DataFrame(
    {
        "9th": ninth_grade_reading_scores,
        "10th": tenth_grade_reading_scores,
        "11th": eleventh_grade_reading_scores,
        "12th": twelfth_grade_reading_scores
    }
)

# Minor data wrangling
reading_scores_by_grade = reading_scores_by_grade[["9th", "10th", "11th", "12th"]]
reading_scores_by_grade.index.name = None

# Display the DataFrame
reading_scores_by_grade
# I also created a table that displayed school performance based on average spending ranges per student. As well as created four bins to group school spending.
# Establish the bins
spending_bins = [0, 585, 630, 645, 680]
spending_labels = ["<$585", "$585-630", "$630-645", "$645-680"]
# Create a copy of the school summary since it has the "Per Student Budget"
school_spending_df = per_school_summary.copy()
# Use `pd.cut` to categorize spending based on the bins.
school_spending_df["Spending Ranges (Per Student)"] = pd.cut(capita_per_school, spending_bins, labels=spending_labels, right=False)
school_spending_df
#  Calculate averages for the desired columns.
spending_math_scores = school_spending_df.groupby(["Spending Ranges (Per Student)"])["Average Math Score"].mean()
spending_reading_scores = school_spending_df.groupby(["Spending Ranges (Per Student)"])["Average Reading Score"].mean()
spending_passing_math = school_spending_df.groupby(["Spending Ranges (Per Student)"])["% Passing Math"].mean()
spending_passing_reading = school_spending_df.groupby(["Spending Ranges (Per Student)"])["% Passing Reading"].mean()
overall_passing_spending = school_spending_df.groupby(["Spending Ranges (Per Student)"])["% Overall Passing"].mean()
# Assemble into DataFrame
spending_summary = pd.DataFrame(
    {
        "Average Math Score" : spending_math_scores,
        "Average Reading Score": spending_reading_scores,
        "% Passing Math": spending_passing_math,
        "% Passing Reading": spending_passing_reading,
        "% Overall Passing": overall_passing_spending
    }
)

# Display results
spending_summary
# Next, I created a dataframe called "size_summary" that shows the performance of schools based on their size (small, medium or large).
# Establish the bins.
size_bins = [0, 1000, 2000, 5000]
size_labels = ["Small (<1000)", "Medium (1000-2000)", "Large (2000-5000)"]
# Categorize the spending based on the bins
# Use `pd.cut` on the "Total Students" column of the `per_school_summary` DataFrame.

per_school_summary["School Size"] =pd.cut(per_school_summary["Total Students"], size_bins, labels=size_labels, right=False)
per_school_summary
# Calculate averages for the desired columns.
size_math_scores = per_school_summary.groupby(["School Size"])["Average Math Score"].mean()
size_reading_scores = per_school_summary.groupby(["School Size"])["Average Reading Score"].mean()
size_passing_math = per_school_summary.groupby(["School Size"])["% Passing Math"].mean()
size_passing_reading = per_school_summary.groupby(["School Size"])["% Passing Reading"].mean()
size_overall_passing = per_school_summary.groupby(["School Size"])["% Overall Passing"].mean()
# Create a DataFrame called `size_summary` that breaks down school performance based on school size (small, medium, or large).
# Use the scores above to create a new DataFrame called `size_summary`
size_summary=pd.DataFrame(
    {
        "Average Math Score" : size_math_scores,
        "Average Reading Score": size_reading_scores,
        "% Passing Math": size_passing_math,
        "% Passing Reading": size_passing_reading,
        "% Overall Passing": size_overall_passing
    })

# Display results
size_summary
# Finally, I used the "per_school_summary" DF to create a new DataFrame called "type_summary" which displays school performance based on its type. 
# Group the per_school_summary DataFrame by "School Type" and average the results.
average_math_score_by_type = per_school_summary.groupby(["School Type"])["Average Math Score"].mean()
average_reading_score_by_type = per_school_summary.groupby(["School Type"])["Average Reading Score"].mean()
average_percent_passing_math_by_type = per_school_summary.groupby(["School Type"])["% Passing Math"].mean()
average_percent_passing_reading_by_type = per_school_summary.groupby(["School Type"])["% Passing Reading"].mean()
average_percent_overall_passing_by_type = per_school_summary.groupby(["School Type"])["% Overall Passing"].mean()
# Assemble the new data by type into a DataFrame called `type_summary`
type_summary =pd.DataFrame(
    {
        "Average Math Score" : average_math_score_by_type,
        "Average Reading Score": average_reading_score_by_type,
        "% Passing Math": average_percent_passing_math_by_type,
        "% Passing Reading": average_percent_passing_reading_by_type,
        "% Overall Passing": average_percent_overall_passing_by_type
    })

# Display results
type_summary
# PyCity Schools Analysis: 
- After reviewing the data, schools who were given higher budgets with 645-675 per students rendered lower test scores, compared to schools with smaller budgets that was around 585 per student. 
- Respective to the data summarized, small and medium sized schools showed higher math test scores than large sized schools at 89%-91% passing vs. 67%.
- According to the data, there are 7 district schools and 8 charter schools. Charter schools showed better performance in both reading and math scores. This can either be due to the quality of teaching or the student population since the data shows that there were more students attending distrct schools than charter schools. 
