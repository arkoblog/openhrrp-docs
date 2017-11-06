=============================
Variable exploration exercise
=============================

The variable exploration exercise is intended to help us identify how the data is going to be restructured for use in the open data portal. More specifically, this exercise will serve to:

    1. Identify naming conventions for variables across all 8 columns,
    2. Identify new variables that are not currently included, but can be extracted from the database.

Methodology
^^^^^^^^^^^

The exercise involves a thorough examination of each and every variable across all the 8 tables, keeping the following guidelines in mind:

Presence of null values
-----------------------
Are there null values (NAs) in the column? If yes, are they explainable? (maybe a branching question, that does not get asked to every respondent)

Presence of additional information
----------------------------------
Do some columns contain information that is better off if separated into a new column? For example, some damage assessment variables also contain information about no damage.

    i. If yes, how should this information be contained in the new column?
    ii. What would we name the column?

Representative Variable Name
----------------------------
Is the column name representative of the information contained within it? If not, what would be a better name for the column?


Next Steps
^^^^^^^^^^
Based on the exercise above the following tasks need to performed in the data restructuring exercise:

Generation of Primary and Composite Keys
----------------------------------------

To facilitate faster table join operations the following primary and composite keys will be generated across all eight tables:

  1. recordID: This ID will uniquely identify each record that was captured in the survey.
  2. buildingID: This ID will uniquely identify each building for which data has been recorded in the survey.
  3. householdID: This ID will uniquely identify each household for which data has been recorded in the survey.
  4. individualID: This ID will uniquely identify every individual for whom data has been recorded in the survey.
  5. injuredindvID: This ID will uniquely identify every injured/missing individual for whom data has been recorded in the survey.
  6. deceasedID: This ID will uniquely identify every deceased individual for whom data has been recorded in the survey.

Creation of housing damage flags
--------------------------------

In the dataset provided by CBS, information about whether the building had any damage was merged with another categorical variable (dm_fndtn1, staircase1, etc.). This information -- about whether the building has any form of damage -- will now be made available in the following variables:

  1. building_da_structural_foundation_flag: Indicates whether there is any damage to the building's foundation
  2. building_da_structural_roof_flag: Indicates whether there is any damage to the building's roof
  3. building_da_masonry_corn_sep_flag: Indicates if there is any corner separation related damage in masonry building
  4. building_da_masonry_diag_cracking_flag: Indicates if there is any diagonal cracking related damage in masonry building
  5. building_da_masonry_ipl_failure_flag: Indicates if there is any in plane failure of walls in masonry building
  6. building_da_masonry_opl_failure_flag: Indicates if there is any out of plane failure of walls in masonry building
  7. building_da_masonry_opl_ncfr_failure_flag: Indicates if there is any out of plane failure of walls (not carrying floor/roof) in masonry building
  8. building_da_masonry_gable_failure_flag: Indicates if there is any gable wall collapse in masonry building
  9. building_da_masonry_delam_failure_flag: Indicates if there is any delamination failure in masonry building
  10. building_da_rcc_beam_failure_flag: Indicates if there is any beam failure in RCC building
  11. building_da_rcc_infill_partition_failure_flag: Indicates if there is any Infill/Partition Wall Damage in RCC building
  12. building_da_nonstructural_staircase_flag: Indicates if there is any damage to the staircase in a building
  13. building_da_nonstructural_parapet_flag: Indicates if there is any damage to the parapet in a building
  14. building_da_nonstructural_clad_glz_flag: Indicates if there is any damage to the Cladding/Glazing in a building

Data cleaning for multi-select variables
----------------------------------------

Variables in the survey that allowed for multiple responses have been spread out into multiple columns. For example, the variable sup_str, which refers to the super structure of a given building, is represented in the data with 11 different columns ranging from sup_str1 to sup_str11, where the first response is located in sup_str1, and subsequent are assigned to increasing columns. After all responses from a particular survey have been assigned, the rest of the columns are assigned Null values. This type of column organization is present in both the building and household table. This format will be coerced into another structure where information will be captured in the form of flags.

Variable Renaming
-----------------

Variables will be renamed in order to make them more representative of the information they hold.


The summary and findings of the variable exploration can be found in the `following google sheets document <https://docs.google.com/spreadsheets/d/1Dk5oqEOvHdbazRGlz9hRJbCp4ODYnw6VJagvIPPwwyY/edit?usp=sharing>`_.

We have also compiled a list of tasks that will be carried out in each of the 8 tables, along with proposed new names for variables. `Please follow this link <https://docs.google.com/spreadsheets/d/16zCt06nJgZds5JioQdq3PzZ0DNZSgjuqnHQCxa3mkzo/edit?usp=sharing>`_ to view the same.
