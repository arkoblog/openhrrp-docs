Household table
=============

Summary
-------

	1. Pre/Post
	2. Count Variables
	3. Rahat

	
Methodology
-----------

*This section contains code used for the analysis, please jump to the next section for results*

A small subset of 5000 rows were taken as an input for analysis. All variables in the table were converted to factors, and a summary was yielded from each table.

.. code-block:: r

	# Exploration of household table from pgSql database
	library(RPostgreSQL)
	library(dplyr)

	root.dir <- "~/hrrp/openhrrp-src"

	pg <- dbDriver("PostgreSQL")
	con <- dbConnect(pg, user="postgres", password="postgres",
	                 host="localhost", dbname="openhrrp")

	##
	# dbExistsTable(con, "household")

	df_household <- dbGetQuery(con, "SELECT * from household limit 5000; ")
	df_household <- as.data.frame(sapply(df_household, as.numeric))

	write.csv(as.data.frame(colnames(df_household)), file=paste0(root.dir, "/outputs/householdColNames.csv"))

	df_household_af <- as.data.frame(sapply(df_household, as.factor))


	summarizeColumn <- function(column) {
	  return (summary(column))
	}

	summarizeColumn(df_household_af$vdrop_cn)



Results
-------

The table below outlines current status of the same:

.. csv-table::
   :file: _data/data_recon_building/buildingColNames_wStatus_221017.csv