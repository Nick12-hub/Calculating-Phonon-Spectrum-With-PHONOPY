# A Simple Tutorials for calculating Phonon 
Calculating Phonon may be simple compared to other calculating process.

Now I show a example for calculting Phonon with __VASP.5.4.4+Phonopy.2.3.2__.

## Installation and Configuration
It is easy and you can refer this article.click[:link: HERE](https://phonopy.github.io/phonopy/Fleur.html).

## Structural optimisation
It is worth noting that the calculation of the phonon spectrum needs to be fully optimised with high precision for the original cell structure, otherwise __false frequencies__ can easily occur.

I show a example which contain __INCAR__ file

### INCAR
```
SYSTEM = 2D_InSe
ISTART = 0
NWRITE = 2   
PREC   = Accurate
ENCUT  = 500
GGA    = PE
NSW    = 200
ISIF   = 3
ISYM   = 2
NBLOCK = 1   
KBLOCK = 1
IBRION = 2
NELM   = 80     
EDIFF  = 1E-08   
EDIFFG = -0.001 
ALGO   = Normal
LDIAG  = .TRUE.
LREAL  = .FALSE.
ISMEAR = 0       
SIGMA  = 0.02
ICHARG = 2
LPLANE = .TRUE.
NPAR   = 4.      
LSCALU = .FALSE.
NSIM   = 4
LWAVE  = .FALSE.
LCHARG = .FALSE.
ICORELEVEL =  1
```
### OPTCELL
For 2d materials,we often add this.Like follwing:
```
100
110
000
```
## Establishing a supercell with Phonopy code
### Rename
Rename Output file（__CONTCAR__） of the previous step to POSCAR
```
cp CONTCAR POSCAR
```
### Establishing a supercell
```
phonopy -d --dim="a a 1" 
```
### Rename
```
cp POSCAR POSCAR-unitcell
cp SPOSCAR POSCAR
```
## Calculating Hessian matrix for phonon frequencies
### INCAR 
```
ISTART = 0
NWRITE = 2
IBRION = 8    
IALGO  = 38
NELM   = 200
EDIFF  = 1E-07
EDIFFG = -0.001
ISMEAR = 0   
SIGMA  = 0.02
ENCUT  = 500
PREC   = Accurate
LREAL  = .FALSE.
LWAVE  = .FALSE.
LCHARG = .FALSE.
ADDGRID = .TRUE.
```
### KPOINTS
If your machines'RAM is enough,increase KPOINTS may be a great choice!

### Running tips
It is a wise Initiative to estimate the consume RAM and decrease Number of cores properly.

## Data processing
Phonopy will generate __FORCE_CONSTRAINS__ which is based on __vasprun.xml__.
### Generate FORCE_CONSTRAINS
```
phonopy --fc vasprun.xml
```
### Self-editing file:__band.conf__
You should generate by yourself and modify it.
```
touch band.conf
vi band.conf

ATOM_NAME = 
DIM = a a 1 
BAND = 0.5 0.0 0.0  0.0 0.0 0.0  0.333333 0.333333 0.0  0.5 0.0 0.0 
FORCE_CONSTANTS = READ

ENTER->"ESC" and :wq!
```
Pay attention to "DIM = a a 1"!It depends on "phonopy -d --dim="a a 1" 
### Generate band.yaml
```
phonopy --dim="a a 1" -c POSCAR-unitcell band.conf
```
And the last process is:
```
phonopy-bandplot  --gnuplot> PBAND.dat
```
Then you will get __PBAND.dat__ and you can draw figures by Origin Gnuplot Matlab etc.(The Phonopy‘s default is __51 Points__ between two __high symmetry points__)

If the __false frequencies__ still exists,change the "**phonopy -d --dim="a a 1"**".Whether to take the **larger or the smaller** often depends on experience！

The simple tutorial is ending and thanks for __Yong He__ !

You can refer to his tutorials!Click[:link: HERE](https://yh-phys.github.io).
