# NKUA Health Data Science Pandas/EHR tutorial

## Introduction and preparatory steps

The purpose of this tutorial is to provide students with hands-on experience of using Python and the Pandas library to import, describe, quality control, and clean electronic health record (EHR) data. For this tutorial, we will be using data that was generated from the Synthea platform - more information is available here: https://synthetichealth.github.io/synthea/. The data are entirely synthetic and do not contain any real patient identifiers and are safe to be stored on your laptop or personal computer. 

The data are available as a series of compressed CSV files and are stored on a GitHub repository: https://github.com/spiros/nkua-health-data-science/tree/master/ in the ‘data/dest’ folder ; there are five main data files (conditions, patients, observations, encounters, and medications). In addition to these files, three dictionaries are available that are used to encode data in these files: LOINC that is used for laboratory measurements, RxNorm that is used for medications, and SNOMED that is used for conditions).

## Part 1: reading material

1. Become familiar with the data schema of the individual files.
You can find the main data dictionary on GitHub https://github.com/spiros/nkua-health-data-science/blob/master/docs/data_dictionary.md
The dataset we will be using is based on a US healthcare provision model so some concepts will be slightly different and the terminology used might be different from other healthcare systems. The Synthea paper is also a good reference to have a quick read through as it talks about how the data were generated and would help you understand the underlying structure a bit more https://academic.oup.com/jamia/article/25/3/230/4098271

2. Become familiar with LOINC and RxNorm that are used to record laboratory measurements and medications respectively. 
A good source of information is the National Library of Medicine pages on RxNorm https://www.nlm.nih.gov/research/umls/rxnorm/index.html 
The main LOINC paper https://academic.oup.com/clinchem/article-abstract/49/4/624/5641953 

## Part 2: importing data and cleaning the patient table

Tip: data files can be directly read from (public) GitHub repositories using Pandas. You will however need to slightly change the URL of the file and replace “blob” with “raw” for example:
https://github.com/spiros/nkua-health-data-science/blob/master/data/dest/patients.csv.gz to 
https://github.com/spiros/nkua-health-data-science/raw/master/data/dest/patients.csv.gz which can then be directly read using the read_csv() function. Alternatively, you can just download the files directly to your PC or laptop and work from them or work via CoCalc.


* Import the `patients` data file in Pandas.
    * How many rows and how many columns were imported? You can use the `shape` property to display this which returns a tuple of rows and columns.
* How many unique patients does the file contain? The [`nunique()`](https://pandas.pydata.org/docs/reference/api/pandas.Series.nunique.html#pandas.Series.nunique) function returns the number of unique values with or without missing values.
* Drop all columns apart from `Id`, `BIRTHDATE`, `DEATHDATE`, `RACE`, `ETHNICITY`, `GENDER`. To do this, use the [`drop`](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.drop.html#pandas.DataFrame.drop) function.
* Examine the data types, do they look correct? Correct the data types where required. The `dtypes` attribute contains the data types for each column in a DataFrame.
    * Pandas has a useful function for converting between different data types [`astype`](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.astype.html#pandas.DataFrame.astype).
* Create a new column, called “exclude” and set the default value to 0
* For the `RACE`, `ETHNICITY` and `GENDER` variables:
    * Count the number of unique values - how many values does each column have and what is the most frequent value?
    * Count the number of missing values - how many missing values does each column have?
    * Confirm that the observed values are correct. 
    * Set the exclude flag to 1 for patients that have incorrect or missing values in any of these variables
* The patient's date of birth (`BIRTHDATE`) is a mandatory field:
    * What are the earliest and latest dates in the dataset? You can use `min` and `max` to display this.
    * How many patients have an invalid date of birth? For the purposes of this tutorial, we will consider the * following dates invalid: 
        * a missing value,
        * a invalid value e.g. a string of numbers or letters, 
        * a date of birth after a date of death,
        * a date in the future e.g. after 2021,
        * a date too far in the past e.g. 1910
    * Set the exclude flag to 1 for all patients with an invalid `BIRTHDATE`
* How many patients did you exclude in total?
* Export the patient file you have QC’ed into a new location (you can use .to_csv() for this)

## Part 3: importing and cleaning the conditions file

* Import the `conditions` file in Pandas
   * Count the number of rows and columns.
   * Examine the data types, do they look correct? Correct the data types where required.
    * Counting unique values:
        * Count the number of unique patients.
        * Count the number of unique encounters.
        * Count the number of unique diagnoses.
   * Missing data:
        * Count the number of encounters that are missing the date of admission.
        * Count the number of encounters that are missing the date of discharge.
        * Count the number of encounters that are missing a diagnoses and the number of patients that are affected by this problem.

* Import the SNOMED-CT data dictionary.
    * Count the number of rows and columns.
    * Count the number of unique diagnoses.
    * Count and delete any rows where the `CODE` or `DESCRIPTION` values are missing.

* Merge the `conditions` table with the SNOMED dictionary.
    * Consider the type of merge that you would perform. The important thing here is to retain all information of patients and their hospitalizations and not delete any data.
    * You should add the `indicator=True` parameter to the merge command and examine it's output.
        * Display any invalid SNOMED codes and the number of patients and hospitalizations that these codes affect. Any codes that are invalid would not have an entry in the dictionary and the `DESCRIPTION` column would be empty.
    * Save the merged dataframe as a new Pandas object and display the total number of rows and columns.

* Explore the diagnoses
    * Display the top 10 most common SNOMED concepts used in the dataset.
    * Display the bottom 10 less common SNOMED concepts used in the dataset.
    * Tip: For both of these tasks, you can use the pandas groupby() function.
    * How many patients have been diagnosed with hypertension?

## Part 4: importing and cleaning the observations file

* Import the `observations` file in Pandas.
   * Count the number of rows and columns.
   * Examine the data types, do they look correct? Correct the data types where required.
    * Counting unique values:
        * Count the number of unique patients.
        * Count the number of LOINC concept identifiers.
        * Count the number of unique encounters.
   * Missing data:
        * Count the number of encounters that are missing the date of admission.
        * Count the number of encounters that are missing the date of discharge.
        * Count the number of encounters that are missing a diagnoses and the number of patients that are affected by this problem.

* Import the LOINC data dictionary:
    * Count the number of rows and columns.
    * Count the number of unique diagnoses.
    * Count and delete any rows where the `CODE` or `DESCRIPTION` values are missing.

* Merge the `observations` table with the LOINC dictionary.
    * Consider the type of merge that you would perform. The important thing here is to retain all information of patients and their hospitalizations and not delete any data.
    * You should add the `indicator=True` parameter to the merge command and examine it's output.
        * Display any invalid LOINC concept codes and the number of patients and hospitalizations that these codes affect. Any codes that are invalid would not have an entry in the dictionary and the `DESCRIPTION` column would be empty.
        * Delete all rows with an invalid LOINC concept identifier.
    * Save the merged dataframe as a new Pandas object and display the total number of rows and columns.

* Explore the lab measurements:
    * Display the top 10 most common LOINC concepts used in the dataset.
    * Display the bottom 10 less common LOINC concepts used in the dataset.
    * Tip: For both of these tasks, you can use the pandas groupby() function.

* Blood pressure measurements:
    * Identify the LOINC concept codes for systolic and diastolic blood pressure measurements.
    * From the `observations` DataFrame, select all systolic and diastolic blood pressure measurements and save them into a new DataFrame. Tip: create a deep copy using [`copy()`](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.copy.html) which will allow you to make changes to the new DataFrame.
        * Display the number of rows and columns.
        * Display the number of patients with at least one measurement of either a systolic or a diastolic blood pressure measurement.
        * Display the number of patients with at least one measurement of both systolic and diastolic blood pressure. Tip: You can use [`set`](https://docs.python.org/3/tutorial/datastructures.html#sets) operations to achieve this.
        * Examine the data types of this DataFrame, are they correct? Set the appropriate data types where appropriate.
        * For systolic and diastolic blood pressure measurements separately:
            * Examine the distribution of values.
            * Plot the distribution of values.
                * Plot using a histogram.
                * Plot using a box plot.
                * Bonus: create a stacked histogram that plots both systolic and diastolic values.
            * Delete any rows with incorrect values e.g. a systolic blood pressure over 220 or a diastolic blood pressure below 30.

* Repeat the same process as in the previous step for body mass index (BMI).

## Part 5: bringing it all together

This part of the tutorial combines all of the things covered in previous parts into a single working example.

* Import the (cleaned) `patient` file which was generated in Part 2 of this tutorial.
    * Delete patients where `exclude=1`.
* Identify patients with a diagnosis of essential hypertension (HT):
    * Create two variables in the `patient` file:
        * One variable that will contain the earliest date of HT diagnosis if applicable. Patients that have not been diagnosed with HT will have a missing value.
        * One variable that will act as a flag to identify patients that have been diagnosed with HT.
* Extract the systolic and diastolic blood pressure measurements for the entire cohort.
    * For the HT cohort, delete any measurements that have occured prior to the date of diagnosis.
* Explore the distribution of measurements between the two cohorts (HT patients and patients not diagnosed with HT).
    * Calculate and display key distribution metrics.
    * Plot the values using a [`boxplot`](https://seaborn.pydata.org/generated/seaborn.boxplot.html).
