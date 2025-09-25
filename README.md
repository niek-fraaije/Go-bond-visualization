# Gō Bond Visualization for Coarse-Grained Models

This repository provides scripts and a Jupyter notebook to generate and visualize Gō bonds in coarse-grained (CG) molecular simulations.

It builds on existing tools: parts of MartiniGlass scripts were used to identify all possible Gō bonds, and a TCL script by Aster Kovacs was used to dynamically visualize these bonds in VMD.

## Overview

The workflow reads CG topologies and trajectories, extracts virtual sites, and applies Gō nonbonded parameters to generate dynamic bonds. These bonds are converted to TCL-compatible `.z` files for visualization in VMD using the `cg_bonds-v6.tcl` script.

The main features include:

- Parsing Martini CG topologies and Go-like nonbonded `.itp` files
- Identifying intra- and inter-molecular Gō bonds
- Mapping bonds onto molecular coordinates for each frame of a trajectory
- Generating compressed `.z` files compatible with VMD
- Support for dynamic visualization of bonds in CG systems

## Usage

1. Place your CG topology (`.top`) and structure (`.gro`) files, along with the trajectory (`.xtc`), in the working directory.  
2. Ensure the Go nonbonded parameter file (`go_nbparams.itp`) is present.  
3. Run the Jupyter notebook to:  
   - Generate intra- and inter-molecular bond lists  
   - Map bonds to the trajectory  
   - Write `.z` files for visualization (`inter_bonds.z` and `intra_bonds.z`)  
4. Visualize the bonds in VMD:  
   - Start VMD in TCL daemon mode:  
     ```bash
     vmd -e daemon.tcl
     ```  
   - Load your structure and trajectory:  
     ```tcl
     daemon_open no_water.gro whole.xtc
     ```  
   - Load the Gō bond files:  
     ```tcl
     daemon_bonds inter_bonds.z   ;# for inter-molecular bonds
     daemon_bonds intra_bonds.z   ;# for intra-molecular bonds
     ```  

> These commands display dynamic Gō bonds for each frame of your trajectory. Colors, widths, and other visualization options can be customized via the TCL script.

---

## Notes

- The bond lists are based on distance cutoffs derived from the Lennard-Jones minimum distance.
- The visualization is primarily for qualitative inspection; bond existence can fluctuate significantly in dynamic simulations.
- Maximum bonds per atom are limited (default: 12) to prevent unrealistic connectivity.

## Requirements

- Python 3.8+
- MDAnalysis
- NumPy
- tqdm
- zlib (standard library)
- VMD (for visualization)

## References

This work is inspired by the Martini coarse-grained force field and Gō-like modeling for protein structure visualization.

