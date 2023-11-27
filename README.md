# SiESTA-Surf

<img src="Images/SiESTA-Surf_LOGO.png" style="">

# ⚠️ NOTE: This repository is still under construction. Please use it with caution. ⚠️

SiESTA-Surf is a Python API to utilizes CUDA for GPU-based computation of molecular solvent excluded surface (SES). 
This repository is developed based on [QuickSES](https://github.com/nezix/QuickSES).
It features a 3D uniform grid for constant-time access to atom neighbors and incorporates a CUDA-implemented Marching Cubes algorithm, as well as a GPU-based method for welding mesh vertices.


## Installation
The nvcc compiler from the NVIDIA CUDA toolkit is required to successfully compile SiESTA. 
It will install the python package siesta-surf, as well as two console programs: 

**QuickSES** to output surface mesh from input PDB. 

**viewobj** to view simple structures (as ball+stick model) or objects surfaces. 

### PyPI (Will be available soon)
```bash
$ pip install siesta-surf
```

### Install from Source distribution (Will be available soon)
```bash
$ wget http://www.placeholder/for/url/to/siesta-surf-0.0.1.tar.gz
$ pip install -v siesta-surf-0.0.1.tar.gz 
```

### Manual installation
```bash
$ git clone https://github.com/miemiemmmm/SiESTA.git
$ cd SiESTA
$ make install 
```


## Quick test
```bash
$ wget https://files.rcsb.org/download/4bso.pdb
$ python3 -c """import siesta as sst; 
xyzr = sst.pdb_to_xyzr('4bso.pdb'); 
print('Test1: Converted PDB to XYZR array: ', xyzr.shape);
sst.pdb_to_file('4bso.pdb', '4bso_pysurf.obj', format='obj', grid_size=0.5);
print('Test2: Surface mesh saved to 4bso_pysurf.obj');
ply_str = sst.pdb_to_string('4bso.pdb'); 
print('Test3: First 400 characters from the result PLY string: '); print(ply_str[:400], '......\n');
sst.xyzr_to_file(xyzr, '4bso_pysurf.ply', format='ply', grid_size=0.3);
print('Test4: Surface mesh computed from the previously computed XYZR array saved to 4bso_pysurf.ply');
""" 
# QuickSES console program
$ QuickSES -i 4bso.pdb -o 4bso_surface.obj -v 0.2
# viewobj console program
$ viewobj 4bso_pysurf.obj 4bso_pysurf.ply   # View the python generated surface mesh
$ viewobj 4bso_surface.obj 4bso.pdb -w 1    # View the QuickSES generated surface mesh
```


## Usage
### Python API
This API focuses on converting any structural file formats (e.g. PDB, sdf, mol2 etc.) to 3D surface triangle mesh.
You could either use the API to convert the structures to vertices and faces as numpy array.
You could also directly convert it to 3D surface triangle mesh. Currently supported object file formats are ply and obj.

```Python
import siesta as sst
sst.pdb_to_xyzr()
sst.pdb_to_file()
sst.pdb_to_string()
sst.pdb_to_surf()
sst.xyzr_to_file()
sst.xyzr_to_string() 
sst.xyzr_to_surf()
```

### QuickSES mini-program
```bash
$> QuickSES -h   # view help
$> QuickSES -i 4bso.pdb -o 4bso_Surface.obj -v 0.2 
```
The default resolution is set to 0.5 Å but can be changed at runtime using -v argument. 
The size of the slice that defines how much memory QuickSES uses can be changed using -s argument.
For the other usage of QuickSES, please refer to its original repository [QuickSES](https://github.com/nezix/QuickSES).

### viewobj mini-program
If you have [Open3D](http://www.open3d.org/) installed in your python environment, you can use **viewobj** to visualize the surface mesh and some commonlu used structure files.



```bash
$ viewobj -h  # view help
$ viewobj 4bso_Surface.obj
```

Current supported format includes:

<table style="width: 600px">
<tr><td colspan="2" style="text-align: center; font-weight: bolder;">Structural</td></tr>
<tr><td>XYZR</td><td>.xyzr</td></tr>
<tr><td>PDB</td><td>.pdb</td></tr>
<tr><td>Sybyl Mol2</td><td>.mol2</td></tr>
<tr><td>MDL SDF</td><td>.sdf</td></tr>
<tr><td>Coord+radius XYZR</td><td>.xyzr</td></tr>
<tr><td colspan="2" style="text-align: center; font-weight: bolder;">Mesh</td></tr>
<tr><td>Polygon File Format (Default output format)</td><td>.ply</td></tr>
<tr><td>Wavefront OBJ format</td><td>.obj</td></tr>
<tr><td>Object File Format</td><td>.off</td></tr>
</table>


## Acknowledgments and links
This project is based on [QuickSES](https://github.com/nezix/QuickSES) by [Xavier Martinez](https://github.com/nezix).
Without these open source projects, this project would not be possible.

Check these links to cite the [QuickSES](https://hal.archives-ouvertes.fr/hal-02370900/document) and the implemented GPU-based SES computation [algorithm](https://www.uni-ulm.de/fileadmin/website_uni_ulm/iui.inst.100/institut/Papers/viscom/2017/hermosilla17ses.pdf). 

[CPDB](https://github.com/vegadj/cpdb) is used for parsing PDB files. 

[Pybind11](https://github.com/pybind/pybind11) is used for wrapping C++ code to Python API.

## License

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
