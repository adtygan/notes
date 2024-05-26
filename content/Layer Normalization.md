![[LayerNorm_1.png]]
- Standardises each item in the batch to have `mean=0, var=1`
- Correction in diagram 
	- **... maybe having all values ~~between 0 and 1~~ with `mean=0, var=1` may be too restrictive ...**
- In the diagram, `N` is number of items in batch

### Difference Between BatchNorm and LayerNorm
 - LayerNorm applies its operation per-item, whereas [[Batch Normalization|BatchNorm]] does per-channel

## References
1. https://www.youtube.com/watch?v=bCz4OMemCcA
