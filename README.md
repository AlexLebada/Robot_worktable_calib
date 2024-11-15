In this case the ABB robot goal is to put silicon gasket onto a metal sheet, which is prepared alternatively on an advanced table after another one was retracted for preparations . After decommissioning, transportation and reinstallation of the robotic cell, for proper movements, the robot coordinate systems needs to be aligned with the two work tables . 










(image only for reference)
    These coordinate systems are  ‘wobj_table_u’ and ‘wobj_table_d’ defined by some data structure, for example: 

wobjdata ‘name’:=[FALSE,TRUE,";",[[1128.8,1854.5,946.4],[0.00280846,0.00237967,0.999975,-0.0059719]],[[0.00417233,0.000238419,0.000119209],[1,-4.60841E-06,3.67227E-05,1.22864E-05]]], 
Where
[1128.8,1854.5,946.4] – represents the origin vector position related to the base reference of the robot,
[0.00280846,0.00237967,0.999975,-0.0059719] – orientation data in quaternions related to the base reference of the robot

	Process of adjusting robot references

	For translation misalignment:
	 simply put the robot TCP in the current ‘wobj_table_u’ /‘wobj_table_d’ and measure with a calliper for each axis (X, Y, Z) from robot TCP to the margins of the work table. Make the calculations and update the vector position for each wobjdata.













	For correcting the rotation angles:
	 a magnetic dial comparator is mounted on the side of the robot gripper and gauge head touching the table in the X,Y, Z direction while the robot is moving  L distance to get the error

L - distance traveled by the robot
d – Dial comparator measuring value after L distance
r – the actual table X axis










	Because the rotation is defined as quaternions we need to call changeWobj() to make the transformation for us after we calculate the angle error in [deg] and [rad] ( Tabel 1,2):
				tan⁡〖θ=dx/L〗


	Check the actual angles at point P by using EulerZYX() which gets us values in [deg]
	If is too high we use OrientZYX() to change the wobjdata in the program.
	We do this steps for each axis rotation a few times, until we read a minimum value that doesnt affect anymore the pouring position


