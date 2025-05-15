# C<sub>α</sub> model: Coarse-Grained Protein Folding and Fold Switching Simulations

This C code implements a one bead-per-amino acid coarse-grained
model for protein folding and protein fold switching, with a structure-based potential.
Conformational sampling is carried out using Langevin dynamics.

This repository developed in the [Wallin Lab](https://www.physics.mun.ca/~jswallin/index.html) under the supervision of **Dr. Stefan Wallin**. The code can be used to simulate **protein folding and fold switching** for systems for which structural information is available for the proteins. Macromolecular crowding effects can be included.  

## How to cite

If you use the C<sub>α</sub> model or build upon it in your own work, please cite the following publications:

1. Simulations of a protein fold switch reveal crowding-induced population shifts driven by disordered regions
S Bazmi, B Seifi, S Wallin
Communications Chemistry 6, 191 (2023)
[Read on Nature](https://www.nature.com/articles/s42004-023-00995-2)

This study uses the C<sub>α</sub> model to explore how macromolecular crowding impacts protein fold switching, revealing the role of intrinsically disordered regions in driving population shifts between conformational states.


2. Conformational entropic barriers in topology-dependent protein folding: perspectives from a simple native-centric polymer model
S Wallin, H.S. Chan
Journal of Physics: Condensed Matter 18, S307–S328 (2006)
[Link to paper](https://iopscience.iop.org/article/10.1088/0953-8984/21/32/329801/pdf)

This paper outlines the theoretical framework behind structure-based coarse-grained protein models and gives details on the Langevin dynamics approach implemented in the code. 



## Files and definitions:

🔧 defs.h:

Parameter definitions for the model and molecular dynamics procedure, including force-field selection (FF_BOND, FF_CONT, ...), temperature, numerical integration parameters, input/output filenames, etc. 

🔧 global.h

Declararations of global variables and arrays (positions, velocities, forces, energy terms, etc.) accessible across most other files, including geometry.c, energy.c, etc.

🔧 geometry.c

Handles the spatial representation of chains and crowders. Includes functions for distance calculations, periodic boundary conditions, and transformations between angular degrees of freedom (bonds, angles, torsions) and Cartesian coordinates. 

🔧 energy.c

Functions for energy and force calculations, including bonded (bond, angle, torsion) and non-bonded (excluded volume, contacts, crowder) interactions. For example, it provides functions like bond(), cont(), crowd_bead(), and crowd_crowd() to compute interaction potentials and forces.

🔧 obs.c

Computes observables like number of native contacts (no_cont), RMSD, radius of gyration, and collects statistics for histograms for bond lengths, angles, and contact maps. These are used to analyze simulation trajectories and structural properties.

🔧 misc.c

Provides misceleneous functions for simulation and control settings, including initialization (printinfo()), output of averages, checkpointing (write_checkpnt()), and trajectory exports (dumppdb()).

🔧 utils.c

Includes various file-based utilities such as reading native structure files, contact maps, and writing PDB files. Also includes custom random number generators (ran3n) and PDB format export for visualization.

🔧 sampling.c

Implements the Langevin dynamics integrator and simulated tempering routine. It handles probabilistic temperature swapping in the simulated tempering scheme, enabling enhanced conformational sampling across temperature landscapes.

🔧 main_fixtemp.c

Main simulation driver for fixed-temperature molecular dynamics. It runs Langevin dynamics at a single temperature, periodically sampling observables and saving system configurations.

🔧 main_simtemp.c

Simulation driver for simulated tempering protocol. It includes temperature flipping based on Metropolis-Hastings algorithm and adaptive weight updates (update_g). Enables exploration of conformational space by dynamically switching temperatures (Marinari and Parisi, 1992)[Link to paper](https://iopscience.iop.org/article/10.1088/0953-8984/21/32/329801/pdf). 

📌 A minimal sequence of commands to compile the code and run a
simulation of a single chain at a fixed (and several) temperature is:

make constants

./constants input 1

make fixtemp (simtemp)

./main

## Results

🎯 Communications Chemistry 6, 191 (2023)

### Simulating the GA/GB fold-switch system:
![Figure 1](figures/a-1.png) 


 A representative experimental structures of the G<sub>A</sub> and G<sub>B</sub> folds shown in ribbon: G<sub>A95</sub> (PDB id 2KDL; blue) and G<sub>B95</sub> (PDB id 2KDM; orange), and contact maps. Population P of the G<sub>A</sub> (triangles) and G<sub>B</sub>(circles) folds as functions of the G<sub>B</sub> contacts strength, κ<sub>B</sub>.

### Population shift:
<img src="figures/a-2.png" width="220"/> 

Crowding causes a shift in fold populations favoring G<sub>B</sub> over G<sub>A</sub>. 


🎯 J. Phys.: Condens. Matter (2006)


<img src="figures/b-1.png" width="260"/> 
Simulated folding rates show strong correlation with experimental data, validating the native-centric polymer model.


-----------------------------------------------------------


Contact: Stefan Wallin, swallin@mun.ca

