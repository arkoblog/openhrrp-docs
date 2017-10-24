Household table
===============

Summary
-------

	1. Pre/Post variables: A number of variables in the Household table seek to capture information about the difference in living conditions of the households, before and after the earthquake. These include:
		a. Residence (**respreq**, **resposq**)
		b. Source of Water (**h2o_pre**, **h2o_pos**)
		c. Source of cooking fuel (**fir_pre**, **fir_pos**)
		d. Source of light (**lit_pre**, **lit_pos**)
		e. Type of toilet (**toilet_pre**, **toilet_pos**)
		d. Type of fixed assets owned (**ast_pre**, **ast_pos**), this variable is also a multiselect variable (refer to 3.) 

 		One thing that we will need to pay attention to is regarding the way in which we aggregate this information at a building level, given that there may be more than one HHD per building.
	
	2. Variables with no definitions in the data dictionary: We weren't able to locate information for certain variables in the data dictionary. 
		a. Numeric: Some variables look like they may be of type *Numeric*. They include: 
			* **age**: It is not so clear as to what this variable represents (age of the respondent vs. age of the building?)
			* **hhd_size**: It may contain information around the size of the household, but not sure
			* **death_cn**:   Count of people who died post earthquake.  
			* **loss_cn**:   Count of people who were badly injured/missing post earthquake.
			* **edrop_cn**:   Count of people who left their education post earthquake.
			* **pdrop_cn**:   Count of pregnant women who stopped checkups earthquake.
			* **vdrop_cn**:   Count of  children who stopped getting vaccinated post earthquake.
			* **oc_ch_cn**:   Count of  people who had to change their occupation post earthquake.
			
			Further investigation is needed as to what these columns mean. 
		
		b. There were two additional variables without any definitions. These columns only contained NAs for the subset of data that we analyzed. **respreqd** and **resposqd** 

	3. Multiselect Variables: The pre/post variables **ast_pre** and **ast_pos** are multiselect variables and require treatment

	4. Special Case - Rahat: This column supposedly captures "the type of earthquake victim id card recieved" This also looks like a multiselect variable, but will need to confirm with the app developers as to why it was made a multiselect variable? And how values inside it are supposed to be interpreted.

	
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
   :file: _data/data_recon_hhd/householdColNames_wStatus_231017.csv