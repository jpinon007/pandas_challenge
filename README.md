# pandas_challenge

# Background : As a chief data scientist of my city's school district, I will be helping the school board and mayor prioritzing and managing the school budgets. I will interpret district-wide standardized test results, specifically math and reading scores, student and school information, and finally visualize trends found in the data. 
# To help solve this challenge, I referenced the homework solution posted on GitLab by my professor to make sure my code was correct. https://git.bootcampcontent.com/Northwestern-University/NU-VIRT-DATA-PT-06-2024-U-LOLC.git

# Solution : I began by importing necessary modules to support my code such as csv, pandas, and pathlib. 

# District Summary: In order to create a high-level snapshot of the district's key metrics in th DataFrame, I had to find the total number of unique schools, total students, total budget, average math score, average reading score, the percentage of students who passed math, the percentage of students who passed reading, and the percentage of students who passed math and reading. 

# School Summary: In order to create a DataFrame that summarizes key metrics about each school i had to determine  the school name, school type, total students, total school budget, per student budget, average math score, average reading score, the percentage of students who passed math, the percentage of students who passed reading, and the percentage of students who passed math and reading. 

# Highest-Performing schools: I was tasked to sort the schools by percentage of overall passing in descending order and had to display the top 5 rowa, and saving the results in a DataFrame called "top_schools".

# Lowest-Performing Schools: I was tasked to sort the schools by percentage of overall passing in ascending order and had to display the top 5 rows, and saving the results in a DataFrame called "bottom_schools".

# Math Scores by Grade: I had to conduct the necessary calculations to create a DataFrame that listed the average math score for students of each grade level (9th, 10th, 11th, 12th) at each school.

# Reading scores by grade: I created a dataframe that listed the average reading score for students of each grade level (9th, 10th, 11th, 12th) at each school.

# Scores by school spending: I made a table that breaks down school performance based on average spending ranges per student and using the code provided to create four bins with reasonable cutoff values to group school spending. As well as categorizing the bins and calculating the mean scores per spending range. Using the scores calculated, I created a DataFrame called spending_summary that includes the average math score, average reading score, the percentage of students who passed math, the percentage of students who passed reading, and the percentage of students who passed math and reading. 

# Scores by school size: I used the code provided to create three bins with reasonable cutoff values to group school size. I then categorized the school size bins, and used the code given to calculate mean scores per size range. Next, I created a DataFrame called size_summary that breaks down school performance based on school size (small, medium, large). 

# Scores by school type: Using the per_school_summary DataFrame, I created a new DataFrame called type_summary. Finally, this new DataFrame would show school performance based on the "School Type".


# PyCity Schools Analysis: 
- After reviewing the data, schools who were given higher budgets with 645-675 per students rendered lower test scores, compared to schools with smaller budgets that was around 585 per student. 
- Respective to the data summarized, small and medium sized schools showed higher math test scores than large sized schools at 89%-91% passing vs. 67%.
- According to the data, there are 7 district schools and 8 charter schools. Charter schools showed better performance in both reading and math scores. This can either be due to the quality of teaching or the student population since the data shows that there were more students attending distrct schools than charter schools. 
