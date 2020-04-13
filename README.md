# simplesolidmechanics
A graphical interface for calculating and comparing large deflection member response of a beam under a pure Ramberg-Osgood material behavior against more traditional material models.
## Ramberg-Osgood
Many solid materials behave linearly for small forces and in a nonlinear fashion for larger forces with no well-defined yield point. Ramberg and Osgood developed an expression to model this behavior from material testing data.

>e = f(s) = a\*s + b\*s\*|s|^(n-1)

where e is the strain, normally denoted with the Greek letter epsilon, and s is the stress, normally denoted with the Greek letter sigma. a, b, and n are material constants with a generally defined as the reciprocal of Young's modulus for the material. To be included in many of the basic equations of solid mechanics we must define stress, s, as a function of strain, e. The Ramberg-Osgood expression is invertible such that we may define

>s = f^-1 (e)

While the inverse of the Ramberg-Osgood expression exists, it is not definable using a finite number of elementary terms. A piecewise simplification of this expression was introduced.

>e = a\*s,   s <= s_y

>e = b\*s\*|s|^(n-1),   s > s_y

where s_y is the presumed offset yield stress value of the material. This piecewise representation is now used througout engineering application. Further information can be found in Section 4.3.9 of the Abaqus Theory Guide. The adoption of the piecewise simplification is largely due to the homogeneity of the two behaviors on either side of the yield value, which lend themselves more easily to tractable results. Very little work exists in the literature on the mechanical application of the unadulterated nonhomogeneous Ramberg-Osgood expression. These two expressions, though similar, are very different and will produce different predictions in the deflection of a member and more importantly in the internal stress response of the member. The piecewise model, sometimes referred to as the general Ramberg-Osgood expression, assumes all elastic behavior is purely linear and all plastic behavior purely power-law nonlinear, ignoring any nonlinear elastic behavior.

## The Program (alpha 0.0.1)
This program allows the user to compare the member response of a cantilever beam with a rectangular cross-section made of a linear, power-law, and both the explicit and piecewise Ramberg-Osgood material models using a newly developed efficient numerical technique for the nonhomogeneous Ramberg-Osgood expression. The user must supply the material parameters, beam geometry, and applied external forces. The program will output a profile of the defleted member, as well as the distribution of curvature, angle, and internal stress response along the length of the member. This program is functional, but still in its infancy so some issues may be present. The program was developed in Python, taking advantage of the many scientific computing libraries available. The grapical user interface was developed using the Tkinter framework.

### Running the Program
Currently the program will only run on Windows machines. To run the program, download the zip file and extract it onto your computer. Within the folder titled "ssmgui" there is a file called "ssmgui.exe" Double-click this file and the program should run. Having python installed on your machine is not necessary.

### Using the Program
The user is expected to provide the parameters for whichever material they may wish to model along with the beam geometry and external forces. The two available external forces are an evenly distributed load along the length of the member and a point load applied at the free end. All material parameters must be positive. The strain-hardening coefficient for a power-law material must be between zero and one, and the exponent for a Ramberg-Osgood material must be greater than one. All beam geometries must be positive and the width and height that make up the cross section of the member must each be less than 20% of the length in order to satisfy the Euler beam assumptions. The forces applied to the beam are in the downward direction and must be negative. At least one of the force entries must contain a nonzero value. Any blank force entries will be presumed zero. If both are zero, the program will not run.

### Major Known Issues
There are some convergence issues involving the power-law member response for sub-yield forces. This is largely due to known issues with the solve_ivp function in the scipy.integrate package. Only negative forces in the downward direction are allowed at the moment. Currently the program performs no checks on the viability of the input parameters in so far as the feasibility of solutions, so it will always attempt a solution and in some cases may result in erroneous results or the program may hang. These issues will be addressed in future updates.

## Example Values
Below are some values you may wish to try to test the program. We will consider parameters taken from 304 Stainless steel and a long very slender member.

Linear material:
>E = 193,054

Power-law material:
>K = 645.117
>m = 0.15823

Ramberg-Osgood material:
>a = 5.1799e-6
>b = 1.75018e-18
>n = 6.32

Beam geometry:
>L = 1.0
>B = 0.00635
>H = 0.00635

A distributed load of zero and a point load of about -8 Newtons or less will result in sub-yield elastic behavior along the entire length of the member. A point load up to about -18 Newtons has a maximum stress value at the fixed end in the neighborhood of yield. This is the area of the stress-strain curve that is most difficult to model using Ramberg-Osgood.
