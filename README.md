# AndesML
Trying to find Porphyry Copper deposits in the Andes

Instructions for use

*Load the Muller et al. 2016 rotation and topology files into GPlates and Export subduction topologies as outlined in "convergence.py".


*Copy out all the line segments. This hack is required because in some cases GPlates (the older versions did not do this) will split the topologies into "points" and "lines", we just want have to rename the "lines" files if this occurs for "convergence.py" to read. Use this bash snippet or similar to find and rename the shape files.

for age in {0..230}; do echo ${age};cp Muller_export/topology_subduction_boundaries_sL_${age}.00Ma/topology_subduction_boundaries_sL_${age}.00Ma_polyline.shx Muller_export/topology_subduction_boundaries_sL_${age}.00Ma.shx;
cp Muller_export/topology_subduction_boundaries_sL_${age}.00Ma/topology_subduction_boundaries_sL_${age}.00Ma_polyline.shp Muller_export/topology_subduction_boundaries_sL_${age}.00Ma.shp;
cp Muller_export/topology_subduction_boundaries_sL_${age}.00Ma/topology_subduction_boundaries_sL_${age}.00Ma_polyline.prj Muller_export/topology_subduction_boundaries_sL_${age}.00Ma.prj;
cp Muller_export/topology_subduction_boundaries_sL_${age}.00Ma/topology_subduction_boundaries_sL_${age}.00Ma_polyline.dbf Muller_export/topology_subduction_boundaries_sL_${age}.00Ma.dbf; done

for age in {0..230}; do echo ${age};cp Muller_export/topology_subduction_boundaries_sR_${age}.00Ma/topology_subduction_boundaries_sR_${age}.00Ma_polyline.shx Muller_export/topology_subduction_boundaries_sR_${age}.00Ma.shx;
cp Muller_export/topology_subduction_boundaries_sR_${age}.00Ma/topology_subduction_boundaries_sR_${age}.00Ma_polyline.shp Muller_export/topology_subduction_boundaries_sR_${age}.00Ma.shp;
cp Muller_export/topology_subduction_boundaries_sR_${age}.00Ma/topology_subduction_boundaries_sR_${age}.00Ma_polyline.prj Muller_export/topology_subduction_boundaries_sR_${age}.00Ma.prj;
cp Muller_export/topology_subduction_boundaries_sR_${age}.00Ma/topology_subduction_boundaries_sR_${age}.00Ma_polyline.dbf Muller_export/topology_subduction_boundaries_sR_${age}.00Ma.dbf; done


*Now you can run the convergence script on the topologies for all time steps, using something like this.

for age in {0..230}; do echo ${age};  python convergence.py Muller_gplates/Global_EarthByte_230-0Ma_GK07_AREPS.rot Muller_export/topology_subduction_boundaries_sL_${age}.00Ma.shp Muller_export/topology_subduction_boundaries_sR_${age}.00Ma.shp Muller_export/topology_platepolygons_${age}.00Ma.shp ${age}; done


*The should output a collection of files labelled as "subStats_0.csv" for each time interval. These are the calculated plate kinematics data.


*The *.pkl files, as produced in Butterworth et al 2016 require an additional step to create. This is the bit that 'co-registers' the copper deposits with the kinematic data from convergece.py and the agegrids. You have to run the "coregLoop" function within Utils_coreg.py 

There are a few manual changes that may need to be done as the Agegrid data, and directories are hard coded. But in principle it is easy , just use the line in python: 

#Read in a shapefile
andeanPoints="XYBer14_t2_ANDES.shp"
[recs,shapes,fields,Nshp]=readTopologyPlatepolygonFile(andeanPoints)

#Run the coregistration
coregData=coregLoop(recs,shapes,fields,Nshp,3)

#Then you can save the data as a pickle file or do whatever you like with it!
