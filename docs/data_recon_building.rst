===================
Data Reconciliation
===================

The purpose of this document is to reconcile table related information present in the documents shared by CBS, with that on the SAV files. 

Bulding table
=============

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
		b. Multi select questions: Where the surveyour can select one or more answers out of the many options provided.   

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
   :widths: 10 10 20 10 10 10 30
   :file: _data/buildingColNames_wStatus_221017.csv