# DCUHRE

Multidimensional adaptive integration. Chuck a function at it, get an integral back.

Works for 2 to 15 dimensions over hyper-rectangles (fancy boxes).

## What is this?

DCUHRE is TOMS Algorithm 698, originally written by Jarle Berntsen, Terje Espelid (University of Bergen) and Alan Genz (Washington State) back in 1991. It's been quietly doing integrals for 30+ years.

This version has been lightly modernised to compile without warnings on modern Fortran compilers.

## Quick Start

```fortran
program example
   implicit none
   external :: my_function

   integer, parameter :: NDIM = 3, NF = 1, NW = 5000
   double precision :: a(NDIM), b(NDIM), result(NF), error(NF), work(NW)
   integer :: neval, ifail

   a = 0.0d0  ! lower bounds
   b = 1.0d0  ! upper bounds

   call dcuhre(NDIM, NF, a, b, 0, 10000, my_function, &
               0.0d0, 1.0d-6, 0, NW, 0, result, error, neval, ifail, work)

   print *, 'Integral:', result(1), 'Error:', error(1)
end program
```

## Building

### CMake
```bash
mkdir build && cd build
cmake ..
cmake --build .
ctest  # run tests
```

### Fortran Package Manager (fpm)
```bash
fpm build
fpm test
```

### Manual (gfortran)
```bash
gfortran -o test test/dtest1.f src/*.f
./test
```

## Parameters

| Param | What it does |
|-------|--------------|
| NDIM | Number of dimensions (2-15) |
| NUMFUN | Number of functions to integrate simultaneously |
| A, B | Lower and upper bounds for each dimension |
| MAXPTS | Max function evaluations allowed |
| EPSABS, EPSREL | Absolute and relative error tolerances |
| KEY | Integration rule: 0=auto, 1=degree 13 (2D), 2=degree 11 (3D), 3=degree 9, 4=degree 7 |

## References

Berntsen, J., Espelid, T.O. and Genz, A. (1991). "Algorithm 698: DCUHRE: An Adaptive Multidimensional Integration Routine for a Vector of Integrals." ACM Trans. Math. Softw. 17(4): 452-456.

## Licence

ACM Software License. Non-commercial use, modification and redistribution permitted with attribution. See LICENSE file.
