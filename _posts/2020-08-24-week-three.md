---
layout: post
title: The Good Stuff (24/8/20 - 30/8/20)
categories: Learning Journey
excerpt_separator:  <!--more-->
---

I'll probably be renaming (and/or splitting up) 'Group Theory, Calculus and Fun Stuff', probably into more manageable sections like Number Theory, Group Theory, Abstract Algebra - Category Theory, Linear Algebra and Logic+Set Theory already exist.

## Group Theory, Calculus and Fun Stuff
- The series differential equation for Bernoulli polynomial might come in handy. MathCounterexamples.net is very, very useful - particularly for topology and vector space concepts. 
- The supremum's like a categorical limit on posets. 
- I saw the proof of the Five-colour theorem (this one was slightly lacking in rigor for the final step that leads to the contradiction though) 
- I'm actively trying to avoid group theory and representation theory (along with QED and computational chemistry). Correction - although I'm avoiding group theory in the context of physics, I'll still be exploring sporadic groups (inspired by monstrous moonshine), because I feel they connect a lot of topics - so I narrowed down on the Mathieu groups (and on the side, the j-invariant) $\rightarrow$ the binary golay code - super interesting, I haven't done much coding theory before. 
- I had been trying to understand the Conway group through the octonions and the Leech lattice, without much headway, although there was a really good PPT intro by Rob Curtis and the CIMPA Conference - took the route through sphere packing, which I am familiar with, but I got stuck at Weyl groups (that's a topic which is too mathematical for me, along with Coxeter groups).
- The eightfold path to E8 looks very promising (through the Hurwitz quaternions), as does a paper about the absolute galois group - wow, I'll probably have a mountain of unread PDFs by the end of the year
- I was digging around for some proofs of the Prime Number Theorem - it would probably be best to go through the zeta function continuation-route, but it is rather heavy on the analysis. Alternatively, I could try the Erdos-Selberg 'elementary' formulation
- I revisited John D. Cook's excellent blog which encompasses a large number of topics - I really enjoyed posts on elliptic functions, Hamming codes, Bessel functions, egg equations, lgamma, constraints - he also has 'diagrams', as in, cheatsheets, providing a succinct summary of different topics (the clearest was the gamma function identities). 
- I found a paper on elliptic curves and the j-invariant - all right for an introduction, but it made a series of unmotivated algebraic manipulations which obscure the underlying symmetry
- I just realised - Michael Penn's 'difference under the integral' is entirely equivalent to Feynman's differentiation under the integral paramaterisation trick. 
- I looked at a bit more number theory - prime number theorem, Legendre's conjecture and Bertrand's postulate (I believe number theory was what got me interested in maths many years ago!).
-  Determinants of tridiagonal matrix yield a cool recurrence relation. 
- Are the Ulam spirals bars related to Dirichlet's theorem? (probably). 
- I looked at a powerful use of generating functions in counting problems (something beyond stats!) 
- Can barycentric coordinates be used beyond triangles? 
- Michael Penn's PhD thesis was related to vertex operator algebras - amazingly, they have far-reaching applications in string theory - hopefully I can find a friendly explanation. I tried some PPTs, but again, much of it seems rather unmotivated
- Speaking of this, I've reached a point where I can reasonably understand mathematical papers (I used to avoid them and stick to websites, but my library has been growing since July), and although I haven't come to favour the dense, lemma-rich method of exposition, a balance between that and a friendly exposition is the ideal learning arrangement [me from the future: take for example Clifford Algebras as Filtered Algebras by Yaim Cooper, MIT]. However, I would rank powerpoint presentations by British universities as my favourite introductory source of all. 
- I'm reading about Airy functions now (I skipped the WKB approximation in Griffiths so I didn't come across it there) - it might be useful to record all the Fourier transforms that I come across in case the reverse pops up somewhere. 
- One thing that I'm going to do more often is search 'A and B', to find relationships - 'Conway group and the Leech Lattice', 'Octonions and E8', 'Mathieu Groups and the Golay Code', 'Icosians and the Janko Group', 'E8 and spinors' - all these yield a trove of papers. 
- I randomly remembered the Zariski topology (as a not-so-pathological non-Hausdorff topology - because of its use in algebraic geometry) - so I decided to pursue that further.
- Summation by parts came as a surprise to me 
- The Golay code also led me to some basic coding theory (dual and parity-check matrices). 
- I'm quite excited to learn about the absolute galois group (my path is practically forged by a MathOverflow post on the most beautiful objects and relationships in mathematics!). 
- I found some old friends, the Lindemann-Weierstrass theorem and the Gelfound-Schneider Theorem (once I look at transcendentality measures, I'll read some proofs). 
- cp4space doesn't disappoint again! - a great post on the Leech lattice (I'm now reasonably confident of what it is, but I want to know how it relates to, say, octonions) 
-  I thought it'd be fun to get back into number theory - where better to start than the Lindemann-Weierstrass theorem> (+ proof, this time!). 
- Transitive groups make total sense now - if you want a intro that summarises all the main features before diving into Wikipedia, Wolfram Math World is probably the best choice (if there are no blogs :p). 
- Took a look at Zorn's lemma and its application to transfinite induction (which just looks like strong induction on ordinals). 
- The inverse scattering transform looks very interesting, but most resources are too hard (I'm not up to date on modern diff eq methods). 
- I needed reminding of the derivation of Stirling's approximation using Laplace's method (whose non-rigorous proof seemed very natural on third glance). 
- I read a bit about Bessel functions. I also came across some awesome John Baez articles on the Leech Lattice, as well as on the discrete harmonic oscillator spectrum due to the phase space constraints, so a 'TWFIMP' binge is likely, I think. 
-  I reread projective linear groups (I didn't know that PGL was isomorphic to PSL if the latter contained all the field roots of unity). 
- I finally looked at the Sylow theorems - very useful in the context of finite simple and solvable groups, which I love because of Galois theory
- I should try using the Coq proof solver (looks more fun than Agda at any rate) 
- Found a very cool example of a non constructive proof (irrational^irrational)
## Linear Algebra
- Levinson recursion for Toeplitz matrices (wonder where I'll need those?). 
- Singular Value Decomposition is awesome - there's geometric intuition on Wiki (incidentally, hyper-ellipsoids popped up in PCA too - can they be linked?). 
- I read the comprehensive page on the Rayleigh quotient - incredibly, it links the variational principle, quantum matrix elements, PCA and Sturm-liouville theory! (I didn't quite get the min-max theorem, I'll redo that later). 
- After some digging around, I found out how to find a matrix representation of a linear operator (just use the basis vectors) - which makes finding the eigenvalues straightforward now. 
- How do you permute a sparse matrix to reduce Cholesky fill-in?
## Physics

- There HAS to be a deeper reason for why differentiating and integrating Grassmas variables yields the same result (probably delving into exterior algebra) - on that note, I'd love to see a unification of differentiation and integration somehow (or formulate it myself!). 
- Quantum dots - most explanations don't really involve anything quantum? 
- Penguin Feynman Diagrams led me back to Quantum Diaries, very expository, especially regarding the Higgs (being 'eaten' by the W's) and its use in Feynman diagrams $\rightarrow$ he cited the 'spinor bible', an excellent amalgamation of spinors, Feynman diagrams and supersymmetry (three of my favourite concepts in quantum!).
-  Measuring the weak coupling constant by comparing decay rates was very cool. 
- I read an outstanding Physics SE answer on why it's not necessary for Boltzmann's constant to have dimensions (I'm not too good at thermodynamics, but I think I can glance at a cheat sheet to remedy that). 
- I noticed that spherical wave transformations (a type of conformal mapping) work a lot like the modular group action. 
- Interesting how solitons can be formed by the non-linear Kerr effect cancelling out dispersove forces - I still have to explore the maths behind it though 
- I hadn't seen the usage of de Broglie's theorem to justify stationary orbits as standing waves (that was a long-standing qualm on my side)

## Computer Science
- Touched up on how DFT is a polynomial evaluation at the roots of unity.
-  Interesting how sudoku's can be represented as a graph coloring problem. 
- I checked out SymPy's trace tech, but it's not too sophisticated.
-  The Phong reflection model is rather basic - how can I upgrade it? (I should see Sebastian Lague's excellent use of shaders in Unity - he makes videos that are both entertaining and informative). 
- Minimising FLOPs in matrix product traces reminded me of optimal tensor contraction order that I did several months back (graph-based methods are, of course, my favorite!). 
- I read about the simplex method for linear programming on Brilliant - again, the method using slack variables reminded me of a similar procedure for finding the furthest polytope vertex in my Rust physics engine (I'll be making a new one in Python, now that I've found a good graphics lib!). 
- The nearest-neighbour algorithm is TSP related? (not ML!) $\rightarrow$ couple of TSP heuristics, including one that I thought of when I was 8! (reversing the order of a subset).
-  There is a nice definition for the inverse Ackermann function - now I see how it arises: from repeated function composition (like log*) -> on the other hand, cp4space introduced his sigma function which dwarfs TREE (I liked the combinatorial logic system with oracles). I liked the 'strategy-stealing' system for poset games 
- Langton's ant was super-cool (I'll try it with a hexagonal grid). 
- Out of VisPy, Kivy and Pyglet, the last suited my purpose very well. I got graphics and collision detection up and running (impulse-based resolution still in progress - Randy Gaul's page, one of the very first I visited, is not the greatest for the mathematical derivation: I found a different set of slides). 
- Most of my old sources are solid: I'm going to do proper broad phase (sort and sweep - Nilson Souto nails it on Toptal - his is far more relevant for 2D). Allen Chou's excellent series will also be very useful. 
- I opted for SAT instead of GJK this time (people don't seem to mention the fact that SAT can be used for general convex polygons too) - but the major improvement has been switching over to NumPy (sometimes a 15:1 code line saving on Rust): extreme ease of use and speed. 
- I'm still a NumPy veteran thanks to CNNs, so I was able to not just vectorise, but matrix-ise the SAT implementation. 
- After watching a video by pontypants, I feel suddenly inspired to use Unreal for a game 
-  I fixed the flickering by employing batch-drawing + double buffering (pyglet is so low-level that it keeps it optional). 
- I took a look at Minkowski Portal Refinement, but contrary to popular belief, it doesn't provide the contact manifold (oh well, I'll have to deal with clipping now). 
- Discrete vs Continuous collision makes sense now - spatial hashing might be worth reading about just in the context of robust data structures 
-  Pampy looks very interesting for pattern matching (Python is sorely lacking in that, but I'm not sure how well it would integrate with duck typing) 
-  The GDC slides (by Erin Catto and Dirk Gregorius) are excellent resources, much better than lots of half-baked sites. I also found some slides for a uni course: they derived the Eberly formula clearly, but it doesn't generalise well to universal constraints
- I read Real-Time Collision Detection, but it feels a bit introductory. I liked 'How to Stop My Constraints from Blowing Up'.
- I found dyn4j's blog as well, which gives a decent (but simplified) overview of the major algorithms, but more importantly, is the first to properly explain clipping to find contact points in SAT. 
- I'm working on understanding sequential impulse (mostly from Erin Catto). Using my poor SAT contact points, I managed to add rotations (angular momentum was, of course, the wrong way) to my (very unstable!) physics engine! 
- Today I had my first taste of constraint-based solving (introduced by Nilson Souto [Part III], dyn4j and the very helpful gamedev SE - the first one on which I asked a question!). 
- Corrected a slight problem: I was using the geometric mean rather than the centroid (so I had to gather the unwieldy "shoelace formula" stuff - my method of translating the vertices to put the centroid at the local origin is still a great idea). 
-  I implemented GJK and EPA as well for a laugh - I just can't seem to get the contact points, though (and most GitHub physics engine routines are in practically obfuscated). Unfortunately I hit a wall in my EPA refinement - I'll probably have to invoke barycentric coordinates somehow, but I still can't see it.
- A paper by Marjin Tamis was very helpful in constraint-based solving: I was introduced to lambda-clamping, global solvers and distance constraints

## Statistics and Machine Learning
- There's a very clear example on conjugate priors on Wiki, really cleared it up. 
- I like to imagine Bayesian methods as a duality between parameters and data (I feel they're interchangeable). 
- I'd like to find out more about recursive Bayesian elimination. 
- I somehow returned to jakevdp! His In-depth series is almost flawless - I learnt about Random Forests, Naive Bayes Classification, PCA and Linear Regression (properly) - I should read his book on SafariBooks (one problem is that he uses sklearn a lot, but doesn't show the underlying algorithms - but it's fine for an intro and some concrete examples). 
- A quick jbstatistics video cleared up some confusion regarding the negative binomial distribution (I knew it was a generalisation of the geometric distribution!) - speaking of which, confusion matrices are useful for precision and recall (and F1 - first real use of the harmonic mean) 
- The old stick-breaking probability question had a set of nice answers on math.overflow - the best one was joining the two ends and seeing if the points lie in a semicircle. 
- I looked at some intuitive explanations of SVD and singular values, on gregorygunderson.com's very concise blog, as well as jeremykun (!), who related the notions of data storage and linear transformations - what's a 'stick-breaking' multinomial? 
-  I need to read about Empricial Bayes and Radomized Sigular Value Decomposition on Gregory Gundersen's excellent site
