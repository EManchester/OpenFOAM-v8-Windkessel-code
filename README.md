#	Windkessel code for OpenFOAM 8.0

This solver applies a three-element Windkessel boundary condition on up to 10 outlet boundaries  in OpenFOAM v8. For Windkessel code compatible with OpenFOAM v4 see my other repositories

Please cite this code using:
[1] Manchester, E. L., Pirola, S., Salmasi, M. Y., O’Regan, D. P., Athanasiou, T., and Xu,
X. Y. (2021). Analysis of Turbulence Effects in a Patient-Specific Aorta with
Aortic Valve Stenosis. Cardiovasc. Eng. Tech. 12, 438453. doi:10.1007/s13239-021-00536-9

Authors: Emily Manchester, Andris Piebalgs and Boram Gu and  Imperial College London, 2022.

## Compilation

Standard compilation procedure.
Clone the files and compile with 'wmake'. This will create a solver in $FOAM_USER_APPBIN.
Note that the Windkessel boundary condition is an OpenFOAM solver application, therefore we do not need to include any additional libraries.
Application name: pimpleFoam_WK

## Case set-up
In the case directory:

1. In 0/p, specify Windkessel outlet boundary condition as follows

    OUTLET_NAME
    {
	      type    WKBC;
	      index		0;
	      value		uniform 0;
    }

    The only user input is 'index' and is increased by an integer with each additional outlet.
    An example for multiple outlets is given in 'caseFiles/p'

2. Copy 'caseFiles/windkesselProperties' into the constant directory and update with outlet names.
   This is where the Windkessel properties are specified.
   User input:
   'C' is compliance, 'R' is resistance and 'Z' is characteristic impedance.
   'outIndex' must equal the 'index' value set in 0/p
   'FDM_order' sets the temporal discretisation order using finite backward difference. Up to 3rd order.

    The following is used to initialises the flow rate and pressure, based on the selected 'FDM_order'. E.g. 3rd order FDM requires three entries for flowrate and pressure, respectively.
    If the simulation ends and is subsequently run on, these values need to be updated as they are used to initialise the subsequent simulation. Replace constant/windkesselProperties file with windkesselProperties copied from any of the processor directories (e.g. processor0/constant/windkesselProperties). The windkesselProperties files are saved to the processors at run time, as specified by writeInterval in the controlDict.

    OUTLET_NAME
    {
    	...

    Flowrate_threeStepBefore      	0;
    Flowrate_twoStepBefore        	0;
    Flowrate_oneStepBefore        	0;
    Pressure_twoStepBefore        	0;
    Pressure_oneStepBefore        	0;
    Pressure_start                	0;
    }
