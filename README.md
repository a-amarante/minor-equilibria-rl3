Minor-Equilibria-RL3 Package
===============================
A package that uses the Ridge-Line algorithm to find equilibria around irregular-shaped minor bodies, such as Asteroids and Comets.
---------------------------------

I've implemented a modified version of the original POLYHEDRON code from D. Tsoulis into the Minor-Equilibria-RL3 package, that can compute numerically the effective gravity field (gravity + centrifugal potential), and its first and second derivatives of a mass distribution in a homogenous polyhedron, according to Petrovic (J of G, 1996). In addition, the Minor-Equilibria-RL3 package also can handle equilibrium points under Solar Radiation Pressure (Burns et al., 1979; Mignard, 1984) perturbation (subroutine ``mfo_pr2.for``) and through mascons method (Geissler et al., 1996). It was implemented the algorithm of the ``Ridge-Line`` (Tardivel, Scheeres & Michel, 2014), that can find numerically and automatically each equilibrium point (dynamical substitute in the case of SRP; Xin et al., 2015) around an irregular-shaped minor body to study their stabilities, through z* and h* sets.

Notable contents of this repository
---------------------------

*    ``minor-equilibria-rl3.for:`` The main program file.  It requires ``equilibrium.inc`` and ``swift.inc`` to compile.

 Input Files
 ---------------

*    ``files.in:`` This should contain a list of names for the input and output files used by main program file. List each file name on a separate line.
*    ``equilibrium.in:`` This file contains parameters that control how an integration is carried out. Any lines beginning with the ) character are assumed to be comments, and will be ignored by main program file. All initial parameters of this file are set for the application of the asteroid Bennu with three different sample models: ``75cm`` (most recent, 99846 vertices and 196608 faces; Barnouin et al., 2019), ``Preliminary`` (25350 vertices and 49152 faces; Barnouin et al., 2019) and ``Radar`` (1348 vertices and 2692 faces; Nolan et al., 2013). There is also the possibility to make the ridge-line automatically for different integration sets to analyze its stability.
*    ``./in/vertex.in:`` This file contains the coordinates of the vertices of the polyhedron. The lines representing vertices. Each vertex line has three white-space separated floating point values representing x, y, z coordinates in units of km. The axes are automatically aligned with the shape principal axes of inertia and the origin corresponds to the center of mass assuming a uniform internal density. The index of the first vertex line is 1. 
*    ``./in/face.in:`` This file contains the index of the vertices belonging at each given facet, in such an order that the plane normal is always pointing outside the body. The lines representing facets. Each facet line has three white-space separated integers specifying three vertices to define the corners of that triangular facet, in order such that the right hand rule points in the direction of the exterior of the shape. 
*    ``./in/cube.in:`` (optional) This file is used from gravitational potential of a cubic grid computed previously through the mascons technique.
*    ``./in/message.in:`` This file contains the text of various messages output by main program file, together with an index number and the number of characters in the string (including spaces used for alignment).

 Dump files
 ---------------

*    ``restart.dmp:`` If your computer crashes while main program file is doing an integration, all is not lost. You can continue the integration from the point at which main program file last saved data in dump files, rather than having to redo the whole calculation. This file also contains the location of equilibrium points.
*    ``restart.tmp:`` The same as above (backup file). If for some reason one of the dump files has become corrupted, look to see if you still have a set of files with the extension .tmp produced during the original integration (if you have subsequently used main program file to do another integration in the same directory, you will have lost these unfortunately). These .tmp files are duplicate copies of the dump files. Copy each one so that they form a set of uncorrupted dump files, and then run the compiled version of main program file.

 N.B. It is important that you replace all the dump files with the .tmp files in this way, rather than just the file that is corrupted.

 Output files
 ---------------

*    ``./out/E*.out:`` These files contain the location of equilibrium points with some additional information, such as geopotential and its derivatives.
*    ``./out/E*matrix.out:`` These files contain the state array associated with stability of each equilibrium point.
*    ``./out/R*.out:`` These files contain the location of ridge-line points with some additional information, such as geopotential and its derivatives.
*    ``./out/R*matrix.out:`` These files contain the state array associated with stability of each ridge-line point.

How to compile and run
----------------------

In the current directory, you should have all files needed to compile and execute the program. Don't hesitate to contact me if any problem arises when trying to use it.
Current package is preconfigured for Linux and Mac OS X users with compile and clean tasks. Use your favorite FORTRAN compiler, such as ``gfortran``, ``f77`` or ``ifort``, to create an executable.  For instance, on Linux or Mac, try:

   ``gfortran minor-equilibria-rl3.for -o minor-equilibria-rl3``

There will likely be warnings due to the code being written in FORTRAN77, but it should compile. Copy or link the executable wherever you want (wherever your input files are) to run your code using ``./minor-equilibria-rl3``.

Or use ifort compiler for no warnings:

   ``ifort minor-equilibria-rl3.for -o minor-equilibria-rl3``

Or use the ``Makefile:``

   To compile on Linux or Mac OS X just run ``make`` on the terminal at current directory. You need to have gfortran compiler. If you don't have you need to install it. For other compilers change the corresponding lines in ``Makefile``. This file can be changed if you want to use another compiler as the default one (gfortran). Usually I use the compiler ifort, which leads to faster integration time.

   To clean the directory *.o files run ``make clean``.

Or use the executable files ``minor-equilibria-rl3_gfort`` or ``minor-equilibria-rl3_ifort`` for Linux and Mac OS X compilers ``gfortran`` or ``ifort``, respectively:

   First type ``chmod +x minor-equilibria-rl3_gfort`` and after that run your code using ``./minor-equilibria-rl3_gfort``.

Tricks and Caveats
------------------

Unfortunately, the code needs to be recompiled any time parameters in the ``equilibrium.inc`` and ``swift.inc`` files get changed. However, these parameters do not need to be adjusted often or almost never.

Disclaimers
------------

* The Ridge-Line algorithm have been tested with the ``Bissection`` and ``Newton-Raphson`` methods.
* I've fixed all the errors I've found.  If you find a bug, let me know so we can try to fix it.
* Any feedback is appreciated, especially bugs, suggestions, or possible contributions.
* Are you going to publish? Please acknowledge the use of my code in any publication referencing:

   ``Amarante, A., Winter, O. C., Sfair, R. (2020), Stability and Evolution of Fallen Particles Around the Surface of Asteroid (101955) Bennu. JGR Planets.``

* The main source code of this repository is available on reasonable request.

Recent Publications
-------------------

The code was used in the following recent study:

* Amarante, A., Winter, O. C., Sfair, R. (2020), Stability and Evolution of Fallen Particles Around the Surface of Asteroid (101955) Bennu. JGR Planets.
