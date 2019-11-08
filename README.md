# PMGImage


# PMG mesh generator

## Code Running Tutorial on HPC

```bash
#On Login Node
# 10 mins testing allocation (interactive mode) on single node with 16 cores
qsub -I -l nodes=2:ppn=16,walltime=00:20:00 -q workq -A hpc_pmg2019

#On Computing Node

cd /work/bwang31/Berea/100_100_100

#Prepare input file
emacs psmesh.in
emacs pm2mesh501.in
emacs phasemap.in

#Set up local mesh refinement file
emacs temp_poi.dat

#Run mesh generator
./fullmesh_vox.exe 

#Convert mesh into nodes/elements.dat
./createvmesh_vox.exe

#clean temp file
rm temp_ibn* meshdata* imat*

#[optional] Convert PMG Mesh into Gmsh
PyGeoMesh

#log out
exit 0
```


## Fenics install on PSC Bridges
```bash
module load mpi/intel_mvapich
module load anaconda3/5.2.0

source activate fenics
cd /pylon5/sc5fp4p/niushi20/Case3_3DSuareDuct/

#On Login Node
# 30 mins testing allocation (interactive mode) on 2 nodes with 28 cores
interact -p RM-small -t 00:30:00
```


## Input File Documentation

1. psmesh.in for fullmesh_vox.exe
```bash
'INPUT FILE FOR PSMESH ALGORITHM (v. 1.01)'

'DOMAIN'
'  Domain Type (Voxel=V; Geometric=G)'			V

'VOXELFILEINFO'                                     #AscII image file name
'  Filename for base voxel data (segmented):'		Berea200_200_200.txt  
'  File type (1: ASCII; 2: Binary short):'		1
'  Total Voxel dimensions (NVX,NVY,NVZ):'		200,200,200
'  Meshed region from file (T/F)'			F
'  Region to Mesh (Xlow,Xhigh):'			1,200
'                 (Ylow,Yhigh):'			1,200
'                 (Zlow,Zhigh):'			1,200

'GEOMINFO'                                  #Only be used for Geometric Domain
'  Domain dimensions x dimension:'			1.
'                    y dimension:'			4.
'                    z dimension:'			1.

'FULLMESH'                                  #Background Element size, Interface element size, Min element size 
'  Base characteristic lengths:'			15.0,14.0,0.01          
'  Call CREATEEDGEPOINTFILE routine (T/F):'		F

# min tet size = 1.0 voxel, it is not make sense to have mesh size lower than image resolution

#Other options keep default
```

2. phasemap.in for for fullmesh_vox.exe
```bash
3       #Number of phases
0 1     #Phase voxel value, 1 = meshing phase, 0-non meshing phase
125 1
255 1
```

3. pm2mesh501.in for createvmesh_vox.exe
```bash
'VOXELFILEINFO'                                     #AscII image file name
'  Filename for base voxel data (segmented):'		Berea100_100_100.txt
'  File type (1: ASCII; 2: Binary short):'		1
'  Voxel dimensions (NVX,NVY,NVZ):'			100,100,100
'  Phase range to mesh sequential numbers (T/F)?'	T
'     If T, Phase range to mesh (IP1,IP2 inclusive):'	0,0

'CUTOUT'
'  Cutout region (xlow,xhigh):'				1,100
'                (ylow,yhigh):'				1,100
'                (zlow,zhigh):'				1,100

#Other options keep default
```




4. temp_poi.dat for fullmesh_vox.exe (Mesh refinement with point of interest)
```bash
1  #Number of refinement points, 0 = no refinement
#10 refinement parameter entries, 1 POI type ID
0.44D+00  0.22D+01  0.50D+00  0.60000002D-01 0.3500000D-02  0.750000000000D+00  0.12000D+00  0.000000000000D+00  0.000000000000D+00  0.000000000000D+00 'SSHL'
...

'GPOI=General Point of Interest - sphere volume refinement' 
xcenter,ycenter,zcenter,meshResolution,decayExponent,Extent,blank,blank,blank,blank,POIcode

'SSHL=Spherical Shell: sphere shell surface refinement'
xcenter,ycenter,zcenter,sphereRadius,meshResolution,decayExponent,extent,blank,blank,blank,POIcode

'UCYL=Uniform Cylinder:'
xcenter,ycenter,zcenter,axisVectorX,axisVectorY,axisVectorZ,halfLength,radius,meshResolution,blank,POIcode

'CUSR=Check for Unresolved Surfaces in a Region'
xcenter,ycenter,zcenter,meshResolution,decayExponent,extent,blank,blank,blank,blank,POIcode

'GSRR=General Surface Region Refinement'
xcenter,ycenter,zcenter,meshResolution,decayExponent,extent,blank,blank,blank,blank,POIcode


```