![The Transformer Model - MachineLearningMastery.com](https://machinelearningmastery.com/wp-content/uploads/2021/08/attention_research_1.png)

## Issue with [[Recurrent Neural Networks|RNNs]] 
- Its **linear time complexity** is bad for very long sequences
- Vanishing or exploding gradients due to chain rule
- Difficulty accessing information from long time ago
	- coz hidden state losing info during propagation
### Input Embedding
- Inputs: Convert i/p sentence to tokens
- Map tokens to Input IDs (fixed, doesn't change during training, depends on vocabulary)
- Input Embedding
	- convert tokens into an embedding vector of size `d_model` (512 in our case)
	- this vector changes during training
### [[Positional Encoding]]

- Self-Attention (Single-Head Attention)
	- $$Attention(Q,K,V) = softmax(\frac{QK^T}{\sqrt{d_k}})V$$
	- `d_k = d_model`
	- Assume, `seq=6, d_k = 512`
	- `Q, K, V` are all the same and mean the input sentence
	- ![[Tsfm_4.png]]
	- Sigmoid is applied row-wise
	- ![[Tsfm_5.png]]
	- The left matrix captures each word's interaction with other words (**Attention**)
	- The final output Attention is the same shape as input embedding and captures 3 things
		- the meaning of each token
		- the position of each token in the sentence
		- the token's / word's interaction with other words
	- Self Attention has 3 cool properties
		- permutation invariance 
		- requires no parameters (multi-head attn. does)
		- if you don't want certain interaction to happen, before doing softmax, in the `(6,6) matrix`, set that entry to $-\infty$ 
			- this will be exploited in decoder later
- Multi-Head Attention
	- At, this stage, 4 copies of the input received from previous layer is made
	- 1 is sent to `Add&Norm` and 3 are sent to `Multi-Head Attention` (refer diag.)
	- The 3 copies are `Q,K,V`
	- ![[Tsfm_6.png]]
	- The different heads are responsible for independently handling different aspects of the embedding
	- ![[Tsfm_7.png]]
		- the colors in the vis. plot represent strength of attention of different heads
		- the pink head is seeing something that most other heads aren't
		- this is because it has a different part of the embedding that others don't
- Queries, Keys and Values
	- ![[Tsfm_8.png]]
	- The $softmax(\frac{QK^T}{\sqrt{d_k}})$ returns what I call a **mixing formula** for each row, which all sum to 1
	- For my Query, if i multiply $softmax(\frac{QK^T}{\sqrt{d_k}})$ with $V$, for each word's embedding, it will return a new embedding which is a mixture of all the words in the sentence, mixed according to how important each word is to this word
	- In this example, the word "love" involves mainly a mixture of romance and comedy which makes sense
- [[Layer Normalization]]
- Decoder
	- ![[Tsfm_10.png]]
	- In here, the Decoder gets `K, V` from Encoder and `Q` from Decoder
	- So its not "Self-Attention" anymore, its "Cross-Attention"
- Masked Multi-Head Attention
	- Causal Mask ![[Tsfm_11.png]]
- Training
	- ![[Tsfm_12.png]]
	- Feed input to decoder by adding `<SOS>`.
	- IRL, you also do the padding for both input and output seq. so that they are of fixed length, but ignore here
	- The `Linear` layer is trained end to end and is the same matrix for all entries. 
		- all it does is, for each decoder output token, project its embedding to a dist. over vocabulary
	- The `Softmax` converts the dist. outputted by `Linear` into a prob. dist. over what decode output token it could map to.
	- During decoding phase, the causal mask helps us run the entire decoding in one shot
		- internalise that the regardless of if you choose padding, or just feed actual sentence, it makes no difference to the output generated
	- Note that here we are doing **teacher forcing** and CE Loss is used
- Inference
	- Store KV cache(output of encoder)
	- ![[Tsfm_13.png]]
	- Notice how here there is only one output instead of 4 outputs like how we say during training?
	- Also, note, generation is gonna happen one token at a time and in the figure above, we have already spent 3 timesteps to generate those 3 words after `<SOS>`
	- It is because we will only take the last output token
		- coz the actual output for this is gonna look like `ti amo molto <EOS>` and we don't need the past tokens
	- While doing the generation, we could do 2 strategies
	- **Greedy:** At each timestep, take output token having max prob. (not a good idea)
	- **Beam Search:** At each timestep, pick `top B` words, then for each of them go find the next `top B` words, so on and so forth (better strategy)
		- note that this means that the tree will grow like `B, BxB, BxBxB, ...`
		- to combat blow up, we only retain `top B` most probable sequences

## References
1. **Umar Jamil**, *Attention is all you need (Transformer) - Model explanation (including math), Inference and Training*: https://www.youtube.com/watch?v=bCz4OMemCcA