- It's a **general-purpose differentiable computer**
	- necessary coz you want to train it to solve arbitrary problems, be it next-word prediction or image recognition
- Expressive (in forward pass)
	- can express general computations (**algorithms**) as message passing
		- nodes store vectors and can look at other nodes' vectors, and use this to update themselves
		- nodes can **query** other nodes for certain information and other nodes return their **keys and values**
- Optimizable (via gradient descent + backprop)
- Efficient (high parallelism compute graph)
- ⚡️Residual connections support learning **short algorithms** fast and first and then extend them longer over training 
	- ⚠️ i don't yet fully understand this but below are my notes
	- transformers is a set of blocks with residual connections 
	- the residual stream would allow supervision through gradients from the topmost layer to flow uninterrupted all the way back to first layer
	- but initially, this residual stream would not contribute anything due to its initialization
	- so, the final layer, benefitting from supervision learns a short algorithm
	- but later, residual stream kicks in and the penultimate layer starts contributing
	- patterns repeats until each block starts contributing, eventually learning a complex algorithm
## References
1. **Andrej Karpathy**, *Transformers: The best idea in AI | Andrej Karpathy and Lex Fridman*: https://youtu.be/9uw3F6rndnA?si=gpOCFxE2-kDCBais