Reviewer:

This article entitled "Resonant Scattering in a Uniform Sphere with Large Optical Depth" presents two novel additions to the (semi-)analytic Lyman-alpha radiative transfer literature: (1) a correction procedure to enforce boundary conditions with no incoming intensity and (2) an eigenfunction expansion handling the time dependence of an impulse source. I am certain that radiative transfer experts will greatly appreciate the efforts made by the authors to rigorously understand this important (but highly idealized) problem, including the appendices. However, broader readership may be somewhat limited due to the highly mathematical presentation style and content. It is unfortunate that the results are relatively complex compared to known analytic solutions (for what is gained) and that the impulse solution converges so slowly. This work is well-written and clearly interesting both as an academic exercise and for furthering our intuition. I recommend acceptance after minor revisions.

General comments:

1. The title may be too general as it does not actually mention the main novel aspects of the work and most people think that resonant scattering in a sphere is a solved problem. I suggest revising to include key words such as (semi-)analytic, BC corrections, or an impulse source, but I leave this up to the authors.
2. I realize there may be a difference in style across fields, but I recommend more thorough citations of previous works. This is helpful for readers to better connect to other fields and appreciate the context of your results.
3. Very minor but I suggest removing redundant significant figures, i.e. replace xs = 0.0 with xs = 0 and tau0 = 1.0 x 10^7 with tau0 = 10^7. This is frequent and distracting.

Abstract:

1. The first several sentences are dense and generic, especially because the basic analytic solution has been known for decades. I suggest stating the novel aspects of your work earlier and motivating the study either through science applications or furthered understanding.

    - **The abstract now leads with the introduction of the novel additions to the basic analytic solution. Additional sentences outlining applications and potential for acceleration of Monte Carlo using the time-dependent escape time distribution have been added to motivate the study through science applications.**

2. "Decreases slowly with line center optical depth" is too vague. I suggest adding something like "specifically (a tau0)^-1/3" as this is the intuition I want to remember.

    - **The specific scaling with line center optical depth has been inserted in this sentence.**

Section 1. Introduction:

1. Please include a few high-level references, e.g. in the first paragraph you can use Dijkstra (2019; Saas-Fee 46, 1). Also, statements like "Lya may ionize atoms...drive an outflow" probably need citations.

    - **A citation of Dijkstra (2019; Saas-Fee 46, 1) has been added after first few sentences. Added reference to Bourrier (2018; A&A 620, A147) after statement about outflow.**

2. Given that your work is quite general, please mention that Lya radiation transport is an active area of study for a wide range of astrophysical settings, i.e. including planets, stars, galaxies, and cosmology.

    - **The first paragraph of the introduction has been reorganized to address the general study of Lya radiation transport first, and then specify that in this work we consider one of its applications to exoplanet atmospheres. A sentence has been added mentioning the general areas referenced in this comment.**

3. Please add more motivation for studying time-dependent sources in the context of Lya (pulse, point-like if you want to be specific). For example, atmospheric flare ups, quasar variability, gamma-ray bursts, cosmic expansion, Lya radiation pressure, etc. Potentially relevant references: Roy, Shu, Fang (2010; ApJ, 716, 604), Xu, Wu, Fang (2011; MNRAS, 418, 2), Smith et al. (2018; MNRAS, 479, 2065) etc. present time-dependence calculations.
4. "Monte Carlo methods directly coupled with fluid dynamics" should cite Smith, Bromm, Loeb (2017; MNRAS, 464, 2963, see also 2016; MNRAS, 460, 3143). Also, potentially relevant is the local sub-grid model of Kimm et al. (2018; MNRAS, 475, 4617).
5. Number of scatterings proportional to tau0 should cite Adams (1972; ApJ, 174, 439).
6. "A method that can accurately characterize..." should mention dynamical core-skipping is used for MCRT codes. It is fine to argue we should still explore others, or that intuition alone is sufficient motivation, but as written this sounds like this is a completely unsolved problem.
7. Several places use "out on the wings" or similar, which should instead use the word "in" (see also after equation 3 and end of section 2 etc.).
8. "To our knowledge...never been quantified" should be updated to reflect recent studies that provide insights about related concepts. For example, Lao & Smith (2020; MNRAS, 497, 3925) use a parametrized boundary condition to incorporate some flexibility and Tomaselli & Ferrara (2021; MNRAS, 504, 89; equation A18) explore an alternative flux-free boundary condition. While this is qualitatively different than your approach and does not quantify errors, it is worth mentioning.
9. In the last paragraph, please mention (after the Dijkstra sentence) that Lao & Smith (2020) generalize the slab and sphere solutions to arbitrary power-law density and emissivity profiles.
10. Please look at and cite Seon & Kim (2020; ApJS, 250, 9) in a relevant context. You may find Appendices B and C interesting (and text/figures for section 3.1). You may also find the exploration of dust in Appendix A3 of Tomaselli & Ferrara (2021) to be interesting and relevant.

Section 2. Steady-State Solution:

1. This is a matter of taste (optional), but I find the notation is greatly simplified by moving to dimensionless units in all variables, e.g. factors of k/Delta go away. Also, modern papers often use the conventions of tau0 = k * R rather than tau0 = k * R / sqrt(pi) Delta.
2. After equation 12: Do you mean "Equation (11) will be shown"?
3. After equation 18: Please mention roughly how many points are required for convergence. Can a non-uniform lattice be used to optimize computation? Can this approach be practically extended to non-uniform density, e.g. as a system of equations with interior and boundary conditions?
4. Figure 1: Please provide brief summaries in the caption (or in the labels) to remind readers skimming this, e.g. H0 = fiducial solution, Hd = divergent solution, Hbc = our additional correction accounting for more self-consistent BCs. Finally, people may be more used to seeing J (rather than H) so please point out when/if there is are important differences for interpreting results.
5. End of Section 2.1: Perhaps it is useful to emphasize that the BC failure occurs before the spatial and frequency diffusion approximations. For example, it is known that the traditional solution is accurate for atua0 > 10^3 (i.e. atau0^1/3 >> 1), but several things may go wrong as you reach atau0 < 1. Do you have further insights about the order of failures?
6. In my opinion it is more helpful to think in terms of atau0 rather than tau0. Perhaps you can periodically remind the reader that T = 10^4 K leads to atau0 = ?, e.g. in the caption of Fig. 1.
7. Fig. 2: Why are the yellow curves V shaped at line center when the standard is more U shaped? Is this due to plotting resolution or not using the wing approximation for the Voigt function? This is more apparent in figure 3, so it is worth explaining this feature properly.
8. Figure 3: It looks like the MC error bars are not representative of the true uncertainty, i.e. small compared to the variation between neighboring bins. Can you fix or comment on this, e.g. is this the Poisson statistical uncertainty?

Section 3. Time-Dependent Diffusion

1. Figure 5: Perhaps a combination of different colors and line styles (in addition to line thickness) would make the trends clearer.
2. Equation 33: Can you remind the reader about why this is the solution? (e.g. ansatz, harmonic forcing, residue calculus, etc.)
3. End of Section 3.1: Please summarize the intuition gained here, e.g. requiring more spatial terms than time terms is a result of the diffusion (Brownian motion / heat equation) behavior?
4. Before equation 42: "hone in on the eigenvalue" Can you be more specific, e.g. do you use a bisection method? Is convergence robust/predictable, e.g. if you take too large of a window then you have two resonance values? Also, do you mean "gamma_nm can be calculated by linear interpolation from" or is it something else?
5. Equation 44: This may not be an obvious step to the reader, e.g. there is a subtraction related to equation 33, but how is the denominator obtained, e.g. jump condition, etc.?
6. Above equation 47: "The overall scale of the Jnm..." It may be good to also put this in the figure 6 caption or to normalize out parameters in the y label.
7. After equation 47: "as shown in Appendix B, where the WKB approximation reveals kappa ... = pi/8".
8. After equation 49: Please cite Adams (1975; ApJ, 201, 350) regarding the "expected atau0^1/3 scaling".
9. Figure 8: Please report the coefficient of the atau0^1/3 scaling used in this figure, e.g. for comparison with t_trap/t_light ~ 0.901 atau0^1/3 after equation 91 of Lao & Smith (2020). If it is much higher or lower, please explain why it differs from the Steady State solution.
10. Figure 8: There are missing MC points at 10^8 and 10^9, which is understandable given the computational expense without core skipping (so this is optional). However, can you comment on how quickly the MC agrees with the analytic scaling or if the systematic excess decays slowly?

Section 4. Discussion:

1. "Laplacian form [for frequency redistribution] ... correct [for photons in the wing] where the line profile is".
2. Figure 9: Give some physical intuition about what fluence is in the caption, e.g. steady state spatially integrated flow of radiation at a given frequency. Mention more about behavior of SS vs partial sum vs time-integrated in the caption, e.g. briefly explain uptick and oscillations. Also, it is a bit confusing that the axis does not go to zero. Perhaps you can use the range [0,30] to show no motion near the core (took me a second to understand why it was so high).
3. Figure 9: I believe you can provide a closed form expression for the purple curve. Please do so if it is simple, e.g. in principle this should be very similar to the J version from equations 81 and 90 of Lao & Smith (2020).
4. Figure 10 Caption: Mention the analytic solution underestimates compared to MC because core scatterings are important when atau0 < 10^3.
5. Last paragraph of Section 4.1: atau0 >= 1 is maybe not the right message, i.e. BC correction does help with the overall shape but the exact solution is still not in perfect agreement (i.e. order unity errors) here. It is probably fine to say atau0 >> 1 or emphasize some improvement from the standard atau0 > 10^3. (Also, it may be best to say derived spectrum J0 rather than H0 in this discussion.)
6. End of Section 4.2: Core-skipping and hybrid diffusion methods "do not track the amount of time photons spend in the line core" is a bit misleading. In fact, state-of-the-art codes use dynamical core skipping that checks the uniformity in the density and velocity fields, i.e. no serious violations are made. In fact, one could estimate how many scatterings and the path lengths that are skipped. Also, rDDMC in practice requires interfacing with traditional MC for core photons for a similar reason that you would not want to apply spatial diffusion methods in optically thin gas.
7. End of Section 4.2: "faces of each grid cell" For your own reference the Lucy path-based estimator method is much better than tracking face fluxes (as done in Huang 2017 and Smith 2017).

Appendix A:

1. Also cite Unno (1952; PASJ, 4, 100) regarding the partial redistribution function, e.g. equation A4 (isotropic version).
2. Also cite Osterbrock (1962; ApJ, 135, 195) regarding the moments of the redistribution function, e.g. equation A7.
3. Also cite Rybicki & Dell'antonio (1994; ApJ, 427, 603) regarding a derivation of the Fokker-Planck terms (their Appendix).
4. After equation A6: Do you mean "scattering cancel for p = 0"?
5. Equation A15: I suggest removing the = 0 at the end, which is misleading in my opinion. At any rate, the explanation in the text is enough.
6. Typo after equation B5: "By setting the denominator".