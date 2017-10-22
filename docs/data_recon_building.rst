===================
Data Reconciliation
===================

The purpose of this document is to reconcile table related information present in the documents shared by CBS, with that on the SAV files. 

Bulding table
=============

Methodology
^^^^^^^^^^^

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
^^^^^^^

1. Not all of the columns had their corresponding definitions in the data dictionary provided by CBS. Whilst is was clear what information the column contained for a few of the columns (dist, vcode, vdcmun, ward, EA, howner_sn, house_sn, legl_own) based on their names, further information is needed for the following columns:

	* **delam1**, 
	* **delam2**, 
	* **delam3**,
	* **fam_cn**,
	* **hgt_pre**,
	* **hgt_pos**,
	* **pl_area** (I assume this refers to the plinth area of the building, but how is it captured (numeric vs. categorical)? )
	* **age** (I assume that this variable refers to the age of the building, but how is it captured (numeric vs. categorical))
	* **floor_pos**,
	* **floor_pre**,

2. Some columns have null values despite having a code for NA (9 usually). These are mostly damage assessment columns such as, but not limited to:

	* Diagonal Cracking
	* Corner Wall Seperation, etc.

3. Some of the questions, because of their multiselect nature has more than one column associated with them):

	* Superstructure type has 11 columns ranging from **sup_str1** to **sup_str11** 
	* Type of geotechnical risk has 7 columns ranging from **gersk_ls1** to **gersk_ls3** 
	* Type of secondary use has 10 columns ranging from **secuse_ls1** to **secuse_ls10** 

	*It is required to convert information contained in these tables to binary flag variables for ease of use.* 


The table below outlines current status of the same:

.. csv-table::
   :widths: 10 10 20 10 10 10 30
   :file: _data/buildingColNames_wStatus_221017.csv