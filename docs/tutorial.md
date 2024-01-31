## Part 3: importing and cleaning the conditions file


* Import the conditions file in Pandas
   * Count the number of rows and columns.
   * Examine the data types, do they look correct? Correct the data types where required.
    * Counting unique values
        * Count the number of unique patients.
        * Count the number of unique encounters.
        * Count the number of unique diagnoses.
   * Missing data
        * Count the number of encounters that are missing the date of admission.
        * Count the number of encounters that are missing the date of discharge.
        * Count the number of encounters that are missing a diagnoses and the number of patients that are affected by this problem.

* Import the SNOMED-CT data dictionary
    * Count the number of rows and columns.
    * Count the number of unique diagnoses.
    * Count and delete any rows where the `CODE` or `DESCRIPTION` values are missing.

* Merge the `conditions` table with the SNOMED dictionary.
    * Consider the type of merge that you would perform. The import thing here is to retain all information of patients and their hospitalizations and not delete any data.
    * You should add the `indicator=True` parameter to the merge command and examine it's output.
        * Display any invalid SNOMED codes and the number of patients and hospitalizations that these codes affect. Any codes that are invalid would not have an entry in the dictionary and the `DESCRIPTION` column would be empty.
    * Save the merged dataframe as a new Pandas object and display the total number of rows and columns.
    

* Explore the diagnoses
    * Display the top 10 most common SNOMED concepts used in the dataset.
    * Display the bottom 10 less common SNOMED concepts used in the dataset.
    * Tip: For both of these tasks, you can use the pandas groupby() function.

