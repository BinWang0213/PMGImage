# PMGImage

#Mesh generaion procedure
1. Edit psmesh.in
   'DOMAIN'
   'VOXELFILEINFO'
   'FULLMESH'

2. Generate mesh 

./fullmesh_vox.exe

3. Convert mesh into nodes.dat and elements.dat

./createvmesh_vox.exe

. phasemap.in
3 phase
Voxel value, phase number (meshed phase)
0 1
125 1
255 1
...

./temp_poi.dat #refinement schemes
#Refinement POI


min tet size = 1.0 voxel 

Mesh
Coarse 69233, 73798

