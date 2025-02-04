- We want model to treat nearby words as "nearby" and distant words as "distant"
	- the input embeddings do not convey this, they convey meaning of token in isolation
- We want PEs to represent a pattern that the model can understand
- ![[PosEnc_1.png]]
- `Encoder_Input[i] = Embedding[i] + Positional_Encoding[i]`
- ![[PosEnc_2.png]]
	- PE(1,3) means the second token in the sentence's 4th embedding vector entry
		- hence for this, `pos=1, 2i=2` and cos to be used
	- `i` goes from `0 to d_model/2 - 1` (Note `d_model` is always kept as an even number)
		- assume `d_model = 6, i ranges [0, 2] [2*0, 2*0+1, 2*1, 2*1+1, 2*2, 2*2+1]`
- PEs are only computed once and reused for **every sentence** during both training and inference
- All we have to do is add these PE values with the input embeddings, which will change for every sentence
- ![[PosEnc_3.png]]
- We see closer positions have similar encodings than farther ones

## References
1. **Umar Jamil**, *Attention is all you need (Transformer) - Model explanation (including math), Inference and Training*: https://www.youtube.com/watch?v=bCz4OMemCcA