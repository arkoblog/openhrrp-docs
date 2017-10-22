######################
Questions and Concerns
######################

The purpose of this document is to highlight any key questions and/or concerns that we will need to address during the course of this project.


Administrative restructuring and its implications
#################################################



What?
^^^^^
The survey was conducted at a time when the Survey Department was restructuring the administrative boundaries of Nepal, as per GoN's directive of adapting a federal structure of government. As a result, geographical information (Municipalities, VDCs, etc) in the data is not consistent, and may be based on either one of the two.

At this point, we do not know if:

1. Geographical information for all of the buildings is based on the old administrative boundaries,
2. Geographical information for some buildings use the old administrative boundaries, and the remaining use new administrative boundaries. 

Why is this a problem?
^^^^^^^^^^^^^^^^^^^^^^
Providing aggregated information at a VDC or a municipality level would prove to be a challenge, as we can't be sure what VDC (based on the new admin structure) the building currently falls in.

How can we solve this?
^^^^^^^^^^^^^^^^^^^^^^

If we have,
	* the geo-location of the building,
	* shapefiles/geojsons for the new admin-boundaries at a ward level,
We could re-map buildings to the correct VDC/Wards, and use that information going forward.