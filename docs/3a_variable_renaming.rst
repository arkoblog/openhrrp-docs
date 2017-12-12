##################################
Variable renaming and reformatting
##################################

The variable renaming exercise was carried out to ensure variable names are representative of the data that they contain. The following guiding principles were used for the same:

  1. **Flag variables** have been prefixed with 'has' or 'is'. All flag variables cant take one of the following states:
      - 0, which is indicative of a FALSE state.
      - 1, which is indicative of a TRUE state.
      - NA, in case of Null values.
  2. The data type for all categorical variables have been enforced to INTEGER, i.e. NUMERIC.
  3. The data type for all numeric variables has been enforced to DOUBLE PRECISION, i.e. FLOAT.
  4. Unique IDs identifying each row in a table has been created, these are suffixed with "_id" for clarity. For eg, household_id = record_id (which is equal to dist + vdcmun + ward + EA + howner_sn) + house_sn + hhd_sn
  5. All variable names are in lowercase.


The below section contains information on the new names of columns across all eight tables.


Common Variables
^^^^^^^^^^^^^^^^

Location variables
------------------

Variables that identify locations across all tables have been refirmatted to also contain information about their parent location. Based on this, following are the new location columns that identify locations at different levels:

  1. **district_id** : "dist" column has been renamed to district_id and is of CHARACTER TYPE of length 2.
  2. **vdcmun_id** : "vdcmun" column has been updated to also contain information about parent district in order to make queries at a vdc level more efficient. The type has been set to CHARACTER, and the variable has a length of four characters. The first two letters correspond to the VDC's parent district (eg. 2302 stands for VDC 02 in District 23).
  3. **ward_id** : "ward" column has been updated to also contain information about parent vdc and district in order to make queries at a ward level more efficient. The type has been set to CHARACTER, and the variable has a length of six characters. The first four letters correspond to the ward's parent vdc and district (eg. 230201 stands for Ward no 2, in VDC 02 of District 23).

Unique identifier (ID) variables
--------------------------------

To facilitate faster table join operations the following primary and composite keys will be generated across all eight tables:

  1. record_id: This ID will uniquely identify each record that was captured in the survey.
  2. building_id: This ID will uniquely identify each building for which data has been recorded in the survey.
  3. household_id: This ID will uniquely identify each household for which data has been recorded in the survey.
  4. individual_id: This ID will uniquely identify every individual for whom data has been recorded in the survey.
  5. injured_missing_individual_id: This ID will uniquely identify every injured/missing individual for whom data has been recorded in the survey.
  6. deceased_individual_id: This ID will uniquely identify every deceased individual for whom data has been recorded in the survey.

*Note: As was observed in the data restructuring exercise, there are instances in the individual table where the there are instances of multiple records for individuals with the same district, vdcmun, ward, EA, howners_sn, house_sn, hhd_sn and mem_sn. Because of this, even the composite IDs mentioned above will not be unique for every individual. To counter this, a unique 'id' column of the type  AUTO_INCREMENT has been generated through PostgreSQL for each and every table.*


Table specific variables
^^^^^^^^^^^^^^^^^^^^^^^^

Main Table
----------

Variables in the main table (apart from location and ID variables) have been renamed as per the table below.

.. csv-table::
   :file: _data/variable_renaming/main_table.csv

House owner
-----------

Variables in the house owner table (apart from location and ID variables) have been renamed as per the table below.

.. csv-table::
  :file: _data/variable_renaming/house_owner.csv

House other place
-----------------

Variables in the house other place table (apart from location and ID variables) have been renamed as per the table below.

.. csv-table::
  :file: _data/variable_renaming/house_other_place.csv

Building
--------

Variables in the building table (apart from location and ID variables) have been renamed as per the table below.

.. csv-table::
  :file: _data/variable_renaming/building.csv


In addition, the following new variables have been added to the building table.

.. csv-table::
  :file: _data/variable_renaming/building_new_variables.csv


Household
---------

Variables in the household table (apart from location and ID variables) have been renamed as per the table below.

.. csv-table::
  :file: _data/variable_renaming/household.csv


In addition, the following new variables have been added to the household table.

.. csv-table::
  :file: _data/variable_renaming/household_new_variables.csv
