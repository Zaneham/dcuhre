## DCUHRE Changelog

A curated, chronologically ordered list of notable changes to DCUHRE.

---
**2025-12-14**  Zane Hambly (Modernisation)

### :green_circle: ADD:
- fpm.toml for Fortran Package Manager support
- LICENSE file with ACM Software License terms
- MODERNISATION_PLAN.md documenting approach
- Golden reference outputs in output/ directory
- Primary source documentation in c:/dev/Hopper/Documents/FORTRAN/DCUHRE/

### :orange_circle: DIFF:
- Fixed deprecated shared DO termination labels (Fortran 2018 deleted feature)
  - d113re.f: lines 115-118, 122-125
  - d132re.f: lines 116-119, 123-126
  - dadhre.f: lines 339-343
- Reference: ISO/IEC 1539-1:2018 (Fortran 2018 Standard), Deleted Features

### :memo: NOTES:
- All changes verified against golden reference output
- Both dtest1 and dtest2 produce identical results to original
- No numerical changes - purely syntactic modernisation

### :book: REFERENCES:
1. Berntsen, J., Espelid, T.O. and Genz, A. (1991). "Algorithm 698: DCUHRE:
   An Adaptive Multidimensional Integration Routine for a Vector of Integrals."
   ACM Trans. Math. Softw. 17(4): 452-456. DOI: 10.1145/210232.210234

2. Berntsen, J., Espelid, T.O. and Genz, A. (1991). "An Adaptive Algorithm
   for the Approximate Calculation of Multiple Integrals."
   ACM Trans. Math. Softw. 17(4): 437-451. DOI: 10.1145/210232.210233

3. ISO/IEC 1539-1:2018. "Information technology - Programming languages -
   Fortran - Part 1: Base language." (Fortran 2018 Standard)

---
**1991-12-01**  Berntsen, Espelid, Genz (Original)

### :green_circle: ADD:
- Initial publication as TOMS Algorithm 698
- Adaptive multidimensional integration over hyper-rectangles
- Support for 2-15 dimensions
- Integration rules: degree 13 (2D), degree 11 (3D), degree 9 and 7 (all)
- Parallel computing support via MDIV parameter

### :book: ORIGINAL AUTHORS:
- Jarle Berntsen, University of Bergen, Norway
- Terje O. Espelid, University of Bergen, Norway
- Alan Genz, Washington State University, USA

### :memo: FUNDING:
- J. Berntsen: Norwegian Research Council for Humanities and Sciences; STATOIL
- T.O. Espelid: Norwegian Research Council for Humanities and Sciences
- A. Genz: Norwegian Marshall Fund
