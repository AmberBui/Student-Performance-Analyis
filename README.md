# Student-Performance-Analyis

## Objective: 
Analyze students' exam performance to identify trends and factors affecting scores in math, reading, and writing. Provide actionable insights.
## Dataset:

**Dataset Link**: [Student Performance Analysis](https://www.kaggle.com/datasets/spscientist/students-performance-in-exams)

**Source:** Kaggle

**Dataset Provider:** Jakki Seshapanpu

**Date Accessed:** 13.01.2025

### Checking missing values

```
SELECT * FROM StudentsPerformance
WHERE math_score IS NULL OR reading_score IS NULL OR writing_score IS NULL
```
There are no null values.

### Basic Descriptive Statistics
#### Overall performance 
```
SELECT AVG(math_score) AS avg_math,
       AVG(reading_score) AS avg_reading,
       AVG(writing_score) AS avg_writing
FROM StudentsPerformance
```

#### Scores distribution 

```
SELECT Exam, 
       Score, 
       COUNT(*) AS StudentNumber
FROM 
    (SELECT 'math_score' AS Exam,
            math_score AS Score
     FROM StudentsPerformance
     
     UNION ALL
     
     SELECT 'writing_score' AS Exam,
            writing_score AS Score
     FROM StudentsPerformance
     
     UNION ALL
     
     SELECT 'reading_score' AS Exam,
            reading_score AS Score
     FROM StudentsPerformance
    ) AS ExamCounts
GROUP BY Exam, Score
ORDER BY Score DESC
```

#### Top performance 
```
SELECT *
FROM StudentsPerformance
WHERE writing_score > 90 AND math_score > 90 AND reading_score > 90
```
#### Performance Categorization

```
WITH Performance_Category AS (
  SELECT 
      (math_score + writing_score + reading_score) / 3 AS Average_Score
  FROM StudentsPerformance
)
SELECT 
    CASE 
        WHEN Average_Score >= 90 THEN 'Excellent'
        WHEN Average_Score BETWEEN 70 AND 89 THEN 'Good'
        ELSE 'Need to Improve'
    END AS Performance_Category,
    COUNT(*) AS Student_Number
FROM Performance_Category
GROUP BY 
    CASE 
        WHEN Average_Score >= 90 THEN 'Excellent'
        WHEN Average_Score BETWEEN 70 AND 89 THEN 'Good'
        ELSE 'Need to Improve'
    END
ORDER BY Performance_Category DESC
```

### Analyzing influencing factors

#### Parental Education

```
SELECT  parental_level_of_education,
        AVG(math_score) AS Avg_Math,
        AVG(writing_score) AS Avg_Writing,
        AVG(reading_score) AS Avg_Reading,
        AVG(math_score + writing_score + reading_score) AS Avg_Total
FROM StudentsPerformance
GROUP BY parental_level_of_education
ORDER BY Avg_Total DESC
```
Students' performance shows a clear trend, with better results belonging to higher parental education levels.

#### Test Preparation 

```
SELECT test_preparation_course,
       AVG(math_score) AS Avg_Math,
       AVG(writing_score) AS Avg_Writing,
       AVG(reading_score) AS Avg_Reading,
       AVG(math_score + writing_score + reading_score) AS Avg_Total
FROM StudentsPerformance
GROUP BY test_preparation_course
ORDER BY Avg_Total DESC
```
Students who completed the preparation course achieved significantly higher scores (an average of 23 points) in all categories compared to students who did not complete.

#### Lunch Type

```
SELECT lunch,
       AVG(math_score) AS avg_math,
       AVG(reading_score) AS avg_reading,
       AVG(writing_score) AS avg_writing,
       AVG(math_score + writing_score + reading_score) AS Avg_Total
FROM StudentsPerformance
GROUP BY lunch
ORDER BY lunch
```

Students with standard lunch plans consistently perform better academically than those with free or reduced lunch plans.

### Insights 

- Students with standard lunch plans and better preparation for tests show higher academic performance compared to those on free/reduced lunch plans. 
- Students whose parents have higher education levels consistently achieve better scores, suggesting a strong link between parental education and student success. 
- These patterns emphasize the influence of socioeconomic status, parental background, and test preparation on academic outcomes.

### Actions
- Lunch Program Enhancements: Improve nutritional quality for all students, with additional focus on those receiving free or reduced lunches to address gaps in performance.

- Parental Engagement: Design programs to empower parents at all education levels, including workshops and resources that support their children's learning.

- Targeted Support: Combine academic assistance like tutoring with support for students from lower-income families or less-educated parental backgrounds to foster equal opportunities.

			


			
			






