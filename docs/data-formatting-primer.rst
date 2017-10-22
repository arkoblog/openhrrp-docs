##############################################
Housing Reconstruction Data: Formatting Primer
##############################################


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
	| Increments from: all administrative codes (ward, vdcmun, ward, ea)
	| Connects to: house_sn
	| Comments: This code identifies house owners within an administrative unit (multiple ownership not permitted). It allows buildings to be tied to a specific owner. Each building has a house owner, but not every building has a /textitunique house owner (multiples are allowed).
	