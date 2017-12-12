#####################
Data related Callouts
#####################

As part of the data restructuring exercise, the development team has encountered the following inconsistencies in the base dataset that has been provided by CBS. Only inconsistencies that could not be explained have been listed below. For a richer, column wise list, please refer to the following document .


Main Table
##########

Missing values for variable *relation*
--------------------------------------
The team has observed 2 rows for which the *relation* field is empty, even when owner and respondent are not the same person (own_res_sme=2).


Missing values for variable *howner_reason*
-------------------------------------------
The team has observed 2 rows for which the *howner_reason* field is empty, even when owner and respondent are not the same person (own_res_sme=2).


Missing values for variable *soc_mob*
-------------------------------------
The team has observed 13,221 rows for which this variable is empty, even though the question was a compulsory question (as verified by KLL team members involved in the development of the app). Is there any reason why this could be the case?


Building Table
##############

Missing values for variable *sup_str1*
--------------------------------------
The team has observed 1 row for which the *sup_str1* field is empty. The team could not explain why this could be the case as this was a compulsory question.


Missing values for variable *positn* and *pln_conf*
---------------------------------------------------
The team has observed 1 row for which the *positn* and *pln_conf* field is empty. The team could not explain why this could be the case as these were compulsory questions.


Missing values for column *gersk*, *dm_grade*, *assd_ar*, *tech_sol*, *repair*
------------------------------------------------------------------------------
The team has observed 12 rows for which all of the above mentioned variables are null. The team could not explain why this could be the case as these were compulsory questions. In addition, the column *sec_use* is also empty for 10 of the 12 rows.


Missing values for variable *fam_cn*
------------------------------------
The team has observed 2 rows for which the *fam_cn* field is empty. The team could not explain why this could be the case as this was a compulsory question.


Household Table
###############

Missing values for variable *rel_hres*
--------------------------------------
The team has observed 149 rows for which the *rel_hres* field is empty, even when respondent is not the household head (Ishhd=2).


Missing values for column *cast*, *edu_levl*, *bank_acc*, *respreq*, *resposq*, *eqid_typ*, *income*, *death*, *loss*, *edrop_tf*, *vdrop_tf*, *oc_ch_tf*, *h2o_pre*, *h2o_pos*, *fir_pre*, *fir_pos*, *lit_pre*, *lit_pos*, *toilet_pre*, *toilet_pos*, *ast_pre1*, *ast_pos1* and *rahat1*
--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
The team has observed 228 rows for which all of the above mentioned variables are null. The team could not explain why this could be the case as these were compulsory questions.


Individual Table
################

Completely null entries
-----------------------
There are 79 entries for in the individual table for which all columns are null. The team could not explain why this could be the case.


Two people are mapped to the same ID
------------------------------------
There are 45 instances where more than one person is mapped to a combination of dist, vdc_mun, ward, EA, howner_sn, house_sn, hhd_sn and mem_sn. They are not duplicate entries as some values for certai variables are different for each entry.


Missing values for variable *disablty*
--------------------------------------
The team has observed 200 rows (excluding the 79 rows for which everything is empty) for which the *disablty* field is empty. Not sure why this could be the case as this was a compulsory question.


Missing values for variable *brth_reg*
--------------------------------------
All rows with age>5 should be null. However there are 90 cases where age <=5 and brth_reg is null. Not sure why this could be the case as this was a compulsory question.


Missing values for variable *edu_lvl*
-------------------------------------
All rows with age<5 should be null. However there are 3722 cases where age >=5 and edu_levl is null. Not sure why this could be the case as this was a compulsory question.


Missing values for variable *marital*
-------------------------------------
All rows with age<10 should be null. However there are 3664 cases where age >=10 and marital is null. Not sure why this could be the case as this was a compulsory question.


Missing values for variable *security1*
---------------------------------------
There are 3870 rows for which this variable is null. Not sure why this could be the case as this was a compulsory question.
