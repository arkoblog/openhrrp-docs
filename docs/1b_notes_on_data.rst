###################################################################
Housing Reconstruction Data: Notes on overall data structure/format
###################################################################

:Authors:
	Bradley Wilson

:Version: 1.0.0 as of 2017/10/22

Data Structure
##############

Data Tables
^^^^^^^^^^^

The HRHRP data comes as *8 .sav files*. For further processing, it is preferable to convert them to a format usable in a more flexible programming language (R, Python). The 8 tables are listed as follows:

	* Building: records for each surveyed structure
	* Household: records for each surveyed household
	* Individual: records for individuals in each surveyed household
	* HouseOtherPlace: damage information for household’s secondary buildings (if present)
	* HouseOwner: gender of house owner
	* InjuredMissing: records for missing and injured individuals
	* Death: records for earthquake fatalities
	* Main Table: survey collection details

Of the 8 tables, the first three (building, household, and individual) contain the bulk of the data. The data includes data for approximately one million households and structures, and five million individuals.


Adminsitrative Coding
^^^^^^^^^^^^^^^^^^^^^

Each record in the data has four administrative codes: district, vdcmun, ward, enumeration area. These codes were collected in the middle of Nepal’s administrative restructuring, and are therefore not coded in the new Nagarpalika/Gaunpalika units. However, some of the municipality aggregations that happened between 2011 and 2015 are represented in the data. Expect to spend some time manually working through administrative re-coding. The administrative units that correspond to the codes in the data are detailed in the XML files that come with the data. There isn’t a list of all areas that were affected by changes, although the included excel spreadsheet provides
the official names and codes compared to other common codes.

Depending on how one wants to link administrative codes to polygon boundaries, the codes in the data may or may not be sufficient. The district, municipality, and ward information do not directly correspond to the ‘hlcit’ or ‘hrrp’ codes that are present in many shapefiles/geojson files for Nepal. See included CSV codefix files for mapping transformations. ‘Codefix’ maps district and vdc/muni data to hlcit codes, and ‘codefix_hrrp’ maps hlcit codes to hrrp codes, with corresponding names. These files don’t represent all the potential coding systems one may encounter, but should provide a starting point for spatial joins.


Survey-Specific Coding
^^^^^^^^^^^^^^^^^^^^^^

Beyond the administrative codes, there are an additional four serial codes that may be attached to each record of data. These codes establish the inter-table relationships and predicate all join operations. Unless otherwise specified, all codes start at one, and reset at the next nested level.

For example, house serial number increments when a house owner has multiple buildings under ownership, and resets to one for the next house owner. Therefore, these extra serial codes construct a hierarchical structure extending beyond the most detailed administrative level (district+vdc/mun+ward+ea). The survey specific codes are as follows:

	1. **howner_sn: House Owner Serial Number**
		* *Increments from*: all administrative codes (ward, vdcmun, ward, ea)
		* *Connects to*: house_sn
		* *Comments*: This code identifies house owners within an administrative unit (multiple ownership not permitted). It allows buildings to be tied to a specific owner. Each building has a house owner, but not every building has a unique house owner (multiples are allowed).

	2. **house_sn: House Serial Number**
		* *Increments from*: howner_sn
		* *Connects to*: hhd_sn
		* *Comments*: This code identifies the building number for a particular house owner. It allows households to be tied to a specific building. Most buildings have a single household tied to them, but some have none and others have multiple.

	3. **hhd_sn: Household Serial Number**
		* *Increments from*: house_sn
		* *Connects to*: mem_sn
		* *Comments*: This code identifies a particular household within a building. All households are tied to a building, but households may be tied to the same building.

	4. **mem_sn: Household Member Serial Number**
		* *Increments from*: hhd_sn
		* *Connects to*: N/A
		* *Comments*: This code identifies the family member within a particular household.

Table Columns
^^^^^^^^^^^^^

All of the categorical data in the survey has been converted into numeric codes. With the exception of columns for age (both building and individual) all data are categorial. Each data table contains both categorical and ordered categorical columns. The coding scheme used to relate the numeric codes in the data tables to categorical survey responses are provided in the XML files present with
the data. It should be noted that the coding scheme is variable specific—the codes as given are not suitable for likert-like analysis and are not consistent throughout the data.

Variables in the survey that allowed for multiple responses have been spread out into multiple columns. For example, the variable sup_str, which refers to the super structure of a given building, is represented in the data with 11 different columns ranging from sup_str1 to sup_str11, where the first response is located in sup_str1, and subsequent are assigned to increasing columns. After all responses from a particular survey have been assigned, the rest of the columns are assigned Null values. This type of column organization is present in both the building and household table. Coercing this format into another data structure may be preferable for further analysis.

Table Joins
###########

Careful attention should be placed on table join operations. Each of the three primary tables (building, household, and individual) represent different observational units. The relationships between tables are mostly straightforward (single household buildings with a series of individuals), but there are a number of other situations that must be handled with care. Three common join
operations are outlined below:

Households to Buildings
^^^^^^^^^^^^^^^^^^^^^^^
It may be desired to add household information to building records. In this situation, two cases must be handled.

	1. Situations where multiple households are tied to a single building record.
	2. Situations where no households are tied to a building record.

For case (1), data can be aggregated (sum, mean, median, mode, etc.) or pulled from a single household. Each column can be handled
differently, if desired (it might make sense to take the median income level, but sum household members for example).

For case (2), building records without household data can be dropped, or left in the new table with empty columns.

Buildings to Households
^^^^^^^^^^^^^^^^^^^^^^^

The opposite action might also be desired, adding building information to household records. This procedure requires the same cases to be handled. It should be noted however, that if a left join operation is used, duplicate building records (for multiple household cases) will be added automatically. Additionally, buildings that don’t match households will automatically be dropped from the new table. If this is not desired, a different join operation (full join, right join) should be used.

Individuals to Households
^^^^^^^^^^^^^^^^^^^^^^^^^
The individual data contains additional information that may help characterize the socio-economic position of households. Joining the individual table to the household table is the most straight-forward of the join operations, as there are no special cases (as far as I am aware). However, this process requires calculating summary statistics at the household level. Most commonly, this will involve binary presence variables (i.e. disabled member present yes/no), counts (i.e. number of members <5), or percentages (i.e. % members abroad).


Recommendations
###############

	1. Determine a coding structure and stick to it. Creating a single code that merges all administrative levels together is recommended. One might even consider creating a few codes at different levels. It doesn’t matter so much what the coding structure is as long as it is consistent.

	2. Decide how to handle the case of multi-households. Perhaps using a combination of modes, sums, and medians to create a ‘household profile’ of sorts. We're not sure this is the best option (it may be preferable to use a single household’s data if the households are similar). It might also be worth flagging the households that are using aggregated data to let users know where household data is actually a single household and where it isn’t. Perhaps by counting the total households in a particular building and adding that number as a column.

	3. Consider data usage in formatting decisions. This is particularly relevant where duplications are being added to the data. For example, if buildings are joined to households with a left join, duplicate building records will be added. If building statistics are calculated from this new table (without accounting for duplicates), they will not be correct. Ultimately this is also the responsibility of the user, but consider what might be intuitive and might not be.
