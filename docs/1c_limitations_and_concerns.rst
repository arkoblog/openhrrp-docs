################################
Current Limitations and Concerns
################################

The purpose of this document is to outline any limitations/concerns that may arise as a result of the nature, or structure of the housing reconstruction data:

Even though the housing reconstruction database contains data for 31 districts, it is important to note that the survey was conducted across the following 3 phases:

  * Phase 1 - Earthquake damage assessment for **all households** using the Census method in 11  **most affected** districts, viz. namely Dolakha, Ramechhap, Okhaldhunga, Sindhupalchok, Kavrepalanchok, Sindhuli, Makawanpur, Dhading, Nuwakot, Rasuwa and Gorakha.
  * Phase 2 - Earthquake damage assessment within Kathmandu valley using the Verification method, for **only those households which were enlisted by the local village and district authorities as earthquake affected households**.
  * Phase 3 - Earthquake damage assessment using the above mentioned verification method in 17 **less affected** districts, viz. Solukhumbu, Shankhuwasabha, Bhojpur, Dhankuta, Khotang, Chitwan, Nawalparasi, Arghakhanchi, Palpa, Gulmi, Syangja, Parbat, Baglung, Myagdi, Kaski, Tanahun, and Lamjung.

The fact that execution of the project spanned multiple phases and took a considerable amount of time for completion, means that there are several questions and concerns that will need to be addressed:

1. Administrative restructuring and its implications
####################################################

The survey was conducted at a time when the Survey Department was restructuring the administrative boundaries of Nepal, as per GoN’s directive of adapting a federal structure of government.
As a result, geographical information (Municipalities, VDCs, etc) in the data is not consistent, and may be based on either one of the two administrative structures. At this point, we do not know if:

  1. Geographical information for all of the buildings is based on the old administrative boundaries,
  2. Geographical information for some buildings is based on the old administrative boundaries, and the remaining are based on the newly adapted administrative boundaries.

  **Example case**

  *Mr. Lekhnath’s house was originally located in Ward No 3, in Dhading Besi, Dhading. During the survey, the surveyor marked Mr. Lekhnath as a resident of Ward 3, Dhading Besi, Dhading. After the administrative restructuring, the GoN decided to merge Wards 3 & 5 to form the new Ward no 13, Nilkantha Municipality.*

  *As per the latest administrative structure, Mr. Lekhnath should be considered as a resident of Nilkantha Municipality, Ward no. 13. However, the data says that Mr. Lekhnath is a resident of Ward 3, Dhading Besi, Dhading.*

  *This would lead to Mr. Lekhnath’s building not being counted as part of Nilkantha.*


Highlighted below are implication(s) of the same:

  1. Houses that belong to a particular municipality (based on new structure) will be incorrectly mapped to the older municipality.
  2. Point 1 will then lead to a situation where earthquake damage data that is aggregated at a municipality/ward level becomes slightly inaccurate/unreliable.


2. Phase-wise execution
#######################

In addition, the phase wise nature of execution also brings forth the following question(s) that need to be addressed:

  1. Even though the survey was conducted in phases, the data provided comprises a single table for all of the buildings. Are the district/vdc/mun codes used in the data all based on the old administrative boundaries?

Highlighted below are some implications(s) of the same:

  1. Statistics have to be presented differently for districts in Phase 1 (which comprises all buildings in the given districts and paints a fuller socio-economic picture) than for districts that were surveyed in the later phases.


3. Changing validation rules
############################

This is an indirect consequence of the phase wise nature of the project. Because of the fact that project requirements were constantly evolving over time, validations rules used during data collection did the same, for example,

  * some question(s) became compulsory questions at a later stage for the project after data had already started coming in,
  * some questions(s) checked answers in previous questions before allowing the users to move ahead.

This has the following implication(s):

  1. Unexplained null values and blank entries for certain compulsory questions at the early stages of the survey.

4. Scale of execution
######################

Given the sheer scale at which the survey was conducted (the project captured data for close to ~1.06MM buildings, ~5.77MM individuals), and the varying nature of the terrain in which it was carried out, its not unusual to expect a few variables with blank entries due to unexplained factors such as, but not limited to network disruption, device problems, server loads, etc.
Missing values for some variables have been observed in the data, however it is important to note that:

1. The proportion of missing values is very low (~ less than 1%) for most columns that DO have missing values.
2. Important variables (in the context of damage assessment) such as Damage Grade, Status of the Damage, Building Structure etc. have no missing values.

However, if there are cases where the number of missing values are significantly high, investigation into why that may happen will be carried out in collaboration with CBS.
