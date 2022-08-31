# Computational Reactor Physics - Shannon Entropy
In this project the convergence of Monte Carlo simulations for nuclear fission reactors will be examined using the Shannon entropy. This is done using openMC and its python API together with numpy and matplotlib inside two jupyter notebooks, 'calculations.ipynb' and 'analysis.ipynb'. The results of the Monte Carlo calculations are stored in '/results', and the finalized plots can be found under '/plots'. Special thanks to my teachers and Uppsala univeristy for helping me throghout the course and this project.

### Theory

The Shannon entropy is a measure of how spread-out fission source sites are in the geometry of the Monte Carlo calculation. To do this, an entropy mesh is placed over the geometry, and each fission site is then counted to the mesh cell where it occurred. Then the Shannon entropy for each generation is calculated using equation 1.

![Equation 1](https://raw.githubusercontent.com/OdinhengeT/shannon-entropy/master/equations/equation_1.png)

Where N is the number of cells in the entropy mesh. In this way, a single number estimate of the "uniformness" (entropy) of the fission source sites can be obtained. This is useful, because it is then much easier to assess the convergence of this measure. rather than looking at the distribution in multiple dimensions. It can also be noted that the Shannon entropy is bound by the interval $S \in \left[0, \log_{2}(N)\right]$, where the maximal value is reached when the fission source site spread is perfectly uniform. 


### Conclusions

We see that, as expected, k effective isn't dependent on the entropy mesh at all (the curves are perfectly overlayed). It is also difficult to pinpoint where k effective has converged, due to the great uncertainties. This is especially true with fewer neutrons per batch.

When it comes to the Shannon entropy, having fewer neutrons per batch introduces numerical uncertainties. This is because the effect of each neutrons random walk on the overall entropy becomes larger.

When changing the coarseness of the entropy mesh, the value of the entropy increases. This is to be expected, as the entropy has an upper bound of the two-logarithm of the number of cells in the entropy mesh, as noted in the theory section.

Curiously enough though, apart from the higher upper bound, the overall curve shape and number of batches needed to reach convergence stays roughly the same (Look at the 250-batch data, as there are indications that the Shannon entropy is still growing after just the 80 batches). However, it is worthwhile to note that because the entropy of finer meshes span a larger range of numbers, the same tolerance (as defined by the convergence_finder function) allows for larger absolute deviations for finer meshes. This is caused by the normalization, as it divides by the difference between the maximum and minimum values.

From the few modifications that I have made to the geometry (including previous tests), it is clear that it plays a huge role in the behavior and convergence of the entropy. Unfortunately, these calculations take quite a lot of time, so there are only two different example geometries in this project. 

The main difference between these geometries is the pitch. The fuel pin radius was also altered with the hope of adjusting for changes to k effective, though this wasn’t very successful (geometry 1 has k $\approx$ 1.09 and geometry 2 k $\approx$ 0.665). 

Perhaps the largest geometrical factor of influence is the overall size of it. The larger the geometrical structure is, the longer it takes for neutrons to spread out in it.

By changing the pitch, the loose coupled-ness of the system is also changed. Thus, making it harder for the neutrons to propagate between sites. This would lead to faster convergence, but the opposite is observed in the data. The reason for the contradicting result is probably that the increasing pitch also affects the number of fuel rods. Since there are fewer fuel rods in total it takes fewer batches for the neutrons to spread out amongst them.

Another worthwhile note, because the fission source sites are of interest, and the multiplication factor, $k_{eff}$, affects how many site samples are generated per neutron on average, systems with large k values generate more source sites per neutron and are thus more computationally efficient to simulate. 
