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
EDIFF  = 1E-08  !always 1e-08 is enough 
EDIFFG = -0.001 
ALGO   = Normal
LDIAG  = .TRUE.
LREAL  = .FALSE.
ISMEAR = 0       !Gaussian smearing
SIGMA  = 0.02
ICHARG = 2
LPLANE = .TRUE.
NPAR   = 4.      !Pay attention to use this option,it is apply on Supercomputing.
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
