# Sensitivity Analysis #

## Written by Jian (Jane) Yin ##
2017.10


This code computes the analytical first-order derivatives of binding free energy with respect to van der Waals parameters.

To run the sensitivity analysis scripts, you need the Amber and parmEd programs installed on your machine.

## Usage ##

First of all, use the Python script provided here to generate molecular information (force field parameters and connectivity i.e. bonds, angles and torsions)  of your system
from the topology file.

For example:

	python parseTopoWithParmed.py full.topo

Only the Amber prmtop format is supported for the topology file (full.topo in this example).

Secondly, run the sensitivity executable with three flags: -crd, -cutoff and -gpu. 

Flags:
  -crd       MD trajectory file
  -cutoff    cutoff distance of nonbonded interactions
  -gpu       yes or no

Example:

	./sensitivity -crd traj.mdcrd -cutoff 9.0 -gpu yes

Only the Amber mdcrd format is supported for the coordinate file. If your trajectory uses the NetCDF or PDB format, you can use cpptraj or other programs to convert it to mdcrd. 
The CUDA feature is currently under construction, so please use option "no" for the gpu flag at this point.

The cutoff distance should match the abrupt cutoff of van ver Waals interaction used in simulations. The sensitivity code does not take into account the long-range corrections 
of the van der Waals interactions possibly used in MD simulations, but the influence to the predicting power of the derivatives is neglectable. 

The derivatives of each frame and the mean values will be printed in radDerivatives.dat (derivatives to the radius) and epsDerivatives.dat (derivatives to the epsilon).
Note that in GAFF radius is used rather than sigma or R_min. So if you are interested in those two qualities, the derivatives need to be converted accordingly.

If you would like to modify the script, issue

	make -f makefile
 
to regenerate the executable after you modify the code. 