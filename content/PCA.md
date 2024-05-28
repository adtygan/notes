- Comes under [[Dimension Reduction]]
- Imagine we have objects (like people or cities) that we want to cluster together based on data we have about them
	- toy problem: given a bunch of balls, cluster them
	- plausible criterion to cluster is based on colour
	- irl, we rarely have problem where simple criteria like that are used
		- usually, its a combination of factors, each with their thresholds
- If we had such simple data (think 1D, 2D or 3D data), we could make a plot and easily discern them
	- pca converts the correlation (or the lack thereof) among objects into a 2d plot
	- here, highly correlated objects are clustered together
- ![[PCA_1.png]]
- ![[PCA_2.png]]
	- here, dist. b/w red and yellow = dist. b/w red and teal
	- but, red and yellow are more different from each other than red and teal!
- First given the data, normalize it so that mean = 0. After this, data looks like below
- Then, fit a line, find unit vector along the line, which is the **eigenvector** for PC1
- ![[PCA_3.png]]
- ![[PCA_4.png]]
	- the distances are found by projecting the points to the line and finding that point's distance from origin
- PCA helps find principal components (PCs) (which are new variables) that are a linear combination of the original variables
	- these pc's must capture as much of the variance in the data as possible
- Eigenvalues determine how much variance is captured by each PC
	- the first pc is the one with the highest eigenvector, so on, so forth
- PC2 is simple, just find normal vector to PC1
- Use the PCs to draw graph 
- ![[PCA_5.png]]
- ![[PCA_6.png]]
- Scree plot shows % of variance accounted by each PC
- The number of PCs is either the number of variables or the number of samples, whichever is smaller
---

## References

1. **StatQuest**, *StatQuest: PCA main ideas in only 5 minutes!!!*: https://youtu.be/HMOI_lkzW08?si=BM9OGWX6w1lRpaOY
2. **StatQuest**, *StatQuest: Principal Component Analysis (PCA), Step-by-Step*: https://youtu.be/FgakZw6K1QQ?si=KlzhTirt-5rpVRM3
3. **Nic Coxen**, *The role of eigenvalues in PCA*: https://blog.devgenius.io/the-role-of-eigenvalues-in-pca-c177718c2cc