<div style="overflow: hidden; padding-top: 56.25%; position: relative;">
  <iframe src="Tsfm Umar Jamil.pdf" style="border: 0; top: 0; left: 0; width: 100%; height: 100%; position: absolute;"></iframe>
</div>

## RNNs
- Convert `n input_seq -> n output_seq` in `n timesteps`
- At every stage, `i/p: input_seq + hidden_state`
- 3 issues
	- Slow computation time for long sequences (linear in time)
	- Vanishing or exploding gradients due to chain rule
	- Difficulty accessing information from long time ago
		- Due to hidden state losing info during propagation

## Enter Transformers
![The Transformer Model - MachineLearningMastery.com](https://machinelearningmastery.com/wp-content/uploads/2021/08/attention_research_1.png)
- Inputs: Convert i/p sentence to tokens
- Map tokens to Input IDs (fixed, doesn't change during training, depends on vocabulary)
- Input Embedding
	- Convert tokens into an embedding vector of size `d_model` (512 in our case)
	- This vector changes during training
- Positional Encoding
	- We want model to treat nearby words as "nearby" and distant words as "distant"
	- The input embeddings do not convey this, they convey meaning of token in isolation
	- We want PEs to represent a pattern that the model can understand
	- ![[Tsfm_1.png]]
	- `Encoder_Input[i] = Embedding[i] + Positional_Encoding[i]`
	- ![[Tsfm_2.png]]
	- PE(1,3) means the second token in the sentence's 4th embedding vector entry
		- Hence for this, `pos=1, 2i=2` and cos to be used
	- `i` goes from `0 to d_model/2 - 1` (Note `d_model` is always kept as an even number)
		- Assume `d_model = 6, i ranges [0, 2] [2*0, 2*0+1, 2*1, 2*1+1, 2*2, 2*2+1]`
	- PEs are only computed once and reused for **every sentence** during both training and inference
	- All we have to do is add these PE values to the input embeddings, which will change for every sentence
	- ![[Tsfm_3.png]]
	- We see closer positions have similar encodings than farther ones
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
		- The meaning of each token
		- The position of each token in the sentence
		- The token's/word's interaction with other words
	- Self Attention has 3 cool properties
		- Permutation invariance 
		- Requires no parameters (Multi-head attn. does)
		- If you don't want certain interaction to happen, before doing softmax, in the `(6,6) matrix`, set that entry to $-\infty$ 
			- This will be exploited in Decoder later
- Multi-Head Attention
	- At, this stage, 4 copies of the input received from previous layer is made
	- 1 is sent to `Add&Norm` and 3 are sent to `Multi-Head Attention` (refer diag.)
	- The 3 copies are `Q,K,V`
	- ![[Tsfm_6.png]]
	- The different heads are responsible for independently handling different aspects of the embedding
	- ![[Tsfm_7.png]]
		- The colors in the vis. plot represent strength of attention of different heads. The Pink head is seeing something that most other heads aren't. 
		- This is because it has a different part of the embedding that others don't
- Queries, Keys and Values
	- ![[Tsfm_8.png]]
	- The $softmax(\frac{QK^T}{\sqrt{d_k}})$ returns what I call a **mixing formula** for each row, which all sum to 1
	- For my Query, if i multiply $softmax(\frac{QK^T}{\sqrt{d_k}})$ with $V$, for each word's embedding, it will return a new embedding which is a mixture of all the words in the sentence, mixed according to how important each word is to this word
	- In this example, the word "love" involves mainly a mixture of romance and comedy which makes sense
- Layer Normalization
	- ![[Tsfm_9.png]]
	- Standardises each item in the batch to have `mean=0, var=1`
	- **Correction:** The diagram bottom left says "between 0 and 1" which is wrong
		- It actually means have `mean=0, var=1` may be restrictive
	- In the diagram, `N` is number of items in batch
	- Layer Norm applies its operation per item, whereas Batch Norm does across channel
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
		- All it does is, for each decoder output token, project its embedding to a dist. over vocabulary
	- The `Softmax` converts the dist. outputted by `Linear` into a prob. dist. over what decode output token it could map to.
	- During decoding phase, the causal mask helps us run the entire decoding in one shot
		- Internalise that the regardless of if you choose padding, or just feed actual sentence, it makes no difference to the output generated
	- Note that here we are doing **teacher forcing** and CE Loss is used.
- Inference
	- Store KV cache(output of encoder)
	- ![[Tsfm_13.png]]
	- Notice how here there is only one output instead of 4 outputs like how we say during training?
	- Also, note, generation is gonna happen one token at a time and in the figure above, we have already spent 3 timesteps to generate those 3 words after `<SOS>`
	- It is because we will only take the last output token
		- Coz the actual output for this is gonna look like `ti amo molto <EOS>` and we don't need the past tokens
	- While doing the generation, we could do 2 strategies
	- **Greedy:** At each timestep, take output token having max prob. (not a good idea)
	- **Beam Search:** At each timestep, pick `top B` words, then for each of them go find the next `top B` words, so on and so forth (better strategy)
		- Note that this means that the tree will grow like `B, BxB, BxBxB, ...`
		- To combat blow up, we only retain `top B` most probable sequences

## References

1. https://www.youtube.com/watch?v=bCz4OMemCcA