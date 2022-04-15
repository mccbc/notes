Reviewer:

This article entitled "Resonant Scattering in a Uniform Sphere with Large Optical Depth" presents two novel additions to the (semi-)analytic Lyman-alpha radiative transfer literature: (1) a correction procedure to enforce boundary conditions with no incoming intensity and (2) an eigenfunction expansion handling the time dependence of an impulse source. I am certain that radiative transfer experts will greatly appreciate the efforts made by the authors to rigorously understand this important (but highly idealized) problem, including the appendices. However, broader readership may be somewhat limited due to the highly mathematical presentation style and content. It is unfortunate that the results are relatively complex compared to known analytic solutions (for what is gained) and that the impulse solution converges so slowly. This work is well-written and clearly interesting both as an academic exercise and for furthering our intuition. I recommend acceptance after minor revisions.

General comments:

1. The title may be too general as it does not actually mention the main novel aspects of the work and most people think that resonant scattering in a sphere is a solved problem. I suggest revising to include key words such as (semi-)analytic, BC corrections, or an impulse source, but I leave this up to the authors.

    - **The title has been changed to include the keywords relating to the novel aspects of the work, rather than a general description of the problem. Now includes mention of "Self-Consistent Boundary Conditions", which is the chief novel aspect of the work.**

2. I realize there may be a difference in style across fields, but I recommend more thorough citations of previous works. This is helpful for readers to better connect to other fields and appreciate the context of your results.

    - **References to several additional papers recommended by the reviewer have been included as indicated.**

3. Very minor but I suggest removing redundant significant figures, i.e. replace xs = 0.0 with xs = 0 and tau0 = 1.0 x 10^7 with tau0 = 10^7. This is frequent and distracting.

    - **These replacements have been made in all figures, figure captions, and in the main body of the text.**

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

    - **A paragraph discussing the motivation of the time-dependent problem has been added to the introduction. References to Roy, Shu, Fang (2010; ApJ, 716, 604), and Xu, Wu, Fang (2011; MNRAS, 418, 2) have been added with mention of their applications. The reference to Smith et al. (2018; MNRAS, 479, 2065) regarding their time-dependent hybrid diffusion method is included in the conclusions section.**

4. "Monte Carlo methods directly coupled with fluid dynamics" should cite Smith, Bromm, Loeb (2017; MNRAS, 464, 2963, see also 2016; MNRAS, 460, 3143). Also, potentially relevant is the local sub-grid model of Kimm et al. (2018; MNRAS, 475, 4617).

    - **Citation to Smith, Bromm, Loeb (2017; MNRAS 464, 2963) added after the statement about MCRT coupled with fluid dynamics.**

5. Number of scatterings proportional to tau0 should cite Adams (1972; ApJ, 174, 439).

    - **This reference has been inserted here.**

6. "A method that can accurately characterize..." should mention dynamical core-skipping is used for MCRT codes. It is fine to argue we should still explore others, or that intuition alone is sufficient motivation, but as written this sounds like this is a completely unsolved problem.

    - **Elaboration on this point is present in S4.2 Impulsive Source. The statement on this topic in the introduction does not necessarily imply that such a method does not exist, merely that it has the potential to accelerate the calculation.** 

7. Several places use "out on the wings" or similar, which should instead use the word "in" (see also after equation 3 and end of section 2 etc.).

    - **This phrasing has been corrected in each instance.**

8. "To our knowledge...never been quantified" should be updated to reflect recent studies that provide insights about related concepts. For example, Lao & Smith (2020; MNRAS, 497, 3925) use a parametrized boundary condition to incorporate some flexibility and Tomaselli & Ferrara (2021; MNRAS, 504, 89; equation A18) explore an alternative flux-free boundary condition. While this is qualitatively different than your approach and does not quantify errors, it is worth mentioning.

    - **Lao & Smith 2020's figure 12 demonstrates the disrepancy our novel solution corrects for, since their equation 36 seems to use the same boundary condition as Harrington 1973. Tomaselli & Ferrara 2021 use a flux-free boundary condition, which also does not address the frequency-dependent correction as they also use an approximation for the frequency-dependent eigenvalues. These references have been added in the discussion of previous works.**

9. In the last paragraph, please mention (after the Dijkstra sentence) that Lao & Smith (2020) generalize the slab and sphere solutions to arbitrary power-law density and emissivity profiles.

    - **This reference has been added along with a short description of the work presented.** 

10. Please look at and cite Seon & Kim (2020; ApJS, 250, 9) in a relevant context. You may find Appendices B and C interesting (and text/figures for section 3.1). You may also find the exploration of dust in Appendix A3 of Tomaselli & Ferrara (2021) to be interesting and relevant.

    - **The work presented in Seon & Kim 2020 (Figure 1) also shows the discrepancy at the peak of the spectrum that our work aims to correct, since the analytic solution is from Neufeld 1990 which is discussed elsewhere in our paper. Figure 2 shows another solution, but this is derived from the same series solution as Dijkstra et al 2006, which is discussed elsewhere in our text, which uses an approximation for the frequency-dependent eigenvalues. The reference has been added in our discussion of previous work in the introduction.**

Section 2. Steady-State Solution:

1. This is a matter of taste (optional), but I find the notation is greatly simplified by moving to dimensionless units in all variables, e.g. factors of k/Delta go away. Also, modern papers often use the conventions of tau0 = k * R rather than tau0 = k * R / sqrt(pi) Delta.

    - **Thank you for the recommendation, but we prefer the current notation.**

2. After equation 12: Do you mean "Equation (11) will be shown"?

    - **Yes. Equation 12 is evaluated at the surface, so it doesn't make sense to talk about its behavior at r=0. This has been corrected.**

3. After equation 18: Please mention roughly how many points are required for convergence. Can a non-uniform lattice be used to optimize computation? Can this approach be practically extended to non-uniform density, e.g. as a system of equations with interior and boundary conditions?

     - **The number of points and value of \sigma_{\rm max} we used in our calculations have been included in this paragraph. The calculation is trivially fast with a uniform lattice. Though a non-uniform lattice would certainly perform better, it is simpler to use a uniform grid.**
     - **Other density profiles are beyond the scope of this work.**

4. Figure 1: Please provide brief summaries in the caption (or in the labels) to remind readers skimming this, e.g. H0 = fiducial solution, Hd = divergent solution, Hbc = our additional correction accounting for more self-consistent BCs. Finally, people may be more used to seeing J (rather than H) so please point out when/if there is are important differences for interpreting results.

    - **The mentioned labels have been added to the captions Figure 1. The legend of Figure 1 has been changed to match the format used in the other figures, i.e. H_0 + H_{bc} rather than H_{0+bc}. The tick labels on the y-axis of the log-scale plot have been changed to 10^(number) format to be consistent with the other figures in the paper.**
    - **The last point regarding H vs. J should be self-explanatory from our equations and their conversion to escape probabilities (see Eq 22). Also, J=0 at the surface for the fiducial solution, so this would not be useful to plot since we are evaluating all solutions at the surface of the sphere.**

5. End of Section 2.1: Perhaps it is useful to emphasize that the BC failure occurs before the spatial and frequency diffusion approximations. For example, it is known that the traditional solution is accurate for atua0 > 10^3 (i.e. atau0^1/3 >> 1), but several things may go wrong as you reach atau0 < 1. Do you have further insights about the order of failures?

     - **We discuss points in section 2.1 that address this. It is mentioned that photons at the peak are optically thin when atau0~1, and that our solution is only valid when the peaks of the distribution lie well outside of the Doppler core, i.e. for large tau0.**

6. In my opinion it is more helpful to think in terms of atau0 rather than tau0. Perhaps you can periodically remind the reader that T = 10^4 K leads to atau0 = ?, e.g. in the caption of Fig. 1.

    - **A sentence mentioning the value of atau0 has been added in the caption of Figure 1. Since we use the same temperature throughout the paper and the optical depth changes only by factors of 10, it is easy for the reader to scale this value for the other plots at different optical depths, so we will not repeat it in every caption.**

7. Fig. 2: Why are the yellow curves V shaped at line center when the standard is more U shaped? Is this due to plotting resolution or not using the wing approximation for the Voigt function? This is more apparent in figure 3, so it is worth explaining this feature properly.

    - **The plotting resolution in the core is intentionally low since the solutions don't use the core component of the line profile. The submitted version of Figure 1 interpolated over x values in the core, leading to the curved shape. To avoid confusion, this interpolation has been turned off and Figure 1 has been re-plotted. The "V" shape is due to the low number of points in the core, and an explanation of this has been added in the paragraph discussing Fig 1.**

8. Figure 3: It looks like the MC error bars are not representative of the true uncertainty, i.e. small compared to the variation between neighboring bins. Can you fix or comment on this, e.g. is this the Poisson statistical uncertainty?

    - **The error used on these points is the square root of the number of counts in each bin, which is normalized and then plotted on a log scale. The bins at the very edge of the spectrum have only tens of photons (10 photons ~ 30% error), but by the 5th data point in from the left side the counts are up to ~100 (~10% error), so the error bars are very small here.**
    - **We conducted an independent calculation to test the accuracy of the error bars shown in the plot. Monte Carlo simulations at an optical depth of tau0=10^4 were performed with N=100,000 and N=10,000 photons, which were each binned in frequency as in Figure 3. Square root N error is not expected to be accurate in bins with less than about 20 counts, so these were excluded from the study. It was found that 68% (22 out of 32) of the remaining points from the higher N simulation fell within the error bars of the lower N simulation, which is consistent with 1-sigma error, as expected.**
    - **A sentence has been inserted in the discussion of Figure 1 stating exactly how we quantify the error of the MC data. sqrt(N) error is a standard approach, and the simple test described above should resolve any concerns that the error bars are not representative of the uncertainty in the data.**

Section 3. Time-Dependent Diffusion

1. Figure 5: Perhaps a combination of different colors and line styles (in addition to line thickness) would make the trends clearer.

    - **This was explored, and unfortunately adding line styles to the plot confuses the trends and makes the plot more visually cluttered. As an alternative solution, the plot has been separated into three panels so that the trend can be seen more effectively while still preserving the scale and relative offset of the solutions.**

2. Equation 33: Can you remind the reader about why this is the solution? (e.g. ansatz, harmonic forcing, residue calculus, etc.)

    - **New text has been added referring to Equation 33 as "an approximate expression for the resonant response of the eigenfunctions".**

3. End of Section 3.1: Please summarize the intuition gained here, e.g. requiring more spatial terms than time terms is a result of the diffusion (Brownian motion / heat equation) behavior?

    - **The sentence has been rephrased to more clearly state that the Pnm terms converge slowly in both n and m. A reference has been added to where this is discussed most clearly in the text, which is the paragraph describing Figure 9. It is mentioned there that additional spatial terms improve the accuracy of the solutions in the line core. Additional frequency modes reduce the "noise" or "ringing" in the solution due to more perfect cancellations with lower-order terms.**
    - **A sentence has been added discussing the physical intuition of the modes. The intensity rapidly falls off at the surface, so if you do not have sufficient spatial resolution you will not be able to resolve this falloff accurately. This is why you need many n terms. For the m terms, you have a very steeply falling function in the line wings and you're trying to resolve it with a Fourier sum. The further out you want to go, the more terms you need.**

4. Before equation 42: "hone in on the eigenvalue" Can you be more specific, e.g. do you use a bisection method? Is convergence robust/predictable, e.g. if you take too large of a window then you have two resonance values? Also, do you mean "gamma_nm can be calculated by linear interpolation from" or is it something else?

    - **The method by which the eigenvalue/eigenmode is refined is described by equations 42, 43, and 44. The phrase "hone in on" has been replaced with a sentence stating that we are about to describe how we refine the value of the eigenfrequency, for clarity.**
    - **After equation 43, a more explicit mention of the refinement procedure has been added, stating what the error of a \gamma_guess is.**
    - **The second point about multiple resonance values in a large window is addressed in the description after Eq 47: "When sweeping through to find resonances, Equation (47) is used to set the scale of the sweep points \gamma_j to ensure no \gamma_nm are missed"**
    - **Regarding the third point: linear interpolation is now mentioned to be explicit about the calculation of \gamma_guess.**

5. Equation 44: This may not be an obvious step to the reader, e.g. there is a subtraction related to equation 33, but how is the denominator obtained, e.g. jump condition, etc.?

    - **The description surrounding Eq 44 has been reworded to state how Jnm is obtained. Two points \gamma_1 and \gamma_2 are plugged in to Equation 33, the result subtracted, and Jnm solved for to obtain Eq 44.**

6. Above equation 47: "The overall scale of the Jnm..." It may be good to also put this in the figure 6 caption or to normalize out parameters in the y label.

    - **This has been added to the Fig 6 caption.**

7. After equation 47: "as shown in Appendix B, where the WKB approximation reveals kappa ... = pi/8".

    - **It has been added that Equation 47 comes from setting the denominator of equation B5 in the WKB approximation to zero.**

8. After equation 49: Please cite Adams (1975; ApJ, 201, 350) regarding the "expected atau0^1/3 scaling".

    - **Added.**

9. Figure 8: Please report the coefficient of the atau0^1/3 scaling used in this figure, e.g. for comparison with t_trap/t_light ~ 0.901 atau0^1/3 after equation 91 of Lao & Smith (2020). If it is much higher or lower, please explain why it differs from the Steady State solution.

    - **The exact scaling of 0.51 (atau0)^(1/3) has been added, and a comparison has been added to the scaling mentioned in Lao & Smith (2020).**

10. Figure 8: There are missing MC points at 10^8 and 10^9, which is understandable given the computational expense without core skipping (so this is optional). However, can you comment on how quickly the MC agrees with the analytic scaling or if the systematic excess decays slowly?

    - **The excess fits well to an exponential decay with tau0, and the expected fractional error at tau0=1e9 is 0.016. A sentence has been added here discussing this.**

Section 4. Discussion:

1. "Laplacian form [for frequency redistribution] ... correct [for photons in the wing] where the line profile is".

    - **This sentence has been rephrased according to the suggested edits.**

2. Figure 9: Give some physical intuition about what fluence is in the caption, e.g. steady state spatially integrated flow of radiation at a given frequency. Mention more about behavior of SS vs partial sum vs time-integrated in the caption, e.g. briefly explain uptick and oscillations. Also, it is a bit confusing that the axis does not go to zero. Perhaps you can use the range [0,30] to show no motion near the core (took me a second to understand why it was so high).

    - **A definition of the fluence has been added in the caption, as well as a reference to the equation where it is defined. We leave explanation of the behavior of the solutions to the body of the text, and reserve the caption for explaining what is shown. A note has been added explaining the starting value of the x-axis - we wish for the reader to focus on the behavior in the line wing and not be distracted by the solutions' behavior in the line core, which is not spatially resolved by the number of modes used.**

3. Figure 9: I believe you can provide a closed form expression for the purple curve. Please do so if it is simple, e.g. in principle this should be very similar to the J version from equations 81 and 90 of Lao & Smith (2020).

    - **This expression is our Equation 12, which is discussed in the paragraph explaining Figure 9.**

4. Figure 10 Caption: Mention the analytic solution underestimates compared to MC because core scatterings are important when atau0 < 10^3.

    - **This information is present in the paragraph discussing Figure 10, right before section 4 begins.**

5. Last paragraph of Section 4.1: atau0 >= 1 is maybe not the right message, i.e. BC correction does help with the overall shape but the exact solution is still not in perfect agreement (i.e. order unity errors) here. It is probably fine to say atau0 >> 1 or emphasize some improvement from the standard atau0 > 10^3. (Also, it may be best to say derived spectrum J0 rather than H0 in this discussion.)

    - **The atau0 >= 1 has been changed to atau0 >> 1.**

6. End of Section 4.2: Core-skipping and hybrid diffusion methods "do not track the amount of time photons spend in the line core" is a bit misleading. In fact, state-of-the-art codes use dynamical core skipping that checks the uniformity in the density and velocity fields, i.e. no serious violations are made. In fact, one could estimate how many scatterings and the path lengths that are skipped. Also, rDDMC in practice requires interfacing with traditional MC for core photons for a similar reason that you would not want to apply spatial diffusion methods in optically thin gas.

    - **This is a good point. Our discussion of these methods have been rephrased to remove the mention of limitations of the alternate approaches, which was an incomplete description given the recent work in this area. As you mention, a lot of state-of-the-art codes have sophisticated approaches to solving this problem and there is not a need for us to criticize those approaches in this paragraph.**

7. End of Section 4.2: "faces of each grid cell" For your own reference the Lucy path-based estimator method is much better than tracking face fluxes (as done in Huang 2017 and Smith 2017).

    - **Noted. The mention of face fluxes has been removed.**

Appendix A:

1. Also cite Unno (1952; PASJ, 4, 100) regarding the partial redistribution function, e.g. equation A4 (isotropic version).

    - **Unno 1952 has been cited after equation A4.**

2. Also cite Osterbrock (1962; ApJ, 135, 195) regarding the moments of the redistribution function, e.g. equation A7.

    - **Osterbrock 1962 has been cited just before equation A7.**

3. Also cite Rybicki & Dell'antonio (1994; ApJ, 427, 603) regarding a derivation of the Fokker-Planck terms (their Appendix).

    - **Rybicki & Dell'antonio 1994 has been cited just before equation A13.**

4. After equation A6: Do you mean "scattering cancel for p = 0"?

    - **Yes. Correction made.**

5. Equation A15: I suggest removing the = 0 at the end, which is misleading in my opinion. At any rate, the explanation in the text is enough.

    - **Agreed. The change has been made.**

6. Typo after equation B5: "By setting the denominator".

    - **Yes. Correction made.**