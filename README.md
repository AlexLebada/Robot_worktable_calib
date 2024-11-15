In this case the ABB robot goal is to put silicon gasket onto a metal sheet, which is prepared alternatively on an advanced table after another one was retracted for preparations . After decommissioning, transportation and reinstallation of the robotic cell, for proper movements, the robot coordinate systems needs to be aligned with the two work tables . 

![image](https://github.com/user-attachments/assets/f73cac7f-805b-42da-9e04-161ad9bac47f)

(*image only for reference*)


These coordinate systems *‘wobj_table_u’* and *‘wobj_table_d’* are defined by some data structure, for example:

###### wobjdata ‘name’:=[FALSE,TRUE,";",[ $${\color{Orange}[1128.8,1854.5,946.4]}$$, $${\color{Green}[0.00280846,0.00237967,0.999975,-0.0059719]}$$ ],[[0.00417233,0.000238419,0.000119209],[1,-4.60841E-06,3.67227E-05,1.22864E-05]]], 
Where<br/>

$${\color{Orange}[1128.8,1854.5,946.4]}$$ – represents the origin vector position relative to the base reference of the robot,

$${\color{Green}[0.00280846,0.00237967,0.999975,-0.0059719]}$$ – orientation data in quaternions related to the base reference of the robot <br/>
<br/>
	
### Process of adjusting robot references

IDE: RobotStudio ABB\
<br/>
1. For translation misalignment:
  - simply put the robot TCP in the current *‘wobj_table_up’ /‘wobj_table_down’* and measure with a calliper for each axis (X, Y, Z) from robot TCP to the margins of the work table. Make the calculations and update the vector position for each *wobjdata*.

<img src="https://github.com/user-attachments/assets/2900311c-f2f4-4e61-89fe-3faa61f51177" width="400" height="250">
<br/>
<br/>

2. For correcting the rotation angles:
  - a magnetic dial comparator is mounted on the side of the robot gripper and gauge head touching the table while the robot is moving L distance, in the X,Y, Z direction  to find the error

	L - distance traveled by the robot\
	d – Dial comparator measuring value after L distance\
	r – the actual table X axis orientation



<img src="https://github.com/user-attachments/assets/17e76a00-5159-47a5-aaf4-5cd52821159e" width="300" height="200">



   - Because the rotation is defined as quaternions we need to call *changeWobj()* to make the transformation for us after we calculate the angle error in [deg] and [rad] ( Tabel_1):

$$\tan{\theta} = \frac{dx}{L}$$

  - Check the actual angles at point P by using *EulerZYX()* which gets us values in [deg]
  - If is too high we use *OrientZYX()* to update the *wobjdata* in the program.
  - We do this steps for each axis rotation a few times, until we read a minimum value that doesnt affect anymore the pouring position


