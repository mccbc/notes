-mc -sph_trans
spherical transport test

tests/montecarlo
convergence.py will run it from scratch

Does the test at a number of steps, mainly to test the generalmover. Testing step size. But it also tests the non-general relativistic (old spherical transport test). Can run that to get an athinput

-nstep = 0, don't want to use the general mover

Moments test

Want to have the moments updating before next meeting


Pleiades

Fiure out where your scratch directory is
Where is lou large format storage? How do you transfer files back and forth

Nobackup2 - PFE node
Documentation - what is your scratch directory?


mc_isothermal (NOT mc_iso_sph)
Compile with sphpol coords, will automatically initialize as a sphere
Third test - isothermal spherical problem. Spectra and moments inside a sphere of constant temp with a logarithmic drop in density as a func of radius. No analytic solution for the output. Compare old method to new method. After Thurs, 

For resonance line test - write it youreslf. Doing the whole problem generator from scratch would be a good 


2022 01 27
---
TODO:

 - [X] Rewrite Hello World in parallel using MPI instead of OpenMP
 - [X] Run hello world C++ script using batch job on Pleiades
 - [x] Prepare GR thin disk tutorial for Asia
 - [x] Prepare density radial profile tutorial for Trent
 - [ ] Implement corrective step in Athena spherical polar mover
     - Choose length dl, take trial step.
     - If zone changes by 1 in any direction, approve step. Otherwise, undo
       step and try again.
     - If step approved, use average opacity in the two zones to calculate
       amount deducted from tauremaining.
     - If tauremaining is negative after step, take a corrective step backwards.
           - Step should be (tau_final - tau_remaining)/opacity2
 - [ ] Consider if stepping back by (delta tau)/(0.5(opacity1 + opacity2)) can 
     ever put you back in the zone you came from

2022 01 24
---

TODO: Pleiades

 - [X] How do you load modules?
    - Include the following line in your startup configuration file:
    - `source /usr/local/lib/global.profile`
    - Then `module load comp-intel mpi-hpe`
    - List of modules: https://www.nas.nasa.gov/hecc/support/kb/software-on-nas-systems_116.html
    - Can also use `module avail` to see available modules

 - [X] What compiler do you use?
    - gcc and g++ are available and can be loaded in as modules (list them using `module avail`)

 - [X] Run hello world in parallel
    - Compile using `mpicc` after loading mpi modules
    - Submit interactive job on Pleiades using `qsub -I`
    - `qsub -I -l select=1:ncpus=4:model=san:mpiprocs=4` to request an interactive session
    - `module load mpi-hpe/mpt` to load mpiexec commands
    - `mpiexec -np 4 ./a.out`
 - [X] Run hello world in parallel with a batch file

Use Lustre /nobackup for large temporary files to be moved off of Pleiades eventually.
Run `ls -l /nobackup/your_username` to find which Lustre file system is assigned to you.

Athena++ procedure
---

Do a trial step. Check the trial step, see if it's good. If not, revert without any additional calculations necessary. Should be very fast and general.

May want to be able to undo the step if needed, even if only one zone is crossed. Main thing to do in case of average opacity is step forward, if you've gone too far, take a step back along the path you've traveled. Make sure the step backwards doesn't go BACK across the zone boundary. Should leave you in the same zone you've updated to.

May be possible to express the photon direction vector in spherical, get r thet phi component, get a first oder estimate of how far you are from the zone boundary using dr or dtheta, treating everything as locally cartesian. 

What is the exact answer? Need to start taking steps away from your initial position. As you take these steps, calculate the optical depth you've traveled. To get exact answer, need to take extremely small steps. Have an array of optical depths vs position. Then you have exactly the optical depth you've moved through and the position you end up in. 
We want first-order accurate absorption coefficients and first order accurate positions in the cell. 

(^Began markdown format^)

2022 01 21
---

C++ questions:

If we change zones and update chi, does the step size also update? It seems like it wouldn't, in which case the distance traveled in the next step would be chi1 * (tau_remaining / chi0) != tau_remaining. 

The while loop goes until tauremaining is depleted, though dl, dmin, and step are not updated at every zone crossing.



Updates:
 - sphericalpolaraltmover
      Rewriting the move method, have logic blocked out now and checking details
      Currently writing step routine, need help understanding syntax to do so
 - pleiades
      Account has been approved and set up, just one more training video to go through
      Should be able to access computer now
 - Owen paper
      Started reading and highlighting, can discuss in arXiv discussion next Monday?
 - CVGS
      Have a temperature profile comparison between the two models & data
      What's going on further in? Turbulence? Thin disk breakdown at small radii?


Log into the PFE (pleiades front end)


TODO:
 [ ] Call NASA and set up pleiades first-time login
 [X] Test code for Asia and Trent - figure out NaN error for rg=6
 [X] Athena spherical code - review and make changes discussed in meeting today


2022 01 13
---

Resonance line - an allowed dipole transition.

Next step for analytic sphere of constant density -> exponentially falling density in atmosphere.
Line center is always super optically thick, but wings are optically thin. 
At some wavelength, there will be a point in the atmosphere where the optical depth is 1. 

Sample an optical depth to move through. This yields a path length. Find characteristic length to surrounding zone in r, theta, phi, and take the minimum. This is delta r. If path length is less than delta r, then take the step and don't cross zones. If path length is longer than delta r, take a step of size delta r and update zones. Also update the optical depth you've moved through. Now you know how far you still have to go before this step is complete, and you can still update the moments in every zone you cross.

Accumulate moments in spherical polar. Use cell center basis vectors to do the calculation, but this comes after.

2022 01 12
---

Athena Spherical Transport Modifications

GeneralMover::Move
------------------

Description: 
Take little steps through the grid. 
Make sure tauremaining is > 0.
Then a while loop that continues evolving until it goes the requisite number of mean free paths.

VerletStep, PropogatePolarization both integrate the path as a function of an affine parameter for the geodesics. 
For my implementation, I'll replace this with a simple Cartesian step and remove the polarization.

 [X] WRITE UpdateZone. (Can use existing UpdateZone?) After taking a step in the mover, call update zones to check if you've moved into a new zone.

 [X] x = xinit + dl * kx
     step is going be some direction, new x is initial x plus component of direction of photon kx times some dl, step size.
     
 [X] Make the step size roughly the size of the mean free path. 
     We know what the opacity coefficients are, scattering+absorption, so then compute 1/(scattering+absorption).
     This is the mean free path, or target dl. 
     Then make the step equal to tau times dl. That's the target step.

 [X] Step may be much bigger than size of zone, so compare step length to width of the zone, delta r (characteristic zone size). 
     Want to take a step that's the minimum of those two. 
     If the MFP is small compared to the size of the zone, then just take a small step. 
     But, if MFP is bigger than the zone, move out of the zone and check boundaries. 
     After you take that step, check to see if you moved out of the zone.

 [ ] Distribute half of the energy into one zone and half into another. 

 [ ] Refinement: make sure you've only crossed one zone at a time by checking grid indices. 
     Should only move one at a time. 
     Even when you make a step of size delta r, make sure you haven't crossed into two different zones. 

     What if you go through a corner? One integer in each direction allowed? 

 [ ] WRITE GetZone that checks to see if an index has changed by 2, indicating the photon has moved two zones. If so, step back.
     Do the motion in cartesian and then calculate the spherical polar coordinates after.
     Keep kr kth kphi as the fundamental directions, and then convert to kx ky kz in the steps, then convert back?

 [ ] Want to tabulate moments as kr, kth, kphi moments to keep it general. 
     Must do the conversion every step. 
     When we tabulate the zones we want the r theta and phi components of the flux. 
     Especially useful for 1d and 2d calculations, 3d could get away with cartesian.

SphericalPolarMover::Move
-------------------------

Description:
For loop that integrates over the number of photons.
Don't need the photon trajectory list - output that we don't need. Can delete.
Tau remaining is set to GetOpticalDepth, which tells you how far you still have to go.
Still need to define the montecarloblock, MCrandom, MCCord.
kr, kth, kph are the photon vector in this setup. Have to convert to kx, ky, kz.

Main loop: while tauremaining and still evolving and not gone too many iterations...

 [ ] Remove all the r face, theta face, phi face stuff.

 [ ] Main thing to do is update moments. 
     UpdateMoments assumes you've calculated dl (the distance traveled in that zone)
     It adds that photon packet to the cell it's in.
     When I cross a zone, I'm going to divide the contribution into two different zones. 
       Divide up half of the contribution into one zone, half into another. 
       Trickiest part of the whole effort is how to distribute the energy between these two zones.
    Use update moments to distribute the energy. 
    Written in such a way that the function UpdateMoments always updates the zone where the photon currently is.
       So how do we split energy across two zones?
       May have to write it in such a way that we call updatemoments on the first zone,
       then call it a second time in the zone the photon has moved into. 
       This will partition the energy and then we can move into the next step.
    Start in one zone, taking steps until you end up in another zone. 


2022 01 10
---

Q: If you have 10% transit depth at some amount away form line center, what is the line center optical depth?

cross section
length scale - jupiter radii

jupiters radius is 1/10 of the suns
Volume of jupiter is 1/1000, mass is 1/1000, density is about the same
Earth is about 1 factor of 10 below jupiter in radius

Almost everything has density 1g/cm^3, for reference. Earth is ~4-5x higher density


2022 01 06
---

Start with spherical polar implementation of lya transfer

Write problem generator, initialize photons function
Shane's version - creates a big box, puts a sphere in the box
Next step is to use it in spherical polar

Then, what's the next problem?
  Is there an interesting first calculation to do?

  Analytic solution for a point source at the center of a sphere.
    Test the code against it. Dijkstra solution.

  Another problem: attempt to compute a transmission spectrum.
    Take same constant density sphere of hydrogen, let the input radiation start
    at a random impact parameter, going in one direction
    x, y, z
    coming from z
    Only extinction. Set up analytic problem on the side where the intensity through
    any sight line is e^-\tau. Then could compute analytically what the transmission spectrum would be.
    Star is emitting light exactly toward the observer.
    What is formula for exact cross section, use density and temperature in sphere to calculate optical depth, make a transmission spectrum (fractional change in flux at every wavelength)
    Then set up a problem generator that does the same thing.
    
  Spherical polar implementation moves things to the boundary in r theta and phi.
  Instead do it in x y z, just check if it's crossed a boundary.
  Set up to do spherical polar grid using current implementation.
  Rewrite photon mover to move in x, y, z, check where it ends up.

  Now have a mapping between x, y, z, and r, theta, phi positions naturally

  Next step of actually trying a simulation:
    Solve fluid dynamics equations, solve radiation transfer equation
    5 variables don't know anything about hydrogen.
    Need to start solving the rate equations to tell us how much H is ionized and how much is neutral.
    First step of fluid dynamics isn't lya transfer, it's ionizing radiation transfer from the star.
    There we just have absorption and emission, no scattering. Familiaraize yourself with what the photoiniozation cross section is for hydrogen. What' tshe thermally averaged recombination rate for hydrogen from ISM book.
    Real stellar spectrum with distribution of energies, gonna take spectrum and do ray tracing, but now there's no scattering. They're abspried somewhere. Apsorption is going to lead to an ionization rate. If there's a cell that has no absrption of ionization flux, it has no rate. We need to be adding aditional variables for number densities of neutral and ionized hydrogen.

    Constantly trying to add one piece each week. Recombination rate, real stellar spectrum, only corona puts out ionizing flux. 

    If you wanna do rad transfer part just try doing it with pure absorption in the code. How rae we going to put in emission of ionizing photons. 

    It's possible that the gas itself can produce ionizing photons. The first photon you emit can be an ionizing photon

    Read ISM book -- section about recombination to differnet levels. 
    Recombination can still yield a photon that can ionize.
    How do we treat this? On-the-spot approximation (assumes mean free path is small)

    Simple H2
        Osterbrock and Ferlin ISM, ionization and recombination balance
        "The Astrophysics of Active Galactic Nuclei and ..."

    First thing to do 
    Understand analytically cross section, ionization fraction, recombination rate, etc
    Then, RT calculation needs an absorption opacity. Derive this from photoionization cross section

    George Tramell paper -discusses transmission spectrum
    Intensity that makes it through is e^-tau of the incident intensity
        Pure absorption with no emission

    Start with the uniform sphere. How much does re-emission from the absorbed radiation affect the answer? use simulation to determine this.
      Have to track position and exit vector of photons when they exit. Then you can make a light curve - how much light exiting at every photon frequency at every angle.
    
    For later --- Shane has a good way of calculating if the planet is not infinitely distant from the star. The photons are not coming in exactly parallel. 

-> Check online conference website for more opporunities to give talks
-> Check astro seminars nearby, driveable



AAS Talk preparation
---

a 10 minute talk is an advertisement for people that want to talk to you after 

why you're doing it
what your motivation is
broad overview of what the new results are
  There's this 30 year old problem, technical, boundary conditions, etc
      show a result to show how things change. Don't have to explain all the details (maybe mention sep constants arent constant)
      show the monte carlo. That's it!

      Have a few more backup slides for addressing easily anticipatable questions.

5 slides other than title and conclusions
A slide is a parcel of your talk that conveys a specific piece of information
Come up with 2 or 3 context slides, and then 2 or 3 results slides, then conclusions

 [X] Clear up order of variable definitions in first paragraph of steady state. Give value of oscillator strength and cite Rybicki and Lightman
 [X] Go through and remove all mentions of a. It should be addressed once in the definition of variables
 [X] REMOVE H_{0+bc} and replace with H_0 + H_bc for ALL FIGURES
 [X] Resolve issues with last paragraph of appendix following Shane and Phil's discussion


2021 11 11
---

[ ] Add to .bashrc
    export LD_LIBRARY_PATH=/usr/local/hdf5/lib:$LD_LIBRARY_PATH
    export LD_LIBRARY_PATH=/usr/lib:$LD_LIBRARY_PATH
    export PATH=/usr/local/visit/bin:$PATH 
    to get hdf5 working on Pella

 [ ] makefile_options['LIBRARY_FLAGS'] += ' -lhdf5 -L/usr/local/hdf5/lib -lz -ldl' 
should be in configure.py in some place - check that it is for HDF5 usage on pella

Paper notes:
% Strong irradiation -- higher upper atmosphere temperature. Top of atmosphere is hot, have to go deeper in before you reach 2500 K where it turns into molecules. You then have a very thick layer of atomic H.
% The transition from the atomic to the molecular layer occurs where the temperature is sufficiently low for molecules to form and may take place ~1 microbar. Detailed calculations have found a pressure level where you switch from atomic to molecular. Then you can get a column - column and cross sections gives you optical depth. pi e^2 / me c osc strength, lc cross section is 1/pi delta
% 10^-14 cm^2, grav is 10^3 cm...
% 1 microbar is 1 in CGS
% Plug in to get high optical depths
% Because the upper atmosphere is so hot and the temp at ever layer is set by a balance of cooling and heating - photoelectric heating and ___ cooling, dominant cooling is electron impact collisional excitation and then de-excitation by emission of a photon
% Kinetic energy of a particle hits something, puts the something in an excited state, then it de-excites and emits a photon
% >>>>>> electron impact collisional excitation
% Heavy elements are either constantly being deposited from above, or beign mixed up from below.
% Heavy elements are much more efficient at cooling via line emission

 [X] Cite Advanced Mathematical Methods for Scientists and Engineers: Asymptotic Methods and Perturbation Theory for matched asymptotic expansion

Ongoing Tasks
---
[ ] Get set up with Pleiades
        Start reading about the different computers -- how many nodes are available, what would be the largest job you could run, read about the different queues.
        Be up and running and ready to do stuff on the cluster right away in January. Read about nodes, differences between them, have a PBS script that runs in advance. Sort of like SLURM, but for Pleiades.
        How do you SSH in and where willl your data be stored? Frontend login, pleiades frontend, where does the data go when you do a big run, how do you log in, etc.
        Learn what the latest CPUs are. More and more clusters are GPUs.
        Athena++ doesn't run on GPUs though. Know the various intel architectures.
        Know how many cores per node you can use.
[ ] Write spherical polar photon mover - implement actual motion!
[X] Look through list of conferences for extrasolar planets
[X] AAS - Register for meeting
    https://aas.org/posts/news/2021/10/confirmed-239th-aas-meeting-will-be-person
 [ ] Use sph_trans test
 [ ] Hard part: sph test does not handle this: If you're skipping over zones and oging back and putting heating in, need another test to make sure the moments are being calculated correctly. Energy densities, fluxes, etc, still need to be accurate. Want to make sure we get the same results when we use all the different transport methods.
 [ ]  Get transfer through the mesh workng this week. Tricky part is how we're going to deposit the energy.

[X] Add to appendix - discussion of asymmetric piece
[X] Write abstract to submit to RT conference in Denmark
[X] Submit abstract for RT conference in Denmark
[X] Paper edits - Sec 3 Shane
[X] Paper edits - Sec 3 Phil
[X] AAS - submit abstract
[X] Check to see what x frequency the optical depth is = 1 (optically thin) for all our plots
      tau(x) = phi(x)/phi(0) tau_0
      x ~ (atau)^(1/2)
[ ] Test, monte carlo, mc_isothermal test problem, run isothermal atmosphere to test scattering, absorption, moments, boosts, cooling, etc. Make sure you didn't screw up the code.


2021 11 04
---
  Follwoing Harrington, we will be using a change of veariable from x to sigma, define those variables, then state what the COV implies on the line wing (wing approximation).
  Harrington showed that if you make a change ov variables, you turn two differential terms (single + double) into a single second derivative.

  Rewrite eq A23 as an integral. Sigma is equal to this integral. Replace it with the x variable. 
  Want content of A23 and A17, but in A17 add another equality to what it ends up beign in sigma units.

  sigma ~ x^3 with some coefficient - cite equation in appendix.

  Hbc / H0 ~ s/(phi * Delta)

  Dimensional analysis gives you sigma ~ tau0
  Sigma & X relation gives you equation 18

  If Jbc >> Hbc, then Jbc ~ H0, so we just need to compare Hbc to Jbc. Then it's a matter of the derivative.

  s/phi/Delta

  Phi goes like a / delta x^2
  Need to plug in a value of x ---- use x peak
  Now put the relationship between sigma and x at beginning of paper, just cite where we have plugged in x ~ atau01/3


Talk Abstract
---
  Title: A Novel Solution to an Old Problem in Lyman Alpha Radiation Transfer

  Abstract: In exoplanet atmospheres, Lyman Alpha radiation can ionize atoms, dissociate molecules, and exert pressure forces that drive an outflow. Monte Carlo simulations of such phenomena have a large computational cost due to the extreme optical depths at Lya line center. We present a novel, semi-analytic solution to the radiative transfer equation for resonant scattering that is shown to correct discrepancies in previous models as compared with Monte Carlo. We also present a time-dependent solution that can be used to accelerate Monte Carlo simulations by sampling the photon escape time distribution directly.


   [X] ADD AT END OF PAGE 4: An additional reason why the assumptions break down is the optical depth at the peak goes as a tau 1/3, so those photons are becoming optically thin. Another reason why everything breaks down at atau~1 is that the photons at the peak of the sspectrum are optically thin at that point.

   [X] Change gamma 00 to gamma 11


Athena edits
---

Want to replace SphericalPolarMover
Declar a new spheriecal polar mover to start from scratch
SphericalMoverTemp

montecarloblock.cpp
  Constructor: generates local grid and tells the code what it's doing
  Sets all these flags, boosts, moments, acceleration, etc
  Sets all the movers
    If coord system is spherical polar, sets up coordinate systems
    Then sets the mover
    GetZonePosition finds places uniform in volume
    General Mover flag specified in radiation block
      If not, uses the standard spherical polar mover
      To add new sphpol mover, add another flag
        If general mover flag... else if connor_flag ... pmover = new SphericalPolarAltMover(this)
      Add flag under general_mover_flag
      connor_flag = pin->GetOrAddBoolean("montecarlo", "connor", false)

Photon movers
How does MC block, as a class, work? How is it structured?
Understanding montecarloblock is the hardest part of the code


Athena run info
---

mpirun -np 11 ~/athena-swdavis/bin/athena -i athinpt.sphtran

run_isothermal_tests.py
Must compile with mc_test

convergence.py in spherical_transfer
run with arguments 14324231 1000 0.1 2
prob=mc_sph_tran -mc --coord=spherical_polar

Could set x1rat to set the cells as logarithmic instead of linear

python configure.py --prob=mc_sph_tran -mc --coord=spherical_polar -mpi
make clean
module load mpi
make all

/src/ ... /montecarlo.cpp
#ifdef MPI_PARALLEL
  Executed at compile time, don't cost anything in performance
  
/src/defs.hpp
  Defines macros, tells you what coords and problem generator you're using
  Set by the configuration.
  If you want to add configuration files, use defs.hpp.in

  "Preprocessor directive" - 


2021 10 05
---

Initial paper edits done - will look over this week
Circular integral in complex space to show eigenmodes - SHOW PLOT

Circular integrals
------------------
Putting i omegas into the equation - need to have both the real and imaginary part of J and DJ
Need two more integrations and to adjust the matching condition
 [ ] Integrate in a circle over 1/z, see how well we do!

Paper edits
-----------
 [X] Eq. 18: If we use dimensional analysis, this is what we find. Not doing a long derivation, but not simply stating it. Put a few statements in there stating which equations you're using and where Eq 18 comes from.

 [X] Discussion of Fig. 4: Becomes narrower in shanes edits - becomes more strongly peaked

 [X] Abstract: Corrective term -> correction

 [X] Intro, phil's edits, long sentence rephrasing things. Add this: ...(recombinations) in the planet's atmosphere.

 [X] Eq. 2: define L. L is a power, energy input, or a luminosity parameter.

 [X] Right after Eq. 2: Add a sentence that says we will use x_s, nu_s, and sigma_s interchangeably.

 [X] After equation 9: The divergent solution and the dijkstra solution agree when the arguments are small (i.e., first term in the expansion)

 [X] Discussion of Fig. 2: Peaks move INWARD at low tau0, doppler core does not move. Change phrasing to be clearer.

 [X] Stuff before Eq. 31: We find that there are eigenmodes that correspond to damped solutions. This is found numericaly. The long timescale behavior of those pieces of the soltuoin can be found from the residue theorem. The answer is _____.

 [X] Eq. 32 & 33 can be cut & substituted with words instead. We know there is more to this problem than jus tthe residues of the poles. (Push off to the wkb appendix)

 [X] Get rid of eq 34 as well

Same spherical problem, turn on the piece of the code that tabulates mean intesntiy and flux and forces in every zone. That's something we can compare to the analytic formulae we have.

It currently runs in cartesian with a sphere. To compare formulae, we would need to rewrite it in spherical. We'd have to put the source epsilon away from the sphere.

Convert cartesian components to radial at output?

Starting point and ending point, take the full step (cartesian) which is exact, after that you check to see if you've moved through subboundaries
The way you deposit energy in one zone or another is not exact though - just put half in one and half in another.
Taking a jump and continually bisecting until you just have one zone boundary in between the test points.

Energy density / mean intensity goes as time spent in each cell
Flux goes as direction of each step, i.e. bouncing back and forth has no contribution to flux



Paper appendix discussion
---

There are different physical contributions --- we are just plotting the contribution from the eigenmodes here. The other piece becomes important for emission away from line center. There is a non-resonant piece, we don't know how to treat it. The non-resonant piece is explicitly asymmetric, so it becomes zero for emission at line center.

The lowest order eigenmode captures the late time behavior, which can be compared directly to Monte Carlo.

What is the contour we want to choose and how do we evaluate it numerically?
We have the wrong contour right now.

We seem to have an infinite number of residues. Residue theorem has to hold if the function is analytic. It seems like there must be some way of getting an exact solution that includes the non-resonant contribution. Why doesn't it automatically show up from doing the contour integral?

FIg 8 - shows only the resonant contribution, not the asymmetric piece.
Only when you include frequency diffusion do you get an infinite number of m for each n. Otherwise there's only a single resonance for each function.

 [X] Edit qualtrics survey
 [X] Shane's edits to paper


PROJECTS FOR SHANE
---
Monte carlo - not conceptually very hard
Photons bounce around, scatter, get binned when they come out
  Intuitively pretty easy to understand

Project where they add a feature to some code, and analyze the output

1) Is it something I'm interested in doing?
    Something that motivates the mentor to focus on an idea - something you want to do anyway

Could think about running the cod eunder another set of assumptions, producing another data set with different parameters. Could give that to the student rather than having them learn Athena
Lya scattering problem - not just spheres. What about other geometries?
  Core skipping method could be implemented in our problem.


NOTES ON TIME-DEPENDENT LYA PROBLEMS
---
Landau damping?

How do we know we're getting all the poles in our sweep?
  We're not --- we stop early.
  We are searching along the axis to get as many as possible.
  How can the residue theorem be wrong? - If it's right, then there's poles we're missing.
  It may not be correct to close the contour in the lower half plane when we know there are poles down there.

  If there was a real part to the resonance and it's not just damped, then we are not sweeping for that. Might be missing it.

  If you don't have single-valued functions, the residue theorem does not work. 
  Multivalued function sqrt(z) needs a branch cut (?)


NOTES ON PAPER EDITS DESCRIBED BY PHIL
---

 [X] Lowest order mode, decay time agrees with MC above some optical depth. Describe what you see in the figure that shows this. Be more specific than "it converges"

 [X] Heating is a niche thing, don't put it in the introduction. Can we explain blueshifted absorption in lya atmospheres? The forces can also contribute to escape. Balmer n=2 to 3 absorption can be set up by Lya getting the H into the n=2 state in the first place. Possible heating done by lya is in deep atmpshere where density is higher, excitation to n=2 state but if you're so dense that you can collisionally de-excite which takes 10.2 ev into an electron and goes into the gas

 [X] Circled paragraph describin gharrinton --- repeats the stuff below. Don't repeat it. 
Don't beat around the bush --- Harrington is wrong! No "the accuracy of their solution is limited by the treatment of the boundary conditions." Then go into detail and explain why it's wrong.

 [X] Have a nice short closed discussion of what happens in Harrington, and don't interrupt it with the other solutions. Don't break it up.

 [X] Reread Lao & Smith to understand what it is they're actually doing. Is there actually a closed-form solutoin?

 [X] State that divergent solution is another solutoin that you can use, doesn't satisfy bc. 
      There's ahrrington, plane parallel. Dijkstra, spherical.
      Neufeld - divergent.

 [X] Here is the problem we want to solve. Move pg 3 first sentence up to be first sentence of section 2. Sphere of radius R with constant density insie. We want to find the intesnity within that sphere. That's the basic problem. Then go into discussion of the radiative transfer problem. 

 [X] Once you have an expression for k, then you can write out expression for tau0 once k is introduced. Make sure you don't use k until you've defined it.f

 [X] What's good about Jd - it's a verys imple analytic formula. Cite neufeld paper. This is an extension of the eneufeld divergent solution using spherical geometry and moving the emission frequency away from line center. But, it doesn't adhere to the boundary condition.

 [X] Think of equations as parts of a sentence. Can't have an equation as the last part of a paragraph.

 [X] Pg 4 - you literally have to use the symbols. "It's still only a good approximation near x=0 at r=R.

 [X] Call x_0 x_peak instead. It's in doppler units.

 [X] Core wing boundary. To derive a core wing boundary, you need an a, which is 4.72e-4. Then you can find the value of x_cw.
      x_cw - value of the photon frequency in doppler units where the lorentzian function equals the doppler function.
      tau_cp - Value of the line center optical depth at which the core moves into the peak of the spectrum
    State that you set x_cw equal to x_peak to find tau_cp

 [X] Fill in discussion in section on results --- discuss SPECIFICS.
      Neufeld, Dijkstra, Smith? This paper this plot, this tau0. These are the cases where our Hbc will correct for the discrepancy between the monte carlo and the analytic solution. Beginning of pg 17.

 [X] Lao & Smith 2020 - atau0=5000, temperature much smaller at T=10K. There should be errors of order 10% compared to Monte Carlo. 1 by 1, specify the individual plot, line, all a tau0 temp parameters, we see that the theory line is below the MC by about this much


ebhlight
========
 - Tetrads for emission and scattering
 - We don't have to construct a full tetrad for resonant scattering, just a lorentz transform
 - How do they move things through the domain at every time step?
 - Make superphotons, push superphotons, main, radiation, tetrads
 - Start with main to understand the procedure
 - We need to understand the structure of how they move particles through 
 - Push superphotons goes over the main parts of it

Solve ivp with complex frequencies? Don't need to separate out real and imaginary parts.
WHen you're solving the BV problem in sigma, the estimates we have for when it begins to decay exponentially are based on using a purely imaginary frequency. 
We want contour 1, and the way we get it is by evaluating all the others.
Could stop integrating at a certain value of real omega, hope that II and IV are small
(Can confirm numerically)
Contribution of every other eigenmode is buried in the integral thta you have to do over III and V, as well as the discontinuity.
If we could se tup the integrator to use a complex frequency, there could be a few tests to do

Contour integrals
---

Experiments:
Start at ome point in the complex plane and integrate a circle, come back to the same point. If you don't get the same value you started with, you've enclosed a pole or there's a branch cut.

Set up a few numerical experiments to determine what's going on with the funciton.

If you want to be integrating over sections III and V you could do it, because you have 

Solve the boundary potblem in sigma - you can have either both imaginary and real frequency components
That reponse is one piece in a 

Do the sigma integration to get omega by omega, then you pick omega to be a sphere around an area and determine what your horrible function is.

Integrate in a circle in the complex plane, count up how many poles you have

Put eq 25 into the integrator, let omega have both imaginary and real parts. Integrate it in a circle in the complex plane. If you don't get the same answer as when you started, you have a pole enclosed.

Have ot solve the boundary value problem for the J(n, sigma, omega), and then that's the thing that appears in the expression below.


CONTOURS
========
We're using the right contours, but there may be a "branch" that we're ignoring that could contain other terms. Asymmetric piece is being left out. We can say it's clear there's some other piece missing for emission away from line center, and then show our generalization for the symmetric case. 

The eigenmodes individually are fine. There's another piece of the solution that's missing and our derivation is mising that somehow. Ideally we would fix our derivation and find the missing piece. If there's a simple way to include that piece...

Somehow we have to use the response at given values of s and be doing some sort of integral of that to obtain the function of time. 

ARFKEN - Contour integration
Chapter 6 (except 6.7 and 6.8)
Builds up to chapter 7, which contains residue theorem

Use s or gamma as the imaginary part of the frequency. That's an efficient way to get the late time behavior. There's good agreement between MC and that. That part is right.

But how do you combine the eigenmodes and this weird orther piece to get a function of time?

The only thing that may do that in principle is the straight fourier transform - real and imaginary parts of the response and do a direct fourier integral over that. But, the convergence was so slow that it wasn't going to give us anything.

Analtically, you can isolate the eigenmode piece which has a divergent denominator.
Didn't work well at low m.
Even at high m, you should be able ot put a norm factor in front of it.
There is an analytic formula for that factor, but this time sometimes it worked and sometimes it didn't. Could be off by a factor of 2 or 3. Means there's an error.
This didn't include the a2 piece because it was non-resonant.
It's hard to see how you could use the a2 piece to do anything else.
Even if you have it, there's the issue of what is the contour you're integrating over.
The solution was derived for purely imaginary frequency.
It goes to zero at line center, but then it's an Airy function outwards.
When you find your function of sigma, is the presence of the frequency there raised to a fractional power? Would force you to have a branch point in your solution.

What is a branch cut?
---

Z (real part) + i*(imaginary part)
Imaginary in y axis, real in x axis.
Nice functions are single valued. Z^2 is single valued.
Z^(1/2)
If you go around a circle to angle 2pi, the function is not single valued.
Draw a branch cut on your complex plane. Don't let a contour ever cross that line. You can make things single valued because you agree that you never go through that line.
Have to do this for multi-valued functions.


Edits to time-dependent section:
---

If we give the piece of the solution that's just a sum over eigenmodes, this is what we find: for fig 8 and 9. However, this only works for the symmetric case, and we know there's a missing piece.

 [X] Numerical recipes - read up on rejection methods, chapter on inverses
      [ ] Implement a simple rejection method on a simple distribution to get a hang of it
      [X] Read specific stuff for gamma distribution, may learn more about what's needed for our specific function

 [X] Edit paper introduction
      [X] How is the present work related to the observations?. Want to justify resonant scattering
      [X] For this paper, don't need to cite all those observations. hst has revealed an extended distribution of hydrogen around a handful of exoplanets, then give every single reference
      [X] these observations motivate study of the physics problem of lya interacting with h atoms surrounding a planet. Observed absorption is motivating us to develop a better numerical approach. HST observations reveal a distribution of hydrogen surrounding the planet to at least several radii.

 [X] Read https:/surr/ui.adsabs.harvard.edu/abs/2006ApJ...645..792T/abstract
 [X] Set up m=1000 runs on Lyra
 [X] Finish sweep test


7 June 2021 Goals:
---

 [X] Sweep test for consistency
 [X] Process m=1000 runs into plots, check convergence using steadystate.py
 [X] Process tau=10^5 MC run, make optical depth scaling plot with x axis as (a tau)^1/3
 [ ] Numerical recipes rejection methods
 [X] Read another section of Chenliang's paper
 [X] Write new sections of paper --- monte carlo description according to shane's meeting notes

21 June 2021 Goals:
---

 [X] TeX up Phil's notes on nondimensionalization and scaling relations
 [X] Plot showing H0 - MC, scale to show different tau on the same plot
 [X] Try modeling the solution as H0 + dH0/dsigma * Delta sigma (slope)
 [X] Proofread intro & steady state sections of the paper


Other
---
 [-] Analytically integrating H0 over frequency, X 4piX 4piR^2 should give luminosity
 [-] Check normalization of other pieces --- Hbc should be normalized to zero in theory? Should have
     no net contribution to the flux.
        H0 should be normalized to 1. (even analytically) CHECK THIS!
        Check normalization of function (Eq. 18 in paper) using s in ftsoln code
        for values of s not equal to zero. What is the contribution from zero?
 [X] Increase number of sigma points by x10 and see if the integral normalizations get better - just with H0 (analytic) so we don't have to let ir run for 30 min
 [-] Run analytic solutions between tau=1e5 and 1e6 to see if the normalization shift is gradual or not. Why is it changing?
 [-] Nondimensionalize pieces of Eq. 18
        turn s in to sbar / tau0
        turn sigma into tau0*sigma
        line profile goes as tau -2/3 (?)
        kR/Delta = tau0
     Plug these scaling relations in to eq. 19 to get scaling of Hbc
        should get Hbc ~ (atau0)^1/3 
     Should take ~10 minutes

2021-06-25
---

Everything was derived to be on the line wing, so we need to ignore what's 
going on in the doppler core because that's where our assumptions break down.

It's true that our equations were derived in the line wing, however, if all 
we're interested in is a superfiicial treatment of the core where we include 
the doppler core, we magically find that this inconsistent thing tends to agree
better with the monte carlo results. 

It's not consistent, and everything breaks down, so we may as well do whatever 
looks best.

 [X] atau^1/3 is the position of the peak in x. Everything goes bad as soon as 
     that peak gets close to the boundary between the doppler core and the 
     damping wing. We're choosing a certian value of the damping parameters a. 
     We can root solve for the position x where the doppler core hits the peak. 
     DO THIS. Solve for tau where the doppler core hits the peak of the 
     distribution (atau^1/3).

Our equations were not derived knowing anything about the doppler core. 
Appendix - if you actually want to get a real Laplacian (differential equation)
you need to plug in 1/x^2 for the line profile. This is why our equations are 
only valid in the wing.

 [X] It's really \bar sigma_i / tau0 (in x units -- CALCULATE THIS) that 
     determines how big the corrective term must be
 [X] Make a theory plot that increases sigma_i and check the size of Hbc
     - Corrective term is bigger for lower optical depths, could do tau=1e6




2021-06-30
---

 [X] Create three panel plot for tau=10^6, xinit varying
 [X] xinit.py plot
      tau0 = 10^5, 10^7, 10^9
      xinit = 0*tau0, 2*tau0, 3*tau0, 4*tau0
      Explain that BC term becomes more important the further away from line center the emitted photons are. 
 [ ] Conclusions: this paper was motivated by wanting to understand the errors introduced by the mathematiclaly incorrect ansatz introduced by Harrington
     We've now quantified that and shown in detail how the BC correction depends on all these different parameters.
 [X] Discussion of steady state mC results: Older references that started with lower tau0. In citation of Smith et al 2015 mention the exact optical depth they used.
 [X] Explain units of Eq. 30
      energy per area per time per hertz - meant to be integrated against nu, not sigma
      Fourier coefficients have an extra unit of time in the numerator
      Eq. 30 - fourier coefficients have same units as the left, divided by time
      Here J has the erg/cm^2/s/Hz
      Total energy input E, k, and Delta
      E/R^2 delta out front

      Delta functions have units of 1/quantity they should be integrated over
      So d^3x has units of 1/R^3

The J in eq. 29 is the standard specific mean intensity in units of energy per unit area per unit time per unit Hz
For the emissivity in 28, etc
"These are standard CGS units for emissivity and mean intensity"

 [X] Jsoln plot: Shows how oscillatory the eigenmodes are, shows how you need to go to high orders to get perfect cancellations

Scale x over m, show that the functions are oscillatory out to sigma_tp?
 [X] Include discussion of sigma_tp and derivation of efoldings, etc

2021-07-07
---

Is there any reason for the theory lines in panel 3 of fig 4 to agree with the monte carlo?
 - Try this log scale instead

 [X] Include the optical depth at the emission frequency in figure captions for xinit plots
 [X] Make log versions of the xinit plots
 [ ] Reintroduce line styles into fig 5. Make the last one slightly thicker.
 [X] Sum Pnm, see if they're close to 1
       [ ] Discuss this in the paper - why does it alternate between 0 and 2?
 [-] Pnm v. m, size of the Pnm scales as a powerlaw (not a very quickly decaying one)
 [X] For Pnm powerlaw, actually mention analytic formula that agrees well with Pnm scaling,discuss how it takes so many m to get the Pnm to be small (i.e., they are converged)
 [X] Run phils Pnm powerlaw code to see if my convergence is better
       [ ] Test this agian on the new time dependent data once it finishes
 [X] !!! Add MC for 5, 6, 7 to the tauscaling plot
 [X] Tauscaling plot: fit only the exponential piece and compare that to the lowest order eigenfrequency
     Error goes as root N, use that as error estimate in the fit after chopping off peak.
 [X] !!!!! Running the code for the time-dependent cases when xinit is not equal to zero
     Is there a big difference in the lowest order eigenmode when you're emitting it in the wing?
     sigma_s = 1 or 2, should see significant difference in the fluence. How different is the wait time?
     If we see the exact same wait time distribution even when things are shifted.
     xs = 6 for tau=10^6 or xs=12 for tau=10^7
 [X] Produce escape time distributions for tau0=1e5, 1e6. See if we get worse exponential falloff agreement



2021-07-09
---


 -  [X] Make xinit plots logscale, reupload to paper. Change y label to be log |P(x)|
 [ ] Find out why discontinuity doesn't occur at x_s in Jsoln plots. Track down bug
         [ ] See if this is related to the wiggles in the Pnm v. m plot
 [X] Plot new scaling.py plot with fixed legend & label. Beautify for final draft!
 [X] Make escape time distribution plots for all MC data, even without time dependent
     fits. See which are worth making with tdep code.


4PI * 4PIr^2 * INT (DNU)
change integral to dsigma
delta function in sigma
normalization, change in luminosity is given entirely by the A(s=0) which should be zero. Is it?

one more three panel plot with optical depth
Put in a placeholder with tau=10^6 repeated. Write something
It scales this way, this is how the agreement with MC goes
Won't be certain how good the agreement is, but
if we plot 106 with 107, 107 will hopefully be a little more like the analytic solution
We want to show convergence.

Write it as if the trend is consistent between 10^5 and 10^6 and 10^7.

Wanna make this point:
  Then think what is the plot that I can show to prove this point?
    I'm gonna make this plot.
    Make the plot
  How do I tailor the plot to make the point as succinctly as possible?
  All of the writing is epxlaining the plot and escribing how it makes the point.


Tow things:
  One is just the trends seen as you increase the x0 position.
  Other is agrement with the analytic solution.


as inition freq. increases from line center, the spectrum becomes asymmetric
the solution captrues this asymmetry well

Describe how as the line center optical depth... same thing as above

Make sure to convey very carefully what's plotted in the figure. Must explain 
what P(x) is, explain what each line is, refer bcak to the equations, try to
read through after writing assuming you're starting off from zero knowledge.

Make sure you're talking to an audience in that boat.

Once you've explaining the frist plot, fig 2 and 3 should be very straighforward.
Same curves as before, have the same meaning. Only thing that changes is the initial freq v.
the line center optical depth.

Just describe what you see inthe plot.


Derivation of the analytic expression
Comparison to Monte Carlo
  Couple paragraphs describing MC code, cite Chenliang's paper (same code)
  Describe a general comment on MC, how it works, with references to some papers
  Talk about the problem setup specific to this test
      Set up a uniform sphere of constant density whose radisu is this, optical depth this to match analytic
      Then specify parameters, specify homogeneous and isotropic sphere
      Take escaping photon samples, bin in frequency, and then that's it
      Treatment of Lya is the same as chenliang's paper. Basically want to show it's already been used.

A paragraph on the MC method to references with other papers and a brief description of how MC works
Describe how the specific problem is handled and the parameters that are chosen
Then get inot comparison of the plots

Time dpeendent scheme is really just a way to get a handle of the escape time distribution



 - Compile code on lyra, run through tests
    - compare outputs (supermongo plotting), maybe make equivalent in python

 - individual cell has optical depth 10^8, for on ephoton to escape 1 cell would be computationally impossible
How do we speed it up?
    - Have a photon, draw a sphere around. After it has traveled through the optical depth, what is the p distribution for the photons that come out of the sphere? What direction, how long does it spend inside?

 - Log normal distribution! (From Phil's work)
    - If this was true, it would be very easy to sample
    - We don't have a series solution for the Ly A case


 - [X] Plot sigma turning point, source, boundary for n=20 rather than n=1
 - [X] find where sigma_tp < sigma_s, for large n small m
         - See if there is exponential growth in between the turning point and the source
         - Plot the eigenfunctions?
 - Only going to get an asymmetry where the turning point is inside the source
     - Label source sigma and turning point sigma
n=20 m=1


 - To quantify asymmetries: Choose values of sigma that are positive, interpolate to find the value of J at the positive and negative of those values, then subtract them


Time-dependent Contour
---

 - Contour you draw at each time should depend on time?
Critical value of time is 1/gamma
Doesn't make sense to go to values of gamma much larger than 1/time
If you did draw a contour that went out near the resonance with gamma of order 1/time, you're only going to include the eigemodes with smaller gammas
The contributions from sections 3 and 5 are not small anymore - could be comparable to the other eigenmodes.
If you include the residues of a certain number of eigenmodes, but sections 3 and 5 are using a value of gamma real so that the exponent is 1. They would naturally have a significant contribution.
AT late times, you end up with exactly the contour you already did