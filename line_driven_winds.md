# Line Driven Winds

Sergei Dyda

---

### O Stars
 - Blue-shifted absorption lines, thermal driving ruled out
 - Radiation pressure due to spectral lines must be driving the wind.
 - **Mass Loss Rate**: Can we explain mass loss rates for theses kinds of winds? 
     - Look at the emission lines - how much gas do we need to have this much emission?
     - Mismatch between basic line driving theory and outflow velocity (from how much lines are blueshifted)
     - Mass loss rate is prop. to v, rho
     - Hard to go from emission to density -- assumes things are optically thin, rho^2, what kind of emission? Free-free? Line (depends on excitation)?
     - *Leads to conclusion that the winds are clumpy - accounts for differences*

### Wind Physics

 - Balance of forces between gravity and a driving force. 
 - Thermal driving - gas pressure
 - Radiation driving - radiation pressure
 - Magnetic driving - star's magnetic field

#### Thermal Driving (Parker Wind)
 - Bondi accretion in reverse
 - Spherically symmetric. Mdot is constant, momentum is conserved (pressure gradient, gravitational force)
 - Get the mass loss rate
 - Critical point corresponds to intersection between Bondi solution and Parker solution (v=0 at outside, accreting inwards vs. v=0 at origin, driving outwards)

#### Line Driving (Castor, Abbot & Klein Wind)
 - Radiation pressure term looks different - depends on number of driving lines. Can drive a wind even for sub-Eddington sources
 - Gives you velocity as a function of radius - solution that passes through the critical point
 - Books: Milhalas & Milhalas, radiation hydro in detail. Lamers & Castanelli for winds.
 - Imagine box of gas, putting radiation in from the left with different frequencies. One available line. Gas is optically thin for frequencies that do not correspond to lines. But, as the gas accelerates, doppler shifts, and there is always a frequency that creates a radiation pressure in the flow.
 - How do you actually get them going in the first place? Tough, unsolved problem. People usually just inject an initial velocity.

##### Force Multiplier M(t)
- Run photoionization code for a bunch of different parameters, then interpolate between them to create lookup tables.

#### Instability
 - How do you generate clumps in these kinds of winds to tinker with the mass loss rate?
     - Line deshadowing instability: Creates clumps on sub-Sobolev scales.
         - **Sobalev Length** v_th / (dv/dl)
     - Sobalev approximation: have to get out of the line core. This term naturally falls out of the radiation force.