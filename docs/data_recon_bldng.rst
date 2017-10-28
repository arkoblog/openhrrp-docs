Building table
======+=======

**Note: This document captures information that may have been updated. Please refer to the :doc:`/2a_updates` section instead.**

Summary
-------

	1. Codes vs. Labels: The current dataset has codes assigned for each value of a categorical variable. For instance, the variable **gd_floor** which indicates the type of ground floor has the following codes associated with it:

		a. 1, which denotes "Mud"
		b. 2, which denotes "Brick/Stone"
		c. 3, which denotes "Timber"
		d. 4, which denotes "RC"
		e. 5, which denotes "Other"

		An alternate way of storing this information would be in the form of labels, as opposed to numbers. That is,

		a. mud, which denotes "Mud"
		b. brick_stone, which denotes "Brick/Stone"
		c. timber, which denotes "Timber"
		d. rc, which denotes "RC"
		e. other, which denotes "Other"

		Storing them as numbers require less space, but it comes at the cost of the user needing a reference for understanding what the number means.

		**What do we want to do?**


	2. Variable Types: A brief examination of the survey questionnaire suggests that questions that were asked fall in either one of the following two categories:

		a. Single select questions: Where the surveyor selects only one out of many options provided.
		b. Multi select questions: Where the surveyor can select one or more answers out of the many options provided.

		For every single select question in the survey, there is only one corresponding column in the data that capture the answer for a particular building. Data cleaning effort for these variables is minimal, depending on the route we take as per point (1)

	3. Multi select questions: Some of the questions, because of their multiselect nature has more than one column associated with them):

		a. Superstructure type has 11 columns ranging from **sup_str1** to **sup_str11**
		b. Type of geotechnical risk has 7 columns ranging from **gersk_ls1** to **gersk_ls3**
		c. Type of secondary use has 10 columns ranging from **secuse_ls1** to **secuse_ls10**

		For these questions, additional data cleaning work would be required to make information more usable.


	4. Missing Definitions: Not all of the columns had their corresponding definitions in the data dictionary provided by CBS. Whilst is was clear what information the column contained for a few of the columns (dist, vcode, vdcmun, ward, EA, howner_sn, house_sn, legl_own) based on their names, further information is needed for the following columns:

		a. **delam1**,
		b. **delam2**,
		c. **delam3**,
		d. **fam_cn**,
		e. **hgt_pre**,
		f. **hgt_pos**,
		g. **pl_area** (I assume this refers to the plinth area of the building, but how is it captured (numeric vs. categorical)? )
		h. **age** (I assume that this variable refers to the age of the building, but how is it captured (numeric vs. categorical))
		i. **floor_pos**,
		j. **floor_pre**,

		After examination of the survey qyuestionnaire, it appears most of the above information comes from a single table in the questionnaire:

		.. image:: _data/data_recon_building/missing_defs.jpg


	5. Damage Assessment Variables: Information for damage assessment is spread across groups of variables. For example, for users to get complete information on building foundation damage, they will have to go through three variables viz. **dm_fndtn1**, **dm_fndtn2**, **dm_fndtn3**. Other variables that have a similar nature include:

		a. **dm_roof1**, **dm_roof2**, **dm_roof3**
		b. **corn_sep1**, **corn_sep2**, **corn_sep3**
		c. **diag_cr1**, **diag_cr2**, **diag_cr3**
		d. **pl_fail1**, **pl_fail2**, **pl_fail3**
		e. **op_fail1**, **op_fail2**, **op_fail3**
		f. **op_fl_nl1**, **op_fl_nl2**, **op_fl_nl3**
		g. **dm_gabl1**, **dm_gabl2**, **dm_gabl3**
		h. **delam1**, **delam2**, **delam3**
		i. **col_fail1**, **col_fail2**, **col_fail3**
		j. **beam_fl1**, **beam_fl2**, **beam_fl3**
		k. **str_case1**, **str_case2**, **str_case3**
		l. **parapet1**, **parapet2**, **parapet3**
		m. **clad_glz1**, **clad_glz2**, **clad_glz3**
		n. **clad_glz1**, **clad_glz2**, **clad_glz3**

		Furthermore, information for "No damage" is contained as a categorical value within the first out of three variable, as illustrated by the picure below.

		.. image:: _data/data_recon_building/dm_fndtn1.png

		Suggestions for improvement:

		1. Rename column headers to include severity ie. dm_fndtn_severe, dm_fndtn_moderate, dm_fndtn_insignfcant
		2. Seperate information about no damage to a separate flag variable, dm_fndtn_flag


Methodology
-----------

*This section contains code used for the analysis, please jump to the next section for results*

A small subset of 5000 rows were taken as an input for analysis. All variables in the table were converted to factors, and a summary was yielded from each table.

.. code-block:: r

	# Exploration of building table from pgSql database
	library(RPostgreSQL)
	library(dplyr)

	root.dir <- "~/hrrp/openhrrp-src"

	pg <- dbDriver("PostgreSQL")
	con <- dbConnect(pg, user="postgres", password="postgres",
	                 host="localhost", dbname="openhrrp")

	# dbExistsTable(con, "building")

	df_building <- dbGetQuery(con, "SELECT * from building limit 5000; ")
	df_building <- as.data.frame(sapply(df_building, as.numeric))

	write.csv(as.data.frame(colnames(df_building)), file=paste0(root.dir, "/outputs/buildingColNames.csv"))

	df_building_af <- as.data.frame(sapply(df_building, as.factor))


	summarizeColumn <- function(column) {
	  return (summary(column))
	}

	summarizeColumn(df_building_af$fam_cn)



Results
-------

The table below outlines current status of the same:

.. csv-table::
   :file: _data/data_recon_building/buildingColNames_wStatus_221017.csv
