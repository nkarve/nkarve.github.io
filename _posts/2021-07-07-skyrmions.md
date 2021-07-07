---
layout: post
title: Baryons, Skyrmions and Topological Twists
categories: Physics
excerpt_separator:  <!--more-->
---

In my [previous] learning journey, I'd mentioned about baryons being topological twists in the pion field. By popular request, I will be giving a lightning review of what this construction, known as the *Skyrmion model*.

Before the QCD model of quarks and gluons was definitively established, the leading model for nuclear interactions was the pion model. A number of different pions field packaged neatly into a larger, composite field (using the generators of $\mathfrak{su}(N_f)$)  interacted with the nucleons (the proton and neutron). This yielded resonably good results, but was later superseded by the quark model, of which the pion model was seen to be an effective field theory. The pions themselves emerged as Goldstone bosons due to spontaneous chiral symmetry breaking - or, when factoring in the small explicit symmetry breaking due to the quark masses, they arose as *pseudo*-Goldstone bosons ("small" owing to the fact that $m_i\ll\Lambda_\text{QCD}$). The popular description of pions is as mesons - bound states of a quark and an antiquark, however the Goldstone bosons description, in which pions are described as vibrations in the various chiral condensates, is more appealing to me at least (the bound state presentation seems to obscure almost all of the genuinely interesting features of pions).

Consider a non-abelian $SU(3)$ gauge theory with $N_f$ flavours of massive quarks transforming in the fundamental representation. It is described by a Lagrangian
<div>
$$
\mathcal L=-\frac12\mathrm{Tr} F_{\mu\nu}F^{\mu\nu}+\sum_i^{N_f}(i\bar\psi_i\not\! D\psi_i-m_i\bar\psi_i\psi_i)
$$
</div>
where $\mathrm{Tr}$ is of course a colour trace. We can construct a low-energy approximation of this model using the so-called first-order chiral Lagrangian of the pion octet:
<div>
$$
\mathcal L_\pi=\frac{f_\pi^2}{4}\mathrm{tr}(\partial^\mu U^\dagger\partial_\mu U)
$$
</div>
where we have neglected pion masses. Here $U$ is an $SU(N_f)$ valued matrix: $U=e^{2i\pi(x)/f_\pi}$, and $\pi(x)\equiv\pi_a(x)T_a$ where $T_a$ are the generators of $\mathfrak{su}(N_f)$, and so this constitutes a non-linear sigma model. This is the simplest Lagrangian constructed out of $U$ that is symmetric under the full chiral symmetry (left and right unitary rotations, minus the axial rotations).

Now what are baryons? Everyone knows that they are once again certain bound states of quarks, but the fundamental definition is that they are objects that are charged under $U(1)_V$ and trivial under $SU(N_c)$. These are generically massive, so it is natural to expect that the Lagrangian of massless pions knows nothing them. This turns out to be wrong, baryons do indeed emerge in a miraculous way!

Recall from your forays into instantons and monopoles that we can often classify field configurations on a manifold using algebraic topology. In our case, let us study *static* configurations of $U$ that are asymptotically the identity. This boundary condition allows us to compactify the base space from $\mathbb R^3$ to $\mathbb S^3$, and so such field configurations are maps $\mathbb S^3\to SU(N_f
)$. Definitionally, such maps are classified by the third homotopy group $\pi_3(SU(N_f))=\mathbb Z$, so every static field configuration can be assigned an integer. This corresponds to the *winding number* of the field, roughly because it measures the number of times a field winds around infinity. This can be computed topologically via
<div>
$$
B=\int\mathrm d^3x\ B^0
\\B^\mu=\frac{1}{24\pi^2}\epsilon^{\mu\nu\rho\sigma}\mathrm{tr}((U^\dagger\partial_\nu U)^{\otimes 3})
$$
</div>
which is a conserved current $\partial_\mu B^\mu=0$. However, trying to explicitly construct these solutions from the chiral Lagrangian above yields nonsensical energies, and this is because no such solitons exist in this framework, via Derrick's theorem. 

Tony Skyrme formulated a different Lagrangian, essentially via trial and error, but employing the general symmetry considerations described above.
<div>
$$
\mathcal L_\text{Skyrme}=\frac{f_\pi^2}{4}\mathrm{tr}(\partial^\mu U^\dagger\partial_\mu U)+\frac{1}{32}\mathrm{tr}([U^\dagger\partial^\mu U, U^\dagger\partial^\nu U]^{\otimes 2})
$$
</div>
which initially looks slightly bizzare, but not so much when you realise that $L_\mu\equiv U^\dagger\partial_\mu U$ is a nice traceless Lorentz vector that we can construct out of the $U$ field - it's the left flavour current. Now we can calculate the energy of the static field configuration (an exercise which I leave to the reader), and finally, employing the Bogomol'nyi bound, we see that the energy is bounded by a constant prefactor times the winding number, which makes a cameo:
<div>
$$
E\ge 6\pi^2f_\pi|B|
$$
</div>
Note that $B$ is an additive charge, and we don't have all that many $U(1)$ symmetries lying around. We have one, in fact, the $U(1)_V$ vector symmetry (left-right symmetric phase rotations) is the only one - so we should identify this with baryon number. Since $U(1)_V$ holds exactly in QCD, unspoiled by anomalies, baryon number conservation is an exact symmetry of the Standard Model. However, there are no global symmetries in any quantum gravity theory (it's a simple thought experiment involving black hole evaporation - try and rediscover it), and BSM models generally only conserve $B-L$ to allow for matter-antimatter asymmetry for early-universe baryogenesis and leptogenesis.

Finally, we can construct explicit solutions that saturate the Bogomol'nyi bound by solving the equations of motion of $L_\mu$. To summarise, baryons appear in a higher-derivative pion model as field configurations with non-trivial winding at infinity. *Incredible*.

