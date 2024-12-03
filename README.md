# STAT 184 Activity 12: Introduction to GitHub
This repository features the data analysis performed for STAT 184 Activity #10 - Creating a Quarto Document.

## Introduction
The aim of the data analysis for Activity 10 was to determine whether gender played a significant role in a student's chance of admission into UC Berkley in the year 1973. The data analysis was performed using [Berkeley's 1973 Graduate Admissions Dataset](https://waf.cs.illinois.edu/discovery/berkeley.csv) and edited in Quarto Markdown. This dataset contains records of applicants, with details about the year and major they applied for, their gender and admission decision. The accompanying R code, provided as part of the assignment, guided the focus and direction of this analysis. This work is documented in the file [STAT184_Activity10-1.qmd](https://github.com/varshag425/STAT184-Sec1-Varsha/blob/main/code/STAT184_Activity10-1.qmd), found in the 'code' folder of this repository.

## Implementation
This analysis features:
- Data loading and pre-processing: loading the dataset and other useful packages
    - ggplot2: used for data visualizations
    - dplyr: for data manipulation
    - tidyr: to create tidy data
- EDA
    - Inspected data using the head() and summary() functions.
        - The head() function samples rows from the Berkeley dataset which allows us to understand the structure of the dataframe and guides the next steps in making the data tidy.
        - A cursory view of this dataset confirmed that there were no missing entries.
        - The major field contains generic labels (including ‘Other’), which restricts analysis to a specific set of majors.
        - There are 12763 total applicants for the year 1973 (used summary() function)
- Data Analyses and corresponding Visualizations:
    - Admission Rates by Gender:
        - Data is segregated into a two way table arranged according to admission decision (accepted/rejected) and gender (Male/Female).
        - Admission rate is calculated as a proportion of acceptances (using summarize()) and is further grouped by gender as shown below.
          ```
            admission_rates_gender <- data %>%
              group_by(Gender) %>%
              summarize(
                Total = n(),
                Accepted = sum(Admission == "Accepted"),
                Admission_Rate = Accepted / Total
              )
            
            # Plotting the admission rates by gender
            ggplot(admission_rates_gender, aes(x = Gender, y = Admission_Rate)) +
              geom_bar(stat = "identity", position = "dodge") +
              labs(title = "Admission Rates by Gender", 
                   x = "Gender", 
                   y = "Admission Rate") +
              theme_minimal()
          ```
    - Admission Rates by Major:
        - Admission rate is grouped according to major.
          ```
            #Admissions rate by major
            admission_rate_major <- data %>%
              select(Major, Admission) %>%
              group_by(Major) %>%
              summarize(
                Total = n(),
                Accepted = sum(Admission == "Accepted"),
                Admission_Rate = Accepted / Total
              )
            ggplot(admission_rate_major, aes(x = Major, y = Admission_Rate)) +
              geom_bar(stat = "identity", position = "dodge") +
              labs(title = "Admission Rates by Major", 
                   x = "Major", 
                   y = "Admission Rate") +
              theme_minimal()
          ```
    - Applications Grouped by Gender and Major:
        - Graphed a bar plot to visualize the distribution of applicants grouped by ‘Major’ and ‘Gender’. This helps verify in the next step if the admission rates were biased toward a particular gender.
          ```
            #Applications by Gender, Major
            applications_gender_major <- data %>%
              select(Major, Gender) %>%
              group_by(Major, Gender) %>%
              summarize(
                Total = n()
              )
            ggplot(applications_gender_major, aes(x = Major, y = Total, fill = Gender)) +
              geom_bar(stat = "identity", position = "dodge") +
              labs(title = "Total Applications by Major and Gender", 
                   x = "Major", 
                   y = "Applications") +
              theme_minimal()
          ```
        - Resulting graph idicates that there were a higher number of male applicants, with the exception of majors C and E.
    - Admission Rates Grouped by Gender and Major
        - Compared gender-specific admission rates across majors.
          ```
          admission_rates_gender_major <- data %>%
              group_by(Major, Gender) %>%
              summarize(
                Total = n(),
                Accepted = sum(Admission == "Accepted"),
                Admission_Rate = Accepted / Total
              )
          ggplot(admission_rates_gender_major, aes(x = Major, y = Admission_Rate, fill = Gender)) +
              geom_bar(stat = "identity", position = "dodge") +
              labs(title = "Admission Rates by Major and Gender", 
                   x = "Major", 
                   y = "Admission Rate") +
              theme_minimal()
          ```
    - Gender Difference in Admission Rates by Major
        - Compare and visualize the gender disparity in admission rates across majors.
          ```
          #See the admissions rate gender difference by major
            admission_rate_diff <- admission_rates_gender_major %>%
              select(Major, Gender, Admission_Rate) %>%
              pivot_wider(names_from = Gender, values_from = Admission_Rate) %>%
              mutate(Difference = M - F)
            
            ggplot(admission_rate_diff, aes(x = Major, y = Difference, fill = Difference > 0)) +
              geom_bar(stat = "identity") +
              scale_fill_manual(values = c("dark green", "skyblue"), 
                                labels = c("Female Higher", "Male Higher")) +  # Red for below 0, skyblue for above 0
              labs(title = "Difference Between Male and Female Admission Rates by Major",
                   x = "Major", 
                   y = "Difference (Male - Female)") +
              geom_hline(yintercept = 0, linetype = "dotted", color = "black", linewidth = 1) +  # Add horizontal dotted line at y = 0
              theme_minimal() +
              theme(legend.title = element_blank())
          ```

## Results and Conclusions

### Admission Rates by Gender
  ![fig-Admission-Rates-by-Gender-barplot-1](https://github.com/user-attachments/assets/11554e7d-6fed-4a08-a5b7-bb5ef145d423)

  There is a clear disparity in the proportion of male vs. female applicants who are admitted. Approximately, male applicants have a 0.45 chance of being accepted, whereas female applicants have a 0.35 chance.

### Admission Rates by Major
  ![fig-Admission-Rates-by-Major-barplot-1](https://github.com/user-attachments/assets/babe8975-838c-44a4-9152-b7aeb542442d)

  Admission rates are uneven across major, which prompts the question about the demographic of the applicants. Specifically, what is the distribution of applicants across each major, and if there is a gender disparity, what does it look like?

### Applications Received Grouped by Gender and Major
  ![fig-Applications-by-Gender-and-Major-barplot-1](https://github.com/user-attachments/assets/4a290184-0433-4872-ae48-deb1c613aa5e)

  There were a higher number of male applicants for most majors, with the exception of majors C and E.

### Admission Rates Grouped by Gender and Major
  ![fig-Admission-Rates-by-Gender-Major-barplot-1](https://github.com/user-attachments/assets/52926486-e456-4bca-aff6-04873c96c087)

  There is a clear gender disparity in admission rates within each major. If there was no gender-specific bias, the height of the bars would ideally be equal.

  In 4 out of the 7 majors (A, B, D and F), female applicants had higher admittance rates into these programs, whereas male applicants had higher admittance rates for majors C, E and Other.


### Gender Difference in Admission Rates by Major
  ![fig-gender-diff-admission-rates-by-major-1](https://github.com/user-attachments/assets/53bf2e5d-5854-4ac3-8198-db43d99ea6c7)

  This graph helps in visualizing the extent of disparity in the admission rates between male and female applicants across each major. The length of the bars indicate how much more likely it was for either a male candidate (blue) or a female candidate (green) to be admitted, in comparison to their female/male counterpart.


## Acknowledgements
Accompanying R code was provided as part of assignment 10.

## Contact Info
Please contact me at vpg5172@psu.edu with any questions/comments/concerns.
