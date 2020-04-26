# simplesolidmechanics
A Windows graphical interface for calculating and comparing large deflection member response of a beam under a pure Ramberg-Osgood material behavior against more traditional material models.
## Ramberg-Osgood
Many solid materials behave almost linearly for small forces and in a nonlinear fashion for larger forces with no well-defined yield point. Ramberg and Osgood developed an expression to model this behavior from material testing data.

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

### Advanced Options
There are eight advanced options de-activated by default. Clicking the "Advanced Options" box will activate six option panes, five of which are functional. Clicking "Default Values" will restore the default values for every parameter.

>alpha

The alpha parameter is the angle at which the point load P is applied on the tip of the member. Zero is an axial force directed into the member and pi/2 is applied perpendicularly to the tip of the member. Because buckling has not yet been implemented, an alpha equal to zero will result in no displacement from the point load or erroneous results. The default value is an approximation of pi/2 = 1.570796

>elements

The elements parameter determines the number of elements the length of the member is apportioned into. Generally, the greater the number of elements the larger the computational load and the better the solution. However, the solve_ivp function in the scipy.integrate package which is used to solve the initial value problem is very robust even with a relatively small number of elements. The default value is 100.

>curve tol

The curve tol parameter sets the tolerance of a valid solution for the curvature at the fixed end of the member. A bisection approach with a shooting method is used to solve the system. The difference between the two guesses on either side of the root from the bisection approach must meet the set tolerance. Setting the tolerance very low can result in a very long time to find a solution, especially for a power-law member. In general, the necessary tolerances for a power-law member can be very tight on the free end for powers less than 0.5, but by checking the difference on both ends the total number of iterations to an acceptable solution can be reduced. Values smaller than 1e-6 may result in unnecessarily long run times. The default value is 1e-3.

>zero tol

The zero tol parameter sets the tolerance of a valid solution for the curvature at the free end of the member. A bisection approach with a shooting method is used to solve the system. The difference between the two free end values which are a result of the two guesses for curvature on the fixed end on either side of the root from the bisection approach must meet the set tolerance. Setting the tolerance very low can result in a very long time to find a solution, especially for a power-law member. In general, the necessary tolerances for a power-law member can be very tight on the free end for powers less than 0.5. To reach an acceptable difference in curvature values on the fixed end for a power-law exponent of around 0.15, the zero tol parameter must be set to around 1e-18, which takes a very long time to reach a solution. By checking the differences on both ends the total number of iterations to an acceptable solution can be significantly reduced, such that for an exponent of 0.15 the default curve tol and zero tol are sufficient for achieving a solution. Values smaller than 1e-6 may result in unnecessarily long run times. The default value is 1e-5.

>guess 1 AND guess 2

These two functions are not implemented, cannot currently be activated. In a future update the user will be able to supply their own initial guesses to begin the bisection solution approach instead of relying upon the in-built automatic guessing function.

>max iter

This function is not currently implemented, but will be in the next update.

>Solver

This parameter allows the user to select which available solver from the solve_ivp function in the scip.integrate package to use when solving the system. There are six solvers available: RK45, RK23, DOP853, Radau, BDF, and LSODA. The default value is the 5th order explicit Runge-Kutta method, RK45. For more about each of the available solvers, consult the scipy documentation for the solve_ivp function.

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

A distributed load w of zero and a point load P of about -8 Newtons or less will result in sub-yield elastic behavior along the entire length of the member. A point load P up to about -18 Newtons has a maximum stress value at the fixed end in the neighborhood of yield. This is the area of the stress-strain curve that is most difficult to model using Ramberg-Osgood.
