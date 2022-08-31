# shannon-entropy
Examining convergence of Monte Carlo simulations for nuclear fission reactors using the Shannon entropy.

### Theory

The Shannon entropy is a meassure of how spread out fission source sites are in the geometry of the Monte Carlo simulation. To do this, an entropy mesh is placed over the geometry, and each fission site is then counted to the meshgrid cell where it occured. Then the Shannon entropy for each generation is calculated using equation 1.

$$\begin{equation}
    S = - \sum_{i=1}^{N} p_{i} \cdot \log_{2}( p_{i} ), \;\;\; p_{i} = \frac{\text{\# fission source sites in i-th cell}}{\text{\# fission source sites in total}}
\end{equation}$$

Where N is the number of cells in the entropy mesh. In this way, a single number estimate of the "uniformness" (entropy) of the fission source sites can be obtained. This is usefull, because it is then much easier to asses the convergence of this meassure. rather than looking at the distribution in multiple dimensions. It can also be noted that the Shannon entropy is bound by the interval $S \in \left[0, \log_{2}(N)\right]$, where the maximal value is reached when the fission source site spread is perfectly uniform. 

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
