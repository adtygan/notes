### nn.Embedding
- Internally, it resembles a matrix
- Number of words as rows and size of each word vector as columns
	- (e.g.) if there are 100 words in vocab and each word is to be embedded into a 512 dim vector, then embedding matrix is of shape `(100, 512)`
- Given a number (here, `token_id`), embedding will always return the same vector for it
- Additionally, this embedding matrix is **trainable**
- You can either start with random weights, or pick a pre-trained embedding matrix and fine-tune it