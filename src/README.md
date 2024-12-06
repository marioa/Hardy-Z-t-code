# Source codes

This repository contain three source codes for calculating the Hardy function, Z(t), for a given value of t. Typically t > 10<sup>15</sup>, although values of t greater than 100 million should be fine. All the codes are based on algorithms devised from a single complex asymptotic expansion for Z(t) and obviously this expansion gets more accurate as t gets large. The relative error in the expansion is O(t<sup>-1/4</sup>), so one should ensure that t is large enough to make this error small.

The source codes are as follows:
1) **zeta14cubic.f90** is a Fortran code for computing a single value of Z(t), for user prescribed values of t and a relative error scale denoted by eps or sometimes et (0<eps<<1). The above mentioned asymptotic expansion can be formulated in terms of cubic Gauss sums, each of  which can be evaluated O((log(t))<sup>2</sup>) operations. This leads to an overall operational count of O((t<sup>1/4</sup>)*(log(t))<sup>3</sup>) for the entire calculation of Z(t).
2) **zeta14cubicmult.f90** is a Fortran code for computing multiple values of Z(t) surrounding a user prescribed central value of t, plus a standard et/eps value. The number of multiple evaluations is restricted to 15, at values of t+i*0.01 for i=-(numbercalc-1)...(numbercalc-1). Numbercalc<=8 is a third user prescribed parameter for this code. Naively one would expect 15 evaluations of Z(t) to require 15 times the number of operations for one evaluation. But in fact 15 simultaneous evaluations require little more than 10% extra operations, compared to a single value evaluation using code option 1. Fixing numbercalc<=8 is quite conservative and probably one could increase this to some degree with no detrimental effects on the accuracy of the calculations.
3) **zeta15quartic.f90** is a Fortran code for computing a single value of Z(t), for user prescribed values of t and et/eps. The asymptotic formula is very flexible and allows one to express Z(t) in terms of quadratic, cubic, quartic, quintic,...,nth-order,.. Gauss sums. The higher the order, the fewer the sums required in the expansion. In theory this means that one should be able to compute Z(t) in successively fewer O(t<sup>1/(n+1)</sup>) operations, for n=2,3,4,... Unfortunately certain "technical difficulties" mean this goal is currently unattainable. Nevertheless, some theoretical progress has been made for the quartic sum case, and this code is a practical algorithmic product based on that work. For t values <10<sup>29</sup> the code runs significantly faster (20-30%) than the corresponding calculation using code option 1. However, it is nowhere near an O(t<sup>1/5</sup>) operational code. For large values of t>10<sup>30</sup>, the efficiency savings decline and one is better off using code option 1.

All three codes use C subroutines selected from the open source [PARI computational library](https://pari.math.u-bordeaux.fr/), for some parts of the overall calculation which require very high precision and will also require a Fortran compiler that supports quad maths (currently gfortran) and an MPI library.